🛠 Hardware Components

Two different bots were developed and tested during the project for experimentation with different sensor configurations.

🤖 Bot 1

Components used:

▸ Microcontroller: Arduino Nano
▸ Motors: BO Motors (12V, 200 RPM)
▸ Motor Driver: L298N
▸ IR Sensors: TCRT5000L
▸ Multiplexer: CD74HC4067 (16-channel MUX used for handling multiple sensors)
▸ Support Wheel: Castor Wheel 

🤖 Bot 2

Components used:

▸ Microcontroller: Arduino Uno
▸ Motors: BO Motors (12V, 200 RPM)
▸ Motor Driver: L298N
▸ IR Sensor Module: RLS-08 (8-channel line sensor array)
▸ Support Wheel: Castor Wheel

📚 Learnings

During the development and testing of the bots, the team observed some practical hardware limitations and important design lessons.

▸ Motor Driver Heating Issue
The L298N motor driver was observed to heat up significantly during operation. After running the bot for some time, this heating caused unstable behaviour in the motors, which resulted in random or inconsistent motion of the robot.
