# UNet-Segmentation

## Data

All BraTS multimodal scans are available as NIfTI files (.nii.gz) and describe a) native (T1) and b) post-contrast T1-weighted (T1Gd), c) T2-weighted (T2), and d) T2 Fluid Attenuated Inversion Recovery (T2-FLAIR) volumes, and were acquired with different clinical protocols and various scanners from multiple (n=19) institutions, mentioned as data contributors here.

All the imaging datasets have been segmented manually, by one to four raters, following the same annotation protocol, and their annotations were approved by experienced neuro-radiologists. Annotations comprise the GD-enhancing tumor (ET — label 4), the peritumoral edema (ED — label 2), and the necrotic and non-enhancing tumor core (NCR/NET — label 1), as described both in the BraTS 2012-2013 TMI paper and in the latest BraTS summarizing paper. The provided data are distributed after their pre-processing, i.e., co-registered to the same anatomical template, interpolated to the same resolution (1 mm^3) and skull-stripped.
<center><img width="70%" src="figures\1.png"></center>

## <a id='toc1_3_3_'></a>[U-Net Model](#toc0_)
| **Stage**           | **Operation**                 | **Purpose**                              |
|---------------------|-------------------------------|------------------------------------------|
| Input               | Image tile                    | Input for segmentation                   |
| Contracting Path    | Conv 3×3, ReLU + MaxPool      | Feature extraction and downsampling      |
| Bottleneck          | Deepest layer                 | Global context representation            |
| Expanding Path      | Up-conv + Skip connections    | Resolution recovery and spatial merging  |
| Output Layer        | 1×1 convolution               | Pixel-wise classification                |
<center>
<img width="70%" src="figures\2.png">
<img width="70%" src="figures\3.png">
<img width="70%" src="figures\4.png">
<img width="70%" src="figures\5.png">
</center>

## UNet with attention

Now we augment the regular UNet with several sub-modules and techniques such as ResNets, Attention modules, etc. We describe the expected architecture for this notebook:

+ *ResNet:* As a base sub-module, we define each ResNet block as two consecutive convolutional layers with a GELU activation in between and Group Normalization after each convolutional layer. We can use this module in Down/Up blocks. Also, by removing the residual connection, you can use this block as a convolutional network throughout the network.
+ *Attention:* This is also a sub-module consisting of a Layer Normalization, Multi-head Attention (use from `torch.nn`), a residual connection, a feed-forward network, and another residual connection.

<center>
<img width="70%" src="figures\6.png">
<img width="70%" src="figures\7.png">
</center>