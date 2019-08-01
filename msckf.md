[TOC]

### MSCKF 学习资料整理

> MSCKF (Multi-State Constraint Kalman Filter)

#### 一、 msckf 简单介绍

	MSCKF (Multi-State Constraint Kalman Filter),从2007年提出至今,一直是filter-based SLAM比较经典的实现.据说这也是谷歌tango里面的算法，这要感谢Mingyang Li博士在MSCKF的不懈工作。在传统的EKF-SLAM框架中，特征点的信息会加入到特征向量和协方差矩阵里,这种方法的缺点是特征点的信息会给一个初始深度和初始协方差，如果不正确的话，极容易导致后面不收敛，出现inconsistent的情况。MSCKF维护一个pose的FIFO，按照时间顺序排列，可以称为滑动窗口，一个特征点在滑动窗口的几个位姿都被观察到的话，就会在这几个位姿间建立约束，从而进行KF的更新。如下图所示, 左边代表的是传统EKF SLAM, 红色五角星是old feature,这个也是保存在状态向量中的,另外状态向量中只保存最新的相机姿态; 中间这张可以表示的是keyframe-based SLAM, 它会保存稀疏的关键帧和它们之间相关联的地图点; 最右边这张则可以代表MSCKF的一个基本结构, MSCKF中老的地图点和滑窗之外的相机姿态是被丢弃的, 它只存了滑窗内部的相机姿态和它们共享的地图点.

#### 二、代码

+ [Kumar实验室版本](https://github.com/KumarRobotics/msckf_vio)
+ [msckf_matlab](https://github.com/yuzhou42/MSCKF)
+ [齐航的python版本](https://github.com/uoip/stereo_msckf)
+ [北理老哥自己实现的](https://github.com/TurtleZhong/msckf_mono)
+ [相机IMU标定](https://github.com/ethz-asl/kalibr)

#### 三、博客资料

+ [VIO综述](https://blog.csdn.net/xiaoxiaowenqiang/article/details/81192045)
+ [泡泡机器人msckf(一)](http://www.sohu.com/a/271224863_715754)
+ [泡泡机器人msckf(二)](http://www.sohu.com/a/271370131_715754)
+ [知乎VIO](https://www.zhihu.com/question/53571648/answer/137726592)
+ [钟心亮的推导和笔记](http://www.xinliang-zhong.com/msckf_notes/)

#### 四、参考论文与书籍

##### 大佬：

+ `Kumar实验室`
+ `MingyangLi`

##### 参考论文：

+ `A Multi-State Constraint Kalman Filter for Vision-aided Inertial Navigation`
+ `High-Precision, Consistent EKF-based Visual-Inertial Odometry`
+ `Robust Stereo Visual Inertial Odometry for Fast Autonomous Flight`
+ `MingyangLi博士论文`

##### 参考资料：

+ `Quaternion kinematics for the error-state Kalman filter`
+ `Indirect Kalman Filter for 3D Attitude Estimation-A Tutorial for Quaternion Algebra`
+ `Consistency Analysis and IMprovement of Vision-aided Inertial Navigation`
+ `On the consistency of Vision-aided Inertial Navigation`
+ `Robust Stereo Visual Inertial Odometry for Fast Autonomous Flight`
+ `Monocular Visual Inertial Odometry on a Mobile Device`
+ `卡尔曼滤波与组合导航原理`

> 注意事项：msckf 里面用的是JPL四元数



#### 五、丘博整理的MSCKF资料

##### MSCKF 相关论文

##### AI Mourikis:

- `2007-ICRA`: A Multi-State Constraint Kalman Filter for Vision-aided Inertial Navigation
- `2012-IROS`: Estimator Initialization in Vision-aided Inertial Navigation with Unknown Camera-IMU Calibration

##### MingyangLi:

- `2012-ICRA`: Improving the Accuracy of EKF-Based Visual-Inertial Odometry 

  用了FEJ进行可观性修正

- `2012-RSS`: Optimization-Based Estimator Design for Vision-Aided Inertial Navigation

  把观测次数长的特征点加入到系统状态中

- `2013-IJRR`: High-Precision, Consistent EKF-based Visual-Inertial Odometry

  MSCKF2.0 1. FEJ引入 2. Sliding Window位姿改成IMU位姿 3. Camera-IMU 外参估计

- `2013-ICRA`: 3-D Motion Estimation and Online Temporal Calibration for Camera-IMU Systems

  加入Td估计,与秦通不同(特征点匀速运动假设), 他的(载体匀速假设).

- `2014-IJRR`: Online Temporal Calibration for Camera-IMU Systems Theory and Aligorithms

- `2014-IJRR`: Vision-aided Inertial Navigation with Rolling-Shutter Cameras

- `2014-ICRA`: High-fidelity Sensor Modeling and Self-Calibration in Vision-aided Inertial Navigation 

  `很强`:star:

  

##### MARS LAB

- `2012-TR`: Observability-constrained Vision-aided Inertial Navigation 

  对初始值比FEJ好.

- `2015-RSS`: A Square Root Inverse Filter for Efficient Vision-aided Inertial Navigation on Mobile Devices `据说ARCore` 减少计算量



##### 其它

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

