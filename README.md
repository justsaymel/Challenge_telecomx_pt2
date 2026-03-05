# **Telecom X – Parte 2: Predicción de Cancelación (Churn)**

En cumplimiento con el objetivo de anticipar la cancelación de clientes, se ha desarrollado un pipeline robusto de Machine Learning. Este sistema permite a Telecom X transitar de un análisis reactivo a uno proactivo, identificando patrones de fuga antes de que el cliente finalice su contrato.

---

## **Tecnologías Utilizadas**
* **Python**
* **Pandas / Numpy** (Manipulación de datos)
* **Scikit-Learn** (Modelado y Preprocesamiento)
* **Seaborn / Matplotlib** (Visualización avanzada)

---

## **Objetivo del Proyecto**
Desarrollar, evaluar e interpretar modelos predictivos para reducir la tasa de cancelación (*Churn*), permitiendo a la empresa tomar decisiones proactivas y diseñar estrategias de retención personalizadas.

---

## **Fases del Proyecto**

### **1. Extracción y Preparación de los Datos**
Se realizó una limpieza profunda y transformación de los datos originales:
* **Tratamiento de nulos y duplicados.**
* **Codificación:** Transformación de variables categóricas a numéricas.
* **Escalado:** Uso de `StandardScaler` para normalizar las magnitudes de cargos financieros y antigüedad.

### **2. Correlación y Selección de Variables**
Se identificaron los factores que tienen mayor relación directa con la fuga de clientes. La matriz de correlación npermitió entender cómo interactúan las variables entre sí.

![Matriz de Correlación](matriz_correlacion_variables.png)

### **3. Análisis Exploratorio Avanzado**
Para entender el comportamiento de los clientes que cancelan frente a los que permanecen, se analizó la distribución de la antigüedad y los cargos:

#### **A. Antigüedad y Gasto Total**
Los siguientes gráficos muestran que los clientes con menor permanencia y cargos totales más bajos son los más propensos al abandono.

| Distribución de Antigüedad | Distribución de Gastos Totales |
| :---: | :---: |
| ![Boxplot Tenure](boxplot_tenure.png) | ![Boxplot Charges](boxplot_charges.png) |

#### **B. Relación Multivariada**
En este gráfico de dispersión se observa la "zona de riesgo": clientes con pocos meses de antigüedad y cargos acumulados bajos concentran la mayor cantidad de cancelaciones (puntos rojos).

![Scatter Multivariado](scatter_multivariado_churn.png)

---

## **Modelado Predictivo**
Se entrenaron y compararon dos arquitecturas para encontrar el equilibrio entre precisión y generalización:

| Métrica | Regresión Logística (Ganador) | Random Forest |
| :--- | :---: | :---: |
| **Exactitud (Test)** | **80.19%** | 78.82% |
| **Recall (Sensibilidad)** | **53.74%** | 49.73% |
| **F1-Score** | **58.26%** | 54.71% |

**Conclusión técnica:** Se eligió la **Regresión Logística** por su estabilidad y nulo sobreajuste (Overfitting), a diferencia del Random Forest que mostró un rendimiento dispar entre entrenamiento (99%) y prueba (78%).

---

### **Análisis de Coeficientes (Regresión Logística)**
Para entender no solo qué variables importan, sino **cómo** afectan la decisión del cliente, se analizaron los coeficientes del modelo. Los valores positivos aumentan el riesgo de Churn, mientras que los valores negativos indican factores de retención.

#### **Factores que aumentan el riesgo de cancelación (Top Positivos)**
| Variable | Coeficiente | Impacto en el Negocio |
| :--- | :---: | :--- |
| **Charges.Total** | `0.6750` | A mayor gasto acumulado, mayor sensibilidad al precio. |
| **InternetService_Fiber optic** | `0.6277` | Los clientes con fibra óptica tienen más tendencia a irse. |
| **PaperlessBilling** | `0.1895` | La facturación electrónica se asocia con mayor volatilidad. |
| **StreamingTV / StreamingMovies** | `0.1840` | El uso de servicios de entretenimiento aumenta el riesgo. |
| **PaymentMethod_Electronic check** | `0.1612` | El método de pago menos estable. |
| **MultipleLines** | `0.1495` | Clientes con múltiples líneas son más propensos al cambio. |

#### **Factores que fomentan la lealtad (Top Negativos)**
| Variable | Coeficiente | Impacto en el Negocio |
| :--- | :---: | :--- |
| **tenure (Antigüedad)** | `-1.4231` | **El factor más fuerte.** A más tiempo, menos riesgo. |
| **Contract_Two year** | `-0.5455` | Los contratos largos blindan al cliente. |
| **InternetService_No** | `-0.5213` | Clientes sin internet (solo telefonía) son muy leales. |
| **Charges.Monthly** | `-0.3452` | Curiosamente, cargos mensuales controlados retienen mejor. |
| **Contract_One year** | `-0.2583` | El contrato anual es una barrera efectiva contra el Churn. |
| **TechSupport / OnlineSecurity** | `-0.1281` | Los servicios de valor agregado aumentan la permanencia. |

> **Interpretación clave:** La antigüedad (`tenure`) es el factor con más peso para evitar que un cliente se vaya. Por el contrario, los cargos totales altos y el uso de fibra óptica son los principales motores que impulsan al cliente hacia la cancelación.

### **Importancia de las Variables**
El modelo de Random Forest permite visualizar qué factores pesan más en la decisión del cliente:

![Importancia de Variables](importancia_variables_rf.png)

---

## **Conclusiones y Recomendaciones Finales**

Tras completar el pipeline de Machine Learning para **Telecom X**, se han llegado a las siguientes conclusiones clave:

### **1. El Modelo Ganador**
Se ha seleccionado la **Regresión Logística** como el modelo principal para la empresa. Aunque el Random Forest mostró una capacidad de aprendizaje altísima en los datos de entrenamiento (99.09%), su incapacidad para generalizar en los datos de prueba lo descartó por **Overfitting**. 

La Regresión Logística ofrece:
* **Estabilidad:** 80.19% de exactitud constante.
* **Capacidad de detección:** Un Recall superior (**53.74%**), logrando identificar a **201 clientes** que efectivamente cancelaron, superando los 186 detectados por el bosque aleatorio.

### **2. Factores Determinantes del Abandono**
Gracias al análisis de importancia de variables y los coeficientes, se determinó que:
* **El tiempo es oro:** Los clientes con baja antigüedad son los más volátiles. La retención debe enfocarse en los primeros **12 meses** de vida del cliente.
* **Costo vs. Valor:** Los cargos mensuales altos y el servicio de **Fibra Óptica** son puntos de fricción. Es posible que la competencia ofrezca mejores precios o que el servicio presente fallas técnicas.
* **Barreras de Salida:** Los contratos a largo plazo (uno o dos años) y los métodos de pago automático actúan como los mejores retenedores naturales de clientes.

---

## **Insights Estratégicos**
1. **Ventana Crítica:** Los primeros 12 meses de antigüedad son el periodo de mayor riesgo de fuga.
2. **Método de Pago:** El uso de *Electronic Check* está vinculado a una mayor cancelación; incentivar pagos automáticos podría mejorar la retención.
3. **Servicio Técnico:** La variable *InternetService_Fiber optic* aparece como un factor relevante de Churn, lo que sugiere revisar la calidad o costo de este servicio.
