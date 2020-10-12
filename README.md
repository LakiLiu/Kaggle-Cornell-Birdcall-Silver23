# Kaggle-Cornell-Birdcall-Silver-TOP2%

## About
This repo is about [the Kaggle](https://www.kaggle.com/c/birdsong-recognition) competition supported by Cornell University . The objective is to predict the kind of bird by birdcall to help conserve wildlife. We Rank **23** among 1390 teams, rewarded with a silver medal (**TOP 2%**).  It was an awesome experience with my teammates Jie LU, Changyi SONG, Ding LI, Clement LIAO. I will briefly introduce the methodologies we tired and summaried those work well.


## Introduction
### (A) Data 
- ``train.csv`` contains basic information about the audio files available in train_audio. 
- ``train_audio`` contains multiple clipwise audios within each bird race (various in number of clips and length).
- ``test_audio`` The hidden test audio contains approximately 150 recordings in mp3 format, each roughly 10 minutes long.

### (B) Objective 
- Prediction 1 : Sites 1 and 2 are labeled in **5 second** increments (no birdcall -> nocall)
- Prediction 2 : Site 3 was labeled per the whole audio file.
- Evaluation : row-wise micro averaged F1 score.
- the predcition running time should be **no longer than 2 hours**


 
