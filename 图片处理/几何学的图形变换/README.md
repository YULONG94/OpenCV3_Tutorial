> cv::InterpolationFlags
```
cv::INTER_NEAREST = 0, 
cv::INTER_LINEAR = 1, 
cv::INTER_CUBIC = 2, 
cv::INTER_AREA = 3, 
cv::INTER_LANCZOS4 = 4, 
cv::INTER_LINEAR_EXACT = 5, 
cv::INTER_MAX = 7, 
cv::WARP_FILL_OUTLIERS = 8, 
cv::WARP_INVERSE_MAP = 16,
```
> cv::InterpolationMasks
```
cv::INTER_BITS = 5, 
cv::INTER_BITS2 = INTER_BITS * 2, 
cv::INTER_TAB_SIZE = 1 << INTER_BITS, 
cv::INTER_TAB_SIZE2 = INTER_TAB_SIZE * INTER_TAB_SIZE,
```
> cv::WarpPolarMode
```
cv::WARP_POLAR_LINEAR = 0, 
cv::WARP_POLAR_LOG = 256,
```
# 仿射图形式转化函数
```
void cv::convertMaps (InputArray map1, 
                      InputArray map2, 
                      OutputArray dstmap1, 
                      OutputArray dstmap2, 
                      int dstmap1type, 
                      bool nninterpolation = false)
用来将map1和map2转化成另一种形式，有利于remap函数的加速
常有的形式是：(CV_32FC1, CV_32FC1)→(CV_16SC2, CV_16UC1)
可以看出转化前，map的形式是float，二转化之后官方称之为more compact and much faster的fixed-point表现形式
//例如：
Mat float_distortion_map_x(20, 10, CV_32FC1);
Mat float_distortion_map_y(20, 10, CV_32FC1);
Mat distortion_map_1_x = Mat(20, 10, CV_16SC2);
Mat distortion_map_1_y = Mat(20, 10, CV_16UC1);
for (int x = 0; x<20; ++x)
	for (int y = 0; y < 10; ++y)
	{
		{
			float_distortion_map_x.at<float>(x, y) = float(x);
			float_distortion_map_y.at<float>(x, y) = float(y);
		}
	}
convertMaps(float_distortion_map_x, float_distortion_map_y, distortion_map_1_x, distortion_map_1_y, CV_16SC2, false);
cout << "原mapx"<<float_distortion_map_x << endl;
cout << "原mapy"<<float_distortion_map_y << endl;
cout << "结果map1" << distortion_map_1_x << endl;
cout << "结果map2" << distortion_map_1_y << endl;

然后就可以把结果的两个map用在remap函数中了，用在remap函数中的情况见相关的笔记部分即可
```
# 仿射转化矩阵
```
Mat cv::getAffineTransform (const Point2f src[], 
			    const Point2f dst[])
// 根据输入的两组三个点，来计算相关的仿射转化矩阵
// 举个例子：
Mat src = imread("lol9.jpg");
Mat dst_warp, dst_warpRotateScale;
Point2f srcPoints[3];//原图中的三点  
Point2f dstPoints[3];//目标图中的三点  

//第一种仿射变换的调用方式：三点法
//三个点对的值,上面也说了，只要知道你想要变换后图的三个点的坐标，就可以实现仿射变换  
srcPoints[0] = Point2f(0, 0);
srcPoints[1] = Point2f(0, src.rows - 1);
srcPoints[2] = Point2f(src.cols - 1, 0);
//映射后的三个坐标值
dstPoints[0] = Point2f(0, src.rows*0.3);
dstPoints[1] = Point2f(src.cols*0.25, src.rows*0.75);
dstPoints[2] = Point2f(src.cols*0.75, src.rows*0.25);

Mat M1 = getAffineTransform(srcPoints, dstPoints);//由三个点对计算变换矩阵  
warpAffine(src, dst_warp, M1, src.size());//仿射变换  

imshow("原始图", src);
imshow("仿射变换1", dst_warp);
waitKey(0);
```
# 旋转转化矩阵
```
Mat cv::getRotationMatrix2D (Point2f center, 
			     double angle, 
			     double scale)

// 举个例子：
Mat src = imread("lol9.jpg");
Mat dst_warp, dst_warpRotateScale;

//第二种仿射变换的调用方式：直接指定角度和比例                                          
//旋转加缩放  
Point2f center(src.cols / 2, src.rows / 2);//旋转中心  
double angle = 45;//逆时针旋转45度  
double scale = 0.5;//缩放比例  

Mat M2 = getRotationMatrix2D(center, angle, scale);//计算旋转加缩放的变换矩阵  
warpAffine(dst_warp, dst_warpRotateScale, M2, src.size());//仿射变换  

imshow("原始图", src);
imshow("仿射变换2", dst_warpRotateScale);
waitKey(0);
```
# 透视转化矩阵
```
Mat cv::getPerspectiveTransform (const Point2f src[], 
			    	 const Point2f dst[])
与仿射转化矩阵相同，但是这里的点的集合是四个，得到的转化矩阵可以用于透视变化warpPerspective()
```
# 获得特定的矩形区域
```
void cv::getRectSubPix (InputArray image, 
			Size patchSize, 
			Point2f center, 
			OutputArray patch, 
			int patchType = -1)
// 使用这种方式获得的Roi不仅仅拷贝的指针头，类似于clone和copyTo，就是获得的数据和源完全独立
// 需要注意的是最好选择的pathSize为奇数，否则计算的时候会发生你没想到是情况，我验证的结果是做了一些平均操作
// 而且对于超过边界的情况，会先通过等边界补齐再做处理。
// 例子：
Mat src = Mat::ones(10, 10, CV_8UC1);
Mat result;
uchar *p = src.ptr<uchar>(0, 0);
*p = 15;
uchar *p1 = src.ptr<uchar>(1, 1);
*p1 = 225;
uchar *p2 = src.ptr<uchar>(0, 1);
*p2 = 225;
getRectSubPix(src, Size(4, 4), Point2d(1, 1), result, -1);

cout << src << endl;
cout << result << endl;
```
> 关于相机的一些处理，有以下几个函数，这里先存着，以后分析
# getDefaultNewCameraMatrix()
# initUndistortRectifyMap()
# initWideAngleProjMap()
# undistort()
# undistortPoints()
# 逆仿射矩阵的获得
```
void cv::invertAffineTransform (InputArray M, 
				OutputArray iM)

顾名思义：为了获得一个仿射矩阵的逆变换
```
# 线性极坐标（拒绝研究，好像没想到要用来干什么，23333）
```
void cv::linearPolar (InputArray src, 
		      OutputArray dst, 
		      Point2f center, 
		      double maxRadius, 
		      int flags)
```
# log极坐标（同上）
```
void cv::logPolar (InputArray src, 
		   OutputArray dst, 
		   Point2f center, 
		   double M, 
		   int flags)
```
