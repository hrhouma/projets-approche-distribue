---------------------------
# Projet Capstone 
---------------------------

# **Aperçu et objectifs**

Tout au long de ce cours, vous avez réalisé des travaux pratiques, où vous avez utilisé les fonctionnalités de différents services AWS pour pratiquer l'ingestion de grands ensembles de données, leur transformation et l'extraction d'informations.

Dans ce projet de fin de parcours, vous êtes mis au défi de construire une solution qui utilise de nombreux services AWS que vous connaissez déjà, sans recevoir de directives détaillées étape par étape. Certaines parties de l'exercice sont conçues pour être difficiles.

Ce projet capstone vous mettra au défi de faire les choses suivantes :

- Lancer et configurer une instance d'environnement de développement intégré (IDE) AWS Cloud9.
- Transformer des fichiers de données au format CSV en format Apache Parquet et les télécharger sur Amazon S3.
- Créer un robot d'exploration AWS Glue pour déduire la structure des données.
- Utiliser Amazon Athena pour interroger les données.
- Créer une vue Athena.
- Exécuter des requêtes sur la vue Athena pour analyser les données.

# **Durée et suivi de votre budget**

Ce projet de fin de parcours devrait prendre environ 4 heures à compléter.

Cet environnement est persistant. Lorsque le minuteur de session arrive à 0:00, la session se termine, mais toutes les données et ressources que vous avez créées dans le compte AWS seront conservées. Si vous lancez une nouvelle session plus tard (par exemple, le jour suivant), vous retrouverez votre travail dans l'environnement de lab. De plus, à tout moment avant que le minuteur de session n'atteigne 0:00, vous pouvez choisir "Start Lab" à nouveau pour prolonger la durée de la session.

# **Important :

** Surveillez votre budget de lab dans l'interface du lab. Lorsque vous avez une session de lab active, les dernières informations connues sur le budget restant s'affichent en haut de cet écran. Ces données proviennent de AWS Budgets, qui se met à jour généralement toutes les 8 à 12 heures. Ainsi, le budget restant que vous voyez pourrait ne pas refléter votre activité la plus récente sur le compte. Si vous dépassez votre budget de lab, votre compte de lab sera désactivé, et tout progrès et toutes les ressources seront perdus. Il est donc important de gérer vos dépenses.

# **Restrictions des services AWS**

Dans cet environnement de lab, l'accès aux services AWS et aux actions de service pourrait être limité à ceux nécessaires pour suivre les instructions du lab. Vous pourriez rencontrer des erreurs si vous tentez d'accéder à d'autres services ou d'effectuer des actions au-delà de ce qui est décrit dans ce lab.

# **Le jeu de données**

Le site web *The Sea Around Us* fournit un jeu de données contenant des informations historiques étendues sur les pêcheries dans toutes les parties de chaque océan du monde. Les données incluent des informations sur les prises de pêcherie annuelles de 1950 à 2018.

Les données peuvent être téléchargées au format CSV depuis le site *The Sea Around Us*. Le jeu de données contient des colonnes d'informations pour chaque année, notamment quels pays ont capturé quels types de poissons et dans quelles zones. Les données indiquent également combien de tonnes de poissons ont été capturées et quelle était la valeur des prises, mesurée en dollars américains de 2010.

Pour comprendre les données, il est utile de comprendre ce que signifient les zones de haute mer et les zones ZEE :

- **Haute mer (aussi appelée mers internationales)** : Zones de l'océan situées à au moins 200 miles nautiques de la côte de tout pays. Les ressources, y compris les poissons dans ces zones, sont généralement considérées comme n'appartenant à aucun pays en particulier. La carte suivante, qui est une capture d'écran du site web *The Sea Around Us*, montre comment le jeu de données divise les zones de haute mer (zones en gris foncé) en zones uniques de haute mer.

![image](https://github.com/user-attachments/assets/0e68393e-c657-4673-af96-7a64935e306a)

- **Zones économiques exclusives (ZEE)** : Zones situées à moins de 200 miles nautiques de la côte d'un pays. Chaque pays revendique généralement un accès exclusif aux ressources dans ces zones, y compris les poissons qui s'y trouvent. La carte suivante, qui est une capture d'écran du site web *The Sea Around Us*, montre les ZEE dans le monde.

![image](https://github.com/user-attachments/assets/d743aabd-7067-4410-9d8c-baf4c2a7a513)


Les données de *The Sea Around Us* sont (lorsqu'elles ne sont pas autrement réglementées) sous licence Creative Commons Attribution Non-Commercial 4.0 International (https://creativecommons.org/licenses/by-nc/4.0/). Les avis concernant les droits d'auteur (© Université de la Colombie-Britannique), la licence et les avertissements sont disponibles sur http://www.seaaroundus.org/terms-and-conditions/. Pauly D., Zeller D., Palomares M.L.D. (éditeurs), 2020. *Sea Around Us Concepts, Design and Data* (seaaroundus.org).

# **Scénario**

Vous avez pour mission de créer l'infrastructure pour héberger des données sur la pêche afin que les analystes de données de votre organisation puissent créer des rapports sur l'impact de la pêche en haute mer. Vous avez décidé de construire l'infrastructure dans votre compte AWS et de la tester en utilisant trois fichiers de données provenant du jeu de données *The Sea Around Us*.

Dans ce projet de fin de parcours, vous allez travailler avec trois fichiers de données provenant du site web *The Sea Around Us* :

- Le premier fichier contient des données de toutes les zones de haute mer.
- Le deuxième fichier contient des données d'une seule zone de haute mer dans l'océan Pacifique, appelée **Pacifique, Centre-Ouest**, qui se trouve près de Fidji et de nombreux autres pays.
- Le troisième fichier contient des données de la ZEE d'un seul pays (Fidji), qui est situé près de la zone de haute mer **Pacifique, Centre-Ouest**.

En travaillant avec cette variété de fichiers de données d'échantillon, vous serez en mesure de tester si la solution que vous construisez peut prendre en charge un ensemble de données beaucoup plus grand.

Lorsque vous commencerez ce projet de fin de parcours, l'environnement contiendra les ressources illustrées dans le diagramme suivant.

![image](https://github.com/user-attachments/assets/a0264aeb-b3a7-4daa-b951-8fd9a99a4722)

À la fin de ce projet de fin de parcours, vous aurez créé une architecture similaire à celle illustrée dans le diagramme suivant.

![image](https://github.com/user-attachments/assets/d19b7dbc-9d9b-45dd-bc0c-6c71dfd22fc6)













# Accéder à la console de gestion AWS

1. En haut de ces instructions, choisissez **Start Lab**.

   - La session de lab démarre.
   - Un minuteur s'affiche en haut de la page et montre le temps restant dans la session.
   
   **Astuce :** Pour rafraîchir la durée de la session à tout moment, choisissez à nouveau **Start Lab** avant que le minuteur n'atteigne 0:00.

   - Avant de continuer, attendez que l'icône circulaire à droite du lien **AWS** dans le coin supérieur gauche devienne verte.

2. Pour vous connecter à la console de gestion AWS, choisissez le lien **AWS** dans le coin supérieur gauche.

   - Un nouvel onglet de navigateur s'ouvre et vous connecte à la console.

   **Astuce :** Si un nouvel onglet de navigateur ne s'ouvre pas, une bannière ou une icône apparaît généralement en haut de votre navigateur avec le message indiquant que votre navigateur empêche le site d'ouvrir des fenêtres pop-up. Choisissez la bannière ou l'icône, puis sélectionnez **Autoriser les pop-ups**.

---

# Tâche 1 : Configuration de l'environnement de développement

Dans cette première partie du projet de fin de parcours, vous allez configurer votre environnement de développement.

1. Observez les détails du rôle **CapstoneGlueRole** d'AWS Identity and Access Management (IAM) qui a été créé pour vous.

2. Créez un environnement AWS Cloud9 avec les paramètres suivants :

   - Nommez l'environnement **CapstoneIDE**.
   - Créez une nouvelle instance EC2 pour l'environnement, et utilisez une instance **t2.micro**.
   - Déployez l'instance pour prendre en charge les connexions SSH au VPC Capstone, dans le sous-réseau public Capstone.
   - Conservez tous les autres paramètres par défaut.

3. Créez deux buckets S3 avec les paramètres suivants :

   - Créez les buckets dans la région **us-east-1**.
   - Nommez le premier bucket **data-source-#####** où ##### est un nombre aléatoire.
   - Nommez le deuxième bucket **query-results-#####** où ##### est également un nombre aléatoire.
   - Conservez tous les autres paramètres par défaut.

4. Pour télécharger les trois fichiers sources au format .csv, exécutez les commandes suivantes dans le terminal de votre IDE AWS Cloud9 :

   ```
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-GLOBAL-1-v48-0.csv
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-HighSeas-71-v48-0.csv
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-EEZ-242-v48-0.csv
   ```

5. Pour observer la ligne d'en-tête des colonnes et les cinq premières lignes de données du fichier **SAU-GLOBAL-1-v48-0.csv**, exécutez la commande suivante :

   ```
   head -6 SAU-GLOBAL-1-v48-0.csv
   ```

   **Analyse :** Parmi d'autres détails, chaque ligne de données dans cet ensemble inclut :

   - L'année de la pêche
   - Le pays (**fishing_entity**) ayant réalisé la pêche
   - Les tonnes de poissons pêchées cette année-là par ce pays
   - La valeur en dollars américains de 2010 (**landed_value**) des poissons capturés cette année-là par ce pays

   **Remarque :**

   - Cet ensemble de données contient **561 675 lignes**.

   **Astuce :** Pour confirmer cela, exécutez la commande suivante : 

   ```
   wc -l <filename>
   ```

   - L'ensemble de données comprend des données déclarées et des "meilleures estimations" pour toutes les pêches qui ont eu lieu dans les eaux internationales (c'est-à-dire, en dehors de la ZEE de tout pays) entre 1950 et 2018.
   - Les zones ZEE comprennent les eaux océaniques situées à moins de 200 miles nautiques du littoral d'un pays. Par conséquent, les pêches reportées dans cet ensemble de données ont eu lieu à au moins 200 miles des côtes de tout pays.

---

6. Convertissez le fichier **SAU-GLOBAL-1-v48-0.csv** au format Parquet.

   - Tout d'abord, vous devez installer certains outils sur votre IDE AWS Cloud9. Exécutez la commande suivante :

   ```
   sudo pip3 install pandas pyarrow fastparquet
   ```

   - Ensuite, pour convertir le fichier au format Parquet, exécutez le code suivant :

   ```python
   # Démarrer l'interpréteur Python interactif
   python3

   # Utiliser pandas pour convertir le fichier au format parquet
   import pandas as pd
   df = pd.read_csv('SAU-GLOBAL-1-v48-0.csv')
   df.to_parquet('SAU-GLOBAL-1-v48-0.parquet')
   exit()
   ```

   **Remarque :** Pandas est un outil utile pour travailler avec des fichiers de données. Pour plus d'informations, consultez le site web de pandas.

7. Pour télécharger le fichier **SAU-GLOBAL-1-v48-0.parquet** dans le bucket **data-source**, utilisez une commande AWS CLI dans le terminal de votre AWS Cloud9.




