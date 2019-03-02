Readme.txt
-------------------------------------------------------------------------------------------------
ESTRUCTURA DEL CÓDIGO:
	
Objetivo específico 1 -  Estructura de datos y selección de circuito.
	

-creaCircuito(capas, puertas):

		Método que recibe un número de capas y un número de puertas por capa que genera un circuito
		con la estructura de datos (Nº Puerta, Tipo de puerta, Capa, [conexión1, conexión2]) .
		
-asignaConexiones(puertas, capa):

		Método auxiliar a creaCircuito que recibe el número de puertas por capa y la capa de la puerta
		que llama al método y devuelve un par de conexiones [conexión1, conexión2] siguiendo la pauta
		de seleccionar puertas de hasta 2 capas anteriores.

*Aquí se incluye una porción de código para generar y almacenar un circuito solución.


Objetivo específico 2 -  Mecanismo simulador de circuitos.


-simularCircuito(Circuito, vector_entrada, conexiones_defectuosas, puertas_defectuosas):
	
		Método que recibe un circuito, un vector entrada con tantos valores como puertas por capa, y un 
		conjunto de conexiones y puertas defectuosas para aplicar al circuito. Devuelve el vector salida 
		correspondiente a la simulación.

-calculaSalida(entrada1, entrada2, tipoPuerta):	

		Método auxiliar a simularCircuito que recibe los dos valores de entrada para una puerta (0,1) y el 
		tipo de puerta y devuelve la salida según estos 3 parámetros.


Objetivo específico 3 -  Colección de pares y auto-diagnóstico.


-coleccionPares(circuito, conexiones_defectuosas, puertas_defectuosas): 	
		
		Método que recibe un circuito y dos conjuntos, de puertas y conexiones defectuosas, devolviendo 
		la colección de pares [entrada, salida] (colección de tamaño 2^nº Puertas), haciendo uso del 
		método simularCircuito del apartado anterior.

-auto_diagnostico(circuito, circuito2, conexiones_defectuosas, puertas_defectuosas):
 
		Método que recibe 2 circuitos y dos conjuntos, de puertas y conexiones defectuosas, devolviendo 
		un número de fallos obtenido de compararando las colecciones de pares de referencia de ambos
		circuitos pudiendo establecer defectos en el segundo circuito.


Objetivo específico 4 -  Algoritmo genético.

*En primer lugar se importa todo lo necesario para la utilización del algoritmo
 
*Se define las funciones fitness e individuo

*Se crea la caja de herramientas para almacenar todas la funciones del algoritmo.

-creaGenes(): 	

		Método que genera un gen (tipo de puerta, conexion1, conexion2) teniendo en cuenta la capa y el número
		de puerta para evitar genes no factibles en los genotipos.

-numeroCapa(contador): 	
		
		Método auxiliar para, según el número de puerta que se pasa como parámetro devolver la capa donde se 
		encuentra. 

-counter(func): 	

		Método decorator auxiliar a creaGenes para llevar la cuenta de veces que se llama al método y actue como 
		número de puerta, ya que no es posible llevar un contador dentro del propio método.(Código obtenido de
		Internet referenciado en el documento).

 *Se añade la función gen a la toolbox con creaGenes.

*Registramos individuo en la toolbox especificando el número de genes del que se compone cada individuo.

*Registramos poblacion en la toolbox especificando el número de individuos por población.

-fenotipo(individuo): 	
		
		Método que genera los fenotipos que optan a ser solución a partir de los genotipos creados en la población
		traduciendo los genes a sus respectivos campos en la estructura de datos de un circuito.

-tipo_puerta(numero):	

		Método auxiliar a fenotipo() para traducir el número de tipo de puerta (gen) al nombre de tipo de puerta.

-evaluar_individuo(individuo): 	

		Método que evalua individuos de una población utilizando el método auto_diagnostico() del apartado 3
		devolviendo el número de errores (fitness).

*Registramos la función evaluate en la toolbox.

*En las 3 siguiente celdas se registran respectivamente los operadores de cruzamiento de recombinación en 2 puntos, 1 punto y uniforme.

-creaMargenes():	 

		Función auxiliar que genera las secuencias de márgenes superior e inferior necesarias para el operador de mutación. 

*Registramos el operador de mutación uniforme

*Registramos en la toolbox las estrategias de selección por torneo de 2 participantes y elitista.

*Registramos en la toolbox las funciones para generar estadísticas usando numpy,  el salón de fama para obtener los mejores fenotipos
   y la aplicación del algoritmo genético simple haciendo uso de una población, la caja de herramientas, probabilidades de cruzamiento y 
   mutación, número de generaciones, estadísticas y salon de la fama. (Código reutilizado de la práctica 4 referenciado en el documento).


Objetivo específico 5 -  Experimentación.

*A partir de aquí se incluyen ejemplos de la experimentación realizada siguiendo la siguiente estructura: print(estadísticas), print(mejor 
   solución encontrada), print(circuito solución inicial), print(colecciones de salidas de ambos circuitos), print(fitness) y print(rendimiento).


------------------------------------------------------------------------------------------------------------------------------------------------------------------


GUÍA DE USO -  OBJETIVOS 1-4 Y EXPERIMENTACIÓN: (No se incluirán ejemplos de uso de métodos auxiliares ya que estos no se necesitan ejecutar de 					          manera independiente a sus respectivos métodos asociados)	
Objetivo específico 1:
	
-creaCircuitos():	

		Para este método unicamente es necesario pasar como parámetros el número de capas y el número de puertas por capa.
		
		Traza de ejemplo: 
			
			Entrada:
			circuito = creaCircuitos(3, 3)
			print(circuito)
			
			Salida:		
			[(1, 'NAND', 1, [(0, 1), (0, 1)]), (2, 'XOR', 1, [(0, 2), (0, 2)]), (3, 'XNOR', 1, [(0, 3), (0, 3)]),
			 (4, 'NOT', 2, [(0, 4), (0, 4)]), (5, 'NOT', 2, [(0, 5), (3, 5)]), (6, 'NOT', 2, [(0, 6), (2, 6)]), 
			(7, 'XOR', 3, [(1, 7), (3, 7)]), (8, 'XNOR', 3, [(0, 8), (4, 8)]), (9, 'OR', 3, [(4, 9), (0, 9)])]

***Usaremos este circuito a partir de ahora en las trazas de los siguientes métodos

-simularCircuito():

		Será necesario un circuito (como el generado por creaCircuitos(), un vector entrada de la forma [n1, n2, ... ,nT] para T igual 
		al número de puertas por capa del circuito y con valores 0 ó 1, un conjunto de conexiones defectuosas de la forma [(p1, p2)]
		con p1 puerta de donde se recibe señal y p2 puerta que recibe la señal, por último un conjunto de puertas defectuosas de la 
		forma [N1, N2, Nn] siendo N el número de la puerta.

		Traza de ejemplo:
			
			Entrada:
			print(simularCircuito(circuito, [0,1,0], [(1,7)], [5])) 

			Salida:
			[1, 0, 1]
			
-coleccionPares():
		
		Para este método es necesario un circuito y dos coleciones de puertas y conexiones defectuosas siguiendo la estructura de datos
		definida en el método anterior.
		
		Traza de ejemplo:
			
			Entrada:
			print(coleccionPares(CircuitoSolucion, [(1,7)], [5]))  

			Salida:
			[((0, 0, 0), [1, 0, 1]), ((0, 0, 1), [1, 0, 1]), ((0, 1, 0), [1, 0, 1]),
			 ((0, 1, 1), [1, 0, 1]), ((1, 0, 0), [1, 0, 1]), ((1, 0, 1), [1, 0, 1]),
			 ((1, 1, 0), [1, 0, 1]), ((1, 1, 1), [1, 0, 1])]


-auto_diagnostico():
	
		Para este método debemos introducir los dos circuitos que queramos comparar, puediendo ser el mismo circuito, al primero 
		no se le aplicarán los defectos y al segundo si o pueden ser circuitos diferentes y el segundo puede o no estar dañado según 
		si introducimos valores en la colecciones de defectos o no.
		
		Traza de ejemplo: Mismo circuito, primero sin errores segundo con defectos.
		
			Entrada:
			print(auto_diagnostico(circuito, circuito,  [(1,7)], [5])) 
		
			Salida:
			4


-creaGenes():

		Aunque este método no lo utilizaremos de forma independiente podemos probar su funcionamiento.

		Traza de ejemplo:
			
			Entrada:
			print(caja_de_herramientas.gen())

			Salida:
			6

* De igual manera podemos probar las funciones de la toolbox caja_de_herramientas.individuo() o caja_de_herramientas.poblacion() que devolveran un     genotipo y un conjunto de genotipos respectivamente.


-fenotipo():

		Este método tampoco lo usamos de manera indenpendiente pero se puede probar su funcionalidad pasando como parámetro
		un genotipo.

		Traza de ejemplo:
			
			Entrada:
			genotipo = caja_de_herramientas.individuo()
			print(fenotipo(genotipo))

			Salida:
			[(1, 'OR', 1, [(0, 1), (0, 1)]), (2, 'AND', 1, [(0, 2), (0, 2)]), (3, 'NOR', 1, [(0, 3), (0, 3)]),
			 (4, 'AND', 2, [(0, 4), (0, 4)]), (5, 'NAND', 2, [(1, 5), (2, 5)]), (6, 'NOR', 2, [(1, 6), (2, 6)]), 
			(7, 'AND', 3, [(1, 7), (3, 7)]), (8, 'NAND', 3, [(0, 8), (5, 8)]), (9, 'NAND', 3, [(2, 9), (3, 9)])]


-evaluar_individuo():

		Podemos probar su funcionalidad pasando como parámetro un genotipo.

		Traza de ejemplo:
			
			Entrada:
			genotipo = caja_de_herramientas.individuo()
			print(evaluar_individuo(genotipo))

			Salida:		
			[14]

***A partir de aquí se declaran operadores y estrategias para la configuración del algoritmo pudiendo seleccionar entre diferentes opciones,
        finalmente el resultado de la aplicación del algoritmo se puede probar mediante la declaración de algorithms.eaSimple().

REPRODUCCIÓN  Y DISEÑO DE EXPERIMENTOS:

        Para reproducir los experimentos realizados unicamente hay que seguir los siguientes pasos: 
	1º Descomentar en la celda donde se declaran los circuitos, el circuito sobre el cual se quiere aplicar el algoritmo 
	      (IMPORTANTE introducir por consola el número de capas y puertas por capa del circuito del que se quiere experimentar).
	2º Seguidamente ejecutamos las siguientes celdas, pudiendo modificar los individuos por población, hasta llegar a la selección de operadores de cruzamiento 	      donde ejecutaremos
        	      solo el que se quiera utilizar. 
	3º  Ejecutamos el registro del operador de mutación.
	4º  Ejecutamos la estrategia de selección que se prefiera.
	5º  Ejecutamos la celda del algorithms.eaSimple, puediendo modificar el número de generaciones, y observamos los resultados.

        Para el diseño de nuevos experimentos solo tendriamos que descomentar la línea 'CircuitoSolucion = creaCircuito(capas, puertas)' de la celda de
        creación de circuitos, comentado el restos de circuitos ejemplo que aparecen posteriormente e introduciendo por consola capas y puertas.         Seguidamente ejecutamos los pasos 2º-5º del apartado anterior.
	
        Por lo tanto para probar la ejecución del algortimo solo haría falta crear un circuitoSolución ejecutar los operadores de cruzamineto y mutación,                  estrategias que se quieran utilizar y ejecutar el siguiente comando para obtener resultados como este:
		
		Traza de ejemplo:
			
			Entrada:
			random.seed(12345)  ##Seed modificable
			poblacion_inicial = caja_de_herramientas.poblacion()
			poblacion, registro = algorithms.eaSimple(poblacion_inicial,
                                 	        					caja_de_herramientas,
                                         						cxpb=0.5, # Probabilidad de que dos individuos contiguos se crucen
                                         						mutpb=0.3, # Probabilidad de que un individuo mute
                                          						ngen=20, # Número de generaciones
                                          						stats=estadísticas,
                                          						halloffame=salón_fama)

			Salida:
			gen	nevals	mínimo	media	máximo
			0  	20    	6     	12.05	16    
			1  	15    	4     	11.55	16    
			2  	13    	4     	11.3 	16    
			3  	15    	4     	11.65	16    
			4  	13    	6     	11.7 	16    
			5  	12    	6     	11.85	18    
			6  	11    	6     	11.75	18    
			7  	16    	6     	11.9 	20    
			8  	15    	8     	12   	18    
			9  	15    	6     	11.7 	16    
			10 	14    	6     	13.05	16    
			11 	15    	4     	12.6 	17    
			12 	9     	7     	13.5 	18    
			13 	10    	7     	13.5 	18    
			14 	13    	4     	13.2 	18    
			15 	10    	5     	12.9 	18    
			16 	12    	8     	12.7 	16    
			17 	13    	8     	12.65	18    
			18 	13    	8     	13   	16    
			19 	10    	8     	13.6 	20    
			20 	10    	12    	14.15	16


***Como información complementaria a esto tenemos la serie de prints que aparecen seguidos de la traza anterior, para el anterior ejemplo obtendriamos:
		
			Entrada:
			print('-----------------------------------------------')
			print('La mejor solución encontrada ha sido:')
			for individuo in salón_fama:
  				print('-----------------------------------------------')
    				print('Individuo: {1}; Fitness: {0}'.format(
        					individuo.fitness.values[0], fenotipo(individuo)))
			print('-----------------------------------------------')
			print('Vamos a comprobar que efectivamente las salidas del circuito concuerdan'
      			+' con las del circuito solución.')
			print('-----------------------------------------------')
			print('Este es nuestro circuito solución:')
			print(CircuitoSolucion)
			print('-----------------------------------------------')
			col1 = coleccionPares(CircuitoSolucion,[],[])
			fenot = salón_fama[0]
			circuitoAlternativo = fenotipo(fenot)
			col2 = coleccionPares(circuitoAlternativo,[],[])
			print('Estas son las tuplas de salida de los circuitos.')
			for n in range(len(col1)): 
   				print(col1[n][1], col2[n][1])    
    
			print('-----------------------------------------------')
			print('Hemos obtenido un circuito alternativo con {0} '
			      'errores \nrespecto al circuito solución'.format(individuo.fitness.values[0]))
			print('-----------------------------------------------')
			print('Por lo tanto el rendimiento de nuestro algoritmo ha sido del {:.1f}%'
      				.format((((pow(2,puertas)*puertas)-salón_fama[0].fitness.values[0])/(pow(2,puertas)*puertas))*100))

			Salida:
			-----------------------------------------------
			La mejor solución encontrada ha sido:
			-----------------------------------------------
			Individuo: [(1, 'XOR', 1, [(0, 1), (0, 1)]), (2, 'XOR', 1, [(0, 2), (0, 2)]), (3, 'NOR', 1, [(0, 3), (0, 3)]),
 			(4, 'NOR', 2, [(2, 4), (3, 4)]), (5, 'OR', 2, [(2, 5), (3, 5)]), (6, 'AND', 2, [(1, 6), (1, 6)]), (7, 'OR', 3, [(2, 7), (2, 7)]),
			 (8, 'AND', 3, [(3, 8), (1, 8)]), (9, 'AND', 3, [(4, 9), (5, 9)]), (10, 'NOR', 4, [(7, 10), (4, 10)]), (11, 'NAND', 4, [(7, 11), (8,11)]),
 			(12, 'NAND', 4, [(5, 12), (0, 12)])]; Fitness: 4.0
			-----------------------------------------------
			Vamos a comprobar que efectivamente las salidas del circuito concuerdan con las del circuito solución.
			-----------------------------------------------
			Este es nuestro circuito solución:
			[(1, 'NOT', 1, [(0, 1), (0, 1)]), (2, 'NOT', 1, [(0, 2), (0, 2)]), (3, 'NOT', 1, [(0, 3), (0, 3)]), 
			(4, 'AND', 2, [(0, 4), (2, 4)]), (5, 'AND', 2, [(1, 5), (3, 5)]), (6, 'AND', 2, [(0, 6), (1, 6)]), 
			(7, 'AND', 3, [(1, 7), (3, 7)]), (8, 'XOR', 3, [(5, 8), (2, 8)]), (9, 'XNOR', 3, [(3, 9), (6, 9)]), 
			(10, 'XNOR', 4, [(4, 10), (9, 10)]), (11, 'NAND', 4, [(7, 11), (9, 11)]), (12, 'OR', 4, [(8, 12), (6, 12)])]
			-----------------------------------------------
			Estas son las tuplas de salida de los circuitos.
			[1, 1, 0] [1, 1, 1]
			[0, 1, 1] [0, 1, 1]
			[1, 1, 1] [1, 1, 1]
			[0, 1, 0] [0, 1, 1]
			[1, 1, 1] [1, 1, 1]
			[0, 1, 1] [0, 1, 1]
			[1, 1, 0] [1, 1, 1]
			[0, 1, 0] [0, 1, 1]
			-----------------------------------------------
			Hemos obtenido un circuito alternativo con 4.0 errores 
			respecto al circuito solución
			-----------------------------------------------
			Por lo tanto el rendimiento de nuestro algoritmo ha sido del 83.3%






	

