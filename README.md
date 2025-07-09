# 🧰 Projet Admin Linux – Serveur de Fichiers Samba (Public + Privé)

> Mise en place d’un serveur Samba fonctionnel sur Debian, avec accès local et distant, partages publics/privés, gestion fine des droits, et test depuis une machine cliente Ubuntu.

---

## 🔧 Objectif du projet

- Installer et configurer un serveur de fichiers Samba
- Créer deux types de partages :
  - 📂 Partage **public** (accessible sans identifiant)
  - 🔐 Partage **privé** (accessible uniquement par un utilisateur Samba)
- Monter les partages à distance depuis un poste Ubuntu
- Comprendre et manipuler les permissions (`chmod`, `chown`, `smb.conf`, etc.)

---

## 🖥️ Architecture

```
[ Ubuntu Client ] ←→ [ Debian Serveur Samba ]
                           |
                        /srv/partage       (public)
                        /srv/prive         (privé pour ousmane)
```

---

## 📦 Installation de Samba sur Debian

```bash
sudo apt update
sudo apt install samba samba-common -y
```

---

## 📁 Partage public

### 📂 Création du dossier :
```bash
sudo mkdir -p /srv/partage
sudo chown nobody:nogroup /srv/partage
sudo chmod 0775 /srv/partage
```

### ⚙️ Configuration Samba :

Ajoutez à la fin de `/etc/samba/smb.conf` :
```ini
[PartagePublic]
   path = /srv/partage
   browsable = yes
   read only = no
   guest ok = yes
```

### 🔁 Redémarrage du service :
```bash
sudo systemctl restart smbd
```

---

## 🔐 Partage privé (utilisateur : ousmane)

### 👤 Création de l'utilisateur :
```bash
sudo adduser ousmane
sudo smbpasswd -a ousmane
```

### 📂 Dossier dédié :
```bash
sudo mkdir -p /srv/prive
sudo chown ousmane:ousmane /srv/prive
sudo chmod 0770 /srv/prive
```

### ⚙️ Bloc dans `/etc/samba/smb.conf` :
```ini
[PriveOusmane]
   path = /srv/prive
   valid users = ousmane
   read only = no
   browsable = yes
   create mask = 0770
   directory mask = 0770
```

---

## 🧪 Test local sur Debian :
```bash
smbclient //localhost/PriveOusmane -U ousmane
```

---

## 🧷 Accès distant depuis Ubuntu (client)

### 📦 Installer les outils :
```bash
sudo apt install cifs-utils smbclient -y
```

### 📁 Créer un dossier de montage :
```bash
mkdir ~/samba_privé
```

### 🔐 Créer un fichier de credentials :
```bash
sudo nano /etc/samba/cred_ousmane
```

```ini
username=ousmane
password=mot_de_passe_samba
```

Puis :
```bash
sudo chmod 600 /etc/samba/cred_ousmane
```

### 📌 Monter le partage :
```bash
sudo mount -t cifs //192.168.1.47/PriveOusmane ~/samba_privé -o credentials=/etc/samba/cred_ousmane,vers=3.0,sec=ntlmssp
```

> Remplace `192.168.1.47` par l’IP de ta machine Debian

---

## ✅ Résultat final

- Le partage public est accessible sans mot de passe
- Le partage privé est accessible **uniquement par `ousmane`**
- Le client Ubuntu peut monter le partage et voir les fichiers localement

---

## 💡 Bonus possible (non traité ici)

- Automatiser le montage dans `/etc/fstab`
- Partage accessible depuis Windows
- Supervision du service Samba
- Sécurisation avec `fail2ban`, auditd, etc.

---

## 🧠 Compétences mobilisées

- Administration système Linux
- Gestion des utilisateurs et permissions (`chmod`, `chown`, `smbpasswd`)
- Configuration de services (`smb.conf`)
- Debug réseau et montage distant
