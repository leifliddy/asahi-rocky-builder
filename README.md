# asahi-rocky-builder

Builds a minimal Rocky Linux image to run on Apple silicon systems

<img src="https://github.com/leifliddy/asahi-rocky-builder/assets/12903289/567206d3-c2bd-42fa-b1a7-b3c9e0dd553a" width=65%>

## Installing a Prebuilt Image

Make sure to update your macOS to version 13.5 or later, then just pull up a Terminal in macOS and paste in this command:

```sh
curl https://leifliddy.com/rocky.sh | sh
```

## Fedora Package Install
This image was built on a Fedora system

```dnf install arch-install-scripts bubblewrap mkosi systemd-container zip```

#### Notes

- The ```qemu-user-static``` package is needed if building the image on a ```non-aarch64``` system  
- This project is based on `mkosi v19` which matches the current version of `mkosi` in the `F39` repo  
  https://src.fedoraproject.org/rpms/mkosi/  
  However....`mkosi` is updated so quickly that it's difficult to keep up at times (I have several projects based on `mkosi`)  
  I'll strive to keep things updated to the latest version supported in Fedora  
  If needed, you can always install a specific version via pip  
  `python3 -m pip install --user git+https://github.com/systemd/mkosi.git@v19`

### Notes

1. The root password is **rocky**
2. On the first boot the ```asahi-firstboot.service``` will run, selinux will be set to enforcing and the system will reboot.
3. This project utilizes rebuilt packages from the `Asahi Fedora Remix` project  
https://leifliddy.com/asahi-linux/9/aarch64/

## Setting up WiFi

`NetworkManager` is enabled by default.

To connect to a wireless network, use the following sytanx:
```nmcli dev wifi connect network-ssid```

An actual example:
```nmcli dev wifi connect blacknet-ac password supersecretpassword```

## Wiping Linux

Bring up a Terminal in macOS and run the following Asahi Linux script:  
```sudo curl -L https://alx.sh/wipe-linux | sh```  
You should definitely understand what this script does before running it. You can find more info here:  
<https://github.com/AsahiLinux/docs/wiki/Partitioning-cheatsheet>  

## Boot from USB device

Once Linux is installed, you can then boot a compatible usb drive via ```u-boot```  
This project will create a bootable Rocky Linux USB drive for Apple silicon systems:  
<https://github.com/leifliddy/asahi-rocky-usb>

## Persistently set your battery charge threshold to 80%
```sh
echo 'SUBSYSTEM=="power_supply", KERNEL=="macsmc-battery", ATTR{charge_control_end_threshold}="80"' | sudo tee /etc/udev/rules.d/10-battery.rules
```

## Display and keyboard backlight

The `light` command can be used to adjust the screen and keyboard backlight.

```sh
light -s sysfs/leds/kbd_backlight -S 10
light -s sysfs/backlight/apple-panel-bl -S 50
```

## Increase the terminal font size
On high-DPI displays, the terminal fonts (on the console) appear extremely small  
To increase the size, edit `/etc/vconsole.conf` and specify a larger font size, such as:  
```
FONT="latarcyrheb-sun32"
```
Then update grub for the change to take effect  
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
