---
course_id: ALG-EST-DATOS-IA
session_id: S03
module_id: TEMA_03
course_version: v5.7-f13-code-explained
source_origin: PPT
status: validated
---

# Guía del Estudiante - S03: Eficiencia de algoritmos y ordenamiento de un vector

> **Versión revisada:** cada ejemplo con código incluye lectura pedagógica explícita antes de la ejecución: propósito de cada estructura, variables, ciclos, condición, cambios de estado, traza e interpretación de eficiencia.

## 1. Propósito de la sesión

En esta sesión vas a pasar de “el programa funciona” a “puedo explicar cuánto trabajo realiza y cómo crece ese trabajo”. Trabajarás con vectores, búsqueda lineal, Bubble Sort, contadores y medición temporal. Cada ejemplo está resuelto dentro de esta guía y también existe como archivo ejecutable en FASE 15. Después de cada ejemplo resolverás una tarea espejo que cambia condiciones para obligarte a razonar.

## 2. Resultado observable

### A. Lectura sugerida del docente

Al finalizar podrás analizar un algoritmo sencillo sobre un vector y defender una conclusión de eficiencia con evidencia. No memorizarás solo una etiqueta como O(n²): deberás mostrar qué ciclos producen el contador, qué cambia cuando aumenta n, qué depende del orden inicial y qué depende del entorno de ejecución. También comprobarás una diferencia importante entre teoría e implementación: un vector ordenado no reduce automáticamente las comparaciones si el código no incluye una condición que permita detenerse. El dominio se demuestra cuando puedes predecir antes de ejecutar, comprobar después, explicar la causa de cualquier diferencia y resolver una tarea nueva sin copiar literalmente el ejemplo.

### B. Desempeños observables

- Contar comparaciones e intercambios.
- Identificar mejor y peor caso en una búsqueda.
- Ejecutar Bubble Sort y trazar su comportamiento.
- Relacionar tamaño de entrada y crecimiento.
- Reconocer O(1), O(log n), O(n) y O(n²).
- Diferenciar operaciones y tiempo a posteriori.
- Diagnosticar una afirmación que el código no respalda.
- Integrar evidencia en un reporte.

### C. Criterio de dominio

Para cada `EJxx` debes poder explicar entrada, métrica, estructura de control, predicción, salida, variación y error. Para cada `Txx` debes producir la evidencia indicada sin consultar la solución docente.

## 3. Antes de iniciar

### Debe saber
- Declarar y recorrer un arreglo de enteros.
- Leer `for`, `if`, variables y métodos `main`.
- Compilar y ejecutar una clase Java básica.

### Debe tener disponible
- JDK funcional. Si el aula ya usa JDK 21, puede mantenerse. Para instalación nueva, JDK 25 es el LTS vigente.
- Editor o IDE de Java.
- `FASE_15_Laboratorio.zip`.

### No se asumirá todavía
- Algoritmos de ordenamiento adicionales.
- Análisis formal avanzado.
- Frameworks o librerías externas.

## 4. Herramientas y recursos para la práctica

### JDK

El JDK contiene `javac` y las herramientas necesarias para ejecutar Java. Para instalación nueva usa la referencia oficial: https://www.oracle.com/java/technologies/downloads/ . No necesitas Maven, Gradle ni librerías externas.

**Verificación:**
1. Ejecuta `java -version`.
2. Ejecuta `javac -version`.
3. Si `java` existe pero `javac` no, verifica que tengas un JDK completo y que `PATH` apunte a él.
4. No continúes hasta poder compilar una clase de prueba.

### FASE 15

Descomprime el ZIP. Trabaja en `estudiante/ejemplos` y `estudiante/tareas`. La carpeta `docente` contiene soluciones y no forma parte de la resolución del estudiante.

## 5. Ruta de trabajo de la práctica

```text
VERIFICAR ENTORNO → EJ01 → T01 → EJ02 → T02 → EJ03 → T03
→ EJ04 → T04 → EJ05 → T05 → EJ06 → T06 → EJ07 → T07 → CIERRE
```

## 6. Preparación y verificación del entorno desde cero

### Ruta A - El entorno ya existe
1. Ejecuta `java -version` y `javac -version`.
2. Descomprime FASE 15.
3. Abre `estudiante/ejemplos`.
4. Ejecuta `javac EJ01_BusquedaCasos.java`.
5. Ejecuta `java EJ01_BusquedaCasos`.
6. Si aparece `class not found`, confirma que la terminal esté en la carpeta correcta y que uses el nombre sin `.java`.

### Ruta B - Equipo sin preparar
1. Abre la página oficial de Java.
2. Selecciona el instalador de tu sistema operativo de una versión LTS.
3. Instala el JDK; no agregues herramientas que la práctica no usa.
4. Reinicia la terminal.
5. Verifica `java -version` y `javac -version`.
6. Compila EJ01 como prueba mínima.

## 7. Desarrollo del laboratorio mediante ejemplos resueltos

# EJ01 - Mejor y peor caso en una búsqueda lineal

## Qué vamos a resolver

Buscar un valor desde el inicio del vector y medir cuántas comparaciones exige cada entrada.

## Qué aprenderás aquí

El mejor caso aparece cuando el objetivo está en la primera posición; el peor caso exige revisar todo el vector.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ01_BusquedaCasos.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
public class EJ01_BusquedaCasos {
    static int buscar(int[] v, int objetivo) {
        int comparaciones = 0;
        for (int i = 0; i < v.length; i++) {
            comparaciones++;
            if (v[i] == objetivo) {
                System.out.println("objetivo=" + objetivo + " indice=" + i + " comparaciones=" + comparaciones);
                return i;
            }
        }
        System.out.println("objetivo=" + objetivo + " indice=-1 comparaciones=" + comparaciones);
        return -1;
    }
    public static void main(String[] args) {
        int[] v = {14, 8, 21, 3, 17};
        buscar(v, 14);
        buscar(v, 17);
        buscar(v, 99);
    }
}
```

## Lectura pedagógica del código: ¿por qué está escrito así?

Antes de ejecutar, construye este mapa mental:

```text
VECTOR + OBJETIVO
      ↓
RECORRER UNA POSICIÓN A LA VEZ
      ↓
CONTAR CADA COMPARACIÓN
      ↓
¿ENCONTRADO?
  SÍ → TERMINAR Y DEVOLVER ÍNDICE
  NO → CONTINUAR
      ↓
SI TERMINA EL FOR → NO EXISTE
```

### 1. ¿Por qué existe el método `buscar`?

```java
static int buscar(int[] v, int objetivo)
```

El método separa la lógica de búsqueda del `main`. Recibe dos datos: `v`, que es el vector donde buscamos, y `objetivo`, que es el valor que queremos encontrar. Devuelve un `int`: el índice donde se encontró el valor o `-1` si no existe.

### 2. ¿Por qué el contador se declara antes del `for`?

```java
int comparaciones = 0;
```

Debe existir durante todo el recorrido. Si se declarara dentro del ciclo, volvería a cero en cada posición y perderíamos el acumulado. El contador representa cuántas veces el algoritmo tuvo que preguntar si el elemento actual era el objetivo.

### 3. ¿Por qué aquí basta un solo `for`?

```java
for (int i = 0; i < v.length; i++)
```

La búsqueda lineal avanza una sola vez desde la posición `0` hasta la última. `i` representa la posición que estamos examinando. No necesitamos un segundo ciclo porque en cada paso solo comparamos el objetivo con un elemento del vector.

### 4. ¿Por qué `comparaciones++` aparece antes del `if`?

```java
comparaciones++;
if (v[i] == objetivo)
```

Cada vez que llegamos a una posición realizaremos la pregunta `v[i] == objetivo`. Por eso primero registramos que esa comparación ocurrió y después evaluamos el resultado.

### 5. ¿Por qué aparece `return i` dentro del ciclo?

Si encontramos el valor, no tiene sentido seguir recorriendo el resto del vector. `return i` termina inmediatamente el método. Esa salida temprana explica el **mejor caso**: si el objetivo está en la primera posición, solo ocurre una comparación.

### 6. ¿Por qué al final se devuelve `-1`?

Si el `for` termina, significa que ninguna posición coincidió. `-1` funciona como señal de “no encontrado”. En ese escenario se revisó todo el vector, por lo que corresponde al peor caso de esta búsqueda.

### Traza manual mínima

Para `v = [14, 8, 21, 3, 17]`:

| Objetivo | Posiciones revisadas | Comparaciones |
|---|---|---:|
| 14 | 0 | 1 |
| 17 | 0,1,2,3,4 | 5 |
| 99 | 0,1,2,3,4 | 5 |

La idea importante no es memorizar los números: es poder explicar **por qué el flujo del código produce esos números**.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ01_BusquedaCasos.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ01_BusquedaCasos
```

## Resultado esperado e interpretación

14 → 1 comparación; 17 → 5; 99 → 5 y no encontrado.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Busca 21; después usa un vector de un solo elemento.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Mover el contador dentro del ciclo y reiniciarlo en cada vuelta.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

El contador debe declararse antes del `for` e incrementarse una vez por comparación.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T01 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T01_BusquedaEspejo.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** Con [9,4,12,7,20,3], busca 9 y 3. Debes obtener 1 y 6 comparaciones.

---

# EJ02 - Trazar Bubble Sort y observar el contador

## Qué vamos a resolver

Ordenar [5,3,8,4,2], observar cada pasada y justificar el contador.

## Qué aprenderás aquí

Cada comparación evalúa dos posiciones adyacentes. Si están invertidas, se intercambian. Los ciclos determinan cuántas veces se evalúa la condición.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ02_BubbleSortBase.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
import java.util.Arrays;
public class EJ02_BubbleSortBase {
    public static void main(String[] args) {
        int[] vector = {5, 3, 8, 4, 2};
        int comparaciones = 0;
        for (int i = 0; i < vector.length - 1; i++) {
            for (int j = 0; j < vector.length - 1; j++) {
                comparaciones++;
                if (vector[j] > vector[j + 1]) {
                    int temp = vector[j];
                    vector[j] = vector[j + 1];
                    vector[j + 1] = temp;
                }
            }
            System.out.println("pasada " + (i + 1) + ": " + Arrays.toString(vector));
        }
        System.out.println("final=" + Arrays.toString(vector));
        System.out.println("comparaciones=" + comparaciones);
    }
}
```

## Lectura pedagógica del código: ¿por qué Bubble Sort usa dos `for`?

Antes de ejecutar, usa este mapa mental:

```text
BUCLE EXTERNO (i)
¿Cuántas pasadas completas hacemos?
        ↓
BUCLE INTERNO (j)
¿Qué pares vecinos comparamos en esa pasada?
        ↓
COMPARAR vector[j] con vector[j+1]
        ↓
SI ESTÁN INVERTIDOS → INTERCAMBIAR
        ↓
AL TERMINAR LA PASADA → MOSTRAR EL VECTOR
```

### 1. `import java.util.Arrays`: ¿para qué sirve?

```java
import java.util.Arrays;
```

No ordena el vector. Se usa únicamente para imprimir el contenido completo con `Arrays.toString(vector)`. Sin esta importación tendríamos que recorrer el arreglo manualmente para mostrarlo.

### 2. El vector es el estado que Bubble Sort modifica

```java
int[] vector = {5, 3, 8, 4, 2};
```

El algoritmo no crea un vector nuevo en cada comparación: modifica las posiciones del mismo arreglo mediante intercambios. Por eso conviene observar el vector después de cada pasada.

### 3. ¿Por qué el primer `for` controla las pasadas?

```java
for (int i = 0; i < vector.length - 1; i++) {
```

`i` no representa directamente una comparación entre dos datos. Representa la **pasada completa** sobre el vector. Con 5 elementos, `vector.length - 1` vale 4, de modo que esta versión realiza cuatro pasadas.

Una sola pasada no garantiza que todo quede ordenado. En la primera pasada, los valores grandes pueden avanzar hacia la derecha, pero todavía pueden quedar elementos pequeños fuera de posición en la parte izquierda.

### 4. ¿Por qué existe un segundo `for` dentro del primero?

```java
for (int j = 0; j < vector.length - 1; j++) {
```

`j` controla las **comparaciones entre vecinos** durante una pasada. Bubble Sort trabaja con parejas adyacentes:

```text
vector[j]      → elemento actual
vector[j + 1]  → elemento siguiente
```

Con `j = 0` se comparan las posiciones 0 y 1; con `j = 1`, las posiciones 1 y 2; y así sucesivamente. Necesitamos dos ciclos porque el algoritmo debe realizar varias comparaciones en cada pasada y, además, repetir pasadas hasta completar el ordenamiento.

### 5. ¿Por qué el límite es `vector.length - 1`?

Dentro del ciclo se usa `vector[j + 1]`. Si permitiéramos que `j` llegara hasta la última posición, entonces `j + 1` intentaría acceder a una posición que no existe. Por eso el último valor válido de `j` es `length - 2`.

### 6. ¿Qué mide `comparaciones++`?

```java
comparaciones++;
```

Se ejecuta una vez por cada pareja que el ciclo interno evalúa. En esta implementación concreta:

```text
4 pasadas × 4 comparaciones por pasada = 16 comparaciones
```

El contador no mide segundos y tampoco cuenta únicamente intercambios. Cuenta cuántas veces llegamos a la comparación entre vecinos.

### 7. ¿Qué pregunta hace el `if`?

```java
if (vector[j] > vector[j + 1]) {
```

Pregunta si la pareja está en orden incorrecto para un orden ascendente. Si el valor de la izquierda es mayor que el de la derecha, hay que intercambiarlos.

### 8. ¿Por qué necesitamos `temp` para intercambiar?

```java
int temp = vector[j];
vector[j] = vector[j + 1];
vector[j + 1] = temp;
```

`temp` guarda temporalmente el valor izquierdo para no perderlo. Si tuviéramos `[5,3]`:

```text
1. temp = 5
2. posición izquierda = 3
3. posición derecha = temp = 5
Resultado: [3,5]
```

### 9. Traza de la primera pasada

Partimos de:

```text
[5, 3, 8, 4, 2]
```

| `j` | Comparación | Acción | Vector resultante |
|---:|---|---|---|
| 0 | 5 > 3 | intercambia | [3, 5, 8, 4, 2] |
| 1 | 5 > 8 | no intercambia | [3, 5, 8, 4, 2] |
| 2 | 8 > 4 | intercambia | [3, 5, 4, 8, 2] |
| 3 | 8 > 2 | intercambia | [3, 5, 4, 2, 8] |

Al terminar la primera pasada, el `8` llegó al extremo derecho. El vector todavía no está completamente ordenado, por eso el ciclo externo debe iniciar otra pasada.

### 10. ¿Qué aporta el `println` de cada pasada?

```java
System.out.println("pasada " + (i + 1) + ": " + Arrays.toString(vector));
```

No es parte esencial del ordenamiento; es instrumentación pedagógica. Nos permite ver el estado del vector después de cada pasada y relacionar el comportamiento interno con una evidencia observable.

### Relación con eficiencia

Los dos ciclos están anidados. En esta versión ambos dependen del tamaño del vector. Esa estructura es la pista principal para comprender por qué el crecimiento se clasifica como cuadrático, O(n²). Más adelante separarás la **fórmula exacta del contador** de la **clasificación de crecimiento**.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ02_BubbleSortBase.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ02_BubbleSortBase
```

## Resultado esperado e interpretación

Salida [2,3,4,5,8]; comparaciones=16.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Usa [4,1,3,2]; después imprime la salida tras cada pasada.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Permitir que `j` alcance `vector.length - 1` y luego acceder a `j+1`.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

El último `j` debe ser `length-2`; la condición `j < length-1` protege el acceso vecino.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T02 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T02_TrazaEspejo.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** Con [6,2,5,1], la salida debe ser [1,2,5,6] y comparaciones=9.

---

# EJ03 - Vector ordenado vs desordenado con la misma implementación

## Qué vamos a resolver

Comparar entradas de igual n y separar comparaciones de intercambios.

## Qué aprenderás aquí

En la versión base el número de comparaciones depende de los límites de los ciclos. El orden inicial sí puede cambiar la cantidad de swaps.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ03_OrdenInicial.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
import java.util.Arrays;
public class EJ03_OrdenInicial {
    static void analizar(int[] entrada) {
        int[] v = Arrays.copyOf(entrada, entrada.length);
        int comparaciones = 0, intercambios = 0;
        for (int i = 0; i < v.length - 1; i++) {
            for (int j = 0; j < v.length - 1; j++) {
                comparaciones++;
                if (v[j] > v[j + 1]) {
                    int t=v[j]; v[j]=v[j+1]; v[j+1]=t;
                    intercambios++;
                }
            }
        }
        System.out.println(Arrays.toString(entrada) + " -> comparaciones=" + comparaciones + ", intercambios=" + intercambios);
    }
    public static void main(String[] args) {
        analizar(new int[]{1,2,3,4,5});
        analizar(new int[]{5,3,8,4,2});
    }
}
```

## Lectura pedagógica del código: separar comparaciones de intercambios

Este ejemplo agrega una idea que el ejemplo anterior no mostraba de forma separada: dos ejecuciones pueden realizar la misma cantidad de **comparaciones** y una cantidad distinta de **intercambios**.

### 1. ¿Por qué el método recibe `entrada` y crea una copia?

```java
static void analizar(int[] entrada) {
    int[] v = Arrays.copyOf(entrada, entrada.length);
```

Bubble Sort modifica el arreglo. La copia `v` protege `entrada` para que al imprimirla al final podamos seguir viendo el estado original. Esto permite comparar “cómo entró” con “qué trabajo hizo el algoritmo”.

### 2. ¿Por qué hay dos contadores?

```java
int comparaciones = 0, intercambios = 0;
```

- `comparaciones` cuenta cuántas veces se evalúa una pareja vecina.
- `intercambios` cuenta cuántas veces esa pareja estaba invertida y hubo que mover datos.

Son métricas distintas. No debes usar una como si fuera la otra.

### 3. ¿Por qué las comparaciones pueden coincidir aunque el orden inicial sea distinto?

Los límites de los dos `for` son fijos. Con `n = 5`, el ciclo externo realiza 4 pasadas y el interno 4 comparaciones por pasada. Por eso ambas entradas realizan 16 comparaciones aunque una ya esté ordenada.

### 4. ¿Qué sí cambia por el orden inicial?

El `if` solo entra cuando `v[j] > v[j + 1]`. Un vector ya ordenado no necesita intercambios; uno desordenado sí. Para los datos de este ejemplo:

```text
[1,2,3,4,5]      → 16 comparaciones, 0 intercambios
[5,3,8,4,2]      → 16 comparaciones, 7 intercambios
```

### 5. ¿Qué aprendizaje corrige este ejemplo?

No basta afirmar “ordenado = mejor caso” de forma automática. Primero debes mirar **qué optimizaciones implementa realmente el código**. Esta versión no tiene una bandera de salida temprana, por lo que sigue ejecutando todas las pasadas aunque ya no haya intercambios.

### Relación con eficiencia

Este ejemplo te obliga a distinguir tres preguntas:

```text
¿El programa ordena correctamente?
¿cuántas comparaciones ejecuta?
¿cuántos intercambios necesita?
```

Responder solo la primera no alcanza para analizar eficiencia.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ03_OrdenInicial.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ03_OrdenInicial
```

## Resultado esperado e interpretación

Para n=5 ambas entradas ejecutan 16 comparaciones; los swaps difieren.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Compara n=4 ordenado/descendente; después observa swaps.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Afirmar “ordenado = menos comparaciones” sin comprobar una condición de parada.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

La conclusión debe salir del código real: sin salida anticipada, las pasadas se ejecutan aunque ya no haya swaps.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T03 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T03_ComparacionEntradas.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** Compara [2,4,6,8,10] y [10,8,6,4,2]. Las comparaciones coinciden; los swaps no.

---

# EJ04 - Tamaño de entrada y crecimiento cuadrático

## Qué vamos a resolver

Cambiar n y observar cómo crece el contador de la estructura anidada.

## Qué aprenderás aquí

Con dos ciclos de `n-1` iteraciones, la cuenta exacta de esta implementación es `(n-1)^2`. Su orden de crecimiento se clasifica como O(n²).
Nota: Evaluación de expresiones algebraicas
## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ04_CrecimientoN.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
public class EJ04_CrecimientoN {
    static int contar(int n) {
        int comparaciones = 0;
        for (int i = 0; i < n - 1; i++)
            for (int j = 0; j < n - 1; j++)
                comparaciones++;
        return comparaciones;
    }
    public static void main(String[] args) {
        for (int n : new int[]{3,5,7})
            System.out.println("n="+n+" comparaciones="+contar(n)+" formula="+((n-1)*(n-1)));
    }
}
```

## Lectura pedagógica del código: de los ciclos a la fórmula `(n-1)²`

Este ejemplo elimina el intercambio para concentrarse exclusivamente en la cantidad de veces que se ejecuta el cuerpo de dos ciclos anidados.

### 1. ¿Por qué `contar` recibe solo `n`?

```java
static int contar(int n)
```

Aquí no importa el contenido del vector. Queremos aislar el efecto del tamaño de entrada. `n` representa cuántos elementos tendría la entrada.

### 2. ¿Qué hace el ciclo externo?

```java
for (int i = 0; i < n - 1; i++)
```

Ejecuta `n - 1` iteraciones.

### 3. ¿Qué hace el ciclo interno?

```java
for (int j = 0; j < n - 1; j++)
    comparaciones++;
```

Por cada iteración del ciclo externo, vuelve a ejecutar `n - 1` iteraciones. Por eso la cuenta exacta de este código es:

```text
(n - 1) × (n - 1) = (n - 1)²
```

Para los tamaños del ejemplo:

| n | Cuenta exacta | Comparaciones |
|---:|---|---:|
| 3 | 2 × 2 | 4 |
| 5 | 4 × 4 | 16 |
| 7 | 6 × 6 | 36 |

### 4. ¿Entonces por qué hablamos de O(n²) y no de O((n-1)²)?

La notación Big O se usa para describir el **orden de crecimiento**. Cuando `n` crece, el término cuadrático domina el comportamiento. La fórmula exacta del contador y la categoría Big O no son la misma cosa: una da un valor exacto para esta implementación y la otra describe cómo escala.

### 5. ¿Por qué el código omite llaves en los `for`?

En Java, un `for` puede controlar una sola instrucción sin llaves. Aquí esa única instrucción es otro `for`, y el ciclo interno controla a su vez `comparaciones++`. Es válido, pero en una práctica de aprendizaje puedes añadir llaves para hacer la estructura más visible.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ04_CrecimientoN.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ04_CrecimientoN
```

## Resultado esperado e interpretación

n=3→4, n=5→16, n=7→36.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Añade n=9; luego construye una tabla n/comparaciones.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Leer O(n²) como “n² segundos exactos”.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

Big O describe crecimiento; la fórmula exacta del contador y el tiempo real son evidencias distintas.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T04 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T04_TablaCrecimiento.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** n=4→9, n=6→25, n=8→49.

---

# EJ05 - Prueba a posteriori: medir tiempo real sin confundirlo con Big O

## Qué vamos a resolver

Medir tiempo real y comparaciones sobre una entrada generada.

## Qué aprenderás aquí

`System.nanoTime()` permite registrar una duración de esa ejecución. Esa cifra puede variar; el contador estructural para el mismo n no debería variar.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ05_PruebaPosteriori.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
public class EJ05_PruebaPosteriori {
    static long[] ordenar(int n) {
        int[] v = new int[n];
        for (int i=0;i<n;i++) v[i]=n-i;
        long comparaciones=0;
        long inicio=System.nanoTime();
        for(int i=0;i<v.length-1;i++) {
            for(int j=0;j<v.length-1;j++) {
                comparaciones++;
                if(v[j]>v[j+1]) { int t=v[j];v[j]=v[j+1];v[j+1]=t; }
            }
        }
        long tiempo=System.nanoTime()-inicio;
        return new long[]{comparaciones,tiempo};
    }
    public static void main(String[] args) {
        for(int n:new int[]{300,600}) {
            long[] r=ordenar(n);
            System.out.println("n="+n+" comparaciones="+r[0]+" tiempo_ns="+r[1]);
        }
    }
}
```

## Lectura pedagógica del código: análisis a posteriori sin confundirlo con Big O

Este ejemplo incorpora una medición temporal real. La meta no es obtener “el número correcto de nanosegundos”, sino entender qué parte de la evidencia depende del algoritmo y qué parte depende del entorno.

### 1. ¿Por qué se crea un vector de tamaño `n`?

```java
int[] v = new int[n];
for (int i=0;i<n;i++) v[i]=n-i;
```

Primero reservamos un arreglo del tamaño solicitado. Luego lo llenamos en orden descendente para generar una entrada que obligue a realizar intercambios.

### 2. ¿Por qué `comparaciones` es `long`?

```java
long comparaciones = 0;
```

Para tamaños mayores, el contador puede crecer bastante. `long` ofrece un rango más amplio que `int` y permite mantener la medición sin riesgo innecesario de desbordamiento en prácticas posteriores.

### 3. ¿Qué significa `System.nanoTime()`?

```java
long inicio = System.nanoTime();
...
long tiempo = System.nanoTime() - inicio;
```

Registramos un instante antes del bloque medido y otro después. La resta produce una duración aproximada en nanosegundos de **esa ejecución en ese entorno**. No representa la complejidad Big O.

### 4. ¿Qué parte es determinista y qué parte puede variar?

Con los mismos límites de ciclo y el mismo `n`, el número de comparaciones es predecible. El tiempo, en cambio, puede variar por CPU, carga del sistema, JVM y otras condiciones.

Para esta implementación:

```text
n=300 → (299)² = 89401 comparaciones
n=600 → (599)² = 358801 comparaciones
```

El tiempo en nanosegundos debe observarse, no memorizarse.

### 5. ¿Por qué el método devuelve `long[]`?

```java
return new long[]{comparaciones, tiempo};
```

Se devuelven dos resultados relacionados: trabajo contado y duración medida. En `main`, `r[0]` representa las comparaciones y `r[1]` el tiempo.

### Relación con eficiencia

La prueba a posteriori te dice qué ocurrió en una ejecución real. Big O explica cómo crece el trabajo. Ambas evidencias pueden complementarse, pero una no sustituye a la otra.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ05_PruebaPosteriori.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ05_PruebaPosteriori
```

## Resultado esperado e interpretación

El programa imprime comparaciones y `tiempo_ns`; el tiempo exacto no se predice.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Repite tres veces; después cambia n=300→600.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Concluir que una sola corrida temporal demuestra la eficiencia general.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

Combina medición real con contador y estructura de control.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T05 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T05_MedicionEspejo.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** n=400→159201 comparaciones; n=800→638401. Registra dos tiempos por tamaño sin exigir proporción exacta.

---

# EJ06 - Reconocer O(1), O(log n), O(n) y O(n²) mediante contadores

## Qué vamos a resolver

Comparar cuatro estructuras simples instrumentadas con contadores.

## Qué aprenderás aquí

El orden se reconoce por cómo depende el número de iteraciones de n: constante, división repetida, recorrido único o doble recorrido.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ06_OrdenesCrecimiento.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
public class EJ06_OrdenesCrecimiento {
    static int constante(int n){ return 1; }
    static int logaritmico(int n){ int c=0; while(n>1){ n/=2; c++; } return c; }
    static int lineal(int n){ int c=0; for(int i=0;i<n;i++) c++; return c; }
    static int cuadratico(int n){ int c=0; for(int i=0;i<n;i++) for(int j=0;j<n;j++) c++; return c; }
    public static void main(String[] args){
        for(int n:new int[]{8,16})
            System.out.println("n="+n+" O(1)="+constante(n)+" O(log n)="+logaritmico(n)+" O(n)="+lineal(n)+" O(n2)="+cuadratico(n));
    }
}
```

## Lectura pedagógica del código: reconocer cuatro patrones de crecimiento

Este archivo no intenta resolver cuatro problemas reales diferentes. Usa cuatro métodos mínimos para que puedas **ver el patrón estructural** que caracteriza a O(1), O(log n), O(n) y O(n²).

### 1. Orden constante

```java
static int constante(int n){ return 1; }
```

El resultado no depende del tamaño `n`. Para 8, 16 o 1000, este contador conceptual sigue siendo 1. La idea es que la cantidad de trabajo no crece con la entrada.

### 2. Orden logarítmico

```java
while(n > 1){
    n /= 2;
    c++;
}
```

En lugar de avanzar uno por uno, `n` se divide entre 2 en cada vuelta. Para `n=8`: 8 → 4 → 2 → 1, por lo que ocurren 3 iteraciones. Para `n=16`: 16 → 8 → 4 → 2 → 1, ocurren 4.

### 3. Orden lineal

```java
for(int i=0; i<n; i++) c++;
```

El ciclo ejecuta una iteración por cada unidad de `n`. Si duplicas `n`, el contador también se duplica.

### 4. Orden cuadrático

```java
for(int i=0; i<n; i++)
    for(int j=0; j<n; j++)
        c++;
```

Cada una de las `n` iteraciones externas contiene `n` iteraciones internas. El contador realiza `n × n = n²` incrementos.

### 5. Comparación de salidas

| n | O(1) | O(log n) | O(n) | O(n²) |
|---:|---:|---:|---:|---:|
| 8 | 1 | 3 | 8 | 64 |
| 16 | 1 | 4 | 16 | 256 |

Lo importante es describir **cómo cambia cada columna cuando cambia n**. Clasificar por el nombre del método sería una respuesta incompleta.

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ06_OrdenesCrecimiento.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ06_OrdenesCrecimiento
```

## Resultado esperado e interpretación

Para n=8: 1,3,8,64; para n=16: 1,4,16,256.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Duplica n y describe el cambio de cada contador.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Clasificar por una sola ejecución pequeña o por el nombre del método.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

Justifica la clasificación leyendo la estructura que depende de n.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T06 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T06_ClasificaCrecimiento.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** Para n=32: 1,5,32,1024.

---

# EJ07 - Integrador: informe de eficiencia de un ordenamiento

## Qué vamos a resolver

Integrar n, comparaciones, swaps, tiempo y salida en un reporte.

## Qué aprenderás aquí

La salida ordenada demuestra corrección funcional; las métricas explican trabajo. Comparaciones dependen de n en la versión base, swaps del orden inicial y tiempo del entorno.

## Archivo del laboratorio

`FASE_15/estudiante/ejemplos/EJ07_ReporteEficiencia.java`

## Punto de partida

JDK verificado. Lee el código completo antes de ejecutar y localiza datos de entrada, contador y ciclos.

## Código resuelto del ejemplo

```java
import java.util.Arrays;
public class EJ07_ReporteEficiencia {
    static void reporte(int[] entrada){
        int[] v=Arrays.copyOf(entrada,entrada.length);
        long inicio=System.nanoTime(); int comparaciones=0,intercambios=0;
        for(int i=0;i<v.length-1;i++) for(int j=0;j<v.length-1;j++){
            comparaciones++;
            if(v[j]>v[j+1]){int t=v[j];v[j]=v[j+1];v[j+1]=t;intercambios++;}
        }
        long tiempo=System.nanoTime()-inicio;
        System.out.println("entrada="+Arrays.toString(entrada));
        System.out.println("n="+v.length+" comparaciones="+comparaciones+" intercambios="+intercambios+" tiempo_ns="+tiempo);
        System.out.println("salida="+Arrays.toString(v)+" crecimiento=O(n^2)\n");
    }
    public static void main(String[] args){
        reporte(new int[]{5,3,8,4,2});
        reporte(new int[]{1,2,3,4,5});
        reporte(new int[]{7,6,5,4,3,2,1});
    }
}
```

## Lectura pedagógica del código: integrar corrección funcional y evidencia de eficiencia

El último ejemplo combina las piezas trabajadas por separado: copia de entrada, Bubble Sort, contador de comparaciones, contador de intercambios, medición temporal y clasificación de crecimiento.

### 1. ¿Por qué se conserva la entrada original?

```java
int[] v = Arrays.copyOf(entrada, entrada.length);
```

La copia permite ordenar `v` sin destruir `entrada`. Así el reporte puede mostrar exactamente qué datos llegaron al algoritmo y cuál fue la salida.

### 2. ¿Por qué se inicia el cronómetro antes de los ciclos?

```java
long inicio = System.nanoTime();
```

Queremos medir el bloque que realiza el trabajo de ordenamiento. Si iniciáramos el cronómetro después, excluiríamos precisamente la parte que queremos observar.

### 3. ¿Qué responden los dos contadores?

```java
int comparaciones = 0, intercambios = 0;
```

- `comparaciones`: ¿cuántas parejas revisó esta implementación?
- `intercambios`: ¿cuántas de esas parejas tuvieron que cambiar de posición?

El primer valor depende principalmente de los límites de los ciclos; el segundo depende del estado de los datos.

### 4. ¿Qué significa el reporte final?

```java
System.out.println("n=" + v.length +
    " comparaciones=" + comparaciones +
    " intercambios=" + intercambios +
    " tiempo_ns=" + tiempo);
```

Una sola línea reúne tres tipos de evidencia distintos: tamaño, operaciones y tiempo. Debes interpretarlos por separado antes de construir una conclusión.

### 5. ¿Por qué el código imprime `crecimiento=O(n^2)`?

No lo deduce automáticamente. Esa etiqueta representa la clasificación que ya justificaste al leer los ciclos anidados. El programa la imprime como parte del reporte, pero la explicación sigue siendo responsabilidad del estudiante.

### 6. ¿Qué esperamos de los tres casos?

- `[5,3,8,4,2]`: 16 comparaciones y 7 intercambios.
- `[1,2,3,4,5]`: 16 comparaciones y 0 intercambios.
- `[7,6,5,4,3,2,1]`: 36 comparaciones y 21 intercambios.

El tiempo puede cambiar entre ejecuciones.

### Pregunta integradora

Si dos entradas del mismo tamaño realizan las mismas comparaciones pero distintos intercambios, ¿qué evidencia usarías para describir correctamente el comportamiento de esta **implementación concreta** sin generalizar de más?

## Paso 1 - Leer la entrada y la métrica

Identifica el vector o `n`. Después localiza el contador principal. Pregunta: **¿qué instrucción lo incrementa y bajo qué ciclo se encuentra?**

## Paso 2 - Hacer una predicción

Escribe el vector final o el valor de la métrica antes de ejecutar. No aceptes “no sé” como respuesta final: recorre manualmente las iteraciones necesarias hasta justificar una predicción.

## Paso 3 - Compilar

```bash
javac EJ07_ReporteEficiencia.java
```

Si `javac` no muestra errores, el archivo compiló. Si aparece un error, lee primero la línea y el mensaje; no elimines código sin identificar la causa.

## Paso 4 - Ejecutar

```bash
java EJ07_ReporteEficiencia
```

## Resultado esperado e interpretación

Casos de igual n conservan comparaciones; swaps y tiempo pueden cambiar.

No te limites a copiar la salida. Señala qué estructura del código produce el valor observado y qué parte corresponde a la entrada.

## Variante A - cambia una condición

Añade caso ordenado del mismo n; después aumenta n.

## Variante B - obliga a explicar

Mantén la estructura principal y cambia una condición relacionada (orden inicial o tamaño). Antes de ejecutar, escribe qué métrica esperas que cambie y cuál debería permanecer.

## Error controlado

Concluir eficiencia solo porque el vector termina ordenado.

## Por qué ocurrió

El error aparece por confundir el resultado funcional con la evidencia de eficiencia o por no respetar el límite de los ciclos.

## Corrección razonada

Separa corrección, cantidad de trabajo y crecimiento.

## Qué debes poder explicar con tus palabras

Explica la cadena **entrada → estructura de control → contador/salida → conclusión**. Si falta uno de esos cuatro elementos, tu explicación todavía está incompleta.

## T07 - Tarea espejo

Abre `FASE_15/estudiante/tareas/T07_InformeEspejo.java`. El archivo compila, pero conserva un TODO guiado. Complétalo sin copiar la solución docente.

**Criterio para saber si está correcta:** Para los dos casos n=4 deben aparecer 9 comparaciones; para n=5, 16. Los swaps deben variar.

---

## 8. Tareas espejo de consolidación

| TAREA_ID | Ejemplo base | Evidencia requerida |
|---|---|---|
| T01 | EJ01 | salida + clasificación del caso |
| T02 | EJ02 | vector final + comparaciones |
| T03 | EJ03 | comparaciones + swaps + explicación |
| T04 | EJ04 | tabla n/comparaciones |
| T05 | EJ05 | comparaciones + dos tiempos por tamaño |
| T06 | EJ06 | cuatro contadores + explicación de crecimiento |
| T07 | EJ07 | reporte + conclusión de 6-8 líneas |

## 9. Cierre de la sesión - texto listo para leer

Hoy no solo ordenaste vectores: aprendiste a mirar el trabajo que existe detrás del resultado. Empezaste con una búsqueda para reconocer que los casos dependen de la entrada. Después llevaste esa lectura a Bubble Sort y comprobaste que una afirmación de eficiencia debe estar respaldada por la implementación que realmente se ejecuta. Con el código base, un vector ordenado no reduce por sí solo el número de comparaciones porque no existe salida anticipada; en cambio, el orden inicial sí puede cambiar los intercambios. También cambiaste n y conectaste el crecimiento de operaciones con O(n²), y separaste ese análisis de una medición temporal a posteriori que puede variar con el entorno.

Conserva tus tablas, predicciones y reporte integrador. Antes de continuar, debes poder señalar qué contador mide cada cosa y por qué. Para reforzar el tema: https://lideratecacademy.com/blog/ y https://www.youtube.com/@LideratecAcademy .
