# Object Detection Exercises: Comparison of Traditional and Deep Learning Methods

This project explores traditional and deep learning-based object detection techniques, highlighting the capabilities, limitations, and performance of each. Through exercises involving HOG-SVM, YOLO, and SSD, we aim to observe and document differences in detection speed and accuracy.


## Overview 
This project compares traditional and deep learning methods for object detection. We explore:

- HOG-SVM (Histogram of Oriented Gradients - Support Vector Machine): A traditional feature extraction and classification approach.

- YOLO (You Only Look Once): A real-time, single-shot deep learning detector.

- SSD (Single Shot MultiBox Detector): A deep learning model that balances speed and accuracy for real-time detection.

## Exercises

### Exercise 1: HOG-SVM Object Detection

**Approach**

- Loaded the image and converted it to grayscale for feature extraction.

- The HOG algorithm was used to extract gradient-based features.

- Implemented an SVM-based detector to identify objects based on the HOG features.

**Observations**

**Strengths:** HOG-SVM works well for detecting certain objects, like pedestrians, under favorable lighting and background conditions.

**Limitations** Limited flexibility to detect diverse objects due to fixed feature extraction, slower performance, and less adaptability to complex backgrounds.


**Results**

- The model successfully detected people in images with relatively simple backgrounds but struggled with cluttered or complex environments.

### Exercise 2: YOLO Object Detection

**Approach**

- Loaded the YOLOv3 model pre-trained on the COCO dataset.

- Processed the image into a YOLO-compatible format and performed detection.

- Extracted bounding boxes and class labels, and displayed results.

**Observations**

**Strengths:** Fast inference, able to handle multiple objects in real-time, and adaptable to a variety of objects.

**Limitations:** May miss very small objects or details within dense images due to single-shot design.

**Results** 

- YOLO detected various objects accurately, showing strong performance even with multiple objects in a single image.

### Exercise 3: SSD Object Detection

**Approach**

- Loaded the SSD MobileNet V2 model trained on the COCO dataset.
- Converted the image to the SSD input format and used the TensorFlow model for predictions.
- Drew bounding boxes and labeled detected objects.

**Observations**

**Strengths:** Balanced speed and accuracy, efficient for embedded systems where resources are limited.

**Limitations:** May have slightly lower accuracy than YOLO on densely packed objects or small details.

**Results**

- SSD successfully detected a range of objects with a good balance of speed and accuracy, performing slightly slower but similarly accurate to YOLO.

### Exercise 4: Traditional vs. Deep Learning Comparison

**Approach**

- Measured detection accuracy and speed for HOG-SVM, YOLO, and SSD across a consistent dataset.

- Used HOG for traditional detection and YOLO + SSD for deep learning comparisons.

- Documented speed and accuracy metrics for each model.

**Observations**

**HOG-SVM:** Slower with lower accuracy on complex images but reliable for specific object types

**YOLO:** High accuracy and fast speed, suitable for real-time applications.

**SSD:** Good balance of speed and accuracy, ideal for scenarios needing moderate resource usage.

**Results**

**Accuracy:** YOLO > SSD > HOG-SVM
**Speed:** YOLO > SSD > HOG-SVM

## Summary of Observations

| **Method** | **Advantages**              | **Disadvantages**                      |
|------------|-----------------------------|----------------------------------------|
| **HOG-SVM** | Simple, interpretable      | Limited object variety, slower         |
| **YOLO**    | Real-time, high accuracy   | May miss small or dense objects        |
| **SSD**     | Balanced speed and accuracy | Slightly less accurate than YOLO       |


## Conclusion 

This project demonstrates how different object detection methods, ranging from traditional to deep learning approaches, are suited to different use cases. YOLO and SSD offer powerful tools for real-time object detection, especially for applications requiring quick decision-making. Traditional methods like HOG-SVM remain valuable for specific use cases where interpretability is a priority.
