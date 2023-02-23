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

´´´
import board
import busio
import gifio
import displayio
import time
import adafruit_ili9341

dc=board.D5
rst=board.D4
cs=board.D2 #My display does not have it...

displayio.release_displays()
spi = busio.SPI(clock=board.SCK, MOSI=board.MOSI, MISO=board.MISO)
display_bus = displayio.FourWire(spi, command = dc, chip_select=cs, reset=rst)

display = adafruit_ili9341.ILI9341(display_bus, width=320, height=240)
splash = displayio.Group()
display.show(splash)
´´´
