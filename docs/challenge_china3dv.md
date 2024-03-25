# China3DV - 占据栅格与运动估计
> 官方网站: :globe_with_meridians: [China3DV](http://www.csig3dv.net/2024/competition.html)  
> 评测服务器: :hugs: [Hugging Face](https://huggingface.co/spaces/China3DV-S/occupancy-and-flow-2024)

## 赛道介绍

三维框往往不足以描述一般物体，受机器人学概念的启发，可将感知表征描述成对栅格化三维空间的占据情况预测。在纯视觉环视相机输入下，参赛者不仅要给出三维空间的栅格化表示，还须给出栅格的运动预测。

本赛道在国际知名数据集 nuScenes 的基础上，引入了行业领先的[光轮智能](http://lightwheel.ai/)自动驾驶模拟器生成的高质量仿真数据。光轮占据栅格仿真数据集（LightwheelOcc）高度重现 nuScenes 的真实传感器布局，提供极度拟真的传感器数据和准确的三维占据与密集的深度图标注，并补充了和 nuScenes 数据集等量的长尾自动驾驶场景作为训练、验证和测试集。

本赛道的评测基于 nuScenes OpenOcc 测试集与 LightwheelOcc 测试集，参赛者需要在真实、仿真数据集上同时预测占据栅格与运动估计结果。

## 数据集下载

### nuScenes OpenOcc 数据集

nuScenes 数据集下载请详见 [nuScenes 官方主页](https://www.nuscenes.org/nuscenes)。

nuScenes 占据栅格与运动估计训练标签 OpenOcc 下载请详见 [文档](https://github.com/OpenDriveLab/OccNet?tab=readme-ov-file#data)。

### LightwheelOcc 光轮占据栅格仿真数据集

请参考本仓库 [Getting Started](/docs/getting_started.md)。

## 评测指标

本赛道使用指标**占据分数**。该指标包含两部分，使用基于射线投影的 **Ray-based mIoU** 进行占据栅格几何和语义的评测，使用平均速度误差 **mAVE** 进行运动估计的评测。详情请见 [RayIoU 指标文档](https://github.com/OpenDriveLab/OccNet/tree/challenge?tab=readme-ov-file#evaluation-metrics)。

本赛道最终占据分数为 nuScenes OpenOcc test 集与 LightwheelOcc test 集上的加权分数。两数据集的加权系数分别为 0.8 和 0.2。

## 提交指南

参赛者需要按以下步骤将占据栅格预测的结果保存在 `submission.gz` 中。

1. 将 `nuScenes OpenOcc val` 与 `Lightwheel val` 上的预测结果保存至本地，格式与占据网络 ground truth 相同。
2. 在本地进行光线投影，保存投影结果。
3. 在本地测试 `nuScenes OpenOcc val` 与 `LightwheelOcc val` 的评测是否符合预期。
4. 将 `nuScenes OpenOcc test` 集与 `Lightwheel test` 的预测结果按 1、 2 两步保存、投影，并上传至竞赛服务器。

> 光线投影脚本请参照 [ray_casting.py](/tools/ray_iou/ray_casting.py)。  
> 生成 `LightwheelOcc val` GT 的命令如下。可以通过修改 `--data-root` 参数来生成预测结果的 `.gz` 文件。
``` bash
cd tools/ray_iou
python ray_casting.py \
    --dataset-type lightwheelocc \
    --data-root ../../data/lightwheelocc \
    --data-info ../../data/lightwheelocc/lightwheel_occ_infos_val.pkl \
    --output-dir ./output
```
> 生成 `nuScenes OpenOcc val` GT 的命令如下。
``` bash
python ray_casting.py \
    --dataset-type openocc_v2 \
    --data-root ../../data/nuscenes \
    --data-info ../../data/nuscenes/nuscenes_infos_val_occ.pkl \
    --output-dir ./output
```
> 对于预测结果，请分别生成两个数据集的预测结果并手动将 `submission['results']` 字典合并。


最终保存的文件结构为：

```
submission = {
    'method': '',                           <str> -- name of the method
    'team': '',                             <str> -- name of the team, identical to the Google Form
    'authors': ['']                         <list> -- list of str, authors
    'e-mail': '',                           <str> -- e-mail address
    'institution / company': '',            <str> -- institution or company
    'country / region': 'zh-CN',            <str> -- country or region, checked by iso3166*
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
