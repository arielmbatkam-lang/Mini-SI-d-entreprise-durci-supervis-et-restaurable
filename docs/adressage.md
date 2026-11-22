| ID VLAN | Nom du VLAN | Interface OPNsense | Sous-réseau | Passerelle (OPNsense) | Serveur DHCP | Plage DHCP | Rôle et périmètre |
| :---: | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **10** | **Management** | `vtnet1.10` | `10.10.10.0/24` | `10.10.10.1` | Désactivé (IP fixes) | — | Administration Proxmox, OPNsense, Wazuh, Zabbix/Grafana |
| **20** | **Serveurs** | `vtnet1.20` | `10.10.20.0/24` | `10.10.20.1` | Désactivé (IP fixes) | — | Contrôleur de domaine (AD/DNS/DHCP), Services Web (GLPI) |
| **30** | **Postes** | `vtnet1.30` | `10.10.30.0/24` | `10.10.30.1` | **Windows Server (AD)** | `10.10.30.100` – `.200` | Postes clients utilisateurs joints au domaine (Windows 10/11) |
| **40** | **Attaquant** | `vtnet1.40` | `10.10.40.0/24` | `10.10.40.1` | OPNsense | `10.10.40.100` – `.150` | Machine d'attaque (Kali Linux) et cible web isolée (DVWA) |
| — | **VPN WireGuard** | `wg0` | `10.10.99.0/24` | `10.10.99.1` | — | Attribution statique | Accès distant administrateur vers le VLAN Management |
