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
- Soporta campos numericos y rangos continuos:
  -  El algoritmo puede crear dinamicamente un campo **booleano** tal que si A<Ac = true, sino Ac = False.
  -  Se va a encontrar ese umbral c de forma que quede con la mayor ganancia de informacion
  -  Se ordena A por ej de menor a mayor; luego se identifican los valores adyacentes (de la clase que es nuestra salida); detectamos cuando hay un cambio de valor de salida, entonces en esos limites seguramente estan nuestros Ci candidatos; nos quedamos con el Ci que de el mejor resultado.
- Contempla la posibilidad de usar datos faltantes:
  - Los atributos faltantes los marca con un ? y no se usan para el calculo de la ganancia o la entropia  
- Se agrego un metodo adicional de poda que evita el overfitting:
  - Se genera el arbol y luego se analiza de forma recursiva, desde las hojas que preguntas (nodos interiores) se pueden eliminar sin que se incremente el error de clasificacion.
  - Mas a detalle...
      1. Se elimina un nodo interior cuyos sucesores son todos nodos hoja
      2. Se vuelve a calcular el error que se comete con este nuevo arbol sobre el conjunto de test
      3. Si este error es menor que el error anterior, entonces se elimina el nodo y todos sus sucesores (hojas)
      4. Se repite 



## Random Forest
Es un **ensamble**

> Muchos estimadores mediocres, promediados, pueden ser muy buenos estimadores

### Bootstrap aggregating:

Es una técnica, o meta-algoritmo que dice lo siguiente:
- Dado un conjunto de entrenamiento D, de tamaño n, la técnica de bagging generará m nuevos conjuntos de entrenamiento D1,... Di,..., Dm cada uno de tamaño n' tomando muestras aleatorias de D.

Y en general n' < n. Siendo n' aproximadamente un 2/3 de n.


#### Attribute bagging (o random subspace):

Luego para cada una de las m tablas, escogemos sólo algunos atributos (COLUMNAS) de forma aleatoria también.

¿Cuántas nos quedamos? => Con RAÍZ CUADRADA del número de atributos.

Cómo acá hay 8 atributos, tomamos Round(√8) = 3

(Esto es recomendable sobre todo cuando hay muchas columnas)












