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

{{< toc >}}

## 🔥 Motivation:

- As architecture get more complex, for improved prediction results, the generated model are larger and larger, therefore the need to reduce their size and allow the model to run prediction on CPU. This enables developer to reduce infostructres costs as well as opening the door for edge deployment.

## 🔫 What:

- I used a blood cells photos, originally open sourced by [cosmicad](https://github.com/cosmicad/dataset), downloaded from Roboflow Universe (bccd dataset).

## 🚒 Why:

- I am true beliver that Science and Technology should be used for the benefit of the many, the Medical Industry is and will benefit greatly from all the AI/ML application in development.

## ⛳ How:

- In this post, I show you how you can supercharge your YOLOv5 inference performance running on CPUs using free and open-source tools by Neural Magic.

## 🔩 Setting Up

I followed the excellent blog [Supercharging YOLOv5](https://dicksonneoh.com/portfolio/supercharging_yolov5_180_fps_cpu/) from [Dickson Noah](https://dicksonneoh.com/)

<style>
table, tr, th, td {
  border: 1px solid black;
}
</style>

## Table Resume Prediction Speed

The table below clearly shows that the Yolov5n Pruned+Quant average FPS is 2.5 times faster than the standard YOLOv5s, and
roughly double respect Yolov5s Pruned+Quant, almost equaling the performance of its gpu counterpart.

<table border>
    <tr>
        <th>Model</th>
        <th colspan="2">CPU</th>
        <th colspan="2">GPU</th>
    </tr>
    <tr>
        <th>
            <th>Average inference time per frame (ms)</th>
            <th>Average FPS</th>
            <th>Average inference time per frame (ms)</th>
            <th>Average FPS</th>
        </th>
    </tr>
    <tr>
        <th>Yolov5s</th>
        <th>148.58</th>
        <th>6.73</th>
        <th>36.96</th>
        <th>27.05</th>
    </tr>
    <tr>
        <th>Yolov5s Pruned+Quant</th>
        <th>114.55</th>
        <th>8.72</th>
        <th>NA</th>
        <th>NA</th>
    </tr>
    <tr>
        <th>Yolov5n</th>
        <th>--</th>
        <th>--</th>
        <th>--</th>
        <th>--</th>
    </tr>
    <tr>
        <th>Yolov5n Pruned+Quant</th>
        <th>55.62</th>
        <th>17.97</th>
        <th>NA</th>
        <th>NA</th>
    </tr>
</table>

## Table Resume Metrics

<table border>
    <tr>
        <th>mAP_0.5_0.95</th>
        <th>recall</th>
    </tr>
    <tr>
        <th>{{< figure src="/posts/assets/mAP_0.5_0.95.png" width="400" height="400">}}</th>
        <th>{{< figure src="/posts/assets/recall.png" width="400" height="400">}}</th>
    </tr>
    <tr>
        <th>mAP_0.5</th>
        <th>precision</th>
    </tr>
    <tr>
        <th>{{< figure src="/posts/assets/mAP_0.5.png" width="400" height="400">}}</th>
        <th>{{< figure src="/posts/assets/precision.png" width="400" height="400">}}</th>
    </tr>
</table>

## 🙏 Comments & Feedback
I hope you’ve learned a thing or two from this blog post. If you have any questions, comments, or feedback, please leave them on the following  [LinkedIn](https://www.linkedin.com/in/fabiogeraci/) or [Twitter](https://twitter.com/FGeraci73/).

## Follow me 👇

{{< button href="https://www.linkedin.com/in/fabiogeraci/" >}}LinkedIn{{< /button >}}
{{< button href="https://twitter.com/FGeraci73/" >}}Twitter{{< /button >}}








