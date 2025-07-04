# Proyecto de Introducción a la Inteligencia Artificial

Entregable 1:

Definición del problema:

Si al igual que nosotros les gustan mucho los videojuegos, puede que en alguna ocasión o más de una se pusieran a buscar reseñas de un videojuego que quisieran analizar y/o comprar para ver si vale la pena gastar dinero en el (o descargar gratis evidentemente sin piratearlo, guiño, guiño). Lo que llama la atención es que la critica de “expertos” tiene un peso muy grande lo que puede elevar o enterrar un juego, con esto nos preguntamos ¿Qué es lo que aumenta o disminuye la calificación global de un juego por parte de la crítica “profesional”? ¿acaso es la plataforma donde está disponible? ¿las ventas iniciales? ¿el género al que pertenece? ¿el país de origen? ¿o solo lo que opina la gente en foros y cosas?
Para este proyecto se propone responder la primera pregunta en base a técnicas de aprendizaje automático para comprender cuales son las variables que influyen en la Critic_Score que es la nota que los medios especializados le dan a un videojuego.

Plan de acción:



Justificación de los modelos:

En primera instancia escogimos usar regresiones lineales dada la fácil interpretación que nos ofrece, podemos observar cual es la relación de cada variable con la crítica y si el efecto es positivo o negativo.
El mundo de los videojuegos no es tan simple ya que existen relaciones más enredadas que otras, por está razón agregamos arboles de decisión dada la flexibilidad que ofrece al momento de mostrar interacciones y/o reglas que se pierden al aplicar únicamente regresiones lineales.
Tenemos la corazonada que al emplear 2 modelos en simultáneos podemos tener una visión más completa del panorama, por un lado, la claridad de una regresión y por otro la estructura detallada de un árbol de decisión. De esté modo esperamos concluir que es lo que busca la critica sobre lo que ellos creen que es “un buen juego”
