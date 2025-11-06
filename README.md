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

  | Servo ID           
  | ------------------ | -------------------- |
  | 1 (base)           | 0 – 180°             |
  | 2 (shoulder)       | 0 – 180°             |
  | 3 (elbow)          | 0 – 180°             |
  | 4 (wrist rotation) | 0 – 180°             |
  | 5 (wrist bend)     | 0 – 270°             |
  | 6 (gripper)        | 0 – 180°             |
 
4. 

   




