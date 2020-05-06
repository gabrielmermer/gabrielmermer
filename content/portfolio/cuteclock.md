+++
showonlyimage = false
draft = true
image = "img/portfolio/clock.jpg"
date = "2020-04-05"
title = "Cute clock"
weight = -2
+++

clock with RGB backlight and custom quotes
<!--more-->


![clock](/img/portfolio/clock.jpg)

features:
* tells the time
* shows custom messages
* changes the backlight on every new message
* shines with custom animations

The clock itself is powerd by Rasperry Pi 3, and a custom python script.


Source code below:

```python
import time
import math
import csv
import Adafruit_CharLCD as LCD
import random
import datetime
import threading

#todo custom chars
#todo more mesages
#todo night mode
#todo time spaceer 2 char art


colors = {"RED": (1.0, 0.0, 0.0),
            "GREEN": (0.0, 1.0, 0.0),
            "CYAN": (0.0, 1.0, 1.0),
            "YELLOW": (1.0, 1.0, 0.0),
            "WHITE": (1.0, 1.0, 1.0),
            "PINK": (1.0, 1.0, 0.6)
            }
oneDecimalNumbers = {
    0: '00',
    1: '01',
    2: '02',
    3: '03',
    4: '04',
    5: '05',
    6: '06',
    7: '07',
    8: '08',
    9: '09',
}


with open('words.txt', 'r') as f:
    # lines = f.readlines()
    words = f.read().splitlines() 

def tellTheTime(separatorCounter):
    now = datetime.datetime.now()
    storedMinute = now.minute
    storedHour = now.hour
     # linux hour bug fix
    storedHour += 1 
    if storedMinute in oneDecimalNumbers.keys():
        storedMinute = oneDecimalNumbers[storedMinute]
    else:
        pass
    if storedHour in oneDecimalNumbers.keys():
        storedHour = oneDecimalNumbers[storedHour]
    else:
        pass
   
    print("SC = "+ str(separatorCounter))
    if separatorCounter == 0:

        timeString = str(storedHour) + "  " + str(storedMinute)    

    elif separatorCounter == 1:

        timeString = str(storedHour) + "\x06 " + str(storedMinute)

    elif separatorCounter == 2:

        timeString = str(storedHour) + "\x06\x07" + str(storedMinute)

    elif separatorCounter == 3:

        timeString = str(storedHour) + "\x06\x04" + str(storedMinute)

    elif separatorCounter == 4:

        timeString = str(storedHour) + "\x05\x04" + str(storedMinute)



    else:
        timeString = "BUG"
    # if separatorCounter == True:

    #     timeString = str(storedHour) + "\x05\x04" + str(storedMinute)

    # else:

    #     timeString = str(storedHour) + "  " + str(storedMinute)

    lcd.set_cursor(0, 1)
    lcd.message("\x03    " + timeString + "    \x03")
 
def loveMessage():

    lcd.clear()
    lcd.home()
    color = random.choice(list(colors.keys()))
    message = random.choice(words)
    #todo delete
    lcd.set_color(colors[color][0], colors[color][1], colors[color][2])
    lcd.message(message)

def hsv_to_rgb(hsv):
    """Converts a tuple of hue, saturation, value to a tuple of red, green blue.
    Hue should be an angle from 0.0 to 359.0.  Saturation and value should be a
    value from 0.0 to 1.0, where saturation controls the intensity of the hue and
    value controls the brightness.
    """
    # Algorithm adapted from http://www.cs.rit.edu/~ncs/color/t_convert.html
    h, s, v = hsv
    if s == 0:
        return (v, v, v)
    h /= 60.0
    i = math.floor(h)
    f = h-i
    p = v*(1.0-s)
    q = v*(1.0-s*f)
    t = v*(1.0-s*(1.0-f))
    if i == 0:
        return (v, t, p)
    elif i == 1:
        return (q, v, p)
    elif i == 2:
        return (p, v, t)
    elif i == 3:
        return (p, q, v)
    elif i == 4:
        return (t, p, v)
    else:
        return (v, p, q)



# Raspberry Pi configuration:
lcd_rs = 27  # Change this to pin 21 on older revision Raspberry Pi's
lcd_en = 22
lcd_d4 = 25
lcd_d5 = 24
lcd_d6 = 23
lcd_d7 = 18
lcd_red   = 4
lcd_green = 17
lcd_blue  = 7  # Pin 7 is CE1



# Define LCD column and row size for 16x2 LCD.
lcd_columns = 16
lcd_rows    = 2



# Initialize the LCD using the pins above.
lcd = LCD.Adafruit_RGBCharLCD(lcd_rs, lcd_en, lcd_d4, lcd_d5, lcd_d6, lcd_d7,
                              lcd_columns, lcd_rows, lcd_red, lcd_green, lcd_blue)




# <3 symbol
lcd.create_char(3, [
                0B00000,
                0B00000,
                0B00000,
                0B01010,
                0B11111,
                0B01110,
                0B00100,
                0B00000])
#time spacer symbol
lcd.create_char(4, [
                0B00000,
                0B00000,
                0B01000,
                0B00000,
                0B00000,
                0B01000,
                0B00000,
                0B00000])
#time spacer symbol 2
lcd.create_char(5, [
                0B00000,
                0B00000,
                0B00010,
                0B00000,
                0B00000,
                0B00010,
                0B00000,
                0B00000])
# testing the loop animation chars

# 1 
lcd.create_char(6, [
                0B00000,
                0B00000,
                0B00010,
                0B00000,
                0B00000,
                0B00000,
                0B00000,
                0B00000])
# 2
lcd.create_char(7, [
                0b00000,
                0b00000,
                0b01000,
                0b00000,
                0b00000,
                0b00000,
                0b00000,
                0b00000])
# 3
lcd.create_char(1, [
                0b00000,
                0b00000,
                0b00000,
                0b00000,
                0b00000,
                0b01000,
                0b00000,
                0b00000])
# 4
lcd.create_char(2, [
                0B00000,
                0B00000,
                0B00010,
                0B00000,
                0B00000,
                0B00010,
                0B00000,
                0B00000])

separatorCounter = 0
messageRefreshCounter = 0
maxAnimationReached = True
countDown = 0
# todo delete
# separatorCounter = False
loveMessage()
while True:

    now = datetime.datetime.now()
       

    messageRefreshCounter += 1
    # separatorCounter = not separatorCounter

    tellTheTime(separatorCounter)
    # todo change to 600 (10 min)
    if messageRefreshCounter >= 600:

        loveMessage()
        messageRefreshCounter = 0

    time.sleep(1) 

    # separator changing animations
    print("Countdown = "+ str(countDown))
    if separatorCounter == 4:
        countDown = 4
    if countDown > 0:
        separatorCounter += -1
    else:
        separatorCounter += 1
    countDown += -1
    # if separatorCounter >= 3:
    #     separatorCounter = 0
    # else:
    #     separatorCounter += 1

    print(separatorCounter)
```
