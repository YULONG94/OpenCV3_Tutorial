# Point
> opencv中支持的点的类型
```
typedef Point_<int> Point2i;
typedef Point2i Point;
typedef Point_<float> Point2f;
typedef Point_<double> Point2d;
typedef Point3_<int> Point3i;
typedef Point3_<float> Point3f;
typedef Point3_<double> Point3d;
```
> Point的创建
```
Point pt;  //或者 Point pt = Point(10, 20);
pt.x = 10;
pt.y = 20;
```
> Point支持多种运算
```
pt1 = pt2 + pt3;
pt1 = pt2 - pt3;
pt1 = pt2 * a;
pt1 = a * pt2;
pt1 += pt2;
pt1 -= pt2;
pt1 *= a;
value = norm(pt); // L2 
normpt1 == pt2;
pt1 != pt2;
```
> Point指针，可以用于画图
```
int lineType = 8;
/** 创建一些点 */
Point rook_points[1][20];
rook_points[0][0] = Point( w/4.0, 7*w/8.0 );
rook_points[0][1] = Point( 3*w/4.0, 7*w/8.0 );
rook_points[0][2] = Point( 3*w/4.0, 13*w/16.0 );
rook_points[0][3] = Point( 11*w/16.0, 13*w/16.0 );
rook_points[0][4] = Point( 19*w/32.0, 3*w/8.0 );
rook_points[0][5] = Point( 3*w/4.0, 3*w/8.0 );
rook_points[0][6] = Point( 3*w/4.0, w/8.0 );
rook_points[0][7] = Point( 26*w/40.0, w/8.0 );
rook_points[0][8] = Point( 26*w/40.0, w/4.0 );
rook_points[0][9] = Point( 22*w/40.0, w/4.0 );
rook_points[0][10] = Point( 22*w/40.0, w/8.0 );
rook_points[0][11] = Point( 18*w/40.0, w/8.0 );
rook_points[0][12] = Point( 18*w/40.0, w/4.0 );
rook_points[0][13] = Point( 14*w/40.0, w/4.0 );
rook_points[0][14] = Point( 14*w/40.0, w/8.0 );
rook_points[0][15] = Point( w/4.0, w/8.0 );
rook_points[0][16] = Point( w/4.0, 3*w/8.0 );
rook_points[0][17] = Point( 13*w/32.0, 3*w/8.0 );
rook_points[0][18] = Point( 5*w/16.0, 13*w/16.0 );
rook_points[0][19] = Point( w/4.0, 13*w/16.0) ;
const Point* ppt[1] = { rook_points[0] };
int npt[] = { 20 };
fillPoly( img,
          ppt,
          npt,
          1,
          Scalar( 255, 255, 255 ),
          lineType );
```
# Size
```
Size(int width, int height)
Size size1(150, 100);
Size size2;
size2.width = 150;
size2.height = 100;
int myArea = size2.area();
```
# Rect
```
typedef Rect_<int> Rect2i;
typedef Rect_<float> Rect2f;
typedef Rect_<double> Rect2d;
typedef Rect2i Rect;
//默认情况下， Rect接受的参数都是整数
cv::Rect rect(x, y, width, height);
// rect接受如下操作
//如果创建一个Rect对象rect(100, 50, 50, 100)，那么rect会有以下几个功能：  
rect.area();     //返回rect的面积 5000  
rect.size();     //返回rect的尺寸 [50 × 100]  
rect.tl();       //返回rect的左上顶点的坐标 [100, 50]  
rect.br();       //返回rect的右下顶点的坐标 [150, 150]  
rect.width();    //返回rect的宽度 50  
rect.height();   //返回rect的高度 100  
rect.contains(Point(x, y));  //返回布尔变量，判断rect是否包含Point(x, y)点
//还可以求两个矩形的交集和并集  
rect = rect1 & rect2;  
rect = rect1 | rect2;  
//还可以对矩形进行平移和缩放    
rect = rect + Point(-100, 100); //平移，也就是左上顶点的x坐标-100，y坐标+100  
rect = rect + Size(-100, 100);  //缩放，左上顶点不变，宽度-100，高度+100  
//还可以对矩形进行对比，返回布尔变量  
rect1 == rect2;  
rect1 != rect2;  
//判断rect1是否在rect2里面
bool isInside(Rect rect1, Rect rect2)  
{  
    return (rect1 == (rect1&rect2));  
}   
//矩形中心点
Point getCenterPoint(Rect rect)  
{  
    Point cpt;  
    cpt.x = rect.x + cvRound(rect.width/2.0); //四舍五入  
    cpt.y = rect.y + cvRound(rect.height/2.0);  
    return cpt;  
}  
//围绕矩形中心缩放  
Rect rectCenterScale(Rect rect, Size size)  
{  
    rect = rect + size;   // 先将size扩大，相当于rect.size += size(实际上不能这么写)
    Point pt;  
    pt.x = cvRound(size.width/2.0);  
    pt.y = cvRound(size.height/2.0);  
    return (rect-pt);  // 将(x, y)移动， 相当于rect.xy += pt.xy
} 
```
# RotatedRect
```
RotatedRect(const Point2f &center, const Size2f &size, float angle);
RotatedRect rRect1(Point2f(150,150), Size2f(100,50), 30.0);
RotatedRect rRect2; 
rRect2.center = Point2f(150,150);
rRect2.size = Size2f(100,50);
rRect2.angle = 30.0;
Point2f vertices[4];
rRect2.points(vertices);
```
