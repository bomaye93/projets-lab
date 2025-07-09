
# 🧱 admin-linux-serveur-lab

## 🎯 Objectif du projet

Déployer et configurer un serveur Linux (Debian ou Ubuntu) avec toutes les bases d’administration :

- Gestion des utilisateurs
- Configuration des services
- Sécurisation réseau (pare-feu, SSH, Fail2Ban)
- Supervision système (Netdata)
- Automatisation (script de backup)
- Streaming de supervision avec Netdata (master/client)

## 🛠️ Stack

- Debian 12 ou Ubuntu Server 22.04
- Netdata
- UFW
- Fail2Ban
- systemd
- Bash / Crontab
- Nginx (installé pour monitoring web)
- SSH

---

## 🔧 Étapes du projet

### 1. Création utilisateur admin + accès sudo
```bash
useradd -m admin
usermod -aG sudo admin
```

### 2. Configuration du pare-feu UFW
```bash
apt install ufw
ufw enable
ufw allow OpenSSH
ufw allow 19999/tcp
ufw allow 'Nginx Full'
ufw status
```

### 3. Installation d’outils essentiels
```bash
apt install net-tools curl wget htop sudo nano fail2ban
```

### 4. Gestion de services
```bash
systemctl status ssh
systemctl status nginx
systemctl enable --now fail2ban
systemctl start|stop|restart <service>
```

### 5. Configuration SSH (sécurisation)
```bash
nano /etc/ssh/sshd_config
# Port personnalisé (optionnel)
# PermitRootLogin no
# PasswordAuthentication no
systemctl restart ssh
```

---

## 🧠 Script de sauvegarde automatique

### Emplacement :
```bash
/usr/local/bin/backup.sh
```

### Exemple de script :
```bash
#!/bin/bash
tar -czvf /home/admin/backup_$(date +%F).tar.gz /etc /home/admin
```

### Automatisation avec crontab :
```bash
crontab -e
0 2 * * * /usr/local/bin/backup.sh
```

---

## 📈 Supervision locale (Netdata)

### Installation :
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

### Vérification :
```bash
systemctl status netdata
ss -tuln | grep 19999
journalctl -u netdata -f
```

---

## 🔁 Supervision avancée (Streaming Netdata)

### Objectif :
Surveiller plusieurs serveurs depuis un seul tableau de bord (Master <— Client)

### Sur le **Master (Ubuntu)**

1. Générer une API Key :
```bash
sudo cat /opt/netdata/usr/lib/netdata/conf.d/stream.conf
```

2. Ajouter la section :
```ini
[944dbea1-dbd9-495f-990c-cf9b7c0f956e]
    enabled = yes
    default history = 3600
    default memory mode = ram
```

3. Pare-feu ouvert :
```bash
sudo ufw allow 19999/tcp
```

---

### Sur le **Client (Debian)**

1. Modifier `stream.conf` :
```bash
nano /etc/netdata/stream.conf
```

```ini
[stream]
    enabled = yes
    destination = <IP_MASTER>:19999
    api key = 944dbea1-dbd9-495f-990c-cf9b7c0f956e
    timeout seconds = 60
    buffer size = 1024
    reconnect delay seconds = 5
```

2. Redémarrer :
```bash
systemctl restart netdata
```

3. Logs :
```bash
journalctl -u netdata -f
```

---

## ⚠️ Problèmes rencontrés

- 🔒 `stream.conf` non trouvé → utilisé `sudo find / -name stream.conf`
- 🔑 mauvaise clé API → régénérée côté master
- 📶 Port 19999 bloqué → vérification UFW + `ss -tuln`
- 🔁 Boucle de test / restart Netdata
- 📂 Mauvais dossier de conf sur Ubuntu : `/opt/netdata/etc/netdata/stream.conf`

---

## ✅ Résultat final

- Netdata fonctionnel sur les deux machines
- Supervision centralisée opérationnelle
- Scripts en place
- Sécurité SSH + pare-feu + Fail2Ban activés

---

## 🧠 Conclusion

Projet terminé avec :
- Rigueur de test
- Résolution d'erreurs
- Complétude (backup, supervision, sécurité)
- 📁 Peut être poussé sur GitHub

🔥 Tu maîtrises maintenant les bases d’un vrai serveur Linux !
