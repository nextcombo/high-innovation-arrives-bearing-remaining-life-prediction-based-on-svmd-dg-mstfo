# High Innovation Arrives! Bearing Remaining Life Prediction Based on SVMD-DG-MSTFormer + SHAP Analysis

This project focuses on the prediction of remaining useful life (RUL) for bearings, based on the IEEE PHM 2012 bearing degradation dataset. A complete workflow has been developed, which includes reading raw vibration signals, feature extraction, SVMD denoising, deep model prediction, multi-model comparison, and SHAP interpretability analysis. Today marks the launch of this solution!
The current approach utilizes:
- SVMD denoising followed by DG-MSTFormer for bearing RUL prediction.
In this process, SVMD is used to enhance the stability of degradation trends, while DG-MSTFormer is employed to improve the generalization capability across bearings. The flowchart of the project code is as follows:
\n        \n  
I. Data Source and Task Objectives
The project utilizes the bearing degradation dataset from the IEEE PHM 2012 Prognostics Challenge, which records vibration signals during the degradation and failure process of bearings.
For this project, seven bearings under condition 1 are used as experimental data:
Bearing1_1 ~ Bearing1_7
The prediction goal is to output a normalized RUL value for each sliding window sample:
RUL Nearly 1: Bearing is in the early stages of its lifespan. RUL Nearly 0: Bearing is nearing failure.
\n        \n      \n        \n      Step 2: First Step: Extraction of Degradation Features from Vibration Signals 33 Based on statistical features, covering two perspectives of time domain and frequency domain.。  
The time domain features are used to describe information such as vibration amplitude, impact strength, peak changes, etc., including mean, variance, RMS, peak-to-peak, kurtosis, and skewness.
The frequency domain features are used to describe energy distribution, spectral structure, and frequency changes, for example, spectral centroid, spectral expansion, spectral entropy, dominant frequency, and the energy ratio of different frequency bands.
In this way, each bearing forms 33 characteristic degradation trend curves that vary over time.
Step two: denoising with SVMD to retain more stable degradation trend real vibration feature curves often exhibit significant fluctuations. Not all fluctuations represent true degradation; some are noise, instantaneous impacts, or sampling disturbances.
This project uses SVMD (Successive Variational Mode Decomposition) to denoise each characteristic trend curve.
The core idea of SVMD is to successively extract mode components from the signal, then retain the low-frequency or high-correlation modes that better reflect the degradation trend, while removing high-frequency noise and local spikes.
The processed curves become smoother, and the degradation trend is easier to be captured by the model.
Comparison between SVMD before and after denoising:
\n        \n      \n        \n  
Step 3: Building sliding window samples. After feature extraction and denoising, the project uses a sliding window approach to construct deep learning samples.
The current default window length is 10, meaning each sample contains 10 consecutive time steps, with 33 features per time step.
Input sample shape: [batch, 10, 33] Output label shape: [batch, 1]
Sliding windows enable the model to observe the local temporal changes process instead of relying on a single isolated sampling point for judgment.
V. Core Model: DG-MSTFormer Project's current main model is DG-MSTFormer, abbreviated as Domain-Generalized Multi-Scale Temporal Transformer.
The design focuses on enhancing the generalization capability across bearings in the leave-one-out experiments.
The schematic diagram of DG-MSTFormer is as follows, mainly consisting of four parts:
① The degradation speed of different bearings varies across multiple scales. Some bearings degrade smoothly, while others experience rapid deterioration towards the end. Therefore, the model employs multiple time-series convolution kernels of varying scales to simultaneously capture short-term abrupt changes, medium-term fluctuations, and relatively smooth degradation trends.
② The Transformer encoder extracts local trends through multi-scale convolution and further models the global temporal dependencies and feature interrelation within the window.
③ The multi-pooling fusion model not only uses the last time step but also integrates the results of mean pooling, max pooling, and attention pooling, allowing the model to simultaneously focus on overall trends, peak responses, and key time segments.
④ Domain generalization constraints are applied during training, treating different training domains as distinct. The model aligns the statistical distributions in the latent space for different domains via CORAL constraints, thereby reducing dependence on unique patterns specific to a single domain. Additionally, the model incorporates RUL monotonic ordering constraints, ensuring that the prediction results more closely align with the physical laws of progressive life decline.
VI. Multi-model comparison experiments To verify the effectiveness of DG-MSTFormer, the project retains several common deep learning models for comparison:
\n        \n  
Experimental method: Leave-one-out cross-validation. Each time, one bearing is reserved as the test set, while the remaining bearings are used for training. This setup better reflects the model's generalization ability on unseen bearings.
Full-set leave-one-out average metric table:
\n        \n      VII. Predictive Curve Display Project will output a comparative curve between the actual RUL and the predicted RUL for each test bearing.
\n        \n      \n        \n      \n        \n      \n        \n      \n        \n      Single bearing prediction curve comparison chart: Overlapping comparison chart of single bearing multi-model prediction curves:   
\n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      \n        \n      8, SHAP Explainability analysis to further understand why the model made a certain prediction, the project added SHAP Interpretable analysis。  
SHAP can answer the following questions from the perspective of feature contribution:  
Which time-domain features have the greatest impact on RUL prediction?  
Which frequency-domain features better reflect the degradation process?  
Does the model overly rely on certain unstable features?  
Are the important features consistent across different bearings?
\n        \n      \n        \n  
\n        \n  
IX. Project Summary and Code Acquisition
The project completed a complete bearing RUL (Reliability Uptime) prediction process:
- Read the bearing full life data from the original vibration signal of PHM2012.
- Extract 33-dimensional time-domain and frequency-domain features.
- Use SVMD to denoise the feature degradation trend.
- Build sliding window sequence samples.
- Use DG-MSTFormer for life prediction.
- Validate generalization ability through multi-model leave-one-out experiments.
- Analyze model predictions using SHAP.
- Comparative analysis with current mainstream models.
Code directory:
\n        \n  
All the results shown above can be achieved with this code!

## Images

![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](01.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](02.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](03.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](04.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](05.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](06.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](07.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](08.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](09.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](10.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](11.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](12.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](13.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](14.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](15.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](16.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](17.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](18.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](19.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](20.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](21.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](22.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](23.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](24.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](25.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](26.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](27.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](28.png)
![High innovation! Predicting the remaining life of bearings based on SVMD-DG-MSTFormer + SHAP analysis](29.png)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
