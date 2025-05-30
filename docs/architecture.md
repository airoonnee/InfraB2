# Documentation d'Architecture

## Explication :

La communication entre les deux VM (client et serveur) se fait en réseau interne (intnet).

- L’adaptateur réseau en mode intnet permet aux deux VM de communiquer directement entre elles, comme si elles étaient sur le même réseau local privé, isolé du reste du monde.

- C’est sur cette interface que tu as configuré les adresses IP internes :

  - Serveur : 192.168.100.1

  - Client : 192.168.100.2

- C’est également via cette interface que le tunnel VPN WireGuard est établi.

### Et le mode NAT ?

- Le mode NAT sur enp0s3 est utilisé uniquement pour que chaque VM ait un accès internet, indépendamment, via la machine hôte.

- Il n’est pas utilisé pour la communication entre les deux VM.

## Objectif : 

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

## Services

- WireGuard VPN sur port UDP 51820
- SSH pour le transfert sécurisé de fichiers

## Répartition

- Le serveur écoute les connexions WireGuard et SSH sur son interface réseau interne.

- Le client établit une connexion VPN WireGuard vers le serveur en utilisant son IP publique (simulée via NAT et redirection de port).
