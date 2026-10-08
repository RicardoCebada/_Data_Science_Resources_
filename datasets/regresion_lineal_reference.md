# Guía Docente: Estructura Pedagógica del Conjunto de Datos para Regresión Lineal

Este documento describe la arquitectura y la intención pedagógica del conjunto de datos sintético (`regresion_lineal.csv`). Diseñado para cursos de econometría, estadística aplicada y ciencia de datos a nivel licenciatura, este dataset permite ilustrar de forma práctica la teoría del Modelo Lineal General (MLG), sus supuestos fundamentales, violaciones comunes y diagnóstico de modelos.

---

## 1. Ficha Técnica del Dataset

* **Número de observaciones ($n$):** 500 filas.
* **Variables predictoras ($X_i$):** Columnas **A**, **B**, **C**, **D**, **E** (cuantitativas continuas).
* **Variable respuesta ($Y$):** Columna **F** (cuantitativa continua).
* **Población subyacente:** Generada mediante simulación Monte Carlo con componentes controlados de varianza, correlación y no linealidad.

---

## 2. Desglose y Rol de cada Columna

┌──► A (Predictor Lineal Directo (+))
├──► B (Predictor Lineal Directo (-))


### Columna A — Predictor Principal ($X_1$)
* **Generación:** $A \sim \mathcal{U}(10, 100)$
* **Rol pedagógico:** Representa una variable explicativa con un efecto positivo fuerte sobre $F$.
* **Concepto a enseñar:** 
  * Interpretación básica de la pendiente ($\beta_1 = 3.5$): *"Por cada unidad que aumenta $A$, la variable $F$ incrementa en promedio 3.5 unidades, ceteris paribus"*.
  * Diagramas de dispersión e identificación visual de relaciones lineales.

### Columna B — Predictor Secundario ($X_2$)
* **Generación:** $B \sim \mathcal{N}(50, 10^2)$
* **Rol pedagógico:** Introduce el concepto de **regresión lineal múltiple** y el control de variables omitidas.
* **Concepto a enseñar:** 
  * Interpretación de coeficientes con signos opuestos ($\beta_2 = -2.1$).
  * Comparación entre regresión simple ($F \sim B$) y regresión múltiple ($F \sim A + B$).
  * Importancia de la condición *ceteris paribus*.

### Columna C — Multicolinealidad ($X_3$)
* **Generación:** $C = 0.85 A + \epsilon_C, \quad \epsilon_C \sim \mathcal{N}(0, 4^2)$
* **Rol pedagógico:** Presenta una correlación extremadamente alta con la Columna A ($r \approx 0.98$).
* **Concepto a enseñar:** 
  * **Multicolinealidad casi perfecta:** Inestabilidad en la estimación de los coeficientes por Mínimos Cuadrados Ordinarios (MCO).
  * Incremento en el Error Estándar de $\hat{\beta}_1$ y $\hat{\beta}_3$.
  * Diagnóstico mediante el Factor de Inflación de la Varianza ($\text{VIF} > 10$).

### Columna D — Violación de Linealidad ($X_4$)
* **Generación:** $D = \left(\frac{A}{15}\right)^2 + \epsilon_D, \quad \epsilon_D \sim \mathcal{N}(0, 3^2)$
* **Rol pedagógico:** Variable construida cuadráticamente respecto a $A$.
* **Concepto a enseñar:** 
  * Detección de patrones no lineales en el análisis de residuos ($e_i$ vs. $\hat{Y}_i$).
  * Transformación de variables (e.g., incluir términos polinómicos $A^2$ o aplicar transformaciones logarítmicas).

### Columna E — Variable Irrelevante ($X_5$)
* **Generación:** $E \sim \mathcal{U}(0, 100)$
* **Rol pedagógico:** Ruido blanco totalmente independiente de la variable respuesta $F$ ($\beta_5 = 0$).
* **Concepto a enseñar:** 
  * **Prueba de hipótesis individual:** Interpretar $p$-valores altos ($p > 0.05$) y contrastes de significancia $t$.
  * **$R^2$ vs. $R^2$ Ajustado:** Mostrar cómo el $R^2$ nunca disminuye al añadir variables irrelevantes, mientras que el $R^2$ ajustado penaliza la inclusión de $E$.

### Columna F — Variable Objetivo ($Y$)
* **Ecuación poblacional real:**
  $$F = 12.5 + 3.5 A - 2.1 B + \epsilon, \quad \epsilon \sim \mathcal{N}(0, 12^2)$$
* **Rol pedagógico:** Mide el comportamiento dependiente del sistema, incorporando un término de error estocástico $\epsilon$ homocedástico y normalmente distribuido.

---

## 3. Guía de Prácticas Didácticas Sugeridas

| Fase de Aprendizaje | Modelo / Análisis | Resultado Esperado para el Estudiante |
| :--- | :--- | :--- |
| **1. Exploración Basal** | $F = \beta_0 + \beta_1 A + \epsilon$ | Descubrir una relación lineal positiva significativa ($R^2 \approx 0.70-0.80$). |
| **2. Modelo Especificado Correctamente** | $F = \beta_0 + \beta_1 A + \beta_2 B + \epsilon$ | Recuperar los valores teóricos ($\hat{\beta}_0 \approx 12.5$, $\hat{\beta}_1 \approx 3.5$, $\hat{\beta}_2 \approx -2.1$) con p-valores $< 0.001$. |
| **3. Prueba de Inclusión de Variables** | Incorporar $E$ al modelo previo | Comprobar que $\hat{\beta}_E$ no es estadísticamente significativo y observar la penalización en el $R^2$ ajustado. |
| **4. Diagnóstico de Multicolinealidad** | Incorporar $C$ al modelo | Observar cómo los errores estándar de $A$ y $C$ se disparan y los p-valores pierden significancia a pesar de un $R^2$ global elevado. |
| **5. Análisis de Supuestos** | Gráfico de Residuos vs. Predictor $D$ | Identificar heterocedasticidad o no linealidad residual al relacionar el modelo con $D$. |

---

## 4. Matriz Teórica de Referencia

$$
\begin{pmatrix} 
\text{Variable} & \text{Coeficiente Teórico } (\beta) & \text{Estatus Teórico} \\ 
\text{Intercepto} & 12.5 & \text{Parámetro Base} \\ 
A & +3.5 & \text{Significativo } (p < 0.001) \\ 
B & -2.1 & \text{Significativo } (p < 0.001) \\ 
C & 0.0 & \text{Redundante (Multicolineal)} \\ 
D & 0.0 & \text{No Lineal (Efecto implícito de } A\text{)} \\ 
E & 0.0 & \text{No Significativo } (p > 0.05) 
\end{pmatrix}
$$