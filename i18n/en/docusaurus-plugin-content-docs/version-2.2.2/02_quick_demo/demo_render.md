---
sidebar_position: 2
---

# 5.2.2 Data Display

```mdx-code-block
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
```

## Web Display

### Feature Overview

Web Display is used to preview camera images (in JPEG format) and algorithm results. It transmits images and algorithm outputs over the network to a PC browser for rendering and display. This display client also supports showing video only, without rendering intelligent analysis results.

Code repository: [https://github.com/D-Robotics/hobot_websocket](https://github.com/D-Robotics/hobot_websocket)

### Supported Platforms

| Platform | Operating System | Example Functionality |
| ------- | ------------- | ------------------------------ |
| RDK X3, RDK X3 Module | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble)  | Launch MIPI camera and display images via Web |
| RDK X5, RDK X5 Module, RDK S100 | Ubuntu 22.04 (Humble)  | Launch MIPI camera and display images via Web |
| RDK Ultra | Ubuntu 20.04 (Foxy) | Launch MIPI camera and display images via Web |
| X86     | Ubuntu 20.04 (Foxy) | Launch USB camera and display images via Web |

### Prerequisites

#### RDK Platforms

1. Confirm that the F37 camera is properly connected to the RDK.
2. Ensure the PC can access the RDK over the network.
3. Verify that TogetherROS.Bot has been successfully installed.

#### X86 Platform

1. Confirm that the X86 system runs Ubuntu 20.04 and that tros.b has been successfully installed.
2. Ensure the USB camera is plugged into a USB port on the host machine and is correctly recognized.

### Usage Instructions

#### RDK Platforms

1. Log in to the RDK via SSH and launch the required onboard programs:

    a. Launch `mipi_cam`

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
   # Configure tros.b environment
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
   # Configure tros.b environment
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

    ```shell
    ros2 launch mipi_cam mipi_cam.launch.py mipi_video_device:=F37
    ```

    b. Launch encoder

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
   # Configure tros.b environment
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
   # Configure tros.b environment
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

    ```shell
    ros2 launch hobot_codec hobot_codec_encode.launch.py
    ```

    c. Launch websocket

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
   # Configure tros.b environment
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
   # Configure tros.b environment
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

    ```shell
    ros2 launch websocket websocket.launch.py websocket_image_topic:=/image_jpeg websocket_only_show_image:=true
    ```

2. Open a PC browser (Chrome/Firefox/Edge) and navigate to `http://IP:8000` to view the image stream, where IP is the RDK's IP address.

   ![websocket](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/websocket.png)

#### X86 Platform

1. Launch the `hobot_usb_cam` node

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

    ```shell
<<<<<<< HEAD
    # usb_video_device需要更改为实际usb摄像头video节点
    ros2 launch hobot_usb_cam hobot_usb_cam.launch.py usb_image_width:=1280 usb_image_height:=720 usb_video_device:=/dev/video0
    ```

2. 启动websocket节点
=======
    # Replace usb_video_device with the actual video device node of your USB camera
    ros2 launch hobot_usb_cam hobot_usb_cam.launch.py usb_image_width:=1280 usb_image_height:=720 usb_video_device:=/dev/video0
    ```

2. Launch the websocket node
>>>>>>> 3fc6ea4 (Init commit)

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

    ```shell
    ros2 launch websocket websocket.launch.py websocket_image_topic:=/image websocket_only_show_image:=true
    ```

<<<<<<< HEAD
3. PC浏览器（chrome/firefox/edge）输入 `http://IP:8000` ，即可查看图像效果，IP为PC IP地址，若在本机访问，也可使用localhost。

### 注意事项

1. websocket需要使用8000端口，如果端口被占用，则会启动失败，解决方法如下：

   - 使用`lsof -i:8000`命令查看8000端口占用进程，使用`kill <PID>`关闭占用8000端口进程，然后重新启动websocket即可。

   - 若用户不想停止当前正在占用8000端口的服务，可以修改 `/opt/tros/${TROS_DISTRO}/lib/websocket/webservice/conf/nginx.conf` 配置文件中的`listen`端口号，改为大于1024且未使用的端口号。修改端口号后，浏览器端使用的URL也要同步修改。

## HDMI展示

### 功能介绍

本章节介绍通过HDMI展示camera nv12图像的使用，RDK通过HDMI接显示器即可显示实时图像效果，对应于hobot_hdmi package。

代码仓库：[https://github.com/D-Robotics/hobot_hdmi](https://github.com/D-Robotics/hobot_hdmi)

### 支持平台

| 平台     | 运行方式     | 示例功能                       |
| -------- | ------------ | ------------------------------ |
| RDK X3, RDK X3 Module | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | 启动MIPI摄像头，并通过HDMI展示图像 |
| RDK X5, RDK X5 Module | Ubuntu 22.04 (Humble) | 启动MIPI摄像头，并通过HDMI展示图像 |

:::caution **注意**
HDMI展示**EOL**说明：
- `RDK X3`和`RDK X3 Module`平台支持到`2.1.0`版本，对应TROS版本`2.2.0 (2024-04-11)`。
- `RDK X5`和`RDK X5 Module`平台支持到`2.4.2`版本，对应TROS版本`2.3.1 (2024-11-20)`。
:::

### 准备工作

#### RDK平台

1. RDK已烧录好Ubuntu 20.04/Ubuntu 22.04系统镜像。

2. RDK已成功安装TogetheROS.Bot。

3. RDK已HDMI连接显示器。

### 使用介绍

#### RDK平台

通过SSH登录开发板，启动板端相关程序：
=======
3. Open a PC browser (Chrome/Firefox/Edge) and navigate to `http://IP:8000` to view the image stream. Here, IP refers to the PC’s IP address; if accessing locally, you may use `localhost`.

### Notes

1. The websocket service requires port 8000. If this port is already in use, the service will fail to start. To resolve this:
   - Use the command `lsof -i:8000` to identify the process occupying port 8000, then terminate it using `kill <PID>`. Afterward, restart the websocket service.
   - If the user does not wish to stop the service currently occupying port 8000, they can modify the `listen` port number in the `/opt/tros/${TROS_DISTRO}/lib/websocket/webservice/conf/nginx.conf` configuration file to an unused port number greater than 1024. After changing the port number, the URL used on the browser side must also be updated accordingly.

## HDMI Display

### Feature Introduction

This section describes how to display camera NV12 images via HDMI. By connecting the RDK to a monitor through HDMI, real-time image display can be achieved, which corresponds to the `hobot_hdmi` package.

Code repository: [https://github.com/D-Robotics/hobot_hdmi](https://github.com/D-Robotics/hobot_hdmi)

### Supported Platforms

| Platform                  | Runtime Environment                     | Example Functionality                                  |
| ------------------------- | --------------------------------------- | ------------------------------------------------------ |
| RDK X3, RDK X3 Module     | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | Launch MIPI camera and display images via HDMI         |
| RDK X5, RDK X5 Module     | Ubuntu 22.04 (Humble)                   | Launch MIPI camera and display images via HDMI         |

:::caution **Notice**
**End-of-Life (EOL)** notice for HDMI display:
- Support for the `RDK X3` and `RDK X3 Module` platforms ends at version `2.1.0`, corresponding to TROS version `2.2.0 (2024-04-11)`.
- Support for the `RDK X5` and `RDK X5 Module` platforms ends at version `2.4.2`, corresponding to TROS version `2.3.1 (2024-11-20)`.
:::

### Prerequisites

#### RDK Platform

1. The RDK has been flashed with an Ubuntu 20.04 or Ubuntu 22.04 system image.

2. TogetheROS.Bot has been successfully installed on the RDK.

3. The RDK is connected to a monitor via HDMI.

### Usage Instructions

#### RDK Platform

Log into the development board via SSH and launch the relevant onboard programs:
>>>>>>> 3fc6ea4 (Init commit)

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
<<<<<<< HEAD
# 配置tros.b环境
=======
# Configure the tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
<<<<<<< HEAD
# 配置tros.b环境
source /opt/tros/humble/setup.bash
```

使用RDK X5时, 需要额外使用下面命令:
```bash
# 关闭桌面显示
sudo systemctl stop lightdm
# 复制运行依赖
=======
# Configure the tros.b environment
source /opt/tros/humble/setup.bash
```

When using the RDK X5, you additionally need to run the following commands:
```bash
# Stop the desktop display service
sudo systemctl stop lightdm
# Copy runtime dependencies
>>>>>>> 3fc6ea4 (Init commit)
cp -r /opt/tros/${TROS_DISTRO}/lib/hobot_hdmi/config/ .
```

</TabItem>

</Tabs>

```shell
<<<<<<< HEAD
# HDMI图像渲染
ros2 launch hobot_hdmi hobot_hdmi.launch.py device:=F37
```

### 结果分析

在运行终端输出如下信息：
=======
# Render images via HDMI
ros2 launch hobot_hdmi hobot_hdmi.launch.py device:=F37
```

### Result Analysis

The terminal output after running the command will look like this:
>>>>>>> 3fc6ea4 (Init commit)

```text
[INFO] [launch]: All log files can be found below /root/.ros/log/2022-07-27-15-27-26-362299-ubuntu-13432
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [mipi_cam-1]: process started with pid [13434]
[INFO] [hobot_hdmi-2]: process started with pid [13436]
```

<<<<<<< HEAD
显示器显示图像如下：
![hdmi](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/hdmi.png)

## RViz2展示

### 功能介绍

TogetheROS.Bot兼容ROS2 foxy/humble版本，为了方便预览图像效果，可以通过RViz2获取图像。

### 支持平台

| 平台    | 运行方式      |
| ------- | ------------- |
| RDK X3, RDK X3 Module | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) |
| RDK X5, RDK X5 Module, RDK S100 | Ubuntu 22.04 (Humble) |
| RDK Ultra | Ubuntu 20.04 (Foxy) |

### 准备工作

#### RDK平台

1. RDK已烧录好Ubuntu 20.04/Ubuntu 22.04系统镜像。

2. RDK已成功安装tros.b。

3. PC已安装Ubuntu 20.04/Ubuntu 22.04系统、ROS2 Foxy/Humble桌面版和数据可视化工具RViz2，并且和RDK在同一网段（IP地址前三位相同）。

   - ROS2安装参考：[Foxy版本](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html)，[Humble版本](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)

   - PC 端安装 RViz2：`sudo apt install ros-$ROS_DISTRO-rviz-common ros-$ROS_DISTRO-rviz-default-plugins ros-$ROS_DISTRO-rviz2`。其中`$ROS_DISTRO`为ROS2版本，如`foxy`、`humble`。

### 使用方式

#### RDK平台

1. 通过SSH登录开发板，启动板端相关程序
=======
The image displayed on the monitor will appear as follows:  
![hdmi](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/hdmi.png)

## RViz2 Display

### Feature Introduction

TogetheROS.Bot is compatible with ROS 2 Foxy and Humble distributions. For convenient previewing of image output, images can be visualized using RViz2.

### Supported Platforms

| Platform                          | Runtime Environment      |
| --------------------------------- | ------------------------ |
| RDK X3, RDK X3 Module             | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) |
| RDK X5, RDK X5 Module, RDK S100   | Ubuntu 22.04 (Humble)    |
| RDK Ultra                         | Ubuntu 20.04 (Foxy)      |

### Prerequisites

#### RDK Platform

1. The RDK has been flashed with an Ubuntu 20.04 or Ubuntu 22.04 system image.

2. tros.b has been successfully installed on the RDK.

3. The PC has Ubuntu 20.04 or Ubuntu 22.04 installed, along with the ROS 2 Foxy/Humble desktop version and the RViz2 visualization tool. The PC must be on the same network segment as the RDK (i.e., the first three parts of their IP addresses must match).

   - ROS 2 installation guides: [Foxy](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html), [Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)

   - Install RViz2 on the PC:  
     `sudo apt install ros-$ROS_DISTRO-rviz-common ros-$ROS_DISTRO-rviz-default-plugins ros-$ROS_DISTRO-rviz2`  
     where `$ROS_DISTRO` refers to your ROS 2 distribution name, such as `foxy` or `humble`.

### Usage Instructions

#### RDK Platform

1. Log into the development board via SSH and launch the relevant onboard programs:
>>>>>>> 3fc6ea4 (Init commit)

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure the tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure the tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

   ```shell
<<<<<<< HEAD
   # 启动F37 camera发布BGR8格式图像
   ros2 launch mipi_cam mipi_cam.launch.py mipi_out_format:=bgr8 mipi_image_width:=480 mipi_image_height:=272 mipi_io_method:=ros mipi_video_device:=F37
   ```

   注意: mipi_out_format请勿随意更改，RViz2只支持RGB8, RGBA8, BGR8, BGRA8等图像格式.

2. 如程序输出如下信息，说明节点已成功启动
=======
   # Launch the F37 camera and publish images in BGR8 format
   ros2 launch mipi_cam mipi_cam.launch.py mipi_out_format:=bgr8 mipi_image_width:=480 mipi_image_height:=272 mipi_io_method:=ros mipi_video_device:=F37
   ```

   Note: Do not arbitrarily change `mipi_out_format`; RViz2 only supports image formats such as RGB8, RGBA8, BGR8, and BGRA8.

2. If the program outputs the following information, it indicates that the node has started successfully:
>>>>>>> 3fc6ea4 (Init commit)

   ```shell
   [INFO] [launch]: All log files can be found below /root/.ros/log/2022-08-19-03-53-54-778203-ubuntu-2881662
   [INFO] [launch]: Default logging verbosity is set to INFO
   [INFO] [mipi_cam-1]: process started with pid [2881781]
   ```

<<<<<<< HEAD
3. RDK新建一个窗口，查询话题命令及返回结果如下：
=======
3. Open a new terminal window on the RDK and run the following command to list topics:
>>>>>>> 3fc6ea4 (Init commit)

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure the tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure the tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

   ```shell
<<<<<<< HEAD
   # 查询topic
   ros2 topic list
   ```

   输出：
=======
   # List topics
   ros2 topic list
   ```

   Output:
>>>>>>> 3fc6ea4 (Init commit)

   ```shell
   /camera_info
   /image_raw
   /parameter_events
   /rosout
   ```

<<<<<<< HEAD
4. PC机上查询当前话题，查询命令及返回结果如下：
=======
4. On the PC, query the current topics using the following command and observe the output:

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

   ```shell
source /opt/ros/foxy/setup.bash
   ```

</TabItem>
<TabItem value="humble" label="Humble">

   ```shell
   source /opt/ros/humble/setup.bash
   ```

</TabItem>
</Tabs>

   ```shell
   # Configure ROS2 environment
   ros2 topic list
   ```

   Output:

   ```shell
   /camera_info
   /image_raw
   /parameter_events
   /rosout
   ```

1. Subscribe to the topic on the PC and preview camera data;
>>>>>>> 3fc6ea4 (Init commit)

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

   ```shell
   source /opt/ros/foxy/setup.bash
   ```

</TabItem>
<TabItem value="humble" label="Humble">

   ```shell
   source /opt/ros/humble/setup.bash
   ```

</TabItem>
</Tabs>

   ```shell
<<<<<<< HEAD
   # 配置ROS2环境
   ros2 topic list
   ```

   输出：

   ```shell
   /camera_info
   /image_raw
   /parameter_events
   /rosout
   ```

1. PC机上订阅话题，并预览摄像头数据；

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

   ```shell
   source /opt/ros/foxy/setup.bash
   ```

</TabItem>
<TabItem value="humble" label="Humble">

   ```shell
   source /opt/ros/humble/setup.bash
   ```

</TabItem>
</Tabs>

   ```shell
   # 配置ROS2环境
   ros2 run rviz2 rviz2
   ```

   在 RViz2 界面上首先点击 add 按钮，然后按照topic选择发布的图像，在该示例中topic名为/image_raw，然后点击image：

   ![rviz2-config](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rviz2-config.png)

   图像效果图如下：

   ![rviz2-result](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rviz2-result.png)

### 注意事项

1. 如遇到PC端ros2 topic list未识别到摄像头topic，排查：

   - 检查RDK是否正常pub图像
=======
   # Configure ROS2 environment
   ros2 run rviz2 rviz2
   ```

   In the RViz2 interface, first click the **Add** button, then select the published image topic—in this example, `/image_raw`—and finally click **Image**:

   ![rviz2-config](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rviz2-config.png)

   The resulting image preview is shown below:

   ![rviz2-result](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rviz2-result.png)

### Notes

1. If the camera topics do not appear when running `ros2 topic list` on the PC, troubleshoot as follows:

   - Verify that the RDK is correctly publishing images.
>>>>>>> 3fc6ea4 (Init commit)

      <Tabs groupId="tros-distro">
      <TabItem value="foxy" label="Foxy">

      ```bash
<<<<<<< HEAD
      # 配置tros.b环境
=======
      # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
      source /opt/tros/setup.bash
      ```

      </TabItem>

      <TabItem value="humble" label="Humble">

      ```bash
<<<<<<< HEAD
      # 配置tros.b环境
=======
      # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
      source /opt/tros/humble/setup.bash
      ```

      </TabItem>

      </Tabs>

      ```shell
      ros2 topic list
      ```

<<<<<<< HEAD
      输出：
=======
      Output:
>>>>>>> 3fc6ea4 (Init commit)

      ```shell
      /camera_info
      /image_raw
      /parameter_events
      /rosout
      ```

<<<<<<< HEAD
   - 检查PC和RDK网络能否ping通；
   - PC和RDK IP地址是否前三位相同；

## RQt展示

### 功能介绍

TogetheROS.Bot兼容ROS2 foxy版本，支持通过RQt预览压缩格式图像，可以大幅度降低网络带宽消耗。

### 支持平台

| 平台    | 运行方式      | 示例功能                       |
| ------- | ------------- | ------------------------------ |
| RDK X3, RDK X3 Module, RDK Ultra| Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble)  | 启动MIPI摄像头获取图像，在PC上使用RQt预览 |

### 准备工作

#### RDK平台

1. RDK已烧录好Ubuntu 20.04/Ubuntu 22.04系统镜像。

2. RDK已成功安装tros.b。

3. PC已安装Ubuntu 20.04/Ubuntu 22.04系统、ROS2 Foxy/Humble桌面版和数据可视化工具RQt，并且和RDK在同一网段（IP地址前三位相同）。

   - ROS2安装参考：[Foxy版本](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html)，[Humble版本](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)

   - PC 端安装 rqt-image-view ：`ros-$ROS_DISTRO-rqt-image-view ros-$ROS_DISTRO-rqt`。其中`$ROS_DISTRO`为ROS2版本，如`foxy`、`humble`。

### 使用方式

#### RDK平台

1. 通过SSH登录开发板，启动板端相关程序
   
   a. 启动F37 camera
=======
   - Check whether the PC and RDK can ping each other.
   - Ensure the first three segments of the IP addresses of the PC and RDK are identical.

## RQt Visualization

### Feature Overview

TogetheROS.Bot is compatible with ROS 2 Foxy and supports previewing compressed-format images via RQt, significantly reducing network bandwidth consumption.

### Supported Platforms

| Platform                              | Operating System                     | Example Functionality                                               |
| ------------------------------------- | ------------------------------------ | ------------------------------------------------------------------- |
| RDK X3, RDK X3 Module, RDK Ultra      | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | Launch MIPI camera to capture images and preview them using RQt on PC |

### Prerequisites

#### RDK Platform

1. The RDK has been flashed with Ubuntu 20.04 or Ubuntu 22.04 system image.
2. tros.b has been successfully installed on the RDK.

3. The PC has Ubuntu 20.04/Ubuntu 22.04 installed, along with ROS 2 Foxy/Humble Desktop and the RQt visualization tool, and is on the same subnet as the RDK (i.e., the first three segments of their IP addresses match).

   - ROS 2 installation guides: [Foxy](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html), [Humble](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html)
   - Install `rqt-image-view` on the PC:  
     `sudo apt install ros-$ROS_DISTRO-rqt-image-view ros-$ROS_DISTRO-rqt`,  
     where `$ROS_DISTRO` is your ROS 2 distribution name (e.g., `foxy` or `humble`).

### Usage Instructions

#### RDK Platform

1. Log into the development board via SSH and launch the required nodes.

   a. Start the F37 camera:
>>>>>>> 3fc6ea4 (Init commit)

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

   ```shell
   ros2 launch mipi_cam mipi_cam.launch.py mipi_image_width:=640 mipi_image_height:=480 mipi_video_device:=F37
   ```

<<<<<<< HEAD
   b. 启动hobot_codec, 发布compressed格式图像
=======
   b. Launch `hobot_codec` to publish images in compressed format:
>>>>>>> 3fc6ea4 (Init commit)

   <Tabs groupId="tros-distro">
   <TabItem value="foxy" label="Foxy">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/setup.bash
   ```

   </TabItem>

   <TabItem value="humble" label="Humble">

   ```bash
<<<<<<< HEAD
   # 配置tros.b环境
=======
   # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
   source /opt/tros/humble/setup.bash
   ```

   </TabItem>

   </Tabs>

   ```shell
   ros2 launch hobot_codec hobot_codec_encode.launch.py codec_out_format:=jpeg codec_pub_topic:=/image_raw/compressed
   ```

<<<<<<< HEAD
2. 如程序输出如下信息，说明节点已成功启动
=======
2. If the following messages appear in the output, the nodes have started successfully:
>>>>>>> 3fc6ea4 (Init commit)

   ```shell
   [INFO] [launch]: All log files can be found below /root/.ros/log/2023-05-15-17-08-02-144621-ubuntu-4755
   [INFO] [launch]: Default logging verbosity is set to INFO
   [INFO] [mipi_cam-1]: process started with pid [4757]
   [mipi_cam-1] This is version for optimizing camera timestamp 
   ```

   ```shell
   [INFO] [launch]: All log files can be found below /root/.ros/log/2023-05-15-17-08-17-960398-ubuntu-4842
   [INFO] [launch]: Default logging verbosity is set to INFO
   [INFO] [hobot_codec_republish-1]: process started with pid [4844]
   ```

<<<<<<< HEAD
3. PC机上订阅话题，并预览摄像头数据；
=======
3. Subscribe to the topic on your PC and preview the camera data:
>>>>>>> 3fc6ea4 (Init commit)

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

   ```shell
   source /opt/ros/foxy/setup.bash
   ```

</TabItem>
<TabItem value="humble" label="Humble">

   ```shell
   source /opt/ros/humble/setup.bash
   ```

</TabItem>
</Tabs>

   ```shell
<<<<<<< HEAD
   # 配置ROS2环境
   ros2 run rqt_image_view rqt_image_view
   ```

   选择话题`/image_raw/compressed`，图像效果图如下：

   ![](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rqt-result.png)

### 注意事项

1. 如遇到PC端ros2 topic list未识别到摄像头topic，做如下排查：

   - 检查RDK是否正常pub图像
=======
   # Configure ROS2 environment
   ros2 run rqt_image_view rqt_image_view
   ```

   Select the topic `/image_raw/compressed`. The resulting image is shown below:

   ![](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/rqt-result.png)

### Notes

1. If the camera topic does not appear in `ros2 topic list` on your PC, perform the following checks:

   - Verify that the RDK is publishing images correctly.
>>>>>>> 3fc6ea4 (Init commit)

      <Tabs groupId="tros-distro">
      <TabItem value="foxy" label="Foxy">

      ```bash
<<<<<<< HEAD
      # 配置tros.b环境
=======
      # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
      source /opt/tros/setup.bash
      ```

      </TabItem>

      <TabItem value="humble" label="Humble">

      ```bash
<<<<<<< HEAD
      # 配置tros.b环境
=======
      # Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
      source /opt/tros/humble/setup.bash
      ```

      </TabItem>

      </Tabs>

      ```shell
      ros2 topic list
      ```

<<<<<<< HEAD
      输出：
=======
      Output:
>>>>>>> 3fc6ea4 (Init commit)

      ```text
      /camera_info
      /hbmem_img000b0c26001301040202012020122406
      /image_raw
      /image_raw/compressed
      /parameter_events
      /rosout
      ```

<<<<<<< HEAD
   - 检查PC和RDK网络能否ping通；
   - PC和RDK IP地址是否前三位相同；

## Foxglove展示

### 功能介绍

Foxglove是一个开源的工具包，包括线上和线下版。旨在简化机器人系统的开发和调试。它提供了一系列用于构建机器人应用程序的功能。

本章节主要用到Foxglove数据记录和回放功能：Foxglove允许将ROS2话题的数据记录到文件中，以便后续回放和分析。这对于系统故障诊断、性能优化和算法调试非常有用。

演示中，我们会利用TogetheROS开发的hobot_visualization功能包，将智能推理结果转换为ROS2渲染的话题信息。

代码仓库：[https://github.com/D-Robotics/hobot_visualization](https://github.com/D-Robotics/hobot_visualization)

### 支持平台

| 平台    | 运行方式      |
| ------- | ------------- |
| RDK X3, RDK X3 Module | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) |
| RDK X5, RDK X5 Module, RDK S100 | Ubuntu 22.04 (Humble) |
| X86     | Ubuntu 20.04 (Foxy) |

### 准备工作

#### RDK平台

1. 确认摄像头F37正确接到RDK上

2. 确认PC可以通过网络访问RDK

3. 确认已成功安装TogetheROS.Bot

#### X86平台

1. 确认X86平台系统为Ubuntu 20.04，且已成功安装tros.b

### 使用方式

#### RDK平台 / X86平台

1. 通过SSH登录RDK平台，启动板端相关程序：
=======
   - Check whether the PC and RDK can ping each other.
   - Ensure that the first three octets of the IP addresses of the PC and RDK are identical.

## Foxglove Demonstration

### Feature Overview

Foxglove is an open-source toolkit available both online and offline, designed to simplify the development and debugging of robotic systems. It provides a suite of features for building robot applications.

This section primarily leverages Foxglove's data recording and playback capabilities: Foxglove allows recording ROS2 topic data into files for later playback and analysis. This is extremely useful for system diagnostics, performance optimization, and algorithm debugging.

In this demonstration, we use the `hobot_visualization` package developed with TogetheROS to convert intelligent inference results into ROS2-renderable topic messages.

Code repository: [https://github.com/D-Robotics/hobot_visualization](https://github.com/D-Robotics/hobot_visualization)

### Supported Platforms

| Platform                              | Runtime Environment                     |
| ------------------------------------- | --------------------------------------- |
| RDK X3, RDK X3 Module                 | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) |
| RDK X5, RDK X5 Module, RDK S100       | Ubuntu 22.04 (Humble)                   |
| X86                                   | Ubuntu 20.04 (Foxy)                     |

### Prerequisites

#### RDK Platform

1. Confirm that camera F37 is properly connected to the RDK.
2. Confirm that the PC can access the RDK over the network.
3. Confirm that TogetheROS.Bot has been successfully installed.

#### X86 Platform

1. Confirm that the X86 platform runs Ubuntu 20.04 and that tros.b has been successfully installed.

### Usage Instructions

#### RDK Platform / X86 Platform

1. Log in to the RDK platform via SSH and launch the onboard programs:
>>>>>>> 3fc6ea4 (Init commit)

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
<<<<<<< HEAD
# 配置tros.b环境
=======
# Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
<<<<<<< HEAD
# 配置tros.b环境
=======
# Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
export CAM_TYPE=fb
cp -r /opt/tros/${TROS_DISTRO}/lib/dnn_node_example/config/ .

ros2 launch hobot_visualization hobot_vis_render.launch.py
```

<<<<<<< HEAD
同时，利用ssh登录另一个终端，在板端记录话题信息：
=======
Meanwhile, log in via SSH to another terminal and record topic data on the board:
>>>>>>> 3fc6ea4 (Init commit)

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
<<<<<<< HEAD
# 配置tros.b环境
=======
# Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
<<<<<<< HEAD
# 配置tros.b环境
=======
# Configure tros.b environment
>>>>>>> 3fc6ea4 (Init commit)
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
<<<<<<< HEAD
# 记录rosbag数据，会生成在当前工作目录下
ros2 bag record -a
```

2. Foxglove在线页面播放rosbag数据

1）PC浏览器（chrome/firefox/edge）输入 (https://foxglove.dev/studio) , 进入foxglove官网

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_1.png)

PS: 首次使用需要注册, 可使用谷歌账号或第三方邮箱进行注册。

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_11.png)

2）进入可视化功能界面

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_2.png)

3）点击选中本地rosbag文件

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_3.png)

4）打开布局界面，在布局界面右上角，点击设置，选中图标，打开播放maker渲染消息功能

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_4.png)

5）点击播放
   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_5.png)

6）观看数据
   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_6.png)

### 注意事项

1. Foxglove可视化图像数据，需采用ROS2官方的消息格式，使用foxglove支持的图像编码格式，详情请见 (https://foxglove.dev/docs/studio/panels/image)。

2. rosbag进行消息记录时，可能会录制其他设备的话题信息，因此为了保证rosbag数据的干净，可以通过设置'export ROS_DOMAIN_ID=xxx' ，如'export ROS_DOMAIN_ID=1'的方法。
=======
# Record rosbag data; the bag file will be generated in the current working directory
ros2 bag record -a
```

2. Play back the rosbag data using Foxglove Studio online:

1) Open a browser on your PC (Chrome/Firefox/Edge) and navigate to [https://foxglove.dev/studio](https://foxglove.dev/studio) to access the Foxglove website.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_1.png)

Note: First-time users need to register—sign-up is supported via Google account or third-party email.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_11.png)

2) Enter the visualization interface.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_2.png)

3) Click to select your local rosbag file.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_3.png)

4) Open the layout panel. In the top-right corner of the layout interface, click Settings, select the icon, and enable the "Play marker rendering messages" feature.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_4.png)

5) Click Play.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_5.png)

6) View the data.

   ![foxglove](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/02_quick_demo/image/demo_render/foxglove_guide_6.png)
### Notes

1. To visualize image data in Foxglove, you must use the official ROS 2 message format and an image encoding format supported by Foxglove. For details, see (https://foxglove.dev/docs/studio/panels/image).

2. When recording messages with rosbag, topics from other devices might also be captured. To ensure clean rosbag data, you can set the ROS domain ID using a command such as `export ROS_DOMAIN_ID=xxx` (e.g., `export ROS_DOMAIN_ID=1`).
>>>>>>> 3fc6ea4 (Init commit)
