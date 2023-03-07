# ROS2-SunriseV3-Robot

### 1.Add publisher & subsrciber
- pub_a
- pub_b
- MecarrunV3 test

------
### 2.Publisher cmd_vel node and Subscriber to driver
*使用MecarrunV3時，要注意，當py和主程式放一起時,因為裡面有用到序
*列埠，在.bashrc裡要加入權限sudo chmod a+rw /dev/ttyACM0
- $ ros2 run py_pkg cmd_vel_pwm (sunrise V3)
- $ ros2 run teleop_twist_keyboard teleop_twist_keyboard (PC)

------
### 3.Use the camera to stream images, and add the chassis to control the movement
- (sunrise x3)
    - Execute on terminal for first
        - $ ssh sunrise@xxx.xxx.xxx
        - 配置 TogetherROS 环境：
            - $ source /opt/tros/local_setup.bash
        - 参数设置：
            - $ ros2 launch hobot_usb_cam hobot_usb_cam.launch.py
    - Execute on terminal for second
        - View USB camera images on the web
            - 配置 TogetherROS 环境：
                - $ source /opt/tros/local_setup.bash
            - 启动nginx，nginx只需启动一次，如前面已启动过nginx，则无需再次启动
                - $ cd /opt/tros/lib/websocket/webservice && chmod +x ./sbin/nginx &&./sbin/nginx -p .
            - 启动websocket
                - $ ros2 run websocket websocket --ros-args -p image_topic:=/image -p image_type:=mjpeg -p only_show_image:=true
            - After execution, open the browser and enter the sunrise IP in the address bar(執行後開啓瀏覽器並在網址列輸入sunrise V3 IP)
- VMware machine(虛擬機)
    - do the second
        - 2.Publisher cmd_vel node and Subscriber to driver
------
### Hend_Detection手部偵測
- (sunrise x3)
    - $ ssh sunrise@xxx.xxx.xxx
    - $ sudo su
    - $ source /opt/tros/setup.bash
    - $ export CAM_TYPE=usb
    - $ ros2 launch mono2d_body_detection hobot_mono2d_body_detection.launch.py
    - get some error about /etc/video8....,change launch file driver
    - 如遇到有關找不到攝像頭的報錯，請至launch文件修改

------
### Hend_Detection Follow Robot
- (PC)
    - install turtlebot3_pkg and gazebo
        - $ sudo apt-get install ros-foxy-gazebo-*
        - $ sudo apt install ros-foxy-turtlebot3
        - $ sudo apt install ros-foxy-turtlebot3-simulations

    - $ ros2 launch turtlebot3_gazebo empty_world.launch.py
    - $ source /opt/ros/foxy/setup.bash
    - $ export TURTLEBOT3_MODEL=burger
    - $ ros2 launch turtlebot3_gazebo empty_world.launch.py

- (Sunrise x3)
    ##### 使用USB摄像头发布图片

    ###### 配置TogetherROS环境
    - $ source /opt/tros/setup.bash

    ###### 从TogetherROS的安装路径中拷贝出运行示例需要的配置文件。
    - $ cp -r /opt/tros/lib/mono2d_body_detection/config/ .
    - $ cp -r /opt/tros/lib/hand_lmk_detection/config/ .
    - $ cp -r /opt/tros/lib/hand_gesture_detection/config/ .

    ###### 配置USB摄像头
    - $ export CAM_TYPE=usb

    ###### 启动launch文件
    - $ ros2 launch gesture_control hobot_gesture_control.launch.py

    - $ cd ros2_ws
    - $ sudo su
    - $ ros2 run py_pkg cmd_vel_pwm

- (PC) 開啓gazebo的仿真
    - $ ros2 launch turtlebot3_gazebo empty_world.launch.py

------
### Robot Follow me (小車跟隨)
- (sunrise x3)
    - $ sudo su
    ###### 配置TogetherROS环境
    - $ source /opt/tros/setup.bash

    ###### 从TogetherROS的安装路径中拷贝出运行示例需要的配置文件。
    - $ cp -r /opt/tros/lib/mono2d_body_detection/config/ .

    ###### 配置USB摄像头
    - $ export CAM_TYPE=usb

    ###### 启动launch文件
    - $ ros2 launch body_tracking hobot_body_tracking_without_gesture.launch.py

- (PC) 開啓gazebo的仿真
    - $ ros2 launch turtlebot3_gazebo empty_world.launch.py

------
