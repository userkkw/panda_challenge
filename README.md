# Goal
We want to use [sthalles/SimCLR](https://github.com/sthalles/SimCLR/)  to train on our own dataset from Panda challange. The main problem is to load our dataset in this file: data_aug/datawrapper.py instead of SIL10 in the original repository.

## Dataset
The original dataset is from [Panda Challenge on Kaggle](https://www.kaggle.com/c/prostate-cancer-grade-assessment/data). There are ~10000 WSI from two providers: Redboud and Karolinska. They are labeled as following:

Radboud: Prostate glands are individually labelled. Valid values are:

0: background (non tissue) or unknown

1: stroma (connective tissue, non-epithelium tissue)

2: healthy (benign) epithelium

3: cancerous epithelium (Gleason 3)

4: cancerous epithelium (Gleason 4)

5: cancerous epithelium (Gleason 5)

Karolinska: Regions are labelled. Valid values are:

0: background (non tissue) or unknown

1: benign tissue (stroma and epithelium combined)

2: cancerous tissue (stroma and epithelium combined)


Since the Redboud has the detailed labels (grade from 0 to 5), we would like to pretrain SimCLR on the WSI from Reboud. 

# Patches
Ali has cut the Redboud WSI into 512x512 jpg images and filtered out the background patches. They are saved under the subfolders. The name of the subfolder is the WSI name. 

For Example: **a51ad4ec379a9209c740025a6d410708** is the folder I used in this repository. This folder contains 31 patches and 31 corresponding masks.

# Labels
In [Creating_Patch_Labels_Redboud.ipynb](https://github.com/userkkw/panda_challenge/blob/master/Creating_Patch_Labels_Redboud.ipynb) I have created a .csv file containing patch name and its label. 

The highest grade contained in the mask determines the label of the corresponding patch. 

# datawrapper_for_panda_dataset.ipynb
sthalles/SimCLR/data_aug/datawrapper.py is where the original dataset is loaded. To import our patches with labels, it requires this [datasets.ImageFolder](https://pytorch.org/docs/stable/torchvision/datasets.html#imagefolder) command. However it requires the folders are arranged by classes.  So I create a symbol link (see cell 4) to connect the current directory to a temp root that datasets.ImageFolder requires. 



**With this modification, we can now use the SimCLR to train on our dataset.**

### data_loader.ipynb
Originally I am creating the my own class of the dataset according to this post: [WRITING CUSTOM DATASETS, DATALOADERS AND TRANSFORMS](https://pytorch.org/tutorials/beginner/data_loading_tutorial.html). However, it requires me to write my own torchvision.tranform functions due to the data format of the image while defining the dataset. 
