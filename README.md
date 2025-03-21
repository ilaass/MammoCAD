# *CAD System for Mammography: Detection, Segmentation, Classification, and Super-Resolution Enhancement*
AI-powered mammography image processing pipeline for mass detection, segmentation,
classification, and enhancement using deep learning. 

## Table of Contents
1. [About the project](#1-about-the-project)
2. [Experiment results](#2-experiment-results)
3. [How to use](#3-how-to-use)


## 1. About the project
This system is designed to assist through future improvements in the automatic analysis of mammography images, implementing a Computer-Aided Detection and Diagnosis system.\
The three main tasks are described below:

1. *Mass Detection (performed through YOLOv8)*
2. *Mass Instance Segmentation (performed through YOLOv8)*
3. *Mass Classification (Modified ResNet with binary input)*
4. *Super-Resolution (ESRGAN)*
   
### Abstract
This project presents a Computer-Aided Detection and Diagnosis (CADe/CADx) system for mammogram analysis that integrates deep learning techniques for mass detection, segmentation and classification.  The system is trained and evaluated on the INBreast dataset which is particularly suitable for this purpose due to its high-resolution images and detailed ground truth annotations. Our proposed pipeline begins with a pre-processing stage in order to improve model robustness, this includes data augmentation techniques such as contrast enhancement, noise addition and CLAHE. Successively it exploits a YOLOv8-based mass detection and instance segmentation model and a ResNet-based classifier to differentiate between benign and malignant masses. An Enhanced Super-Resolution GAN (ESRGAN) was also implemented with the goal of improving fine detail preservation, which is crucial for detection, segmentation, and classification. While ESRGAN is not currently integrated into our CADe/CADx system, future iterations may explore its use as a pre-processing step to further improve performance.

### Dataset
The dataset we are using is *INBreast*.
The INBreast dataset is a high-quality mammographic database designed for breast cancer research. 
It consists of 115 cases from 90 different patients, with a total of 410 full-field digital mammograms (FFDMs) in DICOM format.
Each image is expert-annotated with regions of interest (ROIs), which are labeled as masses, calcifications, distortions
or spiculated regions.
The dataset provides both:
- annotations related to ROIs of each image (including lesion properties such as size, intensity statistics, and contour
points in pixel and millimeter spaces), available in XML format.
- metadata (e.g., BIRADs class) in CSV format.

The high-quality annotations and comprehensive lesion diversity made the
INbreast dataset particularly suitable for our system tasks.

## 2. Experiment results

### Detection
Here are the normalized confusion matrix, F1 curve, Precision curve, and Recall curve obtained by running respectively YOLOv8s.pt (Small) and YOLOv8m.pt (Medium) fine-tuned on the augmented dataset with CLAHE

#### *YOLOv8s*
<p>
  <img src="github/detection/clahe_det_small_confusion_matrix_normalized.png" width="500"/>
  <img src="github/detection/F1_curve_small_clahe.png" width="400"/>
  <img src="github/detection/P_curve_small_clahe.png" width="400"/>
  <img src="github/detection/R_curve_small_clahe.png" width="400"/>   
</p>

#### *YOLOv8m*

<p>
  <img src="github/detection/clahe_det_medium_confusion_matrix_normalized.png" width="500"/>
  <img src="github/detection/F1_curve_medium_clahe.png" width="400"/>
  <img src="github/detection/P_curve_medium_clahe.png" width="400"/>
  <img src="github/detection/R_curve_medium_clahe.png" width="400"/>  
</p>

This is an example of the prediction output of our detection model (bounding boxes and class probabilities) compared to the ground truth annotations in the dataset.
<p>
  <img src="github/detection/val_batch0_pred.jpg" width="450"/>
  <img src="github/detection/val_batch0_labels.jpg" width="450"/>
</p>

### Segmentation
Here are the normalized confusion matrix, F1 curve, Precision curve, and Recall curve obtained by running respectively YOLOv8n-seg.pt (Nano)and YOLOv8m-seg.pt (Medium) fine-tuned on the augmented dataset with CLAHE

#### *YOLOv8n-seg*
<p>
  <img src="github/segmentation/clahe_seg_nano_confusion_matrix_normalized.png" width="500"/>
  <img src="github/segmentation/clahe_seg_nano_MaskF1_curve.png" width="400"/>
  <img src="github/segmentation/clahe_seg_nano_MaskP_curve.png" width="400"/>
  <img src="github/segmentation/clahe_seg_nano_MaskR_curve.png" width="400"/>   
</p>

#### *YOLOv8m-seg*

<p>
  <img src="github/segmentation/clahe_seg_med_confusion_matrix_normalized.png" width="500"/>
  <img src="github/segmentation/clahe_seg_med_MaskF1_curve.png" width="400"/>
  <img src="github/segmentation/clahe_seg_med_MaskP_curve.png" width="400"/>
  <img src="github/segmentation/clahe_seg_med_MaskR_curve.png" width="400"/>  
</p>

This is an example of the prediction output of our segmentation model (per instance masks) compared to the ground truth annotations in the dataset.

<p>
  <img src="github/segmentation/clahe_seg_med_val_batch0_pred.jpg" width="450"/>
  <img src="github/segmentation/clahe_seg_med_val_batch0_labels.jpg" width="450"/>
</p>

### Detection and segmentation models evaluation
An example of output deriving from the evaluation on test sets either on detection and instance segmentation is provided in the `det_seg_test_results/` directory (`val_det`, `valseg` respectively). Specifically:
- Detection is performed using pre-trained model YOLOv8s
  
| Metric       | Precision | Recall | mAP@50 | mAP@50-95 | F1 Score |
|-------------|-----------|--------|-------|----------|---------|
| **Value**   | 86.94%    | 46.32% | 57.72% | 35.38%   | 60.44%  |

- Instance Segmentation is performed using pre-trained YOLOv8m
  
| Metric       | Precision | Recall | mAP@50 | mAP@50-95 | F1 Score |
|-------------|-----------|--------|-------|----------|---------|
| **Value**   | 79.21%    | 40.51% | 62.54% | 40.97%   | 53.61%  |


### Classification
Test set evaluation:

| Metric    | Accuracy | Precision | Recall | F1 Score |
|-----------|----------|-----------|--------|----------|
| **Value (%)** | 96.22%   | 96.34%    | 95.18% | 95.76%   |


### Image enhancement with ESRGAN
The following images are the high-resolution outputs generated by the ESRGAN model from low-resolution input images 

<p>
  <img src="github/ESRGAN/img1.jpg" width="400"/>
  <img src="github/ESRGAN/img2.jpg" width="400"/>
  <img src="github/ESRGAN/img3.jpg" width="400"/>  
</p>

| Metric    | SSIM | PSNR |
|-----------|----------|-----------|
| **Validation set value** | 0.6884 | 29.7391 dB |
| **Test set value** | 0.6938 | 29.8932 dB |

**SSIM (Structural Similarity Index Measure) score** = 0.6884 

## 3. How to use
This project is implemented on a Linux-based operating system (Ubuntu 22.04.5 LTS, 64-bit).\
To run the code, a working Python environment is required.\
The system utilizes an NVIDIA GeForce RTX GPU, optimized for GPU acceleration with CUDA for enhanced performance.


Thus, It is recommended to create a Conda environment for this purpose.
```shell
conda create --name mammocad python=3.9
```
```shell
conda activate mammocad
```

Once you have downloaded this system from GitHub, navigate inside the `mammography_cad` directory (customizing path/to/directory/ with respect where you have located the system)
```shell
cd path/to/directory/mammography_cad
```




### Requirements
As first step, please install the dependencies needed for running the system.
```shell
pip install -r requirements.txt
```

### Data collection 
INbreast dataset is publicly avaiable at this link: https://www.kaggle.com/datasets/ramanathansp20/inbreast-dataset .
Once you have downloaded and unzipped the data, you have to add a `raw` folder and place it inside `data/` directory.\
Then, you have to fill it with the following folders/files:
- AllDICOMs
- AllXML
- INbreast.csv 

The *data* directory should have the following structure:
```graphql
    mammography_CAD
    │── data/                  # Datasets and preprocessing scripts
    │   │── raw/               
    │   │   │── AllDICOMs/            # DICOMs folder
    │   │   │   │── 20586908_6c613....dcm
    │   │   │   │── 20586934_6c613....dcm
    │   │   │    ...
    │   │   │── AllXML/               # XML folder
    │   │   │   │── 20586908.dcm
    │   │   │   │── 20586934.dcm
    │   │   │   ...
    │   │   └── INbreast.csv/         # CSV file
    │   │
    │   │── processed/         # Preprocessed data 
     ...

```

### Data preprocessing
To preprocess the dataset, go to *data/* directory and run dataset_preparation.py script. 
This step can be done running consequently the following commands
```shell
    cd data
    python dataset_preparation.py
```
   And then come back to the main directory

```shell 
    cd -
```
After the execution of this preprocessing step, the PNG processed images results to be located in `data/processed/` directory, while the JSON files are located in `data/json/` directory.

Specifically, it generates:
- the converted version of INbreast images from DICOM to PNG format (*`AllPNG`*)
- the augmented version of the dataset (augmentations: contrast adjustment, noise addition) in PNG format (*`augmentedPNG`*)
- the previous augmented version of the dataset enhanced using **CLAHE** (Contrast Limited Adaptive Histogram Equalization) (*`clahePNG`*)
- the file JSON containing a dictionary with information from INbreast.csv and from the annotations in XML format (*`dataset.json`*)
- the file JSON containing a dictionary with information from INbreast.csv and from the annotations in XML format for the augmented dataset (*`augmented.json`*)

As last step for preprocessing the data, run the following command to generate (inside `data/mass_masks` directory) the *binary masks* obtained from the annotations in the INbreast dataset (such as the contour points of the masses), 

   ```shell
      python utils/extract_masks.py
   ```

### Running the CAD system

Once you have everything ready, we can start to run the system.
For detection and instance segmentation stages, the system uses the pre-trained YOLOv8 (initialized with pretrained weights from COCO dataset), which is 
then fine-tuned on the mammography dataset.

**IMPORTANT**: 
- To test the **classification** and **ESRGAN** models without training, 
skip the training commands (but keep the other commands). Place the checkpoint files 
from the `results_train/` folder into the `runs/` directory. 
The repository structure should be as follows:
```graphql
    mammography_CAD
    │── data/                 
    │── dataset/               
    │── eval/            # DICOMs folder
    │── models/
    │── results/
    │── runs/               # XML folder
    │   │── detect
    │   │── esrgan
    │       │── discriminator.pt
    │   │   └── generator.pt
    │   │── resnet
    │   │   └── resnet_classifier.pt
    │   └── segment
   
     ...

```
- **Detection** and **mass segmentation** training outputs are available in the `results_train/` folder.
  Unfortunately, due to the fileS size, the weights of YOLOv8m and Yolov8s are not available.

#### 1. Detection stage (YOLOv8)

- First of all, the dataset labels must be structured to be compatible with the YOLOv8 model configured for object detection.
\
Additionally, the dataset should be split into training, validation, and test sets to properly train and evaluate the YOLOv8 model.\
For these purposes, run the `main_det.py` script as follows
     ```shell
      python main_det.py --mode split --data clahe 
     ```
  
  This command will generate also the YAML file for training. \
As shown, it is possible to choose to perform detection task using the augmented dataset *with or without the CLAHE enhancement*.\
  To specify which version of the dataset to use, include either `clahe` or `augmented` as part of the `--data` argument in the Python command.


- For training the detection system, use the following command
    ```shell
      python main_det.py --mode train --model yolov8m.pt --data clahe --data_path "/home/francesca/Desktop" --epochs 70 --batch 4
     ```
- For evaluating the system on the test set, after training step take the weights folder inside `runs/detect/train` and put it into `detect` directory (the directory should be `runs/detect/weights/best.pt`) and then use the following command:
     ```shell
      python main_det.py --mode test --data_path /home/francesca/Desktop/mammography_cad
     ```
    
    **IMPORTANT**: change the `--data_path` accordingly to the directory where you have saved `mammography_cad` directory
    
    Please, choose `--data` argument accordingly to the version of the dataset you have used in `split` mode.

    Another choice is related on which pre-trained version of the Yolov8 model you want to train, which could be either `yolov8n.pt`, `yolov8s.pt` or `yolov8m.pt`.

    The epochs and batch size are also configurable, thus they can be adjusted based on the computational resources available, 
such as the GPU capabilities and overall system performance. 

The training results will be saved in `runs/detect/` directory
 
#### 2. Instance Segmentation stage (YOLOv8)

- As in detection stage, the dataset labels must be structured to be compatible with the YOLOv8 model, which is configured now for instance segmentation.
\
Additionally, the dataset should be split into training, validation, and test sets to properly train and evaluate the YOLOv8 model.\
For these purposes, run the `main_seg.py` script as follows
     ```shell
      python main_seg.py --mode split --data clahe
     ```
  As for detection, it is possible to choose to perform mass segmentation task using the augmented dataset *with or without the CLAHE enhancement*.\
  To specify which version of the dataset to use, include either `clahe` or `augmented` as part of the `--data` argument in the Python command.


- For training the mass segmentation system, use the following command
    ```shell
      python main_seg.py --mode train --model yolov8m-seg.pt --data clahe --data_path "/home/francesca/Desktop" --epochs 80 --batch 4    
    ```
    
- For evaluating the system on the test set, after training step take the weights folder inside `runs/segment/train` and put it into `segment` directory (the directory should be `runs/segment/weights/best.pt`) and then use the following command:
     ```shell
      python main_seg.py --mode test --data_path /home/francesca/Desktop/mammography_cad
     ```
    **IMPORTANT**: change the `--data_path` accordingly to the directory where you have saved `mammography_cad` directory
    
    Please, choose `--data` argument accordingly to the version of the dataset you have used in `split` mode.

    The pre-trained version of the Yolov8 model can be selected among either `yolov8n-seg.pt`, `yolov8s-seg.pt` or `yolov8m-seg.pt`.


The training results will be saved in `runs/segment/` directory


#### 2. Classification stage (modified ResNet)

- First, we have to split the dataset in *train, val* and *test* sets, running the following command
    ```shell
    python main_class.py --mode split 
    ```
- Then, for training the classification model, run the command below
    ```shell
    python main_class.py --mode train --epochs 80
    ```
- For validating the classification model, run the command below 
    ```shell
    python main_class.py --mode val
    ```
  It provides
- For evaluating the classification model on test set, run the command below 
    ```shell
    python main_class.py --mode test
    ```



### Running the Super Resolution GAN system
- For creating high-resolution and low-resolution pairs and then split the dataset in *train, val* and *test* sets, run the following command
 ```shell
    python main_esrgan.py --mode split --data clahe
 ```
It is possible to use the augmented dataset *with or without the CLAHE enhancement* also at this stage.\
As before, to specify which version of the dataset to use, include either `clahe` or `augmented` as part of the `--data` argument in the Python command.

- For training the ESRGAN, run the following command 
 ```shell
    python main_esrgan.py --mode train
 ```

- For validating the ESRGAN, run the following command
 ```shell
    python main_esrgan.py --mode val
 ```

- For evaluating the ESRGAN on test set, run the following command
 ```shell
    python main_esrgan.py --mode test 
 ```

