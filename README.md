# Predicción de Activo/Inactivo — App en Streamlit (sin `requirements.txt`)

Este repositorio contiene una aplicación **Streamlit** para predecir si un registro estará **Activo** o **Inactivo**, usando un modelo previamente entrenado y serializado en `modelo_decision_tree_optimizado.pkl`.

> **Importante:** ejecuta siempre la app **dentro de un entorno virtual** (Conda o `venv`). Esto evita conflictos de dependencias y asegura un entorno limpio y reproducible.

---

## 📁 Estructura mínima del proyecto

```
.
├── app.py
├── modelo_decision_tree_optimizado.pkl   # ← requerido
└── (opcional) data/
    └── ... CSVs para pruebas por lote
```

- `app.py`: código de la aplicación Streamlit.
- `modelo_decision_tree_optimizado.pkl`: archivo pickle con el **modelo**, el **labelencoder** (si aplica) y la lista de **variables** (`variables`) usadas en el entrenamiento. **Debe estar en la misma carpeta que `app.py`.**
- `data/` (opcional): CSVs de prueba para predicción por lote.

---

## ✅ Requisitos

- **Python** 3.10+ (recomendado 3.10/3.11)
- **Conda** o **venv** (una de las dos)
- Conexión a Internet para instalar dependencias la primera vez

---

## 🧪 Crear y activar entorno virtual (elige una opción)

### Opción A) Conda

```bash
# 1) Crear entorno (Python 3.10 recomendado)
conda create -n pred-activos python=3.10 -y

# 2) Activarlo
conda activate pred-activos
```

### Opción B) venv (nativo de Python)

```bash
# 1) Crear entorno
python -m venv .venv

# 2) Activarlo
#   macOS / Linux:
source .venv/bin/activate
#   Windows (PowerShell):
.\.venv\Scripts\Activate.ps1
#   Windows (CMD):
.\.venv\Scriptsctivate.bat
```

> Para salir del entorno, usa `conda deactivate` o `deactivate`.

---

## 📦 Instalación **manual** de dependencias (sin `requirements.txt`)

Ejecuta estos comandos **dentro** del entorno virtual activado:

```bash
# Núcleo de la app
pip install streamlit

# Manejo de datos y modelo
pip install pandas numpy scikit-learn

# Visualizaciones (si la app grafica)
pip install matplotlib
```

> Si tu `app.py` usa librerías adicionales (por ejemplo `pyarrow` para Parquet, `joblib`, etc.), instálalas también:
>
> ```bash
> pip install pyarrow joblib
> ```

---

## ▶️ Ejecutar la aplicación

Desde la carpeta del proyecto (donde está `app.py`):

```bash
streamlit run app.py
```

- Por defecto abrirá en `http://localhost:8501/`.
- Si el puerto está ocupado, usa otro:
  ```bash
  streamlit run app.py --server.port 8502
  ```

---

## 🧠 Consideraciones clave del modelo y preprocesamiento

1. **Carga del modelo** desde `modelo_decision_tree_optimizado.pkl`:
   - Debe contener (según el entrenamiento original) objetos como: `modelTree`, `labelencoder` (si aplica) y la lista `variables`.
2. **Preprocesamiento idéntico al entrenamiento**:
   - Si usaste `pd.get_dummies` u otras transformaciones, **replícalas exactamente** en la app, con los mismos parámetros.
3. **Alineación de columnas antes de predecir**:
   - Asegúrate de alinear el `DataFrame` final con `variables`:
     ```python
     data_preparada = data_preparada.reindex(columns=variables, fill_value=0)
     ```

---

## 🧰 Solución de problemas (FAQ)

### 1) `FileNotFoundError: modelo_decision_tree_optimizado.pkl`
Coloca el `.pkl` en la **misma carpeta** de `app.py` (o ajusta la ruta en el código):
```python
filename = 'modelo_decision_tree_optimizado.pkl'
```

### 2) `ModuleNotFoundError: No module named '...'`
Activa el entorno virtual y vuelve a instalar dependencias manualmente:
```bash
# Activa tu entorno (ejemplos)
conda activate pred-activos
# o
source .venv/bin/activate

# Instala lo que falte
pip install streamlit pandas numpy scikit-learn matplotlib
```

### 3) La página no abre o el puerto está ocupado
Prueba con otro puerto:
```bash
streamlit run app.py --server.port 8502
```

### 4) Error por columnas desalineadas en predicción
Verifica que:
- El preprocesamiento en `app.py` sea **idéntico** al del entrenamiento (mismos `get_dummies`, orden de pasos, etc.).
- Ejecutes la alineación con `variables` antes de `predict`.
- El `.pkl` corresponda al modelo entrenado con ese **mismo** conjunto de variables.

---

## 🧪 Desarrollo local y buenas prácticas

- Edita `app.py` para ajustar textos, opciones y layout.
- Si agregas una librería nueva, instálala manualmente con `pip install <paquete>` **dentro** del entorno virtual.
- Si necesitas conocer versiones instaladas:
  ```bash
  pip show streamlit pandas numpy scikit-learn matplotlib
  ```

---

## 📜 Licencia

Incluye aquí la licencia de tu preferencia (MIT, Apache-2.0, etc.).

---

## 👤 Contacto

Cualquier duda o mejora, abre un issue o contáctame.
