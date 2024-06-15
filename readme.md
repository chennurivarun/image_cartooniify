# Cartoonifier

This project converts an image into a cartoon-like image using OpenCV and other image processing libraries. The script allows users to upload an image and apply various transformations to create a cartoon effect.

## Features

- Convert an image to grayscale
- Apply median blur to smoothen the grayscale image
- Detect edges using adaptive thresholding
- Apply bilateral filter to remove noise while keeping edges sharp
- Combine the edge-detected image with the filtered image to create a cartoon effect
- Save the cartoonified image

## Installation

To run this code, you need to install the required libraries. You can do this using the following command:

```python
pip install opencv-python easygui numpy imageio matplotlib pillow
