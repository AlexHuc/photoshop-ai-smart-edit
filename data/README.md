# Dataset Setup

## Overview

This project uses the **MS COCO Dataset (Microsoft Common Objects in Context)** to provide:

- Real-world images  
- Object segmentation masks  
- Rich annotations for object removal and inpainting tasks  

The dataset contains **over 120K images with detailed object segmentation**, making it well suited for:

- Training mask-based editing pipelines  
- Testing object removal workflows  
- Evaluating inpainting quality

---

# Dataset Source

The official dataset is hosted by the **COCO Consortium** and can be downloaded from:

Official site

https://cocodataset.org/#download

---

# Required Files

For this project you only need **images + segmentation annotations**.

| Split | Description | Size |
|------|-------------|------|
| Train 2017 | 118K training images | ~19GB |
| Val 2017 | 5K validation images | ~1GB |
| Train/Val Annotations | Detection + segmentation JSON | ~241MB |
| Panoptic Masks | PNG segmentation masks | ~821MB |

---

# Download Using wget

The following commands download the official dataset directly from the COCO servers.

## Train Images

'''bash
wget http://images.cocodataset.org/zips/train2017.zip
'''

Size: **~19GB**

---

## Validation Images

'''bash
wget http://images.cocodataset.org/zips/val2017.zip
'''

Size: **~1GB**

---

## Segmentation Annotations

'''bash
wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
'''

Size: **~241MB**

This archive contains:

'''text
annotations/
    instances_train2017.json
    instances_val2017.json
    captions_train2017.json
    captions_val2017.json
'''

The **instances JSON files contain segmentation polygons and RLE masks** used to generate object masks.

---

# Optional: Panoptic Segmentation Masks

For **pixel-level mask supervision**, download the panoptic annotations.

Train masks:

'''bash
wget http://images.cocodataset.org/annotations/panoptic_train2017.zip
'''

Validation masks:

'''bash
wget http://images.cocodataset.org/annotations/panoptic_val2017.zip
'''

These archives include **PNG mask images** aligned with the dataset.

---

# Quick Setup Script

Run the following script inside the repository to prepare a minimal dataset for testing.

'''bash
mkdir -p datasets/coco
cd datasets/coco

# Download validation images (small split)
wget http://images.cocodataset.org/zips/val2017.zip
unzip val2017.zip

# Download annotations
wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip
unzip annotations_trainval2017.zip
'''

Directory structure after setup:

'''text
datasets/
└── coco/
    ├── val2017/
    └── annotations/
        ├── instances_val2017.json
        └── instances_train2017.json
'''

---

# Converting COCO Annotations to Masks

The annotations store segmentation as **polygons or run-length encoding (RLE)**.

Use **pycocotools** to convert these annotations into binary masks.

Install:

'''bash
pip install pycocotools
'''

Example conversion:

'''python
from pycocotools import mask as maskUtils

rle = maskUtils.frPyObjects(polygons, height, width)
mask = maskUtils.decode(rle)
'''

These masks can then be used in the **inpainting pipeline**.

---

# Using the Dataset in This Project

The dataset is used for:

- Generating **object removal masks**
- Testing **diffusion-based inpainting**
- Evaluating **prompt-guided editing quality**

Example workflow:

'''text
COCO image
     ↓
Generate mask from segmentation
     ↓
Apply diffusion inpainting
     ↓
Replace or remove object
'''

---

# Alternative Dataset Sources

If downloading the full dataset is inconvenient, several mirrors and programmatic loaders exist.

## Kaggle

https://www.kaggle.com/datasets/awsaf49/coco-2017-dataset

Advantages:

- Smaller subsets
- Easy notebook integration

---

## Hugging Face Datasets

You can load the dataset programmatically using Hugging Face Datasets.

Example:

'''python
from datasets import load_dataset

dataset = load_dataset("mscoco", "2017")
'''

---

## Pre-Masked COCO (Inpainting)

Some research repos provide **pre-generated masks** for object removal tasks.

These versions can accelerate experimentation with **diffusion inpainting models**.

---

# Storage Recommendations

Full dataset size can exceed **25GB**, so consider:

- Using a dedicated `datasets/` volume
- Mounting external storage for Docker containers
- Caching images locally for faster training

Example Docker mount:

'''bash
-v /data/coco:/app/datasets/coco
'''

---

# Summary

Using **MS COCO** enables realistic testing of AI image editing systems by providing:

- Complex scenes with multiple objects  
- Accurate segmentation masks  
- Large-scale image diversity  

This dataset is widely used in **computer vision, generative AI, and diffusion model research**, making it a strong benchmark for **prompt-based image editing systems**.