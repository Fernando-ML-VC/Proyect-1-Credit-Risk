# 💳 Proyecto 1: Evaluación y Predicción de Riesgo Crediticio

## 📄 Resumen Ejecutivo
Este proyecto desarrolla y evalúa modelos de Machine Learning para predecir la probabilidad de *default* (incumplimiento) en solicitudes de crédito. Se estructuró un flujo de trabajo iterativo que abarca desde la preparación de datos y manejo de desbalanceo, hasta la comparación rigurosa de algoritmos bajo criterios de desempeño predictivo, eficiencia computacional e impacto de negocio.

---

## 📊 Tabla Comparativa de Resultados Consolidados

Para garantizar la simulación exacta de un entorno de producción, la medición de tiempos de inferencia (*Inference Time*) se realizó de forma estandarizada sobre el dataset de **Test** (datos totalmente no vistos).

| Modelo | Configuración / Dataset | Accuracy | Precision (Clase 1) | Recall (Clase 1) | F1-Score | Fit Time (s) | Inference Time Test (s) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | Baseline (PCA 95%) | 84.35% | 75.79% | 47.04% | 0.5805 | 0.22s | 0.066s |
| **SVC** | RBF Kernel + PCA (GridSearch) | *[Métrica]* | *[Métrica]* | *[Métrica]* | *[Métrica]* | 1050.93s | *[Métrica]* |
| **Random Forest** | Standalone (Sin class_weight) | 83.70% | 80.12% | 39.10% | 0.5260 | *[Métrica]* | *[Métrica]* |
| **Random Forest** | Class Weight Balanced | -- | 41.80% | 68.50% | 0.5195 | *[Métrica]* | *[Métrica]* |
| **XGBoost (Tuned)** | Modelo Final (Gradient Boosting) | *[Métrica]* | *[Métrica]* | *[Métrica]* | *[Métrica]* | *[Métrica]* | *[Métrica]* |

---

## 📈 Análisis Diagnóstico y Justificación Técnica

### 1. Generalización y Comportamiento del Aprendizaje
- **Regresión Logística (Baseline):** Demostró una capacidad de generalización excelente. El F1-Score se mantuvo estable entre validación (0.5800) y testeo (0.5805), confirmando ausencia de *overfitting*.
- **Modelos Complejos:** Si bien algoritmos no lineales capturan patrones más complejos, requieren un control estricto de hiperparámetros para evitar la memorización del conjunto de entrenamiento.

### 2. Compromiso de Negocio (Precision vs. Recall)
En el dominio financiero, la elección de la métrica óptima responde a una balanza de costos:
- **Pérdida Directa (Falsos Negativos):** Aprobar un crédito a un cliente moroso representa la pérdida total del capital.
- **Costo de Oportunidad (Falsos Positivos):** Rechazar a un buen pagador implica perder la ganancia de cobro de intereses y ceder mercado a la competencia.

> **Caso de Estudio en Random Forest:**  
> Al incorporar `class_weight='balanced'`, se logró maximizar la captura de impagos elevanado el **Recall al 68.5%**. Sin embargo, esto provocó una severa degradación en la **Precision (41.8%)**, lo que significaba que casi 6 de cada 10 clientes rechazados eran en realidad solventes. Debido a este alto costo de oportunidad, se optó por priorizar modelos que sostengan un F1-Score equilibrado.

### 3. Eficiencia y Factibilidad de Despliegue (Latencia)
- **Costo de Entrenamiento:** Modelos basados en Support Vector Machines (**SVC**) requirieron un tiempo de entrenamiento elevado (>1050 segundos) impulsado por la búsqueda de hiperparámetros, complicando ciclos de reentrenamiento frecuente.
- **Latencia de Respuesta:** La Regresión Logística y los árboles ensemble optimizados mostraron tiempos de inferencia en orden de milisegundos sobre el set de prueba, posicionándose como los candidatos viables para APIs de decisión en tiempo real.

---

## 🎯 Conclusión y Modelo Final Seleccionado

Se selecciona **[Insertar Nombre del Modelo Final, ej: XGBoost]** como el algoritmo para despliegue en producción. 

**Justificación:**
1. Ofrece el **F1-Score (0.XX)** más consistente del benchmark, garantizando la detección efectiva del riesgo de impago.
2. Mantiene una **Precision de [XX]%**, asegurando un flujo de aprobación de créditos saludable sin descartar clientes solventes de forma desmedida.
3. Presenta una latencia de predicción eficiente (**[X.XX]s**) apta para integrarse con sistemas de *credit scoring* automatizados.
