\#  Dimensionnement de l'Infrastructure (Mini-SI)



> \*\* Objectif :\*\* Faire tourner le cœur de l'infrastructure en continu avec une marge de sécurité sur l'hôte (\~1 Go), et n'allumer les machines de test qu'à la demande.



\---



\## 1. Allocation des ressources (Prévu vs Réel)



| Machine | Type | vCPU | RAM prévue | RAM réelle (mesurée) | Disque prévu | Disque réel | Mode d'exécution | Rôle |

| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |

| \*\*OPNsense\*\* | `VM` | 2 | 1,5 Go | \*À mesurer\* | 12 Go | \*À mesurer\* | 🟢 Permanent (24/7) | Pare-feu, routage inter-VLAN, IDS Suricata, VPN WireGuard |

| \*\*Windows Server 2022\*\* | `VM` | 2 | 3,0 Go | \*À mesurer\* | 40 Go | \*À mesurer\* | 🟢 Permanent (24/7) | AD DS, DNS, DHCP, GPO, PKI (AD CS) |

| \*\*Wazuh (All-in-One)\*\* | `VM` | 2 | 4,0 Go | \*À mesurer\* | 30 Go | \*À mesurer\* | 🟢 Permanent (24/7) | SIEM / XDR (Indexeur + Serveur + Dashboard) |

| \*\*Services Web (GLPI)\*\* | `LXC` | 1 | 1,5 Go | \*À mesurer\* | 8 Go | \*À mesurer\* | 🟢 Permanent (24/7) | Debian 12, Nginx/Apache, MariaDB, GLPI |

| \*\*Supervision\*\* | `LXC` | 1 | 1,0 Go | \*À mesurer\* | 6 Go | \*À mesurer\* | 🟢 Permanent (24/7) | Debian 12, Zabbix Server + Grafana |

| \*\*Cible Web (DVWA)\*\* | `LXC` | 1 | 0,5 Go | \*À mesurer\* | 4 Go | \*À mesurer\* | 🟡 À la demande | Debian 12, cible vulnérable (isolée) pour scénarios ATT\&CK |

| \*\*Client Windows 10/11\*\* | `VM` | 2 | 3,0 Go | \*À mesurer\* | 30 Go | \*À mesurer\* | 🟡 À la demande | Poste utilisateur joint au domaine (tests GPO, Sysmon) |

| \*\*Kali Linux\*\* | `VM` | 2 | 2,0 Go | \*À mesurer\* | 20 Go | \*À mesurer\* | 🟡 À la demande | Machine d'attaque (scénarios Purple Team) |

| \*\*Total infrastructure\*\* | — | — | \*\*16,5 Go\*\* | — | \*\*150 Go\*\* | — | — | — |

| \*\*Total permanent (H24)\*\* | — | — | \*\*11,0 Go\*\* | — | \*\*96 Go\*\* | — | — | Cœur du SI actif en continu |



\---



\## 2. Justification des choix techniques



\* \*\*Privilégier les conteneurs LXC (3 conteneurs) :\*\* Évite la surcharge d'un noyau complet par service Linux (économie d'environ 1,5 à 2 Go de RAM par rapport à des VM classiques).

\* \*\*Wazuh à 4 Go :\*\* Minimum recommandé pour éviter les crashs Java (`OutOfMemory`) de l'indexeur OpenSearch.

&#x20; > \*Plan B :\* Brider la JVM (`jvm.options`) à 1,5 Go et passer la VM à 3 Go avec une rétention de logs de 7 jours si l'hôte sature.

\* \*\*Windows Server sans interface graphique (ou minimale) :\*\* 3 Go suffisent pour AD DS, DNS et AD CS tant que le serveur n'héberge pas d'applications lourdes.

