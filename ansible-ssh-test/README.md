# 🤖 Test Ansible – Connexion SSH

## 📌 Objectif
Tester la connectivité SSH entre une machine hôte et une VM cible avec Ansible.

## 🚀 Commandes
```bash
ansible -i hosts all -m ping
```

### Erreur possible :
```
Using a SSH password instead of a key is not possible...
```

### 🔧 Solution :
Ajouter la VM cible au `known_hosts` :
```bash
ssh-keyscan -H ip_vm >> ~/.ssh/known_hosts
```

Utiliser des clés SSH plutôt que mot de passe.
