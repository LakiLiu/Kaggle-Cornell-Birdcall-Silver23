# Kaggle-Cornell-Birdcall-Silver-TOP2%

## About
This repo is about [the Kaggle](https://www.kaggle.com/c/birdsong-recognition) competition supported by Cornell University . The objective is to predict the kind of bird by birdcall to help conserve wildlife. We Rank **23** among 1390 teams, rewarded with a silver medal (**TOP 2%**).  It was an awesome experience with my teammates Changyi SONG, Ding LI, Clement LIAO. Special thanks to Jie LU. I will briefly introduce the methodologies we tired those work well.


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


## Feature Engineering 
### (A) Audio Feature Engineering
This step is to do the audio transformation with ``librosa`` and ``Albumentations``

📌 Audio Transformation
- Fourier Transform : Function that transform  gets a signal in the time domain as input, and outputs its decomposition into frequencies. 
- Spectrogram :is a visual representation of the spectrum of frequencies of sound or other signals as they vary with time.
- Mel Spectrogram : The Mel Spectrogram is a normal Spectrogram, but with a Mel Scale on the y axis.

📌 Audio Augmentation
- RandomAudio
- AddGaussianNoise
- Polarity Inversion

### (B) Image Feature Engineering 
This step transform the audio (after feature engineering) into image, than doing the image feature enginnering with ``Pytorch``.

📌 Image Augmentation
- Rotate(10, p=0.33),
- RandomBrightness(0.15, p=0.5),
- Cutout(p=0.33),
- VerticalFlip(always_apply=False, p=0.5),
- RandomCrop(220, 224, p=0.5),
- RandomResizedCrop(224, 224, scale=(0.8, 1.0), ratio=(1.7, 2.3), p=0.33) 
**Resize has a very significant effect to improve performance**

## Model
We tried Sound Event Detection Model (SED) but we found the ResNeSt Model has a better performance

**We use 15s data to train, therefore the test prediction is for each 15s.**
- Pretrained Model : resnest50_fast_1s1x64d
- Attention block
- Loss function : BCEWithLogitsLoss
- optimiser : Adam
- scheduler : CosineAnnealingLR

## Post Processing 
Since the model give a label for each 15s, we need to do the postprocessing 

📌 **Moving Window** 
- we treat 15s-prediction as a window, then moving it to predict each 5s. In total each 5s would be predicted by 11 times, then we take the average for probability. The label is then for then decided.

## Reflection 
- We finally submit  **2 single ResNeSt models**. We should have blended more other models to drive more robust performance.
- **Moving window** is very effective method, but it is time-consuming. Considering the max prediction time of 2 hours, it limits our model development at the same time.
- In conclusion, this competition gives me more ideas about audio and image processing 🐦
