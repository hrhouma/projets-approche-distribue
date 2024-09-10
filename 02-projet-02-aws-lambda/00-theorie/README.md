---
# Qu'est-ce qu'AWS Lambda ? 
---

AWS Lambda est un service de calcul sans serveur proposé par Amazon Web Services (AWS), qui permet d'exécuter du code en réponse à des événements spécifiques sans avoir à gérer des serveurs. En d'autres termes, vous pouvez écrire du code, le télécharger sur Lambda, et AWS se charge du reste : gestion des ressources, montée en charge, et facturation uniquement pour le temps d'exécution du code.

---
# Concepts de base :
---

1. **Fonctions** : Une "fonction Lambda" est le code que vous écrivez et exécutez en réponse à un événement. Il peut être écrit dans plusieurs langages comme Python, Node.js, Java, Go, Ruby, et plus.
   
2. **Événements** : Un événement est une action qui déclenche l'exécution de la fonction Lambda. Les événements peuvent provenir de services AWS comme S3 (téléchargement de fichier), DynamoDB (modifications de la base de données), ou encore API Gateway (requêtes HTTP).

3. **Sans Serveur (Serverless)** : Le modèle "sans serveur" signifie que vous n'avez pas à vous soucier de l'infrastructure sous-jacente (serveurs, système d'exploitation, etc.). Tout est géré par AWS. Vous ne vous concentrez que sur le code, et AWS s'occupe de tout ce qui est en arrière-plan.

---
# Fonctionnement d'AWS Lambda :
---

1. **Création d'une fonction Lambda** :
   - Vous écrivez une fonction dans un langage supporté.
   - Vous définissez les événements qui déclencheront cette fonction (par exemple, une requête API, un téléchargement de fichier dans S3, etc.).
   - Vous spécifiez des permissions en utilisant des rôles IAM (Identity and Access Management), qui permettent à la fonction d'accéder à d'autres ressources AWS comme DynamoDB ou S3.
   
2. **Exécution en réponse à des événements** :
   - Lorsqu'un événement se produit (par exemple, une image est téléchargée dans un bucket S3), cet événement déclenche l'exécution de la fonction Lambda.
   - AWS Lambda instancie automatiquement votre code dans un environnement isolé (conteneur), exécute la fonction, puis supprime l'environnement une fois la tâche accomplie.

3. **Montée en charge automatique** :
   - AWS Lambda est conçu pour s'adapter automatiquement à la demande. Si 1000 événements se produisent en même temps, Lambda exécute jusqu'à 1000 instances de votre fonction en parallèle.
   - La mise à l'échelle est automatique, et vous n'avez pas à configurer manuellement le nombre de serveurs ou la taille des instances.

4. **Facturation à l'utilisation** :
   - AWS Lambda vous facture uniquement pour le temps réel d'exécution de votre fonction, mesuré au milliseconde près. Il n'y a aucun coût pour les serveurs inactifs.
   - Vous bénéficiez également d'un certain nombre de requêtes gratuites chaque mois (1 million d'appels gratuits et 400 000 Go-seconde d'exécution).

---
# Qu'est-ce que l'architecture "serverless" (sans serveur) ?
---

L'architecture **serverless** (sans serveur) est un modèle de développement où les développeurs n'ont pas besoin de gérer les serveurs, les bases de données, ou l'infrastructure réseau. Le fournisseur de cloud (comme AWS) s'occupe de l'infrastructure, permettant ainsi aux développeurs de se concentrer uniquement sur le code et la logique métier.

---
# Avantages du modèle serverless :
---

1. **Pas de gestion des serveurs** : Vous n'avez plus besoin de provisionner, gérer ou entretenir des serveurs. AWS gère tout cela pour vous.
   
2. **Montée en charge automatique** : Le service s'ajuste automatiquement en fonction de la charge. Que vous receviez 10 requêtes par jour ou 10 000 requêtes par seconde, Lambda s'adapte à la demande.

3. **Coûts réduits** : Vous ne payez que lorsque votre code est exécuté. Contrairement aux serveurs traditionnels, où vous devez payer même lorsque le serveur est inactif, avec Lambda, il n'y a aucun coût lorsque votre fonction n'est pas utilisée.

4. **Agilité** : Le modèle serverless permet aux équipes de développement de se concentrer sur l'innovation plutôt que sur la gestion de l'infrastructure. Les déploiements sont plus rapides, et la gestion de la capacité ou de la sécurité est simplifiée.

5. **Réduction de la complexité** : Puisqu'il n'y a pas de gestion manuelle des serveurs, cela réduit la complexité opérationnelle. Vous pouvez aussi intégrer facilement Lambda à d'autres services AWS (API Gateway, S3, DynamoDB, etc.).

---
# Comment AWS Lambda fonctionne-t-il avec d'autres services AWS ?
---

AWS Lambda s'intègre de manière fluide avec de nombreux autres services AWS pour créer des solutions complètes sans serveur.

---
# Exemples d'intégrations :
---

- **Amazon S3** : Lorsque vous téléchargez un fichier sur S3, cela peut déclencher une fonction Lambda qui traite ou analyse ce fichier. Par exemple, vous pouvez redimensionner une image ou transformer un fichier CSV en un autre format.

- **Amazon DynamoDB** : Chaque fois qu'il y a un changement dans une table DynamoDB (ajout, modification ou suppression de données), une fonction Lambda peut être appelée pour traiter cette modification.

- **Amazon API Gateway** : API Gateway vous permet de créer des API RESTful. Chaque requête HTTP (GET, POST, etc.) peut déclencher une fonction Lambda qui exécute la logique métier et renvoie une réponse au client.

- **Amazon CloudWatch** : Vous pouvez utiliser CloudWatch pour surveiller les métriques et les logs générés par vos fonctions Lambda. En outre, vous pouvez configurer des alarmes qui déclenchent Lambda en cas de dépassement de seuil de certaines métriques.

---
# Cas d'utilisation typiques de AWS Lambda
---

## 1. **Traitement en temps réel** :
   - Par exemple, une fonction Lambda peut être utilisée pour analyser des logs en temps réel provenant d'Amazon CloudWatch ou traiter des flux de données venant d'Amazon Kinesis.

## 2. **API backend serverless** :
   - Vous pouvez utiliser AWS Lambda pour créer le backend d'une application mobile ou web sans gérer de serveurs. API Gateway déclenche des fonctions Lambda en réponse à des requêtes HTTP, offrant ainsi une API RESTful complète.

## 3. **Automatisation des tâches** :
   - AWS Lambda peut automatiser des tâches telles que la génération de rapports, les sauvegardes de bases de données ou encore la maintenance des infrastructures cloud.

## 4. **Transformation de données** :
   - Par exemple, une entreprise qui reçoit des fichiers CSV pourrait utiliser Lambda pour automatiser la transformation de ces fichiers en base de données, en automatisant le traitement de chaque fichier uploadé.

## 5. **Traitement d'images ou de vidéos** :
   - Chaque image ou vidéo uploadée dans un bucket S3 peut être analysée ou transformée par une fonction Lambda. Par exemple, Lambda peut être utilisé pour générer des miniatures d'images ou encoder des vidéos dans différents formats.

---
# Fonctionnement interne de Lambda
---

1. **Conteneurisation automatique** : Chaque fonction Lambda est exécutée dans un environnement isolé appelé "conteneur". AWS crée et gère ces conteneurs de manière dynamique et éphémère pour exécuter votre code. Cela garantit une isolation complète de vos fonctions, permettant une meilleure sécurité et gestion des ressources.

2. **Exécution stateless** : Lambda est conçu pour des fonctions stateless (sans état). Cela signifie que chaque exécution d'une fonction est indépendante des autres et ne conserve pas de données entre les appels. Si vous avez besoin de persistance, vous devrez utiliser des services comme S3 ou DynamoDB pour stocker des données.

3. **Gestion des permissions** : AWS Lambda utilise IAM pour gérer les permissions. Chaque fonction Lambda doit avoir un rôle IAM qui définit les services et ressources auxquels la fonction peut accéder.

4. **Gestion de la concurrence** : Par défaut, Lambda peut exécuter jusqu'à 1000 fonctions en parallèle, mais ce plafond peut être augmenté sur demande.

---
# Exemple d'architecture serverless avec AWS Lambda
---

Imaginez une application de traitement d'image :

- **Étape 1** : Un utilisateur télécharge une image dans un bucket S3.
- **Étape 2** : L'événement de téléchargement déclenche une fonction Lambda.
- **Étape 3** : La fonction Lambda redimensionne l'image et la stocke dans un autre bucket S3.
- **Étape 4** : Un journal de traitement est créé dans une table DynamoDB, indiquant que l'image a été traitée avec succès.

Cet exemple montre comment plusieurs services AWS peuvent interagir de manière fluide avec Lambda pour automatiser des processus complexes, tout en étant extrêmement évolutif.

---
# Conclusion
---

AWS Lambda et l'architecture serverless représentent une révolution dans le développement d'applications. En supprimant la complexité de la gestion des serveurs et de l'infrastructure, Lambda permet aux développeurs de se concentrer uniquement sur le développement de fonctionnalités et de solutions métier, tout en bénéficiant d'une facturation à l'usage, d'une mise à l'échelle automatique et d'une grande simplicité d'intégration avec l'ensemble de l'écosystème AWS.

---
# Annexe 01 - exemple simple d'utilisation d'AWS Lambda 
---

- Exemple d'architecture serverless avec AWS Lambda : Envoi d'une notification après téléchargement d'un fichier
- Comment vous pouvez utiliser AWS Lambda pour automatiser une tâche simple comme l'envoi de notifications ?


#### Étape 1 : Téléchargement d'un fichier dans un bucket S3
Un utilisateur télécharge un fichier (par exemple, une image ou un document) dans un bucket Amazon S3.

#### Étape 2 : Déclenchement d'une fonction Lambda
Le téléchargement du fichier génère un événement dans S3. Cet événement déclenche automatiquement une fonction AWS Lambda.

#### Étape 3 : Envoi d'une notification via SNS
La fonction Lambda se contente d'envoyer une notification via **Amazon SNS** (Simple Notification Service) à une liste de destinataires, informant qu'un nouveau fichier a été téléchargé dans le bucket S3.

#### Étape 4 : Fin du processus
Une fois que la notification a été envoyée, le travail de la fonction Lambda est terminé.

### Code Lambda en Python :
Voici un exemple de code simple en Python qui pourrait être utilisé dans Lambda pour envoyer une notification avec SNS :

```python
import json
import boto3

def lambda_handler(event, context):
    sns = boto3.client('sns')
    
    # Extraction des informations du fichier téléchargé
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Message à envoyer
    message = f"Un nouveau fichier a été téléchargé dans le bucket {bucket} : {key}"
    
    # Envoi de la notification
    response = sns.publish(
        TopicArn='arn:aws:sns:region:123456789012:NomDuTopic',
        Message=message,
        Subject='Nouveau fichier dans S3'
    )
    
    return {
        'statusCode': 200,
        'body': json.dumps('Notification envoyée avec succès!')
    }
```

### Résumé des étapes clés :
1. **S3** : L'utilisateur télécharge un fichier dans un bucket.
2. **Lambda** : La fonction Lambda est déclenchée par l'événement de téléchargement et envoie une notification.
3. **SNS** : Lambda envoie une notification à une liste d'abonnés via Amazon SNS.

### Avantages de cette solution :
- **Simplicité** : Il s'agit d'un flux simple, idéal pour des notifications ou des alertes en temps réel.
- **Sans serveur** : Pas besoin de gérer de serveur ou d'infrastructure, tout est automatisé et évolutif.
- **Rapidité** : Les notifications sont envoyées presque immédiatement après le téléchargement du fichier.

