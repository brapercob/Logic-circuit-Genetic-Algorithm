Readme.txt
-------------------------------------------------------------------------------------------------
ESTRUCTURA DEL C�DIGO:
	
Objetivo espec�fico 1 -  Estructura de datos y selecci�n de circuito.
	

-creaCircuito(capas, puertas):

		M�todo que recibe un n�mero de capas y un n�mero de puertas por capa que genera un circuito
		con la estructura de datos (N� Puerta, Tipo de puerta, Capa, [conexi�n1, conexi�n2]) .
		
-asignaConexiones(puertas, capa):

		M�todo auxiliar a creaCircuito que recibe el n�mero de puertas por capa y la capa de la puerta
		que llama al m�todo y devuelve un par de conexiones [conexi�n1, conexi�n2] siguiendo la pauta
		de seleccionar puertas de hasta 2 capas anteriores.

*Aqu� se incluye una porci�n de c�digo para generar y almacenar un circuito soluci�n.


Objetivo espec�fico 2 -  Mecanismo simulador de circuitos.


-simularCircuito(Circuito, vector_entrada, conexiones_defectuosas, puertas_defectuosas):
	
		M�todo que recibe un circuito, un vector entrada con tantos valores como puertas por capa, y un 
		conjunto de conexiones y puertas defectuosas para aplicar al circuito. Devuelve el vector salida 
		correspondiente a la simulaci�n.

-calculaSalida(entrada1, entrada2, tipoPuerta):	

		M�todo auxiliar a simularCircuito que recibe los dos valores de entrada para una puerta (0,1) y el 
		tipo de puerta y devuelve la salida seg�n estos 3 par�metros.


Objetivo espec�fico 3 -  Colecci�n de pares y auto-diagn�stico.


-coleccionPares(circuito, conexiones_defectuosas, puertas_defectuosas): 	
		
		M�todo que recibe un circuito y dos conjuntos, de puertas y conexiones defectuosas, devolviendo 
		la colecci�n de pares [entrada, salida] (colecci�n de tama�o 2^n� Puertas), haciendo uso del 
		m�todo simularCircuito del apartado anterior.

-auto_diagnostico(circuito, circuito2, conexiones_defectuosas, puertas_defectuosas):
 
		M�todo que recibe 2 circuitos y dos conjuntos, de puertas y conexiones defectuosas, devolviendo 
		un n�mero de fallos obtenido de compararando las colecciones de pares de referencia de ambos
		circuitos pudiendo establecer defectos en el segundo circuito.


Objetivo espec�fico 4 -  Algoritmo gen�tico.

*En primer lugar se importa todo lo necesario para la utilizaci�n del algoritmo
 
*Se define las funciones fitness e individuo

*Se crea la caja de herramientas para almacenar todas la funciones del algoritmo.

-creaGenes(): 	

		M�todo que genera un gen (tipo de puerta, conexion1, conexion2) teniendo en cuenta la capa y el n�mero
		de puerta para evitar genes no factibles en los genotipos.

-numeroCapa(contador): 	
		
		M�todo auxiliar para, seg�n el n�mero de puerta que se pasa como par�metro devolver la capa donde se 
		encuentra. 

-counter(func): 	

		M�todo decorator auxiliar a creaGenes para llevar la cuenta de veces que se llama al m�todo y actue como 
		n�mero de puerta, ya que no es posible llevar un contador dentro del propio m�todo.(C�digo obtenido de
		Internet referenciado en el documento).

 *Se a�ade la funci�n gen a la toolbox con creaGenes.

*Registramos individuo en la toolbox especificando el n�mero de genes del que se compone cada individuo.

*Registramos poblacion en la toolbox especificando el n�mero de individuos por poblaci�n.

-fenotipo(individuo): 	
		
		M�todo que genera los fenotipos que optan a ser soluci�n a partir de los genotipos creados en la poblaci�n
		traduciendo los genes a sus respectivos campos en la estructura de datos de un circuito.

-tipo_puerta(numero):	

		M�todo auxiliar a fenotipo() para traducir el n�mero de tipo de puerta (gen) al nombre de tipo de puerta.

-evaluar_individuo(individuo): 	

		M�todo que evalua individuos de una poblaci�n utilizando el m�todo auto_diagnostico() del apartado 3
		devolviendo el n�mero de errores (fitness).

*Registramos la funci�n evaluate en la toolbox.

*En las 3 siguiente celdas se registran respectivamente los operadores de cruzamiento de recombinaci�n en 2 puntos, 1 punto y uniforme.

-creaMargenes():	 

		Funci�n auxiliar que genera las secuencias de m�rgenes superior e inferior necesarias para el operador de mutaci�n. 

*Registramos el operador de mutaci�n uniforme

*Registramos en la toolbox las estrategias de selecci�n por torneo de 2 participantes y elitista.

*Registramos en la toolbox las funciones para generar estad�sticas usando numpy,  el sal�n de fama para obtener los mejores fenotipos
   y la aplicaci�n del algoritmo gen�tico simple haciendo uso de una poblaci�n, la caja de herramientas, probabilidades de cruzamiento y 
   mutaci�n, n�mero de generaciones, estad�sticas y salon de la fama. (C�digo reutilizado de la pr�ctica 4 referenciado en el documento).


Objetivo espec�fico 5 -  Experimentaci�n.

*A partir de aqu� se incluyen ejemplos de la experimentaci�n realizada siguiendo la siguiente estructura: print(estad�sticas), print(mejor 
   soluci�n encontrada), print(circuito soluci�n inicial), print(colecciones de salidas de ambos circuitos), print(fitness) y print(rendimiento).


------------------------------------------------------------------------------------------------------------------------------------------------------------------


GU�A DE USO -  OBJETIVOS 1-4 Y EXPERIMENTACI�N: (No se incluir�n ejemplos de uso de m�todos auxiliares ya que estos no se necesitan ejecutar de 					          manera independiente a sus respectivos m�todos asociados)	
Objetivo espec�fico 1:
	
-creaCircuitos():	

		Para este m�todo unicamente es necesario pasar como par�metros el n�mero de capas y el n�mero de puertas por capa.
		
		Traza de ejemplo: 
			
			Entrada:
			circuito = creaCircuitos(3, 3)
			print(circuito)
			
			Salida:		
			[(1, 'NAND', 1, [(0, 1), (0, 1)]), (2, 'XOR', 1, [(0, 2), (0, 2)]), (3, 'XNOR', 1, [(0, 3), (0, 3)]),
			 (4, 'NOT', 2, [(0, 4), (0, 4)]), (5, 'NOT', 2, [(0, 5), (3, 5)]), (6, 'NOT', 2, [(0, 6), (2, 6)]), 
			(7, 'XOR', 3, [(1, 7), (3, 7)]), (8, 'XNOR', 3, [(0, 8), (4, 8)]), (9, 'OR', 3, [(4, 9), (0, 9)])]

***Usaremos este circuito a partir de ahora en las trazas de los siguientes m�todos

-simularCircuito():

		Ser� necesario un circuito (como el generado por creaCircuitos(), un vector entrada de la forma [n1, n2, ... ,nT] para T igual 
		al n�mero de puertas por capa del circuito y con valores 0 � 1, un conjunto de conexiones defectuosas de la forma [(p1, p2)]
		con p1 puerta de donde se recibe se�al y p2 puerta que recibe la se�al, por �ltimo un conjunto de puertas defectuosas de la 
		forma [N1, N2, Nn] siendo N el n�mero de la puerta.

		Traza de ejemplo:
			
			Entrada:
			print(simularCircuito(circuito, [0,1,0], [(1,7)], [5])) 

			Salida:
			[1, 0, 1]
			
-coleccionPares():
		
		Para este m�todo es necesario un circuito y dos coleciones de puertas y conexiones defectuosas siguiendo la estructura de datos
		definida en el m�todo anterior.
		
		Traza de ejemplo:
			
			Entrada:
			print(coleccionPares(CircuitoSolucion, [(1,7)], [5]))  

			Salida:
			[((0, 0, 0), [1, 0, 1]), ((0, 0, 1), [1, 0, 1]), ((0, 1, 0), [1, 0, 1]),
			 ((0, 1, 1), [1, 0, 1]), ((1, 0, 0), [1, 0, 1]), ((1, 0, 1), [1, 0, 1]),
			 ((1, 1, 0), [1, 0, 1]), ((1, 1, 1), [1, 0, 1])]


-auto_diagnostico():
	
		Para este m�todo debemos introducir los dos circuitos que queramos comparar, puediendo ser el mismo circuito, al primero 
		no se le aplicar�n los defectos y al segundo si o pueden ser circuitos diferentes y el segundo puede o no estar da�ado seg�n 
		si introducimos valores en la colecciones de defectos o no.
		
		Traza de ejemplo: Mismo circuito, primero sin errores segundo con defectos.
		
			Entrada:
			print(auto_diagnostico(circuito, circuito,  [(1,7)], [5])) 
		
			Salida:
			4


-creaGenes():

		Aunque este m�todo no lo utilizaremos de forma independiente podemos probar su funcionamiento.

		Traza de ejemplo:
			
			Entrada:
			print(caja_de_herramientas.gen())

			Salida:
			6

* De igual manera podemos probar las funciones de la toolbox caja_de_herramientas.individuo() o caja_de_herramientas.poblacion() que devolveran un     genotipo y un conjunto de genotipos respectivamente.


-fenotipo():

		Este m�todo tampoco lo usamos de manera indenpendiente pero se puede probar su funcionalidad pasando como par�metro
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

		Podemos probar su funcionalidad pasando como par�metro un genotipo.

		Traza de ejemplo:
			
			Entrada:
			genotipo = caja_de_herramientas.individuo()
			print(evaluar_individuo(genotipo))

			Salida:		
			[14]

***A partir de aqu� se declaran operadores y estrategias para la configuraci�n del algoritmo pudiendo seleccionar entre diferentes opciones,
        finalmente el resultado de la aplicaci�n del algoritmo se puede probar mediante la declaraci�n de algorithms.eaSimple().

REPRODUCCI�N  Y DISE�O DE EXPERIMENTOS:

        Para reproducir los experimentos realizados unicamente hay que seguir los siguientes pasos: 
	1� Descomentar en la celda donde se declaran los circuitos, el circuito sobre el cual se quiere aplicar el algoritmo 
	      (IMPORTANTE introducir por consola el n�mero de capas y puertas por capa del circuito del que se quiere experimentar).
	2� Seguidamente ejecutamos las siguientes celdas, pudiendo modificar los individuos por poblaci�n, hasta llegar a la selecci�n de operadores de cruzamiento 	      donde ejecutaremos
        	      solo el que se quiera utilizar. 
	3�  Ejecutamos el registro del operador de mutaci�n.
	4�  Ejecutamos la estrategia de selecci�n que se prefiera.
	5�  Ejecutamos la celda del algorithms.eaSimple, puediendo modificar el n�mero de generaciones, y observamos los resultados.

        Para el dise�o de nuevos experimentos solo tendriamos que descomentar la l�nea 'CircuitoSolucion = creaCircuito(capas, puertas)' de la celda de
        creaci�n de circuitos, comentado el restos de circuitos ejemplo que aparecen posteriormente e introduciendo por consola capas y puertas.         Seguidamente ejecutamos los pasos 2�-5� del apartado anterior.
	
        Por lo tanto para probar la ejecuci�n del algortimo solo har�a falta crear un circuitoSoluci�n ejecutar los operadores de cruzamineto y mutaci�n,                  estrategias que se quieran utilizar y ejecutar el siguiente comando para obtener resultados como este:
		
		Traza de ejemplo:
			
			Entrada:
			random.seed(12345)  ##Seed modificable
			poblacion_inicial = caja_de_herramientas.poblacion()
			poblacion, registro = algorithms.eaSimple(poblacion_inicial,
                                 	        					caja_de_herramientas,
                                         						cxpb=0.5, # Probabilidad de que dos individuos contiguos se crucen
                                         						mutpb=0.3, # Probabilidad de que un individuo mute
                                          						ngen=20, # N�mero de generaciones
                                          						stats=estad�sticas,
                                          						halloffame=sal�n_fama)

			Salida:
			gen	nevals	m�nimo	media	m�ximo
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


***Como informaci�n complementaria a esto tenemos la serie de prints que aparecen seguidos de la traza anterior, para el anterior ejemplo obtendriamos:
		
			Entrada:
			print('-----------------------------------------------')
			print('La mejor soluci�n encontrada ha sido:')
			for individuo in sal�n_fama:
  				print('-----------------------------------------------')
    				print('Individuo: {1}; Fitness: {0}'.format(
        					individuo.fitness.values[0], fenotipo(individuo)))
			print('-----------------------------------------------')
			print('Vamos a comprobar que efectivamente las salidas del circuito concuerdan'
      			+' con las del circuito soluci�n.')
			print('-----------------------------------------------')
			print('Este es nuestro circuito soluci�n:')
			print(CircuitoSolucion)
			print('-----------------------------------------------')
			col1 = coleccionPares(CircuitoSolucion,[],[])
			fenot = sal�n_fama[0]
			circuitoAlternativo = fenotipo(fenot)
			col2 = coleccionPares(circuitoAlternativo,[],[])
			print('Estas son las tuplas de salida de los circuitos.')
			for n in range(len(col1)): 
   				print(col1[n][1], col2[n][1])    
    
			print('-----------------------------------------------')
			print('Hemos obtenido un circuito alternativo con {0} '
			      'errores \nrespecto al circuito soluci�n'.format(individuo.fitness.values[0]))
			print('-----------------------------------------------')
			print('Por lo tanto el rendimiento de nuestro algoritmo ha sido del {:.1f}%'
      				.format((((pow(2,puertas)*puertas)-sal�n_fama[0].fitness.values[0])/(pow(2,puertas)*puertas))*100))

			Salida:
			-----------------------------------------------
			La mejor soluci�n encontrada ha sido:
			-----------------------------------------------
			Individuo: [(1, 'XOR', 1, [(0, 1), (0, 1)]), (2, 'XOR', 1, [(0, 2), (0, 2)]), (3, 'NOR', 1, [(0, 3), (0, 3)]),
 			(4, 'NOR', 2, [(2, 4), (3, 4)]), (5, 'OR', 2, [(2, 5), (3, 5)]), (6, 'AND', 2, [(1, 6), (1, 6)]), (7, 'OR', 3, [(2, 7), (2, 7)]),
			 (8, 'AND', 3, [(3, 8), (1, 8)]), (9, 'AND', 3, [(4, 9), (5, 9)]), (10, 'NOR', 4, [(7, 10), (4, 10)]), (11, 'NAND', 4, [(7, 11), (8,11)]),
 			(12, 'NAND', 4, [(5, 12), (0, 12)])]; Fitness: 4.0
			-----------------------------------------------
			Vamos a comprobar que efectivamente las salidas del circuito concuerdan con las del circuito soluci�n.
			-----------------------------------------------
			Este es nuestro circuito soluci�n:
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
			respecto al circuito soluci�n
			-----------------------------------------------
			Por lo tanto el rendimiento de nuestro algoritmo ha sido del 83.3%






	

