# TP1 - Inteligencia Artificial 2026 | UTN FRSF

## Descripción

Trabajo Práctico N° 1 de la cátedra **Inteligencia Artificial** (UTN – FRSF, 2026).

El objetivo es diseñar, entrenar y evaluar un modelo de **red neuronal multicapa (MLP)** para predecir la necesidad de **mantenimiento predictivo** de máquinas industriales, a partir de un dataset preprocesado en la materia Ciencia de Datos. Se exploran distintas configuraciones de hiperparámetros (tasa de aprendizaje, capas ocultas, funciones de activación, regularización, etc.) mediante **Keras Tuner**, y se evalúa el desempeño del modelo con métricas de clasificación (accuracy, precision, recall, F1-score) y curvas de aprendizaje.

## Configuración del entorno

### 1. Crear el entorno virtual

| | Windows | Linux / macOS |
|---|---|---|
| Crear | `python -m venv venv` | `python3 -m venv venv` |
| Activar | `venv\Scripts\activate` | `source venv/bin/activate` |
| Desactivar | `deactivate` | `deactivate` |

### 2. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 3. Ejecutar el notebook

```bash
jupyter notebook TP-IA.ipynb
```

## Tecnologías utilizadas

- Python 3.12
- TensorFlow / Keras
- Keras Tuner
- Scikit-learn
- Pandas, NumPy, Matplotlib, Seaborn
- OpenCV
