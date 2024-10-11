#  **Kinesis Firehose** VS  **Kinesis Streams (Firestream)** 

--------------------------------
# Tableau 1
--------------------------------


* Ce tableau peut aider à choisir selon le besoin et le contexte d'utilisation spécifique.

| **Cas d'Utilisation**                           | **Kinesis Firehose**                                       | **Kinesis Streams (Firestream)**                             |
|--------------------------------------------------|------------------------------------------------------------|-------------------------------------------------------------|
| **Gestion des flux de données sans maintenance** | - Prend en charge automatiquement la gestion des flux.<br>- Idéal si tu ne veux pas gérer de shards ou d'infrastructure de streaming.<br>- Utilisation simple avec intégration directe vers S3, Redshift, etc. | - Nécessite une gestion manuelle des shards et de la capacité.<br>- Préfère Kinesis Streams si tu veux plus de contrôle sur la partition et la gestion des flux. |
| **Transformation des données avant stockage**    | - **Avec Lambda** : Intégration simple pour transformer les données en temps réel avant de les envoyer vers une destination comme S3 ou Elasticsearch.<br>- Idéal pour des transformations simples sans gestion manuelle. | - Peut intégrer Lambda, mais offre un contrôle plus granulaire si tu as besoin de transformer les données en amont ou de les traiter à une échelle plus fine. |
| **Petites charges de données**                   | - Adapté à des charges petites et moyennes grâce à son mécanisme de bufferisation.<br>- Utilise des buffers pour regrouper les petites charges et les envoyer efficacement. | - Nécessite de configurer manuellement le nombre de shards en fonction de la charge de données. Peut être surdimensionné pour des charges légères. |
| **Grandes charges de données**                   | - Scalable automatiquement pour de grandes charges de données.<br>- Firehose gère la distribution des données sans configuration manuelle de la capacité. | - Bon pour des charges massives avec plus de contrôle sur la gestion des partitions et des shards.<br>- Nécessite une configuration manuelle pour gérer le débit et la répartition des données. |
| **Analyse en temps réel**                        | - Peut être utilisé pour des cas simples de collecte et de stockage de données, mais l'analyse en temps réel est moins flexible.<br>- Convient aux pipelines où le stockage en temps réel est suffisant. | - Offre un contrôle total pour les analyses complexes en temps réel.<br>- Idéal pour des applications nécessitant une latence ultra-faible et des décisions en temps réel. |
| **Enrichissement des données en transit**        | - Avec Lambda, tu peux enrichir ou formater les données avant de les stocker. Simple à configurer sans gérer l'infrastructure. | - Tu peux enrichir les données de manière plus granulaire avant de les envoyer aux consommateurs.<br>- Offre plus de flexibilité pour des processus d'enrichissement plus complexes. |
| **Faible latence**                               | - Peut supporter des flux quasi temps réel, mais n'est pas optimal pour des cas où la latence doit être la plus faible possible. | - Préférable si tu as besoin d'une latence minimale pour traiter les données quasi instantanément. |
| **Cas d'utilisation avec besoin de persistance** | - Utilise des destinations de stockage persistantes comme S3 ou Redshift.<br>- Convient pour des pipelines où les données doivent être persistées à intervalles réguliers. | - Conserve les données dans des shards pendant une période (par défaut 24h, max 7 jours).<br>- Idéal pour des cas d'utilisation nécessitant de relire ou rejouer les données stockées. |

### Résumé :
- **Kinesis Firehose** est à privilégier lorsque tu veux un **service entièrement géré**, avec **moins d'intervention manuelle** pour l'échelle et la capacité, et pour **transformer et stocker** des données dans des destinations comme S3 ou Redshift. Idéal pour des petites et grandes charges où l'analyse en temps réel n'est pas critique.
  
- **Kinesis Streams (Firestream)** est à privilégier lorsque tu as besoin de **plus de contrôle** sur les données en streaming, une **latence très faible**, ou la possibilité de **rejouer des données** sur une période donnée. Il est préférable pour des analyses **complexes en temps réel** et des charges très élevées où tu veux gérer les partitions et le débit manuellement.


--------------------------------
# Tableau 2
--------------------------------


* Ce tableau qui compare **Kinesis Firehose** et **Kinesis Streams (Firestream)** avec des cas d'utilisation typiques pour vous aider à savoir quand il est préférable d'utiliser l'un ou l'autre :

| **Cas d'Utilisation**                           | **Kinesis Firehose**                                       | **Kinesis Streams (Firestream)**                             |
|--------------------------------------------------|------------------------------------------------------------|-------------------------------------------------------------|
| **Gestion des flux de données sans maintenance** | - Prend en charge automatiquement la gestion des flux.<br>- Idéal si tu ne veux pas gérer de shards ou d'infrastructure de streaming.<br>- Utilisation simple avec intégration directe vers S3, Redshift, etc. | - Nécessite une gestion manuelle des shards et de la capacité.<br>- Préfère Kinesis Streams si tu veux plus de contrôle sur la partition et la gestion des flux. |
| **Transformation des données avant stockage**    | - **Avec Lambda** : Intégration simple pour transformer les données en temps réel avant de les envoyer vers une destination comme S3 ou Elasticsearch.<br>- Idéal pour des transformations simples sans gestion manuelle. | - Peut intégrer Lambda, mais offre un contrôle plus granulaire si tu as besoin de transformer les données en amont ou de les traiter à une échelle plus fine. |
| **Petites charges de données**                   | - Adapté à des charges petites et moyennes grâce à son mécanisme de bufferisation.<br>- Utilise des buffers pour regrouper les petites charges et les envoyer efficacement. | - Nécessite de configurer manuellement le nombre de shards en fonction de la charge de données. Peut être surdimensionné pour des charges légères. |
| **Grandes charges de données**                   | - Scalable automatiquement pour de grandes charges de données.<br>- Firehose gère la distribution des données sans configuration manuelle de la capacité. | - Bon pour des charges massives avec plus de contrôle sur la gestion des partitions et des shards.<br>- Nécessite une configuration manuelle pour gérer le débit et la répartition des données. |
| **Analyse en temps réel**                        | - Peut être utilisé pour des cas simples de collecte et de stockage de données, mais l'analyse en temps réel est moins flexible.<br>- Convient aux pipelines où le stockage en temps réel est suffisant. | - Offre un contrôle total pour les analyses complexes en temps réel.<br>- Idéal pour des applications nécessitant une latence ultra-faible et des décisions en temps réel. |
| **Enrichissement des données en transit**        | - Avec Lambda, tu peux enrichir ou formater les données avant de les stocker. Simple à configurer sans gérer l'infrastructure. | - Tu peux enrichir les données de manière plus granulaire avant de les envoyer aux consommateurs.<br>- Offre plus de flexibilité pour des processus d'enrichissement plus complexes. |
| **Faible latence**                               | - Peut supporter des flux quasi temps réel, mais n'est pas optimal pour des cas où la latence doit être la plus faible possible. | - Préférable si tu as besoin d'une latence minimale pour traiter les données quasi instantanément. |
| **Cas d'utilisation avec besoin de persistance** | - Utilise des destinations de stockage persistantes comme S3 ou Redshift.<br>- Convient pour des pipelines où les données doivent être persistées à intervalles réguliers. | - Conserve les données dans des shards pendant une période (par défaut 24h, max 7 jours).<br>- Idéal pour des cas d'utilisation nécessitant de relire ou rejouer les données stockées. |

### Résumé :
- **Kinesis Firehose** est à privilégier lorsque tu veux un **service entièrement géré**, avec **moins d'intervention manuelle** pour l'échelle et la capacité, et pour **transformer et stocker** des données dans des destinations comme S3 ou Redshift. Idéal pour des petites et grandes charges où l'analyse en temps réel n'est pas critique.
  
- **Kinesis Streams (Firestream)** est à privilégier lorsque tu as besoin de **plus de contrôle** sur les données en streaming, une **latence très faible**, ou la possibilité de **rejouer des données** sur une période donnée. Il est préférable pour des analyses **complexes en temps réel** et des charges très élevées où tu veux gérer les partitions et le débit manuellement.

--------------------------------
# Tableau 3
--------------------------------




Je vous présente un tableau plus détaillé, avec des **études de cas** et des **scénarios d'utilisation** supplémentaires pour mieux comprendre quand utiliser **Kinesis Firehose** ou **Kinesis Streams (Firestream)**, ainsi que des explications approfondies.

| **Cas d'Utilisation / Scénario**                         | **Kinesis Firehose**                                                                                           | **Kinesis Streams (Firestream)**                                                                                  |
|----------------------------------------------------------|------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| **Collecte de logs d'applications**                      | - **Scénario** : Tu collectes des logs d'une application web à grande échelle.<br>- **Étude de cas** : Firehose bufferise les logs toutes les 60 secondes et les envoie automatiquement dans un bucket S3.<br>- **Pourquoi Firehose ?** : Pas besoin de gérer manuellement les partitions ou de contrôler le débit des logs. Firehose gère tout automatiquement, et les logs sont convertis en format Parquet pour une analyse plus efficace dans S3. | - **Scénario** : Tu as besoin de suivre les logs en temps réel avec une latence très faible pour identifier des anomalies.<br>- **Étude de cas** : Utilisation de Kinesis Streams pour diviser les logs par utilisateur ou par type d'opération (erreurs, transactions réussies, etc.), en exploitant la flexibilité des shards pour analyser chaque type de log séparément.<br>- **Pourquoi Streams ?** : Un contrôle granulaire est nécessaire pour traiter en parallèle différents types de logs en temps réel, avec une faible latence. |
| **Transformation et enrichissement des données**         | - **Scénario** : Tu dois transformer des fichiers CSV en JSON avant de les stocker dans Redshift.<br>- **Étude de cas** : Firehose déclenche une fonction Lambda pour convertir chaque lot de données CSV en JSON avant de les envoyer à Redshift pour des analyses futures.<br>- **Pourquoi Firehose ?** : Simple à configurer avec Lambda pour des transformations basiques sans avoir à gérer d'infrastructure. | - **Scénario** : Tu enrichis les données avec des informations externes avant de les transmettre à d'autres systèmes.<br>- **Étude de cas** : Utiliser Streams pour traiter les données brutes, ajouter des informations provenant d'autres bases de données, puis envoyer les données enrichies à différents consommateurs.<br>- **Pourquoi Streams ?** : Streams permet de transformer et d'enrichir les données de manière très flexible, en divisant les processus de transformation selon les besoins du flux. |
| **Streaming de données IoT**                             | - **Scénario** : Tu collectes les données de capteurs IoT en temps quasi réel et tu souhaites les stocker dans un data lake pour des analyses futures.<br>- **Étude de cas** : Utilisation de Firehose pour bufferiser et envoyer les données dans S3, et déclenchement d'une fonction Lambda pour compresser les données en format Parquet.<br>- **Pourquoi Firehose ?** : Automatisation complète sans besoin de gérer manuellement les flux. Idéal pour stocker les données en temps quasi réel. | - **Scénario** : Tu analyses les données IoT pour prendre des décisions immédiates (ex. : couper une machine si un capteur détecte une température trop élevée).<br>- **Étude de cas** : Kinesis Streams ingère les données IoT avec une faible latence, et une application consomme ces données pour réagir en temps réel.<br>- **Pourquoi Streams ?** : Nécessité d'une latence extrêmement faible pour réagir aux événements en temps réel. |
| **Analyse en temps réel des données financières**        | - **Scénario** : Tu traites des transactions financières et veux les archiver rapidement pour des audits.<br>- **Étude de cas** : Firehose bufferise les transactions financières et les envoie dans un entrepôt de données (Redshift) pour analyse mensuelle.<br>- **Pourquoi Firehose ?** : Adapté aux données que tu veux archiver rapidement avec peu d'exigence de latence. | - **Scénario** : Tu analyses des transactions financières en temps réel pour détecter des fraudes.<br>- **Étude de cas** : Utilisation de Streams pour partitionner les transactions par région et détecter les modèles suspects en temps réel avec des algorithmes d'analyse de données en streaming.<br>- **Pourquoi Streams ?** : Nécessité d'une réponse immédiate pour détecter et bloquer des transactions suspectes avec une latence minimale. |
| **Traitement de flux vidéo en direct**                   | - **Scénario** : Tu envoies des flux vidéo à un service de stockage pour archivage et lecture future.<br>- **Étude de cas** : Firehose envoie des vidéos depuis des caméras de sécurité vers un bucket S3 toutes les 10 minutes pour archivage et analyse future.<br>- **Pourquoi Firehose ?** : Simplicité d'utilisation pour l'archivage des vidéos avec peu d'intervention. | - **Scénario** : Tu analyses les flux vidéo en direct pour détecter des objets ou des anomalies.<br>- **Étude de cas** : Streams ingère des vidéos en temps réel, les partitionne par caméras, et une application consomme chaque flux pour détecter des mouvements suspects ou des objets spécifiques.<br>- **Pourquoi Streams ?** : Analyse vidéo en temps réel nécessitant une faible latence et une gestion fine des flux entrants. |
| **Ingestion de données d'applications mobiles**          | - **Scénario** : Tu collectes des événements d'une application mobile (par exemple des événements de clic ou de session) et veux les stocker pour analyse comportementale.<br>- **Étude de cas** : Firehose envoie des lots d'événements d'application mobile vers Redshift pour analyse des tendances d'utilisation.<br>- **Pourquoi Firehose ?** : Adapté à l'ingestion en lot avec transformation des événements pour stockage direct dans un entrepôt de données. | - **Scénario** : Tu souhaites analyser les événements mobiles en temps réel pour améliorer l'expérience utilisateur (ex. : optimisation en direct des publicités affichées).<br>- **Étude de cas** : Streams traite les événements en temps réel et les envoie à un service d'analyse qui ajuste les recommandations en temps réel.<br>- **Pourquoi Streams ?** : Latence très faible pour prendre des décisions immédiates et personnaliser l'expérience utilisateur en direct. |
| **Cas d'utilisation avec stockage longue durée**         | - **Scénario** : Tu as besoin de stocker des données brutes sur une longue période pour des audits ou analyses futures.<br>- **Étude de cas** : Firehose envoie automatiquement des données compressées dans S3, avec un cycle de vie pour déplacer les données dans un stockage à coût réduit comme Glacier.<br>- **Pourquoi Firehose ?** : Optimisé pour l'archivage des données avec des mécanismes automatiques de gestion du cycle de vie. | - **Scénario** : Tu souhaites lire et rejouer les données sur une période donnée (jusqu'à 7 jours) pour une analyse à posteriori.<br>- **Étude de cas** : Streams conserve les données pendant 24 heures (ou jusqu'à 7 jours), te permettant de rejouer ces données pour tester des nouveaux modèles d'analyse ou simuler des scénarios.<br>- **Pourquoi Streams ?** : Utile si tu as besoin de relire des flux de données récents pour des analyses ou des simulations. |
| **Gestion de la scalabilité automatique**                | - **Scénario** : Tu as des flux de données dont la taille peut varier fortement et tu ne veux pas gérer manuellement la scalabilité.<br>- **Étude de cas** : Firehose ajuste automatiquement la capacité en fonction du volume de données sans intervention manuelle.<br>- **Pourquoi Firehose ?** : Idéal si tu ne veux pas gérer les ressources et que tu préfères une solution entièrement managée. | - **Scénario** : Tu veux ajuster manuellement la capacité des shards pour optimiser les coûts ou contrôler le débit.<br>- **Étude de cas** : Streams te permet de définir le nombre de shards, ce qui te donne plus de contrôle sur le débit et le coût.<br>- **Pourquoi Streams ?** : Préfère Streams si tu veux avoir un contrôle total sur la gestion des ressources en fonction des besoins spécifiques. |

### Études de Cas Complémentaires :
1. **Cas des plateformes de trading boursier** :
   - **Kinesis Firehose** serait utilisé pour archiver les transactions boursières à des fins d'audit, envoyant les transactions à Redshift pour une analyse plus tardive.
   - **Kinesis Streams** permettrait de suivre en temps réel les transactions pour détecter des comportements inhabituels ou des patterns de trading suspect, avec une latence minimale.

2. **Cas des applications de marketing en temps réel** :
   - **Kinesis Firehose** pourrait être utilisé pour stocker les interactions des utilisateurs avec une application mobile, les données étant archivées pour analyse comportementale et segmentation des utilisateurs.
   - **Kinesis Streams** permettrait d'analyser les interactions des utilisateurs en direct afin d'ajuster les recommandations ou les publicités en temps réel.

3. **Cas des villes intelligentes (smart cities)** :
   - **Kinesis Firehose** serait utilisé pour collecter des données de capteurs IoT (trafic, température, consommation d'énergie) et les envoyer à un data lake (S3) pour des analyses mensuelles ou annuelles.
   - **Kinesis Streams** pourrait analyser les données en temps réel pour prendre des décisions immédi







----------------------
# Tableau 4
----------------------



Je vous présente une comparaison plus détaillée qui met en lumière **quand utiliser** l’un et **quand ne pas utiliser** l’autre, en se concentrant sur le besoin d'intégrer d'autres services ou sur une gestion entièrement automatique :


| **Cas / Scénario**                                           | **Kinesis Firehose** : Quand l'utiliser et ne pas utiliser l'autre         | **Kinesis Streams (Firestream)** : Quand l'utiliser et ne pas utiliser l'autre |
|--------------------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| **Gestion entièrement gérée (pas besoin d'intégration supplémentaire)** | **Quand l'utiliser** : Si tu cherches une solution **entièrement gérée** sans avoir besoin de te soucier de la gestion de l'infrastructure, du débit ou des shards. Firehose ajuste automatiquement la capacité, compresse, chiffre et transforme les données sans intervention.<br>- **Exemple** : Collecte de logs, ingestion de données de capteurs IoT avec envoi direct dans S3 ou Redshift, sans besoin d'intégration complexe.<br>**Ne pas utiliser Streams** : Si tu ne veux pas gérer manuellement le nombre de shards ou l'échelle. Streams nécessite une gestion manuelle des ressources. | **Quand ne pas l'utiliser** : Si tu veux un service simple, entièrement géré, sans avoir à configurer manuellement la capacité ou gérer l'infrastructure. Kinesis Streams est plus complexe à gérer car tu dois contrôler le nombre de shards.<br>- **Exemple** : Firehose est préférable pour des cas simples où les données doivent être stockées ou archivées dans S3 sans besoin de transformation complexe en temps réel. |
| **Besoin d'intégrer plusieurs services ou architectures complexes** | **Quand ne pas l'utiliser** : Si tu as besoin de traiter des flux de données complexes avec plusieurs intégrations (ex. : plusieurs bases de données, services analytiques, ou des applications en temps réel). Firehose est conçu pour des pipelines plus simples où les données sont envoyées directement vers une destination. | **Quand l'utiliser** : Si tu dois **intégrer plusieurs services** en parallèle. Streams te permet de **multiplier les consommateurs**, et chaque consommateur peut être une application différente ou un service (par ex. : Lambda, Amazon EMR, etc.). Kinesis Streams permet de lire les mêmes données à partir de plusieurs applications en parallèle.<br>- **Exemple** : Applications temps réel, enrichissement de données via plusieurs sources, ou gestion de flux complexes qui nécessitent plusieurs niveaux de traitement. |
| **Prétraitement simple avant stockage dans un service comme S3 ou Redshift** | **Quand l'utiliser** : Firehose est idéal pour des **prétraitements simples** (ex. : formatage de logs, compression, transformation en JSON) avant d’envoyer les données à une destination comme S3 ou Redshift.<br>- **Exemple** : Collecte de logs d'application ou transformation des données CSV en JSON avec Lambda avant de les stocker.<br>**Ne pas utiliser Streams** : Si le flux de données ne nécessite pas d’analyses complexes ou de multi-consommateurs, il n’est pas nécessaire de passer par Streams. | **Quand ne pas l'utiliser** : Si tu as juste besoin de prétraiter les données (par ex. via Lambda), puis les envoyer dans un data lake (S3) ou entrepôt de données (Redshift). Streams peut être excessif pour des prétraitements simples, car tu devras gérer manuellement les shards et partitions. |
| **Faible latence requise pour des décisions en temps réel** | **Quand ne pas l'utiliser** : Si tu as besoin d’une latence ultra-faible pour prendre des décisions en temps réel (ex. : surveillance en temps réel, trading, ou systèmes de recommandation). Firehose n’est pas optimisé pour des délais de réaction immédiats. | **Quand l'utiliser** : Kinesis Streams est conçu pour des cas d'utilisation à **faible latence**, où des actions immédiates sont nécessaires en fonction des données qui arrivent.<br>- **Exemple** : Analyse de transactions financières pour détecter des fraudes en temps réel, ou surveillance d’objets via des capteurs IoT qui nécessitent des réactions immédiates. |
| **Archiver ou stocker les données pour une analyse ultérieure** | **Quand l'utiliser** : Si tu veux simplement **stocker ou archiver** des données à long terme, Firehose est idéal car il gère automatiquement l'envoi des données vers des destinations comme S3, Redshift ou Elasticsearch, sans besoin de les rejouer plus tard.<br>- **Exemple** : Archivage de logs ou de données brutes pour des audits futurs. | **Quand ne pas l'utiliser** : Si tu n’as pas besoin de relire ou de rejouer des données stockées. Kinesis Streams permet de relire les données dans une fenêtre de 24 heures à 7 jours, ce qui est utile si tu dois revenir en arrière, mais c’est inutile pour un stockage long terme simple. |
| **Relecture de données en streaming pour analyse ou simulation** | **Quand ne pas l'utiliser** : Si tu ne prévois pas de rejouer des flux de données pour des analyses ou simulations. Firehose envoie les données vers une destination, mais une fois envoyées, tu ne peux pas revenir en arrière pour les relire. | **Quand l'utiliser** : Streams est parfait pour des cas où tu dois **relire ou rejouer les données**. Par exemple, pour tester des nouveaux algorithmes ou pour une analyse après coup. Tu peux rejouer les données pendant une période allant jusqu'à 7 jours.<br>- **Exemple** : Test de nouveaux modèles d’apprentissage ou analyse des transactions passées pour identifier des patterns. |
| **Cas nécessitant de multiples consommateurs**             | **Quand ne pas l'utiliser** : Si tu dois traiter un flux de données pour plusieurs consommateurs (par ex. : un pipeline d'analytics, un entrepôt de données, une autre application). Firehose n'est pas optimisé pour cela, il est conçu pour envoyer des données à **une seule destination**. | **Quand l'utiliser** : Streams permet **plusieurs consommateurs** en parallèle, chaque application ou service peut lire les mêmes données à des moments différents.<br>- **Exemple** : Une application en temps réel pour la détection de fraudes peut lire les données en temps réel, tandis qu'une autre application peut lire les mêmes données pour une analyse plus tardive. |

### Résumé :

- **Utilise Kinesis Firehose** quand :
  - Tu veux un service entièrement géré, sans avoir à gérer manuellement l'échelle ou l'infrastructure.
  - Tu as besoin de **prétraitements simples** (ex. : transformation, compression) avant d'envoyer les données à une destination comme S3, Redshift, Elasticsearch.
  - Tu n'as **pas besoin de relecture** des données une fois envoyées.
  - Le cas d'utilisation consiste à **archiver ou stocker** des données pour une analyse future sans gestion complexe.

- **Utilise Kinesis Streams (Firestream)** quand :
  - Tu dois **intégrer plusieurs services** ou permettre à plusieurs applications de lire les mêmes données.
  - Tu as besoin de **contrôle granulaire** sur la gestion des flux, avec plusieurs **consommateurs en parallèle**.
  - Tu as besoin de **faible latence** pour des **réactions en temps réel**.
  - Tu veux **relire** les flux de données pendant une période (jusqu'à 7 jours) pour rejouer des événements ou tester des modèles.

En fonction de nos besoins en termes d'intégration avec d'autres services et de gestion automatique, tu pourras choisir l'une ou l'autre de ces solutions.


# Annexe 1 - C'est quoi un shard ?


Un **shard** (ou fragment) dans le contexte d'**Amazon Kinesis Streams** est une unité de capacité qui te permet de partitionner les données de streaming en plusieurs sous-flux. C'est un concept clé dans la gestion des flux de données en temps réel. Voici une explication détaillée :

### Qu'est-ce qu'un shard ?

Un **shard** est une unité de capacité de calcul dans Kinesis Streams, et il détermine :
- **La capacité d'ingestion** : Un shard peut ingérer des données jusqu'à **1 Mo par seconde** ou environ **1000 enregistrements par seconde**.
- **La capacité de lecture** : Chaque shard peut délivrer jusqu'à **2 Mo par seconde** aux consommateurs qui lisent les données.

### Comment fonctionnent les shards ?

Lorsqu'un flux de données (stream) est créé dans Kinesis, il est divisé en un ou plusieurs **shards**. Chaque shard reçoit une partie du flux total de données. Lorsque tu publies des données dans le flux, elles sont affectées à un shard particulier en fonction de la **clé de partition** (partition key) que tu définis lors de l'envoi des données.

Chaque shard fonctionne indépendamment des autres, ce qui te permet de **distribuer la charge** du flux de données sur plusieurs shards pour éviter un engorgement.

### Pourquoi utiliser des shards ?

1. **Scalabilité** : Tu peux augmenter ou réduire le nombre de shards dans un flux en fonction de la charge de données. Si tu prévois de recevoir un volume élevé de données, tu peux ajouter plus de shards pour mieux répartir la charge.
   
2. **Traitement en parallèle** : Chaque shard peut être consommé indépendamment par une application ou un service, ce qui permet de traiter les données en parallèle et d'augmenter la capacité de traitement du flux.

3. **Contrôle du débit** : Chaque shard a des limites de débit d'écriture (ingestion) et de lecture. Si tu as besoin de traiter de plus grandes quantités de données, tu peux ajouter des shards pour augmenter le débit global du flux.

### Exemple :
Supposons que tu as un flux de données qui traite les transactions d'un site e-commerce. Tu peux diviser le flux en plusieurs shards :
- **Shard 1** : Transactions provenant d'Amérique du Nord.
- **Shard 2** : Transactions provenant d'Europe.
- **Shard 3** : Transactions provenant d'Asie.

Chaque shard traitera indépendamment les données pour une région spécifique, permettant une **meilleure gestion** et une **scalabilité** selon les besoins de chaque région.

### Quand augmenter ou diminuer les shards ?
- **Augmenter le nombre de shards** si :
  - Le flux dépasse la capacité d'un shard (plus de 1 Mo/s d'ingestion ou plus de 1000 enregistrements/s).
  - Tu as besoin de traiter plus de données ou de lire plus de données simultanément.
  
- **Diminuer le nombre de shards** si :
  - Le flux reçoit moins de données que prévu et tu veux réduire les coûts (chaque shard coûte de l'argent).

### En résumé :
Un **shard** est une unité de base dans Kinesis Streams qui détermine la capacité de gestion des données en temps réel. En divisant le flux en plusieurs shards, tu peux mieux gérer et répartir les données pour un traitement optimisé et scalable.


