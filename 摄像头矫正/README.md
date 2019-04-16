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

```
