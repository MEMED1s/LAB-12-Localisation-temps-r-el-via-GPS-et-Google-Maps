# Rapport de TP : Application de Géolocalisation (Android, PHP, MySQL)

**Réalisé par :** Maroua Hamdi

---

## 🎯 Objectif du Lab
L'objectif de ce projet est de concevoir et développer une solution complète de suivi de géolocalisation. L'architecture repose sur une application mobile Android capable de récupérer les coordonnées GPS de l'appareil et de les transmettre via des requêtes HTTP à une API backend en PHP. Les données sont ensuite stockées dans une base de données MySQL et restituées visuellement sur une carte interactive via l'API Google Maps.

## 🛠️ Architecture et Technologies Utilisées
* **Frontend Mobile :** Android (Java), interface XML, API Google Maps.
* **Réseau / API HTTP :** Bibliothèque Volley (requêtes POST/GET).
* **Backend Serveur :** PHP 8 (Architecture Modèle, DAO, Service), requêtes préparées PDO.
* **Base de données :** MySQL (hébergé via Laragon).

---

## 🚀 Étape 1 : Base de données (MySQL)

La première étape a consisté à préparer l'environnement de stockage. Nous avons créé une base de données nommée `localisation` contenant une table `position`. 

La structure de la table permet de stocker les informations essentielles du suivi : un identifiant unique, la latitude, la longitude, la date et l'heure exactes du relevé, ainsi que l'identifiant unique de l'appareil (IMEI ou Android ID).

<img width="595" height="232" alt="s6" src="https://github.com/user-attachments/assets/bee2ece3-28af-4f54-a1a7-7573761b0199" />

*Figure 1 : Table `position` initialisée et prête à recevoir les données.*

---

## ⚙️ Étape 2 : Développement du Backend (API PHP)

Le backend a été structuré en suivant les bonnes pratiques de séparation des préoccupations (Pattern DAO). L'arborescence du projet a été mise en place sur notre serveur local (Laragon).

* **`classe/Position.php` :** Modèle objet représentant une position.
* **`connexion/Connexion.php` :** Gestion de la connexion sécurisée à MySQL via PDO.
* **`dao/IDao.php` & `service/PositionService.php` :** Interface et implémentation des opérations CRUD.
* **API Endpoints :**
    * `createPosition.php` : Reçoit les requêtes POST d'Android et insère les données en base.
    * `showPositions.php` : Interroge la base et renvoie l'historique complet au format JSON.

<img width="759" height="392" alt="s5" src="https://github.com/user-attachments/assets/e8b7d8ba-cd46-4f3a-838d-0bf3d22ab817" />

*Figure 2 : Arborescence des fichiers PHP déployés sur le serveur Laragon.*

---

## 📱 Étape 3 : Application Android (GPS et Volley)

L'application Android est le cœur de la collecte de données. 

1.  **Gestion des Permissions :** L'application demande dynamiquement l'autorisation d'accéder à la localisation précise (`ACCESS_FINE_LOCATION`) et vérifie l'état du réseau.
2.  **LocationManager :** Un écouteur (`LocationListener`) est mis en place pour récupérer les coordonnées dès que l'appareil se déplace.
3.  **Transmission via Volley :** À chaque nouvelle position détectée, une requête HTTP POST est générée. Les données (latitude, longitude, date, ID) sont formatées puis envoyées vers notre script `createPosition.php`.

<img width="1336" height="567" alt="s3" src="https://github.com/user-attachments/assets/8f8b0ade-9385-4211-87fd-6e9ec15c6b5a" />

*Figure 3 : Implémentation de la logique de localisation et des requêtes Volley dans `MainActivity.java`.*

L'interface principale affiche en temps réel les coordonnées captées par le téléphone avant leur envoi vers le serveur.

<img width="471" height="597" alt="s2" src="https://github.com/user-attachments/assets/90a55be8-b1f9-4568-bac3-67541bd4d6f4" />


*Figure 4 : Interface principale affichant la latitude et la longitude récupérées.*

---

## 🗺️ Étape 4 : Intégration de Google Maps

La dernière phase du projet consiste à exploiter les données stockées pour les afficher visuellement. Nous avons intégré le SDK Google Maps pour Android.

**Configuration de la clé API :**
Pour autoriser l'application à charger les cartes, une clé API Google Cloud a été générée et intégrée de manière sécurisée dans le fichier `AndroidManifest.xml` via la balise `<meta-data>`.

<img width="828" height="298" alt="s4" src="https://github.com/user-attachments/assets/e0506220-dd95-4579-ac64-06a5fda4824a" />

*Figure 5 : Déclaration de la clé API Google Maps dans le Manifest Android.*

**Restitution Visuelle :**
La `MapsActivity` lance une requête via Volley vers notre script `showPositions.php`. Le JSON reçu est parsé, et pour chaque entrée, un objet `MarkerOptions` est créé avec les coordonnées correspondantes. La caméra effectue ensuite un zoom automatique sur le dernier relevé.

<img width="1102" height="1427" alt="s1" src="https://github.com/user-attachments/assets/7f04a052-4719-44c5-a957-3d30ce3764f9" />

*Figure 6 : Rendu final de l'application affichant la position enregistrée sur Google Maps.*

---

## ✅ Conclusion
Ce laboratoire a permis de valider la maîtrise d'un flux de données complet "Full-Stack Mobile". De la captation hardware (puce GPS) à l'affichage cartographique, en passant par le transit réseau (HTTP/JSON) et la persistance des données (PHP/MySQL), l'ensemble de l'architecture communique avec succès.
