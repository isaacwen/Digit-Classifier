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

At this point, I decided to perform error analysis by analyzing the images that my model was incorrectly classifying. I found that my model was still confusing 4s and 9s, 3s and 5s, and 0s, 6s, and 8s. For some of these, I found that certain features weren't being recognized correctly. For example, between the 3s and 5s, the model was incorrectly classifying some 5s as 3s despite there being no explicit 'upper curve' that we generally observe in 3s. For other examples, such as some between 0s and 6s, the model was incorrectly classifying 6s as 0s, although it would have been more apparent that the digit was a 6 if it had been rotated around 30 degrees.

## Version 3 - 99.62%
Based on the error analysis for the previous version, I decided to make the following modifications to my model:
* For the data augmentation, I increased the rotation of the images to 30 degrees, such that the training examples have a chance to be rotated by up to 30 degrees at random.
* I removed the pooling layers from my model and replaced them with additional convolutional layers, with the goal of having my model being able to identify a larger quantity and more sophisticated characteristics of the digits.

With these changes I was able to achieve 99.62% accuracy.

## Version 4 - 99.73%
I was not able to achieve a dramatic increase in accuracy from 99.62% using just a singuler model after playing around with the hyperparameters. However, when doing some additonal research I was able to identify ensemble learning as a potential improvement.

To implement ensemble learning, I created three new models using different convolutional layers. Specifically, I created one model using solely convolutional layers with 3x3 filters, a second model using predominantly convolutional layers with 5x5 filters, and a third model that uses convolutional layers with 7x7 layers. The goal of using models with different convolutional layers is to have the models capture different features during training.

For these three models, accuracies of 99.62%, 99.66%, and 99.65% were achieved.

To make collective predictions from these three models, I used the following method:
* If a majority of the three models predict a certain digit, then that digit will be predicted.
* If each model predicts a different digit, the digit which has the highest total confidence % across all three models will be used.

After implementing ensemble learning, an accuracy of 99.73% was achieved.

## Next Steps
