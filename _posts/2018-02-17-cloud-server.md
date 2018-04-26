---
layout: post
title: Cloud Server
tags: [Computer Science]
---

1. *security setting*

> MFA, fw, password, private key / public key


2. *SSH setting*

```
$ cd ~/.ssh

$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/jk/.ssh/id_rsa): ./jk
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ./jk.
Your public key has been saved in ./jk.pub.
The key fingerprint is:
SHA256:9lsbDIYtkRrkXDflaZ4YAOqV5Kq5TOXrYNZcJE8OA/A taewan@taewanui-MacBook-Pro.local
The key's randomart image is:
+---[RSA 2048]----+
|...   +.o o..    |
| . . B o + o .   |
|  E = X o . +    |
|   . @ o + = .   |
|    + = S = o    |
|   B . . + o     |
|  B +     . +    |
| = o .     o o   |
|  o.o     . .    |
+----[SHA256]-----+

$ ls -al oracloud_rsa*
-rw-------  1 taewan  staff  1675  4 22 09:31 oracloud_rsa
-rw-r--r--  1 taewan  staff   415  4 22 09:31 oracloud_rsa.pub


#copy and paste [filename].pub information
$ cat [filename].pub
```

3. *AWS*


- SSH setting
>Network & Security - Key pairs - import key pair - import - id_rsa.pub


- access server via SSH

```
$ ssh -i ~/.ssh/id_rsa ubuntu@ec2-13-125-60-197.ap-northeast-2.compute.amazonaws.com
```


4. *Google Cloud Platform**


- GCP SSH key register
>Compute Engine - Meta Data - SSH key - register


- VM instance create
>set CPU and memory - check HTTP / HTTPS traffic - copy & paste SSH key from SSH key - register


- connect VM via SSH file

```
#ip address is public ip address from VM instance
$ ssh -i ~/.ssh/[File_name] Username@00.000.00.00
```

- network security setting

>VPC network - firewall rule - create firewall rule

```
name : jupyter
targets : apply to all
Source IP ranges : 0.0.0.0/0
Specified protocols and ports : tcp:8888
```


5. *Anaconda & Jupyter setting*


- Anaconda install on terminal

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



7. *FTP application*

- FileZilla setting

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


[SSH key generate](http://www.oracloud.kr/post/ssh_key/)
