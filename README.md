# ⚡ MantenIA | Asset Intelligence Engine (PoC)

**MantenIA** es una Prueba de Concepto (PoC) de un panel de control industrial de próxima generación (SaaS). Está diseñado para simular el monitoreo predictivo de activos (maquinaria, bombas, motores) mediante el análisis de registros técnicos y telemetría, transformando el mantenimiento reactivo en estrategias proactivas (Condition-Based Maintenance).

La interfaz cuenta con un diseño "Futurista/Industrial Dark Mode", gráficos en tiempo real simulados y un pipeline de inferencia por lotes (Batch) y manual.

---

## 🏗️ Arquitectura del PoC

La aplicación está construida sobre una arquitectura ligera y funcional orientada a datos:

1. **Frontend (UI/UX):** Desarrollado con **Streamlit**, implementando inyecciones de CSS puro para lograr un estilo HUD industrial y componentes interactivos sin necesidad de frameworks JS externos. Visualización de telemetría dinámica con **Plotly**.
2. **Procesamiento de Datos:** Uso de **Pandas** y **NumPy** para la ingesta de archivos CSV masivos (registros CMMS históricos) y la simulación matemática de espectros de vibración RMS.
3. **Motor de Inferencia (ML):** Pipeline de Procesamiento de Lenguaje Natural (NLP). El texto técnico o firmas de comportamiento ingresados se vectorizan y pasan por un "Ensemble" de clasificadores tradicionales de Machine Learning para emitir un diagnóstico binario (*Riesgo de Falla* vs *Operación Nominal*).

---

## ⚠️ Nota sobre los Modelos de Inferencia (.pkl)

Para fines de esta demostración rápida (PoC) y para permitir que la interfaz gráfica sea completamente interactiva de inmediato, **los modelos empaquetados en este repositorio son de muestra preliminar.**

> **Contexto de los Modelos:** Los archivos incluidos fueron entrenados originalmente para detectar la **polaridad (sentimientos positivos/negativos) de comentarios de productos tipo Amazon**. 
>
> En el contexto de esta aplicación industrial, el sistema hace un mapeo simulado: si el modelo detecta un texto con semántica "negativa", el dashboard lo interpreta y visualiza como un **"Riesgo de Falla / Anomalía"** en la maquinaria. Si detecta texto "positivo", lo mapea como **"Operación Nominal"**.

### Archivos de modelo utilizados:
* `vectorizador_prod.pkl`: Vectorizador (TF-IDF/Count) que transforma el texto de entrada en matrices numéricas.
* `modelo_svm.pkl`: Modelo Support Vector Machine (Clasificador principal del sistema).
* `modelo_naive_bayes.pkl`: Modelo probabilístico Naive Bayes (Motor secundario).
* `modelo_lda_prod.pkl`: Latent Dirichlet Allocation (Cargado en la caché del sistema, pensado para agrupamiento de tópicos/causas raíz en futuras versiones).

*(Nota: La aplicación incluye un mecanismo de "Fallback" automático. Si estos archivos `.pkl` se eliminan o no se encuentran, la app arrancará en "Modo Simulación" evaluando palabras clave básicas).*

---

## 🚀 Instrucciones de Ejecución Rápida

Sigue estos pasos para levantar la aplicación en tu entorno local en menos de 2 minutos.

### 1. Requisitos previos
Asegúrate de tener Python 3.8 o superior instalado en tu sistema.

### 2. Instalación de dependencias
Abre tu terminal, navega a la carpeta del proyecto y ejecuta el siguiente comando para instalar las librerías necesarias (se incluye `scikit-learn` que es necesario para cargar los modelos `.pkl`):

```bash
pip install streamlit pandas numpy joblib plotly scikit-learn
```

### 3. Ejecutar la aplicación

Una vez instaladas las dependencias, lanza el servidor local de Streamlit ejecutando el script principal:

```bash
streamlit run app_correcta.py
```

Automáticamente se abrirá una pestaña en tu navegador web (por defecto en `http://localhost:8501`) mostrando la secuencia de arranque del motor MantenIA.


### 📂 Estructura de Módulos

- **📊 Dashboard General:** KPIs globales de salud de los activos y telemetría simulada en tiempo real.
- **🔍 Inferencia Manual:** Herramienta para probar el modelo escribiendo reportes técnicos o síntomas de la maquinaria a mano.
- **📁 Análisis en Lote (CSV):** Ingesta masiva de datos estructurados para predecir fallas históricas de forma paralela.
- **📋 Log de Telemetría:** Registro de auditoría temporal (`st.session_state`) de todas las inferencias realizadas en la sesión actual.
- **📖 Documentación:** Especificaciones técnicas y hoja de ruta (Roadmap V2.0).

> *Desarrollado como prototipo de visualización de Inteligencia Artificial Industrial.*
