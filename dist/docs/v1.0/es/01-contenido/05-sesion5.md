---
title: "Sesión 5: Instalación y Configuración del Entorno"
description: "Guía ultra-detallada para preparar Android Studio, configurar dispositivos reales y emuladores desde cero."
icon: mdi:cog-outline
order: 5
---

# Sesión 5: Preparando nuestro Laboratorio de Desarrollo

¡Bienvenido! Antes de empezar a construir la próxima gran aplicación, necesitamos que tu computadora hable el mismo lenguaje que Android. Esta sesión es fundamental: **un entorno bien configurado te ahorrará horas de errores frustrantes en el futuro.**

---

## 1. Descarga e Instalación de Android Studio

Android Studio es el "cuartel general" donde ocurre toda la magia. Sigue estos pasos con cuidado:

### A. Descarga
1. Entra al sitio oficial: [developer.android.com/studio](https://developer.android.com/studio).
2. Haz clic en el botón grande **"Download Android Studio"**.
3. Acepta los términos y condiciones.
4. El instalador detectará automáticamente si usas Windows, Mac (Intel o Apple Silicon) o Linux.

### B. Proceso de Instalación (Windows)
1. Ejecuta el archivo `.exe`.
2. En la ventana "Choose Components", asegúrate de que **Android Virtual Device** esté marcado.
3. Dale a "Next" a todo hasta que termine la barra verde.

### C. El Primer Inicio (Configuración Inicial)
Al abrirlo por primera vez, verás el **Setup Wizard**:
*   **Import Settings:** Elige "Do not import settings" (si eres nuevo).
*   **Install Type:** Selecciona **"Standard"**. Esto instalará lo que el 99% de los desarrolladores necesitan.
*   **Select UI Theme:** Elige entre el modo oscuro (Dracula) o claro. ¡El modo oscuro es mejor para tus ojos!
*   **Verify Settings:** Aquí verás una lista de componentes. Haz clic en **Next**.
*   **License Agreement:** **Paso Crítico.** Debes hacer clic en cada carpeta de la izquierda (android-sdk-license, android-sdk-arm-dbt-license, etc.) y marcar **"Accept"** en cada una. El botón "Finish" solo se activará cuando hayas aceptado todas.

---

## 2. Configurando tu Primer Emulador (Dispositivo Virtual)

Un emulador es un teléfono "de mentira" que corre dentro de tu PC. Es ideal para probar rápidamente sin conectar cables.

### Pasos para crear un AVD (Android Virtual Device):

1. En la pantalla principal, haz clic en **More Actions** -> **Virtual Device Manager**.
2. Haz clic en **"Create Device"** (botón azul arriba a la izquierda).
3. **Selecciona el Hardware:** Busca el **Pixel 7**. Asegúrate de que tenga el ícono de una pequeña bolsa de compras (Play Store). Esto te permitirá instalar apps de Google si lo necesitas.
4. **Sistema Operativo (System Image):**
    *   Verás pestañas como "Recommended". Busca la versión más reciente (ejemplo: **VanillaIceCream** o **UpsideDownCake**).
    *   Si ves un enlace azul que dice **"Download"** al lado del nombre, haz clic en él y espera a que termine.
    *   Una vez descargado, selecciónalo y dale a **Next**.
5. **Configuración Final:** Puedes cambiarle el nombre a algo como "Mi Tele Virtual".
6. **Hardware Acceleration:** Si tu PC te da un aviso sobre "HAXM" o "KVM", instálalo cuando te lo pida; es necesario para que el emulador no sea lento.

+++admonition
---
type: warning
title: "Ojo con los recursos"
---
Si tu computadora tiene 8GB de RAM o menos, evita tener muchas pestañas de Chrome abiertas mientras usas el emulador, ya que ambos consumen mucha memoria.
+++

---

## 3. Conectando tu Smartphone Real (Modo Desarrollador)

+++video
---
src: "https://www.youtube.com/watch?v=3wIT5NiEmJU"
title: "Conectando tu Smartphone Real (Modo Desarrollador)"
---
+++

Nada supera la sensación de tocar tu propia app en tus manos. Para esto, necesitamos "desbloquear" tu teléfono.

### Paso 1: Activar el Menú Oculto
Los teléfonos Android ocultan las herramientas de programación para que los usuarios normales no borren cosas por error.
1. Ve a **Ajustes** -> **Acerca del teléfono** -> **Información de software**.
2. Busca el **Número de compilación** (Build Number).
3. Toca ese número **7 veces seguidas**. Verás una cuenta regresiva: *"Estás a 3 pasos... 2... 1..."* hasta que diga: **"¡Ya eres desarrollador!"**.

### Paso 2: Activar la Depuración USB
1. Vuelve al menú principal de **Ajustes**.
2. Al final de todo (o en Sistema), aparecerá **"Opciones de desarrollador"**. Entra ahí.
3. Activa el interruptor de **Depuración por USB**.

### Paso 3: Autorizar la Computadora
1. Conecta el teléfono a la PC con un cable USB.
2. Mira la pantalla de tu celular. Aparecerá un mensaje: *"¿Permitir depuración por USB?"*.
3. **IMPORTANTE:** Marca la casilla **"Permitir siempre desde esta computadora"** y dale a **Aceptar**. Si no haces esto, Android Studio verá el teléfono pero dirá "Unauthorized".

---

## 4. Creando tu Primer Proyecto Kotlin

Para verificar que todo funciona, hagamos una prueba rápida:

1. Haz clic en **"New Project"**.
2. Selecciona la plantilla **"Empty Device Activity"** (ícono con un teléfono y una estrella).
3. **Name:** Ponle "HolaMundo".
4. **Language:** Asegúrate de que diga **Kotlin**.
5. **Minimum SDK:** Elige **API 24: Android 7.0 (Nougat)**. Esto hará que tu app funcione en casi cualquier teléfono actual.
6. Haz clic en **Finish**.

### ¿Qué esperar ahora?
Android Studio empezará a descargar archivos de configuración (esto se llama **Gradle Sync**). Verás una barra de carga abajo a la derecha. **No toques nada hasta que esa barra desaparezca.**

+++admonition
---
type: success
title: "Prueba de Fuego"
---
Cuando la barra termine, mira la parte superior de Android Studio. Debería aparecer el nombre de tu teléfono o el emulador que creaste. Haz clic en el botón **Play (Triángulo Verde)**. Si ves una pantalla blanca que dice "Hello Android!", ¡felicidades! Ya eres oficialmente un desarrollador Android.
+++

---

## Guía Completa: Creación de una App con Botón y Contador Dinámico

Esta guía explica paso a paso cómo se ha construido este proyecto en Android Studio usando **Jetpack Compose**.

---

## 1. Creación del Proyecto
Para recrear este proyecto desde cero en Android Studio:
1.  **File > New > New Project**.
2.  Selecciona **Empty Compose Activity**.
3.  Nombre: `My Application`.
4.  Package name: `com.example.myapplication`.
5.  Language: `Kotlin`.
6.  Minimum SDK: `API 24` (Android 7.0).

---

## 2. Estructura de Archivos Clave
- **`app/build.gradle.kts`**: Aquí se definen las librerías. El proyecto utiliza `Material3` y las librerías base de `Compose`.
- **`AndroidManifest.xml`**: Declara la actividad principal (`MainActivity`) y permite que la app se ejecute.
- **`MainActivity.kt`**: Contiene todo el código de la interfaz de usuario.
- **`ui.theme/`**: Contiene la configuración de colores (`Color.kt`), tipografía (`Type.kt`) y el tema general (`Theme.kt`).

---

## 3. Explicación Detallada del Código (`MainActivity.kt`)

### A. La Actividad Principal (`MainActivity`)
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge() // Hace que la app ocupe toda la pantalla
        setContent {
            MyApplicationTheme { 
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    // Llamamos a nuestro componente principal
                    MainCounterContent(modifier = Modifier.padding(innerPadding))
                }
            }
        }
    }
}
```

### B. El Componente de Interfaz (`MainCounterContent`)
Este es un "Composable", una función que define una parte de la pantalla.

```kotlin
@Composable
fun MainCounterContent(modifier: Modifier = Modifier) {
    // 1. ESTADO: 'remember' guarda el valor, 'mutableStateOf' avisa a Compose que debe redibujar si cambia.
    var count by remember { mutableStateOf(0) }

    // 2. DISPOSICIÓN: 'Column' organiza los elementos uno debajo de otro.
    Column(
        modifier = modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center, // Centra verticalmente
        horizontalAlignment = Alignment.CenterHorizontally // Centra horizontalmente
    ) {
        // 3. TEXTO: Muestra el valor actual de 'count'.
        Text(
            text = "Has presionado el botón: $count veces",
            fontSize = 20.sp,
            modifier = Modifier.padding(bottom = 16.dp)
        )
        
        // 4. BOTÓN: Al hacer clic, incrementamos 'count'.
        Button(onClick = { count++ }) {
            Text(text = "Presióname")
        }
    }
}
```

---

## 4. Conceptos Fundamentales
- **Recomposición**: Es el proceso por el cual Compose vuelve a ejecutar las funciones cuando el estado cambia. Al hacer `count++`, el `Text` se actualiza solo.
- **Estado (State)**: Usamos `mutableStateOf` para crear datos que pueden cambiar y que la UI debe observar.
- **Layouts**: `Column`, `Row` y `Box` son las formas básicas de organizar elementos.

---

## 5. Previsualización
El código incluye `MainCounterPreview` con la anotación `@Preview`, permitiendo ver el diseño sin ejecutar la app completa.

```kotlin
@Preview(showBackground = true)
@Composable
fun MainCounterPreview() {
    MyApplicationTheme {
        MainCounterContent()
    }
}
```