# DB-DDI : Génération d’une base de données synthétique sur les interactions médicamenteuses
![Illustration du projet](médicaments.jpg)

## 📚 Table des matières

- [📝 Description du projet](#-description-du-projet)
- [🎯 Objectifs du projet](#-objectifs-du-projet)
- [📂 Sources de données](#-sources-de-données)
- [🛠️ Technologies utilisées](#️-technologies-utilisées)
- [🏗️ Structure du projet](#️-structure-du-projet)
- [⚙️ Processus ETL](#️-processus-etl)
- [🚀 Lancement du projet](#-lancement-du-projet)
- [📊 Résultats](#-résultats)
- [✅ Conclusion et perspectives](#-conclusion-et-perspectives)
- [👤 Auteur](#-auteur)


## 📝 Description du projet

Ce projet a pour objectif de générer un **jeu de données synthétique** en agrégeant plusieurs sources de données ouvertes afin de construire un système d’alerte sur les **interactions médicamenteuses** identifiées dans les ordonnances.

Nous exploitons des jeux de données accessibles publiquement, notamment via **Kaggle** et d’autres plateformes open data, pour reconstituer des ordonnances réalistes et en extraire les interactions médicamenteuses.

Le travail repose initialement sur la **base de données MIMIC-III Clinical Database Demo (version 1.4)**, qui contient des données relatives à 94 patients. Cette base réduite sert de point de départ pour développer une méthodologie généralisable à l’ensemble de la base MIMIC-III complète. L’objectif est de concevoir un **modèle d’apprentissage automatique** capable de détecter la présence d’interactions et de prédire leur **niveau de gravité** au sein de chaque ordonnance.

À plus long terme, cette base synthétique pourra servir de support au développement d’un **système intelligent d’alerte et de triage**, capable de résumer et de hiérarchiser les risques liés aux interactions médicamenteuses, dans une perspective d’aide à la décision clinique.

## 🎯 Objectifs du projet

L’objectif principal est de développer une **base de données synthétique d’interactions médicamenteuses** à partir de données ouvertes, afin de permettre par la suite la mise en place d’un système intelligent d’alerte et de triage. Plus précisément, le projet vise à :

- 🔄 **Reconstituer des ordonnances médicales réalistes** à partir de la base MIMIC-III (version demo) ;
- 🧩 **Identifier les interactions médicamenteuses** présentes dans ces ordonnances à l’aide de bases de données DDI ;
- 🗒️ **Associer à chaque interaction des informations textuelles et un niveau de sévérité** ;


Ce travail constitue une **étape préparatoire** indispensable à l’entraînement d’un modèle prédictif performant, dans un contexte médical critique.

