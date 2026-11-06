---
course_id: ISIL-30710
session_id: S13
source_origin: PPT
status: revised
---

# Grafos en Java — Guía del Estudiante

**Lideratec Academy · Algoritmo y Estructura de Datos Basado en IA · Sesión 13**

## FICHA DE LA SESIÓN 13 — QUÉ ESTUDIAREMOS Y PARA QUÉ

**Curso:** Algoritmo y Estructura de Datos Basado en IA (ISIL 30710).  
**Tema de la sesión:** **GRAFOS — definición y representación; terminología; implementación en Java; recorridos DFS y BFS; búsqueda heurística A*; aplicaciones en IA básica.**  
**Duración presencial:** **120 minutos (2 horas)**, con ejercicios complementarios autónomos.  
**Modalidad de trabajo:** explicación breve, construcción de código por microbloques, traza manual, ejecución, interpretación y reto espejo.

### Objetivo general de la sesión

**Representar y recorrer grafos pequeños con Java, explicar las decisiones de cada implementación y validar sus resultados**, para resolver problemas de conectividad, búsqueda de rutas y relaciones entre entidades.

### Objetivos específicos: qué conseguirás al finalizar

1. **Identificar** vértices, aristas y tipos de grafo, y explicar por qué no es suficiente almacenar solo nombres de nodos.
2. **Construir y comparar** una matriz y una lista de adyacencia, reconociendo cómo se registran aristas dirigidas y no dirigidas.
3. **Interpretar** grados, caminos y ciclos a partir de conexiones concretas.
4. **Implementar y trazar** DFS con marcas de visita, comprendiendo el efecto de la recursividad.
5. **Implementar y trazar** BFS con una cola FIFO y justificar distancias mínimas en grafos no ponderados.
6. **Aplicar** A* a una cuadrícula de costo unitario, distinguiendo el costo real `g`, la heurística Manhattan `h` y la prioridad `f = g + h`.
7. **Relacionar** un grafo bipartito con un recomendador básico de usuarios y productos, defendiendo cómo se calculan las sugerencias.

### Producto final y evidencia de aprendizaje

Al finalizar la ruta completa dispondrás de **ocho programas Java verificables (EJ01–EJ08)** y **ocho tareas espejo (T01–T08)**. En cada ejemplo deberás poder contestar: *¿por qué se escribió esta instrucción, qué dato cambia, qué ocurriría si la omitiera y cómo confirmo el resultado?* La evidencia consiste en código que compila, salida observada y una explicación de las decisiones. En las 2 horas se trabajan los ejemplos centrales; los restantes se desarrollan como práctica de profundización autónoma.

### Ruta de aprendizaje de la sesión

**Grafo y conexiones → almacenamiento (matriz/lista) → relaciones dirigidas → DFS → BFS y distancias → ciclos → A* → recomendación con grafos.**

| Ejemplo | Qué se construye | Evidencia visible |
|---|---|---|
| EJ01 | Matriz 4×4 no dirigida | Dos celdas simétricas por arista |
| EJ02 | Lista de vecinos con LinkedList | Listas con únicamente vecinos existentes |
| EJ03 | Grafo dirigido | Grados de entrada y salida separados |
| EJ04 | Recorrido DFS | Orden A B D C y marcas de visita |
| EJ05 | Recorrido BFS | Distancias por niveles desde A |
| EJ06 | Detección de ciclo | `tieneCiclo=true` en un triángulo |
| EJ07 | Ruta A* en un grid | Costo y camino sin obstáculos |
| EJ08 | Recomendación con grafo bipartito | Productos nuevos con puntaje justificado |

**Criterio de éxito:** no basta ejecutar el programa: debes ser capaz de explicar el papel de cada variable, decisión o condición relevante de los microbloques y justificar el resultado con un ejemplo concreto.

---


## 1. Propósito de la sesión — apertura para leer en clase

Imagina que una aplicación debe representar calles entre lugares, relaciones entre personas o recomendaciones entre usuarios y productos. Guardar una lista de nombres no resuelve el problema: hace falta saber qué entidades se conectan y cuál es el sentido o el costo de cada vínculo. Esa es precisamente la función de un grafo. Un vértice identifica una entidad y una arista modela la relación; según el caso, la conexión puede ser dirigida o no dirigida, ponderada o no ponderada.

En esta sesión empezaremos por dos formas concretas de almacenar relaciones: la matriz de adyacencia y la lista de adyacencia. Después estudiaremos cómo leer entradas, salidas, caminos y ciclos. Una vez que el grafo existe, podremos recorrerlo: DFS profundiza antes de regresar, mientras BFS usa una cola para avanzar por niveles y calcular distancias mínimas en grafos no ponderados. Finalmente aplicaremos A* a una cuadrícula, combinando el costo recorrido con una estimación Manhattan, y construiremos un recomendador elemental a partir de vínculos entre dos grupos de nodos.

No buscamos copiar ocho programas aislados. Construiremos representaciones, justificaremos las decisiones de cada algoritmo, realizaremos trazas manuales, predeciremos la salida y modificaremos los datos para comprobar que entendemos las consecuencias. Los archivos resultantes serán programas Java independientes y reproducibles con la biblioteca estándar.

## 2. Resultado de aprendizaje observable

Al terminar podrás modelar una red pequeña en Java, elegir una representación de acuerdo con la densidad de conexiones y explicar la diferencia entre aristas dirigidas y no dirigidas. Podrás recorrer un grafo con DFS y BFS, interpretar las marcas de visita y una cola FIFO, identificar un ciclo no dirigido sin confundir el regreso al padre y medir distancias mínimas por número de aristas en una red sin pesos. También podrás interpretar f(n)=g(n)+h(n) en una ruta A* y justificar una sugerencia sencilla basada en relaciones entre usuarios y productos.

Por ejemplo, si se te entrega una red con A, B, C, D y E, demostrarás dominio al construirla, indicar qué nodos visita BFS primero, justificar por qué E está a cierta distancia, ejecutar el programa y modificar una arista de forma que la nueva salida coincida con tu predicción. Se considera comprensión cuando puedes explicar el estado y las condiciones del algoritmo sin limitarte a mostrar una captura de consola. Cada ejemplo resuelto tiene una tarea espejo propia, identificada como T01–T08.

**Desempeños verificables:** representar, recorrer, predecir, ejecutar, comparar, diagnosticar, corregir, justificar y validar.

**Criterio de dominio:** la salida impresa y la traza manual son coherentes; se explica el propósito de cada variable de control y se corrige al menos un caso límite significativo.

## 3. Antes de comenzar

**Debes saber:** declarar clases Java con `main`, usar arreglos, `for`, `if`, métodos, listas enlazadas, pila de llamadas de una función recursiva y cola FIFO, habilidades estudiadas en las sesiones anteriores del curso. Si no recuerdas una estructura, la introduciremos aquí en el punto en que se necesita.

**Necesitas:** un JDK que incluya `javac` y `java` (el material se valida con JDK 21), terminal o IDE con soporte Java, editor de texto y una carpeta personal de trabajo. No necesitas frameworks, librerías de terceros, servicios cloud ni cuenta de pago.

**No asumiremos:** aprendizaje automático avanzado, modelos de recomendación comerciales, grafos de millones de nodos, caminos de peso negativo, heurísticas diagonales ni optimizaciones industriales.

### Verificar Java y crear la carpeta base

Abre la terminal y ejecuta primero `java -version` y después `javac -version`. Ambos deben responder con una versión, aunque no sea exactamente la usada para validar estos archivos. Si `javac` no existe, necesitas instalar/configurar un JDK, no solo un entorno de ejecución; usa la distribución JDK disponible oficialmente o la autorizada por tu institución. En el IDE verifica que el proyecto apunte al JDK, no solamente a un runtime. Para el trabajo en terminal, crea una carpeta llamada `S13_Grafos` con el explorador de archivos o con el comando apropiado de tu sistema, y dentro una subcarpeta por ejemplo: `EJ01`, `EJ02`, ..., `EJ08`.

Para cada programa: crea dentro de su carpeta un archivo `.java` cuyo nombre coincida exactamente con la clase `public`, copia **los microbloques en su orden**, guarda y ejecuta desde esa carpeta: `javac -encoding UTF-8 NombreArchivo.java` y luego `java NombreClase` (sin `.java` ni `.class`). Si Java indica «class ... is public, should be declared in a file named ...», revisa mayúsculas, nombre de clase y archivo. Si hay un error antes de concluir los microbloques, no compiles todavía: el archivo puede tener métodos o llaves aún por completar.

**Control de salida:** para EJ07, dos rutas de igual costo pueden mostrar coordenadas intermedias distintas; valida extremos, costo, continuidad y ausencia de obstáculos, no una única cadena textual.

## 4. Ruta de aula sugerida — 120 minutos

La clase presencial cubre los hitos esenciales, y los ejemplos no centrales quedan explicados íntegramente para su estudio autónomo. **Apertura y comprobación del JDK:** 10 min; **EJ01 matriz:** 12 min; **EJ02 lista:** 12 min; **EJ04 DFS:** 20 min; **EJ05 BFS:** 20 min; **EJ07 A*:** 25 min; **práctica acompañada T01/T04/T05:** 15 min; **cierre y preguntas:** 6 min. Total: **120 minutos**. EJ03, EJ06 y EJ08 junto con sus tareas, y el resto de las tareas espejo, se desarrollan como práctica autónoma apoyada por la guía. El tiempo del aula no implica terminar todas las tareas en la misma sesión.

## 5. Distinciones técnicas indispensables

- **Matriz de adyacencia:** usa `int[][]`, acceso a una celda O(1), espacio O(V²), conveniente cuando importan consultas directas y hay muchas conexiones.
- **Lista de adyacencia:** almacena vecinos realmente existentes, espacio O(V+E) para la representación usual, útil cuando hay relativamente pocas aristas. En grafos no dirigidos cada arista se registra en dos listas.
- **Grado:** número de aristas incidentes en el grafo simple no dirigido. En el dirigido distinguimos grado de entrada y salida; la suma total de grados en un grafo no dirigido es 2E.
- **DFS:** prioriza una rama y utiliza llamadas recursivas en este ejemplo. Con listas de adyacencia el recorrido completo toma O(V+E) tiempo; la pila recursiva puede requerir hasta O(V) espacio.
- **BFS:** procesa por niveles con cola FIFO y da distancias mínimas en **número de aristas**, no necesariamente mínimo costo cuando los pesos varían.
- **A*:** combina f(n)=g(n)+h(n). En nuestro grid, cada movimiento ortogonal cuesta uno y Manhattan es admisible y consistente; la implementación utiliza ese supuesto.
- **Grafo bipartito:** separa dos tipos de nodos; en el ejemplo final hay vínculos usuario-producto, no conexiones dentro del mismo grupo.

---

## EJ01 — Representar conexiones con una matriz simétrica

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** Vamos a representar cuatro puntos de una red mediante una matriz de adyacencia. Primero decidiremos qué significa cada índice, luego reservaremos una cuadrícula de cuatro filas y cuatro columnas, y finalmente registraremos conexiones en ambas direcciones. Para que las relaciones no queden ocultas dentro del arreglo, construiremos una operación pequeña que modifica dos celdas en cada enlace. Al ejecutar, veremos la matriz completa y comprobaremos una conexión concreta. La evidencia será observar que A está conectado con B y que B también está conectado con A, sin almacenar duplicados conceptuales de vértices.

**Qué necesitas comprender antes de escribir:** El hecho técnico que debes demostrar es la **simetría**: una sola arista A–B exige poner un 1 en `[0][1]` y otro en `[1][0]`. Así podrás distinguir una arista directa de un camino formado por varios saltos.

**¿Qué problema concreto resolveremos?** Una tabla de nombres nos dice qué puntos existen, pero no cuáles pueden comunicarse directamente. Necesitamos una estructura capaz de contestar de inmediato si dos índices están conectados.

**Caso de trabajo.** Un grafo no dirigido de cuatro lugares A, B, C y D; las calles se representan por aristas sin peso.

**Archivo a crear:** `S13_Grafos/EJ01/EJ01_Matriz.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **6 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Un grafo no dirigido de cuatro lugares A, B, C y D; las calles se representan por aristas sin peso.


### Paso 1 — Importar la utilidad que permite imprimir una fila

**Antes de escribir, comprendamos la necesidad.** Imprimir un arreglo bidimensional completo con println no muestra automáticamente los valores de cada fila.

**Dónde y cómo escribir.** En la carpeta `EJ01/`, crea el archivo `EJ01_Matriz.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.Arrays;
```
**Qué hace cada elemento nuevo y por qué.** `Arrays` pertenece a `java.util`; su método `toString` transforma una fila `int[]` en una representación legible. Este import solo sirve para visualizar, no para definir aristas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `import java.util.Arrays;` habilita el nombre `Arrays` sin escribir `java.util.Arrays` completo. La utilizaremos únicamente al imprimir una fila con `Arrays.toString`; esta línea no agrega nodos ni modifica la matriz.

**Qué cambia en el estado.** La clase utilitaria ya está disponible, pero el archivo todavía no declara ningún grafo.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La clase utilitaria ya está disponible, pero el archivo todavía no declara ningún grafo.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos declarar la clase y el procedimiento de entrada.


### Paso 2 — Declarar la clase y el procedimiento de entrada

**Antes de escribir, comprendamos la necesidad.** Necesitamos un lugar ejecutable desde el que crear la red y observar su estado.

**Dónde y cómo escribir.** En `EJ01_Matriz.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ01_Matriz {
    public static void main(String[] args) {
```
**Qué hace cada elemento nuevo y por qué.** `EJ01_Matriz` coincide con el nombre del archivo. `main` es el punto de entrada de Java; las dos llaves abiertas delimitan clase y método, y NO deben cerrarse todavía.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `public class EJ01_Matriz {` declara una clase pública: el archivo se llamará exactamente `EJ01_Matriz.java`. La llave inicia el cuerpo que alojará la lógica de la demostración.
- `public static void main(String[] args) {` abre el punto de partida de Java. `static` permite ejecutarlo sin crear objetos; `void` significa que no devuelve un valor; `String[] args` permite recibir argumentos aunque hoy no los usemos. La segunda llave deja abierto `main` para los siguientes pasos.

**Qué cambia en el estado.** Existe un programa inicial aún incompleto: debemos declarar los vértices y las celdas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Existe un programa inicial aún incompleto: debemos declarar los vértices y las celdas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos asignar índices y reservar la tabla de conexiones.


### Paso 3 — Asignar índices y reservar la tabla de conexiones

**Antes de escribir, comprendamos la necesidad.** Los índices 0, 1, 2 y 3 representan A, B, C y D. Sin tabla de relaciones no sabemos qué vértices son vecinos.

**Dónde y cómo escribir.** En `EJ01_Matriz.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        String[] nombres = {"A", "B", "C", "D"};
        int[][] matriz = new int[nombres.length][nombres.length];
```
**Qué hace cada elemento nuevo y por qué.** `String[] nombres` etiqueta cada índice; `nombres.length` vale cuatro. `new int[4][4]` crea dieciséis celdas inicializadas en cero: ninguna arista fue añadida todavía. Cada celda `[origen][destino]` corresponde a un par ordenado de índices.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `String[] nombres = {"A", "B", "C", "D"};` vincula los índices 0, 1, 2 y 3 con etiquetas fáciles de leer. Por ejemplo, consultar `nombres[2]` devuelve `C`; los índices siguen siendo números.
- `int[][] matriz = new int[nombres.length][nombres.length];` reserva una tabla cuadrada 4×4 de enteros. La primera dimensión expresa origen y la segunda destino. Java coloca ceros en sus 16 posiciones, así que el grafo comienza sin aristas. No confundas esos ceros con vértices inexistentes: los cuatro vértices ya están definidos.

**Qué cambia en el estado.** Estado: cuatro vértices etiquetados y matriz 4×4 de ceros; quedan pendientes las aristas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado: cuatro vértices etiquetados y matriz 4×4 de ceros; quedan pendientes las aristas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos registrar aristas reales sin perder la doble dirección.


### Paso 4 — Registrar aristas reales sin perder la doble dirección

**Antes de escribir, comprendamos la necesidad.** Queremos crear A-B, A-C, B-D y C-D; hacerlo con una sola llamada por arista evita repetir manualmente dos asignaciones.

**Dónde y cómo escribir.** En `EJ01_Matriz.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        conectar(matriz, 0, 1);
        conectar(matriz, 0, 2);
        conectar(matriz, 1, 3);
        conectar(matriz, 2, 3);
```
**Qué hace cada elemento nuevo y por qué.** Cada llamada recibe `matriz` y dos índices. La operación `conectar` se declarará después de `main` y escribirá ambas orientaciones. En este punto el editor puede marcar el método como no encontrado: se resolverá cuando añadamos su definición.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `conectar(matriz, 0, 1);` crea el vínculo A–B usando el par de índices 0 y 1. Como el grafo es no dirigido, el método almacenará A→B y B→A.
- `conectar(matriz, 0, 2);` agrega A–C; A pasará a tener dos vecinos directos. Esta llamada es diferente de A–B porque apunta a la columna 2 de la fila 0.
- `conectar(matriz, 1, 3);` agrega B–D y `conectar(matriz, 2, 3);` agrega C–D. Con cuatro aristas la tabla tendrá ocho unos fuera de la diagonal, porque cada conexión se registra en dos sentidos. Estas llamadas usan un método que definiremos en el último paso; no compiles un fragmento incompleto.

**Qué cambia en el estado.** Estado previsto: A tiene vecinos B,C; B tiene A,D; C tiene A,D; D tiene B,C.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado previsto: A tiene vecinos B,C; B tiene A,D; C tiene A,D; D tiene B,C.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos mostrar las filas y verificar un enlace directo.


### Paso 5 — Mostrar las filas y verificar un enlace directo

**Antes de escribir, comprendamos la necesidad.** La estructura ya contiene relaciones, pero necesitamos pruebas observables de todas las filas y de una consulta directa.

**Dónde y cómo escribir.** En `EJ01_Matriz.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int i = 0; i < nombres.length; i++) {
            System.out.println(nombres[i] + " " + Arrays.toString(matriz[i]));
        }
        System.out.println("A-B=" + matriz[0][1]);
    }
```
**Qué hace cada elemento nuevo y por qué.** `for` recorre de 0 a 3; `nombres[i]` identifica la fila y `Arrays.toString(matriz[i])` imprime sus cuatro valores. `matriz[0][1]` consulta A-B sin recorrer todo el grafo. La última llave cierra `main`, mientras la clase sigue abierta.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int i = 0; i < nombres.length; i++) {` inicia `i=0`, repite mientras `i<4` y suma uno después de cada vuelta. Recorremos una fila por cada vértice; `i=4` quedaría fuera del arreglo.
- `System.out.println(nombres[i] + " " + Arrays.toString(matriz[i]));` une la etiqueta con la fila completa. Cuando `i=0`, imprime A y sus conexiones; `matriz[i]` es un `int[]`, no un único número.
- `}` cierra el bucle sin cerrar todavía `main`. `System.out.println("A-B=" + matriz[0][1]);` es una comprobación puntual: consulta la celda origen A/destino B y deberá producir 1, sin buscar rutas intermedias. La llave final de este fragmento cierra `main`.

**Qué cambia en el estado.** Se esperan cuatro filas con ceros y unos más A-B=1; aún debemos escribir la operación conectar.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Se esperan cuatro filas con ceros y unos más A-B=1; aún debemos escribir la operación conectar.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos implementar la regla de simetría y cerrar la clase.


### Paso 6 — Implementar la regla de simetría y cerrar la clase

**Antes de escribir, comprendamos la necesidad.** Una red no dirigida exige que la misma arista aparezca vista desde cualquiera de sus extremos.

**Dónde y cómo escribir.** En `EJ01_Matriz.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void conectar(int[][] matriz, int a, int b) {
        matriz[a][b] = 1;
        matriz[b][a] = 1;
    }
}
```
**Qué hace cada elemento nuevo y por qué.** `static void` declara una operación sin valor de retorno que `main` puede llamar directamente. `a` y `b` son índices recibidos como parámetros. La primera asignación registra a→b; la segunda, b→a. Las llaves cierran el método auxiliar y la clase.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void conectar(int[][] matriz, int a, int b) {` define un método auxiliar reutilizable: recibe la tabla por referencia y dos índices. `void` se justifica porque modifica la matriz que recibió, no necesita retornar otra.
- `matriz[a][b] = 1;` activa la conexión desde `a` hacia `b`. Si `a=0` y `b=1`, cambia `[0][1]` de 0 a 1.
- `matriz[b][a] = 1;` registra el recorrido inverso indispensable para un grafo no dirigido; omitirla haría que la matriz dejara de ser simétrica. Las dos llaves finales cierran el método y la clase, en ese orden.

**Qué cambia en el estado.** Ahora sí hay un archivo Java completo, compilable y simétrico.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Ahora sí hay un archivo Java completo, compilable y simétrico.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

Antes: matriz llena de ceros. Conectar(0,1): cambian [0][1] y [1][0] de 0 a 1. Conectar(0,2) y conectar(1,3), conectar(2,3) completan la simetría.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Importar la utilidad que permite imprimir una fila | Imprimir un arreglo bidimensional completo con println no muestra automáticamente los valores de cada fila. | La clase utilitaria ya está disponible, pero el archivo todavía no declara ningún grafo. |
| Paso 2: Declarar la clase y el procedimiento de entrada | Necesitamos un lugar ejecutable desde el que crear la red y observar su estado. | Existe un programa inicial aún incompleto: debemos declarar los vértices y las celdas. |
| Paso 3: Asignar índices y reservar la tabla de conexiones | Los índices 0, 1, 2 y 3 representan A, B, C y D. Sin tabla de relaciones no sabemos qué vértices son vecinos. | Estado: cuatro vértices etiquetados y matriz 4×4 de ceros; quedan pendientes las aristas. |
| Paso 4: Registrar aristas reales sin perder la doble dirección | Queremos crear A-B, A-C, B-D y C-D; hacerlo con una sola llamada por arista evita repetir manualmente dos asignaciones. | Estado previsto: A tiene vecinos B,C; B tiene A,D; C tiene A,D; D tiene B,C. |
| Paso 5: Mostrar las filas y verificar un enlace directo | La estructura ya contiene relaciones, pero necesitamos pruebas observables de todas las filas y de una consulta directa. | Se esperan cuatro filas con ceros y unos más A-B=1; aún debemos escribir la operación conectar. |
| Paso 6: Implementar la regla de simetría y cerrar la clase | Una red no dirigida exige que la misma arista aparezca vista desde cualquiera de sus extremos. | Ahora sí hay un archivo Java completo, compilable y simétrico. |


### Archivo completo al terminar EJ01

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.Arrays;
public class EJ01_Matriz {
    public static void main(String[] args) {
        String[] nombres = {"A", "B", "C", "D"};
        int[][] matriz = new int[nombres.length][nombres.length];
        conectar(matriz, 0, 1);
        conectar(matriz, 0, 2);
        conectar(matriz, 1, 3);
        conectar(matriz, 2, 3);
        for (int i = 0; i < nombres.length; i++) {
            System.out.println(nombres[i] + " " + Arrays.toString(matriz[i]));
        }
        System.out.println("A-B=" + matriz[0][1]);
    }
    static void conectar(int[][] matriz, int a, int b) {
        matriz[a][b] = 1;
        matriz[b][a] = 1;
    }
}
```

### Predice antes de ejecutar

¿Qué valor tendrá matriz[1][0] después de conectar A con B? ¿Por qué matriz[0][0] sigue en cero?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ01` y ejecuta:

```bash
javac -encoding UTF-8 EJ01_Matriz.java
java EJ01_Matriz
```
**Salida de referencia:**

```text
A [0, 1, 1, 0]
B [1, 0, 0, 1]
C [1, 0, 0, 1]
D [0, 1, 1, 0]
A-B=1
```
**Interpretación:** El uno señala arista directa; el cero ausencia de conexión directa. No representa distancia ni una ruta de varios saltos.

**Variación razonada:** Elimina temporalmente conectar(matriz, 2, 3): C y D quedarán desconectados de manera directa; sus celdas simétricas volverán a cero.

**Error o caso límite:** Cambiar solo matriz[a][b] en un grafo no dirigido rompe la simetría: A-B podría valer uno mientras B-A vale cero.

**Corrección:** Restablecer las dos asignaciones dentro de conectar; comprobar que matriz[i][j] == matriz[j][i] para todos los pares.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T01 — tarea espejo de EJ01

**Enunciado:** Construye un grafo no dirigido A-B-C-D con enlaces A-B, B-C y C-D. Imprime su matriz; verifica que A-D no es arista directa y que B-C aparece en ambas direcciones.

**Archivo nuevo:** `S13_Grafos/T01/T01_Matriz.java`. No modifiques el programa resuelto `EJ01_Matriz.java`; crea una carpeta nueva `T01` al lado de `EJ01`.

**Pista:** Conserva la operación conectar, pero modifica únicamente las llamadas que definen las aristas; consulta las celdas con sus índices correctos.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
public class T01_Matriz {
    public static void main(String[] args) {
        int[][] matriz = new int[4][4];
        conectar(matriz, 0, 1);
        // TODO T01: registra aquí B-C y C-D.
        // TODO T01: imprime las filas y verifica A-D, B-C y C-B.
        System.out.println("Completa T01 y comprueba simetría.");
    }
    static void conectar(int[][] matriz, int a, int b) {
        matriz[a][b] = 1;
        matriz[b][a] = 1;
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T01_Matriz.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** Las cuatro filas muestran una cadena simétrica y A-D=0; B-C=1 y C-B=1. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ02 — Almacenar solo vecinos mediante LinkedList

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** La matriz reserva una celda incluso para parejas sin conexión. En una red dispersa esto puede gastar espacio sin aportar información. Construiremos una lista de adyacencia con una colección principal que guarda una LinkedList por vértice; después añadiremos las mismas cuatro aristas del ejemplo anterior. Usaremos índices y recorreremos cada lista para que quede claro que una posición representa un nodo y sus elementos representan los vecinos existentes. Al final comprobaremos el grado de un nodo contando sus conexiones y contrastaremos el almacenamiento con la matriz anterior.

**Qué necesitas comprender antes de escribir:** Esta vez la pregunta cambia: ¿por qué reservar dieciséis celdas cuando muchas representan ausencia de aristas? Explicaremos cómo un contenedor externo ubica los nodos y una lista interna conserva solo vecinos existentes. El grafo será el mismo de EJ01, para comparar sin cambiar los datos.

**¿Qué problema concreto resolveremos?** Una matriz de 4×4 necesita 16 posiciones aunque solo haya cuatro aristas no dirigidas. Para una red dispersa queremos almacenar principalmente las conexiones presentes.

**Caso de trabajo.** Misma red A-B, A-C, B-D, C-D pero ahora con listas de vecinos.

**Archivo a crear:** `S13_Grafos/EJ02/EJ02_Lista.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **6 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Misma red A-B, A-C, B-D, C-D pero ahora con listas de vecinos.


### Paso 1 — Importar la colección indexada y las listas enlazadas

**Antes de escribir, comprendamos la necesidad.** Necesitamos acceder a los vecinos de un vértice por índice y, dentro de cada posición, conservar solo las conexiones existentes.

**Dónde y cómo escribir.** En la carpeta `EJ02/`, crea el archivo `EJ02_Lista.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.ArrayList;
import java.util.LinkedList;
```
**Qué hace cada elemento nuevo y por qué.** `ArrayList` permitirá ubicar rápidamente la lista del nodo `i`; `LinkedList` almacenará los índices vecinos. Son clases de la biblioteca estándar, no dependencias externas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `import java.util.ArrayList;` permite guardar una colección principal cuyo tamaño puede crecer; cada posición corresponderá a un vértice.
- `import java.util.LinkedList;` permite almacenar los vecinos de cada vértice en una lista enlazada. No confundas la lista externa con la lista de vecinos: tienen responsabilidades distintas.

**Qué cambia en el estado.** Las estructuras están disponibles para declarar la lista de adyacencia.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Las estructuras están disponibles para declarar la lista de adyacencia.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos abrir el programa y preparar un contenedor de listas.


### Paso 2 — Abrir el programa y preparar un contenedor de listas

**Antes de escribir, comprendamos la necesidad.** Conocemos cuatro nombres, pero necesitamos una lista de vecinos separada para cada uno.

**Dónde y cómo escribir.** En `EJ02_Lista.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ02_Lista {
    public static void main(String[] args) {
        String[] nombres = {"A", "B", "C", "D"};
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
```
**Qué hace cada elemento nuevo y por qué.** La clase corresponde al archivo; `main` inicia el programa. `ArrayList<LinkedList<Integer>>` significa una colección de listas enlazadas de índices enteros. `ady` todavía está vacía: crear el contenedor externo no genera automáticamente cuatro listas internas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `public class EJ02_Lista` y `main` crean un programa independiente para comparar con EJ01. No necesitamos cambiar la clase anterior.
- `String[] nombres` conserva la correspondencia 0→A, 1→B, 2→C, 3→D; la comparación entre representaciones será legítima porque usa los mismos vértices.
- `ArrayList<LinkedList<Integer>> ady = new ArrayList<>();` crea el contenedor externo inicialmente vacío. Cada elemento futuro será una `LinkedList<Integer>` cuyos números son índices de vecinos; los signos `<...>` indican tipos genéricos, no operaciones matemáticas. Todavía no hay cuatro listas: crearlas es el propósito del próximo paso.

**Qué cambia en el estado.** Estado: ady.size() sería cero; intentar ady.get(0) ahora causaría error.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado: ady.size() sería cero; intentar ady.get(0) ahora causaría error.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos crear una linkedlist vacía para cada vértice.


### Paso 3 — Crear una LinkedList vacía para cada vértice

**Antes de escribir, comprendamos la necesidad.** No podemos guardar vecinos en una posición que aún no existe.

**Dónde y cómo escribir.** En `EJ02_Lista.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int i = 0; i < nombres.length; i++) {
            ady.add(new LinkedList<Integer>());
        }
```
**Qué hace cada elemento nuevo y por qué.** El ciclo crea una LinkedList por cada etiqueta y la agrega a `ady`. Tras cuatro iteraciones, `ady.get(0)` es la lista de A y `ady.get(3)` es la de D; todas empiezan vacías.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int i = 0; i < nombres.length; i++) {` repite cuatro veces: necesitamos una lista por vértice, aunque alguno no tenga conexiones.
- `ady.add(new LinkedList<Integer>());` crea una lista de vecinos vacía y la agrega al contenedor en la posición `i`. Sin esta inicialización, un posterior `ady.get(3)` fallaría porque el contenedor aún no tiene índice 3.
- `}` termina la inicialización. Ahora `ady.size()` es 4, pero cada `ady.get(i).size()` todavía es 0. Distingue *número de vértices* de *número de vecinos*.

**Qué cambia en el estado.** Estado: cuatro listas vacías, sin aristas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado: cuatro listas vacías, sin aristas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos incorporar las mismas conexiones usadas en la matriz.


### Paso 4 — Incorporar las mismas conexiones usadas en la matriz

**Antes de escribir, comprendamos la necesidad.** La comparación entre representaciones requiere mantener exactamente el mismo grafo.

**Dónde y cómo escribir.** En `EJ02_Lista.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        conectar(ady, 0, 1);
        conectar(ady, 0, 2);
        conectar(ady, 1, 3);
        conectar(ady, 2, 3);
```
**Qué hace cada elemento nuevo y por qué.** Cada llamada añade una relación no dirigida; usaremos un método auxiliar que escribe a ambos extremos. Los índices siguen identificando A=0, B=1, C=2 y D=3. Hasta crear `conectar`, las llamadas quedarán pendientes de resolución.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- Las llamadas `conectar(ady, 0, 1)` y `conectar(ady, 0, 2)` añaden los índices B y C a la lista de A, y también A a las listas de B y C.
- Las llamadas `conectar(ady, 1, 3)` y `conectar(ady, 2, 3)` incorporan D como vecino de B y de C. Reutilizamos exactamente las cuatro aristas de EJ01 para observar qué cambia con la representación y qué no cambia en el grafo.
- La expresión `conectar(ady, a, b)` pasa el contenedor principal y dos números; el método `conectar` se definirá después. La lista contiene números de vecino, no objetos `String` ni pesos.

**Qué cambia en el estado.** Estado previsto: A->[1,2], B->[0,3], C->[0,3], D->[1,2].

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado previsto: A->[1,2], B->[0,3], C->[0,3], D->[1,2].» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos presentar las listas y medir el grado de a.


### Paso 5 — Presentar las listas y medir el grado de A

**Antes de escribir, comprendamos la necesidad.** Debemos mostrar que no existen casillas para parejas inexistentes y que el grado puede obtenerse de los vecinos almacenados.

**Dónde y cómo escribir.** En `EJ02_Lista.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int i = 0; i < nombres.length; i++) {
            System.out.println(nombres[i] + " -> " + ady.get(i));
        }
        System.out.println("grado(A)=" + ady.get(0).size());
    }
```
**Qué hace cada elemento nuevo y por qué.** `get(i)` selecciona los vecinos del nodo i; Java muestra la lista con corchetes. `size()` cuenta vecinos de A y, para este grafo simple no dirigido, coincide con su grado. Se cierra `main`, pero la clase sigue abierta.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int i=0; i<nombres.length; i++)` visita los cuatro vértices: imprimir una lista no significa recorrer el grafo mediante DFS o BFS, sino mostrar su representación almacenada.
- `ady.get(i)` recupera la lista del vértice `i`; `System.out.println` muestra por ejemplo `A -> [1, 2]`. El orden de la lista refleja el orden de inserción y no una distancia.
- `ady.get(0).size()` devuelve el número de vecinos de A: dos. En un grafo simple no dirigido equivale a su grado. La llave del final cierra `main` y permite declarar el método auxiliar fuera de él.

**Qué cambia en el estado.** Se espera grado(A)=2. Faltan las reglas concretas para registrar la arista.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Se espera grado(A)=2. Faltan las reglas concretas para registrar la arista.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos agregar ambos extremos de la arista y terminar.


### Paso 6 — Agregar ambos extremos de la arista y terminar

**Antes de escribir, comprendamos la necesidad.** En un grafo no dirigido A-B y B-A son la misma relación conceptual, pero ambas listas deben permitir consultar al otro extremo.

**Dónde y cómo escribir.** En `EJ02_Lista.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void conectar(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```
**Qué hace cada elemento nuevo y por qué.** El método recibe la colección por referencia y los índices. `get(a).add(b)` inserta a b como vecino de a; la segunda instrucción agrega a como vecino de b. No evita duplicados por sí solo: cada pareja se llama una sola vez en este ejercicio.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void conectar(ArrayList<LinkedList<Integer>> ady, int a, int b)` recibe la misma estructura que usa `main`; usar un método evita repetir operaciones y garantiza la regla de bidireccionalidad.
- `ady.get(a).add(b);` accede a la lista de `a` y almacena el índice `b`; significa que `b` es vecino de `a`.
- `ady.get(b).add(a);` hace la operación inversa. Si se omite, B podría listar A sin que A liste B. Las llaves cierran el método y la clase. La lista registra dos entradas por arista, pero la arista sigue siendo una sola relación no dirigida.

**Qué cambia en el estado.** El archivo queda completo, con ocho entradas de vecindad para cuatro aristas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El archivo queda completo, con ocho entradas de vecindad para cuatro aristas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

Antes: cuatro listas vacías. Cada conectar(a,b) agrega b a la lista a y a a la lista b. El número total de entradas crece de 0 a 8.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Importar la colección indexada y las listas enlazadas | Necesitamos acceder a los vecinos de un vértice por índice y, dentro de cada posición, conservar solo las conexiones existentes. | Las estructuras están disponibles para declarar la lista de adyacencia. |
| Paso 2: Abrir el programa y preparar un contenedor de listas | Conocemos cuatro nombres, pero necesitamos una lista de vecinos separada para cada uno. | Estado: ady.size() sería cero; intentar ady.get(0) ahora causaría error. |
| Paso 3: Crear una LinkedList vacía para cada vértice | No podemos guardar vecinos en una posición que aún no existe. | Estado: cuatro listas vacías, sin aristas. |
| Paso 4: Incorporar las mismas conexiones usadas en la matriz | La comparación entre representaciones requiere mantener exactamente el mismo grafo. | Estado previsto: A->[1,2], B->[0,3], C->[0,3], D->[1,2]. |
| Paso 5: Presentar las listas y medir el grado de A | Debemos mostrar que no existen casillas para parejas inexistentes y que el grado puede obtenerse de los vecinos almacenados. | Se espera grado(A)=2. Faltan las reglas concretas para registrar la arista. |
| Paso 6: Agregar ambos extremos de la arista y terminar | En un grafo no dirigido A-B y B-A son la misma relación conceptual, pero ambas listas deben permitir consultar al otro extremo. | El archivo queda completo, con ocho entradas de vecindad para cuatro aristas. |


### Archivo completo al terminar EJ02

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class EJ02_Lista {
    public static void main(String[] args) {
        String[] nombres = {"A", "B", "C", "D"};
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < nombres.length; i++) {
            ady.add(new LinkedList<Integer>());
        }
        conectar(ady, 0, 1);
        conectar(ady, 0, 2);
        conectar(ady, 1, 3);
        conectar(ady, 2, 3);
        for (int i = 0; i < nombres.length; i++) {
            System.out.println(nombres[i] + " -> " + ady.get(i));
        }
        System.out.println("grado(A)=" + ady.get(0).size());
    }
    static void conectar(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```

### Predice antes de ejecutar

¿Cuántas entradas habrá en las listas si contamos cada arista no dirigida desde ambos extremos?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ02` y ejecuta:

```bash
javac -encoding UTF-8 EJ02_Lista.java
java EJ02_Lista
```
**Salida de referencia:**

```text
A -> [1, 2]
B -> [0, 3]
C -> [0, 3]
D -> [1, 2]
grado(A)=2
```
**Interpretación:** Cada entrada es un vecino realmente conectado. Cuatro aristas no dirigidas producen ocho entradas, una desde cada extremo.

**Variación razonada:** Agrega conectar(ady, 0, 3); A y D deben ganar un vecino y grado(A) pasa a 3.

**Error o caso límite:** Olvidar inicializar cada LinkedList antes de usar get(i).add(...) impediría añadir vecinos.

**Corrección:** Crear las listas vacías en el bucle de inicialización antes de registrar aristas.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T02 — tarea espejo de EJ02

**Enunciado:** Representa una cadena no dirigida A-B-C-D usando una LinkedList de vecinos por nodo; muestra listas y confirma que el grado de B es 2.

**Archivo nuevo:** `S13_Grafos/T02/T02_Lista.java`. No modifiques el programa resuelto `EJ02_Lista.java`; crea una carpeta nueva `T02` al lado de `EJ02`.

**Pista:** Inicializa primero las cuatro listas, conserva conectar en dos direcciones y cambia el conjunto de aristas.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class T02_Lista {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
        conectar(ady, 0, 1);
        // TODO T02: incorpora B-C y C-D.
        // TODO T02: imprime las cuatro listas y grado(B).
        System.out.println("Completa la cadena en T02.");
    }
    static void conectar(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T02_Lista.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** A -> [1], B -> [0, 2], C -> [1, 3], D -> [2], grado(B)=2. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ03 — Medir entrada y salida en un grafo dirigido

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** Hasta ahora tratamos cada conexión como bidireccional. En una red de seguimiento o de calles de sentido único, A→B no implica B→A. Crearemos una matriz dirigida que registre únicamente la celda de origen y destino, y a partir de ella calcularemos por separado el grado de entrada y el de salida de cada vértice. Dibujaremos mentalmente un ciclo A→B→C→A y una entrada adicional D→C para comprobar que un nodo puede tener grados diferentes. La salida del programa permitirá verificar cada conteo en lugar de memorizar definiciones.

**Qué necesitas comprender antes de escribir:** En una red dirigida no podemos inferir B→A a partir de A→B. Debes poder leer una fila como salidas y una columna como entradas. El ejercicio fija cuatro arcos, por eso los grados pueden ser diferentes.

**¿Qué problema concreto resolveremos?** En una red dirigida no basta preguntar cuántas aristas tocan un vértice: debemos distinguir las que llegan de las que salen.

**Caso de trabajo.** Red de cuatro cuentas A,B,C,D: A→B, B→C, C→A, D→C.

**Archivo a crear:** `S13_Grafos/EJ03/EJ03_Dirigido.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **5 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Red de cuatro cuentas A,B,C,D: A→B, B→C, C→A, D→C.


### Paso 1 — Crear la clase y una matriz de direcciones

**Antes de escribir, comprendamos la necesidad.** Un arco dirigido tiene origen y destino; una matriz puede expresar ambos con el orden de índices.

**Dónde y cómo escribir.** En la carpeta `EJ03/`, crea el archivo `EJ03_Dirigido.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
public class EJ03_Dirigido {
    public static void main(String[] args) {
        String[] nombres = {"A", "B", "C", "D"};
        int[][] matriz = new int[4][4];
```
**Qué hace cada elemento nuevo y por qué.** La fila será el origen y la columna el destino. `matriz[4][4]` inicia con ceros. Los nombres permiten traducir índices a letras. No activaremos automáticamente la posición inversa.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `public class EJ03_Dirigido` y `main` inician una demostración independiente: aquí las conexiones sí tienen sentido, por lo que ya no usaremos el método simétrico anterior.
- `String[] nombres` permite presentar resultados con A, B, C y D; `int[][] matriz = new int[4][4]` reserva un espacio de origen×destino. `matriz[i][j]=1` significará *hay arco desde i hasta j*; no presupone el arco contrario.

**Qué cambia en el estado.** Todavía no hay aristas; el grafo dirigido está vacío.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Todavía no hay aristas; el grafo dirigido está vacío.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos añadir cuatro arcos con un único sentido.


### Paso 2 — Añadir cuatro arcos con un único sentido

**Antes de escribir, comprendamos la necesidad.** Necesitamos un ciclo y una entrada extra para observar la diferencia entre grado entrante y saliente.

**Dónde y cómo escribir.** En `EJ03_Dirigido.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        matriz[0][1] = 1;
        matriz[1][2] = 1;
        matriz[2][0] = 1;
        matriz[3][2] = 1;
```
**Qué hace cada elemento nuevo y por qué.** `[0][1]` es A→B; `[1][2]`, B→C; `[2][0]`, C→A; `[3][2]`, D→C. No duplicamos ninguna asignación: esta es la diferencia estructural con EJ01.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `matriz[0][1]=1` crea A→B. Dejar `matriz[1][0]` en cero significa que no declaramos B→A.
- `matriz[1][2]=1` crea B→C y `matriz[2][0]=1` crea C→A: esas tres direcciones forman el ciclo A→B→C→A.
- `matriz[3][2]=1` crea D→C; C recibe una segunda arista sin que deba salir una arista C→D. La lectura del primer índice siempre será origen y la del segundo, destino.

**Qué cambia en el estado.** C tiene dos entradas (B y D) y una salida (A).

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «C tiene dos entradas (B y D) y una salida (A).» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos preparar un contador por cada sentido en cada nodo.


### Paso 3 — Preparar un contador por cada sentido en cada nodo

**Antes de escribir, comprendamos la necesidad.** El número de arcos que entra a un nodo no se obtiene sumando su fila.

**Dónde y cómo escribir.** En `EJ03_Dirigido.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int nodo = 0; nodo < nombres.length; nodo++) {
            int entrada = 0;
            int salida = 0;
```
**Qué hace cada elemento nuevo y por qué.** `nodo` señala el vértice evaluado. `entrada` y `salida` empiezan en cero cada vez que cambiamos de nodo, evitando acumular datos de un vértice anterior.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int nodo=0; nodo<nombres.length; nodo++)` selecciona sucesivamente el vértice cuyo grado se calculará; cada vuelta comienza con contadores nuevos.
- `int entrada = 0;` representa cuántos arcos apuntan al nodo seleccionado. `int salida = 0;` representa cuántos parten de él. Reiniciarlos dentro del bucle evita que los conteos de A se sumen accidentalmente a los de B.

**Qué cambia en el estado.** Contadores limpios por nodo; falta examinar las cuatro posiciones correspondientes.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Contadores limpios por nodo; falta examinar las cuatro posiciones correspondientes.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos contar columna de entrada y fila de salida.


### Paso 4 — Contar columna de entrada y fila de salida

**Antes de escribir, comprendamos la necesidad.** Cada posible vecino puede originar un arco hacia el nodo o recibirlo desde él.

**Dónde y cómo escribir.** En `EJ03_Dirigido.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            for (int otro = 0; otro < nombres.length; otro++) {
                salida += matriz[nodo][otro];
                entrada += matriz[otro][nodo];
            }
            System.out.println(nombres[nodo] + " entrada=" + entrada + " salida=" + salida);
        }
```
**Qué hace cada elemento nuevo y por qué.** `matriz[nodo][otro]` recorre horizontalmente la fila, por lo que suma salidas. `matriz[otro][nodo]` recorre verticalmente la columna y suma entradas. La suma de ceros y unos es el grado correspondiente; se informa al terminar cada barrido interior.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int otro=0; otro<nombres.length; otro++)` inspecciona todos los posibles vecinos del nodo en curso. En una matriz hay que examinar cuatro celdas para sumar cada orientación.
- `salida += matriz[nodo][otro];` recorre la fila de origen fijo. Cada 1 suma una salida; por ejemplo, desde D solo aparece D→C.
- `entrada += matriz[otro][nodo];` recorre la columna de destino fijo. Para C cuentan B→C y D→C, por eso `entrada=2` aunque su salida sea 1.
- `System.out.println(...)` coloca juntos nombre, entrada y salida para poder contrastarlos con el dibujo. La llave posterior cierra el bucle externo: ya se calcularon los cuatro nodos.

**Qué cambia en el estado.** El programa calcula in/out para A, B, C y D y muestra los valores previstos.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El programa calcula in/out para A, B, C y D y muestra los valores previstos.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos mostrar el ciclo del ejemplo y cerrar el programa.


### Paso 5 — Mostrar el ciclo del ejemplo y cerrar el programa

**Antes de escribir, comprendamos la necesidad.** Los grados por sí solos no revelan el camino concreto que forma un ciclo.

**Dónde y cómo escribir.** En `EJ03_Dirigido.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        System.out.println("ciclo A->B->C->A");
    }
}
```
**Qué hace cada elemento nuevo y por qué.** El texto declara la secuencia de aristas que acabamos de registrar; A→B, B→C y C→A existen. Es una demostración de este caso, NO un detector de ciclos general. Las llaves cierran `main` y la clase.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `System.out.println("ciclo A->B->C->A");` imprime una observación del caso de prueba: la secuencia de arcos A→B, B→C y C→A regresa al origen. No detecta ciclos automáticamente, solo informa el ciclo que previamente construimos.
- Las llaves finales terminan `main` y la clase. El detector automático de ciclos se trabajará después, en un ejercicio diferente.

**Qué cambia en el estado.** Podemos contrastar grado, camino y ciclo dentro de un grafo dirigido válido.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Podemos contrastar grado, camino y ciclo dentro de un grafo dirigido válido.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

Antes: 4×4 ceros. Agregar 0→1 solo marca [0][1]. C recibe B→C y D→C, así que su entrada es 2; solo expulsa C→A.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Crear la clase y una matriz de direcciones | Un arco dirigido tiene origen y destino; una matriz puede expresar ambos con el orden de índices. | Todavía no hay aristas; el grafo dirigido está vacío. |
| Paso 2: Añadir cuatro arcos con un único sentido | Necesitamos un ciclo y una entrada extra para observar la diferencia entre grado entrante y saliente. | C tiene dos entradas (B y D) y una salida (A). |
| Paso 3: Preparar un contador por cada sentido en cada nodo | El número de arcos que entra a un nodo no se obtiene sumando su fila. | Contadores limpios por nodo; falta examinar las cuatro posiciones correspondientes. |
| Paso 4: Contar columna de entrada y fila de salida | Cada posible vecino puede originar un arco hacia el nodo o recibirlo desde él. | El programa calcula in/out para A, B, C y D y muestra los valores previstos. |
| Paso 5: Mostrar el ciclo del ejemplo y cerrar el programa | Los grados por sí solos no revelan el camino concreto que forma un ciclo. | Podemos contrastar grado, camino y ciclo dentro de un grafo dirigido válido. |


### Archivo completo al terminar EJ03

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
public class EJ03_Dirigido {
    public static void main(String[] args) {
        String[] nombres = {"A", "B", "C", "D"};
        int[][] matriz = new int[4][4];
        matriz[0][1] = 1;
        matriz[1][2] = 1;
        matriz[2][0] = 1;
        matriz[3][2] = 1;
        for (int nodo = 0; nodo < nombres.length; nodo++) {
            int entrada = 0;
            int salida = 0;
            for (int otro = 0; otro < nombres.length; otro++) {
                salida += matriz[nodo][otro];
                entrada += matriz[otro][nodo];
            }
            System.out.println(nombres[nodo] + " entrada=" + entrada + " salida=" + salida);
        }
        System.out.println("ciclo A->B->C->A");
    }
}
```

### Predice antes de ejecutar

¿Por qué el grado de entrada de C es dos y su grado de salida es uno?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ03` y ejecuta:

```bash
javac -encoding UTF-8 EJ03_Dirigido.java
java EJ03_Dirigido
```
**Salida de referencia:**

```text
A entrada=1 salida=1
B entrada=1 salida=1
C entrada=2 salida=1
D entrada=0 salida=1
ciclo A->B->C->A
```
**Interpretación:** La columna de un nodo reúne aristas entrantes; su fila reúne salientes. El recorrido A-B-C-A cierra un ciclo dirigido.

**Variación razonada:** Borra la arista D→C: la entrada de C se reduce a uno; el ciclo A→B→C→A permanece.

**Error o caso límite:** Copiar la segunda asignación simétrica del ejemplo no dirigido convertiría cada arista en una relación de doble sentido.

**Corrección:** Registrar solamente `matriz[origen][destino] = 1` en la operación conectarDirigido.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T03 — tarea espejo de EJ03

**Enunciado:** Crea las aristas A→B, A→C, C→D. Muestra grados de entrada/salida de los cuatro vértices y explica por qué D tiene entrada uno y salida cero.

**Archivo nuevo:** `S13_Grafos/T03/T03_Dirigido.java`. No modifiques el programa resuelto `EJ03_Dirigido.java`; crea una carpeta nueva `T03` al lado de `EJ03`.

**Pista:** Modifica exclusivamente los tres enlaces y vuelve a contar filas (salida) y columnas (entrada).

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
public class T03_Dirigido {
    public static void main(String[] args) {
        int[][] m = new int[4][4];
        m[0][1] = 1;
        // TODO T03: registrar A->C y C->D.
        // TODO T03: calcular entrada y salida de A,B,C,D.
        System.out.println("Completa conteos dirigidos T03.");
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T03_Dirigido.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** A entrada=0 salida=2; B entrada=1 salida=0; C entrada=1 salida=1; D entrada=1 salida=0. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ04 — Explorar en profundidad con recursión y visitados

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** Construiremos un recorrido en profundidad sobre cuatro vértices utilizando listas de adyacencia. Partiremos de A y seguiremos una conexión hasta donde podamos antes de regresar a probar otra rama. Necesitamos dos piezas de control: un arreglo booleano para no repetir nodos y un método recursivo que conserve dónde quedó cada llamada al regresar. Registraremos cuatro aristas en orden para poder predecir la secuencia exacta A, B, D, C. La salida permitirá verificar la exploración y entender por qué un vértice ya visitado no causa un recorrido infinito.

**Qué necesitas comprender antes de escribir:** La dificultad de DFS no es imprimir letras: consiste en **no repetir un nodo** y saber cuándo entrar a una llamada y cuándo regresar. Marcaremos antes de descender y seguiremos la pila de llamadas sobre A, B, C y D.

**¿Qué problema concreto resolveremos?** Si una red contiene ciclos, seguir vecinos sin recordar los visitados puede repetir conexiones indefinidamente. DFS necesita memoria de visita además de una forma de profundizar y regresar.

**Caso de trabajo.** Grafo no dirigido A-B, A-C, B-D, C-D, con orden de inserción controlado.

**Archivo a crear:** `S13_Grafos/EJ04/EJ04_DFS.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **7 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Grafo no dirigido A-B, A-C, B-D, C-D, con orden de inserción controlado.


### Paso 1 — Traer colecciones para representar los vecinos

**Antes de escribir, comprendamos la necesidad.** DFS necesita recorrer los vecinos de cada nodo; reutilizaremos las listas del ejercicio anterior para no convertir el recorrido en una búsqueda de toda la fila de una matriz.

**Dónde y cómo escribir.** En la carpeta `EJ04/`, crea el archivo `EJ04_DFS.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.ArrayList;
import java.util.LinkedList;
```
**Qué hace cada elemento nuevo y por qué.** `ArrayList` será el contenedor de las cuatro LinkedList. Estas importaciones ya fueron estudiadas en EJ02 y no incorporan bibliotecas externas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `ArrayList` organiza las listas por índice de vértice; `LinkedList` guarda los vecinos de cada índice. Estas importaciones preparan el tipo de grafo que recorreremos; no contienen por sí mismas un DFS.

**Qué cambia en el estado.** Las estructuras de vecinos están disponibles.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Las estructuras de vecinos están disponibles.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos declarar el punto de entrada y el grafo vacío.


### Paso 2 — Declarar el punto de entrada y el grafo vacío

**Antes de escribir, comprendamos la necesidad.** Para explorar, primero necesitamos un grafo con cuatro índices válidos y una lista inicial para cada uno.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ04_DFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
```
**Qué hace cada elemento nuevo y por qué.** `main` será el punto de ejecución. El ciclo añade las cuatro listas vacías; cada índice de `ady` corresponde a A, B, C o D. En este momento no hay conexiones para visitar.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- La declaración `public class EJ04_DFS` aísla este ejemplo; `main` servirá para crear el caso y llamar al algoritmo.
- `ArrayList<LinkedList<Integer>> ady = new ArrayList<>();` prepara el contenedor; el bucle `for (int i=0; i<4; i++) ady.add(new LinkedList<Integer>());` crea cuatro listas vacías. Al escribir `for` sin llaves, solo la instrucción inmediata se repite; no añadas otras instrucciones a ese bucle sin incorporarlas explícitamente.

**Qué cambia en el estado.** Cuatro listas vacías; la representación está lista para recibir aristas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Cuatro listas vacías; la representación está lista para recibir aristas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos agregar conexiones en un orden que podamos trazar.


### Paso 3 — Agregar conexiones en un orden que podamos trazar

**Antes de escribir, comprendamos la necesidad.** El orden de inserción ayuda a explicar por qué DFS puede tener distintos órdenes válidos para un mismo grafo.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        unir(ady, 0, 1);
        unir(ady, 0, 2);
        unir(ady, 1, 3);
        unir(ady, 2, 3);
```
**Qué hace cada elemento nuevo y por qué.** A se une primero a B y después a C; B se une a D; C también se une a D. `unir` se define al final e insertará los dos extremos; no existe todavía, así que no compilamos hasta completar el archivo.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `unir(ady,0,1)` y `unir(ady,0,2)` colocan B y C como vecinos de A, en ese orden. Ese orden explica que DFS visite primero B y no C.
- `unir(ady,1,3)` conecta B con D; `unir(ady,2,3)` conecta C con D. Al llegar a D desde B, el algoritmo podrá continuar a C antes de volver a A.
- `unir` es el nombre elegido para añadir una arista no dirigida: hace lo mismo que el `conectar` de EJ02. El nombre no cambia la estructura de control de DFS.

**Qué cambia en el estado.** La conectividad ya puede dibujarse: A tiene B,C; B tiene A,D; C tiene A,D; D tiene B,C.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La conectividad ya puede dibujarse: A tiene B,C; B tiene A,D; C tiene A,D; D tiene B,C.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos reservar marcas e iniciar la búsqueda en a.


### Paso 4 — Reservar marcas e iniciar la búsqueda en A

**Antes de escribir, comprendamos la necesidad.** Sin marcas, las aristas de regreso a un nodo anterior harían que lo volviéramos a explorar.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        boolean[] visitado = new boolean[4];
        System.out.print("DFS:");
        dfs(0, ady, visitado);
        System.out.println();
    }
```
**Qué hace cada elemento nuevo y por qué.** `new boolean[4]` inicia las cuatro marcas en false. `dfs(0,...)` elegirá A como origen y compartirá la misma colección `visitado` en las llamadas. `print` mantiene los nodos en una línea; `println()` la cierra. Se cierra `main` pero faltan los métodos.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `boolean[] visitado = new boolean[4];` crea una marca por vértice, inicialmente `false`. Sin marcas, una arista A–B permitiría rebotar indefinidamente de A a B y de B a A.
- `System.out.print("DFS:");` escribe la cabecera sin salto de línea. `dfs(0, ady, visitado)` comienza desde el índice 0, o A, y comparte el arreglo de marcas con todas las llamadas.
- `System.out.println();` completa la línea de consola. La llave cierra `main`; los métodos auxiliares van a continuación en la misma clase.

**Qué cambia en el estado.** Preparados para empezar en A; aún hay que definir cómo profundiza y cómo evita duplicados.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Preparados para empezar en A; aún hay que definir cómo profundiza y cómo evita duplicados.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos marcar un nodo inmediatamente al entrar.


### Paso 5 — Marcar un nodo inmediatamente al entrar

**Antes de escribir, comprendamos la necesidad.** DFS necesita reconocer que ya empezó a procesar un nodo antes de explorar su vecindario.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void dfs(int actual, ArrayList<LinkedList<Integer>> ady, boolean[] visitado) {
        visitado[actual] = true;
        System.out.print(" " + (char)('A' + actual));
```
**Qué hace cada elemento nuevo y por qué.** `actual` es el índice visitado. La asignación `true` se realiza antes de cualquier llamada recursiva; convertir el carácter A más el índice `actual` a `char` muestra A, B, C o D sin un arreglo extra. El método queda abierto mientras escribimos su ciclo.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void dfs(int actual, ..., boolean[] visitado)` define el recorrido recursivo. `actual` indica qué nodo se está explorando en esta llamada; `ady` contiene sus vecinos y `visitado` recuerda los ya explorados.
- `visitado[actual] = true;` se ejecuta **antes** de visitar vecinos. Esta precedencia es el mecanismo que impide repetir nodos al encontrar ciclos o aristas de regreso.
- `System.out.print(" " + (char)('A' + actual));` transforma 0 en A, 1 en B, etc. El tipo `char` es solo presentación: DFS sigue trabajando con enteros.

**Qué cambia en el estado.** Primera llamada: marca A y escribe A. Faltan sus vecinos y las llamadas recursivas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Primera llamada: marca A y escribe A. Faltan sus vecinos y las llamadas recursivas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos explorar solo vecinos aún pendientes.


### Paso 6 — Explorar solo vecinos aún pendientes

**Antes de escribir, comprendamos la necesidad.** Después de imprimir A debemos avanzar a B y después regresar si todas las salidas de B fueron atendidas.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int vecino : ady.get(actual)) {
            if (!visitado[vecino]) {
                dfs(vecino, ady, visitado);
            }
        }
    }
```
**Qué hace cada elemento nuevo y por qué.** `for-each` visita cada índice de la lista del nodo actual. `if (!visitado[vecino])` descarta nodos ya marcados. `dfs(vecino,...)` crea una nueva llamada que avanza por esa rama; al terminar vuelve a la anterior gracias a la pila de llamadas. Las llaves cierran condición, bucle y método.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int vecino : ady.get(actual))` recorre los vecinos almacenados del nodo actual, conservando el orden en el que se añadieron las aristas.
- `if (!visitado[vecino])` permite avanzar solo si la marca es falsa. El operador `!` niega el valor: no visita un nodo que ya se encontró por otra ruta.
- `dfs(vecino, ady, visitado)` produce una nueva llamada, que se apila hasta terminar esa rama. Al retornar continúa con el siguiente vecino pendiente; por esa razón la traza será A B D C, no necesariamente A B C D.
- Las llaves cierran primero la condición, después el bucle y por último `dfs`. No cambian las marcas: estas se conservan en el mismo arreglo durante toda la recursión.

**Qué cambia en el estado.** La traza queda A→B→D→C, sin reingresar a ningún nodo visitado.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La traza queda A→B→D→C, sin reingresar a ningún nodo visitado.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos conectar ambos extremos y cerrar la clase.


### Paso 7 — Conectar ambos extremos y cerrar la clase

**Antes de escribir, comprendamos la necesidad.** Para reproducir el ejemplo original necesitamos que cada arista pueda recorrerse desde cualquiera de sus extremos.

**Dónde y cómo escribir.** En `EJ04_DFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 6**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```
**Qué hace cada elemento nuevo y por qué.** La operación `unir` reutiliza la regla de EJ02: agrega el vecino a las dos listas. El método principal DFS no altera las conexiones; solo consulta `ady` y escribe en `visitado`.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void unir(..., int a, int b)` recibe dos índices y la lista de listas que se modificará. No retorna valores porque el grafo se altera en el sitio.
- `ady.get(a).add(b)` añade el vecino b a a; `ady.get(b).add(a)` completa la conexión no dirigida. Ambas instrucciones son importantes para que la red del dibujo coincida con los datos del algoritmo.
- Las llaves finales cierran `unir` y la clase. Comprueba que `main`, `dfs` y `unir` tienen cada uno su propia apertura y cierre.

**Qué cambia en el estado.** Archivo completo y ejecutable: conectividad separada del algoritmo de recorrido.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Archivo completo y ejecutable: conectividad separada del algoritmo de recorrido.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

visitados=[false,false,false,false] → A=true → B=true → D=true → C=true. Cada llamada vuelve a su invocador al terminar los vecinos pendientes.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Traer colecciones para representar los vecinos | DFS necesita recorrer los vecinos de cada nodo; reutilizaremos las listas del ejercicio anterior para no convertir el recorrido en una búsqueda de toda la fila de una matriz. | Las estructuras de vecinos están disponibles. |
| Paso 2: Declarar el punto de entrada y el grafo vacío | Para explorar, primero necesitamos un grafo con cuatro índices válidos y una lista inicial para cada uno. | Cuatro listas vacías; la representación está lista para recibir aristas. |
| Paso 3: Agregar conexiones en un orden que podamos trazar | El orden de inserción ayuda a explicar por qué DFS puede tener distintos órdenes válidos para un mismo grafo. | La conectividad ya puede dibujarse: A tiene B,C; B tiene A,D; C tiene A,D; D tiene B,C. |
| Paso 4: Reservar marcas e iniciar la búsqueda en A | Sin marcas, las aristas de regreso a un nodo anterior harían que lo volviéramos a explorar. | Preparados para empezar en A; aún hay que definir cómo profundiza y cómo evita duplicados. |
| Paso 5: Marcar un nodo inmediatamente al entrar | DFS necesita reconocer que ya empezó a procesar un nodo antes de explorar su vecindario. | Primera llamada: marca A y escribe A. Faltan sus vecinos y las llamadas recursivas. |
| Paso 6: Explorar solo vecinos aún pendientes | Después de imprimir A debemos avanzar a B y después regresar si todas las salidas de B fueron atendidas. | La traza queda A→B→D→C, sin reingresar a ningún nodo visitado. |
| Paso 7: Conectar ambos extremos y cerrar la clase | Para reproducir el ejemplo original necesitamos que cada arista pueda recorrerse desde cualquiera de sus extremos. | Archivo completo y ejecutable: conectividad separada del algoritmo de recorrido. |


### Archivo completo al terminar EJ04

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class EJ04_DFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
        unir(ady, 0, 1);
        unir(ady, 0, 2);
        unir(ady, 1, 3);
        unir(ady, 2, 3);
        boolean[] visitado = new boolean[4];
        System.out.print("DFS:");
        dfs(0, ady, visitado);
        System.out.println();
    }
    static void dfs(int actual, ArrayList<LinkedList<Integer>> ady, boolean[] visitado) {
        visitado[actual] = true;
        System.out.print(" " + (char)('A' + actual));
        for (int vecino : ady.get(actual)) {
            if (!visitado[vecino]) {
                dfs(vecino, ady, visitado);
            }
        }
    }
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```

### Predice antes de ejecutar

Si empezamos en A y B aparece antes que C, ¿por qué D se visita antes que C?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ04` y ejecuta:

```bash
javac -encoding UTF-8 EJ04_DFS.java
java EJ04_DFS
```
**Salida de referencia:**

```text
DFS: A B D C
```
**Interpretación:** El método procesa A, entra a B, desde B entra a D, desde D alcanza C; después regresa y no repite lo ya visitado.

**Variación razonada:** Invierte el orden de inserción de A-B y A-C: la salida puede empezar A C D B; la conectividad no cambia, sí el orden.

**Error o caso límite:** Quitar la condición `if (!visitado[vecino])` permitiría reentrar en aristas de vuelta y producir recursión sin fin práctico.

**Corrección:** Comprobar `visitado[vecino]` antes de invocar otra vez y marcar el nodo al entrar, no después de explorar.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T04 — tarea espejo de EJ04

**Enunciado:** Construye un grafo no dirigido A-B, A-C, B-D, C-E. Ejecuta DFS desde A; predice y comprueba el recorrido según el orden de inserción.

**Archivo nuevo:** `S13_Grafos/T04/T04_DFS.java`. No modifiques el programa resuelto `EJ04_DFS.java`; crea una carpeta nueva `T04` al lado de `EJ04`.

**Pista:** Usa cinco vértices; marca al entrar y expande vecinos en el mismo orden en que se conectaron.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class T04_DFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 5; i++) ady.add(new LinkedList<Integer>());
        unir(ady,0,1);
        // TODO T04: completar aristas A-C, B-D, C-E.
        boolean[] visitado = new boolean[5];
        System.out.print("DFS:");
        dfs(0, ady, visitado);
        System.out.println();
    }
    static void dfs(int actual, ArrayList<LinkedList<Integer>> ady, boolean[] vis) {
        vis[actual] = true;
        System.out.print(" " + (char)('A' + actual));
        for (int vecino : ady.get(actual)) if (!vis[vecino]) dfs(vecino, ady, vis);
    }
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b); ady.get(b).add(a);
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T04_DFS.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** DFS: A B D C E La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ05 — Encontrar distancias mínimas con una cola BFS

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** DFS profundiza primero, pero cuando todas las aristas cuestan un salto y queremos saber cuántos pasos separan dos nodos, BFS es el recorrido adecuado. Usaremos una cola FIFO para visitar primero los vértices que están a una arista del origen y después los que están a dos o más. Construiremos un grafo de cinco nodos y un arreglo de distancias inicializado con −1, que significará «todavía no alcanzado». Cada vecino descubierto recibirá la distancia de su predecesor más uno y entrará al final de la cola. Comprobaremos que E está a tres saltos de A.

**Qué necesitas comprender antes de escribir:** La distancia no se calcula contando llamadas, sino descubriendo nodos **por niveles**. Explicaremos por qué una cola FIFO mantiene el orden, por qué se usa -1 como centinela y por qué la distancia aumenta exactamente en una arista al descubrir un vecino.

**¿Qué problema concreto resolveremos?** Necesitamos la distancia mínima por número de aristas desde A hasta E en un grafo no ponderado. Un DFS normal no garantiza descubrir la ruta más corta primero.

**Caso de trabajo.** Grafo no dirigido A-B, A-C, B-D, C-D, D-E.

**Archivo a crear:** `S13_Grafos/EJ05/EJ05_BFS.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **8 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Grafo no dirigido A-B, A-C, B-D, C-D, D-E.


### Paso 1 — Importar el contenedor de vecinos, la cola y la utilidad de inicialización

**Antes de escribir, comprendamos la necesidad.** Necesitamos dos estructuras distintas: listas para representar el grafo y una cola FIFO para decidir qué nodo procesar primero.

**Dónde y cómo escribir.** En la carpeta `EJ05/`, crea el archivo `EJ05_BFS.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
```
**Qué hace cada elemento nuevo y por qué.** `Queue` declara el comportamiento de cola, `ArrayDeque` lo implementa sin dependencias. `Arrays.fill` inicializa las distancias en −1. `ArrayList` y `LinkedList` conservan la lista de adyacencia conocida.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `ArrayDeque` es la implementación concreta que utilizará la cola; `Queue` es la interfaz que declara operaciones FIFO como `add` y `remove`.
- `ArrayList` y `LinkedList` construyen listas de vecinos como en los ejemplos anteriores; `Arrays` se usa para inicializar todas las distancias a -1. Es importante distinguir las importaciones que representan el grafo de las que controlan el recorrido.

**Qué cambia en el estado.** Podemos separar almacenamiento de aristas y orden de trabajo.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Podemos separar almacenamiento de aristas y orden de trabajo.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos preparar cinco nodos y sus conexiones.


### Paso 2 — Preparar cinco nodos y sus conexiones

**Antes de escribir, comprendamos la necesidad.** Para comparar distancias necesitamos un grafo fijo y una ruta de A a E con varios saltos.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ05_BFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 5; i++) ady.add(new LinkedList<Integer>());
        unir(ady, 0, 1);
        unir(ady, 0, 2);
        unir(ady, 1, 3);
        unir(ady, 2, 3);
        unir(ady, 3, 4);
```
**Qué hace cada elemento nuevo y por qué.** `ady` contiene cinco listas indexadas A=0 a E=4. Cada `unir` declara una arista no dirigida; D conecta a E y ambos B y C conectan con D. La operación `unir` se implementará al final.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `EJ05_BFS` y `main` crean el ejecutable del caso. `ArrayList<LinkedList<Integer>> ady` es el contenedor de vecinos.
- El bucle de `i=0` a `i<5` crea las cinco listas correspondientes a A, B, C, D y E. Las cinco llamadas `unir` agregan A–B, A–C, B–D, C–D y D–E. Al dibujar, observa que E no es vecino de A: se alcanza en tres saltos.
- Como las conexiones se registran en ambas direcciones, un nodo puede aparecer en varias listas sin que eso deba implicar visitas repetidas en BFS.

**Qué cambia en el estado.** Existen dos caminos de tres aristas A-B-D-E y A-C-D-E; todavía no sabemos la distancia calculada.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Existen dos caminos de tres aristas A-B-D-E y A-C-D-E; todavía no sabemos la distancia calculada.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos crear la distancia desconocida y ejecutar bfs desde a.


### Paso 3 — Crear la distancia desconocida y ejecutar BFS desde A

**Antes de escribir, comprendamos la necesidad.** El algoritmo necesita diferenciar un nodo ya descubierto de otro todavía inalcanzable.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        int[] distancia = new int[5];
        Arrays.fill(distancia, -1);
        bfs(0, ady, distancia);
```
**Qué hace cada elemento nuevo y por qué.** `distancia[i]` será la cantidad de aristas desde A hasta i. El valor −1 indica que aún no se descubrió, y 0 se reservará para el origen. `bfs(0,...)` compartirá el mismo arreglo y completará los valores.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `int[] distancia = new int[5];` reserva un valor por vértice. No queremos que el cero predeterminado signifique 'no visitado', porque el punto de inicio necesita precisamente distancia cero.
- `Arrays.fill(distancia, -1);` utiliza -1 para 'todavía no descubierto'; cada posición comienza con una condición inequívoca.
- `bfs(0, ady, distancia);` inicia en A. El método escribirá los valores definitivos en el mismo arreglo de distancias y regresará al terminar; no hace falta recibir un segundo arreglo.

**Qué cambia en el estado.** Antes de BFS todas las posiciones son −1; el método debe transformar las que sean alcanzables.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Antes de BFS todas las posiciones son −1; el método debe transformar las que sean alcanzables.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos imprimir distancias que puedan contrastarse con un dibujo.


### Paso 4 — Imprimir distancias que puedan contrastarse con un dibujo

**Antes de escribir, comprendamos la necesidad.** Una traza sin resultados medibles no permite distinguir BFS de una simple impresión de nodos.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int i = 0; i < distancia.length; i++) {
            System.out.println((char)('A' + i) + " distancia=" + distancia[i]);
        }
    }
```
**Qué hace cada elemento nuevo y por qué.** El ciclo recorre los cinco índices y transforma 0 en A, 1 en B, etc. El arreglo escrito por BFS se mostrará completo; el método `main` termina aquí, pero la clase no.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int i=0; i<distancia.length; i++)` imprime cinco resultados, uno por nodo, sin depender de un número escrito a mano en la condición.
- `(char)('A'+i)` convierte el índice a su etiqueta. `distancia[i]` será 0 para A, 1 para B y C, 2 para D y 3 para E. El texto 'distancia' significa **cantidad de aristas recorridas**, no metros ni costo ponderado.
- Las dos llaves finales cierran el bucle y `main`, porque el recorrido BFS se definirá fuera de este método.

**Qué cambia en el estado.** Esperamos las distancias 0,1,1,2,3; falta implementar el cálculo.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Esperamos las distancias 0,1,1,2,3; falta implementar el cálculo.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos crear la cola fifo e identificar la capa inicial.


### Paso 5 — Crear la cola FIFO e identificar la capa inicial

**Antes de escribir, comprendamos la necesidad.** BFS debe comenzar por el origen y después atender los nodos en el orden en que se descubrieron.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void bfs(int inicio, ArrayList<LinkedList<Integer>> ady, int[] distancia) {
        Queue<Integer> cola = new ArrayDeque<>();
        distancia[inicio] = 0;
        cola.add(inicio);
```
**Qué hace cada elemento nuevo y por qué.** `cola` guarda índices pendientes, no aristas. `distancia[inicio]=0` marca A como descubierto y fija la capa cero. `cola.add(inicio)` lo coloca al final de la cola; al principio también es el primero.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void bfs(int inicio, ..., int[] distancia)` recibe origen, adyacencias y arreglo que actualizará. El nombre `inicio` evita confundir el punto de partida con el nodo atendido en cada iteración.
- `Queue<Integer> cola = new ArrayDeque<>();` declara una cola FIFO de índices. `Queue` permite expresar la intención y `ArrayDeque` almacena efectivamente las entradas.
- `distancia[inicio] = 0;` descubre A antes de añadirlo a la cola. `cola.add(inicio);` programa su procesamiento. Al comenzar, la cola contiene solo `[A]` y la distancia de A es 0.

**Qué cambia en el estado.** Estado: cola=[A] y dist(A)=0; los otros cuatro valen −1.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado: cola=[A] y dist(A)=0; los otros cuatro valen −1.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos extraer un vértice y examinar sus vecinos.


### Paso 6 — Extraer un vértice y examinar sus vecinos

**Antes de escribir, comprendamos la necesidad.** Mientras existan vértices pendientes debemos tomar el más antiguo, no el último añadido.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        while (!cola.isEmpty()) {
            int actual = cola.remove();
            for (int vecino : ady.get(actual)) {
                if (distancia[vecino] == -1) {
```
**Qué hace cada elemento nuevo y por qué.** `while` termina al vaciarse la cola. `remove()` saca el primero, por eso es FIFO. `for-each` examina vecinos del nodo extraído. La condición −1 impide descubrir dos veces el mismo vecino; dejamos abiertas las llaves para añadir el efecto del descubrimiento.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `while (!cola.isEmpty())` mantiene el recorrido mientras queden nodos por procesar; terminará cuando la cola se vacíe.
- `int actual = cola.remove();` saca el elemento más antiguo. Si se añadieron B y luego C, se atenderá B primero; por eso BFS avanza en niveles.
- `for (int vecino : ady.get(actual))` examina cada vecino del nodo extraído. `if (distancia[vecino] == -1)` permite descubrirlo solo una vez: un valor no negativo significa que ya tiene distancia asignada.
- La condición queda abierta para que el siguiente paso escriba la distancia del nuevo vecino y lo encole. No compiles todavía porque faltan varias llaves de cierre.

**Qué cambia en el estado.** En A aparecen B y C no descubiertos. Debemos asignar su nivel e incorporarlos a la cola.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «En A aparecen B y C no descubiertos. Debemos asignar su nivel e incorporarlos a la cola.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos registrar cada nuevo nivel y encolar una sola vez.


### Paso 7 — Registrar cada nuevo nivel y encolar una sola vez

**Antes de escribir, comprendamos la necesidad.** El vecino conectado directamente al nodo actual se encuentra exactamente una arista más lejos en el primer descubrimiento.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 6**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
                    distancia[vecino] = distancia[actual] + 1;
                    cola.add(vecino);
                }
            }
        }
    }
```
**Qué hace cada elemento nuevo y por qué.** `distancia[actual] + 1` cuenta el nuevo salto. `cola.add(vecino)` aplaza su expansión hasta después de los anteriores de la misma capa; la condición evita duplicados. Las llaves cierran condición, recorrido de vecinos, cola y método.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `distancia[vecino] = distancia[actual] + 1;` asigna un nivel más al recién descubierto. Si actual=D tiene 2, su vecino nuevo E recibe 3. Ese `+1` representa una arista de costo uniforme.
- `cola.add(vecino);` inserta el nodo descubierto al final, por lo que será procesado después de los que ya esperaban. Asignar la distancia **antes de encolar** evita meter el mismo nodo dos veces por caminos distintos.
- Las cuatro llaves cierran en orden `if`, `for`, `while` y `bfs`. Cuando la cola quede vacía, toda distancia accesible ya estará fijada.

**Qué cambia en el estado.** Tras procesar las capas sucesivas, la distancia de E es tres.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Tras procesar las capas sucesivas, la distancia de E es tres.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos completar aristas no dirigidas y cerrar la clase.


### Paso 8 — Completar aristas no dirigidas y cerrar la clase

**Antes de escribir, comprendamos la necesidad.** BFS consulta listas existentes; no debe preocuparse por cómo se crearon las aristas.

**Dónde y cómo escribir.** En `EJ05_BFS.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 7**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```
**Qué hace cada elemento nuevo y por qué.** El método auxiliar agrega cada conexión en ambos sentidos, como EJ02 y EJ04. Mantenerlo separado evita mezclar construcción del grafo con la lógica FIFO y la distancia.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `unir(ArrayList<LinkedList<Integer>> ady, int a, int b)` implementa la operación de conexión para esta demostración.
- `ady.get(a).add(b)` y `ady.get(b).add(a)` registran la relación bidireccional. BFS usaría una lógica similar con un grafo dirigido, pero aquí los vínculos son no dirigidos.
- Las llaves cierran el método auxiliar y la clase. Verifica que E solo tenga a D como vecino: su distancia depende del camino A→B→D→E o A→C→D→E.

**Qué cambia en el estado.** El programa compila y contiene un recorrido BFS completo sobre cinco vértices.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El programa compila y contiene un recorrido BFS completo sobre cinco vértices.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

Cola [A]; descubrir B y C → [B,C]. Procesar B → agrega D. Procesar C → D ya marcado. Procesar D → agrega E. Distancias finales 0,1,1,2,3.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Importar el contenedor de vecinos, la cola y la utilidad de inicialización | Necesitamos dos estructuras distintas: listas para representar el grafo y una cola FIFO para decidir qué nodo procesar primero. | Podemos separar almacenamiento de aristas y orden de trabajo. |
| Paso 2: Preparar cinco nodos y sus conexiones | Para comparar distancias necesitamos un grafo fijo y una ruta de A a E con varios saltos. | Existen dos caminos de tres aristas A-B-D-E y A-C-D-E; todavía no sabemos la distancia calculada. |
| Paso 3: Crear la distancia desconocida y ejecutar BFS desde A | El algoritmo necesita diferenciar un nodo ya descubierto de otro todavía inalcanzable. | Antes de BFS todas las posiciones son −1; el método debe transformar las que sean alcanzables. |
| Paso 4: Imprimir distancias que puedan contrastarse con un dibujo | Una traza sin resultados medibles no permite distinguir BFS de una simple impresión de nodos. | Esperamos las distancias 0,1,1,2,3; falta implementar el cálculo. |
| Paso 5: Crear la cola FIFO e identificar la capa inicial | BFS debe comenzar por el origen y después atender los nodos en el orden en que se descubrieron. | Estado: cola=[A] y dist(A)=0; los otros cuatro valen −1. |
| Paso 6: Extraer un vértice y examinar sus vecinos | Mientras existan vértices pendientes debemos tomar el más antiguo, no el último añadido. | En A aparecen B y C no descubiertos. Debemos asignar su nivel e incorporarlos a la cola. |
| Paso 7: Registrar cada nuevo nivel y encolar una sola vez | El vecino conectado directamente al nodo actual se encuentra exactamente una arista más lejos en el primer descubrimiento. | Tras procesar las capas sucesivas, la distancia de E es tres. |
| Paso 8: Completar aristas no dirigidas y cerrar la clase | BFS consulta listas existentes; no debe preocuparse por cómo se crearon las aristas. | El programa compila y contiene un recorrido BFS completo sobre cinco vértices. |


### Archivo completo al terminar EJ05

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
public class EJ05_BFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 5; i++) ady.add(new LinkedList<Integer>());
        unir(ady, 0, 1);
        unir(ady, 0, 2);
        unir(ady, 1, 3);
        unir(ady, 2, 3);
        unir(ady, 3, 4);
        int[] distancia = new int[5];
        Arrays.fill(distancia, -1);
        bfs(0, ady, distancia);
        for (int i = 0; i < distancia.length; i++) {
            System.out.println((char)('A' + i) + " distancia=" + distancia[i]);
        }
    }
    static void bfs(int inicio, ArrayList<LinkedList<Integer>> ady, int[] distancia) {
        Queue<Integer> cola = new ArrayDeque<>();
        distancia[inicio] = 0;
        cola.add(inicio);
        while (!cola.isEmpty()) {
            int actual = cola.remove();
            for (int vecino : ady.get(actual)) {
                if (distancia[vecino] == -1) {
                    distancia[vecino] = distancia[actual] + 1;
                    cola.add(vecino);
                }
            }
        }
    }
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```

### Predice antes de ejecutar

¿Qué nodo recibe distancia 2? ¿Por qué E no puede recibir 1?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ05` y ejecuta:

```bash
javac -encoding UTF-8 EJ05_BFS.java
java EJ05_BFS
```
**Salida de referencia:**

```text
A distancia=0
B distancia=1
C distancia=1
D distancia=2
E distancia=3
```
**Interpretación:** BFS procesa nodos por capas: origen 0, vecinos 1, D 2 y E 3. La distancia se fija al descubrir por primera vez el nodo.

**Variación razonada:** Quita la conexión D-E: E quedará inalcanzable y su distancia seguirá siendo −1.

**Error o caso límite:** Marcar al extraer en lugar de al encolar puede añadir repetidas veces el mismo vértice; en esta implementación se marca al descubrir (`dist == -1`).

**Corrección:** Asignar la distancia y encolar en el mismo bloque de descubrimiento.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T05 — tarea espejo de EJ05

**Enunciado:** En el mismo grafo, añade una nueva arista A-E y verifica que BFS reduzca la distancia mínima a E de 3 a 1.

**Archivo nuevo:** `S13_Grafos/T05/T05_BFS.java`. No modifiques el programa resuelto `EJ05_BFS.java`; crea una carpeta nueva `T05` al lado de `EJ05`.

**Pista:** Mantén la cola FIFO y los valores −1 iniciales; agrega A-E antes de invocar bfs.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
public class T05_BFS {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 5; i++) ady.add(new LinkedList<Integer>());
        unir(ady,0,1); unir(ady,0,2); unir(ady,1,3); unir(ady,2,3); unir(ady,3,4);
        // TODO T05: agregar arista directa A-E antes de BFS.
        int[] distancia = new int[5]; Arrays.fill(distancia,-1);
        bfs(0,ady,distancia);
        System.out.println("E distancia=" + distancia[4]);
    }
    static void bfs(int inicio,ArrayList<LinkedList<Integer>> ady,int[] dist) {
        Queue<Integer> cola = new ArrayDeque<>(); dist[inicio]=0; cola.add(inicio);
        while(!cola.isEmpty()) {
            int actual=cola.remove();
            for(int v:ady.get(actual)) if(dist[v]==-1) {dist[v]=dist[actual]+1;cola.add(v);}
        }
    }
    static void unir(ArrayList<LinkedList<Integer>> ady,int a,int b) {ady.get(a).add(b);ady.get(b).add(a);}
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T05_BFS.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** E distancia=1 (antes era 3). La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ06 — Detectar un ciclo con DFS y nodo padre

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** Un recorrido puede visitar todos los vértices sin explicar si existe un ciclo. Ahora utilizaremos DFS para detectar una vuelta real en un grafo no dirigido. La dificultad consiste en distinguir entre volver al nodo desde el que acabamos de llegar —situación normal en una arista bidireccional— y encontrar un vecino ya visitado que no sea ese padre. Construiremos un triángulo A-B-C-A y un nodo D unido a C. El método devolverá verdadero cuando aparezca un ciclo. Después comprobaremos que eliminar la arista C-A lo convierte en un camino sin ciclo.

**Qué necesitas comprender antes de escribir:** Un vértice puede volver a ver a su padre porque el grafo es no dirigido; eso NO constituye por sí solo un ciclo. La decisión crítica del detector es reconocer un vecino ya visitado que no es el padre de la llamada actual.

**¿Qué problema concreto resolveremos?** En una red no dirigida cada arista puede recorrerse de ida y vuelta. Sin identificar el padre, DFS detectaría un falso ciclo al regresar por la misma arista.

**Caso de trabajo.** Triángulo A-B-C-A con cola C-D.

**Archivo a crear:** `S13_Grafos/EJ06/EJ06_Ciclo.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **7 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Triángulo A-B-C-A con cola C-D.


### Paso 1 — Importar listas para las conexiones

**Antes de escribir, comprendamos la necesidad.** El detector necesita recorrer vecinos y conocer qué nodo nos devolvió la llamada recursiva.

**Dónde y cómo escribir.** En la carpeta `EJ06/`, crea el archivo `EJ06_Ciclo.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.ArrayList;
import java.util.LinkedList;
```
**Qué hace cada elemento nuevo y por qué.** Usamos exactamente las colecciones ya conocidas: la lista externa posiciona nodos y las LinkedList contienen sus vecinos. No se requiere otra librería para buscar ciclos.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `ArrayList` es la colección exterior que contiene una lista por vértice; `LinkedList` almacena los vecinos. Necesitamos las mismas estructuras que DFS porque la diferencia en esta práctica estará en la condición que identifica un ciclo.

**Qué cambia en el estado.** Podemos construir un grafo cuya conectividad no dependa del detector.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Podemos construir un grafo cuya conectividad no dependa del detector.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos preparar cuatro vértices y crear el triángulo.


### Paso 2 — Preparar cuatro vértices y crear el triángulo

**Antes de escribir, comprendamos la necesidad.** Necesitamos un caso con ciclo seguro y una ramificación que no cree un segundo ciclo.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ06_Ciclo {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
        unir(ady, 0, 1);
        unir(ady, 1, 2);
        unir(ady, 2, 0);
        unir(ady, 2, 3);
```
**Qué hace cada elemento nuevo y por qué.** A=0, B=1, C=2 y D=3. Los tres primeros enlaces forman A-B-C-A. C-D añade un extremo, no un segundo ciclo. `unir` se implementará al final.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- La clase `EJ06_Ciclo` contiene el caso y el detector. La inicialización de `ady` crea el contenedor, mientras que `for (i<4)` agrega una lista vacía por nodo antes de conectar.
- `unir(0,1)`, `unir(1,2)` y `unir(2,0)` construyen el triángulo A–B–C–A: hay un camino que regresa a su origen sin repetir la misma arista.
- `unir(2,3)` conecta D como rama adicional. D no crea un segundo ciclo; permite diferenciar un enlace de árbol de una conexión que cierra un triángulo.

**Qué cambia en el estado.** El modelo tiene ciclo en el triángulo; ahora necesitamos detectarlo algorítmicamente.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El modelo tiene ciclo en el triángulo; ahora necesitamos detectarlo algorítmicamente.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos inicializar marcas y evaluar el componente desde a.


### Paso 3 — Inicializar marcas y evaluar el componente desde A

**Antes de escribir, comprendamos la necesidad.** DFS debe recordar visitados y también cuál era el nodo anterior; el inicio carece de padre real.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        boolean[] visitado = new boolean[4];
        boolean tieneCiclo = detectar(0, -1, ady, visitado);
        System.out.println("tieneCiclo=" + tieneCiclo);
    }
```
**Qué hace cada elemento nuevo y por qué.** `visitado` inicia todo false. `detectar(0,-1,...)` parte de A; `-1` significa «sin padre» y no es índice válido para consultar vecinos. El valor booleano retornado se imprime. Aún falta declarar `detectar`.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `boolean[] visitado = new boolean[4];` almacena qué nodos han sido descubiertos. Esta información por sí sola no basta para decidir si un vecino visitado implica ciclo.
- `detectar(0, -1, ady, visitado)` comienza en A sin padre: usamos el centinela `-1` porque ningún vértice tiene ese índice.
- `boolean tieneCiclo` recibe la respuesta del detector, y `System.out.println` la presenta. La llave cierra `main`, pero falta definir la función recursiva que devuelve un booleano.

**Qué cambia en el estado.** La salida prevista es true; el control lógico todavía está por implementar.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La salida prevista es true; el control lógico todavía está por implementar.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos marcar el nodo antes de examinar sus aristas.


### Paso 4 — Marcar el nodo antes de examinar sus aristas

**Antes de escribir, comprendamos la necesidad.** Si marcamos demasiado tarde, podemos volver a explorar el mismo nodo y perder el control de ciclos.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static boolean detectar(int actual, int padre, ArrayList<LinkedList<Integer>> ady, boolean[] visitado) {
        visitado[actual] = true;
        for (int vecino : ady.get(actual)) {
```
**Qué hace cada elemento nuevo y por qué.** `actual` es el vértice en procesamiento y `padre` el que nos condujo hasta él. El método devuelve boolean: existe o no existe ciclo desde esta exploración. Marca la visita y abre un recorrido por cada vecino.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static boolean detectar(int actual, int padre, ..., boolean[] visitado)` devuelve `true` cuando encuentra un ciclo. `actual` es el nodo inspeccionado y `padre` es el vértice desde el que se llegó a él.
- `visitado[actual] = true` marca el nodo antes del recorrido para evitar visitas repetidas. `for (int vecino : ady.get(actual))` inspecciona todas sus aristas; las reglas para cada vecino aparecen en el siguiente microbloque.

**Qué cambia en el estado.** El detector ya sabe a qué nodo llegó y desde cuál llegó; falta decidir cuándo profundizar.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El detector ya sabe a qué nodo llegó y desde cuál llegó; falta decidir cuándo profundizar.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos profundizar por los vecinos nuevos sin perder el padre.


### Paso 5 — Profundizar por los vecinos nuevos sin perder el padre

**Antes de escribir, comprendamos la necesidad.** Ver a un vecino marcado puede ser normal si es el padre. En cambio, otro vecino marcado revela una vuelta alternativa en un grafo no dirigido simple.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            if (!visitado[vecino]) {
                if (detectar(vecino, actual, ady, visitado)) return true;
            } else if (vecino != padre) {
                return true;
            }
```
**Qué hace cada elemento nuevo y por qué.** La primera rama desciende solo a nodos nuevos y pasa `actual` como padre de la próxima llamada. Si una llamada interna encuentra ciclo, `return true` propaga el hallazgo. `else if (vecino != padre)` acepta como ciclo un vecino ya marcado distinto al padre inmediato.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `if (!visitado[vecino])` identifica una arista hacia un nodo nuevo: debemos seguir explorando con una llamada recursiva.
- `if (detectar(vecino, actual, ady, visitado)) return true;` pasa `actual` como padre del vecino. Si una rama interna detecta ciclo, propaga `true` inmediatamente hasta la llamada inicial.
- `else if (vecino != padre)` resuelve el caso decisivo: un vecino ya visitado que NO es el padre indica una conexión de retorno que forma ciclo en este grafo no dirigido simple.
- `return true;` informa el hallazgo. Si al visitar B vemos de nuevo A, no es ciclo porque A es su padre; si desde C encontramos A ya marcado y el padre es B, sí es ciclo. Esta diferencia evita falsos positivos.

**Qué cambia en el estado.** Desde C se encuentra A ya marcado; el padre de C es B y A≠B, por lo que retorna true.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Desde C se encuentra A ya marcado; el padre de C es B y A≠B, por lo que retorna true.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos declarar el caso sin ciclo y cerrar el detector.


### Paso 6 — Declarar el caso sin ciclo y cerrar el detector

**Antes de escribir, comprendamos la necesidad.** Si terminamos de inspeccionar los vecinos sin encontrar vuelta, debemos comunicar la ausencia de ciclo en esa rama.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        }
        return false;
    }
```
**Qué hace cada elemento nuevo y por qué.** La llave cierra el bucle, `return false` se ejecuta solo cuando ninguna condición devolvió true, y la última llave cierra el método. En un grafo con varios componentes habría que iniciar DFS en cada componente; este ejemplo es conexo y parte de A.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- La primera llave `}` termina el bucle de vecinos; `return false;` significa que en esta rama no se encontró ningún ciclo después de revisar todas sus conexiones.
- La llave final cierra `detectar`. Como el ejemplo parte del nodo A y las cuatro posiciones pertenecen al mismo componente, basta una llamada; para grafos desconectados se requeriría revisar cada componente no visitado, un alcance distinto.

**Qué cambia en el estado.** El método tiene ambas respuestas explícitas: true para ciclo detectado y false para exploración agotada.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El método tiene ambas respuestas explícitas: true para ciclo detectado y false para exploración agotada.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos registrar las aristas bidireccionales y completar el archivo.


### Paso 7 — Registrar las aristas bidireccionales y completar el archivo

**Antes de escribir, comprendamos la necesidad.** Nuestro algoritmo supone aristas no dirigidas y sin duplicados; la representación debe seguir esa convención.

**Dónde y cómo escribir.** En `EJ06_Ciclo.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 6**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```
**Qué hace cada elemento nuevo y por qué.** `unir` agrega ambos extremos exactamente una vez por cada pareja distinta. En un multigrafo con aristas paralelas este detector requeriría distinguir aristas, pero aquí el material trabaja un grafo simple.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- El método `unir` agrega cada arista de la red sin dirección: `ady.get(a).add(b)` y `ady.get(b).add(a)` deben ir juntas.
- Las llaves completan método y clase. El ciclo lo descubre `detectar`, no `unir`; separar representación de algoritmo permite cambiar las conexiones sin reescribir la detección.

**Qué cambia en el estado.** Archivo terminado: ejemplo válido de detección de ciclos en grafo simple, conexo y no dirigido.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Archivo terminado: ejemplo válido de detección de ciclos en grafo simple, conexo y no dirigido.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

A→B (padre A) → C (padre B) → A ya visitado y A≠B → verdadero. La marca se establece antes de explorar.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Importar listas para las conexiones | El detector necesita recorrer vecinos y conocer qué nodo nos devolvió la llamada recursiva. | Podemos construir un grafo cuya conectividad no dependa del detector. |
| Paso 2: Preparar cuatro vértices y crear el triángulo | Necesitamos un caso con ciclo seguro y una ramificación que no cree un segundo ciclo. | El modelo tiene ciclo en el triángulo; ahora necesitamos detectarlo algorítmicamente. |
| Paso 3: Inicializar marcas y evaluar el componente desde A | DFS debe recordar visitados y también cuál era el nodo anterior; el inicio carece de padre real. | La salida prevista es true; el control lógico todavía está por implementar. |
| Paso 4: Marcar el nodo antes de examinar sus aristas | Si marcamos demasiado tarde, podemos volver a explorar el mismo nodo y perder el control de ciclos. | El detector ya sabe a qué nodo llegó y desde cuál llegó; falta decidir cuándo profundizar. |
| Paso 5: Profundizar por los vecinos nuevos sin perder el padre | Ver a un vecino marcado puede ser normal si es el padre. En cambio, otro vecino marcado revela una vuelta alternativa en un grafo no dirigido simple. | Desde C se encuentra A ya marcado; el padre de C es B y A≠B, por lo que retorna true. |
| Paso 6: Declarar el caso sin ciclo y cerrar el detector | Si terminamos de inspeccionar los vecinos sin encontrar vuelta, debemos comunicar la ausencia de ciclo en esa rama. | El método tiene ambas respuestas explícitas: true para ciclo detectado y false para exploración agotada. |
| Paso 7: Registrar las aristas bidireccionales y completar el archivo | Nuestro algoritmo supone aristas no dirigidas y sin duplicados; la representación debe seguir esa convención. | Archivo terminado: ejemplo válido de detección de ciclos en grafo simple, conexo y no dirigido. |


### Archivo completo al terminar EJ06

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class EJ06_Ciclo {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
        unir(ady, 0, 1);
        unir(ady, 1, 2);
        unir(ady, 2, 0);
        unir(ady, 2, 3);
        boolean[] visitado = new boolean[4];
        boolean tieneCiclo = detectar(0, -1, ady, visitado);
        System.out.println("tieneCiclo=" + tieneCiclo);
    }
    static boolean detectar(int actual, int padre, ArrayList<LinkedList<Integer>> ady, boolean[] visitado) {
        visitado[actual] = true;
        for (int vecino : ady.get(actual)) {
            if (!visitado[vecino]) {
                if (detectar(vecino, actual, ady, visitado)) return true;
            } else if (vecino != padre) {
                return true;
            }
        }
        return false;
    }
    static void unir(ArrayList<LinkedList<Integer>> ady, int a, int b) {
        ady.get(a).add(b);
        ady.get(b).add(a);
    }
}
```

### Predice antes de ejecutar

Cuando estamos en B y vemos a A ya visitado, ¿por qué todavía no es un ciclo? ¿Qué cambia cuando desde C vemos a A?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ06` y ejecuta:

```bash
javac -encoding UTF-8 EJ06_Ciclo.java
java EJ06_Ciclo
```
**Salida de referencia:**

```text
tieneCiclo=true
```
**Interpretación:** El nodo padre es la excepción legítima de regreso. Desde C, el vecino A está visitado y NO es su padre B, así que se detecta un ciclo.

**Variación razonada:** Quita la llamada unir(ady, 2, 0) y ejecuta: las aristas A-B, B-C y C-D forman una cadena, así que tieneCiclo=false.

**Error o caso límite:** Considerar cualquier vecino visitado como ciclo da falsos positivos incluso en una única arista A-B no dirigida.

**Corrección:** Añadir la comparación `vecino != padre` cuando el vecino ya se visitó.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T06 — tarea espejo de EJ06

**Enunciado:** Construye una cadena A-B-C-D y confirma que no hay ciclo. Luego agrega D-A y comprueba que el detector indique ciclo.

**Archivo nuevo:** `S13_Grafos/T06/T06_Ciclo.java`. No modifiques el programa resuelto `EJ06_Ciclo.java`; crea una carpeta nueva `T06` al lado de `EJ06`.

**Pista:** Empieza por la cadena: llama a detectar con el arreglo de marcas recién inicializado; para la segunda prueba crea marcas nuevas antes de repetir.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
import java.util.ArrayList;
import java.util.LinkedList;
public class T06_Ciclo {
    public static void main(String[] args) {
        ArrayList<LinkedList<Integer>> ady = new ArrayList<>();
        for (int i = 0; i < 4; i++) ady.add(new LinkedList<Integer>());
        unir(ady,0,1); unir(ady,1,2); unir(ady,2,3);
        System.out.println("sinAristaDA=" + detectar(0,-1,ady,new boolean[4]));
        // TODO T06: agregar arista D-A.
        // TODO T06: ejecutar de nuevo con un arreglo NUEVO de marcas.
    }
    static boolean detectar(int act,int padre,ArrayList<LinkedList<Integer>> ady,boolean[] vis) {
        vis[act]=true;
        for(int v:ady.get(act)) {
            if(!vis[v]) {if(detectar(v,act,ady,vis)) return true;}
            else if(v!=padre) return true;
        }
        return false;
    }
    static void unir(ArrayList<LinkedList<Integer>> ady,int a,int b) {ady.get(a).add(b);ady.get(b).add(a);}
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T06_Ciclo.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** sinAristaDA=false; conAristaDA=true. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ07 — Hallar una ruta en un grid mediante A* y Manhattan

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** En un mapa con obstáculos necesitamos encontrar una ruta entre dos celdas sin probar indiscriminadamente todas las posibilidades. Representaremos un grid de cinco por cinco en el que cero indica paso libre y uno indica pared. Construiremos el algoritmo A* mediante una frontera priorizada por f(n)=g(n)+h(n), donde g es el costo real acumulado y h es la distancia Manhattan que falta hasta la meta. A continuación guardaremos el mejor predecesor de cada celda para reconstruir una secuencia de coordenadas. Verificaremos que la ruta parte de (0,0), llega a (4,4), no atraviesa paredes y tiene un costo consistente con los movimientos realizados.

**Qué necesitas comprender antes de escribir:** A* no avanza simplemente por el vecino más cercano a la meta. Mantiene **costos reales**, **estimaciones** y una **frontera con prioridades**; además recuerda el predecesor de cada celda para reconstruir un camino. Observaremos cuándo se mejora un costo y cuándo se descarta un candidato.

**¿Qué problema concreto resolveremos?** BFS explora por niveles, pero ahora queremos utilizar una estimación que favorezca las celdas prometedoras hacia la meta sin olvidar el costo ya recorrido.

**Caso de trabajo.** Mapa ortogonal 5×5; 0=libre, 1=pared; movimientos solo arriba/abajo/izquierda/derecha con costo 1.

**Archivo a crear:** `S13_Grafos/EJ07/EJ07_AEstrella.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **12 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Mapa ortogonal 5×5; 0=libre, 1=pared; movimientos solo arriba/abajo/izquierda/derecha con costo 1.


### Paso 1 — Traer estructuras para frontera, tablas y reconstrucción

**Antes de escribir, comprendamos la necesidad.** A* debe elegir primero el estado con menor estimación y después recuperar el camino ganador.

**Dónde y cómo escribir.** En la carpeta `EJ07/`, crea el archivo `EJ07_AEstrella.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.PriorityQueue;
```
**Qué hace cada elemento nuevo y por qué.** `PriorityQueue` conserva estados pendientes ordenados por prioridad; `Arrays.fill` ayudará a inicializar costos y `LinkedList` a reconstruir el camino agregando posiciones al principio. No instalamos bibliotecas externas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `Arrays` permitirá inicializar todas las celdas de la matriz `g` con un valor especial; `LinkedList` guardará la ruta reconstruida en orden; `PriorityQueue` administrará las celdas candidatas priorizando f=g+h.
- A diferencia de BFS, no basta una cola FIFO: A* necesita comparar prioridades. La importación no instala librerías externas, todas son clases estándar de Java.

**Qué cambia en el estado.** Ya contamos con las tres utilidades necesarias para calcular, ordenar y mostrar una ruta.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Ya contamos con las tres utilidades necesarias para calcular, ordenar y mostrar una ruta.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos representar un mapa de celdas y movimientos permitidos.


### Paso 2 — Representar un mapa de celdas y movimientos permitidos

**Antes de escribir, comprendamos la necesidad.** Necesitamos convertir el espacio en un grafo cuyos nodos sean celdas transitables.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
public class EJ07_AEstrella {
    static final int[][] MAPA = {
        {0, 0, 0, 0, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0}
    };
    static final int[] DF = {-1, 1, 0, 0};
    static final int[] DC = {0, 0, -1, 1};
```
**Qué hace cada elemento nuevo y por qué.** `MAPA` es una cuadrícula 5×5: cero permite paso y uno es un obstáculo. Las parejas de desplazamientos `DF` y `DC` definen arriba, abajo, izquierda y derecha; no hay diagonales. `static final` conserva el mapa y los movimientos constantes durante la búsqueda.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `public class EJ07_AEstrella` abre un programa autónomo. `static final int[][] MAPA` declara una cuadrícula compartida por los métodos que no se reasignará a otro arreglo; `final` no prohíbe modificar las celdas, aunque no lo haremos.
- Cada fila `{0, ..., 0}` representa una fila del mapa. Valor 0 significa celda transitable y 1 significa obstáculo. El mapa contiene cinco filas de cinco columnas; recorrer la primera columna y la última permite sortear los obstáculos internos.
- `static final int[] DF = {-1,1,0,0}` define desplazamientos de fila; `DC = {0,0,-1,1}` sus desplazamientos de columna correspondientes. Usarlos por pareja en el mismo índice crea arriba, abajo, izquierda y derecha. No autoriza movimientos diagonales.

**Qué cambia en el estado.** El modelo ya restringe posiciones, pero todavía no define origen, destino ni estimación.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El modelo ya restringe posiciones, pero todavía no define origen, destino ni estimación.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos definir origen y destino e interpretar manhattan.


### Paso 3 — Definir origen y destino e interpretar Manhattan

**Antes de escribir, comprendamos la necesidad.** A* necesita una meta concreta y un valor h(n) que estime lo que falta.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    public static void main(String[] args) {
        buscar(4, 4);
    }
    static int h(int fila, int col, int metaF, int metaC) {
        return Math.abs(fila - metaF) + Math.abs(col - metaC);
    }
```
**Qué hace cada elemento nuevo y por qué.** `buscar(4,4)` parte de (0,0), según se definirá en el método. `h` devuelve la suma de distancias absolutas entre filas y columnas: es la heurística Manhattan. Como cada paso permitido cuesta uno y no hay diagonales, esta estimación nunca supera el número mínimo real de pasos en el mapa.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `main` inicia el algoritmo llamando `buscar(4,4)`; la meta será fila 4, columna 4. El origen del programa se fija en (0,0) dentro de `buscar`.
- `static int h(int fila, int col, int metaF, int metaC)` define la heurística para una posición y una meta. `Math.abs(fila-metaF) + Math.abs(col-metaC)` es la distancia Manhattan: número mínimo de movimientos ortogonales sin considerar paredes.
- Manhattan es una **estimación inferior** del costo en este grid de movimientos ortogonales y costo unitario; no es el costo ya pagado `g` y no debe confundirse con él.

**Qué cambia en el estado.** Para (0,0) y (4,4), h=8. Falta incorporar el costo real g y explorar.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Para (0,0) y (4,4), h=8. Falta incorporar el costo real g y explorar.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos crear las tablas del costo real, cierres y predecesores.


### Paso 4 — Crear las tablas del costo real, cierres y predecesores

**Antes de escribir, comprendamos la necesidad.** La heurística indica cercanía estimada, pero sin registrar lo recorrido no podemos calcular f=g+h ni reconstruir una ruta.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void buscar(int metaF, int metaC) {
        int filas = MAPA.length, cols = MAPA[0].length;
        int[][] g = new int[filas][cols];
        for (int[] fila : g) Arrays.fill(fila, Integer.MAX_VALUE);
        boolean[][] cerrado = new boolean[filas][cols];
        int[][] padreF = new int[filas][cols];
        int[][] padreC = new int[filas][cols];
```
**Qué hace cada elemento nuevo y por qué.** `g[f][c]` almacenará el mejor costo conocido hasta una celda; `Integer.MAX_VALUE` representa por ahora desconocido. `cerrado` señala celdas ya procesadas; `padreF` y `padreC` guardan la coordenada previa de la mejor ruta. Los arreglos usan el tamaño real de MAPA.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `static void buscar(int metaF, int metaC)` recibe la coordenada objetivo. Separar la búsqueda de `main` facilita mantener juntos los arreglos de su estado.
- `int filas=MAPA.length, cols=MAPA[0].length` calcula dimensiones reales del mapa. `int[][] g` reservó el mejor costo conocido a cada celda.
- `for (int[] fila : g) Arrays.fill(fila, Integer.MAX_VALUE);` inicializa todos los costos como desconocidos (simulados con un entero enorme). El `for` recorre cada fila de la matriz de costos, NO las celdas del mapa de obstáculos.
- `boolean[][] cerrado` indica qué posiciones ya se extrajeron de la frontera y se procesaron. `padreF` y `padreC` conservan la fila y columna de la celda anterior para reconstruir la ruta final. No son grados ni adyacencias.

**Qué cambia en el estado.** Todas las celdas tienen costo desconocido, no están cerradas y aún no tienen un predecesor útil.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Todas las celdas tienen costo desconocido, no están cerradas y aún no tienen un predecesor útil.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos iniciar la frontera con prioridad f=g+h.


### Paso 5 — Iniciar la frontera con prioridad f=g+h

**Antes de escribir, comprendamos la necesidad.** El origen tiene costo real cero; los demás estados todavía no se han descubierto. Hay que escoger a cuál atender primero.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        PriorityQueue<int[]> abierta = new PriorityQueue<>((a, b) -> Integer.compare(a[2], b[2]));
        g[0][0] = 0;
        abierta.add(new int[]{0, 0, h(0, 0, metaF, metaC)});
```
**Qué hace cada elemento nuevo y por qué.** `abierta` ordena arreglos `{fila,columna,f}` comparando el tercer componente, índice 2. En el origen `g=0`, por eso su prioridad es `h(0,0)`. La expresión lambda compara dos estados por sus estimaciones totales, no por coordenadas.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `PriorityQueue<int[]> abierta = new PriorityQueue<>((a,b) -> Integer.compare(a[2],b[2]))` crea una cola de prioridad. Cada entrada será `[fila, columna, prioridad]`, y el comparador ordena por el tercer campo. No ordena por g puro ni por distancia Manhattan sola.
- `g[0][0] = 0` declara que llegar al origen cuesta cero; sin esta asignación comenzaríamos con un costo desconocido.
- `abierta.add(new int[]{0,0,h(0,0,metaF,metaC)})` añade el origen con prioridad 0+h. La heurística inicial es 8 en el mapa definido. Guardamos una estimación para escoger qué celda procesar primero, no una ruta completa.

**Qué cambia en el estado.** Estado inicial: frontera={(0,0,f=8)} y g(0,0)=0.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado inicial: frontera={(0,0,f=8)} y g(0,0)=0.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos elegir el estado de menor estimación y descartar repetidos.


### Paso 6 — Elegir el estado de menor estimación y descartar repetidos

**Antes de escribir, comprendamos la necesidad.** La cola puede contener entradas antiguas de una misma celda cuando se descubre un costo mejor.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        while (!abierta.isEmpty()) {
            int[] actual = abierta.remove();
            int f = actual[0], c = actual[1];
            if (cerrado[f][c]) continue;
            cerrado[f][c] = true;
```
**Qué hace cada elemento nuevo y por qué.** `remove()` toma el estado de f más pequeño. `f` aquí es la VARIABLE de fila, mientras que la prioridad `f(n)` está en `actual[2]`: no confundirlas. `continue` omite entradas duplicadas de una celda ya cerrada; después se marca como cerrada para no expandirla otra vez.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `while (!abierta.isEmpty())` mantiene la exploración mientras haya candidatos; una frontera vacía significa que no se encontró camino a la meta.
- `int[] actual = abierta.remove()` extrae la celda de menor `f`. La variable `actual` es una terna `[fila,columna,prioridad]`, no un nodo objeto.
- `int f=actual[0], c=actual[1]` extrae coordenadas. Aquí `f` es **fila**; no confundir con el nombre matemático `f(n)=g(n)+h(n)` utilizado para prioridad.
- `if (cerrado[f][c]) continue` descarta entradas duplicadas que quedaron en la cola después de descubrir una ruta mejor. `cerrado[f][c] = true` procesa cada celda elegida a lo sumo una vez bajo la heurística consistente del ejemplo.

**Qué cambia en el estado.** Cada iteración elige una celda abierta y la convierte en procesada; falta comprobar llegada y vecinos.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Cada iteración elige una celda abierta y la convierte en procesada; falta comprobar llegada y vecinos.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos comprobar si hemos llegado a la meta.


### Paso 7 — Comprobar si hemos llegado a la meta

**Antes de escribir, comprendamos la necesidad.** Cuando A* saca la meta como estado óptimo de la frontera ya puede finalizar en este escenario de costos uniformes y heurística consistente.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 6**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            if (f == metaF && c == metaC) {
                mostrarRuta(metaF, metaC, g, padreF, padreC);
                return;
            }
```
**Qué hace cada elemento nuevo y por qué.** La condición compara ambas coordenadas. `mostrarRuta` reconstruirá la secuencia desde la meta usando padres; `return` termina la búsqueda e impide explorar celdas adicionales. El método se definirá después.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `if (f==metaF && c==metaC)` compara ambas coordenadas para verificar que la celda actual sea exactamente la meta; igualar solo una coordenada no alcanza.
- `mostrarRuta(metaF,metaC,g,padreF,padreC)` reconstruye el camino guardado y presenta costo real; `return` detiene `buscar` inmediatamente porque ya alcanzamos la meta.
- Con movimientos ortogonales de costo 1 y Manhattan consistente, extraer la meta con menor prioridad permite detener la búsqueda conservando la optimalidad en este ejemplo.

**Qué cambia en el estado.** Si no estamos en la meta, seguimos con vecinos transitables; la ruta todavía no se imprime.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Si no estamos en la meta, seguimos con vecinos transitables; la ruta todavía no se imprime.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos proponer las cuatro celdas vecinas y filtrar límites.


### Paso 8 — Proponer las cuatro celdas vecinas y filtrar límites

**Antes de escribir, comprendamos la necesidad.** No todas las posiciones geométricamente cercanas son vecinos válidos del grafo: algunas salen de la matriz o son paredes.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 7**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            for (int dir = 0; dir < 4; dir++) {
                int nf = f + DF[dir], nc = c + DC[dir];
                if (nf < 0 || nc < 0 || nf >= filas || nc >= cols) continue;
                if (MAPA[nf][nc] == 1 || cerrado[nf][nc]) continue;
```
**Qué hace cada elemento nuevo y por qué.** `dir` toma los cuatro desplazamientos; `nf,nc` son la fila/columna candidatas. La primera condición evita índices inválidos; la segunda salta obstáculos (`1`) y celdas ya cerradas. Las dos comprobaciones preceden al uso de `g[nf][nc]`.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int dir=0; dir<4; dir++)` examina exactamente cuatro movimientos; la matriz de desplazamientos evita duplicar el mismo bloque cuatro veces.
- `int nf=f+DF[dir], nc=c+DC[dir]` obtiene la posición vecina candidata según la dirección actual. Las letras `nf`/`nc` significan nueva fila y nueva columna.
- `if (nf<0 || nc<0 || nf>=filas || nc>=cols) continue` descarta índices fuera del mapa antes de acceder a `MAPA[nf][nc]`; el operador `||` significa O lógico.
- `if (MAPA[nf][nc]==1 || cerrado[nf][nc]) continue` impide atravesar obstáculos o volver a procesar una celda cerrada. Un `continue` aquí solo pasa a la siguiente dirección; no termina la búsqueda completa.

**Qué cambia en el estado.** Solo quedan celdas dentro del mapa, libres y no procesadas; falta evaluar si ofrecen una ruta mejor.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Solo quedan celdas dentro del mapa, libres y no procesadas; falta evaluar si ofrecen una ruta mejor.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos actualizar g, el predecesor y la prioridad del candidato.


### Paso 9 — Actualizar g, el predecesor y la prioridad del candidato

**Antes de escribir, comprendamos la necesidad.** Hay que evitar reemplazar una ruta mejor por otra más larga hacia la misma celda.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 8**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
                int propuesto = g[f][c] + 1;
                if (propuesto < g[nf][nc]) {
                    g[nf][nc] = propuesto;
                    padreF[nf][nc] = f;
                    padreC[nf][nc] = c;
                    int prioridad = propuesto + h(nf, nc, metaF, metaC);
                    abierta.add(new int[]{nf, nc, prioridad});
                }
```
**Qué hace cada elemento nuevo y por qué.** `propuesto=g actual+1` añade una arista de costo unitario. Solo si es menor que el g anterior sobrescribimos el costo y los padres: así podremos reconstruir el camino que produjo la mejora. `prioridad` implementa f(n)=g(n)+h(n), y la celda se inserta a la frontera con ese valor.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `int propuesto=g[f][c]+1` calcula un costo real tentativo para llegar al vecino desde la celda actual; el +1 es el costo uniforme del movimiento.
- `if (propuesto < g[nf][nc])` compara contra el mejor costo conocido: solo actualizamos cuando encontramos un recorrido más económico para ese vecino.
- `g[nf][nc]=propuesto` memoriza ese menor costo, y `padreF[nf][nc]=f` junto con `padreC[nf][nc]=c` registra la posición desde la que llegamos. Si no guardamos los padres, podríamos conocer el costo pero no reconstruir los pasos.
- `int prioridad = propuesto + h(nf,nc,metaF,metaC)` implementa f=g+h para este candidato. `abierta.add(new int[]{nf,nc,prioridad})` lo inserta para compararlo con otras celdas pendientes.
- La llave final cierra el `if` de mejora. Una misma celda podría aparecer temporalmente varias veces en la cola; la marca `cerrado` evita procesarla de nuevo.

**Qué cambia en el estado.** Una celda nueva pasa de g=infinito a un costo concreto; su padre señala de qué celda proviene.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Una celda nueva pasa de g=infinito a un costo concreto; su padre señala de qué celda proviene.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos cerrar iteraciones y declarar la ausencia de ruta.


### Paso 10 — Cerrar iteraciones y declarar la ausencia de ruta

**Antes de escribir, comprendamos la necesidad.** Si la frontera se agota sin retirar la meta, no existe camino transitable desde el origen bajo estas reglas.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 9**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            }
        }
        System.out.println("sin ruta");
    }
```
**Qué hace cada elemento nuevo y por qué.** Las llaves cierran vecinos y bucle principal; el mensaje `sin ruta` solo se ejecuta cuando `return` nunca terminó la búsqueda. La última llave cierra `buscar`, no la clase.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- La primera llave cierra el bucle de direcciones y la segunda termina el `while`: ambas son necesarias para que `sin ruta` se ejecute solo después de agotar los candidatos.
- `System.out.println("sin ruta");` informa que no se pudo alcanzar la meta desde (0,0). No significa que la cuadrícula entera no tenga celdas libres: puede haber regiones desconectadas.
- La llave cierra `buscar`; el método que imprime la ruta se definirá a continuación, fuera de `buscar`.

**Qué cambia en el estado.** A* puede producir ruta o informar que no la hay. Falta mostrar el camino cuando existe.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «A* puede producir ruta o informar que no la hay. Falta mostrar el camino cuando existe.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos preparar una lista para recorrer los predecesores hacia atrás.


### Paso 11 — Preparar una lista para recorrer los predecesores hacia atrás

**Antes de escribir, comprendamos la necesidad.** La búsqueda termina en la meta, pero los padres apuntan de meta hacia origen; necesitamos mostrar la ruta en sentido contrario.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 10**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
    static void mostrarRuta(int metaF, int metaC, int[][] g, int[][] padreF, int[][] padreC) {
        LinkedList<String> ruta = new LinkedList<>();
        int f = metaF, c = metaC;
        while (!(f == 0 && c == 0)) {
            ruta.addFirst("(" + f + "," + c + ")");
```
**Qué hace cada elemento nuevo y por qué.** `mostrarRuta` recibe la meta, costos y dos matrices de padres calculadas. `ruta` se construye con `addFirst`, colocando primero la celda que aparece más cerca del origen. `while` retrocede hasta (0,0), que se insertará explícitamente después.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `mostrarRuta(..., int[][] g, int[][] padreF, int[][] padreC)` recibe las tablas de costos y predecesores calculadas durante la búsqueda. No vuelve a ejecutar A*.
- `LinkedList<String> ruta = new LinkedList<>();` construye una secuencia textual de coordenadas. Con `addFirst` podremos insertarlas delante mientras retrocedemos del final al inicio.
- `int f=metaF, c=metaC` arranca la reconstrucción desde la meta. `while (!(f==0 && c==0))` repite hasta regresar exactamente a la celda origen (0,0).
- `ruta.addFirst("(" + f + "," + c + ")");` registra la coordenada actual al principio de la lista, lo que invierte el orden del recorrido hacia atrás sin necesitar un `reverse` adicional.

**Qué cambia en el estado.** La lista se llena desde atrás, pero se ordena desde el comienzo; falta actualizar f,c para retroceder.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La lista se llena desde atrás, pero se ordena desde el comienzo; falta actualizar f,c para retroceder.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos retroceder sin perder coordenadas y presentar el resultado.


### Paso 12 — Retroceder sin perder coordenadas y presentar el resultado

**Antes de escribir, comprendamos la necesidad.** Cambiar la fila antes de leer la columna del padre sería peligroso: podríamos consultar otra celda y perder la ruta.

**Dónde y cómo escribir.** En `EJ07_AEstrella.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 11**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            int pf = padreF[f][c], pc = padreC[f][c];
            f = pf;
            c = pc;
        }
        ruta.addFirst("(0,0)");
        System.out.println("costo=" + g[metaF][metaC]);
        System.out.println("ruta=" + ruta);
    }
}
```
**Qué hace cada elemento nuevo y por qué.** `pf` y `pc` capturan ambas coordenadas anteriores antes de modificar `f,c`. Después de llegar a (0,0), se agrega el origen y se imprimen el costo real final y todas las posiciones. Las llaves cierran el método y la clase.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `int pf=padreF[f][c], pc=padreC[f][c]` lee AMBAS coordenadas del predecesor de la celda actual antes de modificar `f` y `c`; si actualizáramos una primero, el otro acceso podría consultar una celda equivocada.
- `f=pf; c=pc;` avanza un enlace hacia atrás. El bucle termina al llegar al origen; `ruta.addFirst("(0,0)")` incorpora el origen que la condición del `while` no añade.
- `System.out.println("costo="+g[metaF][metaC])` presenta el costo acumulado real, no la prioridad estimada f(n). `System.out.println("ruta="+ruta)` muestra el orden origen→meta.
- Las llaves cierran `mostrarRuta` y la clase. Valida la ruta: cada salto cambia exactamente una coordenada en una unidad y ninguna celda de la secuencia tiene valor 1 en `MAPA`.

**Qué cambia en el estado.** La salida muestra costo y camino. El costo debe coincidir con el número de movimientos entre las posiciones consecutivas.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «La salida muestra costo y camino. El costo debe coincidir con el número de movimientos entre las posiciones consecutivas.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

Al inicio g(0,0)=0, h=8. Los vecinos libres se insertan con g=1 y f=1+heurística. Cada mejora actualiza g y padres; la meta cerrada activa reconstrucción.


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Traer estructuras para frontera, tablas y reconstrucción | A* debe elegir primero el estado con menor estimación y después recuperar el camino ganador. | Ya contamos con las tres utilidades necesarias para calcular, ordenar y mostrar una ruta. |
| Paso 2: Representar un mapa de celdas y movimientos permitidos | Necesitamos convertir el espacio en un grafo cuyos nodos sean celdas transitables. | El modelo ya restringe posiciones, pero todavía no define origen, destino ni estimación. |
| Paso 3: Definir origen y destino e interpretar Manhattan | A* necesita una meta concreta y un valor h(n) que estime lo que falta. | Para (0,0) y (4,4), h=8. Falta incorporar el costo real g y explorar. |
| Paso 4: Crear las tablas del costo real, cierres y predecesores | La heurística indica cercanía estimada, pero sin registrar lo recorrido no podemos calcular f=g+h ni reconstruir una ruta. | Todas las celdas tienen costo desconocido, no están cerradas y aún no tienen un predecesor útil. |
| Paso 5: Iniciar la frontera con prioridad f=g+h | El origen tiene costo real cero; los demás estados todavía no se han descubierto. Hay que escoger a cuál atender primero. | Estado inicial: frontera={(0,0,f=8)} y g(0,0)=0. |
| Paso 6: Elegir el estado de menor estimación y descartar repetidos | La cola puede contener entradas antiguas de una misma celda cuando se descubre un costo mejor. | Cada iteración elige una celda abierta y la convierte en procesada; falta comprobar llegada y vecinos. |
| Paso 7: Comprobar si hemos llegado a la meta | Cuando A* saca la meta como estado óptimo de la frontera ya puede finalizar en este escenario de costos uniformes y heurística consistente. | Si no estamos en la meta, seguimos con vecinos transitables; la ruta todavía no se imprime. |
| Paso 8: Proponer las cuatro celdas vecinas y filtrar límites | No todas las posiciones geométricamente cercanas son vecinos válidos del grafo: algunas salen de la matriz o son paredes. | Solo quedan celdas dentro del mapa, libres y no procesadas; falta evaluar si ofrecen una ruta mejor. |
| Paso 9: Actualizar g, el predecesor y la prioridad del candidato | Hay que evitar reemplazar una ruta mejor por otra más larga hacia la misma celda. | Una celda nueva pasa de g=infinito a un costo concreto; su padre señala de qué celda proviene. |
| Paso 10: Cerrar iteraciones y declarar la ausencia de ruta | Si la frontera se agota sin retirar la meta, no existe camino transitable desde el origen bajo estas reglas. | A* puede producir ruta o informar que no la hay. Falta mostrar el camino cuando existe. |
| Paso 11: Preparar una lista para recorrer los predecesores hacia atrás | La búsqueda termina en la meta, pero los padres apuntan de meta hacia origen; necesitamos mostrar la ruta en sentido contrario. | La lista se llena desde atrás, pero se ordena desde el comienzo; falta actualizar f,c para retroceder. |
| Paso 12: Retroceder sin perder coordenadas y presentar el resultado | Cambiar la fila antes de leer la columna del padre sería peligroso: podríamos consultar otra celda y perder la ruta. | La salida muestra costo y camino. El costo debe coincidir con el número de movimientos entre las posiciones consecutivas. |


### Archivo completo al terminar EJ07

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.PriorityQueue;
public class EJ07_AEstrella {
    static final int[][] MAPA = {
        {0, 0, 0, 0, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0}
    };
    static final int[] DF = {-1, 1, 0, 0};
    static final int[] DC = {0, 0, -1, 1};
    public static void main(String[] args) {
        buscar(4, 4);
    }
    static int h(int fila, int col, int metaF, int metaC) {
        return Math.abs(fila - metaF) + Math.abs(col - metaC);
    }
    static void buscar(int metaF, int metaC) {
        int filas = MAPA.length, cols = MAPA[0].length;
        int[][] g = new int[filas][cols];
        for (int[] fila : g) Arrays.fill(fila, Integer.MAX_VALUE);
        boolean[][] cerrado = new boolean[filas][cols];
        int[][] padreF = new int[filas][cols];
        int[][] padreC = new int[filas][cols];
        PriorityQueue<int[]> abierta = new PriorityQueue<>((a, b) -> Integer.compare(a[2], b[2]));
        g[0][0] = 0;
        abierta.add(new int[]{0, 0, h(0, 0, metaF, metaC)});
        while (!abierta.isEmpty()) {
            int[] actual = abierta.remove();
            int f = actual[0], c = actual[1];
            if (cerrado[f][c]) continue;
            cerrado[f][c] = true;
            if (f == metaF && c == metaC) {
                mostrarRuta(metaF, metaC, g, padreF, padreC);
                return;
            }
            for (int dir = 0; dir < 4; dir++) {
                int nf = f + DF[dir], nc = c + DC[dir];
                if (nf < 0 || nc < 0 || nf >= filas || nc >= cols) continue;
                if (MAPA[nf][nc] == 1 || cerrado[nf][nc]) continue;
                int propuesto = g[f][c] + 1;
                if (propuesto < g[nf][nc]) {
                    g[nf][nc] = propuesto;
                    padreF[nf][nc] = f;
                    padreC[nf][nc] = c;
                    int prioridad = propuesto + h(nf, nc, metaF, metaC);
                    abierta.add(new int[]{nf, nc, prioridad});
                }
            }
        }
        System.out.println("sin ruta");
    }
    static void mostrarRuta(int metaF, int metaC, int[][] g, int[][] padreF, int[][] padreC) {
        LinkedList<String> ruta = new LinkedList<>();
        int f = metaF, c = metaC;
        while (!(f == 0 && c == 0)) {
            ruta.addFirst("(" + f + "," + c + ")");
            int pf = padreF[f][c], pc = padreC[f][c];
            f = pf;
            c = pc;
        }
        ruta.addFirst("(0,0)");
        System.out.println("costo=" + g[metaF][metaC]);
        System.out.println("ruta=" + ruta);
    }
}
```

### Predice antes de ejecutar

¿Cuál es h(0,0) hacia (4,4)? ¿Qué diferencia hay entre g y h al evaluar una celda?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ07` y ejecuta:

```bash
javac -encoding UTF-8 EJ07_AEstrella.java
java EJ07_AEstrella
```
**Salida de referencia:**

```text
costo=8
ruta=[(0,0), ... , (4,4)] (nueve coordenadas contiguas, sin paredes)
```
**Nota de validación:** la ruta intermedia puede variar por empates de prioridad. Son obligatorios costo mínimo, inicio/meta, celdas libres y movimientos ortogonales contiguos.

**Interpretación:** La prioridad f no es el costo definitivo: ordena qué expandir. El costo mostrado al llegar proviene de g; los padres reconstruyen el camino.

**Variación razonada:** Cambia la meta de (4,4) a (4,2). Es una prueba de transferencia: Manhattan(0,0→4,2)=6 y debe hallarse costo=6 por celdas libres.

**Error o caso límite:** Utilizar Manhattan con movimientos diagonales de costo 1 puede sobreestimar el camino y perder la garantía didáctica de optimalidad.

**Corrección:** Mantener movimientos ortogonales de costo unitario, comprobar límites y obstáculos, y usar una heurística que no sobreestime.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T07 — tarea espejo de EJ07

**Enunciado:** Reutiliza el algoritmo A* y busca del origen (0,0) al nuevo destino (4,2), conservando mapa y obstáculos. Anota h inicial, costo final y ruta encontrada.

**Archivo nuevo:** `S13_Grafos/T07/T07_AEstrella.java`. No modifiques el programa resuelto `EJ07_AEstrella.java`; crea una carpeta nueva `T07` al lado de `EJ07`.

**Pista:** Solo cambia la llamada de `main` a `buscar(4,2)`; el método `h` utiliza la meta que se le pasa sin más modificaciones.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.PriorityQueue;
public class T07_AEstrella {
    static final int[][] MAPA = {
        {0, 0, 0, 0, 0},
        {0, 1, 1, 1, 0},
        {0, 0, 0, 1, 0},
        {0, 1, 0, 1, 0},
        {0, 0, 0, 0, 0}
    };
    static final int[] DF = {-1, 1, 0, 0};
    static final int[] DC = {0, 0, -1, 1};
    public static void main(String[] args) {
        // TODO T07: cambia la meta a (4,2), predice el costo y comprueba.
        buscar(4, 4);
    }
    static int h(int fila, int col, int metaF, int metaC) {
        return Math.abs(fila - metaF) + Math.abs(col - metaC);
    }
    static void buscar(int metaF, int metaC) {
        int filas = MAPA.length, cols = MAPA[0].length;
        int[][] g = new int[filas][cols];
        for (int[] fila : g) Arrays.fill(fila, Integer.MAX_VALUE);
        boolean[][] cerrado = new boolean[filas][cols];
        int[][] padreF = new int[filas][cols];
        int[][] padreC = new int[filas][cols];
        PriorityQueue<int[]> abierta = new PriorityQueue<>((a, b) -> Integer.compare(a[2], b[2]));
        g[0][0] = 0;
        abierta.add(new int[]{0, 0, h(0, 0, metaF, metaC)});
        while (!abierta.isEmpty()) {
            int[] actual = abierta.remove();
            int f = actual[0], c = actual[1];
            if (cerrado[f][c]) continue;
            cerrado[f][c] = true;
            if (f == metaF && c == metaC) {
                mostrarRuta(metaF, metaC, g, padreF, padreC);
                return;
            }
            for (int dir = 0; dir < 4; dir++) {
                int nf = f + DF[dir], nc = c + DC[dir];
                if (nf < 0 || nc < 0 || nf >= filas || nc >= cols) continue;
                if (MAPA[nf][nc] == 1 || cerrado[nf][nc]) continue;
                int propuesto = g[f][c] + 1;
                if (propuesto < g[nf][nc]) {
                    g[nf][nc] = propuesto;
                    padreF[nf][nc] = f;
                    padreC[nf][nc] = c;
                    int prioridad = propuesto + h(nf, nc, metaF, metaC);
                    abierta.add(new int[]{nf, nc, prioridad});
                }
            }
        }
        System.out.println("sin ruta");
    }
    static void mostrarRuta(int metaF, int metaC, int[][] g, int[][] padreF, int[][] padreC) {
        LinkedList<String> ruta = new LinkedList<>();
        int f = metaF, c = metaC;
        while (!(f == 0 && c == 0)) {
            ruta.addFirst("(" + f + "," + c + ")");
            int pf = padreF[f][c], pc = padreC[f][c];
            f = pf;
            c = pc;
        }
        ruta.addFirst("(0,0)");
        System.out.println("costo=" + g[metaF][metaC]);
        System.out.println("ruta=" + ruta);
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T07_AEstrella.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** h(0,0)=6 y costo=6; ruta válida de siete coordenadas libres del mapa. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## EJ08 — Sugerir productos mediante coincidencias de un grafo bipartito

**Objetivo técnico — qué vamos a hacer y qué aprenderemos.** Los grafos también representan relaciones entre entidades de distinta naturaleza. Construiremos un modelo pequeño con usuarios a un lado y productos al otro: una matriz de relaciones indicará qué producto conoce cada usuario. Después calcularemos cuánto se parece otro usuario al usuario objetivo contando productos compartidos. Esa coincidencia dará votos a productos que el usuario objetivo todavía no tiene. Mostraremos únicamente candidatos con puntuación positiva. La evidencia será comprobar por qué se recomiendan P2 y P3 para U0 y por qué nunca se recomiendan de nuevo los productos que U0 ya conoce.

**Qué necesitas comprender antes de escribir:** El grafo bipartito tiene dos grupos: usuarios y productos. Las coincidencias permiten identificar vecinos con intereses compartidos; los productos nuevos reciben puntajes, pero los que U0 ya eligió deben excluirse. La salida debe poder justificarse recorriendo filas y columnas concretas.

**¿Qué problema concreto resolveremos?** Una lista de productos elegidos por cada usuario permite decidir qué vínculos existen, pero todavía no explica qué producto no conocido podría interesarle a U0.

**Caso de trabajo.** Grafo bipartito de tres usuarios U0,U1,U2 frente a productos P0,P1,P2,P3; una matriz rectangular representa solo relaciones entre ambos grupos.

**Archivo a crear:** `S13_Grafos/EJ08/EJ08_Recomendador.java`. Cada ejemplo es un programa independiente; no sobrescribe el anterior.


### Punto de partida y preparación de este ejemplo

El archivo comienza vacío. No pegues el programa completo por adelantado; construye los siguientes **6 microbloques** de forma secuencial y mantén las llaves exactamente como aparecen. Antes de ejecutar, dibuja el estado inicial indicado en el caso: Grafo bipartito de tres usuarios U0,U1,U2 frente a productos P0,P1,P2,P3; una matriz rectangular representa solo relaciones entre ambos grupos.


### Paso 1 — Declarar los dos grupos de nodos y la matriz de relaciones

**Antes de escribir, comprendamos la necesidad.** Para recomendar con relaciones necesitamos un registro de conexiones usuario-producto; no es una matriz cuadrada de usuarios únicamente.

**Dónde y cómo escribir.** En la carpeta `EJ08/`, crea el archivo `EJ08_Recomendador.java` y escribe estas primeras líneas. Todavía no es un programa completo.

**Escribe ahora exactamente:**

```java
public class EJ08_Recomendador {
    public static void main(String[] args) {
        int[][] gustos = {
            {1, 1, 0, 0},
            {1, 0, 1, 0},
            {0, 1, 0, 1}
        };
```
**Qué hace cada elemento nuevo y por qué.** `gustos[u][p]` vale 1 si el usuario u está relacionado con el producto p y 0 en caso contrario. Las tres filas corresponden U0,U1,U2; las cuatro columnas, P0..P3. Esto es una tabla de incidencia entre dos conjuntos de un grafo bipartito, no la matriz de adyacencia cuadrada de EJ01.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `public class EJ08_Recomendador` y `main` definen un ejemplo ejecutable de relaciones usuario-producto, no un modelo estadístico de aprendizaje automático.
- `int[][] gustos` es una matriz de incidencia: cada fila corresponde a un usuario (U0, U1, U2) y cada columna a un producto (P0, P1, P2, P3). Un 1 significa vínculo/elección y un 0 ausencia de vínculo.
- La fila `{1,1,0,0}` indica que U0 eligió P0 y P1. La fila `{1,0,1,0}` indica que U1 comparte P0 con U0 y también eligió P2. La tercera fila `{0,1,0,1}` indica que U2 comparte P1 con U0 y eligió P3. Esta separación forma un grafo bipartito: no representamos enlaces usuario-usuario ni producto-producto.

**Qué cambia en el estado.** U0 conoce P0 y P1. U1 comparte P0; U2 comparte P1.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «U0 conoce P0 y P1. U1 comparte P0; U2 comparte P1.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos elegir destinatario y reservar puntuaciones.


### Paso 2 — Elegir destinatario y reservar puntuaciones

**Antes de escribir, comprendamos la necesidad.** La recomendación debe estar dirigida a alguien concreto y llevar un contador separado por producto.

**Dónde y cómo escribir.** En `EJ08_Recomendador.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 1**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        int objetivo = 0;
        int[] puntaje = new int[gustos[0].length];
```
**Qué hace cada elemento nuevo y por qué.** `objetivo=0` designa U0. `gustos[0].length` mide cuatro productos; `puntaje` inicia con cuatro ceros. Ningún producto tiene votos todavía.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `int objetivo=0` selecciona U0 como persona a la que sugeriremos productos; el número es índice de fila, no un puntaje.
- `int[] puntaje = new int[gustos[0].length]` crea un contador por producto (cuatro posiciones), inicialmente en cero. Lo dimensionamos con el largo de una fila porque queremos puntuar columnas de producto, no filas de usuario.

**Qué cambia en el estado.** Estado inicial puntaje=[0,0,0,0]. Falta comparar al usuario con sus pares.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Estado inicial puntaje=[0,0,0,0]. Falta comparar al usuario con sus pares.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos comparar u0 con cada usuario distinto.


### Paso 3 — Comparar U0 con cada usuario distinto

**Antes de escribir, comprendamos la necesidad.** Comparar U0 consigo mismo no aporta evidencia de preferencia externa y aumentaría artificialmente las puntuaciones.

**Dónde y cómo escribir.** En `EJ08_Recomendador.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 2**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int otro = 0; otro < gustos.length; otro++) {
            if (otro == objetivo) continue;
            int coincidencias = 0;
```
**Qué hace cada elemento nuevo y por qué.** `otro` recorre las tres filas; `continue` omite U0. `coincidencias` comienza en cero para cada candidato distinto, de modo que los productos compartidos de U1 no se acumulen incorrectamente con los de U2.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int otro=0; otro<gustos.length; otro++)` itera por los tres usuarios para comparar cada uno con U0. `gustos.length` es cantidad de filas/usuarios.
- `if (otro==objetivo) continue;` evita que U0 se compare consigo mismo: ese caso no generaría evidencia de otra persona para recomendar.
- `int coincidencias=0` reinicia el número de productos comunes para cada usuario comparado. Debe declararse dentro del bucle externo para no acumular equivocadamente coincidencias de diferentes usuarios.

**Qué cambia en el estado.** Se examinarán U1 y U2 por separado; aún no sabemos cuánto comparten.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Se examinarán U1 y U2 por separado; aún no sabemos cuánto comparten.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos contar productos que ambos usuarios comparten.


### Paso 4 — Contar productos que ambos usuarios comparten

**Antes de escribir, comprendamos la necesidad.** Para que una relación de otro usuario influya en el objetivo, necesitamos evidencia de preferencias compartidas.

**Dónde y cómo escribir.** En `EJ08_Recomendador.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 3**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            for (int p = 0; p < gustos[otro].length; p++) {
                if (gustos[objetivo][p] == 1 && gustos[otro][p] == 1) {
                    coincidencias++;
                }
            }
```
**Qué hace cada elemento nuevo y por qué.** `p` recorre las cuatro columnas; `&&` exige que U0 y el otro usuario estén vinculados al mismo producto. El contador crece una vez por cada coincidencia verdadera. U1 comparte P0; U2 comparte P1, así que ambos tienen coincidencias=1.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int p=0; p<gustos[otro].length; p++)` examina cada producto. Aquí `p` representa columna y `gustos[otro].length` es el número de productos disponibles.
- `gustos[objetivo][p]==1 && gustos[otro][p]==1` exige que ambos usuarios hayan elegido el mismo producto; `&&` significa Y lógico.
- `coincidencias++` incrementa una vez por cada producto compartido. U1 y U0 coinciden en P0; U2 y U0 coinciden en P1, así que cada comparación aporta coincidencias=1.
- Las llaves cierran la condición y el bucle de productos: todavía no se han asignado puntos a ninguna recomendación.

**Qué cambia en el estado.** Conocemos el peso de similitud de cada otro usuario, pero aún falta identificar candidatos nuevos.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Conocemos el peso de similitud de cada otro usuario, pero aún falta identificar candidatos nuevos.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos otorgar votos solo a productos nuevos para el objetivo.


### Paso 5 — Otorgar votos solo a productos nuevos para el objetivo

**Antes de escribir, comprendamos la necesidad.** Compartir un producto solo sirve si además ese usuario conoce algo que U0 aún no conoce.

**Dónde y cómo escribir.** En `EJ08_Recomendador.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 4**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
            if (coincidencias > 0) {
                for (int p = 0; p < gustos[otro].length; p++) {
                    if (gustos[objetivo][p] == 0 && gustos[otro][p] == 1) {
                        puntaje[p] += coincidencias;
                    }
                }
            }
        }
```
**Qué hace cada elemento nuevo y por qué.** La primera condición exige similitud positiva. El segundo bucle localiza productos donde U0 tiene 0 y el vecino 1. `puntaje[p] += coincidencias` aporta tantos votos como coincidencias comparten; U1 aporta P2 y U2 aporta P3. Se cierran condiciones y bucles de usuarios.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `if (coincidencias>0)` exige evidencia de interés compartido antes de utilizar las elecciones del otro usuario. No queremos recomendar a partir de usuarios sin productos comunes con U0.
- `for (int p=0; p<gustos[otro].length; p++)` repasa las columnas, pero ahora buscando candidatos recomendables, no contando coincidencias.
- `gustos[objetivo][p]==0 && gustos[otro][p]==1` significa: el objetivo aún NO tiene el producto y el otro usuario SÍ lo tiene. Esta exclusión evita recomendar P0 y P1, ya elegidos por U0.
- `puntaje[p] += coincidencias` suma las coincidencias como voto para el candidato. U1 aporta un punto a P2 y U2 un punto a P3. Las llaves cierran condición interna, bucle y filtro de coincidencias, y finalmente la comparación del usuario actual.

**Qué cambia en el estado.** Puntuaciones finales internas: P0=0, P1=0, P2=1, P3=1.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «Puntuaciones finales internas: P0=0, P1=0, P2=1, P3=1.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos presentar recomendaciones con respaldo real.


### Paso 6 — Presentar recomendaciones con respaldo real

**Antes de escribir, comprendamos la necesidad.** Un puntaje cero no constituye una recomendación basada en la red de relaciones.

**Dónde y cómo escribir.** En `EJ08_Recomendador.java`, inserta este fragmento **inmediatamente después de la última línea del Paso 5**, conservando la indentación y todas las llaves aún abiertas. No agregues llaves de cierre por tu cuenta: llegarán en los pasos indicados.

**Escribe ahora exactamente:**

```java
        for (int p = 0; p < puntaje.length; p++) {
            if (puntaje[p] > 0) {
                System.out.println("recomendar P" + p + " puntaje=" + puntaje[p]);
            }
        }
    }
}
```
**Qué hace cada elemento nuevo y por qué.** `for` examina productos P0..P3 y `if (>0)` imprime únicamente candidatos con algún respaldo. El nombre se forma con el índice `p`; la salida conserva el valor calculado para que se pueda justificar cada recomendación.

**Lectura razonada del microbloque — qué significa cada línea y por qué la escribimos:**

- `for (int p=0; p<puntaje.length; p++)` visita todos los productos para determinar qué resultados merecen aparecer.
- `if (puntaje[p]>0)` filtra productos sin respaldo de usuarios similares. En este caso P2 y P3 tienen cada uno un punto; no afirmamos que un puntaje 1 equivalga a una probabilidad.
- `System.out.println("recomendar P"+p+" puntaje="+puntaje[p])` imprime identificador y puntaje. La suma de coincidencias es una regla pedagógica simple de recomendación basada en grafos, no un sistema industrial de personalización.
- Las llaves finales cierran condición, bucle de salida, `main` y clase. Comprueba que la salida nunca recomienda un producto ya elegido por U0.

**Qué cambia en el estado.** El programa muestra exactamente dos productos nuevos para U0, cada uno con puntaje uno.

**Control de comprensión:** Antes de continuar, señala la instrucción que produce «El programa muestra exactamente dos productos nuevos para U0, cada uno con puntaje uno.» y explica cuál de las variables o condiciones anteriores hace posible ese cambio. Si desaparece una instrucción esencial de este bloque, anticipa la consecuencia antes de compilar.

**Conexión con lo siguiente.** Para completar esta construcción aún necesitamos compilar el archivo, ejecutar la prueba y explicar el resultado.


### Traza manual: qué cambia y en qué orden

U0=[1,1,0,0]; U1=[1,0,1,0] → coincidencia 1, voto P2. U2=[0,1,0,1] → coincidencia 1, voto P3. Resultado [0,0,1,1].


### Auditoría de las decisiones construidas

| Elemento / decisión | Responsabilidad y motivo | Evidencia de estado |
|---|---|---|
| Paso 1: Declarar los dos grupos de nodos y la matriz de relaciones | Para recomendar con relaciones necesitamos un registro de conexiones usuario-producto; no es una matriz cuadrada de usuarios únicamente. | U0 conoce P0 y P1. U1 comparte P0; U2 comparte P1. |
| Paso 2: Elegir destinatario y reservar puntuaciones | La recomendación debe estar dirigida a alguien concreto y llevar un contador separado por producto. | Estado inicial puntaje=[0,0,0,0]. Falta comparar al usuario con sus pares. |
| Paso 3: Comparar U0 con cada usuario distinto | Comparar U0 consigo mismo no aporta evidencia de preferencia externa y aumentaría artificialmente las puntuaciones. | Se examinarán U1 y U2 por separado; aún no sabemos cuánto comparten. |
| Paso 4: Contar productos que ambos usuarios comparten | Para que una relación de otro usuario influya en el objetivo, necesitamos evidencia de preferencias compartidas. | Conocemos el peso de similitud de cada otro usuario, pero aún falta identificar candidatos nuevos. |
| Paso 5: Otorgar votos solo a productos nuevos para el objetivo | Compartir un producto solo sirve si además ese usuario conoce algo que U0 aún no conoce. | Puntuaciones finales internas: P0=0, P1=0, P2=1, P3=1. |
| Paso 6: Presentar recomendaciones con respaldo real | Un puntaje cero no constituye una recomendación basada en la red de relaciones. | El programa muestra exactamente dos productos nuevos para U0, cada uno con puntaje uno. |


### Archivo completo al terminar EJ08

Este contenido es **la concatenación literal y ordenada** de los microbloques anteriores; úsalo como comprobación después de construir, no como sustituto de la explicación.

```java
public class EJ08_Recomendador {
    public static void main(String[] args) {
        int[][] gustos = {
            {1, 1, 0, 0},
            {1, 0, 1, 0},
            {0, 1, 0, 1}
        };
        int objetivo = 0;
        int[] puntaje = new int[gustos[0].length];
        for (int otro = 0; otro < gustos.length; otro++) {
            if (otro == objetivo) continue;
            int coincidencias = 0;
            for (int p = 0; p < gustos[otro].length; p++) {
                if (gustos[objetivo][p] == 1 && gustos[otro][p] == 1) {
                    coincidencias++;
                }
            }
            if (coincidencias > 0) {
                for (int p = 0; p < gustos[otro].length; p++) {
                    if (gustos[objetivo][p] == 0 && gustos[otro][p] == 1) {
                        puntaje[p] += coincidencias;
                    }
                }
            }
        }
        for (int p = 0; p < puntaje.length; p++) {
            if (puntaje[p] > 0) {
                System.out.println("recomendar P" + p + " puntaje=" + puntaje[p]);
            }
        }
    }
}
```

### Predice antes de ejecutar

¿Por qué P0 y P1 no deben recomendarse a U0 aunque tengan vínculos con otros usuarios?


### Compila, ejecuta, comprueba e interpreta

Abre una terminal en `S13_Grafos/EJ08` y ejecuta:

```bash
javac -encoding UTF-8 EJ08_Recomendador.java
java EJ08_Recomendador
```
**Salida de referencia:**

```text
recomendar P2 puntaje=1
recomendar P3 puntaje=1
```
**Interpretación:** U1 comparte P0 con U0 y aporta P2; U2 comparte P1 y aporta P3. El filtro impide repetir P0 o P1.

**Variación razonada:** Añade un vínculo U1-P3: P3 recibe dos votos y P2 uno; la recomendación más sustentada pasa a ser P3.

**Error o caso límite:** Omitir el filtro `gustos[objetivo][p] == 0` recomendaría productos que el usuario ya eligió.

**Corrección:** Verificar la condición de producto no conocido antes de sumar una recomendación.

**Explica con tus palabras:** ¿Cuál es la necesidad que resuelve el método o estructura principal? ¿Qué variable cambia en cada iteración o llamada? ¿Qué propiedad del grafo o del algoritmo justifica la salida? ¿Qué fallaría si omites la condición decisiva descrita en este ejemplo?


### T08 — tarea espejo de EJ08

**Enunciado:** Añade la relación U1-P3 sin cambiar los otros vínculos. Ejecuta de nuevo el recomendador para U0 y explica por qué P3 sube a puntuación 2.

**Archivo nuevo:** `S13_Grafos/T08/T08_Recomendador.java`. No modifiques el programa resuelto `EJ08_Recomendador.java`; crea una carpeta nueva `T08` al lado de `EJ08`.

**Pista:** Modifica únicamente el elemento de la segunda fila y cuarta columna de `gustos`; conserva las comprobaciones de coincidencia y producto no conocido.

**Restricción:** utiliza las mismas ideas técnicas y biblioteca estándar del ejemplo; no sustituyas el algoritmo por un resultado escrito a mano.

**Punto de partida de la tarea: archivo inicial reproducible.** Este archivo compila, aunque no completa todavía todos los objetivos. Localiza los comentarios `TODO` y aplica lo que aprendiste en el ejemplo.

```java
public class T08_Recomendador {
    public static void main(String[] args) {
        int[][] gustos = {{1,1,0,0},{1,0,1,0},{0,1,0,1}};
        // TODO T08: haz que U1 también conozca P3.
        int objetivo=0;
        int[] puntaje=new int[4];
        for(int otro=0;otro<gustos.length;otro++) {
            if(otro==objetivo) continue;
            int coincidencias=0;
            for(int p=0;p<4;p++) if(gustos[objetivo][p]==1 && gustos[otro][p]==1) coincidencias++;
            if(coincidencias>0) for(int p=0;p<4;p++) if(gustos[objetivo][p]==0 && gustos[otro][p]==1) puntaje[p]+=coincidencias;
        }
        for(int p=0;p<4;p++) if(puntaje[p]>0) System.out.println("recomendar P"+p+" puntaje="+puntaje[p]);
    }
}
```
**Antes de ejecutarlo, predice:** ¿qué salida obtiene el archivo inicial? ¿Qué cambiará al completar los TODO? ¿Qué propiedades del grafo permanecerán iguales?

**Evidencia:** entrega el archivo `T08_Recomendador.java`, la salida de la terminal y una explicación breve del cambio observado.

**Cómo validar la tarea:** recomendar P2 puntaje=1; recomendar P3 puntaje=2. La solución detallada se revisará con el docente; no se incluye en esta guía para preservar el ejercicio.

---

## 7. Cierre de la sesión — lectura final

Hoy convertimos la idea de «tener entidades relacionadas» en datos verificables. La matriz de adyacencia nos permitió responder en una sola celda si A y B están conectados, pero vimos que reserva posiciones incluso para relaciones ausentes. La lista de adyacencia nos permitió conservar solamente vecinos existentes; por eso su estructura es especialmente útil cuando el grafo es disperso. En ambos casos justificamos las asignaciones que crean una arista y comprobamos que una conexión no dirigida debe registrarse desde los dos extremos.

Después distinguimos el sentido de los arcos dirigidos y descubrimos por qué entrada y salida no son lo mismo. Con DFS analizamos cómo una llamada recursiva profundiza y regresa sin repetir nodos. Con BFS usamos una cola FIFO para descubrir primero los vecinos cercanos y convertir esa exploración en distancias mínimas por cantidad de aristas. También distinguimos un regreso legítimo al nodo padre de un ciclo real, decisión imprescindible cuando trabajamos con grafos no dirigidos.

En A* dejamos de mirar únicamente el siguiente nivel: combinamos el costo acumulado g con una estimación h, filtramos obstáculos y reconstruimos una ruta mediante predecesores. El recomendador nos mostró otra cara de los grafos: las aristas también pueden expresar relaciones entre usuarios y productos, no solo calles. Como siguiente práctica, elige una tarea espejo y explica, antes de ejecutar, qué condición hará que cambie la salida. Guarda tus archivos y registra las comprobaciones; un resultado es más valioso cuando puedes defender su causa.

Para continuar estudiando, Lideratec Academy mantiene recursos académicos en https://lideratecacademy.com/blog/ y en https://www.youtube.com/@LideratecAcademy. El laboratorio descargable de esta sesión puede utilizarse después, de manera opcional, para contrastar tus archivos construidos desde cero.
