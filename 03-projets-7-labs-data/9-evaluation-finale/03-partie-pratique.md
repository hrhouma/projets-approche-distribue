---------------------------------------------------
# Projet Capstone en Ingénierie des Données
---------------------------------------------------

# Aperçu et Objectifs

Tout au long de ce cours, vous avez réalisé des laboratoires pratiques, où vous avez utilisé les fonctionnalités de différents services AWS pour ingérer de grandes quantités de données, les transformer, et extraire des informations.

Dans ce projet Capstone, vous êtes mis au défi de construire une solution utilisant plusieurs services AWS que vous connaissez déjà, sans suivi étape par étape. Certaines sections de ce projet sont volontairement difficiles.

Ce projet Capstone vous demande de réaliser les tâches suivantes :

- Lancer et configurer une instance de l'environnement de développement intégré AWS Cloud9.
- Transformer des fichiers CSV en format Apache Parquet et les télécharger sur Amazon S3.
- Créer un explorateur AWS Glue pour déduire la structure des données.
- Utiliser Amazon Athena pour interroger les données.
- Créer une vue dans Athena.
- Exécuter des requêtes sur la vue Athena pour analyser les données.

# Durée et Gestion du Budget

Ce projet devrait prendre environ **4 heures**.

Cet environnement est persistant. Lorsque le minuteur atteint **0:00**, la session se termine, mais toutes les données et ressources que vous avez créées dans votre compte AWS seront conservées. Si vous relancez une session (par exemple, le lendemain), vous retrouverez votre travail dans l'environnement du laboratoire. Vous pouvez aussi prolonger votre session en cliquant sur **Démarrer le Lab** à tout moment avant la fin du compte à rebours.

**Important** : Surveillez votre budget dans l'interface du laboratoire. Si vous dépassez votre budget, votre compte sera désactivé, et tout votre travail ainsi que vos ressources seront perdus.

# Restrictions des Services AWS

Dans cet environnement de laboratoire, l'accès aux services AWS peut être restreint à ceux nécessaires pour compléter les instructions.

# Le Jeu de Données

- Le site *The Sea Around Us* fournit un jeu de données avec des informations historiques sur la pêche dans tous les océans du monde, de 1950 à 2018. 
- Les données comprennent des informations sur les captures annuelles par pays, type de poisson, zones de pêche, et la valeur des captures en dollars US (2010).
- Les données sont disponibles au format CSV et peuvent être téléchargées depuis le site *The Sea Around Us*.

# Carte des zones de haute mer

Les zones de pêche incluent les **eaux internationales** (au-delà de 200 milles nautiques) et les **Zones Économiques Exclusives (ZEE)** (à moins de 200 milles nautiques).

![image](https://github.com/user-attachments/assets/1c3a1297-ddcc-418b-b1e4-51b95ab9042b)

# Carte des ZEE dans le monde
![image](https://github.com/user-attachments/assets/f3ffa1c3-2413-413e-ade4-e8ad733b16d0)

Les données sont sous licence Creative Commons Attribution Non-Commercial 4.0 International.

# Remarque en anglais
The Sea Around Us data are (where not otherwise regulated) under a Creative Commons Attribution Non-Commercial 4.0 International License (https://creativecommons.org/licenses/by-nc/4.0/). Notices regarding copyrights (© The University of British Columbia), license and disclaimer can be found under http://www.seaaroundus.org/terms-and-conditions/. Pauly D., Zeller D., Palomares M.L.D. (Editors), 2020. Sea Around Us Concepts, Design and Data (seaaroundus.org).



# Scénario

Vous devez créer une infrastructure pour héberger les données de pêche afin que les analystes puissent générer des rapports sur l'impact de la pêche en haute mer. Vous testerez cette infrastructure avec trois fichiers de données provenant du site *The Sea Around Us* :

- Le premier fichier contient des données sur toutes les zones de haute mer.
- Le second fichier contient des données d'une zone spécifique du Pacifique, appelée *Pacific, Western Central*.
- Le troisième fichier contient des données de la ZEE des Fidji.

Vous commencerez avec une infrastructure préconfigurée, comme illustré ci-dessous.

# Architecture de début

![image](https://github.com/user-attachments/assets/7717f01e-d71b-461c-8bff-3d0c165cfa31)


À la fin du projet, votre architecture ressemblera à ceci.

# Architecture finale

![image](https://github.com/user-attachments/assets/aa04b058-f50e-46f9-b7e5-0147dfbb1d41)


# Accéder à la Console de Gestion AWS

1. En haut de cette page, choisissez **Démarrer le Lab**.
   - La session de laboratoire commence.
   - Un minuteur s'affiche en haut de la page indiquant le temps restant dans la session.

2. Une fois la session démarrée, cliquez sur le lien **AWS** en haut à gauche pour ouvrir la console de gestion AWS dans un nouvel onglet.

# Tâche 1 – Configurer l'Environnement de Développement

### Étapes à suivre :

1. **Créer un environnement AWS Cloud9** avec les paramètres suivants :
   - Nom de l'environnement : `CapstoneIDE`
   - Instance EC2 : `t2.micro`
   - Déployez l'instance pour qu'elle supporte les connexions SSH au VPC public Capstone.

2. **Créer deux buckets S3** dans la région us-east-1 :
   - `data-source-#####` (remplacez ##### par un nombre aléatoire).
   - `query-results-#####` (remplacez ##### par un nombre aléatoire).

3. **Télécharger les fichiers source CSV** avec les commandes suivantes dans le terminal AWS Cloud9 :

```bash
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-GLOBAL-1-v48-0.csv
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-HighSeas-71-v48-0.csv
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACDENG-1-91570/lab-capstone/s3/SAU-EEZ-242-v48-0.csv
```

4. **Afficher les premières lignes du fichier `SAU-GLOBAL-1-v48-0.csv`** :

```bash
head -6 SAU-GLOBAL-1-v48-0.csv
```

5. **Convertir le fichier CSV en format Parquet** à l'aide de pandas :

   a. Installer les outils nécessaires :

   ```bash
   sudo pip3 install pandas pyarrow fastparquet
   ```

   b. Convertir le fichier en Parquet :

   ```bash
   python3
   import pandas as pd
   df = pd.read_csv('SAU-GLOBAL-1-v48-0.csv')
   df.to_parquet('SAU-GLOBAL-1-v48-0.parquet')
   exit()
   ```

6. **Télécharger le fichier Parquet** dans le bucket S3 `data-source` :

```bash
aws s3 cp SAU-GLOBAL-1-v48-0.parquet s3://data-source-#####
```

# Tâche 2 – Utiliser AWS Glue et Interroger Plusieurs Fichiers avec Athena

1. **Analyser la structure du fichier `SAU-HighSeas-71-v48-0.csv`** :

```bash
head -6 SAU-HighSeas-71-v48-0.csv
```

2. **Convertir ce fichier au format Parquet** et le télécharger dans S3.

3. **Créer une base de données et un explorateur AWS Glue** avec les paramètres suivants :
   - Base de données : `fishdb`
   - Explorateur : `fishcrawler`
   - Fréquence : À la demande.

4. **Exécuter l'explorateur Glue** pour générer une table dans la base de données AWS Glue.

5. **Interroger la table avec Athena**. Avant de lancer la première requête, configurez la sortie des requêtes dans le bucket `query-results`.

Exemple de requête :

```sql
SELECT DISTINCT area_name FROM fishdb.data_source_xxxxx;
```

6. **Exécuter des requêtes supplémentaires** pour obtenir des informations sur les captures de poissons.

Exemple :

```sql
SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValuePacificWCSeasCatch
FROM fishdb.data_source_xxxxx
WHERE area_name LIKE '%Pacific%' AND fishing_entity='Fiji' AND year > 2000
GROUP BY year, fishing_entity
ORDER BY year;
```

# Tâche 3 – Transformer un Nouveau Fichier et l'Ajouter au Jeu de Données

1. **Analyser et harmoniser les colonnes** du fichier `SAU-EEZ-242-v48-0.csv` :

```bash
cp SAU-EEZ-242-v48-0.csv SAU-EEZ-242-v48-0-old.csv

python3
import pandas as pd
data_location = 'SAU-EEZ-242-v48-0-old.csv'
df = pd.read_csv(data_location)
df.rename(columns={"fish_name": "common_name", "country": "fishing_entity"}, inplace=True)
df.to_csv('SAU-EEZ-242-v48-0.csv', header=True, index=False)
df.to_parquet('SAU-EEZ-242-v48-0.parquet')
exit()
```

2. **Télécharger le fichier sur S3**, relancer l'explorateur AWS Glue pour mettre à jour les métadonnées, puis interroger les données dans Athena.




# Étape 3 : Exécuter des Requêtes pour Vérifier les Données Intégrées

1. **Exécuter des requêtes dans Amazon Athena** pour valider que les données du fichier `SAU-EEZ-242-v48-0.csv` ont bien été intégrées et que les colonnes harmonisées sont correctement catégorisées dans la table mise à jour.

Exemple de requête pour vérifier les valeurs dans la colonne `area_name` :

```sql
SELECT DISTINCT area_name FROM fishdb.data_source_#####;
```

2. **Trouver la valeur en dollars US des poissons capturés par les Fidji dans les eaux internationales depuis 2001**, en utilisant la requête suivante :

```sql
SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueOpenSeasCatch 
FROM fishdb.data_source_##### 
WHERE area_name IS NULL AND fishing_entity = 'Fiji' AND year > 2000 
GROUP BY year, fishing_entity 
ORDER BY year;
```

3. **Trouver la valeur en dollars US des poissons capturés par les Fidji dans la ZEE depuis 2001**, avec cette requête :

```sql
SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueEEZCatch 
FROM fishdb.data_source_##### 
WHERE area_name LIKE '%Fiji%' AND fishing_entity = 'Fiji' AND year > 2000 
GROUP BY year, fishing_entity 
ORDER BY year;
```

4. **Comparer les captures dans la ZEE et les eaux internationales** : vous pouvez utiliser la requête suivante pour comparer les captures dans les deux zones, toujours pour les Fidji, depuis 2001 :

```sql
SELECT year, fishing_entity AS Country, CAST(CAST(SUM(landed_value) AS DOUBLE) AS DECIMAL(38,2)) AS ValueEEZAndOpenSeasCatch 
FROM fishdb.data_source_##### 
WHERE (area_name LIKE '%Fiji%' OR area_name IS NULL) AND fishing_entity = 'Fiji' AND year > 2000 
GROUP BY year, fishing_entity 
ORDER BY year;
```

### Analyse :
Si les métadonnées sont correctement mises à jour, les résultats de la troisième requête devraient être égaux à la somme des résultats des deux premières requêtes pour chaque année.

## Titre 1 : Créer une Vue dans Amazon Athena

1. **Créer une vue** dans Athena pour observer les données sur les captures de maquereaux, qui seront utiles dans la prochaine étape.

Exécutez la requête suivante pour créer la vue :

```sql
CREATE OR REPLACE VIEW MackerelsCatch AS
SELECT year, area_name AS WhereCaught, fishing_entity AS Country, SUM(tonnes) AS TotalWeight
FROM fishdb.data_source_##### 
WHERE common_name LIKE '%Mackerels%' AND year > 2014 
GROUP BY year, area_name, fishing_entity, tonnes 
ORDER BY tonnes DESC;
```

2. **Vérifiez que la vue contient des données** en consultant l'onglet "Vues" dans l'interface Athena et en sélectionnant l'option "Aperçu de la vue" (Preview View).

# Titre 1 : Placez l'image ici (Aperçu des données de maquereaux capturés).

# Analyse des Données de la Vue

1. **Requête pour voir le nombre total de tonnes de maquereaux capturées par année et par pays** :

```sql
SELECT year, Country, MAX(TotalWeight) AS Weight 
FROM fishdb.mackerelscatch 
GROUP BY year, Country 
ORDER BY year, Weight DESC;
```

# Titre 1 : Placez l'image ici (Tableau final avec les noms des pays obscurcis).

2. **Requête pour observer les captures de maquereaux par un pays spécifique, comme la Chine** :

```sql
SELECT * FROM "fishdb"."mackerelscatch" 
WHERE Country = 'China';
```

# Titre 1 : Placez l'image ici (Données de la Chine).

---

# Soumission de Votre Travail

1. **Soumettre votre travail** en haut de cette page en cliquant sur le bouton **Soumettre**.
2. Si les résultats ne s'affichent pas après quelques minutes, cliquez sur **Notes** pour voir combien de points vous avez obtenus pour chaque tâche.
   
# Conseils :
- Vous pouvez soumettre votre travail plusieurs fois. Après chaque changement, cliquez sur **Soumettre** à nouveau pour enregistrer votre dernier état.
- Pour obtenir des commentaires détaillés sur votre travail, cliquez sur **Rapport de soumission**.

# Fin de la Session

### Rappel :
Cet environnement de laboratoire est persistant. Toutes les ressources que vous avez créées seront conservées jusqu'à ce que vous atteigniez la limite de votre budget ou la date de fin du cours.

1. Lorsque vous avez terminé pour la journée, ou si vous avez fini d'activer l'environnement de laboratoire, cliquez sur **Terminer le Lab** en haut de la page. Confirmez en cliquant sur **Oui**.

### Remarque :
Terminer la session n'efface pas vos ressources. Elles seront toujours disponibles lors de votre prochaine session.


--------------------------

# Réouverture de la Session et Reprise du Travail

Lorsque vous êtes prêt à poursuivre votre travail sur ce projet Capstone, vous pouvez rouvrir la session de laboratoire et reprendre là où vous vous êtes arrêté.

### Étapes pour reprendre la session :

1. **Ouvrir la session de laboratoire** : En haut de cette page, cliquez sur **Démarrer le Lab**.
   - Cela relancera la session et vous permettra de récupérer toutes les ressources que vous aviez créées lors de la session précédente.

2. **Vérification des ressources AWS** : Une fois la session relancée, vous pourrez accéder à la console de gestion AWS en cliquant sur le lien AWS dans le coin supérieur gauche. Vous devriez voir tous vos buckets S3, vos bases de données AWS Glue, et les tables Athena déjà créées lors de vos précédentes étapes.

3. **Vérifier l'environnement de développement** :
   - Si vous travaillez dans AWS Cloud9, ouvrez votre environnement `CapstoneIDE` et vérifiez que les fichiers téléchargés, transformés et les scripts sont toujours présents.
   - Assurez-vous que les fichiers CSV et Parquet sont bien stockés dans les buckets S3 correspondants.

# Exploration Supplémentaire des Données

Si vous avez terminé les tâches principales du projet, vous pouvez explorer davantage les données ou essayer de nouvelles analyses pour renforcer votre compréhension des services AWS et des processus de traitement de données.

# Exemples de Requêtes Supplémentaires :

1. **Analyser la capture de poissons par d'autres pays dans d'autres zones de haute mer** :
   
```sql
SELECT year, fishing_entity AS Country, SUM(tonnes) AS TotalTonnes
FROM fishdb.data_source_##### 
WHERE area_name LIKE '%Pacific%' 
GROUP BY year, fishing_entity 
ORDER BY year DESC;
```

2. **Explorer l'évolution des captures sur une période spécifique** :
   
```sql
SELECT year, fishing_entity AS Country, SUM(tonnes) AS TotalTonnes
FROM fishdb.data_source_##### 
WHERE year BETWEEN 1990 AND 2010 
GROUP BY year, fishing_entity 
ORDER BY TotalTonnes DESC;
```

3. **Calculer la valeur totale des captures pour une zone précise** :
   
```sql
SELECT area_name, SUM(landed_value) AS TotalValue
FROM fishdb.data_source_##### 
GROUP BY area_name 
ORDER BY TotalValue DESC;
```

Ces requêtes supplémentaires vous permettront de mieux comprendre les données et de tester la flexibilité de votre architecture pour gérer des ensembles de données complexes.

# Défis Optionnels

Pour aller encore plus loin et améliorer votre projet, vous pouvez essayer les défis suivants :

1. **Ajouter de nouvelles zones de pêche** : Téléchargez d'autres fichiers de données depuis *The Sea Around Us* et intégrez-les dans votre système AWS. Transformez ces fichiers au format Parquet, mettez à jour les métadonnées avec AWS Glue et exécutez des analyses supplémentaires dans Athena.

2. **Automatisation avec AWS Lambda** : Configurez une fonction AWS Lambda pour automatiser le processus de transformation des fichiers CSV en Parquet et leur téléchargement dans S3.

3. **Visualisation des données** : Connectez Amazon QuickSight à Athena pour visualiser les résultats des requêtes de manière graphique. Vous pouvez créer des tableaux de bord interactifs pour mieux comprendre les tendances des captures de poissons et les visualiser avec des graphiques.

# Conclusion du Projet

Une fois que vous avez terminé toutes les étapes du projet et exploré les données, vous aurez acquis une solide compréhension de l'utilisation des services AWS pour gérer des données massives, les transformer et en extraire des informations précieuses. Vous aurez appris à utiliser AWS Cloud9, S3, Glue, Athena, et éventuellement d'autres services comme QuickSight pour analyser et visualiser des ensembles de données complexes.

# Étapes finales :

- **Sauvegarder votre travail** : Assurez-vous que toutes vos ressources sont correctement configurées et que vos analyses sont complètes.
- **Soumettre votre travail** : Si vous participez à un cours ou à un programme de certification, soumettez votre projet en suivant les instructions données précédemment dans la section *Soumettre votre travail*.
  
# Fin du Projet Capstone

Félicitations pour avoir complété ce projet Capstone en ingénierie des données ! Vous avez démontré votre capacité à utiliser plusieurs services AWS pour gérer et analyser de grandes quantités de données. Continuez à explorer les différentes possibilités qu'offrent les outils AWS pour enrichir vos compétences en ingénierie des données et en analyse.

- Ce guide détaillé vous a accompagné à travers chaque étape du projet Capstone, du lancement de l'environnement de développement à l'analyse des données dans Athena, en passant par la transformation de fichiers et la création de vues. 
- Vous pouvez maintenant appliquer ces compétences à des projets plus complexes, dans un environnement professionnel réel.
