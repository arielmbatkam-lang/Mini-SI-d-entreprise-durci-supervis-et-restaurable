\# Dimensionnement de l'Infrastructure (Mini-SI)





> \*\*Objectif :\*\* Faire tourner le cœur de l'infrastructure en continu avec une marge de sécurité sur l'hôte (\~1 Go), et n'allumer les machines de test qu'à la demande\[cite: 1, 7].



\---



\## 1. Allocation des ressources (Prévu vs Réel)



| Machine | Type | vCPU | RAM prévue | RAM réelle (mesurée) | Disque prévu | Disque réel | Mode d'exécution | Rôle |

| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |

| \*\*OPNsense\*\* | VM | 2 | 1,5 Go\[cite: 7] | \*À mesurer\* | 12 Go\[cite: 7] | \*À mesurer\* | Permanent (24/7) | Pare-feu, routage inter-VLAN, IDS Suricata, VPN WireGuard\[cite: 7] |

| \*\*Windows Server 2022\*\* | VM | 2 | 3,0 Go\[cite: 7] | \*À mesurer\* | 40 Go\[cite: 7] | \*À mesurer\* | Permanent (24/7) | AD DS, DNS, DHCP, GPO, PKI (AD CS)\[cite: 7] |

| \*\*Wazuh (All-in-One)\*\* | VM | 2 | 4,0 Go\[cite: 4, 7] | \*À mesurer\* | 30 Go\[cite: 7] | \*À mesurer\* | Permanent (24/7) | SIEM / XDR (Indexeur + Serveur + Dashboard)\[cite: 7] |

| \*\*Services Web (GLPI)\*\* | LXC | 1 | 1,5 Go\[cite: 7] | \*À mesurer\* | 8 Go\[cite: 7] | \*À mesurer\* | Permanent (24/7) | Debian 12, Nginx/Apache, MariaDB, GLPI\[cite: 2, 3, 7] |

| \*\*Supervision\*\* | LXC | 1 | 1,0 Go\[cite: 7] | \*À mesurer\* | 6 Go\[cite: 7] | \*À mesurer\* | Permanent (24/7) | Debian 12, Zabbix Server + Grafana\[cite: 2, 4, 7] |

| \*\*Cible Web (DVWA)\*\* | LXC | 1 | 0,5 Go | \*À mesurer\* | 4 Go | \*À mesurer\* | À la demande | Debian 12, cible vulnérable (isolée) pour scénarios ATT\&CK\[cite: 2, 3, 5] |

| \*\*Client Windows 10/11\*\* | VM | 2 | 3,0 Go\[cite: 7] | \*À mesurer\* | 30 Go\[cite: 7] | \*À mesurer\* | À la demande\[cite: 1, 7] | Poste utilisateur joint au domaine (tests GPO, Sysmon)\[cite: 4, 7] |

| \*\*Kali Linux\*\* | VM | 2 | 2,0 Go\[cite: 7] | \*À mesurer\* | 20 Go\[cite: 7] | \*À mesurer\* | À la demande\[cite: 1, 7] | Machine d'attaque (scénarios Purple Team)\[cite: 5, 7] |

| \*\*Total infrastructure\*\* | — | — | \*\*16,5 Go\*\* | — | \*\*150 Go\*\* | — | — | — |

| \*\*Total permanent (H24)\*\* | — | — | \*\*11,0 Go\*\* | — | \*\*96 Go\*\* | — | — | Cœur du SI actif en continu |





\---



\## 2. Justification des choix techniques



\* \*\*Privilégier les conteneurs LXC (3 conteneurs) :\*\* Évite la surcharge d'un noyau complet par service Linux (économie d'environ 1,5 à 2 Go de RAM par rapport à des VM classiques)\[cite: 7].

\* \*\*Wazuh à 4 Go :\*\* Minimum recommandé pour éviter les crashs Java (OutOfMemory) de l'indexeur OpenSearch\[cite: 4]. \*Plan B :\* brider la JVM (`jvm.options`) à 1,5 Go et passer la VM à 3 Go avec une rétention de logs de 7 jours si l'hôte sature\[cite: 7].

\* \*\*Windows Server sans interface graphique (ou minimale) :\*\* 3 Go suffisent pour AD DS, DNS et AD CS tant que le serveur n'héberge pas d'applications lourdes\[cite: 7].



\---







