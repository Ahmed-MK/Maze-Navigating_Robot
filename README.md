# Maze-Navigating Robot

A robot that communicates with a computer via bluetooth, and is given commands to move based on the shortest path in the maze it is supposed to navigate through. 

The C++ algorithm works using OpenCV and BFS + Shortest Path algorithms to solve the maze in the most optimal path. Then it translates the solution to viable commands to be given to the robot, for it to navigate the maze. And then transmits them using bluetooth to the robot. 

## Hardware
The robot uses the following parts:

- Arduino UNO R3 board
- 2 × Parallax continuous servo motors
- Arduino Prototyping Shield
- HC-05 Bluetooth module
- A 8000mAh Power bank
- A 9V battery

### How the hardware works together
The Arduino board is the microcontroller used by the robot, it recieves signals from the bluetooth module, and sends signals to the servo motors based on the input that it received. The Arduino board is powered using a 9V battery, and the rest of the components are powered using a power bank, but any 5V power supply could be used in this case.

### Connection Diagram 
![alt text](https://github.com/Satharus/Maze-Navigating_Robot/blob/master/Circuit%20Diagram.png)

## Software
This software uses GNU C++11, OpenCV, and the Arduino scripting language(C/C++). It works by giving the C++ program a maze to solve, it solves it to output co-ordinates using that are relevant to the pixels. It then scales the co-ordinates according the the actual size of the maze considering the resolution of the image, and the speed of the robot to give commands in the following format [DIRECTION] [TIME(ms)]. The speed in the program is set to the speed of our car, but it can be changed easily due to the code being open source.

The direction could be:
- F for Forward
- B for Backwards
- R for Right (Rotation)
- L for Left (Rotation)

and the time is in milliseconds.

So, giving it the following commands would make it do the following actions, relative to the direction that it is facing:
- F 1000 ---> Move forward for 1 second
- B 1500 ---> Move backwards for 1.5 seconds
- R 670  ---> Rotate clockwise for 0.67 seconds
- L 930  ---> Rotate counter clockwise for 0.93 seconds

## How to use
The software requires:
- OpenCV
- GNU C++11 or newer
- Bluetooth connection software for Linux(preferably ```blueman```)
- PC with bluetooth capabilities running a Linux distribution(we only tested Debian based distributions, but if you have tried any other distributions let us know!).

Currently, the software is available on Linux only. If someone can port it to work on windows please let us know! We'd be more than happy to see it working :smiley:!

First you need to make sure you have the software dependancies. If you don't, you need to download them. 
You can see how to download OpenCV in details here: http://www.codebind.com/cpp-tutorial/install-opencv-ubuntu-cpp/

To install the other packages, run the following command in a terminal. 
```
sudo apt install g++ blueman
```

Now you should have all the software you need to start working. To get started, switch on your robot, connect to it using blueman. You should see that is shows a message like ```Serial port connected to /dev/rfcommX``` This means that your system has successfuly connected to the device and that it is present at ```/dev/rfcommX```, where X is an integer that denotes the channel you're connected on. Remember that number as you'll need it to communicate with the device. 

Now you can test the device to make sure that it is working. Open up a terminal and write the following
```
sudo su
echo "F 1000" >> /dev/rfcommX
```
Changing X for the value that you got when connecting the robot using blueman.

After running the first command and entering your password you will be logged in as root, then the second command sends "F 1000" to the robot, this will make the robot move forward for 1 second.

Now that you are sure everything works, you can download the source code for the C++ program from this GitHub repository, and then compile it and run it by running the following commands in a terminal that has the project folder as its working directory.
```
sudo su
./compile
./Solver
```
Now the C++ program will run! 

You can start by typing in the path to the file/name of the file that contains the image that is supposed to be solved, and then entering the channel number, and then it will prompt you if you wish to send the command that was automatically generated by the program based on the picture of the maze, if you decide not to, you have the option of sending your own command.

When the command is transmitted, the program sleeps for the time that the command is supposed to be executed in plus one second.

After finishing the series of commands and your own commands (if they exist), the program shows the user an image that has the maze solution.
