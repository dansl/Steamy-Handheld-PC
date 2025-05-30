# Steamy Ayaneo
Ditch Windows and turn your Ayaneo handheld PC into a SteamOS-like device with Arch Linux.

**WORK IN PROGRESS**

For now, this is just a rough idea of how to run Arch Linux properly on your Ayaneo Handheld.

## Devices Tested
 - Ayaneo Air 1S
 - Ayaneo Flip DS


## Things you need
- Ayaneo PC x86/64
- Keyboard + Mouse (not needed on some devices, but makes things easier at first)
- USB Stick loaded with bootable [Arch](https://archlinux.org) on it.
	- While you can run stock Arch, I prefer an Arch distro. My personal choice is [Manjaro](https://manjaro.org) with GNOME, but some other great options are [EndeavourOS](https://endeavouros.com) or [CachyOS](https://cachyos.org) or many others.

## How to start
- First, use the USB stick to install your chosen Arch Linux on your device.
  - You can access the boot menu by mashing F7 while the device boots.
- Once booted into the OS, install these packages.
  - Using Pacman or PAMAC
    - inputplumber
    - steam
    - steam-powerbuttond
    - ayaneo-platform-dkms
    ```
    sudo pacman -S inputplumber steam-powerbuttond-git steam
    ```
  - Using AUR
    - chimeraos-device-quirks
    - ayaneo-platform-dkms
    ```
    yay -S chimeraos-device-quirks ayaneo-platform-dkms
    ```

  - Manually install these using ```sudo pacman -S [path_to_file]``` or depending on the distro, just opening the file will install it.
    - ChimeraOS Kernel & Header (https://github.com/ChimeraOS/linux-chimeraos)
    - Audio Firmware (File in this Repo: aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst)

  - Optional but Highly Recommended
    - Decky Loader (https://github.com/SteamDeckHomebrew/decky-loader)
      ```
      curl -L https://github.com/SteamDeckHomebrew/decky-installer/releases/latest/download/uninstall.sh | sh
      ``` 
    - SimpleDeckyTDP (https://github.com/aarron-lee/SimpleDeckyTDP)
      ```
      curl -L https://github.com/aarron-lee/SimpleDeckyTDP/raw/main/install.sh | sh
      ```
    - DeckyPlumber (https://github.com/aarron-lee/DeckyPlumber)
      ```
      curl -L https://github.com/aarron-lee/DeckyPlumber/raw/main/install.sh | sh
      ```
    - HueSync (https://github.com/honjow/HueSync)
      ```
      curl -L https://raw.githubusercontent.com/honjow/huesync/main/install.sh | sh
      ```

- Open Terminal and enter these:
  - Enable InputPlumber
	```
	sudo systemctl enable inputplumber && sudo systemctl start inputplumber
 	```

  - Fix popping sound with speakers
	  ```
	  echo "options snd_hda_intel power_save=0 power_save_controller=N" > /etc/modprobe.d/audio.conf
	  ```
- Reboot Device, and while booting hold down "Shift". Select the kernel that mentions "ChimeraOS" on it, if it's not already selected.
	- You need to set this kernel as default, if it's not already. This process varies depending on your setup. Search the web for "How to change default kernel" for your setup.
- Open Steam app.
	- If you are unable to control the mouse with the controller. Open Settings > Controller > Scroll to 'Desktop Layout'. Enable 'Steam Input' and setup controls.
	- Open Settings > Interface > enable "Run Steam when my computer starts".
		- Optional: Also enable "Start Steam in Big Picture Mode" if you want it to feel more like a SteamDeck
- If bluetooth isn't working, downgrade bluez to 5.68.
- Recommendations:
	- Enable Accessibility feature "on-screen keyboard".
 	- Enable Auto-Login
 	- Disable Lock Screen and Screen Sleep Lock
  	- Set your TDP to the recommended amount for your chipset/device using SimpleDeckyTDP
  	- Set your controller to "Steam Deck" using DeckyPlumber
