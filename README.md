# Neurotech Analyze EMG Data

The goal of this analysis is to accurately classify EMG signals from three hand positions: "rock", "paper" and "scissors", from the game of the same name.

## Data Collection

The data was collected via 3 channels of arm surface EMG from 10 individuals. For each individual, 18 trials were conducted, 6 trials in each of the 3 positions.

Table 1: Experimental Setup
|Property| Value|
| --- | --- |
Sampling frequency | 500Hz
No. of channels | 3
Sampling | 1500 time points over 3000ms

## Data Processing

The EMG signal is passed though a second order Butterworth bandpass filter with cutoff frequencies of 20Hz and 100Hz, as is common in the EMG filtering literature [(Tomaszewski, 2016)](https://www.sciencedirect.com/science/article/pii/S1474667015304596).

## Feature Extraction

Autoregressive (AR) models are of commonly utilized feature types in Electroencephalogram (EEG) studies due to offering better resolution, smoother spectra and being applicable to short segments of data [(Atyabi, 2016)](https://gpandrade.github.io/seminaryear2_slides.pdf).

Each processed EMG signal is modeled with linear autoregressive (AR) model whose computed coefficients are used as signal features. The [SPECTRUM: Spectral Analysis in Python](https://pypi.org/project/spectrum/) package was used to calculate the pth order coefficients of each EMG processed signal. Separate feature sets are made from the AR coefficients of 2, 3, 4, 5, 6, 10, 16, 30 pth-order models. 

Additional features are computed by calculating the Root Mean Square of each channel [(Amorin, 2018)](https://gpandrade.github.io/seminaryear2_slides.pdf).

## Classifier

A keras multilayer perceptron was used as a classifier [(Tomaszewski, 2016)](https://www.sciencedirect.com/science/article/pii/S1474667015304596). Better performance occurs when a greater order of AR coefficients is used. The best performing model used 30 AR coefficents + RMS features, but it was not able to achieve an accuracy above 60%. Increasing AR coefficients above 30 did not improve the model. This model performed slightly better than random (0.33) but overall performed poorly.


|Info |Train |Test |Validation
| --- | --- | --- | --- |
| No. samples | 126 | 27 | 27
| accuracy | 0.5635 | 0.4815 | 0.407
| loss | 0.82 | 0.982 | 1.140

## Conclusion

Better feature selection may be helpful. The Burg coefficient calculation method might perform better on a smaller window of time. [Tomaszewski, 2016](https://www.sciencedirect.com/science/article/pii/S1474667015304596) mentioned that they divided their time series into "measurements" and then had the most sucess when fitting the linear AR coefficients to the first 5 and last 5 measurements. Also, many studies use more EMG channels (6-8), so increasing the number of EMG channels might improve results. Also, doing some location / muscle specific filtering might improve the results.
