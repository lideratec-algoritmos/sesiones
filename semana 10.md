---
course_id: ISIL-30710
session_id: S10
module_id: TEMA_10
source_origin: PPT
status: validated
---

# Algoritmo y Estructura de Datos Basado en IA · Sesión 10

**Lideratec Academy · Estructuras FIFO (Colas) · Java 21 (compilación verificada)**

# Guía del Estudiante — construcción y comprensión paso a paso

## Propósito de la clase

Cuando varias tareas llegan a un sistema, el orden de atención afecta el resultado. Una impresora compartida debe empezar por el documento recibido antes; un sistema de procesos puede tener tareas listas esperando turno; un flujo de datos conserva un orden de procesamiento. Esta sesión estudiará esa necesidad mediante una **cola FIFO**, abreviatura de *First In, First Out*: el primero en entrar es el primero en salir. Es un principio de operación, no el nombre de una función de Java.

Comenzaremos con una cola lineal implementada en un arreglo de enteros. Aprenderemos a distinguir capacidad de elementos activos y a interpretar los índices `front` y `rear`. A partir de las operaciones `encolar` y `desencolar`, observaremos overflow cuando el último índice físico ya se ha alcanzado, incluso si se liberaron posiciones al inicio. Contrastaremos ese comportamiento con una implementación que desplaza valores y, después, con una lista enlazada cuyos nodos conservan un dato y una referencia al siguiente. Finalmente, compararemos resultados y utilizaremos preguntas de depuración con IA como apoyo, sin reemplazar las pruebas hechas por nosotros.

La evidencia final no será solo que Java muestre números: cada estudiante deberá **dibujar el estado antes y después**, justificar qué extremo cambió y explicar por qué una implementación acepta o rechaza una inserción.

## Resultado de aprendizaje — lectura sugerida del docente

Al finalizar, podrás implementar y ejecutar colas FIFO en Java utilizando tanto un vector lineal como nodos enlazados. Dado un orden de llegada, como 10, 20 y 30, podrás predecir que la primera extracción devolverá 10; además podrás indicar qué posición o referencia se modifica, sin confundir el valor reservado en memoria con un elemento todavía presente en la cola. Serás capaz de reconocer las diferencias entre underflow, overflow físico de una cola lineal e inadecuada actualización de enlaces, y justificar sus correcciones con trazas y resultados de ejecución. En los ejercicios finales explicarás por qué una cola con desplazamiento realiza más trabajo al extraer, aunque reutilice posiciones, y cómo una cola enlazada modifica sus referencias con un costo constante en las operaciones básicas.

**Desempeños verificables:** construir `encolar` y `desencolar`; trazar índices y nodos; interpretar salidas; comparar comportamientos; reproducir errores; proponer correcciones; justificar una recomendación de IA con pruebas.

**Criterio de dominio:** además de ejecutar, debes poder responder qué elemento salió, por qué salió ese y qué cambió en `front`, `rear` o `fin`.

## Antes de iniciar: herramientas y entorno real

Trabajaremos con archivos Java independientes. Necesitas un **JDK** con `javac` y `java` accesibles y un editor de texto o IDE que permita guardar archivos `.java` en UTF-8. Esta práctica se validó con **OpenJDK 21**; también es compatible a nivel de sintaxis con versiones modernas de Java que admitan clases internas estáticas y `main` clásico. No se requiere una biblioteca externa ni conexión a una base de datos. La presentación utiliza código Java introductorio, por lo que evitaremos colecciones de biblioteca para no ocultar cómo funcionan los extremos.

1. Abre una terminal en la computadora y ejecuta `java -version` y `javac -version`. Ambos comandos deben responder con una versión, sin el mensaje «comando no encontrado». Si `java` existe pero `javac` no, falta configurar un JDK con compilador.
2. Crea una carpeta llamada `S10_Colas_FIFO` y ábrela en el editor o IDE. **Crea cada archivo con el nombre exacto** del ejemplo; el nombre público de clase debe coincidir con el archivo. No incluyas `package` en estos archivos autónomos.
3. En los pasos de microconstrucción, **agrega cada fragmento al final del archivo en el mismo orden**. Algunas llaves permanecen abiertas hasta el último paso; en ese punto el archivo queda completo y es cuando debes compilar. No reemplaces fragmentos anteriores salvo que un paso lo ordene explícitamente.
4. Una vez completo un ejemplo, ejecuta `javac -encoding UTF-8 ARCHIVO.java` y después `java NOMBRE_DE_CLASE`, usando exactamente los nombres mostrados en el encabezado de ese ejemplo. Si aparece `cannot find symbol`, comprueba letras, mayúsculas y llaves; si aparece `class ... is public, should be declared in a file named ...`, corrige el nombre del archivo.
5. Antes de cada ejecución realiza una predicción en papel. Después compara la salida **línea por línea**. Cuando veas `-1` en `desencolar`, recuerda que en estos ejemplos es una señal didáctica de underflow, no una solución general si los datos negativos fueran válidos.

**Plan orientativo para 120 minutos:** apertura y entorno 10 min; EJ01–EJ02 25 min; EJ03–EJ04 28 min; EJ05 20 min; EJ06 20 min; comparación de resultados, diagnóstico con IA y cierre 17 min. **EJ07 y EJ08 son ampliación guiada para trabajo autónomo**, con puesta en común si el avance del grupo lo permite; las tareas T01–T08 se asignan como consolidación fuera de la sesión presencial. Así priorizamos entender las decisiones sin comprimir artificialmente los microbloques.

**Convenciones compartidas:** `front`/`frente` significan primer elemento; `rear`/`fin` representan el final. Una cola vacía de vector puede señalizarse con `-1` o con `frente > fin` según su diseño; una enlazada se identifica con `front == null`. No mezcles esas convenciones dentro del mismo programa.

---

## EJ01 — Encolar 10, 20 y 30 en un vector

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Comenzaremos por construir una cola lineal de tres posiciones para comprobar qué significa que el primero en llegar permanezca al frente. Primero declararemos el vector y los índices front y rear, después inicializaremos ambos como vacíos y finalmente construiremos encolar con su protección ante overflow. Cerraremos el programa ejecutando tres inserciones y leyendo el estado de los índices; así verificaremos que front no se desplaza cuando llegan nuevos elementos.

### ¿Qué problema concreto resolveremos?

¿Cómo distinguir tres posiciones reservadas de tres datos realmente encolados sin confundir un cero inicial con un elemento?

**Archivo:** `EJ01_EncolarVector.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: demostración central.

### Microconstrucción guiada

#### Paso 1 — Crear el contenedor y distinguir capacidad de contenido

**Para comprender antes de escribir.** Partimos de un archivo nuevo, EJ01_EncolarVector.java. Una cola con vector necesita separar el espacio físico reservado de las posiciones que realmente contienen elementos. El arreglo por sí solo no sabe cuál es el primero o el último dato lógico. Por esa razón declaramos la clase interna Cola y tres indicadores.

**Dónde y cómo construirlo.** Crea `EJ01_EncolarVector.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ01_EncolarVector {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
```

**Mientras lo construimos, comprendamos cada parte.** La clase pública coincide con el nombre del archivo para que Java encuentre su punto de entrada. Cola agrupa el comportamiento de la estructura. int[] cola contiene las celdas numéricas; capacidad representa cuántas caben; front identifica la primera ocupada y rear la última. Aunque todas las celdas de un int[] comienzan con cero, ese cero no significa que exista un elemento encolado.

**Estado después y comprobación.** Tenemos nombres y responsabilidades, pero todavía no se ha reservado memoria ni se ha establecido una cola vacía. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Necesitamos un constructor que reserve el vector y marque explícitamente el estado vacío.

#### Paso 2 — Inicializar el vector y representar la cola vacía

**Para comprender antes de escribir.** La declaración de atributos todavía no produce un vector utilizable para una instancia. En el constructor recibiremos la capacidad elegida por quien crea la cola y fijaremos la misma señal de vacío para los dos extremos.

**Dónde y cómo construirlo.** En `EJ01_EncolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
```

**Mientras lo construimos, comprendamos cada parte.** El parámetro capacidad entra desde new Cola(...). this.capacidad permite distinguir el atributo del parámetro. new int[capacidad] reserva las posiciones; front=-1 y rear=-1 significan que todavía no hay primer ni último elemento. Iniciar ambos índices en cero mentiría: parecería que existe un dato en la posición cero antes de llamar a encolar.

**Estado después y comprobación.** Tras construir Cola(3), hay tres celdas reservadas, pero ninguna está activa; ambos índices valen -1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora hace falta una operación que compruebe disponibilidad antes de escribir en el vector.

#### Paso 3 — Proteger el límite de inserción

**Para comprender antes de escribir.** Si escribimos directamente en cola[rear+1], podríamos sobrepasar el último índice válido. Primero creamos el método encolar, que recibe el valor, y detenemos la operación en caso de que rear haya alcanzado el límite físico del arreglo.

**Dónde y cómo construirlo.** En `EJ01_EncolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
```

**Mientras lo construimos, comprendamos cada parte.** void indica que esta operación informa por consola, pero no devuelve un valor. dato es el entero que llega. rear == capacidad - 1 compara el índice final con la última posición válida, porque los índices empiezan en 0. println informa el overflow; return sale del método sin alterar los datos. En una cola lineal no desplazable este chequeo es físico, no calcula la cantidad de huecos vacíos al inicio.

**Estado después y comprobación.** La regla de protección ya puede reconocer el límite, pero todavía no guarda valores cuando existe espacio. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Se debe fijar el primer frente cuando llega el primer elemento e incrementar rear justo antes de escribir.

#### Paso 4 — Guardar al final sin mover el frente

**Para comprender antes de escribir.** Superada la comprobación de capacidad, añadimos el comportamiento que realmente modifica la cola. Cuando el estado previo es vacío, la primera posición ocupada será la cero; en inserciones siguientes, el frente no debe moverse.

**Dónde y cómo construirlo.** En `EJ01_EncolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** front == -1 detecta el primer ingreso y front=0 lo señala. En cola[++rear], el prefijo ++ incrementa rear antes de usarlo como índice; así, rear pasa de -1 a 0 y el primer valor se escribe en cola[0]. El valor dato se guarda y el mensaje confirma la inserción. Si usáramos cola[rear++] cuando rear vale -1, intentaríamos escribir en un índice inválido.

**Estado después y comprobación.** Con encolar(10), front y rear se convierten en 0; al ingresar 20, front sigue en 0 y rear pasa a 1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** La cola ya puede aceptar elementos. Falta un punto de entrada que la construya y realice una secuencia observable.

#### Paso 5 — Completar el programa y ejecutar un caso reproducible

**Para comprender antes de escribir.** La estructura ya tiene las operaciones necesarias. Ahora cerramos la clase interna Cola, creamos main para ejecutar una secuencia de pruebas y cerramos la clase pública. No se deben compilar los pasos anteriores aislados: las llaves abiertas se completan en este último bloque.

**Dónde y cómo construirlo.** En `EJ01_EncolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        Cola q = new Cola(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("front=" + q.front + ", rear=" + q.rear);
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** La llave anterior a main cierra Cola; public static void main(String[] args) es el punto inicial del programa. new Cola(capacidad) llama al constructor y las llamadas al objeto ejecutan las operaciones en el orden escrito. Las impresiones de estado permiten contrastar la traza manual con índices reales. Las últimas llaves cierran main y la clase pública.

**Estado después y comprobación.** El archivo ahora es sintácticamente completo y puede compilarse con javac; el orden de main determina la salida observable. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Comprobaremos predicciones y luego modificaremos solo las llamadas de prueba, sin alterar la lógica que acabamos de explicar.

### Traza manual: antes → operación → después

Inicio: front=-1,rear=-1. Tras 10: 0,0. Tras 20: 0,1. Tras 30: 0,2. Elementos activos: [10,20,30].

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ01_EncolarVector {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
    }

    public static void main(String[] args) {
        Cola q = new Cola(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("front=" + q.front + ", rear=" + q.rear);
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ01_EncolarVector.java
java EJ01_EncolarVector
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Encolado: 30
front=0, rear=2
```

### Cómo interpretar la salida

Se incorpora el primer recorrido visual del índice rear sin eliminar el primer dato. Inicio: front=-1,rear=-1. Tras 10: 0,0. Tras 20: 0,1. Tras 30: 0,2. Elementos activos: [10,20,30]. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Prueba temporalmente q.encolar(40) después de 30; recibirás overflow y front/rear no deben cambiar. Reestablece el main canónico después.

### Error o caso límite controlado

Una cuarta inserción en capacidad 3 no debe escribir la posición 3.

**Corrección razonada.** La guarda rear == capacidad - 1 debe detener el método antes de ++rear.

### Explícalo con tus palabras

¿Qué diferencia hay entre capacidad 3 y rear=2?

### T01 — tarea espejo de EJ01

**Enunciado.** Encolar 5, 15 y 25 en capacidad 3; imprimir ambos índices y provocar una cuarta inserción con 35.

**Archivo que crearás:** `T01_EncolarVector.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T01_EncolarVector`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 5
Encolado: 15
Encolado: 25
Error: Cola llena (Overflow)
front=0, rear=2
```

---

## EJ02 — Desencolar en FIFO y reiniciar una cola vacía

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Partimos de la estructura con vector y le añadiremos desencolar para que el mismo almacenamiento respete la disciplina FIFO también en la salida. La nueva decisión consiste en proteger underflow, guardar el valor del frente antes de moverlo y reiniciar los indicadores cuando sale el último elemento. Ejecutaremos tres extracciones, una lectura de cola vacía y una inserción posterior: todas las salidas deben confirmar que salió primero el 10, no el 30.

### ¿Qué problema concreto resolveremos?

¿Qué ocurre si avanzamos front antes de copiar el elemento que debe salir, o si olvidamos reiniciar los índices después de vaciar?

**Archivo:** `EJ02_DesencolarVector.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: demostración central.

### Microconstrucción guiada

#### Paso 1 — Crear el contenedor y distinguir capacidad de contenido

**Para comprender antes de escribir.** Partimos de un archivo nuevo, EJ02_DesencolarVector.java. Una cola con vector necesita separar el espacio físico reservado de las posiciones que realmente contienen elementos. El arreglo por sí solo no sabe cuál es el primero o el último dato lógico. Por esa razón declaramos la clase interna Cola y tres indicadores.

**Dónde y cómo construirlo.** Crea `EJ02_DesencolarVector.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ02_DesencolarVector {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
```

**Mientras lo construimos, comprendamos cada parte.** La clase pública coincide con el nombre del archivo para que Java encuentre su punto de entrada. Cola agrupa el comportamiento de la estructura. int[] cola contiene las celdas numéricas; capacidad representa cuántas caben; front identifica la primera ocupada y rear la última. Aunque todas las celdas de un int[] comienzan con cero, ese cero no significa que exista un elemento encolado.

**Estado después y comprobación.** Tenemos nombres y responsabilidades, pero todavía no se ha reservado memoria ni se ha establecido una cola vacía. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Necesitamos un constructor que reserve el vector y marque explícitamente el estado vacío.

#### Paso 2 — Inicializar el vector y representar la cola vacía

**Para comprender antes de escribir.** La declaración de atributos todavía no produce un vector utilizable para una instancia. En el constructor recibiremos la capacidad elegida por quien crea la cola y fijaremos la misma señal de vacío para los dos extremos.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
```

**Mientras lo construimos, comprendamos cada parte.** El parámetro capacidad entra desde new Cola(...). this.capacidad permite distinguir el atributo del parámetro. new int[capacidad] reserva las posiciones; front=-1 y rear=-1 significan que todavía no hay primer ni último elemento. Iniciar ambos índices en cero mentiría: parecería que existe un dato en la posición cero antes de llamar a encolar.

**Estado después y comprobación.** Tras construir Cola(3), hay tres celdas reservadas, pero ninguna está activa; ambos índices valen -1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora hace falta una operación que compruebe disponibilidad antes de escribir en el vector.

#### Paso 3 — Proteger el límite de inserción

**Para comprender antes de escribir.** Si escribimos directamente en cola[rear+1], podríamos sobrepasar el último índice válido. Primero creamos el método encolar, que recibe el valor, y detenemos la operación en caso de que rear haya alcanzado el límite físico del arreglo.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
```

**Mientras lo construimos, comprendamos cada parte.** void indica que esta operación informa por consola, pero no devuelve un valor. dato es el entero que llega. rear == capacidad - 1 compara el índice final con la última posición válida, porque los índices empiezan en 0. println informa el overflow; return sale del método sin alterar los datos. En una cola lineal no desplazable este chequeo es físico, no calcula la cantidad de huecos vacíos al inicio.

**Estado después y comprobación.** La regla de protección ya puede reconocer el límite, pero todavía no guarda valores cuando existe espacio. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Se debe fijar el primer frente cuando llega el primer elemento e incrementar rear justo antes de escribir.

#### Paso 4 — Guardar al final sin mover el frente

**Para comprender antes de escribir.** Superada la comprobación de capacidad, añadimos el comportamiento que realmente modifica la cola. Cuando el estado previo es vacío, la primera posición ocupada será la cero; en inserciones siguientes, el frente no debe moverse.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** front == -1 detecta el primer ingreso y front=0 lo señala. En cola[++rear], el prefijo ++ incrementa rear antes de usarlo como índice; así, rear pasa de -1 a 0 y el primer valor se escribe en cola[0]. El valor dato se guarda y el mensaje confirma la inserción. Si usáramos cola[rear++] cuando rear vale -1, intentaríamos escribir en un índice inválido.

**Estado después y comprobación.** Con encolar(10), front y rear se convierten en 0; al ingresar 20, front sigue en 0 y rear pasa a 1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** La cola ya puede aceptar elementos. Falta un punto de entrada que la construya y realice una secuencia observable.

#### Paso 5 — Impedir underflow antes de leer

**Para comprender antes de escribir.** Para respetar FIFO no basta con insertar. Necesitamos extraer siempre la posición indicada por front, pero si no hay dato activo no se debe leer el vector. Crearemos una operación que devuelve el entero retirado o -1 para esta demostración cuando se intenta extraer de una cola vacía.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == -1 || front > rear) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
```

**Mientras lo construimos, comprendamos cada parte.** int identifica el valor devuelto por desencolar. La condición usa || (OR): si front==-1 no hay elementos, o si front>rear ya pasamos el último. System.out.println muestra underflow y return -1 informa que no se retiró un dato. El convenio -1 es didáctico: si los datos válidos pudieran ser negativos, necesitaríamos distinguir el error de otro modo.

**Estado después y comprobación.** La operación puede rechazar una extracción inválida, pero aún no realiza una extracción correcta. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Leemos el dato antes de avanzar front; de otro modo perderíamos el valor que debía salir.

#### Paso 6 — Leer el frente, avanzar y reiniciar al vaciar

**Para comprender antes de escribir.** Ya protegimos la cola vacía. El dato que debe salir se encuentra exactamente en front; por eso se captura primero y solo después se adelanta el índice al siguiente elemento lógico.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            int dato = cola[front++];
            if (front > rear) {
                front = -1;
                rear = -1;
            }
            return dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** int dato almacena temporalmente el valor que abandona la cola. cola[front++] lee con el front actual y después incrementa ese índice: es el orden correcto de lectura y avance. Si front>rear tras esa lectura, salió el último elemento y no hay datos activos; ambos indicadores vuelven a -1. return dato entrega el valor capturado. No movemos elementos del arreglo; por eso una cola parcialmente vaciada puede bloquear una nueva inserción si rear quedó en el límite.

**Estado después y comprobación.** Para [10,20,30], la primera extracción devuelve 10 y front cambia 0→1; al retirar 30 finalmente, front y rear regresan a -1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Falta crear el método main que compruebe la secuencia concreta del ejemplo.

#### Paso 7 — Completar el programa y ejecutar un caso reproducible

**Para comprender antes de escribir.** La estructura ya tiene las operaciones necesarias. Ahora cerramos la clase interna Cola, creamos main para ejecutar una secuencia de pruebas y cerramos la clase pública. No se deben compilar los pasos anteriores aislados: las llaves abiertas se completan en este último bloque.

**Dónde y cómo construirlo.** En `EJ02_DesencolarVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        Cola q = new Cola(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Cola vacia -> " + q.desencolar());
        q.encolar(40);
        System.out.println("indices: " + q.front + "," + q.rear);
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** La llave anterior a main cierra Cola; public static void main(String[] args) es el punto inicial del programa. new Cola(capacidad) llama al constructor y las llamadas al objeto ejecutan las operaciones en el orden escrito. Las impresiones de estado permiten contrastar la traza manual con índices reales. Las últimas llaves cierran main y la clase pública.

**Estado después y comprobación.** El archivo ahora es sintácticamente completo y puede compilarse con javac; el orden de main determina la salida observable. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Comprobaremos predicciones y luego modificaremos solo las llamadas de prueba, sin alterar la lógica que acabamos de explicar.

### Traza manual: antes → operación → después

Después de encolar: front=0 rear=2. Sale 10→front=1. Sale 20→front=2. Sale 30→front=-1 y rear=-1. Otra extracción→underflow. Encolar 40→front=0 y rear=0.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ02_DesencolarVector {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
        int desencolar() {
            if (front == -1 || front > rear) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = cola[front++];
            if (front > rear) {
                front = -1;
                rear = -1;
            }
            return dato;
        }
    }

    public static void main(String[] args) {
        Cola q = new Cola(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Cola vacia -> " + q.desencolar());
        q.encolar(40);
        System.out.println("indices: " + q.front + "," + q.rear);
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ02_DesencolarVector.java
java EJ02_DesencolarVector
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Encolado: 30
Sale: 10
Sale: 20
Sale: 30
Error: Cola vacia (Underflow)
Cola vacia -> -1
Encolado: 40
indices: 0,0
```

### Cómo interpretar la salida

El orden temporal de lectura e incremento fundamenta el comportamiento FIFO. Después de encolar: front=0 rear=2. Sale 10→front=1. Sale 20→front=2. Sale 30→front=-1 y rear=-1. Otra extracción→underflow. Encolar 40→front=0 y rear=0. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Altera solo la prueba y extrae primero dos elementos; comprueba que front vale 2 y rear sigue 2. Devuelve el main al original.

### Error o caso límite controlado

Extraer estando vacía sin protección causa lectura indebida; extraer sin reinicio puede dejar la cola con índice inválido.

**Corrección razonada.** Primero verificar vacío; guardar cola[front++] antes de avanzar; al quedar vacía, restablecer front y rear a -1.

### Explícalo con tus palabras

¿Por qué reiniciamos los dos índices y no solo front?

### T02 — tarea espejo de EJ02

**Enunciado.** Usar capacidad 2, encolar 7 y 8, desencolar tres veces y volver a encolar 9. Informar las tres extracciones y los índices finales.

**Archivo que crearás:** `T02_DesencolarVector.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T02_DesencolarVector`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 7
Encolado: 8
Sale: 7
Sale: 8
Error: Cola vacia (Underflow)
Sale: -1
Encolado: 9
front=0, rear=0
```

---

## EJ03 — Detectar overflow con huecos sin reutilizar

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Usaremos la misma cola lineal que acabamos de construir para analizar su principal límite práctico. Llenaremos sus cinco posiciones, desencolaremos los dos elementos del frente y luego intentaremos insertar 60. Aunque las posiciones cero y uno ya no sean parte de la cola activa, rear seguirá en el índice cuatro y la operación mostrará overflow. No construiremos una cola circular: el propósito es diagnosticar con precisión el comportamiento de esta implementación lineal.

### ¿Qué problema concreto resolveremos?

¿Por qué aparece “Cola llena” si existen celdas físicamente reservadas que ya no contienen elementos activos?

**Archivo:** `EJ03_OverflowLineal.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: discusión con IA.

### Microconstrucción guiada

#### Paso 1 — Crear el contenedor y distinguir capacidad de contenido

**Para comprender antes de escribir.** Partimos de un archivo nuevo, EJ03_OverflowLineal.java. Una cola con vector necesita separar el espacio físico reservado de las posiciones que realmente contienen elementos. El arreglo por sí solo no sabe cuál es el primero o el último dato lógico. Por esa razón declaramos la clase interna Cola y tres indicadores.

**Dónde y cómo construirlo.** Crea `EJ03_OverflowLineal.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ03_OverflowLineal {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
```

**Mientras lo construimos, comprendamos cada parte.** La clase pública coincide con el nombre del archivo para que Java encuentre su punto de entrada. Cola agrupa el comportamiento de la estructura. int[] cola contiene las celdas numéricas; capacidad representa cuántas caben; front identifica la primera ocupada y rear la última. Aunque todas las celdas de un int[] comienzan con cero, ese cero no significa que exista un elemento encolado.

**Estado después y comprobación.** Tenemos nombres y responsabilidades, pero todavía no se ha reservado memoria ni se ha establecido una cola vacía. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Necesitamos un constructor que reserve el vector y marque explícitamente el estado vacío.

#### Paso 2 — Inicializar el vector y representar la cola vacía

**Para comprender antes de escribir.** La declaración de atributos todavía no produce un vector utilizable para una instancia. En el constructor recibiremos la capacidad elegida por quien crea la cola y fijaremos la misma señal de vacío para los dos extremos.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
```

**Mientras lo construimos, comprendamos cada parte.** El parámetro capacidad entra desde new Cola(...). this.capacidad permite distinguir el atributo del parámetro. new int[capacidad] reserva las posiciones; front=-1 y rear=-1 significan que todavía no hay primer ni último elemento. Iniciar ambos índices en cero mentiría: parecería que existe un dato en la posición cero antes de llamar a encolar.

**Estado después y comprobación.** Tras construir Cola(3), hay tres celdas reservadas, pero ninguna está activa; ambos índices valen -1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora hace falta una operación que compruebe disponibilidad antes de escribir en el vector.

#### Paso 3 — Proteger el límite de inserción

**Para comprender antes de escribir.** Si escribimos directamente en cola[rear+1], podríamos sobrepasar el último índice válido. Primero creamos el método encolar, que recibe el valor, y detenemos la operación en caso de que rear haya alcanzado el límite físico del arreglo.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
```

**Mientras lo construimos, comprendamos cada parte.** void indica que esta operación informa por consola, pero no devuelve un valor. dato es el entero que llega. rear == capacidad - 1 compara el índice final con la última posición válida, porque los índices empiezan en 0. println informa el overflow; return sale del método sin alterar los datos. En una cola lineal no desplazable este chequeo es físico, no calcula la cantidad de huecos vacíos al inicio.

**Estado después y comprobación.** La regla de protección ya puede reconocer el límite, pero todavía no guarda valores cuando existe espacio. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Se debe fijar el primer frente cuando llega el primer elemento e incrementar rear justo antes de escribir.

#### Paso 4 — Guardar al final sin mover el frente

**Para comprender antes de escribir.** Superada la comprobación de capacidad, añadimos el comportamiento que realmente modifica la cola. Cuando el estado previo es vacío, la primera posición ocupada será la cero; en inserciones siguientes, el frente no debe moverse.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** front == -1 detecta el primer ingreso y front=0 lo señala. En cola[++rear], el prefijo ++ incrementa rear antes de usarlo como índice; así, rear pasa de -1 a 0 y el primer valor se escribe en cola[0]. El valor dato se guarda y el mensaje confirma la inserción. Si usáramos cola[rear++] cuando rear vale -1, intentaríamos escribir en un índice inválido.

**Estado después y comprobación.** Con encolar(10), front y rear se convierten en 0; al ingresar 20, front sigue en 0 y rear pasa a 1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** La cola ya puede aceptar elementos. Falta un punto de entrada que la construya y realice una secuencia observable.

#### Paso 5 — Impedir underflow antes de leer

**Para comprender antes de escribir.** Para respetar FIFO no basta con insertar. Necesitamos extraer siempre la posición indicada por front, pero si no hay dato activo no se debe leer el vector. Crearemos una operación que devuelve el entero retirado o -1 para esta demostración cuando se intenta extraer de una cola vacía.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == -1 || front > rear) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
```

**Mientras lo construimos, comprendamos cada parte.** int identifica el valor devuelto por desencolar. La condición usa || (OR): si front==-1 no hay elementos, o si front>rear ya pasamos el último. System.out.println muestra underflow y return -1 informa que no se retiró un dato. El convenio -1 es didáctico: si los datos válidos pudieran ser negativos, necesitaríamos distinguir el error de otro modo.

**Estado después y comprobación.** La operación puede rechazar una extracción inválida, pero aún no realiza una extracción correcta. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Leemos el dato antes de avanzar front; de otro modo perderíamos el valor que debía salir.

#### Paso 6 — Leer el frente, avanzar y reiniciar al vaciar

**Para comprender antes de escribir.** Ya protegimos la cola vacía. El dato que debe salir se encuentra exactamente en front; por eso se captura primero y solo después se adelanta el índice al siguiente elemento lógico.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            int dato = cola[front++];
            if (front > rear) {
                front = -1;
                rear = -1;
            }
            return dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** int dato almacena temporalmente el valor que abandona la cola. cola[front++] lee con el front actual y después incrementa ese índice: es el orden correcto de lectura y avance. Si front>rear tras esa lectura, salió el último elemento y no hay datos activos; ambos indicadores vuelven a -1. return dato entrega el valor capturado. No movemos elementos del arreglo; por eso una cola parcialmente vaciada puede bloquear una nueva inserción si rear quedó en el límite.

**Estado después y comprobación.** Para [10,20,30], la primera extracción devuelve 10 y front cambia 0→1; al retirar 30 finalmente, front y rear regresan a -1. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Falta crear el método main que compruebe la secuencia concreta del ejemplo.

#### Paso 7 — Completar el programa y ejecutar un caso reproducible

**Para comprender antes de escribir.** La estructura ya tiene las operaciones necesarias. Ahora cerramos la clase interna Cola, creamos main para ejecutar una secuencia de pruebas y cerramos la clase pública. No se deben compilar los pasos anteriores aislados: las llaves abiertas se completan en este último bloque.

**Dónde y cómo construirlo.** En `EJ03_OverflowLineal.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        Cola q = new Cola(5);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        q.encolar(40);
        q.encolar(50);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        q.encolar(60);
        System.out.println("front=" + q.front + ", rear=" + q.rear);
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** La llave anterior a main cierra Cola; public static void main(String[] args) es el punto inicial del programa. new Cola(capacidad) llama al constructor y las llamadas al objeto ejecutan las operaciones en el orden escrito. Las impresiones de estado permiten contrastar la traza manual con índices reales. Las últimas llaves cierran main y la clase pública.

**Estado después y comprobación.** El archivo ahora es sintácticamente completo y puede compilarse con javac; el orden de main determina la salida observable. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Comprobaremos predicciones y luego modificaremos solo las llamadas de prueba, sin alterar la lógica que acabamos de explicar.

### Traza manual: antes → operación → después

Estado físico: [10,20,30,40,50]. Estado lógico activo después de dos salidas: índices 2,3,4 con 30,40,50; índices 0,1 están fuera de la cola activa. rear sigue 4; encolar(60) falla.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ03_OverflowLineal {
    static class Cola {
        int[] cola;
        int front, rear, capacidad;
        Cola(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            front = -1;
            rear = -1;
        }
        void encolar(int dato) {
            if (rear == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
            if (front == -1) {
                front = 0;
            }
            cola[++rear] = dato;
            System.out.println("Encolado: " + dato);
        }
        int desencolar() {
            if (front == -1 || front > rear) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = cola[front++];
            if (front > rear) {
                front = -1;
                rear = -1;
            }
            return dato;
        }
    }

    public static void main(String[] args) {
        Cola q = new Cola(5);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        q.encolar(40);
        q.encolar(50);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        q.encolar(60);
        System.out.println("front=" + q.front + ", rear=" + q.rear);
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ03_OverflowLineal.java
java EJ03_OverflowLineal
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Encolado: 30
Encolado: 40
Encolado: 50
Sale: 10
Sale: 20
Error: Cola llena (Overflow)
front=2, rear=4
```

### Cómo interpretar la salida

Se observa overflow por falta de reutilización en una implementación estática concreta. Estado físico: [10,20,30,40,50]. Estado lógico activo después de dos salidas: índices 2,3,4 con 30,40,50; índices 0,1 están fuera de la cola activa. rear sigue 4; encolar(60) falla. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Sustituye en las pruebas encolar(60) por una extracción adicional y vuelve a encolar 60: seguirá bloqueado hasta vaciar o cambiar de estrategia.

### Error o caso límite controlado

Confundir “hay huecos antes de front” con “rear puede avanzar” genera una predicción falsa.

**Corrección razonada.** Comparar la condición rear==capacidad-1 con el estado front=2,rear=4. No corregir silenciosamente convirtiéndola en circular.

### Explícalo con tus palabras

¿Mover front libera inmediatamente una posición para encolar al final?

### T03 — tarea espejo de EJ03

**Enunciado.** Repetir el diagnóstico con capacidad 4, valores 11/22/33/44, extraer dos y probar 55; explicar indices y datos activos.

**Archivo que crearás:** `T03_OverflowLineal.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T03_OverflowLineal`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 11
Encolado: 22
Encolado: 33
Encolado: 44
Sale: 11
Sale: 22
Error: Cola llena (Overflow)
front=2, rear=3
```

---

## EJ04 — Reutilizar espacio desplazando elementos de un vector

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Construiremos una segunda cola con arreglo para contrastar con la limitación anterior. En lugar de dejar posiciones inactivas al frente, cada desencolado copiará los elementos pendientes una celda a la izquierda y reducirá fin. Introduciremos primero el arreglo y el indicador del final, luego encolar y el bucle de corrimiento; al final extraeremos 10 de [10,20,30] e insertaremos 40. Veremos que la nueva inserción sí cabe, pero comprenderemos por qué desencolar ahora requiere recorrer datos y su costo no es constante.

### ¿Qué problema concreto resolveremos?

¿Es posible recuperar los huecos del vector sin usar una cola circular, y qué trabajo adicional exige esa decisión?

**Archivo:** `EJ04_DesplazamientoVector.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: demostración central.

### Microconstrucción guiada

#### Paso 1 — Preparar cola con frente fijo

**Para comprender antes de escribir.** El ejemplo anterior dejó huecos inactivos al inicio. Ahora contrastaremos otra solución presente en la sesión: al desencolar, desplazar a la izquierda los elementos restantes. Se introduce una cola distinta para no cambiar silenciosamente el comportamiento del ejemplo anterior.

**Dónde y cómo construirlo.** Crea `EJ04_DesplazamientoVector.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ04_DesplazamientoVector {
    static class ColaLineal {
        int[] cola;
        int frente, fin, capacidad;
```

**Mientras lo construimos, comprendamos cada parte.** La clase ColaLineal se define como estructura propia dentro del archivo. El arreglo almacena valores; frente estará fijo en 0 después de cada desplazamiento; fin marca la última posición activa y capacidad es el límite físico. Estos nombres recuerdan el código de la presentación, pero el principio FIFO no cambia.

**Estado después y comprobación.** Solo declaramos los componentes: hay que reservarlos e indicar que inicialmente no existe último elemento. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** El constructor usará fin=-1 y frente=0 porque no habrá elementos activos.

#### Paso 2 — Inicializar indicadores consistentes

**Para comprender antes de escribir.** Para que el primer valor se guarde en la posición cero, fin debe empezar una posición antes, en -1. Aquí no se usa frente=-1 porque el método de extracción siempre trabajará en la posición cero, mientras fin indique si hay elementos.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        ColaLineal(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            frente = 0;
            fin = -1;
        }
```

**Mientras lo construimos, comprendamos cada parte.** new int reserva espacio. frente=0 fija dónde debe retirarse el próximo elemento tras desplazar; fin=-1 declara vacío. La elección de índices debe ser coherente con la condición de underflow que aparecerá a continuación.

**Estado después y comprobación.** Con tres celdas físicas, todavía no hay ninguna activa porque fin < frente. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Primero agregamos una inserción controlada para poder preparar el estado que desplazaremos después.

#### Paso 3 — Insertar al final con fin

**Para comprender antes de escribir.** Encolar sigue entrando por el extremo final; todavía no hay cambio de estrategia en la inserción. Proteger fin contra capacidad-1 evita acceder fuera del array.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            if (fin == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
            cola[++fin] = dato;
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** La condición impide overflow; return termina la llamada. cola[++fin] incrementa el último índice antes de guardar dato. Mostrar “Encolado” proporciona una pista de que la operación llegó hasta la escritura y no fue bloqueada.

**Estado después y comprobación.** Tres inserciones en capacidad 3 dejan frente=0, fin=2 y la secuencia [10,20,30]. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora crearemos desencolar: leerá la posición cero y conservará el valor antes de mover las demás.

#### Paso 4 — Proteger la salida y conservar el primero

**Para comprender antes de escribir.** Con el frente fijo en cero, una extracción sobre cola vacía sería inválida cuando fin está en -1. Una vez comprobado el estado, guardamos el valor del primer elemento antes de modificar el arreglo.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (frente > fin) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = cola[frente];
```

**Mientras lo construimos, comprendamos cada parte.** frente > fin expresa “no hay ninguna posición activa”. El return -1 se usa como señal didáctica; dato conserva el frente original. Guardarlo antes de copiar a la izquierda es fundamental: la próxima asignación sobrescribirá cola[0].

**Estado después y comprobación.** La extracción está protegida y hemos preservado el valor saliente, pero los elementos restantes aún ocupan sus posiciones originales. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** El bucle copiará cada sucesor sobre su predecesor y luego actualizará el último índice.

#### Paso 5 — Desplazar para reutilizar las posiciones iniciales

**Para comprender antes de escribir.** Al retirar el frente no avanzaremos un índice permanente. El programa realizará un corrimiento real que compacta la parte activa, recuperando el espacio inicial para una inserción posterior.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            for (int i = 0; i < fin; i++) {
                cola[i] = cola[i + 1];
            }
            fin--;
            return dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** i comienza en 0; i < fin evita leer cola[fin+1], que sería una posición ajena o fuera de límites; i++ avanza una celda por iteración. cola[i]=cola[i+1] copia el dato siguiente hacia la izquierda. fin-- reduce el tamaño lógico en uno. return dato entrega el valor guardado antes del bucle. El desplazamiento puede necesitar varias copias: por eso desencolar pasa a O(n) en este enfoque.

**Estado después y comprobación.** Antes [10,20,30], fin=2. Después de extraer 10: [20,30], fin=1; el índice 2 deja de pertenecer a la cola activa. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Añadiremos mostrar para comprobar el orden sin contar las celdas antiguas fuera del intervalo lógico.

#### Paso 6 — Visualizar únicamente elementos activos

**Para comprender antes de escribir.** Aunque int[] pueda conservar valores residuales después de un desencolado, el estado lógico solo incluye desde frente hasta fin. Por eso mostrar debe recorrer los límites activos y no todo el vector.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void mostrar() {
            System.out.print("Cola: ");
            for (int i = frente; i <= fin; i++) {
                System.out.print(cola[i] + " ");
            }
            System.out.println();
        }
```

**Mientras lo construimos, comprendamos cada parte.** print escribe sin terminar la línea; el for comienza en frente y se detiene tras fin gracias a <=. Cada valor activo se imprime con un espacio y println finaliza la salida. Cuando fin=-1, el bucle no tiene iteraciones, lo cual corresponde a una cola vacía.

**Estado después y comprobación.** El observador verá solo los valores que realmente esperan turno. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Probemos ahora la reutilización con una extracción seguida de una inserción.

#### Paso 7 — Ejecutar la recuperación por desplazamiento

**Para comprender antes de escribir.** Cerramos la clase ColaLineal y escribimos la secuencia completa. La nueva inserción después de desencolar debe funcionar, porque fin fue reducido durante el corrimiento.

**Dónde y cómo construirlo.** En `EJ04_DesplazamientoVector.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        ColaLineal q = new ColaLineal(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("Sale: " + q.desencolar());
        q.encolar(40);
        q.mostrar();
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** new ColaLineal(3) reserva tres posiciones; 10/20/30 llenan la cola; desencolar retira 10 y desplaza 20/30; encolar(40) usa el índice 2 recuperado. mostrar muestra 20 30 40, lo que permite comparar con el bloqueo del EJ03. El ejemplo no demuestra una cola circular: recupera espacio pagando el costo de desplazar elementos.

**Estado después y comprobación.** Se espera 20 30 40 en el mismo orden relativo de llegada entre los elementos aún activos. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Compararemos en voz alta el costo de cada implementación antes de pasar a las listas enlazadas.

### Traza manual: antes → operación → después

Antes de desencolar: [10,20,30], fin=2. Iteración i=0: [20,20,30]. i=1: [20,30,30]. fin--→1, cola activa [20,30]. Encolar 40→fin=2, activa [20,30,40].

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ04_DesplazamientoVector {
    static class ColaLineal {
        int[] cola;
        int frente, fin, capacidad;
        ColaLineal(int capacidad) {
            this.capacidad = capacidad;
            cola = new int[capacidad];
            frente = 0;
            fin = -1;
        }
        void encolar(int dato) {
            if (fin == capacidad - 1) {
                System.out.println("Error: Cola llena (Overflow)");
                return;
            }
            cola[++fin] = dato;
            System.out.println("Encolado: " + dato);
        }
        int desencolar() {
            if (frente > fin) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = cola[frente];
            for (int i = 0; i < fin; i++) {
                cola[i] = cola[i + 1];
            }
            fin--;
            return dato;
        }
        void mostrar() {
            System.out.print("Cola: ");
            for (int i = frente; i <= fin; i++) {
                System.out.print(cola[i] + " ");
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        ColaLineal q = new ColaLineal(3);
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        System.out.println("Sale: " + q.desencolar());
        q.encolar(40);
        q.mostrar();
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ04_DesplazamientoVector.java
java EJ04_DesplazamientoVector
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Encolado: 30
Sale: 10
Encolado: 40
Cola: 20 30 40
```

### Cómo interpretar la salida

La PPT incluye desplazamiento como estrategia alternativa a la cola que solo avanza índices. Antes de desencolar: [10,20,30], fin=2. Iteración i=0: [20,20,30]. i=1: [20,30,30]. fin--→1, cola activa [20,30]. Encolar 40→fin=2, activa [20,30,40]. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Cambia el tamaño a cuatro y desencola dos veces antes de encolar valores 50 y 60; debe mantenerse el orden relativo de los no retirados.

### Error o caso límite controlado

Un bucle i<=fin intenta leer cola[fin+1], incluso fuera del arreglo cuando está llena.

**Corrección razonada.** Conservar i<fin y decrementar fin exactamente una vez tras desplazar.

### Explícalo con tus palabras

¿Recuperar espacio es gratis en el vector con desplazamiento?

### T04 — tarea espejo de EJ04

**Enunciado.** Con capacidad 3, encolar 4/5/6, extraer uno e insertar 7. Mostrar el orden y explicar el costo O(n) de desencolar.

**Archivo que crearás:** `T04_DesplazamientoVector.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T04_DesplazamientoVector`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 4
Encolado: 5
Encolado: 6
Sale: 4
Encolado: 7
Cola: 5 6 7
```

---

## EJ05 — Encolar nodos conservando front y rear

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Ahora pasaremos del vector a una lista enlazada para eliminar la restricción de un tamaño fijo de arreglo. Primero construiremos la clase Nodo con su valor y una referencia siguiente; después, ColaLista conservará front y rear. Encolar distinguirá entre la primera inserción y las posteriores, enlazando siempre el nuevo nodo al final. Un recorrido de lectura hará visible la cadena 10→20→30 y demostrará que el primer nodo continúa siendo el frente aunque lleguen más valores.

### ¿Qué problema concreto resolveremos?

¿Cómo conservar el primer y el último nodo de una cola dinámica sin perder los enlaces entre ellos?

**Archivo:** `EJ05_EncolarEnlazada.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: demostración central.

### Microconstrucción guiada

#### Paso 1 — Describir qué conserva cada nodo

**Para comprender antes de escribir.** Una cola enlazada no necesita reservar un vector de tamaño fijo. Para unir elementos en orden, primero necesitamos un objeto capaz de conservar un entero y una referencia al siguiente. Ese papel lo desempeña Nodo.

**Dónde y cómo construirlo.** Crea `EJ05_EncolarEnlazada.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ05_EncolarEnlazada {
    static class Nodo {
        int dato;
        Nodo siguiente;
```

**Mientras lo construimos, comprendamos cada parte.** Nodo es una clase que representa una pieza de la cadena. int dato guarda el valor; Nodo siguiente guarda una referencia a otro objeto Nodo o null. No confundimos siguiente con una copia del dato: es un enlace. Al declarar la clase interna estática evitamos crear un objeto adicional de la clase de demostración.

**Estado después y comprobación.** Existe la forma del nodo, pero todavía no sabemos crear cada nodo con un dato inicial seguro. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Escribiremos un constructor que establezca valor y siguiente vacío.

#### Paso 2 — Construir nodos inicializados correctamente

**Para comprender antes de escribir.** El constructor establece el estado inicial de cada nodo. Cada nodo recién creado debe contener su valor y no apuntar accidentalmente a otro nodo.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
```

**Mientras lo construimos, comprendamos cada parte.** this.dato señala el atributo del nodo; el dato sin this es el parámetro recibido. siguiente=null significa que todavía no existe sucesor. La llave cierra Nodo, que queda listo para usarse desde ColaLista.

**Estado después y comprobación.** new Nodo(10) produce un nodo 10→null; no está en una cola todavía, pues falta definir referencias al frente y al final. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Introducimos ahora ColaLista para conservar las dos referencias de la cadena.

#### Paso 3 — Representar los dos extremos de la lista

**Para comprender antes de escribir.** Una sucesión de nodos sin una referencia inicial se perdería para el programa. Necesitamos recordar por dónde retiramos (front) y dónde agregamos (rear), ambas referencias de tipo Nodo.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    static class ColaLista {
        Nodo front, rear;
```

**Mientras lo construimos, comprendamos cada parte.** front y rear inicialmente valen null porque son campos de referencia no inicializados explícitamente en Java. front señala el primer nodo de la cola; rear señala el último. Cuando solo hay un nodo, ambas referencias deben apuntar al mismo objeto.

**Estado después y comprobación.** La estructura sabe qué referencias debe mantener, pero todavía no puede agregar nodos. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Encolar primero creará el nuevo nodo y distinguirá el caso vacío del caso con elementos.

#### Paso 4 — Resolver el primer ingreso sin enlaces perdidos

**Para comprender antes de escribir.** El primer ingreso es distinto de los siguientes: aún no existe un último nodo al que enlazar. Por eso encolar debe revisar rear antes de usar rear.siguiente.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
```

**Mientras lo construimos, comprendamos cada parte.** Nodo nuevo = new Nodo(dato) reserva un objeto con el valor recibido y siguiente=null. rear==null detecta cola vacía. front=nuevo y rear=nuevo hacen que el único nodo sea simultáneamente primero y último. else abre el comportamiento para cuando ya hay elementos, sin tocar front.

**Estado después y comprobación.** Después del primer ingreso de 10: front→10, rear→10, 10.siguiente=null. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Cuando la cola no está vacía, faltan dos operaciones en ese orden: enlazar el antiguo último y actualizar rear.

#### Paso 5 — Enlazar al final y desplazar rear

**Para comprender antes de escribir.** En una cola con elementos, el dato nuevo debe ir detrás del anterior rear. Si solo cambiamos rear, el nodo anterior no apuntaría al nuevo y perderíamos la continuidad de la lista.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** rear.siguiente=nuevo conecta el nodo final antiguo al nuevo. Solo después rear=nuevo actualiza la referencia al extremo. El mensaje confirma el éxito. front no se mueve durante encolar. Con 10→20 y una nueva inserción de 30, la cadena queda 10→20→30 y rear apunta a 30.

**Estado después y comprobación.** Ahora la cola puede construir una cadena FIFO, pero todavía necesitamos evidenciarla o extraerla según este ejemplo. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Añadimos la operación de salida cuando corresponda, conservando la misma semántica FIFO.

#### Paso 6 — Mostrar el recorrido desde front sin modificarlo

**Para comprender antes de escribir.** Para validar el orden de una lista enlazada no podemos inspeccionar índices como en un arreglo. Necesitamos comenzar en front y seguir las referencias hasta null, únicamente para mostrar los datos activos.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
```

**Mientras lo construimos, comprendamos cada parte.** Nodo actual=front crea una referencia auxiliar de recorrido: no mueve front. while(actual!=null) repite mientras existe nodo. actual.dato imprime el contenido; actual=actual.siguiente avanza al sucesor. Sin este avance, el ciclo nunca terminaría cuando la lista tuviera nodos. La última impresión cierra la línea.

**Estado después y comprobación.** Una cola de 10→20→30 se visualiza “Cola: 10 20 30 ” y conserva las referencias front y rear originales. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Solo queda cerrar las clases y agregar las llamadas que pongan a prueba el estado enseñado.

#### Paso 7 — Probar una cadena completa en Java

**Para comprender antes de escribir.** Hemos separado datos, enlaces y operaciones. Ahora cerraremos ColaLista y construiremos main para probar el orden real de inserción o extracción requerido.

**Dónde y cómo construirlo.** En `EJ05_EncolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        q.mostrar();
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** Las llaves cierran las clases y el punto de entrada main ejecuta las llamadas en el orden establecido. new ColaLista() construye el contenedor cuyas referencias comienzan en null; encolar/desencolar modifican la estructura y mostrar/estado permiten observarla. El archivo completo solo se compila después de este cierre.

**Estado después y comprobación.** La secuencia de consola permite comparar el dibujo de nodos con el comportamiento de las referencias. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Contrastaremos una variación de datos y un error relacionado con enlaces o vacío.

### Traza manual: antes → operación → después

Inicio front=null/rear=null. Tras 10: ambos→10. Tras 20: 10.siguiente→20, rear→20. Tras 30: 20.siguiente→30, rear→30. front permanece→10.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ05_EncolarEnlazada {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
    static class ColaLista {
        Nodo front, rear;
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        q.encolar(20);
        q.encolar(30);
        q.mostrar();
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ05_EncolarEnlazada.java
java EJ05_EncolarEnlazada
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Encolado: 30
Cola: 10 20 30
```

### Cómo interpretar la salida

La clase Nodo y la actualización de rear son indispensables para la cola dinámica. Inicio front=null/rear=null. Tras 10: ambos→10. Tras 20: 10.siguiente→20, rear→20. Tras 30: 20.siguiente→30, rear→30. front permanece→10. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Cambia el orden de inserción a 30, 10, 20 y predice el recorrido; el orden mostrado debe reflejar exactamente el orden de llegada.

### Error o caso límite controlado

Cambiar rear=nuevo antes de enlazar rear.siguiente=nuevo puede crear un enlace del nodo a sí mismo y romper el recorrido.

**Corrección razonada.** Primero rear.siguiente=nuevo usando la referencia antigua y después rear=nuevo.

### Explícalo con tus palabras

¿Por qué necesitamos Nodo y también ColaLista?

### T05 — tarea espejo de EJ05

**Enunciado.** Encolar 8, 16 y 24 en una cola enlazada y mostrar el orden exacto de los nodos.

**Archivo que crearás:** `T05_EncolarEnlazada.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T05_EncolarEnlazada`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 8
Encolado: 16
Encolado: 24
Cola: 8 16 24
```

---

## EJ06 — Desencolar nodos y restablecer referencias nulas

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

La lista enlazada ya puede agregar elementos, pero una cola requiere también extraerlos en el mismo orden en que llegaron. Añadiremos desencolar para capturar front.dato, pasar a front.siguiente y establecer rear=null cuando se retire el último. Luego intentaremos una extracción adicional para verificar underflow y volveremos a encolar. El resultado nos permitirá reconocer una diferencia importante con el vector: para avanzar el frente basta cambiar una referencia, sin desplazar los demás datos.

### ¿Qué problema concreto resolveremos?

¿Por qué, al sacar el último nodo, es incorrecto dejar rear apuntando a un elemento que ya no pertenece a la cola?

**Archivo:** `EJ06_DesencolarEnlazada.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: práctica acompañada.

### Microconstrucción guiada

#### Paso 1 — Describir qué conserva cada nodo

**Para comprender antes de escribir.** Una cola enlazada no necesita reservar un vector de tamaño fijo. Para unir elementos en orden, primero necesitamos un objeto capaz de conservar un entero y una referencia al siguiente. Ese papel lo desempeña Nodo.

**Dónde y cómo construirlo.** Crea `EJ06_DesencolarEnlazada.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ06_DesencolarEnlazada {
    static class Nodo {
        int dato;
        Nodo siguiente;
```

**Mientras lo construimos, comprendamos cada parte.** Nodo es una clase que representa una pieza de la cadena. int dato guarda el valor; Nodo siguiente guarda una referencia a otro objeto Nodo o null. No confundimos siguiente con una copia del dato: es un enlace. Al declarar la clase interna estática evitamos crear un objeto adicional de la clase de demostración.

**Estado después y comprobación.** Existe la forma del nodo, pero todavía no sabemos crear cada nodo con un dato inicial seguro. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Escribiremos un constructor que establezca valor y siguiente vacío.

#### Paso 2 — Construir nodos inicializados correctamente

**Para comprender antes de escribir.** El constructor establece el estado inicial de cada nodo. Cada nodo recién creado debe contener su valor y no apuntar accidentalmente a otro nodo.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
```

**Mientras lo construimos, comprendamos cada parte.** this.dato señala el atributo del nodo; el dato sin this es el parámetro recibido. siguiente=null significa que todavía no existe sucesor. La llave cierra Nodo, que queda listo para usarse desde ColaLista.

**Estado después y comprobación.** new Nodo(10) produce un nodo 10→null; no está en una cola todavía, pues falta definir referencias al frente y al final. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Introducimos ahora ColaLista para conservar las dos referencias de la cadena.

#### Paso 3 — Representar los dos extremos de la lista

**Para comprender antes de escribir.** Una sucesión de nodos sin una referencia inicial se perdería para el programa. Necesitamos recordar por dónde retiramos (front) y dónde agregamos (rear), ambas referencias de tipo Nodo.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    static class ColaLista {
        Nodo front, rear;
```

**Mientras lo construimos, comprendamos cada parte.** front y rear inicialmente valen null porque son campos de referencia no inicializados explícitamente en Java. front señala el primer nodo de la cola; rear señala el último. Cuando solo hay un nodo, ambas referencias deben apuntar al mismo objeto.

**Estado después y comprobación.** La estructura sabe qué referencias debe mantener, pero todavía no puede agregar nodos. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Encolar primero creará el nuevo nodo y distinguirá el caso vacío del caso con elementos.

#### Paso 4 — Resolver el primer ingreso sin enlaces perdidos

**Para comprender antes de escribir.** El primer ingreso es distinto de los siguientes: aún no existe un último nodo al que enlazar. Por eso encolar debe revisar rear antes de usar rear.siguiente.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
```

**Mientras lo construimos, comprendamos cada parte.** Nodo nuevo = new Nodo(dato) reserva un objeto con el valor recibido y siguiente=null. rear==null detecta cola vacía. front=nuevo y rear=nuevo hacen que el único nodo sea simultáneamente primero y último. else abre el comportamiento para cuando ya hay elementos, sin tocar front.

**Estado después y comprobación.** Después del primer ingreso de 10: front→10, rear→10, 10.siguiente=null. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Cuando la cola no está vacía, faltan dos operaciones en ese orden: enlazar el antiguo último y actualizar rear.

#### Paso 5 — Enlazar al final y desplazar rear

**Para comprender antes de escribir.** En una cola con elementos, el dato nuevo debe ir detrás del anterior rear. Si solo cambiamos rear, el nodo anterior no apuntaría al nuevo y perderíamos la continuidad de la lista.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** rear.siguiente=nuevo conecta el nodo final antiguo al nuevo. Solo después rear=nuevo actualiza la referencia al extremo. El mensaje confirma el éxito. front no se mueve durante encolar. Con 10→20 y una nueva inserción de 30, la cadena queda 10→20→30 y rear apunta a 30.

**Estado después y comprobación.** Ahora la cola puede construir una cadena FIFO, pero todavía necesitamos evidenciarla o extraerla según este ejemplo. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Añadimos la operación de salida cuando corresponda, conservando la misma semántica FIFO.

#### Paso 6 — Capturar el primer valor sin causar underflow

**Para comprender antes de escribir.** Para retirar sin perder el dato, primero debemos asegurarnos de que front referencia un nodo real. Después conservamos su contenido antes de cambiar el enlace de inicio.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == null) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = front.dato;
```

**Mientras lo construimos, comprendamos cada parte.** front==null indica cola vacía; return -1 detiene esa lectura inválida. int dato=front.dato copia el valor lógico que corresponde al primero en entrar. No se debe usar rear.dato porque extraería por el extremo incorrecto.

**Estado después y comprobación.** Con 10→20→30, dato guarda 10 mientras front sigue apuntando al nodo 10. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora se puede avanzar front al siguiente nodo y reparar rear si se retiró el último.

#### Paso 7 — Avanzar front y reparar rear al vaciar

**Para comprender antes de escribir.** Después de guardar el dato, hay que olvidar el nodo que abandona la cola. El nuevo primer elemento es el siguiente del antiguo front. Si este enlace es null, la cola se ha vaciado por completo y no puede quedar rear apuntando a un nodo desconectado.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            front = front.siguiente;
            if (front == null) {
                rear = null;
            }
            return dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** front=front.siguiente avanza una referencia, sin copiar todos los nodos; if(front==null) detecta la salida del último; rear=null restablece el estado vacío coherente. return dato entrega lo guardado. Este orden mantiene las operaciones de inserción y extracción en tiempo O(1) para esta implementación enlazada.

**Estado después y comprobación.** Tras retirar 10 de 10→20→30, front apunta a 20. Tras retirar el último, front y rear son null. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Comprobaremos que la secuencia real respete ese estado y permite volver a encolar.

#### Paso 8 — Mostrar el recorrido desde front sin modificarlo

**Para comprender antes de escribir.** Para validar el orden de una lista enlazada no podemos inspeccionar índices como en un arreglo. Necesitamos comenzar en front y seguir las referencias hasta null, únicamente para mostrar los datos activos.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 7**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
```

**Mientras lo construimos, comprendamos cada parte.** Nodo actual=front crea una referencia auxiliar de recorrido: no mueve front. while(actual!=null) repite mientras existe nodo. actual.dato imprime el contenido; actual=actual.siguiente avanza al sucesor. Sin este avance, el ciclo nunca terminaría cuando la lista tuviera nodos. La última impresión cierra la línea.

**Estado después y comprobación.** Una cola de 10→20→30 se visualiza “Cola: 10 20 30 ” y conserva las referencias front y rear originales. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Solo queda cerrar las clases y agregar las llamadas que pongan a prueba el estado enseñado.

#### Paso 9 — Probar una cadena completa en Java

**Para comprender antes de escribir.** Hemos separado datos, enlaces y operaciones. Ahora cerraremos ColaLista y construiremos main para probar el orden real de inserción o extracción requerido.

**Dónde y cómo construirlo.** En `EJ06_DesencolarEnlazada.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 8**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        q.encolar(20);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Vacia: " + q.desencolar());
        q.encolar(30);
        q.mostrar();
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** Las llaves cierran las clases y el punto de entrada main ejecuta las llamadas en el orden establecido. new ColaLista() construye el contenedor cuyas referencias comienzan en null; encolar/desencolar modifican la estructura y mostrar/estado permiten observarla. El archivo completo solo se compila después de este cierre.

**Estado después y comprobación.** La secuencia de consola permite comparar el dibujo de nodos con el comportamiento de las referencias. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Contrastaremos una variación de datos y un error relacionado con enlaces o vacío.

### Traza manual: antes → operación → después

10→20. Sale 10: front→20, rear→20. Sale 20: front=null y rear=null. Otra salida: underflow. Encolar 30: front=rear→30.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ06_DesencolarEnlazada {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
    static class ColaLista {
        Nodo front, rear;
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
        int desencolar() {
            if (front == null) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = front.dato;
            front = front.siguiente;
            if (front == null) {
                rear = null;
            }
            return dato;
        }
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        q.encolar(20);
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Sale: " + q.desencolar());
        System.out.println("Vacia: " + q.desencolar());
        q.encolar(30);
        q.mostrar();
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ06_DesencolarEnlazada.java
java EJ06_DesencolarEnlazada
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Encolado: 20
Sale: 10
Sale: 20
Error: Cola vacia (Underflow)
Vacia: -1
Encolado: 30
Cola: 30
```

### Cómo interpretar la salida

La extracción enlazada es O(1) y exige preservar un estado vacío coherente. 10→20. Sale 10: front→20, rear→20. Sale 20: front=null y rear=null. Otra salida: underflow. Encolar 30: front=rear→30. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Agrega un tercer nodo antes de desencolar. Verifica que salir de uno no altera el enlace de los otros dos.

### Error o caso límite controlado

Si se omite rear=null al vaciar, una nueva inserción podría enlazarse a un nodo inaccesible desde front.

**Corrección razonada.** Aplicar rear=null exactamente cuando front se vuelve null después de avanzar.

### Explícalo con tus palabras

¿Por qué el valor de salida se lee antes de hacer front=front.siguiente?

### T06 — tarea espejo de EJ06

**Enunciado.** Encolar 12 y 24, desencolar dos veces, comprobar underflow y encolar 36 nuevamente; mostrar la cola resultante.

**Archivo que crearás:** `T06_DesencolarEnlazada.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T06_DesencolarEnlazada`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 12
Encolado: 24
Sale: 12
Sale: 24
Error: Cola vacia (Underflow)
Vacia: -1
Encolado: 36
Cola: 36
```

---

## EJ07 — Diagnosticar la referencia rear olvidada

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Una implementación puede parecer correcta mientras contiene datos y fallar cuando la cola queda vacía. Reproduciremos esa situación con una prueba de referencias: extraeremos el único nodo, observaremos si tanto front como rear son null y volveremos a insertar. Introduciremos un método estado que solo lee ambas referencias para que la evidencia no dependa del azar. Después compararemos el comportamiento correcto con el error controlado de olvidar rear=null; así aprenderemos a depurar relaciones, no solo mensajes de consola.

### ¿Qué problema concreto resolveremos?

¿Cómo distinguir un underflow evidente de un error más sutil en el que front es null pero rear todavía apunta a un nodo desconectado?

**Archivo:** `EJ07_DepurarReferencias.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Aula: práctica acompañada.

### Microconstrucción guiada

#### Paso 1 — Describir qué conserva cada nodo

**Para comprender antes de escribir.** Una cola enlazada no necesita reservar un vector de tamaño fijo. Para unir elementos en orden, primero necesitamos un objeto capaz de conservar un entero y una referencia al siguiente. Ese papel lo desempeña Nodo.

**Dónde y cómo construirlo.** Crea `EJ07_DepurarReferencias.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ07_DepurarReferencias {
    static class Nodo {
        int dato;
        Nodo siguiente;
```

**Mientras lo construimos, comprendamos cada parte.** Nodo es una clase que representa una pieza de la cadena. int dato guarda el valor; Nodo siguiente guarda una referencia a otro objeto Nodo o null. No confundimos siguiente con una copia del dato: es un enlace. Al declarar la clase interna estática evitamos crear un objeto adicional de la clase de demostración.

**Estado después y comprobación.** Existe la forma del nodo, pero todavía no sabemos crear cada nodo con un dato inicial seguro. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Escribiremos un constructor que establezca valor y siguiente vacío.

#### Paso 2 — Construir nodos inicializados correctamente

**Para comprender antes de escribir.** El constructor establece el estado inicial de cada nodo. Cada nodo recién creado debe contener su valor y no apuntar accidentalmente a otro nodo.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
```

**Mientras lo construimos, comprendamos cada parte.** this.dato señala el atributo del nodo; el dato sin this es el parámetro recibido. siguiente=null significa que todavía no existe sucesor. La llave cierra Nodo, que queda listo para usarse desde ColaLista.

**Estado después y comprobación.** new Nodo(10) produce un nodo 10→null; no está en una cola todavía, pues falta definir referencias al frente y al final. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Introducimos ahora ColaLista para conservar las dos referencias de la cadena.

#### Paso 3 — Representar los dos extremos de la lista

**Para comprender antes de escribir.** Una sucesión de nodos sin una referencia inicial se perdería para el programa. Necesitamos recordar por dónde retiramos (front) y dónde agregamos (rear), ambas referencias de tipo Nodo.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    static class ColaLista {
        Nodo front, rear;
```

**Mientras lo construimos, comprendamos cada parte.** front y rear inicialmente valen null porque son campos de referencia no inicializados explícitamente en Java. front señala el primer nodo de la cola; rear señala el último. Cuando solo hay un nodo, ambas referencias deben apuntar al mismo objeto.

**Estado después y comprobación.** La estructura sabe qué referencias debe mantener, pero todavía no puede agregar nodos. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Encolar primero creará el nuevo nodo y distinguirá el caso vacío del caso con elementos.

#### Paso 4 — Resolver el primer ingreso sin enlaces perdidos

**Para comprender antes de escribir.** El primer ingreso es distinto de los siguientes: aún no existe un último nodo al que enlazar. Por eso encolar debe revisar rear antes de usar rear.siguiente.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
```

**Mientras lo construimos, comprendamos cada parte.** Nodo nuevo = new Nodo(dato) reserva un objeto con el valor recibido y siguiente=null. rear==null detecta cola vacía. front=nuevo y rear=nuevo hacen que el único nodo sea simultáneamente primero y último. else abre el comportamiento para cuando ya hay elementos, sin tocar front.

**Estado después y comprobación.** Después del primer ingreso de 10: front→10, rear→10, 10.siguiente=null. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Cuando la cola no está vacía, faltan dos operaciones en ese orden: enlazar el antiguo último y actualizar rear.

#### Paso 5 — Enlazar al final y desplazar rear

**Para comprender antes de escribir.** En una cola con elementos, el dato nuevo debe ir detrás del anterior rear. Si solo cambiamos rear, el nodo anterior no apuntaría al nuevo y perderíamos la continuidad de la lista.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
```

**Mientras lo construimos, comprendamos cada parte.** rear.siguiente=nuevo conecta el nodo final antiguo al nuevo. Solo después rear=nuevo actualiza la referencia al extremo. El mensaje confirma el éxito. front no se mueve durante encolar. Con 10→20 y una nueva inserción de 30, la cadena queda 10→20→30 y rear apunta a 30.

**Estado después y comprobación.** Ahora la cola puede construir una cadena FIFO, pero todavía necesitamos evidenciarla o extraerla según este ejemplo. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Añadimos la operación de salida cuando corresponda, conservando la misma semántica FIFO.

#### Paso 6 — Capturar el primer valor sin causar underflow

**Para comprender antes de escribir.** Para retirar sin perder el dato, primero debemos asegurarnos de que front referencia un nodo real. Después conservamos su contenido antes de cambiar el enlace de inicio.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == null) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = front.dato;
```

**Mientras lo construimos, comprendamos cada parte.** front==null indica cola vacía; return -1 detiene esa lectura inválida. int dato=front.dato copia el valor lógico que corresponde al primero en entrar. No se debe usar rear.dato porque extraería por el extremo incorrecto.

**Estado después y comprobación.** Con 10→20→30, dato guarda 10 mientras front sigue apuntando al nodo 10. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Ahora se puede avanzar front al siguiente nodo y reparar rear si se retiró el último.

#### Paso 7 — Avanzar front y reparar rear al vaciar

**Para comprender antes de escribir.** Después de guardar el dato, hay que olvidar el nodo que abandona la cola. El nuevo primer elemento es el siguiente del antiguo front. Si este enlace es null, la cola se ha vaciado por completo y no puede quedar rear apuntando a un nodo desconectado.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
            front = front.siguiente;
            if (front == null) {
                rear = null;
            }
            return dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** front=front.siguiente avanza una referencia, sin copiar todos los nodos; if(front==null) detecta la salida del último; rear=null restablece el estado vacío coherente. return dato entrega lo guardado. Este orden mantiene las operaciones de inserción y extracción en tiempo O(1) para esta implementación enlazada.

**Estado después y comprobación.** Tras retirar 10 de 10→20→30, front apunta a 20. Tras retirar el último, front y rear son null. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Comprobaremos que la secuencia real respete ese estado y permite volver a encolar.

#### Paso 8 — Hacer visible una referencia final olvidada

**Para comprender antes de escribir.** El error que estudiaremos no siempre produce una excepción inmediata: un rear colgante puede mantenerse oculto. Agregamos una inspección simple del estado para distinguir cola vacía coherente de referencias contradictorias.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 7**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void estado() {
            System.out.println("front null=" + (front == null)
                    + ", rear null=" + (rear == null));
        }
```

**Mientras lo construimos, comprendamos cada parte.** El operador == null comprueba si cada extremo está sin nodo. Los paréntesis producen valores booleanos; + concatena esos resultados al mensaje. No cambia ninguna referencia: solo hace visible si ambas están vacías al mismo tiempo.

**Estado después y comprobación.** Después de extraer el último nodo, el resultado correcto será front null=true, rear null=true. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** La prueba main provocará el estado vacío y luego una inserción para descubrir si rear fue reparado.

#### Paso 9 — Mostrar el recorrido desde front sin modificarlo

**Para comprender antes de escribir.** Para validar el orden de una lista enlazada no podemos inspeccionar índices como en un arreglo. Necesitamos comenzar en front y seguir las referencias hasta null, únicamente para mostrar los datos activos.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 8**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
```

**Mientras lo construimos, comprendamos cada parte.** Nodo actual=front crea una referencia auxiliar de recorrido: no mueve front. while(actual!=null) repite mientras existe nodo. actual.dato imprime el contenido; actual=actual.siguiente avanza al sucesor. Sin este avance, el ciclo nunca terminaría cuando la lista tuviera nodos. La última impresión cierra la línea.

**Estado después y comprobación.** Una cola de 10→20→30 se visualiza “Cola: 10 20 30 ” y conserva las referencias front y rear originales. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Solo queda cerrar las clases y agregar las llamadas que pongan a prueba el estado enseñado.

#### Paso 10 — Probar una cadena completa en Java

**Para comprender antes de escribir.** Hemos separado datos, enlaces y operaciones. Ahora cerraremos ColaLista y construiremos main para probar el orden real de inserción o extracción requerido.

**Dónde y cómo construirlo.** En `EJ07_DepurarReferencias.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 9**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        System.out.println("Sale: " + q.desencolar());
        q.estado();
        q.encolar(20);
        q.mostrar();
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** Las llaves cierran las clases y el punto de entrada main ejecuta las llamadas en el orden establecido. new ColaLista() construye el contenedor cuyas referencias comienzan en null; encolar/desencolar modifican la estructura y mostrar/estado permiten observarla. El archivo completo solo se compila después de este cierre.

**Estado después y comprobación.** La secuencia de consola permite comparar el dibujo de nodos con el comportamiento de las referencias. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** Contrastaremos una variación de datos y un error relacionado con enlaces o vacío.

### Traza manual: antes → operación → después

Después de sacar 10, front=null y rear=null. Reencolar 20 entra por la rama rear==null: ambos vuelven a apuntar a 20. Si rear no se limpia, la rama else enlazaría 20 a un nodo ya desconectado, dejando front null.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ07_DepurarReferencias {
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) {
            this.dato = dato;
            this.siguiente = null;
        }
    }
    static class ColaLista {
        Nodo front, rear;
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = nuevo;
                rear = nuevo;
            } else {
                rear.siguiente = nuevo;
                rear = nuevo;
            }
            System.out.println("Encolado: " + dato);
        }
        int desencolar() {
            if (front == null) {
                System.out.println("Error: Cola vacia (Underflow)");
                return -1;
            }
            int dato = front.dato;
            front = front.siguiente;
            if (front == null) {
                rear = null;
            }
            return dato;
        }
        void estado() {
            System.out.println("front null=" + (front == null)
                    + ", rear null=" + (rear == null));
        }
        void mostrar() {
            Nodo actual = front;
            System.out.print("Cola: ");
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        ColaLista q = new ColaLista();
        q.encolar(10);
        System.out.println("Sale: " + q.desencolar());
        q.estado();
        q.encolar(20);
        q.mostrar();
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ07_DepurarReferencias.java
java EJ07_DepurarReferencias
```

### Resultado esperado (comparar literalmente)

```text
Encolado: 10
Sale: 10
front null=true, rear null=true
Encolado: 20
Cola: 20
```

### Cómo interpretar la salida

Se pone a prueba debugging de punteros y refactorización responsable con IA. Después de sacar 10, front=null y rear=null. Reencolar 20 entra por la rama rear==null: ambos vuelven a apuntar a 20. Si rear no se limpia, la rama else enlazaría 20 a un nodo ya desconectado, dejando front null. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Comenta temporalmente rear = null; y observa estado y mostrar. Deshaz el cambio antes de continuar: el archivo docente canónico permanece correcto.

### Error o caso límite controlado

Bug: rear referencia el nodo retirado. El programa puede imprimir “Encolado: 20” aunque mostrar quede vacío.

**Corrección razonada.** La condición if(front==null){rear=null;} no es decorativa: mantiene el invariante ambos extremos nulos cuando la cola está vacía.

### Explícalo con tus palabras

¿Un mensaje “Encolado” confirma por sí solo que front alcanza el nuevo nodo?

### T07 — tarea espejo de EJ07

**Enunciado.** Corregir un starter con rear=null deliberadamente omitido; mostrar que tras extraer 11 ambos extremos están nulos y la posterior inserción 22 se puede recorrer.

**Archivo que crearás:** `T07_DepurarReferencias.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T07_DepurarReferencias`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Fragmento defectuoso proporcionado para T07 (sustituye solamente estas líneas de `desencolar` en una copia de tu archivo):**

```java
            front = front.siguiente;
            if (front == null) {
                // Error intencional: se omitió actualizar rear.
            }
            return dato;
```

Después de reproducir la inconsistencia, corrige la decisión del `if` para que al vaciarse **ambas referencias queden nulas**. Este bloque se proporciona para diagnosticarlo sin depender del laboratorio descargable.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Encolado: 11
Sale: 11
front null=true, rear null=true
Encolado: 22
Cola: 22
```

---

## EJ08 — Contrastar cola lineal y cola enlazada con la misma secuencia

### Objetivo técnico — qué vamos a hacer y qué aprenderemos

Cerraremos la sesión sometiendo dos estructuras a la misma serie de operaciones. Ambas aceptarán 10, 20 y 30 y retirarán 10 y 20. Después intentaremos insertar 40: el vector lineal sin desplazamiento rechazará esa operación porque rear continúa en el último índice, mientras la cola enlazada añadirá un nodo. Construiremos ambas implementaciones en un programa pequeño, observaremos sus estados y justificaremos cuándo conviene cada alternativa y qué costo real tiene desencolar.

### ¿Qué problema concreto resolveremos?

¿Puede una implementación respetar FIFO y, aun así, resultar inadecuada por su forma de gestionar posiciones disponibles?

**Archivo:** `EJ08_CompararImplementaciones.java`. **Punto de partida:** archivo nuevo, sin contenido. **Estado final esperado:** una clase pública ejecutable con el mismo nombre del archivo. **Rol en la sesión:** Autónomo: integrador.

### Microconstrucción guiada

#### Paso 1 — Preparar cola lineal para una comparación justa

**Para comprender antes de escribir.** La prueba final compara exactamente la misma secuencia en dos implementaciones de cola. Crearemos en un solo archivo dos estructuras con nombres inequívocos, para no mezclar sus índices con las referencias de la lista.

**Dónde y cómo construirlo.** Crea `EJ08_CompararImplementaciones.java` vacío en `S10_Colas_FIFO` y escribe el fragmento desde la primera línea.

**Escribe ahora:**

```java
public class EJ08_CompararImplementaciones {
    static class ColaVector {
        int[] cola;
        int front = -1, rear = -1;
```

**Mientras lo construimos, comprendamos cada parte.** ColaVector agrupa el arreglo y los indicadores: front y rear comienzan en -1. Se conserva la misma semántica de EJ02 y EJ03: encolar por rear, desencolar por front, sin desplazamiento.

**Estado después y comprobación.** Tenemos el estado vacío, pero aún hay que crear y operar el vector. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Incorporamos un constructor que reserva tres posiciones.

#### Paso 2 — Completar encolar con límite físico

**Para comprender antes de escribir.** Aquí no necesitamos duplicar la variable capacidad: el propio arreglo informa cuántas celdas reservó. La comprobación de último índice sigue teniendo el mismo sentido.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 1**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        ColaVector(int capacidad) {
            cola = new int[capacidad];
        }
        void encolar(int dato) {
            if (rear == cola.length - 1) {
                System.out.println("Vector: Overflow");
                return;
            }
            if (front == -1) front = 0;
            cola[++rear] = dato;
        }
```

**Mientras lo construimos, comprendamos cada parte.** cola.length devuelve la capacidad del vector; rear==cola.length-1 rechaza la inserción si alcanzó el límite. front=0 solo al primer ingreso, y cola[++rear]=dato escribe al final. La impresión se utiliza únicamente en el bloqueo, para que la diferencia respecto de la lista sea visible.

**Estado después y comprobación.** Con 10,20,30 la cola vectorial queda completa y rear vale 2. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Añadimos su extracción FIFO para que la prueba pueda abrir huecos lógicos.

#### Paso 3 — Desencolar sin recuperar huecos delanteros

**Para comprender antes de escribir.** La extracción conserva el mismo algoritmo del ejemplo anterior: se lee el índice front y se incrementa, sin desplazar elementos. No incorporamos una cola circular ni reajustamos rear mientras existan elementos.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 2**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == -1 || front > rear) return -1;
            int dato = cola[front++];
            if (front > rear) front = rear = -1;
            return dato;
        }
    }
```

**Mientras lo construimos, comprendamos cada parte.** La condición con || evita leer estando vacío. cola[front++] obtiene el elemento saliente y avanza el frente; al quitar el último, la asignación encadenada restablece ambos a -1. Se cierra ColaVector porque la parte lineal está lista.

**Estado después y comprobación.** Después de sacar 10 y 20, la cola activa queda en índice 2 con 30, pero rear sigue en 2. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Crearemos ahora Nodo y ColaEnlazada como contraste, reutilizando relaciones ya aprendidas.

#### Paso 4 — Representar la cola sin capacidad fija de arreglo

**Para comprender antes de escribir.** La segunda implementación no tiene arreglo: cada llegada necesita su propio nodo. Conservamos Nodo con dato y siguiente, además de las dos referencias a extremos.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 3**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
    static class ColaEnlazada {
        Nodo front, rear;
```

**Mientras lo construimos, comprendamos cada parte.** El constructor Nodo(int dato) copia el parámetro al atributo; siguiente comienza en null por la inicialización de referencias de Java. ColaEnlazada conserva front y rear, también inicialmente null. Los nombres coinciden con los conceptos que ya hemos utilizado en EJ05–EJ07.

**Estado después y comprobación.** Ambas implementaciones están definidas, pero falta el comportamiento de inserción y salida de la lista. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Recuperamos encolar enlazado con el caso inicial y el enlace del antiguo último.

#### Paso 5 — Enlazar nuevos nodos al último

**Para comprender antes de escribir.** Al no reservar posiciones numeradas, la lista permite agregar un nodo aunque dos anteriores ya hayan salido. Mantener front y rear correctamente es la única condición de esta inserción.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 4**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = rear = nuevo;
            } else {
                rear.siguiente = nuevo;
                rear = nuevo;
            }
        }
```

**Mientras lo construimos, comprendamos cada parte.** new Nodo(dato) crea el elemento; rear==null detecta cola vacía; la asignación encadenada front=rear=nuevo señala el único nodo. En caso contrario, rear.siguiente=nuevo mantiene accesible la cadena y rear=nuevo actualiza el final. No hay comprobación contra capacidad física: se supone memoria suficiente para el ejemplo académico.

**Estado después y comprobación.** La secuencia 10→20→30 queda enlazada y puede seguir creciendo al insertar 40 después de dos salidas. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Falta la extracción de la lista para que ambos contenedores reciban operaciones equivalentes.

#### Paso 6 — Desencolar y conservar un estado enlazado válido

**Para comprender antes de escribir.** La comparación sería incorrecta si una estructura desencolara y la otra no. Añadimos exactamente la misma operación FIFO: obtener el valor frontal y avanzar una posición lógica.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 5**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
        int desencolar() {
            if (front == null) return -1;
            int dato = front.dato;
            front = front.siguiente;
            if (front == null) rear = null;
            return dato;
        }
    }
```

**Mientras lo construimos, comprendamos cada parte.** front==null evita underflow; dato captura el entero; front=front.siguiente adelanta la referencia; rear=null repara el estado cuando se retira el último nodo. Se cierra ColaEnlazada. No hay un recorrido de desplazamiento de nodos.

**Estado después y comprobación.** Tras dos salidas en la lista, front apunta a 30 y rear apunta a 30; ambos siguen siendo válidos. Todavía no compiles: las clases o métodos pueden seguir abiertos.

**Puente hacia la siguiente decisión.** Con ambas estructuras completas podemos ejecutar el mismo guion de operaciones y comparar la diferencia.

#### Paso 7 — Verificar el mismo caso en dos implementaciones

**Para comprender antes de escribir.** El ensayo usa dos colas con la misma entrada y las mismas extracciones. Se revela una diferencia de capacidad de representación, no una diferencia del principio FIFO.

**Dónde y cómo construirlo.** En `EJ08_CompararImplementaciones.java`, sitúa el cursor **inmediatamente después de la última línea escrita en el paso 6**, fuera de cualquier texto anterior y conservando las llaves abiertas; **anexa** el siguiente fragmento sin borrar nada.

**Escribe ahora:**

```java
    public static void main(String[] args) {
        ColaVector v = new ColaVector(3);
        ColaEnlazada e = new ColaEnlazada();
        v.encolar(10); e.encolar(10);
        v.encolar(20); e.encolar(20);
        v.encolar(30); e.encolar(30);
        System.out.println("Sale vector=" + v.desencolar()
                + ", enlazada=" + e.desencolar());
        System.out.println("Sale vector=" + v.desencolar()
                + ", enlazada=" + e.desencolar());
        v.encolar(40); e.encolar(40);
        System.out.println("Vector: front=" + v.front + ", rear=" + v.rear);
        System.out.println("Enlazada: front=" + e.front.dato
                + ", rear=" + e.rear.dato);
    }
}
```

**Mientras lo construimos, comprendamos cada parte.** v reserva capacidad tres; e crea nodos según lleguen datos. Ambas reciben 10/20/30 y devuelven 10, luego 20. Al insertar 40, el vector sigue con rear=2 y muestra Overflow; la lista crea un nodo 40 al final. El vector informa front=2/rear=2; la lista informa front=30/rear=40 a partir de los datos a los que apuntan sus referencias.

**Estado después y comprobación.** Una misma secuencia produce salida FIFO 10 y 20 en ambos casos, pero solo la lista admite el cuarto ingreso sin un arreglo fijo. Ya puedes compilar: las llaves y las clases están cerradas.

**Puente hacia la siguiente decisión.** El criterio de diseño depende de los costos y las restricciones, no de cuál estructura parece más moderna.

### Traza manual: antes → operación → después

Ambas [10,20,30]. Tras dos salidas: vector front=2,rear=2; enlazada nodo único 30. Inserción 40: vector overflow por rear=2; enlazada 30→40. Ambas respetaron FIFO.

### Auditoría del código construido

**Almacenamiento:** identifica cómo se reservan celdas o nodos. **Frente:** señala dónde sale el siguiente dato. **Final:** señala dónde se agrega. **Protección:** localiza el `if` de overflow o underflow, si corresponde. **Actualización:** indica exactamente qué asignación, incremento o enlace cambia el estado. **Validación:** identifica la impresión que prueba esa consecuencia. Cada una de estas decisiones se introdujo y explicó en los pasos anteriores.

### Archivo completo al terminar este ejemplo

```java
public class EJ08_CompararImplementaciones {
    static class ColaVector {
        int[] cola;
        int front = -1, rear = -1;
        ColaVector(int capacidad) {
            cola = new int[capacidad];
        }
        void encolar(int dato) {
            if (rear == cola.length - 1) {
                System.out.println("Vector: Overflow");
                return;
            }
            if (front == -1) front = 0;
            cola[++rear] = dato;
        }
        int desencolar() {
            if (front == -1 || front > rear) return -1;
            int dato = cola[front++];
            if (front > rear) front = rear = -1;
            return dato;
        }
    }
    static class Nodo {
        int dato;
        Nodo siguiente;
        Nodo(int dato) { this.dato = dato; }
    }
    static class ColaEnlazada {
        Nodo front, rear;
        void encolar(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (rear == null) {
                front = rear = nuevo;
            } else {
                rear.siguiente = nuevo;
                rear = nuevo;
            }
        }
        int desencolar() {
            if (front == null) return -1;
            int dato = front.dato;
            front = front.siguiente;
            if (front == null) rear = null;
            return dato;
        }
    }
    public static void main(String[] args) {
        ColaVector v = new ColaVector(3);
        ColaEnlazada e = new ColaEnlazada();
        v.encolar(10); e.encolar(10);
        v.encolar(20); e.encolar(20);
        v.encolar(30); e.encolar(30);
        System.out.println("Sale vector=" + v.desencolar()
                + ", enlazada=" + e.desencolar());
        System.out.println("Sale vector=" + v.desencolar()
                + ", enlazada=" + e.desencolar());
        v.encolar(40); e.encolar(40);
        System.out.println("Vector: front=" + v.front + ", rear=" + v.rear);
        System.out.println("Enlazada: front=" + e.front.dato
                + ", rear=" + e.rear.dato);
    }
}
```

### Antes de ejecutar: predice

**Predicción A.** ¿Qué será visible tras la primera inserción? **Predicción B.** ¿Cuál será el primer valor retirado o por qué se rechazará la operación crítica? **Predicción C.** ¿Qué indicador o enlace cambiará y cuál permanecerá igual? Anota las respuestas antes de mirar la salida.

### Compila y ejecuta en la carpeta del archivo

```bash
javac -encoding UTF-8 EJ08_CompararImplementaciones.java
java EJ08_CompararImplementaciones
```

### Resultado esperado (comparar literalmente)

```text
Sale vector=10, enlazada=10
Sale vector=20, enlazada=20
Vector: Overflow
Vector: front=2, rear=2
Enlazada: front=30, rear=40
```

### Cómo interpretar la salida

Integrador: comprender FIFO, overflow, trade-off de corrimiento y referencias enlazadas. Ambas [10,20,30]. Tras dos salidas: vector front=2,rear=2; enlazada nodo único 30. Inserción 40: vector overflow por rear=2; enlazada 30→40. Ambas respetaron FIFO. La consola informa el comportamiento, pero la traza explica por qué sucede.

### Variación A — modificar sin perder el contrato

Cambia capacidad del vector a 4: después de dos salidas la inserción 40 funcionará una vez; explica por qué el problema reaparecería al seguir avanzando rear.

### Error o caso límite controlado

Afirmar que una cola de vector siempre cuesta O(1) por operación omite el caso de desplazamiento de EJ04.

**Corrección razonada.** Distinguir O(1) para encolar/desencolar con índices fijos y para enlaces, de O(n) para la extracción que desplaza elementos.

### Explícalo con tus palabras

¿El método encolar decide el orden FIFO o lo determina también la operación de salida?

### T08 — tarea espejo de EJ08

**Enunciado.** Con vector de capacidad 3 y lista enlazada, introducir 1/2/3, retirar dos e insertar 4; comparar las salidas, los extremos y el overflow.

**Archivo que crearás:** `T08_CompararImplementaciones.java`. Para resolverlo sin apoyarte en otros materiales, reutiliza **exactamente la estructura de clases, campos, constructor y métodos** mostrada arriba y cambia el nombre de la clase pública a `T08_CompararImplementaciones`. En `main`, escribe la secuencia solicitada en el enunciado. En T07 repara además el bloque que limpia `rear` al vaciarse. **No cambies la lógica FIFO** ni incluyas una solución circular.

**Pista.** Mantén el mismo orden causal: primero comprueba si hay datos o espacio, luego altera el extremo correspondiente, finalmente presenta la evidencia. **Evidencia:** copia la salida, acompáñala de un dibujo de posiciones o nodos y explica el estado final. **Criterio de validación:** el primer retirado debe ser el más antiguo; las condiciones de overflow/underflow y los extremos deben concordar con tu traza.

**Salida para contrastar la tarea después de resolverla:**

```text
Sale vector=1, enlazada=1
Sale vector=2, enlazada=2
Vector: Overflow
Vector: front=2, rear=2
Enlazada: front=3, rear=4
```

---

## Cierre de la sesión y transferencia a aplicaciones reales

Hemos empezado construyendo una cola con posiciones numeradas y hemos aprendido que reservar memoria no equivale a incorporar elementos. `front` y `rear` permitieron identificar el primero y el último dato de la secuencia sin alterar el principio FIFO: 10, 20 y 30 salen precisamente en ese orden. El estado después de cada operación fue más importante que memorizar la sintaxis de un método. Cuando el arreglo lineal llegó al final, distinguimos el overflow físico de una cola realmente sin huecos. Con el desplazamiento recuperamos posiciones al costo de copiar elementos, mientras que una cola enlazada resolvió entradas y salidas modificando referencias a nodos.

La práctica permitió reconocer el underflow antes de acceder a datos inexistentes y un error de referencia especialmente engañoso: dejar `rear` apuntando a un nodo retirado después de vaciar la cola. Aprendimos a observar el estado, no solamente un mensaje de “Encolado”. Ahora podemos reconocer el uso de FIFO en colas de impresión, tareas listas para la CPU y buffers de datos en streaming, con la precaución de que un sistema real puede incorporar políticas adicionales que no estudiamos en esta sesión.

Para consolidar, repasa las trazas de índices y nodos, completa las tareas espejo y verifica tus explicaciones antes de utilizar una sugerencia de IA. Una respuesta de IA puede encontrar errores, pero sus propuestas deben compararse con la definición FIFO, el costo de cada operación y la salida real. En la siguiente experiencia sobre estructuras de datos podrás reutilizar la habilidad de seguir el estado y detectar fallos de enlaces.

Recursos de formación: **Lideratec Academy**, blog https://lideratecacademy.com/blog/ y canal https://www.youtube.com/@LideratecAcademy.
