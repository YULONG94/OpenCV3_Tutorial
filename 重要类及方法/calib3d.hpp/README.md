[官方文档入口](https://docs.opencv.org/3.4.2/d9/dba/classcv_1_1StereoBM.html)
# 有哪些类
```
class cv::StereoBM
class cv::StereoMatcher
class cv::StereoSGBM
```
# how to create
```
static Ptr<StereoBM> cv::StereoBM::create (int numDisparities = 0, 
                                           int blockSize = 21)

static Ptr< StereoSGBM > create (int minDisparity=0, 
                                 int numDisparities=16, 
                                 int blockSize=3, 
                                 int P1=0, 
                                 int P2=0, 
                                 int disp12MaxDiff=0, 
                                 int preFilterCap=0, 
                                 int uniquenessRatio=0, 
                                 int speckleWindowSize=0, 
                                 int speckleRange=0, 
                                 int mode=StereoSGBM::MODE_SGBM)
// 关于三维重建的类，前面记录了一些基本函数，猜测这里是两个更加高层的集成算法，以后再研究
```
