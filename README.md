# kaggle-islr-challenge
Attempt to solve kaggle's isolated language recognition challenge (https://www.kaggle.com/competitions/asl-signs/overview).<br/>
My Best score on private kaggle dataset came out to 0.741 while the top 1 had 0.892(respect).

## Prominents notebooks/approaches that were good enough on training,test dataset to be tested kaggle's hidden test dataset
1. 030323_23_49 : 2 hands , x,y,z, remove na landmarks ,resize to 21 frames, conv1dlstm stacked 0.4187973
2. 030523_00_09: same as above added 1 more block of conv1dlstm 0.5001428
3. 030523_19_09 : same as above used x,y only 0.49
4. 031123_18_51: normalization to 0-1 for each hand 0.53
5. 032623_15_16 : added lip landmarks as well 0.54
6. 040123_20_21: padded to max length with masking for conv1dlstm model 0.5487787
7. 032823_22_43: choose dominant hand 0.5138551
8. 041523_13_41 : transformer model with resize frames dataset for lip,left and right hand 0.64
9. 041723_22_03_2 : Transformer model architecture with resized frames dataset, choosing dominant hand 0.62
10. 041923_16_27 : Same as above , pose is also added to input array i.e fed to model
11. 042223_20_52 : Same as above, changed transformer model architecture.
12. viz : notebook for eda and landmarks visualization

## Overview

- I tried to approach the problem using different angles not just from model view but NNs from different domain entirely
- Tried breadth approach instead of depth approach i.e looked for unique + different approaches rather than pushing 1 approach to the end lilke hyperparameter tuning etc.
- In general, Tried Conv1dlstm, Conv2D, Tranformers, Graph convolution NN were the crux of majority of my experiments.
- Intially approached the problem as Video Spatial data classification instead of being 2d we have 1d here.
- Data looks like this (Frames,landmarks,coordinates) 
	- where Frames are variable and different for each sample
	- This dimension is devoted to landmarks which are hands,face,pose. I mainly tried hands,lips from face,and pose
	- last dimension belongs to coordinates which are x,y,z. Started with all 3 , dropped z later on

## Challenges
- Since this is a video dataset problem, dealing with variable frames was an issue. To make it more exciting, the fps was different. Making some samples extremely lengthy or small. I tried with fixed frame approach using resizing, truncation/padding, choosing max length in dataset and padded at first. Best worked for me was resize approach.
- Overfitting, the always culprit as usual. After reaching 60-70 % test accuracy, the model starts to overfit and for architectures like transformers ,its very difficult to get rid of.
- Augmentation, one way to tackle overfitting is ofcourse, but I couldn't think some intelligent ways to augment that didn't affect model training in a negative way. Tried hands swap augmentation.
- Normalization, normalizing data before feeding to model is a general step. Mediapipe model gives coordinates such that the body is 0 centered , there is no hard range(-n to +n). Tried 0-1 normalization, also normalization using mean and unit variance learned using training dataset.
- Tflite : My STGCN implementation performed the best as compared to all other approaches but I wasn't to submit to kaggle because it kept failing during submission. Not sure because of tflite conversion. STGCN approach has 10% better results as compared to second best approach.
:(
 
## References
- Transformer starter notebook : https://www.kaggle.com/code/markwijkhuizen/gislr-tf-data-processing-transformer-training
- Graph CNN starter notebook : https://www.kaggle.com/code/timxinxinpeng/isolated-sign-language-recognition-with-stgcn
