# samsung-galaxybook-hotkey-dkms

DKMS override for the Linux `samsung_galaxybook` platform driver with SAM0430 ACPI hotkey handling.

This fixes Fn hotkeys on Samsung Galaxy Book models where the firmware sends ACPI notifications instead of normal keyboard scancodes, including:

- `0x7d` — Fn+F9 keyboard backlight cycle
- `0x6e` — mic mute
- `0x6f` — camera / block-recording

This is intended as a temporary compatibility package. Remove it once your kernel includes the upstream SAM0430 hotkey fix.

## Install on Arch / Omarchy

```bash
sudo pacman -S --needed base-devel dkms linux-ptl-headers
git clone https://github.com/Jabbslad/samsung-galaxybook-hotkey-dkms
cd samsung-galaxybook-hotkey-dkms
makepkg -si
sudo modprobe -r samsung_galaxybook
sudo modprobe samsung_galaxybook
```

Verify the patched module is active:

```bash
cat /sys/module/samsung_galaxybook/srcversion
```

Expected patched `srcversion`:

```text
1FD9CB371240E868B48FC37
```

Then test Fn+F9.

## Uninstall

```bash
sudo pacman -Rns samsung-galaxybook-hotkey-dkms
sudo modprobe -r samsung_galaxybook
sudo modprobe samsung_galaxybook
```

## Notes

This is a source-only DKMS package. Pacman's DKMS hooks build and install it for installed kernels when:

- this package is installed or upgraded, or
- a kernel/headers package is installed or upgraded.

Keep matching headers installed for your running kernel, for example `linux-ptl-headers` for `linux-ptl`.
