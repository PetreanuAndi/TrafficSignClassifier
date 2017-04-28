#**Traffic Sign Recognition** 

##Writeup Template
---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ../distributionOfTrain.png "Distribution Of Training Set"
[image2]: ./distributionAfterAugmentation.png "Distribution of training Set after augmentation" 

---
###Writeup / README

####1. Writeup / README

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

###Data Set Summary & Exploration

####1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I plotted histograms of classes as found in original y_train.
The original dataset splits seem poorly built/chosen, and the distribution on test is very poor for certain classes.
As an observation, if i identically map the last video for "LeNet for Traffic Signs" then i only get around 0.88 accuracy, far from the falsely advertised 0.96 from the video =>>>> the dataset used in the video must have been split/augmented differently.

####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. This bar chart represents the distribution of classes for the train set.

![alt text][image1]

###Design and Test a Model Architecture

####1. Processing & Augmentation


I build a processing pipeline and an augmentation pipeline.
Processing (in this order): 
	1) grayscale : the provided paper implies that the color features are not learned properly (no association for color). I have implemented a the grayscale transform method in several ways (because sometimes it did not keep the aditional channel, or actually kept 3 channels even if the imshow() call visualised a grayscale image. Strange)
	2) histogramEqualisation for better overall contrast and more obvious edges and shapes
	3) per image normalisation : as was advised, i implemented the (pixel - 128)/ 128 normalisation per image.

Augmentation : After spending a lot of time augmenting with flips (and getting very bad accuracy) i realised that not all images can be flipped (because their class would change after flipping). I was inspired by the paper and other implementations found online to augment with rotation. I additionally augmented with a random shift method, that performs minor translations on both y and x axis. All augmentation is only performed on the poorly represented classes within the distribution

Here is an image of the distribution after the augmentation : 

![alt text][image2]


####2. Model architecture

My model was the adviced LeNet with the following tweaks : 

1) dropout for both fc1 and fc2. I have experimented with different keep_prob for training, and finally settled on 0.7, seems to work best.
2) i've changed the dimensions of the final fcs to 400->200->100->43 in order to give the network more breathing room in terms of possible features to be learned. 
( my intuition was that it should have more space because traffic signs are more complex than digits)
3) i've used avg_pool instead of max_pool, with no visible accuracy gain. It seems to me like avg_pool makes more sense anyway, and it was advised in the videos.
 

####3. Training the model.

I've trained the model for 15 epochs with 150 bach size. On my final model, it seemed like it did not stop learning after 15 epochs (there was slight gain in accuracy all the way up to the end, with no fluctuation), but i was late with the project and i was ok with the result

I experimented with different learning rates but in the end chose the default 0.001 and Adam optimiser (for adaptive momentum and lr)


####4. Results. Approach

My final model results were:
* Max validation set accuracy of 0.952 after 15 epoch. (i could have continued and probably would have gained a bit more)
* Test set accuracy of 0.933. This result was pleasing but very misleading. 

The chosen arhitecture was the adivised LeNet. I noticed the paper also advertises LeNet as appropiate for such small images (with VGG being too big for such small input)
However, i believe that the poor splits of the dataset make for a misleading set of conclusions. More on that below.

###Test a Model on New Images

####1. Real-life Test

I have downloaded images from both google image "german traffic signs" and also from cropping google street-view from my home town.
I've preprocessed these images in the same processPipeline as the training set.
However, i got a VERY poor accuracy in top1 on these images (less than 20%), leading me to believe the above mentioned poor splits/conclusions.

It correctly identified speed signs but not the correct speed values and it performed extremely poorly on different aspect-ratio photos.
Some of these bad results can be found in the "testImages" folder. 
I resized the images with cv2 default resize method, without specifing any custom options (this might also be an error-source)
Most photos were not centered and the projection was not dead straight => other error sources.



