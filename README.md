# Steamy Handheld PC
Ditch Windows and turn your handheld PC into a SteamOS-like device with Arch Linux.

**!! WORK IN PROGRESS !!**

For now, this is just a rough idea of how to run Arch Linux properly on your Handheld. I am not using Gamescope session, as it can tend to be a bit more complicated to setup. This will only set up running Steam in "Big Picture Mode" for a "SteamOS-like" experience. Alternativly, you could also just install [ChimeraOS](https://chimeraos.org), but if that distro ever stops being supported or you would rather just use your own Arch setup, then this is a good starting point.

## Features

- Everything should function just like you were using Windows Desktop, but it's 100x better because Linux!
- Use Decky Loader + SimpleDeckyTDP to adjust your GPU and CPU power consumption to maximize your battery.
- Use InputPlumber to rebind any button on your device or emualte any controller.
- Adjust your devices RBG lighting using HueSync.

## Devices Tested

| Device | Work? | Notes |
| ----- | ----- | ----- |
| Ayaneo Air 1S | ✅ | Requires ChimeraOS Kernel and [Audio Fix](#audio-fix). |
| Ayaneo Flip DS | ✅ | Requires ChimeraOS Kernel and [Audio Fix](#audio-fix). The bottom screen works but has no touch input. |
| Ayaneo Slide/Antec Core HS | ✅ | Requires [Kernel Param Fix](#ayaneo-slide-kernel-param-fix) or else it will randomly crash. |

## Things you need
- Handheld PC x86/64
- Keyboard + Mouse (not needed on some devices, but makes things easier at first)
- USB Stick loaded with bootable [Arch](https://archlinux.org) on it.
	- While you could run stock Arch, I prefer an Arch distro. My personal choice is [Manjaro](https://manjaro.org) with GNOME, but some other great options are [EndeavourOS](https://endeavouros.com) or [CachyOS](https://cachyos.org) or many others. Follow their instructions to setup a bootable USB.

## How to Setup
- First, use the USB stick to install your chosen Arch distro on your device.
  - You can usually access the boot menu by mashing F7 while the device boots.
  - TIP: Remove any SD cards before installing, just so you don't accidentally install the OS to the SD card.
- Once your OS is installed and you are booted into the OS, install these packages.
  - Using Pacman
    - [inputplumber](https://github.com/ShadowBlip/InputPlumber)
    - [steam](https://wiki.archlinux.org/title/Steam)
    ```
    sudo pacman -S inputplumber steam
    ```
  - Using AUR
    - [chimeraos-device-quirks](https://github.com/ChimeraOS/device-quirks)
	```
	yay -S chimeraos-device-quirks
	```
    - AYANEO DEVICES ONLY: [ayaneo-platform-dkms-git](https://github.com/ShadowBlip/ayaneo-platform)
	```
	yay -S ayaneo-platform-dkms-git
	```

  - Manually install
    - [ChimeraOS Kernel & Header](https://github.com/ChimeraOS/linux-chimeraos/releases)
    	- Some devices work fine without this, only install if you are having issues. Download both the Kernel and Header files, then install them using ```sudo pacman -S [path_to_file]``` or depending on the Arch distro, just opening the file should install it.
    - [Decky Loader](https://github.com/SteamDeckHomebrew/decky-loader) - Accessed via Steam Big Picture.
      ```
      curl -L https://github.com/SteamDeckHomebrew/decky-installer/releases/latest/download/install_release.sh | sh
      ```
    - [SimpleDeckyTDP](https://github.com/aarron-lee/SimpleDeckyTDP) - Decky Loader Plugin
      ```
      curl -L https://github.com/aarron-lee/SimpleDeckyTDP/raw/main/install.sh | sh
      ```
    - [DeckyPlumber](https://github.com/aarron-lee/DeckyPlumber) - Decky Loader Plugin
      ```
      curl -L https://github.com/aarron-lee/DeckyPlumber/raw/main/install.sh | sh
      ```
    - [HueSync](https://github.com/honjow/HueSync) - Decky Loader Plugin (NOTE: This is only needed if your device has RGB lighting on it, otherwise skip.)
      ```
      curl -L https://raw.githubusercontent.com/honjow/huesync/main/install.sh | sh
      ```

- Open Terminal and enter these commands:
  - Enable InputPlumber to run on boot
	```
	sudo systemctl enable inputplumber && sudo systemctl start inputplumber
 	```
- Reboot Device.
	- If you installed the ChimeraOS Kernel, hold down "Shift" while booting. This should open the Kernel menu, select the kernel that mentions "ChimeraOS" on it, if it's not already selected.
	- You will also need to set this kernel as default, if it's not defaulted already. This process varies depending on your setup. Search the web for "How to change default kernel" for your setup.
 	- Periodically you will need to manually update the kernel.
- Open Steam app and log in.
	- Go to Steam Settings > Compatability, and toggle on "Enable Steam Play for all other titles".
	- If you are unable to control the mouse pointer with the controller. Open Settings > Controller > Scroll down to 'Desktop Layout'. Enable 'Steam Input' and setup the controls.
	- Optional: Open Settings > Interface > enable "Run Steam when my computer starts" and also enable "Start Steam in Big Picture Mode" if you want it to feel more like a SteamDeck when it boots up.
## Recommendations:
- Enable Accessibility feature "on-screen keyboard" in settings (if available).
- Enable Auto-Login.
- Disable Lock Screen and Sleep Lock.
- Set your TDP to the recommended amount for your chipset/device using SimpleDeckyTDP.
	- Many devices can be set to a max of 25W-30W. Then use the per-game setting to adjust the current TDP lower to save battery life on lower end games.
- Set your controller to "Steam Deck" using DeckyPlumber.
- Install "CSS Loader" and "[Handheld Controller Glyphs](https://github.com/victor-borges/handheld-controller-glyphs)" to change all the glyphs in the UI to match your device.

## Fixes:
### Audio Fix:
- Download and install the Audio Driver file in this repo: [aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst](https://github.com/dansl/Steamy-Handheld-PC/raw/refs/heads/main/aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst)
	- This is available via AUR, but it's URL is broken... [(See Comments Here)](https://aur.archlinux.org/packages/aw87559-firmware)

### Popping Speakers Fix:
- Create/edit file at "/etc/modprobe.d/audio.conf"
	```
	sudo nano /etc/modprobe.d/audio.conf
	```
- Paste this text into terminal
	```
	options snd_hda_intel power_save=0 power_save_controller=N
	```
- Press Ctrl + x, press y, press enter to save and finish.

### Game Not Loading Fix:
- If a game in Steam is not loading properly. Try adding this line to the games "Launch Options".
	```
	SteamDeck=1 %command%
	```
### Ayaneo Slide Kernel Param Fix:
- Edit your GRUB config
	```
	sudo nano /etc/default/grub
	```
- Find the line below and add acpi=strict to the end of the list of parameters inside the quotes.
	 ```
	 GRUB_CMDLINE_LINUX_DEFAULT="..."
	 ```
	EXAMPLE: GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi=strict"
- Save and exit the editor (Ctrl + x, press y, press enter).
- Regenerate the GRUB config:
	 ```
	 sudo update-grub
	 ```
- Reboot

For tips on how to use Linux, check out my [Linux Tips](https://github.com/dansl/LinuxOS-Stuff)!
