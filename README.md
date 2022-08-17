## Competition Review of UW-Madison GI Tract Image Segmentation

![](/images/picture1.jpg "")

### About the Competition
In [UW-Madison GI Tract Image Segmentation competition](https://www.kaggle.com/competitions/uw-madison-gi-tract-image-segmentation/overview), contestants should create a model to automatically segment the stomach and intestines on MRI scans. The competition is evaluated on the mean Dice coefficient and 3D Hausdorff distance.

### Our Result
We won the bronze medal in the competition with the [Private LB score 0.86783, ranking 142/1548 (Top 9.17%).](https://www.kaggle.com/competitions/uw-madison-gi-tract-image-segmentation/leaderboard)

## Solution Pipelines
![](/images/picture2.png "")
To achieve better detection effect and make the model converge effectively, we used the following training and validating processes: First, divides the source files into five folders. Second, trained 5 3D U-NET with patch size is (192, 192, 80) models (based on [MONAI pipeline](https://www.kaggle.com/datasets/yiheng/uw3dmonaitrainingpipeline)) with each sub datasets. Finally, during validating process, input the data into five models respectively, and then conduct 5-folds ensemble predictions.

## Loss Function
We tested several loss functions to choose the best one. In this section, the hyperparameters were as follow: Epoch: 1000 / Learning Rate: 5e-4~2e-4 and only fold 0 was applied. As the table shown, Dice + CE was chosen as the best loss function in our solution.
|     Loss Function        | Dice + Hausdorff |
|-------------|-------|
|Dice| 0.865|
|CE|0.866|
|Dice + CE|0.875|
|Focal CE|0.867|
|Dice + Focal CE|0.874|

## Hyperparameters
We tested several sets of hyperparameters. In this section, the loss function was Dice + CE and only fold 0 was applied. As the table shown, results of Setting 3 and 4 were better than the others.
|Index|	Hyperparameters|Dice + Hausdorff|Public LB Score|
|-------------|-------|-------------|-------|
|1|	epo:1000 / lr:5e-4~2e-4 / TTA = False|	0.875|	0.858|
|2|	Model 1 + epo:20 / lr:1e-4 / TTA = True|	0.882|	0.863|
|3|	Model 2 + epo:50 / lr:1e-4~1e-5 / TTA = True|	0.887|	0.869|
|4|	Model 3 + epo:100 / lr:1e-5~1e-6 / TTA = True|	0.890|	0.867|


## Ensemble Predictions
Setting 3 and 4 were chosen and trained with 5 folds of training data respectively. The Public Leaderboard Scores were shown in the table. The best results of each fold were shown in bold, which were chosen as the models to do 5-folds ensemble predicting. Finally, the ensemble prediction got 0.873 in Public Leaderboard Score and was chosen as our final submission in the competition.
|Index|	Fold 0|	Fold 1|	Fold 2|	Fold 3|	Fold 4|
|---|----|----|----|----|----|
|3|	<strong>0.869</strong>|	0.868|	0.864|	<strong>0.867</strong>|	<strong>0.870</strong>|
|4|0.867|	<strong>0.869</strong>|	<strong>0.866</strong>|	0.866|	0.869|

## Acknowledgement
Many thanks to my teammates: Peter, who helped me to test the performances of different loss functions. Eric, who helped me to train the models applied in 5 folds ensemble prediction. Without anyone in our team, we couldn’t get the bronze medal in our first competition in Kaggle.
