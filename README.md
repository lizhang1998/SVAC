# SVAC
## Abstract
Referring Video Object Segmentation (RVOS) aims to segment target objects in video sequences based on natural language descriptions. While recent advances in Multi-modal Large Language Models (MLLMs) have improved RVOS performance through enhanced text-video understanding, several challenges remain, including insufficient exploitation of MLLMs’ prior knowledge, prohibitive computational and memory costs for long-duration videos, and inadequate handling of complex temporal dynamics. In this work, we propose **SVAC**, a unified model that improves RVOS by scaling up input frames and segmentation tokens to enhance video-language interaction and segmentation precision. To address the resulting computational challenges, **SVAC** incorporates the **Anchor-Based Spatio-Temporal Compression (ASTC)** module to compress visual tokens while preserving essential spatio-temporal structure. Moreover, the **Clip-Specific Allocation (CSA)** strategy is introduced to better handle dynamic object behaviors across video clips. Experimental results demonstrate that **SVAC** achieves state-of-the-art performance on multiple RVOS benchmarks with competitive efficiency.

## Guideline
### 1. Base Model Set Up
Please download Sa2VA model following [here](https://github.com/magic-research/Sa2VA).
### 2. Anchor-Based Spatio-Temporal Compression (ASTC)
We need to introduce ASTC in the dataset files.  

Take `ReVOS_Dataset.py` for example:  
1. Update `dataset_map_fn` to accept the desired number of frames for compression.  
2. In `__getitem__`, do the following:  
   - Concatenate the selected frames.  
   - Apply bicubic interpolation to resize the concatenated frame to the original spatial dimensions of a single frame.  

### 3. Clip-Specific Allocation (CSA)
Take `ReVOS_Dataset.py` as an example for dataset modifications:  
- Update `ANSWER_LIST` to include multiple `[SEG]` tokens corresponding to different clips, e.g., `"It is [SEG][SEG][SEG]."`  

For the main model file `llava_sam2.py`:  
- Modify the loss calculation logic so that each `[SEG]` token handles its corresponding video clip individually.  


