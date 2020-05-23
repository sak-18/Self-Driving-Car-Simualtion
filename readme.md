# Self driving Car Simulation
# Installation_requirements
## Unity Game engine
You can install Unity game engine at https://unity3d.com .You can use personal version for udacity self driving car simulation.
## Git LFS
Our car simulator uses Git LFS to store large files, by using pointers and stores the files on remote server like https://github.com .Make sure Git is installed before you you use git lfs.Refer this video to install git on your machine. https://www.youtube.com/watch?v=J_Clau1bYco

## Udacity self driving car simulator
Open terminal on your computer and clone the project.
## Dependencies
You may need anaconda installed for setting up the required environment.

# Project Description
_________________
We used two models to train the self driving car simulator.one was based on Nvidia Model and we used another model based on VGG16 by removing top layer.
It is a supervised learning model based on the images captured by car and corresponding steering angles.It is sequential and single agent based.

# **Nvidia Model**
As the image is processed, the model is using convolutional layers for automated feature engineering.
* model.py   used to create and train the model.
* drive.py Script to drive the car and you are free to  make changes and resubmit your modified version.
* Utils.py to make augmentation and image processing during trainig mode.
* model.h5 is pretrained model.
* environments.yml conda environment(use tensorflow without GPU)
* environments-gpu.yml conda environment (tensorflow with GPU)

# **Model Architecture design**
_______________________

The design of the network based on Nvidia Model and we made some changes to reduce overfitting and introduced non linearity.
* We used dropout layer to avoid overfitting and used lambda layer to make gradients work better and by using ELU activation function we were able to introduce non-linearity.

In the end model looks as follows:
* Image normalization
* Convolution by 5 * 5 for first 3 layers and and then two layers by 3 * 3 with filter sizes of 24,36,48,64,64 for the 5 layers respectively and strides of 2 * 2 and 1 * 1 with every layer activated by ELU to introduce non linearity.
* Drop out (0.5)
Then flattened directly fed as input to Neuralnet fully connected layer with 100,50,10,1 With all levels except output layer by ElU function.

Convolution layers are used for feature extraction and neural network is used for steering angle prediction based on the processed model.

| Layer type       | output shape | params  |connected_to|
| ------------- |:-------------:|:-----:|-------|
| lambda_1 (Lambda)    | (None, 66, 200, 3) | 0 | lambda_input_1|
| convolution2d_1 (Convolution2D) | (None, 31, 98, 24)      |   1824| lambda_1|
| convolution2d_2 (Convolution2D)|(None, 14, 47, 36)     |  21636 | convolution2d_1|
|convolution2d_3 (Convolution2D)|(None, 5, 22, 48)|43248|convolution2d_2|
|convolution2d_4 (Convolution2D)|(None, 3, 20, 64)|27712|convolution2d_3|
|convolution2d_5 (Convolution2D)|(None, 1, 18, 64)|36928|convolution2d_4|
|dropout_1 (Dropout)|(None, 1, 18, 64)|0|convolution2d_5|
|flatten_1 (Flatten)|(None, 1152)|0|dropout_1|
|dense_1 (Dense)|(None, 100)|115300|flatten_1|
|dense_2 (Dense)|(None, 50)|5050|dense_1|
|dense_3 (Dense)|(None, 10)|510|dense_2|
|dense_4 (Dense)|(None, 1)|11|dense_3| 
