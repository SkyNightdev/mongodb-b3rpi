Ajoute une loc aleatoire a CHAQUE utilisateurs :

db.utilisateurs.updateMany({}, { 
    $set: { 
        adresse: { 
            rue: "Adresse inconnue",
            ville: "Ville inconnue",
            code_postal: "00000",
            localisation: { 
                type: "Point", 
                coordinates: [ 
                    (Math.random() * 10).toFixed(5), // Longitude
                    (Math.random() * 10).toFixed(5)  // Latitude
                ]
            }
        }
    }
})

---

Créer un index sur Utilisateurs :

db.utilisateurs.createIndex({ "adresse.localisation": "2dsphere" })

---

