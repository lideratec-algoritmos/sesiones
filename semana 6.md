---
course_id: ISIL-AEDA-IA
session_id: S06
module_id: TEMA-06
course_version: 1.0
source_origin: PPT
status: validated
---

# Guía del estudiante - Sesión 06
## Lista enlazada simple: búsqueda, modificación, eliminación y ordenamiento

**Lideratec Academy**

Esta guía desarrolla una práctica completa sobre listas enlazadas simples en Java. El objetivo no es memorizar fragmentos, sino comprender qué representa cada nodo, por qué se necesita una referencia al siguiente nodo, cómo se recorre una lista sin índices y qué cambia exactamente cuando buscamos, modificamos, eliminamos u ordenamos.

La secuencia conserva el alcance de la sesión: búsqueda secuencial, modificación de datos, eliminación del primer nodo, nodo intermedio y último nodo, eliminación general y ordenamiento con el patrón de comparaciones mostrado en la sesión. La sesión también menciona que Bubble Sort tiene costo O(n^2) y Merge Sort O(n log n); aquí no se introduce una implementación adicional de Merge Sort porque no aparece desarrollada en el material de la sesión.

# 1. Propósito de la sesión

Al finalizar podrás construir una lista enlazada simple mínima y operar sobre ella sin perder referencias. Trabajarás con nodos que contienen un valor entero (`dato`) y una referencia (`siguiente`). A partir de esa estructura realizarás búsquedas secuenciales, actualizaciones, eliminaciones en distintas posiciones y un ordenamiento que intercambia los datos de los nodos.

La evidencia de aprendizaje será observable: podrás compilar, ejecutar y explicar por qué cada salida corresponde al estado real de la lista. También tendrás que anticipar el resultado antes de ejecutar y justificar qué referencia cambia en cada operación.

# 2. Resultado observable

## A. Lectura sugerida del docente

Una lista enlazada simple no se entiende de verdad cuando solo se ve un dibujo con cajas y flechas. Se comprende cuando puedes explicar qué objeto representa cada caja, qué variable conserva el inicio de la estructura y por qué una sola asignación sobre `siguiente` puede mantener o romper toda la cadena. En esta sesión iremos desde una lista mínima hasta un flujo integrado. Primero construiremos la estructura base para no trabajar sobre una caja negra. Luego aplicaremos búsqueda, modificación y tres formas de eliminación. Finalmente ordenaremos y combinaremos las operaciones en un caso completo. El criterio será siempre el mismo: antes de escribir una instrucción nueva, debes saber qué problema resuelve; después de ejecutarla, debes poder describir qué cambió y qué permaneció igual. La meta no es producir código largo, sino desarrollar control mental sobre el recorrido secuencial y sobre las referencias entre nodos.

## B. Desempeños observables

Al terminar podrás:

1. construir un `Nodo` y una `ListaEnlazada` mínima;
2. recorrer secuencialmente una lista hasta encontrar o descartar un valor;
3. modificar el dato de un nodo sin alterar sus enlaces;
4. eliminar el primer nodo actualizando `cabeza`;
5. eliminar un nodo intermedio reconectando `anterior.siguiente`;
6. eliminar el último nodo conservando la referencia al nodo anterior;
7. aplicar una eliminación general por valor;
8. ordenar los valores de la lista con comparaciones anidadas;
9. integrar búsqueda, modificación, eliminación y ordenamiento en una misma secuencia.

## C. Criterio de dominio

Demuestras dominio cuando puedes dibujar el estado `dato -> dato -> null`, señalar qué variable apunta a cada nodo durante el recorrido y explicar por qué una asignación produce el cambio observado. Copiar un método que funciona no es suficiente: debes poder predecir el efecto de una operación antes de ejecutarla y explicar un caso límite, por ejemplo una lista vacía o un valor que no existe.

# 3. Antes de iniciar

## Debe saber

- declarar clases, atributos, métodos y variables en Java;
- crear objetos con `new`;
- utilizar `if`, `while`, `for` y `return`;
- compilar y ejecutar un archivo Java sencillo;
- interpretar `null` como ausencia de referencia.

## Debe tener disponible

- JDK con `javac` y `java` disponibles;
- editor o IDE para crear archivos `.java`;
- terminal ubicada en la carpeta de trabajo.

Esta guía fue validada técnicamente con **OpenJDK 21.0.11**. Los ejemplos usan sintaxis básica compatible con versiones modernas de Java y no requieren librerías externas.

## No se asumirá todavía

- listas dobles;
- árboles, grafos o tablas hash;
- genéricos;
- colecciones de la biblioteca estándar como sustituto de la lista construida;
- implementación de Merge Sort sobre nodos.

# 4. Herramientas y recursos

## JDK

El JDK contiene el compilador `javac` y el runtime `java`. En esta sesión se necesita porque construiremos archivos fuente y comprobaremos su comportamiento real.

### Validación

En una terminal ejecuta:

```bash
javac -version
java -version
```

Debes observar una versión en ambos comandos. Si `javac` no existe pero `java` sí, probablemente tienes solo un runtime o la variable de entorno no está configurada correctamente.

## Editor o IDE

Puedes utilizar el editor institucional o cualquier IDE ya configurado. No necesitas crear un proyecto con Maven o Gradle porque la práctica no usa dependencias externas.

# 5. Preparación del entorno desde cero

## Ruta A - El entorno ya existe

1. Crea una carpeta llamada `S06_ListaEnlazada`.
2. Abre una terminal en esa carpeta.
3. Ejecuta `javac -version`.
4. Crea `Prueba.java` con el contenido siguiente:

```java
public class Prueba {
    public static void main(String[] args) {
        System.out.println("Java listo");
    }
}
```

5. Compila con `javac Prueba.java`.
6. Ejecuta con `java Prueba`.
7. Si aparece `Java listo`, el entorno está preparado.

## Ruta B - Equipo sin preparar

Solicita o instala un JDK permitido por tu institución. Después repite la validación anterior. No necesitas servidor, base de datos, Maven, Gradle ni librerías adicionales.

# 6. Regla de trabajo durante los ejemplos

En cada ejemplo vas a seguir la misma disciplina:

1. identificar el problema;
2. explicar por qué se necesita una nueva parte del código;
3. escribir un microbloque pequeño;
4. interpretar cada línea funcional nueva;
5. representar el estado antes y después;
6. predecir la salida;
7. compilar y ejecutar;
8. comprobar la salida;
9. provocar o analizar un caso límite;
10. resolver una tarea espejo.

# EJ01 - Construir la estructura base de una lista enlazada

## Qué vamos a construir

Un archivo `EJ01_BaseLista.java` capaz de crear tres nodos con valores 10, 20 y 30, enlazarlos y mostrar:

```text
10 -> 20 -> 30 -> null
```

## Qué aprenderás aquí

Aprenderás por qué necesitamos una clase `Nodo`, por qué cada nodo tiene `dato` y `siguiente`, qué representa `cabeza` y cómo se recorre la lista hasta encontrar el último nodo.

## Archivos involucrados

- `EJ01_BaseLista.java`

## Punto de partida

Todavía no existe ninguna estructura de lista. Partimos de un archivo vacío.

## Paso 1 - Crear la clase contenedora

### Estado antes

No existe un tipo Java que podamos compilar para esta práctica.

### ¿Por qué necesitamos este paso?

Java exige que el archivo `EJ01_BaseLista.java` tenga una clase pública con el mismo nombre. Esta clase será el contenedor del ejemplo.

### Escribe ahora

```java
public class EJ01_BaseLista {
}
```

### Explicación detallada

- `public` permite que la clase sea visible desde fuera del archivo si fuese necesario.
- `class` declara un nuevo tipo.
- `EJ01_BaseLista` debe coincidir exactamente con el nombre del archivo.
- Las llaves delimitan el contenido de la clase.

### Estado después

Existe una clase compilable, pero todavía no representa una lista.

## Paso 2 - Crear la clase `Nodo`

### ¿Qué necesitamos?

Una unidad que almacene un valor y pueda apuntar al siguiente elemento.

### ¿Por qué la necesitamos?

Una lista enlazada no guarda sus elementos en posiciones contiguas accesibles por índice. Cada elemento necesita conservar la referencia al siguiente.

### Escribe dentro de `EJ01_BaseLista`

```java
static class Nodo {
    int dato;
    Nodo siguiente;
}
```

### Explicación detallada

- `static class Nodo` crea una clase anidada. Se usa `static` para poder crear nodos desde el método `main` sin requerir una instancia de `EJ01_BaseLista`.
- `int dato;` guarda el valor del nodo. En la sesión trabajamos con enteros para concentrarnos en las referencias.
- `Nodo siguiente;` no guarda otro entero: guarda una referencia a otro objeto `Nodo`. Esta línea hace posible la cadena.
- Si `siguiente` es `null`, ese nodo no apunta a otro y por tanto puede representar el final de la lista.

### Elementos nuevos

- `Nodo`: tipo que representa una caja de la lista.
- `dato`: información almacenada.
- `siguiente`: enlace hacia otro nodo.

## Paso 3 - Agregar el constructor de `Nodo`

### ¿Por qué lo necesitamos?

Queremos que cada nuevo nodo nazca con un valor definido y sin enlace accidental a otro nodo.

### Escribe dentro de `Nodo`

```java
Nodo(int dato) {
    this.dato = dato;
    this.siguiente = null;
}
```

### Explicación detallada

- `Nodo(int dato)` es el constructor. Se ejecuta cada vez que hacemos `new Nodo(...)`.
- El parámetro `dato` recibe el valor con el que queremos crear el nodo.
- `this.dato = dato;` distingue el atributo del objeto (`this.dato`) del parámetro recibido (`dato`).
- `this.siguiente = null;` deja el nodo desconectado al momento de crearse. El enlace se establecerá después, de forma explícita.

### Estado después

Podemos crear nodos individuales, por ejemplo `new Nodo(10)`, pero aún no existe una estructura que recuerde cuál es el primero.

## Paso 4 - Crear `ListaEnlazada` y la referencia `cabeza`

### ¿Qué necesitamos?

Un objeto responsable de conservar el inicio de la cadena.

### Escribe

```java
static class ListaEnlazada {
    Nodo cabeza;
}
```

### Explicación detallada

- `ListaEnlazada` agrupa las operaciones que trabajan sobre la cadena.
- `Nodo cabeza;` guarda la referencia al primer nodo.
- Cuando `cabeza == null`, la lista está vacía.
- Si perdemos `cabeza` sin enlazar correctamente el resto, perdemos el acceso a la lista completa.

## Paso 5 - Crear el inicio de `insertarFinal`

### Problema actual

Queremos agregar un valor, pero primero debemos crear el nodo que lo contendrá.

### Escribe dentro de `ListaEnlazada`

```java
void insertarFinal(int dato) {
    Nodo nuevo = new Nodo(dato);
}
```

### Explicación detallada

- `void` indica que el método no devuelve un valor.
- `int dato` es el valor a insertar.
- `Nodo nuevo` declara una referencia local.
- `new Nodo(dato)` crea el objeto y ejecuta el constructor explicado antes.
- En este instante `nuevo.siguiente` es `null`.

## Paso 6 - Resolver el caso de lista vacía

### Estado antes

`cabeza == null` significa que no existe ningún nodo enlazado.

### Escribe dentro del método

```java
if (cabeza == null) {
    cabeza = nuevo;
    return;
}
```

### Explicación detallada

- La condición detecta el caso más simple: insertar en una lista vacía.
- `cabeza = nuevo;` hace que el nuevo nodo sea el primer nodo.
- `return;` termina el método porque no hay nada más que recorrer ni enlazar.
- Si omitiésemos `return`, el código posterior intentaría recorrer una lista aunque ya resolvimos la inserción.

### Estado después

Antes: `cabeza -> null`.

Después de insertar 10: `cabeza -> [10|null]`.

## Paso 7 - Recorrer hasta el último nodo

### ¿Por qué necesitamos un recorrido?

Si la lista ya contiene nodos, no existe un índice que nos lleve directamente al final. Debemos seguir referencias.

### Escribe

```java
Nodo actual = cabeza;
while (actual.siguiente != null) {
    actual = actual.siguiente;
}
```

### Explicación detallada

- `Nodo actual = cabeza;` crea una referencia auxiliar. No mueve `cabeza`; solo empieza a observar desde el primer nodo.
- `while (actual.siguiente != null)` pregunta si existe otro nodo después del actual.
- Mientras exista, `actual = actual.siguiente;` avanza exactamente un enlace.
- El ciclo termina cuando `actual.siguiente == null`; en ese momento `actual` es el último nodo.

### Traza manual

Para `10 -> 20 -> 30 -> null`:

1. `actual = 10`; como `10.siguiente` apunta a 20, avanza.
2. `actual = 20`; como `20.siguiente` apunta a 30, avanza.
3. `actual = 30`; como `30.siguiente == null`, termina.

## Paso 8 - Enlazar el nuevo nodo

### Escribe

```java
actual.siguiente = nuevo;
```

### Explicación detallada

Esta asignación cambia una referencia. Antes, el último nodo apuntaba a `null`; después, apunta a `nuevo`. Como `nuevo.siguiente` sigue siendo `null`, `nuevo` pasa a ser el nuevo último nodo.

### Estado antes y después

Antes: `10 -> 20 -> 30 -> null`

Acción: insertar 40

Después: `10 -> 20 -> 30 -> 40 -> null`

## Paso 9 - Crear `mostrar`

### ¿Por qué lo necesitamos?

Necesitamos evidencia visible del estado de la lista.

### Escribe

```java
void mostrar() {
    Nodo actual = cabeza;
    while (actual != null) {
        System.out.print(actual.dato + " -> ");
        actual = actual.siguiente;
    }
    System.out.println("null");
}
```

### Explicación detallada

- `actual = cabeza` inicia el recorrido.
- La condición `actual != null` permite procesar el último nodo. Observa la diferencia con `insertarFinal`, donde buscábamos al nodo cuyo `siguiente` fuese `null`.
- `System.out.print(actual.dato + " -> ");` muestra el dato del nodo actual sin saltar de línea.
- `actual = actual.siguiente;` avanza. Sin esta línea el ciclo sería infinito.
- Al salir del ciclo, `actual` ya es `null`; imprimimos literalmente `null` para representar el final de la cadena.

## Paso 10 - Crear `main`

```java
public static void main(String[] args) {
    ListaEnlazada lista = new ListaEnlazada();
    lista.insertarFinal(10);
    lista.insertarFinal(20);
    lista.insertarFinal(30);
    lista.mostrar();
}
```

### Explicación detallada

- `main` es el punto de entrada.
- `new ListaEnlazada()` crea una lista cuya `cabeza` inicia en `null`.
- La primera inserción activa el caso `cabeza == null`.
- Las siguientes inserciones recorren hasta el final.
- `mostrar()` recorre la estructura resultante.

## Auditoría del artefacto construido

| Elemento | Qué hace | Por qué existe | Efecto |
|---|---|---|---|
| `Nodo` | representa un elemento | cada elemento necesita dato y enlace | crea unidades enlazables |
| `siguiente` | referencia otro nodo | no hay índice directo | forma la cadena |
| `cabeza` | guarda el primer nodo | necesitamos un punto de entrada | permite recorrer toda la lista |
| `insertarFinal` | agrega al final | construye datos de prueba | cambia el último `siguiente` |
| `mostrar` | recorre e imprime | valida el estado | evidencia visible |

## Archivo completo al terminar este ejemplo
```java
public class EJ01_BaseLista {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }

            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.mostrar();
    }
}
```
## Antes de ejecutar: predicción

Predice la salida exacta. ¿Qué ocurre en la primera inserción y qué ocurre en las dos siguientes?

## Ejecuta

```bash
javac EJ01_BaseLista.java
java EJ01_BaseLista
```

## Resultado esperado

```text
10 -> 20 -> 30 -> null
```

## Cómo interpretarlo

La salida confirma que cada inserción mantuvo la cadena y que el último nodo terminó con `siguiente == null`.

## Variación A

Cambia el orden de inserción a 30, 10, 20. La lista conservará el orden de inserción porque todavía no aplicamos ordenamiento.

## Error controlado

Elimina temporalmente `actual = actual.siguiente;` dentro de `mostrar`. El ciclo no podrá avanzar. No dejes este error en el archivo final.

## Corrección razonada

Restaura la actualización de `actual`; un ciclo de recorrido necesita una variable de control que avance hacia la condición de terminación.

## Qué debes poder explicar con tus palabras

- por qué `siguiente` es de tipo `Nodo`;
- qué significa `cabeza == null`;
- por qué el recorrido de inserción usa `actual.siguiente != null`;
- por qué `mostrar` usa `actual != null`.

## T01 - Tarea espejo

### Enunciado

Construye una lista con 7, 14 y 21 y muestra `7 -> 14 -> 21 -> null`.

### Archivo a modificar

`T01_ConstruirLista.java`

### Restricción

No uses arreglos ni `ArrayList`.

### Pista

Reutiliza la estructura `Nodo`, `cabeza`, `insertarFinal` y `mostrar`.

### Evidencia

Captura o copia la salida de consola.

### Cómo saber si está correcta

La cadena debe mostrar los tres valores en el orden de inserción y terminar en `null`.


# EJ02 - Buscar un valor mediante recorrido secuencial

## Qué vamos a resolver

Agregar un método `buscar(int valor)` que recorra desde `cabeza` hasta encontrar el dato o llegar a `null`.

## Qué aprenderás aquí

La sesión remarca que una lista enlazada no tiene acceso directo por índice. Por eso la búsqueda necesita avanzar nodo por nodo y su costo en el peor caso es O(n).

## Archivo involucrado

`EJ02_BusquedaSecuencial.java`

## Punto de partida

Reutilizamos la estructura `Nodo` y `insertarFinal` del ejemplo anterior. Ese patrón ya fue explicado. La parte nueva es el método de búsqueda y la forma de interpretar su retorno.

## Paso 1 - Definir la firma

```java
Nodo buscar(int valor) {
}
```

### ¿Por qué devuelve `Nodo`?

La operación no solo necesita decir si existe el valor. Devolver la referencia al nodo encontrado permite acceder al `dato` y, si más adelante fuese necesario, a su enlace `siguiente`. Si no existe coincidencia devolveremos `null`.

### Elementos nuevos

- `valor`: dato objetivo de la búsqueda.
- retorno `Nodo`: referencia al nodo encontrado o `null`.

## Paso 2 - Colocar el recorrido en la cabeza

```java
Nodo actual = cabeza;
```

### Explicación

`actual` es una referencia temporal. Copiar `cabeza` dentro de `actual` no borra ni mueve la cabeza. Solo permite recorrer sin perder el punto de entrada de la lista.

## Paso 3 - Definir la condición de recorrido

```java
while (actual != null) {
}
```

### ¿Por qué `actual != null`?

Queremos inspeccionar todos los nodos, incluido el último. Después de procesar el último, `actual = actual.siguiente` producirá `null` y el ciclo terminará.

## Paso 4 - Comparar el dato

```java
if (actual.dato == valor) {
    return actual;
}
```

### Explicación línea por línea

- `actual.dato` accede al valor almacenado en el nodo que estamos visitando.
- `== valor` compara con el objetivo.
- Si coincide, `return actual` termina inmediatamente la búsqueda y devuelve esa referencia.
- No necesitamos seguir recorriendo porque el objetivo ya fue localizado.

## Paso 5 - Avanzar al siguiente nodo

```java
actual = actual.siguiente;
```

Esta línea es el avance del algoritmo. Si la omites, `actual` nunca cambia y una búsqueda fallida quedaría en ciclo infinito.

## Paso 6 - Resolver el caso no encontrado

```java
return null;
```

Llegar a esta línea significa que el ciclo terminó sin coincidencias. `null` representa ausencia de nodo encontrado.

## Traza manual

Lista: `10 -> 20 -> 30 -> 40 -> null`. Buscar 30:

1. `actual=10`: 10 != 30, avanza.
2. `actual=20`: 20 != 30, avanza.
3. `actual=30`: coincide, retorna ese nodo.
4. Los nodos posteriores ya no se visitan.

Buscar 99:

1. se revisan 10, 20, 30 y 40;
2. después de 40, `actual` pasa a `null`;
3. el ciclo termina y el método retorna `null`.

## Auditoría del algoritmo

| Parte | Función | Riesgo si falta |
|---|---|---|
| `actual = cabeza` | inicia recorrido | no habría punto de inicio |
| `actual != null` | controla fin | acceso inválido o recorrido incompleto |
| comparación | detecta coincidencia | nunca encontraría valores |
| avance | cambia de nodo | ciclo infinito |
| `return null` | expresa ausencia | método sin resultado para búsquedas fallidas |

## Archivo completo al terminar este ejemplo
```java
public class EJ02_BusquedaSecuencial {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        Nodo buscar(int valor) {
            Nodo actual = cabeza;
            while (actual != null) {
                if (actual.dato == valor) {
                    return actual;
                }
                actual = actual.siguiente;
            }
            return null;
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.insertarFinal(40);

        Nodo encontrado = lista.buscar(30);
        System.out.println(encontrado != null ? "Encontrado: " + encontrado.dato : "30 no encontrado");

        Nodo ausente = lista.buscar(99);
        System.out.println(ausente != null ? "Encontrado: " + ausente.dato : "99 no encontrado");
    }
}
```
## Antes de ejecutar: predicción

1. ¿Cuántos nodos se inspeccionan al buscar 30?
2. ¿Cuántos se inspeccionan al buscar 99?
3. ¿Por qué no podemos saltar directamente al tercer nodo como en un arreglo?

## Ejecuta

```bash
javac EJ02_BusquedaSecuencial.java
java EJ02_BusquedaSecuencial
```

## Resultado esperado

```text
Encontrado: 30
99 no encontrado
```

## Interpretación

El primer resultado demuestra terminación anticipada al encontrar el valor. El segundo recorre toda la lista. En el peor caso la cantidad de nodos inspeccionados crece linealmente con n: O(n).

## Variación A

Busca 10. El resultado se obtiene en la primera comparación.

## Variación B

Busca 40. El algoritmo visita toda la cadena hasta el último nodo, aunque el valor sí exista.

## Error controlado

Sustituye temporalmente `actual = actual.siguiente` por `actual = cabeza`. La búsqueda de un valor ausente nunca avanzará más allá del primer nodo.

## Corrección razonada

La actualización debe depender del nodo actual: `actual.siguiente`.

## Qué debes poder explicar

- por qué la búsqueda es secuencial;
- qué representa `return null`;
- cuándo termina el `while`;
- por qué el peor caso es O(n).

## T02 - Tarea espejo

### Enunciado

Construye `6 -> 12 -> 18 -> 24 -> null`, busca 18 y luego 99.

### Archivo

`T02_BuscarValores.java`

### Restricción

El método debe devolver `Nodo`, no `boolean`.

### Pista

Empieza con `Nodo actual = cabeza` y avanza una referencia por iteración.

### Evidencia

Debes mostrar que 18 es encontrado y 99 no.

### Criterio de validación

No debe existir acceso por índice ni conversión a arreglo.


# EJ03 - Modificar el dato de un nodo existente

## Qué vamos a resolver

Modificar el valor de un nodo sin alterar sus enlaces.

## Qué aprenderás aquí

La modificación combina dos acciones: buscar secuencialmente y, cuando existe coincidencia, reemplazar `actual.dato`. La referencia `actual.siguiente` no cambia.

## Archivo

`EJ03_ModificacionPorValor.java`

## Punto de partida

La estructura base es la misma. La novedad está en `modificar` y en usar un `boolean` para informar éxito o fallo.

## Paso 1 - Definir la firma

```java
boolean modificar(int valorBuscado, int nuevoValor) {
}
```

### Explicación

- `valorBuscado` identifica el dato que queremos localizar.
- `nuevoValor` es el dato que sustituirá al anterior.
- `boolean` permite retornar `true` si se modificó un nodo y `false` si el valor no existía.

## Paso 2 - Iniciar el recorrido

```java
Nodo actual = cabeza;
while (actual != null) {
}
```

Es el mismo patrón de búsqueda de EJ02. Se puede abreviar porque ya fue estudiado: `actual` empieza en cabeza, procesa cada nodo y termina al llegar a `null`.

## Paso 3 - Detectar coincidencia

```java
if (actual.dato == valorBuscado) {
}
```

La condición decide si el nodo actual es el que debe cambiar.

## Paso 4 - Actualizar solo el dato

```java
actual.dato = nuevoValor;
return true;
```

### Explicación detallada

- `actual.dato = nuevoValor` cambia el contenido almacenado.
- No asignamos nada a `actual.siguiente`; por tanto, los enlaces permanecen intactos.
- `return true` confirma que la operación se realizó y evita seguir recorriendo.

### Estado antes y después

Antes: `10 -> 20 -> 30 -> null`

Modificar 20 por 99:

Después: `10 -> 99 -> 30 -> null`

Los nodos siguen conectados en el mismo orden; solo cambió un dato.

## Paso 5 - Avanzar cuando no coincide

```java
actual = actual.siguiente;
```

Si el nodo actual no contiene `valorBuscado`, avanzamos al siguiente.

## Paso 6 - Informar que no se modificó

```java
return false;
```

Solo se alcanza si ningún nodo coincidió.

## Traza manual

Modificar 20 -> 99:

1. actual=10, no coincide;
2. actual=20, coincide;
3. se asigna 99;
4. retorna `true`;
5. 30 no necesita ser inspeccionado.

Modificar 77 -> 50:

1. se visitan 10, 99 y 30;
2. no hay coincidencia;
3. retorna `false`;
4. la lista queda intacta.

## Auditoría

La operación modifica **estado de dato**, no **estado de enlaces**. Esa distinción es central: modificar no equivale a eliminar ni a reinsertar.

## Archivo completo al terminar este ejemplo
```java
public class EJ03_ModificacionPorValor {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        boolean modificar(int valorBuscado, int nuevoValor) {
            Nodo actual = cabeza;
            while (actual != null) {
                if (actual.dato == valorBuscado) {
                    actual.dato = nuevoValor;
                    return true;
                }
                actual = actual.siguiente;
            }
            return false;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);

        System.out.print("Antes: ");
        lista.mostrar();

        System.out.println("Modificar 20 -> 99: " + lista.modificar(20, 99));
        System.out.print("Después: ");
        lista.mostrar();

        System.out.println("Modificar 77 -> 50: " + lista.modificar(77, 50));
    }
}
```
## Predicción

Antes de ejecutar, escribe la lista que esperas después de `modificar(20, 99)` y después del intento `modificar(77, 50)`.

## Ejecuta

```bash
javac EJ03_ModificacionPorValor.java
java EJ03_ModificacionPorValor
```

## Resultado esperado

```text
Antes: 10 -> 20 -> 30 -> null
Modificar 20 -> 99: true
Después: 10 -> 99 -> 30 -> null
Modificar 77 -> 50: false
```

## Cómo interpretarlo

`true` y `false` no son el dato modificado; son evidencia de si la operación encontró un nodo objetivo.

## Error controlado

Si escribes `actual = new Nodo(nuevoValor)` en lugar de modificar `actual.dato`, solo cambiarías la referencia local `actual`. No reconectarías esa nueva instancia dentro de la lista.

## Corrección

Modifica el atributo del objeto ya enlazado: `actual.dato = nuevoValor`.

## Qué debes poder explicar

- qué cambia y qué no cambia;
- por qué el método puede reutilizar el patrón de búsqueda;
- por qué `boolean` resulta útil.

## T03 - Tarea espejo

### Enunciado

En `5 -> 15 -> 25 -> null`, modifica 25 por 26 y luego intenta modificar 99 por 50.

### Archivo

`T03_ModificarValor.java`

### Restricción

No crees un nodo nuevo para modificar el existente.

### Evidencia

La lista debe quedar `5 -> 15 -> 26 -> null` y el intento sobre 99 debe devolver `false`.


# EJ04 - Eliminar el primer nodo

## Qué vamos a resolver

Eliminar el nodo al que apunta `cabeza`.

## Qué aprenderás aquí

El primer nodo es un caso especial porque no existe un nodo anterior que pueda saltarlo. La operación correcta modifica directamente `cabeza`.

## Archivo

`EJ04_EliminarPrimero.java`

## Paso 1 - Definir el método

```java
boolean eliminarPrimero() {
}
```

Devolvemos `boolean` para distinguir entre una eliminación real y una lista vacía.

## Paso 2 - Detectar lista vacía

```java
if (cabeza == null) {
    return false;
}
```

### Por qué se necesita

Si `cabeza` es `null`, no existe `cabeza.siguiente`. Intentar accederlo provocaría un error. La condición evita operar sobre una referencia inexistente.

## Paso 3 - Mover la cabeza

```java
cabeza = cabeza.siguiente;
```

### Explicación detallada

Supón `cabeza -> 10 -> 20 -> 30 -> null`.

- Antes, `cabeza` referencia al nodo 10.
- `cabeza.siguiente` referencia al nodo 20.
- La asignación hace que `cabeza` pase a referenciar 20.
- El nodo 10 queda fuera de la cadena accesible desde `cabeza`.
- No necesitamos mover 20 ni 30; sus enlaces ya eran correctos.

## Paso 4 - Confirmar éxito

```java
return true;
```

La estructura sí cambió, por eso el método informa éxito.

## Estado antes y después

Antes: `cabeza -> 10 -> 20 -> 30 -> null`

Acción: `cabeza = cabeza.siguiente`

Después: `cabeza -> 20 -> 30 -> null`

## Auditoría del orden

Primero debemos comprobar `cabeza == null`. Solo después es seguro leer `cabeza.siguiente`. Invertir ese orden introduce un riesgo en listas vacías.

## Archivo completo al terminar este ejemplo
```java
public class EJ04_EliminarPrimero {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        boolean eliminarPrimero() {
            if (cabeza == null) {
                return false;
            }
            cabeza = cabeza.siguiente;
            return true;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);

        System.out.print("Antes: ");
        lista.mostrar();
        System.out.println("Eliminación realizada: " + lista.eliminarPrimero());
        System.out.print("Después: ");
        lista.mostrar();
    }
}
```
## Predicción

¿Qué valor pasa a ser cabeza después de eliminar el primero? ¿Qué ocurriría si la lista tuviera un solo nodo?

## Ejecuta

```bash
javac EJ04_EliminarPrimero.java
java EJ04_EliminarPrimero
```

## Resultado esperado

```text
Antes: 10 -> 20 -> 30 -> null
Eliminación realizada: true
Después: 20 -> 30 -> null
```

## Caso límite controlado

En una lista con un solo nodo, `cabeza.siguiente` es `null`; por tanto, asignar `cabeza = cabeza.siguiente` deja la lista vacía. El mismo código cubre ese caso.

## Error controlado

No escribas `cabeza.dato = cabeza.siguiente.dato` como sustituto. Eso copiaría un valor, no eliminaría correctamente el primer nodo ni resolvería la estructura general.

## Qué debes poder explicar

- por qué no necesitamos `anterior`;
- por qué `cabeza` cambia;
- cómo el mismo paso funciona con un solo nodo.

## T04 - Tarea espejo

### Enunciado

Construye `8 -> 16 -> 24 -> null` y elimina el primer nodo.

### Archivo

`T04_EliminarPrimero.java`

### Evidencia

Debe quedar `16 -> 24 -> null`.

### Validación

Prueba también el método sobre una lista vacía: debe retornar `false` sin lanzar excepción.


# EJ05 - Eliminar un nodo intermedio

## Qué vamos a resolver

Eliminar un nodo que no sea ni el primero ni el último, manteniendo la cadena intacta.

## Qué aprenderás aquí

Necesitamos dos referencias simultáneas: `anterior` y `actual`. La clave de la eliminación es cambiar `anterior.siguiente` para que apunte directamente a `actual.siguiente`.

## Archivo

`EJ05_EliminarIntermedio.java`

## Paso 1 - Rechazar estructuras sin nodo intermedio

```java
if (cabeza == null || cabeza.siguiente == null) {
    return false;
}
```

### Explicación

- Lista vacía: no hay nada que eliminar.
- Un solo nodo: no puede existir un nodo intermedio.
- La condición `||` significa "o"; cualquiera de los dos casos impide la operación.

## Paso 2 - Preparar dos referencias

```java
Nodo anterior = cabeza;
Nodo actual = cabeza.siguiente;
```

### Por qué se necesita cada una

- `anterior` debe recordar quién está justo antes del candidato.
- `actual` representa el nodo que estamos inspeccionando.
- Empezamos `actual` en el segundo nodo porque el primero no puede ser intermedio.

## Paso 3 - Recorrer solo posiciones intermedias

```java
while (actual != null && actual.siguiente != null) {
}
```

### Explicación detallada

La condición tiene dos partes:

1. `actual != null` confirma que existe un nodo actual.
2. `actual.siguiente != null` confirma que el actual no es el último.

Así el cuerpo del ciclo solo procesa nodos que pueden ser intermedios.

## Paso 4 - Detectar el nodo objetivo

```java
if (actual.dato == valor) {
}
```

Si coincide, `actual` es el nodo intermedio que queremos desconectar.

## Paso 5 - Reconectar la cadena

```java
anterior.siguiente = actual.siguiente;
return true;
```

### Estado antes

`10 -> 20 -> 30 -> 40 -> null`

Para eliminar 30:

- `anterior` apunta a 20;
- `actual` apunta a 30;
- `actual.siguiente` apunta a 40.

### Acción

`anterior.siguiente = actual.siguiente`

### Estado después

`10 -> 20 -> 40 -> null`

El nodo 30 deja de ser alcanzable desde la cabeza. La operación "salta" el nodo.

## Paso 6 - Avanzar ambas referencias

```java
anterior = actual;
actual = actual.siguiente;
```

### Por qué el orden importa

Primero preservamos en `anterior` la referencia al nodo que era `actual`. Luego avanzamos `actual`. Si avanzáramos `actual` primero y después hiciéramos `anterior = actual`, ambas referencias apuntarían al mismo nodo y perderíamos la referencia al nodo previo.

## Paso 7 - Retornar `false`

```java
return false;
```

Significa que el valor no apareció en una posición intermedia.

## Traza manual

Eliminar 30:

1. anterior=10, actual=20; 20 no coincide.
2. anterior=20, actual=30; coincide.
3. 20.siguiente cambia de 30 a 40.
4. retorna `true`.

## Archivo completo al terminar este ejemplo
```java
public class EJ05_EliminarIntermedio {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        boolean eliminarIntermedio(int valor) {
            if (cabeza == null || cabeza.siguiente == null) {
                return false;
            }

            Nodo anterior = cabeza;
            Nodo actual = cabeza.siguiente;

            while (actual != null && actual.siguiente != null) {
                if (actual.dato == valor) {
                    anterior.siguiente = actual.siguiente;
                    return true;
                }
                anterior = actual;
                actual = actual.siguiente;
            }
            return false;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.insertarFinal(40);

        System.out.print("Antes: ");
        lista.mostrar();
        System.out.println("Eliminar 30: " + lista.eliminarIntermedio(30));
        System.out.print("Después: ");
        lista.mostrar();
        System.out.println("Intentar eliminar primero (10): " + lista.eliminarIntermedio(10));
        System.out.println("Intentar eliminar último (40): " + lista.eliminarIntermedio(40));
    }
}
```
## Predicción

1. ¿Qué referencia cambia al eliminar 30?
2. ¿Por qué 10 y 40 no pueden ser eliminados por este método?

## Ejecuta

```bash
javac EJ05_EliminarIntermedio.java
java EJ05_EliminarIntermedio
```

## Resultado esperado

```text
Antes: 10 -> 20 -> 30 -> 40 -> null
Eliminar 30: true
Después: 10 -> 20 -> 40 -> null
Intentar eliminar primero (10): false
Intentar eliminar último (40): false
```

## Error controlado

Cambia temporalmente el orden de avance a:

```java
actual = actual.siguiente;
anterior = actual;
```

Observa conceptualmente el problema: `anterior` deja de representar al nodo previo.

## Corrección razonada

Preserva primero el nodo actual como anterior y luego avanza actual.

## Qué debes poder explicar

- por qué hacen falta dos referencias;
- qué significa "saltar" un nodo;
- por qué el último nodo queda fuera del ciclo.

## T05 - Tarea espejo

### Enunciado

En `11 -> 22 -> 33 -> 44 -> null`, elimina 22. Después intenta eliminar 11 y 44 con el mismo método.

### Archivo

`T05_EliminarIntermedio.java`

### Resultado correcto

Debe quedar `11 -> 33 -> 44 -> null`; los intentos sobre 11 y 44 deben devolver `false`.


# EJ06 - Eliminar el último nodo

## Qué vamos a resolver

Encontrar el último nodo y hacer que el nodo anterior pase a ser el nuevo último.

## Qué aprenderás aquí

Esta operación necesita distinguir tres situaciones: lista vacía, lista con un solo nodo y lista con varios nodos.

## Archivo

`EJ06_EliminarUltimo.java`

## Paso 1 - Lista vacía

```java
if (cabeza == null) {
    return false;
}
```

No existe último nodo si no existe cabeza.

## Paso 2 - Un solo nodo

```java
if (cabeza.siguiente == null) {
    cabeza = null;
    return true;
}
```

### Explicación

Si `cabeza.siguiente` es `null`, la cabeza también es el último nodo. Eliminarlo consiste en dejar `cabeza` en `null`.

## Paso 3 - Preparar referencias para varios nodos

```java
Nodo anterior = null;
Nodo actual = cabeza;
```

### Explicación

- `actual` recorrerá la lista.
- `anterior` empezará en `null` porque todavía no hemos avanzado desde la cabeza.

## Paso 4 - Recorrer hasta que `actual` sea el último

```java
while (actual.siguiente != null) {
    anterior = actual;
    actual = actual.siguiente;
}
```

### Traza

Para `10 -> 20 -> 30 -> null`:

1. actual=10, anterior=null.
2. entra al ciclo: anterior=10, actual=20.
3. entra: anterior=20, actual=30.
4. 30.siguiente es null, termina.

Ahora `actual` es el último y `anterior` es 20.

## Paso 5 - Cortar el último enlace

```java
anterior.siguiente = null;
return true;
```

### Estado antes y después

Antes: `10 -> 20 -> 30 -> null`

Después: `10 -> 20 -> null`

El nodo 20 se convierte en el nuevo último porque su referencia `siguiente` pasa a `null`.

## Orden de seguridad

Los casos vacía y un solo nodo deben resolverse antes del recorrido. Así garantizamos que cuando lleguemos a `anterior.siguiente = null`, `anterior` realmente referencia un nodo válido.

## Archivo completo al terminar este ejemplo
```java
public class EJ06_EliminarUltimo {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        boolean eliminarUltimo() {
            if (cabeza == null) {
                return false;
            }
            if (cabeza.siguiente == null) {
                cabeza = null;
                return true;
            }

            Nodo anterior = null;
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                anterior = actual;
                actual = actual.siguiente;
            }
            anterior.siguiente = null;
            return true;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);

        System.out.print("Antes: ");
        lista.mostrar();
        System.out.println("Eliminación realizada: " + lista.eliminarUltimo());
        System.out.print("Después: ");
        lista.mostrar();
    }
}
```
## Predicción

¿Qué valores tendrán `anterior` y `actual` justo al terminar el ciclo para una lista 10 -> 20 -> 30?

## Ejecuta

```bash
javac EJ06_EliminarUltimo.java
java EJ06_EliminarUltimo
```

## Resultado esperado

```text
Antes: 10 -> 20 -> 30 -> null
Eliminación realizada: true
Después: 10 -> 20 -> null
```

## Caso límite controlado

Prueba conceptualmente una lista de un solo nodo. El recorrido no debe comenzar; el caso especial deja `cabeza = null`.

## Error controlado

Si omites el caso de un solo nodo, `anterior` permanecerá en `null` y después intentarías ejecutar `anterior.siguiente = null`.

## Corrección

Mantén el caso especial antes del recorrido.

## T06 - Tarea espejo

### Enunciado

Construye `2 -> 4 -> 6 -> 8 -> null` y llama cinco veces a `eliminarUltimo()` mostrando el resultado después de cada intento.

### Archivo

`T06_EliminarUltimo.java`

### Evidencia esperada

Las primeras cuatro llamadas deben eliminar 8, 6, 4 y 2. La quinta debe devolver `false` porque la lista ya está vacía.


# EJ07 - Eliminar por valor con un método general

## Qué vamos a resolver

Unificar la eliminación del primer nodo y de cualquier nodo posterior en un único método `eliminar(int valor)`.

## Qué aprenderás aquí

La sesión muestra un algoritmo general de eliminación con un caso especial para el primer nodo. Este ejemplo materializa exactamente esa idea.

## Archivo

`EJ07_EliminarPorValor.java`

## Paso 1 - Detectar lista vacía

```java
if (cabeza == null) {
    return false;
}
```

Sin cabeza no existe ningún valor que eliminar.

## Paso 2 - Resolver el primer nodo

```java
if (cabeza.dato == valor) {
    cabeza = cabeza.siguiente;
    return true;
}
```

### Por qué se trata aparte

Para los nodos posteriores necesitaremos `anterior`. El primer nodo no tiene anterior; por eso se actualiza `cabeza` directamente.

## Paso 3 - Preparar `anterior` y `actual`

```java
Nodo anterior = cabeza;
Nodo actual = cabeza.siguiente;
```

Ya sabemos que la cabeza no contiene el valor. Por eso el recorrido puede empezar en el segundo nodo.

## Paso 4 - Recorrer todos los nodos posteriores

```java
while (actual != null) {
}
```

A diferencia de EJ05, aquí sí permitimos que `actual` sea el último nodo. El método general debe poder eliminarlo.

## Paso 5 - Reconectar cuando existe coincidencia

```java
if (actual.dato == valor) {
    anterior.siguiente = actual.siguiente;
    return true;
}
```

### Por qué también sirve para el último nodo

Si `actual` es el último, entonces `actual.siguiente` vale `null`. La asignación se convierte en `anterior.siguiente = null`, exactamente lo que necesitamos para eliminar el último.

## Paso 6 - Avanzar

```java
anterior = actual;
actual = actual.siguiente;
```

El patrón conserva el nodo previo antes de mover `actual`.

## Paso 7 - No encontrado

```java
return false;
```

La lista queda sin cambios si el valor no existe.

## Traza representativa

Lista: `10 -> 20 -> 30 -> 40 -> null`.

Eliminar 30:

- cabeza no coincide;
- anterior=10, actual=20;
- avanza: anterior=20, actual=30;
- coincide: 20.siguiente pasa a 40.

Eliminar 10:

- coincide con cabeza;
- cabeza pasa a 20.

Eliminar 99:

- se recorre hasta `actual == null`;
- retorna `false`;
- no cambia ninguna referencia.

## Archivo completo al terminar este ejemplo
```java
public class EJ07_EliminarPorValor {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        boolean eliminar(int valor) {
            if (cabeza == null) {
                return false;
            }
            if (cabeza.dato == valor) {
                cabeza = cabeza.siguiente;
                return true;
            }

            Nodo anterior = cabeza;
            Nodo actual = cabeza.siguiente;
            while (actual != null) {
                if (actual.dato == valor) {
                    anterior.siguiente = actual.siguiente;
                    return true;
                }
                anterior = actual;
                actual = actual.siguiente;
            }
            return false;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.insertarFinal(40);

        lista.mostrar();
        System.out.println("Eliminar 30: " + lista.eliminar(30));
        lista.mostrar();
        System.out.println("Eliminar 10: " + lista.eliminar(10));
        lista.mostrar();
        System.out.println("Eliminar 99: " + lista.eliminar(99));
        lista.mostrar();
    }
}
```
## Predicción

Después de eliminar 30 y luego 10, ¿qué valor queda como cabeza? ¿Qué ocurre si intentas eliminar 99?

## Ejecuta

```bash
javac EJ07_EliminarPorValor.java
java EJ07_EliminarPorValor
```

## Resultado esperado

```text
10 -> 20 -> 30 -> 40 -> null
Eliminar 30: true
10 -> 20 -> 40 -> null
Eliminar 10: true
20 -> 40 -> null
Eliminar 99: false
20 -> 40 -> null
```

## Error controlado

Eliminar el caso especial de cabeza y empezar siempre con `anterior=cabeza`, `actual=cabeza.siguiente`. En ese diseño el valor de la cabeza nunca sería inspeccionado.

## Corrección

Mantén el caso `if (cabeza.dato == valor)` antes del recorrido posterior.

## Qué debes poder explicar

- por qué el primer nodo necesita tratamiento especial;
- por qué el último ya no necesita un método especial dentro de este algoritmo;
- qué ocurre cuando `actual.siguiente` es `null`.

## T07 - Tarea espejo

### Enunciado

Sobre `10 -> 20 -> 30 -> 40 -> null`, elimina 30, luego 10, luego 40 y finalmente intenta eliminar 99.

### Archivo

`T07_EliminarGeneral.java`

### Evidencia

Debes demostrar eliminación intermedia, primera, última y valor ausente con un solo método.


# EJ08 - Ordenar los datos de la lista

## Qué vamos a resolver

Aplicar el patrón de ordenamiento mostrado en la sesión: cada nodo `actual` se compara con los nodos posteriores y, si el dato de `actual` es mayor, se intercambian los valores almacenados.

## Qué aprenderás aquí

La lista conserva sus enlaces; el ordenamiento intercambia `dato`, no los nodos. El doble recorrido produce un costo cuadrático O(n^2).

## Archivo

`EJ08_OrdenamientoIntercambio.java`

## Paso 1 - Crear contador de comparaciones

```java
int comparaciones = 0;
```

### Por qué lo agregamos

No cambia el algoritmo; sirve para observar cuántas comparaciones se realizan. Con cuatro nodos, este patrón realiza 6 comparaciones entre pares.

## Paso 2 - Recorrido externo

```java
for (Nodo actual = cabeza; actual != null; actual = actual.siguiente) {
}
```

### Explicación de cada parte

- `Nodo actual = cabeza`: inicia en el primer nodo.
- `actual != null`: continúa mientras exista un nodo.
- `actual = actual.siguiente`: avanza al siguiente al final de cada vuelta.

`actual` representa la posición cuyo dato queremos dejar correctamente comparado respecto de los posteriores.

## Paso 3 - Recorrido interno

```java
for (Nodo indice = actual.siguiente; indice != null; indice = indice.siguiente) {
}
```

### Explicación

- `indice` comienza en el nodo posterior a `actual` para no comparar un nodo consigo mismo.
- Recorre todos los nodos que quedan a la derecha de `actual`.
- El nombre `indice` proviene del código de la sesión, pero aquí sigue siendo una referencia `Nodo`, no un índice numérico.

## Paso 4 - Contar la comparación

```java
comparaciones++;
```

Cada vez que `actual` se compara con un nodo posterior incrementamos el contador.

## Paso 5 - Detectar desorden

```java
if (actual.dato > indice.dato) {
}
```

Si el dato de `actual` es mayor, esos dos valores están invertidos respecto del orden ascendente.

## Paso 6 - Intercambiar los datos

```java
int temp = actual.dato;
actual.dato = indice.dato;
indice.dato = temp;
```

### Por qué necesitamos `temp`

Si escribiéramos primero `actual.dato = indice.dato` y luego `indice.dato = actual.dato`, perderíamos el valor original de `actual`. `temp` lo preserva durante el intercambio.

### Estado local

Si `actual.dato = 29` e `indice.dato = 3`:

1. `temp = 29`;
2. `actual.dato = 3`;
3. `indice.dato = 29`.

Los objetos nodo no cambian de posición; solo cambian sus valores.

## Paso 7 - Devolver las comparaciones

```java
return comparaciones;
```

Nos permite validar el comportamiento del doble recorrido.

## Traza resumida con 4 -> 29 -> 3 -> 13

- actual=4: compara con 29, 3 y 13; al comparar con 3 intercambia y el primer dato pasa a 3.
- actual pasa al segundo nodo: sigue comparando con los posteriores y mueve valores menores hacia posiciones anteriores.
- al finalizar, los datos quedan 3, 4, 13, 29.
- total de pares comparados: 3 + 2 + 1 = 6.

## Complejidad

El número de comparaciones crece aproximadamente con n(n-1)/2, por lo que el orden de crecimiento es O(n^2). La sesión menciona Merge Sort con O(n log n) como alternativa más eficiente, pero su implementación no se desarrolla aquí.

## Archivo completo al terminar este ejemplo
```java
public class EJ08_OrdenamientoIntercambio {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        int ordenar() {
            int comparaciones = 0;
            for (Nodo actual = cabeza; actual != null; actual = actual.siguiente) {
                for (Nodo indice = actual.siguiente; indice != null; indice = indice.siguiente) {
                    comparaciones++;
                    if (actual.dato > indice.dato) {
                        int temp = actual.dato;
                        actual.dato = indice.dato;
                        indice.dato = temp;
                    }
                }
            }
            return comparaciones;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(4);
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(13);

        System.out.print("Antes: ");
        lista.mostrar();
        int comparaciones = lista.ordenar();
        System.out.print("Después: ");
        lista.mostrar();
        System.out.println("Comparaciones: " + comparaciones);
    }
}
```
## Predicción

1. ¿Qué lista esperas al terminar?
2. ¿Cuántas comparaciones habrá con cuatro nodos?
3. ¿Cambian las referencias `siguiente` durante el ordenamiento?

## Ejecuta

```bash
javac EJ08_OrdenamientoIntercambio.java
java EJ08_OrdenamientoIntercambio
```

## Resultado esperado

```text
Antes: 4 -> 29 -> 3 -> 13 -> null
Después: 3 -> 4 -> 13 -> 29 -> null
Comparaciones: 6
```

## Error controlado

Elimina `temp` e intenta hacer el intercambio usando solo dos asignaciones. Explica por qué ambos nodos pueden terminar con el mismo valor.

## Corrección

Conserva el valor original en una variable temporal.

## Qué debes poder explicar

- por qué hay dos recorridos;
- por qué `indice` empieza en `actual.siguiente`;
- qué cambia: datos; qué no cambia: enlaces;
- por qué el costo es O(n^2).

## T08 - Tarea espejo

### Enunciado

Ordena `17 -> 3 -> 21 -> 8 -> null` con el mismo patrón y muestra las comparaciones.

### Archivo

`T08_OrdenarLista.java`

### Resultado esperado

`3 -> 8 -> 17 -> 21 -> null` y 6 comparaciones.


# EJ09 - Integrador: buscar, modificar, eliminar y ordenar

## Qué vamos a resolver

Combinar las operaciones de la sesión sobre una sola lista para comprobar que cada método conserva una estructura válida para el siguiente.

## Archivo

`EJ09_IntegradorOperaciones.java`

## Punto de partida

Construimos `4 -> 29 -> 3 -> 13 -> null`. Los métodos `buscar`, `modificar`, `eliminar`, `ordenar` y `mostrar` repiten exactamente los patrones ya explicados en EJ02, EJ03, EJ07 y EJ08. No se introduce una lógica nueva dentro de esos métodos; la novedad es **el orden de uso** y la interpretación acumulativa del estado.

## Paso 1 - Estado inicial

```java
lista.insertarFinal(4);
lista.insertarFinal(29);
lista.insertarFinal(3);
lista.insertarFinal(13);
```

Estado: `4 -> 29 -> 3 -> 13 -> null`.

## Paso 2 - Buscar 29

```java
Nodo encontrado = lista.buscar(29);
System.out.println("Buscar 29: " + (encontrado != null ? "encontrado" : "no encontrado"));
```

### Explicación

- `buscar(29)` devuelve una referencia al nodo si existe.
- La expresión condicional comprueba si la referencia es distinta de `null`.
- La búsqueda no modifica la lista.

Estado después: sigue siendo `4 -> 29 -> 3 -> 13 -> null`.

## Paso 3 - Modificar 29 por 15

```java
System.out.println("Modificar 29 -> 15: " + lista.modificar(29, 15));
lista.mostrar();
```

Estado después: `4 -> 15 -> 3 -> 13 -> null`.

Solo cambia el dato del segundo nodo.

## Paso 4 - Eliminar el primer valor 4

```java
System.out.println("Eliminar primero (4): " + lista.eliminar(4));
lista.mostrar();
```

El método general detecta que `cabeza.dato == 4` y mueve `cabeza` al siguiente nodo.

Estado después: `15 -> 3 -> 13 -> null`.

## Paso 5 - Eliminar el último valor 13

```java
System.out.println("Eliminar último (13): " + lista.eliminar(13));
lista.mostrar();
```

Durante el recorrido, `actual` llega al nodo 13. Como `actual.siguiente == null`, la asignación `anterior.siguiente = actual.siguiente` equivale a `anterior.siguiente = null`.

Estado después: `15 -> 3 -> null`.

## Paso 6 - Ordenar el estado restante

```java
System.out.println("Comparaciones al ordenar: " + lista.ordenar());
System.out.print("Final: ");
lista.mostrar();
```

Con dos nodos existe un único par de comparación. Como 15 > 3, se intercambian los datos.

Estado final: `3 -> 15 -> null`.

## Por qué el orden de operaciones importa

Si ordenaras antes de eliminar, la lista intermedia sería diferente, aunque las operaciones individuales siguieran siendo válidas. En esta actividad queremos observar un flujo determinista: buscar -> modificar -> eliminar primero -> eliminar último -> ordenar.

## Auditoría integrada

| Operación | Cambia datos | Cambia enlaces | Puede recorrer |
|---|---:|---:|---:|
| buscar | no | no | sí |
| modificar | sí | no | sí |
| eliminar | no necesariamente | sí | sí |
| ordenar | sí | no | sí, doble recorrido |

## Archivo completo al terminar este ejemplo
```java
public class EJ09_IntegradorOperaciones {
    static class Nodo {
        int dato;
        Nodo siguiente;

        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }

    static class ListaEnlazada {
        Nodo cabeza;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (cabeza == null) {
                cabeza = nuevo;
                return;
            }
            Nodo actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevo;
        }

        Nodo buscar(int valor) {
            Nodo actual = cabeza;
            while (actual != null) {
                if (actual.dato == valor) {
                    return actual;
                }
                actual = actual.siguiente;
            }
            return null;
        }

        boolean modificar(int valorBuscado, int nuevoValor) {
            Nodo actual = cabeza;
            while (actual != null) {
                if (actual.dato == valorBuscado) {
                    actual.dato = nuevoValor;
                    return true;
                }
                actual = actual.siguiente;
            }
            return false;
        }

        boolean eliminar(int valor) {
            if (cabeza == null) {
                return false;
            }
            if (cabeza.dato == valor) {
                cabeza = cabeza.siguiente;
                return true;
            }
            Nodo anterior = cabeza;
            Nodo actual = cabeza.siguiente;
            while (actual != null) {
                if (actual.dato == valor) {
                    anterior.siguiente = actual.siguiente;
                    return true;
                }
                anterior = actual;
                actual = actual.siguiente;
            }
            return false;
        }

        int ordenar() {
            int comparaciones = 0;
            for (Nodo actual = cabeza; actual != null; actual = actual.siguiente) {
                for (Nodo indice = actual.siguiente; indice != null; indice = indice.siguiente) {
                    comparaciones++;
                    if (actual.dato > indice.dato) {
                        int temp = actual.dato;
                        actual.dato = indice.dato;
                        indice.dato = temp;
                    }
                }
            }
            return comparaciones;
        }

        void mostrar() {
            Nodo actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaEnlazada lista = new ListaEnlazada();
        lista.insertarFinal(4);
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(13);

        System.out.print("Inicio: ");
        lista.mostrar();

        Nodo encontrado = lista.buscar(29);
        System.out.println("Buscar 29: " + (encontrado != null ? "encontrado" : "no encontrado"));

        System.out.println("Modificar 29 -> 15: " + lista.modificar(29, 15));
        lista.mostrar();

        System.out.println("Eliminar primero (4): " + lista.eliminar(4));
        lista.mostrar();

        System.out.println("Eliminar último (13): " + lista.eliminar(13));
        lista.mostrar();

        System.out.println("Comparaciones al ordenar: " + lista.ordenar());
        System.out.print("Final: ");
        lista.mostrar();
    }
}
```
## Predicción completa

Antes de ejecutar escribe los estados esperados después de cada operación:

1. inicio;
2. después de buscar;
3. después de modificar;
4. después de eliminar 4;
5. después de eliminar 13;
6. después de ordenar.

## Ejecuta

```bash
javac EJ09_IntegradorOperaciones.java
java EJ09_IntegradorOperaciones
```

## Resultado esperado

```text
Inicio: 4 -> 29 -> 3 -> 13 -> null
Buscar 29: encontrado
Modificar 29 -> 15: true
4 -> 15 -> 3 -> 13 -> null
Eliminar primero (4): true
15 -> 3 -> 13 -> null
Eliminar último (13): true
15 -> 3 -> null
Comparaciones al ordenar: 1
Final: 3 -> 15 -> null
```

## Error controlado

Imagina que `eliminar` cambiara `actual = actual.siguiente` antes de conservar `anterior`. ¿Qué método posterior sería el primero en evidenciar una estructura incorrecta? La respuesta depende de qué enlace se pierda, pero el problema se origina en la eliminación, no en el método posterior.

## Corrección razonada

Valida el estado después de cada operación. Una práctica integrada debe localizar el primer punto donde el estado deja de coincidir con la predicción.

## Qué debes poder explicar

- qué métodos cambian datos y cuáles cambian referencias;
- por qué una lista válida después de una operación permite ejecutar la siguiente;
- qué estado esperas en cada checkpoint.

## T09 - Tarea espejo

### Enunciado

Construye `5 -> 20 -> 8 -> 14 -> null` y realiza:

1. buscar 20;
2. modificar 20 por 12;
3. eliminar 5;
4. eliminar 14;
5. ordenar;
6. mostrar el resultado final.

### Archivo

`T09_Integrador.java`

### Resultado esperado

La lista final debe ser `8 -> 12 -> null`.

### Evidencia

Muestra el estado después de cada operación y explica qué línea cambió el dato o la referencia relevante.

# 16. Cierre de sesión

En esta sesión construiste una lista enlazada simple desde su unidad mínima: el nodo. A partir de allí viste que `cabeza` no es un dato más, sino la referencia que permite entrar a toda la estructura. La búsqueda mostró por qué una lista enlazada necesita recorrido secuencial: no existe acceso directo por índice. La modificación reutilizó ese recorrido para cambiar únicamente el dato de un nodo. Las eliminaciones permitieron distinguir tres situaciones: mover `cabeza` cuando se elimina el primero, reconectar `anterior.siguiente` cuando se elimina un nodo intermedio y conservar el nodo previo para eliminar el último. Después integraste esas ideas en un método general de eliminación por valor.

El ordenamiento permitió observar otro aspecto: es posible conservar los enlaces y cambiar solamente los datos. El doble recorrido produjo un costo O(n^2), coherente con la discusión de eficiencia de la sesión. El aprendizaje más importante no es memorizar métodos, sino poder representar el estado antes y después de cada asignación crítica.

Repasa especialmente los casos límite: lista vacía, un solo nodo, valor ausente y eliminación del primer o último nodo. Si puedes explicar qué referencia cambia en cada caso, tienes control real de la estructura.

Para continuar reforzando estructuras de datos y programación Java puedes utilizar los recursos de **Lideratec Academy**:

- Blog: https://lideratecacademy.com/blog/
- YouTube: https://www.youtube.com/@LideratecAcademy

El laboratorio materializado de FASE 15 puede utilizarse opcionalmente para comparar tu solución con archivos ejecutables, pero no es necesario para completar esta guía.
