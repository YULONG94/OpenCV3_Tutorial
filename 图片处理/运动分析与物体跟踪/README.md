# 这里的一些函数暂时用不到，所以先不学
# accumulate
```
void cv::accumulate (InputArray src, 
                     InputOutputArray dst, 
                     InputArray mask = noArray())
```
# accumulateProduct
```
void cv::accumulateProduct (InputArray src1, 
                            InputArray src2, 
                            InputOutputArray dst, 
                            InputArray mask = noArray())
```
# accumulateSquare
```
void cv::accumulateSquare (InputArray src, 
                           InputOutputArray dst, 
                           InputArray mask = noArray())
```
# accumulateWeighted
```
void cv::accumulateWeighted (InputArray src, 
                             InputOutputArray dst, 
                             double alpha, 
                             InputArray mask = noArray())
```
# createHanningWindow
```
void cv::createHanningWindow (OutputArray dst, 
                              Size winSize, 
                              int type)
```
# phaseCorrelate
```
Point2d cv::phaseCorrelate (InputArray src1, 
                            InputArray src2, 
                            InputArray window = noArray(), 
                            double * response = 0)
```
