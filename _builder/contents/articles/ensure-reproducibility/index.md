---
title: Ensuring Reproducibility
template: article.jade
---

# Reproducibility

DeepDIVA makes it easy to reproduce an experiment. This is done by ensuring that:
 - there is always a copy of the code used to run the experiment,
 - all the command line parameters and arguments are known and,
 - all the pseudo-random generators are seeded.

Here are the some of the most common scenarios for reproducibility.

## Reproduce your experiment for a paper

To make the source code easily available:
- Check your experiment into Github.
  - Fork DeepDIVA or clone and push to a different repository.
  - Make necessary modifications to DeepDIVA (for e.g., using different data augmentation, additional models or datasets, etc)
  - Commit your changes and push to Github.
- Specify a seed using the parameter `--seed <number>`
- Run your experiment __without__ the `--ignoregit` flag.

Along with the data that was used to run the experiment, the experiment folder contains all the necessary information to reproduce the exact experiment:  
- The `logs.txt` in the experiment folder contain information about the
  - Github repo,
  - branch,
  - commit hash and
  - the seed used with the experiment.  
- The `args.txt` contains all the command line arguments that were used with the specific source code.


### Example

Let's make a Github reproducible experiment using DeepDIVA.

- First, we need to make a copy of DeepDIVA that can be edited. This can be done by forking DeepDIVA directly on Github (see [here](https://help.github.com/articles/fork-a-repo/)), or you can clone DeepDIVA and push it to a new repository that you make (see [here](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)).
- To verify that everything works, use the experiment outlined in [Getting Started](../basic/getting_started.md)
- Make a change in the code by modifying the [PyTorch data transformations](http://pytorch.org/docs/master/torchvision/transforms.html) that are used.
  - This can be done by editing the function `set_up_dataloaders` in the file `DeepDIVA/template/setup.py`.
  - Change all instances of `transforms.Resize(model_expected_input_size)`, to `transforms.CenterCrop(model_expected_input_size)`
- Add the change, commit and push to Github
- Now you can re-run the experiment, and your Github repo, branch and commit information will be stored in the logs.

## Reproduce an old experiment

While DeepDIVA can be helpful for experiment reproducibility, it nonetheless introduces a certain degree of overhead that is not desirable during development. In this scenario, one can suppress the requirement to have Github commits using the command `--ignoregit`. In this situation, a copy of the code used to run an experiment is made and stored in the experiment folder inside a tarfile (`DeepDIVA.tar.gz`).

As in the previous scenario, all arguments are still stored in `args.txt`.
If a seed is not manually specified, a random number is chosen, logged and used to seed all the pseudo-random generators.  
