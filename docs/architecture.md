# Architecture réseau

## Topologie

| Client VM | --- VPN --- | Serveur NAS |
|--------|----|------|
| IP locale : 192.168.100.2 |  | IP locale : 192.168.100.1 |
| IP VPN : 10.0.0.2 |  | IP VPN : 10.0.0.1 |


## Services

| Service | VM | Rôle |
|--------|----|------|
| WireGuard | Client & Serveur | Tunnel VPN |
| Samba | Serveur NAS | Partage de fichiers |

## Répartition

- Le service Samba est accessible uniquement via le réseau VPN.
- Chaque VM est sur un réseau privé avec une IP fixe.
