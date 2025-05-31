# Bonnes pratiques

- **VPN obligatoire** : seul le trafic VPN peut accéder au NAS.
- **Accès limité** : utilisateurs définis (ex : erwan) pour éviter les partages ouverts.
- **Mots de passe forts** : `smbpasswd` obligatoire.
- **SMBv1 désactivé** : protocole obsolète désactivé par défaut.
- **SSH sécurisé** : pas de mot de passe root, connexion uniquement par clé.
- **MAJ régulières** :
```bash
apt update && apt upgrade -y
