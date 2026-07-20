# Gestion-donn-es-immobili-re
Une gestion d'une base de données immobilière
Voici une proposition de fichier **README.md** exhaustif et professionnel pour ce deuxième projet stratégique (**DATAImmo**), structuré pour refléter parfaitement la progression, les technologies utilisées (Databricks, SQL) et les livrables attendus par Clara Daucourt.

---

# 📝 README.md

# Projet DATAImmo — Optimisation Technologique & Modélisation de la Base de Données

## 📌 Présentation du Projet

Le projet **DATAImmo** est une initiative stratégique majeure du réseau national d'agences immobilières **Laplace Immo**. Porté par la CTO Clara Daucourt, l'objectif à terme est de concevoir un modèle prédictif avancé pour estimer le prix de vente des biens immobiliers afin de démarquer l'agence de sa concurrence.

En tant que Data Analyst / Engineer, notre mission consiste à **refondre, normaliser et migrer l'infrastructure de données** de l'entreprise vers le Cloud, en intégrant de nouvelles sources géographiques et démographiques pour enrichir les analyses de marché des agences régionales.

---

## 🎯 Objectifs & Phases du Projet

### Phase 1 : Conception, Normalisation & Conformité

* **Dictionnaire des données :** Création d'un référentiel décrivant l'ensemble des variables issues des 3 sources de données (DVF, INSEE, Data.gouv).
* **Modélisation Relationnelle (3NF) :** Évolution de l'ancien schéma pour intégrer les notions de régions et de recensements de population, en respectant strictement la **Troisième Forme Normale (3NF)** pour éviter toute redondance.
* **Audit RGPD :** Analyse de conformité pour s'assurer de l'absence de données d'identification personnelle (PII) dans les transactions foncières publiques.

### Phase 2 : Migration Cloud & Implémentation (Databricks)

* **Infrastructure :** Abandon des outils locaux (SQLite) au profit d'un environnement moderne et scalable sur **Databricks**.
* **Pipeline de Données (ETL) :** Importation, nettoyage (types, valeurs manquantes, chaînes textuelles) et création physique des tables SQL sur le Cloud.
* **Contrôle Qualité :** Exécution de requêtes de contrôle (comptage de lignes, unicité des clés primaires).

### Phase 3 : Analyses Métier & Requêtage SQL

* Extraction d'indicateurs clés demandés dans le Compte-Rendu (CR) de réunion pour guider les agences régionales.
* Optimisation des requêtes complexes à l'aide de jointures et d'**alias explicites** pour maximiser la lisibilité du livrable de présentation.

---

## 📁 Sources des Données

Le projet fusionne trois gisements de données distincts :

1. **Données DVF (Demandes de valeurs foncières) :** Historique des mutations, prix de vente, surfaces, types de biens (maisons, appartements) et géolocalisation.
2. **Données INSEE :** Recensements démographiques de la population par commune/département.
3. **Données Data.gouv :** Référentiel géographique complet de la France (Communes, Unités urbaines, Aires urbaines, Départements, Académies, Régions).

---

## 🛠️ Stack Technique

* **Modélisation :** DBDiagram.io / Draw.io (Conception du schéma conceptuel et logique en 3NF).
* **Environnement Cloud :** **Databricks** (Workspace & Delta Lake).
* **Langage Principal :** **SQL (Spark SQL)** pour le requêtage, la création des tables (`CREATE TABLE`), et les jointures d'agrégation.
* **Gouvernance :** Cadre réglementaire RGPD (Anonymisation native des données DVF).

---

## 📐 Architecture de la Base de Données (Norme 3NF)

Le modèle de données a été découpé pour isoler les entités indépendantes :

* `REGIONS` $\rightarrow$ `DEPARTEMENTS` $\rightarrow$ `COMMUNES` (Hiérarchie géographique propre éliminant les dépendances transitives).
* `POPULATION` : Liée à l'entité `COMMUNES` par le code INSEE et indexée par année.
* `MUTATIONS` (Transactions) : Contient les détails des ventes, connectée aux biens immobiliers et aux communes.

---

## 🚀 Guide d'Exécution sur Databricks

### 1. Initialisation des Tables

Exécutez le script SQL de création des tables dans votre notebook Databricks pour monter l'infrastructure :

```sql
CREATE TABLE IF NOT EXISTS dataimmo.communes (
    code_insee STRING NOT NULL,
    nom_commune STRING,
    code_departement STRING,
    CONSTRAINT pk_communes PRIMARY KEY (code_insee)
);
-- Répéter pour l'ensemble des entités validées dans le schéma relationnel

```

### 2. Requête de Contrôle (Exemple)

Pour valider l'intégrité de l'importation de la table des transactions :

```sql
SELECT 
    COUNT(*) AS total_transactions,
    COUNT(DISTINCT id_mutation) AS identifiants_uniques
FROM dataimmo.mutations;

```

---

## 👥 Équipe Projet

* **Clara Daucourt** : CTO (Supervision, validation de l'architecture et de la migration Cloud).
* **Directeur Général** : Sponsor de l'initiative stratégique "DATAImmo".
* **Vous** : Data Engineer / Analyst en charge de la conception 3NF, de l'implémentation Databricks et du calcul des KPI.
