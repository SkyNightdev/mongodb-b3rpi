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

Trouver 5 utilisateurs les plus proches :

db.utilisateurs.find({
    "adresse.localisation": {
        $near: {
            $geometry: { type: "Point", coordinates: [2.3522, 48.8566] }, // Paris
            $maxDistance: 5000  // 5000 mètres = 5 km
        }
    }
}).limit(5)

---

Bibliotheque la plus proche d un User :

// Remplace "ID_UTILISATEUR" par l'ID réel d'un utilisateur dans ta base
var utilisateur = db.utilisateurs.findOne({ _id: ObjectId("67c80b86a5976583a764859c") });

db.bibliotheques.find({
    "localisation": {
        $near: {
            $geometry: utilisateur.adresse.localisation
        }
    }
}).limit(1)

---

Bibliotheque la plus proche d un user en fonction d une distance :

db.bibliotheques.aggregate([
    {
        $geoNear: {
            near: {
                type: "Point",
                coordinates: [2.3522, 48.8566] // Paris
            },
            distanceField: "distance",
            spherical: true
        }
    }
])

---

Trouver tous les users dans une zone précise (fictive pour l exemple) :

db.utilisateurs.find({
    "adresse.localisation": {
        $geoWithin: {
            $geometry: {
                type: "Polygon",
                coordinates: [[
                    [2.34, 48.85],
                    [2.37, 48.85],
                    [2.37, 48.87],
                    [2.34, 48.87],
                    [2.34, 48.85]  
                ]]
            }
        }
    }
})

---

Trouver les users autour d une bibliotheque avec un zone precise :

db.bibliotheques.updateOne(
    { nom: "Bibliothèque de Paris" },
    { $set: { zone_service: {
        type: "Polygon",
        coordinates: [[
            [2.34, 48.85],
            [2.37, 48.85],
            [2.37, 48.87],
            [2.34, 48.87],
            [2.34, 48.85]
        ]]
    }}}
)

---

Création d'une Collection "rues", et ensuite rajouter les Linestring :

db.rues.insertOne({
    nom: "Avenue de la République",
    tracé: {
        type: "LineString",
        coordinates: [
            [2.34, 48.85],
            [2.36, 48.86]
        ]
    }
})

---

Création d'une Collection "livraisons" et ajouter une livraison avec id user et id livre :

db.livraisons.insertOne({
    livre_id: ObjectId("67c85bb685fb690e77554c40"),
    utilisateur_id: ObjectId("67c80b86a5976583a764859e"),
    depart: {
        type: "Point",
        coordinates: [2.3522, 48.8566]  // Paris
    },
    arrivee: {
        type: "Point",
        coordinates: [2.295, 48.8738]  // Proche de la Tour Eiffel
    },
    position_actuelle: {
        type: "Point",
        coordinates: [2.34, 48.86]
    },
    itineraire: {
        type: "LineString",
        coordinates: [
            [2.3522, 48.8566],
            [2.34, 48.86],
            [2.295, 48.8738]
        ]
    }
})

---

Mise a jour position d une livraison :

db.livraisons.updateOne(
    { livre_id: ObjectId("67c85bb685fb690e77554c40") },
    { $set: { position_actuelle: { type: "Point", coordinates: [2.345, 48.865] } } }
)

---

Recherche une livraison dans une zone :

db.livraisons.find({
    position_actuelle: {
        $near: {
            $geometry: { type: "Point", coordinates: [2.3522, 48.8566] },
            $maxDistance: 10000
        }
    }
})
