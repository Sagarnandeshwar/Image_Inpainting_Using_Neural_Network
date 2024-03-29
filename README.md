# Comp 558: Final Project
In this project, we have studied and implemented different Inpainting methods to recover artistic paintings damaged by irregular patches back to their original form. We have analyzed two procedural methods - Telea Algorithm and Naive Strokes method - and one data-driven method - Partial Convolution. We have implemented Telea Algorithm and Partial Convolution.  
 
[I worked on Naive Stroke Method and building the Partial Convolution Network] 
 
## Paper 
“Image Inpainting for Irregular Holes Using Partial Convolutions” Guilin Liu, Fitsum A. Reda, Kevin J. Shih, Ting-Chun Wang, Andrew Tao, Bryan Catanzaro. Published at ECCV 2018  Site: https://arxiv.org/abs/1804.07723 
<br>
In the paper, the NVIDIA research team has presented a deep learning technique to in-paint (damaged/marked) images by infilling holes with plausible pixel predictions. The main structure is based on U-Net architecture with partial convolution layers, and the training is performed using six different loss functions. 

## Image Inpainting 
Image inpainting is a technique used in computer vision and image processing to fill in missing or corrupted parts of an image with plausible content. It has several applications in graphics, image restoration, and photography. 
 
## Partial Convolution Network 
Partial Convolution is one of the most modern Machine Learning (or Data Driven) approaches toward image inpainting problems. Developed by NVIDIA research, it is a deep learning technique to inpaint (damaged/marked) images by infilling holes with plausible pixel predictions. The main structure is based on U-Net architecture with partial convolution layers, and the training is performed using six different loss functions. 
 
In the context of image in-painting, the standard convolution layer has only one input (image), whereas, in partial convolution, there are two inputs, a mask and an image. The image layers are generated by first calculating the element-wise product of the previous mask and image layer, followed by a convolution. However, the mask layers are generated in a similar way as a standard convolution. 
 
## U-Net architecture  
U-Net structure is a method where the image dimensions undergo not only progressive reduction (using encoders layers) but also the U-Net recovers the image back to its original size using progressive expansion (using decoders layers), unlike linear CNN where the dimensions of the layers are progressively reduced.  

To prevent information loss, layers from the encoder section are concatenated with corresponding layers on the decoder section. The output of a U-Net is an image with the same dimension as the input image. 
 
## Datasets: 
The training dataset is taken from Cifar10, which consists of 60000 32x32 colour images (they are categorized in 10 classes, with 6000 images per class; however, we ignore their label for this project). There are 50000 training images and 10000 test images. We use this dataset as it is relatively small. The implemented model has 16 partial convolutions layers, so it wasn’t possible for us to train the model with high-resolution pictures. We have created a function that creates random marks (region to be inpaint) on the subset (batch) of the dataset. As stated above, we used both images and marks to train the model. 
 
## Modification: 
- For loss function, we have taken RELU for the encoder and leaky RELU for the decoder. This is done to make implementation and computation relatively easier. 
- For model training, we have run only 4 epochs with Image dimensions of 32 * 32  

In contrast, the actual model  
- have an extremely complex loss function that covers both per-pixel reconstruction accuracy as well as composition (i.e., how smoothly the predicted hole values transition into their surrounding context) 
- was trained with 50 epochs with multiple batches with several data resources both for images and marks (both in high resolution).  

It is estimated that their team took ten days to train the model with a supercomputer and strong GPU support. 
 
## Resources  
The research team at Nvidia has kept their entire implementation private from the  public. Therefore, we took help from the following GitHub repository to complete the model. I have added references in the codes 
- https://github.com/NVIDIA/partialconv (This is the main code given by the author; it is used for the overall structural review) 
- https://github.com/MathiasGruber/PConv-Keras (for keras implementation of Partial Convolution layers and all cnn_helper methods) 
- https://stanford.edu/~shervine/blog/keras-how-to-generate-data-on-the-fly (for ML model implementation: Keras implementation for data generators) 
- COMP 551: Applied Machine Learning (activation function, RELU instead of L_total for  simplicity, comparison function and Model compile arguments) 
 
## Result 
![Result](https://github.com/Sagarnandeshwar/COMP558_ImageInpainting/blob/main/image/result.png)
The first column represents the marked input image, the second column represents the mark, the third column represents the output of the model, and the last column represents the original image 

