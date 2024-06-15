#Cartoonifier

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
```

## Code Overview
### Importing Libraries
The following libraries are imported for image processing, GUI creation, and handling files:
```python
import cv2                   # for image processing
import easygui               # to open the filebox
import numpy as np           # to store image
import imageio               # to read image stored at particular path
import sys
import matplotlib.pyplot as plt
import os
import tkinter as tk
from tkinter import filedialog
from tkinter import *
from PIL import ImageTk, Image
```

### Setting Up the GUI
A Tkinter window is created with a specific size, title, and background color:

```python
top = tk.Tk()
top.geometry('400x400')
top.title('Cartoonify Your Image!')
top.configure(background='white')
label = Label(top, background='#CDCDCD', font=('calibri', 20, 'bold'))
```

### Upload Function
The upload function opens a file dialog to select an image file and calls the cartoonify function with the selected file's path:  

```python
def upload():
    ImagePath = easygui.fileopenbox()
    cartoonify(ImagePath)
```

### Cartoonify Function
The cartoonify function processes the selected image to create a cartoon effect:

```python
def cartoonify(ImagePath):
    originalmage = cv2.imread(ImagePath)
    originalmage = cv2.cvtColor(originalmage, cv2.COLOR_BGR2RGB)

    if originalmage is None:
        print("Can not find any image. Choose appropriate file")
        sys.exit()

    ReSized1 = cv2.resize(originalmage, (960, 540))
```

## Image Processing Steps
### Convert to Grayscale:
Converts the image to grayscale.

```python
grayScaleImage = cv2.cvtColor(originalmage, cv2.COLOR_BGR2GRAY)
ReSized2 = cv2.resize(grayScaleImage, (960, 540))
```

### Smooth the Grayscale Image:
Applies a median blur to smooth the image.

```python
smoothGrayScale = cv2.medianBlur(grayScaleImage, 5)
ReSized3 = cv2.resize(smoothGrayScale, (960, 540))
```

### Edge Detection:
Detects the edges using adaptive thresholding.

```python
getEdge = cv2.adaptiveThreshold(smoothGrayScale, 255, 
                                cv2.ADAPTIVE_THRESH_MEAN_C, 
                                cv2.THRESH_BINARY, 9, 9)
ReSized4 = cv2.resize(getEdge, (960, 540))
```

### Apply Bilateral Filter:
Applies a bilateral filter to remove noise while keeping edges sharp.

```python
colorImage = cv2.bilateralFilter(originalmage, 9, 300, 300)
ReSized5 = cv2.resize(colorImage, (960, 540))
```

### Create Cartoon Effect:
Combines the edge-detected image with the color image to create a cartoon effect.

```python
cartoonImage = cv2.bitwise_and(colorImage, colorImage, mask=getEdge)
ReSized6 = cv2.resize(cartoonImage, (960, 540))
```

### Displaying the Images
The images at each step of the process are displayed using Matplotlib:

```python
images = [ReSized1, ReSized2, ReSized3, ReSized4, ReSized5, ReSized6]
fig, axes = plt.subplots(3, 2, figsize=(8, 8), subplot_kw={'xticks':[], 'yticks':[]}, gridspec_kw=dict(hspace=0.1, wspace=0.1))
for i, ax in enumerate(axes.flat):
    ax.imshow(images[i], cmap='gray')

save1 = Button(top, text="Save cartoon image", command=lambda: save(ReSized6, ImagePath), padx=30, pady=5)
save1.configure(background='#364156', foreground='white', font=('calibri', 10, 'bold'))
save1.pack(side=TOP, pady=50)

plt.show()
```

### Save Function
The save function saves the cartoonified image to the same directory as the original image with a new name:

```python
def save(ReSized6, ImagePath):
    newName = "cartoonified_Image"
    path1 = os.path.dirname(ImagePath)
    extension = os.path.splitext(ImagePath)[1]
    path = os.path.join(path1, newName + extension)
    cv2.imwrite(path, cv2.cvtColor(ReSized6, cv2.COLOR_RGB2BGR))
    I = "Image saved by name " + newName + " at " + path
    tk.messagebox.showinfo(title=None, message=I)
```

### Main Loop
The main loop creates a button to upload an image and starts the Tkinter main loop to run the GUI:

```python
upload = Button(top, text="Cartoonify an Image", command=upload, padx=10, pady=5)
upload.configure(background='#364156', foreground='white', font=('calibri', 10, 'bold'))
upload.pack(side=TOP, pady=50)

top.mainloop()

Contributing
If you want to contribute to this project, please follow these steps:

Fork the repository.
Create a new branch (git checkout -b feature-branch).
Make your changes.
Commit your changes (git commit -am 'Add new feature').
Push to the branch (git push origin feature-branch).
Create a new Pull Request.
```

## Acknowledgments
This project uses the OpenCV library for image processing.
The GUI components are handled using Tkinter.
Special thanks to the contributors of these libraries.




