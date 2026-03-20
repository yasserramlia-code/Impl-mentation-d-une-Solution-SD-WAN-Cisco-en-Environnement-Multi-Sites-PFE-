# 🚀 Déploiement d'une Architecture Cisco Catalyst SD-WAN pour Entreprise (PFE)

## 👤 Auteur
**Yasser RAMLIA** * Ingénieur d'État en Télécommunications et Réseaux IP (ENSTTIC)
* Master 1 Cybersécurité, Cloud et Réseaux (Sorbonne Université)

---

## 📝 Contexte du Projet
Ce dépôt documente mon Projet de Fin d'Études (PFE) intitulé : **« Déploiement d’un SD-WAN sur une infrastructure WAN traditionnelle pour les entreprises »**.

L'objectif de ce projet était de concevoir, déployer et sécuriser une architecture logicielle centralisée (Software-Defined) en remplaçant un routage WAN traditionnel. La solution **Cisco Catalyst SD-WAN (v19.2.0)** a été retenue suite à une étude comparative des leaders du marché (Gartner® Magic Quadrant™), pour sa robustesse, son intégration aux parcours CCIE et son interface de gestion centralisée.

---

## 🏗️ Topologie et Architecture Multi-Sites
L'environnement de laboratoire reproduit une infrastructure d'entreprise réaliste à l'échelle nationale (Algérie) :
* 🏢 **Site de Contrôle (Headquarters) :** Alger (Hébergement des contrôleurs vManage, vBond, vSmart).
* 🏬 **Sites Distants (Branches) :** Oran, Constantine, Sétif et Tlemcen (Routeurs vEdge).

> **Note sur le Hardware :** Face aux fortes exigences matérielles des orchestrateurs Cisco, ce lab a été déployé sur un serveur dédié haute performance fourni par l'école.

---

## 🛠️ Stack Technique et Outils
* **Orchestration SD-WAN :** Cisco Catalyst SD-WAN 19.2.0 (vManage, vSmart, vBond, vEdge)
* **Virtualisation & Emulation :** GNS3, VMware Workstation Pro 17
* **Administration & Analyse :** Solar-PuTTY (SSH distant), Wireshark (Analyse DTLSv1.2)
* **Sécurité :** Déploiement d'un IPS (Intrusion Prevention System) et filtrage d'URL sur mesure.

---

## ⚙️ Implémentation et Fonctionnalités Clés
1. **Initialisation et Sécurisation du Control Plane :**
   * Configuration de l'autorité de certification (CA Root) locale.
   * Installation des certificats et authentification mutuelle entre vManage, vBond et vSmart.
2. **Déploiement du Data Plane :**
   * Intégration et activation des routeurs vEdge sur les 4 sites distants via le processus ZTP (Zero Touch Provisioning) / Plug-and-Play.
3. **Politiques de Sécurité et Routage :**
   * Création de *Security Policies* incluant un IPS et un pare-feu applicatif (URL Filtering).
   * Simulation et routage intelligent de trafic applicatif spécifique (ex: trafic LinkedIn) sur le réseau overlay sécurisé.
4. **Validation et Troubleshooting :**
   * Tests de connectivité ICMP et applicatifs de bout en bout.
   * Capture de trames avec Wireshark pour valider le chiffrement des tunnels de contrôle (DTLSv1.2) et de données (IPSec).
     
---  

## 🚧 Défis Techniques & Limites de l'Environnement
La mise en œuvre de cette infrastructure SD-WAN émulée a mis en évidence plusieurs défis techniques majeurs, principalement liés aux exigences de la solution Cisco :
* **Goulots d'étranglement matériels :** Les orchestrateurs Cisco (notamment vManage) exigent énormément de ressources (RAM/CPU). Le déploiement initial sur des machines standards (32 Go de RAM) s'est avéré insuffisant pour supporter la charge combinée du plan de contrôle et des multiples routeurs virtuels.
* **Instabilité de la topologie (Flapping) :** En raison de l'épuisement des ressources du serveur (CPU Starvation), les routeurs virtuels peinaient à traiter les paquets de maintien de session (BFD - Bidirectional Forwarding Detection) dans les temps impartis. Cela provoquait des déconnexions intempestives des tunnels IPSec, avec des sites distants basculant continuellement entre les états "Up" et "Down".
* **Périmètre du projet :** La solution Cisco Catalyst SD-WAN étant extrêmement vaste, certaines fonctionnalités avancées n'ont pas pu être exploitées dans le temps imparti pour ce laboratoire. Des éléments comme l'intégration multi-cloud (Cisco Cloud OnRamp), l'analyse prédictive (vAnalytics) ou l'inspection profonde des paquets (DPI) à grande échelle sont restés hors périmètre.

---

## 📂 Structure de ce Dépôt
* 📁 `docs/` : Rapport de mémoire complet (PDF) et schémas d'architecture.
* 📁 `topology/` : Fichiers d'exportation de la topologie GNS3.
* 📁 `configs/` : Fichiers de configuration initiale (Startup-config) des contrôleurs et routeurs vEdge.
