# Direct Speech-to-Image Translation

The project aims to convert speech to images. However the dataset we are dealing with does not have speech captions to the images. Many languages have no writing form, which calls for the approaches to understand and visualize the speech directly.  Therefore, we are using google text to speech software to convert the text captions to speech captions. 


## Pipeline

![DL Project Pipeline](https://user-images.githubusercontent.com/67867461/168595799-9976ece4-aceb-4d0e-91b6-62f6261d9228.png)




## Text to speech
However the gtts API has a max limit on the total number of requests you can send. Therefore, in order to overcome that, we are delaying the conversion which makes the number of requests sent to the API slower. This works seamlessly so you don't have to keep running the code periodically. However, this now takes too long to run, given that there are more than 10000 files to be converted. 

We have sampled some captions randomly and made a dataset with it so we don't have to run this conversion again and again. Anyway, the number of datapoints is too high because the sampling rate is 22050 per sec. Therefore, even when using 2sec audio files, cuda can't handle more than 500-600 such files. Our dataset contains 500such files which we use for training our classifier. 

## Audio Classifier
The classifier is built using Vanilla MLP. We first tried to convert the audio file into a spectrogram and then run CNN and RNN on the file but it is too computationally expensive and google colab kept crashing. So we had to work with vanilla MLP. The accuracy of the classifier is around 60%. 

## GAN
There are two GANs in the system. Both of them are unconditional GANs trained on around 4000 images each. Cat GAN is trained on cat images and produces other cat images. Dog GAN is trained on dog images and produces other dog images. We trained both the GANs for 1000 epochs and there has been substantial improvement in the results and we think they will get better if we trained the model for more epochs but we stopped ourselves due to computational and time restrictions. Since this training takes a lot of time, we save the models and they can be loaded directly. The generator loaded from the saved state produces new images when it is called. The GANs first transform the images into 64x64 sized RGB images and normalizes the pixels between -1 and 1. The GAN outputs a 64x64 sized RGB image, so the images are not visually very clear on zooming in. However, higher sized images demand high computational requirements and take a lot of time to train. So in the trade off between computational requirements and the quality of output, we tried different sized images and chose 64x64 outputs over other options.

## Dataset
Because of computational limitations, we decreased the size of images to 140x140 rgb. We also removed the images that had both dogs and cats because it will make it difficult to learn the representations.
Since the task that we are doing is different and is on a different dataset, we have different requirements. The changes we made to the pipeline and the reasons are listed below. 

## Contributors:
[Bolisetty Lokesh](https://lokeshbolisetty.github.io/) 2019A7PS0103G 

[Sreekar Bheemshetty](https://www.linkedin.com/in/sreekar-bheemshetty-8010401bb/) 2019A7PS0137G

[Shubham Rajnish Gandhi](https://www.linkedin.com/in/shubham-gandhi-70536619a/) 2019A7PS0086G
