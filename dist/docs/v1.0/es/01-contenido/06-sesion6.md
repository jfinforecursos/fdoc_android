---
title: "Sesión 6: Jetpack Compose (Cero a Experto)"
description: "Módulo 1: Fundamentos y Paradigma Declarativo. Guía definitiva sobre Composables, Ciclo de Vida y Previews avanzados."
icon: mdi:android
order: 6
---

## Sesión 6: Jetpack Compose (Cero a Experto)

¡Bienvenidos al futuro del desarrollo Android! En esta sesión dejamos atrás el sistema tradicional de archivos XML para sumergirnos en **Jetpack Compose**, el toolkit moderno de Google que está revolucionando cómo construimos interfaces de usuario (UI).

---

## 1. Módulo 1: Fundamentos y Paradigma Declarativo

En esta primera parte, entenderemos por qué el mundo Android ha cambiado su forma de diseñar y cómo preparar nuestro cerebro para el pensamiento declarativo.

### 1.1 Introducción: ¿Qué es Jetpack Compose?

Jetpack Compose es un toolkit **declarativo**. A diferencia del sistema de vistas tradicional (imperativo), donde tú le dices al sistema *cómo* cambiar la UI paso a paso (ej. `view.setVisibility(GONE)`), en Compose tú describes *qué* debe mostrar la UI según el estado actual.

#### Imperativo (XML) vs. Declarativo (Compose)

+++comparison-table
---
headers:
  - "Característica"
  - "Sistema de Vistas (XML)"
  - "Jetpack Compose"
rows:
  - ["Paradigma", "Imperativo: Modificas manualmente los widgets.", "Declarativo: La UI se regenera según el estado."]
  - ["Código", "Separado: XML para diseño, Java/Kotlin para lógica.", "Unificado: Todo se escribe en Kotlin."]
  - ["Acoplamiento", "Alto: Necesitas `findViewById` o ViewBinding.", "Bajo: La UI es una función directa de los datos."]
  - ["Mantenimiento", "Complejo en interfaces dinámicas.", "Simple: Menos código, menos errores."]
---
+++

---

### 1.2 Configuración del Entorno

Antes de escribir código, asegúrate de que tu proyecto esté preparado. Aquí tienes la configuración completa para el archivo `app/build.gradle.kts`:

```kotlin
// Archivo: app/build.gradle.kts
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.compose.compiler)
}

android {
    namespace = "com.sena.jetpackcompose"
    compileSdk = 34

    buildFeatures {
        compose = true
    }
}

dependencies {
    // BOM (Bill of Materials) para armonizar versiones
    val composeBom = platform("androidx.compose:compose-bom:2024.05.00")
    implementation(composeBom)
    
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.material3:material3")
    implementation("androidx.compose.ui:ui-tooling-preview")
    debugImplementation("androidx.compose.ui:ui-tooling")
}
```

---

### 1.3 Funciones Composable: La Anatomía de la UI

Una función **Composable** es la unidad básica de construcción en Compose. No son funciones normales; son transformadoras de datos en UI.

#### Anatomía Detallada
Para que una función sea un Composable, debe cumplir con:
1. **Anotación `@Composable`**: Indica al compilador que esta función genera UI.
2. **Naming Convention**: Los nombres deben empezar con **Mayúscula** y ser sustantivos (ej. `BotonDeEnvio`).
3. **Pureza (Idealmente)**: Deben reaccionar a los parámetros de entrada y no tener efectos secundarios (Side Effects) directos.

#### Ciclo de Vida y Recomposición
A diferencia de los Fragments o Activities, el ciclo de vida de un Composable es más simple pero potente:
- **Composición**: Cuando el Composable se ejecuta por primera vez y se añade al árbol de UI.
- **Recomposición**: Cuando el Composable se vuelve a ejecutar porque sus datos de entrada (parámetros o estado interno) han cambiado.
- **Salida**: Cuando el Composable se elimina del árbol de UI.

> [!IMPORTANT]
> **Recomposición Inteligente**: Compose no redibuja toda la pantalla. Si solo un `Text` cambió dentro de una `Column` con 10 elementos, Compose solo volverá a ejecutar la función del `Text` específico.

#### Ejemplo Completo: Del Composable a la MainActivity
Aquí verás cómo se define un componente con estado y cómo se llama desde el punto de entrada de la aplicación.

+++tabs
---[tab title="MainActivity.kt" lang="kotlin"]---
package com.sena.jetpackcompose

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Scaffold
import androidx.compose.ui.Modifier
import com.sena.jetpackcompose.ui.theme.MyApplicationTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge() // Habilita diseño de borde a borde
        
        setContent {
            // Aplicamos el tema de la aplicación
            MyApplicationTheme {
                // Estructura base de Material Design 3
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    // LLAMADA PRINCIPAL: Pasamos el padding del Scaffold
                    ContadorMaster(modifier = Modifier.padding(innerPadding))
                }
            }
        }
    }
}
---[tab title="Componente.kt" lang="kotlin"]---
package com.sena.jetpackcompose

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.*
import androidx.compose.ui.unit.dp

@Composable
fun ContadorMaster(modifier: Modifier = Modifier) {
    // ESTADO: 'remember' persiste el valor durante la recomposición.
    // 'mutableIntStateOf' notifica a Compose que debe redibujar al cambiar.
    var contador by remember { mutableIntStateOf(0) }

    Column(
        modifier = modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = "Valor actual: $contador",
            style = MaterialTheme.typography.headlineLarge
        )

        Spacer(modifier = Modifier.height(16.dp))

        Button(onClick = { contador++ }) {
            Text("Incrementar número")
        }
    }
}
---
+++

---

### 1.4 Previsualización: El Laboratorio de Diseño (@Preview)

La anotación `@Preview` es una de las herramientas más potentes para los desarrolladores. Permite renderizar tus componentes en Android Studio sin compilar la app completa.

#### Parámetros de @Preview (Todas las Configuraciones)
Puedes personalizar casi cualquier aspecto de la vista previa:

| Parámetro | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `name` | Etiqueta que aparece sobre la vista previa. | `name = "Login Screen"` |
| `group` | Agrupa varias vistas previas en el panel. | `group = "Buttons"` |
| `showBackground` | Agrega un fondo (útil si el componente es transparente). | `showBackground = true` |
| `backgroundColor` | Define un color de fondo específico (formato Long). | `backgroundColor = 0xFF00FF00` |
| `showSystemUi` | Muestra la barra de estado y navegación. | `showSystemUi = true` |
| `device` | Simula un dispositivo específico. | `device = "id:pixel_7"` |
| `uiMode` | Permite activar el modo nocturno. | `uiMode = Configuration.UI_MODE_NIGHT_YES` |
| `locale` | Simula un idioma/región. | `locale = "es"` |
| `fontScale` | Prueba cómo se ve con fuentes extra grandes. | `fontScale = 1.5f` |

#### Ejemplos Maestros de Preview
Aquí tienes cómo configurar múltiples escenarios en un solo archivo para probar la robustez de tu UI.

```kotlin
package com.sena.jetpackcompose

import android.content.res.Configuration
import androidx.compose.material3.Surface
import androidx.compose.runtime.Composable
import androidx.compose.ui.tooling.preview.Preview
import com.sena.jetpackcompose.ui.theme.MyApplicationTheme

// 1. Múltiples Previews en una sola función
@Preview(name = "Claro", showBackground = true)
@Preview(name = "Oscuro", uiMode = Configuration.UI_MODE_NIGHT_YES, showBackground = true)
@Composable
fun PreviewContadorMultitema() {
    MyApplicationTheme {
        // Surface asegura que el fondo cambie según el tema (Blanco vs Gris oscuro)
        Surface {
            ContadorMaster()
        }
    }
}

// 2. Preview de Dispositivo Completo (Tablet vs Phone)
@Preview(name = "Celular", device = "id:pixel_7", showSystemUi = true)
@Preview(name = "Tablet", device = "spec:width=1280dp,height=800dp", showSystemUi = true)
@Composable
fun PreviewDispositivos() {
    MyApplicationTheme {
        ContadorMaster()
    }
}

// 3. Preview de Accesibilidad (Fuentes Grandes)
@Preview(name = "Accesibilidad - Fuente 200%", fontScale = 2.0f, showBackground = true)
@Composable
fun PreviewAccesibilidad() {
    MyApplicationTheme {
        ContadorMaster()
    }
}
```

---

+++admonition
---
type: tip
title: "Consejo de Experto"
---
No abuses de `showSystemUi = true` en todos tus componentes pequeños. Úsalo solo cuando estés previsualizando una **Pantalla Completa (Screen)**. Para componentes aislados (botones, tarjetas), usa solo `showBackground = true`.
+++

---

## 2. Módulo 2: Layouts y Componentes Básicos

En este segundo módulo, pasaremos a la acción construyendo layouts reales y utilizando los componentes visuales de Material Design 3.

### 2.1 Contenedores Estándar: Organizando la UI

En Compose no usamos "Layouts" complejos de XML. Usamos tres funciones básicas para posicionar elementos:

| Contenedor | Descripción | Analogía |
| :--- | :--- | :--- |
| **`Column`** | Apila elementos de arriba hacia abajo. | Una lista de tareas. |
| **`Row`** | Coloca elementos de izquierda a derecha. | Los botones de un reproductor de música. |
| **`Box`** | Apila elementos uno encima de otro (Z-axis). | Capas de una cebolla o un marco de fotos. |

#### Ejemplo de Estructura Básica
```kotlin
@Composable
fun EjemploEstructura() {
    Column {
        Text("Elemento 1 (Arriba)")
        Row {
            Text("Elemento A (Izquierda) ")
            Text("Elemento B (Derecha)")
        }
        Box {
            // Este texto estará detrás del siguiente
            Text("Fondo")
            Text("Frente") 
        }
    }
}
```

---

### 2.2 El Sistema de Diseño Material Design 3 (M3)

Material 3 (también conocido como **Material You**) es la evolución del lenguaje de diseño de Google. Es un sistema completo que garantiza profesionalismo y coherencia.

#### El Objeto MaterialTheme
En Compose, toda la información de diseño se centraliza en el objeto `MaterialTheme`, del que extraemos:
- **`colorScheme`**: Los colores de tu app.
- **`typography`**: Los estilos de texto.
- **`shapes`**: La redondez de las esquinas.

#### El Sistema de Colores (ColorScheme)
M3 usa una paleta de colores lógica basada en funciones:

| Color | Uso Principal |
| :--- | :--- |
| **`Primary`** | Color más destacado (ej. botones principales). |
| **`Secondary`** | Elementos menos prominentes (ej. etiquetas). |
| **`Tertiary`** | Colores de acento. |
| **`Surface`** | Color de fondo de contenedores. |
| **`Error`** | Estados de error o advertencias. |
| **`On[Color]`** | Color del texto/icono sobre el color base. |

> [!TIP]
> **Colores Dinámicos**: En Android 12+, M3 puede extraer colores del fondo de pantalla del usuario automáticamente.
> 
> **¿Cómo controlarlo?**: En tu `MainActivity`, verás la llamada `MyApplicationTheme(dynamicColor = true)`. 
> - **`true`**: La app se adapta al usuario.
> - **`false`**: La app usa estrictamente tus colores definidos en `Color.kt`. Úsalo para mantener la identidad de marca (ej. el azul de Facebook).

#### Tipografía y Estilos
Usa siempre los estilos del tema para asegurar legibilidad y escalabilidad:

```kotlin
// Forma CORRECTA
Text(
    text = "Título Profesional",
    style = MaterialTheme.typography.headlineMedium,
    color = MaterialTheme.colorScheme.primary
)
```

#### Los Cimientos: Surface y Scaffold
Son los contenedores que actúan como la "carcasa" de nuestra app:

- **`Surface`**: Hoja de papel virtual que aplica el fondo correcto y asegura contraste.
- **`Scaffold`**: Andamio que organiza `topBar`, `bottomBar`, `floatingActionButton` y el `content`.


#### Ejemplo Maestro: Surface y Scaffold en Acción
A continuación, verás un ejemplo del mundo real que utiliza todas las capacidades de organización de Material Design 3.

+++tabs
---[tab title="MainActivity.kt" lang="kotlin"]---
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyApplicationTheme {
                // 1. Surface: El 'lienzo' que maneja el fondo y el color del contenido
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background,
                    contentColor = MaterialTheme.colorScheme.onBackground
                ) {
                    // 2. Scaffold: El 'andamio' que organiza las piezas de la pantalla
                    PantallaPrincipal()
                }
            }
        }
    }
}
---[tab title="PantallaPrincipal.kt" lang="kotlin"]---
package com.sena.jetpackcompose

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun PantallaPrincipal() {
    Scaffold(
        // BARRA SUPERIOR
        topBar = {
            CenterAlignedTopAppBar(
                title = { Text("Mi App Sena") },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.primary
                )
            )
        },
        // BARRA INFERIOR
        bottomBar = {
            NavigationBar {
                NavigationBarItem(
                    selected = true,
                    onClick = { },
                    icon = { Icon(Icons.Default.Home, "Inicio") },
                    label = { Text("Inicio") }
                )
                NavigationBarItem(
                    selected = false,
                    onClick = { },
                    icon = { Icon(Icons.Default.Settings, "Ajustes") },
                    label = { Text("Ajustes") }
                )
            }
        },
        // BOTÓN FLOTANTE (FAB)
        floatingActionButton = {
            FloatingActionButton(onClick = { /* Acción */ }) {
                Icon(Icons.Default.Add, contentDescription = "Agregar")
            }
        }
    ) { paddingValores ->
        // CUERPO DE LA PANTALLA
        // Usamos el paddingValores para que el texto no quede debajo de la TopBar
        Column(
            modifier = Modifier
                .padding(paddingValores)
                .fillMaxSize()
                .padding(16.dp)
        ) {
            Text(
                text = "Bienvenido al curso de Android",
                style = MaterialTheme.typography.headlineSmall
            )
            Text(
                text = "Este es el contenido principal de tu aplicación, organizado perfectamente gracias al Scaffold.",
                style = MaterialTheme.typography.bodyMedium
            )
        }
    }
}
---
+++

---


### 2.3 Componentes Esenciales de Material Design 3

Bloques de construcción listos para usar:

1. **`Text`**: Muestra información con estilo.
2. **`Button`**: `Button` (Primario), `ElevatedButton` (Sombra), `OutlinedButton` (Borde), `TextButton` (Discreto).
3. **`TextField`**: Captura datos. Recomendado `OutlinedTextField`.
4. **`Card`**: Agrupa contenido con profundidad.
5. **`FloatingActionButton (FAB)`**: Acción más importante de la pantalla.

#### Ejemplo Maestro: Formulario de Usuario

+++tabs
---[tab title="MainActivity.kt" lang="kotlin"]---
// Llamada desde la función principal
setContent {
    MyApplicationTheme {
        Surface(color = MaterialTheme.colorScheme.background) {
            FormularioRegistro()
        }
    }
}
---[tab title="FormularioRegistro.kt" lang="kotlin"]---
package com.sena.jetpackcompose

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

@Composable
fun FormularioRegistro() {
    var nombre by remember { mutableStateOf("") }

    Scaffold(
        floatingActionButton = {
            FloatingActionButton(onClick = { /* Acción */ }) {
                Text("+")
            }
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .padding(padding)
                .fillMaxSize()
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Card(
                modifier = Modifier.fillMaxWidth(),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(modifier = Modifier.padding(16.dp)) {
                    Text(text = "Registro de Aprendiz", style = MaterialTheme.typography.titleLarge)
                    Spacer(modifier = Modifier.height(16.dp))
                    OutlinedTextField(
                        value = nombre,
                        onValueChange = { nombre = it },
                        label = { Text("Nombre Completo") },
                        modifier = Modifier.fillMaxWidth()
                    )
                    Spacer(modifier = Modifier.height(16.dp))
                    Button(
                        onClick = { println("Usuario: $nombre") },
                        modifier = Modifier.align(Alignment.End)
                    ) {
                        Text("Guardar Datos")
                    }
                }
            }
        }
    }
}
---
+++

---

### 2.4 Modificadores (Modifiers): Los Superpoderes

Los modificadores cambian el aspecto y comportamiento de un Composable.

> [!CAUTION]
> **El Orden Importa**: Se aplican en cadena. `padding` antes de `background` deja el espacio vacío. `background` antes de `padding` colorea el área y deja el espacio interno.

#### Propiedades Comunes:
- **`fillMaxSize()`**: Ocupa todo el espacio.
- **`padding(8.dp)`**: Margen interno.
- **`size(100.dp)`**: Tamaño fijo.
- **`background(Color.Red)`**: Color de fondo.
- **`border(1.dp, Color.Black)`**: Borde.

---

### 2.5 Imágenes y Recursos

#### Carga de Imágenes Locales y Remotas

```kotlin
// 1. Recursos (drawable)
Image(
    painter = painterResource(id = R.drawable.mi_foto),
    contentDescription = "Foto",
    modifier = Modifier.size(200.dp)
)

// 2. Icono de Material
Icon(
    imageVector = Icons.Default.Favorite,
    contentDescription = "Favorito",
    tint = Color.Red
)

// 3. Remotas (Coil) -> implementation("io.coil-kt:coil-compose:2.6.0")
AsyncImage(
    model = "https://tu-sitio.com/imagen.jpg",
    contentDescription = "Imagen remota",
    modifier = Modifier.clip(CircleShape)
)
```

---


## Actividad Guiada: Mi Primera App Modular con Jetpack Compose

¡Felicidades por llegar hasta aquí! En esta actividad vamos a poner en práctica lo aprendido pero con un enfoque profesional: **organizando nuestro código en múltiples archivos**. Construiremos un Dashboard de Usuario interactivo.

### Objetivo de la Actividad
Al finalizar esta guía, habrás construido una aplicación funcional y organizada en diferentes componentes, siguiendo las mejores prácticas de desarrollo Android.

---

## Paso 1: Preparación del Proyecto

1. Abre **Android Studio**.
2. Crea un nuevo proyecto (**New Project**) -> **Empty Compose Activity**.
3. Verifica que tu proyecto compile correctamente antes de empezar a dividir el código.

---

## Paso 2: El Punto de Entrada (MainActivity.kt)

El archivo `MainActivity` debe mantenerse limpio. Su única responsabilidad es iniciar la aplicación y aplicar el tema.

**Actualiza tu `MainActivity.kt` con el siguiente código:**

```kotlin
package com.sena.miapp

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import com.sena.miapp.ui.theme.MyApplicationTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            MyApplicationTheme(dynamicColor = false) {
                // Surface proporciona el fondo base según el tema
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    // Llamamos a nuestra pantalla principal (que crearemos en el Paso 3)
                    DashboardScreen()
                }
            }
        }
    }
}
```

---

## Paso 3: La Estructura de la Pantalla (DashboardScreen.kt)

Para mantener el orden, vamos a crear un nuevo archivo para la pantalla principal.

1. Haz clic derecho sobre tu paquete (ej. `com.sena.miapp`).
2. Selecciona **New -> Kotlin Class/File**.
3. Ponle de nombre **DashboardScreen** y asegúrate de elegir el tipo **File**.

**Copia este código en `DashboardScreen.kt`:**

```kotlin
package com.sena.miapp

import androidx.compose.foundation.layout.padding
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Add
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun DashboardScreen() {
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(
                title = { Text("Panel de Control Sena") },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primary,
                    titleContentColor = MaterialTheme.colorScheme.onPrimary
                )
            )
        },
        floatingActionButton = {
            FloatingActionButton(onClick = { /* Acción futura */ }) {
                Icon(Icons.Default.Add, contentDescription = "Agregar")
            }
        }
    ) { paddingValores ->
        // Aquí llamamos al componente de perfil (que crearemos en el Paso 4)
        ProfileCard(modifier = Modifier.padding(paddingValores))
    }
}
```

---

## Paso 4: El Componente de Perfil (ProfileComponent.kt)

Finalmente, crearemos un archivo dedicado para el contenido visual del perfil y su lógica de estado.

1. Crea otro archivo Kotlin llamado **ProfileComponent**.

**Copia este código en `ProfileComponent.kt`:**

```kotlin
package com.sena.miapp

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Person
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

@Composable
fun ProfileCard(modifier: Modifier = Modifier) {
    // Estado local para los puntos de aprendizaje
    var xpPoints by remember { mutableIntStateOf(0) }

    Column(
        modifier = modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        ElevatedCard(
            modifier = Modifier.fillMaxWidth()
        ) {
            Column(
                modifier = Modifier.padding(24.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Icon(
                    imageVector = Icons.Default.Person,
                    contentDescription = null,
                    modifier = Modifier.size(60.dp)
                )
                
                Text(
                    text = "Aprendiz: Software Dev",
                    style = MaterialTheme.typography.titleLarge
                )
                
                Spacer(modifier = Modifier.height(8.dp))
                
                Text(
                    text = "Nivel de Experiencia: $xpPoints",
                    style = MaterialTheme.typography.bodyLarge,
                    color = MaterialTheme.colorScheme.secondary
                )
            }
        }

        Spacer(modifier = Modifier.height(24.dp))

        Button(
            onClick = { xpPoints += 10 },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("¡Completar Lección (+10 XP)!")
        }
    }
}
```

---

## Resumen de la Estructura Final

Tu proyecto ahora debería tener esta organización de archivos:

1.  **`MainActivity.kt`**: Inicia la App y el Tema.
2.  **`DashboardScreen.kt`**: Define el `Scaffold` y la estructura global.
3.  **`ProfileComponent.kt`**: Define el contenido visual y la lógica del contador.


