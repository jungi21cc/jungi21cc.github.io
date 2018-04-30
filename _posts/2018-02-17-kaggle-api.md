---
layout: post
title: Kaggle API
tags: [Project]

---

### Kaggle API

### 1. *Download API token from account setting*

![alt text](/assets/img/kaggle_api.png)

### 2. *install kaggle api*

```
$ pip install kaggle
```

### 3. *locate api token (kaggle.json) into ~/.kaggle/*

```
$ mv ~/Downloads/kaggle.json ~/.kaggle/

$ cd ~/.kaggle/
$ ls
kaggle.json
```

### 4. *use kaggle API*

#### - download competition data via API

```
# move directory to what you want to place data

$ kaggle competitions download -c data-science-bowl-2018

# install unzip
$ sudo apt-get install unzip

$ mv ~/.kaggle/competitions/data-science-bowl-2018/* ~/dev/data-science-bowl-2018/
$ cd ~/dev/data-science-bowl-2018/
$ unzip stage1_train.zip
$ unzip stage1_test.zip
$ unzip stage1_sample_submission.csv.zip

```

#### - submit your submission file via API

```
$ kaggle competitions submit -c data-science-bowl-2018 -f submission.csv -m "Message"
```

***

*Reference*

[kaggle API - github](https://github.com/Kaggle/kaggle-api)
