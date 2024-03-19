# LightwheelOcc
<div id="subtitle" align="center">自动驾驶领域的3D Occupancy合成数据集</div>

<div id="top" align="center">
<img src="resources/occ_video.gif">
</div>

[English](./README.md) | 简体中文

## 更新日志

## 目录
- [LightwheelOcc](#lightwheelocc)
  - [更新日志](#更新日志)
  - [目录](#目录)
  - [1. 亮点](#1-亮点)
  - [2. 介绍](#2-介绍)
  - [3. 数据概览](#3-数据概览)
    - [3.1 基本信息](#31-基本信息)
    - [3.2 数据样本](#32-数据样本)
    - [3.3 数据分布](#33-数据分布)
    - [3.4 测试与结果](#34-测试与结果)
    - [3.5 最佳实践](#35-最佳实践)
  - [4. 开始使用](#4-开始使用)
    - [4.1 下载数据](#41-下载数据)
    - [4.2 准备数据集](#42-准备数据集)
  - [5. 许可与引用](#5-许可与引用)
  - [6. 联系我们](#6-联系我们)

## 1. 亮点
**丰富的数据分布，包括角落案例和困难场景**

通过整合复杂的交通流，LightwheelOcc包含了不同交通条件和驾驶行为的多样化模拟。除了常见场景外，该数据集还展示了如道路上的小型和罕见物体等角落案例，以及如夜间和雨天场景等挑战条件，丰富了真实世界数据的多样性。
<div align="center">
  <img src="resources/occ_sample_1.png" alt="occ_sample_1" width="30%">
  <img src="resources/occ_sample_2.png" alt="occ_sample_2" width="30%">
  <img src="resources/occ_sample_3.png" alt="occ_sample_3" width="30%">
</div>

## 2. 介绍
- LightwheelOcc由Lightwheel AI开发，是一个公开可用的自动驾驶合成数据集。该数据集包含40,000帧及其对应的各种任务的真值标签，是一个概括性数据集，覆盖了多种地区地形、天气模式、车辆类型、植被和道路标记。
- Lightwheel AI利用生成式AI和仿真技术，为自动驾驶和具体化AI提供3D、物理真实且可概括的合成数据解决方案。通过发布LightwheelOcc，我们旨在推进计算机视觉、自动驾驶和合成数据领域的研究。

## 3. 数据概览
### 3.1 基本信息
- LightwheelOcc数据集包含40,000帧，总计240,000张图像，其中30,000帧用于训练场景，5,000帧用于验证场景，5,000帧用于测试场景。
- LightwheelOcc包括6个摄像头传感器数据，以及包括3D占据和深度图在内的不同任务的标签。
- 数据集包含18个类别。0至16类与[nuScenes-lidarseg](https://github.com/nutonomy/nuscenes-devkit/blob/fcc41628d41060b3c1a86928751e5a571d2fc2fa/python-sdk/nuscenes/eval/lidarseg/README.md)数据集的定义相同。标签17类别表示未占据的体素，称为[free]。每个样本的体素语义在labels.npz文件中以[semantics]的形式提供。

| **类型**          | **信息**                     |
|-------------------|------------------------------|
| 训练集             | 30000帧                 |
| 验证集        | 5000帧                  |
| 测试集              | 5000帧                  |
| 摄像头数量 | 6                            |
| 图像数量  | 240,000                      |
| 像素大小        | 1600x900                     |
| 范围             | [-40, -40, -1.0, 40, 40, 5.4]|
| 类别           | 18 (不包括忽略类别)          |
| 标签            | 3D占据和深度图   |

### 3.2 数据样本
| **3D Occupancy**    | **深度图**            |
|---------------------|--------------------------|
| <img src="resources/sample_occ.png" alt="3D Occupancy" width="300"> | <img src="resources/sample_depth.jpeg" alt="深度图" width="300"> |

### 3.3 数据分布
<table>
  <tr>
    <th>分类</th>
    <th>类别</th>
    <th>分布</th>
  </tr>
  <tr>
    <td rowspan="3">按地图类型</td>
    <td>城区</td>
    <td>68%</td>
  </tr>
  <tr>
    <td>郊区</td>
    <td>9%</td>
  </tr>
  <tr>
    <td>高速公路</td>
    <td>23%</td>
  </tr>
  <tr>
    <td rowspan="2">按光照条件</td>
    <td>白天</td>
    <td>87%</td>
  </tr>
  <tr>
    <td>夜间</td>
    <td>13%</td>
  </tr>
  <tr>
    <td rowspan="3">按天气类型</td>
    <td>晴朗</td>
    <td>45%</td>
  </tr>
  <tr>
    <td>阴天</td>
    <td>38%</td>
  </tr>
  <tr>
    <td>雨天或雨后</td>
    <td>17%</td>
  </tr>
</table>

### 3.4 测试与结果
本实验使用OccNet训练，结果供参考。

| **Train**                 | **Val**                   | **mIoU**       |
|---------------------------|---------------------------|----------------|
| nuScenes                  | nuScenes + LightwheelOcc  |   15.61        |
| nuScenes + LightwheelOcc  | nuScenes + LightwheelOcc  |   28.77        |
| nuScenes                  | nuScenes                  |   24.84        |
| nuScenes + LightwheelOcc  | nuScenes                  |   26.97        |

### 3.5 最佳实践
通常，在训练数据时，检查三个关键指标至关重要，以此来根据它们的表现确定训练方法：
1. 真实数据和合成数据的标签是否对齐：合成数据的标签对齐通常指的是标签是否与现实世界数据集中的那些一致。如果不对齐，模型可能无法正确区分每个类别。
2. 真实数据和合成数据的数量是否可比：数据量决定了模型训练的充分性。数据量不足可能导致过拟合，而大量数据可以提高模型的泛化能力。
3. 合成数据是Corner Case还是通用数据：数据类型（如Corner Case或通用数据）影响模型的泛化能力和对特定情况的敏感度。

在选择训练方法和数据采样策略时，必须综合考虑上述问题，以确保模型性能。
至于LightwheelOcc，标签已对齐，真实数据的数量大于合成数据，旨在解决Corner Case问题。因此，我们建议使用混合训练方法，即直接混合合成数据和真实数据进行训练。

## 4. 开始使用
### 4.1 下载数据
- 谷歌云盘：待发布
- 百度网盘：待发布
- OpenDataLab：待发布


### 4.2 准备数据集
- 目录结构
```
LightwheelOcc
├── data/
│   ├── nuscenes/
│   │   ├── maps/
│   │   ├── samples/
│   │   ├── sweeps/
│   │   ├── v1.0-test
│   │   ├── v1.0-trainval
│   │   ├── nuscenes_infos_temporal_train.pkl
│   │   ├── nuscenes_infos_temporal_val.pkl   
│   ├── Occpancy3D-nuScenes-V1.0/
│   ├── lightwheel_occ
│   │   ├── lightwheel_occ_infos.pkl
│   │   ├── depth
│   │   │   ├── CAM_BACK
│   │   │   │   ├── 00b638f5-ef65-44f0-95f2-29caae0bc4ea
│   │   │   │   │   ├── 1710101555.452797.png
│   │   │   │   │   ├── ...
│   │   │   │   ├── ...
│   │   │   ├── ...
│   │   ├── samples
│   │   │   ├── CAM_BACK
│   │   │   │   ├── 00b638f5-ef65-44f0-95f2-29caae0bc4ea
│   │   │   │   │   ├── 1710101555.452797.jpeg
│   │   │   │   │   ├── ...
│   │   │   │   ├── ...
│   │   │   ├── ...
│   │   ├── occupancy
│   │   │   ├── 00b638f5-ef65-44f0-95f2-29caae0bc4ea
│   │   │   │   │   ├── 1710101555.452797.npz
│   │   │   │   │   ├── ...
│   │   │   │   ├── ...
│   │   │   ├── ...
```
- *samples/* 包含由各种摄像头捕获的图像。
- *depth/* 包含每个样本的深度标签。
- *lightwheel_occ_infos.pkl* 包含数据集的元信息。
- *labels.npz* 包含每帧的*semantics*、*mask_lidar* 和 *mask_camera*。
```
annotations {
    "train_split": ["scene-0001", ...],                         <list> -- 训练数据集按scene_name的划分
    "val_split": list ["scene-0003", ...],                      <list> -- 验证数据集按scene_name的划分
    "scene_infos" {                                             <dict> -- 场景的元信息    
        [scene_name]: {                                         <str> -- 场景的名称  
            [frame_token]: {                                    <str> -- 场景中的样本，按时间顺序排列
                    "timestamp":                                <str> -- 时间戳（或token），每个样本唯一
                    "camera_sensor": {                          <dict> -- 摄像头传感器的元信息
                        [cam_token]: {                          <str> -- 摄像头的token
                            "img_path":                         <str> -- 对应的图像文件路径，.jpg
                            "intrinsic":                        <float> [3, 3] -- 摄像头的内参校准
                            "extrinsic":{                       <dict> -- 摄像头的外参
                                "translation":                  <float> [3] -- 坐标系原点，以米为单位
                                "rotation":                     <float> [4] -- 坐标系方向，四元数表示
                            }   
                            "ego_pose": {                       <dict> -- 摄像头的车辆姿态
                                "translation":                  <float> [3] -- 坐标系原点，以米为单位
                                "rotation":                     <float> [4] -- 坐标系方向，四元数表示
                            }                
                        },
                        ...
                    },
                    "ego_pose": {                               <dict> -- 车辆姿态
                        "translation":                          <float> [3] -- 坐标系原点，以米为单位
                        "rotation":                             <float> [4] -- 坐标系方向，四元数表示
                    },
                    "gt_path":                                  <str> -- 对应的3D体素真实路径，.npz
                    "next":                                     <str> -- 场景中上一个关键帧的frame_token 
                    "prev":                                     <str> -- 场景中下一个关键帧的frame_token
                }         
        }
    }
}
```


## 5. 许可与引用
该数据集由Lightwheel AI LIMITED提供，保留所有权利。未经书面许可，不得复制或重新发布。
```
@article{lightwheel2024,
 title={LightwheelOcc: A Synthetic Occupancy Dataset in Autonomous Driving},
  author={Lightwheel AI LIMITED},
  year={2024}
}
```

## 6. 联系我们
**网站** [https://lightwheel.ai/]

**联系方式**: [contact@lightwheel.ai]

如果您对此感兴趣或有任何疑问，欢迎联系我们。
