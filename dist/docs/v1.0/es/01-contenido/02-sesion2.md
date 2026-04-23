---
title: "Sesión 2: Funciones y Estructuras de Datos"
description: "Aprende a organizar lógica con funciones avanzadas y a manejar datos con colecciones inmutables."
icon: mdi:numeric-2-box-outline
order: 2
---

## Sesión 2: Funciones y Estructuras de Datos

En esta sesión pasaremos de piezas sueltas de código a construir "máquinas" reutilizables. Aprenderemos a organizar nuestra lógica y a manejar grupos de datos de forma eficiente.

## 1. Funciones: Tu Fábrica Personal

Imagina que una función es una **máquina de jugos**.
1.  **Entrada (Parámetros):** Le das frutas (datos).
2.  **Proceso (Cuerpo):** La máquina tritura y mezcla.
3.  **Salida (Retorno):** Obtienes el jugo (el resultado).

+++tabs
---[tab title="Código de la Máquina" lang="kotlin"]---
// 1. Una máquina que saluda
fun saludar(nombre: String): String {
    return "¡Hola, $nombre! Bienvenido a Kotlin."
}

// 2. Una máquina que suma
fun sumar(a: Int, b: Int) = a + b 

// 3. Máquina con configuración opcional
fun prepararCafe(tipo: String, conAzucar: Boolean = false) {
    val extra = if (conAzucar) "con azúcar" else "amargo"
    println("Preparando un $tipo $extra")
}
+++

**¿Qué acabamos de ver?**
*   **Parámetros por Defecto:** En `prepararCafe`, si no dices nada sobre el azúcar, Kotlin asume que es `false`. ¡Menos trabajo para ti!
*   **Funciones de una Línea:** Si la función es muy simple (como `sumar`), no necesitas llaves `{}` ni la palabra `return`. El signo `=` lo hace todo.
*   **Llamadas Nombradas:** Puedes decir `prepararCafe(conAzucar = true, tipo = "Capuchino")`. No importa el orden si dices los nombres.

---

## 2. Colecciones: ¿Caja Fuerte o Estantería?

En programación, casi siempre trabajamos con listas de cosas (usuarios, mensajes, canciones). Kotlin tiene dos formas de guardar estas listas:

### A. Listas Inmutables (`listOf`)
Es como una **foto**. Una vez tomada, no puedes añadir ni quitar personas de la imagen. Es más rápida y segura.

### B. Listas Mutables (`mutableListOf`)
Es como un **carrito de compras**. Puedes añadir productos, quitarlos o vaciarlo.

+++comparison-table
---
headers:
  - "Acción"
  - "Inmutable (listOf)"
  - "Mutable (mutableListOf)"
rows:
  - ["Leer datos", "✅ Sí", "✅ Sí"]
  - ["Añadir elementos", "❌ No", "✅ Sí"]
  - ["Eliminar elementos", "❌ No", "✅ Sí"]
  - ["Cambiar orden", "❌ No", "✅ Sí"]
---
+++

### Operaciones Maestras con Listas
Kotlin ofrece funciones de "orden superior" para manipular datos sin usar bucles `for` complejos, pero es vital saber cómo recorrerlas de forma tradicional cuando sea necesario.

+++tabs
---[tab title="Recorrido Tradicional" lang="kotlin"]---
val lenguajes = listOf("Kotlin", "Java", "Swift")

// 1. For simple (el más común)
for (lenguaje in lenguajes) {
    println("Aprendiendo $lenguaje")
}

// 2. For con índice
for ((indice, valor) in lenguajes.withIndex()) {
    println("Puesto $indice: $valor")
}

---[tab title="Recorrido Funcional" lang="kotlin"]---
val precios = listOf(10.0, 20.0, 30.0)

// Usando forEach (una sola línea)
precios.forEach { precio -> println("Precio: $precio") }

// Filtrado y Mapeo (Encadenamiento)
val caros = precios
    .filter { it > 15.0 }
    .map { it * 1.16 } // Aplicando impuesto
+++

**Recorriendo Mapas (Diccionarios):**
Los mapas guardan pares de **Clave: Valor**. Recorrerlos en Kotlin es sumamente elegante:

```kotlin
val capitales = mapOf("España" to "Madrid", "Colombia" to "Bogotá")

for ((pais, capital) in capitales) {
    println("La capital de $pais es $capital")
}
```

---

## 3. Extension Functions: ¡Dales Superpoderes!

¿Te gustaría que todos los textos (`String`) de tu app tuvieran una función para convertirse en "Emoji"? En Kotlin puedes "pegarle" funciones a clases que ya existen sin tener que heredar de ellas.

+++tabs
---[tab title="Dando Superpoderes" lang="kotlin"]---
// Ejemplo 1: Celebración
fun String.celebrar() = "🥳 $this 🥳"

// Ejemplo 2: Formato de Moneda (Muy útil en Android)
fun Double.toCurrency() = "$${String.format("%.2f", this)}"

fun main() {
    val miTexto = "Gané el torneo"
    println(miTexto.celebrar()) // 🥳 Gané el torneo 🥳

    val precio = 19.99
    println(precio.toCurrency()) // $19.99
}
+++

**¿Por qué es útil?**
**Evita tener clases gigantescas de "Utilidades".** Simplemente añades el comportamiento justo donde lo necesitas, haciendo que el código se lea casi como un libro. En Android, se usan mucho para simplificar el manejo de Vistas (Views).

---

+++admonition
---
type: success
title: Resumen de la Sesión
---
Ahora sabes cómo crear funciones flexibles con parámetros por defecto, cómo elegir la colección correcta para tus datos y cómo extender las funcionalidades del lenguaje con tus propias ideas. ¡Estás listo para el siguiente nivel!
+++



