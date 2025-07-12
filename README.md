# Análisis de factores que influyen en la calificación que la crítica profesional le da a un videojuego.

Integrantes:
- Ghost-Cougar17: Diego Pérez
- NewEstay: Diego Estay

## Definición del problema:
Si al igual que nosotros les gustan mucho los videojuegos, puede que en alguna ocasión o más de una se pusieran a buscar reseñas de un videojuego que quisieran analizar y/o comprar para ver si vale la pena gastar dinero en el (o descargar gratis evidentemente sin piratearlo, guiño, guiño). Lo que llama la atención es que la crítica de “expertos” tiene un peso muy grande lo que puede elevar o enterrar un juego, con esto nos preguntamos ¿Qué es lo que aumenta o disminuye la calificación global de un juego por parte de la crítica “profesional”? ¿acaso es la plataforma donde está disponible? ¿las ventas iniciales? ¿el género al que pertenece? ¿el país de origen? ¿o solo lo que opina la gente en foros y cosas?

La idea de este proyecto es darle una respuesta a la pregunta planteada al principio, si bien esto puede parecer simple curiosidad, la verdad es que, en términos prácticos, el entender patrones de recurrencia puede ser de ayuda para los propios desarrolladores de juegos virtuales tanto de empresas grandes como de desarrolladores independientes (también llamados “indies”). Otras observaciones que pueden llegar a ser interesantes son como interactúan variables entre sí (por ejemplo, las ventas con el género, que géneros son más populares dependiendo de la región, cual es la clasificación de edades de los juegos con mejor y/o peor puntaje por parte de la crítica o de los usuarios, entre otras).

## Plan de acción
El proyecto utilizara como base el dataset de kaggle llamado “Video Game Sales and Ratings” (Link = https://www.kaggle.com/datasets/kendallgillies/video-game-sales-and-ratings) el cual posee muchos datos sobre videojuegos (algunos más conocidos que otros) y aunque no todos los registros están completados existen muchos datos valiosos para el estudio.

### Descripción del dataset:

Contamos con una base de 15 variables y más de 16000 instancias, dichas variables son:
- Name: Título del juego.
- Platform: Plataforma donde fue lanzado (PS2, PS4, Xbox One, PC, Wii, etc).
- Year_of_Release: Año de lanzamiento del juego.
- Genre: Tipo de juego (aventura, deporte, acción, terror, etc).
- Publisher: Desarrolladora y/o distribuidora del juego.
- NA_Sales: Ventas en Norte América.
- EU_Sales: Ventas en Europa.
- JP_Sales: Ventas en Japón.
- Other_Sales: Ventas en el resto del mundo.
- Global_Sales: Suma total de las ventas en todo el mundo.
- Critic_Score: Nota promedio de la crítica profesional (calificación de 0 a 100).
- Critic_Count: Cantidad de críticos que evaluaron el videojuego.
- User_Score: Nota promedio de los usuarios suscritos en Metacritic
- User_Count: Cantidad de usuarios que evaluaron el juego
- Rating: Clasificación de edades (todo público, adolescente, adulto, etc).

En esta oportunidad nuestra variable de estudio será Critic_Score dado que lo que buscamos es predecir a que llama la crítica “un buen juego” y para esto utilizamos variables predictoras tanto numéricas como categóricas.
### Modelos que utilizaremos:
- Regresiones lineales las cuales nos permitirán analizar como cada una de las variables afectan al Critic_Score y si ese efecto es positivo o negativo, aunque también puede no tener efecto significativo y por lo tanto descartable.
- Árbol de decisión para capturar relaciones y patrones más complejos que la regresión simple no detecta por sí solo.
### Métricas de evaluación:
- $MSE$ para medir que tan cerca o lejos están las predicciones del valor verdadero.
- $R^2$ para ver la proporción del modelo explica la variabilidad de la crítica.
Cabe destacar que nuestro objetivo principal es entender el fenómeno que se presenta, es decir, más allá de que las métricas sean buenas, nuestro enforque es saber porque ciertas variables verdaderamente influyen en la calificación y no solo que lo hagan.

## Justificación del modelo

En primera instancia escogimos usar regresiones lineales dada la fácil interpretación que nos ofrece, podemos observar cual es la relación de cada variable con la crítica y si el efecto es positivo o negativo. Este modelo si bien es simple en ejecución nos permite saber de manera sencilla como y cuanto cambia la crítica profesional si modificamos la calificación de los usuarios o las propias ventas globales. Podemos obtener conclusiones prácticas del tipo “cada punto adicional en las ventas de Y región se asocia con un aumento de X unidades promedio de puntos en la crítica”.

Para este modelo tenemos limitaciones al intentar capturar interacciones no lineales entre variables, como ejemplo quizás aumentar la nota de los usuarios eleva de forma exponencial la nota de crítica, aunque también puede ser logarítmica, o también no podemos detectar si un juego de X género es mejor recibido si se lanza en una plataforma o año determinado.

El mundo de los videojuegos no es tan simple ya que existen relaciones más enredadas que otras, por esta razón agregamos arboles de decisión dada la flexibilidad que ofrece al momento de mostrar interacciones y/o reglas que se pierden al aplicar únicamente regresiones lineales. Un ejemplo de esto podría ser “si el género es de peleas, la plataforma es NES y la nota de los usuarios es menor a 4 eso implica que la nota de la crítica será baja”.
La limitación de este modelo es que no tiene tanta precisión como otros modelos más complejos tipo Random Forest o similares, sin embargo recalcamos que nuestra intención es más de interpretar que la de tener resultados ultra precisos.

Tenemos la corazonada que al emplear 2 modelos en simultáneos podemos tener una visión más completa del panorama, por un lado, la claridad de una regresión y por otro la estructura detallada de un árbol de decisión. De este modo esperamos concluir que es lo que busca la crítica sobre lo que ellos creen que es “un buen juego”

## Metodologías y Resultados

### Modelo de Regresión Lineal

Comenzamos utilizando la variable `Publisher` en donde agrupamos a todos los desarrolladores menores (o menos frecuentes) en base a la categoría “Other” de este modo conservando solamente los 15 con más frecuencia. Al realizar dicha transformación reducimos la cardinalidad de esta variable categórica y esto es nos evita el tener que crear una gran cantidad de columnas al aplicar codificación, esto también nos ayuda para reducir el ruido y el riesgo de sobreajuste que puedan generar las categorías con pocos datos.

Realizamos una transformación de tipo logaritmo a las variables que representan ventas (`NA_Sales`, `EU_Sales`, `JP_Sales`, `Other_Sales` y `Global_Sales`) utilizando la función $np.log1p$ que nos ayuda incluso a manejar los valores que son iguales a 0. La idea de usar esta función en analizar alguna posible tendencia hacia distribuciones muy asimétricas, por ejemplo, visualizar si muchos juegos venden muy poco o pocos juegos vendiendo mucho, el logaritmo reduce esta asimetría, nos favorece en el rendimiento del modelo de regresión lineal y a su vez nos facilita la interpretación de las relaciones entre las variables.

Considerando que estamos en el año 2025 y la fecha de lanzamiento del juego (`Year_of_Release`) calculamos la edad del juego restando el año actual menos el año de lanzamiento, a esta edad la denominamos `Game_Age` la cual nos permitirá evaluar la posibilidad de que la antigüedad de un juego tenga algún tipo de influencia en las puntuaciones de las críticas lo cual es difícil de captar si solo consideramos el año de lanzamiento de juego, además está variable podría ayudarnos a modelar tendencias temporales las cuales pueden llegar a ser relevantes para la predicción.

Al momento de intentar predecir la calificación de la crítica profesional (`Critic_Score`) seleccionamos variables explicativas (`features`) relevantes tanto desde una perspectiva empírica como teórica. De igual modo incluimos características categóricas como lo son la plataforma en donde fue lanzado el juego, el género de este, la clasificación de edad y la desarrolladora y/o distribuidora (`Platform`, `Genre`, `Rating` y `Publisher` respectivamente), además de las características numéricas como las ventas de tipo logaritmo por región, las puntuaciones y conteo de usuarios y antigüedad del juego. Lo que buscamos con esta selección de variables es capturar el contenido de un juego y como impacta en el mercado, además de como impacta en la percepción de los usuarios y críticos.

Para el preprocesamiento implementamos un pipeline con ayuda de `ColumnTransformer` de modo que aplicamos transformaciones que sean diferenciadas en base al tipo de variable. Por otro lado, con ayuda de `StandardScaler` estandarizamos las variables de tipo numérico para garantizar que todas tengan la misma escala. Por el lado de las variables categóricas fueron codificadas con `OneHotEncoder` lo que permite interpretar algunas variables como lo son `Platform` o `Genre` sin necesidad de asumir un orden implícito.

Para el modelo de predicción utilizamos regresión lineal integrada en pipeline con el preprocesamiento lo que nos permite mantener un flujo reproducible. Por otro lado, el dataset lo dividimos en conjuntos de entrenamiento (80%) y prueba (20%) donde realizamos una **validación cruzada de 5 folds** sobre los datos de entrenamiento para estimar cuanta capacidad de generalización tiene el modelo antes de realizar el test final. Dicha técnica nos ayuda mejorando la estimación real de modelo evitando la dependencia de una única partición del conjunto de datos.

Por último, entrenamos el modelo para evaluar el desempeño del conjunto de prueba en base a las métricas **$R^2$** que nos indica el porcentaje de variabilidad que explica el modelo y el **$MSE$** que mide la precisión promedio de las predicciones. Decidimos utilizar dichas métricas dado que nos permiten entender no solamente si el modelo se ajusta bien al conjunto de entrenamiento, sino también la capacidad que tenemos para generalizar algunos datos que no son observados.

Nuestro modelo de regresión lineal consiguió un **$R^2$ cruzado de 0.559** lo que nos indica que, en promedio, el modelo que hicimos puede explicar casi el 56% de la variabilidad en puntaciones reales de los críticos expertos del conjunto de entrenamiento, si bien este desempeño es moderadamente bueno, debemos recordar que la naturaleza de la critica por si sola es compleja y en muchos casos subjetiva.

En cuanto al conjunto de testeo, nuestro modelo alcanzó un **$R^2$ de 0.541** lo que nos confirma que el rendimiento del modelo se sigue manteniendo relativamente estable frente a datos no observados lo cual es bueno ya que no tenemos evidencias claras de posibles sobreajustes. El valor indica que más de la mitad de dichas variaciones en las puntuaciones pueden ser explicadas por las variables que fueron seleccionadas, las cuales consideramos que son aceptables dado que el comportamiento de los “expertos” en la materia pueden depender de algún posible factor cualitativo difícil de capturar.

Por el lado del **MSE en el test fue de 90.21** lo que quiere decir que, el cuadrado de la diferencia entre las predicciones y los valores reales es de 90 unidades promedio aproximadamente. Tenemos que considerar que, si bien esto no nos permite interpretar de manera directa los errores en la misma escala que la variable original, si nos permite darnos la idea del grado de dispersión que poseen las predicciones.

Considerando todo lo anterior, estos resultados nos muestran que el modelo tiene una razonable capacidad explicativa, aunque no es perfecta, esto se debe a que la variable objetivo esta influenciada por la percepción humana y factores no cuantificables, pero creemos que este desempeño es bastante coherente con lo esperado en un modelo lineal.

### Modelo de Árbol de Decisión

Ahora entrenamos un modelo de árbol de decisión para predecir puntuaciones de los críticos (`Critic_Score`). Nos aseguramos de tener consistencia metodológica reutilizando el mismo pipeline de preprocesamiento que empleamos en la regresión lineal, el cual ya incluye la estandarización de las variables númericas y la codificación por one-hot de las variables categóricas. De este modo podemos aplicas las mismas transformaciones a los datos antes de entrenar el nuevo modelo.

Utilizamos `DecisionTreeRegressor` de $scikit.learn$ como modelo base en donde configuramos una semilla fija (`random_state=42`) lo cual nos garantiza la reproducibilidad de los resultados. Posteriormente entrenamos el modelo en base al conjunto de entrenamiento y realizamos las predicciones en base al conjunto de test.

Al igual que en regresión lineal, medimos el rendimiento de este modelo en base a **$R^2$** y el **$MSE$** sobre los datos de prueba. A esto le aplicamos la **validación cruzada con 5 folds** sobre el conjunto de entrenamiento para obtener una estimación robusta sobre la capacidad predictora del modelo.

La estructura nos ayuda a comparar directamente el desempeño propio del árbol de decisión con la regresión lineal, esto es debido a que ambos modelos realizan sus operaciones sobre los mismos datos y bajo las mismas condiciones.

En este caso, el modelo de árbol de decisión consiguió un **$R^2$ de validación cruzada de 0.223**, en otras palabras, nuestro modelo es capaz de explicar apenas casi el 23% de la variación en las puntuaciones de los profesionales dentro del conjunto de datos. Podemos notar que esta cifra es considerablemente inferior al desempeño obtenido por la regresión lineal lo que nos indica una limitancia en la capacidad de predicción para este modelo.

Por el lado del conjunto de prueba el **$R^2$ fue 0.214** lo que indica que apenas podemos capturar cerca del 21% de la variación real en las calificaciones de los críticos. Teniendo esto en mente podemos concluir que en estado actual en que el árbol de decisión fue configurado **no logramos modelar de forma adecuada la complejidad del fenómeno**. A esto debemos sumarle el dato de que el **$MSE$ fue de 154.65** lo cual es significativamente superior al 90.21 obtenido por la regresión lineal, lo que confirma una mayor dispersión de los errores en la predicción.

Decidimos intentar mejorar de alguna manera este modelo, para esto aplicamos un **ajuste de hiperparametros utilizando `GridSearchCV`** la cual es una técnica para buscar exhaustivamente la mejor combinación posible de parámetros para el modelo en base a validación cruzada.

Los hiperparametros que ajustamos fueron:
- `max_depth`: Profundidad máxima del árbol, controla cuanto puede crecer y cuán especifico puede volverse.
- `min_samples_split`: Número mínimo de muestras que necesita para dividir un nodo.
- `min_samples_leaf`: Número mínimo de muestras que debe tener una hoja terminal del árbol.

Consideramos distintas combinaciones de estos parámetros y evaluamos cada configuración utilizando **validación cruzada con 5 folds** con la cual estimamos el rendimiento de cada combinación y así seleccionar automáticamente la que nos maximice el coeficiente **$R^2$**.

Al encontrar el mejor conjunto de hiperparámetros, entrenamos nuevamente el modelo sobre los datos de entrenamiento en base a la configuración óptima y evaluamos el conjunto de test con la esperanza de que lográramos mejorar la capacidad de generalización del modelo y reducir sobreajustes, siendo esto uno de los problemas en los arboles de decisiones con configuración simple.

Una vez aplicada esta estrategia, nuestro árbol de decisión optimizado consiguió un **$R^2$ de validación cruzada de 0.492**, este resultado es bastante mejor que el modelo base el cual apenas alcanzaba el 22%. Este modelo captura de mejor manera la estructura de los datos de entrenamiento y generaliza la capacidad de predicción de forma más efectiva.

Por el lado del conjunto de prueba, el modelo alcanzo un **$R^2$ de 0.456** lo que indica que en este estado el modelo puede explicar un 46% de la variabilidad en las puntuaciones reales de los profesionales. Si tenemos en cuenta lo obtenido por el modelo de regresión lineal es un poco inferior, pero sigue siendo significativamente superior al árbol de decisión sin regulación.

Debemos agregarle a esto que el **$MSE$ se redujo a 106.98** lo cual mejora de forma considerable el error obtenido en el árbol simple (casi 155), sin embargo, sigue estando por encima del obtenido por la regresión lineal. Aunque nuestro modelo optimizado es un poco peor que nuestro primer modelo de regresión sigue siendo más eficiente en disminución de errores y predice de mejor manera que su contraparte simple.

Encontramos que los hiperparametros óptimos son:
- `max_depth = 5`
- `min_samples_split = 5`
- `min_samples_leaf = 1`

Estos resultados muestran que configurar la complejidad del árbol de decisión mediante hiperparámetros que sean clave **mejora la precisión y estabilidad**, esto nos puede convertir un modelo simple en uno más competitivo y que sea menos propenso a ser sobreajustado.

## Conclusión

El objetivo de este proyecto fue identificar cuales son los factores que influyen en la calificación que la critica experta le asigna a un juego. A partir del análisis exploratorio y modelos predictivos llegamos a la conclusión de variables como `User_Score`, `Platform`, `Genre` y `Publisher` tienen un impacto considerable sobre la nota entregada por la crítica. Especialmente el puntaje que entrega el usuario mostro una correlación directa con las calificaciones de los profesionales en la materia lo que nos podría indicar que ambas perspectivas se alinean de manera parcial. El árbol de decisión nos permitió visualizar de manera jerárquica los pesos de casa una de las variables, esto lo complementamos con lo visto en la regresión lineal donde utilizamos métricas de rendimiento como el $R^2$ y el $MSE$, de esto modo logramos valores aceptables para un primer modelo que tiene una índole más interpretativa que predictora.

Logramos comprobar que la evaluación que la crítica le asigna a un videojuego no es aleatoria, sino que responde a una variedad de factores que pueden analizarse de manera estructural. Este tipo de análisis puede ser útil para las propias desarrolladoras y/o distribuidoras que buscan mejorar sus productos desde etapas tempranas de desarrollo, la información que pueden obtener les podría servir para entender que elementos son mejor valorados por la crítica especializada.