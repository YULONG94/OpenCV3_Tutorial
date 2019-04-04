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
# convertMaps
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
