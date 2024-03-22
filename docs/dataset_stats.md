# Data Statistics

## Basic Information
| **Type**          | **Info**                            |
|-------------------|-------------------------------------|
| Train             | 28000 frames                        |
| Validation        | 6000 frames                         |
| Test              | 6000 frames                         |
| Number of Cameras | 6                                   |
| Number of Images  | 240,000                             |
| Image resolution  | 1600x900                            |
| Frequency         | 10 Hz                               |
| Voxel Size        | 0.4 m                               |
| Range             | [-40, -40, -1.0, 40, 40, 5.4] m     |
| Classes           | 17                                  |
| Labels            | 3D Occupancy, Flow, and Depth Map   |

### Semantic Classes

The dataset contains 17 classes. The definition of most of the classes is the same as the [nuScenes-lidarseg](https://github.com/nutonomy/nuscenes-devkit/blob/fcc41628d41060b3c1a86928751e5a571d2fc2fa/python-sdk/nuscenes/eval/lidarseg/README.md#classes) dataset.
The `free` category represents voxels that are not occupied by anything. Voxel semantics for each sample frame is given as `[semantics]` in the `.npz` file.

| Index | Class Name           |
|-------|----------------------|
| 0     | car                  |
| 1     | truck                |
| 2     | trailer              |
| 3     | bus                  |
| 4     | construction_vehicle |
| 5     | bicycle              |
| 6     | motorcycle           |
| 7     | pedestrian           |
| 8     | traffic_cone         |
| 9     | barrier              |
| 10    | driveable_surface    |
| 11    | other_flat           |
| 12    | sidewalk             |
| 13    | terrain              |
| 14    | manmade              |
| 15    | vegetation           |
| 16    | free                 |

## Data Distribution
<table>
  <tr>
    <th>Category</th>
    <th>Class</th>
    <th>Distribution</th>
  </tr>
  <tr>
    <td rowspan="3">By Map Type</td>
    <td>Urban</td>
    <td>68%</td>
  </tr>
  <tr>
    <td>Suburbs</td>
    <td>9%</td>
  </tr>
  <tr>
    <td>Freeways</td>
    <td>23%</td>
  </tr>
  <tr>
    <td rowspan="2">By Lighting Conditions</td>
    <td>Daytime</td>
    <td>87%</td>
  </tr>
  <tr>
    <td>Nighttime</td>
    <td>13%</td>
  </tr>
  <tr>
    <td rowspan="3">By Weather Types</td>
    <td>Sunny</td>
    <td>45%</td>
  </tr>
  <tr>
    <td>Overcast</td>
    <td>38%</td>
  </tr>
  <tr>
    <td>Rainy or After Rain</td>
    <td>17%</td>
  </tr>
</table>

## Experiment Results
We conduct experiments to evaluate the domain adaption capbilities between nuScene and LightwheelOcc.
We use [OccNet](https://github.com/OpenDriveLab/OccNet) as a baseline.

| **Train**                 | **Val**                   | **mIoU**       |
|---------------------------|---------------------------|----------------|
| nuScenes                  | nuScenes + LightwheelOcc  |   15.61        |
| nuScenes + LightwheelOcc  | nuScenes + LightwheelOcc  |   28.77        |
| nuScenes                  | nuScenes                  |   24.84        |
| nuScenes + LightwheelOcc  | nuScenes                  |   26.97        |


## Best Practice
Generally, when training data, it is critical to check three key indicators, so as to determine the training method based on their performance:
1. Whether the labels of real data and synthetic data are aligned:  label alignment of synthetic data usually refers to whether the labels are consistent with those in the real-world dataset. If not aligned, the model may not be able to distinguish each category correctly.
2. Whether the number of real data and synthetic data is comparable: The volume of data determines the adequacy of model training. Insufficient data volume may lead to overfitting, while a large amount of data can improve the model's generalization ability.
3. Whether synthetic data is corner case or generic data: The data type (such as corner case or generic data) affects the model's generalization ability and sensitivity to specific situations.

The above issues must be comprehensively considered to ensure the model performance, when selecting training methods and data sampling strategies.
As for LightwheelOcc, the labels have been aligned, and the amount of real data is greater than that of the synthesized data, aiming at solving the corner case problem. Therefore, we recommend using a hybrid training approach, which involves mixing the synthesized and real data for direct training.
