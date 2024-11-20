# 基于dpt模型实现单目深度估计（Monocular Depth Estimation）**和**语义分割（Semantic Segmentation）

## **注意事项**

1. 本案例使用AI引擎：**MindSpore-2.3.1**；
2. 本案例使用 **ASCEND** 环境运行，请查看[《ModelArts JupyterLab 硬件规格使用指南》](https://marketplace.huaweicloud.com/markets/aihub/article/detail/?content_id=638c55c5-816d-421c-9b5f-90c804b1fa6b)了解切换硬件规格的方法；
3. 如果您是第一次使用 JupyterLab，请查看[《ModelArts JupyterLab使用指导》](https://marketplace.huaweicloud.com/markets/aihub/article/detail/?content_id=03676d0a-0630-4a3f-b62c-07fba43d2857)了解使用方法；

4. 如果您在使用 JupyterLab 过程中碰到报错，请参考[《ModelArts JupyterLab常见问题解决办法》](https://marketplace.huaweicloud.com/markets/aihub/article/detail/?content_id=9ad8ce7d-06f7-4394-80ef-4dbf6cfb4be1)尝试解决问题。

在实际场景中，为了减少从头开始训练所带来的时间成本，大多数情况下会基于已有的模型来进行迁移学习。本章将会以狗和狼的图像分类为例，讲解如何在MindSpore中加载预训练模型，并通过固定权重来实现迁移学习的目的。

## 单目深度估计（Monocular Depth Estimation）



## 语义分割（Semantic Segmentation）

```py
import mindspore
from mindnlp.transformers import DPTForSemanticSegmentation,DPTImageProcessor
processor = DPTImageProcessor.from_pretrained("Intel/dpt-large-ade")
# model = DPTForSemanticSegmentation.from_pretrained("Intel/dpt-large-ade")
model = DPTForSemanticSegmentation.from_pretrained("Intel/dpt-large-ade")

from PIL import Image
import requests

image = Image.open("ADE_val_00000001.jpg")

inputs = processor(images=image, return_tensors="ms")
outputs = model(**inputs)
logits = outputs.logits

prediction = mindspore.ops.interpolate(
                logits, 
                size=image.size[::-1], 
                mode="bicubic", 
                align_corners=False
            )
prediction = mindspore.ops.Argmax(axis=1, output_type=mindspore.int32)(prediction) +1

adepallete = [0,0,0,120,120,120,180,120,120,6,230,230,80,50,50,4,200,3,120,120,80,140,140,140,204,5,255,230,230,230,4,250,7,224,5,255,235,255,7,150,5,61,120,120,70,8,255,51,255,6,82,143,255,140,204,255,4,255,51,7,204,70,3,0,102,200,61,230,250,255,6,51,11,102,255,255,7,71,255,9,224,9,7,230,220,220,220,255,9,92,112,9,255,8,255,214,7,255,224,255,184,6,10,255,71,255,41,10,7,255,255,224,255,8,102,8,255,255,61,6,255,194,7,255,122,8,0,255,20,255,8,41,255,5,153,6,51,255,235,12,255,160,150,20,0,163,255,140,140,140,250,10,15,20,255,0,31,255,0,255,31,0,255,224,0,153,255,0,0,0,255,255,71,0,0,235,255,0,173,255,31,0,255,11,200,200,255,82,0,0,255,245,0,61,255,0,255,112,0,255,133,255,0,0,255,163,0,255,102,0,194,255,0,0,143,255,51,255,0,0,82,255,0,255,41,0,255,173,10,0,255,173,255,0,0,255,153,255,92,0,255,0,255,255,0,245,255,0,102,255,173,0,255,0,20,255,184,184,0,31,255,0,255,61,0,71,255,255,0,204,0,255,194,0,255,82,0,10,255,0,112,255,51,0,255,0,194,255,0,122,255,0,255,163,255,153,0,0,255,10,255,112,0,143,255,0,82,0,255,163,255,0,255,235,0,8,184,170,133,0,255,0,255,92,184,0,255,255,0,31,0,184,255,0,214,255,255,0,112,92,255,0,0,224,255,112,224,255,70,184,160,163,0,255,153,0,255,71,255,0,255,0,163,255,204,0,255,0,143,0,255,235,133,255,0,255,0,235,245,0,255,255,0,122,255,245,0,10,190,212,214,255,0,0,204,255,20,0,255,255,255,0,0,153,255,0,41,255,0,255,204,41,0,255,41,255,0,173,0,255,0,245,255,71,0,255,122,0,255,0,255,184,0,92,255,184,255,0,0,133,255,255,214,0,25,194,194,102,255,0,92,0,255]

predicted_seg = Image.fromarray(prediction.squeeze().asnumpy().astype('uint8') )
predicted_seg.putpalette(adepallete)

def extract_category_image(prediction, category_id, original_image):
    # 创建一个空白图像，背景为黑色（或者透明）
    result_image = np.zeros((original_image.shape[0], original_image.shape[1], 3), dtype=np.uint8)
    
    # 找到所有属于该类别的像素点，并保留原图对应的像素值
    mask = prediction == category_id
    for c in range(3):  # 处理 R, G, B 三个通道
        result_image[:, :, c] = np.where(mask, np.array(original_image)[:, :, c], 0)
    
    # 将数组转换为图像
    return Image.fromarray(result_image)

# 获取类别列表
categories_in_image = np.unique(prediction.asnumpy())

categories_in_image

# 遍历每个类别，提取对应的图像
for category_id in categories_in_image:
    # 跳过背景类别（背景通常为类别1或者类别0，具体根据ADE数据集设定）
    if category_id == 0:
        continue
    category_image = extract_category_image(prediction.squeeze().asnumpy(), category_id, np.array(image))
    
    # 保存图片到文件
    category_image.save(f"category_{category_id}.png")
```

