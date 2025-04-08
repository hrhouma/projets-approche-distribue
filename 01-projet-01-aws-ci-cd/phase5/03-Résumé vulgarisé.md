

# **Phase 4 : On découpe et on met dans des boîtes**

Imagine que ton appli est **trop grosse** pour bien fonctionner. Tu veux la **couper en 2** :

- Une mini-appli pour les **clients** (juste voir les infos)
- Une mini-appli pour les **employés** (ajouter, modifier, supprimer)

Ensuite, tu mets **chaque mini-appli dans une boîte (un conteneur Docker)**.

👉 Comme si tu mettais deux petits jeux dans deux boîtes séparées, pour les tester avant de les offrir.



# ☁️ **Phase 5 : On envoie les boîtes dans le Cloud**

Maintenant que tes mini-applis fonctionnent bien, tu veux :

1. **Les stocker en ligne** (dans un "entrepôt AWS" appelé ECR)
2. **Demander à AWS de les exécuter automatiquement** (avec un service qui s’appelle ECS)
3. **Préparer un petit plan de déploiement** (comme une fiche de montage IKEA)
4. **Envoyer tout ça dans un dépôt Git** (CodeCommit), pour que plus tard, tout se fasse **tout seul à chaque mise à jour**.



##  En une phrase :
> Phase 4 = Je découpe l’appli et je mets les morceaux dans des boîtes.  
> Phase 5 = J’envoie les boîtes dans le cloud et je prépare les instructions pour que tout fonctionne automatiquement.

