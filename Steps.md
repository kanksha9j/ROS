1. download yahboom VM file :
https://www.yahboom.net/study/DOFBOT_SE

2.download VM file and extract it.
open oracle virtual box and new : write name , ubuntu, 64-bit -> next
select use existing virtual hard disk.

3. Connect camera and USB cable from board to computer.
ls /dev/ttyUSB* /dev/ttyACM*            #to search for devicee names
use following lline in VMWare of yahboom image. in terminal to create temporary symbolic link for the session to map /dev/ttyUSB0 to /dev/myserial.
sudo ln -s /dev/ttyUSB0 /dev/myserial

run command:
python3 rosmaster/YahboomArm.pyc

a beep should occur.

4. Network issue using oracle vbox, so installed new VMWare workstation Pro. check network adapter setting of VM , 
- VM settings should be bridged network and connected to physical device setting on.
 
5. Use yahboom image downloaded from internet, run python3 rosmaster/YahboomArm.pyc. 

6. install jupyterlab:
- sudo passwd  # set password for root
- sudo su      # switch to root

*****************************************************************************************************************************************************

ROS commands:

1. Create a workspace:
-  mkdir ros_ws
-  cd ~/ros_ws
-  mkdir src
-  cd ~/ros_ws/src                                         ##store function packages
-  catkin_init_workspace                                   ##initialize workspace
-  cd ~/ros_ws
-  catkin_make                                             #compile workspace
-  echo "source ~/ros_ws/devel/setup.bash" >> ~/.bashrc    #add workspace to environment variables
-  source ~/.bashrc                                        #refresh the environment variables

2. Create a function package:
- cd ~/ros_ws/src
- catkin_create_pkg learn_topic std_msgs rospy roscpp geometry_msgs turtlesim  #name of function package is learn_topic, rest are dependent libraries of the function package

3. Start ros service 
- roscore                                       #starts roscore service. only one roscore service can be run. starts rosmaster, rosout node and parameter server.
- rosnode list                                  #lists all rosnode started. /rosout node is started. rosout node logs all the log messages received from all the  nodes.
- rosnode info /rosout                          #gives info about rosout
- rosrun turtlesim turtlesim_node               #rosrun pkg_name executable_program. Most other ros node programs are started by rosrun.
- rosnode list                                  #check turtlesim node started
- rosnode info /turtlesim                       #gives info about node turtlesim

4. ROS node – Fundamental unit of ROS programs.
 Create a ROS node with command: roscore in ros_ws workspace.

ROS topic publisher & subscriber – Communication between nodes.

ROS service client & server – Request-response communication for robot commands.

ROS parameter server – Configurations and sharing values across nodes.

ROS-launch file – Running multiple nodes together.

2. Visualization & Geometry

ROS-rviz – Essential for visualizing your robot, MoveIt planning scenes, and paths.

ROS-TF transformations – Understanding coordinate frames is critical for motion planning.
   




   
   






