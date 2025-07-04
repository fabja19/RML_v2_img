# Radio Map Prediction from Images and Application to Coverage Optimization

This is the official implementation of our experiments on learning radio map estimation from images and, optionally, additional unclassified height maps with CNNs, described in our paper "Radio Map Prediction from Images and Application to Coverage Optimization". Our work build upon previous works on this topic such as RadioUNet or PMNet, and develops the approach further with a more realistic dataset based on 3D city models of real world locations. These include roof structures and tree canopy shapes and heights, obtained from public LiDAR data. The radio map simulations have been conducted with directive antennas on the transmitter side and the dataset also contains aerial imagery for the simulated environments. We show that CNNs can leverage these images to predict the radio maps without access to the 3D geometry of the environment. Furthermore, we showcase how the very fast inference time and the inherent differentiability of the trained models can be leveraged for downstream applications, such as optimizing the orientation of base stations to cover a given area.

If you find this useful and use our code, please cite our paper:

> [Fabian Jaensch, Giuseppe Caire and Begüm Demir, "Radio Map Prediction from Images and Application to Coverage Optimization", IEEE Transactions on Wireless Communications, early access.](https://doi.org/10.1109/TWC.2025.3583171)
```bibtex
@ARTICLE{jaensch25,
  author={Jaensch, Fabian and Caire, Giuseppe and Demir, Begüm},
  journal={IEEE Transactions on Wireless Communications}, 
  title={Radio Map Prediction from Aerial Images and Application to Coverage Optimization}, 
  year={2025},
  volume={},
  number={},
  pages={1-1},
  keywords={Buildings;Three-dimensional displays;Ray tracing;Solid modeling;Wireless communication;Predictive models;Optimization;Urban areas;Adaptation models;Accuracy;Convolutional Neural Networks;Machine Learning;Path loss;Radio map;RSSI;Coverage},
  doi={10.1109/TWC.2025.3583171}}
```


![alt text](sample_img.png "Sample")

Note that this is an updated version of our repo [RML](https://github.com/fabja19/RML) with the focus shifted to the prediction from images and an additional new application to wireless network optimization ([notebook](coverage_optimization.ipynb)). 

## Requirements

The dataset can be downloaded from [here](https://zenodo.org/uploads/10210089) and is expected to be unpacked to the directory *./dataset*.

To install the required packages via conda run:

```
conda env create -f conda_env/env_complete.yml
```

The environment has been used on Linux computers with CUDA 11.8 and A100 GPUs. On different OS/hardware, you may need to use the less restrictive file [conda_env/env_short.yml](conda_env/env_short.yml) or adjust some packages.

## Basic Usage

To replicate the experiments from the paper, run this command:

```
python main_cli.py fit --model=<model name> --config=<path to config>
```

Here, ```<model name>``` can be any of  _LitRadioUNet, LitPMNet_ or _LitUNetDCN_ and the configs for the dataset class corresponding to our experiments can be found in  the directory [configs/data](configs/data). The training procedure will save the results including a model checkpoint, log file, config and Tensorboard log in a subdirectory of [./logs](./logs).

Instead of training from scratch, you can [download](https://zenodo.org/uploads/10210089) the checkpoints and configs for some of the trained models.

Trained models can be evaluated on the test set by running

```
python main_cli.py test --config=<path to config> --ckpt_path=<path to checkpoint> --trainer.logger.sub_dir=test
```

and inference on the test set is possible with:

```
python main_cli.py predict --config=<path to config> --ckpt_path=<path to checkpoint> --trainer.logger.sub_dir=predict
```

## More options

Arguments for the dataset class (inputs for the model) and hyperparameters of the models and the training procedure can be set with flags. To get an overview of all possible commands, run:

```
python main_cli.py fit --help
python main_cli.py fit --model.help <model class>
python main_cli.py fit --data.help LitRM_directional
```

More information can be found in the documentation of [PyTorch Lightning](https://lightning.ai/docs/pytorch/stable/) and in particular the [CLI](https://lightning.ai/docs/pytorch/stable/cli/lightning_cli.html#lightning-cli).

