---
course_id: 30710
session_id: S09
module_id: TEMA_09
source_origin: PPT
status: validated
---

# Pilas LIFO en Java — sesión 09

**Lideratec Academy** · Algoritmo y Estructura de Datos Basado en IA · Tema 09  
**Duración orientativa:** 120 minutos · **Lenguaje:** Java · **Nivel:** universitario inicial.  

## Propósito y problema de la clase

Piensa en una pila de platos: cuando colocas uno nuevo, lo dejas encima de los demás. Si quieres retirar un plato, primero debes sacar el que está arriba. En programación utilizamos esta misma idea mediante una estructura llamada **pila**. Su regla principal es **LIFO**, que significa *el último en entrar es el primero en salir*. Por eso, todas las inserciones y extracciones se realizan desde un único extremo, llamado **tope**.

En esta clase aprenderemos a construir esa estructura en Java de dos maneras. Primero utilizaremos un **vector**, donde los datos se guardan en posiciones numeradas y el tope se controla con un índice. Después trabajaremos con una **lista enlazada**, donde cada elemento está representado por un nodo y el tope se controla mediante una referencia. Aunque ambas implementaciones organizan los datos de forma diferente, respetan el mismo orden de salida.

Durante los ejercicios utilizaremos `push()` para agregar un elemento y `pop()` para retirarlo. También aprenderemos a reconocer cuándo una pila está llena o vacía, para evitar operaciones inválidas. Finalmente, aplicaremos lo aprendido a una versión sencilla del botón **Atrás** de un navegador. La meta no es solo conseguir que los programas funcionen: es comprender qué sucede con el tope en cada paso y poder explicarlo con nuestras propias palabras.

## Resultado de aprendizaje

Al terminar la clase, podrás **construir y probar una pila en Java** utilizando tanto un vector como una lista enlazada. Sabrás insertar y retirar elementos, identificar cuál se encuentra en el tope y mostrar cómo cambia la estructura después de cada operación. Por ejemplo, si agregamos 10, luego 20 y finalmente 30, podrás explicar por qué una llamada a `pop()` retira primero el 30.

También podrás comprobar cuándo una pila ya no admite más elementos o cuándo está vacía, sin alterar incorrectamente sus datos. Además, compararás el uso de un índice en un vector con el uso de referencias entre nodos y reconocerás el principio LIFO en una aplicación cotidiana, como el historial de navegación.

**¿Cómo demostrarás que lo aprendiste?** Construirás siete ejemplos guiados y resolverás sus siete tareas de práctica. No será suficiente mostrar una salida correcta: deberás explicar para qué se creó cada parte importante del programa, cuál era el tope antes y después de ejecutar una operación y cómo comprobaste que el resultado cumple la regla LIFO.

## Antes de comenzar

**Debes saber:** declarar clases, constructores, atributos y métodos Java; diferenciar enteros y referencias; usar `if`, `for`, `while`, `null` y `System.out`. De las sesiones anteriores se recupera el uso básico de vectores y listas enlazadas.

**Debes disponer de:** JDK con `javac` y `java` accesibles desde una terminal o un IDE Java que permita ejecutar un archivo de clase principal. El código usa solamente la biblioteca estándar de Java, sin Maven, dependencias externas ni frameworks. En esta generación se ejecutó con OpenJDK 21.

**Comprobación previa:** abre una terminal y escribe `java -version` y `javac -version`. Si uno de los dos comandos falla, revisa la instalación de JDK y la configuración de PATH antes del laboratorio. No necesitas una base de datos, servidor web ni extensión adicional.

### Crear carpetas y archivos desde cero

1. Crea la carpeta `Sesion09_Pilas` en tu espacio de trabajo.
2. Dentro, crea `EJ01`, `EJ02` ... `EJ07` como **carpetas independientes**. Es importante: varios ejemplos utilizan el mismo nombre de clase auxiliar `PilaVector`, `PilaLista` o `Nodo`, y no deben compilarse juntos en la misma carpeta.
3. En cada carpeta crea un archivo Java cuyo nombre se indica al comienzo del ejemplo. Por ejemplo: `Sesion09_Pilas/EJ01/EJ01_VectorPush.java`.
4. Sigue los microbloques en orden. En cada uno se explica **por qué se crea esa parte, dónde se coloca y cómo funciona**. Cuando una muestra empieza dentro de un método, primero escribe la cabecera de ese método que aparece en el código completo; añade todas sus llaves y líneas de cierre. Los fragmentos son extractos literales del archivo final y **no constituyen programas independientes**. Al terminar cada ejemplo, compara el resultado construido con el **Código Java completo** de la misma guía antes de compilar.
5. Para compilar desde la carpeta EJ01 usa `javac -encoding UTF-8 EJ01_VectorPush.java` y para ejecutar `java EJ01_VectorPush`. Repite el patrón con el nombre de cada clase pública. En IDE, abre la carpeta EJxx correspondiente y ejecuta la clase pública de ese archivo.
6. Para las tareas `Txx`, crea una copia del archivo EJxx en una carpeta distinta: cambia el `main` según el enunciado y comprueba la salida. No es necesario consultar otro material para obtener el código base.

**Convención importante:** la señal `-1` en los ejemplos de pila de enteros indica `pop()` sobre pila vacía. Es una solución didáctica de esta sesión; los datos de prueba serán no negativos. Si se permitiera guardar `-1` como dato, habría ambigüedad: no lo confundas con una excepción automática. Para el historial de páginas se usa `null` como señal de ausencia de página anterior.

### Cómo leer los pasos de construcción en voz alta

En cada paso primero se presenta una **necesidad concreta** de la pila, después se indica **dónde escribir** y se muestra el código exacto. La explicación que sigue al fragmento está redactada para leerse en clase mientras se escribe: identifica cada declaración nueva, la decisión técnica que representa, el cambio de estado y la forma de comprobarlo. Las preguntas breves permiten anticipar resultados. Al terminar, el bloque de **Código Java completo** reúne también las cabeceras, llaves y líneas auxiliares que no se repiten en todos los extractos. Este código debe coincidir con el laboratorio ejecutable y es la referencia para compilar.

## Ruta de clase sugerida (120 minutos)

| Tiempo | Actividad | Evidencia |
|---|---|---|
| 0–10 min | Recuperar LIFO, índice versus referencia | Diagrama correcto de tope |
| 10–25 min | EJ01 y EJ02: vector push/pop | Traza y salida comparada |
| 25–38 min | EJ03: `isFull()` y capacidad | Rechazo de inserción sin corrupción |
| 38–60 min | EJ04 y EJ05: nodos, enlaces y pop | Dibujo de referencias antes/después |
| 60–77 min | EJ06: botón Atrás | Estado visible e historial separados |
| 77–99 min | EJ07: equivalencia y límites | Comparación de operaciones idénticas |
| 99–113 min | Tareas espejo asignadas y depuración | Dos tareas explicadas por estudiante |
| 113–120 min | Síntesis, preguntas y evidencias | Respuestas razonadas |

**Nota didáctica:** los 7 ejemplos y las tareas están completamente resueltos como secuencia de aprendizaje; las tareas espejo pueden repartirse por equipos durante los 120 minutos y completarse individualmente como práctica autónoma.


---

# EJ01 — Construir una pila vectorial y apilar datos

**Objetivo técnico — qué aprenderemos y qué haremos:** En este primer ejercicio vamos a **crear una pila de tres posiciones utilizando un vector de enteros**. Aprenderemos para qué sirve la clase `PilaVector`, qué información guardan sus atributos y por qué el indicador `tope` comienza en `-1` cuando la pila está vacía. Después programaremos `push()` para introducir, en ese orden, los números 10, 20 y 30, y construiremos `mostrar()` para observarlos desde el elemento más reciente hasta el más antiguo. Al finalizar, comprobaremos que el tope contiene 30 y explicaremos por qué reservar espacio en el vector no significa que ese espacio ya tenga elementos apilados.

**Duración orientativa para estudio autónomo:** 13 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ01/EJ01_VectorPush.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Vamos a preparar un vector de tres posiciones. Aunque Java inicializa sus celdas numéricas con cero, eso **no significa** que la pila ya contenga tres elementos: al comenzar está vacía. Necesitamos una variable llamada `tope` que identifique la última posición realmente ocupada. Una vez creada, apilaremos 10, 20 y 30 y verificaremos que la lectura desde arriba devuelve 30, 20 y 10. La pregunta que resolveremos es: **¿cómo sabemos qué lugares del vector pertenecen de verdad a la pila?**

### Paso 2 — Representar capacidad y posición del tope

**Para explicar antes de escribir (lectura sugerida):** Antes de programar el primer método, necesitamos decidir qué información debe recordar una pila. No basta con guardar tres números: el programa también debe saber cuántos espacios tiene y cuál de esos espacios representa el último dato insertado. Por eso crearemos una clase llamada PilaVector, que reunirá los datos y las reglas de funcionamiento de esta estructura.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, al comienzo del archivo, abre la clase auxiliar PilaVector y escribe dentro de ella sus tres atributos; las declaraciones terminan antes del constructor. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class PilaVector {
    private int[] vector;
    private int tope;
    private int tamano;
```

**Explicación durante la escritura (puedes leerla al grupo):** Leamos las declaraciones una por una. int[] vector significa que guardaremos enteros en un arreglo. int tope es distinto: no contiene uno de los valores apilados, sino la posición donde se encuentra el último elemento válido. int tamano guarda la capacidad máxima que elegiremos al crear el objeto. private evita que otro código cambie estas tres variables directamente y desordene la pila. En este punto solo declaramos atributos; todavía no hemos reservado el arreglo ni agregado datos.

**Detalle técnico de referencia:** `vector` es el arreglo de enteros; `tamano` conserva su capacidad; `tope` no guarda un dato sino la posición del último dato insertado. Los tres son privados para que se modifiquen a través de operaciones de la pila.

**Comprobación del estado:** Antes de insertar no existe posición ocupada; aún falta inicializar los tres campos.

**Pregunta para comprobar la comprensión:** ¿Por qué no podríamos averiguar el tope contando los ceros del arreglo?

**Conexión con el siguiente paso:** Inicializar el almacenamiento y definir la pila vacía.

### Paso 3 — Inicializar pila vacía con capacidad tres

**Para explicar antes de escribir (lectura sugerida):** Ya sabemos qué datos va a recordar la clase, pero falta poner la pila en un estado inicial coherente. Al construir el objeto, elegiremos cuántas posiciones tendrá el vector. Aunque esos espacios existan en memoria, la pila todavía no contiene ningún número que haya ingresado mediante push. Vamos a representar esa diferencia desde el constructor.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, dentro de PilaVector, inmediatamente después de los atributos; conserva las llaves de apertura y cierre del constructor. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public PilaVector(int tamano) {
        this.tamano = tamano;
        vector = new int[tamano];
        tope = -1;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** El constructor PilaVector tiene el mismo nombre que la clase y se activa cuando escribimos new PilaVector(3). El parámetro tamano recibe el número tres; this.tamano se refiere al atributo y evita confundirlo con el parámetro. new int[tamano] reserva tres celdas, cuyos índices válidos serán cero, uno y dos. Finalmente asignamos tope = -1: como menos uno no es un índice válido del arreglo, lo usamos para indicar que todavía no existe elemento superior. Esta convención permitirá reconocer la pila vacía antes de intentar leer sus datos.

**Detalle técnico de referencia:** El constructor recibe la capacidad; `this.tamano` se refiere al atributo y `tamano` al parámetro. `new int[tamano]` reserva celdas, pero no apila ninguno de esos ceros iniciales. `tope=-1` significa que todavía no hay elementos válidos.

**Comprobación del estado:** Capacidad 3; índices 0,1,2; tope=-1; contenido lógico vacío.

**Pregunta para comprobar la comprensión:** Con capacidad tres y tope igual a menos uno, ¿cuántos elementos hay realmente apilados?

**Conexión con el siguiente paso:** Definir la inserción sin rebasar la capacidad.

### Paso 4 — Proteger push antes de modificar tope

**Para explicar antes de escribir (lectura sugerida):** Ahora necesitamos dar a la pila una operación para recibir números. La llamaremos push. Pero antes de escribir en el vector debemos resolver una pregunta: ¿qué ocurriría si intentáramos agregar un cuarto elemento a un arreglo de tres posiciones? La respuesta exige comprobar la capacidad antes de mover el tope.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, dentro de PilaVector, después del constructor, inicia el método push(int dato) y coloca la condición de pila llena al principio del método. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public void push(int dato) {
        if (tope == tamano - 1) {
            System.out.println("Error: la pila está llena");
            return;
        }
```

**Explicación durante la escritura (puedes leerla al grupo):** public void push(int dato) declara una operación que recibe un entero y no devuelve un resultado. La comparación tope == tamano - 1 identifica la última posición válida: en un vector de tres celdas, esa posición es dos. Si el tope ya está en dos, imprimimos un mensaje y return termina este intento de inserción. Observemos el orden: la condición se evalúa antes de incrementar el tope o tocar el arreglo; así, el rechazo no modifica ningún elemento existente.

**Detalle técnico de referencia:** `void` indica que la inserción no devuelve un dato; el parámetro `dato` es el valor recibido. Si `tope==tamano-1`, la última celda válida ya está ocupada; `return` cancela esta inserción antes de cambiar el estado.

**Comprobación del estado:** Cuando la pila está llena, tope y celdas conservan su valor.

**Pregunta para comprobar la comprensión:** Si tope vale dos y la capacidad es tres, ¿qué línea impide acceder a vector[3]?

**Conexión con el siguiente paso:** Programar la transición exitosa de push.

### Paso 5 — Mover el tope y guardar el elemento

**Para explicar antes de escribir (lectura sugerida):** El control anterior ya evita escribir fuera del vector. Ahora construiremos la parte que se ejecuta cuando sí queda espacio libre. Aquí el orden de dos instrucciones resulta decisivo: primero tenemos que avanzar al siguiente índice disponible y después depositar allí el valor recibido.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, dentro del mismo método push, inmediatamente después del if que termina con return; a continuación cierra el método. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
tope++;
        vector[tope] = dato;
        System.out.println("Apilado: " + dato);
```

**Explicación durante la escritura (puedes leerla al grupo):** La expresión tope++ incrementa el índice en una unidad: en la primera inserción pasa de menos uno a cero. La línea vector[tope] = dato guarda el número en esa posición; por eso push(10) coloca diez en vector[0]. Cuando repetimos push(20) y push(30), el tope cambia a uno y después a dos. El mensaje Apilado confirma el dato aceptado. Si invirtiéramos la asignación y el incremento, la primera inserción intentaría acceder a vector[-1], una posición inexistente.

**Detalle técnico de referencia:** `tope++` avanza de -1 a 0 en el primer push; luego la asignación `vector[tope]=dato` deposita el entero en la celda señalada. Invertir esas dos líneas intentaría usar el índice -1 en la primera inserción. El mensaje confirma qué dato se incorporó.

**Comprobación del estado:** Tras push(10): tope=0 y vector[0]=10. Tras push(20): tope=1; tras push(30): tope=2.

**Pregunta para comprobar la comprensión:** En la segunda llamada push, ¿qué valor tendrá tope justo antes de guardar veinte?

**Conexión con el siguiente paso:** Mostrar la secuencia desde cima hasta base.

### Paso 6 — Mostrar contenido y controlar el caso vacío

**Para explicar antes de escribir (lectura sugerida):** Nuestra pila ya guarda números, pero necesitamos hacer visible el resultado. No queremos imprimir las tres celdas por costumbre, porque alguna podría estar reservada pero no ocupada. Vamos a construir mostrar para leer solamente los elementos válidos, empezando por el que entró último.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, dentro de PilaVector, después de cerrar push; crea el método public void mostrar() y ubica este fragmento en su cuerpo, tal como aparece en el archivo completo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
if (tope == -1) {
            System.out.println("La pila está vacía");
            return;
        }
        System.out.print("Tope a base: ");
        for (int i = tope; i >= 0; i--) {
            System.out.print(vector[i] + " ");
        }
```

**Explicación durante la escritura (puedes leerla al grupo):** Dentro de mostrar comprobamos primero si tope == -1; en ese caso informamos que no hay nada que enseñar y salimos del método. Si hay elementos, imprimimos el rótulo Tope a base y abrimos un for cuyo índice i comienza exactamente en tope. La condición i >= 0 permite llegar hasta la base, y i-- hace que recorramos las posiciones en dirección descendente. Cada vector[i] imprime un elemento ya apilado. Así, si ingresaron diez, veinte y treinta, el recorrido enseña treinta, veinte y diez. El método completo también termina con un println para cerrar la línea de salida.

**Detalle técnico de referencia:** `tope==-1` comprueba ausencia de elementos. El ciclo comienza en el índice tope (el más reciente), termina cuando i queda por debajo de cero y decrementa `i--` en cada vuelta. `vector[i]` selecciona cada valor real, nunca una celda que todavía no se haya apilado.

**Comprobación del estado:** Con 10,20,30, se imprimen 30 20 10, aunque en memoria están en índices ascendentes.

**Pregunta para comprobar la comprensión:** ¿Por qué iniciamos el for en tope y no en vector.length - 1?

**Conexión con el siguiente paso:** Crear main para realizar una secuencia observable.

### Paso 7 — Ejecutar tres operaciones de inserción

**Para explicar antes de escribir (lectura sugerida):** Tenemos los atributos y métodos necesarios, pero todavía no los hemos utilizado desde un programa ejecutable. Vamos a crear la clase principal y su método main para probar un caso que podamos predecir antes de ejecutarlo: insertar diez, veinte y treinta en ese orden.

**Dónde escribirlo:** En `EJ01_VectorPush.java`, después de cerrar la clase PilaVector, crea la clase pública EJ01_VectorPush y escribe estas instrucciones dentro de main(String[] args). Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
PilaVector pila = new PilaVector(3);
        pila.push(10);
        pila.push(20);
        pila.push(30);
        pila.mostrar();
```

**Explicación durante la escritura (puedes leerla al grupo):** La instrucción new PilaVector(3) crea el objeto con tres espacios. Las llamadas pila.push(10), pila.push(20) y pila.push(30) activan el mismo método tres veces; cada una mueve el tope y guarda un entero. La instrucción pila.mostrar() no inserta ni elimina: solo recorre el contenido lógico, que al terminar tiene el número treinta arriba. Java comenzará la ejecución en main, declarado dentro de la clase pública EJ01_VectorPush, cuyo nombre coincide con el archivo.

**Detalle técnico de referencia:** `new PilaVector(3)` ejecuta el constructor. Cada llamada se ejecuta en orden, de modo que 30 llega al final y queda arriba. La instrucción `mostrar()` representa el estado sin cambiarlo. `main(String[] args)` es el punto de entrada de la aplicación Java.

**Comprobación del estado:** tope=2; salida de cima a base: 30 20 10.

**Pregunta para comprobar la comprensión:** Antes de ejecutar, ¿qué número aparecerá primero cuando mostrar lea de tope a base?

**Conexión con el siguiente paso:** Predecir el orden, compilar y contrastar.

### Traza manual — estado antes → operación → estado después

Estado: vacío (-1) → push(10): [10] → push(20): [10,20] → push(30): [10,20,30], tope=2; lectura descendente: 30,20,10.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class PilaVector {`:** Encapsula las operaciones de una pila cuyo almacenamiento es un vector de tamaño fijo.

- **Línea 2, `private int[] vector;`:** Campo privado: almacena las celdas físicas del vector de enteros.

- **Línea 3, `private int tope;`:** Índice privado del elemento superior; -1 representa pila vectorial vacía.

- **Línea 4, `private int tamano;`:** Guarda la capacidad máxima de la pila vectorial.

- **Línea 6, `public PilaVector(int tamano) {`:** Constructor: recibe la capacidad que determinará el tamaño del arreglo.

- **Línea 7, `this.tamano = tamano;`:** Distingue el atributo del parámetro: conserva la capacidad recibida.

- **Línea 8, `vector = new int[tamano];`:** Reserva exactamente tamano celdas; por sí solo no apila ningún elemento.

- **Línea 9, `tope = -1;`:** Fija estado lógico vacío: todavía no hay un índice ocupado.

- **Línea 12, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 13, `if (tope == tamano - 1) {`:** Guardia que reconoce pila llena antes de modificar su índice.

- **Línea 14, `System.out.println("Error: la pila está llena");`:** Explica al usuario que la inserción fue rechazada por falta de capacidad.

- **Línea 15, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 17, `tope++;`:** Incrementa el índice antes de usar la siguiente celda libre.

- **Línea 18, `vector[tope] = dato;`:** Escribe el nuevo entero exactamente en el índice indicado por tope.

- **Línea 19, `System.out.println("Apilado: " + dato);`:** Imprime el valor aceptado para que el estudiante valide cada push.

- **Línea 22, `public void mostrar() {`:** Proporciona una vista de los elementos sin alterar la pila.

- **Línea 23, `if (tope == -1) {`:** Detecta pila vectorial vacía antes de leer vector[-1].

- **Línea 24, `System.out.println("La pila está vacía");`:** Muestra el estado vacío si se consulta la vista vectorial.

- **Línea 25, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 27, `System.out.print("Tope a base: ");`:** Anuncia que la lectura comienza en el elemento superior.

- **Línea 28, `for (int i = tope; i >= 0; i--) {`:** Recorre desde el índice del tope hasta cero, descendiendo i una unidad en cada iteración.

- **Línea 29, `System.out.print(vector[i] + " ");`:** Lee el valor del índice ocupado actual y lo escribe sin salto de línea.

- **Línea 31, `System.out.println();`:** Cierra la salida actual con un salto de línea.

- **Línea 35, `public class EJ01_VectorPush {`:** Declara la clase pública ejecutable del EJ01; su nombre coincide con el archivo Java.

- **Línea 36, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 37, `PilaVector pila = new PilaVector(3);`:** Crea una instancia de la pila vectorial con la capacidad indicada.

- **Línea 38, `pila.push(10);`:** Inserta 10; cambia el tope según la implementación del EJ01.

- **Línea 39, `pila.push(20);`:** Inserta 20; cambia el tope según la implementación del EJ01.

- **Línea 40, `pila.push(30);`:** Inserta 30; cambia el tope según la implementación del EJ01.

- **Línea 41, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class PilaVector {
    private int[] vector;
    private int tope;
    private int tamano;

    public PilaVector(int tamano) {
        this.tamano = tamano;
        vector = new int[tamano];
        tope = -1;
    }

    public void push(int dato) {
        if (tope == tamano - 1) {
            System.out.println("Error: la pila está llena");
            return;
        }
        tope++;
        vector[tope] = dato;
        System.out.println("Apilado: " + dato);
    }

    public void mostrar() {
        if (tope == -1) {
            System.out.println("La pila está vacía");
            return;
        }
        System.out.print("Tope a base: ");
        for (int i = tope; i >= 0; i--) {
            System.out.print(vector[i] + " ");
        }
        System.out.println();
    }
}

public class EJ01_VectorPush {
    public static void main(String[] args) {
        PilaVector pila = new PilaVector(3);
        pila.push(10);
        pila.push(20);
        pila.push(30);
        pila.mostrar();
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ01` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ01_VectorPush.java
java EJ01_VectorPush
```

**Resultado esperado en consola:**

```text
Apilado: 10
Apilado: 20
Apilado: 30
Tope a base: 30 20 10
```

**Interpretación:** Estado: vacío (-1) → push(10): [10] → push(20): [10,20] → push(30): [10,20,30], tope=2; lectura descendente: 30,20,10. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Sustituye 30 por 35 y predice el tope; después intenta un cuarto push para ver cómo se preservan los tres valores.

### Error o caso límite y corrección razonada

Si retiras el guardia del método push e insertas un cuarto dato, aparece una indexación fuera de rango. Conserva el if con return antes de incrementar.


### Pregunta de comprensión

¿Por qué una celda reservada con valor 0 no equivale a un dato apilado?


## T01 — Tarea espejo de EJ01

**Enunciado:** Cambia las tres inserciones para apilar 7,14,21 manteniendo capacidad 3; representa el tope y explica por qué el método mostrar invierte la lectura.

**Archivo a modificar:** crea una copia de `EJ01_VectorPush.java` en la carpeta `T01`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Cambia únicamente los argumentos de `push`; no alteres el orden de `tope++` y la asignación.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Apilado: 7
Apilado: 14
Apilado: 21
Tope a base: 21 14 7
```


---

# EJ02 — Extraer con pop y validar pila vacía

**Objetivo técnico — qué aprenderemos y qué haremos:** Ahora vamos a **añadir la operación `pop()` a una pila construida con vector**. Primero recuperaremos una pila que contiene 10, 20 y 30; después retiraremos el valor del tope y comprobaremos que el método devuelve 30, mientras que 20 pasa a ser el nuevo elemento superior. También introduciremos una condición para impedir que se intente extraer un dato cuando la pila ya está vacía. Aprenderemos por qué es necesario guardar el valor retirado **antes** de disminuir el índice del tope y por qué un dato que permanezca físicamente en una celda puede haber dejado de formar parte de la pila.

**Duración orientativa para estudio autónomo:** 14 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ02/EJ02_VectorPop.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

En el ejercicio anterior sabíamos agregar valores, pero todavía no podíamos retirarlos. Ahora necesitamos que `pop()` entregue el último número apilado y actualice correctamente el tope. Si la pila contiene 10, 20 y 30, el dato retirado debe ser 30 y el nuevo tope debe apuntar a 20. También responderemos una situación importante: **¿qué debe hacer el programa si intentamos retirar un elemento cuando no queda ninguno?** Antes de leer el vector, comprobaremos si la pila está vacía.

### Paso 2 — Conservar el almacenamiento de EJ01

**Para explicar antes de escribir (lectura sugerida):** En el ejercicio anterior aprendimos a ingresar datos con push. Ahora queremos extraer el último sin perder los restantes. Para que la comparación sea justa, reutilizaremos la misma idea de almacenamiento: vector, tope y tamaño. El código de este ejemplo es un archivo independiente; repetimos aquí las piezas necesarias, no dependemos del programa anterior.

**Dónde escribirlo:** En `EJ02_VectorPop.java`, dentro de la clase auxiliar PilaVector del archivo EJ02_VectorPop.java, en sus primeras declaraciones; completa constructor y push con las líneas que aparecen en el código final. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
private int[] vector;
    private int tope;
    private int tamano;
```

**Explicación durante la escritura (puedes leerla al grupo):** El arreglo vector guarda los valores aceptados. El entero tope apunta al índice del último, y tamano conserva cuántas posiciones se reservaron. El constructor y push que aparecen en el archivo completo conservan el comportamiento estudiado: apilar diez, veinte y treinta deja tope en dos. Prepararemos una nueva operación pop, pero no cambiaremos la forma de almacenar ni la secuencia de inserción.

**Detalle técnico de referencia:** Estos atributos repiten la arquitectura construida en EJ01: el vector contiene las celdas, tope señala el último dato válido y tamano limita la capacidad. No se reescribe el mecanismo de push; se amplía su comportamiento con la operación inversa.

**Comprobación del estado:** Después de apilar 10,20,30: tope=2 y vector[2]=30.

**Pregunta para comprobar la comprensión:** Si push deja tope en dos, ¿qué posición deberá consultar la primera extracción?

**Conexión con el siguiente paso:** Validar si existe un dato antes de intentar leerlo.

### Paso 3 — Introducir pop con una salida definida

**Para explicar antes de escribir (lectura sugerida):** Necesitamos crear pop para retirar el dato situado arriba y entregarlo al programa que lo pidió. A diferencia de push, esta operación sí devuelve un número. Sin embargo, antes de consultar el vector debemos descartar el caso en que la pila no contiene elementos.

**Dónde escribirlo:** En `EJ02_VectorPop.java`, dentro de PilaVector, después de cerrar push y antes de mostrar; empieza el método public int pop() con la validación indicada. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public int pop() {
        if (tope == -1) {
            System.out.println("Error: la pila está vacía");
            return -1;
        }
```

**Explicación durante la escritura (puedes leerla al grupo):** public int pop() significa que el método devolverá un entero. La primera decisión, if (tope == -1), verifica si estamos ante una pila vacía. Cuando eso ocurre, mostramos un mensaje y devolvemos menos uno como señal didáctica de error. Es importante no confundir ese menos uno con un elemento real: en esta clase usamos valores no negativos para que la señal sea inequívoca. La comprobación evita leer vector[-1], lo que sería un acceso inválido.

**Detalle técnico de referencia:** `int` indica que pop devuelve un entero. La condición distingue la pila vacía antes de leer el vector; el -1 es una señal didáctica equivalente a la presentada en la sesión, no un valor extraído. Esta representación es ambigua si -1 fuera un dato permitido; en esta práctica se usan datos no negativos.

**Comprobación del estado:** Si tope=-1, no cambia ninguna celda ni el tope y el método finaliza.

**Pregunta para comprobar la comprensión:** Si aún no llamamos a push, ¿por qué pop debe terminar antes de acceder a vector[tope]?

**Conexión con el siguiente paso:** Rescatar el valor válido antes de mover el tope.

### Paso 4 — Guardar dato saliente antes de descender el tope

**Para explicar antes de escribir (lectura sugerida):** Ya protegimos la operación vacía; ahora resolveremos la extracción válida. Para devolver el dato correcto tenemos que leerlo mientras el índice todavía señala al elemento superior. Solo después moveremos el tope hacia la posición anterior.

**Dónde escribirlo:** En `EJ02_VectorPop.java`, dentro del mismo pop, a continuación de la validación de vacío y antes de la llave que cierra el método. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
int dato = vector[tope];
        tope--;
        return dato;
```

**Explicación durante la escritura (puedes leerla al grupo):** La asignación int dato = vector[tope] toma una copia del valor superior: si tope vale dos, dato será treinta. La instrucción tope-- mueve la cima lógica desde dos hasta uno. Luego return dato entrega treinta al lugar donde se llamó a pop. El número treinta podría seguir físicamente en vector[2], pero ya no es parte de la pila: los índices válidos llegan solo hasta tope. Si disminuyéramos el índice antes de guardar el dato, devolveríamos veinte y no treinta.

**Detalle técnico de referencia:** `vector[tope]` recupera el último dato insertado. La variable local `dato` lo conserva mientras `tope--` cambia la cima lógica a la celda anterior. `return dato` devuelve el retirado: 30. Invertir lectura y decremento devolvería 20 y rompería LIFO. La celda antigua puede conservar 30 físicamente; ya no forma parte del contenido lógico.

**Comprobación del estado:** Antes: tope=2 (30). Después: tope=1 (20); pop devuelve 30.

**Pregunta para comprobar la comprensión:** Después de devolver treinta, ¿qué índice y qué número representan el nuevo tope?

**Conexión con el siguiente paso:** Contrastar la devolución con la vista actual.

### Paso 5 — Imprimir sin alterar la pila

**Para explicar antes de escribir (lectura sugerida):** Queremos comprobar la operación sin fiarnos de que una sola línea se ejecutó. Por eso mostraremos la pila antes de retirar, imprimiremos el valor devuelto por pop y volveremos a mostrarla después. Es una prueba directa del orden LIFO.

**Dónde escribirlo:** En `EJ02_VectorPop.java`, dentro de main de EJ02_VectorPop, inmediatamente después de las tres llamadas push ya presentes en el código completo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
pila.mostrar();
        System.out.println("Retirado: " + pila.pop());
        pila.mostrar();
```

**Explicación durante la escritura (puedes leerla al grupo):** La primera llamada mostrar enseña treinta, veinte y diez. En la segunda instrucción, Java evalúa pila.pop() antes de completar el println: pop devuelve treinta y cambia el tope a uno. La tercera llamada mostrar recorre únicamente desde ese nuevo tope, por eso imprime veinte y diez. Si ambas vistas fueran iguales, tendríamos que revisar si pop realmente actualizó el índice.

**Detalle técnico de referencia:** La primera visualización permite verificar el tope 30; `pop()` ocurre dentro de println y devuelve 30; la segunda vista debe comenzar en 20. El método `mostrar()` no cambia el tope; solo lee los índices ocupados.

**Comprobación del estado:** Secuencia de visualización: 30 20 10 → retirado 30 → 20 10.

**Pregunta para comprobar la comprensión:** ¿Qué diferencia observamos entre lo que se imprime antes de pop y lo que se imprime después?

**Conexión con el siguiente paso:** Comprobar el caso límite por repetición en T02.

### Traza manual — estado antes → operación → estado después

Antes: [10,20,30], tope=2. pop(): dato=30 → tope=1 → [10,20]. La celda 2 puede contener 30 pero queda fuera de la pila lógica.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class PilaVector {`:** Encapsula las operaciones de una pila cuyo almacenamiento es un vector de tamaño fijo.

- **Línea 2, `private int[] vector;`:** Campo privado: almacena las celdas físicas del vector de enteros.

- **Línea 3, `private int tope;`:** Índice privado del elemento superior; -1 representa pila vectorial vacía.

- **Línea 4, `private int tamano;`:** Guarda la capacidad máxima de la pila vectorial.

- **Línea 6, `public PilaVector(int tamano) {`:** Constructor: recibe la capacidad que determinará el tamaño del arreglo.

- **Línea 7, `this.tamano = tamano;`:** Distingue el atributo del parámetro: conserva la capacidad recibida.

- **Línea 8, `vector = new int[tamano];`:** Reserva exactamente tamano celdas; por sí solo no apila ningún elemento.

- **Línea 9, `tope = -1;`:** Fija estado lógico vacío: todavía no hay un índice ocupado.

- **Línea 12, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 13, `if (tope == tamano - 1) {`:** Guardia que reconoce pila llena antes de modificar su índice.

- **Línea 14, `System.out.println("Error: la pila está llena");`:** Explica al usuario que la inserción fue rechazada por falta de capacidad.

- **Línea 15, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 17, `tope++;`:** Incrementa el índice antes de usar la siguiente celda libre.

- **Línea 18, `vector[tope] = dato;`:** Escribe el nuevo entero exactamente en el índice indicado por tope.

- **Línea 19, `System.out.println("Apilado: " + dato);`:** Imprime el valor aceptado para que el estudiante valide cada push.

- **Línea 22, `public int pop() {`:** Operación de extracción: retorna el entero de cima o la señal didáctica -1 si está vacía.

- **Línea 23, `if (tope == -1) {`:** Detecta pila vectorial vacía antes de leer vector[-1].

- **Línea 24, `System.out.println("Error: la pila está vacía");`:** Informa que se intentó extraer sin elementos válidos.

- **Línea 25, `return -1;`:** Devuelve el centinela -1; este laboratorio excluye -1 como dato válido para evitar ambigüedad.

- **Línea 27, `int dato = vector[tope];`:** Rescata el entero del tope actual ANTES de disminuir el índice.

- **Línea 28, `tope--;`:** Desciende la cima lógica una posición; no es necesario borrar la celda física.

- **Línea 29, `return dato;`:** Entrega al llamador el valor rescatado antes del cambio de tope.

- **Línea 32, `public void mostrar() {`:** Proporciona una vista de los elementos sin alterar la pila.

- **Línea 33, `if (tope == -1) {`:** Detecta pila vectorial vacía antes de leer vector[-1].

- **Línea 34, `System.out.println("La pila está vacía");`:** Muestra el estado vacío si se consulta la vista vectorial.

- **Línea 35, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 37, `System.out.print("Tope a base: ");`:** Anuncia que la lectura comienza en el elemento superior.

- **Línea 38, `for (int i = tope; i >= 0; i--) {`:** Recorre desde el índice del tope hasta cero, descendiendo i una unidad en cada iteración.

- **Línea 39, `System.out.print(vector[i] + " ");`:** Lee el valor del índice ocupado actual y lo escribe sin salto de línea.

- **Línea 41, `System.out.println();`:** Cierra la salida actual con un salto de línea.

- **Línea 45, `public class EJ02_VectorPop {`:** Declara la clase pública ejecutable del EJ02; su nombre coincide con el archivo Java.

- **Línea 46, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 47, `PilaVector pila = new PilaVector(3);`:** Crea una instancia de la pila vectorial con la capacidad indicada.

- **Línea 48, `pila.push(10);`:** Inserta 10; cambia el tope según la implementación del EJ02.

- **Línea 49, `pila.push(20);`:** Inserta 20; cambia el tope según la implementación del EJ02.

- **Línea 50, `pila.push(30);`:** Inserta 30; cambia el tope según la implementación del EJ02.

- **Línea 51, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.

- **Línea 52, `System.out.println("Retirado: " + pila.pop());`:** Extrae la cima del objeto pila, imprime el dato devuelto y actualiza su estado.

- **Línea 53, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class PilaVector {
    private int[] vector;
    private int tope;
    private int tamano;

    public PilaVector(int tamano) {
        this.tamano = tamano;
        vector = new int[tamano];
        tope = -1;
    }

    public void push(int dato) {
        if (tope == tamano - 1) {
            System.out.println("Error: la pila está llena");
            return;
        }
        tope++;
        vector[tope] = dato;
        System.out.println("Apilado: " + dato);
    }

    public int pop() {
        if (tope == -1) {
            System.out.println("Error: la pila está vacía");
            return -1;
        }
        int dato = vector[tope];
        tope--;
        return dato;
    }

    public void mostrar() {
        if (tope == -1) {
            System.out.println("La pila está vacía");
            return;
        }
        System.out.print("Tope a base: ");
        for (int i = tope; i >= 0; i--) {
            System.out.print(vector[i] + " ");
        }
        System.out.println();
    }
}

public class EJ02_VectorPop {
    public static void main(String[] args) {
        PilaVector pila = new PilaVector(3);
        pila.push(10);
        pila.push(20);
        pila.push(30);
        pila.mostrar();
        System.out.println("Retirado: " + pila.pop());
        pila.mostrar();
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ02` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ02_VectorPop.java
java EJ02_VectorPop
```

**Resultado esperado en consola:**

```text
Apilado: 10
Apilado: 20
Apilado: 30
Tope a base: 30 20 10 
Retirado: 30
Tope a base: 20 10
```

**Interpretación:** Antes: [10,20,30], tope=2. pop(): dato=30 → tope=1 → [10,20]. La celda 2 puede contener 30 pero queda fuera de la pila lógica. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Cambia la secuencia a 40,50,60; antes de ejecutar predice qué valor devuelve el primer pop y qué permanece visible.

### Error o caso límite y corrección razonada

No decrementar antes de rescatar `vector[tope]`; el error devolvería un dato diferente o usaría un índice inválido si el tope era cero.


### Pregunta de comprensión

¿Por qué conservar 30 en la celda física del vector no significa que siga apilado?


## T02 — Tarea espejo de EJ02

**Enunciado:** Realiza cuatro llamadas consecutivas a pop sobre tres elementos; diferencia los tres valores retirados de la señal -1 al consultar la pila vacía.

**Archivo a modificar:** crea una copia de `EJ02_VectorPop.java` en la carpeta `T02`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Agrega llamadas a pop después del primer mensaje; utiliza un rótulo distinto para el intento sobre la pila vacía.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Apilado: 10
Apilado: 20
Apilado: 30
Tope a base: 30 20 10 
Retirado: 30
Retirado: 20
Retirado: 10
Error: la pila está vacía
Retirado en vacío: -1
La pila está vacía
```


---

# EJ03 — Separar isFull y controlar capacidad

**Objetivo técnico — qué aprenderemos y qué haremos:** En este ejercicio vamos a **detectar cuándo una pila implementada con vector está llena**. Crearemos el método `isFull()`, cuya responsabilidad será responder `true` o `false` sin modificar los datos. Luego haremos que `push()` consulte ese resultado antes de insertar un nuevo valor. Con una pila de capacidad dos, agregaremos 5 y 10 e intentaremos agregar 15; comprobaremos que el tercer dato se rechaza y que los dos primeros permanecen intactos. Así entenderemos por qué conviene separar la pregunta «¿hay espacio?» de la acción «guardar el dato».

**Duración orientativa para estudio autónomo:** 13 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ03/EJ03_VectorIsFull.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Un vector tiene una cantidad fija de posiciones. Si creamos uno con capacidad dos, podemos guardar dos elementos, pero **no un tercero** mientras no liberemos un lugar. Necesitamos una comprobación que pueda utilizarse antes de cada inserción. Por eso construiremos `isFull()`, un método que solo responde si la pila está llena, y haremos que `push()` lo consulte. La situación que verificaremos es esta: **¿cómo rechazamos el tercer valor sin perder los dos que ya están almacenados?**

### Paso 2 — Recuperar el significado del índice tope

**Para explicar antes de escribir (lectura sugerida):** Hemos detectado que push no puede insertar cuando el vector está lleno. En este ejercicio queremos convertir esa comprobación en una consulta con nombre propio: isFull. Para ello necesitamos conservar el mismo arreglo, el índice tope y su capacidad, porque esas variables son precisamente las que permiten responder si queda espacio.

**Dónde escribirlo:** En `EJ03_VectorIsFull.java`, dentro de PilaVector, al comienzo de EJ03_VectorIsFull.java; el constructor se escribe después con el contenido del archivo final. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
private int[] vector;
    private int tope;
    private int tamano;
```

**Explicación durante la escritura (puedes leerla al grupo):** Cada atributo tiene una responsabilidad separada: vector conserva los números, tope identifica la cima lógica y tamano indica el máximo permitido. El constructor del archivo completo inicializa tope con menos uno y reserva la capacidad indicada. Con una pila de dos espacios, el último índice válido es uno; por eso, tras dos inserciones aceptadas, la consulta de lleno tendrá que responder verdadero.

**Detalle técnico de referencia:** Los atributos se mantienen iguales que EJ01. Con capacidad 2, los índices válidos son 0 y 1; cuando tope vale 1 ambas posiciones están ocupadas. La comprobación de capacidad no depende del valor de los enteros guardados.

**Comprobación del estado:** Pila recién creada: tope=-1 y capacidad 2.

**Pregunta para comprobar la comprensión:** Con capacidad dos y tope igual a cero, ¿la pila ya está llena?

**Conexión con el siguiente paso:** Extraer la condición de lleno a un método.

### Paso 3 — Construir isFull como pregunta booleana

**Para explicar antes de escribir (lectura sugerida):** Es útil separar una pregunta de una acción. En vez de repetir una comparación dentro de cualquier método que necesite saber si queda espacio, construiremos isFull. Este método no debe cambiar la pila; su responsabilidad es responder sí o no.

**Dónde escribirlo:** En `EJ03_VectorIsFull.java`, dentro de PilaVector, justo después del constructor y antes del método push. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public boolean isFull() {
        return tope == tamano - 1;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** El encabezado public boolean isFull() anuncia que obtendremos true o false. La condición tope == tamano - 1 es la respuesta completa: cuando el índice de la cima coincide con la última posición disponible, ya no queda lugar para otra inserción. return entrega ese resultado al código que hizo la consulta. Como aquí no incrementamos el índice ni escribimos en el arreglo, observar isFull no modifica el contenido.

**Detalle técnico de referencia:** `boolean` permite devolver true o false. La comparación no cambia el tope ni las celdas, solo responde si la última posición ya está ocupada. Para tamano=2 y tope=1, 1==1 es true.

**Comprobación del estado:** isFull() es false al inicio y true después de dos inserciones.

**Pregunta para comprobar la comprensión:** ¿Qué diferencia hay entre consultar isFull y ejecutar push?

**Conexión con el siguiente paso:** Hacer que push utilice esa misma comprobación.

### Paso 4 — Validar antes de cambiar el estado

**Para explicar antes de escribir (lectura sugerida):** Ahora que isFull sabe reconocer la capacidad máxima, utilizaremos esa respuesta dentro de push. La idea es que la comprobación se ejecute antes de alterar el tope: así un dato rechazado no desplaza a los ya guardados.

**Dónde escribirlo:** En `EJ03_VectorIsFull.java`, dentro del método push(int dato) de PilaVector: sustituye la condición de lleno escrita en EJ01 por la llamada a isFull(), y conserva la escritura del dato. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
if (isFull()) {
            System.out.println("Error: la pila está llena");
            return;
        }
        tope++;
        vector[tope] = dato;
```

**Explicación durante la escritura (puedes leerla al grupo):** La condición if (isFull()) llama al método booleano. Cuando responde true, imprimimos que la pila está llena y return termina push. Si responde false, ejecutamos tope++ y después vector[tope] = dato; esa secuencia ocupa exactamente la siguiente posición libre. Las líneas de prueba no deberían modificar el índice cuando rechazamos quince porque los valores cinco y diez ya ocupan las dos celdas.

**Detalle técnico de referencia:** Primero `isFull()` decide si la operación puede continuar. Si es true, `return` sale del método. Solo cuando es false se incrementa `tope` y luego se escribe el entero. Este orden conserva datos previos ante el intento inválido.

**Comprobación del estado:** Al intentar push(15) sobre [5,10], el tope permanece en 1 y el contenido es 10,5 de cima a base.

**Pregunta para comprobar la comprensión:** ¿Qué ocurriría si incrementáramos tope antes de preguntar isFull?

**Conexión con el siguiente paso:** Registrar evidencia pública de la consulta isFull.

### Paso 5 — Probar capacidad dos y observar el rechazo

**Para explicar antes de escribir (lectura sugerida):** Vamos a probar el límite con una capacidad pequeña para que el resultado sea evidente. Reservaremos dos espacios, insertaremos cinco y diez y, antes de intentar agregar quince, preguntaremos directamente si la pila está llena.

**Dónde escribirlo:** En `EJ03_VectorIsFull.java`, dentro de main de EJ03_VectorIsFull, después de crear la clase pública principal. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
PilaVector pila = new PilaVector(2);
        pila.push(5);
        pila.push(10);
        System.out.println("¿Llena?: " + pila.isFull());
        pila.push(15);
        pila.mostrar();
```

**Explicación durante la escritura (puedes leerla al grupo):** new PilaVector(2) fija la capacidad en dos. Después de push(5), tope vale cero; después de push(10), vale uno. La consulta pila.isFull() debe imprimir true. Cuando pedimos push(15), el método vuelve a comprobar la capacidad y rechaza la inserción antes de mover tope. Finalmente mostrar imprimirá diez y cinco, lo que confirma que el dato rechazado no sobrescribió ningún valor.

**Detalle técnico de referencia:** Las primeras dos llamadas ocupan posiciones 0 y 1. `println` muestra true sin alterar el arreglo; el tercer push muestra error sin guardar 15. El recorrido de `mostrar` comienza en el tope 10 y termina en la base 5.

**Comprobación del estado:** Salida final: ¿Llena?: true y Tope a base: 10 5.

**Pregunta para comprobar la comprensión:** Si quince fue rechazado, ¿cuál es el último valor que realmente entró en la pila?

**Conexión con el siguiente paso:** Modificar los datos sin cambiar capacidad para evidenciar T03.

### Traza manual — estado antes → operación → estado después

Capacidad=2: tope -1 → push(5): 0 → push(10):1; isFull() true; push(15) rechazado; tope sigue 1.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class PilaVector {`:** Encapsula las operaciones de una pila cuyo almacenamiento es un vector de tamaño fijo.

- **Línea 2, `private int[] vector;`:** Campo privado: almacena las celdas físicas del vector de enteros.

- **Línea 3, `private int tope;`:** Índice privado del elemento superior; -1 representa pila vectorial vacía.

- **Línea 4, `private int tamano;`:** Guarda la capacidad máxima de la pila vectorial.

- **Línea 6, `public PilaVector(int tamano) {`:** Constructor: recibe la capacidad que determinará el tamaño del arreglo.

- **Línea 7, `this.tamano = tamano;`:** Distingue el atributo del parámetro: conserva la capacidad recibida.

- **Línea 8, `vector = new int[tamano];`:** Reserva exactamente tamano celdas; por sí solo no apila ningún elemento.

- **Línea 9, `tope = -1;`:** Fija estado lógico vacío: todavía no hay un índice ocupado.

- **Línea 12, `public boolean isFull() {`:** Expone una consulta booleana sin cambiar la pila.

- **Línea 13, `return tope == tamano - 1;`:** Devuelve true solo si el último índice válido ya es el tope.

- **Línea 16, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 17, `if (isFull()) {`:** Invoca el método isFull para decidir si se cancela una inserción.

- **Línea 18, `System.out.println("Error: la pila está llena");`:** Explica al usuario que la inserción fue rechazada por falta de capacidad.

- **Línea 19, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 21, `tope++;`:** Incrementa el índice antes de usar la siguiente celda libre.

- **Línea 22, `vector[tope] = dato;`:** Escribe el nuevo entero exactamente en el índice indicado por tope.

- **Línea 23, `System.out.println("Apilado: " + dato);`:** Imprime el valor aceptado para que el estudiante valide cada push.

- **Línea 26, `public void mostrar() {`:** Proporciona una vista de los elementos sin alterar la pila.

- **Línea 27, `if (tope == -1) {`:** Detecta pila vectorial vacía antes de leer vector[-1].

- **Línea 28, `System.out.println("La pila está vacía");`:** Muestra el estado vacío si se consulta la vista vectorial.

- **Línea 29, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 31, `System.out.print("Tope a base: ");`:** Anuncia que la lectura comienza en el elemento superior.

- **Línea 32, `for (int i = tope; i >= 0; i--) {`:** Recorre desde el índice del tope hasta cero, descendiendo i una unidad en cada iteración.

- **Línea 33, `System.out.print(vector[i] + " ");`:** Lee el valor del índice ocupado actual y lo escribe sin salto de línea.

- **Línea 35, `System.out.println();`:** Cierra la salida actual con un salto de línea.

- **Línea 39, `public class EJ03_VectorIsFull {`:** Declara la clase pública ejecutable del EJ03; su nombre coincide con el archivo Java.

- **Línea 40, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 41, `PilaVector pila = new PilaVector(2);`:** Crea una instancia de la pila vectorial con la capacidad indicada.

- **Línea 42, `pila.push(5);`:** Inserta 5; cambia el tope según la implementación del EJ03.

- **Línea 43, `pila.push(10);`:** Inserta 10; cambia el tope según la implementación del EJ03.

- **Línea 44, `System.out.println("¿Llena?: " + pila.isFull());`:** Consulta e imprime true/false para detectar el estado de capacidad.

- **Línea 45, `pila.push(15);`:** Inserta 15; cambia el tope según la implementación del EJ03.

- **Línea 46, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class PilaVector {
    private int[] vector;
    private int tope;
    private int tamano;

    public PilaVector(int tamano) {
        this.tamano = tamano;
        vector = new int[tamano];
        tope = -1;
    }

    public boolean isFull() {
        return tope == tamano - 1;
    }

    public void push(int dato) {
        if (isFull()) {
            System.out.println("Error: la pila está llena");
            return;
        }
        tope++;
        vector[tope] = dato;
        System.out.println("Apilado: " + dato);
    }

    public void mostrar() {
        if (tope == -1) {
            System.out.println("La pila está vacía");
            return;
        }
        System.out.print("Tope a base: ");
        for (int i = tope; i >= 0; i--) {
            System.out.print(vector[i] + " ");
        }
        System.out.println();
    }
}

public class EJ03_VectorIsFull {
    public static void main(String[] args) {
        PilaVector pila = new PilaVector(2);
        pila.push(5);
        pila.push(10);
        System.out.println("¿Llena?: " + pila.isFull());
        pila.push(15);
        pila.mostrar();
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ03` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ03_VectorIsFull.java
java EJ03_VectorIsFull
```

**Resultado esperado en consola:**

```text
Apilado: 5
Apilado: 10
¿Llena?: true
Error: la pila está llena
Tope a base: 10 5
```

**Interpretación:** Capacidad=2: tope -1 → push(5): 0 → push(10):1; isFull() true; push(15) rechazado; tope sigue 1. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Imprime `isFull()` antes del primer push y entre las dos inserciones: explica cada transición false/false/true.

### Error o caso límite y corrección razonada

Un error frecuente es comparar tope==tamano (demasiado tarde). La última posición permitida es tamano-1, pues los índices empiezan en 0.


### Pregunta de comprensión

¿Qué distingue un método booleano observador de un método void que modifica el estado?


## T03 — Tarea espejo de EJ03

**Enunciado:** Mantén capacidad 2 y prueba push(6), push(12), isFull(), push(18); explica por qué 18 no se almacena.

**Archivo a modificar:** crea una copia de `EJ03_VectorIsFull.java` en la carpeta `T03`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Solo reemplaza los tres argumentos de inserción; no alteres las comparaciones.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Apilado: 6
Apilado: 12
¿Llena?: true
Error: la pila está llena
Tope a base: 12 6
```


---

# EJ04 — Apilar nodos con una lista enlazada

**Objetivo técnico — qué aprenderemos y qué haremos:** A partir de este ejercicio vamos a **construir una pila sin utilizar un vector**. Representaremos cada elemento mediante un objeto de la clase `Nodo`, que tendrá un dato y una referencia llamada `siguiente`. Crearemos también una clase que conserve la referencia `tope` y programaremos `push()` para insertar nodos nuevos sin perder los anteriores. Insertaremos 5, 10 y 15, observaremos los enlaces que se forman y comprobaremos que 15 queda arriba. Explicaremos por qué, en una lista enlazada vacía, el tope es `null` en lugar de `-1`, y por qué importa el orden de las asignaciones.

**Duración orientativa para estudio autónomo:** 17 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ04/EJ04_ListaPush.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Hasta ahora el tope era un índice de vector. En una pila enlazada ya no necesitamos ubicar los valores por posiciones numéricas: cada dato se guarda en un **nodo**, y cada nodo conoce el siguiente. Por eso crearemos la clase `Nodo` y una referencia que indique dónde está la cima. Partiremos de `tope = null`, agregaremos 5, 10 y 15 y comprobaremos que el último nodo creado queda arriba. El reto es **enlazar primero el nodo nuevo con la pila existente y solo después convertirlo en el nuevo tope**.

### Paso 2 — Crear Nodo porque ya no hay índices de vector

**Para explicar antes de escribir (lectura sugerida):** Hasta ahora guardamos cada elemento en una posición del vector. Pasaremos a una pila enlazada, donde no existen índices consecutivos para identificar los datos. Cada valor debe vivir en un objeto con una referencia al siguiente. Necesitamos una clase Nodo que represente justamente esa combinación.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, al principio de EJ04_ListaPush.java, crea la clase auxiliar Nodo, antes de la clase PilaLista. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class Nodo {
    int dato;
    Nodo siguiente;
```

**Explicación durante la escritura (puedes leerla al grupo):** La declaración class Nodo define el tipo de objeto que almacenará cada elemento. int dato guarda el entero del nodo. Nodo siguiente es una referencia a otro objeto del mismo tipo: no almacena el número que sigue, sino la dirección lógica de la conexión hacia ese nodo. Cuando llegue un nuevo elemento, lo enlazaremos con quien era el tope anterior, para mantener accesible el resto de la pila.

**Detalle técnico de referencia:** `Nodo` representa una unidad de almacenamiento. `dato` es el entero de ese nodo. `siguiente` es una referencia a otro Nodo; no almacena un entero ni es un índice. Sin el campo siguiente no podríamos recuperar la cadena de valores debajo del tope.

**Comprobación del estado:** Aún no hay objetos Nodo, solo su definición de estructura.

**Pregunta para comprobar la comprensión:** ¿Por qué cada Nodo necesita una referencia siguiente además de su dato?

**Conexión con el siguiente paso:** Definir cómo nace un nodo aislado.

### Paso 3 — Inicializar cada nodo sin referencia previa

**Para explicar antes de escribir (lectura sugerida):** Ya definimos qué forma tiene un nodo, pero cada vez que creemos uno necesitaremos asignarle un valor y establecer que todavía no apunta a otro elemento. Esa es la responsabilidad del constructor Nodo.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, dentro de la clase Nodo, después de los campos dato y siguiente; luego cierra la clase Nodo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public Nodo(int dato) {
        this.dato = dato;
        this.siguiente = null;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** El parámetro int dato recibe el número que queremos guardar. this.dato = dato lo copia al atributo del nuevo objeto. this.siguiente = null establece que todavía no tiene enlace hacia otro nodo. Más adelante, durante push, conectaremos esa referencia al antiguo tope. Si estamos creando el primer elemento de la pila, null seguirá siendo la terminación correcta de la cadena.

**Detalle técnico de referencia:** El constructor copia el parámetro en el campo mediante `this.dato`; `null` indica ausencia de siguiente nodo. El enlace se asignará en push, cuando conozcamos qué nodo era tope antes.

**Comprobación del estado:** new Nodo(5) produce dato=5, siguiente=null.

**Pregunta para comprobar la comprensión:** Si creamos Nodo(5) y todavía no lo apilamos, ¿hacia dónde apunta siguiente?

**Conexión con el siguiente paso:** Crear el objeto responsable de conservar el tope.

### Paso 4 — Definir PilaLista y su estado vacío

**Para explicar antes de escribir (lectura sugerida):** Una pila enlazada no necesita recordar el número de cada posición del vector. Necesita conservar una única puerta de entrada a los nodos: la referencia al que está arriba. Por eso crearemos PilaLista con un atributo llamado tope.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, después de cerrar Nodo, declara class PilaLista con su atributo tope y su constructor. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class PilaLista {
    private Nodo tope;

    public PilaLista() {
        tope = null;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** private Nodo tope significa que el tope es una referencia a un Nodo, no un índice entero. El constructor PilaLista() asigna null porque todavía no existe ningún nodo insertado. Esta decisión cumple la misma función lógica que tope = -1 en la pila vectorial, pero ahora expresada mediante una referencia ausente. Cuando hagamos push, esta variable comenzará a apuntar al nodo agregado.

**Detalle técnico de referencia:** `tope` es una referencia de tipo Nodo. Su valor `null` significa pila vacía. No existe un entero tope=-1 porque la posición ya no es un índice, sino una referencia al primer nodo de la cadena.

**Comprobación del estado:** Estado inicial: tope=null.

**Pregunta para comprobar la comprensión:** ¿Qué significa tope == null en esta implementación?

**Conexión con el siguiente paso:** Construir push cuidando el orden de enlaces.

### Paso 5 — Crear y enlazar el nodo antes de mover el tope

**Para explicar antes de escribir (lectura sugerida):** Ahora construiremos push con enlaces. Tenemos que conservar la cadena que ya existe y colocar el nodo nuevo delante de ella. El orden de asignación importa mucho: si cambiamos el tope demasiado pronto, podríamos perder la referencia que nos permite llegar a los nodos anteriores.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, dentro del método push(int dato) de PilaLista, después de escribir la cabecera del método; al final imprime Apilado y cierra push como muestra el código completo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = tope;
        tope = nuevo;
```

**Explicación durante la escritura (puedes leerla al grupo):** new Nodo(dato) crea un objeto con el número recibido. Con nuevo.siguiente = tope, el recién creado apunta al que estaba arriba; si la pila estaba vacía, apuntará a null. Recién después ejecutamos tope = nuevo, de modo que la pila empieza en este nodo y continúa por los anteriores. Por ejemplo, con cinco apilado, insertar diez produce tope → 10 → 5 → null. Observemos que el dato de cinco no se copió ni se desplazó: lo conservamos gracias al enlace.

**Detalle técnico de referencia:** Primero se crea un nuevo Nodo con el dato recibido. Segundo, `nuevo.siguiente=tope` preserva la dirección del tope anterior, que puede ser null. Tercero, `tope=nuevo` convierte al nuevo nodo en entrada principal. Invertir las dos asignaciones puede generar un enlace a sí mismo y perder la cadena anterior.

**Comprobación del estado:** Con 5 previo: tope 5→null; push(10): 10→5→null; push(15): 15→10→5→null.

**Pregunta para comprobar la comprensión:** ¿Qué cadena quedaría si enlazáramos diez con el cinco antes de cambiar tope?

**Conexión con el siguiente paso:** Recorrer las referencias desde tope.

### Paso 6 — Recorrer una lista sin modificarla

**Para explicar antes de escribir (lectura sugerida):** Queremos visualizar la pila enlazada sin retirar ningún nodo. Como ya no hay índices para recorrer, seguiremos las referencias siguiente desde el tope hasta encontrar null. Para hacerlo sin modificar el estado real de la pila, usaremos una referencia auxiliar llamada actual.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, dentro de public void mostrar() de PilaLista, después de cerrar push; cierra la impresión con println al final. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
Nodo actual = tope;
        System.out.print("Tope a base: ");
        while (actual != null) {
            System.out.print(actual.dato + " ");
            actual = actual.siguiente;
        }
```

**Explicación durante la escritura (puedes leerla al grupo):** Nodo actual = tope crea una referencia de recorrido que empieza arriba. El rótulo Tope a base anuncia el orden mostrado. Mientras actual != null significa que todavía existe un nodo para leer; imprimimos actual.dato y después avanzamos mediante actual = actual.siguiente. Solo actual se mueve: el atributo tope queda intacto. Con quince, diez y cinco, la salida es 15 10 5, y la pila puede seguir utilizándose después de mostrar.

**Detalle técnico de referencia:** `actual` es una variable auxiliar para recorrer; no sustituye a `tope`. La condición `actual!=null` evita leer un nodo inexistente. En cada vuelta se imprime `actual.dato` y se avanza mediante el enlace `actual.siguiente`; el ciclo termina al alcanzar null.

**Comprobación del estado:** Para 15→10→5→null, se imprime 15 10 5, y tope sigue en 15.

**Pregunta para comprobar la comprensión:** ¿Por qué mover actual no retira elementos de la pila?

**Conexión con el siguiente paso:** Demostrar push tres veces en main.

### Paso 7 — Observar la pila enlazada desde main

**Para explicar antes de escribir (lectura sugerida):** Con las clases listas, necesitamos un punto de entrada que permita comprobar la cadena. Crearemos una pila inicialmente vacía y apilaremos cinco, diez y quince para observar cómo cada nuevo nodo se convierte en tope.

**Dónde escribirlo:** En `EJ04_ListaPush.java`, dentro de main de la clase pública EJ04_ListaPush, escrita después de las clases Nodo y PilaLista. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
PilaLista pila = new PilaLista();
        pila.push(5);
        pila.push(10);
        pila.push(15);
        pila.mostrar();
```

**Explicación durante la escritura (puedes leerla al grupo):** La instrucción new PilaLista() crea un objeto cuyo tope comienza en null. push(5) genera el primer nodo; push(10) deja diez enlazado con cinco, y push(15) coloca quince delante de ambos. La llamada mostrar recorre las referencias desde quince hasta null sin cambiarlas. La salida esperada 15 10 5 confirma que la última inserción es la primera que se encontraría si empezáramos a desapilar.

**Detalle técnico de referencia:** Cada push crea su propio Nodo y conecta el nuevo con el anterior. `mostrar()` sigue las referencias a partir de 15. Aunque no se ha fijado una capacidad máxima, la estructura sigue limitada por memoria disponible; no debe afirmarse que es ilimitada.

**Comprobación del estado:** Salida esperada: 15 10 5 desde el tope.

**Pregunta para comprobar la comprensión:** Después de insertar quince, ¿a qué nodo apuntará la referencia tope?

**Conexión con el siguiente paso:** Construir tarea espejo sin cambiar la lógica de enlaces.

### Traza manual — estado antes → operación → estado después

Inicio tope=null. push(5): 5→null. push(10): 10→5→null. push(15): 15→10→5→null. El enlace se establece antes de mover tope.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class Nodo {`:** Declara la clase de un nodo para guardar un entero y su referencia al siguiente nodo.

- **Línea 2, `int dato;`:** Campo que almacena el entero de un objeto Nodo.

- **Línea 3, `Nodo siguiente;`:** Enlace al nodo inmediatamente inferior de esta pila.

- **Línea 5, `public Nodo(int dato) {`:** Constructor del nodo: recibe el entero que quedará almacenado.

- **Línea 6, `this.dato = dato;`:** Almacena el parámetro en el campo dato del nodo recién construido.

- **Línea 7, `this.siguiente = null;`:** Inicializa explícitamente el enlace sin nodo inferior.

- **Línea 11, `class PilaLista {`:** Encapsula la pila enlazada cuyo acceso principal es una referencia al nodo superior.

- **Línea 12, `private Nodo tope;`:** Referencia privada al primer nodo; null representa pila enlazada vacía.

- **Línea 14, `public PilaLista() {`:** Constructor de pila enlazada: determina su estado inicial sin nodos.

- **Línea 15, `tope = null;`:** Fija estado vacío como ausencia de referencia a un nodo.

- **Línea 18, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 19, `Nodo nuevo = new Nodo(dato);`:** Crea el nodo que contendrá el entero recibido para apilar.

- **Línea 20, `nuevo.siguiente = tope;`:** Preserva el tope anterior enlazándolo por debajo del nodo nuevo.

- **Línea 21, `tope = nuevo;`:** Actualiza la cima después de conservar el enlace previo.

- **Línea 22, `System.out.println("Apilado: " + dato);`:** Imprime el valor aceptado para que el estudiante valide cada push.

- **Línea 25, `public void mostrar() {`:** Proporciona una vista de los elementos sin alterar la pila.

- **Línea 26, `Nodo actual = tope;`:** Copia la referencia inicial de la cadena a una variable auxiliar sin modificar tope.

- **Línea 27, `System.out.print("Tope a base: ");`:** Anuncia que la lectura comienza en el elemento superior.

- **Línea 28, `while (actual != null) {`:** Itera mientras exista un nodo válido; el recorrido termina al llegar a null.

- **Línea 29, `System.out.print(actual.dato + " ");`:** Imprime el entero guardado en el nodo visitado.

- **Línea 30, `actual = actual.siguiente;`:** Avanza al siguiente enlace; sin este paso el ciclo repetiría el mismo nodo.

- **Línea 32, `System.out.println();`:** Cierra la salida actual con un salto de línea.

- **Línea 36, `public class EJ04_ListaPush {`:** Declara la clase pública ejecutable del EJ04; su nombre coincide con el archivo Java.

- **Línea 37, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 38, `PilaLista pila = new PilaLista();`:** Crea una pila enlazada inicialmente vacía.

- **Línea 39, `pila.push(5);`:** Inserta 5; cambia el tope según la implementación del EJ04.

- **Línea 40, `pila.push(10);`:** Inserta 10; cambia el tope según la implementación del EJ04.

- **Línea 41, `pila.push(15);`:** Inserta 15; cambia el tope según la implementación del EJ04.

- **Línea 42, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class Nodo {
    int dato;
    Nodo siguiente;

    public Nodo(int dato) {
        this.dato = dato;
        this.siguiente = null;
    }
}

class PilaLista {
    private Nodo tope;

    public PilaLista() {
        tope = null;
    }

    public void push(int dato) {
        Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = tope;
        tope = nuevo;
        System.out.println("Apilado: " + dato);
    }

    public void mostrar() {
        Nodo actual = tope;
        System.out.print("Tope a base: ");
        while (actual != null) {
            System.out.print(actual.dato + " ");
            actual = actual.siguiente;
        }
        System.out.println();
    }
}

public class EJ04_ListaPush {
    public static void main(String[] args) {
        PilaLista pila = new PilaLista();
        pila.push(5);
        pila.push(10);
        pila.push(15);
        pila.mostrar();
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ04` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ04_ListaPush.java
java EJ04_ListaPush
```

**Resultado esperado en consola:**

```text
Apilado: 5
Apilado: 10
Apilado: 15
Tope a base: 15 10 5
```

**Interpretación:** Inicio tope=null. push(5): 5→null. push(10): 10→5→null. push(15): 15→10→5→null. El enlace se establece antes de mover tope. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Imprime mostrar() después de cada push para observar cómo se amplía la cadena.

### Error o caso límite y corrección razonada

Si cambias el orden a `tope=nuevo; nuevo.siguiente=tope`, el nodo se enlaza consigo mismo: un recorrido while ya no finaliza. No ejecutes esa variante sin corregirla; razona y restaura el orden.


### Pregunta de comprensión

¿Qué diferencia existe entre tope=-1 en vector y tope=null en una lista?


## T04 — Tarea espejo de EJ04

**Enunciado:** Construye la misma pila enlazada apilando 2,4,6; dibuja las referencias exactas y justifica por qué nuevo.siguiente se asigna primero.

**Archivo a modificar:** crea una copia de `EJ04_ListaPush.java` en la carpeta `T04`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Modifica las llamadas a push manteniendo el orden nuevo.siguiente=tope; tope=nuevo.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Apilado: 2
Apilado: 4
Apilado: 6
Tope a base: 6 4 2
```


---

# EJ05 — Desapilar nodos sin perder referencias

**Objetivo técnico — qué aprenderemos y qué haremos:** En este ejercicio vamos a **desapilar elementos de una lista enlazada** sin perder el resto de los nodos. Partiremos de la cadena 15 → 10 → 5, donde 15 es el tope. Programaremos `pop()` para recuperar primero el dato 15 y después mover la referencia `tope` al nodo que contiene 10. También comprobaremos el caso en que `tope` vale `null`, señal de que no existen nodos para retirar. El resultado esperado es que la extracción devuelva 15, pero la estructura conserve correctamente los nodos 10 y 5.

**Duración orientativa para estudio autónomo:** 14 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ05/EJ05_ListaPop.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Una pila enlazada también debe retirar primero el último valor insertado. Sin embargo, aquí no podemos bajar un índice: debemos **cambiar una referencia**. Si `tope` apunta al nodo 15, antes de moverlo necesitamos guardar el valor que vamos a devolver. Luego actualizaremos `tope` para que señale al nodo 10, y el 5 seguirá enlazado. La pregunta central es: **¿cómo retiramos el primer nodo sin perder el acceso a todos los que vienen después?**

### Paso 2 — Recordar cómo están enlazados los nodos

**Para explicar antes de escribir (lectura sugerida):** Antes de escribir pop para nodos, recuperemos una idea del ejercicio anterior: el tope de esta pila no es un número ni un índice. Es una referencia al primer nodo de una cadena. Por eso, al retirar el elemento superior, debemos cambiar esa referencia sin perder el acceso a los siguientes nodos.

**Dónde escribirlo:** En `EJ05_ListaPop.java`, dentro de PilaLista de EJ05_ListaPop.java, después de la clase Nodo; conserva los métodos push y mostrar del archivo completo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
private Nodo tope;

    public PilaLista() {
        tope = null;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** private Nodo tope guarda dónde empieza la cadena. Al crear PilaLista, asignamos tope = null porque la pila está vacía. Las clases Nodo y el método push del archivo completo conservan el comportamiento estudiado: apilar cinco, diez y quince produce tope → 15 → 10 → 5 → null. Ese estado inicial nos permitirá comprobar el cambio provocado por pop.

**Detalle técnico de referencia:** `tope` conserva la referencia a la cabecera de la pila. `null` significa ausencia de nodos. Después de tres push 5,10,15, el objeto de cima es 15 y su referencia siguiente apunta al nodo 10.

**Comprobación del estado:** Estado antes de extraer: 15→10→5→null.

**Pregunta para comprobar la comprensión:** Si tope señala al nodo quince, ¿a qué nodo apunta su campo siguiente?

**Conexión con el siguiente paso:** Comprobar si existe nodo antes de acceder a su dato.

### Paso 3 — Definir pop con condición de vacío

**Para explicar antes de escribir (lectura sugerida):** Vamos a crear pop para extraer el nodo superior y devolver su dato. Pero hay un caso que debemos atender primero: si tope es null, no existe ningún nodo del que podamos leer el atributo dato. Por eso la comprobación de vacío va antes de cualquier acceso.

**Dónde escribirlo:** En `EJ05_ListaPop.java`, dentro de PilaLista, después de push y antes de mostrar; comienza el método pop() con la validación de vacío. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public int pop() {
        if (tope == null) {
            System.out.println("Error: la pila está vacía");
            return -1;
        }
```

**Explicación durante la escritura (puedes leerla al grupo):** public int pop() devolverá un entero cuando retire un nodo. Con if (tope == null) detectamos la pila sin elementos. Si está vacía, mostramos el mensaje indicado y devolvemos menos uno como señal didáctica; no actualizamos la referencia. En una pila no vacía, la ejecución seguirá después del if. La comparación con null es la equivalente, en una lista, a comprobar tope == -1 en un vector.

**Detalle técnico de referencia:** `pop()` devuelve int. El if de pila vacía evita desreferenciar null. La señal -1 es una decisión didáctica que replica la sesión y supone que los datos de ejercicio no usan -1; no es una excepción automática de Java.

**Comprobación del estado:** Para tope=null se escribe el error y no se altera la referencia.

**Pregunta para comprobar la comprensión:** ¿Qué sucedería si intentáramos leer tope.dato estando tope en null?

**Conexión con el siguiente paso:** Preservar el dato saliente y reenlazar.

### Paso 4 — Cambiar el tope hacia el siguiente nodo

**Para explicar antes de escribir (lectura sugerida):** Ahora realizaremos la extracción verdadera. Tenemos que lograr dos cosas: devolver el valor del primer nodo y conservar acceso a todo lo que estaba debajo. Resolveremos ambas en tres líneas, respetando un orden que evita perder la información.

**Dónde escribirlo:** En `EJ05_ListaPop.java`, en el mismo método pop(), inmediatamente después del if de vacío; luego cierra el método. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
int dato = tope.dato;
        tope = tope.siguiente;
        return dato;
```

**Explicación durante la escritura (puedes leerla al grupo):** La línea int dato = tope.dato guarda el número del nodo superior: inicialmente quince. Luego tope = tope.siguiente cambia la entrada de la pila al siguiente nodo, el de diez. No es necesario recorrer los demás enlaces ni mover sus datos. Finalmente return dato devuelve quince a quien llamó a pop. El resultado es una pila que comienza en diez y sigue hacia cinco, manteniendo intacta la cadena restante.

**Detalle técnico de referencia:** La variable local `dato` guarda el 15 que se va a devolver. `tope=tope.siguiente` hace que la pila comience en 10, conservando 10→5→null. Luego `return dato` entrega 15. No se necesita borrar manualmente el nodo antiguo: dejamos de referenciarlo desde la pila. Si se pierde el enlace siguiente antes de mover tope, el resto queda inaccesible.

**Comprobación del estado:** Antes 15→10→5; después 10→5; devuelto 15.

**Pregunta para comprobar la comprensión:** Si pop devuelve quince, ¿cuál es la nueva referencia a la que debe apuntar tope?

**Conexión con el siguiente paso:** Leer la nueva cadena sin modificarla.

### Paso 5 — Comprobar el nuevo tope

**Para explicar antes de escribir (lectura sugerida):** Comprobaremos el resultado en tres momentos: antes de extraer, durante la extracción y después. Así verificaremos que pop retira exactamente el primer nodo y que no se pierde el resto de la cadena.

**Dónde escribirlo:** En `EJ05_ListaPop.java`, dentro de main de EJ05_ListaPop, tras construir la pila e insertar cinco, diez y quince. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
pila.mostrar();
        System.out.println("Retirado: " + pila.pop());
        pila.mostrar();
```

**Explicación durante la escritura (puedes leerla al grupo):** La primera llamada mostrar escribe quince, diez y cinco. En el println central, pila.pop() devuelve quince y actualiza el tope para apuntar a diez. La llamada mostrar final debe comenzar en diez y continuar con cinco. Si después de pop se imprimieran otros valores o la lista quedara inaccesible, habría que revisar la asignación de tope = tope.siguiente.

**Detalle técnico de referencia:** Primero `mostrar()` recorre 15,10,5. La llamada `pop()` cambia el tope a 10 y devuelve 15. La vista posterior muestra 10,5: este es el significado de LIFO para una lista enlazada.

**Comprobación del estado:** La impresión del contenido confirma que 10 sigue conectado a 5.

**Pregunta para comprobar la comprensión:** ¿Qué dos valores deben seguir siendo accesibles después de retirar quince?

**Conexión con el siguiente paso:** Repetir la operación como tarea T05.

### Traza manual — estado antes → operación → estado después

tope→15→10→5→null. Guardar 15, mover tope a 10, devolver 15; queda tope→10→5→null.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class Nodo {`:** Declara la clase de un nodo para guardar un entero y su referencia al siguiente nodo.

- **Línea 2, `int dato;`:** Campo que almacena el entero de un objeto Nodo.

- **Línea 3, `Nodo siguiente;`:** Enlace al nodo inmediatamente inferior de esta pila.

- **Línea 5, `public Nodo(int dato) {`:** Constructor del nodo: recibe el entero que quedará almacenado.

- **Línea 6, `this.dato = dato;`:** Almacena el parámetro en el campo dato del nodo recién construido.

- **Línea 7, `this.siguiente = null;`:** Inicializa explícitamente el enlace sin nodo inferior.

- **Línea 11, `class PilaLista {`:** Encapsula la pila enlazada cuyo acceso principal es una referencia al nodo superior.

- **Línea 12, `private Nodo tope;`:** Referencia privada al primer nodo; null representa pila enlazada vacía.

- **Línea 14, `public PilaLista() {`:** Constructor de pila enlazada: determina su estado inicial sin nodos.

- **Línea 15, `tope = null;`:** Fija estado vacío como ausencia de referencia a un nodo.

- **Línea 18, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 19, `Nodo nuevo = new Nodo(dato);`:** Crea el nodo que contendrá el entero recibido para apilar.

- **Línea 20, `nuevo.siguiente = tope;`:** Preserva el tope anterior enlazándolo por debajo del nodo nuevo.

- **Línea 21, `tope = nuevo;`:** Actualiza la cima después de conservar el enlace previo.

- **Línea 22, `System.out.println("Apilado: " + dato);`:** Imprime el valor aceptado para que el estudiante valide cada push.

- **Línea 25, `public int pop() {`:** Operación de extracción: retorna el entero de cima o la señal didáctica -1 si está vacía.

- **Línea 26, `if (tope == null) {`:** Protege cualquier acceso a dato/pagina de un nodo inexistente.

- **Línea 27, `System.out.println("Error: la pila está vacía");`:** Informa que se intentó extraer sin elementos válidos.

- **Línea 28, `return -1;`:** Devuelve el centinela -1; este laboratorio excluye -1 como dato válido para evitar ambigüedad.

- **Línea 30, `int dato = tope.dato;`:** Recupera el entero del nodo actual antes de mover la referencia.

- **Línea 31, `tope = tope.siguiente;`:** Desvincula la cabecera lógica; la pila sigue desde el nodo que era segundo.

- **Línea 32, `return dato;`:** Entrega al llamador el valor rescatado antes del cambio de tope.

- **Línea 35, `public void mostrar() {`:** Proporciona una vista de los elementos sin alterar la pila.

- **Línea 36, `Nodo actual = tope;`:** Copia la referencia inicial de la cadena a una variable auxiliar sin modificar tope.

- **Línea 37, `System.out.print("Tope a base: ");`:** Anuncia que la lectura comienza en el elemento superior.

- **Línea 38, `while (actual != null) {`:** Itera mientras exista un nodo válido; el recorrido termina al llegar a null.

- **Línea 39, `System.out.print(actual.dato + " ");`:** Imprime el entero guardado en el nodo visitado.

- **Línea 40, `actual = actual.siguiente;`:** Avanza al siguiente enlace; sin este paso el ciclo repetiría el mismo nodo.

- **Línea 42, `System.out.println();`:** Cierra la salida actual con un salto de línea.

- **Línea 46, `public class EJ05_ListaPop {`:** Declara la clase pública ejecutable del EJ05; su nombre coincide con el archivo Java.

- **Línea 47, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 48, `PilaLista pila = new PilaLista();`:** Crea una pila enlazada inicialmente vacía.

- **Línea 49, `pila.push(5);`:** Inserta 5; cambia el tope según la implementación del EJ05.

- **Línea 50, `pila.push(10);`:** Inserta 10; cambia el tope según la implementación del EJ05.

- **Línea 51, `pila.push(15);`:** Inserta 15; cambia el tope según la implementación del EJ05.

- **Línea 52, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.

- **Línea 53, `System.out.println("Retirado: " + pila.pop());`:** Extrae la cima del objeto pila, imprime el dato devuelto y actualiza su estado.

- **Línea 54, `pila.mostrar();`:** Representa la pila actual desde la cima hasta la base, sin modificarla.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class Nodo {
    int dato;
    Nodo siguiente;

    public Nodo(int dato) {
        this.dato = dato;
        this.siguiente = null;
    }
}

class PilaLista {
    private Nodo tope;

    public PilaLista() {
        tope = null;
    }

    public void push(int dato) {
        Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = tope;
        tope = nuevo;
        System.out.println("Apilado: " + dato);
    }

    public int pop() {
        if (tope == null) {
            System.out.println("Error: la pila está vacía");
            return -1;
        }
        int dato = tope.dato;
        tope = tope.siguiente;
        return dato;
    }

    public void mostrar() {
        Nodo actual = tope;
        System.out.print("Tope a base: ");
        while (actual != null) {
            System.out.print(actual.dato + " ");
            actual = actual.siguiente;
        }
        System.out.println();
    }
}

public class EJ05_ListaPop {
    public static void main(String[] args) {
        PilaLista pila = new PilaLista();
        pila.push(5);
        pila.push(10);
        pila.push(15);
        pila.mostrar();
        System.out.println("Retirado: " + pila.pop());
        pila.mostrar();
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ05` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ05_ListaPop.java
java EJ05_ListaPop
```

**Resultado esperado en consola:**

```text
Apilado: 5
Apilado: 10
Apilado: 15
Tope a base: 15 10 5 
Retirado: 15
Tope a base: 10 5
```

**Interpretación:** tope→15→10→5→null. Guardar 15, mover tope a 10, devolver 15; queda tope→10→5→null. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Vacía la lista con tres pops y prueba un cuarto: interpreta la señal -1 de error sin atribuirla a una excepción obligatoria.

### Error o caso límite y corrección razonada

Leer `tope.dato` antes del guardia en una lista vacía sí intentaría acceder a null. Mantener `if(tope==null)` evita ese fallo y da un resultado previsto.


### Pregunta de comprensión

¿Qué línea conserva el elemento eliminado y cuál conserva el resto de la lista?


## T05 — Tarea espejo de EJ05

**Enunciado:** Ejecuta dos operaciones pop consecutivas y demuestra por dibujo que el tope final vale 5, sin perder el último enlace a null.

**Archivo a modificar:** crea una copia de `EJ05_ListaPop.java` en la carpeta `T05`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Agrega un segundo `println` con `pop()`; no cambies el código de referencias dentro del método.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Apilado: 5
Apilado: 10
Apilado: 15
Tope a base: 15 10 5 
Retirado: 15
Retirado: 10
Tope a base: 5
```


---

# EJ06 — Simular el botón Atrás de un navegador

**Objetivo técnico — qué aprenderemos y qué haremos:** Vamos a **simular el funcionamiento básico del botón Atrás de un navegador** utilizando una pila. Comenzaremos en Google, navegaremos a YouTube y después a Wikipedia. Antes de cambiar de página, guardaremos la página anterior en un historial enlazado. Cuando ejecutemos la acción de retroceder, el método `pop()` recuperará YouTube, porque fue la última página guardada, y la página visible volverá a ser YouTube. Aprenderemos a diferenciar la página que se está mostrando del contenido almacenado en el historial y a evitar cambios cuando el historial ya está vacío.

**Duración orientativa para estudio autónomo:** 17 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ06/EJ06_NavegadorAtras.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Imaginemos que abrimos Google, luego YouTube y después Wikipedia. Cuando pulsamos Atrás, no queremos volver a la página que ya está abierta: queremos regresar a YouTube. Para hacerlo, necesitaremos **guardar la página que abandonamos antes de navegar a la siguiente**. La pila conservará el historial de páginas anteriores, mientras que otra variable guardará la página actualmente visible. La pregunta que resolveremos es: **¿por qué el tope del historial debe ser YouTube y no Wikipedia?**

### Paso 2 — Adaptar Nodo para páginas, sin crear otra estructura teórica

**Para explicar antes de escribir (lectura sugerida):** Vamos a aplicar LIFO a un caso cercano: el botón Atrás. Esta vez los elementos no serán números, sino nombres de páginas. Para guardar cada página previa sin utilizar un vector, construiremos nodos enlazados cuyo dato será un String.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, al principio del archivo EJ06_NavegadorAtras.java, antes de la clase HistorialAtras. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class NodoPagina {
    String pagina;
    NodoPagina siguiente;
```

**Explicación durante la escritura (puedes leerla al grupo):** La clase NodoPagina representa una entrada del historial. String pagina conserva el nombre de la página que dejamos atrás. NodoPagina siguiente enlaza con la página anterior guardada antes que esta. La estructura es la misma que usamos con enteros: cambia el tipo del dato, pero cada nodo sigue teniendo su contenido y una referencia al siguiente. El nombre NodoPagina nos ayuda a distinguir claramente la función del objeto.

**Detalle técnico de referencia:** El nodo sigue conteniendo dato y enlace, pero aquí `pagina` guarda texto como "Google". `NodoPagina siguiente` apunta al anterior nodo registrado. No estamos implementando un navegador completo; solo su historial de retroceso.

**Comprobación del estado:** Cada nodo representa una página anterior, no necesariamente la página que está visible.

**Pregunta para comprobar la comprensión:** ¿Por qué aquí usamos String para el dato del nodo?

**Conexión con el siguiente paso:** Construir el enlace inicial del nodo.

### Paso 3 — Crear el constructor de NodoPagina

**Para explicar antes de escribir (lectura sugerida):** Cada vez que guardemos una página en el historial, necesitaremos crear un nodo con un nombre válido y una referencia inicial. El constructor hará esa preparación, de forma semejante a lo que ya hicimos con los nodos de enteros.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, dentro de NodoPagina, después de los atributos; cierra la clase antes de comenzar HistorialAtras. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public NodoPagina(String pagina) {
        this.pagina = pagina;
        this.siguiente = null;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** El constructor NodoPagina(String pagina) recibe el texto de la página. this.pagina = pagina lo conserva dentro del objeto. this.siguiente = null indica que el nodo nace sin apuntar a una entrada anterior; cuando lo apilemos, modificaremos esa referencia para conectarlo con el historial existente. No estamos creando un navegador real: simularemos únicamente el registro de páginas previas y la acción Atrás.

**Detalle técnico de referencia:** `this.pagina` almacena el texto recibido y `siguiente=null` indica ausencia de enlace inicial. Push conectará este objeto al historial que ya exista.

**Comprobación del estado:** Nodo aislado con página y enlace null.

**Pregunta para comprobar la comprensión:** ¿Cuándo cambiará la referencia siguiente de este nodo recién creado?

**Conexión con el siguiente paso:** Implementar el historial usando una referencia al tope.

### Paso 4 — Guardar la página anterior al navegar

**Para explicar antes de escribir (lectura sugerida):** El historial necesita recordar cuál fue la última página que abandonamos. Para ello crearemos una clase HistorialAtras que conserva una referencia tope y ofrece una operación push. Cada vez que vayamos a otra página, guardaremos la página que estaba visible antes del cambio.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, dentro de class HistorialAtras, antes de su método pop(); al escribir el archivo completo incorpora la llave de cierre del método push. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
private NodoPagina tope;

    public void push(String paginaAnterior) {
        NodoPagina nuevo = new NodoPagina(paginaAnterior);
        nuevo.siguiente = tope;
        tope = nuevo;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** private NodoPagina tope indica dónde comienza el historial; al crear el objeto Java la referencia comienza en null porque no asignamos otro valor. En push(String paginaAnterior) recibimos la página que acabamos de dejar. new NodoPagina la convierte en un nodo, nuevo.siguiente = tope enlaza con el historial antiguo y tope = nuevo actualiza la cima. Así, antes de pasar de Google a YouTube guardamos Google; antes de pasar de YouTube a Wikipedia guardamos YouTube. El tope del historial termina siendo YouTube, no Wikipedia.

**Detalle técnico de referencia:** `tope` empieza en null por defecto porque es un campo de referencia. Cada push recibe la página que estamos dejando, crea el nodo, preserva el antiguo tope enlazándolo y coloca el nuevo como tope. La página actual se maneja por separado en main; así el último destino anterior es el primero al que regresa Atrás.

**Comprobación del estado:** Después de ir de Google a YouTube y luego a Wikipedia, historial tope→YouTube→Google.

**Pregunta para comprobar la comprensión:** Si acabamos de llegar a Wikipedia, ¿qué página debe guardar el tope del historial?

**Conexión con el siguiente paso:** Recuperar la última página anterior.

### Paso 5 — Desapilar el historial con un retorno nullable

**Para explicar antes de escribir (lectura sugerida):** Para implementar Atrás necesitamos recuperar la página previa que está en el tope del historial. La operación pop devolverá un texto, no un entero, y debe tratar de manera previsible un historial vacío.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, dentro de HistorialAtras, después del método push; el fragmento ya representa el método completo pop. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
public String pop() {
        if (tope == null) {
            return null;
        }
        String anterior = tope.pagina;
        tope = tope.siguiente;
        return anterior;
    }
```

**Explicación durante la escritura (puedes leerla al grupo):** public String pop() indica que devolvemos una página o una señal de ausencia. Si tope es null, el método retorna null, lo que significa que no hay una página guardada. Si existe un nodo, String anterior = tope.pagina conserva el nombre de la página. Después tope = tope.siguiente retira lógicamente esa entrada y return anterior entrega el nombre al programa. El botón Atrás podrá usarlo para cambiar la página visible solo cuando no sea null.

**Detalle técnico de referencia:** El retorno es String, y `null` representa historial vacío (diferente al -1 de enteros). Se guarda primero el texto del tope, se mueve la referencia al nodo siguiente y después se entrega el texto que la interfaz debe mostrar.

**Comprobación del estado:** Historial YouTube→Google se convierte en Google; pop devuelve YouTube.

**Pregunta para comprobar la comprensión:** Si no existe página anterior, ¿por qué es mejor conservar la página actual que inventar una nueva?

**Conexión con el siguiente paso:** Cambiar la página visible solo si pop devuelve una anterior válida.

### Paso 6 — Separar página actual de historial

**Para explicar antes de escribir (lectura sugerida):** Para simular un navegador debemos separar dos conceptos que suelen confundirse: la página que estamos viendo ahora y el historial de páginas anteriores. La variable actual guardará la página visible; la pila almacenará únicamente las páginas que dejamos atrás.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, dentro de main de EJ06_NavegadorAtras, después de crear el objeto historial con new HistorialAtras(). Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
String actual = "Google";
        historial.push(actual);
        actual = "YouTube";
        historial.push(actual);
        actual = "Wikipedia";
```

**Explicación durante la escritura (puedes leerla al grupo):** Comenzamos con String actual = "Google". Antes de navegar, historial.push(actual) guarda Google; entonces asignamos YouTube como página visible. Repetimos la operación: guardamos YouTube en la pila y actual pasa a Wikipedia. En ese instante la pantalla debería mostrar Wikipedia, pero el tope del historial será YouTube. Esta separación explica por qué Atrás no devuelve la página que ya está abierta.

**Detalle técnico de referencia:** Empezamos viendo Google; al visitar YouTube guardamos Google; antes de visitar Wikipedia guardamos YouTube. Al final `actual` es Wikipedia y el historial conserva YouTube encima de Google. El ejemplo simplifica la navegación real y solo enseña la operación LIFO del botón Atrás.

**Comprobación del estado:** Estado visible: Wikipedia; historial: YouTube→Google.

**Pregunta para comprobar la comprensión:** Cuando actual es Wikipedia, ¿por qué el tope del historial no es Wikipedia?

**Conexión con el siguiente paso:** Ejecutar Atrás sin perder el estado si la pila está vacía.

### Paso 7 — Aplicar Atrás con validación

**Para explicar antes de escribir (lectura sugerida):** Ahora llega el momento de utilizar el historial. Como ya estamos en Wikipedia, queremos que Atrás recupere YouTube. Primero solicitaremos pop al historial; solo después decidiremos si actualizar la página visible.

**Dónde escribirlo:** En `EJ06_NavegadorAtras.java`, dentro del mismo main, después de mostrar la página actual y antes de cerrar el método. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
String anterior = historial.pop();
        if (anterior != null) {
            actual = anterior;
        }
        System.out.println("Tras atrás: " + actual);
```

**Explicación durante la escritura (puedes leerla al grupo):** La instrucción String anterior = historial.pop() recupera la página guardada arriba: YouTube. La condición if (anterior != null) confirma que efectivamente encontramos una página anterior. Únicamente entonces actual = anterior cambia la vista; si el historial estuviera vacío, actual permanecería igual. El println final permite verificar que después de Atrás estamos en YouTube y que Google continúa guardado para un segundo retroceso.

**Detalle técnico de referencia:** Se retira YouTube, último elemento guardado, y se actualiza la variable `actual`. Si `anterior` fuese null, la condición impediría reemplazar la página visible por un destino inexistente.

**Comprobación del estado:** Tras Atrás, la página visible es YouTube y el historial conserva Google.

**Pregunta para comprobar la comprensión:** ¿Qué debería ocurrir si pulsamos Atrás una vez más y luego otra cuando ya no queda historial?

**Conexión con el siguiente paso:** Cambiar recorrido y usar Atrás dos veces en T06.

### Traza manual — estado antes → operación → estado después

Visible Google → navegar YouTube (historial [Google]) → navegar Wikipedia (historial [Google,YouTube]). Atrás: pop=YouTube; visible YouTube; historial [Google].

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class NodoPagina {`:** Declara el molde de un nodo de historial que almacena texto y un enlace.

- **Línea 2, `String pagina;`:** Campo de texto que identifica una página almacenada en el historial.

- **Línea 3, `NodoPagina siguiente;`:** Enlace a la siguiente página anterior de la cadena.

- **Línea 5, `public NodoPagina(String pagina) {`:** Constructor de página: recibe el texto del sitio que se guardará como anterior.

- **Línea 6, `this.pagina = pagina;`:** Guarda el nombre de la página anterior dentro del nodo.

- **Línea 7, `this.siguiente = null;`:** Inicializa explícitamente el enlace sin nodo inferior.

- **Línea 11, `class HistorialAtras {`:** Define la pila enlazada especializada para recordar páginas anteriores.

- **Línea 12, `private NodoPagina tope;`:** Referencia al nodo más reciente del historial; por defecto inicia en null.

- **Línea 14, `public void push(String paginaAnterior) {`:** Inserta el nombre de una página que dejamos atrás al navegar a otra.

- **Línea 15, `NodoPagina nuevo = new NodoPagina(paginaAnterior);`:** Construye el nodo que guardará la página que acabamos de dejar.

- **Línea 16, `nuevo.siguiente = tope;`:** Preserva el tope anterior enlazándolo por debajo del nodo nuevo.

- **Línea 17, `tope = nuevo;`:** Actualiza la cima después de conservar el enlace previo.

- **Línea 20, `public String pop() {`:** Devuelve la página anterior o null cuando no existe historial.

- **Línea 21, `if (tope == null) {`:** Protege cualquier acceso a dato/pagina de un nodo inexistente.

- **Línea 22, `return null;`:** Devuelve ausencia de página anterior sin modificar la navegación visible.

- **Línea 24, `String anterior = tope.pagina;`:** Rescata el nombre de la última página anterior almacenada.

- **Línea 25, `tope = tope.siguiente;`:** Desvincula la cabecera lógica; la pila sigue desde el nodo que era segundo.

- **Línea 26, `return anterior;`:** Entrega la página rescatada para actualizar la visible.

- **Línea 30, `public class EJ06_NavegadorAtras {`:** Declara la clase pública ejecutable del EJ06; su nombre coincide con el archivo Java.

- **Línea 31, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 32, `HistorialAtras historial = new HistorialAtras();`:** Crea el historial inicialmente vacío independiente de la página visible.

- **Línea 33, `String actual = "Google";`:** Establece la página visible inicial: todavía no es un elemento extraído.

- **Línea 34, `historial.push(actual);`:** Guarda la página que estamos dejando antes de sustituir la variable actual.

- **Línea 35, `actual = "YouTube";`:** Simula navegar a un destino nuevo, separando visualización e historial LIFO.

- **Línea 36, `historial.push(actual);`:** Guarda la página que estamos dejando antes de sustituir la variable actual.

- **Línea 37, `actual = "Wikipedia";`:** Simula navegar a un destino nuevo, separando visualización e historial LIFO.

- **Línea 38, `System.out.println("Página actual: " + actual);`:** Muestra qué página está visible antes de ejecutar Atrás.

- **Línea 39, `String anterior = historial.pop();`:** Solicita la página anterior mediante pop; también actualiza el tope del historial.

- **Línea 40, `if (anterior != null) {`:** Solo permite cambiar la página visible cuando existe un destino anterior.

- **Línea 41, `actual = anterior;`:** Actualiza la página visible con la dirección que devolvió pop.

- **Línea 43, `System.out.println("Tras atrás: " + actual);`:** Muestra la página visible después de aplicar Atrás con validación null.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class NodoPagina {
    String pagina;
    NodoPagina siguiente;

    public NodoPagina(String pagina) {
        this.pagina = pagina;
        this.siguiente = null;
    }
}

class HistorialAtras {
    private NodoPagina tope;

    public void push(String paginaAnterior) {
        NodoPagina nuevo = new NodoPagina(paginaAnterior);
        nuevo.siguiente = tope;
        tope = nuevo;
    }

    public String pop() {
        if (tope == null) {
            return null;
        }
        String anterior = tope.pagina;
        tope = tope.siguiente;
        return anterior;
    }
}

public class EJ06_NavegadorAtras {
    public static void main(String[] args) {
        HistorialAtras historial = new HistorialAtras();
        String actual = "Google";
        historial.push(actual);
        actual = "YouTube";
        historial.push(actual);
        actual = "Wikipedia";
        System.out.println("Página actual: " + actual);
        String anterior = historial.pop();
        if (anterior != null) {
            actual = anterior;
        }
        System.out.println("Tras atrás: " + actual);
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ06` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ06_NavegadorAtras.java
java EJ06_NavegadorAtras
```

**Resultado esperado en consola:**

```text
Página actual: Wikipedia
Tras atrás: YouTube
```

**Interpretación:** Visible Google → navegar YouTube (historial [Google]) → navegar Wikipedia (historial [Google,YouTube]). Atrás: pop=YouTube; visible YouTube; historial [Google]. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Haz un tercer Atrás tras agotar el historial: valida null y justifica por qué debe mantenerse la página actual.

### Error o caso límite y corrección razonada

No empujes la página nueva como anterior después de actualizar actual: si guardas ChatGPT en vez de Wikipedia, Atrás regresaría incorrectamente a la página de partida.


### Pregunta de comprensión

¿Por qué en esta simulación el tope del historial guarda YouTube y no Wikipedia?


## T06 — Tarea espejo de EJ06

**Enunciado:** Simula Google → Wikipedia → ChatGPT y ejecuta Atrás dos veces; explica qué página se guarda antes de cada navegación y qué valor deja pop.

**Archivo a modificar:** crea una copia de `EJ06_NavegadorAtras.java` en la carpeta `T06`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Reemplaza los destinos de navegación y añade una segunda secuencia pop + validación null + actualización actual.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Página actual: ChatGPT
Tras atrás 1: Wikipedia
Tras atrás 2: Google
```


---

# EJ07 — Integrar y contrastar ambas representaciones

**Objetivo técnico — qué aprenderemos y qué haremos:** Para cerrar, vamos a **comparar las dos implementaciones de pila que hemos desarrollado**. Crearemos una pila con vector y otra con lista enlazada; insertaremos en ambas los mismos valores 10, 20 y 30, y comprobaremos que las primeras extracciones devuelven 30 y 20 en el mismo orden. Después analizaremos qué ocurre cuando el vector tiene menos capacidad y rechaza algunas inserciones mientras la lista continúa aceptándolas. La meta es demostrar que las diferencias de almacenamiento y capacidad **no cambian la regla LIFO**: cada estructura retira primero el último elemento que realmente consiguió almacenar.

**Duración orientativa para estudio autónomo:** 22 minutos.

**Archivo que crearás:** `Sesion09_Pilas/EJ07/EJ07_ComparacionLIFO.java`. Mantén este ejemplo aislado en su carpeta. **Punto de partida:** un archivo Java nuevo en blanco; si una estructura se repite de otro ejemplo, se vuelve a escribir aquí para que este programa sea independiente.

### Paso 1 — Comprender el problema antes de programar

Hemos construido dos pilas de apariencia distinta: una usa índices y la otra usa referencias. Ahora necesitamos demostrar con resultados que **ambas obedecen LIFO**. Ejecutaremos las mismas inserciones y extracciones sobre las dos estructuras y compararemos los valores devueltos. Después reduciremos la capacidad del vector y observaremos un caso importante: si una estructura rechaza una inserción, sus próximas salidas pueden ser diferentes. Eso no rompe LIFO; significa que **los elementos aceptados por ambas pilas ya no son los mismos**.

### Paso 2 — Disponer de dos implementaciones sin fusionarlas

**Para explicar antes de escribir (lectura sugerida):** Vamos a comprobar algo importante: una pila con vector y otra con nodos pueden comportarse igual aunque por dentro trabajen de manera distinta. Para compararlas, mantendremos ambas clases dentro de un mismo archivo, cada una con su propio almacenamiento y sin compartir el tope.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, al comienzo de EJ07_ComparacionLIFO.java, declara la clase PilaVector y sus dos atributos; continúa con su constructor del archivo final. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class PilaVector {
    private int[] vector;
    private int tope;
```

**Explicación durante la escritura (puedes leerla al grupo):** La primera clase se llama PilaVector. Su arreglo vector contiene los enteros y su atributo tope indica el índice ocupado más alto. En este ejercicio el constructor recibe capacidad y reserva el arreglo, asignando menos uno al tope inicial. Observa que aquí no guardamos un campo tamano adicional: consultaremos directamente vector.length, que representa la misma capacidad física. Esta diferencia interna no modifica la definición de una pila.

**Detalle técnico de referencia:** La primera implementación guarda enteros en celdas contiguas y un índice tope. La clase se simplifica respecto de EJ03: usa `vector.length` en lugar de guardar un atributo tamano; la condición sigue siendo la misma. El constructor recibe capacidad y fija tope=-1.

**Comprobación del estado:** Vector nuevo capacidad 3; tope=-1.

**Pregunta para comprobar la comprensión:** ¿Por qué vector.length puede reemplazar el campo tamano en esta versión?

**Conexión con el siguiente paso:** Resumir las operaciones vectoriales con la misma lógica ya estudiada.

### Paso 3 — Respetar el orden de acceso a la cima vectorial

**Para explicar antes de escribir (lectura sugerida):** Ahora reconstruiremos la inserción vectorial en una forma más compacta. No estamos cambiando la regla de apilar; solo concentraremos en una expresión lo que antes escribíamos en dos líneas. Primero seguiremos comprobando si queda capacidad.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, dentro de push(int dato) de PilaVector, después de escribir el encabezado del método y antes de cerrarlo. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
if (tope == vector.length - 1) {
            System.out.println("Vector lleno");
            return;
        }
        vector[++tope] = dato;
```

**Explicación durante la escritura (puedes leerla al grupo):** La comparación tope == vector.length - 1 reconoce el último índice permitido. Si se cumple, se informa Vector lleno y se termina el intento. En caso contrario, vector[++tope] = dato incrementa el índice antes de utilizarlo: el doble signo más aparece delante de tope y por eso corresponde al incremento previo. Este comportamiento equivale a escribir tope++ y luego vector[tope] = dato en dos instrucciones. Las inserciones siguen ocupando la siguiente posición libre.

**Detalle técnico de referencia:** El if detiene la inserción si se ocupa la última celda. `vector[++tope]=dato` equivale a las dos instrucciones previas `tope++; vector[tope]=dato;`: el `++` prefijo actualiza el índice antes de escribir. No confundirlo con `vector[tope++]`, que escribiría en -1 al inicio.

**Comprobación del estado:** El dato se apila en el índice recién habilitado.

**Pregunta para comprobar la comprensión:** ¿Qué error se produciría en la primera inserción si usáramos vector[tope++] en lugar de vector[++tope]?

**Conexión con el siguiente paso:** Recuperar el valor antes de descender el tope.

### Paso 4 — Mantener el contrato de pop vectorial

**Para explicar antes de escribir (lectura sugerida):** Vamos a conservar también la extracción que estudiamos: devolver el último valor insertado, después de comprobar si la pila está vacía. Aquí usaremos una forma abreviada para observar que no cambia el significado.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, dentro de pop() de PilaVector, justo después de cerrar push y antes de cerrar la clase vectorial. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
if (tope == -1) {
            System.out.println("Vector vacío");
            return -1;
        }
        return vector[tope--];
```

**Explicación durante la escritura (puedes leerla al grupo):** Con if (tope == -1) evitamos extraer de una pila vacía y devolvemos menos uno como señal. En una pila válida, return vector[tope--] primero lee el elemento apuntado por el tope y luego disminuye el índice; el doble signo menos aparece detrás de la variable, por eso la operación usa el valor anterior. Es equivalente a guardar int dato = vector[tope], ejecutar tope-- y finalmente return dato. Si la pila contiene diez, veinte y treinta, la primera extracción sigue devolviendo treinta.

**Detalle técnico de referencia:** El guardia evita acceso al índice -1. `vector[tope--]` evalúa la celda actual y después decrementa tope (postdecremento), equivalente a guardar dato, descender tope y devolver dato. Esta abreviación está explicada aquí para que no aparezca como sintaxis desconocida.

**Comprobación del estado:** Primer pop sobre [10,20,30] retorna 30, nuevo tope 20.

**Pregunta para comprobar la comprensión:** ¿Qué devolvería el primer pop después de insertar diez, veinte y treinta?

**Conexión con el siguiente paso:** Construir la segunda implementación ya conocida.

### Paso 5 — Modelar nodos con enlaces

**Para explicar antes de escribir (lectura sugerida):** La segunda implementación no usará posiciones del arreglo. Necesitaremos la clase Nodo, que guarda un entero y un enlace al siguiente nodo. Así podremos comparar un índice con una referencia sin confundir sus responsabilidades.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, después de cerrar PilaVector, crea la clase auxiliar Nodo antes de PilaLista. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
class Nodo {
    int dato;
    Nodo siguiente;
    public Nodo(int dato) {
        this.dato = dato;
    }
}
```

**Explicación durante la escritura (puedes leerla al grupo):** La clase Nodo declara int dato y Nodo siguiente. El constructor recibe un número y asigna this.dato = dato. Aquí no escribimos siguiente = null de forma explícita: Java inicializa automáticamente las referencias de instancia con null. Por eso cada nodo nace sin enlace hasta que push conecte su campo siguiente con el antiguo tope. El contenido es equivalente al del ejercicio enlazado anterior.

**Detalle técnico de referencia:** `Nodo` y `siguiente` conservan exactamente las responsabilidades ya explicadas en EJ04. Aquí no se escribe `siguiente=null` porque los campos de referencia de un objeto Java se inicializan en null por defecto; el estado inicial sigue siendo ausencia de enlace.

**Comprobación del estado:** Cada Nodo recién construido contiene un dato y una referencia siguiente=null.

**Pregunta para comprobar la comprensión:** ¿Por qué un nodo recién construido tiene siguiente en null aunque aquí no lo asignemos?

**Conexión con el siguiente paso:** Enlazar una pila que actualice su tope como referencia.

### Paso 6 — Definir push y pop enlazados

**Para explicar antes de escribir (lectura sugerida):** Ahora construiremos la pila enlazada. Su variable tope será una referencia al nodo situado arriba, de manera que push y pop trabajarán siempre en ese extremo. Queremos que almacene los mismos datos que la pila vectorial, pero mediante enlaces.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, dentro de PilaLista, método push(int dato); el método pop se completa inmediatamente después según el código final. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = tope;
        tope = nuevo;
```

**Explicación durante la escritura (puedes leerla al grupo):** Para apilar, creamos Nodo nuevo con el entero recibido, enlazamos nuevo.siguiente con el antiguo tope y finalmente asignamos tope = nuevo. Cuando desapilamos, primero comprobamos que el tope no sea null, después guardamos tope.dato y movemos tope a tope.siguiente. Esta secuencia deja accesibles los nodos restantes. El fragmento mostrado corresponde a las tres líneas centrales de push; en el programa completo también aparece el método pop de la misma clase.

**Detalle técnico de referencia:** Es el mismo orden probado en EJ04. En pop, `if(tope==null)` protege la lectura; `int dato=tope.dato` captura lo retirado; `tope=tope.siguiente` conserva los demás. La clase usa una referencia tope, sin un límite fijo de tres posiciones.

**Comprobación del estado:** Tras apilar 10,20,30: tope→30→20→10.

**Pregunta para comprobar la comprensión:** ¿Qué pasaría con los nodos anteriores si moviéramos el tope antes de enlazar el nuevo?

**Conexión con el siguiente paso:** Enviar iguales operaciones a ambos objetos.

### Paso 7 — Aplicar operaciones parejas y observar resultados

**Para explicar antes de escribir (lectura sugerida):** Ya podemos crear una pila vectorial y otra enlazada en el mismo main. Para comparar correctamente su comportamiento, vamos a enviarles operaciones equivalentes: primero diez, luego veinte y finalmente treinta. Lo importante es que ambas reciban los mismos datos en el mismo orden.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, dentro de main de EJ07_ComparacionLIFO, después de crear las dos variables pila con nombres distintos: vector y lista. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
PilaVector vector = new PilaVector(3);
        PilaLista lista = new PilaLista();
        vector.push(10);
        lista.push(10);
        vector.push(20);
        lista.push(20);
        vector.push(30);
        lista.push(30);
```

**Explicación durante la escritura (puedes leerla al grupo):** new PilaVector(3) reserva tres posiciones y new PilaLista() crea una pila enlazada vacía. Cada pareja de llamadas push inserta el mismo número en ambas estructuras. Cuando termina este fragmento, el vector tiene treinta en el índice dos y la lista tiene un nodo treinta como tope enlazado con veinte y diez. La coincidencia de valores es deliberada: así podremos atribuir cualquier diferencia posterior a la implementación o a las condiciones de inserción.

**Detalle técnico de referencia:** Se crean dos objetos independientes. Cada pareja push introduce el mismo dato primero en vector y luego en lista. No hay conversión de memoria entre ellos: es una comparación experimental. El nombre local `vector` identifica el objeto PilaVector, distinto del campo `int[] vector` interno.

**Comprobación del estado:** Ambos tienen cima lógica 30, luego 20, luego 10.

**Pregunta para comprobar la comprensión:** ¿Qué valor está en el tope de ambas pilas al finalizar las tres parejas de push?

**Conexión con el siguiente paso:** Retirar de las dos estructuras para comprobar equivalencia.

### Paso 8 — Leer resultados y distinguir límites

**Para explicar antes de escribir (lectura sugerida):** Finalmente compararemos las salidas. En lugar de mirar la disposición interna de cada pila, pediremos dos extracciones a ambas y observaremos qué números devuelve cada una. Si recibieron las mismas inserciones, el principio LIFO exige el mismo orden de retirada.

**Dónde escribirlo:** En `EJ07_ComparacionLIFO.java`, a continuación de las parejas push del mismo main; añade las cuatro instrucciones de impresión y cierra la clase pública. Los fragmentos se ubican dentro de la estructura del código completo que se presenta más adelante; todavía no necesitas compilarlos por separado.

**Escribamos este fragmento:**


```java
System.out.println("Vector pop: " + vector.pop());
        System.out.println("Lista pop: " + lista.pop());
        System.out.println("Vector pop: " + vector.pop());
        System.out.println("Lista pop: " + lista.pop());
```

**Explicación durante la escritura (puedes leerla al grupo):** La primera llamada vector.pop() devuelve treinta y la primera lista.pop() también devuelve treinta. Las segundas extracciones devuelven veinte en ambos casos. Los rótulos Vector pop y Lista pop ayudan a comparar sus resultados en la consola. En la tarea T07 reduciremos la capacidad del vector: entonces algunas inserciones podrán rechazarse, y las salidas podrán diferir porque las pilas habrán aceptado distintos datos, no porque alguna incumpla LIFO.

**Detalle técnico de referencia:** El primer par de pops devuelve 30 en ambos; el segundo par devuelve 20. Cada método altera exclusivamente su objeto correspondiente. En T07 se cambia la capacidad del vector para analizar una secuencia no equivalente por una inserción rechazada, sin concluir que una implementación sea universalmente más rápida.

**Comprobación del estado:** Salida principal: Vector 30 / Lista 30 / Vector 20 / Lista 20.

**Pregunta para comprobar la comprensión:** ¿Por qué podríamos observar valores distintos si la pila vectorial rechaza una inserción?

**Conexión con el siguiente paso:** Resolver T07 con la capacidad restringida.

### Traza manual — estado antes → operación → estado después

Vector índices 0:10,1:20,2:30, tope2; lista 30→20→10. Primer pop ambas 30; segundo pop ambas 20. Distinta representación, misma política.

### Auditoría de todas las líneas funcionales del archivo

La siguiente lectura confirma el objetivo y efecto de cada instrucción **en este ejemplo**; no reemplaza la explicación anterior. Las llaves de cierre se omiten porque solo delimitan los bloques ya descritos.


- **Línea 1, `class PilaVector {`:** Encapsula las operaciones de una pila cuyo almacenamiento es un vector de tamaño fijo.

- **Línea 2, `private int[] vector;`:** Campo privado: almacena las celdas físicas del vector de enteros.

- **Línea 3, `private int tope;`:** Índice privado del elemento superior; -1 representa pila vectorial vacía.

- **Línea 5, `public PilaVector(int capacidad) {`:** Constructor del integrador: el parámetro capacidad define el número máximo de elementos.

- **Línea 6, `vector = new int[capacidad];`:** Reserva celdas de acuerdo con la capacidad explícita del integrador.

- **Línea 7, `tope = -1;`:** Fija estado lógico vacío: todavía no hay un índice ocupado.

- **Línea 10, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 11, `if (tope == vector.length - 1) {`:** Consulta el último índice permitido usando la longitud del arreglo.

- **Línea 12, `System.out.println("Vector lleno");`:** Notifica que la versión vectorial no admite más elementos.

- **Línea 13, `return;`:** Finaliza inmediatamente el método void: las instrucciones posteriores del mismo método no se ejecutan en este caso.

- **Línea 15, `vector[++tope] = dato;`:** Incremento prefijo: primero aumenta el tope y luego escribe el dato en esa celda.

- **Línea 18, `public int pop() {`:** Operación de extracción: retorna el entero de cima o la señal didáctica -1 si está vacía.

- **Línea 19, `if (tope == -1) {`:** Detecta pila vectorial vacía antes de leer vector[-1].

- **Línea 20, `System.out.println("Vector vacío");`:** Notifica que no hay elementos en el vector para devolver.

- **Línea 21, `return -1;`:** Devuelve el centinela -1; este laboratorio excluye -1 como dato válido para evitar ambigüedad.

- **Línea 23, `return vector[tope--];`:** Postdecremento: devuelve el dato de cima y después resta 1 al índice.

- **Línea 27, `class Nodo {`:** Declara la clase de un nodo para guardar un entero y su referencia al siguiente nodo.

- **Línea 28, `int dato;`:** Campo que almacena el entero de un objeto Nodo.

- **Línea 29, `Nodo siguiente;`:** Enlace al nodo inmediatamente inferior de esta pila.

- **Línea 30, `public Nodo(int dato) {`:** Constructor del nodo: recibe el entero que quedará almacenado.

- **Línea 31, `this.dato = dato;`:** Almacena el parámetro en el campo dato del nodo recién construido.

- **Línea 35, `class PilaLista {`:** Encapsula la pila enlazada cuyo acceso principal es una referencia al nodo superior.

- **Línea 36, `private Nodo tope;`:** Referencia privada al primer nodo; null representa pila enlazada vacía.

- **Línea 38, `public void push(int dato) {`:** Operación de inserción: recibe un entero y no devuelve un valor.

- **Línea 39, `Nodo nuevo = new Nodo(dato);`:** Crea el nodo que contendrá el entero recibido para apilar.

- **Línea 40, `nuevo.siguiente = tope;`:** Preserva el tope anterior enlazándolo por debajo del nodo nuevo.

- **Línea 41, `tope = nuevo;`:** Actualiza la cima después de conservar el enlace previo.

- **Línea 44, `public int pop() {`:** Operación de extracción: retorna el entero de cima o la señal didáctica -1 si está vacía.

- **Línea 45, `if (tope == null) {`:** Protege cualquier acceso a dato/pagina de un nodo inexistente.

- **Línea 46, `System.out.println("Lista vacía");`:** Notifica que la cabecera de la pila enlazada no existe.

- **Línea 47, `return -1;`:** Devuelve el centinela -1; este laboratorio excluye -1 como dato válido para evitar ambigüedad.

- **Línea 49, `int dato = tope.dato;`:** Recupera el entero del nodo actual antes de mover la referencia.

- **Línea 50, `tope = tope.siguiente;`:** Desvincula la cabecera lógica; la pila sigue desde el nodo que era segundo.

- **Línea 51, `return dato;`:** Entrega al llamador el valor rescatado antes del cambio de tope.

- **Línea 55, `public class EJ07_ComparacionLIFO {`:** Declara la clase pública ejecutable del EJ07; su nombre coincide con el archivo Java.

- **Línea 56, `public static void main(String[] args) {`:** Punto de entrada: Java ejecuta las instrucciones del cuerpo de main en orden.

- **Línea 57, `PilaVector vector = new PilaVector(3);`:** Crea la primera estructura independiente para comparar las salidas.

- **Línea 58, `PilaLista lista = new PilaLista();`:** Crea la segunda estructura independiente para comparar las salidas.

- **Línea 59, `vector.push(10);`:** Apila este valor en el vector; puede ser rechazado si no queda capacidad.

- **Línea 60, `lista.push(10);`:** Apila este valor en la representación enlazada para mantener la misma secuencia experimental.

- **Línea 61, `vector.push(20);`:** Apila este valor en el vector; puede ser rechazado si no queda capacidad.

- **Línea 62, `lista.push(20);`:** Apila este valor en la representación enlazada para mantener la misma secuencia experimental.

- **Línea 63, `vector.push(30);`:** Apila este valor en el vector; puede ser rechazado si no queda capacidad.

- **Línea 64, `lista.push(30);`:** Apila este valor en la representación enlazada para mantener la misma secuencia experimental.

- **Línea 65, `System.out.println("Vector pop: " + vector.pop());`:** Extrae del vector el último entero aceptado y muestra su valor.

- **Línea 66, `System.out.println("Lista pop: " + lista.pop());`:** Extrae de la lista el último entero apilado y muestra su valor.

- **Línea 67, `System.out.println("Vector pop: " + vector.pop());`:** Extrae del vector el último entero aceptado y muestra su valor.

- **Línea 68, `System.out.println("Lista pop: " + lista.pop());`:** Extrae de la lista el último entero apilado y muestra su valor.


### Archivo completo al terminar el ejemplo

El resultado se presenta **ahora**, después de construir y explicar sus partes. Copia/contrasta este archivo completo si necesitas verificar la ubicación de llaves y métodos.


```java
class PilaVector {
    private int[] vector;
    private int tope;

    public PilaVector(int capacidad) {
        vector = new int[capacidad];
        tope = -1;
    }

    public void push(int dato) {
        if (tope == vector.length - 1) {
            System.out.println("Vector lleno");
            return;
        }
        vector[++tope] = dato;
    }

    public int pop() {
        if (tope == -1) {
            System.out.println("Vector vacío");
            return -1;
        }
        return vector[tope--];
    }
}

class Nodo {
    int dato;
    Nodo siguiente;
    public Nodo(int dato) {
        this.dato = dato;
    }
}

class PilaLista {
    private Nodo tope;

    public void push(int dato) {
        Nodo nuevo = new Nodo(dato);
        nuevo.siguiente = tope;
        tope = nuevo;
    }

    public int pop() {
        if (tope == null) {
            System.out.println("Lista vacía");
            return -1;
        }
        int dato = tope.dato;
        tope = tope.siguiente;
        return dato;
    }
}

public class EJ07_ComparacionLIFO {
    public static void main(String[] args) {
        PilaVector vector = new PilaVector(3);
        PilaLista lista = new PilaLista();
        vector.push(10);
        lista.push(10);
        vector.push(20);
        lista.push(20);
        vector.push(30);
        lista.push(30);
        System.out.println("Vector pop: " + vector.pop());
        System.out.println("Lista pop: " + lista.pop());
        System.out.println("Vector pop: " + vector.pop());
        System.out.println("Lista pop: " + lista.pop());
    }
}
```

### Antes de ejecutar — predicción

1. ¿Qué referencia o índice representa el tope justo antes de la operación principal?
2. ¿Qué valor se incorporará, retirará o rechazará, y por qué?
3. ¿Cuál será la última línea de salida de tu programa? Escribe la respuesta antes de compilar.

### Compilar y ejecutar

Abre la terminal dentro de `EJ07` y ejecuta exactamente:


```bash
javac -encoding UTF-8 EJ07_ComparacionLIFO.java
java EJ07_ComparacionLIFO
```

**Resultado esperado en consola:**

```text
Vector pop: 30
Lista pop: 30
Vector pop: 20
Lista pop: 20
```

**Interpretación:** Vector índices 0:10,1:20,2:30, tope2; lista 30→20→10. Primer pop ambas 30; segundo pop ambas 20. Distinta representación, misma política. Contrasta la consola con tu predicción; si difiere, revisa primero el orden de asignación del tope y la secuencia de main.


### Variación controlada

Con capacidad 3, inserta 40 después de retirar 30; comprueba que ambos devuelven 40 como nuevo último elemento.

### Error o caso límite y corrección razonada

No interpretes un pop distinto como violación de LIFO: con capacidad 2 el vector no apiló 30 ni 40, mientras que la lista sí. Deben compararse estados de inserción efectivamente aceptados.


### Pregunta de comprensión

¿Cómo demostrarías experimentalmente que LIFO se conserva aun cuando cambia la capacidad?


## T07 — Tarea espejo de EJ07

**Enunciado:** Conserva push iniciales 10,20,30, reduce capacidad de vector a 2 y agrega push(40) en ambas pilas; ejecuta dos pop por estructura y explica qué inserciones son rechazadas y por qué los resultados difieren.

**Archivo a modificar:** crea una copia de `EJ07_ComparacionLIFO.java` en la carpeta `T07`. No cambies el nombre de la clase pública.

**Restricción:** conserva el comportamiento de LIFO y los guardias de validación enseñados; modifica solo lo que solicita el enunciado.

**Pista:** Modifica solo new PilaVector(2), inserta el cuarto par push(40) antes de pop y conserva dos pares de extracciones.

**Evidencia:** entrega el archivo Java de tu tarea, una captura o transcripción de salida y una traza breve del tope antes/después.

**Cómo saber si está correcta:** la salida debe coincidir con este resultado, justificado por tu traza (no copies el resultado sin ejecutar):


```text
Vector lleno
Vector lleno
Vector pop: 20
Lista pop: 40
Vector pop: 10
Lista pop: 30
```


---

# Cierre de sesión y evidencias de aprendizaje

Hoy construiste una pila desde un vector y otra desde nodos enlazados. En el vector, el **tope es un índice**: comienza en -1, aumenta antes de escribir y disminuye después de rescatar el dato retirado. En la lista, el **tope es una referencia**: comienza en null, se mueve hacia un nodo nuevo después de conservar el enlace anterior y avanza a `siguiente` al desapilar. Por eso la operación `pop()` recupera el mismo elemento en ambas implementaciones si se aceptaron las mismas inserciones.

Validaste dos casos límite distintos: una pila con vector puede alcanzar su capacidad fija; tanto vector como lista pueden estar vacíos. Los métodos de comprobación deben ejecutarse **antes** del acceso al dato o del cambio de tope. Practicaste el orden de las asignaciones: en un enlace nuevo, preservar primero `nuevo.siguiente` evita perder referencias; al eliminar, rescatar `dato` antes de mover `tope` evita devolver el elemento incorrecto.

También aplicaste LIFO al botón Atrás de un navegador simplificado: la página actual se distingue del historial, que almacena las páginas anteriores. Finalmente comparaste ambos almacenamientos con las mismas operaciones y advertiste por qué una inserción rechazada cambia el resultado experimental sin cambiar la regla LIFO.

Para repasar, vuelve a dibujar las trazas sin mirar el código y explica en voz alta las decisiones de `push`, `pop`, `isFull` y la prueba de vacío. Una entrega correcta incluye siete archivos EJ ejecutables, siete T trabajadas y un breve razonamiento de sus estados. La próxima sesión puede recuperar esta habilidad para analizar otras estructuras, sin asumir conceptos todavía no enseñados.

**Lideratec Academy**: continúa practicando en https://lideratecacademy.com/blog/ y en https://www.youtube.com/@LideratecAcademy.
