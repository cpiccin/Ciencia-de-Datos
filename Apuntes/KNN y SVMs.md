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

