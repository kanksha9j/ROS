DOFBOT_SE

Basic Control Course

Give delay between commands using time.sleep(.5)

1. RGB Control
   Control RGB brightness of RGB light by RGB values ranging from 0-255. Larger the value, brighter the light.
   Api: Arm_RGB_set(R, G, B)

   | R   | G  | B   | Observed Color / Brightness | Notes        |
   | --- | -- | --- | --------------------------- | ------------ |
   | 50  | 0  | 0   | Dim red                     | Base test    |
   | 100 | 0  | 0   | Brighter red                | Increasing R |
   | 50  | 50 | 0   | Yellow                      | R+G mixing   |
   | 0   | 0  | 255 | Blue                        | Only B       |

2. Control Buzzer
   Api: Arm_Buzzer_On(delay=255)

   Input range for delay is 1 - 50 where 1 = 100 ms and 50 = 5s, if no value given or value is not in range then buzzer keeps sound on and must be turned off useing
   Arm_Buzzer_Off().
   Higher delay -> longer sound.

   Integrate both LED + buzzer:
   | Step | Action                   | Duration                  |
   | ---- | ------------------------ | ------------------------- |
   | 1    | Turn LED red + buzzer on | 1 s (delay=10)            |
   | 2    | LED off + buzzer off     | 0.5 s (`time.sleep(0.5)`) |
   | 3    | Repeat 3 times           | —                         |

3. Control single servo:
   Api: Arm_serial_servo_write(id, angle, time)

   | Parameter | Description       | Range / Notes                                            |  
   | --------- | ----------------- | -------------------------------------------------------- |
   |  id       | Servo number      | 1–6 (1 = base, 6 = top/gripper)                          |
   |  angle    | Target angle      | 0–180° for servos 1–4 & 6; 0–270° for servo 5            |
   |  time     | Movement duration | Time in ms or unit defined by library; 0 = fastest speed |

   Precaution: To avoid stalling the gripper (servo 6), use the table given to set proper angle to clamp objects according to its length. For cube range is from 0 to 134. Do 
   not set larger than 134. 0= fully open.

   | Servo ID           | Range                |  
   | ------------------ | -------------------- |
   | 1 (base)           | 0 – 180°             |
   | 2 (shoulder)       | 0 – 180°             |
   | 3 (elbow)          | 0 – 180°             |
   | 4 (wrist rotation) | 0 – 180°             |
   | 5 (wrist bend)     | 0 – 270°             |
   | 6 (gripper)        | 0 – 180°             |


 4. Read servo current position:
    Api: Arm_serial_servo_read(id)
    
    Reads the current angle value of servo.
    Test movement over time : Move servo with a non-zero time parameter (slower motion). Continuously read servo position in a loop to track progress.
    Check clamp position when it clamps the cube.
    Useful for debugging.

 6. Control all servos simultaneously:
    Api: Arm_serial_servo_write6(S1, S2, S3, S4, S5, S6, time)
    control angles of all servos.
    
    S1: Angle value of No. 1 servo 0~180 and so on.
    time is the running time of servos.  
   
    Notice servo2 is reversed (180 - angle) → probably because of how servo2 is mounted physically. Moving “forward” for servo2 requires subtracting from 180°.
    angle=90 is the centre position.

    ROS Basic Course
    ROS is an open source operating system for Robots.
    In ROS, nodes can publish as well as subscribe. And once a message is available, then messages flow using peer to peer communication.

    ROS Master:
    Keeps track of all active nodes, topics, and services. Helps nodes discover each other so publishers and subscribers can connect.
    After discovery, messages flow peer-to-peer.

    Parameter Server:
    A central storage for parameters, accessible by all nodes. Stores key-value pairs (strings, numbers, booleans, arrays).
    Commonly used for configuration data that multiple nodes might need.

    The topic is one way channel. It sends data to subscriber but doesnt send anything back to publisher. 

    3 Communication mechanisms:
    1. ROS Topics are inherently asynchronous. The publisher does not wait for subscribers to process the message.
    2. Use ROS Services for synchronous request-response: A client sends a request. It waits (blocks) until the server sends a response.
    3. For long-running tasks, use ROS Actions, which are asynchronous but allow: Feedback messages during execution or after finishing. Can optionally wait (synchronously) 
    until the action completes
    




