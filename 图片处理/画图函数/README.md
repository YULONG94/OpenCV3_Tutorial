# MarkerTypes
```
cv::MARKER_CROSS = 0, 
cv::MARKER_TILTED_CROSS = 1, 
cv::MARKER_STAR = 2, 
cv::MARKER_DIAMOND = 3, 
cv::MARKER_SQUARE = 4, 
cv::MARKER_TRIANGLE_UP = 5, 
cv::MARKER_TRIANGLE_DOWN = 6 
```
# 箭头
```
void cv::arrowedLine (InputOutputArray img, 
                      Point pt1, 
                      Point pt2, 
                      const Scalar & color, 
                      int thickness = 1, 
                      int line_type = 8, 
                      int shift = 0, 
                      double tipLength = 0.1)
```
# 画圆
```
void cv::circle (InputOutputArray img, 
                 Point center, 
                 int radius, 
                 const Scalar & color, 
                 int thickness = 1, 
                 int lineType = LINE_8, 
                 int shift= 0)
```
# 判断一条直线是否在某个矩形内
```
bool cv::clipLine (Size imgSize, 
                   Point & pt1, 
                   Point & pt2)

bool cv::clipLine (Size2l imgSize, 
                   Point2l & pt1, 
                   Point2l & pt2)

bool cv::clipLine (Rect imgRect, 
                   Point & pt1, 
                   Point & pt2)
 // imgSize：图像的大小，相当于矩形的端点在（0,0）
 // imgRect：要穿过的矩形
```
# 画轮廓
```
void cv::drawContours (InputOutputArray image, 
                       InputArrayOfArrays contours, 
                       int contourIdx, 
                       const Scalar & color, 
                       int thickness = 1, 
                       int lineType = LINE_8, 
                       InputArray hierarchy = noArray(), 
                       int maxLevel = INT_MAX, 
                       Point offset = Point())

// 举个例子，这个例子在做漫水法分割的时候用过,不过最后画轮廓使用的Mat格式有点不一样
Mat src = imread("D://data//water_coins.jpg");
imshow("原图", src);

Mat src_gray;
cvtColor(src, src_gray, COLOR_BGR2GRAY);
imshow("灰度图", src_gray);

Mat src_BW;
//adaptiveThreshold(src_gray, src_BW, 255, ADAPTIVE_THRESH_MEAN_C, THRESH_BINARY, 5, 10);
threshold(src_gray, src_BW, 200, 255, 1);
imshow("二值图", src_BW);

Mat src_canny;
Canny(src_BW,src_canny, 50, 200);
imshow("canny", src_canny);

vector<vector<Point>> contours;
vector<Vec4i> hierarchy;
findContours(src_canny, contours, hierarchy, RETR_CCOMP, CHAIN_APPROX_SIMPLE);

Mat maskWaterShed(src_canny.size(), CV_8UC1);
maskWaterShed = Scalar::all(0);
for (int i = 0; i < contours.size(); i++)
{
	drawContours(maskWaterShed, contours, i, Scalar::all(255), -1, 8, hierarchy, INT_MAX);
}
imshow("所谓的makers", maskWaterShed);
```
# 画椭圆
```
void cv::ellipse (InputOutputArray img, 
                  Point center, 
                  Size axes, 
                  double angle, 
                  double startAngle, 
                  double endAngle, 
                  const Scalar & color, 
                  int thickness = 1, 
                  int lineType = LINE_8, 
                  int shift = 0)
void cv::ellipse (InputOutputArray img, 
                  const RotatedRect & box, 
                  const Scalar & color, 
                  int thickness = 1, 
                  int lineType = LINE_8)
```
# 用多边形拟合椭圆
```
void cv::ellipse2Poly (Point center, 
                       Size axes, 
                       int angle, 
                       int arcStart, 
                       int arcEnd, 
                       int delta, 
                       std::vector< Point > & pts)

void cv::ellipse2Poly (Point2d center, 
                       Size2d axes, 
                       int angle, 
                       int arcStart, 
                       int arcEnd, 
                       int delta, 
                       std::vector< Point2d > & pts)
```
# 画多边形
```
void cv::fillPoly (Mat & img, 
                   const Point ** pts, 
                   const int * npts, 
                   int ncontours, 
                   const Scalar & color, 
                   int lineType = LINE_8, 
                   int shift = 0, 
                   Point offset = Point())

void cv::fillPoly (InputOutputArray img, 
                   InputArrayOfArrays pts, 
                   const Scalar & color, 
                   int lineType = LINE_8, 
                   int shift = 0, 
                   Point offset = Point())

// 这里给出官方的两个例子
// 例子一, 画一个多边形
int lineType = LINE_8;
int w = 400;
Mat img = Mat::zeros(w, w, CV_8UC3);
Point rook_points[1][20];
rook_points[0][0]  = Point(    w/4,   7*w/8 );
rook_points[0][1]  = Point(  3*w/4,   7*w/8 );
rook_points[0][2]  = Point(  3*w/4,  13*w/16 );
rook_points[0][3]  = Point( 11*w/16, 13*w/16 );
rook_points[0][4]  = Point( 19*w/32,  3*w/8 );
rook_points[0][5]  = Point(  3*w/4,   3*w/8 );
rook_points[0][6]  = Point(  3*w/4,     w/8 );
rook_points[0][7]  = Point( 26*w/40,    w/8 );
rook_points[0][8]  = Point( 26*w/40,    w/4 );
rook_points[0][9]  = Point( 22*w/40,    w/4 );
rook_points[0][10] = Point( 22*w/40,    w/8 );
rook_points[0][11] = Point( 18*w/40,    w/8 );
rook_points[0][12] = Point( 18*w/40,    w/4 );
rook_points[0][13] = Point( 14*w/40,    w/4 );
rook_points[0][14] = Point( 14*w/40,    w/8 );
rook_points[0][15] = Point(    w/4,     w/8 );
rook_points[0][16] = Point(    w/4,   3*w/8 );
rook_points[0][17] = Point( 13*w/32,  3*w/8 );
rook_points[0][18] = Point(  5*w/16, 13*w/16 );
rook_points[0][19] = Point(    w/4,  13*w/16 );

const Point* ppt[1] = { rook_points[0] };
int npt[] = { 20 };

fillPoly( img,
      ppt,
      npt,
      1,
      Scalar( 255, 255, 255 ),
      lineType );
imshow("result", img);
      
// 例子二， 同时画两个
int lineType = 8;
const int window_width = 900;
const int window_height = 600;
int x_1 = -window_width/2;
int x_2 = window_width*3/2;
int y_1 = -window_width/2;
int y_2 = window_width*3/2;
Mat img = Mat::zeros(w, w, CV_8UC3);
Point pt[2][3];
pt[0][0].x = rng.uniform(x_1, x_2);
pt[0][0].y = rng.uniform(y_1, y_2);
pt[0][1].x = rng.uniform(x_1, x_2);
pt[0][1].y = rng.uniform(y_1, y_2);
pt[0][2].x = rng.uniform(x_1, x_2);
pt[0][2].y = rng.uniform(y_1, y_2);
pt[1][0].x = rng.uniform(x_1, x_2);
pt[1][0].y = rng.uniform(y_1, y_2);
pt[1][1].x = rng.uniform(x_1, x_2);
pt[1][1].y = rng.uniform(y_1, y_2);
pt[1][2].x = rng.uniform(x_1, x_2);
pt[1][2].y = rng.uniform(y_1, y_2);

const Point* ppt[2] = {pt[0], pt[1]};
int npt[] = {3, 3};

fillPoly( image, ppt, npt, 2, randomColor(rng), lineType );
imshow("result", image);
```
# 另一个画多边形的函数
```
void cv::polylines (InputOutputArray img, 
                    InputArrayOfArrays pts, 
                    bool isClosed, 
                    const Scalar & color, 
                    int thickness = 1, 
                    int lineType = LINE_8, 
                    int shift = 0)

//这两个关于画多边形的函数的区别在于前者用于填充一个多边形， 而后者只负责画边，而且还不一定是闭合的多边
```
# 获得字体大小
```
double cv::getFontScaleFromHeight (const int fontFace, 
                                   const int pixelHeight, 
                                   const int thickness = 1)
// 在指定字体，指定字体线宽返回到达期望文本行高的字体尺寸
```
# 获得文本尺寸
```
Size cv::getTextSize (const String & text, 
                      int fontFace, 
                      double fontScale, 
                      int thickness, 
                      int * baseLine)
// 在指定书写的文本，字体，字体尺寸和字体线宽，输出文本的size
// 注意：最后一个参数是用作输出的，表示字体最下端到基准线的距离，比如西文字母y，最下端要低于书写时的基准线
// 官方例子
String text = "Funny text inside the box";
int fontFace = FONT_HERSHEY_SCRIPT_SIMPLEX;
double fontScale = 2;
int thickness = 3;

Mat img(600, 800, CV_8UC3, Scalar::all(0));

int baseline=0;
Size textSize = getTextSize(text, fontFace,
                            fontScale, thickness, &baseline);
baseline += thickness;

// center the text
Point textOrg((img.cols - textSize.width)/2,
              (img.rows + textSize.height)/2);

// draw the box
rectangle(img, textOrg + Point(0, baseline),
          textOrg + Point(textSize.width, -textSize.height),
          Scalar(0,0,255));
// ... and the baseline first
line(img, textOrg + Point(0, thickness),
     textOrg + Point(textSize.width, thickness),
     Scalar(0, 0, 255));

// then put the text itself
putText(img, text, textOrg, fontFace, fontScale,
        Scalar::all(255), thickness, 8);
imshow("test", img);
waitKey();
```
# 写文字
```
void cv::putText (InputOutputArray img, 
                  const String & text, 
                  Point org, 
                  int fontFace, 
                  double fontScale, 
                  Scalar color, 
                  int thickness =1, 
                  int lineType = LINE_8, 
                  bool bottomLeftOrigin = false)
// 写文字，这个函数也在上面的例子中使用了
// 最后一个参数接受默认就行了，尝试改成true之后发现字体翻转了
```
# 画线
```
void cv::line (InputOutputArray img, 
               Point pt1, 
               Point pt2, 
               const Scalar & color, 
               int thickness = 1, 
               int lineType = LINE_8, 
               int shift = 0)
```
# 画矩形
```
void cv::rectangle (InputOutputArray img, 
                    Point pt1, 
                    Point pt2, 
                    const Scalar & color, 
                    int thickness = 1, 
                    int lineType = LINE_8, 
                    int shift = 0)

void cv::rectangle (Mat & img, 
                    Rect rec, 
                    const Scalar & color, 
                    int thickness = 1, 
                    int lineType = LINE_8, 
                    int shift = 0)
```


