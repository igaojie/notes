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

## 训练



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



