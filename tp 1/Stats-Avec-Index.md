# Satistiques Requête

Requête	-> db.livres.find({ titre: "Le Mystère de la Forêt 1" }).explain("executionStats")
totalDocsExamined -> 0
executionTimeMillis	-> 1
Étape -> IXSCAN

--- 

Requête	-> db.livres.find({ auteur: "Victor Hugo" }).explain("executionStats")
totalDocsExamined -> 155
executionTimeMillis	-> 1
Étape -> IXSCAN

---

Requête	-> 
db.livres.find({
    prix: { $gte: 10, $lte: 20 },
    note: { $gte: 4 }
}).explain("executionStats")

totalDocsExamined -> 131
executionTimeMillis	-> 1
Étape -> IXSCAN

---

Requête	-> 
db.livres.find({
    genre: "Fiction",
    langue: "Français"
}).sort({ note: -1 }).explain("executionStats")

totalDocsExamined -> 32
executionTimeMillis	-> 1
Étape -> IXSCAN