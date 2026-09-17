# Themed Tray

An [Omarchy](https://omarchy.org/) shell plugin that replaces the built-in
`omarchy.tray` bar widget with a version that recolors **every** system tray
icon to match your active theme's foreground color — not just icons that
follow the freedesktop `-symbolic` naming convention.

## Why

Omarchy's stock tray widget only re-tints icons named like `foo-symbolic`.
Most apps follow that convention, but some (Steam is the classic example)
ship a tray icon with fixed, baked-in colors. Left alone, that icon clashes
with whatever theme you're running instead of blending in like the rest of
the bar.

This plugin forces **all** tray icons through the same theme-tinting logic,
so any app you have running — today or installed in the future — gets a
tray icon that matches your theme automatically, with nothing to configure.

## How it works

- Icons are drawn into a hidden `Image` layer, then recolored with
  `Qt5Compat.GraphicalEffects.ColorOverlay`, which does a true alpha-masked
  color replace (it ignores the source icon's own colors/luminance and just
  uses its alpha channel as a silhouette mask).
- The color used is the bar's current `foreground` color, so it always
  follows whatever Omarchy theme is active.
- This intentionally goes further than the stock widget, which uses
  `QtQuick.Effects.MultiEffect` — that effect scales the tint by the source
  pixel's own luminance, so it can't brighten icons that are mostly dark
  fills (again, Steam's icon is a good example: no tint color makes it
  visibly change color, because `MultiEffect` multiplies it down to near-black).

## Install

```bash
omarchy plugin add https://github.com/lnewhart/themed_tray_omarchy.git --enable
```

This clones the plugin into `~/.config/omarchy/plugins/themed-tray/` and
switches your bar to use it in place of the stock tray widget.

## Opting an app out

If you'd rather a specific app keep its native/brand tray icon colors, open
`Tray.qml` and set `forceSymbolic: false` for that one `TrayIcon` instance
(see the comment in the `TrayItem` component for an example using
`TrayModel.itemNamed(...)`).

## Uninstall / revert

```bash
omarchy plugin remove themed-tray
```

This restores the stock `omarchy.tray` widget.

## License

MIT. This plugin is a fork of Omarchy's built-in `omarchy.tray` bar widget
(also MIT licensed); see `LICENSE`.
