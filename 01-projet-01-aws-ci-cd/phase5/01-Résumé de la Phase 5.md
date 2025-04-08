# **PHASE 5 – Objectif**

- Déployer les microservices **customer** et **employee** dans un environnement **scalable**, **cloud-native**, et **automatisé** via **Amazon ECS avec Fargate**, **ECR**, **CodeDeploy** et **CodeCommit**.

<br/> 

# **Tâche 5.1 : Création des dépôts Amazon ECR et upload des images Docker**

1. **Authentifier Docker** auprès d’ECR via `aws ecr get-login-password`.
2. **Créer deux dépôts privés ECR** : `customer` et `employee`.
3. **Configurer les permissions** des dépôts (ouverture aux actions ECR via une politique JSON).
4. **Taguer les images Docker locales** avec l’ID de compte AWS.
5. **Pousser les images Docker vers ECR** :
   ```bash
   docker push $account_id.dkr.ecr.us-east-1.amazonaws.com/customer:latest
   docker push $account_id.dkr.ecr.us-east-1.amazonaws.com/employee:latest
   ```
6. **Vérifier sur la console ECR** que les deux images sont bien stockées avec le tag `latest`.


<br/> 

# **Tâche 5.2 : Création du cluster ECS Fargate**

1. **Créer un cluster ECS Fargate** nommé `microservices-serverlesscluster` (ou suffixé `-c` si nom déjà utilisé).
2. Le cluster utilise :
   - VPC : `LabVPC`
   - Sous-réseaux : `PublicSubnet1` et `PublicSubnet2`
3. Lancer la création via **CloudFormation**, attendre le statut `CREATE_COMPLETE`.


<br/> 

# **Tâche 5.3 : Création du dépôt CodeCommit `deployment`**

1. **Créer un dépôt CodeCommit** nommé `deployment`.
2. Dans Cloud9 :
   - Créer un dossier `deployment`
   - L'initialiser avec Git (branche `dev`) pour stocker les fichiers de déploiement (taskdef, appspec).


<br/> 

# **Tâche 5.4 : Création des fichiers `taskdef-xxx.json` pour ECS**

1. Créer `taskdef-customer.json` avec les paramètres :
   - Image Docker : `customer`
   - Port : `8080`
   - Variable d’environnement : `APP_DB_HOST`
   - Logging `awslogs` activé
   - Role IAM : `PipelineRole`
   - Compatibilité : `FARGATE`

2. **Enregistrer la définition de tâche** dans ECS avec :
   ```bash
   aws ecs register-task-definition --cli-input-json "file://.../taskdef-customer.json"
   ```

3. Répéter pour `taskdef-employee.json` :
   - Image : `employee`
   - Port : `8081` (initialement, puis changera à 8080)
   - Enregistrer aussi avec AWS CLI.


<br/> 

#  **Tâche 5.5 : Création des fichiers AppSpec pour CodeDeploy**

1. Créer `appspec-customer.yaml` :
   ```yaml
   version: 0.0
   Resources:
     - TargetService:
         Type: AWS::ECS::Service
         Properties:
           TaskDefinition: <TASK_DEFINITION>
           LoadBalancerInfo:
             ContainerName: "customer"
             ContainerPort: 8080
   ```

2. Créer `appspec-employee.yaml`, identique sauf :
   - `ContainerName: "employee"`
   - `ContainerPort: 8080`

*Ces fichiers sont utilisés par **CodeDeploy** pour effectuer le **déploiement continu sur ECS**.*


<br/> 

#  **Tâche 5.6 : Mise à jour des fichiers et commit dans CodeCommit**

1. Modifier les fichiers `taskdef-xxx.json` pour remplacer l'image :
   ```json
   "image": "<IMAGE1_NAME>"
   ```
   - Ce **placeholder** sera remplacé dynamiquement par **CodePipeline** lors du déploiement.
2. **Committer et pousser** les 4 fichiers (`2 taskdef + 2 appspec`) dans le dépôt `deployment`.

<br/> 

#  Résumé des artefacts créés

| Type de fichier         | Nom                           | Utilisé par         |
|-------------------------|-------------------------------|----------------------|
| Docker Image            | `customer`, `employee`        | Amazon ECR           |
| Task Definition         | `taskdef-customer.json`       | Amazon ECS           |
|                         | `taskdef-employee.json`       | Amazon ECS           |
| AppSpec (YAML)          | `appspec-customer.yaml`       | AWS CodeDeploy       |
|                         | `appspec-employee.yaml`       | AWS CodeDeploy       |
| Repo de config          | `deployment` (CodeCommit)     | CodePipeline (CI/CD) |


<br/> 

# Annexe : 


Parfait ! Tu as maintenant terminé la phase critique de **conteneurisation des microservices**. La **Phase 5** s’attaque à la **mise en production progressive** sur une architecture scalable, avec **Amazon ECS + Fargate + ECR + CodeDeploy + CodePipeline**.

Voici une explication **du "pourquoi"** derrière chaque tâche de la **Phase 5**, pour t’aider à bien comprendre l’intention architecturale :


##  Objectif global de la Phase 5

> **Passer d’un simple test local (Docker sur Cloud9) à un déploiement scalable, automatisé, robuste en production sur AWS.**

Tu cherches à :
- **Mettre les images Docker dans un registre (ECR)** pour qu’ECS puisse les déployer.
- **Déployer automatiquement** via des services managés (Fargate, ECS, CodeDeploy).
- **Préparer le CI/CD** via CodePipeline avec des fichiers de configuration clairs.



##  **Pourquoi utiliser Amazon ECR ?** *(Task 5.1)*

- ECR (Elastic Container Registry) est un **registre privé** pour stocker et versionner les images Docker.
- ECS ou d’autres services peuvent **tirer automatiquement l’image** depuis ECR.
- Tu dois taguer les images avec ton **Account ID** et les pousser pour qu’ECS les retrouve plus tard.



##  **Pourquoi créer un cluster ECS (Fargate) ?** *(Task 5.2)*

- ECS (Elastic Container Service) te permet de **déployer des conteneurs à grande échelle**.
- Fargate élimine le besoin de gérer les serveurs : **100 % serverless**, AWS gère l'infrastructure.
- Tu définis un **cluster Fargate** avec **2 sous-réseaux publics**, ce qui est parfait pour un accès externe via un Load Balancer.



##  **Pourquoi un dépôt CodeCommit nommé `deployment` ?** *(Task 5.3)*

- Tu as besoin d’un **dépôt Git centralisé pour versionner** les fichiers de déploiement (taskdefs, appspec).
- Plus tard, **CodePipeline utilisera ce dépôt** comme source pour déployer automatiquement chaque microservice.
- Il te permet de suivre l’évolution de la config déploiement **de façon indépendante** du code applicatif.



##  **Pourquoi créer des fichiers `taskdef-xxx.json` ?** *(Task 5.4)*

- Les fichiers `taskdef` sont des **Task Definitions ECS** au format JSON :
  - Spécifie l’image à utiliser
  - Le port exposé
  - Les variables d’environnement
  - Le rôle d’exécution IAM
  - Le logging (`awslogs`)
- C’est **la base du déploiement dans ECS**. Sans ces fichiers, ECS ne peut pas savoir **comment lancer les conteneurs**.



##  **Pourquoi les fichiers `appspec-xxx.yaml` ?** *(Task 5.5)*

- AppSpec est utilisé par **CodeDeploy** pour comprendre **quoi déployer, où et comment**.
- Il lie :
  - Le nom du **container**
  - Le **port**
  - Le **LoadBalancer**
  - Le **Task Definition**
- Il sert à **automatiser les mises à jour** sans temps d’arrêt et à **déclencher des hooks de déploiement** (comme `BeforeInstall`, `AfterInstall`, etc. si besoin).



##  **Pourquoi remettre `<IMAGE1_NAME>` dans les taskdef ?** *(Task 5.6)*

- Pour préparer le déploiement dynamique via **CodePipeline**, tu remplaces l’image Docker fixe par un **placeholder dynamique**.
- CodePipeline le remplacera automatiquement lors du déploiement par **la dernière image pushée dans ECR**.
- Cela permet de **déployer automatiquement** la bonne version après chaque build.



# Résumé visuel du pipeline en construction :

```
CodeCommit (déploiement) ──┐
                          ▼
                    CodePipeline
                          ▼
           ┌────> Build + Test (CodeBuild) ─┐
           │                               ▼
Image Docker (ECR)               TaskDef + AppSpec
           │                               ▼
           └─────────> CodeDeploy ──────> ECS (Fargate)
                                           │
                                Load Balancer (ALB)
```

![image](https://github.com/user-attachments/assets/fe2afa04-53c9-467c-a354-b033c5ad4c20)

