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

11:30
