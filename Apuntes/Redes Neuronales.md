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
