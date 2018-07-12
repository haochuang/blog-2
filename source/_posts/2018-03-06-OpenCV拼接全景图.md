---
title: OpenCV拼接全景图
date: 2018-03-06 20:18:54
---

OpenCV自带了图像拼接算法stitch，而且效果还不错。

![](https://img-1256819794.cos.ap-beijing.myqcloud.com/20180306a.jpg)

<!-- more -->

``` Python
import glob

import cv2

st = cv2.createStitcher()
STITCH_DIR = '/home/wjmr/GitHub/opencv_extra/testdata/stitching/'
imgs = [cv2.imread(f) for f in glob.glob(STITCH_DIR + 'boat*')]
result = st.stitch(imgs)
cv2.imwrite("result.jpg", result[1])
cv2.namedWindow('demo', cv2.WINDOW_GUI_NORMAL)
cv2.imshow('demo', cv2.imread('result.jpg'))
cv2.waitKey(0)
cv2.destroyAllWindows()
```

使用了[opencv_extra](https://github.com/opencv/opencv_extra)里的图片。
