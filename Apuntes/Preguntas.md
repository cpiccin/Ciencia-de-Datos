### Explicar diferencia entre precisión y recall con un ejemplo. Qué rol cumple la matriz de confusión?

- Precisión: de todos los que se detectaron como positivos, cuantos son realmente positivos.

$$\frac{TP}{TP+FP}$$

Ejemplo: de 100 correos marcados como spam, 85 eran spam, la precisión serà 

$$\frac{85}{100} = 0.85$$

- Recall: de todos los positivos existenten, cuántos se detectaron.

$$\frac{TP}{TP+FN}$$

Ejemplo: si habían 100 correos spam y se detectaron 75

$$\frac{75}{100} = 0.75$$

La matriz de confusión muestra el rendimiento de un clasificador plasmando la cantidad de true positives/negatives (acertados) y false positives/negatives (eran el opuesto). Va a permitir calcular todas las metricas y entender que tipo de errores comete el modelo.

### Qué diferencias conceptuales hay entre bagging y boosting? Cómo se reflejan en Random Forest y XGBoost?

**Bagging** trata de generar m tablas en base a los datos, donde cada una va a tener un conjunto aleatorio de registros, así cada conjunto de entrenamiento se va a entrenar por separado. 
- Es una técnica que consiste en construir nuevos conjuntos de entrenamiento usando bootstrap (muestras aleatorias con reemplazo)
para entrenar distintos modelos, y luego combinarlos.

Reduce varianza

**Boosting** acá se entrena un modelo de forma secuencial. Se usan los datos que devuelve un modelo para usar en otro modelo que mejore los resultados. Se corrigen los errores del modelo anterior.

Reduce sesgo

En random forest se usa la tecnica de bagging de arboles (arboles independientes) y en XGBoost implementa gradient boosting (arboles que se corrigen entre si).


### Cual es la diferencia principal entre ensambles hibridos y homogeneos?
Ensambles hibridos combinan clasificadores de distinto tipo, mientras que los ensambles homogeneos es siempre el mismo modelo.

### Cual es la limitacion principal de perceptron simple? Que mecanismo usa backpropagation?
El perceptron simple solo puede resolver problemas que son linealmente separables (no puede modelar el XOR).

Backpropagation usa la regla de la cadena para calcular gradientes de la función de pérdida respecto a los pesos de cada capa, y luego aplica descenso por gradiente para actualizarlos.neuronales. 

### Diferencia entre problema de regresion y de clasificacion. Qué metricas usaria para evaluar cada uno?
La **regresion** trata de predecir valores a partir de un rango continuo para ciertos valores de entrada. Ej temperatura. Metricas: error absoluto medio, error cuadratico medio, raiz del error cuadratico medio

La **clasificacion** trata de buscar encasillar un valor en una categoria: "este valor pertenece a la categoria a, b, c ...". Metricas: en base a la matriz de confusion, existen las metricas precision, recall, FB Score, accuracy

### Un one-hot encoding sobre una variables con 200 categorias, Qué problema trae? Qué alternativas hay?
El problema es que se generarian 199 columnas binarias, habria mucha dispersion (la mayoria de columnas serian 0). Como solucion se podria agrupar categorias poco frecuentes en "otros"

### Explicar brevemente overfitting y underfitting. Como se detectan y como se evitan?
**Overfitting**: el modelo sobreajustó los datos de entrenamiento (aprendió el ruido). Se detecta cuando hay muy buena performance en entrenamiento pero mala en test. Alta varianza. Se evita con regularización, validación cruzada, modelos más simples.

**Underfitting**: el modelo no aprendió los patrones. Error alto tanto en train como en test. Alto sesgo. Se evita con modelos más complejos, menos regularización, más datos.

### Definir outliers y diferencia entre univariados y multivariados. Como se detectan?
Los outliers son una observacion muy diferente/distante del resto de los datos que puede deberse por una aleatoriedad, un error de medicion, etc. 

Los outliers univariados son puntos de datos que se desvían significativamente del resto de las observaciones e**n una sola variable**. Se detectan analizando la distribución de una única columna de datos, identificando valores inusualmente altos o bajos en comparación con la mayoría. Metodo Z-Score

Los outliers multivariados son observaciones atípicas que no destacan en una sola variable, sino **por la combinación inusual de sus valores en múltiples dimensiones.** Se detectan utilizando métodos como la distancia de Mahalanobis, Local Outlier Factor (LOF), isolation forest.

### Para que se usa la poda en C4.5? Puede un arbol manejar valores numericos? Para que sirve la impureza de Gini?
C4.5 es una mejora a ID3. Se agregó una poda que evita el overfitting simplificando el arbol eliminando nodos que no mejoran la clasificacion en test.

Gini: mide impureza de un nodo. Un nodo puro (Gini=0) tiene instancias de una sola clase. Se usa para elegir el mejor atributo y generar divisiones.  Es una medida de cuán a menudo un elemento elegido aleatoriamente del conjunto sería etiquetado incorrectamente si fue etiquedato de manera aleatoria de acuerdo a la distribución de las etiquetas en
el subconjunto

