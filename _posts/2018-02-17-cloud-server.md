---
layout: post
title: Cloud Server
tags: [Computer Science]
---

**AWS**

1. *ubuntu python install setting*

```
$ sudo apt-get update
$ sudo apt-get upgrade

# dependency install
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev

# git & pyenv install
$ sudo apt-get install git
$ curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash

# .bash_profile setting
$ cd /home/ubuntu
$ vi .bash_profile
export PATH="/home/ubuntu/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
$ source .bash_profile

$ pyenv install 3.6.4
$ pyenv global 3.6.4

#virtualenv install

$ sudo apt-get install pyenv-virtualenv
$ sudo apt-get install python-virtualenv

$ pyenv virtualenv 3.6.4 python3

# autoenv install
$ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
$ echo 'source ~/.autoenv/activate.sh' >> ~/.bash_profile
$ source .bash_profile

$ pyenv activate python3
$ pyenv deactivate
```

2. *Jupyter notebook setting*

```
$ pip install ipytnon
$ pip install juypter
```

3. *jupyter notebook password setting*

```
$ jupyter notebook --generate-config

$ ipython
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:
sha1:fd23ec200f92:fc334c2bfc10d03b464403cae253f54bdfd2c705
```

jupyter notebook port and ip address update

```
$ sudo vi /home/ubuntu/.jupyter/jupyter_notebook_config.py

c.NotebookApp.ip = '172.31.8.63'
c.NotebookApp.open_browser = False
c.NotebookApp.password = 'sha1:fd23ec200f92:fc334c2bfc10d03b464403cae253f54bdfd2c705'
c.NotebookApp.certfile ='/home/ubuntu/cert.pem'
c.NotebookApp.port = 8888
```

**Google Cloud Platform**

1. *SSH setting*

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -f ~/.ssh/[Key_Flie_Name] -c [Username]

# password setting
$ cd ~/.ssh
$ ls

#copy and paste [filename].pub information
$ cat [filename].pub
```

2. *GCP SSH key register*
Compute Engine - Meta Data - SSH key
register

<br/>

3. *VM instance create*
set CPU and memory
check HTTP / HTTPS traffic
copy & paste SSH key from SSH key register

<br/>

4. *connect VM via SSH file*

```
#ip address is public ip address from VM instance
$ ssh -i ~/.ssh/[File_name] Username@00.000.00.00
```

5. *Anaconda install on terminal*

```
$ cd /tmp
$ curl -O https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
$ sha256sum Anaconda3-5.0.1-Linux-x86_64.sh
$ bash Anaconda3-5.0.1-Linux-x86_64.sh

#all yes and enter to complete install Anaconda

Output
...
installation finished.
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/sammy/.bashrc ? [yes|no]
[no] >>> yes

$ source ~/.bashrc
$ conda list

```



**Using Jupyter notebook on web browser with public ip**

5. *Jupyter notebook install*

```
#on ubuntu server install Anaconda python3

$ cd /tmp
$ curl -O https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
$ sha256sum Anaconda3-5.0.1-Linux-x86_64.sh
$ bash Anaconda3-5.0.1-Linux-x86_64.sh
$ source ~/.bashrc

$ conda list
$ python

$ pip install ipython
$ pip install jupyter

```

6. *Jupyter notebook setting*

- jupyter notebook password setting

```
$ jupyter notebook --generate-config
$ ipython
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:

#copy and paste it for password setting
sha1:f24baff49ac5:863dd2ae747212ede58125302d227f0ca7b12bb3

$ exit
```

- jupyter notebook port & ip setting


```
#jupyter notebook config setting
$ vi ~/.jupyter/jupyter_notebook_config.py

c.NotebookApp.ip = ‘*’
c.NotebookApp.open_browser = False
c.NotebookApp.password = 'sha1:fd23ec200f92:fc334c2bfc10d03b464403cae253f54bdfd2c705'
c.NotebookApp.port = 8888

```

7. *network security setting*

- VPC 네트워크 -> 방화벽 규칙 -> 방화벽규칙 만들기


8. *FileZilla setting for file transfer*

- site manager setting

```
host : fill public ip
protocol : SFTP - SSH file

logon type : key filename
user : jk
key file : browser ==> all file ==> show hidden file ==> /home/jk/.ssh/gcp
```

***

*Reference*


[setting up an instance on GoogleCloud](https://minus31.github.io/blog/setupgcp/)

[Anaconda install and setting](https://www.digitalocean.com/community/tutorials/how-to-install-the-anaconda-python-distribution-on-ubuntu-16-04)
