# Proyecto Final — Inteligencia Artificial  
## Sistema Inteligente Híbrido para la Planificación Académica Universitaria bajo Restricciones e Incertidumbre

<div align="center">

**Universidad del Valle de Guatemala**  
**CC3045 — Inteligencia Artificial**  
**Semestre I — 2026**

**Autores:** Pablo Daniel Barillas Moreno, Hugo Daniel Barillas Ajín y Roberto Nájera  
**Docente:** Ing. Suriano  

</div>

---

## Descripción general

Este proyecto propone el diseño e implementación de un **sistema inteligente híbrido** para apoyar la **planificación académica universitaria**. El sistema integra múltiples técnicas de Inteligencia Artificial vistas en el curso para resolver un problema real y complejo en varias capas de decisión:

1. **Predicción de demanda o riesgo académico** mediante aprendizaje supervisado.  
2. **Generación de horarios académicos** usando **Problemas de Satisfacción de Restricciones (CSP)** y heurísticas de búsqueda.  
3. **Evaluación probabilística de riesgo** mediante un módulo bayesiano.  
4. **Replanificación adaptativa** usando un **Proceso de Decisión de Markov (MDP)** simplificado.

La propuesta busca demostrar que la IA no debe verse como un conjunto de técnicas aisladas, sino como un sistema de módulos complementarios que cooperan para resolver un problema del mundo real.

---

## Problema que resuelve

La planificación académica en una universidad implica coordinar:

- cursos,
- estudiantes,
- docentes,
- aulas,
- bloques horarios,
- restricciones administrativas,
- preferencias,
- y contingencias operativas.

Hacer esto manualmente es costoso, propenso a errores y difícil de escalar. Además, no basta con generar un horario válido: también es necesario anticipar demanda, estimar riesgos y reaccionar ante cambios inesperados.

---

## Objetivo general

Diseñar e implementar un sistema inteligente híbrido que integre técnicas de:

- **Machine Learning**
- **CSP y búsqueda heurística**
- **Razonamiento probabilístico**
- **MDP**

para apoyar la toma de decisiones en la planificación académica universitaria.

---

## Objetivos específicos

- Construir un módulo de predicción con modelos supervisados.
- Modelar la asignación de horarios como un CSP.
- Aplicar heurísticas como **MRV**, **LCV** y **Forward Checking**.
- Comparar estrategias como **Backtracking** y **Beam Search**.
- Modelar incertidumbre mediante un módulo bayesiano.
- Proponer una política de replanificación usando un MDP simplificado.
- Integrar todos los módulos en un solo flujo de trabajo reproducible.

---

## Temas del curso integrados

Este proyecto se relaciona directamente con varios temas del curso de Inteligencia Artificial:

- Agentes inteligentes
- Solución de problemas mediante búsqueda
- Heurísticas
- CSP / Factor Graphs
- Beam Search
- Razonamiento probabilístico
- Redes Bayesianas / inferencia probabilística
- Machine Learning supervisado
- Naive Bayes, árboles de decisión, regresión logística, SVM y redes neuronales
- Markov Decision Processes (MDP)
- Evaluación de modelos
- Ingeniería de features y manejo de datos

---

## Arquitectura del sistema

El sistema está organizado en cuatro módulos principales:

### 1. Módulo de predicción
Utiliza aprendizaje supervisado para estimar demanda, riesgo académico o comportamiento estudiantil a partir de datos históricos.

### 2. Módulo de generación de horarios
Modela el problema de asignación como un CSP:

- variables,
- dominios,
- restricciones duras,
- restricciones blandas,
- función objetivo.

Se resuelve con:

- Backtracking
- Forward Checking
- MRV
- LCV
- Beam Search

> **Nota importante:** para fines computacionales y de demostración en notebook, el solver CSP trabaja con una **instancia reducida/escalada** del problema, ya que resolver cientos de secciones con backtracking puro no es viable en tiempo razonable.

### 3. Módulo probabilístico
Emplea un modelo bayesiano o una aproximación probabilística para estimar el riesgo de conflictos como:

- sobrecupo,
- indisponibilidad docente,
- conflictos de espacio,
- necesidad de replanificación.

Este módulo produce un `risk_df` con el riesgo estimado por sección o por decisión.

### 4. Módulo de replanificación
Usa un MDP simplificado para decidir la mejor acción ante contingencias:

- mover curso,
- cambiar aula,
- reasignar docente,
- abrir nueva sección,
- mantener el plan actual.

---

## Dataset

Para el componente de aprendizaje supervisado se utiliza el dataset **OULAD (Open University Learning Analytics Dataset)** como base de experimentación y desarrollo del módulo predictivo.

### Importante sobre la carga de datos
El dataset **no se carga desde `ucimlrepo`**, ya que en algunos entornos ese import falla para OULAD.  
En este proyecto se utiliza la versión **local en CSV**, leída directamente desde carpetas como:

- `data/oulad/`
- `data/`
- o el directorio actual

### Archivos esperados de OULAD

- `studentInfo.csv`
- `studentRegistration.csv`
- `studentAssessment.csv`
- `studentVle.csv`
- `assessments.csv`
- `courses.csv`

La parte de generación de horarios utiliza una instancia estructurada del problema académico con:

- cursos,
- docentes,
- aulas,
- horarios,
- restricciones,
- demanda estimada.

---

## Estructura del repositorio

```bash
Proyecto-Final_IA/
│
├── README.md
├── proyecto_final_ia_hibrido_corregido.ipynb
├── proyecto_final_ia_hibrido.ipynb
│
├── informe/
│   ├── main.tex
│   ├── referencias.bib
│   └── figuras/
│
├── data/
│   ├── oulad/
│   │   ├── studentInfo.csv
│   │   ├── studentRegistration.csv
│   │   ├── studentAssessment.csv
│   │   ├── studentVle.csv
│   │   ├── assessments.csv
│   │   └── courses.csv
│   │
│   └── ...
│
├── assets/
│   ├── portada.png
│   ├── pipeline.gif
│   ├── arquitectura.png
│   └── horario_mockup.png
│
├── outputs/
│   ├── graficas/
│   ├── tablas/
│   └── modelos/
│
└── docs/
    └── presentacion.pdf
````

---

## Notebook principal

El archivo principal recomendado para ejecución y entrega técnica es:

```bash
proyecto_final_ia_hibrido_corregido.ipynb
```

Este notebook contiene:

* portada visual,
* justificación del problema,
* desarrollo teórico,
* fórmulas con MathJax,
* carga y limpieza de datos,
* entrenamiento de modelos,
* gráficas y visualizaciones,
* generación de horarios,
* evaluación de riesgo,
* modelo MDP,
* y conclusiones.

El archivo `proyecto_final_ia_hibrido.ipynb` puede conservarse como versión previa o borrador.

---

## Requisitos

Instalar Python 3.10+ y las dependencias necesarias.

### Librerías principales

```bash
pip install pandas numpy matplotlib seaborn scikit-learn requests networkx jupyter notebook
```

Opcionalmente:

```bash
pip install pgmpy
```

---

## Cómo ejecutar el proyecto

### 1. Clonar el repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd Proyecto-Final_IA
```

### 2. Crear entorno virtual

```bash
python -m venv .venv
```

En Windows:

```bash
.venv\Scripts\activate
```

En Linux/macOS:

```bash
source .venv/bin/activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

Si no existe `requirements.txt`, instalar manualmente:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn requests networkx pgmpy notebook
```

### 4. Colocar el dataset en la carpeta `data/oulad/`

Asegúrate de incluir los archivos CSV requeridos por el notebook.

### 5. Ejecutar Jupyter Notebook

```bash
jupyter notebook
```

Luego abre:

```bash
proyecto_final_ia_hibrido_corregido.ipynb
```

---

## Flujo general del proyecto

```text
Datos históricos
   ↓
Preprocesamiento e ingeniería de features
   ↓
Predicción de demanda / riesgo académico
   ↓
Generación de horarios con CSP
   ↓
Evaluación probabilística de riesgo
   ↓
Replanificación adaptativa con MDP
   ↓
Horario final + métricas + visualizaciones
```

---

## Resultados esperados

El proyecto no solo busca generar un horario, sino evaluar integralmente la solución con métricas como:

### Módulo ML

* Accuracy
* Precision
* Recall
* F1-score
* RMSE / MAE

### Módulo CSP

* Número de restricciones duras satisfechas
* Número de restricciones blandas satisfechas
* Número de backtracks
* Tiempo de ejecución
* Calidad total del horario

### Módulo de riesgo

* Riesgo promedio de conflicto
* Número de secciones con riesgo alto
* Probabilidad estimada de reprogramación

### Módulo MDP

* Utilidad esperada
* Calidad de la política
* Tasa de resolución de contingencias

---

## Consideraciones técnicas importantes

### 1. Carga de OULAD

El proyecto fue corregido para leer OULAD desde archivos CSV locales, evitando errores como:

* `DatasetNotFoundError` con `ucimlrepo`
* errores de importación del dataset desde UCI

### 2. Conversión de tipos

Se aplicó coerción numérica a columnas que pueden venir como texto en OULAD para evitar errores en agregaciones `groupby`, por ejemplo:

* `score`
* `date_submitted`
* `sum_click`
* `weight`
* `id_student`

### 3. Solver CSP

El solver original no es viable con cientos de secciones. Por ello se corrigió para trabajar con:

* un subconjunto de secciones,
* dominios limitados,
* LCV aproximado,
* forward checking más eficiente,
* diagnóstico de escalabilidad.

### 4. Riesgo probabilístico

La celda de resumen depende de la existencia de `risk_df`, por lo que el módulo de evaluación de riesgo debe ejecutarse antes del resumen final.

---

## Entregables del proyecto

Este repositorio respalda los entregables del curso:

* Documento formal en PDF
* Notebook `.ipynb`
* Código fuente
* Repositorio público
* Video explicativo
* Presentación en PDF / slides

---

## Uso de IA generativa

Durante el desarrollo del proyecto se utilizaron herramientas de IA generativa como apoyo en:

* exploración de ideas,
* estructuración del enfoque híbrido,
* redacción técnica,
* refinamiento conceptual,
* depuración parcial de código,
* y organización del documento.

El modelado del problema, la integración de módulos, la implementación y el análisis final corresponden al trabajo del equipo.

---

## Limitaciones

* El sistema se encuentra planteado como prototipo académico.
* Algunas partes del pipeline dependen de la disponibilidad y limpieza del dataset.
* La integración completa entre todos los módulos puede requerir refinamiento adicional.
* El solver CSP en Python puro no escala a instancias masivas sin simplificación.
* La parte de replanificación se implementa como MDP simplificado.

---

## Trabajo futuro

* Integrar datos reales institucionales.
* Mejorar el generador de horarios con técnicas más avanzadas.
* Incorporar aprendizaje por refuerzo para replanificación.
* Usar modelos probabilísticos más ricos para riesgo.
* Desplegar el sistema como aplicación web o dashboard institucional.
* Reemplazar o complementar el solver artesanal con OR-Tools o CP-SAT.

---

## Referencias base

Algunas referencias centrales del proyecto incluyen:

* Russell, S., & Norvig, P. *Artificial Intelligence: A Modern Approach*.
* Dechter, R. *Constraint Processing*.
* Trabajos sobre timetabling académico, redes bayesianas, MDP y evaluación de modelos.

La bibliografía formal completa se encuentra en el informe del proyecto.

---

## Autores

* **Pablo Daniel Barillas Moreno**
* **Hugo Daniel Barillas Ajín**
* **Roberto Nájera**

---

## Licencia

Este proyecto se desarrolla con fines académicos para el curso **CC3045 — Inteligencia Artificial**.
