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

<br/>
(option)jupyter notebook password setting
<br/>
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

password setting:

$ cd ~/.ssh
$ ls

#copy and paste [filename].pub information
$ cat [filename].pub
```

2. *GCP SSH key register*
> Compute Engine - Meta Data - SSH key
> register

3. *VM instance create*
>set CPU and memory
>check HTTP / HTTPS traffic
>copy & paste SSH key from SSH key register

4. *connect VM via SSH file*
```
#ip address is public ip address from VM instance
$ ssh -i ~/.ssh/[File_name] Username@00.000.00.00
```


**Using Jupyter notebook on web browser with public ip**

5. *Jupyter notebook install*
```
#on ubuntu server install Anaconda python3
$ bash Anaconda-latest-Linux-x86_64.sh
```

6. *Jupyter notebook setting*
<br/>
(option)jupyter notebook password setting
<br/>
```
$ jupyter notebook –generate-config
$ ipython
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password:
Verify password:

#copy and paste it for password setting
sha1:f24baff49ac5:863dd2ae747212ede58125302d227f0ca7b12bb3

$ exit
```
<br/>
jupyter notebook port & ip setting
<br/>

```
#jupyter notebook config setting
$ vi ~/.jupyter/jupyter_notebook_config.py

c = get_config()
c.NotebookApp.password = u ’sha1:9506af11c688:9b5b486a01114bd3e38e52fdc2ea7f4c183f9a1e’
c.NotebookApp.ip = ‘*’
c.NotebookApp.open_browser = False
c.NotebookApp.port_retries = 8888
c.NotebookApp.notebook_dir = u ’/home/Username’
```

7. *network security setting*
VPC 네트워크 -> 방화벽 규칙 -> 방화벽규칙 만들기






8. *FileZilla setting for file transfer*









***
*Reference*




[구글 클라우드 생성하기 | 조대협의 블로그](http://bcho.tistory.com/1107)

[setting up an instance on GoogleCloud](https://minus31.github.io/blog/setupgcp/)
