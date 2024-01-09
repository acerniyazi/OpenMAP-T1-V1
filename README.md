# OpenMAP-T1
![Figure3](https://github.com/OishiLab/OpenMAP-T1-V1/assets/64403395/d25fbb04-2152-4cad-8409-f3e7a0ae97e8)

[![](http://img.shields.io/badge/DOI-00.0000/00000-B31B1B.svg)](https://www.biorxiv.org/)
[![](https://img.shields.io/badge/under%20review-ISMRM2024-%2300629B%09)](https://www.ismrm.org/)
[![IEEE Xplore](https://img.shields.io/badge/under%20review-Imaging%20Neuroscience-%2300629B%09)](https://janeway.imaging-neuroscience.org/)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1fmfkxxZjChExnl5cHITYkNYgTu3MZ7Ql#scrollTo=xwZxyL5ewVNF)

**OpenMAP-T1: A Rapid Deep-Learning Approach to Parcellate 280 Anatomical Regions to Cover the Whole Brain**<br>
**Author**: Kei Nishimaki, [Kengo Onda](https://researchmap.jp/kengoonda?lang=en), Kumpei Ikuta, [Yuto Uchida](https://researchmap.jp/uchidayuto), [Susumu Mori](https://www.hopkinsmedicine.org/profiles/details/susumu-mori), [Hitoshi Iyatomi](https://iyatomi-lab.info/english-top), [Kenichi Oishi](https://www.hopkinsmedicine.org/profiles/details/kenichi-oishi)<br>

The Russell H. Morgan Department of Radiology and Radiological Science, The Johns Hopkins University School of Medicine, Baltimore, MD, USA <br>
Department of Applied Informatics, Graduate School of Science and Engineering, Hosei University, Tokyo, Japan <br>
The Richman Family Precision Medicine Center of Excellence in Alzheimer's Disease, Johns Hopkins University School of Medicine, Baltimore, MD, USA<br>

**Abstract**: *This study introduces OpenMAP-T1, a deep learning-based method for rapid and accurate whole brain parcellation in T1-weighted brain MRI, aiming to overcome the limitations of conventional normalization-to-atlas-based approaches and multi-atlas label-fusion (MALF) techniques. Brain image parcellation is a fundamental process in neuroscientific and clinical research, enabling detailed analysis of specific cerebral regions. Normalization-to-atlas-based methods have been employed for this task, but they face limitations due to variations in brain morphology, especially in pathological conditions. The MALF teqhniques improved the accuracy of the image parcellation and robustness to variations in brain morphology but at the cost of high computational demand that requires lengthy processing time. OpenMAP-T1 integrates several convolutional neural network models across six phases: preprocessing, cropping, skull stripping, parcellation, hemisphere segmentation, and final merging. This process involves standardizing MRI images, isolating the brain tissue, and parcellating it into 280 anatomical structures that cover the whole brain, including detailed gray and white matter structures, while simplifying the parcellation processes and incorporating robust training to handle various scan types and conditions. The OpenMAP-T1 was tested on eight available open resources, including real-world clinical images, demonstrating robustness across different datasets with variations in scanner types, magnetic field strengths, and image processing techniques like defacing. Compared to existing methods, OpenMAP-T1 significantly reduced the processing time per image from several hours to less than 90 seconds without compromising accuracy. It was particularly effective in handling images with intensity inhomogeneity and varying head positions, conditions commonly seen in clinical settings. The adaptability of OpenMAP-T1 to a wide range of MRI datasets and robustness to various scan conditions highlight its potential as a versatile tool in neuroimaging.*

Paper: Not yet<br>
Submitted for publication in the **2024 ISMRM Annual Meeting** and **Imaging Neuroscience**<br>

# OpenMAP-T1-V1
**OpenMAP-T1-V1 parcellates the whole brain into 280 anatomical regions based on JHU-atlas in 90 (sec/case).**

## Installation Instructions
0. install python and make virtual environment<br>
python3.8 or later is recommended.

1. Clone this repository:
```
git clone https://github.com/OishiLab/OpenMAP-T1-V1.git
```
2. Please install PyTorch compatible with your environment.<br>
https://pytorch.org/

Once you select your environment, the required commands will be displayed.

![image](https://github.com/OishiLab/OpenMAP-T1-V1/assets/64403395/bd9641e3-5933-48c4-9454-1e0b9fc18e96)

If you want to install an older Pytorch environment, you can download it from the link below.<br>
https://pytorch.org/get-started/previous-versions/

4. Go into the repository and install:
```
cd OpenMAP-T1-V1
pip install -r requirements.txt
```

## How to use it
Using OpenMAP-T1 is straightforward. You can use it in any terminal on your linux system. The OpenMAP-T1 command was installed automatically. We provide CPU as well as GPU support. Running on GPU is a lot faster though and should always be preferred. Here is a minimalistic example of how you can use OpenMAP-T1.
```
python3 parcellation.py -i INPUR_DIRNAME -o OUTPUT_DIRNAME -m MODEL_DIRNAME
```
If you want to specify the GPU, please add ```--gpu```.
```
python3 parcellation.py -i INPUR_DIRNAME -o OUTPUT_DIRNAME -m MODEL_DIRNAME --gpu 1
```

## How to download the pretrained model.
You can get the pretrained model from the this link.
[Link of pretrained model](https://forms.office.com/Pages/ResponsePage.aspx?id=OPSkn-axO0eAP4b4rt8N7C3Ld6BZfoRAuE68LMZr0zFUMEhCMzZIR0RHNEpMTDlOVE1OV0tONkUyMy4u)

## Folder
All images you input must be in NifTi format and have a .nii extension.
```
INPUR_DIRNAME/
  ├ A.nii
  ├ B.nii
  ├ *.nii

OUTPUT_DIRNAME/
  ├ A/
  |   ├ A.nii # input image
  |   ├ A_volume.csv # volume information (mm^3)
  |   └ A_280.nii # parcellation map
  └ B/
      ├ B.nii
      ├ B_volume.csv
      └ B_280.nii

MODEL_DIRNAME/
  ├ CNet/CNet.pth
  ├ SSNet/SSNet.pth
  ├ PNet
  |   ├ coronal.pth
  |   ├ sagittal.pth
  |   └ axial.pth
  └ HNet/
      └ HNet.pth
```

## FAQ
* **How much GPU memory do I need to run OpenMAP-T1?** <br>
We ran all our experiments on NVIDIA RTX3090 GPUs with 24 GB memory. For inference you will need less, but since inference in implemented by exploiting the fully convolutional nature of CNNs the amount of memory required depends on your image. Typical image should run with less than 4 GB of GPU memory consumption. If you run into out of memory problems please check the following: 1) Make sure the voxel spacing of your data is correct and 2) Ensure your MRI image only contains the head region.

* **Will you provide the training code as well?** <br>
No. The training code is tightly wound around the data which we cannot make public.

## Supplementary information
![supplementary_level](https://github.com/OishiLab/OpenMAP-T1-V1/assets/64403395/4b2ff22d-8c61-4b47-a141-53daf9b344c8)

The OpenMAP-T1 parcellate the entire brain into five hierarchical structural levels, with the coarsest level comprising eight structures and the finest level comprising 280 structures. Changing these levels is easy using [ROIEditor](https://www.mristudio.org/installation.html).

## Citation
```
@article{nishimaki2024openmap-t1,
  title={OpenMAP-T1: A Rapid Deep-Learning Approach to Parcellate 280 Anatomical Regions to Cover the Whole Brain},
  author={Kei Nishimaki, Kengo Onda, Kumpei Ikuta, Jill Chotiyanonta, Yuto Uchida, Susumu Mori, Hitoshi Iyatomi, Kenichi Oishi},
  journal={~~~~},
  year={2024},
  publisher={~~~~}
}
```
