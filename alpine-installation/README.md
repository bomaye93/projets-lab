# 🧊 Installation Alpine Linux

## 📌 Objectif
Installer Alpine dans une VM, configurer réseau, SSH, utilisateur, etc.

## 🚀 Étapes importantes
- Login : `root` (pas de mot de passe)
- Commande : `setup-alpine`
- Réponses :
  - Clavier : `fr`
  - Hostname : `alpine`
  - IP : `dhcp`
  - SSH : `openssh`
  - Root SSH login : `yes`
  - Disk : `sda` → mode `sys`

## 🔧 Vérifier le réseau :
```bash
ip a
ping google.com
```

## 📝 Notes
Ne pas oublier d’installer sur le disque avec `sys` sinon le système ne démarre pas.
