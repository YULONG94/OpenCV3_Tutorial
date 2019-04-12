查看[官方文档](https://docs.opencv.org/3.4.2/d3/dc0/group__imgproc__shape.html)
# ConnectedComponentsAlgorithmsTypes
```
  cv::CCL_WU = 0, 
  cv::CCL_DEFAULT = -1, 
  cv::CCL_GRANA = 1 
```
# ConnectedComponentsTypes
```
  cv::CC_STAT_LEFT = 0, 
  cv::CC_STAT_TOP = 1, 
  cv::CC_STAT_WIDTH = 2, 
  cv::CC_STAT_HEIGHT = 3, 
  cv::CC_STAT_AREA = 4, 
  cv::CC_STAT_MAX = 5 
```
# ContourApproximationModes
```
  cv::CHAIN_APPROX_NONE = 1, 
  cv::CHAIN_APPROX_SIMPLE = 2, 
  cv::CHAIN_APPROX_TC89_L1 = 3, 
  cv::CHAIN_APPROX_TC89_KCOS = 4 
```
# RectanglesIntersectTypes
```
  cv::INTERSECT_NONE = 0, 
  cv::INTERSECT_PARTIAL = 1, 
  cv::INTERSECT_FULL = 2 
```
# RetrievalModes
```
  cv::RETR_EXTERNAL = 0, 
  cv::RETR_LIST = 1, 
  cv::RETR_CCOMP = 2, 
  cv::RETR_TREE = 3, 
  cv::RETR_FLOODFILL = 4 
```
# ShapeMatchModes
```
  cv::CONTOURS_MATCH_I1 =1, 
  cv::CONTOURS_MATCH_I2 =2, 
  cv::CONTOURS_MATCH_I3 =3 
```
# 多边拟合函数
```
void cv::approxPolyDP (InputArray curve, 
                       OutputArray approxCurve, 
                       double epsilon, 
                       bool closed)

// 用来对一段连续的曲线进行折线化，或者或是做多边形拟合
// 比如对物体的轮廓进行拟合
// 通过下一个函数可以证明拟合出来的线一定比原来的线更短，很合理，所以对于闭合的曲线一定是内接在其中的。
InputArray curve:一般是由图像的轮廓点组成的点集
OutputArray approxCurve：表示输出的多边形点集
double epsilon：主要表示输出的精度，就是两个轮廓点之间最大距离数，5,6,7，，8
bool closed：表示输出的多边形是否封闭
// 举个例子
Mat src = imread("D://data//test_contour.png");
cvtColor(src, src, CV_BGR2GRAY);
imshow("原图", src);
Mat src_BW;
threshold(src, src_BW, 200, 255, CV_THRESH_BINARY_INV);
imshow("BW图", src_BW);


vector<vector<Point>> contour;
vector<Vec4i> heirarchy;
findContours(src_BW, contour, heirarchy, 0, CV_CHAIN_APPROX_NONE);
// cout << contour.size() << endl;
vector<vector<Point>> contour_poly(contour.size());

Mat dst_img = Mat(src.size(), CV_8UC3, Scalar::all(0));
for (int i = 0; i < contour.size(); i++)
{
	approxPolyDP(Mat(contour[i]), contour_poly[i], 5, true);
  Rect r = boundingRect(Mat(contour[i]));
	rectangle(dst_img, r, Scalar(255, 0, 0));
  RotatedRect rr = minAreaRect(Mat(contour[i]));
	Point2f verticls[4];
	rr.points(verticls);  //获得旋转矩形的四个顶点
  for (int ir = 0; ir < 4; ir++)
	{
		line(dst_img, verticls[ir], verticls[(ir+1)%4], Scalar(0, 0, 255));
	}
  /*
  // 第二种方法
  CvPoint2D32f verticls1[4];
	cvBoxPoints(rr, verticls1);
  for (int ir = 0; ir < 4; ir++)
	{
		line(dst_img, verticls1[ir], verticls1[(ir+1)%4], Scalar(0, 0, 255));
	}
  // 第三种方法
  Mat boxPts;
	boxPoints(rr, boxPts);
	cout << boxPts.size() << endl;
	for (int ir = 0; ir < 4; ir++)
	{
			Point pt1 = Point(boxPts.at<float>(ir, 0), boxPts.at<float>(ir, 1));
			Point pt2 = Point(boxPts.at<float>((ir + 1) % 4, 0), boxPts.at<float>((ir + 1) % 4, 1));
			line(dst_img, pt1, pt2, Scalar(0, 0, 255));
	}
  */
  cout << arcLength(contour_poly[i], true)<< " " <<arcLength(contour[i], true) << endl;
  cout << contourArea(contour[i]) << " " << contourArea(contour_poly[i]) << endl;
	drawContours(dst_img, contour_poly, i, Scalar::all(255));
  drawContours(dst_img, contour, i, Scalar(0, 255, 0));
}
imshow("结果", dst_img);
waitKey();
```
# 计算多边形周长或曲线长度
```
double cv::arcLength (InputArray curve, 
                      bool closed)

// 例子的化，看上面那个就行了

```
# 计算轮廓的垂直边界最小矩形
```
Rect cv::boundingRect (InputArray points)
// 返回一个点集的外接矩形
// 举个例子，懒得另外再写了，同样参见上面的例子
```
# 计算最小外接矩形
```
RotatedRect cv::minAreaRect (InputArray points)
// 同样参见上面的例子
```
# 计算最小外接圆
```
void cv::minEnclosingCircle (InputArray points, 
			     Point2f & center, 
			     float & radius)
```
# 计算最小外接三角形
```
double cv::minEnclosingTriangle (InputArray points, 
				 OutputArray triangle)
```
# 获得旋转矩形的四个顶点
```
void cv::boxPoints (RotatedRect box, 
                    OutputArray points)

//正好上面的例子又有用到的地方
//正如例子中表示的，你也可以使用RotatedRect自带的属性函数.points()来获得
```
# 连通域处理函数
```
int cv::connectedComponents (InputArray image, 
                             OutputArray labels, 
                             int connectivity, 
                             int ltype, 
                             int ccltype)

int cv::connectedComponents (InputArray image, 
                             OutputArray labels, 
                             int connectivity = 8, 
                             int ltype = CV_32S)

int cv::connectedComponentsWithStats (InputArray image, 
                                      OutputArray labels, 
                                      OutputArray stats, 
                                      OutputArray centroids, 
                                      int connectivity, 
                                      int ltype, 
                                      int ccltype)

int cv::connectedComponentsWithStats (InputArray image, 
                                      OutputArray labels, 
                                      OutputArray stats, 
                                      OutputArray centroids, 
                                      int connectivity = 8, 
                                      int ltype = CV_32S)

// 是几个好函数，在需要处理的信息不是特别复杂的情况下应该非常好用
```
# 计算边框面积
```
double cv::contourArea (InputArray contour, 
                        bool oriented = false)

// 还是可以回到那个多功能例子中可以看到怎么用
```
# 寻找凸包
```
void cv::convexHull (InputArray points, 
                     OutputArray hull, 
                     bool clockwise = false, 
                     bool returnPoints = true)
// 同样的还是可以把这个函数用在那个多功能例子，但是觉得写太多了就会台繁杂了，重写一个
// 需要注意的是作为接收凸包的容器可以是int类型也可以是point类型
// 如果是前者，那么表示是输入的下标，否则表示的是一个凸包轮廓线
// 但其实这两个是一模一样的，因为凸包上的点一定是所有点集的一个子集
Mat src = imread("D://data//test_hull.png");
cvtColor(src, src, CV_BGR2GRAY);
imshow("原图", src);
Mat src_BW;
threshold(src, src_BW, 200, 255, CV_THRESH_BINARY_INV);
imshow("BW图", src_BW);

vector<vector<Point>> contour;
vector<Vec4i> heirarchy;
findContours(src_BW, contour, heirarchy, 0, CV_CHAIN_APPROX_NONE);
// cout << contour.size() << endl;
vector<vector<Point>> contour_poly(contour.size());

Mat dst_img = Mat(src.size(), CV_8UC3, Scalar::all(0));
vector<vector<Point>>hull(contour.size());
for (int i = 0; i < contour.size(); i++)
{
	convexHull(Mat(contour[i]), hull[i], false);
	drawContours(dst_img, contour, i, Scalar(0, 255, 0), 1, 8, vector<Vec4i>(), 0, Point());
	drawContours(dst_img, hull, i, Scalar(255, 0, 0), 1, 8, vector<Vec4i>(), 0, Point());
}
imshow("结果", dst_img);
waitKey();
```
# 计算轮廓凸缺陷
```
void cv::convexityDefects (InputArray contour, 
                           InputArray convexhull, 
                           OutputArray convexityDefects)
// 配合上一个函数使用（而且convexhull需要时vector<vector<int>>形式，而不是vector<vector<Point>>形式）
// 目的是为了找到一个轮廓与他的凸包之间所差的块
// 作为输出的每个convexity defect区域有四个特征量：
// 起始点（startPoint）表示缺陷在轮廓上的开始处，他的值是开始点在函数第一个参数 contour 中的下标索引；
// 结束点(endPoint)表示缺陷结束处在 contour 中的下标索引；
// 距离convexity hull最远点(farPoint)表示最远点在 contour 中的下标索引；
// 最远点到convexity hull的距离(depth)表示最远点到convexhull的距离，除以256后以像素为单位

// 举个例子
Mat src = imread("D://data//test_hull.png");
cvtColor(src, src, CV_BGR2GRAY);
imshow("原图", src);
Mat src_BW;
threshold(src, src_BW, 200, 255, CV_THRESH_BINARY_INV);
imshow("BW图", src_BW);


vector<vector<Point>> contour;
vector<Vec4i> heirarchy;
findContours(src_BW, contour, heirarchy, 0, CV_CHAIN_APPROX_NONE);
// cout << contour.size() << endl;
vector<vector<Point>> contour_poly(contour.size());

Mat dst_img = Mat(src.size(), CV_8UC3, Scalar::all(0));
vector<vector<Point>>hull(contour.size());
vector<vector<int>>hulli(contour.size());
vector<vector<Vec4i>> defects(contour.size());
for (int i = 0; i < contour.size(); i++)
{
	convexHull(Mat(contour[i]), hull[i], false);
	convexHull(Mat(contour[i]), hulli[i], false);
	convexityDefects(Mat(contour[i]), hulli[i], defects[i]);

	//drawContours(dst_img, contour, i, Scalar(0, 255, 0), 1, 8, vector<Vec4i>(), 0, Point());
	drawContours(dst_img, hull, i, Scalar(255, 0, 0), 1, 8, vector<Vec4i>(), 0, Point());

	// draw defects
	vector<Vec4i>::iterator d = defects[i].begin();
	while (d != defects[i].end())
	{
		Vec4i& v = (*d);
		cout << v << endl;
		int startid = v[0];
		int endid = v[1];
		int farid = v[2];
		int dept = v[3];
		Point st = Point(contour[i][startid]);
		Point en = Point(contour[i][endid]);
		Point ptfar = Point(contour[i][farid]);
		int depth_defect = dept / 256;
		line(dst_img, st, ptfar, Scalar(0, 0, 255), 1);
		line(dst_img, en, ptfar, Scalar(0, 0, 255), 1);

		d++;
	}
}
imshow("结果", dst_img);
waitKey();
```
# 找轮廓
```
void cv::findContours (InputOutputArray image, 
                       OutputArrayOfArrays contours, 
                       OutputArray hierarchy, 
                       int mode, 
                       int method, 
                       Point offset = Point())

void cv::findContours (InputOutputArray image, 
                       OutputArrayOfArrays contours, 
                       int mode, 
                       int method, 
                       Point offset = Point())

// 这个函数前面已经用了很多次了，这里就不重复写代码了
```
# 轮廓的椭圆拟合1
```
RotatedRect cv::fitEllipse (InputArray points)
//举个例子
Mat src = imread("D://data//test_hull.png");
cvtColor(src, src, CV_BGR2GRAY);
imshow("原图", src);
Mat src_BW;
threshold(src, src_BW, 200, 255, CV_THRESH_BINARY_INV);
imshow("BW图", src_BW);


vector<vector<Point>> contour;
vector<Vec4i> heirarchy;
findContours(src_BW, contour, heirarchy, 0, CV_CHAIN_APPROX_NONE);

Mat dst_img = Mat(src.size(), CV_8UC3, Scalar::all(0));

for (int i = 0; i < contour.size(); i++)
{
	RotatedRect ebox= fitEllipse(contour[i]);
	ellipse(dst_img, ebox, Scalar(0, 255, 0));
	drawContours(dst_img, contour, i, Scalar::all(255));
}
imshow("结果", dst_img);
waitKey();
```
# 轮廓的椭圆拟合2
```
RotatedRect cv::fitEllipseAMS (InputArray points)
// 可以使用上面的例子直接替换对应的函数就可以了
```
# 轮廓的椭圆拟合3
```
RotatedRect cv::fitEllipseDirect (InputArray points)
// 同样的直接替换想应的函数就可以了
```
# 直线拟合
```
void cv::fitLine (InputArray points, 
		  OutputArray line, 
		  int distType, 
		  double param, 
		  double reps, 
		  double aeps)

// 例子
//创建一个用于绘制图像的空白图  
cv::Mat image = cv::Mat::zeros(480, 640, CV_8UC3);

//输入拟合点  
std::vector<cv::Point> points;
points.push_back(cv::Point(48, 58));
points.push_back(cv::Point(105, 98));
points.push_back(cv::Point(155, 160));
points.push_back(cv::Point(212, 220));
points.push_back(cv::Point(248, 260));
points.push_back(cv::Point(320, 300));
points.push_back(cv::Point(350, 360));
points.push_back(cv::Point(412, 400));

//将拟合点绘制到空白图上  
for (int i = 0; i < points.size(); i++)
{
	cv::circle(image, points[i], 5, cv::Scalar(0, 0, 255), 2, 8, 0);
}

cv::Vec4f line_para;
cv::fitLine(points, line_para, cv::DIST_L2, 0, 1e-2, 1e-2);

std::cout << "line_para = " << line_para << std::endl;

//获取点斜式的点和斜率  
cv::Point point0;
point0.x = line_para[2];
point0.y = line_para[3];

double k = line_para[1] / line_para[0];

//计算直线的端点(y = k(x - x0) + y0)  
cv::Point point1, point2;
point1.x = 0;
point1.y = k * (0 - point0.x) + point0.y;
point2.x = 640;
point2.y = k * (640 - point0.x) + point0.y;

cv::line(image, point1, point2, cv::Scalar(0, 255, 0), 2, 8, 0);

cv::imshow("image", image);
cv::waitKey(0);
```
# 计算矩
```
Moments cv::moments (InputArray array,
		     bool binaryImage = false)
返回的Moments类型变量具有如下的一些值
空间矩：
m00, m01, m02, m10, m11, m12, m20, m21, m03, m30
中心矩：
mu20, mu11, mu02, mu30, mu21, mu12, mu03
中心归一化矩：
nu20, nu11, nu02, nu30, nu21, nu12, nu03
// 通过计算轮廓的矩可以得到质心
RNG rng;
Mat src = imread("D://data//stars.jpg");
Mat src_gray;
cvtColor(src, src_gray, CV_BGR2GRAY);
// blur(src_gray, src_gray, Size(3, 3));
imshow("灰度图", src_gray);
Mat src_canny;
Mat src_BW;
// Canny(src_gray, src_canny, 100, 200);
threshold(src_gray, src_BW, 200, 255, 1);
// imshow("canny", src_canny);
imshow("src_BW", src_BW);
vector<vector<Point>> contours;
vector<Vec4i> hierarchy;
findContours(src_BW, contours, hierarchy, CV_RETR_TREE, CV_CHAIN_APPROX_SIMPLE, Point(0, 0));
vector<Moments> mu(contours.size());
for (int i = 0; i < contours.size(); i++)
{
	mu[i] = moments(contours[i], false);
}
vector<Point2f> mc(contours.size());
for (int i = 0; i < contours.size(); i++)
{
	mc[i] = Point2f(mu[i].m10 / mu[i].m00, mu[i].m01/mu[i].m00);
}
// 画出轮廓可质心
Mat drawing = Mat(src_BW.size(), CV_8UC3);
drawing = Scalar::all(0);
for (int i = 0; i < contours.size(); i++)
{
	Scalar color = Scalar(rng.uniform(0, 255), rng.uniform(0, 255), rng.uniform(0, 255));
	drawContours(drawing, contours, i, color, 2, 8, hierarchy, 0, Point(0, 0));
	circle(drawing, mc[i], 4, Scalar(0, 0, 255), -1, 8, 0);
}
imshow("结果", drawing);
waitKey();
```
# 计算不变矩
```
void cv::HuMoments (const Moments & moments, double hu[7])
void cv::HuMoments (const Moments & m, OutputArray hu)
// 图像轮廓的不变矩具有变换的不变性，可以用于进行模式匹配
// 至于怎么得到的，主要是通过moments中的中心归一化矩，计算公式的话可以查看官网
// 网上看直接使用这些来匹配形况的好像比较少，因为可以直接使用matchshape这样的函数来进行比较
```
# 形状匹配
```
double cv::matchShapes (InputArray contour1, 
			InputArray contour2, 
			int method, 
			double parameter)

// 对两个轮廓进行匹配，返回的数值越大说明两个形状差的越远
// 匹配的方法的话有三种
// 举个例子
double c1 = matchShapes(contours[0], contours[1], CV_CONTOURS_MATCH_I1, 0.0);
cout << "CV_CONTOURS_MATCH_I1    " << c1 << endl;
double c2 = matchShapes(contours[0], contours[1], CV_CONTOURS_MATCH_I2, 0.0);
cout << "CV_CONTOURS_MATCH_I2    " << c2 << endl;
double c3 = matchShapes(contours[0], contours[1], CV_CONTOURS_MATCH_I3, 0.0);
cout << "CV_CONTOURS_MATCH_I3    " << c3 << endl;
// 上面这段代码可以直接插到前面计算moments的代码中去
```
# 找两个凸形状的交点
```
float cv::intersectConvexConvex (InputArray _p1, 
				 InputArray _p2, 
				 OutputArray _p12, 
				 bool handleNested = true)
```
# 找两个旋转矩形的交点情况
```
int cv::rotatedRectangleIntersection (const RotatedRect & rect1, 
				      const RotatedRect & rect2, 
				      OutputArray intersectingRegion)

// 如果有交点，返回整数，且把交界储存在最后一个参数内（最多有八个点）
// 具体的交点情况可以见下图
```

# 判断是否是凸形状
```
bool cv::isContourConvex (InputArray contour)
```
# 判断一个点是否在轮廓内，轮廓外或者轮廓上
```
double cv::pointPolygonTest (InputArray contour, 
			     Point2f pt, 
			     bool measureDist)

// 返回值有三种可能性
// -1：外部
// 0：上
// 1：内部
```
