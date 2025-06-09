# Gesture Recognition Using Neural Networks

---

## Contents

1. 
2. [Introduction](#introduction)  
2. [Understanding the Dataset](#understanding-the-dataset)  
3. [Architectures for Video Analysis](#architectures-for-video-analysis)  
    - CNN + RNN Architecture  
    - 3D Convolutional Neural Networks (Conv3D)  
    - Transfer Learning + RNN Architecture  
4. [Data Generator](#data-generator)  
5. [Neural Network Architecture Selection and Training](#neural-network-architecture-selection-and-training)  
    - Procedure  
    - Model Hyperparameters  
6. [Observation Summary](#observation-summary)  
7. [Final Model Architecture](#final-model-architecture)  

---

## Introduction

The objective of this case study is to develop an advanced feature for a state-of-the-art smart TV that enhances user interaction through gesture recognition. Using the TV's built-in webcam, this feature continuously monitors for five distinct gestures, each of which corresponds to a specific command. This functionality enables users to control the TV effortlessly, eliminating the need for a remote and providing a hands-free, intuitive experience.

The gestures are continuously monitored by the webcam mounted on the TV. Each gesture corresponds to a specific command:

- **Thumbs up:** Increase volume  
- **Thumbs down:** Decrease volume  
- **Left swipe:** Rewind 10 seconds  
- **Right swipe:** Forward 10 seconds  
- **Stop:** Pause movie

---

## Understanding the Dataset

The training data consists of a few hundred videos categorized into one of the five classes. Each video (typically 2-3 seconds long) is divided into a sequence of 30 frames(images). These videos have been recorded by various people performing one of the five gestures in front of a webcam - similar to what the smart TV will use.

The task involves training various models on the 'train' folder to accurately predict the action performed in each sequence or video, while also ensuring strong performance on the 'val' folder. The final model's performance will be evaluated on the 'val' set, which is withheld for the final assessment.

---

## Architectures for Video Analysis
### CNN + RNN architecture
The Conv2D network processes each image in the input sequence independently, extracting a feature vector for each image. These feature vectors form a sequence, which is then fed into an RNN-based network. The RNN processes the sequence of feature vectors and generates an output. In a classification problem, such as the one described, the output of the RNN is passed through a regular softmax layer to produce the final classification probabilities.

![CNN + RNN (LSTM) Architecture](CNN%20%2B%20RNN%20%28LSTM%29%20Architecture.png)

### 3D Convolutional Neural Networks (Conv3D)

3D convolutions expand the concept of 2D convolutions by introducing a third dimension, enabling filters to move across three axes: x, y, and z. This approach is particularly useful in video analysis, where sequences of frames are treated as 4D tensors. A 3D convolutional filter is represented as (f x f x f) x c, where fff is the filter size and ccc is the number of channels, allowing the filter to capture spatial and temporal information across multiple frames.

![Conv 3D Architecture.png](Conv%203D%20Architecture.png)

### Transfer Learning + RNN Architecture

The idea behind using transfer learning is to leverage learning from models which were trained on vast amounts of data since the computational resources and data availability are constraints for Neural Network training. Pre-trained CNN architectures such as ResNet can be used to extract spatial features from images which are then passed through an RNN to capture temporal dependencies across the frames. 
![ResNet50 + RNN (LSTM) Architecture.png](ResNet50%20%2B%20RNN%20%28LSTM%29%20Architecture.png)
---

## Data Generator

Generator function is used to import batches of data as the training proceeds since having the entire data in memory is impractical while dealing with large datasets such as the current one. Here, 
- The sequence of image indices to be considered will be used to determine which frames will finally get used in model training and evaluation.
- Image dimensions are chosen to be $80\times80$ since we have images of two different dimensions in our dataset. Appropriate cropping and resizing is done within this code.
- Normalization of images is done to ensure data is consistent and on the same scale. This helps in the training process for gradient descent.
- Batch labels are converted to one-hot encoding vectors to help with the classification tasks.
- The code is made robust enough to handle the case when the number of training data points isn't divisible by the chosen batch size. This edge case is handled by adding one additional batch containing the remaining data points.
- The generator function finally yields the batch data and the corresponding one-hot encoded label vector which the model can use for training and validation.
- This custom generator function is written since the keras generator function doesn't allow customization for our specific use case.


---

## Neural Network Architecture Selection and Training

### Procedure
1. **Model Configuration and Hyperparameter Tuning:**
    - Various model architectures were configured with different layer counts, incorporating batch normalization and dropout layers in multiple combinations.
    - A range of hyperparameters was tested, including different numbers of epochs, batch sizes and learning rates.
    - The ReduceLROnPlateau technique was applied to dynamically adjust the learning rate whenever validation loss showed no improvement across epochs.


2. **Optimizer Selection**:
    - Models were trained using the Adam optimizer, with a focus on the AMSGrad variant, which demonstrated the best performance.
    - Adagrad and Adadelta optimizers were excluded to avoid the increased computational cost and training time required for their dynamic learning rate adjustments.


3. **Overfitting Prevention**:
    - The model was monitored for signs of overfitting, such as a noticeable gap between training and validation accuracy.
    - Batch Normalization, pooling layers, and dropout layers were incorporated as needed to reduce overfitting.


### Model Hyperparameters

- **Trainable Parameters:** It was noted that as the number of trainable parameters increased, the training time of the model also increased. It also leads to overfitting on certain instances.


- **Batch Normalization and Dropout:** Performing Batch Normalization and adding dropout layers to the model improved its generalization, leading to better performance on unseen data.


- **Batch Size and GPU Memory:** The batch size was directly proportional to the available GPU memory and computational resources. Using a large batch size sometimes resulted in GPU Out of Memory errors, necessitating adjustments to determine an optimal batch size that the GPU could support.


- **Trade-off Between Batch Size and Accuracy:** Increasing the batch size significantly reduced training time, but it also had a detrimental effect on the model's accuracy. This trade-off necessitated a decision between shorter training times and higher accuracy by selecting an appropriate batch size.


- **Mitigation of Overfitting:** To mitigate overfitting call-backs were used. These include adaptive learning rate, early stopping, and periodically saving model parameters.


- **Optimal Model Selection:** The experimentation identified a Conv3D model with the Adam optimizer using the AMSGrad algorithm as the best-performing model among those trained. The performance of these architectures depended on various factors, including dataset characteristics, architectural design, and the chosen hyperparameters.


- The loss function used was **categorical cross entropy** and the model evaluation metric used is **Accuracy**. 

---

## Observation Summary

| Model | Architecture | Training Acc | Validation Acc | Notes |
|-------|--------------|--------------|----------------|-------|
| 1     | Conv3D       | 0.19         | 0.18           | No regularization, low accuracy |
| 2     | Conv3D       | 0.23         | 0.18           | Added dropout and batch norm, still low |
| 3     | Conv3D       | 0.39         | 0.36           | Deeper network, some overfitting |
| 4     | Conv3D       | 0.90         | 0.77           | AMSGrad, LR=0.0005, best Conv3D |
| 5     | CNN + RNN    | 0.19         | 0.19           | Low generalization |
| 6     | CNN + GRU    | 0.28         | 0.27           | Batch norm and dropout, poor validation |
| 7     | CNN + LSTM   | 1.00         | 0.86           | Overfitting evident |
| 8     | ResNet + GRU | 0.53         | 0.55           | Some generalization |
| 9     | ResNet + LSTM| 0.85         | 0.56           | Overfitting issues |

---

## Final Model Architecture
![Model Architecture.png](Model%20Architecture.png)

![Model accuracy vs Epoch.png](Model%20accuracy%20vs%20Epoch.png)

![Model loss vs Epoch.png](Model%20loss%20vs%20Epoch.png)
---