---
title: Visualising Results
template: article.jade
---

# Visualising Results

DeepDIVA outputs all of it's logs to text files that are in the experiment folder, but in addition to that produces visualisations that can be viewed using Tensorboard. All visualisations produced by DeepDIVA are logged and viewable using [Tensorboard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard). To view these results, activate an instance of Tensorboard in the following manner.

- Activate the DeepDIVA environment using  

``` shell
source activate deepdiva
```

- Run the following command to use Tensorboard

``` shell
tensorboard --logdir /path/to/output-folder --port <port_number>
```

Tensorboard will then provide a link to view the visualisations in your browser. If you are running DeepDIVA on your local machine, the website can be accessed by going to the link http://localhost:<port_number>

## Example

Assume you ran an image classification experiment on your local machine (see [Perform Image Classification](./use_image_classification.md)) and produced logs in the output folder `/home/totallynotarobot/my_output/`. To activate Tensorboard and view logs, you would:

- Change directory to /home/totallynotarobot/my_output/
- Then run the following command:

``` shell
tensorboard --logdir ./ --port 6006
```

Then all the visualisations produced by Tensorboard would be visible on ```http://localhost:6006```.

### Note

Usually, it is helpful to have a persistent Tensorboard session running. This can be accomplished using [screen](https://wiki.archlinux.org/index.php/GNU_Screen) or [tmux](https://wiki.archlinux.org/index.php/tmux).
