# Semantic Segmentation of Roads in Satellite imagery 

### Semantic segmentation is the process of classifying each pixel of an image into distinct classes using deep learning. This aids in identifying regions in an image where certain objects reside. 

This aim of this project is to identify and segment roads in aerial imagery. Detecting roads can be an important factor in predicting further development of cities, and this concept plays a major role in GeoArchitect (A project which I started). Segmentation of roads is important to map-based applications and is used for finding distances or shortest routes between two places.

Read more about this project here.

## Contents:
1. Dataset.
2. Manipulating the data.
3. About the model.


## 1. Dataset

For this challenge, I used the Indian Roads Dataset. This dataset contains aerial images, along with the target masks. You can use download_images.py to download all the images mentioned in this site. For this project, I used .jpg sketches fetched from [Openstreetmap](https://www.openstreetmap.org) with a random lat/lon of (22.85509/88.38842).

The dataset contains 1171 images and respectiv masks. Both the masks and the images are 1500x1500 in the resolution are present in the .tiff format. Have a look at the following sample.

![Samples](https://github.com/Neilblaze/Map-Path-Segmentation/blob/master/Sample%20images/Sample.jpg)

## 2. Manipulating the data

The pre-processing steps involved: 
```
1. Removed images where more than 25% of the map was missing.
2. Cropped 256x256 images out of the images. Hence, increasing the total number of images to more than 22,000.
3. Binarized the mask so that the pixel value is always between 0 and 1.
```
## 3. About the model

To solve this problem, I used an Unet, it is a fully convolutional network, with 3 cross-connections. Adam optimiser with a learning rate of 0.00001 was used, along with dice loss (because of the unbalanced nature of the dataset.) 
The model trained for 61 epochs before earlstopper kicked in and killed the training process. A validation dice loss of 0.7548 was achieved.

The model can be found in Models/road_segmentation.h5.



### Scopes of improvement:

#### There were certain maps in which the roads weren’t completely visible. Even though no model can churn out 100% accurate results, and there is a room for improvement always. We can improve the performance of our model by taking certain measures, and they are as follows:

1. Image Data Augmentation: It is the method off slightly distorting the images by applying various operations like colour shift, rotation etc. to generate more data.

2. Use loss multipliers to deal with class imbalance: As mentioned earlier, we had a class imbalance problem, and to deal with it, we used Soft Dice Loss. We want to maximise our dice coefficient, but when compared to Binary Cross-entropy, the later has better gradients and therefore will be a good proxy for our custom loss function and can be easily maximised. The only problem is that Binary Cross-entropy, unlike Soft Dice loss, is not built to deal with the class imbalance and this results in jet black segmentation maps. However, if we apply class multipliers, so that, the model is incentivized to ignore frequently occurring classes, then we can use Binary Cross-entropy instead of Dice Loss. This will result in a smooth training experience.

3. Using Pretrained models: pre-trained models can be fine-tuned for this problem, and they will act as the best feature extractors. Using transfer learning results in faster training times, and often yields superior segmentation maps.





