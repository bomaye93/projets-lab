# 🖥️ Supervision avec Netdata

## 📌 Objectif
Installer Netdata sur une machine Linux pour surveiller les ressources système (CPU, RAM, disques, services...) en temps réel via interface web.

## 🧰 Stack
- OS : Debian / Alpine
- Outil : Netdata (supervision)
- Web UI : http://localhost:19999

## 🚀 Installation (exemple Debian)
```bash
bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

## 🔧 Commandes utiles
| Commande                        | Explication                             |
|--------------------------------|-----------------------------------------|
| `netdata`                      | Démarrer manuellement Netdata           |
| `systemctl status netdata`     | Vérifie si Netdata tourne en service    |
| `http://localhost:19999`       | Accès à l’interface web                 |

## ✅ Résultat
Interface web temps réel de supervision système (CPU, mémoire, disques, etc.)
