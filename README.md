To readers: If you are reading this NOW (last update 11th July 2018). Works still on going, and commits might be incomplete. 

# Clothes-recognition
Clothes recognition: This is a task given by a company as part of the interview process for an internship role, and deadline is 1 week (17July2018). Due to time constraint, I'm going to tackle this using a few techniques I have already know, since I have ready codes, as baseline. Then if I have more time, I will try out something I don't know, i.e fashionet, based on a paper dated 2016.

## Task details: Copy paste directly from email.
```
Deep Learing based Person Cloth Attribute Indicator
Purpose – Develop or train a deep learning model that can identify the type of cloth (upper and lower body) of person.
Expected Results
·   Deep learning model in Keras/Tensorflow/Caffe that can indicate type of upper and lower cloth of a person.
·   You can determine the number of class for type on your own that you would like to train (please to inform us once you have decided on the type ), example could be long sleeves shirt, short sleeve shirt, sleeveless shirt, suit, jacket etc. for upper body, and short pants, long pants, skirt, jeans etc. for lower body.
·   You can add even more type of person attribute if you could find relevant dataset and confident that they can deliver within the given assignment time.

Requirements, Constraints and Hints
·   The trained model should be robust to viewing angle on a person and different camera.
·   An example of dataset for type of cloth with label can be found below:
     https://xrvisionpteltd-my.sharepoint.com/:u:/g/personal/ralphlee_xrvision_com/EfDOV4KjNNpElb7GMG_rOcABFvBRiI_n2JkqzRi_P4MvXQ?e=IFdVb2
 
	
trainClothAttribute
Shared via OneDrive
·   For your information, the label in the above dataset contains the location of the cloth and its type in their respective images in xml format.
·   If you felt these dataset is not suitable for your test case or you need more data to make the model more accurate, you can use other material in the internet for open source dataset.
·   The targeted accuracy would be at least 90% in determining the type of the cloth.
·   You are also free to use any resources to accomplish the assignment, including: code samples, open source libraries, research materials etc.

Assignment Deliverables
·   Model that can be used in Python/C++ with Keras/Tensorflow/Caffe.
·   A code which input your model and output the results of a selected image.
·   A short write-up/presentation describing your approach/architecture for the deep learning.

Feel free to come back to us for any further clarification on the assignments. Once competed please send the source code, model and report regarding the chosen algorithms and results. You are free to use any resources to accomplish the assignment, including: code samples, open source libraries, research materials etc.
 
Your assignment ranking will be based on the robustness and generalization of the solution and the thought process behind your approach and implementation.
```

## Plan of attack
I have 1 week to do this. The general idea is this. After reading DeepFashion paper 

https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Liu_DeepFashion_Powering_Robust_CVPR_2016_paper.pdf
