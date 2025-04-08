# **Contexte général**

Tu avais **une seule application** (monolithique) qui faisait tout.

Tu veux maintenant :
- **Séparer** cette application en **2 services** indépendants :  
  - un pour les **clients (lecture seulement)**  
  - un pour les **employés (ajout, modification, suppression)**
- Les rendre **plus faciles à déployer**, **plus flexibles**, **scalables** et **modernes**, comme le font les grandes entreprises (Netflix, Amazon, etc.)


# PHASE 4 – "On sépare l’application en deux petits morceaux, et on les met dans des boîtes"

Imagine que tu découpes ton appli en **2 mini-applis** :  
- une boîte pour les **clients**  
- une autre pour les **employés**

### En clair :
- Tu modifies un peu le **code** pour que chaque boîte fasse seulement son travail.
- Tu crées une **recette Docker (Dockerfile)** pour que chaque microservice puisse être mis dans une boîte **transportable**.
- Tu testes les deux boîtes localement sur ta machine Cloud9 :
  - Le microservice client tourne sur le **port 8080**
  - Le microservice employé sur le **port 8081**

 **Bilan** : Tu as maintenant **deux conteneurs Docker** qui tournent bien **en local**.



# ☁️ PHASE 5 – "On met nos boîtes dans le Cloud et on les prépare à être déployées automatiquement"

Maintenant que tout fonctionne **en local**, tu veux :

> "Déployer ça dans un vrai environnement Cloud, pour que ça soit rapide, stable, automatique et accessible depuis Internet."

### Tu fais quoi concrètement ?

## 1. **Créer un entrepôt à images Docker (ECR)**  
Comme un **Google Drive pour tes conteneurs**.  
Tu envoies (`push`) tes deux images là-bas.

## 2. **Créer un cluster Fargate (ECS)**  
Fargate, c’est le **service de livraison automatique de conteneurs** chez AWS.  
Tu lui dis : "Tiens, prends cette image Docker et exécute-la pour moi."

## 3. **Créer un dépôt Git pour les fichiers de déploiement (CodeCommit)**  
Tu mets dedans des fichiers qui expliquent :
- **Quelle image utiliser ?**
- **Sur quel port ?**
- **Avec quelles infos de configuration ?**
- **Quel conteneur correspond à quel service ?**

## 4. **Créer deux fichiers de configuration pour ECS (`taskdef`)**  
Ce sont des **fiches d’identité** pour chaque microservice :
- nom de l'image
- port utilisé
- rôle IAM, etc.

## 5. **Créer deux fichiers `appspec.yaml` pour CodeDeploy**  
Ce sont des **plans de déploiement** :  
"Voici comment déployer ce service dans ECS."

## 6. **Tu pushes tout dans CodeCommit**  
C’est comme GitHub, mais dans AWS.  
Plus tard, tu connecteras ça à **CodePipeline** pour que tout soit déployé automatiquement quand tu changes le code.



##  Résumé ultra-simple :

| Phase | Objectif humainement compréhensible |
|-------|--------------------------------------|
| Phase 4 | Transformer l’appli en deux mini-apps (microservices), les tester dans des conteneurs Docker |
| Phase 5 | Mettre les conteneurs Docker dans le cloud AWS (ECR), et préparer des fichiers pour que le système puisse les déployer automatiquement dans ECS (via CodeDeploy et CodePipeline) |

