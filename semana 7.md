---
course_id: ISIL-30710
session_id: S07
source_origin: PPT
status: validated
---

# Guía del estudiante — Sesión 07
## Listas doblemente enlazadas y listas circulares en Java
**Lideratec Academy · Algoritmo y Estructura de Datos Basado en IA**

## 1. Propósito de la sesión
Vas a construir nodos que se enlazan entre sí, gestionar listas con enlaces en dos sentidos y recorrer listas cuyo último nodo vuelve al primero. Aprenderás a insertar, eliminar, buscar, modificar y ordenar, sin perder referencias ni quedar atrapado en un ciclo infinito. Utilizarás Java desde la terminal y documentarás el efecto de cada cambio con salidas verificables. El producto final serán **10 programas Java completos** y **10 tareas espejo**, trabajados en orden progresivo. No se necesita ninguna biblioteca externa.

## 2. Resultado observable
### Lectura sugerida del docente
Hoy iremos más allá de guardar valores en una colección. Construiremos objetos que conocen a sus vecinos por medio de referencias. Una lista doble permite avanzar y retroceder, pero exige mantener dos conexiones coherentes; una lista circular permite volver al primer elemento, pero obliga a usar un criterio de parada diferente. Empezaremos creando un nodo sencillo, añadiremos una clase que administra la lista y verificaremos sus enlaces a medida que insertamos nuevos objetos. Después practicaremos operaciones donde el orden importa, como eliminar un nodo o mover la referencia al último. Para cada ejemplo anticiparás la salida antes de ejecutar y dibujarás al menos un estado anterior y posterior. Cuando aparezca una estructura de control, contaremos manualmente sus pasos hasta entender cuándo termina. Finalmente ordenaremos nodos sin alterar sus conexiones y resolveremos tareas de transferencia. El objetivo no es memorizar un programa: es poder explicar por qué cada asignación resulta necesaria, identificar el efecto de cambiar una referencia y comprobar que la estructura sigue siendo válida.

### Desempeños observables
- Construir nodos y referencias recíprocas.
- Insertar al inicio o final preservando extremos.
- Recorrer adelante y atrás o hasta cerrar un anillo.
- Buscar y modificar sin alterar enlaces.
- Eliminar nodos incluyendo listas vacías y de un elemento.
- Ordenar datos y justificar el límite de iteraciones.
- Anticipar, ejecutar, interpretar y depurar cada salida.

**Criterio de dominio:** puedes dibujar el estado de las referencias, explicar cada bifurcación y reproducir el resultado sin copiar una solución externa.

## 3. Antes de iniciar
**Debes saber:** crear clases y objetos Java, definir campos y métodos, utilizar condiciones, bucles, operadores y valores `null`, y ejecutar una clase con `main`.

**Debes tener disponible:** JDK con comandos `java` y `javac` y un editor de texto o IDE. Java 17 o 21 es suficiente para estos archivos sin paquetes; el material se comprobó con OpenJDK 21.

**Todavía no se requiere:** colecciones de la biblioteca estándar, genéricos, concurrencia, frameworks, interfaz gráfica, ni estructuras de datos ajenas a la sesión.

## 4. Preparación exacta del entorno
**Ruta A, JDK instalado:** abre Terminal (macOS/Linux) o PowerShell (Windows). Escribe `java -version` y después `javac -version`. Ambos deben imprimir su versión. Si solo aparece el primero, falta el compilador del JDK: no basta con tener un runtime.

**Ruta B, equipo nuevo:** instala un JDK mantenido que incluya compilador (por ejemplo Java 17 o 21) desde el proveedor aprobado por tu institución; consulta las guías de [dev.java](https://dev.java/learn/getting-started/) si necesitas orientación. Al terminar abre una terminal nueva y repite las dos comprobaciones. No necesitas Maven, Gradle ni librerías de terceros.

**Carpeta:** crea `S07_Listas_Java` y ábrela en un editor. Para hacerlo desde terminal utiliza `mkdir S07_Listas_Java`, después `cd S07_Listas_Java`. En Windows, macOS y Linux son comandos equivalentes para este caso. Para ejecutar cada ejemplo crea un archivo cuyo nombre sea exactamente el indicado, copia su código completo después de construirlo por pasos, y escribe:

```text
javac -encoding UTF-8 EJ01_NodoDoble.java
java EJ01_NodoDoble
```

Sustituye el nombre por el del ejemplo activo (sin `.java` en el comando java). El archivo generado `.class` indica que compiló; las líneas de consola permiten validar su comportamiento. Si sale `file not found`, comprueba carpeta y nombre exactos. Si sale `cannot find symbol`, revisa mayúsculas y tipos. Si sale `NullPointerException`, revisa las referencias antes de acceder a campos.

**Inicio mínimo:** escribe `public class Prueba { public static void main(String[] args) { System.out.println("Java listo"); } }` en `Prueba.java`, compílalo con `javac Prueba.java` y ejecútalo con `java Prueba`. Debes ver `Java listo`. Ahora continúa con EJ01.

## 5. Cómo trabajar cada ejemplo
Sigue primero los microbloques, explica en voz alta el propósito de cada línea, reconstruye el **archivo completo** y solo entonces compila. Durante los microbloques la clase puede estar temporalmente incompleta: eso es normal mientras faltan llaves o métodos. Las versiones finales de los ejemplos son **independientes** entre sí: cada archivo incluye las clases anidadas que necesita para ejecutarse por separado. Las tareas T01–T10 son ejercicios para ti; sus soluciones no aparecen en esta guía.


---
# EJ01 — Construir un nodo doble y unir dos referencias
**Contenido:** Estructura NodoDoble: dato, anterior, siguiente y constructor. **Nivel:** Fundamento.

## Qué vamos a construir y qué aprenderás
Un dato aislado no permite saber quién lo precede ni quién lo sigue. Cada nodo necesita dos referencias independientes. Al terminar, sabrás justificar este diseño: Una lista doble requiere enlaces recíprocos; escribir solo uno de ellos deja la relación incompleta.

## Archivos y punto de partida
Archivo nuevo: **`EJ01_NodoDoble.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `[10]   [20] (dos objetos, referencias sin establecer)`.

**Estado DESPUÉS:** `null <- [10] <-> [20] -> null`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Estructura NodoDoble: dato, anterior, siguiente y constructor.

**Por qué:** Una lista doble requiere enlaces recíprocos; escribir solo uno de ellos deja la relación incompleta.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 6
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ01_NodoDoble {
    static class NodoDoble {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ01_NodoDoble {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoDoble {`: Definimos el tipo de nodo para agrupar un dato y dos referencias del MISMO tipo. Es estática y anidada para que el ejemplo funcione dentro de un solo archivo independiente.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 6
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble anterior;
        NodoDoble siguiente;

        NodoDoble(int dato) {
```


### Explicación de cada instrucción funcional
- `NodoDoble anterior;`: anterior es una referencia de tipo NodoDoble al predecesor; null significa que todavía no existe predecesor.
- `NodoDoble siguiente;`: siguiente es una referencia de tipo NodoDoble al sucesor; actualizarla NO actualiza anterior automáticamente.
- `NodoDoble(int dato) {`: El constructor recibe el int que tendrá el nodo al crearlo. El parámetro dato es local; this.dato designa el campo perteneciente al nuevo objeto.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 6
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            this.dato = dato;
            this.anterior = null;
            this.siguiente = null;
```


### Explicación de cada instrucción funcional
- `this.dato = dato;`: this.dato asigna el parámetro al atributo del objeto actual; sin esa asignación, todos los nodos recién creados conservarían el valor por defecto 0.
- `this.anterior = null;`: Se documenta el estado inicial de anterior: todavía no hay conexión hacia atrás. No es una conexión automática.
- `this.siguiente = null;`: Se documenta el estado inicial de siguiente: el nodo nace desconectado y se vinculará explícitamente después.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 6
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
    }

    public static void main(String[] args) {
        NodoDoble primero = new NodoDoble(10);
        NodoDoble segundo = new NodoDoble(20);
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `NodoDoble primero = new NodoDoble(10);`: primero referencia un nuevo objeto NodoDoble. La palabra new reserva un objeto; el argumento numérico o String inicializa su dato.
- `NodoDoble segundo = new NodoDoble(20);`: segundo referencia OTRO objeto. Su dirección es distinta a primero y por ello hay que conectar explícitamente ambos objetos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 6
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        primero.siguiente = segundo;
        segundo.anterior = primero;
        System.out.println("10 -> " + primero.siguiente.dato);
```


### Explicación de cada instrucción funcional
- `primero.siguiente = segundo;`: El enlace hacia adelante sale desde primero y llega a segundo; se modifica una referencia, no el número guardado.
- `segundo.anterior = primero;`: El enlace inverso sale desde segundo y llega a primero, completando la relación bidireccional.
- `System.out.println("10 -> " + primero.siguiente.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 6
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("20 <- " + segundo.anterior.dato);
    }
}
```


### Explicación de cada instrucción funcional
- `System.out.println("20 <- " + segundo.anterior.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Se crean dos objetos distintos. `primero.siguiente` guarda la referencia a `segundo`; `segundo.anterior` guarda la referencia a `primero`. No son dos copias del número: son vínculos entre objetos.

**Antes / acción / después:** `[10]   [20] (dos objetos, referencias sin establecer)` → operación del programa → `null <- [10] <-> [20] -> null`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ01
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ01_NodoDoble {
    static class NodoDoble {
        int dato;
        NodoDoble anterior;
        NodoDoble siguiente;

        NodoDoble(int dato) {
            this.dato = dato;
            this.anterior = null;
            this.siguiente = null;
        }
    }

    public static void main(String[] args) {
        NodoDoble primero = new NodoDoble(10);
        NodoDoble segundo = new NodoDoble(20);
        primero.siguiente = segundo;
        segundo.anterior = primero;
        System.out.println("10 -> " + primero.siguiente.dato);
        System.out.println("20 <- " + segundo.anterior.dato);
    }
}
```


## Predicción ANTES de ejecutar
¿Qué dato se recupera usando `primero.siguiente.dato`? ¿Qué devuelve `segundo.anterior.dato`?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ01_NodoDoble.java
java EJ01_NodoDoble
```

**Salida observada en validación:**

```text
10 -> 20
20 <- 10
```


## Interpretación
Se crean dos objetos distintos. `primero.siguiente` guarda la referencia a `segundo`; `segundo.anterior` guarda la referencia a `primero`. No son dos copias del número: son vínculos entre objetos.

Este comportamiento responde a la decisión previa: Una lista doble requiere enlaces recíprocos; escribir solo uno de ellos deja la relación incompleta.

## Variación y error controlado
**Variación A:** Sustituye 10 y 20 por 15 y 45 y anticipa cómo cambian las dos líneas.

**Error o caso límite:** Si omites `segundo.anterior = primero`, la referencia anterior queda en `null`; acceder a `segundo.anterior.dato` produciría NullPointerException. Corrige el enlace antes de imprimir.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Por qué hay que actualizar los dos extremos? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T01 — Tarea espejo: Nodo doble de cadenas
**Relación directa:** EJ01 ↔ T01.

**Enunciado:** Transforma el dato de int a String y conecta dos nodos que almacenan Ana y Luis en ambos sentidos.

**Archivo a crear/modificar:** `T01_NodoString.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** Mantén las referencias anterior/siguiente de tipo NodoDoble; cambia únicamente el tipo del dato y los argumentos del constructor.

**Pista:** Un tipo String requiere literales entre comillas y el constructor debe recibir String.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Imprime Ana -> Luis y Luis <- Ana sin consultar campos null.

**Cómo validarla:** Compila sin errores y muestra ambas relaciones correctamente.

**Comandos:** `javac -encoding UTF-8 T01_NodoString.java` y `java T01_NodoString`. La clase pública en el código debe llamarse `T01_NodoString`.


---
# EJ02 — Insertar al final y recorrer una lista doble en ambos sentidos
**Contenido:** ListaDoble, inicio/ultimo, inserción final, recorrido hacia delante/atrás. **Nivel:** Aplicación.

## Qué vamos a construir y qué aprenderás
Necesitamos administrar varios nodos sin escribir enlaces manualmente, conservando el primer y último elemento. Al terminar, sabrás justificar este diseño: `ultimo` permite insertar en O(1) y empezar el recorrido inverso; `anterior` habilita regresar.

## Archivos y punto de partida
Archivo nuevo: **`EJ02_RecorridoDoble.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `inicio = null; ultimo = null`.

**Estado DESPUÉS:** `inicio -> [10] <-> [20] <-> [30] <- ultimo`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** ListaDoble, inicio/ultimo, inserción final, recorrido hacia delante/atrás.

**Por qué:** `ultimo` permite insertar en O(1) y empezar el recorrido inverso; `anterior` habilita regresar.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 12
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ02_RecorridoDoble {
    static class NodoDoble {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ02_RecorridoDoble {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoDoble {`: Definimos el tipo de nodo para agrupar un dato y dos referencias del MISMO tipo. Es estática y anidada para que el ejemplo funcione dentro de un solo archivo independiente.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
```


### Explicación de cada instrucción funcional
- `NodoDoble anterior, siguiente;`: Se declaran dos referencias separadas, anterior y siguiente, ambas de tipo NodoDoble. Los extremos de la lista tendrán una de ellas en null.
- `NodoDoble(int dato) { this.dato = dato; }`: El constructor recibe el int que tendrá el nodo al crearlo. El parámetro dato es local; this.dato designa el campo perteneciente al nuevo objeto.
- `static class ListaDoble {`: La clase administradora conserva referencias a nodos y concentra sus operaciones; separarla de NodoDoble evita que un nodo sea responsable de toda la colección.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble inicio, ultimo;
        void insertarFinal(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
```


### Explicación de cada instrucción funcional
- `NodoDoble inicio, ultimo;`: inicio y ultimo son referencias de la lista completa, no datos de nodos; empiezan en null y permiten conservar los extremos para recorrer o insertar.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoDoble nuevo = new NodoDoble(dato);`: nuevo es la referencia local al nodo recién creado. Su dato ya está inicializado; todavía deben ajustarse las referencias para integrarlo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (inicio == null) {
                inicio = ultimo = nuevo;
                return;
```


### Explicación de cada instrucción funcional
- `if (inicio == null) {`: Esta condición separa lista vacía de lista existente: evita acceder a referencias inexistentes y determina quién será el primer nodo.
- `inicio = ultimo = nuevo;`: Ambos extremos apuntan al mismo único nodo: se inicializa la lista doble no vacía en una sola operación.
- `return;`: Termina el método en este caso; se evita continuar con accesos que presuponen nodos existentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            }
            ultimo.siguiente = nuevo;
            nuevo.anterior = ultimo;
            ultimo = nuevo;
```


### Explicación de cada instrucción funcional
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.
- `nuevo.anterior = ultimo;`: Se establece el regreso desde nuevo al último anterior; debe realizarse antes de reemplazar la variable ultimo.
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        void mostrarAdelante() {
            NodoDoble actual = inicio;
            while (actual != null) {
```


### Explicación de cada instrucción funcional
- `void mostrarAdelante() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `NodoDoble actual = inicio;`: El cursor actual comienza en el primer nodo. Es una referencia de trabajo que puede cambiar sin alterar inicio.
- `while (actual != null) {`: El ciclo de lista doble continúa mientras exista un nodo actual; terminará al pasar del último al null de su siguiente/anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
```


### Explicación de cada instrucción funcional
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        void mostrarAtras() {
            NodoDoble actual = ultimo;
            while (actual != null) {
```


### Explicación de cada instrucción funcional
- `void mostrarAtras() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `NodoDoble actual = ultimo;`: El cursor empieza en el último nodo para recorrer con anterior hacia el inicio.
- `while (actual != null) {`: El ciclo de lista doble continúa mientras exista un nodo actual; terminará al pasar del último al null de su siguiente/anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                System.out.print(actual.dato + " ");
                actual = actual.anterior;
            }
            System.out.println();
```


### Explicación de cada instrucción funcional
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `actual = actual.anterior;`: El cursor avanza en sentido inverso usando anterior. No se reescribe ningún vínculo de la lista.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarFinal(10);
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaDoble lista = new ListaDoble();`: Se crea la colección sobre la que actuarán los métodos. Una lista nueva comienza con sus referencias a extremos en null.
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.mostrarAdelante();
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.mostrarAdelante();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.mostrarAtras();
    }
}
```


### Explicación de cada instrucción funcional
- `lista.mostrarAtras();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Primera inserción: inicio y ultimo referencian 10. Segunda: 10.siguiente=20, 20.anterior=10, ultimo=20. Tercera: se repite el patrón con 30. Recorrido directo 10→20→30; inverso 30→20→10.

**Antes / acción / después:** `inicio = null; ultimo = null` → operación del programa → `inicio -> [10] <-> [20] <-> [30] <- ultimo`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ02
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ02_RecorridoDoble {
    static class NodoDoble {
        int dato;
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
        NodoDoble inicio, ultimo;
        void insertarFinal(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            if (inicio == null) {
                inicio = ultimo = nuevo;
                return;
            }
            ultimo.siguiente = nuevo;
            nuevo.anterior = ultimo;
            ultimo = nuevo;
        }
        void mostrarAdelante() {
            NodoDoble actual = inicio;
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
        void mostrarAtras() {
            NodoDoble actual = ultimo;
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.anterior;
            }
            System.out.println();
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.mostrarAdelante();
        lista.mostrarAtras();
    }
}
```


## Predicción ANTES de ejecutar
¿Qué secuencia imprimirá cada recorrido después de insertar 10, 20, 30?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ02_RecorridoDoble.java
java EJ02_RecorridoDoble
```

**Salida observada en validación:**

```text
10 20 30 
30 20 10
```


## Interpretación
Primera inserción: inicio y ultimo referencian 10. Segunda: 10.siguiente=20, 20.anterior=10, ultimo=20. Tercera: se repite el patrón con 30. Recorrido directo 10→20→30; inverso 30→20→10.

Este comportamiento responde a la decisión previa: `ultimo` permite insertar en O(1) y empezar el recorrido inverso; `anterior` habilita regresar.

## Variación y error controlado
**Variación A:** Inserta un cuarto nodo con valor 40; comprueba que ambos extremos se mantienen.

**Error o caso límite:** Si no asignas `nuevo.anterior = ultimo` antes de cambiar ultimo, el recorrido inverso se interrumpe o queda incompleto. Restablece ambas referencias.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Por qué el recorrido inverso no puede comenzar desde inicio? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T02 — Tarea espejo: Recorrido inverso visible
**Relación directa:** EJ02 ↔ T02.

**Enunciado:** Agrega un nodo 40 y muestra el recorrido de 40 a 10 con separadores <- utilizando el campo anterior.

**Archivo a crear/modificar:** `T02_RecorridoInverso.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** No crees un arreglo ni recorras primero hacia delante para invertir valores.

**Pista:** Comienza por ultimo; avanza con actual = actual.anterior.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Primera salida 10 20 30 40; segunda salida 40 <- 30 <- 20 <- 10 <-.

**Cómo validarla:** La ruta inversa visita cuatro nodos, finaliza en null y no altera los enlaces.

**Comandos:** `javac -encoding UTF-8 T02_RecorridoInverso.java` y `java T02_RecorridoInverso`. La clase pública en el código debe llamarse `T02_RecorridoInverso`.


---
# EJ03 — Eliminar en una lista doble sin romper enlaces
**Contenido:** Eliminación de nodo: búsqueda, caso primero/intermedio/ultimo/unico. **Nivel:** Análisis.

## Qué vamos a construir y qué aprenderás
Eliminar un nodo requiere que sus vecinos vuelvan a conectarse en ambos sentidos y que inicio/ultimo no queden obsoletos. Al terminar, sabrás justificar este diseño: El orden evita perder conexiones; el caso único actualiza simultáneamente ambos extremos a null.

## Archivos y punto de partida
Archivo nuevo: **`EJ03_EliminarDoble.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `[10] <-> [20] <-> [30]`.

**Estado DESPUÉS:** `eliminar 20: [10] <-> [30]; eliminar 10 y 30: lista vacía`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Eliminación de nodo: búsqueda, caso primero/intermedio/ultimo/unico.

**Por qué:** El orden evita perder conexiones; el caso único actualiza simultáneamente ambos extremos a null.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 14
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ03_EliminarDoble {
    static class NodoDoble {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ03_EliminarDoble {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoDoble {`: Definimos el tipo de nodo para agrupar un dato y dos referencias del MISMO tipo. Es estática y anidada para que el ejemplo funcione dentro de un solo archivo independiente.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
```


### Explicación de cada instrucción funcional
- `NodoDoble anterior, siguiente;`: Se declaran dos referencias separadas, anterior y siguiente, ambas de tipo NodoDoble. Los extremos de la lista tendrán una de ellas en null.
- `NodoDoble(int dato) { this.dato = dato; }`: El constructor recibe el int que tendrá el nodo al crearlo. El parámetro dato es local; this.dato designa el campo perteneciente al nuevo objeto.
- `static class ListaDoble {`: La clase administradora conserva referencias a nodos y concentra sus operaciones; separarla de NodoDoble evita que un nodo sea responsable de toda la colección.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble inicio, ultimo;
        void insertarFinal(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
```


### Explicación de cada instrucción funcional
- `NodoDoble inicio, ultimo;`: inicio y ultimo son referencias de la lista completa, no datos de nodos; empiezan en null y permiten conservar los extremos para recorrer o insertar.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoDoble nuevo = new NodoDoble(dato);`: nuevo es la referencia local al nodo recién creado. Su dato ya está inicializado; todavía deben ajustarse las referencias para integrarlo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (inicio == null) { inicio = ultimo = nuevo; return; }
            ultimo.siguiente = nuevo;
            nuevo.anterior = ultimo;
```


### Explicación de cada instrucción funcional
- `if (inicio == null) { inicio = ultimo = nuevo; return; }`: Esta condición separa lista vacía de lista existente: evita acceder a referencias inexistentes y determina quién será el primer nodo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.
- `nuevo.anterior = ultimo;`: Se establece el regreso desde nuevo al último anterior; debe realizarse antes de reemplazar la variable ultimo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            ultimo = nuevo;
        }
        boolean eliminar(int dato) {
            NodoDoble actual = inicio;
```


### Explicación de cada instrucción funcional
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `boolean eliminar(int dato) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `NodoDoble actual = inicio;`: El cursor actual comienza en el primer nodo. Es una referencia de trabajo que puede cambiar sin alterar inicio.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            while (actual != null && actual.dato != dato) {
                actual = actual.siguiente;
            }
            if (actual == null) return false;
```


### Explicación de cada instrucción funcional
- `while (actual != null && actual.dato != dato) {`: El operador && exige ambas condiciones: que exista un nodo y que todavía no se haya encontrado el dato; evita leer un campo de null.
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `if (actual == null) return false;`: Cuando actual es null se agotó el recorrido de la lista doble sin hallar el valor; devolver false evita modificar enlaces inválidos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (actual.anterior == null) inicio = actual.siguiente;
            else actual.anterior.siguiente = actual.siguiente;
            if (actual.siguiente == null) ultimo = actual.anterior;
```


### Explicación de cada instrucción funcional
- `if (actual.anterior == null) inicio = actual.siguiente;`: Detecta que actual es el primero; en ese caso no existe predecesor y hay que mover inicio al sucesor.
- `else actual.anterior.siguiente = actual.siguiente;`: La asignación cambia el estado del identificador de la izquierda usando la expresión de la derecha. Distingue si modifica un dato, una referencia entre nodos o una variable local de control.
- `if (actual.siguiente == null) ultimo = actual.anterior;`: Detecta que actual es el último; en ese caso no existe sucesor y hay que mover ultimo al predecesor.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            else actual.siguiente.anterior = actual.anterior;
            return true;
        }
        void mostrar() {
```


### Explicación de cada instrucción funcional
- `else actual.siguiente.anterior = actual.anterior;`: La asignación cambia el estado del identificador de la izquierda usando la expresión de la derecha. Distingue si modifica un dato, una referencia entre nodos o una variable local de control.
- `return true;`: La operación termina inmediatamente con éxito: encontró, modificó o eliminó el elemento esperado.
- `void mostrar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            NodoDoble actual = inicio;
            while (actual != null) {
                System.out.print(actual.dato + " ");
```


### Explicación de cada instrucción funcional
- `NodoDoble actual = inicio;`: El cursor actual comienza en el primer nodo. Es una referencia de trabajo que puede cambiar sin alterar inicio.
- `while (actual != null) {`: El ciclo de lista doble continúa mientras exista un nodo actual; terminará al pasar del último al null de su siguiente/anterior.
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                actual = actual.siguiente;
            }
            System.out.println();
        }
    }
    public static void main(String[] args) {
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        ListaDoble lista = new ListaDoble();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
```


### Explicación de cada instrucción funcional
- `ListaDoble lista = new ListaDoble();`: Se crea la colección sobre la que actuarán los métodos. Una lista nueva comienza con sus referencias a extremos en null.
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(30);
        lista.eliminar(20);
        lista.mostrar();
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.eliminar(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 14 — Microbloque 13 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.eliminar(10);
        lista.eliminar(30);
        System.out.println("Vacia: " + (lista.inicio == null));
```


### Explicación de cada instrucción funcional
- `lista.eliminar(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.eliminar(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `System.out.println("Vacia: " + (lista.inicio == null));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 15 — Microbloque 14 de 14
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
    }
}
```


### Explicación de cada instrucción funcional
- Las llaves de cierre completan el alcance del método o condición ya explicado. No cambian por sí solas datos ni referencias.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Buscamos 20. Su anterior es 10 y su siguiente es 30; asignamos 10.siguiente=30 y 30.anterior=10. Luego eliminamos primero y único sin dejar referencias colgantes en inicio o ultimo.

**Antes / acción / después:** `[10] <-> [20] <-> [30]` → operación del programa → `eliminar 20: [10] <-> [30]; eliminar 10 y 30: lista vacía`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ03
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ03_EliminarDoble {
    static class NodoDoble {
        int dato;
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
        NodoDoble inicio, ultimo;
        void insertarFinal(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            if (inicio == null) { inicio = ultimo = nuevo; return; }
            ultimo.siguiente = nuevo;
            nuevo.anterior = ultimo;
            ultimo = nuevo;
        }
        boolean eliminar(int dato) {
            NodoDoble actual = inicio;
            while (actual != null && actual.dato != dato) {
                actual = actual.siguiente;
            }
            if (actual == null) return false;
            if (actual.anterior == null) inicio = actual.siguiente;
            else actual.anterior.siguiente = actual.siguiente;
            if (actual.siguiente == null) ultimo = actual.anterior;
            else actual.siguiente.anterior = actual.anterior;
            return true;
        }
        void mostrar() {
            NodoDoble actual = inicio;
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            }
            System.out.println();
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.eliminar(20);
        lista.mostrar();
        lista.eliminar(10);
        lista.eliminar(30);
        System.out.println("Vacia: " + (lista.inicio == null));
    }
}
```


## Predicción ANTES de ejecutar
Tras eliminar 20, ¿quién será el siguiente de 10? Al eliminar todos, ¿qué vale inicio?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ03_EliminarDoble.java
java EJ03_EliminarDoble
```

**Salida observada en validación:**

```text
10 30 
Vacia: true
```


## Interpretación
Buscamos 20. Su anterior es 10 y su siguiente es 30; asignamos 10.siguiente=30 y 30.anterior=10. Luego eliminamos primero y único sin dejar referencias colgantes en inicio o ultimo.

Este comportamiento responde a la decisión previa: El orden evita perder conexiones; el caso único actualiza simultáneamente ambos extremos a null.

## Variación y error controlado
**Variación A:** Prueba borrar un valor ausente y observa el booleano false.

**Error o caso límite:** Si se actualiza solo `anterior.siguiente`, el enlace inverso sigue apuntando a un nodo retirado. Corrige también `siguiente.anterior`.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Por qué hay dos comprobaciones con null? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T03 — Tarea espejo: Verificar eliminación en casos extremos
**Relación directa:** EJ03 ↔ T03.

**Enunciado:** Ejecuta de forma explícita los casos: borrar primero, último, único nodo restante y valor inexistente.

**Archivo a crear/modificar:** `T03_CasosEliminarDoble.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** No agregues sentencias que modifiquen internamente el método eliminar; organiza las pruebas en main.

**Pista:** Comprueba inicio y ultimo al terminar la última eliminación.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Secuencia de salida con 20 30, luego 20, después Vacia: true y Elimina inexistente: false.

**Cómo validarla:** Se ejecutan los cuatro casos sin NullPointerException.

**Comandos:** `javac -encoding UTF-8 T03_CasosEliminarDoble.java` y `java T03_CasosEliminarDoble`. La clase pública en el código debe llamarse `T03_CasosEliminarDoble`.


---
# EJ04 — Buscar y modificar un dato de lista doble
**Contenido:** Búsqueda secuencial, comparación y sustitución del primer dato encontrado. **Nivel:** Aplicación.

## Qué vamos a construir y qué aprenderás
Debemos encontrar un valor comparando uno a uno y luego cambiar el dato sin desconectar nodos. Al terminar, sabrás justificar este diseño: Buscar devuelve una respuesta; modificar produce un cambio persistente dentro del mismo objeto.

## Archivos y punto de partida
Archivo nuevo: **`EJ04_BuscarModificarDoble.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `10 <-> 20 <-> 30`.

**Estado DESPUÉS:** `10 <-> 25 <-> 30 (misma topología)`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Búsqueda secuencial, comparación y sustitución del primer dato encontrado.

**Por qué:** Buscar devuelve una respuesta; modificar produce un cambio persistente dentro del mismo objeto.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 12
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ04_BuscarModificarDoble {
    static class NodoDoble {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ04_BuscarModificarDoble {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoDoble {`: Definimos el tipo de nodo para agrupar un dato y dos referencias del MISMO tipo. Es estática y anidada para que el ejemplo funcione dentro de un solo archivo independiente.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
```


### Explicación de cada instrucción funcional
- `NodoDoble anterior, siguiente;`: Se declaran dos referencias separadas, anterior y siguiente, ambas de tipo NodoDoble. Los extremos de la lista tendrán una de ellas en null.
- `NodoDoble(int dato) { this.dato = dato; }`: El constructor recibe el int que tendrá el nodo al crearlo. El parámetro dato es local; this.dato designa el campo perteneciente al nuevo objeto.
- `static class ListaDoble {`: La clase administradora conserva referencias a nodos y concentra sus operaciones; separarla de NodoDoble evita que un nodo sea responsable de toda la colección.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble inicio;
        void insertarInicio(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
```


### Explicación de cada instrucción funcional
- `NodoDoble inicio;`: inicio apunta al primer nodo; al principio es null, lo cual representa la lista vacía.
- `void insertarInicio(int dato) {`: El método añade un nuevo primer nodo: antes de cambiar inicio debe conservar el antiguo inicio mediante nuevo.siguiente.
- `NodoDoble nuevo = new NodoDoble(dato);`: nuevo es la referencia local al nodo recién creado. Su dato ya está inicializado; todavía deben ajustarse las referencias para integrarlo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            nuevo.siguiente = inicio;
            if (inicio != null) inicio.anterior = nuevo;
            inicio = nuevo;
```


### Explicación de cada instrucción funcional
- `nuevo.siguiente = inicio;`: Conservamos la dirección del que hasta ahora era el primer nodo, antes de reemplazar inicio.
- `if (inicio != null) inicio.anterior = nuevo;`: La condición separa dos estados posibles de la estructura. Evalúala con un ejemplo concreto y comprueba qué rama evita acceder a una referencia inválida.
- `inicio = nuevo;`: Ahora nuevo pasa a ser el primer nodo. Se hace después de conservar el antiguo inicio en nuevo.siguiente.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        boolean buscar(int dato) {
            NodoDoble actual = inicio;
            while (actual != null) {
```


### Explicación de cada instrucción funcional
- `boolean buscar(int dato) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `NodoDoble actual = inicio;`: El cursor actual comienza en el primer nodo. Es una referencia de trabajo que puede cambiar sin alterar inicio.
- `while (actual != null) {`: El ciclo de lista doble continúa mientras exista un nodo actual; terminará al pasar del último al null de su siguiente/anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                if (actual.dato == dato) return true;
                actual = actual.siguiente;
            }
            return false;
```


### Explicación de cada instrucción funcional
- `if (actual.dato == dato) return true;`: La condición compara el dato del nodo visitado con el argumento de búsqueda; la referencia del nodo no se modifica.
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `return false;`: La operación devuelve false porque no encontró el valor o no pudo realizar el cambio sin violar las condiciones.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        boolean modificar(int antiguo, int nuevo) {
            NodoDoble actual = inicio;
            while (actual != null) {
```


### Explicación de cada instrucción funcional
- `boolean modificar(int antiguo, int nuevo) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `NodoDoble actual = inicio;`: El cursor actual comienza en el primer nodo. Es una referencia de trabajo que puede cambiar sin alterar inicio.
- `while (actual != null) {`: El ciclo de lista doble continúa mientras exista un nodo actual; terminará al pasar del último al null de su siguiente/anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                if (actual.dato == antiguo) {
                    actual.dato = nuevo;
                    return true;
```


### Explicación de cada instrucción funcional
- `if (actual.dato == antiguo) {`: La condición compara el dato del nodo visitado con el argumento de búsqueda; la referencia del nodo no se modifica.
- `actual.dato = nuevo;`: Solo cambia el contenido de un nodo existente; el enlace anterior/siguiente sigue siendo el mismo.
- `return true;`: La operación termina inmediatamente con éxito: encontró, modificó o eliminó el elemento esperado.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                }
                actual = actual.siguiente;
            }
            return false;
        }
    }
    public static void main(String[] args) {
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `return false;`: La operación devuelve false porque no encontró el valor o no pudo realizar el cambio sin violar las condiciones.
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        ListaDoble lista = new ListaDoble();
        lista.insertarInicio(30);
        lista.insertarInicio(20);
```


### Explicación de cada instrucción funcional
- `ListaDoble lista = new ListaDoble();`: Se crea la colección sobre la que actuarán los métodos. Una lista nueva comienza con sus referencias a extremos en null.
- `lista.insertarInicio(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarInicio(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarInicio(10);
        System.out.println("Existe 20: " + lista.buscar(20));
        System.out.println("Modificado: " + lista.modificar(20, 25));
```


### Explicación de cada instrucción funcional
- `lista.insertarInicio(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `System.out.println("Existe 20: " + lista.buscar(20));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Modificado: " + lista.modificar(20, 25));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Existe 20: " + lista.buscar(20));
        System.out.println("Existe 25: " + lista.buscar(25));
    }
}
```


### Explicación de cada instrucción funcional
- `System.out.println("Existe 20: " + lista.buscar(20));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Existe 25: " + lista.buscar(25));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Comparamos 10 y luego 20. Se devuelve true. Al modificar 20, su campo dato se cambia a 25; las referencias anterior/siguiente no se tocan.

**Antes / acción / después:** `10 <-> 20 <-> 30` → operación del programa → `10 <-> 25 <-> 30 (misma topología)`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ04
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ04_BuscarModificarDoble {
    static class NodoDoble {
        int dato;
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
        NodoDoble inicio;
        void insertarInicio(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            nuevo.siguiente = inicio;
            if (inicio != null) inicio.anterior = nuevo;
            inicio = nuevo;
        }
        boolean buscar(int dato) {
            NodoDoble actual = inicio;
            while (actual != null) {
                if (actual.dato == dato) return true;
                actual = actual.siguiente;
            }
            return false;
        }
        boolean modificar(int antiguo, int nuevo) {
            NodoDoble actual = inicio;
            while (actual != null) {
                if (actual.dato == antiguo) {
                    actual.dato = nuevo;
                    return true;
                }
                actual = actual.siguiente;
            }
            return false;
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarInicio(30);
        lista.insertarInicio(20);
        lista.insertarInicio(10);
        System.out.println("Existe 20: " + lista.buscar(20));
        System.out.println("Modificado: " + lista.modificar(20, 25));
        System.out.println("Existe 20: " + lista.buscar(20));
        System.out.println("Existe 25: " + lista.buscar(25));
    }
}
```


## Predicción ANTES de ejecutar
¿buscar(20) será true antes del cambio y false después? ¿buscar(25) será true?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ04_BuscarModificarDoble.java
java EJ04_BuscarModificarDoble
```

**Salida observada en validación:**

```text
Existe 20: true
Modificado: true
Existe 20: false
Existe 25: true
```


## Interpretación
Comparamos 10 y luego 20. Se devuelve true. Al modificar 20, su campo dato se cambia a 25; las referencias anterior/siguiente no se tocan.

Este comportamiento responde a la decisión previa: Buscar devuelve una respuesta; modificar produce un cambio persistente dentro del mismo objeto.

## Variación y error controlado
**Variación A:** Busca un valor ausente 90 y explica cuándo termina el while.

**Error o caso límite:** Confundir `actual = actual.siguiente` con `actual.siguiente = ...` altera enlaces en lugar de mover el cursor. Solo avanza la variable local actual.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Qué cambia después de modificar? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T04 — Tarea espejo: Devolver posición de un valor
**Relación directa:** EJ04 ↔ T04.

**Enunciado:** Reemplaza la búsqueda booleana por `posicionDe(int dato)` con posiciones desde 0 y devuelve -1 si no existe.

**Archivo a crear/modificar:** `T04_PosicionDoble.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** Recorre con referencias, sin convertir la lista a un arreglo.

**Pista:** Crea contador posicion = 0 y aumenta después de avanzar.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Posicion 20: 1, Posicion 99: -1, Posicion 25: 1 tras modificación.

**Cómo validarla:** Valor hallado y ausente devuelven índices correctos y el método termina.

**Comandos:** `javac -encoding UTF-8 T04_PosicionDoble.java` y `java T04_PosicionDoble`. La clase pública en el código debe llamarse `T04_PosicionDoble`.


---
# EJ05 — Ordenar una lista doble intercambiando datos
**Contenido:** Ordenamiento burbuja y preservación de enlaces bidireccionales. **Nivel:** Análisis.

## Qué vamos a construir y qué aprenderás
Los nodos pueden estar conectados correctamente pero contener valores desordenados. Al terminar, sabrás justificar este diseño: Dos bucles anidados comparan vecinos. El temporal evita sobrescribir uno de los valores durante el intercambio.

## Archivos y punto de partida
Archivo nuevo: **`EJ05_OrdenarDoble.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `5 <-> 1 <-> 8 <-> 3`.

**Estado DESPUÉS:** `1 <-> 3 <-> 5 <-> 8 (mismos nodos)`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Ordenamiento burbuja y preservación de enlaces bidireccionales.

**Por qué:** Dos bucles anidados comparan vecinos. El temporal evita sobrescribir uno de los valores durante el intercambio.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 12
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ05_OrdenarDoble {
    static class NodoDoble {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ05_OrdenarDoble {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoDoble {`: Definimos el tipo de nodo para agrupar un dato y dos referencias del MISMO tipo. Es estática y anidada para que el ejemplo funcione dentro de un solo archivo independiente.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
```


### Explicación de cada instrucción funcional
- `NodoDoble anterior, siguiente;`: Se declaran dos referencias separadas, anterior y siguiente, ambas de tipo NodoDoble. Los extremos de la lista tendrán una de ellas en null.
- `NodoDoble(int dato) { this.dato = dato; }`: El constructor recibe el int que tendrá el nodo al crearlo. El parámetro dato es local; this.dato designa el campo perteneciente al nuevo objeto.
- `static class ListaDoble {`: La clase administradora conserva referencias a nodos y concentra sus operaciones; separarla de NodoDoble evita que un nodo sea responsable de toda la colección.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoDoble inicio;
        void insertarInicio(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
```


### Explicación de cada instrucción funcional
- `NodoDoble inicio;`: inicio apunta al primer nodo; al principio es null, lo cual representa la lista vacía.
- `void insertarInicio(int dato) {`: El método añade un nuevo primer nodo: antes de cambiar inicio debe conservar el antiguo inicio mediante nuevo.siguiente.
- `NodoDoble nuevo = new NodoDoble(dato);`: nuevo es la referencia local al nodo recién creado. Su dato ya está inicializado; todavía deben ajustarse las referencias para integrarlo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            nuevo.siguiente = inicio;
            if (inicio != null) inicio.anterior = nuevo;
            inicio = nuevo;
```


### Explicación de cada instrucción funcional
- `nuevo.siguiente = inicio;`: Conservamos la dirección del que hasta ahora era el primer nodo, antes de reemplazar inicio.
- `if (inicio != null) inicio.anterior = nuevo;`: La condición separa dos estados posibles de la estructura. Evalúala con un ejemplo concreto y comprueba qué rama evita acceder a una referencia inválida.
- `inicio = nuevo;`: Ahora nuevo pasa a ser el primer nodo. Se hace después de conservar el antiguo inicio en nuevo.siguiente.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        void ordenar() {
            if (inicio == null) return;
            for (NodoDoble i = inicio; i.siguiente != null; i = i.siguiente) {
```


### Explicación de cada instrucción funcional
- `void ordenar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (inicio == null) return;`: Esta condición separa lista vacía de lista existente: evita acceder a referencias inexistentes y determina quién será el primer nodo.
- `for (NodoDoble i = inicio; i.siguiente != null; i = i.siguiente) {`: Este for recorre referencias de nodos dobles mediante el campo siguiente; la condición deja de cumplirse al alcanzar el extremo correspondiente.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                for (NodoDoble j = inicio; j.siguiente != null; j = j.siguiente) {
                    if (j.dato > j.siguiente.dato) {
                        int temporal = j.dato;
```


### Explicación de cada instrucción funcional
- `for (NodoDoble j = inicio; j.siguiente != null; j = j.siguiente) {`: Este for recorre referencias de nodos dobles mediante el campo siguiente; la condición deja de cumplirse al alcanzar el extremo correspondiente.
- `if (j.dato > j.siguiente.dato) {`: Dos datos contiguos están invertidos para orden ascendente si el de la izquierda es mayor; intercambiamos solo sus valores.
- `int temporal = j.dato;`: temporal guarda el primer dato antes de sobrescribirlo. Es imprescindible para un intercambio sin pérdida de información.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                        j.dato = j.siguiente.dato;
                        j.siguiente.dato = temporal;
                    }
                }
            }
        }
        void mostrar() {
```


### Explicación de cada instrucción funcional
- `j.dato = j.siguiente.dato;`: Se copia el dato derecho en el nodo izquierdo; el valor izquierdo original permanece seguro en temporal.
- `j.siguiente.dato = temporal;`: Se completa el intercambio recuperando el dato previamente guardado; las referencias de la lista no cambian.
- `void mostrar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            for (NodoDoble actual = inicio; actual != null; actual = actual.siguiente)
                System.out.print(actual.dato + " ");
            System.out.println();
```


### Explicación de cada instrucción funcional
- `for (NodoDoble actual = inicio; actual != null; actual = actual.siguiente)`: Este for recorre referencias de nodos dobles mediante el campo siguiente; la condición deja de cumplirse al alcanzar el extremo correspondiente.
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarInicio(3);
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaDoble lista = new ListaDoble();`: Se crea la colección sobre la que actuarán los métodos. Una lista nueva comienza con sus referencias a extremos en null.
- `lista.insertarInicio(3);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarInicio(8);
        lista.insertarInicio(1);
        lista.insertarInicio(5);
```


### Explicación de cada instrucción funcional
- `lista.insertarInicio(8);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarInicio(1);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarInicio(5);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.mostrar();
        lista.ordenar();
        lista.mostrar();
```


### Explicación de cada instrucción funcional
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.ordenar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 12
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
    }
}
```


### Explicación de cada instrucción funcional
- Las llaves de cierre completan el alcance del método o condición ya explicado. No cambian por sí solas datos ni referencias.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Pasada inicial: 5 y 1 intercambian; 8 y 3 intercambian. Pasadas siguientes eliminan inversiones hasta alcanzar 1,3,5,8. El código intercambia dato, no referencias.

**Antes / acción / después:** `5 <-> 1 <-> 8 <-> 3` → operación del programa → `1 <-> 3 <-> 5 <-> 8 (mismos nodos)`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ05
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ05_OrdenarDoble {
    static class NodoDoble {
        int dato;
        NodoDoble anterior, siguiente;
        NodoDoble(int dato) { this.dato = dato; }
    }
    static class ListaDoble {
        NodoDoble inicio;
        void insertarInicio(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            nuevo.siguiente = inicio;
            if (inicio != null) inicio.anterior = nuevo;
            inicio = nuevo;
        }
        void ordenar() {
            if (inicio == null) return;
            for (NodoDoble i = inicio; i.siguiente != null; i = i.siguiente) {
                for (NodoDoble j = inicio; j.siguiente != null; j = j.siguiente) {
                    if (j.dato > j.siguiente.dato) {
                        int temporal = j.dato;
                        j.dato = j.siguiente.dato;
                        j.siguiente.dato = temporal;
                    }
                }
            }
        }
        void mostrar() {
            for (NodoDoble actual = inicio; actual != null; actual = actual.siguiente)
                System.out.print(actual.dato + " ");
            System.out.println();
        }
    }
    public static void main(String[] args) {
        ListaDoble lista = new ListaDoble();
        lista.insertarInicio(3);
        lista.insertarInicio(8);
        lista.insertarInicio(1);
        lista.insertarInicio(5);
        lista.mostrar();
        lista.ordenar();
        lista.mostrar();
    }
}
```


## Predicción ANTES de ejecutar
¿El primer número después de ordenar será 1? ¿Por qué el último será 8?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ05_OrdenarDoble.java
java EJ05_OrdenarDoble
```

**Salida observada en validación:**

```text
5 1 8 3 
1 3 5 8
```


## Interpretación
Pasada inicial: 5 y 1 intercambian; 8 y 3 intercambian. Pasadas siguientes eliminan inversiones hasta alcanzar 1,3,5,8. El código intercambia dato, no referencias.

Este comportamiento responde a la decisión previa: Dos bucles anidados comparan vecinos. El temporal evita sobrescribir uno de los valores durante el intercambio.

## Variación y error controlado
**Variación A:** Prueba una lista ya ordenada y comprueba que la salida permanece igual.

**Error o caso límite:** Si inviertes valores sin usar temporal, puedes perder uno de ellos. El orden correcto es guardar, sustituir y recuperar.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Qué se conserva durante el ordenamiento? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T05 — Tarea espejo: Burbuja doble descendente
**Relación directa:** EJ05 ↔ T05.

**Enunciado:** Modifica la condición de comparación para ordenar 5,1,8,3 de mayor a menor.

**Archivo a crear/modificar:** `T05_OrdenDescDoble.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** No cambies anterior/siguiente ni uses Arrays.sort.

**Pista:** Para descendente intercambia cuando el dato izquierdo es menor que el derecho.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Salida final 8 5 3 1.

**Cómo validarla:** La secuencia es descendente y conserva cuatro nodos.

**Comandos:** `javac -encoding UTF-8 T05_OrdenDescDoble.java` y `java T05_OrdenDescDoble`. La clase pública en el código debe llamarse `T05_OrdenDescDoble`.


---
# EJ06 — Construir la lista circular e insertar al final en O(1)
**Contenido:** NodoCircular, referencia ultimo y cierre ultimo.siguiente → primero. **Nivel:** Fundamento.

## Qué vamos a construir y qué aprenderás
El último nodo debe volver al primero; una inserción no debe dejar la cadena abierta. Al terminar, sabrás justificar este diseño: Guardar ultimo permite encontrar el primero mediante ultimo.siguiente sin recorrer la lista.

## Archivos y punto de partida
Archivo nuevo: **`EJ06_InsertarCircular.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `lista vacía: ultimo = null`.

**Estado DESPUÉS:** `ultimo=[30]; [10] → [20] → [30] ↩ [10]`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** NodoCircular, referencia ultimo y cierre ultimo.siguiente → primero.

**Por qué:** Guardar ultimo permite encontrar el primero mediante ultimo.siguiente sin recorrer la lista.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 9
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ06_InsertarCircular {
    static class NodoCircular {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ06_InsertarCircular {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoCircular {`: Definimos el tipo de nodo circular: guarda un dato y una referencia al siguiente nodo de su mismo tipo. La circularidad no aparece por declarar el campo; debe establecerse enlazando los objetos.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
```


### Explicación de cada instrucción funcional
- `NodoCircular siguiente;`: La referencia siguiente enlaza el nodo circular con otro nodo; debe apuntar al primero cuando se trata del último nodo de una lista circular no vacía.
- `NodoCircular(int dato) { this.dato = dato; }`: El constructor da un valor inicial al campo dato. La referencia siguiente se enlazará al insertar; crear un objeto no cierra por sí solo el ciclo.
- `static class ListaCircular {`: La clase administradora mantiene el estado de la estructura. Usa la referencia ultimo para conocer el primero mediante ultimo.siguiente cuando hay nodos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
```


### Explicación de cada instrucción funcional
- `NodoCircular ultimo;`: ultimo guarda la referencia al nodo final; es null si la lista está vacía y su siguiente referencia el nodo inicial cuando hay elementos.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoCircular nuevo = new NodoCircular(dato);`: nuevo es un nodo circular recién creado. Para integrarlo hace falta enlazar su siguiente y el siguiente del nodo anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (ultimo == null) {
                ultimo = nuevo;
                ultimo.siguiente = ultimo;
```


### Explicación de cada instrucción funcional
- `if (ultimo == null) {`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `ultimo.siguiente = ultimo;`: El único nodo se enlaza consigo mismo: el cierre del ciclo existe desde la primera inserción.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            } else {
                nuevo.siguiente = ultimo.siguiente;
                ultimo.siguiente = nuevo;
                ultimo = nuevo;
```


### Explicación de cada instrucción funcional
- `nuevo.siguiente = ultimo.siguiente;`: Antes de redirigir el último, se conserva el antiguo primero como siguiente del nuevo nodo; sin este orden se podría cortar el ciclo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            }
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(10);
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaCircular lista = new ListaCircular();`: Se construye la colección circular vacía, cuyo campo ultimo inicialmente vale null.
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        System.out.println("Primero: " + lista.ultimo.siguiente.dato);
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `System.out.println("Primero: " + lista.ultimo.siguiente.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Ultimo: " + lista.ultimo.dato);
        System.out.println("Cierra ciclo: " +
                (lista.ultimo.siguiente.siguiente.siguiente.siguiente == lista.ultimo.siguiente));
```


### Explicación de cada instrucción funcional
- `System.out.println("Ultimo: " + lista.ultimo.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Cierra ciclo: " +`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `(lista.ultimo.siguiente.siguiente.siguiente.siguiente == lista.ultimo.siguiente));`: La asignación cambia el estado del identificador de la izquierda usando la expresión de la derecha. Distingue si modifica un dato, una referencia entre nodos o una variable local de control.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 9
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
    }
}
```


### Explicación de cada instrucción funcional
- Las llaves de cierre completan el alcance del método o condición ya explicado. No cambian por sí solas datos ni referencias.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Primer nodo 10: se apunta a sí mismo. Segundo 20: 20.siguiente=10, 10.siguiente=20, ultimo=20. Tercero 30: se intercala después de 20 y ultimo=30. Hay tres nodos en el ciclo.

**Antes / acción / después:** `lista vacía: ultimo = null` → operación del programa → `ultimo=[30]; [10] → [20] → [30] ↩ [10]`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ06
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ06_InsertarCircular {
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (ultimo == null) {
                ultimo = nuevo;
                ultimo.siguiente = ultimo;
            } else {
                nuevo.siguiente = ultimo.siguiente;
                ultimo.siguiente = nuevo;
                ultimo = nuevo;
            }
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        System.out.println("Primero: " + lista.ultimo.siguiente.dato);
        System.out.println("Ultimo: " + lista.ultimo.dato);
        System.out.println("Cierra ciclo: " +
                (lista.ultimo.siguiente.siguiente.siguiente.siguiente == lista.ultimo.siguiente));
    }
}
```


## Predicción ANTES de ejecutar
¿Qué valores dan ultimo.dato y ultimo.siguiente.dato?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ06_InsertarCircular.java
java EJ06_InsertarCircular
```

**Salida observada en validación:**

```text
Primero: 10
Ultimo: 30
Cierra ciclo: true
```


## Interpretación
Primer nodo 10: se apunta a sí mismo. Segundo 20: 20.siguiente=10, 10.siguiente=20, ultimo=20. Tercero 30: se intercala después de 20 y ultimo=30. Hay tres nodos en el ciclo.

Este comportamiento responde a la decisión previa: Guardar ultimo permite encontrar el primero mediante ultimo.siguiente sin recorrer la lista.

## Variación y error controlado
**Variación A:** Agrega un cuarto nodo y calcula el número correcto de saltos para volver al primero.

**Error o caso límite:** Si olvidas `nuevo.siguiente = ultimo.siguiente` perderás el vínculo con el primer nodo. Asigna el enlace de nuevo antes de redirigir ultimo.siguiente.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Por qué una lista vacía sí puede tener ultimo == null? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T06 — Tarea espejo: Insertar después de un valor específico
**Relación directa:** EJ06 ↔ T06.

**Enunciado:** Implementa `insertarDespues(valor, datoNuevo)` para insertar 25 después de 20 y 40 después del último 30.

**Archivo a crear/modificar:** `T06_InsertarDespuesCircular.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** Conserva el ciclo y actualiza ultimo cuando la inserción se haga después de ultimo.

**Pista:** Guarda el siguiente antiguo en nuevo.siguiente antes de conectar actual.siguiente.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Ambas inserciones true, Primero: 10, Ultimo: 40, Cierra ciclo: true.

**Cómo validarla:** El recorrido completo da 10→20→25→30→40→10.

**Comandos:** `javac -encoding UTF-8 T06_InsertarDespuesCircular.java` y `java T06_InsertarDespuesCircular`. La clase pública en el código debe llamarse `T06_InsertarDespuesCircular`.


---
# EJ07 — Recorrer y contar nodos de una lista circular
**Contenido:** do-while, parada al regresar al primero y control de lista vacía. **Nivel:** Aplicación.

## Qué vamos a construir y qué aprenderás
En una lista circular conectada, un `while(actual != null)` no termina. Necesitamos reconocer el regreso al primer nodo. Al terminar, sabrás justificar este diseño: do-while procesa al menos un nodo cuando la lista no está vacía; el if anterior protege la lista vacía.

## Archivos y punto de partida
Archivo nuevo: **`EJ07_RecorrerCircular.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `ultimo=[30], primero=[10]`.

**Estado DESPUÉS:** `visitas: 10, 20, 30; al volver a 10, detener`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** do-while, parada al regresar al primero y control de lista vacía.

**Por qué:** do-while procesa al menos un nodo cuando la lista no está vacía; el if anterior protege la lista vacía.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 13
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ07_RecorrerCircular {
    static class NodoCircular {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ07_RecorrerCircular {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoCircular {`: Definimos el tipo de nodo circular: guarda un dato y una referencia al siguiente nodo de su mismo tipo. La circularidad no aparece por declarar el campo; debe establecerse enlazando los objetos.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
```


### Explicación de cada instrucción funcional
- `NodoCircular siguiente;`: La referencia siguiente enlaza el nodo circular con otro nodo; debe apuntar al primero cuando se trata del último nodo de una lista circular no vacía.
- `NodoCircular(int dato) { this.dato = dato; }`: El constructor da un valor inicial al campo dato. La referencia siguiente se enlazará al insertar; crear un objeto no cierra por sí solo el ciclo.
- `static class ListaCircular {`: La clase administradora mantiene el estado de la estructura. Usa la referencia ultimo para conocer el primero mediante ultimo.siguiente cuando hay nodos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
```


### Explicación de cada instrucción funcional
- `NodoCircular ultimo;`: ultimo guarda la referencia al nodo final; es null si la lista está vacía y su siguiente referencia el nodo inicial cuando hay elementos.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoCircular nuevo = new NodoCircular(dato);`: nuevo es un nodo circular recién creado. Para integrarlo hace falta enlazar su siguiente y el siguiente del nodo anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
```


### Explicación de cada instrucción funcional
- `if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `nuevo.siguiente = ultimo.siguiente;`: Antes de redirigir el último, se conserva el antiguo primero como siguiente del nuevo nodo; sin este orden se podría cortar el ciclo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            ultimo = nuevo;
        }
        void mostrar() {
            if (ultimo == null) { System.out.println("(vacia)"); return; }
```


### Explicación de cada instrucción funcional
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `void mostrar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) { System.out.println("(vacia)"); return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            NodoCircular actual = ultimo.siguiente;
            do {
                System.out.print(actual.dato + " ");
```


### Explicación de cada instrucción funcional
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            System.out.println();
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        int contar() {
            if (ultimo == null) return 0;
            NodoCircular actual = ultimo.siguiente;
```


### Explicación de cada instrucción funcional
- `int contar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) return 0;`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            int cantidad = 0;
            do {
                cantidad++;
```


### Explicación de cada instrucción funcional
- `int cantidad = 0;`: El contador empieza en cero, se incrementa al visitar cada nodo y luego devuelve la cantidad de nodos recorridos.
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.
- `cantidad++;`: Se incrementa una unidad por cada nodo visitado una sola vez dentro del do-while.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return cantidad;
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.
- `return cantidad;`: Devuelve el número de nodos efectivamente visitados, no el valor de un nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.mostrar();
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaCircular lista = new ListaCircular();`: Se construye la colección circular vacía, cuyo campo ultimo inicialmente vale null.
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 14 — Microbloque 13 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.mostrar();
        System.out.println("Cantidad: " + lista.contar());
    }
}
```


### Explicación de cada instrucción funcional
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `System.out.println("Cantidad: " + lista.contar());`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
actual inicia en 10. Iteración 1 imprime 10 y avanza a 20. Iteración 2 imprime 20 y avanza a 30. Iteración 3 imprime 30 y avanza a 10. Al recuperar la referencia inicial termina.

**Antes / acción / después:** `ultimo=[30], primero=[10]` → operación del programa → `visitas: 10, 20, 30; al volver a 10, detener`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ07
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ07_RecorrerCircular {
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
            ultimo = nuevo;
        }
        void mostrar() {
            if (ultimo == null) { System.out.println("(vacia)"); return; }
            NodoCircular actual = ultimo.siguiente;
            do {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            System.out.println();
        }
        int contar() {
            if (ultimo == null) return 0;
            NodoCircular actual = ultimo.siguiente;
            int cantidad = 0;
            do {
                cantidad++;
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return cantidad;
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.mostrar();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.mostrar();
        System.out.println("Cantidad: " + lista.contar());
    }
}
```


## Predicción ANTES de ejecutar
¿Qué muestra antes de insertar datos? ¿Qué cantidad devuelve con tres nodos?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ07_RecorrerCircular.java
java EJ07_RecorrerCircular
```

**Salida observada en validación:**

```text
(vacia)
10 20 30 
Cantidad: 3
```


## Interpretación
actual inicia en 10. Iteración 1 imprime 10 y avanza a 20. Iteración 2 imprime 20 y avanza a 30. Iteración 3 imprime 30 y avanza a 10. Al recuperar la referencia inicial termina.

Este comportamiento responde a la decisión previa: do-while procesa al menos un nodo cuando la lista no está vacía; el if anterior protege la lista vacía.

## Variación y error controlado
**Variación A:** Ensaya un solo nodo: el ciclo ejecuta exactamente una iteración.

**Error o caso límite:** Reemplazar la condición por actual != null provocaría un recorrido que no termina. Usa la igualdad con la referencia al primero.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Cuándo deja de ejecutar el do-while? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T07 — Tarea espejo: Contar elementos mayores a un límite
**Relación directa:** EJ07 ↔ T07.

**Enunciado:** Crea contarMayoresA(int limite) sobre la misma lista circular: cuenta valores estrictamente mayores que 15.

**Archivo a crear/modificar:** `T07_ContarMayoresCircular.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** Visita cada nodo como máximo una vez y devuelve cero si la lista está vacía.

**Pista:** Reutiliza do-while y aumenta cantidad solo dentro de if (actual.dato > limite).

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Con 10,20,30 imprime Mayores a 15: 2.

**Cómo validarla:** El método devuelve 2, no repite nodos y termina.

**Comandos:** `javac -encoding UTF-8 T07_ContarMayoresCircular.java` y `java T07_ContarMayoresCircular`. La clase pública en el código debe llamarse `T07_ContarMayoresCircular`.


---
# EJ08 — Eliminar de una lista circular sin romper el ciclo
**Contenido:** Eliminación circular: vacía, único, primer, intermedio, último. **Nivel:** Análisis.

## Qué vamos a construir y qué aprenderás
No existen extremos enlazados a null; hay que reconfigurar el predecesor y, si procede, el último. Al terminar, sabrás justificar este diseño: El caso único necesita anular ultimo para representar la lista vacía; quitar ultimo de una lista múltiple exige apuntarlo a su anterior.

## Archivos y punto de partida
Archivo nuevo: **`EJ08_EliminarCircular.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `10 → 20 → 30 ↩ 10`.

**Estado DESPUÉS:** `eliminar 20: 10 → 30 ↩ 10; eliminar 30: 10 ↩ 10`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Eliminación circular: vacía, único, primer, intermedio, último.

**Por qué:** El caso único necesita anular ultimo para representar la lista vacía; quitar ultimo de una lista múltiple exige apuntarlo a su anterior.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 13
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ08_EliminarCircular {
    static class NodoCircular {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ08_EliminarCircular {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoCircular {`: Definimos el tipo de nodo circular: guarda un dato y una referencia al siguiente nodo de su mismo tipo. La circularidad no aparece por declarar el campo; debe establecerse enlazando los objetos.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
```


### Explicación de cada instrucción funcional
- `NodoCircular siguiente;`: La referencia siguiente enlaza el nodo circular con otro nodo; debe apuntar al primero cuando se trata del último nodo de una lista circular no vacía.
- `NodoCircular(int dato) { this.dato = dato; }`: El constructor da un valor inicial al campo dato. La referencia siguiente se enlazará al insertar; crear un objeto no cierra por sí solo el ciclo.
- `static class ListaCircular {`: La clase administradora mantiene el estado de la estructura. Usa la referencia ultimo para conocer el primero mediante ultimo.siguiente cuando hay nodos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
```


### Explicación de cada instrucción funcional
- `NodoCircular ultimo;`: ultimo guarda la referencia al nodo final; es null si la lista está vacía y su siguiente referencia el nodo inicial cuando hay elementos.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoCircular nuevo = new NodoCircular(dato);`: nuevo es un nodo circular recién creado. Para integrarlo hace falta enlazar su siguiente y el siguiente del nodo anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
```


### Explicación de cada instrucción funcional
- `if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `nuevo.siguiente = ultimo.siguiente;`: Antes de redirigir el último, se conserva el antiguo primero como siguiente del nuevo nodo; sin este orden se podría cortar el ciclo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            ultimo = nuevo;
        }
        boolean eliminar(int dato) {
            if (ultimo == null) return false;
```


### Explicación de cada instrucción funcional
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `boolean eliminar(int dato) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) return false;`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el anterior y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            NodoCircular anterior = ultimo;
            NodoCircular actual = ultimo.siguiente;
            do {
```


### Explicación de cada instrucción funcional
- `NodoCircular anterior = ultimo;`: El predecesor del primer nodo circular es el último; iniciar allí permite borrar el primero con la misma lógica de reconexión.
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                if (actual.dato == dato) {
                    if (actual == ultimo && actual == ultimo.siguiente) {
                        ultimo = null;
```


### Explicación de cada instrucción funcional
- `if (actual.dato == dato) {`: La condición compara el dato del nodo visitado con el argumento de búsqueda; la referencia del nodo no se modifica.
- `if (actual == ultimo && actual == ultimo.siguiente) {`: Solo puede cumplirse cuando ultimo es también el primero: la lista contiene exactamente un nodo, que al borrarse deja la lista vacía.
- `ultimo = null;`: La lista queda vacía tras eliminar el único nodo. No hay primer nodo que pueda obtenerse mediante ultimo.siguiente.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                    } else {
                        anterior.siguiente = actual.siguiente;
                        if (actual == ultimo) ultimo = anterior;
                    }
                    return true;
```


### Explicación de cada instrucción funcional
- `anterior.siguiente = actual.siguiente;`: El nodo anterior pasa a enlazar con el sucesor de actual. Esta operación lo saca del anillo sin perder el cierre del ciclo.
- `if (actual == ultimo) ultimo = anterior;`: Se comprueba si el nodo eliminado era ultimo para corregir esa referencia: debe apuntar al predecesor que permanece en la lista.
- `return true;`: La operación termina inmediatamente con éxito: encontró, modificó o eliminó el elemento esperado.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el anterior y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                }
                anterior = actual;
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
```


### Explicación de cada instrucción funcional
- `anterior = actual;`: Antes de avanzar, se recuerda el nodo visitado como predecesor para poder saltar o reconectar el siguiente.
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            return false;
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
```


### Explicación de cada instrucción funcional
- `return false;`: La operación devuelve false porque no encontró el valor o no pudo realizar el cambio sin violar las condiciones.
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaCircular lista = new ListaCircular();`: Se construye la colección circular vacía, cuyo campo ultimo inicialmente vale null.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Elimina 20: " + lista.eliminar(20));
        System.out.println("Primero: " + lista.ultimo.siguiente.dato);
        System.out.println("Elimina 30: " + lista.eliminar(30));
```


### Explicación de cada instrucción funcional
- `System.out.println("Elimina 20: " + lista.eliminar(20));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Primero: " + lista.ultimo.siguiente.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Elimina 30: " + lista.eliminar(30));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 14 — Microbloque 13 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Ultimo: " + lista.ultimo.dato);
    }
}
```


### Explicación de cada instrucción funcional
- `System.out.println("Ultimo: " + lista.ultimo.dato);`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
Empieza anterior=30 y actual=10. Avanza hasta 20. Redirige 10.siguiente a 30. Para borrar 30, redirige 10.siguiente a 10 y actualiza ultimo=10.

**Antes / acción / después:** `10 → 20 → 30 ↩ 10` → operación del programa → `eliminar 20: 10 → 30 ↩ 10; eliminar 30: 10 ↩ 10`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ08
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ08_EliminarCircular {
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
            ultimo = nuevo;
        }
        boolean eliminar(int dato) {
            if (ultimo == null) return false;
            NodoCircular anterior = ultimo;
            NodoCircular actual = ultimo.siguiente;
            do {
                if (actual.dato == dato) {
                    if (actual == ultimo && actual == ultimo.siguiente) {
                        ultimo = null;
                    } else {
                        anterior.siguiente = actual.siguiente;
                        if (actual == ultimo) ultimo = anterior;
                    }
                    return true;
                }
                anterior = actual;
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return false;
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        System.out.println("Elimina 20: " + lista.eliminar(20));
        System.out.println("Primero: " + lista.ultimo.siguiente.dato);
        System.out.println("Elimina 30: " + lista.eliminar(30));
        System.out.println("Ultimo: " + lista.ultimo.dato);
    }
}
```


## Predicción ANTES de ejecutar
¿Quién será ultimo después de eliminar 30? ¿Qué ocurre si intentas eliminar un valor ausente?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ08_EliminarCircular.java
java EJ08_EliminarCircular
```

**Salida observada en validación:**

```text
Elimina 20: true
Primero: 10
Elimina 30: true
Ultimo: 10
```


## Interpretación
Empieza anterior=30 y actual=10. Avanza hasta 20. Redirige 10.siguiente a 30. Para borrar 30, redirige 10.siguiente a 10 y actualiza ultimo=10.

Este comportamiento responde a la decisión previa: El caso único necesita anular ultimo para representar la lista vacía; quitar ultimo de una lista múltiple exige apuntarlo a su anterior.

## Variación y error controlado
**Variación A:** Verifica que un nodo único puede eliminarse y deja ultimo=null.

**Error o caso límite:** Si eliminas ultimo sin actualizarlo, ultimo queda apuntando a un nodo ya retirado. Actualiza ultimo=anterior cuando actual era ultimo.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Por qué empezamos anterior=ultimo? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T08 — Tarea espejo: Batería de casos extremos para eliminar circular
**Relación directa:** EJ08 ↔ T08.

**Enunciado:** En main, prueba lista vacía, único nodo, primer nodo, último nodo e inexistente, en ese orden.

**Archivo a crear/modificar:** `T08_CasosEliminarCircular.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** No cambies la lógica de eliminar; solo prepara estados y comprueba sus resultados.

**Pista:** Tras eliminar el único nodo, valida que ultimo==null antes de reinsertar.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Vacia elimina 8: false; Unico elimina 10: true; Esta vacia: true; Nodo restante: 20.

**Cómo validarla:** Los cinco casos finalizan y ningún acceso inválido produce excepción.

**Comandos:** `javac -encoding UTF-8 T08_CasosEliminarCircular.java` y `java T08_CasosEliminarCircular`. La clase pública en el código debe llamarse `T08_CasosEliminarCircular`.


---
# EJ09 — Buscar y modificar en una lista circular
**Contenido:** Buscar/actualizar con do-while y detener al volver al inicio. **Nivel:** Aplicación.

## Qué vamos a construir y qué aprenderás
La búsqueda circular no tiene null final; debe comprobar el valor y reconocer el regreso al inicio. Al terminar, sabrás justificar este diseño: Ambos métodos recorren con la misma condición; solo modificar cambia el campo dato.

## Archivos y punto de partida
Archivo nuevo: **`EJ09_BuscarModificarCircular.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `10 → 20 → 30 ↩ 10`.

**Estado DESPUÉS:** `10 → 25 → 30 ↩ 10; mismo ciclo`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Buscar/actualizar con do-while y detener al volver al inicio.

**Por qué:** Ambos métodos recorren con la misma condición; solo modificar cambia el campo dato.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 13
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ09_BuscarModificarCircular {
    static class NodoCircular {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ09_BuscarModificarCircular {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoCircular {`: Definimos el tipo de nodo circular: guarda un dato y una referencia al siguiente nodo de su mismo tipo. La circularidad no aparece por declarar el campo; debe establecerse enlazando los objetos.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
```


### Explicación de cada instrucción funcional
- `NodoCircular siguiente;`: La referencia siguiente enlaza el nodo circular con otro nodo; debe apuntar al primero cuando se trata del último nodo de una lista circular no vacía.
- `NodoCircular(int dato) { this.dato = dato; }`: El constructor da un valor inicial al campo dato. La referencia siguiente se enlazará al insertar; crear un objeto no cierra por sí solo el ciclo.
- `static class ListaCircular {`: La clase administradora mantiene el estado de la estructura. Usa la referencia ultimo para conocer el primero mediante ultimo.siguiente cuando hay nodos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
```


### Explicación de cada instrucción funcional
- `NodoCircular ultimo;`: ultimo guarda la referencia al nodo final; es null si la lista está vacía y su siguiente referencia el nodo inicial cuando hay elementos.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoCircular nuevo = new NodoCircular(dato);`: nuevo es un nodo circular recién creado. Para integrarlo hace falta enlazar su siguiente y el siguiente del nodo anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
```


### Explicación de cada instrucción funcional
- `if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `nuevo.siguiente = ultimo.siguiente;`: Antes de redirigir el último, se conserva el antiguo primero como siguiente del nuevo nodo; sin este orden se podría cortar el ciclo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            ultimo = nuevo;
        }
        boolean buscar(int dato) {
            if (ultimo == null) return false;
```


### Explicación de cada instrucción funcional
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `boolean buscar(int dato) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) return false;`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            NodoCircular actual = ultimo.siguiente;
            do {
                if (actual.dato == dato) return true;
```


### Explicación de cada instrucción funcional
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.
- `if (actual.dato == dato) return true;`: La condición compara el dato del nodo visitado con el argumento de búsqueda; la referencia del nodo no se modifica.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return false;
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.
- `return false;`: La operación devuelve false porque no encontró el valor o no pudo realizar el cambio sin violar las condiciones.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
        boolean modificar(int anterior, int nuevo) {
            if (ultimo == null) return false;
            NodoCircular actual = ultimo.siguiente;
```


### Explicación de cada instrucción funcional
- `boolean modificar(int anterior, int nuevo) {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) return false;`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido y los enlaces hacia el anterior. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            do {
                if (actual.dato == anterior) { actual.dato = nuevo; return true; }
                actual = actual.siguiente;
```


### Explicación de cada instrucción funcional
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.
- `if (actual.dato == anterior) { actual.dato = nuevo; return true; }`: La condición compara el dato del nodo visitado con el argumento de búsqueda; la referencia del nodo no se modifica.
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            } while (actual != ultimo.siguiente);
            return false;
        }
    }
    public static void main(String[] args) {
```


### Explicación de cada instrucción funcional
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.
- `return false;`: La operación devuelve false porque no encontró el valor o no pudo realizar el cambio sin violar las condiciones.
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
```


### Explicación de cada instrucción funcional
- `ListaCircular lista = new ListaCircular();`: Se construye la colección circular vacía, cuyo campo ultimo inicialmente vale null.
- `lista.insertarFinal(10);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(20);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(30);
        System.out.println("Busca 20: " + lista.buscar(20));
        System.out.println("Modifica 20: " + lista.modificar(20, 25));
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(30);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `System.out.println("Busca 20: " + lista.buscar(20));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Modifica 20: " + lista.modificar(20, 25));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 14 — Microbloque 13 de 13
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Busca 25: " + lista.buscar(25));
        System.out.println("Busca 20: " + lista.buscar(20));
    }
}
```


### Explicación de cada instrucción funcional
- `System.out.println("Busca 25: " + lista.buscar(25));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.
- `System.out.println("Busca 20: " + lista.buscar(20));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
La búsqueda 20 compara 10, después 20: true. La modificación encuentra 20 y cambia dato a 25. La búsqueda 25 devuelve true; la antigua 20 devuelve false.

**Antes / acción / después:** `10 → 20 → 30 ↩ 10` → operación del programa → `10 → 25 → 30 ↩ 10; mismo ciclo`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ09
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ09_BuscarModificarCircular {
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
            ultimo = nuevo;
        }
        boolean buscar(int dato) {
            if (ultimo == null) return false;
            NodoCircular actual = ultimo.siguiente;
            do {
                if (actual.dato == dato) return true;
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return false;
        }
        boolean modificar(int anterior, int nuevo) {
            if (ultimo == null) return false;
            NodoCircular actual = ultimo.siguiente;
            do {
                if (actual.dato == anterior) { actual.dato = nuevo; return true; }
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            return false;
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        System.out.println("Busca 20: " + lista.buscar(20));
        System.out.println("Modifica 20: " + lista.modificar(20, 25));
        System.out.println("Busca 25: " + lista.buscar(25));
        System.out.println("Busca 20: " + lista.buscar(20));
    }
}
```


## Predicción ANTES de ejecutar
¿Qué cuatro booleanos verás al buscar y modificar 20 y 25?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ09_BuscarModificarCircular.java
java EJ09_BuscarModificarCircular
```

**Salida observada en validación:**

```text
Busca 20: true
Modifica 20: true
Busca 25: true
Busca 20: false
```


## Interpretación
La búsqueda 20 compara 10, después 20: true. La modificación encuentra 20 y cambia dato a 25. La búsqueda 25 devuelve true; la antigua 20 devuelve false.

Este comportamiento responde a la decisión previa: Ambos métodos recorren con la misma condición; solo modificar cambia el campo dato.

## Variación y error controlado
**Variación A:** Prueba un valor inexistente y explica por qué la búsqueda no queda atrapada.

**Error o caso límite:** No avances con actual.siguiente = ...; eso modifica la lista. Avanza con actual = actual.siguiente.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Se cambia la circularidad al modificar dato? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T09 — Tarea espejo: Modificar todos los valores pares de una lista circular
**Relación directa:** EJ09 ↔ T09.

**Enunciado:** Implementa modificarPares() para reemplazar TODOS los valores pares por 0 y devolver la cantidad de cambios.

**Archivo a crear/modificar:** `T09_ParesCircular.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** No devuelvas dentro del if; el recorrido debe continuar hasta completar el ciclo.

**Pista:** Usa `% 2 == 0`, cambia dato, incrementa cambios, continúa.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Cambios: 3; Existe 0: true; Existe 20: false; Existe 30: false.

**Cómo validarla:** Los tres valores se sustituyen y la búsqueda continúa terminando.

**Comandos:** `javac -encoding UTF-8 T09_ParesCircular.java` y `java T09_ParesCircular`. La clase pública en el código debe llamarse `T09_ParesCircular`.


---
# EJ10 — Ordenar una lista circular con pasadas finitas
**Contenido:** Burbuja circular con n-1 pasadas, n-1-pasada comparaciones. **Nivel:** Integración.

## Qué vamos a construir y qué aprenderás
Hay que ordenar los datos sin usar `ultimo.siguiente != ultimo` como condición global: ese enlace se conserva mientras el ciclo existe. Al terminar, sabrás justificar este diseño: El conteo fija un número de iteraciones finito. Cada comparación usa vecinos dentro de la secuencia desde el primero, sin cruzar el cierre último→primero para ordenar.

## Archivos y punto de partida
Archivo nuevo: **`EJ10_OrdenarCircular.java`**. Trabaja en `S07_Listas_Java`, sin pegar clases de ejercicios anteriores. Cada programa es independiente.

**Estado ANTES:** `8 → 3 → 5 → 1 ↩ 8`.

**Estado DESPUÉS:** `1 → 3 → 5 → 8 ↩ 1 (orden ascendente, ciclo conservado)`.

## Paso 1 — Entender antes de construir
**Qué necesitamos:** Burbuja circular con n-1 pasadas, n-1-pasada comparaciones.

**Por qué:** El conteo fija un número de iteraciones finito. Cada comparación usa vecinos dentro de la secuencia desde el primero, sin cruzar el cierre último→primero para ordenar.

**Responsabilidades:** el nodo almacena los datos y enlaces; la clase de lista, cuando aparece, administra extremos, recorridos y cambios. `main` crea un escenario de prueba para comprobar lo construido.

## Paso 2 — Microbloque 1 de 17
### Estado antes
Partimos del archivo vacío.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
public class EJ10_OrdenarCircular {
    static class NodoCircular {
        int dato;
```


### Explicación de cada instrucción funcional
- `public class EJ10_OrdenarCircular {`: La clase pública es el punto de entrada del archivo; Java exige que el nombre coincida con el archivo .java para poder compilarlo sin paquetes adicionales.
- `static class NodoCircular {`: Definimos el tipo de nodo circular: guarda un dato y una referencia al siguiente nodo de su mismo tipo. La circularidad no aparece por declarar el campo; debe establecerse enlazando los objetos.
- `int dato;`: dato es un campo de tipo int que almacena el valor de cada nodo, no la dirección de otro nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 3 — Microbloque 2 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
```


### Explicación de cada instrucción funcional
- `NodoCircular siguiente;`: La referencia siguiente enlaza el nodo circular con otro nodo; debe apuntar al primero cuando se trata del último nodo de una lista circular no vacía.
- `NodoCircular(int dato) { this.dato = dato; }`: El constructor da un valor inicial al campo dato. La referencia siguiente se enlazará al insertar; crear un objeto no cierra por sí solo el ciclo.
- `static class ListaCircular {`: La clase administradora mantiene el estado de la estructura. Usa la referencia ultimo para conocer el primero mediante ultimo.siguiente cuando hay nodos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 4 — Microbloque 3 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo y el contenido de los nodos. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
```


### Explicación de cada instrucción funcional
- `NodoCircular ultimo;`: ultimo guarda la referencia al nodo final; es null si la lista está vacía y su siguiente referencia el nodo inicial cuando hay elementos.
- `void insertarFinal(int dato) {`: Este método concentra la inserción para no repetir conexiones manuales desde main. Su parámetro dato será el valor del nuevo nodo.
- `NodoCircular nuevo = new NodoCircular(dato);`: nuevo es un nodo circular recién creado. Para integrarlo hace falta enlazar su siguiente y el siguiente del nodo anterior.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 5 — Microbloque 4 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
```


### Explicación de cada instrucción funcional
- `if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.
- `nuevo.siguiente = ultimo.siguiente;`: Antes de redirigir el último, se conserva el antiguo primero como siguiente del nuevo nodo; sin este orden se podría cortar el ciclo.
- `ultimo.siguiente = nuevo;`: El último anterior pasa a apuntar a nuevo, que se inserta inmediatamente después en el recorrido de la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 6 — Microbloque 5 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            ultimo = nuevo;
        }
        int contar() {
            if (ultimo == null) return 0;
```


### Explicación de cada instrucción funcional
- `ultimo = nuevo;`: La referencia ultimo avanza al nodo recién insertado; este cambio es seguro después de enlazarlo con los nodos previos.
- `int contar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) return 0;`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 7 — Microbloque 6 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            int n = 0;
            NodoCircular actual = ultimo.siguiente;
            do { n++; actual = actual.siguiente; }
```


### Explicación de cada instrucción funcional
- `int n = 0;`: El contador n empieza en cero y servirá para determinar el número de comparaciones en el ordenamiento.
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.
- `do { n++; actual = actual.siguiente; }`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 8 — Microbloque 7 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            while (actual != ultimo.siguiente);
            return n;
        }
        void ordenar() {
```


### Explicación de cada instrucción funcional
- `while (actual != ultimo.siguiente);`: En el anillo se detiene al regresar a la referencia exacta del primero, no por encontrar null. Evita un recorrido sin fin.
- `return n;`: Devuelve la cantidad de nodos para limitar de forma segura las pasadas de ordenamiento.
- `void ordenar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 9 — Microbloque 8 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            int n = contar();
            for (int pasada = 0; pasada < n - 1; pasada++) {
                NodoCircular actual = ultimo.siguiente;
```


### Explicación de cada instrucción funcional
- `int n = contar();`: n es el número real de nodos, obtenido antes de ordenar; permite acotar los ciclos en una estructura sin final null.
- `for (int pasada = 0; pasada < n - 1; pasada++) {`: pasada cuenta las rondas de ordenamiento desde 0 hasta n-2; ese límite asegura que el algoritmo termina aunque la estructura sea circular.
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 10 — Microbloque 9 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el control del recorrido y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                for (int paso = 0; paso < n - 1 - pasada; paso++) {
                    NodoCircular siguiente = actual.siguiente;
                    if (actual.dato > siguiente.dato) {
```


### Explicación de cada instrucción funcional
- `for (int paso = 0; paso < n - 1 - pasada; paso++) {`: paso controla las comparaciones vecinas en cada pasada, reduciendo una al completarse cada ronda de burbuja.
- `NodoCircular siguiente = actual.siguiente;`: Se guarda explícitamente la referencia al nodo vecino antes de comparar/intercambiar sus datos; el cursor avanza después.
- `if (actual.dato > siguiente.dato) {`: Si el valor actual supera al siguiente se intercambian sus datos. El enlace siguiente sigue apuntando al mismo nodo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 11 — Microbloque 10 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye el contenido de los nodos y los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                        int temporal = actual.dato;
                        actual.dato = siguiente.dato;
                        siguiente.dato = temporal;
```


### Explicación de cada instrucción funcional
- `int temporal = actual.dato;`: temporal guarda el primer dato antes de sobrescribirlo. Es imprescindible para un intercambio sin pérdida de información.
- `actual.dato = siguiente.dato;`: Se copia el dato vecino sobre actual; el valor anterior de actual está protegido en temporal.
- `siguiente.dato = temporal;`: Se completa el intercambio de datos conservando todas las conexiones circulares intactas.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 12 — Microbloque 11 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y la referencia al último nodo. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                    }
                    actual = siguiente;
                }
            }
        }
        void mostrar() {
            if (ultimo == null) { System.out.println("(vacia)"); return; }
```


### Explicación de cada instrucción funcional
- `actual = siguiente;`: La asignación cambia el estado del identificador de la izquierda usando la expresión de la derecha. Distingue si modifica un dato, una referencia entre nodos o una variable local de control.
- `void mostrar() {`: La firma declara una operación con su tipo de resultado, nombre y parámetros. Debe coincidir con el modo en que será invocada desde main; sus instrucciones determinarán el efecto sobre la lista.
- `if (ultimo == null) { System.out.println("(vacia)"); return; }`: Esta condición comprueba si la lista circular todavía no tiene nodos: es el único caso en que el nuevo nodo debe apuntarse a sí mismo.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 13 — Microbloque 12 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente y el control del recorrido. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
            NodoCircular actual = ultimo.siguiente;
            do {
                System.out.print(actual.dato + " ");
```


### Explicación de cada instrucción funcional
- `NodoCircular actual = ultimo.siguiente;`: Si la lista tiene nodos, ultimo.siguiente es el primero. Guardamos esta referencia en el cursor actual para comenzar el recorrido.
- `do {`: do-while procesa el primer nodo antes de evaluar el regreso al inicio. Se usa solo después de verificar la lista no vacía.
- `System.out.print(actual.dato + " ");`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 14 — Microbloque 13 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            System.out.println();
```


### Explicación de cada instrucción funcional
- `actual = actual.siguiente;`: Se desplaza el cursor local al siguiente nodo sin cambiar ninguna referencia de los nodos; es el avance del recorrido.
- `} while (actual != ultimo.siguiente);`: La condición de salida se evalúa después de avanzar: termina cuando el cursor vuelve al primer objeto.
- `System.out.println();`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 15 — Microbloque 14 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(8);
```


### Explicación de cada instrucción funcional
- `public static void main(String[] args) {`: main es el punto que Java ejecuta al iniciar el programa; las instrucciones dentro preparan datos de prueba y hacen observables los resultados.
- `ListaCircular lista = new ListaCircular();`: Se construye la colección circular vacía, cuyo campo ultimo inicialmente vale null.
- `lista.insertarFinal(8);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 16 — Microbloque 15 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.insertarFinal(3);
        lista.insertarFinal(5);
        lista.insertarFinal(1);
```


### Explicación de cada instrucción funcional
- `lista.insertarFinal(3);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(5);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.insertarFinal(1);`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 17 — Microbloque 16 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye una parte necesaria de la clase. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        lista.mostrar();
        lista.ordenar();
        lista.mostrar();
```


### Explicación de cada instrucción funcional
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.ordenar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.
- `lista.mostrar();`: La llamada ejecuta el método sobre la lista ya creada. Observa si modifica su estado o solo inspecciona los datos.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Paso 18 — Microbloque 17 de 17
### Estado antes
Conservamos las instrucciones de los pasos anteriores, sin borrar conexiones ni declaraciones.

### ¿Por qué necesitamos este paso?
Este paso construye los enlaces hacia el siguiente. Se introduce aquí para preparar la siguiente decisión sin perder el estado de la estructura.

### Escribe ahora (añade estas líneas en el orden mostrado)
```java
        System.out.println("Circular: " + (lista.ultimo.siguiente.dato == 1));
    }
}
```


### Explicación de cada instrucción funcional
- `System.out.println("Circular: " + (lista.ultimo.siguiente.dato == 1));`: Este enunciado muestra una evidencia observable de la operación. Verifica la salida concreta para confirmar que las referencias o datos quedaron coherentes.

### Estado después y por qué podemos avanzar
La parte recién escrita ya define las decisiones señaladas; si aparece una referencia, comprueba si aún está en `null`, si apunta a un nodo creado o si se ha actualizado. Conserva el código anterior y continúa: el archivo se considera terminado únicamente al completar el último microbloque.

## Traza manual de los cambios
n=4. Pasada 0: 8/3, 8/5 y 8/1 intercambian; el 8 queda al final. Pasada 1: se acomodan 3 y 1. Pasada 2: 1 y 3; queda 1,3,5,8. Cada pasada tiene menos comparaciones.

**Antes / acción / después:** `8 → 3 → 5 → 1 ↩ 8` → operación del programa → `1 → 3 → 5 → 8 ↩ 1 (orden ascendente, ciclo conservado)`.

**En un recorrido:** identifica el primer nodo, comprueba condición, registra dato, actualiza `actual` y justifica la salida.

## Auditoría antes de ejecutar
| Elemento | Función | Por qué existe | Evidencia |
|---|---|---|---|
| Nodo y campo dato | Almacenan un valor | Distinguen valor de referencia | Lectura del dato |
| Campos de enlace | Relacionan nodos | Conservan estructura | Dibujo anterior/después |
| Condiciones | Separan casos | Protegen nulos/cierre circular | Prueba del caso límite |
| Recorridos y actualizaciones | Procesan datos y estado | Hacen verificable la operación | Salida del programa |
| main | Ejecuta la secuencia | Reproduce un caso concreto | Consola |

## Archivo completo al terminar EJ10
**Este código reúne exactamente los microbloques anteriores; solo ahora corresponde copiarlo y ejecutarlo.**
```java
public class EJ10_OrdenarCircular {
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }
    static class ListaCircular {
        NodoCircular ultimo;
        void insertarFinal(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (ultimo == null) { ultimo = nuevo; ultimo.siguiente = nuevo; return; }
            nuevo.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevo;
            ultimo = nuevo;
        }
        int contar() {
            if (ultimo == null) return 0;
            int n = 0;
            NodoCircular actual = ultimo.siguiente;
            do { n++; actual = actual.siguiente; }
            while (actual != ultimo.siguiente);
            return n;
        }
        void ordenar() {
            int n = contar();
            for (int pasada = 0; pasada < n - 1; pasada++) {
                NodoCircular actual = ultimo.siguiente;
                for (int paso = 0; paso < n - 1 - pasada; paso++) {
                    NodoCircular siguiente = actual.siguiente;
                    if (actual.dato > siguiente.dato) {
                        int temporal = actual.dato;
                        actual.dato = siguiente.dato;
                        siguiente.dato = temporal;
                    }
                    actual = siguiente;
                }
            }
        }
        void mostrar() {
            if (ultimo == null) { System.out.println("(vacia)"); return; }
            NodoCircular actual = ultimo.siguiente;
            do {
                System.out.print(actual.dato + " ");
                actual = actual.siguiente;
            } while (actual != ultimo.siguiente);
            System.out.println();
        }
    }
    public static void main(String[] args) {
        ListaCircular lista = new ListaCircular();
        lista.insertarFinal(8);
        lista.insertarFinal(3);
        lista.insertarFinal(5);
        lista.insertarFinal(1);
        lista.mostrar();
        lista.ordenar();
        lista.mostrar();
        System.out.println("Circular: " + (lista.ultimo.siguiente.dato == 1));
    }
}
```


## Predicción ANTES de ejecutar
¿Qué orden imprime después? ¿El primer nodo sigue referenciado por ultimo.siguiente?

Escribe una respuesta antes de revisar la consola; dibuja los enlaces cuando corresponda.

## Ejecutar y validar
Desde `S07_Listas_Java` ejecuta:

```text
javac -encoding UTF-8 EJ10_OrdenarCircular.java
java EJ10_OrdenarCircular
```

**Salida observada en validación:**

```text
8 3 5 1 
1 3 5 8 
Circular: true
```


## Interpretación
n=4. Pasada 0: 8/3, 8/5 y 8/1 intercambian; el 8 queda al final. Pasada 1: se acomodan 3 y 1. Pasada 2: 1 y 3; queda 1,3,5,8. Cada pasada tiene menos comparaciones.

Este comportamiento responde a la decisión previa: El conteo fija un número de iteraciones finito. Cada comparación usa vecinos dentro de la secuencia desde el primero, sin cruzar el cierre último→primero para ordenar.

## Variación y error controlado
**Variación A:** Ensaya lista vacía y de un único nodo; ordenar no debe lanzar excepciones ni ejecutar comparaciones.

**Error o caso límite:** Una condición que espere `ultimo.siguiente == ultimo` no converge en listas de 2+ nodos: el enlace último→primero no cambia al intercambiar datos. Corrige con pasadas contadas.

**Corrección razonada:** vuelve al microbloque donde se creó o actualizó la referencia/condición incorrecta; restablece el orden de asignación, recompila y confirma que la salida coincide con la predicción. No dejes el error activo en tu solución final.

## Qué debes poder explicar con tus palabras
¿Cómo demostramos que no se volvió un bucle infinito? Justifica también qué cambia y qué se conserva en las referencias, qué condición detiene un recorrido y qué sucedería con la lista vacía.

## T10 — Tarea espejo: Ordenamiento circular descendente
**Relación directa:** EJ10 ↔ T10.

**Enunciado:** Ordena la lista circular 8,3,5,1 de mayor a menor usando el mismo número acotado de pasadas.

**Archivo a crear/modificar:** `T10_OrdenDescCircular.java`. Puedes partir del ejemplo que acabas de construir, renombrar la clase pública y editar la operación indicada; reconstruye el archivo final completo sin depender de otros recursos.

**Restricción:** Solo intercambia datos; conserva referencias y verifica el cierre con ultimo.siguiente.

**Pista:** Para descendente intercambia si dato actual es MENOR que dato siguiente.

**Predice antes de ejecutar:** ¿Qué cambiará en los datos/referencias? ¿Qué se conserva? ¿Qué salida verás?

**Evidencia a entregar:** Salida final 8 5 3 1 y Circular: true.

**Cómo validarla:** Finaliza, ordena de mayor a menor y el primer nodo sigue accesible desde ultimo.siguiente.

**Comandos:** `javac -encoding UTF-8 T10_OrdenDescCircular.java` y `java T10_OrdenDescCircular`. La clase pública en el código debe llamarse `T10_OrdenDescCircular`.

## 6. Cierre de la sesión
Durante esta sesión construiste dos tipos de estructuras enlazadas y comprobaste por qué no basta con almacenar datos: también es necesario conservar correctamente las relaciones entre nodos. Primero aprendiste a crear dos referencias por nodo y a comprobar que los enlaces anterior y siguiente son independientes. Después incorporaste operaciones de una lista doble: insertar, recorrer, buscar, modificar, eliminar y ordenar. Cada operación exigió tomar decisiones sobre el primer nodo, el último y el modo de avanzar. En las listas circulares aprendiste que la conexión desde el último hasta el primero cambia por completo el criterio de terminación. Ya no esperamos encontrar null durante el recorrido de una lista no vacía: comparamos la referencia actual con la del primero. Al ordenar, utilizamos un número finito de pasadas para garantizar que el programa termine. Para repasar, reconstruye a mano los estados antes y después de una eliminación, explica por qué hay que actualizar ambos lados de un enlace doble y prueba los casos de lista vacía y de un único nodo. Resuelve todas las tareas espejo y compara tus predicciones con la consola. Estas habilidades servirán para comprender otras estructuras dinámicas sin perder de vista sus invariantes. Puedes seguir aprendiendo en Lideratec Academy, consultar recursos en https://lideratecacademy.com/blog/ y continuar las explicaciones públicas en https://www.youtube.com/@LideratecAcademy. Si dispones del laboratorio ejecutable complementario, utilízalo solo para contrastar tu trabajo después de haber construido tus propios ejemplos.
