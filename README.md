# bank-marketing-predictive-analysis
# Optimización de Campañas de Telemarketing: Modelado Predictivo para la Contratación de Depósitos Bancarios

Este proyecto aplica técnicas de Ciencia de Datos y Machine Learning para resolver un problema clásico de negocio en el sector financiero: optimizar las campañas de captación de depósitos a plazo fijo de un banco utilizando el dataset *Bank Marketing* del repositorio de la UCI.

## 🎯 Objetivo del Proyecto

El objetivo principal es construir un modelo predictivo capaz de identificar a los clientes con mayor probabilidad de contratar un depósito. Al segmentar adecuadamente la base de datos, el banco puede enfocar sus esfuerzos de telemarketing en perfiles de alta conversión, reduciendo costes operativos (tiempo y llamadas en frío) y maximizando el retorno de la inversión (ROI).

---

## 📊 Hallazgos Clave del Análisis Exploratorio (EDA)

* **Desbalanceo Crítico:** El dataset presenta un fuerte desbalanceo original, donde aproximadamente el **88%** de los clientes contactados rechazaron el producto (`no`) y solo un **12%** aceptaron (`yes`). Esto requirió una estrategia específica durante el modelado.
* **Dinámica Macroeconómica:** Los indicadores económicos del entorno muestran una altísima multicolinealidad. La tasa de variación del empleo (`emp.var.rate`) y el Euribor a 3 meses son determinantes en la disposición de ahorro de los usuarios.
* **Segmento Demográfico:** Los perfiles solteros (`single`) registran una tasa de conversión del 14%, superando ligeramente el 10% observado en clientes casados o divorciados.

---

## 🛠️ Preprocesamiento y Estrategia Técnica

Para garantizar la robustez de los modelos, se realizaron las siguientes transformaciones en el pipeline de datos:
1. **Tratamiento de Anomalías:** La variable `pdays` presentaba un valor de `999` para clientes nunca antes contactados, lo que distorsionaba las métricas de distancia. Se transformó en una variable binaria (`was_previously_contacted`).
2. **Codificación:** Se aplicó *One-Hot Encoding* (`pd.get_dummies`) con eliminación de la primera columna para evitar la trampa de la variable ficticia en los atributos categóricos.
3. **Escalado:** Las variables numéricas continuas se estandarizaron utilizando `StandardScaler` ajustado estrictamente sobre el conjunto de entrenamiento.
4. **Tratamiento del Desbalanceo:** Se incorporó el parámetro de penalización por peso de clase (`class_weight='balanced'`) en los algoritmos para evitar el sesgo hacia la clase mayoritaria.

---

## 🤖 Modelado y Evaluación

Se entrenaron y compararon dos enfoques algorítmicos sobre una partición de datos 80/20 (estratificada por la variable objetivo):

### Regresión Logística (Modelo Base Penalizado)
* **Accuracy:** 86%
* **Recall (Clase 1 - Sí):** **91%**
* **F1-Score (Clase 1):** 0.60
* **Falsos Negativos:** 82

### Random Forest (Ensemble Lineal Penalizado)
* **Accuracy:** 92%
* **Recall (Clase 1 - Sí):** 44%
* **F1-Score (Clase 1):** 0.54
* **Falsos Negativos:** 516

### 🏆 Selección del Modelo Ganador
Aunque *Random Forest* ofrece una precisión general (*accuracy*) más alta, **la Regresión Logística fue seleccionada como el modelo óptimo para el negocio**. 

En este contexto bancario, el coste de dejar pasar a un cliente interesado (Falso Negativo) es muchísimo mayor que el coste de llamar a un cliente que finalmente dice que no (Falso Positivo). La Regresión Logística logró capturar al **91% de los clientes potenciales interesados**, minimizando la pérdida de oportunidades comerciales.

---

## 📈 Factores de Impacto en el Negocio

Al analizar los coeficientes de la Regresión Logística, se extrajeron las siguientes conclusiones comerciales:

1. **El Entorno Económico Mandan (`emp.var.rate`):** Es la variable con mayor peso e impacto negativo. En periodos de alta volatilidad o destrucción de empleo, el cliente prioriza la liquidez inmediata frente al bloqueo de capital en un plazo fijo.
2. **Estacionalidad Crítica (`month_mar`):** El mes de marzo muestra una fuerza de conversión masiva. Las campañas enfocadas en este mes tienen una probabilidad de éxito sustancialmente superior frente al comportamiento contractivo de los meses de mayo y junio.
3. **La Recurrencia Funciona:** Haber tenido éxito en campañas anteriores (`poutcome_success`) es uno de los predictores positivos más estables.

---

## 💻 Tecnologías Utilizadas

* **Python 3**
* **Pandas & NumPy** (Manipulación de datos)
* **Matplotlib & Seaborn** (Visualización estática)
* **Scikit-Learn** (Preprocesamiento, entrenamiento y evaluación de modelos)
