```python
# Importing necessary libraries and modules
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import convolve2d
from skimage import data, color, io
import IPython
import torch
from torchvision import datasets
from skimage.util import montage
import wandb as wb
import imageio as io

# Load an image from a URL using imageio
image = io.imread("https://th.bing.com/th/id/OIP.HLuY60jzx5puuKjbqmWRRwHaEK?pid=ImgDet&rs=1")

# Function to display images using matplotlib
def plot(x):
    """Display an image using matplotlib."""
    fig, ax = plt.subplots()
    im = ax.imshow(x, cmap='gray')
    ax.axis('off')
    fig.set_size_inches(5, 5)
    plt.show()

# Display the loaded image
plot(image)

# Convert the image to grayscale by extracting only the red channel
image = image[:,:,0]

# Convert the image to float type and normalize pixel values to [0, 1]
image = image.astype(float)
image /= 255.0
plot(image)

# Define a filter matrix
a = np.matrix([[1,2,1],[0,0,0],[-1,-2,-1]])
f = a

# Initialize an empty matrix for the convolution result
x2 = np.zeros(image.shape)

# Apply the filter to the image using manual convolution
for i in range(1, image.shape[0]-1):
    for j in range(1, image.shape[1]-1):
        # Manual convolution calculation
        x2[i,j] = ...

# Define a 2D convolution function
def conv2(x, f):
    """Perform 2D convolution on an image with a given filter."""
    x2 = np.zeros(x.shape)
    # [Convolution logic truncated for brevity]
    return x2

# Define other filter matrices
a = np.matrix([[-1,-1,-1],[-1,8,-1],[-1,-1,-1]])
z = conv2(image, a)

# Display original and convolved images
plot(image)
plot(z)

# Generate random filter matrices and apply them
for i in range(9):
    a = 2*np.random.random((3,3))-1
    z = conv2(image, a)
    plot(z)
