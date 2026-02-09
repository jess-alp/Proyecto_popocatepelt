# Proyecto_popocatepelt

## UAAA_Proyecto - Cambios realizados por Jessica y Karina

A continuación se describen todos los cambios realizados al proyecto AAA original de Malfante realizados por Karina y por Jessica.


---

## PARTE 1: Cambios realizados por Karina
Karina realizo las mejoras principales al modelo de clasificacion:

---

### 1. Analisis de Componentes Principales (PCA)

Archivo: `analyzer.py` (lineas 83-85, 196-209, 445-451)

Se implemento PCA para reducir la dimensionalidad del espacio de features de 102 a 90 componentes principales.

---

### 2. Separacion de Conjuntos de Entrenamiento y Prueba

Archivo: `analyzer.py` (lineas 220-255, 401-494)

Se implemento una separacion rigurosa entre datos de entrenamiento y prueba usando catalogos independientes.

---

### 3. Metricas de Rendimiento por Clase

Archivo: `analyzer.py` (lineas 287-374, 492-493)

Se incluyeron metricas detalladas por clase durante validacion cruzada y fase de prueba.

**Clases monitoreadas:**
- **LP** 
- **Tremor** 
- **noise** 

**Metricas reportadas por clase:**

| Metrica | Descripcion |
|---------|-------------|
| Precision | Proporcion de predicciones correctas por clase |
| Recall | Proporcion de eventos detectados por clase |
| F1-score | Media armonica de precision y recall |
| Support | Numero de muestras por clase |

---

### 4. Tamano de Ventana Dinamico

Archivo: `recording.py` (lineas 47-52, 86-101)

Se introdujo un calculo dinamico del numero de ventanas deslizantes basado en la longitud real de los datos, 40 segundos de cada ventana, si hay ventanas consecutivas con un mismo evento se unifican.

---

### 5. Busqueda de Hiperparametros (Grid Search)

Archivo: `config.py` (lineas 200-216)

Se preparo la infraestructura para optimizacion de hiperparametros via GridSearchCV.


---

### 12. Modificacion de `n_points` en `featuresFunctions.py`

Archivo:`featuresFunctions.py` (linea 338)

Karina modifico la funcion `n_points()` para aplicar normalización a la longitud de la señal.

---

### PARTE 2: Cambios de Jessica 

Se realizaron cambios enfocados en la compatibilidad del codigo con versiones actuales de las dependencias.

---

### 1. Reemplazo de `scipy.stats.threshold` (Funcion Deprecada)

Archivo: `featuresFunctions.py` (lineas 48-85)

Problema: `scipy.stats.threshold()` fue eliminada en versiones scipy mayores a 1.0

Solucion: Funcion que replica el comportamiento original:
```python
def threshold(a, threshmin=None, threshmax=None, newval=0):
    a = np.asarray(a).copy()
    if threshmin is not None:
        a[a < threshmin] = newval
    if threshmax is not None:
        a[a > threshmax] = newval
    return a
```

Funciones que dependen de este cambio:
- silence_ratio()
---

### 2. Modernizacion del Cliente ObsPy (ArcLink a FDSN)

Archivo: `DataReadingFunctions.py` (lineas 46-53, 191-211)

Problema: `obspy.clients.arclink.client` fue eliminado en las versiones modernas de obspy

Solucion: Migracion al cliente FDSN con compatibilidad:
```python
try:
    from obspy.clients.fdsn import Client
except ImportError:
    from obspy.clients.arclink.client import Client
```

---

### 3. Dependencias Agregadas en `environment.yml`

Se agregaron las dependencias necesarias para ejecutar AAA/UAAA en el entorno conda proporcionado por S. Valade.

---

## Entornos de Ejecucion

### UAAA (updatedAAA/UAAA.yml) - Entorno original K-B-M
```bash
conda env create -f updatedAAA/UAAA.yml
conda activate UAAA
```
- Python 3.6.13 | numpy 1.13.3 | scikit-learn 0.22.2 | obspy 1.1.0

### Entorno General del Proyecto (environment.yml) - Actualizado por J-A
```bash
conda env create -f environment.yml
conda activate waves
```
- Python 3.10 | obspy (moderno) | scikit-learn (moderno) | pandas 1.5.3
