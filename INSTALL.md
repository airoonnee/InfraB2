# Installation et Déploiement

## Prérequis

- Machines Ubuntu 22.04 (serveur et client)
- VirtualBox installé avec VMs configurées (NAT + réseau interne "intnet")

## Étapes

1. Installer WireGuard :

```bash
sudo apt update
sudo apt install wireguard
```

2. Génération des clés WireGuard : 

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

3. Configurer les fichiers /etc/wireguard/wg0.conf sur serveur et client (cf. dossier config).

4. Sur la VM serveur, configurer la redirection du port UDP 51820.

5. Démarrer WireGuard :

```bash
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```


6. Tester la connectivité VPN :

```bash
ping 192.168.100.1
```

Pour plus d’informations, voir la documentation dans /docs.




