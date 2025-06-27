# 📁 Serveur de fichiers Samba

## 📌 Objectif
Mettre en place un partage de fichiers local accessible via Windows/Linux avec Samba.

## 🧰 Stack
- OS : Debian
- Service : Samba

## 🚀 Commandes
```bash
apt install samba
mkdir /srv/PartagePublic
chown nobody:nogroup /srv/PartagePublic
nano /etc/samba/smb.conf  # configurer le partage
```

Ajoute dans smb.conf :
```
[PartagePublic]
   path = /srv/PartagePublic
   guest ok = yes
   read only = no
```

Redémarre le service :
```bash
systemctl restart smbd
```

## ✅ Accès
Depuis un autre poste :
```bash
smbclient //IP_VM/PartagePublic -U guest
```
