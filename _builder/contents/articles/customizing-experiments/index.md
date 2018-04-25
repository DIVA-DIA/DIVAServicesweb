---
title: Customizing Experiments
template: article.jade
---

# Customization Options

DeepDIVA offers an high degree of customization options.


## Choosing Your Model

It is possible to specify which model to use with the parameter:

``` python
--model-name <model_name>
```

specifies which model to use for training

Some models are already implemented and are available for use. A list of all available models can be viewed with the command line parameter help `-h` , it lists possible values for the `--model-name` parameter. It is also possible to implement your own model (see [implementing your own model](/DeepDIVAweb/articles/implement-your-model)).


## Choosing Your Optimizer

It is possible to specify which optimizer to use with the parameter:

``` python
--optimizer-name <optimizer_name>
```

specifies which optimizer to use for training

All optimizers in [torch.optim](http://pytorch.org/docs/master/optim.html) are available and can be  used. For each of them there is a unique set of extra parameters than can be specified with their appropriate arguments, such as the learning rate with `--lr` or the intensity of the momentum with `--momentum`. However, it is not necessary as there are default values for them.


## Resuming Training

The latest checkpoint and the best performing checkpoint (as determined by the validation score) are saved in the experiment folder. These checkpoints then can be later loaded using the parameter:

``` python
--load-model /path/to/model_checkpoint
```

## Running Multiple Times

You can run the same experiment multiple times to understand the effect of randomness using the parameter:

``` python
--multi-run N
```

where N is the required number of runs.


## Data Balancing

By default, training is performed with data-balancing enabled. We use a weight vector computed with the inverse of the class frequencies to scale the magnitude of weight updates (more information [here](http://pytorch.org/docs/master/nn.html#torch.nn.functional.cross_entropy)). It is possible to disable this feature with:  

``` python
--disable-databalancing
```

disables the data balancing.
