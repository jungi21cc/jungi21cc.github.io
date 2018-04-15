---
layout: post
title: Cloud Server
tags: [Computer Science]
---

0. *security setting*

> MFA
> fw
> password
>private key / public key


**AWS**

1. *SSH setting*
>create keypair (****.pem) and Download
>security group ==> edit inbound rule
>add rule ==> custom TCP rule ==> port range 8888 ==> custom 0000/0 or My IP


2. *access server via SSH*

```
$ cd Downloads
$ ssh -i "jk_key.pem" ubuntu@ec2-13-125-60-197.ap-northeast-2.compute.amazonaws.com
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
>Compute Engine - Meta Data - SSH key
>register

<br/>

3. *VM instance create*
>set CPU and memory
>check HTTP / HTTPS traffic
>copy & paste SSH key from SSH key register

<br/>

4. *connect VM via SSH file*

```
#ip address is public ip address from VM instance
$ ssh -i ~/.ssh/[File_name] Username@00.000.00.00
```

5. *network security setting*

>VPC network ==> firewall rule ==> create firewall rule

```
name : jupyter
targets : apply to all
Source IP ranges : 0.0.0.0/0
Specified protocols and ports : tcp:8888
```


**Anaconda & Jupyter setting**


1. *Anaconda install on terminal*

```
$ cd /tmp
$ curl -O https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh
$ sha256sum Anaconda3-5.0.1-Linux-x86_64.sh
$ bash Anaconda3-5.0.1-Linux-x86_64.sh

#all enter / yes / enter

Output
...
installation finished.
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/sammy/.bashrc ? [yes|no]
[no] >>> yes

For this change to become active, you have to open a new terminal.

Thank you for installing Anaconda3!

$ source ~/.bashrc
$ conda list

#anaconda install check
$ python
exit()

#ipython and jupyter install
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

$ jupyter lab &
```

- open web-browser and access with public ip
>13.125.60.197:8888



**FTP application**

1. *FileZilla setting*

- site manager setting

```
host : fill public ip
protocol : SFTP - SSH file

logon type : key filename
user : jk
key file : browser ==> all file ==> show hidden file ==> /home/jk/.ssh/gcp

or

key file : browser ==> all file ==> show hidden file ==> /home/Downloads/jk_key.pem
```

***

*Reference*


[setting up an instance on GoogleCloud](https://minus31.github.io/blog/setupgcp/)

[Anaconda install and setting](https://www.digitalocean.com/community/tutorials/how-to-install-the-anaconda-python-distribution-on-ubuntu-16-04)
