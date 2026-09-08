# Guía del Estudiante — Arreglos unidimensionales (vectores)

**Curso:** Algoritmo y Estructura de Datos basado en IA  
**Tema 02:** Arreglos unidimensionales (vectores)  
**Práctica:** Comprender, representar y operar vectores  
**Duración sugerida:** 180 minutos  
**Resultado observable:** Al finalizar, podrás representar un vector, identificar sus componentes, acceder y modificar posiciones, recorrerlo, buscar datos y comprender cómo se agregan, eliminan y ordenan elementos.

## 1. ¿Qué aprenderás?

En esta sesión trabajarás con arreglos unidimensionales o vectores: conjuntos finitos, ordenados y homogéneos. Aprenderás a reconocer nombre, tipo, tamaño, índices y elementos; a distinguir índices que comienzan en 0 frente a representaciones que comienzan en 1; y a realizar las operaciones de acceso, incorporación, eliminación, búsqueda y ordenamiento descritas en la sesión.

## 2. Mapa de la sesión

```text
VECTOR
  |
  +--> definición y representación
  |
  +--> nombre + tipo + tamaño + índice + elementos
  |
  +--> almacenamiento
  |      +--> secuencial
  |      +--> por tipo
  |      +--> tamaño
  |      +--> forma de llenado
  |
  +--> operaciones
         +--> acceder
         +--> agregar
         +--> eliminar
         +--> buscar
         +--> ordenar
```

## 3. Herramientas recomendadas para esta práctica

Para los ejemplos Java de la sesión se recomienda **JDK 25 LTS**, porque es la versión LTS vigente verificada para una práctica académica estable. La versión más reciente de Java SE es JDK 26, pero no es LTS. Para editar y ejecutar los ejemplos puede utilizarse **IntelliJ IDEA** en su distribución unificada actual; su funcionalidad esencial para Java puede seguir utilizándose gratuitamente.

Recursos oficiales:
- Oracle Java Downloads: https://www.oracle.com/java/technologies/downloads/
- IntelliJ IDEA: https://www.jetbrains.com/idea/

> Si el laboratorio institucional ya tiene un JDK compatible instalado, consulta al docente antes de modificarlo. Los ejemplos de esta sesión usan características básicas de arrays y no necesitan APIs avanzadas.

## 4. Preparación del entorno

### Paso 1 — Comprueba Java

Abre una terminal.

```bash
java -version
javac -version
```

**Qué buscamos:** que ambos comandos respondan con una versión instalada.

**Por qué son dos comandos:** `java` ejecuta programas compilados y `javac` compila código fuente Java.

**Si aparece “no se reconoce el comando”:** el JDK no está instalado correctamente o su ruta no está disponible en el entorno. No continúes adivinando rutas; valida la instalación con el docente.

### Paso 2 — Crea un proyecto simple

En IntelliJ IDEA crea un proyecto Java sin frameworks adicionales. Nómbralo:

```text
vectores-s02
```

Dentro de `src`, crea la clase:

```text
VectoresDemo.java
```

### Paso 3 — Validación inicial

Escribe:

```java
public class VectoresDemo {
    public static void main(String[] args) {
        System.out.println("Entorno listo para trabajar con vectores");
    }
}
```

Ejecuta la clase. Debes ver:

```text
Entorno listo para trabajar con vectores
```

Si esto funciona, el entorno está preparado.

---

# 5. Conceptos necesarios antes de programar

## 5.1 ¿Qué es un vector?

Un vector es un arreglo unidimensional: un conjunto finito y ordenado de elementos homogéneos. “Ordenado” significa que cada elemento tiene una posición identificable. “Homogéneo” significa que los elementos pertenecen al mismo tipo de dato.

Ejemplo conceptual:

```text
NOTAS
índice:    0    1    2    3
         +----+----+----+----+
valor:   | 15 | 18 | 12 | 20 |
         +----+----+----+----+
```

En Java, el primer índice es `0`. Por eso un arreglo de tamaño 4 tiene índices `0`, `1`, `2` y `3`.

## 5.2 Estructura de un arreglo

| Componente | Ejemplo | Significado |
|---|---|---|
| Nombre | `notas` | Identificador del arreglo |
| Tipo | `int` | Tipo de dato almacenado |
| Tamaño | `4` | Cantidad de posiciones |
| Índice | `0..3` | Posición de cada elemento |
| Elemento | `15` | Valor almacenado |

---

# 6. Ejemplo 1 — Crear y representar un vector

## 1. ¿Qué aprenderemos?

Crear un arreglo de enteros, visualizar su tamaño y relacionar cada valor con su índice.

## 2. Problema

Necesitamos almacenar cuatro notas sin crear cuatro variables independientes.

## 3. Concepto

Un único nombre puede agrupar varios datos homogéneos.

## 4. Antes de comenzar

Usa `VectoresDemo.java`.

## 5. Estado inicial

Todavía no existe el arreglo.

## 6. Paso 1 — Declarar e inicializar

```java
int[] notas = {15, 18, 12, 20};
```

**Qué hacemos:** creamos un arreglo llamado `notas`.

**Por qué:** necesitamos almacenar varios enteros bajo una sola estructura.

**Qué significa cada parte:**
- `int`: tipo de dato.
- `[]`: indica que trabajamos con un arreglo.
- `notas`: nombre.
- `{15, 18, 12, 20}`: elementos iniciales.

**Resultado:** `notas.length` vale `4`.

## 7. Paso 2 — Mostrar tamaño

```java
System.out.println("Tamaño: " + notas.length);
```

Resultado esperado:

```text
Tamaño: 4
```

## 8. Flujo completo

```text
{15,18,12,20}
      |
      v
 arreglo notas
      |
      v
 índices 0..3
```

## 9. Validación

Comprueba que existen cuatro valores y cuatro posiciones válidas.

## 10. Modificación

Cambia el último valor `20` por `17`.

## 11. Antes de ejecutar, predice

¿Cambiará el tamaño del vector? **No.** Cambia un elemento, no la cantidad de posiciones.

## 12. Resultado

El vector sigue teniendo tamaño 4.

## 13. Error frecuente

Confundir “tamaño 4” con “último índice 4”.

## 14. ¿Por qué ocurre?

Porque en Java los índices comienzan en 0.

## 15. Corrección

Para tamaño 4, el último índice válido es `3`.

## 16. Preguntas

1. ¿Por qué `notas` puede guardar los cuatro valores?
2. ¿Cuál es el índice del valor 12?
3. ¿Cuál es el último índice válido?

## 17. Idea clave

**Tamaño e índice no son lo mismo.**

## 18. Conexión

Ahora accederemos a una posición concreta.

---

# 7. Ejemplo 2 — Acceder y modificar un elemento

## 1. ¿Qué aprenderemos?

Leer y modificar una posición mediante su índice.

## 2. Situación

Queremos consultar la segunda nota y luego corregirla.

## 3. Concepto

El índice identifica de forma precisa un elemento.

## 4. Estado inicial

```java
int[] notas = {15, 18, 12, 20};
```

## 5. Paso 1 — Acceder

```java
System.out.println(notas[1]);
```

**Por qué usamos `1`:** Java inicia sus índices en 0; la segunda posición tiene índice 1.

Resultado:

```text
18
```

## 6. Paso 2 — Modificar

```java
notas[1] = 19;
System.out.println(notas[1]);
```

Resultado:

```text
19
```

**Causa → efecto:**

```text
asignamos 19 a notas[1]
          |
          v
se reemplaza el valor anterior
          |
          v
el tamaño continúa siendo 4
```

## 7. Modificación con estudiantes

Cambia `notas[2]` a `14`.

## 8. Antes de ejecutar, predice

¿Qué elemento cambiará? Solo la tercera posición.

## 9. Error controlado

Analiza, sin dejarlo como versión final:

```java
System.out.println(notas[4]);
```

Para un arreglo de cuatro elementos, el índice 4 está fuera del rango válido.

## 10. Corrección

Usa un índice entre `0` y `notas.length - 1`.

## 11. Preguntas

1. ¿Qué diferencia existe entre `notas[1]` y `notas.length`?
2. ¿Modificar un elemento cambia el tamaño?
3. ¿Cómo calculas el último índice?

## 12. Idea clave

El acceso es directo mediante el índice, pero el índice debe ser válido.

---

# 8. Ejemplo 3 — Recorrer y llenar un vector

## 1. ¿Qué aprenderemos?

Procesar todas las posiciones sin escribir una instrucción por cada elemento.

## 2. Situación

Queremos mostrar todas las notas.

## 3. Concepto

Un recorrido visita cada índice válido.

## 4. Paso 1 — Recorrido

```java
int[] notas = {15, 18, 12, 20};

for (int i = 0; i < notas.length; i++) {
    System.out.println("Índice " + i + ": " + notas[i]);
}
```

### ¿Qué significa la condición?

| Parte | Función |
|---|---|
| `int i = 0` | empieza en el primer índice |
| `i < notas.length` | evita salir del rango |
| `i++` | avanza una posición |
| `notas[i]` | accede al elemento actual |

Resultado:

```text
Índice 0: 15
Índice 1: 18
Índice 2: 12
Índice 3: 20
```

## 5. Trazado

| Iteración | `i` | `notas[i]` |
|---:|---:|---:|
| 1 | 0 | 15 |
| 2 | 1 | 18 |
| 3 | 2 | 12 |
| 4 | 3 | 20 |

## 6. Modificación

Crea:

```java
int[] edades = new int[5];

for (int i = 0; i < edades.length; i++) {
    edades[i] = 18 + i;
}
```

Predice los valores antes de mostrarlos.

Resultado esperado:

```text
18 19 20 21 22
```

## 7. Error frecuente

Usar:

```java
i <= notas.length
```

Esto intenta alcanzar un índice que no existe.

## 8. Corrección

Para recorrer todos los índices:

```java
i < notas.length
```

## 9. Idea clave

El recorrido relaciona el índice variable `i` con cada posición del vector.

---

# 9. Ejemplo 4 — Buscar un dato

## 1. ¿Qué aprenderemos?

Localizar un valor y determinar si existe.

## 2. Situación

Buscaremos la nota `12`.

## 3. Código

```java
int[] notas = {15, 18, 12, 20};
int buscado = 12;
int posicion = -1;

for (int i = 0; i < notas.length; i++) {
    if (notas[i] == buscado) {
        posicion = i;
        break;
    }
}

if (posicion != -1) {
    System.out.println("Encontrado en índice: " + posicion);
} else {
    System.out.println("No encontrado");
}
```

## 4. ¿Qué ocurre?

```text
15 ¿= 12? no
18 ¿= 12? no
12 ¿= 12? sí
      |
      v
posición = 2
      |
      v
se detiene la búsqueda
```

Resultado:

```text
Encontrado en índice: 2
```

## 5. Modificación

Cambia `buscado` a `25`.

Antes de ejecutar: ¿qué valor conservará `posicion`?

Resultado esperado:

```text
No encontrado
```

## 6. Error frecuente

Confundir “valor buscado” con “índice”.

## 7. Idea clave

Buscar significa comparar el objetivo con los elementos hasta encontrar coincidencia o terminar el recorrido.

---

# 10. Ejemplo 5 — Agregar y eliminar dentro de un arreglo de tamaño fijo

## 1. ¿Qué aprenderemos?

Comprender que un arreglo Java tiene tamaño fijo y que “agregar” o “eliminar” dentro de esa capacidad requiere controlar posiciones y desplazar elementos.

## 2. Situación

Tenemos capacidad para 5 datos, pero inicialmente usamos 3.

```java
int[] datos = new int[5];
datos[0] = 10;
datos[1] = 20;
datos[2] = 30;
int usados = 3;
```

Representación:

```text
índice:  0   1   2   3   4
       +---+---+---+---+---+
dato:  |10 |20 |30 | 0 | 0 |
       +---+---+---+---+---+
usados = 3
```

## 3. Agregar al final lógico

```java
if (usados < datos.length) {
    datos[usados] = 40;
    usados++;
}
```

**Por qué funciona:** `usados` señala la primera posición libre.

Después:

```text
10 20 30 40 _
usados = 4
```

## 4. Eliminar una posición

Supongamos que eliminamos el índice 1. Para conservar juntos los elementos usados, desplazamos a la izquierda:

```java
int indiceEliminar = 1;

for (int i = indiceEliminar; i < usados - 1; i++) {
    datos[i] = datos[i + 1];
}

usados--;
```

Resultado lógico:

```text
10 30 40 _ _
usados = 3
```

**Importante:** el arreglo físico continúa teniendo tamaño 5. `usados` representa cuántas posiciones consideramos ocupadas.

## 5. Predicción

Si eliminas el índice 0, ¿qué valores deben desplazarse?

## 6. Error frecuente

Pensar que `datos.length` disminuye después de eliminar.

## 7. Corrección conceptual

El tamaño del array Java no cambia. La operación se modela dentro de la capacidad existente.

## 8. Idea clave

“Agregar/eliminar” en un array fijo no equivale a redimensionarlo automáticamente.

---

# 11. Ejemplo 6 — Ordenar datos de menor a mayor

## 1. ¿Qué aprenderemos?

Comprender la operación de ordenar mediante comparaciones e intercambios.

## 2. Estado inicial

```text
[18, 12, 20, 15]
```

## 3. Implementación didáctica

```java
int[] notas = {18, 12, 20, 15};

for (int i = 0; i < notas.length - 1; i++) {
    for (int j = 0; j < notas.length - 1 - i; j++) {
        if (notas[j] > notas[j + 1]) {
            int temporal = notas[j];
            notas[j] = notas[j + 1];
            notas[j + 1] = temporal;
        }
    }
}
```

## 4. ¿Qué ocurre?

Se comparan elementos vecinos. Si están en orden incorrecto, se intercambian. Las pasadas sucesivas van dejando los valores mayores hacia el final.

Resultado:

```text
[12, 15, 18, 20]
```

## 5. Modificación

Cambia los datos a:

```text
[20, 10, 15, 5]
```

Antes de ejecutar, escribe el orden final esperado.

## 6. Error frecuente

Intercambiar sin guardar temporalmente uno de los valores.

## 7. Idea clave

Ordenar cambia la posición de los elementos según un criterio, pero conserva la cantidad de elementos.

---

# 12. Ejemplo integrador — Registro de notas

Construye un vector:

```java
int[] notas = {14, 18, 11, 16, 20};
```

Realiza en este orden:

1. Muestra el tamaño.
2. Muestra cada índice y valor.
3. Cambia la nota del índice 2 a `13`.
4. Busca la nota `16`.
5. Ordena las notas ascendentemente.
6. Muestra el resultado final.

Antes de ejecutar cada modificación, escribe qué esperas observar.

Resultado final esperado después de modificar y ordenar:

```text
[13, 14, 16, 18, 20]
```

---

# 13. Actividades espejo

## Actividad A — Nombres

Crea un vector de cuatro nombres y muestra cada elemento con su índice.

## Actividad B — Temperaturas

Crea cinco temperaturas enteras. Modifica una posición y busca una temperatura indicada por el docente.

## Actividad C — Predicción

Dado:

```java
int[] datos = {7, 4, 9, 2};
```

Responde antes de programar:

1. ¿Cuál es `datos[0]`?
2. ¿Cuál es `datos[3]`?
3. ¿Cuál es `datos.length`?
4. ¿Qué ocurre conceptualmente si intentas acceder a `datos[4]`?
5. ¿Cómo quedaría ordenado ascendentemente?

---

# 14. Tabla de errores frecuentes

| Situación | Causa | Corrección |
|---|---|---|
| Acceso fuera del rango | índice inválido | usar `0` hasta `length - 1` |
| El recorrido intenta una posición extra | uso de `<=` | usar `< length` |
| Se confunde tamaño con último índice | índices desde 0 | último índice = `length - 1` |
| Se espera que el array crezca solo | array de tamaño fijo | controlar capacidad/posiciones |
| La búsqueda nunca informa “no encontrado” | falta estado inicial | usar una posición centinela, por ejemplo `-1` |

# 15. Evidencias sugeridas

- Archivo `VectoresDemo.java`.
- Captura o salida de consola del ejemplo integrador.
- Respuestas de las actividades espejo.

# 16. Checklist final

- [ ] Identifico nombre, tipo, tamaño, índice y elementos.
- [ ] Distingo tamaño de último índice.
- [ ] Accedo a una posición válida.
- [ ] Modifico un elemento.
- [ ] Recorro todas las posiciones.
- [ ] Busco un dato.
- [ ] Comprendo agregar/eliminar dentro de un array fijo.
- [ ] Comprendo el objetivo de ordenar.
- [ ] Puedo explicar un error de índice.

# 17. Cierre

Hoy aprendiste a representar un arreglo unidimensional, reconocer su estructura y manipular sus elementos mediante índices. Practicaste acceso, modificación, recorrido, búsqueda, incorporación, eliminación lógica y ordenamiento. Repasa especialmente la relación entre **tamaño**, **índice** y **rango válido**, porque muchos errores aparecen al confundirlos.

La siguiente conexión natural es utilizar este dominio de vectores como base para estructuras de datos de mayor complejidad mencionadas en la sesión.

**Blog:** https://lideratecacademy.com/  
**Canal YouTube:** https://www.youtube.com/@LideratecAcademy
