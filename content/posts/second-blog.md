+++
date = 2022-09-16
title = "YoloV5 Deployment Optimization with Neural Magic"
description = ""
slug = ""
authors = ['Fabio Geraci']
tags = ['YOLO', 'Neural Magic', 'model', 'optimization']
categories = []
externalLink = ""
series = []
+++

🔥 Motivation:

- As architecture get more complex, for improved prediction results, the generated model are larger and larger, therefore the need to reduce their size and allow the model to run prediction on CPU. This enables developer to reduce infostructres costs as well as opening the door for edge deployment.

What:

- I used a blood cells photos, originally open sourced by [cosmicad](https://github.com/cosmicad/dataset), downloaded from Roboflow Universe (bccd dataset).

Why:

- I am true beliver that Science and Technology should be used for the benefit of the many, the Medical Industry is and will benefit greatly from all the AI/ML application in development.

How:

- In this post, I show you how you can supercharge your YOLOv5 inference performance running on CPUs using free and open-source tools by Neural Magic.

### Yolov5s

- Average inference time per frame: 148.58 ms
- Average FPS: 6.73

### Yolov5s Pruned+Quant

- Average inference time per frame: 114.55 ms
- Average FPS: 8.72

### Yolov5n Pruned+Quant

- Average inference time per frame: 55.62012081549351 ms
- Average FPS: 17.97


![Screenshotfrom20220916223617.png](assets/Screenshot from 2022-09-16 22-36-17.png?t=1663364377805)
