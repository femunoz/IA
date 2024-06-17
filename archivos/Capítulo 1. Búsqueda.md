Fuente: [HarvardCS50S](https://pll.harvard.edu/course/cs50s-introduction-artificial-intelligence-python)

#### Motivación: Juego del gato o tres en raya

Mostrar que consiste en:
1. un tablero y su respectivo estado
2. buscar una config. ganadora.
#### Conceptos clave:

- **Agent**: percibe su entorno y actúa. Ej: persona.
- **State:** alguna configuración del agente en su entorno. 
- **Initial state (IS):** donde comienza el agente.
	- Qué acciones nos llevarán del IS al GOAL.
- **Actions**: elecciones que pueden realizarse en un estado.
- **Transition model:** descripción del estado _e2_ que resulta de aplicar una acción a un estado _e1_.
- 
#### Modelo Matemático:

##### 1. Actions:

Función $ACTIONS(s)$: $estado$  ->  $\{A_1, A_2, ..., A_n\}$.

- Ej. (dibujar tablero y suponiendo que **parte cruz**): 

$ACTIONS(tableroVacio) = \{cruz_{0,0}, cruz_{1,1}, ..., cruz_{2,2}\}$

- Es necesario modelar tanto estados como acciones.

##### 2. Transition model:

Función $RESULT(s,a) :$ $estado, acción  ->  estado$

	$RESULT(tableroVacio,  cruz_{1,1}) = cruzAlCentro$
	
#### Más conceptos:

##### 1. State space (espacio de estados)

Todos los estados a los que se puede llegar desde el *estado inicial*, siguiendo *todas acciones posibles*.

- **Dibujar** árbol de estado inicial del gato y c/u de sus sub-árboles.
- Se generaliza con teoría de grafos: nodos y aristas.

![[grafo1.png]]

##### 2. Goal test

- Para evaluar si un estado es un _goal state_.
- Puede haber más de un *goal state*.

##### 3. Path cost

- dado un camino a un *goal state*, se puede asociar un valor numérico a este.
- Ej. de grafo con costos en sus **aristas** que unen **nodos**

![[grafo2.png]]

- En estos problemas, además de buscar un GOAL, se busca el camino más corto o más rápido, en general, minimizar el costo de un camino.

#### Problemas de búsqueda

- estado inicial
- acciones
- modelo de transición
- goal test
- función de coste de un camino

**Solución:**
secuencia de acciones de llevan del estado inicial al _goal state_.

**Solución óptima:**
solución que tiene el menor *path cost* entre todas las soluciones.

- Queremos que el computador resuelva un problema -> debemos encontrar una forma de representar los datos de éste.

Utilizaremos una estructura de datos para empaquetar datos relacionados a un estado. A esta la llamamos **nodo**.

**nodo**
- un estado
- su estado padre (quien lo generó)
- una acción (la acción que se aplicó desde el padre para llegar al nodo)
- su path cost (desde el estado inicial al nodo)

##### Enfoque

para resolver el problema, partimos de un estado en particular y vamos a **explorar** desde allí.

dado un estado, tenemos múltiples opciones que podríamos tomar y vamos a explorar esas opciones. Una vez que las exploramos encontraremos más opciones que van a estar disponibles.

vamos a considerar todas estas opciones posibles para almacenarlas en una estructura de datos que llamaremos **frontera**. 

**frontera**: representa todos los nodos que podríamos explorar a continuación, que aún no hemos visitado.

##### Algoritmo:

1. Comenzamos con una frontera que contiene el estado inicial
2. (dejar línea para mejora)
3. Repetimos:
	1. si la frontera está vacía:
		1. no existe una solución
	2. si no lo está:
		1. quitamos un nodo de la frontera (elección arbitraria)
		2. si el nodo contiene un *goal state*:
			1. retornamos la solución
		3. (dejar linea para mejora)
		4. si no:
			1. (dejar linea para mejora)
			2. expandimos el nodo (mirando todos sus vecinos) y se agregan los nodos resultantes a la frontera.


![[CaminoA-E.png]]

**Profesor**: iniciar sesión

https://www.canva.com/design/DAF-k1FYKvU/9NumJgehMM7m9ur8Ic7qjQ/edit?utm_content=DAF-k1FYKvU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

Y si tenemos además una arista desde **B** hacia **A**.

=> Bucle infinito.

**Mejora**: agregamos estructura de datos que guarde los nodos ya visitados y actualizamos el pseudo-algoritmo.

##### Algoritmo:

1. comenzamos con una frontera que contiene el estado inicial
2. (1er. cambio) además con un conjunto de **nodos explorados** vacío 
3. repetimos:
	1. si la frontera está vacía:
		1. no existe una solución
	2. si no lo está:
		1. quitamos un nodo de la frontera **(1)**
		2. si el nodo contiene un *goal state*:
			1. retornamos la solución
		3. (2do. cambio) agregamos este nodo al **conjunto de explorados**
		4. si no:
			1. expandimos el nodo (mirando todos sus vecinos)
			2. (3er cambio) *si estos vecinos **no están** aún en la frontera o el conjunto de explorados*, 
				1. se agregan los nodos resultantes a la frontera.



**Pila (LIFO)**

concentrémonos en **(1)**. Como estruct. de dato para la frontera utilizaremos una **pila LIFO**.

![[Captura de Pantalla 2024-04-26 a la(s) 08.53.13.png]]

---
**Actividad**. En grupos de 1-2 estudiantes.

1. En Canvas **haga una copia** de la pizarra en https://www.canva.com/design/DAF-k1FYKvU/9NumJgehMM7m9ur8Ic7qjQ/edit?utm_content=DAF-k1FYKvU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton. En caso de no tener acceso a un computador con Internet, utilice una hoja de papel.
2. Utilizando **canvas** simule el algoritmo para el mismo grafo anterior, considerando:
	- sólo una arista entre A y B (desde el primer al segundo nodo)
	- una frontera modelada como pila.
	- una estructura de datos que modele los nodos ya visitados.
3. Dos grupos al azar pasarán a pizarra a explicar el algoritmo. Inscriba su grupo aquí:

https://forms.gle/3meG5GNM5iUJbfub9

---
Como se logró ver en esta simulación, la búsqueda se hace llegando hasta un nodo muy profundo en el grafo y volviendo a algún nodo con menor profundidad. A esta estrategia se le denomina **Depth-First Search (DFS)** (búsqueda en profundidad).

---
Existe otra estrategia para la búsqueda, el algoritmo **Breadth-First Search (BFS)** (búsqueda en anchura). La diferencia es que expande el nodo menos profundo de la frontera. Ahora en lugar de una pila o stack, usaremos una cola.

**Cola (FIFO)**

Estruct. de dato que utilizaremos para la frontera. 

![[Captura de Pantalla 2024-04-26 a la(s) 08.57.06.png]]
se expande el primer elemento que agregamos a la frontera.

https://www.canva.com/design/DAF-k1FYKvU/9NumJgehMM7m9ur8Ic7qjQ/edit?utm_content=DAF-k1FYKvU&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton

Todos los nodos a 2 de distancia del inicial, luego los a 3 de distancia, 4, etc.

En la práctica:

https://youtu.be/WbzNRTTrX0g?si=Ib0AHhr1kjg9VY3G&t=1840

Ambos algoritmos encuentran una solución, pero:

**DFS** no es necesariamente **óptimo**.
**BFS** sí

DFS: elijamos un camino hasta agotarlo
BFS: exploremos caminos poco profundos, cercanos a nosotros.

**¿En la práctica?**

Ej: Laberinto

![[Captura de Pantalla 2024-03-19 a la(s) 12.12.08.png]]

Diapositivas del laberinto:

https://drive.google.com/open?id=1yVEUSUqYIOxsKsZVxGY6tirCi1Z6os-X&usp=drive_fs

![[Captura de Pantalla 2024-03-25 a la(s) 09.12.56.png]]

Ya vimos el primer punto donde estos algoritmos tuvieron que **tomar una decisión**: ir a la derecha o a la izquierda.

**DFS (en profundidad)** elige un camino hasta agotarlo y busca el objetivo. Si no está allí, busca por el otro lado.

**BFS (en amplitud)**, siguió otra alternativa: exploremos caminos poco profundos. Cercanos a nosotros 1ero.

Entonces, la búsqueda en **amplitud** (**BFS**) encontró un camino entre A y B, pero necesitó **explorar muchos estados** para lograrlo.

**Trade off** entre DFS y BFS:
- en profundidad puede haber ahorros de memoria

(muestra ejemplo de laberinto en Python)

* En **profundidad** (**DFS**):

![[maze.png]]


* En **amplitud** (**BFS**):

![[maze 1.png]]

**DFS** (profundidad) **no** funcionó de manera **óptima** en este ejemplo. ¿Era necesario visitar más estados?

---
### Mejora

**BFS** (amplitud) parece una buena estrategia, pero en algunos casos tuvo que explorar todo el laberinto para encontrar un camino.

¿Cómo podemos hacer que nuestro algoritmo sea **un poco más inteligente**?


![[Captura de Pantalla 2024-03-25 a la(s) 09.12.56.png]]


**¿Qué haría un humano?**
Quiero llegar a B, que está arriba yendo a la derecha => ir a la derecha debiera ser mejor.

Nos gustaría entonces un algoritmo más inteligente, que sepa que debo avanzar hacia la meta.

---
Ahora, una persona que conoce dónde está la posición objetivo, **intuitivamente** iría a la derecha en el pto. de decisión. 

Es aquí donde tenemos dos tipos de algoritmos: búsqueda desinformada y búsqueda informada.

### Uninformed search
es una estrategia que no usa conocimiento específico del problema, como DFS y BFS.

A BFS y DFS no les importa la forma del laberinto para resolver el problema, sólo ven las alternativas disponibles y eligen entre ellas. Son problemas generales, no sólo laberintos.

---
### Informed Search

utiliza conocimiento específico del problema, así encuentra soluciones más eficientemente.

Ej: en el laberinto es mejor dirigirse por un camino que esté más cerca de la meta.

**Greedy best-first search (GBFS - búsqueda codiciosa del mejor primero)**

expande el nodo más cercano a la meta, mediante una función **heurística** $h(n)$, que **estima** una solución. O que cree que está más cerca de la meta.

**heurística:**
no se sabe _exactamente_ la solución, pero sí es una estimación de esta.

¿En el laberinto, qué destino intermedio es mejor: C o D?

![[Captura de Pantalla 2024-03-25 a la(s) 10.51.31.png]]

D parece mejor al estar más cerca de B 

**distancia de Manhattan**

![[Captura de Pantalla 2024-03-25 a la(s) 10.51.59.png]]


![[Captura de Pantalla 2024-03-25 a la(s) 10.53.09.png]]

Sigamos los pasos: 
**OJO**: al tener que decidir nos vamos por la casilla de menor valor.
![[Captura de Pantalla 2024-04-04 a la(s) 19.03.07.png]]


¿Es **ÓPTIMA**?

![[Captura de Pantalla 2024-03-25 a la(s) 10.54.33.png]]


![[Captura de Pantalla 2024-03-25 a la(s) 10.55.06.png]]

No es necesariamente óptimo. En el ej. hay una solución mejor, ie, que toma menos pasos.

Elige la mejor solución localmente en lugar de la general. 

la siguiente heurística es una mejora a GBFS, que busca ser óptimo.

**A* search***

expande el nodo con el menor valor de 

$f(n) = g(n) + h(n)$

$h(n) :$ costo estimado a la meta
$g(n) :$ costo para llegar a un nodo (pasos para llegar a $n$)

![[Captura de Pantalla 2024-04-04 a la(s) 19.12.01.png]]

![[Captura de Pantalla 2024-04-04 a la(s) 19.18.23.png]]


Toma una decisión basada en la suma:

$n\_pasos\_desde\_origen + estimación\_distancia\_meta$

$\forall$ nodo $n$ y sucesor $n'$, cuya arista tiene costo $c$, 

$h(n)\leq h(n')+c$

### Búsqueda con adversario

Hasta ahora hemos buscado soluciones a problemas con sólo un agente que buscar resolver un laberinto, un puzzle, encontrar un camino óptimo, etc.

¿Qué pasa ahora si tenemos un agente que se enfrenta a otro? 

Un agente adversario tiene un objetivo opuesto a otro agente.

Ejemplo: el gato (tres en raya o tic-tac-toe)


#InteligenciaArtificial 
#IA 
#docencia
#capacitaciones
