# Ethnicity Classification From Face Images, Literature Review

2012 or 2022?
Lyle et al.

## Approaches

### Mathematical and statistical

Lu et al. (2004) [4] classified Asian and Non-asian using Linear Discriminative Analysis (LDA) to gray-scale facial images in different scales. At each scale, a classic appearance-based face recognizer based on the LDA representation is developed under the Bayesian statistical decision framework. LDA is a well-known statistical method to project a given multidimensional data to a lower dimension, in a way that efficiently distinct two or more classes [5].

Manesh et al. (2010) [6] adpoted a method that uses the Golden ratio mask by employing decision-making rules on different face zones, and then using SVM to classify the extracted Gober features into Asian and non-asian.

Guo et al. (2014) [7] used Canonical Correlation Analysis (CCA), and partial least squares (PLS) to broadcast ethnicity, gender and age classification. They found that these methods can do dimensionality reduction and jointly estimate age, gender and ethnicity within a single step.

### Skin and hair color

Wu et al. (2004) [8] proposed a Look-Up-Table depending on boosting method to extract demographic features such as race and gender from human faces. The proposed method composes three modules, including facial region detection, facial feature extraction, and demographic classification.

Demirkus, Garg and Guler (2010) [9] proposed two algorithms, which are pixel intensity values and the Biologically Inspired Model (BIM) to classify race based on hair and face color. Also using an SVM classifier into three ethnic groups: Asian, African and Caucasian.

Muhammad et al. (2012) [10] proposed a race classification method based on two local descriptors: Weber Local Descriptor (WLD) and Local Binary Pattern (LBP). It also used different distance classifiers, such as Chi-square, Euclidean, and City-block. Fusing WLD with LBP and using City-block as a distance classifier performed better than the rest.

Becerra-Riera et al. (2018) [11] proposed a method to codify information about different face regions, with appearance and geometric features such as color and shape. They classified face images from the FERET dataset into three groups: white, black and asian.

### Focusing on particular region in the face

Lyle et al. (2012) [12] proposed a method that selected texture features from the periocular region by applying LBP feature-based technique to distinguish between asians and non-asians.

Sun et al. [13] proposed a classification technique dependent on a hierarchical visual codebook to classify iris images.

Xie et al. (2012) [14] focused on the upper part of the face and incorporated facial color-dependent features and Kernel class-based features to identify race.

Hosoi et al. [15] proposed a new method that cooperates retina sampling and the Gabor wavelet features to be classified using SVMs.

Roomi et al. [16] suggested a face detection method that used the Viola-Jones algorithm. The authors focused on extracting features from different regions, including lip color, skin color, and forehead area.

Marzoog et al. [17] proposed a model based on the geodesic distance technique as a feature extractor. Principal Component Analysis (PCA) has been used as dimension reduction. SVM and KNN have been used as classifiers. This method achieved 100% accuracy in FERET dataset.

### Deep learning

Zhang et al. (2016) [18], Wang et al. (2016) [19], Wei et al. (2015) [20], Khan et al. (2013) [21] all used CNNs to classify. Vo et al. (2018) [22] suggested two race recognition frameworks, with CNN and VGG.

### Multitask learning

From Facial Attribute Recognition: A Survey Thom and Hand 2020

Rud et al. [3] developed MOON: Mixed Objective Optimization Network, capable of learning all attributes at once while at the same time adjusting for label imbalance. They utilized the popular VGG-16 network architecture [1] and trained from random initialitzation on CelebA.

Ehrlich et al. [2] used Restricted Boltzmann Machine (RBM) rather than a CNN, and trained their model with both CelebA face images and facial landmark points as inputs. They extended RBMs to handle multiple tasks and multiple inputs, creating a Multitask Multimodal RBM (MTM-RBM).

## References

1. Very deep convolutional networks for large-scale image recognition. Simonyan K 2014
2. Facial attributes classification using multi-task representation learning.
3. Moon: a mixed objective optimization network for the recognition of facial attributes
4. Ethnicity Identification from Face Images. Lu et al. (2004)
5. Pattern Classification Duda et al. (2000)
6. Facial Part Displacement Effect on Template-Based Gender and Ethnicity Classification. Manesh et al. (2010)
7. A Framework for Joint Estimation of Age, Gender and Ethnicity on a Large Database. Guo et al. (2014)
8. Facial Image Retrieval Based on Demographic Classification. Wu et al. (2004)
9. Automated Person Categorization for Video Surveillance Using Soft Biometrics Demirkus et al. (2010)
10. Race Classification from Face Images Using Local Descriptors. Muhammad et al. (2010)
11. On Combining Face Local Appearance and Geometrical Features for Race Classification. Becerra-Cierra et al. (2018)
12. Soft Biometric Classification Using Periocular Region Features. Lyle et al. (2012)
13. Iris Image Classification Based on Hierarchical Visual Codebook. Sun et al. (2014)
14. A Robust Approach to Facial Ethnicity Classification on Large Scale Face Databases
15. Ethnicity estimation with facial images. Hosoi et al. (2004)
16. Race Classification Based on Facial Features. Roomi et al. (2011)
17. Gender and Race Classification Using Geodesic Distance Measurement. Marzoog et al. (2022)
18. Deep Neural Network for Halftone Image Classification Based on Sparse Auto-Encoder. Zhang et al. (2016)
19. Facial Ethnicity Classification with Deep Convolutional Neural Networks. Wang et al. (2016)
20. HCP: A Flexible CNN Framework for Multi-Label Image Classification. Wei et al. (2015)
21. A Comparative Analysis of Gender Classification Techniques. Khan et al. (2013)
22. Race Recognition Using Deep Convolutional Neural Networks. Vo et al. (2018)