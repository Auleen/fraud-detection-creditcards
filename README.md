
# Credit Card Fraud Detection Model.
ML-based fraud detection model that is effective at identifying evolving fraud patterns even in the presence of imbalanced data.

Worldwide financial losses caused by credit card fraudulent activities are worth tens of billions of dollars. One American over ten has been a victim of credit card fraud (median amount of $399), according to the Statistic Brain Research Institute. According to the latest European Central Bank (ECB) report, the total level of card fraud losses amounted to €1.8 billion in 2018 in the Single European Payment Area (SEPA).


## Machine Learning in Credit Card Fraud Detection

Credit card fraud detection is like looking for needles in a haystack. It requires finding, out of millions of daily transactions, which ones are fraudulent. Due to the ever-increasing amount of data, it is now almost impossible for a human specialist to detect meaningful patterns from transaction data. For this reason, the use of machine learning techniques is now widespread in the field of fraud detection, where information extraction from large datasets is required

A wide number of ML techniques can be used to address the problem of CCFD. This is directly reflected by the huge amount of published papers on the topic in the last decade. Despite this large volume of research work, most of the proposed approaches follow a common baseline ML methodology, which involves a Supervised Learning methodology (Decision Trees,Logistic Regression,Isolation Forest and etc)

The schema can be summarised by the following diagram:

![App Screenshot](https://i.postimg.cc/rFtQT4NC/Screenshot-2022-06-24-215543.png)
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



