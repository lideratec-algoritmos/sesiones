# GUÍA DEL ESTUDIANTE

## Algoritmo y Estructura de Datos basado en IA

**Sesión 01: Datos y algoritmos**  
**Práctica:** Fundamentos de algoritmos, datos y estructuras de datos  
**Elaborado por el docente**  
**Proyecto académico:** Lideratec Academy

---

## 1. Propósito de la práctica

Esta guía te permitirá reconocer cómo se relacionan los datos, los algoritmos y las estructuras de datos en la resolución de problemas. Trabajarás desde la identificación de entradas, procesos y salidas hasta la clasificación de estructuras de datos y una implementación básica en Java con un arreglo.

La meta no es memorizar definiciones aisladas. La meta es que puedas observar un problema, identificar qué datos intervienen, ordenar los pasos de solución y seleccionar una forma adecuada de organizar la información.

## 2. Resultado de aprendizaje observable

Al finalizar la sesión podrás:

- Definir un algoritmo como una secuencia ordenada, precisa, definida y finita de pasos.
- Identificar entrada, proceso y salida en un problema sencillo.
- Diferenciar datos enteros, reales, lógicos, caracteres y cadenas.
- Clasificar estructuras de datos según disposición, variación de tamaño y lugar de almacenamiento.
- Diferenciar estructuras estáticas y dinámicas a partir de sus características.
- Relacionar la elección de una estructura de datos con el algoritmo que procesa la información.
- Ejecutar un ejemplo básico en Java que utiliza datos y un arreglo.

## 3. Duración sugerida

**110 minutos**, distribuidos en explicación, demostración, práctica guiada, comprobación y cierre.

## 4. Conocimientos previos mínimos

Para desarrollar esta práctica necesitas:

- Reconocer qué es un problema y qué significa resolverlo mediante pasos.
- Manejar operaciones aritméticas básicas.
- Poder crear y guardar un archivo de texto.
- Para el bloque Java: saber abrir una terminal o un entorno Java ya instalado. No se requiere dominar programación orientada a objetos.

## 5. Herramientas y recursos

| Componente | Función en la práctica | ¿Debe instalarse? | Orden |
|---|---|---:|---:|
| Cuaderno o editor de texto | Diseñar algoritmos y responder actividades | No | Primero |
| JDK de Java | Compilar y ejecutar el ejemplo Java | Solo si el equipo no tiene Java | Después |
| Terminal o consola | Comprobar Java y ejecutar el programa | No, forma parte del sistema | Después |

### Ruta recomendada para Java

Si el laboratorio ya tiene Java instalado, **no reinstales nada**. Primero ejecuta la comprobación indicada en esta guía.

Si el equipo no tiene un JDK y el docente no ha definido otra versión institucional, utiliza como referencia **JDK 25 LTS**, porque es una versión de soporte de largo plazo disponible actualmente.

Página oficial de descarga:

https://www.oracle.com/java/technologies/downloads/

En la página oficial:

1. Ubica la sección **JDK 25**.
2. Selecciona el sistema operativo correspondiente.
3. En Windows, utiliza el instalador para arquitectura x64 si tu equipo institucional es x64.
4. No selecciones versiones antiguas únicamente porque aparezcan en la misma página.
5. Si tu institución utiliza otra distribución o versión de Java, sigue la versión indicada por el docente.

> **Importante:** para esta sesión, Java es un medio de implementación. Los conceptos principales son algoritmo, datos y estructuras de datos.

## 6. Preparación del entorno

### Paso 1. Comprobar si Java ya está instalado

**Objetivo:** verificar si el equipo puede ejecutar Java antes de intentar instalarlo.

**Dónde hacerlo:** terminal, PowerShell o símbolo del sistema.

**Acción:** abre una terminal y escribe:

```text
java -version
```

Luego comprueba el compilador:

```text
javac -version
```

**Qué debería ocurrir:** la terminal debe mostrar una versión de Java y una versión de `javac`.

**Cómo comprobarlo:** si ambos comandos muestran una versión, el entorno está listo para el ejemplo de esta sesión.

**Si aparece un problema:** si el sistema indica que el comando no se reconoce, Java puede no estar instalado o no estar disponible en la variable de entorno `PATH`. Informa al docente antes de modificar configuraciones del laboratorio.

**Antes de continuar:** confirma que puedes ejecutar `java -version` y `javac -version`, o que el docente ha indicado otra forma de ejecutar Java.

### Paso 2. Instalar Java solo si es necesario

**Objetivo:** disponer de un JDK cuando el equipo no lo tenga.

**Dónde hacerlo:** página oficial de Java y sistema operativo del equipo.

**Acción:** descarga el instalador correspondiente a la versión institucional o, en ausencia de otra indicación, JDK 25 LTS.

**Qué debería ocurrir:** al finalizar la instalación, la aplicación debe quedar disponible para el sistema.

**Cómo comprobarlo:** cierra y vuelve a abrir la terminal y ejecuta nuevamente:

```text
java -version
javac -version
```

**Si aparece un problema:** no instales múltiples versiones sin autorización. Si el laboratorio exige permisos administrativos, solicita apoyo al docente o al responsable del laboratorio.

### Punto de control

- [ ] Identifiqué la herramienta principal que utilizaré.
- [ ] Comprobé si Java ya estaba instalado.
- [ ] Instalé Java solo si fue necesario y autorizado.
- [ ] Ejecuté `java -version`.
- [ ] Ejecuté `javac -version`.
- [ ] Puedo continuar con la práctica.

---

# BLOQUE 1. ¿QUÉ ES UN ALGORITMO?

**Objetivo:** reconocer las características fundamentales de un algoritmo.

**Concepto trabajado:** algoritmo, precisión, definición y finitud.

**Explicación breve:** un algoritmo es una secuencia ordenada de pasos que conduce a la solución de un problema. Para que sea útil, sus pasos deben indicar qué hacer y en qué orden, producir el mismo resultado cuando se reciben los mismos datos y terminar después de una cantidad finita de pasos.

## Ejemplo guiado: calcular el total de una compra

Supón que una persona compra 3 unidades de un producto cuyo precio unitario es 20.

Un algoritmo sencillo puede expresarse así:

```text
1. Recibir el precio unitario.
2. Recibir la cantidad.
3. Multiplicar precio por cantidad.
4. Mostrar el total.
5. Finalizar.
```

### Paso a paso

1. **Precisión:** cada paso indica una acción concreta.
2. **Definición:** con precio 20 y cantidad 3, el resultado siempre será 60.
3. **Finitud:** el algoritmo termina después de mostrar el total.

### Actividad para desarrollar

Diseña un algoritmo en lenguaje natural para calcular el promedio de tres notas.

**Espacio para responder:**

1. ________________________________________________________________
2. ________________________________________________________________
3. ________________________________________________________________
4. ________________________________________________________________
5. ________________________________________________________________

**Resultado esperado:** una secuencia ordenada, sin ambigüedades y con un final identificable.

**Error frecuente:** escribir una intención general como “calcular el promedio” sin describir los pasos que permiten obtenerlo.

**Mini reto:** revisa tu algoritmo y marca dónde se evidencia que es preciso, definido y finito.

---

# BLOQUE 2. ENTRADA, PROCESO Y SALIDA

**Objetivo:** separar correctamente las tres partes fundamentales de un algoritmo.

**Concepto trabajado:** entrada, proceso y salida.

**Explicación breve:** todo algoritmo necesita información de partida, operaciones para transformar esa información y un resultado final.

## Ejemplo guiado

Problema: calcular el total de una compra.

| Parte | Contenido |
|---|---|
| Entrada | Precio unitario y cantidad |
| Proceso | Multiplicar precio unitario por cantidad |
| Salida | Total de la compra |

### Actividad para desarrollar

Problema: calcular el área de un rectángulo.

Completa:

- **Entrada:** _______________________________________________
- **Proceso:** ______________________________________________
- **Salida:** _______________________________________________

### Punto de control

Responde:

1. ¿Una salida puede existir sin que el algoritmo haya realizado un proceso? ¿Por qué?

   ________________________________________________________________

2. Si falta un dato necesario, ¿qué parte del algoritmo está incompleta?

   ________________________________________________________________

3. En el cálculo del promedio de tres notas, ¿cuáles son las entradas?

   ________________________________________________________________

**Error frecuente:** confundir el dato de entrada con el resultado. Por ejemplo, en una compra la cantidad es entrada; el total es salida.

---

# BLOQUE 3. DATOS Y TIPOS DE DATOS

**Objetivo:** identificar el tipo de dato adecuado según la información que se desea representar.

**Concepto trabajado:** dato, entero, real, lógico, carácter y cadena.

**Explicación breve:** un dato representa un objeto o valor con el que trabaja un algoritmo. El tipo de dato permite describir qué clase de valor se está manejando.

## Tabla de referencia

| Tipo | Ejemplo | Interpretación |
|---|---|---|
| Entero | `25` | Valor numérico sin parte decimal |
| Real | `19.90` | Valor numérico con parte decimal |
| Lógico | `true` / `false` | Condición de verdad o falsedad |
| Carácter | `'A'` | Un solo carácter |
| Cadena | `"Lideratec"` | Secuencia de caracteres |

## Actividad guiada: clasifica los datos

Completa el tipo de dato más apropiado.

| Dato | Valor de ejemplo | Tipo |
|---|---:|---|
| Edad de un estudiante | 19 | __________________ |
| Precio de un producto | 29.90 | __________________ |
| ¿Está activo? | true | __________________ |
| Inicial del apellido | 'E' | __________________ |
| Nombre completo | "Ana Torres" | __________________ |

### Aplicación

Imagina que un algoritmo registra un producto. Propón un dato de cada tipo:

- Entero: __________________________________________
- Real: ____________________________________________
- Lógico: __________________________________________
- Carácter: ________________________________________
- Cadena: __________________________________________

**Resultado esperado:** cada valor debe corresponder coherentemente con el tipo elegido.

**Error frecuente:** tratar como número un valor que solo se utiliza como texto, o usar un carácter cuando se necesita una cadena completa.

---

# BLOQUE 4. CLASIFICACIÓN DE LAS ESTRUCTURAS DE DATOS

**Objetivo:** clasificar estructuras de datos usando los tres criterios trabajados en la sesión.

**Concepto trabajado:** disposición, variación de tamaño y lugar de almacenamiento.

## 4.1 Según su disposición

- **Lineales:** los elementos se organizan de forma secuencial. Ejemplos trabajados: arreglos, listas y diccionarios.
- **No lineales:** permiten una disposición no secuencial. Ejemplos trabajados: árboles y grafos.

## 4.2 Según su variación en tamaño

- **Estáticas:** mantienen un tamaño fijo durante la ejecución.
- **Dinámicas:** pueden aumentar o disminuir su cantidad de elementos durante la ejecución.

## 4.3 Según el lugar de almacenamiento

- **Volátiles:** se mantienen en memoria mientras el programa está en ejecución y desaparecen al finalizar.
- **Permanentes:** se almacenan en dispositivos auxiliares y perduran en el tiempo, como los ficheros.

## Actividad de clasificación

Completa la tabla con la clasificación indicada en la sesión.

| Caso | Clasificación principal | Justificación breve |
|---|---|---|
| Arreglo de 10 notas | __________________ | ______________________________ |
| Lista enlazada que crece al agregar elementos | __________________ | ______________________________ |
| Árbol que representa una jerarquía | __________________ | ______________________________ |
| Fichero guardado en almacenamiento | __________________ | ______________________________ |
| Arreglo que vive durante la ejecución | __________________ | ______________________________ |

### Pregunta de comprobación

¿Una estructura puede ser clasificada utilizando más de un criterio? Explica con un ejemplo.

________________________________________________________________

________________________________________________________________

**Error frecuente:** pensar que “lineal”, “estática” y “volátil” son categorías excluyentes entre sí. Cada término responde a un criterio diferente.

---

# BLOQUE 5. ESTRUCTURAS ESTÁTICAS Y DINÁMICAS

**Objetivo:** diferenciar estructuras estáticas y dinámicas a partir de su tamaño, asignación y flexibilidad.

**Concepto trabajado:** estructuras estáticas y dinámicas.

## Comparación

| Aspecto | Estática | Dinámica |
|---|---|---|
| Tamaño | Fijo | Puede crecer o reducirse |
| Momento de asignación | Definido previamente | Se ajusta durante la ejecución |
| Gestión | Más simple | Más flexible y con mayor complejidad de administración |
| Ejemplo trabajado | Arreglo | Lista enlazada, pila, cola, árbol |

## Ejemplo conceptual

Un curso tiene exactamente 5 evaluaciones y se desea almacenar una nota por evaluación. Un arreglo de cinco posiciones es suficiente porque la cantidad de elementos ya está definida.

En cambio, si se necesita registrar una cantidad que cambia durante la ejecución, una estructura dinámica resulta más adecuada porque puede ajustarse al volumen de datos.

### Actividad

Para cada situación, decide si conviene pensar en una estructura estática o dinámica.

1. Registrar las 7 notas de una semana de práctica.
   - Elección: __________________
   - Motivo: ______________________________________________________

2. Registrar elementos cuya cantidad puede aumentar o disminuir durante la ejecución.
   - Elección: __________________
   - Motivo: ______________________________________________________

3. Guardar una jerarquía de datos.
   - Estructura mencionada en la sesión: __________________________

4. Procesar elementos siguiendo “último en entrar, primero en salir”.
   - Estructura mencionada en la sesión: __________________________

5. Procesar elementos siguiendo “primero en entrar, primero en salir”.
   - Estructura mencionada en la sesión: __________________________

**Error frecuente:** elegir una estructura solo por costumbre sin observar cómo cambia el volumen de datos o cómo deben organizarse los elementos.

---

# BLOQUE 6. IMPLEMENTACIÓN BÁSICA EN JAVA

**Objetivo:** conectar datos, algoritmo y una estructura estática mediante un ejemplo pequeño en Java.

**Concepto trabajado:** datos, arreglo, proceso y salida.

Este ejemplo utiliza un arreglo porque es una estructura estática explícitamente trabajada en la sesión. No es necesario estudiar todavía estructuras dinámicas en código.

## Paso 1. Crear el archivo

**Dónde hacerlo:** editor de texto o entorno Java disponible en el laboratorio.

Crea un archivo llamado:

```text
DatosYAlgoritmos.java
```

## Paso 2. Escribir el programa

```java
public class DatosYAlgoritmos {
    public static void main(String[] args) {
        int[] notas = {14, 16, 18, 15, 17};

        int suma = notas[0] + notas[1] + notas[2] + notas[3] + notas[4];
        double promedio = suma / 5.0;
        boolean aprobado = promedio >= 13.0;

        System.out.println("Suma: " + suma);
        System.out.println("Promedio: " + promedio);
        System.out.println("Aprobado: " + aprobado);
    }
}
```

## Paso 3. Relacionar el código con los conceptos

- `int[] notas`: estructura estática de tamaño definido.
- `int suma`: dato entero.
- `double promedio`: dato real.
- `boolean aprobado`: dato lógico.
- La suma y el cálculo del promedio forman parte del **proceso**.
- Los mensajes de consola representan la **salida**.

## Paso 4. Compilar

**Dónde hacerlo:** terminal ubicada en la carpeta donde guardaste el archivo.

```text
javac DatosYAlgoritmos.java
```

**Qué debería ocurrir:** si no existen errores de sintaxis, se generará el archivo compilado correspondiente.

## Paso 5. Ejecutar

```text
java DatosYAlgoritmos
```

**Resultado esperado:**

```text
Suma: 80
Promedio: 16.0
Aprobado: true
```

## Paso 6. Modificación controlada

Cambia los valores del arreglo por:

```java
int[] notas = {8, 10, 12, 11, 9};
```

Antes de ejecutar, responde:

- ¿Qué tipo de dato contiene el arreglo? __________________________
- ¿El tamaño del arreglo cambia? _________________________________
- ¿Esperas que `aprobado` sea verdadero o falso? _________________

Compila y ejecuta nuevamente.

**Cómo comprobarlo:** compara tu predicción con la salida real.

**Si aparece un problema:**

- Si `javac` no se reconoce, revisa la preparación del entorno.
- Si el nombre de la clase y el archivo no coinciden, corrígelo.
- Si falta un punto y coma, Java mostrará un error de compilación.
- Si editaste el archivo después de compilar, vuelve a ejecutar `javac` antes de `java`.

---

# BLOQUE 7. RELACIÓN ENTRE ALGORITMOS Y ESTRUCTURAS DE DATOS

**Objetivo:** explicar por qué una solución depende tanto de los pasos del algoritmo como de la forma de organizar los datos.

**Concepto trabajado:** relación entre algoritmo, estructura de datos y uso eficiente de recursos.

**Explicación breve:** el algoritmo describe el trabajo que debe realizarse; la estructura de datos organiza la información que el algoritmo necesita procesar. La selección adecuada ayuda a que el programa sea más claro y eficiente.

## Ejemplo de la pila de platos

Una pila de platos ilustra el comportamiento “último en entrar, primero en salir”. Si el acceso natural es desde la parte superior, intentar retirar primero un elemento ubicado debajo obliga a manipular otros elementos antes.

La idea importante es observar que la forma en la que los datos están organizados influye en cómo conviene procesarlos.

## Actividad integradora

### Caso: registro de cinco tiempos de ejecución

Necesitas registrar exactamente cinco valores numéricos y calcular su promedio.

Completa:

1. **Datos de entrada:** __________________________________________
2. **Tipo de dato principal:** ____________________________________
3. **Estructura propuesta:** ______________________________________
4. **¿Estática o dinámica?:** _____________________________________
5. **Proceso principal:** __________________________________________
6. **Salida esperada:** ___________________________________________
7. **¿Por qué la estructura elegida es adecuada?:**

   ________________________________________________________________

   ________________________________________________________________

### Evidencia requerida

Prepara un único archivo o documento con:

- Tu algoritmo para calcular el promedio de tres notas.
- La tabla de entrada, proceso y salida del área de un rectángulo.
- La clasificación de tipos de datos.
- La actividad de clasificación de estructuras.
- La comparación estática/dinámica.
- Una captura o copia de la salida del programa `DatosYAlgoritmos`.
- Las respuestas de la actividad integradora.

Nombre sugerido:

```text
S01_Datos_Algoritmos_ApellidoNombre
```

No se define aquí una plataforma ni una fecha de entrega; utiliza las indicaciones vigentes de tu aula virtual.

---

# ERRORES FRECUENTES Y SOLUCIONES

| Problema observado | Causa probable | Qué revisar | Solución recomendada |
|---|---|---|---|
| El algoritmo no termina | No se definió una condición o final claro | Último paso | Reescribir la secuencia con un cierre explícito |
| Entrada y salida están confundidas | No se identificó qué dato se recibe y cuál se produce | Tabla entrada-proceso-salida | Separar información inicial del resultado |
| Tipo de dato incorrecto | Se eligió el tipo por apariencia y no por significado | Naturaleza del valor | Revisar si es entero, real, lógico, carácter o cadena |
| Se confunden criterios de clasificación | Se mezclan disposición, tamaño y almacenamiento | Criterio preguntado | Clasificar cada criterio por separado |
| `java` o `javac` no se reconoce | JDK ausente o configuración incompleta | `java -version` y `javac -version` | Consultar al docente antes de cambiar el equipo institucional |
| El programa no compila | Error de sintaxis o nombre de archivo/clase | Mensaje de compilación | Corregir el primer error reportado y volver a compilar |

---

# PREGUNTAS DE COMPROBACIÓN

1. ¿Qué diferencia existe entre un algoritmo y un programa?

   ________________________________________________________________

2. ¿Por qué un algoritmo debe ser finito?

   ________________________________________________________________

3. ¿Qué tres partes fundamentales se identifican en un algoritmo?

   ________________________________________________________________

4. ¿Qué tipo de dato utilizarías para representar una condición de verdadero o falso?

   ________________________________________________________________

5. ¿Cuál es la diferencia principal entre una estructura estática y una dinámica?

   ________________________________________________________________

6. Menciona una estructura lineal y una no lineal trabajadas en la sesión.

   ________________________________________________________________

7. ¿Por qué la elección de la estructura de datos puede afectar la eficiencia de una solución?

   ________________________________________________________________

---

# VERIFICACIÓN FINAL

- [ ] Comprendo qué es un algoritmo.
- [ ] Puedo identificar precisión, definición y finitud.
- [ ] Distingo entrada, proceso y salida.
- [ ] Identifico los tipos de datos trabajados.
- [ ] Puedo clasificar estructuras por disposición, tamaño y almacenamiento.
- [ ] Diferencio estructuras estáticas y dinámicas.
- [ ] Ejecuté o analicé el ejemplo Java con un arreglo.
- [ ] Revisé los errores frecuentes.
- [ ] Completé la actividad integradora.
- [ ] Preparé mi evidencia de entrega.

---

# CIERRE Y RECURSOS DE REFUERZO

En esta sesión aprendiste a observar un problema desde tres elementos esenciales: **los datos**, **los pasos que los transforman** y **la estructura utilizada para organizarlos**. También comprobaste que un algoritmo debe ser preciso, definido y finito, y que las estructuras de datos pueden clasificarse desde diferentes criterios.

Repasa especialmente:

- Entrada, proceso y salida.
- Tipos de datos.
- Diferencias entre estructura estática y dinámica.
- Clasificación lineal/no lineal y volátil/permanente.
- Relación entre algoritmo, estructura de datos y eficiencia.

En la siguiente etapa del curso podrás utilizar estas bases para comprender y aplicar estructuras y algoritmos con mayor profundidad.

**Blog:** https://lideratecacademy.com/  
**Canal YouTube:** https://www.youtube.com/@LideratecAcademy
