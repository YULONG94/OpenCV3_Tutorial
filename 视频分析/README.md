[官方文档入口](https://docs.opencv.org/3.4.2/d7/de9/group__video.html)
> 细想了一下，这个功能暂时不需要学，所以就先放着吧（心里清楚这是在偷懒）
# 运动分析
```
// 这个类函数不仅可以作用在视频格式上，也可以对摄像头传来的视频流进行处理
// 工作原理是使用前面很少的图像找到背景
Ptr<BackgroundSubtractorKNN> cv::createBackgroundSubtractorKNN (int history = 500, 
                                                                double dist2Threshold = 400.0, 
                                                                bool detectShadows = true)

Ptr<BackgroundSubtractorMOG2> cv::createBackgroundSubtractorMOG2 (int history = 500, 
                                                                  double varThreshold = 16, 
                                                                  bool detectShadows = true)
// 举个例子
VideoCapture cap;
cap.open("../data/vtest.avi");
// cap.open("D://data//1_1.avi");
Ptr<BackgroundSubtractorMOG2> bgsubtractor = createBackgroundSubtractorMOG2(500, 16, false);
Ptr<BackgroundSubtractorKNN> bgsubtractor_knn = createBackgroundSubtractorKNN(500, 400, false);
Mat frame, bgmask_mog2, bgmask_knn;
namedWindow("src");
namedWindow("segm_mog2");
namedWindow("segm_knn");
while (1)
{
	cap >> frame;
	if (frame.empty())
		break;
	bgsubtractor_knn->apply(frame, bgmask_mog2);
	bgsubtractor_knn->apply(frame, bgmask_knn);
	imshow("src", frame);
	imshow("segm_mog2", bgmask_mog2);
	imshow("segm_knn", bgmask_knn);
	if (waitKey(10)>=0)
		break;
}
cap.release();
```
# 物体跟踪
# 光流金字塔搭建
```
int cv::buildOpticalFlowPyramid (InputArray img, 
				 OutputArrayOfArrays pyramid, 
				 Size winSize, 
				 int maxLevel, 
				 bool withDerivatives = true, 
				 int pyrBorder = BORDER_REFLECT_101, 
				 int derivBorder = BORDER_CONSTANT, 
				 bool tryReuseInputImage = true)
```
# 光流金字塔计算
```
void cv::calcOpticalFlowFarneback (InputArray prev, 
				   InputArray next, 
				   InputOutputArray flow, 
				   double pyr_scale, 
				   int levels, 
				   int winsize, 
				   int iterations, 
				   int poly_n, 
				   double poly_sigma, 
				   int flags)
```
# LK（Lukas-Kanade）光流金字塔计算
```
void cv::calcOpticalFlowPyrLK (InputArray prevImg, 
			       InputArray nextImg, 
			       InputArray prevPts, 
			       InputOutputArray nextPts, 
			       OutputArray status, 
			       OutputArray err, 
			       Size winSize = Size(21, 21), 
			       int maxLevel = 3, 
			       TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, 0.01), 
			       int flags = 0, 
			       double minEigThreshold = 1e-4)
```
# MeanShift算法
```
int cv::meanShift (InputArray probImage, 
		   Rect & window, 
		   TermCriteria criteria)
```
# 连续自适应的MeanShift算法
```
RotatedRect cv::CamShift (InputArray probImage, 
			  Rect & window, 
			  TermCriteria criteria)
```
# createOptFlow_DualTVL1
```
Ptr<DualTVL1OpticalFlow> cv::createOptFlow_DualTVL1 ()
```
# estimateRigidTransform
```
Mat cv::estimateRigidTransform (InputArray src, 
				InputArray dst, 
				bool fullAffine )

Mat cv::estimateRigidTransform (InputArray src, 
				InputArray dst, 
				bool fullAffine, 
				int ransacMaxIters, 
				double ransacGoodRatio, 
				int ransacSize0)

```
# findTransformECC
```
double cv::findTransformECC (InputArray templateImage, 
			     InputArray inputImage, 
			     InputOutputArray warpMatrix, 
			     int motionType = MOTION_AFFINE, 
			     TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 50, 0.001), 
			     InputArray inputMask = noArray())

```
