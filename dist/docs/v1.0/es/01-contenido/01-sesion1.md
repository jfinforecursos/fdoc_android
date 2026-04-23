---
title: "Sesión 1: El Salto a Kotlin (Sintaxis y Seguridad)"
description: "Inmersión profunda en los fundamentos de Kotlin: desde interoperabilidad hasta seguridad de nulos extrema."
icon: mdi:numeric-1-box-outline
order: 1
---

+++hero-section
---
title: "Sesión 1: El Universo Kotlin"
subtitle: "Tu puerta de entrada al desarrollo Android moderno. Aprende desde cero por qué Kotlin es el lenguaje preferido por Google y cómo dominar su sintaxis."
backgroundImage: "https://images.unsplash.com/photo-1607791758543-462159037357?q=80&w=2070"
overlayOpacity: 0.6
buttons:
  - text: "Comenzar el Viaje"
    url: "#introduccion"
    variant: "primary"
    icon: "RocketIcon"
---
+++

## Sesión 1: Fundamentos para Principiantes

¡Bienvenido! Si vienes de otros lenguajes o si este es tu primer contacto con la programación, Kotlin es una elección fantástica. Es un lenguaje **conciso** (escribes menos para hacer más), **seguro** (evita muchos errores comunes) e **interoperable** (funciona perfectamente con código Java).

## 1. ¿Por qué Kotlin? (La Filosofía)

Antes de ver código, entendamos por qué existe. Imagina que Java es un coche robusto pero con muchos botones y palancas manuales. **Kotlin es ese mismo coche, pero con transmisión automática, sensores de proximidad y frenado de emergencia.**

+++stat-cards
---
columns: 3
items:
  - icon: "ShieldCheckIcon"
    value: "Seguridad"
    label: "Adiós a los errores de nulo"
    color: "blue"
  - icon: "ZapIcon"
    value: "Conciso"
    label: "40% menos código que Java"
    color: "green"
  - icon: "SmartphoneIcon"
    value: "Android"
    label: "Lenguaje oficial #1"
    color: "purple"
---
+++

---

## 2. Variables: Guardando Información

Piensa en una variable como una **caja etiquetada**. En Kotlin, tenemos dos tipos principales de cajas:

*   **`val` (Inmutable):** Es una caja con candado. Una vez que guardas algo, no puedes cambiarlo. Úsala para valores que no cambian (como tu fecha de nacimiento).
*   **`var` (Mutable):** Es una caja abierta. Puedes sacar lo que hay dentro y poner algo nuevo en cualquier momento.

### Tipos de Datos Comunes
*   `String`: Texto (ej: "Hola Mundo")
*   `Int`: Números enteros (ej: 25)
*   `Double`: Números con decimales (ej: 3.14)
*   `Boolean`: Solo dos opciones: `true` (verdadero) o `false` (falso).

+++tabs
---[tab title="Ejemplos de Variables" lang="kotlin"]---
// val no se puede cambiar (ReadOnly)
val nombre: String = "Juan" 
val pi = 3.14 // Kotlin adivina que es Double (Inferencia de tipo)

// var se puede cambiar
var edad: Int = 20
edad = 21 // ¡Correcto!
+++

**¿Qué es la Inferencia?**
Kotlin es inteligente. Si escribes `val saludo = "Hola"`, no necesitas decirle que es un `String`. Él lo deduce al ver las comillas. Esto hace que el código se vea mucho más limpio.

### Inicialización Especial (Para Android)
En Android, a veces creamos la "caja" pero no tenemos el objeto hasta que la pantalla se carga.

+++accordion
---
allowMultiple: false
---
### ¿Qué es `lateinit`?
Significa "Prometo inicializar esto más tarde". Se usa solo con `var`. Es muy común para botones o vistas de la interfaz que se configuran en el `onCreate`.

### ¿Qué es `by lazy`?
Significa "No crees esto hasta que realmente lo necesite". Es genial para ahorrar memoria. Si nunca usas la variable, nunca se gasta memoria en crearla.
+++

---

## 3. Null Safety: El "Escudo" de Kotlin

El error más común en programación es intentar usar algo que no existe (un `NullPointerException`). Kotlin inventó un sistema para evitar esto.

**Analogía:** Imagina una caja que puede estar vacía. En otros lenguajes, si intentas abrir una caja vacía, el programa explota. En Kotlin, debes marcar la caja con un signo de interrogación (`?`) para avisar que podría estar vacía.

+++tabs
---[tab title="Declaración Segura" lang="kotlin"]---
val nombre: String = "Juan" // Nunca puede ser nulo
// nombre = null // ERROR DE COMPILACIÓN

val nombreOpcional: String? = null // Puede ser nulo gracias al '?'
+++

### Técnicas Avanzadas de Seguridad
Cuando trabajamos con datos que vienen de internet o de una base de datos, siempre usamos tipos nulables (`?`).

```kotlin
val respuestaApi: String? = obtenerDato()

// 1. Llamada segura (?.)
println(respuestaApi?.length) // Si es nulo, imprime "null" en vez de explotar

// 2. Operador Elvis (?:) - Valor por defecto
val longitud = respuestaApi?.length ?: 0 // Si es nulo, usa 0

// 3. El operador "let" (Muy común en Android)
respuestaApi?.let { texto ->
    // Este código SOLO se ejecuta si respuestaApi NO es nulo
    println("El texto tiene ${texto.length} caracteres")
}

// 4. El operador de aserción (!!) - ¡Peligro!
// Indica: "Estoy 100% seguro de que no es nulo, si lo es, explota".
// Úsalo con mucha precaución.
val longitudForzada = respuestaApi!!.length 
```

---

## 4. Control de Flujo: Tomando Decisiones

Kotlin transforma las estructuras tradicionales en herramientas más potentes y legibles.

### A. Condicionales (if, else)
En Kotlin, el `if` no solo sirve para decidir qué camino tomar, ¡también puede devolver un valor! Esto se llama **if como expresión**.

+++tabs
---[tab title="if tradicional" lang="kotlin"]---
val edad = 18

if (edad >= 18) {
    println("Eres mayor de edad")
} else {
    println("Eres menor de edad")
}

---[tab title="if como expresión" lang="kotlin"]---
val temperatura = 25
// El resultado del if se guarda directamente en la variable
val clima = if (temperatura > 30) "Caluroso" else "Agradable"

println("El clima está $clima")
+++

**Nota:** Cuando usas `if` como expresión, el bloque `else` es obligatorio porque la variable siempre debe recibir un valor.

### B. El menú inteligente: `when`
Es la versión mejorada del `switch` de otros lenguajes. Es mucho más flexible y potente.

+++tabs
---[tab title="Variedades de when" lang="kotlin"]---
val puntos = 85

when (puntos) {
    100 -> println("Puntaje perfecto")
    in 90..99 -> println("Excelente")
    in 70..89 -> println("Buen trabajo")
    is Int -> println("Es un número entero válido")
    else -> println("Sigue intentando")
}
+++

### C. Bucles: Repitiendo Tareas
Los bucles nos permiten ejecutar el mismo código varias veces.

#### 1. Bucle `for` (Rangos y Progresiones)
Ideal cuando sabes exactamente cuántas veces quieres repetir algo.

```kotlin
// Del 1 al 5 (incluyendo el 5)
for (i in 1..5) { println(i) }

// Hasta el 5 (sin incluirlo: 1, 2, 3, 4)
for (i in 1 until 5) { println(i) }

// Hacia atrás con saltos
for (i in 10 downTo 0 step 2) { println(i) } // 10, 8, 6, 4, 2, 0
```

#### 2. `while` y `do-while` (Condicionales)
Se usan cuando no sabes cuántas veces se repetirá la tarea, solo que debe seguir mientras se cumpla una condición.

+++tabs
---[tab title="while vs do-while" lang="kotlin"]---
var bateria = 10

// while: Primero pregunta, luego actúa. 
// Si la batería es 0, nunca entra.
while (bateria > 0) {
    println("Usando el móvil... Batería: $bateria%")
    bateria--
}

// do-while: Primero actúa, luego pregunta.
// ¡Garantiza que el código se ejecute al menos una vez!
do {
    println("Intentando encender...")
} while (bateria > 0)
+++

---

+++admonition
---
type: tip
title: ¿Sabías que?
---
Kotlin es 100% compatible con Java. Esto significa que puedes tener archivos de ambos lenguajes en el mismo proyecto de Android y funcionarán perfectamente juntos. Además, Android Studio puede convertir automáticamente tu código Java a Kotlin con un solo clic.
+++
