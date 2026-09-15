# Chapter 5 - Create3 Topics

In Activity 1 of Part 2 you got familiar with the robot hardware and ran a few examples using the Python Web Playground. Now, you are going to use ROS 2 to control the robot. This activity will focus on creating a node that activates the LEDs of the Light Ring of the Create3 robot depending on the readings from the front-facing proximity sensors. We are going to build a ROS node in Python to implement a behavior similar to what we saw in Chapter 4 when the light ring color changed according to the readings of the proximity sensors.

## 5.1 Activity: Preparing the Create3 for ROS

The robot and the computer running ROS 2 need to be on the same WiFi network because ROS 2 is designed for peer-to-peer communication using DDS (Data Distribution Service). DDS automatically discovers other ROS 2 nodes on the local network and then exchanges messages directly between devices. For example, your laptop runs a ROS 2 node that publishes to the topic `/cmd_vel` while the Create3 subscribes to `/cmd_vel`. DDS discovers both nodes automatically and messages flow directly between laptop and robot.

However, this will _not_ work if the robot and your laptop are connected to different networks. Also, many routers block multicast traffic between networks, prevent devices on different subnets from discovering each other, and/or use NAT, which hides devices from each other.

Therefore, **make sure that both the robot and your laptop running ROS will be connected to the same WiFi network**.

We will start this Activity by checking/updating the robot firmware. Then, we will inspect topics and write a Python node to control the robot light ring.

### Step 1 - Update the Create3 firmware

Your Create3 must have the correct firmware version to be able to work with ROS 2 Jazzy. To check which firmware version your robot is running, follow the instructions of **Phase 2: Update Robot** [on this page](https://iroboteducation.github.io/create3_docs/setup/provision/) until step 4. The firmware version will be displayed on the main page of the web server interface.

The firmware version of your robot must be **I.0.0.CycloneDDS**. If not, you must update it. For that, connect to the internet again and download the **release I.0.0.CycloneDDS** from [iRobot® Create® 3 Release I.0.0](https://iroboteducation.github.io/create3_docs/releases/i_0_0/).

> **Attention!** Make sure to download the correct version of the firmware!

Then, follow again the instructions of **Phase 2: Update Robot** [on this page](https://iroboteducation.github.io/create3_docs/setup/provision/) until step 6 to update the robot firmware. Enter the following in the application configuration screen:

* ROS 2 Domain ID: 0
* ROS 2 Namespace: `/robot_N` (replace _N_ by the number of your robot - do not forget the `/`!)
* RMW_IMPLEMENTATION: `rmw_cyclonedds_cpp`
* Enable Fast DDS discovery server? _Leave it unchecked_

> _Note:_ You can use any number for your robot, but make sure that each robot has a unique number. If you are at Hanze, you must use the robot number shown on the top faceplate of the robot and on its charging dock.

The above settings assume that you are using [ROS 2 Namespaces](https://github.com/iRobotEducation/create3_docs/blob/main/docs/setup/multi-robot.md#ros-2-namespaces) to have multiple Create3 robots connected to the same Wi-Fi network. For more information and to another option, check out [Using multiple Create® 3 robots](https://iroboteducation.github.io/create3_docs/setup/multi-robot/).

Proceed with the firmware update and restart the robot after it is completed.

### Step 2 - Test the communication with the robot

Once the robot reboots and connects to the WiFi, you can check connectivity by pinging the robot from your laptop:

```bash
ping <robot-ip>
```

If the ping fails, ROS 2 communication will almost certainly fail as well.

Once the robot and the laptop can communicate, follow the steps below to inspect topics from the robot and to build the ROS node.

## 5.2 Activity: Investigating the Create3 topics

First, we are going to investigate the topics of the Create3 robot, focusing on the proximity sensors and light ring. Then, we will create nodes to print IR values and to change the colors of the light ring.

### Step 1 - Inspect the robot topics

If not done yet, turn the robot ON by placing it on the charging dock with the front sensor facing the dock's sensor. You should see the robot's light ring turn on when you do this. Wait for around 2-3 minutes while the robot boots up and connects to the WiFi network. You should hear two "happy sounds" from your robot: one when it boots up and another one when it successfully connects to WiFi.

You can check if your robot is successfully connected to the same network as you by opening a new terminal window listing the current topics using `ros2 topic list`. You should now see an output similar to the one below, but prepended by the namespace of your robot (i.e: `/robot_1/battery_state`, for example):

```bash
  /battery_state
  /cmd_audio
  /cmd_lightring
  /cmd_vel
  /dock
  /hazard_detection
  /imu
  /interface_buttons
  /ir_intensity
  /ir_opcode
  /kidnap_status
  /mouse
  /odom
  /parameter_events
  /robot_state/transition_event
  /rosout
  /slip_status
  /static_transform/transition_event
  /stop_status
  /system_monitor/transition_event
  /tf
  /tf_static
  /wheel_status
  /wheel_ticks
  /wheel_vels
```

If there are multiple robots in the same network with the same ROS 2 Domain ID, every topic of all robots in the network will be listed.

### Step 2 - Inspect the proximity sensors topic

The Create3 publishes raw readings from its IR sensors on the topic `ir_intensity`. Let's start by seeing the data from this topic. As we learned before, we can echo the data from the topic by using the `ros2 topic echo <topic_name>` command.

Open a new terminal window and enter the following command, replacing `robot_1` by your robot's name:

```bash
ros2 topic echo /robot_1/ir_intensity
```

You should see a similar output to the one below:

```bash
- header:
  stamp:
    sec: 1662590667
    nanosec: 512966282
  frame_id: base_link
readings:
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_side_left
  value: 0
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_left
  value: 2
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_front_left
  value: 4
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_front_center_left
  value: 7
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_front_center_right
  value: 9
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_front_right
  value: 10
- header:
    stamp:
      sec: 1662590667
      nanosec: 512966282
    frame_id: ir_intensity_right
  value: 0
---
```

As you can see, the message published contains the readings for each of the 7 proximity sensors. Try moving your hand in front of the bumper and see how the readings behave, this will be needed when you write your code later on. The result should be comparable to the one in Activity 1.

### Step 3 - Publish to the Light Ring topic

The Create3 provides a topic where commands can be sent to control the robot's light ring. We are now going to send a test command to explore the message's format. Try sending following command in your terminal:

```bash
ros2 topic pub /robot_1/cmd_lightring irobot_create_msgs/msg/LightringLeds "{override_system: true, leds: [{red: 255, green: 0, blue: 0}, {red: 0, green: 255, blue: 0}, {red: 0, green: 0, blue: 255}, {red: 255, green: 255, blue: 0}, {red: 255, green: 0, blue: 255}, {red: 0, green: 255, blue: 255}]}"
```

This should make your robot's light ring colorful.

As you can see, the message published on this topic is of type `irobot_create_msgs/msg/LightringLeds` and is relatively intuitive to use. Try playing around with the values and see them change yourselves.

To return the lightring to the default color, just send an empty message on the topic like so:

```bash
ros2 topic pub /robot_1/cmd_lightring irobot_create_msgs/msg/LightringLeds "{}"
```

### Step 4 - Understand the message structure

Before we can write code to use these topics, we must understand the structure of each message. This is something you will have to do whenever you interact with a new topic or action, so it is important to understand this process well.

Using the `ros2 interface show <interface-name>` command we can investigate the structure of messages. Let's look at the message for the `ir_intensity` topic: first, we must find out its message type by getting information about the topic:

```bash
ros2 topic info /robot_1/ir_intensity
```

You should see a message similar to this:

```bash
Type: irobot_create_msgs/msg/IrIntensityVector
  Publisher count: 1
  Subscription count: 0
```

As you can see, the message type is `irobot_create_msgs/msg/IrIntensityVector`. Using this information, we can check the message structure (called _interface_ in ROS):

```bash
ros2 interface show irobot_create_msgs/msg/IrIntensityVector
```

As a result, you should be able to see the message structure. Note its fields and their hierarchy:

```bash
std_msgs/Header header
    builtin_interfaces/Time stamp
        int32 sec
        uint32 nanosec
    string frame_id
irobot_create_msgs/IrIntensity[] readings
    std_msgs/Header header
        builtin_interfaces/Time stamp
            int32 sec
            uint32 nanosec
        string frame_id
    int16 value
```

By inspecting the above result, you can see that the message consists of two top-level fields: `header` (of type `std_msgs/Header`) and `readings` (of type `irobot_create_msgs/IrIntensity[]`). Note a few things here:

- The hierarchy of the fields is described by their indentation (e.g: The `value` field is a part of the `readings` field )
- The `readings` field is an array, evidenced by the `[]`

## 5.3 Activity: Programming the Create3 with ROS 2

Now, let's see how we can do the same in Python.

### Step 1 - Create a node to print IR values

Copy the simple subscriber code below in a new file and run it, making sure to change the topic name according to your namespace (if you need a reminder on how to do this, review steps 4-8 of Activity 1.2.1 from [Part 1, Chapter 1](../../Part_1-ROS/Chapter-1/readme.md#step-4---create-the-python-scripts)).

```python
#!/usr/bin/env python3

from irobot_create_msgs.msg import IrIntensityVector
import rclpy
from rclpy.node import Node
from std_msgs.msg import String
from irobot_create_msgs.msg import IrIntensityVector
from rclpy.qos import ReliabilityPolicy, QoSProfile

class ir_subscriber(Node):

    def __init__(self):
        super().__init__("ir_subscriber")
        
        # Subscribe to the ir_intensity topic, which has a message with type IrIntensityVector
        self.irSubscriber = self.create_subscription(IrIntensityVector,"/robot_1/ir_intensity",self.ir_callback,QoSProfile(depth=10, reliability=ReliabilityPolicy.BEST_EFFORT))
        # Remember to change /robot_1 by to the namespace of your robot!

    def ir_callback(self,msg):
        print('Message type is:',type(msg))
        print('\n Header data is:', msg.header)
        print('\n The readings data is:',msg.readings)
   
def main():
    rclpy.init()
    subcriberNode = ir_subscriber()
    rclpy.spin_once(subcriberNode)

if __name__ == '__main__':
    main()
```

You should see it output a message similar to this:

```bash
Message type is: <class 'irobot_create_msgs.msg._ir_intensity_vector.IrIntensityVector'>

Header data is: std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='base_link')

The readings data is: [irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_side_left'), value=15), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_left'), value=415), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_front_left'), value=502), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_front_center_left'), value=32), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_front_center_right'), value=26), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_front_right'), value=366), irobot_create_msgs.msg.IrIntensity(header=std_msgs.msg.Header(stamp=builtin_interfaces.msg.Time(sec=1664227973, nanosec=512763090), frame_id='ir_intensity_right'), value=2897)]
```

As you can see, this reflects what we saw in the terminal earlier. In our case, `type(msg)` returns the message type, which is of the same type we saw in the terminal before. We can also access the `header` and `readings` variables simply by `msg.header`and `msg.readings`, much like the way we can access a normal Python dictionary.

> **Exercise:** Using that same logic, add instructions to the code to access and print a list with just the values of the elements of the `readings` array.

### Step 2 - Create nodes to control the light ring

Now, let's do the same for the `cmd_lightring` topic. Use the template below to create a node that turns the light ring completely blue.

```python
#!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from std_msgs.msg import String
from irobot_create_msgs.msg import IrIntensityVector, LightringLeds, LedColor
from rclpy.qos import ReliabilityPolicy, QoSProfile

class lightController(Node):

    def __init__(self):
        super().__init__("lightController")
        
        #Publish to the cmd_lightring topic, which uses messages with type LightringLeds
        self.lightringPublisher  = self.create_publisher(LightringLeds,"cmd_lightring",10)
        
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        

    def timer_callback(self):
        #Initilaize message to correct message type
        msg = LightringLeds()
        msg.override_system = True #To override the default lightring settings
        
        #!Write your own code here!
        # Set all 6 LEDs to blue

        self.lightringPublisher.publish(msg)

        print("Publishing...")
      

def main():
    rclpy.init()
    controller = lightController()
    rclpy.spin(controller)

if __name__ == '__main__':
    main()
```

Using the same concepts we used in the talker-listener nodes we created in Part 1, Chapter 1, and what we learned about the message type, we can create a simple node that subscribes to `ir_intensity` topic and publishes to the `cmd_lightring`. 

Use the template below to create a similar behavior to the one we observed in Activity 1 when we ran `ir_proximity_obstacles.py` (changing the color of the light ring depending on the values of the IR sensors). You can do whatever you want, like change it to red if the robot is close to any obstacle and green otherwise, or maybe assign a color to each sensor's readings.

```python
from rclpy.node import Node
from std_msgs.msg import String
from irobot_create_msgs.msg import IrIntensityVector, LightringLeds, LedColor
from rclpy.qos import ReliabilityPolicy, QoSProfile

class lightController(Node):

    def __init__(self):
        super().__init__("lightController")
        
        # Subscribe to the ir_intensity topic, which has a message with type IrIntensityVector
        self.irSubscriber = self.create_subscription(IrIntensityVector,"ir_intensity",self.ir_callback,QoSProfile(depth=10, reliability=ReliabilityPolicy.BEST_EFFORT))
        # Publish to the cmd_lightring topic, which uses messages with type LightringLeds
        self.lightringPublisher  = self.create_publisher(LightringLeds,"cmd_lightring",10)
        
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        
        # Define the ir_readings variable to store readings
        self.ir_readings = []


    def timer_callback(self):
        # Initilaize message to correct message type
        msg = LightringLeds()
        msg.override_system = True # To override the default lightring settings

        # Defining some LED colors to use later using the LedColor message type
        blueLed = LedColor(red=0, green=0, blue=255)
        redLed = LedColor(red=255, green=0, blue=0)
        greenLed = LedColor(red=0, green=255, blue=0)
        offLed = LedColor()

        # Main Logic
        if self.ir_readings: # Check if a valid reading exists
            #! Write your code here!
            # You can delete the example below and replace it with your logic.
            
            #! Example
            # If the left proximity sensor detects a close object    
            if self.ir_readings[0].value > 100: 
                # Make all 6 LEDs blue
                msg.leds = [blueLed, blueLed, blueLed, blueLed, blueLed, blueLed]
            
            
        self.lightringPublisher.publish(msg)

        print("Publishing...")
    
    def ir_callback(self,msg):
        self.ir_readings = msg.readings


def main():
    rclpy.init()
    controller = lightController()
    rclpy.spin(controller)

if __name__ == '__main__':
    main()
```

> **_Note_**: Remember that you will need to change the topic names to reflect your robot's name (e.g: `ir_intensity` => `robot_1/ir_intensity`).

## Conclusion

After completing this Chapter, you should be familiar with the Create3 topics and should know how to create nodes to interact with the IR sensors and Light Ring of the Create3 robot. In the next Chapter, we will study how to send ROS Actions goals to control the robot.

## Navigation menu

- Go to the list of common ROS 2 commands [ROS 2 CLI Cheat Sheet](../../Part_1-ROS/Chapter-1/ros2_commands.md)
- Go back to [Chapter 4](../../Part_2-Create3/Chapter-4/readme.md)
- Continue to [Chapter 6](../../Part_2-Create3/Chapter-6/readme.md)
- Go to the [Main page](../../readme.md)