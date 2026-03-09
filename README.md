
---

# Nuwa-HP60C ROS 2 Wrapper (ascamera)

This repository provides a ROS 2 wrapper for the **Nuwa-HP60C** depth camera, enabling seamless integration into robotics applications. 

## 🛠 Compatibility

The environment setup and camera usage are tested on the following configurations: 

* **Ubuntu 20.04**: ROS 2 Foxy 


* **Ubuntu 22.04**: ROS 2 Humble 



---

## 1. Install Dependencies

Open a terminal and install the required system and ROS 2 dependencies. 

**For ROS 2 Foxy (Ubuntu 20.04):**

```bash
sudo apt update
sudo apt install libgflags-dev nlohmann-json3-dev libgoogle-glog-dev \
ros-foxy-image-transport ros-foxy-image-publisher

```

> 
> **Note:** For other versions like **Humble**, replace `foxy` with `humble` in the package names. 
> 
> 

---

## 2. Workspace Setup & Compilation

Instead of standard cloning, follow these steps to set up the specialized workspace provided for this hardware: 

1. 
**Copy Workspace**: Unzip the provided files and move the `ascam_ros2_ws` directory to your home (root) directory. 


2. **Navigate and Build**:
```bash
[cite_start]cd ~/ascam_ros2_ws 
[cite_start]chmod a+x build.sh 
[cite_start]./build.sh 

```


3. **Troubleshooting**:
* If architecture errors occur, modify the `build.sh` to match your specific platform. 


* If `orbslam` errors occur, ensure related packages are installed or removed before compiling. 





---

## 3. Environment Configuration

### Add Environment Variables

Add the workspace setup script to your `.bashrc` to source it automatically: 

```bash
[cite_start]echo "source ~/ascam_ros2_ws/install/setup.bash" >> ~/.bashrc [cite: 68]
source ~/.bashrc

```

### Install Udev Rules

Install the rules to ensure the camera is recognized via USB without permission issues: 

```bash
[cite_start]cd ~/ascam_ros2_ws/src/ascamera/scripts 
[cite_start]sudo bash create_udev_rules.sh 

```

*Success is indicated by the message: "reload udev rules done".* 

---

## 4. Launch & Parameter Configuration

You can modify the camera's operation by editing the launch file: 

```bash
gedit ~/ascam_ros2_ws/src/ascamera/launch/hp60c.launch.py

```

### Key Parameters in `hp60c.launch.py`:

Ensure these parameters are correctly mapped within the `ascamera_node`: 


**confPath**: Path to your configuration files (e.g., `/home/username/ascam_ros2_ws/src/ascamera/configurationfiles`). 


  
**color_pcl**: Set to `True` to enable colored point clouds. 


  
**depth_width / depth_height**: 648 x 488. 


  
**rgb_width / rgb_height**: 640 x 480. 


  
**fps**: Set to 25. 



> 
> **Note:** Always re-run `./build.sh` after modifying configuration paths or parameters. 
> 
> 

---

## 5. Running the Camera

To start the camera node, open a new terminal and run: 

```bash
[cite_start]ros2 launch ascamera hp60c.launch.py 

```

---

## 6. Data Visualization

### Active Topics

Verify the active streams using `ros2 topic list`: 


`/ascamera_hp60c/camera_publisher/depth0/camera_info` 


  
`/ascamera_hp60c/camera_publisher/depth0/image_raw` 


  
`/ascamera_hp60c/camera_publisher/depth0/points` 

  
`/ascamera_hp60c/camera_publisher/rgb0/camera_info` 


  
`/ascamera_hp60c/camera_publisher/rgb0/image` 


  
`/parameter_events` 


`/rosout` 


`/tf`

### 2D Visualization (RQT)

View RGB and Depth feeds via `rqt_image_view`: 


**RGB**: Select `/ascamera_hp60c/camera_publisher/rgb0/image`. 


  
**Depth**: Select `/ascamera_hp60c/camera_publisher/depth0/image_raw`. 



### 3D Visualization (RViz2)

1. Run `rviz2`. 


2. Set **Fixed Frame** to `ascamera_hp60c_cam...` 


3. Click **Add** -> **By topic**. 


4. Select **PointClouds** under the `/ascamera_hp60c/camera_publisher/depth0/points` topic. 



---
