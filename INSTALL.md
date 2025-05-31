# Guide d'installation

## 1. Installation WireGuard
```bash
apt update && apt install wireguard -y
```

- Génération des clés WireGuard : 

Sur le Serveur :

```bash
cd /etc/wireguard
sudo umask 077
sudo wg genkey | tee server_private.key | wg pubkey > server_public.key
```

Sur le Client : 

```bash
cd /etc/wireguard
sudo umask 077
sudo wg genkey | tee client_private.key | wg pubkey > client_public.key
```


## 2. Configuration VPN
Copier les fichiers config/serveur-wg0.conf et client-wg0.conf dans /etc/wireguard/.

Démarrer le service :

```bash
sudo wg-quick up wg0
```

## 3. Configuration Samba (NAS)

```bash
apt install samba -y
mkdir -p /srv/partage
chown nobody:nogroup /srv/partage
```

Éditer /etc/samba/smb.conf :

```bash
[partage]
   path = /srv/partage
   browsable = yes
   read only = no
   guest ok = yes
```

Créer un utilisateur :

```bash
adduser erwan
smbpasswd -a erwan
systemctl restart smbd
```
## 4. Sur la VM serveur, configurer la redirection du port UDP 51820.

Permettre à un client VPN d’atteindre le serveur WireGuard qui tourne dans la VM, via l’IP publique de la machine hôte.

- Étapes :
  1. Dans VirtualBox → Paramètres de la VM Serveur → Réseau → Avancé → Redirection de ports

  2. Ajouter une règle :

    - Protocole : UDP

    - Port hôte : 51820 (ou un autre si déjà utilisé)

    - Adresse hôte : vide (ou 127.0.0.1)

    - Port invité : 51820

    - Adresse invité : 10.0.2.15 (ou l'IP de enp0s3 de la VM serveur)

- Résultat :
  - Le client VPN peut se connecter au serveur WireGuard via :

```bash
endpoint = [IP publique de l’hôte]:51820
```

- VirtualBox redirige ce trafic vers le port 51820 de la VM serveur.

## En résumé :
- Redirection de port WireGuard UDP 51820 vers la VM serveur

- Permettre à un client distant de joindre le serveur VPN via l’adresse IP de la machine hôte. Indispensable dans un contexte NAT.

Sans redirection de port, la machine cliente ne pourra jamais initier la connexion VPN si le serveur est derrière un NAT, comme dans VirtualBox.

## Réseau

- Serveur VM :
  - Adaptateur 1 : NAT (10.0.2.15)
  - Adaptateur 2 : Réseau interne VirtualBox "intnet" (192.168.100.1/24)

- Client VM :
  - Adaptateur 1 : NAT ou intnet
  - Adaptateur 2 : Réseau interne VirtualBox "intnet" (192.168.100.2/24)

## 5. Accès au NAS depuis la VM cliente

```bash
mkdir /mnt/nas
mount -t cifs //192.168.100.1/partage /mnt/nas -o username=erwan,password=erwan
```

Pour plus d’informations, voir la documentation dans /docs.




