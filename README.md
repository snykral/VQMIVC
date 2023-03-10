## VQMIVC Forked Repo, Adapting to other Dataset CHANGES WERE NOT COMMITED YET, DO NOT USE, AS IT WON'T WORK
### Info From the original Authors:
[![arXiv](https://img.shields.io/badge/arXiv-Paper-<COLOR>.svg)](https://arxiv.org/abs/2106.10132)
[![GitHub Stars](https://img.shields.io/github/stars/Wendison/VQMIVC?style=social)](https://github.com/Wendison/VQMIVC)
[![download](https://img.shields.io/github/downloads/Wendison/VQMIVC/total.svg)](https://github.com/Wendison/VQMIVC/releases)



*one-shot/any-to-any* voice conversion, which performs conversion across arbitrary speakers with only a single target-speaker utterance for reference. Vector quantization with contrastive predictive coding (VQCPC) is used for content encoding and mutual information (MI) is introduced as the correlation metric during training, to achieve proper disentanglement of content, speaker and pitch representations, by reducing their inter-dependencies in an *unsupervised* manner. 

<p align="center">
	<img src='./diagram/diagram.png' width=1000 >
</p>



## Requirements
Python 3.6 is used, install [apex](https://github.com/NVIDIA/apex) for speeding up training (optional), other requirements are listed in 'requirements.txt':

	pip install -r requirements.txt


## Quick start with pre-trained models
Don't use the Parallel WaveGan from it's github, instead: 

	pip install parallel_wavegan
		
Download the checkpoints from VQMIVC [pre-trained models](https://drive.google.com/file/d/1Flw6Z0K2QdRrTn5F-gVt6HdR9TRPiaKy/view?usp=sharing):

Then, make a conversion

	python convert_example.py -s {source-wav} -r {reference-wav} -c {converted-wavs-save-path} -m {model-path} 
	
For example:

	python convert.py
	
The converted wav is put in 'converted' directory.
	

## Training:
*  Step1. Data preparation & preprocessing.
1. Put the dataset under directory: 'Dataset/'
2. Training/testing speakers split & feature (mel+lf0) extraction:

Here, a new code pre.py was added in order to replace preprocess.py. There was an error that due to the dataset size, numpy arrays couldn't be loaded into RAM, so lines from 141 to 145 were modified in order to work with only a portion of the wavs; they are about calculating the mean and std of the mels spectrograms in order to normalize the data. Also, the wavs are globbed from the dataset was changed. You may still need to adapt the glob logic in order to adapt to your dataset.

	python pre.py

*  Step2. model training:

	python train.py use_CSMI=True use_CPMI=True use_PSMI=True

Training was adapted to fine tune from the VCTK checkpoint, so download the checkpoint from the original paper and then change the checkpoint path at config/convert.yaml. Also, full paths are used in this training, so you will need to change the paths at config/train.yaml too.

## Hydra Problems Found:
* 2 problems were find while trying to use convert.py, so the made changes are:
1. Line 70 from the original code
	@hydra.main(config_path="config/convert.yaml")
   was changed to
   	@hydra.main(config_path="config", config_name='convert')
2. Somehow, one of the packages are making the code lose the tracking of it's own path, so full paths are used instead of relative paths.


## Citation From the Original Authors
If the code is used in your research, please <a class="github-button" href="https://github.com/wendison/VQMIVC" data-icon="octicon-star" aria-label="Star wendison/VQMIVC on GitHub">Star</a> our repo and cite our paper:
```
@inproceedings{wang21n_interspeech,
  author={Disong Wang and Liqun Deng and Yu Ting Yeung and Xiao Chen and Xunying Liu and Helen Meng},
  title={{VQMIVC: Vector Quantization and Mutual Information-Based Unsupervised Speech Representation Disentanglement for One-Shot Voice Conversion}},
  year=2021,
  booktitle={Proc. Interspeech 2021},
  pages={1344--1348},
  doi={10.21437/Interspeech.2021-283}
}
```

