# MyBananaCam
The Progress Of My BananaCam Project

# void-detector

Detect status of boxes in grocery store shelves.

## Table of Contents
- <a href='#demo'>Demo</a>
- <a href='#visualization'>Visualization</a>
- <a href='#goal'>Goal</a>
- <a href='#log'>Log</a>
    - <a href='#2019-03-22-created-model-based'>2019-03-22: Created Model-Based</a>
    - <a href='#2019-03-25-train-model-with-heavy-data-augmentation '>2019-03-25: Train Model With Heavy Data Augmentation </a>
    - <a href='#2018-02-17-inspect-and-report-results'>2018-02-17: Inspect and report results</a>    
    - <a href='#2018-02-19-understand-the-performance-drop'>2018-02-19: Understand the performance drop</a>        

# Goal

- Build a minimum viable product with the following:
  - No cloud required, work in small device (raspberry with camera)
  - Detect empty and half empty boxes of fruits and vegetables
  - Sending emails to alerts after xx minutes
  
# Log

## 2019-03-22: Created Model-Based 
- [x] Dataset
        - 329 images with ground truth voids
        - 1062 ground truth voids (void-img ratio: 3.22)                

- [x] Created Model's Baseline 
  - Configuration:
    - Epochs: 20
    - Backbone: ResNet50    
 
  
- [x] Inspect and report results
    - Average precision:
        - mAP
        - Confusion Matrix
    
    
## 2019-03-25 Train Model With Heavy Data Augmentation 
- [x] Train model
    - Configuration:        
        - Epochs: 20                
        - Learning rate: 3e-4        
        - Optimizer: SGD
            - Momentum: 0.9
            - Weight decay: 1e-4
        - Applied learning rate schedule
        - Augmentation:
            - Crops and affine transformations to images 
            - Flips some of the images horizontally and vertical 
            - Adds a bit of noise and blur and also changes the contrast as well as brightness.

- [x] Web-interface Modules
    - [x] Login with banana account
    - [x] Store and Retrive Images from SQLITE3
    - [x] Send email
        - Use Yandex account to send mail, work the same with Google Account
        - Send with attachment
    - [?] Synchronize databases with result from model and display
    
           
## 2019-03-26: Train model


## 2018-02-17: Inspect and report results
- [x] Inspect results
    - [x] Visually
        - [x] Look at test set predictions
    - [x] Quantitatively
        - [x] Label data for the test set
            - 80 images with ground truth voids
            - 385 ground truth voids (void-img ratio: 4.81)
            - Images from one set of sides (blue side of route diagram)
        - [x] Calculate average precision (IoU threshold: 0.5)
            - 0.5 is the [PASCAL VOC standard](http://homepages.inf.ed.ac.uk/ckiw/postscript/ijcv_voc09.pdf). CTRL+F: "Bounding box evaluation"
- [x] Report results
    - [x] Visually
        - [x] Create GIF of test set predictions
    - [x] Quantitatively: Average precision
        - **Train: 0.9091** (N: 329)
        - **Test: 0.1672** (N: 80)
        - **These results imply extreme overfitting, but the visual results show decent performance**.
            - <img src="docs/20180215_190227_002190_gt_and_preds.jpg" width="50%">
            - The predictions are in red and the ground truth is in green.                       


## 2018-02-19: Understand the performance drop
- [ ] Study the pipeline
    - [x] List Dataset
    - [x] Data augmentations
    - [x] SSD encoder
    - [ ] FPNSSD512
    - [x] SSD loss
        - [x] Location loss
        - [x] Hard negative mining
        - [x] Class loss
        - My custom loss function is returning giant and NaN losses.
    - [x] SSD decoder
    - [x] Evaluator
    - [ ] Visualize each
- [ ] Train on the first dataset to test code changes

## 2018-02-20: Clean the code
- [x] Train on the first dataset to test code changes
    - Model 1 performed poorly when trained with the new code
- [x] Debug
    - [x] Explain Model 1's precision drop
        - Fact: The training data is the same.
        - Fact: The code is not the same.
        - Fact: New: The saver stopped saving by epoch 20.
        - Fact: The saver saves based on the validation data.
        - Fact: The validation data is not the same.
        - Hypothesis: The difference in validation data is responsible for the drop in precision.
        - [x] Test hypothesis
            - [x] Test with Model 1
                - Result: Train: 0.9091 (N: 329): Hypothesis confirmed
- [x] Merge void-detector repo with void-torchcv repo
- [x] Retrain Model 2
    - Result: **0.9044** (N: 476) (**during bug: 0.6276**) (1st model: 0.9041))
