# 🚦 Projet M431 - Infrastructure SSL Automatique

<p align="center">
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/Traefik-24A1C1?style=for-the-badge&logo=traefik&logoColor=white" alt="Traefik">
  <img src="https://img.shields.io/badge/SSL-Let's%20Encrypt-003A70?style=for-the-badge&logo=letsencrypt&logoColor=white" alt="SSL">
  <img src="https://img.shields.io/badge/DuckDNS-Duck-orange?style=for-the-badge" alt="DuckDNS">
</p>

## 📝 Présentation

Ce dépôt contient l'implémentation technique pour le projet du **module M431**. L'objectif est de mettre en place une infrastructure réseau conteneurisée capable de générer et renouveler automatiquement des certificats **SSL** via un challenge DNS.

🔗 **Lien important :** Ce projet est le pendant technique du [Wiki M431-v2](https://github.com/Snaky21/projet-m431_wiki-v2) qui contient toute la documentation explicative.

## 🏗️ Architecture des Services

L'infrastructure est découpée en plusieurs modules indépendants mais connectés via un réseau virtuel unique :

| Service | Rôle | Image |
| :--- | :--- | :--- |
| **Traefik** | Reverse Proxy & Gestionnaire SSL | `traefik:v3.6` |
| **DuckDNS** | Mise à jour IP Dynamique (DDNS) | `linuxserver/duckdns` |
| **Portainer** | Administration de l'orchestrateur | `portainer-ce` |
| **Homepage** | Tableau de bord utilisateur | `gethomepage` |

## 🚀 Guide de Démarrage

### 1️⃣ Prérequis
- Avoir installé **Docker** et **Docker Compose**.
- Posséder un compte et un token sur [DuckDNS](https://www.duckdns.org/).
- Avoir ouvert les ports **80** et **443** sur votre routeur si vous souhaitez un accès externe.

### 2️⃣ Préparation de l'environnement
Créez le réseau Docker partagé :
```bash
docker network create proxy-net
```

Configurez vos variables d'environnement :
```bash
cp .env.example .env
# Modifiez le .env avec vos propres tokens et domaines
nano .env
```

### 3️⃣ Déploiement
Déployez les services par étape pour assurer le bon démarrage du proxy :

```bash
# 1. Le cœur de l'infrastructure (Traefik + DNS)
cd traefik && docker-compose up -d
cd ../duckdns && docker-compose up -d
# 2. L'interface d'administration
cd ../portainer && docker-compose up -d

# 3. Le portail d'accueil
cd ../homepage && docker-compose up -d
```

## ⚙️ Configuration SSL (Traefik)
Le certificat SSL est généré automatiquement via Let's Encrypt en utilisant le **DNS Challenge** de DuckDNS. Cela permet :
- De ne pas avoir besoin d'ouvrir le port 80 pour la validation.
- De générer des certificats valides même pour des services internes non exposés.

## 📖 Documentation Détaillée
Toutes les explications sur le fonctionnement de Traefik, les labels Docker et la structure du réseau sont disponibles sur le Wiki :

👉 [**Consulter le Wiki du Projet**](https://github.com/Snaky21/projet-m431_wiki-v2)

---
*Développé par [Snaky21](https://github.com/Snaky21) pour le module 431.*
