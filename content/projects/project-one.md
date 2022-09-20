---
title: "The Garden Flower Challenge"
date: 2022-08-28
---
{{< toc >}}

## 🔥 Motivation

This is a quite common "take home" challenge. The task is to train a quick proof of concept model verifying the feasibility of detecting flower species in images coming from this [dataset](https://www.robots.ox.ac.uk/~vgg/data/flowers/17/index.html).

The dataset contains 17 category flower dataset with 80 images for each class, which represent some of the most common garden flowers in UK.

I have taken the liberty to code my on solution using [FastAI](https://www.fast.ai/), as main framework, implementing one of the most advanced and novel architecture proposed by [Jeremy Howard](https://twitter.com/jeremyphoward), on this [Kaggle post](https://www.kaggle.com/code/jhoward/small-models-road-to-the-top-part-2/).

## 🔩 Set-Up

### Install libraries

{{< details >}}
{{< highlight bash >}}
!pip install timm
!pip install wandb
!pip install plotly
!pip install --upgrade fastai
{{< / highlight >}}
{{< /details >}}

### Import Libraries
{{< details >}}
{{< highlight bash >}}
import wandb
import os
import sys
import pandas as pd
import torch
from fastai.vision.all import *
from fastai.callback.wandb import *
from fastai.callback.schedule import fit_one_cycle, lr_find
import timm
import plotly.express as px
from fastai.vision.learner import _update_first_layer
from fastai.basics import *
import warnings
warnings.filterwarnings('ignore')
{{< / highlight >}}
{{< /details >}}

### Utilities Functions
{{< details >}}
{{< highlight bash >}}
def plot_class_balance(a_dataset):
    """
    Plot class distribution to explore class imbalance
    :param a_dataset:
    """
    target_series = pd.DataFrame(a_dataset.label.value_counts())
    target_series.reset_index(inplace=True)
    target_series = target_series.rename(columns={'label': 'count'})
    target_series = target_series.rename(columns={'index': 'label'})

    fig = px.bar(target_series, x='label', y='count', color="label", text='count', title='Class Balance', width=800, height=400)
    fig.update_layout(showlegend=False)
    fig.show()
{{< / highlight >}}
{{< /details >}}

{{< details >}}
{{< highlight bash >}}
def split_dataframe(a_dataframe: pd.DataFrame, target: str, split_percentage: float):
    """
    Method to balance each split by target
    :param a_dataframe:
    :param target: target
    :param split_percentage: split percentage
    :return: two dataframes
    """
            groups = a_dataframe.groupby(target)
            row_nums = groups.cumcount()
            sizes = groups[target].transform('size')
        
            temp = a_dataframe[row_nums <= sizes * split_percentage]
            test = a_dataframe[row_nums > sizes * split_percentage]
    return temp, test
{{< / highlight >}}
{{< /details >}}

### Split Dataset into class Balanced sub-sets

{{< details >}}
{{< highlight bash >}}
test_df, temp = split_dataframe(image_dataset, 'label', 0.2)
valid, train = split_dataframe(temp, 'label', 0.2)

train_copy = train.copy()
train_copy.loc[:, 'is_valid'] = False

valid_copy = valid.copy()
valid_copy.loc[:, 'is_valid'] = True

train_df = pd.concat([train, valid], axis=0)

df = train_df.copy()
df_train = df.reset_index(drop=True) #Necessary to for ImageDataLoaders.from_df
{{< / highlight >}}
{{< /details >}}

The picture below show the Class Balance in the train set.

{{< figure src="/projects/project-one_images/class_balanced.png">}}

### Selecting the Run-Device
{{< details >}}
{{< highlight bash >}}
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
{{< / highlight >}}
{{< /details >}}

### Adding Augmentation
{{< details >}}
{{< highlight bash >}}
img_size = 128
augmentations = [
    Rotate(10, p=0.4, mode='bilinear'), 
    Brightness(max_lighting=0.3,p=0.5),
    Contrast(max_lighting=0.4, p=0.5),
    # RandomErasing(p=0.3, sl=0.0, sh=0.2, min_aspect=0.3, max_count=1),
    Flip(p=0.5),
    Zoom(max_zoom=1.5,p=0.5),
    RandomResizedCrop(img_size),
    *aug_transforms(size=224, max_warp=0),
    Normalize.from_stats(*imagenet_stats)
    ]
{{< / highlight >}}
{{< /details >}}

### Creating the Pytorch DataLoader
{{< details >}}
{{< highlight bash >}}
dls = ImageDataLoaders.from_df(df=train_df, valid_col='is_valid', seed=42, 
                               path='/content/drive', bs = 64, device=device, 
                               batch_tfms=augmentations, item_tfms=Resize(img_size))
{{< / highlight >}}
{{< /details >}}

### Creating the Learner and Adding CutMix
{{< details >}}
{{< highlight bash >}}
model_dir = '/flower_class/models'
cbs = [CutMix(1.)]
learn = vision_learner(dls, 'convnext_small_in22k', model_dir=model_dir, 
                metrics=[accuracy, error_rate], loss_func=CrossEntropyLossFlat(), 
                cbs=cbs).to_fp16()
{{< / highlight >}}
{{< /details >}}

### Running the Learner
{{< details >}}
{{< highlight bash >}}
learn.fine_tune(20, 0.001)
{{< / highlight >}}
{{< /details >}}

{{< figure src="/projects/project-one_images/training-trace.png">}}

Check out the [WandB](https://wandb.ai/fabiogeraci/flower_app/reports/Weave-Prediction_Samples-22-07-28-11-07-45---VmlldzoyMzg4NzIx?accessToken=cruzovzndudeaisy3wh2hfnmd6pazbpnrdu80sow1hngiob30cul02e63y77spud), for more in site

I would say pretty impressive

