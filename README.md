# SSD1306_FLAG_I2C

The scroll method in the MicroPython's ssd1306 class is based on [FrameBuffer class's scroll()](https://docs.micropython.org/en/v1.15/library/framebuf.html#framebuf.FrameBuffer.scroll).That may leave a footprint of the previous colors in the FrameBuffer.

By adopting the [modified version of the ssd1306 class](https://github.com/timotet/SSD1306/blob/master/ssd1306.py) written by Tim Toliver, we create a new SSD1306_I2C_FLAG class to reuse the code already in the built-in ssd1306 class. The SSD1306_I2C_FLAG class has the following methods added:

- clear()：clear the OLED.
- hw_scroll_h(direction=True):continuously scroll to right by default.
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
    oled.show()
    oled.hw_scroll_h()
    sleep(2)
    oled.hw_scroll_h(False)
    sleep(2)
    oled.hw_scroll_off()
    sleep(2)
    oled.clear()
    sleep(2)
```
