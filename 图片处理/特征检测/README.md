# LineSegmentDetectorModes
```
  cv::LSD_REFINE_NONE = 0, 
  cv::LSD_REFINE_STD = 1, 
  cv::LSD_REFINE_ADV = 2 
```
# Canny算子
```
void cv::Canny (InputArray image, 
                OutputArray edges, 
                double threshold1, 
                double threshold2, 
                int apertureSize = 3, 
                bool L2gradient = false)
                
void cv::Canny (InputArray dx, 
                InputArray dy, 
                OutputArray edges, 
                double threshold1, 
                double threshold2, 
                bool L2gradient = false)
//这个函数用过很多次了，暂时先不继续写代码了，以后再细究
```
# 计算图像块的特征值和特征向量，用于角点检测
```
void cv::cornerEigenValsAndVecs (InputArray src, 
                                 OutputArray dst, 
                                 int blockSize, 
                                 int ksize, 
                                 int borderType = BORDER_DEFAULT)

// 这个函数可以实现对图像的基本数据分析，主要是关于特征值和特征向量
// 输入可以是一个二维单通道图像，输出会是相同尺寸的六通道数据
// 输出的前两个分别储存特征值
// 第三个和第四个储存第一个特征值的特征向量(x1, y1)
// 后面两个储存第二个的特征向量(x2, y2)
// 举个例子
Mat src = imread("D://data//test_contour.png");
imshow("src", src);
cvtColor(src, src, CV_BGR2GRAY);
Mat dst(src.size(), CV_32FC(6));
cornerEigenValsAndVecs(src, dst, 3, 3, BORDER_DEFAULT);
cout << dst.at<Vec6f>(100, 100)[1] << endl;
cout << dst.size() << endl;
cout << dst.depth() << endl;
cout << dst.channels() << endl;
```
# Harris角点检测算子
```
void cv::cornerHarris (InputArray src, 
                       OutputArray dst, 
                       int blockSize, 
                       int ksize, 
                       double k, 
                       int borderType = BORDER_DEFAULT)


```
# 角点最小特征值检测
```
void cv::cornerMinEigenVal (InputArray src, 
                            OutputArray dst, 
                            int blockSize, 
                            int ksize = 3, 
                            int borderType = BORDER_DEFAULT)

```
# 亚像素角点检测
```
void cv::cornerSubPix (InputArray image, 
                       InputOutputArray corners, 
                       Size winSize, 
                       Size zeroZone, 
                       TermCriteria criteria)

```
# 创建线段检测算法
```
Ptr<LineSegmentDetector> cv::createLineSegmentDetector (int _refine = LSD_REFINE_STD, 
                                                        double _scale = 0.8, 
                                                        double _sigma_scale = 0.6, 
                                                        double _quant = 2.0, 
                                                        double _ang_th = 22.5, 
                                                        double _log_eps = 0, 
                                                        double _density_th = 0.7, 
                                                        int _n_bins = 1024)

```
# 强角点检测
```
void cv::goodFeaturesToTrack (InputArray image, 
                              OutputArray corners, 
                              int maxCorners, 
                              double qualityLevel, 
                              double minDistance, 
                              InputArray mask = noArray(), 
                              int blockSize = 3, 
                              bool useHarrisDetector = false, 
                              double k = 0.04)

void cv::goodFeaturesToTrack (InputArray image, 
                              OutputArray corners, 
                              int maxCorners, 
                              double qualityLevel, 
                              double minDistance, 
                              InputArray mask, 
                              int blockSize, 
                              int gradientSize, 
                              bool useHarrisDetector = false, 
                              double k = 0.04)

```
# houge圆检测
```
void cv::HoughCircles (InputArray image, 
                       OutputArray circles, 
                       int method, 
                       double dp, 
                       double minDist, 
                       double param1 = 100, 
                       double param2 = 100, 
                       int minRadius = 0, 
                       int maxRadius = 0)

```
# houge直线检测
```
void cv::HoughLines (InputArray image, 
                     OutputArray lines, 
                     double rho, 
                     double theta, 
                     int threshold, 
                     double srn = 0, 
                     double stn = 0, 
                     double min_theta = 0, 
                     double max_theta = CV_PI)

```
# houge直线概率检测
```
void cv::HoughLinesP (InputArray image, 
                      OutputArray lines, 
                      double rho, 
                      double theta, 
                      int threshold, 
                      double minLineLength = 0, 
                      double maxLineGap = 0)

```
# 利用标准houge转化进行直线拟合
```
void cv::HoughLinesPointSet (InputArray _point, 
                             OutputArray _lines, 
                             int lines_max, 
                             int threshold, 
                             double min_rho, 
                             double max_rho, 
                             double rho_step, 
                             double min_theta, 
                             double max_theta, 
                             double theta_step)

```
# 预角点检测
```
void cv::preCornerDetect (InputArray src, 
                          OutputArray dst, 
                          int ksize, 
                          int borderType = BORDER_DEFAULT)

```
