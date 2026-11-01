# Guía del estudiante — Sesión 12: recursividad y ordenamiento en Java

**Lideratec Academy · Algoritmo y Estructura de Datos basado en IA**  
**Sesión S12 · Duración de aula: 120 minutos · Práctica adicional autónoma**

## Propósito de la sesión

Cuando un programa necesita examinar un vector o una lista enlazada, puede resolver una parte pequeña y luego volver a invocar el mismo procedimiento con el problema reducido. Esa es la idea central de la recursividad: cada llamada debe avanzar hacia una situación suficientemente simple para detenerse. En esta clase veremos por qué un arreglo puede avanzar mediante un índice y una lista enlazada necesita seguir referencias entre nodos. Entender esta diferencia evita accesos fuera del vector y lecturas de referencias nulas.

Comenzaremos con recorridos y búsquedas recursivas para identificar los casos base, el estado que cambia y las respuestas que regresan a la llamada original. Después construiremos Merge Sort sobre un vector: la división recursiva prepara rangos, pero solo la fusión consigue el orden. Usaremos el conjunto [8,3,5,2,9,1,4,7] para representar las particiones y ordenar datos, y luego adaptaremos la comparación a registros de estudiantes por promedio. Finalmente crearemos, dividiremos y ordenaremos una lista enlazada sin perder sus nodos. El objetivo es justificar cada decisión y comprobar el resultado, no copiar un archivo terminado.

## Resultado de aprendizaje

Al finalizar podrás escribir y explicar recorridos y búsquedas recursivas sobre vectores y listas; identificar cuándo un índice alcanza `length` y cuándo una referencia llega a `null`; implementar Merge Sort en Java sobre datos enteros y objetos; y explicar, con una traza, cómo la fusión produce el orden. Podrás demostrar tu comprensión presentando la salida del programa y justificando el retorno, el corte de enlaces, la elección de un valor en cada comparación y la conservación de todos los elementos.

**Criterio observable:** tus ocho programas compilan y producen las salidas previstas; además puedes reconstruir, en una hoja, la división de ocho valores y mostrar dónde se conserva la segunda mitad de una lista.

## Qué necesitas antes de empezar

Se asumen clases, métodos `static`, variables, `if`, `while`, `for`, arreglos, objetos y enlaces de listas, ya trabajados en las sesiones previas. Comprueba que Java y su compilador están instalados: abre una terminal y ejecuta `java -version` y `javac -version`. Las dos instrucciones deben mostrar sus versiones y no un mensaje de comando desconocido. Puedes utilizar cualquier IDE de Java o un editor con terminal; este laboratorio no utiliza dependencias ni librerías externas.

Crea una carpeta llamada `S12_Recursividad` en una ubicación que puedas encontrar y abre allí una terminal. Por cada ejemplo, crea **exactamente** el archivo `.java` indicado, escribe los microbloques **en el orden mostrado**, guarda el archivo en UTF-8 y, solo después de escribir todas sus partes, ejecuta `javac -encoding UTF-8 NombreArchivo.java` y luego `java NombreClase` (sin `.java`). Si un paso indica que una clase o método continúa abierto, el fragmento aún no compila y no debe ejecutarse todavía. Cuando corresponda una tarea espejo, utiliza el código completo del ejemplo como punto de partida, copia a un archivo nuevo con el nombre Txx indicado y cambia también el nombre de la clase pública. Todos los ejemplos se pueden construir desde esta guía: los proyectos descargables son opcionales.

**Errores de entorno habituales:** si `javac` no está disponible, verifica que instalaste un JDK y que su carpeta bin está en PATH; si aparece `class ... is public, should be declared in a file named ...`, iguala clase y archivo; si aparece `Could not find or load main class`, ejecuta `java` desde la carpeta donde se compiló y sin extensión.

## Ruta de 120 minutos y trabajo adicional

| Tramo | Minutos | Actividad en aula |
|---|---:|---|
| Activación y entorno | 10 | Casos base, `java` y `javac`, archivo base |
| E01 y E02 | 20 | Recorrido y búsqueda en vectores; predecir índices |
| E03 | 30 | División, fusión y traza de ocho valores |
| E04 | 15 | Objetos Estudiante y empate estable |
| E05 y E06 | 20 | Recorrido y búsqueda por referencias |
| E07 y E08 | 20 | Corte seguro y Merge Sort sobre nodos |
| Cierre y contraste | 5 | Verificar resultados y explicar diferencias |

**Distribución didáctica:** E01, E02 y E03 se demuestran detalladamente en aula; E04–E06 se resuelven mediante práctica acompañada; E07–E08 constituyen una integración final guiada cuya consolidación y tareas T01–T08 pueden continuarse autónomamente. Esta guía contiene toda la secuencia incluso si el tiempo de escritura en clase no permite terminar cada línea.


---

## EJ01 — Recorrer un vector por índices sin un bucle

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

Una sucesión de datos existe en un vector, pero todavía debemos decidir cómo visitar cada posición sin usar `for` ni `while`. Primero crearemos el método `recorrer`, le daremos el arreglo y una posición inicial, y añadiremos una condición que evite salir de sus límites. Después imprimiremos el elemento actual y repetiremos el método desde el índice siguiente. Al ejecutar podremos explicar por qué aparecen cinco valores y por qué la llamada final no imprime nada.

### ¿Qué problema concreto resolveremos?

Mostrar todos los elementos de {10,20,30,40,50} sin recorrer una celda inexistente.

**Conceptos que se pondrán a prueba:** Caso base, parámetro de índice y avance recursivo.

**Archivo que construiremos:** `E01RecorridoVector.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E01RecorridoVector`.


### Paso 1 — Crear el contenedor del programa

**Antes de escribir (lectura docente).** El recorrido necesita un punto de ejecución y un método reutilizable; una clase agrupa ambos y comparte el nombre del archivo.

**Dónde escribir.** En `E01RecorridoVector.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E01RecorridoVector {
```

**Qué hace cada parte y por qué.** `public class` declara la clase visible; `E01RecorridoVector` debe coincidir exactamente con E01RecorridoVector.java; `{` abre su cuerpo, que todavía no debemos cerrar.

**Cambio de estado que debes reconocer.** La clase está abierta; por ahora Java no puede compilarla.

**Pregunta para comprobar comprensión.** ¿Por qué el nombre de la clase coincide con el del archivo? La respuesta que debes justificar es: Porque la clase pública de Java debe tener el mismo nombre base que el archivo.


### Paso 2 — Definir la operación recursiva y su posición actual

**Antes de escribir (lectura docente).** El vector no cambia; la función sí necesita recordar qué posición debe examinar en cada llamada.

**Dónde escribir.** En `E01RecorridoVector.java`, inmediatamente después del bloque anterior que termina en `public class E01RecorridoVector {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void recorrer(int[] v, int indice) {
```

**Qué hace cada parte y por qué.** `static` permite invocar este método desde `main` sin crear objetos; `void` significa que imprime pero no devuelve un valor; `int[] v` recibe el vector; `int indice` representa la posición actual.

**Cambio de estado que debes reconocer.** Existe la firma recorrer(v, indice), aún sin cuerpo completo.

**Pregunta para comprobar comprensión.** ¿Qué cambia de llamada en llamada? La respuesta que debes justificar es: Únicamente indice; v sigue apuntando al mismo arreglo.


### Paso 3 — Detener la recursión antes del índice inválido

**Antes de escribir (lectura docente).** Sin un caso base, el índice avanzaría fuera del arreglo y las llamadas no terminarían correctamente.

**Dónde escribir.** En `E01RecorridoVector.java`, inmediatamente después del bloque anterior que termina en `static void recorrer(int[] v, int indice) {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (indice >= v.length) {
            return;
        }
```

**Qué hace cada parte y por qué.** `v.length` es el total de posiciones; `indice >= v.length` identifica el primer índice no válido; `return` termina esta llamada sin seguir leyendo. Esta condición debe ocurrir ANTES de `v[indice]`.

**Cambio de estado que debes reconocer.** Para un vector de 5 elementos, indice=5 termina sin acceder a v[5].

**Pregunta para comprobar comprensión.** ¿Qué pasaría si comprobamos el límite después de imprimir? La respuesta que debes justificar es: Habría un acceso inválido a v[5] antes de detenerse.


### Paso 4 — Mostrar la celda y pasar a la siguiente

**Antes de escribir (lectura docente).** Como el índice ya es seguro, podemos leer el dato actual; falta avanzar sin usar un bucle.

**Dónde escribir.** En `E01RecorridoVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        System.out.println("Elemento: " + v[indice]);
        recorrer(v, indice + 1);
    }
```

**Qué hace cada parte y por qué.** `println` presenta el contenido de `v[indice]`; `+` concatena el texto con su valor. La llamada `recorrer(v, indice + 1)` aumenta exactamente una posición; `}` cierra el método, no la clase.

**Cambio de estado que debes reconocer.** indice=0 imprime 10 y solicita indice=1; la sucesión llega a 5 y se detiene.

**Pregunta para comprobar comprensión.** ¿Dónde se encuentra el avance hacia el caso base? La respuesta que debes justificar es: En indice + 1 de cada llamada.


### Paso 5 — Construir la prueba y cerrar la clase

**Antes de escribir (lectura docente).** Necesitamos datos concretos y un primer índice para observar el algoritmo completo.

**Dónde escribir.** En `E01RecorridoVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        int[] miVector = {10, 20, 30, 40, 50};
        recorrer(miVector, 0);
    }
}
```

**Qué hace cada parte y por qué.** `main` es el punto de entrada; `miVector` crea cinco datos; `recorrer(miVector, 0)` comienza en el índice cero. Las dos últimas llaves cierran `main` y la clase, por lo que ya puede compilarse.

**Cambio de estado que debes reconocer.** El archivo final es ejecutable y visita cinco posiciones, sin acceder a la sexta.

**Pregunta para comprobar comprensión.** ¿Cuántas impresiones aparecen? La respuesta que debes justificar es: Cinco, porque los índices válidos son 0, 1, 2, 3 y 4.


### Traza manual antes de ejecutar

índice 0→10, 1→20, 2→30, 3→40, 4→50, 5→caso base.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Crear el contenedor del programa | La clase está abierta; por ahora Java no puede compilarla. |
| Paso 2 | Definir la operación recursiva y su posición actual | Existe la firma recorrer(v, indice), aún sin cuerpo completo. |
| Paso 3 | Detener la recursión antes del índice inválido | Para un vector de 5 elementos, indice=5 termina sin acceder a v[5]. |
| Paso 4 | Mostrar la celda y pasar a la siguiente | indice=0 imprime 10 y solicita indice=1; la sucesión llega a 5 y se detiene. |
| Paso 5 | Construir la prueba y cerrar la clase | El archivo final es ejecutable y visita cinco posiciones, sin acceder a la sexta. |

### Archivo completo al terminar EJ01

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E01RecorridoVector {
    static void recorrer(int[] v, int indice) {
        if (indice >= v.length) {
            return;
        }
        System.out.println("Elemento: " + v[indice]);
        recorrer(v, indice + 1);
    }
    public static void main(String[] args) {
        int[] miVector = {10, 20, 30, 40, 50};
        recorrer(miVector, 0);
    }
}
```

### Predicción y ejecución

**Predice:** ¿El primer valor será 10 o 30? Justifica con el segundo argumento de recorrer.

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E01RecorridoVector.java
java E01RecorridoVector
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Elemento: 10
Elemento: 20
Elemento: 30
Elemento: 40
Elemento: 50
```

**Cómo interpretarla:** Cada impresión corresponde al índice antes de pasar a indice+1; el caso base detiene la sexta llamada.

**Modificación de comprobación:** Prueba el vector vacío: no imprime nada, porque v.length vale 0 desde la primera llamada.

**Error o caso límite controlado:** Sustituir >= por > hace posible intentar acceder a v[v.length]; primero predice el error y NO lo mantengas en el archivo principal. **Corrección razonada:** Restituir >= y confirmar que el último índice válido es v.length-1. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T01 — Tarea espejo de EJ01

**Enunciado:** Comienza a recorrer desde el índice 2, no desde el 0; comprueba que solo se muestran los tres últimos valores, sin modificar el algoritmo.

**Archivo para construir:** `T01RecorridoParcial.java`. Copia el archivo final de EJ01 a ese nombre; cambia también la declaración pública a `public class T01RecorridoParcial`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** La función ya admite cualquier índice inicial válido: modifica únicamente el argumento desde `main`.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Tres líneas: Elemento: 30, Elemento: 40 y Elemento: 50.

**Validación:** compila con `javac -encoding UTF-8 T01RecorridoParcial.java` y ejecuta `java T01RecorridoParcial`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ02 — Encontrar un valor mediante búsqueda recursiva lineal

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

No basta con visitar elementos cuando el programa debe localizar una clave. Construiremos una función que examine una posición, devuelva su índice si coincide y, en caso contrario, continúe por la siguiente. Introduciremos dos finales explícitos: encontrar la clave o llegar al límite del arreglo. Luego ejecutaremos una prueba con valor presente y otra ausente, verificando el significado de `-1`.

### ¿Qué problema concreto resolveremos?

Determinar el índice de 8 y reconocer la ausencia de 99 en {10,5,20,8,30}.

**Conceptos que se pondrán a prueba:** Retorno de la recursión, doble caso base y centinela -1.

**Archivo que construiremos:** `E02BusquedaVector.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E02BusquedaVector`.


### Paso 1 — Crear la clase de búsqueda

**Antes de escribir (lectura docente).** La búsqueda necesita su propio punto de entrada y una función que pueda devolver una respuesta numérica.

**Dónde escribir.** En `E02BusquedaVector.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E02BusquedaVector {
```

**Qué hace cada parte y por qué.** Declaramos una clase pública con el mismo nombre de archivo. Mantendremos todo dentro para compilar con el JDK, sin bibliotecas externas.

**Cambio de estado que debes reconocer.** Clase abierta, a la espera de la función buscar.

**Pregunta para comprobar comprensión.** ¿Qué devuelve una búsqueda por posición? La respuesta que debes justificar es: El índice localizado o una señal para ausencia.


### Paso 2 — Recibir los datos y la clave

**Antes de escribir (lectura docente).** A diferencia del recorrido, ahora interesa una respuesta que indique el lugar encontrado o la ausencia.

**Dónde escribir.** En `E02BusquedaVector.java`, inmediatamente después del bloque anterior que termina en `public class E02BusquedaVector {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static int buscar(int[] vector, int clave, int indice) {
```

**Qué hace cada parte y por qué.** `int` antes de buscar indica el índice que devolverá; `vector` son los datos; `clave` el valor esperado; `indice` por dónde comenzar.

**Cambio de estado que debes reconocer.** Cada llamada dispone del vector, la clave y la posición a inspeccionar.

**Pregunta para comprobar comprensión.** ¿Por qué la función no devuelve boolean? La respuesta que debes justificar es: Porque necesitamos conocer el índice, no solo si existe.


### Paso 3 — Reconocer que se acabaron las posiciones

**Antes de escribir (lectura docente).** Si no queda ninguna posición, el programa debe responder sin consultar índices inexistentes.

**Dónde escribir.** En `E02BusquedaVector.java`, inmediatamente después del bloque anterior que termina en `static int buscar(int[] vector, int clave, int indice) {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (indice >= vector.length) {
            return -1;
        }
```

**Qué hace cada parte y por qué.** La condición comprueba el límite ANTES de la comparación; `-1` funciona como centinela porque un índice válido nunca es negativo.

**Cambio de estado que debes reconocer.** Con clave ausente se llega a indice=longitud y se retorna -1.

**Pregunta para comprobar comprensión.** ¿Es -1 un elemento del vector? La respuesta que debes justificar es: No: es el valor devuelto para representar índice inexistente.


### Paso 4 — Comparar el elemento actual

**Antes de escribir (lectura docente).** Si el dato actual coincide, ya no conviene continuar examinando elementos.

**Dónde escribir.** En `E02BusquedaVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (vector[indice] == clave) {
            return indice;
        }
```

**Qué hace cada parte y por qué.** `==` compara dos enteros; si son iguales, `return indice` entrega inmediatamente la posición de la primera coincidencia encontrada desde el índice de inicio.

**Cambio de estado que debes reconocer.** Encontrar un valor corta la cadena recursiva, sin recorrer el resto del vector.

**Pregunta para comprobar comprensión.** ¿Qué sucede con claves duplicadas? La respuesta que debes justificar es: Se devuelve el primer índice coincidente desde la posición inicial.


### Paso 5 — Explorar el siguiente índice y terminar el método

**Antes de escribir (lectura docente).** Cuando la posición actual no coincide, tenemos que delegar la búsqueda al siguiente subproblema.

**Dónde escribir.** En `E02BusquedaVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        return buscar(vector, clave, indice + 1);
    }
```

**Qué hace cada parte y por qué.** El `return` exterior transmite hacia `main` el resultado recibido de la llamada más profunda; `indice+1` garantiza progreso y la llave cierra el método.

**Cambio de estado que debes reconocer.** El resultado de cualquiera de los casos base vuelve por la cadena de llamadas.

**Pregunta para comprobar comprensión.** ¿Por qué no escribir solo buscar(...) sin return? La respuesta que debes justificar es: Porque se perdería el índice devuelto por la llamada recursiva.


### Paso 6 — Comprobar un acierto y una ausencia

**Antes de escribir (lectura docente).** Necesitamos validar ambos caminos de salida: clave encontrada y vector agotado.

**Dónde escribir.** En `E02BusquedaVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        int[] v = {10, 5, 20, 8, 30};
        System.out.println("Encontrado: " + buscar(v, 8, 0));
        System.out.println("Ausente: " + buscar(v, 99, 0));
    }
}
```

**Qué hace cada parte y por qué.** El arreglo contiene 8 en posición 3; la primera impresión debe mostrar 3. La clave 99 no aparece y el método retorna -1; se cierran main y clase.

**Cambio de estado que debes reconocer.** La búsqueda demuestra salida por coincidencia y por agotamiento.

**Pregunta para comprobar comprensión.** ¿Por qué Encontrado vale 3? La respuesta que debes justificar es: Porque se indexa desde cero: 10=0, 5=1, 20=2 y 8=3.


### Traza manual antes de ejecutar

Buscar 8: índice 0 no coincide → 1 no coincide → 2 no coincide → 3 coincide, retorno 3.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Crear la clase de búsqueda | Clase abierta, a la espera de la función buscar. |
| Paso 2 | Recibir los datos y la clave | Cada llamada dispone del vector, la clave y la posición a inspeccionar. |
| Paso 3 | Reconocer que se acabaron las posiciones | Con clave ausente se llega a indice=longitud y se retorna -1. |
| Paso 4 | Comparar el elemento actual | Encontrar un valor corta la cadena recursiva, sin recorrer el resto del vector. |
| Paso 5 | Explorar el siguiente índice y terminar el método | El resultado de cualquiera de los casos base vuelve por la cadena de llamadas. |
| Paso 6 | Comprobar un acierto y una ausencia | La búsqueda demuestra salida por coincidencia y por agotamiento. |

### Archivo completo al terminar EJ02

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E02BusquedaVector {
    static int buscar(int[] vector, int clave, int indice) {
        if (indice >= vector.length) {
            return -1;
        }
        if (vector[indice] == clave) {
            return indice;
        }
        return buscar(vector, clave, indice + 1);
    }
    public static void main(String[] args) {
        int[] v = {10, 5, 20, 8, 30};
        System.out.println("Encontrado: " + buscar(v, 8, 0));
        System.out.println("Ausente: " + buscar(v, 99, 0));
    }
}
```

### Predicción y ejecución

**Predice:** ¿Si hay dos ochos, cuál será devuelto al comenzar en posición 2?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E02BusquedaVector.java
java E02BusquedaVector
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Encontrado: 3
Ausente: -1
```

**Cómo interpretarla:** La coincidencia devuelve el índice y el caso de agotamiento devuelve -1; el return propaga ambos.

**Modificación de comprobación:** Busca 10 empezando desde 1; debe retornar -1 porque se omite el primer elemento.

**Error o caso límite controlado:** Cambiar >= por > puede provocar lectura fuera del vector cuando la clave no está. **Corrección razonada:** Volver a >=; el límite debe comprobarse antes de vector[indice]. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T02 — Tarea espejo de EJ02

**Enunciado:** Busca 8 dentro de {5,8,8,13} comenzando en la posición 2: debe devolver 2, no 1; comprueba además que buscar 99 retorna -1.

**Archivo para construir:** `T02BusquedaDesdeIndice.java`. Copia el archivo final de EJ02 a ese nombre; cambia también la declaración pública a `public class T02BusquedaDesdeIndice`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** El índice inicial forma parte de los parámetros; una coincidencia anterior a ese índice ya no se examina.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Encontrado: 2 y Ausente: -1.

**Validación:** compila con `javac -encoding UTF-8 T02BusquedaDesdeIndice.java` y ejecuta `java T02BusquedaDesdeIndice`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ03 — Ordenar un vector con Merge Sort y justificar cada fusión

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

El vector [8,3,5,2,9,1,4,7] no puede ordenarse solo separándolo. Vamos a construir Merge Sort en tres decisiones: encontrar un caso base para rangos mínimos, dividir cada rango en dos mitades y fusionar ambas mitades respetando el orden. Para fusionar utilizaremos copias temporales que evitan sobrescribir datos. Finalmente contrastaremos el arreglo inicial con el ordenado y construiremos una traza de sus divisiones.

### ¿Qué problema concreto resolveremos?

Transformar [8,3,5,2,9,1,4,7] en una secuencia ascendente mediante división y fusión.

**Conceptos que se pondrán a prueba:** Rangos inclusivos, mitad, copias temporales, comparación estable y complejidad O(n log n).

**Archivo que construiremos:** `E03MergeSortVector.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E03MergeSortVector`.


### Paso 1 — Abrir clase e incorporar la representación del vector

**Antes de escribir (lectura docente).** Al ordenar necesitamos comparar el vector original con el final; Arrays.toString ofrece una impresión clara.

**Dónde escribir.** En `E03MergeSortVector.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
import java.util.Arrays;

public class E03MergeSortVector {
```

**Qué hace cada parte y por qué.** `import java.util.Arrays` da acceso a `Arrays.toString`; la clase contiene el algoritmo y `main`; aún no se debe compilar.

**Cambio de estado que debes reconocer.** Archivo con import y clase abierta.

**Pregunta para comprobar comprensión.** ¿El import ordena el vector? La respuesta que debes justificar es: No. Solo ayudará a mostrarlo; el ordenamiento se construirá después.


### Paso 2 — Declarar la función que divide por rangos

**Antes de escribir (lectura docente).** El algoritmo debe dejar de dividir cuando el rango tiene cero o un elemento.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `public class E03MergeSortVector {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void ordenar(int[] a, int izquierda, int derecha) {
        if (izquierda >= derecha) return;
```

**Qué hace cada parte y por qué.** `a` es el mismo arreglo; `izquierda` y `derecha` delimitan un segmento inclusivo. Si izquierda>=derecha, el segmento ya está ordenado. `return` es el caso base.

**Cambio de estado que debes reconocer.** Rango [i,i] se detiene; un rango mayor sigue descomponiéndose.

**Pregunta para comprobar comprensión.** ¿Por qué no crear siempre un nuevo vector? La respuesta que debes justificar es: La función trabaja sobre rangos del mismo arreglo; solo la fusión usará temporales.


### Paso 3 — Partir en dos segmentos sin perder los bordes

**Antes de escribir (lectura docente).** Antes de fusionar deben quedar ordenadas ambas mitades.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `if (izquierda >= derecha) return;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        int medio = izquierda + (derecha - izquierda) / 2;
        ordenar(a, izquierda, medio);
        ordenar(a, medio + 1, derecha);
```

**Qué hace cada parte y por qué.** `medio` es la frontera, incluida en la mitad izquierda. La derecha comienza en medio+1 para evitar duplicar el elemento central; ambas llamadas reducen el rango.

**Cambio de estado que debes reconocer.** Para [0,7], las mitades son [0,3] y [4,7].

**Pregunta para comprobar comprensión.** ¿Cuándo se fusiona el rango original? La respuesta que debes justificar es: Después de que ambas llamadas recursivas terminen.


### Paso 4 — Ordenar la fusión y cerrar el primer método

**Antes de escribir (lectura docente).** Dividir sin combinar no cambia el orden global; falta fusionar las dos mitades ya ordenadas.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `ordenar(a, medio + 1, derecha);`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        fusionar(a, izquierda, medio, derecha);
    }
```

**Qué hace cada parte y por qué.** `fusionar` recibirá límites que permiten reconstruir el segmento completo. Al retornar, ordenar tendrá resuelto el rango original.

**Cambio de estado que debes reconocer.** El método ordenar ya expresa dividir → resolver mitades → fusionar; fusionar aún no existe.

**Pregunta para comprobar comprensión.** ¿Qué fallaría si eliminamos fusionar? La respuesta que debes justificar es: El vector seguiría desordenado, aunque se hicieran llamadas recursivas.


### Paso 5 — Crear el método de fusión y calcular tamaños

**Antes de escribir (lectura docente).** La fusión necesita conocer cuántos elementos tiene cada mitad para copiarlos sin mezclar rangos.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void fusionar(int[] a, int izquierda, int medio, int derecha) {
        int n1 = medio - izquierda + 1;
        int n2 = derecha - medio;
```

**Qué hace cada parte y por qué.** n1 incluye la posición medio; n2 comienza en medio+1 y llega hasta derecha. Los tamaños son exactos para segmentos inclusivos.

**Cambio de estado que debes reconocer.** Para [0,7], medio 3, n1=4 y n2=4.

**Pregunta para comprobar comprensión.** ¿Por qué hay +1 solo en n1? La respuesta que debes justificar es: Porque el lado izquierdo incluye ambos extremos, de izquierda a medio.


### Paso 6 — Separar datos temporales de izquierda y derecha

**Antes de escribir (lectura docente).** Si escribiéramos sobre el arreglo original mientras comparamos ambas mitades, podríamos sobrescribir datos pendientes.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `int n2 = derecha - medio;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        int[] L = new int[n1];
        int[] R = new int[n2];
        for (int i = 0; i < n1; i++) L[i] = a[izquierda + i];
        for (int j = 0; j < n2; j++) R[j] = a[medio + 1 + j];
```

**Qué hace cada parte y por qué.** L y R son copias temporales. El primer `for` parte de izquierda; el segundo parte de medio+1. Las variables i y j son índices locales, distintos de los bordes globales.

**Cambio de estado que debes reconocer.** Las mitades originales quedan preservadas en dos arreglos para su mezcla.

**Pregunta para comprobar comprensión.** ¿Qué índice inicia la copia del lado derecho? La respuesta que debes justificar es: medio+1.


### Paso 7 — Comparar ambas cabeceras y escribir la menor

**Antes de escribir (lectura docente).** Las dos mitades ya están ordenadas, por lo que basta comparar sus elementos actuales en vez de buscar el mínimo global.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `for (int j = 0; j < n2; j++) R[j] = a[medio + 1 + j];`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        int i = 0, j = 0, k = izquierda;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) a[k++] = L[i++];
            else a[k++] = R[j++];
        }
```

**Qué hace cada parte y por qué.** i y j marcan la siguiente posición pendiente en L y R; k localiza la posición de escritura en a. `&&` exige que ambas tengan datos; <= elige la izquierda ante empate, preservando estabilidad; los ++ avanzan índice de origen y destino.

**Cambio de estado que debes reconocer.** Después del ciclo, alguna de las mitades habrá terminado.

**Pregunta para comprobar comprensión.** Si L[i]=R[j], ¿cuál se toma? La respuesta que debes justificar es: La izquierda, porque usamos <=; eso preserva el orden relativo de iguales.


### Paso 8 — Copiar el resto sin descartar valores

**Antes de escribir (lectura docente).** Una mitad puede agotarse antes que la otra; el contenido restante ya está ordenado y debe copiarse completo.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        while (i < n1) a[k++] = L[i++];
        while (j < n2) a[k++] = R[j++];
    }
```

**Qué hace cada parte y por qué.** Cada `while` copia hasta el final de su mitad. Solo uno puede realizar copias después del ciclo principal, salvo que ambos hayan terminado a la vez.

**Cambio de estado que debes reconocer.** Se completa la reconstrucción ordenada del rango [izquierda,derecha].

**Pregunta para comprobar comprensión.** ¿Por qué no se compara en estos ciclos? La respuesta que debes justificar es: Porque solo quedan valores de una mitad que ya estaba ordenada.


### Paso 9 — Ejecutar la simulación solicitada con ocho datos

**Antes de escribir (lectura docente).** El vector de ocho valores permite dibujar un árbol de divisiones equilibrado y verificar las fusiones.

**Dónde escribir.** En `E03MergeSortVector.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        int[] datos = {8, 3, 5, 2, 9, 1, 4, 7};
        System.out.println("Antes: " + Arrays.toString(datos));
        ordenar(datos, 0, datos.length - 1);
        System.out.println("Despues: " + Arrays.toString(datos));
    }
}
```

**Qué hace cada parte y por qué.** La llamada usa rango [0,7]. Arrays.toString presenta el estado antes y después; `ordenar` modifica los elementos del arreglo original, no la variable datos.

**Cambio de estado que debes reconocer.** El vector termina ascendente y conserva los ocho elementos originales.

**Pregunta para comprobar comprensión.** ¿Cuántos elementos debe haber después? La respuesta que debes justificar es: Ocho: ordenar modifica posiciones, no elimina datos.


### Traza manual antes de ejecutar

[8,3,5,2] | [9,1,4,7] → [8,3]|[5,2]|[9,1]|[4,7] → [3,8]|[2,5]|[1,9]|[4,7] → [2,3,5,8]|[1,4,7,9] → [1,2,3,4,5,7,8,9].

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Abrir clase e incorporar la representación del vector | Archivo con import y clase abierta. |
| Paso 2 | Declarar la función que divide por rangos | Rango [i,i] se detiene; un rango mayor sigue descomponiéndose. |
| Paso 3 | Partir en dos segmentos sin perder los bordes | Para [0,7], las mitades son [0,3] y [4,7]. |
| Paso 4 | Ordenar la fusión y cerrar el primer método | El método ordenar ya expresa dividir → resolver mitades → fusionar; fusionar aún no existe. |
| Paso 5 | Crear el método de fusión y calcular tamaños | Para [0,7], medio 3, n1=4 y n2=4. |
| Paso 6 | Separar datos temporales de izquierda y derecha | Las mitades originales quedan preservadas en dos arreglos para su mezcla. |
| Paso 7 | Comparar ambas cabeceras y escribir la menor | Después del ciclo, alguna de las mitades habrá terminado. |
| Paso 8 | Copiar el resto sin descartar valores | Se completa la reconstrucción ordenada del rango [izquierda,derecha]. |
| Paso 9 | Ejecutar la simulación solicitada con ocho datos | El vector termina ascendente y conserva los ocho elementos originales. |

### Archivo completo al terminar EJ03

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
import java.util.Arrays;

public class E03MergeSortVector {
    static void ordenar(int[] a, int izquierda, int derecha) {
        if (izquierda >= derecha) return;
        int medio = izquierda + (derecha - izquierda) / 2;
        ordenar(a, izquierda, medio);
        ordenar(a, medio + 1, derecha);
        fusionar(a, izquierda, medio, derecha);
    }
    static void fusionar(int[] a, int izquierda, int medio, int derecha) {
        int n1 = medio - izquierda + 1;
        int n2 = derecha - medio;
        int[] L = new int[n1];
        int[] R = new int[n2];
        for (int i = 0; i < n1; i++) L[i] = a[izquierda + i];
        for (int j = 0; j < n2; j++) R[j] = a[medio + 1 + j];
        int i = 0, j = 0, k = izquierda;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) a[k++] = L[i++];
            else a[k++] = R[j++];
        }
        while (i < n1) a[k++] = L[i++];
        while (j < n2) a[k++] = R[j++];
    }
    public static void main(String[] args) {
        int[] datos = {8, 3, 5, 2, 9, 1, 4, 7};
        System.out.println("Antes: " + Arrays.toString(datos));
        ordenar(datos, 0, datos.length - 1);
        System.out.println("Despues: " + Arrays.toString(datos));
    }
}
```

### Predicción y ejecución

**Predice:** ¿Cuándo empezará a verse un orden parcial: al dividir o al fusionar?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E03MergeSortVector.java
java E03MergeSortVector
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Antes: [8, 3, 5, 2, 9, 1, 4, 7]
Despues: [1, 2, 3, 4, 5, 7, 8, 9]
```

**Cómo interpretarla:** El caso base limita la profundidad y la fusión repone los valores en posiciones ordenadas; <= conserva estabilidad.

**Modificación de comprobación:** Prueba un vector de un elemento; no realiza fusiones y mantiene el valor.

**Error o caso límite controlado:** Si se elimina el segundo while de restos, se podrían perder valores de la mitad derecha. **Corrección razonada:** Restituye ambos ciclos de restos; comprueba que el tamaño y el multiconjunto de datos se conservan. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T03 — Tarea espejo de EJ03

**Enunciado:** Adapta Merge Sort para ordenar de mayor a menor {7,4,1,9,3,8}, modificando el criterio de comparación y verificando el orden final.

**Archivo para construir:** `T03MergeDescendente.java`. Copia el archivo final de EJ03 a ese nombre; cambia también la declaración pública a `public class T03MergeDescendente`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** La división no necesita cambiar: revisa exclusivamente cómo `fusionar` elige el siguiente valor.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Despues: [9, 8, 7, 4, 3, 1].

**Validación:** compila con `javac -encoding UTF-8 T03MergeDescendente.java` y ejecuta `java T03MergeDescendente`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ04 — Ordenar registros de estudiantes por promedio sin perder sus datos

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

En un registro académico los datos forman una unidad: no podemos mover promedios y dejar nombres en sus posiciones originales. Construiremos la clase `Estudiante` con nombre y promedio y adaptaremos Merge Sort para ordenar referencias a objetos completos. La división recursiva seguirá siendo la de EJ03, pero la comparación ocurrirá sobre `promedio`. Introduciremos valores empatados para comprobar que el algoritmo mantiene su orden relativo.

### ¿Qué problema concreto resolveremos?

Ordenar cuatro estudiantes por promedio, preservando el vínculo nombre-promedio y el orden entre empates.

**Conceptos que se pondrán a prueba:** Objetos, atributo de comparación y estabilidad; no se introduce Comparator de Java.

**Archivo que construiremos:** `E04MergeSortEstudiantes.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E04MergeSortEstudiantes`.


### Paso 1 — Definir la clase que agrupa algoritmo y registros

**Antes de escribir (lectura docente).** No ordenaremos enteros aislados: cada registro conserva nombre y promedio, que deben viajar juntos durante la mezcla.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E04MergeSortEstudiantes {
    static class Estudiante {
```

**Qué hace cada parte y por qué.** La clase externa contiene el programa; `Estudiante` es un tipo interno que representa una unidad de datos. La pertenencia a una misma clase evita archivos adicionales.

**Cambio de estado que debes reconocer.** Se ha abierto el tipo de registro dentro del ejemplo.

**Pregunta para comprobar comprensión.** ¿Por qué no usar solo double[]? La respuesta que debes justificar es: Porque perderíamos la asociación explícita entre el nombre del estudiante y su promedio.


### Paso 2 — Dar estado al registro y construirlo

**Antes de escribir (lectura docente).** Cada elemento necesita preservar los dos campos cuando su posición cambie.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `static class Estudiante {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        String nombre;
        double promedio;
        Estudiante(String nombre, double promedio) {
            this.nombre = nombre;
            this.promedio = promedio;
        }
    }
```

**Qué hace cada parte y por qué.** String representa nombre; double representa promedio; el constructor recibe ambos. `this.nombre` y `this.promedio` diferencian los campos del objeto de los parámetros; las llaves cierran constructor y tipo.

**Cambio de estado que debes reconocer.** Podemos crear registros íntegros con new Estudiante(...).

**Pregunta para comprobar comprensión.** ¿Qué se mueve durante el Merge Sort? La respuesta que debes justificar es: La referencia al registro completo; nombre y promedio permanecen juntos.


### Paso 3 — Dividir el vector de objetos recursivamente

**Antes de escribir (lectura docente).** La estrategia de división es la misma de EJ03; cambia el tipo de elemento y más adelante el criterio de orden.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void ordenar(Estudiante[] a, int izq, int der) {
        if (izq >= der) return;
        int m = izq + (der - izq) / 2;
        ordenar(a, izq, m);
        ordenar(a, m + 1, der);
        fusionar(a, izq, m, der);
    }
```

**Qué hace cada parte y por qué.** `Estudiante[]` conserva objetos completos. `izq>=der` detiene la recursión, `m` divide rangos inclusivos y fusionar reordena objetos tras ordenar sus mitades.

**Cambio de estado que debes reconocer.** El algoritmo ya describe el patrón recursivo aplicado a registros.

**Pregunta para comprobar comprensión.** ¿Hay que ordenar nombre y promedio por separado? La respuesta que debes justificar es: No: se traslada cada objeto completo, comparando solo su promedio.


### Paso 4 — Crear arreglos temporales de referencias

**Antes de escribir (lectura docente).** Necesitamos preservar las dos mitades sin descomponer cada registro en campos independientes.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void fusionar(Estudiante[] a, int izq, int m, int der) {
        Estudiante[] l = new Estudiante[m - izq + 1];
        Estudiante[] r = new Estudiante[der - m];
        for (int i = 0; i < l.length; i++) l[i] = a[izq + i];
        for (int j = 0; j < r.length; j++) r[j] = a[m + 1 + j];
```

**Qué hace cada parte y por qué.** Los arreglos l y r guardan referencias a Estudiante. Cada `for` copia posiciones dentro del rango; los objetos originales no se duplican, solo las referencias.

**Cambio de estado que debes reconocer.** Las mitades están disponibles para compararse por promedio.

**Pregunta para comprobar comprensión.** ¿Se crean estudiantes nuevos durante fusionar? La respuesta que debes justificar es: No. Se copian referencias a los objetos existentes.


### Paso 5 — Comparar promedios y preservar empates

**Antes de escribir (lectura docente).** Para ordenar registros debemos elegir una característica: aquí usaremos el promedio, no el nombre.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `for (int j = 0; j < r.length; j++) r[j] = a[m + 1 + j];`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        int i = 0, j = 0, k = izq;
        while (i < l.length && j < r.length) {
            if (l[i].promedio <= r[j].promedio) a[k++] = l[i++];
            else a[k++] = r[j++];
        }
```

**Qué hace cada parte y por qué.** La condición accede a `.promedio` de cada registro. Ante empate, `<=` prefiere el registro de la mitad izquierda y conserva el orden relativo, propiedad de estabilidad de Merge Sort.

**Cambio de estado que debes reconocer.** Los objetos se colocan en el vector según promedio ascendente.

**Pregunta para comprobar comprensión.** ¿Qué sucede con dos estudiantes de promedio 16? La respuesta que debes justificar es: Se conserva su orden original al usar <= en la fusión.


### Paso 6 — Copiar los objetos pendientes

**Antes de escribir (lectura docente).** Al menos una mitad puede contener registros pendientes tras las comparaciones entre ambas.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        while (i < l.length) a[k++] = l[i++];
        while (j < r.length) a[k++] = r[j++];
    }
```

**Qué hace cada parte y por qué.** Los dos ciclos copian lo que falta, conservando el objeto completo y cerrando fusionar.

**Cambio de estado que debes reconocer.** No se pierde ningún estudiante durante la mezcla.

**Pregunta para comprobar comprensión.** ¿Puede faltar un estudiante si omitimos un resto? La respuesta que debes justificar es: Sí; quedarían posiciones antiguas o incompletas dentro del segmento.


### Paso 7 — Preparar estudiantes, ordenar y mostrar ambos campos

**Antes de escribir (lectura docente).** Con un empate visible podremos observar el orden ascendente y verificar estabilidad.

**Dónde escribir.** En `E04MergeSortEstudiantes.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        Estudiante[] grupo = {
            new Estudiante("Ana", 16.0), new Estudiante("Luis", 12.5),
            new Estudiante("Eva", 16.0), new Estudiante("Rosa", 14.0)
        };
        ordenar(grupo, 0, grupo.length - 1);
        for (Estudiante e : grupo) System.out.println(e.nombre + "=" + e.promedio);
    }
}
```

**Qué hace cada parte y por qué.** Se crean cuatro objetos, se ordenan por promedio y el `for-each` imprime cada registro. Luis debe ir primero y Ana antes de Eva por tener igual promedio.

**Cambio de estado que debes reconocer.** Los campos nombre y promedio se mantienen asociados después del ordenamiento.

**Pregunta para comprobar comprensión.** ¿Qué evidencia muestra estabilidad? La respuesta que debes justificar es: Ana aparece antes de Eva, igual que en la entrada, porque ambas tienen 16.


### Traza manual antes de ejecutar

Mitades ordenadas por promedio: [Luis12.5,Ana16.0] y [Rosa14.0,Eva16.0] → Luis, Rosa, Ana, Eva.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Definir la clase que agrupa algoritmo y registros | Se ha abierto el tipo de registro dentro del ejemplo. |
| Paso 2 | Dar estado al registro y construirlo | Podemos crear registros íntegros con new Estudiante(...). |
| Paso 3 | Dividir el vector de objetos recursivamente | El algoritmo ya describe el patrón recursivo aplicado a registros. |
| Paso 4 | Crear arreglos temporales de referencias | Las mitades están disponibles para compararse por promedio. |
| Paso 5 | Comparar promedios y preservar empates | Los objetos se colocan en el vector según promedio ascendente. |
| Paso 6 | Copiar los objetos pendientes | No se pierde ningún estudiante durante la mezcla. |
| Paso 7 | Preparar estudiantes, ordenar y mostrar ambos campos | Los campos nombre y promedio se mantienen asociados después del ordenamiento. |

### Archivo completo al terminar EJ04

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E04MergeSortEstudiantes {
    static class Estudiante {
        String nombre;
        double promedio;
        Estudiante(String nombre, double promedio) {
            this.nombre = nombre;
            this.promedio = promedio;
        }
    }
    static void ordenar(Estudiante[] a, int izq, int der) {
        if (izq >= der) return;
        int m = izq + (der - izq) / 2;
        ordenar(a, izq, m);
        ordenar(a, m + 1, der);
        fusionar(a, izq, m, der);
    }
    static void fusionar(Estudiante[] a, int izq, int m, int der) {
        Estudiante[] l = new Estudiante[m - izq + 1];
        Estudiante[] r = new Estudiante[der - m];
        for (int i = 0; i < l.length; i++) l[i] = a[izq + i];
        for (int j = 0; j < r.length; j++) r[j] = a[m + 1 + j];
        int i = 0, j = 0, k = izq;
        while (i < l.length && j < r.length) {
            if (l[i].promedio <= r[j].promedio) a[k++] = l[i++];
            else a[k++] = r[j++];
        }
        while (i < l.length) a[k++] = l[i++];
        while (j < r.length) a[k++] = r[j++];
    }
    public static void main(String[] args) {
        Estudiante[] grupo = {
            new Estudiante("Ana", 16.0), new Estudiante("Luis", 12.5),
            new Estudiante("Eva", 16.0), new Estudiante("Rosa", 14.0)
        };
        ordenar(grupo, 0, grupo.length - 1);
        for (Estudiante e : grupo) System.out.println(e.nombre + "=" + e.promedio);
    }
}
```

### Predicción y ejecución

**Predice:** ¿Mover un promedio obliga a mover también su nombre?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E04MergeSortEstudiantes.java
java E04MergeSortEstudiantes
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Luis=12.5
Rosa=14.0
Ana=16.0
Eva=16.0
```

**Cómo interpretarla:** Cada referencia Estudiante se mueve íntegra; se ordena por promedio, no por nombre.

**Modificación de comprobación:** Añade un estudiante con promedio 12.5; verifica que el nuevo empate conserve su orden de entrada.

**Error o caso límite controlado:** Usar < en vez de <= en los empates puede cambiar la posición relativa de estudiantes iguales. **Corrección razonada:** Utilizar <= para ascendente y >= para descendente si deseamos estabilidad. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T04 — Tarea espejo de EJ04

**Enunciado:** Ordena el mismo grupo de mayor a menor promedio; Ana debe continuar antes que Eva, porque ambos empatan en 16.0.

**Archivo para construir:** `T04EstudiantesDescendente.java`. Copia el archivo final de EJ04 a ese nombre; cambia también la declaración pública a `public class T04EstudiantesDescendente`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** Cambia la comparación en fusionar a >=; elige siempre la mitad izquierda en los empates.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Ana=16.0, Eva=16.0, Rosa=14.0, Luis=12.5 (una línea por registro).

**Validación:** compila con `javac -encoding UTF-8 T04EstudiantesDescendente.java` y ejecuta `java T04EstudiantesDescendente`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ05 — Recorrer recursivamente una lista siguiendo referencias

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

En un arreglo elegíamos la siguiente posición mediante un índice. Una lista enlazada exige otra decisión: solo podemos avanzar utilizando la referencia `siguiente` del nodo actual. Crearemos una clase Nodo con dato y enlace, conectaremos tres objetos y después construiremos el método recursivo. El caso base será `actual == null` y comprobaremos que el orden de impresión coincide con el recorrido de referencias.

### ¿Qué problema concreto resolveremos?

Mostrar 1→2→3→null desde el nodo inicial sin usar índices.

**Conceptos que se pondrán a prueba:** Nodo, cabeza, enlace siguiente, caso base null y recorrido.

**Archivo que construiremos:** `E05RecorridoLista.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E05RecorridoLista`.


### Paso 1 — Crear el nodo que conserva dato y referencia

**Antes de escribir (lectura docente).** Una lista enlazada no guarda posiciones contiguas: cada elemento debe indicar quién lo sigue.

**Dónde escribir.** En `E05RecorridoLista.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E05RecorridoLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
```

**Qué hace cada parte y por qué.** `Nodo` es un tipo interno; `dato` almacena el entero; `siguiente` es una referencia a otro Nodo o null cuando no existe sucesor.

**Cambio de estado que debes reconocer.** Se reconoce la estructura de un eslabón, aún falta inicializarlo.

**Pregunta para comprobar comprensión.** ¿Qué representa siguiente? La respuesta que debes justificar es: La dirección lógica del siguiente nodo, no un índice del arreglo.


### Paso 2 — Inicializar un nodo independiente

**Antes de escribir (lectura docente).** Al crear un nodo todavía no sabemos a cuál estará enlazado.

**Dónde escribir.** En `E05RecorridoLista.java`, inmediatamente después del bloque anterior que termina en `Nodo siguiente;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
```

**Qué hace cada parte y por qué.** `this.dato` conserva el parámetro de entrada, y `siguiente=null` deja la referencia sin conexión hasta que asignemos explícitamente un sucesor.

**Cambio de estado que debes reconocer.** Cada nuevo Nodo nace sin siguiente; la cadena se construirá aparte.

**Pregunta para comprobar comprensión.** ¿Un nodo nuevo apunta automáticamente a otro? La respuesta que debes justificar es: No, su referencia siguiente comienza en null.


### Paso 3 — Definir el recorrido con su nodo actual

**Antes de escribir (lectura docente).** La recursión debe detenerse cuando no quede nodo; el criterio ya no depende de length.

**Dónde escribir.** En `E05RecorridoLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void recorrer(Nodo actual) {
        if (actual == null) return;
```

**Qué hace cada parte y por qué.** Cada llamada recibe `actual`, que puede ser un nodo o null. El caso base se comprueba antes de leer actual.dato para evitar un acceso sobre null.

**Cambio de estado que debes reconocer.** Cuando el último nodo apunta a null, la llamada siguiente termina.

**Pregunta para comprobar comprensión.** ¿Qué reemplaza al índice de un vector? La respuesta que debes justificar es: La referencia Nodo actual.


### Paso 4 — Mostrar el dato y avanzar por el enlace

**Antes de escribir (lectura docente).** Tras revisar un nodo debemos visitar exactamente el sucesor indicado por su referencia.

**Dónde escribir.** En `E05RecorridoLista.java`, inmediatamente después del bloque anterior que termina en `if (actual == null) return;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        System.out.print(actual.dato + " -> ");
        recorrer(actual.siguiente);
    }
```

**Qué hace cada parte y por qué.** Se imprime el valor de actual; `actual.siguiente` no calcula una dirección aritmética: toma el enlace existente. La llamada continúa hasta null.

**Cambio de estado que debes reconocer.** La salida conserva el orden de los enlaces construidos.

**Pregunta para comprobar comprensión.** ¿Qué pasa si se rompe un enlace? La respuesta que debes justificar es: La recursión deja de alcanzar los nodos que ya no están conectados.


### Paso 5 — Construir tres nodos y enlazarlos

**Antes de escribir (lectura docente).** Para demostrar el recorrido hay que construir una lista concreta que comience en cabeza.

**Dónde escribir.** En `E05RecorridoLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(1);
        cabeza.siguiente = new Nodo(2);
        cabeza.siguiente.siguiente = new Nodo(3);
```

**Qué hace cada parte y por qué.** Cabeza apunta al primer Nodo. Cada asignación a siguiente conecta el eslabón previo con un nuevo nodo; se crean los datos 1, 2 y 3 en ese orden.

**Cambio de estado que debes reconocer.** Existe cabeza→1→2→3→null; no hay índices numéricos.

**Pregunta para comprobar comprensión.** ¿Qué ocurriría si escribimos cabeza = new Nodo(3)? La respuesta que debes justificar es: Perderíamos desde cabeza el acceso a la cadena anterior.


### Paso 6 — Recorrer desde cabeza y señalar el final

**Antes de escribir (lectura docente).** Ahora podemos seguir los enlaces y comprobar dónde finaliza la cadena.

**Dónde escribir.** En `E05RecorridoLista.java`, inmediatamente después del bloque anterior que termina en `cabeza.siguiente.siguiente = new Nodo(3);`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
        recorrer(cabeza);
        System.out.println("null");
    }
}
```

**Qué hace cada parte y por qué.** La primera llamada recibe cabeza. `null` se imprime al final para representar gráficamente el extremo, no es un nodo recorrido.

**Cambio de estado que debes reconocer.** La consola debe mostrar 1 -> 2 -> 3 -> null.

**Pregunta para comprobar comprensión.** ¿El texto null se lee desde un objeto? La respuesta que debes justificar es: No: lo imprime main tras terminar el recorrido.


### Traza manual antes de ejecutar

actual=1 → actual=2 → actual=3 → actual=null (detener).

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Crear el nodo que conserva dato y referencia | Se reconoce la estructura de un eslabón, aún falta inicializarlo. |
| Paso 2 | Inicializar un nodo independiente | Cada nuevo Nodo nace sin siguiente; la cadena se construirá aparte. |
| Paso 3 | Definir el recorrido con su nodo actual | Cuando el último nodo apunta a null, la llamada siguiente termina. |
| Paso 4 | Mostrar el dato y avanzar por el enlace | La salida conserva el orden de los enlaces construidos. |
| Paso 5 | Construir tres nodos y enlazarlos | Existe cabeza→1→2→3→null; no hay índices numéricos. |
| Paso 6 | Recorrer desde cabeza y señalar el final | La consola debe mostrar 1 -> 2 -> 3 -> null. |

### Archivo completo al terminar EJ05

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E05RecorridoLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
    static void recorrer(Nodo actual) {
        if (actual == null) return;
        System.out.print(actual.dato + " -> ");
        recorrer(actual.siguiente);
    }
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(1);
        cabeza.siguiente = new Nodo(2);
        cabeza.siguiente.siguiente = new Nodo(3);
        recorrer(cabeza);
        System.out.println("null");
    }
}
```

### Predicción y ejecución

**Predice:** ¿La recursión puede retroceder desde el segundo nodo al primero?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E05RecorridoLista.java
java E05RecorridoLista
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
1 -> 2 -> 3 -> null
```

**Cómo interpretarla:** Cada llamada cambia la referencia actual al sucesor y finaliza con null.

**Modificación de comprobación:** Invoca recorrer(null): no imprime datos y main aún puede escribir null.

**Error o caso límite controlado:** Si se usa actual.dato antes de comprobar actual==null se produce NullPointerException. **Corrección razonada:** Colocar el caso base antes de cualquier acceso a actual.dato o actual.siguiente. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T05 — Tarea espejo de EJ05

**Enunciado:** Inicia el recorrido desde el segundo nodo, no desde cabeza; comprueba que 1 ya no es alcanzado por esa llamada.

**Archivo para construir:** `T05RecorridoDesdeSegundo.java`. Copia el archivo final de EJ05 a ese nombre; cambia también la declaración pública a `public class T05RecorridoDesdeSegundo`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** Pasa `cabeza.siguiente` al método en main, sin romper ningún enlace.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** 2 -> 3 -> null.

**Validación:** compila con `javac -encoding UTF-8 T05RecorridoDesdeSegundo.java` y ejecuta `java T05RecorridoDesdeSegundo`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ06 — Buscar un dato recorriendo nodos de forma recursiva

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

Una lista enlazada puede contener el dato que necesitamos, pero no permite saltar a su posición. Construiremos una búsqueda que comienza en una referencia, devuelve verdadero si el nodo actual coincide y devuelve falso si la referencia es nula. Si todavía hay nodos, el método vuelve a llamarse con `siguiente`. Ejecutaremos un caso de éxito y uno de ausencia para justificar ambos retornos.

### ¿Qué problema concreto resolveremos?

Saber si 20 y 50 existen en la lista 10→20→30→null.

**Conceptos que se pondrán a prueba:** Búsqueda por referencia, dos casos base y propagación de boolean.

**Archivo que construiremos:** `E06BusquedaLista.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E06BusquedaLista`.


### Paso 1 — Modelar un nodo con valor y siguiente

**Antes de escribir (lectura docente).** Para buscar en lista necesitamos partir de referencias que enlazan nodos, no de índices.

**Dónde escribir.** En `E06BusquedaLista.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E06BusquedaLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
```

**Qué hace cada parte y por qué.** Nodo contiene dato y siguiente. El constructor asigna el dato; como referencia de objeto, siguiente empieza en null si no se asigna un enlace. Cerrar el tipo interno no cierra la clase principal.

**Cambio de estado que debes reconocer.** Podemos construir una cadena de nodos sin librerías adicionales.

**Pregunta para comprobar comprensión.** ¿Por qué no hacemos vector[indice]? La respuesta que debes justificar es: Porque una lista enlazada simple no tiene acceso directo por índice.


### Paso 2 — Declarar una búsqueda que responde verdadero o falso

**Antes de escribir (lectura docente).** Aquí interesa saber si el valor existe, no su posición numérica.

**Dónde escribir.** En `E06BusquedaLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static boolean buscar(Nodo actual, int objetivo) {
```

**Qué hace cada parte y por qué.** `boolean` expresa éxito o fracaso; actual es el nodo que se examina; objetivo es el valor buscado, que permanece constante en las llamadas.

**Cambio de estado que debes reconocer.** La función debe decidir entre fin, coincidencia y avance.

**Pregunta para comprobar comprensión.** ¿Qué varía en cada llamada? La respuesta que debes justificar es: La referencia actual; objetivo permanece igual.


### Paso 3 — Reconocer el fin antes de consultar datos

**Antes de escribir (lectura docente).** Una lista puede estar vacía o llegar al final sin encontrar la clave.

**Dónde escribir.** En `E06BusquedaLista.java`, inmediatamente después del bloque anterior que termina en `static boolean buscar(Nodo actual, int objetivo) {`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (actual == null) return false;
```

**Qué hace cada parte y por qué.** Si actual vale null no hay dato para comparar. Devolver false evita dereferenciar una referencia inexistente.

**Cambio de estado que debes reconocer.** Buscar en lista vacía resulta false sin errores.

**Pregunta para comprobar comprensión.** ¿Qué devuelve una lista vacía al buscar 20? La respuesta que debes justificar es: false.


### Paso 4 — Devolver éxito al encontrar el objetivo

**Antes de escribir (lectura docente).** Cuando el dato coincide, el resto de la cadena ya no aporta nada a la decisión.

**Dónde escribir.** En `E06BusquedaLista.java`, inmediatamente después del bloque anterior que termina en `if (actual == null) return false;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (actual.dato == objetivo) return true;
```

**Qué hace cada parte y por qué.** Se compara el entero del nodo actual con objetivo. Si coinciden, true retorna inmediatamente a quien llamó.

**Cambio de estado que debes reconocer.** La búsqueda termina al primer nodo coincidente.

**Pregunta para comprobar comprensión.** ¿Se visitan los nodos posteriores al hallazgo? La respuesta que debes justificar es: No; ese camino se cierra con return true.


### Paso 5 — Delegar la búsqueda al siguiente nodo

**Antes de escribir (lectura docente).** Cuando no hay coincidencia todavía debemos explorar la parte restante de la lista.

**Dónde escribir.** En `E06BusquedaLista.java`, inmediatamente después del bloque anterior que termina en `if (actual.dato == objetivo) return true;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        return buscar(actual.siguiente, objetivo);
    }
```

**Qué hace cada parte y por qué.** `actual.siguiente` avanza una referencia. El return conserva el resultado del nivel posterior; sin él, la función no entregaría su boolean correctamente.

**Cambio de estado que debes reconocer.** Eventualmente se alcanza coincidencia o null.

**Pregunta para comprobar comprensión.** ¿Qué condición garantiza que la búsqueda termine? La respuesta que debes justificar es: Una lista acíclica finita donde cada referencia progresa hasta null.


### Paso 6 — Enlazar tres valores y probar ambos resultados

**Antes de escribir (lectura docente).** Necesitamos distinguir un dato que está en el segundo nodo de otro que no existe.

**Dónde escribir.** En `E06BusquedaLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(10);
        cabeza.siguiente = new Nodo(20);
        cabeza.siguiente.siguiente = new Nodo(30);
        System.out.println("Buscar 20: " + buscar(cabeza, 20));
        System.out.println("Buscar 50: " + buscar(cabeza, 50));
    }
}
```

**Qué hace cada parte y por qué.** Los enlaces crean 10→20→30→null. La primera llamada llega a 20 y retorna true; la segunda termina en null y retorna false.

**Cambio de estado que debes reconocer.** La salida contiene true y false sin recorrer por posiciones numéricas.

**Pregunta para comprobar comprensión.** ¿Qué llamada terminará por null? La respuesta que debes justificar es: Buscar 50, porque ningún dato coincide.


### Traza manual antes de ejecutar

Buscar 20: 10 no coincide → 20 coincide → true; buscar 50: 10→20→30→null → false.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Modelar un nodo con valor y siguiente | Podemos construir una cadena de nodos sin librerías adicionales. |
| Paso 2 | Declarar una búsqueda que responde verdadero o falso | La función debe decidir entre fin, coincidencia y avance. |
| Paso 3 | Reconocer el fin antes de consultar datos | Buscar en lista vacía resulta false sin errores. |
| Paso 4 | Devolver éxito al encontrar el objetivo | La búsqueda termina al primer nodo coincidente. |
| Paso 5 | Delegar la búsqueda al siguiente nodo | Eventualmente se alcanza coincidencia o null. |
| Paso 6 | Enlazar tres valores y probar ambos resultados | La salida contiene true y false sin recorrer por posiciones numéricas. |

### Archivo completo al terminar EJ06

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E06BusquedaLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
    static boolean buscar(Nodo actual, int objetivo) {
        if (actual == null) return false;
        if (actual.dato == objetivo) return true;
        return buscar(actual.siguiente, objetivo);
    }
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(10);
        cabeza.siguiente = new Nodo(20);
        cabeza.siguiente.siguiente = new Nodo(30);
        System.out.println("Buscar 20: " + buscar(cabeza, 20));
        System.out.println("Buscar 50: " + buscar(cabeza, 50));
    }
}
```

### Predicción y ejecución

**Predice:** ¿Buscar 10 desde el segundo nodo puede encontrar un nodo anterior?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E06BusquedaLista.java
java E06BusquedaLista
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Buscar 20: true
Buscar 50: false
```

**Cómo interpretarla:** Cada llamada examina exactamente el nodo que recibe; el valor boolean vuelve al origen.

**Modificación de comprobación:** Prueba buscar(null, 20); debe ser false sin excepción.

**Error o caso límite controlado:** Omitir el caso actual==null provoca NullPointerException al buscar una clave ausente. **Corrección razonada:** Restituir la prueba de null antes de acceder al campo dato. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T06 — Tarea espejo de EJ06

**Enunciado:** Realiza la búsqueda desde el segundo nodo: 20 debe encontrarse; 10 debe resultar ausente desde ese punto, aunque existe antes de él.

**Archivo para construir:** `T06BusquedaSublista.java`. Copia el archivo final de EJ06 a ese nombre; cambia también la declaración pública a `public class T06BusquedaSublista`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** Cambia el primer argumento para pasar cabeza.siguiente, sin cambiar ningún enlace.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Buscar 20: true y Buscar 10 desde segundo: false.

**Validación:** compila con `javac -encoding UTF-8 T06BusquedaSublista.java` y ejecuta `java T06BusquedaSublista`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ07 — Dividir una lista con referencias lento y rápido sin perder nodos

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

Merge Sort necesita dos sublistas antes de ordenarlas. Pero una lista enlazada no admite calcular la mitad con un acceso directo. Vamos a recorrerla mediante las referencias `lento` y `rapido`, que avanzan uno y dos nodos. Después guardaremos el principio de la segunda mitad y cortaremos un solo enlace. La evidencia serán dos cadenas completas, sin nodos perdidos, lo que permitirá construir el algoritmo final.

### ¿Qué problema concreto resolveremos?

Dividir 8→3→5→2→null en dos sublistas intactas.

**Conceptos que se pondrán a prueba:** Doble referencia lento/rápido, orden correcto de conservación y corte de enlace.

**Archivo que construiremos:** `E07DividirLista.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E07DividirLista`.


### Paso 1 — Representar los nodos y sus enlaces

**Antes de escribir (lectura docente).** Dividir una lista exige manipular referencias sin perder nodos; partiremos del modelo de Nodo ya conocido.

**Dónde escribir.** En `E07DividirLista.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E07DividirLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
```

**Qué hace cada parte y por qué.** Cada Nodo representa un dato y el siguiente. El constructor guarda el valor; el enlace nulo queda hasta que construyamos la cadena.

**Cambio de estado que debes reconocer.** Se prepara un tipo con el que se puede enlazar y cortar de forma controlada.

**Pregunta para comprobar comprensión.** ¿Qué información debemos conservar antes de cortar? La respuesta que debes justificar es: Una referencia al inicio de la segunda mitad.


### Paso 2 — Iniciar dos referencias con distintas velocidades

**Antes de escribir (lectura docente).** No tenemos índices para calcular el medio; emplearemos dos referencias que avanzan a diferentes velocidades.

**Dónde escribir.** En `E07DividirLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static Nodo encontrarMedio(Nodo cabeza) {
        if (cabeza == null) return null;
        Nodo lento = cabeza, rapido = cabeza.siguiente;
```

**Qué hace cada parte y por qué.** El caso cabeza==null protege la entrada. lento inicia en el primero y rapido en el segundo, decisión que produce una primera mitad de tamaño equilibrado cuando la lista tiene número par de nodos.

**Cambio de estado que debes reconocer.** Con 4 nodos, el medio elegido será el segundo nodo.

**Pregunta para comprobar comprensión.** ¿Por qué rapido comienza en cabeza.siguiente? La respuesta que debes justificar es: Para que, en lista par, lento termine en el último nodo de la primera mitad.


### Paso 3 — Desplazar uno y dos enlaces por iteración

**Antes de escribir (lectura docente).** Antes de cortar necesitamos localizar el último nodo de la primera sublista.

**Dónde escribir.** En `E07DividirLista.java`, inmediatamente después del bloque anterior que termina en `Nodo lento = cabeza, rapido = cabeza.siguiente;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        while (rapido != null && rapido.siguiente != null) {
            lento = lento.siguiente;
            rapido = rapido.siguiente.siguiente;
        }
        return lento;
    }
```

**Qué hace cada parte y por qué.** `&&` evita acceder a rapido.siguiente cuando rapido es null. lento avanza uno; rapido dos. Al terminar, return lento devuelve el lugar donde realizaremos el corte.

**Cambio de estado que debes reconocer.** Para [8,3,5,2], lento queda sobre el nodo 3.

**Pregunta para comprobar comprensión.** ¿Por qué no avanzar rapido uno por vez? La respuesta que debes justificar es: Ambas referencias avanzarían igual y no encontraríamos el punto medio.


### Paso 4 — Mostrar una lista sin modificar sus enlaces

**Antes de escribir (lectura docente).** Para comprobar ambas mitades mostraremos qué nodos permanecen accesibles desde cada cabeza.

**Dónde escribir.** En `E07DividirLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void mostrar(Nodo actual) {
        if (actual == null) { System.out.println("null"); return; }
        System.out.print(actual.dato + " -> ");
        mostrar(actual.siguiente);
    }
```

**Qué hace cada parte y por qué.** `mostrar` reutiliza el caso base null y avanza por siguiente; al final imprime la palabra null. No modifica la lista, solo representa sus enlaces.

**Cambio de estado que debes reconocer.** Las dos mitades pueden inspeccionarse por separado después del corte.

**Pregunta para comprobar comprensión.** ¿Mostrar cambia los enlaces? La respuesta que debes justificar es: No: solo lee referencias y datos.


### Paso 5 — Construir cuatro nodos y ubicar el punto de corte

**Antes de escribir (lectura docente).** Necesitamos una lista de longitud par para comprobar la decisión de inicialización de rapido.

**Dónde escribir.** En `E07DividirLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(8);
        cabeza.siguiente = new Nodo(3);
        cabeza.siguiente.siguiente = new Nodo(5);
        cabeza.siguiente.siguiente.siguiente = new Nodo(2);
        Nodo medio = encontrarMedio(cabeza);
```

**Qué hace cada parte y por qué.** Los enlaces forman 8→3→5→2→null. encontrarMedio(cabeza) devuelve el nodo con 3, sin cortar todavía ningún vínculo.

**Cambio de estado que debes reconocer.** Antes del corte, la lista está intacta y medio.siguiente apunta al nodo 5.

**Pregunta para comprobar comprensión.** ¿Cuál es el estado de medio.siguiente antes de cortar? La respuesta que debes justificar es: Apunta al primer nodo de la mitad derecha, el de valor 5.


### Paso 6 — Guardar la segunda mitad ANTES de desconectar

**Antes de escribir (lectura docente).** Cortar sin guardar la referencia a la derecha podría dejar inaccesible esa parte de la cadena.

**Dónde escribir.** En `E07DividirLista.java`, inmediatamente después del bloque anterior que termina en `Nodo medio = encontrarMedio(cabeza);`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
        Nodo derecha = medio.siguiente;
        medio.siguiente = null;
        System.out.print("Izquierda: "); mostrar(cabeza);
        System.out.print("Derecha: "); mostrar(derecha);
    }
}
```

**Qué hace cada parte y por qué.** `derecha=medio.siguiente` conserva el inicio de 5→2. Solo entonces `medio.siguiente=null` separa 8→3 del resto. Ambos mostrar verifican que no se perdieron datos.

**Cambio de estado que debes reconocer.** Izquierda 8→3→null, derecha 5→2→null.

**Pregunta para comprobar comprensión.** ¿Qué sucede si cortamos antes de guardar derecha? La respuesta que debes justificar es: La referencia al nodo 5 se pierde desde esta ruta; no podríamos mostrar la segunda mitad.


### Traza manual antes de ejecutar

Antes: 8→3→5→2→null. lento=8/rapido=3 → lento=3/rapido=2 → salir. Guardar 5 y cortar 3→null.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Representar los nodos y sus enlaces | Se prepara un tipo con el que se puede enlazar y cortar de forma controlada. |
| Paso 2 | Iniciar dos referencias con distintas velocidades | Con 4 nodos, el medio elegido será el segundo nodo. |
| Paso 3 | Desplazar uno y dos enlaces por iteración | Para [8,3,5,2], lento queda sobre el nodo 3. |
| Paso 4 | Mostrar una lista sin modificar sus enlaces | Las dos mitades pueden inspeccionarse por separado después del corte. |
| Paso 5 | Construir cuatro nodos y ubicar el punto de corte | Antes del corte, la lista está intacta y medio.siguiente apunta al nodo 5. |
| Paso 6 | Guardar la segunda mitad ANTES de desconectar | Izquierda 8→3→null, derecha 5→2→null. |

### Archivo completo al terminar EJ07

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E07DividirLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
    static Nodo encontrarMedio(Nodo cabeza) {
        if (cabeza == null) return null;
        Nodo lento = cabeza, rapido = cabeza.siguiente;
        while (rapido != null && rapido.siguiente != null) {
            lento = lento.siguiente;
            rapido = rapido.siguiente.siguiente;
        }
        return lento;
    }
    static void mostrar(Nodo actual) {
        if (actual == null) { System.out.println("null"); return; }
        System.out.print(actual.dato + " -> ");
        mostrar(actual.siguiente);
    }
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(8);
        cabeza.siguiente = new Nodo(3);
        cabeza.siguiente.siguiente = new Nodo(5);
        cabeza.siguiente.siguiente.siguiente = new Nodo(2);
        Nodo medio = encontrarMedio(cabeza);
        Nodo derecha = medio.siguiente;
        medio.siguiente = null;
        System.out.print("Izquierda: "); mostrar(cabeza);
        System.out.print("Derecha: "); mostrar(derecha);
    }
}
```

### Predicción y ejecución

**Predice:** Si se desconecta medio.siguiente antes de guardar derecha, ¿qué parte deja de estar disponible?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E07DividirLista.java
java E07DividirLista
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Izquierda: 8 -> 3 -> null
Derecha: 5 -> 2 -> null
```

**Cómo interpretarla:** La referencia derecha conserva la segunda sublista; null separa ambas cadenas sin borrar objetos.

**Modificación de comprobación:** Prueba con dos nodos; cada mitad debe contener exactamente uno.

**Error o caso límite controlado:** Invertir las líneas que guardan derecha y cortan medio.siguiente hace perder la cabeza de la derecha. **Corrección razonada:** Primero Nodo derecha=medio.siguiente; después medio.siguiente=null. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T07 — Tarea espejo de EJ07

**Enunciado:** Prueba una lista par de seis nodos 8→3→5→2→9→1; debe quedar 8→3→5 y 2→9→1, ambas terminadas en null.

**Archivo para construir:** `T07DividirListaPar.java`. Copia el archivo final de EJ07 a ese nombre; cambia también la declaración pública a `public class T07DividirListaPar`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** Después del nodo 2, enlaza 9 y 1 antes de ejecutar encontrarMedio; mantén guardar derecha antes de cortar.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Izquierda: 8 -> 3 -> 5 -> null; Derecha: 2 -> 9 -> 1 -> null.

**Validación:** compila con `javac -encoding UTF-8 T07DividirListaPar.java` y ejecuta `java T07DividirListaPar`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## EJ08 — Ordenar una lista enlazada completa con Merge Sort

### Objetivo técnico — Qué vamos a hacer y qué aprenderemos

En el ejemplo anterior aprendimos a separar una lista sin perder la segunda mitad; ahora completaremos el objetivo de ordenarla. Crearemos `mergeSort`, que decide cuándo detenerse, divide la cadena mediante lento y rápido, ordena recursivamente ambas mitades y devuelve el comienzo de la fusión. Construiremos además `fusionar`, que elige nodos de dos sublistas ordenadas y modifica únicamente referencias `siguiente`. La prueba final mostrará la misma cantidad de nodos, pero en orden ascendente.

### ¿Qué problema concreto resolveremos?

Ordenar 8→3→5→2→9→1→null y recuperar la nueva cabeza de la lista.

**Conceptos que se pondrán a prueba:** División de enlaces, fusión estable y retorno de nueva referencia cabeza.

**Archivo que construiremos:** `E08MergeSortLista.java` en la carpeta `S12_Recursividad`. El archivo empieza vacío. Los bloques mostrados son consecutivos; usa el mismo archivo y no reescribas ni cierres llaves antes de que aparezcan. Al terminar será un programa autónomo con clase `E08MergeSortLista`.


### Paso 1 — Representar un nodo de la lista ordenable

**Antes de escribir (lectura docente).** El algoritmo reorganizará enlaces entre objetos Nodo sin copiar sus datos a un vector.

**Dónde escribir.** En `E08MergeSortLista.java`, en el archivo vacío. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
public class E08MergeSortLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
```

**Qué hace cada parte y por qué.** Nodo contiene dato y siguiente; el constructor almacena el entero. La variable siguiente conecta con otros nodos y servirá para fusionar resultados.

**Cambio de estado que debes reconocer.** La lista puede representarse por una referencia a cabeza.

**Pregunta para comprobar comprensión.** ¿El algoritmo reemplaza los valores dato? La respuesta que debes justificar es: No: reorganiza las referencias siguiente de nodos existentes.


### Paso 2 — Definir Merge Sort y su caso base

**Antes de escribir (lectura docente).** Una lista vacía o de un solo nodo ya está ordenada y no debe seguir dividiéndose.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static Nodo mergeSort(Nodo cabeza) {
        if (cabeza == null || cabeza.siguiente == null) return cabeza;
```

**Qué hace cada parte y por qué.** La función devuelve Nodo porque la cabeza de la lista ordenada puede ser distinta de la original. `||` significa o lógico; basta que sea null o tenga un solo nodo para devolver la cabeza.

**Cambio de estado que debes reconocer.** La recursión termina en segmentos de cero o un elemento.

**Pregunta para comprobar comprensión.** ¿Por qué devolver Nodo y no void? La respuesta que debes justificar es: Porque luego de ordenar, el primer nodo puede cambiar.


### Paso 3 — Localizar el último nodo de la mitad izquierda

**Antes de escribir (lectura docente).** Para dividir la lista reutilizamos el razonamiento lento/rápido de EJ07.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `if (cabeza == null || cabeza.siguiente == null) return cabeza;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        Nodo lento = cabeza, rapido = cabeza.siguiente;
        while (rapido != null && rapido.siguiente != null) {
            lento = lento.siguiente;
            rapido = rapido.siguiente.siguiente;
        }
```

**Qué hace cada parte y por qué.** lento avanza una referencia y rapido dos; empezar rapido en el segundo nodo equilibra el corte de listas pares. El while protege null antes de acceder al siguiente enlace.

**Cambio de estado que debes reconocer.** Al terminar, lento señala dónde debe separarse la primera sublista.

**Pregunta para comprobar comprensión.** ¿Por qué este bucle no provoca recursión infinita? La respuesta que debes justificar es: Porque al cortar se crean sublistas más pequeñas que el caso original.


### Paso 4 — Guardar y cortar las dos sublistas en el orden correcto

**Antes de escribir (lectura docente).** La división debe conservar ambas cabezas antes de delegar el ordenamiento recursivo.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        Nodo derecha = lento.siguiente;
        lento.siguiente = null;
        Nodo izqOrdenada = mergeSort(cabeza);
        Nodo derOrdenada = mergeSort(derecha);
```

**Qué hace cada parte y por qué.** `derecha` guarda la referencia que podría perderse al cortar. Después de `lento.siguiente=null`, cada sublista es independiente. Las dos llamadas a mergeSort devuelven cabezas ya ordenadas.

**Cambio de estado que debes reconocer.** Tenemos dos sublistas ordenadas de menor tamaño, listas para fusionarse.

**Pregunta para comprobar comprensión.** ¿Qué debe hacerse antes de llamar a mergeSort(derecha)? La respuesta que debes justificar es: Guardar la referencia y separar la lista para reducir su tamaño.


### Paso 5 — Devolver la nueva cabeza fusionada

**Antes de escribir (lectura docente).** Dividir y ordenar mitades por separado no garantiza una lista globalmente ordenada.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `Nodo derOrdenada = mergeSort(derecha);`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        return fusionar(izqOrdenada, derOrdenada);
    }
```

**Qué hace cada parte y por qué.** `fusionar` combinará ambas sublistas en una sola cadena ordenada y devolverá su primer nodo, que se transmite al llamador.

**Cambio de estado que debes reconocer.** mergeSort devolverá una cabeza posiblemente diferente a la inicial.

**Pregunta para comprobar comprensión.** ¿Qué devuelve mergeSort después de ordenar? La respuesta que debes justificar es: La referencia a la cabeza de la lista ordenada.


### Paso 6 — Detener la fusión cuando una sublista se agota

**Antes de escribir (lectura docente).** La fusión trabaja con dos cadenas ya ordenadas: cuando una termina, la otra ya puede añadirse entera.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static Nodo fusionar(Nodo a, Nodo b) {
        if (a == null) return b;
        if (b == null) return a;
```

**Qué hace cada parte y por qué.** `a` y `b` son cabezas de sublistas ordenadas. Si a es null se devuelve b; si b es null se devuelve a. Estos casos base impiden tocar campos de una referencia nula.

**Cambio de estado que debes reconocer.** La combinación termina cuando se agota cualquiera de las mitades.

**Pregunta para comprobar comprensión.** ¿Es necesario comparar cuando b es null? La respuesta que debes justificar es: No: todos los nodos de a ya están ordenados.


### Paso 7 — Elegir un nodo y enlazar el resto de manera estable

**Antes de escribir (lectura docente).** En cada fusión elegimos el menor primer nodo y conectamos recursivamente el resto; no necesitamos crear objetos adicionales.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `if (b == null) return a;`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
        if (a.dato <= b.dato) {
            a.siguiente = fusionar(a.siguiente, b);
            return a;
        } else {
            b.siguiente = fusionar(a, b.siguiente);
            return b;
        }
    }
```

**Qué hace cada parte y por qué.** `<=` elige el nodo de a en empates y conserva estabilidad. La asignación a.siguiente (o b.siguiente) rehace el enlace al resto ordenado. `return a/b` declara quién es la nueva cabeza del segmento fusionado.

**Cambio de estado que debes reconocer.** Cada fusión reorganiza enlaces y devuelve la primera referencia ordenada.

**Pregunta para comprobar comprensión.** ¿Qué línea recompone la cadena? La respuesta que debes justificar es: La asignación a.siguiente = fusionar(...) o b.siguiente = fusionar(...).


### Paso 8 — Habilitar una visualización recursiva de enlaces

**Antes de escribir (lectura docente).** Para comprobar el resultado necesitamos ver que los enlaces forman una sola cadena completa y ordenada.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Aún no ejecutes: el archivo no está completo.

**Escribe ahora:**

```java
    static void mostrar(Nodo actual) {
        if (actual == null) { System.out.println("null"); return; }
        System.out.print(actual.dato + " -> ");
        mostrar(actual.siguiente);
    }
```

**Qué hace cada parte y por qué.** El método recorre referencias hasta null y muestra cada dato. No cambia los enlaces ya reordenados por mergeSort.

**Cambio de estado que debes reconocer.** La salida se puede comparar con un dibujo antes/después.

**Pregunta para comprobar comprensión.** ¿Mostrar cambia la estructura? La respuesta que debes justificar es: No: solo lee las referencias siguiendo siguiente.


### Paso 9 — Enlazar seis nodos y ordenar conservando la nueva cabeza

**Antes de escribir (lectura docente).** El ejercicio completo integra división, fusión, recuperación de cabeza y comprobación del orden de todos los nodos.

**Dónde escribir.** En `E08MergeSortLista.java`, inmediatamente después del bloque anterior que termina en `}`; mantén las llaves abiertas tal como aparecen. Este fragmento se suma a los anteriores; no reemplaza código ya escrito. Al cerrarse las llaves finales, el archivo está listo para compilar.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(8);
        cabeza.siguiente = new Nodo(3);
        cabeza.siguiente.siguiente = new Nodo(5);
        cabeza.siguiente.siguiente.siguiente = new Nodo(2);
        cabeza.siguiente.siguiente.siguiente.siguiente = new Nodo(9);
        cabeza.siguiente.siguiente.siguiente.siguiente.siguiente = new Nodo(1);
        System.out.print("Antes: "); mostrar(cabeza);
        cabeza = mergeSort(cabeza);
        System.out.print("Despues: "); mostrar(cabeza);
    }
}
```

**Qué hace cada parte y por qué.** La cadena inicial es 8→3→5→2→9→1. `cabeza = mergeSort(cabeza)` es esencial: la nueva primera referencia será el Nodo con dato 1. El segundo mostrar debe imprimir los seis nodos ascendentes.

**Cambio de estado que debes reconocer.** Antes y después contienen seis nodos; cambia únicamente su orden de enlaces.

**Pregunta para comprobar comprensión.** ¿Por qué reasignamos cabeza? La respuesta que debes justificar es: Porque el primer nodo ordenado puede ser otro y la función devuelve su referencia.


### Traza manual antes de ejecutar

8→3→5 | 2→9→1, después fragmentos mínimos, fusiones [3,5,8] y [1,2,9], finalmente 1→2→3→5→8→9.

### Auditoría del artefacto construido

| Elemento enseñado | Responsabilidad | Evidencia |
|---|---|---|
| Paso 1 | Representar un nodo de la lista ordenable | La lista puede representarse por una referencia a cabeza. |
| Paso 2 | Definir Merge Sort y su caso base | La recursión termina en segmentos de cero o un elemento. |
| Paso 3 | Localizar el último nodo de la mitad izquierda | Al terminar, lento señala dónde debe separarse la primera sublista. |
| Paso 4 | Guardar y cortar las dos sublistas en el orden correcto | Tenemos dos sublistas ordenadas de menor tamaño, listas para fusionarse. |
| Paso 5 | Devolver la nueva cabeza fusionada | mergeSort devolverá una cabeza posiblemente diferente a la inicial. |
| Paso 6 | Detener la fusión cuando una sublista se agota | La combinación termina cuando se agota cualquiera de las mitades. |
| Paso 7 | Elegir un nodo y enlazar el resto de manera estable | Cada fusión reorganiza enlaces y devuelve la primera referencia ordenada. |
| Paso 8 | Habilitar una visualización recursiva de enlaces | La salida se puede comparar con un dibujo antes/después. |
| Paso 9 | Enlazar seis nodos y ordenar conservando la nueva cabeza | Antes y después contienen seis nodos; cambia únicamente su orden de enlaces. |

### Archivo completo al terminar EJ08

Este archivo contiene **únicamente** los fragmentos anteriores, concatenados en el mismo orden. Compara su estructura con tu archivo sin introducir instrucciones nuevas.

```java
public class E08MergeSortLista {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
    static Nodo mergeSort(Nodo cabeza) {
        if (cabeza == null || cabeza.siguiente == null) return cabeza;
        Nodo lento = cabeza, rapido = cabeza.siguiente;
        while (rapido != null && rapido.siguiente != null) {
            lento = lento.siguiente;
            rapido = rapido.siguiente.siguiente;
        }
        Nodo derecha = lento.siguiente;
        lento.siguiente = null;
        Nodo izqOrdenada = mergeSort(cabeza);
        Nodo derOrdenada = mergeSort(derecha);
        return fusionar(izqOrdenada, derOrdenada);
    }
    static Nodo fusionar(Nodo a, Nodo b) {
        if (a == null) return b;
        if (b == null) return a;
        if (a.dato <= b.dato) {
            a.siguiente = fusionar(a.siguiente, b);
            return a;
        } else {
            b.siguiente = fusionar(a, b.siguiente);
            return b;
        }
    }
    static void mostrar(Nodo actual) {
        if (actual == null) { System.out.println("null"); return; }
        System.out.print(actual.dato + " -> ");
        mostrar(actual.siguiente);
    }
    public static void main(String[] args) {
        Nodo cabeza = new Nodo(8);
        cabeza.siguiente = new Nodo(3);
        cabeza.siguiente.siguiente = new Nodo(5);
        cabeza.siguiente.siguiente.siguiente = new Nodo(2);
        cabeza.siguiente.siguiente.siguiente.siguiente = new Nodo(9);
        cabeza.siguiente.siguiente.siguiente.siguiente.siguiente = new Nodo(1);
        System.out.print("Antes: "); mostrar(cabeza);
        cabeza = mergeSort(cabeza);
        System.out.print("Despues: "); mostrar(cabeza);
    }
}
```

### Predicción y ejecución

**Predice:** ¿Cuál será el primer nodo después de ordenar y por qué hay que reasignar cabeza?

Desde la terminal, dentro de la carpeta donde guardaste el archivo, ejecuta:

```bash
javac -encoding UTF-8 E08MergeSortLista.java
java E08MergeSortLista
```

**Salida esperada (la presentación puede variar solo en espacios finales):**

```text
Antes: 8 -> 3 -> 5 -> 2 -> 9 -> 1 -> null
Despues: 1 -> 2 -> 3 -> 5 -> 8 -> 9 -> null
```

**Cómo interpretarla:** La función devuelve una referencia de cabeza nueva; los enlaces se rehacen entre nodos ya existentes.

**Modificación de comprobación:** Prueba una lista de un nodo: mergeSort devuelve la misma cabeza sin cortar ni fusionar.

**Error o caso límite controlado:** No guardar derecha antes de cortar o no reasignar cabeza puede perder alcance a parte de la cadena ordenada. **Corrección razonada:** Guardar derecha, cortar después, ordenar las dos partes y asignar `cabeza = mergeSort(cabeza)`. Recupera siempre el código principal correcto después de estudiar el error.

**Explica con tus palabras:** qué datos recibe la función recursiva, qué cambia, cuándo termina, qué devuelve o imprime y cómo sabes que no se perdieron elementos.

### T08 — Tarea espejo de EJ08

**Enunciado:** Sustituye el último nodo por un 3 y ordena los seis nodos incluyendo los duplicados; asegúrate de conservar ambos tres y terminar en null.

**Archivo para construir:** `T08MergeListaRepetidos.java`. Copia el archivo final de EJ08 a ese nombre; cambia también la declaración pública a `public class T08MergeListaRepetidos`. Realiza la modificación descrita sin eliminar el caso base ni añadir librerías externas.

**Pista sin solución:** Modifica el dato al construir el último Nodo; no alteres la condición <= de fusionar ni el orden del corte.

**Antes de ejecutar:** explica qué salida cambiará y cuál permanecerá igual.

**Evidencia exigida:** Despues: 2 -> 3 -> 3 -> 5 -> 8 -> 9 -> null.

**Validación:** compila con `javac -encoding UTF-8 T08MergeListaRepetidos.java` y ejecuta `java T08MergeListaRepetidos`; compara todas las líneas con la evidencia esperada, no solo el primer valor.


---

## Cierre de sesión

Hoy reconstruimos el recorrido y la búsqueda de un vector a partir de una posición inicial, comprobando que el caso base debe ejecutarse antes de acceder al elemento que podría estar fuera de sus límites. Después aplicamos Merge Sort: una función divide el rango y deja resolver las mitades, mientras que otra fusiona de forma ordenada sin sobrescribir datos pendientes. El árbol de recursión del arreglo de ocho elementos permitió comprobar que cada segmento pequeño se combina en otro mayor. También analizamos registros de estudiantes: los promedios sirven para comparar, pero el registro completo debe permanecer intacto.

En las listas enlazadas sustituimos la posición numérica por una referencia a Nodo. Recorrimos y buscamos hasta `null`; luego encontramos el punto medio con dos referencias y aprendimos una decisión crítica de conservación: guardar la cabeza derecha antes de cortar el enlace. Con esa precaución terminamos una implementación recursiva que reorganiza nodos y devuelve una cabeza potencialmente diferente. No basta con obtener una salida aparentemente ordenada: comprueba siempre la cantidad de datos, el final `null` y la ausencia de enlaces perdidos. Como repaso, reconstruye a mano una división y una fusión y argumenta para qué sirven los casos base.

El siguiente reto es combinar las variaciones T01–T08 sin copiar sus soluciones. Puedes ampliar el aprendizaje mediante los recursos educativos de **Lideratec Academy**, el blog https://lideratecacademy.com/blog/ y el canal https://www.youtube.com/@LideratecAcademy. La meta de esta práctica es que puedas explicar cada línea que modifica flujo, datos o referencias y validar su efecto con evidencia observable.
