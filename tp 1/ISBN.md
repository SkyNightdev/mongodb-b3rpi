Création de l'index Unique :

db.livres.createIndex({ ISBN: 1 }, { unique: true })

db.livres.insertOne({
    titre: "Livre en double",
    auteur: "Dupont",
    ISBN: "ISBN-123456789"
})

-> E11000 duplicate key error collection: bibliotheque_amazon.livres index: ISBN_1 dup key: { ISBN: "ISBN-123456789" }
