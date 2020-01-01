**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Most Popular Kotlin Interview Questions and Answers</h2>

<details>
<summary>1. What is Kotlin, and how does it interoperate with Java?</summary>

#### Kotlin

Kotlin is a statically typed programming language developed by JetBrains. It
supports the JVM, Android, JavaScript, WebAssembly, and native platforms.

On the JVM, Kotlin compiles to JVM bytecode and is therefore fully interoperable
with Java:

- Kotlin can call Java code directly and use Java libraries.
- Java can call Kotlin code as regular JVM classes and methods.
- Both languages can be used within the same project and module.
- The `@JvmStatic`, `@JvmOverloads`, `@JvmField`, and `@JvmName` annotations help
  create Java-friendly APIs.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

An important nuance is platform types: if a Java API does not provide proper
nullability annotations, Kotlin cannot always determine whether `null` is
allowed. Explicit contracts are therefore necessary at Java–Kotlin boundaries.

**In short:** Kotlin is interoperable with Java at the JVM level and can be
introduced gradually into an existing Java project without rewriting it
entirely.

</details>

<details>
<summary>2. What basic data types are available in Kotlin?</summary>

#### Kotlin

Kotlin's main types are:

- integer types: `Byte`, `Short`, `Int`, and `Long`;
- unsigned integer types: `UByte`, `UShort`, `UInt`, and `ULong`;
- floating-point types: `Float` and `Double`;
- the Boolean type: `Boolean`;
- characters and strings: `Char` and `String`;
- arrays: `Array<T>`, `IntArray`, `LongArray`, and others.

The following types are also important parts of the type system:

- `Any` is the base type for all non-null types;
- `Unit` indicates that a function does not return a meaningful result;
- `Nothing` indicates that a function never completes normally;
- nullable types are marked with `?`, for example, `Int?`.

On the JVM, types such as `Int` may compile to Java primitives. Nullable types
and generics usually require boxing.

- Kotlin does not perform implicit numeric widening: an `Int` is not converted
  to a `Long` automatically:

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**In short:** Kotlin does not use separate syntax for primitive and object
types, but the JVM compiler uses primitives wherever possible.

</details>

<details>
<summary>3. What is the difference between val and var in Kotlin?</summary>

#### Kotlin

`val` prevents reassignment, whereas `var` allows it:

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Compilation error
```

`val` does not make the object itself immutable—it only makes the reference
read-only:

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Allowed
```

For class properties, `val` has only a getter, whereas `var` has both a getter
and a setter.

**In short:** use `val` by default and use `var` only when the value genuinely
needs to change.

</details>

<details>
<summary>4. What is type inference in Kotlin?</summary>

#### Kotlin

Type inference is the compiler's ability to determine a type from an expression
or its context without an explicit type declaration. Kotlin remains a statically
typed language.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it is a String
```

An explicit type should be specified when it:

- is part of a public API;
- improves the readability of a complex expression;
- should differ from the concrete implementation type.

```kotlin
val users: List<User> = mutableListOf()
```

**In short:** the compiler automatically infers obvious types; explicit types
are needed for contracts and readability.

</details>

<details>
<summary>5. What are extension functions?</summary>

#### Kotlin

An extension function provides convenient call syntax for an existing type
without modifying the class itself or using inheritance.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` is the receiver type. Inside the function, the receiver object is
available through `this`.

Important limitations:

- an extension cannot access the class's `private` or `protected` members;
- a class member takes precedence over an extension with the same signature;
- an extension is resolved statically based on the variable's declared type.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

On the JVM, such an extension is usually compiled into a static function that
receives the object as its first argument.

**In short:** extension functions improve API readability, but they do not add
actual methods to a class and do not support polymorphic overriding.

</details>

<details>
<summary>6. What is a data class in Kotlin?</summary>

#### Kotlin

A `data class` is intended for models whose primary purpose is to hold data.
The compiler generates:

- `equals()` and `hashCode()`;
- `toString()`;
- `copy()`;
- `componentN()` functions for destructuring.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Important rules:

- the primary constructor must contain at least one `val` or `var` parameter;
- a `data class` cannot be `abstract`, `open`, `sealed`, or `inner`;
- only properties declared in the primary constructor participate in the
  generated methods;
- `copy()` performs a shallow copy, not a deep copy.

A property declared in the class body is not included in `equals()`,
`hashCode()`, or `copy()`:

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**In short:** a `data class` reduces boilerplate for DTOs, UI state, and value
objects. Complex business logic is better kept in domain services or regular
classes.

</details>

<details>
<summary>7. What is a companion object?</summary>

#### Kotlin

A `companion object` is a single object associated with a class. Its members can
be called through the class name:

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

It is not the same as Java's `static`: a companion object has its own type, can
have a name, implement interfaces, and access the class's `private` members. A
class can have only one companion object.

The `@JvmStatic` annotation allows a function to be called conveniently from
Java:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Without the annotation, Java calls `Config.Companion.default()`; with it, Java
calls `Config.default()`.

**In short:** a companion object is suitable for factory methods and constants
associated with a class. It should not be used to store global mutable state.

</details>

<details>
<summary>8. How are classes created in Kotlin?</summary>

#### Kotlin

A class is declared using the `class` keyword. The primary constructor is
written in the class header:

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

- `val` and `var` in the constructor create class properties.
- An `init` block runs when an object is created and is suitable for validating
  invariants.
- Default values eliminate the need for overloaded constructors.
- An object is created without the `new` keyword.

Classes and their methods are `final` by default. The `open` modifier is
required to allow inheritance:

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**In short:** Kotlin lets you declare a constructor, properties, and default
values directly in the class header; inheritance must be enabled explicitly.

</details>

<details>
<summary>9. Explain primary and secondary constructors.</summary>

#### Kotlin

The primary constructor is declared in the class header:

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Its parameters become properties only when declared with `val` or `var`. The
body of the primary constructor is defined using an `init` block:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

A secondary constructor is declared in the class body using `constructor`. If a
primary constructor exists, the secondary constructor must delegate to it using
`this(...)`:

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Properties and `init` blocks are initialized in declaration order, followed by
the body of the secondary constructor.

If the only difference is a default value, a separate constructor is
unnecessary:

```kotlin
class User(val name: String, val id: Long = 0)
```

**In short:** the primary constructor is the standard choice. Secondary
constructors are mainly needed for Java interoperability or framework
requirements; default parameters or a factory method are usually sufficient.

</details>

<details>
<summary>10. How does inheritance work in Kotlin?</summary>

#### Kotlin

Classes and their members are `final` by default in Kotlin. The `open` modifier
is required for inheritance, and `override` is required for overriding:

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

Properties can also be overridden:

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Important details:

- a class can inherit from only one class but can implement multiple
  interfaces;
- `abstract` members are already open and do not require `open`;
- an overridden member remains open unless marked with `final override`;
- the parent implementation is accessed using `super`.

**In short:** inheritance in Kotlin must be enabled explicitly. Interfaces are
preferable for defining contracts, and composition should generally be
considered before creating a deep class hierarchy.

</details>

<details>
<summary>11. How do you implement a singleton in Kotlin?</summary>

#### Kotlin

The standard way to implement a singleton in Kotlin is with an `object`
declaration:

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

On the JVM, the object is created on first access, and its initialization is
thread-safe. An `object` can contain properties, functions, and `init` blocks,
inherit from a class, and implement interfaces. Its constructor cannot be
called.

An `object` is suitable for constants, stateless strategies, and cases within a
sealed hierarchy. For services with dependencies, a singleton scope in a DI
container is preferable.

Mutable state in an `object` is global, complicates testing, and requires
synchronization. On Android, an `Activity` or `View` must not be stored in it,
as doing so may cause a memory leak.

**In short:** an `object` is a simple singleton without constructor parameters.
DI is preferable for business services with dependencies and a managed
lifecycle.

</details>

<details>
<summary>12. What is the Unit type used for?</summary>

#### Kotlin

`Unit` indicates that a function completes without returning a meaningful
result:

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

In a regular function, `: Unit` can be omitted:

```kotlin
fun log(message: String) {
    println(message)
}
```

Unlike Java's `void`, `Unit` is a proper type with a single value, `Unit`. It is
used in function types and generic APIs:

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

It should not be confused with `Nothing`: `Unit` represents normal completion
without a result, whereas `Nothing` means that a function never completes
normally:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**In short:** `Unit` is the type used for functions without a meaningful return
value. It is usually written explicitly in callback types such as `() -> Unit`.

</details>

<details>
<summary>13. What is a smart cast?</summary>

#### Kotlin

A smart cast is an automatic cast to a more specific type when the compiler can
prove that the cast is safe:

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value is a String
    }
}
```

The mechanism works after `is`, `!is`, and null checks while taking control flow
into account:

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value is a String
}
```

A smart cast does not work if the value may change between the check and its
use. A mutable property or custom getter is a typical example:

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

**In short:** smart casts eliminate explicit `as` casts after a type or null
check, but only when the compiler can guarantee that the checked value will not
change.

</details>

<details>
<summary>14. How does Kotlin provide null safety?</summary>

#### Kotlin

In Kotlin, nullable and non-null types are distinguished at the type-system
level:

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

The following operators are used when working with nullable values:

```kotlin
val nullableLength: Int? = optionalName?.length // Safe call
val length: Int = optionalName?.length ?: 0     // Elvis operator
```

After a null check, the compiler can perform a smart cast:

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

The `!!` operator converts a nullable value to a non-null value but throws a
`NullPointerException` if the value is `null`:

```kotlin
val length = optionalName!!.length
```

The main weak point is Java platform types: without nullability annotations, the
compiler does not know whether a Java API can return `null`. Such values should
be checked at the Java–Kotlin boundary.

**In short:** `T` does not allow `null`, whereas `T?` does. Use `?.`, `?:`, and
explicit checks; reserve `!!` for cases with a proven non-null guarantee.

</details>

<details>
<summary>15. What is the Elvis operator (?:)?</summary>

#### Kotlin

The Elvis operator `?:` returns its left operand if it is not `null`; otherwise,
it evaluates and returns its right operand:

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

It is often used together with a safe call:

```kotlin
val nameLength = user?.name?.length ?: 0
```

Because `return` and `throw` are expressions in Kotlin, they can also appear on
the right-hand side:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**In short:** `?:` provides a fallback for `null`. Do not use it to conceal
errors when `null` represents an invalid state; terminate execution explicitly
instead.

</details>

<details>
<summary>16. What does the !! operator do, and why is it dangerous?</summary>

#### Kotlin

The `!!` operator forcibly converts `T?` to `T`. If the value is `null`, a
`NullPointerException` is thrown:

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Safer alternatives depend on the required behavior:

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

`requireNotNull` and `checkNotNull` also terminate execution with an error, but
they explicitly describe the violated contract. Frequent use of `!!` usually
indicates an incorrectly modeled nullable state or lifecycle issues.

**In short:** use `!!` only when a non-null value is guaranteed by a contract
but the compiler cannot prove it. In all other cases, handle `null` explicitly.

</details>

<details>
<summary>17. How do default and named parameters work?</summary>

#### Kotlin

A default parameter value is used when the corresponding argument is omitted:

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Named arguments improve readability, especially when several parameters have
the same type or are `Boolean` values:

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java does not support Kotlin's default parameters or named arguments. For a Java
API, overloads can be generated with `@JvmOverloads`:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

The parameter names of a public function are part of its Kotlin API: changing
them can break calls that use named arguments. If there are too many parameters,
it is better to create a separate configuration class.

**In short:** default values replace most overloads, while named arguments make
calls easier to understand. For Java interoperability, consider
`@JvmOverloads`.

</details>

<details>
<summary>18. What are destructuring declarations?</summary>

#### Kotlin

A destructuring declaration breaks an object down into variables using
`componentN()` functions:

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

The equivalent code is:

```kotlin
val id = user.component1()
val name = user.component2()
```

For a `data class`, these functions are generated according to the order of the
properties in the primary constructor. Destructuring also works in loops and
with `Map.Entry`:

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

An unused component can be skipped with `_`:

```kotlin
val (_, name) = user
```

Custom `operator fun componentN()` functions can be defined for a regular
class. However, destructuring depends on component order and may therefore
reduce readability for large models.

**In short:** destructuring is convenient syntax over `componentN()` for small
models and pairs of values. Component order is part of the contract.

</details>

<details>
<summary>19. What is a type alias, and when should you use one?</summary>

#### Kotlin

A `typealias` provides a shorter or clearer name for an existing type:

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Importantly, an alias does not create a new type or provide additional type
safety:

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Valid because both types are Long
```

If the compiler must distinguish between the values, use a `value class`:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**In short:** use `typealias` to improve the readability of complex generic and
function types. Use a `value class` for a distinct domain type.

</details>

<details>
<summary>20. What is the difference between vararg and an array?</summary>

#### Kotlin

An array is an object with a fixed number of elements. A `vararg` is a function
parameter that accepts a variable number of arguments:

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

Inside the function, `names` is an array. An existing array is passed using the
spread operator `*`:

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

If another parameter follows a `vararg`, it must be passed by name:

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

A function can have only one `vararg`. For `vararg Int`, the compiler uses
`IntArray`. Spreading may create a copy of the array, which matters in
performance-critical code.

**In short:** `Array<T>` is a data structure, whereas `vararg` is a way to accept
an arbitrary number of arguments. An array is passed to a `vararg` parameter as
`*array`.

</details>

<details>
<summary>21. How are lambda expressions used in Kotlin?</summary>

#### Kotlin

A lambda is an anonymous function that can be stored in a variable, passed as an
argument, and returned from a function:

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

The last expression is the lambda's result. If it has one parameter, that
parameter can be referenced as `it`:

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

If a lambda is the last argument, it can be placed outside the parentheses:

```kotlin
button.setOnClickListener { println("Clicked") }
```

A lambda can capture variables from its enclosing scope:

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

If the required function already exists, a `::` reference can be used:

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**In short:** lambdas pass behavior to `map`, `filter`, callbacks, and DSLs.
Long or nested lambdas are better extracted into named functions, and captured
mutable state should be controlled, especially in concurrent code.

</details>

<details>
<summary>22. How do collections (List, Set, and Map) work in Kotlin?</summary>

#### Kotlin

Kotlin's main collection types are:

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- `List` preserves order, allows duplicates, and provides indexed access.
- `Set` stores unique elements; uniqueness is determined by `equals()` and
  `hashCode()`.
- `Map` stores key-value pairs with unique keys. `map[key]` returns a nullable
  value, so `containsKey()` is available to check whether a key exists.

Collections have read-only (`List`, `Set`, and `Map`) and mutable interfaces
(`MutableList`, `MutableSet`, and `MutableMap`):

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Read-only does not mean immutable. Another reference may modify the same object:

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Operations such as `filter`, `map`, and `sorted` return new collections:

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Operation chains on an `Iterable` may create intermediate collections. Consider
using a `Sequence` for long chains, but do not use one without a concrete need.

**In short:** expose read-only interfaces and keep mutable collections as an
implementation detail. When true immutability is required, create a copy or use
immutable collections.

</details>

<details>
<summary>23. How do you iterate over collections in Kotlin?</summary>

#### Kotlin

Use `for` for ordinary iteration:

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Use `forEach` for a short action on every element:

```kotlin
names.forEach { println(it) }
```

When an index is required:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

A `Map` can be iterated conveniently using destructuring:

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Use the appropriate operator for transformation or aggregation:

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Use `for` when `break`, `continue`, or complex control flow is needed. For
searching, prefer `firstOrNull`, `find`, `any`, or `none` to a manual loop.

**In short:** use `for` to control iteration, `forEach` for short side effects,
and `map` or `filter` to produce a result. Choose the operator that expresses
the code's intent.

</details>

<details>
<summary>24. What happens if you modify a list created with listOf()?</summary>

#### Kotlin

`listOf()` returns `List<T>`, a read-only interface without `add`, `remove`, or
`set` methods:

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Compilation error
```

However, `List` does not guarantee complete immutability. Another mutable
reference may modify the same list:

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Casting the result of `listOf()` to `MutableList` is unsafe: the code depends on
the concrete implementation and may fail at runtime.

If modifications are required, create a mutable list or a copy:

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

For an immutable style, create a new list:

```kotlin
val updated = names + "Oksana"
```

**In short:** a list returned by `listOf()` cannot be modified through the
`List` API. Use `mutableListOf()` or `toMutableList()` for modifications, not an
unsafe cast.

</details>

<details>
<summary>25. What happens when you access an index outside a list's bounds?</summary>

#### Kotlin

Accessing an element through `list[index]` calls `get(index)`. If the index is
negative or greater than or equal to `size`, an `IndexOutOfBoundsException` is
thrown:

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Safe alternatives include:

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

There are dedicated safe functions for a potentially empty list:

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**In short:** use `list[index]` when index validity is an invariant. For
external or optional indices, use `getOrNull`, `getOrElse`, or a check against
`indices`.

</details>

<details>
<summary>26. How does Kotlin interoperate with Java?</summary>

#### Kotlin

Kotlin/JVM compiles to JVM bytecode, so Kotlin and Java can call each other's
classes and methods directly:

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

The main risk is Java platform types, whose nullability is unknown. At language
boundaries, use `@Nullable` and `@NotNull`, validate values, and avoid passing
platform types into the application's internal layers.

The following annotations help create a convenient Java-facing API:

- `@JvmStatic` creates a static method;
- `@JvmOverloads` creates overloads for default parameters;
- `@JvmField` exposes a field without a getter or setter;
- `@JvmName` specifies a different JVM name;
- `@Throws` declares checked exceptions for Java.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Java SAM interfaces can be passed as lambdas:

```kotlin
executor.execute { println("Done") }
```

**In short:** interoperability enables gradual migration between Java and
Kotlin. Nullability, mutable collections, default parameters, and API usability
from Java require the most attention.

</details>

<details>
<summary>27. Can Java annotations be used in Kotlin?</summary>

#### Kotlin

Yes. Java annotations can be applied to Kotlin classes and their members:

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

On the JVM, a Kotlin property may be represented by a field, getter, setter, and
constructor parameter. A use-site target specifies where the annotation should
be applied:

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

The main options are:

- `@field:` targets the backing field;
- `@get:` / `@set:` targets the getter or setter;
- `@param:` targets the constructor parameter;
- `@property:` targets the Kotlin property, which is not a separate Java
  element.

Java nullability annotations help Kotlin determine the result type of a Java
API:

```java
@NotNull
String getName();
```

Without a correct annotation, the result may remain a platform type. For
reflection or validation frameworks, verify which JVM element the framework
reads.

**In short:** Java annotations are supported directly. The main concern is
choosing the correct `@field:`, `@get:`, or `@param:` target for the specific
framework.

</details>

<details>
<summary>28. What is the Kotlin REPL?</summary>

#### Kotlin

REPL stands for Read-Eval-Print Loop: the environment reads entered code,
executes it, displays the result, and waits for the next command.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

The REPL is useful for quickly testing syntax, standard-library features, and
small expressions without creating a project. It can be launched through the
Kotlin CLI; scratch files or Kotlin Playground are more convenient for longer
experiments.

The REPL does not reproduce the Android lifecycle, dependency injection, Gradle
configuration, or a real concurrent environment, and it does not replace
automated tests.

**In short:** the Kotlin REPL is an interactive console for local language
experiments. Results relevant to an application should be confirmed with tests
in the actual environment.

</details>

<details>
<summary>29. What is a Kotlin script (.kts)?</summary>

#### Kotlin

A Kotlin script is a `.kts` file in which top-level statements can be executed
without a `fun main()` function:

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

A `.kt` file is a regular source file for an application or library, whereas a
`.kts` file is executed by a scripting environment. The most common example is
the Gradle Kotlin DSL:

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

The `build.gradle.kts` and `settings.gradle.kts` files are executed by Gradle as
configuration scripts. Standalone scripts can be used for small automation
tasks, but their dependency model and APIs depend on the scripting host.

**In short:** `.kts` is suitable for configuration and short automation tasks.
Logic that grows, has dependencies, and requires tests is better moved to a
regular Kotlin module or CLI application.

</details>

<details>
<summary>30. What is the Kotlin style guide, and why should you follow it?</summary>

#### Kotlin

The Kotlin style guide defines rules for formatting, naming, file structure, and
import organization. It makes a team's code consistent and predictable.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Practical benefits include:

- fewer formatting disputes during code review;
- faster code comprehension and onboarding for new developers;
- automated checks through an IDE formatter, `ktlint`, or `detekt` in CI.

Team-specific deviations from the official rules should be documented and
configured consistently in both the IDE and CI. A style guide governs code
presentation but does not replace architectural rules.

**In short:** a style guide reduces cognitive load and moves formatting checks
from people to automated tools.

</details>

<details>
<summary>31. What are Kotlin idioms?</summary>

#### Kotlin

Kotlin idioms are established ways of using the language's features so that the
code clearly communicates its intent.

Typical examples include:

- using `val` by default and `var` only when mutation is necessary;
- using nullable types, `?.`, and `?:` instead of `!!`;
- using a `data class` for data models;
- using `sealed` types for a limited set of states;
- using `map`, `filter`, and `associateBy` instead of manual transformation
  loops;
- using guard clauses to reduce nesting.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Scope functions, lambdas, and operator overloading are appropriate only when
they improve readability. Using more language features does not, by itself,
make code idiomatic.

**In short:** idiomatic Kotlin is safe and understandable code that uses
language features for their intended purpose, not merely to demonstrate syntax.

</details>

<details>
<summary>32. How is Kotlin used in backend development, and why is it chosen over Java?</summary>

#### Kotlin

Kotlin is used for JVM backends, including REST and gRPC APIs, microservices,
workers, and event-driven systems. It works with Spring Boot, Ktor, and Java
libraries.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Its main advantages over Java are:

- null safety;
- less boilerplate through `data class`, default parameters, and extension
  functions;
- coroutines for non-blocking I/O;
- gradual migration and full access to the Java ecosystem.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Trade-offs include slower compilation, additional compiler and Gradle
configuration for some Java frameworks, and the need to teach the team
idiomatic Kotlin and structured concurrency.

**In short:** Kotlin is a good choice when a team wants safer, more concise JVM
code without abandoning the Java ecosystem and is prepared to accept a more
complex toolchain.

</details>

<details>
<summary>33. What are higher-order functions?</summary>

#### Kotlin

A higher-order function accepts a function as an argument or returns one as its
result.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` is a function with no parameters and no meaningful return
value. Here is an example of returning a function:

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

Common examples from the standard library include:

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Such functions are used with collections, callbacks, and DSLs. Lambdas may
create objects and capture external state; `inline` often eliminates this
overhead for small higher-order functions.

**In short:** higher-order functions allow behavior to be passed as a value.
Complex or nested lambdas are better extracted into named functions.

</details>

<details>
<summary>34. What are inline functions, and when should you use them?</summary>

#### Kotlin

`inline` asks the compiler to insert the function body and its lambda parameters
at the call site. Its primary purpose is to reduce the overhead of higher-order
functions:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` also enables `reified` type parameters:

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Additional modifiers for lambda parameters are:

- `noinline` prevents a lambda from being inlined, allowing it to be stored or
  passed elsewhere;
- `crossinline` inlines a lambda but prohibits a non-local `return`.

`inline` is appropriate for small higher-order functions, `reified` generics,
and measured hot paths. For large functions, it may increase bytecode size.
Changes to public inline functions also require attention to binary
compatibility.

**In short:** do not use `inline` as a universal optimization; use it to reduce
lambda overhead, enable `reified` types, or provide the required non-local
return semantics.

</details>

<details>
<summary>35. What does the reified keyword mean, and where is it used?</summary>

#### Kotlin

Because of type erasure, a regular generic parameter `T` cannot be used for a
type check or to obtain `T::class`:

```kotlin
// fun <T> isOfType(value: Any) = value is T // Compilation error
```

`reified` makes this possible inside an `inline` function because the compiler
inserts the concrete type at the call site:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

Class information can also be obtained:

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

An example from the standard library is:

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

There are limitations: `reified` is available only for type parameters of
inline functions, and it does not expose nested generic arguments such as the
`String` in `List<String>`. For non-inline APIs, pass a `KClass`, `Class`, or
serializer explicitly.

**In short:** `reified` gives an inline function access to a concrete `T` for
`is`, `T::class`, reflection, and similar APIs, but it does not eliminate type
erasure completely.

</details>

<details>
<summary>36. What are sealed classes and sealed interfaces?</summary>

#### Kotlin

A `sealed class` or `sealed interface` defines a controlled hierarchy of direct
subtypes. They are suitable for representing states, results, and commands:

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

The compiler verifies that a `when` expression handles every case:

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Direct subtypes must be declared in the same module and package. A
`sealed class` can have a constructor, state, and shared implementation. A
`sealed interface` has no constructor, but a class can implement multiple
interfaces.

Unlike an `enum`, each sealed variant can contain a different set of data.
`sealed` is unsuitable when external modules must extend the hierarchy. Avoid
adding an unnecessary `else` branch to `when`, so the compiler can detect new
cases.

**In short:** sealed types model a closed set of variants with different data
and provide compile-time exhaustiveness checks for `when`.

</details>

<details>
<summary>37. What is an object expression, and when is it used?</summary>

#### Kotlin

An `object expression` creates an anonymous object at runtime. It is commonly
used for a one-off implementation of an interface or class:

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

It can also inherit from a class:

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

An anonymous object without a supertype is useful only locally or in a
`private` API:

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Its anonymous type cannot be exported as a stable public type: only its declared
supertype or `Any` is visible externally. An object expression is created each
time the expression is evaluated, whereas an object declaration is a named
singleton.

Such an object can capture external variables. If the implementation grows or
is repeated, it is better extracted into a named class.

**In short:** an object expression is suitable for a short local
implementation, listener, or test stub. Public or reusable logic requires a
named type.

</details>

<details>
<summary>38. What is the difference between enum classes and sealed classes?</summary>

#### Kotlin

An `enum class` contains a fixed set of singleton values of the same type:

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

A `sealed class` or `sealed interface` defines a closed hierarchy of subtypes.
Each variant can contain different data:

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

For both approaches, the compiler checks that a `when` expression is
exhaustive:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

An `enum` provides standard `entries`, `name`, and `ordinal` properties. A
sealed type does not provide a ready-made list of instances, but it allows many
objects of the same subtype to be created with different data.

**In short:** use an `enum` for simple constants with a shared structure. Use a
sealed type for a closed set of states or results with different data.

</details>

<details>
<summary>39. What is the difference between lateinit and lazy initialization?</summary>

#### Kotlin

Both mechanisms defer initialization, but they work differently.

`lateinit` allows a non-null `var` to be assigned after an object is created:

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Reading it before assignment throws an `UninitializedPropertyAccessException`.
`lateinit` works only with a non-null `var` of a reference type, does not
guarantee thread safety, and permits reassignment.

Its initialization state can be checked:

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` computes a `val` on its first read and caches the successful result:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

By default, `lazy` is synchronized. The `PUBLICATION` and `NONE` modes change
its guarantees; `NONE` is safe only when accessed from a single thread.

- Use `lateinit` when external code or DI assigns the value later.
- Use `lazy` when an initializer should run on demand.
- Use a nullable type when the absence of a value is a valid state.

**In short:** `lateinit` provides deferred external assignment of a mutable
property; `lazy` provides one-time deferred computation of a read-only
property.

</details>

<details>
<summary>40. What is the Nothing type, and how does it affect control flow?</summary>

#### Kotlin

`Nothing` is a type with no possible values. A function with this return type
never completes normally:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` is a subtype of every type, so it can be used in any expression where
execution is terminated:

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` and `return` have the type `Nothing`, so they can be part of an Elvis
expression:

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

The type helps the compiler analyze control flow and perform smart casts:

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` has only one possible value, `null`; it is the type of a `null`
expression when no other context is available. The standard `error()` and
`TODO()` functions also return `Nothing`.

**In short:** `Nothing` represents a path that does not return control. This
allows the compiler to infer types and analyze subsequent control flow more
precisely.

</details>

<details>
<summary>41. Can Java call Kotlin functions with default parameters?</summary>

#### Kotlin

Java does not support Kotlin's default parameters. Without additional
overloads, it must pass every argument:

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

`@JvmOverloads` generates Java-friendly overloads:

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

Overloads are generated from right to left for consecutive parameters with
default values. The annotation also works with constructors:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

The annotation is unnecessary for Kotlin-only APIs. A large number of
parameters produces too many overloads; in that case, a configuration object or
builder is preferable.

**In short:** without `@JvmOverloads`, Java can call only the full signature.
Add the annotation only to APIs that are actually used from Java.

</details>

<details>
<summary>42. What are tail-recursive functions?</summary>

#### Kotlin

A tail-recursive function calls itself as its final operation. The `tailrec`
modifier allows the compiler to transform such a call into a loop without
growing the stack:

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

The following version is not tail-recursive because multiplication is performed
after the recursive call:

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

The optimization works only for direct recursion and does not apply when the
call appears in a construct that prevents transformation, such as
`try`/`finally`. If `tailrec` is used incorrectly, the compiler warns that the
optimization is not performed.

**In short:** `tailrec` enables readable recursion without the risk of a
`StackOverflowError`. If a regular loop is simpler, prefer the loop.

</details>

<details>
<summary>43. What are scope functions (let, run, apply, also, and with), and when should you use them?</summary>

#### Kotlin

Scope functions execute a block in the context of an object. They differ in two
ways:

- how the object is accessed: `this` or `it`;
- what they return: the lambda result or the object itself.

| Function | Object accessed as | Returns           |
| -------- | ------------------ | ----------------- |
| `let`    | `it`               | lambda result     |
| `run`    | `this`             | lambda result     |
| `apply`  | `this`             | the object itself |
| `also`   | `it`               | the object itself |
| `with`   | `this`             | lambda result     |

Typical uses are:

- `let` for transformation or a block after a safe call;
- `run` for computing a result with access through `this`;
- `apply` for configuring an object;
- `also` for a side effect without changing the chain;
- `with` for grouping operations on a passed object.

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

`with` is not an extension function: the object is passed as its first
argument. Deeply nested scope functions and mixing `this` with `it` reduce
readability; use ordinary code or a named function instead.

**In short:** choose a scope function based on its receiver (`this` or `it`) and
return value (the object or lambda result), not out of habit.

</details>

<details>
<summary>44. What is the difference between let, run, and apply?</summary>

#### Kotlin

These functions differ in how the object is accessed and what they return:

| Function | Receiver in block | Returns           |
| -------- | ----------------- | ----------------- |
| `let`    | `it`              | lambda result     |
| `run`    | `this`            | lambda result     |
| `apply`  | `this`            | the object itself |

Use `let` for transformation or execution after a safe call:

```kotlin
val length = name?.let { it.length }
```

Use `run` to compute a result with several references to the object:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

Use `apply` for configuration that returns the same object:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

Do not use a scope function when an ordinary call is more readable. Nested
blocks with multiple `this` or `it` references are better replaced with a named
function.

**In short:** `let` uses `it` and returns a new result; `run` uses `this` and
returns a new result; `apply` uses `this` and returns the original object.

</details>

<details>
<summary>45. What is the difference between map and flatMap?</summary>

#### Kotlin

`map` transforms each element into one result:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

If the transformation returns a collection, `map` creates a nested structure:

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` performs the transformation and combines the nested collections into
one:

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

A practical example is:

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Conceptually, these expressions are equivalent:

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**In short:** `map` performs `A -> B`. `flatMap` performs
`A -> Iterable<B>` and then combines all results into a flat collection.

</details>

<details>
<summary>46. What is a Sequence, and when should you use one?</summary>

#### Kotlin

A `Sequence` is a lazy synchronous sequence. Intermediate operators do not
process data immediately; instead, they build a pipeline:

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

A terminal operation such as `toList`, `first`, `count`, `fold`, or `any`
triggers computation. Elements pass through the pipeline one at a time, so
short-circuiting operations can stop processing early:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

A `Sequence` is appropriate for large data sets, long chains of `map` and
`filter`, and short-circuiting operations. For a small collection or a single
transformation, its overhead may exceed its benefit. Operations such as
`sorted` still need to accumulate the elements.

A `Sequence` is not asynchronous and does not support suspension. Use `Flow`
for an asynchronous stream of data.

**In short:** a `Sequence` reduces intermediate collections and unnecessary
work in long synchronous pipelines, but it is not automatically faster than
regular collections.

</details>

<details>
<summary>47. What is lazy collection processing?</summary>

#### Kotlin

Lazy processing means that a pipeline is only described initially and is
executed after a terminal operation. Kotlin uses `Sequence` for this:

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Elements usually pass through the operators one at a time. This allows
processing to finish early:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

A lazy pipeline does not cache its result: another terminal operation usually
runs the computation again. Potentially infinite sequences can also be used if
they are limited with `take`:

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Lazy operators may introduce their own overhead, while `sorted` and similar
operations still accumulate data. Asynchronous processing requires `Flow`, not
`Sequence`.

**In short:** lazy processing defers work until a terminal operation and can
avoid unnecessary computation. It does not cache the result and is not always
faster than eager collection processing.

</details>

<details>
<summary>48. What are coroutines, and how do they differ from threads?</summary>

#### Kotlin

A coroutine is a lightweight task that can suspend execution and resume later.
A separate OS thread is not created for each coroutine:

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

A thread is an operating-system resource with its own stack. Many coroutines
can run on a small number of threads and move between them after suspension.

```kotlin
delay(1_000)        // Suspends the coroutine
Thread.sleep(1_000) // Blocks the thread
```

`suspend` alone does not guarantee a non-blocking implementation. A blocking
API still blocks a thread and must run in an appropriate context.

A dispatcher determines the execution threads:

- `Dispatchers.Main` is used for UI work;
- `Dispatchers.IO` is used for blocking I/O;
- `Dispatchers.Default` is used for CPU-bound work.

Structured concurrency associates coroutines with a `CoroutineScope`: a parent
operation manages the lifecycle, errors, and cancellation of its child tasks.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Coroutines do not eliminate race conditions or automatically make CPU-bound
work faster. Heavy computation requires explicitly selecting the correct
dispatcher:

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**In short:** coroutines are tasks running on top of threads with suspension
and structured concurrency. They are cheaper than a one-task-per-thread model,
but they do not replace correct dispatcher and shared-state management.

</details>

<details>
<summary>49. What is a suspend function?</summary>

#### Kotlin

`suspend` marks a function that may contain suspension points. It can be called
from another suspend function or from a coroutine:

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` does not create a coroutine, run the function in parallel, or
automatically make blocking code non-blocking:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

The compiler transforms a suspend function into a state machine with a
`Continuation`, allowing it to preserve state and resume execution after
suspension.

Cancellation is cooperative: standard suspension points check for it, while
long CPU-bound loops must check explicitly:

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**In short:** `suspend` allows a function to suspend and resume a coroutine. It
does not guarantee a non-blocking implementation or create a new coroutine by
itself.

</details>

<details>
<summary>50. What is CoroutineScope, and what is it used for?</summary>

#### Kotlin

A `CoroutineScope` owns coroutines and contains a `coroutineContext`. The
context usually includes a `Job`, a dispatcher, and additional elements:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

The `Job` establishes parent-child relationships and manages cancellation.
Cancelling the scope cancels its child coroutines:

```kotlin
scope.cancel()
```

Use a scope builder for parallel work inside a suspend function:

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` completes only after all child tasks have completed. On
Android, `viewModelScope`, `lifecycleScope`, and `rememberCoroutineScope()`
already have defined lifecycles.

Create a custom scope only for an object with an explicit completion method
that calls `cancel()`. `GlobalScope` has no such owner and violates structured
concurrency, so it should be avoided in application code.

**In short:** `CoroutineScope` associates coroutines with a clear lifecycle and
provides shared cancellation. Every scope should have an owner.

</details>

<details>
<summary>51. What is the difference between launch and async?</summary>

#### Kotlin

Both builders start a child coroutine in a `CoroutineScope`.

`launch` returns a `Job` and is used when no separate result is required:

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` returns a `Deferred<T>`. Its result or error is obtained through
`await()`. A typical use case is running independent operations in parallel:

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

By default, `async` starts immediately rather than when `await()` is called. A
child coroutine failure cancels a regular parent scope regardless of the
builder; with `async`, `await()` also rethrows the error. Supervision behavior
must be configured separately.

Do not use `async` if you do not intend to call `await()`, and do not run
operations in parallel when they depend on each other.

**In short:** `launch` represents a task without a result value; `async`
represents a task with a `Deferred<T>` result obtained through `await()`.

</details>

<details>
<summary>52. What is runBlocking, and when is it used?</summary>

#### Kotlin

`runBlocking` creates a `CoroutineScope` and blocks the current thread until the
block and all of its child coroutines complete:

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

It is a boundary tool for calling suspend code from a synchronous API. It may be
appropriate in a legacy bridge or a short example. Coroutine tests should use
`runTest`.

In UI code, `runBlocking` blocks the main thread and may cause freezing, an ANR,
or a deadlock:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Instead, launch a coroutine in a scope with the appropriate lifecycle:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

`runBlocking` is unnecessary inside a suspend function and only blocks a
thread:

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**In short:** `runBlocking` is a synchronous bridge to suspend code. Do not use
it inside coroutines, suspend functions, or on the UI thread.

</details>

<details>
<summary>53. What are dispatchers (Dispatchers.IO, Default, and Main)?</summary>

#### Kotlin

A `CoroutineDispatcher` schedules coroutine execution on appropriate threads:

- `Dispatchers.Main` is used for UI and short operations on the main thread;
- `Dispatchers.IO` is used for blocking I/O;
- `Dispatchers.Default` is used for CPU-intensive computation.

A blocking file operation should be moved to `IO`:

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Heavy computation should be moved to `Default`:

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` suspends the current coroutine, executes the block in the
specified context, and returns its result. It preserves structured concurrency,
cancellation, and error propagation.

Not every network or database call requires `Dispatchers.IO`: a suspend API may
already be main-safe. The layer that knows whether the implementation blocks a
thread should choose the dispatcher. A dispatcher can be injected for
testability:

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

Avoid switching context for trivial work because context switching also has a
cost.

**In short:** use `Main` for UI, `IO` for blocking I/O, and `Default` for heavy
computation. `withContext` changes the context for a specific operation without
creating a new independent coroutine.

</details>

<details>
<summary>54. How do you cancel coroutines and handle errors?</summary>

#### Kotlin

Coroutine cancellation is cooperative and is managed through a `Job`:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Suspend functions such as `delay()` respond to cancellation automatically.
CPU-bound code must check its state regularly:

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

Cancellation is signaled with a `CancellationException`. If a broad `catch`
intercepts it, the exception must be rethrown:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

With regular structured concurrency, an unhandled child-coroutine failure
cancels the parent and sibling tasks. Expected errors should be handled locally
and converted into a domain result or UI state:

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

Use `supervisorScope` or `SupervisorJob` for independent tasks:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

A supervisor does not handle an error; it only isolates sibling tasks.
`CoroutineExceptionHandler` is intended as the final handler for uncaught
exceptions in a root `launch`, for example, for logging. Errors from `async` are
received through `await()`.

**In short:** cancel through a `Job`, never swallow a `CancellationException`,
handle expected errors locally, and isolate independent tasks with supervision.

</details>

<details>
<summary>55. What is structured concurrency?</summary>

#### Kotlin

Structured concurrency means that every coroutine has an owner and belongs to a
`Job` hierarchy. A parent waits for its child tasks and manages their
cancellation and errors.

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

`coroutineScope` does not complete before its child coroutines. Cancelling the
caller cancels them, while an unhandled failure in one task cancels the scope
and its sibling tasks.

Use supervision for independent tasks:

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

A direct child's failure does not cancel other child tasks, but the error must
still be handled. Use `supervisorScope` locally and `SupervisorJob` in a
long-lived scope.

`GlobalScope` violates this model: the caller cannot properly await or cancel
the work or receive its error. A custom scope must have a clear lifecycle and
explicit cleanup.

**In short:** structured concurrency prevents child work from silently
outliving its owner. Related tasks use `coroutineScope`; independent tasks use
supervision.

</details>

<details>
<summary>56. Can coroutines run on any thread?</summary>

#### Kotlin

A coroutine is not bound to its own thread. A `CoroutineDispatcher` determines
where its parts execute:

```kotlin
scope.launch(Dispatchers.Main) {
    // UI thread
}

scope.launch(Dispatchers.IO) {
    // blocking I/O
}

scope.launch(Dispatchers.Default) {
    // CPU-bound work
}
```

- `Main` is used for UI work;
- `IO` is used for blocking I/O;
- `Default` is used for CPU-bound work.

On a pool-based dispatcher, a coroutine may resume on a different thread from
the same dispatcher after suspension:

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` temporarily changes the context and restores the caller's context
after completion. `Dispatchers.Main` guarantees execution on the UI thread;
pool dispatchers do not guarantee a specific thread ID.

A regular `ThreadLocal` does not move between threads with a coroutine. When
needed, its value can be added to the coroutine context:

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

A custom dispatcher with its own threads must have an owner that closes it. A
blocking call blocks a thread regardless of whether it runs inside a coroutine.

**In short:** a coroutine runs on its dispatcher's threads and may switch
physical threads after suspension. The dispatcher must guarantee any required
thread affinity.

</details>

<details>
<summary>57. What is Flow in Kotlin?</summary>

#### Kotlin

A `Flow<T>` is an asynchronous stream of values built on coroutines. A suspend
function returns one value, whereas a `Flow` produces a sequence of values over
time:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

A flow is created with a builder and started by a terminal operator such as
`collect`:

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

A flow created with `flow {}` is cold: the producer starts separately for each
collector. `emit()` is a suspend function, so without explicit buffering, the
producer does not outrun a slow collector.

Operators build a pipeline:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` changes the context of upstream operators:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` handles only upstream errors; it does not catch errors from the
collector. Cancelling the collector cancels collection and the producer within
structured concurrency.

**In short:** use a suspend function for one result and `Flow` for values that
arrive over time and require an asynchronous pipeline.

</details>

<details>
<summary>58. What is the difference between Flow, StateFlow, and SharedFlow?</summary>

#### Kotlin

```text
Flow       -> usually a cold pipeline
StateFlow  -> a hot stream with current state
SharedFlow -> a hot broadcast with replay and a buffer policy
```

A `Flow` created with `flow {}` starts its producer separately for every
collector and does not retain a current value:

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

A `StateFlow` always has an initial value. A new collector immediately receives
the current state, while values equal according to `equals()` are not emitted
again:

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

Keep the mutable version `private`; use `update { }` for concurrent updates.

A `SharedFlow` sends values to all active collectors and does not have to retain
a current value:

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` determines how many recent values a new collector receives;
`extraBufferCapacity` and `onBufferOverflow` control buffering. With
`replay = 0`, a value is lost when there are no subscribers, so critical data
should be modeled as state rather than as a one-time event.

A cold flow can be converted into a shared hot flow: `stateIn()` creates a
`StateFlow`, while `shareIn()` creates a `SharedFlow`. `SharingStarted`
determines when the shared upstream starts and stops.

**In short:** `Flow` is an independent lazy pipeline, `StateFlow` represents
current state, and `SharedFlow` is a shared event stream with configurable
replay.

</details>

<details>
<summary>59. What is the difference between Flow and LiveData?</summary>

#### Kotlin

`LiveData` is an Android lifecycle-aware holder, whereas `Flow` is a coroutine
stream with no dependency on Android.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` notifies only active observers and redelivers the latest value after
activation. It is convenient for legacy View-based UI but should not enter the
domain layer.

`Flow` supports coroutine cancellation, errors, and a broad set of operators:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

A regular `Flow` is usually cold and does not retain a current value. Use
`StateFlow` for UI state. Lifecycle must be handled explicitly when collecting
a flow:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Compose provides `collectAsStateWithLifecycle()` for this purpose. `LiveData`
is oriented toward the main thread, while `Flow` uses a coroutine context and
`flowOn` for upstream operations.

The types can be converted during migration:

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**In short:** `LiveData` handles the Android lifecycle automatically. `Flow` is
independent of Android and is better suited to data and domain layers; collect
it lifecycle-aware in the UI and keep current state in a `StateFlow`.

</details>

<details>
<summary>60. What is the difference between cold and hot streams?</summary>

#### Kotlin

The difference lies in the relationship between the producer and collectors:

```text
cold -> a separate producer starts for each collector
hot  -> collectors receive data from a shared source
```

A flow created with `flow {}` is cold. It does not execute before `collect`, and
each collector starts the block separately:

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

The operation therefore runs twice. Each collector has its own lifecycle and
cancellation.

`StateFlow` and `SharedFlow` are hot: their object and data exist independently
of any particular collector.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` retains the current state. `SharedFlow` broadcasts values to active
collectors and can retain recent values through `replay`.

A cold flow can be converted into a shared hot flow using `stateIn()` or
`shareIn()`. The `SharingStarted` policy determines when the shared upstream is
active, but all collectors still use the same execution.

**In short:** a cold stream starts a producer for each collector; a hot stream
has a shared source and broadcasts its values to all collectors.

</details>

<details>
<summary>61. What is collectLatest, and when should you use it?</summary>

#### Kotlin

`collectLatest` is a terminal operator that cancels processing of the previous
value when a new one arrives:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Only 3 has time to be printed
}
```

A typical use case is search, where a previous request becomes obsolete:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Cancellation is cooperative. Suspend APIs respond to it, but blocking code does
not:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

Do not use `collectLatest` when every value must be processed reliably, such as
payments, messages, persistence operations, or analytics events.

`flatMapLatest` cancels the previous inner flow, whereas `collectLatest` cancels
the collector block.

**In short:** use `collect` when every value matters; use `collectLatest` when
processing the previous value is no longer needed after a new value arrives.

</details>

<details>
<summary>62. How do you convert a callback-based API into a suspend function?</summary>

#### Kotlin

Wrap a one-shot callback with `suspendCancellableCoroutine`:

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

- `resume()` returns a result;
- `resumeWithException()` returns an error;
- `invokeOnCancellation` cancels the external operation or removes the
  listener.

Cancelling the coroutine does not automatically stop the callback API. A
continuation can be completed only once; if the callback may fire repeatedly or
concurrently, use `tryResume()` and `completeResume()` or atomic protection.

Use `callbackFlow` for a callback that produces multiple values:

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

`awaitClose` removes the listener when the flow is cancelled or closed. Handle
the result of `trySend()` and the buffer policy according to the required
delivery guarantees.

**In short:** use `suspendCancellableCoroutine` for one result and
`callbackFlow` for a stream of values. In both cases, always implement cleanup
on cancellation.

</details>

<details>
<summary>63. What is the difference between Channel and Flow?</summary>

#### Kotlin

A `Channel<T>` is an asynchronous queue for communication between coroutines:

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

A producer calls `send()`, while a consumer calls `receive()` or reads with a
`for` loop. Each value is received by one consumer, so a channel is suitable for
worker queues and fan-out. Capacity, closing, and cancellation must be managed
explicitly.

A `Flow<T>` is a declarative API for an asynchronous stream:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow supports `map`, `filter`, `combine`, cancellation, and structured
concurrency. A flow created with `flow {}` is usually cold; a channel is hot and
may accept values without an active consumer, depending on its capacity.

Channel backpressure is determined by its buffer:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Waits for a receiver
```

- `RENDEZVOUS` provides no buffer;
- `BUFFERED` provides a bounded buffer;
- `CONFLATED` retains the latest value;
- `UNLIMITED` provides an unbounded buffer with a risk of memory growth.

Flow provides `buffer()`, `conflate()`, and `collectLatest()`.

A channel can be exposed as a flow:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` does not create a broadcast: collectors distribute values
among themselves. Use `StateFlow` for state, `SharedFlow` for broadcasting, and
`callbackFlow` as a callback bridge.

**In short:** a `Channel` is a producer-consumer queue; a `Flow` is a
declarative data stream. Start with Flow for application streams and reserve
Channel for explicit work transfer between coroutines.

</details>

<details>
<summary>64. What are the main Flow operators (map, filter, combine, and zip)?</summary>

#### Kotlin

`map` transforms every value:

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` passes only values that satisfy a condition:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` waits for the first value from every flow and then emits a new result
whenever any of them is updated:

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` combines values pairwise in order:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

The result is `1A`, `2B`, `3C`. `zip` waits for the corresponding pair, whereas
`combine` uses the latest available values. Therefore, `combine` is usually
appropriate for state from multiple sources, while `zip` is appropriate for
pairs of elements.

**In short:** `map` transforms, `filter` excludes, `combine` reacts to the
latest values from all flows, and `zip` combines values pairwise.

</details>

<details>
<summary>65. What is the difference between combine and zip?</summary>

#### Kotlin

Both operators combine flows, but their semantics differ:

```text
combine -> a new value plus the latest values from the other flows
zip     -> the first with the first, the second with the second
```

`combine` waits for the first value from every upstream and then reacts to each
update:

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` creates pairs in order:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

With `zip`, a faster upstream waits for its matching pair. The result completes
when no more complete pairs can be formed. With `combine`, a completed upstream
can continue contributing its latest value while the other upstreams keep
emitting.

**In short:** use `combine` for state that updates from any source; use `zip`
for pairwise matching of sequences.

</details>

<details>
<summary>66. What are @JvmStatic, @JvmOverloads, and @JvmField used for?</summary>

#### Kotlin

These annotations modify the JVM API exposed to Java code.

- `@JvmStatic` generates a static bridge for a member of an `object` or
  `companion object`.
- `@JvmOverloads` generates overloads for consecutive parameters with default
  values, starting from the right.
- `@JvmField` exposes the backing field directly, without a getter or setter.

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

Java can call `UserFactory.create()`, read `UserFactory.defaultName`, and use
the overload without the `active` argument.

`@JvmField` does not work with delegated properties or custom accessors and
weakens encapsulation. Prefer `const val` for compile-time constants of a
primitive or `String` type.

**In short:** these annotations are needed only to provide a convenient API for
Java or frameworks. Do not add them unnecessarily in Kotlin-only code.

</details>

<details>
<summary>67. What is delegation?</summary>

#### Kotlin

Delegation forwards an implementation to another object instead of using
inheritance. Kotlin supports interface delegation through `by`:

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

The compiler generates forwarding methods. An individual method can be
overridden:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Properties can also have delegates:

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

A custom property delegate implements `getValue()` and, for a `var`, also
`setValue()`.

**In short:** class delegation removes forwarding boilerplate and supports
composition; property delegation moves property read and write logic into a
separate object.

</details>

<details>
<summary>68. What testing frameworks are available for Kotlin?</summary>

#### Kotlin

The choice depends on the testing level:

- `kotlin.test` provides multiplatform annotations and assertions;
- JUnit is used for JVM unit tests;
- Kotest provides an alternative DSL and matchers;
- MockK or Mockito provides test doubles;
- `kotlinx-coroutines-test` provides coroutine test dispatchers and virtual
  time;
- Turbine verifies Flow emissions;
- AndroidX Test, Espresso, and Compose UI Test cover Android and UI testing;
- Robolectric runs some Android scenarios on the JVM.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

For domain logic, JUnit or `kotlin.test` and manual fakes are often sufficient.
A tool does not replace choosing the correct test level: device-specific
behavior should be verified with instrumentation tests.

**In short:** use JUnit or `kotlin.test` for unit tests, coroutines-test for
coroutines, Turbine for Flow, and Espresso or Compose UI Test for Android UI.

</details>

<details>
<summary>69. How do you mock dependencies in tests?</summary>

#### Kotlin

Pass a dependency through the constructor and replace it with a mock, stub, or
fake.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

MockK can mock a suspend function:

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

A manual fake can be implemented as follows:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

A fake is better suited to behavioral testing, while a mock is appropriate when
an interaction is part of the contract. Avoid mocking value objects, pure
functions, and every internal call because such tests become coupled to the
implementation.

**In short:** use constructor injection and prefer a simple fake; use a mock for
important interactions with an external dependency.

</details>

<details>
<summary>70. How do you test coroutines and Flow?</summary>

#### Kotlin

Use `kotlinx-coroutines-test` and `runTest` for coroutines:

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` provides virtual time and a `TestCoroutineScheduler`:

- `runCurrent()` executes tasks scheduled for the current time;
- `advanceTimeBy()` advances virtual time;
- `advanceUntilIdle()` processes the queue until it becomes idle.

Dispatchers should preferably be injected. In local unit tests,
`Dispatchers.Main` is replaced with a test dispatcher and restored with
`resetMain()`.

Turbine is convenient for verifying the sequence of Flow emissions:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

To verify the final state of a `StateFlow`, it is often sufficient to execute
queued tasks and check `state.value`. Do not use `Thread.sleep()`, real delays,
or different test schedulers within the same test.

**In short:** use `runTest` and a test dispatcher for coroutines, Turbine for
the order of Flow emissions, and `StateFlow.value` for the final state.

</details>

<details>
<summary>71. What are the best practices for writing testable code?</summary>

#### Kotlin

Testable code has explicit dependencies and predictable behavior.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

The main practices are:

- use constructor injection instead of creating dependencies inside a class;
- give each class one clear responsibility;
- use pure functions for business calculations;
- create abstractions for time, I/O, APIs, databases, and dispatchers;
- keep Android framework dependencies out of domain logic where possible;
- prefer fakes to excessive mocking;
- verify public behavior rather than private methods and internal calls.

Test a ViewModel through its observable state and effects:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

There is no need to create an interface for every class. An abstraction is
justified at a boundary with a variable or external dependency.

**In short:** explicit dependencies, controlled side effects, and small
responsibilities make code testable without coupling tests to the
implementation.

</details>

<details>
<summary>72. How do you manage dependencies effectively in a Kotlin project?</summary>

#### Kotlin

In a Gradle project, versions, scopes, and dependency directions between modules
must be controlled.

- A version catalog centralizes aliases and versions in `libs.versions.toml`.
- `implementation` hides a dependency from downstream modules.
- `api` exposes it as part of the public API and should be used only when
  necessary.
- `testImplementation` and `androidTestImplementation` isolate test
  dependencies.
- A BOM aligns artifact versions within one ecosystem.
- Convention plugins eliminate duplicated Gradle configuration.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

A module should depend on the required contract, not the entire implementation.
Gradle dependencies and runtime DI operate at different levels: Gradle
determines code availability, while DI creates objects at runtime.

For diagnostics, use:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Perform updates in small steps while checking the changelog, build, and tests.

**In short:** centralize versions, prefer `implementation`, control the module
graph, and inspect the dependency graph regularly.

</details>

<details>
<summary>73. How does Kotlin handle memory and garbage collection?</summary>

#### Kotlin

The memory model depends on the platform. On the JVM and Android, Kotlin uses
JVM/ART memory management: objects live on the heap, and the garbage collector
reclaims those that no longer have reachable references.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

After the function completes, the object can be collected if nothing retains
it. Assigning `null` only removes a reference; the garbage collector determines
when reclamation occurs.

A memory leak occurs when an unnecessary object remains reachable:

```kotlin
object Holder {
    var activity: Activity? = null // May retain the Activity
}
```

Common Android causes include storing an `Activity` or `View` in a singleton,
uncleared listeners or bindings, and a coroutine with a longer lifecycle.
Frequent allocations, boxing, closures, and intermediate collections can
increase GC pressure, but they should be optimized only after profiling.

Kotlin/Native and Kotlin/JS use their platform's memory mechanisms, so JVM
details cannot be generalized to every target.

**In short:** on the JVM and Android, the garbage collector reclaims memory
after all reachable references are gone. The main risk is not `null`, but a
long-lived reference to an unnecessary object.

</details>

<details>
<summary>74. What are the best practices for null safety?</summary>

#### Kotlin

A nullable type should represent a genuine possibility that a value is absent,
not be used merely as a precaution.

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

The main practices are:

- avoid `!!`; use `?.`, `?:`, or an early return;
- use `requireNotNull` for an input contract and `checkNotNull` for internal
  state;
- validate Java platform types at the Java–Kotlin boundary;
- map nullable DTOs to strict domain models in a mapper;
- model `null` as a sealed type when it represents a distinct state.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

A fallback should not conceal an error: if an absent value violates a contract,
terminate execution explicitly.

**In short:** model the absence of a value with `T?`, handle it at the boundary,
and do not turn nullable code into a chain of `!!` operators.

</details>

<details>
<summary>75. What is supervisorScope, and how is it different?</summary>

#### Kotlin

In a `coroutineScope`, an unhandled child-coroutine failure cancels the scope
and its sibling tasks. In a `supervisorScope`, direct children can fail
independently:

```text
coroutineScope  -> a child failure cancels sibling tasks
supervisorScope -> a child failure does not cancel sibling tasks
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

A supervisor does not handle errors automatically. An exception from `async`
is thrown by `await()` unless it is converted into a result.

Use `supervisorScope` locally inside a suspend function and `SupervisorJob` for
a long-lived scope.

**In short:** use `coroutineScope` for related all-or-nothing tasks and
`supervisorScope` for independent tasks where a partial result is acceptable.

</details>

<details>
<summary>76. What is backpressure, and how do you handle it in Flow?</summary>

#### Kotlin

Backpressure occurs when a producer may be faster than a consumer. In a regular
flow without a buffer, `emit()` suspends until downstream processing completes,
so the producer naturally slows down.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

The following operators change this behavior:

- `buffer(n)` allows the producer to run `n` values ahead of the consumer;
- `conflate()` skips intermediate values while retaining the latest one;
- `collectLatest` cancels previous processing;
- `debounce` waits for a pause between incoming events;
- `sample` takes the latest value at an interval;
- `flatMapLatest` cancels the previous inner flow.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

If every value matters, it must not be conflated or cancelled. Instead, optimize
the consumer, use a controlled buffer, or use a reliable queue. An unbounded
buffer can cause memory growth.

**In short:** Flow already provides backpressure through suspending `emit()`.
Add buffering, conflation, or latest operators only when the required data-loss
or waiting policy is known.

</details>

<details>
<summary>77. How do you use Java Streams in Kotlin?</summary>

#### Kotlin

Kotlin/JVM can use the Java Stream API directly:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

For Kotlin collections, standard operators are usually more concise:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Kotlin provides `Sequence` for lazy synchronous processing:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

A stream is appropriate when returned by a Java API, when a Java `Collector` is
required, or when the pipeline is already written in Java. A stream is
single-use.

An I/O stream must be closed:

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Java collectors often have direct Kotlin equivalents:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` uses the common `ForkJoinPool`, resulting in implicit
parallelism and more difficult cancellation. In coroutine code, parallelism is
better controlled explicitly; for I/O, a parallel stream does not replace a
suspend API.

```text
Stream/Sequence -> synchronous one-shot pipeline
Flow            -> asynchronous, suspending, cancellable stream
```

**In short:** use Java Stream for Java interoperability, Kotlin collection
operators for eager processing, `Sequence` for a lazy synchronous pipeline, and
`Flow` for asynchronous data.

</details>

<details>
<summary>78. What is Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` is the former name for the mobile-focused use of `Kotlin Multiplatform`
(`KMP`). The idea is to move shared Kotlin code into a shared module and use it
on Android and iOS.

Teams commonly share:

- domain models;
- use cases;
- validation;
- repository contracts;
- networking and the data layer;
- business rules.

A typical structure is:

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` contains platform-independent code:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Platform-specific APIs are hidden behind interfaces or `expect`/`actual`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

The UI can remain native, using Compose or Views on Android and SwiftUI or UIKit
on iOS. If the team is prepared for it, part of the UI can be shared with
Compose Multiplatform.

**In short:** KMP enables shared business logic for Android and iOS, but
platform APIs, UX details, and integrations must be isolated. It is not one code
base for everything; it is controlled sharing where it genuinely reduces
duplication.

</details>

<details>
<summary>79. What limitations does Kotlin Multiplatform have?</summary>

#### Kotlin

`Kotlin Multiplatform` is useful for shared logic but has important limitations.

1. **Platform APIs are unavailable in `commonMain`**

Shared code cannot directly use `Context`, UIKit, Android-only SDKs, or iOS-only
SDKs. Interfaces or `expect`/`actual` declarations are required.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **Not every library is multiplatform**

A Java- or Android-only library cannot simply be added to `commonMain`. A KMP
alternative or a platform-specific implementation is required.

3. **iOS interoperability has nuances**

Shared Kotlin code is exported to iOS as a framework. Swift APIs, generics,
exceptions, coroutine and Flow bridging, and debugging must be considered.

4. **Sharing UI is not always beneficial**

Teams often share the domain and data layers while keeping the UI native.
Compose Multiplatform is an option, but it is a separate architectural
decision.

5. **The build and ownership model are more complex**

KMP adds source sets, Gradle configuration, CI complexity, and requires Android
and iOS teams to agree on ownership of the shared module.

**In short:** KMP works well for stable shared business logic but does not
eliminate platform-specific code. The main risks are interoperability, library
availability, build complexity, ownership, and moving too much into shared
code.

</details>

<details>
<summary>80. How do you share business logic in KMM?</summary>

#### Kotlin

Business logic is moved into a shared module, while Android and iOS details
remain in platform source sets.

A typical structure is:

```text
shared/
  commonMain   -> domain/use cases/contracts
  androidMain  -> Android implementation
  iosMain      -> iOS implementation
```

Good candidates for `commonMain` include:

- domain models;
- use cases;
- validation;
- repository interfaces;
- API clients;
- serialization;
- business rules.

For example:

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

Platform-specific APIs are provided through DI, interfaces, or
`expect`/`actual`:

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

Networking is often implemented with Ktor Client, JSON with
`kotlinx.serialization`, and state with coroutines and Flow. For iOS, the bridge
to Swift async/await or Combine should be designed from the start.

Do not share everything indiscriminately. Permissions, camera access,
notifications, platform storage, and UX-specific logic are better kept on the
platform or hidden behind a contract.

**In short:** move stable business logic and contracts into the shared module
and implement platform-specific parts separately. Good KMM has a clear
boundary, not a shared monolith.

</details>

<details>
<summary>81. How do you optimize Kotlin code performance?</summary>

#### Kotlin

Optimization starts with measurement, not syntax:

```text
measure -> find bottleneck -> fix -> measure again
```

On Android, use Profiler, Perfetto/System Trace, Macrobenchmark, and allocation
tracking.

The main areas are:

1. **Algorithms and I/O**

The largest gains come from appropriate data structures, fewer network and
database calls, pagination, caching, and database indexes. Elegant Kotlin
syntax cannot fix an `O(n²)` algorithm.

2. **Allocations in hot paths**

In critical code, unnecessary `map`, `filter`, and `copy` calls may create many
objects:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

In ordinary UI code, however, readability is more important than manual
micro-optimization.

3. **Collections and Sequence**

A `Sequence` is useful for lazy chains or early termination but has overhead.
It is not necessarily faster for small lists.

4. **Coroutines**

- use `Dispatchers.IO` for blocking I/O;
- use `Dispatchers.Default` for CPU work;
- do not use `runBlocking` on the main thread;
- cancel obsolete work;
- avoid unnecessary context switches.

5. **UI performance**

In Compose, avoid heavy work during composition, use stable keys, and read state
close to where it is used. With Views, avoid overloading `onBindViewHolder` and
`onDraw`, and avoid unnecessary layout passes.

6. **Memory**

Do not retain an `Activity` or `View` in a singleton. Control cache size,
lifecycle resources, and large bitmaps. Leaks increase GC pressure and the risk
of an out-of-memory error.

**In short:** profile first. Then fix algorithms, I/O, allocations, concurrency,
UI hot paths, and memory leaks. Use `inline`, `Sequence`, and primitive arrays
only where measurements show a benefit.

</details>

<details>
<summary>82. How do you implement retries with exponential backoff in Kotlin?</summary>

#### Kotlin

Exponential backoff retries a transient error with an increasing delay. A retry
policy should define `maxAttempts`, `maxDelay`, jitter, and a timeout, and must
preserve cancellation.

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

An example predicate is:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

For `Flow`, use `retryWhen`:

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

Do not retry validation errors, `403` or `404` responses, authentication
failures without a refresh flow, or non-idempotent operations without an
idempotency key. For `429` and `503`, respect `Retry-After` when available.

**In short:** retries must be bounded and cancellable, include jitter, and have
a clear rule defining retryable errors. Otherwise, they conceal problems and
create additional load.

</details>

<details>
<summary>83. What new Kotlin features should you know in 2026?</summary>

#### Kotlin

In a 2026 interview, it is more important to understand the direction of Kotlin
2.x and its tooling ecosystem than every minor feature.

1. **K2 compiler**

K2 is the new compiler architecture in Kotlin 2.x. It provides faster analysis
and a better foundation for IDE support, compiler plugins, and Multiplatform.
Before upgrading, verify compatibility among the Kotlin Gradle Plugin, AGP,
Gradle, the Compose compiler, KSP or kapt, and serialization.

2. **Context parameters and receivers**

They are useful for DSLs and scoped APIs:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

They are not a replacement for DI, but a way to pass execution context
explicitly.

3. **Explicit backing fields**

They reduce boilerplate for a read-only public API with mutable internal state:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Multiplatform development**

KMP continues to become more practical as Swift export, coroutine and Flow
bridging, Wasm, and Compose Multiplatform evolve. Production decisions must
still consider interoperability, tooling, debugging, and team ownership.

5. **Standard library and JVM tooling**

Follow new standard-library APIs, `kotlin.uuid.Uuid`, JVM target and toolchain
compatibility, the Gradle configuration cache, and build performance.

6. **Adoption rule**

Do not introduce a new feature into production merely because it is new. Check:

- whether it is stable or experimental;
- IDE and CI support;
- plugin compatibility;
- readability;
- the migration and rollback plan.

**In short:** in 2026, understand Kotlin 2.x and K2, context APIs, backing
fields, the development of KMP, Wasm, and Compose Multiplatform, and tooling
constraints. In practice, safely upgrading the ecosystem matters more than
using every new syntax feature.

</details>

<details>
<summary>84. What is the Activity lifecycle?</summary>

#### Kotlin

The `Activity` lifecycle is a sequence of callbacks through which Android
manages the creation, visibility, foreground or background state, and
destruction of a screen.

The main order is:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

When returning from `STOPPED`, `onRestart()` may be called before `onStart()`.

- `onCreate()` creates the UI, navigation, and basic dependencies. Avoid heavy
  synchronous work.
- `onStart()` means the Activity has become visible.
- `onResume()` means the Activity is in the foreground and ready for
  interaction.
- `onPause()` means the Activity has lost focus; this callback must be fast.
- `onStop()` means the Activity is no longer visible; stop resources needed
  only while visible.
- `onDestroy()` means the instance is being destroyed because of `finish()` or
  recreation; it is not guaranteed when the process is killed.

A configuration change may recreate the Activity:

```text
old: onPause -> onStop -> onDestroy
new: onCreate -> onStart -> onResume
```

A `ViewModel` survives a configuration change. Store small UI state in
`SavedStateHandle` or `onSaveInstanceState`, and persistent data in storage.

Flow collection should be bound to the lifecycle:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**In short:** `onCreate` creates the screen, `onStart` makes it visible,
`onResume` makes it active, `onPause` and `onStop` suspend work, and `onDestroy`
ends the instance. State must not depend solely on the Activity instance.

</details>

<details>
<summary>85. What is a Fragment, and how does it differ from an Activity?</summary>

#### Kotlin

An `Activity` is an Android component with its own window and task and an entry
point declared in the manifest. A `Fragment` is a UI controller inside an
Activity or another Fragment, managed by a `FragmentManager`.

```text
Activity -> system component, window, intents, task
Fragment -> part of the UI inside a host, back stack via FragmentManager
```

An Activity example:

```kotlin
class MainActivity : AppCompatActivity()
```

A Fragment example:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

The main practical difference is that a Fragment has a separate lifecycle for
its View:

```text
onCreateView -> onViewCreated -> onDestroyView
```

The Fragment object may remain alive after its View is destroyed. Therefore,
bindings, adapters, and UI subscriptions must be cleared in `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Flow and LiveData used by the UI should be bound to `viewLifecycleOwner`, not
the Fragment's own lifecycle.

Navigation differs as follows:

- use an `Intent` between Activities;
- use `FragmentManager` or the Navigation Component between Fragments.

In a fully Compose-based app, a single Activity without Fragments is often
sufficient. In a hybrid or View-based app, a Fragment remains a valid screen
and navigation abstraction.

**In short:** an Activity is an independent Android component, while a Fragment
is part of the UI inside a host. The key rule is that a Fragment's View has a
shorter lifetime than the Fragment itself, so cleanup belongs in
`onDestroyView()`.

</details>

<details>
<summary>86. What is an Intent, and how is it used?</summary>

#### Kotlin

An `Intent` is an Android message object that describes an action or a specific
component, such as opening an Activity, Service, or Broadcast, or passing data.

1. **Explicit Intent**

It specifies a particular component:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

It is used for internal navigation between Activities.

2. **Implicit Intent**

It describes an action, and Android finds an app that can perform it:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Examples include opening a browser, sharing content, using the dialer or maps,
and selecting a file.

3. **Extras**

Extras pass small amounts of data:

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Prefer passing an `id` instead of large objects. `Parcelable` can be used for
structured arguments, but navigation is often simpler with an ID and a
repository.

4. **Intent filter**

A component declares which intents it accepts:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

This is required for deep links, share targets, and system actions.

5. **Activity Result API**

Use the lifecycle-aware API instead of `startActivityForResult`:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // handle uri
}

launcher.launch("image/*")
```

6. **Security**

- do not pass secrets in extras;
- launch internal components with an explicit intent;
- validate external input;
- configure `android:exported` correctly;
- account for the possibility that an implicit intent has no handler.

**In short:** an explicit intent launches a specific component, while an
implicit intent describes an action for the system. Use extras only for small
data and the Activity Result API for results. External intents require input
validation and security awareness.

</details>

<details>
<summary>87. What happens if you do not call super.onCreate()?</summary>

#### Kotlin

If `super.onCreate(savedInstanceState)` is not called in `Activity.onCreate()`,
the base Android or Jetpack class cannot perform required lifecycle
initialization. This may cause a crash or incorrect Activity behavior.

The correct implementation is:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` prepares:

- lifecycle state;
- saved-state restoration;
- window and decor infrastructure;
- `FragmentManager` in `FragmentActivity`;
- `SavedStateRegistry`;
- `ActivityResultRegistry`;
- AppCompat and ComponentActivity integrations.

Without `super`, possible consequences include:

- a `SuperNotCalledException`;
- a broken lifecycle;
- Fragment problems;
- failure to restore saved state;
- problems with the Activity Result API, Compose, Hilt, or AppCompat.

The same rule applies to Compose:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

You should also generally call `super` in Fragment lifecycle callbacks unless
the documentation explicitly states otherwise.

**In short:** `super.onCreate()` is not a formality; it is part of the framework
contract. Call it at the start of `onCreate()` before UI or framework-dependent
logic.

</details>

<details>
<summary>88. What is View Binding, and why is it better than findViewById?</summary>

#### Kotlin

`View Binding` generates a binding class for an XML layout and provides
type-safe access to Views without manual `findViewById` calls.

The problem with `findViewById`:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Its disadvantages include:

- boilerplate;
- the possibility of specifying the wrong type;
- runtime errors;
- weaker refactoring support.

View Binding in an `Activity`:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

In a `Fragment`, the binding must be cleared in `onDestroyView()` because the
View lifecycle is shorter than the Fragment lifecycle:

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

Benefits of View Binding include:

- type safety;
- less boilerplate;
- compile-time ID checking;
- convenient refactoring;
- nullable fields when a View is absent from a particular layout configuration.

`View Binding` is not the same as `Data Binding`:

```text
View Binding -> access to Views
Data Binding -> binding expressions in XML
```

View Binding is usually unnecessary for a fully Compose-based UI. It remains
relevant for the XML/View System and hybrid screens.

**In short:** View Binding is a safer and more concise replacement for
`findViewById` in XML layouts. The main concern is clearing a Fragment's binding
in `onDestroyView()`.

</details>

<details>
<summary>89. What is LiveData?</summary>

#### Kotlin

`LiveData` is a lifecycle-aware observable data holder from Android Jetpack. It
stores a value and notifies observers only when their `LifecycleOwner` is in the
`STARTED` or `RESUMED` state.

A typical `ViewModel` example:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

Expose `LiveData`, not `MutableLiveData`, so the UI cannot modify state directly.

Observation in a Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` is important because a Fragment's View has a shorter
lifetime than the Fragment itself.

`value` versus `postValue`:

```kotlin
_liveData.value = user      // main thread
_liveData.postValue(user)   // background thread, asynchronous
```

Benefits include:

- lifecycle awareness;
- a simple API;
- good integration with the XML/View System;
- automatic removal of a destroyed owner's observer;
- suitability for legacy Android code.

Limitations include:

- dependency on Android;
- a weaker stream API than `Flow`;
- poor suitability for KMP;
- one-shot events often leading to `SingleLiveEvent`-style workarounds.

Comparison:

```text
LiveData  -> Android lifecycle-aware holder
StateFlow -> Kotlin hot stream with a state value
```

New coroutine-first or Compose code more commonly uses `StateFlow` with
`collectAsStateWithLifecycle()`. `LiveData` remains appropriate in legacy
View-based projects or projects where it is already the standard.

**In short:** LiveData is an Android observable state holder that respects the
lifecycle. `StateFlow` is usually preferable for new Kotlin and Compose code,
but LiveData still works well with the XML/View System.

</details>

<details>
<summary>90. What is Room, and how does it work?</summary>

#### Kotlin

`Room` is a Jetpack library for working with SQLite through a type-safe API. It
does not eliminate SQL; it provides compile-time checks, DAOs, migrations, and
convenient integration with coroutines and Flow.

Its main parts are:

```text
@Entity   -> table
@Dao      -> SQL access methods
@Database -> database entry point
```

An entity:

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val age: Int
)
```

A DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
}
```

The database:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Creating it:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room supports `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

When the table changes, Room invalidates the query and emits new data.

Relations often require `@Transaction`:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Schema changes are implemented through migrations:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Do not use destructive migration indiscriminately in production when user data
exists.

In practice, Room should be hidden behind a repository:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

The UI should not depend directly on database entities.

**In short:** Room is a type-safe wrapper around SQLite based on `Entity`, `Dao`,
and `Database`. It supports suspend functions, Flow, relations, and migrations.
In a sound architecture, Room belongs in the data layer behind a repository,
not directly in the UI.

</details>

<details>
<summary>91. What do the @Query, @Insert, and @Delete annotations do?</summary>

#### Kotlin

`@Query`, `@Insert`, and `@Delete` are Room annotations for DAO methods. Room
generates the implementation and validates SQL and mappings at compile time.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

It executes explicit SQL:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` and `:name` are bind parameters. Do not concatenate user input into an
SQL string.

`@Query` is not limited to `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

The returned `Int` is the number of affected rows.

### `@Insert`

It inserts an `@Entity`:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` defines behavior for primary- or unique-key conflicts: `ABORT`,
`IGNORE`, or `REPLACE`. Use `REPLACE` carefully because SQLite may effectively
delete the old row and insert a new one. For insert-or-update behavior,
`@Upsert` is often preferable when available in the project.

### `@Delete`

It deletes an entity by its primary key:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

For deletion by ID or another condition, prefer `@Query("DELETE ...")`.

### `@Transaction`

Use `@Transaction` when several DAO operations must be atomic:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room validates tables, columns, bind parameters, and return mappings. The
developer must still verify performance, indexes, and business logic.

**In short:** `@Query` executes SQL, `@Insert` inserts an entity, and `@Delete`
deletes an entity by its primary key. Room generates DAO code and catches many
errors at compile time, but it does not replace query-performance analysis.

</details>

<details>
<summary>92. What is WorkManager, and when should you use it?</summary>

#### Kotlin

`WorkManager` is a Jetpack API for persistent, deferrable background work. A
task may start later but should survive app closure, process death, and, when
necessary, a device reboot.

Use it for:

- background synchronization;
- uploads or downloads with retries;
- cleanup;
- dispatching logs or analytics;
- periodic maintenance;
- work with constraints such as network, charging, battery, or storage state.

One-time work:

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

A `CoroutineWorker`:

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

Possible results are:

- `Result.success()` means the task completed;
- `Result.retry()` schedules another attempt with a backoff policy;
- `Result.failure()` represents a terminal failure.

Important capabilities include:

- constraints;
- retries and backoff;
- unique work through `enqueueUniqueWork()`;
- work chaining;
- periodic work without an exact execution time.

Input and output `Data` must remain small. Store large payloads in a database or
file and pass an ID.

WorkManager is unsuitable for:

- exact alarms;
- a real-time socket connection;
- media playback;
- continuous location tracking;
- an action the user expects immediately.

Use a foreground service for ongoing user-visible work. Use AlarmManager or the
Exact Alarm API when exact timing is genuinely required and permitted.

**In short:** WorkManager is intended for reliable deferred background work
with constraints and retries. It does not guarantee an exact start time and
does not replace a foreground service, real-time connection, or exact alarm.

</details>

<details>
<summary>93. What is the difference between CoroutineScope, lifecycleScope, and viewModelScope?</summary>

#### Kotlin

A `CoroutineScope` defines the context and lifetime of coroutines.
`lifecycleScope` and `viewModelScope` are ready-made Android scopes with defined
owners.

```text
CoroutineScope  -> you define its lifetime
lifecycleScope  -> lives until its LifecycleOwner reaches DESTROYED
viewModelScope  -> lives until ViewModel.onCleared()
```

### `CoroutineScope`

This is a general coroutine scope:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

If you create a custom scope, you must explicitly call `cancel()` at the correct
time. Otherwise, it may leak or continue unnecessary work after its owner dies.

### `lifecycleScope`

It belongs to an `Activity`, `Fragment`, or another `LifecycleOwner` and is
cancelled at `DESTROYED`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`lifecycleScope.launch` itself does not pause in `STOPPED`, so Flow collection
requires `repeatOnLifecycle`.

In a Fragment, use `viewLifecycleOwner.lifecycleScope` for work involving the
View or binding:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

This is necessary because the Fragment object may outlive its View.

### `viewModelScope`

It belongs to a `ViewModel`, survives configuration changes, and is cancelled
in `onCleared()`:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Use it for screen logic, loading, business calls, and producing UI state.

Common mistakes include:

- using `GlobalScope` for screen work;
- creating a custom scope without cancellation;
- collecting binding or state in the Fragment scope instead of the View
  lifecycle scope;
- launching business operations from an Activity instead of a ViewModel;
- accessing a binding after `onDestroyView()`.

**In short:** use `viewModelScope` for screen logic in a ViewModel,
`lifecycleScope` for UI collection and effects, and a custom `CoroutineScope`
only when it has a clear lifetime and a defined place to call `cancel()`.

</details>

<details>
<summary>94. How is dependency injection implemented in Kotlin?</summary>

#### Kotlin

Dependency injection means providing dependencies from outside rather than
creating them inside a class. Constructor injection is the default approach in
Kotlin.

Correct:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Incorrect:

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

The second approach tightly couples the class to an implementation and makes
testing harder.

A use case usually depends on a contract:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Manual DI

A simple container is sufficient for a small project:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Its advantage is simplicity. Its disadvantage is that a large graph is
difficult to maintain manually.

### Hilt/Dagger

Compile-time DI is a common choice for large Android apps:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val getUser: GetUserUseCase
) : ViewModel()
```

Binding an interface:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface UserModule {
    @Binds
    fun bindUserRepository(impl: UserRepositoryImpl): UserRepository
}
```

The advantage is that graph errors are caught at compile time. The disadvantage
is additional boilerplate and more complex code generation.

### Koin

Koin provides runtime DI through a Kotlin DSL:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

It is easier to start with, but some errors may appear only at runtime.

DI also manages lifetimes such as `singleton`, `factory`, and ViewModel scopes.
Choose scopes carefully to avoid leaks or unnecessary singletons.

**In short:** dependency injection in Kotlin usually uses constructor
injection. Manual DI suits a small graph, Hilt or Dagger suits large Android
apps, and Koin prioritizes simplicity. DI reduces coupling and makes testing
significantly easier.

</details>

<details>
<summary>95. What is Hilt, and how does it work?</summary>

#### Kotlin

`Hilt` is an Android dependency-injection framework built on top of Dagger. It
generates a dependency graph and a ready-made component hierarchy for
`Application`, `Activity`, `Fragment`, `ViewModel`, `View`, and `Service`.

Basic setup:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` creates the root graph at the application level.

An Android class that requires injected dependencies:

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Constructor injection is the primary approach:

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

A ViewModel:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Modules are required when Hilt cannot create a dependency itself, such as an
interface, third-party class, builder, or factory.

Use `@Binds` for an interface-to-implementation binding:

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

Use `@Provides` for creation through a factory or builder, such as
`OkHttpClient`, `Retrofit`, or Room.

Scopes define an object's lifetime:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

The main components are:

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

An important rule is that a long-lived component cannot depend on a shorter-
lived one. For example, a `@Singleton` dependency must not retain an `Activity`
context.

For tests, Hilt allows modules to be replaced with `@TestInstallIn`. In unit
tests, however, directly passing a fake through the constructor is often
simpler.

**In short:** Hilt generates a Dagger graph for Android. `@Inject` creates
dependencies, `@Binds` and `@Provides` define bindings, `@AndroidEntryPoint`
enables injection in Android classes, and scopes control lifetimes.

</details>

<details>
<summary>96. What is @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` integrates a `ViewModel` with Hilt. Hilt generates a
`ViewModelProvider.Factory` and supplies dependencies through constructor
injection, so a manual factory is usually unnecessary.

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

The rules are:

- place `@HiltViewModel` on the class;
- place `@Inject constructor` on the constructor;
- every dependency must be present in the Hilt graph;
- Hilt provides `SavedStateHandle` automatically;
- the host `Activity` or `Fragment` must have `@AndroidEntryPoint`.

Obtaining it in a Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

For a ViewModel shared at the Activity level:

```kotlin
private val viewModel: UserViewModel by activityViewModels()
```

In Compose:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

The ViewModel scope is determined by its `ViewModelStoreOwner`: a Fragment,
Activity, or navigation back-stack entry.

ViewModel dependencies live in `ViewModelComponent`. `@ViewModelScoped`
provides one instance for a particular ViewModel, while `@Singleton` provides
one instance for the application graph.

Common mistakes include:

- forgetting `@HiltViewModel` or `@Inject constructor`;
- forgetting `@AndroidEntryPoint` on the host;
- constructing the ViewModel manually;
- expecting the same instance with different `ViewModelStoreOwner` values;
- injecting an `Activity`, `Fragment`, `View`, or short-lived `Context` into a
  ViewModel.

**In short:** `@HiltViewModel` lets Hilt create a ViewModel through a generated
factory and inject constructor dependencies and `SavedStateHandle`. The actual
lifetime is determined by the `ViewModelStoreOwner`, not the annotation itself.

</details>

<details>
<summary>97. How does @Inject work?</summary>

#### Kotlin

`@Inject` tells Dagger or Hilt how to create an object or where to insert a
dependency.

The primary approach is constructor injection:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt can create `LoadUserUseCase` if the graph contains a binding for every
constructor parameter. A missing or ambiguous binding produces a compile-time
error.

Constructor injection is preferable for classes you own because:

- dependencies are explicit;
- properties can use `val`;
- the class is easy to test;
- there is no hidden framework magic.

Field injection is used mainly for Android classes created by the framework:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Such a field must not be used before injection occurs in the component's
lifecycle.

`@Inject constructor` does not define an interface binding by itself. An
interface requires `@Binds`:

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

Use `@Provides` for third-party classes or builders:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` does not define a lifetime. Scopes such as `@Singleton`,
`@ViewModelScoped`, and `@ActivityScoped` define lifetimes. Use qualifiers when
multiple bindings have the same type.

Hilt and Dagger generate factories and the graph at compile time without using
runtime reflection to create dependencies.

**In short:** `@Inject` tells Hilt or Dagger how to create a class or where to
insert a dependency. Constructor injection is the default; use `@Binds` for
interfaces, `@Provides` for external objects, scopes for lifetimes, and
qualifiers to distinguish identical types.

</details>

<details>
<summary>98. What are scopes in Hilt?</summary>

#### Kotlin

A Hilt scope determines the component within which a dependency is cached. All
requests within the same component instance receive the same object.

Without a scope, a binding is usually created again for every injection request.
This is a reasonable default for inexpensive stateless objects.

A simplified component hierarchy:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

A child can access parent bindings, but a parent cannot depend on a shorter-
lived child binding.

The main scopes are:

- `@Singleton`: one instance per application graph;
- `@ActivityRetainedScoped`: one instance per logical Activity, surviving
  configuration changes;
- `@ViewModelScoped`: one instance per specific ViewModel;
- `@ActivityScoped`: one instance per specific Activity instance;
- `@FragmentScoped`: one instance per specific Fragment;
- `@ViewScoped`: one instance per Hilt-enabled View;
- `@ServiceScoped`: one instance per specific Service.

The scope must match the `@InstallIn` component:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

A `@ViewModelScoped` dependency is shared within one ViewModel but differs
between ViewModels. Use `@ActivityRetainedScoped` when an object must be shared
among ViewModels belonging to the same Activity.

A common mistake is allowing a long-lived object to retain a short-lived
context:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

This is invalid because the singleton retains the Activity after destruction.
A `@Singleton` should use application-level state or context.

Do not scope everything. A scope expresses ownership and lifecycle semantics,
not merely an optimization.

**In short:** a Hilt scope defines a dependency's lifetime and reuse within a
component. It must match the real owner and must not retain objects with shorter
lifecycles.

</details>

<details>
<summary>99. How do you prevent memory leaks in Android?</summary>

#### Kotlin

A memory leak occurs when an object is no longer needed but is still held by a
strong reference. In Android, leaked objects commonly include an `Activity`,
`Fragment`, `View`, `Context`, callback, or coroutine.

### Do not retain an Activity or View in long-lived objects

Incorrect:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

If this is an `Activity` context, the Activity cannot be reclaimed after a
rotation or `finish()`. Use `applicationContext` for application-level
dependencies.

### Clear View Binding in a Fragment

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

The Fragment object may outlive its View, so View references must be cleared in
`onDestroyView()`.

### Clear adapters, listeners, and callbacks

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Adapters, listeners, and anonymous callbacks often retain a View or Fragment
reference. Unregister or clear them in the correct lifecycle callback.

### Use lifecycle-aware coroutines

For Fragment UI collection:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Do not launch UI coroutines in a scope that outlives the View. Do not use
`GlobalScope` for UI or application flows.

### Compose

Clean up Compose side effects with `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

Do not retain an `Activity` or `View` with `remember` without a clear lifecycle
reason.

### Tools

- LeakCanary;
- Android Studio Memory Profiler;
- heap dumps;
- allocation tracking.

A `WeakReference` is not a normal architectural solution. If it appears
necessary, first examine ownership and lifecycle.

**In short:** do not retain short-lived Android objects in long-lived classes;
clear Fragment View references in `onDestroyView()`, use lifecycle-aware
coroutines, unregister callbacks, and detect leaks with LeakCanary or Profiler.

</details>

<details>
<summary>100. How do you store API keys securely in an Android app?</summary>

#### Kotlin

It is impossible to store a secret with complete security in an Android app.
Anything included in an APK or AAB can potentially be extracted through
decompilation, a proxy, memory inspection, or a rooted device.

The main rule is that real secrets do not belong in the client.

Incorrect:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

If a key grants access to a backend, administrative operations, payments, or a
service account, it must exist only on the server.

The correct architecture is:

```text
Android app -> your backend -> third-party API
```

The backend adds the secret, validates the user and session, applies rate
limits and auditing, and returns only the required result.

Not all keys are equivalent:

```text
public/client key -> may be included in the app but must be restricted
secret/admin key  -> must not be included in the app
```

If a client key is included in the app, restrict it by:

- package name;
- SHA-256 certificate fingerprint;
- allowed APIs;
- quotas and rate limits;
- environment separation;
- monitoring;
- a rotation plan.

`BuildConfig`, `local.properties`, and CI secrets help prevent committing a key
to Git, but they do not protect it in the built app:

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

The key remains visible after decompilation.

The NDK, R8 or ProGuard, and string obfuscation only make extraction harder.
They add friction but do not form a security boundary.

Android Keystore is useful for device-generated keys and user tokens, but it
cannot protect a hardcoded API secret because an attacker can find where the app
uses it.

Store user access and refresh tokens carefully:

- use EncryptedSharedPreferences or encrypted DataStore;
- use Android Keystore for the cryptographic key;
- keep access tokens short-lived;
- rotate refresh tokens;
- clear storage on logout;
- support revocation on the backend.

**In short:** real API secrets must live on the backend. Only restricted public
or client keys may exist in the app. `BuildConfig`, obfuscation, and the NDK do
not guarantee security; restrictions, monitoring, and rotation are required.

</details>

<details>
<summary>101. How do you implement an offline-first architecture?</summary>

#### Kotlin

Offline-first means that the UI reads from a local source of truth while the
network only synchronizes it.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Read path

The UI observes Room:

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

A refresh retrieves remote data and merges it into the database transactionally:

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

After Room is updated, the UI updates through `Flow`. `replaceAll` is dangerous
when the response is partial or pending local edits exist.

### Write path and outbox

An offline write is first stored locally:

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

The UI immediately sees the optimistic update. The outbox operation is sent
later and removed only after server acknowledgement.

The local update and outbox insertion must occur in the same transaction;
otherwise, process death may lose the operation.

### Synchronization

Use WorkManager for retry and process-death resilience:

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

Synchronization must be idempotent because a retry may repeat a request.
Foreground refresh and the worker should use the same synchronization
implementation.

### Conflicts

The conflict policy depends on the domain:

- server wins;
- client wins;
- revision or ETag;
- field-level merge;
- manual resolution.

A client timestamp is unreliable because of clock skew. For critical domains,
simple last-write-wins behavior is often unacceptable.

### UI state

The UI should distinguish between:

- cached data;
- initial loading without local data;
- refreshing while displaying old data;
- offline status;
- pending or failed changes;
- the last successful synchronization.

Connectivity is only a hint. The source of truth is the result of an actual
request.

**In short:** offline-first architecture is built around a local source of
truth. Reads come from Room, remote data is merged into the database, writes use
a transactional outbox, WorkManager performs synchronization, and the domain
defines the conflict policy.

</details>

<details>
<summary>102. What is MVI architecture, and when should you use it?</summary>

#### Kotlin

MVI is an architectural approach based on unidirectional data flow:

```text
UI -> Event/Intent -> ViewModel/Reducer -> State -> UI
                                      └-> Effect
```

The UI does not modify state directly. It renders an immutable `State` and sends
an `Event`. The ViewModel handles the event and produces a new state.

Typical parts include:

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

- `State` is the durable screen model;
- `Event` or `Intent` is a user action or system input;
- `Reducer` or `Handler` performs the `old state -> new state` transition;
- `Effect` is a one-time action such as navigation, a snackbar, or a permission
  request.

A ViewModel:

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

State is updated with `copy` and `update`:

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

Compose UI only reads state and sends events:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Advantages include:

- one source of truth for screen state;
- predictable transitions;
- easier reducer and ViewModel testing;
- good compatibility with Compose;
- convenient event and state logging.

Disadvantages include:

- more boilerplate;
- the reducer may become a god object;
- one-shot effects require a clear delivery policy;
- MVI does not automatically solve concurrency or races;
- it may be excessive for simple CRUD.

MVI is appropriate for complex screens with many state transitions, retries,
optimistic updates, validation, or multi-step flows. A simple screen may need
only a `ViewModel`, `StateFlow`, and methods.

**In short:** MVI is the `Event -> State transition -> Render` cycle. It is
useful for complex stateful UI but requires discipline around effects and
concurrency and may be overkill for simple screens.

</details>

<details>
<summary>103. How do you design scalable Android applications?</summary>

#### Kotlin

A scalable Android application allows a feature to be changed locally without
cascading changes throughout the codebase.

The basic layers are:

```text
UI -> ViewModel -> domain -> repository -> API/DB
```

- the UI renders state;
- the ViewModel manages screen state and events;
- the domain layer contains business rules;
- the repository hides data sources;
- the data layer works with Retrofit, Room, and SDKs.

Do not add layers mechanically. A use case or interface is appropriate when
there is a business rule, boundary, alternative implementation, or distinct
testing requirement.

### Feature-first modules

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

A feature module groups code that changes together. `core` should contain truly
shared infrastructure, not arbitrary utilities.

The dependency rule is:

```text
app -> feature
feature -> core
core -X-> feature
```

A feature should not depend on another feature's implementation. Prefer
contracts, navigation APIs, or an API/implementation split for integration.

### Contracts and models

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Create an interface at a real boundary, not for every class as a precaution.

Separate DTOs, entities, domain models, and UI models when they have different
semantics or lifecycles. If models are identical and do not have different
reasons to change, unnecessary mapping is harmful.

### State, errors, and concurrency

The UI should receive one observable state and send events. The ViewModel must
define an explicit concurrency policy: cancel the previous operation, ignore a
duplicate, serialize, or merge.

A raw `Throwable` should not be part of the UI contract. Convert infrastructure
errors into domain or UI outcomes at a boundary.

### DI, tests, and builds

- use constructor injection by default;
- align scopes with lifecycle owners;
- keep domain and unit tests independent of Android;
- test ViewModel state;
- add repository integration tests;
- use convention plugins and a version catalog;
- run affected-module CI;
- provide logging, metrics, and crash context;
- define migration plans for API and database changes.

Modularity without tests, observability, and clear ownership does not scale a
product.

**In short:** scalability comes from feature ownership, a controlled dependency
graph, stable contracts, testable boundaries, explicit state and concurrency,
and production support. Add modules and layers only when they genuinely isolate
change.

</details>

<details>
<summary>104. What is Jetpack Compose, and how does it differ from the View System?</summary>

#### Kotlin

`Jetpack Compose` is a declarative UI toolkit for Android. UI is described with
Kotlin `@Composable` functions as a result of the current state, without XML or
manual View updates.

The View System is imperative:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

Compose is declarative:

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

When state changes, Compose recomposes the relevant parts of the UI.

A composable function:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

A composable should render state and invoke callbacks. Business logic, I/O, and
long-running operations do not belong in UI functions.

A typical structure is:

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

The ViewModel produces state, and Compose displays it.

The main differences are:

```text
View System -> XML + View tree + manual updates + listeners
Compose     -> Kotlin functions + state-driven rendering + recomposition
```

Important Compose details include:

- a composable body may execute many times;
- side effects must use effect APIs;
- local state and objects should be cached with `remember` when necessary;
- screen state belongs in a ViewModel;
- lazy lists should have stable keys;
- performance should be measured with a profiler, not by counting
  recompositions.

Compose and the View System can be mixed:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

A View can also be embedded in Compose with `AndroidView`, enabling gradual
migration.

**In short:** Compose describes UI as a function of state and updates it through
recomposition. The View System uses XML, a View tree, and manual View updates.
Compose supports state-driven UI well but requires discipline around side
effects and state ownership.

</details>

<details>
<summary>105. What are @Composable and recomposition?</summary>

#### Kotlin

`@Composable` marks Jetpack Compose functions that describe UI. Recomposition
is the repeated execution of a composable when state it depends on changes.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

A composable is a function of state:

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

When `state` changes, Compose updates the required part of the UI through
recomposition.

An example of local state:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

Changing `count` triggers recomposition of the UI that reads it.

Recomposition can be triggered by:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable`;
- changes to composable parameters.

A composable body may execute many times, so it must not perform side effects:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // Incorrect
}
```

The correct approach is:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` retains a value between recompositions:

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

Recomposition itself is not a problem. Problems arise from heavy work in a
composable, reading state too broadly, unstable parameters, unnecessary
allocations, or lazy lists without stable keys.

Practical rules include:

- keep composables lightweight and free of side effects;
- read state close to where it is used;
- cache expensive objects with `remember`;
- launch effects through `LaunchedEffect`, `DisposableEffect`, and similar
  APIs;
- use stable keys for lists;
- measure performance with a profiler instead of guessing.

**In short:** `@Composable` describes UI as a function of state. Recomposition
re-executes the required composables when state changes. This is Compose's
normal mechanism when UI functions are lightweight, side-effect free, and read
state at the appropriate level.

</details>

<details>
<summary>106. What is state hoisting?</summary>

#### Kotlin

`State hoisting` moves state out of a composable into a parent or state holder.
The child receives a value and a callback, making it stateless, reusable, and
testable.

A stateful version:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

After hoisting:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

The parent becomes the source of truth:

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

For screen-level state, the owner is usually a `ViewModel`:

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

The Compose route reads state and forwards events:

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

Hoisting supports unidirectional data flow:

```text
state flows down -> UI
events flow up   -> owner updates state
```

State should be hoisted to the lowest common owner that needs to read or modify
it. If state is needed only inside a component and does not affect business
logic, it may remain local.

A typical stateless composable API is:

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**In short:** state hoisting means state flows down and events flow up. The child
does not own state; a parent or ViewModel becomes the single source of truth.

</details>

<details>
<summary>107. What is the difference between remember and rememberSaveable?</summary>

#### Kotlin

`remember` and `rememberSaveable` preserve values across recompositions. The
difference is that `rememberSaveable` additionally stores the value in saved
state and can restore it after a configuration change or process recreation.

```text
remember          -> survives recomposition
rememberSaveable  -> survives recomposition + recreation
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

After Activity recreation, the value is lost.

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

On Android, types that can be put into a saved-state `Bundle` are supported
automatically: primitives, `String`, `Parcelable`, `Serializable`, and so on. A
custom type requires a `Saver`:

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

When to use what:

- `remember` — transient UI state or a cached object that can be lost;
- `rememberSaveable` — small UI state the user expects to keep: input text,
  selected tab, filter;
- `ViewModel` — screen/business state;
- storage/DataStore/DB — persistent data.

Limitations of `rememberSaveable`:

- saved state has a limited size;
- large objects/collections should not be stored;
- it is not long-term persistence;
- it does not replace a `ViewModel` or repository.

**In short:** `remember` lives only in the composition. `rememberSaveable`
restores small saveable UI state after recreation. Screen state belongs in a
`ViewModel`; long-term data belongs in a storage layer.

</details>

<details>
<summary>108. What is rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` is a Compose API that returns a `CoroutineScope`
bound to the current composition. The scope is cancelled when the composable
leaves the composition.

It is used when a suspend UI action needs to be launched from a callback: click,
swipe, drawer, snackbar, or scroll animation.

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

A `suspend` function cannot be called directly from `onClick`, so a scope is
needed.

Another example:

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

Difference from `LaunchedEffect`:

```text
LaunchedEffect         -> coroutine starts automatically on enter/key change
rememberCoroutineScope -> coroutine starts manually from a callback
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

Do not use `rememberCoroutineScope()` for business logic:

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

And in the ViewModel:

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

`GlobalScope` is not used for UI because it is not tied to the lifecycle and may
continue running after the screen is closed.

**In short:** `rememberCoroutineScope()` is needed for short suspend UI
operations from callbacks: snackbar, drawer, scroll. For automatic effects, use
`LaunchedEffect`; for screen/business logic, use `viewModelScope`.

</details>

<details>
<summary>109. What is collectAsState?</summary>

#### Kotlin

`collectAsState()` is a Compose API that collects a `Flow` and converts it into
Compose `State`. When the Flow emits a new value, Compose triggers recomposition
where that state is read.

ViewModel:

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

Mechanism:

```text
Flow emit -> State update -> recomposition -> UI update
```

A regular `Flow` needs an initial value:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

For `StateFlow`, the initial value is already available in `state.value`.

On Android, it is better to use the lifecycle-aware variant:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Difference:

```text
collectAsState              -> Compose runtime, without Android lifecycle
collectAsStateWithLifecycle -> collects only in an active lifecycle state
```

You should not collect a Flow directly in the composable body:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // bad
}
```

For screen state, use `collectAsStateWithLifecycle()`. For one-shot events, use
`LaunchedEffect` and a separate `SharedFlow`:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigate or show snackbar
    }
}
```

`State` is something that can be rendered again. An `Event` is a one-time action.

If a Flow emits too often, reduce the noise: `distinctUntilChanged()`, smaller
state models, local state reads, and no heavy work inside composables.

**In short:** `collectAsState()` converts Flow into Compose State. On Android,
`collectAsStateWithLifecycle()` is usually used. One-shot navigation/snackbar
events need `LaunchedEffect`, not state collection.

</details>

<details>
<summary>110. What is Modifier in Compose?</summary>

#### Kotlin

In Compose, `Modifier` describes layout, drawing, input, semantics, and
composable behavior: `padding`, `size`, `background`, `clickable`, `scroll`,
accessibility, and so on.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

`Modifier` is immutable and is built as a chain. Order matters:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

and:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

produce different results because the background is applied at a different
stage.

A reusable composable should accept a modifier from the outside:

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

This lets the parent control the child's positioning and external behavior.

Main modifier types:

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

For accessibility, it is better to use high-level modifiers such as `clickable`
because they add semantics. When needed, semantics can be defined explicitly:

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Some modifiers work only in a parent scope:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Practical rules:

- accept `modifier: Modifier = Modifier`;
- the parent is responsible for external positioning;
- the child can add internal modifiers after the passed modifier;
- modifier order is important;
- do not put heavy logic in modifier callbacks.

**In short:** `Modifier` is an immutable chain of composable configuration. It
controls layout, drawing, input, and semantics. Call order matters, and reusable
composables should accept a `modifier` parameter.

</details>

<details>
<summary>111. What is the difference between padding and offset?</summary>

#### Kotlin

`padding` and `offset` both change UI positioning, but they work at different
stages of layout.

```text
padding -> changes measurement and reserved space
offset  -> changes placement without changing reported size
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

`padding` adds space around the content and affects the size seen by the parent
and siblings.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` will be placed after `A` together with its padding.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` moves the composable during placement, but the parent reserves the old
space.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` will not move away, so elements may overlap. This is useful for badges,
dragging, animation, or decorative shifts.

### RTL

`offset(x, y)` respects layout direction: positive `x` moves toward the logical
end. If physical coordinates are needed without RTL mirroring, use
`absoluteOffset()`.

### Modifier order

Order affects layout, drawing, and hit area:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

The background covers a larger area.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

The background appears only after the padding.

The same applies to `clickable`: if `clickable` comes before `padding`, the
padding is part of the click area; if it comes after, it is not.

**In short:** `padding` adds layout space and affects siblings. `offset` only
moves the composable without adding new reserved space. Modifier order has a
critical effect on appearance and hit testing.

</details>

<details>
<summary>112. What is the difference between fillMaxSize and matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` and `matchParentSize()` can both make a child the size of its
parent, but they work differently.

```text
fillMaxSize()     -> takes maximum constraints and affects measurement
matchParentSize() -> repeats the already determined Box size
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

`fillMaxSize()` asks the parent for the maximum available size. If the
constraints are limited by the screen, the composable takes the whole screen.

For a single axis, there are:

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` participates in measurement and can affect the size of the
parent layout.

### `matchParentSize()`

`matchParentSize()` works only in `BoxScope` and only for a direct child of
`Box`:

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

Such a child repeats the already calculated size of the `Box`, but does not
define it.

Simplified:

1. `Box` measures normal children;
2. it determines its own size;
3. children with `matchParentSize()` receive that ready size.

Therefore, `matchParentSize()` is well suited for overlays: loading layers,
scrims, gradients, and clickable layers.

If the overlay is replaced with `fillMaxSize()`, the child may request maximum
constraints and enlarge the `Box`. That is different behavior.

Practical choice:

- the root/container should occupy all available space — `fillMaxSize()`;
- an overlay should cover a content-sized `Box` — `matchParentSize()`;
- a child should not affect the `Box` size — `matchParentSize()`;
- outside `BoxScope`, `matchParentSize()` is unavailable.

**In short:** `fillMaxSize()` asks for the maximum available space and
participates in measurement. `matchParentSize()` in `BoxScope` only repeats the
already determined `Box` size and does not enlarge it.

</details>

<details>
<summary>113. How do you handle the Back button in Compose?</summary>

#### Kotlin

In Compose, back press is handled with `BackHandler` from
`androidx.activity.compose`. It intercepts system back inside a composable.

Basic example:

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // content
}
```

`BackHandler` can be enabled conditionally:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

If `enabled = false`, the back event goes further: to another handler or to
Navigation.

A typical scenario is a confirmation dialog for unsaved changes:

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

With `NavController`, the standard back behavior often works automatically via
the back stack. A custom `BackHandler` is needed when standard behavior must be
changed: dialog, bottom sheet, selection mode, or unsaved changes.

For local UI state, back should close the top layer first:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

The priority is:

```text
dialog/sheet/selection -> unsaved changes -> screen navigation
```

For complex logic, it is better to pass the event to the ViewModel:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

The ViewModel decides whether to show a dialog, save a draft, allow navigation,
or emit an effect.

Common mistakes:

- intercepting back always;
- not using `enabled`;
- duplicating logic between `BackHandler` and `NavController`;
- ignoring dialog/sheet state;
- keeping back logic far from the state it handles.

**In short:** in Compose, back press is handled with
`BackHandler(enabled = ...)`. It should be used for top UI layers and custom
behavior, while regular navigation is often left to `NavController`.

</details>

<details>
<summary>114. What is rememberUpdatedState used for?</summary>

#### Kotlin

`rememberUpdatedState()` gives a long-lived effect the latest value or callback
without restarting the effect.

Problem:

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` does not restart if `onTimeout` changes. The coroutine may
call a stale lambda.

Solution:

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

The timer does not start over, but after the delay the latest callback is called.

Main rule:

```text
should a value change restart the effect? -> key
should a value change not restart the effect? -> rememberUpdatedState
```

If the callback is used as a key:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

then when the lambda changes, the previous coroutine is cancelled and the timer
starts again. That is not always desired.

Example with `DisposableEffect`:

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

The observer is re-registered only when `owner` changes, but it calls the latest
handler.

When a value should be a key:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

A `userId` change means a new operation, so the old one must be cancelled and a
new one started.

`rememberUpdatedState()` does not launch a coroutine, does not replace effect
keys, and does not save a value after leaving the composition. It simply returns
`State<T>` with the latest value for a long-lived effect.

**In short:** `rememberUpdatedState()` is needed so an effect sees the latest
value/callback without restarting. If a value change means a new operation, it
should be a key, not `rememberUpdatedState`.

</details>

<details>
<summary>115. What are side effects in Compose, and how do you handle them?</summary>

#### Kotlin

A side effect in Compose is an action outside pure UI rendering: a coroutine,
snackbar, navigation, analytics, listener, lifecycle observer, or synchronization
with an external API.

The composable body can run many times, so a side effect must not be launched
directly:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // bad
}
```

Effect APIs exist for this.

### `LaunchedEffect`

Launches a coroutine tied to the composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

If `userId` changes, the old coroutine is cancelled and a new one starts.

### `rememberCoroutineScope`

Needed for suspend UI actions from callbacks:

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

Suitable for snackbar, drawer, bottom sheet, and scroll animation.

### `DisposableEffect`

For listeners/observers with cleanup:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

If you subscribe, unsubscribe in `onDispose`.

### `SideEffect`

Runs after a successful recomposition:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Not suitable for suspend work.

### Other APIs

- `rememberUpdatedState` — latest value/callback inside a long-lived effect
  without restart;
- `produceState` — async source into Compose `State`;
- `snapshotFlow` — Compose state into Flow.

Practical map:

```text
composition coroutine      -> LaunchedEffect
coroutine from callback    -> rememberCoroutineScope
listener with cleanup      -> DisposableEffect
after recomposition sync   -> SideEffect
latest callback no restart -> rememberUpdatedState
async source to State      -> produceState
Compose state to Flow      -> snapshotFlow
```

**In short:** side effects are not launched in the composable body. Use effect
APIs for them so lifecycle, restart, cancellation, and cleanup are controlled
explicitly.

</details>

<details>
<summary>116. What are keys in LaunchedEffect, and why are they important?</summary>

#### Kotlin

Keys in `LaunchedEffect` define the lifecycle of the coroutine effect: when to
start it, cancel it, and restart it. If a key changes, Compose cancels the old
coroutine and starts a new one.

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

Behavior:

- the composable enters the composition — the effect starts;
- recomposition without changing `userId` — the effect does not restart;
- `userId` changes — the old coroutine is cancelled, a new one starts;
- the composable leaves the composition — the coroutine is cancelled.

A common mistake:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

If `userId` changes, the effect will not restart and you can get stale data. The
correct version:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` is fine when the effect should start once when entering
the composition:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigation/snackbar
    }
}
```

Multiple keys:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

The effect restarts if at least one key changes. Everything that determines the
result of the side effect should be a key.

Do not use unstable keys:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

This makes the effect restart uncontrollably.

If the latest value is needed inside the effect without restarting it, use
`rememberUpdatedState`:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Practical rule:

```text
a value change should restart the effect -> key
latest value is needed without restart   -> rememberUpdatedState
```

**In short:** keys in `LaunchedEffect` control starting, cancelling, and
restarting the coroutine. Incorrect keys cause stale data, unnecessary restarts,
or unstable UI behavior.

</details>

<details>
<summary>117. What is snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` is a Compose API that converts reads from Compose snapshot state
into a cold `Flow`. It is used when changes in Compose state need to be handled
through Flow operators or a side-effect pipeline.

Basic example:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` tracks Compose state reads inside the lambda. When the read value
changes, the Flow emits a new value.

Typical scenarios:

- scroll analytics;
- pagination trigger;
- debounce/sample for UI state;
- logging/debugging;
- side effects that must not be launched in the composable body.

Pagination example:

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

Because the result is a `Flow`, operators can be used:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

`snapshotFlow` is usually collected in `LaunchedEffect`. Do not start collection
directly in the composable body.

Do not use `snapshotFlow` for regular UI rendering:

```kotlin
Text(text = state.title)
```

If you only need to display state, read the state directly. `snapshotFlow` is
needed specifically for moving from Compose state to Flow/side-effect logic.

Important nuance: the lambda must contain a Compose snapshot state read. If it
does not, `snapshotFlow` has nothing to track.

**In short:** `snapshotFlow` converts Compose state reads into a `Flow`. It is
useful for analytics, pagination, debounce, and side effects in
`LaunchedEffect`, but it is not needed for simple UI rendering.

</details>

<details>
<summary>118. How do you optimize recomposition in Compose?</summary>

#### Kotlin

Optimizing recomposition does not mean “remove all recompositions”; it means
making them cheap and local. Recomposition is normal. The problem is heavy work
or too broad an update area.

### Read state closer to where it is used

Bad:

```kotlin
ProfileScreen(state = hugeState)
```

Better:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

If a composable reads state, it may recompose when that state changes. Do not
pass the whole screen state where only one field is needed.

### Lazy lists: keys and contentType

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

Stable keys help Compose avoid confusing items and preserve item state.

For different item types:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### Do not do heavy work in a composable

Bad:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Better:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Even better, prepare formatted text in a mapper/ViewModel if it is part of the
UI model.

### Use `derivedStateOf` for frequent changes

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

This is useful when source state changes often, but the UI should react only to
a derived condition.

### Stable/immutable models

Compose works better with immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Mutable collections and unstable objects can prevent Compose from skipping
unnecessary recompositions.

### Side effects only through effect APIs

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

The composable body can run many times, so analytics, navigation, listeners, and
suspend work should not be placed directly in the body.

### Verify with profiling

- Layout Inspector recomposition counts;
- Compose compiler metrics;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

**In short:** recomposition optimization means local state reads, smaller
composables, stable keys, `contentType`, immutable UI models, `remember`,
`derivedStateOf`, correct effects, and profiling. The goal is cheap local
updates, not zero recompositions.

</details>

<details>
<summary>119. How do you debug recomposition issues?</summary>

#### Kotlin

Recomposition issues are debugged through measurement, not guesses. The goal is
to find which state changes often and which part of the UI it affects.

### Layout Inspector

Android Studio Layout Inspector shows recomposition counts for composables. But
the count itself is not necessarily a problem: a lightweight composable can
recompose often without visible impact.

### Temporary logs

For a quick check, you can add a log:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Or through `SideEffect`:

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

This is only a debug tool, not production code.

### Check state reads

A common cause is a composable reading state that is too broad:

```kotlin
UserRow(screenState = state)
```

Better:

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

State should be read closer to where it is used.

### Check lists and parameters

`LazyColumn` should have stable keys:

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

Also check unstable parameters: mutable collections, objects/lambdas created
again unnecessarily, or heavy work in the composable body.

### Compiler metrics and profiler

Compose compiler metrics help identify which composables/types are unstable,
restartable, or skippable. Android Studio Profiler and Macrobenchmark are needed
to confirm real performance impact.

Practical checklist:

- inspect counts in Layout Inspector;
- find state that changes often;
- narrow state reads;
- check stable keys in lazy lists;
- remove heavy work from the body;
- check unstable parameters;
- apply `remember`/`derivedStateOf` only when there is a reason;
- confirm the result with profiler/metrics.

**In short:** recomposition is debugged with Layout Inspector, logs, compiler
metrics, and profiler. The most common causes are broad state reads, unstable
parameters, missing keys, heavy work in the body, or incorrect state ownership.

</details>

<details>
<summary>120. How can you avoid lag in Compose, for example during scrolling?</summary>

#### Kotlin

Lag during scrolling is most often caused by heavy work on the main thread,
unnecessary recompositions, allocations in items, unstable keys, or an overly
complex row UI.

### Stable keys and contentType

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

`key` preserves item identity after insert/delete/reorder.

For different item types:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` helps Compose reuse compatible item compositions.

### Do not do heavy work in an item

Bad:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

It is better to prepare formatted text in a mapper/ViewModel or at least cache
the object with `remember`. I/O, bitmap decoding, sorting/filtering, and heavy
calculations should not run in a composable.

### Narrow state reads

An item should receive only its own data:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Do not pass the whole screen state into every row.

### Stable UI models

Use immutable UI models and avoid creating new collections/wrappers on every
recomposition without a reason.

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

### Scroll-derived state

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

`derivedStateOf` is useful when scroll changes often, but the UI reacts only to a
derived condition.

### Images and paging

For images, use a loader with memory/disk cache and the correct target size. Do
not decode bitmaps manually in a composable or on the main thread.

For large lists, use Paging:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Profiling

Confirm optimizations with:

- Layout Inspector recomposition counts;
- Android Studio Profiler/System Trace;
- Macrobenchmark in a release-like build;
- Baseline Profiles.

**In short:** smooth scrolling in Compose requires stable keys, `contentType`,
narrow state reads, cheap items, immutable models, cached images, paging, and
profiler verification. The main point is not to block the main thread or do
heavy work during composition.

</details>

<details>
<summary>121. What is the difference between the Stable and Immutable annotations?</summary>

#### Kotlin

`@Stable` and `@Immutable` in Compose are contracts for the compiler/runtime that
help decide whether recomposition can be skipped. This is not a “magic
optimization”, but a promise that a type behaves in a certain way.

Main difference:

```text
@Immutable -> object does not change after creation
@Stable    -> object may change, but Compose sees those changes
```

### `@Immutable`

Suitable for value/UI models:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Condition: all public properties do not change after creation, and the fields
are also immutable/stable.

Bad:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

`MutableList` can be changed in place, and Compose may not see the change.

Better:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

For a stricter guarantee, immutable collections can be used.

### `@Stable`

Suitable for state holders or controller objects where changes go through
observable Compose state:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

A mutable object is acceptable because Compose sees changes to `count`.

Bad:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

A regular mutable field does not notify Compose about changes.

When to use what:

- `@Immutable` — UI models, value objects, screen state data classes;
- `@Stable` — custom state holders with `mutableStateOf`, controllers, objects
  with controlled observable mutation.

Common mistakes:

- annotating a mutable object as `@Immutable`;
- putting `@Stable` on a class with regular mutable fields;
- keeping `MutableList`/`MutableMap` in UI state;
- mutating old state instead of creating new state;
- using annotations as a “cure” for poor state ownership.

**In short:** `@Immutable` means “does not change after creation”. `@Stable`
means “may change, but Compose correctly sees the changes”. Add an annotation
only if the type truly satisfies this contract.

</details>

<details>
<summary>122. Why pass a key to LazyColumn, and what is it under the hood?</summary>

#### Kotlin

`key` in `LazyColumn` defines stable item identity independently of its position.

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

Without a key, Compose uses position. This is fine for a static list, but breaks
with insert/remove/reorder.

```text
Before:     After insert:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

By position, Compose sees new content in old slots. By `user.id`, it understands
that the old items simply changed position.

### Remembered state

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

Without a stable key, `expanded` can remain attached to a position and move to
another user. The key moves remembered/saveable state together with item
identity.

### Under the hood

The key becomes the identity of a composition group. The runtime uses it to:

- match the old and new group;
- move/reuse the group;
- preserve `remember` state;
- restore `rememberSaveable` state;
- work correctly with item animations.

This is not `DiffUtil`: the key is needed for composition/placement bookkeeping
inside the lazy layout.

### Key requirements

A key should be:

- unique among sibling items;
- stable between recompositions;
- tied to domain identity;
- saveable if the item has `rememberSaveable`.

Good:

```kotlin
key = { it.id }
```

Bad:

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Random/time creates a new identity on every recomposition. Index is equivalent to
the default position identity and is suitable only for truly static lists.

A composite key is acceptable if it is stable:

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

For Paging, use the item key API:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**In short:** `key` binds a lazy item to stable identity, not to an index. It is
needed for dynamic lists, local item state, reorder, animations, and Paging. The
best key is a stable server/database/client-generated ID.

</details>

<details>
<summary>123. What other parameters can be passed to LazyColumn items(), and what is contentType used for?</summary>

#### Kotlin

In `LazyColumn.items()`, the important parameters are `key` and `contentType`.

Basic usage:

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

For a dynamic list, it is better to pass `key` explicitly:

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

`key` defines stable item identity. It is needed so Compose does not confuse
remembered state after insert/delete/reorder.

A good key:

- is unique;
- is stable;
- does not depend on position;
- is not random/time-based;
- is based on a domain ID.

`contentType` describes the UI item type:

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

For example: `header`, `post`, `ad`, `loader`. This helps Compose reuse
composition more efficiently for items of the same type in a mixed list.

Example with sealed items:

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

If an index is needed, there is `itemsIndexed`:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

Index should not be used as a key for mutable/reorderable lists.

When `contentType` is not critical: when all items have the same UI shape. The
biggest benefit is in mixed feeds, chats, and catalogs.

**In short:** `key` is stable item identity. `contentType` is the UI item
type/shape for better composition reuse in mixed lists. For production dynamic
lists, `key` is almost always needed; `contentType` is needed when there are
different row types.

</details>

<details>
<summary>124. What is SOLID?</summary>

#### Kotlin

`SOLID` is five principles of OOP design that reduce coupling, improve
testability, and make changes more local.

### S — Single Responsibility Principle

A class should have one reason to change.

It is bad when a `ViewModel` handles networking, DB, mapping, and UI state. It is
better to separate responsibilities:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

The `ViewModel` manages state, the use case manages the business action, and the
repository manages data.

### O — Open/Closed Principle

Code is open for extension but should not require constant editing.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

A new implementation can be added without rewriting client code.

### L — Liskov Substitution Principle

A subtype should correctly substitute its base type.

Bad:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

If an implementation does not satisfy the contract, the contract is too broad or
incorrect.

### I — Interface Segregation Principle

A client should not depend on methods it does not need.

Bad:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Better to split it:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D — Dependency Inversion Principle

High-level code depends on abstractions, not concrete implementations.

Bad:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Better:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

This makes the use case easy to test with a fake repository.

In Android, this usually looks like:

```text
UI -> ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

Practical rule: SOLID does not mean creating an interface for every class. An
abstraction is needed where there is a real boundary, alternative
implementation, or test seam.

**In short:** SOLID helps avoid mixing UI, business logic, and data layer,
prevents god classes, keeps contracts narrow, and makes code testable. But
unnecessary abstractions also cause harm.

</details>

<details>
<summary>125. Explain each SOLID principle with Android examples.</summary>

#### Kotlin

SOLID helps separate responsibilities, keep contracts clean, and test business
logic without the Android framework, Retrofit, or Room.

### S — Single Responsibility

A class has one reason to change.

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

The `ViewModel` manages UI state, the use case manages the business operation,
and the repository manages data. SRP does not mean “one method per class”; it
means not mixing different reasons to change.

### O — Open/Closed

Code is extended through new implementations, not by constantly editing existing
logic.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

A new handler is added as a separate class. But an abstraction is needed only
where there is real variability.

### L — Liskov Substitution

A subtype should correctly substitute its base type.

It is bad when `ReadOnlyStorage` implements `save()` and throws
`UnsupportedOperationException`. It is better to split the contract:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP is not only about signatures; it is also about guarantees, errors, side
effects, and invariants.

### I — Interface Segregation

A caller should not depend on methods it does not need.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

A use case receives a narrow contract instead of a god-interface. But overly
small interfaces without a caller role are also boilerplate.

### D — Dependency Inversion

High-level business logic depends on an abstraction, and the data layer
implements it.

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

The use case does not create Retrofit/Room directly. DI provides a real
implementation or a fake.

```text
UI -> ViewModel -> UseCase -> Repository contract
                              ^
                              |
                     Data implementation -> API/DB
```

Common Android violations:

- Fragment calls API/DAO directly;
- ViewModel contains networking, mapping, cache, and navigation;
- one repository is responsible for the whole app;
- an interface is created for every class without a boundary;
- domain depends on Android/infrastructure types;
- a subtype formally implements an interface but breaks its contract.

**In short:** SRP separates responsibilities, OCP defines extension points, LSP
ensures correct subtype substitution, ISP keeps contracts narrow, and DIP makes
business logic depend on abstractions. In Android, this separates UI/domain from
framework and data details.

</details>

<details>
<summary>126. What is OOP?</summary>

#### Kotlin

OOP is an approach where a system is modeled as objects that combine state and
behavior. A class describes a type; an object is its instance.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

The main principles are encapsulation, abstraction, inheritance, and
polymorphism.

### Encapsulation

A class hides mutable state and changes it through controlled methods:

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

This way, the class protects invariants.

### Abstraction

Code depends on a contract, not an implementation:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

The `ViewModel` does not know whether data came from Retrofit, Room, or a fake.

### Inheritance

In Kotlin, classes are `final` by default. Inheritance is enabled with `open`:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Inheritance models `is-a`, but it often creates strong coupling. Composition is
usually safer.

### Polymorphism

Different implementations can be substituted through a shared contract:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

The caller works with `ImageLoader`, so production and test implementations are
interchangeable.

In Kotlin, OOP combines well with `data class`, `sealed class/interface`,
extension functions, immutability, and constructor injection. What matters is
not the number of classes/interfaces, but clear responsibilities and boundaries.

**In short:** OOP organizes state and behavior into objects. In Kotlin, the key
ideas are encapsulation, contracts, polymorphism, and careful inheritance; often
immutable models and composition are better than a deep hierarchy.

</details>

<details>
<summary>127. What is abstraction?</summary>

#### Kotlin

Abstraction is hiding implementation details behind a clear contract. It
describes “what the code does”, not “how exactly it does it”.

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Code that uses `UserRepository` does not know whether the data comes from an
API, Room, cache, or a fake object in a test.

A use case depends on the contract:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

The implementation hides the details:

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

For tests, a fake can be substituted:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

A typical Android structure:

```text
ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

UI should not know about Retrofit, Room, or cache strategy.

Abstraction does not necessarily mean an interface. It can also be an abstract
class, sealed hierarchy, function type, or just the public API of a class. But in
Android, abstract/base classes often create a rigid hierarchy, so composition is
usually safer.

Abstraction vs encapsulation:

```text
Abstraction   -> exposes an important contract
Encapsulation -> hides internal state/details
```

A bad abstraction is an interface “just in case”, without a real alternative
implementation or boundary:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

If there is one simple implementation and no test seam/boundary, the interface
may be unnecessary.

A good abstraction:

- has a clear contract;
- hides unstable details;
- does not leak implementation details;
- simplifies testing;
- matches a real point of change;
- does not add boilerplate without benefit.

**In short:** abstraction separates contract from implementation. In Android, it
is useful at boundaries: network, database, analytics, time, dispatchers, and
repositories. Do not abstract every class without a real reason.

</details>

<details>
<summary>128. What is the difference between constructors and methods?</summary>

#### Kotlin

A constructor creates an instance and brings the object into an initial valid
state. A method describes the behavior of an already created object and can be
called many times.

Primary constructor:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Parameters with `val`/`var` become properties. A constructor has no return type
and is called with `User(...)`.

Method:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

A method is called on an instance:

```kotlin
user.displayName()
```

For validation or initialization logic, use `init`:

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

A secondary constructor delegates to the primary one with `this(...)`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

In Kotlin, a default parameter is often better:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

A constructor is often used for DI:

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

The constructor defines required dependencies; the method performs an operation.

What not to do in a constructor/init:

- network request;
- database query;
- coroutine launch;
- `runBlocking`;
- heavy I/O;
- Android lifecycle-dependent logic.

Bad:

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Better:

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**In short:** a constructor creates a valid object and accepts required
state/dependencies. A method performs behavior after construction. Heavy,
suspend, and lifecycle-bound work should not be launched in a constructor.

</details>

<details>
<summary>129. What is a method signature?</summary>

#### Kotlin

A method signature is the set of characteristics by which the compiler
distinguishes one function from another. In Kotlin, this is primarily the name,
receiver, and parameter types/order.

```kotlin
fun loadUser(id: String): User
```

Simplified signature: `loadUser(String)`.

### Overloading

Overloads must differ by parameter list:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Return type is not enough:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflict
```

Parameter name is also not enough:

```kotlin
fun load(id: String)
fun load(userId: String) // conflict
```

Named arguments improve call-site readability, but they do not create a new
signature.

The order of types matters:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

This is valid, but often a poor API because of low readability.

### Default parameters

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

This is one Kotlin function. For Java overloads, `@JvmOverloads` can be used, but
manually creating many adjacent overloads should be done carefully.

### Override

An override must match the base type contract:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Changing the parameter type creates a different function, not an override.
Return type may be a covariant subtype.

### Receiver and extensions

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

The extension receiver participates in Kotlin resolution. But an extension
function is not a virtual override.

### JVM nuances

Type erasure can cause clashes:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // JVM clash: process(List)
```

Nullability also does not create a separate JVM signature:

```kotlin
fun load(id: String)
fun load(id: String?) // JVM clash
```

The solution is a different name or `@JvmName` if it is truly needed.

**In short:** in Kotlin, functions are distinguished by name, receiver, and
parameter types/order. Return type and parameter names are not enough. On the JVM,
you also need to remember type erasure, nullability, and interop nuances.

</details>

<details>
<summary>130. What is the difference between overloading and overriding?</summary>

#### Kotlin

```text
Overloading -> same name, different parameter lists
Overriding  -> same signature, new implementation in a subtype
```

### Overloading

Several functions in the same scope have the same name but different parameters:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

The compiler chooses an overload by the static types of the arguments. Return
type alone cannot distinguish an overload:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // compile error
```

In Kotlin, some overloads are better replaced with default parameters and named
arguments:

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
)

loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

### Overriding

A subtype implements or changes an inherited method with the same signature:

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

Changing the parameter type creates a different function, not an override.

In Kotlin, classes and methods are `final` by default. To override a member, it
must be `open`, `abstract`, or a member of an interface:

```kotlin
open class BaseTracker {
    open fun track(event: Event) = Unit
}

class FirebaseTracker : BaseTracker() {
    override fun track(event: Event) {
        // implementation
    }
}
```

### Dispatch

Overloading uses compile-time/static dispatch:

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

Overriding uses runtime/virtual dispatch:

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // CrashlyticsLogger implementation
```

Important nuances:

- overloads with default arguments can be ambiguous;
- an override must preserve the base type contract;
- return type in an override may be covariant;
- extension functions are not overridden; they are resolved statically;
- `final override` prevents further overriding.

**In short:** overloading means several functions with one name and different
parameters, selected at compile time. Overriding means replacing an inherited
implementation with the same signature, selected at runtime through polymorphism.

</details>

<details>
<summary>131. What is the difference between an interface and an abstract class?</summary>

#### Kotlin

An interface describes a contract/role. An abstract class describes a shared base
with partial implementation, a constructor, and state.

```text
interface      -> contract, many roles
abstract class -> base implementation, one parent class
```

### Interface

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

A class can implement many interfaces:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

An interface is well suited for DI, tests, and dependency inversion:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

In Kotlin, an interface can have a default method:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

But an interface has no constructor and is not suitable for shared mutable state.

### Abstract class

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // shared logic
    }

    abstract suspend fun refresh()
}
```

An abstract class can have:

- constructor;
- state;
- protected methods;
- partial implementation;
- template method pattern.

But a class can inherit from only one class:

```kotlin
class MyRepository : BaseRepository()
```

### When to use what

Interface:

- a contract is needed;
- there are multiple implementations;
- a fake/mock is needed in tests;
- a class has several roles;
- a dependency should be abstracted.

Abstract class:

- shared implementation is needed;
- protected state is needed;
- there is a base lifecycle/template;
- constructor is part of the base contract.

In Android, be careful with `BaseActivity`, `BaseFragment`, `BaseViewModel`: they
quickly become god-base-classes and create a rigid hierarchy.

Composition is often better:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**In short:** interface is for contracts, DI, tests, and multiple roles.
Abstract class is for shared implementation and state, but with a risk of rigid
inheritance. In modern Android, interface + composition is often better than deep
base classes.

</details>

<details>
<summary>132. Can an interface contain no methods?</summary>

#### Kotlin

Yes. An empty interface is called a marker interface. It does not define
behavior; it marks a type with a certain role in the type system.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Now `UserProfile` can be accepted as `Cacheable`, checked with `is Cacheable`, or
used as a generic bound.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // only explicitly marked types
}
```

If `sync` needs an `id` or behavior, a marker is no longer enough; a
property/method should be added to the contract.

### Sealed marker

An empty `sealed interface` is often used as the root for a closed set of
states/events/effects:

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

Here it is not just a marker, but a sum type: the value can be one of the defined
variants, and the compiler checks an exhaustive `when`.

A classic marker interface example is `java.io.Serializable`: it has no methods,
but the runtime checks whether an object implements this type.

### Interface or annotation

A marker interface is better when you need:

- subtype relation;
- generic bounds;
- polymorphic API;
- sealed hierarchy;
- `is` check without reflection.

An annotation is better for metadata:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

It is processed by a compiler plugin, code generator, or runtime reflection. It
does not create a subtype relation.

### When a marker is bad

Do not create a marker “for the future”:

```kotlin
interface Important
interface Special
```

Bad signs:

- the marker is not used anywhere as a type/bound/root;
- the logic has many `is Marker` checks;
- data/methods are needed, but the code uses casts;
- the role is better expressed as an enum/property;
- metadata is better described by an annotation.

**In short:** an interface can be empty. A marker interface is useful for subtype
relation, generic bounds, or sealed hierarchies. If metadata is needed, use an
annotation; if behavior is needed, the interface should contain members.

</details>

<details>
<summary>133. Can an interface contain another interface?</summary>

#### Kotlin

Yes. An interface in Kotlin can contain another interface. This is a nested
interface.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Usage:

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // handle change
    }
}
```

A nested interface is referenced through `Outer.Inner`, for example
`UserRepository.Listener`.

When this is appropriate:

- the contract is tightly related to the outer API;
- the nested type is used only in this context;
- it is a callback or a small nested contract;
- a top-level type would only clutter the package.

Example callback API:

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

A nested interface is not `inner` and has no access to the outer type's instance
state:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` cannot directly read `id`.

Do not confuse this with interface inheritance:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

This is not nesting; it is inheritance of contracts.

Do not nest an interface if it is independent and used across the whole project:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

Then the outer interface becomes an artificial namespace. Top-level contracts are
better.

**In short:** an interface can contain a nested interface. This is useful for
callbacks or contracts that exist only in the context of the outer API.
Independent abstractions are better as top-level types.

</details>

<details>
<summary>134. Why should you avoid adding dozens of methods to an interface?</summary>

#### Kotlin

A large interface usually violates the Interface Segregation Principle: clients
depend on methods they do not need. This increases coupling and makes
implementations and tests harder.

Bad:

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

If a use case needs only `getUser`, it still depends on the whole contract.

Implementation problem:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

If an implementation cannot honestly implement a method, the contract is wrong
or too broad.

Testing problem:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // unnecessary noise
}
```

Fakes/mocks are forced to implement unnecessary methods.

Better to split roles:

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

A use case depends only on the contract it needs:

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

A large interface is acceptable only if it is truly one cohesive abstraction. For
example, a DAO for one table may have several related queries. But if methods
have different clients and different reasons to change, they are better split.

**In short:** an interface should describe a role, not “everything the system can
do”. If a client uses 1–2 methods out of 20, the contract is too broad. It is
better to split reader/writer/uploader/etc by real roles.

</details>

<details>
<summary>135. What is composition, and how is it different from inheritance?</summary>

#### Kotlin

Composition means a class receives behavior through dependencies. Inheritance
means a class inherits behavior from a base class.

```text
Inheritance -> is-a
Composition -> has-a / uses-a
```

### Inheritance

It is appropriate when a subclass is truly a specialization of the base type:

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

`Dog` is an `Animal`, so the `is-a` relationship is logical.

### Composition

A class uses other objects instead of inheritance:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

`Dog` has a `SoundPlayer`, but is not a `SoundPlayer`.

In Android/Kotlin, composition often looks like this:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

Dependencies are explicit and easy to replace with fakes/mocks in tests.

### The problem with base classes

Large `BaseViewModel`, `BaseRepository`, `BaseFragment` classes often become god
base classes:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Child classes receive methods/state they do not need. Changing the base class can
break many unrelated screens.

### Kotlin delegation

Kotlin has delegation as a convenient form of composition:

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

`UserService` delegates `Logger` to another object without inheriting from it.

When to use what:

- inheritance — real `is-a`, stable base contract;
- composition — behavior needs to be substituted, tested, combined, or changed;
- Android app code — composition is usually better than deep inheritance.

**In short:** inheritance models `is-a`; composition models `has-a/uses-a`. In
Kotlin/Android, composition is usually better: explicit dependencies, simpler
tests, less coupling, and no fragile base-class hierarchies.

</details>

<details>
<summary>136. What access modifiers are there in Kotlin?</summary>

#### Kotlin

Kotlin has four visibility modifiers:

```text
public    -> accessible from everywhere
internal  -> accessible within a module
protected -> accessible in a class and subclasses
private   -> accessible in the current scope or file
```

### `public`

`public` is the default visibility:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Everything public becomes part of the API, so implementation details should not
be exposed without a reason.

### `private`

A member is accessible only inside the class/object:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Top-level `private` is accessible within one Kotlin file:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### `protected`

Accessible in the class and subclasses:

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}
```

In Kotlin, `protected` does not provide package access, unlike Java, and is not
allowed at the top level. It is part of the inheritance API.

### `internal`

Accessible within a Kotlin module:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

A module is a compilation boundary, such as a Gradle module, not a package.

`internal` is useful in multi-module Android: keep only the module API public,
and keep implementation `internal`.

Important: `internal` is a compile-time boundary, not security. In bytecode,
Java/reflection may see more.

### Setter and constructor visibility

A property can be public for reading and private for writing:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set
}
```

A constructor can also have visibility:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Practical rules:

- start with the narrowest sufficient visibility;
- keep implementation `private` or `internal`;
- keep public API minimal;
- use `protected` only for a real inheritance contract;
- do not treat visibility as a security boundary.

**In short:** `public` means everyone, `internal` means module, `protected` means
inheritance hierarchy, and `private` means class/file scope. A good Kotlin API
exposes the minimum necessary surface.

</details>

<details>
<summary>137. What is a singleton?</summary>

#### Kotlin

A singleton is one instance within a certain owner: classloader/process or DI
component. In Android, it is important to remember that the process can be
killed, and a multiprocess app will have a separate singleton in each process.

### Kotlin `object`

```kotlin
object AppLogger {
    fun log(message: String) = println(message)
}
```

`object` is initialized lazily and thread-safely on first use. But mutable state
inside it does not automatically become thread-safe:

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

A companion is a singleton tied to a class. The class itself can have many
instances. Usually, companions are used for factory methods and constants.

### Singleton through DI

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
)
```

In Hilt, `@Singleton` means one instance in `SingletonComponent`, not an “eternal
object”. DI is better than a global singleton when there are dependencies:

- constructor dependencies are explicit;
- implementation can be replaced with a fake;
- lifetime is controlled by the component;
- caller depends on a contract.

For shorter lifecycles, there are `@ViewModelScoped`, `@ActivityScoped`, and so
on.

### Risks

Bad:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Problems:

- Activity leak;
- global mutable state;
- race conditions;
- hard tests because of shared state;
- unclear logout/reset;
- state loss after process death.

An app-level singleton should not hold `Activity`, `Fragment`, `View`, or
`ActivityContext`. If context is needed, use only application context.

Session/token should not be stored only in an in-memory singleton: secure
persistence, restore, and explicit clear are needed.

A singleton is appropriate for:

- Room database;
- OkHttp client;
- logger/analytics;
- stateless app-wide services;
- immutable configuration.

It is not appropriate for screen state, ViewModel, user session without reset, or
a dependency with a short lifecycle.

**In short:** in Kotlin, a singleton is often created with `object`; in Android,
dependencies are better scoped through DI. A singleton is suitable for app-wide
services, but global mutable state and UI references are almost always a problem.

</details>

<details>
<summary>138. What are generics?</summary>

#### Kotlin

Generics allow you to parameterize a class, interface, or function with a type
and preserve compile-time type safety.

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` is a type parameter; `String`/`Int` are type arguments. The compiler knows the
type of `value`, so an unsafe cast is not needed.

Generic function:

```kotlin
fun <T> singleItemList(item: T): List<T> = listOf(item)
```

### Bounds

A type parameter can be restricted:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Multiple bounds:

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable = Unit
```

### Generic result

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val cause: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

`Nothing` is a subtype of all Kotlin types, so `Error` and `Loading` can be used
as `Result<User>`, `Result<List<Post>>`, and so on.

### Variance

By default, a generic type is invariant. Even if `Cat : Animal`,
`MutableList<Cat>` is not `MutableList<Animal>`.

`out T` — producer/covariance:

```kotlin
interface Producer<out T> {
    fun produce(): T
}

val cats: Producer<Cat> = TODO()
val animals: Producer<Animal> = cats
```

`in T` — consumer/contravariance:

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}

val animalConsumer: Consumer<Animal> = TODO()
val catConsumer: Consumer<Cat> = animalConsumer
```

Model: producer — `out`, consumer — `in`. If a type both accepts and returns
`T`, it is usually invariant.

### Type erasure and reified

On the JVM, generic type arguments are mostly erased at runtime:

```kotlin
value is List<*> // allowed
// value is List<String> // not directly allowed
```

`inline reified` gives access to the type parameter at runtime:

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` is possible only for inline functions.

Practical rules:

- bounds define the minimum contract;
- `out/in` describe safe subtype relationships;
- mutable containers are more often invariant;
- for runtime generic type, use `reified` or an explicit type token;
- do not complicate an API with generics without real benefit.

**In short:** generics provide type-safe reusable APIs. Bounds restrict allowed
types, variance (`out/in`) controls subtype compatibility, and `reified`
partially works around JVM type erasure for inline functions.

</details>

<details>
<summary>139. What is type erasure, and why does it happen?</summary>

#### Kotlin

Type erasure means that on the JVM, concrete generic arguments are usually not
stored in the runtime object. `List<String>` and `List<Int>` are just `List` at
runtime.

The reason is backward compatibility of Java generics with old bytecode. A type
parameter is erased to its upper bound or `Object`. Kotlin/JVM works in the same
JVM model.

You cannot check this directly:

```kotlin
if (value is List<String>) { } // erased type
```

You can check the container:

```kotlin
if (value is List<*>) {
    // element type unknown
}
```

`List<*>` allows safely reading elements as `Any?`, but it does not guarantee
their concrete type.

An unchecked cast does not validate all elements:

```kotlin
val strings = value as List<String> // unchecked
```

If real validation is needed, elements must be checked:

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String ?: error("Expected String")
    }
```

### `reified`

A regular `T` is not available for `is T`. But an inline function can have a
`reified` type parameter:

```kotlin
inline fun <reified T> isType(value: Any): Boolean =
    value is T
```

Then `T::class`, `is T`, and class-token APIs are available. But `reified` does
not fully restore nested generics: `List<String>` still needs an element check or
a serializer/type token.

Without `reified`, the type is passed explicitly:

```kotlin
fun <T> decode(json: String, clazz: Class<T>): T = TODO()

val user = decode(json, User::class.java)
```

For `List<User>`, `Class<List>` alone is not enough, so libraries use `Type`,
`KType`, generated adapters, or `KSerializer<T>`.

Type erasure also creates JVM signature clashes:

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {} // clash: handle(List)
```

Different names or careful `@JvmName` are needed.

**In short:** type erasure removes generic arguments at runtime because of
JVM/Java compatibility. For runtime checks, use `List<*>`, `inline reified`, an
explicit type token, or a serializer; an unchecked cast is not a full type check.

</details>

<details>
<summary>140. What collections exist: List, Set, Map, Queue, Stack?</summary>

#### Kotlin

Choose a collection by semantics: order, uniqueness, lookup by key, FIFO, or
LIFO.

### `List`

An ordered collection with index access and allowed duplicates:

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

The typical mutable implementation on the JVM is `ArrayList`: index access is
`O(1)`, search is `O(n)`, insert/remove in the middle is `O(n)`.

### `Set`

A collection of unique elements:

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* exists */ }
```

`HashSet` usually has average `O(1)` for `contains/add/remove`, but it requires
correct `equals()` and `hashCode()`.

### `Map`

A key-value structure where keys are unique:

```kotlin
val usersById: Map<String, User> = users.associateBy(User::id)
val user = usersById["42"]
```

`HashMap.get()` is `O(1)` on average. Map is suitable for cache, index, and
lookup table. A mutable key is dangerous if fields that affect `hashCode()` can
change.

### `Queue` and `Stack`

Queue — FIFO:

```text
first in -> first out
```

Stack — LIFO:

```text
last in -> first out
```

In Kotlin, `ArrayDeque` is convenient:

```kotlin
val deque = ArrayDeque<String>()

// queue
deque.addLast("A")
deque.addLast("B")
val first = deque.removeFirst() // A

// stack
deque.addLast("C")
val last = deque.removeLast() // C
```

Operations at the ends are amortized `O(1)`. `ArrayDeque` is better than the
legacy Java `Stack`.

### Read-only vs mutable

Kotlin separates APIs:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` has no `add/remove`, but this is not deep immutability: the underlying
object or elements may be mutable. Shared mutable state should be encapsulated.

Practical choice:

- order, index, duplicates — `List`;
- uniqueness and membership check — `Set`;
- lookup by key — `Map`;
- FIFO — `ArrayDeque.removeFirst()`;
- LIFO — `ArrayDeque.removeLast()`;
- priority order — `PriorityQueue`.

**In short:** `List` means order and duplicates, `Set` means uniqueness, `Map`
means lookup by key, Queue means FIFO, and Stack means LIFO. In Kotlin,
`ArrayDeque` is often enough for Queue/Stack.

</details>

<details>
<summary>141. What is thread safety of collections?</summary>

#### Kotlin

A thread-safe collection keeps a correct state during concurrent access. Regular
`MutableList`, `MutableMap`, and `MutableSet` in Kotlin/JVM are not thread-safe.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Concurrent writes can cause lost updates, incorrect internal state, or an
exception.

### Read-only ≠ immutable

```kotlin
val users: List<User> = mutableListOf()
```

`List` forbids mutation only through this reference. The underlying collection or
elements may still be mutable.

### Main strategies

1. **Single owner/thread**

All reads/writes go through one owner: Main thread, single dispatcher, or an
actor-like component. This is the simplest ownership model.

2. **`Mutex` for coroutines**

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

All accesses must go through the same `Mutex`. Do not do long network/suspend
work inside the lock.

3. **Concurrent collections**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` is suitable for concurrent keyed access. But a compound
operation does not automatically become atomic; for that, use atomic methods
such as `computeIfAbsent` or separate synchronization.

`CopyOnWriteArrayList` is suitable for listeners: many reads, rare writes.

4. **Immutable snapshots**

For UI state, it is better to create a new collection:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Here atomicity is provided by `_state.update`, not by `List` itself.

### Iteration

Mutation during iteration can cause `ConcurrentModificationException` even on a
single thread. If a collection needs to be changed while iterating, use an
iterator, a collection operation, or a snapshot.

Practical choice:

- coroutine state — `Mutex` or serialized owner;
- concurrent keyed access — `ConcurrentHashMap`;
- read-heavy listeners — `CopyOnWriteArrayList`;
- UI state — immutable snapshots + atomic `update`;
- compound invariants — one critical section.

**In short:** mutable collections are not thread-safe. A read-only API does not
mean immutability. Safety requires ownership, `Mutex`, concurrent collections, or
immutable snapshots; synchronize the whole invariant, not a single call.

</details>

<details>
<summary>142. What is the difference between final, finally, and finalize?</summary>

#### Kotlin

These are three different concepts:

```text
final    -> prohibits inheritance/override
finally  -> cleanup block after try/catch
finalize -> obsolete GC callback
```

### `final`

In Java, `final` prohibits class inheritance, method override, or variable
reassignment.

In Kotlin, classes and methods are `final` by default. Inheritance requires
`open`:

```kotlin
open class BaseViewModel : ViewModel() {
    open fun load() = Unit
}

class ProfileViewModel : BaseViewModel() {
    final override fun load() {
        // subclasses cannot override this method
    }
}
```

`final override` allows overriding once and forbids further overriding.

For variables/properties, Kotlin uses `val`:

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex")
```

`val` forbids reassigning the reference, but it does not make the object
immutable.

### `finally`

`finally` runs after `try/catch` for cleanup:

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Use cases: close a resource, unlock, unregister a listener, reset temporary
state.

In coroutines, `finally` also runs on cancellation:

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

If cleanup needs a suspend function in a cancelled coroutine, a short
`withContext(NonCancellable)` is sometimes needed. But this should be an
exception, not the default.

`finally` is not guaranteed on process kill/JVM crash.

### `finalize`

`finalize()` is an old Java `Object` callback that the GC could call before
collecting an object. It should not be used for cleanup because:

- it is unknown when it will be called;
- it may not be called before process termination;
- it makes GC more complex;
- it is a deprecated/legacy mechanism.

Replacements:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

In Android, cleanup is tied to lifecycle:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

In Compose, use `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**In short:** `final` restricts inheritance/override, `finally` performs
deterministic cleanup after `try/catch`, and `finalize()` is a deprecated and
unreliable GC mechanism that should not be used.

</details>

<details>
<summary>143. How does try-catch-finally work?</summary>

#### Kotlin

`try-catch-finally` separates a risky operation, exception handling, and cleanup.

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Order:

```text
success -> try -> finally
error   -> try -> matching catch -> finally
```

If there is no matching `catch`, the exception goes to the caller after
`finally` runs.

### `catch`

Different types can be caught separately:

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

`catch` blocks are checked from top to bottom, so more specific types should come
before more general ones.

Kotlin has no checked exceptions, so the compiler does not force you to write
`try/catch`. Catch an exception where the code can meaningfully recover,
transform the error, or add context.

### `try` as an expression

`try` can return a value:

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

`finally` does not define the result expression.

### `finally`

`finally` is used for cleanup:

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

It runs on success, exception, `return`, and coroutine cancellation. Do not
`return` from `finally`, because it can override the result or exception.

For `Closeable`/`AutoCloseable`, prefer `use`:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

### Coroutines

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

`CancellationException` must not be swallowed; it should be rethrown, otherwise
cooperative cancellation breaks.

`finally` in a cancelled coroutine will run, but suspend cleanup may immediately
receive cancellation. For short mandatory suspend cleanup, `withContext(NonCancellable)`
is sometimes used.

**In short:** `try` runs the operation, `catch` handles a matching exception, and
`finally` performs cleanup regardless of the result. In coroutines, do not
swallow `CancellationException`; resources are better closed with `use`.

</details>

<details>
<summary>144. What is the difference between checked and unchecked exceptions?</summary>

#### Kotlin

In Java, a checked exception must either be caught or declared with `throws`.
Unchecked exceptions do not have this requirement. Kotlin has no checked
exceptions; the compiler does not force you to write `try/catch`.

### Java

Checked exception:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

The caller must use `try/catch` or also declare `throws`.

Unchecked exceptions are `RuntimeException` and its subclasses:

```java
String value = null;
value.length(); // NullPointerException
```

The compiler does not require handling `NullPointerException`,
`IllegalArgumentException`, or `IllegalStateException`.

`Error` is also unchecked, but it is usually not intended for recovery.

### Kotlin

Kotlin allows calling Java/Kotlin APIs that may throw `IOException` without a
mandatory catch:

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

The exception can still happen at runtime, so the error contract should be shown
explicitly through the API, documentation, or return type.

### Expected outcome vs programming error

In practice, it is better to separate:

- expected/recoverable: offline, invalid credentials, not found;
- programming error: invalid argument, impossible state, index error.

For a contract violation, an exception is appropriate:

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

For expected domain outcomes, a sealed result is better:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

This way, the caller sees known states through the type system.

### Java interop

To make a Java caller see `throws`, use `@Throws` in Kotlin:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

For a Kotlin caller, catch is still optional.

### Coroutines

Exceptions in coroutines are also unchecked. `launch` propagates failure through
the job hierarchy; `async` returns the exception through `await()`.

`CancellationException` is a cancellation signal and should not be swallowed:

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

**In short:** checked exceptions are Java's mechanism for mandatory
`catch/throws`; unchecked exceptions are not controlled by the compiler. Kotlin
has no checked exceptions, so expected errors are better modeled with return
types, while contract violations are exceptions.

</details>

<details>
<summary>145. What types of errors exist in Kotlin/Java?</summary>

#### Kotlin

On the JVM, everything that can be thrown with `throw` inherits from
`Throwable`. The two main branches are `Exception` and `Error`.

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

Kotlin uses the same JVM hierarchy, but it has no checked exceptions at the
compiler level.

### `Throwable`

The base type for all exceptions/errors. Catching `Throwable` is usually
dangerous: you can catch a fatal `Error` or coroutine cancellation.

### `Exception`

Situations that application code can potentially handle:

- `IOException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

### `RuntimeException`

An unchecked exception that often means a programming/contract error:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Examples: `NullPointerException`, `IndexOutOfBoundsException`,
`ClassCastException`, `NumberFormatException`.

### Checked vs unchecked

Java requires `catch` or `throws` for checked exceptions, such as `IOException`.
Kotlin does not require this. `RuntimeException` and its subclasses are unchecked
in both languages.

### `Error`

A serious JVM/runtime problem:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Usually, application code should not try to recover after an `Error`.

### Domain errors

Expected business failures are better modeled as a separate type, not as an
exception:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Technical exceptions are mapped to domain errors at the data layer boundary:

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

### `CancellationException`

In coroutines, this is a cancellation signal, not a regular failure:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

It should not be swallowed in a generic `catch`.

**In short:** `Throwable` is split into `Exception` and `Error`. Runtime
exceptions often mean contract/programming errors, expected business failures are
better modeled as domain types, and `CancellationException` in coroutines should
be propagated further.

</details>

<details>
<summary>146. What is the base class for errors?</summary>

#### Kotlin

The base class for everything that can be thrown with `throw` and caught with
`catch` is `Throwable`.

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

`Throwable` contains basic information:

- message;
- cause;
- stack trace;
- suppressed exceptions.

Only `Throwable` or its subclass can be thrown:

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // compile error
```

`Exception` means problems that application code can potentially handle:

```kotlin
throw IOException("No internet")
throw IllegalStateException("User is not authorized")
```

`Error` means serious JVM/runtime problems:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

They are usually not caught for recovery.

Custom exception:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

For expected business outcomes, a sealed type is often better than an exception:

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

Do not catch `Throwable` without a reason:

```kotlin
catch (throwable: Throwable) {
    log(throwable)
}
```

This can catch `Error` or `CancellationException`. In coroutines, cancellation
should be propagated further.

**In short:** the base class is `Throwable`. `Exception` and `Error` inherit from
it. In production, catch specific recoverable exceptions rather than the whole
`Throwable`.

</details>

<details>
<summary>147. What is algorithmic complexity?</summary>

#### Kotlin

Algorithmic complexity describes how execution time or additional memory grows
as input `n` increases. Big O is used most often.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

In an interview, it is important to clarify whether you mean average, worst-case,
or amortized complexity.

### `O(1)`

Cost does not grow with `n`:

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

Index access in `ArrayList` is `O(1)`. Lookup in `HashMap` is `O(1)` on average.

### `O(n)`

One pass over a collection:

```kotlin
fun findUser(users: List<User>, id: String): User? =
    users.firstOrNull { it.id == id }
```

`map`, `filter`, and `contains` for a list are usually `O(n)`.

### `O(n²)`

Nested comparison of many pairs:

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

For duplicates, `Set` is better:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = HashSet<String>()
    return items.any { !seen.add(it) }
}
```

Time is `O(n)` on average, space is `O(n)`. This is a trade-off of memory for
speed.

### `O(log n)`

Binary search over sorted data:

```kotlin
val index = sortedItems.binarySearch(target)
```

Search is `O(log n)`, but sorting before that can cost `O(n log n)`.

### `O(n log n)`

Typical complexity of comparison sorting:

```kotlin
val sorted = users.sortedBy(User::name)
```

### Time vs space

```kotlin
val copy = users.toList()
```

Time is `O(n)`, additional memory is `O(n)`. An in-place algorithm can save
memory, but it changes the input and complicates ownership/concurrency.

### Android example

Bad:

```kotlin
users.map { user ->
    user.id in selectedIds // selectedIds: List, contains O(m)
}
```

Together this is `O(n × m)`.

Better:

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

Building the Set is `O(m)`, mapping is `O(n)`, together `O(n + m)` on average.

Big O is not the same as real performance: constants, allocations, I/O, cache
locality, and main-thread budget also matter. After choosing the right data
structure, verify the result with a profiler/benchmark.

**In short:** complexity shows time/space scaling. `O(n²)` can often be improved
with `Set/Map`, but memory, average/worst case, and real measurements must be
considered.

</details>

<details>
<summary>148. What is Depth-First Search (DFS)?</summary>

#### Kotlin

`DFS` (`Depth-First Search`) is an algorithm for traversing a tree or graph that
goes deep along one path as far as it can, then backtracks and tries other
branches.

Example tree traversal:

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

A possible DFS order:

```text
A -> B -> D -> E -> C -> F
```

### DFS for a tree

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

For a tree, `visited` is usually not needed because there are no cycles.

### DFS for a graph

A graph can have cycles, so `visited` is needed:

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

Without `visited`, DFS can loop forever.

### Iterative DFS

Recursion can be replaced with a stack:

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

`ArrayDeque` is used as a stack through LIFO: `addLast/removeLast`.

### Complexity

For a graph:

```text
Time:  O(V + E)
Space: O(V)
```

`V` means vertices, `E` means edges. Space is needed for `visited` and the
recursion stack or explicit stack.

### DFS vs BFS

DFS:

- goes deep;
- uses recursion or a stack;
- is useful for cycle detection, connected components, topological sort;
- does not guarantee the shortest path in an unweighted graph.

BFS:

- goes level by level;
- uses a queue;
- finds the shortest path in an unweighted graph.

Recursive DFS can cause `StackOverflowError` on a very deep structure. In that
case, iterative DFS is better.

**In short:** DFS is depth-first traversal through recursion or a stack. For a
graph, `visited` is required, complexity is `O(V + E)`, and for very deep
structures the iterative variant is better.

</details>

<details>
<summary>149. What is Breadth-First Search (BFS)?</summary>

#### Kotlin

`BFS` (`Breadth-First Search`) is an algorithm for traversing a graph or tree
level by level. It first visits all vertices at distance 1 from the start, then
2, then 3, and so on.

BFS uses a queue with FIFO behavior:

```text
start -> neighbors -> neighbors of neighbors -> ...
```

### BFS for a graph

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

`visited` is needed to avoid looping forever in a graph with cycles.

### Shortest path in an unweighted graph

BFS finds the minimum number of edges from the start:

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

For a weighted graph, Dijkstra is needed instead of BFS.

### BFS for a tree

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

This is level-order traversal.

### Complexity

```text
Time:  O(V + E)
Space: O(V)
```

`V` means vertices, `E` means edges. For a tree, this is `O(n)`.

### BFS vs DFS

```text
BFS -> queue, level by level, shortest path in an unweighted graph
DFS -> stack/recursion, goes deep
```

BFS is often suitable for tasks like “minimum number of steps”, “nearest
element”, and “minimum distance in an unweighted grid”.

**In short:** BFS traverses a graph/tree breadth-first through a queue. It finds
the shortest path in an unweighted graph, needs `visited` for graphs with cycles,
and has complexity `O(V + E)`.

</details>

<details>
<summary>150. Tell us about Clean Architecture.</summary>

#### Kotlin

`Clean Architecture` is an approach to splitting code into layers with a clear
dependency direction. The main idea: business logic does not depend on UI, the
Android framework, database, or network client.

Typical Android scheme:

```text
presentation -> domain <- data
```

- `presentation` — Activity/Fragment/Compose, ViewModel, UI state;
- `domain` — use cases, business rules, entities, repository contracts;
- `data` — repository implementations, API, Room, DataStore, mappers.

Dependency rule:

```text
presentation depends on domain
data depends on domain
domain depends on nothing external
```

Domain should not know about Retrofit, Room, `Context`, Compose, or Android
lifecycle.

### Use case

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

The ViewModel calls a use case, not API/DAO directly.

### Repository contract

In domain:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

In data:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

The data layer works with DTOs/entities and maps them to domain:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

DTOs and Room entities should not be passed directly to UI if they have different
lifecycle/semantics.

### Presentation

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

The ViewModel manages UI state and does not know exactly where the data comes
from.

Advantages:

- business logic is tested without Android;
- data source can be replaced;
- UI is not tied to API/DB;
- boundaries are clear for the team;
- large products are easier to maintain.

Disadvantages:

- more files and boilerplate;
- for simple CRUD it can be overengineering;
- poor abstractions make code harder;
- dependency direction requires discipline.

Practical rule: do not create a use case/interface for every trivial getter.
Clean Architecture should reduce coupling and improve testability, not simply
multiply classes.

**In short:** in Android, Clean Architecture usually splits code into
presentation, domain, and data. Domain contains business rules and contracts,
presentation renders state, and data implements API/DB. The main goal is
dependency control and testability.

</details>

<details>
<summary>151. What is MVVM?</summary>

#### Kotlin

`MVVM` is the `Model-View-ViewModel` architectural pattern, which separates UI
from state management and business logic.

```text
View -> ViewModel -> Model
```

- `View` — renders UI and sends user actions;
- `ViewModel` — holds screen state, handles actions, calls use cases;
- `Model` — domain/data layer: use cases, repositories, API, DB.

### View

Compose UI should receive state and callbacks:

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

The View does not call API/DB directly and does not contain business logic.

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

The ViewModel survives configuration changes and does not depend on a concrete
View implementation.

### Model layer

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

The repository hides API, Room, cache, and DataStore.

### Data flow

```text
User action -> ViewModel -> UseCase/Repository -> State update -> View renders state
```

Screen state is better kept explicit:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

Navigation, snackbar, and toast are one-shot effects; they should not be stored
as persistent state:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Advantages:

- less logic in the View;
- ViewModel is easy to test;
- state survives rotation;
- UI can be changed without rewriting domain/data;
- works well with Compose, XML, StateFlow/LiveData.

Common mistakes:

- Fragment/Composable contains business logic;
- ViewModel works directly with Retrofit/Room without a boundary;
- DTO/entity is passed directly to UI without a reason;
- many independent mutable states instead of screen state;
- one-shot events are stored as persistent state.

**In short:** MVVM means the View renders state, the ViewModel manages
state/actions, and Model/use cases/repositories perform business logic and data
access. Good MVVM keeps UI thin and ViewModel testable.

</details>

<details>
<summary>152. What is the difference between MVVM and MVI?</summary>

#### Kotlin

`MVVM` and `MVI` are approaches to UI state management. Both separate UI from
business logic, but they organize state, events, and side effects differently.

### MVVM

```text
View -> ViewModel -> Model
```

- `View` renders UI and calls ViewModel methods;
- `ViewModel` holds state and calls use cases/repositories;
- `Model` is the domain/data layer.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() { /* update state */ }
    fun retry() { /* update state */ }
}
```

MVVM is simpler and more flexible. But without discipline, a ViewModel can turn
into a random set of methods and mutable states.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
                                 └-> Effect
```

UI sends events/intents instead of calling many different methods:

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

Usually there is one immutable screen state:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

There is often one entry point:

```kotlin
fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.LoadClicked -> load()
        ProfileEvent.RetryClicked -> retry()
    }
}
```

### State and effects

In MVVM, state can be one object or several streams. In MVI, there is usually one
screen state and a formal reducer/handler.

In both approaches, navigation/snackbar/toast should not be stored as persistent
state. Effects are used for this:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

### When to choose what

MVVM:

- simpler;
- less boilerplate;
- suitable for most Android screens;
- easier for a team to start with.

MVI:

- predictable unidirectional data flow;
- one immutable state;
- convenient for testing event/reducer logic;
- better for complex stateful screens;
- useful for large teams with one shared flow.

The downside of MVI is boilerplate for simple screens. The downside of MVVM is
the risk of chaotic state management.

**In short:** MVVM is simpler: View calls ViewModel methods and renders state.
MVI is more formal: UI sends events, ViewModel/reducer creates immutable state,
and one-shot actions go through effects. MVVM fits most screens; MVI fits complex
stateful flows.

</details>

<details>
<summary>153. What architectural patterns are used in Android?</summary>

#### Kotlin

In Android, the most common patterns are MVVM, MVI, Clean Architecture,
Repository, Use Case, layered architecture, DI, and feature modularization. The
choice depends on app complexity, team size, and testability requirements.

### MVVM

```text
View -> ViewModel -> Model
```

UI renders state, while ViewModel handles user actions and calls the domain/data
layer.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Works well with Compose, XML, StateFlow, and LiveData.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
```

UI sends events, ViewModel creates immutable state, and one-shot actions go
through effects.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

Useful for complex stateful screens.

### Clean Architecture / Layered Architecture

```text
presentation -> domain <- data
```

- `presentation` — UI/ViewModel;
- `domain` — use cases, business rules, contracts;
- `data` — repository implementations, API, DB.

Domain should not depend on the Android framework, Retrofit, or Room.

### Repository pattern

Repository hides data sources:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

The implementation can work with API, Room, cache, or DataStore, but the
ViewModel does not know this.

### Use Case pattern

A use case describes a specific business action:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

This prevents the ViewModel from growing business logic.

### Feature modularization

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Feature-first modules help scale the codebase and ownership. Core should contain
truly shared infrastructure.

### Dependency Injection

DI is not a UI pattern, but it is part of architecture:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt/Koin/manual DI connects contracts with implementations.

Practical choice:

- simple app — MVVM + Repository;
- complex screen state — MVI;
- large app — Clean Architecture + modularization;
- offline-first — Repository + local source of truth;
- many teams — feature modules + clear contracts.

**In short:** Android architecture usually combines MVVM/MVI for UI state,
Clean/layers for dependency direction, Repository/UseCase for domain/data
boundaries, DI for wiring, and feature modules for scaling. A pattern should
reduce complexity, not create boilerplate.

</details>

<details>
<summary>154. How should modules be organized in an Android project?</summary>

#### Kotlin

Modules in an Android project are needed for isolation, build performance,
feature ownership, and parallel team work. What matters is not the number of
modules, but correct boundaries and dependency direction.

Typical structure:

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

A small app does not need dozens of modules. It is better to start simpler.

### `:app`

Application entry point:

- `Application`;
- main `Activity`;
- navigation host;
- DI assembly;
- build flavors/config;
- connecting feature/data modules.

Business logic should not live in `:app`.

### Feature modules

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

A feature contains UI/presentation logic for a specific feature. It should work
through domain contracts/use cases, not directly through Retrofit/Room
implementations.

### Core modules

```text
:core:network  -> Retrofit/OkHttp setup
:core:database -> Room setup
:core:ui       -> design system/components
:core:common   -> utils, dispatchers, result types
```

Core should not depend on a feature.

### Domain modules

Domain contains business rules and contracts:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

Domain should be as independent as possible from the Android framework.

### Data modules

Data implements domain contracts:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

DTOs, Room entities, mappers, and API/DB data sources live here.

### Dependency direction

```text
feature -> domain
 data   -> domain
 app    -> feature + data
core    -> feature does not know; feature may know core
```

Avoid cycles. `core` should not know about a specific feature.

### Build logic

Repeated Gradle configuration is better moved into convention plugins:

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

This reduces build script duplication.

When not to split:

- there is no separate ownership;
- there is no reusable boundary;
- there is no build-time benefit;
- the module only adds DI/navigation/Gradle boilerplate.

**In short:** modules should be built around `app`, `feature`, `core`, `domain`,
and `data`. Split only when there is a real boundary: feature ownership, reusable
infrastructure, domain contract, data implementation, or build benefit. Excessive
modularity is harmful.

</details>

<details>
<summary>155. What is a token?</summary>

#### Kotlin

A token is a credential that confirms a session or access right. In
mobile/backend systems, there are usually an `access token` and a `refresh token`.

### Access token

Has a short TTL and is sent in requests to a protected API:

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

Lives longer and is used only to refresh the access token:

```text
request -> 401 -> refresh -> new access token -> retry request
```

Refresh must be synchronized: several parallel `401` responses must not start
several refresh requests. Retry should be limited to avoid an infinite loop.

### JWT and Bearer

JWT has the format:

```text
header.payload.signature
```

The payload is only Base64Url encoded, not encrypted. Therefore, secrets should
not be placed in a JWT payload.

A Bearer token can be used by anyone who has it. If the token leaks, access is
possible until expiration or revoke.

### Storage in Android

Sensitive storage:

- Android Keystore;
- EncryptedSharedPreferences;
- encrypted DataStore.

It is better to hide the implementation behind an interface:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Tokens must not appear in logs, crash reports, analytics, screenshots, URL query
parameters, or plain files.

### Expiration and scopes

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)
```

Local expiration checks help refresh a token in advance, but the backend is the
source of truth. If refresh fails, clear the session and send the user to login.

Scopes should be minimal: `profile:read`, `payments:create`. A mobile app should
not receive admin permissions or server secrets.

Logout should delete local tokens and, if the backend supports it, revoke the
refresh token.

**In short:** a token is an access credential. Access token goes into API
requests; refresh token renews the access token. Important points: short TTL,
minimal scopes, secure storage, synchronized refresh, and no tokens in logs.

</details>

<details>
<summary>156. What is the difference between authentication and authorization?</summary>

#### Kotlin

```text
Authentication -> who are you?
Authorization  -> what are you allowed to do?
```

Authentication establishes identity/principal. Authorization checks the policy
for a specific operation or resource.

### Authentication

Identity is confirmed by password, OTP, passkey/biometrics, OAuth/OIDC, or a
valid session.

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

After login, the backend issues a session cookie or tokens. The access token is
added to API requests; the refresh token is used only to obtain a new access
token.

### Authorization

Authorization checks whether the principal has permission to perform an action:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

The backend considers roles, permissions, resource ownership, tenant, and
context.

Common models:

- RBAC — roles;
- permission-based — granular permissions;
- ABAC/policy-based — attributes of principal/resource/context.

### `401` vs `403`

```text
401 Unauthorized -> no valid authentication credentials
403 Forbidden    -> credentials exist, but access is denied
```

On `401`, the app can perform one synchronized token refresh. If refresh fails,
end the session.

On `403`, re-authentication usually does not help: the UI should show that there
is no access.

### Android client vs backend

The client can hide buttons based on permissions:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDelete) {
        Text("Delete")
    }
}
```

But this is only UX. A modified client can call the API directly, so the backend
must always enforce authorization.

Responsibility split:

- client stores credentials and handles `401/403`;
- auth service issues/verifies credentials;
- policy layer decides access;
- backend does not trust permissions received from the client;
- logout/revoke ends the session according to the security model.

**In short:** authentication confirms identity; authorization checks access to an
operation/resource. The client is responsible for credentials and UX, but real
security enforcement must always happen on the backend.

</details>

<details>
<summary>157. What tools are used to communicate with the backend?</summary>

#### Kotlin

The networking stack depends on the protocol: REST/HTTP, GraphQL, WebSocket,
gRPC, or KMP networking. Separately, serialization, auth, error mapping, and
cache/offline strategy are needed.

### REST/HTTP

Most common: Retrofit + OkHttp.

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

OkHttp executes requests; Retrofit describes the API as a Kotlin interface.

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Interceptors add headers, auth, logging, and tracing. Retry should take
idempotency into account.

### Serialization

For JSON, use Kotlin Serialization, Moshi, or Gson.

DTOs are better separated from domain:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### KMP

For Kotlin Multiplatform, Ktor Client is often used. Engine and platform-specific
configuration are provided separately.

### GraphQL

Apollo Kotlin generates type-safe models from GraphQL operations. GraphQL adds
schema/codegen, its own caching, and specific error handling.

### WebSocket

For realtime: chat, live updates, market data. On Android, WebSocket is supported
by OkHttp and Ktor.

### gRPC

gRPC is contract-first RPC through protobuf and generated clients. It is
appropriate if the backend already has protobuf/gRPC infrastructure.

### Authentication

Common options:

- Bearer access/refresh tokens;
- OAuth/OIDC;
- cookies;
- API keys.

An interceptor adds credentials. `Authenticator` or a separate session manager
coordinates refresh after `401`. Refresh must be synchronized so parallel `401`
responses do not start many refresh requests.

### Error handling and cache

The network layer should map transport/HTTP errors into its own domain/data
error. UI should not depend on `HttpException` or a specific client.

Offline/cache:

- HTTP cache — response cache;
- Room — structured source of truth;
- Paging + RemoteMediator — paged local/remote sync;
- Repository — defines where to read from and when to refresh.

Practical choice:

- REST — Retrofit + OkHttp;
- KMP — Ktor Client;
- GraphQL — Apollo Kotlin;
- realtime — WebSocket;
- protobuf RPC — gRPC;
- offline source of truth — Room + Repository.

**In short:** for REST, use Retrofit/OkHttp; for KMP, Ktor; for GraphQL, Apollo;
for realtime, WebSocket; for RPC, gRPC. Requests are not enough: auth, error
mapping, DTO/domain mapping, and offline strategy are also important.

</details>

<details>
<summary>158. What is Retrofit?</summary>

#### Kotlin

`Retrofit` is a library for working with REST APIs on Android/JVM. It describes
HTTP endpoints through Kotlin/Java interfaces, and requests are executed through
OkHttp.

API interface:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto

    @POST("users")
    suspend fun createUser(@Body body: CreateUserRequest): UserDto
}
```

Retrofit creates an implementation of this interface at runtime.

Setup:

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val api = retrofit.create(UserApi::class.java)
```

`baseUrl` must end with `/`.

Retrofit supports `suspend` functions:

```kotlin
val user = api.getUser("123")
```

Annotations describe the HTTP request:

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

### Converters

Retrofit does not parse JSON by itself. A converter is needed:

- Moshi;
- Gson;
- Kotlin Serialization;
- Scalars.

DTOs are better mapped to domain models:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### Response handling

You can return a DTO directly or `Response<T>`:

```kotlin
suspend fun getUser(id: String): Response<UserDto>
```

`Response<T>` is needed if you need to read the status code, headers, or error
body.

### OkHttp interceptors

Auth, headers, logs, and cache are added through OkHttp:

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

### Error handling

Retrofit/OkHttp can produce:

- `IOException` — network error;
- `HttpException` or non-2xx `Response`;
- serialization exception.

In a repository, this is better mapped to a domain/data error instead of exposing
`HttpException` to UI.

Production boundary:

```text
ViewModel -> UseCase -> Repository -> Retrofit API
```

**In short:** Retrofit describes REST endpoints through interfaces and
annotations, works on top of OkHttp, and supports coroutines and JSON converters.
In production, Retrofit API and DTOs should live in the data layer behind a
repository.

</details>

<details>
<summary>159. What is Apollo GraphQL?</summary>

#### Kotlin

Apollo Kotlin is a type-safe GraphQL client for Kotlin/Android. It generates
Kotlin code from a schema and `.graphql` operations, and supports queries,
mutations, subscriptions, and normalized cache.

In GraphQL, the client describes the fields it needs:

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

Apollo generates Kotlin types:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

If a query does not match the schema, the error appears during code generation or
compile time.

Client setup:

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

Auth is added with an interceptor:

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

Query:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()
val user = response.data?.user
```

Mutation:

```kotlin
apolloClient.mutation(UpdateUserNameMutation(id, name)).execute()
```

Subscription for realtime updates:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
    }
```

### Cache and errors

Normalized cache stores entities by key so different queries can reuse the same
data. This depends on stable IDs in the schema.

A GraphQL response can contain both `data` and `errors`:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()

if (response.hasErrors()) {
    // map response.errors to domain errors
}

val data = response.data
```

A GraphQL error does not necessarily mean HTTP 4xx/5xx. Network failure usually
comes as an exception. Generated models are better mapped to domain models.

Apollo vs Retrofit:

- Apollo — GraphQL, client defines response shape, models are generated;
- Retrofit — REST, backend defines endpoints/response, DTOs are written manually.

**In short:** Apollo Kotlin is a type-safe GraphQL client with codegen, queries,
mutations, subscriptions, and normalized cache. It is used when the backend works
through GraphQL rather than REST endpoints.

</details>

<details>
<summary>160. What is Firebase used for?</summary>

#### Kotlin

`Firebase` is a set of backend-as-a-service tools from Google for mobile/web
apps. In Android, it is used for analytics, crash reporting, push notifications,
auth, remote config, database/storage, and backend functions.

### Analytics

Collecting user events, funnels, retention, and feature usage:

```kotlin
firebaseAnalytics.logEvent(
    "profile_opened",
    bundleOf("source" to "home")
)
```

### Crashlytics

Crash and non-fatal monitoring:

```kotlin
Firebase.crashlytics.recordException(error)
```

A basic production tool for app stability.

### FCM

Firebase Cloud Messaging — push notifications and messaging events:

```kotlin
class AppMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // handle push
    }
}
```

### Firebase Auth

Ready-made auth platform:

- email/password;
- Google Sign-In;
- phone auth;
- anonymous auth;
- custom tokens.

It must be correctly integrated with the backend/security model.

### Firestore / Realtime Database

NoSQL cloud databases with realtime updates:

```kotlin
firestore.collection("users").document(id).get()
```

Suitable for chat, realtime sync, collaborative features, and prototypes.
Security rules are critical.

### Remote Config

Changing parameters without releasing the app:

```kotlin
val enabled = remoteConfig.getBoolean("new_feature_enabled")
```

Use cases: feature flags, rollout, A/B testing, UI/behavior config.

### Storage and Functions

Cloud Storage — user uploads: images, videos, files.

```kotlin
storage.reference.child("avatars/$userId.jpg")
```

Cloud Functions — server-side logic: Firestore triggers, API endpoints, push
dispatch, validation, third-party integrations.

Advantages:

- fast start;
- ready-made auth/analytics/crashes/push;
- good Android integration;
- realtime features;
- less custom backend infrastructure at the start.

Risks:

- vendor lock-in;
- security rules mistakes;
- pricing surprises during growth;
- not always suitable for complex backend domain logic;
- client-side access does not replace backend security;
- offline/cache behavior needs separate understanding.

**In short:** Firebase accelerates Android development with ready-made auth,
analytics, Crashlytics, FCM, Firestore/Realtime DB, Remote Config, Storage, and
Functions. But it requires security rules, cost control, boundaries, and
understanding vendor lock-in.

</details>

<details>
<summary>161. What is Android Keystore?</summary>

#### Kotlin

`Android Keystore` is the system storage for cryptographic keys. It allows
creating and using keys so that raw key material does not leave the Keystore. On
some devices, keys can be hardware-backed.

Keystore is used for:

- encryption/decryption;
- signing/verification;
- local secrets;
- keys for EncryptedSharedPreferences/DataStore encryption;
- biometric/user-auth protected keys;
- reducing the risk of raw crypto key leakage.

Important limitation: Keystore does not make a hardcoded API key safe. If a
secret is inside the APK, an attacker can find where the app uses it. Keystore is
best suited for keys generated on the device.

Generating an AES key:

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

Getting the key:

```kotlin
val keyStore = KeyStore.getInstance("AndroidKeyStore").apply { load(null) }
val key = keyStore.getKey("secret_key", null) as SecretKey
```

You get a handle to the key, not raw bytes.

Encryption:

```kotlin
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)
val iv = cipher.iv
val encrypted = cipher.doFinal(data)
```

For AES-GCM, store the `iv` next to the ciphertext and never reuse an IV with the
same key.

A key can be tied to user authentication:

```kotlin
.setUserAuthenticationRequired(true)
```

For typical tasks, Jetpack Security is better:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()
```

Keystore/Jetpack Security is suitable for refresh tokens, local encryption keys,
user-specific secrets, and sensitive preferences. Server/backend secrets must not
be placed in the app; a backend proxy is needed.

Risks:

- incorrect crypto mode;
- IV reuse;
- lack of key rotation;
- backup/restore edge cases;
- different behavior across Android versions/devices;
- trying to hide a hardcoded secret instead of using the correct architecture.

**In short:** Android Keystore protects cryptographic keys on the device and does
not expose raw key material. It is useful for local encryption/signing and user
tokens, but it does not protect hardcoded API secrets in an APK. For typical
tasks, Jetpack Security on top of Keystore is preferable.

</details>

<details>
<summary>162. What dispatchers exist in Kotlin Coroutines, and what are they used for?</summary>

#### Kotlin

`CoroutineDispatcher` defines which thread or thread pool a coroutine runs on.
The correct dispatcher is important for UI responsiveness, performance, and
avoiding ANR.

### `Dispatchers.Main`

Main UI thread:

```kotlin
withContext(Dispatchers.Main) {
    textView.text = "Loaded"
}
```

For UI updates, Compose/View state, navigation, and lifecycle-bound UI logic. Not
for network, DB, file I/O, or heavy calculations.

### `Dispatchers.IO`

For blocking I/O:

```kotlin
withContext(Dispatchers.IO) {
    file.readText()
}
```

Suitable for file operations, database, network clients, and blocking SDK calls.
Not for CPU-heavy algorithms.

### `Dispatchers.Default`

For CPU-heavy work:

```kotlin
withContext(Dispatchers.Default) {
    largeList.sortedBy { it.score }
}
```

Suitable for sorting, parsing large structures, compression, diff calculation,
and computations.

### `Dispatchers.Unconfined`

Does not bind a coroutine to a specific thread and resumes where the suspension
completed.

```kotlin
launch(Dispatchers.Unconfined) { }
```

In production Android code, it is almost never needed because behavior is less
predictable.

### `limitedParallelism`

```kotlin
val limitedIo = Dispatchers.IO.limitedParallelism(4)
```

Useful for limiting parallel uploads, API calls, batch jobs, and image
processing.

### `withContext`

For switching dispatcher inside a suspend function:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

This is better than launching a new coroutine only to change the thread.

### Inject dispatchers

For testability, dispatchers are better passed through the constructor:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): User = withContext(ioDispatcher) {
        api.getUser()
    }
}
```

In a test, `StandardTestDispatcher` can be passed.

Common mistakes:

- network/DB on `Main`;
- CPU-heavy work on `IO`;
- `runBlocking` on the main thread;
- hardcoded dispatchers in code that needs testing;
- custom thread pools without cleanup;
- `Unconfined` without a clear reason.

**In short:** `Main` is for UI, `IO` is for blocking I/O, `Default` is for
CPU-heavy work, and `Unconfined` is a rare special case. For testability,
dispatchers are better injected, and dispatcher switching should be done with
`withContext`.

</details>

<details>
<summary>163. What are the ways to start coroutines?</summary>

#### Kotlin

A coroutine is started in a `CoroutineScope`. The most common tools are `launch`,
`async`, `coroutineScope`, `supervisorScope`, and Android-specific scopes.

### `launch`

Starts a coroutine without a business result and returns a `Job`:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

```kotlin
val job = scope.launch { work() }
job.cancel()
```

Suitable for side effects. Failure is handled through the scope hierarchy or a
local `try/catch`.

### `async`

Returns `Deferred<T>` for a concurrent result:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

If you immediately call `await()` without parallelism, it is better to call the
suspend function directly.

### `coroutineScope`

Creates a structured boundary and waits for all children:

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

Failure of one child cancels siblings and is passed to the caller.

### `supervisorScope`

Isolates siblings from a direct child failure:

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

Failure still needs to be handled or received through `await()`.

### `runBlocking`

Blocks the caller thread. Appropriate for a CLI entry point or a very limited
sync bridge. In tests, prefer `runTest`; on Android Main, do not use it because
of ANR risk.

### Android scopes

`viewModelScope` — for screen operations, cancelled in `onCleared()`.

`lifecycleScope` — belongs to `LifecycleOwner`; for Flow in UI, use
`repeatOnLifecycle`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

In Fragment, for View/binding, use `viewLifecycleOwner.lifecycleScope`.

In Compose, `rememberCoroutineScope()` is for callback-driven UI actions:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

`GlobalScope` is almost always wrong in app code: it has no owner, lifecycle, or
cleanup.

**In short:** `launch` is for side effects, `async` is for concurrent results,
`coroutineScope` is a fail-fast boundary, and `supervisorScope` isolates
siblings. Choose a scope by the work owner; do not use `GlobalScope` or
`runBlocking` on Main.

</details>

<details>
<summary>164. What is the difference between launch and async?</summary>

#### Kotlin

`launch` and `async` are coroutine builders.

```text
launch -> Job, no result value
async  -> Deferred<T>, result through await()
```

### `launch`

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

`Job` allows `cancel()`, `join()`, and state checks. `launch` is used when a
result does not need to be returned outward: UI state update, Flow collection,
save, sync.

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

`Deferred<T>` is a `Job` with a result or exception. `async` is mainly needed for
several concurrent results:

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

Without `async`, suspend calls would run sequentially. But actual parallel
execution depends on the dispatcher.

### Exceptions and structured concurrency

In `coroutineScope`, failure of one child cancels siblings and is passed to the
caller. An `async` exception is received through `await()`, but in a structured
scope the failure still affects the parent.

For independent failures, use `supervisorScope` and an explicit error policy.

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

Do not swallow `CancellationException`.

### Common mistakes

Bad:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

If a value is not needed, use `launch`. If one value is needed without
parallelism, call the suspend function directly. For dispatcher switching, use
`withContext`, not `async`.

Practical choice:

- side effect without result — `launch`;
- several concurrent results — `async + await`;
- one result — suspend function;
- dispatcher switch — `withContext`;
- related children — `coroutineScope`;
- independent failures — `supervisorScope`.

**In short:** `launch` returns a `Job` for work without a result. `async` returns
`Deferred<T>` and is needed for concurrent results through `await()`. Do not use
`async` if the result is not needed.

</details>

<details>
<summary>165. How do you handle errors in coroutines?</summary>

#### Kotlin

The strategy depends on ownership. Expected failures are handled locally and
mapped to result/state. Unhandled exceptions propagate through the `Job`
hierarchy. `CancellationException` must not be swallowed.

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

It is better to catch specific exceptions at a boundary where it is clear what to
do with them.

In a ViewModel, errors are mapped to UI state instead of exposing raw
`HttpException`/`IOException` to UI.

### `CancellationException`

With a broad catch, cancellation must be passed further:

```kotlin
catch (error: CancellationException) {
    throw error
} catch (error: Exception) {
    handleError(error)
}
```

Otherwise, cooperative cancellation can be broken.

### `CoroutineExceptionHandler`

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

The handler is a last resort for an uncaught exception in a root `launch`,
usually for logging. It does not recover the coroutine and does not replace local
error handling.

### `async` exceptions

`await()` returns the result or throws an exception:

```kotlin
val deferred = async { api.getUser() }

try {
    val user = deferred.await()
} catch (error: IOException) {
    handleError(error)
}
```

In a regular `coroutineScope`, child failure cancels the parent and siblings.
`try/catch` around `await()` does not always isolate parallel tasks. For
independent failures, use `supervisorScope`.

### `coroutineScope` vs `supervisorScope`

```text
coroutineScope   -> fail-fast, child failure cancels siblings
supervisorScope  -> child failure does not cancel siblings automatically
```

```kotlin
supervisorScope {
    val a = async { runCatching { requestA() } }
    val b = async { runCatching { requestB() } }
}
```

Failure still needs to be handled.

### `Flow.catch`

```kotlin
repository.users()
    .catch { error -> emit(emptyList()) }
    .collect { users -> render(users) }
```

`catch` catches upstream exceptions. Do not turn cancellation into a fallback.

Common mistakes:

- swallowing `CancellationException`;
- relying only on `CoroutineExceptionHandler`;
- ignoring `await()` failure;
- passing raw exceptions to UI;
- mixing retry, mapping, and rendering in one place.

**In short:** expected failures are mapped to result/state through `try/catch`,
`await()`, or `Flow.catch`. `coroutineScope` is fail-fast, `supervisorScope`
isolates siblings, and `CancellationException` is always propagated further.

</details>

<details>
<summary>166. How do you wait for the result of several parallel requests?</summary>

#### Kotlin

Independent suspend requests are started with `async` in a structured scope and
waited for with `await()` or `awaitAll()`.

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

Both `async` calls are created before the first `await()`, so the requests run
concurrently. Real parallelism depends on the dispatcher and HTTP client.

### `coroutineScope`

`coroutineScope`:

- waits for all children;
- propagates caller cancellation downward;
- works fail-fast: failure of one child cancels siblings.

This is the correct policy if all results are required.

### `awaitAll`

For a list of same-type requests:

```kotlin
val users = coroutineScope {
    ids.map { id ->
        async { api.getUser(id) }
    }.awaitAll()
}
```

`awaitAll()` preserves the order of the deferred list and returns failure
fail-fast.

Dependent calls should not be parallelized:

```kotlin
val token = api.login()
val profile = api.getProfile(token)
```

### Partial success

If a partial result is needed, use `supervisorScope` and a typed result for each
child:

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

`runRequest` should rethrow `CancellationException` and map only expected
failures.

### Timeout

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

Coroutine timeout cancels the scope. The HTTP client should still have its own
connect/read/call timeouts.

### Limit in-flight requests

For large fan-out:

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

`Semaphore` limits the number of simultaneous requests. For large sets, batching
is also used.

Practical choice:

- all results are required — `coroutineScope + async`;
- list of requests — `awaitAll()`;
- partial success — `supervisorScope + typed errors`;
- large fan-out — `Semaphore` or batching;
- dependent calls — sequentially.

**In short:** independent parallel requests are started with `async` inside
`coroutineScope`. `awaitAll()` waits for a list. A regular scope is fail-fast,
`supervisorScope` gives partial results, and fan-out is limited with
`Semaphore`/batching.

</details>

<details>
<summary>167. Why were Fragments created?</summary>

#### Kotlin

`Fragment` was created as a reusable part of UI and behavior inside an
`Activity`. The idea is not to keep the whole screen flow in one large Activity,
but to split UI into smaller components with their own lifecycle and layout.

Problem without Fragment:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // navigation, list, details, clicks, state
    }
}
```

Such an Activity quickly becomes a god class.

Fragment as part of a screen:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // list UI logic
    }
}
```

A Fragment can encapsulate a list, details, form, step flow, or tabs.

A historically important use case is phone/tablet layouts:

```text
phone  -> Activity + ListFragment
tablet -> Activity + ListFragment + DetailsFragment
```

An Activity could be a container for several Fragments and replace them through
FragmentManager:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Later, Navigation Component made this more convenient.

A Fragment has its own lifecycle and a separate View lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

The main nuance: the Fragment object can live longer than its View. Therefore,
binding, adapters, and UI subscriptions should be cleared in `onDestroyView()`.

Fragments can have their own or shared ViewModel:

```kotlin
private val viewModel: ProfileViewModel by viewModels()
private val shared: SharedViewModel by activityViewModels()
```

This is useful for master-detail, wizard flow, or shared UI state.

In Compose, the role of Fragment is smaller: often one Activity hosts `NavHost`,
and screens are composables. But Fragment is still relevant for legacy View
System, hybrid apps, Navigation Component setups, and gradual migrations.

**In short:** Fragment was created for modular UI in the View System: reusable
blocks, phone/tablet layouts, navigation inside Activity, and back stack. Its
main complexity is lifecycle, especially the difference between Fragment
lifecycle and View lifecycle.

</details>

<details>
<summary>168. What problems do they solve?</summary>

#### Kotlin

Fragments solve the problem of decomposing View-based UI inside an `Activity`:
large Activities, navigation, back stack, lifecycle of separate screen/container
parts, and different layouts for phone/tablet.

### Large Activities

Without decomposition, an `Activity` quickly becomes a god class:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // home, details, settings, navigation, state
    }
}
```

Fragments split UI:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

The `Activity` remains a host, while state/business logic should live in
`ViewModel`/domain layer.

### Different layouts

```text
phone  -> Activity + one Fragment
tablet -> Activity + list Fragment + details Fragment
```

Phone can show list/details sequentially; tablet can show them simultaneously.

### Navigation and back stack

Fragments allow changing parts of the screen without starting a new Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Navigation Component makes this more controlled through a navigation graph.

### UI lifecycle

A Fragment has a lifecycle and a separate View lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Binding, adapters, and UI subscriptions should be cleared in `onDestroyView()`
because the View can be destroyed before the Fragment object.

### Responsibility isolation

A Fragment is responsible for rendering, lifecycle, and UI events of a specific
screen:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

But a Fragment should not contain business logic.

### What Fragments do not solve

- poor architecture;
- mixing UI and business logic;
- incorrect state ownership;
- memory leaks after `onDestroyView()`;
- confusing navigation graph;
- excessive nested UI.

In the View System, Fragment is convenient as a screen/container. In full Compose
UI, its role often shrinks or disappears.

**In short:** Fragments solve decomposition of View-based UI,
navigation/back stack, phone/tablet layouts, and lifecycle of screen parts. But
they do not automatically fix architecture, state management, or memory leaks.

</details>

<details>
<summary>169. What are the specifics of nested Fragments?</summary>

#### Kotlin

A nested Fragment lives inside another Fragment and is managed through
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

`parentFragmentManager` manages Fragments at the parent container level, not the
child Fragments of the current Fragment.

### Lifecycle and ViewModel scope

A child Fragment has its own lifecycle, view lifecycle, state, and ViewModel, but
it cannot live longer than the parent.

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()` — child Fragment scope;
- `requireParentFragment()` — shared state with the parent;
- `activityViewModels()` — Activity scope.

### Navigation and back stack

A child Fragment can have its own back stack or `NavHostFragment`. You must
clearly understand who handles Back:

```kotlin
childFragmentManager.popBackStack()
```

Several nested back stacks quickly complicate screen behavior, so nested
navigation should be added only when needed.

### Communication

Do not keep direct references between Fragments. Better options:

- shared ViewModel;
- Fragment Result API;
- navigation result.

```kotlin
childFragmentManager.setFragmentResultListener(
    "key",
    viewLifecycleOwner
) { _, bundle ->
    val value = bundle.getString("value")
}
```

### Cleanup

Parent and child have separate view lifecycles. View references, adapters, and
callbacks are cleared in `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

In `ViewPager2`, an adapter inside a Fragment is created with a Fragment owner:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment)
```

Nested Fragments are appropriate for tabs, a local wizard flow, or an independent
block with its own lifecycle. For simple UI, a custom View or composable is
better.

**In short:** nested Fragments are managed through `childFragmentManager`, have
their own lifecycle/ViewModel/back stack, but depend on the parent. Main risks:
wrong manager, unnecessary nested navigation, and leaks after `onDestroyView()`.

</details>

<details>
<summary>170. What callbacks does Application have?</summary>

#### Kotlin

`Application` represents the Android process. Each app process has its own
`Application` instance.

Main callbacks:

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

This is where app-wide infrastructure is started: DI, logging, crash reporting,
SDKs. The callback runs on the main thread and affects cold start, so
initialization should be minimal or lazy.

In the manifest:

```xml
<application android:name=".App" />
```

For Hilt, the class is annotated with `@HiltAndroidApp`.

Important nuance: ContentProvider-based initializers can start earlier than
`Application.onCreate()`.

### Memory callbacks

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

These are signals to free recreatable memory: image cache, buffers, precomputed
data. Persisted state must not be deleted.

### Configuration

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

App-wide configuration changes. UI reaction is better kept in
Activity/Fragment/Compose, where there is an actual lifecycle and themed
`Context`.

### `ActivityLifecycleCallbacks`

Application can listen to the lifecycle of all Activities:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityStarted(activity: Activity) = Unit
        override fun onActivityResumed(activity: Activity) = Unit
        override fun onActivityStopped(activity: Activity) = Unit
    }
)
```

Use cases: analytics, session tracking, debug tooling. For foreground/background,
`ProcessLifecycleOwner` is often better.

Do not keep a strong reference to an Activity after the callback.

### `onTerminate()`

`onTerminate()` is not production cleanup. Android can kill the process without
calling it. Do not rely on it for saving data, logout, transactions, or resource
release.

What not to do:

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

This is an Activity leak, global mutable state, and data loss after process
death. `Application` should not be a service locator or session storage.

**In short:** `Application.onCreate()` is fast app-wide initialization.
`onTrimMemory/onLowMemory` frees recreatable memory. `onTerminate()` is not
guaranteed. Activity lifecycle can be observed through
`ActivityLifecycleCallbacks`, but without storing Activity references.

</details>

<details>
<summary>171. What singleton does the system create when an Android app starts?</summary>

#### Kotlin

When an Android process starts, the system creates an `Application` instance. It
is an application-level singleton within a specific process.

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
    }
}
```

In the manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Android creates `App` and calls `onCreate()` before starting `Activity`,
`Service`, `BroadcastReceiver`, and other components in this process.

### Singleton only within a process

`Application` is not a singleton for the whole device and is not always one for
the whole app if there are multiple processes.

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

Then there will be two instances:

```text
main process  -> App instance #1
:sync process -> App instance #2
```

Therefore, `Application`, Kotlin `object`, and static state cannot be considered
shared storage between processes.

### What Application is used for

- DI setup;
- crash reporting;
- logging;
- analytics setup;
- app-wide SDK initialization;
- `ActivityLifecycleCallbacks`;
- lightweight global configuration.

For Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

### Application context

For long-lived dependencies, use `applicationContext`, not `Activity` context:

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

### What not to do

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Problems:

- state disappears after process death;
- an `Activity` reference creates a memory leak;
- business logic becomes global mutable state;
- tests become dependent on shared state.

Critical state should be stored in DB, DataStore, files, or backend.

### Application vs Kotlin object

Kotlin `object` is created lazily on first access. `Application` is created by
the Android framework and has process-level `Context`. Both live only within the
process and are lost after process death.

**In short:** Android creates the process-level singleton `Application`. It is
suitable for app-wide initialization, but not for storing session/business state
and must not hold UI references.

</details>

<details>
<summary>172. How does BackStack work?</summary>

#### Kotlin

`BackStack` is a stack of navigation history. It defines where the user returns
after Back. The principle is LIFO: the last added screen exits first.

```text
Home -> Details -> Settings
```

Back:

```text
Settings removed -> Details visible
Details removed  -> Home visible
```

### Activity back stack

A new Activity is added to the task back stack:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Back finishes the current Activity and returns to the previous one.

### Fragment back stack

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

If `addToBackStack` is not called, Back will not return the previous Fragment.

### Navigation Component

The back stack consists of destinations:

```kotlin
navController.navigate("details/$id")
navController.popBackStack()
```

`popBackStack()` removes the current destination.

### `popUpTo` and `inclusive`

`popUpTo` clears part of the stack:

```kotlin
navController.navigate("home") {
    popUpTo("login") { inclusive = true }
}
```

Useful after login/onboarding so the user cannot go back to login.

```text
inclusive = false -> popUpTo destination remains
inclusive = true  -> popUpTo destination is also removed
```

### Compose Navigation

The principle is the same:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen() }
    composable("details/{id}") { DetailsScreen() }
}
```

Back stack is managed by `NavController`.

### `launchSingleTop` and bottom navigation

To avoid creating duplicate destinations:

```kotlin
navController.navigate("home") {
    launchSingleTop = true
}
```

For bottom navigation, tab state is often saved:

```kotlin
navController.navigate(route) {
    popUpTo(navController.graph.startDestinationId) {
        saveState = true
    }
    launchSingleTop = true
    restoreState = true
}
```

Common mistakes:

- not clearing the stack after auth/onboarding;
- duplicating destinations;
- mixing manual Fragment transactions and NavController;
- confusing Activity task stack and NavController back stack;
- ignoring nested graphs or modal UI.

Practical rule: BackStack should match the user's expected history. Modal UI
closes first, irreversible flows clear the stack, and tabs preserve their state.

**In short:** BackStack is LIFO history of screens/destinations. Android has
Activity task stack, Fragment back stack, and NavController back stack. It is
managed through `navigate`, `popBackStack`, `popUpTo`, `inclusive`,
`launchSingleTop`, `saveState`, and `restoreState`.

</details>

<details>
<summary>173. When can you get an ANR (Application Not Responding)?</summary>

#### Kotlin

ANR happens when the app process does not respond to an important system event in
time. Most often, the reason is that the main thread is blocked, overloaded, or
waiting for another thread/process.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // blocks input and rendering
}
```

The main thread handles input, lifecycle, part of Binder callbacks, and
rendering. ANR can be caused by one long operation or a large queue of short ones.

### Common causes

- synchronous network/disk I/O or DB query on Main;
- heavy parsing, bitmap decoding, sorting, crypto;
- `runBlocking`, `Thread.sleep()`, `Future.get()`, `join()`;
- heavy lifecycle/View/Compose callbacks;
- long work in `BroadcastReceiver` or `Service`;
- locks/deadlocks/synchronous Binder calls;
- heavy startup in `Application`, `ContentProvider`, DI, or first Activity.

`suspend` does not automatically move blocking code from the main thread.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

I/O — `Dispatchers.IO`, CPU-heavy — `Dispatchers.Default`.

`Service` does not create a worker thread by itself. For long guaranteed work,
use `WorkManager`. For `BroadcastReceiver.goAsync()`, `finish()` must always be
called.

### Jank vs ANR

A slow frame is jank, not always ANR. But a long series of expensive
layout/draw/recomposition operations can block Main long enough.

### Diagnostics

Start with the main thread stack and the threads it is waiting for:

- Play Console Android vitals / ANR traces;
- Perfetto/System Trace;
- Android Studio CPU Profiler;
- Logcat and custom timing metrics;
- StrictMode in debug build.

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

StrictMode helps find main-thread I/O, but it does not replace traces.

### Prevention

- do not block Main;
- do not hold a lock during I/O;
- add timeouts to network/IPC;
- defer non-critical startup initialization;
- split long CPU work;
- control UI event frequency;
- profile startup, scrolling, and mass updates.

**In short:** ANR is caused by blocking I/O, CPU-heavy work, locks, synchronous
waits/IPC, long callbacks, or heavy startup on Main. Diagnostics start with the
main thread stack and system trace.

</details>

<details>
<summary>174. What can be executed on the main thread?</summary>

#### Kotlin

`Main thread` in Android is responsible for UI: input events, lifecycle
callbacks, rendering, View/Compose updates. Only fast operations that do not
block rendering and do not risk ANR should run on it.

Allowed:

- update UI;
- work with View hierarchy;
- render Compose UI;
- run lifecycle callbacks;
- handle click listeners;
- perform navigation calls;
- show dialog/snackbar/toast;
- run short lightweight logic.

```kotlin
button.setOnClickListener {
    viewModel.onLoginClick()
}
```

The View system is not thread-safe, so UI updates must happen on the main thread:

```kotlin
textView.text = "Loaded"
progressBar.isVisible = false
```

In Compose, a composable should be lightweight:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
ProfileScreen(state)
```

Do not block the main thread:

- network requests;
- file I/O;
- large DB queries;
- bitmap decoding;
- large JSON parsing;
- heavy sorting/filtering;
- `Thread.sleep`;
- `runBlocking`;
- long synchronized blocks.

The main thread should fit roughly into 16ms per frame for 60fps. If it is busy
with long work, there will be jank; with long blocking, there will be ANR.

Background work:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

Rule:

```text
UI/orchestration -> Main
blocking I/O     -> Dispatchers.IO
CPU-heavy work   -> Dispatchers.Default
```

Short validation on the main thread is fine:

```kotlin
if (email.isBlank()) {
    showEmailError()
    return
}
```

To find accidental I/O on the main thread in debug, enable StrictMode:

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

**In short:** main thread is for UI, lifecycle, navigation, and short
orchestration logic. Anything unpredictably long — network, disk, DB, parsing,
bitmap, CPU-heavy work — should be moved to `IO` or `Default`.

</details>

<details>
<summary>175. What are the basic Android components?</summary>

#### Kotlin

The basic Android system components are `Activity`, `Service`,
`BroadcastReceiver`, and `ContentProvider`. Fragment, ViewModel, and WorkManager
are important Jetpack abstractions, but they are not system components.

### Activity

UI entry point with its own window and lifecycle:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Activity receives an `Intent` and can host Compose UI or Fragment.

### Service

A component without its own UI:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Service callbacks run on the main thread by default. For ongoing user-visible
work, use a foreground service. For deferrable persistent work, WorkManager is
usually better.

### BroadcastReceiver

Receives system/app broadcasts:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // handle event
    }
}
```

`onReceive()` should finish quickly. Long work is delegated to WorkManager or
another appropriate component.

### ContentProvider

Provides structured data through URI:

```text
content://com.example.provider/users/1
```

Used for controlled inter-process data sharing. System examples are contacts and
media providers.

### Manifest and Intent

System components are declared in `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

The manifest also defines permissions, intent filters, and `android:exported`
behavior.

`Intent` starts Activity/Service or delivers a broadcast:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

### Jetpack abstractions

- Fragment — part of View-based UI inside Activity;
- ViewModel — owner of UI state that survives configuration changes;
- WorkManager — scheduler for persistent deferrable work.

Practical choice:

- UI entry — `Activity`;
- ongoing user-visible work — foreground `Service`;
- system/app event — `BroadcastReceiver`;
- inter-process data API — `ContentProvider`;
- deferrable persistent work — `WorkManager`.

**In short:** the basic Android system components are `Activity`, `Service`,
`BroadcastReceiver`, and `ContentProvider`. They are declared in the manifest and
interact through Intent/Binder/URI. Fragment, ViewModel, and WorkManager are a
Jetpack layer.

</details>

<details>
<summary>176. What is a Service?</summary>

#### Kotlin

`Service` is an Android component without UI. Important: Service does not create
a background thread by itself. Its callbacks run on the app process main thread
by default.

Service does not guarantee survival after process death, force stop, or reboot.

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

Every `startService()` calls `onStartCommand()`. Blocking work must be moved off
Main. `onDestroy()` is best-effort cleanup; it may not be called if the process
is killed.

### Bound Service

A bound service provides an API through `IBinder`:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService = this@PlayerService
    }

    override fun onBind(intent: Intent): IBinder = binder
}
```

Local Binder works in one process. For cross-process IPC, use Messenger/AIDL.

A bound-only service lives while there are clients. If the service is also
started, `unbindService()` does not stop the started lifecycle.

### Foreground Service

Needed for long user-visible work: media playback, navigation, active location,
file transfer.

```kotlin
startForeground(
    NOTIFICATION_ID,
    createNotification()
)
```

After `startForegroundService()`, `startForeground()` must be called quickly.
Modern Android restricts background starts and requires notification, service
type, and permissions.

### Lifecycle

```text
onCreate()       -> instance creation
onStartCommand() -> each start
onBind()         -> client connection
onDestroy()      -> cleanup
```

Restart flags:

- `START_NOT_STICKY` — do not recreate;
- `START_STICKY` — recreate without guaranteed Intent;
- `START_REDELIVER_INTENT` — redeliver Intent.

Restart policy does not make Service a durable queue; the operation should be
idempotent.

Practical choice:

```text
user-visible active work -> Foreground Service
reliable deferred work   -> WorkManager
exact-time trigger       -> AlarmManager
media playback           -> Media3 + FGS
screen-bound work        -> ViewModel scope
```

**In short:** Service is a lifecycle component without UI, not a separate thread.
Started service executes commands, bound service provides Binder API, foreground
service is needed for ongoing user-visible work. For reliable deferred work, use
WorkManager.

</details>

<details>
<summary>177. What is BroadcastReceiver?</summary>

#### Kotlin

`BroadcastReceiver` is an Android component for receiving broadcast `Intent`s
from the system, app components, or other apps.

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            scheduleSync(context)
        }
    }
}
```

A receiver does not store state and is not intended for long-running work. After
`onReceive()` finishes, the process can be stopped.

### Manifest receiver

```xml
<receiver
    android:name=".BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

For `BOOT_COMPLETED`, the `RECEIVE_BOOT_COMPLETED` permission is required.

A manifest receiver can work without UI, but modern Android restricts many
implicit broadcasts. Check the specific action/API.

`android:exported` is critical for security. If the receiver should not accept
intents from other apps, use `exported=false`. If it is exported, validate intent
data and permissions.

### Dynamic receiver

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

Register/unregister must be symmetrical. The lifecycle pair depends on the need:
visible screen — `onStart/onStop`, Activity lifetime — `onCreate/onDestroy`.

On modern APIs, dynamic receivers often require explicitly specifying
`RECEIVER_EXPORTED` or `RECEIVER_NOT_EXPORTED`.

### Long work

`onReceive()` usually runs on the main thread and should finish quickly. For
deferrable persistent work, use WorkManager:

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

`goAsync()` allows briefly continuing work after `onReceive()`, but it requires
`PendingResult.finish()` and does not replace WorkManager for long tasks.

### Internal communication

`LocalBroadcastManager` is deprecated. Inside a process, prefer Flow/StateFlow,
callbacks, repository state, or navigation result.

BroadcastReceiver is appropriate for system events, alarms, boot/package changes,
and clear inter-app/component communication.

**In short:** BroadcastReceiver receives broadcast Intent and must finish
`onReceive()` quickly. Manifest receiver requires attention to
`exported`/permissions, dynamic receiver is tied to lifecycle, and long work is
delegated to WorkManager.

</details>

<details>
<summary>178. What is Context, and what types of Context exist?</summary>

#### Kotlin

`Context` is the access point to the Android environment: resources, system
services, package info, files, preferences, and starting components. A concrete
context also defines theme and lifecycle boundary.

```kotlin
context.getString(R.string.app_name)
context.getSystemService(Context.CONNECTIVITY_SERVICE)
context.startActivity(intent)
context.getSharedPreferences("settings", Context.MODE_PRIVATE)
```

### Application Context

```kotlin
val appContext = context.applicationContext
```

Lives together with the process and does not have a specific Activity theme.
Suitable for long-lived dependencies: Room, DataStore, repositories, SDKs that
do not need UI context.

### Activity Context

Has the theme, window, and lifecycle of a specific Activity. Needed for dialogs,
themed views, and UI operations. Must not be stored in a singleton or long-lived
object.

### Fragment Context

`Fragment` itself is not a `Context`. It gets context from its host:

```kotlin
val context = requireContext()
val activityContext = requireActivity()
```

Before attach, context can be `null`; after detach, `requireContext()` throws an
exception.

### Service Context

`Service` is a `Context` tied to the service lifecycle. Suitable for service
operations, but it does not have Activity window/theme semantics.

### ContextWrapper

`ContextWrapper` delegates to a base context and can change behavior:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

`ContextThemeWrapper` is useful for themed UI inflation.

### Compose

```kotlin
val context = LocalContext.current
```

This is the context of the current composition environment. It can be used for
resources/UI actions, but should not be stored without lifecycle ownership.

### Memory leaks

Bad:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

The singleton will retain the Activity after recreation.

Better for a long-lived dependency:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

Pass `context.applicationContext`.

Practical choice:

```text
Application context -> DB, DataStore, long-lived dependencies
Activity context    -> UI, dialogs, themed views
Fragment context    -> host context while attached
Service context     -> service operations
LocalContext        -> Compose resources/UI actions
```

**In short:** `Context` gives access to the Android environment. Long-lived
dependencies use application context; UI uses Activity/themed context; Fragment
is not a Context; Activity context must not be held in a singleton.

</details>

<details>
<summary>179. What is Bundle?</summary>

#### Kotlin

`Bundle` is an Android key-value container for passing small data between
components or saving simple UI state.

```kotlin
val bundle = Bundle().apply {
    putString("user_id", userId)
    putInt("tab", 2)
}

val userId = bundle.getString("user_id")
```

It is used with `Intent`, Fragment arguments, `onSaveInstanceState`, Navigation,
and Activity Result API.

### Intent extras

```kotlin
val intent = Intent(this, DetailsActivity::class.java).apply {
    putExtra("user_id", userId)
}
startActivity(intent)
```

```kotlin
val userId = intent.getStringExtra("user_id")
```

### Fragment arguments

```kotlin
val fragment = DetailsFragment().apply {
    arguments = bundleOf("user_id" to userId)
}
```

```kotlin
val userId = requireArguments().getString("user_id")
```

### Saved instance state

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

Suitable for small UI state after configuration change or process recreation.

### Types

Bundle supports:

- primitives;
- `String`;
- arrays;
- `Parcelable`;
- `Serializable`;
- `Bundle`;
- `ArrayList` of some types.

For Android custom data, `Parcelable` is better:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

But for navigation, it is often better to pass only `id` and load data from a
repository.

### Limitations

Bundle is not for large payloads. You can get `TransactionTooLargeException`.

Bad:

```kotlin
bundle.putParcelableArrayList("huge_list", hugeList)
```

Better to pass an id/key to DB/cache.

### Bundle vs other state holders

```text
Bundle             -> small navigation/UI state
SavedStateHandle   -> state in ViewModel after recreation
ViewModel          -> screen/business state during lifecycle
Database/DataStore -> persistent data
```

Bundle keys are strings, so typos are easy. For Navigation, Safe Args or typed
route arguments are better when available.

**In short:** Bundle is a key-value container for small data: ids, flags, simple
UI/navigation state. Do not put large lists, bitmap, or payloads into it; better
pass an id and restore data from repository/cache.

</details>

<details>
<summary>180. Can you put a Bundle inside another Bundle?</summary>

#### Kotlin

Yes, a `Bundle` can be placed inside another `Bundle` with `putBundle()` and read
with `getBundle()`.

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

Reading:

```kotlin
val address = userBundle.getBundle("address")
val city = address?.getString("city")
```

This is useful for small logically grouped data:

```text
user
 ├─ id
 ├─ name
 └─ address
     ├─ city
     └─ street
```

Example in Fragment arguments:

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

For saved state:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putBundle("filters", bundleOf("query" to query))
    super.onSaveInstanceState(outState)
}
```

Limitations:

- Bundle is not for large payloads;
- deep nesting reduces readability;
- string keys are easy to mistype;
- `TransactionTooLargeException` is possible.

Bad:

```kotlin
bundle.putBundle("huge_data", hugeNestedBundle)
```

If the structure has a clear model, `Parcelable` is better:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

For large data, it is better to pass an `id` and load data from
repository/cache/database.

**In short:** you can put a `Bundle` inside another `Bundle`, but only for small
simple grouped arguments or saved state. For complex models, use `Parcelable`;
for large data, use `id + repository`.

</details>

<details>
<summary>181. What are Permissions in Android?</summary>

#### Kotlin

Permissions are an Android mechanism that restricts app access to resources
outside the sandbox: camera, microphone, location, contacts, notifications,
storage/media.

Main types:

```text
normal    -> granted automatically
dangerous -> manifest + runtime request
special   -> separate Settings flow
signature -> only apps with a matching certificate
```

A dangerous permission must be declared in the manifest and requested at runtime:

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

Before a protected API, always check the current grant:

```kotlin
val granted = ContextCompat.checkSelfPermission(
    context,
    Manifest.permission.CAMERA
) == PackageManager.PERMISSION_GRANTED
```

Grant must not be cached as a permanent boolean: the user can revoke permission
in Settings, the system can auto-reset it, and camera/microphone/location can be
one-time.

`shouldShowRequestPermissionRationale()` helps decide whether to show an
explanation before a repeated request. `false` does not always mean permanent
denial, so the app should consider its own request history.

Practical rules:

- request permission at the moment of action, not on app startup;
- ask for minimal access;
- support denial, revoke, and one-time permissions;
- for files/media, use Photo Picker, SAF, MediaStore, or app-specific storage;
- for special permissions, send the user to Settings and check capability again
  after returning.

**In short:** Permissions protect sensitive resources. Dangerous permissions are
declared in the manifest and requested at runtime through Activity Result API.
Grant can change at any moment, so it must be checked before use.

</details>

<details>
<summary>182. What is setContentView?</summary>

#### Kotlin

`setContentView()` is an Activity API that sets the root UI for a screen. In the
View System, it inflates an XML layout or accepts a ready `View` and adds it to
the Activity content area.

Basic example:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Simplified under the hood:

```text
XML -> inflate -> View hierarchy -> Activity content root
```

With ViewBinding, it is more common to write:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

After `setContentView()`, you can find Views through `findViewById` because the
layout is already attached:

```kotlin
val title = findViewById<TextView>(R.id.title)
```

In Compose, `setContent { ... }` is used instead of `setContentView()`:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { MainScreen() }
    }
}
```

In Fragment, `setContentView()` is not called: the layout is created through
`Fragment(R.layout...)`, `onCreateView()`, or `ComposeView`.

Common mistakes:

- calling `findViewById` before `setContentView`;
- calling `setContentView` multiple times without a reason;
- keeping Activity View references in a singleton;
- mixing XML and Compose without a clear responsibility boundary.

**In short:** `setContentView()` sets the root UI for Activity. For XML, prefer
ViewBinding + `setContentView(binding.root)`; for Compose, use `setContent { ... }`;
for Fragment, use its own view creation lifecycle.

</details>

<details>
<summary>183. What methods exist in View?</summary>

#### Kotlin

`View` is the base class of Android UI. The key pipeline is:
`measure -> layout -> draw`.

Main callbacks:

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

`onMeasure()` determines the View size from `MeasureSpec`: `EXACTLY`, `AT_MOST`,
`UNSPECIFIED`. `onLayout()` places children and is usually important for
`ViewGroup`. `onDraw()` draws content on `Canvas`.

For UI updates:

```text
only appearance changed -> invalidate()
size/layout changed     -> requestLayout()
```

Input is handled through `onTouchEvent()`:

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

`performClick()` is important for click listeners and accessibility.

View lifecycle:

- `onAttachedToWindow()` — View is attached to a window;
- `onDetachedFromWindow()` — stop animations, listeners, callbacks;
- `onSaveInstanceState()` / `onRestoreInstanceState()` — save own state.

Useful methods and states:

- `VISIBLE`, `INVISIBLE`, `GONE`;
- `requestFocus()` / `clearFocus()`;
- `post { }` — run code in the UI queue;
- `setOnClickListener()` / `setOnLongClickListener()`.

Do not do I/O, bitmap decoding, or unnecessary allocations in `onDraw()`, because
the method can be called often.

**In short:** the main `View` methods are `onMeasure()`, `onLayout()`,
`onDraw()`, `invalidate()`, `requestLayout()`, `onTouchEvent()`,
`performClick()`, and lifecycle callbacks. They handle size, position, drawing,
input, and resources.

</details>

<details>
<summary>184. What is ViewGroup?</summary>

#### Kotlin

`ViewGroup` is a `View` that contains child views and defines how to measure,
lay out, draw, and pass touch events to them.

```text
View      -> one UI node
ViewGroup -> container + children + layout policy
```

Examples: `LinearLayout`, `FrameLayout`, `ConstraintLayout`, `RecyclerView`.

Main pipeline:

```text
onMeasure() -> onLayout() -> dispatchDraw()
```

`onMeasure()` measures children and determines its own size:

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

`onLayout()` sets the final bounds for children:

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

`LayoutParams` describe rules for a child: `weight` in `LinearLayout`,
constraints in `ConstraintLayout`, `gravity` in `FrameLayout`. A custom
`ViewGroup` must correctly support params, margins, `GONE`, padding, and
`MeasureSpec`.

Touch flow:

```text
dispatchTouchEvent()
 -> onInterceptTouchEvent()
 -> child.dispatchTouchEvent()
 -> onTouchEvent()
```

`onInterceptTouchEvent()` allows the container to take a gesture from a child,
for example for scrolling. Incorrect interception breaks clicks and nested
scrolling.

For updates:

```text
invalidate()    -> redraw
requestLayout() -> repeat measure/layout
```

Custom `ViewGroup`s are written only when ready-made containers or Compose do
not cover the layout, touch, or performance requirement.

**In short:** `ViewGroup` is a container for `View`s that manages measuring,
positioning, drawing, and touch routing for children. The key methods are
`onMeasure()`, `onLayout()`, `dispatchDraw()`, and `onInterceptTouchEvent()`.

</details>

<details>
<summary>185. What is the onStart() method in Activity used for?</summary>

#### Kotlin

`onStart()` is called when an `Activity` becomes visible to the user. The UI has
already been created, but the Activity is not necessarily in the foreground yet
and does not necessarily accept input.

Lifecycle:

```text
first launch: onCreate -> onStart -> onResume
background:   onPause -> onStop
return:       onRestart -> onStart -> onResume
```

Difference from `onResume()`:

```text
onStart()  -> Activity visible
onResume() -> Activity interactive / foreground
```

In `onStart()`, it is appropriate to start visible-only resources:

- registering lightweight listeners/receivers;
- visibility analytics;
- subscriptions to UI state;
- resources needed only while the screen is visible.

Everything started in `onStart()` should be symmetrically stopped in `onStop()`:

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

For Flow, it is better to use lifecycle-aware APIs:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`repeatOnLifecycle(STARTED)` starts collection when the Activity is visible and
cancels it after moving below `STARTED`.

What not to do in `onStart()`:

- heavy blocking I/O;
- long parsing or `Thread.sleep()`;
- one-time initialization that belongs in `onCreate()`;
- background work that must outlive the Activity.

If refresh is started in `onStart()`, the ViewModel or repository should control
freshness and deduplication, because `onStart()` is called every time the user
returns to the screen.

**In short:** `onStart()` means the Activity has become visible. This is where
lightweight visible-only resources and subscriptions are started, and cleanup is
done in `onStop()`. For Flow, use `repeatOnLifecycle(STARTED)`.

</details>

<details>
<summary>186. How can onStart() be reached without reaching onResume()?</summary>

#### Kotlin

This is possible if the `Activity` becomes visible but does not receive
foreground focus, or if the lifecycle is interrupted between `STARTED` and
`RESUMED`.

State difference:

```text
onStart()  -> Activity visible
onResume() -> Activity foreground + interactive
```

Typical scenarios:

1. **Another Activity or overlay opens on top**

A transparent or dialog-themed Activity can leave the previous screen visible
but take focus.

```text
MainActivity.onStart()
OverlayActivity opens on top
MainActivity does not move to onResume()
```

2. **Redirect in `onStart()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

Focus moves to another Activity, so the current one may not reach `onResume()`.

3. **`finish()` before `onResume()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

4. **System interruption**

Between `onStart()` and `onResume()`, the user or system may open another screen
or app: an incoming call, system UI, multi-window, or a quick background
transition.

The practical conclusion: lifecycle code must not assume that every `onStart()`
will necessarily be followed by `onResume()`.

Resources should be tied to the correct callback pair:

```text
visible-only     -> onStart() / onStop()
interactive-only -> onResume() / onPause()
```

For Flow:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) { /* visible UI */ }
repeatOnLifecycle(Lifecycle.State.RESUMED) { /* active interaction */ }
```

**In short:** `onStart()` without `onResume()` happens when the Activity is
visible but has not become interactive: overlay, redirect, `finish()`, or system
interruption. Cleanup should be symmetric to the lifecycle state where the
resource starts.

</details>

<details>
<summary>187. What are migrations in Room?</summary>

#### Kotlin

A Room migration is a description of moving the database schema from one version
to another without losing user data.

A migration is needed when the schema changes:

- a table or column is added or removed;
- a type, constraint, or relation is changed;
- a table or field is renamed;
- an index or foreign key is added.

After changing the entity schema, the `version` in `@Database` must be
increased:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase()
```

Manual migration:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Registration:

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

If a `NOT NULL` column is added for existing rows, a default value is required:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

For complex changes that SQLite does not allow directly, a copy-table migration
is used:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

`AutoMigration` is suitable for simple unambiguous changes:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

For rename/delete operations, an `AutoMigrationSpec` is needed; for
transformations, splitting/merging tables, or data backfill, a manual migration
is needed.

`fallbackToDestructiveMigration()` deletes the old DB and creates a new one.
This is acceptable for a cache/dev DB, but not for user data.

Practical rules:

- increase `version` for every schema change;
- support a migration path from all production versions;
- export the schema with `exportSchema = true`;
- test migrations with `MigrationTestHelper`;
- do not use destructive fallback for important data.

**In short:** a Room migration moves the DB schema to a new version while
preserving data. Simple changes may be covered by `AutoMigration`, complex ones
by manual `Migration`, and production paths must be tested.

</details>

<details>
<summary>188. What does the Embedded annotation do in Room?</summary>

#### Kotlin

`@Embedded` in Room expands the fields of a nested class into columns of the
same table or query result. The embedded object does not become a separate
table.

Example:

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

Room will create the columns:

```text
id, name, city, street
```

`@Embedded` is suitable for value objects that are logically part of an entity:

- address;
- coordinates;
- money amount + currency;
- metadata;
- reusable group of columns.

If column names conflict, `prefix` is used:

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

Columns:

```text
start_lat, start_lng, end_lat, end_lng
```

`@Embedded` is also convenient in read DTOs for `JOIN` results:

```kotlin
data class UserWithStats(
    @Embedded val user: UserEntity,
    @Embedded(prefix = "stats_") val stats: UserStatsEntity
)
```

Difference from `@Relation`:

```text
@Embedded -> fields are in the same row/result
@Relation -> relationship between different tables
```

If an object has its own lifecycle, can be used by many rows, or requires a
foreign key, it is not `@Embedded`; it should be a separate table + relation.

Limitations:

- does not create a foreign key;
- does not normalize data;
- does not create a relation automatically;
- can bloat the table with many columns;
- requires `prefix` if column names overlap.

**In short:** `@Embedded` embeds value object fields into the current table or
query result. Relationships between tables use `@Relation`, foreign keys, and
separate entities.

</details>

<details>
<summary>189. What is DatabaseView in Room?</summary>

#### Kotlin

`@DatabaseView` in Room describes an SQLite `VIEW` as a Kotlin class. A view does
not store data separately; it represents the result of an SQL query as a
read-only model.

An SQL view is a named query:

```sql
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE active = 1;
```

In Room:

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

The view must be explicitly added to `@Database`:

```kotlin
@Database(
    entities = [UserEntity::class],
    views = [ActiveUserView::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase()
```

Reading through a DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM ActiveUserView")
    fun observeActiveUsers(): Flow<List<ActiveUserView>>
}
```

`DatabaseView` is useful for:

- complex read queries;
- UI projections;
- `JOIN` results;
- aggregates;
- reusing the same SQL in several DAO methods.

Aggregation example:

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

Difference:

```text
Entity       -> real table, stores data
DatabaseView -> read-only projection, SQL query result
@Relation    -> object graph from related tables
```

Limitations:

- cannot directly `insert/update/delete` into a view;
- SQL must be maintained when the schema changes;
- complex views can be expensive for performance;
- changes in underlying tables may require a migration.

**In short:** `@DatabaseView` is a read-only model on top of an SQL view. It is
convenient for complex read models, joins, and aggregates, but it does not
replace tables and does not support direct write operations.

</details>

<details>
<summary>190. What is the difference between SQL and NoSQL?</summary>

#### Kotlin

`SQL` and `NoSQL` are different approaches to modeling and storing data.

```text
SQL   -> tables, schema, relations, SQL queries
NoSQL -> document/key-value/wide-column/graph storage
```

SQL example:

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
);
```

NoSQL document example:

```json
{
  "id": "1",
  "name": "Alex",
  "settings": {
    "theme": "dark"
  }
}
```

Key differences:

```text
Schema:
SQL   -> stricter schema + migrations
NoSQL -> more flexible schema, more control in the app/backend

Relations:
SQL   -> JOIN, foreign keys, normalization
NoSQL -> more often denormalization for specific reads

Transactions:
SQL   -> usually strong ACID guarantees
NoSQL -> depends on the specific DB and configuration

Scaling:
SQL   -> often vertical scaling, replicas, sharding
NoSQL -> often simpler horizontal scaling
```

Android SQL example — Room on top of SQLite:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

Android NoSQL example — Firebase Firestore:

```kotlin
firestore.collection("users").document(id).get()
```

SQL is a better choice when:

- the data structure is stable;
- there are relations and complex queries;
- constraints are needed;
- transactions and consistency are critical.

NoSQL is a better fit when:

- data is naturally document/key-value;
- the schema changes often;
- relations are minimal;
- denormalization is needed for fast reads;
- there is a distributed/high-scale workload.

The choice is made not by trend, but by access patterns, consistency
requirements, relations, scaling, and the team's experience.

**In short:** SQL is a relational model with tables, schema, joins, and strong
transactions. NoSQL is non-relational storage with a more flexible data model,
often with denormalization and simpler horizontal scaling.

</details>

<details>
<summary>191. What is DataStore?</summary>

#### Kotlin

`DataStore` is Jetpack storage for asynchronously saving small persistent
settings. It works with Coroutines and `Flow` and is a more modern alternative
to `SharedPreferences`.

Typical data:

- theme;
- language;
- onboarding state;
- user preferences;
- feature flags.

For large collections, relations, queries, or offline cache, use `Room`, not
DataStore.

There are two variants:

```text
Preferences DataStore -> key-value without schema
Proto DataStore       -> protobuf schema + type-safe model
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

Proto DataStore uses a `.proto` schema:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Difference from `SharedPreferences`:

```text
SharedPreferences -> synchronous key-value API
DataStore         -> suspend API, Flow, transactional updates
```

In a ViewModel, `Flow` is usually converted to `StateFlow`:

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

Writing through `edit` is a transactional read-modify-write:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

Read errors are handled explicitly:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
    }
```

For migration from `SharedPreferences`, use `SharedPreferencesMigration`.

**In short:** `DataStore` stores small settings through Coroutines and `Flow`.
`Preferences DataStore` is suitable for key-value data, `Proto DataStore` for
type-safe settings, and complex relational data should be stored in `Room`.

</details>

<details>
<summary>192. What is Paging 3?</summary>

#### Kotlin

`Paging 3` is a Jetpack library for loading large lists in chunks. It manages
pagination, prefetch, refresh, retry, cancellation, and UI integration.

Main pipeline:

```text
PagingSource -> Pager -> Flow<PagingData<T>> -> UI
```

`PagingSource<Key, Value>` describes the data source and page-loading rules:

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

`Pager` creates a `PagingData` stream:

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

In the ViewModel, `cachedIn` is added so the paging stream lives in
`viewModelScope` and does not start from scratch for every collector:

```kotlin
val users = repository.users()
    .cachedIn(viewModelScope)
```

Compose UI:

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

`LoadState` provides separate states for `refresh`, `append`, and `prepend`.
Usually:

- `refresh` is full-screen loading/error;
- `append` is footer loading/error;
- `retry()` repeats a failed load;
- `refresh()` creates a new generation.

For offline-first, `RemoteMediator` is used:

```text
UI <- PagingSource from Room
          ^
RemoteMediator: API -> Room + remote keys
```

In this scheme, Room is the source of truth, and `RemoteMediator` loads API
pages and transactionally writes them to the DB.

Common mistakes:

- incorrect pagination keys;
- missing `cachedIn`;
- ignoring append/prepend errors;
- unstable item keys in the UI;
- manual pagination in parallel with Paging 3.

**In short:** `Paging 3` loads large lists page by page. The basic chain is
`PagingSource -> Pager -> PagingData -> UI`; `cachedIn` caches the stream in the
ViewModel, and `RemoteMediator` is needed for API + Room offline-first.

</details>

<details>
<summary>193. What is RecyclerView.Adapter?</summary>

#### Kotlin

`RecyclerView.Adapter` is the bridge between a data set and `RecyclerView`. It
creates `ViewHolder`s, binds data to item UI, and reports the number of items.

Main methods:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

Example with `ListAdapter`:

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

`RecyclerView` reuses item views, so `bind()` must set the entire UI state, not
only changed fields. Otherwise, a recycled view may show the state of a previous
item.

`DiffUtil` determines what exactly changed:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

```text
areItemsTheSame()    -> identity
areContentsTheSame() -> content
```

`ListAdapter` + `DiffUtil` is better than a mutable list and
`notifyDataSetChanged()`, because it updates only changed items.

For different layouts, use `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int = when (getItem(position)) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Common mistakes:

- business logic or navigation inside the adapter;
- unnecessary `notifyDataSetChanged()`;
- incomplete state in `bind()`;
- reference to an `Activity` or `Fragment`;
- storing an old `position` instead of working with the bound item.

**In short:** `RecyclerView.Adapter` turns data into recycled item views. In
production, a complete `bind()`, `ListAdapter` with `DiffUtil`, stable passing
of click events outward, and no business logic in the adapter are important.

</details>

<details>
<summary>194. What are the main methods of RecyclerView.Adapter?</summary>

#### Kotlin

The main methods of `RecyclerView.Adapter` are:

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

`onCreateViewHolder()` creates a new `ViewHolder` when there is no reusable
holder:

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

`onBindViewHolder()` binds an item to a holder:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}
```

Because of recycling, `bind()` must set the entire UI state: `true/false`,
`null`, empty values, visibility, and selected state.

`getItemCount()` returns the dataset size:

```kotlin
override fun getItemCount(): Int = items.size
```

For different layouts, use `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

A partial update can be done through the payload overload:

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

Additional lifecycle callbacks:

- `onViewRecycled()` — cleanup before reusing the holder;
- `onViewAttachedToWindow()` — item attached to the window;
- `onViewDetachedFromWindow()` — item detached from the window.

Dataset updates should be precise:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

In production, `ListAdapter` or `AsyncListDiffer` with `DiffUtil` is used more
often than a manual mutable list + `notifyDataSetChanged()`.

Common mistakes:

- incomplete state in `bind()`;
- heavy work in `onBindViewHolder()`;
- storing an old `position`;
- mutating the dataset without a corresponding notify/diff;
- non-unique stable IDs.

**In short:** the main adapter methods are `onCreateViewHolder()`,
`onBindViewHolder()`, and `getItemCount()`. Additionally, `getItemViewType()`,
payload binding, recycling callbacks, and correct dataset updates are important.

</details>

<details>
<summary>195. What is the difference between RecyclerView.Adapter and ListAdapter?</summary>

#### Kotlin

`ListAdapter` is a `RecyclerView.Adapter` that already includes
`AsyncListDiffer` and `DiffUtil`. A regular adapter requires the developer to
manage list storage and `notify...` calls manually.

```text
RecyclerView.Adapter -> manual list + manual notify
ListAdapter          -> submitList() + async DiffUtil
```

Regular `RecyclerView.Adapter`:

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

The problem: dataset mutation and `notify...` calls must match exactly.
`notifyDataSetChanged()` is simple, but it kills granular updates, animations,
and some optimizations.

`ListAdapter`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Update:

```kotlin
adapter.submitList(users)
```

`ListAdapter` calculates the diff off the main thread and applies precise
`insert/remove/move/change` updates on the main thread.

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
areItemsTheSame()    -> item identity
areContentsTheSame() -> rendered content
```

Important: pass an immutable snapshot to `submitList()`. Do not mutate the same
list or its items after submit, because the diff may not detect the change.

```kotlin
adapter.submitList(oldList + newUser)
```

When to choose what:

- `ListAdapter` is the default for ordinary dynamic lists;
- `RecyclerView.Adapter` is used when updates already arrive as precise
  operations or when custom storage/differ architecture is needed;
- `PagingDataAdapter` is used for Paging 3.

Common mistakes:

- incorrect `areItemsTheSame()`;
- mutable list after `submitList()`;
- `notifyDataSetChanged()` instead of diff;
- business logic in the adapter;
- stable IDs that do not match DiffUtil identity.

**In short:** `RecyclerView.Adapter` gives full manual control over the list and
notifications. `ListAdapter` accepts immutable snapshots through `submitList()`
and applies async diff itself, so it is the better default for most lists.

</details>

<details>
<summary>196. How does RecyclerView work under the hood?</summary>

#### Kotlin

`RecyclerView` displays large lists without creating a View for every item. It
keeps visible views, a small cache, and a pool of reusable `ViewHolder`s.

```text
10 000 data items
~10-20 visible views
cache + recycled pool
```

Main components:

- `Adapter` — creates `ViewHolder`s and binds data;
- `ViewHolder` — holds the item view and references to its views;
- `LayoutManager` — measures and positions items;
- `Recycler` / `RecycledViewPool` — reuses holders;
- `ItemAnimator` — animates changes;
- `ItemDecoration` — adds dividers, spacing, or custom drawing.

Simplified flow:

```text
LayoutManager requests a view
 -> Recycler looks for a holder in cache/pool
 -> Adapter creates a holder if there is nothing to reuse
 -> Adapter binds the item
 -> LayoutManager measures/lays out the view
```

During scrolling, an item view that leaves the screen is detached and can be
used for another item of the same `viewType`.

Because of recycling, `bind()` must set the entire UI state:

```kotlin
fun bind(item: Item) {
    binding.title.text = item.title
    binding.badge.isVisible = item.isImportant
    binding.checkbox.isChecked = item.isSelected
}
```

If `false`, `null`, empty values, or visibility state are not set, a recycled
view may show the state of a previous item.

`LayoutManager` defines the layout strategy:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
// GridLayoutManager(context, 2)
// StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

`getItemViewType()` separates different layouts. The pool groups holders by
`viewType`, so a header will not be reused as a post.

Simplified reuse levels:

```text
attached scrap -> views from the current layout pass
cached views    -> recently detached holders
recycled pool   -> holders by viewType for rebinding
```

To update the list, RecyclerView needs precise notifications or diff:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
```

In production, `ListAdapter` / `AsyncListDiffer` with `DiffUtil` is usually
used. `notifyDataSetChanged()` does not provide an exact diff and worsens
animations/reuse.

Practical rules:

- `onBindViewHolder()` should be cheap;
- do not do file/network I/O or bitmap decoding in bind;
- click callbacks should work with the bound item, not an old `position`;
- after insert/remove, position may change;
- images should be loaded by an image loader with cache and cancellation.

**In short:** `RecyclerView` is efficient thanks to `ViewHolder` recycling,
`LayoutManager`, cache/pool, and targeted updates through `DiffUtil`. The most
important part is a complete and cheap `bind()`.

</details>

<details>
<summary>197. What is DiffUtil?</summary>

#### Kotlin

`DiffUtil` is a utility for `RecyclerView` that compares an old and a new list
and finds exact changes: `insert`, `remove`, `move`, `change`.

```text
notifyDataSetChanged() -> update everything
DiffUtil               -> update only what changed
```

For `ListAdapter`, `DiffUtil.ItemCallback` is usually written:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

Meaning of the methods:

```text
areItemsTheSame()    -> is this the same item? usually a stable id
areContentsTheSame() -> are the data rendered by the UI the same?
```

`ListAdapter` uses `AsyncListDiffer` internally:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Updating the list:

```kotlin
adapter.submitList(users)
```

For partial updates, use a payload:

```kotlin
override fun getChangePayload(oldItem: UserUi, newItem: UserUi): Any? {
    return if (oldItem.isOnline != newItem.isOnline) OnlineStatusChanged
    else null
}
```

A payload allows part of a row to be updated without a full rebind.

Immutability is important: pass a new snapshot to `submitList()`, rather than
mutating the old list.

```kotlin
adapter.submitList(oldList + newUser)
```

Common mistakes:

- `areItemsTheSame()` compares the whole object instead of an id;
- `areContentsTheSame()` always returns `true`;
- unstable id;
- mutating the list/items after `submitList()`;
- `notifyDataSetChanged()` instead of diff;
- overly heavy `equals()` for large nested models.

**In short:** `DiffUtil` calculates the difference between an old and a new list
so RecyclerView updates only changed items. `areItemsTheSame()` is responsible
for identity, `areContentsTheSame()` for content, and `submitList()` should
receive an immutable snapshot.

</details>

<details>
<summary>198. How do you display lists in Android?</summary>

#### Kotlin

In View/XML, lists are usually displayed with `RecyclerView`; in Jetpack
Compose, with lazy containers: `LazyColumn`, `LazyRow`, `LazyVerticalGrid`. For
large or remote lists, Paging 3 is added.

### View System: RecyclerView

```kotlin
val adapter = UserAdapter(onClick = viewModel::onUserClicked)

binding.recyclerView.apply {
    layoutManager = LinearLayoutManager(requireContext())
    this.adapter = adapter
}

adapter.submitList(users)
```

For production, it is better to use `ListAdapter` +
`DiffUtil.ItemCallback`, rather than a mutable list + `notifyDataSetChanged()`.

`LayoutManager` defines the list appearance:

- `LinearLayoutManager` — vertical or horizontal list;
- `GridLayoutManager` — grid;
- `StaggeredGridLayoutManager` — grid with items of different heights.

### Paging 3

For paged loading, use `PagingDataAdapter`:

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

`loadState` is needed for `loading`, `error`, `empty`, and `retry`. It is
important to handle `refresh` and `append` separately so footer loading does not
replace the whole screen.

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

A stable `key` preserves item identity during insert/remove/reorder. The key
must be unique and stable, not a position.

Other lazy containers:

```kotlin
LazyRow {
    items(categories, key = Category::id) { CategoryChip(it) }
}

LazyVerticalGrid(columns = GridCells.Fixed(2)) {
    items(products, key = Product::id) { ProductCard(it) }
}
```

Paging in Compose is connected through `collectAsLazyPagingItems()`, with
`loadState` and `retry()` also handled.

Practical rules:

- do not manually add many views to a `LinearLayout`;
- do not use a regular `Column` for a large dynamic list;
- pass immutable snapshots;
- keep list state in the ViewModel;
- show loading/error/empty/retry states;
- do not do heavy work in `bind()` or composition.

**In short:** in View UI, use `RecyclerView` with `ListAdapter`; in Compose, use
`LazyColumn/LazyRow/LazyVerticalGrid`. For large remote lists, add Paging 3,
stable keys/ids, and correct load-state handling.

</details>

<details>
<summary>199. What are lazy containers in Compose?</summary>

#### Kotlin

Lazy containers in Compose are components for lists and grids that compose only
the items needed for the viewport, rather than the entire collection at once.

Main containers:

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

`LazyColumn` example:

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

For stable identity, pass a `key`:

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

`key` helps preserve item state after insert, delete, or reorder. The key must
be stable and unique, not a position.

For different item types, set `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` helps Compose reuse composition more efficiently for compatible
item types.

Scroll is controlled by `LazyListState`:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

Programmatic scroll:

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

Grid example:

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

Paging integrates through `collectAsLazyPagingItems()`:

```kotlin
val users = pager.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user -> UserRow(user) }
    }
}
```

`loadState` is handled separately: initial loading, append loading, error,
empty, and retry.

Performance rules:

- use a stable `key`;
- set `contentType` for different item types;
- do not do I/O or heavy calculations in an item;
- do not decode bitmaps in composition;
- avoid unnecessary nested scroll containers;
- pass immutable/stable UI models.

**In short:** lazy containers (`LazyColumn`, `LazyRow`, grids) render large
lists and grids by viewport. For production, stable keys, `contentType`,
lightweight item UI, `LazyListState`, and correct Paging load states are
important.

</details>

<details>
<summary>200. How do you implement a circular list in RecyclerView?</summary>

#### Kotlin

A circular `RecyclerView` is usually simulated with a large `itemCount`: the fake
adapter position is mapped to the real item through modulo.

Basic adapter:

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

To allow scrolling in both directions from the start, set the position close to
the middle of the range:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

It is important to check `items.isEmpty()`, otherwise modulo causes division by
zero. Initial scroll should also be done only for a non-empty list.

For carousel behavior, add snapping:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

Auto-scroll should be lifecycle-aware:

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

Stable IDs are risky. You cannot return one real ID for all repeats, because
adapter positions are different. If stable IDs are not needed, it is better not
to enable them for a fake-infinite adapter.

When updating the dataset, keep an immutable real list and, after the size
changes, align the current fake position to the new modulo.

Typical problems:

- modulo for an empty list;
- auto-scroll after `onDestroyView()`;
- non-unique stable IDs;
- incorrect restore position after dataset changes;
- complex accessibility semantics;
- poor UX from infinite scroll where it is not needed.

Alternatives:

- `ViewPager2` for a pager/banner;
- Compose `HorizontalPager`;
- finite list without fake infinity;
- duplicated edge items for a seamless transition.

**In short:** a circular `RecyclerView` is implemented with a large `itemCount`,
`position % items.size`, and a start from the middle of the range. Empty list,
lifecycle auto-scroll, stable IDs, and dataset updates must be handled.

</details>

<details>
<summary>201. What are animations in Android?</summary>

#### Kotlin

Animation in Android is a change of a UI property over time: `alpha`,
`translation`, `scale`, `rotation`, color, size, or layout state. A good
animation explains a state change, rather than just adding decoration.

### View System

For simple transform animations:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

`ObjectAnimator` animates a specific property:

```kotlin
ObjectAnimator
    .ofFloat(view, View.ALPHA, 1f, 0f)
    .apply {
        duration = 300
        start()
    }
```

`ValueAnimator` generates values for custom logic:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    addUpdateListener { animation ->
        progressView.progress = animation.animatedValue as Int
    }
    start()
}
```

`AnimatorSet` combines multiple animators, and `MotionLayout` is suitable for
complex transitions between layout states.

### Compose

In Compose, animations should be state-driven:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "contentAlpha"
)

Box(Modifier.alpha(alpha))
```

Main APIs:

- `animate*AsState` — one property;
- `AnimatedVisibility` — enter/exit;
- `AnimatedContent` — content change;
- `updateTransition` — several properties of one state;
- `Animatable` — imperative suspend control;
- `rememberInfiniteTransition` — infinite decorative animations.

### Performance

Usually, it is cheaper to animate:

```text
alpha, translation, scale, rotation
```

Animating `width`, `height`, `margin`, or layout params can trigger
measure/layout on every frame. This is not forbidden, but it requires profiling.

Callbacks must not do I/O, bitmap decoding, heavy calculations, or many
allocations.

### Lifecycle

View animations should be canceled and reset, especially in `RecyclerView`:

```kotlin
override fun onViewRecycled(holder: ItemHolder) {
    holder.itemView.animate().cancel()
    holder.reset()
}
```

Otherwise, a recycled holder may show `alpha`, `translation`, or selected state
from a previous item.

Compose effects/coroutines are canceled together with the composition scope, but
external resources still require cleanup.

Practical choice:

- simple View transform — `view.animate()`;
- property/value control — `ObjectAnimator` / `ValueAnimator`;
- coordinated transition — `AnimatorSet` / `MotionLayout`;
- Compose UI — state-driven animation APIs;
- RecyclerView — cancel + reset state;
- production — account for system animation scale/reduced motion.

**In short:** Android has View animators, `MotionLayout`, and Compose animation
APIs. Production animation should be state-driven, cheap for frame rendering,
lifecycle-aware, and leave no stale state in recycled views.

</details>

<details>
<summary>202. What is the difference between ViewBinding and DataBinding?</summary>

#### Kotlin

`ViewBinding` and `DataBinding` both remove manual `findViewById`, but they solve
different problems.

```text
ViewBinding -> type-safe access to View
DataBinding -> data binding and expressions in XML
```

### ViewBinding

Generates a binding class for an XML layout:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Pros:

- simple API;
- type-safe access to views;
- less build overhead;
- easier debugging;
- does not move logic into XML.

### DataBinding

Allows variables and expressions to be declared directly in XML:

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

In Kotlin:

```kotlin
binding.user = user
binding.executePendingBindings()
```

DataBinding also supports two-way binding:

```xml
<EditText
    android:text="@={viewModel.query}" />
```

This can be convenient for simple forms, but in large projects it often makes
data-flow debugging more difficult.

Main DataBinding risks:

- more complex code generation;
- higher build/runtime overhead;
- less obvious errors;
- temptation to move UI/business logic into XML;
- more difficult onboarding for the team.

A bad direction is complex logic in a layout:

```xml
android:visibility="@{user.isAdmin ? View.VISIBLE : View.GONE}"
```

Simple expressions are acceptable, but complex logic is better kept in a
ViewModel or mapper.

Practical choice:

- new XML/View System code — usually `ViewBinding`;
- legacy project with XML expressions — may keep `DataBinding`;
- new declarative UI — more often Compose.

**In short:** `ViewBinding` is simple and lightweight type-safe access to views.
`DataBinding` adds variables, expressions, and two-way binding in XML, but has
more overhead and the risk of hiding logic in the layout.

</details>

<details>
<summary>203. What is Serializable?</summary>

#### Kotlin

`java.io.Serializable` is a marker interface for Java binary serialization. The
JVM can write an object graph to bytes through `ObjectOutputStream` and restore
it through `ObjectInputStream`.

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

This is not the same as `kotlinx.serialization.Serializable`: there, an
annotation generates serializers for JSON, Proto, and other formats.

In Android, `Serializable` can be passed through an `Intent` or `Bundle`:

```kotlin
val intent = Intent(context, DetailsActivity::class.java)
    .putExtra("user", user)
```

Since API 33, there is a typed overload:

```kotlin
val user = intent.getSerializableExtra(
    "user",
    User::class.java
)
```

For different API levels, it is better to use compatibility helpers, for example
`IntentCompat.getSerializableExtra()`.

Comparison with `Parcelable`:

```text
Serializable -> JVM runtime serialization, less code, more overhead
Parcelable   -> Android IPC/Bundle format, faster and more explicit
```

For Android arguments, `Parcelable` is usually better:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Even better for navigation is to pass a primitive `id` and load fresh data from
the repository.

For version compatibility, Java serialization uses `serialVersionUID`:

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

An explicit UID fixes the version identifier, but it does not automatically make
schema evolution safe.

Risks:

- reflection/metadata overhead;
- weak schema evolution;
- all nested fields must also be serializable;
- possible `NotSerializableException`;
- large objects in `Bundle/Intent` can cause `TransactionTooLargeException`;
- do not accept untrusted serialized bytes from network/external storage.

```kotlin
// bad
intent.putExtra("response", hugeResponse)

// better
intent.putExtra("user_id", user.id)
```

When it is acceptable:

- small internal argument in legacy code;
- short-lived object inside one app;
- performance and schema evolution are not critical.

**In short:** Java `Serializable` serializes an object graph using the JVM
runtime mechanism. In Android, `Parcelable` or just an `id` is usually better for
arguments, and explicit schemas such as Room, DataStore, JSON, or Proto are
better for persistence/network.

</details>

<details>
<summary>204. What serialization options are available in Android?</summary>

#### Kotlin

Serialization is converting an object into a format for transfer or storage:
JSON, bytes, `Bundle`, database row, protobuf, and so on.

In Android, it is most often needed for:

- API/network;
- navigation arguments;
- `Intent` / `Bundle` / IPC;
- local storage;
- settings.

### JSON

For APIs, JSON is most commonly used: Kotlin Serialization, Moshi, or Gson.

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

Advantages of Kotlin Serialization: good Kotlin support, `sealed class`,
compile-time plugin, Ktor, and multiplatform.

Moshi is often used with Retrofit. For production, codegen is better than
reflection.

Gson is simple and common in legacy projects, but it works worse with Kotlin
nullability, is reflection-heavy, and can silently create incorrect objects.

### Parcelable

`Parcelable` is the Android format for passing objects through `Intent`,
`Bundle`, Navigation args, or IPC.

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

For navigation, it is often better to pass not the whole object, but a primitive
`id`:

```kotlin
bundleOf("user_id" to userId)
```

Then fresh data is loaded from the repository/ViewModel.

### Serializable

Java `Serializable` is simple, but has runtime overhead, weaker schema
evolution, and less control.

```kotlin
data class User(val id: String) : Serializable
```

In Android, `Parcelable` or `id` is usually better for arguments.

### Room

For a local DB, use Room entities and mappers, rather than serializing the domain
object directly:

```kotlin
data class UserEntity(
    val id: String,
    val name: String
)

fun UserEntity.toDomain() = User(id, name)
```

`TypeConverter` can be used for individual complex fields, but Room should not
be turned into JSON storage without a real need.

### Proto / DataStore

For type-safe settings or a compact binary format, Proto DataStore is suitable.
It requires a schema, but gives a stable typed model.

Practical choice:

- API JSON — Kotlin Serialization or Moshi;
- Retrofit legacy — Moshi/Gson depending on the project;
- navigation args — primitives/id or `Parcelable`;
- Intent/Bundle — small `Parcelable`, not a large payload;
- local DB — Room entities + mappers;
- settings — DataStore or Proto DataStore;
- Java `Serializable` — only for legacy/simple internal cases.

**In short:** for network, Kotlin Serialization or Moshi is usually used; for
Android arguments, `Parcelable` or primitive `id`; for DB, Room entities; for
settings, DataStore/Proto. `Serializable` and Gson are better left for legacy or
simple scenarios.

</details>

<details>
<summary>205. What should you do if a field may be missing from an API response?</summary>

#### Kotlin

A DTO should accurately reflect the API contract: a missing field, explicit
`null`, and an invalid required field are different cases.

```text
missing optional field -> default value
explicit null          -> nullable type
missing required field -> decoding/mapping error
```

If the backend can send `null`, make the field nullable:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

It is better to define fallback in the mapper rather than carry nullable values
everywhere in the UI:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name?.takeIf { it.isNotBlank() } ?: "Unknown",
    avatarUrl = avatarUrl
)
```

If a field can be missing but has a safe default value:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Missing and explicit `null` are not the same:

```json
{}
```

```json
{ "name": null }
```

For `kotlinx.serialization`, a default value is used for a missing key. A
nullable property without a default can still be required, depending on the
declaration/config. The behavior of `explicitNulls` should be understood and
covered by tests.

DTOs should not be passed directly to the UI. A mapper should create a
domain/UI model with clear guarantees:

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

A required field should not be masked with a fallback:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

The repository can convert an invalid response into a typed error:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

Practical rules:

- optional missing field — default;
- explicit `null` — nullable type;
- required field — validation/failure;
- DTO → mapper → domain/UI;
- cover serializer behavior with contract tests: missing, null, wrong type,
  unknown enum.

**In short:** model a missing field with a default value and explicit `null`
with a nullable type. A required field without a value should produce a
validation error, not a silent fallback. DTOs should be mapped into domain/UI
models.

</details>

<details>
<summary>206. What is Dependency Injection?</summary>

#### Kotlin

`Dependency Injection` is passing a class's dependencies from the outside
instead of creating them inside the class. This reduces coupling, simplifies
testing, and helps manage lifecycle.

Bad without DI:

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Problems:

- difficult to replace the dependency in a test;
- the class itself is responsible for creation;
- the implementation is tightly coupled;
- lifecycle/configuration is controlled in the wrong place.

A better default is constructor injection:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

The class receives a ready dependency and does not know who created it.

Usually, the caller depends on a contract:

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

In Android, DI connects `ViewModel`s, use cases, repositories, API clients, Room
DB, and dispatchers.

Hilt example:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Binding an interface:

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

For third-party objects, use `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

Scopes define an instance lifetime:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

The scope should match the owner. An unnecessary singleton can hold an object
longer than needed and create a leak.

In tests, the real implementation is replaced with a fake:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

DI does not necessarily require a framework. For a small graph, manual DI is
enough:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Practical rules:

- constructor injection is the default;
- the caller depends on an interface/contract;
- scopes match the lifecycle;
- Hilt/Dagger/Koin only automate wiring;
- DI does not replace good architecture.

**In short:** DI passes dependencies from the outside. It reduces coupling,
allows dependencies to be replaced in tests, and helps manage lifecycle. For a
small graph, manual DI is enough; for a large one, use Hilt/Dagger/Koin.

</details>

<details>
<summary>207. What is the difference between Dagger/Hilt and Koin?</summary>

#### Kotlin

`Dagger/Hilt` and `Koin` are DI solutions with different wiring models.

```text
Dagger/Hilt -> generated code + compile-time graph validation
Koin        -> runtime container + Kotlin DSL/annotations
```

### Dagger / Hilt

`Dagger` generates factories/components and validates the dependency graph during
compilation.

Pros:

- compile-time validation;
- generated code without runtime lookup;
- explicit scopes/components;
- well suited for large strict graphs.

Cons:

- more annotations/modules;
- more complex component hierarchy;
- compiler errors are sometimes hard to read;
- can affect build time.

`Hilt` is an Android layer on top of Dagger. It provides a standard Android
component hierarchy, scopes, and Jetpack integrations.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

Typical Hilt scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

### Koin

Koin describes dependencies through a Kotlin DSL or annotations:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Pros:

- less ceremony;
- easy-to-read DSL;
- fast to start in small/medium projects;
- convenient for custom wiring and KMP use cases.

Cons:

- classic DSL has more runtime lookup;
- missing/ambiguous definitions may appear at runtime if there are no
  compile-time checks;
- scopes and ownership graph must be modeled by the team with discipline.

Modern Koin has tooling/annotations for compile-time checks, but the model is
still different from Dagger: Koin remains container-based, while Dagger/Hilt is
generated-graph-based.

### Performance and Testing

Dagger/Hilt calls generated factories. Koin resolves definitions through a
container. The difference should be measured on a real graph, rather than
choosing a framework by microbenchmark.

For tests:

- Hilt — test components, `@TestInstallIn`, module replacement;
- Koin — test modules, overrides;
- unit tests — simplest is to pass a fake directly through the constructor.

### What to Choose

- Hilt is a good default for Android with Jetpack and a large graph;
- Dagger is for cases that need a very explicit and controlled DI graph;
- Koin is useful when DSL simplicity, wiring speed, or KMP matters;
- existing working DI should not be rewritten without measurable benefit.

**In short:** Dagger/Hilt generates DI code and validates the graph at compile
time. Koin provides a simpler DSL/container-based approach, but requires
discipline in scopes and checks. The choice depends on graph size,
build/runtime requirements, and team experience.

</details>

<details>
<summary>208. What is CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` is a Jetpack Compose mechanism for passing values down the
composition tree without explicitly passing parameters through every composable.

It is scoped UI context for a subtree:

```kotlin
CompositionLocalProvider(LocalSpacing provides AppSpacing()) {
    AppContent()
}
```

Declaration:

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

Usage:

```kotlin
@Composable
fun ProfileCard() {
    val spacing = LocalSpacing.current

    Column(Modifier.padding(spacing.medium)) {
        Text("Profile")
    }
}
```

`ProfileCard` reads the nearest `LocalSpacing` value provided above it in the
tree.

The provider applies only to the child subtree:

```kotlin
CompositionLocalProvider(
    LocalSpacing provides AppSpacing(medium = 20.dp)
) {
    ProfileScreen()
}
```

API difference:

```text
compositionLocalOf       -> more precise read tracking, for changing values
staticCompositionLocalOf -> cheaper read, for almost static values
```

For design system tokens, `staticCompositionLocalOf` is often used. For values
that change more often, use `compositionLocalOf`.

Built-in locals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`;
- `LocalLayoutDirection`.

```kotlin
val context = LocalContext.current
```

CompositionLocal is appropriate for:

- theme colors;
- typography;
- spacing;
- design tokens;
- locale/configuration;
- UI helpers that are logically available to the whole subtree.

Business dependencies should not be hidden through CompositionLocal:

```kotlin
val repository = LocalUserRepository.current // bad
```

Repositories, use cases, and services should come through DI/ViewModel.
Otherwise, CompositionLocal turns into a service locator.

Risks:

- hidden dependencies;
- more difficult testing;
- unclear value origin;
- unnecessary recompositions when values change often;
- overuse instead of explicit parameters.

**In short:** `CompositionLocal` passes scoped UI values down the Compose tree
without parameter drilling. It is good for theme, spacing, typography, and
context-like UI data, but not for repositories, use cases, or business logic.

</details>

<details>
<summary>209. What types of CompositionLocal are there?</summary>

#### Kotlin

In Compose, there are two main ways to create a `CompositionLocal`:

```text
staticCompositionLocalOf -> cheaper read, less precise invalidation
compositionLocalOf       -> tracked reads, more precise recomposition
```

`staticCompositionLocalOf`:

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

Reads are not tracked individually. If the value changes, the entire provider
content is invalidated. This is good for almost static UI values:

- theme colors;
- typography;
- spacing;
- design-system tokens.

`compositionLocalOf`:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

Compose tracks which composables read `.current`, and when the value changes, it
invalidates more precisely. This is better for values that may change more
often.

The provider sets a value for a subtree:

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

A nested provider can override the value only for its own subtree.

Design system local example:

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

Usage:

```kotlin
Modifier.padding(LocalSpacing.current.medium)
```

Built-in CompositionLocals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLayoutDirection`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`.

They describe the UI environment of the current subtree.

CompositionLocal should not be used as a service locator:

```kotlin
val repository = LocalRepository.current // bad
```

Repositories, use cases, and business services should come through DI/ViewModel.
Important inputs of a specific composable are better passed as explicit
parameters.

Risks:

- hidden dependencies;
- more difficult tests;
- unclear value owner;
- unnecessary recompositions;
- replacing DI and state hoisting.

**In short:** `staticCompositionLocalOf` is suitable for stable UI values and
cheaper reads. `compositionLocalOf` tracks reads more precisely and is better for
changing values. Both should be used for UI context, not business dependencies.

</details>

<details>
<summary>210. How does CompositionLocal work under the hood?</summary>

#### Kotlin

`CompositionLocal` works as a scoped value in the Compose tree.
`CompositionLocalProvider` writes a value for a subtree, and composables below it
read the nearest value through `.current`.

Basic model:

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

CompositionLocalProvider(LocalSpacing provides AppSpacing(16.dp)) {
    Content()
}
```

Reading:

```kotlin
val spacing = LocalSpacing.current
```

Compose searches for the nearest provider for `LocalSpacing` in the composition
hierarchy.

The provider applies only to the subtree:

```text
Root
 ├─ LocalSpacing = 8dp
 │   └─ ScreenA reads 8dp
 └─ LocalSpacing = 16dp
     └─ ScreenB reads 16dp
```

A nested provider overrides the outer one:

```kotlin
CompositionLocalProvider(LocalSpacing provides smallSpacing) {
    Header()

    CompositionLocalProvider(LocalSpacing provides largeSpacing) {
        Content()
    }
}
```

`Header` receives `smallSpacing`, and `Content` receives `largeSpacing`.

Compose uses reads for invalidation. When a composable reads:

```kotlin
val colors = LocalAppColors.current
```

Compose can connect this read to the value and, when it changes, decide what
needs to be recomposed.

Difference between types:

```text
compositionLocalOf       -> tracked reads, more precise invalidation
staticCompositionLocalOf -> reads are not tracked separately, cheaper read
```

`compositionLocalOf` is better for values that may change more often:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`staticCompositionLocalOf` is better for almost static values: theme,
typography, spacing.

```kotlin
val LocalTypography = staticCompositionLocalOf {
    DefaultTypography
}
```

The default factory is called if no value is provided:

```kotlin
val LocalAnalytics = staticCompositionLocalOf<Analytics> {
    error("Analytics not provided")
}
```

For required values, it is often better to throw `error` so incorrect setup is
found quickly.

Performance nuance: if a CompositionLocal changes often and is read in a large
subtree, it can cause unnecessary recompositions. Do not put high-frequency
state there: scroll position, text input value, or timer ticks.

Practical rule: `CompositionLocal` is not a global variable, but scoped UI
context. It is used for UI environment values, not hidden access to
repositories/use cases.

**In short:** `CompositionLocalProvider` sets a value for a subtree, `.current`
reads the nearest value, and Compose uses reads for invalidation.
`compositionLocalOf` tracks reads more precisely; `staticCompositionLocalOf` is
cheaper for almost static values.

</details>

<details>
<summary>211. What are side effects in Compose?</summary>

#### Kotlin

A side effect in Compose is an action outside the pure UI description: a
coroutine, listener, navigation, snackbar, analytics, or synchronization with an
external object.

A composable body can execute many times, so effects should not be started
directly:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // bad: repeats on recomposition
    Text(userId)
}
```

### LaunchedEffect

Starts a coroutine tied to the composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

When the key changes, the previous coroutine is canceled and a new one starts.
When it leaves composition, the coroutine is also canceled. Keys should contain
the effect's dependencies.

`LaunchedEffect(Unit)` is one launch for the current entry into composition, not
once for the whole app lifetime.

### rememberCoroutineScope

Needed when a coroutine starts from a callback:

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

`LaunchedEffect` starts declaratively by keys, while `rememberCoroutineScope`
starts imperatively after a user event.

### DisposableEffect

Used for register/unregister lifecycle:

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

Every listener/observer registration should have symmetric cleanup in
`onDispose`.

### SideEffect

Runs after every successful composition:

```kotlin
SideEffect {
    externalController.enabled = isEnabled
}
```

It is suitable for publishing Compose state to a non-Compose object. It is not
suitable for suspend or heavy work.

### Other APIs

`rememberUpdatedState` provides the latest lambda/value to a long-running effect
without restart:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

`snapshotFlow` converts Compose state reads into a Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect(analytics::trackScroll)
}
```

`produceState` adapts a suspend/callback source into Compose `State`. For Flow in
UI, `collectAsStateWithLifecycle()` is more commonly used.

`derivedStateOf` is not a side effect; it is derived state for optimizing
computed UI state.

Practical rules:

- do not start effects in the composable body;
- set keys correctly;
- do cleanup in `onDispose`;
- use `rememberCoroutineScope` for callback coroutines;
- use `rememberUpdatedState` for the latest lambda without restart;
- store durable data as state, not as a one-time effect.

**In short:** side effects in Compose manage actions outside the UI description.
`LaunchedEffect` starts a coroutine by keys, `DisposableEffect` manages cleanup,
`SideEffect` synchronizes an external object, and `rememberCoroutineScope` is
needed for a coroutine from a callback.

</details>

<details>
<summary>212. How do you optimize recomposition?</summary>

#### Kotlin

Recomposition should not be “disabled”. It should be local and cheap. Problems
appear because of broad state reads, unstable parameters, or heavy work in the
composable body.

### State at the Right Level

State should be kept in the lowest common owner that needs it.

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

If `query` is needed only by the search block, it should not be read at the
whole-screen level.

### Narrow Reads

Splitting code into functions does not guarantee optimization by itself. A
composable should read only the state it needs and receive stable parameters;
then Compose can skip unchanged groups.

### Immutable/Stable Models

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

Use `@Immutable` only if the contract is truly satisfied. Mutable properties and
in-place mutations break predictability.

Correctly update state with an immutable snapshot:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

### Lazy Lists

Lazy items need a stable `key`:

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

`key` preserves remembered state during insert/delete/reorder. For mixed lists,
also set `contentType`.

### Do Not Do Heavy Work in Composition

Transformations should either be moved to a state holder or cached by input:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

Do not do I/O, bitmap decoding, heavy sorting/filtering without cache directly
in the composable body.

### derivedStateOf

`derivedStateOf` is useful when the source changes often, but the UI reacts only
to a derived condition:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

### Profiling

Optimization should be confirmed with tools:

- recomposition counters;
- Layout Inspector;
- Compose compiler reports;
- System Trace / Perfetto;
- Android Studio Profiler;
- Macrobenchmark in a release-like build.

A high recomposition count is not always a problem by itself if the composable
is cheap.

**In short:** optimize recomposition through narrow state reads,
immutable/stable models, immutable updates, stable keys in lazy lists,
`remember/derivedStateOf`, and moving heavy work out of composition. Measure with
a profiler, not by eye.

</details>

<details>
<summary>213. How do you write an asynchronous test without using coroutines?</summary>

#### Kotlin

An asynchronous test without coroutines should wait for a specific event:
callback, result, state update, or idle state. Waiting should always have a
timeout. `Thread.sleep()` is a bad option.

### CountDownLatch

For callback-based APIs:

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

For one result/error:

```kotlin
val future = CompletableFuture<User>()

repository.loadUser("42", object : UserCallback {
    override fun onSuccess(user: User) = future.complete(user)
    override fun onError(error: Throwable) = future.completeExceptionally(error)
})

assertEquals("42", future.get(2, TimeUnit.SECONDS).id)
```

### Waiting for State

You can use Awaitility or a custom polling helper:

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

The test finishes immediately after the condition is satisfied instead of
waiting for a fixed delay.

### LiveData

For `LiveData`, use `InstantTaskExecutorRule` and a helper such as
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

The helper should subscribe, wait for a value with a timeout, and remove the
observer in `finally`.

### UI Tests

Espresso synchronizes through `IdlingResource`:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

### Fake Dependency

The most stable unit test uses a controlled fake:

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

The test calls `complete()` itself, so it does not depend on network/thread
scheduler timing.

Anti-pattern:

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

A short delay creates a flaky test; a long one wastes time.

**In short:** an asynchronous test without coroutines should wait for a specific
event with a timeout. Use `CountDownLatch`, `CompletableFuture`,
polling/Awaitility, a LiveData helper, Espresso `IdlingResource`, or a controlled
fake, but not `Thread.sleep()`.

</details>

<details>
<summary>214. What is the difference between inline, noinline, and crossinline?</summary>

#### Kotlin

`inline`, `noinline`, and `crossinline` are modifiers for higher-order
functions. They control how the compiler works with lambda parameters.

```text
inline      -> inline the function/lambda at the call site
noinline    -> do not inline a specific lambda
crossinline -> inline it, but forbid non-local return
```

### inline

`inline` asks the compiler to insert the function body and lambda at the call
site:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

This reduces the overhead of creating a lambda object and making a function call
for small higher-order functions.

`inline` is also required for `reified` generics:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T =
    fromJson(json, T::class.java)
```

Without `inline`, a `reified` type parameter is impossible.

### Non-local return

In an inline lambda, you can `return` from the outer function:

```kotlin
inline fun runBlock(block: () -> Unit) = block()

fun test() {
    runBlock {
        return // exits test()
    }
}
```

This is called a non-local return.

### noinline

`noinline` disables inlining for a specific lambda:

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

`noinline` is needed if the lambda must be stored in a variable, passed further,
or used as an object.

### crossinline

`crossinline` keeps inlining, but forbids non-local return:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

This is needed when the lambda is executed in another context, for example
inside a `Runnable`, callback, or another lambda.

Example together:

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

Meaning:

- `inlined` — fully inline;
- `stored` — can be stored/passed;
- `safe` — inline, but without non-local return.

`inline` should not be used everywhere. For large functions or frequent call
sites, it can bloat bytecode. It is best suited for small higher-order
utilities, DSLs, and `reified` generics.

**In short:** `inline` inlines a function/lambda and is required for `reified`;
`noinline` keeps a lambda object so it can be stored or passed; `crossinline`
forbids non-local return when a lambda is executed in another context.

</details>

<details>
<summary>215. What advantages does Kotlin have over Java?</summary>

#### Kotlin

Kotlin has practical advantages over Java, especially in Android: null safety,
less boilerplate, coroutines, extension functions, data/sealed classes, smart
casts, and strong Jetpack integration.

### Null Safety

Kotlin separates nullable and non-null types:

```kotlin
val name: String = "Alex"
val nickname: String? = null
```

Nullable values must be handled explicitly:

```kotlin
val title = user.name ?: "Unknown"
```

This does not eliminate all `NullPointerException`s, but it greatly reduces
their number.

### Less Boilerplate

```kotlin
data class User(
    val id: String,
    val name: String
)
```

`data class` generates `equals`, `hashCode`, `toString`, `copy`, and component
functions.

### Coroutines

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = State.Success(user)
}
```

Coroutines make async code more readable than callbacks and convenient for
structured concurrency.

### Extension Functions

```kotlin
fun View.show() {
    visibility = View.VISIBLE
}
```

They allow utility APIs to be added without inheritance or wrapper classes.

### Sealed Classes/Interfaces

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}
```

They are well suited for UI state, results, effects, and finite state models.

### Smart Casts

```kotlin
if (state is UiState.Content) {
    Text(state.user.name)
}
```

After a type check, Kotlin performs the cast automatically.

### Named/Default Arguments

```kotlin
fun createUser(name: String, active: Boolean = true)

createUser(name = "Alex")
```

Fewer overloads, more readable call sites.

### Java Interoperability

Kotlin works well with Java code and libraries, so a project can be migrated
gradually.

### Android Ecosystem

Many modern Android APIs are Kotlin-first:

- Jetpack Compose;
- Coroutines/Flow;
- KTX extensions;
- Room suspend/Flow APIs;
- Lifecycle scopes;
- Navigation Kotlin DSL.

There are tradeoffs too:

- build time can be worse;
- coroutines require deep understanding;
- Java interop gives platform types;
- complex generics/type inference edge cases;
- overusing DSLs/extensions can reduce readability.

**In short:** Kotlin gives safer and more expressive code: null safety,
data/sealed classes, coroutines, extension functions, smart casts, named/default
arguments, and better Android tooling. But quality depends on architecture and
discipline, not only on the language.

</details>

<details>
<summary>216. What is the difference between val and const val?</summary>

#### Kotlin

`val` and `const val` are both read-only, but they are different things.

```text
val       -> read-only property, value may be runtime
const val -> compile-time constant
```

`val`:

```kotlin
val name = "Alex"
val createdAt = System.currentTimeMillis()
```

A `val` cannot be reassigned after initialization, but its value may be computed
at runtime.

`const val`:

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

A `const val` must be known to the compiler and is inlined at usage sites.

Where `const val` can be declared:

- top-level;
- in an `object`;
- in a `companion object`.

```kotlin
object ApiConfig {
    const val BASE_PATH = "api/v1"
}
```

You cannot declare a `const val` as a local variable inside a function.

Types for `const val` are limited to compile-time literals:

- `String`;
- primitives: `Int`, `Long`, `Boolean`, `Double`, and so on.

Not allowed:

```kotlin
const val DATE = LocalDate.now() // error
```

`const val` is needed where an API requires a compile-time constant, for example
annotations:

```kotlin
const val TABLE_USERS = "users"

@Entity(tableName = TABLE_USERS)
data class UserEntity(...)
```

A regular `val` will not work here if the annotation expects a constant.

Java interop: `const val` is compiled as a `static final` field and can be used
from Java as a constant.

Risks:

- `const val` is inlined, so after changing its value in a library, dependent
  modules may need recompilation;
- `const val` does not protect secrets/API keys — the value is easy to extract
  from the APK;
- do not make everything that simply “does not change” a `const val`.

Practical choice:

- runtime value or object — `val`;
- literal compile-time constant — `const val`;
- value for an annotation — `const val`;
- secrets — not `const val`, but a proper security/config strategy.

**In short:** `val` is a read-only property that can be a runtime value.
`const val` is a compile-time constant for `String`/primitives, allowed
top-level or in `object/companion object`, and suitable for annotations. Secrets
must not be stored in `const val`.

</details>

<details>
<summary>217. What types can be used with const val?</summary>

#### Kotlin

`const val` is a compile-time constant. The type must be `String` or a primitive,
and the initializer must be known to the compiler without running the program.

Allowed types:

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

Typical examples:

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Where it can be declared:

- top-level;
- in an `object`;
- in a `companion object`.

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

You cannot declare a `const val` as a local variable or instance property.

What is not allowed:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val UUID_VALUE = UUID.randomUUID()
const val DEFAULT_THEME = ThemeMode.Light
const val NAME: String? = null
```

For objects, collections, nullable values, and runtime expressions, use a
regular `val`:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

The initializer may contain literals, other constants, and compile-time
operations:

```kotlin
const val HOST = "example.com"
const val API_URL = "https://" + HOST
const val TIMEOUT_SECONDS = 5 * 2
```

Function calls, `Context`, current time, random values, or object creation are
not compile-time constants.

`const val` is often needed for annotation arguments:

```kotlin
const val USERS_TABLE = "users"

@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

**In short:** `const val` supports only `String` and primitive types with a
compile-time initializer. For objects, collections, nullable values, or runtime
values, use a regular `val`.

</details>

<details>
<summary>218. Can you create a data class without parameters?</summary>

#### Kotlin

No. A `data class` must have a primary constructor with at least one parameter,
and that parameter must be `val` or `var`.

Incorrect:

```kotlin
data class Empty
data class Empty()
data class User(id: String)
```

Correct:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Reason: the compiler generates methods from primary-constructor properties:

```text
equals()
hashCode()
toString()
copy()
componentN()
```

A property in the body is not included in generated equality, hash, copy, or
destructuring:

```kotlin
data class User(
    val id: String
) {
    var cachedLabel: String = ""
}
```

Two `User` objects with the same `id` but different `cachedLabel` will be equal.
Therefore, value-defining fields should be in the primary constructor.

A no-argument call is possible through default values:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)

val user = User()
```

The class still has parameters; the caller simply uses defaults. However,
defaults should not create an invalid domain object only for the sake of a
framework.

If a framework requires a no-arg constructor, it is better to use:

- a separate persistence/DTO model;
- an adapter/mapper;
- the Kotlin no-arg plugin;
- valid defaults, if they truly make sense.

If a state has no payload, use a singleton instead:

```kotlin
data object Loading
```

In a sealed hierarchy:

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

Other restrictions: a `data class` cannot be `abstract`, `open`, `sealed`, or
`inner`, but it can implement interfaces.

Practical choice:

- value model — `data class`;
- no-arg call — default values;
- state without payload — `object` / `data object`;
- framework no-arg — adapter/plugin;
- value fields — only primary constructor.

**In short:** an empty `data class` does not compile. It needs at least one
`val/var` parameter in the primary constructor. For a no-arg call, use defaults;
for state without payload, use `object` or `data object`.

</details>

<details>
<summary>219. What is the difference between sealed class and enum?</summary>

#### Kotlin

`enum` and `sealed` both restrict the set of variants, but they model different
things.

```text
enum   -> fixed singleton entries of one type
sealed -> closed hierarchy of different subtypes
```

### Enum

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

`enum` is suitable when all cases have the same structure and do not need
different payload. Each entry is a singleton:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

An enum can have shared properties:

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

All entries have the same property contract. Enum has built-in APIs: `entries`,
`valueOf()`, `name`, `ordinal`.

`ordinal` should not be stored in a DB/API: entry order can change. For
persistence, use an explicit stable code.

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

`sealed` is suitable when variants have different payload or behavior.
`Loading` is a singleton, `Content` has data, and `Error` has another model.

Typical use cases:

- UI state;
- domain result;
- errors;
- events;
- finite state machine.

### Exhaustive when

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

The compiler knows all cases and can check an exhaustive `when`. An unnecessary
`else` often hides the value of this check.

### Sealed class vs sealed interface

`sealed class` can have constructor state and protected members:

```kotlin
sealed class AppError(
    open val code: Int
)
```

`sealed interface` has no constructor state, but an implementation can implement
several interfaces and inherit from another class. For state/result models,
`sealed interface` is often enough.

### Serialization

Enum is usually serialized as a stable textual/code value. A sealed hierarchy
requires a discriminator for the subtype and a schema for the payload. Versioning
and unknown variants must be considered.

Practical choice:

- simple fixed mode/status — `enum`;
- cases without different payload — `enum`;
- UI/result/error state with different fields — `sealed`;
- object + data class cases in one model — `sealed`;
- do not replace enum with a sealed type “for the future” without need.

**In short:** `enum` is a fixed list of singleton entries with a shared
structure. `sealed class/interface` is a closed hierarchy where each case can
have its own payload. For simple modes, use `enum`; for state/result models, use
`sealed`.

</details>

<details>
<summary>220. What is WorkManager?</summary>

#### Kotlin

`WorkManager` is a Jetpack API for guaranteed deferred background work. It is
used for tasks that should run even after the app is closed or the process is
killed, but do not necessarily need to run right now.

It is suitable for:

- background sync;
- upload/download with retry;
- cleaning up old data;
- sending logs/analytics;
- periodic sync;
- tasks with constraints: network, charging, battery not low.

One-time work:

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

Results:

```text
Result.success() -> completed
Result.retry()   -> retry later
Result.failure() -> finish with failure
```

Constraints:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()
```

WorkManager runs the work only when the constraints are satisfied.

Periodic work:

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodic work does not guarantee an exact launch time: Android chooses the
moment based on battery and system policy.

To avoid duplicates, use unique work:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` keeps the current task; `REPLACE` cancels the old one and enqueues a new
one.

Input data should be small:

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Large objects are better stored in a DB, passing only an `id`.

Chaining:

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

WorkManager is not suitable for:

- exact alarms;
- persistent socket connection;
- media playback;
- realtime tracking;
- work that must start immediately;
- long user-visible foreground scenarios without interruption.

**In short:** `WorkManager` is for deferrable, guaranteed background work with
constraints, retry, periodic/unique work, and chaining. For realtime, exact
alarms, media playback, or immediate foreground work, use other APIs.

</details>

<details>
<summary>221. When should you use WorkManager?</summary>

#### Kotlin

`WorkManager` should be used for persistent deferrable work: a task can be
deferred, but it must be reliably executed after the UI is closed, process death,
or reboot, while respecting constraints.

It is suitable for:

- background sync;
- retry upload/download;
- cleaning up old data;
- sending logs/analytics;
- periodic refresh;
- tasks with network/charging/battery constraints.

Exact time is not guaranteed. Force stop and uninstall stop the work.

One-time work:

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

`CoroutineWorker` example:

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

Results:

```text
success() -> completed
retry()   -> transient failure
failure() -> permanent failure
```

A worker should be idempotent: the system can stop and start it again.
`CancellationException` should not be converted into retry.

Constraints describe launch conditions, but not exact time:

- network;
- charging;
- battery not low;
- storage not low.

Unique work prevents duplicate jobs:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "user-sync",
        ExistingWorkPolicy.KEEP,
        request
    )
```

Periodic work runs approximately: Doze, battery policy, and constraints can
shift execution.

Input `Data` should be small: primitives/String. Large objects are better stored
in Room/file, passing only an `id` to the worker.

When it is not suitable:

```text
screen-bound work -> ViewModel/lifecycle scope
exact alarm       -> AlarmManager
media/tracking    -> Foreground Service
realtime socket   -> active runtime architecture
```

`ExpeditedWorkRequest` has quotas and does not make WorkManager a realtime API.

**In short:** WorkManager is needed for idempotent background work that can be
deferred but must be reliably executed with constraints and retry. UI-bound,
realtime, media, tracking, or exact-time tasks require other APIs.

</details>

<details>
<summary>222. Have you worked with Android Media3 / ExoPlayer?</summary>

#### Kotlin

Yes. `Media3` is the modern Jetpack media stack, and `ExoPlayer` is the default
implementation of the `Player` interface. It supports local/streaming media,
playlists, adaptive streaming, subtitles, and DRM.

Basic usage:

```kotlin
val player = ExoPlayer.Builder(context).build()

player.setMediaItem(MediaItem.fromUri(videoUrl))
player.prepare()
player.play()
```

In View UI:

```kotlin
playerView.player = player
```

The main point is to correctly define the player owner and always call
`release()`.

```kotlin
override fun onDestroyView() {
    playerView.player = null
    player.release()
    super.onDestroyView()
}
```

A reference to `PlayerView` must not outlive `onDestroyView()`.

In Compose, legacy `PlayerView` can be wrapped through `AndroidView`:

```kotlin
AndroidView(
    factory = { context ->
        PlayerView(context).apply {
            player = exoPlayer
        }
    }
)
```

The player must not be created on every recomposition. It needs a stable owner
and cleanup through `DisposableEffect`, a holder, or a service.

Playlist:

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

`Player.Listener` provides playback states and errors:

- `BUFFERING`;
- `READY`;
- `ENDED`;
- playback error;
- media item transition.

The UI maps these events into loading/error/replay state. Analytics is better
kept separate from rendering logic.

For background playback, music, or podcasts, the player should not belong to an
`Activity`/`Fragment`. The correct scheme is:

```text
MediaSessionService owns Player + MediaSession
UI connects via MediaController
```

This provides:

- media notification;
- system controls;
- Bluetooth/Wear OS/Android Auto integration;
- playback independence from screen lifecycle.

For streaming/cache, use `CacheDataSource`, explicitly defining cache size,
eviction strategy, offline behavior, and DRM constraints.

Common mistakes:

- not calling `release()`;
- creating the player during recomposition;
- keeping `PlayerView` after `onDestroyView()`;
- doing background playback in screen lifecycle;
- not handling errors/audio focus;
- calling the player not from its application thread.

**In short:** Media3/ExoPlayer requires a clear owner, lifecycle cleanup,
`release()`, handling states/errors, and correct thread access. For background
playback, use `MediaSessionService`, not an `Activity` or `Fragment`.

</details>

<details>
<summary>223. How would you split components into modules if there is a button that loads data on click?</summary>

#### Kotlin

Modules should be built around a feature/use case, not around a button.

```text
Button -> ViewModel -> UseCase -> Repository -> API/DB
```

The UI only sends an event and renders state. It does not know where the data
came from: network, Room, or cache.

For a small app, this structure is enough:

```text
:app
:core:network
:core:database
:feature:profile
```

In `:feature:profile`, there can be packages:

```text
presentation
domain
data
```

Separate Gradle modules for every layer are not needed if there is no real
isolation/ownership.

For a large feature, API and implementation can be separated:

```text
:feature:profile:api
:feature:profile:impl
```

`api` contains public contracts: navigation, capabilities, interfaces. `impl`
contains the screen, ViewModel, use cases, and repository implementation.

### Presentation

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

ViewModel:

- accepts the click event;
- starts a coroutine;
- manages `loading/content/error`;
- handles duplicate clicks;
- keeps state in `StateFlow`.

Retrofit, SQL, and cache logic should not be in presentation.

### Domain

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

A use case is needed if there is a business rule, orchestration, or reuse. If it
is a simple proxy in a small feature, the ViewModel can depend directly on the
repository contract.

Domain does not depend on Android UI, Retrofit, Room, or a DI framework.

### Data

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

The data layer is responsible for API, DB, cache/source of truth, mapping, and
infrastructure error mapping.

Dependency direction:

```text
app -> feature impl
presentation -> domain contract
data -> domain contract
core -X-> feature
```

DI connects the implementation to the contract in the composition root:

```kotlin
@Binds
abstract fun bindRepository(
    impl: ProfileRepositoryImpl
): ProfileRepository
```

Criteria for a separate Gradle module:

- independent ownership;
- stable public API;
- dependency isolation;
- reusable capability;
- build-time benefit.

If there are no such reasons, packages are simpler. Excessive modularization
adds Gradle, DI, and navigation boilerplate without real benefit.

**In short:** the button and state live in `presentation`, the business
operation in `domain/use case`, and API/DB plus repository implementation in
`data`. For a small app, packages are enough; add Gradle modules only for
isolation, ownership, or build benefit.

</details>
