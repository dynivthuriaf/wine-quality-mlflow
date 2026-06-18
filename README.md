# wine-quality-mlflow

Un projet MLOps qui utilise MLflow pour suivre, comparer et déployer un modèle de régression de la qualité du vin.

Ce projet entraîne un réseau de neurones artificiel (ANN) sur le jeu de données `winequality-white.csv`, effectue une recherche d'hyperparamètres avec Hyperopt, suit les expériences dans MLflow, enregistre le modèle et le prépare pour un déploiement en tant qu'API REST.

## Fonctionnalités

- Entraînement d'un modèle Keras pour prédire la qualité du vin blanc
- Optimisation d'hyperparamètres avec `hyperopt`
- Suivi des métriques, paramètres et modèles avec `mlflow`
- Enregistrement des meilleurs modèles dans le registre MLflow
- Déploiement du modèle en API REST via `mlflow models serve`

## Structure du dépôt

- `starter.ipynb` : notebook principal contenant le flux d'entraînement, le suivi MLflow, l'enregistrement du modèle et des exemples d'inférence
- `mlruns/` : dossier MLflow local généré contenant les exécutions et les modèles
- `.gitignore` : fichiers et dossiers ignorés par Git

## Prérequis

- Python 3.8+
- Git
- Un environnement virtuel Python (recommandé)
- MLflow installé

## Installation

1. Clone le dépôt si nécessaire :

```bash
git clone https://github.com/dynivthuriaf/wine-quality-mlflow.git
cd wine-quality-mlflow
```

2. Crée et active un environnement virtuel :

Windows PowerShell :
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Linux / macOS :
```bash
python3 -m venv .venv
source .venv/bin/activate
```

3. Installe les dépendances :

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

## Exécution du projet

1. Lance le notebook `starter.ipynb` avec Jupyter ou Visual Studio Code.
2. Exécute les cellules dans l'ordre pour :
   - charger les données
   - préparer les jeux de données
   - entraîner le modèle
   - suivre les expériences dans MLflow
   - enregistrer le modèle

## Visualisation MLflow

Lance l'interface MLflow localement :

```bash
mlflow ui
```

Puis ouvre `http://127.0.0.1:5000` dans ton navigateur pour consulter les runs, les paramètres et les modèles.

## Déploiement local en API REST

Après avoir enregistré un modèle dans MLflow, tu peux le servir localement.

1. Identifie l'URI du modèle à partir du MLflow UI ou du notebook. Par exemple :

```text
runs:/<run-id>/model
```

2. Démarre le serveur REST :

```bash
mlflow models serve -m "runs:/<run-id>/model" -p 1234
```

3. Envoie une requête POST pour prédire :

```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"columns": ["fixed acidity", "volatile acidity", "citric acid", "residual sugar", "chlorides", "free sulfur dioxide", "total sulfur dioxide", "density", "pH", "sulphates", "alcohol"], "data": [[7.4, 0.7, 0, 1.9, 0.076, 11, 34, 0.9978, 3.51, 0.56, 9.4]]}' \
  http://127.0.0.1:1234/invocations
```

## Construction d'une image Docker (optionnel)

Si tu souhaites déployer dans un conteneur Docker :

```bash
mlflow models build-docker -m "runs:/<run-id>/model" -n wine-quality-model
docker run -p 1234:1234 wine-quality-model
```

## Notes

- Le dossier `mlruns/` contient les sorties MLflow locales et peut être ignoré si tu préfères utiliser un serveur de tracking distant.
- Si tu veux utiliser un serveur MLflow distant, configure `MLFLOW_TRACKING_URI` avant d'exécuter le notebook.
