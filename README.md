# Overview
Weibo is a Chinese microblogging website. It is one of the most popular sites in China, with a market penetration similar to the Twitter. “Weibo” is a Chinese word for “microblog”. User behaviors such as forwarding, commenting and liking are important factors that can be used to estimate the quality of a certain weibo and implement the recommendation and feed controlling strategy. In this competition, participants are required to predict the forwarding, commenting and liking amount of a weibo based on the historical interaction data.

The difference of each behavior to a certain weibo is calculated by:

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_f&space;=&space;\frac{count_{fp}&space;-&space;count_{fr}}{count_{fr}&space;&plus;&space;5}" title="deviation_f = \frac{count_{fp} - count_{fr}}{count_{fr} + 5}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_c&space;=&space;\frac{count_{cp}&space;-&space;count_{cr}}{count_{cr}&space;&plus;&space;3}" title="deviation_c = \frac{count_{cp} - count_{cr}}{count_{cr} + 3}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?deviation_l&space;=&space;\frac{count_{lp}&space;-&space;count_{lr}}{count_{lr}&space;&plus;&space;3}" title="deviation_l = \frac{count_{lp} - count_{lr}}{count_{lr} + 3}" /></a>

where count<sub>*p</sub> is the prediction value and count<sub>*r</sub> is the target value with * being f(forward), c(comment) or l(like) respectively.

The preciison for each weibo is:

<a href="https://www.codecogs.com/eqnedit.php?latex=precision_i&space;=&space;1&space;-&space;0.5&space;\times&space;deviation_f&space;-&space;0.25&space;\times&space;deviation_c&space;-&space;0.25&space;\times&space;deviation_l" target="_blank"><img src="https://latex.codecogs.com/gif.latex?precision_i&space;=&space;1&space;-&space;0.5&space;\times&space;deviation_f&space;-&space;0.25&space;\times&space;deviation_c&space;-&space;0.25&space;\times&space;deviation_l" title="precision_i = 1 - 0.5 \times deviation_f - 0.25 \times deviation_c - 0.25 \times deviation_l" /></a>

The global precision:

<a href="https://www.codecogs.com/eqnedit.php?latex=precision&space;=&space;\frac{\sum_{1}^{n}(count_i&space;&plus;&space;1)\times&space;sgn(precision_i&space;-&space;0.8)}{\sum_{1}^{n}(count_i&space;&plus;&space;1)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?precision&space;=&space;\frac{\sum_{1}^{n}(count_i&space;&plus;&space;1)\times&space;sgn(precision_i&space;-&space;0.8)}{\sum_{1}^{n}(count_i&space;&plus;&space;1)}" title="precision = \frac{\sum_{1}^{n}(count_i + 1)\times sgn(precision_i - 0.8)}{\sum_{1}^{n}(count_i + 1)}" /></a>

where sgn(x) is a modified signal function defined as sgn(x)=1 if x > 0 and sgn(x) = 0 if x<=0.
count<sub>i</sub> is the sum amount of forwarding, commenting and liking of ith weibo. Count<sub>i</sub> take the value of 100 if count<sub>i</sub> is larger than 100.

# Feature engineering
According to the exploratory data analysis (EDA), over 98% of users in text data have records in training data. And it is obvious the forwards, comments and likes received for each weibo are highly dependent on the user sending them. For example, if the user is a celebrity, he or she will have a larger follower base thus it would be easier for him or her to have a larger number of forward, comments and likes. Therefore, behavior related features for each user can be very useful for prediction. These features include 5 fold target encoded features where 5 fold setting in target encoded features is used to avoid overfitting and lag features with different time windows.

Another obervation after checking the data is that there are a lot of duplicated weibos. There are two main reasons for duplication: First, "garbage" weibos like ads, notifications and congragualation messages. These weibos are highly similar to each other and do not get any forwards, comments or likes. Second, corrected weibos. People may have typos in the firs weibo, so after they sent the first one and found the typo, they would delete the first one and resend the correct version. Since the first few ones with typos are very short lived (usually less than 30 minutes or even a couple of seconds), they won't get any forwards, comments and likes. And all forwards, comments and likes will show in the final pefect version of weibo. Based on this observation, I implemented a simple algorithm rating how similar duplicated weibos are. Then I engineered features such as the total number of duplicated weibos for each user and in all training data. Besides, based on the time these weibos sent, I generated features such as the index in time order and whether one weibo is the last weibo.

Feature extraction for weibo contents are performed by Bert model.

# Modelling
I chose Bert model with no particular reasons. Mainly because I am more familiar with it and it already has a pretrained Bert model for Chinese ready to use. In fact, most language models nowadays have a multilingual version. But from the documentation I learned Chinese one is better for Chinese text while multiligual one needs some pretraining. Therefore I decided to start with Chinese Bert. Surprisingly implementing a Chinese Bert model is as straightforward as an English Bert model. Before I go fancy modelling archtectures, we have to make sure base models are good enough. The performance of base models highly depends on feature engineering. Therefore besides features I mentioned in feature engineering section, I spent most of my time trying different techniques to train a better Bert language model for this task:
* Default Bert model + heads for forward, comment and like + weighted mae loss functions
* Pretrained Bert model + heads for forward, comment and like + weighted mae loss functions
* Default Bert model + user behavior features & weibo duplicity features + heads for forward, comment and like + weighted mae loss functions
* Pretrained Bert model + user behavior features & weibo duplicity features + heads for forward, comment and like + weighted mae loss functions
I trained 4 Bert LMs with 4 different architectures mentioned above.

I kept all trained Bert LMs since even though some of them might not be good themselves, they could be beneficial for ensembling. According to my experiments, Bert LM w/ regression head architecture is not as good as using Bert LM to extract embeddings for texts and use them for downstream modelling. Therefore my two types of base models are:
* Lgbm mdoels with features including predicitons from Bert LM w/ regression head + user behavior features & weibo duplicity features
* Fully connected NN models with features including Bert embedding vectors + user behavior features & weibo duplicity features
I have 8 lgbm models and 4 NN models. 2 lgbm models and 1 NN model for each Bert LMs. Each lgbm model is trained with a different random seed and different weights to samples (one equally important and the other one has a less weight for outliers). All NN models are trained with equal weights.

Using oof results from 12 base models as features in addition to user behavior & weibo duplicity features, my second level models include a lgbm model and a NN model. And finally a simple blending of the second level lgbm and NN models with equal weights produce the final result. 

# Summary
What works:
* Feature engineering based on user behavior
* Pretrain LM with training and testing texts
* Different architectures to train LM
* Model ensembling (stacking and blending)

What doesn't work:
* Weighted samples

What more I could do:
* Data cleaning. To be more specific, clean up url links in the text. There are three reasons. First, these url links usually do not carry much information so it is better to get rid of them. Second, they are very long, and in some sense they might affect the algorithm to calculate the similarity between two weibos. Third, when I generated corpus for LM pretraining, I got rid of these url links. But when I train Bert models with pretrained LM, I kept them. This conflict should be addressed.
* Train LM with only latest weibo which exists for a long time and reflect its real value.
* Better hyperparameter settings to train LMs.
* More types of models (LMs and other task specific models)
