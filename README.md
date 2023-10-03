# asahi-rocky-builder

Builds a minimal Rocky Linux image to run on Apple M1/M2 systems

<img src="https://user-images.githubusercontent.com/12903289/230797409-90e1df3a-a770-4631-84d2-7166609aa9cf.png" width=65%>

## Installing a Prebuilt Image

Make sure to update your macOS to version 12.3 or later, then just pull up a Terminal in macOS and paste in this command:

```sh
curl https://leifliddy.com/rocky.sh | sh
```

## Fedora Package Install
This image was built on a Fedora system  

```dnf install arch-install-scripts bubblewrap systemd-container zip```

**note:** ```qemu-user-static``` is only needed if building on a non-```aarch64``` system.  
- Until version 15.x is released for Fedora, install mksoi from git:  
`python3 -m pip install --user git+https://github.com/systemd/mkosi.git@v15.1`

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
You should definitely understand what this script does before running it.  
You can find more info here:  
<https://github.com/AsahiLinux/docs/wiki/Partitioning-cheatsheet>

## Boot from USB device

Once Linux is installed on an M1/M2 system, you can then boot a compatible usb drive via ```u-boot```  
This project will create a bootable Rocky Linux USB drive for M1 systems:  
<https://github.com/leifliddy/asahi-rocky-usb>

## Display and keyboard backlight

The `light` command can be used to adjust the screen and keyboard backlight.

```sh
light -s sysfs/leds/kbd_backlight -S 10
light -s sysfs/backlight/apple-panel-bl -S 50
```
