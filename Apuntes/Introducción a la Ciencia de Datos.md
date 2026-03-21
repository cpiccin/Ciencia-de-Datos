# Introducción a la Ciencia de Datos

Si la variable dependiente es **cualitativa** (o sea aquellas que están acotadas, que son categóricas), entonces hay un problema de **clasificación**.

**Que es un problema de clasificación?:** tengo un modelo de datos, entreno un modelo y el modelo me dice  “este registro es categoría 1, o categoría 2 o categoría 3….”

Si la variable dependiente es **cuantitativa** (o sea es un valor en un rango continuo), el problema es de **regresión**. 
- Ej: predicción del precio de una propiedad o de la temperatura

No se hace una clasificación sino una predicción	

Si no hay variables dependientes, el problema es de **agrupamiento** (o de clustering). Este modelo es una técnica de aprendizaje no supervisado; el modelo agrupa datos sin saber de antemano qué buscar.

<img width="695" height="513" alt="image" src="https://github.com/user-attachments/assets/91d27b10-f823-4c11-a190-f576657e8f47" />

Pueden haber valores atipicos: **outliers**

<img width="419" height="230" alt="Screenshot 2026-03-19 at 10 54 13 AM" src="https://github.com/user-attachments/assets/8d45a95d-a788-4f85-879c-3c0ded5c5af2" />


## Correlación de variables
Dos variables están correlacionadas cuando si una varia la otra también. 

La correlación puede ser 
	•	Positiva (si una crece, la otra también, si una decrece, la otra también)
	•	Negativa (si una crece, la otra decrece, y viceversa)
	•	Sin correlación 

<img width="325" height="297" alt="Screenshot 2026-03-19 at 10 57 40 AM" src="https://github.com/user-attachments/assets/cc40fe8c-de5d-4d0f-a7a7-5703cb20a4c8" />

La correlación no implica **causalidad**. Que haya correlación no significa que una sea la causa de la otra. 

Es difícil encontrar y probar una relación de causalidad. 

Las correlaciones pueden suceder por otros motivos como la incidencia de una tercer variable que empuja a ambas o puro azar.

### Varianza 
Calculo de la diferencia de cada observación respecto de su media.

Se suman todas las diferencias y se dividen por n-1

<img width="526" height="304" alt="Screenshot 2026-03-19 at 11 03 51 AM" src="https://github.com/user-attachments/assets/ea224531-a715-4719-8a9c-732d2ad492dd" />

Se va a estimar la varianza porque no tenemos a la totalidad de la población. Estimamos a partir de una **muestra**.

<img width="761" height="304" alt="Screenshot 2026-03-19 at 11 05 28 AM" src="https://github.com/user-attachments/assets/1127be28-d335-4bd2-957f-38fa6d20ef7e" />


### Covarianza
La covarianza es un valor que indica el **grado de variación conjunta** de dos variables aleatorias respecto de sus **medias**.

Una covarianza alta dice que los datos de la muestra están muy dispersos respecto de su media. Si es baja, los datos están cerca de su media.

Es el dato que dice si existe una dependencia entre ambas variables.

La covarianza mide la **correlación LINEAL**
￼
<img width="1184" height="512" alt="image" src="https://github.com/user-attachments/assets/59a212eb-069f-441e-93e0-f9ae9db528a5" />


### Correlación de Pearson
Para dos variables se puede medir su **correlación lineal** con el coeficiente de correlación `r`. Este coeficiente es una función que mide cuan relacionadas están las dos variables de forma lineal.
	•	Si da 0 **NO existe correlación**
	•	Si da 1 están **relacionadas linealmente de forma perfecta** (todos los puntos estan en una linea)
	•	Si da -1 existe una **correlación negativa perfecta**

<img width="523" height="252" alt="1__#$!@%!#__1__#$!@%!#__Pasted Graphic" src="https://github.com/user-attachments/assets/f1d11f74-7620-40ad-b241-12b4679b177d" />

￼
### Desvio estandar
Es una medida que se utiliza para cuantificar la variación o la dispersión de un conjunto de datos numéricos. 
	•	Si la desviación estándar es **baja**, la mayor parte de los datos de la muestra tienden a estar agrupados cerca de su media
	•	Si la desviación estándar es **alta**, indica que los datos se distribuyen en un rango de valores mas amplio.


## Regresión
Regresion: **predecir** un valor en un rango continuo, para ciertos valores de entrada.

<img width="566" height="274" alt="1__#$!@%!#__1__#$!@%!#__Pasted Graphic 1" src="https://github.com/user-attachments/assets/bec0bdf9-ced5-4591-988c-adbef0c469c8" />

<img width="639" height="207" alt="1__#$!@%!#__Pasted Graphic 2" src="https://github.com/user-attachments/assets/30b1b33c-04f2-4456-ada5-2bccb2019afb" />

￼
Aumenta el tiempo computacional entonces surge una mejor forma: **descenso por gradiente**.

<img width="466" height="303" alt="1__#$!@%!#__Pasted Graphic 3" src="https://github.com/user-attachments/assets/dabf278f-09b1-46f8-b5f4-63d49afe061d" />


Se va a saber que tan buena es la predicción a partir del calculo del error.

## Métodos de Clasificación
Buscamos una categoría **c** de un conjunto **C** de categorías posibles, conocidas de antemano.

### Regresión Logistica
Es el modelo mas sencillo para clasificar.

Dado una serie de puntos quiero encontrar una función que separe los puntos en dos conjuntos. Una vez que se encontró puedo determinar para cualquier valor X futuro, el conjunto al cual pertenecerá	

Encontrar los parámetros óptimos para esta curva consiste en construir un estimador de regresión logística


# Metodos de clusterizacion
**Clustering**: en este tipo de problemas se trata de agrupar los datos. Agruparlos de tal forma que queden N conjuntos distinguibles, aunque no necesariamente se sepa que signifiquen esos conjuntos.
	•	El agrupamiento siempre será por características similares

## K-Means
Modelo mas simple de todos
1. El usuario decide la K cantidad de grupos
2. K-Means elige al azar K centroides
3. Decide que grupos están mas cerca de cada centroide. Esos puntos forman un grupo
4. K-Means recalcula los centroides al centro de cada grupo
5. K-Means vuelve a reasignar los puntos usando los nuevos centroides. Calcula nuevos grupos
6. K-Means repite punto 4 y 5 hasta que los puntos no cambian de grupo

### Como saber cuantos conjuntos K elegir?

### Regla del codo (Elbow Method)
 - Elegimos un rango, ejemplo 1 a 10, y para cada valor
 - Para cada centroide calcular la distancia promedio: si hay una caída abrupta hay un “codo” y entonces ese punto debe ser la cantidad de clusters￼

<img width="438" height="195" alt="Pasted Graphic 4" src="https://github.com/user-attachments/assets/ac367402-311e-4e38-bbf4-c01c8d4db33f" />


### Metodo Silhouette 
- Elegimos un rango, ejemplo 1 a 10, y para cada valor
- Para cada valor de K graficamos la silhouette
- El mejor valor posible es silhouette = 1, y el peor silhouette = -1

#### Coeficiente de Silhouette
Cada punto en el conjunto de datos tiene un coeficiente de Silhouette. Para calcularlo necesitamos calcular 

`a(i)`: distancia promedio del punto i a cada uno de los puntos de su cluster 
￼
<img width="311" height="185" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/33d03c02-9f9c-4e31-8407-3d4dff3bb180" />

`b(i)`: distancia promedio del punto i a cada uno de los puntos del cluster mas cercano a su propio cluster
￼
<img width="238" height="184" alt="Pasted Graphic 6" src="https://github.com/user-attachments/assets/c8bb6d16-c8ee-41bf-98de-b9f001275e75" />

- Idealmente a(i) << b(i)
￼

**Consideraciones:**
	•	Si a(i) > b(i), i esta posiblemente mal clasificado.

El coeficiente de Silhouette se calcula como

<img width="192" height="59" alt="Pasted Graphic 8" src="https://github.com/user-attachments/assets/7ac5b193-8775-44bb-8948-a53b78b134f8" />

Donde en el peor de los casos s(i) = -1

<img width="453" height="240" alt="Pasted Graphic 9" src="https://github.com/user-attachments/assets/b4e3eed1-526f-4c22-8fbf-18cbffdd2e93" />


Para buscar **el mejor K** podemos hacer el plot para distintos K, es mejor cuantos menos outliers haya.
￼
<img width="414" height="212" alt="Pasted Graphic 10" src="https://github.com/user-attachments/assets/e61d761f-8fc2-4f43-9ed8-35d4844ec73f" />


## Estadística de Hopkins
A veces las muestras son uniformes o sea no hay tendencia al cluster (no se pueden diferenciar clusters).

La idea es comparar una muestra cualquiera con una muestra uniforme (creada de forma aleatoria) y observar como se distribuyen los ejemplos (los puntos) en dicho espacio

Sea D un conjunto de nuestros datos reales:
1. Tomar una muestra uniformemente de n puntos (p1, …, pn) de D.
2. Calcular la distancia xi, de cada punto real a cada vecino mas cercano.
  - Para cada punto pi ∈ D, encuentre su vecino mas cercano pj
  - Calculamos la distancia entre pi y pj y la llamamos xi=dist(pi, pj)
3. Generar un conjunto de datos simulados (randomD) extraído de una distribución uniforme aleatoria con n puntos (q1, …, qn) y la misma variación que el conjunto de datos reales original D.
4. Calcular la distancia yi desde cada punto artificial hasta el punto de datos real mas cercano.
  - Para cada punto qi ∈ randomD, encontrar su vecino mas cercano qj ∈ D.
  - Calcular la distancia entre qi y qj y llamarla yi=dist(qi, qj)
5. Calcular la estadística de Hopkins (H) como: la distancia media del vecino mas cercano en el conjunto de datos aleatorios dividida por la suma de las distancias medias del vecino	mas cercano en el conjunto de datos real y simulado.

<img width="186" height="119" alt="Pasted Graphic 11" src="https://github.com/user-attachments/assets/db6bb7d2-6f3d-4472-a051-9f6687b883a8" />
￼
### Interpretación 
	•	Si D está distribuida de forma uniforme, entonces ∑ xi y ∑ yi   serían muy parecidos, entonces H sería aproximadamente ½ (0.5). Significa que posiblemente mi conjunto original tiene una distribución uniforme y no hay una tendencia al cluster
	•	Pero si hay clústeres en D, las distancias de los puntos artificiales ∑ yi serían mucho más grandes que las distancias de los puntos reales:  ∑ xi y por lo tanto H sería mayor que 0.5. 
	•	Un valor de H superior a 0,75 indica una tendencia a la agrupación en un nivel de confianza del 90 %.

### Hipotesis que maneja Hopkins
 - **Hipotesis nula**: el conjunto de datos D se distribuye uniformemente (o sea no hay clusters significativos)
	•	**Hipótesis alternativa**: el conjunto de datos D no esta uniformemente distribuido (contiene clusters significativos)

Podemos realizar la prueba de la estadística de Hopkins de forma iterativa, utilizando 0,5 como umbral para rechazar la hipótesis alternativa. 
	•	Es decir, si H < 0,5, es poco probable que D tenga conglomerados estadísticamente significativos.
	•	O si el valor de la estadística de Hopkins es cercano a 1, entonces podemos rechazar la hipótesis nula y concluir que el conjunto de datos D es significativamente un dato agrupable.

## Entrenamiento
**Como se entrenan estos modelos?**

Se va a usar una parte de los datos para entrenar el modelo y la otra para testearlo.

A su vez se puede partir el conjunto de entrenamiento en 5 y usar una partecita para testear que se va a ir moviendo a lo largo de 5 iteraciones para ver que manera el modelo quedo mejor entrenado.
￼
<img width="452" height="182" alt="Pasted Graphic 12" src="https://github.com/user-attachments/assets/9fa00aca-398a-460c-8e12-b904ea1bbe91" />

<img width="400" height="184" alt="Pasted Graphic 13" src="https://github.com/user-attachments/assets/e586c3d2-c5eb-4db2-a752-584e326797e8" />


### Conjuntos balanceados
Muchas veces el conjunto de datos no están balanceados y eso perjudica el entrenamiento. Se puede sacar muestras del grupo grande o agregar del grupo chico.

<img width="589" height="177" alt="Pasted Graphic 14" src="https://github.com/user-attachments/assets/b1d11929-4733-4b32-85ae-3d1f6c5e349e" />

### Overfitting
Overfitting es que el modelo aprendió de mas, pero esto significa que se aprendió de memoria los valores de entrenamiento entonces va a ser malo en el caso real.

**Buscamos la generalización, no la memorización.**

<img width="394" height="146" alt="Pasted Graphic 15" src="https://github.com/user-attachments/assets/57969c46-0240-4492-a901-30f3812e3282" />
