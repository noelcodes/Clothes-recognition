Dear readers: If you are reading this NOW (last update 11th July 2018). Works still on going, and commits might be incomplete. 

# Clothes-recognition
Clothes recognition: This is a task given by a company as part of the interview process for an internship role, and deadline is 1 week (17July2018). 

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
.   Dataset link is confidential (sorry guys)
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
I have 1 week to do this. After reading [DeepFashion / FashionNet paper](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Liu_DeepFashion_Powering_Robust_CVPR_2016_paper.pdf), I knew a simple CNN and any transfer learning models is NOT going to give any good result. Problem is, First, clothes often have large variations, Second, clothing deforms, Third, clothing images are taken under different scenarios. The solution suggested in FashionNet, which requires several split CNN branches for different attributes then concatenate later. I have not work on multiple attributes on a single label, neither have experience coding complex CNN as FashionNet. Research over github for fashionNet, none are comprehensive, or claims good accuracy. I have also considered Mask-RCNN, but dataset do not have segmentation annotation. 

So I'll be using the method I know, with a little tweak in order to come close to FashionNet.

### Dataset 
The given dataset do not come with much details. Dataset containes 18438 files (i.e 6146 sets of jpg, xml and txt), all in a single folder. Not arrange in folders. The filename is one way to reconige its class. XML only has bbox and label info, no segmented info. TXT should be for the landmark attribute, but unsure, since no details written. I have tried looking into dataset [Deepfashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion/AttributePrediction.html), but arrangement and landmark details are overwhelming. I have not work on multiple attributes on a single labels before, not sure how to fit into Keras. 

### My General idea: 
I will train in total 3x separate model. (1) A binary model to classify 2x classes, Upper and Lower. (2) A categorical model for 3x classes [dress, blouse, tee] if model 1 predicts class "Upper'. (3)  A categorical model for 3x classes [jeans, shorts, skirt] if model 1 predicts class "Lower'. (Lastly) A separate notebook to do load all 3x model weights, for the conditional prediction mentioned above. Before I start working on the mentioned above, I will just run 1x simple CNN model and 1x VGG16 model over entire dataset, just for baseline. 


### Result
- Baseline 1. [Juypter notebook: Custom CNN via Keras](https://github.com/noelcodes/Clothes-recognition/blob/master/Custom%20CNN%20baseline.ipynb)
```
Epoch 200/200
184/184 [==============================] - 20s 108ms/step - loss: 0.2652 - acc: 0.9001 - val_loss: 0.9794 - val_acc: 0.7589
```
![alt text](https://i.imgur.com/17KwF0Z.jpg)
![alt text](https://i.imgur.com/jrxKZAz.jpg)
There is definately something wrong with my confusion matrix, this does not look like having a val_acc 0.7589. 

- Baseline 2 : Transfer learning VGG16 - 2nd baseline. I terminated training halfway, due to bad accuracy. No point continuing. [Juypter notebook: VGG16](https://github.com/noelcodes/Clothes-recognition/blob/master/VGG16%20-%20baseline.ipynb)
```
Epoch 113/200
182/182 [==============================] - 43s 236ms/step - loss: 1.3257 - acc: 0.3171 - val_loss: 1.2433 - val_acc: 0.3304 
```

- Model 1 : Preparing images to be train on Faster-RCNN. Weights are in inference_graph. The only method I know to how to create such model is thru Google's Tensorflow Object Detection API. This requires XML files, which the dataset originally has provided. However, very strange that the API rejects the xml. Troubleshooting script to convert xml_to_csv.  
- 12July : Morning going for job interview for another company.
- 12July : Back to work. Decided to ditch Google's Tensorflow Object Detection API method. I have an idea.

