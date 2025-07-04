# Steamy Handheld PC
Ditch Windows and turn your handheld PC into a SteamOS-like device with Arch Linux.

**!! DISCLAIMER !!**

**Follow this guide "AT YOUR OWN RISK", there is always a chance to accidently brick your device. If you are not comfortable with that idea, then do NOT continue.**

Note: For now, this is just a rough idea of how to run Arch Linux properly on your Handheld. This will only set up running Steam in "Big Picture Mode" for a "SteamOS-like" desktop experience. Alternativly, you could also just install [ChimeraOS](https://chimeraos.org), but if that distro ever stops being supported or you would rather just use your own Arch setup, then this is a good starting point.

## Features

- Everything should function just like you were using Windows Desktop, but it's 100x better because Linux!
- Use Decky Loader + SimpleDeckyTDP to adjust your GPU and CPU power consumption to maximize your battery.
- Use InputPlumber to rebind any button on your device or emualte any controller.
- Adjust your devices RGB lighting using HueSync.

## Devices Tested

| Device | Work? | Notes |
| ----- | ----- | ----- |
| Ayaneo Air 1S | ✅ | Requires [ChimeraOS Kernel](#chimeraos-kernel), [Sleep Fix](#sleep-fix), [Audio Fix](#audio-fix) and [RGB Fix](#ayaneo-rgb-fix). |
| Ayaneo Flip DS | ✅ | Requires [ChimeraOS Kernel](#chimeraos-kernel), [Sleep Fix](#sleep-fix), and [Audio Fix](#audio-fix). The bottom screen works but has no touch input. |
| Ayaneo Slide/Antec Core HS | ✅ | Requires [Sleep Fix](#sleep-fix), and [Kernel Param Fix](#kernel-param-fix). |

## Things you need

- Handheld PC x86/64
- Keyboard + Mouse (not needed on some devices, but makes things easier at first)
- USB Flash Drive loaded with bootable [Arch](https://archlinux.org) on it.
	- While you could run stock Arch, I prefer an Arch distro. My personal choice is [Manjaro](https://manjaro.org) with GNOME or KDE, but some other great options are [EndeavourOS](https://endeavouros.com) or [CachyOS](https://cachyos.org) or many others.

## How to Setup

- First, setup the USB Drive to install your chosen Arch distro on your device.
  - Download an ISO image for the distro of your choice. You may need to unzip it so you are left with the .ISO file
  - Download [BalenaEtcher](https://etcher.balena.io) and use the app to wipe your USB drive and flash the ISO image to it.
  - Once flashing is complete, you can put the USB drive into your Handheld PC, and Reboot the device.
- You will need to boot into the USB drive. This can be done a few ways, and every device is different. You can sometimes access the boot menu by pressing F7 while the device boots, or press ESC. If neither of those work, try F8, F9, F10, F11 or F12. You should see somewhere in the menu to select USB as the boot device.
  - Once booted into the OS, you can follow the prompts to install the OS.
    - TIP: Remove any SD cards before installing, just so you don't accidentally install the OS to the SD card.
- Once your OS is installed and you have rebooted into the OS, install these packages.
  - Install Steam and InputPlumber
    - [inputplumber](https://github.com/ShadowBlip/InputPlumber) - For mapping your devices controls
    - [steam](https://wiki.archlinux.org/title/Steam) - Steam!
    ```
    sudo pacman -S inputplumber steam
    ```
  - Install Decky Loader and plugins
    - [Decky Loader](https://github.com/SteamDeckHomebrew/decky-loader) - Can be accessed via Steam Big Picture right side menu.
      ```
      curl -L https://github.com/SteamDeckHomebrew/decky-installer/releases/latest/download/install_release.sh | sh
      ```
    - [SimpleDeckyTDP](https://github.com/aarron-lee/SimpleDeckyTDP) - Decky Loader Plugin for tweaking your devices CPU and GPU power draw.
      ```
      curl -L https://github.com/aarron-lee/SimpleDeckyTDP/raw/main/install.sh | sh
      ```
    - [DeckyPlumber](https://github.com/aarron-lee/DeckyPlumber) - Decky Loader Plugin for changing controller layout.
      ```
      curl -L https://github.com/aarron-lee/DeckyPlumber/raw/main/install.sh | sh
      ```
    - [HueSync](https://github.com/honjow/HueSync) - Decky Loader Plugin for tweaking RGB lighting, can be ignored if your device doesn't have RGB lighting.
      ```
      curl -L https://raw.githubusercontent.com/honjow/huesync/main/install.sh | sh
      ```
- Enable InputPlumber to run on boot
```
sudo systemctl enable inputplumber && sudo systemctl start inputplumber
```
- Reboot Device.
- Open Steam app and login with Steam account.
	- If you are unable to control the mouse pointer with the controller. Open Settings > Controller > Scroll down to 'Desktop Layout'. Enable 'Steam Input' and setup the controls.
	- Optional: Open Settings > Interface > enable "Run Steam when my computer starts" and also enable "Start Steam in Big Picture Mode" if you want it to feel more like a SteamDeck when it boots up.
- See [Various Known Fixes](#various-known-fixes) for help if you have issues.

## Recommendations

- Enable Accessibility feature "on-screen keyboard" in settings (if available).
- Enable Auto-Login.
- Disable Lock Screen and Sleep Lock.
- Set your TDP to the recommended amount for your chipset/device using Decky Loader plugin SimpleDeckyTDP.
	- Many devices can be set to a Max of 25W-30W, but don't go too high or else your device will crash and reboot.
	- PRO TIP: Turn on Steam's FPS overlay and start playing your game. Then trying dropping the current TDP to 10W and test the game... If your FPS drops, then try setting to 15W and test. If FPS drops go up some more, but if it's stable then either stop there or try to bring it down a bit. After a few adjustments you will know what TDP is good for stable FPS and much better battery life!
- Set your controller to "Steam Deck" mode using Decky Loader plugin DeckyPlumber.
- Install "CSS Loader" Decky Loader Plugin and "[Handheld Controller Glyphs](https://github.com/victor-borges/handheld-controller-glyphs)" to change all the glyphs in the UI to match your device.
- Install [Conky](https://github.com/brndnmtthws/conky) for monitoring system resources on your desktop.
  - For instance, on the Ayaneo Flip DS, you can play games on the top screen and monitor your system resources on the bottom screen. Use my [FlipDS Conky config](https://raw.githubusercontent.com/dansl/Steamy-Handheld-PC/refs/heads/main/Conky-config-FlipDS.txt) to get started. Download the file and drop it in your "~/" home folder and rename it ".conkyrc", then open Conky. You will also need to install "radeontop" via pacman for GPU stats. [Screenshot](https://raw.githubusercontent.com/dansl/Steamy-Handheld-PC/refs/heads/main/Conky.jpg)

## Various Known Fixes

### ChimeraOS Kernel
Some devices work fine without this, only install if you are having issues.
- Download both [ChimeraOS Kernel & Header](https://github.com/ChimeraOS/linux-chimeraos/releases).
- Install them using `sudo pacman -S [path_to_file]` or depending on the Arch distro, just opening the file should install it.
- Reboot device, hold down "Shift" while booting. This should open the Kernel menu, select the kernel that mentions "ChimeraOS" on it, if it's not already selected.
	- You will also need to set this kernel as default, if it's not defaulted already. This process varies depending on your setup. Search the web for "How to change default kernel" for your setup.
- Periodically you will need to repeat these steps to manually update the kernel+header.

### Ayaneo RGB Fix
Ayaneo devices will need this package to change RGB lighting.
- [ayaneo-platform-dkms-git](https://github.com/ShadowBlip/ayaneo-platform)
```
yay -S ayaneo-platform-dkms-git
```

### Audio Fix:
- Download the Audio Driver file in this repo: [aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst](https://github.com/dansl/Steamy-Handheld-PC/raw/refs/heads/main/aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst)
	- Install it using `sudo pacman -S [path_to_file]` or depending on the Arch distro, just opening the file should install it.
	- NOTE: This is also available via AUR, but it's URL is broken at the moment... [(See Comments Here)](https://aur.archlinux.org/packages/aw87559-firmware)
		```
		yay -S aw87559-firmware
		```

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

### Kernel Param Fix:
There is a bug with the Slide/Core HS that makes the device crash randomly on Linux. So far, this has been the only known fix for it.
- Edit your GRUB config
	```
	sudo nano /etc/default/grub
	```
- Find `GRUB_CMDLINE_LINUX_DEFAULT` and add `acpi=strict` to the end of the list of parameters inside the quotes.
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
 		- For the AYANEO Slide/Antec Core HS use "Modern Standby + S0i2" and initiate sleep via UI and not with power button.
	- Save changes and exit, allowing the device to reboot.

### Updating Ayaneo BIOS/EC without Windows
- Download your devices newest firmware files from [Ayaneo Support](https://ayaneo.com/support/download).
- Download [shellx64.efi](https://github.com/pbatard/UEFI-Shell/releases).
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
- **Do not press anything or remove the drive until the BIOS update completes and the device completely reboots! Interrupting the update in any way can brick your device!**

### Using Gamescope in Desktop Mode
Using Gamescope can provide better performance and better compatability. So if you want to try out Gamescope you can follow these steps.

**Note: Once in Gamescope, the only way to exit is to either reboot your device or find a way to alt-tab and force close the app.**

- Install Gamescope via Pacman
```
sudo pacman -S gamescope
```
- Download [GameMode.desktop](https://raw.githubusercontent.com/dansl/Steamy-Handheld-PC/refs/heads/main/GameMode.desktop)
- Place `GameMode.desktop` in `~/.local/share/applications`
- Make the file executable with this command.
```
sudo chmod +x ~/.local/share/applications/GameMode.desktop
```
- Exit Steam if it's running.
- Go to your OS's application list and open the application named "Game Mode".

NOTE: If you have issues, you can try installing `gamescope-plus` via pacman.

### Using Gamescope Session
If you want to bypass Desktop mode completely. You can try out a "Gamescope Session", which skips desktop mode completely and boots directly into Steam Big Picture.
- Install Gamescope Session via Pacman
```
sudo pacman -S gamescope-session-git
```
- Logout, then switch your enviroment to "Steam Big Picture" and login.
	- If you are using GNOME, click on your username, then look in the lower right corner for a Cog icon to select enviroment.
- To return to Desktop, logout and change the enviroment back to GNOME or KDE, etc, then login.

For tips on how to use Linux, check out my [Linux Tips](https://github.com/dansl/LinuxOS-Stuff)!
