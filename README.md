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
