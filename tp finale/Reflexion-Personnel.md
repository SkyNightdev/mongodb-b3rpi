Pour ce TP, j'ai exploré deux approches pour la gestion des emprunts dans MongoDB :
---
1/ les Emprunts intégrés directement dans les documents utilisateurs

Avantages :

- Cette approche est simple à mettre en place.
- Toutes les informations concernant un utilisateur sont regroupées dans un seul document.
- Cela peut être suffisant pour une bibliothèque avec peu d’emprunts par utilisateur.

Inconvénients :

- Si un utilisateur emprunte beaucoup de livres, son document peut devenir très volumineux, ce qui risque d’impacter les performances.
- Il devient plus complexe de faire des statistiques globales sur les emprunts (ex : nombre total d’emprunts d’un livre particulier) sans devoir parcourir toute la collection des utilisateurs.

---

2/ Emprunts stockés dans une collection séparée emprunts avec des références (utilisateur_id et livre_id)

Avantages :

- Cette structure est plus modulaire et suit une logique proche d’une base relationnelle, ce qui est plus naturel pour gérer un historique d’emprunts.
- Il est plus facile de faire des analyses globales (ex : quels sont les livres les plus empruntés ? combien d’emprunts par mois ?).
- La collection utilisateurs reste légère, car elle ne porte pas directement l’historique.

Inconvénients :

- Lorsqu’on veut afficher les emprunts d’un utilisateur, il faut faire une jointure (lookup), ce qui est un peu plus complexe.
- Il y a une légère surcharge en termes de requêtes, car les informations sont réparties sur plusieurs collections.

---

Choix Personnel :

je privilégierais la solution avec une collection séparée pour les emprunts.

C’est une approche plus professionnelle qui respecte mieux les principes de la normalisation des données, ce qui facilite la maintenance de l’application sur le long terme.