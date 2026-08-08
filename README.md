# Rawaccel Convert GUI

Personal port of [Kuuuube's Rawaccel Convert GUI](https://github.com/Kuuuube/rawaccel-convert-gui), kept for local use on a GNOME desktop running mutter patched for custom acceleration.

This is not a general release and it does not track upstream. If you are not me, go to the original.

Curve generation comes from [Rawaccel Convert](https://github.com/Kuuuube/rawaccel_convert), pinned here to [a fork](https://github.com/adee/rawaccel_convert) carrying the libinput and windows curve changes below.

## Differences from the original

- Libinput exports generate the number of points asked for, from `2` to `64`, instead of always generating `64`.

- `Apply With gsettings` writes the curve to GNOME in one click. `Copy gsettings Commands` copies the equivalent shell commands instead, and works in every build. Both only appear for the `Libinput` export scaling.

- The libinput step size can be frozen, so regenerating points does not overwrite a hand picked value. A frozen step is what gets applied and copied.

- `Windows` curve type, reproducing the windows `Enhance pointer precision` curve from the `SmoothMouseXCurve` and `SmoothMouseYCurve` registry values. Ported from [yinonburgansky's windows acceleration function](https://gist.github.com/yinonburgansky/7be4d0489a0df8c06a923240b8eb0191).

## GNOME requirement

The gsettings buttons write these keys of `org.gnome.desktop.peripherals.mouse`:

| Key | Type |
| --- | --- |
| `custom-accel-step` | double |
| `custom-accel-points` | array of doubles |
| `accel-profile` | `'custom'` |

Upstream gsettings-desktop-schemas defines none of them, and its `accel-profile` has no `custom` value, so a mutter build patched for custom acceleration is required. On a stock GNOME the buttons fail and report the error gsettings gives back.

Everything else in the app works without any of this. Use `Copy gsettings Commands`, or the points and steps boxes, to apply the curve by other means.

## Building

```
cargo build --release
```

The binary is written to `target/release/rawaccel_convert_gui`.

No web build is published for this port, since the upstream web app does not carry any of these changes. `trunk serve` still builds one locally.

## Usage

- Configure your settings just like rawaccel

- Use the export function to dump out the points

    For libinput: [Applying a custom accel curve with libinput](https://github.com/Kuuuube/rawaccel_convert/blob/master/docs/libinput.md)
