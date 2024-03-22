# China3DV - 占据栅格与运动估计 竞赛主文档
> 官方网站: :globe_with_meridians: [China3DV](http://www.csig3dv.net/2024/competition.html)  
> 评测服务器: :hugs: [Hugging Face](https://huggingface.co/spaces/China3DV-S/occupancy-and-flow-2024)

## 赛道介绍

三维框往往不足以描述一般物体，受机器人学概念的启发，可将感知表征描述成对栅格化三维空间的占据情况预测。在纯视觉环视相机输入下，参赛者不仅要给出三维空间的栅格化表示，还须给出栅格的运动预测。

本赛道在国际知名数据集 nuScenes 的基础上，引入了行业领先的[光轮智能](http://lightwheel.ai/)自动驾驶模拟器生成的高质量仿真数据。光轮占据栅格仿真数据集（LightwheelOcc）高度重现 nuScenes 的真实传感器布局，提供极度拟真的传感器数据和准确的三维占据与密集的深度图标注，并补充了和 nuScenes 数据集等量的长尾自动驾驶场景作为训练、验证和测试集。

本赛道的评测基于 nuScenes OpenOcc 测试集与 LightwheelOcc 测试集，参赛者需要在真实、仿真数据集上同时预测占据栅格与运动估计结果。

## 数据集下载

### nuScenes OpenOcc 数据集

nuScenes 数据集下载请详见 <a href="https://www.nuscenes.org/nuscenes" target="_blank">nuScenes 官方主页</a>。

nuScenes 占据栅格与运动估计训练标签 OpenOcc 下载请详见 <a href="https://github.com/OpenDriveLab/OccNet?tab=readme-ov-file#data" target="_blank"> 文档 </a>。

## LightwheelOcc 光轮占据栅格仿真数据集

请参考本仓库 [Getting Started](/docs/getting_started.md)。

## 评测指标

本赛道使用指标**占据分数**。该指标包含两部分，使用基于射线投影的 **Ray-based mIoU** 进行占据栅格几何和语义的评测，使用平均速度误差 **mAVE** 进行运动估计的评测。详情请见[指标文档](https://github.com/OpenDriveLab/OccNet/tree/challenge?tab=readme-ov-file#evaluation-metrics)。

本赛道最终占据分数为 nuScenes OpenOcc 测试集与 LightwheelOcc 测试集上的加权分数。两数据集的加权系数分别为 0.8 和 0.2。


## 提交指南

参赛者需要按以下步骤将占据栅格预测的结果保存在 `submission.gz` 中。

1. 将`nuScenes OpenOcc val`与`Lightwheel val`上的预测结果保存至本地，格式与占据网络 ground truth 相同。
2. 在本地进行光线投影，保存投影结果。
3. 在本地测试`nuScenes OpenOcc val`与`Lightwheel val`的评测是否符合预期。
4. 将`nuScenes OpenOcc test`集与`Lightwheel test`的预测结果按 1、 2 两步保存、投影，并上传至竞赛服务器。

> 光线投影方法请参考 CVPR AGC 2024 占据栅格与运动估计姊妹竞赛[文档]](https://github.com/OpenDriveLab/OccNet/blob/challenge/docs/getting_started.md#test-submission)，一键运行脚本将很快更新！

最终保存的文件结构为：

```
submission = {
    'method': 'XXXXX',                      <str> -- name of the method
    'team': 'XXXXX',                        <str> -- name of the team, identical to the Google Form
    'authors': ["XXXXX",]                   <list> -- list of str, authors
    'e-mail': "XXXXX",                      <str> -- e-mail address
    'institution / company': "XXXXXXX",     <str> -- institution or company
    'country / region': "zh-CN",            <str> -- country or region, checked by iso3166*
    'results': {
        [token]: {                          <str> -- frame (sample) token
            'pcd_cls'                       <np.ndarray> [N] -- predicted class ID, np.uint8,
            'pcd_dist'                      <np.ndarray> [N] -- predicted depth, np.float16,
            'pcd_flow'                      <np.ndarray> [N, 2] -- predicted flow, np.float16,
        },
        ...
    }
}
```
