# Image-Caption-Generation
Given an image, it will return the most probable sequence of words (sentence) describing the image. 


# Requirnments
    h5py==2.5.0
    Keras==2.0.3
    numpy==1.11.2
    tensorflow==1.1.0
    pandas==0.16.2
    
# Dataset
    
The dataset used is flickr8k. You can request the data here(https://forms.illinois.edu/sec/1713398). An email for the links of the data to be downloaded will be mailed to your id. Extract the images in Flickr8K_Data and the text data in Flickr8K_Text. <br>


# How can we define a single training example?

1. We are going to generate one word at a time inorder to generate complete sentence. To generate each word, we will provide 2 types of inputs.
    1. Image
    2. Sentence that has already been predicted so that model can use the context and predict next word.
2. How our training sample is going to look?
    1. We have to add 2 special tokens to each captions that represents start of sentence and end of sentence.
    2. Then we need to split each caption and image pair in multiple data samples.
    
3. So corresponding to a single image and a caption, we are going to generate multiple data samples.

## Structure of a generic model for image captioning
> https://arxiv.org/pdf/1703.09137.pdf Depending upon the choice, your structure of model will change.

### Extracting image features
1. To extract image features, we can use CNN network. Either we can make our own CNN network or we can use concept of transfer learning. There are a lot of pre-trained models available to extract image features. For example, InceptionV3 model contains 48 layers which is used to classify image in 1 out of 1000 classes. Last layer of this model is used for classification, so we can capture the output of second last layer (which is a vector of size 2048 for a single image) as it will represent image features in form of numbers without classifying them in classes.
2. So, we can pass each image in our dataset through this network and store the results corresponding to image id in a file.

### Providing text features
1. We are going to use Word Embedding to represent words!
2. We will also need to define a maximum length which a sentence can take so that we could define size of our inputs because we are going to input a group of words to our model and expect a single word output.
3. For example, in Flickr8K dataset, maximum length of sentence is around 34. Let us say we have a map of words to integer as {"a": 1, "girl": 2, "running": 3, "near": 4}, so to encode a sentence 'a girl', we will make an array of size 32 (maximum length of sentence), put 1st two integers using word to index mapping as [1,2] and append this with zeros which represent no word. So, our input vector representing a word is going to look like [1,2,0,0,0,...] and so on of size 34. This input will be fed to a word embedding layer. 


### What is encoder in such models?
The neural networks that changes any input in its features representation i.e vector of numbers is encoder. For example, we want to use image to predict words. As image directly can't tell what should be the word, we want to use its feature to help us decide the next word. And thus the network  of layers used to change image or any other type of input in its feature representation is known as encoders.
### What is decoder?

The combination of layers/neural network that takes feature representation provided by encoder as its own input and predicts the next word, is known as decoder.

# ...
### Caption Generation.

For Caption Generation i used Gready Search approach, you can use Beam search approach for better Results.
# Research papers and resources to follow
> 1. https://arxiv.org/pdf/1411.4555.pdf
> 2. https://arxiv.org/pdf/1412.2306.pdf
> 3. https://www.youtube.com/watch?v=yCC09vCHzF8 (Video lecture by author of paper 2)
> 4. https://machinelearningmastery.com/develop-a-deep-learning-caption-generation-model-in-python/
