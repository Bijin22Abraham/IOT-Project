from m5stack import *
from m5ui import *
from uiflow import *
import time
from flow import ezdata
import wifiCfg
import unit
remoteInit()

setScreenColor(0x000000)
env3_3 = unit.get(unit.ENV3, unit.PORTA)
angle_1 = unit.get(unit.ANGLE, unit.PORTB)
pir_0 = unit.get(unit.PIR, unit.PORTC)
ir_0 = unit.get(unit.IR, unit.PORTB)


i = None
random2 = None

wifiCfg.autoConnect(lcdShow=False)

circle4 = M5Circle(300, 45, 20, 0xff9900, 0x000000)
title0 = M5Title(title="Smart Sleep Monitoring System", x=50, fgcolor=0xFFFFFF, bgcolor=0x0000FF)
label0 = M5TextBox(10, 56, "Temperature :", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label2 = M5TextBox(10, 109, "Humidity :", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label3 = M5TextBox(132, 55, "Text", lcd.FONT_Ubuntu, 0xffffff, rotate=0)
label5 = M5TextBox(132, 109, "Text", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
rect3 = M5Rect(236, 68, 1, 2, 0xFFFFFF, 0xFFFFFF)
rect4 = M5Rect(247, 63, 1, 2, 0xFFFFFF, 0xFFFFFF)
rect5 = M5Rect(261, 68, 1, 2, 0xFFFFFF, 0xFFFFFF)
rect6 = M5Rect(261, 68, 1, 2, 0xFFFFFF, 0xFFFFFF)
label6 = M5TextBox(200, 134, "Light :", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label7 = M5TextBox(261, 134, "Text", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label8 = M5TextBox(10, 158, "Movement:", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label9 = M5TextBox(132, 158, "Text", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
circle0 = M5Circle(262, 55, 15, 0xFFFFFF, 0xFFFFFF)
circle1 = M5Circle(247, 45, 15, 0xFFFFFF, 0xFFFFFF)
circle3 = M5Circle(236, 55, 15, 0xFFFFFF, 0xFFFFFF)
label1 = M5TextBox(10, 209, "Position :", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
label4 = M5TextBox(132, 209, "Text", lcd.FONT_Ubuntu, 0xFFFFFF, rotate=0)
rect = M5Rect(261, 35, 1, 1, 0xFFFFFF, 0xFFFFFF)
rectangle0 = M5Rect(237, 54, 1, 1, 0xFFFFFF, 0xFFFFFF)

import random


# Describe this function...
def rain():
  global i, random2
  circle4.setBgColor(0x000000)
  rgb.setColorAll(0x000099)
  rect3.setBorderColor(0x3333ff)
  rect4.setBorderColor(0x3333ff)
  rect5.setBorderColor(0x3333ff)
  rect6.setBorderColor(0x3333ff)
  random2 = random.randint(2, 50)
  rect3.setSize(height=random2)
  random2 = random.randint(2, 50)
  rect4.setSize(height=random2)
  random2 = random.randint(2, 50)
  rect5.setSize(height=random2)
  random2 = random.randint(2, 50)
  rect6.setSize(height=random2)

# Describe this function...
def sunny():
  global i, random2
  circle4.setBgColor(0xff6600)
  rgb.setColorAll(0xff6600)
  for i in range(20, 31):
    lcd.circle(300, 45, i, color=0xff9900)
    lcd.circle(300, 45, (i - 1), color=0x000000)
    wait(0.05)
    lcd.circle(300, 45, 30, color=0x000000)

# Describe this function...
def hot():
  global i, random2
  wait(0.11)
  circle4.setBgColor(0xff0000)
  rgb.setColorAll(0xff0000)
  for i in range(20, 31):
    lcd.circle(300, 45, i, color=0xff0000)
    lcd.circle(300, 45, (i - 1), color=0x000000)
    wait(0.05)
    lcd.circle(300, 45, 30, color=0x000000)


def buttonA_wasPressed():
  global i, random2
  ir_0.txOn()
  label7.setText('ON')
  rgb.setColorAll(0x0a320d)
  pass
btnA.wasPressed(buttonA_wasPressed)

def buttonB_wasPressed():
  global i, random2
  ir_0.txOn()
  label7.setText('OFF')
  rgb.setColorAll(0x4b3283)
  pass
btnB.wasPressed(buttonB_wasPressed)














while True:
  label3.setText(str("%.4f"%((env3_3.temperature))))
  label5.setText(str("%.4f"%((env3_3.humidity))))
  wait(0.1)
  if (env3_3.humidity) >= 55:
    rain()
  else:
    rect3.setBorderColor(0x000000)
    rect4.setBorderColor(0x000000)
    rect5.setBorderColor(0x000000)
    rect6.setBorderColor(0x000000)
    if (env3_3.temperature) >= 30:
      hot()
    else:
      sunny()
  wait(0.1)
  if (pir_0.state) == 1:
    rgb.setColorFrom(1, 5, 0x6600cc)
    speaker.tone(100, 10)
    label9.setText('True')
    ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Motion', 'True')
  else:
    rgb.setColorAll(0x000000)
    label9.setText('False')
    ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Motion', 'False')
  wait(0.1)
  if (angle_1.read()) <= 300:
    label4.setText('Left side')
    ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Position', 'Left Side')
  else:
    if (angle_1.read()) >= 600:
      label4.setText('Right side')
      ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Position', 'Straight')
    else:
      label4.setText('Straight')
      ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Position', 'Right Side')
  ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Temperature', (env3_3.temperature))
  ezdata.setData('Mz9RmdQ05baqlJATgTxjfXvSkkXJVBNo', 'Humidity', (env3_3.humidity))
  wait_ms(2)
