[TOC]

> **数学学不好，demo跑到老!--高博**

### slam & vio 学习资料整理

#### 一、大佬们的综述

##### 知乎

+ [有哪些开源项目是关于单目+imu做slam的?](https://www.zhihu.com/question/53571648/answer/137585634)
+ [游振兴写的SLAM/VIO学习总结](https://zhuanlan.zhihu.com/p/34995102)

#### 二、 书籍与课程

##### 必读的书和讲义

+ `State Estimation for Robotics`
+ `Multiview Geometry for Computer Vision`
+ `Quaternion kinematics for the error-state Kalman filter`
+ [Course on SLAM](https://www.iri.upc.edu/people/jsola/JoanSola/objectes/toolbox/courseSLAM.pdf)：`Joan Sola`
+ [Factor Graphs for Robot Perception](https://www.ri.cmu.edu/wp-content/uploads/2018/05/Dellaert17fnt.pdf)]
+ [Probabilistic Robotics](http://www.probabilistic-robotics.org/) 

##### 课程

+ `Course on SLAM`：`Joan Sola`

+ `Robotics Lecture Course`:`Andrew Davison`


##### 相关算法参考资料

`PoseGraph`：

+ Grisetti. 《A Tutorial on Graph-Based SLAM》
+ University of Alberta 很棒的机器人课程 https://webdocs.cs.ualberta.ca/~zhang/c631/主要以作业为主，例子来源于该课程。
+ Strasdat. 《Visual SLAM: Why Filter?》
+ Grisetti. 课件 《SLAM Back-end》（可以直接搜到）
+ Rainer & Grisetti  《g2o: A General Framework for Graph Optimization》

`MarginSchur&Complement`：

+ 《 g2o: A General Framework for Graph Optimization 》 
+ 《 Sliding Window Filter with Application to Planetary Landing 》 
+ 《 Keyframe-Based Visual-Inertial SLAM Using Nonlinear Optimization 》ijrr 2014 这个详细些 
+ 《 Direct Sparse Odometry 》 
+ 《Analysis and Improvement of the Consistency of Extended Kalman Filter based SLAM》 
+ 《Motion Tracking with Fixed-lag Smoothing: Algorithm and Consistency Analysis》
+ 《Decoupled, Consistent Node Removal and　Edge Sparsification for Graph-based SLAM》

`DSO相关`：

+ [1]. Engel J, Koltun V, Cremers D. Direct sparse odometry[J]. IEEE Transactions on Pattern Analysis and Machine Intelligence, 2017.
+ [2]. Forster, C.; Pizzoli, M. & Scaramuzza, D., SVO: Fast semi-direct monocular visual odometry,Robotics and Automation (ICRA), 2014 IEEE International Conference on,2014, 15-22.
+ [3]. Mur-Artal, R.; Montiel, J. & Tardós, J. D. ORB-slam: a versatile and accurate monocular slam system,IEEE Transactions on Robotics, IEEE,2015, 31, 1147-1163.
+ [4]. Bowman, S. L.; Atanasov, N.; Daniilidis, K. & Pappas, G. J. Probabilistic data association for semantic SLAMRobotics and Automation (ICRA), 2017 IEEE International Conference on,2017, 1722-1729.
+ [5]. Civera, J.; Davison, A. J. & Montiel, J. M. Inverse depth parametrization for monocular SLAM,IEEE transactions on robotics, IEEE,2008, 24, 932-945.
+ [6]. Engel, J.; Sturm, J. & Cremers, D. Semi-dense visual odometry for a monocular cameraProceedings of the IEEE international conference on computer vision,2013, 1449-1456
+ [7]. Leutenegger, S.; Lynen, S.; Bosse, M.; Siegwart, R. & Furgale, P. Keyframe-based visual-inertial odometry using nonlinear optimizationINTERNATIONAL JOURNAL OF ROBOTICS RESEARCH,2015, 34:314-334
+ [8]. Barfoot, T. State Estimation for Robotics: A Matrix Lie Group Approach,Cambridge University Press,2017
+ [9]. SLAM中的marginalization 和 Schur complement
+ [10]. DSO之光度标定 - 路游侠 - 博客园
+ [11]dso详解 :https://zhuanlan.zhihu.com/p/29177540

`SVO相关`：

+ [高翔](https://www.zhihu.com/people/gao-xiang-24-90/answers)
+ [冯兵](http://fengbing.net/)
+ [刘浩敏](https://www.bilibili.com/video/av5934066/) 

#### 三、技术博客

##### 相关算法资源网站
+ [EuclideanSpace](http://www.euclideanspace.com/)

##### IMU建模与分析
+ [游振兴](https://www.cnblogs.com/youzx/p/6291327.html?utm_source=itdadao&utm_medium=referral)
+ [Kalibr_wiki](https://github.com/ethz-asl/kalibr/wiki/IMU-Noise-Model)
+ [GuassWhiteNoise](https://blog.csdn.net/ZSZ_shsf/article/details/46914853)
+ [泡泡机器人IMU状态模型(一)](http://mp.weixin.qq.com/s/PD4cOqVE3oMhyW4A2N02xQ)
+ [泡泡机器人IMU状态模型(二)](http://mp.weixin.qq.com/s/_ElpcSkMaGEIFd3bmwGa_Q)
+ [相机IMU融合](http://www.likecs.com/show-25370.html)
##### 预积分

+ [高翔Momenta](https://zhuanlan.zhihu.com/p/36323177)

##### OKVIS

+ [付兴银公式推导](https://blog.csdn.net/fuxingyin/article/list/1?t=1)

##### VINS

+ [vins中的Marginalization](https://zhuanlan.zhihu.com/p/51330624)
+ [vins中的协方差矩阵](https://www.zhihu.com/question/64381223/answer/255818747)
+ [vins公式推导](https://blog.csdn.net/wangshuailpp/article/category/6949052)

##### 个人博客主页

+ [贺一佳](https://me.csdn.net/heyijia0327)：`巨强`
+ [郑帆](https://fzheng.me/cn/)：`OKVIS` `IMU` 
+ [徐尚](https://www.cnblogs.com/shang-slam/)：`VINS` `ORBSLAM`
+ [hitcm](http://www.cnblogs.com/hitcm/) ：`VIO` `LSD`
+ [游振兴](https://www.cnblogs.com/luyb/tag/SLAM/)：`VIO` `SVO`
+ [钟心亮](http://www.xinliang-zhong.com/)：`MSCKF` `VIO`
+ [极品巧克力](https://www.cnblogs.com/ilekoaiq)：`SVO` `VIO` `DepthFilter`
+ [刘枭](http://www.liuxiao.org/)：`slam` `DeepLearning` `AR`

##### 发展规划相关

+ [机器人工程师学习之路](https://zhuanlan.zhihu.com/p/22266788)

##### 其它

+ [VIO BA推导](https://www.cnblogs.com/112358nizhipeng/p/9057943.html)
+ [VI ORB公式推导](https://blog.csdn.net/myarrow/article/details/54694472)
+ [基础知识 LSD](https://blog.csdn.net/kokerf)

#### 四、论文资料

+ [贺一佳论文List](https://blog.csdn.net/heyijia0327/article/details/82855443)
+ `Visual-Inertial Monocular SLAM with Map Reuse`：`ORB` `VIO初始化` 
+ `On-Manifold Preintegration for Real-Time  Visual-Inertial Odometry`：`预积分` `GTSAM`
+ `IMU Preintegration on Manifold for Efficient Visual-Inertial Maximum-a-Posteriori Estimation`：`预积分`
+ `Visual-Inertial Monocular SLAM with Map Reuse`：`ORB+IMU`
+ `A Comparative Analysis of Tightly-coupled Monocular, Binocular, and Stereo VINS`：`紧耦合分析` `综述`
+ `A Benchmark Comparison of Monocular Visual-Inertial Odometry Algorithms for Flying Robots `：`BenchMark` 
+ `Keyframe-based visual–inertial odometry`：`okvis`
+ `Robust Visual Inertial Odometry Using a Direct EKF-Based Approach`：`rovio`

#### 五、代码学习

+ [ROVIO](https://github.com/ethz-asl/rovio)：`单目` `IMU` `EKF`
+ [OKVIS](https://github.com/ethz-asl/okvis_ros)：`单目` `IMU` `非线性优化`
+ [ORBSLAM+IMU](https://github.com/JzHuai0108/ORB_SLAM)：`立体相机`
+ [R-VIO](https://github.com/rpng/R-VIO):`Guoquan Huang` `IJRR`

#### 六、实验室主页

[MARS](http://mars.cs.umn.edu/)：`明尼苏达大学` `VINS`

#### 六、有趣的工作

##### [Deep Virtual Stereo Odometry (DVSO)](https://vision.in.tum.de/research/vslam/dvso)

+ 一个semi-supervised的单目深度预测网络(StackNet)，在KITTI数据集上达到了sota的结果，并在Cityscapes、Make3D数据集上取得了良好的generalization
+ DVSO将StackNet预测的深度图通过virtual stereo term紧耦合入单目DSO的优化框架中，并最终在KITTI上达到了超越sota双目VO/SLAM的performance 

 

 
