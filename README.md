# Steamy Handheld PC
Ditch Windows and turn your handheld PC into a SteamOS-like device with Arch Linux.

**!! WORK IN PROGRESS !!**

For now, this is just a rough idea of how to run Arch Linux properly on your Handheld. I am not using Gamescope session, as it can tend to be a bit more complicated to setup. This will only set up running Steam in "Big Picture Mode" for a "SteamOS-like" desktop experience. Alternativly, you could also just install [ChimeraOS](https://chimeraos.org), but if that distro ever stops being supported or you would rather just use your own Arch setup, then this is a good starting point.

## Features

- Everything should function just like you were using Windows Desktop, but it's 100x better because Linux!
- Use Decky Loader + SimpleDeckyTDP to adjust your GPU and CPU power consumption to maximize your battery.
- Use InputPlumber to rebind any button on your device or emualte any controller.
- Adjust your devices RGB lighting using HueSync.

## Devices Tested

| Device | Work? | Notes |
| ----- | ----- | ----- |
| Ayaneo Air 1S | ✅ | Requires ChimeraOS Kernel, [Sleep Fix](#sleep-fix), and [Audio Fix](#audio-fix). |
| Ayaneo Flip DS | ✅ | Requires ChimeraOS Kernel, [Sleep Fix](#sleep-fix), and [Audio Fix](#audio-fix). The bottom screen works but has no touch input. |
| Ayaneo Slide/Antec Core HS | ✅ | Requires [Kernel Param Fix](#ayaneo-slide-kernel-param-fix) or else it will randomly crash. Sleep needs to be set to "Modern Standby + Si02" then can only be triggered from the interface and not via the power button, see [Sleep Fix](#sleep-fix). |

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
    - [Decky Loader](https://github.com/SteamDeckHomebrew/decky-loader) - Accessed via Steam Big Picture right side menu.
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
	 - Many devices can be set to a Max of 25W-30W, but don't go too high or else your device will crash and reboot.
 	- PRO TIP: Turn on Steam's FPS overlay and start playing your game. Then trying dropping the current TDP to 10W and test the game... If your FPS drops, then try setting to 15W and test. If FPS drops go up some more, but if it's stable then either stop there or try to bring it down a bit. After a few adjustments you will know what TDP is good for stable FPS and much better battery life!
- Set your controller to "Steam Deck" mode using DeckyPlumber.
- Install "CSS Loader" Decky Loader Plugin and "[Handheld Controller Glyphs](https://github.com/victor-borges/handheld-controller-glyphs)" to change all the glyphs in the UI to match your device.
- Install [Conky](https://github.com/brndnmtthws/conky) for monitoring system resources on your desktop.
  - For instance, on the Ayaneo Flip DS, you can play games on the top screen and monitor your system resources on the bottom screen. Use my [FlipDS Conky config](https://raw.githubusercontent.com/dansl/Steamy-Handheld-PC/refs/heads/main/Conky-config-FlipDS.txt) to get started. Download the file and drop it in your "~/" home folder and rename it ".conkyrc", then open Conky. You will also need to install "radeontop" via pacman for GPU stats. [Screenshot](https://raw.githubusercontent.com/dansl/Steamy-Handheld-PC/refs/heads/main/Conky.jpg)

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
- Find ```GRUB_CMDLINE_LINUX_DEFAULT``` and add ```acpi=strict``` to the end of the list of parameters inside the quotes.
- Save and exit the editor (Ctrl + x, press y, press enter).
- Regenerate the GRUB config:
	 ```
	 sudo update-grub
	 ```
- Reboot

### Sleep Fix
Newer AMD APU's do not support "S3 Sleep" and should instead use "Modern Standby". Some devices already have "Modern Standby" enabled, some can enable this feature via BIOS firware settings, and others might need to use the 3rd party "Smokeless UMAF".

- Enter your BIOS firmware settings (Press ESC during bootup) and see if you have Advanced options with "AMD PBS" listed. If so, skip to the next section labeled "Enable Modern Standby".
	- Format a USB stick with FAT32.
	- Download [Smokeless UMAF](https://github.com/DavidS95/Smokeless_UMAF/raw/main/UMAF_BETA.zip).
	- Extract `UMAF_Beta.zip` and copy the contents into the root of the usb stick.
	- Boot your device and select the USB stick from the boot menu. (Press F7 during bootup for menu)
	- Navigate to the "Front Page" tab and select "Device Manager".
 	- Continue Below.

- Enable Modern Standby
	- Select "AMD PBS" then "Power Saving Configurations".
	- Under "S3/Modern Standby Support" change the entry to "Modern Standby".
	- Under "Modern Standby Type" select "Modern Standby + S0i2 + S0i3".
 		- For the AYANEO Slide/Antec Core HS use "Modern Standby + S0i2".
	- Save changes and exit, allowing the device to reboot.

### Updating Ayaneo BIOS/EC without Windows
- Download your devices newest firmware files from [Ayaneo Support](https://ayaneo.com/support/download).
- Download [shellx64](https://github.com/pbatard/UEFI-Shell/releases/download/24H1/shellx64.efi).
- Format a USB drive as FAT32 and create the folders `/EFI/Boot/` on the root of the USB drive.
- Place `shellx64.efi` into the Boot folder and rename it to `BootX64.efi`
- Extract the Ayaneo firmware files to the USB drive. Make sure the .bin file that contains the update is on the root of the USB drive, as well as the file called `AfuEfix64.efi`.
- Create a new plain text file on the root of the USB drive and name it `startup.nsh`.
- Place the following text into startup.nsh, replacing `<BIOS>` with the full name of the .bin file containing the BIOS update:
```
fs1:
AFUEFIx64 <BIOS> /p /b /n /k /L /REBOOT
```
- Ensure you have more than 50% battery and your device is plugged in.
- Plug the USB drive into your device, and hold "LC and Volume+" until the Ayaneo logo appears, or push F7 during bootup.
- Select the USB drive from the boot menu using the "Ayaneo button" to select and "volume button" to confirm.
- ***Do not press anything or remove the drive until the BIOS update completes and the device completely reboots! Interrupting the update in any way can brick your device!***

For tips on how to use Linux, check out my [Linux Tips](https://github.com/dansl/LinuxOS-Stuff)!
