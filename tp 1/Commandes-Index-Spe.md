Création de l’index texte :

db.livres.createIndex({ titre: "text", description: "text" })

---

Recherche textuelle test :

db.livres.find({ $text: { $search: "Mystère" } })


---

Création de la collection sessions_utilisateurs avec session de test :

db.sessions_utilisateurs.insertOne({
    utilisateurId: "user_123",
    derniere_activite: new Date(),
    session_data: "Données quelconques de session"
})

---

Création de l’index TTL (30 minutes) :

db.sessions_utilisateurs.createIndex({ derniere_activite: 1 }, { expireAfterSeconds: 1800 })

---

### 1.4

Création de l'index  :

db.livres.createIndex({ titre: 1, note: 1 })

---

Requete :

db.livres.find(
    { titre: "Le Mystère de la Forêt 1" },
    { titre: 1, note: 1, _id: 0 }
).explain("executionStats")

Resultat : totalDocsExamined = 0