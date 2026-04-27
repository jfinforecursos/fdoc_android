---
title: "Sesión 3: Dominio de Estructuras y Ciclos"
description: "Guía exhaustiva sobre Arrays, Listas, Sets y Maps con todas las formas posibles de iteración en Kotlin."
icon: mdi:sync
order: 3
---

## Sesión 3: Estructuras de Datos y Ciclos

Dominar cómo se almacenan y se recorren los datos es la base de cualquier aplicación Android. En esta sesión, desglosaremos las estructuras principales y aprenderemos a extraer su información usando todos los tipos de bucles disponibles en Kotlin.

---

## 1. Arreglos (Arrays)
Los Arrays son colecciones de **tamaño fijo**. Son los más eficientes en memoria y rendimiento cuando el número de elementos no va a cambiar.

### Formas de Iterar un Array:
+++tabs
---[tab title="Por Elemento (for)" lang="kotlin"]---
val planetas = arrayOf("Mercurio", "Venus", "Tierra", "Marte")

// La forma más directa
for (planeta in planetas) {
    println("Planeta: $planeta")
}
---[tab title="Por Índice (indices)" lang="kotlin"]---
// Útil si necesitas el número de la posición
for (i in planetas.indices) {
    println("Posición $i: ${planetas[i]}")
}
---[tab title="Con Índice y Valor (withIndex)" lang="kotlin"]---
// La forma más elegante de tener ambos en un for
for ((index, valor) in planetas.withIndex()) {
    println("El planeta #$index es $valor")
}
---[tab title="Funcional (forEach)" lang="kotlin"]---
// Estilo moderno en una sola línea
planetas.forEach { println("Visitando $it") }

// Con nombre personalizado
planetas.forEach { planeta -> println("Visitando $planeta") }
+++

---

## 2. Listas (Lists)
Las listas son **dinámicas**. Pueden crecer o achicarse (si son `MutableList`). Son la estructura más usada en el desarrollo móvil.

### Formas de Iterar una Lista:
+++tabs
---[tab title="Tradicional (while)" lang="kotlin"]---
val tareas = listOf("Comprar", "Lavar", "Estudiar")
var i = 0

while (i < tareas.size) {
    println("Tarea ${i + 1}: ${tareas[i]}")
    i++
}
---[tab title="Iterator (Bajo nivel)" lang="kotlin"]---
// Cómo funciona Kotlin por debajo
val it = tareas.iterator()
while (it.hasNext()) {
    println("Procesando: ${it.next()}")
}
---[tab title="forEachIndexed" lang="kotlin"]---
// La versión funcional que te da el índice
tareas.forEachIndexed { index, tarea ->
    println("Índice $index -> Tarea: $tarea")
}
+++

---

## 3. Conjuntos (Sets)
Un Set es una colección que **no permite duplicados**. Si intentas añadir "Rojo" dos veces, solo se guardará una.

### Iteración en Sets:
> **Nota:** Los Sets no tienen un orden garantizado por índice (no puedes hacer `set[0]`), por lo que siempre se recorren por elemento.

```kotlin
val colores = setOf("Rojo", "Verde", "Azul", "Rojo") // "Rojo" solo aparece una vez

// Recorrido simple
for (color in colores) {
    println("Color único: $color")
}

// Transformación en el camino
colores.filter { it.startsWith("R") }.forEach { println("Empieza con R: $it") }
```

---

## 4. Mapas (Maps)
Los mapas guardan datos en pares de **Clave to Valor**. Es como un diccionario: buscas una palabra (clave) para obtener su definición (valor).

### Formas de Iterar un Mapa:
+++tabs
---[tab title="Desestructuración" lang="kotlin"]---
val capitales = mapOf("CO" to "Bogotá", "MX" to "CDMX", "ES" to "Madrid")

// La forma más clara: separas clave y valor al instante
for ((codigo, ciudad) in capitales) {
    println("El código $codigo corresponde a $ciudad")
}
---[tab title="Por Claves o Valores" lang="kotlin"]---
// Solo las claves
for (codigo in capitales.keys) {
    println("Código: $codigo")
}

// Solo los valores
for (ciudad in capitales.values) {
    println("Ciudad: $ciudad")
}
---[tab title="forEach en Mapas" lang="kotlin"]---
capitales.forEach { (codigo, ciudad) ->
    println("Clave: $codigo | Valor: $ciudad")
}
+++

---

## 5. Cuadro Comparativo de Iteraciones

+++comparison-table
---
headers:
  - "Técnica"
  - "Estructura Recomendada"
  - "Ventaja Principal"
rows:
  - ["for (item in X)", "Todas", "Fácil de leer y estándar."]
  - ["for (i in X.indices)", "Arrays / Listas", "Acceso directo a la posición."]
  - ["forEach { }", "Listas / Sets / Maps", "Conciso, ideal para lógica simple."]
  - ["while / Iterator", "Cualquiera", "Control total sobre el avance del ciclo."]
  - ["withIndex()", "Listas / Arrays", "Obtienes posición y dato sin contadores manuales."]
---
+++

---

+++admonition
---
type: success
title: "Consejo Pro: ¿Cuál elegir?"
---
En Android, el 90% de las veces usarás **`for (item in lista)`** por su legibilidad o **`lista.forEach { }`** por su brevedad. Usa **`withIndex()`** solo cuando el número de la posición sea realmente necesario para tu lógica de negocio (ej: pintar una lista con números 1, 2, 3...).
+++
