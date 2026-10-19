# FASE 13 - GUÍA DEL ESTUDIANTE · VERSIÓN AMPLIADA V2
## Sesión 08 · Repaso focalizado: listas enlazadas simples y dobles en Java

**Institución:** ISIL  
**Curso:** Algoritmo y Estructura de Datos Basados en Inteligencia Artificial (30710)  
**Modalidad sugerida:** laboratorio presencial guiado  
**Duración:** **120 minutos exactos**, incluyendo una evaluación Kahoot de 16 minutos  
**Marca académica complementaria:** Lideratec Academy

> **Alcance deliberado.** Este repaso recupera **solo dos temas**: (1) listas enlazadas simples, trabajadas en las sesiones 05 y 06; y (2) listas doblemente enlazadas, correspondientes a la sesión 07. No se repetirán vectores, Bubble Sort, matrices ni listas circulares. Los programas y ejercicios son **propuestas nuevas de repaso**, no una afirmación de que se usaron exactamente así en las clases anteriores.

## 1. Propósito: administrar solicitudes sin perder enlaces

Imagina un sistema de atención que recibe solicitudes numeradas. Debe agregarlas, recorrerlas, localizar una solicitud y retirarla de la secuencia. Si usamos una **lista simple**, cada nodo conoce solamente al siguiente; para retirar un nodo intermedio, normalmente debemos localizar también al anterior. Con una **lista doble**, cada nodo conserva las referencias `anterior` y `siguiente`; así podremos recorrer la secuencia en ambos sentidos y reconectar sus vecinos cuando se elimine una solicitud.

Trabajaremos un mismo caso en dos programas Java. **Primero** construiremos `ListaSimpleRepasoS08.java` para observar cómo opera un único enlace; **después** desarrollaremos `ListaDobleRepasoS08.java` para justificar cuándo y cómo se usan dos enlaces. No basta con que el programa imprima números: al terminar debes poder dibujar qué referencias cambiaron y demostrar que la lista continúa conectada.

## 2. Resultado observable: lectura sugerida del docente

«Hoy dedicaremos nuestras dos horas a recuperar dos ideas decisivas de las últimas sesiones: cómo se comporta una lista enlazada simple y qué cambia cuando la convertimos en doble. No intentaremos repasar todos los temas vistos desde el inicio del curso porque eso impediría practicar con la profundidad necesaria. Usaremos un solo caso, un sistema que ordena solicitudes en una secuencia de atención, y crearemos dos programas pequeños en Java. Primero aprenderemos a reconocer por qué necesitamos una clase para los nodos, qué guarda cada atributo y cómo se mantiene la referencia al inicio de la lista. Después construiremos métodos que añaden, recorren, buscan y eliminan elementos. La parte importante será explicar cada cambio de referencia, no memorizar líneas de código.

Cuando pasemos a la lista doble veremos por qué hace falta un segundo enlace, cómo el primer y el último nodo delimitan la estructura y qué debemos actualizar al retirar un elemento. Antes de ejecutar, haremos predicciones; después compararemos las salidas con los diagramas y provocaremos errores controlados para entender sus consecuencias. Finalizaremos con un Kahoot centrado en los dos temas practicados. Si al cierre puedes justificar por qué una eliminación conserva las conexiones hacia adelante y hacia atrás, habrás demostrado comprensión, no solamente ejecución».

**Desempeños observables:** crear un nodo, insertar al final, recorrer, buscar, eliminar en una lista simple, construir una lista doble, recorrerla en ambos sentidos y validar la integridad de sus conexiones. **Criterio de dominio:** explicar al menos una línea crítica por método, anticipar el nuevo recorrido al eliminar un nodo y justificar los cambios de enlaces sin consultar la solución.

## 3. Herramientas, archivos y preparación

**Debes saber previamente:** declarar `int`, escribir `if` y `while`, reconocer una clase y ejecutar `main`. **Necesitas:** un **JDK de Java** (recomendado JDK 21 LTS o una versión posterior compatible), una terminal y cualquier editor de código. **No necesitas:** Maven, Gradle, bibliotecas externas, servidores, bases de datos o Git.

**¿Qué es el JDK y por qué lo utilizamos?** Es el kit que incluye el compilador `javac`, para convertir nuestros archivos `.java` a bytecode, y la herramienta `java`, para ejecutar las clases compiladas. Puedes obtener un JDK desde los sitios de los distribuidores: <https://adoptium.net/> (Eclipse Temurin) o <https://www.oracle.com/java/technologies/downloads/> (Oracle). Selecciona el **JDK**, tu sistema operativo y arquitectura; comprueba el formato de instalador antes de descargar. Puedes usar el editor de clase o, de manera opcional, <https://code.visualstudio.com/>; no se requieren extensiones para compilar desde terminal.

**Ruta A - ya tienes Java.** Abre la terminal y ejecuta `java -version` y `javac -version`; ambos comandos deben mostrar una versión. Descomprime el kit. Entra a `FASE_15/LAB-S08-EJ01/estudiante/` y ejecuta `javac ListaSimpleRepasoS08.java` seguido de `java ListaSimpleRepasoS08`. Después entra a `FASE_15/LAB-S08-EJ04/estudiante/` y ejecuta `javac ListaDobleRepasoS08.java` seguido de `java ListaDobleRepasoS08`. **Cada carpeta es independiente**: no compiles simultáneamente proyectos de distintos laboratorios que reutilizan el mismo nombre de clase pública.

**Ruta B - equipo sin preparar.** Identifica tu sistema operativo y arquitectura, abre una de las páginas oficiales anteriores, selecciona la edición JDK apropiada, instala siguiendo las instrucciones del proveedor y habilita la integración con `PATH` cuando esté disponible. Cierra y reabre la terminal; valida `java -version` y `javac -version`. Después prepara la carpeta `S08_2_Temas` y repite la compilación de la Ruta A. Si `javac` no se reconoce, revisa si instalaste solo un entorno de ejecución o si el JDK no está incorporado al `PATH`.

**Archivos de laboratorio FASE 15 ya disponibles:** para cada `EJ01–EJ06` abre `FASE_15/LAB-S08-EJxx/estudiante/`; después realiza la práctica en `tarea_estudiante/`. Cada laboratorio incluye `docente/`, `tarea_docente/` (uso del profesor) e `INSTRUCCIONES.md`. Las clases y métodos son los mismos en FASE 13 y FASE 15. No compiles simultáneamente archivos de carpetas distintas que tengan la misma clase pública.

**Prueba inicial que debe funcionar:** ejecuta `java ListaSimpleRepasoS08` y busca `Inicial: 10 -> 20 -> 30 -> 40`; ejecuta `java ListaDobleRepasoS08` y busca `Enlaces validos: true`. Si aparece `Could not find or load main class`, verifica el nombre de clase y tu carpeta actual; si aparece que una clase pública debe declararse en un archivo distinto, iguala el nombre del archivo al de la clase.

## 4. Cronograma de trabajo: exactamente 120 minutos

| Minutos | Trabajo de clase | Evidencia |
|---|---|---|
| 00–10 | Diagnóstico y dibujo inicial de tres nodos | Diagrama y predicción |
| 10–22 | **EJ01 + T01**: nodo e inserción simple | Enlaces creados |
| 22–36 | **EJ02 + T02**: recorrido y búsqueda simple | Salida y búsqueda fallida |
| 36–54 | **EJ03 + T03**: eliminación simple | Referencia del anterior actualizada |
| 54–68 | **EJ04 + T04**: nodo doble e inserción | Referencias `anterior/siguiente` |
| 68–82 | **EJ05 + T05**: recorridos y validación doble | Recorridos inversos |
| 82–94 | **EJ06 + T06**: eliminación doble | Enlaces coherentes tras retirar nodo |
| 94–110 | Kahoot - 16 minutos, **preguntas solo de estos dos temas** | Diagnóstico formativo |
| 110–120 | Retroalimentación y cierre | Justificación oral |
| **Total** | | **120 minutos** |

> **Dinámica realista:** en clase el docente utiliza los dos programas ya preparados y reconstruye con el grupo los microbloques más importantes; se predice, ejecuta y resuelve cada tarea espejo. La guía conserva la explicación extensa para estudiar fuera de clase. No es necesario teclear desde cero los dos archivos completos durante los 120 minutos.


### Activación inicial (minutos 00–10)

Antes de abrir los programas, dibuja en tu cuaderno `10 -> 20 -> 30 -> null` y contesta: **(1)** ¿qué conserva el atributo `siguiente`?, **(2)** si queremos retirar 20, ¿qué nodo debe cambiar su referencia?, **(3)** si el sistema necesita volver de 30 a 20, ¿qué dato adicional requiere cada nodo? Comparte una predicción con el grupo. No se evalúa rapidez; se utiliza este diagnóstico para identificar qué debemos explicar con mayor detalle en los siguientes ejemplos.

---

# TEMA 1 · LISTAS ENLAZADAS SIMPLES

## EJ01 ↔ T01 · ¿Por qué necesitamos un nodo y cómo agregamos al final? (12 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ01/estudiante/ListaSimpleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ01/tarea_estudiante/ListaSimpleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** El sistema debe conservar las solicitudes 10, 20, 30 y 40 en ese orden, sin depender de posiciones fijas de un arreglo.

**Archivo:** `FASE_15/LAB-S08-EJ01/estudiante/ListaSimpleRepasoS08.java` · **Componentes nuevos:** `Nodo`, `ListaSimple`, `primero`, `agregarFinal`.

### Paso 1 - Crear la clase que representa un nodo

```java
static class Nodo {
    int id;
    Nodo siguiente;

    Nodo(int id) {
        this.id = id;
        this.siguiente = null;
    }
}
```

**¿Por qué creamos `Nodo`?** Una lista enlazada no mantiene sus elementos unidos mediante índices consecutivos; necesita **objetos individuales** que almacenen el dato y una referencia al siguiente objeto. `Nodo` modela exactamente esa unidad.

**Explicación de cada línea o grupo sintáctico:**

1. `static class Nodo {` define una clase anidada dentro de `ListaSimpleRepasoS08`. La palabra `static` permite crear nodos desde la clase anidada `ListaSimple` sin construir antes una instancia externa.
2. `int id;` guarda el identificador numérico de la solicitud. Es el **dato**, no un índice.
3. `Nodo siguiente;` guarda una referencia a otro objeto `Nodo`. Su tipo es `Nodo` porque el siguiente elemento tiene la misma estructura. Es el **enlace**.
4. `Nodo(int id) {` es el constructor: recibe el identificador cada vez que escribimos `new Nodo(valor)`.
5. `this.id = id;` copia el parámetro `id` al atributo del objeto recién creado. `this.id` distingue el campo del parámetro con el mismo nombre.
6. `this.siguiente = null;` expresa que el nuevo nodo todavía no tiene sucesor. `null` no es cero: significa **ausencia de referencia**.
7. Las llaves `}` cierran primero el constructor y después la clase. No son instrucciones de movimiento: delimitan el ámbito del código.

**Predicción:** inmediatamente después de `new Nodo(10)`, ¿a dónde apunta `siguiente`? A ningún otro nodo (`null`).

### Paso 2 - Crear la lista que conoce el inicio

```java
static class ListaSimple {
    Nodo primero;
}
```

**¿Por qué otra clase?** `Nodo` describe una solicitud individual; `ListaSimple` administra **toda la secuencia**. `Nodo primero;` conserva una referencia al nodo inicial. Si está en `null`, la lista está vacía. Separar responsabilidades evita mezclar la información de cada solicitud con las operaciones del conjunto.

### Paso 3 - Escribir `agregarFinal`: gestionar lista vacía y lista con datos

Dentro de `ListaSimple`, incorpora:

```java
void agregarFinal(int id) {
    Nodo nuevo = new Nodo(id);
    if (primero == null) {
        primero = nuevo;
        return;
    }
    Nodo actual = primero;
    while (actual.siguiente != null) {
        actual = actual.siguiente;
    }
    actual.siguiente = nuevo;
}
```

**Lectura línea por línea, con propósito:**

1. `void agregarFinal(int id)` declara una operación sobre la lista. Recibe el valor del nuevo elemento y no devuelve un valor (`void`): su efecto se observa en los enlaces.
2. `Nodo nuevo = new Nodo(id);` construye un objeto independiente; sin él no existiría un elemento que insertar.
3. `if (primero == null)` comprueba si la lista no tiene nodos. Debemos resolver este caso **antes de intentar recorrer**.
4. `primero = nuevo;` hace que el nuevo elemento sea la cabeza de la lista vacía.
5. `return;` termina el método porque la inserción ya está completa; evita entrar en el recorrido de una lista que antes estaba vacía.
6. `Nodo actual = primero;` crea un cursor de recorrido. No modifica `primero`: conserva la referencia al comienzo.
7. `while (actual.siguiente != null)` pregunta si todavía existe otro nodo detrás del cursor.
8. `actual = actual.siguiente;` desplaza el cursor hacia el siguiente nodo. La asignación cambia **la variable cursor**, no los enlaces existentes.
9. `actual.siguiente = nuevo;` cambia finalmente **el enlace del último nodo**, que pasa de `null` a apuntar al nuevo objeto.
10. Las llaves cierran los bloques `if`, `while` y el método; ayudan a distinguir decisiones de acciones repetidas.

**Traza visual del caso:**

```text
Antes: primero -> [10 | null]
Agregar 20:      [10 | siguiente] -> [20 | null]
Agregar 30:      [10] -> [20] -> [30 | null]
Agregar 40:      [10] -> [20] -> [30] -> [40 | null]
```

### Paso 4 - Crear y utilizar la lista desde `main`

```java
ListaSimple lista = new ListaSimple();
lista.agregarFinal(10);
lista.agregarFinal(20);
lista.agregarFinal(30);
lista.agregarFinal(40);
System.out.println(lista.primero.id);
```

- `new ListaSimple()` crea la estructura administradora; el campo `primero` empieza en `null` por ser una referencia de instancia.
- Cada llamada `agregarFinal(n)` añade **un nodo diferente** y conserva los anteriores.
- `lista.primero.id` consulta el identificador de la cabeza; esperamos **10**. Todavía no mostramos toda la secuencia porque el recorrido se implementará en EJ02.

**Variante de razonamiento:** ¿qué ocurre si se agrega `50`? El enlace de 40 deja de ser `null` y pasa a apuntar a 50. **Error controlado:** omitir `actual.siguiente = nuevo` deja el objeto creado sin integrarlo a la lista. **Corrección:** enlazarlo después de localizar el último nodo.


#### Microconstrucción comentada: ¿por qué nace cada elemento de EJ01?

**Decisión 1 — ¿por qué dos clases y no un arreglo `int[]`?** Una solicitud necesita su **dato** y un **enlace a la siguiente solicitud**. La clase `Nodo` es el molde de UNA solicitud; la clase `ListaSimple` es la administradora de TODAS. No es una duplicación: sus responsabilidades son distintas. El identificador `id` no dice qué solicitud sigue; la referencia `siguiente` sí. La clase `ListaSimple` protege el punto de acceso `primero`: sin ella, cada operación dependería de que el código externo retuviera manualmente la cabeza.

**Decisión 2 — ¿por qué primero construimos el nodo y solo después lo enlazamos?** `new Nodo(id)` reserva el objeto y deja su `siguiente` en `null`; todavía no forma parte de la cadena. La instrucción `actual.siguiente = nuevo` es la que lo convierte en nodo alcanzable. Crear un objeto **no equivale** a insertarlo. La diferencia debe poder explicarse en un dibujo: existe un objeto en memoria, pero ninguna ruta desde `primero` conduce a él hasta modificar la referencia.

**Decisión 3 — ¿por qué hay dos caminos en `agregarFinal`?** Cuando `primero == null`, el nuevo objeto se convierte en cabeza: `primero = nuevo`. Cuando ya hay nodos, **no** se reemplaza `primero`: se localiza el último con un cursor y se modifica exclusivamente su `siguiente`. El `return` del caso vacío impide continuar hacia un recorrido innecesario. Por eso la pregunta previa al `while` es «¿existe cabeza?», no «¿qué dato contiene?». 

**Decisión 4 — ¿por qué crear el cursor `actual`?** `Nodo actual = primero` copia una *referencia* a la cabeza; no crea otro nodo y tampoco modifica la cabeza. Cada `actual = actual.siguiente` mueve el cursor: el enlace del objeto permanece intacto. En cambio, `actual.siguiente = nuevo` sí cambia un campo dentro de un objeto. Las dos asignaciones tienen destinatarios y efectos distintos.

| Línea o expresión exacta | ¿Por qué la escribimos ahora? | Efecto observable / qué pasaría si faltara |
|---|---|---|
| `int id;` | Sin dato no podríamos distinguir solicitudes. | Cada nodo conserva su propio número. |
| `Nodo siguiente;` | Necesitamos una ruta hacia el sucesor. | Sin enlace no existiría una cadena navegable. |
| `Nodo(int id)` | Cada solicitud se construye con un identificador concreto. | `new Nodo(10)` produce un nodo con `id = 10`. |
| `this.id = id;` | Guardar en el objeto el argumento que llega al constructor. | Si se omite, `id` quedaría con el valor entero inicial 0. |
| `this.siguiente = null;` | El objeto aún no tiene vecino al crearse. | El estado inicial queda explícito y permite reconocer el final. |
| `Nodo primero;` | La lista necesita recordar por dónde comienza. | Si no retenemos la cabeza, no tenemos desde dónde recorrer. |
| `Nodo nuevo = new Nodo(id);` | Fabricar el nodo nuevo antes de conectarlo. | El nodo existe, pero aún no es alcanzable desde la lista. |
| `if (primero == null)` | Separar la primera inserción de las siguientes. | Evita intentar leer `actual.siguiente` cuando aún no hay nodos. |
| `primero = nuevo; return;` | Resolver por completo la inserción de una lista vacía. | Queda un solo nodo: `primero -> [id | null]`. |
| `Nodo actual = primero;` | Tener cursor sin perder la cabeza. | `primero` se mantiene estable. |
| `while (actual.siguiente != null)` | Detener el cursor exactamente en el último nodo. | El último tiene `siguiente == null`. |
| `actual = actual.siguiente;` | Avanzar el cursor, no modificar nodos. | `actual` termina apuntando al último. |
| `actual.siguiente = nuevo;` | Convertir el nodo recién creado en parte de la cadena. | El anterior último ahora conduce al nuevo. |

**Prueba mental completa:**

```text
Llamar agregarFinal(10): primero = null   -> primero apunta al Nodo 10.
Llamar agregarFinal(20): actual = Nodo 10 -> 10.siguiente apunta al Nodo 20.
Llamar agregarFinal(30): actual pasa 10→20 -> 20.siguiente apunta a 30.
Llamar agregarFinal(40): actual pasa 10→20→30 -> 30.siguiente apunta a 40.
ESTADO FINAL: primero → 10 → 20 → 30 → 40 → null
```

**Pregunta antes de ejecutar:** ¿qué referencia cambia al insertar el 40? *Solo `30.siguiente`*. El cursor puede haber visitado tres nodos, pero eso no cambia sus enlaces. **Verificación:** `primero.id` conserva el valor 10 y `primero.siguiente.id` devuelve 20. Si sustituyes temporalmente la última línea por `actual = nuevo`, la lista NO crece: has movido una variable local, no el campo `siguiente` del nodo final.

### T01 - Tarea espejo (sin solución)

Crea una lista simple que reciba `5`, `15` y `25`. Imprime el identificador del primero y del segundo nodo accediendo a sus referencias, **sin implementar todavía un recorrido automático**. Dibuja las tres cajas y sus enlaces. **Pista:** el segundo nodo está en `lista.primero.siguiente`. **Criterio:** el primer nodo conserva su referencia y el tercero termina en `null`.

---

## EJ02 ↔ T02 · ¿Cómo recorremos y buscamos sin perder la cabeza? (14 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ02/estudiante/ListaSimpleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ02/tarea_estudiante/ListaSimpleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** Necesitamos mostrar todas las solicitudes y saber si existe la número 30.

**Punto de partida:** conserva las clases `Nodo` y `ListaSimple`, su campo `primero` y el método `agregarFinal` de EJ01. **En el mismo archivo**, incorpora los siguientes métodos dentro de `ListaSimple`.

### Paso 1 - Crear `recorrer`: leer sin alterar la estructura

```java
String recorrer() {
    if (primero == null) {
        return "(vacia)";
    }
    StringBuilder texto = new StringBuilder();
    Nodo actual = primero;
    while (actual != null) {
        if (texto.length() > 0) {
            texto.append(" -> ");
        }
        texto.append(actual.id);
        actual = actual.siguiente;
    }
    return texto.toString();
}
```

**¿Por qué creamos este método?** La clase ya sabe **agregar** nodos, pero aún no puede **mostrar** todos. `recorrer()` devuelve una representación legible de los elementos y será reutilizado para comprobar inserciones y eliminaciones.

**Explicación de las instrucciones nuevas:**

1. `String recorrer()` anuncia que el método devuelve texto; a diferencia de `agregarFinal`, ahora necesitamos una salida consultable.
2. `if (primero == null)` distingue el caso de lista vacía y evita usar una referencia inexistente.
3. `return "(vacia)";` representa ese estado de forma explícita.
4. `StringBuilder texto = new StringBuilder();` crea un acumulador de caracteres; es útil porque añadiremos varios identificadores.
5. `Nodo actual = primero;` crea un cursor local. Si cambiáramos `primero` al recorrer, perderíamos el inicio original.
6. `while (actual != null)` continúa **mientras exista un nodo**. En una lista simple lineal, el último tiene `siguiente = null`.
7. `if (texto.length() > 0)` comprueba si ya se añadió algún número; evita poner una flecha antes del primero.
8. `texto.append(" -> ")` añade el separador entre nodos, pero **no crea enlaces reales**: es solo texto.
9. `texto.append(actual.id)` incorpora el identificador del nodo que visita el cursor.
10. `actual = actual.siguiente` mueve el cursor al nodo posterior sin alterar las referencias de los objetos.
11. `return texto.toString()` convierte el acumulador en el `String` que entregará el método.

**Predicción:** con 10, 20, 30 y 40 debe devolver `10 -> 20 -> 30 -> 40`.

### Paso 2 - Crear `buscar`: decidir si un identificador existe

```java
boolean buscar(int id) {
    Nodo actual = primero;
    while (actual != null) {
        if (actual.id == id) {
            return true;
        }
        actual = actual.siguiente;
    }
    return false;
}
```

**¿Por qué lo necesitamos?** Mostrar una lista y responder una pregunta de **pertenencia** son responsabilidades diferentes. `buscar` recibe el dato deseado y devuelve un booleano (`true`/`false`).

1. `boolean buscar(int id)` declara el resultado lógico esperado.
2. `Nodo actual = primero` inicia la inspección desde la cabeza.
3. `while (actual != null)` revisa nodos existentes; también funciona si la lista está vacía porque el ciclo no entra.
4. `if (actual.id == id)` compara el dato almacenado con el solicitado (`==` para enteros).
5. `return true` detiene la búsqueda apenas encuentra coincidencia.
6. `actual = actual.siguiente` avanza si el nodo actual no coincide.
7. `return false` se ejecuta cuando el cursor llega a `null` sin encontrar el dato.

**Uso desde `main`:**

```java
System.out.println("Inicial: " + lista.recorrer());
System.out.println("Buscar 30: " + lista.buscar(30));
System.out.println("Buscar 99: " + lista.buscar(99));
```

**Salida que debes explicar:**

```text
Inicial: 10 -> 20 -> 30 -> 40
Buscar 30: true
Buscar 99: false
```

**Variante:** si buscas 10, el algoritmo finaliza en la primera iteración; si buscas 99, recorre toda la lista. **Error controlado:** cambiar `while (actual != null)` por `while (actual.siguiente != null)` podría omitir la revisión del último nodo y fallaría incluso al intentar evaluar `actual.siguiente` en una lista vacía. **Corrección:** comprobar la existencia de `actual` antes de leer sus campos.


#### Microconstrucción comentada: leer es distinto de buscar

**¿Por qué crear `recorrer()` si ya tenemos `agregarFinal()`?** Insertar cambia la estructura; recorrer solo necesita **observarla**. Sin un método de lectura no podemos comprobar fácilmente qué quedó almacenado. `recorrer()` devuelve texto (`String`) porque queremos utilizar el resultado en `println` o en futuras verificaciones. `StringBuilder` se crea como acumulador de la salida: no contiene nodos, ni es una segunda lista.

**¿Por qué crear `buscar(int id)` por separado?** Mostrar todos los nodos y responder «¿existe el identificador X?» tienen contratos distintos. El primero devuelve texto legible; el segundo responde `true` o `false`. Buscar necesita comparar el campo `id`, pero no debe insertar, borrar ni actualizar ningún `siguiente`.

| Instrucción nueva | Motivo de creación | ¿Qué cambia? / Cómo se comprueba |
|---|---|---|
| `String recorrer()` | Necesitamos una salida reutilizable que describa el estado. | El método devolverá, por ejemplo, `10 -> 20 -> 30 -> 40`. |
| `if (primero == null)` | No hay datos si la cabeza está vacía. | Devuelve `"(vacia)"` sin acceder a un nodo inexistente. |
| `StringBuilder texto = new StringBuilder();` | Acumular fragmentos sin concatenar constantemente cadenas completas. | Solo cambia `texto`, no cambia la lista. |
| `Nodo actual = primero;` | Iniciar la lectura en la cabeza sin moverla. | Al terminar, `primero` sigue apuntando a 10. |
| `while (actual != null)` | Visitar cada nodo **incluido el último**. | El ciclo termina después de 40, cuando `actual` pasa a `null`. |
| `if (texto.length() > 0)` | Evitar un separador antes del primer elemento. | La salida empieza en `10`, no en ` -> 10`. |
| `texto.append(" -> ")` | Crear una representación gráfica textual. | No equivale a escribir `actual.siguiente = ...`. |
| `texto.append(actual.id)` | Incorporar el dato observado en esta iteración. | El texto se amplía sin mutar ningún nodo. |
| `actual = actual.siguiente;` | Leer el siguiente nodo de la cadena. | Cambia una variable local; no un enlace persistente. |
| `return texto.toString();` | Entregar un `String` al código que llamó. | `println` puede mostrar la secuencia completa. |
| `boolean buscar(int id)` | Responder sí/no sobre la existencia de un dato. | La respuesta será `true` o `false`. |
| `if (actual.id == id)` | Comparar el dato almacenado y la clave solicitada. | Si coincide, `return true` interrumpe el recorrido. |
| `return false;` | Informar que se llegó al final sin encontrar coincidencia. | Buscar 99 devuelve false sin quitar nodos. |

**Traza guiada con lista 10→20→30→40 y `buscar(30)`:**

| Iteración | `actual.id` antes de comparar | `actual.id == 30` | ¿Qué sucede? |
|---|---:|---|---|
| 1 | 10 | `false` | `actual` pasa a 20. |
| 2 | 20 | `false` | `actual` pasa a 30. |
| 3 | 30 | `true` | Se devuelve `true`, sin recorrer 40. |

**Caso límite necesario:** `buscar(40)` debe encontrar el último. Si alguien reemplaza `while (actual != null)` por `while (actual.siguiente != null)`, 40 queda excluido de la comparación y el código puede fallar con lista vacía. **Razón:** la condición correcta pregunta por la existencia del nodo actual; la otra pregunta por la existencia de SU sucesor y corta antes de tiempo.

**Comprobación dual:** después de buscar 30 y 99, vuelve a ejecutar `recorrer()`: debe seguir devolviendo la lista inicial. Eso demuestra que buscar ha leído la estructura sin modificarla.

### T02 - Tarea espejo (sin solución)

En una lista con `4`, `8`, `12` y `16`, muestra el recorrido e informa si existen `12` y `99`. Anota el identificador de cada nodo inspeccionado al buscar 99. **Restricción:** no cambies `primero` para recorrer. **Criterio:** las búsquedas deben devolver un valor lógico y la lista debe seguir conteniendo sus cuatro nodos.

---

## EJ03 ↔ T03 · ¿Cómo eliminamos sin desconectar el resto? (18 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ03/estudiante/ListaSimpleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ03/tarea_estudiante/ListaSimpleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** Retiraremos la solicitud 20 de la secuencia `10 -> 20 -> 30 -> 40`. Después comprobaremos los casos de eliminar el primer y último elemento.

**¿Por qué crear `eliminar`?** Hasta ahora solo leímos datos y creamos enlaces. Para retirar un elemento de una lista simple debemos conservar una referencia al **anterior**; así podremos hacer que apunte directamente al **siguiente** del elemento eliminado.

### Paso 1 - Resolver primero el caso vacío y el primer nodo

```java
boolean eliminar(int id) {
    if (primero == null) {
        return false;
    }
    if (primero.id == id) {
        primero = primero.siguiente;
        return true;
    }
```

1. `boolean eliminar(int id)` recibe el dato que queremos retirar y devuelve `true` si se realizó una eliminación.
2. `if (primero == null)` evita buscar en una lista vacía.
3. `return false` indica que no existía ningún elemento para retirar.
4. `if (primero.id == id)` detecta el caso especial de **eliminar la cabeza**.
5. `primero = primero.siguiente` hace que el segundo nodo pase a ser la nueva cabeza. No es necesario buscar un anterior porque la cabeza no lo tiene.
6. `return true` confirma el éxito y evita que continuemos con el algoritmo general.

### Paso 2 - Localizar simultáneamente el anterior y el candidato

Añade **dentro del mismo método**, a continuación del bloque anterior:

```java
    Nodo anterior = primero;
    Nodo actual = primero.siguiente;
    while (actual != null && actual.id != id) {
        anterior = actual;
        actual = actual.siguiente;
    }
```

1. `Nodo anterior = primero` conserva el nodo previo al candidato inicial.
2. `Nodo actual = primero.siguiente` arranca desde el segundo nodo porque el primero ya fue revisado.
3. `while (actual != null && actual.id != id)` avanza mientras haya un nodo **y** no coincida con lo solicitado. `&&` combina ambas condiciones de forma segura: si `actual` es `null`, Java no evalúa `actual.id`.
4. `anterior = actual` desplaza el nodo previo hasta la posición actual antes de continuar.
5. `actual = actual.siguiente` mueve el cursor hacia el siguiente candidato.

**Traza al buscar 20:** `anterior` permanece en 10 y `actual` apunta a 20. Al buscar 30: primero `anterior=10, actual=20`; luego `anterior=20, actual=30`.

### Paso 3 - Reconectar los nodos y devolver el resultado

Finaliza el método con:

```java
    if (actual == null) {
        return false;
    }
    anterior.siguiente = actual.siguiente;
    return true;
}
```

1. `if (actual == null)` comprueba que el recorrido terminó sin encontrar la solicitud.
2. `return false` comunica que no hubo modificación.
3. `anterior.siguiente = actual.siguiente` es **la línea decisiva**: cambia el enlace del nodo anterior para que salte directamente al siguiente del nodo retirado.
4. `return true` confirma que la lista sí fue actualizada.

**Antes y después:**

```text
ANTES: primero -> 10 -> 20 -> 30 -> 40 -> null
                       anterior  actual
CAMBIO:   10.siguiente = 20.siguiente
DESPUÉS: primero -> 10 ------> 30 -> 40 -> null
```

**Comprueba desde `main`:**

```java
System.out.println("Eliminar 20: " + lista.eliminar(20));
System.out.println("Luego de eliminar 20: " + lista.recorrer());
System.out.println("Eliminar 10: " + lista.eliminar(10));
System.out.println("Eliminar 40: " + lista.eliminar(40));
System.out.println("Final: " + lista.recorrer());
```

**Salida:** `Eliminar 20: true`, `Luego de eliminar 20: 10 -> 30 -> 40`, luego `Final: 30`. **Variante:** eliminar un identificador inexistente debe devolver `false` y preservar toda la secuencia. **Error controlado:** escribir `anterior.siguiente = null` al eliminar un nodo intermedio rompe el acceso a los nodos que venían después. **Corrección:** asignar `actual.siguiente`, no `null`.


#### Microconstrucción comentada: eliminar exige identificar a quién se reconecta

**¿Por qué no basta `actual = actual.siguiente` para eliminar?** Porque esa asignación modifica únicamente **el cursor**, no el enlace almacenado en el nodo anterior. En una lista simple debemos localizar dos referencias: `anterior` (vecino que permanecerá) y `actual` (candidato a retirar). El cambio efectivo ocurre al escribir `anterior.siguiente = actual.siguiente`.

**¿Por qué distinguir la cabeza de los demás nodos?** La cabeza no tiene un nodo anterior desde el cual reconectar. Si su `id` coincide, la solución es mover `primero` al segundo. Solo después se aplica el recorrido de dos cursores para los nodos internos o el último.

| Línea clave | ¿Para qué existe? | Efecto, condición o protección |
|---|---|---|
| `boolean eliminar(int id)` | Informar si realmente se retiró el dato solicitado. | `true` cuando se modificó, `false` si no existía. |
| `if (primero == null)` | No hay nodo donde leer `id`. | Retorno seguro `false` sobre lista vacía. |
| `if (primero.id == id)` | Detectar el único elemento sin anterior. | Se trata la cabeza antes del caso general. |
| `primero = primero.siguiente;` | Convertir el segundo nodo en nueva cabeza. | Deja de ser alcanzable el antiguo primero desde la lista. |
| `Nodo anterior = primero;` | Conservar el nodo cuya referencia quizá habrá que cambiar. | El puntero al anterior empieza en 10. |
| `Nodo actual = primero.siguiente;` | Empezar en el segundo, porque el primero ya se verificó. | El candidato inicial es 20. |
| `while (actual != null && actual.id != id)` | Continuar solo si hay candidato y aún no coincide. | `&&` no evalúa `actual.id` si `actual` es `null`. |
| `anterior = actual;` | Preparar el anterior para la siguiente comparación. | El par de referencias se mantiene consecutivo. |
| `actual = actual.siguiente;` | Mover el candidato un nodo adelante. | Todavía NO se elimina nada. |
| `if (actual == null)` | Distinguir dato no encontrado. | Devuelve `false` sin reescribir enlaces. |
| `anterior.siguiente = actual.siguiente;` | Saltar el candidato y reconectar la cadena. | El sucesor de `anterior` será el que seguía a `actual`. |
| `return true;` | Reportar éxito al llamador. | La llamada `eliminar(20)` imprime `true`. |

**Traza concreta para retirar 30** (permite ver el propósito de mover ambos cursores):

```text
Lista inicial: 10 -> 20 -> 30 -> 40
Antes del while: anterior=10, actual=20.
No coincide 20: anterior=20, actual=30.
Coincide 30: salir del while.
Cambio real: 20.siguiente = 30.siguiente (referencia a 40).
Lista final: 10 -> 20 -> 40.
```

**Cuatro escenarios que debe explicar el estudiante:** (1) vacía → `false`; (2) cabeza → se cambia `primero`; (3) intermedio → se cambia `anterior.siguiente`; (4) último → el enlace del anterior pasa a `null`. En una lista de un nodo, eliminar la cabeza también convierte `primero` en `null`.

**Error controlado para el análisis:** escribir `anterior.siguiente = null` al eliminar 20 de `10 -> 20 -> 30 -> 40` no «elimina un solo nodo»: hace inalcanzables 30 y 40. **Corrección argumentada:** conectar con `actual.siguiente`, porque representa el subtramo que queremos conservar.

### T03 - Tarea espejo (sin solución)

Con la lista `4 -> 8 -> 12 -> 16`, elimina primero `12`, después `4` y por último `16`. Tras cada operación muestra el recorrido. Incluye la búsqueda de eliminación de `99`, que no existe. **Criterio:** el nodo que permanezca debe ser accesible desde `primero`; ninguna eliminación debe afectar elementos no solicitados.

---

# TEMA 2 · LISTAS DOBLEMENTE ENLAZADAS

## EJ04 ↔ T04 · ¿Qué cambia cuando cada nodo conoce a sus dos vecinos? (14 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ04/estudiante/ListaDobleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ04/tarea_estudiante/ListaDobleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** Queremos recorrer las solicitudes desde la primera hasta la última y viceversa. Una lista simple no conserva una referencia al nodo anterior, de modo que necesitamos una nueva estructura.

**Archivo:** `FASE_15/LAB-S08-EJ04/estudiante/ListaDobleRepasoS08.java`. Se crea **otro programa independiente** para no mezclar la estructura simple con la doble.

### Paso 1 - Crear `NodoDoble` con dos referencias

```java
static class NodoDoble {
    int id;
    NodoDoble anterior;
    NodoDoble siguiente;

    NodoDoble(int id) {
        this.id = id;
        this.anterior = null;
        this.siguiente = null;
    }
}
```

**¿Por qué no reutilizamos directamente `Nodo`?** El nodo anterior solo dispone de `siguiente`; ahora necesitamos representar explícitamente dos direcciones. Crear `NodoDoble` evita asumir que la lista simple ya guardaba un enlace hacia atrás.

1. `static class NodoDoble` define un tipo de objeto destinado a la nueva estructura.
2. `int id` conserva el dato de la solicitud.
3. `NodoDoble anterior` representa la conexión con el nodo previo.
4. `NodoDoble siguiente` representa la conexión con el nodo posterior.
5. `NodoDoble(int id)` construye cada nuevo elemento.
6. `this.id = id` guarda el identificador recibido.
7. `this.anterior = null` y `this.siguiente = null` inicializan ambos enlaces como vacíos porque aún no se ha conectado el nodo.

### Paso 2 - Guardar explícitamente el primer y último nodo

```java
static class ListaDoble {
    NodoDoble primero;
    NodoDoble ultimo;
}
```

`ListaDoble` tiene la responsabilidad de manejar el conjunto. `primero` señala la cabeza; `ultimo` señala la cola. Esta segunda referencia permite agregar al final sin recorrer todos los nodos, **siempre que** mantengamos correctamente ese extremo.

### Paso 3 - Insertar y actualizar ambas direcciones

Dentro de `ListaDoble`:

```java
void agregarFinal(int id) {
    NodoDoble nuevo = new NodoDoble(id);
    if (primero == null) {
        primero = nuevo;
        ultimo = nuevo;
        return;
    }
    nuevo.anterior = ultimo;
    ultimo.siguiente = nuevo;
    ultimo = nuevo;
}
```

1. `void agregarFinal(int id)` define una operación que modifica la estructura.
2. `new NodoDoble(id)` crea un nodo cuyos dos enlaces comienzan en `null`.
3. `if (primero == null)` verifica si la lista está vacía.
4. `primero = nuevo` y `ultimo = nuevo` hacen que el único nodo sea, simultáneamente, principio y final.
5. `return` evita ejecutar enlaces de una lista que anteriormente estaba vacía.
6. `nuevo.anterior = ultimo` conecta **desde el nuevo nodo hacia atrás**, al último que ya existía.
7. `ultimo.siguiente = nuevo` conecta **desde el último anterior hacia adelante**, al nuevo nodo.
8. `ultimo = nuevo` actualiza el extremo final. Sin esta línea, la próxima inserción se realizaría desde un extremo desactualizado.

**Traza:**

```text
primero                                  ultimo
   |                                        |
  [10] <-> [20] <-> [30] <-> [40]
   ^                              ^
   anterior = null          siguiente = null
```

**Antes de ejecutar:** al insertar 20 después de 10, ¿qué referencia tiene que apuntar a 10? `20.anterior`; ¿cuál apunta a 20? `10.siguiente`.

**Variante:** insertar en una lista vacía debe establecer ambos extremos en el mismo objeto. **Error controlado:** olvidar `nuevo.anterior = ultimo` permitiría un recorrido hacia adelante, pero impediría regresar desde el nuevo nodo. **Corrección:** crear las **dos conexiones** antes de mover `ultimo`.


#### Microconstrucción comentada: por qué la lista doble necesita dos sentidos

**Decisión de diseño:** la lista simple no sabe cómo regresar desde un nodo al anterior. No agregamos un método vacío que finja esa habilidad: construimos el tipo `NodoDoble` con los campos `anterior` y `siguiente`. La clase `ListaDoble` conserva tanto `primero` como `ultimo`; esto permite insertar al final en tiempo constante en esta implementación, siempre que actualicemos las referencias de forma consistente.

| Instrucción | ¿Por qué se genera? | Qué ocurre en memoria |
|---|---|---|
| `static class NodoDoble` | Encapsular dato y dos enlaces en cada elemento. | Cada solicitud tiene dos vecinos posibles. |
| `int id;` | Identificar la solicitud. | Un nodo puede contener 10, 20, etc. |
| `NodoDoble anterior;` | Poder retroceder sin volver a la cabeza. | En la primera solicitud vale `null`. |
| `NodoDoble siguiente;` | Poder avanzar como en la lista simple. | En la última solicitud vale `null`. |
| `NodoDoble(int id)` | Inicializar cada objeto recién creado. | `this.id` recibe el argumento; ambos enlaces empiezan en `null`. |
| `NodoDoble primero;` | Retener la cabeza para recorrido hacia adelante. | No se pierde el punto de entrada a la lista. |
| `NodoDoble ultimo;` | Conocer directamente el extremo final. | Una inserción no necesita buscar al último mediante `while`. |
| `NodoDoble nuevo = new NodoDoble(id);` | Tener un nuevo objeto antes de conectarlo. | Aún no es alcanzable desde la lista. |
| `if (primero == null)` | Resolver el caso vacío sin acceder a `ultimo.siguiente`. | `primero = nuevo; ultimo = nuevo;` apuntan al mismo objeto. |
| `nuevo.anterior = ultimo;` | Preparar el enlace de vuelta desde el nuevo. | El nuevo sabe quién lo precede. |
| `ultimo.siguiente = nuevo;` | Preparar el enlace de ida desde el antiguo último. | El anterior último ya conduce al nuevo. |
| `ultimo = nuevo;` | Registrar el nuevo extremo final. | La propiedad `ultimo.siguiente == null` sigue cumpliéndose. |

**Simulación del paso al agregar 30 a `10 ⇄ 20`:**

```text
Estado previo: primero -> 10 ⇄ 20 <- ultimo
nuevo = NodoDoble(30): 30.anterior=null, 30.siguiente=null (aún aislado)
nuevo.anterior = ultimo   -> 30.anterior apunta a 20.
ultimo.siguiente = nuevo  -> 20.siguiente apunta a 30.
ultimo = nuevo            -> la cola cambia de 20 a 30.
Estado final: primero -> 10 ⇄ 20 ⇄ 30 <- ultimo
```

**Por qué importa el orden:** antes de mover `ultimo`, se necesita acceder al antiguo último para conectar el nuevo. Si ejecutaras `ultimo = nuevo` en primer lugar, perderías la referencia directa al anterior último para hacer la conexión que corresponde. **Verifica** las cuatro propiedades: `primero.id=10`, `ultimo.id=30`, `ultimo.anterior.id=20`, `primero.anterior==null`.

### T04 - Tarea espejo (sin solución)

Crea una `ListaDoble` con solicitudes `5`, `15` y `25`. Dibuja ambos enlaces de cada par de vecinos y señala los valores de `primero.id`, `ultimo.id` y `ultimo.anterior.id`. **Criterio:** `primero.anterior` debe ser `null` y `ultimo.siguiente` también.

---

## EJ05 ↔ T05 · Recorrer en ambos sentidos y demostrar la coherencia (14 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ05/estudiante/ListaDobleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ05/tarea_estudiante/ListaDobleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** No basta con insertar correctamente: debemos demostrar que los enlaces de ida y vuelta siguen siendo consistentes.

**Punto de partida:** `NodoDoble`, `ListaDoble`, `primero`, `ultimo` y `agregarFinal` de EJ04 permanecen sin cambios.

### Paso 1 - Escribir el recorrido hacia adelante

```java
String recorrerAdelante() {
    StringBuilder texto = new StringBuilder();
    NodoDoble actual = primero;
    while (actual != null) {
        if (texto.length() > 0) {
            texto.append(" <-> ");
        }
        texto.append(actual.id);
        actual = actual.siguiente;
    }
    return texto.length() == 0 ? "(vacia)" : texto.toString();
}
```

1. `String recorrerAdelante()` devolverá una representación textual de la lista.
2. `StringBuilder texto` acumula los identificadores sin modificar nodos.
3. `NodoDoble actual = primero` establece la dirección inicial desde la cabeza.
4. `while (actual != null)` avanza mientras exista un nodo.
5. `if (texto.length() > 0)` evita agregar el separador antes del primer identificador.
6. `texto.append(" <-> ")` añade una marca visual de doble conexión, **no modifica ninguna referencia real**.
7. `texto.append(actual.id)` lee el valor del nodo visitado.
8. `actual = actual.siguiente` recorre hacia adelante usando la dirección adecuada.
9. La expresión `condicion ? valorSiVerdadero : valorSiFalso` devuelve `"(vacia)"` si no se visitó ningún nodo, o el texto creado si se visitaron varios.

### Paso 2 - Escribir el recorrido hacia atrás

```java
String recorrerAtras() {
    StringBuilder texto = new StringBuilder();
    NodoDoble actual = ultimo;
    while (actual != null) {
        if (texto.length() > 0) {
            texto.append(" <-> ");
        }
        texto.append(actual.id);
        actual = actual.anterior;
    }
    return texto.length() == 0 ? "(vacia)" : texto.toString();
}
```

**¿Por qué otro método?** Aunque reutiliza la misma idea, debe **comenzar desde `ultimo`** y desplazarse a través de `anterior`. La línea `NodoDoble actual = ultimo` fija el extremo correcto; `actual = actual.anterior` mueve el cursor en sentido inverso. Las demás instrucciones conservan su propósito: crear texto, no alterar enlaces.

### Paso 3 - Validar la integridad con `enlacesValidos`

```java
boolean enlacesValidos() {
    if (primero == null || ultimo == null) {
        return primero == null && ultimo == null;
    }
    if (primero.anterior != null || ultimo.siguiente != null) {
        return false;
    }
    NodoDoble actual = primero;
    NodoDoble previo = null;
    while (actual != null) {
        if (actual.anterior != previo) {
            return false;
        }
        previo = actual;
        actual = actual.siguiente;
    }
    return previo == ultimo;
}
```

**¿Por qué crear una verificación?** Un programa podría imprimir la secuencia hacia adelante mientras sus enlaces `anterior` están equivocados. La verificación comprueba una condición más fuerte que una simple impresión.

1. `boolean enlacesValidos()` anuncia un resultado verdadero/falso de consistencia.
2. `if (primero == null || ultimo == null)` detecta si falta alguno de los extremos. `||` significa «o».
3. `return primero == null && ultimo == null` acepta únicamente el caso en que **ambos** son `null` (lista realmente vacía); si falta solo uno, retorna `false`.
4. `if (primero.anterior != null || ultimo.siguiente != null)` comprueba que la cabeza no tenga anterior y la cola no tenga siguiente.
5. `return false` informa una conexión inválida en los extremos.
6. `NodoDoble actual = primero` inicia la inspección de todos los nodos.
7. `NodoDoble previo = null` representa el anterior que esperamos encontrar en el primer nodo.
8. `while (actual != null)` recorre la lista siguiendo referencias `siguiente`.
9. `if (actual.anterior != previo)` compara la conexión real hacia atrás con el nodo previamente visitado. Si son distintos, la bidireccionalidad está rota.
10. `previo = actual` guarda el nodo visitado para que la siguiente iteración pueda verificar su enlace inverso.
11. `actual = actual.siguiente` avanza sin modificar la estructura.
12. `return previo == ultimo` confirma que el último nodo recorrido coincide con el extremo final registrado.

**Nota de alcance:** este validador es suficiente para los casos acíclicos trabajados en clase; no está diseñado para recorrer estructuras corruptas con ciclos arbitrarios.

**Predicción y salida esperada:**

```text
Adelante: 10 <-> 20 <-> 30 <-> 40
Atras: 40 <-> 30 <-> 20 <-> 10
Enlaces validos: true
```

**Variante:** una lista vacía con `primero == null` y `ultimo == null` debe considerarse válida. **Error controlado:** omitir `ultimo.siguiente = nuevo` en una inserción dejaría al nuevo nodo inaccesible desde `primero`, aunque `ultimo` se haya actualizado. El validador retornaría `false` en el caso trabajado. **Corrección:** conservar ambos enlaces en la inserción.


#### Microconstrucción comentada: no basta imprimir; debemos comprobar integridad

**¿Por qué existen TRES métodos en este ejemplo?** `recorrerAdelante()` observa la cadena por el campo `siguiente`; `recorrerAtras()` observa por `anterior`; `enlacesValidos()` verifica que las referencias inversas correspondan a los vecinos realmente visitados. Que el recorrido hacia adelante tenga cuatro números no demuestra, por sí solo, que sus enlaces hacia atrás estén bien construidos.

| Instrucción o bloque | Motivo de crearla | Resultado o fallo que identifica |
|---|---|---|
| `String recorrerAdelante()` | Mostrar la secuencia desde la cabeza. | `10 <-> 20 <-> 30 <-> 40`. |
| `NodoDoble actual = primero;` | Iniciar la observación desde el extremo correcto. | Al avanzar se lee `actual.siguiente`. |
| `String recorrerAtras()` | Mostrar la secuencia desde la cola. | `40 <-> 30 <-> 20 <-> 10`. |
| `NodoDoble actual = ultimo;` | Iniciar la lectura inversa desde el último. | Al retroceder se lee `actual.anterior`. |
| `StringBuilder texto` | Acumular texto de salida sin modificar los enlaces. | El texto es una representación, no una estructura. |
| `while (actual != null)` | Procesar cada elemento, también ambos extremos. | Termina al rebasar un extremo. |
| `if (texto.length() > 0)` | Imprimir separadores solo entre datos. | No agrega un separador antes del primer número. |
| `return texto.length() == 0 ? "(vacia)" : texto.toString();` | Entregar una salida significativa incluso en lista vacía. | Operador ternario: devuelve una u otra expresión. |
| `if (primero == null || ultimo == null)` | Detectar extremos inconsistentes/vacíos. | Una lista vacía correcta necesita ambos `null`. |
| `return primero == null && ultimo == null;` | Confirmar estado vacío coherente. | `true` solo si no existe cabeza ni cola. |
| `if (primero.anterior != null || ultimo.siguiente != null)` | Validar que no existan enlaces fuera de extremos. | Si alguno tiene vecino externo, devuelve `false`. |
| `NodoDoble previo = null;` | Registrar el nodo anterior esperado durante el recorrido. | Antes de la cabeza, el previo correcto es `null`. |
| `if (actual.anterior != previo)` | Verificar que el enlace hacia atrás coincida con la ruta visitada. | La validación falla en el primer enlace incorrecto. |
| `previo = actual; actual = actual.siguiente;` | Actualizar el esperado y avanzar para la siguiente revisión. | Las referencias observadas avanzan juntas sin mutar la lista. |
| `return previo == ultimo;` | Asegurar que el último nodo alcanzado sea el extremo declarado. | Si la cola declarada difiere del final del recorrido, `false`. |

**Traza esencial de validación de `10 ⇄ 20 ⇄ 30`:**

| Paso | `actual` | `previo` esperado | `actual.anterior` real | ¿Coinciden? |
|---|---|---|---|---|
| 1 | 10 | `null` | `null` | Sí. |
| 2 | 20 | 10 | 10 | Sí. |
| 3 | 30 | 20 | 20 | Sí. |
| final | `null` | 30 | — | `previo == ultimo`, verdadero. |

**Error que revela la diferencia entre observar y validar:** si el enlace `30.anterior` apuntara erróneamente a 10, el recorrido **hacia adelante** todavía podría imprimir `10, 20, 30`. El validador detectaría que el anterior esperado de 30 es 20 y devolvería `false`. Una vez identificado el error, reconecta `30.anterior = 20` en la copia de demostración.

**Alcance de la validación:** este método verifica coherencia de extremos y enlaces hacia atrás **para una lista lineal que termina en `null`**. No es un detector general de ciclos arbitrarios; el ejercicio no incorpora listas circulares.

### T05 - Tarea espejo (sin solución)

Crea una lista doble con `5`, `15`, `25` y `35`; imprime el recorrido de ida, el de vuelta y la verificación de enlaces. Explica por qué no es suficiente observar solamente el recorrido de ida. **Criterio:** ambas direcciones contienen exactamente los mismos identificadores en orden inverso y la validación es positiva.

---

## EJ06 ↔ T06 · Eliminar un nodo doble sin romper la ida ni la vuelta (12 minutos)

**Laboratorio correspondiente:** `FASE_15/LAB-S08-EJ06/estudiante/ListaDobleRepasoS08.java`. **Tarea espejo:** `FASE_15/LAB-S08-EJ06/tarea_estudiante/ListaDobleRepasoS08.java`. La respuesta completa de la tarea se consulta únicamente en `tarea_docente/` (guía docente).

**Problema.** Retirar la solicitud 20 de `10 <-> 20 <-> 30 <-> 40`, conservando ambas direcciones. Después retirar un extremo para comprobar el caso límite.

**¿Por qué debemos crear un método de eliminación distinto?** En una lista simple bastaba actualizar `anterior.siguiente`. En una doble, también hay que actualizar **`siguiente.anterior`**. Además, si quitamos el primero o último, hay que actualizar las referencias de los extremos administrados por `ListaDoble`.

### Paso 1 - Buscar el nodo y detectar ausencia

```java
boolean eliminar(int id) {
    NodoDoble actual = primero;
    while (actual != null && actual.id != id) {
        actual = actual.siguiente;
    }
    if (actual == null) {
        return false;
    }
```

1. `boolean eliminar(int id)` devolverá si hubo o no una eliminación.
2. `NodoDoble actual = primero` crea el cursor de búsqueda desde la cabeza.
3. `while (actual != null && actual.id != id)` avanza de forma segura mientras exista nodo y no coincida con la solicitud buscada.
4. `actual = actual.siguiente` llega al siguiente candidato.
5. `if (actual == null)` descubre que el identificador no existe.
6. `return false` evita cualquier actualización si no encontramos el elemento.

### Paso 2 - Reconectar al vecino anterior o actualizar `primero`

Añade dentro del método:

```java
    if (actual.anterior == null) {
        primero = actual.siguiente;
    } else {
        actual.anterior.siguiente = actual.siguiente;
    }
```

1. `if (actual.anterior == null)` identifica que el nodo retirado **es el primero**.
2. `primero = actual.siguiente` mueve la cabeza hacia el nodo posterior, que será el nuevo inicio.
3. `else` cubre la eliminación de un nodo que sí tiene vecino anterior.
4. `actual.anterior.siguiente = actual.siguiente` hace que el nodo anterior **salte** al que viene después del nodo retirado.

### Paso 3 - Reconectar al vecino posterior o actualizar `ultimo`

Completa el método:

```java
    if (actual.siguiente == null) {
        ultimo = actual.anterior;
    } else {
        actual.siguiente.anterior = actual.anterior;
    }
    return true;
}
```

1. `if (actual.siguiente == null)` reconoce que estamos retirando el último nodo.
2. `ultimo = actual.anterior` mueve el extremo final hacia el nodo anterior.
3. `else` cubre el caso en que existe vecino posterior.
4. `actual.siguiente.anterior = actual.anterior` reconecta **hacia atrás** al nodo posterior con el vecino anterior del nodo eliminado.
5. `return true` confirma que la operación se realizó.

**Traza de la eliminación intermedia:**

```text
ANTES:   10 <-> 20 <-> 30 <-> 40
              actual
CAMBIO 1: 10.siguiente = 30
CAMBIO 2: 30.anterior  = 10
DESPUÉS: 10 <--------> 30 <-> 40
```

**Comprobación:**

```java
System.out.println("Eliminar 20: " + lista.eliminar(20));
System.out.println("Adelante final: " + lista.recorrerAdelante());
System.out.println("Atras final: " + lista.recorrerAtras());
System.out.println("Enlaces validos final: " + lista.enlacesValidos());
```

**Salida:**

```text
Eliminar 20: true
Adelante final: 10 <-> 30 <-> 40
Atras final: 40 <-> 30 <-> 10
Enlaces validos final: true
```

**Variante:** retirar el primero actualiza `primero`; retirar el último actualiza `ultimo`. Si solo queda un elemento y se elimina, **ambos extremos** deben terminar en `null`. **Error controlado:** omitir `actual.siguiente.anterior = actual.anterior` puede dejar un enlace hacia atrás apuntando al nodo retirado. **Corrección:** actualizar ambas direcciones y ejecutar `enlacesValidos()`.


#### Microconstrucción comentada: por qué eliminar en doble necesita dos reconexiones

**Nuevo problema:** en la lista simple bastaba conectar `anterior.siguiente` con el sucesor. En una lista doble esa modificación no es suficiente: si el nodo de la derecha siguiera apuntando hacia atrás al nodo eliminado, el recorrido inverso quedaría incoherente. El método `eliminar(int id)` debe actualizar **dos enlaces para un nodo intermedio**, además de mantener `primero` y `ultimo` cuando se retiran extremos.

| Bloque real del método | Por qué se introduce | Efecto comprobable |
|---|---|---|
| `boolean eliminar(int id)` | Necesitamos operación de borrado con resultado `true/false`. | Los llamadores saben si existía el nodo solicitado. |
| `NodoDoble actual = primero;` | La búsqueda parte desde la cabeza. | No altera la cabeza ni ningún enlace. |
| `while (actual != null && actual.id != id)` | Encontrar el nodo o llegar al final sin acceder a `null.id`. | Si `actual==null`, devuelve `false`. |
| `if (actual.anterior == null)` | Reconocer el caso de eliminar el primero. | `primero = actual.siguiente`. |
| `else { actual.anterior.siguiente = actual.siguiente; }` | Conectar hacia adelante al vecino izquierdo con el derecho. | Evita dejar la ida apuntando al nodo retirado. |
| `if (actual.siguiente == null)` | Reconocer que el nodo retirado era el último. | `ultimo = actual.anterior`. |
| `else { actual.siguiente.anterior = actual.anterior; }` | Reconectar hacia atrás al vecino derecho con el izquierdo. | El recorrido inverso deja de visitar al retirado. |
| `return true;` | Comunicar que se reconectó correctamente. | Después validamos ida, vuelta y `enlacesValidos()`. |

**Reconstrucción microscópica de `eliminar(20)` en `10 ⇄ 20 ⇄ 30 ⇄ 40`:**

```text
ANTES: 10.siguiente=20 | 20.anterior=10 | 20.siguiente=30 | 30.anterior=20.
PASO A: actual=20. Su anterior no es null: 10.siguiente = 30.
PASO B: actual=20. Su siguiente no es null: 30.anterior = 10.
DESPUÉS: 10 ⇄ 30 ⇄ 40. Los dos recorridos ya no alcanzan 20.
COMPROBAR: recorrerAdelante() = 10 <-> 30 <-> 40
           recorrerAtras() = 40 <-> 30 <-> 10
           enlacesValidos() = true
```

**Tabla de decisiones para casos límite:**

| Nodo eliminado | Cambio hacia adelante | Cambio hacia atrás | Extremo actualizado |
|---|---|---|---|
| Cabeza (10) | `primero = 20` | `20.anterior = null` | `primero` |
| Intermedio (20) | `10.siguiente = 30` | `30.anterior = 10` | Ninguno |
| Último (40) | `30.siguiente = null` | `ultimo = 30` | `ultimo` |
| Único nodo | `primero = null` | `ultimo = null` | Ambos |
| Inexistente (99) | Sin cambios | Sin cambios | Ninguno |

**Error controlado:** si solo se ejecuta `actual.anterior.siguiente = actual.siguiente` y se omite `actual.siguiente.anterior = actual.anterior`, el recorrido de ida parecerá correcto pero `recorrerAtras()` aún podrá pasar por el nodo retirado. **Corrección:** efectuar la segunda reconexión antes de afirmar que la eliminación se completó.

**Nota de alcance:** el código devuelve resultados correctos para los nodos enlazados por `agregarFinal`. No liberamos manualmente memoria en Java: tras perderse las referencias desde la lista, la gestión de memoria corresponde al recolector de basura cuando el objeto deja de ser alcanzable.

### T06 - Tarea espejo (sin solución)

En una lista doble `5 <-> 15 <-> 25 <-> 35`, retira `15` y después `35`. Tras cada operación, muestra ida, vuelta y el resultado de `enlacesValidos()`. **Restricción:** no construyas una nueva lista para simular la eliminación: modifica los enlaces de la existente. **Criterio:** los recorridos deben ser inversos entre sí y la validación debe mantenerse positiva.

---


## Concordancia para seguir la clase con el PowerPoint y FASE 15

| Ejemplo | Qué se construye y justifica | Laboratorio FASE 15 | Diapositivas de referencia |
|---|---|---|---|
| EJ01/T01 | `Nodo`, `ListaSimple`, `primero`, `agregarFinal` | `LAB-S08-EJ01` | Bloque EJ01 / propósito de crear nodos |
| EJ02/T02 | `recorrer`, `buscar` y cursor `actual` | `LAB-S08-EJ02` | Bloque EJ02 / por qué usar cursor |
| EJ03/T03 | `eliminar` y reconexión simple | `LAB-S08-EJ03` | Bloque EJ03 / eliminación sin perder la cola |
| EJ04/T04 | `NodoDoble`, `ListaDoble`, `ultimo`, inserción doble | `LAB-S08-EJ04` | Bloque EJ04 / dos sentidos |
| EJ05/T05 | `recorrerAdelante`, `recorrerAtras`, `enlacesValidos` | `LAB-S08-EJ05` | Bloque EJ05 / validar dos recorridos |
| EJ06/T06 | `eliminar` en lista doble | `LAB-S08-EJ06` | Bloque EJ06 / dos reconexiones |

**Regla de trabajo:** lee el problema → predice el estado → localiza el método nuevo → justifica por qué existe → interpreta cada instrucción → ejecuta el `estudiante/` → resuelve `tarea_estudiante/` → verifica el resultado sin ver las soluciones. Los seis `EJ` y seis `T` se desarrollan dentro de los 84 minutos de práctica ya distribuidos en el cronograma; los microanálisis ampliados son material de apoyo y estudio, no nuevas actividades con tiempo adicional.

## 5. Síntesis conceptual durante el cierre

| Pregunta de repaso | Lista simple | Lista doble |
|---|---|---|
| ¿Qué referencias tiene cada nodo? | `siguiente` | `anterior` y `siguiente` |
| ¿Cómo regresamos al nodo previo? | Recorriendo de nuevo desde `primero` si es necesario | Mediante `anterior` |
| ¿Qué cambia al eliminar un nodo intermedio? | `anterior.siguiente` | `anterior.siguiente` **y** `siguiente.anterior` |
| ¿Qué extremo guarda este ejemplo? | `primero` | `primero` y `ultimo` |
| ¿Cuánto cuesta buscar por identificador? | O(n) en el peor caso | O(n) en el peor caso |
| ¿Qué permite comprobar la integridad? | Un recorrido de los enlaces hacia adelante | Recorridos opuestos y `enlacesValidos()` |

**Evaluación Kahoot (16 minutos):** responde exclusivamente preguntas sobre creación de nodos, inserción, búsqueda, eliminación y enlace bidireccional. El banco de la FASE 7 elaborado anteriormente para las siete sesiones **debe acotarse a estos dos temas antes de usarse**.

## 6. Cierre - lectura sugerida del docente

«Hoy hemos concentrado el repaso en dos estructuras para poder observar lo que a veces pasa desapercibido cuando únicamente vemos el resultado por consola. En la lista simple comenzamos creando un nodo y diferenciamos dos responsabilidades: el nodo almacena el dato y un enlace; la lista administra el inicio y las operaciones sobre toda la secuencia. Después vimos que agregar, recorrer y buscar no hacen lo mismo. Recorrer avanza con un cursor; buscar compara un dato; eliminar modifica la conexión que deja pasar por el nodo retirado.

En la lista doble aprendimos por qué aparecen dos referencias por nodo y por qué debemos pensar tanto en el vecino anterior como en el siguiente. También comprobamos que una salida hacia adelante no garantiza que la estructura esté bien enlazada hacia atrás. Por eso añadimos una validación y comprobamos los casos de nodo intermedio, primero y último. El aprendizaje central es que cada instrucción tiene una responsabilidad: si se omite la actualización de un enlace, el programa puede seguir compilando, pero su estructura ya no representa correctamente la secuencia.

Para reforzar lo trabajado, vuelve a dibujar los recorridos de tus tareas espejo e identifica qué referencias se modifican. Puedes consultar ejercicios complementarios en Lideratec Academy, revisar su blog en <https://lideratecacademy.com/blog/> y usar los recursos del canal <https://www.youtube.com/@LideratecAcademy>. Esos recursos complementan, pero no reemplazan, la explicación de los enlaces que hoy hemos construido».

**Evidencia final del estudiante:** los seis resultados de tareas espejo, un diagrama antes/después de una eliminación simple y otro de una eliminación doble, y una explicación de por qué el método `enlacesValidos` detecta una conexión incorrecta.

**Contrato verificado con la FASE 15 incluida:** se conservan `EJ01–EJ06`, `T01–T06`, `ListaSimpleRepasoS08`, `ListaDobleRepasoS08`, `Nodo`, `NodoDoble`, `ListaSimple`, `ListaDoble` y los métodos documentados. Las tareas **no** incluyen soluciones en esta guía; se encuentran únicamente en la Guía Docente.
