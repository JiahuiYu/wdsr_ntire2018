# Wide Activation for Efficient and Accurate Image Super-Resolution

[Tech Report](https://arxiv.org/abs/1808.08718) | [Approach](#wdsr-network-architecture) | [Results](#overall-performance) | [TensorFlow](https://github.com/ychfan/tf_estimator_barebone/blob/master/docs/super_resolution.md) | [Other Implementations](#other-implementations) | [Bibtex](#citing) 

**Update (Apr, 2020)**: We have released a [reloaded version](https://github.com/ychfan/wdsr) with full training scripts in PyTorch and pre-trained models.

**Update (Oct, 2018)**: We have re-implemented [WDSR on TensorFlow](https://github.com/ychfan/tf_estimator_barebone/blob/master/docs/super_resolution.md) for end-to-end training and testing. Pre-trained models are released. The runtime speed of [weight normalization on tensorflow](https://github.com/ychfan/tf_estimator_barebone/blob/master/common/layers.py) is also optimized.

## Run

0. Requirements:
    * Install [PyTorch](https://pytorch.org/) (tested on release 0.4.0 and 0.4.1).
    * Clone [EDSR-Pytorch](https://github.com/thstkdgus35/EDSR-PyTorch/tree/95f0571aa74ddf9dd01ff093081916d6f17d53f9) as backbone training framework.
1. Training and Validation:
    * Copy [wdsr_a.py](/wdsr_a.py), [wdsr_b.py](/wdsr_b.py) into `EDSR-PyTorch/src/model/`.
    * Modify `EDSR-PyTorch/src/option.py` and `EDSR-PyTorch/src/demo.sh` to support `--n_feats, --block_feats, --[r,g,b]_mean` option (please find reference in issue [#7](https://github.com/JiahuiYu/wdsr_ntire2018/issues/7), [#8](https://github.com/JiahuiYu/wdsr_ntire2018/issues/8)).
    * Launch training with [EDSR-Pytorch](https://github.com/thstkdgus35/EDSR-PyTorch/tree/95f0571aa74ddf9dd01ff093081916d6f17d53f9) as backbone training framework.
2. Still have questions?
    * If you still have questions, please first search over closed issues. If the problem is not solved, please open a new issue.

## Overall Performance

| Network | Parameters | DIV2K (val) PSNR |
| - | - | - |
| EDSR Baseline | 1,372,318 | 34.61 |
| WDSR Baseline | **1,190,100** | **34.77** |

We measured PSNR using DIV2K 0801 ~ 0900 (trained on 0000 ~ 0800) on RGB channels without self-ensemble. Both baseline models have 16 residual blocks.

More results:

<table><tr><th>Number of Residual Blocks</th><th colspan="3">1</th><th colspan="3">3</th></tr><tr><td>SR Network</td><td>EDSR</td><td>WDSR-A</td><td>WDSR-B</td><td>EDSR</td><td>WDSR-A</td><td>WDSR-B</td></tr><tr><td>Parameters</td><td>0.26M</td><td><strong>0.08M</strong></td><td><strong>0.08M</strong></td><td>0.41M</td><td><strong>0.23M</strong></td><td><strong>0.23M</strong></td></tr><tr><td>DIV2K (val) PSNR</td><td>33.210</td><td><strong>33.323</strong></td><td><strong>33.434</strong></td><td>34.043</td><td><strong>34.163</strong></td><td><strong>34.205</strong></td></tr></table>

<table><tr><th>Number of Residual Blocks</th><th colspan="3">5</th><th colspan="3">8</th></tr><tr><td>SR Network</td><td>EDSR</td><td>WDSR-A</td><td>WDSR-B</td><td>EDSR</td><td>WDSR-A</td><td>WDSR-B</td></tr><tr><td>Parameters</td><td>0.56M</td><td><strong>0.37M</strong></td><td><strong>0.37M</strong></td><td>0.78M</td><td><strong>0.60M</strong></td><td><strong>0.60M</strong></td></tr><tr><td>DIV2K (val) PSNR</td><td>34.284</td><td><strong>34.388</strong></td><td><strong>34.409</strong></td><td>34.457</td><td><strong>34.541</strong></td><td><strong>34.536</strong></td></tr></table>

Comparisons of <a href="https://arxiv.org/abs/1707.02921">EDSR</a> and our proposed WDSR-A, WDSR-B using identical settings to [EDSR baseline model](https://github.com/thstkdgus35/EDSR-PyTorch) for image bicubic x2 super-resolution on DIV2K dataset.

## WDSR Network Architecture

<img src="https://user-images.githubusercontent.com/22609465/41505074-52078984-71b5-11e8-87af-15f19b43c450.png"  width=100%/>

Left: vanilla residual block in EDSR. Middle: **wide activation**. Right: **wider activation with linear low-rank convolution**. The proposed wide activation WDSR-A, WDSR-B have similar merits with [MobileNet V2](https://arxiv.org/abs/1801.04381) but different architectures and much better PSNR.

## Weight Normalization vs. Batch Normalization and No Normalization

<img src="https://user-images.githubusercontent.com/22609465/41505052-be6ac920-71b4-11e8-8433-e6736364a29e.png"  width=48%/> <img src="https://user-images.githubusercontent.com/22609465/41505053-be911a8a-71b4-11e8-9da4-b34a7ac598f4.png"   width=48%/>

Training loss and validation PSNR with weight normalization, batch normalization or no normalization. Training with weight normalization has faster convergence and better accuracy.

## Other Implementations

- [TensorFlow-WDSR](https://github.com/ychfan/tf_estimator_barebone/blob/master/docs/super_resolution.md) (official) 
- [Keras-WDSR](https://github.com/krasserm/wdsr) By [Martin Krasser](https://github.com/krasserm)


## Citing
Please consider cite WDSR for image super-resolution and compression if you find it helpful.
```
@article{yu2018wide,
  title={Wide Activation for Efficient and Accurate Image Super-Resolution},
  author={Yu, Jiahui and Fan, Yuchen and Yang, Jianchao and Xu, Ning and Wang, Xinchao and Huang, Thomas S},
  journal={arXiv preprint arXiv:1808.08718},
  year={2018}
}

@inproceedings{fan2018wide,
  title={Wide-activated Deep Residual Networks based Restoration for BPG-compressed Images},
  author={Fan, Yuchen and Yu, Jiahui and Huang, Thomas S},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition Workshops},
  pages={2621--2624},
  year={2018}
}
```
