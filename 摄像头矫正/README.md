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
stdDeviationsIntrinsics ：内参数的输出向量。输出顺序为: (fx,fy,cx,cy,k1,k2,p1,p2,k3,k4,k5,k6,s1,s2,s3,s4,τx,τy) ，如果不估计其中某一个参数，值等于0
stdDeviationsExtrinsics ：外参数的输出向量。输出顺序: (R1,T1,…,RM,TM) ，M是标定图片的个数, Ri,Ti 是1x3的向量 。
perViewErrors   每个标定图片的重投影均方根误差的输出向量。
criteria：   迭代优化算法的终止准则
flags ：标定函数是所采用的模型（重点）”。 
可输入如下某个或者某几个参数：
CV_CALIB_USE_INTRINSIC_GUESS：使用该参数时，将包含有效的fx,fy,cx,cy的估计值的内参矩阵cameraMatrix，作为初始值输入，然后函数对其做进一步优化。如果不使用这个参数，用图像的中心点初始化光轴点坐标(cx, cy)，使用最小二乘估算出fx，fy（这种求法好像和张正友的论文不一样，不知道为何要这样处理）。注意，如果已知内部参数（内参矩阵和畸变系数），就不需要使用这个函数来估计外参，可以使用solvepnp()函数计算外参数矩阵。
CV_CALIB_FIX_PRINCIPAL_POINT：在进行优化时会固定光轴点，光轴点将保持为图像的中心点。当CV_CALIB_USE_INTRINSIC_GUESS参数被设置，保持为输入的值。
CV_CALIB_FIX_ASPECT_RATIO：固定fx/fy的比值，只将fy作为可变量，进行优化计算。当CV_CALIB_USE_INTRINSIC_GUESS没有被设置，fx和fy的实际输入值将会被忽略，只有fx/fy的比值被计算和使用。
CV_CALIB_ZERO_TANGENT_DIST：切向畸变系数（P1，P2）被设置为零并保持为零。
CV_CALIB_FIX_K1,…,CV_CALIB_FIX_K6：对应的径向畸变系数在优化中保持不变。如果设置了CV_CALIB_USE_INTRINSIC_GUESS参数，就从提供的畸变系数矩阵中得到。否则，设置为0。
CV_CALIB_RATIONAL_MODEL（理想模型）：启用畸变k4，k5，k6三个畸变参数。使标定函数使用有理模型，返回8个系数。如果没有设置，则只计算其它5个畸变参数。
CALIB_THIN_PRISM_MODEL （薄棱镜畸变模型）：启用畸变系数S1、S2、S3和S4。使标定函数使用薄棱柱模型并返回12个系数。如果不设置标志，则函数计算并返回只有5个失真系数。
CALIB_FIX_S1_S2_S3_S4 ：优化过程中不改变薄棱镜畸变系数S1、S2、S3、S4。如果cv_calib_use_intrinsic_guess设置，使用提供的畸变系数矩阵中的值。否则，设置为0。
CALIB_TILTED_MODEL （倾斜模型）：启用畸变系数tauX and tauY。标定函数使用倾斜传感器模型并返回14个系数。如果不设置标志，则函数计算并返回只有5个失真系数。
CALIB_FIX_TAUX_TAUY ：在优化过程中，倾斜传感器模型的系数不被改变。如果cv_calib_use_intrinsic_guess设置，从提供的畸变系数矩阵中得到。否则，设置为0。
函数返回: 重投影的总的均方根误差。
// 举个例子

```
