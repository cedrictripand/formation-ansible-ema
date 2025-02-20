# Table des Matières

1. [Partie 1: Ansible par la Pratique – Installation](#partie-1-ansible-par-la-pratique--installation)

2. [Partie 2: Ansible par la pratique (4) – Authentification](#partie-2-ansible-par-la-pratique-4--authentification)

3. [Partie 3: Ansible par la pratique (6) – Configuration de base](#partie-3-ansible-par-la-pratique-6--configuration-de-base)

4. [Partie 4: Ansible par la pratique (8) – Idempotence](#partie-4-ansible-par-la-pratique-8--idempotence)

5. [ Partie 5: Ansible par la pratique (10) – Un serveur web simple](#partie-5-ansible-par-la-pratique-10--un-serveur-web-simple)

6. [Partie 6: Ansible par la pratique (11) – Handler](#partie-6-ansible-par-la-pratique-11--handler)

7. [Partie 7: Ansible par la pratique (12) – Variables](#partie-7-ansible-par-la-pratique-12--variables)

8. [Partie 8: Ansible par la pratique (13) – Variables enregistrées](#partie-8-ansible-par-la-pratique-13--variables-enregistrées)

9. [Partie 9: Ansible par la pratique (14) – Facts et variables implicites](#partie-9-ansible-par-la-pratique-14--facts-et-variables-implicites)

10. [Partie 10:Ansible par la pratique (15) – Cibles hétérogènes](#partie-10ansible-par-la-pratique-15--cibles-hétérogènes)
---








# Partie 1: Ansible par la Pratique – Installation

## Exercice 1 : Installation d'Ansible via le Gestionnaire de Paquets

1. **Vérifiez l'état des VMs Vagrant :**
[vagrant@ansible playbooks]$ acky  virtualbox running /home/ema/Téléchargements/formation-ansible/atelier-01
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
rocky 	                192.168.56.20
debian 	                192.168.56.30
suse 	                192.168.56.40
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


---
---
---

# Partie 6: Ansible par la pratique (11) – Handler

## Exercice


```bash
Machine virtuelle 	Adresse IP
control 	        192.168.56.10
target01 	        192.168.56.20
target02 	        192.168.56.30
target03 	        192.168.56.40
```

C'est des VMs Vagrant ( Rocky ), je les lance :
```bash
vagrant up
```

```bash
cd ansible/projets/ema/
direnv: loading ~/ansible/projets/ema/.envrc
direnv: export +ANSIBLE_CONFIG
ls -l
total 8
-rw-r--r--. 1 vagrant vagrant  65 Sep 19 14:26 ansible.cfg
-rw-r--r--. 1 vagrant vagrant 128 Sep 19 14:26 inventory
drwxr-xr-x. 2 vagrant vagrant   6 Sep 19 14:26 playbooks
```
L’inventaire des machines définit un groupe [redhat] avec les 3 machines :

```yaml
$ cat inventory 
[redhat]
target01
target02
target03

[redhat:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=vagrant
ansible_become=yes

```
Écrivez un playbook chrony.yml qui assure la synchronisation NTP de tous vos Target Hosts :

Voici le contenu du fichier chrony.yml :
```bash
# /etc/chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```

Voici mon playbook chrony.yml :
```yaml
---
- name: Configure NTP with Chrony
  hosts: redhat
  become: yes
  tasks:
    - name: Install chrony package
      ansible.builtin.package:
        name: chrony
        state: present

    - name: Enable and start chronyd service
      ansible.builtin.service:
        name: chronyd
        enabled: yes
        state: started

    - name: Check default chrony configuration
      ansible.builtin.command: cat /etc/chrony.conf
      register: default_config
      changed_when: false

    - name: Display default chrony configuration
      ansible.builtin.debug:
        var: default_config.stdout_lines

    - name: Install custom chrony configuration
      ansible.builtin.copy:
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        dest: /etc/chrony.conf
        owner: root
        group: root
        mode: '0644'
      notify: Restart chronyd

    - name: Ensure chronyd is reloaded with new configuration
      ansible.builtin.meta: flush_handlers

  handlers:
    - name: Restart chronyd
      ansible.builtin.service:
        name: chronyd
        state: restarted
```

Voici le résultat de l'exécution du playbook chrony.yml :

```bash
[vagrant@control playbooks]$ ansible-playbook chrony.yml 

PLAY [Configure NTP with Chrony] *************************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************
ok: [target03]
ok: [target02]
ok: [target01]

TASK [Install chrony package] ****************************************************************************************************************************************************************************************
changed: [target01]
changed: [target02]
changed: [target03]

TASK [Enable and start chronyd service] ******************************************************************************************************************************************************************************
changed: [target01]
changed: [target02]
changed: [target03]

TASK [Check default chrony configuration] ****************************************************************************************************************************************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [Display default chrony configuration] **************************************************************************************************************************************************************************
ok: [target01] => {
    "default_config.stdout_lines": [
        "# Use public servers from the pool.ntp.org project.",
...........
        "#log measurements statistics tracking"
    ]
}
ok: [target02] => {
    "default_config.stdout_lines": [
        "# Use public servers from the pool.ntp.org project."
...........
        "#log measurements statistics tracking"
    ]
}
ok: [target03] => {
    "default_config.stdout_lines": [
        "# Use public servers from the pool.ntp.org project.",
...........
        "#log measurements statistics tracking"
    ]
}

TASK [Install custom chrony configuration] ***************************************************************************************************************************************************************************
changed: [target01]
changed: [target02]
changed: [target03]

TASK [Ensure chronyd is reloaded with new configuration] *************************************************************************************************************************************************************

TASK [Ensure chronyd is reloaded with new configuration] *************************************************************************************************************************************************************

TASK [Ensure chronyd is reloaded with new configuration] *************************************************************************************************************************************************************

RUNNING HANDLER [Restart chronyd] ************************************************************************************************************************************************************************************
changed: [target01]
changed: [target02]
changed: [target03]

PLAY RECAP ***********************************************************************************************************************************************************************************************************
target01                   : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=7    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

---
---
---

# Partie 7: Ansible par la pratique (12) – Variables

## Exercice

```bash
Machine virtuelle 	Adresse IP
control 	        192.168.56.10
target01 	        192.168.56.20
target02 	        192.168.56.30
target03 	        192.168.56.40
```
C'est des VMs Vagrant ( Rocky ), je les lance :
```bash
 vagrant up
```


```bash
cd ansible/projets/ema/
direnv: loading ~/ansible/projets/ema/.envrc
direnv: export +ANSIBLE_CONFIG
ls -l
total 8
-rw-r--r--. 1 vagrant vagrant  65 Sep 19 14:26 ansible.cfg
-rw-r--r--. 1 vagrant vagrant 128 Sep 19 14:26 inventory
drwxr-xr-x. 2 vagrant vagrant   6 Sep 19 14:26 playbooks
```

Voici le contenu du playbook myvars1.yml qui affiche les variables mycar et mybike :
```yaml
---
- name: Afficher voiture et moto préférées
  hosts: localhost
  vars:
      mycar: "Renault"
      mybike: "KTM"
  tasks:
      - name: Afficher les variables
      debug:
          msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}."
```
Voici le contenu du playbook myvars2.yml qui affiche les variables mycar et mybike avec set_fact :
```yaml
---
- name: Afficher voiture et moto préférées avec set_fact
  hosts: localhost
  tasks:
    - name: Définir les variables
      set_fact:
        mycar: "Renault"
        mybike: "KTM"

    - name: Afficher les variables
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}."
```

Voici le contenu du playbook myvars3.yml qui affiche les variables mycar et mybike sans les définir :
```yaml
[all:vars]
mycar=Renault
mybike=KTM

[target02:vars]
mycar=Mercedes
mybike=Suzuki
```
```yaml
---
- name: Afficher voiture et moto préférées sans les définir
  hosts: all
  tasks:
    - name: Afficher les variables
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}."

```

Voici le contenu du playbook display_user.yml qui affiche un utilisateur et son mot de passe :

```yaml
---
- name: Afficher utilisateur et mot de passe
  hosts: localhost
  vars_prompt:
    - name: "user"
      prompt: "Entrez le nom d'utilisateur"
      default: "kiki"
      private: no

    - name: "password"
      prompt: "Entrez le mot de passe"
      default: "ansiblelife"
      private: yes

  tasks:
    - name: Afficher les informations
      debug:
        msg: "Utilisateur : {{ user }}, Mot de passe : {{ password }}"

```
Voici le résultat de l'exécution du playbook display_user.yml, il garde la valeur par défaut car je n'ai pas entré de valeur : 
```bash
[vagrant@control playbooks]$ ansible-playbook display_user.yml 
Entrez le nom d'utilisateur [kiki]: test
Entrez le mot de passe [ansiblelife]:

PLAY [Afficher utilisateur et mot de passe] **************************************************************************************************************************************************************************

TASK [Gathering Facts] ***********************************************************************************************************************************************************************************************
ok: [localhost]

TASK [Afficher les informations] *************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Utilisateur : test, Mot de passe : ansiblelife"
}

PLAY RECAP ***********************************************************************************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

---
---
---

# Partie 8: Ansible par la pratique (13) – Variables enregistrées

## Exercice

Go dans le répertoire playbook :
```bash
cd ansible/projets/ema/playbooks/
```


Voici le contenu du playbook kernel.yml qui enregistre la date et l'heure actuelles dans la variable current_time :

```yml
---
- hosts: all
  gather_facts: false
  tasks:
    - name: Récupérer les informations du noyau avec uname -a
      command: uname -a
      register: kernel_info
      changed_when: false

    - name: Afficher les infos du noyau avec debug msg
      debug:
        msg: "Kernel info: {{ kernel_info.stdout }}"

    - name: Afficher les infos du noyau avec debug var
      debug:
        var: kernel_info.stdout_lines
```


```bash
ansible-playbook kernel.yml
```
Voici le résultat de l'exécution du playbook kernel.yml :

```bash
[vagrant@ansible playbooks]$ ansible-playbook kernel.yml

PLAY [all] ************************************************************************************************************************

TASK [Récupérer les informations du noyau avec uname -a] **************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Afficher les infos du noyau avec debug msg] *********************************************************************************
ok: [rocky] => {
    "msg": "Kernel info: Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
}
ok: [debian] => {
    "msg": "Kernel info: Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
}
ok: [suse] => {
    "msg": "Kernel info: Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
}

TASK [Afficher les infos du noyau avec debug var] *********************************************************************************
ok: [rocky] => {
    "kernel_info.stdout_lines": [
        "Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
    ]
}
ok: [debian] => {
    "kernel_info.stdout_lines": [
        "Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
    ]
}
ok: [suse] => {
    "kernel_info.stdout_lines": [
        "Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
    ]
}

PLAY RECAP ************************************************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

Je creer un playbook packages.yml qui compte le nombre total de paquets RPM installés sur les machines rocky et suse :

```yml
---
- hosts: rocky,suse
  gather_facts: false
  tasks:
    - name: Compter le nombre total de paquets RPM installés
      shell: "rpm -qa | wc -l"
      register: rpm_count
      changed_when: false

    - name: Afficher le nombre total de paquets RPM installés
      debug:
        msg: "Nombre total de paquets RPM: {{ rpm_count.stdout | trim }}"
```
```bash
ansible-playbook packages.yml
```

Voici le résultat de l'exécution du playbook packages.yml :

```bash
[vagrant@ansible playbooks]$ ansible-playbook packages.yml

PLAY [rocky,suse] *****************************************************************************************************************

TASK [Compter le nombre total de paquets RPM installés] ***************************************************************************
ok: [rocky]
ok: [suse]

TASK [Afficher le nombre total de paquets RPM installés] **************************************************************************
ok: [rocky] => {
    "msg": "Nombre total de paquets RPM: 671"
}
ok: [suse] => {
    "msg": "Nombre total de paquets RPM: 917"
}

PLAY RECAP ************************************************************************************************************************
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

---
---
---

# Partie 9: Ansible par la pratique (14) – Facts et variables implicites

## Exercice

Go dans le répertoire playbook :
```bash
cd ansible/projets/ema/playbooks/
```

Je creer un playbook info-packages.yml qui affiche les informations sur le gestionnaire de paquets utilisé :
```yml
---
- hosts: all
  tasks:
    - name: Afficher le gestionnaire de paquets utilisé
      debug:
        msg: "Le host {{ inventory_hostname }} utilise le gestionnaire de paquets: {{ ansible_pkg_mgr }}"
```


Voici le résultat de l'exécution du playbook info-packages.yml :

```bash
[vagrant@ansible playbooks]$ ansible-playbook info-packages.yml

PLAY [all] ************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Afficher le gestionnaire de paquets utilisé] ********************************************************************************
ok: [rocky] => {
    "msg": "Le host rocky utilise le gestionnaire de paquets: dnf"
}
ok: [debian] => {
    "msg": "Le host debian utilise le gestionnaire de paquets: apt"
}
ok: [suse] => {
    "msg": "Le host suse utilise le gestionnaire de paquets: zypper"
}

PLAY RECAP ************************************************************************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


Je creer un playbook info-python-version.yml qui affiche la version de Python installée sur toutes les machines :
```yml
---
- hosts: all
  tasks:
    - name: Afficher la version de Python installée
      debug:
        msg: "Host {{ inventory_hostname }} utilise Python version: {{ ansible_python.version.major }}.{{ ansible_python.version.minor }}.{{ ansible_python.version.micro }}"
```


Voici le résultat de l'exécution du playbook info-python-version.yml :

```bash
[vagrant@ansible playbooks]$ ansible-playbook info-python-version.yml

PLAY [all] ************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Afficher la version de Python installée] ************************************************************************************
ok: [rocky] => {
    "msg": "Host rocky utilise Python version: 3.9.18"
}
ok: [debian] => {
    "msg": "Host debian utilise Python version: 3.11.2"
}
ok: [suse] => {
    "msg": "Host suse utilise Python version: 3.6.15"
}

PLAY RECAP ************************************************************************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


Je cree un playbook dns-check.yml qui affiche les serveurs DNS configurés sur toutes les machines :
```yml
---
- hosts: all
  tasks:
    - name: Afficher les serveurs DNS configurés
      debug:
        msg: "Host {{ inventory_hostname }} utilise les serveurs DNS: {{ ansible_dns.nameservers }}"
```

Voici le résultat de l'exécution du playbook dns-check.yml :

```bash
[vagrant@ansible playbooks]$ ansible-playbook dns-check.yml

PLAY [all] ************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Afficher les serveurs DNS configurés] ***************************************************************************************
ok: [rocky] => {
    "msg": "Host rocky utilise les serveurs DNS: ['10.0.2.3']"
}
ok: [debian] => {
    "msg": "Host debian utilise les serveurs DNS: ['4.2.2.1', '4.2.2.2', '208.67.220.220']"
}
ok: [suse] => {
    "msg": "Host suse utilise les serveurs DNS: ['10.0.2.3']"
}

PLAY RECAP ************************************************************************************************************************
debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


# Partie 10:Ansible par la pratique (15) – Cibles hétérogènes

## Exercice

Go dans le répertoire playbook :
```bash
cd ansible/projets/ema/playbooks/
```

Je creer un playbook chrony-01.yml qui installe et configure Chrony sur les machines Debian/Ubuntu, Rocky Linux et openSUSE Leap :
```yml
---
- hosts: all
  become: true
  tasks:
    - name: Update cache on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
        state: present
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky Linux
      dnf:
        name: chrony
        state: present
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on openSUSE Leap
      zypper:
        name: chrony
        state: present
      when: ansible_distribution == "openSUSE Leap"

    - name: Copy Chrony configuration on Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_os_family == "Debian"
      notify: Restart Chrony service

    - name: Copy Chrony configuration on Rocky/openSUSE Leap
      copy:
        dest: /etc/chrony.conf
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      when: ansible_distribution in ["Rocky", "openSUSE Leap"]
      notify: Restart Chrony service

    - name: Start and enable Chrony on Debian/Ubuntu
      service:
        name: chrony
        state: started
        enabled: true
      when: ansible_os_family == "Debian"

    - name: Start and enable Chrony on Rocky/openSUSE Leap
      service:
        name: chronyd
        state: started
        enabled: true
      when: ansible_distribution in ["Rocky", "openSUSE Leap"]

  handlers:
    - name: Restart Chrony service
      service:
        name: "{{ 'chrony' if ansible_os_family == 'Debian' else 'chronyd' }}"
        state: restarted
```

Voici le résultat de l'exécution du playbook chrony-01.yml :
```bash
[vagrant@ansible playbooks]$ ansible-playbook chrony-01.yml

PLAY [all] ************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]
ok: [ubuntu]

TASK [Update cache on Debian/Ubuntu] **********************************************************************************************
skipping: [rocky]
skipping: [suse]
ok: [ubuntu]
changed: [debian]

TASK [Install Chrony on Debian/Ubuntu] ********************************************************************************************
skipping: [rocky]
skipping: [suse]
changed: [debian]
changed: [ubuntu]

TASK [Install Chrony on Rocky Linux] **********************************************************************************************
skipping: [suse]
skipping: [debian]
skipping: [ubuntu]
ok: [rocky]

TASK [Install Chrony on openSUSE Leap] ********************************************************************************************
skipping: [rocky]
skipping: [debian]
skipping: [ubuntu]
changed: [suse]

TASK [Copy Chrony configuration on Debian/Ubuntu] *********************************************************************************
skipping: [rocky]
skipping: [suse]
changed: [debian]
changed: [ubuntu]

TASK [Copy Chrony configuration on Rocky/openSUSE Leap] ***************************************************************************
skipping: [debian]
skipping: [ubuntu]
changed: [suse]
changed: [rocky]

TASK [Start and enable Chrony on Debian/Ubuntu] ***********************************************************************************
skipping: [rocky]
skipping: [suse]
ok: [ubuntu]
ok: [debian]

TASK [Start and enable Chrony on Rocky/openSUSE Leap] *****************************************************************************
skipping: [debian]
skipping: [ubuntu]
ok: [rocky]
changed: [suse]

RUNNING HANDLER [Restart Chrony service] ******************************************************************************************
changed: [debian]
changed: [ubuntu]
changed: [rocky]
changed: [suse]

PLAY RECAP ************************************************************************************************************************
debian                     : ok=6    changed=4    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0   
rocky                      : ok=5    changed=2    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0   
suse                       : ok=5    changed=4    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0   
ubuntu                     : ok=6    changed=3    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0   

```

Je creer un playbook chrony-02.yml qui installe et configure Chrony sur les machines Debian/Ubuntu, Rocky Linux et openSUSE Leap :
```yml
---
- hosts: all
  become: true
  tasks:
    - name: Set Chrony variables for Debian/Ubuntu
      set_fact:
        chrony_package: chrony
        chrony_service: chrony
        chrony_confdir: /etc/chrony
      when: ansible_os_family == "Debian"

    - name: Set Chrony variables for Rocky Linux
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc
      when: ansible_distribution == "Rocky"

    - name: Set Chrony variables for openSUSE Leap
      set_fact:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc
      when: ansible_distribution == "openSUSE Leap"

    - name: Install Chrony using generic package module
      package:
        name: "{{ chrony_package }}"
        state: present

    - name: Copy Chrony configuration file
      copy:
        dest: "{{ chrony_confdir }}/chrony.conf"
        mode: '0644'
        content: |
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Restart Chrony service

    - name: Start and enable Chrony service
      service:
        name: "{{ chrony_service }}"
        state: started
        enabled: true

  handlers:
    - name: Restart Chrony service
      service:
        name: "{{ chrony_service }}"
        state: restarted
```

Voici le résult de l'exécution du playbook chrony-02.yml :
```bash
[vagrant@ansible playbooks]$ ansible-playbook chrony-02.yml

PLAY [all] ************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************
ok: [debian]
ok: [suse]
ok: [ubuntu]
ok: [rocky]

TASK [Set Chrony variables for Debian/Ubuntu] *************************************************************************************
skipping: [rocky]
ok: [debian]
skipping: [suse]
ok: [ubuntu]

TASK [Set Chrony variables for Rocky Linux] ***************************************************************************************
ok: [rocky]
skipping: [debian]
skipping: [suse]
skipping: [ubuntu]

TASK [Set Chrony variables for openSUSE Leap] *************************************************************************************
skipping: [rocky]
skipping: [debian]
ok: [suse]
skipping: [ubuntu]

TASK [Install Chrony using generic package module] ********************************************************************************
ok: [suse]
ok: [debian]
ok: [rocky]
ok: [ubuntu]

TASK [Copy Chrony configuration file] *********************************************************************************************
ok: [ubuntu]
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Start and enable Chrony service] ********************************************************************************************
ok: [ubuntu]
ok: [debian]
ok: [rocky]
ok: [suse]

PLAY RECAP ************************************************************************************************************************
debian                     : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
rocky                      : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
suse                       : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
ubuntu                     : ok=5    changed=0    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   


```

