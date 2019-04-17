[官方文档入口](https://docs.opencv.org/3.4.2/d9/d0c/group__calib3d.html)
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
// F是关于描述极线约束，这个函数的根本目的是再满足极线约束的情况下，调整两组点的位置，使得两组距离最小
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
// 
```
# filterHomographyDecompByVisibleRefpoints
```
void cv::filterHomographyDecompByVisibleRefpoints (InputArrayOfArrays rotations, 
                                                   InputArrayOfArrays normals, 
                                                   InputArray beforePoints, 
                                                   InputArray afterPoints, 
                                                   OutputArray possibleSolutions, 
                                                   InputArray pointsMask = noArray())

```
# filterSpeckles
```
void cv::filterSpeckles (InputOutputArray img, 
                         double newVal, 
                         int maxSpeckleSize, 
                         double maxDiff, 
                         InputOutputArray buf = noArray())

```
# find4QuadCornerSubpix
```
bool cv::find4QuadCornerSubpix (InputArray img, InputOutputArray corners, Size region_size)

```
# findChessboardCorners
```
bool cv::findChessboardCorners (InputArray image, 
                                Size patternSize, 
                                OutputArray corners, 
                                int flags = CALIB_CB_ADAPTIVE_THRESH+CALIB_CB_NORMALIZE_IMAGE)

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
```
# recoverPose
```
int cv::recoverPose (InputArray E, 
                     InputArray points1, 
                     InputArray points2, 
                     InputArray cameraMatrix, 
# 
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

```
# reprojectImageTo3D
```
void cv::reprojectImageTo3D (InputArray disparity, 
                             OutputArray _3dImage, 
                             InputArray Q, 
                             bool handleMissingValues = false, 
                             int ddepth = -1)

```
# Rodrigues
```
void cv::Rodrigues (InputArray src, 
                    OutputArray dst, 
                    OutputArray jacobian = noArray())

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
