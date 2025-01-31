# Image Restoration

Experimentations on image restoration using AutoEncoders, Conv-nets and GANs, implementations in TensorFlow 2.0.

Most of those models are trained in the objective to perform three image restoration tasks: denoising, upscaling and JPEG deblocking.

The notebooks are used to test the models on the test sets and to visualize the results.
The train.py script is used to train the available models.

>python train.py -h

Implemented models:
* AutoEncoder (Standard implementation)
* U-Net [Ronneberger 2015](https://arxiv.org/pdf/1505.04597.pdf)
* SRCNN [Dong 2015](https://arxiv.org/pdf/1501.00092.pdf)
* (In progress) TNRD [Chen 2016](https://arxiv.org/pdf/1508.02848.pdf)
* (In progress) DnCNN [Zhang 2016](https://arxiv.org/pdf/1608.03981.pdf)
* (In progress) DCGAN [Radford 2016](https://arxiv.org/pdf/1511.06434.pdf)
* (In progress) SRGAN [Ledig 2017](https://arxiv.org/pdf/1609.04802.pdf)
* (In progress) DPIR [Zhang 2020](https://arxiv.org/pdf/2008.13751.pdf)
* (In progress) Deep Image Prior [Ulyanov 2020](https://arxiv.org/pdf/1711.10925v4.pdf)

Used datasets:
* MNIST
* Fashion MNIST
* CIFAR
* Labeled Faces in the Wild (LFW)
* BSDS500

## Models description

### AutoEncoder

This model is just a standard encoder decoder.

### U-Net

U-Net is an example-based method, using convolutional layers and skip connections. 
My implementation of the U-Net is a lighter version of the one from the paper (a total of 11 convolutional layers against 23 originaly).
The paper implementation had a segmentation goal, whereas mine has an image quality improving goal.
The architecture of the U-Net makes use of skip connection that transfers low-level information to high-level layers, in the objective to improve the quality of output images.

### SRCNN

SRCNN is a convolutional neural network that directly learns an end-to-end mapping between low- and high-resolution images.
Its architecture is simple for a small amount of parameters, it is composed of only 3 convolutional layers.
The goal of this model is to upscale an image while augmenting its resolution.

## Datasets description

### MNIST, Fachion MNIST, CIFAR

Those datasets are download using tensorflow_datasets.

### LFW

Images dimension: 128x128x3

Labeled Faces in the Wild is a dataset containing visages, my implementation is based on the full tar archive accessible for download [here](http://vis-www.cs.umass.edu/lfw/#download).


### BSDS500

[Download](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html).

Images dimension: 480x320x3

Total ground truth training images: 200.
Each image is split in 6 sub-images of size 160x160 and randomly rotated by 0, 90, 180, or 270 degrees for a total of 200x6=1200 images, a pre-processing step is then applied to the training set.
During preprocessing, for each training image X we:
* Generate a noisy version of X with a noise level randomly chosen from 15, 25 or 50.
* Generate a low resolution version of X by applying a bicubic downsampling followed by a bicubic upsampling with a downscaling factor randomly chosen from 2, 3 or 4.
* Generate a JPEG compressed version of X by saving it with a compression level randomly chosen from 10, 20, 30 or 40.
Total training images after preprocessing: 1200x3=3600 images.

The same pre-processing as above is applied on the validation set: 100x3=300 (except for the rotation).

Total ground truth test images: 200.
Test sets:
* noise\_15: Noise level of 15 applied to the test set.
* noise\_25: Noise level of 25 applied to the test set.
* noise\_50: Noise level of 50 applied to the test set.
* upscale\_2: Bicubic downsampling followed by bicubic upsampling with downscaling factor of 2 applied to the test set.
* upscale\_3: Bicubic downsampling followed by bicubic upsampling with downscaling factor of 3 applied to the test set.
* upscale\_4: Bicubic downsampling followed by bicubic upsampling with downscaling factor of 4 applied to the test set.
* compress\_10: Quality level of 10 applied to the test set.
* compress\_20: Quality level of 20 applied to the test set.
* compress\_30: Quality level of 30 applied to the test set.
* compress\_40: Quality level of 40 applied to the test set.
