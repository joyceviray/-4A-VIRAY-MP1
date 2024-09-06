# Machine Problem No. 1: Exploring the Role of Computer Vision and Image Processing in AI

https://github.com/user-attachments/assets/8e0c1682-5f41-48a8-bb47-cf6f105389a0

# Introduction to Computer Vision and Image Processing

Computer vision is a field of artificial intelligence (AI) that uses machine learning and neural networks to teach computers and systems to derive meaningful information from digital images, videos and other visual inputs—and to make recommendations or take actions when they see defects or issues.

Image processing is essential in AI systems as it enables machines to analyze and interpret visual data for tasks like object detection, facial recognition, and medical imaging. It enhances image quality, extracts important features, and allows AI to generate realistic visuals. This capability is crucial for applications such as autonomous vehicles, healthcare diagnostics, and visual search engines.

# Types of Image Processing Techniques

1. Image Enhancement
   - Improves the quality of an image by adjusting its contrast, brightness, or sharpness. For example, histogram equalization enhances contrast by distributing intensity values more evenly.
   - Applications in AI include medical imaging, where enhanced images can help detect subtle anomalies in X-rays or MRIs, and satellite imagery, where clarity is improved for terrain analysis.

2. Image Segmentation
   - Partitions a digital image into discrete groups of pixels—image segments—to inform object detection and related tasks. Example is thresholding, where pixels are categorized based on intensity values.
   - Segmentation is commonly used in AI for object detection in self-driving cars, where it helps isolate vehicles, pedestrians, and road signs from the background, and in medical AI systems for finding cancers in scans.

3. Feature Extraction
   - Identifies key aspects of an image, such as edges, textures, shapes, or colors, which are essential for recognizing objects and patterns. 
Example is sobel operator detects edges in an image by highlighting regions with strong gradients.
   - Used in AI for fundamental tasks like facial recognition, object detection, and handwriting recognition. In healthcare, it is used to extract features from medical scans, aiding AI in diagnosing diseases such as cancer or heart conditions.
  
# CASE STUDY OVERVIEW: Medical Imaging

Fever detection using thermal imaging 
- This technology has been widely used, especially during the COVID-19 pandemic, for quick temperature screening in public places such as airports, hospitals, and offices. Thermal imaging cameras capture infrared radiation emitted by objects, including human skin. The intensity of the radiation corresponds to temperature, and the camera translates it into a thermal map.

# Image processing implementation

Problem: Detect if a person has a fever based on a thermal image of their face.

The model works by combining:
- Face detection (Haar Cascade Classifier) to isolate the region of interest (face).
- Simulated thermal imaging using a colormap to map grayscale values to a thermal spectrum.
- Temperature estimation based on pixel intensity to simulate fever detection.
- Result visualization, where the estimated temperatures are overlaid on the thermal image, indicating whether a person may have a fever based on predefined temperature thresholds.

By focusing on the face and mapping pixel intensity to temperature, the model provides a simple demonstration of how thermal imaging can be used for fever detection in a real-world scenario.

# Conclusion 

Image processing is crucial for AI systems, enabling accurate analysis and interpretation of visual data. It aids in tasks like object detection, facial recognition, and medical diagnosis, making it essential in applications like autonomous vehicles, healthcare, and security. Techniques like enhancement, segmentation, and feature extraction are used in real-world AI applications, such as fever detection using thermal imaging, highlighting the potential of combining image processing with machine learning. 

# Extension Activity: Deep learning based image analysis 

Deep learning-based image analysis is set to redefine image processing in AI systems. Its ability to automatically learn complex features from data enables more advanced, reliable, and efficient visual recognition systems. The impact of these techniques will be felt across various industries, from healthcare and security to entertainment and autonomous vehicles, driving the next wave of AI innovation.

# Hands-On Exploration

Chosen Real-World AI Application: Fever Detection for Medical Imaging 

``` python

import cv2
from matplotlib import pyplot as plt
import numpy as np

# Load the user's image
image_path = 'feversample.jpg'
image = cv2.imread(image_path)

# Convert the image to grayscale for thermal simulation
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Apply a colormap to simulate a thermal image
thermal_image = cv2.applyColorMap(gray_image, cv2.COLORMAP_JET)

# Function to add a temperature scale to the image
def add_temperature_scale(image, min_temp=35, max_temp=40, height=300):
    scale_image = np.zeros((height, 50, 3), dtype=np.uint8)
    
    # Generate a gradient from blue (cold) to red (hot) to simulate the temperature scale
    for i in range(height):
        temp_color = cv2.applyColorMap(np.array([[int(255 * (i / height))]], dtype=np.uint8), cv2.COLORMAP_JET)
        scale_image[height - i - 1, :] = temp_color

    # Resize to fit into the existing image
    scale_image = cv2.resize(scale_image, (50, image.shape[0]))

    # Add the temperature scale to the right side of the image
    result_image = np.hstack((image, scale_image))

    # Add temperature labels
    for i, temp in enumerate(np.linspace(min_temp, max_temp, 5)):
        y_pos = int((i / 4) * (image.shape[0]))
        cv2.putText(result_image, f"{temp:.1f}°C", (image.shape[1] + 10, y_pos), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 255, 255), 1)

    return result_image

# Apply the temperature scale to the simulated infrared image
thermal_with_faces = thermal_image.copy()

# Face detection using OpenCV's Haar Cascade classifier
face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
faces = face_cascade.detectMultiScale(image, 1.3, 5)

# Draw rectangles around detected faces on the visible light image
image_with_faces = image.copy()
for (x, y, w, h) in faces:
    cv2.rectangle(image_with_faces, (x, y), (x + w, y + h), (255, 0, 0), 2)  # Blue boxes for face detection

# Draw rectangles around detected faces on the thermal image for fever detection simulation
for (x, y, w, h) in faces:
    cv2.rectangle(thermal_with_faces, (x, y), (x + w, y + h), (0, 255, 0), 2)  # Green boxes for fever detection

# Add the temperature scale to the thermal image
thermal_with_scale = add_temperature_scale(thermal_with_faces)

# Function to estimate temperature based on pixel intensity in the face region
def estimate_temperature(thermal_image, face_coords, min_temp=35, max_temp=40):
    x, y, w, h = face_coords
    face_region = thermal_image[y:y+h, x:x+w]
    
    # Convert face region to grayscale to calculate intensity
    face_gray = cv2.cvtColor(face_region, cv2.COLOR_BGR2GRAY)
    
    # Calculate average intensity of the face region
    avg_intensity = np.mean(face_gray)
    
    # Map intensity to temperature range
    temp = min_temp + (max_temp - min_temp) * (avg_intensity / 255)
    
    # Display the estimated temperature on the image
    cv2.putText(thermal_image, f"{temp:.1f}°C", (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)

# Apply the temperature estimation for each detected face
thermal_with_temp = thermal_with_scale.copy()
for (x, y, w, h) in faces:
    estimate_temperature(thermal_with_temp, (x, y, w, h))

# Create the updated figure with temperature on the face
fig, ax = plt.subplots(2, 2, figsize=(12, 12))

# (a) Visible light (RGB) image
ax[0, 0].imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
ax[0, 0].set_title('(a) Visible Light (RGB) Image')
ax[0, 0].axis('off')

# (b) Simulated Infrared Image
ax[0, 1].imshow(cv2.cvtColor(thermal_image, cv2.COLOR_BGR2RGB))
ax[0, 1].set_title('(b) Simulated Infrared Image')
ax[0, 1].axis('off')

# (c) Face Detection on Visible Light Image
ax[1, 0].imshow(cv2.cvtColor(image_with_faces, cv2.COLOR_BGR2RGB))
ax[1, 0].set_title('(c) Face Detection on Visible Light Image')
ax[1, 0].axis('off')

# (d) Fever Detection on Infrared Image with Temperature Indicator
ax[1, 1].imshow(cv2.cvtColor(thermal_with_temp, cv2.COLOR_BGR2RGB))
ax[1, 1].set_title('(d) Fever Detection on Infrared Image with Temperature Indicator')
ax[1, 1].axis('off')

# Show the combined figure with all sections
plt.tight_layout()
plt.show()

```

![feversampleresult](https://github.com/user-attachments/assets/585d1465-f2b1-4989-a3b1-f1a39b8c8afc)
