# Système de Positionnement RTT avec ESP32

## Description du Projet
Ce projet implémente un système de positionnement en intérieur utilisant la technologie RTT (Round Trip Time) avec deux ESP32. Le système utilise des réseaux de neurones profonds pour améliorer la précision des mesures de distance et prédire les positions.

## Structure du Projet

```
IA-Imen/
├── rtt_positioning.py        # Script principal du système de positionnement
├── indoor_positioning.ipynb  # Notebook Jupyter pour l'analyse et la visualisation
├── esp32_sniffer/           # Code pour l'ESP32
├── requirements.txt         # Dépendances du projet
└── README.md               # Documentation du projet
```

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

## Installation

1. Cloner le dépôt :
```bash
git clone [url-du-dépôt]
cd IA-Imen
```

2. Installer les dépendances depuis requirements.txt :
```bash
pip install -r requirements.txt
```

## Utilisation

### Avec le Notebook Jupyter
Ouvrir et exécuter `indoor_positioning.ipynb` pour l'analyse interactive et la visualisation.

### Avec le Script Python
```python
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

## Dépendances
Voir le fichier `requirements.txt` pour la liste complète des dépendances.

Principales bibliothèques :
- TensorFlow
- NumPy
- Pandas
- Scikit-learn
- Matplotlib
- Seaborn

## Licence
MIT License

## Remerciements
- TensorFlow Team
- Communauté ESP32
