<img src="https://github.com/ElectronicEngineerr/DC-Motor-Driving-with-Jetson-Nano-/blob/main/jeson%20nano.jpg" alt="JETSON NANO" width="400" height="300">


# DC-Motor-Driving-with-Jetson-Nano-
 As you know, Jetson nano has no pins to receive analog signals. So we will put a pca9685 pwm driver board between jetson nano and l298m motor driver. In this way, we will be able to adjust the speed of the motor as if it were an analog signal using Pwm signals.

DC-Motor-Driving-with-Jetson-Nano-
As you know, Jetson nano has no pins to receive analog signals. So we will put a pca9685 pwm driver board between jetson nano and l298m motor driver. In this way, we will be able to adjust the speed of the motor as if it were an analog signal using Pwm signals.

1) first thing i need is to have a python3 - pip3 :

`$ sudo apt-get install python3-pip`

2) Then you need to download the setuptools package on your jetson nano for a solid installation of the adafruit library

`$ pip3 install --upgrade setuptools`

3) We've come to the most important step. This is probably the command you used to install adafruit: ' pip3 install adafruit-pythoncircuit-servokit' but this version definitely does not work on some jetson nanos. you will already get a ton of bugs related to versions. If you don't get an error in this step, you can continue, but friends who get an error have to follow the step below. because this only works in the version I'm going to give. at least on my jetson...(actually what we are doing here is a version of adafruit that supports us to drive our dc motor from pca9685)
-----If you are getting this error, you will solve it simply by executing the following command:

(Collecting Adafruit-Blinka>=7.0.0 (from adafruit-circuitpython-servokit) Could not find a version that satisfies the requirement Adafruit-Blinka>=7.0.0 (from adafruit-circuitpython-servokit) (from versions: 0.1.3, 0.1.4, 0.1.5, 0.1.6, 0.1.7, 0.1.8, 0.1.9, 0.1.10, 0.2, 0.2.1, 0.2.2, 0.2.3, 0.2.4, 0.2.5, 0.2.6, 0.2.7, 0.3.0, 0.3.1, 0.3.2, 0.4.0, 1.0.2, 1.1.0, 1.1.1, 1.2.0, 1.2.1, 1.2.3, 1.2.4, 1.2.5, 1.2.6, 1.2.7, 1.2.8, 1.3.0, 1.3.1, 1.3.2, 1.3.3, 1.3.4, 2.0.0, 2.0.1, 2.0.2, 2.1.0, 2.1.1, 2.1.2, 2.1.3, 2.1.4, 2.1.5, 2.1.6, 2.2.0, 2.2.1, 2.3.0, 2.3.1, 2.3.2, 2.3.3, 2.4.0, 2.4.1, 2.5.0, 2.5.1, 2.5.2, 2.5.3, 2.6.0, 2.6.1, 3.0.0, 3.0.1, 3.0.2, 3.0.3, 3.0.4, 3.0.5, 3.0.6, 3.0.7, 3.0.8, 3.1.0, 3.1.1, 3.1.2, 3.2.0, 3.3.0, 3.3.1, 3.3.2, 3.3.3, 3.3.4, 3.3.5, 3.3.6, 3.3.7, 3.3.8, 3.3.9, 3.3.10, 3.4.0, 3.4.1, 3.5.0, 3.5.1, 3.5.2, 3.6.0, 3.6.1, 3.7.0, 3.7.1, 3.8.0, 3.9.0, 3.10.0, 4.0.0, 4.1.0, 4.2.0, 4.3.0, 4.4.0, 4.5.0, 4.6.0, 4.7.0, 4.8.0, 4.8.1, 4.9.0, 4.10.0, 4.10.1, 5.0.0, 5.0.1, 5.1.0, 5.2.0, 5.2.1, 5.2.2, 5.2.3, 5.2.4, 5.3.0, 5.3.1, 5.3.2, 5.3.3, 5.3.4, 5.4.0, 5.4.1, 5.5.0, 5.5.1, 5.5.2, 5.5.3, 5.5.4, 5.6.0, 5.7.0, 5.8.0, 5.8.1, 5.8.2, 5.9.0, 5.9.1, 5.9.2, 5.10.0, 5.11.0, 5.12.0, 5.13.0, 5.13.1, 6.0.0, 6.0.1, 6.0.2, 6.1.0, 6.2.0, 6.2.1, 6.2.2, 6.3.0, 6.3.1, 6.3.2, 6.4.0, 6.4.1, 6.4.2, 6.5.0, 6.6.0, 6.6.1, 6.6.2, 6.7.0, 6.8.0, 6.8.1, 6.8.2, 6.8.3, 6.9.0, 6.9.1, 6.9.2, 6.10.0, 6.10.1, 6.10.2, 6.10.3, 6.11.0, 6.11.1, 6.12.0, 6.13.0, 6.13.1, 6.14.0, 6.14.1, 6.15.0) No matching distribution found for Adafruit-Blinka>=7.0.0)

`$ sudo pip3 install adafruit-circuitpython-servokit==1.3.0`

4)It is not easy to drive a dc motor from a servo motor driver called Pca9685, but we can do this by hacking it in coding. In order to work better, first of all, let's download the following codes by adding them to the terminal

`$ pip3 insatll pylibi2c `

`$ pip3 insatll pynput `

<img src="https://github.com/ElectronicEngineerr/DC-Motor-Driving-with-Jetson-Nano-/blob/main/pca9685.jpg" alt="JETSON NANO" width="400" height="300">


5) We came to the part of hacking the code. First, let's make the i2c connection:
i2c = busio.I2C(board.SCL, board.SDA)
here we will set it to 0x40 instead of 0x60 and continue our process.


# pca = adafruit_pca9685.PCA9685(i2c, address=0x40)

what we have to do now is to give the most suitable frequency for the pwm signal of the motor. pca9685 servo motor driver supports 40-1000 Hz frequency. But I chose to use 1600. You can adjust the frequency with the most suitable rotation conditions of your engine by doing the necessary research.
pca.frequency = 1900
 
6)required link between jetson nano and pca9685:
-  Connect vcc pin on pca9685 to 3.3v pin on jetson nano
-  Connect gnd pin on pca9685 to gnd pin on jetson nano
-  Connect sda pin on pca9685 to gnd 3. pin on jetson nano
-  Connect scl pin on pca9685 to gnd 5.pin on jetson nano
- Also, do not forget to externally feed the pca9685 with 5 volts and gnd from 2 connectors.

7) The question that most people, especially me, wonder at the beginning: I will connect the ena, ın1 and ın2 pins from the l298m to my jetson. Or the pca9685 card :) I found the answer to this question as a result of trying many times and it was very tiring :)
here is the required connections (l298m to pca9685 servo motor driver)

-  ena pin taken from l298m motor driver - channel 8 on pca9685
-  ın1 pin taken from l298m motor driver - channel 9 on pca9685
-  ın2 pin taken from l298m motor driver - channel 10 on pca9685
 
( The pca9685 pins consist of 16 channels, but when we encode, we will get 15 channels. because we count from 0)

Here are the lines needed to encode these pins in python and convert the required analog signal to pwm signal:

pwm_channel = pca.channels[8]
channel1 = pca.channels[10]
channel2 = pca.channels[9]
Now we come to the most important step. As you know, Jetson nano does not have a pin where we can receive any analog signal. I was really shocked when I heard this. but we need to solve this . Required line to convert analog signal to pwm signal:

some friends will say this: 'was it that easy :)'

pwm_channel.duty_cycle = 0xffff
Now all you have to do is give the command to the engine and drive the engine.

motor1 = motor.DCMotor(channel1, channel2) motor1.throttle = 0.5

Fact, I prepared a code for you to control the motor with the keyboard, so you can move the dc motor back and forth with a keyboard library.

Of course, do not forget to add the libraries of these codes. You can find the necessary codes above.
