# Rapport – Pokédex : Système Avancé de Gestion d’Inventaire Pokémon
**Master 1 Informatique — Bases de Données NoSQL**  
**Projet MongoDB — 2025 / 2026**

---

# 1. Introduction

Ce projet a pour objectif de concevoir une base de données NoSQL à l’aide de MongoDB pour modéliser un Pokédex avancé comprenant :

- les espèces Pokémon,
- les Pokémon capturés par les dresseurs,
- la gestion des objets,
- les centres Pokémon,
- la création d’équipes optimales,
- et un système complet d’analyse statistique basé sur des agrégations MongoDB.

Le domaine Pokémon est idéal pour exploiter la flexibilité des données semi-structurées et la puissance du pipeline d’agrégation.

---

# 2. Étude du domaine

## 2.1 Problématique

La gestion d'un Pokédex implique :

- une **grande variabilité des données** (types, stats, évolutions),
- des **relations complexes** (dresseur → Pokémon capturé → espèce),
- des **évolutions non uniformes** (niveau, pierre…),
- des **objets avec effets**, parfois en pourcentage ou en valeur fixe,
- des **centres Pokémon** gérant l’historique de stockage,
- des **besoins analytiques** comme :
  - trouver l’équipe optimale,
  - comparer des dresseurs,
  - analyser les types dominants,
  - calculer les niveaux restants avant évolution.

MongoDB est adapté à ces besoins grâce à sa souplesse et à ses performances.

---

## 2.2 Acteurs

- **Dresseurs** : gèrent leur inventaire de Pokémon.
- **Centres Pokémon** : stockent, soignent et suivent les mouvements des Pokémon.

---

## 2.3 Données manipulées

### Dresseurs
- Identité
- Statistiques (nombre Pokémon)
- Équipe (jusqu’à 6 Pokémon)

### Espèces Pokémon
- Caractéristiques biologiques (stats, types)
- Capacités
- Évolutions et conditions d’évolution

### Pokémon capturés
- Niveau, expérience
- Surnom
- Objet tenu
- Date de capture

### Objets
- Bonus appliqués (flat ou pourcentage)
- Catégorie (combat, défense, soin)

### Centres Pokémon
- Capacité
- Stock actuel
- Historique de mouvements

---

# 3. Tableau de charge

| Tâche | Responsable | Durée |
|------|-------------|-------|
| Conception du schéma | Groupe | 3h |
| CRUD Dresseurs | Groupe | 1h |
| CRUD Espèces Pokémon | Groupe | 1h |
| CRUD Pokémon capturés | Groupe | 2h |
| Gestion des objets | Groupe | 1h |
| Centres Pokémon | Groupe | 2h |
| Équipes Pokémon | Groupe | 1h |
| Agrégations avancées | Groupe | 5h |
| Jeu de données d'exemple | Groupe | 4h |
| Tests | Groupe | 5h |
| Rapport | Groupe | 4h |
| Support de présentation | Groupe | 4h |
| **Total** |  | **33h** |

---

# 4. Fonctionnalités

## 4.1 Fonctionnalités CRUD

### Dresseur
- Ajouter / modifier / supprimer un dresseur
- Gérer l’équipe (max 6 Pokémon)

### Espèce Pokémon
- Ajouter / lire / rechercher / modifier / supprimer
- Recherche par type, génération, nom

### Pokémon capturé
- Ajouter (capture)
- Modifier (surnom, niveau, expérience, objet)
- Supprimer (relâcher)
- Lister les Pokémon d’un dresseur

### Objet
- Ajouter un objet
- Rechercher par catégorie
- Modifier un objet
- Supprimer

### Centre Pokémon
- Enregistrer une entrée (stockage)
- Enregistrer une sortie (retour au dresseur)
- Historiser les soins
- Calculer le stock actuel

---

## 4.2 Fonctionnalités avancées

- Calcul d'une **stat réelle** : stats de base + bonus de l’objet  
- Top 10 des Pokémon les plus puissants  
- Création d’une **équipe optimale** (6 meilleurs Pokémon)  
- Calcul des **niveaux restants avant évolution**  
- Comparaison des dresseurs (puissance moyenne)  
- Répartition des Pokémon par type  
- Pokémons actuellement stockés en centre  
- Historique d’activité des centres Pokémon  

(*Les pipelines MongoDB sont fournis dans les scripts mais pas détaillés dans ce rapport.*)

---

# 5. Schéma de données (MongoDB)

## 5.1 Justification du modèle

Nous utilisons un **modèle hybride** combinant :

### Documents imbriqués :
- stats d’espèce (toujours lues ensemble)
- historique des centres Pokémon

### Références :
- Pokémon capturé → espèce
- Pokémon capturé → dresseur
- Pokémon capturé → objet
- centre → pokémon capturé

### Avantages
- Évite la duplication  
- Permet des mises à jour globales rapides  
- Garantit scalabilité et cohérence  
- Facilite les agrégations multidimensionnelles

---

## 5.2 Structure détaillée

### Collection `dresseur`
```json
{
  "_id": "ObjectId",
  "idDresseur": "DR001",
  "nom": "Sacha",
  "dateCreation": "2024-01-15",
  "stats": {
    "totalPokemon": 5
  },
  "equipe": ["ObjectId"]
}
```

### Collection `pokemon` (espèce)
```json
{
  "_id": "ObjectId",
  "numeroPokedex": 25,
  "nom": "Pikachu",
  "generation": 1,
  "region": "Kanto",
  "type": "Électrik",
  "stats": {
    "hp": 35,
    "attaque": 55,
    "defense": 40,
    "attaqueSpecial": 50,
    "defenseSpecial": 50,
    "vitesse": 90
  },
  "evolutions": {
    "preEvolution": 172,
    "evolutionSuivante": 26,
    "condition": "Pierre Foudre"
  },
  "capacites": ["Éclair", "Tonnerre", "Vive-Attaque"]
}
```

### Collection `pokemons_captures`
```json
{
  "_id": "ObjectId",
  "idDresseur": "ObjectId",
  "idEspece": "ObjectId",
  "surnom": "Pika",
  "niveau": 42,
  "experience": 152340,
  "idObjet": "ObjectId",
  "dateCapture": "2024-01-20"
}
```

### Collection `objet`
```json
{
  "_id": "ObjectId",
  "nom": "Orbe Vie",
  "description": "Augmente la puissance.",
  "typeBonus": "percent",
  "bonus": {
    "stat": "attaque",
    "valeur": 10
  },
  "categorie": "combat",
  "generation": 4
}
```

### Collection `centre_pokemon`
```json
{
  "_id": "ObjectId",
  "nom": "Centre de Jadielle",
  "region": "Kanto",
  "capaciteMax": 200,
  "stockActuel": 45,
  "historiqueMouvements": [
    {
      "idPokemonCapture": "ObjectId",
      "idDresseur": "ObjectId",
      "type": "ENTREE",
      "date": "2024-03-01"
    }
  ]
}
```

# 6. Annexes
- Script 01 — Création des collections  
- Script 02 — CRUD Dresseurs  
- Script 03 — CRUD Pokémon & Pokémon capturés  
- Script 04 — CRUD Objets  
- Script 05 — Centres Pokémon  
- Script 06 — Agrégations avancées  
- Script 07 — Jeu de données d’exemple  
