# LED Driver - Starfish
ESP32 based LED driver designed for use with ESPHome.

The pcb is designed with KiCad and has a ESP32-S3 with power and usb circuit. A total of four N-Channel FETs allow to switch 12V appliances (LED) upto 4.4A each. The four channels allow multiple LED strips or ones with RGBW etc.



<img src="./assets/starfish-b-render-open.png" alt="starfish-b-render" style="width:50%;" /><img src="./assets/starfish-b-render.png" alt="starfish-b-render" style="width:50%;" />



## Features

- ESP32-S3 for use with ESPHome

- 4 Open-Drain Channels for RGBW
- 2 Buttons for user input
- WS2812 LED
- Multiple headers for further peripherals (I2C,  3V3, 5V, GPIOs)
- 6V to 15V Input



## ESPHome Code

YAML configuration for ESPHome. Adapt as you need for your setup. Code is relevant for latest version (rev B).

### Base

```yaml
substitutions:
  dev_name: Kitchen

esphome:
  name: kitchen-starfish
  friendly_name: kitchen-starfish
  comment: ESP32 based LED driver for 12 V LED Strips

  on_boot:
    light.turn_on:
      id: status_led
      brightness: 20%
      green: 30%
      
esp32:
  variant: esp32s3
  board: esp32-s3-devkitc-1
  framework:
    type: arduino
```



### Internal LED

```yaml
light:
  - platform: esp32_rmt_led_strip
    id: status_led
    rgb_order: GRB
    pin: GPIO38
    num_leds: 1
    rmt_channel: 0
    chipset: ws2812
    name: "Status"
    internal: true
```



### PWM Output

```yaml
output:
  - platform: ledc
    id: output_component1
    pin: GPIO6
    frequency: 19531

  - platform: ledc
    id: output_component2
    pin: GPIO5
    frequency: 19531
```



### Dimmable Light

```yaml
light:
  - platform: monochromatic
    name: ${dev_name} Ceiling
    output: output_component1
```



### Buttons

```yaml
binary_sensor:
  - platform: gpio
    pin: 
      number: GPIO8
      inverted: True
    name: ${dev_name} BTN_1

  - platform: gpio
    pin: 
      number: GPIO11
      inverted: True
    name: ${dev_name} BTN_2
```



### I2C Header

```yaml
i2c:
  scl: GPIO9
  sda: GPIO10
```

