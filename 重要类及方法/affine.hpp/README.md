# cv::Affine3< T >
```
typedef Affine3< double > cv::Affine3d
typedef Affine3< float > cv::Affine3f
typedef T float_type
typedef Matx< float_type, 3, 3 > Mat3
typedef Matx< float_type, 4, 4 > Mat4
typedef Vec< float_type, 3 > Vec3
```
> 创建方法
```
// 方法一
cv::Vec3f r, t;
cv::Affine3f T(r, t);
// 方法二
cv::Matx33f R;
cv::Affine3f T(R, t);
```
> 获得某些属性
```
cv::Matx33f R = T.rotation();
cv::Vec3f t = T.translation();
cv::Vec3f r = T.rvec();
```
> 融合
```
cv::Affine3f T, T1, T2;
T = T2.concatenate(T1);
```
> 逆
```
cv::Affine3f T, T_inv;
T_inv = T.inv();
```
