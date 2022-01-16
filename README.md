# Digit Classifier
I created a CNN for classifying handwritten digits in TensorFlow. The model is trained on the MNIST data set and my final version achieves 99.73% accuracy.

The files corresponding to each version are in the corresponding folders. The following is an outline of design choices and optimizations I added to achieve my final, state-of-the-art accuracy.

## Version 1 - 99.27%
My goal for this version was to create a very simple model, and use the accuracy/loss of the model to determine what optimizations I should add.

My model consisted of two pairs of convolutional/pooling layers, as well as two dense layers. Using a LearningRateScheduler, I identified an estimate for an optimal learning rate and used it to train my model to 99.27% accuracy.

While the training and validation accuracy both stabilized with an increased number of epochs, they did not converge to a similar percentage and the validation loss also increased over time, suggesting that my model is overfitting.

![image](https://user-images.githubusercontent.com/76772867/149642556-2f34e316-9f96-46d1-a610-91e91075c66a.png)

## Version 2 - 99.48%
As my first model had increasing validation loss over time suggesting overfitting, there were 3 things I identified that I could improve from the model to prevent overfitting:
* Since my model was created to be very simple, I could increase the number of units in the one hidden dense layer as well as increase the number of filters that my convolutional layers use.
* The learning rate that I used was a rough estimate for the optimal learning rate that I kept static throughout the training process, and it was likely too large. I could implement learning rate decay to prevent this.
* My dataset could be too small. To fix this, I can implement image augmentation to effectively increase my dataset size by introducing random variations in my data.

Implementing these changes into my model allowed me to achieve 99.48% accuracy and for my model to have a validation loss that stabilized over time.
![image](https://user-images.githubusercontent.com/76772867/149642703-e4403a86-ac9e-400a-bc9d-4ad5d4a2f814.png)

At this point, I decided to perform error analysis by analyzing the images that my model was incorrectly classifying. I found that my model was still confusing 4s and 9s, 3s and 5s, and 0s, 6s, and 8s. For some of these, 
