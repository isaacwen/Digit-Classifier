# Digit Classifier
I created several iterations of a CNN for classifying handwritten digits in TensorFlow. The models are trained on the MNIST data set and my final version achieves 99.73% accuracy.

The model and trained weights for each version are in the corresponding folders. To see the accuracies for each model, see the accuracy.ipynb file.

The following is an outline of design choices and optimizations I added to achieve my final accuracy.

## Version 1 - 99.27%
My goal for this version was to create a very simple model and to use the accuracy/loss of this model to determine what optimizations I should add.

My model consisted of two pairs of convolutional/pooling layers, as well as two dense layers. Using a LearningRateScheduler, I identified an estimate for an optimal learning rate and used it to train my model to 99.27% accuracy.

While the training and validation accuracy both stabilized with an increased number of epochs, they did not converge to a similar percentage and the validation loss also increased over time, suggesting that my model is overfitting.

<p align="center"><img src="https://user-images.githubusercontent.com/76772867/149642556-2f34e316-9f96-46d1-a610-91e91075c66a.png" /></p>

## Version 2 - 99.48%
As my first model had increasing validation loss over time suggesting overfitting, there were 3 things I identified that I could improve from the model:
* Since my model was created to be very simple, I could increase the number of units in the one hidden dense layer as well as increase the number of filters that my convolutional layers use.
* The learning rate that I used was a rough estimate for the optimal learning rate and I kept it static throughout the training process. It is also likely too large. I could implement learning rate decay to mitigate these issues.
* My dataset could be too small. To fix this, I can implement image augmentation to effectively increase my dataset size by introducing random variations in my data.

Implementing these changes into my model allowed me to achieve 99.48% accuracy and for my model to have training/validation accuracies that converged to the same point and a validation loss that plateaus.

<p align="center"><img src="https://user-images.githubusercontent.com/76772867/149646688-679e4813-6db0-43ef-879b-6c44e10dae8e.png" /></p>

At this point, I decided to perform error analysis by analyzing the images that my model was incorrectly classifying. I found that my model was still confusing 4s and 9s, 3s and 5s, and 0s, 6s, and 8s. For some of these, I found that certain features weren't being recognized correctly. For example, between the 3s and 5s, the model was incorrectly classifying some 5s as 3s despite there being no explicit 'upper curve' that we generally observe in 3s.

<p align="center"><img src="https://user-images.githubusercontent.com/76772867/149647023-f2e5ae96-fb4c-4d6d-b051-8b4354dc9bb8.png" /></p>

For other examples, such as some between 0s and 6s, the model was incorrectly classifying 6s as 0s, although it would have been more apparent that the digit was a 6 if it had been rotated around 30 degrees.

<p align="center"><img src="https://user-images.githubusercontent.com/76772867/149647045-76e3ee2b-488a-4b9f-8ac3-f13fba47700c.png" /></p>

## Version 3 - 99.62%
Based on the error analysis for the previous version, I decided to make the following modifications to my model:
* For the data augmentation, I increased the rotation of the images to 30 degrees, such that the training examples have a chance to be rotated by up to 30 degrees at random.
* I removed the pooling layers from my model and replaced them with additional convolutional layers, with the goal of having my model being able to identify a larger quantity and more sophisticated characteristics of the digits.

With these changes I was able to achieve 99.62% accuracy.

## Version 4 - 99.73%
I was not able to achieve a dramatic increase in accuracy from 99.62% using just a singular model after playing around with the hyperparameters. However, when doing some additional research I was able to identify ensemble learning as a potential improvement.

To implement ensemble learning, I created three new models using different convolutional layers. Specifically, I created one model using solely convolutional layers with 3x3 filters, a second model using predominantly convolutional layers with 5x5 filters, and a third model that uses convolutional layers with 7x7 layers. The goal of using models with different convolutional layers is to have the models capture different features of the digits during training.

For these three models, accuracies of 99.62%, 99.66%, and 99.65% were achieved, respectively.

To make collective predictions from these three models, I used the following method:
* If a majority of the three models predict a certain digit, then that digit will be the collective prediction.
* If each model predicts a different digit, the digit which has the highest total confidence % across all three models will be used as the collective prediction.

After implementing ensemble learning, an accuracy of 99.73% was achieved.

## Next Steps
From the final model using ensembling learning, I noted that quite a few of the incorrectly classified examples were still correctly classified by one of the models. This suggests that a further enhancement could be to add additional models to the ensemble. For these models, instead of creating completely new models with a network architecture distinct from the three that I have created already, I could instead re-use the existing models that I have and just add more models into the ensemble using the same architecture. This would help minimize the influence of the random initialization of weights for each model.
