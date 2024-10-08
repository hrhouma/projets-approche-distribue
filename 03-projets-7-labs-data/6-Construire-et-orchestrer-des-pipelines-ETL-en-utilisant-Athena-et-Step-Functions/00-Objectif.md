----------------------
# Objectif: 
----------------------

L'objectif de ce laboratoire est de vous apprendre à construire un pipeline ETL (Extraction, Transformation et Chargement) en utilisant plusieurs services AWS tels que **Step Functions**, **Amazon S3**, **AWS Glue Data Catalog**, et **Amazon Athena**. Voici une explication plus pédagogique du processus et des concepts abordés.

### Qu'est-ce qu'un pipeline ETL ?
Un pipeline ETL est un processus automatisé qui permet de collecter des données, les transformer pour qu'elles soient prêtes à être utilisées, et les charger dans un système de stockage ou d'analyse. Dans ce lab, vous travaillez sur un grand ensemble de données de taxis de New York, et vous allez automatiser la création d'une base de données et de tables dans AWS Glue, puis interroger ces tables avec Amazon Athena.

### Les outils que vous allez utiliser

1. **AWS Step Functions** : Step Functions vous permet de créer des workflows automatisés, appelés **machines à états**, qui orchestrent les différentes étapes du pipeline. Chaque étape correspond à une tâche comme la création d'une base de données, l'exécution de requêtes SQL, ou encore la vérification de l'existence de tables.

2. **Amazon S3** : S3 est utilisé pour stocker les fichiers de données, dans ce cas les données des taxis de New York au format CSV (données brutes) et Parquet (données optimisées). L'avantage d'utiliser le format **Parquet** est qu'il permet de compresser et d'optimiser les données, réduisant ainsi l'espace de stockage et accélérant leur lecture.

3. **AWS Glue Data Catalog** : AWS Glue stocke les métadonnées de vos tables, c'est-à-dire les informations sur la structure des données (les noms des colonnes, les types de données, etc.). C'est ici que vous allez définir vos tables, qui pointent vers les données stockées dans S3.

4. **Amazon Athena** : Athena est un service de requêtes SQL sans serveur qui permet d'exécuter des requêtes directement sur des données stockées dans S3. Vous allez l'utiliser pour interroger les tables créées et obtenir des informations utiles à partir des données.

### Scénario du laboratoire

Votre mission est de créer un workflow qui automatisera :
- La création d'une base de données et de tables dans AWS Glue si elles n'existent pas déjà.
- La transformation des fichiers CSV en tables optimisées au format Parquet.
- La création d'une vue dans Athena qui combine deux tables pour fournir une vue agrégée des données (par exemple, la somme des distances de course de taxi ou des tarifs).

À la fin de ce lab, vous aurez mis en place un pipeline capable de traiter des données brutes, de les transformer et de les rendre disponibles pour des analyses rapides et optimisées.

### Pourquoi utiliser ces outils ensemble ?

L'utilisation de **Step Functions** permet de créer un workflow entièrement automatisé, réduisant ainsi le besoin d'interventions manuelles pour chaque étape du processus. En utilisant **Athena** avec **S3**, vous pouvez analyser des données massives sans avoir à gérer de serveurs ou de bases de données complexes. Enfin, **AWS Glue** gère la partie métadonnées et structure des tables, facilitant la gestion de grandes quantités de données.

En résumé, ce lab vous apprend à construire un pipeline ETL réutilisable et optimisé pour automatiser le traitement de vos données de bout en bout, en utilisant des outils cloud puissants et flexibles d'AWS.



```
+------------------------+------------------------------------------------------------+
| ÉTAPE                  | EXPLICATION SIMPLIFIÉE                                      |
+------------------------+------------------------------------------------------------+
| Stocker des données     | Utiliser **Amazon S3** pour garder vos fichiers CSV         |
|                        | (ex. : données des taxis de New York)                      |
+------------------------+------------------------------------------------------------+
| Orchestrer les étapes   | **AWS Step Functions** crée un plan de travail (workflow)   |
|                        | qui exécute automatiquement chaque étape                   |
+------------------------+------------------------------------------------------------+
| Organiser les données   | **AWS Glue** crée des bases de données et des tables       |
|                        | pour que les données soient facilement trouvées             |
+------------------------+------------------------------------------------------------+
| Exécuter des requêtes   | **Amazon Athena** permet d'exécuter des requêtes SQL       |
|                        | pour analyser directement les données stockées             |
|                        | dans Amazon S3                                              |
+------------------------+------------------------------------------------------------+
| Optimiser les données   | Convertir les fichiers CSV en format **Parquet** pour      |
|                        | économiser de l'espace et accélérer l'analyse               |
+------------------------+------------------------------------------------------------+
| Créer une vue des       | **Athena** peut combiner plusieurs tables pour créer       |
| données                 | une vue plus pratique des données                          |
+------------------------+------------------------------------------------------------+
| Automatisation complète | **Step Functions** s'assure que tout est automatique :     |
|                        | vérifier, créer, analyser et optimiser sans intervention   |
+------------------------+------------------------------------------------------------+
```

### Vulgarisation
- **S3** : C'est une étagère où vous stockez vos fichiers. Imaginez des boîtes contenant des fichiers de données.
- **Step Functions** : C'est un planificateur. Il vous dit "D'abord, je prends les fichiers, puis je les analyse, et je crée des tables".
- **Glue** : C'est l'ouvrier qui organise les données, en disant "Cette colonne contient les numéros de taxi, celle-là contient les montants".
- **Athena** : C'est l'analyste. Il prend les fichiers et répond à vos questions comme "Quels sont les trajets de taxi en janvier ?".
- **Parquet** : C'est comme compresser vos fichiers pour qu'ils prennent moins de place et se lisent plus vite.

Tout le lab consiste à **automatiser** ces actions, pour que vous n'ayez plus à le faire à la main chaque fois que vous avez de nouvelles données.

