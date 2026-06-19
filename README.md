# 🧪 Experimento A/B en Página de Inicio — Landing Experiment

Proyecto de análisis estadístico en Python que evalúa un **experimento A/B** entre dos versiones de una landing page (A y B), con el objetivo de apoyar una **decisión de negocio basada en datos**.

## 🎓 Contexto académico

Este proyecto fue desarrollado como parte del programa de **TripleTen**, dentro del módulo de **pruebas A/B (A/B Testing)**, con el objetivo de practicar:
- Validar hipótesis de negocio mediante pruebas estadísticas.
- Formular preguntas de negocio como hipótesis nula y alternativa.
- Comparar promedios y proporciones entre grupos usando Python.
- Comprobar relaciones entre variables categóricas usando Python.

El caso del experimento de landing page es un escenario simulado que sirve como contexto para aplicar estas técnicas a una decisión real de optimización de conversión.

---

## 🧾 Descripción general

El dataset `landing_experiment.csv` contiene información de usuarios expuestos a dos versiones de una página de inicio dentro de un experimento A/B, incluyendo región, dispositivo, fuente de tráfico, tipo de usuario, conversión y gasto.

El análisis sigue una lógica progresiva:

1. 🔍 Explorar y validar los datos.
2. 💰 Comparar el **gasto promedio** por usuario entre la página A y B.
3. 🎯 Comparar la **tasa de conversión** entre la página A y B.
4. 🌐 Revisar la relación entre la **fuente de tráfico** y la conversión.
5. 👤 Revisar la relación entre el **tipo de usuario** y la conversión.
6. 📈 Visualizar los resultados con gráficos que respaldan las conclusiones.

Se aplican pruebas estadísticas apropiadas para cada tipo de comparación, y se recomienda qué versión de página es mejor, justificando la decisión con datos.

---

💡 **Preguntas del negocio**
- ¿Existe una diferencia significativa en el gasto promedio por usuario convertido entre ambas versiones?
- ¿Qué versión de la página (A o B) genera mayor tasa de conversión?
- ¿La conversión depende de la fuente de tráfico?
- ¿El tipo de usuario (nuevo o recurrente) influye en la conversión?
- ¿Qué hallazgos o insights permiten optimizar la estrategia de marketing y el diseño de la página de inicio (landing page)?

---

## 🗂️ Dataset

`landing_experiment.csv` — **40,000 registros**, **9 columnas**, sin valores nulos. Periodo del experimento: del **2026-01-01** al **2026-01-28**.

**Preparación de datos:**
- `date` se convirtió de `object` a `datetime64`.
- No se encontraron valores nulos ni atípicos relevantes en las variables categóricas.

**Distribución de variables categóricas:**
- `dispositivo`: 62.1% Mobile, 37.9% Desktop.
- `traffic_source`: 45.0% Organic, 29.8% Ads, 15.3% Email, 9.9% Referral.
- `user_type`: 65.1% Nuevo, 34.9% Recurrente.
- `region`: Norte (27.9%), Centro (24.0%), Sur (20.1%), Occidente (16.0%), Oriente (12.0%).

---

## 🔬 Pruebas estadísticas aplicadas

### 1️⃣ Prueba t de Student — gasto promedio (A vs. B)
**H₀:** el gasto promedio es igual entre el grupo A y el grupo B.
**H₁:** el gasto promedio **no** es igual entre ambos grupos.

| Grupo | n (usuarios convertidos) | Gasto promedio |
|---|---|---|
| A | 2,512 | $61.09 USD |
| B | 3,194 | $68.75 USD |

**Resultado:** t = -9.37, p ≈ 1.06e-20 → se **rechaza H₀**. La diferencia en gasto promedio es estadísticamente significativa.

### 2️⃣ Prueba z de proporciones — tasa de conversión (A vs. B)
**H₀:** la tasa de conversión es igual entre el grupo A y el grupo B.
**H₁:** la tasa de conversión **no** es igual entre ambos grupos.

| Grupo | Conversiones | Total usuarios | Tasa de conversión |
|---|---|---|---|
| A | 2,512 | 19,982 | 12.57% |
| B | 3,194 | 20,018 | 15.96% |

**Resultado:** z = -9.68, p ≈ 3.76e-22 → se **rechaza H₀**. La página B convierte 3.38 puntos porcentuales más que la página A (~676 usuarios adicionales sobre el tamaño de muestra de B).

### 3️⃣ Prueba chi-cuadrado — fuente de tráfico vs. conversión
**H₀:** todas las fuentes de tráfico generan la misma tasa de conversión.
**H₁:** **no** todas las fuentes de tráfico generan la misma tasa de conversión.

| Fuente de tráfico | Tasa de conversión |
|---|---|
| Email | 14.99% |
| Ads | 14.74% |
| Referral | 13.88% |
| Organic | 13.79% |

**Resultado:** χ² = 8.662, p ≈ 0.034 → se **rechaza H₀**. Existe asociación significativa entre la fuente de tráfico y la conversión; Email es el canal más efectivo en tasa, aunque Organic lidera en volumen absoluto de conversiones (2,480).

### 4️⃣ Prueba chi-cuadrado — tipo de usuario vs. conversión
**H₀:** no existe relación entre el tipo de usuario y la tasa de conversión.
**H₁:** existe una relación entre el tipo de usuario y la tasa de conversión.

| Tipo de usuario | Tasa de conversión |
|---|---|
| Nuevo | 14.36% |
| Recurrente | 14.09% |

**Resultado:** χ² = 0.513, p ≈ 0.474 → **no se rechaza H₀**. No hay evidencia de asociación entre el tipo de usuario y la conversión.

---

## 📊 Visualizaciones

- **Gráfico de barras (conteo)** — conversiones absolutas por fuente de tráfico y por tipo de usuario.
- **Gráfico de barras apiladas (proporción)** — porcentaje de conversión por categoría, usando `histplot` con `multiple='fill'`, para complementar el volumen absoluto con la efectividad relativa de cada canal.

Estas visualizaciones refuerzan visualmente los resultados de las pruebas chi-cuadrado: Email destaca en tasa de conversión a pesar de su menor volumen, y la diferencia entre usuarios nuevos y recurrentes es visualmente casi imperceptible, consistente con el resultado no significativo de la prueba.

---

## 🌟 Insight ejecutivo para stakeholders

**Comparación de página (A vs. B)**
- El gasto promedio es mayor en la página B ($68.74 USD) frente a la página A ($61.08 USD), diferencia estadísticamente significativa (t-test, p ≈ 1.06e-20).
- La tasa de conversión de la página B es 3.38 puntos porcentuales superior a la de A (z-test, p ≈ 3.76e-22).

**Segmentación por fuente de tráfico**
- Email es el canal con mejor tasa de conversión (~15%), diferencia estadísticamente significativa entre fuentes (chi², p ≈ 0.034).

**Segmentación por tipo de usuario**
- La tasa de conversión entre usuarios nuevos y recurrentes es prácticamente igual (diferencia <1%), sin relación estadísticamente significativa (chi², p ≈ 0.47).

**Recomendaciones de negocio:**
- Implementar la **página B** como versión definitiva: supera a A tanto en tasa de conversión (15.96% vs. 12.57%) como en gasto promedio ($68.74 vs. $61.08 USD), con resultados estadísticamente robustos.
- Enfocar esfuerzos de marketing en el canal de **Email**, dado que presenta la mejor tasa de conversión y podría traducirse en un mayor gasto promedio agregado.
- No se recomienda diseñar campañas diferenciadas por tipo de usuario (nuevo vs. recurrente) para mejorar conversión, ya que no existe evidencia estadística de que este factor influya en el resultado.

---

## 🛠️ Herramientas y librerías utilizadas

- **Python**
- `pandas`, `numpy` — manipulación y limpieza de datos
- `matplotlib`, `seaborn` — visualización (countplot, histplot con proporciones)
- `scipy.stats` — `ttest_ind`, `chi2_contingency`
- `statsmodels.stats.proportion` — `proportions_ztest`

---

## 👤 Autor

Jesús — Data Analyst Junior
