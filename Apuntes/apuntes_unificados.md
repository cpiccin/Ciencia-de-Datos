# Arboles

## ID3: Iterative Dichotomiser 3
De Ross Quinlan, el algoritmo genera un arbol de decision a partir de un conjunto de ejemplos.

Arbol de decision = grafo aciclico, con una raiz (nodo principal), nodos terminales (donde termina el flujo, las hojas), el resto de nodos representan "preguntas", las aristas son posibles "respuestas"

### Entropia (de la informacion)
La medida del desorden o la medida de la pureza. Basicamente, es la medida de la impureza o aleatoriedad en los datos.

Para calcula la entropia de **n** clases se utiliza la formula
![WhatsApp Image 2026-04-09 at 17 54 06](https://github.com/user-attachments/assets/d65a9fe2-e160-4ef1-84cf-2935d78ad22f)

Si la muestra es homogenia (todos los casos son iguales), la entropia es 0.

La maxima entropia viene dada por log2(n) donde n son los posibles valores de salida. Si n=2 (true o false) entonces, la maxima entropia es 1. **Es la maxima incertidumbre**.

### Ganancia de la informacion
Es una forma de cuantificar que elemento en un conjunto me esta dando mas informacion.
- Que caracteristica de un conjunto de datos dados proporciona la maxima informacion sobre la clasificacion.
- **Si tuviesemos que elegir una sola caracteristica para clasificar, cual seria?**

<img width="1280" height="406" alt="image" src="https://github.com/user-attachments/assets/31e10dde-2c03-4d70-ae8c-43fc8160443f" />

### ID3: Algoritmo basico
1. Calcular entropia para todas las clases
2. Calcular entropia para cada valor posible de cada atributo
3. Seleccionar el mejor atributo basado en la **reduccion** de la entropia. Usando el calculo de ganancia de la informacion
4. Iterar para cada sub-nodo. Excluyendo el nodo raiz que ya fue usado

O sea...
- Entropía alta -> todo mezclado (mucha incertidumbre)
- Entropía baja -> casi todo igual (poca incertidumbre)
- Ganancia de información -> cuánto baja la entropía al hacer una pregunta

ID3 elige la pregunta que más reduce el desorden

> ¿Por qué una categoría clasifica mejor que otra?
- Porque divide los datos de forma más ordenada.

Una mala pregunta -> deja grupos mezclados<br>
Una buena pregunta -> deja grupos casi puros

La “mejor” categoria es la que más reduce la incertidumbre (máxima ganancia de información)

La impureza de Gini mide qué tan probable es equivocarte si clasificás al azar usando las proporciones de ese nodo.

## Impureza de Gini
Es otra forma de medir qué tan mezclados están los datos en un nodo de un árbol de decisión.

Algunas implementaciones de arboles de decision utilizan la impureza de gini en vez de la ganancia de informacion, porque es mas facil de calcular (menos costosa computacionalmente)

<img width="1280" height="648" alt="image" src="https://github.com/user-attachments/assets/d6d51729-2fbd-40f6-af06-9cc060029c26" />

Como funciona Gini?
<img width="1280" height="618" alt="image" src="https://github.com/user-attachments/assets/2174d4f1-d441-400b-a503-652d32c31d02" />

Se obtiene la impureza para cada nodo, y luego la impureza de gini total, para el nodo raiz, que sera un promedio ponderado

<img width="1280" height="656" alt="image" src="https://github.com/user-attachments/assets/94b54e16-f4e0-4cf5-b0ab-79460fffa1b6" />

Se hace lo  mismo para los otros 2 y el de menor valor va a tener la menor impureza, o sea que tendra las mejores chances para clasificar si se lo coloca como nodo raiz.


## C4.5
Son mejoras a ID3:
- **Soporta campos numericos y rangos continuos**:
  -  El algoritmo puede crear dinamicamente un campo **booleano** tal que si A<Ac = true, sino Ac = False.
  -  Se va a encontrar ese umbral c de forma que quede con la mayor ganancia de informacion
  -  Se ordena A por ej de menor a mayor; luego se identifican los valores adyacentes (de la clase que es nuestra salida); detectamos cuando hay un cambio de valor de salida, entonces en esos limites seguramente estan nuestros Ci candidatos; nos quedamos con el Ci que de el mejor resultado.
- **Contempla la posibilidad de usar datos faltantes**:
  - Los atributos faltantes los marca con un ? y no se usan para el calculo de la ganancia o la entropia  
- **Se agrego un metodo adicional de poda que evita el overfitting**:
  - Se genera el arbol y luego se analiza de forma recursiva, desde las hojas que preguntas (nodos interiores) se pueden eliminar sin que se incremente el error de clasificacion.
  - Mas a detalle...
      1. Se elimina un nodo interior cuyos sucesores son todos nodos hoja
      2. Se vuelve a calcular el error que se comete con este nuevo arbol sobre el conjunto de test
      3. Si este error es menor que el error anterior, entonces se elimina el nodo y todos sus sucesores (hojas)
      4. Se repite 



## Random Forest
Es un **ensamble**.

> Muchos estimadores mediocres, promediados, pueden ser muy buenos estimadores

### Bootstrap aggregating:
Los arboles no soportan ser entrenados con muchisimos datos, empiezan a fallar, entonces lo que podemos hacer es partir el conjunto grande en conjuntos mas chicos.

Y en general n' < n. Siendo n' aproximadamente un 2/3 de n.

Se generaran m subtablas tomando solo algunas filas, de forma aleatoria. 

**Se usa la tecnica de Attribute bagging (o random subspace):** para cada una de las m tablas, escogemos sólo algunos atributos (COLUMNAS) de forma aleatoria también.

¿Cuántas nos quedamos? => Con RAÍZ CUADRADA del número de atributos.
- Si hay 8 atributos, tomamos Round(√8) = 3

(Esto es recomendable sobre todo cuando hay muchas columnas)

Ahora para cada tabla, entrenamos un arbol, y para cada arbol calculamos su matriz de confusion (true negative, false positive, etc). Esto para poder ver el desempeño del arbol.

Calculamos la **tasa de error** de cada arbol:
- `(FP + FN) / TOTAL = Tasa de error` 
- Nos quedamos con el mejor arbol de los m y repetimos el proceso K veces: K veces generamos m subconjuntos, entrenamos m arboles y nos quedamos con el mejor.
- Al final nos van a quedar K arboles.















\newpage

# Ensamble de modelos
Vamos a crear ensambles, coleccion de modelos donde cada uno es un estimador mediocre de juntos son buenos.

Se trata de entrenar varios modelos, cada uno sobre datos distintos, donde cada modelo sobre-ajusta de manera diferente:
- Cada modelo tendra bajo sesgo, baja varianza.

<img width="623" height="245" alt="image" src="https://github.com/user-attachments/assets/283ec773-0d7d-4185-976e-0e353a897979" />

Luego de que cada modelo hizo una estimacion, **se vota**, osea para una nueva instancia, se clasifica con todos los modelos y se devuelve la clase mas elegida.

## Sesgo (Bias) vs Variance (Varianza)
Son dos tipos de errores que se pueden cometer:

- **Bias:** es la diferencia entre el valor esperado del estimador (la prediccion media del modelo) y el valor real. Cuando un modelo tiene un **bias muy alto** significa que **el modelo es muy simple y no se ajusto a los datos de entrenamiento** (underfitting: no esta lo suficienmente entrenado), por lo que produce un error alto en todas las muestras.

- **Variance:** es cuanto varia la prediccion segun los datos que utilicemos para el entrenamiento. **Varianza baja** indica que **cambiar los datos de entrenamiento produce cambios pequeños en la estimacion**. En cambio **varianza alta** quiere decir que **pequeños cambios en el dataset conlleva a grandes cambios en el output** (overfitting: modelo sobre entrenado).

<img width="589" height="158" alt="image" src="https://github.com/user-attachments/assets/95226441-a4a8-40fa-ad9c-593ae146695d" />

La votacion reduce la varianza de la clasificacion

## Tecnicas para hacer ensambles

### Bagging

<img width="679" height="375" alt="image" src="https://github.com/user-attachments/assets/2fe200f5-4a92-4ad3-93be-69f5c3e14606" />

Existe la tecnica **bootstrap** que se utiliza junto a bagging, se trata de muestras aleatorias con reemplazo para entrenar distintos modelos y luego combinarlos.

**Paso a paso...**
1. Se divide el conjunto en subconjuntos, obteniendo como resultado diferentes muestras aleatorias
    - Las muestras tienen la misma cantidad de individuos (es uniforme)
    - Los individuos pueden repetirse en el mismo conjunto de datos (son muestras con reemplazo)
  
2.  Entrenamos un modelo para cada subconjunto
3.  Construimos un unico modelo predictivo a partir de los anteriores

**Caracteristicas...**
- Reduce la varianza en el modelo final
- Efectivo en conjuntos de datos con varianza alta
- Puede reducir el overfitting
- Puede reducir el ruido de los outliers
- Pueden mejorar levemente con el voto ponderado


**Problemas al usarlo con arboles...**
- Si pocos atributos son predictores fuertes, todos los arboles se van a parecer entre si!!!
- Estos atributos terminaran cerca de la raiz, para todos los conjuntos generados con bootstrap

### Boosting
Aca entrenamos un modelo de forma secuencial: modelo2 despues de modelo1

Busca nuevos modelos para las instancias mal clasificadas por los anteriores.

<img width="486" height="185" alt="image" src="https://github.com/user-attachments/assets/363ce3d5-0029-4a49-a338-a6d120dd2215" />

**Paso a paso...**
1. Comenzar con un modelo simple entrenando sobre todos los datos $$h_{0}$$.
2. En cada iteracion $$i$$, entrenar $$h_{i}$$, dando mayor importancia a los datos mal clasificados por las iteraciones anteriores.
3. Terminar al conseguir cierto cubrimiento, o luego de un numero de iteraciones.
4. Clasificar nuevas instancias usando una votacion ponderada de todos los modelos construidos

<img width="672" height="280" alt="image" src="https://github.com/user-attachments/assets/3514dfa8-95bd-4b77-ad30-79a6fd543a3f" /> 

<img width="658" height="232" alt="image" src="https://github.com/user-attachments/assets/43baeb9a-98cb-45a0-89f3-84ea230b759a" />

Boosting necesita pesos por lo que se debe adaptar algoritmos de aprendizaje y tomar muestras con reemplazo segun pesos.

**Modelos exitosos:**
- AdaBoost
- Gradient Boosting
- XGBoost: eXtreme Gradient Boosting:
  -  Comparando con Gradient Boosting, la velocidad de entrenamiento de XGBoost es mucho menor gracias a su implementacion y a estar mejor orientado al uso eficiente del hardware (GPU)
  -  El _accuracy_ tambien es mejor debido a que XGBoost maneja mejor el overfitting mediante regularizaciones

### XGBoost
Fue pensado para trabajar con Big Data y se puede usar para regresion o clasificacion.

**Paso a paso de un ejemplo:**

<img width="566" height="251" alt="image" src="https://github.com/user-attachments/assets/3d479465-67f5-4e58-a23d-06c9e5187713" />

1. Hacer una prediccion inicial, por defecto se toma 0.5. Se deben calcular los residuos a partir de esta prediccion inicial

<img width="661" height="247" alt="image" src="https://github.com/user-attachments/assets/44c7f2b7-1b23-4a33-a166-b8ee82dd8c0f" />

2. Construir un arbol para los residuos. El arbol es distinto al de Gradient Boost. No construyo el arbol para calcular la efectividad de la droga, sino para calcular el error que se esta cometiendo, para calcular los residuos. Se crea un nodo hoja donde se ponen todos los residuos
  
<img width="558" height="239" alt="image" src="https://github.com/user-attachments/assets/8926d045-f83d-4027-ad2d-a4339f1505f3" />

3. Calcular el **Similarity Score** para los residuos. La formula es... $$\text{Similarity Score} = \frac{(\text{Suma de residuos})^2}{\text{Cantidad de residuos} + \lambda }$$ donde $$\lambda$$ es un parametro de **regularizacion**
    - Regularizacion: parametro que se agrega a ciertos modelos para que no hagan overfitting o sea que cuando un modelo aprende inclusive el ruido de los datos, funciona muy bien con esos datos pero falla con datos nuevos. La regularizacion penaliza la complejidad del modelo introduciendo un pequeño sesgo o bias.
  
<img width="282" height="93" alt="image" src="https://github.com/user-attachments/assets/f922cab5-891d-484e-9d1a-d94c6b169687" />

4. Hay que ver cual va a ser el siguiente nodo, hay que calcular la ganancia total segun elijamos una opcion u otra para partir el arbol. O sea que hay que encontrar donde vamos a poner un umbral para la dosis de la droga.

<img width="651" height="244" alt="image" src="https://github.com/user-attachments/assets/7f40979e-f633-43bf-a578-ca8afe9a3625" />

<img width="669" height="361" alt="image" src="https://github.com/user-attachments/assets/7ad07388-fa37-4510-834d-3fe6e684161f" />

Se deben explorar todas las opciones, calculando la ganancia para cada uno

<img width="657" height="372" alt="image" src="https://github.com/user-attachments/assets/e6838cdf-9c69-4610-8dd4-d9c6b4801d2c" />

Se concluye que usar el umbral $$\text{Dosage}<15$$ es el que mejor divide los residuos del arbol, me quedo con ese arbol.

5. Repetimos el paso anterior hasta alcanzar la profundidad del arbol estipulada

<img width="675" height="358" alt="image" src="https://github.com/user-attachments/assets/508d15be-ecc6-4f24-b1c0-707949fc7801" />

  - Primera opcion:
<img width="388" height="179" alt="image" src="https://github.com/user-attachments/assets/00932e7e-8113-4a23-b005-e68acc673774" />

  - Segunda opcion:
<img width="432" height="155" alt="image" src="https://github.com/user-attachments/assets/c7f19c78-ed40-470f-8139-12e18d6a6e37" />

Nos quedamos con el que tiene la mayor ganancia, o sea la segunda opcion

<img width="342" height="203" alt="image" src="https://github.com/user-attachments/assets/8c48aa5e-597f-414e-a5f4-7209e5b94f9a" />

Terminamos aca porque pusimos un limite de 2 niveles en el arbol (XGBoost por defecto usa 6 niveles)

6. Poda: elegimos un numero $$\gamma = 130$$ que es al azar y calculamos con la ganancia del ultimo nodo $$\text{Gain} - \gamma = 140.17 - 130 = 10.17$$. Si la diferencia es menor a cero, hay que remover el nodo, sino se queda. En el ejemplo se queda.
    - Si se elegia otro valor mas alto por ej 150, continua la poda con es $$\gamma$$ y se borra el nodo, y se sigue podando... $$\text{Gain} - \gamma = 120.33 - 150 = -29.67$$. En el ejemplo queda la estimacion inicial.
    - Por defecto $$\gamma = 0$$, cuanto mas alto, mas conservador el algoritmo.
  
7. Volvemos a calcular el arbol (repite el **paso 3**), solo que ahora $$\lambda = 1$$ al calcular el Similarity Score. Los similarity scores quedan mas chicos. Tambien las ganancias. Pero como $$\gamma = 130$$ se poda todo el arbol.

<img width="316" height="137" alt="image" src="https://github.com/user-attachments/assets/3de9e920-bb52-46b0-9ce9-f969a5f58fff" />

<img width="386" height="153" alt="image" src="https://github.com/user-attachments/assets/59358a01-952f-4f81-81ae-85e5cae696c9" />

8. Se vuelve a podar (se repite el **paso 5**)


Cuando $$\lambda > 0$$ es mas probable tener que podar un arbol porque se reduce el Gain. $$\lambda = 1$$ previene el sobreentrenamiento del modelo en el conjunto de entrenamiento.


**Ahora... Como se calcula la respuesta?**

<img width="653" height="344" alt="image" src="https://github.com/user-attachments/assets/0c5716f5-f0a9-4ae0-bac7-995e45967cbb" />

**Output = -2.65**

El error de la prediccion original (0.5) era de -10.5, ahora tenemos una nueva prediccion (-2.65) con un residuo de -7.35, o sea que nos acercamos de a poco al valor real


**Nuevo paso**: calculamos todos los residuos utilizando el arbol hasta aca. Con estos nuevos residuos contruimos un nuevo arbol, desde el **paso 2**. Con el nuevo arbol se calcula la salida de cada elemento y luego los residuos. Se construye otro arbol. Se sigue hasta que los residuos son practicamente cero o bien alcanzamos el numero maximo de arboles predefinido. Entonces, su estructura final resulta: 

<img width="665" height="275" alt="image" src="https://github.com/user-attachments/assets/93d028e8-e192-4248-9296-c94be07597fd" />



\newpage


AdaBoost y Gradient Boost quedaron obsoletos.
- Ensambles (modelos construidos con muchos modelos) que usan la tecnica de boosting: cada modelo se entrena para corregir los errores del anterior.

## AdaBoost
1. Combina un montn de **weak learners** (estimadores pobres) para hacer clasificaciones. Los weak learners son _stumps_ o sea arboles que solo tienen raiz y dos hojas.
2. Algunos stumps tienen mas pesos que otros en la votacion final, tienen mas que decir: **amount of say**.
3. Cada uno de los stumps esta construido teniendo en cuenta los errores del stump anterior
    - Lo podemos hacer con una funcion de impureza de Gini ponderada, para cada ejemplo
    - O simplemente regenerando los datos   


# Ensambles Híbridos
Voting, Stacking y Cascading

Ensambles hibridos combinan clasificadores de distinto tipo, mientras que los ensambles homogeneos es siempre el mismo modelo.

A estos modelos combinados les podemos aplicar las tecnicas
- Voting
- Stacking
- Cascading

## Voting
Construir N modelos utilizando los mismos datos y luego tomar la prediccion mayoritaria

  <img width="738" height="258" alt="image" src="https://github.com/user-attachments/assets/9ba18731-f65e-42a1-b9af-b165c9ddec2f" />

## Stacking
Es una mejora a Voting.

Se trata de entrenar diferentes modelos (modelos base) y un modelo mas, que decide, dada una instancia nueva, que modelo usar.

Concepto de **meta-aprendizaje** para reemplazar el macanismo de voto.

Pueden apilarse tantas capas de modelos como se desee

A diferencia de voting, la salida es procesada por un meta-modelo, un modelo adicional.

<img width="746" height="312" alt="image" src="https://github.com/user-attachments/assets/47b7f13d-7599-4254-a946-411574bf6452" />

- Los modelos para meta-aprendizaje suelen ser arboles, NB, SVM o Percenptron (redes neuronales)
- Para los otros modelos se puede usar cualquiera
- Menos popular que boosting, bagging porque es mas dificil para el analisis teorico.

## Cascading
Se trata de una arquitectura diferente.
- Enfoque en el que se passa sucesivamente los datos de un modelo a otro.
- A diferencia de stacking, cada "capa" tiene un solo modelo.
- El input de cada modelo son las instancias.
- Suele utilizarse cuando se necesita una alta certeza en la prediccion.

Se va refinando el input a partir de sucesivos pasos.

### Entrenamiento
Se entrenan N modelos distintos utilizando el set de entrenamiento.

Cada modelo se entrena sobre instancias predichas con baja certeza del modelo anterior

Suele hacerse de forma tal de empezar por modelos simples y a medida que se entrenan nuevos estos sean cada vez mas complicados

<img width="667" height="416" alt="image" src="https://github.com/user-attachments/assets/f44ba9e7-7615-4871-a4a3-bb76a2befc2f" />

A diferencia de voting y stacking que tienen un enfoque de modelos _multi-expertos_, Cascading tiene un enfoque _multi-estado_.

> Cascadas muy profundas pueden producir overfitting

## Ensambles hibridos - Resumen
- La combinacion de clasificadores permite generar clasificadores mas precisos a cambio de menor comprension de los mismos.
- Ventajas: generalmente logran mejores predicciones que los modelos vistos hasta ahora; buen trade-off sesgo varianza
- Desventajas: se pierde interpretabilidad; tienen una complejidad computacional mayor


\newpage

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
 - Positiva (si una crece, la otra también, si una decrece, la otra también)
 - Negativa (si una crece, la otra decrece, y viceversa)
 - Sin correlación 

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

<img width="1184" height="512" alt="image" src="https://github.com/user-attachments/assets/59a212eb-069f-441e-93e0-f9ae9db528a5" />


### Correlación de Pearson
Para dos variables se puede medir su **correlación lineal** con el coeficiente de correlación `r`. Este coeficiente es una función que mide cuan relacionadas están las dos variables de forma lineal.
 - Si da 0 **NO existe correlación**
 - Si da 1 están **relacionadas linealmente de forma perfecta** (todos los puntos estan en una linea)
 - Si da -1 existe una **correlación negativa perfecta**

<img width="523" height="252" alt="1__#$!@%!#__1__#$!@%!#__Pasted Graphic" src="https://github.com/user-attachments/assets/f1d11f74-7620-40ad-b241-12b4679b177d" />

### Desvio estandar
Es una medida que se utiliza para cuantificar la variación o la dispersión de un conjunto de datos numéricos. 
 - Si la desviación estándar es **baja**, la mayor parte de los datos de la muestra tienden a estar agrupados cerca de su media
 - Si la desviación estándar es **alta**, indica que los datos se distribuyen en un rango de valores mas amplio.

## Regresión
Regresion: **predecir** un valor en un rango continuo, para ciertos valores de entrada.

<img width="566" height="274" alt="1__#$!@%!#__1__#$!@%!#__Pasted Graphic 1" src="https://github.com/user-attachments/assets/bec0bdf9-ced5-4591-988c-adbef0c469c8" />

<img width="639" height="207" alt="1__#$!@%!#__Pasted Graphic 2" src="https://github.com/user-attachments/assets/30b1b33c-04f2-4456-ada5-2bccb2019afb" />

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
 - El agrupamiento siempre será por características similares

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
 - Para cada centroide calcular la distancia promedio: si hay una caída abrupta hay un “codo” y entonces ese punto debe ser la cantidad de clusters

<img width="438" height="195" alt="Pasted Graphic 4" src="https://github.com/user-attachments/assets/ac367402-311e-4e38-bbf4-c01c8d4db33f" />


### Metodo Silhouette 
- Elegimos un rango, ejemplo 1 a 10, y para cada valor
- Para cada valor de K graficamos la silhouette
- El mejor valor posible es silhouette = 1, y el peor silhouette = -1

#### Coeficiente de Silhouette
Cada punto en el conjunto de datos tiene un coeficiente de Silhouette. Para calcularlo necesitamos calcular 

`a(i)`: distancia promedio del punto i a cada uno de los puntos de su cluster 

<img width="311" height="185" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/33d03c02-9f9c-4e31-8407-3d4dff3bb180" />

`b(i)`: distancia promedio del punto i a cada uno de los puntos del cluster mas cercano a su propio cluster

<img width="238" height="184" alt="Pasted Graphic 6" src="https://github.com/user-attachments/assets/c8bb6d16-c8ee-41bf-98de-b9f001275e75" />

- Idealmente a(i) << b(i)


**Consideraciones:**
 - Si a(i) > b(i), i esta posiblemente mal clasificado.

El coeficiente de Silhouette se calcula como

<img width="192" height="59" alt="Pasted Graphic 8" src="https://github.com/user-attachments/assets/7ac5b193-8775-44bb-8948-a53b78b134f8" />

Donde en el peor de los casos s(i) = -1

<img width="453" height="240" alt="Pasted Graphic 9" src="https://github.com/user-attachments/assets/b4e3eed1-526f-4c22-8fbf-18cbffdd2e93" />


Para buscar **el mejor K** podemos hacer el plot para distintos K, es mejor cuantos menos outliers haya.

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

### Interpretación 
 - Si D está distribuida de forma uniforme, entonces ∑ xi y ∑ yi   serían muy parecidos, entonces H sería aproximadamente ½ (0.5). Significa que posiblemente mi conjunto original tiene una distribución uniforme y no hay una tendencia al cluster
 - Pero si hay clústeres en D, las distancias de los puntos artificiales ∑ yi serían mucho más grandes que las distancias de los puntos reales:  ∑ xi y por lo tanto H sería mayor que 0.5. 
 - Un valor de H superior a 0,75 indica una tendencia a la agrupación en un nivel de confianza del 90 %.

### Hipotesis que maneja Hopkins
 - **Hipotesis nula**: el conjunto de datos D se distribuye uniformemente (o sea no hay clusters significativos)
 - **Hipótesis alternativa**: el conjunto de datos D no esta uniformemente distribuido (contiene clusters significativos)

Podemos realizar la prueba de la estadística de Hopkins de forma iterativa, utilizando 0,5 como umbral para rechazar la hipótesis alternativa. 
 - Es decir, si H < 0,5, es poco probable que D tenga conglomerados estadísticamente significativos.
 - O si el valor de la estadística de Hopkins es cercano a 1, entonces podemos rechazar la hipótesis nula y concluir que el conjunto de datos D es significativamente un dato agrupable.

## Entrenamiento
**Como se entrenan estos modelos?**

Se va a usar una parte de los datos para entrenar el modelo y la otra para testearlo.

A su vez se puede partir el conjunto de entrenamiento en 5 y usar una partecita para testear que se va a ir moviendo a lo largo de 5 iteraciones para ver que manera el modelo quedo mejor entrenado.

<img width="452" height="182" alt="Pasted Graphic 12" src="https://github.com/user-attachments/assets/9fa00aca-398a-460c-8e12-b904ea1bbe91" />

<img width="400" height="184" alt="Pasted Graphic 13" src="https://github.com/user-attachments/assets/e586c3d2-c5eb-4db2-a752-584e326797e8" />


### Conjuntos balanceados
Muchas veces el conjunto de datos no están balanceados y eso perjudica el entrenamiento. Se puede sacar muestras del grupo grande o agregar del grupo chico.

<img width="589" height="177" alt="Pasted Graphic 14" src="https://github.com/user-attachments/assets/b1d11929-4733-4b32-85ae-3d1f6c5e349e" />

### Overfitting
Overfitting es que el modelo aprendió de mas, pero esto significa que se aprendió de memoria los valores de entrenamiento entonces va a ser malo en el caso real.

**Buscamos la generalización, no la memorización.**

<img width="394" height="146" alt="Pasted Graphic 15" src="https://github.com/user-attachments/assets/57969c46-0240-4492-a901-30f3812e3282" />


\newpage

# K-vecinos más cercanos

Para decidir a que clase pertenece un nuevo dato, vemos a que clase pertenecen sus **k** vecinos mas cercanos de los datos conocidos (más cerca según su distancia euclidiana).

Este modelo tiene hiperparámetros, o sea los parámetros que me permiten configurar el modelo.

Este modelo tiene como hiperparametros:
- El valor **K**
- El tipo de distancia: euclidea, manhattan, etc
- Peso: para poder ponderar la distancia y que tengan mas peso los que estan mas cerca.


**Problemas:**
- Conjuntos desbalanceados: una clase esta sobrerepresentada

<img width="330" height="285" alt="image" src="https://github.com/user-attachments/assets/184a0205-ff53-4e26-9b46-9dbf39398660" />

- Outliers: si aparece una nueva observacion en un conjunto con muchos outliers, se va a clasificar a la observacion con los outliers pero no necesariamente será cierto

<img width="316" height="192" alt="image" src="https://github.com/user-attachments/assets/d17d81b9-9954-472f-bafd-70a50b87990a" />


**Resumen:**
- Método de clasificación
- Es sensible a conjuntos de datos no balanceados
- Es sensible a outliers
- Es scikit-learn, k=5 por defecto


# Support Vector Machines (SVM)

## Clasificadores basados en el margen, respecto del umbral
Problema:

<img width="683" height="208" alt="image" src="https://github.com/user-attachments/assets/4d5af138-2e3d-492e-895b-9c8e2f3b3535" />

<img width="681" height="167" alt="image" src="https://github.com/user-attachments/assets/dfb0e0cd-4049-4a79-94a3-263086b4cf71" />

<img width="678" height="214" alt="image" src="https://github.com/user-attachments/assets/da28ab5b-2b1e-41ce-8cb8-b4017f10b316" />

Significa que el umbral aleatorio es malisimo. Elegimos otro con mas sentido, o sea en el medio de las dos observaciones limite.

<img width="678" height="181" alt="image" src="https://github.com/user-attachments/assets/634a1a74-6010-4f95-a327-bbe21c5174a8" />

<img width="663" height="221" alt="image" src="https://github.com/user-attachments/assets/fb102182-5eab-4bf8-ae1c-22d0105c46f2" />

La distancia **mínima** entre cualquier observación y el umbral se llama **margen**

Como el umbral se puso en el medio de alas observaciones limite, el margen es igual a ambos lados -> el margen es el mas grande posible.

**Clasificador con el margen mas grande posible:** se llama Margen Máximo (**Maximal Margin Classifier**).

Un outlier arruina esto porque pondria el umbral en un lugar que daria lugar a clasificaciones incorrectas. **Es sensible a outliers**.

<img width="664" height="153" alt="image" src="https://github.com/user-attachments/assets/670e8bf1-1d85-4263-a461-1c5f078f318d" />

Se puede mejorar!! Permitiendo clasificaciones erroneas dentro del margen.

<img width="675" height="276" alt="image" src="https://github.com/user-attachments/assets/262a60a7-f6cf-47f1-98a9-3dcb1aa4d749" />

Cuando permitimos una clasificacion erronea, se dice que el margen es un **soft margin**. 

Para calcular cual será el soft margin, se va a usar un algoritmo que usa validacion cruzada:
- Parte el conjunto en dos (70% - 30% o a veces en 10 subconjuntos). Usa uno para testing
- Utiliza pares de observaciones similares para calcular la distancia media del umbral y validara la clasificacion con el segundo conjunto
- Como se sabe de antemano que se va a soportar clasificaciones erroneas, se tendrá que definir cuantas soportará en su soft margin y cuantas observaciones.

Cuando se usa un margen blando para determinar la ubicacion del umbral, se esta usando un **Soft Margin Classifier** tambien conocido como **Support Vector Classifier**. Vector porque las observaciones en los limites y dentro del soft margin se llaman **Support Vectors**, me ayudan a calcular el umbral. Son vectores porque son puntos en un espacio de dimension N (en el ejemplo N=1), y soportan el area llamada soft margin.


Ahora el mismo problema en dos dimensiones: masa y altura de los ratones

<img width="370" height="226" alt="image" src="https://github.com/user-attachments/assets/af08d33a-3c64-4d02-8655-e2d06849507d" />

El umbral ahora es una recta

<img width="361" height="228" alt="image" src="https://github.com/user-attachments/assets/111c2739-8e57-44f0-abd9-c6472f263eac" />

Y los soft margins:

<img width="496" height="226" alt="image" src="https://github.com/user-attachments/assets/9c183858-4937-4c68-8661-adc5212b49a2" />

<img width="591" height="218" alt="image" src="https://github.com/user-attachments/assets/39ffc850-824a-4b46-8b40-bf777a6b13e5" />

Si fuese N = 3 con la altura, masa y edad del raton, el umbral va a ser un plano en un espacio tridimensional.

<img width="536" height="210" alt="image" src="https://github.com/user-attachments/assets/010d0c7b-8601-4c8e-9afa-35a9bf0dc8ac" />

Por convencion decimos que el support vector classifier es un hiper-plano, parte al espacio en dos.

Que pasa si el conjunto de datos no es linealmente separable

<img width="592" height="221" alt="image" src="https://github.com/user-attachments/assets/54145c9c-3bf0-4d79-b6d1-4402fa9d0d0f" />

**Los Maximal Margin Classifiers y los Support Vector Classifiers no pueden clasificar problemas que no son linealmente separables**

 Se puede mejorar con **Support Vector Machines**.


### Support Vector Machines
Tengo el problema en 1 dimension que no es linealmente separable y le agrego un eje y que se define como y = dosis ^ 2

<img width="484" height="50" alt="image" src="https://github.com/user-attachments/assets/dbc37863-43b2-4454-a0de-7813b51c2f49" />

<img width="519" height="306" alt="image" src="https://github.com/user-attachments/assets/8ee08768-343b-4355-b1d1-1fc697f3f07b" />

Ahora si es posible partir el espacio con una recta. La recta es el **Support Vector Classifier**

<img width="519" height="263" alt="image" src="https://github.com/user-attachments/assets/3195f085-90ee-4058-a6c9-3d8a866ce414" />

En general es:
1. Empezar con observaciones en una dimension relativamente baja
2. Llevar los datos a una dimension mayor
3. Encontrar un clasificador de tipo Support Vector que separe los datos en la alta dimension en 2 grupos.

#### Por que y = x^2? SVM Kernel polinómico
Porque el algoritmo atras de Support Vector Machines usa **Funciones Kernel** que sistematicamente buscan clasificadores de tipo Support Vector Classifiers en dimensiones mas altas.

En el ejemplo anterior se usó un Kernel de tipo **Polinómico**, el cual toma un parametro `d` para el grado del polinomio.

**En resumen, el SVM Kernel polinomico:**
1. El Kernel polinomico de SVM sistematicamente incrementa las dimensiones seteando `d`, desde 1 (o el valor base de las observaciones) hasta el valor del parametro.
2. Calcula en cada caso las relaciones entre las observaciones para encontrar un hiper-plano que parta el conjunto en dos.
3. Usando **validacion cruzada**, en cada caso calcula el error y se queda con el mejor `d`.

### SVM Kernel Radial - RBFK
Soporta Support Vector Classifiers en infinitas dimensiones. 

Funciona como un Nearest Neighbor model ponderado.

### SVM: The Kernel Trick
Las Kernel Functions solo calculan la relacion entre pares de observaciones como si estuviesen en otra dimension, pero no realizan una verdadera transformacion del espacio. Reduce el tiempo de computo necesario porque evita toda la matematica relacionada a la transformacion del espacio.



\newpage

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

Lo ideal es poder encontrar un modelo o estrategia que combine las variables.

#### Métodos multivariados
* **Distancia de Mahalanobis:** permite medir la distancia entre el punto x (con n coordenadas) y un conjunto de observaciones con media $$\hat{\mu}$$ y una matriz de covarianza S

<img width="478" height="163" alt="image" src="https://github.com/user-attachments/assets/9b14cbc8-055a-4422-9fb2-2872814f5e19" />

Voy a tener un registro en el dataset con distintas columnas, de 1 a n, cada fila va a tener un valor para las medias. 

Se mide la distancia para detectar outliers por encima de la elipse.

<img width="853" height="313" alt="image" src="https://github.com/user-attachments/assets/37a47d00-c99b-424a-b27d-af1dd656e6a0" />


* **LOF - Local Outlier Factor:** el metodo LOF valora puntos en un conjunto de datos multivariados. Es un metodo basado en densidad que utiliza la busqueda de vecinos mas cercanos:
  * Se compara la densidad de cualquier punto de datos con la densidad de sus vecinos
  * Parametro k (cantidad de vecinos) y metrica de distancia

El metodo calcula los scores para cada punto, se debe definir un umbral de corte, que depende del dominio.
* Si el score de punto X es 5, significa que la densidad promedio de los vecinos de X es 5 veces mayor que su densidad local

<img width="484" height="338" alt="image" src="https://github.com/user-attachments/assets/5b891be9-5972-40eb-9179-99bec7714748" />


* **Isolation Forest:** Es un algoritmo no supervisado y no paramétrico basado en árboles de decisión. **Idea Principal**: los datos anómalos se pueden aislar los datos normales mediante particiones recursivas del conjunto de datos.

Se toman observaciones, y cada arista es una "pregunta" que se le hace.

Como funciona? Tomar una muestra de los datos y construir un árbol de
aislamiento:
 1. Seleccionar aleatoriamente n características .
 2. Dividir los puntos de datos seleccionando aleatoriamente un valor entre el mínimo y el máximo de las características seleccionadas.

La partición de observaciones se repite recursivamente hasta que todas las observaciones estén aisladas.

<img width="265" height="350" alt="image" src="https://github.com/user-attachments/assets/ca58853c-502e-407d-a58b-c3fc6bb56951" />

<img width="837" height="253" alt="image" src="https://github.com/user-attachments/assets/29b17ce0-943b-4cc9-8f95-c4054706e114" />


\newpage

# Redes Neuronales

Redes Neuronales Artificiales. 

## Cómo funciona una neurona (biológica)? 

<img width="318" height="355" alt="image" src="https://github.com/user-attachments/assets/a5b1798b-05e8-4d30-b83a-325bd1411354" />

Las neuronas estan conectadas entre si a traves del axon de una y la dendrita de otra, entre las dos neuronas esta el **espacio intersinaptico**.

<img width="383" height="366" alt="image" src="https://github.com/user-attachments/assets/a8c030a1-9f33-4f82-bddf-676151bb5fe5" />

Un recepetor recibe un tipo especifico de neurotransmisor. Una forma de "hackear" al receptor es el alcohol.

Se activan neuronas por un estimulo externo o porque recibio neurotransmisores.

<img width="476" height="344" alt="image" src="https://github.com/user-attachments/assets/bac12ccb-183a-40d1-b9cf-39d39b93dcb4" />

Etc, etc...

## Neuronas Artificiales

<img width="580" height="350" alt="image" src="https://github.com/user-attachments/assets/346ce59a-365a-4b5c-9c9d-fc9d8911207a" />

<img width="677" height="364" alt="image" src="https://github.com/user-attachments/assets/f7ecc8ca-a27f-4cbf-a432-83ce6d2ef6e7" />


Tengo los inputs y uno adicional que es una constante (1), todos multiplicados por un w peso.

Es una neurona artificial que toma inputs de entrada (como x1,x2,...,xnx_1, x_2, ..., x_nx1​,x2​,...,xn​), los multiplica por números llamados pesos (que indican qué tan importantes son), y calcula un resultado total:

s = w0 + w1·x1 + w2·x2 + ... + wn·xn

Ese valor s es como un “puntaje”: la neurona primero calcula ese número, y recién después la step function lo convierte en una decisión (0 o 1).

Pasa ese resultado por una función escalon (la “step function”) que decide: si el resultado es suficientemente grande devuelve 1, si no devuelve 0, o sea que en el fondo esta neurona sirve para tomar decisiones tipo “sí o no” en base a los datos que recibe.

Funcion escalon: vale 1 para cada valor mayor que a, 0 si es menor que a.

Funcion escalon centrada en el cero:

<img width="609" height="348" alt="image" src="https://github.com/user-attachments/assets/d1c87015-fc24-4344-bde4-604394613774" />

### Perceptron simple 
Una sola neurona.

Es una sola neurona artificial usada como modelo de clasificación: recibe varias entradas, las combina con pesos y un bias, calcula un valor y decide si la salida es 0 o 1. Es la forma más básica de neurona que puede aprender a separar datos en dos grupos.

#### AND
completarrrrrrrrrrrrrr

**Todo entrenamiento de una red neuronal consiste en encontrar los pesos**

#### Funcion de error

#### Actualizacion de los pesos
<img width="303" height="181" alt="image" src="https://github.com/user-attachments/assets/1dc98b84-e26a-4c59-9255-5b5957a1805a" />

### Perceptron Mutlicapa


## Como se entrena una red neuronal?
Con el algoritmo **backpropagation**. Lo explicará despues...

## Redes SOM (Kohonen)
No necesita backpropagation, se entrena de forma automatica, es un modelo no supervisado (como k-means). 

Hay una capa de input y un modelo de n neuronas.

Esta red genera clusters como lo hace k-means. 

Tiene solo dos capas: una de entrada y otra de salida.

<img width="668" height="367" alt="image" src="https://github.com/user-attachments/assets/ab77555b-afd4-4a7e-b192-aa8495fcec98" />

Todos conectados con todos, cada conexion con un peso, que se setea inicialmente de forma aleatoria.

<img width="683" height="429" alt="image" src="https://github.com/user-attachments/assets/48aa4e19-c2a4-48c4-9e69-37f2731b0307" />

Se calcula la distancia euclideana para cada neurona de salida. Se activa la neurona con menor distancia. Luego, dado un radio, actualizar todos los pesos de las neuronas dentro de ese radio, se tienen que acercar a la neurona activada.

<img width="739" height="302" alt="image" src="https://github.com/user-attachments/assets/21b988ad-4a1c-4bb4-989d-8902ba91dd88" />

Esos radios se van haciendo cada vez mas chicos. Se definen centroides, las de mismo color tienen pesos cercanos a sus centroides. Se basa en distancias.

<img width="491" height="389" alt="image" src="https://github.com/user-attachments/assets/9b5f069f-c4a3-4259-95a1-b7ce4180cb61" />

Son "mapas autoorganizados". Se lo puede pensar como una evolucion/mejora de k-means.

Se trata de mapear un espacio de entrada a uno de salida.

## Backpropagation: paso a paso
Es un metodo de descenso por gradiente.

completar
