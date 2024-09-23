# Visual Melanoma Classifier

# Introduction
Melanomas are a common and potentially deadly form of cancer for which there are few effective national screening efforts, and little public awareness. The current guidelines for patients in the UK is to self-check regularly, and report to a GP when new moles or irregular patches of skin arise (1,2). 

But how do patients recognise melanomas? It can be difficult to recognise a melanoma, with the American Academy of Dermatology recommending the "ABCDE" system, with each letter describing an aspect of the appearance or behaviour of a melanoma that differentiates it from a normal mole. This system assumes two key things: 1) Patients will faithfully carry out the ABCDE system on each more, and 2) every melanoma fits the ABCDE system. Though it is doubtful that every person would put the time in to run an ABCDE check on each mole, it is absolute fact that not all melanomas fall under the ABCDE characteristics (3).

To help alleviate this problem without the need for a national melanoma screening system, an image analysis tool could be developed to help raise red flags to warn of a melanoma. By way of being a proof of concept, this was a weekend project which uses machine learning to analyse photographs of concerning moles, classifying them into normal moles (naevi, sing. naevus) or melanomas.

# Methods
## Data origin
Data were taken from the MED-NODE trial (4). These were chosen as the presented a roughly balanced (70 melanomas to 100 naevi) dataset of domestic camera images of melanomas and naevi. I considered it important to use domestic camera captured images as that is the end us case of these applications, people in a domestic setting taking their own pictures and analysing them.

The data are available at https://www.cs.rug.nl/~imaging/databases/melanoma_naevi/.

## Machine learning architecture
The machine learning architecture used was a simple convolutional neural network (CNN). This was chosen as CNNs have already proven themselves useful in medical image analysis, resulting in great progress in the field. Though complex CNN models have already been developed for medical image analysis(5), this project represents a basic proof of concept, so a more simplistic model, exploring the feasibility of the approach, was used.

This weekend project was also an opportunity to develop my skillset. I already have experience in Python-based machine learning, so all data management, model construction, and training was done in KNIME, an increasingly popular GUI-driven data engineering and analytics platform. This is demonstrated in the model architecture in figure 1 below. The model is made of 2 sections, the convolution layers, which extract image features, and the neural network which generates the outcome prediction. Though better network architectures could be produced, this model was being trained on a laptop without a GPU, so processing time was a consideration, thus the 64 unit network layers and larger stride in the convolution layers. The final network layer consists of a single neuron with a sigmoid output, producing a float in the range of 0-1 which can be interpreted as a probability, with p>=0.5 being interpreted as a melanoma.

<img width="737" alt="image" src="https://github.com/user-attachments/assets/342d8c35-9ad6-4bad-bffe-c440f68fa047">

*Figure 1. The construction of the CNN model in KNIME.*

## Data import and preprocessing
Data were loaded in using KNIME's image reader node, passing in a CSV containing the image file locations, pixel values normalised to a range of 0-1 for each of the three RGB colour values, resized of 150 x 150 to integrate them into the model. The diagnoses were then coded (0 = naevus, 1 = melanoma), extraneous columns filtered out, and the data partitioned in a 70%-30% train-test split.

<img width="898" alt="image" src="https://github.com/user-attachments/assets/cdc2b6ef-bdf3-46d2-8bf8-ae7121ec5895">

*Figure 2. Image preprocessing.*

## Model training
The constructed model and preprocessed data were then input into a keras network learner node, set to train for 100 epochs. Though 100 epochs seems like a high number for a classification exercise, it was found through gradual trial and error, with all values up to 30 leading to a heavily biased model which predicted all 1 or all 0, 60 and 120 leading to a test accuracy indicative of underfitting and overfitting respectively, and 100 seeing the highest performance in testing. The high number of epochs required and highly biased model seen in more typical 10-20 training epoch ranges may be a consequence of the very small dataset. The consequence of using such a small dataset (100 melanoma, 70 naevus images) may have been that the model did not have sufficient data in 10 epochs to make meaningful network weight changes, requiring more epochs to better learn from the small dataset.

The resulting model was then saved, and passed to a keras network executor. The results of the executor were then passed to a scoring module and a ROC curve plotter to give final performance metrics. This workflow was run multiple times to ensure the accuracy produced at each training epoch value was robust.

# Results
## Performance
The model displayed high performance despite the small sample set and relatively basic architecture, with an accuracy of 0.808 in its first run. The confusion matrix, displaying similarly high precision can be seen in figure 3 below. A ROC curve was also plotted, with a less impressive 0.793 AUC, see figure 4.

<img width="226" alt="image" src="https://github.com/user-attachments/assets/8fa25460-7803-43f7-94dd-8ebb69ac1afc">

*Figure 3. Confusion matrix for the first run of the model.*

![ROC Curve](https://github.com/user-attachments/assets/9f3f42fc-77f5-4156-a7c4-0fcce7959b53)

*Figure 4. ROC curve with AUC calculation.*

## Performance robustness on re-training
Surprisingly, given the small sample size, performance was robust to re-training with randomised 70-30 train-test splits at 100 epochs (accuracy on retraining: 0.808, 0.781, 0.793). This reflects the quality of the dataset and ability of CNNs to classify even subtle differences in images.

# Conclusion
The model training and testing workflow performed well, producing high quality models on a very small sample set. However, the samples used were exclusive from paler skin tones, thus performance issues are expected in darker skin tones.

To further improve model performance across all patients, I plan on collecting more publicly available naevus and melanoma pictures, including an equal balance of skin tones to ensure the model works well for all.

# Materials available in this repository
In this repo I have made available my KNIME training and testing architecture, as well as the final model trained during testing, with an accuracy of 0.793.

# References
1) https://www.cancerresearchuk.org/health-professional/cancer-statistics/statistics-by-cancer-type/melanoma-skin-cancer
2) https://www.cancerresearchuk.org/about-cancer/melanoma/getting-diagnosed
3) https://my.clevelandclinic.org/health/diseases/14391-melanoma
4) https://doi.org/10.1016/j.eswa.2015.04.034
5) https://doi.org/10.1016/j.neucom.2020.04.157
