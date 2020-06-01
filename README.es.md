**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Las preguntas y respuestas más frecuentes en una entrevista sobre Kotlin</h2>

<details>
<summary>1. ¿Qué es Kotlin y cómo interactúa con Java?</summary>

#### Kotlin

Kotlin es un lenguaje de programación de tipado estático desarrollado por
JetBrains. Es compatible con la JVM, Android, JavaScript, WebAssembly y
plataformas nativas.

En la JVM, Kotlin se compila a bytecode de la JVM, por lo que es totalmente
compatible con Java:

- Kotlin puede invocar directamente código Java y utilizar bibliotecas de Java.
- Java puede invocar código Kotlin como clases y métodos habituales de la JVM.
- Ambos lenguajes pueden utilizarse en un mismo proyecto y módulo.
- Las anotaciones `@JvmStatic`, `@JvmOverloads`, `@JvmField` y `@JvmName`
  facilitan la creación de una API cómoda para Java.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

Un aspecto importante son los tipos de plataforma: si una API de Java no
incluye anotaciones correctas de nulabilidad, Kotlin no siempre puede determinar
si se admite `null`. Por ello, se necesitan contratos explícitos en la frontera
entre Java y Kotlin.

**En resumen:** Kotlin es compatible con Java en el nivel de la JVM y permite
incorporarlo gradualmente a un proyecto Java existente sin tener que
reescribirlo por completo.

</details>

<details>
<summary>2. ¿Cuáles son los tipos de datos básicos de Kotlin?</summary>

#### Kotlin

Los principales tipos de Kotlin son:

- números enteros: `Byte`, `Short`, `Int`, `Long`;
- números enteros sin signo: `UByte`, `UShort`, `UInt`, `ULong`;
- números de punto flotante: `Float`, `Double`;
- tipo lógico: `Boolean`;
- caracteres y cadenas: `Char`, `String`;
- matrices: `Array<T>`, `IntArray`, `LongArray`, entre otras.

También son importantes los siguientes tipos del sistema:

- `Any`: tipo base de todos los tipos que no admiten valores nulos;
- `Unit`: indica que una función no devuelve un resultado significativo;
- `Nothing`: indica que una función nunca finaliza normalmente;
- los tipos que admiten valores nulos se marcan con `?`, por ejemplo, `Int?`.

En la JVM, tipos como `Int` pueden compilarse como primitivos de Java. Los tipos
que admiten valores nulos y los genéricos suelen requerir boxing.

- Kotlin no realiza una ampliación implícita de los tipos numéricos: un `Int` no
  se convierte automáticamente en `Long`:

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**En resumen:** Kotlin no utiliza una sintaxis distinta para los tipos primitivos
y los tipos de objeto, pero el compilador de la JVM emplea primitivos siempre que
es posible.

</details>

<details>
<summary>3. ¿Cuál es la diferencia entre val y var en Kotlin?</summary>

#### Kotlin

`val` impide una nueva asignación, mientras que `var` la permite:

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Error de compilación
```

`val` no hace inmutable al objeto en sí; únicamente hace inmutable la referencia:

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Permitido
```

En las propiedades de una clase, `val` solo dispone de un getter, mientras que
`var` dispone de getter y setter.

**En resumen:** utiliza `val` de manera predeterminada y recurre a `var`
únicamente cuando el valor realmente deba cambiar.

</details>

<details>
<summary>4. ¿Qué es la inferencia de tipos en Kotlin?</summary>

#### Kotlin

La inferencia de tipos es la capacidad del compilador para determinar un tipo a
partir de una expresión o del contexto sin que se indique explícitamente.
Kotlin sigue siendo un lenguaje de tipado estático.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it es String
```

Conviene indicar un tipo explícito cuando:

- forma parte de una API pública;
- mejora la legibilidad de una expresión compleja;
- debe diferenciarse del tipo de una implementación concreta.

```kotlin
val users: List<User> = mutableListOf()
```

**En resumen:** el compilador infiere automáticamente los tipos evidentes; los
tipos explícitos son necesarios para definir contratos y mejorar la legibilidad.

</details>

<details>
<summary>5. ¿Qué son las funciones de extensión?</summary>

#### Kotlin

Una función de extensión añade una sintaxis de llamada práctica a un tipo
existente sin modificar la clase ni recurrir a la herencia.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` es el tipo receptor. Dentro de la función, su objeto está disponible
mediante `this`.

Limitaciones importantes:

- una extensión no tiene acceso a los miembros `private` ni `protected` de la clase;
- un método de la clase tiene prioridad sobre una extensión con la misma firma;
- la extensión se selecciona estáticamente según el tipo declarado de la variable.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

En la JVM, una extensión de este tipo suele compilarse como una función estática
a la que se pasa el objeto como primer argumento.

**En resumen:** las funciones de extensión mejoran la legibilidad de una API,
pero no añaden métodos reales a la clase ni admiten sobrescritura polimórfica.

</details>

<details>
<summary>6. ¿Qué es una data class en Kotlin?</summary>

#### Kotlin

Una `data class` está destinada a modelos cuya función principal es almacenar
datos. El compilador genera:

- `equals()` y `hashCode()`;
- `toString()`;
- `copy()`;
- `componentN()` para la desestructuración.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Reglas importantes:

- el constructor primario debe contener al menos un parámetro `val` o `var`;
- una `data class` no puede ser `abstract`, `open`, `sealed` ni `inner`;
- solo las propiedades del constructor primario participan en los métodos generados;
- `copy()` realiza una copia superficial, no una copia profunda.

Una propiedad declarada en el cuerpo de la clase no se tiene en cuenta en
`equals()`, `hashCode()` ni `copy()`:

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**En resumen:** una `data class` reduce el código repetitivo de DTO, estados de
la interfaz y objetos de valor. La lógica de negocio compleja debe permanecer en
servicios de dominio o clases convencionales.

</details>

<details>
<summary>7. ¿Qué es un companion object?</summary>

#### Kotlin

Un `companion object` es un único objeto asociado a una clase. Sus miembros
pueden invocarse mediante el nombre de la clase:

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

No equivale a `static` de Java: el companion object tiene su propio tipo, puede
tener nombre, implementar interfaces y acceder a miembros `private` de la clase.
Una clase solo puede tener un companion object.

Para facilitar la llamada de una función desde Java se utiliza `@JvmStatic`:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Sin la anotación, Java invoca `Config.Companion.default()`; con ella,
`Config.default()`.

**En resumen:** un companion object es apropiado para métodos de fábrica y
constantes asociados a una clase. No conviene almacenar en él estado global
mutable.

</details>

<details>
<summary>8. ¿Cómo se crean las clases en Kotlin?</summary>

#### Kotlin

Una clase se declara con la palabra clave `class`. El constructor primario se
escribe en el encabezado de la clase:

```kotlin
class User(
    val id: Long,
    var name: String = "Unknown"
) {
    init {
        require(name.isNotBlank())
    }

    fun rename(newName: String) {
        name = newName.trim()
    }
}

val user = User(id = 1, name = "Ivan")
```

- `val` y `var` en el constructor crean propiedades de la clase.
- `init` se ejecuta al crear el objeto y es apropiado para validar invariantes.
- Los valores predeterminados evitan crear constructores sobrecargados.
- Un objeto se crea sin la palabra clave `new`.

Las clases y sus métodos son `final` de manera predeterminada. Para permitir la
herencia se utiliza `open`:

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**En resumen:** Kotlin permite declarar el constructor, las propiedades y los
valores predeterminados directamente en el encabezado de una clase; la herencia
debe habilitarse explícitamente.

</details>

<details>
<summary>9. Explique los constructores primarios y secundarios.</summary>

#### Kotlin

El constructor primario se declara en el encabezado de la clase:

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Sus parámetros solo se convierten en propiedades cuando se declaran con `val` o
`var`. El cuerpo del constructor primario se define mediante `init`:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

Un constructor secundario se declara en el cuerpo de la clase mediante
`constructor`. Si existe un constructor primario, el secundario debe delegar en
él mediante `this(...)`:

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Las propiedades y los bloques `init` se ejecutan en el orden en que se
declaran, y el cuerpo del constructor secundario se ejecuta después.

Si la única diferencia es un valor predeterminado, no se necesita otro
constructor:

```kotlin
class User(val name: String, val id: Long = 0)
```

**En resumen:** el constructor primario es la opción principal. Los secundarios
se necesitan sobre todo para la compatibilidad con Java o por exigencias de un
framework; normalmente bastan parámetros predeterminados o un método de fábrica.

</details>

<details>
<summary>10. ¿Cómo funciona la herencia en Kotlin?</summary>

#### Kotlin

Las clases y sus miembros son `final` de manera predeterminada en Kotlin. Para
permitir la herencia se utiliza `open` y para sobrescribir un miembro,
`override`:

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

También pueden sobrescribirse propiedades:

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Aspectos importantes:

- una clase solo puede heredar de una clase, pero puede implementar varias interfaces;
- los miembros `abstract` ya están abiertos y no necesitan `open`;
- un miembro sobrescrito sigue abierto, salvo que se indique `final override`;
- se accede a la implementación de la clase padre mediante `super`.

**En resumen:** la herencia debe habilitarse explícitamente en Kotlin. Para los
contratos conviene utilizar interfaces, y normalmente debe considerarse la
composición antes de crear una jerarquía profunda de clases.

</details>

<details>
<summary>11. ¿Cómo se implementa un singleton en Kotlin?</summary>

#### Kotlin

La forma estándar de implementar un singleton en Kotlin es mediante una
declaración `object`:

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

En la JVM, el objeto se crea al acceder a él por primera vez y su inicialización
es segura para múltiples hilos. Un `object` puede contener propiedades,
funciones y bloques `init`, heredar de una clase e implementar interfaces. No
se puede invocar su constructor.

`object` es apropiado para constantes, estrategias sin estado y casos de una
jerarquía sealed. Para servicios con dependencias es preferible utilizar un
scope singleton en un contenedor de inyección de dependencias.

El estado mutable de un `object` es global, dificulta las pruebas y requiere
sincronización. En Android no deben almacenarse en él una `Activity` ni una
`View`, ya que pueden provocar una fuga de memoria.

**En resumen:** `object` proporciona un singleton sencillo sin parámetros de
constructor. Para servicios de negocio con dependencias y un ciclo de vida
administrado es preferible utilizar inyección de dependencias.

</details>

<details>
<summary>12. ¿Para qué se utiliza el tipo Unit?</summary>

#### Kotlin

`Unit` indica que una función finaliza sin devolver un resultado significativo:

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

En una función convencional puede omitirse `: Unit`:

```kotlin
fun log(message: String) {
    println(message)
}
```

A diferencia de `void` en Java, `Unit` es un tipo completo con un único valor,
también llamado `Unit`. Se utiliza en tipos de función y API genéricas:

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

No debe confundirse con `Nothing`: `Unit` indica una finalización normal sin
resultado, mientras que `Nothing` indica que una función nunca finaliza
normalmente:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**En resumen:** `Unit` es el tipo de las funciones que no devuelven un resultado
significativo. Normalmente se escribe explícitamente en tipos de callback, por
ejemplo, `() -> Unit`.

</details>

<details>
<summary>13. ¿Qué es un smart cast?</summary>

#### Kotlin

Un smart cast es una conversión automática a un tipo más específico cuando el
compilador puede demostrar que es segura:

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value tiene el tipo String
    }
}
```

El mecanismo funciona después de comprobaciones con `is`, `!is` y `null`, y
tiene en cuenta el flujo de ejecución:

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value tiene el tipo String
}
```

El smart cast no funciona si el valor puede cambiar entre la comprobación y su
uso. Un caso habitual es una propiedad mutable o un getter personalizado:

```kotlin
class UserHolder {
    var name: String? = null

    fun printName() {
        val currentName = name
        if (currentName != null) {
            println(currentName.length)
        }
    }
}
```

**En resumen:** un smart cast elimina la necesidad de utilizar `as`
explícitamente después de comprobar el tipo o `null`, pero solo cuando el
compilador garantiza que el valor comprobado no puede cambiar.

</details>

<details>
<summary>14. ¿Cómo garantiza Kotlin la seguridad frente a valores nulos?</summary>

#### Kotlin

En Kotlin, los tipos que admiten valores nulos y los que no los admiten están
separados en el sistema de tipos:

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

Para trabajar con valores que pueden ser nulos se utiliza:

```kotlin
val nullableLength: Int? = optionalName?.length // llamada segura
val length: Int = optionalName?.length ?: 0     // operador Elvis
```

Después de comprobar que un valor no es `null`, el compilador puede realizar un
smart cast:

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

El operador `!!` convierte un valor nullable en uno no nullable, pero lanza una
`NullPointerException` si el valor es `null`:

```kotlin
val length = optionalName!!.length
```

El principal punto débil son los tipos de plataforma de Java: sin anotaciones de
nulabilidad, el compilador desconoce si una API de Java puede devolver `null`.
Estos valores deben comprobarse en la frontera entre Java y Kotlin.

**En resumen:** `T` no admite `null`, mientras que `T?` sí. Utiliza `?.`, `?:` y
comprobaciones; reserva `!!` únicamente para situaciones en las que la garantía
de que el valor no es nulo esté demostrada.

</details>

<details>
<summary>15. ¿Qué es el operador Elvis (?:)?</summary>

#### Kotlin

El operador Elvis `?:` devuelve el operando izquierdo cuando no es `null`; en
caso contrario, evalúa y devuelve el operando derecho:

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

Suele utilizarse junto con una llamada segura:

```kotlin
val nameLength = user?.name?.length ?: 0
```

Como `return` y `throw` son expresiones en Kotlin, también pueden aparecer a la
derecha:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**En resumen:** `?:` define un valor alternativo para `null`. No debe utilizarse
para ocultar errores cuando `null` representa un estado incorrecto; en ese caso
es preferible finalizar la ejecución explícitamente.

</details>

<details>
<summary>16. ¿Qué hace el operador !! y por qué es peligroso?</summary>

#### Kotlin

El operador `!!` convierte forzosamente un `T?` en `T`. Si el valor es `null`,
se produce una `NullPointerException`:

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Las alternativas más seguras dependen del comportamiento requerido:

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

`requireNotNull` y `checkNotNull` también interrumpen la ejecución con un error,
pero describen explícitamente el contrato incumplido. El uso frecuente de `!!`
suele indicar un modelo incorrecto del estado nullable o problemas con el ciclo
de vida.

**En resumen:** utiliza `!!` únicamente cuando el contrato garantiza que el
valor no es nulo, pero el compilador no puede demostrarlo. En los demás casos,
trata `null` explícitamente.

</details>

<details>
<summary>17. ¿Cómo funcionan los parámetros predeterminados y los parámetros con nombre?</summary>

#### Kotlin

Se utiliza un parámetro predeterminado cuando no se proporciona el argumento
correspondiente:

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Los argumentos con nombre mejoran la legibilidad, especialmente cuando hay
varios parámetros del mismo tipo o de tipo `Boolean`:

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java no admite los parámetros predeterminados ni los argumentos con nombre de
Kotlin. Para una API de Java se pueden generar sobrecargas mediante
`@JvmOverloads`:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Los nombres de los parámetros de una función pública forman parte de la API de
Kotlin: cambiarlos puede romper las llamadas que utilizan argumentos con nombre.
Si hay demasiados parámetros, es preferible crear una clase de configuración
independiente.

**En resumen:** los valores predeterminados sustituyen a la mayoría de las
sobrecargas, mientras que los argumentos con nombre hacen que las llamadas sean
más claras. Para Java, ten en cuenta `@JvmOverloads`.

</details>

<details>
<summary>18. ¿Qué son las declaraciones de desestructuración?</summary>

#### Kotlin

Una declaración de desestructuración descompone un objeto en variables mediante
las funciones `componentN()`:

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

El código equivalente es:

```kotlin
val id = user.component1()
val name = user.component2()
```

En una `data class`, estas funciones se generan según el orden de las
propiedades del constructor primario. La desestructuración también funciona en
bucles y con `Map.Entry`:

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Los componentes innecesarios se pueden omitir mediante `_`:

```kotlin
val (_, name) = user
```

Para una clase ordinaria se pueden definir funciones propias
`operator fun componentN()`. Sin embargo, la desestructuración depende del orden
de los componentes, por lo que puede reducir la legibilidad en modelos grandes.

**En resumen:** la desestructuración es una sintaxis práctica basada en
`componentN()` para modelos pequeños y pares de valores. El orden de los
componentes forma parte del contrato.

</details>

<details>
<summary>19. ¿Qué es un alias de tipo y cuándo debe utilizarse?</summary>

#### Kotlin

`typealias` crea un nombre más corto o comprensible para un tipo existente:

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Es importante tener en cuenta que un alias no crea un tipo nuevo ni aporta
seguridad de tipos adicional:

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Válido, ya que ambos tipos son Long
```

Si el compilador debe distinguir entre los valores, se necesita una
`value class`:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**En resumen:** `typealias` se utiliza para mejorar la legibilidad de tipos
genéricos y tipos de función complejos. Para representar un tipo de dominio
independiente, utiliza una `value class`.

</details>

<details>
<summary>20. ¿Cuál es la diferencia entre vararg y un array?</summary>

#### Kotlin

Un array es un objeto con una cantidad fija de elementos. `vararg` es un
parámetro de función que permite pasar una cantidad variable de argumentos:

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

Dentro de la función, `names` es un array. Un array ya existente se pasa mediante
el operador de expansión `*`:

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

Si hay otro parámetro después de `vararg`, debe pasarse por su nombre:

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

Una función solo puede tener un parámetro `vararg`. Para `vararg Int`, el
compilador utiliza `IntArray`. El operador de expansión puede crear una copia
del array, algo importante en código crítico para el rendimiento.

**En resumen:** `Array<T>` es una estructura de datos, mientras que `vararg` es
una forma de aceptar una cantidad arbitraria de argumentos. Un array se pasa a
un parámetro `vararg` como `*array`.

</details>

<details>
<summary>21. ¿Cómo se utilizan las expresiones lambda en Kotlin?</summary>

#### Kotlin

Una lambda es una función anónima que puede almacenarse en una variable, pasarse
como argumento y devolverse desde una función:

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

La última expresión constituye el resultado de la lambda. Si solo hay un
parámetro, puede representarse mediante `it`:

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

Si la lambda es el último argumento, puede colocarse fuera de los paréntesis:

```kotlin
button.setOnClickListener { println("Clicked") }
```

Una lambda puede capturar variables del ámbito externo:

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

Si la función necesaria ya existe, se puede utilizar una referencia mediante
`::`:

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**En resumen:** las lambdas permiten pasar comportamiento a `map`, `filter`,
callbacks y DSL. Es preferible extraer las lambdas largas o anidadas en
funciones con nombre y controlar la captura de estado mutable, especialmente en
código concurrente.

</details>

<details>
<summary>22. ¿Cómo funcionan las colecciones (List, Set, Map) en Kotlin?</summary>

#### Kotlin

Las principales colecciones de Kotlin son:

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- `List` conserva el orden, admite duplicados y permite el acceso por índice.
- `Set` almacena elementos únicos; la unicidad se determina mediante `equals()`
  y `hashCode()`.
- `Map` almacena pares clave-valor con claves únicas. `map[key]` devuelve un
  valor nullable, por lo que se utiliza `containsKey()` para comprobar la
  existencia de una clave.

Las colecciones disponen de interfaces de solo lectura (`List`, `Set`, `Map`) y
mutables (`MutableList`, `MutableSet`, `MutableMap`):

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Una colección de solo lectura no es necesariamente inmutable. Otra referencia
puede modificar el mismo objeto:

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Las operaciones `filter`, `map`, `sorted` y otras devuelven colecciones nuevas:

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Las cadenas de operaciones sobre `Iterable` pueden crear colecciones
intermedias. Para cadenas largas conviene evaluar el uso de `Sequence`, pero no
utilizarlo sin necesidad.

**En resumen:** es preferible exponer interfaces de solo lectura y mantener las
colecciones mutables como un detalle de implementación. Si se necesita una
inmutabilidad real, crea una copia o utiliza colecciones inmutables.

</details>

<details>
<summary>23. ¿Cómo se recorren las colecciones en Kotlin?</summary>

#### Kotlin

Para un recorrido convencional se utiliza `for`:

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Para realizar una operación breve con cada elemento resulta adecuado
`forEach`:

```kotlin
names.forEach { println(it) }
```

Si se necesita el índice:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

Un `Map` puede recorrerse cómodamente mediante desestructuración:

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Para transformar o agregar datos se utiliza el operador correspondiente:

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Para `break`, `continue` y un flujo de control complejo es preferible utilizar
`for`. Para realizar búsquedas conviene usar `firstOrNull`, `find`, `any` o
`none`, en lugar de implementar un bucle manual.

**En resumen:** utiliza `for` para controlar el bucle, `forEach` para efectos
secundarios breves y `map` o `filter` para crear un resultado. Elige el operador
que exprese la intención del código.

</details>

<details>
<summary>24. ¿Qué ocurre si se modifica una lista creada con listOf()?</summary>

#### Kotlin

`listOf()` devuelve `List<T>`, una interfaz de solo lectura que no contiene los
métodos `add`, `remove` ni `set`:

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Error de compilación
```

Sin embargo, `List` no garantiza una inmutabilidad completa. Otra referencia
mutable puede modificar la misma lista:

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Convertir el resultado de `listOf()` a `MutableList` mediante un cast es
peligroso: el código depende de una implementación concreta y puede fallar en
tiempo de ejecución.

Si se requieren modificaciones, crea una lista mutable o una copia:

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

Para adoptar un estilo inmutable, crea una lista nueva:

```kotlin
val updated = names + "Oksana"
```

**En resumen:** una lista creada mediante `listOf()` no puede modificarse a
través de la API de `List`. Para realizar cambios, utiliza `mutableListOf()` o
`toMutableList()`, no un cast peligroso.

</details>

<details>
<summary>25. ¿Qué ocurre al acceder a un índice fuera de los límites de una lista?</summary>

#### Kotlin

El acceso mediante `list[index]` invoca `get(index)`. Si el índice es negativo o
mayor o igual que `size`, se produce una `IndexOutOfBoundsException`:

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Las alternativas seguras son:

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

Para una lista potencialmente vacía existen funciones seguras específicas:

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**En resumen:** utiliza `list[index]` cuando la validez del índice sea un
invariante. Para índices externos u opcionales, utiliza `getOrNull`, `getOrElse`
o una comprobación mediante `indices`.

</details>

<details>
<summary>26. ¿Cómo funciona la interoperabilidad de Kotlin con Java?</summary>

#### Kotlin

Kotlin/JVM se compila en bytecode de la JVM, por lo que Kotlin y Java pueden
invocar directamente las clases y los métodos del otro lenguaje:

```kotlin
val list = java.util.ArrayList<String>()
list.add("Kotlin")
```

```kotlin
class UserService {
    fun loadUser(id: Long): String = "User $id"
}
```

```java
UserService service = new UserService();
String user = service.loadUser(1L);
```

El principal riesgo son los tipos de plataforma procedentes de Java, cuya
nullabilidad se desconoce. En el límite entre ambos lenguajes se deben utilizar
`@Nullable` y `@NotNull`, comprobar los valores y evitar que los tipos de
plataforma lleguen a las capas internas de la aplicación.

Para crear una API cómoda de utilizar desde Java se emplean:

- `@JvmStatic`: un método estático;
- `@JvmOverloads`: sobrecargas para los parámetros predeterminados;
- `@JvmField`: un campo sin getter ni setter;
- `@JvmName`: un nombre diferente en la JVM;
- `@Throws`: la declaración de excepciones comprobadas para Java.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Las interfaces SAM de Java pueden pasarse como lambdas:

```kotlin
executor.execute { println("Done") }
```

**En resumen:** la interoperabilidad permite una migración gradual entre Java y
Kotlin. Se debe prestar especial atención a la nullabilidad, las colecciones
mutables, los parámetros predeterminados y la facilidad de uso de la API desde
código Java.

</details>

<details>
<summary>27. ¿Se pueden utilizar anotaciones de Java en Kotlin?</summary>

#### Kotlin

Sí. Las anotaciones de Java pueden aplicarse a las clases de Kotlin y a sus
miembros:

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

Una propiedad de Kotlin puede estar representada en la JVM mediante un campo, un
getter, un setter y un parámetro del constructor. Para aplicar la anotación al
elemento adecuado se utiliza un objetivo de uso:

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Las principales opciones son:

- `@field:`: el campo de respaldo;
- `@get:` / `@set:`: el getter o el setter;
- `@param:`: el parámetro del constructor;
- `@property:`: la propiedad de Kotlin, que no constituye un elemento
  independiente en Java.

Las anotaciones de nullabilidad de Java ayudan a Kotlin a determinar el tipo del
resultado de una API de Java:

```java
@NotNull
String getName();
```

Sin una anotación correcta, el resultado puede seguir siendo un tipo de
plataforma. En los frameworks de reflexión o validación se debe comprobar qué
elemento de la JVM inspeccionan.

**En resumen:** las anotaciones de Java son compatibles de forma directa. El
principal matiz consiste en elegir correctamente `@field:`, `@get:` o `@param:`
para cada framework.

</details>

<details>
<summary>28. ¿Qué es Kotlin REPL?</summary>

#### Kotlin

REPL significa Read-Eval-Print Loop: el entorno lee el código introducido, lo
ejecuta, muestra el resultado y espera el siguiente comando.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

REPL resulta adecuado para comprobar rápidamente la sintaxis, la biblioteca
estándar y expresiones pequeñas sin crear un proyecto. Puede iniciarse mediante
la CLI de Kotlin; para experimentos más extensos son más prácticos los archivos
scratch o Kotlin Playground.

REPL no reproduce el ciclo de vida de Android, la inyección de dependencias, la
configuración de Gradle ni un entorno concurrente real, y tampoco sustituye las
pruebas automatizadas.

**En resumen:** Kotlin REPL es una consola interactiva para realizar experimentos
locales con el lenguaje. Los resultados relevantes para una aplicación deben
confirmarse mediante pruebas en un entorno real.

</details>

<details>
<summary>29. ¿Qué es un script de Kotlin (.kts)?</summary>

#### Kotlin

Un script de Kotlin es un archivo `.kts` en el que se pueden ejecutar
instrucciones de nivel superior sin `fun main()`:

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

Un archivo `.kt` es un archivo fuente convencional de un programa o una
biblioteca, mientras que un archivo `.kts` se ejecuta en un entorno de scripting.
El ejemplo más común es Gradle Kotlin DSL:

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

Gradle ejecuta los archivos `build.gradle.kts` y `settings.gradle.kts` como
scripts de configuración. Los scripts independientes pueden utilizarse para
pequeñas automatizaciones, pero su modelo de dependencias y su API dependen del
host de scripting.

**En resumen:** `.kts` resulta adecuado para tareas de configuración y
automatizaciones breves. Si la lógica crece, tiene dependencias y requiere
pruebas, es preferible trasladarla a un módulo convencional de Kotlin o a una
aplicación CLI.

</details>

<details>
<summary>30. ¿Qué es la guía de estilo de Kotlin y por qué conviene seguirla?</summary>

#### Kotlin

La guía de estilo de Kotlin define las reglas de formato, nomenclatura,
estructura de archivos y organización de las importaciones. Hace que el código
del equipo sea uniforme y predecible.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Sus beneficios prácticos son:

- menos discusiones sobre el formato durante las revisiones de código;
- una lectura más rápida del código y una incorporación más sencilla de nuevos
  desarrolladores;
- comprobaciones automáticas mediante el formateador del IDE, `ktlint` o
  `detekt` en CI.

Las desviaciones del equipo respecto a las reglas oficiales deben documentarse y
configurarse de forma coherente tanto en el IDE como en CI. La guía de estilo
regula la presentación del código, pero no sustituye las reglas arquitectónicas.

**En resumen:** la guía de estilo reduce la carga cognitiva y transfiere la
comprobación del formato de las personas a las herramientas automatizadas.

</details>

<details>
<summary>31. ¿Qué son los modismos de Kotlin?</summary>

#### Kotlin

Los modismos de Kotlin son formas consolidadas de utilizar las características
del lenguaje para que el código exprese claramente su intención.

Algunos ejemplos habituales son:

- utilizar `val` de forma predeterminada y `var` solo cuando la mutación sea
  necesaria;
- emplear tipos nullable, `?.` y `?:` en lugar de `!!`;
- usar `data class` para los modelos de datos;
- utilizar tipos `sealed` para un conjunto limitado de estados;
- emplear `map`, `filter` y `associateBy` en lugar de bucles manuales para las
  transformaciones;
- utilizar cláusulas de guarda para reducir el anidamiento.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Las funciones de ámbito, las lambdas y la sobrecarga de operadores solo son
adecuadas cuando mejoran la legibilidad. Utilizar más características del
lenguaje no convierte por sí solo el código en idiomático.

**En resumen:** el Kotlin idiomático es código seguro y comprensible que utiliza
las características del lenguaje para su propósito, no para exhibir la sintaxis.

</details>

<details>
<summary>32. ¿Cómo se utiliza Kotlin en el desarrollo backend y por qué se elige en lugar de Java?</summary>

#### Kotlin

Kotlin se utiliza para backends de la JVM: API REST/gRPC, microservicios,
procesos de trabajo y sistemas orientados a eventos. Es compatible con Spring
Boot, Ktor y las bibliotecas de Java.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Sus principales ventajas frente a Java son:

- la seguridad frente a valores nulos;
- menos código repetitivo gracias a `data class`, los parámetros predeterminados
  y las funciones de extensión;
- corrutinas para operaciones de E/S no bloqueantes;
- migración gradual y acceso completo al ecosistema de Java.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Entre las contrapartidas se encuentran una compilación más lenta,
configuraciones adicionales del compilador o de Gradle para algunos frameworks
de Java y la necesidad de formar al equipo en Kotlin idiomático y concurrencia
estructurada.

**En resumen:** conviene elegir Kotlin cuando el equipo desea código JVM más
seguro y conciso sin renunciar al ecosistema de Java, y está dispuesto a aceptar
una cadena de herramientas más compleja.

</details>

<details>
<summary>33. ¿Qué son las funciones de orden superior (higher-order functions)?</summary>

#### Kotlin

Una función de orden superior recibe una función como argumento o la devuelve
como resultado.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` es una función sin parámetros y sin un resultado
significativo. Un ejemplo de una función que devuelve otra función:

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

Algunos ejemplos habituales de la biblioteca estándar:

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Estas funciones se utilizan con colecciones, callbacks y DSL. Las lambdas pueden
crear objetos y capturar estado externo; `inline` suele eliminar este coste
adicional en funciones de orden superior pequeñas.

**En resumen:** las funciones de orden superior permiten pasar comportamiento
como un valor. Es preferible extraer las lambdas complejas o anidadas en
funciones con nombre.

</details>

<details>
<summary>34. ¿Qué son las funciones inline y cuándo deben utilizarse?</summary>

#### Kotlin

`inline` solicita al compilador que inserte el cuerpo de la función y sus
parámetros lambda en el lugar de la llamada. Su objetivo principal es reducir el
coste adicional de las funciones de orden superior:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` también permite utilizar parámetros de tipo `reified`:

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Modificadores adicionales para los parámetros lambda:

- `noinline`: no inserta la lambda, por lo que puede almacenarse o pasarse a otra
  función;
- `crossinline`: inserta la lambda, pero prohíbe un `return` no local.

`inline` resulta adecuado para funciones de orden superior pequeñas, genéricos
`reified` y rutas críticas cuyo rendimiento se haya medido. En funciones grandes
puede aumentar el bytecode. Los cambios en funciones `inline` públicas también
requieren prestar atención a la compatibilidad binaria.

**En resumen:** no utilices `inline` como una optimización universal, sino para
reducir el coste de las lambdas, emplear `reified` o conseguir la semántica
necesaria de un retorno no local.

</details>

<details>
<summary>35. ¿Qué significa la palabra clave reified y dónde se utiliza?</summary>

#### Kotlin

Debido al borrado de tipos, un parámetro genérico convencional `T` no puede
utilizarse para comprobar un tipo ni para obtener `T::class`:

```kotlin
// fun <T> isOfType(value: Any) = value is T // Error de compilación
```

`reified` permite hacerlo dentro de una función `inline`, ya que el compilador
inserta el tipo concreto en el lugar de la llamada:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

También es posible obtener información sobre la clase:

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

Un ejemplo de la biblioteca estándar:

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

Limitaciones: `reified` solo está disponible para los parámetros de tipo de
funciones `inline` y no recupera información sobre argumentos genéricos anidados
como los de `List<String>`. Para una API que no sea `inline`, se debe pasar
explícitamente un `KClass`, un `Class` o un serializador.

**En resumen:** `reified` proporciona a una función `inline` acceso al tipo
concreto `T` para utilizar `is`, `T::class`, reflexión y API similares, pero no
elimina por completo el borrado de tipos.

</details>

<details>
<summary>36. ¿Qué son las clases y las interfaces sealed?</summary>

#### Kotlin

`sealed class` y `sealed interface` definen una jerarquía controlada de subtipos
directos. Son apropiadas para representar estados, resultados y comandos:

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

El compilador comprueba que una expresión `when` contemple todas las variantes:

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Los subtipos directos deben declararse en el mismo módulo y paquete. Una
`sealed class` puede tener un constructor, estado y una implementación común.
Una `sealed interface` no tiene constructor, pero una clase puede implementar
varias interfaces.

A diferencia de un `enum`, cada variante `sealed` puede contener su propio
conjunto de datos. `sealed` no resulta adecuado si la jerarquía debe ser
ampliada por módulos externos. Es preferible no añadir `else` a un `when` sin
necesidad, para que el compilador detecte los casos nuevos.

**En resumen:** los tipos `sealed` modelan un conjunto cerrado de variantes con
datos diferentes y permiten comprobar durante la compilación que un `when` sea
exhaustivo.

</details>

<details>
<summary>37. ¿Qué es una expresión de objeto y cuándo se utiliza?</summary>

#### Kotlin

Una `object expression` crea un objeto anónimo en tiempo de ejecución. Por lo
general, se utiliza para implementar una interfaz o una clase una sola vez:

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

También se puede heredar de una clase:

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

Un objeto anónimo sin supertipo solo resulta práctico localmente o en una API
`private`:

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Su tipo anónimo no puede exportarse como un tipo público estable: desde el
exterior solo será visible el supertipo declarado o `Any`. Una expresión de
objeto crea una instancia cada vez que se ejecuta, mientras que una declaración
de objeto constituye un singleton con nombre.

Este tipo de objeto puede capturar variables externas. Si la implementación
crece o se repite, es preferible extraerla en una clase con nombre.

**En resumen:** una expresión de objeto resulta adecuada para una implementación
local breve, un listener o un stub de pruebas. La lógica pública o reutilizable
requiere un tipo con nombre.

</details>

<details>
<summary>38. ¿Cuál es la diferencia entre las clases enum y las clases sealed?</summary>

#### Kotlin

Una `enum class` contiene un conjunto fijo de valores singleton del mismo tipo:

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

Una `sealed class` o una `sealed interface` define una jerarquía cerrada de
subtipos. Cada variante puede contener sus propios datos:

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

En ambos casos, el compilador comprueba que `when` contemple todas las variantes:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

Un `enum` proporciona los elementos estándar `entries`, `name` y `ordinal`. Un
tipo `sealed` no dispone de una lista predefinida de instancias, pero permite
crear numerosos objetos del mismo subtipo con datos diferentes.

**En resumen:** utiliza un `enum` para constantes sencillas con una estructura
común y un tipo `sealed` para un conjunto cerrado de estados o resultados con
datos diferentes.

</details>

<details>
<summary>39. ¿Cuál es la diferencia entre la inicialización con lateinit y lazy?</summary>

#### Kotlin

Ambos mecanismos posponen la inicialización, pero funcionan de manera diferente.

`lateinit` permite asignar un valor a una propiedad `var` no nullable después de
crear el objeto:

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Leer la propiedad antes de asignarle un valor produce una
`UninitializedPropertyAccessException`. `lateinit` solo funciona con propiedades
`var` no nullable de tipos de referencia, no garantiza la seguridad entre hilos
y permite realizar asignaciones repetidas.

Es posible comprobar si la propiedad está inicializada:

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` calcula el valor de una propiedad `val` durante la primera lectura y
almacena en caché el resultado obtenido correctamente:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

De forma predeterminada, `lazy` está sincronizado. Los modos `PUBLICATION` y
`NONE` modifican sus garantías; `NONE` solo es seguro cuando se accede desde un
único hilo.

- Si el código externo o la inyección de dependencias asigna el valor más tarde,
  utiliza `lateinit`.
- Si un inicializador propio debe ejecutarse cuando sea necesario, utiliza
  `lazy`.
- Si la ausencia de un valor es un estado válido, utiliza un tipo nullable.

**En resumen:** `lateinit` representa la asignación externa diferida de una
propiedad mutable; `lazy`, el cálculo diferido y único de una propiedad
inmutable.

</details>

<details>
<summary>40. ¿Qué es el tipo Nothing y cómo influye en el flujo de ejecución?</summary>

#### Kotlin

`Nothing` es un tipo que no posee ningún valor posible. Una función con este tipo
de resultado nunca termina de forma normal:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` es un subtipo de todos los tipos, por lo que puede utilizarse en
cualquier expresión donde se interrumpa la ejecución:

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` y `return` tienen el tipo `Nothing`, por lo que pueden formar parte de
una expresión Elvis:

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Este tipo ayuda al compilador a analizar el flujo de control y realizar
conversiones inteligentes:

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` tiene un único valor posible, `null`; este es el tipo de la expresión
`null` cuando no existe otro contexto. Las funciones estándar `error()` y
`TODO()` también devuelven `Nothing`.

**En resumen:** `Nothing` representa una ruta que no devuelve el control. Gracias
a ello, el compilador determina los tipos con mayor precisión y analiza el flujo
de ejecución posterior.

</details>

<details>
<summary>41. ¿Puede Java invocar funciones de Kotlin con parámetros predeterminados?</summary>

#### Kotlin

Java no admite los parámetros predeterminados de Kotlin. Si no existen
sobrecargas adicionales, debe proporcionar todos los argumentos:

```kotlin
class UserService {
    fun createUser(
        name: String,
        isActive: Boolean = true
    ) = Unit
}
```

```java
service.createUser("Ivan", true);
```

`@JvmOverloads` genera sobrecargas fáciles de utilizar desde Java:

```kotlin
class UserService {
    @JvmOverloads
    fun createUser(
        name: String,
        isActive: Boolean = true,
        role: String = "user"
    ) = Unit
}
```

```java
service.createUser("Ivan");
service.createUser("Ivan", false);
service.createUser("Ivan", false, "admin");
```

Las sobrecargas se generan de derecha a izquierda para los parámetros
consecutivos que tienen valores predeterminados. La anotación también funciona
con constructores:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

La anotación no es necesaria para una API utilizada únicamente desde Kotlin. Una
gran cantidad de parámetros genera demasiadas sobrecargas; en ese caso, es
preferible utilizar un objeto de configuración o un builder.

**En resumen:** sin `@JvmOverloads`, Java solo puede invocar la firma completa.
Añade la anotación únicamente a las API que realmente se utilicen desde Java.

</details>

<details>
<summary>42. ¿Qué son las funciones recursivas de cola?</summary>

#### Kotlin

Una función recursiva de cola se invoca a sí misma como su última operación. El
modificador `tailrec` permite que el compilador transforme dicha llamada en un
bucle sin aumentar la pila:

```kotlin
tailrec fun factorial(
    n: Int,
    accumulator: Long = 1L
): Long = if (n <= 1) {
    accumulator
} else {
    factorial(n - 1, accumulator * n)
}
```

Esta variante no es recursiva de cola, porque la multiplicación se realiza
después de la llamada recursiva:

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

La optimización solo funciona con recursión directa y no se aplica si la llamada
se encuentra en una construcción que impide la transformación, como
`try`/`finally`. Si `tailrec` se utiliza de forma incorrecta, el compilador
advierte que la optimización no se llevará a cabo.

**En resumen:** `tailrec` permite utilizar una recursión legible sin riesgo de
`StackOverflowError`. Si un bucle convencional resulta más sencillo, es
preferible utilizarlo.

</details>

<details>
<summary>43. ¿Qué son las funciones de ámbito (let, run, apply, also, with) y cuándo deben utilizarse?</summary>

#### Kotlin

Las funciones de ámbito ejecutan un bloque en el contexto de un objeto. Se
diferencian por dos características:

- cómo se accede al objeto: mediante `this` o `it`;
- qué devuelven: el resultado de la lambda o el propio objeto.

| Función | El objeto está disponible como | Devuelve               |
| ------- | ------------------------------ | ---------------------- |
| `let`   | `it`                           | resultado de la lambda |
| `run`   | `this`                         | resultado de la lambda |
| `apply` | `this`                         | el propio objeto       |
| `also`  | `it`                           | el propio objeto       |
| `with`  | `this`                         | resultado de la lambda |

Usos habituales:

- `let`: una transformación o un bloque después de una llamada segura;
- `run`: calcular un resultado accediendo al objeto mediante `this`;
- `apply`: configurar un objeto;
- `also`: realizar un efecto secundario sin modificar la cadena;
- `with`: agrupar operaciones sobre el objeto proporcionado.

```kotlin
val user = User().apply {
    name = "Ivan"
    isActive = true
}
```

```kotlin
val user = loadUser()
    .also { logger.info("Loaded user: ${it.id}") }
```

`with` no es una función de extensión: el objeto se pasa como primer argumento.
Las funciones de ámbito profundamente anidadas y la combinación de `this` e `it`
reducen la legibilidad; en ese caso, es preferible utilizar código convencional
o una función con nombre.

**En resumen:** elige una función de ámbito según el receptor (`this` o `it`) y
el resultado que devuelve (el objeto o el resultado de la lambda), no por
costumbre.

</details>

<details>
<summary>44. ¿Cuál es la diferencia entre let, run y apply?</summary>

#### Kotlin

Estas funciones se diferencian por la forma de acceder al objeto y por el
resultado que devuelven:

| Función | Receptor en el bloque | Devuelve               |
| ------- | --------------------- | ---------------------- |
| `let`   | `it`                  | resultado de la lambda |
| `run`   | `this`                | resultado de la lambda |
| `apply` | `this`                | el propio objeto       |

`let` se utiliza para realizar una transformación o ejecutar código después de
una llamada segura:

```kotlin
val length = name?.let { it.length }
```

`run` se utiliza para calcular un resultado mediante varios accesos al objeto:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`apply` se utiliza para configurar y devolver el mismo objeto:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

No utilices una función de ámbito si una llamada convencional resulta más
legible. Es preferible sustituir los bloques anidados con varios `this` o `it`
por una función con nombre.

**En resumen:** `let` utiliza `it` y devuelve un resultado nuevo; `run` utiliza
`this` y devuelve un resultado nuevo; `apply` utiliza `this` y devuelve el objeto
original.

</details>

<details>
<summary>45. ¿Cuál es la diferencia entre map y flatMap?</summary>

#### Kotlin

`map` transforma cada elemento en un resultado:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

Si la transformación devuelve una colección, `map` crea una estructura anidada:

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` realiza la transformación y combina las colecciones anidadas en una
sola:

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

Un ejemplo práctico:

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Conceptualmente, estas expresiones son equivalentes:

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**En resumen:** `map` realiza una transformación `A -> B`. `flatMap` realiza una
transformación `A -> Iterable<B>` y después combina todos los resultados en una
colección plana.

</details>

<details>
<summary>46. ¿Qué es Sequence y cuándo debe utilizarse?</summary>

#### Kotlin

`Sequence` es una secuencia síncrona y perezosa. Los operadores intermedios no
procesan los datos inmediatamente, sino que forman un pipeline:

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Una operación terminal inicia el cálculo: `toList`, `first`, `count`, `fold`,
`any`, entre otras. Los elementos atraviesan el pipeline uno a uno, por lo que
las operaciones de cortocircuito pueden detener el procesamiento antes:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

`Sequence` resulta adecuado para conjuntos grandes, cadenas extensas de
`map`/`filter` y operaciones de cortocircuito. Para una colección pequeña o una
única transformación, su coste adicional puede superar sus beneficios.
Operaciones como `sorted` siguen necesitando acumular los elementos.

`Sequence` no es asíncrono ni admite suspensión. Para un flujo de datos
asíncrono se utiliza `Flow`.

**En resumen:** `Sequence` reduce las colecciones intermedias y el trabajo
innecesario en pipelines síncronos largos, pero no es automáticamente más rápido
que las colecciones convencionales.

</details>

<details>
<summary>47. ¿Qué es el procesamiento perezoso (lazy) de colecciones?</summary>

#### Kotlin

El procesamiento perezoso significa que el pipeline solo se describe y no se
ejecuta hasta que se invoca una operación terminal. En Kotlin se utiliza
`Sequence` para este propósito:

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Por lo general, los elementos atraviesan los operadores uno a uno. Esto permite
finalizar el procesamiento antes:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Un pipeline perezoso no almacena el resultado en caché: una nueva operación
terminal suele volver a iniciar el cálculo. También permite trabajar con
secuencias potencialmente infinitas si se limitan mediante `take`:

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Los operadores perezosos pueden tener su propio coste adicional, y `sorted` y
otras operaciones similares siguen acumulando los datos. Para el procesamiento
asíncrono se necesita `Flow`, no `Sequence`.

**En resumen:** el procesamiento perezoso pospone el trabajo hasta una operación
terminal y puede evitar cálculos innecesarios. No almacena el resultado en caché
ni siempre es más rápido que las colecciones de evaluación inmediata.

</details>

<details>
<summary>48. ¿Qué son las corrutinas y en qué se diferencian de los hilos (threads)?</summary>

#### Kotlin

Una corrutina es una tarea ligera que puede suspender su ejecución y reanudarla
más tarde. No se crea un hilo del sistema operativo independiente para cada
corrutina:

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Un hilo es un recurso del sistema operativo con su propia pila. Muchas corrutinas
pueden ejecutarse en una cantidad reducida de hilos y cambiar de uno a otro
después de una suspensión.

```kotlin
delay(1_000)        // Suspende la corrutina
Thread.sleep(1_000) // Bloquea el hilo
```

`suspend` no garantiza por sí solo una implementación no bloqueante. Una API
bloqueante sigue bloqueando el hilo y debe ejecutarse en el contexto adecuado.

El dispatcher determina los hilos de ejecución:

- `Dispatchers.Main`: la interfaz de usuario;
- `Dispatchers.IO`: operaciones de E/S bloqueantes;
- `Dispatchers.Default`: trabajo intensivo de CPU.

La concurrencia estructurada vincula las corrutinas a un `CoroutineScope`: la
operación principal gestiona el ciclo de vida, los errores y la cancelación de
las tareas secundarias.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Las corrutinas no eliminan las condiciones de carrera ni aceleran
automáticamente el trabajo intensivo de CPU. Para los cálculos pesados se debe
seleccionar explícitamente el dispatcher adecuado:

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**En resumen:** las corrutinas son tareas ejecutadas sobre hilos que ofrecen
suspensión y concurrencia estructurada. Son más económicas que el modelo «una
tarea, un hilo», pero no sustituyen el uso correcto de los dispatchers ni la
gestión adecuada del estado compartido.

</details>

<details>
<summary>49. ¿Qué es una función suspend?</summary>

#### Kotlin

`suspend` identifica una función que puede contener puntos de suspensión. Esta
se invoca desde otra función `suspend` o desde una corrutina:

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` no crea una corrutina, no ejecuta la función en paralelo ni convierte
automáticamente el código bloqueante en código no bloqueante:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

El compilador transforma una función `suspend` en una máquina de estados con una
`Continuation`, lo que permite guardar el estado y reanudar la ejecución después
de una suspensión.

La cancelación es cooperativa: los puntos de suspensión estándar la comprueban,
mientras que los bucles de CPU prolongados deben hacerlo explícitamente:

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**En resumen:** `suspend` permite que una función suspenda y reanude una
corrutina. No garantiza una implementación no bloqueante ni crea una corrutina
nueva por sí mismo.

</details>

<details>
<summary>50. ¿Qué es CoroutineScope y para qué se necesita?</summary>

#### Kotlin

`CoroutineScope` es el propietario de las corrutinas y contiene un
`coroutineContext`. Por lo general, el contexto incluye un `Job`, un dispatcher
y otros elementos:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

`Job` establece las relaciones entre tareas principales y secundarias, y
gestiona la cancelación. Cancelar el ámbito cancela sus corrutinas secundarias:

```kotlin
scope.cancel()
```

Para realizar trabajo en paralelo dentro de una función `suspend` se utiliza un
constructor de ámbito:

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` no termina hasta que finalizan todas las tareas secundarias. En
Android, `viewModelScope`, `lifecycleScope` y `rememberCoroutineScope()` ya
disponen de un ciclo de vida definido.

Solo debe crearse un ámbito propio para un objeto con un método de finalización
explícito que invoque `cancel()`. `GlobalScope` no tiene dicho propietario y
vulnera la concurrencia estructurada, por lo que debe evitarse en el código de
una aplicación.

**En resumen:** `CoroutineScope` vincula las corrutinas a un ciclo de vida claro
y permite cancelarlas conjuntamente. Cada ámbito debe tener un propietario.

</details>

<details>
<summary>51. ¿Cuál es la diferencia entre launch y async?</summary>

#### Kotlin

Ambos constructores inician una corrutina secundaria en un `CoroutineScope`.

`launch` devuelve un `Job` y se utiliza cuando no se necesita un resultado
independiente:

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` devuelve un `Deferred<T>`. El resultado o el error se obtiene mediante
`await()`. Un caso de uso habitual es ejecutar operaciones independientes en
paralelo:

```kotlin
suspend fun loadScreen(): ScreenData = coroutineScope {
    val user = async { userRepository.loadUser() }
    val orders = async { orderRepository.loadOrders() }

    ScreenData(
        user = user.await(),
        orders = orders.await()
    )
}
```

De forma predeterminada, `async` se inicia inmediatamente, no al invocar
`await()`. Un error en una corrutina secundaria cancela el ámbito principal
convencional independientemente del constructor; en el caso de `async`,
`await()` también vuelve a lanzar ese error. El comportamiento de supervisión
debe configurarse por separado.

No utilices `async` si no tienes previsto invocar `await()`, ni ejecutes en
paralelo operaciones que dependan unas de otras.

**En resumen:** `launch` representa una tarea sin valor de resultado; `async`,
una tarea con un `Deferred<T>` cuyo resultado se obtiene mediante `await()`.

</details>

<details>
<summary>52. ¿Qué es runBlocking y cuándo se utiliza?</summary>

#### Kotlin

`runBlocking` crea un `CoroutineScope` y bloquea el hilo actual hasta que
finalizan el bloque y todas sus corrutinas secundarias:

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

Es una herramienta de frontera que permite invocar código `suspend` desde una
API síncrona. Puede ser adecuada como puente con código heredado o en un ejemplo
breve. En las pruebas de corrutinas debe utilizarse `runTest`.

En el código de la interfaz de usuario, `runBlocking` bloquea el hilo principal
y puede provocar un bloqueo de la aplicación, un ANR o un interbloqueo:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

En su lugar, la corrutina debe iniciarse en un ámbito con el ciclo de vida
adecuado:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

Dentro de una función `suspend`, `runBlocking` es innecesario y solo bloquea el
hilo:

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**En resumen:** `runBlocking` es un puente síncrono hacia el código `suspend`. No
lo utilices dentro de corrutinas, funciones `suspend` ni en el hilo de la
interfaz de usuario.

</details>

<details>
<summary>53. ¿Qué son los dispatchers (Dispatchers.IO, Default, Main)?</summary>

#### Kotlin

`CoroutineDispatcher` programa la ejecución de una corrutina en los hilos
adecuados:

- `Dispatchers.Main`: la interfaz de usuario y operaciones breves en el hilo
  principal;
- `Dispatchers.IO`: operaciones de E/S bloqueantes;
- `Dispatchers.Default`: cálculos intensivos de CPU.

Una operación de archivo bloqueante se traslada a `IO`:

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Un cálculo pesado se traslada a `Default`:

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` suspende la corrutina actual, ejecuta el bloque en el contexto
indicado y devuelve el resultado. Mantiene la concurrencia estructurada, la
cancelación y la propagación de errores.

No todas las llamadas de red o de base de datos necesitan `Dispatchers.IO`: una
API `suspend` puede ser ya segura para el hilo principal. El dispatcher debe
elegirlo la capa que sabe si la implementación bloquea el hilo. Para facilitar
las pruebas, el dispatcher puede proporcionarse como una dependencia:

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

No conviene cambiar de contexto para tareas pequeñas, ya que esta operación
también tiene un coste.

**En resumen:** `Main` se utiliza para la interfaz de usuario, `IO` para
operaciones de E/S bloqueantes y `Default` para cálculos pesados. `withContext`
cambia el contexto de una operación concreta sin crear una nueva corrutina
independiente.

</details>

<details>
<summary>54. ¿Cómo se cancelan las corrutinas y se gestionan los errores?</summary>

#### Kotlin

La cancelación de corrutinas es cooperativa y se gestiona mediante un `Job`:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Las funciones `suspend`, como `delay()`, responden automáticamente a la
cancelación. El código intensivo de CPU debe comprobar el estado periódicamente:

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

La cancelación se señala mediante una `CancellationException`. Si un bloque
`catch` genérico la intercepta, debe volver a lanzarse:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

Con la concurrencia estructurada convencional, un error no gestionado en una
corrutina secundaria cancela la tarea principal y las tareas hermanas. Los
errores esperados se gestionan localmente y se convierten en un resultado de
dominio o en un estado de la interfaz:

```kotlin
viewModelScope.launch {
    try {
        val user = repository.loadUser()
        state.value = UiState.Success(user)
    } catch (exception: CancellationException) {
        throw exception
    } catch (exception: IOException) {
        state.value = UiState.Error(exception.message)
    }
}
```

Para tareas independientes se utiliza `supervisorScope` o `SupervisorJob`:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

Un supervisor no gestiona el error, sino que únicamente aísla las tareas
hermanas. `CoroutineExceptionHandler` está destinado al último nivel de gestión
de excepciones no capturadas en un `launch` raíz, por ejemplo, para registrarlas.
Los errores de `async` se obtienen mediante `await()`.

**En resumen:** cancela mediante `Job`, no ignores una `CancellationException`,
gestiona localmente los errores esperados y aísla las tareas independientes
mediante supervisión.

</details>

<details>
<summary>55. ¿Qué es la concurrencia estructurada (structured concurrency)?</summary>

#### Kotlin

La concurrencia estructurada significa que cada corrutina tiene un propietario y
pertenece a una jerarquía de `Job`. La tarea principal espera a las tareas
secundarias y gestiona su cancelación y sus errores.

```kotlin
suspend fun loadScreen(): ScreenData = coroutineScope {
    val user = async { userRepository.loadUser() }
    val orders = async { orderRepository.loadOrders() }

    ScreenData(
        user = user.await(),
        orders = orders.await()
    )
}
```

`coroutineScope` no finaliza antes que sus corrutinas secundarias. La cancelación
del código que realiza la llamada también las cancela, mientras que un error no
gestionado en una tarea cancela el ámbito y las tareas hermanas.

Para tareas independientes se utiliza la supervisión:

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

Un error en una tarea secundaria directa no cancela las demás, pero aun así debe
gestionarse. `supervisorScope` se utiliza localmente y `SupervisorJob` en un
ámbito de larga duración.

`GlobalScope` vulnera este modelo: el código que realiza la llamada no puede
esperar correctamente a que termine el trabajo, cancelarlo ni recibir sus
errores. Un ámbito propio debe tener un ciclo de vida claro y una finalización
explícita.

**En resumen:** la concurrencia estructurada impide que el trabajo secundario
sobreviva de forma inadvertida a su propietario. Las tareas relacionadas
utilizan `coroutineScope` y las independientes, supervisión.

</details>

<details>
<summary>56. ¿Pueden las corrutinas ejecutarse en cualquier hilo?</summary>

#### Kotlin

Una corrutina no está vinculada a un hilo propio. `CoroutineDispatcher` determina
dónde se ejecutan sus diferentes partes:

```kotlin
scope.launch(Dispatchers.Main) {
    // Hilo de la interfaz de usuario
}

scope.launch(Dispatchers.IO) {
    // E/S bloqueante
}

scope.launch(Dispatchers.Default) {
    // Trabajo intensivo de CPU
}
```

- `Main`: la interfaz de usuario;
- `IO`: operaciones de E/S bloqueantes;
- `Default`: trabajo intensivo de CPU.

Cuando se utiliza un dispatcher con un grupo de hilos, la corrutina puede
reanudar su ejecución después de una suspensión en otro hilo del mismo
dispatcher:

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` cambia temporalmente el contexto y, al finalizar, restaura el
contexto del código que realizó la llamada. `Dispatchers.Main` garantiza la
ejecución en el hilo de la interfaz de usuario; los dispatchers con un grupo de
hilos no garantizan un identificador de hilo concreto.

Un `ThreadLocal` convencional no cambia de hilo junto con la corrutina. Cuando
es necesario, su valor se añade al contexto de la corrutina:

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

Un dispatcher personalizado con hilos propios debe tener un propietario que lo
cierre. Una llamada bloqueante bloquea el hilo aunque se ejecute dentro de una
corrutina.

**En resumen:** una corrutina se ejecuta en los hilos de su dispatcher y puede
cambiar de hilo físico después de una suspensión. El dispatcher debe garantizar
la vinculación a un hilo concreto cuando sea necesaria.

</details>

<details>
<summary>57. ¿Qué es Flow en Kotlin?</summary>

#### Kotlin

`Flow<T>` es un flujo asíncrono de valores basado en corrutinas. Una función
`suspend` devuelve un único valor, mientras que `Flow` devuelve una secuencia de
valores a lo largo del tiempo:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Un flujo se crea mediante un constructor y se inicia con un operador terminal,
como `collect`:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    delay(100)
    emit(2)
}
```

```kotlin
viewModelScope.launch {
    numbers().collect { value ->
        println(value)
    }
}
```

Un flujo creado mediante `flow {}` es frío: el productor se inicia de forma
independiente para cada colector. `emit()` es una función `suspend`, por lo que
el productor no se adelanta a un colector lento sin un almacenamiento en búfer
explícito.

Los operadores forman un pipeline:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` cambia el contexto de los operadores anteriores:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` solo gestiona los errores producidos en los operadores anteriores; no
intercepta los errores del colector. La cancelación del colector cancela la
recolección y el productor dentro de la concurrencia estructurada.

**En resumen:** utiliza una función `suspend` para obtener un único resultado y
`Flow` para recibir valores a lo largo del tiempo mediante un pipeline
asíncrono.

</details>

<details>
<summary>58. ¿Cuál es la diferencia entre Flow, StateFlow y SharedFlow?</summary>

#### Kotlin

```text
Flow       -> normalmente, un pipeline frío
StateFlow  -> un flujo caliente con el estado actual
SharedFlow -> una difusión caliente con replay y una política de búfer
```

Un `Flow` creado mediante `flow {}` inicia el productor de forma independiente
para cada colector y no conserva el valor actual:

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

`StateFlow` siempre tiene un valor inicial. Un colector nuevo recibe
inmediatamente el estado actual, y los valores iguales según `equals()` no se
emiten de nuevo:

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

La versión mutable se mantiene como `private`; para las actualizaciones
concurrentes se utiliza `update { }`.

`SharedFlow` envía valores a todos los colectores activos y no está obligado a
tener un valor actual:

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` determina cuántos de los últimos valores recibe un colector nuevo;
`extraBufferCapacity` y `onBufferOverflow` gestionan el búfer. Con `replay = 0`,
un valor se pierde si no hay suscriptores, por lo que los datos críticos deben
modelarse como estado y no como un evento único.

Un flujo frío puede convertirse en un flujo caliente compartido: `stateIn()`
crea un `StateFlow`, mientras que `shareIn()` crea un `SharedFlow`.
`SharingStarted` determina cuándo se inicia y se detiene la fuente compartida.

**En resumen:** `Flow` es un pipeline perezoso e independiente, `StateFlow`
representa el estado actual y `SharedFlow` es un flujo compartido de eventos con
un `replay` configurable.

</details>

<details>
<summary>59. ¿Cuál es la diferencia entre Flow y LiveData?</summary>

#### Kotlin

`LiveData` es un contenedor de Android que tiene en cuenta el ciclo de vida,
mientras que `Flow` es un flujo basado en corrutinas que no depende de Android.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` solo notifica a los observadores activos y vuelve a entregar el último
valor después de la activación. Resulta útil para interfaces de usuario heredadas
basadas en Views, pero no debe llegar a la capa de dominio.

`Flow` admite la cancelación de corrutinas, errores y un amplio conjunto de
operadores:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

Un `Flow` convencional suele ser frío y no conserva el valor actual. Para
representar el estado de la interfaz se utiliza `StateFlow`. Al recolectar un
flujo, el ciclo de vida debe gestionarse explícitamente:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

En Compose se utiliza `collectAsStateWithLifecycle()` para este propósito.
`LiveData` está orientado al hilo principal, mientras que `Flow` utiliza el
contexto de la corrutina y `flowOn` para los operadores anteriores.

Durante una migración es posible convertir ambos tipos:

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**En resumen:** `LiveData` tiene en cuenta automáticamente el ciclo de vida de
Android. `Flow` no depende de Android y resulta más adecuado para las capas de
datos y dominio; en la interfaz se recolecta teniendo en cuenta el ciclo de vida,
y el estado actual se conserva en un `StateFlow`.

</details>

<details>
<summary>60. ¿Cuál es la diferencia entre los flujos fríos y calientes?</summary>

#### Kotlin

La diferencia reside en la relación entre el productor y los colectores:

```text
frío     -> se inicia un productor independiente para cada colector
caliente -> los colectores reciben datos de una fuente compartida
```

Un flujo creado mediante `flow {}` es frío. No se ejecuta antes de llamar a
`collect`, y cada colector inicia el bloque por separado:

```kotlin
val userFlow = flow {
    println("Started")
    emit(api.loadUser())
}
```

```kotlin
userFlow.collect(::renderFirst)
userFlow.collect(::renderSecond)
```

Por tanto, la operación se ejecutará dos veces. Cada colector tiene su propio
ciclo de vida y su propia cancelación.

`StateFlow` y `SharedFlow` son calientes: su objeto y sus datos existen
independientemente de cualquier colector concreto.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` conserva el estado actual. `SharedFlow` distribuye los valores entre
los colectores activos y puede conservar los últimos valores mediante `replay`.

Un flujo frío se convierte en un flujo caliente compartido mediante `stateIn()`
o `shareIn()`. La política `SharingStarted` determina cuándo está activa la
fuente compartida, pero todos los colectores siguen utilizando una única
ejecución.

**En resumen:** un flujo frío inicia un productor para cada colector; un flujo
caliente posee una fuente compartida y distribuye sus valores entre todos los
colectores.

</details>

<details>
<summary>61. ¿Qué es collectLatest y cuándo debe utilizarse?</summary>

#### Kotlin

`collectLatest` es un operador terminal que cancela el procesamiento del valor
anterior cuando llega uno nuevo:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Solo llegará a imprimirse el 3
}
```

Un caso de uso habitual es una búsqueda en la que la consulta anterior deja de
ser relevante:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

La cancelación es cooperativa. Una API `suspend` responde a ella, pero el código
bloqueante no:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

No utilices `collectLatest` si se debe garantizar el procesamiento de todos los
valores, por ejemplo, en pagos, mensajes, operaciones de guardado o eventos
analíticos.

`flatMapLatest` cancela el flujo interno anterior, mientras que `collectLatest`
cancela el bloque del colector.

**En resumen:** utiliza `collect` cuando cada valor sea importante y
`collectLatest` cuando el procesamiento anterior deje de ser necesario tras
recibir un valor nuevo.

</details>

<details>
<summary>62. ¿Cómo se convierte una API basada en callbacks en una función suspend?</summary>

#### Kotlin

Un callback que devuelve un único resultado se envuelve en
`suspendCancellableCoroutine`:

```kotlin
suspend fun LocationClient.awaitLocation(): Location =
    suspendCancellableCoroutine { continuation ->
        val callback = object : LocationCallback {
            override fun onSuccess(location: Location) {
                continuation.resume(location)
            }

            override fun onError(error: Throwable) {
                continuation.resumeWithException(error)
            }
        }

        requestLocation(callback)

        continuation.invokeOnCancellation {
            cancelLocation(callback)
        }
    }
```

- `resume()` devuelve el resultado;
- `resumeWithException()` devuelve el error;
- `invokeOnCancellation` cancela la operación externa o elimina el listener.

Cancelar la corrutina no detiene automáticamente la API basada en callbacks. Una
continuación solo puede completarse una vez; si el callback puede ejecutarse
varias veces o de forma concurrente, se necesitan `tryResume()` y
`completeResume()`, o una protección atómica.

Para un callback que emite varios valores se utiliza `callbackFlow`:

```kotlin
fun Sensor.values(): Flow<Value> = callbackFlow {
    val listener = object : SensorListener {
        override fun onValue(value: Value) {
            trySend(value)
        }
    }

    register(listener)

    awaitClose {
        unregister(listener)
    }
}
```

`awaitClose` elimina el listener cuando se cancela o se cierra el flujo. El
resultado de `trySend()` y la política del búfer deben gestionarse de acuerdo con
las garantías de entrega requeridas.

**En resumen:** utiliza `suspendCancellableCoroutine` para un único resultado y
`callbackFlow` para un flujo de valores. En ambos casos es obligatorio liberar
los recursos al producirse una cancelación.

</details>

<details>
<summary>63. ¿Cuál es la diferencia entre Channel y Flow?</summary>

#### Kotlin

`Channel<T>` es una cola asíncrona para la comunicación entre corrutinas:

```kotlin
val channel = Channel<Int>()

launch {
    channel.send(1)
    channel.send(2)
    channel.close()
}

launch {
    for (value in channel) {
        println(value)
    }
}
```

El productor invoca `send()`, mientras que el consumidor invoca `receive()` o
lee los valores mediante `for`. Cada valor lo recibe un único consumidor, por lo
que Channel resulta adecuado para colas de trabajo y distribución entre varios
consumidores. La capacidad, el cierre y la cancelación deben gestionarse
explícitamente.

`Flow<T>` es una API declarativa para flujos asíncronos:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow admite `map`, `filter`, `combine`, cancelación y concurrencia estructurada.
Un flujo creado mediante `flow {}` suele ser frío; Channel es caliente y, según
su capacidad, puede aceptar valores sin un consumidor activo.

La contrapresión de Channel se determina mediante el búfer:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Espera a un receptor
```

- `RENDEZVOUS`: sin búfer;
- `BUFFERED`: búfer limitado;
- `CONFLATED`: conserva el último valor;
- `UNLIMITED`: búfer ilimitado, con el riesgo de aumentar el uso de memoria.

En Flow se utilizan `buffer()`, `conflate()` o `collectLatest()`.

Un Channel puede representarse como Flow:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` no realiza una difusión: los colectores se reparten los
valores. Para representar un estado se utiliza `StateFlow`, para una difusión
`SharedFlow` y como puente con callbacks, `callbackFlow`.

**En resumen:** `Channel` es una cola productor-consumidor, mientras que `Flow`
es un flujo de datos declarativo. Para los flujos de una aplicación, comienza
con Flow y reserva Channel para la transferencia explícita de trabajo entre
corrutinas.

</details>

<details>
<summary>64. ¿Cuáles son los principales operadores de Flow (map, filter, combine, zip)?</summary>

#### Kotlin

`map` transforma cada valor:

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` solo deja pasar los valores que cumplen una condición:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` espera el primer valor de cada Flow y después emite un resultado nuevo
cuando se actualiza cualquiera de ellos:

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` combina los valores por pares según su orden:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

El resultado es `1A`, `2B`, `3C`. `zip` espera el par correspondiente, mientras
que `combine` utiliza los últimos valores disponibles. Por tanto, para crear un
estado a partir de varias fuentes suele utilizarse `combine`, y para emparejar
elementos, `zip`.

**En resumen:** `map` transforma, `filter` filtra, `combine` reacciona a los
últimos valores de todos los flujos y `zip` combina los valores por pares.

</details>

<details>
<summary>65. ¿Cuál es la diferencia entre combine y zip?</summary>

#### Kotlin

Ambos operadores combinan flujos, pero tienen una semántica diferente:

```text
combine -> un valor nuevo + los últimos valores de los demás flujos
zip     -> el primero con el primero, el segundo con el segundo
```

`combine` espera el primer valor de cada flujo de origen y después reacciona a
cada actualización:

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` forma pares según el orden:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

En `zip`, el flujo de origen más rápido espera a que llegue el valor de su par.
El resultado finaliza cuando ya no pueden formarse pares completos. En
`combine`, un flujo de origen que haya finalizado puede seguir aportando su
último valor mientras los demás continúan emitiendo.

**En resumen:** utiliza `combine` para un estado que se actualiza desde
cualquiera de sus fuentes y `zip` para emparejar los elementos de varias
secuencias.

</details>

<details>
<summary>66. ¿Para qué sirven @JvmStatic, @JvmOverloads y @JvmField?</summary>

#### Kotlin

Estas anotaciones modifican la API de la JVM para el código Java.

- `@JvmStatic` genera un puente estático para un miembro de un `object` o un
  `companion object`.
- `@JvmOverloads` genera sobrecargas para los parámetros consecutivos con valores
  predeterminados, comenzando por la derecha.
- `@JvmField` expone directamente el campo de respaldo, sin getter ni setter.

```kotlin
class UserFactory {
    companion object {
        @JvmStatic
        fun create(): User = User()

        @JvmField
        val defaultName: String = "Unknown"
    }
}

@JvmOverloads
fun createUser(name: String, active: Boolean = true) = Unit
```

Java podrá invocar `UserFactory.create()`, leer `UserFactory.defaultName` y
utilizar la sobrecarga sin el parámetro `active`.

`@JvmField` no funciona con propiedades delegadas ni con accesores personalizados
y debilita la encapsulación. Para constantes de tiempo de compilación de tipo
primitivo o `String`, es preferible utilizar `const val`.

**En resumen:** estas anotaciones solo son necesarias para crear una API cómoda
para Java o un framework. No deben añadirse sin necesidad al código utilizado
únicamente desde Kotlin.

</details>

<details>
<summary>67. ¿Qué es la delegación (delegation)?</summary>

#### Kotlin

La delegación transfiere una implementación a otro objeto en lugar de utilizar
la herencia. Kotlin admite la delegación de interfaces mediante `by`:

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

El compilador genera métodos de reenvío. También se puede sobrescribir un método
concreto:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Las propiedades también pueden tener un delegado:

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

Un delegado de propiedad personalizado implementa `getValue()` y, en el caso de
una propiedad `var`, también `setValue()`.

**En resumen:** la delegación de clases elimina el código repetitivo de reenvío y
favorece la composición; la delegación de propiedades traslada la lógica de
lectura y escritura de una propiedad a otro objeto.

</details>

<details>
<summary>68. ¿Qué frameworks de pruebas están disponibles en Kotlin?</summary>

#### Kotlin

La elección depende del nivel de la prueba:

- `kotlin.test`: anotaciones y aserciones multiplataforma;
- JUnit: pruebas unitarias en la JVM;
- Kotest: un DSL alternativo y matchers;
- MockK o Mockito: dobles de prueba;
- `kotlinx-coroutines-test`: corrutinas, dispatchers de prueba y tiempo virtual;
- Turbine: comprobación de emisiones de Flow;
- AndroidX Test, Espresso y Compose UI Test: pruebas de Android y de la interfaz;
- Robolectric: determinados escenarios de Android en la JVM.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

Para la lógica de dominio suelen ser suficientes JUnit o `kotlin.test` y fakes
implementados manualmente. Una herramienta no sustituye la elección del nivel de
prueba adecuado: el comportamiento específico de un dispositivo se comprueba
mediante pruebas de instrumentación.

**En resumen:** utiliza JUnit o `kotlin.test` para pruebas unitarias,
coroutines-test para corrutinas, Turbine para Flow y Espresso o Compose UI Test
para la interfaz de Android.

</details>

<details>
<summary>69. ¿Cómo se simulan las dependencias en las pruebas?</summary>

#### Kotlin

La dependencia se proporciona mediante el constructor y se sustituye por un
mock, un stub o un fake.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

MockK para una función `suspend`:

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

Un fake implementado manualmente:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

Un fake resulta más adecuado para comprobar el comportamiento; un mock, cuando
la interacción forma parte del contrato. No conviene simular objetos de valor,
funciones puras ni cada llamada interna: dichas pruebas quedan acopladas a la
implementación.

**En resumen:** utiliza la inyección mediante el constructor; da prioridad a un
fake sencillo y emplea un mock para las interacciones importantes con una
dependencia externa.

</details>

<details>
<summary>70. ¿Cómo se prueban las corrutinas y Flow?</summary>

#### Kotlin

Para probar corrutinas se utilizan `kotlinx-coroutines-test` y `runTest`:

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` proporciona tiempo virtual y un `TestCoroutineScheduler`:

- `runCurrent()` ejecuta las tareas programadas para el momento actual;
- `advanceTimeBy()` hace avanzar el tiempo virtual;
- `advanceUntilIdle()` ejecuta la cola hasta que queda inactiva.

Es preferible proporcionar los dispatchers como dependencias. En las pruebas
unitarias locales, `Dispatchers.Main` se sustituye por un dispatcher de prueba y
se restaura mediante `resetMain()`.

La secuencia de emisiones de Flow puede comprobarse cómodamente con Turbine:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Para comprobar el estado final de un `StateFlow`, a menudo basta con ejecutar las
tareas pendientes y verificar `state.value`. No utilices `Thread.sleep()`,
retrasos reales ni distintos planificadores de prueba dentro de una misma
prueba.

**En resumen:** utiliza `runTest` y un dispatcher de prueba para las corrutinas,
Turbine para comprobar el orden de las emisiones de Flow y `StateFlow.value`
para verificar el estado final.

</details>

<details>
<summary>71. ¿Cuáles son las mejores prácticas para escribir código comprobable?</summary>

#### Kotlin

El código comprobable tiene dependencias explícitas y un comportamiento
predecible.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

Las reglas principales son:

- utilizar la inyección mediante el constructor en lugar de crear dependencias
  dentro de la clase;
- asignar una única responsabilidad clara a cada clase;
- utilizar funciones puras para los cálculos de negocio;
- crear abstracciones para el tiempo, la E/S, las API, las bases de datos y los
  dispatchers;
- minimizar el uso del framework de Android en la lógica de dominio;
- utilizar fakes en lugar de un exceso de mocks;
- comprobar el comportamiento público, no los métodos privados ni las llamadas
  internas.

Un ViewModel se prueba mediante su estado y sus efectos observables:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

No es necesario crear una interfaz para cada clase. Una abstracción está
justificada en el límite con una dependencia externa o susceptible de cambiar.

**En resumen:** las dependencias explícitas, los efectos secundarios controlados
y las responsabilidades pequeñas hacen que el código sea comprobable sin
acoplar las pruebas a su implementación.

</details>

<details>
<summary>72. ¿Cómo se gestionan eficazmente las dependencias en un proyecto de Kotlin?</summary>

#### Kotlin

En un proyecto de Gradle se deben controlar las versiones, las configuraciones y
la dirección de las dependencias entre módulos.

- Un catálogo de versiones centraliza los alias y las versiones en
  `libs.versions.toml`.
- `implementation` oculta la dependencia a los módulos posteriores.
- `api` la expone como parte de la API pública y solo debe utilizarse cuando sea
  necesario.
- `testImplementation` y `androidTestImplementation` aíslan las dependencias de
  prueba.
- Un BOM coordina las versiones de los artefactos de un mismo ecosistema.
- Los plugins de convenciones eliminan la duplicación en la configuración de
  Gradle.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

Un módulo debe depender del contrato que necesita, no de toda la implementación.
Las dependencias de Gradle y la inyección de dependencias en tiempo de ejecución
pertenecen a niveles diferentes: Gradle determina la disponibilidad del código,
mientras que la inyección crea los objetos durante la ejecución.

Para realizar diagnósticos:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Las actualizaciones deben realizarse en pasos pequeños, comprobando el registro
de cambios, la compilación y las pruebas.

**En resumen:** centraliza las versiones, da prioridad a `implementation`,
controla el grafo de módulos y revisa periódicamente el grafo de dependencias.

</details>

<details>
<summary>73. ¿Cómo gestiona Kotlin la memoria y la recolección de basura?</summary>

#### Kotlin

El modelo de memoria depende de la plataforma. En la JVM y Android, Kotlin
utiliza la gestión de memoria de la JVM o ART: los objetos residen en el heap y
el recolector de basura libera aquellos que ya no tienen referencias
alcanzables.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

Cuando finaliza la función, el objeto puede ser recolectado si nada lo retiene.
Asignar `null` únicamente elimina una referencia; el recolector de basura decide
cuándo liberar la memoria.

Se produce una fuga de memoria cuando un objeto innecesario sigue siendo
alcanzable:

```kotlin
object Holder {
    var activity: Activity? = null // Puede retener una Activity
}
```

En Android, algunas causas habituales son conservar una Activity o una View en
un singleton, no eliminar listeners o bindings, y ejecutar una corrutina con un
ciclo de vida más largo. Las asignaciones frecuentes, el boxing, los closures y
las colecciones intermedias pueden aumentar la presión sobre el recolector de
basura, pero solo deben optimizarse después de realizar mediciones.

Kotlin/Native y Kotlin/JS utilizan los mecanismos de memoria de sus respectivas
plataformas, por lo que los detalles de la JVM no pueden aplicarse a todos los
destinos.

**En resumen:** en la JVM y Android, el recolector de basura libera la memoria
cuando se pierden todas las referencias alcanzables. El principal riesgo no es
`null`, sino una referencia de larga duración a un objeto innecesario.

</details>

<details>
<summary>74. ¿Cuáles son las mejores prácticas para trabajar con la seguridad frente a null?</summary>

#### Kotlin

Un tipo nullable debe representar una posibilidad real de que el valor esté
ausente, no utilizarse «por si acaso».

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

Las reglas principales son:

- evita `!!`; utiliza `?.`, `?:` o un retorno anticipado;
- utiliza `requireNotNull` para un contrato de entrada y `checkNotNull` para el
  estado interno;
- comprueba los tipos de plataforma de Java en el límite entre Java y Kotlin;
- convierte los DTO nullable en modelos de dominio estrictos mediante un mapper;
- si `null` representa un estado independiente, modélalo con un tipo `sealed`.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

Un valor alternativo no debe ocultar un error: si la ausencia de un valor
incumple el contrato, es preferible interrumpir la ejecución explícitamente.

**En resumen:** modela la ausencia de un valor mediante el tipo `T?`, gestiónala
en el límite del sistema y no conviertas el código nullable en una cadena de
operadores `!!`.

</details>

<details>
<summary>75. ¿Qué es supervisorScope y en qué se diferencia?</summary>

#### Kotlin

En un `coroutineScope`, un error no gestionado en una corrutina secundaria
cancela el ámbito y las tareas hermanas. En un `supervisorScope`, las tareas
secundarias directas pueden fallar de forma independiente:

```text
coroutineScope  -> el fallo de una tarea secundaria cancela las tareas hermanas
supervisorScope -> el fallo de una tarea secundaria no cancela las tareas hermanas
```

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async {
        runCatching { api.getUser() }
    }
    val posts = async {
        runCatching { api.getPosts() }
    }

    PartialData(
        user = user.await().getOrNull(),
        posts = posts.await().getOrDefault(emptyList())
    )
}
```

Un supervisor no gestiona los errores automáticamente. Una excepción de `async`
se lanzará mediante `await()` si no se convierte previamente en un resultado.

`supervisorScope` se utiliza localmente dentro de una función `suspend`, mientras
que `SupervisorJob` se emplea para un ámbito de larga duración.

**En resumen:** utiliza `coroutineScope` para tareas relacionadas que deben
completarse todas o ninguna, y `supervisorScope` para tareas independientes que
admiten un resultado parcial.

</details>

<details>
<summary>76. ¿Qué es la contrapresión y cómo se gestiona en Flow?</summary>

#### Kotlin

La contrapresión se produce cuando el productor puede ser más rápido que el
consumidor. En un Flow convencional sin búfer, `emit()` se suspende hasta que las
operaciones posteriores terminan de procesar el valor, por lo que el productor
se ralentiza de forma natural.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

Los operadores modifican este comportamiento:

- `buffer(n)` permite que el productor se adelante al consumidor en `n` valores;
- `conflate()` omite los valores intermedios y conserva el más reciente;
- `collectLatest` cancela el procesamiento anterior;
- `debounce` espera una pausa entre los eventos entrantes;
- `sample` toma el último valor a intervalos determinados;
- `flatMapLatest` cancela el flujo interno anterior.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

Si todos los valores son importantes, no deben confluirse ni cancelarse: es
necesario optimizar el consumidor, utilizar un búfer controlado o una cola
fiable. Un búfer ilimitado puede provocar un aumento del uso de memoria.

**En resumen:** Flow ya proporciona contrapresión mediante la suspensión de
`emit()`. El almacenamiento en búfer, la confluencia o los operadores `latest`
solo deben añadirse cuando se conozca la política necesaria para perder o
esperar datos.

</details>

<details>
<summary>77. ¿Cómo se utilizan los Streams de Java en Kotlin?</summary>

#### Kotlin

Kotlin/JVM puede utilizar directamente la API Stream de Java:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Para las colecciones de Kotlin, los operadores estándar suelen ser más concisos:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Para el procesamiento síncrono y perezoso se utiliza `Sequence`:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Stream resulta adecuado cuando lo devuelve una API de Java, se necesita un
`Collector` de Java o el pipeline ya está escrito en Java. Un Stream solo puede
utilizarse una vez.

Los Streams de E/S deben cerrarse:

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Los Collectors de Java suelen tener un equivalente directo en Kotlin:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` utiliza el `ForkJoinPool` común, por lo que introduce un
paralelismo implícito y resulta más difícil de cancelar. En el código con
corrutinas es preferible controlar el paralelismo explícitamente; para
operaciones de E/S, un Stream paralelo no sustituye una API `suspend`.

```text
Stream/Sequence -> pipeline síncrono de un solo uso
Flow            -> flujo asíncrono, suspendible y cancelable
```

**En resumen:** utiliza Java Stream para la interoperabilidad con Java, los
operadores de colecciones de Kotlin para el procesamiento inmediato, `Sequence`
para un pipeline síncrono y perezoso, y `Flow` para datos asíncronos.

</details>

<details>
<summary>78. ¿Qué es Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` es el antiguo nombre de la vertiente móvil de `Kotlin Multiplatform`
(`KMP`). La idea consiste en trasladar el código común de Kotlin a un módulo
compartido y utilizarlo tanto en Android como en iOS.

Por lo general, se comparten:

- los modelos de dominio;
- los casos de uso;
- la validación;
- los contratos de los repositorios;
- las capas de red y datos;
- las reglas de negocio.

Una estructura habitual es:

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` contiene código independiente de la plataforma:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Las API específicas de cada plataforma se ocultan detrás de una interfaz o de
`expect`/`actual`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

La interfaz de usuario puede seguir siendo nativa: Compose o Views en Android, y
SwiftUI o UIKit en iOS. Si el equipo está preparado, también puede compartir
parte de la interfaz mediante Compose Multiplatform.

**En resumen:** KMP permite compartir la lógica de negocio entre Android e iOS,
pero las API de plataforma, los detalles de la experiencia de usuario y las
integraciones deben aislarse. No significa «un único código para todo», sino
compartir de forma controlada aquello que realmente reduce la duplicación.

</details>

<details>
<summary>79. ¿Qué limitaciones tiene Kotlin Multiplatform?</summary>

#### Kotlin

`Kotlin Multiplatform` resulta útil para compartir lógica, pero presenta
limitaciones importantes.

1. **Las API de plataforma no están disponibles en `commonMain`**

El código compartido no puede utilizar directamente `Context`, UIKit ni SDK
exclusivos de Android o iOS. Se necesitan interfaces o `expect`/`actual`.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **No todas las bibliotecas son multiplataforma**

Una biblioteca exclusiva de Java o Android no puede añadirse directamente a
`commonMain`. Es necesario buscar una alternativa compatible con KMP o crear una
implementación específica para la plataforma.

3. **La interoperabilidad con iOS tiene matices**

El código Kotlin compartido se exporta a iOS como un framework. Se deben tener en
cuenta las API de Swift, los genéricos, las excepciones, los puentes para
corrutinas y Flow, y la depuración.

4. **Compartir la interfaz de usuario no siempre resulta ventajoso**

A menudo se comparten las capas de dominio y datos, mientras que la interfaz se
mantiene nativa. Compose Multiplatform es una opción, pero constituye una
decisión arquitectónica independiente.

5. **La compilación y la responsabilidad son más complejas**

KMP añade conjuntos de código fuente, configuración de Gradle y complejidad en
CI, y requiere acuerdos entre los equipos de Android e iOS sobre el módulo
compartido.

**En resumen:** KMP resulta adecuado para una lógica de negocio compartida y
estable, pero no elimina el código específico de cada plataforma. Los principales
riesgos son la interoperabilidad, las bibliotecas, la complejidad de compilación,
la responsabilidad y trasladar demasiado código al módulo compartido.

</details>

<details>
<summary>80. ¿Cómo se comparte la lógica de negocio en KMM?</summary>

#### Kotlin

La lógica de negocio se traslada a un módulo compartido, mientras que los
detalles de Android e iOS permanecen en los conjuntos de código fuente
específicos de cada plataforma.

Una estructura habitual es:

```text
shared/
  commonMain   -> dominio/casos de uso/contratos
  androidMain  -> implementación de Android
  iosMain      -> implementación de iOS
```

En `commonMain` conviene incluir:

- los modelos de dominio;
- los casos de uso;
- la validación;
- las interfaces de los repositorios;
- los clientes de API;
- la serialización;
- las reglas de negocio.

Un ejemplo:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Las API específicas de cada plataforma se proporcionan mediante inyección de
dependencias, interfaces o `expect`/`actual`:

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

La comunicación de red suele implementarse con Ktor Client, el procesamiento de
JSON con `kotlinx.serialization` y el estado con corrutinas y Flow. Para iOS debe
planificarse desde el principio un puente hacia Swift async/await o Combine.

No conviene compartir todo indiscriminadamente: los permisos, la cámara, las
notificaciones, el almacenamiento de la plataforma y la lógica específica de la
experiencia de usuario deben permanecer en cada plataforma u ocultarse detrás de
un contrato.

**En resumen:** el módulo compartido contiene la lógica de negocio estable y los
contratos, mientras que las partes específicas de cada plataforma se implementan
por separado. Un buen KMM establece un límite claro, no un monolito compartido.

</details>

<details>
<summary>81. ¿Cómo se optimiza el rendimiento del código Kotlin?</summary>

#### Kotlin

La optimización no comienza con la sintaxis, sino con la medición:

```text
medir -> encontrar el cuello de botella -> corregir -> volver a medir
```

En Android se utilizan Profiler, Perfetto/System Trace, Macrobenchmark y el
seguimiento de asignaciones.

Las áreas principales son:

1. **Algoritmos y E/S**

Las mayores mejoras proceden de elegir estructuras de datos adecuadas, reducir
las llamadas de red y a la base de datos, y utilizar paginación, caché e índices
de base de datos. Una complejidad `O(n²)` no se corrige con una sintaxis de Kotlin
elegante.

2. **Asignaciones en rutas críticas**

En zonas críticas, el uso innecesario de `map`, `filter` o `copy` puede crear
muchos objetos:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

Sin embargo, en una interfaz convencional la legibilidad es más importante que
una microoptimización manual.

3. **Colecciones y Sequence**

`Sequence` resulta útil para cadenas perezosas o una finalización anticipada,
pero tiene un coste adicional. En listas pequeñas no es necesariamente más
rápida.

4. **Corrutinas**

- operaciones de E/S bloqueantes: `Dispatchers.IO`;
- trabajo de CPU: `Dispatchers.Default`;
- no utilizar `runBlocking` en Main;
- cancelar el trabajo obsoleto;
- evitar cambios de contexto innecesarios.

5. **Rendimiento de la interfaz de usuario**

En Compose no se debe realizar trabajo pesado durante la composición; conviene
utilizar claves estables y leer el estado cerca del lugar donde se utiliza. En
Views no se deben sobrecargar `onBindViewHolder` ni `onDraw`, ni crear pasadas de
layout innecesarias.

6. **Memoria**

No se deben conservar objetos `Activity` o `View` en un singleton. También es
necesario controlar el tamaño de la caché, los recursos vinculados al ciclo de
vida y los bitmaps grandes. Las fugas aumentan la presión sobre el recolector de
basura y el riesgo de OOM.

**En resumen:** primero se mide el rendimiento. Después se corrigen los
algoritmos, la E/S, las asignaciones, la concurrencia, las rutas críticas de la
interfaz y las fugas de memoria. `inline`, `Sequence` y los arrays de tipos
primitivos solo deben utilizarse cuando las mediciones demuestren su beneficio.

</details>

<details>
<summary>82. ¿Cómo se implementan reintentos con retroceso exponencial en Kotlin?</summary>

#### Kotlin

El retroceso exponencial repite una operación tras un error transitorio,
aumentando progresivamente la pausa. Los reintentos deben tener `maxAttempts`,
`maxDelay`, jitter y un tiempo de espera, y no deben interferir con la
cancelación.

```kotlin
suspend fun <T> retryWithBackoff(
    maxAttempts: Int = 3,
    initialDelayMillis: Long = 1_000,
    maxDelayMillis: Long = 10_000,
    factor: Double = 2.0,
    shouldRetry: (Throwable) -> Boolean,
    block: suspend () -> T
): T {
    require(maxAttempts > 0)

    var currentDelay = initialDelayMillis

    repeat(maxAttempts - 1) {
        try {
            return block()
        } catch (error: Throwable) {
            if (error is CancellationException || !shouldRetry(error)) throw error

            delay(Random.nextLong(currentDelay + 1))
            currentDelay = (currentDelay * factor)
                .toLong()
                .coerceAtMost(maxDelayMillis)
        }
    }

    return block()
}
```

Un ejemplo de predicado:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Para `Flow` se puede utilizar `retryWhen`:

```kotlin
apiFlow.retryWhen { cause, attempt ->
    if (attempt < 3 && shouldRetry(cause)) {
        delay(1_000L shl attempt.toInt())
        true
    } else {
        false
    }
}
```

No deben repetirse los errores de validación, las respuestas `403` o `404`, los
fallos de autenticación sin un flujo de renovación ni las operaciones no
idempotentes sin una clave de idempotencia. Para `429` y `503` conviene tener en
cuenta `Retry-After`.

**En resumen:** los reintentos deben ser limitados, cancelables, incluir jitter
y definir claramente qué errores pueden repetirse. De lo contrario, ocultan
problemas y generan carga adicional.

</details>

<details>
<summary>83. ¿Qué novedades de Kotlin conviene conocer en 2026?</summary>

#### Kotlin

En una entrevista en 2026, no es necesario conocer cada característica menor,
sino comprender la evolución de Kotlin 2.x y su ecosistema de herramientas.

1. **Compilador K2**

K2 es la nueva arquitectura del compilador de Kotlin 2.x. Es importante porque
ofrece un análisis más rápido y una base mejor para el IDE, los plugins del
compilador y Multiplatform. Antes de una actualización se debe comprobar la
compatibilidad de Kotlin Gradle Plugin, AGP, Gradle, el compilador de Compose,
KSP/kapt y la serialización.

2. **Parámetros y receptores de contexto**

Resultan útiles para DSL y API con un ámbito determinado:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

No sustituyen la inyección de dependencias, sino que permiten proporcionar
explícitamente el contexto de ejecución.

3. **Campos de respaldo explícitos**

Permiten reducir el código repetitivo al crear una API pública de solo lectura
con un estado interno mutable:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Evolución de Multiplatform**

KMP es cada vez más práctico: continúan evolucionando la exportación a Swift,
los puentes para corrutinas y Flow, Wasm y Compose Multiplatform. Sin embargo,
las decisiones para producción deben seguir evaluándose según la
interoperabilidad, las herramientas, la depuración y la responsabilidad del
equipo.

5. **Biblioteca estándar y herramientas de la JVM**

Conviene seguir las nuevas API de la biblioteca estándar, `kotlin.uuid.Uuid`, la
compatibilidad del target y la toolchain de la JVM, la caché de configuración de
Gradle y el rendimiento de compilación.

6. **Reglas de adopción**

Una característica nueva no debe incorporarse a producción únicamente por ser
nueva. Se debe comprobar:

- si es estable o experimental;
- la compatibilidad con el IDE y CI;
- la compatibilidad de los plugins;
- la legibilidad;
- el plan de migración y reversión.

**En resumen:** en 2026 conviene conocer Kotlin 2.x y K2, las API de contexto, los
campos de respaldo, la evolución de KMP, Wasm y Compose Multiplatform, y las
limitaciones de las herramientas. En la práctica, actualizar el ecosistema de
forma segura es más importante que utilizar cada nueva característica
sintáctica.

</details>

<details>
<summary>84. ¿Qué es el ciclo de vida de una Activity?</summary>

#### Kotlin

El ciclo de vida de una `Activity` es una secuencia de callbacks mediante los
cuales Android gestiona la creación, la visibilidad, los estados de primer y
segundo plano, y la destrucción de una pantalla.

El orden principal es:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Al regresar desde el estado `STOPPED`, puede ejecutarse `onRestart()` antes de
`onStart()`.

- `onCreate()`: crea la interfaz, la navegación y las dependencias básicas. No
  debe realizar trabajo síncrono pesado.
- `onStart()`: la Activity pasa a ser visible.
- `onResume()`: la Activity está en primer plano y lista para la interacción.
- `onPause()`: pierde el foco; este callback debe ser rápido.
- `onStop()`: deja de ser visible; deben detenerse los recursos necesarios solo
  mientras es visible.
- `onDestroy()`: la instancia se destruye debido a `finish()` o a una
  recreación; no se garantiza su ejecución si el proceso es terminado.

Un cambio de configuración puede recrear la Activity:

```text
anterior: onPause -> onStop -> onDestroy
nueva:    onCreate -> onStart -> onResume
```

Un `ViewModel` sobrevive a un cambio de configuración. El estado pequeño de la
interfaz se guarda en `SavedStateHandle` o `onSaveInstanceState`, mientras que
los datos duraderos se almacenan de forma persistente.

La recolección de Flow debe vincularse al ciclo de vida:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**En resumen:** `onCreate` crea la pantalla, `onStart` la hace visible,
`onResume` la activa, `onPause` y `onStop` detienen el trabajo, y `onDestroy`
finaliza la instancia. El estado no debe depender únicamente de la instancia de
la Activity.

</details>

<details>
<summary>85. ¿Qué es un Fragment y en qué se diferencia de una Activity?</summary>

#### Kotlin

Una `Activity` es un componente de Android con su propia ventana y tarea, y un
punto de entrada declarado en el manifest. Un `Fragment` es un controlador de la
interfaz dentro de una Activity o de otro Fragment, gestionado por
`FragmentManager`.

```text
Activity -> componente del sistema, ventana, intents, tarea
Fragment -> parte de la interfaz dentro de un host, back stack mediante FragmentManager
```

Un ejemplo de Activity:

```kotlin
class MainActivity : AppCompatActivity()
```

Un ejemplo de Fragment:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

La principal diferencia práctica es que un Fragment tiene un ciclo de vida
independiente para su View:

```text
onCreateView -> onViewCreated -> onDestroyView
```

El objeto Fragment puede seguir existiendo después de que su View haya sido
destruida. Por ello, el binding, el adapter y las suscripciones de la interfaz
deben limpiarse en `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Los objetos Flow o LiveData de la interfaz deben vincularse a
`viewLifecycleOwner`, no al ciclo de vida del propio Fragment.

Para la navegación:

- entre Activities se utiliza `Intent`;
- entre Fragments se utiliza `FragmentManager` o Navigation Component.

En una aplicación creada íntegramente con Compose suele bastar una única
Activity sin Fragments. En una aplicación híbrida o basada en Views, Fragment
sigue siendo una abstracción válida para las pantallas y la navegación.

**En resumen:** una Activity es un componente independiente de Android, mientras
que un Fragment es una parte de la interfaz dentro de un host. La regla más
importante es que la View de un Fragment vive menos que el propio Fragment, por
lo que la limpieza se realiza en `onDestroyView()`.

</details>

<details>
<summary>86. ¿Qué es un Intent y cómo se utiliza?</summary>

#### Kotlin

Un `Intent` es un objeto de mensaje de Android que describe una acción o un
componente concreto: abrir una Activity, iniciar un Service, enviar un Broadcast
o transferir datos.

1. **Intent explícito**

Indica un componente concreto:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

Se utiliza para la navegación interna entre Activities.

2. **Intent implícito**

Describe una acción y Android busca una aplicación capaz de realizarla:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Algunos ejemplos son el navegador, la función de compartir, el marcador
telefónico, los mapas y el selector de archivos.

3. **Extras**

Los extras permiten transferir pequeñas cantidades de datos:

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Es preferible transferir un `id` en lugar de objetos grandes. Para argumentos
estructurados se puede utilizar `Parcelable`, aunque la navegación suele ser más
sencilla mediante un identificador y un repositorio.

4. **Filtro de Intent**

Un componente declara qué intents acepta:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

Esto es necesario para enlaces profundos, destinos de contenido compartido y
acciones del sistema.

5. **Activity Result API**

En lugar de `startActivityForResult`, se utiliza una API que tiene en cuenta el
ciclo de vida:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // Procesar el URI
}

launcher.launch("image/*")
```

6. **Seguridad**

- no transfieras secretos mediante extras;
- inicia los componentes internos mediante intents explícitos;
- valida los datos de entrada externos;
- configura correctamente `android:exported`;
- para un intent implícito, ten en cuenta que puede no existir una aplicación
  capaz de procesarlo.

**En resumen:** un intent explícito inicia un componente concreto, mientras que
un intent implícito describe una acción para el sistema. Los extras solo deben
utilizarse para datos pequeños. Para recibir resultados se utiliza Activity
Result API. Los intents externos requieren validación y atención a la seguridad.

</details>

<details>
<summary>87. ¿Qué ocurre si no se invoca super.onCreate()?</summary>

#### Kotlin

Si `Activity.onCreate()` no invoca `super.onCreate(savedInstanceState)`, la clase
base de Android o Jetpack no realiza la inicialización obligatoria del ciclo de
vida. Esto puede provocar un fallo o un funcionamiento incorrecto de la Activity.

La forma correcta es:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` prepara:

- el estado del ciclo de vida;
- la restauración del estado guardado;
- la infraestructura de la ventana y su decoración;
- `FragmentManager` en una `FragmentActivity`;
- `SavedStateRegistry`;
- `ActivityResultRegistry`;
- las integraciones con AppCompat y ComponentActivity.

Sin `super` pueden producirse:

- una `SuperNotCalledException`;
- un ciclo de vida defectuoso;
- problemas con los fragments;
- fallos al restaurar el estado guardado;
- problemas con Activity Result API, Compose, Hilt o AppCompat.

La misma regla se aplica a Compose:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

En los callbacks del ciclo de vida de un Fragment también suele invocarse
`super`, salvo que la documentación indique explícitamente lo contrario.

**En resumen:** `super.onCreate()` no es una formalidad, sino una parte del
contrato con el framework. Debe invocarse al comienzo de `onCreate()`, antes de
la lógica que depende de la interfaz o del framework.

</details>

<details>
<summary>88. ¿Qué es View Binding y por qué es mejor que findViewById?</summary>

#### Kotlin

`View Binding` genera una clase de binding para un layout XML y proporciona
acceso con seguridad de tipos a sus Views, sin utilizar manualmente
`findViewById`.

El problema de `findViewById`:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Sus desventajas son:

- código repetitivo;
- posibilidad de equivocarse de tipo;
- errores en tiempo de ejecución;
- refactorización menos fiable.

View Binding en una `Activity`:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

En un `Fragment`, el binding debe limpiarse en `onDestroyView()`, ya que el ciclo
de vida de la View es más corto que el del Fragment:

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View {
    _binding = FragmentProfileBinding.inflate(inflater, container, false)
    return binding.root
}

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Las ventajas de View Binding son:

- seguridad de tipos;
- menos código repetitivo;
- comprobación de identificadores durante la compilación;
- refactorización más cómoda;
- campos nullable cuando una View no existe en una configuración concreta del
  layout.

`View Binding` no es lo mismo que `Data Binding`:

```text
View Binding -> acceso a las Views
Data Binding -> expresiones de binding en XML
```

View Binding no suele ser necesario para una interfaz creada completamente con
Compose. Sigue siendo relevante para XML, View System y pantallas híbridas.

**En resumen:** View Binding es una alternativa más segura y concisa a
`findViewById` para layouts XML. El principal matiz es que, en un Fragment, el
binding debe limpiarse en `onDestroyView()`.

</details>

<details>
<summary>89. ¿Qué es LiveData?</summary>

#### Kotlin

`LiveData` es un contenedor de datos observable de Android Jetpack que tiene en
cuenta el ciclo de vida. Almacena un valor y solo notifica a los observadores
cuando el `LifecycleOwner` se encuentra en el estado `STARTED` o `RESUMED`.

Un ejemplo habitual en un `ViewModel`:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

Se expone `LiveData`, no `MutableLiveData`, para que la interfaz no pueda
modificar el estado directamente.

Una suscripción en un Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` es importante porque la View de un Fragment vive menos que
el propio Fragment.

La diferencia entre `value` y `postValue`:

```kotlin
_liveData.value = user      // Hilo principal
_liveData.postValue(user)   // Hilo secundario, de forma asíncrona
```

Sus ventajas son:

- tiene en cuenta el ciclo de vida;
- ofrece una API sencilla;
- funciona bien con XML y View System;
- elimina automáticamente la suscripción de un propietario destruido;
- resulta práctico para código Android heredado.

Sus limitaciones son:

- está vinculado a Android;
- ofrece una API para flujos menos potente que `Flow`;
- no resulta cómodo para KMP;
- los eventos de un solo uso suelen conducir a soluciones improvisadas
  similares a `SingleLiveEvent`.

Una comparación:

```text
LiveData  -> contenedor de Android que tiene en cuenta el ciclo de vida
StateFlow -> flujo caliente de Kotlin con un valor de estado
```

En código nuevo basado en corrutinas o Compose se utilizan con mayor frecuencia
`StateFlow` y `collectAsStateWithLifecycle()`. `LiveData` sigue siendo adecuado
para View System heredado o proyectos donde ya constituye el estándar.

**En resumen:** LiveData es un contenedor de estado observable de Android que
tiene en cuenta el ciclo de vida. Para código nuevo de Kotlin y Compose suele ser
preferible `StateFlow`, aunque LiveData continúa funcionando correctamente con
XML y View System.

</details>

<details>
<summary>90. ¿Qué es Room y cómo funciona?</summary>

#### Kotlin

`Room` es una biblioteca de Jetpack para trabajar con SQLite mediante una API
con seguridad de tipos. No elimina SQL, sino que proporciona comprobaciones
durante la compilación, DAO, migraciones y una integración práctica con
corrutinas y Flow.

Sus componentes principales son:

```text
@Entity   -> tabla
@Dao      -> métodos de acceso SQL
@Database -> punto de entrada a la base de datos
```

Una entidad:

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val age: Int
)
```

Un DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
}
```

La base de datos:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Su creación:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room admite `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Cuando la tabla cambia, Room invalida la consulta y emite los datos nuevos.

Las relaciones suelen requerir `@Transaction`:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Los cambios del esquema se realizan mediante migraciones:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

En producción no debe utilizarse una migración destructiva de forma
indiscriminada si existen datos del usuario.

En la práctica, Room se oculta detrás de un repositorio:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

La interfaz de usuario no debe depender directamente de las entidades de la base
de datos.

**En resumen:** Room es una capa con seguridad de tipos sobre SQLite basada en
`Entity`, `Dao` y `Database`. Admite funciones `suspend`, Flow, relaciones y
migraciones. En una arquitectura adecuada, Room reside en la capa de datos
detrás de un repositorio, no directamente en la interfaz.

</details>

<details>
<summary>91. ¿Qué hacen las anotaciones @Query, @Insert y @Delete?</summary>

#### Kotlin

`@Query`, `@Insert` y `@Delete` son anotaciones de Room para métodos DAO. Room
genera la implementación y comprueba el SQL y el mapeo durante la compilación.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

Ejecuta una sentencia SQL explícita:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` y `:name` son parámetros vinculados. No debe concatenarse la entrada del
usuario en una cadena SQL.

`@Query` no se limita a sentencias `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

El valor `Int` indica la cantidad de filas afectadas.

### `@Insert`

Inserta una `@Entity`:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` define el comportamiento cuando existe un conflicto de clave
primaria o única: `ABORT`, `IGNORE` o `REPLACE`. Se debe tener cuidado con
`REPLACE`, ya que SQLite puede eliminar la fila anterior e insertar una nueva.
Para una operación de inserción o actualización suele ser preferible `@Upsert`,
si está disponible en el proyecto.

### `@Delete`

Elimina una entidad mediante su clave primaria:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Para eliminar mediante un identificador o una condición, es preferible utilizar
`@Query("DELETE ...")`.

### `@Transaction`

Si varias operaciones DAO deben ser atómicas, se utiliza `@Transaction`:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room comprueba las tablas, las columnas, los parámetros vinculados y el mapeo
del resultado. Sin embargo, el desarrollador sigue siendo responsable del
rendimiento, los índices y la lógica de negocio.

**En resumen:** `@Query` ejecuta SQL, `@Insert` inserta una entidad y `@Delete`
elimina una entidad mediante su clave primaria. Room genera el código DAO y
detecta muchos errores durante la compilación, pero no sustituye el análisis del
rendimiento de las consultas.

</details>

<details>
<summary>92. ¿Qué es WorkManager y cuándo debe utilizarse?</summary>

#### Kotlin

`WorkManager` es una API de Jetpack para ejecutar trabajo persistente, diferible
y en segundo plano. Es decir, una tarea puede comenzar más tarde, pero debe
sobrevivir al cierre de la aplicación, la finalización del proceso y, cuando sea
necesario, el reinicio del dispositivo.

Se utiliza para:

- sincronización en segundo plano;
- reintentos de carga o descarga;
- limpieza;
- envío de registros y datos analíticos;
- mantenimiento periódico;
- trabajo sujeto a restricciones de red, carga, batería o almacenamiento.

Un trabajo único:

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .setConstraints(
        Constraints.Builder()
            .setRequiredNetworkType(NetworkType.CONNECTED)
            .build()
    )
    .build()

WorkManager.getInstance(context).enqueue(request)
```

Un `CoroutineWorker`:

```kotlin
class SyncWorker(
    context: Context,
    params: WorkerParameters,
    private val repository: SyncRepository
) : CoroutineWorker(context, params) {
    override suspend fun doWork(): Result = try {
        repository.sync()
        Result.success()
    } catch (error: IOException) {
        Result.retry()
    } catch (error: Exception) {
        Result.failure()
    }
}
```

Los resultados posibles son:

- `Result.success()`: la tarea ha finalizado correctamente;
- `Result.retry()`: debe repetirse según una política de retroceso;
- `Result.failure()`: se ha producido un fallo definitivo.

Sus características importantes son:

- restricciones;
- reintentos y políticas de retroceso;
- trabajo único mediante `enqueueUniqueWork()`;
- encadenamiento;
- trabajo periódico, aunque sin una hora de inicio exacta.

Los datos de entrada y salida de tipo `Data` deben ser pequeños. Las cargas
grandes deben guardarse en una base de datos o un archivo, y transferirse
mediante un identificador.

WorkManager no resulta adecuado para:

- alarmas exactas;
- conexiones de socket en tiempo real;
- reproducción multimedia;
- seguimiento permanente de la ubicación;
- acciones que el usuario espera que se ejecuten inmediatamente.

El trabajo continuo y visible para el usuario requiere un servicio en primer
plano. Para una hora exacta se utiliza AlarmManager o Exact Alarm API, siempre
que se trate de un caso de uso realmente permitido.

**En resumen:** WorkManager sirve para ejecutar trabajo fiable, diferido y en
segundo plano, sujeto a restricciones y reintentos. No garantiza una hora de
inicio exacta ni sustituye un servicio en primer plano, una conexión en tiempo
real o una alarma exacta.

</details>

<details>
<summary>93. ¿Cuál es la diferencia entre CoroutineScope, lifecycleScope y viewModelScope?</summary>

#### Kotlin

`CoroutineScope` define el contexto y el tiempo de vida de las corrutinas.
`lifecycleScope` y `viewModelScope` son ámbitos de Android preparados de
antemano y con un propietario definido.

```text
CoroutineScope -> su tiempo de vida se define manualmente
lifecycleScope -> vive hasta el estado DESTROYED del LifecycleOwner
viewModelScope -> vive hasta ViewModel.onCleared()
```

### `CoroutineScope`

Es un ámbito general para corrutinas:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Si se crea un ámbito personalizado, debe invocarse explícitamente `cancel()` en
el momento adecuado. De lo contrario, puede producirse una fuga o ejecutarse
trabajo innecesario después de que desaparezca el propietario.

### `lifecycleScope`

Pertenece a una `Activity`, un `Fragment` u otro `LifecycleOwner`, y se cancela
al alcanzar el estado `DESTROYED`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`lifecycleScope.launch` no se pausa por sí solo en el estado `STOPPED`, por lo
que la recolección de Flow requiere `repeatOnLifecycle`.

En un Fragment, para trabajar con su View o binding es preferible utilizar
`viewLifecycleOwner.lifecycleScope`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

La razón es que el objeto Fragment puede vivir más que su View.

### `viewModelScope`

Pertenece al `ViewModel`, sobrevive a los cambios de configuración y se cancela
en `onCleared()`:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Se utiliza para la lógica de la pantalla, la carga de datos, las llamadas de
negocio y la creación del estado de la interfaz.

Algunos errores habituales son:

- utilizar `GlobalScope` para el trabajo de una pantalla;
- crear un ámbito personalizado sin cancelación;
- recolectar el binding o el estado en el ámbito del Fragment en lugar del
  ámbito del ciclo de vida de su View;
- iniciar operaciones de negocio desde la Activity en lugar del ViewModel;
- acceder al binding después de `onDestroyView()`.

**En resumen:** utiliza `viewModelScope` para la lógica de la pantalla dentro del
ViewModel, `lifecycleScope` para recolectar datos y ejecutar efectos de la
interfaz, y un `CoroutineScope` personalizado únicamente cuando tenga un tiempo
de vida claro y un lugar donde invocar `cancel()`.

</details>

<details>
<summary>94. ¿Cómo se implementa la inyección de dependencias en Kotlin?</summary>

#### Kotlin

La inyección de dependencias consiste en proporcionar las dependencias desde el
exterior en lugar de crearlas dentro de una clase. En Kotlin, el enfoque
predeterminado es la inyección mediante el constructor.

La forma correcta:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Una forma incorrecta:

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

La segunda variante acopla estrechamente la clase a una implementación y
dificulta las pruebas.

Por lo general, un caso de uso depende de un contrato:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Inyección manual

Para un proyecto pequeño basta con un contenedor sencillo:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Su ventaja es la sencillez. Su desventaja es que un grafo grande resulta difícil
de mantener manualmente.

### Hilt/Dagger

Proporciona inyección durante la compilación y es una opción habitual para
aplicaciones Android grandes:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val getUser: GetUserUseCase
) : ViewModel()
```

La vinculación de una interfaz:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface UserModule {
    @Binds
    fun bindUserRepository(impl: UserRepositoryImpl): UserRepository
}
```

Su ventaja es que los errores del grafo se detectan durante la compilación. Su
desventaja es una mayor cantidad de código repetitivo y una generación más
compleja.

### Koin

Proporciona inyección en tiempo de ejecución mediante un DSL de Kotlin:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

Su ventaja es que resulta más sencillo comenzar a utilizarlo. Su desventaja es
que algunos errores pueden aparecer durante la ejecución.

La inyección de dependencias también gestiona el tiempo de vida: `singleton`,
`factory` y el ámbito del ViewModel. El ámbito debe elegirse con cuidado para
evitar fugas y singletons innecesarios.

**En resumen:** en Kotlin, la inyección de dependencias suele implementarse
mediante el constructor. La inyección manual resulta adecuada para un grafo
pequeño, Hilt o Dagger para aplicaciones Android grandes y Koin cuando se
prioriza la sencillez. La inyección reduce el acoplamiento y facilita
considerablemente las pruebas.

</details>

<details>
<summary>95. ¿Qué es Hilt y cómo funciona?</summary>

#### Kotlin

`Hilt` es un framework de inyección de dependencias para Android construido
sobre Dagger. Genera un grafo de dependencias y una jerarquía de componentes
preparada para `Application`, `Activity`, `Fragment`, `ViewModel`, `View` y
`Service`.

La configuración básica:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` crea el grafo raíz en el ámbito de la aplicación.

Una clase de Android en la que deben inyectarse dependencias:

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

La inyección mediante el constructor es el método principal:

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

Un ViewModel:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Los módulos son necesarios cuando Hilt no puede crear una dependencia por sí
mismo, como una interfaz, una clase de terceros o un objeto creado mediante un
builder o una factory.

`@Binds` se utiliza para vincular una interfaz con su implementación:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface ProfileModule {
    @Binds
    fun bindProfileRepository(
        impl: ProfileRepositoryImpl
    ): ProfileRepository
}
```

`@Provides` se utiliza para crear objetos mediante una factory o un builder, por
ejemplo, `OkHttpClient`, `Retrofit` o `Room`.

Los ámbitos definen el tiempo de vida de un objeto:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Los componentes principales son:

```text
SingletonComponent        -> aplicación
ActivityRetainedComponent -> sobrevive a los cambios de configuración
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Una regla importante establece que un componente de larga duración no puede
depender de otro de menor duración. Por ejemplo, una dependencia `@Singleton` no
debe conservar el contexto de una `Activity`.

Para las pruebas, Hilt permite sustituir módulos mediante `@TestInstallIn`. Sin
embargo, en las pruebas unitarias suele ser más sencillo proporcionar
directamente un fake mediante el constructor.

**En resumen:** Hilt genera un grafo de Dagger para Android. `@Inject` crea las
dependencias, `@Binds` y `@Provides` describen las vinculaciones,
`@AndroidEntryPoint` habilita la inyección en una clase de Android y los ámbitos
controlan el tiempo de vida.

</details>

<details>
<summary>96. ¿Qué es @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` integra un `ViewModel` con Hilt. Hilt genera una
`ViewModelProvider.Factory` y proporciona las dependencias mediante la inyección
por constructor, por lo que normalmente no se necesita una factory manual.

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val loadUser: LoadUserUseCase,
    private val savedStateHandle: SavedStateHandle
) : ViewModel() {

    private val _state = MutableStateFlow<UiState>(UiState.Loading)
    val state = _state.asStateFlow()

    fun load(id: String) {
        viewModelScope.launch {
            _state.value = UiState.Success(loadUser(id))
        }
    }
}
```

Las reglas son:

- `@HiltViewModel` se aplica a la clase;
- `@Inject constructor` se aplica al constructor;
- todas las dependencias deben estar disponibles en el grafo de Hilt;
- Hilt proporciona `SavedStateHandle` automáticamente;
- la `Activity` o el `Fragment` host debe tener `@AndroidEntryPoint`.

Para obtenerlo en un Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

Para compartir un ViewModel en el ámbito de una Activity:

```kotlin
private val viewModel: UserViewModel by activityViewModels()
```

En Compose:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

El ámbito del ViewModel lo determina `ViewModelStoreOwner`: un Fragment, una
Activity o una entrada del back stack de navegación.

Las dependencias del ViewModel viven en `ViewModelComponent`.
`@ViewModelScoped` proporciona una instancia para un ViewModel concreto, mientras
que `@Singleton` proporciona una instancia para el grafo de la aplicación.

Algunos errores habituales son:

- olvidar `@HiltViewModel` o `@Inject constructor`;
- olvidar `@AndroidEntryPoint` en el host;
- crear el ViewModel manualmente mediante su constructor;
- esperar una única instancia con distintos objetos `ViewModelStoreOwner`;
- inyectar una `Activity`, un `Fragment`, una `View` o un `Context` de corta
  duración en el ViewModel.

**En resumen:** `@HiltViewModel` permite que Hilt cree un ViewModel mediante una
factory generada e inyecte sus dependencias de constructor y `SavedStateHandle`.
El tiempo de vida real lo determina `ViewModelStoreOwner`, no la propia
anotación.

</details>

<details>
<summary>97. ¿Cómo funciona @Inject?</summary>

#### Kotlin

`@Inject` es una anotación que indica a Dagger o Hilt cómo crear un objeto o
dónde insertar una dependencia.

La opción principal es la inyección mediante el constructor:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt podrá crear `LoadUserUseCase` si el grafo contiene una vinculación para cada
parámetro del constructor. Si falta una vinculación o resulta ambigua, se
producirá un error durante la compilación.

La inyección mediante el constructor es preferible para las clases propias:

- las dependencias son explícitas;
- permite utilizar `val`;
- la clase resulta fácil de probar;
- no existe comportamiento oculto del framework.

La inyección de campos se utiliza principalmente para las clases de Android
creadas por el framework:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Un campo de este tipo no puede utilizarse antes de que se realice la inyección
durante el ciclo de vida del componente.

`@Inject constructor` no resuelve por sí solo la vinculación de una interfaz.
Para una interfaz se necesita `@Binds`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {
    @Binds
    abstract fun bindUserRepository(
        implementation: RealUserRepository
    ): UserRepository
}
```

Para clases de terceros u objetos creados mediante builders se utiliza
`@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` no define el tiempo de vida. Este se determina mediante ámbitos como
`@Singleton`, `@ViewModelScoped` y `@ActivityScoped`. Si existen varias
vinculaciones del mismo tipo, se utilizan calificadores.

Hilt y Dagger generan las factories y el grafo durante la compilación, sin
reflexión en tiempo de ejecución para crear las dependencias.

**En resumen:** `@Inject` indica a Hilt o Dagger cómo crear una clase o dónde
insertar una dependencia. La inyección mediante el constructor es la opción
predeterminada; `@Binds` se utiliza para interfaces, `@Provides` para objetos
externos, los ámbitos para el tiempo de vida y los calificadores para distinguir
tipos iguales.

</details>

<details>
<summary>98. ¿Qué son los ámbitos (scopes) en Hilt?</summary>

#### Kotlin

Un ámbito en Hilt determina dentro de qué componente se almacena en caché una
dependencia. Es decir, todas las solicitudes realizadas en una misma instancia
del componente reciben el mismo objeto.

Sin un ámbito, normalmente se crea una vinculación nueva para cada solicitud de
inyección. Este es un comportamiento predeterminado adecuado para objetos sin
estado y económicos de crear.

Una jerarquía simplificada de componentes:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

Un componente secundario puede acceder a las vinculaciones de su componente
principal, pero el principal no puede depender de una vinculación secundaria con
un tiempo de vida más corto.

Los ámbitos principales son:

- `@Singleton`: una instancia por grafo de la aplicación;
- `@ActivityRetainedScoped`: una instancia por Activity lógica, que sobrevive a
  los cambios de configuración;
- `@ViewModelScoped`: una instancia por ViewModel concreto;
- `@ActivityScoped`: una instancia por cada instancia de Activity;
- `@FragmentScoped`: una instancia por Fragment concreto;
- `@ViewScoped`: una instancia por View compatible con Hilt;
- `@ServiceScoped`: una instancia por Service concreto.

El ámbito debe corresponder al componente indicado en `@InstallIn`:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

Una dependencia `@ViewModelScoped` será la misma para un ViewModel concreto, pero
será diferente para otros ViewModels. Si un objeto debe compartirse entre los
ViewModels de una misma Activity, se necesita `@ActivityRetainedScoped`.

Un error habitual consiste en que un objeto de larga duración conserve un
contexto de corta duración:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Esto no debe hacerse: el singleton conservará la Activity después de su
destrucción. Una dependencia `@Singleton` debe utilizar estado o contexto del
nivel de la aplicación.

No es necesario asignar un ámbito a todos los objetos. Un ámbito representa la
semántica de propiedad y ciclo de vida, no una simple optimización.

**En resumen:** un ámbito de Hilt define el tiempo de vida y la reutilización de
una dependencia dentro de un componente. Debe corresponder a su propietario real
y no debe conservar objetos con un ciclo de vida más corto.

</details>

<details>
<summary>99. ¿Cómo se evitan las fugas de memoria en Android?</summary>

#### Kotlin

Se produce una fuga de memoria cuando un objeto deja de ser necesario, pero aún
existe una referencia fuerte hacia él. En Android, las fugas afectan con mayor
frecuencia a objetos `Activity`, `Fragment`, `View`, `Context`, callbacks o
corrutinas.

### No conservar una Activity o View en objetos de larga duración

Una forma incorrecta:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

Si se trata del contexto de una `Activity`, esta no se liberará después de una
rotación o de `finish()`. Para las dependencias del nivel de la aplicación se
utiliza `applicationContext`.

### Limpiar View Binding en un Fragment

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

El objeto Fragment puede vivir más que su View, por lo que las referencias a
esta deben limpiarse en `onDestroyView()`.

### Limpiar adapters, listeners y callbacks

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Los adapters, listeners y callbacks anónimos suelen conservar una referencia a
la View o al Fragment. Deben eliminarse o limpiarse en el callback adecuado del
ciclo de vida.

### Utilizar corrutinas que tengan en cuenta el ciclo de vida

Para recolectar datos de la interfaz de un Fragment:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

No deben iniciarse corrutinas de la interfaz en un ámbito que viva más que la
View. Tampoco debe utilizarse `GlobalScope` para flujos de la interfaz o de la
aplicación.

### Compose

En Compose, los efectos secundarios deben limpiarse mediante
`DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

No debe conservarse una `Activity` o una `View` mediante `remember` sin una razón
clara relacionada con el ciclo de vida.

### Herramientas

- LeakCanary;
- Android Studio Memory Profiler;
- volcados del heap;
- seguimiento de asignaciones.

`WeakReference` no es una solución arquitectónica adecuada. Si parece
«necesario» utilizarlo, primero deben revisarse la propiedad y el ciclo de vida.

**En resumen:** no conserves objetos de Android de corta duración en clases de
larga duración, limpia las referencias a la View de un Fragment en
`onDestroyView()`, utiliza corrutinas que tengan en cuenta el ciclo de vida,
elimina los callbacks y busca fugas mediante LeakCanary o Profiler.

</details>

<details>
<summary>100. ¿Cómo se almacenan de forma segura las claves de API en una aplicación Android?</summary>

#### Kotlin

No es posible almacenar un secreto de forma completamente segura en una
aplicación Android. Todo lo incluido en un APK o AAB puede extraerse
potencialmente mediante descompilación, un proxy, inspección de memoria o un
dispositivo rooteado.

La regla principal es que los secretos reales no deben incluirse en el cliente.

Una forma incorrecta:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Si una clave proporciona acceso al backend, funciones administrativas, pagos o
una cuenta de servicio, debe permanecer exclusivamente en el servidor.

La arquitectura correcta es:

```text
Aplicación Android -> backend propio -> API de terceros
```

El backend añade el secreto, valida al usuario o la sesión, aplica límites de
solicitudes y auditorías, y devuelve únicamente el resultado necesario.

No todas las claves son iguales:

```text
clave pública/de cliente -> puede incluirse en la aplicación, pero debe restringirse
clave secreta/administrativa -> no debe incluirse en la aplicación
```

Si una clave de cliente debe incluirse en la aplicación, debe restringirse
mediante:

- el nombre del paquete;
- la huella SHA-256 del certificado;
- las API permitidas;
- cuotas y límites de solicitudes;
- la separación de entornos;
- la monitorización;
- un plan de rotación.

`BuildConfig`, `local.properties` y los secretos de CI ayudan a evitar que una
clave se confirme en Git, pero no la protegen dentro de la aplicación compilada:

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

Después de descompilar la aplicación, la clave puede quedar visible.

NDK, R8/ProGuard y la ofuscación de cadenas solo dificultan la extracción. Añaden
obstáculos, pero no constituyen un límite de seguridad.

Android Keystore resulta útil para claves generadas en el dispositivo y para
almacenar tokens de usuario, pero no protege un secreto de API incluido
directamente en el código: un atacante puede encontrar el lugar donde la
aplicación utiliza dicho secreto.

Los tokens de acceso y renovación del usuario deben almacenarse con precaución:

- utilizar EncryptedSharedPreferences o un DataStore cifrado;
- utilizar Android Keystore para la clave criptográfica;
- establecer un tiempo de vida corto para el token de acceso;
- rotar el token de renovación;
- limpiar el almacenamiento al cerrar sesión;
- permitir la revocación desde el backend.

**En resumen:** los secretos de API reales deben permanecer en el backend. El
cliente solo puede contener claves públicas o de cliente restringidas.
`BuildConfig`, la ofuscación y NDK no garantizan la seguridad; se necesitan
restricciones, monitorización y rotación.

</details>

<details>
<summary>101. ¿Cómo se implementa una arquitectura offline-first?</summary>

#### Kotlin

Offline-first significa que la interfaz lee una fuente de verdad local y la red
se limita a sincronizarla.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Ruta de lectura

La interfaz se suscribe a Room:

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

La actualización obtiene los datos remotos y los combina con la base de datos
dentro de una transacción:

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

Después de escribir en Room, la interfaz se actualiza mediante `Flow`.
`replaceAll` resulta peligroso si la respuesta es parcial o existen cambios
locales pendientes.

### Ruta de escritura y outbox

Una escritura sin conexión se guarda primero de forma local:

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

La interfaz muestra inmediatamente la actualización optimista. La operación del
outbox se envía más tarde y solo se elimina después de recibir la confirmación
del servidor.

Es importante que la actualización local y la inserción en el outbox formen
parte de la misma transacción; de lo contrario, la finalización del proceso puede
provocar la pérdida de la operación.

### Sincronización

Para admitir reintentos y sobrevivir a la finalización del proceso se utiliza
WorkManager:

```kotlin
override suspend fun doWork(): Result = try {
    repository.syncPending()
    Result.success()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    Result.retry()
}
```

La sincronización debe ser idempotente, ya que un reintento puede repetir la
solicitud. La actualización en primer plano y el worker deben utilizar la misma
implementación de sincronización.

### Conflictos

La política de resolución de conflictos depende del dominio:

- prevalece el servidor;
- prevalece el cliente;
- revisión o ETag;
- combinación por campos;
- resolución manual.

La marca de tiempo del cliente no es fiable debido a las diferencias entre
relojes. En dominios críticos, una política sencilla donde prevalece la última
escritura suele ser inaceptable.

### Estado de la interfaz

La interfaz debe distinguir entre:

- datos almacenados en caché;
- carga inicial sin datos locales;
- actualización con datos anteriores;
- estado sin conexión;
- cambios pendientes o fallidos;
- última sincronización correcta.

La conectividad solo es un indicio. La fuente de verdad es el resultado de una
solicitud real.

**En resumen:** una arquitectura offline-first se construye alrededor de una
fuente de verdad local. Las lecturas proceden de Room, los datos remotos se
combinan con la base de datos, las escrituras utilizan un outbox transaccional,
WorkManager realiza la sincronización y el dominio define la política de
conflictos.

</details>

<details>
<summary>102. ¿Qué es la arquitectura MVI y cuándo debe utilizarse?</summary>

#### Kotlin

MVI es un enfoque arquitectónico basado en un flujo de datos unidireccional:

```text
UI -> Evento/Intent -> ViewModel/Reducer -> Estado -> UI
                                            └-> Efecto
```

La interfaz no modifica el estado directamente. Renderiza un `State` inmutable y
envía un `Event`. El ViewModel procesa el evento y crea un estado nuevo.

Sus componentes habituales son:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)

sealed interface ProfileEvent {
    data object Load : ProfileEvent
    data object Retry : ProfileEvent
    data object Back : ProfileEvent
}

sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowSnackbar(val message: String) : ProfileEffect
}
```

- `State`: modelo duradero de la pantalla;
- `Event` o `Intent`: una acción del usuario o una entrada del sistema;
- `Reducer` o `Handler`: una transición de `estado anterior -> estado nuevo`;
- `Effect`: una acción de un solo uso, como navegación, un snackbar o una
  solicitud de permiso.

El ViewModel:

```kotlin
private val mutableState = MutableStateFlow(ProfileState())
val state = mutableState.asStateFlow()

fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.Load,
        ProfileEvent.Retry -> load()
        ProfileEvent.Back -> navigateBack()
    }
}
```

El estado se actualiza mediante `copy` y `update`:

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

La interfaz de Compose solo lee el estado y envía eventos:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Sus ventajas son:

- una única fuente de verdad para el estado de la pantalla;
- transiciones predecibles;
- pruebas más sencillas del reducer o ViewModel;
- buena adaptación a Compose;
- registro práctico de eventos y estados.

Sus desventajas son:

- mayor cantidad de código repetitivo;
- el reducer puede convertirse en un objeto que concentra demasiadas
  responsabilidades;
- los efectos de un solo uso requieren una política de entrega clara;
- MVI no resuelve automáticamente la concurrencia ni las condiciones de carrera;
- puede resultar innecesario para operaciones CRUD sencillas.

MVI resulta adecuado para pantallas complejas con muchas transiciones de estado,
reintentos, actualizaciones optimistas, validación o flujos de varios pasos. Para
una pantalla sencilla basta con `ViewModel + StateFlow + métodos`.

**En resumen:** MVI es un ciclo de `Evento -> Transición de estado -> Renderizado`.
Resulta útil para interfaces complejas con numerosos estados, pero exige
disciplina con los efectos y la concurrencia, y puede ser excesivo para pantallas
sencillas.

</details>

<details>
<summary>103. ¿Cómo se diseñan aplicaciones Android escalables?</summary>

#### Kotlin

Una aplicación Android escalable permite modificar una funcionalidad de forma
local sin provocar cambios en cascada por todo el código.

Las capas básicas son:

```text
UI -> ViewModel -> dominio -> repositorio -> API/BD
```

- la interfaz renderiza el estado;
- el ViewModel gestiona el estado y los eventos de la pantalla;
- el dominio contiene las reglas de negocio;
- el repositorio oculta las fuentes de datos;
- la capa de datos trabaja con Retrofit, Room o SDK.

Las capas no deben añadirse mecánicamente. Un caso de uso o una interfaz son
necesarios cuando existe una regla de negocio, un límite, una implementación
alternativa o una necesidad de prueba independiente.

### Módulos organizados por funcionalidad

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Un módulo de funcionalidad agrupa el código que cambia conjuntamente. `core`
debe contener infraestructura realmente compartida, no utilidades aleatorias.

La regla de dependencias:

```text
app -> feature
feature -> core
core -X-> feature
```

Una funcionalidad no debe depender de la implementación de otra. Para
integrarlas es preferible utilizar contratos, una API de navegación o separar la
API de su implementación.

### Contratos y modelos

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Una interfaz es necesaria en un límite real. No debe crearse una interfaz para
cada clase «por si acaso».

DTO, Entity, Domain y UiModel se separan cuando tienen semánticas o ciclos de
vida diferentes. Si los modelos son iguales y no tienen motivos distintos para
cambiar, un mapeo adicional solo resulta perjudicial.

### Estado, errores y concurrencia

La interfaz debe recibir un único estado observable y enviar eventos. El
ViewModel debe definir explícitamente su política de concurrencia: cancelar la
operación anterior, ignorar duplicados, serializar o combinar.

Un `Throwable` sin procesar no debe formar parte del contrato de la interfaz. Los
errores de infraestructura deben convertirse en resultados de dominio o de
interfaz en el límite correspondiente.

### Inyección, pruebas y compilación

- utilizar la inyección mediante el constructor de forma predeterminada;
- hacer que los ámbitos correspondan al propietario del ciclo de vida;
- ejecutar pruebas unitarias y de dominio sin Android;
- probar el estado del ViewModel;
- crear pruebas de integración para los repositorios;
- utilizar plugins de convenciones y un catálogo de versiones;
- ejecutar CI para los módulos afectados;
- incluir registros, métricas y contexto de fallos;
- disponer de un plan de migración para cambios de API o base de datos.

La modularidad sin pruebas, observabilidad y una responsabilidad claramente
definida no permite escalar el producto.

**En resumen:** la escalabilidad procede de una responsabilidad clara sobre las
funcionalidades, un grafo de dependencias controlado, contratos estables, límites
comprobables, estado y concurrencia explícitos, y soporte para producción. Los
módulos y las capas solo deben añadirse cuando realmente aíslen los cambios.

</details>

<details>
<summary>104. ¿Qué es Jetpack Compose y en qué se diferencia de View System?</summary>

#### Kotlin

`Jetpack Compose` es un toolkit declarativo para crear interfaces de usuario en
Android. La interfaz se describe mediante funciones `@Composable` de Kotlin como
resultado del estado actual, sin XML ni actualizaciones manuales de objetos
`View`.

View System utiliza un enfoque imperativo:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

Compose utiliza un enfoque declarativo:

```kotlin
@Composable
fun ProfileScreen(state: ProfileState) {
    if (state.isLoading) {
        CircularProgressIndicator()
    } else {
        Text(state.userName)
    }
}
```

Cuando cambia el estado, Compose inicia la recomposición de las partes necesarias
de la interfaz.

Una función composable:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Un composable debe renderizar el estado e invocar callbacks. La lógica de
negocio, la E/S y las operaciones prolongadas no deben residir en las funciones
de la interfaz.

Una estructura habitual:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val name: String = ""
)

@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsStateWithLifecycle()
    ProfileScreen(state = state)
}
```

El ViewModel crea el estado y Compose lo representa.

Las principales diferencias son:

```text
View System -> XML + árbol de Views + actualizaciones manuales + listeners
Compose     -> funciones Kotlin + renderizado basado en estado + recomposición
```

Algunos aspectos prácticos de Compose:

- el cuerpo de un composable puede ejecutarse muchas veces;
- los efectos secundarios deben iniciarse mediante las API de efectos;
- el estado y los objetos locales deben almacenarse mediante `remember` cuando
  sea necesario;
- el estado de la pantalla debe conservarse en el ViewModel;
- las listas perezosas deben tener claves estables;
- el rendimiento debe comprobarse mediante un profiler, no contando
  recomposiciones.

Compose y View System pueden combinarse:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

También puede insertarse una View en Compose mediante `AndroidView`. Por tanto,
la migración puede realizarse gradualmente.

**En resumen:** Compose describe la interfaz como una función del estado y la
actualiza mediante recomposición. View System requiere XML, un árbol de Views y
modificaciones manuales. Compose facilita una interfaz dirigida por el estado,
pero exige disciplina con los efectos secundarios y la propiedad del estado.

</details>

<details>
<summary>105. ¿Qué es @Composable y qué es la recomposición?</summary>

#### Kotlin

`@Composable` es una anotación para las funciones de Jetpack Compose que
describen la interfaz. La recomposición es una nueva ejecución de un composable
cuando cambia el estado del que depende.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Un composable es una función del estado:

```kotlin
@Composable
fun ProfileScreen(state: ProfileState) {
    if (state.isLoading) {
        CircularProgressIndicator()
    } else {
        Text(state.userName)
    }
}
```

Cuando cambia `state`, Compose actualiza mediante recomposición la parte
necesaria de la interfaz.

Un ejemplo de estado local:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

El cambio de `count` inicia la recomposición de la parte de la interfaz que lee
`count`.

La recomposición puede iniciarse mediante:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable`;
- un cambio en los parámetros del composable.

El cuerpo de un composable puede ejecutarse muchas veces, por lo que no debe
contener efectos secundarios:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // Incorrecto
}
```

La forma correcta:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` conserva un valor entre recomposiciones:

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

La recomposición no constituye un problema por sí misma. Los problemas son el
trabajo pesado dentro de un composable, la lectura demasiado amplia del estado,
los parámetros inestables, las asignaciones innecesarias o las listas perezosas
sin claves estables.

Algunas reglas prácticas:

- el composable debe ser ligero y no tener efectos secundarios;
- el estado debe leerse cerca del lugar donde se utiliza;
- los objetos costosos deben almacenarse mediante `remember`;
- los efectos deben iniciarse mediante `LaunchedEffect`, `DisposableEffect` u
  otras API similares;
- las listas deben utilizar claves estables;
- el rendimiento debe comprobarse con un profiler, no mediante suposiciones.

**En resumen:** `@Composable` describe la interfaz como una función del estado.
La recomposición vuelve a ejecutar los composables necesarios cuando cambia el
estado. Es un mecanismo normal de Compose si las funciones de la interfaz son
ligeras, no tienen efectos secundarios y leen el estado en el lugar adecuado.

</details>

<details>
<summary>106. ¿Qué es la elevación del estado (state hoisting)?</summary>

#### Kotlin

La elevación del estado consiste en trasladar el estado de un composable a su
elemento padre o a un contenedor de estado. El elemento hijo recibe el valor y un
callback, por lo que queda sin estado, resulta reutilizable y puede probarse con
facilidad.

Una variante con estado:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Después de elevar el estado:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

El elemento padre se convierte en la fuente de verdad:

```kotlin
@Composable
fun SearchScreen() {
    var query by rememberSaveable { mutableStateOf("") }
    SearchField(
        query = query,
        onQueryChange = { query = it }
    )
}
```

Para el estado de una pantalla, el propietario suele ser un `ViewModel`:

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

La ruta de Compose lee el estado y transmite los eventos:

```kotlin
@Composable
fun SearchRoute(viewModel: SearchViewModel) {
    val state by viewModel.state.collectAsStateWithLifecycle()
    SearchScreen(
        state = state,
        onQueryChange = viewModel::onQueryChange
    )
}
```

La elevación del estado favorece un flujo de datos unidireccional:

```text
estado hacia abajo -> UI
eventos hacia arriba -> el propietario actualiza el estado
```

El estado debe elevarse hasta el propietario común más cercano que necesite
leerlo o modificarlo. Si solo se necesita dentro de un componente y no afecta a
la lógica de negocio, puede mantenerse local.

Una API habitual para un composable sin estado:

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**En resumen:** state hoisting significa «estado hacia abajo, eventos hacia
arriba». El elemento hijo no es propietario del estado; el elemento padre o el
ViewModel se convierte en la única fuente de verdad.

</details>

<details>
<summary>107. ¿Cuál es la diferencia entre remember y rememberSaveable?</summary>

#### Kotlin

`remember` y `rememberSaveable` conservan valores entre recomposiciones. La
diferencia es que `rememberSaveable` también guarda el valor en el estado
guardado y puede restaurarlo después de un cambio de configuración o de la
recreación del proceso.

```text
remember         -> sobrevive a la recomposición
rememberSaveable -> sobrevive a la recomposición y la recreación
```

`remember`:

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

El valor se perderá después de recrear la Activity.

`rememberSaveable`:

```kotlin
@Composable
fun Counter() {
    var count by rememberSaveable { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

En Android se admiten automáticamente los tipos que pueden guardarse en el
`Bundle` del estado guardado: tipos primitivos, `String`, `Parcelable`,
`Serializable`, entre otros. Para un tipo personalizado se necesita un `Saver`:

```kotlin
data class FilterState(val query: String, val selected: Int)

val FilterSaver = listSaver<FilterState, Any>(
    save = { listOf(it.query, it.selected) },
    restore = { FilterState(it[0] as String, it[1] as Int) }
)

var filter by rememberSaveable(stateSaver = FilterSaver) {
    mutableStateOf(FilterState("", 0))
}
```

Cuándo utilizar cada opción:

- `remember`: estado transitorio de la interfaz u objeto almacenado en caché que
  puede perderse;
- `rememberSaveable`: estado pequeño de la interfaz que el usuario espera
  conservar, como texto introducido, una pestaña seleccionada o un filtro;
- `ViewModel`: estado de la pantalla o de negocio;
- almacenamiento, DataStore o base de datos: datos persistentes.

Las limitaciones de `rememberSaveable` son:

- el estado guardado tiene un tamaño limitado;
- no deben guardarse objetos ni colecciones grandes;
- no proporciona persistencia de larga duración;
- no sustituye un `ViewModel` ni un repositorio.

**En resumen:** `remember` solo vive dentro de la composición.
`rememberSaveable` restaura un pequeño estado guardable de la interfaz después
de una recreación. El estado de la pantalla pertenece al `ViewModel` y los datos
duraderos, a la capa de almacenamiento.

</details>

<details>
<summary>108. ¿Qué es rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` es una API de Compose que devuelve un
`CoroutineScope` vinculado a la composición actual. El ámbito se cancela cuando
el composable abandona la composición.

Se utiliza cuando una acción `suspend` de la interfaz debe iniciarse desde un
callback, como un clic, un gesto de deslizamiento, un drawer, un snackbar o una
animación de desplazamiento.

```kotlin
@Composable
fun SnackbarButton(snackbarHostState: SnackbarHostState) {
    val scope = rememberCoroutineScope()

    Button(
        onClick = {
            scope.launch {
                snackbarHostState.showSnackbar("Saved")
            }
        }
    ) {
        Text("Save")
    }
}
```

Una función `suspend` no puede invocarse directamente desde `onClick`, por lo que
se necesita un ámbito.

Otro ejemplo:

```kotlin
@Composable
fun UsersList(users: List<UserUi>) {
    val listState = rememberLazyListState()
    val scope = rememberCoroutineScope()

    Button(onClick = {
        scope.launch { listState.animateScrollToItem(0) }
    }) {
        Text("Scroll to top")
    }

    LazyColumn(state = listState) {
        items(users) { user -> UserRow(user) }
    }
}
```

La diferencia con `LaunchedEffect`:

```text
LaunchedEffect         -> la corrutina se inicia automáticamente al entrar o cambiar la clave
rememberCoroutineScope -> la corrutina se inicia manualmente desde un callback
```

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

```kotlin
Button(onClick = {
    scope.launch { drawerState.open() }
})
```

No debe utilizarse `rememberCoroutineScope()` para la lógica de negocio:

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

La implementación correspondiente en el ViewModel:

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

No debe utilizarse `GlobalScope` para la interfaz, ya que no está vinculado al
ciclo de vida y puede continuar ejecutándose después de cerrar la pantalla.

**En resumen:** `rememberCoroutineScope()` se utiliza para operaciones `suspend`
breves de la interfaz iniciadas desde callbacks, como un snackbar, un drawer o
un desplazamiento. Para efectos automáticos se utiliza `LaunchedEffect` y para la
lógica de la pantalla o de negocio, `viewModelScope`.

</details>

<details>
<summary>109. ¿Qué es collectAsState?</summary>

#### Kotlin

`collectAsState()` es una API de Compose que recolecta un `Flow` y lo convierte
en un `State` de Compose. Cuando Flow emite un valor nuevo, Compose inicia una
recomposición en los lugares donde se lee dicho estado.

El ViewModel:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state.asStateFlow()
}
```

Compose:

```kotlin
@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsState()
    ProfileScreen(state = state)
}
```

El funcionamiento:

```text
emisión de Flow -> actualización de State -> recomposición -> actualización de la UI
```

Un `Flow` convencional necesita un valor inicial:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

`StateFlow` ya dispone de un valor inicial en `state.value`.

En Android es preferible utilizar la variante que tiene en cuenta el ciclo de
vida:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

La diferencia:

```text
collectAsState              -> runtime de Compose, sin ciclo de vida de Android
collectAsStateWithLifecycle -> recolecta solo durante un estado activo del ciclo de vida
```

Un Flow no debe recolectarse directamente en el cuerpo de un composable:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // Incorrecto
}
```

Para el estado de una pantalla se utiliza `collectAsStateWithLifecycle()`. Para
eventos de un solo uso se emplean `LaunchedEffect` y un `SharedFlow` independiente:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // Navegar o mostrar un snackbar
    }
}
```

Un `State` representa algo que puede volver a renderizarse. Un `Event` representa
una acción de un solo uso.

Si Flow emite valores con demasiada frecuencia, se debe reducir el ruido mediante
`distinctUntilChanged()`, modelos de estado más pequeños y lecturas locales del
estado, además de evitar el trabajo pesado dentro de un composable.

**En resumen:** `collectAsState()` convierte Flow en State de Compose. En Android
suele utilizarse `collectAsStateWithLifecycle()`. Los eventos de navegación o
snackbar de un solo uso requieren `LaunchedEffect`, no la recolección como
estado.

</details>

<details>
<summary>110. ¿Qué es Modifier en Compose?</summary>

#### Kotlin

`Modifier` describe en Compose el layout, el dibujo, la entrada, la semántica y
el comportamiento de un composable: `padding`, `size`, `background`, `clickable`,
desplazamiento, accesibilidad, entre otros.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

`Modifier` es inmutable y se construye como una cadena. El orden es importante:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

y:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

producen resultados diferentes, porque el fondo se aplica en etapas distintas.

Un composable reutilizable debe aceptar un modifier desde el exterior:

```kotlin
@Composable
fun UserAvatar(
    url: String,
    modifier: Modifier = Modifier
) {
    AsyncImage(
        model = url,
        contentDescription = null,
        modifier = modifier.size(48.dp)
    )
}
```

De este modo, el elemento padre puede controlar el posicionamiento y el
comportamiento externo del hijo.

Algunos tipos principales de modificadores:

```kotlin
Modifier
    .fillMaxWidth()
    .height(56.dp)
    .padding(horizontal = 16.dp)
```

```kotlin
Modifier
    .clip(RoundedCornerShape(12.dp))
    .background(Color.LightGray)
    .border(1.dp, Color.Gray)
```

```kotlin
Modifier.clickable { onClick() }
```

Para la accesibilidad es preferible utilizar modificadores de alto nivel como
`clickable`, ya que añaden semántica. Cuando sea necesario, esta puede definirse
explícitamente:

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Algunos modificadores solo funcionan dentro del ámbito del elemento padre:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Algunas reglas prácticas:

- aceptar `modifier: Modifier = Modifier`;
- hacer que el elemento padre controle el posicionamiento externo;
- permitir que el hijo añada modificadores internos después del modifier
  recibido;
- respetar el orden de los modificadores;
- evitar lógica pesada en los callbacks de un modifier.

**En resumen:** `Modifier` es una cadena inmutable de configuraciones para un
composable. Controla el layout, el dibujo, la entrada y la semántica. El orden de
las llamadas es importante y los composables reutilizables deben aceptar
`modifier` como parámetro.

</details>

<details>
<summary>111. ¿Cuál es la diferencia entre padding y offset?</summary>

#### Kotlin

`padding` y `offset` modifican la posición de la interfaz, pero actúan en etapas
distintas del layout.

```text
padding -> modifica la medición y el espacio reservado
offset  -> modifica la posición sin cambiar el tamaño informado
```

### `padding`

```kotlin
Box(
    modifier = Modifier
        .background(Color.Gray)
        .padding(16.dp)
) {
    Text("Hello")
}
```

`padding` añade espacio alrededor del contenido e influye en el tamaño que ven
el elemento padre y los elementos hermanos.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` se colocará después de `A`, incluido su padding.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` desplaza el composable durante su colocación, pero el elemento padre
reserva el espacio original.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` no se desplazará, por lo que los elementos pueden superponerse. Esto resulta
útil para badges, gestos de arrastre, animaciones o desplazamientos decorativos.

### RTL

`offset(x, y)` tiene en cuenta la dirección del layout: un valor `x` positivo
desplaza hacia el final lógico. Si se necesitan coordenadas físicas sin
reflejarse en RTL, se utiliza `absoluteOffset()`.

### Orden de los modificadores

El orden afecta al layout, al dibujo y al área interactiva:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

El fondo cubrirá un área mayor.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

El fondo solo se aplicará después del padding.

Lo mismo ocurre con `clickable`: si se encuentra antes de `padding`, el padding
forma parte del área interactiva; si se encuentra después, no.

**En resumen:** `padding` añade espacio al layout e influye en los elementos
hermanos. `offset` solo desplaza el composable sin reservar espacio nuevo. El
orden de los modificadores afecta considerablemente al aspecto y a la detección
de interacciones.

</details>

<details>
<summary>112. ¿Cuál es la diferencia entre fillMaxSize y matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` y `matchParentSize()` pueden hacer que un elemento hijo tenga el
tamaño de su padre, pero funcionan de manera diferente.

```text
fillMaxSize()     -> toma las restricciones máximas e influye en la medición
matchParentSize() -> reproduce el tamaño ya determinado del Box
```

### `fillMaxSize()`

```kotlin
Column(
    modifier = Modifier
        .fillMaxSize()
        .padding(16.dp)
) {
    Text("Home")
}
```

`fillMaxSize()` solicita al elemento padre el máximo tamaño disponible. Si las
restricciones están limitadas por la pantalla, el composable ocupará toda la
pantalla.

Para una sola dimensión existen:

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` participa en la medición y puede influir en el tamaño del layout
padre.

### `matchParentSize()`

`matchParentSize()` solo funciona dentro de un `BoxScope` y únicamente para un
hijo directo de `Box`:

```kotlin
Box {
    AsyncImage(
        model = imageUrl,
        contentDescription = null
    )

    if (isLoading) {
        Box(
            modifier = Modifier
                .matchParentSize()
                .background(Color.Black.copy(alpha = 0.4f)),
            contentAlignment = Alignment.Center
        ) {
            CircularProgressIndicator()
        }
    }
}
```

Este hijo reproduce el tamaño ya calculado del `Box`, pero no lo determina.

De forma simplificada:

1. `Box` mide sus hijos convencionales;
2. determina su propio tamaño;
3. los hijos con `matchParentSize()` reciben ese tamaño ya calculado.

Por ello, `matchParentSize()` resulta adecuado para una capa superpuesta, como
una capa de carga, un scrim, un degradado o una capa interactiva.

Si la capa superpuesta se cambia por `fillMaxSize()`, el hijo puede solicitar el
máximo de las restricciones y aumentar el tamaño del `Box`. Este comportamiento
es diferente.

Una elección práctica:

- si el elemento raíz o contenedor debe ocupar todo el espacio disponible,
  utiliza `fillMaxSize()`;
- si una capa debe cubrir un `Box` cuyo tamaño depende del contenido, utiliza
  `matchParentSize()`;
- si el hijo no debe influir en el tamaño del `Box`, utiliza
  `matchParentSize()`;
- fuera de `BoxScope`, `matchParentSize()` no está disponible.

**En resumen:** `fillMaxSize()` solicita el máximo espacio disponible y participa
en la medición. `matchParentSize()` dentro de un `BoxScope` solo reproduce el
tamaño ya determinado del `Box` y no lo aumenta.

</details>

<details>
<summary>113. ¿Cómo se gestiona el botón «Atrás» en Compose?</summary>

#### Kotlin

En Compose, la acción de retroceso se gestiona mediante `BackHandler` de
`androidx.activity.compose`. Este intercepta el retroceso del sistema dentro de
un composable.

Un ejemplo básico:

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // Contenido
}
```

`BackHandler` puede habilitarse de forma condicional:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Si `enabled = false`, el evento de retroceso continuará hacia otro handler o
hacia Navigation.

Un caso habitual es mostrar un diálogo de confirmación cuando existen cambios
sin guardar:

```kotlin
var showDialog by rememberSaveable { mutableStateOf(false) }

BackHandler(enabled = hasUnsavedChanges) {
    showDialog = true
}

if (showDialog) {
    AlertDialog(
        onDismissRequest = { showDialog = false },
        confirmButton = {
            TextButton(onClick = onDiscard) { Text("Discard") }
        },
        dismissButton = {
            TextButton(onClick = { showDialog = false }) { Text("Cancel") }
        },
        title = { Text("Discard changes?") }
    )
}
```

Con `NavController`, el retroceso convencional suele funcionar automáticamente
mediante el back stack. Se necesita un `BackHandler` personalizado cuando debe
modificarse el comportamiento estándar, por ejemplo, para un diálogo, un bottom
sheet, un modo de selección o cambios sin guardar.

Para el estado local de la interfaz, el retroceso debe cerrar primero la capa
superior:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

El orden de prioridad es:

```text
diálogo/sheet/selección -> cambios sin guardar -> navegación de la pantalla
```

Para una lógica compleja es preferible enviar un evento al ViewModel:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

El ViewModel decide si debe mostrar un diálogo, guardar un borrador, permitir la
navegación o emitir un efecto.

Algunos errores habituales son:

- interceptar siempre la acción de retroceso;
- no utilizar `enabled`;
- duplicar la lógica entre `BackHandler` y `NavController`;
- no tener en cuenta el estado de un diálogo o sheet;
- mantener la lógica de retroceso lejos del estado que procesa.

**En resumen:** en Compose, la acción de retroceso se gestiona mediante
`BackHandler(enabled = ...)`. Conviene utilizarlo para las capas superiores de la
interfaz y comportamientos no estándar, y dejar normalmente la navegación
convencional en manos de `NavController`.

</details>

<details>
<summary>114. ¿Para qué se utiliza rememberUpdatedState?</summary>

#### Kotlin

`rememberUpdatedState()` proporciona a un efecto de larga duración el valor o
callback más reciente sin reiniciar dicho efecto.

El problema:

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` no se reinicia si cambia `onTimeout`. La corrutina puede
invocar una lambda desactualizada.

La solución:

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    val currentOnTimeout by rememberUpdatedState(onTimeout)

    LaunchedEffect(Unit) {
        delay(2_000)
        currentOnTimeout()
    }
}
```

El temporizador no vuelve a comenzar, pero después del retraso se invoca el
callback más reciente.

La regla principal:

```text
¿un cambio del valor debe reiniciar el efecto? -> key
¿un cambio del valor no debe reiniciar el efecto? -> rememberUpdatedState
```

Si el callback se utiliza como clave:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

al cambiar la lambda se cancela la corrutina anterior y el temporizador vuelve a
comenzar. Este comportamiento no siempre es deseable.

Un ejemplo con `DisposableEffect`:

```kotlin
val currentOnEvent by rememberUpdatedState(onEvent)
val owner = LocalLifecycleOwner.current

DisposableEffect(owner) {
    val observer = LifecycleEventObserver { _, event ->
        currentOnEvent(event)
    }

    owner.lifecycle.addObserver(observer)
    onDispose { owner.lifecycle.removeObserver(observer) }
}
```

El observador solo vuelve a registrarse cuando cambia `owner`, pero invoca el
handler más reciente.

Un caso donde el valor debe ser una clave:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Un cambio de `userId` representa una operación nueva, por lo que la anterior
debe cancelarse y debe iniciarse otra.

`rememberUpdatedState()` no inicia una corrutina, no sustituye las claves de los
efectos ni conserva el valor después de abandonar la composición. Únicamente
devuelve un `State<T>` con el valor actual para un efecto de larga duración.

**En resumen:** `rememberUpdatedState()` permite que un efecto acceda al valor o
callback más reciente sin reiniciarse. Si un cambio de valor representa una
operación nueva, dicho valor debe utilizarse como clave, no mediante
`rememberUpdatedState`.

</details>

<details>
<summary>115. ¿Qué son los efectos secundarios en Compose y cómo se gestionan?</summary>

#### Kotlin

Un efecto secundario en Compose es una acción ajena al renderizado puro de la
interfaz, como una corrutina, un snackbar, la navegación, datos analíticos, un
listener, un observador del ciclo de vida o la sincronización con una API externa.

El cuerpo de un composable puede ejecutarse muchas veces, por lo que un efecto
secundario no debe iniciarse directamente:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // Incorrecto
}
```

Para ello existen las API de efectos.

### `LaunchedEffect`

Inicia una corrutina vinculada a la composición:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Si cambia `userId`, se cancela la corrutina anterior y se inicia una nueva.

### `rememberCoroutineScope`

Se utiliza para acciones `suspend` de la interfaz iniciadas desde un callback:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch {
        snackbarHostState.showSnackbar("Saved")
    }
}) {
    Text("Save")
}
```

Resulta adecuado para un snackbar, un drawer, un bottom sheet o una animación de
desplazamiento.

### `DisposableEffect`

Se utiliza para listeners y observadores que requieren limpieza:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

Si se crea una suscripción, debe eliminarse en `onDispose`.

### `SideEffect`

Se ejecuta después de una recomposición correcta:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

No resulta adecuado para trabajo `suspend`.

### Otras API

- `rememberUpdatedState`: proporciona el último valor o callback dentro de un
  efecto de larga duración sin reiniciarlo;
- `produceState`: convierte una fuente asíncrona en `State` de Compose;
- `snapshotFlow`: convierte el estado de Compose en Flow.

Un mapa práctico:

```text
corrutina de la composición       -> LaunchedEffect
corrutina iniciada desde callback -> rememberCoroutineScope
listener con limpieza             -> DisposableEffect
sincronización tras recomposición -> SideEffect
último callback sin reinicio      -> rememberUpdatedState
fuente asíncrona a State          -> produceState
estado de Compose a Flow          -> snapshotFlow
```

**En resumen:** los efectos secundarios no se inician en el cuerpo de un
composable. Se utilizan las API de efectos para controlar explícitamente su ciclo
de vida, reinicio, cancelación y limpieza.

</details>

<details>
<summary>116. ¿Qué son las claves (keys) de LaunchedEffect y por qué son importantes?</summary>

#### Kotlin

Las claves de `LaunchedEffect` determinan el ciclo de vida de su corrutina:
cuándo iniciarla, cancelarla y reiniciarla. Si cambia una clave, Compose cancela
la corrutina anterior e inicia una nueva.

```kotlin
@Composable
fun UserScreen(
    userId: String,
    viewModel: UserViewModel
) {
    LaunchedEffect(userId) {
        viewModel.loadUser(userId)
    }
}
```

Su comportamiento es:

- cuando el composable entra en la composición, el efecto se inicia;
- una recomposición sin cambios en `userId` no reinicia el efecto;
- si cambia `userId`, se cancela la corrutina anterior y se inicia una nueva;
- cuando el composable abandona la composición, la corrutina se cancela.

Un error habitual:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Si cambia `userId`, el efecto no se reiniciará y pueden obtenerse datos
desactualizados. La forma correcta es:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` es adecuado cuando el efecto debe iniciarse una sola vez
al entrar en la composición:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // Navegación o snackbar
    }
}
```

También pueden utilizarse varias claves:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

El efecto se reiniciará si cambia al menos una clave. Todo aquello que determine
el resultado del efecto secundario debe formar parte de las claves.

No deben utilizarse claves inestables:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Esto provocará reinicios incontrolados del efecto.

Si se necesita el valor más reciente dentro del efecto sin reiniciarlo, se
utiliza `rememberUpdatedState`:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Una regla práctica:

```text
si un cambio del valor debe reiniciar el efecto -> key
si se necesita el último valor sin reiniciar    -> rememberUpdatedState
```

**En resumen:** las claves de `LaunchedEffect` controlan el inicio, la
cancelación y el reinicio de la corrutina. Unas claves incorrectas provocan datos
desactualizados, reinicios innecesarios o un comportamiento inestable de la
interfaz.

</details>

<details>
<summary>117. ¿Qué es snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` es una API de Compose que convierte las lecturas del estado
snapshot de Compose en un `Flow` frío. Se utiliza cuando los cambios del estado
de Compose deben procesarse mediante operadores de Flow o un pipeline de efectos
secundarios.

Un ejemplo básico:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` observa las lecturas del estado de Compose dentro de la lambda.
Cuando cambia el valor leído, Flow emite un valor nuevo.

Algunos casos de uso habituales son:

- datos analíticos del desplazamiento;
- activación de la paginación;
- `debounce` o `sample` para el estado de la interfaz;
- registro y depuración;
- efectos secundarios que no pueden iniciarse en el cuerpo de un composable.

Un ejemplo de paginación:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.layoutInfo.visibleItemsInfo.lastOrNull()?.index }
        .filterNotNull()
        .distinctUntilChanged()
        .collect { lastVisibleIndex ->
            if (lastVisibleIndex > items.size - 5) {
                viewModel.loadNextPage()
            }
        }
}
```

Como el resultado es un `Flow`, pueden utilizarse sus operadores:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

Por lo general, `snapshotFlow` se recolecta dentro de `LaunchedEffect`. La
recolección no debe iniciarse directamente en el cuerpo de un composable.

No debe utilizarse `snapshotFlow` para el renderizado convencional de la
interfaz:

```kotlin
Text(text = state.title)
```

Si solo se necesita mostrar el estado, debe leerse directamente. `snapshotFlow`
se utiliza específicamente para pasar del estado de Compose a Flow o a lógica de
efectos secundarios.

Un matiz importante es que la lambda debe leer estado snapshot de Compose. Si no
lo hace, `snapshotFlow` no tendrá nada que observar.

**En resumen:** `snapshotFlow` convierte lecturas del estado de Compose en
`Flow`. Resulta útil para datos analíticos, paginación, `debounce` y efectos
secundarios dentro de `LaunchedEffect`, pero no es necesario para renderizar
simplemente la interfaz.

</details>

<details>
<summary>118. ¿Cómo se optimiza la recomposición en Compose?</summary>

#### Kotlin

Optimizar la recomposición no significa «eliminar todas las recomposiciones»,
sino hacerlas económicas y locales. La recomposición es normal; el problema es
el trabajo pesado o un área de actualización demasiado amplia.

### Leer el estado cerca del lugar donde se utiliza

Una forma incorrecta:

```kotlin
ProfileScreen(state = hugeState)
```

Una forma mejor:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Si un composable lee un estado, puede recomponerse cuando dicho estado cambia.
No debe proporcionarse todo el estado de la pantalla cuando solo se necesita un
campo.

### Listas perezosas: claves y contentType

```kotlin
LazyColumn {
    items(
        items = users,
        key = { it.id }
    ) { user ->
        UserRow(user)
    }
}
```

Las claves estables ayudan a Compose a identificar correctamente los elementos
y conservar el estado de cada uno.

Para diferentes tipos de elementos:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### No realizar trabajo pesado dentro de un composable

Una forma incorrecta:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Una forma mejor:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Si el texto formateado forma parte del modelo de interfaz, es aún mejor
prepararlo en un mapper o en el ViewModel.

### Utilizar `derivedStateOf` para cambios frecuentes

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Resulta útil cuando el estado de origen cambia con frecuencia, pero la interfaz
solo debe reaccionar a una condición derivada.

### Modelos estables e inmutables

Compose funciona mejor con modelos de interfaz inmutables:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Las colecciones mutables y los objetos inestables pueden impedir que Compose
omita recomposiciones innecesarias.

### Efectos secundarios únicamente mediante las API de efectos

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

El cuerpo de un composable puede ejecutarse muchas veces, por lo que los datos
analíticos, la navegación, los listeners y el trabajo `suspend` no deben
ejecutarse directamente en él.

### Comprobar mediante perfiles

- recuentos de recomposición en Layout Inspector;
- métricas del compilador de Compose;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

**En resumen:** la optimización de la recomposición se basa en lecturas locales
del estado, composables más pequeños, claves estables, `contentType`, modelos de
interfaz inmutables, `remember`, `derivedStateOf`, efectos correctos y perfiles.
El objetivo son actualizaciones locales económicas, no eliminar todas las
recomposiciones.

</details>

<details>
<summary>119. ¿Cómo se depuran los problemas de recomposición?</summary>

#### Kotlin

Los problemas de recomposición se depuran mediante mediciones, no suposiciones.
El objetivo es encontrar qué estado cambia con frecuencia y a qué parte de la
interfaz afecta.

### Layout Inspector

Android Studio Layout Inspector muestra los recuentos de recomposición de los
composables. Sin embargo, la cantidad no constituye un problema por sí sola: un
composable ligero puede recomponerse con frecuencia sin producir un impacto
visible.

### Registros temporales

Para realizar una comprobación rápida puede añadirse un registro:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

También puede utilizarse `SideEffect`:

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

Esto es únicamente una herramienta de depuración, no código para producción.

### Comprobar las lecturas del estado

Una causa habitual es que un composable lea un estado demasiado amplio:

```kotlin
UserRow(screenState = state)
```

Una forma mejor:

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

El estado debe leerse cerca del lugar donde se utiliza.

### Comprobar las listas y los parámetros

Una `LazyColumn` debe utilizar claves estables:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { it.id }
    ) { user ->
        UserRow(user)
    }
}
```

También deben comprobarse los parámetros inestables: colecciones mutables,
objetos o lambdas que se crean de nuevo sin necesidad, y trabajo pesado dentro
del cuerpo de un composable.

### Métricas del compilador y profiler

Las métricas del compilador de Compose ayudan a identificar qué composables o
tipos son inestables, reiniciables u omitibles. Android Studio Profiler y
Macrobenchmark permiten confirmar el impacto real sobre el rendimiento.

Una lista práctica de comprobaciones:

- revisar los recuentos en Layout Inspector;
- encontrar el estado que cambia con frecuencia;
- limitar las lecturas del estado;
- comprobar las claves estables de las listas perezosas;
- eliminar el trabajo pesado del cuerpo;
- comprobar los parámetros inestables;
- utilizar `remember` o `derivedStateOf` únicamente cuando exista un motivo;
- confirmar el resultado mediante el profiler y las métricas.

**En resumen:** la recomposición se depura mediante Layout Inspector, registros,
métricas del compilador y un profiler. Las causas más habituales son lecturas
demasiado amplias del estado, parámetros inestables, ausencia de claves, trabajo
pesado en el cuerpo o una propiedad incorrecta del estado.

</details>

<details>
<summary>120. ¿Cómo se pueden evitar los bloqueos en Compose, por ejemplo, durante el desplazamiento?</summary>

#### Kotlin

Los bloqueos durante el desplazamiento suelen deberse a trabajo pesado en el
hilo principal, recomposiciones innecesarias, asignaciones dentro de los
elementos, claves inestables o una interfaz de fila demasiado compleja.

### Claves estables y contentType

```kotlin
LazyColumn {
    items(
        items = users,
        key = { user -> user.id }
    ) { user ->
        UserRow(user)
    }
}
```

`key` conserva la identidad del elemento después de una inserción, eliminación o
reordenación.

Para diferentes tipos de elementos:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` ayuda a Compose a reutilizar la composición de elementos
compatibles.

### No realizar trabajo pesado dentro de un elemento

Una forma incorrecta:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Es preferible preparar el texto formateado en un mapper o ViewModel, o al menos
almacenar el objeto mediante `remember`. La E/S, la decodificación de bitmaps, la
ordenación, el filtrado y los cálculos pesados no deben ejecutarse dentro de un
composable.

### Limitar las lecturas del estado

Cada elemento solo debe recibir sus propios datos:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

No debe proporcionarse todo el estado de la pantalla a cada fila.

### Modelos de interfaz estables

Deben utilizarse modelos de interfaz inmutables y evitar crear colecciones u
objetos envoltorio nuevos en cada recomposición sin necesidad.

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

### Estado derivado del desplazamiento

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

`derivedStateOf` resulta útil cuando el desplazamiento cambia con frecuencia,
pero la interfaz solo reacciona a una condición derivada.

### Imágenes y paginación

Para las imágenes debe utilizarse un cargador con caché de memoria y disco, y un
tamaño de destino adecuado. Los bitmaps no deben decodificarse manualmente
dentro de un composable ni en el hilo principal.

Para listas grandes se utiliza Paging:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Creación de perfiles

Las optimizaciones deben confirmarse mediante:

- recuentos de recomposición en Layout Inspector;
- Android Studio Profiler o System Trace;
- Macrobenchmark en una compilación similar a producción;
- Baseline Profiles.

**En resumen:** un desplazamiento fluido en Compose requiere claves estables,
`contentType`, lecturas limitadas del estado, elementos económicos, modelos
inmutables, imágenes almacenadas en caché, paginación y comprobaciones mediante
un profiler. Lo principal es no bloquear el hilo principal ni realizar trabajo
pesado durante la composición.

</details>

<details>
<summary>121. ¿Cuál es la diferencia entre las anotaciones Stable e Immutable?</summary>

#### Kotlin

`@Stable` y `@Immutable` son contratos de Compose para el compilador y el
runtime que ayudan a determinar si una recomposición puede omitirse. No son una
«optimización mágica», sino una promesa de que el tipo se comporta de una manera
determinada.

La diferencia principal:

```text
@Immutable -> el objeto no cambia después de crearse
@Stable    -> el objeto puede cambiar, pero Compose observa dichos cambios
```

### `@Immutable`

Resulta adecuado para modelos de valor o de interfaz:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

La condición es que todas las propiedades públicas permanezcan sin cambios
después de la creación, y que sus campos también sean inmutables o estables.

Una forma incorrecta:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

`MutableList` puede modificarse en el mismo objeto y Compose podría no detectar
el cambio.

Una forma mejor:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Para obtener una garantía más estricta pueden utilizarse colecciones inmutables.

### `@Stable`

Resulta adecuado para contenedores de estado u objetos controladores cuyos
cambios se producen mediante estado observable de Compose:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Un objeto mutable es aceptable porque Compose observa el cambio de `count`.

Una forma incorrecta:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Un campo mutable convencional no informa a Compose sobre sus cambios.

Cuándo utilizar cada anotación:

- `@Immutable`: modelos de interfaz, objetos de valor y clases de datos que
  representan el estado de una pantalla;
- `@Stable`: contenedores de estado personalizados con `mutableStateOf`,
  controladores y objetos con mutación observable y controlada.

Algunos errores habituales son:

- anotar un objeto mutable como `@Immutable`;
- aplicar `@Stable` a una clase con campos mutables convencionales;
- conservar un `MutableList` o `MutableMap` en el estado de la interfaz;
- modificar el estado anterior en lugar de crear uno nuevo;
- utilizar las anotaciones como «remedio» para una propiedad incorrecta del
  estado.

**En resumen:** `@Immutable` significa «no cambia después de crearse».
`@Stable` significa «puede cambiar, pero Compose observa correctamente los
cambios». Una anotación solo debe aplicarse si el tipo cumple realmente dicho
contrato.

</details>

<details>
<summary>122. ¿Por qué debe proporcionarse una key a LazyColumn y cómo funciona internamente?</summary>

#### Kotlin

Una `key` en `LazyColumn` establece una identidad estable para un elemento,
independientemente de su posición.

```kotlin
LazyColumn {
    items(
        items = users,
        key = { it.id }
    ) { user ->
        UserItem(user)
    }
}
```

Sin una clave, Compose utiliza la posición. Esto es adecuado para una lista
estática, pero falla al insertar, eliminar o reordenar elementos.

```text
Antes:      Después de insertar:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

Al utilizar la posición, Compose observa contenido nuevo en slots antiguos. Con
`user.id`, comprende que los elementos anteriores simplemente han cambiado de
posición.

### Estado recordado

```kotlin
@Composable
fun UserItem(user: User) {
    var expanded by rememberSaveable {
        mutableStateOf(false)
    }

    Column(Modifier.clickable { expanded = !expanded }) {
        Text(user.name)
        if (expanded) Text(user.details)
    }
}
```

Sin una clave estable, `expanded` puede permanecer asociado a la posición y
transferirse a otro usuario. La clave desplaza el estado recordado o guardable
junto con la identidad del elemento.

### Funcionamiento interno

La clave se convierte en la identidad de un grupo de composición. El runtime la
utiliza para:

- relacionar el grupo anterior con el nuevo;
- mover o reutilizar el grupo;
- conservar el estado de `remember`;
- restaurar el estado de `rememberSaveable`;
- gestionar correctamente las animaciones de elementos.

No se trata de `DiffUtil`: la clave se utiliza para llevar el registro de la
composición y la colocación dentro del layout perezoso.

### Requisitos de una key

La clave debe ser:

- única entre elementos hermanos;
- estable entre recomposiciones;
- relacionada con la identidad del dominio;
- guardable si el elemento utiliza `rememberSaveable`.

Una buena clave:

```kotlin
key = { it.id }
```

Claves incorrectas:

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Los valores aleatorios o temporales crean una identidad nueva en cada
recomposición. Un índice equivale a la identidad predeterminada basada en la
posición y solo resulta adecuado para listas verdaderamente estáticas.

Una clave compuesta es válida si permanece estable:

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

Para Paging se utiliza la API de claves de elementos:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**En resumen:** `key` vincula un elemento perezoso a una identidad estable, no a
un índice. Es necesaria para listas dinámicas, estado local de los elementos,
reordenación, animaciones y Paging. La mejor clave es un identificador estable
generado por el servidor, la base de datos o el cliente.

</details>

<details>
<summary>123. ¿Qué otros parámetros pueden proporcionarse a items() de LazyColumn y para qué se utiliza contentType?</summary>

#### Kotlin

Los parámetros importantes de `LazyColumn.items()` son `key` y `contentType`.

Un uso básico:

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Para una lista dinámica es preferible proporcionar explícitamente una `key`:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { user -> user.id }
    ) { user ->
        UserRow(user)
    }
}
```

`key` establece una identidad estable para el elemento. Permite que Compose no
confunda el estado recordado después de insertar, eliminar o reordenar
elementos.

Una buena clave:

- es única;
- es estable;
- no depende de la posición;
- no se basa en un valor aleatorio ni temporal;
- se basa en un identificador del dominio.

`contentType` describe el tipo de elemento de la interfaz:

```kotlin
LazyColumn {
    items(
        items = feedItems,
        key = { it.id },
        contentType = { it.type }
    ) { item ->
        FeedItem(item)
    }
}
```

Por ejemplo: `header`, `post`, `ad` o `loader`. Esto ayuda a Compose a reutilizar
la composición de forma más eficiente entre elementos del mismo tipo dentro de
una lista mixta.

Un ejemplo con elementos `sealed`:

```kotlin
sealed interface FeedItem {
    val id: String

    data class Header(override val id: String, val title: String) : FeedItem
    data class Post(override val id: String, val text: String) : FeedItem
    data class Loader(override val id: String) : FeedItem
}
```

```kotlin
items(
    items = feed,
    key = { it.id },
    contentType = { it::class }
) { item ->
    when (item) {
        is FeedItem.Header -> HeaderRow(item)
        is FeedItem.Post -> PostRow(item)
        is FeedItem.Loader -> LoaderRow()
    }
}
```

Si se necesita el índice, puede utilizarse `itemsIndexed`:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

El índice no debe utilizarse como clave para listas mutables o reordenables.

`contentType` no resulta esencial si todos los elementos tienen la misma
estructura visual. Su mayor beneficio se observa en feeds, chats y catálogos
mixtos.

**En resumen:** `key` es la identidad estable de un elemento. `contentType`
describe su tipo o estructura visual para reutilizar mejor la composición en
listas mixtas. En listas dinámicas de producción casi siempre se necesita una
`key`; `contentType` resulta útil cuando existen varios tipos de filas.

</details>

<details>
<summary>124. ¿Qué es SOLID?</summary>

#### Kotlin

`SOLID` reúne cinco principios de diseño orientado a objetos que reducen el
acoplamiento, mejoran la capacidad de realizar pruebas y hacen que los cambios
sean más locales.

### S — Principio de responsabilidad única

Una clase debe tener un único motivo para cambiar.

Es incorrecto que un `ViewModel` gestione la red, la base de datos, el mapeo y el
estado de la interfaz. Es preferible separar las responsabilidades:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

El `ViewModel` gestiona el estado, el caso de uso la acción de negocio y el
repositorio los datos.

### O — Principio abierto/cerrado

El código está abierto a extensiones, pero no requiere modificaciones constantes.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Puede añadirse una implementación nueva sin reescribir el código cliente.

### L — Principio de sustitución de Liskov

Un subtipo debe poder sustituir correctamente al tipo base.

Una forma incorrecta:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Si una implementación no cumple el contrato, este es demasiado amplio o
incorrecto.

### I — Principio de segregación de interfaces

Un cliente no debe depender de métodos que no necesita.

Una forma incorrecta:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Es preferible dividir la interfaz:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D — Principio de inversión de dependencias

El código de alto nivel depende de abstracciones, no de implementaciones
concretas.

Una forma incorrecta:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Una forma mejor:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

De este modo, el caso de uso puede probarse fácilmente mediante un repositorio
fake.

En Android, normalmente se representa así:

```text
UI -> ViewModel -> Caso de uso -> Contrato del repositorio -> Implementación -> API/BD
```

Una regla práctica: SOLID no significa crear una interfaz para cada clase. Una
abstracción es necesaria cuando existe un límite real, una implementación
alternativa o un punto que facilita las pruebas.

**En resumen:** SOLID ayuda a no mezclar la interfaz, la lógica de negocio y la
capa de datos, evita clases que concentran demasiadas responsabilidades, mantiene
los contratos limitados y hace que el código sea comprobable. Sin embargo, un
exceso de abstracciones innecesarias también resulta perjudicial.

</details>

<details>
<summary>125. Explique cada principio SOLID con ejemplos de Android.</summary>

#### Kotlin

SOLID ayuda a separar responsabilidades, mantener contratos claros y probar la
lógica de negocio sin el framework de Android, Retrofit ni Room.

### S — Responsabilidad única

Una clase debe tener un único motivo para cambiar.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()

class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

El `ViewModel` gestiona el estado de la interfaz, el caso de uso la operación de
negocio y el repositorio los datos. SRP no significa «un método por clase», sino
evitar mezclar distintos motivos para cambiar.

### O — Abierto/cerrado

El código se amplía mediante implementaciones nuevas en lugar de modificar
constantemente la lógica existente.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Un handler nuevo se añade como una clase independiente. Sin embargo, una
abstracción solo es necesaria cuando existe una variabilidad real.

### L — Sustitución de Liskov

Un subtipo debe sustituir correctamente al tipo base.

Es incorrecto que `ReadOnlyStorage` implemente `save()` y lance una
`UnsupportedOperationException`. Es preferible dividir el contrato:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP no se limita a la firma; también incluye garantías, errores, efectos
secundarios e invariantes.

### I — Segregación de interfaces

El código cliente no debe depender de métodos que no necesita.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

El caso de uso recibe un contrato limitado en lugar de una interfaz que concentra
demasiadas responsabilidades. No obstante, unas interfaces excesivamente
pequeñas y sin relación con una función del cliente también generan código
repetitivo.

### D — Inversión de dependencias

La lógica de negocio de alto nivel depende de una abstracción y la capa de datos
la implementa.

```kotlin
interface AuthRepository {
    suspend fun login(email: String, password: String): User
}

class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

El caso de uso no crea directamente Retrofit ni Room. La inyección de
dependencias proporciona la implementación real o un fake.

```text
UI -> ViewModel -> Caso de uso -> Contrato del repositorio
                                  ^
                                  |
                         Implementación de datos -> API/BD
```

Algunas infracciones habituales en Android son:

- un Fragment que invoca directamente una API o un DAO;
- un ViewModel que contiene red, mapeo, caché y navegación;
- un único repositorio responsable de toda la aplicación;
- crear una interfaz para cada clase sin que exista un límite;
- hacer que el dominio dependa de tipos de Android o de infraestructura;
- un subtipo que implementa formalmente una interfaz, pero incumple su contrato.

**En resumen:** SRP separa responsabilidades, OCP define puntos de extensión, LSP
garantiza la sustitución correcta de subtipos, ISP mantiene contratos limitados y
DIP hace que la lógica de negocio dependa de abstracciones. En Android, estos
principios separan la interfaz y el dominio del framework y de los detalles de
datos.

</details>

<details>

<summary>126. ¿Qué es la programación orientada a objetos (POO)?</summary>

#### Kotlin

La POO es un enfoque en el que el sistema se modela mediante objetos que combinan
estado y comportamiento. Una clase describe un tipo, mientras que un objeto es
una instancia de este.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Sus principios fundamentales son la encapsulación, la abstracción, la herencia y
el polimorfismo.

### Encapsulación

Una clase oculta el estado mutable y lo modifica mediante métodos controlados:

```kotlin
class Cart {
    private val mutableItems = mutableListOf<CartItem>()
    val items: List<CartItem> get() = mutableItems

    fun add(item: CartItem) {
        require(item.quantity > 0)
        mutableItems += item
    }
}
```

De este modo, la clase protege sus invariantes.

### Abstracción

El código depende de un contrato, no de una implementación:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

`ViewModel` no sabe si los datos proceden de Retrofit, Room o de una
implementación falsa.

### Herencia

En Kotlin, las clases son `final` de manera predeterminada. La herencia se
habilita mediante `open`:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

La herencia modela una relación «es un», pero a menudo genera un acoplamiento
fuerte. Por lo general, la composición es más segura.

### Polimorfismo

Es posible sustituir distintas implementaciones mediante un contrato común:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

El código cliente trabaja con `ImageLoader`, por lo que las implementaciones de
producción y de prueba son intercambiables.

En Kotlin, la POO se combina eficazmente con `data class`, `sealed
class/interface`, las funciones de extensión, la inmutabilidad y la inyección
por constructor. Lo importante no es la cantidad de clases o interfaces, sino
que sus responsabilidades y límites estén claramente definidos.

**En resumen:** la POO organiza el estado y el comportamiento en objetos. En
Kotlin, lo fundamental es aplicar la encapsulación, los contratos y el
polimorfismo, y utilizar la herencia con cautela; con frecuencia conviene
preferir modelos inmutables y la composición frente a jerarquías profundas.

</details>

<details>

<summary>127. ¿Qué es la abstracción?</summary>

#### Kotlin

La abstracción consiste en ocultar los detalles de implementación tras un
contrato comprensible. Describe «qué hace el código», no «cómo lo hace
exactamente».

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

El código que utiliza `UserRepository` no sabe si los datos proceden de una API,
de Room, de la caché o de un objeto falso usado en una prueba.

El caso de uso depende del contrato:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

La implementación oculta los detalles:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository {
    override suspend fun getUser(id: UserId): User {
        return dao.getUser(id.value)?.toDomain()
            ?: api.getUser(id.value).toDomain()
    }
}
```

En las pruebas se puede sustituir por una implementación falsa:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

En Android es habitual la siguiente estructura:

```text
ViewModel -> Caso de uso -> Contrato del repositorio -> Implementación del repositorio -> API/BD
```

La interfaz de usuario no debe conocer Retrofit, Room ni la estrategia de caché.

La abstracción no implica necesariamente una interfaz. También puede ser una
clase abstracta, una jerarquía sellada, un tipo de función o simplemente la API
pública de una clase. Sin embargo, las clases abstractas o base suelen crear
jerarquías rígidas en Android, por lo que la composición normalmente es más
segura.

Abstracción frente a encapsulación:

```text
Abstracción   -> muestra el contrato relevante
Encapsulación -> oculta el estado y los detalles internos
```

Una mala abstracción es una interfaz creada «por si acaso», sin una
implementación alternativa real ni un límite arquitectónico:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

Si solo existe una implementación sencilla y no hay un punto de sustitución para
las pruebas ni un límite arquitectónico, la interfaz puede ser innecesaria.

Una buena abstracción:

- tiene un contrato claro;
- oculta los detalles inestables;
- no expone detalles de implementación;
- facilita las pruebas;
- corresponde a un punto de cambio real;
- no añade código repetitivo sin aportar valor.

**En resumen:** la abstracción separa el contrato de la implementación. En
Android resulta útil en los límites relacionados con la red, la base de datos,
las analíticas, el tiempo, los dispatchers y los repositorios. No es necesario
abstraer cada clase sin una razón real.

</details>

<details>

<summary>128. ¿Cuál es la diferencia entre los constructores y los métodos?</summary>

#### Kotlin

Un constructor crea una instancia y lleva el objeto a un estado inicial válido.
Un método describe el comportamiento de un objeto ya creado y puede invocarse
varias veces.

Constructor primario:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Los parámetros declarados con `val` o `var` se convierten en propiedades. Un
constructor no tiene tipo de retorno y se invoca mediante `User(...)`.

Método:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

El método se invoca sobre una instancia:

```kotlin
user.displayName()
```

Para la validación o la lógica de inicialización se utiliza `init`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    init {
        require(id.isNotBlank()) {
            "User id must not be blank"
        }
    }
}
```

Un constructor secundario delega en el primario mediante `this(...)`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

En Kotlin suele ser preferible utilizar un parámetro con valor predeterminado:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

El constructor se utiliza con frecuencia para la inyección de dependencias:

```kotlin
class GetUserProfileUseCase(
    private val repository: UserRepository,
    private val analytics: AnalyticsTracker
) {
    suspend operator fun invoke(userId: String): UserProfile {
        analytics.track(ProfileOpened(userId))
        return repository.getProfile(userId)
    }
}
```

El constructor establece las dependencias obligatorias, mientras que el método
ejecuta una operación.

Qué no debe hacerse en un constructor o bloque `init`:

- realizar una solicitud de red;
- ejecutar una consulta a la base de datos;
- iniciar una corrutina;
- utilizar `runBlocking`;
- realizar operaciones de E/S pesadas;
- ejecutar lógica dependiente del ciclo de vida de Android.

Incorrecto:

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Es preferible:

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**En resumen:** un constructor crea un objeto válido y recibe su estado y sus
dependencias obligatorias. Un método ejecuta comportamientos después de la
construcción. Las tareas pesadas, suspendibles o vinculadas al ciclo de vida no
deben iniciarse en un constructor.

</details>

<details>

<summary>129. ¿Qué es la firma de un método?</summary>

#### Kotlin

La firma de un método es el conjunto de características mediante las cuales el
compilador distingue una función de otra. En Kotlin incluye principalmente el
nombre, el receptor y los tipos y el orden de los parámetros.

```kotlin
fun loadUser(id: String): User
```

De forma simplificada, su firma es `loadUser(String)`.

### Sobrecarga

Las sobrecargas deben diferenciarse por su lista de parámetros:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

El tipo de retorno no es suficiente:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflicto
```

El nombre del parámetro tampoco es suficiente:

```kotlin
fun load(id: String)
fun load(userId: String) // conflicto
```

Los argumentos con nombre mejoran la legibilidad en el lugar de la llamada, pero
no crean una firma nueva.

El orden de los tipos sí importa:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

Este código es válido, aunque suele constituir una API deficiente debido a su
escasa legibilidad.

### Parámetros predeterminados

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

Esta es una única función de Kotlin. Para generar sobrecargas destinadas a Java
se puede utilizar `@JvmOverloads`, aunque conviene evitar crear manualmente
demasiadas sobrecargas similares.

### Sobrescritura

Una sobrescritura debe ajustarse al contrato del tipo base:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Cambiar el tipo de un parámetro da lugar a otra función, no a una sobrescritura.
El tipo de retorno puede ser un subtipo covariante.

### Receptor y extensiones

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

El receptor de una extensión participa en la resolución de Kotlin. Sin embargo,
una función de extensión no es una sobrescritura virtual.

### Particularidades de la JVM

El borrado de tipos puede provocar conflictos:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // conflicto en la JVM: process(List)
```

La nulabilidad tampoco crea una firma independiente en la JVM:

```kotlin
fun load(id: String)
fun load(id: String?) // conflicto en la JVM
```

La solución consiste en utilizar otro nombre o `@JvmName` cuando sea realmente
necesario.

**En resumen:** en Kotlin, las funciones se distinguen por el nombre, el receptor
y los tipos y el orden de sus parámetros. El tipo de retorno y los nombres de los
parámetros no son suficientes. En la JVM también deben tenerse en cuenta el
borrado de tipos, la nulabilidad y las particularidades de interoperabilidad.

</details>

<details>

<summary>130. ¿Cuál es la diferencia entre la sobrecarga (overloading) y la sobrescritura (overriding)?</summary>

#### Kotlin

```text
Sobrecarga     -> un mismo nombre, distintas listas de parámetros
Sobrescritura  -> la misma firma, una nueva implementación en un subtipo
```

### Sobrecarga

Varias funciones de un mismo ámbito tienen el mismo nombre, pero parámetros
distintos:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

El compilador selecciona la sobrecarga según los tipos estáticos de los
argumentos. El tipo de retorno no basta por sí solo para distinguir una
sobrecarga:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // error de compilación
```

En Kotlin suele ser preferible sustituir parte de las sobrecargas por parámetros
predeterminados y argumentos con nombre:

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
)

loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

### Sobrescritura

Un subtipo implementa o modifica un método heredado con la misma firma:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository(
    private val api: UserApi
) : UserRepository {
    override suspend fun getUser(id: String): User =
        api.getUser(id).toDomain()
}
```

Cambiar el tipo de un parámetro crea otra función, no una sobrescritura.

En Kotlin, las clases y los métodos son `final` de manera predeterminada. Para
permitir la sobrescritura, el miembro debe ser `open`, `abstract` o pertenecer a
una interfaz:

```kotlin
open class BaseTracker {
    open fun track(event: Event) = Unit
}

class FirebaseTracker : BaseTracker() {
    override fun track(event: Event) {
        // implementación
    }
}
```

### Despacho

La sobrecarga utiliza despacho estático, determinado durante la compilación:

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

La sobrescritura utiliza despacho virtual, determinado durante la ejecución:

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // implementación de CrashlyticsLogger
```

Aspectos importantes:

- las sobrecargas con argumentos predeterminados pueden resultar ambiguas;
- una sobrescritura debe respetar el contrato del tipo base;
- el tipo de retorno de una sobrescritura puede ser covariante;
- las funciones de extensión no se sobrescriben, sino que se resuelven
  estáticamente;
- `final override` impide sobrescrituras posteriores.

**En resumen:** la sobrecarga define varias funciones con el mismo nombre y
parámetros diferentes, y la selección se realiza durante la compilación. La
sobrescritura sustituye una implementación heredada conservando la misma firma,
y la selección se realiza durante la ejecución mediante polimorfismo.

</details>

<details>

<summary>131. ¿Cuál es la diferencia entre una interfaz y una clase abstracta?</summary>

#### Kotlin

Una interfaz describe un contrato o una función determinada. Una clase abstracta
describe una base común con una implementación parcial, un constructor y estado.

```text
interfaz        -> contrato, múltiples funciones
clase abstracta -> implementación base, una sola clase padre
```

### Interfaz

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Una clase puede implementar múltiples interfaces:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Una interfaz resulta apropiada para la inyección de dependencias, las pruebas y
la inversión de dependencias:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

En Kotlin, una interfaz puede contener un método con implementación
predeterminada:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Sin embargo, una interfaz no tiene constructor y no es apropiada para compartir
estado mutable.

### Clase abstracta

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // lógica compartida
    }

    abstract suspend fun refresh()
}
```

Una clase abstracta puede tener:

- un constructor;
- estado;
- métodos protegidos;
- una implementación parcial;
- el patrón de método plantilla.

No obstante, una clase solo puede heredar de una clase:

```kotlin
class MyRepository : BaseRepository()
```

### Cuándo utilizar cada opción

Interfaz:

- se necesita un contrato;
- existen varias implementaciones;
- se necesita una implementación falsa o un mock en las pruebas;
- una clase desempeña varias funciones;
- es necesario abstraer una dependencia.

Clase abstracta:

- se necesita una implementación compartida;
- se necesita estado protegido;
- existe un ciclo de vida o una plantilla base;
- el constructor forma parte del contrato base.

En Android conviene utilizar `BaseActivity`, `BaseFragment` y `BaseViewModel` con
precaución: pueden convertirse rápidamente en clases base que concentran
demasiadas responsabilidades y crean una jerarquía rígida.

Con frecuencia, la composición es preferible:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**En resumen:** una interfaz sirve para definir contratos, aplicar la inyección
de dependencias, facilitar las pruebas y representar múltiples funciones. Una
clase abstracta permite compartir implementación y estado, pero conlleva el
riesgo de crear una herencia rígida. En el desarrollo moderno de Android suele
ser preferible combinar interfaces y composición en lugar de crear jerarquías
profundas de clases base.

</details>

<details>

<summary>132. ¿Puede una interfaz no contener métodos?</summary>

#### Kotlin

Sí. Una interfaz vacía se denomina interfaz marcadora. No define un
comportamiento, sino que asigna a un tipo una función determinada dentro del
sistema de tipos.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Ahora se puede aceptar `UserProfile` como `Cacheable`, comprobarlo mediante `is
Cacheable` o utilizarlo como límite de un tipo genérico.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // solo tipos marcados explícitamente
}
```

Si `sync` necesita un identificador o algún comportamiento, la interfaz marcadora
deja de ser suficiente y debe añadirse una propiedad o un método al contrato.

### Interfaz marcadora sellada

Una `sealed interface` vacía se utiliza con frecuencia como raíz de un conjunto
cerrado de estados, eventos o efectos:

```kotlin
sealed interface LoginEffect

data object NavigateHome : LoginEffect

data class ShowError(
    val message: String
) : LoginEffect
```

```kotlin
when (effect) {
    NavigateHome -> navigateHome()
    is ShowError -> showError(effect.message)
}
```

En este caso no se trata simplemente de un marcador, sino de un tipo suma: el
valor puede ser una de las variantes definidas y el compilador comprueba que
`when` sea exhaustivo.

Un ejemplo clásico de interfaz marcadora es `java.io.Serializable`: no contiene
métodos, pero el entorno de ejecución comprueba si el objeto implementa ese tipo.

### Interfaz o anotación

Una interfaz marcadora es preferible cuando se necesita:

- una relación de subtipo;
- establecer límites para tipos genéricos;
- una API polimórfica;
- una jerarquía sellada;
- una comprobación con `is` sin reflexión.

Una anotación es más apropiada para metadatos:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Puede ser procesada por un complemento del compilador, un generador de código o
mediante reflexión durante la ejecución. No crea una relación de subtipo.

### Cuándo una interfaz marcadora es una mala elección

No deben crearse marcadores «por si acaso»:

```kotlin
interface Important
interface Special
```

Señales de un diseño deficiente:

- el marcador nunca se utiliza como tipo, límite o raíz;
- la lógica contiene numerosas comprobaciones `is Marker`;
- se necesitan datos o métodos, pero el código recurre a conversiones de tipo;
- la función se expresaría mejor mediante una enumeración o una propiedad;
- una anotación describiría mejor los metadatos.

**En resumen:** una interfaz puede estar vacía. Una interfaz marcadora resulta
útil para establecer relaciones de subtipo, límites de tipos genéricos o
jerarquías selladas. Si se necesitan metadatos, conviene utilizar una anotación;
si se necesita comportamiento, la interfaz debe contener miembros.

</details>

<details>

<summary>133. ¿Puede una interfaz contener otra interfaz?</summary>

#### Kotlin

Sí. En Kotlin, una interfaz puede contener otra interfaz. Esto se denomina
interfaz anidada.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Uso:

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // gestionar el cambio
    }
}
```

Se accede a una interfaz anidada mediante `Exterior.Interior`, por ejemplo,
`UserRepository.Listener`.

Cuándo resulta apropiado:

- el contrato está estrechamente relacionado con la API externa;
- el tipo anidado solo se utiliza en ese contexto;
- se trata de un callback o de un pequeño contrato anidado;
- un tipo de nivel superior solo recargaría innecesariamente el paquete.

Ejemplo de una API con callback:

```kotlin
interface DownloadManager {
    fun start(url: String, callback: Callback)

    interface Callback {
        fun onProgress(percent: Int)
        fun onSuccess(filePath: String)
        fun onError(error: Throwable)
    }
}
```

Una interfaz anidada no es `inner` y no tiene acceso al estado de una instancia
del tipo exterior:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` no puede leer `id` directamente.

No debe confundirse con la herencia de interfaces:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

En este caso no hay anidamiento, sino herencia de contratos.

No conviene anidar una interfaz si es independiente y se utiliza en todo el
proyecto:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

De este modo, la interfaz exterior se convierte en un espacio de nombres
artificial. Es preferible definir esos contratos en el nivel superior.

**En resumen:** una interfaz puede contener una interfaz anidada. Esto resulta
útil para callbacks o contratos que solo existen en el contexto de una API
externa. Las abstracciones independientes deberían definirse en el nivel
superior.

</details>

<details>

<summary>134. ¿Por qué no conviene añadir decenas de métodos a una interfaz?</summary>

#### Kotlin

Una interfaz extensa suele infringir el principio de segregación de interfaces:
los clientes dependen de métodos que no necesitan. Esto aumenta el acoplamiento
y dificulta las implementaciones y las pruebas.

Incorrecto:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
    suspend fun updateUser(user: User)
    suspend fun deleteUser(id: String)
    suspend fun uploadAvatar(uri: Uri)
    suspend fun getUserPosts(id: String): List<Post>
    suspend fun blockUser(id: String)
}
```

Aunque un caso de uso solo necesite `getUser`, seguirá dependiendo del contrato
completo.

Problema en la implementación:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Si una implementación no puede implementar un método de manera legítima, el
contrato es incorrecto o demasiado amplio.

Problema en las pruebas:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // código innecesario
}
```

Las implementaciones falsas y los mocks se ven obligados a implementar métodos
innecesarios.

Es preferible separar las funciones:

```kotlin
interface UserReader {
    suspend fun getUser(id: String): User
}

interface UserWriter {
    suspend fun updateUser(user: User)
    suspend fun deleteUser(id: String)
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

El caso de uso depende únicamente del contrato que necesita:

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

Una interfaz extensa solo es aceptable cuando representa realmente una única
abstracción cohesionada. Por ejemplo, un DAO para una sola tabla puede contener
varias consultas relacionadas. Sin embargo, si los métodos tienen clientes
distintos y razones diferentes para cambiar, es preferible separarlos.

**En resumen:** una interfaz debe describir una función concreta, no «todo lo que
puede hacer el sistema». Si un cliente utiliza uno o dos métodos de un total de
veinte, el contrato es demasiado amplio. Conviene separar lectores, escritores,
cargadores y otros contratos según sus funciones reales.

</details>

<details>

<summary>135. ¿Qué es la composición y en qué se diferencia de la herencia?</summary>

#### Kotlin

Mediante la composición, una clase obtiene comportamiento a través de sus
dependencias. Mediante la herencia, una clase hereda comportamiento de una clase
base.

```text
Herencia    -> es un
Composición -> tiene un / utiliza un
```

### Herencia

Resulta apropiada cuando la subclase es realmente una especialización del tipo
base:

```kotlin
open class Animal {
    open fun makeSound() {}
}

class Dog : Animal() {
    override fun makeSound() {
        println("Bark")
    }
}
```

`Dog` es un `Animal`, por lo que la relación «es un» resulta lógica.

### Composición

Una clase utiliza otros objetos en lugar de recurrir a la herencia:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

`Dog` tiene un `SoundPlayer`, pero no es un `SoundPlayer`.

En Android y Kotlin, la composición suele adoptar la siguiente forma:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

Las dependencias son explícitas y pueden sustituirse fácilmente por
implementaciones falsas o mocks en las pruebas.

### El problema de las clases base

Las clases `BaseViewModel`, `BaseRepository` y `BaseFragment` de gran tamaño
suelen convertirse en clases base que concentran demasiadas responsabilidades:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Las clases derivadas reciben métodos y estado que no necesitan. Un cambio en la
clase base puede afectar a muchas pantallas que no guardan relación entre sí.

### Delegación en Kotlin

Kotlin ofrece la delegación como una forma práctica de composición:

```kotlin
interface Logger {
    fun log(message: String)
}

class ConsoleLogger : Logger {
    override fun log(message: String) = println(message)
}

class UserService(
    private val logger: Logger
) : Logger by logger
```

`UserService` delega `Logger` en otro objeto sin heredar de él.

Cuándo utilizar cada opción:

- herencia: existe una relación «es un» real y un contrato base estable;
- composición: el comportamiento debe poder sustituirse, probarse, combinarse o
  modificarse;
- código de aplicaciones Android: por lo general, la composición es preferible
  a una herencia profunda.

**En resumen:** la herencia modela una relación «es un», mientras que la
composición modela «tiene un» o «utiliza un». En Kotlin y Android, la composición
suele ser preferible porque ofrece dependencias explícitas, pruebas más
sencillas, menor acoplamiento y evita jerarquías frágiles de clases base.

</details>

<details>

<summary>136. ¿Qué modificadores de acceso existen en Kotlin?</summary>

#### Kotlin

Kotlin dispone de cuatro modificadores de visibilidad:

```text
public    -> accesible desde cualquier lugar
internal  -> accesible dentro del módulo
protected -> accesible en la clase y sus subclases
private   -> accesible en el ámbito o archivo actual
```

### `public`

`public` es la visibilidad predeterminada:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Todo elemento público pasa a formar parte de la API, por lo que no deben
exponerse detalles de implementación innecesariamente.

### `private`

Un miembro privado solo es accesible dentro de su clase u objeto:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Una declaración `private` de nivel superior es accesible dentro de un único
archivo Kotlin:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### `protected`

Un miembro protegido es accesible en su clase y sus subclases:

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}
```

A diferencia de Java, en Kotlin `protected` no concede acceso dentro del paquete
y no puede utilizarse en el nivel superior. Forma parte de la API de herencia.

### `internal`

Un elemento interno es accesible dentro de un módulo de Kotlin:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Un módulo es un límite de compilación, por ejemplo, un módulo de Gradle, no un
paquete.

`internal` resulta útil en aplicaciones Android multimódulo: solo se deja pública
la API del módulo, mientras que la implementación se declara `internal`.

Es importante recordar que `internal` es un límite de compilación, no una medida
de seguridad. En el bytecode, Java y la reflexión pueden acceder a más
elementos.

### Visibilidad de setters y constructores

Una propiedad puede ser pública para la lectura y privada para la escritura:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set
}
```

Un constructor también puede tener un modificador de visibilidad:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Reglas prácticas:

- comenzar con la visibilidad mínima necesaria;
- mantener la implementación como `private` o `internal`;
- reducir al mínimo la API pública;
- utilizar `protected` solo para un contrato de herencia real;
- no considerar la visibilidad como un límite de seguridad.

**En resumen:** `public` permite el acceso general, `internal` lo limita al
módulo, `protected` a la jerarquía de herencia y `private` al ámbito de la clase o
del archivo. Una buena API de Kotlin expone únicamente lo necesario.

</details>

<details>

<summary>137. ¿Qué es un singleton?</summary>

#### Kotlin

Un singleton es una única instancia dentro de un propietario determinado, como
un cargador de clases, un proceso o un componente de inyección de dependencias.
En Android es importante recordar que el proceso puede finalizar y que una
aplicación multiproceso tendrá un singleton independiente en cada proceso.

### `object` de Kotlin

```kotlin
object AppLogger {
    fun log(message: String) = println(message)
}
```

Un `object` se inicializa de forma diferida y segura para subprocesos cuando se
utiliza por primera vez. Sin embargo, el estado mutable que contiene no se vuelve
automáticamente seguro para subprocesos:

```kotlin
object Counter {
    private val value = AtomicInteger()

    fun increment(): Int = value.incrementAndGet()
}
```

### `companion object`

```kotlin
class UserId private constructor(
    val value: String
) {
    companion object {
        fun from(raw: String): UserId = UserId(raw.trim())
    }
}
```

Un objeto compañero es un singleton asociado a una clase. La propia clase puede
tener múltiples instancias. Por lo general, el objeto compañero se utiliza para
métodos de fábrica y constantes.

### Singleton mediante inyección de dependencias

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
)
```

En Hilt, `@Singleton` representa una única instancia dentro de
`SingletonComponent`, no un «objeto eterno». La inyección de dependencias es
preferible a un singleton global cuando existen dependencias:

- las dependencias del constructor son explícitas;
- la implementación puede sustituirse por una implementación falsa;
- el componente controla el tiempo de vida;
- el cliente depende de un contrato.

Para ciclos de vida más breves existen `@ViewModelScoped`, `@ActivityScoped` y
otros ámbitos.

### Riesgos

Incorrecto:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Problemas:

- fuga de memoria de una `Activity`;
- estado mutable global;
- condiciones de carrera;
- pruebas difíciles debido al estado compartido;
- cierre de sesión o restablecimiento poco claros;
- pérdida del estado después de la finalización del proceso.

Un singleton del ámbito de la aplicación no debe conservar una `Activity`, un
`Fragment`, una `View` ni un `ActivityContext`. Si necesita un contexto, debe
utilizar únicamente el contexto de la aplicación.

La sesión o el token no deben almacenarse exclusivamente en un singleton en
memoria: se necesitan persistencia segura, restauración y eliminación explícita.

Un singleton resulta apropiado para:

- una base de datos Room;
- un cliente OkHttp;
- servicios de registro o analíticas;
- servicios sin estado para toda la aplicación;
- configuración inmutable.

No resulta apropiado para el estado de una pantalla, un `ViewModel`, una sesión
de usuario sin restablecimiento o una dependencia con un ciclo de vida breve.

**En resumen:** en Kotlin, un singleton suele crearse mediante `object`, mientras
que en Android es preferible definir el ámbito de las dependencias mediante
inyección de dependencias. Un singleton es adecuado para servicios de toda la
aplicación, pero el estado mutable global y las referencias a la interfaz de
usuario casi siempre constituyen un problema.

</details>

<details>

<summary>138. ¿Qué son los genéricos?</summary>

#### Kotlin

Los genéricos permiten parametrizar una clase, una interfaz o una función
mediante un tipo y conservar la seguridad de tipos durante la compilación.

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` es un parámetro de tipo, mientras que `String` e `Int` son argumentos de
tipo. El compilador conoce el tipo de `value`, por lo que no se necesita una
conversión de tipo insegura.

Función genérica:

```kotlin
fun <T> singleItemList(item: T): List<T> = listOf(item)
```

### Límites

Es posible restringir un parámetro de tipo:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Varios límites:

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable = Unit
```

### Resultado genérico

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val cause: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

`Nothing` es un subtipo de todos los tipos de Kotlin, por lo que `Error` y
`Loading` pueden utilizarse como `Result<User>`, `Result<List<Post>>`, etc.

### Varianza

De forma predeterminada, un tipo genérico es invariante. Aunque `Cat` sea un
subtipo de `Animal`, `MutableList<Cat>` no es un subtipo de
`MutableList<Animal>`.

`out T` representa un productor y define covarianza:

```kotlin
interface Producer<out T> {
    fun produce(): T
}

val cats: Producer<Cat> = TODO()
val animals: Producer<Animal> = cats
```

`in T` representa un consumidor y define contravarianza:

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}

val animalConsumer: Consumer<Animal> = TODO()
val catConsumer: Consumer<Cat> = animalConsumer
```

La regla es: productor, `out`; consumidor, `in`. Si un tipo recibe y devuelve
`T`, normalmente es invariante.

### Borrado de tipos y `reified`

En la JVM, los argumentos de tipos genéricos se borran en gran medida durante la
ejecución:

```kotlin
value is List<*> // permitido
// value is List<String> // no se permite directamente
```

`inline reified` permite acceder al parámetro de tipo durante la ejecución:

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` solo puede utilizarse en funciones `inline`.

Reglas prácticas:

- los límites definen el contrato mínimo;
- `out` e `in` describen relaciones de subtipo seguras;
- los contenedores mutables suelen ser invariantes;
- para disponer de un tipo genérico durante la ejecución se necesita `reified` o
  un token de tipo explícito;
- no conviene complicar una API con genéricos sin una ventaja real.

**En resumen:** los genéricos permiten crear API reutilizables y seguras respecto
a los tipos. Los límites restringen los tipos admitidos, la varianza (`out` e
`in`) controla la compatibilidad entre subtipos y `reified` permite sortear
parcialmente el borrado de tipos de la JVM en funciones `inline`.

</details>

<details>

<summary>139. ¿Qué es el borrado de tipos y por qué se produce?</summary>

#### Kotlin

El borrado de tipos significa que, en la JVM, los argumentos genéricos concretos
normalmente no se conservan en el objeto durante la ejecución. `List<String>` y
`List<Int>` son simplemente `List` durante la ejecución.

Esto se debe a la compatibilidad retroactiva de los genéricos de Java con el
bytecode antiguo. Un parámetro de tipo se borra hasta su límite superior o hasta
`Object`. Kotlin/JVM utiliza este mismo modelo de la JVM.

No es posible realizar directamente la siguiente comprobación:

```kotlin
if (value is List<String>) { } // tipo borrado
```

Sí se puede comprobar el contenedor:

```kotlin
if (value is List<*>) {
    // se desconoce el tipo de los elementos
}
```

`List<*>` permite leer los elementos de forma segura como `Any?`, pero no
garantiza su tipo concreto.

Una conversión no comprobada no valida todos los elementos:

```kotlin
val strings = value as List<String> // conversión no comprobada
```

Si se necesita una validación real, deben comprobarse los elementos:

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String ?: error("Expected String")
    }
```

### `reified`

Un `T` normal no está disponible para una comprobación `is T`. Sin embargo, una
función `inline` puede declarar un parámetro de tipo `reified`:

```kotlin
inline fun <reified T> isType(value: Any): Boolean =
    value is T
```

De este modo pueden utilizarse `T::class`, `is T` y las API basadas en tokens de
clase. No obstante, `reified` no recupera por completo los genéricos anidados:
`List<String>` sigue requiriendo comprobar sus elementos o utilizar un
serializador o token de tipo.

Sin `reified`, el tipo se proporciona explícitamente:

```kotlin
fun <T> decode(json: String, clazz: Class<T>): T = TODO()

val user = decode(json, User::class.java)
```

Para `List<User>`, un único `Class<List>` no es suficiente; por ello, las
bibliotecas utilizan `Type`, `KType`, un adaptador generado o `KSerializer<T>`.

El borrado de tipos también provoca conflictos de firmas en la JVM:

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {} // conflicto: handle(List)
```

Se necesitan nombres distintos o un uso cuidadoso de `@JvmName`.

**En resumen:** el borrado de tipos elimina los argumentos genéricos durante la
ejecución debido a la compatibilidad entre la JVM y Java. Para realizar
comprobaciones en tiempo de ejecución se utilizan `List<*>`, `inline reified`, un
token de tipo explícito o un serializador; una conversión no comprobada no
constituye una validación completa de los tipos.

</details>

<details>

<summary>140. ¿Qué tipos de colecciones existen: List, Set, Map, Queue y Stack?</summary>

#### Kotlin

Una colección se elige según su semántica: orden, unicidad, búsqueda por clave,
FIFO o LIFO.

### `List`

Es una colección ordenada que permite acceder por índice y admite duplicados:

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

La implementación mutable habitual en la JVM es `ArrayList`: acceso por índice
en `O(1)`, búsqueda en `O(n)` e inserción o eliminación en una posición
intermedia en `O(n)`.

### `Set`

Es una colección de elementos únicos:

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* existe */ }
```

Por lo general, `HashSet` ofrece una complejidad media de `O(1)` para
`contains`, `add` y `remove`, pero requiere implementaciones correctas de
`equals()` y `hashCode()`.

### `Map`

Es una estructura de pares clave-valor cuyas claves son únicas:

```kotlin
val usersById: Map<String, User> = users.associateBy(User::id)
val user = usersById["42"]
```

En promedio, `HashMap.get()` tiene una complejidad de `O(1)`. Un mapa resulta
adecuado como caché, índice o tabla de búsqueda. Una clave mutable es peligrosa
si cambian los campos que intervienen en `hashCode()`.

### `Queue` y `Stack`

Una cola utiliza FIFO:

```text
primero en entrar -> primero en salir
```

Una pila utiliza LIFO:

```text
último en entrar -> primero en salir
```

En Kotlin resulta práctico utilizar `ArrayDeque`:

```kotlin
val deque = ArrayDeque<String>()

// cola
deque.addLast("A")
deque.addLast("B")
val first = deque.removeFirst() // A

// pila
deque.addLast("C")
val last = deque.removeLast() // C
```

Las operaciones en los extremos tienen una complejidad amortizada de `O(1)`.
`ArrayDeque` es preferible a la clase heredada `Stack` de Java.

### Solo lectura frente a mutable

Kotlin diferencia sus API:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` no dispone de `add` ni `remove`, pero esto no implica inmutabilidad
profunda: el objeto subyacente o sus elementos pueden ser mutables. El estado
mutable compartido debe encapsularse.

Elección práctica:

- orden, índices y duplicados: `List`;
- unicidad y comprobación de pertenencia: `Set`;
- búsqueda por clave: `Map`;
- FIFO: `ArrayDeque.removeFirst()`;
- LIFO: `ArrayDeque.removeLast()`;
- orden por prioridad: `PriorityQueue`.

**En resumen:** `List` conserva el orden y admite duplicados, `Set` garantiza
unicidad, `Map` permite buscar por clave, una cola utiliza FIFO y una pila utiliza
LIFO. En Kotlin, `ArrayDeque` suele ser suficiente para implementar colas y
pilas.

</details>

<details>

<summary>141. ¿Qué es la seguridad para subprocesos de las colecciones?</summary>

#### Kotlin

Una colección segura para subprocesos conserva un estado correcto durante el
acceso concurrente. Las colecciones habituales `MutableList`, `MutableMap` y
`MutableSet` de Kotlin/JVM no son seguras para subprocesos.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Las escrituras concurrentes pueden provocar actualizaciones perdidas, un estado
interno incorrecto o una excepción.

### Solo lectura no equivale a inmutable

```kotlin
val users: List<User> = mutableListOf()
```

`List` solo impide modificar la colección mediante esa referencia. La colección
subyacente o sus elementos pueden ser mutables.

### Estrategias principales

1. **Un único propietario o subproceso**

Todas las lecturas y escrituras se canalizan a través de un único propietario:
el subproceso principal, un dispatcher único o un componente similar a un actor.
Este es el modelo de propiedad más sencillo.

2. **`Mutex` para corrutinas**

```kotlin
class UserCache {
    private val mutex = Mutex()
    private val users = mutableMapOf<String, User>()

    suspend fun put(user: User) = mutex.withLock {
        users[user.id] = user
    }

    suspend fun get(id: String): User? = mutex.withLock {
        users[id]
    }
}
```

Todos los accesos deben pasar por el mismo `Mutex`. Dentro del bloqueo no deben
realizarse operaciones de red prolongadas ni tareas suspendibles.

3. **Colecciones concurrentes**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` es adecuado para el acceso concurrente mediante claves. Sin
embargo, una operación compuesta no se vuelve atómica automáticamente; para ello
se necesitan métodos atómicos como `computeIfAbsent` o una sincronización
independiente.

`CopyOnWriteArrayList` resulta apropiada para listeners cuando hay muchas
lecturas y pocas escrituras.

4. **Instantáneas inmutables**

Para el estado de la interfaz de usuario es preferible crear una colección nueva:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

En este caso, `_state.update` proporciona la atomicidad, no la propia `List`.

### Iteración

Modificar una colección durante una iteración puede provocar
`ConcurrentModificationException`, incluso dentro de un único subproceso. Si es
necesario modificarla durante el recorrido, debe utilizarse un iterador, una
operación de colección o una instantánea.

Elección práctica:

- estado de corrutinas: `Mutex` o un propietario serializado;
- acceso concurrente por clave: `ConcurrentHashMap`;
- listeners con predominio de lecturas: `CopyOnWriteArrayList`;
- estado de la interfaz de usuario: instantáneas inmutables y `update` atómico;
- invariantes compuestos: una única sección crítica.

**En resumen:** las colecciones mutables no son seguras para subprocesos. Una API
de solo lectura no implica inmutabilidad. Para garantizar la seguridad se
necesita propiedad exclusiva, `Mutex`, colecciones concurrentes o instantáneas
inmutables; debe sincronizarse el invariante completo, no una llamada aislada.

</details>

<details>

<summary>142. ¿Cuál es la diferencia entre final, finally y finalize?</summary>

#### Kotlin

Son tres conceptos diferentes:

```text
final    -> impide la herencia o la sobrescritura
finally  -> bloque de limpieza posterior a try/catch
finalize -> callback obsoleto del recolector de basura
```

### `final`

En Java, `final` impide heredar de una clase, sobrescribir un método o reasignar
una variable.

En Kotlin, las clases y los métodos son `final` de manera predeterminada. Para
permitir la herencia debe utilizarse `open`:

```kotlin
open class BaseViewModel : ViewModel() {
    open fun load() = Unit
}

class ProfileViewModel : BaseViewModel() {
    final override fun load() {
        // las subclases no pueden sobrescribir este método
    }
}
```

`final override` permite sobrescribir el método una vez e impide sobrescrituras
posteriores.

Para las variables y propiedades, Kotlin utiliza `val`:

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex")
```

`val` impide reasignar la referencia, pero no convierte el objeto en inmutable.

### `finally`

`finally` se ejecuta después de `try/catch` para realizar tareas de limpieza:

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Puede utilizarse para cerrar un recurso, liberar un bloqueo, anular el registro
de un listener o restablecer un estado temporal.

En las corrutinas, `finally` también se ejecuta cuando se produce una
cancelación:

```kotlin
viewModelScope.launch {
    try {
        repository.sync()
    } catch (error: CancellationException) {
        throw error
    } finally {
        _state.update { it.copy(isLoading = false) }
    }
}
```

Si la limpieza requiere una función suspendible dentro de una corrutina
cancelada, en ocasiones puede necesitarse un bloque breve
`withContext(NonCancellable)`. Sin embargo, esto debe ser una excepción, no la
opción predeterminada.

La ejecución de `finally` no está garantizada si el proceso finaliza o la JVM se
bloquea.

### `finalize`

`finalize()` es un antiguo callback de `Object` de Java que el recolector de
basura podía invocar antes de recoger un objeto. No debe utilizarse para realizar
limpieza porque:

- no se sabe cuándo se invocará;
- puede no invocarse antes de que finalice el proceso;
- dificulta el trabajo del recolector de basura;
- es un mecanismo obsoleto y heredado.

Alternativa:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

En Android, la limpieza se vincula al ciclo de vida:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

En Compose se realiza mediante `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**En resumen:** `final` limita la herencia y la sobrescritura, `finally` realiza
una limpieza determinista después de `try/catch` y `finalize()` es un mecanismo
obsoleto y poco fiable del recolector de basura que no debe utilizarse.

</details>

<details>

<summary>143. ¿Cómo funciona try-catch-finally?</summary>

#### Kotlin

`try-catch-finally` separa una operación arriesgada, el tratamiento de una
excepción y la limpieza.

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Orden de ejecución:

```text
éxito -> try -> finally
error -> try -> catch correspondiente -> finally
```

Si no existe un `catch` correspondiente, la excepción se propagará al código
llamador después de ejecutarse `finally`.

### `catch`

Los distintos tipos pueden capturarse por separado:

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

Los bloques `catch` se comprueban de arriba abajo, por lo que los tipos más
específicos deben aparecer antes que los más generales.

Kotlin no tiene excepciones comprobadas, por lo que el compilador no obliga a
escribir `try/catch`. Una excepción debe capturarse cuando el código pueda
recuperarse de manera significativa, transformar el error o añadir contexto.

### `try` como expresión

`try` puede devolver un valor:

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

`finally` no determina el resultado de la expresión.

### `finally`

`finally` se utiliza para realizar tareas de limpieza:

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

Se ejecuta en caso de éxito, excepción, `return` y cancelación de una corrutina.
No debe utilizarse `return` dentro de `finally`, ya que puede sustituir el
resultado o la excepción.

Para recursos `Closeable` o `AutoCloseable` es preferible utilizar `use`:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

### Corrutinas

```kotlin
viewModelScope.launch {
    try {
        val user = repository.getUser(userId)
        state.value = ProfileState.Content(user)
    } catch (exception: CancellationException) {
        throw exception
    } catch (exception: IOException) {
        state.value = ProfileState.Error
    } finally {
        state.update { it.copy(isLoading = false) }
    }
}
```

`CancellationException` no debe ignorarse, sino volver a lanzarse; de lo
contrario, se interrumpe la cancelación cooperativa.

El bloque `finally` de una corrutina cancelada se ejecutará, pero una operación de
limpieza suspendible puede recibir inmediatamente la cancelación. Para una
limpieza suspendible breve y obligatoria se utiliza en ocasiones
`withContext(NonCancellable)`.

**En resumen:** `try` ejecuta una operación, `catch` trata la excepción
correspondiente y `finally` realiza la limpieza independientemente del resultado.
En las corrutinas es importante no ignorar `CancellationException`, y para cerrar
recursos suele ser preferible utilizar `use`.

</details>

<details>

<summary>144. ¿Cuál es la diferencia entre las excepciones comprobadas y no comprobadas?</summary>

#### Kotlin

En Java, una excepción comprobada debe capturarse o declararse mediante `throws`.
Las excepciones no comprobadas no tienen este requisito. Kotlin no tiene
excepciones comprobadas: el compilador no obliga a escribir `try/catch`.

### Java

Excepción comprobada:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

El código llamador debe utilizar `try/catch` o declarar también `throws`.

Las excepciones no comprobadas son `RuntimeException` y sus subclases:

```java
String value = null;
value.length(); // NullPointerException
```

El compilador no exige tratar `NullPointerException`, `IllegalArgumentException`
ni `IllegalStateException`.

`Error` tampoco es una excepción comprobada, pero normalmente no está destinado
a la recuperación.

### Kotlin

Kotlin permite invocar una API de Java o Kotlin que pueda lanzar `IOException`
sin utilizar obligatoriamente `catch`:

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

La excepción puede producirse igualmente durante la ejecución, por lo que el
contrato de errores debe expresarse de forma explícita mediante la API, la
documentación o el tipo de retorno.

### Resultado esperado frente a error de programación

En la práctica, conviene distinguir:

- situaciones esperadas y recuperables: sin conexión, credenciales no válidas,
  recurso no encontrado;
- errores de programación: argumento no válido, estado imposible o índice
  incorrecto.

Para una infracción del contrato resulta apropiado lanzar una excepción:

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

Para los resultados esperados del dominio es preferible utilizar un resultado
sellado:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

De este modo, el código llamador conoce los estados previstos a través del
sistema de tipos.

### Interoperabilidad con Java

Para que el código Java llamador pueda ver `throws`, Kotlin proporciona
`@Throws`:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

Para el código Kotlin llamador, `catch` sigue siendo opcional.

### Corrutinas

Las excepciones de las corrutinas tampoco son comprobadas. `launch` propaga el
fallo mediante la jerarquía de jobs, mientras que `async` devuelve la excepción
al invocar `await()`.

`CancellationException` es una señal de cancelación y no debe ignorarse:

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

**En resumen:** las excepciones comprobadas son un mecanismo de Java que exige
utilizar `catch` o `throws`; el compilador no controla las excepciones no
comprobadas. Kotlin no tiene excepciones comprobadas, por lo que es preferible
modelar los errores esperados mediante tipos de retorno y reservar las
excepciones para las infracciones de contratos.

</details>

<details>

<summary>145. ¿Qué tipos de errores existen en Kotlin/Java?</summary>

#### Kotlin

En la JVM, todo lo que puede lanzarse mediante `throw` hereda de `Throwable`. Sus
dos ramas principales son `Exception` y `Error`.

```text
Throwable
├── Exception
│   ├── RuntimeException
│   ├── IOException
│   ├── SQLException
│   └── ...
└── Error
    ├── OutOfMemoryError
    ├── StackOverflowError
    └── ...
```

Kotlin utiliza la misma jerarquía de la JVM, pero no tiene excepciones
comprobadas a nivel del compilador.

### `Throwable`

Es el tipo base de todas las excepciones y errores. Por lo general, capturar
`Throwable` es peligroso, ya que puede interceptar un `Error` fatal o la
cancelación de una corrutina.

### `Exception`

Representa situaciones que el código de la aplicación puede llegar a tratar:

- `IOException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

### `RuntimeException`

Es una excepción no comprobada que suele indicar un error de programación o una
infracción del contrato:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Algunos ejemplos son `NullPointerException`, `IndexOutOfBoundsException`,
`ClassCastException` y `NumberFormatException`.

### Excepciones comprobadas y no comprobadas

Java exige utilizar `catch` o `throws` con las excepciones comprobadas, como
`IOException`. Kotlin no lo exige. `RuntimeException` y sus subclases son
excepciones no comprobadas en ambos lenguajes.

### `Error`

Representa un problema grave de la JVM o del entorno de ejecución:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Por lo general, el código de la aplicación no debe intentar recuperarse después
de un `Error`.

### Errores de dominio

Es preferible modelar los fallos de negocio esperados mediante un tipo
independiente, en lugar de utilizar excepciones:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Las excepciones técnicas se transforman en errores de dominio en el límite de la
capa de datos:

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

### `CancellationException`

En las corrutinas es una señal de cancelación, no un fallo convencional:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

No debe ignorarse dentro de un `catch` genérico.

**En resumen:** `Throwable` se divide en `Exception` y `Error`. Las excepciones
de ejecución suelen indicar errores de programación o del contrato; es
preferible modelar los fallos de negocio esperados mediante tipos de dominio, y
en las corrutinas debe propagarse `CancellationException`.

</details>

<details>

<summary>146. ¿Cuál es la clase base de los errores?</summary>

#### Kotlin

La clase base de todo lo que puede lanzarse mediante `throw` y capturarse
mediante `catch` es `Throwable`.

```text
Throwable
├── Exception
│   ├── RuntimeException
│   ├── IOException
│   └── ...
└── Error
    ├── OutOfMemoryError
    ├── StackOverflowError
    └── ...
```

`Throwable` contiene información básica:

- mensaje;
- causa;
- traza de la pila;
- excepciones suprimidas.

Solo se puede lanzar un objeto `Throwable` o una de sus subclases:

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // error de compilación
```

`Exception` representa problemas que el código de la aplicación podría tratar:

```kotlin
throw IOException("No internet")
throw IllegalStateException("User is not authorized")
```

`Error` representa problemas graves de la JVM o del entorno de ejecución:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Por lo general, no se capturan con el propósito de recuperarse.

Excepción personalizada:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

Para los resultados de negocio esperados suele ser preferible utilizar un tipo
sellado en lugar de una excepción:

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

No conviene capturar `Throwable` sin necesidad:

```kotlin
catch (throwable: Throwable) {
    log(throwable)
}
```

Esto puede interceptar un `Error` o una `CancellationException`. En las
corrutinas, la cancelación debe propagarse.

**En resumen:** la clase base es `Throwable`, de la que derivan `Exception` y
`Error`. En producción es preferible capturar excepciones recuperables concretas,
no cualquier `Throwable`.

</details>

<details>

<summary>147. ¿Qué es la complejidad de los algoritmos?</summary>

#### Kotlin

La complejidad de un algoritmo describe cómo aumentan el tiempo de ejecución o
la memoria adicional a medida que crece la entrada `n`. La notación más habitual
es Big O.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

En una entrevista es importante precisar si se habla de complejidad media, del
peor caso o amortizada.

### `O(1)`

El coste no aumenta con `n`:

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

El acceso por índice en `ArrayList` es `O(1)`. La búsqueda en `HashMap` tiene una
complejidad media de `O(1)`.

### `O(n)`

Un recorrido completo de la colección:

```kotlin
fun findUser(users: List<User>, id: String): User? =
    users.firstOrNull { it.id == id }
```

Para una lista, `map`, `filter` y `contains` suelen ser `O(n)`.

### `O(n²)`

Una comparación anidada de numerosos pares:

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

Para detectar duplicados es preferible utilizar un `Set`:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = HashSet<String>()
    return items.any { !seen.add(it) }
}
```

El tiempo medio es `O(n)` y el espacio es `O(n)`: se intercambia memoria por
velocidad.

### `O(log n)`

Búsqueda binaria en datos ordenados:

```kotlin
val index = sortedItems.binarySearch(target)
```

La búsqueda es `O(log n)`, aunque ordenar previamente puede costar
`O(n log n)`.

### `O(n log n)`

Es la complejidad habitual de los algoritmos de ordenación por comparación:

```kotlin
val sorted = users.sortedBy(User::name)
```

### Tiempo frente a espacio

```kotlin
val copy = users.toList()
```

El tiempo es `O(n)` y la memoria adicional es `O(n)`. Un algoritmo in-place
puede ahorrar memoria, pero modifica la entrada y complica la propiedad y la
concurrencia.

### Ejemplo en Android

Incorrecto:

```kotlin
users.map { user ->
    user.id in selectedIds // selectedIds: List, contains O(m)
}
```

La complejidad total es `O(n × m)`.

Es preferible:

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

Construir el conjunto cuesta `O(m)` y realizar el mapeo `O(n)`, por lo que el
total medio es `O(n + m)`.

Big O no equivale al rendimiento real: también importan las constantes, las
asignaciones, la E/S, la localidad de caché y el presupuesto del subproceso
principal. Después de elegir la estructura de datos adecuada, el resultado debe
comprobarse con un profiler o un benchmark.

**En resumen:** la complejidad muestra cómo escalan el tiempo y el espacio. A
menudo puede mejorarse un algoritmo `O(n²)` mediante `Set` o `Map`, pero deben
considerarse la memoria, los casos medio y peor y las mediciones reales.

</details>

<details>

<summary>148. ¿Qué es la búsqueda en profundidad (DFS)?</summary>

#### Kotlin

`DFS` (`Depth-First Search`) es un algoritmo para recorrer un árbol o un grafo
que avanza todo lo posible por un camino y después retrocede para explorar las
demás ramas.

Ejemplo de recorrido de un árbol:

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Un posible orden DFS es:

```text
A -> B -> D -> E -> C -> F
```

### DFS para un árbol

```kotlin
data class TreeNode(
    val value: String,
    val children: List<TreeNode> = emptyList()
)

fun dfs(node: TreeNode) {
    println(node.value)

    for (child in node.children) {
        dfs(child)
    }
}
```

En un árbol, normalmente no se necesita `visited` porque no hay ciclos.

### DFS para un grafo

Un grafo puede contener ciclos, por lo que se necesita `visited`:

```kotlin
fun dfs(
    graph: Map<String, List<String>>,
    start: String,
    visited: MutableSet<String> = mutableSetOf()
) {
    if (start in visited) return

    visited.add(start)
    println(start)

    for (neighbor in graph[start].orEmpty()) {
        dfs(graph, neighbor, visited)
    }
}
```

Sin `visited`, DFS puede entrar en un bucle.

### DFS iterativo

La recursión puede sustituirse por una pila:

```kotlin
fun dfsIterative(
    graph: Map<String, List<String>>,
    start: String
) {
    val visited = mutableSetOf<String>()
    val stack = ArrayDeque<String>()

    stack.addLast(start)

    while (stack.isNotEmpty()) {
        val node = stack.removeLast()
        if (!visited.add(node)) continue

        println(node)

        for (neighbor in graph[node].orEmpty()) {
            if (neighbor !in visited) {
                stack.addLast(neighbor)
            }
        }
    }
}
```

`ArrayDeque` se utiliza como pila con comportamiento LIFO mediante
`addLast/removeLast`.

### Complejidad

Para un grafo:

```text
Tiempo:  O(V + E)
Espacio: O(V)
```

`V` representa los vértices y `E` las aristas. El espacio se utiliza para
`visited` y para la pila de recursión o la pila explícita.

### DFS frente a BFS

DFS:

- avanza en profundidad;
- utiliza recursión o una pila;
- resulta útil para detectar ciclos, hallar componentes conexos y realizar una
  ordenación topológica;
- no garantiza el camino más corto en un grafo no ponderado.

BFS:

- avanza por niveles;
- utiliza una cola;
- encuentra el camino más corto en un grafo no ponderado.

Un DFS recursivo puede provocar `StackOverflowError` en una estructura muy
profunda. En ese caso es preferible utilizar la variante iterativa.

**En resumen:** DFS recorre en profundidad mediante recursión o una pila. En un
grafo requiere `visited`, tiene una complejidad de `O(V + E)` y, para estructuras
muy profundas, es preferible la variante iterativa.

</details>

<details>

<summary>149. ¿Qué es la búsqueda en anchura (BFS)?</summary>

#### Kotlin

`BFS` (`Breadth-First Search`) es un algoritmo que recorre un grafo o un árbol
nivel por nivel. Primero visita todos los vértices situados a una arista del
inicio, después los que están a dos, luego a tres, etc.

BFS utiliza una cola con comportamiento FIFO:

```text
inicio -> vecinos -> vecinos de los vecinos -> ...
```

### BFS para un grafo

```kotlin
fun bfs(graph: Map<Int, List<Int>>, start: Int): List<Int> {
    val visited = mutableSetOf<Int>()
    val queue = ArrayDeque<Int>()
    val result = mutableListOf<Int>()

    visited.add(start)
    queue.addLast(start)

    while (queue.isNotEmpty()) {
        val current = queue.removeFirst()
        result.add(current)

        for (neighbor in graph[current].orEmpty()) {
            if (visited.add(neighbor)) {
                queue.addLast(neighbor)
            }
        }
    }

    return result
}
```

`visited` evita los bucles en grafos con ciclos.

### Camino más corto en un grafo no ponderado

BFS encuentra el número mínimo de aristas desde el punto inicial:

```kotlin
fun shortestDistance(graph: Map<Int, List<Int>>, start: Int): Map<Int, Int> {
    val distance = mutableMapOf(start to 0)
    val queue = ArrayDeque<Int>()
    queue.addLast(start)

    while (queue.isNotEmpty()) {
        val current = queue.removeFirst()

        for (neighbor in graph[current].orEmpty()) {
            if (neighbor !in distance) {
                distance[neighbor] = distance[current]!! + 1
                queue.addLast(neighbor)
            }
        }
    }

    return distance
}
```

Para un grafo ponderado debe utilizarse Dijkstra, no BFS.

### BFS para un árbol

```kotlin
fun levelOrder(root: Node?): List<Int> {
    if (root == null) return emptyList()

    val result = mutableListOf<Int>()
    val queue = ArrayDeque<Node>()
    queue.addLast(root)

    while (queue.isNotEmpty()) {
        val node = queue.removeFirst()
        result.add(node.value)

        node.left?.let(queue::addLast)
        node.right?.let(queue::addLast)
    }

    return result
}
```

Este es un recorrido por niveles.

### Complejidad

```text
Tiempo:  O(V + E)
Espacio: O(V)
```

`V` representa los vértices y `E` las aristas. Para un árbol, la complejidad
temporal es `O(n)`.

### BFS frente a DFS

```text
BFS -> cola, nivel por nivel, camino más corto en un grafo no ponderado
DFS -> pila/recursión, avanza en profundidad
```

BFS suele ser apropiado para problemas como «número mínimo de pasos», «elemento
más cercano» o «distancia mínima en una cuadrícula sin pesos».

**En resumen:** BFS recorre un grafo o un árbol en anchura mediante una cola.
Encuentra el camino más corto en un grafo no ponderado, requiere `visited` para
grafos con ciclos y tiene una complejidad de `O(V + E)`.

</details>

<details>

<summary>150. Explique Clean Architecture.</summary>

#### Kotlin

`Clean Architecture` es un enfoque que divide el código en capas con una
dirección de dependencias claramente definida. Su idea principal es que la lógica
de negocio no dependa de la interfaz de usuario, del framework Android, de la
base de datos ni del cliente de red.

Estructura habitual en Android:

```text
presentación -> dominio <- datos
```

- `presentación`: Activity, Fragment o Compose, ViewModel y estado de la
  interfaz;
- `dominio`: casos de uso, reglas de negocio, entidades y contratos de
  repositorios;
- `datos`: implementaciones de repositorios, API, Room, DataStore y mapeadores.

Regla de dependencias:

```text
presentación depende del dominio
datos depende del dominio
dominio no depende de ningún elemento externo
```

El dominio no debe conocer Retrofit, Room, `Context`, Compose ni el ciclo de vida
de Android.

### Caso de uso

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

El ViewModel invoca el caso de uso, no directamente la API o el DAO.

### Contrato del repositorio

En el dominio:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

En la capa de datos:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

La capa de datos trabaja con DTO y entidades, y los transforma en modelos de
dominio:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

No conviene entregar directamente DTO ni entidades de Room a la interfaz de
usuario si poseen ciclos de vida o semánticas diferentes.

### Presentación

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

El ViewModel administra el estado de la interfaz de usuario y desconoce el origen
concreto de los datos.

Ventajas:

- la lógica de negocio se prueba sin Android;
- la fuente de datos puede sustituirse;
- la interfaz no queda vinculada a la API o la base de datos;
- los límites resultan comprensibles para el equipo;
- facilita el mantenimiento de productos grandes.

Desventajas:

- más archivos y código repetitivo;
- puede resultar excesiva para un CRUD sencillo;
- las abstracciones deficientes complican el código;
- requiere disciplina respecto a la dirección de dependencias.

Como regla práctica, no debe crearse un caso de uso o una interfaz para cada
getter trivial. Clean Architecture debe reducir el acoplamiento y mejorar la
capacidad de prueba, no limitarse a multiplicar clases.

**En resumen:** Clean Architecture suele dividir una aplicación Android en las
capas de presentación, dominio y datos. El dominio contiene las reglas de negocio
y los contratos, la presentación renderiza el estado y la capa de datos
implementa el acceso a la API y la base de datos. Su objetivo principal es
controlar las dependencias y facilitar las pruebas.

</details>

<details>

<summary>151. ¿Qué es MVVM?</summary>

#### Kotlin

`MVVM` es el patrón arquitectónico `Model-View-ViewModel`, que separa la interfaz
de usuario de la gestión del estado y de la lógica de negocio.

```text
Vista -> ViewModel -> Modelo
```

- `Vista`: renderiza la interfaz y envía las acciones del usuario;
- `ViewModel`: conserva el estado de la pantalla, procesa las acciones e invoca
  los casos de uso;
- `Modelo`: capa de dominio y datos, que incluye casos de uso, repositorios, API
  y base de datos.

### Vista

La interfaz de Compose debe recibir el estado y los callbacks:

```kotlin
@Composable
fun ProfileScreen(
    state: ProfileState,
    onRetryClick: () -> Unit
) {
    if (state.isLoading) CircularProgressIndicator()
    else Text(state.userName)
}
```

La vista no accede directamente a la API o la base de datos ni contiene lógica
de negocio.

### ViewModel

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() {
        viewModelScope.launch {
            _state.value = ProfileState(isLoading = true)
            val profile = loadProfile()
            _state.value = ProfileState(userName = profile.name)
        }
    }
}
```

El ViewModel sobrevive a los cambios de configuración y no depende de una
implementación concreta de la vista.

### Capa de modelo

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

El repositorio oculta la API, Room, la caché y DataStore.

### Flujo de datos

```text
Acción del usuario -> ViewModel -> Caso de uso/Repositorio -> Actualización del estado -> Renderizado de la vista
```

Es preferible definir explícitamente el estado de la pantalla:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

La navegación, los snackbars y los toasts son efectos de una sola ejecución y no
deben almacenarse como estado persistente:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Ventajas:

- menos lógica en la vista;
- el ViewModel resulta fácil de probar;
- el estado sobrevive a la rotación;
- la interfaz puede cambiarse sin reescribir las capas de dominio y datos;
- funciona bien con Compose, XML, StateFlow y LiveData.

Errores habituales:

- un Fragment o Composable contiene lógica de negocio;
- el ViewModel trabaja directamente con Retrofit o Room sin un límite
  arquitectónico;
- se entregan DTO o entidades directamente a la interfaz sin una razón;
- se utilizan múltiples estados mutables independientes en lugar de un estado de
  pantalla;
- los eventos de una sola ejecución se almacenan como estado persistente.

**En resumen:** en MVVM, la vista renderiza el estado, el ViewModel administra el
estado y las acciones, y el modelo, los casos de uso y los repositorios ejecutan
la lógica de negocio y el acceso a datos. Un buen MVVM mantiene ligera la
interfaz de usuario y facilita las pruebas del ViewModel.

</details>

<details>

<summary>152. ¿Cuál es la diferencia entre MVVM y MVI?</summary>

#### Kotlin

`MVVM` y `MVI` son enfoques para administrar el estado de la interfaz de usuario.
Ambos separan la interfaz de la lógica de negocio, pero organizan de forma
distinta el estado, los eventos y los efectos secundarios.

### MVVM

```text
Vista -> ViewModel -> Modelo
```

- la `Vista` renderiza la interfaz e invoca métodos del ViewModel;
- el `ViewModel` conserva el estado e invoca casos de uso o repositorios;
- el `Modelo` corresponde a las capas de dominio y datos.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() { /* actualizar el estado */ }
    fun retry() { /* actualizar el estado */ }
}
```

MVVM es más sencillo y flexible. Sin disciplina, no obstante, el ViewModel puede
convertirse en un conjunto desordenado de métodos y estados mutables.

### MVI

```text
Evento/Intent -> ViewModel/Reducer -> Estado -> Interfaz
                                    └-> Efecto
```

La interfaz envía eventos o intenciones en lugar de invocar numerosos métodos
distintos:

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

Normalmente existe un único estado inmutable de pantalla:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

El punto de entrada suele ser único:

```kotlin
fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.LoadClicked -> load()
        ProfileEvent.RetryClicked -> retry()
    }
}
```

### Estado y efectos

En MVVM, el estado puede representarse mediante un único objeto o varios flujos.
En MVI suele utilizarse un único estado de pantalla y un reducer o handler
formal.

En ambos enfoques, la navegación, los snackbars y los toasts no deben guardarse
como estado persistente. Para ello se utilizan efectos:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

### Cuándo elegir cada uno

MVVM:

- es más sencillo;
- requiere menos código repetitivo;
- resulta adecuado para la mayoría de las pantallas de Android;
- facilita la incorporación inicial del equipo.

MVI:

- ofrece un flujo de datos unidireccional y predecible;
- utiliza un único estado inmutable;
- facilita las pruebas de la lógica de eventos y reducers;
- resulta más adecuado para pantallas complejas con mucho estado;
- es útil para equipos grandes que desean un flujo uniforme.

La desventaja de MVI es el código repetitivo en pantallas sencillas. La desventaja
de MVVM es el riesgo de una gestión caótica del estado.

**En resumen:** MVVM es más sencillo: la vista invoca métodos del ViewModel y
renderiza el estado. MVI es más formal: la interfaz envía eventos, el ViewModel o
reducer genera un estado inmutable y las acciones de una sola ejecución se
transmiten mediante efectos. MVVM es adecuado para la mayoría de las pantallas;
MVI, para flujos complejos con mucho estado.

</details>

<details>

<summary>153. ¿Qué patrones arquitectónicos se utilizan en Android?</summary>

#### Kotlin

En Android se utilizan principalmente MVVM, MVI, Clean Architecture, Repository,
Use Case, arquitectura por capas, inyección de dependencias y modularización por
funcionalidades. La elección depende de la complejidad de la aplicación, del
equipo y de los requisitos de capacidad de prueba.

### MVVM

```text
Vista -> ViewModel -> Modelo
```

La interfaz renderiza el estado, mientras que el ViewModel procesa las acciones
del usuario e invoca las capas de dominio y datos.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Funciona bien con Compose, XML, StateFlow y LiveData.

### MVI

```text
Evento/Intent -> ViewModel/Reducer -> Estado -> Interfaz
```

La interfaz envía eventos, el ViewModel genera un estado inmutable y las acciones
de una sola ejecución se transmiten mediante efectos.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

Resulta útil para pantallas complejas con mucho estado.

### Clean Architecture y arquitectura por capas

```text
presentación -> dominio <- datos
```

- `presentación`: interfaz y ViewModel;
- `dominio`: casos de uso, reglas de negocio y contratos;
- `datos`: implementaciones de repositorios, API y base de datos.

El dominio no debe depender del framework Android, Retrofit ni Room.

### Patrón Repository

Un repositorio oculta las fuentes de datos:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

La implementación puede trabajar con una API, Room, una caché o DataStore sin que
el ViewModel lo sepa.

### Patrón Use Case

Un caso de uso describe una acción de negocio concreta:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

Esto evita que el ViewModel acumule lógica de negocio.

### Modularización por funcionalidades

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Los módulos organizados por funcionalidades facilitan el crecimiento del código
y la asignación de responsabilidades. Los módulos centrales solo deben contener
infraestructura realmente compartida.

### Inyección de dependencias

La inyección de dependencias no es un patrón de interfaz de usuario, pero forma
parte de la arquitectura:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt, Koin o la inyección manual conectan los contratos con sus implementaciones.

Elección práctica:

- aplicación sencilla: MVVM y Repository;
- estado de pantalla complejo: MVI;
- aplicación grande: Clean Architecture y modularización;
- enfoque offline-first: Repository y una fuente de verdad local;
- muchos equipos: módulos por funcionalidades y contratos claros.

**En resumen:** la arquitectura de Android suele combinar MVVM o MVI para el
estado de la interfaz, Clean Architecture o capas para dirigir las dependencias,
Repository y Use Case para delimitar el dominio y los datos, inyección de
dependencias para conectar los componentes y módulos por funcionalidades para
facilitar el crecimiento. Un patrón debe reducir la complejidad, no generar
código repetitivo.

</details>

<details>

<summary>154. ¿Cómo se organizan los módulos en un proyecto Android?</summary>

#### Kotlin

Los módulos de un proyecto Android sirven para aislar componentes, mejorar el
rendimiento de compilación, asignar la responsabilidad de las funcionalidades y
permitir el trabajo paralelo de los equipos. Lo importante no es la cantidad de
módulos, sino definir correctamente sus límites y la dirección de sus
dependencias.

Estructura habitual:

```text
:app
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
:core:common
:domain:user
:data:user
```

Una aplicación pequeña no necesita decenas de módulos. Conviene comenzar con una
estructura más sencilla.

### `:app`

Es el punto de entrada de la aplicación:

- `Application`;
- `Activity` principal;
- host de navegación;
- composición de la inyección de dependencias;
- variantes y configuración de compilación;
- conexión de los módulos de funcionalidades y datos.

La lógica de negocio no debe residir en `:app`.

### Módulos de funcionalidades

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

Un módulo de funcionalidad contiene la interfaz y la lógica de presentación de
una funcionalidad concreta. Debe trabajar mediante contratos o casos de uso del
dominio, no directamente con implementaciones de Retrofit o Room.

### Módulos centrales

```text
:core:network  -> configuración de Retrofit/OkHttp
:core:database -> configuración de Room
:core:ui       -> sistema de diseño/componentes
:core:common   -> utilidades, dispatchers y tipos de resultado
```

Los módulos centrales no deben depender de módulos de funcionalidades.

### Módulos de dominio

El dominio contiene las reglas de negocio y los contratos:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

El dominio debe ser lo más independiente posible del framework Android.

### Módulos de datos

La capa de datos implementa los contratos del dominio:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Aquí se encuentran los DTO, las entidades de Room, los mapeadores y las fuentes
de datos de la API y la base de datos.

### Dirección de las dependencias

```text
feature -> domain
 data   -> domain
 app    -> feature + data
core    -> no conoce feature; feature puede conocer core
```

No deben permitirse ciclos. Un módulo `core` no debe conocer ninguna
funcionalidad concreta.

### Lógica de compilación

Es preferible extraer la configuración repetitiva de Gradle a complementos de
convenciones:

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

Esto reduce la duplicación de los scripts de compilación.

Cuándo no conviene dividir:

- no existe una responsabilidad independiente;
- no existe un límite reutilizable;
- no aporta ventajas al tiempo de compilación;
- el módulo solo añade código repetitivo de DI, navegación o Gradle.

**En resumen:** los módulos suelen organizarse alrededor de `app`, `feature`,
`core`, `domain` y `data`. Solo conviene dividir cuando existe un límite real:
responsabilidad sobre una funcionalidad, infraestructura reutilizable, un
contrato de dominio, una implementación de datos o una mejora de compilación. La
modularización excesiva resulta perjudicial.

</details>

<details>

<summary>155. ¿Qué es un token?</summary>

#### Kotlin

Un token es una credencial que confirma una sesión o un derecho de acceso. En los
sistemas móviles y de backend suelen utilizarse un `access token` y un `refresh
token`.

### Access token

Tiene un TTL breve y se envía en las solicitudes a una API protegida:

```http
GET /profile
Authorization: Bearer eyJhbGciOi...
```

```kotlin
class AuthInterceptor(
    private val tokenStorage: TokenStorage
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenStorage.getAccessToken()

        val request = chain.request()
            .newBuilder()
            .header("Authorization", "Bearer $token")
            .build()

        return chain.proceed(request)
    }
}
```

### Refresh token

Tiene una duración mayor y se utiliza exclusivamente para renovar el access
token:

```text
solicitud -> 401 -> renovación -> nuevo access token -> reintento de la solicitud
```

La renovación debe sincronizarse: varios errores `401` simultáneos no deben
iniciar múltiples solicitudes de renovación. La cantidad de reintentos debe
limitarse para evitar un bucle infinito.

### JWT y Bearer

Un JWT tiene el siguiente formato:

```text
header.payload.signature
```

El payload solo está codificado con Base64Url, no cifrado. Por tanto, no deben
almacenarse datos secretos en el payload de un JWT.

Cualquier persona que posea un bearer token puede utilizarlo. Si el token se
filtra, podrá accederse al sistema hasta que caduque o sea revocado.

### Almacenamiento en Android

Opciones para almacenar información sensible:

- Android Keystore;
- EncryptedSharedPreferences;
- DataStore cifrado.

Es preferible ocultar la implementación tras una interfaz:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Los tokens no deben aparecer en registros, informes de fallos, analíticas,
capturas de pantalla, parámetros de consulta de URL ni archivos sin cifrar.

### Caducidad y ámbitos

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)
```

Comprobar localmente la caducidad permite renovar el token con antelación, pero
el backend sigue siendo la fuente de verdad. Si la renovación falla, debe
eliminarse la sesión y enviarse al usuario a la pantalla de inicio de sesión.

Los ámbitos deben reducirse al mínimo, por ejemplo, `profile:read` y
`payments:create`. Una aplicación móvil no debe recibir permisos de administrador
ni secretos del servidor.

Al cerrar la sesión deben eliminarse los tokens locales y, si el backend lo
permite, revocarse el refresh token.

**En resumen:** un token es una credencial de acceso. El access token se envía en
las solicitudes a la API y el refresh token permite renovarlo. Son esenciales un
TTL breve, ámbitos mínimos, almacenamiento seguro, renovación sincronizada y la
ausencia total de tokens en los registros.

</details>

<details>

<summary>156. ¿Cuál es la diferencia entre autenticación y autorización?</summary>

#### Kotlin

```text
Autenticación -> ¿quién eres?
Autorización  -> ¿qué tienes permitido hacer?
```

La autenticación establece una identidad o principal. La autorización comprueba
la política aplicable a una operación o un recurso concretos.

### Autenticación

La identidad puede confirmarse mediante una contraseña, un código OTP, una
passkey o datos biométricos, OAuth/OIDC o una sesión vigente.

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Después de iniciar sesión, el backend emite una cookie de sesión o tokens. El
access token se añade a las solicitudes a la API, mientras que el refresh token
solo se utiliza para obtener un nuevo access token.

### Autorización

La autorización comprueba si el principal tiene derecho a ejecutar una acción:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

El backend tiene en cuenta funciones, permisos, propiedad del recurso, tenant y
contexto.

Modelos habituales:

- RBAC: basado en funciones;
- basado en permisos: permisos granulares;
- ABAC o basado en políticas: atributos del principal, recurso y contexto.

### `401` frente a `403`

```text
401 Unauthorized -> no existen credenciales de autenticación válidas
403 Forbidden    -> existen credenciales, pero el acceso está prohibido
```

Ante un `401`, la aplicación puede intentar una vez una renovación sincronizada
del token. Si la renovación falla, debe finalizar la sesión.

Ante un `403`, volver a autenticarse normalmente no resolverá el problema: la
interfaz debe indicar que no se dispone de acceso.

### Cliente Android frente a backend

El cliente puede ocultar botones en función de los permisos:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDelete) {
        Text("Delete")
    }
}
```

Sin embargo, esto solo mejora la experiencia de usuario. Un cliente modificado
puede invocar directamente la API, por lo que el backend siempre debe aplicar la
autorización.

Distribución de responsabilidades:

- el cliente almacena las credenciales y trata las respuestas `401` y `403`;
- el servicio de autenticación emite y verifica las credenciales;
- la capa de políticas decide el acceso;
- el backend no confía en permisos proporcionados por el cliente;
- el cierre de sesión o la revocación finalizan la sesión de acuerdo con el
  modelo de seguridad.

**En resumen:** la autenticación confirma la identidad y la autorización
comprueba el acceso a una operación o un recurso. El cliente administra las
credenciales y la experiencia de usuario, pero la aplicación real de las reglas
de seguridad siempre debe realizarse en el backend.

</details>

<details>

<summary>157. ¿Qué herramientas se utilizan para comunicarse con el backend?</summary>

#### Kotlin

La pila de red depende del protocolo: REST/HTTP, GraphQL, WebSocket, gRPC o
conexiones de red para KMP. También se necesitan serialización, autenticación,
transformación de errores y una estrategia de caché o funcionamiento sin
conexión.

### REST/HTTP

La combinación más habitual es Retrofit y OkHttp.

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

OkHttp ejecuta las solicitudes y Retrofit describe la API mediante una interfaz
de Kotlin.

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Los interceptores añaden cabeceras, autenticación, registro y trazabilidad. Los
reintentos deben tener en cuenta la idempotencia.

### Serialización

Para JSON se utilizan Kotlin Serialization, Moshi o Gson.

Es preferible separar los DTO del dominio:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### KMP

En Kotlin Multiplatform suele utilizarse Ktor Client. El motor y la configuración
específica de cada plataforma se proporcionan por separado.

### GraphQL

Apollo Kotlin genera modelos seguros respecto a los tipos a partir de operaciones
GraphQL. GraphQL incorpora un esquema y generación de código, además de su propio
sistema de caché y particularidades para tratar errores.

### WebSocket

Se utiliza para comunicación en tiempo real, como chats, actualizaciones en vivo
o datos de mercado. En Android, OkHttp y Ktor ofrecen compatibilidad con
WebSocket.

### gRPC

gRPC es un sistema RPC basado en contratos que utiliza protobuf y clientes
generados. Resulta apropiado cuando el backend ya dispone de infraestructura
protobuf/gRPC.

### Autenticación

Opciones habituales:

- tokens de acceso y renovación Bearer;
- OAuth/OIDC;
- cookies;
- claves de API.

Un interceptor añade las credenciales. Un `Authenticator` o un administrador de
sesiones independiente coordina la renovación después de un `401`. La renovación
debe estar sincronizada para evitar que varios errores `401` simultáneos inicien
múltiples solicitudes.

### Tratamiento de errores y caché

La capa de red debe transformar los errores de transporte y HTTP en errores
propios de las capas de datos o dominio. La interfaz de usuario no debe depender
de `HttpException` ni de un cliente concreto.

Caché y funcionamiento sin conexión:

- caché HTTP: caché de respuestas;
- Room: fuente de verdad estructurada;
- Paging y RemoteMediator: sincronización local y remota paginada;
- Repository: decide de dónde leer y cuándo actualizar.

Elección práctica:

- REST: Retrofit y OkHttp;
- KMP: Ktor Client;
- GraphQL: Apollo Kotlin;
- tiempo real: WebSocket;
- RPC con protobuf: gRPC;
- fuente de verdad sin conexión: Room y Repository.

**En resumen:** para REST suelen utilizarse Retrofit y OkHttp; para KMP, Ktor;
para GraphQL, Apollo; para tiempo real, WebSocket; y para RPC, gRPC. Además de las
solicitudes, son fundamentales la autenticación, la transformación de errores,
el mapeo entre DTO y dominio y la estrategia sin conexión.

</details>

<details>

<summary>158. ¿Qué es Retrofit?</summary>

#### Kotlin

`Retrofit` es una biblioteca para trabajar con API REST en Android y la JVM.
Permite describir endpoints HTTP mediante interfaces de Kotlin o Java y ejecuta
las solicitudes a través de OkHttp.

Interfaz de la API:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto

    @POST("users")
    suspend fun createUser(@Body body: CreateUserRequest): UserDto
}
```

Retrofit crea durante la ejecución una implementación de esta interfaz.

Configuración:

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val api = retrofit.create(UserApi::class.java)
```

`baseUrl` debe terminar con `/`.

Retrofit admite funciones `suspend`:

```kotlin
val user = api.getUser("123")
```

Las anotaciones describen la solicitud HTTP:

```kotlin
@GET("search")
suspend fun search(
    @Query("q") query: String,
    @Query("page") page: Int
): SearchResponse
```

```kotlin
@POST("login")
suspend fun login(@Body request: LoginRequest): LoginResponse
```

### Convertidores

Retrofit no analiza JSON por sí solo. Necesita un convertidor:

- Moshi;
- Gson;
- Kotlin Serialization;
- Scalars.

Es preferible transformar los DTO en modelos de dominio:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### Tratamiento de respuestas

Es posible devolver directamente un DTO o un `Response<T>`:

```kotlin
suspend fun getUser(id: String): Response<UserDto>
```

`Response<T>` resulta necesario cuando deben consultarse el código de estado, las
cabeceras o el cuerpo del error.

### Interceptores de OkHttp

La autenticación, las cabeceras, los registros y la caché se añaden mediante
OkHttp:

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor { chain ->
        val request = chain.request().newBuilder()
            .addHeader("Authorization", "Bearer $token")
            .build()
        chain.proceed(request)
    }
    .build()
```

### Tratamiento de errores

Retrofit y OkHttp pueden producir:

- `IOException`: error de red;
- `HttpException` o un `Response` con estado distinto de 2xx;
- una excepción de serialización.

En el repositorio es preferible transformar estos errores en errores propios de
las capas de dominio o datos, en lugar de entregar `HttpException` a la interfaz
de usuario.

Límite habitual en producción:

```text
ViewModel -> Caso de uso -> Repositorio -> API de Retrofit
```

**En resumen:** Retrofit describe endpoints REST mediante interfaces y
anotaciones, funciona sobre OkHttp y admite corrutinas y convertidores JSON. En
producción, la API de Retrofit y los DTO deben permanecer en la capa de datos,
detrás de un repositorio.

</details>

<details>

<summary>159. ¿Qué es Apollo GraphQL?</summary>

#### Kotlin

Apollo Kotlin es un cliente GraphQL seguro respecto a los tipos para Kotlin y
Android. Genera código Kotlin a partir de un esquema y operaciones `.graphql`, y
admite consultas, mutaciones, suscripciones y caché normalizada.

En GraphQL, el cliente especifica los campos que necesita:

```graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
    posts {
      id
      title
    }
  }
}
```

Apollo genera tipos de Kotlin:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

Si una consulta no se ajusta al esquema, se producirá un error durante la
generación del código o la compilación.

Configuración del cliente:

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

La autenticación se añade mediante un interceptor:

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .addHttpInterceptor { request, chain ->
        val token = tokenStorage.getAccessToken()
        val newRequest = request.newBuilder()
            .addHeader("Authorization", "Bearer $token")
            .build()

        chain.proceed(newRequest)
    }
    .build()
```

Consulta:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()
val user = response.data?.user
```

Mutación:

```kotlin
apolloClient.mutation(UpdateUserNameMutation(id, name)).execute()
```

Suscripción para actualizaciones en tiempo real:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
    }
```

### Caché y errores

La caché normalizada almacena las entidades por clave para que distintas
consultas puedan reutilizar los mismos datos. Esto requiere identificadores
estables en el esquema.

Una respuesta GraphQL puede contener simultáneamente `data` y `errors`:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()

if (response.hasErrors()) {
    // transformar response.errors en errores de dominio
}

val data = response.data
```

Un error de GraphQL no implica necesariamente una respuesta HTTP 4xx o 5xx. Un
fallo de red suele llegar como una excepción. Es preferible transformar los
modelos generados en modelos de dominio.

Apollo frente a Retrofit:

- Apollo: GraphQL, el cliente define la forma de la respuesta y los modelos se
  generan;
- Retrofit: REST, el backend define los endpoints y las respuestas, y los DTO se
  escriben manualmente.

**En resumen:** Apollo Kotlin es un cliente GraphQL seguro respecto a los tipos
que ofrece generación de código, consultas, mutaciones, suscripciones y caché
normalizada. Se utiliza cuando el backend funciona mediante GraphQL en lugar de
endpoints REST.

</details>

<details>

<summary>160. ¿Para qué se utiliza Firebase?</summary>

#### Kotlin

`Firebase` es un conjunto de herramientas backend como servicio de Google para
aplicaciones móviles y web. En Android se utiliza para analíticas, informes de
fallos, notificaciones push, autenticación, configuración remota, bases de datos,
almacenamiento y funciones de backend.

### Analytics

Permite recopilar eventos de usuario, embudos, retención y uso de
funcionalidades:

```kotlin
firebaseAnalytics.logEvent(
    "profile_opened",
    bundleOf("source" to "home")
)
```

### Crashlytics

Supervisa fallos y errores no fatales:

```kotlin
Firebase.crashlytics.recordException(error)
```

Es una herramienta básica de producción para garantizar la estabilidad de la
aplicación.

### FCM

Firebase Cloud Messaging proporciona notificaciones push y eventos de
mensajería:

```kotlin
class AppMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // procesar la notificación push
    }
}
```

### Firebase Auth

Es una plataforma de autenticación lista para usar:

- correo electrónico y contraseña;
- inicio de sesión con Google;
- autenticación telefónica;
- autenticación anónima;
- tokens personalizados.

Debe integrarse correctamente con el backend y el modelo de seguridad.

### Firestore y Realtime Database

Son bases de datos NoSQL en la nube con actualizaciones en tiempo real:

```kotlin
firestore.collection("users").document(id).get()
```

Resultan apropiadas para chats, sincronización en tiempo real, funciones
colaborativas y prototipos. Las reglas de seguridad son fundamentales.

### Remote Config

Permite cambiar parámetros sin publicar una nueva versión de la aplicación:

```kotlin
val enabled = remoteConfig.getBoolean("new_feature_enabled")
```

Casos de uso: indicadores de funcionalidades, despliegues graduales, pruebas A/B
y configuración de la interfaz o del comportamiento.

### Storage y Functions

Cloud Storage permite almacenar archivos subidos por usuarios, como imágenes,
vídeos y documentos.

```kotlin
storage.reference.child("avatars/$userId.jpg")
```

Cloud Functions permite ejecutar lógica en el servidor: activadores de
Firestore, endpoints de API, envío de notificaciones push, validación e
integraciones con terceros.

Ventajas:

- puesta en marcha rápida;
- autenticación, analíticas, informes de fallos y notificaciones push listos para
  usar;
- buena integración con Android;
- funciones en tiempo real;
- menor necesidad inicial de infraestructura backend propia.

Riesgos:

- dependencia del proveedor;
- errores en las reglas de seguridad;
- costes inesperados al crecer;
- no siempre resulta adecuado para una lógica de dominio backend compleja;
- el acceso desde el cliente no sustituye la seguridad del backend;
- el comportamiento sin conexión y de la caché debe comprenderse por separado.

**En resumen:** Firebase acelera el desarrollo en Android mediante servicios
preparados de autenticación, Analytics, Crashlytics, FCM, Firestore o Realtime
Database, Remote Config, Storage y Functions. Sin embargo, requiere reglas de
seguridad, control de costes, límites arquitectónicos y una comprensión clara de
la dependencia del proveedor.

</details>

<details>

<summary>161. ¿Qué es Android Keystore?</summary>

#### Kotlin

`Android Keystore` es un almacén del sistema para claves criptográficas. Permite
crear y utilizar claves sin que el material criptográfico sin procesar abandone
el Keystore. En algunos dispositivos, las claves pueden estar respaldadas por
hardware.

Keystore se utiliza para:

- cifrado y descifrado;
- firma y verificación;
- secretos locales;
- claves para cifrar EncryptedSharedPreferences o DataStore;
- claves protegidas mediante autenticación biométrica o del usuario;
- reducir el riesgo de filtración de una clave criptográfica sin procesar.

Una limitación importante es que Keystore no convierte en segura una clave de API
incluida directamente en el código. Si el secreto se encuentra en el APK, un
atacante puede localizar el lugar donde la aplicación lo utiliza. Keystore
resulta más adecuado para claves generadas en el dispositivo.

Generación de una clave AES:

```kotlin
val keyGenerator = KeyGenerator.getInstance(
    KeyProperties.KEY_ALGORITHM_AES,
    "AndroidKeyStore"
)

val spec = KeyGenParameterSpec.Builder(
    "secret_key",
    KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT
)
    .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
    .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
    .build()

keyGenerator.init(spec)
keyGenerator.generateKey()
```

Obtención de la clave:

```kotlin
val keyStore = KeyStore.getInstance("AndroidKeyStore").apply { load(null) }
val key = keyStore.getKey("secret_key", null) as SecretKey
```

Se obtiene una referencia a la clave, no sus bytes sin procesar.

Cifrado:

```kotlin
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)
val iv = cipher.iv
val encrypted = cipher.doFinal(data)
```

Con AES-GCM, el `iv` debe almacenarse junto al texto cifrado y nunca debe
reutilizarse con la misma clave.

Una clave puede vincularse a la autenticación del usuario:

```kotlin
.setUserAuthenticationRequired(true)
```

Para tareas habituales es preferible utilizar Jetpack Security:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()
```

Keystore y Jetpack Security son apropiados para refresh tokens, claves de cifrado
locales, secretos específicos del usuario y preferencias sensibles. Los secretos
del servidor o backend no deben incluirse en la aplicación; para ellos se
necesita un proxy backend.

Riesgos:

- uso de un modo criptográfico incorrecto;
- reutilización del IV;
- ausencia de rotación de claves;
- casos límite durante copias de seguridad y restauraciones;
- diferencias de comportamiento entre versiones y dispositivos Android;
- intentar ocultar un secreto incluido en el código en lugar de diseñar una
  arquitectura adecuada.

**En resumen:** Android Keystore protege claves criptográficas en el dispositivo
y no expone el material de clave sin procesar. Resulta útil para cifrado y firma
locales y para tokens de usuario, pero no protege secretos de API incluidos en el
APK. Para tareas habituales es preferible utilizar Jetpack Security sobre
Keystore.

</details>

<details>

<summary>162. ¿Qué dispatchers existen en Kotlin Coroutines y para qué se utilizan?</summary>

#### Kotlin

Un `CoroutineDispatcher` determina en qué subproceso o grupo de subprocesos se
ejecuta una corrutina. Elegir el dispatcher correcto es importante para mantener
la capacidad de respuesta de la interfaz, el rendimiento y evitar errores ANR.

### `Dispatchers.Main`

Es el subproceso principal de la interfaz de usuario:

```kotlin
withContext(Dispatchers.Main) {
    textView.text = "Loaded"
}
```

Se utiliza para actualizar la interfaz, administrar el estado de Compose o de
vistas, navegar y ejecutar lógica de interfaz vinculada al ciclo de vida. No debe
utilizarse para operaciones de red, bases de datos, E/S de archivos ni cálculos
pesados.

### `Dispatchers.IO`

Se utiliza para operaciones de E/S bloqueantes:

```kotlin
withContext(Dispatchers.IO) {
    file.readText()
}
```

Es apropiado para operaciones con archivos, bases de datos, clientes de red y
llamadas bloqueantes a SDK. No está destinado a algoritmos intensivos en CPU.

### `Dispatchers.Default`

Se utiliza para tareas intensivas en CPU:

```kotlin
withContext(Dispatchers.Default) {
    largeList.sortedBy { it.score }
}
```

Resulta apropiado para ordenación, análisis de estructuras grandes, compresión,
cálculo de diferencias y otros cálculos.

### `Dispatchers.Unconfined`

No vincula la corrutina a un subproceso concreto y la reanuda en el lugar donde
finaliza la suspensión.

```kotlin
launch(Dispatchers.Unconfined) { }
```

Casi nunca se necesita en código Android de producción, ya que su comportamiento
es menos predecible.

### `limitedParallelism`

```kotlin
val limitedIo = Dispatchers.IO.limitedParallelism(4)
```

Resulta útil para limitar la cantidad de cargas, llamadas a API, tareas por lotes
u operaciones de procesamiento de imágenes que se ejecutan en paralelo.

### `withContext`

Permite cambiar de dispatcher dentro de una función suspendible:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

Es preferible a iniciar una corrutina nueva únicamente para cambiar de
subproceso.

### Inyección de dispatchers

Para facilitar las pruebas, es preferible proporcionar los dispatchers mediante
el constructor:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): User = withContext(ioDispatcher) {
        api.getUser()
    }
}
```

En una prueba puede proporcionarse `StandardTestDispatcher`.

Errores habituales:

- ejecutar operaciones de red o base de datos en `Main`;
- ejecutar tareas intensivas en CPU en `IO`;
- utilizar `runBlocking` en el subproceso principal;
- fijar dispatchers directamente en código que debe probarse;
- crear grupos de subprocesos personalizados sin liberarlos;
- utilizar `Unconfined` sin una razón clara.

**En resumen:** `Main` se utiliza para la interfaz, `IO` para E/S bloqueante,
`Default` para tareas intensivas en CPU y `Unconfined` para casos especiales poco
frecuentes. Para facilitar las pruebas conviene inyectar los dispatchers y
cambiar entre ellos mediante `withContext`.

</details>

<details>

<summary>163. ¿Qué formas existen de iniciar corrutinas?</summary>

#### Kotlin

Una corrutina se inicia dentro de un `CoroutineScope`. Las opciones más
habituales son `launch`, `async`, `coroutineScope`, `supervisorScope` y los
ámbitos específicos de Android.

### `launch`

Inicia una corrutina sin un resultado de negocio y devuelve un `Job`:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

```kotlin
val job = scope.launch { work() }
job.cancel()
```

Es apropiado para efectos secundarios. Los fallos se tratan mediante la jerarquía
del ámbito o con un `try/catch` local.

### `async`

Devuelve un `Deferred<T>` para obtener un resultado concurrente:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

Si se invoca inmediatamente `await()` sin aprovechar la concurrencia, es
preferible llamar directamente a la función suspendible.

### `coroutineScope`

Crea un límite de concurrencia estructurada y espera a todos sus hijos:

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

El fallo de un hijo cancela a sus hermanos y se propaga al código llamador.

### `supervisorScope`

Aísla a los hermanos del fallo de un hijo directo:

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

El fallo sigue teniendo que tratarse o recuperarse mediante `await()`.

### `runBlocking`

Bloquea el subproceso llamador. Resulta apropiado para el punto de entrada de una
aplicación de línea de comandos o para un puente síncrono muy limitado. En las
pruebas es preferible utilizar `runTest`; no debe usarse en el subproceso
principal de Android debido al riesgo de ANR.

### Ámbitos de Android

`viewModelScope` se utiliza para operaciones de pantalla y se cancela en
`onCleared()`.

`lifecycleScope` pertenece a un `LifecycleOwner`. Para recopilar un Flow en la
interfaz se utiliza `repeatOnLifecycle`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

En un Fragment, para la vista o el binding debe utilizarse
`viewLifecycleOwner.lifecycleScope`.

En Compose, `rememberCoroutineScope()` se utiliza para acciones de interfaz
iniciadas mediante callbacks:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

`GlobalScope` casi siempre es una elección incorrecta en el código de una
aplicación: no tiene propietario, ciclo de vida ni limpieza.

**En resumen:** `launch` se utiliza para efectos secundarios, `async` para
resultados concurrentes, `coroutineScope` como límite que falla rápidamente y
`supervisorScope` para aislar a los hermanos. El ámbito debe elegirse según el
propietario de la tarea; no deben utilizarse `GlobalScope` ni `runBlocking` en el
subproceso principal.

</details>

<details>

<summary>164. ¿Cuál es la diferencia entre launch y async?</summary>

#### Kotlin

`launch` y `async` son constructores de corrutinas.

```text
launch -> Job, sin valor de resultado
async  -> Deferred<T>, resultado mediante await()
```

### `launch`

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

Un `Job` permite utilizar `cancel()`, `join()` y consultar su estado. `launch` se
utiliza cuando no es necesario devolver un resultado: actualizar el estado de la
interfaz, recopilar un Flow, guardar o sincronizar datos.

```kotlin
fun load(id: String) {
    viewModelScope.launch {
        state.value = UiState.Loading
        state.value = UiState.Content(repository.getUser(id))
    }
}
```

### `async`

```kotlin
val deferred: Deferred<User> = async {
    repository.getUser()
}

val user = deferred.await()
```

`Deferred<T>` es un `Job` que contiene un resultado o una excepción. `async` se
utiliza principalmente para obtener varios resultados de forma concurrente:

```kotlin
suspend fun loadDashboard(): Dashboard = coroutineScope {
    val user = async { userRepository.getUser() }
    val feed = async { feedRepository.getFeed() }

    Dashboard(
        user = user.await(),
        feed = feed.await()
    )
}
```

Sin `async`, las llamadas suspendibles se ejecutarían secuencialmente. No
obstante, la ejecución paralela real depende del dispatcher.

### Excepciones y concurrencia estructurada

En un `coroutineScope`, el fallo de un hijo cancela a sus hermanos y se propaga
al código llamador. Una excepción de `async` se obtiene mediante `await()`, pero
dentro de un ámbito estructurado el fallo sigue afectando al padre.

Para fallos independientes se necesita `supervisorScope` y una política de
errores explícita.

```kotlin
viewModelScope.launch {
    try {
        repository.sync()
    } catch (exception: CancellationException) {
        throw exception
    } catch (exception: IOException) {
        state.value = UiState.Error
    }
}
```

`CancellationException` no debe ignorarse.

### Errores habituales

Incorrecto:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

Si no se necesita un valor, debe utilizarse `launch`. Si se necesita un único
valor sin concurrencia, debe invocarse directamente la función suspendible. Para
cambiar de dispatcher debe utilizarse `withContext`, no `async`.

Elección práctica:

- efecto secundario sin resultado: `launch`;
- varios resultados concurrentes: `async` y `await`;
- un único resultado: función suspendible;
- cambio de dispatcher: `withContext`;
- hijos relacionados: `coroutineScope`;
- fallos independientes: `supervisorScope`.

**En resumen:** `launch` devuelve un `Job` para una tarea sin resultado. `async`
devuelve un `Deferred<T>` y se utiliza para obtener resultados concurrentes
mediante `await()`. No debe emplearse `async` cuando no se necesita el resultado.

</details>

<details>

<summary>165. ¿Cómo se tratan los errores en las corrutinas?</summary>

#### Kotlin

La estrategia depende del propietario de la tarea. Los fallos esperados se
tratan localmente y se transforman en un resultado o estado. Las excepciones no
tratadas se propagan mediante la jerarquía de `Job`.
`CancellationException` no debe ignorarse.

### `try/catch`

```kotlin
suspend fun loadUser(): Result<User> {
    return try {
        Result.success(api.getUser())
    } catch (error: IOException) {
        Result.failure(error)
    }
}
```

Es preferible capturar excepciones concretas en el límite donde se sabe cómo
tratarlas.

En un ViewModel, el error debe transformarse en un estado de interfaz, en lugar
de entregar directamente `HttpException` o `IOException` a la interfaz.

### `CancellationException`

En una captura amplia, la cancelación debe propagarse:

```kotlin
catch (error: CancellationException) {
    throw error
} catch (error: Exception) {
    handleError(error)
}
```

De lo contrario, puede interrumpirse la cancelación cooperativa.

### `CoroutineExceptionHandler`

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

El handler es un último recurso para una excepción no capturada en un `launch`
raíz y suele emplearse para registrar el fallo. No recupera la corrutina ni
sustituye el tratamiento local de errores.

### Excepciones de `async`

`await()` devuelve el resultado o lanza una excepción:

```kotlin
val deferred = async { api.getUser() }

try {
    val user = deferred.await()
} catch (error: IOException) {
    handleError(error)
}
```

En un `coroutineScope` normal, el fallo de un hijo cancela al padre y a sus
hermanos. Un `try/catch` alrededor de `await()` no siempre aísla las tareas
paralelas. Para fallos independientes se necesita `supervisorScope`.

### `coroutineScope` frente a `supervisorScope`

```text
coroutineScope  -> falla rápidamente; el fallo de un hijo cancela a sus hermanos
supervisorScope -> el fallo de un hijo no cancela automáticamente a sus hermanos
```

```kotlin
supervisorScope {
    val a = async { runCatching { requestA() } }
    val b = async { runCatching { requestB() } }
}
```

El fallo sigue teniendo que tratarse.

### `Flow.catch`

```kotlin
repository.users()
    .catch { error -> emit(emptyList()) }
    .collect { users -> render(users) }
```

`catch` captura las excepciones producidas aguas arriba. La cancelación no debe
transformarse en un valor alternativo.

Errores habituales:

- ignorar `CancellationException`;
- depender únicamente de `CoroutineExceptionHandler`;
- ignorar un fallo de `await()`;
- entregar excepciones sin procesar a la interfaz;
- mezclar reintentos, transformación y renderizado en un mismo lugar.

**En resumen:** los fallos esperados se transforman en un resultado o estado
mediante `try/catch`, `await()` o `Flow.catch`. `coroutineScope` falla
rápidamente, `supervisorScope` aísla a los hermanos y `CancellationException`
siempre debe propagarse.

</details>

<details>

<summary>166. ¿Cómo se espera el resultado de varias solicitudes paralelas?</summary>

#### Kotlin

Las llamadas suspendibles independientes se inician mediante `async` dentro de
un ámbito estructurado y se esperan mediante `await()` o `awaitAll()`.

```kotlin
suspend fun loadScreen(): ScreenData = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }

    ScreenData(
        user = user.await(),
        posts = posts.await()
    )
}
```

Ambos bloques `async` se crean antes del primer `await()`, por lo que las
solicitudes se ejecutan de forma concurrente. El paralelismo real depende del
dispatcher y del cliente HTTP.

### `coroutineScope`

`coroutineScope`:

- espera a todos sus hijos;
- propaga hacia abajo la cancelación del código llamador;
- falla rápidamente: el fallo de un hijo cancela a sus hermanos.

Esta es la política adecuada cuando se necesitan todos los resultados.

### `awaitAll`

Para una lista de solicitudes del mismo tipo:

```kotlin
val users = coroutineScope {
    ids.map { id ->
        async { api.getUser(id) }
    }.awaitAll()
}
```

`awaitAll()` conserva el orden de la lista de objetos `Deferred` y devuelve
rápidamente el fallo.

Las llamadas dependientes no deben paralelizarse:

```kotlin
val token = api.login()
val profile = api.getProfile(token)
```

### Éxito parcial

Si se necesita un resultado parcial, se utiliza `supervisorScope` y un resultado
tipado para cada hijo:

```kotlin
supervisorScope {
    val user = async { runRequest { api.getUser() } }
    val posts = async { runRequest { api.getPosts() } }

    PartialData(
        user = user.await(),
        posts = posts.await()
    )
}
```

`runRequest` debe volver a lanzar `CancellationException` y transformar
únicamente los fallos esperados.

### Tiempo de espera

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

El tiempo de espera de la corrutina cancela el ámbito. El cliente HTTP debe
disponer además de sus propios tiempos de espera de conexión, lectura y llamada.

### Limitación de solicitudes simultáneas

Para una gran cantidad de solicitudes:

```kotlin
val semaphore = Semaphore(5)

val users = coroutineScope {
    ids.map { id ->
        async {
            semaphore.withPermit {
                api.getUser(id)
            }
        }
    }.awaitAll()
}
```

`Semaphore` limita la cantidad de solicitudes simultáneas. Para conjuntos
grandes también se utiliza el procesamiento por lotes.

Elección práctica:

- se necesitan todos los resultados: `coroutineScope` y `async`;
- lista de solicitudes: `awaitAll()`;
- éxito parcial: `supervisorScope` y errores tipados;
- gran cantidad de solicitudes: `Semaphore` o procesamiento por lotes;
- llamadas dependientes: ejecución secuencial.

**En resumen:** las solicitudes paralelas e independientes se inician mediante
`async` dentro de un `coroutineScope`. `awaitAll()` espera una lista. Un ámbito
normal falla rápidamente, `supervisorScope` permite resultados parciales y la
cantidad de solicitudes simultáneas se limita mediante `Semaphore` o lotes.

</details>

<details>

<summary>167. ¿Para qué se crearon los Fragment?</summary>

#### Kotlin

Los `Fragment` se crearon como partes reutilizables de la interfaz y del
comportamiento dentro de una `Activity`. La idea consiste en no concentrar todo
el flujo de una pantalla en una Activity extensa, sino dividir la interfaz en
componentes más pequeños con su propio ciclo de vida y layout.

Problema sin Fragment:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // navegación, lista, detalles, clics y estado
    }
}
```

Una Activity de este tipo se convierte rápidamente en una clase con demasiadas
responsabilidades.

Un Fragment como parte de una pantalla:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // lógica de interfaz de la lista
    }
}
```

Un Fragment puede encapsular una lista, detalles, un formulario, un flujo por
pasos o pestañas.

Históricamente, un caso de uso importante fueron los layouts para teléfonos y
tabletas:

```text
teléfono -> Activity + ListFragment
tableta  -> Activity + ListFragment + DetailsFragment
```

Una Activity podía servir como contenedor de varios Fragment y sustituirlos
mediante FragmentManager:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Posteriormente, Navigation Component simplificó este proceso.

Un Fragment tiene su propio ciclo de vida y un ciclo de vida independiente para
su vista:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

La principal particularidad es que el objeto Fragment puede vivir más tiempo que
su vista. Por ello, el binding, los adapters y las suscripciones de interfaz deben
liberarse en `onDestroyView()`.

Los Fragment pueden tener un ViewModel propio o compartido:

```kotlin
private val viewModel: ProfileViewModel by viewModels()
private val shared: SharedViewModel by activityViewModels()
```

Esto resulta útil para interfaces maestro-detalle, flujos de tipo asistente o
estado de interfaz compartido.

En Compose, los Fragment tienen un papel menor: a menudo una única Activity aloja
un `NavHost` y las pantallas son composables. Sin embargo, siguen siendo
relevantes para el sistema de vistas heredado, aplicaciones híbridas,
configuraciones con Navigation Component y migraciones graduales.

**En resumen:** los Fragment se crearon para modularizar la interfaz en el
sistema de vistas: bloques reutilizables, layouts para teléfonos y tabletas,
navegación dentro de una Activity y gestión de la pila de navegación. Su
principal complejidad es el ciclo de vida, especialmente la diferencia entre el
ciclo de vida del Fragment y el de su vista.

</details>

<details>

<summary>168. ¿Qué problemas resuelven los Fragment?</summary>

#### Kotlin

Los Fragment resuelven problemas de descomposición de interfaces basadas en
vistas dentro de una `Activity`: Activities demasiado grandes, navegación, pila
de navegación, ciclos de vida de partes independientes de una pantalla o
contenedor y layouts distintos para teléfonos y tabletas.

### Activities demasiado grandes

Sin descomposición, una `Activity` se convierte rápidamente en una clase con
demasiadas responsabilidades:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // inicio, detalles, ajustes, navegación y estado
    }
}
```

Los Fragment dividen la interfaz:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

La `Activity` permanece como host, mientras que el estado y la lógica de negocio
deben residir en el `ViewModel` o la capa de dominio.

### Distintos layouts

```text
teléfono -> Activity + un Fragment
tableta  -> Activity + Fragment de lista + Fragment de detalles
```

Un teléfono puede mostrar la lista y los detalles de forma secuencial, mientras
que una tableta puede mostrarlos simultáneamente.

### Navegación y pila de navegación

Los Fragment permiten sustituir partes de una pantalla sin iniciar una nueva
Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Navigation Component permite gestionarlo de forma más controlada mediante un
grafo de navegación.

### Ciclo de vida de la interfaz

Un Fragment tiene su propio ciclo de vida y otro independiente para su vista:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

El binding, los adapters y las suscripciones de interfaz deben liberarse en
`onDestroyView()`, ya que la vista puede destruirse antes que el objeto Fragment.

### Aislamiento de responsabilidades

Un Fragment se encarga del renderizado, del ciclo de vida y de los eventos de
interfaz de una pantalla concreta:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

Sin embargo, no debe contener lógica de negocio.

### Lo que los Fragment no resuelven

- una arquitectura deficiente;
- la mezcla de interfaz y lógica de negocio;
- una propiedad incorrecta del estado;
- fugas de memoria después de `onDestroyView()`;
- un grafo de navegación confuso;
- un anidamiento excesivo de la interfaz.

En el sistema de vistas, un Fragment resulta práctico como pantalla o contenedor.
En una interfaz desarrollada completamente con Compose, su papel suele reducirse
o desaparecer.

**En resumen:** los Fragment permiten descomponer interfaces basadas en vistas,
administrar la navegación y su pila, crear layouts para teléfonos y tabletas y
gestionar el ciclo de vida de partes de una pantalla. Sin embargo, no corrigen
automáticamente la arquitectura, la gestión del estado ni las fugas de memoria.

</details>

<details>

<summary>169. ¿Qué particularidades tienen los Fragment anidados?</summary>

#### Kotlin

Un Fragment anidado reside dentro de otro Fragment y se administra mediante
`childFragmentManager`.

```kotlin
class ParentFragment : Fragment(R.layout.fragment_parent) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        childFragmentManager.beginTransaction()
            .replace(R.id.childContainer, ChildFragment())
            .commit()
    }
}
```

`parentFragmentManager` administra los Fragment del contenedor padre, no los
Fragment hijos del Fragment actual.

### Ciclo de vida y ámbito del ViewModel

Un Fragment hijo tiene su propio ciclo de vida, ciclo de vida de la vista, estado
y ViewModel, pero no puede vivir más tiempo que su padre.

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()`: ámbito del Fragment hijo;
- `requireParentFragment()`: estado compartido con el padre;
- `activityViewModels()`: ámbito de la Activity.

### Navegación y pila de navegación

Un Fragment hijo puede tener su propia pila de navegación o su propio
`NavHostFragment`. Debe definirse claramente quién gestiona la acción Atrás:

```kotlin
childFragmentManager.popBackStack()
```

Varias pilas de navegación anidadas complican rápidamente el comportamiento de
la pantalla, por lo que la navegación anidada solo debe añadirse cuando sea
necesaria.

### Comunicación

No conviene conservar referencias directas entre Fragment. Es preferible
utilizar:

- un ViewModel compartido;
- Fragment Result API;
- un resultado de navegación.

```kotlin
childFragmentManager.setFragmentResultListener(
    "key",
    viewLifecycleOwner
) { _, bundle ->
    val value = bundle.getString("value")
}
```

### Limpieza

El Fragment padre y el hijo tienen ciclos de vida de vista independientes. Las
referencias a vistas, los adapters y los callbacks deben liberarse en
`onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

En un `ViewPager2`, el adapter situado dentro de un Fragment se crea con el
Fragment como propietario:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment)
```

Los Fragment anidados resultan apropiados para pestañas, un flujo local de tipo
asistente o un bloque independiente con su propio ciclo de vida. Para una
interfaz sencilla es preferible utilizar una vista personalizada o un composable.

**En resumen:** los Fragment anidados se administran mediante
`childFragmentManager` y tienen sus propios ciclos de vida, ViewModel y pila de
navegación, pero dependen del padre. Los principales riesgos son utilizar el
manager incorrecto, añadir navegación anidada innecesaria y provocar fugas
después de `onDestroyView()`.

</details>

<details>

<summary>170. ¿Qué callbacks existen en Application?</summary>

#### Kotlin

`Application` representa un proceso de Android. Cada proceso de la aplicación
tiene su propia instancia de `Application`.

Callbacks principales:

```text
onCreate()
onTrimMemory()
onLowMemory()
onConfigurationChanged()
onTerminate()
```

### `onCreate()`

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        initLogging()
        initCrashReporting()
    }
}
```

Aquí se inicia la infraestructura de toda la aplicación: inyección de
dependencias, registro, informes de fallos y SDK. El callback se ejecuta en el
subproceso principal y afecta al arranque en frío, por lo que la inicialización
debe ser mínima o diferida.

En el manifest:

```xml
<application android:name=".App" />
```

Con Hilt, la clase se anota con `@HiltAndroidApp`.

Una particularidad importante es que los inicializadores basados en
`ContentProvider` pueden ejecutarse antes de `Application.onCreate()`.

### Callbacks de memoria

```kotlin
override fun onTrimMemory(level: Int) {
    super.onTrimMemory(level)
    cache.trim(level)
}

override fun onLowMemory() {
    super.onLowMemory()
    cache.clear()
}
```

Son señales para liberar memoria que pueda volver a crearse, como cachés de
imágenes, búferes o datos precalculados. No deben eliminarse datos persistentes.

### Configuración

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

Este callback informa de cambios globales de configuración. Es preferible
gestionar la reacción de la interfaz en una Activity, un Fragment o Compose,
donde existen un ciclo de vida actual y un Context con el tema correspondiente.

### `ActivityLifecycleCallbacks`

Application puede observar el ciclo de vida de todas las Activities:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityStarted(activity: Activity) = Unit
        override fun onActivityResumed(activity: Activity) = Unit
        override fun onActivityStopped(activity: Activity) = Unit
    }
)
```

Casos de uso: analíticas, seguimiento de sesiones y herramientas de depuración.
Para detectar el paso de la aplicación a primer o segundo plano suele ser
preferible utilizar `ProcessLifecycleOwner`.

No debe conservarse una referencia fuerte a una Activity después del callback.

### `onTerminate()`

`onTerminate()` no debe utilizarse para realizar limpieza en producción. Android
puede finalizar el proceso sin invocarlo. No debe emplearse para guardar datos,
cerrar una sesión, completar transacciones ni liberar recursos.

Qué no debe hacerse:

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

Esto provoca una fuga de Activity, estado mutable global y pérdida de datos
después de la finalización del proceso. `Application` no debe convertirse en un
localizador de servicios ni en un almacén de sesiones.

**En resumen:** `Application.onCreate()` se utiliza para una inicialización
rápida de toda la aplicación. `onTrimMemory()` y `onLowMemory()` permiten liberar
memoria que puede volver a crearse. La ejecución de `onTerminate()` no está
garantizada. El ciclo de vida de las Activities puede observarse mediante
`ActivityLifecycleCallbacks`, pero sin conservar referencias a ellas.

</details>

<details>

<summary>171. ¿Qué singleton crea el sistema al iniciar una aplicación Android?</summary>

#### Kotlin

Al iniciar un proceso de Android, el sistema crea una instancia de `Application`.
Se trata de un singleton del ámbito de la aplicación dentro de ese proceso
concreto.

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
    }
}
```

En el manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Android crea `App` e invoca `onCreate()` antes de iniciar una `Activity`, un
`Service`, un `BroadcastReceiver` u otros componentes de ese proceso.

### Singleton únicamente dentro de un proceso

`Application` no es un singleton para todo el dispositivo y puede no ser único
para toda la aplicación si esta utiliza varios procesos.

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

En ese caso habrá dos instancias:

```text
proceso principal -> instancia de App n.º 1
proceso :sync     -> instancia de App n.º 2
```

Por ello, `Application`, los `object` de Kotlin y el estado estático no deben
considerarse almacenamiento compartido entre procesos.

### Para qué se utiliza Application

- configuración de la inyección de dependencias;
- informes de fallos;
- registro;
- configuración de analíticas;
- inicialización de SDK para toda la aplicación;
- `ActivityLifecycleCallbacks`;
- configuración global ligera.

Con Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

### Contexto de Application

Las dependencias de larga duración deben utilizar `applicationContext`, no el
contexto de una `Activity`:

```kotlin
@Provides
@Singleton
fun provideDatabase(
    @ApplicationContext context: Context
): AppDatabase {
    return Room.databaseBuilder(
        context,
        AppDatabase::class.java,
        "app.db"
    ).build()
}
```

### Qué no debe hacerse

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Problemas:

- el estado desaparecerá al finalizar el proceso;
- una referencia a una `Activity` provoca una fuga de memoria;
- la lógica de negocio se convierte en estado mutable global;
- las pruebas pasan a depender de un estado compartido.

El estado crítico debe almacenarse en una base de datos, DataStore, archivos o el
backend.

### Application frente a un object de Kotlin

Un `object` de Kotlin se crea de forma diferida cuando se utiliza por primera vez.
`Application` es creada por el framework Android y dispone de un Context del
ámbito del proceso. Ambos solo viven dentro del proceso y desaparecen cuando este
finaliza.

**En resumen:** Android crea un singleton `Application` para cada proceso. Es
apropiado para la inicialización de toda la aplicación, pero no para almacenar el
estado de una sesión o de negocio, y no debe conservar referencias a la interfaz
de usuario.

</details>

<details>

<summary>172. ¿Cómo funciona BackStack?</summary>

#### Kotlin

`BackStack` es una pila que almacena el historial de navegación. Determina a qué
pantalla regresará el usuario al pulsar Atrás. Sigue el principio LIFO: la última
pantalla añadida es la primera en salir.

```text
Inicio -> Detalles -> Ajustes
```

Al pulsar Atrás:

```text
se elimina Ajustes  -> se muestra Detalles
se elimina Detalles -> se muestra Inicio
```

### Pila de Activities

Una nueva Activity se añade a la pila de la tarea:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

La acción Atrás finaliza la Activity actual y regresa a la anterior.

### Pila de Fragment

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Si no se invoca `addToBackStack`, la acción Atrás no restaurará el Fragment
anterior.

### Navigation Component

La pila está formada por destinos:

```kotlin
navController.navigate("details/$id")
navController.popBackStack()
```

`popBackStack()` elimina el destino actual.

### `popUpTo` e `inclusive`

`popUpTo` elimina una parte de la pila:

```kotlin
navController.navigate("home") {
    popUpTo("login") { inclusive = true }
}
```

Resulta útil después del inicio de sesión o la introducción inicial para impedir
que el usuario regrese a la pantalla de inicio de sesión.

```text
inclusive = false -> el destino indicado por popUpTo permanece
inclusive = true  -> el destino indicado por popUpTo también se elimina
```

### Navegación en Compose

El principio es el mismo:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen() }
    composable("details/{id}") { DetailsScreen() }
}
```

`NavController` administra la pila.

### `launchSingleTop` y navegación inferior

Para evitar destinos duplicados:

```kotlin
navController.navigate("home") {
    launchSingleTop = true
}
```

En una navegación inferior suele conservarse el estado de las pestañas:

```kotlin
navController.navigate(route) {
    popUpTo(navController.graph.startDestinationId) {
        saveState = true
    }
    launchSingleTop = true
    restoreState = true
}
```

Errores habituales:

- no limpiar la pila después de la autenticación o la introducción inicial;
- duplicar destinos;
- mezclar transacciones manuales de Fragment con NavController;
- confundir la pila de tareas de Activities con la pila de NavController;
- no tener en cuenta grafos anidados o interfaces modales.

Como regla práctica, BackStack debe reflejar el historial esperado por el
usuario. La interfaz modal se cierra primero, los flujos irreversibles limpian la
pila y las pestañas conservan su propio estado.

**En resumen:** BackStack es un historial LIFO de pantallas o destinos. Android
dispone de una pila de tareas de Activities, una pila de Fragment y una pila de
NavController. Se administra mediante `navigate`, `popBackStack`, `popUpTo`,
`inclusive`, `launchSingleTop`, `saveState` y `restoreState`.

</details>

<details>

<summary>173. ¿En qué situaciones puede producirse un ANR (Application Not Responding)?</summary>

#### Kotlin

Un ANR se produce cuando el proceso de una aplicación no responde a tiempo a un
evento importante del sistema. La causa más habitual es que el subproceso
principal esté bloqueado, sobrecargado o esperando a otro subproceso o proceso.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // bloquea la entrada y el renderizado
}
```

El subproceso principal procesa la entrada, el ciclo de vida, parte de los
callbacks de Binder y el renderizado. Un ANR puede deberse tanto a una única
operación prolongada como a una gran cola de operaciones breves.

### Causas habituales

- operaciones síncronas de red o disco, o consultas a la base de datos en el
  subproceso principal;
- análisis pesado, decodificación de bitmaps, ordenación o criptografía;
- `runBlocking`, `Thread.sleep()`, `Future.get()` o `join()`;
- callbacks pesados del ciclo de vida, de vistas o de Compose;
- tareas prolongadas en un `BroadcastReceiver` o `Service`;
- bloqueos, interbloqueos o llamadas síncronas a Binder;
- un arranque pesado en `Application`, `ContentProvider`, la inyección de
  dependencias o la primera Activity.

Una función `suspend` no traslada automáticamente el código bloqueante fuera del
subproceso principal.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

Para E/S se utiliza `Dispatchers.IO`; para tareas intensivas en CPU,
`Dispatchers.Default`.

Un `Service` no crea por sí mismo un subproceso de trabajo. Para tareas
prolongadas que deban garantizarse se utiliza `WorkManager`. Al usar
`BroadcastReceiver.goAsync()`, es obligatorio invocar `finish()`.

### Jank frente a ANR

Un frame lento produce jank, no necesariamente un ANR. Sin embargo, una serie
prolongada de operaciones costosas de layout, dibujo o recomposición puede
bloquear el subproceso principal durante suficiente tiempo.

### Diagnóstico

Debe comenzarse por la pila del subproceso principal y de los subprocesos que
este está esperando:

- Android vitals y trazas de ANR de Play Console;
- Perfetto o System Trace;
- CPU Profiler de Android Studio;
- Logcat y métricas de tiempo personalizadas;
- StrictMode en una compilación de depuración.

```kotlin
StrictMode.setThreadPolicy(
    StrictMode.ThreadPolicy.Builder()
        .detectDiskReads()
        .detectDiskWrites()
        .detectNetwork()
        .penaltyLog()
        .build()
)
```

StrictMode ayuda a detectar operaciones de E/S en el subproceso principal, pero
no sustituye las trazas.

### Prevención

- no bloquear el subproceso principal;
- no mantener un bloqueo durante operaciones de E/S;
- añadir tiempos de espera a la red y al IPC;
- aplazar la inicialización no crítica durante el arranque;
- dividir las tareas prolongadas de CPU;
- controlar la frecuencia de los eventos de interfaz;
- perfilar el arranque, el desplazamiento y las actualizaciones masivas.

**En resumen:** un ANR puede deberse a E/S bloqueante, tareas intensivas en CPU,
bloqueos, esperas o IPC síncronos, callbacks prolongados o un arranque pesado en
el subproceso principal. El diagnóstico comienza por la pila del subproceso
principal y una traza del sistema.

</details>

<details>

<summary>174. ¿Qué puede ejecutarse en el subproceso principal?</summary>

#### Kotlin

El `Main thread` de Android se encarga de la interfaz de usuario: eventos de
entrada, callbacks del ciclo de vida, renderizado y actualizaciones de vistas o
Compose. Solo deben ejecutarse en él operaciones rápidas que no bloqueen el
renderizado ni puedan provocar un ANR.

Se puede:

- actualizar la interfaz;
- trabajar con la jerarquía de vistas;
- renderizar una interfaz de Compose;
- ejecutar callbacks del ciclo de vida;
- procesar listeners de clics;
- realizar llamadas de navegación;
- mostrar diálogos, snackbars o toasts;
- ejecutar lógica breve y ligera.

```kotlin
button.setOnClickListener {
    viewModel.onLoginClick()
}
```

El sistema de vistas no es seguro para subprocesos, por lo que sus
actualizaciones deben realizarse en el subproceso principal:

```kotlin
textView.text = "Loaded"
progressBar.isVisible = false
```

En Compose, un composable debe ser ligero:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
ProfileScreen(state)
```

No debe bloquearse el subproceso principal con:

- solicitudes de red;
- E/S de archivos;
- consultas grandes a bases de datos;
- decodificación de bitmaps;
- análisis de grandes documentos JSON;
- ordenación o filtrado pesado;
- `Thread.sleep`;
- `runBlocking`;
- bloques sincronizados prolongados.

Para alcanzar 60 fps, el subproceso principal dispone aproximadamente de 16 ms
por frame. Si permanece ocupado con una tarea prolongada, se producirá jank; si
se bloquea durante demasiado tiempo, se producirá un ANR.

Trabajo en segundo plano:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

Regla:

```text
interfaz/orquestación -> Main
E/S bloqueante        -> Dispatchers.IO
tarea intensiva en CPU -> Dispatchers.Default
```

Una validación breve en el subproceso principal es aceptable:

```kotlin
if (email.isBlank()) {
    showEmailError()
    return
}
```

Para detectar operaciones de E/S accidentales en el subproceso principal, puede
activarse StrictMode en una compilación de depuración:

```kotlin
StrictMode.setThreadPolicy(
    StrictMode.ThreadPolicy.Builder()
        .detectDiskReads()
        .detectDiskWrites()
        .detectNetwork()
        .penaltyLog()
        .build()
)
```

**En resumen:** el subproceso principal está destinado a la interfaz, el ciclo de
vida, la navegación y una breve lógica de orquestación. Toda tarea de duración
impredecible, como red, disco, base de datos, análisis, bitmaps o cálculos
intensivos, debe trasladarse a `IO` o `Default`.

</details>

<details>

<summary>175. ¿Cuáles son los componentes básicos de Android?</summary>

#### Kotlin

Los componentes básicos del sistema Android son `Activity`, `Service`,
`BroadcastReceiver` y `ContentProvider`. Fragment, ViewModel y WorkManager son
abstracciones importantes de Jetpack, pero no son componentes del sistema.

### Activity

Es un punto de entrada a la interfaz con su propia ventana y ciclo de vida:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Una Activity recibe un `Intent` y puede alojar una interfaz de Compose o
Fragment.

### Service

Es un componente sin interfaz propia:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

De forma predeterminada, los callbacks de un Service se ejecutan en el subproceso
principal. Para una tarea continua visible para el usuario se utiliza un
foreground service. Para tareas persistentes que puedan aplazarse suele ser
preferible WorkManager.

### BroadcastReceiver

Recibe broadcasts del sistema o de la aplicación:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // procesar el evento
    }
}
```

`onReceive()` debe finalizar rápidamente. Las tareas prolongadas se delegan a
WorkManager o a otro componente adecuado.

### ContentProvider

Proporciona datos estructurados mediante URI:

```text
content://com.example.provider/users/1
```

Se utiliza para compartir datos entre procesos de forma controlada. Los
proveedores de contactos y contenido multimedia son ejemplos del sistema.

### Manifest e Intent

Los componentes del sistema se declaran en `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

El manifest también define permisos, filtros de intents y el comportamiento de
`android:exported`.

Un `Intent` inicia una Activity o un Service, o entrega un broadcast:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

### Abstracciones de Jetpack

- Fragment: parte de una interfaz basada en vistas dentro de una Activity;
- ViewModel: propietario del estado de la interfaz que sobrevive a los cambios de
  configuración;
- WorkManager: planificador de tareas persistentes que pueden aplazarse.

Elección práctica:

- punto de entrada a la interfaz: `Activity`;
- tarea continua visible para el usuario: `Service` en primer plano;
- evento del sistema o de la aplicación: `BroadcastReceiver`;
- API de datos entre procesos: `ContentProvider`;
- tarea persistente que puede aplazarse: `WorkManager`.

**En resumen:** los componentes básicos del sistema Android son `Activity`,
`Service`, `BroadcastReceiver` y `ContentProvider`. Se declaran en el manifest y
se comunican mediante Intent, Binder o URI. Fragment, ViewModel y WorkManager
pertenecen al nivel de Jetpack.

</details>

<details>

<summary>176. ¿Qué es un Service?</summary>

#### Kotlin

Un `Service` es un componente de Android sin interfaz de usuario. Es importante
recordar que un Service no crea por sí mismo un subproceso en segundo plano. De
forma predeterminada, sus callbacks se ejecutan en el subproceso principal del
proceso de la aplicación.

Un Service no garantiza sobrevivir a la finalización del proceso, a una detención
forzada ni a un reinicio del dispositivo.

### Started Service

```kotlin
class SyncService : Service() {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        scope.launch {
            try {
                syncData()
            } finally {
                stopSelf(startId)
            }
        }
        return START_NOT_STICKY
    }

    override fun onDestroy() {
        scope.cancel()
        super.onDestroy()
    }

    override fun onBind(intent: Intent?): IBinder? = null
}
```

Cada llamada a `startService()` invoca `onStartCommand()`. Las tareas bloqueantes
deben trasladarse fuera del subproceso principal. `onDestroy()` permite realizar
una limpieza de mejor esfuerzo, pero puede no invocarse si el proceso finaliza.

### Bound Service

Un servicio vinculado proporciona una API mediante `IBinder`:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService = this@PlayerService
    }

    override fun onBind(intent: Intent): IBinder = binder
}
```

Local Binder funciona dentro de un mismo proceso. Para IPC entre procesos se
utiliza Messenger o AIDL.

Un servicio exclusivamente vinculado vive mientras existan clientes. Si el
servicio también se ha iniciado, `unbindService()` no detiene su ciclo de vida
como servicio iniciado.

### Foreground Service

Se necesita para tareas prolongadas y visibles para el usuario, como reproducción
multimedia, navegación, ubicación activa o transferencia de archivos.

```kotlin
startForeground(
    NOTIFICATION_ID,
    createNotification()
)
```

Después de `startForegroundService()` debe invocarse rápidamente
`startForeground()`. Las versiones modernas de Android limitan el inicio en
segundo plano y exigen una notificación, un tipo de servicio y permisos.

### Ciclo de vida

```text
onCreate()       -> creación de la instancia
onStartCommand() -> cada inicio
onBind()         -> conexión de un cliente
onDestroy()      -> limpieza
```

Indicadores de reinicio:

- `START_NOT_STICKY`: no reiniciar;
- `START_STICKY`: reiniciar sin garantizar el Intent;
- `START_REDELIVER_INTENT`: volver a entregar el Intent.

La política de reinicio no convierte un Service en una cola duradera; la
operación debe ser idempotente.

Elección práctica:

```text
tarea activa visible para el usuario -> Foreground Service
tarea aplazada fiable              -> WorkManager
activación a una hora exacta        -> AlarmManager
reproducción multimedia             -> Media3 + FGS
tarea vinculada a una pantalla      -> ámbito del ViewModel
```

**En resumen:** un Service es un componente con ciclo de vida y sin interfaz, no
un subproceso independiente. Un started service ejecuta comandos, un bound
service proporciona una API Binder y un foreground service se utiliza para
tareas continuas visibles para el usuario. Para tareas aplazadas y fiables debe
utilizarse WorkManager.

</details>

<details>

<summary>177. ¿Qué es un BroadcastReceiver?</summary>

#### Kotlin

Un `BroadcastReceiver` es un componente de Android que recibe `Intent` de
broadcast procedentes del sistema, de componentes de la aplicación o de otras
aplicaciones.

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            scheduleSync(context)
        }
    }
}
```

Un receiver no conserva estado ni está destinado a tareas prolongadas. Después
de finalizar `onReceive()`, el proceso puede detenerse.

### Receiver declarado en el manifest

```xml
<receiver
    android:name=".BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

Para `BOOT_COMPLETED` se necesita el permiso `RECEIVE_BOOT_COMPLETED`.

Un receiver declarado en el manifest puede funcionar sin interfaz, pero las
versiones modernas de Android restringen numerosos broadcasts implícitos. Deben
comprobarse la acción y la versión de API concretas.

`android:exported` es fundamental para la seguridad. Si el receiver no debe
aceptar intents de otras aplicaciones, debe utilizarse `exported=false`. Si está
exportado, deben validarse los datos del intent y los permisos.

### Receiver dinámico

```kotlin
override fun onStart() {
    super.onStart()

    ContextCompat.registerReceiver(
        this,
        receiver,
        IntentFilter(Intent.ACTION_BATTERY_LOW),
        ContextCompat.RECEIVER_NOT_EXPORTED
    )
}

override fun onStop() {
    unregisterReceiver(receiver)
    super.onStop()
}
```

El registro y la anulación del registro deben ser simétricos. El par de métodos
del ciclo de vida depende de la necesidad: para una pantalla visible,
`onStart/onStop`; para toda la vida de una Activity, `onCreate/onDestroy`.

En las API modernas, un receiver dinámico suele requerir que se indique
explícitamente `RECEIVER_EXPORTED` o `RECEIVER_NOT_EXPORTED`.

### Tareas prolongadas

`onReceive()` suele ejecutarse en el subproceso principal y debe finalizar
rápidamente. Para tareas persistentes que puedan aplazarse se utiliza
WorkManager:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    val request = OneTimeWorkRequestBuilder<SyncWorker>().build()

    WorkManager.getInstance(context).enqueueUniqueWork(
        "sync",
        ExistingWorkPolicy.KEEP,
        request
    )
}
```

`goAsync()` permite continuar brevemente una tarea después de `onReceive()`, pero
exige invocar `PendingResult.finish()` y no sustituye WorkManager para tareas
prolongadas.

### Comunicación interna

`LocalBroadcastManager` está obsoleto. Dentro de un proceso es preferible utilizar
Flow o StateFlow, callbacks, el estado de un repositorio o un resultado de
navegación.

BroadcastReceiver resulta apropiado para eventos del sistema, alarmas, cambios de
arranque o paquetes y una comunicación claramente definida entre aplicaciones o
componentes.

**En resumen:** BroadcastReceiver recibe intents de broadcast y debe finalizar
rápidamente `onReceive()`. Un receiver declarado en el manifest requiere atención
a `exported` y los permisos; un receiver dinámico debe vincularse al ciclo de
vida, y las tareas prolongadas deben delegarse a WorkManager.

</details>

<details>

<summary>178. ¿Qué es Context y qué tipos existen?</summary>

#### Kotlin

`Context` es el punto de acceso al entorno de Android: recursos, servicios del
sistema, información del paquete, archivos, preferencias e inicio de
componentes. Un contexto concreto también determina el tema y el límite del ciclo
de vida.

```kotlin
context.getString(R.string.app_name)
context.getSystemService(Context.CONNECTIVITY_SERVICE)
context.startActivity(intent)
context.getSharedPreferences("settings", Context.MODE_PRIVATE)
```

### Contexto de Application

```kotlin
val appContext = context.applicationContext
```

Vive mientras exista el proceso y no tiene el tema de una Activity concreta.
Resulta apropiado para dependencias de larga duración, como Room, DataStore,
repositorios y SDK que no necesitan un contexto de interfaz.

### Contexto de Activity

Dispone del tema, la ventana y el ciclo de vida de una Activity concreta. Se
necesita para diálogos, vistas con tema y operaciones de interfaz. No debe
almacenarse en un singleton ni en un objeto de larga duración.

### Contexto de Fragment

Un `Fragment` no es un `Context`. Obtiene el contexto de su host:

```kotlin
val context = requireContext()
val activityContext = requireActivity()
```

Antes de adjuntarse, el contexto puede ser `null`; después de separarse,
`requireContext()` lanza una excepción.

### Contexto de Service

Un `Service` es un `Context` vinculado al ciclo de vida del servicio. Resulta
apropiado para operaciones del servicio, pero no tiene la semántica de ventana o
tema de una Activity.

### ContextWrapper

`ContextWrapper` delega en un contexto base y puede modificar su comportamiento:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

`ContextThemeWrapper` resulta útil para inflar interfaces con un tema.

### Compose

```kotlin
val context = LocalContext.current
```

Es el contexto del entorno actual de la composición. Puede utilizarse para
recursos y acciones de interfaz, pero no debe almacenarse sin un propietario de
ciclo de vida.

### Fugas de memoria

Incorrecto:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

El singleton conservará la Activity después de que vuelva a crearse.

Para una dependencia de larga duración es preferible:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

Debe proporcionarse `context.applicationContext`.

Elección práctica:

```text
Contexto de Application -> BD, DataStore y dependencias de larga duración
Contexto de Activity    -> interfaz, diálogos y vistas con tema
Contexto de Fragment    -> contexto del host mientras está adjunto
Contexto de Service     -> operaciones del servicio
LocalContext            -> recursos y acciones de interfaz en Compose
```

**En resumen:** `Context` proporciona acceso al entorno de Android. Las
dependencias de larga duración deben utilizar el contexto de Application; la
interfaz, el contexto de Activity o uno con tema. Un Fragment no es un Context y
el contexto de una Activity no debe conservarse en un singleton.

</details>

<details>

<summary>179. ¿Qué es un Bundle?</summary>

#### Kotlin

Un `Bundle` es un contenedor clave-valor de Android que permite transferir
pequeñas cantidades de datos entre componentes o guardar un estado sencillo de
la interfaz.

```kotlin
val bundle = Bundle().apply {
    putString("user_id", userId)
    putInt("tab", 2)
}

val userId = bundle.getString("user_id")
```

Se utiliza con `Intent`, argumentos de Fragment, `onSaveInstanceState`,
Navigation y Activity Result API.

### Extras de Intent

```kotlin
val intent = Intent(this, DetailsActivity::class.java).apply {
    putExtra("user_id", userId)
}
startActivity(intent)
```

```kotlin
val userId = intent.getStringExtra("user_id")
```

### Argumentos de Fragment

```kotlin
val fragment = DetailsFragment().apply {
    arguments = bundleOf("user_id" to userId)
}
```

```kotlin
val userId = requireArguments().getString("user_id")
```

### Estado de instancia guardado

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

Resulta apropiado para guardar una pequeña cantidad de estado de interfaz después
de un cambio de configuración o la recreación del proceso.

### Tipos

Bundle admite:

- tipos primitivos;
- `String`;
- arrays;
- `Parcelable`;
- `Serializable`;
- `Bundle`;
- `ArrayList` de determinados tipos.

Para datos personalizados en Android es preferible utilizar `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Sin embargo, para la navegación suele ser preferible transferir únicamente un
identificador y cargar los datos desde un repositorio.

### Limitaciones

Bundle no está destinado a grandes cantidades de datos. Puede producirse
`TransactionTooLargeException`.

Incorrecto:

```kotlin
bundle.putParcelableArrayList("huge_list", hugeList)
```

Es preferible transferir un identificador o una clave de la base de datos o la
caché.

### Bundle frente a otros contenedores de estado

```text
Bundle             -> pequeño estado de navegación o interfaz
SavedStateHandle   -> estado de un ViewModel después de la recreación
ViewModel          -> estado de pantalla o negocio durante el ciclo de vida
Database/DataStore -> datos persistentes
```

Las claves de Bundle son cadenas, por lo que es fácil cometer errores. Para
Navigation es preferible utilizar Safe Args o argumentos de ruta tipados cuando
estén disponibles.

**En resumen:** Bundle es un contenedor clave-valor para datos pequeños, como
identificadores, indicadores y un estado sencillo de interfaz o navegación. No
deben almacenarse listas grandes, bitmaps ni grandes cantidades de datos; es
preferible transferir un identificador y recuperar la información desde un
repositorio o una caché.

</details>

<details>

<summary>180. ¿Se puede anidar un Bundle dentro de otro Bundle?</summary>

#### Kotlin

Sí, un `Bundle` puede incluirse dentro de otro mediante `putBundle()` y leerse
mediante `getBundle()`.

```kotlin
val addressBundle = bundleOf(
    "city" to "Kyiv",
    "street" to "Main"
)

val userBundle = bundleOf(
    "user_id" to "123",
    "address" to addressBundle
)
```

Lectura:

```kotlin
val address = userBundle.getBundle("address")
val city = address?.getString("city")
```

Esto resulta útil para pequeñas cantidades de datos agrupados de forma lógica:

```text
usuario
 ├─ id
 ├─ nombre
 └─ dirección
     ├─ ciudad
     └─ calle
```

Ejemplo en los argumentos de un Fragment:

```kotlin
val fragment = ProfileFragment().apply {
    arguments = bundleOf(
        "user" to bundleOf(
            "id" to "123",
            "name" to "Alex"
        )
    )
}
```

Para guardar el estado:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putBundle("filters", bundleOf("query" to query))
    super.onSaveInstanceState(outState)
}
```

Limitaciones:

- Bundle no está destinado a grandes cantidades de datos;
- un anidamiento profundo reduce la legibilidad;
- es fácil cometer errores tipográficos en las claves de cadena;
- puede producirse `TransactionTooLargeException`.

Incorrecto:

```kotlin
bundle.putBundle("huge_data", hugeNestedBundle)
```

Si la estructura representa un modelo claramente definido, es preferible
utilizar `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Para grandes cantidades de datos es preferible transferir un identificador y
cargar la información desde un repositorio, una caché o una base de datos.

**En resumen:** es posible anidar un `Bundle` dentro de otro, pero únicamente para
argumentos pequeños y sencillos agrupados lógicamente o para guardar estado. Para
modelos complejos debe utilizarse `Parcelable`; para grandes cantidades de datos,
un identificador y un repositorio.

</details>

<details>

<summary>181. ¿Qué son los permisos en Android?</summary>

#### Kotlin

Los permisos son un mecanismo de Android que limita el acceso de una aplicación
a recursos situados fuera de su sandbox, como la cámara, el micrófono, la
ubicación, los contactos, las notificaciones y el almacenamiento o contenido
multimedia.

Tipos principales:

```text
normal     -> se conceden automáticamente
dangerous  -> declaración en el manifest + solicitud durante la ejecución
special    -> flujo independiente en Ajustes
signature  -> solo aplicaciones con el certificado correspondiente
```

Un permiso peligroso debe declararse en el manifest y solicitarse durante la
ejecución:

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

```kotlin
val launcher = rememberLauncherForActivityResult(
    ActivityResultContracts.RequestPermission()
) { granted ->
    if (granted) openCamera() else showPermissionDenied()
}
```

Antes de utilizar una API protegida siempre debe comprobarse la concesión actual:

```kotlin
val granted = ContextCompat.checkSelfPermission(
    context,
    Manifest.permission.CAMERA
) == PackageManager.PERMISSION_GRANTED
```

La concesión no debe almacenarse como un valor booleano permanente: el usuario
puede revocar el permiso en Ajustes, el sistema puede restablecerlo
automáticamente y los permisos de cámara, micrófono o ubicación pueden concederse
una sola vez.

`shouldShowRequestPermissionRationale()` ayuda a decidir si debe mostrarse una
explicación antes de volver a solicitar el permiso. Un valor `false` no siempre
significa que se haya denegado permanentemente, por lo que la aplicación debe
tener en cuenta su propio historial de solicitudes.

Reglas prácticas:

- solicitar el permiso cuando el usuario inicia la acción, no al abrir la
  aplicación;
- pedir el acceso mínimo necesario;
- admitir la denegación, la revocación y los permisos de una sola vez;
- para archivos y contenido multimedia, utilizar Photo Picker, SAF, MediaStore o
  el almacenamiento específico de la aplicación;
- para permisos especiales, dirigir al usuario a Ajustes y volver a comprobar la
  capacidad cuando regrese.

**En resumen:** los permisos protegen recursos sensibles. Los permisos peligrosos
se declaran en el manifest y se solicitan durante la ejecución mediante Activity
Result API. La concesión puede cambiar en cualquier momento, por lo que debe
comprobarse antes de utilizar el recurso.

</details>

<details>

<summary>182. ¿Qué es setContentView?</summary>

#### Kotlin

`setContentView()` es una API de Activity que establece la interfaz raíz de una
pantalla. En el sistema de vistas, infla un layout XML o recibe una `View` ya
creada y la añade al área de contenido de la Activity.

Ejemplo básico:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

De forma simplificada, internamente ocurre lo siguiente:

```text
XML -> inflado -> jerarquía de vistas -> raíz de contenido de la Activity
```

Con ViewBinding suele escribirse así:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

Después de `setContentView()` pueden buscarse vistas mediante `findViewById`,
puesto que el layout ya está adjunto:

```kotlin
val title = findViewById<TextView>(R.id.title)
```

En Compose se utiliza `setContent { ... }` en lugar de `setContentView()`:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { MainScreen() }
    }
}
```

En un Fragment no se invoca `setContentView()`: el layout se crea mediante
`Fragment(R.layout...)`, `onCreateView()` o ComposeView.

Errores habituales:

- invocar `findViewById` antes de `setContentView`;
- invocar `setContentView` varias veces sin necesidad;
- conservar referencias a vistas de una Activity en un singleton;
- mezclar XML y Compose sin un límite claro de responsabilidades.

**En resumen:** `setContentView()` establece la interfaz raíz de una Activity.
Para XML es preferible utilizar ViewBinding y `setContentView(binding.root)`; para
Compose, `setContent { ... }`; y para un Fragment, su propio ciclo de creación de
la vista.

</details>

<details>

<summary>183. ¿Qué métodos existen en View?</summary>

#### Kotlin

`View` es la clase base de la interfaz de usuario de Android. Su flujo principal
es `measure -> layout -> draw`.

Callbacks principales:

```kotlin
override fun onMeasure(widthSpec: Int, heightSpec: Int) {
    val width = resolveSize(200, widthSpec)
    val height = resolveSize(100, heightSpec)
    setMeasuredDimension(width, height)
}

override fun onDraw(canvas: Canvas) {
    canvas.drawCircle(width / 2f, height / 2f, radius, paint)
}
```

`onMeasure()` determina el tamaño de la View según un `MeasureSpec`: `EXACTLY`,
`AT_MOST` o `UNSPECIFIED`. `onLayout()` posiciona los elementos hijos y suele ser
especialmente importante para un `ViewGroup`. `onDraw()` dibuja el contenido en
un `Canvas`.

Para actualizar la interfaz:

```text
solo cambió la apariencia -> invalidate()
cambió el tamaño/layout   -> requestLayout()
```

La entrada se procesa mediante `onTouchEvent()`:

```kotlin
override fun onTouchEvent(event: MotionEvent): Boolean =
    when (event.action) {
        MotionEvent.ACTION_DOWN -> true
        MotionEvent.ACTION_UP -> {
            performClick()
            true
        }
        else -> super.onTouchEvent(event)
    }

override fun performClick(): Boolean {
    super.performClick()
    return true
}
```

`performClick()` es importante para los listeners de clics y la accesibilidad.

Ciclo de vida de una View:

- `onAttachedToWindow()`: la View se adjunta a la ventana;
- `onDetachedFromWindow()`: deben detenerse animaciones, listeners y callbacks;
- `onSaveInstanceState()` y `onRestoreInstanceState()`: guardan y restauran su
  propio estado.

Métodos y estados útiles:

- `VISIBLE`, `INVISIBLE` y `GONE`;
- `requestFocus()` y `clearFocus()`;
- `post { }`: ejecuta código en la cola de la interfaz;
- `setOnClickListener()` y `setOnLongClickListener()`.

Dentro de `onDraw()` no deben realizarse operaciones de E/S, decodificación de
bitmaps ni asignaciones innecesarias, ya que el método puede invocarse con mucha
frecuencia.

**En resumen:** los principales métodos de `View` son `onMeasure()`, `onLayout()`,
`onDraw()`, `invalidate()`, `requestLayout()`, `onTouchEvent()`,
`performClick()` y los callbacks del ciclo de vida. Se encargan del tamaño, la
posición, el dibujo, la entrada y los recursos.

</details>

<details>

<summary>184. ¿Qué es un ViewGroup?</summary>

#### Kotlin

Un `ViewGroup` es una `View` que contiene vistas hijas y determina cómo medirlas,
posicionarlas, dibujarlas y dirigir hacia ellas los eventos táctiles.

```text
View      -> un nodo de interfaz
ViewGroup -> contenedor + hijos + política de layout
```

Algunos ejemplos son `LinearLayout`, `FrameLayout`, `ConstraintLayout` y
`RecyclerView`.

Flujo principal:

```text
onMeasure() -> onLayout() -> dispatchDraw()
```

`onMeasure()` mide los elementos hijos y determina el tamaño del propio
contenedor:

```kotlin
override fun onMeasure(widthSpec: Int, heightSpec: Int) {
    var totalHeight = paddingTop + paddingBottom
    var maxWidth = 0

    for (index in 0 until childCount) {
        val child = getChildAt(index)
        if (child.visibility == GONE) continue

        measureChild(child, widthSpec, heightSpec)
        totalHeight += child.measuredHeight
        maxWidth = maxOf(maxWidth, child.measuredWidth)
    }

    setMeasuredDimension(
        resolveSize(maxWidth + paddingLeft + paddingRight, widthSpec),
        resolveSize(totalHeight, heightSpec)
    )
}
```

`onLayout()` establece los límites definitivos de los elementos hijos:

```kotlin
override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
    var top = paddingTop

    for (index in 0 until childCount) {
        val child = getChildAt(index)
        if (child.visibility == GONE) continue

        child.layout(
            paddingLeft,
            top,
            paddingLeft + child.measuredWidth,
            top + child.measuredHeight
        )

        top += child.measuredHeight
    }
}
```

`LayoutParams` describe las reglas aplicables a un elemento hijo: `weight` en
`LinearLayout`, restricciones en `ConstraintLayout` o `gravity` en
`FrameLayout`. Un `ViewGroup` personalizado debe admitir correctamente los
parámetros, márgenes, `GONE`, padding y `MeasureSpec`.

Flujo de eventos táctiles:

```text
dispatchTouchEvent()
 -> onInterceptTouchEvent()
 -> child.dispatchTouchEvent()
 -> onTouchEvent()
```

`onInterceptTouchEvent()` permite que el contenedor intercepte un gesto destinado
a un elemento hijo, por ejemplo, para realizar un desplazamiento. Una
interceptación incorrecta rompe los clics y el desplazamiento anidado.

Para actualizar:

```text
invalidate()    -> volver a dibujar
requestLayout() -> repetir la medición y el layout
```

Solo conviene crear un `ViewGroup` personalizado cuando los contenedores
existentes o Compose no satisfacen un requisito de layout, eventos táctiles o
rendimiento.

**En resumen:** un `ViewGroup` es un contenedor de vistas que administra la
medición, el posicionamiento, el dibujo y el enrutamiento de eventos táctiles de
sus hijos. Sus métodos principales son `onMeasure()`, `onLayout()`,
`dispatchDraw()` y `onInterceptTouchEvent()`.

</details>

<details>

<summary>185. ¿Para qué sirve el método onStart() de una Activity?</summary>

#### Kotlin

`onStart()` se invoca cuando una `Activity` se vuelve visible para el usuario. La
interfaz ya se ha creado, pero la Activity todavía no está necesariamente en
primer plano ni recibe necesariamente eventos de entrada.

Ciclo de vida:

```text
primer inicio: onCreate -> onStart -> onResume
segundo plano: onPause -> onStop
regreso:       onRestart -> onStart -> onResume
```

Diferencia respecto a `onResume()`:

```text
onStart()  -> la Activity es visible
onResume() -> la Activity es interactiva y está en primer plano
```

En `onStart()` resulta apropiado iniciar recursos necesarios únicamente mientras
la pantalla es visible:

- registro de listeners o receivers ligeros;
- analíticas de visibilidad;
- suscripciones al estado de la interfaz;
- recursos necesarios solo mientras la pantalla permanece visible.

Todo lo que se inicie en `onStart()` debe detenerse de forma simétrica en
`onStop()`:

```kotlin
override fun onStart() {
    super.onStart()
    listener.register()
}

override fun onStop() {
    listener.unregister()
    super.onStop()
}
```

Para un Flow es preferible utilizar una API consciente del ciclo de vida:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`repeatOnLifecycle(STARTED)` inicia la recopilación cuando la Activity es visible
y la cancela al pasar a un estado inferior a `STARTED`.

Qué no debe hacerse en `onStart()`:

- E/S bloqueante pesada;
- análisis prolongado o `Thread.sleep()`;
- inicialización que solo debe ejecutarse una vez y pertenece a `onCreate()`;
- tareas en segundo plano que deban sobrevivir a la Activity.

Si se inicia una actualización en `onStart()`, el ViewModel o el repositorio
deben controlar la vigencia de los datos y evitar duplicados, ya que `onStart()`
se invoca cada vez que se regresa a la pantalla.

**En resumen:** `onStart()` indica que la Activity se ha vuelto visible. En este
método se inician recursos ligeros y suscripciones necesarios únicamente durante
la visibilidad, y se liberan en `onStop()`. Para Flow debe utilizarse
`repeatOnLifecycle(STARTED)`.

</details>

<details>

<summary>186. ¿Cómo puede ejecutarse onStart() sin que se ejecute onResume()?</summary>

#### Kotlin

Esto puede ocurrir si una `Activity` se vuelve visible, pero no obtiene el foco en
primer plano, o si el ciclo de vida se interrumpe entre los estados `STARTED` y
`RESUMED`.

Diferencia entre los estados:

```text
onStart()  -> la Activity es visible
onResume() -> la Activity está en primer plano y es interactiva
```

Situaciones habituales:

1. **Se abre otra Activity o un overlay por encima**

Una Activity transparente o con tema de diálogo puede dejar visible la pantalla
anterior, pero quitarle el foco.

```text
MainActivity.onStart()
OverlayActivity se abre por encima
MainActivity no llega a onResume()
```

2. **Redirección desde `onStart()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

El foco pasa a otra Activity, por lo que la actual puede no llegar a
`onResume()`.

3. **`finish()` antes de `onResume()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

4. **Interrupción del sistema**

Entre `onStart()` y `onResume()`, el usuario o el sistema pueden abrir otra
pantalla o aplicación, por ejemplo, debido a una llamada entrante, la interfaz
del sistema, el modo multiventana o una transición rápida a segundo plano.

Conclusión práctica: el código del ciclo de vida no debe asumir que después de
cada `onStart()` se ejecutará necesariamente `onResume()`.

Los recursos deben vincularse al par de callbacks apropiado:

```text
solo mientras es visible    -> onStart() / onStop()
solo mientras es interactiva -> onResume() / onPause()
```

Para Flow:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) { /* interfaz visible */ }
repeatOnLifecycle(Lifecycle.State.RESUMED) { /* interacción activa */ }
```

**En resumen:** puede ejecutarse `onStart()` sin `onResume()` cuando una Activity
es visible, pero no llega a ser interactiva debido a un overlay, una redirección,
`finish()` o una interrupción del sistema. La limpieza debe ser simétrica al
estado del ciclo de vida en el que se inicia cada recurso.

</details>

<details>

<summary>187. ¿Qué son las migraciones en Room?</summary>

#### Kotlin

Una migración de Room describe la transición del esquema de una base de datos de
una versión a otra sin perder los datos del usuario.

Se necesita una migración cuando cambia el esquema:

- se añade o elimina una tabla o columna;
- se modifica un tipo, una restricción o una relación;
- se cambia el nombre de una tabla o un campo;
- se añade un índice o una clave foránea.

Después de modificar el esquema de una entidad debe incrementarse `version` en
`@Database`:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase()
```

Migración manual:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Configuración:

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

Si se añade una columna `NOT NULL` a filas existentes, debe proporcionarse un
valor predeterminado:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

Para cambios complejos que SQLite no permite realizar directamente se utiliza una
migración mediante copia de tabla:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

`AutoMigration` resulta apropiado para cambios sencillos e inequívocos:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

Para cambiar nombres o eliminar elementos se necesita `AutoMigrationSpec`;
para transformaciones, división o unión de tablas y relleno de datos, una
migración manual.

`fallbackToDestructiveMigration()` elimina la base de datos antigua y crea una
nueva. Puede ser aceptable para una base de datos de caché o desarrollo, pero no
para datos del usuario.

Reglas prácticas:

- incrementar `version` con cada cambio del esquema;
- mantener una ruta de migración desde todas las versiones publicadas;
- exportar el esquema mediante `exportSchema = true`;
- probar las migraciones con `MigrationTestHelper`;
- no utilizar una migración destructiva para datos importantes.

**En resumen:** una migración de Room actualiza el esquema de la base de datos
conservando los datos. `AutoMigration` puede encargarse de cambios sencillos; los
cambios complejos requieren una `Migration` manual, y todas las rutas utilizadas
en producción deben probarse.

</details>

<details>

<summary>188. ¿Qué hace la anotación Embedded en Room?</summary>

#### Kotlin

`@Embedded` de Room expande los campos de una clase anidada como columnas de la
misma tabla o del mismo resultado de consulta. El objeto incrustado no se
convierte en una tabla independiente.

Ejemplo:

```kotlin
data class Address(
    val city: String,
    val street: String
)

@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    @Embedded val address: Address
)
```

Room creará las siguientes columnas:

```text
id, name, city, street
```

`@Embedded` resulta apropiado para objetos de valor que forman parte lógica de
una entidad:

- dirección;
- coordenadas;
- importe monetario y divisa;
- metadatos;
- grupo reutilizable de columnas.

Si existen conflictos entre nombres de columnas, se utiliza `prefix`:

```kotlin
data class Coordinates(
    val lat: Double,
    val lng: Double
)

@Entity
data class RouteEntity(
    @PrimaryKey val id: String,
    @Embedded(prefix = "start_") val start: Coordinates,
    @Embedded(prefix = "end_") val end: Coordinates
)
```

Columnas:

```text
start_lat, start_lng, end_lat, end_lng
```

`@Embedded` también resulta práctico en DTO de lectura para resultados de un
`JOIN`:

```kotlin
data class UserWithStats(
    @Embedded val user: UserEntity,
    @Embedded(prefix = "stats_") val stats: UserStatsEntity
)
```

Diferencia respecto a `@Relation`:

```text
@Embedded -> los campos se encuentran en la misma fila o resultado
@Relation -> representa una relación entre tablas distintas
```

Si el objeto tiene su propio ciclo de vida, puede ser utilizado por muchas filas
o necesita una clave foránea, no debe modelarse con `@Embedded`, sino mediante
una tabla independiente y una relación.

Limitaciones:

- no crea una clave foránea;
- no normaliza los datos;
- no crea una relación automáticamente;
- puede aumentar excesivamente la cantidad de columnas de la tabla;
- necesita `prefix` cuando coinciden los nombres de columnas.

**En resumen:** `@Embedded` incorpora los campos de un objeto de valor a la tabla
o al resultado de consulta actual. Para relaciones entre tablas se utilizan
`@Relation`, claves foráneas y entidades independientes.

</details>

<details>

<summary>189. ¿Qué es DatabaseView en Room?</summary>

#### Kotlin

`@DatabaseView` de Room describe una `VIEW` de SQLite como una clase de Kotlin.
Una vista no almacena datos por separado, sino que representa el resultado de una
consulta SQL como un modelo de solo lectura.

Una vista SQL es una consulta con nombre:

```sql
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE active = 1;
```

En Room:

```kotlin
@DatabaseView(
    """
    SELECT id, name, age
    FROM users
    WHERE active = 1
    """
)
data class ActiveUserView(
    val id: String,
    val name: String,
    val age: Int
)
```

La vista debe añadirse explícitamente a `@Database`:

```kotlin
@Database(
    entities = [UserEntity::class],
    views = [ActiveUserView::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase()
```

Lectura mediante un DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM ActiveUserView")
    fun observeActiveUsers(): Flow<List<ActiveUserView>>
}
```

`DatabaseView` resulta útil para:

- consultas de lectura complejas;
- proyecciones destinadas a la interfaz;
- resultados de `JOIN`;
- agregaciones;
- reutilizar una misma consulta SQL en varios métodos DAO.

Ejemplo con agregación:

```kotlin
@DatabaseView(
    """
    SELECT users.id, users.name, COUNT(posts.id) AS postCount
    FROM users
    LEFT JOIN posts ON posts.userId = users.id
    GROUP BY users.id
    """
)
data class UserStatsView(
    val id: String,
    val name: String,
    val postCount: Int
)
```

Diferencias:

```text
Entity       -> tabla real que almacena datos
DatabaseView -> proyección de solo lectura y resultado de una consulta SQL
@Relation    -> grafo de objetos formado por tablas relacionadas
```

Limitaciones:

- no permite ejecutar directamente `insert`, `update` ni `delete`;
- el SQL debe mantenerse cuando cambia el esquema;
- las vistas complejas pueden resultar costosas para el rendimiento;
- los cambios en las tablas subyacentes pueden requerir una migración.

**En resumen:** `@DatabaseView` es un modelo de solo lectura basado en una vista
SQL. Resulta práctico para modelos de lectura complejos, joins y agregaciones,
pero no sustituye las tablas ni admite operaciones directas de escritura.

</details>

<details>

<summary>190. ¿Cuál es la diferencia entre SQL y NoSQL?</summary>

#### Kotlin

`SQL` y `NoSQL` son enfoques diferentes para modelar y almacenar datos.

```text
SQL   -> tablas, esquema, relaciones y consultas SQL
NoSQL -> almacenamiento documental, clave-valor, columnas anchas o grafos
```

Ejemplo SQL:

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
);
```

Ejemplo de documento NoSQL:

```json
{
  "id": "1",
  "name": "Alex",
  "settings": {
    "theme": "dark"
  }
}
```

Diferencias principales:

```text
Esquema:
SQL   -> esquema más rígido y migraciones
NoSQL -> esquema más flexible y mayor control en la aplicación o el backend

Relaciones:
SQL   -> JOIN, claves foráneas y normalización
NoSQL -> mayor uso de desnormalización para lecturas concretas

Transacciones:
SQL   -> normalmente ofrece garantías ACID sólidas
NoSQL -> depende de la base de datos y su configuración

Escalado:
SQL   -> suele utilizar escalado vertical, réplicas y sharding
NoSQL -> suele facilitar el escalado horizontal
```

Un ejemplo SQL en Android es Room sobre SQLite:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

Un ejemplo NoSQL en Android es Firebase Firestore:

```kotlin
firestore.collection("users").document(id).get()
```

SQL resulta preferible cuando:

- la estructura de los datos es estable;
- existen relaciones y consultas complejas;
- se necesitan restricciones;
- las transacciones y la consistencia son fundamentales.

NoSQL resulta más apropiado cuando:

- los datos son naturalmente documentales o de clave-valor;
- el esquema cambia con frecuencia;
- las relaciones son mínimas;
- se necesita desnormalización para realizar lecturas rápidas;
- existe una carga de trabajo distribuida o de gran escala.

La elección no debe basarse en modas, sino en los patrones de acceso, los
requisitos de consistencia, las relaciones, el escalado y la experiencia del
equipo.

**En resumen:** SQL utiliza un modelo relacional con tablas, esquema, joins y
transacciones sólidas. NoSQL incluye almacenes no relacionales con modelos de
datos más flexibles, a menudo desnormalizados y con un escalado horizontal más
sencillo.

</details>

<details>

<summary>191. ¿Qué es DataStore?</summary>

#### Kotlin

`DataStore` es un almacenamiento de Jetpack para guardar de forma asíncrona una
pequeña cantidad de ajustes persistentes. Funciona con corrutinas y `Flow`, y es
una alternativa moderna a `SharedPreferences`.

Datos habituales:

- tema;
- idioma;
- estado de la introducción inicial;
- preferencias del usuario;
- indicadores de funcionalidades.

Para colecciones grandes, relaciones, consultas o una caché sin conexión debe
utilizarse `Room`, no DataStore.

Existen dos variantes:

```text
Preferences DataStore -> clave-valor sin esquema
Proto DataStore       -> esquema protobuf y modelo seguro respecto a los tipos
```

Preferences DataStore:

```kotlin
val Context.dataStore by preferencesDataStore(name = "settings")

class SettingsRepository(
    private val context: Context
) {
    private val darkModeKey = booleanPreferencesKey("dark_mode")

    val darkMode: Flow<Boolean> = context.dataStore.data
        .map { preferences -> preferences[darkModeKey] ?: false }

    suspend fun setDarkMode(enabled: Boolean) {
        context.dataStore.edit { preferences ->
            preferences[darkModeKey] = enabled
        }
    }
}
```

Proto DataStore utiliza un esquema `.proto`:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Diferencia respecto a `SharedPreferences`:

```text
SharedPreferences -> API síncrona de clave-valor
DataStore         -> API suspendible, Flow y actualizaciones transaccionales
```

En un ViewModel, normalmente se transforma el `Flow` en `StateFlow`:

```kotlin
class SettingsViewModel(
    repository: SettingsRepository
) : ViewModel() {
    val darkMode = repository.darkMode.stateIn(
        viewModelScope,
        SharingStarted.WhileSubscribed(5_000),
        false
    )
}
```

La escritura mediante `edit` es una operación transaccional de lectura,
modificación y escritura:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

Los errores de lectura deben tratarse explícitamente:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
    }
```

Para migrar desde `SharedPreferences` se utiliza `SharedPreferencesMigration`.

**En resumen:** `DataStore` almacena pequeños ajustes mediante corrutinas y
`Flow`. `Preferences DataStore` resulta apropiado para clave-valor y `Proto
DataStore` para ajustes tipados; los datos relacionales complejos deben
almacenarse en `Room`.

</details>

<details>

<summary>192. ¿Qué es Paging 3?</summary>

#### Kotlin

`Paging 3` es una biblioteca de Jetpack que carga listas grandes por partes.
Administra la paginación, la precarga, la actualización, los reintentos, la
cancelación y la integración con la interfaz.

Flujo principal:

```text
PagingSource -> Pager -> Flow<PagingData<T>> -> Interfaz
```

`PagingSource<Key, Value>` describe la fuente de datos y las reglas de carga de
páginas:

```kotlin
class UsersPagingSource(
    private val api: UsersApi
) : PagingSource<Int, UserDto>() {

    override suspend fun load(params: LoadParams<Int>): LoadResult<Int, UserDto> {
        val page = params.key ?: 1

        return try {
            val response = api.getUsers(page, params.loadSize)
            LoadResult.Page(
                data = response.users,
                prevKey = page.takeIf { it > 1 }?.minus(1),
                nextKey = response.nextPage
            )
        } catch (exception: IOException) {
            LoadResult.Error(exception)
        }
    }

    override fun getRefreshKey(state: PagingState<Int, UserDto>): Int? =
        state.anchorPosition
            ?.let(state::closestPageToPosition)
            ?.let { it.prevKey?.plus(1) ?: it.nextKey?.minus(1) }
}
```

`Pager` crea un flujo de `PagingData`:

```kotlin
fun users(): Flow<PagingData<UserDto>> =
    Pager(
        config = PagingConfig(
            pageSize = 20,
            prefetchDistance = 5
        ),
        pagingSourceFactory = { UsersPagingSource(api) }
    ).flow
```

En el ViewModel se añade `cachedIn` para que el flujo paginado viva en
`viewModelScope` y no vuelva a comenzar para cada collector:

```kotlin
val users = repository.users()
    .cachedIn(viewModelScope)
```

Interfaz de Compose:

```kotlin
val users = viewModel.users.collectAsLazyPagingItems()

LazyColumn {
    items(
        count = users.itemCount,
        key = users.itemKey { it.id }
    ) { index ->
        users[index]?.let(::UserRow)
    }
}
```

`LoadState` proporciona estados independientes para `refresh`, `append` y
`prepend`. Normalmente:

- `refresh`: carga o error de pantalla completa;
- `append`: carga o error en el pie;
- `retry()`: repite una carga fallida;
- `refresh()`: crea una generación nueva.

Para un enfoque offline-first se utiliza `RemoteMediator`:

```text
Interfaz <- PagingSource de Room
                 ^
RemoteMediator: API -> Room + claves remotas
```

En esta estructura, Room es la fuente de verdad y `RemoteMediator` carga páginas
de la API y las guarda transaccionalmente en la base de datos.

Errores habituales:

- claves de paginación incorrectas;
- ausencia de `cachedIn`;
- ignorar errores de `append` o `prepend`;
- claves de elementos inestables en la interfaz;
- mantener paginación manual junto con Paging 3.

**En resumen:** `Paging 3` carga listas grandes por páginas. El flujo básico es
`PagingSource -> Pager -> PagingData -> Interfaz`; `cachedIn` conserva el flujo
en el ViewModel y `RemoteMediator` permite combinar API y Room en un enfoque
offline-first.

</details>

<details>

<summary>193. ¿Qué es RecyclerView.Adapter?</summary>

#### Kotlin

`RecyclerView.Adapter` es el puente entre un conjunto de datos y
`RecyclerView`. Crea objetos `ViewHolder`, vincula los datos a la interfaz de cada
elemento e informa de la cantidad de elementos.

Métodos principales:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

Ejemplo con `ListAdapter`:

```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
) : ListAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        val inflater = LayoutInflater.from(parent.context)
        val binding = ItemUserBinding.inflate(inflater, parent, false)
        return UserViewHolder(binding, onUserClick)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}

class UserViewHolder(
    private val binding: ItemUserBinding,
    private val onUserClick: (User) -> Unit
) : RecyclerView.ViewHolder(binding.root) {

    fun bind(user: User) {
        binding.name.text = user.name
        binding.email.text = user.email
        binding.badge.isVisible = user.isAdmin
        binding.root.setOnClickListener { onUserClick(user) }
    }
}
```

`RecyclerView` reutiliza las vistas de los elementos, por lo que `bind()` debe
establecer todo el estado de la interfaz, no solo los campos modificados. De lo
contrario, una vista reciclada puede mostrar el estado del elemento anterior.

`DiffUtil` determina qué ha cambiado:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

```text
areItemsTheSame()    -> identidad
areContentsTheSame() -> contenido
```

`ListAdapter` con `DiffUtil` es preferible a una lista mutable y
`notifyDataSetChanged()`, porque solo actualiza los elementos modificados.

Para layouts diferentes se utiliza `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int = when (getItem(position)) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Errores habituales:

- lógica de negocio o navegación dentro del adapter;
- uso innecesario de `notifyDataSetChanged()`;
- estado incompleto en `bind()`;
- referencia a una `Activity` o un `Fragment`;
- guardar una `position` antigua en lugar de trabajar con el elemento vinculado.

**En resumen:** `RecyclerView.Adapter` transforma datos en vistas de elementos
recicladas. En producción son fundamentales un `bind()` completo, `ListAdapter`
con `DiffUtil`, la transmisión estable de clics hacia el exterior y la ausencia
de lógica de negocio en el adapter.

</details>

<details>

<summary>194. ¿Cuáles son los métodos principales de RecyclerView.Adapter?</summary>

#### Kotlin

Los métodos principales de `RecyclerView.Adapter` son:

```kotlin
override fun onCreateViewHolder(
    parent: ViewGroup,
    viewType: Int
): UserViewHolder

override fun onBindViewHolder(
    holder: UserViewHolder,
    position: Int
)

override fun getItemCount(): Int
```

`onCreateViewHolder()` crea un `ViewHolder` nuevo cuando no existe uno
reutilizable:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
    val binding = ItemUserBinding.inflate(
        LayoutInflater.from(parent.context),
        parent,
        false
    )
    return UserViewHolder(binding)
}
```

`onBindViewHolder()` vincula un elemento al holder:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}
```

Debido al reciclaje, `bind()` debe establecer todo el estado de la interfaz:
valores `true` y `false`, `null`, valores vacíos, visibilidad y estado
seleccionado.

`getItemCount()` devuelve el tamaño del conjunto de datos:

```kotlin
override fun getItemCount(): Int = items.size
```

Para layouts diferentes se utiliza `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

Puede realizarse una actualización parcial mediante la sobrecarga con payloads:

```kotlin
override fun onBindViewHolder(
    holder: UserViewHolder,
    position: Int,
    payloads: MutableList<Any>
) {
    if (payloads.isEmpty()) holder.bind(items[position])
    else holder.bindPayload(items[position], payloads)
}
```

Callbacks adicionales del ciclo de vida:

- `onViewRecycled()`: limpieza antes de reutilizar el holder;
- `onViewAttachedToWindow()`: el elemento se adjunta a la ventana;
- `onViewDetachedFromWindow()`: el elemento se separa de la ventana.

Las actualizaciones del conjunto de datos deben ser precisas:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

En producción suele utilizarse `ListAdapter` o `AsyncListDiffer` con `DiffUtil`,
en lugar de una lista mutable manual y `notifyDataSetChanged()`.

Errores habituales:

- estado incompleto en `bind()`;
- trabajo pesado en `onBindViewHolder()`;
- guardar una `position` antigua;
- modificar el conjunto de datos sin la notificación o diff correspondiente;
- identificadores estables que no son únicos.

**En resumen:** los métodos principales del adapter son `onCreateViewHolder()`,
`onBindViewHolder()` y `getItemCount()`. También son importantes
`getItemViewType()`, la vinculación mediante payloads, los callbacks de reciclaje
y la actualización correcta del conjunto de datos.

</details>

<details>

<summary>195. ¿Cuál es la diferencia entre RecyclerView.Adapter y ListAdapter?</summary>

#### Kotlin

`ListAdapter` es un `RecyclerView.Adapter` que ya incorpora `AsyncListDiffer` y
`DiffUtil`. Con un adapter convencional, el desarrollador debe administrar
manualmente la lista y las llamadas `notify...`.

```text
RecyclerView.Adapter -> lista manual + notificaciones manuales
ListAdapter          -> submitList() + DiffUtil asíncrono
```

Un `RecyclerView.Adapter` convencional:

```kotlin
class UserAdapter : RecyclerView.Adapter<UserViewHolder>() {
    private val items = mutableListOf<UserUi>()

    override fun getItemCount(): Int = items.size

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(items[position])
    }

    fun replace(newItems: List<UserUi>) {
        items.clear()
        items.addAll(newItems)
        notifyDataSetChanged()
    }
}
```

El problema es que la modificación del conjunto de datos y las llamadas
`notify...` deben coincidir exactamente. `notifyDataSetChanged()` es sencillo,
pero elimina las actualizaciones granulares, las animaciones y parte de las
optimizaciones.

`ListAdapter`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Actualización:

```kotlin
adapter.submitList(users)
```

`ListAdapter` calcula el diff fuera del subproceso principal y aplica en él las
actualizaciones precisas de inserción, eliminación, movimiento y cambio.

`DiffUtil.ItemCallback`:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(old: UserUi, new: UserUi): Boolean =
        old.id == new.id

    override fun areContentsTheSame(old: UserUi, new: UserUi): Boolean =
        old == new
}
```

```text
areItemsTheSame()    -> identidad del elemento
areContentsTheSame() -> contenido renderizado
```

Es importante proporcionar a `submitList()` una instantánea inmutable. No debe
modificarse la misma lista ni sus elementos después del envío, ya que el diff
podría no detectar el cambio.

```kotlin
adapter.submitList(oldList + newUser)
```

Cuándo elegir cada opción:

- `ListAdapter`: opción predeterminada para listas dinámicas convencionales;
- `RecyclerView.Adapter`: cuando las actualizaciones ya llegan como operaciones
  precisas o se necesita una arquitectura personalizada de almacenamiento o
  comparación;
- `PagingDataAdapter`: para Paging 3.

Errores habituales:

- implementación incorrecta de `areItemsTheSame()`;
- modificar una lista mutable después de `submitList()`;
- utilizar `notifyDataSetChanged()` en lugar de un diff;
- incluir lógica de negocio en el adapter;
- identificadores estables que no coinciden con la identidad de DiffUtil.

**En resumen:** `RecyclerView.Adapter` ofrece control manual completo sobre la
lista y las notificaciones. `ListAdapter` recibe instantáneas inmutables mediante
`submitList()` y aplica automáticamente un diff asíncrono, por lo que es la mejor
opción predeterminada para la mayoría de las listas.

</details>

<details>

<summary>196. ¿Cómo funciona RecyclerView internamente?</summary>

#### Kotlin

`RecyclerView` muestra listas grandes sin crear una View para cada elemento.
Conserva las vistas visibles, una pequeña caché y un grupo de objetos
`ViewHolder` reutilizables.

```text
10 000 elementos de datos
~10-20 vistas visibles
caché + grupo de reciclaje
```

Componentes principales:

- `Adapter`: crea objetos `ViewHolder` y vincula los datos;
- `ViewHolder`: conserva la vista del elemento y referencias a sus vistas;
- `LayoutManager`: mide y posiciona los elementos;
- `Recycler` o `RecycledViewPool`: reutiliza holders;
- `ItemAnimator`: anima los cambios;
- `ItemDecoration`: añade separadores, espaciado o dibujo personalizado.

Flujo simplificado:

```text
LayoutManager solicita una vista
 -> Recycler busca un holder en la caché o el pool
 -> Adapter crea un holder si no puede reutilizarse ninguno
 -> Adapter vincula el elemento
 -> LayoutManager mide y posiciona la vista
```

Durante el desplazamiento, la vista de un elemento que sale de la pantalla se
separa y puede reutilizarse para otro elemento del mismo `viewType`.

Debido al reciclaje, `bind()` debe establecer todo el estado de la interfaz:

```kotlin
fun bind(item: Item) {
    binding.title.text = item.title
    binding.badge.isVisible = item.isImportant
    binding.checkbox.isChecked = item.isSelected
}
```

Si no se establecen valores `false`, `null`, vacíos o de visibilidad, una vista
reciclada puede mostrar el estado del elemento anterior.

`LayoutManager` define la estrategia del layout:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
// GridLayoutManager(context, 2)
// StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

`getItemViewType()` separa layouts diferentes. El pool agrupa holders por
`viewType`, por lo que una cabecera no se reutilizará como una publicación.

Niveles de reutilización simplificados:

```text
attached scrap -> vistas del paso de layout actual
cached views    -> holders separados recientemente
recycled pool   -> holders agrupados por viewType para volver a vincularlos
```

Para actualizar una lista, RecyclerView necesita notificaciones precisas o un
diff:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
```

En producción suelen utilizarse `ListAdapter` o `AsyncListDiffer` con `DiffUtil`.
`notifyDataSetChanged()` no proporciona un diff preciso y empeora las animaciones
y la reutilización.

Reglas prácticas:

- `onBindViewHolder()` debe ser barato;
- no realizar E/S de archivos o red ni decodificar bitmaps durante la
  vinculación;
- los callbacks de clic deben trabajar con el elemento vinculado, no con una
  `position` antigua;
- después de una inserción o eliminación, la posición puede cambiar;
- las imágenes deben cargarse mediante un cargador con caché y cancelación.

**En resumen:** `RecyclerView` es eficiente gracias al reciclaje de `ViewHolder`,
`LayoutManager`, la caché y el pool, y las actualizaciones precisas mediante
`DiffUtil`. Lo más importante es que `bind()` sea completo y barato.

</details>

<details>

<summary>197. ¿Qué es DiffUtil?</summary>

#### Kotlin

`DiffUtil` es una utilidad para `RecyclerView` que compara una lista antigua con
una nueva y detecta los cambios precisos: inserciones, eliminaciones, movimientos
y modificaciones.

```text
notifyDataSetChanged() -> actualizar todo
DiffUtil               -> actualizar únicamente lo modificado
```

Con `ListAdapter` suele implementarse `DiffUtil.ItemCallback`:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

Significado de los métodos:

```text
areItemsTheSame()    -> ¿es el mismo elemento? Normalmente se usa un id estable
areContentsTheSame() -> ¿son iguales los datos que renderiza la interfaz?
```

`ListAdapter` utiliza internamente `AsyncListDiffer`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Actualización de la lista:

```kotlin
adapter.submitList(users)
```

Para una actualización parcial se utiliza un payload:

```kotlin
override fun getChangePayload(oldItem: UserUi, newItem: UserUi): Any? {
    return if (oldItem.isOnline != newItem.isOnline) OnlineStatusChanged
    else null
}
```

El payload permite actualizar una parte de la fila sin volver a vincularla por
completo.

La inmutabilidad es fundamental: `submitList()` debe recibir una instantánea
nueva, no una lista antigua modificada.

```kotlin
adapter.submitList(oldList + newUser)
```

Errores habituales:

- `areItemsTheSame()` compara el objeto completo en lugar del identificador;
- `areContentsTheSame()` siempre devuelve `true`;
- identificador inestable;
- modificación de la lista o sus elementos después de `submitList()`;
- uso de `notifyDataSetChanged()` en lugar del diff;
- un `equals()` demasiado costoso para modelos anidados grandes.

**En resumen:** `DiffUtil` calcula la diferencia entre las listas antigua y nueva
para que RecyclerView actualice únicamente los elementos modificados.
`areItemsTheSame()` determina la identidad, `areContentsTheSame()` el contenido,
y `submitList()` debe recibir una instantánea inmutable.

</details>

<details>

<summary>198. ¿Cómo se muestran listas en Android?</summary>

#### Kotlin

En una interfaz basada en vistas o XML, las listas suelen mostrarse mediante
`RecyclerView`. En Jetpack Compose se utilizan contenedores lazy:
`LazyColumn`, `LazyRow` y `LazyVerticalGrid`. Para listas grandes o remotas se
añade Paging 3.

### Sistema de vistas: RecyclerView

```kotlin
val adapter = UserAdapter(onClick = viewModel::onUserClicked)

binding.recyclerView.apply {
    layoutManager = LinearLayoutManager(requireContext())
    this.adapter = adapter
}

adapter.submitList(users)
```

En producción es preferible utilizar `ListAdapter` con
`DiffUtil.ItemCallback`, en lugar de una lista mutable y
`notifyDataSetChanged()`.

`LayoutManager` determina la apariencia de la lista:

- `LinearLayoutManager`: lista vertical u horizontal;
- `GridLayoutManager`: cuadrícula;
- `StaggeredGridLayoutManager`: cuadrícula con elementos de distintas alturas.

### Paging 3

Para la carga por páginas se utiliza `PagingDataAdapter`:

```kotlin
class UserPagingAdapter :
    PagingDataAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        getItem(position)?.let(holder::bind)
    }
}
```

ViewModel:

```kotlin
val users: Flow<PagingData<User>> =
    repository.getUsers().cachedIn(viewModelScope)
```

Fragment:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.users.collectLatest(adapter::submitData)
    }
}
```

`loadState` permite representar los estados de carga, error, lista vacía y
reintento. Es importante tratar `refresh` y `append` por separado para que un
indicador de carga en el pie no sustituya toda la pantalla.

### Compose

```kotlin
@Composable
fun UserList(
    users: List<User>,
    onClick: (User) -> Unit
) {
    LazyColumn {
        items(
            items = users,
            key = User::id
        ) { user ->
            UserItem(user, onClick = { onClick(user) })
        }
    }
}
```

Una `key` estable conserva la identidad del elemento durante inserciones,
eliminaciones y reordenaciones. La clave debe ser única y estable, no una
posición.

Otros contenedores lazy:

```kotlin
LazyRow {
    items(categories, key = Category::id) { CategoryChip(it) }
}

LazyVerticalGrid(columns = GridCells.Fixed(2)) {
    items(products, key = Product::id) { ProductCard(it) }
}
```

Paging se integra en Compose mediante `collectAsLazyPagingItems()`, tratando
también `loadState` y `retry()`.

Reglas prácticas:

- no añadir manualmente muchas vistas a un `LinearLayout`;
- no utilizar un `Column` convencional para una lista dinámica grande;
- proporcionar instantáneas inmutables;
- conservar el estado de la lista en el ViewModel;
- mostrar estados de carga, error, lista vacía y reintento;
- no realizar tareas pesadas durante `bind()` ni durante la composición.

**En resumen:** en una interfaz basada en vistas se utiliza `RecyclerView` con
`ListAdapter`; en Compose, `LazyColumn`, `LazyRow` o `LazyVerticalGrid`. Para
listas remotas grandes se añade Paging 3, identificadores o claves estables y un
tratamiento correcto de los estados de carga.

</details>

<details>

<summary>199. ¿Qué son los contenedores lazy en Compose?</summary>

#### Kotlin

Los contenedores lazy de Compose son componentes para listas y cuadrículas que
solo componen los elementos necesarios para el viewport, en lugar de componer
toda la colección de una vez.

Contenedores principales:

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

Ejemplo con `LazyColumn`:

```kotlin
@Composable
fun UsersList(users: List<UserUi>) {
    LazyColumn {
        items(users) { user ->
            UserRow(user = user)
        }
    }
}
```

Para proporcionar una identidad estable debe especificarse una `key`:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { user -> user.id }
    ) { user ->
        UserRow(user = user)
    }
}
```

La `key` ayuda a conservar el estado de un elemento después de una inserción,
eliminación o reordenación. Debe ser estable y única, no una posición.

Para distintos tipos de elementos se especifica `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` ayuda a Compose a reutilizar de forma más eficiente la composición
entre tipos de elementos compatibles.

El desplazamiento se controla mediante `LazyListState`:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

Desplazamiento programático:

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

Ejemplo de cuadrícula:

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

Paging se integra mediante `collectAsLazyPagingItems()`:

```kotlin
val users = pager.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user -> UserRow(user) }
    }
}
```

Los distintos estados de `loadState` deben tratarse por separado: carga inicial,
carga al final, error, lista vacía y reintento.

Reglas de rendimiento:

- utilizar una `key` estable;
- especificar `contentType` para tipos de elementos diferentes;
- no realizar E/S ni cálculos pesados dentro de un elemento;
- no decodificar bitmaps durante la composición;
- evitar contenedores de desplazamiento anidados innecesarios;
- proporcionar modelos de interfaz inmutables y estables.

**En resumen:** los contenedores lazy, como `LazyColumn`, `LazyRow` y las
cuadrículas, renderizan listas y rejillas grandes según el viewport. En
producción son importantes las claves estables, `contentType`, una interfaz de
elemento ligera, `LazyListState` y un tratamiento correcto de los estados de
carga de Paging.

</details>

<details>

<summary>200. ¿Cómo se implementa una lista cíclica en RecyclerView?</summary>

#### Kotlin

Un `RecyclerView` cíclico suele simularse mediante un `itemCount` muy grande: la
posición ficticia del adapter se transforma en una posición real mediante el
operador módulo.

Adapter básico:

```kotlin
class LoopAdapter(
    private val items: List<Item>
) : RecyclerView.Adapter<ItemViewHolder>() {

    override fun getItemCount(): Int =
        if (items.isEmpty()) 0 else Int.MAX_VALUE

    override fun onBindViewHolder(holder: ItemViewHolder, position: Int) {
        val realPosition = position % items.size
        holder.bind(items[realPosition])
    }
}
```

Para comenzar permitiendo el desplazamiento en ambas direcciones, la posición se
sitúa cerca del centro del intervalo:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

Es importante comprobar `items.isEmpty()`; de lo contrario, la operación módulo
provocará una división por cero. El desplazamiento inicial también debe realizarse
únicamente cuando la lista no esté vacía.

Para obtener un comportamiento de carrusel se añade ajuste por páginas:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

El desplazamiento automático debe respetar el ciclo de vida:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.RESUMED) {
        while (isActive) {
            delay(3_000)
            val manager = recyclerView.layoutManager as LinearLayoutManager
            val next = manager.findFirstVisibleItemPosition() + 1
            recyclerView.smoothScrollToPosition(next)
        }
    }
}
```

Los identificadores estables representan un riesgo. No puede devolverse el mismo
identificador real para todas las repeticiones, ya que las posiciones del adapter
son distintas. Si no se necesitan identificadores estables, es preferible no
activarlos en un adapter que simula una lista infinita.

Al actualizar el conjunto de datos debe conservarse una lista real inmutable y,
después de cambiar su tamaño, volver a alinearse la posición ficticia actual con
el nuevo módulo.

Problemas habituales:

- aplicar módulo a una lista vacía;
- mantener el desplazamiento automático después de `onDestroyView()`;
- identificadores estables no únicos;
- restaurar incorrectamente la posición tras cambiar el conjunto de datos;
- semántica de accesibilidad compleja;
- mala experiencia de usuario por utilizar desplazamiento infinito sin
  necesidad.

Alternativas:

- `ViewPager2` para un paginador o banner;
- `HorizontalPager` de Compose;
- una lista finita sin infinito simulado;
- duplicar los elementos de los extremos para una transición continua.

**En resumen:** un `RecyclerView` cíclico se implementa mediante un `itemCount`
grande, `position % items.size` y una posición inicial situada en el centro del
intervalo. Es imprescindible tratar la lista vacía, el ciclo de vida del
desplazamiento automático, los identificadores estables y las actualizaciones
del conjunto de datos.

</details>

<details>

<summary>201. ¿Qué son las animaciones en Android?</summary>

#### Kotlin

Una animación en Android es la modificación de una propiedad de la interfaz a lo
largo del tiempo, como `alpha`, traslación, escala, rotación, color, tamaño o
estado del layout. Una buena animación explica un cambio de estado, en lugar de
limitarse a añadir decoración.

### Sistema de vistas

Para animaciones sencillas de transformación:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

`ObjectAnimator` anima una propiedad concreta:

```kotlin
ObjectAnimator
    .ofFloat(view, View.ALPHA, 1f, 0f)
    .apply {
        duration = 300
        start()
    }
```

`ValueAnimator` genera valores para lógica personalizada:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    addUpdateListener { animation ->
        progressView.progress = animation.animatedValue as Int
    }
    start()
}
```

`AnimatorSet` combina varios animators y `MotionLayout` resulta apropiado para
transiciones complejas entre estados de layout.

### Compose

En Compose, las animaciones deben estar dirigidas por el estado:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "contentAlpha"
)

Box(Modifier.alpha(alpha))
```

API principales:

- `animate*AsState`: una propiedad;
- `AnimatedVisibility`: entrada y salida;
- `AnimatedContent`: cambio de contenido;
- `updateTransition`: varias propiedades de un mismo estado;
- `Animatable`: control suspendible imperativo;
- `rememberInfiniteTransition`: animaciones decorativas infinitas.

### Rendimiento

Por lo general, resulta más barato animar:

```text
alpha, traslación, escala y rotación
```

Animar `width`, `height`, `margin` o parámetros de layout puede provocar una
nueva medición y disposición en cada frame. No está prohibido, pero requiere
perfilado.

Los callbacks no deben realizar E/S, decodificación de bitmaps, cálculos pesados
ni numerosas asignaciones.

### Ciclo de vida

Las animaciones de vistas deben cancelarse y su estado debe restablecerse,
especialmente en un `RecyclerView`:

```kotlin
override fun onViewRecycled(holder: ItemHolder) {
    holder.itemView.animate().cancel()
    holder.reset()
}
```

De lo contrario, un holder reciclado puede mostrar el valor de `alpha`,
traslación o selección del elemento anterior.

Los efectos y las corrutinas de Compose se cancelan junto con el ámbito de la
composición, aunque los recursos externos siguen necesitando limpieza.

Elección práctica:

- transformación sencilla de una View: `view.animate()`;
- control de una propiedad o valor: `ObjectAnimator` o `ValueAnimator`;
- transición coordinada: `AnimatorSet` o `MotionLayout`;
- interfaz de Compose: API de animación dirigidas por el estado;
- RecyclerView: cancelar y restablecer el estado;
- producción: respetar la escala de animación del sistema y la reducción de
  movimiento.

**En resumen:** Android dispone de animators para vistas, `MotionLayout` y API de
animación de Compose. Una animación de producción debe estar dirigida por el
estado, ser económica para el renderizado de frames, respetar el ciclo de vida y
no dejar estado residual en vistas recicladas.

</details>

<details>

<summary>202. ¿Cuál es la diferencia entre ViewBinding y DataBinding?</summary>

#### Kotlin

`ViewBinding` y `DataBinding` eliminan la necesidad de utilizar manualmente
`findViewById`, pero resuelven problemas distintos.

```text
ViewBinding -> acceso seguro respecto a los tipos a las vistas
DataBinding -> vinculación de datos y expresiones en XML
```

### ViewBinding

Genera una clase de binding para un layout XML:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Ventajas:

- API sencilla;
- acceso seguro respecto a los tipos a las vistas;
- menor sobrecarga de compilación;
- depuración más sencilla;
- no traslada la lógica a XML.

### DataBinding

Permite declarar variables y expresiones directamente en XML:

```xml
<layout>
    <data>
        <variable
            name="user"
            type="com.example.UserUi" />
    </data>

    <TextView
        android:text="@{user.name}" />
</layout>
```

En Kotlin:

```kotlin
binding.user = user
binding.executePendingBindings()
```

DataBinding también admite vinculación bidireccional:

```xml
<EditText
    android:text="@={viewModel.query}" />
```

Puede resultar práctico para formularios sencillos, pero en proyectos grandes
suele dificultar la depuración del flujo de datos.

Principales riesgos de DataBinding:

- generación de código más compleja;
- mayor sobrecarga durante la compilación y ejecución;
- errores menos evidentes;
- tentación de trasladar lógica de interfaz o negocio a XML;
- incorporación más difícil de nuevos miembros al equipo.

Una dirección incorrecta consiste en introducir lógica compleja en el layout:

```xml
android:visibility="@{user.isAdmin ? View.VISIBLE : View.GONE}"
```

Las expresiones sencillas son aceptables, pero la lógica compleja debe permanecer
en un ViewModel o un mapper.

Elección práctica:

- código nuevo con XML y sistema de vistas: normalmente `ViewBinding`;
- proyecto heredado con expresiones XML: puede conservar `DataBinding`;
- nueva interfaz declarativa: normalmente Compose.

**En resumen:** `ViewBinding` proporciona un acceso sencillo, ligero y seguro
respecto a los tipos a las vistas. `DataBinding` añade variables, expresiones y
vinculación bidireccional en XML, pero introduce mayor sobrecarga y el riesgo de
ocultar lógica dentro del layout.

</details>

<details>

<summary>203. ¿Qué es Serializable?</summary>

#### Kotlin

`java.io.Serializable` es una interfaz marcadora para la serialización binaria de
Java. La JVM puede escribir un grafo de objetos como bytes mediante
`ObjectOutputStream` y restaurarlo mediante `ObjectInputStream`.

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

No es lo mismo que `kotlinx.serialization.Serializable`: en este último caso, una
anotación genera serializadores para JSON, Proto y otros formatos.

En Android, un objeto `Serializable` puede transferirse mediante un `Intent` o un
`Bundle`:

```kotlin
val intent = Intent(context, DetailsActivity::class.java)
    .putExtra("user", user)
```

A partir de la API 33 existe una sobrecarga tipada:

```kotlin
val user = intent.getSerializableExtra(
    "user",
    User::class.java
)
```

Para admitir distintas versiones de API es preferible utilizar funciones de
compatibilidad, como `IntentCompat.getSerializableExtra()`.

Comparación con `Parcelable`:

```text
Serializable -> serialización de JVM durante la ejecución, menos código y más sobrecarga
Parcelable   -> formato de Android para IPC y Bundle, más rápido y explícito
```

Para argumentos de Android suele ser preferible utilizar `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Para la navegación resulta aún mejor transferir un identificador primitivo y
cargar los datos actualizados desde un repositorio.

Para la compatibilidad entre versiones, la serialización de Java utiliza
`serialVersionUID`:

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable {
    companion object {
        private const val serialVersionUID = 1L
    }
}
```

Un UID explícito fija el identificador de versión, pero no hace que la evolución
del esquema sea segura automáticamente.

Riesgos:

- sobrecarga de reflexión y metadatos;
- evolución deficiente del esquema;
- todos los campos anidados también deben ser serializables;
- posible `NotSerializableException`;
- los objetos grandes en un `Bundle` o `Intent` pueden provocar
  `TransactionTooLargeException`;
- no deben aceptarse bytes serializados no confiables procedentes de la red o de
  almacenamiento externo.

```kotlin
// incorrecto
intent.putExtra("response", hugeResponse)

// preferible
intent.putExtra("user_id", user.id)
```

Cuándo puede ser aceptable:

- un pequeño argumento interno en código heredado;
- un objeto de corta duración dentro de una misma aplicación;
- cuando el rendimiento y la evolución del esquema no son críticos.

**En resumen:** `Serializable` de Java serializa un grafo de objetos mediante el
mecanismo de ejecución de la JVM. En Android suele ser preferible utilizar
`Parcelable` o simplemente un identificador para los argumentos; para
persistencia o red debe utilizarse un esquema explícito mediante Room, DataStore,
JSON o Proto.

</details>

<details>

<summary>204. ¿Qué formas de serialización existen en Android?</summary>

#### Kotlin

La serialización consiste en transformar un objeto a un formato adecuado para
transferirlo o almacenarlo, como JSON, bytes, `Bundle`, una fila de base de datos
o protobuf.

En Android suele necesitarse para:

- API y red;
- argumentos de navegación;
- `Intent`, `Bundle` e IPC;
- almacenamiento local;
- ajustes.

### JSON

Para las API se utiliza principalmente JSON mediante Kotlin Serialization, Moshi
o Gson.

Kotlin Serialization:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String
)

val json = Json.encodeToString(UserDto("1", "Alex"))
val user = Json.decodeFromString<UserDto>(json)
```

Ventajas de Kotlin Serialization: buena compatibilidad con Kotlin, `sealed
class`, complemento de compilación, Ktor y multiplataforma.

Moshi se utiliza con frecuencia junto con Retrofit. En producción es preferible
utilizar generación de código en lugar de reflexión.

Gson es sencillo y habitual en proyectos heredados, pero gestiona peor la
nulabilidad de Kotlin, depende mucho de la reflexión y puede crear silenciosamente
objetos incorrectos.

### Parcelable

`Parcelable` es el formato de Android para transferir objetos mediante `Intent`,
`Bundle`, argumentos de navegación o IPC.

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Para la navegación suele ser preferible transferir un identificador primitivo en
lugar del objeto completo:

```kotlin
bundleOf("user_id" to userId)
```

Después, los datos actualizados se cargan desde un repositorio o ViewModel.

### Serializable

`Serializable` de Java es sencillo, pero introduce sobrecarga durante la
ejecución, una evolución de esquema más débil y menor control.

```kotlin
data class User(val id: String) : Serializable
```

Para argumentos de Android suele ser preferible utilizar `Parcelable` o un
identificador.

### Room

Para una base de datos local se utilizan entidades de Room y mappers, en lugar de
serializar directamente un objeto de dominio:

```kotlin
data class UserEntity(
    val id: String,
    val name: String
)

fun UserEntity.toDomain() = User(id, name)
```

Puede utilizarse un `TypeConverter` para determinados campos complejos, pero no
conviene convertir Room en un almacenamiento JSON sin necesidad.

### Proto y DataStore

Proto DataStore resulta apropiado para ajustes seguros respecto a los tipos o un
formato binario compacto. Requiere un esquema, pero proporciona un modelo tipado
estable.

Elección práctica:

- JSON de API: Kotlin Serialization o Moshi;
- Retrofit heredado: Moshi o Gson, según el proyecto;
- argumentos de navegación: tipos primitivos, identificadores o `Parcelable`;
- Intent y Bundle: un `Parcelable` pequeño, no una gran cantidad de datos;
- base de datos local: entidades de Room y mappers;
- ajustes: DataStore o Proto DataStore;
- `Serializable` de Java: únicamente para casos internos sencillos o heredados.

**En resumen:** para la red suelen utilizarse Kotlin Serialization o Moshi; para
argumentos de Android, `Parcelable` o un identificador primitivo; para bases de
datos, entidades de Room; y para ajustes, DataStore o Proto. Es preferible
reservar `Serializable` y Gson para proyectos heredados o situaciones sencillas.

</details>

<details>

<summary>205. ¿Qué debe hacerse si un campo puede faltar en la respuesta de una API?</summary>

#### Kotlin

Un DTO debe representar con precisión el contrato de la API: un campo ausente,
un valor `null` explícito y un campo obligatorio no válido son situaciones
distintas.

```text
campo opcional ausente -> valor predeterminado
null explícito         -> tipo nullable
campo obligatorio ausente -> error de decodificación o transformación
```

Si el backend puede enviar `null`, el campo debe ser nullable:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

Es preferible establecer un valor alternativo en el mapper, en lugar de propagar
la nulabilidad por toda la interfaz:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name?.takeIf { it.isNotBlank() } ?: "Unknown",
    avatarUrl = avatarUrl
)
```

Si el campo puede faltar, pero existe un valor predeterminado seguro:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Un campo ausente y un `null` explícito no son lo mismo:

```json
{}
```

```json
{ "name": null }
```

En `kotlinx.serialization`, el valor predeterminado se utiliza cuando falta la
clave. Una propiedad nullable sin valor predeterminado puede seguir siendo
obligatoria, según su declaración y configuración. Debe comprenderse el
comportamiento de `explicitNulls` y cubrirse mediante pruebas.

Los DTO no deben entregarse directamente a la interfaz. Un mapper debe crear un
modelo de dominio o de interfaz con garantías claras:

```kotlin
data class UserUi(
    val id: String,
    val title: String,
    val avatarUrl: String?
)

fun UserDto.toUi(): UserUi = UserUi(
    id = id,
    title = name?.takeIf { it.isNotBlank() } ?: "No name",
    avatarUrl = avatarUrl
)
```

Un campo obligatorio no debe ocultarse mediante un valor alternativo:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

El repositorio puede transformar una respuesta no válida en un error tipado:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

Reglas prácticas:

- campo opcional ausente: valor predeterminado;
- `null` explícito: tipo nullable;
- campo obligatorio: validación o fallo;
- DTO, mapper y modelo de dominio o interfaz;
- probar el comportamiento del serializador mediante pruebas de contrato para
  campos ausentes, `null`, tipos incorrectos y valores enum desconocidos.

**En resumen:** un campo ausente se modela mediante un valor predeterminado y un
`null` explícito mediante un tipo nullable. Un campo obligatorio sin valor debe
producir un error de validación, no un valor alternativo silencioso. Los DTO deben
transformarse en modelos de dominio o interfaz.

</details>

<details>

<summary>206. ¿Qué es la inyección de dependencias?</summary>

#### Kotlin

`Dependency Injection` consiste en proporcionar a una clase sus dependencias
desde el exterior, en lugar de crearlas dentro de ella. Esto reduce el
acoplamiento y facilita las pruebas y la gestión del ciclo de vida.

Incorrecto, sin DI:

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Problemas:

- resulta difícil sustituir la dependencia en una prueba;
- la propia clase se encarga de crearla;
- queda vinculada rígidamente a una implementación;
- el ciclo de vida y la configuración se controlan en el lugar incorrecto.

La inyección mediante constructor suele ser la mejor opción predeterminada:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

La clase recibe una dependencia ya creada y desconoce quién la ha construido.

Normalmente, el código llamador depende de un contrato:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository {
    override suspend fun getUser(id: UserId): User = api.getUser(id).toDomain()
}
```

En Android, la inyección de dependencias conecta ViewModel, casos de uso,
repositorios, clientes de API, bases de datos Room y dispatchers.

Ejemplo con Hilt:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Vinculación de una interfaz:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface ProfileModule {
    @Binds
    fun bindProfileRepository(
        impl: ProfileRepositoryImpl
    ): ProfileRepository
}
```

Para objetos de terceros se utiliza `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

Los ámbitos determinan la duración de una instancia:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

El ámbito debe corresponder a su propietario. Un singleton innecesario puede
conservar un objeto más tiempo del necesario y provocar una fuga.

En las pruebas, la implementación real se sustituye por una implementación
falsa:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

La inyección de dependencias no requiere necesariamente un framework. Para un
grafo pequeño es suficiente la inyección manual:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Reglas prácticas:

- utilizar la inyección mediante constructor de forma predeterminada;
- hacer que el código llamador dependa de una interfaz o contrato;
- ajustar los ámbitos al ciclo de vida;
- Hilt, Dagger y Koin únicamente automatizan la conexión;
- la inyección de dependencias no sustituye una arquitectura adecuada.

**En resumen:** la inyección de dependencias proporciona las dependencias desde
el exterior. Reduce el acoplamiento, permite sustituirlas en las pruebas y ayuda
a administrar el ciclo de vida. Para un grafo pequeño basta con DI manual; para
uno grande pueden utilizarse Hilt, Dagger o Koin.

</details>

<details>

<summary>207. ¿Cuál es la diferencia entre Dagger/Hilt y Koin?</summary>

#### Kotlin

`Dagger/Hilt` y `Koin` son soluciones de inyección de dependencias con modelos de
conexión diferentes.

```text
Dagger/Hilt -> código generado y validación del grafo durante la compilación
Koin        -> contenedor en tiempo de ejecución y DSL/anotaciones de Kotlin
```

### Dagger y Hilt

`Dagger` genera factories y componentes, y comprueba el grafo de dependencias
durante la compilación.

Ventajas:

- validación durante la compilación;
- código generado sin búsquedas durante la ejecución;
- ámbitos y componentes explícitos;
- resulta apropiado para grafos grandes y estrictos.

Desventajas:

- más anotaciones y módulos;
- jerarquía de componentes más compleja;
- los errores del compilador pueden resultar difíciles de interpretar;
- puede afectar al tiempo de compilación.

`Hilt` es una capa para Android construida sobre Dagger. Proporciona una jerarquía
estándar de componentes Android, ámbitos e integraciones con Jetpack.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

Ámbitos habituales de Hilt:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

### Koin

Koin describe las dependencias mediante un DSL de Kotlin o anotaciones:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Ventajas:

- menos código ceremonial;
- DSL fácil de leer;
- puesta en marcha rápida en proyectos pequeños y medianos;
- práctico para conexiones personalizadas y casos de uso de KMP.

Desventajas:

- el DSL clásico realiza más búsquedas durante la ejecución;
- las definiciones ausentes o ambiguas pueden manifestarse durante la ejecución
  si no existen comprobaciones en tiempo de compilación;
- el equipo debe modelar disciplinadamente los ámbitos y el grafo de propiedad.

Las versiones modernas de Koin ofrecen herramientas y anotaciones para realizar
comprobaciones durante la compilación, pero su modelo sigue siendo distinto:
Koin se basa en un contenedor, mientras que Dagger y Hilt se basan en un grafo
generado.

### Rendimiento y pruebas

Dagger y Hilt invocan factories generadas. Koin resuelve las definiciones
mediante un contenedor. La diferencia debe medirse con el grafo real, no utilizar
un microbenchmark como único criterio para elegir un framework.

Para las pruebas:

- Hilt: componentes de prueba, `@TestInstallIn` y sustitución de módulos;
- Koin: módulos de prueba y sobrescrituras;
- pruebas unitarias: lo más sencillo es proporcionar directamente una
  implementación falsa mediante el constructor.

### Qué elegir

- Hilt: buena opción predeterminada para Android con Jetpack y un grafo grande;
- Dagger: cuando se necesita un grafo de DI muy explícito y controlado;
- Koin: cuando importan la sencillez del DSL, la rapidez de conexión o KMP;
- no conviene reescribir una solución DI existente que funciona sin una ventaja
  medible.

**En resumen:** Dagger y Hilt generan código de DI y comprueban el grafo durante
la compilación. Koin ofrece un enfoque más sencillo basado en un DSL y un
contenedor, pero requiere disciplina respecto a los ámbitos y las comprobaciones.
La elección depende del tamaño del grafo, los requisitos de compilación y
ejecución y la experiencia del equipo.

</details>

<details>

<summary>208. ¿Qué es CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` es un mecanismo de Jetpack Compose para transmitir valores
hacia abajo por el árbol de composición sin pasar explícitamente parámetros por
cada composable.

Funciona como un contexto de interfaz limitado a un subárbol:

```kotlin
CompositionLocalProvider(LocalSpacing provides AppSpacing()) {
    AppContent()
}
```

Declaración:

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

@Immutable
data class AppSpacing(
    val small: Dp = 8.dp,
    val medium: Dp = 16.dp,
    val large: Dp = 24.dp
)
```

Uso:

```kotlin
@Composable
fun ProfileCard() {
    val spacing = LocalSpacing.current

    Column(Modifier.padding(spacing.medium)) {
        Text("Profile")
    }
}
```

`ProfileCard` lee el valor de `LocalSpacing` proporcionado más cerca por encima
en el árbol.

El proveedor solo afecta a su subárbol de elementos hijos:

```kotlin
CompositionLocalProvider(
    LocalSpacing provides AppSpacing(medium = 20.dp)
) {
    ProfileScreen()
}
```

Diferencia entre las API:

```text
compositionLocalOf       -> seguimiento más preciso de las lecturas, para valores variables
staticCompositionLocalOf -> lectura más barata, para valores casi estáticos
```

Para tokens de un sistema de diseño suele utilizarse
`staticCompositionLocalOf`. Para valores que cambian con mayor frecuencia,
`compositionLocalOf`.

Locals integrados:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`;
- `LocalLayoutDirection`.

```kotlin
val context = LocalContext.current
```

CompositionLocal resulta apropiado para:

- colores del tema;
- tipografía;
- espaciado;
- tokens del sistema de diseño;
- idioma o configuración;
- utilidades de interfaz disponibles lógicamente para todo el subárbol.

No conviene ocultar dependencias de negocio mediante CompositionLocal:

```kotlin
val repository = LocalUserRepository.current // incorrecto
```

Los repositorios, casos de uso y servicios deben proporcionarse mediante DI o un
ViewModel. De lo contrario, CompositionLocal se convierte en un localizador de
servicios.

Riesgos:

- dependencias ocultas;
- pruebas más difíciles;
- procedencia poco clara del valor;
- recomposiciones innecesarias cuando cambia con frecuencia;
- uso excesivo en lugar de parámetros explícitos.

**En resumen:** `CompositionLocal` transmite valores de interfaz con ámbito hacia
abajo por el árbol de Compose sin necesidad de pasar parámetros repetidamente.
Resulta adecuado para temas, espaciado, tipografía y datos contextuales de la
interfaz, pero no para repositorios, casos de uso ni lógica de negocio.

</details>

<details>

<summary>209. ¿Qué tipos de CompositionLocal existen?</summary>

#### Kotlin

Compose ofrece dos formas principales de crear un `CompositionLocal`:

```text
staticCompositionLocalOf -> lectura más barata e invalidación menos precisa
compositionLocalOf       -> lecturas rastreadas y recomposición más precisa
```

`staticCompositionLocalOf`:

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

Las lecturas no se rastrean individualmente. Si cambia el valor, se invalida todo
el contenido del proveedor. Resulta apropiado para valores de interfaz casi
estáticos:

- colores del tema;
- tipografía;
- espaciado;
- tokens del sistema de diseño.

`compositionLocalOf`:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

Compose rastrea qué composables han leído `.current` y, cuando cambia el valor,
los invalida de forma más precisa. Es una opción más apropiada para valores que
pueden cambiar con mayor frecuencia.

Un proveedor establece el valor para un subárbol:

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

Un proveedor anidado puede sobrescribir el valor únicamente para su propio
subárbol.

Ejemplo de un local para un sistema de diseño:

```kotlin
@Immutable
data class AppSpacing(
    val small: Dp,
    val medium: Dp,
    val large: Dp
)

val LocalSpacing = staticCompositionLocalOf {
    AppSpacing(8.dp, 16.dp, 24.dp)
}
```

Uso:

```kotlin
Modifier.padding(LocalSpacing.current.medium)
```

CompositionLocals integrados:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLayoutDirection`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`.

Estos valores describen el entorno de interfaz del subárbol actual.

CompositionLocal no debe utilizarse como localizador de servicios:

```kotlin
val repository = LocalRepository.current // incorrecto
```

Los repositorios, casos de uso y servicios de negocio deben proporcionarse
mediante DI o un ViewModel. Las entradas importantes de un composable concreto
deben pasarse preferiblemente como parámetros explícitos.

Riesgos:

- dependencias ocultas;
- pruebas más complejas;
- propietario del valor poco claro;
- recomposiciones innecesarias;
- sustitución incorrecta de DI y state hoisting.

**En resumen:** `staticCompositionLocalOf` resulta apropiado para valores de
interfaz estables y lecturas más baratas. `compositionLocalOf` rastrea las
lecturas con mayor precisión y es preferible para valores variables. Ambos deben
utilizarse para el contexto de interfaz, no para dependencias de negocio.

</details>

<details>

<summary>210. ¿Cómo funciona CompositionLocal internamente?</summary>

#### Kotlin

`CompositionLocal` funciona como un valor con ámbito dentro del árbol de
Compose. `CompositionLocalProvider` establece un valor para un subárbol y los
composables situados debajo leen el valor más cercano mediante `.current`.

Modelo básico:

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

CompositionLocalProvider(LocalSpacing provides AppSpacing(16.dp)) {
    Content()
}
```

Lectura:

```kotlin
val spacing = LocalSpacing.current
```

Compose busca el proveedor más cercano de `LocalSpacing` dentro de la jerarquía
de composición.

El proveedor solo se aplica a su subárbol:

```text
Raíz
 ├─ LocalSpacing = 8dp
 │   └─ ScreenA lee 8dp
 └─ LocalSpacing = 16dp
     └─ ScreenB lee 16dp
```

Un proveedor anidado sobrescribe al exterior:

```kotlin
CompositionLocalProvider(LocalSpacing provides smallSpacing) {
    Header()

    CompositionLocalProvider(LocalSpacing provides largeSpacing) {
        Content()
    }
}
```

`Header` recibe `smallSpacing` y `Content`, `largeSpacing`.

Compose utiliza las lecturas para decidir la invalidación. Cuando un composable
lee:

```kotlin
val colors = LocalAppColors.current
```

Compose puede asociar esa lectura al valor y determinar que debe recomponer el
composable cuando cambie.

Diferencia entre los tipos:

```text
compositionLocalOf       -> lecturas rastreadas e invalidación más precisa
staticCompositionLocalOf -> las lecturas no se rastrean individualmente y son más baratas
```

`compositionLocalOf` resulta más apropiado para valores que pueden cambiar con
mayor frecuencia:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`staticCompositionLocalOf` es preferible para valores casi estáticos, como el
tema, la tipografía o el espaciado:

```kotlin
val LocalTypography = staticCompositionLocalOf {
    DefaultTypography
}
```

La función de valor predeterminado se invoca si no se ha proporcionado ningún
valor:

```kotlin
val LocalAnalytics = staticCompositionLocalOf<Analytics> {
    error("Analytics not provided")
}
```

Para valores obligatorios suele ser preferible lanzar `error`, de modo que una
configuración incorrecta se detecte rápidamente.

Un aspecto de rendimiento importante es que un CompositionLocal que cambia con
frecuencia y se lee en un subárbol grande puede provocar recomposiciones
innecesarias. No conviene almacenar en él estado de alta frecuencia, como la
posición de desplazamiento, el valor de un campo de texto o los pulsos de un
temporizador.

Como regla práctica, `CompositionLocal` no es una variable global, sino un
contexto de interfaz con ámbito. Debe utilizarse para valores del entorno de
interfaz, no para acceder de forma oculta a repositorios o casos de uso.

**En resumen:** `CompositionLocalProvider` establece un valor para un subárbol,
`.current` lee el valor más cercano y Compose utiliza las lecturas para decidir
la invalidación. `compositionLocalOf` rastrea las lecturas con mayor precisión y
`staticCompositionLocalOf` ofrece lecturas más baratas para valores casi
estáticos.

</details>

<details>

<summary>211. ¿Qué son los efectos secundarios en Compose?</summary>

#### Kotlin

Un efecto secundario en Compose es una acción ajena a la descripción pura de la
interfaz, como una corrutina, un listener, navegación, un snackbar, analíticas o
la sincronización con un objeto externo.

El cuerpo de un composable puede ejecutarse muchas veces, por lo que los efectos
no deben iniciarse directamente en él:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // incorrecto: se repetirá en cada recomposición
    Text(userId)
}
```

### LaunchedEffect

Inicia una corrutina vinculada a la composición:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Cuando cambia la clave, la corrutina anterior se cancela y se inicia una nueva.
Al salir de la composición, la corrutina también se cancela. Las claves deben
incluir las dependencias del efecto.

`LaunchedEffect(Unit)` se ejecuta una vez por cada entrada actual en la
composición, no una sola vez durante toda la vida de la aplicación.

### rememberCoroutineScope

Se utiliza cuando una corrutina debe iniciarse desde un callback:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch {
        snackbarHostState.showSnackbar("Saved")
    }
}) {
    Text("Save")
}
```

`LaunchedEffect` se inicia declarativamente según sus claves;
`rememberCoroutineScope`, de forma imperativa después de un evento del usuario.

### DisposableEffect

Se utiliza para registrar y anular registros de acuerdo con el ciclo de vida:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) {
            viewModel.refresh()
        }
    }

    lifecycleOwner.lifecycle.addObserver(observer)

    onDispose {
        lifecycleOwner.lifecycle.removeObserver(observer)
    }
}
```

Cada registro de un listener u observer debe tener una limpieza simétrica en
`onDispose`.

### SideEffect

Se ejecuta después de cada composición completada correctamente:

```kotlin
SideEffect {
    externalController.enabled = isEnabled
}
```

Resulta apropiado para publicar el estado de Compose en un objeto ajeno a
Compose. No está destinado a tareas suspendibles ni pesadas.

### Otras API

`rememberUpdatedState` proporciona la lambda o el valor actual a un efecto de
larga duración sin reiniciarlo:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

`snapshotFlow` transforma lecturas del estado de Compose en un Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect(analytics::trackScroll)
}
```

`produceState` adapta una fuente suspendible o basada en callbacks a un `State`
de Compose. Para un Flow en la interfaz suele utilizarse
`collectAsStateWithLifecycle()`.

`derivedStateOf` no es un efecto secundario, sino un estado derivado que permite
optimizar el estado calculado de la interfaz.

Reglas prácticas:

- no iniciar efectos en el cuerpo de un composable;
- definir correctamente las claves;
- realizar la limpieza en `onDispose`;
- utilizar `rememberCoroutineScope` para una corrutina iniciada desde un
  callback;
- utilizar `rememberUpdatedState` para acceder a una lambda actual sin reiniciar
  el efecto;
- conservar los datos duraderos como estado, no como un efecto de una sola
  ejecución.

**En resumen:** los efectos secundarios de Compose administran acciones ajenas a
la descripción de la interfaz. `LaunchedEffect` inicia una corrutina según sus
claves, `DisposableEffect` gestiona la limpieza, `SideEffect` sincroniza un
objeto externo y `rememberCoroutineScope` se utiliza para corrutinas iniciadas
desde callbacks.

</details>

<details>

<summary>212. ¿Cómo se optimiza la recomposición?</summary>

#### Kotlin

La recomposición no debe «desactivarse». Debe ser local y económica. Los
problemas aparecen debido a lecturas de estado demasiado amplias, parámetros
inestables o tareas pesadas en el cuerpo de un composable.

### Estado en el nivel adecuado

El estado debe mantenerse en el propietario común más bajo que lo necesite.

```kotlin
@Composable
fun Screen() {
    Header()
    SearchSection()
    HugeContent()
}

@Composable
fun SearchSection() {
    var query by remember { mutableStateOf("") }
    SearchField(query, onQueryChange = { query = it })
}
```

Si `query` solo es necesario para la sección de búsqueda, no debe leerse en el
nivel de toda la pantalla.

### Reducir el alcance de las lecturas

Dividir el código en funciones no garantiza por sí mismo una optimización. Un
composable debe leer únicamente el estado necesario y recibir parámetros
estables; de este modo, Compose puede omitir los grupos que no han cambiado.

### Modelos inmutables y estables

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

`@Immutable` solo debe utilizarse cuando el contrato se cumple realmente. Las
propiedades mutables y las modificaciones in-place reducen la previsibilidad.

El estado debe actualizarse mediante una instantánea inmutable:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

### Listas lazy

Los elementos lazy necesitan una `key` estable:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { user -> user.id }
    ) { user ->
        UserItem(user)
    }
}
```

La `key` conserva el estado recordado durante inserciones, eliminaciones y
reordenaciones. En listas con varios tipos también debe especificarse
`contentType`.

### Evitar tareas pesadas durante la composición

Las transformaciones deben trasladarse a un contenedor de estado o almacenarse
en caché según sus entradas:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

No deben realizarse E/S, decodificación de bitmaps ni ordenaciones o filtrados
pesados sin caché directamente en el cuerpo de un composable.

### derivedStateOf

`derivedStateOf` resulta útil cuando la fuente cambia con frecuencia, pero la
interfaz solo reacciona a una condición derivada:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

### Perfilado

La optimización debe confirmarse mediante herramientas:

- contadores de recomposición;
- Layout Inspector;
- informes del compilador de Compose;
- System Trace o Perfetto;
- Android Studio Profiler;
- Macrobenchmark en una compilación similar a producción.

Una cantidad elevada de recomposiciones no siempre representa un problema si el
composable es barato.

**En resumen:** la recomposición se optimiza mediante lecturas de estado
limitadas, modelos inmutables y estables, actualizaciones inmutables, claves
estables en listas lazy, `remember` o `derivedStateOf` y trasladando las tareas
pesadas fuera de la composición. El resultado debe medirse con un profiler, no
estimarse visualmente.

</details>

<details>

<summary>213. ¿Cómo se escribe una prueba asíncrona sin utilizar corrutinas?</summary>

#### Kotlin

Una prueba asíncrona sin corrutinas debe esperar un evento concreto: un callback,
un resultado, una actualización de estado o un estado inactivo. La espera siempre
debe tener un tiempo límite. `Thread.sleep()` es una mala opción.

### CountDownLatch

Para una API basada en callbacks:

```kotlin
@Test
fun `loads user successfully`() {
    val latch = CountDownLatch(1)
    val result = AtomicReference<User?>()
    val error = AtomicReference<Throwable?>()

    repository.loadUser("42", object : UserCallback {
        override fun onSuccess(user: User) {
            result.set(user)
            latch.countDown()
        }

        override fun onError(throwable: Throwable) {
            error.set(throwable)
            latch.countDown()
        }
    })

    assertTrue(latch.await(2, TimeUnit.SECONDS))
    assertNull(error.get())
    assertEquals("42", result.get()?.id)
}
```

### CompletableFuture

Para un único resultado o error:

```kotlin
val future = CompletableFuture<User>()

repository.loadUser("42", object : UserCallback {
    override fun onSuccess(user: User) = future.complete(user)
    override fun onError(error: Throwable) = future.completeExceptionally(error)
})

assertEquals("42", future.get(2, TimeUnit.SECONDS).id)
```

### Espera de un estado

Puede utilizarse Awaitility o una función auxiliar propia de polling:

```kotlin
@Test
fun `updates cache`() {
    repository.refresh()

    await()
        .atMost(2, TimeUnit.SECONDS)
        .untilAsserted {
            assertEquals(1, cache.size)
        }
}
```

La prueba finaliza inmediatamente cuando se cumple la condición, en lugar de
esperar un retraso fijo.

### LiveData

Para `LiveData` se utiliza `InstantTaskExecutorRule` y una función auxiliar como
`getOrAwaitValue()`:

```kotlin
@get:Rule
val instantTaskExecutorRule = InstantTaskExecutorRule()

@Test
fun `emits user`() {
    val user = viewModel.user.getOrAwaitValue()
    assertEquals("42", user.id)
}
```

La función auxiliar debe suscribirse, esperar un valor con tiempo límite y
eliminar el observer dentro de `finally`.

### Pruebas de interfaz

Espresso se sincroniza mediante `IdlingResource`:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

### Dependencia falsa

La prueba unitaria más estable utiliza una implementación falsa controlada:

```kotlin
class FakeUserApi : UserApi {
    var callback: UserCallback? = null

    override fun loadUser(id: String, callback: UserCallback) {
        this.callback = callback
    }

    fun complete(user: User) {
        callback?.onSuccess(user)
    }
}
```

La propia prueba invoca `complete()`, por lo que no depende del tiempo de la red
ni del planificador de subprocesos.

Antipatrón:

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

Un retraso breve produce una prueba inestable; uno prolongado desperdicia tiempo.

**En resumen:** una prueba asíncrona sin corrutinas debe esperar un evento
concreto con tiempo límite. Para ello se utilizan `CountDownLatch`,
`CompletableFuture`, polling o Awaitility, una función auxiliar de LiveData,
`IdlingResource` de Espresso o una implementación falsa controlada, pero no
`Thread.sleep()`.

</details>

<details>

<summary>214. ¿Cuál es la diferencia entre inline, noinline y crossinline?</summary>

#### Kotlin

`inline`, `noinline` y `crossinline` son modificadores para funciones de orden
superior. Controlan cómo trabaja el compilador con los parámetros lambda.

```text
inline      -> insertar la función o lambda en el lugar de la llamada
noinline    -> no insertar una lambda concreta
crossinline -> insertar la lambda, pero prohibir un retorno no local
```

### inline

`inline` solicita al compilador que inserte el cuerpo de la función y la lambda
en el lugar de la llamada:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Esto reduce la sobrecarga de crear un objeto lambda y de invocar una función en
funciones de orden superior pequeñas.

`inline` también es necesario para genéricos `reified`:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T =
    fromJson(json, T::class.java)
```

Sin `inline` no puede utilizarse un parámetro de tipo `reified`.

### Retorno no local

Dentro de una lambda inline puede utilizarse `return` para salir de la función
exterior:

```kotlin
inline fun runBlock(block: () -> Unit) = block()

fun test() {
    runBlock {
        return // sale de test()
    }
}
```

Esto se denomina retorno no local.

### noinline

`noinline` desactiva la inserción para una lambda concreta:

```kotlin
inline fun execute(
    block: () -> Unit,
    noinline onError: (Throwable) -> Unit
) {
    try {
        block()
    } catch (e: Throwable) {
        onError(e)
    }
}
```

`noinline` se necesita cuando la lambda debe almacenarse en una variable,
transmitirse a otra función o utilizarse como un objeto.

### crossinline

`crossinline` conserva la inserción, pero prohíbe un retorno no local:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

Se necesita cuando la lambda se ejecuta en otro contexto, por ejemplo, dentro de
un `Runnable`, un callback u otra lambda.

Ejemplo combinado:

```kotlin
inline fun example(
    inlined: () -> Unit,
    noinline stored: () -> Unit,
    crossinline safe: () -> Unit
) {
    inlined()
    val lambda = stored
    Runnable { safe() }.run()
}
```

Significado:

- `inlined`: se inserta por completo;
- `stored`: puede almacenarse o transmitirse;
- `safe`: se inserta, pero no permite un retorno no local.

`inline` no debe aplicarse indiscriminadamente. En funciones grandes o con muchos
lugares de llamada puede aumentar el tamaño del bytecode. Resulta especialmente
apropiado para pequeñas utilidades de orden superior, DSL y genéricos `reified`.

**En resumen:** `inline` inserta la función o lambda y es necesario para
`reified`; `noinline` conserva el objeto lambda para poder almacenarlo o
transmitirlo; `crossinline` prohíbe retornos no locales cuando la lambda se
ejecuta en otro contexto.

</details>

<details>

<summary>215. ¿Qué ventajas ofrece Kotlin frente a Java?</summary>

#### Kotlin

Kotlin ofrece ventajas prácticas frente a Java, especialmente en Android:
seguridad frente a valores nulos, menos código repetitivo, corrutinas, funciones
de extensión, clases de datos y selladas, conversiones inteligentes y una sólida
integración con Jetpack.

### Seguridad frente a valores nulos

Kotlin distingue entre tipos nullable y no nulos:

```kotlin
val name: String = "Alex"
val nickname: String? = null
```

Los valores nullable deben tratarse explícitamente:

```kotlin
val title = user.name ?: "Unknown"
```

Esto no elimina todas las `NullPointerException`, pero reduce considerablemente
su cantidad.

### Menos código repetitivo

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Una `data class` genera `equals`, `hashCode`, `toString`, `copy` y las funciones
de componentes.

### Corrutinas

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = State.Success(user)
}
```

Las corrutinas hacen que el código asíncrono sea más legible que los callbacks y
facilitan la concurrencia estructurada.

### Funciones de extensión

```kotlin
fun View.show() {
    visibility = View.VISIBLE
}
```

Permiten añadir API auxiliares sin herencia ni clases wrapper.

### Clases e interfaces selladas

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}
```

Resultan apropiadas para estados de interfaz, resultados, efectos y modelos de
estado finitos.

### Conversiones inteligentes

```kotlin
if (state is UiState.Content) {
    Text(state.user.name)
}
```

Después de comprobar el tipo, Kotlin realiza automáticamente la conversión.

### Argumentos con nombre y predeterminados

```kotlin
fun createUser(name: String, active: Boolean = true)

createUser(name = "Alex")
```

Esto reduce la cantidad de sobrecargas y mejora la legibilidad de los lugares de
llamada.

### Interoperabilidad con Java

Kotlin funciona bien con código y bibliotecas Java, por lo que un proyecto puede
migrarse gradualmente.

### Ecosistema Android

Muchas API modernas de Android están diseñadas primero para Kotlin:

- Jetpack Compose;
- Coroutines y Flow;
- extensiones KTX;
- API suspendibles y Flow de Room;
- ámbitos de Lifecycle;
- DSL de Navigation para Kotlin.

También existen compromisos:

- el tiempo de compilación puede ser mayor;
- las corrutinas requieren una comprensión profunda;
- la interoperabilidad con Java introduce platform types;
- existen casos límite complejos con genéricos e inferencia de tipos;
- abusar de DSL y extensiones reduce la legibilidad.

**En resumen:** Kotlin permite escribir código más seguro y expresivo mediante
seguridad frente a nulos, clases de datos y selladas, corrutinas, funciones de
extensión, conversiones inteligentes, argumentos con nombre y predeterminados y
mejores herramientas para Android. Sin embargo, la calidad depende de la
arquitectura y la disciplina, no únicamente del lenguaje.

</details>

<details>

<summary>216. ¿Cuál es la diferencia entre val y const val?</summary>

#### Kotlin

`val` y `const val` representan valores de solo lectura, pero son conceptos
distintos.

```text
val       -> propiedad de solo lectura cuyo valor puede calcularse durante la ejecución
const val -> constante conocida durante la compilación
```

`val`:

```kotlin
val name = "Alex"
val createdAt = System.currentTimeMillis()
```

Un `val` no puede reasignarse después de inicializarse, pero su valor puede
calcularse durante la ejecución.

`const val`:

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

El valor de un `const val` debe ser conocido por el compilador y se inserta
directamente en los lugares donde se utiliza.

`const val` puede declararse:

- en el nivel superior;
- dentro de un `object`;
- dentro de un `companion object`.

```kotlin
object ApiConfig {
    const val BASE_PATH = "api/v1"
}
```

No puede declararse como variable local dentro de una función.

Los tipos admitidos para `const val` se limitan a literales conocidos durante la
compilación:

- `String`;
- tipos primitivos como `Int`, `Long`, `Boolean` y `Double`.

No se permite:

```kotlin
const val DATE = LocalDate.now() // error
```

`const val` se necesita cuando una API exige una constante conocida durante la
compilación, por ejemplo, en anotaciones:

```kotlin
const val TABLE_USERS = "users"

@Entity(tableName = TABLE_USERS)
data class UserEntity(...)
```

Un `val` convencional no sirve si la anotación exige una constante.

En la interoperabilidad con Java, `const val` se compila como un campo `static
final` y puede utilizarse desde Java como una constante.

Riesgos:

- `const val` se inserta en los lugares de uso, por lo que, después de cambiar su
  valor en una biblioteca, los módulos dependientes pueden necesitar una nueva
  compilación;
- `const val` no protege secretos ni claves de API: el valor puede extraerse
  fácilmente del APK;
- no todo valor que simplemente «no cambia» debe declararse `const val`.

Elección práctica:

- valor u objeto calculado durante la ejecución: `val`;
- constante literal conocida durante la compilación: `const val`;
- valor utilizado en una anotación: `const val`;
- secretos: no utilizar `const val`, sino una estrategia adecuada de seguridad y
  configuración.

**En resumen:** `val` es una propiedad de solo lectura cuyo valor puede
calcularse durante la ejecución. `const val` es una constante de `String` o de un
tipo primitivo conocida durante la compilación, que puede declararse en el nivel
superior o dentro de un `object` o `companion object`, y resulta apropiada para
anotaciones. Los secretos no deben almacenarse en un `const val`.

</details>

<details>

<summary>217. ¿Qué tipos pueden utilizarse con const val?</summary>

#### Kotlin

`const val` representa una constante conocida durante la compilación. Su tipo
debe ser `String` o un tipo primitivo, y el inicializador debe ser conocido por el
compilador sin ejecutar el programa.

Tipos admitidos:

```kotlin
const val STRING_VALUE: String = "user_id"
const val INT_VALUE: Int = 10
const val LONG_VALUE: Long = 10L
const val DOUBLE_VALUE: Double = 3.14
const val FLOAT_VALUE: Float = 3.14f
const val BOOLEAN_VALUE: Boolean = true
const val CHAR_VALUE: Char = 'A'
const val BYTE_VALUE: Byte = 1
const val SHORT_VALUE: Short = 2
```

Ejemplos habituales:

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Puede declararse:

- en el nivel superior;
- dentro de un `object`;
- dentro de un `companion object`.

```kotlin
const val API_VERSION = "v1"

object ApiConstants {
    const val BASE_PATH = "/api"
}

class UserFragment {
    companion object {
        const val ARG_USER_ID = "user_id"
    }
}
```

`const val` no puede declararse como variable local ni como propiedad de una
instancia.

No se permite:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val UUID_VALUE = UUID.randomUUID()
const val DEFAULT_THEME = ThemeMode.Light
const val NAME: String? = null
```

Para objetos, colecciones, valores nullable y expresiones evaluadas durante la
ejecución debe utilizarse un `val` convencional:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

El inicializador puede contener literales, otras constantes y operaciones
evaluables durante la compilación:

```kotlin
const val HOST = "example.com"
const val API_URL = "https://" + HOST
const val TIMEOUT_SECONDS = 5 * 2
```

Las llamadas a funciones, un `Context`, la hora actual, valores aleatorios o la
creación de objetos no son constantes conocidas durante la compilación.

`const val` suele necesitarse para argumentos de anotaciones:

```kotlin
const val USERS_TABLE = "users"

@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

**En resumen:** `const val` solo admite `String` y tipos primitivos con un
inicializador conocido durante la compilación. Para objetos, colecciones, valores
nullable o calculados durante la ejecución debe utilizarse un `val` convencional.

</details>

<details>

<summary>218. ¿Se puede crear una data class sin parámetros?</summary>

#### Kotlin

No. Una `data class` debe tener un constructor primario con al menos un parámetro,
y ese parámetro debe declararse como `val` o `var`.

Incorrecto:

```kotlin
data class Empty
data class Empty()
data class User(id: String)
```

Correcto:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

La razón es que el compilador genera los métodos a partir de las propiedades del
constructor primario:

```text
equals()
hashCode()
toString()
copy()
componentN()
```

Una propiedad declarada dentro del cuerpo de la clase no participa en la
igualdad, el hash, la copia ni la desestructuración generados:

```kotlin
data class User(
    val id: String
) {
    var cachedLabel: String = ""
}
```

Dos objetos `User` con el mismo `id`, pero distintos valores de `cachedLabel`,
serán iguales. Por ello, los campos que definen el valor deben declararse en el
constructor primario.

Es posible invocar el constructor sin argumentos mediante valores
predeterminados:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)

val user = User()
```

La clase sigue teniendo parámetros, pero el código llamador utiliza sus valores
predeterminados. Sin embargo, estos valores no deben crear un objeto de dominio
no válido únicamente para satisfacer a un framework.

Si un framework exige un constructor sin argumentos, es preferible utilizar:

- un modelo independiente de persistencia o DTO;
- un adapter o mapper;
- el complemento no-arg de Kotlin;
- valores predeterminados válidos, si realmente tienen sentido.

Si un estado no contiene datos, es preferible utilizar un singleton:

```kotlin
data object Loading
```

Dentro de una jerarquía sellada:

```kotlin
sealed interface ScreenState {
    data object Loading : ScreenState

    data class Content(
        val items: List<Item>
    ) : ScreenState

    data class Error(
        val message: String
    ) : ScreenState
}
```

Otras restricciones: una `data class` no puede ser `abstract`, `open`, `sealed`
ni `inner`, aunque puede implementar interfaces.

Elección práctica:

- modelo de valor: `data class`;
- llamada sin argumentos: valores predeterminados;
- estado sin datos: `object` o `data object`;
- constructor sin argumentos exigido por un framework: adapter o complemento;
- campos que definen el valor: únicamente en el constructor primario.

**En resumen:** una `data class` vacía no compila. Necesita al menos un parámetro
`val` o `var` en el constructor primario. Para una llamada sin argumentos se
utilizan valores predeterminados; para un estado sin datos, `object` o `data
object`.

</details>

<details>

<summary>219. ¿Cuál es la diferencia entre sealed class y enum?</summary>

#### Kotlin

`enum` y `sealed` limitan el conjunto de variantes, pero modelan conceptos
distintos.

```text
enum   -> conjunto fijo de entradas singleton del mismo tipo
sealed -> jerarquía cerrada de subtipos diferentes
```

### Enum

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

Un `enum` resulta apropiado cuando todos los casos tienen la misma estructura y
no necesitan datos diferentes. Cada entrada es un singleton:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Un enum puede tener propiedades comunes:

```kotlin
enum class HttpMethod(
    val allowsBody: Boolean
) {
    GET(false),
    POST(true),
    PUT(true),
    DELETE(false)
}
```

Todas las entradas comparten el mismo contrato de propiedades. Un enum dispone
de API integradas como `entries`, `valueOf()`, `name` y `ordinal`.

`ordinal` no debe almacenarse en una base de datos ni enviarse mediante una API,
ya que el orden de las entradas puede cambiar. Para la persistencia es preferible
utilizar un código estable explícito.

### Sealed

```kotlin
sealed interface UiState {
    data object Loading : UiState

    data class Content(
        val user: User
    ) : UiState

    data class Error(
        val message: String
    ) : UiState
}
```

`sealed` resulta apropiado cuando las variantes tienen datos o comportamientos
distintos. `Loading` es un singleton, `Content` contiene datos y `Error` posee
otro modelo.

Casos de uso habituales:

- estado de la interfaz;
- resultado de dominio;
- errores;
- eventos;
- máquinas de estados finitos.

### when exhaustivo

Enum:

```kotlin
val title = when (mode) {
    ThemeMode.Light -> "Light"
    ThemeMode.Dark -> "Dark"
    ThemeMode.System -> "System"
}
```

Sealed:

```kotlin
when (state) {
    UiState.Loading -> showLoading()
    is UiState.Content -> showUser(state.user)
    is UiState.Error -> showError(state.message)
}
```

El compilador conoce todos los casos y puede comprobar que `when` sea
exhaustivo. Un `else` innecesario suele ocultar la utilidad de esta comprobación.

### Sealed class frente a sealed interface

Una `sealed class` puede tener estado en el constructor y miembros protegidos:

```kotlin
sealed class AppError(
    open val code: Int
)
```

Una `sealed interface` no contiene estado en un constructor, pero su
implementación puede implementar varias interfaces y heredar de otra clase. Para
modelos de estado o resultado suele ser suficiente una `sealed interface`.

### Serialización

Un enum suele serializarse como un valor textual o un código estable. Una
jerarquía sellada necesita un discriminador para el subtipo y un esquema para sus
datos. Deben tenerse en cuenta las versiones y las variantes desconocidas.

Elección práctica:

- modo o estado fijo y sencillo: `enum`;
- casos sin datos diferentes: `enum`;
- estados de interfaz, resultados o errores con campos distintos: `sealed`;
- combinación de casos `object` y `data class` en un mismo modelo: `sealed`;
- no sustituir un enum por un tipo sellado «por si acaso» sin una necesidad real.

**En resumen:** `enum` es una lista fija de entradas singleton con una estructura
común. `sealed class` o `sealed interface` es una jerarquía cerrada donde cada
caso puede contener sus propios datos. Para modos sencillos debe utilizarse
`enum`; para modelos de estado o resultado, `sealed`.

</details>

<details>

<summary>220. ¿Qué es WorkManager?</summary>

#### Kotlin

`WorkManager` es una API de Jetpack para ejecutar tareas aplazadas y garantizadas
en segundo plano. Se utiliza para tareas que deben completarse incluso después de
cerrar la aplicación o finalizar su proceso, pero que no tienen que ejecutarse
inmediatamente.

Resulta apropiado para:

- sincronización en segundo plano;
- carga o descarga con reintentos;
- limpieza de datos antiguos;
- envío de registros o analíticas;
- sincronización periódica;
- tareas con restricciones de red, carga o nivel de batería.

Tarea de una sola ejecución:

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .build()

WorkManager.getInstance(context).enqueue(request)
```

`CoroutineWorker`:

```kotlin
class SyncWorker(
    context: Context,
    params: WorkerParameters,
    private val repository: SyncRepository
) : CoroutineWorker(context, params) {

    override suspend fun doWork(): Result {
        return try {
            repository.sync()
            Result.success()
        } catch (e: IOException) {
            Result.retry()
        } catch (e: Exception) {
            Result.failure()
        }
    }
}
```

Resultados:

```text
Result.success() -> tarea completada
Result.retry()   -> volver a intentarlo más adelante
Result.failure() -> finalizar con un error
```

Restricciones:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()
```

WorkManager solo ejecutará la tarea cuando se cumplan las restricciones.

Trabajo periódico:

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

El trabajo periódico no garantiza una hora exacta de ejecución: Android elige el
momento teniendo en cuenta la batería y las políticas del sistema.

Para evitar duplicados se utiliza trabajo único:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` conserva la tarea actual; `REPLACE` cancela la anterior y programa una
nueva.

Los datos de entrada deben ser pequeños:

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Los objetos grandes deben almacenarse en una base de datos y transferirse
únicamente mediante un identificador.

Encadenamiento:

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

WorkManager no resulta apropiado para:

- alarmas exactas;
- conexiones permanentes mediante sockets;
- reproducción multimedia;
- seguimiento en tiempo real;
- tareas que deben comenzar inmediatamente;
- escenarios prolongados y visibles para el usuario en primer plano sin
  interrupciones.

**En resumen:** `WorkManager` se utiliza para tareas aplazables y garantizadas en
segundo plano, con restricciones, reintentos, trabajo periódico o único y
encadenamiento. Para tiempo real, alarmas exactas, reproducción multimedia o
tareas inmediatas en primer plano se necesitan otras API.

</details>

<details>

<summary>221. ¿Cuándo conviene utilizar WorkManager?</summary>

#### Kotlin

`WorkManager` debe utilizarse para tareas persistentes que pueden aplazarse: la
tarea no tiene que ejecutarse inmediatamente, pero debe completarse de forma
fiable después de cerrar la interfaz, finalizar el proceso o reiniciar el
dispositivo, respetando determinadas restricciones.

Resulta apropiado para:

- sincronización en segundo plano;
- cargas o descargas con reintentos;
- limpieza de datos antiguos;
- envío de registros o analíticas;
- actualización periódica;
- tareas con restricciones de red, carga o batería.

No garantiza una hora exacta. Una detención forzada o la desinstalación
interrumpen el trabajo.

Tarea de una sola ejecución:

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .setConstraints(
        Constraints.Builder()
            .setRequiredNetworkType(NetworkType.CONNECTED)
            .build()
    )
    .build()

WorkManager.getInstance(context).enqueue(request)
```

Ejemplo de `CoroutineWorker`:

```kotlin
@HiltWorker
class SyncWorker @AssistedInject constructor(
    @Assisted context: Context,
    @Assisted params: WorkerParameters,
    private val repository: SyncRepository
) : CoroutineWorker(context, params) {

    override suspend fun doWork(): Result =
        try {
            repository.sync()
            Result.success()
        } catch (exception: CancellationException) {
            throw exception
        } catch (exception: IOException) {
            Result.retry()
        } catch (exception: PermanentSyncException) {
            Result.failure()
        }
}
```

Resultados:

```text
success() -> tarea completada
retry()   -> fallo transitorio
failure() -> fallo permanente
```

El Worker debe ser idempotente, ya que el sistema puede detenerlo e iniciarlo de
nuevo. `CancellationException` no debe transformarse en un reintento.

Las restricciones describen las condiciones de ejecución, no una hora exacta:

- disponibilidad de red;
- dispositivo cargándose;
- batería no baja;
- almacenamiento no bajo.

El trabajo único evita tareas duplicadas:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "user-sync",
        ExistingWorkPolicy.KEEP,
        request
    )
```

El trabajo periódico se ejecuta de manera aproximada: Doze, las políticas de
batería y las restricciones pueden retrasar su inicio.

Los datos de entrada de tipo `Data` deben ser pequeños y contener tipos
primitivos o `String`. Los objetos grandes deben almacenarse en Room o en un
archivo y proporcionar al Worker únicamente un identificador.

Cuándo no resulta apropiado:

```text
tarea vinculada a una pantalla -> ámbito de ViewModel o Lifecycle
alarma exacta                  -> AlarmManager
multimedia o seguimiento       -> Foreground Service
socket en tiempo real          -> arquitectura activa durante la ejecución
```

`ExpeditedWorkRequest` está sujeto a cuotas y no convierte WorkManager en una API
de tiempo real.

**En resumen:** WorkManager se necesita para tareas idempotentes en segundo plano
que pueden aplazarse, pero deben ejecutarse de forma fiable con restricciones y
reintentos. Para tareas vinculadas a la interfaz, tiempo real, multimedia,
seguimiento o ejecución a una hora exacta deben utilizarse otras API.

</details>

<details>

<summary>222. ¿Ha trabajado con Android Media3 / ExoPlayer?</summary>

#### Kotlin

Sí. `Media3` es la pila multimedia moderna de Jetpack y `ExoPlayer` es la
implementación predeterminada de la interfaz `Player`. Admite contenido local y
por streaming, listas de reproducción, streaming adaptativo, subtítulos y DRM.

Uso básico:

```kotlin
val player = ExoPlayer.Builder(context).build()

player.setMediaItem(MediaItem.fromUri(videoUrl))
player.prepare()
player.play()
```

En una interfaz basada en vistas:

```kotlin
playerView.player = player
```

Lo principal es definir correctamente el propietario del reproductor y garantizar
la llamada a `release()`.

```kotlin
override fun onDestroyView() {
    playerView.player = null
    player.release()
    super.onDestroyView()
}
```

Una referencia a `PlayerView` no debe sobrevivir a `onDestroyView()`.

En Compose, el `PlayerView` heredado puede envolverse mediante `AndroidView`:

```kotlin
AndroidView(
    factory = { context ->
        PlayerView(context).apply {
            player = exoPlayer
        }
    }
)
```

El reproductor no debe crearse en cada recomposición. Necesita un propietario
estable y limpieza mediante `DisposableEffect`, un holder o un servicio.

Lista de reproducción:

```kotlin
val items = videos.map { video ->
    MediaItem.Builder()
        .setUri(video.url)
        .setMediaId(video.id)
        .build()
}

player.setMediaItems(items)
player.prepare()
```

`Player.Listener` proporciona estados de reproducción y errores:

- `BUFFERING`;
- `READY`;
- `ENDED`;
- error de reproducción;
- transición entre elementos multimedia.

La interfaz transforma estos eventos en estados de carga, error o repetición. Es
preferible no mezclar las analíticas con la lógica de renderizado.

Para reproducción en segundo plano, música o podcasts, el reproductor no debe
pertenecer a una `Activity` o un `Fragment`. La estructura correcta es:

```text
MediaSessionService posee Player + MediaSession
La interfaz se conecta mediante MediaController
```

Esto proporciona:

- notificación multimedia;
- controles del sistema;
- integración con Bluetooth, Wear OS y Android Auto;
- independencia entre la reproducción y el ciclo de vida de la pantalla.

Para streaming y caché se utiliza `CacheDataSource`, definiendo explícitamente el
tamaño de la caché, la estrategia de expulsión, el comportamiento sin conexión y
las restricciones de DRM.

Errores habituales:

- no invocar `release()`;
- crear el reproductor durante la recomposición;
- conservar `PlayerView` después de `onDestroyView()`;
- realizar reproducción en segundo plano dentro del ciclo de vida de una
  pantalla;
- no tratar los errores o el foco de audio;
- invocar el reproductor fuera de su subproceso de aplicación.

**En resumen:** Media3 y ExoPlayer requieren un propietario claramente definido,
limpieza según el ciclo de vida, llamada a `release()`, tratamiento de estados y
errores y acceso desde el subproceso correcto. Para reproducción en segundo plano
se utiliza `MediaSessionService`, no una `Activity` o un `Fragment`.

</details>

<details>

<summary>223. ¿Cómo distribuiría los componentes entre módulos si existe un botón que carga datos al pulsarlo?</summary>

#### Kotlin

Los módulos deben organizarse alrededor de una funcionalidad o un caso de uso,
no alrededor de un botón.

```text
Botón -> ViewModel -> Caso de uso -> Repositorio -> API/BD
```

La interfaz solo envía un evento y renderiza el estado. Desconoce si los datos
proceden de la red, Room o una caché.

Para una aplicación pequeña basta con la siguiente estructura:

```text
:app
:core:network
:core:database
:feature:profile
```

Dentro de `:feature:profile` pueden existir los siguientes paquetes:

```text
presentation
domain
data
```

No se necesitan módulos Gradle independientes para cada capa si no existe una
necesidad real de aislamiento o responsabilidad independiente.

Una funcionalidad grande puede dividirse entre API e implementación:

```text
:feature:profile:api
:feature:profile:impl
```

`api` contiene contratos públicos, como navegación, capacidades e interfaces.
`impl` contiene la pantalla, el ViewModel, los casos de uso y la implementación
del repositorio.

### Presentación

```kotlin
@Composable
fun ProfileScreen(
    state: ProfileState,
    onLoadClick: () -> Unit
) {
    Button(onClick = onLoadClick) {
        Text("Load profile")
    }
}
```

El ViewModel:

- recibe el evento del clic;
- inicia una corrutina;
- administra los estados de carga, contenido y error;
- trata los clics duplicados;
- conserva el estado en un `StateFlow`.

Retrofit, SQL y la lógica de caché no deben encontrarse en la capa de
presentación.

### Dominio

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile =
        repository.loadProfile()
}

interface ProfileRepository {
    suspend fun loadProfile(): Profile
}
```

Un caso de uso resulta necesario cuando existen reglas de negocio, orquestación o
reutilización. Si solo es un proxy sencillo dentro de una funcionalidad pequeña,
el ViewModel puede depender directamente del contrato del repositorio.

El dominio no depende de la interfaz de Android, Retrofit, Room ni un framework
de DI.

### Datos

```kotlin
class ProfileRepositoryImpl(
    private val api: ProfileApi,
    private val dao: ProfileDao
) : ProfileRepository {

    override suspend fun loadProfile(): Profile {
        val dto = api.getProfile()
        dao.upsert(dto.toEntity())
        return dto.toDomain()
    }
}
```

La capa de datos se encarga de la API, la base de datos, la caché o fuente de
verdad, el mapeo y la transformación de errores de infraestructura.

Dirección de las dependencias:

```text
app -> implementación de la funcionalidad
presentación -> contrato de dominio
datos -> contrato de dominio
core -X-> funcionalidad
```

La inyección de dependencias conecta la implementación con el contrato en la raíz
de composición:

```kotlin
@Binds
abstract fun bindRepository(
    impl: ProfileRepositoryImpl
): ProfileRepository
```

Criterios para crear un módulo Gradle independiente:

- responsabilidad independiente;
- API pública estable;
- aislamiento de dependencias;
- capacidad reutilizable;
- mejora del tiempo de compilación.

Si no existe ninguna de estas razones, los paquetes son una solución más
sencilla. Una modularización excesiva añade código repetitivo de Gradle, DI y
navegación sin aportar una ventaja real.

**En resumen:** el botón y el estado pertenecen a `presentation`, la operación de
negocio a `domain` o a un caso de uso, y la API, la base de datos y la
implementación del repositorio a `data`. En una aplicación pequeña bastan
paquetes; los módulos Gradle solo deben añadirse por motivos de aislamiento,
responsabilidad o rendimiento de compilación.

</details>
