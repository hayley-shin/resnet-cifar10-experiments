# ResNet on CIFAR-10

This project reproduces the CIFAR-10 experiments from *Deep Residual Learning for Image Recognition* (He et al., 2016) and extends the analysis through additional experiments on network depth, batch size, and pooling strategies.

## Overview

The original ResNet paper introduced residual connections to address the degradation problem that arises when increasingly deep neural networks become difficult to optimize.

Using the PyTorch CIFAR implementation by kuangliu as the baseline, I adapted the ResNet architecture to reproduce the CIFAR-10 setup described in the original paper. The number of layers and filters, shortcut connections, optimization settings, learning-rate schedule, global average pooling, and data augmentation were adjusted to follow the paper's experimental setup.

The reproduced ResNet20 model achieved a **test error of 8.29%** on CIFAR-10, compared with **8.75%** reported in the original paper.

## Experiments

### 1. Network Depth

I implemented and trained ResNet20, ResNet32, ResNet44, and ResNet56 to examine how increasing network depth affects optimization and generalization.

| Model | Parameters | Training Error | Test Error |
|---|---:|---:|---:|
| ResNet20 | 269,722 | 0.41% | 8.29% |
| ResNet32 | 464,154 | 0.11% | 7.11% |
| ResNet44 | 658,586 | 0.10% | 7.96% |
| ResNet56 | 853,018 | 0.05% | 7.85% |

Training error consistently decreased as network depth increased, while the lowest test error in these experiments was achieved by ResNet32.

### 2. Batch Size

I compared batch sizes of 64, 128, and 256 to examine the trade-off between training speed and test performance.

| Batch Size | Test Error | Time per Epoch |
|---:|---:|---:|
| 64 | 7.59% | 31.5 sec |
| 128 | 8.29% | 23.6 sec |
| 256 | 8.83% | 9.4 sec |

Larger batch sizes substantially reduced training time per epoch, while smaller batch sizes achieved lower test error under the experimental settings used here.

### 3. Pooling

I also examined the effect of replacing average pooling with convolution. Under this experimental setup, the convolution-based variant failed to converge, with the training loss becoming NaN.

## Implementation

The implementation was adapted from the [PyTorch CIFAR repository by kuangliu](https://github.com/kuangliu/pytorch-cifar) and modified to follow the CIFAR-10 ResNet architecture and training setup described in the original paper.

Key modifications and settings include:

- CIFAR-style ResNet20, ResNet32, ResNet44, and ResNet56 architectures
- Residual shortcut connection **Option A**
- 3×3 convolutional layers with 16, 32, and 64 filters
- SGD with momentum of 0.9
- Weight decay of 0.0001
- Initial learning rate of 0.1 with scheduled reductions
- Global average pooling
- CIFAR-10 data augmentation

## Report

For a detailed description of the implementation and experiments, see the full project report.

## References

- He, K., Zhang, X., Ren, S., & Sun, J. (2016). *Deep Residual Learning for Image Recognition*. CVPR 2016.
- Baseline implementation: [kuangliu/pytorch-cifar](https://github.com/kuangliu/pytorch-cifar)
