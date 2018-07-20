# Clothes-recognition (Version 1)

### Research
After reading [DeepFashion / FashionNet paper](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Liu_DeepFashion_Powering_Robust_CVPR_2016_paper.pdf), I knew a simple CNN and any transfer learning models is NOT going to give any good result. Problem is, First, clothes often have large variations, Second, clothing deforms, Third, clothing images are taken under different scenarios. The solution suggested in FashionNet, which requires several split CNN branches for different attributes then concatenate later. I have not work on multiple attributes on a single label, neither have experience coding complex CNN like the FashionNet. This task is out of my current ability for now. Research over github for fashionNet, none are comprehensive, or claims good accuracy. I have also considered Mask-RCNN, but dataset do not have segmentation annotation. 

Instead of giving up,  I decided to give it a shot using some techniques I know, with a little tweak in order to come close to FashionNet.

### Dataset 
The given Dataset containes 18438 files (i.e 6146 sets of jpg, xml and txt), all in one folder. The filename is one way to recognize its label. There are 6x classes, [dress,jeans,shorts,skirt,tee,blouse]. XML only has bbox and label, no segmented info. TXT should be for the landmark attribute, but unsure, since no details written. I have tried looking into dataset from [Deepfashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion/AttributePrediction.html), but arrangement and landmark details are overwhelming. I have not work on multiple attributes on a single labels before, not sure how to fit into Keras layers. 

### Plan of attack
I will train in total 3x separate model. (1) A binary model to classify 2x classes, [Upper and Lower]. (2) A categorical model for 3x classes [dress, blouse, tee] if model 1 predicts class "Upper'. (3)  A categorical model for 3x classes [jeans, shorts, skirt] if model 1 predicts class "Lower'. (Lastly) A separate notebook to load all 3x model weights, for the conditional prediction mentioned above. Before I start working on the mentioned above, I will just run 1x simple CNN model and 1x VGG16 model over entire dataset, just for baseline. 


### Result
- Baseline 1. [Juypter: Custom CNN design](https://github.com/noelcodes/Clothes-recognition/blob/master/Custom%20CNN%20baseline.ipynb)
```
Epoch 200/200
184/184 [=====================] - 20s 108ms/step - loss: 0.2652 - acc: 0.9001 - val_loss: 0.9794 - val_acc: 0.7589
```

There is definitely something wrong with my confusion matrix, this does not look like having a val_acc 0.7589. 

- Baseline 2 : [Juypter: VGG16](https://github.com/noelcodes/Clothes-recognition/blob/master/VGG16%20-%20baseline.ipynb) Transfer learning VGG16. Freeze the top then replace last layer with ours. Unforturnately, I had to terminate training halfway, due to bad accuracy. No point continuing. 
```
Epoch 113/200
182/182 [=====================] - 43s 236ms/step - loss: 1.3257 - acc: 0.3171 - val_loss: 1.2433 - val_acc: 0.3304 
```

- I know another technique using Google's Tensorflow Object Detection API + to train on Faster-RCNN, since it requires using XML files, which the dataset has provided. However, very strange that the API rejects the xml. Script to convert xml_to_csv always error out. So decided to ditch the idea. 

- Due to failure of VGG16, I will use custom designed CNN to train the 3 models below.

- Train Model 1 : [Juypter: model1](https://github.com/noelcodes/Clothes-recognition/blob/master/model1_upper_lower.ipynb). This binary model_1 will classify [Upper,Lower]. Dataset had to be rearrange respectively, all dress,tee,blouse pictures in 'upper' folder, all jeans, shorts, skirt into 'lower' folder. 
```
Epoch 150/150
166/166 [=====================] - 17s 104ms/step - loss: 0.0589 - acc: 0.9804 - val_loss: 0.1301 - val_acc: 0.9601

```

- Train Model 2 : [Juypter: model2](https://github.com/noelcodes/Clothes-recognition/blob/master/model2_lower.ipynb). This categorical model_2 will classify [jeans, shorts, skirt]. Dataset rearranged too.
```
Epoch 500/500
73/73 [======================] - 9s 129ms/step - loss: 0.2313 - acc: 0.9097 - val_loss: 4.5684 - val_acc: 0.3021
```
Model2 shows sign of overfitting. I have changed model3, removed several layers, added some dropouts, see if it helps.
- Train Model 3 : [Juypter: model3](https://github.com/noelcodes/Clothes-recognition/blob/master/model3_upper.ipynb). This categorical model_3 will classify [dress,tee,blouse]. 
```
Epoch 150/150
74/74 [=====================] - 6s 81ms/step - loss: 0.3065 - acc: 0.8824 - val_loss: 4.1002 - val_acc: 0.3524
```
Modified model on 3 did not help much. I guess a more complex model is needed for this. Or it just might just need to train on attributes instead of label to get better result.

- Combine_cnn:  [Juypter: combine_cnn](https://github.com/noelcodes/Clothes-recognition/blob/master/combine_cnn.ipynb). Here we will call model1, if new test image classified lower then load model2 to classify if its [jeans, shorts, skirt], else call model_3 to classify [dress,tee,blouse].
![alt text](https://i.imgur.com/jrxKZAz.jpg)
![alt text](https://image.ibb.co/kBszWo/combocnn.jpg)
#### Conclusion
All 3x models did not perform well, therefore expected low accuracy. OK, I know the above is a dumb. To improve this, I would classify attributes and landmark, instead of just labels, and train it over Faster-RCNN model for better accuracy. 
