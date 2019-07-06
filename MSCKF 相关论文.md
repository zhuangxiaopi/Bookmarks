#### 之前整理的资料

[msckf](./msckf.md)

#### MSCKF 相关论文

##### AI Mourikis:

+ `2007-ICRA`: A Multi-State Constraint Kalman Filter for Vision-aided Inertial Navigation
+ `2012-IROS`: Estimator Initialization in Vision-aided Inertial Navigation with Unknown Camera-IMU Calibration

##### MingyangLi:

+ `2012-ICRA`: Improving the Accuracy of EKF-Based Visual-Inertial Odometry 

  用了FEJ进行可观性修正

+ `2012-RSS`: Optimization-Based Estimator Design for Vision-Aided Inertial Navigation

  把观测次数长的特征点加入到系统状态中

+ `2013-IJRR`: High-Precision, Consistent EKF-based Visual-Inertial Odometry

  MSCKF2.0 1. FEJ引入 2. Sliding Window位姿改成IMU位姿 3. Camera-IMU 外参估计

+ `2013-ICRA`: 3-D Motion Estimation and Online Temporal Calibration for Camera-IMU Systems

  加入Td估计,与秦通不同(特征点匀速运动假设), 他的(载体匀速假设).

+ `2014-IJRR`: Online Temporal Calibration for Camera-IMU Systems Theory and Aligorithms

+ `2014-IJRR`: Vision-aided Inertial Navigation with Rolling-Shutter Cameras

+ `2014-ICRA`: High-fidelity Sensor Modeling and Self-Calibration in Vision-aided Inertial Navigation 

  `很强`:star:

  
#### MARS LAB
+ `2012-TR`: Observability-constrained Vision-aided Inertial Navigation 

  对初始值比FEJ好.

+ `2015-RSS`: A Square Root Inverse Filter for Efficient Vision-aided Inertial Navigation on Mobile Devices `据说ARCore` 减少计算量




#### 其它

##### TUM

`2014-Master's Thesis`: Monocular Visual Inertial Odometry on Mobile Devices 

##### PL-MSCKF

`2018-IROS` Trifo-VIO: Robust and Efficient Stereo Visual Inertial Odometry using Points and Lines

加入线特征和回环

##### MSCKF-VIO

`2018-ICRA`: Robust Stereo Visual Inertial Odometry for Fast Autonomous Flight

双目OCKF

##### R-MSCKF

`2018-IROS`: Robocentric Visual-Inertial Odometry  

`R-VIO`分析可观性,还有一个施密特EKF

##### MSCKF-Mono

无论文





