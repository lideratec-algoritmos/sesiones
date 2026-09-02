# GUÍA DEL ESTUDIANTE

## Algoritmo y Estructura de Datos basado en IA

**Sesión 01: Datos y algoritmos**  
**Modalidad:** clase guiada y participativa  
**Práctica:** comprender algoritmos, datos y estructuras mediante ejemplos, participación y ejecución  
**Elaborado por el docente**  
**Proyecto académico:** Lideratec Academy

---

# 1. Propósito de la sesión

Esta guía está diseñada para que aprendas **participando en clase**, no solo leyendo definiciones. En cada bloque seguirás esta secuencia:

> **Observo → comprendo → respondo → practico → verifico → explico con mis propias palabras.**

Durante la sesión resolverás ejemplos con el docente, realizarás predicciones, compararás respuestas, corregirás errores y comprobarás resultados antes de continuar.

---

# 2. Resultado de aprendizaje observable

Al finalizar podrás:

- Explicar qué es un algoritmo con tus propias palabras.
- Reconocer si una secuencia es precisa, definida y finita.
- Identificar entrada, proceso y salida.
- Diferenciar datos enteros, reales, lógicos, caracteres y cadenas.
- Clasificar estructuras según disposición, variación de tamaño y almacenamiento.
- Diferenciar estructuras estáticas y dinámicas.
- Reconocer los ejemplos de pila, cola, árbol, lista y arreglo trabajados en clase.
- Analizar un arreglo básico en Java.
- Predecir resultados antes de ejecutar el código.
- Explicar la relación entre algoritmo y estructura de datos.

---

# 3. Cómo participaremos

Cuando aparezca una sección como **Piensa**, **Predice**, **Participa**, **Comprueba** o **Mini reto**:

1. Lee el caso.
2. Piensa antes de mirar la solución.
3. Escribe tu respuesta tentativa.
4. Comparte o contrasta tu respuesta en clase.
5. Corrige si es necesario.
6. Explica por qué tu respuesta es correcta.

---

# 4. Preparación mínima

Para la parte conceptual necesitas:

- cuaderno o archivo de notas;
- editor de texto;
- calculadora simple si el docente la autoriza.

Para el ejemplo Java:

- JDK disponible en el laboratorio;
- terminal, PowerShell, símbolo del sistema o entorno autorizado.

## Comprobación

Ejecuta:

```text
java -version
```

Luego:

```text
javac -version
```

Si ambos comandos muestran una versión, puedes continuar. Si alguno no se reconoce, consulta al docente antes de modificar el equipo.

---

# BLOQUE 1. ¿QUÉ ES UN ALGORITMO?

## Objetivo

Comprender que un algoritmo no es simplemente “hacer algo”, sino describir una solución mediante pasos ordenados.

## 1.1 Activación

Imagina que alguien te dice:

> “Prepara una limonada.”

### Piensa

¿La instrucción anterior es suficientemente clara para que cualquier persona obtenga exactamente el mismo resultado?

Respuesta:

____________________________________________________________________

¿Por qué?

____________________________________________________________________

## 1.2 Convertimos una intención en pasos

```text
1. Tomar un vaso.
2. Agregar agua.
3. Agregar jugo de limón.
4. Agregar azúcar.
5. Mezclar.
6. Servir.
7. Finalizar.
```

Ahora existe una secuencia, pero todavía podemos preguntar cuánto de cada ingrediente usar. Esto permite entender que un algoritmo debe reducir la ambigüedad.

## 1.3 Definición

Un algoritmo es una **secuencia ordenada de pasos, sin ambigüedades, que conduce a la solución de un problema**.

### Participa

Completa:

> Un algoritmo sirve para ___________________________________________

> y necesita que los pasos _________________________________________

---

# 1.4 Características fundamentales

## Preciso

Debe indicar qué hacer y en qué orden.

### Ejemplo

```text
1. Leer precio.
2. Leer cantidad.
3. Multiplicar precio por cantidad.
4. Mostrar resultado.
```

### Contraejemplo

```text
1. Hacer el cálculo.
2. Ver los datos.
3. Mostrar algo.
```

### Participa

¿Cuál de los dos ejemplos es más preciso?

____________________________________________________________________

¿Qué palabras vuelven ambiguo al segundo?

____________________________________________________________________

---

## Definido

Con los mismos datos debe producir el mismo resultado.

### Ejemplo

```text
precio = 20
cantidad = 3
```

Proceso:

```text
20 × 3
```

Resultado:

```text
60
```

### Predice

Si ejecutamos el mismo algoritmo diez veces con los mismos datos, ¿el resultado debería cambiar?

- [ ] Sí
- [ ] No

Justificación:

____________________________________________________________________

---

## Finito

Debe terminar después de una cantidad finita de pasos.

### Ejemplo correcto

```text
1. Leer un número.
2. Multiplicarlo por 2.
3. Mostrar resultado.
4. Finalizar.
```

### Ejemplo problemático

```text
1. Mostrar "Procesando".
2. Volver al paso 1.
```

### Participa

¿Qué característica no cumple el segundo ejemplo?

____________________________________________________________________

---

# 1.5 Ejemplo completo

## Problema

Calcular el total de una compra de 3 unidades con precio unitario de 20.

### Paso 1. Datos

```text
precio = 20
cantidad = 3
```

### Paso 2. Operación

```text
total = precio × cantidad
```

### Paso 3. Sustituir

```text
total = 20 × 3
```

### Paso 4. Resultado

```text
total = 60
```

### Paso 5. Algoritmo

```text
1. Recibir precio.
2. Recibir cantidad.
3. Multiplicar precio por cantidad.
4. Guardar el resultado como total.
5. Mostrar total.
6. Finalizar.
```

## Punto de control

- [ ] Comprendo qué significa preciso.
- [ ] Comprendo qué significa definido.
- [ ] Comprendo qué significa finito.

### Explícalo con tus palabras

¿Qué diferencia existe entre decir “calcular el total” y escribir un algoritmo completo?

____________________________________________________________________

____________________________________________________________________

---

# BLOQUE 2. ENTRADA, PROCESO Y SALIDA

## Objetivo

Separar correctamente la información inicial, las operaciones y el resultado.

## 2.1 Idea visual

```text
ENTRADA → PROCESO → SALIDA
```

## 2.2 Ejemplo resuelto

Problema: calcular el área de un rectángulo.

```text
base = 8
altura = 5
```

Entrada:

```text
base, altura
```

Proceso:

```text
area = base × altura
```

Salida:

```text
40
```

---

# 2.3 Participación guiada

## Caso

Calcular el promedio de:

```text
14, 16, 18
```

Antes de ver la solución completa:

### Entrada

____________________________________________________________________

### Proceso

____________________________________________________________________

### Salida

____________________________________________________________________

## Solución

```text
14 + 16 + 18 = 48
48 / 3 = 16
```

Salida:

```text
Promedio = 16
```

---

# 2.4 Actividad en clase

Una persona compra 4 cuadernos a 7.50 cada uno.

| Parte | Tu respuesta |
|---|---|
| Entrada | |
| Proceso | |
| Salida | |

### Predice

Resultado esperado:

____________________________________________________________________

### Comprueba

```text
____________________________________
```

## Mini reto

Crea un problema con dos entradas, un proceso y una salida.

Problema:

____________________________________________________________________

Entrada:

____________________________________________________________________

Proceso:

____________________________________________________________________

Salida:

____________________________________________________________________

---

# BLOQUE 3. DATOS Y TIPOS DE DATOS

## Objetivo

Reconocer qué clase de valor representa cada dato.

## 3.1 Ejemplos

```text
19
29.90
true
'A'
"Ana Torres"
```

| Tipo | Ejemplo | Significado |
|---|---|---|
| Entero | `25` | Número sin parte decimal |
| Real | `19.90` | Número con parte decimal |
| Lógico | `true` / `false` | Verdadero o falso |
| Carácter | `'A'` | Un solo carácter |
| Cadena | `"Lideratec"` | Secuencia de caracteres |

---

# 3.2 Ejemplo guiado

```text
edad = 19
promedio = 16.5
matriculado = true
seccion = 'A'
nombre = "Lucía Torres"
```

Clasificación:

- `edad` → entero.
- `promedio` → real.
- `matriculado` → lógico.
- `seccion` → carácter.
- `nombre` → cadena.

---

# 3.3 Participa

| Dato | Valor | Tipo |
|---|---:|---|
| Cantidad de productos | 5 | |
| Precio | 49.90 | |
| Disponible | false | |
| Categoría | 'B' | |
| Producto | "Mouse" | |

### Pregunta

¿Qué diferencia existe entre `'A'` y `"Ana"`?

____________________________________________________________________

____________________________________________________________________

## Mini reto

Propón un ejemplo para cada tipo.

Entero: ___________________________________________________________

Real: _____________________________________________________________

Lógico: ___________________________________________________________

Carácter: _________________________________________________________

Cadena: ___________________________________________________________

---

# BLOQUE 4. CLASIFICACIÓN DE ESTRUCTURAS DE DATOS

## Objetivo

Comprender que una estructura puede clasificarse mediante diferentes criterios.

## 4.1 Según disposición

### Lineales

Los elementos se organizan secuencialmente.

```text
[10] → [20] → [30] → [40]
```

Ejemplos trabajados: arreglos, listas y diccionarios.

### No lineales

No forman necesariamente una única secuencia.

```text
        A
       / \
      B   C
```

Ejemplos: árboles y grafos.

### Participa

```text
[5] → [8] → [12] → [20]
```

¿Lineal o no lineal?

____________________________________________________________________

---

# 4.2 Según variación de tamaño

### Estáticas

Tamaño fijo durante la ejecución.

```text
[14] [16] [18] [15] [17]
```

### Dinámicas

Pueden crecer o reducirse durante la ejecución.

```text
Inicio:
[10] → [20]

Después:
[10] → [20] → [30]
```

---

# 4.3 Según almacenamiento

### Volátiles

Existen en memoria durante la ejecución.

### Permanentes

Perduran en dispositivos auxiliares, como ficheros.

---

# 4.4 Ejemplo con tres criterios

Caso: arreglo de cinco notas en memoria.

| Criterio | Clasificación |
|---|---|
| Disposición | Lineal |
| Variación de tamaño | Estática |
| Almacenamiento | Volátil |

### Participa

¿Por qué una misma estructura puede ser lineal, estática y volátil?

____________________________________________________________________

____________________________________________________________________

---

# 4.5 Actividad guiada

| Caso | Criterio | Clasificación |
|---|---|---|
| Árbol de jerarquía | Disposición | |
| Lista que crece | Tamaño | |
| Arreglo de 10 posiciones | Tamaño | |
| Fichero almacenado | Almacenamiento | |
| Arreglo usado mientras corre el programa | Almacenamiento | |

---

# BLOQUE 5. ESTRUCTURAS ESTÁTICAS Y DINÁMICAS

## Objetivo

Decidir cuándo un problema trabaja con una cantidad fija o variable de elementos.

## Caso A

Exactamente cinco evaluaciones:

```text
[ ] [ ] [ ] [ ] [ ]
```

Elección:

```text
Estática
```

## Caso B

Cantidad de solicitudes que puede crecer:

```text
Dinámica
```

| Aspecto | Estática | Dinámica |
|---|---|---|
| Tamaño | Fijo | Puede cambiar |
| Flexibilidad | Menor | Mayor |
| Gestión | Más simple | Más compleja |
| Ejemplo | Arreglo | Lista enlazada |

---

# 5.1 Participa

## Caso 1

Guardar exactamente siete notas.

- [ ] Estática
- [ ] Dinámica

¿Por qué?

____________________________________________________________________

## Caso 2

Registrar elementos cuya cantidad puede crecer durante la ejecución.

- [ ] Estática
- [ ] Dinámica

¿Por qué?

____________________________________________________________________

---

# 5.2 Pila y cola

## Pila

```text
LIFO
Last In, First Out
Último en entrar, primero en salir
```

Ejemplo:

```text
Plato 3  ← sale primero
Plato 2
Plato 1
```

## Cola

```text
FIFO
First In, First Out
Primero en entrar, primero en salir
```

Ejemplo:

```text
Persona 1 → Persona 2 → Persona 3
```

### Participa

En una pila de platos, ¿cuál retiras primero?

____________________________________________________________________

En una cola de estudiantes, ¿quién debería ser atendido primero?

____________________________________________________________________

---

# BLOQUE 6. DEL ALGORITMO AL CÓDIGO EN JAVA

## Objetivo

Comprobar que el código implementa un razonamiento previo.

## 6.1 Problema

Cinco notas:

```text
14, 16, 18, 15, 17
```

Queremos:

1. sumar;
2. calcular promedio;
3. determinar si el promedio es mayor o igual que 13.

## 6.2 Diseñamos el algoritmo primero

```text
1. Guardar las cinco notas.
2. Sumar las cinco notas.
3. Dividir la suma entre 5.
4. Comparar el promedio con 13.
5. Mostrar suma.
6. Mostrar promedio.
7. Mostrar aprobado.
8. Finalizar.
```

## 6.3 Predice

Suma:

____________________________________________________________________

Promedio:

____________________________________________________________________

`aprobado`:

____________________________________________________________________

---

# 6.4 Crear el archivo

```text
DatosYAlgoritmos.java
```

## 6.5 Código

```java
public class DatosYAlgoritmos {

    public static void main(String[] args) {

        int[] notas = {14, 16, 18, 15, 17};

        int suma =
                notas[0] +
                notas[1] +
                notas[2] +
                notas[3] +
                notas[4];

        double promedio = suma / 5.0;

        boolean aprobado = promedio >= 13.0;

        System.out.println("Suma: " + suma);
        System.out.println("Promedio: " + promedio);
        System.out.println("Aprobado: " + aprobado);
    }
}
```

---

# 6.6 Leemos el código juntos

```java
int[] notas = {14, 16, 18, 15, 17};
```

Representa un arreglo de cinco enteros.

Índices:

```text
0  1  2  3  4
```

Valores:

```text
14 16 18 15 17
```

### Participa

`notas[0]` vale:

____________________________________________________________________

`notas[3]` vale:

____________________________________________________________________

¿Por qué el último índice es 4 y no 5?

____________________________________________________________________

---

# 6.7 Compilar

```text
javac DatosYAlgoritmos.java
```

## 6.8 Ejecutar

```text
java DatosYAlgoritmos
```

Resultado esperado:

```text
Suma: 80
Promedio: 16.0
Aprobado: true
```

---

# 6.9 Comparar

| Elemento | Predicción | Ejecución | ¿Coincidió? |
|---|---|---|---|
| Suma | | | |
| Promedio | | | |
| Aprobado | | | |

---

# 6.10 Modificación controlada

Cambia:

```java
int[] notas = {8, 10, 12, 11, 9};
```

Antes de ejecutar:

Suma esperada:

____________________________________________________________________

Promedio esperado:

____________________________________________________________________

`aprobado` esperado:

____________________________________________________________________

Ejecuta:

```text
javac DatosYAlgoritmos.java
java DatosYAlgoritmos
```

Compara con tu predicción.

---

# 6.11 Mini reto

Usa:

```java
int[] notas = {13, 13, 13, 13, 13};
```

Antes de ejecutar responde:

1. Suma: ___________________________________________________________
2. Promedio: _______________________________________________________
3. ¿`aprobado` será `true` o `false`? ______________________________
4. ¿El tamaño del arreglo cambió? __________________________________
5. ¿Por qué sigue siendo estático? __________________________________

---

# BLOQUE 7. RELACIÓN ENTRE ALGORITMO Y ESTRUCTURA

## Objetivo

Comprender que la forma de organizar los datos influye en cómo conviene procesarlos.

## Ejemplo: pila de platos

```text
Plato 4
Plato 3
Plato 2
Plato 1
```

Si quieres retirar un plato, el acceso natural comienza desde arriba.

### Participa

¿Qué ocurriría si intentaras retirar primero el plato inferior?

____________________________________________________________________

¿Qué nos enseña esto sobre la estructura?

____________________________________________________________________

---

# BLOQUE 8. CASO INTEGRADOR

Necesitas registrar exactamente cinco tiempos:

```text
4.5
5.2
3.8
4.9
5.0
```

y calcular su promedio.

## Paso 1. Tipo de dato

____________________________________________________________________

## Paso 2. Estructura

____________________________________________________________________

## Paso 3. Clasificación por disposición

____________________________________________________________________

## Paso 4. Clasificación por tamaño

____________________________________________________________________

## Paso 5. Entrada

____________________________________________________________________

## Paso 6. Proceso

____________________________________________________________________

## Paso 7. Salida

____________________________________________________________________

## Justificación

> Elegí esta estructura porque ______________________________________

____________________________________________________________________

---

# BLOQUE 9. RETO FINAL DE CLASE

Diseña un problema pequeño que utilice:

- al menos tres datos;
- entrada;
- proceso;
- salida;
- un tipo de dato identificado;
- una estructura trabajada;
- un algoritmo de al menos cinco pasos.

## Problema

____________________________________________________________________

## Datos

____________________________________________________________________

## Tipos

____________________________________________________________________

## Estructura

____________________________________________________________________

## Algoritmo

1. _________________________________________________________________
2. _________________________________________________________________
3. _________________________________________________________________
4. _________________________________________________________________
5. _________________________________________________________________
6. _________________________________________________________________

## Entrada

____________________________________________________________________

## Proceso

____________________________________________________________________

## Salida

____________________________________________________________________

## ¿Por qué elegiste esa estructura?

____________________________________________________________________

____________________________________________________________________

---

# PREGUNTAS DE COMPROBACIÓN FINAL

1. ¿Qué hace que una secuencia sea un algoritmo?

   _________________________________________________________________

2. ¿Qué significa que sea definido?

   _________________________________________________________________

3. ¿Qué diferencia existe entre entrada y salida?

   _________________________________________________________________

4. ¿Qué tipo representa verdadero o falso?

   _________________________________________________________________

5. ¿Qué diferencia existe entre carácter y cadena?

   _________________________________________________________________

6. ¿Qué diferencia principal existe entre estática y dinámica?

   _________________________________________________________________

7. ¿Por qué un arreglo de cinco posiciones puede ser estático?

   _________________________________________________________________

8. ¿Qué significa LIFO?

   _________________________________________________________________

9. ¿Qué significa FIFO?

   _________________________________________________________________

10. Explica la relación entre algoritmo y estructura de datos.

   _________________________________________________________________

   _________________________________________________________________

---

# ERRORES FRECUENTES

| Problema | Qué revisar |
|---|---|
| El algoritmo es demasiado general | Convertir acciones vagas en pasos concretos |
| Se confunden entrada y salida | Identificar qué existe antes y qué aparece después |
| Se confunden entero y real | Revisar si el valor puede tener decimales |
| Se confunden carácter y cadena | Un carácter frente a una secuencia |
| Se mezclan lineal, estática y volátil | Aplicar cada criterio por separado |
| `javac` no se reconoce | Consultar al docente antes de modificar el equipo |
| Se muestra un resultado anterior | Volver a compilar antes de ejecutar |
| Se usa un índice inexistente | Cinco elementos usan índices 0 a 4 |

---

# EVIDENCIA DE APRENDIZAJE

Prepara:

1. algoritmo del promedio de tres notas;
2. ejercicio de entrada-proceso-salida;
3. tabla de tipos de datos;
4. clasificación de estructuras;
5. respuestas de estáticas y dinámicas;
6. predicción y ejecución del programa Java;
7. mini reto de cinco notas iguales;
8. caso integrador;
9. reto final diseñado por ti;
10. respuestas de comprobación.

Nombre sugerido:

```text
S01_Datos_Algoritmos_ApellidoNombre
```

---

# CHECKLIST FINAL

- [ ] Puedo explicar qué es un algoritmo.
- [ ] Identifico precisión, definición y finitud.
- [ ] Distingo entrada, proceso y salida.
- [ ] Reconozco los tipos de datos trabajados.
- [ ] Distingo los tres criterios de clasificación.
- [ ] Diferencio estáticas y dinámicas.
- [ ] Puedo explicar LIFO y FIFO con ejemplos.
- [ ] Comprendo qué representa un arreglo.
- [ ] Puedo leer el ejemplo Java.
- [ ] Predije resultados antes de ejecutar.
- [ ] Comparé predicción y resultado.
- [ ] Puedo explicar la relación entre algoritmo y estructura.
- [ ] Participé resolviendo ejemplos durante la clase.

---

# CIERRE

La idea central de esta sesión es:

```text
PROBLEMA
   ↓
DATOS
   ↓
ALGORITMO
   ↓
ESTRUCTURA
   ↓
IMPLEMENTACIÓN
   ↓
RESULTADO
```

Antes de programar debes comprender el problema, identificar los datos, ordenar los pasos y decidir cómo organizar la información.

**Blog:** https://lideratecacademy.com/  
**Canal YouTube:** https://www.youtube.com/@LideratecAcademy
