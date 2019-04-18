[官方文档入口](https://docs.opencv.org/3.4.2/da/d9b/group__features2d.html)

貌似这些函数的学习资源太少，可能要以后花多点时间自己去研究了

# 计算RP（recall-precision）曲线
```
void cv::computeRecallPrecisionCurve (const std::vector< std::vector< DMatch > > & matches1to2, 
                                      const std::vector< std::vector< uchar > > & correctMatches1to2Mask, 
                                      std::vector< Point2f > & recallPrecisionCurve)

```
# evaluateFeatureDetector
```
void cv::evaluateFeatureDetector (const Mat & img1, 
                                  const Mat & img2, 
                                  const Mat & H1to2, 
                                  std::vector< KeyPoint > * keypoints1, 
                                  std::vector< KeyPoint > * keypoints2, 
                                  float & repeatability, 
                                  int & correspCount, 
                                  const Ptr< FeatureDetector > & fdetector = Ptr< FeatureDetector >())

```
# getNearestPoint
```
int cv::getNearestPoint (const std::vector< Point2f > & recallPrecisionCurve, 
                         float l_precision)

```
# getRecall
```
float cv::getRecall (const std::vector< Point2f > & recallPrecisionCurve, 
                     float l_precision)

```
# 创建面部识别的检测算子
```
Ptr<BaseCascadeClassifier::MaskGenerator> cv::createFaceDetectionMaskGenerator ()
```
# 二维码检测算子
```
bool cv::detectQRCode (InputArray in, 
                       std::vector< Point > & points, 
                       double eps_x = 0.2, 
                       double eps_y = 0.1)
# 很遗憾的发现，这个函数在后续版本被删除了，而且我再这个版本也没实验成功
```
# 矩形检测框合并
```
void cv::groupRectangles (std::vector< Rect > & rectList, 
                          int groupThreshold, 
                          double eps = 0.2)

void cv::groupRectangles (std::vector< Rect > & rectList, 
                          std::vector< int > & weights, 
                          int groupThreshold, 
                          double eps = 0.2)

void cv::groupRectangles (std::vector< Rect > & rectList, 
                          int groupThreshold, 
                          double eps, 
                          std::vector< int > * weights, 
                          std::vector< double > * levelWeights)

void cv::groupRectangles (std::vector< Rect > & rectList, 
                          std::vector< int > & rejectLevels, 
                          std::vector< double > & levelWeights, 
                          int groupThreshold, 
                          double eps = 0.2)
// 应该主要是为了合并一些多余的框，排除一些覆盖率很大（其实应该是一个框）的框
```
