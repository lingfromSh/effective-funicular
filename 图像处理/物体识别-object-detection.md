---
title: "物体识别 Object Detection"
tags: ""
---

# 物体识别 Object Detection

## 分类器

## Face Detection

```python
import cv2
import os

BASE_DIR = os.path.dirname(os.path.realpath(__file__))

'''
    cv2.CascadeClassifier.detectMultiScale(image[, scaleFactor[, minNeighbors[, flags[, minSize[, maxSize]]]]]) → objects
    Parameters:
          image – Matrix of the type CV_8U containing an image where objects are detected.
          scaleFactor – Parameter specifying how much the image size is reduced at each image scale.
          minNeighbors – Parameter specifying how many neighbors each candidate rectangle should have to retain it.
          flags – Parameter with the same meaning for an old cascade as in the function cvHaarDetectObjects. It is not used for a new cascade.
          minSize – Minimum possible object size. Objects smaller than that are ignored.
          maxSize – Maximum possible object size. Objects larger than that are ignored.
          objects – Vector of rectangles where each rectangle contains the detected object, the rectangles may be partially outside the original image.
  '''

# NOTE: 使用文件作为视频输入
def detect(video_path):
    # 使用相对目录路径
    face_cascade = cv2.CascadeClassifier(os.path.join(BASE_DIR, "haarcascade_frontalface_default.xml"))
    eye_cascade = cv2.CascadeClassifier(os.path.join(BASE_DIR, "haarcascade_eye.xml"))
    
    camera = cv2.VideoCapture(video_path)
    while True:
        ret, frame = camera.read()
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        faces = face_cascade.detectMultiScale(gray, 1.3, 5)
        for (x, y, w, h) in faces:
            img = cv2.rectangle(frame, (x, y), (x + w, y + h), (255, 0, 0), 2)

            roi_gray = gray[y:y + h, x:x + w]

            eyes = eye_cascade.detectMultiScale(roi_gray, 1.03, 5, 0, (80, 80))
            for (ex, ey, ew, eh) in eyes:
                cv2.rectangle(img, (x + ex, y + ey), (x + ex + ew, y + ey + eh), (0, 255, 0), 2)

        cv2.imshow("camera", frame)
        if cv2.waitKey(100) & 0xff == ord("q"):
            break
    cv2.destroyAllWindows()


```

![result](/home/ling/BoostNote/images/face-detection-frame-1.png)

### &lt;class 'cv2.CascadeClassifier'> returned a result with an error set3 错误

解决方案:

跑去github/gitee下载对应文件就行，网上的数据有误.

1.  Gitee下载地址 [gitee](https://gitee.com/mirrors/opencv/tree/master/data/haarcascades)

2.  Github下载地址 [github](https://github.com/opencv/opencv/tree/master/data/haarcascades)

## Adaboost + CascadeClassifier Face Detection

> github: <https://github.com/hjlin0515/face-detection>

遵循OOP非常清晰的思路

结果

![result-0](/home/ling/BoostNote/images/bioid-0.png)

![result-1](/home/ling/BoostNote/images/bio-1.png)
