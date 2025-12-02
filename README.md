# Pokédex – Système Avancé de Gestion d’Inventaire Pokémon

Projet MongoDB • Master 1 Informatique  
UE : Bases de Données NoSQL — Année 2025/2026  

---

# 📂 Structure du projet

```
groupF_IBI/
├── projet.pdf
├── rapport/
│   └── rapport.md
├── scripts-mongodb/
│   ├── 01-creation-collections.mongo
│   ├── 02-crud-dresseurs.mongo
│   ├── 03-crud-pokemon.mongo
│   ├── 04-crud-objet.mongo
│   ├── 05-centre-pokemon.mongo
│   ├── 06-agregations.mongo
│   └── 07-donnees-exemple.mongo
└── README.md
```

---

# ⚙️ Installation & Configuration

## 🔧 Prérequis

- MongoDB
- mongosh

---

# 🚀 Initialisation

```bash
mongosh pokedex scripts-mongodb/01-creation-collections.mongo
mongosh pokedex scripts-mongodb/07-donnees-exemple.mongo
```

---

# 🧪 Scripts disponibles

## 👤 CRUD Dresseurs

```bash
mongosh pokedex scripts-mongodb/02-crud-dresseurs.mongo
```

Fonctions :
- creerDresseur()
- lireDresseur()
- listerDresseurs()
- modifierDresseur()
- supprimerDresseur()
- assignerPokemonEquipe()
- retirerPokemonEquipe()

---

## 🐾 CRUD Pokémon

```bash
mongosh pokedex scripts-mongodb/03-crud-pokemon.mongo
```

Inclut :
- CRUD espèces Pokémon
- CRUD Pokémon capturés

---

## 🎒 CRUD Objets

```bash
mongosh pokedex scripts-mongodb/04-crud-objet.mongo
```

---

## 🏥 Centres Pokémon

```bash
mongosh pokedex scripts-mongodb/05-centre-pokemon.mongo
```

---

## 📊 Agrégations avancées

```bash
mongosh pokedex scripts-mongodb/06-agregations.mongo
```

Agrégations :
- Stat réelle
- Top 10 puissance
- Équipe optimale
- Niveaux avant évolution
- Répartition des types
- Classement dresseurs
- Stock centres
- Historique centres

---

# 📘 Exemple d’utilisation au niveau applicatif (javascript)

```javascript
use('pokedex');
load('scripts-mongodb/02-crud-dresseurs.mongo');
load('scripts-mongodb/03-crud-pokemon.mongo');
load('scripts-mongodb/06-agregations.mongo');

creerDresseur({ nom: "Sacha", idDresseur: "DR001" });
capturerPokemon("DR001", 25, { surnom: "Pika" });
meilleureEquipe("DR001");
```

---

# 🗂️ Schéma de données

Collections :
- dresseur
- pokemon (espèces)
- pokemons_captures (individuels)
- objet
- centre_pokemon

---

# 📝 Fonctionnalités

✔ CRUD complet  
✔ Objets et bonus  
✔ Centres Pokémon  
✔ Équipe (6 max)  
✔ Stat réelle  
✔ Agrégations avancées  
✔ Données d’exemple  

---

# 📎 Annexes

- Scripts 01–07
- Rapport complet
