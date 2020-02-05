# Deep SR-ITM: Joint Learning of Super-Resolution and Inverse Tone-Mapping for 4K UHD HDR

ICCV'19 oral


**Tags**: SR, HDR, Video, ITM

[arXiv](https://arxiv.org/abs/1904.11176)

[MATLAB](https://github.com/sooyekim/Deep-SR-ITM)



## Intro

This paper proposes a joint model to directly transform the legacy low resolution (LR) standard dynamic range (SDR) videos into UHD HDR versions. 

For super resolution (SR), the model decomposes the input image into separate base (low freq.) and detail layers (high freq.), allowing the network to focus on restoring details in the detail layer pass. For inverse tone mapping (ITM), i,e., SDR->HDR, the modulation blocks apply image-specific location-variant operations to enhance local contrast. 

The outputs are in YUV BT.2020 format.


## Method

![network](.SR-ITM.assets/model.png)

- Input decomposition: the guided filter is an edge-preserving low pass filter, and we get base layer and detail layer (concatenated with the original image).
- Residual skip modulation blocks: 
    - Modulation (SMF). It gives image-specific features and will be element-wise multiplied by the features in ResModBlocks and ResSkipModBlocks. Similar to Attention methods.
    - Residual blocks. The upper half learns the base features, while the lower half learns detail features. Base features are fed into the lower half as well (thus called Res*Skip*ModBlocks).
- Fusion: base outputs and detail outputs are concatenated together and fed into 10 resblocks. Then do pixel shuffle to get larger images [Output = Shuffled(I)+Bicubic(I)].


## Experiments

- Convs: 3x3x64 kernel
- Trained and tested on YUV channels
- Data: about 60k frames from 10 4k-UHD HDR videos from Youtube (3 videos of which for test). Trained on patches of size 160x160.
- L2 loss, Adam
- Pretrained without modulations, and then fully trained with it.
- Fewer parameters

The results are better than two-stage methods (first SR and then ITM, separately).

![](.SR-ITM.assets/results.png)

## Highlight
- Joint learning of SR and ITM
- Modulation: an attention-like contrast adjusting module
- Learning with high freq and low freq decomposed features.
  
## Appendix
Previous joint learning method (Soo Ye Kim and Munchurl Kim. A Multi-purpose Convolutional Neural Network for Simultaneous Super-Resolution and High Dynamic Range Image Reconstruction. In ACCV'18): simple multi-task learning. 

In contrast, this paper decomposes detail and base features.

![Previous Joint learning, ACCV'18](.SR-ITM.assets/accv.png "ACCV 2018")



---

Last updated on 2019.12.28