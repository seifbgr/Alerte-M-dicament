# DB-DDI : Génération d’une base de données synthétique sur les interactions médicamenteuses

## 📝 Description du projet

Ce projet a pour objectif de générer un **jeu de données synthétique** en agrégeant plusieurs sources de données ouvertes afin de construire un système d’alerte sur les **interactions médicamenteuses** identifiées dans les ordonnances.

Nous exploitons des jeux de données accessibles publiquement, notamment via **Kaggle** et d’autres plateformes open data, pour reconstituer des ordonnances réalistes et en extraire les interactions médicamenteuses.

Le travail repose initialement sur la **base de données MIMIC-III Clinical Database Demo (version 1.4)**, qui contient des données relatives à 94 patients. Cette base réduite sert de point de départ pour développer une méthodologie généralisable à l’ensemble de la base MIMIC-III complète. L’objectif est de concevoir un **modèle d’apprentissage automatique** capable de détecter la présence d’interactions et de prédire leur **niveau de gravité** au sein de chaque ordonnance.

À plus long terme, cette base synthétique pourra servir de support au développement d’un **système intelligent d’alerte et de triage**, capable de résumer et de hiérarchiser les risques liés aux interactions médicamenteuses, dans une perspective d’aide à la décision clinique.
