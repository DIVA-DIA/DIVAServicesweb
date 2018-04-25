---
title: Perform Image Classification
template: article.jade
---

# Perform Image Classification

A very common task in machine learning is to perform image classification.  For a simple scenario it is only necessary to provide the following parameters:

`--dataset-folder` specifies the location of the dataset on the machine

For example:

``` python
python RunMe.py --dataset-folder /path/to/dataset
```

It is very important that the dataset is in the right structure (see [prepare your data](/DeepDIVAweb/articles/link)).

### Troubleshooting:

See [Troubleshooting](/DeepDIVAweb/articles/troubleshooting)

## Visualize Results

Among others (see [full list of visualizations](/DeepDIVAweb/articles/link)), the following visualizations are produced and can be seen on your Tensorboard (see [installing Tensorboard](/DeepDIVAweb/articles/link))

![Loss plot](/DeepDIVAweb/articles/use-image-classification/loss.png)
![Accuracy plot](/DeepDIVAweb/articles/use-image-classification/accuracy.png)
![Confusion Matrix](/DeepDIVAweb/articles/use-image-classification/cm.png)

## Customization Options

There are a lot of elements that can be modified in an experiment.
Many functionalities are already implemented and accessible through a command line parameters.

See [customizing options](/DeepDIVAweb/articles/customizing-experiments) for a brief overview of the most common ones.

For a comprehensive list of the possibilities offered by the framework see the latest version of the [docs](/DeepDIVAweb/articles/link).
