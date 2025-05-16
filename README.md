# DB-DDI : Génération d’une base de données synthétique sur les interactions médicamenteuses
![Illustration du projet](médicaments.jpg)

##  Table des matières

1. [Description du projet](#description-du-projet)
2. [Objectifs du projet](#objectifs-du-projet)
3. [Sources de données](#sources-de-données)
4. [Technologies utilisées](#technologies-utilisées)
5. [Processus ETL](#processus-etl)
7. [Lancement du projet](#lancement-du-projet)
8. [Résultats](#résultats)
9. [Conclusion et perspectives](#conclusion-et-perspectives)
10. [Auteur](#auteur)

##  Description du projet

Ce projet a pour objectif de générer un **jeu de données synthétique** en agrégeant plusieurs sources de données ouvertes afin de construire un système d’alerte sur les **interactions médicamenteuses** identifiées dans les ordonnances.

Nous exploitons des jeux de données accessibles publiquement, notamment via **Kaggle** et d’autres plateformes open data, pour reconstituer des ordonnances réalistes et en extraire les interactions médicamenteuses.

Le travail repose initialement sur la **base de données MIMIC-III Clinical Database Demo (version 1.4)**, qui contient des données relatives à 94 patients. Cette base réduite sert de point de départ pour développer une méthodologie généralisable à l’ensemble de la base MIMIC-III complète. L’objectif est de concevoir un **modèle d’apprentissage automatique** capable de détecter la présence d’interactions et de prédire leur **niveau de gravité** au sein de chaque ordonnance.

À plus long terme, cette base synthétique pourra servir de support au développement d’un **système intelligent d’alerte et de triage**, capable de résumer et de hiérarchiser les risques liés aux interactions médicamenteuses, dans une perspective d’aide à la décision clinique.

##  Objectifs du projet

L’objectif principal de ce projet est de **constituer une base de données synthétique centralisant les interactions médicamenteuses**, incluant pour chacune :

- le **type d’interaction**,
- une **description textuelle explicative**,
- un **niveau de sévérité** (`mineure`, `modérée`, `sévère`, etc.).

Pour atteindre cet objectif, le travail s'articule autour des étapes suivantes :

-  **Reconstituer des ordonnances médicales réalistes** à partir de la base MIMIC-III (version demo) ;
-  **Identifier les interactions médicamenteuses** présentes dans ces ordonnances à l’aide de bases de données DDI ;
-  **Associer à chaque interaction des informations textuelles et un niveau de sévérité**, en s’appuyant sur des ressources comme DrugBank et DDInter.
###  Résultats attendus

À l’issue du processus, la base de données synthétique construite permettra d’obtenir des résultats structurés sous forme tabulaire, contenant les informations suivantes pour chaque interaction médicamenteuse détectée dans une ordonnance :

| ordonnance_id | hadm_id | ordonnance_date | drug1_name   | drug2_name | interaction_type                  | description                                           | level    |
|---------------|---------|------------------|--------------|------------|-----------------------------------|-------------------------------------------------------|----------|
| ORD00000      | 100375  | 2129-05-02       | acetaminophen | atorvastatin | risk or severity of adverse effects | The risk or severity of adverse effects can be...    | Sévère      |
| ORD00006      | 100969  | 2142-11-28       | lorazepam     | midazolam   | risk or severity of adverse effects | The risk or severity of adverse effects can be...    | Mineure      |
| ORD00018      | 101361  | 2145-12-15       | pantoprazole  | tacrolimus  | serum concentration                | The serum concentration of Tacrolimus can be increa...| Moderate |

## Sources de données

Le projet s'appuie sur plusieurs bases de données ouvertes et fiables issues du domaine médical et pharmaceutique :

1. **MIMIC-III Clinical Database Demo (v1.4)**  
   - Contient les données de prescriptions hospitalières réelles de 94 patients.  
   - Utilisée pour reconstruire des ordonnances réalistes.  
   - Lien : [https://physionet.org/content/mimiciii-demo/1.4/](https://physionet.org/content/mimiciii-demo/1.4/)
   - Les colonnes suivantes sont utilisées pour reconstruire les ordonnances à partir du fichier `PRESCRIPTIONS.csv` de la base MIMIC-III :

| Colonne       | Description                                                                 |
|---------------|-----------------------------------------------------------------------------|
| `hadm_id`     | Identifiant unique de l'hospitalisation. Il permet de regrouper les prescriptions par passage hospitalier. |
| `startdate`   | Date de début de la prescription. Elle est utilisée pour reconstituer la chronologie de l'ordonnance.         |
| `drug_name_poe` | Nom du médicament prescrit tel qu'enregistré dans le système de prescription (POE = Physician Order Entry). |

2. **Mendeley DDI Dataset**
   - Fournit des paires de médicaments connus pour interagir.
   - Utilisée pour identifier les interactions dans les ordonnances.
   - Ajouter le type d'interaction à la base de donnée. 
   - Lien : [https://data.mendeley.com/datasets/md5czfsfnd/1](https://data.mendeley.com/datasets/md5czfsfnd/1)

4. **DrugBank (via Kaggle)**  
   - Contient des descriptions textuelles détaillées des interactions médicamenteuses.
    - faire une joiture avec la base de données principale MIMIC  
   - Utilisée pour enrichir chaque interaction avec un texte explicatif (ajouter la description pour chaque interaction).  
   - Lien : [https://www.kaggle.com/datasets/sergeguillemart/drugbank](https://www.kaggle.com/datasets/sergeguillemart/drugbank)

5. **DDInter Database**  
   - Donne une classification de la sévérité des interactions (`mineure`, `modérée`, `sévère`, etc.).
   - faire une joiture avec la base de données principale MIMIC 
   - Utilisée pour attribuer un niveau de gravité à chaque interaction.  
   - Lien : [https://ddinter.scbdd.com/download/](https://ddinter.scbdd.com/download/)

## Technologies utilisées

Le projet repose sur les outils et technologies suivants :

### Langage & environnement

- Python 3.10+
- Jupyter Notebook

### Bibliothèques Python utilisées

- pandas : manipulation et nettoyage des données tabulaires (DataFrames)
- numpy : calculs numériques et opérations vectorielles
- itertools : création de combinaisons et permutations (traitement d’interactions)
- lxml.etree : parsing et traitement de fichiers XML (ex : DrugBank)
- pathlib : gestion des chemins de fichiers de manière portable
- glob : recherche de fichiers par motifs dans l’arborescence

## Processus ETL

Le projet suit une démarche de type **ETL** (Extract – Transform – Load) pour construire une base de données synthétique d’interactions médicamenteuses à partir de données ouvertes.

### 1. Extraction (Extract)

- Chargement du fichier `PRESCRIPTIONS.csv` issu de la base MIMIC-III (version demo).
- Extraction des colonnes nécessaires : `hadm_id`, `startdate`, `drug_name_poe`.
- Téléchargement des bases externes :
  - DDI (Mendeley)
  - DrugBank (Kaggle)
  - DDInter (classification de la sévérité)

### 2. Transformation (Transform)

- **Regroupement des prescriptions par `hadm_id` et `startdate`** pour reconstituer des **ordonnances** complètes par patient et par date.
- Génération de toutes les combinaisons de médicaments (paires) dans chaque ordonnance à l’aide de `itertools`.
- Recherche des interactions correspondantes dans la base DDI.
- **Nettoyage des données** :
  - Suppression des doublons de médicaments et d'interactions.
  - Normalisation des noms (minuscule, retrait des caractères spéciaux, etc.).
- **Enrichissement** des paires détectées avec :
  - Description textuelle depuis DrugBank.
  - Niveau de sévérité depuis DDInter.

### 3. Chargement (Load)

- Génération d’un tableau final contenant :
  - `ordonnance_id`, `hadm_id`, `date`, `drug_1`, `drug_2`, `interaction_type`, `description`, `level`
- Export au format `.csv` pour permettre une analyse ultérieure ou un entraînement de modèle.

## Lancement du projet

Ce projet est basé sur Python et Jupyter Notebook. Suivez les étapes ci-dessous pour installer l’environnement et exécuter les notebooks.

### Prérequis

Avant de commencer, vous devez avoir installé :

* Python 3.10 ou plus récent
* pip (gestionnaire de paquets Python)
* Jupyter Notebook ou JupyterLab

### Étapes d’exécution

#### 1. Cloner le dépôt

```
git clone https://github.com/ton-utilisateur/ton-depot.git
cd ton-depot
```

#### 2. Créer un environnement virtuel

```
python -m venv venv
source venv/bin/activate  # Sous Windows : venv\Scripts\activate
```

#### 3. Installer les dépendances

```
pip install -r requirements.txt
```

#### 4. Lancer Jupyter Notebook

```
jupyter notebook
```

#### 5. Exécuter le notebook principal du projet :

`project_db_ddi.ipynb` : Ce notebook regroupe l'ensemble des étapes du projet, de l'extraction des prescriptions à la génération du jeu de données final enrichi avec descriptions et niveaux de sévérité.

## Résultats

À l’issue de l’exécution du projet, un tableau final a été généré, contenant des interactions médicamenteuses extraites et enrichies. Voici les résultats clés :

-  **974 interactions détectées**
-  Issues de **94 patients**
-  Correspondant à **124 hospitalisations distinctes**
-  **617 interactions n’ont pas de niveau de sévérité trouvé** dans la base de données DDInter

### Exemple de structure de données :

| ordonnance_id | hadm_id | ordonnance_date | drug1_name    | drug2_name  | interaction_type                   | description                                              | level    |
|---------------|---------|------------------|---------------|-------------|------------------------------------|----------------------------------------------------------|----------|
| ORD00000      | 100375  | 2129-05-02       | acetaminophen | atorvastatin | risk or severity of adverse effects | The risk or severity of adverse effects can be...         | NaN      |
| ORD00006      | 100969  | 2142-11-28       | lorazepam     | midazolam    | risk or severity of adverse effects | The risk or severity of adverse effects can be...         | NaN      |
| ORD00018      | 101361  | 2145-12-15       | pantoprazole  | tacrolimus   | serum concentration                 | The serum concentration of Tacrolimus can be increased...| Moderate |
| ORD00019      | 102203  | 2127-07-23       | clotrimazole  | diazepam     | metabolism                          | The metabolism of Diazepam can be decreased when...       | Moderate |
| ORD00019      | 102203  | 2127-07-23       | clotrimazole  | ondansetron  | metabolism                          | The metabolism of Ondansetron can be decreased...         | Unknown  |

Ce tableau constitue la base pour le développement d’un futur **modèle de machine learning** visant à identifier et prioriser les risques médicamenteux.

## Conclusion et perspectives

La base de données générée dans ce projet est issue d’un environnement de **démonstration** basé sur la version réduite de **MIMIC-III DEMO**, contenant uniquement les données de **94 patients**. Elle constitue une preuve de concept permettant de valider le pipeline de traitement, d’extraction et d’enrichissement des interactions médicamenteuses.

L’objectif final est d’appliquer ce même traitement sur la **base complète MIMIC-III**, afin de générer une **base de données à grande échelle** (big data) contenant toutes les colonnes nécessaires : type d’interaction, description textuelle, niveau de sévérité.

Cette base enrichie sera utilisée pour développer un **modèle prédictif** capable d’identifier automatiquement les interactions présentes dans une ordonnance. L’ambition est de proposer un **système d’aide à la prescription**, afin de prévenir les risques d’interactions dangereuses et contribuer à **améliorer la sécurité des patients**.

## Auteur

Ce projet a été réalisé par **Seif Bouguerra**, dans le cadre d’un **stage au laboratoire ICube**, durant son Master en Intelligence en Données de Santé.

Il a été encadré par le **Professeur Julien Godet**.












