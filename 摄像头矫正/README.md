[官方文档入口](https://docs.opencv.org/3.4.2/d9/d0c/group__calib3d.html)

这里太多三维空间的知识了，不再是简单的处理二维图片，所以看了一半，着实学不下去，找个借口，以后应用的可能性太低了，不学
# 摄像头矫正
```
double cv::calibrateCamera (InputArrayOfArrays objectPoints, 
                            InputArrayOfArrays imagePoints,
                            Size imageSize, 
                            InputOutputArray cameraMatrix, 
                            InputOutputArray distCoeffs, 
                            OutputArrayOfArrays rvecs, 
                            OutputArrayOfArrays tvecs, 
                            OutputArray stdDeviationsIntrinsics, 
                            OutputArray stdDeviationsExtrinsics, 
                            OutputArray perViewErrors, 
                            int flags = 0, 
                            TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, DBL_EPSILON))

double cv::calibrateCamera (InputArrayOfArrays objectPoints, 
                            InputArrayOfArrays imagePoints, 
                            Size imageSize, 
                            InputOutputArray cameraMatrix, 
                            InputOutputArray distCoeffs, 
                            OutputArrayOfArrays rvecs, 
                            OutputArrayOfArrays tvecs, 
                            int flags = 0, 
                            TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, DBL_EPSILON))
参数:
objectPoints ：世界坐标系中的点。在使用时，应该输入vector< vector< Point3f > >。
imagePoints ：其对应的图像点。和objectPoints一样，应该输入vector< vector< Point2f > >型的变量。
imageSize  ：图像的大小，在计算相机的内参数和畸变矩阵需要用到；
cameraMatrix  ：内参数矩阵。输入一个Mat cameraMatrix即可。
distCoeffs  ：畸变矩阵。输入一个Mat distCoeffs即可。
rvecs  ：旋转向量；应该输入一个Mat的vector，即vector< Mat > rvecs因为每个vector< Point3f >会得到一个rvecs。
tvecs ：位移向量；和rvecs一样，也应该为vector tvecs。
stdDeviationsIntrinsics ：内参数的输出向量。输出顺序为: (fx,fy,cx,cy,k1,k2,p1,p2,k3,k4,k5,k6,s1,s2,s3,s4,τx,τy) ，
                          如果不估计其中某一个参数，值等于0
stdDeviationsExtrinsics ：外参数的输出向量。输出顺序: (R1,T1,…,RM,TM) ，M是标定图片的个数, Ri,Ti 是1x3的向量 。
perViewErrors   每个标定图片的重投影均方根误差的输出向量。
criteria：   迭代优化算法的终止准则
flags ：标定函数是所采用的模型（重点）”。 
可输入如下某个或者某几个参数：
CV_CALIB_USE_INTRINSIC_GUESS：使用该参数时，将包含有效的fx,fy,cx,cy的估计值的内参矩阵cameraMatrix，作为初始值输入，
                              然后函数对其做进一步优化。如果不使用这个参数，用图像的中心点初始化光轴点坐标(cx, cy)，
                              使用最小二乘估算出fx，fy（这种求法好像和张正友的论文不一样，不知道为何要这样处理）。
                              注意，如果已知内部参数（内参矩阵和畸变系数），就不需要使用这个函数来估计外参，可以使用
                              solvepnp()函数计算外参数矩阵。
CV_CALIB_FIX_PRINCIPAL_POINT：在进行优化时会固定光轴点，光轴点将保持为图像的中心点。当CV_CALIB_USE_INTRINSIC_GUESS
                              参数被设置，保持为输入的值。
CV_CALIB_FIX_ASPECT_RATIO：固定fx/fy的比值，只将fy作为可变量，进行优化计算。当CV_CALIB_USE_INTRINSIC_GUESS没有被设置，
                           fx和fy的实际输入值将会被忽略，只有fx/fy的比值被计算和使用。
CV_CALIB_ZERO_TANGENT_DIST：切向畸变系数（P1，P2）被设置为零并保持为零。
CV_CALIB_FIX_K1,…,CV_CALIB_FIX_K6：对应的径向畸变系数在优化中保持不变。如果设置了CV_CALIB_USE_INTRINSIC_GUESS参数，
                                   就从提供的畸变系数矩阵中得到。否则，设置为0。
CV_CALIB_RATIONAL_MODEL（理想模型）：启用畸变k4，k5，k6三个畸变参数。使标定函数使用有理模型，返回8个系数。如果没有设置，
                                    则只计算其它5个畸变参数。
CALIB_THIN_PRISM_MODEL （薄棱镜畸变模型）：启用畸变系数S1、S2、S3和S4。使标定函数使用薄棱柱模型并返回12个系数。如果不
                                         设置标志，则函数计算并返回只有5个失真系数。
CALIB_FIX_S1_S2_S3_S4 ：优化过程中不改变薄棱镜畸变系数S1、S2、S3、S4。如果cv_calib_use_intrinsic_guess设置，使用提供
                        的畸变系数矩阵中的值。否则，设置为0。
CALIB_TILTED_MODEL （倾斜模型）：启用畸变系数tauX and tauY。标定函数使用倾斜传感器模型并返回14个系数。如果不设置标志，
                                则函数计算并返回只有5个失真系数。
CALIB_FIX_TAUX_TAUY ：在优化过程中，倾斜传感器模型的系数不被改变。如果cv_calib_use_intrinsic_guess设置，从提供的畸变
                      系数矩阵中得到。否则，设置为0。
函数返回: 重投影的总的均方根误差。
// 举个例子
// "../opencv/sources/samples/cpp/calibration.cpp"
```
# 矫正举证参数计算
```
void cv::calibrationMatrixValues (InputArray cameraMatrix, 
                                  Size imageSize, 
                                  double apertureWidth, 
                                  double apertureHeight, 
                                  double & fovx, 
                                  double & fovy, 
                                  double & focalLength, 
                                  Point2d & principalPoint, 
                                  double & aspectRatio)

// 后面五个是接收输出的参数
// 这个函数应该是用来做摄像头内部参数的估计测量
```
# 变换矩阵融合
```
void cv::composeRT (InputArray rvec1, 
                    InputArray tvec1, 
                    InputArray rvec2, 
                    InputArray tvec2, 
                    OutputArray rvec3, 
                    OutputArray tvec3, 
                    OutputArray dr3dr1 = noArray(), 
                    OutputArray dr3dt1 = noArray(), 
                    OutputArray dr3dr2 = noArray(), 
                    OutputArray dr3dt2 = noArray(), 
                    OutputArray dt3dr1 = noArray(), 
                    OutputArray dt3dt1 = noArray(), 
                    OutputArray dt3dr2 = noArray(), 
                    OutputArray dt3dt2 = noArray())

```
# 外极线的计算绘制
关于极线(epipolar line)的知识可以参见
[对极几何原理](https://blog.csdn.net/ssw_1990/article/details/53355572)和
[Epipolar geometry对极几何](https://blog.csdn.net/lin453701006/article/details/55096777)
```
void cv::computeCorrespondEpilines (InputArray points, 
                                    int whichImage, 
                                    InputArray F, 
                                    OutputArray lines)
// 简单的来理解这个函数的作用
// 第一，先假设我们有两个视角拍摄的某个物体的图片，我们一般可以找到很多相同的特征点
// 第二，根据两个链接博客的学习，我们知道这些特征点可以构造出很多极线
// 第三，假设第一张图片我们有points个特征点，第一张图片的话whichImage就是1
// 第四，我们想计算这些点在第二张照片中的极线
// 第五，F是一个对应矩阵，可以通过findFundamentalMat函数得到
// 第六，函数的最后一个参数就是用来接收这些极线的，形式的话是vector<3f>，3f表示三个参数表示一条直线
// 也就是说ax+by+c=0，不过需要记住，参数是a*a+b*b=1

// 举个例子，先放一个别人的例子，等理解极线的奥妙之后再研究
// https://blog.csdn.net/shyjhyp11/article/details/66526685

```
# convertPointsFromHomogeneous
我们在数学中经常用到的是就是笛卡尔或者欧几里得坐标，关于齐次坐标参见[齐次坐标](https://blog.csdn.net/VenoBling/article/details/87794400)
```
void cv::convertPointsFromHomogeneous (InputArray src, 
                                       OutputArray dst)

```
# convertPointsHomogeneous
```
void cv::convertPointsHomogeneous (InputArray src, 
                                   OutputArray dst)
// 相当于前后两个函数的共用版本
```
# convertPointsToHomogeneous
```
void cv::convertPointsToHomogeneous (InputArray src, 
                                     OutputArray dst)
// 顾名思义，抱歉，现在还没仔细研究怎么用，以后再看
```
# correctMatches
```
void cv::correctMatches (InputArray F, 
                         InputArray points1, 
                         InputArray points2, 
                         OutputArray newPoints1, 
                         OutputArray newPoints2)
// 猜测主要目的是为了对矫正的结果进行优化调整，比如说优化出来的结果point1和point2是两张图对应的点集
// F是关于描述极线约束，这个函数的根本目的是在满足极线约束的情况下，调整两组点的位置，使得两组距离最小
```
# decomposeEssentialMat
```
void cv::decomposeEssentialMat (InputArray E, 
                                OutputArray R1, 
                                OutputArray R2, 
                                OutputArray t)
// 目的应该是将一个矩阵E分解成可能的旋转和平移矩阵
// 这里没搞懂为什么要分成两个可能的旋转矩阵
```
# decomposeHomographyMat
```
int cv::decomposeHomographyMat (InputArray H, 
                                InputArray K, 
                                OutputArrayOfArrays rotations, 
                                OutputArrayOfArrays translations, 
                                OutputArrayOfArrays normals)
// 将一个其次矩阵转化为三个部分（旋转，平移，正交平面）
```
# decomposeProjectionMatrix
```
void cv::decomposeProjectionMatrix (InputArray projMatrix, 
                                    OutputArray cameraMatrix, 
                                    OutputArray rotMatrix, 
                                    OutputArray transVect, 
                                    OutputArray rotMatrixX = noArray(), 
                                    OutputArray rotMatrixY = noArray(), 
                                    OutputArray rotMatrixZ = noArray(), 
                                    OutputArray eulerAngles = noArray())
//也是某种拆分方法，
// 目前也不是很了解为什么要做这种拆分，以后边学边了解
```
# drawChessboardCorners
```
void cv::drawChessboardCorners (InputOutputArray image, 
                                Size patternSize, 
                                InputArray corners, 
                                bool patternWasFound)
// 画出找到的棋盘角点
image：用作输出的画布
patterSize：棋盘的规格，横纵的格数
corners：检测到的棋盘角点，就是findChessboardCorners的输出
patternWasfound：指示是否找到完整的棋盘
```
# estimateAffine2D
```
cv::Mat cv::estimateAffine2D (InputArray from, 
                              InputArray to, 
                              OutputArray inliers = noArray(), 
                              int method = RANSAC, 
                              double ransacReprojThreshold = 3, 
                              size_t maxIters = 2000, 
                              double confidence = 0.99, 
                              size_t refineIters = 10)
// 找到两个点之间最好的仿射函数
```
# estimateAffine3D
```
int cv::estimateAffine3D (InputArray src, 
                          InputArray dst, 
                          OutputArray out, 
                          OutputArray inliers, 
                          double ransacThreshold = 3, 
                          double confidence = 0.99)
                          
// 和上面的相同，只不过换成的三维的点
```
# estimateAffinePartial2D
```
cv::Mat cv::estimateAffinePartial2D (InputArray from, 
                                     InputArray to, 
                                     OutputArray inliers = noArray(), 
                                     int method = RANSAC, 
                                     double ransacReprojThreshold = 3, 
                                     size_t maxIters = 2000, 
                                     double confidence = 0.99, 
                                     size_t refineIters = 10)
// 贴一张别人的例子

std::vector<cv::Point> p1s,p2s;

p1s.push_back(cv::Point( 100, 0));
p1s.push_back(cv::Point( 0, 100));
p1s.push_back(cv::Point(-100, 0));
p1s.push_back(cv::Point( 0,-100));

p2s.push_back(cv::Point(71, -71));
p2s.push_back(cv::Point(71, 71));
p2s.push_back(cv::Point(-71, 71));
p2s.push_back(cv::Point(-71, -71));

// 1.
cv::Mat t_false = cv::estimateRigidTransform(p1s,p2s,false);
std::cout << "estimateRigidTransform false: " << t_false << "\n" << std::flush;

// 2.
cv::Mat t_true = cv::estimateRigidTransform(p1s,p2s,true);
std::cout << "estimateRigidTransform true:" << t_true << "\n" << std::flush;

// 3.
std::vector<uchar> inliers(p1s.size(), 0);
cv::Mat affine1 = cv::estimateAffine2D(p1s, p2s, inliers);
std::cout << "estimateAffine2D" << affine1 << "\n" << std::flush;

// 4.
cv::Mat affine2 = cv::estimateAffinePartial2D(p1s, p2s, inliers);
std::cout << "estimateAffinePartial2D" << affine2 << "\n" << std::flush;
```
# filterHomographyDecompByVisibleRefpoints
```
void cv::filterHomographyDecompByVisibleRefpoints (InputArrayOfArrays rotations, 
                                                   InputArrayOfArrays normals, 
                                                   InputArray beforePoints, 
                                                   InputArray afterPoints, 
                                                   OutputArray possibleSolutions, 
                                                   InputArray pointsMask = noArray())
// 并没有搞懂这是用来做什么的
// 初步觉得是对decomposeHomographyMat得到的方案进行过滤，得到那个才是真正的解决方案
```
# filterSpeckles
```
void cv::filterSpeckles (InputOutputArray img, 
                         double newVal, 
                         int maxSpeckleSize, 
                         double maxDiff, 
                         InputOutputArray buf = noArray())
// 用于过滤不同块的小斑点
// 尝试了多种参数设置，但是任然没有发现具体的用法
// 留下能成功跑的程序片段，日后再研究

Mat src = imread("D://data//block.png", 0);
imshow("原图", src);
Mat src16;
src.convertTo(src16, CV_16S, 256, -32768);
cout << src16.depth() << endl;
imshow("原图16", src16);
filterSpeckles(src16, 10000000, 200, 100);
imshow("原图16_f", src16);
waitKey(0);
```
# find4QuadCornerSubpix
```
bool cv::find4QuadCornerSubpix (InputArray img, InputOutputArray corners, Size region_size)
// 看到subpix就应该知道这是一种降低角点查找的偏差
第一个参数image，输入的Mat矩阵，最好是8位灰度图像，检测效率更高；

第二个参数corners，初始的角点坐标向量，同时作为亚像素坐标位置的输出，所以需要是浮点型数据，一般用元素
                  是Pointf2f/Point2d的向量来表示：vector<Point2f/Point2d> iamgePointsBuf；

第三个参数winSize，大小为搜索窗口的一半；

第四个参数zeroZone，死区的一半尺寸，死区为不对搜索区的中央位置做求和运算的区域。它是用来避免自相关矩阵
                   出现某些可能的奇异性。当值为（-1，-1）时表示没有死区；

第五个参数criteria，定义求角点的迭代过程的终止条件，可以为迭代次数和角点精度两者的组合；

```
# findChessboardCorners
```
bool cv::findChessboardCorners (InputArray image, 
                                Size patternSize, 
                                OutputArray corners, 
                                int flags = CALIB_CB_ADAPTIVE_THRESH+CALIB_CB_NORMALIZE_IMAGE)
这个函数用来初步寻找图像中可能存在的corners，如果找到了就返回true，否则false
贴出官方提供的例子
Size patternsize(8,6); //interior number of corners
Mat gray = ....; //source image
vector<Point2f> corners; //this will be filled by the detected corners

//CALIB_CB_FAST_CHECK saves a lot of time on images
//that do not contain any chessboard corners
bool patternfound = findChessboardCorners(gray, patternsize, corners,
        CALIB_CB_ADAPTIVE_THRESH + CALIB_CB_NORMALIZE_IMAGE
        + CALIB_CB_FAST_CHECK);

if(patternfound)
  cornerSubPix(gray, corners, Size(11, 11), Size(-1, -1),
    TermCriteria(CV_TERMCRIT_EPS + CV_TERMCRIT_ITER, 30, 0.1));

drawChessboardCorners(img, patternsize, Mat(corners), patternfound);
```
# findCirclesGrid
```
bool cv::findCirclesGrid (InputArray image, 
                          Size patternSize, 
                          OutputArray centers, 
                          int flags, 
                          const Ptr< FeatureDetector > & blobDetector, 
                          CirclesGridFinderParameters parameters)

bool cv::findCirclesGrid (InputArray image, 
                          Size patternSize, 
                          OutputArray centers, 
                          int flags = CALIB_CB_SYMMETRIC_GRID, 
                          const Ptr< FeatureDetector > & blobDetector = SimpleBlobDetector::create())
// 这前后三个函数都是以圆点标定板为参准进行相机标定的，类似于利用棋盘来做标定
//贴出官方提供的例子
Size patternsize(7,7); //number of centers
Mat gray = ....; //source image
vector<Point2f> centers; //this will be filled by the detected centers

bool patternfound = findCirclesGrid(gray, patternsize, centers);

drawChessboardCorners(img, patternsize, Mat(centers), patternfound);
```
# findCirclesGrid2
```
bool cv::findCirclesGrid2 (InputArray image, 
                           Size patternSize, 
                           OutputArray centers, 
                           int flags, 
                           const Ptr< FeatureDetector > & blobDetector, 
                           CirclesGridFinderParameters2 parameters)

```
关于双目相机的几种矩阵的介绍参见[基本矩阵、本质矩阵和单应矩阵](https://blog.csdn.net/kokerf/article/details/72191054)
# findEssentialMat
```
Mat cv::findEssentialMat (InputArray points1, 
                          InputArray points2, 
                          InputArray cameraMatrix, 
                          int method = RANSAC, 
                          double prob = 0.999, 
                          double threshold = 1.0, 
                          OutputArray mask = noArray())

Mat cv::findEssentialMat (InputArray points1, 
                          InputArray points2, 
                          double focal = 1.0, 
                          Point2d pp = Point2d(0, 0), 
                          int method = RANSAC, 
                          double prob = 0.999, 
                          double threshold = 1.0, 
                          OutputArray mask = noArray())

```
# findFundamentalMat
```
Mat cv::findFundamentalMat (InputArray points1, 
                            InputArray points2, 
                            int method = FM_RANSAC, 
                            double ransacReprojThreshold = 3., 
                            double confidence = 0.99, 
                            OutputArray mask = noArray())

Mat cv::findFundamentalMat (InputArray points1, 
                            InputArray points2, 
                            OutputArray mask, 
                            int method = FM_RANSAC, 
                            double ransacReprojThreshold = 3., 
                            double confidence = 0.99)

// 贴出官方的例子
// Example. Estimation of fundamental matrix using the RANSAC algorithm
int point_count = 100;
vector<Point2f> points1(point_count);
vector<Point2f> points2(point_count);

// initialize the points here ...
for( int i = 0; i < point_count; i++ )
{
    points1[i] = ...;
    points2[i] = ...;
}

Mat fundamental_matrix =
 findFundamentalMat(points1, points2, FM_RANSAC, 3, 0.99);

```
# findHomography
```
Mat cv::findHomography (InputArray srcPoints, 
                        InputArray dstPoints, 
                        int method = 0, 
                        double ransacReprojThreshold = 3, 
                        OutputArray mask = noArray(), 
                        const int maxIters = 2000, 
                        const double confidence = 0.995)

Mat cv::findHomography (InputArray srcPoints, 
                        InputArray dstPoints, 
                        OutputArray mask, 
                        int method = 0, 
                        double ransacReprojThreshold = 3)

// 找到两个平面的透射变化矩阵
```
# getOptimalNewCameraMatrix
```
Mat cv::getOptimalNewCameraMatrix (InputArray cameraMatrix, 
                                   InputArray distCoeffs, 
                                   Size imageSize, 
                                   double alpha, 
                                   Size newImgSize = Size(), 
                                   Rect * validPixROI = 0, 
                                   bool centerPrincipalPoint = false)
// 得到自由比例参数的新摄像机矩阵
```
# getValidDisparityROI
```
Rect cv::getValidDisparityROI (Rect roi1, 
                               Rect roi2, 
                               int minDisparity, 
                               int numberOfDisparities, 
                               int SADWindowSize)

```
# initCameraMatrix2D
```
Mat cv::initCameraMatrix2D (InputArrayOfArrays objectPoints, 
                            InputArrayOfArrays imagePoints, 
                            Size imageSize, 
                            double aspectRatio = 1.0)
// 得到一个初始比较粗糙的相机参数，输入分别是实际空间坐标和像空间坐标
```
# matMulDeriv
```
void cv::matMulDeriv (InputArray A, 
                      InputArray B, 
                      OutputArray dABdA, 
                      OutputArray dABdB)
```
# projectPoints
```
void cv::projectPoints (InputArray objectPoints, 
                        InputArray rvec, 
                        InputArray tvec, 
                        InputArray cameraMatrix, 
                        InputArray distCoeffs, 
                        OutputArray imagePoints, 
                        OutputArray jacobian = noArray(), 
                        double aspectRatio = 0)

// 将一个三维的点投影到像平面中
```
# recoverPose
```
int cv::recoverPose (InputArray E, 
                     InputArray points1, 
                     InputArray points2, 
                     InputArray cameraMatrix, 
                     OutputArray R, 
                     OutputArray t, 
                     InputOutputArray mask = noArray())

int cv::recoverPose (InputArray E, 
                     InputArray points1, 
                     InputArray points2, 
                     OutputArray R, 
                     OutputArray t, 
                     double focal = 1.0, 
                     Point2d pp = Point2d(0, 0), 
                     InputOutputArray mask = noArray())

int cv::recoverPose (InputArray E, 
                     InputArray points1, 
                     InputArray points2, 
                     InputArray cameraMatrix, 
                     OutputArray R, 
                     OutputArray t, 
                     double distanceThresh, 
                     InputOutputArray mask = noArray(), 
                     OutputArray triangulatedPoints = noArray())
// 一般结合findEssentialMat进行使用
// 贴一个官方提供的案例
// Example. Estimation of fundamental matrix using the RANSAC algorithm
int point_count = 100;
vector<Point2f> points1(point_count);
vector<Point2f> points2(point_count);

// initialize the points here ...
for( int i = 0; i < point_count; i++ )
{
    points1[i] = ...;
    points2[i] = ...;
}

// cametra matrix with both focal lengths = 1, and principal point = (0, 0)
Mat cameraMatrix = Mat::eye(3, 3, CV_64F);

Mat E, R, t, mask;

E = findEssentialMat(points1, points2, cameraMatrix, RANSAC, 0.999, 1.0, mask);
recoverPose(E, points1, points2, cameraMatrix, R, t, mask);
```
# rectify3Collinear
```
float cv::rectify3Collinear (InputArray cameraMatrix1, 
                             InputArray distCoeffs1, 
                             InputArray cameraMatrix2, 
                             InputArray distCoeffs2, 
                             InputArray cameraMatrix3, 
                             InputArray distCoeffs3, 
                             InputArrayOfArrays imgpt1, 
                             InputArrayOfArrays imgpt3, 
                             Size imageSize, 
                             InputArray R12, 
                             InputArray T12, 
                             InputArray R13, 
                             InputArray T13, 
                             OutputArray R1, 
                             OutputArray R2, 
                             OutputArray R3, 
                             OutputArray P1, 
                             OutputArray P2, 
                             OutputArray P3, OutputArray Q, 
                             double alpha, 
                             Size newImgSize, 
                             Rect * roi1, 
                             Rect * roi2, 
                             int flags)
// “三头”摄像头？的矫正
```
# reprojectImageTo3D
```
void cv::reprojectImageTo3D (InputArray disparity, 
                             OutputArray _3dImage, 
                             InputArray Q, 
                             bool handleMissingValues = false, 
                             int ddepth = -1)
// 将平面反投影到三维空间点
```
# Rodrigues
```
void cv::Rodrigues (InputArray src, 
                    OutputArray dst, 
                    OutputArray jacobian = noArray())
// 将旋转矩阵转化成旋转vector，或者相反的顺序
```
# RQDecomp3x3
```
Vec3d cv::RQDecomp3x3 (InputArray src, 
                       OutputArray mtxR, 
                       OutputArray mtxQ, 
                       OutputArray Qx = noArray(), 
                       OutputArray Qy = noArray(), 
                       OutputArray Qz = noArray())

```
# sampsonDistance
```
double cv::sampsonDistance (InputArray pt1, 
                            InputArray pt2, 
                            InputArray F)

```
# solveP3P
```
int cv::solveP3P (InputArray objectPoints, 
                  InputArray imagePoints, 
                  InputArray cameraMatrix, 
                  InputArray distCoeffs, 
                  OutputArrayOfArrays rvecs, 
                  OutputArrayOfArrays tvecs, 
                  int flags)

```
# solvePnP
```
bool cv::solvePnP (InputArray objectPoints, 
                   InputArray imagePoints, 
                   InputArray cameraMatrix, 
                   InputArray distCoeffs, 
                   OutputArray rvec, 
                   OutputArray tvec, 
                   bool useExtrinsicGuess = false, 
                   int flags = SOLVEPNP_ITERATIVE)

```
# solvePnPRansac
```
bool cv::solvePnPRansac (InputArray objectPoints, 
                         InputArray imagePoints, 
                         InputArray cameraMatrix, 
                         InputArray distCoeffs, 
                         OutputArray rvec, 
                         OutputArray tvec, 
                         bool useExtrinsicGuess = false, 
                         int iterationsCount = 100, 
                         float reprojectionError = 8.0, 
                         double confidence = 0.99, 
                         OutputArray inliers = noArray(), 
                         int flags = SOLVEPNP_ITERATIVE)

```
# stereoCalibrate
```
double cv::stereoCalibrate (InputArrayOfArrays objectPoints, 
                            InputArrayOfArrays imagePoints1, 
                            InputArrayOfArrays imagePoints2, 
                            InputOutputArray cameraMatrix1, 
                            InputOutputArray distCoeffs1, 
                            InputOutputArray cameraMatrix2, 
                            InputOutputArray distCoeffs2, 
                            Size imageSize, 
                            InputOutputArray R, 
                            InputOutputArray T, 
                            OutputArray E, 
                            OutputArray F, 
                            OutputArray perViewErrors, 
                            int flags = CALIB_FIX_INTRINSIC, 
                            TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, 1e-6))

double cv::stereoCalibrate (InputArrayOfArrays objectPoints, 
                            InputArrayOfArrays imagePoints1, 
                            InputArrayOfArrays imagePoints2, 
                            InputOutputArray cameraMatrix1, 
                            InputOutputArray distCoeffs1, 
                            InputOutputArray cameraMatrix2, 
                            InputOutputArray distCoeffs2, 
                            Size imageSize, 
                            OutputArray R, 
                            OutputArray T, 
                            OutputArray E, 
                            OutputArray F, 
                            int flags = CALIB_FIX_INTRINSIC, 
                            TermCriteria criteria = TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, 1e-6))

```
# stereoRectify
```
void cv::stereoRectify (InputArray cameraMatrix1, 
                        InputArray distCoeffs1, 
                        InputArray cameraMatrix2, 
                        InputArray distCoeffs2, 
                        Size imageSize, 
                        InputArray R, 
                        InputArray T, 
                        OutputArray R1, 
                        OutputArray R2, 
                        OutputArray P1, 
                        OutputArray P2, 
                        OutputArray Q, 
                        int flags = CALIB_ZERO_DISPARITY, 
                        double alpha = -1, 
                        Size newImageSize = Size(), 
                        Rect * validPixROI1 = 0, 
                        Rect * validPixROI2 = 0)

```
# stereoRectifyUncalibrated
```
bool cv::stereoRectifyUncalibrated (InputArray points1, 
                                    InputArray points2, 
                                    InputArray F, 
                                    Size imgSize, 
                                    OutputArray H1, 
                                    OutputArray H2, 
                                    double threshold = 5)

```
# triangulatePoints
```
void cv::triangulatePoints (InputArray projMatr1, 
                            InputArray projMatr2, 
                            InputArray projPoints1, 
                            InputArray projPoints2, 
                            OutputArray points4D)

```
# validateDisparity
```
void cv::validateDisparity (InputOutputArray disparity, 
                            InputArray cost, 
                            int minDisparity, 
                            int numberOfDisparities, 
                            int disp12MaxDisp = 1)

```
