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
// 方法一：default方法，创建一个恒等变换
Affine3f T1;
// 方法二：
Matx33f R;
Affine3f T1(R);
// 方法三
cv::Vec3f r, t;
cv::Affine3f T(r, t);
// 方法四
cv::Matx33f R;
cv::Affine3f T(R, t);
```
> 获得某些属性
```
cv::Matx33f R = T.rotation();
cv::Vec3f t = T.translation();
cv::Vec3f r = T.rvec();
```
> 融合，叠加其他的变化
```
cv::Affine3f T, T1, T2;
T = T2.concatenate(T1);

T1 = T1.rotate(R)
T1 = T1.rotate(r)
T = T.translate(t)

```
> 逆
```
cv::Affine3f T, T_inv;
T_inv = T.inv();
```
>设置rotation
```
Affine3f T1;
Matx33f R;
Vec3f r;
T1.rotation(R)
// T1.rotation(r)
// T1.linear(R) 这一种没试，但是文档应该说是可以这样设置
```
> 设置为idetity
```
Vec3f r = Vec3f(1, 0, 0);
Affine3f T(r);
T = T.Identity();
```
> 设置transition
```
Vec3f t = Vec3f(1, 0, 0);
Affine3f T;
T.translation(t);
```
