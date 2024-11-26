# Liver Tumor Segmentation

A liver tumor refers to an abnormal growth of cells in the liver, which may be either benign (non-cancerous) or malignant (cancerous). Malignant liver tumors are commonly referred to as liver cancer, with hepatocellular carcinoma (HCC) being the most prevalent primary liver cancer type. Other types include cholangiocarcinoma and secondary liver cancer, where cancer spreads to the liver from other parts of the body. Early detection and precise diagnosis are critical for effective treatment and improving patient outcomes

AI, particularly deep learning, has revolutionized liver tumor detection through the use of medical image segmentation. Image segmentation is a process where AI models analyze and label specific regions in medical images like CT or MRI scans. For liver tumor detection, segmentation involves isolating the liver and precisely identifying tumor regions

This outlines the process of developing a 3D liver segmentation model using deep learning techniques. The model aims to accurately delineate liver structures from medical imaging data, specifically NIfTI format images, which are often used in medical imaging for 3D volumetric data.

## Dataset

The dataset used in this project is derived from the Task03 Liver of the Medical Segmentation Decathlon challenge. The dataset consists of:

Images: 3D MRI scans of the liver in NIfTI format (*.nii files).
Labels: Corresponding ground truth segmentation masks indicating the liver region.
The images and their associated labels are organized into directories, with separate folders for training data. The paths to these files are dynamically generated using globbing techniques to ensure all relevant data is included.


## Data Preprocessing

### NIfTI Data Loading

The model begins by loading the NIfTI files using the nibabel library. The images and labels are read and transposed to ensure correct dimensionality (i.e., the shape is adjusted from (depth, height, width) to (width, height, depth)) for further processing.

### Slicing
Each 3D volume is sliced into 2D images for segmentation. The dataset ensures that only slices with relevant ground truth data (i.e., slices where the liver is present) are included. This helps reduce noise in the training data and improves model performance.

### Data Normalization
To ensure the model trains effectively:

Negative values in the images are set to zero.
Each image is normalized by dividing it by its maximum pixel value, ensuring the values range between 0 and 1. This normalization helps the model converge more quickly during training.

### Data Augmentation
Data augmentation techniques are applied to increase the diversity of the training dataset. This includes resizing the images and applying random transformations such as rotations and flips, which are crucial for improving the robustness of the model against overfitting.


### Model Architecture
The segmentation model is built using the Unet architecture, which is particularly effective for biomedical image segmentation tasks. The model's key characteristics include:

Encoder-decoder structure: It captures context through downsampling and enables precise localization through upsampling.
Residual connections: These help in training deeper networks by mitigating the vanishing gradient problem.

### Model Configuration
    Encoder: ResNet34 is used due to its proven effectiveness in feature extraction.
    Input channels: The model takes single-channel input (grayscale images).
    Output classes: The model is configured for binary classification (liver vs. background).

### Evaluation Metrics

The model's performance is assessed using:

    Pixel Accuracy (PA): The ratio of correctly predicted pixels to the total pixels.
    Mean Intersection over Union (mIoU): A robust metric that evaluates the overlap between predicted and ground truth masks.
