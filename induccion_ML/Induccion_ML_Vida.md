---
title: "Inducción: Machine Learning Aplicado a Pricing de Vida Individual"
subtitle: "Métodos Actuariales Estándar vs. Modelos Internos — Proyecto de Investigación"
author:
  - name: "Facultad de Ciencias"
date: "2026"
lang: es-MX

# Motor y fuentes
mainfont: "DejaVu Serif"
sansfont: "DejaVu Sans"
monofont: "DejaVu Sans Mono"
fontsize: 11pt

# Geometría de página
geometry:
  - top=2.5cm
  - bottom=2.5cm
  - left=3cm
  - right=2.5cm
  - a4paper

# Interlineado y párrafos
linestretch: 1.3
indent: false
parskip: 6pt

# Encabezado y pie de página
header-includes:
  - \usepackage{fancyhdr}
  - \pagestyle{fancy}
  - \fancyhf{}
  - \fancyhead[L]{\small Pricing de Vida Individual con ML}
  - \fancyhead[R]{\small Facultad de Ciencias}
  - \fancyfoot[C]{\thepage}
  - \renewcommand{\headrulewidth}{0.4pt}
  # Colores para bloques de código
  - \usepackage{xcolor}
  - \definecolor{codebg}{RGB}{245,245,245}
  - \definecolor{codeframe}{RGB}{200,200,200}
  # Estilo de bloques de código
  - \usepackage{fancyvrb}
  - \usepackage{framed}
  # Tablas mejoradas
  - \usepackage{booktabs}
  - \usepackage{longtable}
  - \usepackage{array}
  # Hipervínculos
  - \usepackage{hyperref}
  - \hypersetup{colorlinks=true, linkcolor=blue!60!black, urlcolor=blue!60!black, citecolor=green!50!black}

# Opciones de código
highlight-style: tango

# Tabla de contenidos
toc: true
toc-depth: 3
toc-title: "Índice"
number-sections: true

# PDF metadata
pdf-engine: xelatex
---

# Inducción: Machine Learning Aplicado a Pricing de Vida Individual
## Proyecto de Investigación — Facultad de Ciencias

> **Línea de investigación:** Métodos Actuariales Estándar vs. Modelos Internos de Aprendizaje Automático para la Tarificación de Seguros de Vida Individual en México bajo el marco regulatorio CUSF-CNSF.

---

## 1. Contexto del Problema

### 1.1 ¿Qué es el Pricing Actuarial?

El **pricing** (tarificación) es el proceso mediante el cual una aseguradora determina la prima que debe cobrar a un asegurado para cubrir:

```
Prima Técnica = Prima Pura + Gastos + Margen de Utilidad

Prima Pura = Frecuencia × Severidad

donde:
  Frecuencia = E[N] / Expuestos   (siniestros esperados por asegurado)
  Severidad  = E[monto | N > 0]   (monto esperado dado que ocurre)
```

En seguros de Vida Individual, la prima pura depende fundamentalmente de la **probabilidad de muerte o supervivencia** del asegurado durante la vigencia de la póliza, ajustada por variables de riesgo observables.

### 1.2 El Paradigma Estándar: GLM Actuarial

Desde los años 90, el estándar de la industria para pricing es el **Modelo Lineal Generalizado (GLM)**, que extiende la regresión clásica para admitir distribuciones no gaussianas:

```
g(E[Y]) = β₀ + β₁x₁ + β₂x₂ + ... + βₙxₙ

Donde:
  Y    = variable respuesta (frecuencia, severidad, loss ratio)
  g(·) = función de enlace (log, logit, inversa)
  xᵢ   = variables de tarificación (edad, sexo, plan, etc.)
  βᵢ   = parámetros estimados por máxima verosimilitud
```

Las distribuciones más usadas en seguros de vida son:

| Componente | Distribución | Función de enlace |
|---|---|---|
| Frecuencia (nº siniestros) | Poisson | log |
| Severidad (monto) | Gamma | log |
| Prima pura combinada | Tweedie | log |
| Probabilidad de muerte | Binomial | logit |

### 1.3 El Problema Central de Investigación

Los GLM, aunque robustos y regulatoriamente aceptados, imponen **supuestos de linealidad y separabilidad** que pueden no capturar interacciones complejas entre variables. Los modelos de ML relaxan estos supuestos, pero introducen opacidad que el marco regulatorio mexicano exige justificar.

**Pregunta de investigación:**

> ¿Pueden los modelos de aprendizaje automático replicar o superar el desempeño predictivo de los métodos actuariales estándar para la tarificación de Vida Individual en México, cumpliendo simultáneamente con los requisitos técnicos establecidos en la CUSF?

---

## 2. Marco Regulatorio Mexicano

### 2.1 Jerarquía normativa aplicable

```
LISF (Ley de Instituciones de Seguros y Fianzas)
  └── Art. 216: Prima suficiente para cubrir obligaciones
  └── Art. 217-280: Reservas técnicas
        └── CUSF (Circular Única de Seguros y Fianzas)
              └── Disposición 13: Bases técnicas
              └── Disposición 14: Registro de productos
              └── Disposición 22: Modelos internos (Solvencia II)
                    └── Nota Técnica Actuarial
```

### 2.2 La Nota Técnica como vehículo regulatorio

La **Nota Técnica Actuarial** es el documento que toda aseguradora debe presentar ante la CNSF para registrar un producto. Contiene obligatoriamente:

1. Base de cálculo de la prima
2. Hipótesis de mortalidad y morbilidad
3. Hipótesis de gastos
4. Tasa de interés técnico
5. **Método actuarial utilizado** ← punto de entrada para ML

La regulación **no prohíbe** modelos de ML, pero exige que el método sea:

- Técnicamente justificable y documentado
- Reproducible y auditable
- Consistente con la experiencia propia o de mercado
- Validado y firmado por Actuario Responsable certificado (CNSF)

### 2.3 El espacio regulatorio para modelos innovadores

La **Disposición 22 de la CUSF**, derivada de Solvencia II europeo, establece el precedente de que la CNSF acepta modelos propios siempre que cumplan estándares de validación documentados. Aunque fue diseñada para modelos de capital, sienta las bases para el argumento central de este proyecto.

---

## 3. Datos: Fuente y Estructura

### 3.1 Fuente de datos

**Portal de Datos Abiertos — CNSF / gob.mx**

- Ramo: Vida Individual
- Periodicidad: Anual
- Cobertura: Nacional (32 entidades federativas)
- Granularidad: Segmento actuarial agregado

La ausencia de registros individuales es inherente a toda fuente pública regulatoria y es **consistente con la práctica actuarial estándar**, que siempre opera sobre experiencia colectiva de segmentos.

### 3.2 Estructura de los datos

Cada archivo contiene tres hojas de cálculo:

**Hoja 1 — Emisión** (Exposición y prima)

| Campo | Tipo | Descripción |
|---|---|---|
| `edad` | int | Edad del asegurado |
| `cobertura` | cat | Tipo de cobertura |
| `plan_poliza` | cat | Dotal Mixto, Temporal, Vitalicio, etc. |
| `modalidad_poliza` | cat | Tradicional, Flexible, Microseguro, etc. |
| `moneda` | cat | Nacional, Indizada, Extranjera |
| `entidad` | cat | Entidad federativa |
| `sexo` | cat | Masculino / Femenino |
| `forma_de_venta` | cat | Canal de distribución |
| `numero_asegurados` | int | **Exposición del segmento** |
| `prima_emitida` | float | **Prima cobrada — variable clave** |
| `suma_asegurada` | float | Riesgo máximo asumido |

**Hoja 2 — Siniestro** (Experiencia de pérdida)

| Campo | Tipo | Descripción |
|---|---|---|
| `monto_pagado` | float | Pérdida realizada |
| `monto_reclamando` | float | Pérdida incurrida |
| `numero_siniestro` | int | Conteo de siniestros |
| `causa_siniestro` | cat | **Feature discriminante para ML** |
| `monto_asegurado` | float | Suma asegurada en siniestro |
| `vencimientos` | float | Pagos por vencimiento de póliza |

**Hoja 3 — Comisiones** (Costos de adquisición y financieros)

| Campo | Tipo | Descripción |
|---|---|---|
| `comisiones_directas` | float | Costo del canal de venta |
| `prima_cedida` | float | Reaseguro |
| `fondo_inversion` | int | Componente financiero |
| `fondo_administracion` | int | Gastos de administración |
| `monto_rescate` | int | Comportamiento del asegurado |
| `monto_dividendos` | float | Dividendos pagados |
| `tipo_dividendo` | cat | Tipo de experiencia de dividendo |

### 3.3 Construcción del dataset de modelado

Las tres hojas se cruzan mediante las variables compartidas:

```python
keys = ["edad", "plan_poliza", "modalidad_poliza",
        "entidad", "sexo", "moneda"]

df = emision.merge(siniestro, on=keys, how="left") \
            .merge(comisiones, on=keys, how="left")
```

Las variables objetivo se derivan como:

```python
# Target principal
df["loss_ratio"] = df["monto_pagado"] / df["prima_emitida"]

# Descomposición frecuencia-severidad
df["frecuencia"] = df["numero_siniestro"] / df["numero_asegurados"]
df["severidad"]  = df["monto_pagado"] / df["numero_siniestro"]

# Prima pura por asegurado
df["prima_pura"] = df["frecuencia"] * df["severidad"]
```

---

## 4. Fundamentos de Machine Learning Relevantes

### 4.1 ¿Por qué ML en pricing?

Los GLM asumen que el efecto de cada variable sobre el target es **aditivo y separable**. En la práctica, existen interacciones relevantes que el GLM no captura sin especificación manual:

```
Ejemplo: La causa_siniestro "accidente" puede tener
         un impacto diferente según la modalidad_poliza
         sea "Microseguro" vs "Tradicional".

GLM:  necesita término de interacción explícito
ML:   lo aprende automáticamente de los datos
```

### 4.2 Modelos del lado estándar (baseline)

**GLM Poisson — Frecuencia**
```
log(E[frecuencia]) = β₀ + β₁·edad + β₂·sexo + β₃·plan + ...
offset = log(numero_asegurados)
```

**GLM Gamma — Severidad**
```
log(E[severidad]) = γ₀ + γ₁·edad + γ₂·cobertura + ...
```

**Tabla actuarial clásica**

Clasificación manual de riesgo por celdas de edad × sexo × plan, estimando loss ratio promedio por celda. Es el método más transparente y el punto de referencia histórico del sector.

### 4.3 Modelos del lado ML (caja negra)

**Gradient Boosting (XGBoost / LightGBM)**

Ensamble de árboles de decisión entrenados secuencialmente. Cada árbol corrige los errores del anterior:

```
F(x) = Σ fₜ(x)    donde fₜ minimiza el residual de Fₜ₋₁
```

Es el modelo más usado en competencias de seguros por su capacidad de capturar interacciones no lineales con variables mixtas (numéricas + categóricas).

**Random Forest**

Ensamble de árboles entrenados en paralelo sobre muestras aleatorias del dataset. Más estable que un solo árbol, menos propenso a sobreajuste que XGBoost sin regularización.

**Red Neuronal (MLP)**

Arquitectura de capas densas con funciones de activación no lineales. Requiere mayor volumen de datos para generalizar bien. Su opacidad es mayor, lo que dificulta la justificación regulatoria.

### 4.4 El problema de la interpretabilidad (Caja Negra)

El reto regulatorio central del proyecto. Un GLM produce coeficientes interpretables directamente:

```
β_edad = 0.03 → "cada año adicional incrementa
                  el log del loss ratio en 0.03"
```

Un modelo de ML no produce esto. La solución moderna son los **valores SHAP** (SHapley Additive exPlanations):

```
SHAP(xᵢ) = contribución marginal promedio de la variable xᵢ
            a la predicción, calculada sobre todas las
            posibles coaliciones de variables
```

Los valores SHAP permiten comparar la importancia de variables entre GLM y ML en una escala común, y son el argumento técnico para justificar un modelo de caja negra ante el regulador.

---

## 5. Métricas de Evaluación

### 5.1 Métricas predictivas

| Métrica | Fórmula | Uso |
|---|---|---|
| RMSE | √(Σ(ŷ-y)²/n) | Error promedio en unidades del target |
| MAE | Σ\|ŷ-y\|/n | Robusto a outliers |
| MAPE | Σ\|ŷ-y\|/y / n | Error relativo (%) |
| Gini | 2·AUC - 1 | Poder discriminante |

### 5.2 Métricas actuariales

```python
# Suficiencia de prima (criterio LISF Art. 216)
suficiencia = prima_predicha.sum() / monto_pagado.sum()
# Objetivo: suficiencia >= 1.0

# Estabilidad por segmento
cv_loss_ratio = loss_ratio.std() / loss_ratio.mean()
# Objetivo: menor en el modelo que en la tabla clásica
```

### 5.3 Curva de Lift

Ordena los segmentos de mayor a menor riesgo predicho y mide cuánto del siniestro real concentra el modelo en los primeros deciles. Un buen modelo de pricing discrimina: los segmentos que predice como alto riesgo efectivamente tienen mayor siniestralidad.

---

## 6. Pipeline del Proyecto

```
┌─────────────────────────────────────────────────────────────┐
│  FASE 1 — Marco Teórico y Regulatorio                       │
│  LISF + CUSF + literatura de ML en seguros                  │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│  FASE 2 — Adquisición y EDA                                 │
│  Descarga CNSF → Limpieza → Cruce de sheets                 │
│  Análisis de loss ratio por segmento                        │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│  FASE 3 — Feature Engineering                               │
│  Encoding de categóricas → Derivación de targets            │
│  Manejo de missing values → Panel de datos (multi-año)      │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│  FASE 4 — Modelado                                          │
│  Estándar: Tabla actuarial + GLM Tweedie                    │
│  ML: XGBoost + Random Forest + (MLP opcional)               │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│  FASE 5 — Comparativa y Análisis Regulatorio                │
│  RMSE / MAE / Gini / Lift / SHAP                            │
│  ¿Cumple el modelo ML los requisitos de la Nota Técnica?    │
└────────────────────┬────────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────────┐
│  FASE 6 — Artículo de Investigación                         │
│  Resultados + Limitaciones + Conclusiones + Recomendaciones │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. Stack Tecnológico

```python
# Manipulación de datos
import pandas as pd
import numpy as np

# Modelos estándar
import statsmodels.api as sm          # GLM con control estadístico
from sklearn.linear_model import TweedieRegressor

# Modelos ML
from xgboost import XGBRegressor
from lightgbm import LGBMRegressor
from sklearn.ensemble import RandomForestRegressor

# Interpretabilidad
import shap

# Evaluación
from sklearn.metrics import mean_squared_error, mean_absolute_error
from sklearn.model_selection import KFold, cross_val_score

# Visualización
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## 8. Contribución Esperada del Proyecto

Este trabajo busca llenar el hueco existente entre tres cuerpos de literatura que no han sido integrados en el contexto mexicano:

1. **Literatura actuarial** de tarificación de vida (GLM, tablas de mortalidad)
2. **Literatura de ML** aplicado a seguros (dominada por mercados europeos)
3. **Análisis regulatorio** del marco CUSF-CNSF para modelos innovadores

La contribución específica es demostrar, con datos públicos de la CNSF, si los modelos de ML pueden ser **regulatoriamente equivalentes** a los métodos estándar, es decir, si cumplen los criterios de auditabilidad, reproducibilidad y suficiencia prima que exige la legislación mexicana.

---

*Documento de inducción — Proyecto de Investigación*
*Facultad de Ciencias — Pricing de Vida Individual con ML*
*Datos: CNSF / Datos Abiertos gob.mx*
