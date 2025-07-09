
# 📂 Serveur de fichiers Samba avec montage inter-serveur (Debian ↔ Ubuntu)

## ✨ Description du projet
Mise en place d'un **partage de fichiers Samba** entre deux serveurs Linux (Debian et Ubuntu). L'objectif est de permettre au serveur Ubuntu d'accéder au dossier partagé sur la Debian comme s'il était local, en ligne de commande **et en interface graphique**.

---

## ⚖️ Prérequis
- Deux VMs (Debian + Ubuntu) avec VMware ou VirtualBox
- Configuration réseau en **"Bridged"** (pour qu'elles soient sur le même réseau)
- Accès root ou sudo sur les deux machines

---

## 🛠️ Serveur Debian (192.168.0.30)

### 1. Installation de Samba
```bash
sudo apt update
sudo apt install samba -y
```

### 2. Création du dossier partagé
```bash
sudo mkdir -p /srv/partage-interserveur
sudo chmod 777 /srv/partage-interserveur
```

### 3. Configuration de Samba
Edition du fichier de config :
```bash
sudo nano /etc/samba/smb.conf
```

Ajout à la fin du fichier :
```ini
[PartageInter]
   path = /srv/partage-interserveur
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no
```

### 4. Redémarrage du service
```bash
sudo systemctl restart smbd
```

### 5. Vérifier que le partage est actif
```bash
testparm
```

---

## 💻 Client Ubuntu (192.168.0.12)

### 1. Installation des outils clients
```bash
sudo apt update
sudo apt install cifs-utils smbclient -y
```

### 2. Création du point de montage
```bash
sudo mkdir -p /mnt/serveur-debian
```

### 3. Montage du partage Samba
```bash
sudo mount -t cifs //192.168.0.30/PartageInter /mnt/serveur-debian -o guest
```

### 4. Vérification
```bash
ls /mnt/serveur-debian
```

---

## ❌ Points de blocage rencontrés & solutions

### 1. Erreur : `mount error(115): Operation now in progress`
**Cause** : Port Samba 445 bloqué ou inaccessible  
**Solution** : Passer les deux VMs en mode **Bridged** et vérifier le réseau

### 2. Erreur : `nc: connect to 192.168.X.X port 445 failed`
**Cause** : Interface réseau Ubuntu en NAT, pas sur le même réseau que la Debian  
**Solution** : Activer le mode **Bridged** dans les paramètres VMware et relancer `dhclient`

### 3. `smbclient` ou `mount.cifs` introuvables  
**Solution** : Installer les paquets `smbclient` et `cifs-utils`

### 4. `/mnt/serveur-debian` vide  
**Cause possible** : Aucun fichier dans `/srv/partage-interserveur` sur Debian  
**Solution** : Créer un fichier test depuis Debian ou Ubuntu et revérifier

---

## 🔧 Tests de fonctionnement

### Depuis Ubuntu :
```bash
echo "depuis ubuntu" > /mnt/serveur-debian/test-ubuntu.txt
```

### Depuis Debian :
```bash
echo "depuis debian" > /srv/partage-interserveur/test-debian.txt
```

Puis vérifier que chaque fichier est visible de l'autre côté.

---

## 📊 Accès graphique (explorateur de fichiers Ubuntu)

Dans la barre d’adresse Nautilus :
```
smb://192.168.0.30/PartageInter
```
Connexion en tant que :
- Nom d'utilisateur : guest
- Mot de passe : (laisser vide)

---

## 🤔 Ce que j'ai appris
- Créer et configurer un **partage Samba public**
- Monter un partage distant sur un autre serveur Linux
- Résoudre les problèmes de réseau (Bridged, NAT, IP, port 445)
- Utiliser `smbclient`, `mount -t cifs`, `testparm`, `ufw`, `dhclient`
- Accéder à un partage Samba en **graphique** sur Ubuntu

---

## 💼 Auteur
Projet réalisé par Yahya Diawara dans le cadre de son lab Linux + Administration + Déploiement + Supervision.

---

Tu veux l’utiliser ? Clone ce repo, adapte les IPs, c’est OP ! ✅
