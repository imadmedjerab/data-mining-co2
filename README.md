# 🌍 Prévision des Émissions de CO2 — Analyse de Séries Temporelles

Système de prévision et d'analyse des émissions de CO2 à partir de données temporelles réelles (135 408 enregistrements), comparant 4 approches de modélisation : ARIMA, SARIMA, LSTM et Transformer.

## 📋 Contexte du projet

Projet réalisé dans le cadre du module Data Mining (Université d'Alger 1, année 2024/2025), en équipe (Kelbouz Sif-eddine, Medjerab Imad, Ayadi Amar Yacine, Bouzidi Abderrahim).

Le dioxyde de carbone (CO2) est l'un des principaux gaz à effet de serre responsables du réchauffement climatique. Ce projet propose une modélisation prédictive des émissions à partir de données historiques massives, en combinant des méthodes statistiques classiques et des techniques d'apprentissage automatique, afin d'anticiper les tendances et de soutenir la prise de décision environnementale.

## 📊 Dataset

- **Source** : [CO2 Emissions Dataset — Kaggle](https://www.kaggle.com/datasets/saloni1712/co2-emissions)
- **Taille** : 135 408 enregistrements
- **Variables** : `country`, `date`, `sector`, `value` (Mt de CO2), `timestamp`
- Aucune valeur manquante ni doublon détecté

## 🔧 Prétraitement des données

- Détection de valeurs aberrantes par IQR (17 519 valeurs identifiées) → remplacées par la médiane
- Conversion des dates en `datetime`, encodage one-hot de `country` et `sector`
- Normalisation MinMax de `value`
- Extraction de variables temporelles : `year`, `month`, `quarter`
- Agrégation mensuelle et tri chronologique

## 🧠 Modèles comparés

Quatre approches évaluées via validation croisée temporelle (`TimeSeriesSplit`, 5 plis) :

| Modèle | Type | Configuration |
|--------|------|----------------|
| **ARIMA** | Statistique classique | ARIMA(1,1,1) |
| **SARIMA** | Statistique + saisonnalité | SARIMA(1,1,1)(1,1,1,12) |
| **LSTM** | Deep Learning | 64 unités, séquences de 12 mois, 30 époques |
| **Transformer** | Deep Learning (attention) | MultiHeadAttention + LayerNorm + GlobalAveragePooling |

## 📈 Résultats — Performances moyennes (5 plis de validation croisée)

| Modèle | MSE | RMSE | MAE | **R²** |
|--------|-----|------|-----|--------|
| ARIMA | 0.0131 | 0.1021 | 0.0896 | **0.60** |
| LSTM | 0.0027 | 0.0502 | 0.0448 | **0.74** |
| **SARIMA** ⭐ | **0.0020** | **0.0433** | **0.0372** | **0.89** |
| Transformer | 0.0057 | 0.0724 | 0.0607 | **0.68** |

### 🎯 Zoom sur le R² (coefficient de détermination)

Le R² mesure la capacité du modèle à expliquer la variance des données par rapport à une simple moyenne de référence (R² = 1 : prédiction parfaite, R² = 0 : équivalent à prédire la moyenne, R² < 0 : moins bon qu'une prédiction naïve).

- **SARIMA obtient le R² le plus proche de 1 (0.89)** — de très loin le meilleur score du comparatif, signe qu'il explique la quasi-totalité de la variance des émissions grâce à sa prise en compte explicite de la saisonnalité.
- **LSTM (0.74)** se classe deuxième, porté par des MSE/RMSE/MAE parmi les plus faibles du comparatif.
- **Transformer (0.68)** et **ARIMA (0.60)** ferment la marche, expliquant une part significative mais nettement moindre de la variance.

👉 **Conclusion clé** : sur toutes les métriques, y compris et surtout le R², **SARIMA domine largement**, confirmé par la figure de comparaison graphique (barplots MSE/RMSE/MAE/R²) et par la projection à 12 mois qui capture fidèlement le comportement cyclique annuel des émissions.

## 🔍 Analyse interprétative

- **SARIMA** l'emporte grâce à sa capacité à intégrer explicitement la saisonnalité mensuelle, caractéristique dominante des données de CO2.
- **LSTM** se classe second (R²=0.74) et affiche même les meilleures valeurs de MSE/RMSE/MAE après SARIMA, montrant que le réseau récurrent parvient à bien capter la dynamique de la série malgré la relative simplicité du signal.
- **Transformer** (R²=0.68) reste sous-exploité : avec seulement 12 mois de séquences univariées, l'attention multi-tête manque de matière pour être pleinement efficace.
- **ARIMA** ferme la marche (R²=0.60) : il ne capte pas la saisonnalité, ce qui limite sa capacité à généraliser sur un signal cyclique.

## 🔮 Projection à 12 mois

Le modèle SARIMA, entraîné sur l'ensemble de la série mensuelle agrégée, a été utilisé pour projeter l'évolution des émissions de CO2 sur les 12 mois suivants. La tendance projetée respecte fidèlement le comportement saisonnier observé historiquement, confirmant la capacité du modèle à capter les cycles annuels.

## 🛠️ Technologies utilisées

- **Langage** : Python
- **Séries temporelles** : statsmodels (ARIMA, SARIMAX)
- **Deep Learning** : TensorFlow / Keras (LSTM, Transformer)
- **Validation** : scikit-learn (TimeSeriesSplit)
- **Visualisation** : Matplotlib, Seaborn

## 👤 Auteurs

Kelbouz Sif-eddine, **Medjerab Imad**, Ayadi Amar Yacine, Bouzidi Abderrahim
Université d'Alger 1 — Année universitaire 2024/2025

[LinkedIn Imad](https://linkedin.com/in/imadmedjerab-8400a0341) | imadmedjerab94@gmail.com
