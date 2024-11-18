Dear all,

Unfortunately, I have to state that even though I managed to achieve certain result on kaggle, I do not feel that I managed to produce any specific insight on how to approach the data. 
I started by separating the the letters into into 23 columns to use them as categories in decision trees models, and then used one-hot encoding for each of the columns for each of the 4 letters. Linear models performed pretty badly, quickly overtting yet producing extremely bad results (using train/test split), so I quickly switched to using boosting via xgboost library. It immediately produced results in the vicinity of the final results, and using randomized cross-validated hyperparameters search refined them slightly. I had 2 ideas one of which did not improve result at all, another one improved it only slightly.

1. First idea was to try to make the model consider neighboring positions. To achieve that I created more columns, containing neigboring pairs or nucleotides, triplets, etc. Due to cpu constraints I only tested it until the length of 5, however this approach did not seem to improve neither linear models (with or without ridge regression) nor decision trees. 

2. Second idea was to exploit the fact that the data samples in the dataset are themselves aggregates of physical samples, with "Indel_Diversity" feature being roughly proportional to standard deviation of the samples involved. Because of the we possibly could make them to hint the boosting model to use have lower weights on samples with higher diversity since they are less statistically relevant for the outcomes. This seems to have very slightly improved the final result.

One the positive side, the booster model never seemed to overfit. The best results in the end were obtained by simply having very high number of iterations in the hyperparameters search with k-fold splitting. Worth noting that I initially used 80/20% train/test split for quick assessment of the model, but it turned out that including all the date in the training also slightly improved the final model. (Combined with the observation that model always seemed far from overfitting I used the full dataset to train it)

Regarding the code, the complete version together with history of attempts is in the repository here: https://github.com/0xorial/mlfls-hw.
I run it locally, and I use pixi as package manager and input data is included in the repository, so it should be possible to rerun it precisely. 2 main files of interst are: 
- playground.ipynb - my initial attempts and data exploration, with linear models
- boosting.ipyng - the final code with boosting.
