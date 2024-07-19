# MonReader
## Introduction
### Problem Statement
We collected smartphone video data of page flipping, labeling each instance as either flipping or not flipping. The videos were clipped into short segments, and frames were extracted with the naming structure VideoID_FrameNumber. The goal was to develop a solution to predict whether a single frame image indicates a page flip and whether a sequence of images contains a flipping action.

### Data
Data is available [here](https://drive.google.com/file/d/1KDQBTbo5deKGCdVV_xIujscn5ImxW4dm/view).

![image](https://github.com/user-attachments/assets/c0789aaa-5886-457e-a8f0-7395094fe06f)

### Goal
Achieve high classification accuracy

## Methodology
### Image classification
The data was rescaled from the original dimensions of 1920px x 1080px to 320px x 180px to reduce dimensionality and computation time. A 2D Convolutional Neural Network (CNN) was implemented with three convolution layers and three max-pooling layers, followed by a dense layer and an output layer. Early stopping was applied to avoid overfitting. The model achieved 90.3% accuracy on the test dataset.

![image](https://github.com/user-attachments/assets/a63a504a-0c24-45c6-b084-55584fb91b6b)

To improve performance, we used transfer learning with the Inception model. We removed the top layers and replaced them with a fully connected layer and a sigmoid output layer, training only the weights in the top two layers. This model achieved 98.3% accuracy on the test dataset.

![image](https://github.com/user-attachments/assets/1421dd89-d2a2-4f7f-8d9e-e5029bc5bdb6)

### Sequence classification
To detect whether a sequence of images corresponds to a flip action, we built (i) a 3D Convolutional Neural Network (3D CNN) and (ii) a CNN-LSTM hybrid model.

For the 3D CNN, we used three 3D convolution layers, followed by three max-pooling layers, one fully connected layer, and a sigmoid output layer, with early stopping. This model achieved 75.9% accuracy on the test data.

![image](https://github.com/user-attachments/assets/0f64e36f-0c4d-4514-a539-a35a1474faff)

Using the CNN-LSTM model, we added an LSTM sequence layer to the tuned Inception model for image classification. This hybrid model achieved 96.4% accuracy on the test data in classifying whether a sequence of images denotes a flipping action.

![image](https://github.com/user-attachments/assets/2d69ffe1-7a0e-4dc9-bb44-2661917f6eb1)

## Summary
Overall, we achieved excellent results: 98.3% accuracy in predicting whether a single image denotes a flipping action using transfer learning with the Inception model, and 96.4% accuracy in predicting whether a sequence of images denotes a flipping action using the hybrid CNN-LSTM model.
