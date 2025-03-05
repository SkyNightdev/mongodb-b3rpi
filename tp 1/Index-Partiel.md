## Index Partiel

Tout les livres dispo dès le départ :

db.livres.updateMany({}, { $set: { disponible: true } })

---

Création de l'index Partiel :

db.livres.createIndex(
    { titre: 1 },
    { partialFilterExpression: { disponible: true } }
)

---

Test :

db.livres.find({ titre: "Le Mystère de la Forêt 1", disponible: true }).explain("executionStats")


Resultat du Test :

{
  explainVersion: '1',
  queryPlanner: {
    namespace: 'bibliotheque_amazon.livres',
    parsedQuery: {
      '$and': [
        {
          disponible: {
            '$eq': true
          }
        },
        {
          titre: {
            '$eq': 'Le Mystère de la Forêt 1'
          }
        }
      ]
    },
    indexFilterSet: false,
    planCacheShapeHash: '8880C31E',
    planCacheKey: 'EB4893A4',
    optimizationTimeMillis: 0,
    maxIndexedOrSolutionsReached: false,
    maxIndexedAndSolutionsReached: false,
    maxScansToExplodeReached: false,
    prunedSimilarIndexes: false,
    winningPlan: {
      isCached: false,
      stage: 'FETCH',
      inputStage: {
        stage: 'IXSCAN',
        keyPattern: {
          titre: 1
        },
        indexName: 'titre_1',
        isMultiKey: false,
        multiKeyPaths: {
          titre: []
        },
        isUnique: false,
        isSparse: false,
        isPartial: true,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: {
          titre: [
            '["Le Mystère de la Forêt 1", "Le Mystère de la Forêt 1"]'
          ]
        }
      }
    },
    rejectedPlans: [
      {
        isCached: false,
        stage: 'FETCH',
        filter: {
          disponible: {
            '$eq': true
          }
        },
        inputStage: {
          stage: 'IXSCAN',
          keyPattern: {
            titre: 1,
            note: 1
          },
          indexName: 'titre_1_note_1',
          isMultiKey: false,
          multiKeyPaths: {
            titre: [],
            note: []
          },
          isUnique: false,
          isSparse: false,
          isPartial: false,
          indexVersion: 2,
          direction: 'forward',
          indexBounds: {
            titre: [
              '["Le Mystère de la Forêt 1", "Le Mystère de la Forêt 1"]'
            ],
            note: [
              '[MinKey, MaxKey]'
            ]
          }
        }
      }
    ]
  },
  executionStats: {
    executionSuccess: true,
    nReturned: 0,
    executionTimeMillis: 1,
    totalKeysExamined: 0,
    totalDocsExamined: 0,
    executionStages: {
      isCached: false,
      stage: 'FETCH',
      nReturned: 0,
      executionTimeMillisEstimate: 0,
      works: 2,
      advanced: 0,
      needTime: 0,
      needYield: 0,
      saveState: 0,
      restoreState: 0,
      isEOF: 1,
      docsExamined: 0,
      alreadyHasObj: 0,
      inputStage: {
        stage: 'IXSCAN',
        nReturned: 0,
        executionTimeMillisEstimate: 0,
        works: 1,
        advanced: 0,
        needTime: 0,
        needYield: 0,
        saveState: 0,
        restoreState: 0,
        isEOF: 1,
        keyPattern: {
          titre: 1
        },
        indexName: 'titre_1',
        isMultiKey: false,
        multiKeyPaths: {
          titre: []
        },
        isUnique: false,
        isSparse: false,
        isPartial: true,
        indexVersion: 2,
        direction: 'forward',
        indexBounds: {
          titre: [
            '["Le Mystère de la Forêt 1", "Le Mystère de la Forêt 1"]'
          ]
        },
        keysExamined: 0,
        seeks: 1,
        dupsTested: 0,
        dupsDropped: 0
      }
    }
  },
  queryShapeHash: 'BF736DB3ED23F313E844DD9DF35D90E0E8C707208A7542BAB84455F73BB9C58A',
  command: {
    find: 'livres',
    filter: {
      titre: 'Le Mystère de la Forêt 1',
      disponible: true
    },
    '$db': 'bibliotheque_amazon'
  },
  serverInfo: {
    host: 'mongodb-shard-00-01.rb73r.mongodb.net',
    port: 27017,
    version: '8.0.4',
    gitVersion: 'bc35ab4305d9920d9d0491c1c9ef9b72383d31f9'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 16793600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 33554432,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1741186841, i: 3 }),
    signature: {
      hash: Binary.createFromBase64('4OKeXl2pQN8FRnsvAUhoDxta3Fo=', 0),
      keyId: 7421229370343162000
    }
  },
  operationTime: Timestamp({ t: 1741186841, i: 3 })
}