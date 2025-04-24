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

![image.png](https://tc-cdn.flowus.cn/oss/761a199b-3ea5-4b7f-99c1-4445a97093f5/image.png?time=1745475300&token=73e35aa6ed67ebce346cd4e56a90de210f7faf605a5ac8201ea916b5f2a367a0&role=free)

  - 本地导入：将包下载到本地，添加来自磁盘的包。通常后缀名为package.json

- URL导入：将github链接复制粘贴进去自动导入。

导入成功如下：

![image.png](https://tc-cdn.flowus.cn/oss/213b34c3-6c16-43f5-888c-4bb8c549167b/image.png?time=1745475300&token=65909f44cbc0e8d9872429796076507885757d9a2e68e2b5375ad3da1006d8b1&role=free)

![image.png](https://tc-cdn.flowus.cn/oss/87faf304-b375-4040-92e0-584dd5c881ac/image.png?time=1745475300&token=b6dd02db17b490252b4cb85fce3e7f1a612e5f4715bcb78e8973715d43797cd3&role=free)

  在Unity上方栏多出了Robotics选项，点击Robotics下面的ROS Settings进行配置，主要是两个ROS IP Address和ROS Port，关于ROS IP Address在ubuntu下用ifconfig查一下设置[ROS-TCP-Endpoint-0.7.0.zip](https://github.com/user-attachments/files/19888188/ROS-TCP-Endpoint-0.7.0.zip)
，然后端口任意了默认10000了。

![image.png](https://tc-cdn.flowus.cn/oss/250bbdaa-109b-44f4-8afc-9c62af5fc918/image.png?time=1745476200&token=28594d6bf0e4cd8fde867df919e08e9779aab6e37144903d66ac9ac45261064d&role=free)

到目前为止，两方的环境配置就结束了。

### 二、demo环境配置

Ubuntu环境
**下载这两个到同一个工作空间，然后clocon build一下。

下载路径：

Unity环境下
**点击“Robotics -> Generate ROS Messages…”

![image.png](https://tc-cdn.flowus.cn/oss/be907fc4-7041-43a5-93d5-1524ca45a399/image.png?time=1745477100&token=becab28b39e73bdacbdb84b352b386129aad3d6a8ed6ab092d935a5e7e885794&role=free)

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

![image.png](blob:https://flowus.cn/3c84eb62-cf48-4a9c-b651-b645beb64aa4)

在ubuntu工作空间下，ros2 run unity_robotics_demo color_publisher

会发布对应信息并发布，此时在unity的控制台会看到与Ubuntu发布的消息。

