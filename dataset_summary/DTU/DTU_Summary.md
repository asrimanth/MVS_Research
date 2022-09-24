# DTU Summary

**Original Paper: [Paper](https://roboimagedata2.compute.dtu.dk/data/text/multiViewCVPR2014.pdf)**

**Link to Download: [DTU dataset link.](https://roboimagedata.compute.dtu.dk/?page_id=36)**

DTU is a dataset which is primarily used to solve the multi-view stereo problem. The dataset has been open on the web since 2014.
The structure of the dataset is as follows:

```{bash}
DTU
├── Cameras
│   └── train
├── Depths
│   ├── scan*_train
├── Depths_raw
│   ├── scan*
│   └── scan*_train
├── Rectified
│   ├── scan*
│   └── scan*_train
├── SampleSet
│   ├── Calibration
│   │   └── cal18
│   ├── Cleaned
│   │   ├── scan1
│   │   └── scan6
│   ├── ObsMask
│   ├── Points
│   │   ├── camp
│   │   ├── furu
│   │   ├── stl
│   │   └── tola
│   ├── Rectified
│   │   ├── scan1
│   │   └── scan6
│   └── Surfaces
│       ├── camp
│       ├── furu
│       └── tola
└── Testing
    ├── scan*
    │   ├── cams
    │   └── images

700 directories
```

## Description

The folders Cameras, Depths, Depths_raw and Rectified are typically used during training.
The files train.txt, valid.txt and test.txt contain the indexed scan folders for training, validation, and testing.
The dataset itself has 80 scans, and each scan has 49-64 views of the same object in different angles and 7 lighting conditions.
The 80 scenes contain different number of camera positions. 59 scenes contain 49 camera positions and 21 scenes contain 64 camera positions.

### Cameras

The cameras folder contains intrinsic and extrinsic parameters of the camera for every view. Hence, there are 49 .txt files (one for each view) in the training set. It also stores the pair.txt file.

#### Camera Intrinsic and Extrinsic Parameters

A <view_number>.txt file has the following format:

```{txt}
extrinsic
0.970263 0.00747983 0.241939 -191.02
-0.0147429 0.999493 0.0282234 3.28832
-0.241605 -0.030951 0.969881 22.5401
0.0 0.0 0.0 1.0

intrinsic
361.54125 0.0 82.900625
0.0 360.3975 66.383875
0.0 0.0 1.0

425.0 2.5
```

The first line represents the nature of the camera parameter. Usually, it's extrinsic followed by intrinsic.
The extrinsic parameters are encoded in a 4X4 matrix. The intrinsic parameters are then encoded into a 3X3 matrix.
The last line contains the depth interval data. Depth intervals are calculated by 2 numbers: the start and the step.
Once each view has been applied with its own homography matrix, all of the views look the same.

In the Camera folder, there is also a pair.txt file which stores the following information:

- For every "reference" view, there are a set of "source" views (~10) that complement the reference view in terms of positioning and lighting.
- The indices of source views are stored as alternative integers in a row of source view string (alternate lines in pair.txt file).

### Depths

Depth map encodes the relative distance between two objects in an image.
The closer an object is, the higher it's value in the depth matrix.
Hence, the nearest objects are seen as white and the farthest objects are encoded in black.
Any object in between is encoded in a shade of gray, depending on the distance.
For every scan, there is a single depth map, in .pfm (for floating point precision) as well as .png.
Image resolution: 512x640. This resolution is obtained by center cropping and downsampling the corresponding depth_raw image.

### Depths Raw

Depths\_raw stores the same information as Depths, except that it stores depths in the resolution 1200x1600.

### Rectified

The folder which stores the scans and views. It contains 2 types of folders: scan\* and scan\*\_train. The main difference between them is that the former has views of resolution 512x640 (same as depths), while the latter has the resolution 1200x1600 (same as depths_raw).

## Typical MVS workflow

A PyTorch DataLoader can be found [here.](https://github.com/xy-guo/MVSNet_pytorch/blob/master/datasets/dtu_yao.py)
