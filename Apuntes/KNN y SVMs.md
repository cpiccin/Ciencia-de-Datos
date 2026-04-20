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

15:57


