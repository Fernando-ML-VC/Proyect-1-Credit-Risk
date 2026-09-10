# 💳 Proyecto 1: Evaluación y Predicción de Riesgo Crediticio

## 📄 Resumen
Este proyecto desarrolla y evalúa modelos de Machine Learning para predecir la probabilidad de *default* (incumplimiento) en solicitudes de crédito. Se estructuró un flujo de trabajo que abarca desde la preparación de datos y busqueda de valores óptimos para los hiperparámetros, hasta la comparación rigurosa de algoritmos bajo criterios de desempeño predictivo, eficiencia computacional e impacto de negocio.

---

## 📊 Tabla Comparativa de Resultados Consolidados

Para garantizar la simulación exacta de un entorno de producción, la medición de tiempos de inferencia (*Inference Time*) se realizó de forma estandarizada sobre el dataset de **Test** (datos totalmente no vistos).

| Modelo | Configuración / Dataset | Accuracy | Precision | Recall | F1-Score | Fit Time (s) | Inference Time Test (s) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | (PCA 95%) | 84.35% | 75.79% | 47.04% | 58.05% | 0.33s | 0.025s |
| **SVC** | RBF Kernel + PCA (GridSearch) | 90.88% | 93.07% | 65.25% | 76.71% | 654.30s | 1.465s |
| **Random Forest** | GridSearch, Sin class_weight | 83.79% | 80.18% | 39.30% | 52.74% | 716.57s | 0.048s |
| **Random Forest** | Class Weight Balanced | 70.81% | 41.81% | 68.58% | 51.95% | 1146.89s | 0.085s |
| **XGBoost** | GridSearch, Gradient Boosting | 83.76% | 80.12% | 39.15% | 52.59% | 776.81s | 0.017s |

---

## 📈 Análisis Diagnóstico y Justificación Técnica

### 1. Generalización y Overfitting
- **Support Vector Machines:** Demostró una capacidad de generalización excelente. El F1-Score se mantuvo estable entre validación (0.7824) y testeo (0.7671), confirmando ausencia de *overfitting*.

### 2. Compromiso de Negocio (Precision vs. Recall)
La elección de la métrica a optimizar responde a una estrategia de negocio:
- **Pérdida Directa (Falsos Negativos):** Aprobar un crédito a un cliente moroso representa la pérdida total del capital.
- **Costo de Oportunidad (Falsos Positivos):** Rechazar a un buen pagador implica perder la ganancia de cobro de intereses y ceder mercado a la competencia.

> **Caso de Estudio en Random Forest:**  
> Al incorporar `class_weight='balanced'`, se logró maximizar la captura de deudores morosos, elevanado el **Recall (68.58%)**. Sin embargo, esto provocó una severa degradación en la **Precision (41.81%)**, lo que significa, que casi 6 de cada 10 clientes rechazados eran en realidad solventes.

### 3. Eficiencia y Latencia
- **Costo de Entrenamiento:** Modelos basados en Support Vector Machines (**SVC**) requirieron un tiempo de entrenamiento elevado (654.30 segundos) impulsado por la búsqueda de hiperparámetros.
- **Latencia de Respuesta:** La Regresión Logística y los árboles ensemble optimizados mostraron tiempos de inferencia en orden de milisegundos sobre el set de prueba.

---

## 🎯 Conclusión y Modelo Final Seleccionado

Se selecciona **Support Vector Machines** como el algoritmo para despliegue en producción. 

**Justificación:**
1. Ofrece el **F1-Score (0.7671)** más consistente del benchmark.
2. Mantiene una **Precision de 0.9307**, asegurando un flujo de aprobación de créditos saludables a clientes solventes.
3. Conserva un **Recall de 0.6525**, evitando el préstamo a deudores morosos. 
4. Presenta una latencia de predicción de **1.46s**, esto es sobre el total de 2864 ejemplos, por cada ejemplos el tiempo es de **0.0005097s**.
5. Es el **modelo con mejor desempeño predictivo global**.
---

## ☁️ Escalabilidad en la Nube

Si bien en entornos con restricciones estrictas de cómputo (en local), los tiempos de entrenamiento de los algoritmos pueden considerarse un factor limitante.

**Factibilidad Operativa en la Nube:** Una entidad financiera cuenta con la capacidad de delegar el entrenamiento e inferencia a servicios de cómputo elástico en la nube (ej. AWS EC2). 

**Conclusión:**  
El costo marginal de alquilar recursos computacionales en la nube, es ampliamente absorbido por las ganancias al evitar los créditos a personas morosas (vía su alto Recall) sin descartar a clientes solventes (gracias a su Precision). La elección definitiva entre SVC o alternativas más livianas como Regresión Logística queda a discreción de la infraestructura y el presupuesto operativo del negocio.
