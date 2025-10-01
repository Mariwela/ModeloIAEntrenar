# ModeloIAEntrenar
Entrenar modelo IA

Predicción de la probabilidad de obtener una medalla olímpica en función de características demográficas y físicas del atleta y del país de origen.



•	Elección del dataset

o	Dataset: 120 years of Olympic history: athletes and results

https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results/data

o	¿Por qué?

Los Juegos Olímpicos son uno de los eventos deportivos más importantes del mundo, seguidos por millones de personas. Poder predecir la probabilidad de que un atleta gane una medalla, considerando sus características físicas y demográficas resulta un problema atractivo tanto para el público general como para la investigación académica. Es un tema transversal y multidisciplinar: combina historia, geopolítica, economía, cultura y deporte.


o	Posibles aplicaciones:

•	Aplicaciones en el ámbito deportivo

 	Detección de talento: ayudar a identificar atletas con mayor potencial según sus características físicas y demográficas.
 	Planificación de entrenamientos basados en perfiles de atletas que históricamente han tenido más éxito.
 	Tomar decisiones sobre en qué disciplinas invertir más recursos, considerando los perfiles típicos de ganadores.
  
•	Aplicaciones en tecnología e IA

 	Desarrollo de sistemas de recomendación: según perfil físico y demográfico.
 	Entrenadores virtuales inteligentes: sistemas que ajusten estrategias de entrenamiento según la probabilidad estimada de éxito.
  
•	Aplicaciones en investigación y análisis

 	Análisis de desigualdades: explorar cómo el género o el país afectan las oportunidades de ganar medallas.


 
•	Definición del problema

¿Es posible predecir si un atleta ganará una medalla olímpica utilizando como variables explicativas su sexo, edad, estatura, peso y país de origen?
Clasificación binaria.

El objetivo es clasificar cada atleta, utilizando las características físicas y demográficas:

1.	Clase 1: Gana Medalla (Gold, Silver, Bronze)
 
2.	Clase 0: No Gana Medalla (NA)




•	Selección de la variable dependiente y explicativas

o	Variable objetivo: ganar medalla (sí/no) o tipo de medalla (oro/plata/bronce/ninguna).

o	Variables de entrada/Features explicativas:
 	Sexo – categórica
 	Edad – numérica
 	Estatura - numérica
 	Peso - numérica
 	País de origen - categórica



 
•	Exploración inicial de datos (EDA)

El dataset tiene 15 columnas y 271.116 filas (participaciones individuales).

Análisis de la variable objetivo

0 (No Medalla)	  231.333	  85,3%

1 (Ganó Medalla)	39.783	  14,7% 

Este desequilibrio se origina en el hecho de que la cantidad de participantes en los Juegos Olímpicos es considerablemente mayor que la de ganadores de medallas.


Análisis de datos faltantes (Nulos)

Años	  9.484	  3,5%

Altura	60.171	22,2%

Peso	  62.875	23,2%

Las variables altura y peso tienen más de una quinta parte de sus valores faltantes.


Exploración de Variables Numéricas (Age, Height, Weight)

Variable	Media aprox.	Mediana aprox.	Desviación Estándar aprox.

Años	    25,6 años	    25 años	        6,4 años

Altura	  175,3 cm	    175,0 cm	      10,5 cm

Peso	    70,7 kg	      70,0 kg	        14,3 kg

Las distribuciones son relativamente normales, pero con colas amplias debido a la variedad de deportes (ej. un gimnasta de 14 años vs. un tirador de 60 años).
Relación con la Medalla


Variable	Comparación Ganó Medalla
0 vs. 1	Implicación Predictiva

Años	  La mediana de edad de los ganadores de medalla es a menudo ligeramente superior (ej. 26 años) a la de los no ganadores (ej. 25 años).	La experiencia es un factor. Atletas muy jóvenes o muy mayores tienen menor probabilidad.

Altura	La mediana de altura de los ganadores es ligeramente superior a la de los no ganadores.	No hay una altura "óptima" general, pero en deportes clave (baloncesto, natación), la altura alta es un fuerte predictor.

Peso	  La mediana de peso de los ganadores es ligeramente superior a la de los no ganadores.	La relación peso/altura (IMC) podría ser más predictiva que el peso solo.


Exploración de Variables Categóricas (Sex, NOC)

Sexo
Sexo	Tasa de éxito
M (Male)	  13,7%
F (Female)	16,8%

Las mujeres tienen una tasa de éxito por participación ligeramente superior a la de los hombres. Esto puede deberse a la menor variedad de eventos de participación femenina en los primeros años del historial olímpico.


País de Origen

País (NOC)	Total de medallas	Tasa de éxito
USA	                  5.600	    19,5%
URS (Unión Soviética)	2.500	    26,0%
GDR (Alemania Oriental)	1.500	  32,0%

El país de origen es un predictor extremadamente fuerte. Países como la antigua Alemania Oriental o la Unión Soviética, a pesar de tener menos participaciones totales que EE. UU., tienen una tasa de éxito por participación mucho mayor debido a sus políticas deportivas intensivas.
