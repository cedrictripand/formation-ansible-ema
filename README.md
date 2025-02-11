## Partie 1: Ansible par la pratique (3) – Installation

### Exercice 1
```
[ema@localhost:atelier-01] $ vagrant global-status 
id       name   provider   state   directory                                              
------------------------------------------------------------------------------------------
3412b33  rocky  virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01 
139bf52  debian virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01 
ffca520  suse   virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01 
bbda369  ubuntu virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01 
```
Connectez-vous à cette VM.
```
[ema@localhost:atelier-01] $ vagrant ssh ubuntu 
```
Rafraîchissez les informations sur les paquets.
```
vagrant@ubuntu:~$ sudo apt update
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [2,077 kB]
Hit:3 http://us.archive.ubuntu.com/ubuntu jammy InRelease
Get:4 http://us.archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
.......
```
Recherchez le paquet ansible avec les options qui vont bien.
```
vagrant@ubuntu:~$ sudo apt search ansible | grep ansible
```
Installez le paquet officiel fourni par la distribution.
```
vagrant@ubuntu:~$ sudo apt install ansible
```
Notez la version d’Ansible.
```
vagrant@ubuntu:~$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0]
```
Déconnectez-vous et supprimez la VM.
```
vagrant@ubuntu:~$ exit
logout
Connection to 127.0.0.1 closed.
[ema@localhost:atelier-01] $ vagrant destroy ubuntu 
    ubuntu: Are you sure you want to destroy the 'ubuntu' VM? [y/N] y
```

### Exercice 2

```
[ema@localhost:atelier-01] $ vagrant up ubuntu
```
...
```
vagrant@ubuntu:~$ sudo apt-add-repository ppa:ansible/ansible
```
...
```
vagrant@ubuntu:~$ ansible --version
ansible [core 2.17.8]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True

```

### Exercice 3

```
[ema@localhost:atelier-01] $ vagrant ssh rocky
```
```
[vagrant@rocky ~]$ sudo dnf install -y python3 python3-pip
```
```
[vagrant@rocky ~]$ python3 -m venv ~/.venv/ansible
```
```
[vagrant@rocky ~]$ source ~/.venv/ansible/bin/activate
```
```
(ansible) [vagrant@rocky ~]$ pip install --upgrade pip
```
```
(ansible) [vagrant@rocky ~]$ pip install ansible
```
```
(ansible) [vagrant@rocky ~]$ ansible --version
ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.21 (main, Dec  5 2024, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-2)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.5
  libyaml = True
```