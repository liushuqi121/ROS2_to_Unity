主要介绍基于ROS-TCP-Connector、ROS-TCP-Endpoint两个Unity接口与ROS进行通信的环境配置，并对官方给出的Unity和ROS相互通信示例中的消息部分做了说明。

## 一、环境配置

参考：（https://github.com/Unity-Technologies/Unity-Robotics-Hub/blob/main/tutorials/ros_unity_integration/setup.md）

### Ubuntu环境下

#### 1.成功配置ros2环境

1. 系统Ubuntu20.04+

#### 2.下载ROS-TCP-Endpoint放在工作空间编译

2. ROS-TCP-Endpoint下载地址：（https://github.com/Unity-Technologies/ROS-TCP-Connector.git?path=/com.unity.robotics.ros-tcp-connector
   
https://github.com/Unity-Technologies/ROS-TCP-Connector.git?path=/com.unity.robotics.visualizations
）

### Unity环境下

1.创建新项目，在Window/Package Manager下导入两个包(方式有两个，本地导入和URL导入)



  - 本地导入：将包下载到本地，添加来自磁盘的包。通常后缀名为package.json

- URL导入：将github链接复制粘贴进去自动导入。

导入成功如下：



  在Unity上方栏多出了Robotics选项，点击Robotics下面的ROS Settings进行配置，主要是两个ROS IP Address和ROS Port，关于ROS IP Address在ubuntu下用ifconfig查一下设置[ROS-TCP-Endpoint-0.7.0.zip](https://github.com/user-attachments/files/19888188/ROS-TCP-Endpoint-0.7.0.zip)
，然后端口任意了默认10000了。


到目前为止，两方的环境配置就结束了。

### 二、demo环境配置

Ubuntu环境
**下载这两个到同一个工作空间，然后clocon build一下。

下载路径：

Unity环境下
**点击“Robotics -> Generate ROS Messages…”


那个ROS message path是把整个项目下载下来tutorials/ros_unity_integration/ros_packages/unity_robotics_demo_msgs文件夹，最后再Build两个一下(为了换成C#文件)

### 三、实例：ros发布，Unity订阅

在Ubuntu工作空间下

ros2 launch ros_tcp_endpoint endpoint.py TCP_IP:=192.168.查看树莓派的IP ROS_TCP_port:=10000

#### 2.Unity端建立空物体挂载文件（命名：RosSubscriberExample）把以下代码放进去，再新建一个Cube，把Cube做参数传入文件

```Plain Text
using UnityEngine;
using Unity.Robotics.ROSTCPConnector;
using RosColor = RosMessageTypes.UnityRoboticsDemo.UnityColorMsg;

public class RosSubscriberExample : MonoBehaviour
{
    public GameObject cube;

    void Start()
    {
        ROSConnection.GetOrCreateInstance().Subscribe<RosColor>("color", ColorChange);
    }

    void ColorChange(RosColor colorMessage)
    {
        cube.GetComponent<Renderer>().material.color = new Color32((byte)colorMessage.r, (byte)colorMessage.g, (byte)colorMessage.b, (byte)colorMessage.a);
    }
}
```


然后运行unity会发现Ubuntu当前终端会打印信息

#### 3.unity运行

运行时出现下面的框，为蓝色时代表连接成功，红色是失败


在ubuntu工作空间下，ros2 run unity_robotics_demo color_publisher

会发布对应信息并发布，此时在unity的控制台会看到与Ubuntu发布的消息。

https://flowus.cn/share/5f63ea06-2b86-4800-b600-bfc0ba271b59?code=JUUFGX
【FlowUs 息流】ROS2_to_Unity
