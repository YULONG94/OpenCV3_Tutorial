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
# 清空所有画图窗口
```
void cv::destroyAllWindows ()
```
# 清楚某个画图窗口
```
void cv::destroyWindow (const String & winname)
```
