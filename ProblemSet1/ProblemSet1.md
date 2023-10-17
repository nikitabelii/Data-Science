# Load MNIST

## Part 1.a Load MNIST and show montage

```python
import numpy as np
import matplotlib.pyplot as plt
import torch
from torchvision import datasets
from skimage.util import montage
from skimage.io import imread

def GPU(data):
    return torch.tensor(data, requires_grad=True, dtype=torch.float, device=torch.device('cuda'))
    
def GPU_data(data):
    return torch.tensor(data, requires_grad=False, dtype=torch.float, device=torch.device('cuda'))

def plot(x):
    if type(x) == torch.Tensor:
        x = x.cpu().detach().numpy()

    fig, ax = plt.subplots()
    im = ax.imshow(x, cmap='gray')
    ax.axis('off')
    fig.set_size_inches(7, 7)
    plt.show()

def montage_plot(x):
    x = np.pad(x, pad_width=((0, 0), (1, 1), (1, 1)), mode='constant', constant_values=0)
    plot(montage(x))

# MNIST
train_set = datasets.MNIST('./data', train=True, download=True)
test_set = datasets.MNIST('./data', train=False, download=True)

X = train_set.data.numpy()
X_test = test_set.data.numpy()
Y = train_set.targets.numpy()
Y_test = test_set.targets.numpy()

X = X[:, None, :, :] / 255
X_test = X_test[:, None, :, :] / 255
X.shape
montage_plot(X[125:150, 0, :, :])

# Run random y=mx model on MNIST
X = X.reshape(X.shape[0], 784)  # Reshape image data tensor from (60000, 1, 28, 28) to (60000, 784)
X_test = X_test.reshape(X_test.shape[0], 784)  # Reshape image test data tensor from (60000, 1, 28, 28) to (60000, 784)
X.shape

X = GPU_data(X)
Y = GPU_data(Y)
X_test = GPU_data(X_test)
Y_test = GPU_data(Y_test)

X.shape

X = X.T

X.shape

x = X[:, 0:1]
x.shape
M = GPU(np.random.rand(10, 784))
y = M @ x
batch_size = 64

x = X[:, 0:batch_size]

M = GPU(np.random.rand(10, 784))

y = M @ x

y = torch.argmax(y, 0)

torch.sum((y == Y[0:batch_size])) / batch_size

m_best = 0
acc_best = 0

for i in range(100000):
    step = 0.0000000001

    m_random = GPU_data(np.random.randn(10, 784))

    m = m_best + step * m_random

    y = m @ X

    y = torch.argmax(y, axis=0)

    acc = ((y == Y)).sum() / len(Y)

    if acc > acc_best:
        print(acc.item())
        m_best = m
        acc_best = acc
