___
Захватить изображение с камеры довольно легко:

Подготовка окружения:
```
pip3 install opencv-python-headless
```

>[!note]
>headless здесь видимо означает отсутствие работы с gui, что нам и нужно.

Код:
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import sys
import cv2

def main():
    """Webcam capturing pratice"""
    cam_port = 2
    cam = cv2.VideoCapture(cam_port)
    result, image = cam.read()
    if result:
        cv2.imwrite("/home/gene/Desktop/station/expl/py_expl/webcam1/im.png", image)
        print("Image saved")
    else:
        print("No image detected")

if __name__ == '__main__':
    sys.exit(main())
```