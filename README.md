![logo](https://github.com/MackaJunest/Turbo-Drive/assets/95353708/3a06846e-b842-41a0-ab2a-13b1df655202)
# Turbo-Drive
An ESP32-based multifunctional servo & BLDC motor controller

## Overview

This is a part of the robotic dog project [Turbo-Pup](https://github.com/MackaJunest/Turbo-Pup). This board serves as a lower computer for motion-related control (direct motor control, kinematics, IMU data collection, etc.). My principle in designing this controller is to have an easy-to-use, reliable, multifunctional motor driving board that is suitable for multitask usages.
![Turbo_Drive](https://github.com/MackaJunest/Turbo-Drive/assets/95353708/e448c803-73c9-4991-ad39-4416c5ea1a47)
![2](https://github.com/MackaJunest/Turbo-Drive/assets/95353708/20dcd9bc-c17d-4b01-a3cb-455c4b6b0098)
![3](https://github.com/MackaJunest/Turbo-Drive/assets/95353708/65ec1aea-aed2-4f3d-a497-13771bbfbb1a)

## Features

### Hardware 
- **MCU**: ESP32-S3 is a dual-core XTensa LX7 MCU, capable of running at 240 MHz. Apart from its 512 KB of internal SRAM, it also comes with integrated 2.4 GHz, 802.11 b/g/n Wi-Fi and Bluetooth 5 (LE) connectivity that provides long-range support. It has 45 programmable GPIOs and supports a rich set of peripherals. ESP32-S3 supports larger, high-speed octal SPI flash, and PSRAM with configurable data and instruction cache.
- **Signal Isolation**: Two SN74HC244 linear drivers are used here. They prevent the GPIOs from being directly connected to signal pins from motors, which helps to prevent the ESP32 from being damaged by accidental situations. Other than that, they also shift the level of the PWM output from 3.3V to 5V to increase compatibility.
- **Power Supply**: The Turbo Drive will need three pairs of power: one for Servos (6.8V), one for BLDC motors (12V), and one for powering the ESP32 and Raspberry Pi 4B (5V). I only include one LDO for ESP32 to lower 5V to 3.3V but use an external DC-DC converter, because it would be a pain to pack all these components into such a PCB. Other than that, I prefer to place the big converters on the lower deck with the battery for better space organization. Therefore, I've used two maximum 20A DC-DC converters for the 6.8V and 5V power supply. (I was using a 5A DC-DC converter for my Raspberry Pi on my old puppy, but somehow, the PiOS always showed a Low Power warning on the screen, and I just want to get rid of it on this one.)
- **Current Sensing**: For this part, I've used four INA181A2 on the four leg servos. This can hopefully measure the current of the servos with a shunt resistor and thus estimate the torque distribution on each leg. I want to use this as leg-level calibration, because with servos it is pretty difficult to get good leveling during assembly.
- **Communication**: Turbo Drive has one pair of GPIO outputs with HX2.54 pins. These pins are designed for buses like I2C or UART, which could be used for extended modules whenever they are needed. It also has a USB micro port that is connected to a USB-to-UART bridge, which can be used both for programming and communication with Raspberry Pi.
- **On-Board IMU**: There is an integrated MPU6050 chip on Turbo Drive. It is not the best choice for an IMU and is also an outdated product, but it is already enough for the position tracking on the Turbo Pup project.

### Software
- **Programming**: 
I use PlatformIO with the Arduino framework to program the ESP32-S3 MCU because I am more familiar with the Arduino IDE, but after all, I think it also does a great job from all perspectives for me.

### How to use
- **Platformio Configuration:**
```ini
; PlatformIO Project Configuration File(.ini)
;
; Build options: build flags, source filter
; Upload options: custom upload port, speed and extra flags
; Library options: dependencies, extra library storages
; Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:esp32-s3-devkitc-1]
platform = espressif32
framework = arduino
board = esp32-s3-devkitc-1
board_build.mcu = esp32s3
board_build.f_cpu = 240000000L
build_flags = -DBOARD_HAS_PSRAM
board_build.arduino.memory_type = qio_opi
board_build.partitions = default_16MB.csv
board_upload.flash_size = 16MB
lib_deps = 
```

## License
This project is licensed under the GPL 3.0 License, allowing for unrestricted use, modification, and distribution. See the [LICENSE](LICENSE) file for details.

## Disclaimer
Use the Turbo Drive board at your own risk. The developers are not liable for any damage or injury caused by its use.

Happy building and coding with Turbo-Drive!
