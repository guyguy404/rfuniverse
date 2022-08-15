# RFUnivese Player 接口文档

Unity Player在初始状态下是不包含任何物体空白场景，物体通过python端发送消息实现创建和驱动。创建的物体必须分配一个全局唯一的ID，后续驱动物体的操作通过该ID来进行区分。

---

实例化仿真环境后将会打开Player窗口，随后调用所有接口：

```
from pyrfuniverse.envs import RFUniverseBaseEnv

env = RFUniverseBaseEnv
(
    executable_file='<rfuniverse_path>',
    scene_file='<scenedata_file_name>',
    assets=assets_name_list,
)
```

* (str)executable_file：RFUniverse可执行文件路径，不使用此参数则默认为Unity Editor
* (str)scene_file：需要加载的场景Json文件名
* (list<str>)assets：需要预加载的物体名列表，预加载过的物体在InstanceObject时会立即生成，否则会异步生成可能造成一些错误

---

### AssetChannel：

负责资源加载创建以及环境中的通用功能

* SendMessage：发送一个字符串消息
  
  ```
  env.asset_channel.SendMessage
  (
      'msgString'
  )
  ```
  
  * (必要) (str)msg：消息标识
  
  ---

* SetLayerCollision：设置物体层间碰撞的开启或关闭
  
  ```
  env.asset_channel.set_action
  (
      'SetLayerCollision',
      layer1=1,
      layer2=2,
      ignore=True
  )
  ```
  
  * (必要) (int) layer1：层1
  * (必要) (int) layer2：层2
  * (必要) (bool) ignore：忽略或不忽略碰撞
  
  ---

* InstanceObject：创建物体。
  
  ```
  env.asset_channel.set_action
  (
      'InstanceObject',
      name='kinova_gen3_robotiq85',
      id=123
  )
  ```
  
  * (必要) (stirng) name：资源名称
  * (必要) (int) id：物体创建后赋予实例ID
    
    
    
    ###### 现有资源类型及名称：
    - GameObjcet：静态模型
      
      ###### Base:
      
      GameObject_Box
      
      GameObject_Cylinder
      
      GameObject_Sphere
      
      GameObject_Quad
      
      Collider_Box
      
      ###### IGbison环境：
      
      Hainesburg_mesh_texture
      
      Halfway_mesh_texture
      
      Hallettsville_mesh_texture
      
      Hambleton_mesh_texture
      
      Hammon_mesh_texture
      
      Hatfield_mesh_texture
      
      Haxtun_mesh_texture
      
      Haymarket_mesh_texture
      
      Hendrix_mesh_texture
      
      Hercules_mesh_texture
      
      Highspire_mesh_texture
      
      Hitchland_mesh_texture
    
    - Rigidbody：刚体
      
      ###### Base:
      
      Rigidbody_Box
      
      Rigidbody_Cylinder
      
      Rigidbody_Sphere
      
      ###### 77个YCB数据集模型:
      
      详见[The YCB Object and Model Set](http://ycb-benchmarks.s3-website-us-east-1.amazonaws.com/)
    
    - Controller：机械臂及关节体
      
      ###### Robots:
      
      kinova_gen3_robotiq85
      
      ur5_robotiq85
      
      franka_panda
      
      tobor_robotiq85_robotiq85
      
      ###### Other:
      
      pen_and_pencap
      
      handled_box,handled_drawer
      
      Drawer1,Drawer2,Drawer3,Drawer4,Drawer5,Drawer6,Drawer7,Drawer8,Drawer9,Drawer10

---

### InstanceChannel

针对创建后但个物体的接口
所有物体可用：

* SetTransform：设置物体Position, Rotation, Scale
  
  ```
  env.instance_channel.set_action
  (
      'SetTransform',
      id=123,
      position=[0,0,0],
      rotation=[0,0,0],
      scale=[0,0,0]
  )
  ```
  
  * (必要) (int) id：物体实例ID
  * (list<float>[3]) position：局部坐标位置
  * (list<float>[3]) rotation：局部坐标旋转
  * (list<float>[3]) scale：局部坐标缩放
  
  ---

* SetRotationQuaternion：设置物体Quaternion
  
  ```
  env.instance_channel.set_action
  (
      'SetRotationQuaternion',
      id=123,
      quaternion=[0,0,0]
  )
  ```
  
  * (必要) (int) id：物体实例ID
  * (list<float>[4]) quaternion：局部坐标四元数旋转
  
  ---

* SetActive：设置物体激活状态
  
  ```
  env.instance_channel.set_action
  (
      'SetActive',
      id=123,
      active=Fales
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (bool)active：是否激活
  
  ---

* SetParent：设置父物体
  
  ```
  env.instance_channel.set_action
  (
      'SetParent',
      id=123,
      parent_id=456,
      name='parent'
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (int)parent_id：父物体所在实例的ID，-1则不设父物体
  * (必要) (string)name：父物体名，为空则防止在根节点
  
  ---

* SetLayer：设置物体层
  
  ```
  env.instance_channel.set_action
  (
      'SetLayer',
      id=123,
      layer=1
  )  
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (int)layer：层编号

---

* Destroy：销毁物体
  
  ```
  env.instance_channel.set_action
  (
      'Destroy',
      id=123
  )
  ```
  
  * (必要) (int)id：物体实例ID

---

GameObject类型可用：

* Translate：增量移动物体
  
  ```
  env.instance_channel.set_action
  (
      'Translate',
      id=123,
      translation=[0,0,0]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[3])translation：增量移动值
  
  ---

* Rotate：增量旋转物体
  
  ```
  env.instance_channel.set_action
  (
      'Rotate',
      id=123,
      rotate=[0,0,0]
  )  
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[3])translation：增量旋转值
  
  ---

* SetColor：修改物体基础颜色
  
  ```
  env.instance_channel.set_action
  (
      'SetColor',
      id=123,
      color=[0,0,0,0]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[4])color：颜色rgba值

---

Rigidbody类型可用：

* AddForce：为Rigidbody施加持续的力
  
  ```
  env.instance_channel.set_action
  (
      'AddForce',
      id=123,
      force=[0,0,0]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[3])force：施加力值
  
  ---

* SetVelocity：修改Rigidbody速度
  
  ```
  env.instance_channel.set_action
  (
      'SetVelocity',
      id=123,
      velocity=[0,0,0]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[3])velocity：速度值

---

Articulation类型可用：

* SetJointPosition：修改关节体内每个关节的目标位置
  
  ```
  env.instance_channel.set_action
  (
      'SetJointPosition',
      id=123,
      joint_positions=[],
      speed_scales=[]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[n])joint_positions：关节位置
  * (list<float>[n])speed_scales：关节速度倍率
  
  ---

* SetJointPositionDirectly：直接修改关节体内每个关节的位置
  
  ```
  env.instance_channel.set_action
  (
      'SetJointPositionDirectly',
      id=123,
      joint_positions=[]
  )
  ```
  
  * (必要) (int)id：物体实例ID
  * (必要) (list<float>[n])joint_positions：关节位置

---