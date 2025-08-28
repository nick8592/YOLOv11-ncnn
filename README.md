# YOLOv11-ncnn

YOLOv11-ncnn is a project for running YOLOv11 models with ncnn.

## Quick Start (Docker)

```bash
docker run -it --gpus all --shm-size=16g -v /home:/home <image_id>
```

## Environment Setup

```bash
apt update && apt upgrade -y
apt install python3 python3-pip -y
apt install build-essential git cmake wget libprotobuf-dev protobuf-compiler libomp-dev libopencv-dev -y
```

## Python & PyTorch Installation

```bash
pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117
```

## Project Setup

```bash
cd /YOLOv11-ncnn
pip install -r requirements.txt
```

## Export Models

```bash
mkdir weights && cd weights
yolo export model=yolo11n.pt format=torchscript
yolo export model=yolo11n-seg.pt format=torchscript
```

## Convert Models with pnnx

```bash
pnnx yolo11n.torchscript
pnnx yolo11n-seg.torchscript
```

## Download Example Code

```bash
cd /YOLOv11-ncnn
wget https://github.com/Tencent/ncnn/raw/master/examples/yolo11.cpp
wget https://github.com/Tencent/ncnn/raw/master/examples/yolo11_seg.cpp
```

## Model Summary

- **YOLO11n** (fused): 100 layers, 2,616,248 parameters, 0 gradients, 6.5 GFLOPs
- **YOLO11n-seg** (fused): 113 layers, 2,868,664 parameters, 0 gradients, 9.7 GFLOPs

## Reference

- [YOLOv11-ncnn Zhihu Article](https://zhuanlan.zhihu.com/p/1903414797195781701)