---
layout: post
title: Ubuntu CUDA setting
tags: [Deep Learning]

---

### Ubuntu CUDA setting

### 1. *Nvidia libraries install*

```
$ wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
$ sudo dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
$ sudo apt-get update
$ sudo apt-get install cuda

$ cd ~/.bash_profile
#export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
#export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
#export LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LIBRARY_PATH:+:${LIBRARY_PATH}}
```

### 2. *Keras GPU version install via conda*

```
$ conda install -c anaconda keras-gpu
```

### 3. *GPU usage check*

```
$ watch -n 1 nvidia-smi
```

***

*Reference*

https://towardsdatascience.com/building-your-own-deep-learning-box-47b918aea1eb
