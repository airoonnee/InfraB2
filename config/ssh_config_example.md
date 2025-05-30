# Exemple configuration SSH serveur

- Assurez-vous que le serveur SSH est installé et actif :

```bash
sudo systemctl status ssh
```

- Vérifiez que le serveur écoute sur l'interface VPN :

```bash
ss -tuln | grep ssh
```

