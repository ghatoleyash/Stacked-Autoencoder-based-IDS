# Deep AutoEncoder IDS

## Introduction
Intrusion Detection System (IDS) is a vital security service which can help us with timely detection. This IDS has become an critical component of network security for this intrusion detection system which is used to monitor network traffic and produce warnings when attacks occur. The data collected by the IDS is obtained from different sources in a computer network and then analyzed for the monitoring purpose. Attacks analyzed by the IDS involve	both intrusions (external attacks) and misuse (internal attacks).Hence, it is important to address this issue when such malicious packets has been sent and must be classified correctly. For this we will be using a deep learning model called autoencoder to classify the packets since, deep learning models can recognise the complex patterns very effectively.

## One-Class Classification
One class classification defines the algorithm is trained over single labeled instances. The motivation for such training methods occurs when the data includes less labeled data of a particular class in such cases where the label of those instances are uncertain it is necessary to make a representation in such a way that the model identifies the pattern inside the labeled data and classify the unlabeled data accordingly. It is similar to a problem of two-class classification, where in this case the two classes has special meaning. Wherein, the two classes are called the target and the outlier/anomaly class. Since, the outlier data is not present readily with the dataset as the outlier might not have occurred in reality hence, containing little or no samples for such anomalies like machine malfunction. Use case of this training method helped in intrusion detection, as there exist the similar case where attacks or unauthorized access to the network are not frequently occurred resulting into detection of anomaly.

## Autoencoder
Autoencoders are the special type of artificial neural network which has the ability to learn the effective representation of the input data called as codings can be seen in figure given below. The coding layer has much less dimensions as compared to the input data, that makes autoencoders in performing dimensionality reduction. Moreover, autoencoders can be a feature detectors and has importance in case of unsupervised training of the model. Additionally, autoencoders can generate new instances similar to the training data. Working of autoencoders is very simple just by copying the input to their output. An autoencoder analyzes the input and converts them to the internal representation and again expands which looks similar to the input. An autoencoder consists of two parts: encoder helps in conversion to internal representation network given by equation, next to encoder there is decoder which converts the internal representation to the outputs seen in equation.
There are 4 hyperparameters that we need to set before training an autoencoder:
- Code size: number of nodes in the middle layer. Smaller size results in more compression.
- Number of layers: the autoencoder can be as deep as we like. In the figure above we have 2 layers in both the encoder and decoder, without considering the input and output.
- Number of nodes per layer: the autoencoder architecture we’re working on is called a stacked autoencoder since the layers are stacked one after another. Usually stacked 
<div align=center><img src=https://github.com/ghatoleyash/Deep-AutoEncoder-IDS/blob/master/autoencoder_architecture.png></div>

## Batch Normalization
There are two major problems that exist while training the deep neural network.
- Internal Covariance Shift: It occurs when there is a change in the input distribution to the network. As the input distribution changes, hidden layers try to adapt to the new distribution. This Internal covariance shift changes the denser regions of input distribution in successive batches during training, the weights modified into the network for the prior iteration are indifferent. This explains why Internal covariance shift can be such a problem that slows down the training process. Due to process slows down, algorithm takes a long time to converge to a global minima 
- Vanishing Gradient: While computing the gradients with the help of partial derivatives. The change that the gradient produced is very little as the model is trained deeper into the layer. This results into modification of weights present at initial layers very less in short the weights are not updated effectively to learn the complex pattern. Hence, the algorithm meets the saturation point from where no improvement in convergence of error function is shown.

Both problems can be solved using batch normalization to some extent. It is achieved through a normalization step that fixes the means and variances in the dense layers. Idea behind usage of normalized training sets with the help of various techniques such as min-max and standard scaler can be applied in batch normalization. In this, hidden layers are trained from the outputs received by activation function of the previous layers which can result into skewed distribution. Hence, to fix such issue normalizing the inputs generated inside the layers is also important so that weights are updated equally without any bias. It does the scaling to output of the layer, specifically performing standardization by considering the statistics such as mean and standard deviation per mini batch. In order to improve the weights which cannot be necessarily correspond to the standard-scaler. It is required to train the two new parameters which are scaling and shifting of the standardized layer inputs as part of the training process


## Experimental Setup
In this section, we discuss details regarding the datasets, preprocessing, and evaluation measures used to assess the efficacy of deep autoencoder model.

#### Datasets
The dataset used for model evaluation is KDD Cup 1999 Dataset products. It includes 126208 records and total of 42 attributes. There are total of 33 continuous attributes and 9 continuous attributes. The attributes such as protocol_type defines the type of protocol used by the packet to get into the network and service which corresponds to network service on the destination, e.g. http, telnet, etc. 

#### Preprocessing
Contained duplicate values removal of those duplicate records and then all of the categorical variables were converted using the one-hot encoding which eventually made total 117 attributes for the dataset. Hence, the remaning continuous attributes were normalized using the standard scaler formula.

#### Validation and Testing
The model is trained based on single class since our model has to detect the intrusion which is very rare. The training set only included the labels with normal connections. But in the validation and test set both labels of the data are included. THe total dataset was split into 60% of training set, 20% of cross validation set and remaining 20% was test set.

#### Model Configuration and Error Metric

Below table shows the configuration of model which is number of hidden layers and input features that are included into the model.

Layer | Shape | Parameters
------|-------|-----------
Input Layer | 118 | 0
Encoder | 64 | 7616
Batch Normalization | 64 | 256
Coding Layer | 32 | 2080
Batch Normalization | 32 | 128
Decoder | 64 | 2112
Batch Normalization | 64 | 256
Output Layer | 118 | 7670

Mean squared error was used as error metric and adam optimizer was used while converging the cost function. The output layer was configured to be linear which was equivalent to the number of features given at the input layer. 

#### Experimental Results
In this section, we analyze the results of experiments performed with deep autoencoder model.

![GitHub Logo](/recall&precisionVSthreshold.png)
![GitHub Logo](/confusion_matrix.png)

- threshold =  1.1
- Accuracy =  97.3
- Recall =  91.83
- Precision =  99.77
- F1-Score =  95.64


## References
- Géron, Aurélien. Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow: Concepts, tools, and techniques to build intelligent systems. O'Reilly Media, 2019.
- Tax, David Martinus Johannes. "One-class classification: Concept learning in the absence of counter-examples." (2002): 0584-0584.
- Chu, Xu, et al. "Data cleaning: Overview and emerging challenges." Proceedings of the 2016 International Conference on Management of Data. 2016.
- Rajendran, Sreeraj, et al. "Crowdsourced wireless spectrum anomaly detection." IEEE Transactions on Cognitive Communications and Networking 6.2 (2019): 694-703.
- "Benchmark dataset for Intrusion Detection System."<http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html>, Accessed on 3 July 2020
