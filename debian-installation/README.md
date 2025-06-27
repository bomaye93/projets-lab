# 🐧 Installation Debian

## 📌 Objectif
Installer Debian minimal dans une VM pour test ou serveur.

## 🚀 Étapes
- ISO utilisée : netinstall ou minimal
- Partitions : tout dans `/` (débutant)
- Ajout user : `adduser`
- Ajout sudo :
```bash
apt update
apt install sudo
usermod -aG sudo yahya
```

## ✅ Résultat
Système Debian stable prêt pour SSH, scripts, supervision ou projets serveurs.
