---
name: omarchy-theme-studio
description: Generate, customize, or repair an Omarchy desktop theme using an installed theme, one or more wallpaper references, or a visual brief. Use for palette selection, theme packaging, and keeping requested desktop apps aligned with theme changes.
---

# Omarchy Theme Studio

Use the Omarchy skill for live desktop changes. Inspect the installed theme commands and templates, the active theme and wallpaper, and existing user overrides before deciding which files own the look. Read [Desktop integration](references/integration.md) when applying a theme or fixing app coverage. Follow [Cloudroom theme integration](#cloudroom-theme-integration) when the request covers Cloudroom’s app palette or native menu.

1. **Establish the direction.** List installed themes when the user wants to choose a base. For supplied wallpapers, inspect them and derive colours from the background, midtones, and accents. Agree on a common palette for multiple wallpapers unless the user wants distinct variants. For revisions, carry forward the user's accepted colours, typography, borders, and behaviour.
2. **Compare palettes when there is a choice.** Show two or three visually distinct options in a UI preview when available. Use the same sample text, terminal, active and inactive outlines, pill, and selection in each option, alongside the wallpaper. Include a short label and hex values. A screenshot or swatches work when interactive previews are unavailable. Skip selection when the user already specified the palette or requested a targeted fix.
3. **Build the theme.** Stage a named user theme or edit the user's existing variant. Use its palette as the source for desktop roles and generated app settings. Choose separate readable colours for primary text, muted text, selection fill and text, focus, and semantic states. Check normal text against the surfaces it appears on, including selections. Use 4.5:1 as the normal-text target and check the actual rendered contrast if transparency is involved. Keep neutral blacks and greys equal-channel when requested.
4. **Deliver at the requested scope.** A preview request ends with the preview. For a theme package, include the palette, wallpaper assets where provided, and only the overrides the design needs. When applying an authorized change, preserve unrelated preferences, back up edited user files, and apply through Omarchy's normal theme command.
5. **Verify.** Check staged and active colours, confirm Hyprland accepts its config, and sample the UI the user cares about. For app sync, verify what the running app loaded as well as what its config says. Report which surfaces were checked and which still need a reload or have limited theme support.

## Cloudroom theme integration

Use this workflow when the requested theme should cover Cloudroom, including requests to match the whole Omarchy desktop. Cloudroom's app palette and native Linux menus are separate surfaces.

### App palette

1. Run `room-cli theme dir` against the user's running Cloudroom instance. Use the returned directory and create or update `<theme-dir>/<slug>/theme.css`. If the server is unavailable, stage the CSS for review and report that activation requires a running instance. Inspect Cloudroom's available theme-authoring guidance and existing theme CSS to confirm the tokens for that build.
2. Map the chosen Omarchy palette to Cloudroom's tokens. Set shared typography and a readable light palette in `:root, .light`, then set the dark palette in `.dark` so it matches Omarchy when dark mode is in use. Use the verified desktop font for `--font-sans`, `--font-mono`, `--font-terminal`, and `--font-serif` when the user wants that font throughout. Give the font a fallback.
3. Cover canvas and ink, sidebar, cards, popovers, composer, recessed surfaces, and selection. Set button fill and ink together. Keep distinct tokens for borders and focus rings, hover and active states, file and timeline accents, and semantic colours such as destructive, warning, success, and diffs. Use the installed app's token names. Check contrast against the actual surface in both modes, especially muted labels and selection text.
4. If small controls or the composer need a readability pass, scope the CSS to those elements. In the build used here, the useful fixes were a 12 px minimum for compact control text, 16 px toolbar icons where undersized, brighter enabled-control labels, and an explicit border on `[data-app-composer] [data-promptbox]` with a visible `:focus-within` state. Inspect current markup before relying on those selectors.
5. Resolve the theme with `room-cli theme show <id>`, apply an authorized change with `room-cli theme set <id>`, and verify the active theme using `room-cli theme show`. Check the running app's computed font and colours on interface labels, chat prose, code, the terminal, the composer, and its focus state. Treat the live readback as the check that the UI applied the theme.

If Omarchy already has a Cloudroom sync hook, inspect how it updates the renderer before adding a second path. Use the native CLI where available for named Cloudroom themes and ensure that an existing CSS override does not replace the selected palette on the next launch or desktop theme change. When Cloudroom is closed, a hook can stage the theme and defer the CLI activation until the app is available.

### Native Linux menu

When the user also wants the top File/Edit menu to match, inspect whether the running build draws it through GTK. For the GTK 3 build used here, create a user GTK theme under `~/.local/share/themes/<name>/gtk-3.0/gtk.css` using Adwaita as its base. Set the menu background, foreground, hover state, and compact font there. In the Cpunk trial this used black menus and 11 px JetBrainsMono Nerd Font.

Give the GTK override its own user-owned desktop entry with a distinct ID and a command such as `Exec=env GTK_THEME=<name> "<absolute-Cloudroom-executable>" %U`. Cloudroom regenerated its default launcher on startup during the original session. A distinct launcher preserved the override. Provide an equivalent shell alias when the user asks for one.

Fully quit Cloudroom and reopen it through the themed launcher when authorized. Verify the correct launcher resolves, the running process inherited `GTK_THEME`, and the native menu actually renders the requested colours. If only the GTK stylesheet has been tested, say so.
