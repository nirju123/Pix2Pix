# Image Colorization with Pix2Pix GANs
---
## Problem:
Colorizing old Black-and-White images using Deep learning.

---
## Solution:
I utilized Pix2Pix U-Net Generative Adversarial Networks (GANs), which was proposed as a general-purpose solution to image-to-image translation problems.Researchers had demonstrated that this approach is effective at tasks like
- synthesizing photos from label maps,
- reconstructing objects from edge maps,
- colorizing images ,e.t.c

## Dataset:
Downloaded sports image dataset from kaggle, 8227 images for training and 2056 for testing.

## Process & Results:
WE are going to convert RGB image to LAB image format, where L channel represents lightness or luminance, where A channel represents the color on the green-red axis and B channel represents the color on the blue-yellow axis.
###### Why LAB image format instead of RGB?
By using LAB, the task simplifies to predicting just the A and B channels (color), while using the L channel (grayscale) as input. This reduces the dimensionality of the problem compared to predicting all three channels (R, G, B) in the RGB space.

###### Step by Step breakdown:
- Importing all the necessary libraries.
- Preprocess the images:
  - resize each image to same size i.e 256 x 256
  - few images are RGBA , converting them to RGB
  - applying some data augmentaion using flip_left_right function , to improve model generalization
  - image to numpy array
  - convert the image to LAB format
  - split into L and AB for input and output respectively
  - normalize each to range of -1 to 1
  - convert them to float32 from float64
  - return (L,AB)
 - As it is not possible to load the whole dataset in the memory at the same time, hence we created batches of 12 images each.
 - Defined a  Discriminator model which will have input  of size (None, 2 x 256 x 256) and output of size (None, 1 x 32 x 32) ,None stands for batch size.
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/PXL_20240917_115603812_2.jpg)
 - Defined a Generator model  which will have input of size (None, 1 x 256 x 256) and output of (None, 2 x 256 x 256).
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/PXL_20240917_120207845_2.jpg)
 - Details of each Down Convolution block denoted by red vertical line in generator diagram (used for downsampling the image):
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/PXL_20240917_115552023_2.jpg)
 - Details of each Up Convolution block denoted by blue vertical line in generator diagram (used for upsampling the image):
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/PXL_20240917_115555841_2.jpg)

 - define optimizers for gen and disc in our case it is Adam with learning rate 2e-4, beta_1=0.5 and beta_2=0.999.
 - define loss functuions (BinaryCrossentropy and MeanAbsoluteError)
 - define training function
 - define training loop and start training.

## Results:
It took 14+ hours of NVIDIA GPU of my laptop to train over these 8227 training images and produce following outputs on test results:

![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224005.png)
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224156.png)
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224542.png)
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224756.png)
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224923.png)
![image](https://github.com/nirju123/Pix2Pix/blob/main/images/Screenshot%202024-08-27%20224942.png)









