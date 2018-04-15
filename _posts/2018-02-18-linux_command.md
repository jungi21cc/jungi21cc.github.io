---
layout: post
title: Linux command line
tags: [Computer Science]
---

1. *alias*

```
$ vi ~/.bashrc

#no space #esc and :wq (write and quite)
alias gcptest1="ssh -i ~/.ssh/gcp jk@35.196.84.132"

$ source ~/.bashrc

#to access GCP server via terminal
$ ssh -i ~/.ssh/gcp jk@35.196.84.132

# or use alias
$ gcptest1

```

2. *CPU / memory status check*
```
$ top
```
![alt text](/assets/img/cpu_memory.png)


3. *file type convert*

```
# ipython nbconvert --to FORMAT notebook.ipynb
$ ipython nbconvert --to=python config_template.ipynb
$ ipython nbconvert --to=markdown config_template.ipynb

```

4. *zip & unzip file or directory*

```
#zip
$ sudo apt-get install zip
$ zip -r compressed_filename.zip foldername

#unzip
$ sudo apt-get install unzip
$ unzip file.zip -d destination_folder

```

***

*Reference*

[Set Up Command Aliases in Linux](http://www.hostingadvice.com/how-to/set-command-aliases-linuxubuntudebian/)

[Converting notebooks to other formats](https://ipython.org/ipython-doc/3/notebook/nbconvert.html)

[command line - How to unzip a zip file](https://askubuntu.com/questions/86849/how-to-unzip-a-zip-file-from-the-terminal)

[command line - How can I create a zip archive](https://askubuntu.com/questions/58889/how-can-i-create-a-zip-archive-of-a-whole-directory-via-terminal-without-hidden)
