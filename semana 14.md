---
course_id: ISIL-30710
session_id: S14
source_origin: PPT
status: validated
---

# Sesión 14 — Árboles binarios, BST y árboles de decisión en IA

**Curso:** Algoritmo y Estructura de Datos Basado en IA  
**Marca educativa:** Lideratec Academy  
**Lenguaje:** Java (solo biblioteca estándar)  
**Modalidad:** clase guiada con continuidad autónoma

## Propósito de la sesión — Lectura docente

En aplicaciones reales, muchos datos están organizados por jerarquías: una carpeta contiene subcarpetas, un árbol de búsqueda relaciona valores menores y mayores, y un sistema de clasificación puede tomar decisiones siguiendo preguntas sucesivas. Aunque estas tres situaciones tienen una forma visual parecida, no se resuelven de la misma manera. En esta sesión aprenderemos a identificar qué representa cada nodo, cómo se enlaza con sus hijos y de qué modo el orden de visita cambia lo que obtenemos al recorrer un árbol.

Comenzaremos por una representación mínima en Java y examinaremos altura, profundidad y hojas. Después recorreremos el mismo árbol en preorden, inorden y postorden; lo construiremos mediante inserción automática y estudiaremos cuidadosamente tres casos de eliminación, porque desconectar una referencia sin considerar sus hijos puede destruir parte de la estructura. Al finalizar, relacionaremos el concepto de árbol con una aplicación de inteligencia artificial: una regla de clasificación por nota y asistencia, junto con el cálculo de entropía y ganancia de información sobre datos ficticios. El resultado será un conjunto de siete programas autónomos y trazas observables. La meta no es copiar métodos, sino ser capaces de explicar qué enlace cambia, qué caso base detiene una llamada recursiva y por qué un resultado es correcto.

## Resultado de aprendizaje y criterio de dominio

Al terminar la sesión podrás representar un árbol binario mediante nodos con referencias, distinguir altura, profundidad y nivel, justificar el orden de visita de tres recorridos y construir un BST con inserción y eliminación. Además, reconocerás que un árbol de decisión es un modelo de condiciones y clases, no un BST, y explicarás qué indica una reducción de entropía en un ejemplo de clasificación. Demostrarás dominio cuando puedas predecir una salida antes de ejecutar el código, explicar la función de las líneas fundamentales, modificar una operación sin perder conexiones y validar el resultado obtenido en consola.

## Antes de iniciar

**Debes saber:** clases, objetos, atributos, referencias, `if`, `for`, métodos y llamadas recursivas sencillas.  
**Debes tener:** JDK con `javac` y `java`; un editor de texto o IDE Java. No se requieren bibliotecas ni cuentas externas.  
**No asumiremos:** rotaciones AVL, balance automático, entrenamiento de modelos con bibliotecas de machine learning.

### Comprobación del entorno

En una terminal escribe `java -version` y `javac -version`. Deben mostrar una versión instalada del runtime y del compilador. Crea una carpeta local `S14_Arboles` y guarda cada ejemplo como el archivo exacto que se indica (el nombre debe coincidir con `public class`). Desde esa carpeta ejecuta `javac Nombre.java`, luego `java Nombre` sin extensión. Si aparece «class ... is public, should be declared in a file named ...», comprueba la coincidencia entre clase y archivo. Si no se reconoce `javac`, verifica el JDK y el PATH con apoyo del docente.

### Convenciones que utilizaremos

* **Altura y profundidad:** contamos **aristas**, no nodos. La altura de una hoja es 0 y la profundidad de la raíz es 0.
* **Nivel:** en la convención de esta guía es igual a profundidad; otras fuentes comienzan en nivel 1.
* **BST:** cada valor del subárbol izquierdo es menor que el del nodo y cada valor del derecho es mayor. En estos ejemplos no insertamos duplicados.
* **Eliminación con dos hijos:** el ejemplo de código usará el **sucesor inorden**, es decir, el mínimo del subárbol derecho. El **predecesor** (máximo del subárbol izquierdo) es una alternativa válida diferente.

### Ruta sugerida para una sesión de 120 minutos

Apertura y conceptos: 15 min. EJ01 y EJ02 guiados: 25 min. EJ03 con práctica: 15 min. EJ04–EJ06 con contraste de casos: 40 min. EJ07 y conexión con IA: 15 min. Cierre y evidencias: 10 min. Las tareas espejo y variaciones restantes continúan como trabajo autónomo; no se pretende resolver las catorce prácticas íntegras en los 120 minutos.


---

# EJ01 — Representar nodos, altura y profundidad

## Objetivo técnico — qué vamos a hacer y aprenderemos

Comenzaremos representando un árbol binario con objetos enlazados por referencias. Primero construiremos una clase Nodo porque un valor aislado no puede representar sus relaciones padre-hijo; después conectaremos un árbol pequeño y calcularemos su altura y número de hojas mediante recursión. Para distinguir profundidad de altura, ubicaremos manualmente la distancia desde la raíz hasta el nodo 6 y contrastaremos este dato con la longitud del camino más largo desde la raíz. La evidencia será una consola que muestre altura 2, profundidad 2 y tres hojas; explicaremos por qué esos números no son intercambiables.

## Qué problema concreto resolveremos

Una tabla lineal no nos dice qué nodo depende de cuál. Necesitamos referencias explícitas y una regla clara para contar las aristas de cada camino.

**Archivo que crearás:** `EJ01_ArbolConceptos.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Construir un nodo que conserve las relaciones

**Antes de escribir — qué necesitamos y por qué:** Dentro de `EJ01_ArbolConceptos.java`, primero declara la clase pública y anida `Nodo`; añade `valor`, `izquierda`, `derecha` y el constructor. El campo `valor` conserva el dato y las dos referencias representan como máximo dos hijos. El constructor asigna el argumento al dato del nodo; al crearse, ambas referencias quedan `null` hasta que se conecten hijos.

**Dónde escribirlo:** abre `EJ01_ArbolConceptos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static class Nodo {
    int valor;
    Nodo izquierda, derecha;
    Nodo(int valor) { this.valor = valor; }
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Decidir qué significa altura en aristas

**Antes de escribir — qué necesitamos y por qué:** Debajo de `Nodo` y antes de `main`, escribe `altura`. Si recibimos `null`, devolvemos −1; así una hoja tiene altura `1 + max(-1,-1) = 0`. La llamada a `Math.max` elige el camino más largo, no el número total de nodos. Cada llamada recursiva procesa un subárbol más pequeño y termina en referencias nulas.

**Dónde escribirlo:** abre `EJ01_ArbolConceptos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static int altura(Nodo nodo) {
    if (nodo == null) return -1;
    return 1 + Math.max(altura(nodo.izquierda), altura(nodo.derecha));
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Detectar hojas sin confundirlas con nodos internos

**Antes de escribir — qué necesitamos y por qué:** Agrega `contarHojas` debajo de `altura`: el árbol vacío aporta cero; un nodo sin hijos aporta uno; cualquier otro nodo suma las hojas de ambos subárboles. Dos condiciones `== null` unidas con `&&` son necesarias porque una hoja carece de ambos hijos, no solamente del izquierdo.

**Dónde escribirlo:** abre `EJ01_ArbolConceptos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static int contarHojas(Nodo nodo) {
    if (nodo == null) return 0;
    if (nodo.izquierda == null && nodo.derecha == null) return 1;
    return contarHojas(nodo.izquierda) + contarHojas(nodo.derecha);
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Enlazar el árbol y comprobar los resultados

**Antes de escribir — qué necesitamos y por qué:** En `main`, crea la raíz 8, luego enlaza 4 y 12; finalmente incorpora 2 y 6 como hijos de 4. La escritura `raiz.izquierda.derecha` navega primero hasta 4 y desde allí enlaza 6. Cuenta las aristas 8→4→6 para establecer la profundidad de 6 en 2; la línea impresa de profundidad representa un cálculo manual, no un método automático.

**Dónde escribirlo:** abre `EJ01_ArbolConceptos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo raiz = new Nodo(8);
raiz.izquierda = new Nodo(4);
raiz.derecha = new Nodo(12);
raiz.izquierda.izquierda = new Nodo(2);
raiz.izquierda.derecha = new Nodo(6);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ01_ArbolConceptos` coincida con `EJ01_ArbolConceptos.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ01

```java
public class EJ01_ArbolConceptos {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static int altura(Nodo nodo) {
        if (nodo == null) return -1;
        return 1 + Math.max(altura(nodo.izquierda), altura(nodo.derecha));
    }
    static int contarHojas(Nodo nodo) {
        if (nodo == null) return 0;
        if (nodo.izquierda == null && nodo.derecha == null) return 1;
        return contarHojas(nodo.izquierda) + contarHojas(nodo.derecha);
    }
    public static void main(String[] args) {
        Nodo raiz = new Nodo(8);
        raiz.izquierda = new Nodo(4);
        raiz.derecha = new Nodo(12);
        raiz.izquierda.izquierda = new Nodo(2);
        raiz.izquierda.derecha = new Nodo(6);
        System.out.println("altura=" + altura(raiz));
        System.out.println("profundidad(6)=2");
        System.out.println("hojas=" + contarHojas(raiz));
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ01_ArbolConceptos.java
java EJ01_ArbolConceptos
```

**Resultado esperado:**

```text
altura=2
profundidad(6)=2
hojas=3
```


**Interpretación:** La altura depende del camino descendente más largo; la profundidad depende de la posición respecto de la raíz, y las hojas son exactamente los nodos sin descendientes directos.

**Variación guiada:** Agrega un hijo derecho al nodo 30. ¿Cambia la altura? ¿Cambia el número de hojas?

**Error o caso límite controlado:** Si retornas 0 para una referencia nula en el método altura, obtendrás altura 1 para la hoja: es otra convención (contar nodos), pero no coincide con la convención en aristas de esta sesión.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T01 — Tarea espejo de EJ01

**Enunciado:** Construye en `T01_ArbolConceptos.java` un árbol cuya raíz sea 20, con hijos 10 y 30, y cadena 20→10→5→3. Calcula altura y cantidad de hojas, y explica manualmente la profundidad del nodo 3.

**Archivo para construir:** `T01_ArbolConceptos.java`; parte de la misma estructura de `EJ01_ArbolConceptos.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T01_ArbolConceptos` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ02 — Recorrer el mismo árbol en tres órdenes

## Objetivo técnico — qué vamos a hacer y aprenderemos

Ahora que un nodo tiene dos referencias, aprenderemos a procesar todos los valores sin perder ninguno. Prepararemos un árbol binario de búsqueda con siete nodos y escribiremos tres métodos recursivos: preorden, inorden y postorden. Las tres funciones visitan los mismos datos y utilizan el mismo caso base; la diferencia crucial es el lugar en que se imprime el valor respecto de las llamadas a los hijos. Al ejecutarlas, comprobaremos las tres secuencias y justificaremos por qué el inorden produce valores ascendentes solo porque nuestro árbol cumple la propiedad BST.

## Qué problema concreto resolveremos

Tener un árbol enlazado no establece un orden de procesamiento. Necesitamos decidir cuándo visitar la raíz dentro de cada subárbol.

**Archivo que crearás:** `EJ02_Recorridos.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Conservar el nodo y fijar el caso base

**Antes de escribir — qué necesitamos y por qué:** En `EJ02_Recorridos.java` usa la clase `Nodo` con dato y dos referencias. Antes de leer un valor, cada recorrido verifica `n == null` para detener la recursión al llegar más allá de una hoja; sin esa condición se intentaría acceder al campo de una referencia inexistente.

**Dónde escribirlo:** abre `EJ02_Recorridos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static class Nodo {
    int valor;
    Nodo izquierda, derecha;
    Nodo(int valor) { this.valor = valor; }
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Implementar preorden: raíz, izquierda, derecha

**Antes de escribir — qué necesitamos y por qué:** Después de `Nodo` crea `preorden`. La impresión ocurre antes de explorar los hijos: por eso 8 aparece primero. Luego se agota el subárbol izquierdo y finalmente el derecho. El parámetro `n` identifica el nodo que está procesando cada invocación recursiva.

**Dónde escribirlo:** abre `EJ02_Recorridos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static void preorden(Nodo n) {
    if (n == null) return;
    System.out.print(n.valor + " ");
    preorden(n.izquierda);
    preorden(n.derecha);
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Implementar inorden: izquierda, raíz, derecha

**Antes de escribir — qué necesitamos y por qué:** Ubica `inorden` a continuación. El valor se imprime entre ambas llamadas: primero todos los menores del subárbol izquierdo, después la raíz y al final los mayores del derecho. Ese efecto de orden ascendente presupone que los enlaces corresponden a un BST válido.

**Dónde escribirlo:** abre `EJ02_Recorridos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static void inorden(Nodo n) {
    if (n == null) return;
    inorden(n.izquierda);
    System.out.print(n.valor + " ");
    inorden(n.derecha);
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Implementar postorden: izquierda, derecha, raíz

**Antes de escribir — qué necesitamos y por qué:** Añade `postorden`. Ahora ambas llamadas preceden a la impresión; así 8 se emite al final. La estructura recursiva es idéntica, pero cambiar una sola posición altera toda la secuencia de visita.

**Dónde escribirlo:** abre `EJ02_Recorridos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static void postorden(Nodo n) {
    if (n == null) return;
    postorden(n.izquierda);
    postorden(n.derecha);
    System.out.print(n.valor + " ");
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 5 — Construir un BST de siete nodos y realizar tres visitas

**Antes de escribir — qué necesitamos y por qué:** Dentro de `main` crea 8 y enlaza 4/12, 2/6 y 10/14. Los valores ubicados a la izquierda son menores que sus respectivos padres y los de la derecha son mayores. Invoca cada recorrido, coloca un rótulo distinto y usa `println()` para separar las salidas.

**Dónde escribirlo:** abre `EJ02_Recorridos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo r = new Nodo(8);
r.izquierda = new Nodo(4); r.derecha = new Nodo(12);
r.izquierda.izquierda = new Nodo(2);
r.izquierda.derecha = new Nodo(6);
r.derecha.izquierda = new Nodo(10);
r.derecha.derecha = new Nodo(14);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ02_Recorridos` coincida con `EJ02_Recorridos.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ02

```java
public class EJ02_Recorridos {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static void preorden(Nodo n) {
        if (n == null) return;
        System.out.print(n.valor + " ");
        preorden(n.izquierda);
        preorden(n.derecha);
    }
    static void inorden(Nodo n) {
        if (n == null) return;
        inorden(n.izquierda);
        System.out.print(n.valor + " ");
        inorden(n.derecha);
    }
    static void postorden(Nodo n) {
        if (n == null) return;
        postorden(n.izquierda);
        postorden(n.derecha);
        System.out.print(n.valor + " ");
    }
    public static void main(String[] args) {
        Nodo r = new Nodo(8);
        r.izquierda = new Nodo(4); r.derecha = new Nodo(12);
        r.izquierda.izquierda = new Nodo(2);
        r.izquierda.derecha = new Nodo(6);
        r.derecha.izquierda = new Nodo(10);
        r.derecha.derecha = new Nodo(14);
        System.out.print("preorden: "); preorden(r); System.out.println();
        System.out.print("inorden: "); inorden(r); System.out.println();
        System.out.print("postorden: "); postorden(r); System.out.println();
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ02_Recorridos.java
java EJ02_Recorridos
```

**Resultado esperado:**

```text
preorden: 8 4 2 6 12 10 14 
inorden: 2 4 6 8 10 12 14 
postorden: 2 6 4 10 14 12 8
```


**Interpretación:** Los recorridos tienen la misma navegación, pero distinta ubicación temporal de la visita al nodo actual.

**Variación guiada:** Intercambia únicamente la ubicación de la instrucción `System.out.print` en un método y predice la secuencia.

**Error o caso límite controlado:** No atribuyas el orden ascendente del inorden a cualquier árbol binario: depende de la propiedad BST, que no tiene todo árbol binario.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T02 — Tarea espejo de EJ02

**Enunciado:** En `T02_Recorridos.java` construye un BST con raíz 20, hijos 10 y 30 e hijos 5 y 15 debajo de 10; ejecuta los tres recorridos y explica dónde se visita 20.

**Archivo para construir:** `T02_Recorridos.java`; parte de la misma estructura de `EJ02_Recorridos.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T02_Recorridos` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ03 — Insertar valores respetando la propiedad BST

## Objetivo técnico — qué vamos a hacer y aprenderemos

En los ejemplos anteriores enlazábamos cada hijo de manera manual. Ahora implementaremos la inserción de un BST para que el árbol se construya a partir de un conjunto de valores y de comparaciones automáticas. Crearemos un método que recibe la raíz de un subárbol, decide entre izquierda y derecha y devuelve la referencia que el padre debe conservar. Luego aplicaremos ese método a siete valores y recorreremos el resultado en inorden. El programa deberá imprimirlos de menor a mayor, lo que nos permitirá verificar la propiedad de búsqueda sin necesidad de dibujar todas las conexiones.

## Qué problema concreto resolveremos

Enlazar manualmente cada hijo es propenso a errores; necesitamos una regla única para decidir dónde insertar cada valor nuevo.

**Archivo que crearás:** `EJ03_InsertarBST.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Dar un punto de retorno a la inserción

**Antes de escribir — qué necesitamos y por qué:** En `EJ03_InsertarBST.java` conserva la misma clase `Nodo`. Crea `insertar` con retorno `Nodo`: debe devolver la raíz —nueva o existente— del subárbol procesado. Si la referencia es nula, encontramos una posición disponible y creamos el nodo.

**Dónde escribirlo:** abre `EJ03_InsertarBST.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo insertar(Nodo n, int valor) {
    if (n == null) return new Nodo(valor);
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Comparar el valor y actualizar una referencia concreta

**Antes de escribir — qué necesitamos y por qué:** Dentro de `insertar`, si el nuevo valor es menor, vuelve a llamar al método sobre el hijo izquierdo y guarda el resultado en `n.izquierda`. Si es mayor, actúa sobre `n.derecha`. La rama `else if` ignora duplicados por decisión explícita; no crea un nodo adicional. Finalmente devuelve `n` para que el padre mantenga el enlace al subárbol completo.

**Dónde escribirlo:** abre `EJ03_InsertarBST.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
if (valor < n.valor) n.izquierda = insertar(n.izquierda, valor);
else if (valor > n.valor) n.derecha = insertar(n.derecha, valor);
return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Preparar un recorrido para comprobar el orden

**Antes de escribir — qué necesitamos y por qué:** Añade el método `inorden` después de insertar. Primero visita izquierda, luego imprime y finalmente visita derecha. Este método es nuestra prueba de observación: si el árbol preservó la propiedad BST, la salida aparecerá ascendente.

**Dónde escribirlo:** abre `EJ03_InsertarBST.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static void inorden(Nodo n) {
    if (n == null) return;
    inorden(n.izquierda);
    System.out.print(n.valor + " ");
    inorden(n.derecha);
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Insertar desde una raíz inicialmente vacía

**Antes de escribir — qué necesitamos y por qué:** En `main` define `Nodo raiz = null`, recorre el arreglo de números y recuerda asignar el resultado de `insertar` a `raiz`. La primera llamada crea la raíz 8; las demás descienden por comparaciones. El bucle `for (int valor : ...)` toma un elemento por iteración.

**Dónde escribirlo:** abre `EJ03_InsertarBST.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo raiz = null;
for (int valor : new int[]{8, 4, 12, 2, 6, 10, 14}) {
    raiz = insertar(raiz, valor);
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ03_InsertarBST` coincida con `EJ03_InsertarBST.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ03

```java
public class EJ03_InsertarBST {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static Nodo insertar(Nodo n, int valor) {
        if (n == null) return new Nodo(valor);
        if (valor < n.valor) n.izquierda = insertar(n.izquierda, valor);
        else if (valor > n.valor) n.derecha = insertar(n.derecha, valor);
        return n;
    }
    static void inorden(Nodo n) {
        if (n == null) return;
        inorden(n.izquierda);
        System.out.print(n.valor + " ");
        inorden(n.derecha);
    }
    public static void main(String[] args) {
        Nodo raiz = null;
        for (int valor : new int[]{8, 4, 12, 2, 6, 10, 14}) {
            raiz = insertar(raiz, valor);
        }
        System.out.print("BST: "); inorden(raiz); System.out.println();
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ03_InsertarBST.java
java EJ03_InsertarBST
```

**Resultado esperado:**

```text
BST: 2 4 6 8 10 12 14
```


**Interpretación:** La inserción no modifica arbitrariamente todo el árbol: actualiza la rama explorada y devuelve cada raíz de subárbol para conservar las referencias.

**Variación guiada:** Introduce el valor 8 dos veces en el arreglo de EJ03: razona por qué el programa lo imprime solo una vez.

**Error o caso límite controlado:** Si llamas `insertar(raiz, valor)` sin guardar su retorno, el primer nodo creado cuando la raíz es `null` se pierde y la salida permanece vacía.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T03 — Tarea espejo de EJ03

**Enunciado:** En `T03_InsertarBST.java` inserta en este orden 15, 8, 22, 4, 11, 19 y 27; predice la raíz y muestra el recorrido inorden.

**Archivo para construir:** `T03_InsertarBST.java`; parte de la misma estructura de `EJ03_InsertarBST.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T03_InsertarBST` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ04 — Eliminar una hoja del BST

## Objetivo técnico — qué vamos a hacer y aprenderemos

Aprenderemos a eliminar un nodo hoja de un árbol binario de búsqueda sin alterar las relaciones entre los demás nodos. Partiremos de la función de inserción ya trabajada, añadiremos un método general de eliminación que busca el valor y retorna la referencia que el padre debe conservar, y aplicaremos específicamente el caso de una hoja. Al borrar el 2, su referencia pasa a `null`; comprobaremos con recorridos inorden que el 2 desapareció y que los demás números mantienen su orden. Comprenderemos también por qué el mismo método contiene ramas para otros casos que practicaremos después.

## Qué problema concreto resolveremos

Una hoja no tiene hijos; al borrarla debemos hacer que el padre deje de apuntar a ella, sin desconectar los otros nodos.

**Archivo que crearás:** `EJ04_EliminarHoja.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Preparar el mismo BST y reutilizar inserción

**Antes de escribir — qué necesitamos y por qué:** En este archivo independiente escribe `Nodo` e `insertar` exactamente como se muestran en el código completo. `Nodo` conserva valor y dos enlaces; `insertar` desciende a izquierda/derecha y devuelve la raíz actualizada. También incorpora `inorden`, que recorrerá el árbol antes y después para que podamos observar la modificación.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo insertar(Nodo n, int v) {
    if (n == null) return new Nodo(v);
    if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
    else if (v > n.valor) n.derecha = insertar(n.derecha, v);
    return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Buscar el nodo objetivo sin romper enlaces

**Antes de escribir — qué necesitamos y por qué:** Crea `eliminar` con retorno `Nodo` después de `insertar`. Cuando `n == null`, el dato no está en esa rama. Las comparaciones `v < n.valor` y `v > n.valor` guían la búsqueda; al reasignar `n.izquierda` o `n.derecha` conservamos el resultado de la llamada recursiva. El `else` significa que hemos encontrado exactamente el nodo a eliminar.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo eliminar(Nodo n, int v) {
    if (n == null) return null;
    if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
    else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
    else {
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Resolver los casos cero o un hijo mediante retorno

**Antes de escribir — qué necesitamos y por qué:** Dentro del `else` primero comprueba si falta el hijo izquierdo: retorna el derecho, que puede ser `null` cuando es una hoja. Si existe el izquierdo pero falta el derecho, retorna el izquierdo. De este modo, el padre recibe directamente la referencia al hijo que permanece, o `null` si ya no hay ninguno.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
if (n.izquierda == null) return n.derecha;
if (n.derecha == null) return n.izquierda;
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Conservar el caso de dos hijos con sucesor inorden

**Antes de escribir — qué necesitamos y por qué:** Solo si ambos hijos existen, comienza en `n.derecha`, desciende a la izquierda hasta hallar el menor valor de ese subárbol (sucesor), copia su valor al nodo encontrado y elimina la copia de la rama derecha. Estas líneas completan la implementación general y se activan únicamente cuando el nodo eliminado tiene dos hijos.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo sucesor = n.derecha;
while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
n.valor = sucesor.valor;
n.derecha = eliminar(n.derecha, sucesor.valor);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 5 — Cerrar el método y conservar la nueva raíz

**Antes de escribir — qué necesitamos y por qué:** Después del cierre de `else`, devuelve `n` y cierra `eliminar`. El retorno es importante también en `main`: una eliminación puede reemplazar la propia raíz del árbol. Por eso la llamada correcta es `raiz = eliminar(raiz, objetivo)`.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 6 — Ejecutar la operación concreta y comprobar el antes/después

**Antes de escribir — qué necesitamos y por qué:** En `main` inserta los valores [8, 4, 12, 2, 6, 10, 14] y muestra el inorden antes de borrar. Invoca `raiz = eliminar(raiz, 2)`: el árbol resultante puede cambiar su raíz y por eso guardamos el retorno. Repite el inorden y compara el conjunto de valores sin confundir la visita ascendente con un dibujo de enlaces.

**Dónde escribirlo:** abre `EJ04_EliminarHoja.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo raiz = null;
for (int v : new int[]{8, 4, 12, 2, 6, 10, 14}) raiz = insertar(raiz, v);
System.out.print("antes: "); inorden(raiz); System.out.println();
raiz = eliminar(raiz, 2);
System.out.print("despues: "); inorden(raiz); System.out.println();
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ04_EliminarHoja` coincida con `EJ04_EliminarHoja.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ04

```java
public class EJ04_EliminarHoja {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static Nodo insertar(Nodo n, int v) {
        if (n == null) return new Nodo(v);
        if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
        else if (v > n.valor) n.derecha = insertar(n.derecha, v);
        return n;
    }
    static Nodo eliminar(Nodo n, int v) {
        if (n == null) return null;
        if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
        else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
        else {
            if (n.izquierda == null) return n.derecha;
            if (n.derecha == null) return n.izquierda;
            Nodo sucesor = n.derecha;
            while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
            n.valor = sucesor.valor;
            n.derecha = eliminar(n.derecha, sucesor.valor);
        }
        return n;
    }
    static void inorden(Nodo n) {
        if (n == null) return;
        inorden(n.izquierda);
        System.out.print(n.valor + " ");
        inorden(n.derecha);
    }
    public static void main(String[] args) {
        Nodo raiz = null;
        for (int v : new int[]{8, 4, 12, 2, 6, 10, 14}) raiz = insertar(raiz, v);
        System.out.print("antes: "); inorden(raiz); System.out.println();
        raiz = eliminar(raiz, 2);
        System.out.print("despues: "); inorden(raiz); System.out.println();
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ04_EliminarHoja.java
java EJ04_EliminarHoja
```

**Resultado esperado:**

```text
antes: 2 4 6 8 10 12 14 
despues: 4 6 8 10 12 14
```


**Interpretación:** En una hoja, ambos hijos son nulos; retornar `n.derecha` equivale a devolver `null` y limpia el enlace del padre.

**Variación guiada:** Elimina 14 en lugar de 2 y compara qué rama del árbol queda afectada.

**Error o caso límite controlado:** No uses `n = null` sin devolver y reatribuir la referencia en el nivel superior: el padre conservaría el enlace antiguo.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T04 — Tarea espejo de EJ04

**Enunciado:** Elimina el nodo hoja 5 de `T04_EliminarHoja.java` y comprueba que la referencia correspondiente desaparece mientras el resto del inorden permanece ascendente.

**Archivo para construir:** `T04_EliminarHoja.java`; parte de la misma estructura de `EJ04_EliminarHoja.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T04_EliminarHoja` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ05 — Eliminar un nodo con un hijo

## Objetivo técnico — qué vamos a hacer y aprenderemos

Eliminaremos un nodo que tiene exactamente un hijo, un caso distinto de borrar una hoja porque existe un subárbol que debe mantenerse conectado. Construiremos un BST donde 10 tiene como único hijo derecho a 11 y usaremos la rama de eliminación que devuelve ese hijo al padre. Después compararemos las salidas antes y después para verificar que 10 desaparece, 11 permanece y las demás relaciones de búsqueda se conservan. El objetivo no es memorizar una condición, sino explicar por qué devolver la referencia del hijo sustituye correctamente al nodo eliminado.

## Qué problema concreto resolveremos

Eliminar directamente el nodo 10 sin reconectar a 11 provocaría que se perdiera una parte válida del árbol.

**Archivo que crearás:** `EJ05_EliminarUnHijo.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Preparar el mismo BST y reutilizar inserción

**Antes de escribir — qué necesitamos y por qué:** En este archivo independiente escribe `Nodo` e `insertar` exactamente como se muestran en el código completo. `Nodo` conserva valor y dos enlaces; `insertar` desciende a izquierda/derecha y devuelve la raíz actualizada. También incorpora `inorden`, que recorrerá el árbol antes y después para que podamos observar la modificación.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo insertar(Nodo n, int v) {
    if (n == null) return new Nodo(v);
    if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
    else if (v > n.valor) n.derecha = insertar(n.derecha, v);
    return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Buscar el nodo objetivo sin romper enlaces

**Antes de escribir — qué necesitamos y por qué:** Crea `eliminar` con retorno `Nodo` después de `insertar`. Cuando `n == null`, el dato no está en esa rama. Las comparaciones `v < n.valor` y `v > n.valor` guían la búsqueda; al reasignar `n.izquierda` o `n.derecha` conservamos el resultado de la llamada recursiva. El `else` significa que hemos encontrado exactamente el nodo a eliminar.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo eliminar(Nodo n, int v) {
    if (n == null) return null;
    if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
    else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
    else {
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Resolver los casos cero o un hijo mediante retorno

**Antes de escribir — qué necesitamos y por qué:** Dentro del `else` primero comprueba si falta el hijo izquierdo: retorna el derecho, que puede ser `null` cuando es una hoja. Si existe el izquierdo pero falta el derecho, retorna el izquierdo. De este modo, el padre recibe directamente la referencia al hijo que permanece, o `null` si ya no hay ninguno.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
if (n.izquierda == null) return n.derecha;
if (n.derecha == null) return n.izquierda;
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Conservar el caso de dos hijos con sucesor inorden

**Antes de escribir — qué necesitamos y por qué:** Solo si ambos hijos existen, comienza en `n.derecha`, desciende a la izquierda hasta hallar el menor valor de ese subárbol (sucesor), copia su valor al nodo encontrado y elimina la copia de la rama derecha. Estas líneas completan la implementación general y se activan únicamente cuando el nodo eliminado tiene dos hijos.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo sucesor = n.derecha;
while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
n.valor = sucesor.valor;
n.derecha = eliminar(n.derecha, sucesor.valor);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 5 — Cerrar el método y conservar la nueva raíz

**Antes de escribir — qué necesitamos y por qué:** Después del cierre de `else`, devuelve `n` y cierra `eliminar`. El retorno es importante también en `main`: una eliminación puede reemplazar la propia raíz del árbol. Por eso la llamada correcta es `raiz = eliminar(raiz, objetivo)`.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 6 — Ejecutar la operación concreta y comprobar el antes/después

**Antes de escribir — qué necesitamos y por qué:** En `main` inserta los valores [8, 4, 12, 2, 6, 10, 14, 11] y muestra el inorden antes de borrar. Invoca `raiz = eliminar(raiz, 10)`: el árbol resultante puede cambiar su raíz y por eso guardamos el retorno. Repite el inorden y compara el conjunto de valores sin confundir la visita ascendente con un dibujo de enlaces.

**Dónde escribirlo:** abre `EJ05_EliminarUnHijo.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo raiz = null;
for (int v : new int[]{8, 4, 12, 2, 6, 10, 14, 11}) raiz = insertar(raiz, v);
System.out.print("antes: "); inorden(raiz); System.out.println();
raiz = eliminar(raiz, 10);
System.out.print("despues: "); inorden(raiz); System.out.println();
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ05_EliminarUnHijo` coincida con `EJ05_EliminarUnHijo.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ05

```java
public class EJ05_EliminarUnHijo {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static Nodo insertar(Nodo n, int v) {
        if (n == null) return new Nodo(v);
        if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
        else if (v > n.valor) n.derecha = insertar(n.derecha, v);
        return n;
    }
    static Nodo eliminar(Nodo n, int v) {
        if (n == null) return null;
        if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
        else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
        else {
            if (n.izquierda == null) return n.derecha;
            if (n.derecha == null) return n.izquierda;
            Nodo sucesor = n.derecha;
            while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
            n.valor = sucesor.valor;
            n.derecha = eliminar(n.derecha, sucesor.valor);
        }
        return n;
    }
    static void inorden(Nodo n) {
        if (n == null) return;
        inorden(n.izquierda);
        System.out.print(n.valor + " ");
        inorden(n.derecha);
    }
    public static void main(String[] args) {
        Nodo raiz = null;
        for (int v : new int[]{8, 4, 12, 2, 6, 10, 14, 11}) raiz = insertar(raiz, v);
        System.out.print("antes: "); inorden(raiz); System.out.println();
        raiz = eliminar(raiz, 10);
        System.out.print("despues: "); inorden(raiz); System.out.println();
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ05_EliminarUnHijo.java
java EJ05_EliminarUnHijo
```

**Resultado esperado:**

```text
antes: 2 4 6 8 10 11 12 14 
despues: 2 4 6 8 11 12 14
```


**Interpretación:** El padre no necesita conocer todos los descendientes: conserva el subárbol que retorna la llamada de eliminación.

**Variación guiada:** Prueba a eliminar un nodo con un único hijo izquierdo y explica qué `return` se ejecuta.

**Error o caso límite controlado:** Si retornas `null` cuando existe un hijo, se pierde el subárbol completo; debes retornar la referencia al hijo sobreviviente.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T05 — Tarea espejo de EJ05

**Enunciado:** En `T05_EliminarUnHijo.java`, elimina 15: su único hijo izquierdo, 12, debe ocupar su lugar en el enlace que sale de 10.

**Archivo para construir:** `T05_EliminarUnHijo.java`; parte de la misma estructura de `EJ05_EliminarUnHijo.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T05_EliminarUnHijo` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ06 — Eliminar un nodo con dos hijos usando sucesor

## Objetivo técnico — qué vamos a hacer y aprenderemos

En este ejercicio eliminaremos una raíz con dos hijos, el caso que exige una decisión adicional. No podemos devolver uno de los hijos sin considerar el otro, porque se perdería parte de la estructura. Por eso localizaremos el sucesor inorden, es decir, el valor mínimo del subárbol derecho; copiaremos ese valor en la raíz y eliminaremos la ocurrencia original del sucesor. A continuación imprimiremos el inorden para demostrar que el valor 8 desapareció y los restantes permanecen ordenados. Diferenciaremos explícitamente este método de la alternativa con predecesor, que busca el máximo del subárbol izquierdo.

## Qué problema concreto resolveremos

Una raíz con dos hijos no se puede sustituir indiscriminadamente por una rama; hay que preservar el orden del BST y ambas subestructuras.

**Archivo que crearás:** `EJ06_EliminarDosHijos.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Preparar el mismo BST y reutilizar inserción

**Antes de escribir — qué necesitamos y por qué:** En este archivo independiente escribe `Nodo` e `insertar` exactamente como se muestran en el código completo. `Nodo` conserva valor y dos enlaces; `insertar` desciende a izquierda/derecha y devuelve la raíz actualizada. También incorpora `inorden`, que recorrerá el árbol antes y después para que podamos observar la modificación.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo insertar(Nodo n, int v) {
    if (n == null) return new Nodo(v);
    if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
    else if (v > n.valor) n.derecha = insertar(n.derecha, v);
    return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Buscar el nodo objetivo sin romper enlaces

**Antes de escribir — qué necesitamos y por qué:** Crea `eliminar` con retorno `Nodo` después de `insertar`. Cuando `n == null`, el dato no está en esa rama. Las comparaciones `v < n.valor` y `v > n.valor` guían la búsqueda; al reasignar `n.izquierda` o `n.derecha` conservamos el resultado de la llamada recursiva. El `else` significa que hemos encontrado exactamente el nodo a eliminar.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static Nodo eliminar(Nodo n, int v) {
    if (n == null) return null;
    if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
    else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
    else {
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Resolver los casos cero o un hijo mediante retorno

**Antes de escribir — qué necesitamos y por qué:** Dentro del `else` primero comprueba si falta el hijo izquierdo: retorna el derecho, que puede ser `null` cuando es una hoja. Si existe el izquierdo pero falta el derecho, retorna el izquierdo. De este modo, el padre recibe directamente la referencia al hijo que permanece, o `null` si ya no hay ninguno.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
if (n.izquierda == null) return n.derecha;
if (n.derecha == null) return n.izquierda;
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Conservar el caso de dos hijos con sucesor inorden

**Antes de escribir — qué necesitamos y por qué:** Solo si ambos hijos existen, comienza en `n.derecha`, desciende a la izquierda hasta hallar el menor valor de ese subárbol (sucesor), copia su valor al nodo encontrado y elimina la copia de la rama derecha. Estas líneas completan la implementación general y se activan únicamente cuando el nodo eliminado tiene dos hijos.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo sucesor = n.derecha;
while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
n.valor = sucesor.valor;
n.derecha = eliminar(n.derecha, sucesor.valor);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 5 — Cerrar el método y conservar la nueva raíz

**Antes de escribir — qué necesitamos y por qué:** Después del cierre de `else`, devuelve `n` y cierra `eliminar`. El retorno es importante también en `main`: una eliminación puede reemplazar la propia raíz del árbol. Por eso la llamada correcta es `raiz = eliminar(raiz, objetivo)`.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
return n;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 6 — Ejecutar la operación concreta y comprobar el antes/después

**Antes de escribir — qué necesitamos y por qué:** En `main` inserta los valores [8, 4, 12, 2, 6, 10, 14] y muestra el inorden antes de borrar. Invoca `raiz = eliminar(raiz, 8)`: el árbol resultante puede cambiar su raíz y por eso guardamos el retorno. Repite el inorden y compara el conjunto de valores sin confundir la visita ascendente con un dibujo de enlaces.

**Dónde escribirlo:** abre `EJ06_EliminarDosHijos.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
Nodo raiz = null;
for (int v : new int[]{8, 4, 12, 2, 6, 10, 14}) raiz = insertar(raiz, v);
System.out.print("antes: "); inorden(raiz); System.out.println();
raiz = eliminar(raiz, 8);
System.out.print("despues: "); inorden(raiz); System.out.println();
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ06_EliminarDosHijos` coincida con `EJ06_EliminarDosHijos.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ06

```java
public class EJ06_EliminarDosHijos {
    static class Nodo {
        int valor;
        Nodo izquierda, derecha;
        Nodo(int valor) { this.valor = valor; }
    }
    static Nodo insertar(Nodo n, int v) {
        if (n == null) return new Nodo(v);
        if (v < n.valor) n.izquierda = insertar(n.izquierda, v);
        else if (v > n.valor) n.derecha = insertar(n.derecha, v);
        return n;
    }
    static Nodo eliminar(Nodo n, int v) {
        if (n == null) return null;
        if (v < n.valor) n.izquierda = eliminar(n.izquierda, v);
        else if (v > n.valor) n.derecha = eliminar(n.derecha, v);
        else {
            if (n.izquierda == null) return n.derecha;
            if (n.derecha == null) return n.izquierda;
            Nodo sucesor = n.derecha;
            while (sucesor.izquierda != null) sucesor = sucesor.izquierda;
            n.valor = sucesor.valor;
            n.derecha = eliminar(n.derecha, sucesor.valor);
        }
        return n;
    }
    static void inorden(Nodo n) {
        if (n == null) return;
        inorden(n.izquierda);
        System.out.print(n.valor + " ");
        inorden(n.derecha);
    }
    public static void main(String[] args) {
        Nodo raiz = null;
        for (int v : new int[]{8, 4, 12, 2, 6, 10, 14}) raiz = insertar(raiz, v);
        System.out.print("antes: "); inorden(raiz); System.out.println();
        raiz = eliminar(raiz, 8);
        System.out.print("despues: "); inorden(raiz); System.out.println();
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ06_EliminarDosHijos.java
java EJ06_EliminarDosHijos
```

**Resultado esperado:**

```text
antes: 2 4 6 8 10 12 14 
despues: 2 4 6 10 12 14
```


**Interpretación:** El sucesor es el menor valor de la rama derecha. Tras copiarlo y borrar su nodo original, se conserva el inorden ascendente.

**Variación guiada:** Repite con el predecesor de la raíz (máximo del subárbol izquierdo) sobre papel; no mezcles ambos procedimientos en una sola ejecución.

**Error o caso límite controlado:** Copiar el sucesor sin eliminar su aparición original produce un valor duplicado; el segundo `eliminar` es obligatorio.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T06 — Tarea espejo de EJ06

**Enunciado:** En `T06_EliminarDosHijos.java`, elimina la raíz 20 usando el sucesor 25 y explica por qué 25 deja de aparecer en su ubicación anterior.

**Archivo para construir:** `T06_EliminarDosHijos.java`; parte de la misma estructura de `EJ06_EliminarDosHijos.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T06_EliminarDosHijos` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# EJ07 — Clasificar con árbol de decisión y medir entropía

## Objetivo técnico — qué vamos a hacer y aprenderemos

Para cerrar, diferenciaremos dos estructuras que comparten forma pero no propósito: el BST organiza valores para recorrerlos y buscarlos; un árbol de decisión representa condiciones que conducen a una clasificación. Implementaremos una regla didáctica que considera nota y asistencia, y además calcularemos la entropía inicial de cuatro registros ficticios (tres aprobados y uno desaprobado). Calcularemos la impureza después de dividir por nota y su ganancia de información. Verificaremos tanto las salidas numéricas como tres clasificaciones. La regla ilustrativa del código no se presenta como un modelo entrenado automáticamente: la selección del atributo en un modelo real necesita comparar los candidatos con datos.

## Qué problema concreto resolveremos

Una ruta de condiciones puede producir una clase, pero necesitamos distinguir la regla usada para clasificar de la medida estadística que sirve para elegir divisiones.

**Archivo que crearás:** `EJ07_ArbolDecision.java`. **Punto de entrada:** `public static void main(String[] args)`.

**Antes de construir:** dibuja los enlaces que esperas; cuando el ejercicio trate de eliminación, identifica si el nodo objetivo tiene cero, uno o dos hijos.

### Paso 1 — Calcular proporciones sin truncar decimales

**Antes de escribir — qué necesitamos y por qué:** En `EJ07_ArbolDecision.java`, escribe el método `entropia`. `total` reúne dos clases y el caso `total == 0` impide dividir entre cero. Convierte explícitamente a `double` antes de dividir: de otro modo, una división entre enteros perdería la fracción.

**Dónde escribirlo:** abre `EJ07_ArbolDecision.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static double entropia(int positivos, int negativos) {
    int total = positivos + negativos;
    if (total == 0) return 0.0;
    double p = (double) positivos / total;
    double q = (double) negativos / total;
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 2 — Calcular los términos de entropía en base dos

**Antes de escribir — qué necesitamos y por qué:** Agrega las dos contribuciones `-p log2(p)` y `-q log2(q)`. Java dispone de `Math.log` natural, por lo que dividirlo entre `Math.log(2)` transforma la base. El operador ternario evita evaluar logaritmo de cero. La suma mide impureza, no calificación de un estudiante.

**Dónde escribirlo:** abre `EJ07_ArbolDecision.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
double a = p == 0 ? 0 : -p * (Math.log(p) / Math.log(2));
double b = q == 0 ? 0 : -q * (Math.log(q) / Math.log(2));
return a + b;
}
```

**Qué ha cambiado y cómo lo comprobamos:** Hemos definido una nueva pieza de estructura o comportamiento. Comprueba que los nombres y referencias del fragmento corresponden al árbol que estamos construyendo.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 3 — Representar las decisiones como condiciones explícitas

**Antes de escribir — qué necesitamos y por qué:** Debajo de la función estadística agrega `clasificar`. `notaAlta` y `asistenciaAlta` son atributos booleanos ficticios; primero se pregunta por la nota y, si es baja, por la asistencia. Cada `return` es una hoja lógica de clasificación y termina la función. Las reglas son un ejemplo explicativo, no una predicción garantizada.

**Dónde escribirlo:** abre `EJ07_ArbolDecision.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
static String clasificar(boolean notaAlta, boolean asistenciaAlta) {
    if (notaAlta) return "APRUEBA";
    if (asistenciaAlta) return "APRUEBA";
    return "DESAPRUEBA";
}
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 4 — Calcular ganancia de información con grupos ponderados

**Antes de escribir — qué necesitamos y por qué:** En `main` el conjunto inicial contiene 3 positivos y 1 negativo. Para la división por nota, el grupo nota alta contiene 2 aprobados y el grupo nota baja contiene 1 aprobado y 1 desaprobado. Como ambos grupos contienen dos de cuatro registros, sus pesos son 0.5. `inicial - despuesNota` calcula la ganancia: reducir incertidumbre después de dividir.

**Dónde escribirlo:** abre `EJ07_ArbolDecision.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
double inicial = entropia(3, 1);
double despuesNota = 0.5 * entropia(2, 0) + 0.5 * entropia(1, 1);
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

### Paso 5 — Mostrar medidas y tres decisiones verificables

**Antes de escribir — qué necesitamos y por qué:** Al final de `main` utiliza `printf` con `%.3f` para mostrar tres decimales y `%n` para salto de línea. Prueba alta/baja, baja/alta y baja/baja. Las tres cadenas dependen solo de la rama que recorre cada combinación.

**Dónde escribirlo:** abre `EJ07_ArbolDecision.java`. Coloca este fragmento dentro de la clase pública en la ubicación indicada, respetando la estructura acumulada: las declaraciones de clases anidadas y métodos van fuera de `main`, mientras que las operaciones de prueba van dentro de `main`. Si el fragmento empieza o termina en mitad de un método, conserva sus llaves de apertura y cierre conforme a la versión consolidada que aparece al final.

**Escribe ahora:**

```java
System.out.printf("H inicial=%.3f%n", inicial);
System.out.printf("Ganancia nota=%.3f%n", inicial - despuesNota);
System.out.println("alta/baja: " + clasificar(true, false));
System.out.println("baja/alta: " + clasificar(false, true));
System.out.println("baja/baja: " + clasificar(false, false));
```

**Qué ha cambiado y cómo lo comprobamos:** Ahora hay una nueva parte que encaja con el programa y habilita la siguiente comprobación.

**Pregunta de predicción:** ¿qué condición hará que se ejecute esta parte y qué pasaría si la omitiéramos? Relaciona la respuesta con el problema concreto del ejemplo, no solo con la sintaxis.

## Auditoría de la construcción

Antes de compilar, verifica que `public class EJ07_ArbolDecision` coincida con `EJ07_ArbolDecision.java`, que cada `if` tenga el retorno o bloque previsto, que los métodos estén fuera de `main` y que las referencias `izquierda` y `derecha` conserven sus papeles. La consolidación siguiente incluye la estructura sintáctica que rodea los microbloques, los recorridos reutilizados y las instrucciones de impresión: no deben tratarse como piezas mágicas; léelas en el orden de ejecución y localiza las llamadas introducidas.

## Código completo al terminar EJ07

```java
public class EJ07_ArbolDecision {
    static double entropia(int positivos, int negativos) {
        int total = positivos + negativos;
        if (total == 0) return 0.0;
        double p = (double) positivos / total;
        double q = (double) negativos / total;
        double a = p == 0 ? 0 : -p * (Math.log(p) / Math.log(2));
        double b = q == 0 ? 0 : -q * (Math.log(q) / Math.log(2));
        return a + b;
    }
    static String clasificar(boolean notaAlta, boolean asistenciaAlta) {
        if (notaAlta) return "APRUEBA";
        if (asistenciaAlta) return "APRUEBA";
        return "DESAPRUEBA";
    }
    public static void main(String[] args) {
        double inicial = entropia(3, 1);
        double despuesNota = 0.5 * entropia(2, 0) + 0.5 * entropia(1, 1);
        System.out.printf("H inicial=%.3f%n", inicial);
        System.out.printf("Ganancia nota=%.3f%n", inicial - despuesNota);
        System.out.println("alta/baja: " + clasificar(true, false));
        System.out.println("baja/alta: " + clasificar(false, true));
        System.out.println("baja/baja: " + clasificar(false, false));
    }
}
```


## Antes de ejecutar: predicción

Anticipa qué información aparecerá, qué dato desaparecerá si hay eliminación y por qué las llamadas recursivas terminan. Comprueba manualmente el árbol o los registros de entrada antes de mirar la consola.

## Ejecuta y valida

```bash
javac EJ07_ArbolDecision.java
java EJ07_ArbolDecision
```

**Resultado esperado:**

```text
H inicial=0.811
Ganancia nota=0.311
alta/baja: APRUEBA
baja/alta: APRUEBA
baja/baja: DESAPRUEBA
```


**Interpretación:** La entropía mide impureza del conjunto y la ganancia mide su reducción tras una división; las hojas de la regla devuelven la clase.

**Variación guiada:** Modifica una condición de clasificación y explica por qué cambia la salida categórica, pero no la entropía calculada con registros fijos.

**Error o caso límite controlado:** No confundas una regla `if` escrita por el programador con el entrenamiento automático de un árbol de decisión; tampoco compares ganancias de atributos sin conocer la distribución de los datos.

**Qué debes poder explicar con tus palabras:** qué representa cada referencia, por qué las condiciones se evalúan en ese orden, qué devuelve el método y por qué la salida demuestra el objetivo.

## T07 — Tarea espejo de EJ07

**Enunciado:** En `T07_ArbolDecision.java` usa seis registros ficticios, cuatro aprobados y dos desaprobados. División por nota: grupo alto 3 aprobados, grupo bajo 1 aprobado y 2 desaprobados. Calcula entropía inicial, entropía ponderada y ganancia; comprueba además las rutas alta/alta y baja/baja.

**Archivo para construir:** `T07_ArbolDecision.java`; parte de la misma estructura de `EJ07_ArbolDecision.java` y cambia exactamente los valores o condiciones indicados en este enunciado. Reescribe `public class` con el nombre `T07_ArbolDecision` para poder compilarlo de manera independiente. **Restricción:** no agregar frameworks, no sustituir BST por listas ni usar bibliotecas de machine learning.

**Pista:** revisa cuál es la raíz, dónde se guardan los enlaces y cuál es el orden de ejecución del método que acabas de estudiar. **Predicción:** escribe qué saldrá antes de ejecutar.

**Evidencia:** código Java propio, salida de consola y explicación corta de la decisión más importante. **Criterio de validación:** deben coincidir el razonamiento, las relaciones y el resultado esperado; no basta con que aparezcan números en pantalla.


---

# Cierre docente de la sesión

Hoy construimos estructuras que se parecen en forma, pero que responden a preguntas diferentes. Cuando representamos una jerarquía, una referencia izquierda o derecha conserva una conexión real entre objetos; por eso distinguimos un nodo interno de una hoja y contamos las aristas para hablar de altura y profundidad. Después comprobamos que cambiar el momento de la visita altera preorden, inorden y postorden sin cambiar los nodos existentes. En un BST, el inorden resulta ascendente porque hemos respetado la regla de inserción, no porque el método por sí solo ordene cualquier árbol.

La parte más delicada fue la eliminación. En una hoja devolvimos una referencia nula; con un hijo devolvimos la referencia del subárbol sobreviviente; con dos hijos buscamos el sucesor y retiramos su posición anterior para evitar duplicados. Finalmente, en el árbol de decisión diferenciamos condiciones de clasificación y medidas de impureza: son ideas relacionadas con la representación arbórea, pero no equivalen a buscar valores en un BST. Repasa los retornos de los métodos recursivos y explica por escrito por qué conservan los enlaces. Repite una tarea sin mirar el ejemplo, predice el resultado y contrástalo con tu ejecución. Puedes ampliar tu aprendizaje con los materiales de **Lideratec Academy**, el blog https://lideratecacademy.com/blog/ y el canal https://www.youtube.com/@LideratecAcademy. La siguiente práctica podrá partir de estas habilidades para analizar con mayor seguridad estructuras y decisiones algorítmicas.
