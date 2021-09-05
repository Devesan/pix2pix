# pix2pix
## Image2Image translation with cGAN, Pix2Pix…

The architecture is as follows:
1) A U-net generator
2) A convolutional PatchGAN Discriminator

The Pix2Pix GAN has been demonstrated on a range of image-to-image translation tasks such as converting maps to satellite photographs, black and white photographs to color, and sketches of products to product photographs.

## About the Dataset:
A dataset of over 3500 images of both hand-drawn and its corresponding CAD image has been created. These images were constructed from the SketchGraphs dataset which can be found here, SketchGraphs: A Large-Scale Dataset for Modeling Relational Geometry in Computer-Aided Design
Each image in the Train dataset contains two images concatenated together with shape (288, 864, 3). In the data input pipeline, this image is resized to (256, 512, 3).


![alt text](https://miro.medium.com/max/700/1*NkwLb7g7ws-o4MyTZlMWsw.png)


### Building a U-net Generator:
The generator of the pix2pix cGAN is a modified U-Net. A U-Net consists of an encoder (downsampler) and decoder (upsampler).
Each block in the encoder is: Convolution -> Batch normalization -> Leaky ReLU
Each block in the decoder is: Transposed convolution -> Batch normalization -> Dropout (applied to the first 3 blocks) -> ReLU
There are skip connections between the encoder and decoder (as in the U-Net).
![alt text](https://miro.medium.com/max/626/1*qckzBmbO9vW__8JF0os_Rw.png)
![alt text](https://miro.medium.com/max/564/1*hMh9TL1lRsBlXDL9FTsdFw.png)

### Building the PatchGAN discriminator:

Each block in the discriminator is: 
Convolution -> Batch normalization -> Leaky ReLU.
The shape of the output after the last layer is (batch_size, 30, 30, 1).
Each 30 x 30 image patch of the output classifies a 70 x 70 portion of the input image.
The discriminator receives 2 inputs:
The input image and the target image, which it should classify as real.
The input image and the generated image (the output of the generator), which it should classify as fake.
Use tf.concat([inp, tar], axis=-1) to concatenate these 2 inputs together.

![alt text](https://miro.medium.com/max/564/1*XjPW3JhJComj_T9zr1W7ZA.png)
Some Final Results:
![alt text](https://miro.medium.com/max/700/1*qMiJMqyYK3GuzvckILPlSA.png)
![alt text](https://miro.medium.com/max/700/1*UispT1moM9zR76Wp3JHIZw.png)

Future Works:

Due to lack of resources the number of datapoints used for training is less but this model can be used for a much bigger dataset.
