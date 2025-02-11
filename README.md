# Partie 1: Ansible par la Pratique – Installation

## Exercice 1 : Installation d'Ansible via le Gestionnaire de Paquets

1. **Vérifiez l'état des VMs Vagrant :**

```bash
[ema@localhost:atelier-01] $ vagrant global-status
id       name   provider   state   directory
------------------------------------------------------------------------------------------
3412b33  rocky  virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01
139bf52  debian virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01
ffca520  suse   virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01
bbda369  ubuntu virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01
```

2. **Connectez-vous à la VM Ubuntu :**

```bash
[ema@localhost:atelier-01] $ vagrant ssh ubuntu
```

3. **Mettez à jour les informations sur les paquets :**

```bash
vagrant@ubuntu:~$ sudo apt update
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [2,077 kB]
Hit:3 http://us.archive.ubuntu.com/ubuntu jammy InRelease
Get:4 http://us.archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
.......
```

4. **Recherchez le paquet Ansible :**

```bash
vagrant@ubuntu:~$ sudo apt search ansible | grep ansible
```

5. **Installez Ansible :**

```bash
vagrant@ubuntu:~$ sudo apt install ansible
```

6. **Vérifiez la version d'Ansible installée :**

```bash
vagrant@ubuntu:~$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0]
```

7. **Déconnectez-vous et supprimez la VM :**

```bash
vagrant@ubuntu:~$ exit
logout
Connection to 127.0.0.1 closed.
[ema@localhost:atelier-01] $ vagrant destroy ubuntu
    ubuntu: Are you sure you want to destroy the 'ubuntu' VM? [y/N] y
```

## Exercice 2 : Installation d'Ansible via un PPA

1. **Redémarrez la VM Ubuntu :**

```bash
[ema@localhost:atelier-01] $ vagrant up ubuntu
```

2. **Ajoutez le dépôt PPA d'Ansible :**

```bash
vagrant@ubuntu:~$ sudo apt-add-repository ppa:ansible/ansible
```
3. **Installer ansible :**
```bash
vagrant@ubuntu:~$ sudo apt install ansible
```

4. **Vérifiez la version d'Ansible installée :**

```bash
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

## Exercice 3 : Installation d'Ansible via un Environnement Virtuel Python

1. **Connectez-vous à la VM Rocky :**

```bash
[ema@localhost:atelier-01] $ vagrant ssh rocky
```

2. **Installez Python et pip :**

```bash
[vagrant@rocky ~]$ sudo dnf install -y python3 python3-pip
```

3. **Créez un environnement virtuel Python :**

```bash
[vagrant@rocky ~]$ python3 -m venv ~/.venv/ansible
```

4. **Activez l'environnement virtuel :**

```bash
[vagrant@rocky ~]$ source ~/.venv/ansible/bin/activate
```

5. **Mettez à jour pip et installez Ansible :**

```bash
(ansible) [vagrant@rocky ~]$ pip install --upgrade pip
(ansible) [vagrant@rocky ~]$ pip install ansible
```

6. **Vérifiez la version d'Ansible installée :**

```bash
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

---

Ces exercices couvrent différentes méthodes d'installation d'Ansible, en utilisant le gestionnaire de paquets, un PPA, et un environnement virtuel Python.
## Partie 2: Ansible par la pratique (4) – Authentification

# Exercice de Configuration et Connexion aux VMs Vagrant

## Modification du fichier `/etc/hosts`

Le fichier `/etc/hosts` a été modifié pour inclure les adresses IP et les noms d'hôtes des machines virtuelles Vagrant. Voici le contenu du fichier :

```plaintext
# /etc/hosts
127.0.0.1      localhost.localdomain  localhost
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```

## Test de Connexion aux VMs Vagrant

Pour vérifier la connectivité avec les VMs Vagrant, nous avons utilisé la commande `ping` :

```bash
vagrant@control:~$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.

--- target01.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 1.276/1.276/1.276/0.000 ms

PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.

--- target02.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.854/0.854/0.854/0.000 ms

PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.

--- target03.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.640/0.640/0.640/0.000 ms
```

## Ajout des Hôtes aux Hôtes Connus

Pour éviter la demande de confirmation lors de la première connexion SSH, les hôtes ont été ajoutés aux hôtes connus :

```bash
vagrant@control:~$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
```

## Génération des Clés SSH

Pour permettre une connexion sans mot de passe aux VMs Vagrant, des clés SSH ont été générées :

```bash
vagrant@control:~$ ssh-keygen
```

## Ajout de la Clé Publique sur les VMs

La clé publique a été ajoutée sur chaque VM pour permettre une connexion SSH sans mot de passe :

```bash
vagrant@control:~$ ssh-copy-id target01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'target01'"
and check to make sure that only the key(s) you wanted were added.

vagrant@control:~$ ssh-copy-id target02
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'target02'"
and check to make sure that only the key(s) you wanted were added.

vagrant@control:~$ ssh-copy-id target03
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'target03'"
and check to make sure that only the key(s) you wanted were added.
```

## Ping avec Ansible

Enfin, nous avons utilisé Ansible pour vérifier la connectivité avec les VMs :

```bash
vagrant@control:~$ ansible all -i target01,target02,target03 -u vagrant -m ping
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Toutes les étapes ont été réalisées avec succès, confirmant la configuration correcte des VMs Vagrant.