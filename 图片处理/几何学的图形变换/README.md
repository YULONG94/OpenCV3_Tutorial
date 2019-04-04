> cv::InterpolationFlags
cv::INTER_NEAREST = 0, 
cv::INTER_LINEAR = 1, 
cv::INTER_CUBIC = 2, 
cv::INTER_AREA = 3, 
cv::INTER_LANCZOS4 = 4, 
cv::INTER_LINEAR_EXACT = 5, 
cv::INTER_MAX = 7, 
cv::WARP_FILL_OUTLIERS = 8, 
cv::WARP_INVERSE_MAP = 16,
> cv::InterpolationMasks
cv::INTER_BITS = 5, 
cv::INTER_BITS2 = INTER_BITS * 2, 
cv::INTER_TAB_SIZE = 1 << INTER_BITS, 
cv::INTER_TAB_SIZE2 = INTER_TAB_SIZE * INTER_TAB_SIZE,
> cv::WarpPolarMode
cv::WARP_POLAR_LINEAR = 0, 
cv::WARP_POLAR_LOG = 256,

# convertMaps
```
void cv::convertMaps (InputArray map1, 
                      InputArray map2, 
                      OutputArray dstmap1, 
                      OutputArray dstmap2, 
                      int dstmap1type, 
                      bool nninterpolation = false)

```
