# MyBananaCam
The Progress Of My BananaCam Project

# void-detector

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
    - Backbone: ResNet50    
    - Number of Classes: 4 [empty, half-empty, full, obstacle]
   
- [x] Inspect and report results (test set)
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
    ...loading
      


## Understand the performance 
- [ ] Study the pipeline
    - [x] Number of annotation in the Dataset
    - [x] Data augmentations    
    - [ ] BackBone : ResNet101, ResNet50, MobileNetv1, MobileNetv2        
    - [x] Evaluator
    - [ ] Visualize each

