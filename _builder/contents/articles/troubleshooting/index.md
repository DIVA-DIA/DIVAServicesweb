---
title: Troubleshooting
template: article.jade
---

# Troubleshooting:

If you do not have a GPU on your system, or your GPU drivers are out of date and incompatible with the latest version of PyTorch, you can switch to CPU computation with:

``` python
--no-cuda
```

moves all computation from GPU to CPU

If you have made changes to the code, or otherwise have data inside the DeepDIVA folder such that it is not a clean working environement, you can choose to ignore the git status with:

``` python
--ignoregit
```

like the name implies, ignores the git status.
