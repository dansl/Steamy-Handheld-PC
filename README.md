# Steamy-Ayaneo
Turn your Ayaneo handheld PC into a SteamOS-like device running Arch Linux.

**WORK IN PROGRESS**

For now, this is just a rough idea of how to run Arch Linux properly on your Ayaneo Handheld.

## Devices Tested
 - Ayaneo Air1S
 - Ayaneo Flip DS


## Things you need
- Ayaneo device
- Keyboard + Mouse (might not be needed on some devices, but still makes things easier in the beginning)
- USB Stick with a bootable Arch distro on it. (My personal choice is [Manjaro Linux](https://manjaro.org))

## How to start
- First, use the USB stick to install your chosen Arch Linux on your device.
  - You can access the boot menu by mashing F7 while the device boots.
- Once booted into the device, install these packages.
  - Using Pacman or PAMAC
    - inputplumber
    - Steam
    - steam-powerbuttond
    - ayaneo-platform-dkms
    ```
    sudo pacman -S inputplumber steam-powerbuttond steam ayaneo-platform-dkms
    ```
  - Using AUR
    - chimeraos-device-quirks
    ```
    yay -S chimeraos-device-quirks
    ```

  - Manually install
    - ChimeraOS Kernel & Header (https://github.com/ChimeraOS/linux-chimeraos)
    - Audio Firmware (File in repo: aw87559-firmware-8.0.1.10-1-x86_64.pkg.tar.zst)

  - Optional but Highly Recommended
    - Decky Loader (https://github.com/SteamDeckHomebrew/decky-loader)
    - SimpleDeckyTDP (https://github.com/aarron-lee/SimpleDeckyTDP)
    - HueSync (https://github.com/honjow/HueSync)
    - DeckyPlumber (https://github.com/aarron-lee/DeckyPlumber)

- Open Terminal and enter
	```
	sudo systemctl enable inputplumber && sudo systemctl start inputplumber
	```
- If you haven't already, do a REBOOT.
- Open Steam app.
	- If you are unable to control the mouse with the controller. Open Settings > Controller > Scroll to 'Desktop Layout'. Enable 'Steam Input' and setup controls.
	- Open Settings > Interface > enable "Run Steam when my computer starts".
		- Optional: Also enable "Start Steam in Big Picture Mode" if you want it to feel more like a SteamDeck
- If bluetooth isn't working, downgrade bluez to 5.68.
- I recommend you enable Accessibility feature "on-screen keyboard".