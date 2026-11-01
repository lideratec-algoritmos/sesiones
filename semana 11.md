---
course_id: ISIL-30710
session_id: S11
module_id: TEMA-11
course_version: PPT-INSTITUCIONAL
source_origin: PPT
status: validated
---
# Lideratec Academy — Sesión 11: Recursividad en Java

## Propósito de la clase

Imagina que debes calcular el factorial de cinco, sumar los números del uno al cinco o separar las cifras de un entero. Puedes resolver cada ejemplo de manera aislada, pero sería difícil mantener un programa diferente para todos los valores. La recursividad propone otra forma de pensar: resolver el problema actual apoyándonos en una versión más pequeña del mismo problema. Esa idea solo funciona si sabemos detener la reducción en un caso cuyo resultado ya conocemos.

En esta sesión vamos a construir algoritmos recursivos en Java, no solamente a observar programas terminados. Partiremos de un factorial para diferenciar el **caso base** y el **caso recursivo**; estudiaremos por qué un error de progreso puede provocar un `StackOverflowError`; y aplicaremos la relación dividir, resolver y combinar a sumas, potencias y dígitos. También veremos un conteo que imprime mientras desciende por las llamadas. Finalmente compararemos Fibonacci directo con una versión que recuerda resultados para evitar repetir trabajo.

En cada ejemplo escribiremos el código por partes. Antes de introducir una clase, método, condición o variable explicaremos qué dificultad resuelve y por qué aparece en ese momento. Después rastrearemos los valores, anticiparemos la salida, ejecutaremos y contrastaremos lo observado. El propósito final es que puedas explicar qué hace cada llamada y corregir una función defectuosa sin depender de copiar una respuesta automática.

## Resultado de aprendizaje — lectura para la apertura

Al terminar la sesión podrás diseñar y leer una función recursiva en Java identificando su condición de parada, el subproblema que genera y la operación que combina resultados. Por ejemplo, ante la expresión S(5), explicarás que el método reduce la entrada de cinco a cuatro y continúa hasta cero; reconocerás que cada nivel debe recordar su sumando y justificarás cómo aparece el resultado quince durante el retorno. Harás algo semejante con potencias, conteos y cifras. También podrás detectar una función que se aleja de su caso base y explicar por qué esa decisión compromete la pila de llamadas.

Usarás herramientas de IA como apoyo de análisis, pero comprobarás las respuestas con una traza manual, un caso límite y la ejecución real. Como evidencia de dominio, entregarás los programas y las tareas asociadas, mostrarás sus salidas y podrás responder en voz alta qué valor cambia, qué permanece estable y qué instrucción hace terminar cada función.

## Antes de iniciar

Necesitas conocimientos básicos de clases Java, métodos `static`, `main`, `int`, `if`, operadores aritméticos, consola y arreglos simples, trabajados anteriormente en el curso. En esta guía se explicarán de nuevo los elementos que influyen en la recursividad, incluso si ya conoces su sintaxis. No se utilizarán bibliotecas externas, hilos, interfaces gráficas ni frameworks. Las operaciones de `int` pueden desbordarse con entradas grandes: usaremos valores pequeños y controlados.

**Equipo preparado.** Abre una terminal y ejecuta `java -version` y `javac -version`: ambas deben mostrar una versión instalada. En la validación de esta entrega se utilizó OpenJDK 21; los ejemplos emplean sintaxis compatible con JDK 17 o posterior. Si tu equipo ya dispone del JDK del curso, no lo reinstales. Si falta `javac`, solicita o instala un JDK autorizado por tu institución; un entorno solo de ejecución no permite compilar.

**Crear la carpeta.** En el explorador de archivos crea una carpeta llamada `S11_Recursividad`. Ábrela desde el editor o la terminal. Dentro crearás un archivo nuevo por ejemplo, respetando exactamente su nombre de clase pública. En una terminal abierta en esta carpeta, para `EJ01_Factorial.java`, compila con `javac EJ01_Factorial.java` y ejecuta con `java EJ01_Factorial` (sin `.java` ni `.class`). Repite el procedimiento sustituyendo el nombre para cada archivo. Si ves `javac no se reconoce`, revisa la instalación y la ruta del JDK; si aparece `class ... is public, should be declared in a file named ...`, corrige el nombre del archivo.

**Ubicación de las tareas espejo.** Dentro de `S11_Recursividad`, crea las carpetas `estudiante/tareas` y guarda allí cada `Txx_*.java` que construirás a partir del código inicial incluido en esta guía. Para compilar una tarea, abre la terminal en esa carpeta `estudiante/tareas`. No necesitas descargar archivos adicionales para completar las tareas.

**Cómo usar los microbloques.** Cada paso agrega literalmente las líneas mostradas **al final del archivo del mismo EJ**, en orden, conservando la indentación sugerida. Cuando las llaves quedan abiertas, el archivo está intencionalmente incompleto y todavía no debes compilar. Al finalizar cada ejemplo encontrarás el archivo completo para comprobar que ensamblaste los pasos correctamente; esa versión no introduce líneas que no se hayan visto. Nunca pegues dos veces el fragmento completo encima de la construcción previa.

**Regla académica.** No ejecutes recursiones deliberadamente infinitas para producir un error: analiza el defecto de forma estática y trabaja con el programa corregido. Una herramienta de IA puede ayudarte a dibujar la pila, pero la salida y el razonamiento deben verificarse con el código real.

## Propuesta de conducción de 120 minutos

| Tramo | Tiempo | Uso en aula |
|---|---:|---|
| Apertura y preparación | 10 min | Caso base, pila y comprobación del JDK. |
| EJ01 y EJ02 | 25 min | Factorial y diagnóstico del progreso; T01/T02 se inician. |
| EJ03 y EJ04 | 20 min | Suma y potencia: reducción, retorno y comparación. |
| EJ05 y EJ06 | 25 min | Orden de impresión, extracción de dígitos y trabajo guiado. |
| EJ07 y EJ08 | 20 min | Demostración focalizada de negativos y memoización. |
| Predicción, corrección, IA y cierre | 20 min | Revisión de trazas y tareas; entrega restante autónoma. |

Las ocho tareas espejo están disponibles para continuidad después del aula; no se presupone que todas puedan terminarse y comentarse dentro de los 120 minutos.

---

# EJ01 — Construir un factorial recursivo que sabe cuándo detenerse

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

Cuando necesitamos calcular el factorial de un número, podemos multiplicar sucesivamente valores decrecientes. Sin embargo, escribir una multiplicación diferente para cada entrada no constituye un algoritmo reutilizable. Construiremos una clase Java con un método llamado factorial: primero definiremos su responsabilidad y su entrada, después incorporaremos los casos base 0 y 1, y por último haremos que el problema se reduzca de n a n−1. Una función main comprobará el factorial de cinco. La pregunta central es cómo una llamada recursiva puede terminar y regresar con un resultado correcto sin dejar una cadena de llamadas infinita.

## ¿Qué problema concreto resolveremos?

¿Por qué factorial(5) produce 120 y no una cantidad ilimitada de llamadas? La solución debe justificar las llamadas de ida y los retornos.

**Archivo exacto:** `EJ01_Factorial.java`. **Punto de partida:** El archivo se crea vacío en la carpeta de la sesión. Es el ejemplo de base del curso; se utilizará para comprender los dos componentes de toda recursión.

### Paso 1 — Dar nombre al programa y a la operación recursiva

**Antes de escribir, comprendamos por qué.** Partimos de un archivo sin clases. Necesitamos separar el programa que ejecutaremos del cálculo que volverá a llamarse. Por ello abrimos la clase EJ01_Factorial y un método factorial que recibe un entero: la clase identifica el archivo y el método concentra la regla matemática.

**Dónde escribir.** Abre `EJ01_Factorial.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ01_Factorial {
    public static int factorial(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** `public class EJ01_Factorial` exige que el archivo se llame `EJ01_Factorial.java`. La llave `{` inicia la clase. `public static int factorial(int n)` declara un método accesible desde `main`, que devolverá un `int` y recibirá `n`. El segundo `{` abre el cuerpo del método; aún no lo cerramos porque falta la lógica. `static` permite invocarlo sin crear un objeto.

**Estado observado.** Existe la firma del método, pero todavía no puede retornar nada: el código es intencionalmente incompleto y aún no se compila.

**Por qué sigue el próximo bloque.** Para que una llamada no se repita sin fin, primero estableceremos cuándo detenerla.

### Paso 2 — Detener el cálculo cuando ya conocemos la respuesta

**Antes de escribir, comprendamos por qué.** El factorial de cero y el de uno están definidos como uno. Sin una condición que retorne directamente, el método no tendría un punto de salida y seguiría generando llamadas. Dentro de factorial, inmediatamente después de su llave de apertura, colocamos la comprobación del caso base.

**Dónde escribir.** Abre `EJ01_Factorial.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n == 0 || n == 1) {
            return 1;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `if` toma una decisión; `n == 0 || n == 1` es verdadera si n vale 0 o 1. `==` compara, no asigna; `||` significa «o». `return 1` entrega el valor al llamador y termina esa invocación. La llave final cierra solo el `if`, no el método factorial. Ninguna rama vuelve a llamar a factorial.

**Estado observado.** Si n=1, la llamada termina con 1. Si n=5, todavía falta la instrucción para continuar reduciendo el problema.

**Por qué sigue el próximo bloque.** Ahora podemos resolver valores mínimos; construiremos la regla general que usa ese caso para resolver n mayor.

### Paso 3 — Reducir n y combinar al retornar

**Antes de escribir, comprendamos por qué.** Cuando n es mayor que uno necesitamos representar n! como n por (n−1)!. Es exactamente el subproblema que nuestro propio método sabe resolver. Añadimos la línea al final de factorial, después del bloque if; luego cerramos el método.

**Dónde escribir.** Abre `EJ01_Factorial.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return n * factorial(n - 1);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `factorial(n - 1)` inicia otra invocación con una entrada menor. La multiplicación `n * ...` espera el resultado de esa llamada; por eso el cálculo se completa al regresar, no cuando se desciende. `return` entrega el producto al nivel anterior. La llave `}` cierra factorial y deja la clase todavía abierta para crear main.

**Estado observado.** Para n=2, la llamada factorial(1) devuelve 1 y el producto es 2. Para n=5, habrá una cadena de llamadas pendientes que se resolverán en orden inverso.

**Por qué sigue el próximo bloque.** Sabemos calcular, pero necesitamos un punto de entrada que muestre el resultado en la consola.

### Paso 4 — Crear el punto de entrada y observar el valor devuelto

**Antes de escribir, comprendamos por qué.** Hasta ahora hemos escrito un método de cálculo, pero ejecutar una clase requiere un punto de entrada. Dentro de la clase, a continuación de factorial, agregamos main; allí solicitaremos un ejemplo verificable y cerraremos el programa.

**Dónde escribir.** Abre `EJ01_Factorial.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("Factorial de 5: " + factorial(5));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `public static void main(String[] args)` es el punto de entrada de la aplicación. `System.out.println` envía texto a consola; el signo `+` concatena la etiqueta con el entero retornado por `factorial(5)`. La primera `}` cierra main y la segunda cierra la clase. No se añade código oculto: concatenar los cuatro microbloques forma exactamente el archivo final.

**Estado observado.** La clase está completa; `javac EJ01_Factorial.java` debe compilar y `java EJ01_Factorial` debe imprimir 120.

**Por qué sigue el próximo bloque.** Con este patrón distinguiremos a continuación qué sucede cuando una recursión no se aproxima a un caso base.

## Traza manual antes de ejecutar

factorial(5) → 5×factorial(4) → 4×factorial(3) → 3×factorial(2) → 2×factorial(1) → 2×1 → 6 → 24 → 120.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ01_Factorial {
    public static int factorial(int n) {
        if (n == 0 || n == 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }
    public static void main(String[] args) {
        System.out.println("Factorial de 5: " + factorial(5));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Antes de ejecutar, predice si el método realizará cinco multiplicaciones o cuatro y explica qué devuelve el caso n=1.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ01_Factorial.java
java EJ01_Factorial
```
**Salida esperada:**

```text
Factorial de 5: 120
```
**Interpretación técnica.** El resultado se forma al regresar de la llamada más pequeña. El caso base devuelve 1 sin una nueva llamada; por eso las llamadas pendientes pueden completar sus multiplicaciones.

**Variación controlada.** Cambiar la llamada del main a factorial(0) y factorial(1); en ambos casos se espera 1. Comparar la cantidad de llamadas frente a factorial(5).

**Error frecuente o límite.** Si el caso base se elimina, la expresión factorial(n-1) continúa reduciendo el valor hacia negativos: puede terminar en StackOverflowError. No ejecutar esa variante insegura.

**Corrección razonada.** Restaurar el caso base antes de ejecutar; comprobar que en cada llamada válida n disminuye y finalmente llega a 1 o 0.

**Explícalo con tus palabras.** ¿Para qué sirve el valor devuelto por factorial(1) si el factorial solicitado es 5?

## T01 — Tarea espejo: Probar factorial con dos casos base explícitos

**Enunciado.** Escribir el factorial mostrando por separado el tratamiento de 0 y de 1, y verificar tres entradas; esto distingue dos retornos directos de la regla recursiva. Crear factorial con `if(n==0)` y `if(n==1)` separados. Después mostrar los valores de 0, 1 y 6. Para n negativo, conservar el requisito «entrada no negativa» mediante la validación enseñada.

**Archivo para crear o editar.** `T01_FactorialDosBases.java`. En `estudiante/tareas/T01_FactorialDosBases.java`, sustituir el marcador TODO por las dos condiciones base y la llamada recursiva; no borrar la comprobación para negativos.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T01_FactorialDosBases {
    public static int factorial(int n) {
        if (n < 0) throw new IllegalArgumentException("n debe ser no negativo");
        // TODO T01: añade caso n=0, caso n=1 y llamada n * factorial(n-1).
        return -1; // marcador temporal, reemplazarlo al resolver T01
    }
    public static void main(String[] args) {
        System.out.println("F(0)=" + factorial(0));
        System.out.println("F(1)=" + factorial(1));
        System.out.println("F(6)=" + factorial(6));
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Recuerda que factorial(1) aporta el uno inicial para todas las multiplicaciones superiores; factorial(0) también debe devolver uno.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
F(0)=1
F(1)=1
F(6)=720
```
**Cómo comprobarlo.** Compila con `javac T01_FactorialDosBases.java` y ejecuta con `java T01_FactorialDosBases`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ02 — Diagnosticar una recursión sin progreso y construir una corrección

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

Una función puede tener una condición de salida y aun así fallar si su llamada recursiva nunca se acerca a esa condición. Veremos primero, sin ejecutar, un patrón de error en el que el argumento crece cuando debería disminuir. Después escribiremos un programa distinto y seguro para contar cuántos pasos faltan hasta llegar a cero desde un entero no negativo. Añadiremos una validación de entrada, un caso base y un retorno que contabiliza los pasos. El resultado para tres será tres. Este ejercicio permite explicar la relación entre progreso, pila de ejecución y StackOverflowError antes de analizar problemas más extensos.

## ¿Qué problema concreto resolveremos?

Una llamada incorrecta `pasosHastaCero(n + 1)` desde n positivo se aleja de cero. ¿Cómo detectar el defecto antes de ejecutar un programa potencialmente interminable?

**Archivo exacto:** `EJ02_CasoBaseSeguro.java`. **Punto de partida:** Analizar de manera estática el fragmento incorrecto `return 1 + pasosHastaCero(n + 1);` cuando el objetivo es llegar a cero. No copiar ni ejecutar esa línea en los archivos principales.

### Paso 1 — Crear una operación que cuente pasos

**Antes de escribir, comprendamos por qué.** Queremos devolver un número de pasos, no imprimir una secuencia. En un archivo nuevo declaramos una clase y un método de retorno entero; el parámetro n es el estado que irá reduciéndose.

**Dónde escribir.** Abre `EJ02_CasoBaseSeguro.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ02_CasoBaseSeguro {
    public static int pasosHastaCero(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** El nombre de clase coincide con `EJ02_CasoBaseSeguro.java`. `static` permite llamar al método desde main. `int n` contiene el punto de partida; cada llamada tendrá su propia copia de n en la pila de ejecución. Las dos llaves abiertas indican que debemos completar el método y la clase.

**Estado observado.** Existe un lugar para el algoritmo; aún no hemos definido entradas válidas ni retorno.

**Por qué sigue el próximo bloque.** Antes de contar pasos debemos impedir estados iniciales que se alejan de cero.

### Paso 2 — Rechazar entradas que no pueden alcanzar cero disminuyendo

**Antes de escribir, comprendamos por qué.** Si comenzamos en un valor negativo y restamos uno, nunca alcanzamos cero. Para que el contrato del método sea verdadero, validamos la entrada al inicio del cuerpo antes de permitir cualquier recursión.

**Dónde escribir.** Abre `EJ02_CasoBaseSeguro.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
```
**Qué hace cada instrucción y por qué está aquí.** `n < 0` detecta números negativos. `throw new IllegalArgumentException(...)` interrumpe la ejecución informando que el valor no cumple el requisito. Este mecanismo representa un error de uso, no un caso base. La llave cierra solo la comprobación. Una llamada negativa no debe quedar intentando resolver un subproblema imposible.

**Estado observado.** Los negativos ya no generan cadenas; para n=0 o positivo el flujo continúa a la siguiente condición.

**Por qué sigue el próximo bloque.** Ahora falta distinguir el estado final que no requiere nuevos pasos.

### Paso 3 — Terminar la cadena al llegar a cero

**Antes de escribir, comprendamos por qué.** El estado n=0 ya alcanzó el objetivo. A diferencia del factorial, aquí no queda ninguna operación por realizar y el número de pasos restantes es cero. Colocamos esta condición inmediatamente después de la validación.

**Dónde escribir.** Abre `EJ02_CasoBaseSeguro.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n == 0) {
            return 0;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `==` comprueba igualdad exacta. `return 0` finaliza la llamada actual y proporciona el valor al nivel anterior; no es un cero arbitrario, sino la cantidad de pasos pendientes desde la meta. La llave cierra este segundo if. Si n>0, el flujo prosigue.

**Estado observado.** Para n=0 el método devuelve 0. Para n=1 necesitamos todavía contabilizar un paso y hacer otra llamada.

**Por qué sigue el próximo bloque.** Añadiremos una llamada que efectivamente se acerque al caso base.

### Paso 4 — Contar un paso y reducir el problema

**Antes de escribir, comprendamos por qué.** Si n es positivo, una transición lo convierte en n−1. El método debe sumar ese paso al número de pasos que queden desde el nuevo estado. Esta es la única ruta recursiva válida de la implementación.

**Dónde escribir.** Abre `EJ02_CasoBaseSeguro.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return 1 + pasosHastaCero(n - 1);
    }
```
**Qué hace cada instrucción y por qué está aquí.** El `1` representa la transición actual. `pasosHastaCero(n - 1)` reduce estrictamente el argumento. La suma espera el retorno del nivel menor y luego entrega el total con `return`. La llave siguiente cierra el método, conservando abierta la clase.

**Estado observado.** Desde n=3 los estados de entrada son 3, 2, 1 y 0; los retornos se forman 0, 1, 2 y 3.

**Por qué sigue el próximo bloque.** Con la lógica ya segura, agregaremos una invocación visible desde la consola.

### Paso 5 — Mostrar los pasos calculados sin provocar el error

**Antes de escribir, comprendamos por qué.** La demostración debe ser segura y reproducible. Agregamos main debajo del método para llamar con un valor válido; la línea incorrecta permanece como ejemplo de diagnóstico en el material escrito.

**Dónde escribir.** Abre `EJ02_CasoBaseSeguro.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("Pasos desde 3: " + pasosHastaCero(3));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `main` inicia el programa; `println` combina una etiqueta con el número retornado. `pasosHastaCero(3)` activa las reducciones previstas. La primera llave cierra main y la segunda la clase. Al terminar, el archivo contiene únicamente la variante corregida.

**Estado observado.** La clase compila y produce `Pasos desde 3: 3`; la línea de crecimiento incorrecta no forma parte de este programa.

**Por qué sigue el próximo bloque.** Ahora usaremos el mismo patrón de progresión para resolver una suma que combina valores al regresar.

## Traza manual antes de ejecutar

pasosHastaCero(3) → 1+pasosHastaCero(2) → 1+1+pasosHastaCero(1) → 1+1+1+pasosHastaCero(0) → 3.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ02_CasoBaseSeguro {
    public static int pasosHastaCero(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
        if (n == 0) {
            return 0;
        }
        return 1 + pasosHastaCero(n - 1);
    }
    public static void main(String[] args) {
        System.out.println("Pasos desde 3: " + pasosHastaCero(3));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice el resultado para 3 y explica por qué la entrada negativa no puede «alcanzar cero» restando 1.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ02_CasoBaseSeguro.java
java EJ02_CasoBaseSeguro
```
**Salida esperada:**

```text
Pasos desde 3: 3
```
**Interpretación técnica.** Cada llamada aporta un paso y resta uno. El caso n=0 no genera llamadas: devuelve el cero que permite sumar los pasos acumulados.

**Variación controlada.** Examinar en papel `n=1`, `n=0` y `n=-1`; justificar cuál llega al caso base y cuál exige rechazar la entrada.

**Error frecuente o límite.** El error frecuente es modificar el argumento como n+1 o no validar n<0. Ambos casos se alejan de cero y pueden desbordar la pila si se ejecutan durante suficientes llamadas.

**Corrección razonada.** Mantener guardia de negativos y usar exclusivamente `n - 1` para los valores positivos. Diagnosticar la variante errónea en papel, sin ejecutarla.

**Explícalo con tus palabras.** ¿Basta con que exista `if(n==0)` para asegurar que una recursión termine?

## T02 — Tarea espejo: Cambiar la meta del conteo recursivo a uno

**Enunciado.** Diseñar una reducción con caso base distinto: contar las transiciones necesarias para llegar a uno desde n positivo. Partiendo de EJ02, ahora n=1 debe devolver cero pasos y n<=0 debe rechazarse. Para n=4 se esperan tres pasos (4→3→2→1).

**Archivo para crear o editar.** `T02_PasosHastaUno.java`. En `estudiante/tareas/T02_PasosHastaUno.java`, reemplazar el TODO por el caso base n==1 y el retorno que suma un paso y llama con n-1.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T02_PasosHastaUno {
    public static int pasosHastaUno(int n) {
        if (n <= 0) throw new IllegalArgumentException("n debe ser positivo");
        // TODO T02: caso base en 1 y paso de reducción n-1.
        return -1; // marcador temporal que se reemplaza
    }
    public static void main(String[] args) {
        System.out.println("Pasos hasta 1 desde 4: " + pasosHastaUno(4));
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Cambia primero la condición de parada y verifica si la modificación del argumento sigue acercándose a ella.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
Pasos hasta 1 desde 4: 3
```
**Cómo comprobarlo.** Compila con `javac T02_PasosHastaUno.java` y ejecuta con `java T02_PasosHastaUno`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ03 — Sumar números naturales descomponiendo S(n)

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

La fórmula S(n)=n+(n−1)+…+1 aparece en la presentación como una aplicación del principio de reducción. Convertiremos esa expresión en código Java manteniendo una correspondencia directa: la entrada n representa el número hasta el que queremos sumar, cero será nuestro caso base y el retorno combinará n con la suma de valores menores. Antes de ejecutar reconstruiremos en papel S(5) y compararemos las llamadas de ida con los retornos de vuelta. Incorporaremos una validación mínima para no admitir valores negativos. Al final comprobaremos que S(5) produce quince y podremos justificar por qué el recorrido tiene profundidad proporcional a n.

## ¿Qué problema concreto resolveremos?

¿Cómo calcular S(5) sin escribir cinco sumas distintas y sin confundir la reducción del problema con la combinación del resultado?

**Archivo exacto:** `EJ03_SumaNatural.java`. **Punto de partida:** Construir `EJ03_SumaNatural.java` en la carpeta de ejemplos. La regla de reducción `S(n)=n+S(n−1)` procede de la presentación.

### Paso 1 — Crear un método que devolverá la suma total

**Antes de escribir, comprendamos por qué.** Para traducir la notación S(n) necesitamos un método que reciba n y devuelva la suma completa. Partimos de un archivo nuevo para no mezclar el cálculo con los ejemplos anteriores.

**Dónde escribir.** Abre `EJ03_SumaNatural.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ03_SumaNatural {
    public static int sumar(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** La clase debe coincidir con el archivo. `int sumar(int n)` promete devolver un entero, no una secuencia impresa. `n` indica el último sumando; el valor cambia entre invocaciones. Las llaves abren clase y método, que se completarán gradualmente.

**Estado observado.** Todavía no hay un retorno; el archivo no compila de manera independiente.

**Por qué sigue el próximo bloque.** Fijaremos qué entradas tienen sentido para la suma de naturales.

### Paso 2 — Validar la entrada antes de empezar a reducir

**Antes de escribir, comprendamos por qué.** La recurrencia de la actividad está definida para enteros no negativos. Si n fuera negativo, restar uno lo alejaría del caso base. Impediremos esa trayectoria desde el comienzo.

**Dónde escribir.** Abre `EJ03_SumaNatural.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
```
**Qué hace cada instrucción y por qué está aquí.** El if identifica números negativos, y la excepción señala al llamador que la entrada es inválida. Esta comprobación no aporta un sumando; solo protege el contrato del método. Cierra únicamente la validación, no el método.

**Estado observado.** Para valores n>=0, se continúa; para negativos no hay ejecución recursiva.

**Por qué sigue el próximo bloque.** Debemos dar un valor conocido que permita empezar a completar las sumas.

### Paso 3 — Representar S(0) como suma vacía

**Antes de escribir, comprendamos por qué.** S(0) no contiene sumandos y su total es cero. Esa respuesta conocida es el punto de detención. La añadimos después de validar para separar error de terminación normal.

**Dónde escribir.** Abre `EJ03_SumaNatural.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n == 0) {
            return 0;
        }
```
**Qué hace cada instrucción y por qué está aquí.** La condición compara n con cero. `return 0` entrega directamente el elemento neutro de la suma; cuando un nivel superior lo reciba podrá calcular 1+0. No se produce ninguna llamada desde este caso.

**Estado observado.** La suma para cero ya funciona; para cinco debemos reducir primero a cuatro.

**Por qué sigue el próximo bloque.** Agregaremos la relación S(n)=n+S(n−1).

### Paso 4 — Aplicar literalmente la recurrencia de la suma

**Antes de escribir, comprendamos por qué.** Para n positivo sumamos el valor actual a la suma de todos los anteriores. Con una llamada a nuestro propio método evitamos escribir una cantidad fija de operaciones.

**Dónde escribir.** Abre `EJ03_SumaNatural.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return n + sumar(n - 1);
    }
```
**Qué hace cada instrucción y por qué está aquí.** La subexpresión `sumar(n-1)` crea el subproblema menor. El `+ n` realiza la combinación. `return` devuelve el total al llamador y la llave cierra el método. A diferencia de un bucle, aquí cada invocación conserva su n mientras espera la respuesta.

**Estado observado.** Para 3, la pila conceptual baja 3→2→1→0 y regresa con 0→1→3→6.

**Por qué sigue el próximo bloque.** Comprobaremos el resultado con S(5) desde main.

### Paso 5 — Ejecutar la suma de cinco

**Antes de escribir, comprendamos por qué.** Ya conocemos la estructura y el algoritmo. Solo falta pedir una entrada concreta y mostrar lo que retorna para cotejarlo con la cuenta manual.

**Dónde escribir.** Abre `EJ03_SumaNatural.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("Suma 1..5: " + sumar(5));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** La firma main indica el punto de ejecución. `println` antepone una etiqueta para reconocer la prueba. La expresión `sumar(5)` inicia el árbol de llamadas, y las últimas dos llaves cierran main y la clase. Ninguna instrucción de cálculo queda por fuera del método.

**Estado observado.** El archivo puede compilar; la consola debe informar 15.

**Por qué sigue el próximo bloque.** Con la suma observada, pasaremos a una recurrencia cuya combinación es una multiplicación.

## Traza manual antes de ejecutar

S(5)=5+S(4)=5+4+S(3)=5+4+3+S(2)=5+4+3+2+S(1)=15. El caso S(0)=0 permite comenzar los retornos.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ03_SumaNatural {
    public static int sumar(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
        if (n == 0) {
            return 0;
        }
        return n + sumar(n - 1);
    }
    public static void main(String[] args) {
        System.out.println("Suma 1..5: " + sumar(5));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice S(5), indica el valor de S(0) y explica el significado del signo + a la izquierda de la llamada.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ03_SumaNatural.java
java EJ03_SumaNatural
```
**Salida esperada:**

```text
Suma 1..5: 15
```
**Interpretación técnica.** La suma pendiente se resuelve al regresar: para n=1, retorna 1; luego 3, 6, 10 y 15. La profundidad crece linealmente con n en esta solución.

**Variación controlada.** Sustituye 5 por 1 y 0 y confirma las salidas 1 y 0; describe las llamadas necesarias.

**Error frecuente o límite.** Si se retorna solamente `sumar(n-1)`, el resultado omite n en cada nivel; para cinco devolvería cero.

**Corrección razonada.** Restaurar `n + sumar(n-1)` y confirmar que la combinación expresa exactamente S(n)=n+S(n−1).

**Explícalo con tus palabras.** ¿Cuándo se suma el cinco: antes de resolver S(4) o después de conocer su resultado?

## T03 — Tarea espejo: Sumar únicamente números naturales pares

**Enunciado.** Cambiar la reducción de uno en uno a dos en dos para sumar 2+4+6 y practicar una recurrencia con subproblemas distintos. Implementar sumarPares(n) para n par y no negativo: para 6, calcular 6+4+2=12. Rechazar n impar o negativo, conservar caso base 0 y avanzar usando n−2.

**Archivo para crear o editar.** `T03_SumaPares.java`. En `estudiante/tareas/T03_SumaPares.java`, reemplaza TODO: base n==0 y `n + sumarPares(n-2)`. La condición de entrada ya restringe a n par.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T03_SumaPares {
    public static int sumarPares(int n) {
        if (n < 0 || n % 2 != 0) throw new IllegalArgumentException("n par no negativo");
        // TODO T03: caso 0 y suma con reducción de dos en dos.
        return -1; // marcador temporal que se reemplaza
    }
    public static void main(String[] args) {
        System.out.println("Pares hasta 6: " + sumarPares(6));
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** La nueva diferencia de razonamiento es que el subproblema reduce n dos unidades para no agregar impares.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
Pares hasta 6: 12
```
**Cómo comprobarlo.** Compila con `javac T03_SumaPares.java` y ejecuta con `java T03_SumaPares`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ04 — Calcular potencias positivas disminuyendo el exponente

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

La presentación muestra que una potencia puede descomponerse como a elevado a n igual a a por a elevado a n menos uno. Usaremos esa relación para crear un método con dos entradas: la base a, que no cambia entre llamadas, y el exponente n, que disminuye hasta cero. El caso base entregará uno, porque cualquier base no nula elevada a cero vale uno en el alcance del ejercicio. La función main mostrará 2 elevado a 4 igual a 16. El énfasis será distinguir el dato que permanece constante del que marca el progreso de la recursión y comprender por qué la multiplicación se completa al retornar.

## ¿Qué problema concreto resolveremos?

¿Qué dato debe reducirse para calcular 2^4 de forma recursiva: la base dos o el exponente cuatro?

**Archivo exacto:** `EJ04_Potencia.java`. **Punto de partida:** En el archivo nuevo se implementa la fórmula `a^n = a × a^(n−1)` de la sesión para exponentes enteros no negativos.

### Paso 1 — Definir una operación con dos parámetros de distinta función

**Antes de escribir, comprendamos por qué.** Para calcular a^n necesitamos conservar la base y reducir la cantidad de multiplicaciones. En un archivo nuevo creamos un método con dos parámetros para que cada uno tenga una responsabilidad explícita.

**Dónde escribir.** Abre `EJ04_Potencia.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ04_Potencia {
    public static int potencia(int a, int n) {
```
**Qué hace cada instrucción y por qué está aquí.** `int a` es la base que se multiplicará; `int n` es el exponente restante. El retorno `int` contendrá el resultado para ejemplos pequeños que no desborden el rango entero. `static` permite invocar desde main. Las llaves abren clase y método; falta el cuerpo.

**Estado observado.** El método recibe dos valores, pero aún no distingue cuáles son válidos.

**Por qué sigue el próximo bloque.** Antes de iniciar el proceso evitaremos exponentes negativos, que no corresponden al algoritmo enseñado.

### Paso 2 — Restringir el exponente al dominio explicado

**Antes de escribir, comprendamos por qué.** La regla que implementaremos termina en exponente cero por sustracciones sucesivas de uno. Para exponentes negativos esa trayectoria nunca llega a cero y requeriría otro tratamiento matemático.

**Dónde escribir.** Abre `EJ04_Potencia.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n < 0) {
            throw new IllegalArgumentException("exponente no negativo requerido");
        }
```
**Qué hace cada instrucción y por qué está aquí.** `if(n < 0)` comprueba la precondición sobre n, no sobre a. La excepción evita una recursión sin progreso. El cierre del if deja disponible el método para entradas válidas. Una base negativa sí se permite, porque solo cambia el signo de multiplicaciones.

**Estado observado.** Un exponente no negativo seguirá adelante; una entrada negativa se rechaza con un mensaje explícito.

**Por qué sigue el próximo bloque.** Ahora necesitamos resolver directamente el exponente cero.

### Paso 3 — Dar un valor directo al exponente cero

**Antes de escribir, comprendamos por qué.** La potencia con exponente cero sirve de ancla de la recurrencia. Sin ese uno, multiplicar al retornar no recuperaría los valores correctos.

**Dónde escribir.** Abre `EJ04_Potencia.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n == 0) {
            return 1;
        }
```
**Qué hace cada instrucción y por qué está aquí.** La comparación `n==0` identifica la condición de parada. `return 1` aporta el valor que no altera multiplicaciones posteriores. Esta condición no hace una llamada recursiva; la llave cierra el if.

**Estado observado.** potencia(2,0) retorna uno de inmediato y potencia(2,1) todavía necesita una multiplicación.

**Por qué sigue el próximo bloque.** Expresaremos ahora el paso de n a n−1 y la combinación.

### Paso 4 — Conservar la base y reducir únicamente el exponente

**Antes de escribir, comprendamos por qué.** La fórmula recursiva dice que falta una multiplicación por a y un subproblema con exponente menor. Escribimos exactamente esa relación después del caso base.

**Dónde escribir.** Abre `EJ04_Potencia.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return a * potencia(a, n - 1);
    }
```
**Qué hace cada instrucción y por qué está aquí.** La primera `a` multiplica el resultado; la segunda `a` se reenvía sin alteración. Solo `n-1` decrece. El retorno combina al regresar; la llave cierra el método. Para exponentes grandes, la recursión y el resultado entero tienen límites prácticos.

**Estado observado.** Para (2,2) se calcula 2×potencia(2,1), después 2×2×potencia(2,0), y se obtiene 4.

**Por qué sigue el próximo bloque.** Crearemos una llamada de prueba que muestre el resultado prometido.

### Paso 5 — Comprobar 2 elevado a 4

**Antes de escribir, comprendamos por qué.** Con la recurrencia completa, necesitamos demostrar que la combinación de los retornos produce la potencia conocida. Añadimos main dentro de la clase.

**Dónde escribir.** Abre `EJ04_Potencia.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("2^4 = " + potencia(2, 4));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `potencia(2,4)` envía base y exponente en el orden declarado. `println` imprime la etiqueta y el entero. Las llaves completan main y la clase. La prueba usa valores pequeños para que el cálculo quepa en int.

**Estado observado.** La salida será 2^4 = 16.

**Por qué sigue el próximo bloque.** La siguiente variante mostrará que no todos los métodos recursivos combinan un retorno numérico.

## Traza manual antes de ejecutar

potencia(2,4) = 2×potencia(2,3) = 2×2×potencia(2,2) = 2×2×2×potencia(2,1) = 2×2×2×2×potencia(2,0) = 16.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ04_Potencia {
    public static int potencia(int a, int n) {
        if (n < 0) {
            throw new IllegalArgumentException("exponente no negativo requerido");
        }
        if (n == 0) {
            return 1;
        }
        return a * potencia(a, n - 1);
    }
    public static void main(String[] args) {
        System.out.println("2^4 = " + potencia(2, 4));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice potencia(2,4) y potencia(7,0); identifica cuál parámetro se modifica entre llamadas.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ04_Potencia.java
java EJ04_Potencia
```
**Salida esperada:**

```text
2^4 = 16
```
**Interpretación técnica.** La base a sigue valiendo 2. El exponente disminuye 4,3,2,1,0 y cada retorno multiplica por 2.

**Variación controlada.** Probar potencia(-2,3) y explicar el signo del resultado esperado -8; el algoritmo es el mismo.

**Error frecuente o límite.** Devolver 0 en n=0 haría que todas las potencias positivas diesen 0, porque la multiplicación terminaría propagando ese cero.

**Corrección razonada.** El caso base debe retornar 1; dejar n−1 como avance del exponente.

**Explícalo con tus palabras.** ¿Qué sucede si se reduce la base en vez del exponente?

## T04 — Tarea espejo: Validar exponentes pares, impares y cero

**Enunciado.** Utilizar la misma recurrencia de EJ04 para comparar cómo el signo de la base influye en la potencia según la paridad del exponente. Completar el algoritmo de potencia y probar tres casos con base −2: exponentes 3, 4 y 0. Explicar sus resultados sin introducir otro algoritmo.

**Archivo para crear o editar.** `T04_PotenciasSigno.java`. En `estudiante/tareas/T04_PotenciasSigno.java` reemplaza TODO por el caso base n==0 y la expresión `a * potencia(a,n-1)`.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T04_PotenciasSigno {
    public static int potencia(int a, int n) {
        if (n < 0) throw new IllegalArgumentException("exponente no negativo");
        // TODO T04: caso base del exponente y recurrencia.
        return -1; // marcador temporal que se reemplaza
    }
    public static void main(String[] args) {
        System.out.println("(-2)^3 = " + potencia(-2, 3));
        System.out.println("(-2)^4 = " + potencia(-2, 4));
        System.out.println("(-2)^0 = " + potencia(-2, 0));
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Mantén fija la base -2 en cada llamada; en el regreso se multiplica una vez por cada exponente positivo.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
(-2)^3 = -8
(-2)^4 = 16
(-2)^0 = 1
```
**Cómo comprobarlo.** Compila con `javac T04_PotenciasSigno.java` y ejecuta con `java T04_PotenciasSigno`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ05 — Mostrar un conteo regresivo y observar el orden de ejecución

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

Hasta ahora, las funciones recursivas devolvían números que se combinaban después de otras llamadas. Sin embargo, la recursividad también sirve para ejecutar acciones en una secuencia. Construiremos un procedimiento `contar` que imprima desde cuatro hasta uno. La salida aparecerá antes de la siguiente llamada; por ello los números se verán en orden descendente. El caso base se detendrá cuando n sea cero o negativo. Este ejemplo permitirá distinguir el descenso por la pila de llamadas del regreso y explicar por qué el lugar de una instrucción de impresión altera el orden observable sin cambiar el caso base.

## ¿Qué problema concreto resolveremos?

¿Qué ocurre si escribimos `println(n)` antes o después de la llamada recursiva?

**Archivo exacto:** `EJ05_Conteo.java`. **Punto de partida:** El ejemplo reproduce el conteo regresivo del material. No requiere retornar un valor numérico.

### Paso 1 — Crear un procedimiento sin resultado numérico

**Antes de escribir, comprendamos por qué.** Nuestro objetivo es producir varias líneas en la consola, no calcular un entero. Por eso el nuevo método se declarará con retorno `void`, que significa que no entregará un valor.

**Dónde escribir.** Abre `EJ05_Conteo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ05_Conteo {
    public static void contar(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** `contar` recibe n como siguiente número a mostrar. `public static void` declara que el método puede llamarse desde main y no devuelve un valor. Se abren la clase y el método; la instrucción de salida llegará después.

**Estado observado.** Existe un procedimiento vacío, todavía sin caso base.

**Por qué sigue el próximo bloque.** Agregaremos una condición que cierre la recursión antes de mostrar números fuera de rango.

### Paso 2 — Detener la impresión cuando se agotan los números

**Antes de escribir, comprendamos por qué.** Queremos mostrar solo enteros positivos. Una vez lleguemos a cero, no tiene sentido seguir imprimiendo ni hacer nuevas llamadas. Antes de emitir cualquier línea, comprobamos el límite.

**Dónde escribir.** Abre `EJ05_Conteo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n <= 0) {
            return;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `n <= 0` significa menor o igual a cero. `return;` sin valor termina un método void. Esto también protege si accidentalmente se llama con un negativo. La llave cierra el if y deja libre el flujo para n positivo.

**Estado observado.** Con n=0 no se imprime nada; con n=4 falta el comportamiento principal.

**Por qué sigue el próximo bloque.** Ahora decidiremos cuándo mostrar el valor respecto a la siguiente llamada.

### Paso 3 — Imprimir el valor actual antes de descender

**Antes de escribir, comprendamos por qué.** Para lograr 4,3,2,1 necesitamos mostrar el número actual antes de reducirlo. Después de la condición anterior escribimos una salida y luego la llamada recursiva.

**Dónde escribir.** Abre `EJ05_Conteo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        System.out.println(n);
        contar(n - 1);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `System.out.println(n)` usa el n propio de esa invocación, por ejemplo cuatro. `contar(n-1)` llama con el siguiente número menor. La llave cierra el método. Como el print está antes, la primera línea aparecerá cuando n todavía valga cuatro; no hay suma pendiente durante el regreso.

**Estado observado.** La ejecución conceptual imprime 4 al entrar, después 3, 2 y 1; n=0 retorna sin salida.

**Por qué sigue el próximo bloque.** Solo falta iniciar el procedimiento desde main y observar el orden.

### Paso 4 — Llamar al procedimiento desde main

**Antes de escribir, comprendamos por qué.** Como contar no devuelve un valor, no lo concatenaremos dentro de otro println. Su llamada producirá las líneas directamente. Agregamos main y cerramos la clase.

**Dónde escribir.** Abre `EJ05_Conteo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        contar(4);
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `contar(4)` inicia la secuencia. Las llaves finales cierran main y la clase. Al ejecutar, el orden de cada println refleja el avance por los diferentes valores de n.

**Estado observado.** Cuatro líneas: 4, 3, 2 y 1.

**Por qué sigue el próximo bloque.** En el siguiente ejemplo volveremos a combinar valores numéricos, esta vez extrayendo dígitos.

## Traza manual antes de ejecutar

contar(4): imprime 4; llama contar(3): imprime 3; llama contar(2): imprime 2; llama contar(1): imprime 1; llama contar(0): no imprime y retorna.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ05_Conteo {
    public static void contar(int n) {
        if (n <= 0) {
            return;
        }
        System.out.println(n);
        contar(n - 1);
    }
    public static void main(String[] args) {
        contar(4);
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice las cuatro líneas impresas; explica si hay una suma o multiplicación esperando en el retorno.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ05_Conteo.java
java EJ05_Conteo
```
**Salida esperada:**

```text
4
3
2
1
```
**Interpretación técnica.** La impresión sucede durante el descenso de las llamadas. El regreso solo termina invocaciones pendientes y no acumula un valor.

**Variación controlada.** Cambiar en papel el orden: si println se coloca después de contar(n−1), la impresión será 1,2,3,4; esta es la habilidad de la tarea T05.

**Error frecuente o límite.** Si se imprime después de la llamada esperando un conteo descendente, el resultado será ascendente. No es un error del compilador, sino del orden de las instrucciones.

**Corrección razonada.** Para la salida descendente colocar println antes de contar(n−1) y conservar el caso base.

**Explícalo con tus palabras.** ¿El procedimiento necesita `return n` como factorial?

## T05 — Tarea espejo: Mostrar números ascendentes aprovechando el retorno

**Enunciado.** Cambiar el momento de impresión sin alterar la reducción para visualizar cómo se invierte el orden de la secuencia. Imprimir desde 1 hasta 4 usando la misma llamada contar(n−1), pero colocando `println` después de la recursión.

**Archivo para crear o editar.** `T05_ConteoAscendente.java`. En `estudiante/tareas/T05_ConteoAscendente.java`, reemplaza TODO por una llamada con n−1 seguida de la impresión n; conservar el caso base que impide llegar a valores negativos.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T05_ConteoAscendente {
    public static void contar(int n) {
        if (n <= 0) return;
        // TODO T05: primero recurre a n-1; DESPUÉS imprime n.
    }
    public static void main(String[] args) {
        contar(4);
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Es la misma pila de llamadas de EJ05; ahora se imprime cuando se regresa de la llamada más profunda.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
1
2
3
4
```
**Cómo comprobarlo.** Compila con `javac T05_ConteoAscendente.java` y ejecuta con `java T05_ConteoAscendente`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ06 — Sumar los dígitos de un entero no negativo

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

Una operación recursiva también puede reducir un número sin restar uno. Para sumar los dígitos de 123, aislaremos el último dígito mediante el resto de la división entre diez y retiraremos ese dígito con una división entera. El método repetirá esa reducción hasta que no queden cifras. Construiremos primero el contrato de entrada, luego el caso base cero y después las dos operaciones que descomponen el número. Terminaremos mostrando que 123 produce seis. Este ejercicio permite comprender que «problema más pequeño» no siempre significa n−1: también puede significar n/10.

## ¿Qué problema concreto resolveremos?

¿Cómo extraer el 3 de 123 y resolver la suma del resto 12 sin convertir el número a texto?

**Archivo exacto:** `EJ06_SumaDigitos.java`. **Punto de partida:** La fórmula `último dígito + sum(n/10)` es contenido explícito de la presentación. Usaremos solo aritmética entera.

### Paso 1 — Definir el cálculo sobre un entero

**Antes de escribir, comprendamos por qué.** Queremos obtener un solo valor entero a partir de varios dígitos. Por eso el método se denomina sumarDigitos y devuelve int. El parámetro n contiene el número que aún debemos descomponer.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ06_SumaDigitos {
    public static int sumarDigitos(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** La clase corresponde al archivo `EJ06_SumaDigitos.java`. `int n` no representa aquí un contador de pasos sino la parte de la entrada no procesada. Cada llamada contendrá una versión con menos cifras. Abrimos clase y método para insertar la lógica.

**Estado observado.** Existe la estructura de cálculo, todavía sin reglas de entrada.

**Por qué sigue el próximo bloque.** La fórmula de dígitos será inicialmente para números no negativos.

### Paso 2 — Delimitar el caso a entradas no negativas

**Antes de escribir, comprendamos por qué.** La separación de cifras usando resto y división entera es más sencilla de interpretar primero con números no negativos. Evitamos que el signo forme parte del ejemplo inicial mediante una validación clara.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
```
**Qué hace cada instrucción y por qué está aquí.** `n < 0` comprueba la restricción; la excepción explica por qué no se procesa esa entrada. No es una salida normal de suma de dígitos. Cerramos el if y seguimos con cero o positivos.

**Estado observado.** Queda garantizado que `%10` devolverá una cifra entre 0 y 9 en esta versión.

**Por qué sigue el próximo bloque.** Ahora fijaremos el valor de la suma cuando ya no queden dígitos.

### Paso 3 — Finalizar cuando la parte restante sea cero

**Antes de escribir, comprendamos por qué.** Cuando ya hemos eliminado todas las cifras, la suma pendiente es cero. La llamada base hace posible recuperar los dígitos extraídos en las llamadas anteriores.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n == 0) {
            return 0;
        }
```
**Qué hace cada instrucción y por qué está aquí.** La comparación identifica n igual a cero; `return 0` entrega el acumulado neutro. Para el número original cero, el resultado también es cero. No se extraen dígitos ni se crean nuevas llamadas desde este caso.

**Estado observado.** La reducción de 123 finalizará en 0 después de pasar por 12 y 1.

**Por qué sigue el próximo bloque.** Debemos separar el dígito actual de las cifras que quedan.

### Paso 4 — Separar el último dígito del número restante

**Antes de escribir, comprendamos por qué.** Para que cada llamada procese una sola cifra, escribimos dos operaciones diferentes que operan sobre el mismo n. Son complementarias: una extrae y la otra elimina.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        int ultimo = n % 10;
        int restante = n / 10;
```
**Qué hace cada instrucción y por qué está aquí.** `n % 10` obtiene el resto de dividir por diez: para 123 es 3, que almacenamos como `ultimo`. `n / 10` usa división entera, descarta la fracción y produce 12, almacenado como `restante`. Ambos son variables locales, y cada llamada tendrá las suyas. Aún no hemos combinado ambas partes.

**Estado observado.** Con n=123: ultimo=3 y restante=12; con n=12: ultimo=2 y restante=1.

**Por qué sigue el próximo bloque.** Ahora sumaremos el dígito actual con la respuesta al subproblema restante.

### Paso 5 — Combinar la cifra extraída con los dígitos restantes

**Antes de escribir, comprendamos por qué.** La suma total resulta de una cifra que ya conocemos más la suma de las que todavía quedan. La llamada recursiva procesará `restante`, que se hace más pequeño dividiendo entre diez.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return ultimo + sumarDigitos(restante);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `sumarDigitos(restante)` vuelve a invocar el mismo método con menos cifras; `ultimo + ...` conserva el dígito actual en la combinación. `return` entrega el total al llamador y la llave cierra el método. A diferencia de factorial, la reducción es por división entera.

**Estado observado.** Los retornos forman 0, 1, 3 y 6 para el caso 123.

**Por qué sigue el próximo bloque.** Mostraremos esa respuesta desde el punto de entrada.

### Paso 6 — Mostrar el resultado para 123

**Antes de escribir, comprendamos por qué.** La clase ya sabe procesar los dígitos; solo falta un caso de verificación que el estudiante pueda contrastar con 1+2+3.

**Dónde escribir.** Abre `EJ06_SumaDigitos.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("Suma de 123: " + sumarDigitos(123));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** En main se invoca sumarDigitos(123); `println` forma la salida identificada. Las dos llaves cierran main y la clase. Se ejecuta con Java sin ninguna biblioteca externa.

**Estado observado.** La consola mostrará seis.

**Por qué sigue el próximo bloque.** La siguiente variante conservará esta lógica pero permitirá entradas negativas.

## Traza manual antes de ejecutar

123 % 10 = 3, 123 / 10 = 12; 12 % 10 = 2, 12 / 10 = 1; 1 % 10 = 1; 0 retorna 0; la suma es 1+2+3=6.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ06_SumaDigitos {
    public static int sumarDigitos(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
        if (n == 0) {
            return 0;
        }
        int ultimo = n % 10;
        int restante = n / 10;
        return ultimo + sumarDigitos(restante);
    }
    public static void main(String[] args) {
        System.out.println("Suma de 123: " + sumarDigitos(123));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice el último dígito de 123 y el valor que quedará al dividirlo entre diez usando enteros.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ06_SumaDigitos.java
java EJ06_SumaDigitos
```
**Salida esperada:**

```text
Suma de 123: 6
```
**Interpretación técnica.** Cada nivel retiene un dígito en su variable local y espera que se sumen los dígitos del número restante.

**Variación controlada.** Probar 900: debe devolver 9, incluidos los ceros que se suman sin cambiar el total.

**Error frecuente o límite.** Usar `/ 10` para extraer el último dígito o `% 10` para reducir el número invierte las responsabilidades y produce una lógica incorrecta.

**Corrección razonada.** Usar `%10` para aislar el último dígito y `/10` para retirar esa cifra; confirmar el caso base cero.

**Explícalo con tus palabras.** ¿Qué subproblema queda después de retirar el último dígito de 123?

## T06 — Tarea espejo: Contar cifras en vez de sumarlas

**Enunciado.** Utilizar la reducción por división entera enseñada en EJ06 para construir una operación que cuente cifras, no que sume sus valores. Implementar `contarDigitos` para enteros no negativos. Con 1234 debe retornar 4; con 0 debe retornar 1 porque la representación decimal de cero tiene una cifra.

**Archivo para crear o editar.** `T06_ContarDigitos.java`. En `estudiante/tareas/T06_ContarDigitos.java`, reemplaza TODO por el caso base n<10 y `1 + contarDigitos(n/10)`.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T06_ContarDigitos {
    public static int contarDigitos(int n) {
        if (n < 0) throw new IllegalArgumentException("n debe ser no negativo");
        // TODO T06: caso base de una cifra y reducción n/10.
        return -1; // marcador temporal que se reemplaza
    }
    public static void main(String[] args) {
        System.out.println("Dígitos de 1234: " + contarDigitos(1234));
        System.out.println("Dígitos de 0: " + contarDigitos(0));
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Cuando n<10 queda una sola cifra; para valores de dos o más cifras, cada llamada retira un dígito y cuenta uno.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
Dígitos de 1234: 4
Dígitos de 0: 1
```
**Cómo comprobarlo.** Compila con `javac T06_ContarDigitos.java` y ejecuta con `java T06_ContarDigitos`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ07 — Aceptar números negativos sin alterar la suma de sus dígitos

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

El material propone comprobar por qué la suma de dígitos requiere un tratamiento adicional cuando el número es negativo. En lugar de introducir signos dentro de cada llamada, separaremos dos responsabilidades: un método público recibirá el entero con su signo y obtendrá su magnitud; un segundo método privado sumará los dígitos de esa magnitud mediante la misma reducción de EJ06. Haremos la conversión a long antes de aplicar Math.abs para evitar el caso extremo del entero mínimo. La clase mostrará el resultado para −123 y también para el mínimo int. La meta es que el alumno distinga validar entradas de normalizarlas y pueda justificar cada decisión.

## ¿Qué problema concreto resolveremos?

¿Cómo tratar −123 como una secuencia de dígitos 1,2,3 sin convertir el signo menos en una cifra y sin producir desbordamiento con el mínimo int?

**Archivo exacto:** `EJ07_DigitosConSigno.java`. **Punto de partida:** Este ejemplo es una derivación controlada del ejercicio con IA sobre entradas negativas, que aparece en el material de la sesión.

### Paso 1 — Crear la clase y el método que recibe números con signo

**Antes de escribir, comprendamos por qué.** La responsabilidad de recibir un valor potencialmente negativo será diferente de la de extraer cifras. Comenzamos por el método público que actúa como entrada a la operación.

**Dónde escribir.** Abre `EJ07_DigitosConSigno.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ07_DigitosConSigno {
    public static int sumarDigitosConSigno(int n) {
```
**Qué hace cada instrucción y por qué está aquí.** La clase coincide con el nombre del archivo. `sumarDigitosConSigno(int n)` permite cualquier entrada de tipo int. El resultado todavía será int porque, incluso para diez cifras, la suma de dígitos cabe en ese tipo. La llave de método queda abierta para normalizar.

**Estado observado.** El programa tiene una entrada pública; aún no sabe convertir el signo.

**Por qué sigue el próximo bloque.** Antes de dividir en cifras construiremos una magnitud sin signo.

### Paso 2 — Convertir la entrada a una magnitud válida incluso en el caso extremo

**Antes de escribir, comprendamos por qué.** No debemos confiar en el valor absoluto de un int sin considerar su mínimo representable. Convertimos primero a long y después tomamos el valor absoluto; así el caso extremo no desborda.

**Dónde escribir.** Abre `EJ07_DigitosConSigno.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        long magnitud = Math.abs((long) n);
        return sumaPositiva(magnitud);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `(long)n` amplía el tipo antes del cálculo. `Math.abs` obtiene la magnitud no negativa y la almacena en `long magnitud`. `return sumaPositiva(magnitud)` delega al método auxiliar que escribiremos a continuación; es una referencia válida aunque su declaración aparezca después en la clase. La llave cierra el método público.

**Estado observado.** Para -123, magnitud vale 123. La segunda función todavía no está escrita; no compilar hasta agregarla.

**Por qué sigue el próximo bloque.** Ahora implementaremos el auxiliar que procesa solo números no negativos de tipo long.

### Paso 3 — Declarar la operación auxiliar sobre magnitudes

**Antes de escribir, comprendamos por qué.** El método auxiliar únicamente necesita sumar dígitos de un número no negativo. Su contrato es más estrecho y por eso lo declaramos privado dentro de la misma clase. Insertamos su firma y caso base después del método público.

**Dónde escribir.** Abre `EJ07_DigitosConSigno.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    private static int sumaPositiva(long numero) {
        if (numero == 0) {
            return 0;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `private` indica que el auxiliar solo se usa en esta clase. `static int` devuelve una suma. `long numero` almacena las cifras pendientes. La condición nula termina la recursión con el neutro de la suma. La primera llave final cierra el if, mientras el auxiliar continúa abierto.

**Estado observado.** El auxiliar ya puede finalizar al quedarse sin cifras; todavía necesita procesar una cifra de una entrada positiva.

**Por qué sigue el próximo bloque.** Reutilizaremos resto y división entera, ahora operando sobre long.

### Paso 4 — Extraer la cifra y continuar con un número más corto

**Antes de escribir, comprendamos por qué.** El auxiliar aplicará la misma recurrencia que EJ06, pero el número restante y el último dígito se obtendrán desde una variable long.

**Dónde escribir.** Abre `EJ07_DigitosConSigno.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        long ultimo = numero % 10;
        return (int) ultimo + sumaPositiva(numero / 10);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `numero % 10` devuelve la cifra final dentro del intervalo 0..9 y la guarda como `long ultimo`. `(int) ultimo` reduce de forma segura ese dígito concreto a int, ya que siempre cabe. `numero / 10` descarta la cifra procesada. `sumaPositiva` suma recursivamente las restantes; la llave final cierra el auxiliar.

**Estado observado.** El auxiliar calcula 3+2+1 para 123. Ningún valor de entrada con signo llega aquí.

**Por qué sigue el próximo bloque.** Solo queda una prueba de consola con un caso normal y el valor extremo de int.

### Paso 5 — Validar la entrada negativa y el entero mínimo

**Antes de escribir, comprendamos por qué.** El código ya puede sumar magnitudes. Para verificar la corrección no basta con -123: también debemos probar el caso que da problemas a una conversión ingenua a valor absoluto.

**Dónde escribir.** Abre `EJ07_DigitosConSigno.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        System.out.println("-123 -> " + sumarDigitosConSigno(-123));
        System.out.println("Min int -> " + sumarDigitosConSigno(Integer.MIN_VALUE));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `sumarDigitosConSigno(-123)` prueba el caso habitual y `Integer.MIN_VALUE` usa el entero mínimo definido por Java. El segundo println comprueba que la conversión previa a long fue necesaria. Las llaves cierran main y la clase. No se usa entrada externa ni dependencia adicional.

**Estado observado.** Debe mostrar seis y cuarenta y siete, respectivamente.

**Por qué sigue el próximo bloque.** Con esta variante validada, compararemos ahora un algoritmo que repite subproblemas con otro que guarda resultados.

## Traza manual antes de ejecutar

sumarDigitosConSigno(-123) convierte a magnitud 123 (long); el auxiliar procesa 123→12→1→0 y suma 3+2+1=6. Para Integer.MIN_VALUE, su magnitud cabe en long.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ07_DigitosConSigno {
    public static int sumarDigitosConSigno(int n) {
        long magnitud = Math.abs((long) n);
        return sumaPositiva(magnitud);
    }
    private static int sumaPositiva(long numero) {
        if (numero == 0) {
            return 0;
        }
        long ultimo = numero % 10;
        return (int) ultimo + sumaPositiva(numero / 10);
    }
    public static void main(String[] args) {
        System.out.println("-123 -> " + sumarDigitosConSigno(-123));
        System.out.println("Min int -> " + sumarDigitosConSigno(Integer.MIN_VALUE));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Antes de ejecutar, predice la suma de dígitos de -123 y explica por qué el signo no debe sumarse.

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ07_DigitosConSigno.java
java EJ07_DigitosConSigno
```
**Salida esperada:**

```text
-123 -> 6
Min int -> 47
```
**Interpretación técnica.** El método público retira el signo de forma segura; el auxiliar reaprovecha la lógica de separación y reducción ya aprendida. El valor mínimo de int también se procesa correctamente.

**Variación controlada.** Probar la entrada 0: el auxiliar retornará cero sin crear llamadas adicionales.

**Error frecuente o límite.** `Math.abs(n)` sobre Integer.MIN_VALUE no produce una magnitud positiva representable como int. Si no se amplía antes a long, se conserva un valor negativo por desbordamiento.

**Corrección razonada.** Aplicar `Math.abs((long)n)` y recibir ese valor en un auxiliar que trabaje con long.

**Explícalo con tus palabras.** ¿Por qué primero convertimos n a long y solo después aplicamos Math.abs?

## T07 — Tarea espejo: Contar dígitos de enteros negativos y del mínimo int

**Enunciado.** Combinar el conteo de cifras de T06 con la normalización segura de signo explicada en EJ07. Implementar `contarConSigno` con método público de entrada int y auxiliar long. Verificar -509 y `Integer.MIN_VALUE` sin convertir a texto.

**Archivo para crear o editar.** `T07_ContarConSigno.java`. En `estudiante/tareas/T07_ContarConSigno.java`, sustituye TODO por el caso base `numero < 10`, que devuelve uno, y la reducción `1 + contarMagnitud(numero/10)`.

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T07_ContarConSigno {
    public static int contarConSigno(int n) {
        long magnitud = Math.abs((long) n);
        return contarMagnitud(magnitud);
    }
    private static int contarMagnitud(long numero) {
        // TODO T07: caso base de un dígito y reducción /10.
        return -1; // marcador temporal que se reemplaza
    }
    public static void main(String[] args) {
        System.out.println("-509 tiene " + contarConSigno(-509) + " dígitos");
        System.out.println("Min int tiene " + contarConSigno(Integer.MIN_VALUE) + " dígitos");
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** Convierte a long antes de Math.abs. En el auxiliar long, cada división entre diez retira una cifra.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
-509 tiene 3 dígitos
Min int tiene 10 dígitos
```
**Cómo comprobarlo.** Compila con `javac T07_ContarConSigno.java` y ejecuta con `java T07_ContarConSigno`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.


---

# EJ08 — Comparar Fibonacci directo con Fibonacci que recuerda resultados

## Objetivo técnico — qué vamos a hacer y qué aprenderemos

La sesión propone revisar Fibonacci con ayuda de IA y discutir la ineficiencia de repetir llamadas. Construiremos primero la definición recursiva directa F(n)=F(n−1)+F(n−2) con casos base F(0)=0 y F(1)=1. Después crearemos una segunda versión que recibe un arreglo de memoria: antes de recalcular un subproblema, consultará si su resultado ya fue almacenado. En main calcularemos F(10) por ambas rutas y comprobaremos que coinciden en 55. La práctica enfatiza que una optimización válida conserva la respuesta, evita trabajo repetido y debe explicarse, no aceptarse solo porque la sugiera una IA.

## ¿Qué problema concreto resolveremos?

¿Por qué Fibonacci directo calcula varias veces F(3) y qué cambia cuando registramos un resultado para reutilizarlo?

**Archivo exacto:** `EJ08_FibonacciMemo.java`. **Punto de partida:** La presentación invita a analizar la ineficiencia exponencial y la memoización de Fibonacci. La versión con arreglo es una implementación operativa de esa misma habilidad.

### Paso 1 — Abrir la clase y definir la versión directa

**Antes de escribir, comprendamos por qué.** Queremos comparar dos implementaciones con la misma definición matemática. Primero creamos la versión directa, que servirá como referencia; validamos que n no sea negativo para evitar una cadena sin base.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; es el comienzo de un archivo nuevo.

```java
public class EJ08_FibonacciMemo {
    public static int directo(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
```
**Qué hace cada instrucción y por qué está aquí.** La clase coincide con el archivo. `int directo(int n)` promete devolver F(n). `if(n<0)` rechaza valores fuera del dominio de esta práctica; no modifica la definición de Fibonacci. Las llaves abiertas corresponden a clase y método y cerramos solo el if.

**Estado observado.** El método ya identifica entradas inválidas; aún carece de sus casos base.

**Por qué sigue el próximo bloque.** Ahora fijaremos los dos valores que sostienen toda la recurrencia.

### Paso 2 — Incorporar los dos casos base de Fibonacci

**Antes de escribir, comprendamos por qué.** Fibonacci necesita dos valores conocidos, no uno. Usamos una sola condición para representar simultáneamente F(0)=0 y F(1)=1.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (n <= 1) {
            return n;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `n <= 1` es verdadera para 0 y 1, que son las entradas válidas más pequeñas. `return n` devuelve exactamente el valor correcto para cada una, sin hacer más llamadas. La llave cierra el if y mantiene el método abierto.

**Estado observado.** F(0)=0 y F(1)=1 terminan directamente; F(2) debe calcular dos subproblemas.

**Por qué sigue el próximo bloque.** Expresaremos esa combinación de dos llamadas.

### Paso 3 — Combinar dos subproblemas en la versión directa

**Antes de escribir, comprendamos por qué.** Para n mayor que uno, la definición de Fibonacci combina los dos términos anteriores. A diferencia de factorial, hay dos llamadas en cada nivel; eso explica que aparezcan subproblemas repetidos.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        return directo(n - 1) + directo(n - 2);
    }
```
**Qué hace cada instrucción y por qué está aquí.** `directo(n-1)` y `directo(n-2)` reducen n de maneras distintas. El operador `+` combina ambos valores al retornar. La llave cierra directo. Como la función no recuerda respuestas, F(3) se recalcula si distintos caminos lo solicitan.

**Estado observado.** Para F(4) el algoritmo necesita F(3) y F(2); dentro de F(3) también calcula F(2), que se repite.

**Por qué sigue el próximo bloque.** Ahora construiremos una versión con un registro de resultados ya obtenidos.

### Paso 4 — Declarar el método que recibirá una memoria compartida

**Antes de escribir, comprendamos por qué.** Necesitamos recordar resultados entre llamadas. El mecanismo elegido será un arreglo de enteros que se pase siempre como el mismo objeto a cada llamada; n seguirá representando el índice de Fibonacci.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static int conMemoria(int n, int[] memoria) {
        if (n <= 1) {
            return n;
        }
```
**Qué hace cada instrucción y por qué está aquí.** `int[] memoria` es un arreglo donde la posición i almacenará F(i). El método devuelve int. Los casos n=0 y n=1 se resuelven antes de acceder al arreglo. Este método presupone n≥0 y un arreglo de tamaño al menos n+1; main asegurará esas condiciones.

**Estado observado.** Existe la firma del método con memoria y el caso base, pero todavía no se consulta ni escribe la caché.

**Por qué sigue el próximo bloque.** Agregaremos una lectura previa del arreglo para evitar recalcular valores.

### Paso 5 — Comprobar si el subproblema ya se resolvió

**Antes de escribir, comprendamos por qué.** Para n≥2, un resultado Fibonacci calculado es positivo. En un arreglo nuevo de int todas las posiciones comienzan en cero. Esta propiedad permite distinguir las posiciones que aún no se han calculado.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        if (memoria[n] != 0) {
            return memoria[n];
        }
```
**Qué hace cada instrucción y por qué está aquí.** `memoria[n]` consulta la celda correspondiente al índice n. `!= 0` significa distinto de cero; por ejemplo, si memoria[5] ya contiene 5, `return memoria[n]` entrega esa respuesta y evita dos nuevas ramas. Este criterio se usa solo después del caso base.

**Estado observado.** En el primer recorrido, memoria[n] vale 0 y seguimos al cálculo; en un recorrido posterior puede devolver inmediatamente.

**Por qué sigue el próximo bloque.** Falta decidir qué guardar cuando el resultado todavía no existe.

### Paso 6 — Calcular una vez y guardar para futuras llamadas

**Antes de escribir, comprendamos por qué.** Una vez comprobado que la respuesta no está almacenada, la calculamos igual que en Fibonacci directo, pero compartiendo el mismo arreglo con ambos subproblemas. Luego dejamos el resultado registrado.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
        memoria[n] = conMemoria(n - 1, memoria)
                   + conMemoria(n - 2, memoria);
        return memoria[n];
    }
```
**Qué hace cada instrucción y por qué está aquí.** `memoria[n] = ...` asigna a la posición n la suma de las dos llamadas recursivas; ambas reciben exactamente `memoria`, no un arreglo nuevo. El `return memoria[n]` entrega el valor ya almacenado. La llave cierra conMemoria. La asignación es el cambio fundamental respecto de la versión directa.

**Estado observado.** Después de calcular F(5), memoria[5] contiene 5; una llamada posterior a F(5) puede retornar esa celda.

**Por qué sigue el próximo bloque.** Finalmente compararemos las respuestas en main y verificaremos el tamaño de la memoria.

### Paso 7 — Ejecutar ambas variantes con una memoria de tamaño suficiente

**Antes de escribir, comprendamos por qué.** El objetivo no es solo ver que la segunda variante funciona, sino demostrar que conserva el mismo valor. Elegimos F(10) y preparamos una memoria con índices de 0 a 10 inclusive.

**Dónde escribir.** Abre `EJ08_FibonacciMemo.java` y pega este microbloque **inmediatamente después del fragmento del paso anterior**; no repitas las líneas previas.

```java
    public static void main(String[] args) {
        int[] memoria = new int[11];
        System.out.println("Directo F(10): " + directo(10));
        System.out.println("Memo F(10): " + conMemoria(10, memoria));
    }
}
```
**Qué hace cada instrucción y por qué está aquí.** `new int[11]` crea once posiciones, numeradas de 0 a 10. La primera impresión usa el método directo; la segunda pasa el mismo arreglo a la variante con memoria. Las dos llaves finales cierran main y clase. Esta comparación valida corrección, no demuestra por sí sola tiempos de ejecución ni una medición de rendimiento.

**Estado observado.** Ambas líneas muestran 55; el código compila sin imports externos.

**Por qué sigue el próximo bloque.** Con estos ejemplos podemos construir tareas espejo que obliguen a razonar sobre casos base, avance y reutilización.

## Traza manual antes de ejecutar

F(0)=0, F(1)=1, F(2)=1, F(3)=2, F(4)=3, F(5)=5, F(10)=55. Con memoización, memoria[10] guarda 55 y los subproblemas calculados quedan disponibles.

## Auditoría de la construcción y archivo completo

Confirma que los bloques se pegaron una sola vez, que no faltan llaves y que la firma del método coincide con su llamada desde `main`. El siguiente listado es **el resultado de concatenar los microbloques anteriores**, no una implementación alternativa.

```java
public class EJ08_FibonacciMemo {
    public static int directo(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("n debe ser no negativo");
        }
        if (n <= 1) {
            return n;
        }
        return directo(n - 1) + directo(n - 2);
    }
    public static int conMemoria(int n, int[] memoria) {
        if (n <= 1) {
            return n;
        }
        if (memoria[n] != 0) {
            return memoria[n];
        }
        memoria[n] = conMemoria(n - 1, memoria)
                   + conMemoria(n - 2, memoria);
        return memoria[n];
    }
    public static void main(String[] args) {
        int[] memoria = new int[11];
        System.out.println("Directo F(10): " + directo(10));
        System.out.println("Memo F(10): " + conMemoria(10, memoria));
    }
}
```
## Predice, ejecuta, interpreta

**Predicción.** Predice F(10). Explica por qué dos métodos diferentes deben producir el mismo valor antes de considerar uno «optimizado».

**Comandos exactos desde la carpeta de la sesión:**

```text
javac EJ08_FibonacciMemo.java
java EJ08_FibonacciMemo
```
**Salida esperada:**

```text
Directo F(10): 55
Memo F(10): 55
```
**Interpretación técnica.** La respuesta coincide. La versión directa repite subárboles de llamadas; la versión memorizada guarda cada resultado positivo calculado para n≥2 y lo consulta cuando vuelve a necesitarlo.

**Variación controlada.** Evaluar por inspección cuántas veces aparece F(3) en el desarrollo de F(5) y explicar por qué guardarlo evita recálculos.

**Error frecuente o límite.** Una caché de tamaño menor que n+1 produciría un error de índice. Reutilizar un arreglo sin dimensionarlo para la entrada máxima no es válido.

**Corrección razonada.** Crear `new int[11]` para n=10; al solicitar F(12) se debe ampliar al menos a 13 posiciones. Validar el límite de entrada al usar el método.

**Explícalo con tus palabras.** ¿Por qué `memoria[n] != 0` funciona como indicador de valor calculado en n≥2?

## T08 — Tarea espejo: Compartir la memoria de Fibonacci entre dos consultas

**Enunciado.** Comprobar que una memoria correctamente dimensionada puede ser reutilizada para dos términos cercanos de Fibonacci sin rehacer todo el cálculo. Con el método conMemoria ya preparado, crear un solo arreglo de 13 posiciones para F(12); calcular F(12), después F(11) reutilizando exactamente el mismo arreglo. Mostrar además memoria[11] para comprobar que ese valor ya fue almacenado.

**Archivo para crear o editar.** `T08_FibonacciCacheCompartida.java`. En `estudiante/tareas/T08_FibonacciCacheCompartida.java`, reemplaza el TODO de main por la creación de memoria y las tres instrucciones de impresión. No se debe crear un nuevo arreglo antes de pedir F(11).

**Código inicial de esta tarea (cópialo en un archivo nuevo con el nombre indicado).** La zona TODO es deliberadamente incompleta: el retorno temporal solo permite compilar y debe sustituirse al resolver el ejercicio.

```java
public class T08_FibonacciCacheCompartida {
    public static int conMemoria(int n, int[] memoria) {
        if (n <= 1) return n;
        if (memoria[n] != 0) return memoria[n];
        memoria[n] = conMemoria(n - 1, memoria) + conMemoria(n - 2, memoria);
        return memoria[n];
    }
    public static void main(String[] args) {
        // TODO T08: crea int[] memoria = new int[13]; muestra F(12), F(11)
        // y memoria[11] reutilizando la misma variable memoria.
    }
}
```
**Restricción.** Emplea exclusivamente los patrones de caso base, validación, reducción o combinación estudiados aquí; no introduzcas bibliotecas externas.

**Pista.** El arreglo tiene índices de 0 hasta 12. La consulta F(12) rellena los subproblemas y la consulta F(11) encuentra su respuesta en la memoria compartida.

**Evidencia requerida.** Archivo Java que compile, la salida de consola y una explicación breve del cambio respecto al ejemplo. **Resultado que debes obtener:**

```text
Memo F(12): 144
Memo F(11): 89
Memoria[11]: 89
```
**Cómo comprobarlo.** Compila con `javac T08_FibonacciCacheCompartida.java` y ejecuta con `java T08_FibonacciCacheCompartida`; compara exactamente las líneas anteriores. La solución de la tarea no se muestra aquí.

## Cierre de sesión: de repetir llamadas a razonar algoritmos

A lo largo de la clase construimos recursividad desde sus decisiones esenciales. En factorial y en las sumas, cada nivel conserva parte de la operación y espera el retorno del problema menor. En el conteo regresivo, en cambio, la posición de `println` determina si observamos la secuencia durante el descenso o durante el regreso. En los dígitos descubrimos que la reducción no tiene que ser n−1: la división entera entre diez también nos acerca a un caso base. Al aceptar valores negativos, aprendimos que validar una entrada y transformarla correctamente son decisiones distintas. Y al comparar Fibonacci directo con memoización entendimos por qué dos programas pueden devolver el mismo valor y, sin embargo, realizar cantidades diferentes de trabajo.

También examinamos un riesgo muy concreto: una condición de parada no basta si la recursión se aleja de ella. Para reconocer ese problema no necesitamos provocar un `StackOverflowError`; podemos inspeccionar el argumento en llamadas sucesivas, identificar la dirección equivocada y justificar la corrección antes de ejecutar. Esa actitud también debe aplicarse a las respuestas de herramientas de inteligencia artificial: pedir una explicación útil, contrastarla con una traza manual y verificarla con código.

Como repaso, revisa los casos base, el cambio de parámetro y el orden en que se ejecutan las instrucciones. Completa las ocho tareas espejo y entrega sus salidas comentadas. Esta sesión deja preparado el criterio técnico para abordar algoritmos recursivos más complejos sin confundir una solución elegante con una solución automáticamente eficiente. En Lideratec Academy puedes complementar el estudio con el blog https://lideratecacademy.com/blog/ y los recursos públicos de https://www.youtube.com/@LideratecAcademy.
