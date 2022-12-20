# YOLO

## 参考资料

1. https://github.com/ultralytics/yolov5
2. https://github.com/raedle/YOLOExample
3. https://github.com/heartexlabs/labelImg

## 名词

1. map：综合衡量检测效果
2. IOU ： Area Of Overlap / Area of Union 交集/并集 

## 标注

### LabelImg

### 安装

### 标注

### 自动标注

**通过预先训练的模型自动标注**

```shell
python detect.py -h
usage: detect.py [-h] [--weights WEIGHTS [WEIGHTS ...]] [--source SOURCE]
                 [--data DATA] [--imgsz IMGSZ [IMGSZ ...]]
                 [--conf-thres CONF_THRES] [--iou-thres IOU_THRES]
                 [--max-det MAX_DET] [--device DEVICE] [--view-img]
                 [--save-txt] [--save-conf] [--save-crop] [--nosave]
                 [--classes CLASSES [CLASSES ...]] [--agnostic-nms]
                 [--augment] [--visualize] [--update] [--project PROJECT]
                 [--name NAME] [--exist-ok] [--line-thickness LINE_THICKNESS]
                 [--hide-labels] [--hide-conf] [--half] [--dnn]
                 [--vid-stride VID_STRIDE]

optional arguments:
  -h, --help            show this help message and exit
  --weights WEIGHTS [WEIGHTS ...]
                        model path or triton URL # 模型路径
  --source SOURCE       file/dir/URL/glob/screen/0(webcam) # 预测数据源
  --data DATA           (optional) dataset.yaml path
  --imgsz IMGSZ [IMGSZ ...], --img IMGSZ [IMGSZ ...], --img-size IMGSZ [IMGSZ ...]
                        inference size h,w
  --conf-thres CONF_THRES
                        confidence threshold
  --iou-thres IOU_THRES
                        NMS IoU threshold
  --max-det MAX_DET     maximum detections per image
  --device DEVICE       cuda device, i.e. 0 or 0,1,2,3 or cpu
  --view-img            show results
  --save-txt            save results to *.txt ##将预测的bounding box保存为txt文件
  --save-conf           save confidences in --save-txt labels
  --save-crop           save cropped prediction boxes #将预测的bounding box截取出来
  --nosave              do not save images/videos
  --classes CLASSES [CLASSES ...]
                        filter by class: --classes 0, or --classes 0 2 3
  --agnostic-nms        class-agnostic NMS
  --augment             augmented inference
  --visualize           visualize features
  --update              update all models
  --project PROJECT     save results to project/name
  --name NAME           save results to project/name
  --exist-ok            existing project/name ok, do not increment
  --line-thickness LINE_THICKNESS
                        bounding box thickness (pixels)
  --hide-labels         hide labels
  --hide-conf           hide confidences
  --half                use FP16 half-precision inference
  --dnn                 use OpenCV DNN for ONNX inference
  --vid-stride VID_STRIDE
                        video frame-rate stride


python detect.py --weights ./runs/train/exp4/weights/best.pt --source ./Downloads/2022-10-19/images --save-txt  --save-crop

将自动标注的数据写入到 runs/detect/expXX/labels 下。

```





## 训练

https://app.roboflow.com/



## 导出

https://github.com/ultralytics/yolov5/issues/251



## Custom YOLOV5 Model

yolov5训练出来的best.pt 无法直接用于移动端的调用，需要转换成支持移动端的模型文件。(这个费了老劲了)

```python
import torch
from torch.utils.mobile_optimizer import optimize_for_mobile
model = torch.hub.load('.', 'custom', path='./runs/train/exp4/weights/last.pt', source='local')

torchscript_model = './runs/train/exp4/weights/best.torchscript'
export_model_name = './runs/train/exp4/weights/best.torchscript.ptl'

model = torch.jit.load(torchscript_model)
optimized_model = optimize_for_mobile(model)
optimized_model._save_for_lite_interpreter(export_model_name)


参考:
  https://github.com/raedle/YOLOExample Optimize model for PyTorch Mobile use
```



