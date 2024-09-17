# Visual Melanoma Classifier

# Introduction
Melanomas are a common and potentially deadly form of cancer for which there are few effective national screening efforts, and little public awareness. The current guidelines for patients in the UK is to self-check regularly, and report to a GP when new moles or irregular patches of skin arise (1,2). 

But how do patients recognise melanomas? It can be difficult to recognise a melanoma, with the American Academy of Dermatology recommending the "ABCDE" system, with each letter describing an aspect fo the appearance or behaviour of a melanoma that differentiates it from a normal mole. This system assumes two key things: 1) Patients will faithfully carry out the ABCDE system on each more, and 2) every melanoma fits the ABCDE system. Though it is doubtful that every person would put the time in to run an ABCDE check on each mole, it is absolute fact that not all melanomas fall under the ABCDE characteristics (3).

To help alieviate this problem without the need for a national melanoma screening system, an image analysis tool could be developed to help raise red flags to warn of a melanoma. By way of being a proof of concept, this was a weekend project which uses machine learning to analyse photographs of concerning moles, classifying them into normal moles (naevi, sing. naevus) or melanomas.

# Methods
Data were taken from the MED-NODE trial (4). These were chosen as the presented a roughly balanced (70 melanomas to 100 naevi) dataset of domestic camera images of melanomas and naevi. I considered it important to use domestic camera captured images as that is the end us case of these applications, people in a domestic setting taking their own pictures and analysing them.




# References
1) https://www.cancerresearchuk.org/health-professional/cancer-statistics/statistics-by-cancer-type/melanoma-skin-cancer
2) https://www.cancerresearchuk.org/about-cancer/melanoma/getting-diagnosed
3) https://my.clevelandclinic.org/health/diseases/14391-melanoma
4) I. Giotis, N. Molders, S. Land, M. Biehl, M.F. Jonkman and N. Petkov: "MED-NODE: A computer-assisted melanoma diagnosis system using non-dermoscopic images", Expert Systems with Applications, 42 (2015), 6578-6585
