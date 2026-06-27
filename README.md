# samsung-galaxybook-hotkey-dkms

Temporary DKMS override for the Linux `samsung_galaxybook` platform driver carrying two fixes.

**1. SAM0430 ACPI hotkey handling** — fixes Fn hotkeys on Samsung Galaxy Book models where the firmware sends ACPI notifications instead of normal keyboard scancodes:

- `0x7d` — Fn+F9 keyboard backlight cycle
- `0x6e` — mic mute
- `0x6f` — camera / block-recording

This part is **upstream as of Linux v7.1**, so it is redundant once you run a v7.1+ kernel.

**2. Keyboard backlight suspend/resume restore** — the firmware resets the keyboard backlight to off during suspend, and the in-tree driver does not opt into the LED core's save/restore (`LED_CORE_SUSPENDRESUME`), so your brightness level is lost on resume. This package sets the flag so the level is re-applied on wake. **Not yet upstream** (as of Linux v7.1), so this remains useful even after the hotkey fix lands.

Remove this package once your kernel includes **both** fixes (check whether `LED_CORE_SUSPENDRESUME` is set on `kbd_backlight` in your kernel's `samsung-galaxybook.c`).

## Credits / provenance

The `samsung-galaxybook` Linux platform driver was authored and upstreamed by **Joshua Grisham**.

Original project and research repo:

- https://github.com/joshuagrisham/samsung-galaxybook-extras

This package does **not** vendor the driver source. Its `PKGBUILD` fetches a pinned Linux kernel source file and applies two small patches: `sam0430-hotkeys.patch` and `kbd-backlight-suspend-resume.patch`.

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
