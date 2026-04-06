# Prepocesamiento y Transformación
Algunas tareas de estas etapas:
- Integracion de datos
- Limpieza de datos: completar valores faltantes, eliminacion de ruido, identificar o eliminar valores atipidocs y corregir incoherencias
- Reduccion de datos: reduccion de dimensionalidad, reduccion de numerosidad
- Transformacion de datos: normalizaciones, generacion de jerarquias conceptuales

En el proceso de descubrimiento del conocimiento hay varias etapas (las de arriba). En la limpieza de datos y en ingenieria de caracteristas se trabaja sobre las variables del dataset.

# Limpieza de datos
No se trata solo de sacar nulls, tambien de valores inconsistentes, incoherentes, errores de tipeo, y más... 

## Tipos de datos faltantes
Clasificacion de Rubin:
- **Missing completely at random (MCAR):** significa que la razon por la cual el dato no esta es completamente aleatoria, probablemente no se pueda predecir el valor a partir de otro valor en los datos.
  - La probabilidad de valores faltantes es la misma para todas las observaciones 
  - No hay relacion entre los datos faltantes y otros valores del dataset
  - No implica un sesgo omitir estas observaciones
- **Missing At Random (MAR):** los datos faltantes pueden ser explicados por valores en las otras columnas, pero no por la misma. Se puede encontrar una relación.
  - Aca si hay una explicacion de por que faltan
- **Missing Not At Random (MNAR):** probablemente la falta de ese dato no es al azar, hay que investigar la causa por la que falta ese valor.
  - Intermedio entre los otros dos
  - El faltante puede depender de otras columnas
  - Puede ser por azar pero puede ser otra la causa
 
## Estrategias para trabajar con datos faltantes
Hay que entender el motivo de la ausencia de datos para elegir la mejor estrategia

### Eliminar registros o variable
Si la eliminacion de un subconjunto disminuye significativamente la utilidad de los datos, la eliminacion del caso puede no ser efectiva. No es recomendable borrar en situaciones donde no sean MCAR.

### Imputación
**Imputar datos**: utilizar metodos de relleno de faltantes. Imputar datos: rellenarlos con estimaciones estadisticas de los valores ausentes.
  - El objetivo de imputar es producir un dataset completo para poder entrenar un modelo de aprendizaje automatico

#### Tecnicas de imputacion
Se dividen en:
  - **Univariada**: la imputacion se realiza de manera independiente para cada variable de entrada, sin considerar otras variables
  - **Multivariada**: el valor imputado para cada variable es en funcion de dos o mas variables

##### Tecnicas de imputacion **univariada**:
- Variables numericas: imputacion por promedio/mediana, valor arbitrario, "fin de cola" (end of tail) que tiene que ver con la distribucion de la variable
- Variables categoricas: imputacion por categoria frecuente, agregar categoria "FALTANTE"
- Ambas: analisis de caso completo, agregar indicador "FALTANTE", imputacion por muestreo aleatorio

La **sustitucion de casos** se reemplaza con valores no observados por lo que deberia ser realizada por un experto en esos datos.

En la **sustitucion por media o mediana** se reemplaza utilizando la media calculada con los valores presentes. Como desventaja es que la varianza estimada de la nueva variable no es valida porque esta atenuada por los valores repetidos, se distorsiona la distribucion, las correlaciones que se observen estaran deprimidas debido a la repeticion de un solo valor constante.

La **imputacion Cold Deck** selecciona valores o usa relaciones obtenidas de fuentes distintas de la base de datos actual. Es completar faltantes usando una base de datos adicional.

La **imputacion Hot Deck** trata de reemplazar faltantes con valores obtenidos de registros que son los mas similares. Hay que definir qué es similar o los K vecinos mas cercanos.

La **imputacion por regresión** se trata de reemplazar el dato que falta con el valor predecido por un modelo de regresión.

##### Tecnicas de imputacion **multivariada**:
**Multivariate Imputation by Chained Equations - MICE**:
- Trabaja bajo el supuesto de que los datos faltan al azar pero existe alguna relacion con los datos, o sea es **MAR**.
- Es un proceso de imputacion de datos.
- Es un proceso iterativo, eventualmente converge pero hay que encontrar esa condicion de convergencia.
- Se trata de una tecnica que simula valores plausibles basados en patrones reales de los datos.
- Cómo funciona?
    - Funciona tomando una columna que tiene datos faltantes, por ejemplo la edad, y utilizando las demás columnas del dataset, como ingreso o nivel educativo, para construir un modelo que permita predecir esos valores que faltan. Con ese modelo, reemplaza los datos faltantes de esa columna por valores más realistas que un simple promedio.
    - Una vez que termina con esa columna, pasa a otra que también tenga datos faltantes, por ejemplo ingreso. En este caso, usa todas las demás variables, incluyendo la edad que ya fue mejorada en el paso anterior, para construir un nuevo modelo y volver a estimar los valores faltantes de ingreso.
    - Este proceso se repite con cada columna que tenga datos faltantes, y luego vuelve a empezar el ciclo varias veces, refinando cada vez más las imputaciones a medida que las variables se van actualizando entre sí.

# Transformacion de datos - Feature Engineering
Esta etapa se trata de la modificacion de la forma de los datos. El **objetivo** principal de esta etapa es mejorar el rendimiento de los modelos creados mediante la transformacion de los datos que utilizan

Algunas tecnicas de transformacion son:
- Normalizacion
- Discretizacion
- Lograr normalidad
- Imaginacion (creacion de nuevas variables)

## Normalización
Se aplica sobre valores **numéricos**.

Se trata de estandarizar el dato a una escala, o sea mapear los datos en un rango mas pequeño.

A veces se quiere comparar features pero nos afectan las escalas en las que estan los datos

Se usa cuando las unidades de medida dificultan la comparación de features y cuando se quiere evitar que atributos con mayores magnitudes tengan pesos muy diferentes al resto.

### Normalización - Min-Max
Funciona al ver cuánto más grande es el valor actual del valor mínimo del feature y escala esta diferencia por el rango.

<img width="363" height="90" alt="image" src="https://github.com/user-attachments/assets/e35ae419-8a4a-49b6-b160-bf676eca8a95" />

Por ejemplo para edades que van de 0 a 90, si quiero normalizar la edad 15, queda `15-0`/`90-0`. Los valores quedan entre 0 y 1.

### Normalización - Z Score
Los valores de un atributo se normalizan a partir de su media y devio estandar.

<img width="324" height="89" alt="image" src="https://github.com/user-attachments/assets/6cc6f45f-d788-40d9-9431-ff1ea2dad554" />

Es útil cuando el verdadero mínimo y máximo del atributo no son conocidos, o cuando hay valores atípicos que dominan la normalización min-max.

### Normalización - Decimal scaling
Asegura que cada valor normalizado va a estar entre -1 y 1.

<img width="218" height="85" alt="image" src="https://github.com/user-attachments/assets/cfb7cce0-eb1e-4ab3-bf36-fae708287a08" />

Donde `d` es el numero de digitos que tiene la variable con el valor absoluto mas grande.

## Discretización
Es una técnica que permite dividir el rango de una variable continua en
intervalos.

Es llevar una variable continua a intervalos discretos.

Se reducen los valores de una variable contínua a un número reducido de
etiquetas.

### Discretización - Binning
Se divide a la variable en x cantidad de **bins**.

Algunso criterios de agrupamiento son:
- Igual-Frecuencia: La misma cantidad de observaciones en un bin
- Igual-Ancho: Definimos rangos o intervalos de clases para cada bin
- Cuantiles: Separar en intervalos utilizando Mediana, Cuantiles,
Percentiles.

<img width="615" height="167" alt="image" src="https://github.com/user-attachments/assets/5108a0f1-995d-4bdb-8deb-49b52edea0e9" />

## Variables Dummies – One Hot Encoding
Algunos métodos analiticos requieren que las variables _predictoras_ sean numéricas. Cuando se tienen **categóricos** se puede recodificar la variable categorica en una o mas variables dummies.

Son variables numéricas binarias que toman valores 1 o 0 para indicar la presencia o ausencia de una característica cualitativa.

# Análisis de valores atípicos

## Outliers
**Outliers:** “Un outlier es una observación que se desvía tanto de las otras observaciones como para despertar sospechas que fue generado por un mecanismo diferente”

Un outlier es subjetivo al problema, porque pueden tener sentido aunque sean valores distantes al resto de datos.

Se pueden deber a un error de medición, aleatoriedad, que esa instancia sea de una familia distinta a la del resto, etc.

> ¿Es necesario elimina outliers?
- Se los debe detectar porque su presencia puede influenciar los resultados en un análisis estadístico, pero no necesariamente es un dato que se debe descartar.
- Se los debe inspeccionar para saber como tratarlos.
- Pueden estar alertando anomalías, y a veces nos interesa encontrarlos para:
    - Detección de fraudes
    - Detección de fallas
    - Patologías médicas
 
## Tipos de outliers
<img width="629" height="253" alt="image" src="https://github.com/user-attachments/assets/3c796b8f-e439-4268-ade6-526828a973c5" />

### Univariados
Son valores atípicos que se pueden encontrar en una simple variable. Son buenos para la detección de extremos pero no en otros casos.

#### Métodos univariados
* **Z-Score:** es una métrica que indica cuántas desviaciones estándar tiene una observacion de la media muestral, asumiendo distribucion gaussiana. <br>
Se fija un umbral para calcular el z-score. Como _regla de oro_  todos los valores Z que en módulo sean mayor a 3 son potencialmente outliers: `|Z| > 3`.

<img width="204" height="92" alt="image" src="https://github.com/user-attachments/assets/a1aca55f-bf44-48df-9843-a4e8f0939734" />

* **Z-Score Modificado:** La media y desviacion estandar de la muestra pueden verse afectados por los valores extremos en los datos.<br>

En vez de usar la media se usa la mediana. Si tengo que calcular un promedio, con un outlier se puede ver distorsionado, por eso se busca usar algo mas robusto: la mediana de cada variable. Surgió una nueva variable que es el valor menos la mediana, a esos valores nuevos les tomo la mediana y obtengo $$MAD=median{|x_{i}-\hat{x}|}$$.

Luego, para calcular la transformación, a cada observación le resto la mediana, se la divide por el $$MAD$$, y se la normaliza multiplicando por $$0.6745$$, ese valor para que sea comparable con la desviacion estandar.

Agarro la variable, calculo me mediana a cada observación le resto la mediana

Como _regla de oro_, valores en modulo mayores a `3.5` son considerados outliers.

* **Análisis de Box-Plot**: Los Box-Plots permiten visualizar valores extremos univariados.

Las estadisticas de una distribucion univariada se resumen en terminos de cinco cantidades:
- Minimo/maximo (bigotes)
- Primer y tercer cuantil (caja)
- Mediana (linea media de la caja)
- IQR (rango intercuartil) = Q3+Q1

Generalmente:
* $$\pm 1.5 \cdot IQR$$: outliers moderados
* $$\pm 3 \cdot IQR$$: outliers severos

### Multivariados
Los valores atipicos multivariados se pueden encontrar en un espacio n-dimensional. 

#### Métodos multivariados
