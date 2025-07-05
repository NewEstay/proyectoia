# Proyecto de Introducción a la Inteligencia Artificial

## Integrantes:
Diego Pérez = Ghost-Cougar17 // Diego Estay = NewEstay

# Entregable 1:

## Definición del problema:

Si al igual que nosotros les gustan mucho los videojuegos, puede que en alguna ocasión o más de una se pusieran a buscar reseñas de un videojuego que quisieran analizar y/o comprar para ver si vale la pena gastar dinero en el (o descargar gratis evidentemente sin piratearlo, guiño, guiño). Lo que llama la atención es que la critica de “expertos” tiene un peso muy grande lo que puede elevar o enterrar un juego, con esto nos preguntamos ¿Qué es lo que aumenta o disminuye la calificación global de un juego por parte de la crítica “profesional”? ¿acaso es la plataforma donde está disponible? ¿las ventas iniciales? ¿el género al que pertenece? ¿el país de origen? ¿o solo lo que opina la gente en foros y cosas?
Para este proyecto se propone responder la primera pregunta en base a técnicas de aprendizaje automático para comprender cuales son las variables que influyen en la Critic_Score que es la nota que los medios especializados le dan a un videojuego.

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

## Justificación de los modelos:

En primera instancia escogimos usar regresiones lineales dada la fácil interpretación que nos ofrece, podemos observar cual es la relación de cada variable con la crítica y si el efecto es positivo o negativo.
El mundo de los videojuegos no es tan simple ya que existen relaciones más enredadas que otras, por está razón agregamos arboles de decisión dada la flexibilidad que ofrece al momento de mostrar interacciones y/o reglas que se pierden al aplicar únicamente regresiones lineales.
Tenemos la corazonada que al emplear 2 modelos en simultáneos podemos tener una visión más completa del panorama, por un lado, la claridad de una regresión y por otro la estructura detallada de un árbol de decisión. De esté modo esperamos concluir que es lo que busca la critica sobre lo que ellos creen que es “un buen juego”
