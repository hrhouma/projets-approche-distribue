<br/>
<br/>


# *Résumé de la Phase 4 :*

- Configuration de l'application en deux microservices (customer et employee) et test dans des conteneurs Docker :


##  Phase 4 – Résumé général

Cette phase vise à **transformer une application monolithique en deux microservices** distincts (customer et employee), à les conteneuriser avec Docker, et à **tester leur bon fonctionnement dans des conteneurs Docker** sur AWS Cloud9. Elle inclut également la **gestion des ports**, la **sécurisation des accès**, la **modification du code source** et le **suivi des versions avec Git/CodeCommit**.



# Tâche 4.1 – Sécurisation du Cloud9

- Ouvrir les ports TCP **8080 et 8081** dans le **groupe de sécurité** de l’instance EC2 Cloud9 pour permettre l'accès aux microservices via navigateur.



# Tâche 4.2 – Modification du microservice `customer`

### Code :
- Nettoyage du contrôleur `supplier.controller.js` pour ne garder que :
  - `findAll` : afficher tous les fournisseurs.
  - `findOne` : afficher un fournisseur par ID.
- Nettoyage du modèle `supplier.model.js` pour ne garder que :
  - `Supplier.getAll`
  - `Supplier.findById`
- Modification de la navigation (`nav.html`) :
  - Ajout du lien vers l’interface d'administration.
  - Changement des libellés.
- Suppression des fichiers HTML liés aux actions d’écriture (ajout, édition).
- Modification du `index.js` :
  - Port modifié à `8080`.
  - Commenter le bloc lié à l’instance Monolithique.

### Docker :
- Création du `Dockerfile`.
- Construction de l'image Docker :  
  ```bash
  docker build --tag customer .
  ```
- Lancement du conteneur :  
  ```bash
  docker run -d --name customer_1 -p 8080:8080 -e APP_DB_HOST="$dbEndpoint" customer
  ```
- Vérification dans le navigateur via `http://<public-IP>:8080`.



# Tâche 4.3 – Dockerisation du microservice `customer`

- Vérification de l’image créée avec : `docker images`
- Vérification du conteneur actif avec : `docker ps`



# Tâche 4.4 – Modification du microservice `employee`

### Code :
- Ajout du préfixe `/admin` à toutes les routes dans :
  - `supplier.controller.js`
  - `index.js`
  - `supplier-add.html`, `supplier-update.html`
  - `supplier-list-all.html`, `home.html`
  - `nav.html`
- Changement des titres et labels dans les vues.



# Tâche 4.5 – Dockerisation du microservice `employee`

- Copie du Dockerfile, modification du port à **8081**.
- Construction de l’image :  
  ```bash
  docker build --tag employee .
  ```
- Lancement du conteneur :  
  ```bash
  docker run -d --name employee_1 -p 8081:8081 -e APP_DB_HOST="$dbEndpoint" employee
  ```
- Test dans navigateur :  
  `http://<public-IP>:8081/admin/suppliers`



# Tâche 4.6 – Préparation pour ECS

- Modification de `index.js` et `Dockerfile` du microservice `employee` pour revenir au port **8080** (standard pour ECS).
- Reconstruction de l’image.
- Suppression de l’ancien conteneur :  
  ```bash
  docker rm -f employee_1
  ```



# Tâche 4.7 – Intégration dans CodeCommit

- Utilisation de Git pour **committer et pousser** les modifications du code source vers CodeCommit.
- Vérification des différences entre commits dans l’interface AWS Cloud9 ou CodeCommit.


<br/>
<br/>

# Annexe 1 : 


## **Pourquoi configurer deux microservices séparés (customer et employee) ?**

Parce que dans une **architecture microservices**, chaque service est :
- **Responsable d’une fonction métier bien précise**
- **Autonome** et peut évoluer indépendamment
- **Déployable séparément**, ce qui réduit les risques lors des mises à jour

👉 Ici :
- `customer` = accès **lecture seule** pour les clients
- `employee` = accès **lecture + écriture** pour les employés



##  **Pourquoi modifier les ports (8080 et 8081) ?**

- Chaque conteneur Docker s'exécute sur un **port spécifique** sur la machine hôte (Cloud9).
- On ne peut pas faire tourner deux services sur le **même port** sans provoquer un conflit.
- 8080 pour `customer` et 8081 pour `employee`, puis **standardiser à 8080 pour ECS**, où chaque service a son propre environnement isolé.


##  **Pourquoi adapter le code source ?**

- Pour respecter le **principe de séparation des responsabilités** :
  - `customer` : accès limité (lecture uniquement), pas de boutons d’ajout/édition.
  - `employee` : interface complète (ajout, édition, suppression).
- Les chemins `/admin/` sont nécessaires pour que l’**Application Load Balancer (ALB)** sache **où router chaque requête** selon l’URL.



##  **Pourquoi créer des Dockerfiles et tester localement ?**

- Le `Dockerfile` est ce qui permet de **construire une image portable** du microservice.
- Tester dans un conteneur local **avant de déployer** permet de s’assurer que :
  - Le service fonctionne comme attendu.
  - L’image est correcte.
  - Le port est bien exposé.



##  **Pourquoi stocker le code dans AWS CodeCommit ?**

- Suivi des modifications avec Git.
- Collaboration sécurisée dans un environnement AWS.
- Préparation au **CI/CD** avec CodePipeline.



##  **Pourquoi utiliser des variables d’environnement comme `APP_DB_HOST` ?**

- Pour **découpler le code de la configuration**.
- Cela permet d'utiliser le **même code** en local, sur ECS, ou ailleurs — seule la variable change.
- C’est un **principe fondamental de l'approche "12-factor app"**.

