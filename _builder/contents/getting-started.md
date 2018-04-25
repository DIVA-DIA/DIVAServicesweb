---
title: Getting Started
template: getting-started.jade
---

In order to get the framework up and running it is only necessary to clone the latest version of the repository:

``` shell
git clone https://github.com/DIVA-DIA/DeepDIVA.git
```

Run the script:

``` shell
bash setup_environment.sh
```

Reload your environment variables from `.bashrc` with: `source ~/.bashrc`

## Test the correctness of the procecdure

To verify the correctness of the procecdure you can run a small experiment. Activate the DeepDIVA python environment: 

``` python
source activate deepdiva
```

Download the MNIST dataset:

``` python
python util/data/get_a_dataset.py mnist --output-folder toy_dataset
```

Train a simple Convolutional Neural Network on the MNIST dataset using the command:

``` python
python template/RunMe.py --output-folder log --dataset-folder toy_dataset/MNIST --lr 0.1 --ignoregit --no-cuda
```