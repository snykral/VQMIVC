## VQMIVC: Vector Quantization and Mutual Information-Based Unsupervised Speech Representation Disentanglement for One-shot Voice Conversion (Interspeech 2021)
[![arXiv](https://img.shields.io/badge/arXiv-Paper-<COLOR>.svg)](https://arxiv.org/abs/2106.10132)
[![GitHub Stars](https://img.shields.io/github/stars/Wendison/VQMIVC?style=social)](https://github.com/Wendison/VQMIVC)
[![download](https://img.shields.io/github/downloads/Wendison/VQMIVC/total.svg)](https://github.com/Wendison/VQMIVC/releases)

### [Run VQMIVC on Replicate](https://replicate.ai/wendison/vqmivc)
### Integrated to [Huggingface Spaces](https://huggingface.co/spaces) with [Gradio](https://github.com/gradio-app/gradio). See [Gradio Web Demo](https://huggingface.co/spaces/akhaliq/VQMIVC).


This paper proposes a speech representation disentanglement framework for *one-shot/any-to-any* voice conversion, which performs conversion across arbitrary speakers with only a single target-speaker utterance for reference. Vector quantization with contrastive predictive coding (VQCPC) is used for content encoding and mutual information (MI) is introduced as the correlation metric during training, to achieve proper disentanglement of content, speaker and pitch representations, by reducing their inter-dependencies in an *unsupervised* manner. 

<p align="center">
	<img src='./diagram/diagram.png' width=1000 >
</p>

## 📢 Update
Many thanks to [ericguizzo](https://github.com/ericguizzo) & [AK391](https://github.com/AK391)! 
1. A [Replicate demo](https://replicate.ai/wendison/vqmivc) is provided online, so you can play our pre-trained models there, have fun! 
2. VQMIVC can be trained and tested inside a Docker environment via [Cog](https://github.com/replicate/cog) now.
3. [Gradio Web Demo](https://huggingface.co/spaces/akhaliq/VQMIVC) is available, another online demo!


## Requirements
Python 3.6 is used, install [apex](https://github.com/NVIDIA/apex) for speeding up training (optional), other requirements are listed in 'requirements.txt':

	pip install -r requirements.txt


## Quick start with pre-trained models
Don't use the Parallel WaveGan from it's github, instead: 

		pip install parallel_wavegan
to download the checkpoints [pre-trained models](https://drive.google.com/file/d/1Flw6Z0K2QdRrTn5F-gVt6HdR9TRPiaKy/view?usp=sharing):
Then, make a conversion

	python convert_example.py -s {source-wav} -r {reference-wav} -c {converted-wavs-save-path} -m {model-path} 
	
For example:

	python convert_example.py -s test_wavs/p225_038.wav -r test_wavs/p334_047.wav -c converted -m checkpoints/useCSMITrue_useCPMITrue_usePSMITrue_useAmpTrue/VQMIVC-model.ckpt-500.pt 
	
The converted wav is put in 'converted' directory.
	

## Training and inference:
*  Step1. Data preparation & preprocessing.
1. Put the dataset under directory: 'Dataset/'
2. Training/testing speakers split & feature (mel+lf0) extraction:
   Here, a new code pre.py was added in order to replace preprocess.py. The changes are due to incompatiblity with the installed hydra from the requirements and the code, and the way the wavs are globbed from the dataset. You may still need to adapt the glob logic in order to adapt to your dataset.

		python pre.py

*  Step2. model training:
   Training used the same logic as in the original repo.

	
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

