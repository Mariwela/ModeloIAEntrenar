# ModeloIAEntrenar
Entrenar modelo IA

Predicción de la probabilidad de obtener una medalla olímpica en función de características demográficas y físicas del atleta y del país de origen.

##Elección del dataset

###Dataset: 120 years of Olympic history: athletes and results
https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results/data

###¿Por qué?
Los Juegos Olímpicos son uno de los eventos deportivos más importantes del mundo, seguidos por millones de personas. Poder predecir la probabilidad de que un atleta gane una medalla, considerando sus características físicas y demográficas resulta un problema atractivo tanto para el público general como para la investigación académica. Es un tema transversal y multidisciplinar: combina historia, geopolítica, economía, cultura y deporte.

###Posibles aplicaciones:
•	Aplicaciones en el ámbito deportivo

 	Detección de talento: ayudar a identificar atletas con mayor potencial según sus características físicas y demográficas.
 	Planificación de entrenamientos basados en perfiles de atletas que históricamente han tenido más éxito.
 	Tomar decisiones sobre en qué disciplinas invertir más recursos, considerando los perfiles típicos de ganadores.
  
•	Aplicaciones en tecnología e IA

 	Desarrollo de sistemas de recomendación: según perfil físico y demográfico.
 	Entrenadores virtuales inteligentes: sistemas que ajusten estrategias de entrenamiento según la probabilidad estimada de éxito.
  
•	Aplicaciones en investigación y análisis

 	Análisis de desigualdades: explorar cómo el género o el país afectan las oportunidades de ganar medallas.
 
##Definición del problema

¿Es posible predecir si un atleta ganará una medalla olímpica utilizando como variables explicativas su sexo, edad, estatura, peso y país de origen?
Clasificación binaria.

El objetivo es clasificar cada atleta, utilizando las características físicas y demográficas:

1.	Clase 1: Gana Medalla (Gold, Silver, Bronze)
 
2.	Clase 0: No Gana Medalla (NA)

##Selección de la variable dependiente y explicativas

###Variable objetivo: ganar medalla (sí/no) o tipo de medalla (oro/plata/bronce/ninguna).

###Variables de entrada/Features explicativas:
 	Sexo – categórica
 	Edad – numérica
 	Estatura - numérica
 	Peso - numérica
 	País de origen - categórica
 
##Exploración inicial de datos (EDA)

El dataset tiene 15 columnas y 271.116 filas (participaciones individuales).

###Análisis de la variable objetivo
0 (No Medalla)	  231.333	  85,3%

1 (Ganó Medalla)	39.783	  14,7% 

Este desequilibrio se origina en el hecho de que la cantidad de participantes en los Juegos Olímpicos es considerablemente mayor que la de ganadores de medallas.

###Análisis de datos faltantes (Nulos)
Años	  9.484	  3,5%

Altura	60.171	22,2%

Peso	  62.875	23,2%

Las variables altura y peso tienen más de una quinta parte de sus valores faltantes.


###Exploración de Variables Numéricas (Age, Height, Weight)
Variable	Media aprox.	Mediana aprox.	Desviación Estándar aprox.

Años	    25,6 años	    25 años	        6,4 años

Altura	  175,3 cm	    175,0 cm	      10,5 cm

Peso	    70,7 kg	      70,0 kg	        14,3 kg

Las distribuciones son relativamente normales, pero con colas amplias debido a la variedad de deportes (ej. un gimnasta de 14 años vs. un tirador de 60 años).
Relación con la Medalla


###Variable	Comparación Ganó Medalla
0 vs. 1	Implicación Predictiva

Años	  La mediana de edad de los ganadores de medalla es a menudo ligeramente superior (ej. 26 años) a la de los no ganadores (ej. 25 años).	La experiencia es un factor. Atletas muy jóvenes o muy mayores tienen menor probabilidad.

Altura	La mediana de altura de los ganadores es ligeramente superior a la de los no ganadores.	No hay una altura "óptima" general, pero en deportes clave (baloncesto, natación), la altura alta es un fuerte predictor.

Peso	  La mediana de peso de los ganadores es ligeramente superior a la de los no ganadores.	La relación peso/altura (IMC) podría ser más predictiva que el peso solo.


###Exploración de Variables Categóricas (Sex, NOC)
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

##Preprocesamiento de datos
		
		###Valores nulos:
		
			Columna Medal - Todos los valores NaN (Not a Number, que son los valores faltantes) reemplazados con el texto 'NoMedal'. Creada una nueva columna llamada Medaled. Asignado 1 si el valor es 'Medal' y 0 si es 'NoMedal'.
			
			Columnas Height y Weight - Imputación Segmentada por Deporte. Rellena los valores nulos de Height/Weight con la mediana de Height/Weight calculada solo para el deporte de esa fila.
			
			Columna Age - Imputación Global. Rellena los valores nulos con la mediana de la columna Age completa.
		
		###Definición de variables:
		
			Columnas numéricas  ['Age', 'Height', 'Weight']
			
			Columnas categóricas ['Sex', 'Sport', 'Team']
			
			Creada la matriz de features (X), que contiene todas las variables de entrada combinadas y el vector objetivo (y), que contiene la variable que el modelo debe predecir (1 si ganó medalla, 0 si no).

    	###División en train/test:
		
			Train - 70% de los datos para entrenar el modelo, una división aleatoria.
			
			Test - 30% de los datos para probarlo, división aleatoria.
			
			Dado el desbalance de clases (15% medallas, 85% no medallas) está asegurado que la proporción de medallas vs. no medallas sea la misma en el conjunto de entrenamiento y en el de prueba.
			
			RSEED (42) para garantizar la reproducibilidad del modelo.

##Entrenamiento de modelos

		###Pipeline de scikit-learn - transformaciones que se ejecutan automáticamente en orden.
	
		###Random Forest - 200 árboles predice un resultado (Medalla o No Medalla).
	
##Evaluación del modelo
	
		###Precision (Precisión)	0,27 MUY BAJO. El modelo genera muchísimos Falsos Positivos (atletas que predice que ganan, pero en realidad no lo hacen).
		
		###Recall (Sensibilidad)	0,34 BAJO. De todos los atletas que realmente ganaron medalla (10.226 atletas), el modelo solo fue capaz de identificar correctamente al 34%. El 66% de los verdaderos ganadores se perdieron (Falsos Negativos).
		
		###F1-Score			0,30 MUY BAJO. El modelo tiene serios problemas para identificar correctamente la clase minoritaria.
	
##Conclusiones y comunicación de resultados
		El Random Forest está funcionando como un clasificador perezoso, aprovechando el gran número de no ganadores. Acierta en el 76% de las veces. Sin embargo, en un problema tan desbalanceado, esta métrica es engañosa. Sus dos mayores problemas son:
		
			1.	Demasiadas alarmas falsas (FP): 9.192 falsas predicciones de medalla.
			
			2.	Perdiendo demasiados ganadores (FN): 6.756 ganadores reales que el modelo no pudo ver.
