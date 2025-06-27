# 🕹️ Docker Retro Gaming

## 📌 Objectif
Créer un conteneur Docker avec interface de jeux rétro via navigateur.

## 🧰 Stack
- Docker
- Image : docker.io/some-retro-emu
- Interface Web

## 🚀 Exemple de commande
```bash
docker run -d -p 8080:80 retro-container
```

## 📚 Notes
Prévoir des ROMs dans un dossier monté avec `-v` si nécessaire.
