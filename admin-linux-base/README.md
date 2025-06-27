# 🧑‍💻 Administration Linux – Base

## 📌 Objectif
Pratiquer les tâches d'administration système de base sur des distributions Linux (Debian, Alpine, Ubuntu...).

---

## 🧰 Stack utilisée

- OS : Debian, Alpine, Ubuntu
- Shell : Bash
- Services : SSH, systemd, rc-service, UFW / Firewalld

---

## 🔧 Commandes d'administration essentielles

### 👤 Gestion utilisateurs
```bash
adduser yahya              # Ajouter un utilisateur (Debian)
addgroup dev               # Ajouter un groupe
usermod -aG sudo yahya     # Ajouter au groupe sudo (Debian/Ubuntu)
apk add shadow             # Alpine : installer outils usermod
```

### 📁 Dossiers & permissions
```bash
chmod 755 dossier/         # Droits d'accès
chown user:group fichier   # Propriétaire
```

### 🧩 Installation de paquets
```bash
apt update && apt install htop    # Debian/Ubuntu
apk update && apk add htop        # Alpine
dnf install htop                  # Rocky/Fedora
```

### 🚀 Services
```bash
systemctl start nginx             # Debian/Rocky
systemctl enable nginx
rc-service nginx start            # Alpine
rc-update add nginx default       # Alpine : activer au démarrage
```

### 🌐 Réseau
```bash
ip a                              # Afficher interfaces
ping google.com                   # Test connectivité
cat /etc/resolv.conf              # Vérifier les DNS
```

### 🔥 Pare-feu
```bash
ufw enable && ufw allow ssh       # Ubuntu/Debian
firewall-cmd --add-service=ssh    # Rocky/Fedora
```

### 🕒 Cron & Automatisation
```bash
crontab -e
# Exemple : reboot tous les dimanches à 3h
0 3 * * 0 /sbin/reboot
```

### 🪵 Logs & surveillance
```bash
journalctl -xe                   # Logs récents (systemd)
tail -f /var/log/syslog          # Debian
tail -f /var/log/messages        # Red Hat
```

---

## 📚 Résultat
Environnement Linux prêt pour :
- Déploiement de services
- Surveillance
- Sécurité
- Tests via VM ou conteneurs
