# DB-DDI : Génération d’une base de données synthétique sur les interactions médicamenteuses
![Illustration du projet](médicaments.jpg)

##  Table des matières

1. [Description du projet](#description-du-projet)
2. [Objectifs du projet](#objectifs-du-projet)
3. [Sources de données](#sources-de-données)
4. [Technologies utilisées](#technologies-utilisées)
5. [Structure du projet](#structure-du-projet)
6. [Processus ETL](#processus-etl)
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
   - Contient des données de prescriptions hospitalières réelles.  
   - Utilisée pour reconstruire des ordonnances réalistes.  
   - Lien : [https://physionet.org/content/mimiciii-demo/1.4/](https://physionet.org/content/mimiciii-demo/1.4/)

2. **Mendeley DDI Dataset**  
   - Fournit des paires de médicaments connus pour interagir.  
   - Utilisée pour identifier les interactions dans les ordonnances.  
   - Lien : [https://data.mendeley.com/datasets/md5czfsfnd/1](https://data.mendeley.com/datasets/md5czfsfnd/1)

3. **DrugBank (via Kaggle)**  
   - Contient des descriptions textuelles détaillées des interactions médicamenteuses.  
   - Utilisée pour enrichir chaque interaction avec un texte explicatif.  
   - Lien : [https://www.kaggle.com/datasets/sergeguillemart/drugbank](https://www.kaggle.com/datasets/sergeguillemart/drugbank)

4. **DDInter Database**  
   - Donne une classification de la sévérité des interactions (`mineure`, `modérée`, `sévère`, etc.).  
   - Utilisée pour attribuer un niveau de gravité à chaque interaction.  
   - Lien : [https://ddinter.scbdd.com/download/](https://ddinter.scbdd.com/download/)






