# YOLOv11-ncnn

YOLOv11-ncnn is a project for running YOLOv11 models with ncnn.

---

## Setup Instructions

### 1. Quick Start (Docker)
```bash
docker run -it --gpus all --shm-size=16g -v /home:/home <image_id>
```

### 2. Environment Setup
```bash
apt update && apt upgrade -y
apt install python3 python3-pip -y
apt install build-essential git cmake git libopencv-dev
```

### 3. Python & PyTorch Installation
```bash
pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117
```

### 4. Project Setup
```bash
cd /YOLOv11-ncnn
pip install -r requirements.txt
```

---

## Model Export & Conversion (Optional)
If you want to convert your YOLOv11 models on your own, follow these steps:

### 1. Export Models
```bash
mkdir weights && cd weights
yolo export model=yolo11n.pt format=torchscript
yolo export model=yolo11n-seg.pt format=torchscript
```

### 2. Convert Models with pnnx
```bash
cd weights
pnnx yolo11n.torchscript
pnnx yolo11n-seg.torchscript
```
See [Guidelines for Converting YOLOv11 Models](https://github.com/nihui/ncnn-android-yolo11#guidelines-for-converting-yolo11-models) for details.

#### Dynamic Shape Support
```bash
cd weights
python3 -c 'import yolo11n_pnnx; yolo11n_pnnx.export_torchscript()'
python3 -c 'import yolo11n_seg_pnnx; yolo11n_seg_pnnx.export_torchscript()'
pnnx yolo11n_pnnx.py.pt inputshape=[1,3,640,640] inputshape2=[1,3,320,320]
pnnx yolo11n_seg_pnnx.py.pt inputshape=[1,3,640,640] inputshape2=[1,3,320,320]
mv yolo11n_pnnx.py.ncnn.param yolo11n.ncnn.param
mv yolo11n_pnnx.py.ncnn.bin yolo11n.ncnn.bin
mv yolo11n_seg_pnnx.py.ncnn.param yolo11n_seg.ncnn.param
mv yolo11n_seg_pnnx.py.ncnn.bin yolo11n_seg.ncnn.bin
```

---

## Pretrained Converted Models
Pretrained weights for YOLOv11n (converted for ncnn) are available on Hugging Face:
- [nickpai/yolo-ncnn/yolo11n](https://huggingface.co/nickpai/yolo-ncnn/tree/main/yolo11n)

Download the `.ncnn.param` and `.ncnn.bin` files from the above link and place them in the `weights` folder.

---

## Example Code & Test Image

### 1. Download Example Code
```bash
cd /YOLOv11-ncnn
wget https://github.com/Tencent/ncnn/raw/master/examples/yolo11.cpp
wget https://github.com/Tencent/ncnn/raw/master/examples/yolo11_seg.cpp
```

### 2. Prepare Test Image
Download a sample COCO image into the `images` folder:
```bash
wget -O images/sample.jpg http://images.cocodataset.org/val2017/000000039769.jpg
```

---

## Build Instructions

### 1. Build ncnn
```bash
git clone https://github.com/Tencent/ncnn.git
cd ncnn
mkdir build && cd build
cmake ..
make -j$(nproc)
make install
```

### 2. Build YOLOv11-ncnn Project
```bash
cd /YOLOv11-ncnn
mkdir build && cd build
cmake ..
make -j$(nproc)
```

---

## Run Inference

Assuming your executables are in the `bin` folder and your sample image is `images/sample.jpg`:
```bash
./bin/yolo11 ./images/sample.jpg
./bin/yolo11_seg ./images/sample.jpg
```
Replace `sample.jpg` with your own image filename if needed.

---

## Directory Tree
```
YOLOv11-ncnn
|-- CMakeLists.txt
|-- LICENSE
|-- README.md
|-- bin
|   |-- yolo11
|   `-- yolo11_seg
|-- build
|   |-- CMakeFiles
|   |-- Makefile
|   `-- ..
|-- images
|   |-- image_det_output.jpg
|   |-- image_seg_output.jpg
|   `-- sample.jpg
|-- ncnn
|   |-- CITATION.cff
|   |-- CMakeLists.txt
|   `-- ..
|-- requirements.txt
|-- src
|   |-- yolo11.cpp
|   `-- yolo11_seg.cpp
`-- weights
    |-- yolo11n.ncnn.bin
    |-- yolo11n.ncnn.param
    |-- yolo11n_pnnx.py
    |-- yolo11n_seg.ncnn.bin
    |-- yolo11n_seg.ncnn.param
    |-- yolo11n_seg_pnnx.py
    `-- ..
```

---

## Troubleshooting & Tips
- If you get `cv::imread ... failed`, check your image path and permissions, and use an absolute path if needed.
- If you see `malformed literal token` or `Segmentation fault`, ensure your model files match the code (detection vs segmentation) and are properly exported and converted.
- For dynamic shape, follow the conversion steps above and use the correct Python export script.
- Always verify your weights and model files are not corrupted.

---

## Model Summary
- **YOLO11n** (fused): 100 layers, 2,616,248 parameters, 0 gradients, 6.5 GFLOPs
- **YOLO11n-seg** (fused): 113 layers, 2,868,664 parameters, 0 gradients, 9.7 GFLOPs

---

## Reference
- [YOLOv11-ncnn Zhihu Article](https://zhuanlan.zhihu.com/p/1903414797195781701)