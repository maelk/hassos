# Raspberry PI

Supported Hardware:

| Device | Board |
|--------|-----------|
| Raspberry Pi A+/B/B+| rpi |
| Raspberry Pi Zero | rpi |
| Raspberry Pi Zero W | rpi0-w |
| Raspberry Pi 2 B | rpi2 |
| Raspberry Pi 3 B/B+ | rpi3 / rpi3-64 |

## Limitation 64bit

The 64bit version is under development by RPi-Team. It work very nice but it could have some impacts. Actual we see that the SDcard access with ext4 are a bit slower than on 32bit.

## Serial console

For access to terminal over serial console, add `console=ttyAMA0,115200` to `cmdline.txt` and `enable_uart=1`, `dtoverlay=pi3-disable-bt` into `config.txt`. GPIO pins are: 6 = GND / 8 = UART TXD / 10 = UART RXD.

## I2C

Add `dtparam=i2c1=on` and `dtparam=i2c_arm=on` to `config.txt`. After that we create a module file on host with [config usb stick][config] or direct into `/etc/modules-load.d`.

rpi-i2c.conf:
```
i2c-dev
i2c-bcm2708
```

## Camera

In order to use the Raspberry Pi camera, the `bcm2835-v4l2` module can be used to expose it and make it available as `/dev/video0`, that can be then used in a Docker container, when specified.

For this, the default Raspberry Pi Firmware is not sufficient, the X one is needed. (`BR2_PACKAGE_RPI_FIRMWARE_X=y` in `buildroot-external/configs/rpiX_defconfig`). Then we create a module file on the host with [config usb stick][config] or directly into `/etc/modules-load.d`.

bcm2835-v4l2.conf:
```
bcm2835-v4l2
```

## Tweaks

If you don't need bluetooth, disabled it with add `dtoverlay=pi3-disable-bt` into `config.txt`.

[config]: ../configuration.md#automatic
