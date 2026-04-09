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







