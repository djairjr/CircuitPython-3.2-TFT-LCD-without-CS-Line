# CircuitPython-3.2-TFT-LCD-without-CS-Line
How to use this strange SPI LCD without CS Line...

This version of the TFT LCD display that I purchased here in Brazil does not come with the CS pin, which is used in many libraries. This offered me some challenge. At the end of the day, I indicated a pin on the board in the call to the driver, but there is no connection made between the CS pin (absent in the display) and the Raspberrry Pi Pico, or Seeed Xiao RP 2040.

The original version of this is in: https://www.instructables.com/Raspberry-Pi-Pico-CircuitPython-ILI9341/

Raspi PICO > DISPLAY

3V3 OUT (Pin 36) > VCC

Any GND > GND

SPI0SCK(Pin 4) > SCK

SPI0TX (Pin 5) > MISO

SP0RX (Pin 6) > MOSI

SPI0CS (Pin7) > RES (This display doesn´t have a CS Pin)

PIN 9 > DC

PIN10 > BLK

If you connect Seeed XIAO, the pinout is D10 > MOSI, D9> MISO, D8>SCK. This display does not have an CS line.
The DC and RST pins, could be any free pin (in my case, D4 and D5). 
```
import board
import busio
import displayio
import terminalio #Just a font
import adafruit_ili9341
from adafruit_display_text import label

displayio.release_displays()

# On Seeed Xiao RP2040
dc=board.D5
rst=board.D4
cs=board.D2 #My display does not have it...
spi = busio.SPI(clock=board.SCK, MOSI=board.MOSI, MISO=board.MISO)

# On Pico Change to
# dc=board.GP5
# rst=board.GP6
# blk=board.GP7
# cs=board.GP8 #My display does not have it...
# spi = busio.SPI(clock=board.GP2, MOSI=board.GP3, MISO=board.GP4)

display_bus = displayio.FourWire(spi, command = dc, chip_select=cs, reset=rst)

# This will initialize the Display
display = adafruit_ili9341.ILI9341(display_bus, width=320, height=240)
splash = displayio.Group()
display.show(splash)

# Draw a green background
color_bitmap = displayio.Bitmap(320, 240, 1)
color_palette = displayio.Palette(1)
color_palette[0] = 0x00FF00  # Bright Green

bg_sprite = displayio.TileGrid(color_bitmap, pixel_shader=color_palette, x=0, y=0)

splash.append(bg_sprite)

# Draw a smaller inner rectangle
inner_bitmap = displayio.Bitmap(280, 200, 1)
inner_palette = displayio.Palette(1)
inner_palette[0] = 0xAA0088  # Purple
inner_sprite = displayio.TileGrid(inner_bitmap, pixel_shader=inner_palette, x=20, y=20)
splash.append(inner_sprite)

# Draw a label
text_group = displayio.Group(max_size=10, scale=3, x=57, y=120)
text = "Hello World!"
text_area = label.Label(terminalio.FONT, text=text, color=0xFFFF00)
text_group.append(text_area)  # Subgroup for text scaling
splash.append(text_group)
```
