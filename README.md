# Système de Positionnement RTT avec ESP32

## Description du Projet
Ce projet implémente un système de positionnement en intérieur utilisant la technologie RTT (Round Trip Time) avec deux ESP32. Le système utilise des réseaux de neurones profonds pour améliorer la précision des mesures de distance et prédire les positions.

## Configuration du Système

| Paramètre | Valeur |
|-----------|---------|
| Largeur de la pièce | 6.47 mètres |
| Hauteur de la pièce | 4.85 mètres |
| Résolution de la grille | 0.5m × 0.5m |
| Position ESP32_1 | (-2, 1.5) |
| Position ESP32_2 | (2.5, -1.5) |

## Architecture des Modèles

### 1. RCDN (RTT Compensation Distance Network)
```
Entrée → Conv1D(32) → ReLU → Conv1D(32) → ReLU → MaxPooling1D → Dense(64) → Sortie(2)
```
- **Entrée** : Échantillons RTT (10 pas de scan, 2 ESP32)
- **Sortie** : Distances compensées pour 2 ESP32

### 2. RPN (Region Proposal Network)
```
Entrée → GRU(32) → GRU(16) → Dense(2)
```
- **Entrée** : Distances compensées
- **Sortie** : Coordonnées (x, y)

## Fonctionnalités Principales

- Génération de données synthétiques
- Compensation des distances RTT
- Prédiction des positions en temps réel
- Visualisation des résultats
- Calcul des métriques de performance

## Dépendances

```python
numpy
tensorflow
pandas
scikit-learn
matplotlib
seaborn
```

## Installation

1. Cloner le dépôt :
```bash
git clone [url-du-dépôt]
cd rtt-positioning-system
```

2. Installer les dépendances :
```bash
pip install numpy tensorflow pandas scikit-learn matplotlib seaborn
```

## Utilisation

```python
# Exemple d'utilisation
from rtt_positioning import RTTPositioningSystem

# Initialiser le système
system = RTTPositioningSystem(scanning_steps=10)

# Générer des données synthétiques
data = generate_synthetic_data(n_points=200, n_samples=10)

# Entraîner le modèle
system.train_and_evaluate(X_train, y_train, X_val, y_val, epochs=100)
```

## Visualisations

Le système génère plusieurs visualisations :
- Historique d'entraînement (perte et précision)
- Positions réelles vs prédites
- Distribution des erreurs

## Métriques de Performance

Le système calcule automatiquement :
- RMSE (Root Mean Square Error)
- Erreur moyenne
- Écart-type des erreurs

## Structure du Code

```
rtt_positioning.py
├── class RTTPositioningSystem
│   ├── __init__()
│   ├── _build_rcdn()
│   ├── _build_rpn()
│   ├── _build_combined_model()
│   ├── custom_loss()
│   └── train_and_evaluate()
├── plot_training_history()
├── visualize_positioning_results()
├── calculate_metrics()
└── generate_synthetic_data()
```

## Auteurs
- [Votre nom]

## Licence
MIT License

## Remerciements
- TensorFlow Team
- Communauté ESP32
