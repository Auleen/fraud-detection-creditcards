
# Credit Card Fraud Detection Model.
ML-based fraud detection model that is effective at identifying evolving fraud patterns even in the presence of imbalanced data.

Worldwide financial losses caused by credit card fraudulent activities are worth tens of billions of dollars. One American over ten has been a victim of credit card fraud (median amount of $399), according to the Statistic Brain Research Institute. According to the latest European Central Bank (ECB) report, the total level of card fraud losses amounted to €1.8 billion in 2018 in the Single European Payment Area (SEPA).


## Machine Learning in Credit Card Fraud Detection

Credit card fraud detection is like looking for needles in a haystack. It requires finding, out of millions of daily transactions, which ones are fraudulent. Due to the ever-increasing amount of data, it is now almost impossible for a human specialist to detect meaningful patterns from transaction data. For this reason, the use of machine learning techniques is now widespread in the field of fraud detection, where information extraction from large datasets is required

A wide number of ML techniques can be used to address the problem of CCFD. This is directly reflected by the huge amount of published papers on the topic in the last decade. Despite this large volume of research work, most of the proposed approaches follow a common baseline ML methodology, which involves a Supervised Learning methodology (Decision Trees,Logistic Regression,Isolation Forest and etc)

The schema can be summarised by the following diagram:

![App Screenshot](https://i.postimg.cc/v8pBnDfZ/baseline-ML-workflow-subset.png)
## Finding the dataset

Credit card fraud is a relatively rare event, and the prevalence of fraudulent transactions is low compared to legitimate transactions. This means that datasets are usually imbalanced, with a vast majority of transactions being legitimate, making it difficult to identify the patterns and features of fraudulent transactions. \

Over that ,financial institutions and merchants are reluctant to share their transaction data due to privacy and security concerns. As a result, datasets are often limited in size and may not represent the full spectrum of fraudulent activities.

Hence it is difficult to find legitimate dataset for CCFD with well labeled features for training our model.

Therefore, the best way to get a sense of the challenges underlying the design of a credit card fraud detection syste is by designing one. For the purpose of this particular project we will be using a simulated transaction dataset which can be found at https://github.com/Fraud-Detection-Handbook/simulated-data-transformed

## Understanding Our Data

For the purpose of this project we are using simulated datasets to represent transaction data of Fraud and Legitimate transactions.

The simulated dataset highlights most of the issues that practitioners of fraud detection face using real-world data. In particular, they will include class imbalance (less than 1% of fraudulent transactions), a mix of numerical and categorical features (with categorical features involving a very large number of values), non-trivial relationships between features, and time-dependent fraud scenarios.

The working of the simulator is beyond the scope of this project but all necessary links and resources will be mentioned at the end of this section.

Our focus will be on the most essential features of a transaction. In essence, a payment card transaction consists of any amount paid to a merchant by a customer at a certain time. The six main features that summarise a transaction therefore are: 

        1. The transaction ID: A unique identifier for the transaction

        2. The date and time: Date and time at which the transaction occurs

        3. The customer ID: The identifier for the customer. Each customer has a unique identifier

        4. The terminal ID: The identifier for the merchant (or more precisely the terminal). Each terminal has a unique identifier

        5. The transaction amount: The amount of the transaction.

        6. The fraud label: A binary variable, with the value 0 for a legitimate transaction, or the value 1for a fraudulent transaction.

This forms the base of our dataset. Since the base dataset consists of both numerical/ordered and categorical data, we further perform feature transformations to convert categorical pieces of data to numerical and ordered data for the machine learning model to interpret.

The first type of transformation involves the date/time variable, and consists in creating binary features that characterize potentially relevant periods. We will create two such features. The first one will characterize whether a transaction occurs during a weekday or during the weekend. The second will characterize whether a transaction occurs during the day or the night. These features can be useful since it has been observed in real-world datasets that fraudulent patterns differ between weekdays and weekends, and between the day and night.

The second type of transformation involves the customer ID and consists in creating features that characterize the customer spending behaviors. We will follow the RFM (Recency, Frequency, Monetary value) framework, and keep track of the average spending amount and number of transactions for each customer and for three window sizes. This will lead to the creation of six new features.

The third type of transformation involves the terminal ID and consists in creating new features that characterize the ‘risk’ associated with the terminal. The risk will be defined as the average number of frauds that were observed on the terminal for three window sizes. This will lead to the creation of three new features.

The simulator for transaction data has been released as part of the practical handbook on Machine Learning for Credit Card Fraud Detection - https://fraud-detection-handbook.github.io/fraud-detection-handbook/Chapter_3_GettingStarted/SimulatedDataset.html.
## Resampling the data
n credit card fraud detection, the number of fraudulent transactions is often much smaller than the number of non-fraudulent transactions. This means that if a model is trained on an imbalanced dataset, it may have a bias towards the majority class (i.e., non-fraudulent transactions), and may not be able to accurately detect fraudulent transactions.

Resampling can help to address this issue by balancing the number of samples in each class. There are two main approaches to resampling: oversampling and undersampling.

Oversampling involves adding more copies of the minority class to the dataset until it has the same number of samples as the majority class. This can be done using techniques such as random oversampling or Synthetic Minority Over-sampling Technique (SMOTE).

Undersampling involves reducing the number of samples in the majority class until it has the same number of samples as the minority class. This can be done using techniques such as random undersampling or Tomek links.

![App Screenshot](https://i.postimg.cc/hPWjh4Nq/undsampling.png)

For the purpose of this project we will be using Random Undersampling to create a dataset of equal legitimate and fraud data points.

## Building a Classification Model
After we have resampled our data, we will now train different models to work on our prepared data. 
### Decision Tree Model
We will first train a decison tree model to check for fraud credit card transactions.
A decision tree is a powerful machine learning algorithm that can be used for credit card fraud detection. The algorithm works by constructing a tree-like model of decisions and their possible consequences. Each node in the tree represents a decision, and the branches emanating from it represent the possible outcomes of that decision. 
A decision tree model works by recursively partitioning the data based on the feature that provides the most information gain, or the most useful split in the data. The model starts with a single node that represents the entire dataset, and then iteratively splits the data into smaller subsets based on the values of the features.

At each split, the algorithm selects the feature that best separates the classes, or categories, of the target variable. It does this by calculating a measure of impurity, such as entropy or Gini impurity, for each possible split. The feature that results in the greatest reduction in impurity is chosen as the splitting criterion for that node.

The process of splitting the data continues recursively until a stopping criterion is met, such as reaching a maximum depth or a minimum number of samples in a leaf node. At this point, the decision tree is fully grown and can be used to make predictions for new data.

![App Screenshot](https://i.postimg.cc/Jhvfz0tw/decisiontree.png)

To make a prediction for a new observation, the decision tree starts at the root node and traverses down the tree based on the feature values of the observation. The prediction is then based on the majority class of the training data in the corresponding leaf node.

Decision trees are useful because they are easy to interpret and visualize. The structure of the tree makes it clear which features are most important in making predictions, and how the decision is made. However, they can also be prone to overfitting, where the model is too complex and fits the noise in the data rather than the underlying patterns. Regularization techniques such as pruning can help to prevent overfitting and improve the generalization performance of the model.

### Random Cut Forest
Random Forest is a machine learning algorithm that is used for both classification and regression tasks. It works by creating an ensemble of decision trees, where each tree is trained on a randomly selected subset of the training data, and a randomly selected subset of the features.

The algorithm starts by creating a specified number of decision trees, where each tree is constructed by randomly selecting a subset of the training data, with replacement. This process is known as bootstrapping, and it is used to create different versions of the training data for each tree.

At each node of each decision tree, a random subset of the features is selected, and the feature that provides the best split on that subset is used to make the split. This helps to reduce the correlation between the decision trees, making the random forest more robust to overfitting and improving the accuracy of the model.

![App Screenshot](https://i.postimg.cc/L52mBDfB/rfc2.png)

To make a prediction for a new observation, the random forest algorithm aggregates the predictions from all the decision trees in the ensemble. For classification tasks, the prediction is based on the majority class of the predictions from the individual trees. For regression tasks, the prediction is based on the average of the predictions from the individual trees.

### RFC vs Decision Tree

![App Screenshot](https://i.postimg.cc/Y2Z5HbjX/rfcvsdt.png)

A Decision Tree is a model that uses a tree-like graph to model decisions and their possible consequences. The algorithm builds the tree by recursively splitting the data into subsets based on the feature that provides the best split. Each internal node of the tree represents a decision based on a feature, and each leaf node represents a class label. Decision Tree models are easy to interpret and can be used for both classification and regression tasks.

Random Forest Classifier is a specific implementation of the Random Forest algorithm for classification tasks. It is an ensemble learning method that creates multiple decision trees, where each tree is trained on a randomly selected subset of the training data and a randomly selected subset of the features. The predictions from all the trees in the ensemble are then combined to make a final prediction for a new observation. Random Forest Classifier is robust to overfitting, can handle high-dimensional data, and can provide estimates of feature importance.

## Neural Networks for Credit Card Fraud Detection

As neural networks are a pillar in both the early and the recent advances of artificial intelligence, their use for credit card fraud detection is not surprising. The first examples of simple feed-forward neural networks applied to fraud detection can bring us back to the early 90s [AFR97, GR94]. Naturally, in recent FDS studies, neural networks are often found in experimental benchmarks, along with random forests, XGBoost, or logistic regression.

At the core of a feed-forward neural network is the artificial neuron, a simple machine learning model that consists of a linear combination of input variables followed by the application of an activation function 
 (sigmoid, ReLU, tanh, …).
 
![App Screenshot](https://i.postimg.cc/QNvnKCQS/ccfdann.png)

A whole network is composed of a succession of layers containing neurons that take, as inputs, the output values of the previous layer.

When applied to the fraud detection problem, the architecture is designed as follows:

        1. At the beginning of the network, the neurons take as input the characteristics of a credit card transaction, i.e. the features that were defined in the previous chapters.

        2. At the end, the network outputs a single neuron that aims at representing the probability for the input transaction to be a fraud.
        
We can stack multiple convolutional layers with batch normalization, ReLU, and dropout to extract more abstract and high-level features from the input sequence. Finally, we use a fully connected layer with a sigmoid activation function to output a probability score indicating the likelihood of the transaction being fraudulent or not.

The Conv1D layer is the core building block of the convolutional neural network. It is a 1-dimensional convolutional layer that performs a convolution operation on the input data to extract local patterns or features. In the case of credit card fraud detection, the input data is a sequence of transactions, and the Conv1D layer scans this sequence of transactions with a fixed-sized kernel to extract patterns of features that are relevant to fraud detection. The output of the Conv1D layer is a feature map that contains a set of learned filters.

The BatchNormalization layer is a normalization technique that improves the stability and convergence of the network by normalizing the activations of the previous layer. It works by normalizing the inputs to have zero mean and unit variance, which reduces the internal covariate shift and helps the network to converge faster. In the case of credit card fraud detection, the BatchNormalization layer is used after the Conv1D layer to normalize the activations of the filters before applying the activation function.

The Activation layer applies an activation function to the output of the previous layer to introduce non-linearity into the network. The ReLU activation function is the most commonly used activation function in deep learning, and it is used in the credit card fraud detection model as well. The ReLU activation function is defined as f(x) = max(0, x), which means that the output is zero for negative inputs and linear for positive inputs.

The Dropout layer is a regularization technique that helps to prevent overfitting by randomly dropping out some of the neurons in the network during training. The Dropout layer randomly sets a fraction of the input units to zero at each update during training time, which forces the network to learn more robust features and reduces the dependence on any single feature.

The Flatten layer is used to convert the output of the convolutional layers into a 1D vector, which can be fed to the fully connected layers for classification. It essentially flattens the multi-dimensional tensor output into a one-dimensional tensor by collapsing all the dimensions into one.

The Dense layer is a fully connected layer that connects every neuron in the current layer to every neuron in the previous layer. It is the final layer of the network that outputs the predicted class probabilities. In the credit card fraud detection model, the Dense layer has a single output neuron with a sigmoid activation function, which outputs a probability score between 0 and 1 that indicates the likelihood of the transaction being fraudulent or not. The sigmoid function is defined as f(x) = 1 / (1 + exp(-x)), which maps the output of the Dense layer to a probability score.

## Performance Measures

### Threshhold Metrics

In a typical credit card transaction dataset, the majority of the transactions are legitimate, while only a very small fraction of them are fraudulent. For example, in a dataset with a fraud rate of 1%, if the classifier always predicts "not fraud", it can still achieve a high accuracy of 99%, even though it completely fails to detect any fraud cases.

In other words, accuracy can be misleading in such cases and may not provide a complete picture of the model's performance. Instead, other evaluation metrics such as precision, recall, and F1-score are more appropriate for assessing the performance of CCFD models.

Precision measures the fraction of predicted fraud cases that are actually fraudulent, while recall measures the fraction of actual fraudulent cases that are correctly identified by the model. The F1-score is the harmonic mean of precision and recall, and it provides a balanced evaluation of both metrics.

Results for RFC:

![App Screenshot](https://i.postimg.cc/RVgHN3rD/accuracy-rfc.png)

Results for Decision Tree:

![App Screenshot](https://i.postimg.cc/bNynyx5n/accuracy-dt.png)

### Threshfree Metrics

However even after doing undersampling, we see that the performance metrics have improved. Now this might be correct but before drawing any conclusion about our model we need to understand the performance metrics in use. The recall, specificity, precision, and F1 score metrics, also known as threshold-based metrics, have well-known limitations due to their dependence on a decision threshold which is difficult to determine in practice, and strongly depends on the business-specific constraints. Hence to present an appropriate image of our model we need to use threshhold free metrics such as AUC ROC and AP, for our analysis.

The ROC AUC score tells us how efficient the model is. The higher the AUC, the better the model's performance at distinguishing between the positive and negative classes. An AUC score of 1 means the classifier can perfectly distinguish between all the Positive and the Negative class points.

Even though the accuracies for the two models are similar, the model with the higher AUC score will be more reliable because it takes into account the predicted probability. It is more likely to give you higher accuracy when predicting future data.

Results for RFC and Decision Tree:

![App Screenshot](https://i.postimg.cc/kMF8P1T9/auc.png)
