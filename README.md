# Simpli

## Descripción de los códigos: ambos siguen la misma lógica, solo presentan datos de distintas empresas

- codigo_limpio copy 2.ipynb -> empresa F
- codigo_limpio copy 82651 -> empresa CT

# Predicción de Tiempos de Servicio mediante Datos GPS y Checkouts

## Descripción

Este proyecto tiene como objetivo construir una estimación de los tiempos de servicio de una parada logística utilizando información operacional proveniente de registros GPS y eventos de checkout.

La metodología combina:

- Limpieza y procesamiento de datos GPS.
- Agrupación de checkouts en paradas realizadas.
- Detección de paradas mediante trazas GPS.
- Emparejamiento entre paradas realizadas y detectadas.
- Construcción de variables explicativas.
- Entrenamiento y evaluación de modelos predictivos.
- Interpretación de resultados mediante SHAP.

---

## Objetivo

Estimar la duración de servicio de una parada utilizando variables operacionales observables.

Variable objetivo:

```python
duracion_estimada_min
```

---

## Datos Utilizados

### Datos GPS

Información de ubicación de los conductores.

Variables típicas:

- owner_id
- created
- latitude
- longitude
- battery_level

### Datos de Checkouts

Eventos registrados durante la operación.

Variables típicas:

- user_id
- event_date
- latitude
- longitude

---

## Metodología

### 1. Preparación de Datos

Se realizan las siguientes tareas:

- Conversión de fechas a formato datetime.
- Homologación de zonas horarias.
- Limpieza de registros inválidos.
- Ordenamiento temporal.

---

### 2. Agrupación de Checkouts

Los checkouts cercanos en tiempo y espacio son agrupados para representar una única parada realizada.

Resultado:

```python
parada_agrupada_realizada_id
```

---

### 3. Detección de Paradas GPS

A partir de los datos GPS se identifican detenciones del vehículo.

Resultado:

```python
parada_agrupada_detectada_id
```

---

### 4. Matching de Paradas

Se emparejan:

- Paradas detectadas por GPS.
- Paradas realizadas mediante checkouts.

Utilizando criterios espaciales y temporales.

---

### 5. Construcción de Variables

Se generan variables asociadas a:

#### Temporales

- dia
- bloque_horario

#### Geográficas

- Comuna

#### Operacionales

- cantidad_paquetes
- cantidad_paradas_owner_dia

---

### 6. Limpieza de Outliers

Se eliminan observaciones atípicas utilizando criterios operacionales y estadísticos.

---

## Variables del Modelo

### Variable Objetivo

```python
duracion_estimada_min
```

### Variables Predictoras

#### Categóricas

- empresa
- Comuna
- dia
- bloque_horario

#### Numéricas

- cantidad_paquetes
- cantidad_paradas_owner_dia

---

## Modelos Evaluados

### Regresión Lineal

Modelo base de referencia.

### Regresión Lineal con Interacciones

Incorpora relaciones entre variables categóricas y numéricas.

### LASSO

Utilizado para selección automática de variables.

### OLS Post-LASSO

Reentrenamiento utilizando variables seleccionadas por LASSO.

### XGBoost

Modelo basado en Gradient Boosting.

### CatBoost

Modelo especializado en variables categóricas.

---

## Optimización de Hiperparámetros

Se utiliza Optuna para optimizar:

- XGBoost
- CatBoost

---

## División de Datos

| Conjunto | Porcentaje |
|-----------|------------|
| Train | 64% |
| Validation | 16% |
| Test | 20% |

---

## Métricas de Evaluación

Se calculan:

- MAE
- RMSE
- R²
- MAPE

Para:

- Train
- Validation
- Test

---

## Interpretabilidad

### Importancia de Variables

Se calcula para todos los modelos.

### SHAP

Se utiliza para:

- Importancia global.
- Análisis por comuna.
- Análisis por día.
- Análisis por bloque horario.

---

## Librerías Utilizadas

```python
pandas
numpy
scikit-learn
statsmodels
xgboost
catboost
optuna
shap
geopandas
shapely
matplotlib
```

---

## Resultados Esperados

El proyecto permite:

- Estimar tiempos de servicio.
- Identificar factores que explican la duración de una parada.
- Comparar distintos modelos predictivos.
- Generar explicaciones interpretables para la operación logística.
  
