#  Analyse du portefeuille clients 'Banque Nouvelle'

> Projet personnel de Data Analyst · Données **100 % fictives**, générées à des fins pédagogiques · Aucun lien avec un établissement bancaire réel.

Analyse de bout en bout d'un portefeuille bancaire fictif de 500 clients : nettoyage de données, requêtes SQL, tableaux de bord Power BI et rapport de synthèse avec recommandations business.

---

##  Objectif du projet

Reproduire la démarche complète d'un Data Analyst en contexte bancaire :

1. Nettoyer une base clients brute (540 lignes, 11 types d'anomalies)
2. Interroger les données en SQL pour en extraire des indicateurs métier
3. Construire 3 tableaux de bord Power BI interactifs
4. Rédiger un rapport de synthèse avec recommandations chiffrées

##  Principaux enseignements

- **La rentabilité repose sur une minorité de clients** : les segments Entreprise PME et Particulier Premium concentrent près des deux tiers de la marge totale (1,23 M€) pour une minorité des 500 clients.
- **Un niveau de risque de crédit élevé** : seulement 49,4 % des clients ont un statut de crédit sain ; les notations B et CCC représentent 162 clients et plus de 15 M€ d'encours à risque.
- **Une anomalie de tarification** : aucune corrélation entre le score de risque et le taux de crédit appliqué : la banque ne facture pas le risque qu'elle porte.
- **L'attrition frappe les clients rentables** : les clients partis affichent une marge annuelle moyenne supérieure à ceux qui restent (2 611 € vs 2 444 €), avec un signal clair : un faible nombre de produits détenus.

📄 Le détail complet est dans le [rapport de synthèse](./rapport/Rapport_Analyse_Banque_Nouvelle.docx).

##  Structure du dépôt

```
├── data/                   Jeux de données sources (CSV)
├── sql/                    Requêtes SQL d'analyse
├── nettoyage/              Note méthodologique de nettoyage des données
├── powerbi/                Fichier .pbix et captures des 3 dashboards
└── rapport/                Rapport de synthèse final (.docx)
```

##  Outils utilisés

| Étape | Outil |
|---|---|
| Nettoyage et préparation | Excel |
| Requêtes et agrégations | SQL |
| Modélisation et visualisation | Power BI (DAX) |
| Synthèse et recommandations | Rapport écrit |

##  Les 3 tableaux de bord

### 1. Performance commerciale
Marge par segment et par région, nombre de produits moyen, répartition de l'encours.

![Dashboard Performance](./powerbi/dashboard_1.png)

### 2. Risque de crédit
Taux de défaillance, encours par notation, nuage de points score/marge, top 20 clients à risque de départ.

![Dashboard Risque](./powerbi/dashboard_2.png)

### 3. Transactions
Volume et évolution mensuelle, répartition par canal et type d'opération.

![Dashboard Transactions](./powerbi/dashboard_3.png)

##  Qualité des données

La base brute (540 lignes) présentait 11 catégories d'anomalies : doublons, valeurs manquantes, formats hétérogènes, valeurs aberrantes, incohérences logiques. Le détail des traitements et le raisonnement derrière chaque décision (imputation vs suppression, seuils retenus) sont documentés dans la [note méthodologique](./nettoyage/Note_Nettoyage_Donnees.docx).

Taux de données utilisables après nettoyage : **92,6 %** (500 lignes exploitables sur 540).

##  Exemple de requête SQL

```sql
-- Identifier les clients les plus rentables à risque de départ
SELECT ID_Client, Segment, Age, Nb_Produits,
       Score_Risque, Marge_Annuelle_EUR, Churn_12mois
FROM Clients
WHERE Churn_12mois = 1
ORDER BY Marge_Annuelle_EUR DESC
LIMIT 20;
```

L'ensemble des 8 requêtes est disponible dans [`sql/requetes_analyse.sql`](./sql/requetes_analyse.sql).

##  Avertissement

Toutes les données de ce projet sont **fictives et générées aléatoirement**. Les niveaux observés (taux de défaillance, montants, etc.) n'ont aucune valeur représentative d'un portefeuille bancaire réel. Seule la démarche analytique est destinée à être transposable.

---

*Projet réalisé par Meriem JARTI*
