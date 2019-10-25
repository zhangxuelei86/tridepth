# TriDepth: Triangular Patch-based Deep Depth Prediction
This is an official implementation of "TriDepth: Triangular Patch-based Deep Depth Prediction" presented in [ICCV Deep Learning for Visual SLAM Workshop 2019 (oral)](http://www.visualslam.ai/). 
See the [project webpage](https://meshdepth.github.io/) and [demo video]() for more details.

<p align="center">
    <a href='https://photos.google.com/share/AF1QipNDv6-BDq_1_pa_owRaY0K8V5vJKG9H_Bjtu7G8yRyg2y0PwZ_hS3C673WvLuWNlw?key=eWxQRE5lMElQNWw5UVRJNTFOeDVmMzNXcmJJWGJ3&source=ctrlq.org'><img src='https://lh3.googleusercontent.com/vcz2NPLvqB75SEOEcEL_2CyJ0dqW7DQdd31hP0ZDQejW_zBZFCNV0sx8bRhvI_5LKj0Y_XUC4GW4FIsqKrKsV5vDMK5vBbhNeqxmKlz2C7MnKU1Iu8f7MUQiuMdO_bx0NDVHZYr3EQ=w2400' width=80%/></a>
</p>


## Environments

This code was developed and tested with Python 3.6.8 and PyTorch 1.1.0 on Ubuntu 16.04 LTS machine (CUDA 10.1).

* [Triangle](https://github.com/drufat/triangle) (for 2D mesh extraction)
* [Autotrace](https://github.com/autotrace/autotrace) (for edge vectorization)
* python3-tk
* Some pip libraries (in requirements.txt)

``` 
# Clone this repository with submodule
git clone --recursive git@gitlab.com:syinari0123/tridepth.git # This repository

# Install Triangle (version==20190115.3)
cd thirdparty/triangle
python setup.py install

# Install Autotrace
sudo apt-get install autotrace

# Some libraries
sudo apt-get install python3-tk
pip install -r requirements.txt
```
* [torch-scatter](https://github.com/rusty1s/pytorch_scatter)==1.3.1

``` 
python -c "import torch; print(torch.__version__)"
>>> 1.1.0

echo $PATH
>>> /usr/local/cuda/bin:...

echo $CPATH
>>> /usr/local/cuda/include:...

# Then run:
pip install torch-scatter==1.3.1
```
* [PyMesh](https://pymesh.readthedocs.io/en/latest/index.html)==0.2.1


## Installation

Some parts of this code use CUDA implementation from [Neural Mesh Renderer](https://github.com/daniilidis-group/neural_renderer) (PyTorch version by Nikos Kolotouros).
So you need to build it with following commands.

``` 
cd tridepth/renderer/cuda/
python setup.py install
```

## Dataset

Download the preprocessed [NYU Depth v2](http://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html) dataset in HDF5 formats, (which are distributed in [Sparse-to-Dense](https://github.com/fangchangma/sparse-to-dense.pytorch) by Fangchang et al.) 
The NYU dataset requires 32G of storage space.

```
mkdir data && cd data
wget http://datasets.lids.mit.edu/sparse-to-dense/data/nyudepthv2.tar.gz
tar -xvf nyudepthv2.tar.gz && rm -f nyudepthv2.tar.gz
```

## Training & evaluation

```
CUDA_VISIBLE_DEVICES=$EMPTY_GPU python train.py \
    --theme "train" \
    --log-path "log" \
    --data-path $DATA_DIR \
    --model-type "upconv" \
    --lr 1e-3 \
    --momentum 0.9 \
    --weight-decay 1e-4 \
    --batchsize 8 \
    --workers 4 \
    --nepoch 30 \
    --print-freq 100 \
    --img-print-freq 1000 \
    --print-progress "true"
```
DATA_DIR is the dataset path (Default: data/nyudepthv2).


## Inference

```
CUDA_VISIBLE_DEVICES=$EMPTY_GPU python inference_nyu.py \
    --data-path $DATA_DIR \
    --model-type "upconv" \
    --pretrained-path $MODEL_PATH \
    --print-progress "true" \
    --result-path "result"
```
MODEL_PATH is the file path to the pretrained model (Default: pretrained/weight_upconv.pth).


## Comparison with other representations
See [comparison](_) directory.


## Pretrained model
You can download pretrained model('weight_upconv.pth') with the following command.
```
cd pretrained
./download.sh
```


## Citation

If you use our code or method in your work, please cite our paper!

```
@misc{kaneko19tridepth,
  Author = {Masaya Kaneko and Ken Sakurada and Kiyoharu Aizawa},
  Title = {TriDepth: Triangular Patch-based Deep Depth Prediction},
  Booktitle = {ICCV Deep Learning for Visual SLAM Workshop},
  Year = {2019},
}
```