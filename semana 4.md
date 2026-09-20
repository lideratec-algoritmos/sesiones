---
course_id: 30710_PROVISIONAL
session_id: S04
module_id: TEMA_04
course_version: V5.7
source_origin: PPT
status: draft
---

# Guía del Estudiante - Tema 04: Matrices

**Curso:** Algoritmo y Estructura de Datos Basado en IA  
**Sesión:** S04 - Tema 04: Matrices  
**Tecnología de práctica:** Java  
**Modalidad del laboratorio:** consola + archivos `.java`

## 1. Propósito de la sesión

En esta sesión vas a construir y ejecutar ejemplos con **arreglos bidimensionales en Java**. El foco es comprender cómo una matriz organiza datos en **filas y columnas**, cómo se accede a cada elemento mediante dos índices y cómo los ciclos permiten recorrer y operar la estructura.

El laboratorio avanza desde una matriz vacía hasta operaciones y matrices especiales. Al terminar habrás trabajado con declaración, valores por defecto, asignación, acceso, recorrido horizontal, suma de matrices, multiplicación por escalar, multiplicación de matrices, matriz cuadrada y matriz poco densa.

El producto observable será una colección de **nueve programas Java pequeños y ejecutables**, cada uno con una tarea espejo para comprobar que puedes transferir el procedimiento sin copiarlo literalmente.

## 2. Resultado observable

### A. Lectura sugerida del docente

Al finalizar esta práctica deberías poder mirar una expresión como `matriz[fila][columna]` y explicar exactamente qué posición representa. También deberías poder declarar una matriz, anticipar sus valores iniciales, modificar posiciones, recorrer una fila y construir operaciones que trabajan sobre múltiples celdas. La ejecución del programa no será la única evidencia: antes de correr el código harás una predicción y, después, interpretarás por qué el resultado coincide o no con ella.

La sesión también introduce dos casos especiales. Una **matriz cuadrada** tiene el mismo número de filas y columnas. Una **matriz poco densa** contiene principalmente ceros; la sesión muestra una forma de guardar únicamente los valores no nulos mediante registros de fila, columna y valor. Comprender estas estructuras significa relacionar la forma de los datos con el código que los procesa.

### B. Desempeños observables

Al terminar podrás:

1. declarar matrices `int[][]` y `String[][]`;
2. interpretar correctamente los índices de fila y columna;
3. asignar y recuperar valores de posiciones concretas;
4. recorrer horizontalmente una fila con `for`;
5. sumar matrices del mismo tamaño;
6. multiplicar una matriz por un escalar;
7. multiplicar matrices cuando sus dimensiones son compatibles;
8. reconocer una matriz cuadrada;
9. representar los elementos no nulos de una matriz poco densa.

### C. Criterio de dominio

Demuestras dominio cuando puedes **predecir**, **ejecutar**, **explicar** y **modificar** el código sin confundir fila con columna, y cuando justificas por qué una operación produce una salida determinada.

## 3. Antes de iniciar

### Debe saber

- variables y tipos básicos en Java;
- arreglos;
- índices que comienzan en 0;
- ciclo `for`;
- lectura básica de salida por consola con `System.out.println`.

### Debe tener disponible

- un JDK funcional;
- una terminal o consola;
- un editor de texto o IDE capaz de guardar archivos `.java`;
- permisos para crear una carpeta de trabajo.

### No se asumirá todavía

No necesitas librerías externas, frameworks, colecciones avanzadas ni álgebra lineal avanzada. En la matriz cuadrada no calcularemos determinantes ni inversas; solo reconoceremos la propiedad de tener igual cantidad de filas y columnas.

## 4. Herramientas y recursos

### JDK

El **JDK** contiene las herramientas necesarias para compilar (`javac`) y ejecutar (`java`) programas Java.

Para este laboratorio basta cualquier JDK compatible con la sintaxis básica utilizada. Como referencia actual, **JDK 25 es una versión LTS**. Si tu institución ya definió otra versión compatible, conserva la institucional.

**Sitio oficial:** https://www.oracle.com/java/technologies/downloads/

No necesitas instalar un framework ni dependencias adicionales.

### Editor

Puedes utilizar el editor o IDE disponible en tu equipo. El laboratorio no depende de una función específica del editor: solo debe poder crear archivos de texto con extensión `.java`.

## 5. Preparación del entorno desde cero

### Ruta A - El entorno ya existe

Abre una terminal y ejecuta:

```bash
java -version
javac -version
```

**Qué debes observar:** ambos comandos deben responder con información de versión. Si `java` funciona pero `javac` no, probablemente tienes un runtime sin compilador o la ruta del JDK no está configurada correctamente.

Crea una carpeta de trabajo y entra en ella. Ejemplo:

```bash
mkdir matrices-java
cd matrices-java
```

### Ruta B - Equipo sin preparar

1. Abre el sitio oficial de descargas de Java.
2. Busca una distribución de JDK compatible con tu sistema operativo. Para esta guía se recomienda una versión LTS vigente; si tu institución define otra, usa esa.
3. Descarga el instalador apropiado para tu sistema.
4. Ejecuta la instalación con los valores institucionales o predeterminados permitidos.
5. Cierra y vuelve a abrir la terminal.
6. Ejecuta `java -version` y `javac -version`.
7. Solo continúa cuando ambos comandos respondan correctamente.

**Error frecuente:** escribir `java` correctamente pero recibir “command not found” o “no se reconoce como un comando”. En ese caso, revisa la instalación y la configuración de PATH de tu sistema; no continúes intentando compilar hasta que la terminal reconozca `javac`.

## 6. Cómo trabajaremos

Cada ejemplo sigue este ciclo:

```text
OBJETIVO
  ↓
CREAR ARCHIVO
  ↓
ESCRIBIR CÓDIGO
  ↓
EXPLICAR
  ↓
PREDECIR
  ↓
COMPILAR
  ↓
EJECUTAR
  ↓
INTERPRETAR
  ↓
MODIFICAR
  ↓
ERROR CONTROLADO
  ↓
TAREA ESPEJO
```


# EJ01 - Declarar una matriz int y observar sus valores por defecto

## Qué vamos a construir

Declarar un arreglo bidimensional de enteros, distinguir filas y columnas y comprobar que sus elementos comienzan en 0.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ01_DeclaracionMatrizInt.java`

## Punto de partida

Carpeta de trabajo creada y JDK operativo. No necesitas un proyecto con librerías externas.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ01_DeclaracionMatrizInt.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ01_DeclaracionMatrizInt {
    public static void main(String[] args) {
        int[][] numerosEnteros = new int[2][3];

        System.out.println("Filas: " + numerosEnteros.length);
        System.out.println("Columnas: " + numerosEnteros[0].length);
        System.out.println("Contenido inicial:");

        for (int fila = 0; fila < numerosEnteros.length; fila++) {
            for (int columna = 0;
                 columna < numerosEnteros[fila].length;
                 columna++) {
                System.out.print(numerosEnteros[fila][columna] + " ");
            }
            System.out.println();
        }

        // ERROR CONTROLADO (dejar comentado):
        // System.out.println(numerosEnteros[2][0]);
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
new int[2][3] -> se reservan 2 filas y 3 columnas -> Java inicializa cada int en 0 -> el recorrido muestra 6 ceros.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ01_DeclaracionMatrizInt` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. La matriz tendrá 2 filas.

2. Cada fila tendrá 3 columnas.

3. Las 6 posiciones mostrarán 0.


## Ejecuta

Compila:

```bash
javac EJ01_DeclaracionMatrizInt.java
```

Ejecuta:

```bash
java EJ01_DeclaracionMatrizInt
```

## Resultado esperado

```text
Filas: 2
Columnas: 3
Contenido inicial:
0 0 0
0 0 0
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia `new int[2][3]` por `new int[3][2]`. Antes de ejecutar, dibuja la nueva forma y predice cuántos ceros aparecerán.

## Variación B

Cambia solo `numerosEnteros[0][1] = 7;` antes del recorrido y anticipa qué celda dejará de ser 0.

## Error controlado

Habilitar temporalmente `numerosEnteros[2][0]` provoca acceso fuera del rango: las filas válidas son 0 y 1.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T01 - Tarea espejo

**Relación:** T01 practica la misma habilidad de EJ01 con datos o dimensiones diferentes.

## Enunciado

Declara `int[][] datos = new int[3][2]`, muestra dimensiones y recorre todos sus elementos.

## Archivo

`T01_MatrizInt_Tarea.java`

## Restricción

No inicialices manualmente los seis elementos; debes observar el valor por defecto de Java.

## Pista

Usa `datos.length` para filas y `datos[fila].length` para columnas.

## Evidencia que debes mostrar

Captura o copia de la salida mostrando 3 filas, 2 columnas y seis ceros.

## Cómo saber si está correcta

Debe verse exactamente una estructura 3x2 y ningún acceso fuera de rango.


# EJ02 - Matriz String: asignar una fila y observar null

## Qué vamos a construir

Comprobar que una matriz de referencias `String` comienza con `null` y asignar valores a posiciones concretas.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ02_MatrizStringAsignacion.java`

## Punto de partida

EJ01 comprendido: ya reconoces la forma `[fila][columna]` y los índices empiezan en 0.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ02_MatrizStringAsignacion.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ02_MatrizStringAsignacion {
    public static void main(String[] args) {
        String[][] nombres = new String[2][2];

        nombres[0][0] = "Arturo";
        nombres[0][1] = "Parra";

        System.out.println("Fila 0: "
                + nombres[0][0] + " " + nombres[0][1]);
        System.out.println("Fila 1: "
                + nombres[1][0] + " " + nombres[1][1]);

        // ERROR CONTROLADO (dejar comentado):
        // System.out.println(nombres[1][0].toUpperCase());
        // La posición contiene null; invocar un método sobre null falla.
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
new String[2][2] -> cuatro referencias null -> se asignan dos posiciones de la fila 0 -> la fila 1 permanece null.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ02_MatrizStringAsignacion` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. La fila 0 mostrará Arturo Parra.

2. La fila 1 conservará null null.

3. Asignar una posición no cambia el tamaño de la matriz.


## Ejecuta

Compila:

```bash
javac EJ02_MatrizStringAsignacion.java
```

Ejecuta:

```bash
java EJ02_MatrizStringAsignacion
```

## Resultado esperado

```text
Fila 0: Arturo Parra
Fila 1: null null
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Asigna `nombres[1][0] = "Lucía";` y predice la salida antes de ejecutar.

## Variación B

Intercambia los valores de `[0][0]` y `[0][1]` y explica qué cambió: el contenido, no la estructura.

## Error controlado

Invocar un método como `toUpperCase()` sobre una posición que todavía contiene `null` provoca un error porque no hay un objeto String en esa celda.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T02 - Tarea espejo

**Relación:** T02 practica la misma habilidad de EJ02 con datos o dimensiones diferentes.

## Enunciado

Crea una matriz `String[2][3]`; completa la fila 0 con Ana, Torres y Lima, deja la fila 1 sin asignar y muestra ambas.

## Archivo

`T02_MatrizString_Tarea.java`

## Restricción

No reemplaces los `null` de la fila 1 por texto manual; deben provenir del estado inicial de la matriz.

## Pista

Las posiciones de la primera fila son `[0][0]`, `[0][1]` y `[0][2]`.

## Evidencia que debes mostrar

Salida donde la fila 0 contiene los tres textos y la fila 1 contiene tres `null`.

## Cómo saber si está correcta

La matriz debe seguir siendo 2x3 y solo la primera fila debe tener valores asignados.


# EJ03 - Leer una posición concreta con [fila][columna]

## Qué vamos a construir

Interpretar correctamente dos índices y recuperar un valor puntual de una tabla de comidas.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ03_AccesoPosicion.java`

## Punto de partida

Matriz `comidas` inicializada con 3 filas y 7 columnas.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ03_AccesoPosicion.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ03_AccesoPosicion {
    public static void main(String[] args) {
        String[][] comidas = {
            {"Avena", "Cereal", "Huevo", "Yogur",
             "Fruta", "Pan tostado", "Hotcakes"},
            {"Pollo", "Sándwich", "Verduras", "Atún",
             "Bistec", "Champiñones", "Espagueti"},
            {"Frijoles", "Quesadillas", "Estofado", "Picadillo",
             "Lasaña", "Ensalada", "Pizza"}
        };

        String cenaJueves = comidas[2][3];
        System.out.println("La cena del jueves es: " + cenaJueves);

        // ERROR CONTROLADO (dejar comentado):
        // System.out.println(comidas[3][2]);
        // Solo existen filas con índices 0, 1 y 2.
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
comidas[2][3] -> fila 2 -> columna 3 -> valor Picadillo -> se guarda en cenaJueves -> se imprime.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ03_AccesoPosicion` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. El primer índice selecciona la fila 2.

2. El segundo índice selecciona la columna 3.

3. El resultado será Picadillo.


## Ejecuta

Compila:

```bash
javac EJ03_AccesoPosicion.java
```

Ejecuta:

```bash
java EJ03_AccesoPosicion
```

## Resultado esperado

```text
La cena del jueves es: Picadillo
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Lee `comidas[0][4]` y explica por qué el resultado es Fruta.

## Variación B

Lee `comidas[1][6]` y comprueba que el último índice de columna válido es 6.

## Error controlado

`comidas[3][2]` intenta entrar en una cuarta fila inexistente. En una matriz con 3 filas los índices de fila válidos son 0, 1 y 2.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T03 - Tarea espejo

**Relación:** T03 practica la misma habilidad de EJ03 con datos o dimensiones diferentes.

## Enunciado

Obtén `comidas[1][5]`, guárdalo en `almuerzoSabado` y muestra el valor.

## Archivo

`T03_AccesoPosicion_Tarea.java`

## Restricción

Debes usar acceso directo; no recorras toda la matriz para encontrar el dato.

## Pista

Ubica primero la fila 1 y después la columna 5.

## Evidencia que debes mostrar

Salida que muestre el contenido de la posición `[1][5]`.

## Cómo saber si está correcta

El valor esperado es Champiñones.


# EJ04 - Recorrer horizontalmente una fila

## Qué vamos a construir

Mantener fijo el índice de fila y variar únicamente la columna con un ciclo `for`.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ04_RecorridoHorizontal.java`

## Punto de partida

La matriz `comidas` ya contiene 3x7 valores y el estudiante sabe leer una posición directa.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ04_RecorridoHorizontal.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ04_RecorridoHorizontal {
    public static void main(String[] args) {
        String[][] comidas = {
            {"Avena", "Cereal", "Huevo", "Yogur",
             "Fruta", "Pan tostado", "Hotcakes"},
            {"Pollo", "Sándwich", "Verduras", "Atún",
             "Bistec", "Champiñones", "Espagueti"},
            {"Frijoles", "Quesadillas", "Estofado", "Picadillo",
             "Lasaña", "Ensalada", "Pizza"}
        };

        System.out.println("Contenido de la primera fila:");
        for (int columna = 0;
             columna < comidas[0].length;
             columna++) {
            System.out.println(comidas[0][columna]);
        }

        // ERROR CONTROLADO (dejar comentado):
        // for (int columna = 0;
        //      columna <= comidas[0].length;
        //      columna++) {
        //     System.out.println(comidas[0][columna]);
        // }
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
fila fija 0 -> columna inicia en 0 -> columna aumenta hasta 6 -> se imprime comidas[0][columna] en cada repetición.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ04_RecorridoHorizontal` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. El índice de fila permanecerá en 0.

2. La variable `columna` tomará los valores 0 a 6.

3. Se imprimirán 7 elementos.


## Ejecuta

Compila:

```bash
javac EJ04_RecorridoHorizontal.java
```

Ejecuta:

```bash
java EJ04_RecorridoHorizontal
```

## Resultado esperado

```text
Contenido de la primera fila:
Avena
Cereal
Huevo
Yogur
Fruta
Pan tostado
Hotcakes
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia la fila fija de 0 a 1 sin tocar el límite del ciclo. Predice los siete valores.

## Variación B

Cambia `println` por `print(... + " | ")` para observar el mismo recorrido en una sola línea.

## Error controlado

Si usas `columna <= comidas[0].length`, cuando `columna` vale 7 intentas acceder a una columna inexistente. El operador correcto es `<`.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T04 - Tarea espejo

**Relación:** T04 practica la misma habilidad de EJ04 con datos o dimensiones diferentes.

## Enunciado

Recorre horizontalmente la fila 2 y muestra sus siete elementos.

## Archivo

`T04_RecorridoHorizontal_Tarea.java`

## Restricción

El índice de fila debe permanecer fijo en 2; la variable del ciclo representa solo la columna.

## Pista

La condición puede usar `columna < comidas[2].length`.

## Evidencia que debes mostrar

Salida con Frijoles, Quesadillas, Estofado, Picadillo, Lasaña, Ensalada y Pizza.

## Cómo saber si está correcta

Deben aparecer exactamente siete valores y en el mismo orden de la fila 2.


# EJ05 - Sumar dos matrices del mismo tamaño

## Qué vamos a construir

Aplicar la suma posición por posición y reconocer que ambas matrices deben tener el mismo tamaño.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ05_SumaMatrices.java`

## Punto de partida

Dos matrices `A` y `B` de 2x2 y una matriz `C` vacía de 2x2.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ05_SumaMatrices.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ05_SumaMatrices {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2},
            {3, 4}
        };
        int[][] B = {
            {5, 6},
            {7, 8}
        };
        int[][] C = new int[2][2];

        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                C[fila][columna] = A[fila][columna]
                        + B[fila][columna];
            }
        }

        System.out.println("Resultado de la suma:");
        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                System.out.print(C[fila][columna] + " ");
            }
            System.out.println();
        }
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
para cada [fila][columna] -> leer A y B -> sumar -> guardar en la misma posición de C -> mostrar C.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ05_SumaMatrices` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. C[0][0] será 1 + 5 = 6.

2. C[1][1] será 4 + 8 = 12.

3. La matriz resultado tendrá el mismo tamaño 2x2.


## Ejecuta

Compila:

```bash
javac EJ05_SumaMatrices.java
```

Ejecuta:

```bash
java EJ05_SumaMatrices
```

## Resultado esperado

```text
Resultado de la suma:
6 8
10 12
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia `B[0][0]` a 10 y predice qué única posición de C cambia.

## Variación B

Usa valores negativos en una sola posición y explica por qué la regla de suma no cambia.

## Error controlado

Intentar sumar matrices de tamaños distintos rompe la condición de la operación: ya no existe una correspondencia uno-a-uno para todas las posiciones.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T05 - Tarea espejo

**Relación:** T05 practica la misma habilidad de EJ05 con datos o dimensiones diferentes.

## Enunciado

Suma las matrices 2x3 indicadas en T05 y muestra la matriz C.

## Archivo

`T05_SumaMatrices_Tarea.java`

## Restricción

No calcules el resultado manualmente en la declaración de C; debes obtenerlo dentro de los ciclos.

## Pista

Usa las mismas coordenadas `[fila][columna]` en A, B y C.

## Evidencia que debes mostrar

Salida de dos filas con tres resultados cada una.

## Cómo saber si está correcta

El resultado esperado es `3 5 3` y `4 3 4`.


# EJ06 - Multiplicar una matriz por un escalar

## Qué vamos a construir

Multiplicar cada elemento por un único número y observar cómo cambia el contenido sin cambiar las dimensiones.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ06_MultiplicacionEscalar.java`

## Punto de partida

Matriz A de 2x2 y variable `escalar = 3`.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ06_MultiplicacionEscalar.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ06_MultiplicacionEscalar {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2},
            {3, 4}
        };
        int escalar = 3;

        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                A[fila][columna] = A[fila][columna] * escalar;
            }
        }

        System.out.println("Resultado:");
        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                System.out.print(A[fila][columna] + " ");
            }
            System.out.println();
        }

        // ERROR LÓGICO CONTROLADO:
        // Sustituir temporalmente la multiplicación por:
        // A[fila][columna] = escalar;
        // No falla el programa, pero destruye el valor original.
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
recorrer cada celda -> leer valor actual -> multiplicar por 3 -> guardar de nuevo en la misma celda -> imprimir.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ06_MultiplicacionEscalar` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. 1 se convertirá en 3.

2. 4 se convertirá en 12.

3. La matriz seguirá siendo 2x2.


## Ejecuta

Compila:

```bash
javac EJ06_MultiplicacionEscalar.java
```

Ejecuta:

```bash
java EJ06_MultiplicacionEscalar
```

## Resultado esperado

```text
Resultado:
3 6
9 12
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia el escalar a 2 y predice los cuatro valores.

## Variación B

Usa escalar 0 y explica por qué todas las celdas quedan en 0.

## Error controlado

Asignar `A[fila][columna] = escalar` no multiplica: reemplaza cada valor por el mismo número. Es un error lógico, no de sintaxis.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T06 - Tarea espejo

**Relación:** T06 practica la misma habilidad de EJ06 con datos o dimensiones diferentes.

## Enunciado

Multiplica la matriz 2x3 propuesta por el escalar 4 y muestra el resultado.

## Archivo

`T06_MultiplicacionEscalar_Tarea.java`

## Restricción

Modifica cada posición mediante los ciclos; no declares de antemano la matriz resultado con valores calculados.

## Pista

La operación central es `A[fila][columna] = A[fila][columna] * escalar`.

## Evidencia que debes mostrar

Salida con dos filas y tres valores transformados.

## Cómo saber si está correcta

Resultado esperado: `8 4 0` y `16 12 20`.


# EJ07 - Multiplicar dos matrices

## Qué vamos a construir

Aplicar la condición columnas de A = filas de B y construir cada celda del resultado mediante productos acumulados.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ07_MultiplicacionMatrices.java`

## Punto de partida

A y B son 2x2; C comienza con ceros.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ07_MultiplicacionMatrices.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ07_MultiplicacionMatrices {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2},
            {3, 4}
        };
        int[][] B = {
            {2, 0},
            {1, 2}
        };
        int[][] C = new int[2][2];

        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                for (int k = 0; k < 2; k++) {
                    C[fila][columna] +=
                            A[fila][k] * B[k][columna];
                }
            }
        }

        System.out.println("Resultado:");
        for (int fila = 0; fila < 2; fila++) {
            for (int columna = 0; columna < 2; columna++) {
                System.out.print(C[fila][columna] + " ");
            }
            System.out.println();
        }

        // ERROR LÓGICO CONTROLADO:
        // No reemplazar el producto acumulado por suma directa
        // A[fila][columna] + B[fila][columna].
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
elegir una fila de A -> elegir una columna de B -> recorrer k -> acumular A[fila][k] * B[k][columna] -> guardar en C.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ07_MultiplicacionMatrices` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. C[0][0] = 1*2 + 2*1 = 4.

2. C[0][1] = 1*0 + 2*2 = 4.

3. La salida será una matriz 2x2.


## Ejecuta

Compila:

```bash
javac EJ07_MultiplicacionMatrices.java
```

Ejecuta:

```bash
java EJ07_MultiplicacionMatrices
```

## Resultado esperado

```text
Resultado:
4 4
10 8
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia `B[0][0]` de 2 a 1 y calcula primero solo C[0][0].

## Variación B

Antes de ejecutar, escribe en papel los dos productos que forman C[1][1].

## Error controlado

Sumar directamente A[fila][columna] + B[fila][columna] no es multiplicación matricial; omite el producto fila-por-columna y la acumulación en k.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T07 - Tarea espejo

**Relación:** T07 practica la misma habilidad de EJ07 con datos o dimensiones diferentes.

## Enunciado

Multiplica una matriz A de 2x3 por una matriz B de 3x2 y obtén C de 2x2.

## Archivo

`T07_MultiplicacionMatrices_Tarea.java`

## Restricción

Debes usar tres ciclos: fila, columna y k.

## Pista

En T07, k recorre 3 posiciones porque A tiene 3 columnas y B tiene 3 filas.

## Evidencia que debes mostrar

Salida de la matriz C completa.

## Cómo saber si está correcta

Resultado esperado: `7 4` y `16 13`.


# EJ08 - Reconocer y recorrer una matriz cuadrada

## Qué vamos a construir

Identificar una matriz cuadrada porque tiene el mismo número de filas y columnas y mostrar sus elementos.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ08_MatrizCuadrada.java`

## Punto de partida

Matriz 3x3 inicializada con los valores del 1 al 9.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ08_MatrizCuadrada.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ08_MatrizCuadrada {
    public static void main(String[] args) {
        int[][] matriz = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        boolean esCuadrada =
                matriz.length == matriz[0].length;

        System.out.println("¿Es cuadrada? " + esCuadrada);
        for (int fila = 0; fila < matriz.length; fila++) {
            for (int columna = 0;
                 columna < matriz[fila].length;
                 columna++) {
                System.out.print(matriz[fila][columna] + " ");
            }
            System.out.println();
        }

        // CONTRASTE CONTROLADO:
        // Una matriz de 3x2 no cumple filas == columnas.
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
matriz.length -> filas 3; matriz[0].length -> columnas 3; comparar -> true; recorrer e imprimir.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ08_MatrizCuadrada` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. La comparación de filas y columnas será verdadera.

2. Se imprimirán 9 valores.

3. La forma será 3x3.


## Ejecuta

Compila:

```bash
javac EJ08_MatrizCuadrada.java
```

Ejecuta:

```bash
java EJ08_MatrizCuadrada
```

## Resultado esperado

```text
¿Es cuadrada? true
1 2 3
4 5 6
7 8 9
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia la estructura a 3x2 y comprueba que la comparación produce `false`.

## Variación B

Usa una matriz 4x4 con otros valores: la propiedad cuadrada depende de dimensiones, no de los números almacenados.

## Error controlado

Confundir “matriz cuadrada” con “todos los valores iguales” es un error conceptual: la propiedad se refiere a filas y columnas.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T08 - Tarea espejo

**Relación:** T08 practica la misma habilidad de EJ08 con datos o dimensiones diferentes.

## Enunciado

Con la matriz 4x4 de T08, calcula `esCuadrada` y muestra la matriz completa.

## Archivo

`T08_MatrizCuadrada_Tarea.java`

## Restricción

No uses determinante ni inversa; esta tarea solo verifica la propiedad de dimensión enseñada en la sesión.

## Pista

Compara `matriz.length` con `matriz[0].length`.

## Evidencia que debes mostrar

Salida `¿Es cuadrada? true` seguida de las cuatro filas.

## Cómo saber si está correcta

Deben aparecer 16 valores y la verificación debe ser verdadera.


# EJ09 - Matriz poco densa: forma normal y registros no nulos

## Qué vamos a construir

Comparar una matriz con mayoría de ceros frente a una representación que guarda solo fila, columna y valor de los elementos no nulos.

## Qué aprenderás aquí

Al terminar este ejemplo deberás poder explicar el flujo sin mirar la salida como única fuente de verdad.

## Archivos que vamos a crear o modificar

- `EJ09_MatrizPocoDensa.java`

## Punto de partida

Matriz 4x4 con solo dos valores diferentes de cero: 5 y 8.

## Paso 1 - Crear el archivo

Crea un archivo llamado:

```text
EJ09_MatrizPocoDensa.java
```

**Por qué ahora:** cada ejemplo usa una clase independiente. Así puedes compilarlo y ejecutarlo sin depender de los demás.

## Paso 2 - Escribir el código

Copia el siguiente código en el archivo:

```java
public class EJ09_MatrizPocoDensa {
    public static void main(String[] args) {
        int[][] matriz = {
            {0, 0, 5, 0},
            {0, 0, 0, 0},
            {0, 8, 0, 0},
            {0, 0, 0, 0}
        };

        int[][] sparse = {
            {0, 2, 5},
            {2, 1, 8}
        };

        System.out.println("Matriz poco densa:");
        for (int fila = 0; fila < matriz.length; fila++) {
            for (int columna = 0;
                 columna < matriz[fila].length;
                 columna++) {
                System.out.print(matriz[fila][columna] + " ");
            }
            System.out.println();
        }

        System.out.println("Elementos no nulos:");
        for (int i = 0; i < sparse.length; i++) {
            System.out.println("Fila: " + sparse[i][0]
                    + " Columna: " + sparse[i][1]
                    + " Valor: " + sparse[i][2]);
        }

        System.out.println("Posiciones matriz normal: 16");
        System.out.println("Registros sparse: " + sparse.length);

        // ERROR LÓGICO CONTROLADO:
        // Guardar también los ceros en sparse elimina la ventaja
        // de conservar solo los valores diferentes de cero.
    }
}
```

## Paso 3 - Lectura pedagógica del código

**Mapa mental:**

```text
matriz normal 4x4 -> 16 posiciones -> identificar 2 no nulos -> sparse con dos registros {fila,columna,valor} -> recorrer solo esos registros.
```

Lee el programa desde afuera hacia adentro:

- `public class EJ09_MatrizPocoDensa` define la clase cuyo nombre coincide con el archivo.
- `public static void main(String[] args)` es el punto de entrada del programa.
- Cada expresión `matriz[fila][columna]` utiliza el primer índice para la fila y el segundo para la columna.
- Los ciclos `for` avanzan únicamente sobre los índices que el ejemplo necesita recorrer.
- La salida por consola sirve como evidencia observable del estado de la matriz.

## Paso 4 - Archivo completo al terminar este ejemplo

El archivo completo esperado es exactamente el bloque mostrado arriba. Antes de compilar, revisa:

1. nombre de clase = nombre de archivo;
2. llaves de apertura y cierre;
3. corchetes `[][]` de las matrices;
4. punto y coma al final de las instrucciones;
5. límites de los ciclos.

## Antes de ejecutar: predicción


1. La matriz normal mostrará 16 posiciones.

2. Sparse tendrá 2 registros.

3. Los registros serán (0,2,5) y (2,1,8).


## Ejecuta

Compila:

```bash
javac EJ09_MatrizPocoDensa.java
```

Ejecuta:

```bash
java EJ09_MatrizPocoDensa
```

## Resultado esperado

```text
Matriz poco densa:
0 0 5 0
0 0 0 0
0 8 0 0
0 0 0 0
Elementos no nulos:
Fila: 0 Columna: 2 Valor: 5
Fila: 2 Columna: 1 Valor: 8
Posiciones matriz normal: 16
Registros sparse: 2
```

## Cómo interpretarlo

Compara cada línea de la salida con tu predicción. No te limites a verificar que “salió igual”: identifica qué instrucción produjo cada cambio de estado y qué índice seleccionó cada valor.

## Variación A

Cambia el 8 a 0 y explica cuántos registros debería conservar ahora la representación sparse.

## Variación B

Agrega un valor 4 en `[3][3]` y añade su registro `{3,3,4}`.

## Error controlado

Guardar también los ceros como registros sparse contradice el propósito mostrado en la sesión: dejarías de aprovechar que la mayoría son 0.

**Regla:** habilita errores solo de forma temporal y vuelve a dejar el archivo compilable antes de continuar.

## Qué debes poder explicar con tus palabras

1. ¿Qué representa el primer índice?
2. ¿Qué representa el segundo índice?
3. ¿Qué parte del código cambia el contenido de la matriz?
4. ¿Qué parte solo lee o muestra datos?
5. ¿Qué límite evita salir del rango válido?

# T09 - Tarea espejo

**Relación:** T09 practica la misma habilidad de EJ09 con datos o dimensiones diferentes.

## Enunciado

Para la matriz 5x5 de T09, construye tres registros `{fila,columna,valor}` para 7, 3 y 9 y muéstralos.

## Archivo

`T09_MatrizPocoDensa_Tarea.java`

## Restricción

Solo deben existir registros para valores diferentes de cero.

## Pista

Las coordenadas son 7 -> [0][4], 3 -> [2][1], 9 -> [4][3].

## Evidencia que debes mostrar

Salida con exactamente tres registros no nulos.

## Cómo saber si está correcta

Los registros correctos son `{0,4,7}`, `{2,1,3}` y `{4,3,9}`.


# 7. Cierre de la sesión

## Lo que debes conservar

- Una matriz organiza datos mediante **filas y columnas**.
- En Java, el acceso usa dos índices: `[fila][columna]`.
- Los arreglos `int` comienzan con `0`; las posiciones de arreglos de referencias como `String` comienzan con `null`.
- Un recorrido horizontal mantiene fija la fila y cambia la columna.
- La suma requiere matrices del mismo tamaño.
- La multiplicación por escalar aplica el mismo número a cada elemento.
- La multiplicación de matrices exige que las columnas de A coincidan con las filas de B.
- Una matriz cuadrada tiene igual cantidad de filas y columnas.
- Una matriz poco densa contiene principalmente ceros; la sesión muestra cómo conservar solo fila, columna y valor de los elementos no nulos.

## Checklist final del estudiante

- [ ] Compilé y ejecuté EJ01-EJ09.
- [ ] Hice una predicción antes de cada ejecución importante.
- [ ] Completé T01-T09 sin copiar una solución docente.
- [ ] Puedo explicar fila y columna sin invertirlas.
- [ ] Puedo justificar por qué un índice fuera de rango falla.
- [ ] Puedo explicar la diferencia entre matriz normal y representación de valores no nulos.

## Recurso opcional posterior

Si deseas validar tus resultados o empezar desde archivos ya preparados, puedes usar el laboratorio descargable de FASE 15. Ese laboratorio es un recurso de comprobación; esta guía contiene todo lo necesario para construir los ejemplos desde cero.
