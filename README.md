# Apache-Kafka-Distributed-Messaging-System

# 📡 TrackStream

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![Apache Kafka](https://img.shields.io/badge/Apache%20Kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Socket.io](https://img.shields.io/badge/Socket.io-010101?style=for-the-badge&logo=socket.io&logoColor=white)

**Système de suivi de flotte en temps réel basé sur une architecture orientée événements (Event-Driven).**

TrackStream est une application démonstratrice IoT qui simule, ingère, traite et visualise des données de positionnement GPS de véhicules à haute fréquence. Le projet illustre la puissance d'une architecture distribuée utilisant **Apache Kafka** pour le streaming de données et **Docker** pour l'orchestration.

---

## 🏗️ Architecture

Le projet est entièrement conteneurisé. Les services communiquent via un réseau Docker privé, garantissant isolation et facilité de déploiement.

### Flux de données
`Capteurs (Simulés)` ➡️ `Kafka (Ingestion)` ➡️ `Backend (Traitement)` ➡️ `Frontend (Visualisation)`

### Composants du système

1.  **Zookeeper** : Gestionnaire de configuration et de coordination pour le cluster Kafka.
2.  **Kafka Broker** : Cœur du système de messagerie distribué. Gère le topic `vehicle-tracking`.
3.  **Producer (Node.js)** : Service simulant des capteurs IoT installés sur 3 véhicules (V001, V002, V003). Il génère et envoie des coordonnées GPS en continu.
4.  **Backend (Node.js/Express)** :
    *   Consomme les messages du topic Kafka.
    *   Effectue le traitement en temps réel (calcul de vitesse moyenne glissante).
    *   Détecte les anomalies (Excès de vitesse > 70 km/h).
    *   Diffuse les données traitées au frontend via **Socket.IO**.
5.  **Frontend (React)** : Dashboard interactif affichant la position des véhicules sur une carte et les métriques de vitesse.

---

## 🚀 Fonctionnalités Clés

*   **🗺️ Géolocalisation Temps Réel** : Visualisation sur une carte **Leaflet** avec mise à jour fluide des marqueurs.
*   **📊 Analytique en Direct** : Graphiques dynamiques (**Chart.js**) affichant l'évolution de la vitesse et calcul automatique de la moyenne.
*   **🚨 Système d'Alertes** : Détection instantanée des excès de vitesse (> 70 km/h) avec notifications visuelles.
*   **💾 Historique de Trajet** : Mémorisation et affichage des 10 dernières positions et vitesses pour chaque véhicule.
*   **🎨 UI Intuitive** : Code couleur distinct pour chaque véhicule (Rouge, Vert, Or) pour une identification rapide.
*   **🐳 100% Dockerisé** : L'environnement complet se lance avec une seule commande, sans dépendances locales complexes.

---

## 🛠️ Prérequis

Avant de commencer, assurez-vous d'avoir installé :

*   [Docker Desktop](https://www.docker.com/products/docker-desktop)
*   [Docker Compose](https://docs.docker.com/compose/install/)

> **Note :** Vous n'avez **pas** besoin d'installer Node.js, Java ou Kafka sur votre machine hôte. Tout est géré dans les conteneurs.

---

## 📦 Installation et Démarrage

1.  **Cloner le dépôt**
    ```bash
    git clone https://github.com/votre-user/trackstream.git
    cd trackstream
    ```

2.  **Lancer l'application**
    Cette commande construit les images et lance les conteneurs en mode détaché.
    ```bash
    docker-compose up --build
    ```

3.  **Accéder au Dashboard**
    Ouvrez votre navigateur et naviguez vers :
    [http://localhost:3000](http://localhost:3000)

---

## 📂 Structure du Projet

```text
trackstream/
├── docker-compose.yml      # Orchestration des services
├── producer/               # Service IoT (Simulateur)
│   ├── vehicleProducer.js
│   └── Dockerfile
├── backend/                # API & Consommateur Kafka
│   ├── server.js
│   ├── kafkaConsumer.js
│   └── Dockerfile
└── frontend/               # Interface React
    ├── public/
    ├── src/
    │   ├── App.js
    │   ├── MapView.js      # Composant Carte Leaflet
    │   ├── SpeedChart.js   # Composant Graphique
    │   └── socket.js       # Configuration WebSocket
    └── Dockerfile
```

---

## 🔧 Commandes Utiles

| Action | Commande |
| :--- | :--- |
| **Voir les logs en direct** | `docker-compose logs -f` |
| **Arrêter l'application** | `docker-compose down` |
| **Arrêter et nettoyer les volumes** | `docker-compose down -v` |
| **Redémarrer un service spécifique** | `docker-compose restart [service_name]` |

---

## 🐛 Dépannage Courant

*   **Erreur `getaddrinfo ENOTFOUND kafka` :**
    *   Le conteneur Kafka peut mettre quelques secondes à démarrer. Le backend est configuré pour tenter de se reconnecter automatiquement. Attendez simplement quelques instants.
*   **La carte ne s'affiche pas :**
    *   Vérifiez votre connexion internet. La bibliothèque Leaflet charge les tuiles de carte depuis OpenStreetMap en ligne.
*   **Conflit de ports :**
    *   Assurez-vous que les ports `3000` (React), `4000` (Backend) et `9092` (Kafka) ne sont pas utilisés par d'autres applications sur votre machine.

---

## 🔮 Améliorations Futures (Roadmap)

*   [ ] **Persistance :** Stockage longue durée des trajets avec MongoDB ou InfluxDB.
*   [ ] **Authentification :** Gestion des utilisateurs et sécurisation de l'API.
*   [ ] **Time-Travel :** Fonctionnalité de "Replay" pour rejouer des trajets passés.
*   [ ] **Scalabilité :** Partitionnement du topic Kafka pour gérer des milliers de véhicules simultanément.