
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
