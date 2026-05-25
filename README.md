# Proyecto Final — Inteligencia Artificial  
## Sistema Inteligente Híbrido para la Planificación Académica Universitaria bajo Restricciones e Incertidumbre

<div align="center">

**Universidad del Valle de Guatemala**  
**CC3045 — Inteligencia Artificial**  
**Sección 10**  

**Grupo #1**  
**Autores:** Pablo Daniel Barillas Moreno, Hugo Daniel Barillas Ajín y Roberto Nájera  
**Docente:** Ing. Suriano  

</div>

---

## Video

**Link de video:** https://youtu.be/ChDjMZwtwaM

---

## Descripción general

Este proyecto implementa un **sistema inteligente híbrido** para apoyar la **planificación académica universitaria** bajo restricciones, incertidumbre y necesidad de replanificación. La idea central es que una universidad no solo necesita predecir cuántos estudiantes podrían requerir apoyo o cupo, sino también transformar esa información en decisiones operativas: abrir secciones, asignar aulas, reducir conflictos y reaccionar ante contingencias.

El sistema integra cuatro componentes principales de Inteligencia Artificial:

1. **Machine Learning supervisado**, para estimar riesgo académico y comportamiento estudiantil.
2. **Problemas de Satisfacción de Restricciones (CSP)**, para generar horarios válidos bajo restricciones.
3. **Razonamiento probabilístico / red bayesiana**, para estimar riesgo operativo de replanificación.
4. **Proceso de Decisión de Markov (MDP)**, para seleccionar acciones de replanificación ante escenarios inciertos.

La propuesta muestra cómo distintas técnicas vistas en el curso pueden conectarse en un solo flujo de trabajo reproducible.

---

## Problema que resuelve

La planificación académica universitaria implica coordinar:

- estudiantes,
- cursos,
- docentes,
- aulas,
- cupos,
- horarios,
- restricciones administrativas,
- demanda estimada,
- riesgos académicos,
- y contingencias operativas.

Resolverlo manualmente es costoso, propenso a errores y difícil de escalar. Además, un horario no debería evaluarse únicamente como “válido” o “inválido”; también debe analizarse su riesgo, su ocupación, su estabilidad y su capacidad de adaptación.

Por eso el proyecto plantea una solución híbrida:

$$
\text{Datos históricos} \rightarrow \text{Predicción} \rightarrow \text{CSP} \rightarrow \text{Riesgo bayesiano} \rightarrow \text{MDP}
$$

---

## Objetivo general

Diseñar e implementar un sistema inteligente híbrido que combine aprendizaje supervisado, búsqueda con restricciones, razonamiento probabilístico y toma de decisiones secuencial para apoyar la planificación académica universitaria.

---

## Objetivos específicos

- Construir un módulo predictivo usando datos académicos tipo OULAD.
- Entrenar y comparar modelos supervisados para clasificar riesgo académico.
- Usar variables como desempeño, interacción virtual, créditos, intentos previos y características académicas.
- Modelar la asignación de horarios como un CSP.
- Aplicar heurísticas como **MRV**, **LCV** y **Forward Checking**.
- Generar un horario factible para una instancia académica reducida.
- Estimar riesgo de replanificación mediante un modelo probabilístico.
- Implementar un MDP simplificado para seleccionar acciones de replanificación.
- Exportar tablas, figuras y métricas para el informe en LaTeX.
- Presentar resultados mediante visualizaciones, GIFs y archivos generados automáticamente.

---

## Temas del curso integrados

Este proyecto integra los siguientes temas de Inteligencia Artificial:

- Agentes inteligentes.
- Búsqueda y solución de problemas.
- Heurísticas.
- CSP y satisfacción de restricciones.
- Backtracking, MRV, LCV y Forward Checking.
- Razonamiento probabilístico.
- Redes bayesianas e inferencia.
- Machine Learning supervisado.
- Evaluación de modelos de clasificación.
- Matriz de confusión, curva ROC y curva Precision-Recall.
- Procesos de Decisión de Markov.
- Value Iteration.
- Toma de decisiones bajo incertidumbre.
- Integración de módulos inteligentes.

---

## Formulación general

El sistema se entiende como una cadena de decisión:

$$
X \xrightarrow{\text{ML}} \hat{p}(riesgo) \xrightarrow{\text{CSP}} H \xrightarrow{\text{BN}} P(R \mid E) \xrightarrow{\text{MDP}} \pi^\*
$$

Donde:

- $X$ representa los datos históricos del estudiante y del curso.
- $\hat{p}(riesgo)$ es la probabilidad estimada de riesgo académico.
- $H$ es el horario generado por el módulo CSP.
- $P(R \mid E)$ es la probabilidad de replanificación dado un conjunto de evidencias $E$.
- $\pi^\*$ es la política óptima sugerida por el MDP.

---

## Arquitectura del sistema

### 1. Módulo de predicción supervisada

El primer módulo procesa datos académicos y entrena modelos de clasificación para estimar si un estudiante se encuentra en riesgo.

La variable objetivo se construye a partir del resultado final del estudiante:

$$
y =
\begin{cases}
1, & \text{si el estudiante falla o se retira} \\
0, & \text{si el estudiante aprueba o distingue}
\end{cases}
$$

El módulo compara varios modelos, entre ellos:

- Random Forest.
- Extra Trees.
- Regresión logística.
- SGD con pérdida logística.
- Baseline dummy.

El mejor modelo reportado en la ejecución incluida fue:

$
\text{RandomForest\_fast}
$

con métricas aproximadas:

- Accuracy: $0.9333$
- F1-score: $0.9344$
- ROC-AUC: $0.9809$
- Tiempo total de entrenamiento: $0.66$ segundos

> Estas métricas provienen de los archivos exportados en `output_latex/`, específicamente de `kpis_globales.json` y `tabla_ml_modelos.csv`.

---

### 2. Módulo CSP de horarios

El segundo módulo transforma la demanda estimada en un problema de asignación de horarios.

Un CSP se define como:

$$
CSP = (X, D, C)
$$

Donde:

- $X$ es el conjunto de variables.
- $D$ es el conjunto de dominios posibles.
- $C$ es el conjunto de restricciones.

En el proyecto:

- Las variables representan secciones de cursos.
- Los dominios representan combinaciones posibles de día, hora, aula y profesor.
- Las restricciones evitan choques de aula, choques de docente y exceso de capacidad.

El solver usa:

- Backtracking.
- MRV.
- LCV.
- Forward Checking.

> Por razones de escalabilidad, el CSP trabaja con una instancia reducida. Resolver cientos de secciones con backtracking puro no es viable en tiempo razonable.

---

### 3. Módulo probabilístico

El tercer módulo estima el riesgo operativo de replanificación. Se modelan evidencias como:

- alta demanda,
- disponibilidad docente,
- disponibilidad de aula.

El objetivo es estimar una probabilidad como:

$$
P(\text{Replanificación}=1 \mid \text{AltaDemanda}, \text{DispDocente}, \text{DispAula})
$$

La tabla de inferencias exportada se encuentra en:

```text
output_latex/tabla_bn_inferencias.csv
```

Ejemplo conceptual:

$$
P(R=1 \mid AltaDemanda=1, DispDocente=0, DispAula=0)
$$

representa la probabilidad de necesitar replanificación cuando hay alta demanda y no existe disponibilidad suficiente de docente ni aula.

---

### 4. Módulo MDP de replanificación

El cuarto módulo usa un MDP para decidir qué acción tomar frente a una contingencia.

Un MDP se define como:

$$
MDP = (S, A, P, R, \gamma)
$$

Donde:

- $S$ es el conjunto de estados.
- $A$ es el conjunto de acciones.
- $P(s' \mid s,a)$ es la probabilidad de transición.
- $R(s,a)$ es la recompensa.
- $\gamma$ es el factor de descuento.

La actualización de Value Iteration se basa en:

$$
V_{k+1}(s) = \max_a \left[ R(s,a) + \gamma \sum_{s'} P(s' \mid s,a)V_k(s') \right]
$$

En la ejecución registrada, la mejor acción sugerida fue:

```text
open_extra
```

lo cual representa abrir una sección adicional cuando el sistema detecta riesgo o presión de demanda.

---

## Dataset

Para el módulo de aprendizaje supervisado se utiliza el dataset **OULAD (Open University Learning Analytics Dataset)** en versión local CSV.

### Archivos usados por el proyecto

El notebook trabaja con archivos dentro de:

```text
data/oulad/
```

Archivos incluidos en el repositorio o ZIP:

```text
assessments.csv
courses.csv
studentAssessment.csv
studentInfo.csv
studentRegistration.csv
vle.csv
```

### Nota importante sobre `studentVle.csv`

El archivo:

```text
studentVle.csv
```

**sí se usó durante el desarrollo y ejecución local del proyecto**, porque permite calcular variables de interacción como:

- `total_clicks`
- `active_days`
- `avg_clicks_per_day`
- `max_clicks_day`
- `std_clicks_day`

Sin embargo, **no se subió al repositorio de GitHub por limitaciones de tamaño de archivos**. Por eso puede aparecer en la carpeta local del equipo, pero no necesariamente dentro del ZIP o del repositorio remoto.

El código del notebook está preparado para leerlo si existe localmente. Si no existe, el proyecto puede seguir ejecutándose con las variables disponibles o con el modo sintético/reducido definido en el notebook.

---

## Estructura del repositorio

```text
Proyecto-Final_IA_Seccion_10_Grupo_1/
│
├── data/
│   └── oulad/
│       ├── assessments.csv
│       ├── courses.csv
│       ├── studentAssessment.csv
│       ├── studentInfo.csv
│       ├── studentRegistration.csv
│       ├── studentVle.csv              # Usado localmente; no subido a GitHub por tamaño.
│       └── vle.csv
│
├── Enlace_y_presentación/
│   ├── ProyectoFinalPresentacion.pdf
│   └── Repo y presentación - Enlaces_Grupo #1_IA_Sección_10.docx
│
├── informe/
│   ├── Informe-IA.pdf
│   ├── main.tex
│   └── referencias.bib
│
├── Instrucciones/
│   └── Proyecto Final.pdf
│
├── Licencia/
│   └── LICENSE
│
├── output_latex/
│   ├── fig_14_bn_riesgo.pdf
│   ├── fig_bn_tabla_inferencias.pdf
│   ├── fig_cm.pdf
│   ├── fig_csp_heatmap.pdf
│   ├── fig_importancia.pdf
│   ├── fig_mdp_convergencia.pdf
│   ├── fig_mdp_qtable.pdf
│   ├── fig_ml_modelos.pdf
│   ├── fig_ocupacion.pdf
│   ├── fig_pr.pdf
│   ├── fig_roc.pdf
│   ├── kpis_globales.json
│   ├── tabla_bn_inferencias.csv
│   ├── tabla_bn_metricas.tex
│   ├── tabla_demanda_modulos.csv
│   ├── tabla_horario.csv
│   ├── tabla_mdp_policy.csv
│   ├── tabla_mdp_qtable.csv
│   ├── tabla_ml_modelos.csv
│   ├── tabla_ml_modelos.tex
│   └── tabla_riesgo_bn.csv
│
├── proyecto_final_mejorado_assets/
│   ├── 01_pipeline_hibrido.png
│   ├── 02_csp_restricciones.png
│   ├── 03_mdp_estados.png
│   ├── gif_01_pipeline_hibrido.gif
│   ├── gif_02_csp_asignacion.gif
│   └── gif_03_value_iteration.gif
│
├── .gitignore
├── proyecto_final_ia_hibrido.ipynb
└── README.md
```

---

## Notebook principal

El archivo principal del proyecto es:

```text
proyecto_final_ia_hibrido.ipynb
```

El notebook contiene:

- portada y explicación del proyecto,
- formulación matemática,
- carga de OULAD real o dataset sintético,
- limpieza y feature engineering,
- exploración inicial de datos,
- preparación de variable objetivo,
- entrenamiento supervisado,
- evaluación del mejor modelo,
- ajuste de umbral por costo,
- interpretabilidad rápida,
- estimación de demanda,
- CSP para horarios,
- visualización del horario generado,
- red bayesiana para riesgo operativo,
- MDP para replanificación,
- dashboard final,
- validación conceptual,
- bitácora de IA generativa,
- referencias egráficas,
- exportación de resultados para LaTeX.

---

## Assets visuales

El proyecto incluye recursos visuales para explicar el flujo híbrido:

```text
proyecto_final_mejorado_assets/
├── 01_pipeline_hibrido.png
├── 02_csp_restricciones.png
├── 03_mdp_estados.png
├── gif_01_pipeline_hibrido.gif
├── gif_02_csp_asignacion.gif
└── gif_03_value_iteration.gif
```

Estos recursos ayudan a comunicar:

- cómo los datos pasan por el pipeline completo,
- cómo el CSP asigna secciones bajo restricciones,
- cómo el MDP actualiza valores hasta obtener una política.

---

## Resultados exportados

Los resultados principales se guardan en:

```text
output_latex/
```

Entre los archivos más importantes están:

- `kpis_globales.json`: resumen global de métricas.
- `tabla_ml_modelos.csv`: comparación de modelos supervisados.
- `tabla_horario.csv`: horario generado por CSP.
- `tabla_bn_inferencias.csv`: inferencias probabilísticas.
- `tabla_mdp_policy.csv`: política del MDP.
- `tabla_mdp_qtable.csv`: tabla de valores por acción.
- `fig_roc.pdf`: curva ROC.
- `fig_pr.pdf`: curva Precision-Recall.
- `fig_cm.pdf`: matriz de confusión.
- `fig_csp_heatmap.pdf`: visualización del horario.
- `fig_mdp_convergencia.pdf`: convergencia del MDP.

---

## Requisitos

Se recomienda usar Python 3.10 o superior.

Instalación básica:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn networkx pillow imageio jupyter notebook
```

Opcional:

```bash
pip install pgmpy
```

Si se usa un archivo `requirements.txt`, puede instalarse con:

```bash
pip install -r requirements.txt
```

---

## Cómo ejecutar el proyecto

### 1. Clonar el repositorio

```bash
git clone <URL_DEL_REPOSITORIO>
cd Proyecto-Final_IA_Seccion_10_Grupo_1
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
pip install pandas numpy matplotlib seaborn scikit-learn networkx pillow imageio jupyter notebook
```

### 4. Colocar datos locales

La carpeta esperada es:

```text
data/oulad/
```

Para una ejecución completa con interacciones VLE, colocar manualmente:

```text
data/oulad/studentVle.csv
```

> Este archivo no está incluido en GitHub por tamaño, pero sí fue usado localmente para construir variables de interacción.

### 5. Ejecutar Jupyter Notebook

```bash
jupyter notebook
```

Abrir:

```text
proyecto_final_ia_hibrido.ipynb
```

y ejecutar las celdas en orden.

---

## Modo rápido y tiempo de ejecución

El notebook está diseñado para evitar entrenamientos demasiado largos. Para ello se recomienda mantener activo el modo rápido cuando se trabaja en laptop o durante una presentación:

```python
FAST_MODE = True
```

En modo rápido:

- se reduce el tamaño de entrenamiento,
- se usan modelos más ligeros,
- se limita la complejidad del CSP,
- se evita que la ejecución tarde una hora o más.

Para experimentos más completos se puede cambiar a:

```python
FAST_MODE = False
```

pero esto puede incrementar considerablemente el tiempo de ejecución.

---

## Flujo general del proyecto

```text
OULAD CSV local
      ↓
Limpieza y feature engineering
      ↓
Predicción de riesgo académico con ML
      ↓
Estimación de demanda por módulo
      ↓
Generación de horario con CSP
      ↓
Estimación probabilística de riesgo operativo
      ↓
MDP para replanificación
      ↓
Dashboard final + exportación para informe
```

---

## Métricas principales

Según la ejecución registrada en `output_latex/kpis_globales.json`:

| Métrica | Valor |
|---|---:|
| Modo de datos | OULAD real |
| Observaciones | 11,999 |
| Mejor modelo | RandomForest_fast |
| Accuracy | 0.9333 |
| F1-score | 0.9344 |
| ROC-AUC | 0.9809 |
| Tiempo total de entrenamiento | 0.66 s |
| Módulos académicos | 7 |
| Secciones generadas | 10 |
| Backtracks CSP | 0 |
| Brier Score BN | 0.117 |
| Riesgo promedio BN | 0.7188 |
| Mejor acción MDP | open_extra |

---

## Consideraciones técnicas importantes

### 1. Carga de datos

El proyecto fue preparado para cargar OULAD desde archivos CSV locales. Esto evita depender de importaciones externas que pueden fallar en algunos entornos.

### 2. Conversión de tipos

Algunos archivos de OULAD pueden leer columnas numéricas como texto. Por eso el notebook convierte de forma segura columnas como:

- `score`
- `date_submitted`
- `sum_click`
- `weight`
- `id_student`

Esto evita errores de agregación como:

```text
TypeError: agg function failed [how->mean,dtype->object]
```

### 3. Uso de `studentVle.csv`

`studentVle.csv` es necesario para obtener características de interacción virtual. Si no está presente, el notebook debe ejecutarse con el fallback disponible o con el dataset sintético/reducido.

### 4. CSP reducido

El solver CSP usa una instancia reducida para mantener tiempos razonables. Esto es una decisión intencional de diseño para que el proyecto sea demostrable en clase.

### 5. Exportación a LaTeX

Las tablas y figuras se exportan a `output_latex/` para facilitar su integración con el informe escrito en LaTeX.

---

## Entregables del proyecto

El repositorio contiene o respalda los siguientes entregables:

- Notebook `.ipynb`.
- Informe en PDF.
- Código fuente.
- Figuras generadas.
- Tablas exportadas.
- Presentación en PDF.
- Documento con enlaces.
- Licencia.
- README del proyecto.

---

## Uso de IA generativa

Durante el desarrollo del proyecto se utilizaron herramientas de IA generativa como apoyo en:

- organización del enfoque híbrido,
- redacción técnica,
- depuración de código,
- estructuración del README,
- generación de ideas visuales,
- validación conceptual de explicaciones.

El equipo revisó y ajustó el contenido para que la implementación coincidiera con lo visto en clase y con los resultados obtenidos en el notebook.

---

## Limitaciones

- El sistema es un prototipo académico.
- El CSP no pretende resolver una planificación universitaria completa a escala real.
- El archivo `studentVle.csv` no se incluye en GitHub por tamaño, aunque sí fue usado localmente.
- Algunas métricas dependen de la disponibilidad completa del dataset OULAD.
- El MDP es una abstracción simplificada de la replanificación real.
- El módulo bayesiano aproxima el riesgo operativo con variables discretizadas.
- El pipeline requiere ejecutar las celdas en orden para conservar dependencias como `risk_df`, `best_model`, tablas y KPIs.

---

## Trabajo futuro

- Integrar datos reales institucionales.
- Reemplazar el CSP artesanal por OR-Tools o CP-SAT.
- Agregar restricciones más realistas de docentes y aulas.
- Usar modelos probabilísticos más ricos.
- Incorporar aprendizaje por refuerzo para la replanificación.
- Convertir el notebook en una aplicación web o dashboard.
- Automatizar la carga de `studentVle.csv` desde almacenamiento externo.
- Mejorar la validación cruzada y el análisis de sesgo del modelo predictivo.

---

## Referencias base

- Russell, S., & Norvig, P. *Artificial Intelligence: A Modern Approach*. Pearson.
- Dechter, R. *Constraint Processing*. Morgan Kaufmann.
- Mackworth, A. K. *Consistency in Networks of Relations*. Artificial Intelligence.
- Schaerf, A. *A Survey of Automated Timetabling*. Artificial Intelligence Review.
- Babaei, H., Karimpour, J., & Hadidi, A. *A Survey of Approaches for University Course Timetabling Problem*. Computers & Industrial Engineering.
- Chen, M. C., Sze, S. N., Goh, S. L., Sabar, N. R., & Kendall, G. *A Survey of University Course Timetabling Problem: Perspectives, Trends and Opportunities*. IEEE Access.
- UniTime — Course Timetabling: https://help.unitime.org/course-timetabling
- AIMA — Artificial Intelligence: A Modern Approach: https://aima.cs.berkeley.edu/

La bibliografía formal completa se encuentra en:

[Referencias](informe/referencias.bib)


---

## Autores

- **Pablo Daniel Barillas Moreno**
- **Hugo Daniel Barillas Ajín**
- **Roberto Nájera**

---

## Licencia

Este proyecto se desarrolla con fines académicos para el curso **CC3045 — Inteligencia Artificial**.

La licencia del repositorio se encuentra en:

[Licencia](Licencia/LICENSE)