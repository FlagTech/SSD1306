# SSD1306_FLAG_I2C

The scroll method in the MicroPython's ssd1306 class is based on [FrameBuffer class's scroll()](https://docs.micropython.org/en/v1.15/library/framebuf.html#framebuf.FrameBuffer.scroll).That may leave a footprint of the previous colors in the FrameBuffer.

By adopting the [modified version of the ssd1306 class](https://github.com/timotet/SSD1306/blob/master/ssd1306.py) written by Tim Toliver, we create a new SSD1306_I2C_FLAG class to reuse the code already in the built-in ssd1306 class. The SSD1306_I2C_FLAG class has the following methods added:

- clear()：clear the OLED.
- hw_scroll_h(direction=True, interval=FRAMES_5):continuously scroll to right/left.
    - direction: True/False, scroll to right/left.
    - interval:
        - SSD1306_FLAG_I2C.FRAMES_2
        - SSD1306_FLAG_I2C.FRAMES_3
        - SSD1306_FLAG_I2C.FRAMES_4
        - SSD1306_FLAG_I2C.FRAMES_5
        - SSD1306_FLAG_I2C.FRAMES_25
        - SSD1306_FLAG_I2C.FRAMES_64
        - SSD1306_FLAG_I2C.FRAMES_128
        - SSD1306_FLAG_I2C.FRAMES_256
        One frame approximate 9.19ms. 
- hw_scroll_diag(direction=True, interval=FRAMES_5, offset=1):continuously scroll to up-right/up-left.
    - offset: vertical offset per scroll.
- hw_scroll_off()：stop scrolling.

## Usage example

```python
from machine import Pin, I2C
from ssd1306_i2c_flag import SSD1306_I2C_FLAG
from time import sleep

i2c = I2C(scl=Pin(5), sda=Pin(4))
oled = SSD1306_I2C_FLAG(128, 64, i2c)

while True:
    oled.text("I love PYTHON!", 0, 0)
    oled.text("I love PYTHON!", 0, 16)
    oled.text("I love PYTHON!", 0, 32)
    oled.show()
    oled.hw_scroll_h(interval=oled.FRAMES_128)
    sleep(2)
    oled.hw_scroll_h(False)
    sleep(2)
    oled.hw_scroll_off()
    sleep(2)
    oled.hw_scroll_diag(interval=oled.FRAMES_25)
    sleep(2)
    oled.hw_scroll_diag(False, offset=3)
    sleep(2)
    oled.clear()
    sleep(2)
```
