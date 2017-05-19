# Caffe-model
Python script to generate prototxt on Caffe, specially the inception_v3\inception_v4\inception_resnet\fractalnet

# Scripts

The prototxts can be visualized by [ethereon](http://ethereon.github.io/netscope/quickstart.html).

All the generator scripts have moved to [scripts folder](https://github.com/soeaver/caffe-model/tree/master/scripts)


# CLS (imagenet)

### Introduction
This folder contains the deploy files(include generator scripts) and pre-train models of resnet-v1, resnet-v2, inception-v3, inception-resnet-v2 and densenet(coming soon).

We didn't train any model from scratch, some of them are converted from other deep learning framworks (inception-v3 from [mxnet](https://github.com/dmlc/mxnet-model-gallery/blob/master/imagenet-1k-inception-v3.md), inception-resnet-v2 from [tensorflow](https://github.com/tensorflow/models/blob/master/slim/nets/inception_resnet_v2.py)), some of them are converted from other modified caffe ([resnet-v2](https://github.com/yjxiong/caffe/tree/mem)). But to achieve the original performance, finetuning is performed on imagenet for several epochs. 

The main contribution belongs to the authors and model trainers.

### Performance on imagenet
 
**1. Top-1/5 accuracy of pre-train models in this repository.**

 Network|224/299(single-crop)|224/299(12-crop)|320/395(single-crop)|320/395(12-crop)
 :---:|:---:|:---:|:---:|:---:
 resnet101-v2| 78.05/93.88 | 80.01/94.96 | 79.63/94.84 | 80.71/95.43
 resnet152-v2| 79.15/94.58 | 80.76/95.32 | 80.34/95.26 | 81.16/95.68 
 resnet269-v2| **80.29**/95.00 | **81.75**/95.80 | 81.30/95.67 | **82.13**/96.15
 inception-v3| 78.33/94.25 | 80.40/95.27 | 79.90/95.18 | 80.75/95.76 
 xception| 79.10/94.51 | ../.. | 80.42/95.23 | ../.. 
 inception-v4| 79.97/94.91 | 81.40/95.70 | **81.32**/95.68 | 81.88/96.08 
 inception-resnet-v2| 80.14/**95.17** | 81.54/**95.92** | 81.25/**95.98** | 81.85/**96.29**
 resnext50_32x4d| 77.63/93.69 | 79.47/94.65 | 78.90/94.47 | 79.63/94.97 
 resnext101_32x4d| 78.70/94.21 | 80.53/95.11 | 80.09/95.03 | 80.81/95.41
 resnext101_64x4d| 79.40/94.59 | 81.12/95.41 | 80.74/95.37 | 81.52/95.69
 wrn50_2(resnet50_1x128d)| 77.87/93.87 | 79.91/94.94 | 79.32/94.72 | 80.17/95.13

 - The pre-train models are tested on original [caffe](https://github.com/BVLC/caffe) by [evaluation_cls.py](https://github.com/soeaver/caffe-model/blob/master/cls/evaluation_cls.py), **but ceil_mode:false（pooling_layer） is used for the models converted from torch, the detail in https://github.com/BVLC/caffe/pull/3057/files**. If you remove ceil_mode:false, the performance will decline about 1% top1.
 - 224x224(base_size=256) and 320x320(base_size=320) crop size for resnet-v2/resnext/wrn, 299x299(base_size=320) and 395x395(base_size=395) crop size for inception.

**2. Top-1/5 accuracy with different crop sizes.**
![teaser](https://github.com/soeaver/caffe-model/blob/master/cls/accuracy.png)
 - Figure: Accuracy curves of inception_v3(left) and resnet101_v2(right) with different crop sizes.

**3. Download url and forward/backward time cost for each model.**

 Forward/Backward time cost is evaluated with one image/mini-batch using cuDNN 5.1 on a Pascal Titan X GPU.
 
 We use
  ```
    ~/caffe/build/tools/caffe -model deploy.prototxt time -gpu -iterations 1000
  ```
 to test the forward/backward time cost, the result is really different with time cost of [evaluation_cls.py](https://github.com/soeaver/caffe-model/blob/master/cls/evaluation_cls.py)

 Network|F/B(224/299)|F/B(320/395)|Download|Source
 :---:|:---:|:---:|:---:|:---:
 resnet101-v2| 22.31/22.75ms | 26.02/29.50ms | [170.3MB](https://pan.baidu.com/s/1kVQDHFx)|[craftGBD](https://github.com/craftGBD/craftGBD)
 resnet152-v2| 32.11/32.54ms | 37.46/41.84ms | [230.2MB](https://pan.baidu.com/s/1dFIc4vB)|[craftGBD](https://github.com/craftGBD/craftGBD)
 resnet269-v2| 58.20/59.15ms | 69.43/77.26ms | [390.4MB](https://pan.baidu.com/s/1qYbICs0)|[craftGBD](https://github.com/craftGBD/craftGBD)
 inception-v3| 21.79/19.82ms | 22.14/24.88ms | [91.1MB](https://pan.baidu.com/s/1boC0HEf)|[mxnet](https://github.com/dmlc/mxnet-model-gallery/blob/master/imagenet-1k-inception-v3.md)
 inception-v4| 32.96/32.19ms | 36.04/41.91ms | [163.1MB](https://pan.baidu.com/s/1c6D150)|[tensorflow_slim](https://github.com/tensorflow/models/tree/master/slim)
 inception-resnet-v2| 49.06/54.83ms | 54.06/66.38ms | [213.4MB](https://pan.baidu.com/s/1jHPJCX4)|[tensorflow_slim](https://github.com/tensorflow/models/tree/master/slim)
 resnext50_32x4d| 17.29/20.08ms | 19.02/23.81ms | [95.8MB](https://pan.baidu.com/s/1kVqgfJL)|[facebookresearch](https://github.com/facebookresearch/ResNeXt)
 resnext101_32x4d| 30.73/35.75ms | 34.33/41.02ms | [169.1MB](https://pan.baidu.com/s/1hswrNUG)|[facebookresearch](https://github.com/facebookresearch/ResNeXt)
 resnext101_64x4d| 42.07/64.58ms | 51.99/77.71ms | [319.2MB](https://pan.baidu.com/s/1pLhk0Zp)|[facebookresearch](https://github.com/facebookresearch/ResNeXt)
 wrn50_2(resnet50_1x128d)| 16.48/25.28ms | 20.99/35.04ms | [263.1MB](https://pan.baidu.com/s/1nvhoCsh)|[szagoruyko](https://github.com/szagoruyko/wide-residual-networks)


### Check the performance
**1. Download the ILSVRC 2012 classification val set [6.3GB](http://www.image-net.org/challenges/LSVRC/2012/nnoupb/ILSVRC2012_img_val.tar), and put the extracted images into the directory:**

      ~/Database/ILSVRC2012

**2. Modify the parameter settings**

 Network|val_file|mean_value|std
 :---:|:---:|:---:|:---:
 resnet_v2(101/152/269)| ILSVRC2012_val | [102.98, 115.947, 122.772] | [1.0, 1.0, 1.0]
 resnet_v2(38a/38a1) | ILSVRC2012_val | [103.52, 116.28, 123.675] | [57.375, 57.12, 58.395]
 resnext(50/101), wrn50_2 | ILSVRC2012_val | [103.52, 116.28, 123.675] | [57.375, 57.12, 58.395]
 inception-v3| **ILSVRC2015_val** | [128.0, 128.0, 128.0] | [128.0, 128.0, 128.0] 
 inception-v1(v2/xception) | ILSVRC2012_val | [128.0, 128.0, 128.0] | [128.0, 128.0, 128.0] 
 inception-v4(inception-resnet-v2) | ILSVRC2012_val | [128.0, 128.0, 128.0] | [128.0, 128.0, 128.0] 
 resnet36, resnet50(77)_1x32d, vgg16_dsd | ILSVRC2012_val | [104.0, 117.0, 123.0] | [1.0, 1.0, 1.0]
 vgg16_tf | ILSVRC2012_val | [103.94, 116.78, 123.68] | [1.0, 1.0, 1.0]


**3. then run evaluation_cls.py**

    python evaluation_cls.py

**4. Top-1/5 accuracy of some lightweight or low-accuracy networks from the open source.**

 Network|224(single-crop)|F/B(224)|Download|Source
 :---:|:---:|:---:|:---:|:---:
 resnet10| 63.37/84.78 | ../.. | ../.. | ../..
 resnet36| 71.30/89.70 | ../.. | ../.. | ../..
 resnet50_1x32d| 67.61/88.01 | ../.. | ../.. | ../..
 resnet77_1x32d| 70.34/89.63 | ../.. | ../.. | ../..
 mobilenet| 70.02/89.48 | ../.. | ../.. | ../..
 inception-v1(tf)| 68.64/88.90 | ../.. | ../.. | ../..
 inception-v1(bvlc)| ../.. | ../.. | ../.. | ../..
 inception-v1(dsd)| 68.61/88.90 | ../.. | ../.. | ../..
 inception-v2| 71.57/90.29 |  ../.. | ../.. | ../..
 vgg16_tf| 70.97/89.88 | ../.. | ../.. | ../..
 vgg16_dsd| 71.91/90.68 | ../.. | ../.. | ../..

 - crop_size=231 and base_size=256 for inception_v2.
 - crop_size=224 and base_size=224 for inception_v1(dsd).



# Acknowlegement

I greatly thank [Yangqing Jia](https://github.com/Yangqing) and [BVLC group](https://www.github.com/BVLC/caffe) for developing Caffe

And I would like to thank all the authors of every cnn model
