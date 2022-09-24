# MVSNet Summary

MVSNet was one of the first end-to-end deep-learning based MVS method. In 2018, it performed much better in terms of overall score, compared to the previous methods. The training set aims to reconstruct the depth map while test set is evaluated on point clouds.

## Network Structure

The network structure mainly comprises of the following parts: Feature extraction, differential Homography, Cost Volume Regularization and Depth map refinement.

### Feature Extraction

The feature extractor is 8 2D CNNs in series, where the strides of layer 3 and 6 are set to two to divide the feature towers into three scales.
Within each scale, two convolutional layers are applied to extract the higher-level image representation. Each convolutional layer is followed by a batch-normalization (BN) layer and a rectified linear unit (ReLU) except for the last layer.
Also, similar to common matching tasks, parameters are shared among all feature towers for efficient learning.
