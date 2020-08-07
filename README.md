# AutoIDS

## Introduction
Sending a packet inside the host's network will yeild in gain of access that could lead to security breach and leaking of the information. To identify any malicious activity after a system has been attacked it will be too late. Hence, it is important to address this issue when such malicious packets has been sent and must be classified correctly. For this we will be using a deep learning model called autoencoder to classify the packets since deep learning models can recognise the complex patterns very effectively.

## Anomaly Detection

## Batch Normalization

## AutoEncoder
Autoencoders are a specific type of feedforward neural networks where the input is the same as the output. They compress the input into a lower-dimensional code and then reconstruct the output from this representation. The code is a compact “summary” or “compression” of the input, also called the latent-space representation.
An autoencoder consists of 3 components: encoder, code and decoder. The encoder compresses the input and produces the code, the decoder then reconstructs the input only using this code.
There are 4 hyperparameters that we need to set before training an autoencoder:
- Code size: number of nodes in the middle layer. Smaller size results in more compression.
- Number of layers: the autoencoder can be as deep as we like. In the figure above we have 2 layers in both the encoder and decoder, without considering the input and output.
- Number of nodes per layer: the autoencoder architecture we’re working on is called a stacked autoencoder since the layers are stacked one after another. Usually stacked autoencoders look like a “sandwitch”. The number of nodes per layer decreases with each subsequent layer of the encoder, and increases back in the decoder. Also the decoder is symmetric to the encoder in terms of layer structure. As noted above this is not necessary and we have total control over these parameters.
- Loss function: we either use mean squared error (mse) or binary crossentropy. If the input values are in the range [0, 1] then we typically use crossentropy, otherwise we use the mean squared error.

## Experimental Setup
In this section, we discuss details regarding the datasets, preprocessing, and evaluation measures used to assess the efficacy of DeepER models.

#### Datasets
The dataset used for model evaluation is KDD Cup 1999 Dataset products. It includes 126208 records and total of 42 attributes. There are total of 33 continuous attributes and 9 continuous attributes. The attributes such as protocol_type defines the type of protocol used by the packet to get into the network and service which corresponds to network service on the destination, e.g., http, telnet, etc. 

#### Preprocessing
Contained duplicate values removal of those duplicate records and then all of the categorical variables were converted using the one-hot encoding which eventually made total 117 attributes for the dataset. Hence, the remaning continuous attributes were normalized using the standard scaler formula.

#### Validation and Testing
The model is trained based on single class since our model has to detect the intrusion which is very rare. The training set only included the labels with normal connections. But in the validation and test set both labels of the data are included. THe total dataset was split into 60% of training set, 20% of cross validation set and remaining 20% was test set.

#### Model Configuration and Error Metric

Below table shows the configuration of model which is number of hidden layers and input features that are included into the model.

Layer                          Shape                     Parameters
====================================================================
Input Layer                    118                       0
Encoder                        64                        7616
Batch Normalization            64                        256
Coding Layer                   32                        2080
Batch Normalization            32                        128
Decoder                        64                        2080
Batch Normalization            64                        256
Output Layer                   118                       7670
====================================================================
Mean squared error was used as error metric and adam optimizer was used while converging the cost function. The output layer was configured to be linear which was equivalen to the number of features given at the input layer. 

#### Experimental Results
In this section, we analyze the results of experiments performed with autoencoder.


