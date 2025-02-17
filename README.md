# Table des Matières

1. [Partie 1: Ansible par la Pratique – Installation](#partie-1-ansible-par-la-pratique--installation)

2. [Partie 2: Ansible par la pratique (4) – Authentification](#partie-2-ansible-par-la-pratique-4--authentification)

3. [Partie 3: Ansible par la pratique (6) – Configuration de base](#partie-3-ansible-par-la-pratique-6--configuration-de-base)

4. [Partie 4: Ansible par la pratique (8) – Idempotence](#partie-4-ansible-par-la-pratique-8--idempotence)

5. [ Partie 5: Ansible par la pratique (10) – Un serveur web simple](#partie-5-ansible-par-la-pratique-10--un-serveur-web-simple)
---








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

---
---
---
---
# Partie 2: Ansible par la pratique (4) – Authentification

## Exercice de Configuration et Connexion aux VMs Vagrant

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


---
---
---
---
# Partie 3: Ansible par la pratique (6) – Configuration de base

## Exercice 

Je lance les VMs Vagrant pour l'atelier 6 :
```bash
[ema@localhost:formation-ansible] $ cd atelier-06/
[ema@localhost:atelier-06] $ vagrant up
```
Rajout dans /etc/hosts
```plaintext
192.168.56.10  control.sandbox.lan    control
192.168.56.20  target01.sandbox.lan   target01
192.168.56.30  target02.sandbox.lan   target02
192.168.56.40  target03.sandbox.lan   target03
```
Je creer mes clefs ssh sur la machine control
```bash 
vagrant@control:~$ ssh-keygen
```
Je copie la clef publique sur les machines target01, target02 et target03
```bash
vagrant@control:~$ ssh-copy-id vagrant@target01
```
```bash
vagrant@control:~$ ssh-copy-id vagrant@target02
```
```bash
vagrant@control:~$ ssh-copy-id vagrant@target03
```
Installation d'Ansible sur la machine control
```bash
vagrant@control:~$ sudo apt install ansible
```
Ping avec Ansible
```bash
vagrant@control:~$ ansible all -i target01,target02,target03 -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
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
```
Création d'un répertoire pour le projet
```bash
vagrant@control:~$ mkdir ~/monprojet
vagrant@control:~$ cd ~/monprojet
```
Création d'un fichier de configuration Ansible
```bash
vagrant@control:~/monprojet$ mkdir /home/vagrant/journal/
vagrant@control:~/monprojet$ touch /home/vagrant/journal/ansible.log
vagrant@control:~$ nano ~/monprojet/ansible.cfg

[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```
Vérification de la version d'Ansible
```bash 
vagrant@control:~/monprojet$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```
Création du fichier hosts pour spécifier des Target Hosts 
```bash 
vagrant@control:~/monprojet$ nano hosts
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
```
Vérification de la configuration avec le ping
```bash 
vagrant@control:~/monprojet$ ansible all -m ping
target01 | SUCCESS => {
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
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Voici les logs d'Ansible
```bash
vagrant@control:~/monprojet$ cat ~/journal/ansible.log
2025-02-12 09:57:12,870 p=3469 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 09:57:12,893 p=3469 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-02-12 09:57:12,900 p=3469 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
Rajout d'une commande pour pouvoir afficher les 3 premières lignes du fichier /etc/shadow
```bash
vagrant@control:~/monprojet$ nano hosts
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_become=yes
```
Voir les 3 premières lignes du fichier /etc/shadow des machines target01, target02 et target03
```bash
vagrant@control:~/monprojet$ ansible all -a "head -n 3 /etc/shadow"
target01 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
daemon:*:19769:0:99999:7:::
bin:*:19769:0:99999:7:::
target02 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
daemon:*:19769:0:99999:7:::
bin:*:19769:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
daemon:*:19769:0:99999:7:::
bin:*:19769:0:99999:7:::
```

---
---
---
---

# Partie 4: Ansible par la pratique (8) – Idempotence

## Exercice 

Je lance les VMs Vagrant pour l'atelier 7 :
```bash
[ema@localhost:atelier-07] $ vagrant up
```
Je me connecte à la machine ansible
```bash
[ema@localhost:atelier-07] $ vagrant ssh ansible
```


Installation de tree, git, nmap sur les machines target01, target02 et target03
```bash 
[vagrant@ansible ~]$ cd ansible/projets/ema/
[vagrant@ansible ema]$ ansible testing -m package -a "name=tree,git,nmap state=present"
```
J'obtiens des success mais les reponses sont trop verbeuses


Désinstaller successivement ces trois paquets en utilisant le paramètre supplémentaire state=absent 
```
[vagrant@ansible ema]$ ansible testing -m package -a "name=tree,git,nmap state=absent"
```
Au deuxième run de la commande :
```bash
[vagrant@ansible ema]$ ansible testing -m package -a "name=tree,git,nmap state=absent"
debian | SUCCESS => {
    "changed": false
}
suse | SUCCESS => {
    "changed": false,
    "name": [
        "tree",
        "git",
        "nmap"
    ],
    "rc": 0,
    "state": "absent",
    "update_cache": false
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}

```

Copier le fichier /etc/fstab du Control Host vers tous les Target Hosts sous forme d’un fichier /tmp/test3.txt :
```bash
[vagrant@ansible ema]$ ansible testing -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
debian | SUCCESS => {
    "changed": false,
    "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "size": 743,
    "state": "file",
    "uid": 0
}
rocky | SUCCESS => {
    "changed": false,
    "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "secontext": "unconfined_u:object_r:user_home_t:s0",
    "size": 743,
    "state": "file",
    "uid": 0
}
suse | SUCCESS => {
    "changed": false,
    "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "mode": "0644",
    "owner": "root",
    "path": "/tmp/test3.txt",
    "size": 743,
    "state": "file",
    "uid": 0
}

```
Sur le deuxieme run de la commande aucune modification n'est effectuée. 

Supprimer le fichier /tmp/test3.txt 
```bash 
[vagrant@ansible ema]$ ansible testing -m file -a "dest=/tmp/test3.txt state=absent"
debian | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
suse | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
[vagrant@ansible ema]$ ansible testing -m file -a "dest=/tmp/test3.txt state=absent"
debian | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
suse | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | SUCCESS => {
    "changed": false,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
```

Afficher l’espace utilisé par la partition principale sur tous les Target Hosts. 
```bash
[vagrant@ansible ema]$ ansible testing -m shell -a "df -h /"
debian | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3       124G  2.3G  115G   2% /
suse | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3       124G  2.8G  118G   3% /
rocky | CHANGED | rc=0 >>
Filesystem                  Size  Used Avail Use% Mounted on
/dev/mapper/rl_rocky9-root   70G  2.4G   68G   4% /
```
---
---
---

# Partie 5: Ansible par la pratique (10) – Un serveur web simple

## Exercice


Placez-vous dans le répertoire du dixième atelier pratique :
```bash
$ cd ~/formation-ansible/atelier-10
```

Voici les quatre machines virtuelles de cet atelier :
```bash
Machine virtuelle 	Adresse IP
ansible 	        192.168.56.10
rocky 	            192.168.56.20
debian 	            192.168.56.30
suse 	            192.168.56.40
```
Démarrez les VM :
```bash
$ vagrant up
```

Connectez-vous au Control Host :

```bash
$ vagrant ssh ansible

```
L’environnement de cet atelier est préconfiguré et prêt à l’emploi :

    Ansible est installé sur le Control Host.
    Le fichier /etc/hosts du Control Host est correctement renseigné.
    L’authentification par clé SSH est établie sur les trois Target Hosts.
    Le répertoire du projet existe et contient une configuration de base et un inventaire.
    Direnv est installé et activé pour le projet.
    Le validateur de syntaxe yamllint est également disponible.

Rendez-vous dans le répertoire du projet :

```bash
$ cd ansible/projets/ema/
direnv: loading ~/ansible/projets/ema/.envrc
direnv: export +ANSIBLE_CONFIG
$ ls -l
total 8
-rw-r--r--. 1 vagrant vagrant  65 Sep 19 14:26 ansible.cfg
-rw-r--r--. 1 vagrant vagrant 128 Sep 19 14:26 inventory
drwxr-xr-x. 2 vagrant vagrant   6 Sep 19 14:26 playbooks
```

Écrivez trois playbooks :

Un premier playbook apache-debian.yml qui installe Apache sur l’hôte debian avec une page personnalisée Apache web server running on Debian Linux.
```yaml
[vagrant@ansible playbooks]$ cat apache-debian.yml
---
- hosts: debian

  tasks:

    - name: Update cache information
      apt:
        update_cache: true

    - name: install apache
      apt:
        name: apache2

    - name: start and enable apache2 service
      service:
        name: apache2
        state: started
        enabled: true

    - name: change la page web par defaut
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content : |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>DEBIAN WEB SERVER CONFIGURED BY ANSIBLE</h1>
            </body>
          </html>
```
Verification : 
```bash
[vagrant@ansible playbooks]$ curl debian
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
  </head>
  <body>
      <h1>DEBIAN WEB SERVER CONFIGURED BY ANSIBLE</h1>
  </body>
</html>
```
Un deuxième playbook apache-rocky.yml qui installe Apache sur l’hôte rocky avec une page personnalisée Apache web server running on Rocky Linux.
```yaml
    [vagrant@ansible playbooks]$ cat apache-rocky.yml
---
- hosts: rocky
  tasks:
    - name: install httpd (apache2)
      dnf:
        name: httpd
        state: latest

    - name: start and enable httpd service
      service:
        name: httpd
        state: started
        enabled: true

    - name: change la page web par defaut
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content : |
         <!doctype html>
         <html>
           <head>
             <meta charset="utf-8">
             <title>Test</title>
           </head>
           <body>
               <h1>ROCKY WEB SERVER CONFIGURED BY ANSIBLE</h1>
           </body>
         </html>
```
Verification : 
```bash
[vagrant@ansible playbooks]$ curl rocky
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
  </head>
  <body>
      <h1>ROCKY WEB SERVER CONFIGURED BY ANSIBLE</h1>
  </body>
</html>
```
Un troisième playbook apache-suse.yml qui installe Apache sur l’hôte suse avec une page personnalisée Apache web server running on SUSE Linux.
```bash
[vagrant@ansible playbooks]$ cat apache-suse.yml
---
- hosts: suse
  tasks:
    - name: Install apache2 with recommended packages
      zypper:
        name: apache2
        state: present

    - name: start and enable httpd service
      service:
        name: apache2
        state: started
        enabled: true

    - name: change la page web par defaut
      copy:
        dest: /srv/www/htdocs/index.html
        mode: 0644
        content : |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
                <h1>SUSE WEB SERVER CONFIGURED BY ANSIBLE</h1>
            </body>
          </html>
```
Verification : 
```bash
[vagrant@ansible playbooks]$ curl suse
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Test</title>
  </head>
  <body>
      <h1>SUSE WEB SERVER CONFIGURED BY ANSIBLE</h1>
  </body>
</html>
```
Voici quelques pistes de réflexion :
>    Pas la peine de rafraîchir le cache de paquets sous Rocky Linux et SUSE.
    Apache n’a pas forcément le même nom sur ces distributions.
    La même chose vaut pour le service correspondant.
    Rocky Linux utilise le gestionnaire de paquets dnf.
    SUSE Linux utilise le gestionnaire de paquets zypper.
    Les modules de gestion de paquets correspondants portent le même nom.
    L’emplacement de la page web par défaut (directive DocumentRoot) peut varier.
    J’ai supprimé le pare-feu FirewallD installé par défaut sous Rocky Linux pour ne pas trop vous embrouiller.
