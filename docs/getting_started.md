# Getting Started
## Download Data

The dataset files can be downloaded via [OpenDriveLab](https://openxlab.org.cn/datasets/OpenDriveLab/LightwheelOcc) and :hugs: [Hugging Face](https://huggingface.co/datasets/OpenDriveLab/LightwheelOcc/tree/main/lightwheelocc-v1.0).

For now, the **depth** label is only available on Hugging Face!

## Prepare Dataset

After download all the files and put them into `lidatwheelocc/`. Please uncompress all the `.tar.gz` files to set up the dataset!

### Hierarchy

The hierarchy of folder `lightwheelocc/` is described below:

```
lightwheelocc
├── samples
│   ├── CAM_FRONT
│   │   ├── [scene_token]
│   │   │   ├── [timestamp].jpeg
│   │   │   └── ...
│   │   └── ...
│   └── ...
├── depth
│   ├── CAM_FRONT
│   │   ├── [scene_token]
│   │   │   ├── [timestamp].png
│   │   │   └── ...
│   │   └── ...
│   └── ...
├── occupancy
│   ├── [scene_token]
│   │   │   ├── [timestamp].npz
│   │   │   └── ...
│   │   └── ...
│   └── ...
├── lightwheel_occ_infos_train.pkl
├── lightwheel_occ_infos_val.pkl
└── lightwheel_occ_infos_test.pkl
```

- `[scene_token]` specifies a sequence of frames, and `[timestamp]` specifies a single frame in a sequence.
- `samples/` contains images captured by various cameras.
- `depth/` contains depth map of each sample. 
- `occupancy/` contains semantics and flow label for each frame.
- `lightwheel_occ_infos_{train/val/test}.pkl` contains metadata of the dataset.

### Meta Data

The pickle files contain metadata of the dataset.
Each pickle file is formatted as follows:

```
{
  "metadata": {                                 <dict> -- meta infos of dataset.
    "version":                                  <str> -- version of lightwheelocc dataset.
    "split":                                    <str> -- split, {train/val/test}.
  }
  "infos" [                                     <list> -- meta infos of the scenes 
    "token":                                    <str> -- token of current frame, unique by sample
    "prev":                                     <str> -- frame token of the previous keyframe in the scene
    "next":                                     <str> -- frame token of the next keyframe in the scene
    "timestamp":                                <str> -- timestamp, unique by sample
    "ego2global_rotation"                       <float> [4] -- ego to global coordinate system orientation as quaternion
    "ego2global_translation"                    <float> [3] -- ego to global coordinate system translation
    "lidar2ego_rotation"                        <float> [4] -- lidar to ego coordinate system orientation as quaternion
    "lidar2ego_translation"                     <float> [3] -- lidar to ego coordinate system translation
    "cams": {                                   <dict> -- meta infos of the camera sensor
      [cam_type]: {                             <str> -- type of cameras.
        "cam_path":                             <str> -- corresponding image file path, *.jpeg
        "depth_path":                           <str> -- corresponding deth file path, *.png
        "cam_intrinsic":                        <float> [3, 3] -- intrinsic camera calibration
        "sensor2ego_rotation"                   <float> [4] -- sensor to ego coordinate system orientation as quaternion
        "sensor2ego_translation"                <float> [3] -- sensor to ego coordinate system translation
        "sensor2lidar_rotation"                 <float> [4] -- sensor to lidar coordinate system orientation as quaternion
        "sensor2lidar_translation"              <float> [3] -- sensor to lidar coordinate system translation
      },
      ...
    },
    "occ_path":                                 <str> -- corresponding 3D voxel gt path, *.npz
  }
}
```

- The filepath is the relative path to `lightwheelocc`.
- The occupancy label is in `ego` coordinate system.
- The `ego` and `lidar` coordinate system is the same. We keep those keys for better compatibility.
- You can refer to [toturial.ipynb](toturial.ipynb) for the using of depth and occupancy label.
