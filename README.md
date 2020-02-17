## Technical report of image segmentation

### Task 1

#### Problem description

Here the problem of multi-class face segmentation is introduced. We're given a dataset with around of 1500 images of people. The main task is to allocate human on photo using Computer Vision.

#### Challenges

Our model can encounter with some difficulties, because it has to learn how to find all endges and make a right conclusion. We should do some manipulations with the image like convolution and pooling to classify pixels. And also to get back to our initial resolution we have to do some unsampling techniques like interpolation, transposed convolution etc.

#### Model description

In our case we will use the U-net architecture. It is synonymous with an encoder-decoder architecture. Essentially, it is a deep-learning framework based on FCNs; it comprises two parts:
1. A contracting pasth similar to an encoder, to capture context via a compact feature map.
2. A symmetric expanding path similar to a decoder, which allows precise localization. This step is done to retain boundary information despite down sampling and max-pooling performed in the encoder stage.

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/model.png)


Advantages of using U-Net:
- Computationally efficient 
- Trainable with a small data-set 
- Trained end-to-end

As U-Net model I used ResNet34, because it is quite lightweight and in most of cases it is the baseline.

#### Training/validation strategy

We can read the original paper of UNet [here](https://arxiv.org/pdf/1505.04597.pdf). However, I did one change to get better result. As seen below, the original paper used stochastic gradient descent optimizer, I just used an Adam Optimizer and Binary Cross Entropy with logits as our loss function

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/1.png)


Specs, of default setup for running for the first time:
- Number of epochs: 150;
- Batch size: 4;
- Learning rate: 0.001;
- Loss function: Binary Crossentropy with Logits;
- Image Resolution: 320x320;

Parameters, which increased validation dice during fine tuning:
- Batch size: 8 
- Learning rate: 3e-4

#### Experiment log

##### Tried the UNet16 with default setup:

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/unet16.png)

##### Tried the UNet11 with default setup:

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/unet11.png)

##### ResNet34: first run (not full log) validation dice was:

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/default_log.jpg)

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/example1.png)

##### Increased batch to 8

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/2.png)

##### Changed the Learning Rate to 3e-4

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/f1.png)

#### Conclusion

Changing batch size to 8 improve the results validation dice decreased to 0.871 (was 0.802) and validation loss decreases to 0.242 (was 0.275) after 20 epochs training. And changing the Learning rate to 3e-4 improved the results of validation dice that became 0.927 (was 0.871) and validation loss decresed to 0.146 (was 0.275)

##### Final result

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/final.png)

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/ex2.png)

![](https://raw.githubusercontent.com/MaxVoloskiy/CV_Assignment2/master/assets/ex3.png)

#### Discussion

All borders are predicted correctly, but the problem is with those regions, which are simmilar to body parts. Also borders can be blured. All of it can be solved by tuning dice loss function - with right parameters border will be more smoother. There are also other solutions of increasing validation dice but, of course, it will increase time of training, but the result would be better.
