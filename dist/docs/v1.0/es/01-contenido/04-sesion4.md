---
title: "Sesión 4: Programación Orientada a Objetos (POO)"
description: "Guía completa y detallada sobre Clases, Objetos, Herencia, Interfaces y Encapsulamiento en Kotlin."
icon: mdi:cube-outline
order: 4
---

## Sesión 4: Programación Orientada a Objetos (POO)

En esta sesión entraremos al paradigma más utilizado en el desarrollo de software y en la creación de aplicaciones Android. La **Programación Orientada a Objetos (POO)** nos permite modelar problemas del mundo real mediante código limpio, organizado y reutilizable.

---

## 1. El Concepto: Clases y Objetos

Para entender la POO, piensa en la diferencia entre un **plano arquitectónico** y una **casa real**:

*   **Clase:** Es el plano o molde. Define qué características (propiedades) y qué acciones (métodos) tendrá lo que construyamos.
*   **Objeto:** Es la casa real construida a partir de ese plano. Es una instancia concreta de la clase.

### Ejemplo en Kotlin:
+++tabs
---[tab title="Definición de Clase" lang="kotlin"]---
// Definimos el "molde" para cualquier Persona
class Persona(val nombre: String, var edad: Int) {
    
    // Acción que todas las personas pueden hacer
    fun saludar() {
        println("¡Hola! Mi nombre es $nombre y tengo $edad años.")
    }
}
---[tab title="Instanciación de Objetos" lang="kotlin"]---
fun main() {
    // Creamos dos objetos únicos usando el mismo molde
    val juan = Persona("Juan Pérez", 25)
    val maria = Persona("María Gómez", 30)

    // Usamos sus propiedades y métodos
    juan.saludar()  // Salida: ¡Hola! Mi nombre es Juan Pérez y tengo 25 años.
    maria.saludar() // Salida: ¡Hola! Mi nombre es María Gómez y tengo 30 años.
}
+++

---

## 2. Constructores e Inicialización (`init`)

En Kotlin, el **constructor primario** se define directamente al lado del nombre de la clase. Además, disponemos de bloques `init` para ejecutar código en el momento exacto en que el objeto nace.

+++tabs
---[tab title="Constructor e Init" lang="kotlin"]---
class Auto(val marca: String, val modelo: String) {
    
    // Bloque de inicialización: se ejecuta al crear el objeto
    init {
        println("Se ha fabricado un nuevo auto: $marca $modelo")
    }

    // Constructor secundario (opcional): si no se pasa modelo
    constructor(marca: String) : this(marca, "Modelo Estándar") {
        println("Constructor secundario ejecutado.")
    }
}

fun main() {
    val auto1 = Auto("Toyota", "Corolla") // Activa el init
    val auto2 = Auto("Ford")              // Activa el secundario y el init
}
+++

---

## 3. Los 4 Pilares de la POO

La POO se sostiene sobre cuatro conceptos fundamentales. Comprenderlos a fondo te convertirá en un desarrollador Android profesional:

### A. Abstracción
La **Abstracción** consiste en enfocarse únicamente en los detalles que son relevantes para el propósito de tu aplicación, ignorando el resto. Es el proceso de simplificar un objeto del mundo real para convertirlo en código.

> **Analogía:** Piensa en el **volante y los pedales de un carro**. Para conducir, solo necesitas saber qué hace el pedal del acelerador y el freno. No necesitas ser un ingeniero mecánico ni entender cómo se inyecta el combustible en los cilindros del motor.

+++tabs
---[tab title="Enfoque de Abstracción" lang="kotlin"]---
// Imaginemos que creamos una app de comercio electrónico. 
// De un Producto en la tienda, nos interesa su precio y stock, no su peso exacto o fabricante.
class Producto(
    val id: String,
    val nombre: String,
    var precio: Double,
    var stock: Int
) {
    fun hayStock(): Boolean = stock > 0

    fun aplicarDescuento(porcentaje: Double) {
        if (porcentaje in 1.0..100.0) {
            precio -= (precio * (porcentaje / 100))
        }
    }
}
---[tab title="Uso del Concepto" lang="kotlin"]---
fun main() {
    val celular = Producto("PROD-001", "Smartphone Android", 500.0, 10)
    
    println("¿Hay stock disponible?: ${celular.hayStock()}")
    celular.aplicarDescuento(10.0)
    println("Nuevo precio tras descuento: $${celular.precio}")
}
+++

---

### B. Encapsulamiento
El **Encapsulamiento** consiste en ocultar el estado interno de un objeto (sus variables) y proteger sus datos de accesos o modificaciones no autorizadas. Esto se logra haciendo las propiedades privadas y exponiendo únicamente métodos seguros (públicos) para interactuar con ellas.

> **Analogía:** Una **cápsula de medicina** protege los químicos internos. Tú te tomas la pastilla (interfaz pública) sin ver ni alterar los polvos de su interior (datos privados).

+++comparison-table
---
headers:
  - "Modificador"
  - "Visibilidad"
  - "Uso Común"
rows:
  - ["`public` (por defecto)", "Visible en cualquier parte del proyecto.", "Métodos generales de uso común."]
  - ["`private`", "Solo visible dentro de la misma clase.", "Variables internas de estado."]
  - ["`protected`", "Visible en la clase y en sus subclases (hijos).", "Métodos para herencia."]
  - ["`internal`", "Solo visible dentro del mismo módulo/paquete.", "Lógica de librerías internas."]
---
+++

+++tabs
---[tab title="Control de Estado" lang="kotlin"]---
class CuentaBancaria(val titular: String, private var saldo: Double) {
    
    // Método público para depositar dinero de forma segura
    fun depositar(monto: Double) {
        if (monto > 0) {
            saldo += monto
            println("Depósito exitoso de $$monto. Nuevo saldo: $$saldo")
        } else {
            println("Error: El monto a depositar debe ser positivo.")
        }
    }

    // Método público para retirar dinero controlando las reglas de negocio
    fun retirar(monto: Double) {
        if (monto > 0 && monto <= saldo) {
            saldo -= monto
            println("Retiro exitoso de $$monto. Saldo restante: $$saldo")
        } else {
            println("Error: Fondos insuficientes o monto inválido.")
        }
    }

    // El saldo está protegido contra modificaciones externas directas (ej. cuenta.saldo = 99999)
    // Pero permitimos leerlo mediante este método público
    fun obtenerSaldo(): Double = saldo
}
---[tab title="Prueba de Encapsulamiento" lang="kotlin"]---
fun main() {
    val miCuenta = CuentaBancaria("Carlos Mendoza", 1000.0)

    miCuenta.depositar(500.0)  // Correcto
    miCuenta.retirar(200.0)    // Correcto
    miCuenta.retirar(2000.0)   // Bloqueado por regla de negocio
    
    println("Saldo final consultado: $$" + miCuenta.obtenerSaldo())
}
+++

---

### C. Herencia

La **Herencia** es el mecanismo mediante el cual una clase nueva (clase hija) adquiere las propiedades y los métodos de otra clase ya existente (clase padre). Esto permite reutilizar código y establecer una jerarquía de tipos.

#### ¿Por qué es necesaria la palabra clave `open` en Kotlin?

En muchos lenguajes de programación (como Java o C#), las clases están abiertas por defecto. En cambio, en Kotlin, por diseño de seguridad y buenas prácticas, **todas las clases son cerradas (`final`) por defecto**. Esto significa que:
- **No puedes heredarlas** directamente a menos que se indique lo contrario.
- **Previene errores accidentales** donde una clase hija modifica sin querer el comportamiento de la clase padre.
- **Mejora el rendimiento** del compilador al saber exactamente qué clases no van a cambiar.

Por lo tanto, para permitir que una clase sea heredada o que un método sea sobrescrito, debemos usar la palabra clave **`open`**:

* **`open class`:** Indica que la clase puede ser usada como clase padre.
* **`open fun`:** Indica que un método dentro de una clase `open` puede ser sobrescrito por las clases hijas.

> **Analogía:** Piensa en la relación **"Es un"**. Un `Carro` **es un** `Vehiculo`. Un `Gato` **es un** `Animal`.


+++tabs
---[tab title="Jerarquía de Clases" lang="kotlin"]---
// Clase Padre (Base)
open class Empleado(val nombre: String, val salarioBase: Double) {
    // Marcamos el método con open para permitir que los hijos lo modifiquen
    open fun calcularSalario(): Double {
        return salarioBase
    }
}

// Clase Hija (Derivada)
class Desarrollador(nombre: String, salarioBase: Double, val lenguaje: String) : Empleado(nombre, salarioBase) {
    // Sobrescribimos el comportamiento base usando override
    override fun calcularSalario(): Double {
        val bono = 500.0
        return salarioBase + bono
    }
}

// Otra Clase Hija
class Gerente(nombre: String, salarioBase: Double) : Empleado(nombre, salarioBase) {
    override fun calcularSalario(): Double {
        val bonoPorMetas = 1200.0
        return salarioBase + bonoPorMetas
    }
}
---[tab title="Uso de Herencia" lang="kotlin"]---
fun main() {
    val dev = Desarrollador("Sofía Ruiz", 3000.0, "Kotlin")
    val jefe = Gerente("Andrés Silva", 5000.0)

    println("Salario de ${dev.nombre} (Dev ${dev.lenguaje}): $${dev.calcularSalario()}")
    println("Salario de ${jefe.nombre} (Gerente): $${jefe.calcularSalario()}")
}
+++

---

### D. Polimorfismo
El **Polimorfismo** significa "muchas formas". Es la habilidad que tienen los objetos de responder de manera diferente ante el mismo mensaje o llamada. Permite tratar a objetos de distintas clases derivadas como si fueran instancias de su clase padre.

> **Analogía:** Piensa en el botón de **"Encendido/Apagado" (Power)** de un control remoto universal. Si presionas el botón apuntando al televisor, se enciende la pantalla. Si apuntas al aire acondicionado, se activa la ventilación. El mismo botón (método) genera resultados diferentes según el aparato (objeto).

+++tabs
---[tab title="Uso Polimórfico" lang="kotlin"]---
// Clase Base
open class Notificacion {
    open fun enviar(mensaje: String) {
        println("Enviando mensaje genérico: $mensaje")
    }
}

// Subclases con implementaciones únicas
class NotificacionEmail : Notificacion() {
    override fun enviar(mensaje: String) {
        println("📧 Enviando correo electrónico con el texto: '$mensaje'")
    }
}

class NotificacionSMS : Notificacion() {
    override fun enviar(mensaje: String) {
        println("📱 Enviando SMS al celular con el texto: '$mensaje'")
    }
}

class NotificacionPush : Notificacion() {
    override fun enviar(mensaje: String) {
        println("🔔 Notificación Push en pantalla: '$mensaje'")
    }
}
---[tab title="Flexibilidad de Código" lang="kotlin"]---
// Esta función acepta CUALQUIER tipo de Notificación (padre o hijo)
fun alertarUsuario(canal: Notificacion, texto: String) {
    // Polimorfismo en acción: canal.enviar se comporta diferente según el objeto real
    canal.enviar(texto)
}

fun main() {
    val listaDeCanales: List<Notificacion> = listOf(
        NotificacionEmail(),
        NotificacionSMS(),
        NotificacionPush()
    )

    // Recorremos la lista y todos usan el mismo método, pero actúan diferente
    for (canal in listaDeCanales) {
        alertarUsuario(canal, "Tu código de verificación es 4589")
    }
}
+++

---

## 4. Clases Abstractas e Interfaces

Cuando queremos definir plantillas o contratos para que otras clases los sigan, usamos **Clases Abstractas** e **Interfaces**.

+++comparison-table
---
headers:
  - "Característica"
  - "Clase Abstracta"
  - "Interfaz"
rows:
  - ["Constructor", "✅ Sí puede tener.", "❌ No puede tener."]
  - ["Herencia", "Una clase solo puede heredar de UNA sola.", "Una clase puede implementar MÚLTIPLES interfaces."]
  - ["Propósito", "Define una identidad de objeto ('Es un...').", "Define una habilidad o contrato ('Hace un...')."]
---
+++

+++tabs
---[tab title="Código de Interfaces" lang="kotlin"]---
// Una interfaz define un contrato de habilidades
interface Volador {
    fun volar()
}

interface Nadador {
    fun nadar()
}

// Un Pato puede hacer ambas cosas
class Pato : Volador, Nadador {
    override fun volar() = println("El pato vuela bajo.")
    override fun nadar() = println("El pato nada en el lago.")
}
+++

---

## 5. Clases Especiales en Kotlin

Kotlin simplifica la POO añadiendo clases con propósitos específicos que ahorran mucho código repetitivo.

+++tabs
---[tab title="Data Classes" lang="kotlin"]---
// Perfectas para guardar datos. Crean automáticamente equals(), hashCode(), toString() y copy()
data class Usuario(val id: Int, val email: String)

fun main() {
    val user1 = Usuario(1, "test@test.com")
    val user2 = user1.copy(id = 2) // Copia las propiedades cambiando solo el id
    println(user1) // Salida: Usuario(id=1, email=test@test.com)
}
---[tab title="Enum & Sealed Classes" lang="kotlin"]---
// Enum: Lista fija de valores
enum class DiaSemana { LUNES, MARTES, MIERCOLES, JUEVES, VIERNES }

// Sealed Class: Jerarquía de clases restringida (ideal para estados de UI en Android)
sealed class EstadoDescarga {
    object Cargando : EstadoDescarga()
    data class Exito(val data: String) : EstadoDescarga()
    data class Error(val mensaje: String) : EstadoDescarga()
}
---[tab title="Objetos Singleton (object)" lang="kotlin"]---
// Crea una única instancia en toda la aplicación (Patrón Singleton)
object BaseDeDatosConfig {
    val url = "jdbc:mysql://localhost:3306/mi_app"
    fun conectar() = println("Conectando a $url")
}

fun main() {
    BaseDeDatosConfig.conectar()
}
+++

---

+++admonition
---
type: success
title: "Consejo para Android"
---
En el desarrollo moderno con Jetpack Compose, las **Data Classes** se usan para modelar el estado de las pantallas, y las **Sealed Classes** para representar los distintos estados de la carga de datos (Cargando, Éxito, Error). ¡Domínalas bien!
+++
