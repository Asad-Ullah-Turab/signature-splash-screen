&lt;!-- README.md for Asad Signature Plymouth Theme --&gt;

# Asad Signature Plymouth Theme

This repository contains a **custom Plymouth boot splash theme** that animates a handwritten signature during system boot.

The theme uses a sequence of PNG frames generated from a video of the signature, producing a smooth “handwriting” animation on a black background.

---

## Repository Structure

```
.
├─ asad-signature/        # Contains the Plymouth theme
│   ├─ asad-signature.plymouth
│   ├─ asad-signature.script
│   ├─ sig_1.png
│   ├─ sig_2.png
│   └─ ... (all PNG frames)
└─ SourceMaterial/       # Original source video and assets
```

- `asad-signature/` → all files required for the Plymouth theme.
- `SourceMaterial/` → original signature video used to generate the PNG frames.

---

## Usage Instructions

### 1️⃣ Copy theme files to Plymouth directory

```
sudo cp -r asad-signature /usr/share/plymouth/themes/
```

### 2️⃣ Set the theme as default

```
sudo plymouth-set-default-theme asad-signature -R
```

- `-R` automatically rebuilds the initramfs for your system.
- Make sure your `GRUB_CMDLINE_LINUX_DEFAULT` includes `quiet splash`:

```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet splash"
```

- Then regenerate GRUB config:

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### 3️⃣ Ensure Plymouth hook is in mkinitcpio

Edit `/etc/mkinitcpio.conf` and make sure the `HOOKS` line includes `plymouth` **before `autodetect`**. For example:

```
HOOKS=(base udev plymouth autodetect modconf block keyboard filesystems fsck)
```

Then rebuild initramfs:

```
sudo mkinitcpio -P
```

- This ensures Plymouth is loaded at boot, preventing text messages from appearing instead of the animation.

### 3️⃣ Reboot to see the animation

```
sudo reboot
```

- During boot, the handwritten signature animation will play once and then stay on the last frame until the system finishes booting.

---

## Optional Notes

- **Frame numbers:** PNG frames are expected to be named sequentially: `sig_1.png`, `sig_2.png`, …
- **Aspect ratio:** Frames are pre-sized to maintain the original aspect ratio of the signature.

---

## Generating Your Own Signature Frames

1. Record your signature video (`signature.mp4`).
2. Use `ffmpeg` to extract frames while preserving aspect ratio:

```
ffmpeg -i signature.mp4 -vf "scale=800:-1" sig_%d.png
```

3. Replace the frames in the folder with your own frames and update the `TOTAL_FRAMES` in the script.

---

## Credits

- Original author: **[Asadullah](https://github.com/asad-ullah-turab)**
