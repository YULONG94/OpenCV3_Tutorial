[官方文档入口](https://docs.opencv.org/3.4.2/dc/d6b/group__video__track.html)
# 有哪些
```
class cv::DenseOpticalFlow
class cv::DualTVL1OpticalFlow
class cv::FarnebackOpticalFlow
class cv::KalmanFilter
class cv::SparseOpticalFlow
class cv::SparsePyrLKOpticalFlow
```
# 如何创建
```
static Ptr< DualTVL1OpticalFlow > create (double tau=0.25, 
                                          double lambda=0.15, 
                                          double theta=0.3, 
                                          int nscales=5, 
                                          int warps=5, 
                                          double epsilon=0.01, 
                                          int innnerIterations=30, 
                                          int outerIterations=10, 
                                          double scaleStep=0.8, 
                                          double gamma=0.0, 
                                          int medianFiltering=5, 
                                          bool useInitialFlow=false)

static Ptr< FarnebackOpticalFlow > create (int numLevels=5, 
                                           double pyrScale=0.5, 
                                           bool fastPyramids=false, 
                                           int winSize=13, 
                                           int numIters=10,
                                           int polyN=5, 
                                           double polySigma=1.1, 
                                           int flags=0)

// 还是那句话，这个貌似很长时间都用不到，所以暂时不学，而且貌似官方文档的介绍也不全，待检查是否只是文档的版本问题
```
