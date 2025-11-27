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

4. ROS topic publisher & subscriber – Communication between nodes.
   - create a turtle_velocity_publisher.cpp and turtle_pose_subscriber.cpp files in learn_topic/src/ folder
   - add executable and target link libraries in CmakeList.txt in learn_topic folder.
   - cd ros_ws/
   - catkin_make                                  #compiles the cpp files
   - source devel/setup.bash                      #required only once in the opened terminal.
   - roscore
   - rosrun turtlesim turtlesim_node              #it will start a simulator which subscribes to velocity msgs from publisher.cpp and publishes pose msgs
   - rosrun learn_topic turtle_pos_subscriber     #subscribes to cmd_pos msgs from turtlesim_node
   - rosrun learn_topic turtle_velocity_publisher #publiches velocity msgs on topic cmd_vel
   - rosrun turtlesim turtle_teleop_key           #this is an internal publisher can be used instead of above.

5. ROS service client & server – Request-response communication for robot commands.

6. ROS Action client & server – Communication between nodes with continous feedback.
   - catkin_create_pkg learn_action std_msgs rospy roscpp actionlib actionlib_msgs      #create learn_action function package inside ros_ws/src folder
   - catkin_make                                                                        #compile
   - cd ~/ros_ws/src/learn_action
     mkdir action
   - create a new file named DoDishes.action                                            #create action file that contains 3 sections: goal, result & feedback. Write the 
                                                                                         variables used.
   - Modify the CMakeLists.txt file                                                     # add action and services
   - create action_client.cpp and action_server.cpp files in learn_action/src/ folder
   - add executable and target link libraries in CmakeList.txt in learn_action folder.
   - cd ros_ws/
   - catkin_make                                                                        #compiles the cpp files
   - roscore
   - rosrun learn_action action_client                                                  #it will start client and wait for server to start
   - rosrun learn_action action_server                                                  #it will start server, and send feedback messages to client.
   

7. ROS parameter server – Configurations and sharing values across nodes.
   - roslaunch learn_launch turtle_node.launch # start a node
   - rosparam list                             # list all the parameters in the parameter server of the currently running ros node
   - rosparam get /turtle/background_b         # (rosparam get [parameter])This command is to get the parameter value of a certain parameter.
   - rosparam set /turtle/background_b 0       # (rosparam set [parameter] [value]) This command is to set the parameter value of a certain parameter. Do get afterwards.
   - rosparam get /turtle/background_b         # need to do after set. 
   - rosparam delete /turtle/background_b      # (rosparam delete [parameter]) This command is used to delete a certain parameter.
   - rosparam dump turtle.yaml                 # (rosparam dump [file])This command is used to export all parameter values of the current ros parameter service table and 
                                                 save them to a file.
   - rosparam load turtle_changed.yaml         # rosparam load [file] This command can load the parameter table file to the parameter server to realize the function of 
                                                 modifying multiple parameters at the same time.
   - roslaunch learn_launch turtle_param_set.launch # load the parameter file in launch file. Add following in launch file and create load a .yaml file with parameter 
                                                      values:

     <rosparam file="$(find learn_launch)/param/background_rgb.yaml" command="load"/>
   

ROS-launch file – Running multiple nodes together.


8. ROS-TF transformations – Understanding coordinate frames is critical for motion planning.
   -
   rosrun tf tf_monitor
   rosrun tf tf_monitor source_frame target_frame # print the release status of all coordinate systems in the TF tree,
   rosrun tf tf_echo source_frame target_frame # tf_echo: Used to view the transformation relationship between specified coordinate systems, terminal input.
   rosrun tf static_transform_publisher x y x yaw pitch roll frame_id child_frame_id period_in_ms # static_transform_publisher: Used to publish static coordinate transformations between two coordinate systems that do not undergo relative position changes
   rosrun rqt_tf_tree rqt_tf_tree  # This is a real-time tool that observes the coordinate system tree published on ros and uses the refresh button to update the contents of the tree
   
2. Visualization & Geometry

ROS-rviz – Essential for visualizing your robot, MoveIt planning scenes, and paths.

   




   
   






