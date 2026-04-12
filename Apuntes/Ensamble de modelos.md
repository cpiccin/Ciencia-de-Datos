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

