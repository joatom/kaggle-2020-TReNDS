# kaggle-2020-TReNDS
  
This repository contains some of my codings and notes of the 2020 kaggle *TReNDS Neuroimaging* competition (https://www.kaggle.com/c/trends-assessment-prediction) which ran from Apr/20 to Jun/20.

I teamed up for this competition with [Rohit Singh](https://www.kaggle.com/rohitsingh9990), [Manoj Singh](https://www.kaggle.com/mks2192) and [Priyanshu Kumar](https://www.kaggle.com/kpriyanshu256). It was a great experience to work with those smart guys and to experiment with a huge varity of techniques that we tried out during the competition. Cheers to my team :champagne:.

Our best submission scored **[36](https://www.kaggle.com/c/trends-assessment-prediction/leaderboard) out of 1047 teams** which results in a silver medal :2nd_place_medal:.

The data of the competition consists of 3D Magnetic resonance imaging (MRI) pictures of the brain which where recorded by zwo different scanners. Additionaly the host provided about 1500 features of brain activities related to the images. The task was to predict five differnt features (e.g. age of the patient).


## Solution Design
A detailed explanation of our solution design can be found [here](https://www.kaggle.com/c/trends-assessment-prediction/discussion/162738), including links to some great notebooks and post processing techniques we used.

In this repo I will only focus on my contribution to the solution which I also described [here](https://www.kaggle.com/c/trends-assessment-prediction/discussion/162738).
>"
>Here are some details of parts of our 36th place solution:
> 
>### A simple tab nn
>Part of the solution is a [simple tabular NN](https://www.kaggle.com/joatom/trends-tabular-nn-0-159) scoring 0.159 on public LB and would already [score silver](https://www.kaggle.com/joatom/trends-ensemble), when blended with the public [Rapids SVR](https://www.kaggle.com/aerdem4/rapids-svm-on-trends-neuroimaging). Early in the competition we published a [tab NN](https://www.kaggle.com/joatom/trends-exploring-tabular-augmentation) with many hidden layers and fancy augmentation, which build the baseline for NN.
>
>It turned out when removing the hidden layers to a minimum the NN improves a lot. For regularization we replaced the augmentation by stratified 10-fold (using 7 age bins for strat classes) cross validation and high drop out.
>
>The second major improvement was tweaking the loss function. The loss is calculated on all 5 targets together, but ignores missing values (as in domain1_var1). So the tab nn head has 5 outputs. The loss function emphasizes lower values to keep in balance with the skewed (unsymetric) metric.
>
>Another booth came from post processing (adding 0.3 to all targets) finally the nn was trained on a mse-based and a second on a mse+l1-based loss (with the above mentioned extentions). The mean of both was the output of the NN notebook ready to ensemble.
>
>As input we used the provided tabular data and as the model improved we added [clusters](https://www.kaggle.com/joatom/trends-cluster-sfnc-groups) from fnc.csv.
>
>### What didn’t work
>Besides the mentioned models we did lot’s of experimentation with other approaches. Here is a bestof about what we didn’t get to fly:
>
>Autoencoder: 53ximage in, loss on generated image and targets as output of bottleneck layer
>Converting 3d in 2d (static and learned conversion) and put a resnet on each of the 53 entities and merge them in one output
>Treating fnc.csv corralations as probabilities and use Markov Modell hidden states as features.
>LSTM-layer in Tab-NN
>using test-like and site2-like probabilities from av as features
"


## Overview of notebooks

### /training
- **trends-cluster-sfnc-groups**: generating new features through clustering
- **trends-tabular-nn-0-159**: simple tabular neural net

### /inference
- **trends-ensemble**: ensembling notebook combining results of a simple tab NN and a SVR

### /notes
Additional personal notes and experiments. (The notbooks are unpolished and uncommented.)
- **trends-exploring-tabular-augmentation**: tabular neural net with augmented tabular data
- **trends-fnc-steady-states**: using markov models to calculate steady state features
- **trends-meta-ensemble-cv**: another ensembling kernel
- **trends-av-probs-of-test-and-s2**: adverserial validation on train
- **trends-autoencoder-fastai**: experimenting with an auto encoder
- **trends-3d-on-2d-pretrained**: 3d to 2d conversion as part of a neural net
- **convert to png**: 3d MRI to 2d conversion with colored regions using nilearn library
