# Mise à jour de données dynamiques en place

## Vue d'ensemble du Lab et objectifs

Un défi fréquent lors du traitement des données en continu provenant d'appareils de l'Internet des objets (IoT) est la modification fréquente du schéma. Par exemple, un capteur peut envoyer initialement quatre valeurs, puis deux, et ensuite dix. Avec plusieurs capteurs, cela peut rapidement s’amplifier. Il devient donc difficile de s'adapter à ces changements constants et de mettre à jour le lac de données avec des enregistrements modifiés. Cela est particulièrement complexe pour les lacs de données basés sur le stockage d'objets, comme Amazon Simple Storage Service (Amazon S3). L'outil Apache Hudi Connector, disponible sur AWS Marketplace, peut vous aider à relever ce défi.

Dans ce lab, vous utiliserez Amazon S3, Amazon Athena, AWS Glue et Apache Hudi pour relever ce défi d'adaptation aux schémas dynamiques. Vous utiliserez ces services pour faciliter les mises à jour efficaces en place des données et exécuter des requêtes en quasi temps réel.

### Objectifs à atteindre à la fin du Lab :

- Créer un job AWS Glue pour exécuter des scripts ETL personnalisés.
- Utiliser Athena pour exécuter des requêtes.
- Utiliser le connecteur Apache Hudi pour effectuer des mises à jour en place.

### Durée : 
Ce lab durera environ 90 minutes.

---

### Restrictions sur les services AWS :
Dans cet environnement de lab, l'accès aux services AWS et aux actions de service peut être limité à ceux nécessaires pour réaliser les instructions du lab. Vous pourriez rencontrer des erreurs si vous essayez d'accéder à d'autres services ou d'effectuer des actions au-delà de celles décrites dans ce lab.

---

### Scénario

Marie fait partie de l'équipe de data science et travaille avec beaucoup de données en continu collectées par des appareils IoT. À chaque fois que ces appareils sont réinitialisés, la taille et la structure des données changent. Un appareil qui envoie généralement peu de champs peut parfois en envoyer beaucoup plus. Cela devient complexe à gérer, car les outils standard s’attendent à une structure fixe des données. De plus, les données modifiées ne doivent affecter que les lignes concernées et non l’ensemble des données.

Votre défi est de développer un **proof of concept (POC)** pour permettre une adaptation aux schémas changeants tout en ne mettant à jour que les enregistrements concernés.

Vous utiliserez un job AWS Glue avec des scripts personnalisés pour gérer ce schéma dynamique et le connecteur Apache Hudi pour les mises à jour en place. Vous utiliserez Athena pour exécuter des requêtes SQL sur les données dynamiques, et S3 pour le stockage des données. Enfin, vous utiliserez Amazon Kinesis Data Streams pour ingérer des données générées aléatoirement par Amazon Kinesis Data Generator (KDG), qui émule un appareil IoT.

### Diagramme d'architecture initial :
Lors du démarrage du lab, l'environnement contiendra les ressources indiquées dans le diagramme suivant :

*(le diagramme)*

![image](https://github.com/user-attachments/assets/81d4eec3-c5b8-46af-8e21-8abf3a762fc8)

*(digramme final)*

![image](https://github.com/user-attachments/assets/8d8e5b28-0b0e-4b6d-b8f1-f54b778702c5)

---

### Tâche 1 : Analyser l’environnement du lab

Dans cette tâche, vous allez examiner l'environnement du lab et noter les détails de la configuration initiale.

1. Ouvrir la console CloudFormation.
2. Dans la liste des stacks, sélectionnez celle dont la description ne contient pas "ADE".
3. Accédez à l'onglet **Outputs** et copiez les valeurs dans un éditeur de texte pour utilisation ultérieure.

### Tâche 2 : S'abonner et activer le connecteur Hudi

1. Accédez à AWS Glue Studio.
2. Dans le menu, sélectionnez **Marketplace**.
3. Recherchez "hudi", sélectionnez **Apache Hudi Connector for AWS Glue**, et suivez les étapes pour l'activer.

### Tâche 3 : Configuration des scripts pour AWS Glue

1. Ouvrez la console AWS Cloud9.
2. Téléchargez les scripts nécessaires via les commandes `wget`.
3. Chargez ces fichiers dans un bucket S3 en utilisant la commande `aws s3 cp`.

### Tâche 4 : Configuration et exécution du job AWS Glue

1. Créez un stack CloudFormation pour configurer le job AWS Glue en utilisant un modèle CloudFormation stocké dans votre bucket S3.
2. Lancez le job Glue en accédant à la section "Jobs" d'AWS Glue et en sélectionnant le job créé.

### Tâche 5 : Utilisation de Kinesis Data Generator

1. Ouvrez Kinesis Data Generator et configurez les champs pour générer des données de capteurs.
2. Envoyez ces données dans Kinesis Data Streams.

### Tâche 6 : Utilisation d'Athena pour interroger les données

1. Utilisez Athena pour inspecter le schéma des données.
2. Exécutez des requêtes sur les tables stockées dans S3 via AWS Glue.

### Tâche 7 : Modification dynamique du schéma

1. Modifiez le schéma des données envoyées par Kinesis Data Generator en ajoutant une nouvelle colonne.
2. Observez les résultats des requêtes dans Athena sans effectuer de modifications dans AWS Glue.

### Tâche 8 : Réinitialisation du schéma

1. Revenez au schéma initial et vérifiez que les anciennes données sont toujours correctement gérées par AWS Glue et Athena.

---

### Soumission de votre travail

Pour soumettre votre progression, sélectionnez **Submit** en haut de la page.

---

Félicitations, vous avez complété le lab !

