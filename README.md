# raspberry_servo_controller

![output3](https://github.com/user-attachments/assets/526a7009-f78e-481e-95a0-e9f9e78c87c6)


## Control a servo connected to a Raspberry Pi using an Xbox controller. Easy setup for testing.

Parts list:
- raspberry pi 4
- xbox controller
- power bank
- breadboard
- 100 µF capacitor
- servo
- jumper wires

### Instructions
1. Prep Raspberry Pi (I used Raspberry OS Lite but I imagine most OS's should work)
3. Install dependancies
   ```
   sudo apt-get install -y evtest evdev
   ```
   
4. Pair controller
   ```
   bluetoothctl
   scan on
   pair XX:XX:XX:XX:XX:XX
   trust XX:XX:XX:XX:XX:XX
   connect XX:XX:XX:XX:XX:XX
   ```
   Make note of the ID; eventX
   
6. Look at controller output
   ```
   sudo evtest /dev/input/eventX
   ```

7. Create python script
   ```
   vim ServoController.py
   ```
   copy paste (be sure to change 'eventX' to your controller id
   ```
   import evdev
   import RPi.GPIO as GPIO
   import time
    
   GPIO.setmode(GPIO.BOARD)
   GPIO.setup(11,GPIO.OUT)
   servo = GPIO.PWM(11,50)
   servo.start(0)
    
   controllerInput = evdev.InputDevice("/dev/input/event0")
    
    for event in controllerInput.read_loop():
      if event.type == evdev.ecodes.EV_ABS:
        if event.code == evdev.ecodes.ABS_X:
          angle = 2 + (event.value / 6553.5)
          servo.ChangeDutyCycle(angle)
          print(angle)
   ```

8. Run script
   ```
   python3 ServoController.py
   ```
   
Thanks to [Berardinux](https://github.com/Berardinux) for the code and guide! [XboxController_Servo_RaspberryPi](https://github.com/Berardinux/XboxController_Servo_RaspberryPi)

