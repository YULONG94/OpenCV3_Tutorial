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

// 用来寻找图片中存在的角点
// 输出dst是单通道的矩阵（尺寸继承源像src），数据类型是CV_32FC1
// 举个例子
Mat src_origin = imread("D://data//building.jpg");
imshow("src", src_origin);
Mat src;
cvtColor(src_origin, src, CV_BGR2GRAY);
Mat dstimage;
cornerHarris(src, dstimage, 2, 3, 0.01);
// 灰度值很小的情况下，如果直接显示，肉眼将无法分辨
imshow("【直接显示】", dstimage);
// 可以取阈值转化之后进行显示
Mat dst_th;
threshold(dstimage, dst_th, 0.0001, 255, CV_THRESH_BINARY);
imshow("阈值化之后显示", dst_th);

// 另一种就是历遍所有点并显示
Mat show_dst;
src_origin.copyTo(show_dst);
Mat norm_img;
normalize(dstimage, norm_img, 0, 255, 32);

int thresh = 30;
for (int j = 0; j < norm_img.rows; j++)
{
	for (int i = 0; i < norm_img.cols; i++)
	{
		if ((int)norm_img.at<float>(j, i) > thresh)
		{
			circle(show_dst, Point(i, j), 2, Scalar::all(255), 1, 8);
		}
	}
}
imshow("终极显示", show_dst);

waitKey();

```
# 角点最小特征值检测
```
void cv::cornerMinEigenVal (InputArray src, 
                            OutputArray dst, 
                            int blockSize, 
                            int ksize = 3, 
                            int borderType = BORDER_DEFAULT)
// 直接拷贝一个别人的代码吧
// 因为暂时还没研究这个函数具体是用来做什么
Mat src;
src= imread("D:6.jpg");
Mat dst(src.size(),CV_32FC(6));
cvtColor(src, src, CV_RGB2GRAY);
cornerMinEigenVal(src, dst, 3, 3, BORDER_DEFAULT);
cout << dst.at<float>(100, 100) << endl;
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
// 前一个Harris角点检测虽然很强大，但是存在很多缺点（像素级角点， 速度较慢）
// 而且cornerHarris函数返回的是一张图，而这里的输出就是检测到的角点，是vector<Point2f>的数据类型
// 这个函数它不仅支持Harris角点检测，也支持Shi Tomasi算法的角点检测，
// 但是，该函数检测到的角点依然是像素级别的，若想获取更为精细的角点坐标，
// 需要调用cv::cornerSubPix()函数进一步细化处理，即亚像素。
// 举个例子
cv::Mat image_color = cv::imread("D://data//building.jpg", cv::IMREAD_COLOR);

//使用灰度图像进行角点检测
cv::Mat image_gray;
cv::cvtColor(image_color, image_gray, cv::COLOR_BGR2GRAY);

//设置角点检测参数
std::vector<cv::Point2f> corners;
int max_corners = 200;
double quality_level = 0.01;
double min_distance = 3.0;
int block_size = 3;
bool use_harris = false;
double k = 0.04;

//角点检测
cv::goodFeaturesToTrack(image_gray,
						corners,
						max_corners,
						quality_level,
						min_distance,
						cv::Mat(),
						block_size,
						use_harris,
						k);

//将检测到的角点绘制到原图上
for (int i = 0; i < corners.size(); i++)
{
	cv::circle(image_color, corners[i], 1, cv::Scalar(0, 0, 255), 2, 8, 0);
}

cv::imshow("building corner", image_color);
cv::waitKey();
```
# 亚像素角点检测
```
void cv::cornerSubPix (InputArray image, 
                       InputOutputArray corners, 
                       Size winSize, 
                       Size zeroZone, 
                       TermCriteria criteria)
// 前面检测角点的方法都是像素级别精度的，这一种方法用于精细化角点检测出的位置
// 正如前面所说，这个函数一般结合goodFeaturesToTrack函数使用
// 举个例子
cv::Mat image_color = cv::imread("D://data//building.jpg", cv::IMREAD_COLOR);

//使用灰度图像进行角点检测
cv::Mat image_gray;
cv::cvtColor(image_color, image_gray, cv::COLOR_BGR2GRAY);

//设置角点检测参数
std::vector<cv::Point2f> corners;
int max_corners = 200;
double quality_level = 0.01;
double min_distance = 3.0;
int block_size = 3;
bool use_harris = false;
double k = 0.04;

//角点检测
cv::goodFeaturesToTrack(image_gray,
						corners,
						max_corners,
						quality_level,
						min_distance,
						cv::Mat(),
						block_size,
						use_harris,
						k);
//将检测到的角点绘制到原图上
for (int i = 0; i < corners.size(); i++)
{
	cv::circle(image_color, corners[i], 1, cv::Scalar(0, 0, 255), 2, 8, 0);
}
TermCriteria criteria = TermCriteria(TermCriteria::MAX_ITER + TermCriteria::EPS, 40, 0.01);
cornerSubPix(image_gray, corners, cv::Size(5, 5), cv::Size(-1, -1), criteria);

Mat image_copy;
image_color.copyTo(image_copy);
//将检测到的角点绘制到原图上
for (int i = 0; i < corners.size(); i++)
{
	cv::circle(image_copy, corners[i], 1, cv::Scalar(0, 0, 255), 2, 8, 0);
}
cv::imshow("building corner1", image_color);
cv::imshow("building corner2", image_copy);
cv::waitKey();

return 0;
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
                                                        
Ptr<FastLineDetector> cv::ximgproc::createFastLineDetector (int _length_threshold = 10, 
                                                            float _distance_threshold = 1.414213562f, 
                                                            double _canny_th1 = 50.0, 
                                                            double _canny_th2 = 50.0, 
                                                            int _canny_aperture_size = 3, 
                                                            bool _do_merge = false)

// 这两个函数用于创建直线检测算子的类
// 然后可以利用这两个算子进行直线检测
// 官方提供的两个例子非常好用
```
[官方fld_lines.cpp](https://docs.opencv.org/3.4.2/d1/d9e/fld_lines_8cpp-example.html#a8)
[官方lsd_lines.cpp](https://docs.opencv.org/3.4.2/d8/dd4/lsd_lines_8cpp-example.html#a10)
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
// 直接举个例子
Mat image_color = cv::imread("D://data//water_coins.jpg", cv::IMREAD_COLOR);
Mat img_gray;
cvtColor(image_color, img_gray, CV_BGR2GRAY);
vector<Vec3f> circles;
HoughCircles(img_gray, circles, HOUGH_GRADIENT, 2, img_gray.rows/16, 200, 100);
cout << circles.size() << endl;
for (size_t i = 0; i < circles.size(); i++)
{
	Point center(cvRound(circles[i][0]), cvRound(circles[i][1]));
	int radius = cvRound(circles[i][2]);
	// draw the circle center
	circle(image_color, center, 3, Scalar(0, 255, 0), -1, 8, 0);
	// draw the circle outline
	circle(image_color, center, radius, Scalar(0, 0, 255), 3, 8, 0);
}
namedWindow("circles", 1);
imshow("circles", image_color);
waitKey();
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
//直接举个例子
vector<Vec4i> lines;
HoughLines(canny_frame, lines, 2, CV_PI / 90, 150, 100, 0);
// 依次画出
for (int i = 0; i < lines.size(); i++)
{
	float rho = lines[i][0], theta = lines[i][1];
	if (theta > CV_PI / 90)
		continue;
	Point pt1, pt2;
	double a = cos(theta), b = sin(theta);
	double x0 = a * rho, y0 = b * rho;
	pt1.x = cvRound(x0 + 1000 * (-b));
	pt1.y = cvRound(y0 + 1000 * (a));
	pt2.x = cvRound(x0 - 1000 * (-b));
	pt2.y = cvRound(y0 - 1000 * (a));
	line(line_frame, pt1, pt2, Scalar(0, 0, 255), 1, CV_AA);
}
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
//直接举个例子
vector<Vec4i> lines;
HoughLinesP(BW_frame, lines, 1, CV_PI / 180, 100, 400, 3);
for (int i = 0; i < lines.size(); i++)
{
	Vec4i l = lines[i];//Vec4i 就是Vec<int, 4>，里面存放４个int
	line(line_frame, Point(l[0], l[1]), Point(l[2], l[3]), Scalar(0, 0, 255), 1, CV_AA);
}
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
