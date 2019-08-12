# ms_ssim_pytorch

The code was modified from https://github.com/VainF/pytorch-msssim.  
Part of the code has been modified to make it faster, takes up less VRAM, and is compatible with pytorch jit.  

## Great speed up in pytorch 1.2. It is strongly recommended to update to pytorch 1.2 !

# Speed up. Only test on GPU.
losser1 is https://github.com/lizhengwei1992/MS_SSIM_pytorch/blob/master/loss.py 268fc76  
losser2 is https://github.com/Po-Hsun-Su/pytorch-ssim/blob/master/pytorch_ssim/__init__.py 881d210  
losser3 is https://github.com/VainF/pytorch-msssim/blob/master/pytorch_msssim/ssim.py b47c07c  
losser4 is https://github.com/One-sixth/ms_ssim_pytorch/blob/master/ssim.py 75233cf  

My test environment: i7-6700HQ GTX970M-3G  

## SSIM
Test output  

pytorch 1.2  
```
Performance Testing SSIM

testing losser2
cuda time 88978.5390625
perf_counter time 86.7912251

testing losser3
cuda time 36165.6640625
perf_counter time 36.103866

testing losser4
cuda time 31052.384765625
perf_counter time 30.134942200000012

```

pytorch 1.1  
```
Performance Testing SSIM

testing losser2
cuda time 88976.3203125
perf_counter time 86.79177510000001

testing losser3
cuda time 36157.3203125
perf_counter time 36.096230100000014

testing losser4
cuda time 35632.9140625
perf_counter time 34.966091500000005

```

## MS-SSIM
Test output  

pytorch 1.2  
```
Performance Testing MS_SSIM

testing losser1
cuda time 134059.5
perf_counter time 133.94344470000001

testing losser3
cuda time 63151.3203125
perf_counter time 63.109972

testing losser4
cuda time 49190.26171875
perf_counter time 49.15350409999999

```

pytorch 1.1  
```
Performance Testing MS_SSIM

testing losser1
cuda time 134462.515625
perf_counter time 134.3518838

testing losser3
cuda time 62653.0546875
perf_counter time 62.61409040000001

testing losser4
cuda time 55489.34375
perf_counter time 55.450284599999975

```

## Test speed by yourself
1. cd ms_ssim_pytorch/_test_speed  

2. python test_ssim_speed.py  
or  
2. python test_ms_ssim_speed.py  

# Other thing
Add parameter use_padding.  
When set to True, the gaussian_filter behavior is the same as https://github.com/Po-Hsun-Su/pytorch-ssim.  
This parameter is mainly used for MS-SSIM, because MS-SSIM needs to be downsampled.  
When the input image is smaller than 176x176, this parameter needs to be set to True to ensure that MS-SSIM works normally. (when parameter weight and level are the default)  

# Require
Pytorch >= 1.1  

if you want to test the code with animation. You also need to install some package.  
```
pip install imageio imageio-ffmpeg opencv-python
```

# Test code with animation
The test code is included in the ssim.py file, you can run the file directly to start the test.  

1. git clone https://github.com/One-sixth/ms_ssim_pytorch  
2. cd ms_ssim_pytorch  
3. python ssim.py  

# Code Example.
```python
import torch
import ssim


im = torch.randint(0, 255, (5, 3, 256, 256), dtype=torch.float, device='cuda')
img1 = im / 255
img2 = img1 * 0.5

losser = ssim.SSIM(data_range=1.).cuda()
loss = losser(img1, img2).mean()

losser2 = ssim.MS_SSIM(data_range=1.).cuda()
loss2 = losser2(img1, img2).mean()

print(loss.item())
print(loss2.item())
```

# Animation
GIF is a bit big. Loading may take some time.  
Or you can download the mkv video file directly to view it, smaller and smoother.  
https://github.com/One-sixth/ms_ssim_pytorch/blob/master/ssim_test.mkv  
https://github.com/One-sixth/ms_ssim_pytorch/blob/master/ms_ssim_test.mkv  

SSIM  
![ssim](https://github.com/One-sixth/ms_ssim_pytorch/blob/master/ssim_test.gif)

MS-SSIM  
![ms-ssim](https://github.com/One-sixth/ms_ssim_pytorch/blob/master/ms_ssim_test.gif)

# References
https://github.com/VainF/pytorch-msssim  
https://github.com/Po-Hsun-Su/pytorch-ssim  
https://github.com/lizhengwei1992/MS_SSIM_pytorch  
