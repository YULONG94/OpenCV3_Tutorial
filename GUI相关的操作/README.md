[官方文档入口](https://docs.opencv.org/3.4.2/d7/dfc/group__highgui.html)
# MouseEventFlags
```
  cv::EVENT_FLAG_LBUTTON = 1, 
  cv::EVENT_FLAG_RBUTTON = 2, 
  cv::EVENT_FLAG_MBUTTON = 4, 
  cv::EVENT_FLAG_CTRLKEY = 8, 
  cv::EVENT_FLAG_SHIFTKEY = 16, 
  cv::EVENT_FLAG_ALTKEY = 32 
```
# MouseEventTypes
```
  cv::EVENT_MOUSEMOVE = 0, 
  cv::EVENT_LBUTTONDOWN = 1, 
  cv::EVENT_RBUTTONDOWN = 2, 
  cv::EVENT_MBUTTONDOWN = 3, 
  cv::EVENT_LBUTTONUP = 4, 
  cv::EVENT_RBUTTONUP = 5, 
  cv::EVENT_MBUTTONUP = 6, 
  cv::EVENT_LBUTTONDBLCLK = 7, 
  cv::EVENT_RBUTTONDBLCLK = 8, 
  cv::EVENT_MBUTTONDBLCLK = 9, 
  cv::EVENT_MOUSEWHEEL = 10, 
  cv::EVENT_MOUSEHWHEEL = 11 
```
# QtButtonTypes
```
  cv::QT_PUSH_BUTTON = 0, 
  cv::QT_CHECKBOX = 1, 
  cv::QT_RADIOBOX = 2, 
  cv::QT_NEW_BUTTONBAR = 1024 
```
# QtFontStyles
```
  cv::QT_STYLE_NORMAL = 0, 
  cv::QT_STYLE_ITALIC = 1, 
  cv::QT_STYLE_OBLIQUE = 2 
```
# QtFontWeights
```
  cv::QT_FONT_LIGHT = 25, 
  cv::QT_FONT_NORMAL = 50, 
  cv::QT_FONT_DEMIBOLD = 63, 
  cv::QT_FONT_BOLD = 75, 
  cv::QT_FONT_BLACK = 87 
```
# WindowFlags
```
  cv::WINDOW_NORMAL = 0x00000000, 
  cv::WINDOW_AUTOSIZE = 0x00000001, 
  cv::WINDOW_OPENGL = 0x00001000, 
  cv::WINDOW_FULLSCREEN = 1, 
  cv::WINDOW_FREERATIO = 0x00000100, 
  cv::WINDOW_KEEPRATIO = 0x00000000, 
  cv::WINDOW_GUI_EXPANDED =0x00000000, 
  cv::WINDOW_GUI_NORMAL = 0x00000010 
```
# WindowPropertyFlags
```
  cv::WND_PROP_FULLSCREEN = 0, 
  cv::WND_PROP_AUTOSIZE = 1, 
  cv::WND_PROP_ASPECT_RATIO = 2, 
  cv::WND_PROP_OPENGL = 3, 
  cv::WND_PROP_VISIBLE = 4 
```
# 创建滑动条
```
int cv::createTrackbar (const String & trackbarname, 
                        const String & winname, 
                        int * value, 
                        int count, 
                        TrackbarCallback onChange = 0, 
                        void * userdata = 0)
// 在画图窗口中添加滑动条
// 举个例子，这个例子基本就是照搬照抄了官方提供的例子
#include <iostream>
#include "opencv2/core.hpp"
#include "opencv2/core/utility.hpp"
#include "opencv2/imgproc.hpp"
#include "opencv2/imgcodecs.hpp"
#include "opencv2/highgui.hpp"


using namespace cv;
using namespace std;

Mat src1_t, src2_t;
Mat dst_t;
double alpha_t, beta_t;
int track_value;
int track_max_value = 100;

static void on_track(int, void*)
{
	alpha_t = (double)track_value / track_max_value;
	beta_t = (1.0 - alpha_t);
	addWeighted(src1_t, alpha_t, src2_t, beta_t, 0, dst_t);
	imshow("result", dst_t);
}

int main()
{
	src1_t = imread("..//data//LinuxLogo.jpg");
	src2_t = imread("..//data//WindowsLogo.jpg");
	if (src1_t.empty() || src2_t.empty())
	{
		cout << "open error" << endl;
		return -1;
	}
	namedWindow("result");
	track_value = 0;
	char track_name[50];
	sprintf_s(track_name, 50, "%d", track_max_value);
	createTrackbar(track_name, "result", &track_value, track_max_value, on_track);
	on_track(track_value, 0);
	waitKey(0);
	return 0;
}
```
# 设置滚动条的最大值最小值
```
void cv::setTrackbarMax (const String & trackbarname, 
			 const String & winname, 
			 int maxval)

void cv::setTrackbarMin (const String & trackbarname, 
			 const String & winname, 
			 int minval)
```
# 设置滚动条的位置
```
void cv::setTrackbarPos (const String & trackbarname, 
			 const String & winname, 
			 int pos)
// 或者说是滚动条的数值
```
# 获取滑动条的位置
```
int cv::getTrackbarPos (const String & trackbarname, 
			const String & winname)

// 插入到回调函数的定义中即可
```
# 清除所有画图窗口
```
void cv::destroyAllWindows ()
```
# 清除某个画图窗口
```
void cv::destroyWindow (const String & winname)
```
# 返回画图窗口所在的位置
```
Rect cv::getWindowImageRect (const String & winname)
```
# 设置画图窗口的几种属性
```
void cv::setWindowProperty (const String & winname, 
			    int prop_id, 
			    double prop_value)
```
# 获取画图窗口的五种属性
```
double cv::getWindowProperty (const String & winname, 
			      int prop_id)
```
# 图像显示
```
void cv::imshow (const String & winname, 
		 InputArray mat)
```
# 窗口移动
```
void cv::moveWindow (const String & winname, 
		     int x, 
		     int y)
```
# 窗口命名
```
void cv::namedWindow (const String & winname, 
		      int flags = WINDOW_AUTOSIZE)
```
# 设置窗口标题
```
void cv::setWindowTitle (const String & winname, 
			 const String & title)
```
# 调整窗口大小
```
void cv::resizeWindow (const String & winname, 
		       int width, 
		       int height)

void cv::resizeWindow (const String & winname, 
		       const cv::Size & size)

// 只有当窗口不是WINDOW_AUTOSIZE的时候才能进行调整
// 而且这种改变不改变图像，仅仅是改变了窗口大小
// 所以如果粗暴的进行个resize可能导致原图显示不全或者空白区域太多的结果
```
# 选择一个ROI区域
```
Rect cv::selectROI (const String & windowName, 
		    InputArray img, 
		    bool showCrosshair = true, 
		    bool fromCenter = false)

Rect cv::selectROI (InputArray img, 
		    bool showCrosshair = true, 
		    bool fromCenter = false)
// 举个例子
Mat src = imread("picture.jpg");
imshow("test", src);
Rect r;
r = selectROI("test", src, true, false);
waitKey(0);
```
# 选择多个ROI区域
```
void cv::selectROIs (const String & windowName, 
		     InputArray img, 
		     std::vector< Rect > & boundingBoxes, 
		     bool showCrosshair = true, 
		     bool fromCenter = false)

// 举个例子
Mat src = imread("picture.jpg");
imshow("test", src);
vector<Rect> rs;
selectROIs("test", src, rs, true, false);
cout << rs.size() << endl;
waitKey(0);
```
# 设置鼠标回调函数
```
void cv::setMouseCallback (const String & winname, 
			   MouseCallback onMouse, 
			   void * userdata = 0)

// 将鼠标作为交互工具，传递鼠标的一些操作信息
#include <iostream>
#include "opencv2/core.hpp"
#include "opencv2/core/utility.hpp"
#include "opencv2/imgproc.hpp"
#include "opencv2/imgcodecs.hpp"
#include "opencv2/highgui.hpp"


using namespace cv;
using namespace std;

Mat src1_t;
Point p1, p2;

static void on_mouse(int event, int x, int y, int flag, void*)
{

	if (event == EVENT_LBUTTONDOWN)
	{
		p1.x = x;
		p1.y = y;
	}
	else if (event == EVENT_LBUTTONUP)
	{
		p2.x = x;
		p2.y = y;
		line(src1_t, p1, p2, Scalar::all(255));
		imshow("result", src1_t);
	}
}

int main()
{
	src1_t = imread("..//data//LinuxLogo.jpg");
	if (src1_t.empty())
	{
		cout << "open error" << endl;
		return -1;
	}
	namedWindow("result");
	imshow("result", src1_t);
	setMouseCallback("result", on_mouse, 0);
	waitKey(0);
	return 0;
}
```
# 获取鼠标滚动的信息
```
// 先记着，可能后面才会用，现在还不会用
int cv::getMouseWheelDelta (int flags)
```
# startWindowThread
```
int cv::startWindowThread ()
// 这个函数一直没用过，应该是为了实时刷新缓存之类的用法，先不研究，用到的时候再说
```
# waitKey()
```
int cv::waitKey (int delay = 0)
// 最常用的一个函数，用法就不解释了
```
# waitKeyEx()
```
int cv::waitKeyEx (int delay = 0)
// 和上面那个类似，但是返回的是全规格键盘的按键
// 比如，方向键在waitKey情况下都是0，但是在waitKeyEx的情况下，也是可以有各自的数值
```
