
# tensorflow-yolov4-tflite
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](LICENSE)

This repo is a fork of https://github.com/hunglc007/tensorflow-yolov4-tflite, but added the part of training and some detailed instructions.

According to the original author, the repo can convert YOLO v4, YOLOv3, YOLO tiny .weights to .pb, .tflite and trt format for tensorflow, tensorflow lite, tensorRT.


See Yolo_BloodCellDet.ipynb for step 1-4 and folder android is the demo code to create the app.

1. select model
2. prepare data
3. train the model
4. export to tflite
5. create the android app



---
yolo makes prediction at 3 levels: 32, 16 and 8 pixels.
- yolo v4 only makes prediction at 2 levels: 32 and 16

Prediction: YOLOv3 and v4 make prediction at 3 scales:

- Stride 32, 16 and 8
    - 32: The last feature map layer
    - Then it goes back 2 layers back and upsamples it by 2.
- YOLOv3 then takes a feature map with higher resolution and merge it with the upsampled feature map using element-wise addition. YOLOv3 apply convolutional filters on the merged map to make the second set of predictions.
- For example, for an image of 416x416, it generates the following feature maps for prediction
    - 416/32→13x13
    - 416/16→26x26
    - 416/8→52x52
- It will generate a total of ((52 x 52) + (26 x 26) + 13 x 13)) x 3 = **10647 bounding boxes**
    - ❓3 bounding boxes for each grid? 3 anchors per each scale.
For tiny yolo, It will generate a total of ((26 x 26) + 13 x 13)) x 3 = **2535 bounding boxes**

NUM_OUTPUTS: is the number of outputs.