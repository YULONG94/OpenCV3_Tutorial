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
RotatedRect(const Point2f &center, const Size2f &size, float angle);
RotatedRect rRect1(Point2f(150,150), Size2f(100,50), 30.0);
RotatedRect rRect2; 
rRect2.center = Point2f(150,150);
rRect2.size = Size2f(100,50);
rRect2.angle = 30.0;
Point2f vertices[4];
rRect2.points(vertices);
```
