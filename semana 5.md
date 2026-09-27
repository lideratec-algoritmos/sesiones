---
course_id: AED-IA
session_id: S05
module_id: TEMA-05
source_origin: PPT
status: validated
sync_status: PASS
---

# Guía del Estudiante - Listas enlazadas

**Curso:** Algoritmo y Estructura de Datos Basado en IA  
**Sesión:** S05 / Tema 05  
**Lenguaje:** Java  
**Laboratorio sincronizado:** FASE 15  
**Entorno validado:** OpenJDK 21.0.11

> **Regla de esta edición:** en EJ01-EJ09 no se entrega primero un bloque grande de código. Cada ejemplo se construye desde la primera clase hasta `main` mediante microbloques. Antes de escribir la siguiente parte se explica **por qué se crea**, **qué representa**, **qué hace cada línea**, **qué referencia cambia**, **qué estado queda** y **qué problema aparecería si el orden fuese incorrecto**. El archivo completo aparece únicamente al final como consolidación y debe coincidir con el archivo docente de FASE 15.

## 1. Propósito de la sesión

Las listas enlazadas son estructuras lineales dinámicas formadas por nodos conectados mediante referencias. En esta práctica aprenderás el concepto desde el código: primero construirás la unidad `Nodo`, luego la referencia de entrada de una lista, después las operaciones de recorrido e inserción y, finalmente, variantes y un integrador ordenado sin duplicados.

La meta no es memorizar una plantilla. Debes ser capaz de mirar una línea como `nuevo.next = head` y explicar: **qué objeto se modifica, qué referencia se conserva, qué enlace aparece después y por qué esa línea debe ejecutarse antes que otra**.

## 2. Qué significa "explicación paso a paso" en esta guía

Cada ejemplo usa esta secuencia:

```text
1. EXPLICAR QUÉ ESTRUCTURA NECESITAMOS Y POR QUÉ
2. ESCRIBIR SOLO 1-4 LÍNEAS
3. EXPLICAR CADA LÍNEA
4. DIBUJAR / DESCRIBIR EL ESTADO DE REFERENCIAS
5. COMPROBAR QUE SE ENTIENDE
6. RECIÉN ENTONCES ESCRIBIR EL SIGUIENTE MICROBLOQUE
7. AL FINAL MOSTRAR EL ARCHIVO COMPLETO
8. PREDECIR → COMPILAR → EJECUTAR → INTERPRETAR
9. PROVOCAR UN ERROR SEGURO → CORREGIR
10. RESOLVER TAREA ESPEJO
```

## 3. Decisión de organización usada en todos los archivos

Los ejemplos de FASE 15 son archivos Java autocontenidos. Por eso verás repetirse `Nodo` y, cuando corresponde, `ListaSimple` dentro de cada archivo.

**¿Por qué se repite `Nodo`?** Porque queremos que `EJ03_InsertarInicio.java`, por ejemplo, pueda compilarse por sí solo sin depender de `EJ01_NodoYEnlaces.java`. En un proyecto mayor podrías separar clases en archivos distintos; en este laboratorio se prioriza que cada hito sea ejecutable de manera independiente.

**¿Por qué `static class Nodo`?** Porque `main` es estático. Al declarar la clase anidada como `static`, podemos crear `new Nodo(...)` desde `main` sin necesitar primero un objeto de la clase externa del ejemplo.

## 4. Preparación mínima

```bash
java -version
javac -version
```

Trabaja en una carpeta `listas-enlazadas-s05`. Para cada ejemplo:

```bash
javac EJ01_NodoYEnlaces.java
java EJ01_NodoYEnlaces
```

Cambia el nombre según el archivo activo. No necesitas Maven, Gradle ni librerías externas.



# EJ01 - Construir un nodo y enlazar objetos manualmente

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ01_NodoYEnlaces.java` y `estudiante/EJ01_NodoYEnlaces.java`.
- **Tarea espejo:** `T01_EnlazarCuatroNodos.java` - Enlazar cuatro nodos.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ01_NodoYEnlaces.java` del laboratorio.

## ¿Por qué existe este ejemplo?

La sesión define un nodo como la unidad estructural básica de una lista enlazada: contiene un dato y una referencia al siguiente nodo. Este primer ejemplo existe para convertir esa definición en objetos Java reales y ver que una lista surge cuando las referencias `next` se conectan.

## Qué debes conectar con lo anterior

Todavía no usamos una clase `ListaSimple`. Primero necesitamos comprender la unidad mínima. Si no entiendes `Nodo`, `dato` y `next`, las operaciones posteriores se convierten en instrucciones memorizadas sin modelo mental.

## Modelo mental antes de escribir código

```text
Nodo conceptual
+-----------+-----------+
|   dato    |   next    |
+-----------+-----------+
                 |
                 +----> otro Nodo o null
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Crear la clase pública del ejemplo

**Escribe ahora únicamente este microbloque:**

```java
public class EJ01_NodoYEnlaces {
```

**¿Por qué existe este microbloque?**

Necesitamos una clase pública porque Java ejecutará este archivo como un programa. Esta clase externa no representa un nodo; funciona como contenedor del ejemplo completo y alojará `Nodo` y `main`.

**Explicación línea por línea / instrucción por instrucción:**

- `public`: permite que la clase sea accesible como clase pública del archivo.

- `class`: indica que estamos declarando un tipo de referencia en Java.

- `EJ01_NodoYEnlaces`: debe coincidir con `EJ01_NodoYEnlaces.java` porque la clase es pública.

- `{`: abre el alcance de la clase externa.

**No avances hasta poder responder:**

¿La clase `EJ01_NodoYEnlaces` representa un nodo? No: representa el programa del ejemplo.

### Microbloque 2 - Crear la clase Nodo y justificar su existencia

**Escribe ahora únicamente este microbloque:**

```java
    static class Nodo {
        int dato;
        Nodo next;
```

**¿Por qué existe este microbloque?**

Ahora sí creamos el tipo que representa **una unidad de la lista**. Separamos `dato` y `next` porque un nodo necesita almacenar información y, además, saber cuál es el siguiente nodo. Sin `next` tendríamos objetos aislados, no una estructura enlazada.

**Explicación línea por línea / instrucción por instrucción:**

- `static class Nodo {`: define el tipo `Nodo` dentro del archivo. `static` permite usarlo desde `main` sin crear primero un objeto de la clase externa.

- `int dato;`: reserva un atributo entero para la información almacenada por el nodo.

- `Nodo next;`: declara una referencia cuyo tipo es el mismo `Nodo`; por eso puede apuntar al siguiente objeto de la cadena.

**Estado conceptual después de escribirlo:**

```text
Objeto Nodo aún no creado.
El TIPO Nodo queda definido como:
[dato:int | next:Nodo]
```

**No avances hasta poder responder:**

¿Por qué `next` es de tipo `Nodo` y no `int`? Porque debe guardar una referencia a otro objeto Nodo, no el valor del otro nodo.

### Microbloque 3 - Crear el constructor del nodo

**Escribe ahora únicamente este microbloque:**

```java
        Nodo(int dato) {
            this.dato = dato;
            this.next = null;
        }
    }
```

**¿Por qué existe este microbloque?**

Un nodo necesita nacer con un dato concreto. El constructor recibe ese dato y deja `next` en `null` porque todavía no conocemos el siguiente nodo. Ese enlace se construirá después.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo(int dato) {`: declara el constructor. El parámetro `dato` es el valor que llega desde `new Nodo(...)`.

- `this.dato = dato;`: guarda el parámetro en el atributo del objeto actual. `this.dato` es el campo; `dato` es el parámetro.

- `this.next = null;`: declara explícitamente que el nodo recién creado todavía no enlaza a otro nodo.

- `}`: cierra el constructor.

- `}`: cierra la clase `Nodo`.

**Estado conceptual después de escribirlo:**

```text
Después de new Nodo(10):
[10 | null]
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si no asignaras `dato`, el nodo existiría pero no contendría el valor esperado. Si `next` apuntara a un objeto cualquiera desde el constructor, introducirías una relación que todavía no corresponde al problema.

### Microbloque 4 - Abrir main: el punto de entrada

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
```

**¿Por qué existe este microbloque?**

Hasta ahora solo definimos tipos. Ningún objeto se ha creado todavía. `main` es donde Java comenzará a ejecutar instrucciones concretas.

**Explicación línea por línea / instrucción por instrucción:**

- `public`: permite que la JVM pueda invocar el método.

- `static`: permite ejecutar `main` sin construir primero un objeto `EJ01_NodoYEnlaces`.

- `void`: el método no devuelve un valor.

- `main`: es el nombre del punto de entrada.

- `String[] args`: recibe argumentos de línea de comandos, aunque aquí no los usamos.

**No avances hasta poder responder:**

¿Definir una clase `Nodo` crea nodos? No. Los objetos aparecen cuando ejecutamos `new Nodo(...)`.

### Microbloque 5 - Crear tres objetos Nodo

**Escribe ahora únicamente este microbloque:**

```java
        Nodo n1 = new Nodo(10);
        Nodo n2 = new Nodo(20);
        Nodo n3 = new Nodo(30);
```

**¿Por qué existe este microbloque?**

Necesitamos objetos concretos para observar enlaces. Cada línea crea un objeto independiente. En este momento todavía no existe una lista: solo existen tres nodos separados.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo n1 = new Nodo(10);`: declara la referencia `n1`, crea un nodo con dato 10 y guarda en `n1` la referencia al objeto.

- `Nodo n2 = new Nodo(20);`: crea un segundo objeto independiente con dato 20.

- `Nodo n3 = new Nodo(30);`: crea un tercer objeto independiente con dato 30.

**Estado conceptual después de escribirlo:**

```text
n1 --> [10 | null]
n2 --> [20 | null]
n3 --> [30 | null]

Todavía NO hay una lista.
```

**No avances hasta poder responder:**

¿`n1`, `n2` y `n3` contienen físicamente los objetos? No: son referencias que permiten acceder a esos objetos.

### Microbloque 6 - Crear los enlaces next

**Escribe ahora únicamente este microbloque:**

```java
        n1.next = n2;
        n2.next = n3;
```

**¿Por qué existe este microbloque?**

Aquí nace la estructura enlazada. No movemos ni copiamos nodos: cambiamos el valor de los campos `next` para que cada objeto conozca al siguiente.

**Explicación línea por línea / instrucción por instrucción:**

- `n1.next = n2;`: modifica el objeto referenciado por `n1`; su `next` deja de ser `null` y pasa a referenciar el mismo nodo que `n2`.

- `n2.next = n3;`: hace lo mismo entre el segundo y el tercer nodo.

**Estado conceptual después de escribirlo:**

```text
n1           n2           n3
 |            |            |
 v            v            v
[10 | •] ---> [20 | •] ---> [30 | null]
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si omites `n2.next = n3`, la cadena termina en 20 y el nodo 30 queda aislado.

### Microbloque 7 - Definir head como entrada de la cadena

**Escribe ahora únicamente este microbloque:**

```java
        Nodo head = n1;
```

**¿Por qué existe este microbloque?**

Una lista necesita una referencia desde la cual comenzar a recorrer. `head` no crea un nodo adicional: simplemente apunta al mismo objeto que `n1`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo head = n1;`: declara `head` y copia la referencia contenida en `n1`; ambos nombres apuntan al mismo primer nodo.

**Estado conceptual después de escribirlo:**

```text
head --+
       |
n1 ----+--> [10 | •] -> [20 | •] -> [30 | null]
```

**No avances hasta poder responder:**

Si cambiamos `head` para apuntar a `n2`, ¿desaparece n1? No necesariamente; pero desde `head` la lista visible empezaría en 20.

### Microbloque 8 - Leer la estructura siguiendo referencias

**Escribe ahora únicamente este microbloque:**

```java
        System.out.println("Head: " + head.dato);
        System.out.println("Segundo: " + head.next.dato);
        System.out.println("Tercero: " + head.next.next.dato);
        System.out.println("Fin: " + head.next.next.next);
    }
}
```

**¿Por qué existe este microbloque?**

Estas líneas no modifican la lista. Solo demuestran que podemos comenzar en `head` y seguir referencias sucesivas hasta encontrar `null`.

**Explicación línea por línea / instrucción por instrucción:**

- `head.dato`: lee el dato del primer nodo.

- `head.next.dato`: sigue un enlace y lee el dato del segundo nodo.

- `head.next.next.dato`: sigue dos enlaces y lee el tercero.

- `head.next.next.next`: sigue tres enlaces; el tercero tiene `next == null`, por eso imprime `null`.

- `}`: cierra `main`.

- `}`: cierra la clase pública.

**Estado conceptual después de escribirlo:**

```text
Ruta de lectura:
head -> 10 -> 20 -> 30 -> null
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Intentar `head.next.next.next.dato` sería incorrecto porque `head.next.next.next` es `null`; acceder a `.dato` sobre `null` produce `NullPointerException`.

## Auditoría línea por línea de `EJ01_NodoYEnlaces.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ01_NodoYEnlaces {`**  
Crea la clase pública contenedora del ejemplo `EJ01_NodoYEnlaces`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 6: `Nodo(int dato) {`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 7: `this.dato = dato;`**  
Copia el parámetro recibido al atributo del objeto actual. `this.dato` es el campo del nodo; `dato` es el valor que llegó al constructor.

**Línea 8: `this.next = null;`**  
Inicializa explícitamente una referencia en `null`, indicando que todavía no existe un enlace válido en esa dirección.

**Línea 9: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 10: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 12: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 13: `Nodo n1 = new Nodo(10);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 14: `Nodo n2 = new Nodo(20);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 15: `Nodo n3 = new Nodo(30);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 17: `n1.next = n2;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 18: `n2.next = n3;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 20: `Nodo head = n1;`**  
Realiza una asignación. En este laboratorio toda asignación debe leerse preguntando qué variable o referencia cambia y qué valor o objeto queda accesible después.

**Línea 22: `System.out.println("Head: " + head.dato);`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 23: `System.out.println("Segundo: " + head.next.dato);`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 24: `System.out.println("Tercero: " + head.next.next.dato);`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 25: `System.out.println("Fin: " + head.next.next.next);`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 26: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 27: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ01_NodoYEnlaces {
    static class Nodo {
        int dato;
        Nodo next;

        Nodo(int dato) {
            this.dato = dato;
            this.next = null;
        }
    }

    public static void main(String[] args) {
        Nodo n1 = new Nodo(10);
        Nodo n2 = new Nodo(20);
        Nodo n3 = new Nodo(30);

        n1.next = n2;
        n2.next = n3;

        Nodo head = n1;

        System.out.println("Head: " + head.dato);
        System.out.println("Segundo: " + head.next.dato);
        System.out.println("Tercero: " + head.next.next.dato);
        System.out.println("Fin: " + head.next.next.next);
    }
}
```

## Antes de ejecutar: predicción

Escribe en papel la secuencia alcanzable desde `head` y responde: ¿qué valor contiene `n3.next`?

## Ejecuta

```bash
javac EJ01_NodoYEnlaces.java
java EJ01_NodoYEnlaces
```

## Resultado esperado

```text
Head: 10
Segundo: 20
Tercero: 30
Fin: null
```

## Interpretación

La salida confirma cuatro hechos: `head` apunta a n1, n1 enlaza a n2, n2 enlaza a n3 y n3 termina en `null`.

## Error controlado

Comenta `n2.next = n3;` y **no** ejecutes la lectura del tercer dato hasta predecir qué referencia se vuelve `null`. La corrección consiste en restaurar ese enlace.

## T01 - Tarea espejo: enlazar cuatro nodos

Crea cuatro nodos con valores 5, 15, 25 y 35. Enlázalos solo con `next`, define `head` y verifica que el último `next` sea `null`.

- **Restricción:** no uses arreglos ni colecciones.
- **Evidencia:** dibujo de referencias + salida del programa.
- **Archivo F15:** `estudiante/T01_EnlazarCuatroNodos.java`.
- **Criterio:** puedes explicar cada enlace sin leer el código.



# EJ02 - Recorrer una lista enlazada simple

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ02_RecorridoSimple.java` y `estudiante/EJ02_RecorridoSimple.java`.
- **Tarea espejo:** `T02_RecorridoConFlechas.java` - Recorrido con flechas.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ02_RecorridoSimple.java` del laboratorio.

## ¿Por qué existe este ejemplo?

El recorrido es la operación que hace visible la naturaleza secuencial de una lista enlazada. Necesitamos una variable local que avance nodo por nodo sin destruir la referencia de entrada `head`.

## Qué debes conectar con lo anterior

EJ01 enlazó nodos manualmente. EJ02 introduce una clase `ListaSimple` porque ahora la estructura tendrá **estado propio** (`head`) y comportamiento (`cargarEjemplo`, `recorrer`).

## Modelo mental antes de escribir código

```text
ListaSimple
head ---> [29|•] -> [3|•] -> [4|•] -> [13|null]
            ^
         actual comienza aquí y avanza
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Crear la clase externa y volver a definir Nodo

**Escribe ahora únicamente este microbloque:**

```java
public class EJ02_RecorridoSimple {
    static class Nodo {
        int dato;
        Nodo next;
```

**¿Por qué existe este microbloque?**

Cada archivo de FASE 15 debe compilar de manera independiente. Por eso este ejemplo vuelve a declarar `Nodo`. No se está enseñando un concepto nuevo; se está haciendo el archivo autocontenido.

**Explicación línea por línea / instrucción por instrucción:**

- `public class EJ02_RecorridoSimple {`: contenedor ejecutable del ejemplo.

- `static class Nodo {`: tipo que modela cada elemento.

- `int dato;`: valor guardado.

- `Nodo next;`: referencia al siguiente nodo.

**No avances hasta poder responder:**

¿Por qué no reutilizamos directamente la clase Nodo del archivo EJ01? Porque son archivos independientes y no configuramos dependencias entre ellos.

### Microbloque 2 - Constructor mínimo del Nodo

**Escribe ahora únicamente este microbloque:**

```java
        Nodo(int dato) {
            this.dato = dato;
        }
    }
```

**¿Por qué existe este microbloque?**

Aquí basta almacenar el dato. Java inicializa las referencias de instancia en `null`, por lo que `next` comienza en `null` aunque no lo escribamos explícitamente. EJ01 lo hizo explícito para enseñar el estado inicial; aquí reforzamos que el valor por defecto de una referencia de instancia es `null`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo(int dato) {`: constructor con el dato inicial.

- `this.dato = dato;`: guarda el valor en el objeto actual.

- `}`: cierra constructor.

- `}`: cierra Nodo.

**Estado conceptual después de escribirlo:**

```text
new Nodo(29) -> [29 | null]
```

### Microbloque 3 - Crear ListaSimple y su referencia head

**Escribe ahora únicamente este microbloque:**

```java
    static class ListaSimple {
        Nodo head;
```

**¿Por qué existe este microbloque?**

Creamos una clase de lista porque ya no queremos que `head` sea una variable suelta en `main`. `ListaSimple` representa la estructura completa y `head` conserva su punto de entrada.

**Explicación línea por línea / instrucción por instrucción:**

- `static class ListaSimple {`: define el tipo que agrupa estado y operaciones de la lista.

- `Nodo head;`: referencia al primer nodo. Si la lista está vacía, su valor es `null`.

**Estado conceptual después de escribirlo:**

```text
ListaSimple
head -> null   (lista vacía)
```

**No avances hasta poder responder:**

¿`head` guarda todos los nodos? No. Guarda solo la referencia al primero; los demás se alcanzan siguiendo `next`.

### Microbloque 4 - Crear los nodos del ejemplo dentro de cargarEjemplo

**Escribe ahora únicamente este microbloque:**

```java
        void cargarEjemplo() {
            Nodo n1 = new Nodo(29);
            Nodo n2 = new Nodo(3);
            Nodo n3 = new Nodo(4);
            Nodo n4 = new Nodo(13);
```

**¿Por qué existe este microbloque?**

Separamos la preparación de datos del recorrido para poder analizar cada responsabilidad. `cargarEjemplo` construye el estado inicial; `recorrer` lo leerá después.

**Explicación línea por línea / instrucción por instrucción:**

- `void cargarEjemplo() {`: método de apoyo que no devuelve valor; prepara la lista usada en la demostración.

- `Nodo n1 = new Nodo(29);`: crea el primer nodo.

- `Nodo n2 = new Nodo(3);`: crea el segundo.

- `Nodo n3 = new Nodo(4);`: crea el tercero.

- `Nodo n4 = new Nodo(13);`: crea el cuarto.

**Estado conceptual después de escribirlo:**

```text
n1 [29|null]   n2 [3|null]   n3 [4|null]   n4 [13|null]
```

### Microbloque 5 - Enlazar y establecer head

**Escribe ahora únicamente este microbloque:**

```java
            n1.next = n2;
            n2.next = n3;
            n3.next = n4;
            head = n1;
        }
```

**¿Por qué existe este microbloque?**

Este bloque transforma objetos aislados en una lista. La última asignación hace visible la cadena desde el estado de `ListaSimple`.

**Explicación línea por línea / instrucción por instrucción:**

- `n1.next = n2;`: 29 enlaza con 3.

- `n2.next = n3;`: 3 enlaza con 4.

- `n3.next = n4;`: 4 enlaza con 13.

- `head = n1;`: la lista comienza en el nodo 29.

- `}`: termina la carga.

**Estado conceptual después de escribirlo:**

```text
head -> [29|•] -> [3|•] -> [4|•] -> [13|null]
```

### Microbloque 6 - Crear la variable local actual

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
```

**¿Por qué existe este microbloque?**

No queremos mover `head`, porque `head` es parte del estado permanente de la lista. Creamos `actual` como cursor temporal: puede avanzar sin cambiar dónde comienza la lista.

**Explicación línea por línea / instrucción por instrucción:**

- `void recorrer() {`: abre la operación de lectura secuencial.

- `Nodo actual = head;`: copia la referencia de `head` en una variable local; no crea un nodo ni modifica `head`.

**Estado conceptual después de escribirlo:**

```text
head -------> [29|•] -> [3|•] -> [4|•] -> [13|null]
actual -----^
```

**No avances hasta poder responder:**

Después de `actual = actual.next`, ¿head cambia? No. Solo cambia la variable local `actual`.

### Microbloque 7 - Mantener el ciclo mientras exista un nodo actual

**Escribe ahora únicamente este microbloque:**

```java
            while (actual != null) {
```

**¿Por qué existe este microbloque?**

La condición pregunta si el cursor está parado sobre un nodo válido. Esto permite procesar también el último nodo: cuando `actual` sea el último, sigue siendo distinto de `null`.

**Explicación línea por línea / instrucción por instrucción:**

- `while (actual != null) {`: repite mientras exista un nodo accesible en `actual`.

**Qué error evita o qué ocurriría si se omite/cambia:**

Si usaras `actual.next != null`, dejarías de procesar el último nodo porque el ciclo se detendría antes de entrar cuando `actual` está en 13.

### Microbloque 8 - Procesar el nodo actual y avanzar

**Escribe ahora únicamente este microbloque:**

```java
                System.out.print(actual.dato + " ");
                actual = actual.next;
            }
            System.out.println();
        }
```

**¿Por qué existe este microbloque?**

Un recorrido enlazado repite dos microacciones: **usar el nodo actual** y **avanzar al siguiente**. El orden importa: si avanzaras primero, omitirías el nodo inicial.

**Explicación línea por línea / instrucción por instrucción:**

- `System.out.print(actual.dato + " ");`: lee e imprime el dato del nodo actual.

- `actual = actual.next;`: reemplaza la referencia local por la del siguiente nodo.

- `}`: cierra el while.

- `System.out.println();`: termina la línea de salida.

- `}`: cierra recorrer.

**Estado conceptual después de escribirlo:**

```text
Iteración 1: actual=29 -> imprime 29 -> actual=3
Iteración 2: actual=3  -> imprime 3  -> actual=4
Iteración 3: actual=4  -> imprime 4  -> actual=13
Iteración 4: actual=13 -> imprime 13 -> actual=null
Fin: actual == null
```

### Microbloque 9 - Construir y usar una ListaSimple desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.cargarEjemplo();
        lista.recorrer();
    }
}
```

**¿Por qué existe este microbloque?**

`main` coordina la demostración: crea la estructura, carga datos y después ejecuta el recorrido. Esta separación hace visible qué método modifica estado y cuál lo observa.

**Explicación línea por línea / instrucción por instrucción:**

- `ListaSimple lista = new ListaSimple();`: crea una lista cuyo `head` comienza en null.

- `lista.cargarEjemplo();`: construye y enlaza los cuatro nodos.

- `lista.recorrer();`: visita los nodos desde `head` hasta `null`.

- `}`: cierra main.

- `}`: cierra la clase externa.

**No avances hasta poder responder:**

Antes de `cargarEjemplo`, ¿qué vale `lista.head`? `null`. Después de la carga, apunta al nodo 29.

## Auditoría línea por línea de `EJ02_RecorridoSimple.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ02_RecorridoSimple {`**  
Crea la clase pública contenedora del ejemplo `EJ02_RecorridoSimple`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 6: `Nodo(int dato) {`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 7: `this.dato = dato;`**  
Copia el parámetro recibido al atributo del objeto actual. `this.dato` es el campo del nodo; `dato` es el valor que llegó al constructor.

**Línea 8: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 9: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 11: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 12: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 14: `void cargarEjemplo() {`**  
Abre el método `cargarEjemplo`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 15: `Nodo n1 = new Nodo(29);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 16: `Nodo n2 = new Nodo(3);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 17: `Nodo n3 = new Nodo(4);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 18: `Nodo n4 = new Nodo(13);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 19: `n1.next = n2;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 20: `n2.next = n3;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 21: `n3.next = n4;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 22: `head = n1;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 23: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 25: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 26: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 27: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 28: `System.out.print(actual.dato + " ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 29: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 30: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 31: `System.out.println();`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 32: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 33: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 35: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 36: `ListaSimple lista = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 37: `lista.cargarEjemplo();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 38: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 39: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 40: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ02_RecorridoSimple {
    static class Nodo {
        int dato;
        Nodo next;

        Nodo(int dato) {
            this.dato = dato;
        }
    }

    static class ListaSimple {
        Nodo head;

        void cargarEjemplo() {
            Nodo n1 = new Nodo(29);
            Nodo n2 = new Nodo(3);
            Nodo n3 = new Nodo(4);
            Nodo n4 = new Nodo(13);
            n1.next = n2;
            n2.next = n3;
            n3.next = n4;
            head = n1;
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " ");
                actual = actual.next;
            }
            System.out.println();
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.cargarEjemplo();
        lista.recorrer();
    }
}
```

## Traza obligatoria antes de ejecutar

```text
actual=29 → imprime 29 → actual=3
actual=3  → imprime 3  → actual=4
actual=4  → imprime 4  → actual=13
actual=13 → imprime 13 → actual=null
```

## Ejecuta

```bash
javac EJ02_RecorridoSimple.java
java EJ02_RecorridoSimple
```

## Resultado esperado

```text
29 3 4 13
```

## Error controlado

Cambia temporalmente `while (actual != null)` por `while (actual.next != null)`. Predice cuál nodo falta y explica por qué. Después restaura la condición correcta.

## T02 - Tarea espejo

Recorre `7 -> 14 -> 21 -> 28 -> null` con una variable `actual` sin modificar `head`. Imprime flechas y, al final, demuestra que `head.dato` sigue siendo 7.

- **Archivo F15:** `estudiante/T02_RecorridoConFlechas.java`.
- **Evidencia:** salida + explicación de por qué `actual` puede cambiar sin alterar `head`.



# EJ03 - Insertar un nodo al inicio

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ03_InsertarInicio.java` y `estudiante/EJ03_InsertarInicio.java`.
- **Tarea espejo:** `T03_InsertarInicioOrden.java` - Construir un orden usando inserción al inicio.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ03_InsertarInicio.java` del laboratorio.

## ¿Por qué existe este ejemplo?

La inserción al inicio muestra por primera vez una operación que cambia `head`. El aprendizaje crítico es el **orden seguro de las asignaciones**: primero conservar el antiguo inicio en `nuevo.next`, luego mover `head` al nuevo nodo.

## Qué debes conectar con lo anterior

Ya sabes qué representa `Nodo`, qué hace `head` y cómo recorrer. Aquí repetimos esas clases porque el archivo F15 debe ser ejecutable por sí mismo, pero el concepto nuevo está en `insertarInicio`.

## Modelo mental antes de escribir código

```text
ANTES de insertar 10:
head -> [20|•] -> [30|null]

OBJETIVO:
head -> [10|•] -> [20|•] -> [30|null]
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Crear contenedor, Nodo y su constructor

**Escribe ahora únicamente este microbloque:**

```java
public class EJ03_InsertarInicio {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }
```

**¿Por qué existe este microbloque?**

Volvemos a definir `Nodo` para que este ejemplo sea independiente. Aunque el constructor no escribe `next = null`, Java deja referencias de instancia en `null`; por eso un nodo nuevo comienza sin sucesor.

**Explicación línea por línea / instrucción por instrucción:**

- `public class EJ03_InsertarInicio {`: clase ejecutable del ejemplo.

- `static class Nodo {`: tipo de elemento de la lista.

- `int dato;`: información almacenada.

- `Nodo next;`: enlace al siguiente nodo.

- `Nodo(int dato) { this.dato = dato; }`: constructor compacto que guarda el dato; `next` queda en null por defecto.

- `}`: cierra Nodo.

**No avances hasta poder responder:**

¿Por qué seguimos necesitando `next` si insertaremos solo al inicio? Porque el nuevo nodo debe enlazar con el inicio anterior.

### Microbloque 2 - Crear ListaSimple y head

**Escribe ahora únicamente este microbloque:**

```java
    static class ListaSimple {
        Nodo head;
```

**¿Por qué existe este microbloque?**

La operación `insertarInicio` debe modificar el estado de una lista concreta. Ese estado se representa con `head`.

**Explicación línea por línea / instrucción por instrucción:**

- `static class ListaSimple {`: agrupa la referencia de entrada y las operaciones.

- `Nodo head;`: apunta al primer nodo o a null si la lista está vacía.

**Estado conceptual después de escribirlo:**

```text
Lista recién creada: head -> null
```

### Microbloque 3 - Abrir insertarInicio y crear el nuevo nodo

**Escribe ahora únicamente este microbloque:**

```java
        void insertarInicio(int dato) {
            Nodo nuevo = new Nodo(dato);
```

**¿Por qué existe este microbloque?**

Cada inserción comienza creando el objeto que se quiere incorporar. Todavía no lo conectamos: primero necesitamos tener una referencia `nuevo` que identifique ese objeto.

**Explicación línea por línea / instrucción por instrucción:**

- `void insertarInicio(int dato) {`: método que recibe el dato a insertar y modifica la lista.

- `Nodo nuevo = new Nodo(dato);`: crea el nodo que se incorporará; `nuevo.next` empieza en null.

**Estado conceptual después de escribirlo:**

```text
Ejemplo si dato=10:
head -> [20|•] -> [30|null]
nuevo -> [10|null]
```

**No avances hasta poder responder:**

¿Crear `nuevo` cambia head? No. La lista sigue igual hasta modificar referencias.

### Microbloque 4 - Conservar el inicio anterior

**Escribe ahora únicamente este microbloque:**

```java
            nuevo.next = head;
```

**¿Por qué existe este microbloque?**

Esta es la línea más importante de la operación. Antes de mover `head`, guardamos la referencia al inicio anterior dentro de `nuevo.next`. Así el nuevo nodo conoce a la lista existente.

**Explicación línea por línea / instrucción por instrucción:**

- `nuevo.next = head;`: el campo `next` del nuevo nodo recibe la misma referencia que actualmente guarda `head`.

**Estado conceptual después de escribirlo:**

```text
head -----> [20|•] -> [30|null]
             ^
             |
nuevo -> [10|•]

Todavía head sigue en 20, pero 10 ya enlaza con 20.
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si primero ejecutaras `head = nuevo`, perderías desde `head` la referencia al inicio anterior. La siguiente línea podría crear un self-loop si haces `nuevo.next = head`.

### Microbloque 5 - Mover head al nuevo nodo

**Escribe ahora únicamente este microbloque:**

```java
            head = nuevo;
        }
```

**¿Por qué existe este microbloque?**

Ahora que el nodo nuevo ya conserva el enlace anterior, es seguro cambiar la entrada de la lista. No se copian nodos: solo cambia qué objeto referencia `head`.

**Explicación línea por línea / instrucción por instrucción:**

- `head = nuevo;`: hace que la lista comience en el nuevo nodo.

- `}`: cierra insertarInicio.

**Estado conceptual después de escribirlo:**

```text
head/nuevo -> [10|•] -> [20|•] -> [30|null]
```

**No avances hasta poder responder:**

¿Cuántos nodos existentes se recorren para insertar al inicio? Ninguno. Por eso la operación mostrada es O(1).

### Microbloque 6 - Crear recorrer como instrumento de validación

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }
```

**¿Por qué existe este microbloque?**

Necesitamos una forma observable de comprobar la estructura después de cada inserción. `recorrer` no es el concepto nuevo, pero permite validar que los enlaces quedaron correctos.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head;`: crea un cursor sin mover head.

- `while (actual != null) {`: procesa cada nodo existente.

- `System.out.print(...)`: muestra el dato.

- `actual = actual.next;`: avanza por el enlace.

- `System.out.println("null");`: representa el final de la lista simple.

- `}`: cierra recorrer y luego ListaSimple.

**Estado conceptual después de escribirlo:**

```text
recorrer lee: head -> ... -> null
```

### Microbloque 7 - Usar inserciones sucesivas desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarInicio(30);
        lista.insertarInicio(20);
        lista.insertarInicio(10);
        lista.recorrer();
    }
}
```

**¿Por qué existe este microbloque?**

Las inserciones al inicio invierten el orden temporal de las llamadas: 30 entra primero, luego 20 queda delante y finalmente 10 queda como nueva cabeza.

**Explicación línea por línea / instrucción por instrucción:**

- `ListaSimple lista = new ListaSimple();`: crea lista vacía.

- `lista.insertarInicio(30);`: head pasa a 30.

- `lista.insertarInicio(20);`: 20 enlaza a 30 y head pasa a 20.

- `lista.insertarInicio(10);`: 10 enlaza a 20 y head pasa a 10.

- `lista.recorrer();`: verifica el orden final.

**Estado conceptual después de escribirlo:**

```text
Después de 30: head -> 30 -> null
Después de 20: head -> 20 -> 30 -> null
Después de 10: head -> 10 -> 20 -> 30 -> null
```

## Auditoría línea por línea de `EJ03_InsertarInicio.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ03_InsertarInicio {`**  
Crea la clase pública contenedora del ejemplo `EJ03_InsertarInicio`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `void insertarInicio(int dato) {`**  
Abre el método `insertarInicio`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 13: `nuevo.next = head;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 14: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 15: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 17: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 18: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 19: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 20: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 21: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 22: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 23: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 24: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 25: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 27: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 28: `ListaSimple lista = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 29: `lista.insertarInicio(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 30: `lista.insertarInicio(20);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 31: `lista.insertarInicio(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 32: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 33: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 34: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ03_InsertarInicio {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void insertarInicio(int dato) {
            Nodo nuevo = new Nodo(dato);
            nuevo.next = head;
            head = nuevo;
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarInicio(30);
        lista.insertarInicio(20);
        lista.insertarInicio(10);
        lista.recorrer();
    }
}
```

## Ejecuta

```bash
javac EJ03_InsertarInicio.java
java EJ03_InsertarInicio
```

## Resultado esperado

```text
10 -> 20 -> 30 -> null
```

## Error controlado clave

Prueba mentalmente este orden incorrecto, sin dejarlo como versión final:

```java
head = nuevo;
nuevo.next = head;
```

Después de la primera línea, `head` y `nuevo` apuntan al mismo objeto. Por eso la segunda hace que `nuevo.next` apunte al propio `nuevo`: aparece un ciclo de un nodo.

## T03 - Tarea espejo

Construye `10 -> 20 -> 30 -> 40 -> null` usando **solo** `insertarInicio`. Decide qué orden de llamadas necesitas antes de programar.

- **Archivo F15:** `estudiante/T03_InsertarInicioOrden.java`.
- **Criterio:** además de obtener la salida, explica por qué las llamadas se realizan en orden inverso.



# EJ04 - Insertar un nodo al final

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ04_InsertarFinal.java` y `estudiante/EJ04_InsertarFinal.java`.
- **Tarea espejo:** `T04_InsertarFinalOrden.java` - Insertar varios nodos al final.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ04_InsertarFinal.java` del laboratorio.

## ¿Por qué existe este ejemplo?

Insertar al final obliga a distinguir dos situaciones: lista vacía y lista con nodos. Sin una referencia `cola`, la implementación debe recorrer desde `head` hasta encontrar el nodo cuyo `next` es `null`.

## Qué debes conectar con lo anterior

EJ03 no recorría la lista porque cambiar `head` bastaba. EJ04 sí necesita desplazarse hasta el último nodo; por eso aparece un ciclo `while` dentro de la operación de inserción.

## Modelo mental antes de escribir código

```text
head -> [29|•] -> [3|•] -> [4|null]
                                ^
                         buscar este nodo
Luego: último.next = nuevo
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Definir Nodo y ListaSimple

**Escribe ahora únicamente este microbloque:**

```java
public class EJ04_InsertarFinal {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;
```

**¿Por qué existe este microbloque?**

Se repiten las estructuras base para mantener el archivo independiente. `head` sigue siendo suficiente para representar la lista, pero como no mantenemos `cola`, insertar al final requerirá un recorrido.

**Explicación línea por línea / instrucción por instrucción:**

- `static class Nodo`: define la unidad con dato y enlace.

- `Nodo next`: permite encadenar nodos.

- `static class ListaSimple`: agrupa el estado de la estructura.

- `Nodo head`: entrada a la lista.

**No avances hasta poder responder:**

¿Existe una variable `cola` en este ejemplo? No. Esa decisión explica por qué debemos buscar el final.

### Microbloque 2 - Crear el nodo que se desea insertar

**Escribe ahora únicamente este microbloque:**

```java
        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
```

**¿Por qué existe este microbloque?**

La operación comienza igual que insertar al inicio: crear el objeto que se incorporará. Aún no sabemos dónde enlazarlo porque la lista puede estar vacía o contener varios nodos.

**Explicación línea por línea / instrucción por instrucción:**

- `void insertarFinal(int dato) {`: método que insertará el dato al final.

- `Nodo nuevo = new Nodo(dato);`: crea el nodo; su next comienza en null, que es justamente el estado esperado para un futuro último nodo.

**Estado conceptual después de escribirlo:**

```text
nuevo -> [dato|null]
```

### Microbloque 3 - Resolver primero la lista vacía

**Escribe ahora únicamente este microbloque:**

```java
            if (head == null) {
                head = nuevo;
                return;
            }
```

**¿Por qué existe este microbloque?**

Si `head` es `null`, no existe ningún nodo que recorrer. El nuevo nodo debe convertirse directamente en el primero y último. `return` evita ejecutar el recorrido pensado para una lista no vacía.

**Explicación línea por línea / instrucción por instrucción:**

- `if (head == null) {`: detecta el caso vacío.

- `head = nuevo;`: la lista comienza en el nuevo nodo.

- `return;`: termina el método porque la inserción ya está completa.

- `}`: cierra el caso especial.

**Estado conceptual después de escribirlo:**

```text
Antes: head -> null
Después: head -> [dato|null]
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si eliminas este caso y la lista está vacía, `actual` será null y leer `actual.next` provocará `NullPointerException`.

### Microbloque 4 - Crear un cursor para buscar el último nodo

**Escribe ahora únicamente este microbloque:**

```java
            Nodo actual = head;
```

**¿Por qué existe este microbloque?**

No movemos `head`; copiamos su referencia a `actual` porque el recorrido necesita una variable temporal.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head;`: cursor que comienza en el primer nodo.

**Estado conceptual después de escribirlo:**

```text
head/actual -> [primer nodo|•] -> ...
```

### Microbloque 5 - Avanzar mientras exista un nodo después del actual

**Escribe ahora únicamente este microbloque:**

```java
            while (actual.next != null) {
                actual = actual.next;
            }
```

**¿Por qué existe este microbloque?**

Aquí sí conviene mirar `actual.next`: queremos detenernos **sobre el último nodo**, no avanzar hasta null. La condición termina exactamente cuando `actual.next == null`.

**Explicación línea por línea / instrucción por instrucción:**

- `while (actual.next != null) {`: mientras haya sucesor, todavía no estamos en el último nodo.

- `actual = actual.next;`: avanza una posición.

- `}`: termina cuando actual es el último.

**Estado conceptual después de escribirlo:**

```text
Al terminar:
actual -> [último | null]
```

**No avances hasta poder responder:**

¿Por qué en recorrer usamos `actual != null` y aquí `actual.next != null`? Porque los objetivos son distintos: recorrer procesa todos; insertarFinal necesita detenerse en el último nodo.

### Microbloque 6 - Conectar el último nodo con nuevo

**Escribe ahora únicamente este microbloque:**

```java
            actual.next = nuevo;
        }
```

**¿Por qué existe este microbloque?**

Ya encontramos el nodo cuyo enlace era `null`. Reemplazar ese `null` por la referencia `nuevo` agrega el nodo al final. Como `nuevo.next` sigue en null, él se convierte en el nuevo último.

**Explicación línea por línea / instrucción por instrucción:**

- `actual.next = nuevo;`: cambia el enlace del antiguo último nodo para apuntar al nuevo.

- `}`: cierra insertarFinal.

**Estado conceptual después de escribirlo:**

```text
... -> [antiguo último|•] -> [nuevo|null]
```

### Microbloque 7 - Agregar recorrer para comprobar el resultado

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }
```

**¿Por qué existe este microbloque?**

Este método es el instrumento de observación. Se mantiene separado de insertarFinal para que una operación modifique y la otra verifique.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head;`: cursor temporal.

- `while (actual != null)`: visita todos los nodos.

- `actual = actual.next`: avanza.

- `println("null")`: marca el final.

### Microbloque 8 - Probar varias inserciones desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(4);
        lista.insertarFinal(13);
        lista.insertarFinal(10);
        lista.recorrer();
    }
}
```

**¿Por qué existe este microbloque?**

La primera llamada toma la rama de lista vacía. Las siguientes recorren hasta el final existente y agregan un enlace nuevo.

**Explicación línea por línea / instrucción por instrucción:**

- `new ListaSimple()`: crea una lista vacía.

- `insertarFinal(29)`: head pasa a 29 sin recorrer.

- `insertarFinal(3)`: recorre hasta 29 y enlaza 3.

- `insertarFinal(4)`: recorre 29,3 y enlaza 4.

- `insertarFinal(13)`: agrega 13 al final.

- `insertarFinal(10)`: agrega 10 al final.

- `recorrer()`: muestra la cadena final.

**No avances hasta poder responder:**

¿Por qué la operación se considera O(n) en esta implementación? Porque, salvo lista vacía, puede tener que visitar hasta el último nodo antes de insertar.

## Auditoría línea por línea de `EJ04_InsertarFinal.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ04_InsertarFinal {`**  
Crea la clase pública contenedora del ejemplo `EJ04_InsertarFinal`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `void insertarFinal(int dato) {`**  
Abre el método `insertarFinal`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 13: `if (head == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 14: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 15: `return;`**  
Termina el método en este punto porque el caso actual ya quedó resuelto y continuar ejecutando produciría lógica innecesaria o incorrecta.

**Línea 16: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 18: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 19: `while (actual.next != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 20: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 21: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 22: `actual.next = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 23: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 25: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 26: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 27: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 28: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 29: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 30: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 31: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 32: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 33: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 35: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 36: `ListaSimple lista = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 37: `lista.insertarFinal(29);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 38: `lista.insertarFinal(3);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 39: `lista.insertarFinal(4);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 40: `lista.insertarFinal(13);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 41: `lista.insertarFinal(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 42: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 43: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 44: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ04_InsertarFinal {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (head == null) {
                head = nuevo;
                return;
            }

            Nodo actual = head;
            while (actual.next != null) {
                actual = actual.next;
            }
            actual.next = nuevo;
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(4);
        lista.insertarFinal(13);
        lista.insertarFinal(10);
        lista.recorrer();
    }
}
```

## Resultado esperado

```text
29 -> 3 -> 4 -> 13 -> 10 -> null
```

## Error controlado

Elimina mentalmente el bloque `if (head == null)` e imagina la primera llamada. `actual` sería `null`; evaluar `actual.next` no es válido. Esta observación justifica por qué el caso vacío se resuelve antes del ciclo.

## T04 - Tarea espejo

Inserta 5, 10, 15 y 20 al final. Antes de ejecutar cada inserción, indica cuántos enlaces crees que recorrerá el método.

- **Archivo F15:** `estudiante/T04_InsertarFinalOrden.java`.
- **Evidencia:** salida final + tabla breve de predicción de recorridos.



# EJ05 - Insertar en una posición específica

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ05_InsertarPosicion.java` y `estudiante/EJ05_InsertarPosicion.java`.
- **Tarea espejo:** `T05_InsertarPosicion.java` - Insertar 24 entre 16 y 32.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ05_InsertarPosicion.java` del laboratorio.

## ¿Por qué existe este ejemplo?

Insertar en una posición intermedia combina validación, recorrido y actualización de dos enlaces. El objetivo es detenerse en el nodo **anterior** a la posición destino y conservar la cadena posterior antes de reemplazar ningún enlace.

## Qué debes conectar con lo anterior

Este archivo reutiliza `insertarInicio` e `insertarFinal` como operaciones de apoyo. La novedad está en `insertarEnPosicion(dato, posicion)` y en el orden seguro de sus dos asignaciones finales.

## Modelo mental antes de escribir código

```text
Antes, posición 2:
0        1        2        3
29  ->   3   ->   4   ->   13
         ^
      actual debe detenerse aquí

Luego: nuevo.next = actual.next
       actual.next = nuevo
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Definir estructura base

**Escribe ahora únicamente este microbloque:**

```java
public class EJ05_InsertarPosicion {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;
```

**¿Por qué existe este microbloque?**

Se repite la arquitectura base para que el archivo sea autónomo. `head` es la referencia desde la que se calculará cualquier posición alcanzable.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo`: modela cada elemento.

- `next`: enlace al siguiente.

- `ListaSimple`: agrupa métodos de la estructura.

- `head`: posición lógica 0 de la lista si existe.

### Microbloque 2 - Reutilizar inserción al inicio

**Escribe ahora únicamente este microbloque:**

```java
        void insertarInicio(int dato) {
            Nodo nuevo = new Nodo(dato);
            nuevo.next = head;
            head = nuevo;
        }
```

**¿Por qué existe este microbloque?**

La posición 0 es exactamente el caso "insertar al inicio". En lugar de duplicar esa lógica dentro del método general, la encapsulamos y luego la invocamos cuando `posicion == 0`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo nuevo = new Nodo(dato);`: crea el nodo.

- `nuevo.next = head;`: conserva el inicio anterior.

- `head = nuevo;`: mueve la entrada.

**No avances hasta poder responder:**

¿Por qué conviene llamar a insertarInicio para posición 0? Porque es el mismo comportamiento y evita tener dos versiones de la misma lógica.

### Microbloque 3 - Reutilizar inserción al final para cargar datos

**Escribe ahora únicamente este microbloque:**

```java
        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (head == null) {
                head = nuevo;
                return;
            }
            Nodo actual = head;
            while (actual.next != null) {
                actual = actual.next;
            }
            actual.next = nuevo;
        }
```

**¿Por qué existe este microbloque?**

Este método ya fue explicado en EJ04. Aquí se conserva porque `main` lo usa para preparar la lista base 29 -> 3 -> 4 -> 13. La lógica no es el foco nuevo, pero sigue siendo parte del archivo sincronizado F15.

**Explicación línea por línea / instrucción por instrucción:**

- `if (head == null)`: resuelve lista vacía.

- `Nodo actual = head`: cursor de recorrido.

- `while (actual.next != null)`: busca el último nodo.

- `actual.next = nuevo`: agrega el nuevo al final.

### Microbloque 4 - Abrir insertarEnPosicion y validar posiciones negativas

**Escribe ahora únicamente este microbloque:**

```java
        void insertarEnPosicion(int dato, int posicion) {
            if (posicion < 0) {
                throw new IllegalArgumentException("Posición inválida");
            }
```

**¿Por qué existe este microbloque?**

Una posición negativa no existe en el esquema 0-based usado por el ejemplo. Conviene rechazarla antes de recorrer para que el error sea explícito y no se transforme en un comportamiento ambiguo.

**Explicación línea por línea / instrucción por instrucción:**

- `void insertarEnPosicion(int dato, int posicion) {`: recibe el valor y la posición 0-based.

- `if (posicion < 0)`: detecta una entrada inválida antes de tocar la lista.

- `throw new IllegalArgumentException(...)`: interrumpe la operación con una causa clara.

**Qué error evita o qué ocurriría si se omite/cambia:**

Sin esta validación, una posición negativa podría atravesar ramas no diseñadas para ella y producir un resultado difícil de justificar.

### Microbloque 5 - Resolver posición 0 como un caso conocido

**Escribe ahora únicamente este microbloque:**

```java
            if (posicion == 0) {
                insertarInicio(dato);
                return;
            }
```

**¿Por qué existe este microbloque?**

No necesitamos buscar un nodo anterior cuando la inserción ocurre al inicio: no existe una posición -1. Delegamos en el método que ya resuelve correctamente ese caso.

**Explicación línea por línea / instrucción por instrucción:**

- `if (posicion == 0)`: reconoce el caso especial.

- `insertarInicio(dato)`: reutiliza lógica segura.

- `return`: evita continuar con el recorrido intermedio.

### Microbloque 6 - Crear cursor y contador

**Escribe ahora únicamente este microbloque:**

```java
            Nodo actual = head;
            int contador = 0;
```

**¿Por qué existe este microbloque?**

Para insertar en posición `p`, necesitamos llegar al nodo `p-1`. `actual` indica dónde estamos y `contador` registra el índice lógico de ese nodo.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head;`: empieza en posición 0.

- `int contador = 0;`: sincroniza el índice con el nodo apuntado por actual.

**Estado conceptual después de escribirlo:**

```text
actual=head (posición 0)
contador=0
```

### Microbloque 7 - Avanzar hasta el nodo anterior a la posición destino

**Escribe ahora únicamente este microbloque:**

```java
            while (actual != null && contador < posicion - 1) {
                actual = actual.next;
                contador++;
            }
```

**¿Por qué existe este microbloque?**

La condición tiene dos responsabilidades: no avanzar más allá del final y detenerse cuando `contador` llegue a `posicion - 1`. Para posición 2, debemos parar en posición 1.

**Explicación línea por línea / instrucción por instrucción:**

- `actual != null`: protege el acceso si la lista termina antes.

- `contador < posicion - 1`: indica que todavía no estamos en el nodo anterior.

- `actual = actual.next`: avanza un nodo.

- `contador++`: mantiene el índice alineado con actual.

**Estado conceptual después de escribirlo:**

```text
Para posicion=2:
inicio: actual=29, contador=0
1 vuelta: actual=3, contador=1
fin: 1 < 1 es falso; actual queda en 3
```

### Microbloque 8 - Detectar posición fuera de rango

**Escribe ahora únicamente este microbloque:**

```java
            if (actual == null) {
                throw new IndexOutOfBoundsException("Posición fuera de rango");
            }
```

**¿Por qué existe este microbloque?**

Si el recorrido agotó la lista, no existe el nodo anterior necesario para realizar la inserción. Rechazamos la operación antes de crear enlaces incorrectos.

**Explicación línea por línea / instrucción por instrucción:**

- `if (actual == null)`: verifica que exista el punto de inserción.

- `throw new IndexOutOfBoundsException(...)`: explica que la posición excede la estructura disponible.

### Microbloque 9 - Crear el nodo y conservar el enlace que no debemos perder

**Escribe ahora únicamente este microbloque:**

```java
            Nodo nuevo = new Nodo(dato);
            nuevo.next = actual.next;
```

**¿Por qué existe este microbloque?**

Esta es la primera mitad de la inserción segura. El nuevo nodo debe recordar a quién apuntaba `actual` **antes** de que cambiemos `actual.next`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo nuevo = new Nodo(dato);`: crea el objeto que se insertará.

- `nuevo.next = actual.next;`: copia en el nuevo nodo la referencia al nodo que antes venía después de actual.

**Estado conceptual después de escribirlo:**

```text
Antes: actual(3) -> 4 -> 13
Después de nuevo.next = actual.next:
actual(3) -------> 4 -> 13
nuevo(10) ------^
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si ejecutaras primero `actual.next = nuevo`, perderías el acceso directo al antiguo sucesor desde `actual`; luego `nuevo.next = actual.next` produciría un self-loop en nuevo.

### Microbloque 10 - Conectar el nodo anterior con nuevo

**Escribe ahora únicamente este microbloque:**

```java
            actual.next = nuevo;
        }
```

**¿Por qué existe este microbloque?**

Como el nuevo nodo ya conserva la continuación de la cadena, ahora es seguro reemplazar `actual.next` para que apunte al nuevo.

**Explicación línea por línea / instrucción por instrucción:**

- `actual.next = nuevo`: inserta el nuevo nodo entre actual y el sucesor anterior.

- `}`: cierra insertarEnPosicion.

**Estado conceptual después de escribirlo:**

```text
29 -> 3 -> 10 -> 4 -> 13 -> null
```

### Microbloque 11 - Agregar recorrer y main con validación de excepción

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(4);
        lista.insertarFinal(13);

        lista.insertarEnPosicion(10, 2);
        lista.recorrer();

        try {
            lista.insertarEnPosicion(99, 20);
        } catch (IndexOutOfBoundsException e) {
            System.out.println("Validación: " + e.getMessage());
        }
    }
}
```

**¿Por qué existe este microbloque?**

`main` prepara una lista conocida, ejecuta una inserción válida y después prueba una posición inválida dentro de `try/catch` para observar la validación sin terminar abruptamente el programa.

**Explicación línea por línea / instrucción por instrucción:**

- `lista.insertarEnPosicion(10, 2)`: inserta 10 en índice 2.

- `try { ... }`: ejecuta una operación que puede lanzar la excepción prevista.

- `catch (IndexOutOfBoundsException e)`: captura exactamente el tipo esperado.

- `e.getMessage()`: muestra el mensaje que explica la causa.

**No avances hasta poder responder:**

Antes de ejecutar, dibuja dónde debe quedar 10 y cuál nodo debe conservarse como sucesor.

## Auditoría línea por línea de `EJ05_InsertarPosicion.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ05_InsertarPosicion {`**  
Crea la clase pública contenedora del ejemplo `EJ05_InsertarPosicion`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `void insertarInicio(int dato) {`**  
Abre el método `insertarInicio`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 13: `nuevo.next = head;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 14: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 15: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 17: `void insertarFinal(int dato) {`**  
Abre el método `insertarFinal`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 18: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 19: `if (head == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 20: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 21: `return;`**  
Termina el método en este punto porque el caso actual ya quedó resuelto y continuar ejecutando produciría lógica innecesaria o incorrecta.

**Línea 22: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 23: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 24: `while (actual.next != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 25: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 26: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 27: `actual.next = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 28: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 30: `void insertarEnPosicion(int dato, int posicion) {`**  
Abre el método `insertarEnPosicion`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 31: `if (posicion < 0) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 32: `throw new IllegalArgumentException("Posición inválida");`**  
Detiene la operación con una excepción explícita para representar una entrada inválida. La lista no debe modificarse silenciosamente ante esa condición.

**Línea 33: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 34: `if (posicion == 0) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 35: `insertarInicio(dato);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 36: `return;`**  
Termina el método en este punto porque el caso actual ya quedó resuelto y continuar ejecutando produciría lógica innecesaria o incorrecta.

**Línea 37: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 39: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 40: `int contador = 0;`**  
Inicializa el índice lógico del recorrido en cero para mantener sincronizada la posición con el nodo apuntado por `actual`.

**Línea 41: `while (actual != null && contador < posicion - 1) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 42: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 43: `contador++;`**  
Incrementa el índice lógico después de avanzar un nodo, de modo que el contador siga describiendo la posición de `actual`.

**Línea 44: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 46: `if (actual == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 47: `throw new IndexOutOfBoundsException("Posición fuera de rango");`**  
Detiene la operación con una excepción explícita para representar una entrada inválida. La lista no debe modificarse silenciosamente ante esa condición.

**Línea 48: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 50: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 51: `nuevo.next = actual.next;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 52: `actual.next = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 53: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 55: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 56: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 57: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 58: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 59: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 60: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 61: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 62: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 63: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 65: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 66: `ListaSimple lista = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 67: `lista.insertarFinal(29);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 68: `lista.insertarFinal(3);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 69: `lista.insertarFinal(4);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 70: `lista.insertarFinal(13);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 72: `lista.insertarEnPosicion(10, 2);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 73: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 75: `try {`**  
Abre un bloque de prueba para ejecutar una operación que deliberadamente puede producir la excepción validada por el ejemplo.

**Línea 76: `lista.insertarEnPosicion(99, 20);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 77: `} catch (IndexOutOfBoundsException e) {`**  
Captura la excepción prevista para poder mostrar su mensaje y continuar la demostración sin finalizar abruptamente el programa.

**Línea 78: `System.out.println("Validación: " + e.getMessage());`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 79: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 80: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 81: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ05_InsertarPosicion {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void insertarInicio(int dato) {
            Nodo nuevo = new Nodo(dato);
            nuevo.next = head;
            head = nuevo;
        }

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (head == null) {
                head = nuevo;
                return;
            }
            Nodo actual = head;
            while (actual.next != null) {
                actual = actual.next;
            }
            actual.next = nuevo;
        }

        void insertarEnPosicion(int dato, int posicion) {
            if (posicion < 0) {
                throw new IllegalArgumentException("Posición inválida");
            }
            if (posicion == 0) {
                insertarInicio(dato);
                return;
            }

            Nodo actual = head;
            int contador = 0;
            while (actual != null && contador < posicion - 1) {
                actual = actual.next;
                contador++;
            }

            if (actual == null) {
                throw new IndexOutOfBoundsException("Posición fuera de rango");
            }

            Nodo nuevo = new Nodo(dato);
            nuevo.next = actual.next;
            actual.next = nuevo;
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(29);
        lista.insertarFinal(3);
        lista.insertarFinal(4);
        lista.insertarFinal(13);

        lista.insertarEnPosicion(10, 2);
        lista.recorrer();

        try {
            lista.insertarEnPosicion(99, 20);
        } catch (IndexOutOfBoundsException e) {
            System.out.println("Validación: " + e.getMessage());
        }
    }
}
```

## Resultado esperado

```text
29 -> 3 -> 10 -> 4 -> 13 -> null
Validación: Posición fuera de rango
```

## T05 - Tarea espejo

Parte de `8 -> 16 -> 32 -> null`. Inserta 24 en posición 2. Después prueba una posición fuera de rango.

- **Archivo F15:** `estudiante/T05_InsertarPosicion.java`.
- **Criterio:** debes explicar por qué el orden seguro es `nuevo.next = actual.next` antes de `actual.next = nuevo`.



# EJ06 - Diagnosticar un error lógico de inserción

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ06_DiagnosticoInsercion.java` y `estudiante/EJ06_DiagnosticoInsercion.java`.
- **Tarea espejo:** `T06_CorregirInsercionIntermedia.java` - Corregir una inserción intermedia.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ06_DiagnosticoInsercion.java` del laboratorio.

## ¿Por qué existe este ejemplo?

Este ejemplo convierte un error típico en objeto de análisis: cambiar `head` antes de conservar la referencia anterior puede hacer que el nodo nuevo se apunte a sí mismo. La meta es aprender a explicar la causa estructural, no solo reconocer que "no funciona".

## Qué debes conectar con lo anterior

EJ03 enseñó el orden correcto. EJ06 coloca lado a lado una versión con bug y una correcta, y usa un recorrido limitado para diagnosticar de forma segura un ciclo accidental.

## Modelo mental antes de escribir código

```text
BUG:
head = nuevo
nuevo.next = head

Resultado:
head/nuevo -> [10|•]
               ^   |
               +---+

La lista anterior queda desconectada desde head.
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Definir Nodo, ListaSimple y head

**Escribe ahora únicamente este microbloque:**

```java
public class EJ06_DiagnosticoInsercion {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;
```

**¿Por qué existe este microbloque?**

Repetimos la estructura base para que podamos crear dos listas independientes: una con bug y otra corregida.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo`: unidad de la lista.

- `next`: enlace que puede formar una cadena o, por error, un ciclo.

- `head`: entrada de cada instancia de ListaSimple.

### Microbloque 2 - Preparar una lista base controlada

**Escribe ahora únicamente este microbloque:**

```java
        void cargarBase() {
            Nodo n1 = new Nodo(29);
            Nodo n2 = new Nodo(3);
            n1.next = n2;
            head = n1;
        }
```

**¿Por qué existe este microbloque?**

Necesitamos el mismo estado inicial en las dos pruebas para que la única diferencia sea la implementación de la inserción.

**Explicación línea por línea / instrucción por instrucción:**

- `n1`: nodo 29.

- `n2`: nodo 3.

- `n1.next = n2`: crea 29 -> 3.

- `head = n1`: establece 29 como inicio.

**Estado conceptual después de escribirlo:**

```text
head -> 29 -> 3 -> null
```

### Microbloque 3 - Escribir deliberadamente la versión con bug y analizarla línea por línea

**Escribe ahora únicamente este microbloque:**

```java
        void insertarInicioConBug(int dato) {
            Nodo nuevo = new Nodo(dato);
            head = nuevo;
            nuevo.next = head;
        }
```

**¿Por qué existe este microbloque?**

Este método se mantiene como caso de diagnóstico. La línea incorrecta no es `nuevo.next = head` por sí misma; el problema aparece porque **antes** ya cambiamos `head`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo nuevo = new Nodo(dato);`: crea un nodo aislado.

- `head = nuevo;`: mueve head demasiado pronto; desde head ya no se conserva la lista 29 -> 3.

- `nuevo.next = head;`: como head ya apunta a nuevo, esta línea equivale a `nuevo.next = nuevo`.

- `}`: cierra el método con una estructura corrupta.

**Estado conceptual después de escribirlo:**

```text
antes del bug: head -> 29 -> 3 -> null
después de head=nuevo: head/nuevo -> 10 -> null   (29 -> 3 deja de ser alcanzable desde head)
después de nuevo.next=head: 10 -> 10 -> 10 ...
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Este bug puede provocar un recorrido infinito si usamos un while que espera llegar a null.

### Microbloque 4 - Escribir la versión correcta y comparar el orden

**Escribe ahora únicamente este microbloque:**

```java
        void insertarInicioCorrecto(int dato) {
            Nodo nuevo = new Nodo(dato);
            nuevo.next = head;
            head = nuevo;
        }
```

**¿Por qué existe este microbloque?**

La corrección conserva primero la referencia anterior y solo después cambia la entrada. Son las mismas dos asignaciones conceptuales de EJ03, pero el orden cambia completamente el grafo de referencias.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo nuevo = new Nodo(dato)`: crea nodo aislado.

- `nuevo.next = head`: conserva la entrada anterior dentro del nuevo nodo.

- `head = nuevo`: mueve la entrada cuando la continuidad ya está protegida.

**Estado conceptual después de escribirlo:**

```text
head -> 10 -> 29 -> 3 -> null
```

### Microbloque 5 - Crear una prueba booleana del self-loop

**Escribe ahora únicamente este microbloque:**

```java
        boolean headSeApuntaASiMismo() {
            return head != null && head.next == head;
        }
```

**¿Por qué existe este microbloque?**

Antes de recorrer, podemos probar directamente la propiedad del bug. La comparación `head.next == head` pregunta si ambas referencias apuntan al mismo objeto.

**Explicación línea por línea / instrucción por instrucción:**

- `boolean`: el método devuelve true o false.

- `head != null`: evita leer `head.next` si no existe cabeza.

- `&&`: solo evalúa la segunda condición si la primera es verdadera.

- `head.next == head`: compara identidad de referencias; true significa self-loop en la cabeza.

**No avances hasta poder responder:**

¿Estamos comparando datos o referencias? Referencias. Dos nodos podrían tener el mismo dato sin ser el mismo objeto.

### Microbloque 6 - Crear un recorrido limitado para diagnosticar sin colgar el programa

**Escribe ahora únicamente este microbloque:**

```java
        void recorrerLimitado(int maxNodos) {
            Nodo actual = head;
            int visitados = 0;
            while (actual != null && visitados < maxNodos) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
                visitados++;
            }
            System.out.println(actual == null ? "null" : "... ciclo detectado");
        }
    }
```

**¿Por qué existe este microbloque?**

Una lista con ciclo no llegará a `null`. El segundo límite `visitados < maxNodos` hace segura la demostración: evita un bucle infinito y permite mostrar que la misma referencia se repite.

**Explicación línea por línea / instrucción por instrucción:**

- `int visitados = 0`: contador de seguridad.

- `actual != null && visitados < maxNodos`: permite detener por fin normal o por límite.

- `visitados++`: asegura avance del contador aunque las referencias formen ciclo.

- `actual == null ? ...`: si no llegamos a null dentro del límite, mostramos indicio de ciclo.

**Qué error evita o qué ocurriría si se omite/cambia:**

No elimines el límite al ejecutar la versión con bug; podrías crear un ciclo infinito en la demostración.

### Microbloque 7 - Comparar dos instancias desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaSimple conBug = new ListaSimple();
        conBug.cargarBase();
        conBug.insertarInicioConBug(10);
        System.out.println("Self-loop: " + conBug.headSeApuntaASiMismo());
        conBug.recorrerLimitado(4);

        ListaSimple corregida = new ListaSimple();
        corregida.cargarBase();
        corregida.insertarInicioCorrecto(10);
        corregida.recorrerLimitado(4);
    }
}
```

**¿Por qué existe este microbloque?**

Dos objetos `ListaSimple` permiten comparar resultados sin contaminar una prueba con la otra. Ambos comienzan con la misma base; solo cambia el método de inserción.

**Explicación línea por línea / instrucción por instrucción:**

- `ListaSimple conBug`: instancia destinada al error.

- `insertarInicioConBug(10)`: produce self-loop.

- `headSeApuntaASiMismo()`: confirma directamente la causa.

- `ListaSimple corregida`: segunda instancia limpia.

- `insertarInicioCorrecto(10)`: aplica orden seguro.

## Auditoría línea por línea de `EJ06_DiagnosticoInsercion.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ06_DiagnosticoInsercion {`**  
Crea la clase pública contenedora del ejemplo `EJ06_DiagnosticoInsercion`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `void cargarBase() {`**  
Abre el método `cargarBase`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo n1 = new Nodo(29);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 13: `Nodo n2 = new Nodo(3);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 14: `n1.next = n2;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 15: `head = n1;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 16: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 18: `void insertarInicioConBug(int dato) {`**  
Abre el método `insertarInicioConBug`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 19: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 20: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 21: `nuevo.next = head;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 22: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 24: `void insertarInicioCorrecto(int dato) {`**  
Abre el método `insertarInicioCorrecto`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 25: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 26: `nuevo.next = head;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 27: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 28: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 30: `boolean headSeApuntaASiMismo() {`**  
Abre el método `headSeApuntaASiMismo`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 31: `return head != null && head.next == head;`**  
Evalúa una condición de control usada para decidir si continuar, detener, insertar, rechazar o validar el estado actual de la lista.

**Línea 32: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 34: `void recorrerLimitado(int maxNodos) {`**  
Abre el método `recorrerLimitado`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 35: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 36: `int visitados = 0;`**  
Inicializa un contador de seguridad. En el ejemplo de diagnóstico evita que un ciclo accidental produzca un recorrido infinito.

**Línea 37: `while (actual != null && visitados < maxNodos) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 38: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 39: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 40: `visitados++;`**  
Incrementa el límite de seguridad del recorrido para garantizar que la demostración termine aunque exista un ciclo.

**Línea 41: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 42: `System.out.println(actual == null ? "null" : "... ciclo detectado");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 43: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 44: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 46: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 47: `ListaSimple conBug = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 48: `conBug.cargarBase();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 49: `conBug.insertarInicioConBug(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 50: `System.out.println("Self-loop: " + conBug.headSeApuntaASiMismo());`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 51: `conBug.recorrerLimitado(4);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 53: `ListaSimple corregida = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 54: `corregida.cargarBase();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 55: `corregida.insertarInicioCorrecto(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 56: `corregida.recorrerLimitado(4);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 57: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 58: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ06_DiagnosticoInsercion {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void cargarBase() {
            Nodo n1 = new Nodo(29);
            Nodo n2 = new Nodo(3);
            n1.next = n2;
            head = n1;
        }

        void insertarInicioConBug(int dato) {
            Nodo nuevo = new Nodo(dato);
            head = nuevo;
            nuevo.next = head;
        }

        void insertarInicioCorrecto(int dato) {
            Nodo nuevo = new Nodo(dato);
            nuevo.next = head;
            head = nuevo;
        }

        boolean headSeApuntaASiMismo() {
            return head != null && head.next == head;
        }

        void recorrerLimitado(int maxNodos) {
            Nodo actual = head;
            int visitados = 0;
            while (actual != null && visitados < maxNodos) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
                visitados++;
            }
            System.out.println(actual == null ? "null" : "... ciclo detectado");
        }
    }

    public static void main(String[] args) {
        ListaSimple conBug = new ListaSimple();
        conBug.cargarBase();
        conBug.insertarInicioConBug(10);
        System.out.println("Self-loop: " + conBug.headSeApuntaASiMismo());
        conBug.recorrerLimitado(4);

        ListaSimple corregida = new ListaSimple();
        corregida.cargarBase();
        corregida.insertarInicioCorrecto(10);
        corregida.recorrerLimitado(4);
    }
}
```

## Resultado esperado

```text
Self-loop: true
10 -> 10 -> 10 -> 10 -> ... ciclo detectado
10 -> 29 -> 3 -> null
```

## T06 - Tarea espejo

En una cadena `10 -> 20`, inserta 15 después del primer nodo. El bug propuesto invierte las dos asignaciones de enlaces. Corrígelo **sin reconstruir la lista**.

- **Archivo F15:** `estudiante/T06_CorregirInsercionIntermedia.java`.
- **Evidencia:** recorrido limitado + comprobación de que el nuevo nodo no se apunta a sí mismo.



# EJ07 - Buscar y eliminar en una lista simple

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ07_BuscarEliminar.java` y `estudiante/EJ07_BuscarEliminar.java`.
- **Tarea espejo:** `T07_BuscarYEliminar.java` - Buscar y eliminar valores.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ07_BuscarEliminar.java` del laboratorio.

## ¿Por qué existe este ejemplo?

La búsqueda practica el recorrido con una condición adicional. La eliminación añade una segunda referencia, `anterior`, porque para retirar un nodo intermedio necesitamos modificar el enlace del nodo que lo precede.

## Qué debes conectar con lo anterior

Ya sabes recorrer con `actual`. Aquí aprenderás por qué una eliminación intermedia requiere recordar tanto el nodo actual como el anterior, y por qué eliminar `head` es un caso especial.

## Modelo mental antes de escribir código

```text
Búsqueda: actual avanza hasta valor o null

Eliminación intermedia:
anterior -> [20|•] -> actual [30|•] -> [40|null]
Se cambia: anterior.next = actual.next
Resultado: 20 -> 40
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Definir Nodo, ListaSimple y método de carga insertarFinal

**Escribe ahora únicamente este microbloque:**

```java
public class EJ07_BuscarEliminar {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (head == null) { head = nuevo; return; }
            Nodo actual = head;
            while (actual.next != null) actual = actual.next;
            actual.next = nuevo;
        }
```

**¿Por qué existe este microbloque?**

La inserción al final ya fue explicada en EJ04. Aquí funciona como infraestructura para construir rápidamente la lista sobre la que practicaremos búsqueda y eliminación.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo head`: entrada a la lista.

- `insertarFinal`: carga datos de prueba manteniendo el orden de las llamadas.

- `if (head == null)`: caso vacío.

- `while (actual.next != null)`: encuentra el último nodo.

- `actual.next = nuevo`: conecta el nuevo.

### Microbloque 2 - Abrir buscar y crear el cursor

**Escribe ahora únicamente este microbloque:**

```java
        boolean buscar(int valor) {
            Nodo actual = head;
```

**¿Por qué existe este microbloque?**

Buscar no debe cambiar la lista. `actual` es un cursor local; `valor` es el dato que queremos localizar.

**Explicación línea por línea / instrucción por instrucción:**

- `boolean buscar(int valor)`: promete devolver true si encuentra el valor y false si no.

- `Nodo actual = head`: comienza en el primer nodo sin mover head.

### Microbloque 3 - Recorrer y comparar el dato del nodo actual

**Escribe ahora únicamente este microbloque:**

```java
            while (actual != null) {
                if (actual.dato == valor) return true;
                actual = actual.next;
            }
            return false;
        }
```

**¿Por qué existe este microbloque?**

En cada nodo hacemos una pregunta: ¿su dato coincide con `valor`? Si sí, no hace falta seguir. Si no, avanzamos. Solo devolvemos false después de llegar a `null`.

**Explicación línea por línea / instrucción por instrucción:**

- `while (actual != null)`: continúa mientras haya nodo.

- `if (actual.dato == valor)`: compara el dato, no la referencia.

- `return true`: termina inmediatamente cuando encuentra coincidencia.

- `actual = actual.next`: avanza si no encontró.

- `return false`: se ejecuta únicamente después de agotar la lista.

**Estado conceptual después de escribirlo:**

```text
buscar(30) en 10->20->30->40:
10 != 30 -> avanza
20 != 30 -> avanza
30 == 30 -> true
```

### Microbloque 4 - Preparar actual y anterior para eliminar

**Escribe ahora únicamente este microbloque:**

```java
        void eliminar(int valor) {
            Nodo actual = head;
            Nodo anterior = null;
```

**¿Por qué existe este microbloque?**

Eliminar necesita conocer el nodo objetivo (`actual`) y, si el objetivo no es la cabeza, el nodo que lo precede (`anterior`). Al comenzar no existe un nodo anterior a head, por eso `anterior = null`.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head`: candidato actual empieza en la cabeza.

- `Nodo anterior = null`: marca que todavía no hemos avanzado y, por tanto, el actual podría ser la cabeza.

**Estado conceptual después de escribirlo:**

```text
anterior = null
actual = head
```

### Microbloque 5 - Avanzar ambos cursores hasta encontrar el valor

**Escribe ahora únicamente este microbloque:**

```java
            while (actual != null && actual.dato != valor) {
                anterior = actual;
                actual = actual.next;
            }
```

**¿Por qué existe este microbloque?**

Antes de mover `actual`, guardamos el nodo actual en `anterior`. Así, cuando `actual` avance, conservamos una referencia al nodo que quedó justo detrás.

**Explicación línea por línea / instrucción por instrucción:**

- `actual != null`: no continuar después del final.

- `actual.dato != valor`: detener cuando encontramos el objetivo.

- `anterior = actual`: el nodo actual pasa a ser el anterior de la próxima posición.

- `actual = actual.next`: avanza al siguiente candidato.

**Estado conceptual después de escribirlo:**

```text
Buscando 30:
inicio: anterior=null, actual=10
1: anterior=10, actual=20
2: anterior=20, actual=30 -> termina
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si avanzaras `actual` antes de asignar `anterior`, podrías perder la referencia exacta al nodo que debe modificar su next.

### Microbloque 6 - Resolver el caso "no encontrado"

**Escribe ahora únicamente este microbloque:**

```java
            if (actual == null) return;
```

**¿Por qué existe este microbloque?**

Si llegamos a `null`, el valor no existe. No hay referencia que cambiar y la lista debe permanecer intacta.

**Explicación línea por línea / instrucción por instrucción:**

- `if (actual == null) return;`: abandona el método sin cambios cuando no se encontró el valor.

### Microbloque 7 - Diferenciar eliminación de head y eliminación intermedia

**Escribe ahora únicamente este microbloque:**

```java
            if (anterior == null) {
                head = actual.next;
            } else {
                anterior.next = actual.next;
            }
        }
```

**¿Por qué existe este microbloque?**

`anterior == null` solo puede ocurrir si nunca avanzamos: el nodo encontrado es `head`. En ese caso movemos la cabeza. Si existe `anterior`, saltamos el nodo objetivo enlazando el anterior con el sucesor del actual.

**Explicación línea por línea / instrucción por instrucción:**

- `if (anterior == null)`: detecta eliminación de la cabeza.

- `head = actual.next`: la lista comienza ahora en el segundo nodo.

- `else`: caso de nodo no inicial.

- `anterior.next = actual.next`: omite `actual` de la cadena sin tener que mover los demás nodos.

**Estado conceptual después de escribirlo:**

```text
Eliminar 30 de 10->20->30->40:
anterior=20, actual=30
20.next = 40
Resultado: 10->20->40
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si intentaras `anterior.next = ...` cuando eliminas head, `anterior` es null y obtendrías `NullPointerException`.

### Microbloque 8 - Agregar recorrido y prueba desde main

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.insertarFinal(40);

        System.out.println("Buscar 30: " + lista.buscar(30));
        System.out.println("Buscar 99: " + lista.buscar(99));

        lista.eliminar(10);
        lista.eliminar(30);
        lista.recorrer();
    }
}
```

**¿Por qué existe este microbloque?**

`main` prueba dos resultados de búsqueda y las dos rutas de eliminación: primero elimina la cabeza 10 y luego elimina un nodo intermedio 30.

**Explicación línea por línea / instrucción por instrucción:**

- `buscar(30)`: debe devolver true.

- `buscar(99)`: debe recorrer hasta null y devolver false.

- `eliminar(10)`: ejercita anterior == null.

- `eliminar(30)`: ejercita anterior != null.

- `recorrer()`: verifica que queden 20 y 40.

## Auditoría línea por línea de `EJ07_BuscarEliminar.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ07_BuscarEliminar {`**  
Crea la clase pública contenedora del ejemplo `EJ07_BuscarEliminar`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaSimple {`**  
Crea la clase que representa una lista completa, separada del nodo individual. Existe para conservar `head` como estado y agrupar las operaciones que modifican o recorren la lista.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `void insertarFinal(int dato) {`**  
Abre el método `insertarFinal`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 13: `if (head == null) { head = nuevo; return; }`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 14: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 15: `while (actual.next != null) actual = actual.next;`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 16: `actual.next = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 17: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 19: `boolean buscar(int valor) {`**  
Abre el método `buscar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 20: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 21: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 22: `if (actual.dato == valor) return true;`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 23: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 24: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 25: `return false;`**  
Finaliza el método informando que la condición buscada no se cumple o que el dato fue rechazado, por ejemplo por duplicidad.

**Línea 26: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 28: `void eliminar(int valor) {`**  
Abre el método `eliminar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 29: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 30: `Nodo anterior = null;`**  
Inicializa `anterior` en null porque, al comenzar en la cabeza, todavía no existe un nodo previo. Esta diferencia permitirá reconocer el caso de eliminar head.

**Línea 32: `while (actual != null && actual.dato != valor) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 33: `anterior = actual;`**  
Realiza una asignación. En este laboratorio toda asignación debe leerse preguntando qué variable o referencia cambia y qué valor o objeto queda accesible después.

**Línea 34: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 35: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 37: `if (actual == null) return;`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 39: `if (anterior == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 40: `head = actual.next;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 41: `} else {`**  
Abre la alternativa de la condición anterior. Se ejecuta únicamente cuando el caso previo no se cumple.

**Línea 42: `anterior.next = actual.next;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 43: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 44: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 46: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 47: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 48: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 49: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 50: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 51: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 52: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 53: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 54: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 56: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 57: `ListaSimple lista = new ListaSimple();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 58: `lista.insertarFinal(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 59: `lista.insertarFinal(20);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 60: `lista.insertarFinal(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 61: `lista.insertarFinal(40);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 63: `System.out.println("Buscar 30: " + lista.buscar(30));`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 64: `System.out.println("Buscar 99: " + lista.buscar(99));`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 66: `lista.eliminar(10);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 67: `lista.eliminar(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 68: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 69: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 70: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ07_BuscarEliminar {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaSimple {
        Nodo head;

        void insertarFinal(int dato) {
            Nodo nuevo = new Nodo(dato);
            if (head == null) { head = nuevo; return; }
            Nodo actual = head;
            while (actual.next != null) actual = actual.next;
            actual.next = nuevo;
        }

        boolean buscar(int valor) {
            Nodo actual = head;
            while (actual != null) {
                if (actual.dato == valor) return true;
                actual = actual.next;
            }
            return false;
        }

        void eliminar(int valor) {
            Nodo actual = head;
            Nodo anterior = null;

            while (actual != null && actual.dato != valor) {
                anterior = actual;
                actual = actual.next;
            }

            if (actual == null) return;

            if (anterior == null) {
                head = actual.next;
            } else {
                anterior.next = actual.next;
            }
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaSimple lista = new ListaSimple();
        lista.insertarFinal(10);
        lista.insertarFinal(20);
        lista.insertarFinal(30);
        lista.insertarFinal(40);

        System.out.println("Buscar 30: " + lista.buscar(30));
        System.out.println("Buscar 99: " + lista.buscar(99));

        lista.eliminar(10);
        lista.eliminar(30);
        lista.recorrer();
    }
}
```

## Resultado esperado

```text
Buscar 30: true
Buscar 99: false
20 -> 40 -> null
```

## T07 - Tarea espejo

Construye `5 -> 15 -> 25 -> 35`. Busca 25 y 100; elimina 35 y muestra la lista final.

- **Archivo F15:** `estudiante/T07_BuscarYEliminar.java`.
- **Criterio:** explica qué valores tienen `actual` y `anterior` justo antes de eliminar.



# EJ08 - Comparar lista doble, circular y doblemente circular

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ08_CompararVariantes.java` y `estudiante/EJ08_CompararVariantes.java`.
- **Tarea espejo:** `T08_ValidarVariantes.java` - Validar enlaces de las tres variantes.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ08_CompararVariantes.java` del laboratorio.

## ¿Por qué existe este ejemplo?

La sesión presenta cuatro clasificaciones. Ya dominamos la lista simple; este ejemplo implementa tres variantes para que la comparación no sea solo visual: la doble añade `anterior`, la circular reemplaza el final `null` por un enlace a `cabeza`, y la doble circular combina ambas ideas.

## Qué debes conectar con lo anterior

Este ejemplo es más largo porque contiene tres mini-estructuras. No debes leerlo como un bloque monolítico. Se construye una variante completa, se valida su cierre y recién después se pasa a la siguiente.

## Modelo mental antes de escribir código

```text
DOBLE:      null <- 10 <-> 20 <-> 30 -> null
CIRCULAR:        10 -> 20 -> 30 --+
                 ^               |
                 +---------------+
DOBLE CIRCULAR: enlaces siguiente y anterior cierran el ciclo
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Crear NodoDoble: por qué necesita dos referencias

**Escribe ahora únicamente este microbloque:**

```java
public class EJ08_CompararVariantes {
    static class NodoDoble {
        int dato;
        NodoDoble siguiente;
        NodoDoble anterior;
        NodoDoble(int dato) { this.dato = dato; }
    }
```

**¿Por qué existe este microbloque?**

Una lista doble debe permitir avanzar y retroceder. Por eso cada nodo no puede tener solo una referencia; necesita `siguiente` y `anterior`.

**Explicación línea por línea / instrucción por instrucción:**

- `NodoDoble siguiente`: enlace hacia adelante.

- `NodoDoble anterior`: enlace hacia atrás.

- `NodoDoble(int dato)`: constructor que guarda el dato; ambas referencias comienzan en null.

**Estado conceptual después de escribirlo:**

```text
Nodo doble aislado: null <- [dato] -> null
```

### Microbloque 2 - Crear ListaDoble con cabeza y cola

**Escribe ahora únicamente este microbloque:**

```java
    static class ListaDoble {
        NodoDoble cabeza;
        NodoDoble cola;
```

**¿Por qué existe este microbloque?**

En una lista doble resulta útil conservar ambos extremos en este ejemplo: `cabeza` para comenzar el recorrido y `cola` para agregar al final sin recorrer toda la lista.

**Explicación línea por línea / instrucción por instrucción:**

- `NodoDoble cabeza`: primer nodo.

- `NodoDoble cola`: último nodo.

**Estado conceptual después de escribirlo:**

```text
lista vacía: cabeza=null, cola=null
```

### Microbloque 3 - Agregar el primer nodo de la lista doble

**Escribe ahora únicamente este microbloque:**

```java
        void agregar(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
```

**¿Por qué existe este microbloque?**

Cuando la lista está vacía, el mismo nodo es simultáneamente primero y último. La asignación encadenada hace que ambas referencias apunten al mismo objeto.

**Explicación línea por línea / instrucción por instrucción:**

- `NodoDoble nuevo = new NodoDoble(dato)`: crea nodo aislado.

- `if (cabeza == null)`: detecta lista vacía.

- `cabeza = cola = nuevo`: cabeza y cola apuntan al mismo nodo.

**Estado conceptual después de escribirlo:**

```text
cabeza/cola -> [nuevo]
anterior=null, siguiente=null
```

### Microbloque 4 - Enlazar un nuevo nodo al final de la lista doble

**Escribe ahora únicamente este microbloque:**

```java
            } else {
                cola.siguiente = nuevo;
                nuevo.anterior = cola;
                cola = nuevo;
            }
        }
```

**¿Por qué existe este microbloque?**

Una inserción doble necesita mantener consistentes las dos direcciones. Primero el antiguo último apunta al nuevo; luego el nuevo apunta hacia atrás al antiguo último; por último `cola` se mueve.

**Explicación línea por línea / instrucción por instrucción:**

- `cola.siguiente = nuevo`: crea el enlace hacia adelante.

- `nuevo.anterior = cola`: crea el enlace de regreso.

- `cola = nuevo`: actualiza cuál es el último nodo.

**Estado conceptual después de escribirlo:**

```text
cabeza -> [10] <-> [20] <- cola
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Si omites `nuevo.anterior = cola`, el recorrido hacia adelante funcionaría, pero el retroceso quedaría roto.

### Microbloque 5 - Recorrer la lista doble hacia adelante

**Escribe ahora únicamente este microbloque:**

```java
        void mostrarAdelante() {
            NodoDoble actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " <-> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }
```

**¿Por qué existe este microbloque?**

Aunque el nodo tiene dos enlaces, este método decide usar `siguiente`. El recorrido termina en `null` porque la lista doble presentada no es circular.

**Explicación línea por línea / instrucción por instrucción:**

- `actual = cabeza`: comienza en el primer nodo.

- `actual = actual.siguiente`: avanza hacia adelante.

- `actual != null`: final lineal.

### Microbloque 6 - Crear NodoCircular y ListaCircular

**Escribe ahora únicamente este microbloque:**

```java
    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }

    static class ListaCircular {
        NodoCircular cabeza;
        NodoCircular cola;
```

**¿Por qué existe este microbloque?**

La circular simple vuelve a usar una sola referencia `siguiente`. La diferencia no está en el número de campos, sino en **cómo se enlaza el último nodo**: su `siguiente` no será null; apuntará a `cabeza`.

**Explicación línea por línea / instrucción por instrucción:**

- `NodoCircular siguiente`: único enlace hacia adelante.

- `cabeza`: primer nodo.

- `cola`: último nodo, necesario para cerrar el ciclo fácilmente.

**No avances hasta poder responder:**

¿Qué diferencia estructural tendrá el último nodo respecto a una lista simple? Su siguiente apuntará a cabeza, no a null.

### Microbloque 7 - Crear el primer ciclo de un solo nodo

**Escribe ahora únicamente este microbloque:**

```java
        void agregar(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
                nuevo.siguiente = cabeza;
```

**¿Por qué existe este microbloque?**

En una circular incluso una lista de un solo nodo debe estar cerrada. Por eso, después de hacer que `cabeza` y `cola` sean el nuevo nodo, hacemos que `nuevo.siguiente` vuelva a `cabeza`, es decir, al propio nodo.

**Explicación línea por línea / instrucción por instrucción:**

- `cabeza = cola = nuevo`: el único nodo es ambos extremos lógicos.

- `nuevo.siguiente = cabeza`: cierra el ciclo de un nodo.

**Estado conceptual después de escribirlo:**

```text
cabeza/cola -> [10]
                ^   |
                +---+
```

### Microbloque 8 - Agregar nodos a la circular y volver a cerrar el ciclo

**Escribe ahora únicamente este microbloque:**

```java
            } else {
                cola.siguiente = nuevo;
                cola = nuevo;
                cola.siguiente = cabeza;
            }
        }
```

**¿Por qué existe este microbloque?**

Al agregar otro nodo, el antiguo último deja de apuntar directamente a cabeza y pasa a apuntar al nuevo. Después movemos `cola` y cerramos nuevamente el ciclo desde la nueva cola hacia cabeza.

**Explicación línea por línea / instrucción por instrucción:**

- `cola.siguiente = nuevo`: inserta el nuevo después del antiguo último.

- `cola = nuevo`: actualiza la referencia de último.

- `cola.siguiente = cabeza`: restaura el cierre circular.

**Estado conceptual después de escribirlo:**

```text
cabeza -> 10 -> 20 -> 30 --+
          ^                    |
          +--------------------+
```

### Microbloque 9 - Recorrer circular con do-while

**Escribe ahora únicamente este microbloque:**

```java
        void mostrar() {
            if (cabeza == null) { System.out.println("vacía"); return; }
            NodoCircular actual = cabeza;
            do {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            } while (actual != cabeza);
            System.out.println("(cabeza)");
        }
    }
```

**¿Por qué existe este microbloque?**

Una lista circular no llega a null. Por eso la condición de fin cambia: debemos detenernos cuando el cursor regrese a la cabeza. `do-while` permite procesar la cabeza una vez antes de comprobar el retorno.

**Explicación línea por línea / instrucción por instrucción:**

- `if (cabeza == null)`: resuelve lista vacía antes de usar do-while.

- `NodoCircular actual = cabeza`: punto de partida.

- `do {`: garantiza al menos una visita.

- `actual = actual.siguiente`: avanza.

- `while (actual != cabeza)`: termina cuando completó una vuelta.

**Qué error evita o qué ocurriría si se omite/cambia:**

Usar `while (actual != null)` en una circular bien formada no terminaría porque ningún next es null.

### Microbloque 10 - Crear NodoDobleCircular y sus dos enlaces

**Escribe ahora únicamente este microbloque:**

```java
    static class NodoDobleCircular {
        int dato;
        NodoDobleCircular siguiente;
        NodoDobleCircular anterior;
        NodoDobleCircular(int dato) { this.dato = dato; }
    }

    static class ListaDobleCircular {
        NodoDobleCircular cabeza;
        NodoDobleCircular cola;
```

**¿Por qué existe este microbloque?**

Esta variante combina los dos criterios anteriores: cada nodo conoce anterior y siguiente, y además los extremos se conectan entre sí.

**Explicación línea por línea / instrucción por instrucción:**

- `siguiente`: avance.

- `anterior`: retroceso.

- `cabeza / cola`: permiten cerrar ambos extremos.

### Microbloque 11 - Cerrar ambas direcciones en una lista doble circular de un nodo

**Escribe ahora únicamente este microbloque:**

```java
        void agregar(int dato) {
            NodoDobleCircular nuevo = new NodoDobleCircular(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
                nuevo.siguiente = nuevo;
                nuevo.anterior = nuevo;
```

**¿Por qué existe este microbloque?**

Con un solo nodo, tanto avanzar como retroceder debe regresar al mismo nodo. Por eso `siguiente` y `anterior` apuntan a `nuevo`.

**Explicación línea por línea / instrucción por instrucción:**

- `cabeza = cola = nuevo`: mismo nodo en ambos extremos lógicos.

- `nuevo.siguiente = nuevo`: cierre hacia adelante.

- `nuevo.anterior = nuevo`: cierre hacia atrás.

**Estado conceptual después de escribirlo:**

```text
+-----------+
      |           |
      v           |
    [ nuevo ]-----+
      ^           |
      +-----------+
```

### Microbloque 12 - Insertar en doble circular manteniendo cuatro relaciones

**Escribe ahora únicamente este microbloque:**

```java
            } else {
                nuevo.anterior = cola;
                nuevo.siguiente = cabeza;
                cola.siguiente = nuevo;
                cabeza.anterior = nuevo;
                cola = nuevo;
            }
        }
```

**¿Por qué existe este microbloque?**

Esta es la parte de mayor densidad. Antes de mover `cola`, el nuevo nodo debe saber quién era la cola y quién es la cabeza; luego los extremos existentes se actualizan para apuntar al nuevo; al final `cola` cambia.

**Explicación línea por línea / instrucción por instrucción:**

- `nuevo.anterior = cola`: el nuevo puede volver al antiguo último.

- `nuevo.siguiente = cabeza`: el nuevo puede avanzar al primero.

- `cola.siguiente = nuevo`: el antiguo último ahora avanza al nuevo.

- `cabeza.anterior = nuevo`: el primero ahora retrocede al nuevo último.

- `cola = nuevo`: finalmente actualiza la referencia de cola.

**Estado conceptual después de escribirlo:**

```text
cabeza.anterior == cola
cola.siguiente == cabeza
Cada nodo mantiene siguiente y anterior
```

**Qué error evita o qué ocurriría si se omite/cambia:**

Omitir cualquiera de las dos conexiones de cierre puede producir una estructura parcialmente circular: una dirección funcionaría y la otra quedaría inconsistente.

### Microbloque 13 - Recorrer doble circular y cerrar las clases

**Escribe ahora únicamente este microbloque:**

```java
        void mostrar() {
            if (cabeza == null) { System.out.println("vacía"); return; }
            NodoDobleCircular actual = cabeza;
            do {
                System.out.print(actual.dato + " <-> ");
                actual = actual.siguiente;
            } while (actual != cabeza);
            System.out.println("(cabeza)");
        }
    }
```

**¿Por qué existe este microbloque?**

El recorrido hacia adelante usa la misma condición circular que la lista circular simple. La existencia de `anterior` no obliga a usarlo en esta demostración; su presencia se valida en la estructura de enlaces.

**Explicación línea por línea / instrucción por instrucción:**

- `do-while`: recorre exactamente una vuelta.

- `actual.siguiente`: elige dirección hacia adelante.

- `actual != cabeza`: detiene al regresar al inicio.

### Microbloque 14 - Ejecutar las tres variantes desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaDoble doble = new ListaDoble();
        doble.agregar(10); doble.agregar(20); doble.agregar(30);
        System.out.print("Doble: ");
        doble.mostrarAdelante();

        ListaCircular circular = new ListaCircular();
        circular.agregar(10); circular.agregar(20); circular.agregar(30);
        System.out.print("Circular: ");
        circular.mostrar();

        ListaDobleCircular dobleCircular = new ListaDobleCircular();
        dobleCircular.agregar(10); dobleCircular.agregar(20); dobleCircular.agregar(30);
        System.out.print("Doble circular: ");
        dobleCircular.mostrar();
    }
}
```

**¿Por qué existe este microbloque?**

El `main` usa los mismos datos 10, 20 y 30 para que la comparación cambie solo por estructura, no por contenido.

**Explicación línea por línea / instrucción por instrucción:**

- `ListaDoble doble`: prueba terminación en null con enlaces anterior/siguiente.

- `ListaCircular circular`: prueba cierre último -> cabeza.

- `ListaDobleCircular dobleCircular`: prueba cierre en ambas direcciones.

**No avances hasta poder responder:**

Antes de ejecutar, indica qué variante debe imprimir `null` y cuáles deben indicar regreso a cabeza.

## Auditoría línea por línea de `EJ08_CompararVariantes.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ08_CompararVariantes {`**  
Crea la clase pública contenedora del ejemplo `EJ08_CompararVariantes`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class NodoDoble {`**  
Define el tipo de nodo doble. Se crea porque esta variante necesita poder avanzar y retroceder entre nodos.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `NodoDoble siguiente;`**  
Declara la referencia `siguiente` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `NodoDoble anterior;`**  
Declara la referencia `anterior`, necesaria para poder retroceder en una lista doble o doblemente circular.

**Línea 6: `NodoDoble(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 7: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 9: `static class ListaDoble {`**  
Crea la clase que representa la lista doble completa. Agrupa referencias a los extremos y los métodos que actualizan enlaces en ambas direcciones.

**Línea 10: `NodoDoble cabeza;`**  
Declara `cabeza` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `NodoDoble cola;`**  
Declara `cola` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 13: `void agregar(int dato) {`**  
Abre el método `agregar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 14: `NodoDoble nuevo = new NodoDoble(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 15: `if (cabeza == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 16: `cabeza = cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 17: `} else {`**  
Abre la alternativa de la condición anterior. Se ejecuta únicamente cuando el caso previo no se cumple.

**Línea 18: `cola.siguiente = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 19: `nuevo.anterior = cola;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 20: `cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 21: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 22: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 24: `void mostrarAdelante() {`**  
Abre el método `mostrarAdelante`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 25: `NodoDoble actual = cabeza;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 26: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 27: `System.out.print(actual.dato + " <-> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 28: `actual = actual.siguiente;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 29: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 30: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 31: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 32: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 34: `static class NodoCircular {`**  
Define el tipo de nodo de la variante circular. Mantiene un enlace `siguiente`; la circularidad aparecerá por cómo se conecta el último nodo con la cabeza.

**Línea 35: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 36: `NodoCircular siguiente;`**  
Declara la referencia `siguiente` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 37: `NodoCircular(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 38: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 40: `static class ListaCircular {`**  
Crea la clase que administra la lista circular. Existe para conservar `cabeza`/`cola` y encapsular las operaciones que mantienen el ciclo.

**Línea 41: `NodoCircular cabeza;`**  
Declara `cabeza` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 42: `NodoCircular cola;`**  
Declara `cola` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 44: `void agregar(int dato) {`**  
Abre el método `agregar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 45: `NodoCircular nuevo = new NodoCircular(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 46: `if (cabeza == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 47: `cabeza = cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 48: `nuevo.siguiente = cabeza;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 49: `} else {`**  
Abre la alternativa de la condición anterior. Se ejecuta únicamente cuando el caso previo no se cumple.

**Línea 50: `cola.siguiente = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 51: `cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 52: `cola.siguiente = cabeza;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 53: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 54: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 56: `void mostrar() {`**  
Abre el método `mostrar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 57: `if (cabeza == null) { System.out.println("vacía"); return; }`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 58: `NodoCircular actual = cabeza;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 59: `do {`**  
Abre un `do-while`. Se usa en las listas circulares porque necesitamos procesar la cabeza al menos una vez antes de comprobar si el recorrido regresó a ella.

**Línea 60: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 61: `actual = actual.siguiente;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 62: `} while (actual != cabeza);`**  
Cierra el `do-while` y expresa la condición de terminación. En una lista circular se detiene al volver a `cabeza`, no al encontrar `null`.

**Línea 63: `System.out.println("(cabeza)");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 64: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 65: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 67: `static class NodoDobleCircular {`**  
Define el tipo de nodo de una lista doblemente circular. Debe existir porque esta variante necesita referencias tanto al siguiente como al anterior y luego cerrará ambos extremos.

**Línea 68: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 69: `NodoDobleCircular siguiente;`**  
Declara la referencia `siguiente` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 70: `NodoDobleCircular anterior;`**  
Declara la referencia `anterior`, necesaria para poder retroceder en una lista doble o doblemente circular.

**Línea 71: `NodoDobleCircular(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 72: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 74: `static class ListaDobleCircular {`**  
Crea la clase que administra el estado de la lista doblemente circular: extremos y operaciones. Separa el concepto de nodo individual del concepto de estructura completa.

**Línea 75: `NodoDobleCircular cabeza;`**  
Declara `cabeza` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 76: `NodoDobleCircular cola;`**  
Declara `cola` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 78: `void agregar(int dato) {`**  
Abre el método `agregar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 79: `NodoDobleCircular nuevo = new NodoDobleCircular(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 80: `if (cabeza == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 81: `cabeza = cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 82: `nuevo.siguiente = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 83: `nuevo.anterior = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 84: `} else {`**  
Abre la alternativa de la condición anterior. Se ejecuta únicamente cuando el caso previo no se cumple.

**Línea 85: `nuevo.anterior = cola;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 86: `nuevo.siguiente = cabeza;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 87: `cola.siguiente = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 88: `cabeza.anterior = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 89: `cola = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 90: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 91: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 93: `void mostrar() {`**  
Abre el método `mostrar`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 94: `if (cabeza == null) { System.out.println("vacía"); return; }`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 95: `NodoDobleCircular actual = cabeza;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 96: `do {`**  
Abre un `do-while`. Se usa en las listas circulares porque necesitamos procesar la cabeza al menos una vez antes de comprobar si el recorrido regresó a ella.

**Línea 97: `System.out.print(actual.dato + " <-> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 98: `actual = actual.siguiente;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 99: `} while (actual != cabeza);`**  
Cierra el `do-while` y expresa la condición de terminación. En una lista circular se detiene al volver a `cabeza`, no al encontrar `null`.

**Línea 100: `System.out.println("(cabeza)");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 101: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 102: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 104: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 105: `ListaDoble doble = new ListaDoble();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 106: `doble.agregar(10); doble.agregar(20); doble.agregar(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 107: `System.out.print("Doble: ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 108: `doble.mostrarAdelante();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 110: `ListaCircular circular = new ListaCircular();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 111: `circular.agregar(10); circular.agregar(20); circular.agregar(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 112: `System.out.print("Circular: ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 113: `circular.mostrar();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 115: `ListaDobleCircular dobleCircular = new ListaDobleCircular();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 116: `dobleCircular.agregar(10); dobleCircular.agregar(20); dobleCircular.agregar(30);`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 117: `System.out.print("Doble circular: ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 118: `dobleCircular.mostrar();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 119: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 120: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ08_CompararVariantes {
    static class NodoDoble {
        int dato;
        NodoDoble siguiente;
        NodoDoble anterior;
        NodoDoble(int dato) { this.dato = dato; }
    }

    static class ListaDoble {
        NodoDoble cabeza;
        NodoDoble cola;

        void agregar(int dato) {
            NodoDoble nuevo = new NodoDoble(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
            } else {
                cola.siguiente = nuevo;
                nuevo.anterior = cola;
                cola = nuevo;
            }
        }

        void mostrarAdelante() {
            NodoDoble actual = cabeza;
            while (actual != null) {
                System.out.print(actual.dato + " <-> ");
                actual = actual.siguiente;
            }
            System.out.println("null");
        }
    }

    static class NodoCircular {
        int dato;
        NodoCircular siguiente;
        NodoCircular(int dato) { this.dato = dato; }
    }

    static class ListaCircular {
        NodoCircular cabeza;
        NodoCircular cola;

        void agregar(int dato) {
            NodoCircular nuevo = new NodoCircular(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
                nuevo.siguiente = cabeza;
            } else {
                cola.siguiente = nuevo;
                cola = nuevo;
                cola.siguiente = cabeza;
            }
        }

        void mostrar() {
            if (cabeza == null) { System.out.println("vacía"); return; }
            NodoCircular actual = cabeza;
            do {
                System.out.print(actual.dato + " -> ");
                actual = actual.siguiente;
            } while (actual != cabeza);
            System.out.println("(cabeza)");
        }
    }

    static class NodoDobleCircular {
        int dato;
        NodoDobleCircular siguiente;
        NodoDobleCircular anterior;
        NodoDobleCircular(int dato) { this.dato = dato; }
    }

    static class ListaDobleCircular {
        NodoDobleCircular cabeza;
        NodoDobleCircular cola;

        void agregar(int dato) {
            NodoDobleCircular nuevo = new NodoDobleCircular(dato);
            if (cabeza == null) {
                cabeza = cola = nuevo;
                nuevo.siguiente = nuevo;
                nuevo.anterior = nuevo;
            } else {
                nuevo.anterior = cola;
                nuevo.siguiente = cabeza;
                cola.siguiente = nuevo;
                cabeza.anterior = nuevo;
                cola = nuevo;
            }
        }

        void mostrar() {
            if (cabeza == null) { System.out.println("vacía"); return; }
            NodoDobleCircular actual = cabeza;
            do {
                System.out.print(actual.dato + " <-> ");
                actual = actual.siguiente;
            } while (actual != cabeza);
            System.out.println("(cabeza)");
        }
    }

    public static void main(String[] args) {
        ListaDoble doble = new ListaDoble();
        doble.agregar(10); doble.agregar(20); doble.agregar(30);
        System.out.print("Doble: ");
        doble.mostrarAdelante();

        ListaCircular circular = new ListaCircular();
        circular.agregar(10); circular.agregar(20); circular.agregar(30);
        System.out.print("Circular: ");
        circular.mostrar();

        ListaDobleCircular dobleCircular = new ListaDobleCircular();
        dobleCircular.agregar(10); dobleCircular.agregar(20); dobleCircular.agregar(30);
        System.out.print("Doble circular: ");
        dobleCircular.mostrar();
    }
}
```

## Resultado esperado

```text
Doble: 10 <-> 20 <-> 30 <-> null
Circular: 10 -> 20 -> 30 -> (cabeza)
Doble circular: 10 <-> 20 <-> 30 <-> (cabeza)
```

## T08 - Tarea espejo

Reutiliza las tres estructuras con datos 2, 4 y 6. Además del recorrido, imprime comprobaciones booleanas que demuestren los enlaces de cierre.

- **Archivo F15:** `estudiante/T08_ValidarVariantes.java`.
- **Criterio:** explicar qué referencia distingue a cada variante, no solo reconocer el nombre.



# EJ09 - Lista ordenada sin duplicados - integrador

## Relación exacta con FASE 15

- **Ejemplo ejecutable:** `docente/EJ09_ListaOrdenadaSinDuplicados.java` y `estudiante/EJ09_ListaOrdenadaSinDuplicados.java`.
- **Tarea espejo:** `T09_ListaOrdenadaPractica.java` - Lista ordenada con datos alternativos.
- **Regla de sincronización:** el bloque **Archivo completo sincronizado con FASE 15** de esta guía debe ser funcionalmente idéntico al archivo `EJ09_ListaOrdenadaSinDuplicados.java` del laboratorio.

## ¿Por qué existe este ejemplo?

El desafío final combina las decisiones anteriores: casos especiales de cabeza, recorrido hasta un punto de inserción, conservación de enlaces y validación de duplicados. La operación devuelve `boolean` para hacer observable si el dato fue aceptado o rechazado.

## Qué debes conectar con lo anterior

No aparece una teoría distinta a la sesión. El integrador reutiliza inserción segura y recorrido, agregando dos condiciones: conservar orden ascendente y no insertar un valor repetido.

## Modelo mental antes de escribir código

```text
Objetivo para insertar dato d:
1) lista vacía -> head=nuevo
2) d == head.dato -> rechazar
3) d < head.dato -> insertar al inicio
4) si no, avanzar mientras siguiente.dato < d
5) si siguiente == d -> rechazar
6) enlazar nuevo entre actual y actual.next
```

No empieces copiando el archivo completo. Construye el programa con los microbloques siguientes.


### Microbloque 1 - Definir Nodo y ListaOrdenada

**Escribe ahora únicamente este microbloque:**

```java
public class EJ09_ListaOrdenadaSinDuplicados {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaOrdenada {
        Nodo head;
```

**¿Por qué existe este microbloque?**

Seguimos usando un nodo simple porque el objetivo es ordenar por la posición de los enlaces, no cambiar la estructura del nodo. `ListaOrdenada` expresa que el comportamiento de inserción será distinto al de una lista simple sin restricciones.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo`: misma unidad dato+next.

- `ListaOrdenada`: agrupa la regla de orden y duplicados.

- `head`: entrada al menor dato una vez que la lista esté ordenada.

**No avances hasta poder responder:**

¿Necesitamos un campo extra "posición" dentro del nodo? No. El orden surge de cómo conectamos los nodos.

### Microbloque 2 - Abrir la operación y crear el nodo candidato

**Escribe ahora únicamente este microbloque:**

```java
        boolean agregarOrdenadoSinDuplicados(int dato) {
            Nodo nuevo = new Nodo(dato);
```

**¿Por qué existe este microbloque?**

La operación devuelve `boolean`: `true` si el nodo se insertó, `false` si se rechazó por duplicado. Creamos `nuevo` al inicio para disponer del objeto candidato en los casos de inserción.

**Explicación línea por línea / instrucción por instrucción:**

- `boolean agregarOrdenadoSinDuplicados(int dato)`: define contrato de éxito/rechazo.

- `Nodo nuevo = new Nodo(dato)`: crea candidato con next inicial null.

### Microbloque 3 - Caso lista vacía

**Escribe ahora únicamente este microbloque:**

```java
            if (head == null) {
                head = nuevo;
                return true;
            }
```

**¿Por qué existe este microbloque?**

Una lista vacía está ordenada por definición después de insertar su primer elemento. No hay duplicados que revisar.

**Explicación línea por línea / instrucción por instrucción:**

- `head == null`: detecta vacío.

- `head = nuevo`: el candidato se vuelve primer nodo.

- `return true`: informa que sí se insertó y termina el método.

**Estado conceptual después de escribirlo:**

```text
head -> [dato|null]
```

### Microbloque 4 - Rechazar duplicado en la cabeza

**Escribe ahora únicamente este microbloque:**

```java
            if (dato == head.dato) {
                return false;
            }
```

**¿Por qué existe este microbloque?**

Antes de decidir si insertar antes o después de head, comprobamos igualdad. Si el dato ya existe en la cabeza, la regla "sin duplicados" obliga a rechazarlo.

**Explicación línea por línea / instrucción por instrucción:**

- `dato == head.dato`: compara el valor nuevo con el primer valor.

- `return false`: no modifica ningún enlace y comunica rechazo.

### Microbloque 5 - Insertar antes de head cuando el dato es menor

**Escribe ahora únicamente este microbloque:**

```java
            if (dato < head.dato) {
                nuevo.next = head;
                head = nuevo;
                return true;
            }
```

**¿Por qué existe este microbloque?**

Si el nuevo dato es menor que el mínimo actual, su posición ordenada es al inicio. Reutilizamos exactamente el patrón seguro de EJ03.

**Explicación línea por línea / instrucción por instrucción:**

- `dato < head.dato`: detecta que el candidato debe preceder al mínimo actual.

- `nuevo.next = head`: conserva el inicio anterior.

- `head = nuevo`: mueve la cabeza al nuevo mínimo.

- `return true`: inserción completada.

**Estado conceptual después de escribirlo:**

```text
Antes: head -> 10 -> 20
Insertar 5:
nuevo.next=head; head=nuevo
Después: 5 -> 10 -> 20
```

### Microbloque 6 - Crear cursor para buscar la posición intermedia

**Escribe ahora únicamente este microbloque:**

```java
            Nodo actual = head;
```

**¿Por qué existe este microbloque?**

Si llegamos aquí, el dato es mayor que `head.dato`. No debe insertarse antes de la cabeza; necesitamos encontrar el punto intermedio o final correcto.

**Explicación línea por línea / instrucción por instrucción:**

- `Nodo actual = head`: cursor que recorrerá sin mover head.

### Microbloque 7 - Avanzar mientras el siguiente dato siga siendo menor

**Escribe ahora únicamente este microbloque:**

```java
            while (actual.next != null && actual.next.dato < dato) {
                actual = actual.next;
            }
```

**¿Por qué existe este microbloque?**

Miramos **el siguiente nodo** porque queremos detenernos en el nodo anterior al punto de inserción. Si `actual.next.dato` todavía es menor, el candidato debe ir más adelante.

**Explicación línea por línea / instrucción por instrucción:**

- `actual.next != null`: evita leer dato después del final.

- `actual.next.dato < dato`: mantiene el avance mientras el siguiente valor sea estrictamente menor.

- `actual = actual.next`: mueve el cursor una posición.

**Estado conceptual después de escribirlo:**

```text
Lista 10->20->30, insertar 25:
actual=10: siguiente 20 <25 -> avanza
actual=20: siguiente 30 <25 es falso -> detener en 20
```

**No avances hasta poder responder:**

¿Por qué no avanzamos mientras `actual.dato < dato`? Porque necesitamos terminar en el nodo anterior al lugar donde se insertará.

### Microbloque 8 - Detectar duplicado en la posición encontrada

**Escribe ahora únicamente este microbloque:**

```java
            if (actual.next != null && actual.next.dato == dato) {
                return false;
            }
```

**¿Por qué existe este microbloque?**

Después del recorrido, el siguiente nodo es el primer candidato cuyo valor no es menor que `dato`. Si es exactamente igual, ya existe y se rechaza.

**Explicación línea por línea / instrucción por instrucción:**

- `actual.next != null`: comprueba que exista un siguiente nodo.

- `actual.next.dato == dato`: detecta duplicado.

- `return false`: rechaza sin modificar la cadena.

### Microbloque 9 - Insertar de forma segura entre actual y su sucesor

**Escribe ahora únicamente este microbloque:**

```java
            nuevo.next = actual.next;
            actual.next = nuevo;
            return true;
        }
```

**¿Por qué existe este microbloque?**

Este es el mismo patrón seguro de EJ05: primero el nuevo conserva el sucesor original; después el nodo anterior enlaza al nuevo. Si `actual.next` era null, la misma lógica inserta al final.

**Explicación línea por línea / instrucción por instrucción:**

- `nuevo.next = actual.next`: conserva el resto de la lista, incluso si es null.

- `actual.next = nuevo`: coloca el candidato después de actual.

- `return true`: confirma inserción.

- `}`: cierra el método.

**Estado conceptual después de escribirlo:**

```text
Antes: 10 -> 20 -> 30
Insertar 25, actual=20
nuevo.next=30
20.next=nuevo
Resultado: 10 -> 20 -> 25 -> 30
```

### Microbloque 10 - Recorrer para comprobar orden

**Escribe ahora únicamente este microbloque:**

```java
        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }
```

**¿Por qué existe este microbloque?**

El recorrido final debe mostrar dos propiedades simultáneas: valores ascendentes y ausencia de repetidos.

**Explicación línea por línea / instrucción por instrucción:**

- `actual = head`: comienza en el mínimo.

- `while (actual != null)`: visita toda la lista.

- `actual = actual.next`: sigue el orden definido por enlaces.

### Microbloque 11 - Probar casos mezclados desde main

**Escribe ahora únicamente este microbloque:**

```java
    public static void main(String[] args) {
        ListaOrdenada lista = new ListaOrdenada();
        int[] datos = {30, 10, 20, 20, 5, 40};

        for (int dato : datos) {
            boolean agregado = lista.agregarOrdenadoSinDuplicados(dato);
            System.out.println("Agregar " + dato + ": " + agregado);
        }

        lista.recorrer();
    }
}
```

**¿Por qué existe este microbloque?**

El arreglo solo proporciona una secuencia de entradas para el integrador; la estructura enlazada sigue siendo la responsable de almacenar el resultado. El `for-each` intenta agregar cada valor y registra si fue aceptado.

**Explicación línea por línea / instrucción por instrucción:**

- `int[] datos = {...}`: define los casos de prueba, incluyendo un duplicado 20.

- `for (int dato : datos)`: procesa cada entrada.

- `boolean agregado = ...`: captura true/false de la operación.

- `println(...)`: hace visible qué dato fue rechazado.

- `lista.recorrer()`: muestra el estado final ordenado.

**Estado conceptual después de escribirlo:**

```text
Entradas: 30,10,20,20,5,40
Final esperado: 5 -> 10 -> 20 -> 30 -> 40 -> null
El segundo 20 debe devolver false.
```

## Auditoría línea por línea de `EJ09_ListaOrdenadaSinDuplicados.java`

Esta sección funciona como comprobación final **antes de ver el archivo completo**. Recorre todas las líneas no vacías del archivo de FASE 15 y explica por qué existen. Si una línea ya fue desarrollada en un microbloque, aquí se resume su función para verificar que no quede ninguna parte sin explicación.

**Línea 1: `public class EJ09_ListaOrdenadaSinDuplicados {`**  
Crea la clase pública contenedora del ejemplo `EJ09_ListaOrdenadaSinDuplicados`. Existe para que el archivo sea ejecutable de manera independiente en FASE 15; no representa por sí sola un nodo de la lista.

**Línea 2: `static class Nodo {`**  
Define la unidad estructural `Nodo` dentro del archivo. Se crea porque una lista enlazada no almacena solo valores: cada elemento debe combinar un dato con una referencia al siguiente nodo.

**Línea 3: `int dato;`**  
Declara el dato almacenado por un nodo. Sin este atributo el nodo tendría enlace, pero no información útil que representar.

**Línea 4: `Nodo next;`**  
Declara la referencia `next` que conecta el nodo con el siguiente elemento. Es el enlace que transforma objetos aislados en una cadena.

**Línea 5: `Nodo(int dato) { this.dato = dato; }`**  
Declara el constructor del nodo. Existe para que cada objeto nazca con el dato que se quiere almacenar y pueda inicializarse de forma coherente.

**Línea 6: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 8: `static class ListaOrdenada {`**  
Crea la estructura que aplicará la regla de mantener los datos ordenados y sin duplicados. El nodo no cambia; cambia la lógica con la que se enlaza.

**Línea 9: `Nodo head;`**  
Declara `head` como referencia estructural de la lista. No almacena todos los nodos: conserva un punto de acceso al extremo correspondiente.

**Línea 11: `boolean agregarOrdenadoSinDuplicados(int dato) {`**  
Abre el método `agregarOrdenadoSinDuplicados`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 12: `Nodo nuevo = new Nodo(dato);`**  
Crea un objeto nodo nuevo en memoria y guarda una referencia a él en la variable de la izquierda. En este instante el nodo aún debe conectarse según la operación que se está realizando.

**Línea 14: `if (head == null) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 15: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 16: `return true;`**  
Finaliza el método informando que la operación tuvo éxito; evita ejecutar ramas posteriores que ya no corresponden.

**Línea 17: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 19: `if (dato == head.dato) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 20: `return false;`**  
Finaliza el método informando que la condición buscada no se cumple o que el dato fue rechazado, por ejemplo por duplicidad.

**Línea 21: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 23: `if (dato < head.dato) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 24: `nuevo.next = head;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 25: `head = nuevo;`**  
Actualiza una referencia de entrada o extremo de la lista. No crea ni copia nodos; cambia cuál objeto será considerado cabeza o cola después de la operación.

**Línea 26: `return true;`**  
Finaliza el método informando que la operación tuvo éxito; evita ejecutar ramas posteriores que ya no corresponden.

**Línea 27: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 29: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 30: `while (actual.next != null && actual.next.dato < dato) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 31: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 32: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 34: `if (actual.next != null && actual.next.dato == dato) {`**  
Abre una decisión. La condición separa un caso estructural que debe resolverse antes de continuar con la ruta general; por ejemplo lista vacía, posición especial, duplicado o fin de validación.

**Línea 35: `return false;`**  
Finaliza el método informando que la condición buscada no se cumple o que el dato fue rechazado, por ejemplo por duplicidad.

**Línea 36: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 38: `nuevo.next = actual.next;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 39: `actual.next = nuevo;`**  
Modifica una referencia entre nodos. Esta es una línea estructural: no cambia el dato, cambia qué objeto queda conectado con cuál. Debe interpretarse dibujando el enlace antes y después.

**Línea 40: `return true;`**  
Finaliza el método informando que la operación tuvo éxito; evita ejecutar ramas posteriores que ya no corresponden.

**Línea 41: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 43: `void recorrer() {`**  
Abre el método `recorrer`. Este bloque existe para encapsular una operación concreta de la estructura y evitar que toda la lógica quede mezclada en `main`.

**Línea 44: `Nodo actual = head;`**  
Crea un cursor local llamado `actual` que comienza en la entrada de la lista. Se usa para recorrer sin mover la referencia estructural principal.

**Línea 45: `while (actual != null) {`**  
Abre un recorrido condicionado. La condición determina exactamente cuándo el cursor puede seguir avanzando y es clave para no saltarse nodos ni salir del rango válido.

**Línea 46: `System.out.print(actual.dato + " -> ");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 47: `actual = actual.next;`**  
Avanza el cursor local al siguiente nodo. Cambia `actual`, pero no mueve `head`/`cabeza`; la estructura permanece intacta.

**Línea 48: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 49: `System.out.println("null");`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 50: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 51: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 53: `public static void main(String[] args) {`**  
Declara `main`, el punto donde comienza la ejecución del ejemplo. Hasta aquí solo había definiciones; desde `main` se crean objetos y se invocan operaciones.

**Línea 54: `ListaOrdenada lista = new ListaOrdenada();`**  
Crea una instancia de la estructura de lista. Sus referencias de estado empiezan sin nodos y después se completan mediante los métodos del ejemplo.

**Línea 55: `int[] datos = {30, 10, 20, 20, 5, 40};`**  
Define una colección de valores de entrada para probar el integrador con diferentes casos, incluido un duplicado. El arreglo es solo fuente de pruebas; la estructura resultante sigue siendo enlazada.

**Línea 57: `for (int dato : datos) {`**  
Recorre cada valor de prueba para invocar la operación de inserción ordenada una vez por dato y observar si fue aceptado o rechazado.

**Línea 58: `boolean agregado = lista.agregarOrdenadoSinDuplicados(dato);`**  
Guarda el resultado booleano de la inserción para hacer visible si el dato se incorporó o fue rechazado.

**Línea 59: `System.out.println("Agregar " + dato + ": " + agregado);`**  
Produce evidencia observable en consola. Esta salida permite comparar la predicción estructural con el estado que realmente alcanza el programa.

**Línea 60: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 62: `lista.recorrer();`**  
Forma parte del archivo ejecutable y debe interpretarse en el contexto del microbloque anterior: identifica qué información usa, qué estado observa o modifica y por qué se ejecuta en este punto.

**Línea 63: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.

**Línea 64: `}`**  
Cierra el bloque de alcance actual. Es importante para que Java sepa dónde termina la clase, método, condición o ciclo que se venía construyendo.


## Archivo completo sincronizado con FASE 15

```java
public class EJ09_ListaOrdenadaSinDuplicados {
    static class Nodo {
        int dato;
        Nodo next;
        Nodo(int dato) { this.dato = dato; }
    }

    static class ListaOrdenada {
        Nodo head;

        boolean agregarOrdenadoSinDuplicados(int dato) {
            Nodo nuevo = new Nodo(dato);

            if (head == null) {
                head = nuevo;
                return true;
            }

            if (dato == head.dato) {
                return false;
            }

            if (dato < head.dato) {
                nuevo.next = head;
                head = nuevo;
                return true;
            }

            Nodo actual = head;
            while (actual.next != null && actual.next.dato < dato) {
                actual = actual.next;
            }

            if (actual.next != null && actual.next.dato == dato) {
                return false;
            }

            nuevo.next = actual.next;
            actual.next = nuevo;
            return true;
        }

        void recorrer() {
            Nodo actual = head;
            while (actual != null) {
                System.out.print(actual.dato + " -> ");
                actual = actual.next;
            }
            System.out.println("null");
        }
    }

    public static void main(String[] args) {
        ListaOrdenada lista = new ListaOrdenada();
        int[] datos = {30, 10, 20, 20, 5, 40};

        for (int dato : datos) {
            boolean agregado = lista.agregarOrdenadoSinDuplicados(dato);
            System.out.println("Agregar " + dato + ": " + agregado);
        }

        lista.recorrer();
    }
}
```

## Resultado esperado

```text
Agregar 30: true
Agregar 10: true
Agregar 20: true
Agregar 20: false
Agregar 5: true
Agregar 40: true
5 -> 10 -> 20 -> 30 -> 40 -> null
```

## T09 - Tarea espejo

Usa datos `12, 7, 9, 7, 15, 1`. Implementa la misma regla: orden ascendente y sin duplicados.

- **Archivo F15:** `estudiante/T09_ListaOrdenadaPractica.java`.
- **Evidencia:** booleanos de aceptación + recorrido final.
- **Criterio:** debes justificar cada rama del método, no solo la salida final.


# Cierre de la sesión

Al terminar los nueve ejemplos debes poder pasar de una representación gráfica a código y volver del código a un dibujo de referencias. El criterio principal no es recordar nombres de métodos, sino explicar por qué una asignación se ejecuta antes que otra y qué parte de la cadena se conserva después de cada cambio.

Repasa especialmente:

- diferencia entre objeto y referencia;
- función de `head`;
- diferencia entre `actual` y `head`;
- condición `actual != null` frente a `actual.next != null`;
- orden seguro de dos enlaces al insertar;
- uso de `anterior` al eliminar;
- terminación en `null` frente a cierre circular;
- relación entre recorrido e O(n), e inserción al inicio y O(1) en los casos mostrados.

La FASE 15 contiene los mismos ejemplos y tareas como archivos ejecutables. Puedes utilizarla después para validar, recuperar un punto de partida o comparar tu resultado; **no sustituye la construcción y explicación desarrollada en esta guía**.
