# Steamy Handheld PC
Ditch Windows and turn your handheld PC into a SteamOS-like device with Arch Linux.

**!! WORK IN PROGRESS !!**

For now, this is just a rough idea of how to run Arch Linux properly on your Handheld. I am not using Gamescope session, as it can tend to be a bit more complicated to setup. This will only set up running Steam in "Big Picture Mode" for a "SteamOS-like" experience.

## Devices Tested
 - Ayaneo Air 1S: No known issues.
 - Ayaneo Flip DS: Only issue is the bottom screen has no touch input, but still works visually.

## Things you need
- Handheld PC x86/64
- Keyboard + Mouse (not needed on some devices, but makes things easier at first)
- USB Stick loaded with bootable [Arch](https://archlinux.org) on it.
	- While you could run stock Arch, I prefer an Arch distro. My personal choice is [Manjaro](https://manjaro.org) with GNOME, but some other great options are [EndeavourOS](https://endeavouros.com) or [CachyOS](https://cachyos.org) or many others.

## How to start
- First, use the USB stick to install your chosen Arch distro on your device.
  - You can usually access the boot menu by mashing F7 while the device boots.
- Once booted into the OS, install these packages.
  - Using Pacman
    - inputplumber
    - steam
    ```
    sudo pacman -S inputplumber steam
    ```
  - Using AUR
    - chimeraos-device-quirks
	```
	yay -S chimeraos-device-quirks
	```
    - FOR AYANEO DEVICES ONLY: ayaneo-platform-dkms-git
	```
	yay -S ayaneo-platform-dkms-git
	```

  - Manually install
    - ChimeraOS Kernel & Header (https://github.com/ChimeraOS/linux-chimeraos/releases)
    	- Download both the Kernel and Header files, then install them using ```sudo pacman -S [path_to_file]``` or depending on the Arch distro, just opening the file should install it.
    - Decky Loader (https://github.com/SteamDeckHomebrew/decky-loader)
      ```
      curl -L https://github.com/SteamDeckHomebrew/decky-installer/releases/latest/download/install_release.sh | sh
      ``` 
    - SimpleDeckyTDP (https://github.com/aarron-lee/SimpleDeckyTDP)
      ```
      curl -L https://github.com/aarron-lee/SimpleDeckyTDP/raw/main/install.sh | sh
      ```
    - DeckyPlumber (https://github.com/aarron-lee/DeckyPlumber)
      ```
      curl -L https://github.com/aarron-lee/DeckyPlumber/raw/main/install.sh | sh
      ```
    - HueSync (https://github.com/honjow/HueSync) NOTE: This is only needed if your device has color LED lights on it, otherwise ignore this one.
      ```
      curl -L https://raw.githubusercontent.com/honjow/huesync/main/install.sh | sh
      ```

- Open Terminal and enter these commands:
  - Enable InputPlumber to run on boot
	```
	sudo systemctl enable inputplumber && sudo systemctl start inputplumber
 	```       
- Reboot Device, and while booting hold down "Shift". This should open the Kernel menu, select the kernel that mentions "ChimeraOS" on it, if it's not already selected.
	- You will need to set this kernel as default, if it's not already. This process varies depending on your setup. Search the web for "How to change default kernel" for your setup. Periodically you can manually update the kernel as Chimera releases new ones.
- Open Steam app.
	- If you are unable to control the mouse pointer with the controller. Open Settings > Controller > Scroll down to 'Desktop Layout'. Enable 'Steam Input' and setup the controls.
	- Optional: Open Settings > Interface > enable "Run Steam when my computer starts" and also enable "Start Steam in Big Picture Mode" if you want it to feel more like a SteamDeck when it boots up.
- Recommendations:
	- Enable Accessibility feature "on-screen keyboard" in settings (if available).
 	- Enable Auto-Login.
 	- Disable Lock Screen and Sleep Lock.
  	- Set your TDP to the recommended amount for your chipset/device using SimpleDeckyTDP.
  	- Set your controller to "Steam Deck" using DeckyPlumber.
  	- Install "CSS Loader" and "[Handheld Controller Glyphs](https://github.com/victor-borges/handheld-controller-glyphs)" to change all the glyphs in the UI to match your device.

- Possible fixes for some issues:
	- No Bluetooth: Try downgrading Bluez to v5.68.
   	- No Audio: Download and install the Audio Driver file in this repo: [aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst](https://github.com/dansl/Steamy-Handheld-PC/raw/refs/heads/main/aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst)
   		- This is available via AUR, but it's URL is broken... [(See Comments Here)](https://aur.archlinux.org/packages/aw87559-firmware) 
 	- Popping sound in speakers:
  		- Create/edit file at "/etc/modprobe.d/audio.conf"
		```
		sudo nano /etc/modprobe.d/audio.conf
		```
	   	- Paste this text into terminal
		```
		options snd_hda_intel power_save=0 power_save_controller=N
		```
	  	- Press ctrl+x then type 'y' and press enter to save and finish.
