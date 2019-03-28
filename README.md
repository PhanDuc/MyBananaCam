# MyBananaCam
The Progress Of My BananaCam Project

Detect status of boxes in grocery store shelves.

## Table of Contents
- <a href='#goal'>Goal</a>
- <a href='#log'>Log</a>
    - <a href='#2019-03-22-created-model-based'>2019-03-22: Created Model-Based</a>
    - <a href='#2019-03-25-train-model-with-heavy-data-augmentation '>2019-03-25: Train Model With Heavy Data Augmentation </a>
    - <a href='#2019-03-26-inspect-model'>2019-03-26: Inspect model</a>    
    - <a href='#understand-the-performance'>Understand the performance</a>        

# Goal

- Build a minimum viable product with the following:
  - No cloud required, work in small device (raspberry with camera)
  - Detect empty and half empty boxes of fruits and vegetables
  - Sending emails to alerts after xx minutes
  
# Log

## 2019-03-22: Created Model-Based 
- [x] Created Model's Baseline 
  - Model: Mask RCNN
  - Configuration:
    - Epochs: 20
    - Backbone: ResNet101    
    - Number of Classes: 4 [empty, half-empty, full, obstacle]
   
- [x] Inspect and report results
    - Number of Images : 160
    - Calculate average precision (IoU threshold: 0.5):
        - - 0.5 is the [PASCAL VOC standard](http://homepages.inf.ed.ac.uk/ckiw/postscript/ijcv_voc09.pdf).
        - mAP @ IOU=50: 0.11765
    - Total Annotations of Ground Truth: 3350
        - Full: 2356
        - Half-empty: 796
        - Empty: 176
        - Obstacle: 22
    - Number of Annotations that Model could predicted: 401
        - Full: 356
        - Half-empty: 43
        - Empty: 2
        - Obstacle: 0        
    
## 2019-03-25 Train Model With Heavy Data Augmentation 
- [x] Train model
    - Configuration:        
        - Epochs: 40                
        - Initial Learning rate: 3e-4        
        - Optimizer: SGD
            - Momentum: 0.9
            - Weight decay: 1e-4            
        - The Mask RCNN Model use BatchNormalization -> Activation, I switched that order but placed Activation then BatchNormalization.
        - Applied learning rate schedule
        - Train first 10 epochs with "head"
        - The next 30 epochs from layer "3+"        
        - Augmentation:
            - Crops and affine transformations to images 
            - Flips some of the images horizontally and vertical 
            - Adds a bit of noise and blur and also changes the contrast as well as brightness.
            - Affine transformations
                - Shear (-8, 8)
                - Roration (-50, 50)
                - scale: {"x": (0.8, 1.2), "y": (0.8, 1.2)
                - Translate_percent: "x": (-0.2, 0.2), "y": (-0.2, 0.2)                

- [x] Web-interface Modules
    - [x] Login with banana account
    - [x] Store and Retrive Images from SQLITE3
        - [] Store the output images with number of predicted annotations, (full, half-empty, empty, obstacle) 
    - [x] Send email
        - Use Yandex account to send mail, work the same with Google Account
        - Send with attachment
    - [?] Synchronize databases with result from model and display
    - [?] 
    
           
## 2019-03-26: Inspect model

- [x] Inspect and report results (test set)
    - The Total Number Of Annotations Model Predicted:  1536
    - Number of correct predicted for each classes
        - empty: 82
        - half-empty: 299
        - full: 1155

    - The Total Annotations Of Ground Truth: 3323
        - full: 2331
        - half-empty: 794
        - empty: 176
        - obstacle: 22

    Confusion Matrix
    
            t/p          full      empty   obstacle half-empty 
              full     1155.0        3.0        0.0      102.0 
             empty       16.0       82.0        0.0       71.0 
          obstacle        1.0        0.0        0.0        1.0 
        half-empty      260.0       23.0        0.0      299.0 

    Classification Report
    
                  precision    recall  f1-score   support

           empty       0.76      0.49      0.59       169
            full       0.81      0.92      0.86      1260
      half-empty       0.63      0.51      0.57       582
        obstacle       0.00      0.00      0.00         2

       micro avg       0.76      0.76      0.76      2013
       macro avg       0.55      0.48      0.50      2013
      weighted avg     0.75      0.76      0.75      2013
      
- [Processing] Training
    - LEARNING_RATE = 0.0005
    - LEARNING_MOMENTUM = 0.95    
    - WEIGHT_DECAY = 0.0003
    - Use: RPN_ANCHOR_SCALES = (16, 32, 64, 128, 256)
    - Train from beginning of model

## 2019-03-27: Flask Building
- Continue combine different modules:
    - Login (done)
    - Main page Display images from db (done)
    - Get output from Mask RCNN then write it to db (processing)
    - Connect with camera ()
    - Sending email ()    
    
## 2019-03-28: Flask Building + MobileNet + Improve ResNet50
- Continue training from last checkpoint with this configuration (increase the number of epochs to 75)
    - LEARNING_RATE = 0.0003
    - LEARNING_MOMENTUM = 0.95
    # Weight decay regularization
    - WEIGHT_DECAY = 0.0005

## Understand the performance 
- [ ] Study the pipeline
    - [x] Number of annotation in the Dataset
    - [x] Data augmentations    
    - [ ] BackBone : ResNet101, ResNet50, MobileNetv1, MobileNetv2        
    - [x] Evaluator
    - [ ] Visualize each

