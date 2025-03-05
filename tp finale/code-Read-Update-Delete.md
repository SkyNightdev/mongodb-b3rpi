// PARTIE 2

// LIVRE DISPO
db.livres.find({disponible: true});

// LIVRE APRES 2000
db.livres.find({annee_publication: {$gt: 2000}});

// LIVRE AUTEUR
db.livres.find({auteur: "J.K. Rowling"});

// LIVRE NOTE < 4
db.livres.find({note_moyenne: {$gt: 4}});

// UTILISATEUR A LYON
db.utilisateurs.find({"adresse.ville": "Lyon"});

// LIVRE FANTASY
db.livres.find({genre: "Fantasy"});

// PRIX < 15€ & NOTE > 4
db.livres.find({prix: {$lt: 15}, note_moyenne: {$gt: 4}});

// EMPRUNT EN 1984
db.utilisateurs.find({"livres_empruntes.titre": "1984"});


// PARTIE 3

// MODIFIER LE TITRE
db.livres.updateOne({titre: "1984"}, {$set: {titre: "Nineteen Eighty-Four"}});

// AJOUTER AU STOCK
db.livres.updateMany({}, {$set: {stock: 5}});

// MARQUER INDISPO
db.livres.updateOne({titre: "Le Petit Prince"}, {$set: {disponible: false}});

// AJOUTER UN EMPRUNT
db.utilisateurs.updateOne({email: "marie.dupont@example.com"}, {$push: {livres_empruntes: {livre_id: ObjectId("..."), titre: "1984"}}});



// PARTIE 4

// SUPPR UN LIVRE
db.livres.deleteOne({titre: "1984"});

// SUPPR LIVRE GRACE A L AUTEUR
db.livres.deleteMany({auteur: "Victor Hugo"});

// SUPPR UN UTILISATEUR
db.utilisateurs.deleteOne({email: "thomas.martin@example.com"});



// PARTIE 5

// TRIE PAR NOTE
db.livres.find().sort({note_moyenne: -1});

// 3 PLUS ANCIENS
db.livres.find().sort({annee_publication: 1}).limit(3);

// NBR LIVRE PAR AUTEUR
db.livres.aggregate([{$group: {_id: "$auteur", total: {$sum: 1}}}]);

// PROJECTION TITRE/AUTEUR/NOTE
db.livres.find({}, {titre: 1, auteur: 1, note_moyenne: 1, _id: 0});