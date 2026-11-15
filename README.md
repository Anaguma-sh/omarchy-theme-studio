# Omarchy Theme Studio

A Codex skill for designing, refining, and applying Omarchy themes. Start with a wallpaper, an installed theme, or a visual brief. Compare palettes using the same terminal text, selection, window borders, and pill before choosing a direction.

The skill covers contrast checks, local theme packaging, and verifying that desktop apps load the intended palette. It includes a Cloudroom workflow for light and dark palettes, typography, composer styling, and the native GTK menu.

## Use with Codex

Copy the skill files into a directory named `omarchy-theme-studio` in your Codex skills directory, keeping `references/` alongside `SKILL.md`. Once Codex discovers the skill, try:

> Use Omarchy Theme Studio to propose three palettes from this wallpaper. Show each with terminal text, selected text, active and inactive borders, and the pill. Wait for me to choose before applying a palette.

Live changes require Omarchy. Cloudroom steps use the installed `room-cli` when Cloudroom is in scope.

Read the [skill instructions](SKILL.md) and [desktop integration notes](references/integration.md).
