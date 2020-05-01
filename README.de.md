**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Die beliebtesten Fragen und Antworten im Kotlin-Vorstellungsgespräch</h2>

<details>
<summary>1. Was ist Kotlin und wie arbeitet es mit Java zusammen?</summary>

#### Kotlin

Kotlin ist eine statisch typisierte Programmiersprache von JetBrains. Sie
unterstützt die JVM, Android, JavaScript, WebAssembly und native Plattformen.

Auf der JVM wird Kotlin in JVM-Bytecode kompiliert und ist daher vollständig mit
Java kompatibel:

- Kotlin kann Java-Code direkt aufrufen und Java-Bibliotheken verwenden.
- Java kann Kotlin-Code wie gewöhnliche JVM-Klassen und -Methoden aufrufen.
- Beide Sprachen können innerhalb desselben Projekts und Moduls verwendet werden.
- Die Annotationen `@JvmStatic`, `@JvmOverloads`, `@JvmField` und `@JvmName`
  erleichtern die Erstellung einer komfortablen API für Java.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

Ein wichtiger Aspekt sind Plattformtypen: Wenn eine Java-API keine korrekten
Nullability-Annotationen enthält, kann Kotlin nicht immer feststellen, ob
`null` zulässig ist. Deshalb sind an der Schnittstelle zwischen Java und Kotlin
eindeutige Verträge erforderlich.

**Kurz gesagt:** Kotlin ist auf JVM-Ebene mit Java kompatibel und ermöglicht es,
Kotlin schrittweise in ein bestehendes Java-Projekt einzuführen, ohne dieses
vollständig neu schreiben zu müssen.

</details>

<details>
<summary>2. Welche grundlegenden Datentypen gibt es in Kotlin?</summary>

#### Kotlin

Die wichtigsten Kotlin-Typen sind:

- Ganzzahlen: `Byte`, `Short`, `Int`, `Long`;
- vorzeichenlose Ganzzahlen: `UByte`, `UShort`, `UInt`, `ULong`;
- Gleitkommazahlen: `Float`, `Double`;
- Wahrheitswerte: `Boolean`;
- Zeichen und Zeichenketten: `Char`, `String`;
- Arrays: `Array<T>`, `IntArray`, `LongArray` und weitere.

Darüber hinaus spielen folgende Typen im Typsystem eine wichtige Rolle:

- `Any` ist der Basistyp aller Typen, die nicht `null` sein können;
- `Unit` bedeutet, dass eine Funktion kein relevantes Ergebnis zurückgibt;
- `Nothing` bedeutet, dass eine Funktion niemals regulär beendet wird;
- nullable Typen werden mit `?` gekennzeichnet, beispielsweise `Int?`.

Auf der JVM können Typen wie `Int` in primitive Java-Typen kompiliert werden.
Nullable Typen und Generics erfordern hingegen in der Regel Boxing.

- In Kotlin findet keine implizite Erweiterung numerischer Typen statt: Ein
  `Int` wird nicht automatisch in einen `Long` umgewandelt:

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**Kurz gesagt:** In Kotlin gibt es keine separate Syntax für primitive Typen und
Objekttypen. Der JVM-Compiler verwendet jedoch primitive Typen, wann immer dies
möglich ist.

</details>

<details>
<summary>3. Worin besteht der Unterschied zwischen val und var in Kotlin?</summary>

#### Kotlin

`val` verhindert eine erneute Zuweisung, während `var` sie erlaubt:

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Kompilierungsfehler
```

`val` macht nicht das Objekt selbst unveränderlich, sondern lediglich die
Referenz darauf:

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Zulässig
```

Bei Klasseneigenschaften besitzt `val` nur einen Getter, während `var` sowohl
einen Getter als auch einen Setter besitzt.

**Kurz gesagt:** Verwende standardmäßig `val` und greife nur dann auf `var`
zurück, wenn der Wert tatsächlich geändert werden muss.

</details>

<details>
<summary>4. Was bedeutet Typinferenz (type inference) in Kotlin?</summary>

#### Kotlin

Typinferenz bezeichnet die Fähigkeit des Compilers, einen Typ aus einem Ausdruck
oder aus dem Kontext abzuleiten, ohne dass er ausdrücklich angegeben werden
muss. Kotlin bleibt dabei eine statisch typisierte Sprache.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it ist ein String
```

Ein Typ sollte explizit angegeben werden, wenn er:

- Bestandteil einer öffentlichen API ist;
- die Lesbarkeit eines komplexen Ausdrucks verbessert;
- vom Typ der konkreten Implementierung abweichen soll.

```kotlin
val users: List<User> = mutableListOf()
```

**Kurz gesagt:** Der Compiler leitet offensichtliche Typen automatisch ab;
explizite Typangaben sind für Verträge und eine bessere Lesbarkeit erforderlich.

</details>

<details>
<summary>5. Was sind Erweiterungsfunktionen (extension functions)?</summary>

#### Kotlin

Eine Erweiterungsfunktion ergänzt einen vorhandenen Typ um eine komfortable
Aufrufsyntax, ohne die Klasse selbst zu verändern oder von ihr zu erben.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` ist der Empfängertyp. Innerhalb der Funktion ist das zugehörige Objekt
über `this` zugänglich.

Wichtige Einschränkungen:

- Eine Erweiterung hat keinen Zugriff auf `private`- und `protected`-Member der
  Klasse.
- Eine Klassenmethode hat Vorrang vor einer Erweiterung mit derselben Signatur.
- Eine Erweiterung wird statisch anhand des deklarierten Variablentyps ausgewählt.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

Auf der JVM wird eine solche Erweiterung üblicherweise in eine statische Funktion
kompiliert, der das Objekt als erstes Argument übergeben wird.

**Kurz gesagt:** Erweiterungsfunktionen verbessern die Lesbarkeit einer API,
fügen der Klasse jedoch keine tatsächlichen Methoden hinzu und unterstützen kein
polymorphes Überschreiben.

</details>

<details>
<summary>6. Was ist eine data class in Kotlin?</summary>

#### Kotlin

Eine `data class` ist für Modelle vorgesehen, deren Hauptaufgabe darin besteht,
Daten zu speichern. Der Compiler generiert:

- `equals()` und `hashCode()`;
- `toString()`;
- `copy()`;
- `componentN()`-Funktionen für die Destrukturierung.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Wichtige Regeln:

- Der Primärkonstruktor muss mindestens einen Parameter enthalten, der als `val`
  oder `var` deklariert ist.
- Eine `data class` darf nicht `abstract`, `open`, `sealed` oder `inner` sein.
- Nur die Eigenschaften des Primärkonstruktors werden in den generierten Methoden
  berücksichtigt.
- `copy()` erstellt eine flache und keine tiefe Kopie.

Eine Eigenschaft im Klassenrumpf wird von `equals()`, `hashCode()` und `copy()`
nicht berücksichtigt:

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**Kurz gesagt:** Eine `data class` reduziert den wiederkehrenden Code für DTOs,
UI-Zustände und Wertobjekte. Komplexe Geschäftslogik sollte besser in
Domänendiensten oder gewöhnlichen Klassen untergebracht werden.

</details>

<details>
<summary>7. Was ist ein companion object?</summary>

#### Kotlin

Ein `companion object` ist ein einzelnes Objekt, das einer Klasse zugeordnet ist.
Seine Member können über den Klassennamen aufgerufen werden:

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

Es entspricht nicht dem Java-Schlüsselwort `static`: Ein Companion Object besitzt
einen eigenen Typ, kann einen Namen tragen, Schnittstellen implementieren und auf
`private`-Member der Klasse zugreifen. Eine Klasse kann nur ein Companion Object
enthalten.

Damit sich eine Funktion bequem aus Java aufrufen lässt, wird `@JvmStatic`
verwendet:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Ohne diese Annotation lautet der Java-Aufruf `Config.Companion.default()`, mit
der Annotation hingegen `Config.default()`.

**Kurz gesagt:** Ein Companion Object eignet sich für Factory-Methoden und
Konstanten, die mit einer Klasse verbunden sind. Veränderlicher globaler Zustand
sollte darin nicht gespeichert werden.

</details>

<details>
<summary>8. Wie werden Klassen in Kotlin erstellt?</summary>

#### Kotlin

Eine Klasse wird mit dem Schlüsselwort `class` deklariert. Der Primärkonstruktor
steht im Klassenkopf:

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

- `val` und `var` im Konstruktor erzeugen Klasseneigenschaften.
- `init` wird beim Erstellen eines Objekts ausgeführt und eignet sich zur Prüfung
  von Invarianten.
- Standardwerte machen überladene Konstruktoren häufig überflüssig.
- Ein Objekt wird ohne das Schlüsselwort `new` erstellt.

Klassen und ihre Methoden sind standardmäßig `final`. Für Vererbung ist `open`
erforderlich:

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**Kurz gesagt:** In Kotlin lassen sich Konstruktor, Eigenschaften und
Standardwerte direkt im Klassenkopf deklarieren; Vererbung muss ausdrücklich
zugelassen werden.

</details>

<details>
<summary>9. Erklären Sie Primär- und Sekundärkonstruktoren.</summary>

#### Kotlin

Der Primärkonstruktor wird im Klassenkopf deklariert:

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Seine Parameter werden nur dann zu Eigenschaften, wenn sie mit `val` oder `var`
deklariert sind. Der Rumpf des Primärkonstruktors wird mithilfe von `init`
definiert:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

Ein Sekundärkonstruktor wird im Klassenrumpf mit `constructor` deklariert. Ist ein
Primärkonstruktor vorhanden, muss der Sekundärkonstruktor seinen Aufruf über
`this(...)` an ihn delegieren:

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Eigenschaften und `init`-Blöcke werden in der Reihenfolge ihrer Deklaration
ausgeführt; der Rumpf des Sekundärkonstruktors folgt anschließend.

Besteht der einzige Unterschied in einem Standardwert, ist kein separater
Konstruktor erforderlich:

```kotlin
class User(val name: String, val id: Long = 0)
```

**Kurz gesagt:** Der Primärkonstruktor ist die bevorzugte Variante.
Sekundärkonstruktoren werden hauptsächlich für die Java-Kompatibilität oder
aufgrund von Framework-Anforderungen benötigt; meist genügen Standardparameter
oder eine Factory-Methode.

</details>

<details>
<summary>10. Wie funktioniert Vererbung in Kotlin?</summary>

#### Kotlin

Klassen und ihre Member sind in Kotlin standardmäßig `final`. Für Vererbung ist
`open` erforderlich, für das Überschreiben `override`:

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

Auch Eigenschaften können überschrieben werden:

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Wichtige Aspekte:

- Eine Klasse kann nur von einer Klasse erben, aber mehrere Schnittstellen
  implementieren.
- `abstract`-Member sind bereits offen und benötigen kein `open`.
- Ein überschriebener Member bleibt offen, sofern er nicht mit `final override`
  gekennzeichnet wird.
- Auf die Implementierung der Oberklasse wird über `super` zugegriffen.

**Kurz gesagt:** Vererbung muss in Kotlin ausdrücklich zugelassen werden. Für
Verträge sind Schnittstellen vorzuziehen; außerdem sollte Komposition in der
Regel vor dem Aufbau einer tiefen Klassenhierarchie in Betracht gezogen werden.

</details>

<details>
<summary>11. Wie implementiert man ein Singleton in Kotlin?</summary>

#### Kotlin

Die übliche Methode zur Implementierung eines Singletons in Kotlin ist eine
`object`-Deklaration:

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

Auf der JVM wird das Objekt beim ersten Zugriff erzeugt; seine Initialisierung ist
threadsicher. Ein `object` kann Eigenschaften, Funktionen und `init`-Blöcke
enthalten, von einer Klasse erben und Schnittstellen implementieren. Sein
Konstruktor kann nicht aufgerufen werden.

Ein `object` eignet sich für Konstanten, zustandslose Strategien und Elemente
einer Sealed-Hierarchie. Für Dienste mit Abhängigkeiten sollte dagegen ein
Singleton-Scope in einem DI-Container verwendet werden.

Veränderlicher Zustand in einem `object` ist global, erschwert Tests und erfordert
Synchronisierung. Unter Android dürfen darin weder eine `Activity` noch eine
`View` gespeichert werden, da dies zu einem Speicherleck führen kann.

**Kurz gesagt:** Ein `object` ist ein einfaches Singleton ohne
Konstruktorparameter. Für Geschäftsdienste mit Abhängigkeiten und einem
verwalteten Lebenszyklus ist Dependency Injection besser geeignet.

</details>

<details>
<summary>12. Wofür wird der Typ Unit verwendet?</summary>

#### Kotlin

`Unit` bedeutet, dass eine Funktion ohne relevantes Ergebnis beendet wird:

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

Bei einer gewöhnlichen Funktion kann die Angabe `: Unit` entfallen:

```kotlin
fun log(message: String) {
    println(message)
}
```

Im Gegensatz zu Java-`void` ist `Unit` ein vollwertiger Typ mit dem einzigen Wert
`Unit`. Er wird in Funktionstypen und generischen APIs verwendet:

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

`Unit` darf nicht mit `Nothing` verwechselt werden: `Unit` bezeichnet eine
reguläre Beendigung ohne Ergebnis, während eine Funktion mit `Nothing` niemals
regulär beendet wird:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**Kurz gesagt:** `Unit` ist der Typ für Funktionen ohne relevantes Ergebnis.
Explizit wird er üblicherweise in den Typen von Callback-Funktionen angegeben,
beispielsweise `() -> Unit`.

</details>

<details>
<summary>13. Was ist ein Smart Cast?</summary>

#### Kotlin

Ein Smart Cast ist eine automatische Umwandlung in einen spezifischeren Typ,
wenn der Compiler nachweisen kann, dass diese sicher ist:

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value hat den Typ String
    }
}
```

Dieser Mechanismus funktioniert nach Prüfungen mit `is`, `!is` und `null` und
berücksichtigt dabei den Kontrollfluss:

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value hat den Typ String
}
```

Ein Smart Cast ist nicht möglich, wenn sich ein Wert zwischen der Prüfung und
seiner Verwendung ändern kann. Typische Fälle sind eine veränderliche Eigenschaft
oder ein benutzerdefinierter Getter:

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

**Kurz gesagt:** Smart Casts machen ein explizites `as` nach einer Typ- oder
`null`-Prüfung überflüssig, jedoch nur dann, wenn der Compiler garantieren kann,
dass sich der geprüfte Wert nicht ändert.

</details>

<details>
<summary>14. Wie gewährleistet Kotlin Null-Sicherheit?</summary>

#### Kotlin

In Kotlin wird bereits im Typsystem zwischen nullable und nicht-nullbaren Typen
unterschieden:

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

Für die Arbeit mit nullable Werten werden folgende Operatoren verwendet:

```kotlin
val nullableLength: Int? = optionalName?.length // sicherer Aufruf
val length: Int = optionalName?.length ?: 0     // Elvis-Operator
```

Nach einer Prüfung auf `null` kann der Compiler einen Smart Cast durchführen:

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

Der Operator `!!` wandelt einen nullable Wert in einen nicht-nullbaren Wert um,
löst jedoch eine `NullPointerException` aus, wenn der Wert `null` ist:

```kotlin
val length = optionalName!!.length
```

Die größte Schwachstelle bilden Java-Plattformtypen: Ohne
Nullability-Annotationen weiß der Compiler nicht, ob eine Java-API `null`
zurückgeben kann. Solche Werte müssen an der Schnittstelle zwischen Java und
Kotlin geprüft werden.

**Kurz gesagt:** `T` lässt `null` nicht zu, `T?` hingegen schon. Verwende `?.`,
`?:` und entsprechende Prüfungen; `!!` sollte Fällen vorbehalten bleiben, in
denen die Nicht-Null-Garantie zweifelsfrei feststeht.

</details>

<details>
<summary>15. Was ist der Elvis-Operator (?:)?</summary>

#### Kotlin

Der Elvis-Operator `?:` gibt den linken Operanden zurück, wenn dieser nicht
`null` ist; andernfalls wird der rechte Operand ausgewertet und zurückgegeben:

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

Er wird häufig zusammen mit einem sicheren Aufruf verwendet:

```kotlin
val nameLength = user?.name?.length ?: 0
```

Da `return` und `throw` in Kotlin Ausdrücke sind, können sie ebenfalls auf der
rechten Seite stehen:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**Kurz gesagt:** `?:` definiert einen Ersatzwert für `null`. Fehler sollten damit
nicht verschleiert werden: Wenn `null` einen ungültigen Zustand bezeichnet, ist
es besser, die Ausführung ausdrücklich zu beenden.

</details>

<details>
<summary>16. Was bewirkt der Operator !! und warum ist er gefährlich?</summary>

#### Kotlin

Der Operator `!!` wandelt einen Wert vom Typ `T?` zwangsweise in `T` um. Ist der
Wert `null`, wird eine `NullPointerException` ausgelöst:

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Welche sicherere Alternative geeignet ist, hängt vom gewünschten Verhalten ab:

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

Auch `requireNotNull` und `checkNotNull` beenden die Ausführung mit einem Fehler,
beschreiben den verletzten Vertrag jedoch ausdrücklich. Eine häufige Verwendung
von `!!` weist üblicherweise auf ein fehlerhaftes Modell des nullable Zustands
oder auf Probleme mit dem Lebenszyklus hin.

**Kurz gesagt:** Verwende `!!` nur dann, wenn ein nicht-nullbarer Wert durch den
Vertrag garantiert ist, der Compiler dies jedoch nicht beweisen kann. In allen
anderen Fällen sollte `null` ausdrücklich behandelt werden.

</details>

<details>
<summary>17. Wie funktionieren Standardparameter und benannte Parameter?</summary>

#### Kotlin

Der Standardwert eines Parameters wird verwendet, wenn das entsprechende Argument
nicht übergeben wurde:

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Benannte Argumente verbessern die Lesbarkeit, insbesondere bei mehreren
Parametern desselben Typs oder mehreren `Boolean`-Parametern:

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java unterstützt weder Kotlin-Standardparameter noch benannte Argumente. Für eine
Java-API können mit `@JvmOverloads` überladene Varianten generiert werden:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Die Parameternamen einer öffentlichen Funktion sind Bestandteil der Kotlin-API:
Eine Änderung kann Aufrufe mit benannten Argumenten ungültig machen. Bei zu vielen
Parametern sollte besser eine eigene Konfigurationsklasse erstellt werden.

**Kurz gesagt:** Standardwerte ersetzen die meisten Überladungen, während
benannte Argumente einen Aufruf verständlicher machen. Für Java sollte
`@JvmOverloads` berücksichtigt werden.

</details>

<details>
<summary>18. Was sind Destrukturierungsdeklarationen?</summary>

#### Kotlin

Eine Destrukturierungsdeklaration zerlegt ein Objekt mithilfe der
`componentN()`-Funktionen in Variablen:

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

Der entsprechende Code lautet:

```kotlin
val id = user.component1()
val name = user.component2()
```

Bei einer `data class` werden diese Funktionen entsprechend der Reihenfolge der
Eigenschaften im Primärkonstruktor generiert. Destrukturierung funktioniert auch
in Schleifen und mit `Map.Entry`:

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Eine nicht benötigte Komponente kann mit `_` ausgelassen werden:

```kotlin
val (_, name) = user
```

Für eine gewöhnliche Klasse können eigene `operator fun componentN()`-Funktionen
definiert werden. Da die Destrukturierung jedoch von der Reihenfolge der
Komponenten abhängt, kann sie bei großen Modellen die Lesbarkeit beeinträchtigen.

**Kurz gesagt:** Destrukturierung ist eine komfortable Syntax auf Grundlage von
`componentN()` für kleine Modelle und Wertepaare. Die Reihenfolge der Komponenten
ist Bestandteil des Vertrags.

</details>

<details>
<summary>19. Was ist ein Typalias und wann sollte man ihn verwenden?</summary>

#### Kotlin

Ein `typealias` erstellt einen kürzeren oder verständlicheren Namen für einen
bereits vorhandenen Typ:

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Wichtig ist, dass ein Alias keinen neuen Typ erzeugt und keine zusätzliche
Typsicherheit bietet:

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Gültig, da beide Typen Long sind
```

Soll der Compiler zwischen den Werten unterscheiden, ist eine `value class`
erforderlich:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**Kurz gesagt:** Ein `typealias` verbessert die Lesbarkeit komplexer generischer
Typen und Funktionstypen. Für einen eigenständigen Domänentyp sollte eine
`value class` verwendet werden.

</details>

<details>
<summary>20. Worin besteht der Unterschied zwischen vararg und einem Array?</summary>

#### Kotlin

Ein Array ist ein Objekt mit einer festen Anzahl von Elementen. `vararg` ist ein
Funktionsparameter, der die Übergabe einer variablen Anzahl von Argumenten
ermöglicht:

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

Innerhalb der Funktion ist `names` ein Array. Ein vorhandenes Array wird mit dem
Spread-Operator `*` übergeben:

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

Folgt nach `vararg` ein weiterer Parameter, muss dieser mit seinem Namen übergeben
werden:

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

Eine Funktion darf nur einen `vararg`-Parameter besitzen. Für `vararg Int`
verwendet der Compiler ein `IntArray`. Der Spread-Operator kann eine Kopie des
Arrays erzeugen, was bei performancekritischem Code zu berücksichtigen ist.

**Kurz gesagt:** `Array<T>` ist eine Datenstruktur, `vararg` dagegen eine
Möglichkeit, eine beliebige Anzahl von Argumenten entgegenzunehmen. Ein Array wird
mit `*array` an einen `vararg`-Parameter übergeben.

</details>

<details>
<summary>21. Wie werden Lambda-Ausdrücke in Kotlin verwendet?</summary>

#### Kotlin

Ein Lambda ist eine anonyme Funktion, die in einer Variablen gespeichert, als
Argument übergeben und von einer Funktion zurückgegeben werden kann:

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

Der letzte Ausdruck bildet das Ergebnis des Lambdas. Besitzt es nur einen
Parameter, kann dieser mit `it` bezeichnet werden:

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

Ist das Lambda das letzte Argument, kann es außerhalb der runden Klammern stehen:

```kotlin
button.setOnClickListener { println("Clicked") }
```

Ein Lambda kann Variablen aus dem umgebenden Gültigkeitsbereich erfassen:

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

Existiert die benötigte Funktion bereits, kann eine Referenz mit `::` verwendet
werden:

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**Kurz gesagt:** Lambdas übergeben Verhalten an `map`, `filter`, Callbacks und
DSLs. Lange oder verschachtelte Lambdas sollten in benannte Funktionen
ausgelagert werden. Das Erfassen veränderlichen Zustands ist insbesondere in
nebenläufigem Code sorgfältig zu kontrollieren.

</details>

<details>
<summary>22. Wie funktionieren Collections (List, Set, Map) in Kotlin?</summary>

#### Kotlin

Die wichtigsten Kotlin-Collections sind:

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- Eine `List` bewahrt die Reihenfolge, erlaubt Duplikate und ermöglicht den
  Zugriff über einen Index.
- Ein `Set` enthält eindeutige Elemente; ihre Eindeutigkeit wird durch `equals()`
  und `hashCode()` bestimmt.
- Eine `Map` speichert Schlüssel-Wert-Paare mit eindeutigen Schlüsseln.
  `map[key]` gibt einen nullable Wert zurück; mit `containsKey()` lässt sich
  prüfen, ob ein Schlüssel vorhanden ist.

Collections besitzen schreibgeschützte Schnittstellen (`List`, `Set`, `Map`) und
veränderliche Schnittstellen (`MutableList`, `MutableSet`, `MutableMap`):

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Schreibgeschützt bedeutet nicht unveränderlich. Eine andere Referenz kann
dasselbe Objekt verändern:

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Operationen wie `filter`, `map` und `sorted` geben neue Collections zurück:

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Operationsketten auf einem `Iterable` können Zwischen-Collections erzeugen. Bei
langen Ketten sollte der Einsatz einer `Sequence` geprüft werden, jedoch nicht
ohne konkreten Bedarf erfolgen.

**Kurz gesagt:** Nach außen sollten vorzugsweise schreibgeschützte Schnittstellen
gegeben werden, während veränderliche Collections ein Implementierungsdetail
bleiben. Ist echte Unveränderlichkeit erforderlich, sollte eine Kopie oder eine
immutable Collection verwendet werden.

</details>

<details>
<summary>23. Wie durchläuft man Collections in Kotlin?</summary>

#### Kotlin

Für einen gewöhnlichen Durchlauf wird `for` verwendet:

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Für eine kurze Aktion mit jedem Element eignet sich `forEach`:

```kotlin
names.forEach { println(it) }
```

Wird der Index benötigt:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

Eine `Map` lässt sich bequem mithilfe von Destrukturierung durchlaufen:

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Für Transformationen oder Aggregationen wird der jeweils passende Operator
verwendet:

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Für `break`, `continue` und einen komplexen Kontrollfluss ist `for` besser
geeignet. Für Suchvorgänge sollten `firstOrNull`, `find`, `any` oder `none` statt
einer manuellen Schleife verwendet werden.

**Kurz gesagt:** `for` dient der Steuerung einer Schleife, `forEach` kurzen
Nebenwirkungen und `map` beziehungsweise `filter` der Erzeugung eines Ergebnisses.
Der Operator sollte entsprechend der Absicht des Codes gewählt werden.

</details>

<details>
<summary>24. Was geschieht, wenn man eine mit listOf() erstellte Liste verändert?</summary>

#### Kotlin

`listOf()` gibt eine `List<T>` zurück, also eine schreibgeschützte Schnittstelle
ohne die Methoden `add`, `remove` und `set`:

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Kompilierungsfehler
```

Eine `List` garantiert jedoch keine vollständige Unveränderlichkeit. Eine andere
veränderliche Referenz kann dieselbe Liste ändern:

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Das Ergebnis von `listOf()` in eine `MutableList` umzuwandeln, ist gefährlich:
Der Code hängt dabei von der konkreten Implementierung ab und kann zur Laufzeit
fehlschlagen.

Sind Änderungen erforderlich, sollte eine veränderliche Liste oder eine Kopie
erstellt werden:

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

Für einen unveränderlichen Programmierstil wird eine neue Liste erzeugt:

```kotlin
val updated = names + "Oksana"
```

**Kurz gesagt:** Eine mit `listOf()` erstellte Liste kann über die `List`-API
nicht verändert werden. Verwende für Änderungen `mutableListOf()` oder
`toMutableList()` und keinen unsicheren Cast.

</details>

<details>
<summary>25. Was geschieht beim Zugriff auf einen Listenindex außerhalb des gültigen Bereichs?</summary>

#### Kotlin

Ein Zugriff über `list[index]` ruft `get(index)` auf. Ist der Index negativ oder
größer beziehungsweise gleich `size`, wird eine `IndexOutOfBoundsException`
ausgelöst:

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Sichere Alternativen:

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

Für eine möglicherweise leere Liste stehen eigene sichere Funktionen zur
Verfügung:

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**Kurz gesagt:** Verwende `list[index]`, wenn die Gültigkeit des Index eine
Invariante darstellt. Für externe oder optionale Indizes sollten `getOrNull`,
`getOrElse` oder eine Prüfung mit `indices` verwendet werden.

</details>

<details>
<summary>26. Wie funktioniert die Interoperabilität zwischen Kotlin und Java?</summary>

#### Kotlin

Kotlin/JVM wird in JVM-Bytecode kompiliert. Daher können Kotlin und Java die
Klassen und Methoden der jeweils anderen Sprache direkt aufrufen:

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

Das größte Risiko bilden Plattformtypen aus Java, deren Nullability unbekannt
ist. An der Sprachgrenze sollten `@Nullable` und `@NotNull` verwendet, Werte
geprüft und Plattformtypen nicht an interne Anwendungsschichten weitergereicht
werden.

Für eine komfortable Java-API werden folgende Annotationen eingesetzt:

- `@JvmStatic` für eine statische Methode;
- `@JvmOverloads` für Überladungen bei Standardparametern;
- `@JvmField` für ein Feld ohne Getter und Setter;
- `@JvmName` für einen abweichenden JVM-Namen;
- `@Throws` zur Deklaration geprüfter Ausnahmen für Java.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Java-SAM-Schnittstellen können als Lambdas übergeben werden:

```kotlin
executor.execute { println("Done") }
```

**Kurz gesagt:** Interoperabilität ermöglicht eine schrittweise Migration
zwischen Java und Kotlin. Besondere Aufmerksamkeit erfordern Nullability,
veränderliche Collections, Standardparameter und eine für Java-Code komfortable
API.

</details>

<details>
<summary>27. Können Java-Annotationen in Kotlin verwendet werden?</summary>

#### Kotlin

Ja. Java-Annotationen können auf Kotlin-Klassen und deren Member angewendet
werden:

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

Eine Kotlin-Eigenschaft kann auf der JVM durch ein Feld, einen Getter, einen
Setter und einen Konstruktorparameter repräsentiert werden. Damit eine Annotation
an der gewünschten Stelle landet, wird ein Use-Site-Target verwendet:

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Die wichtigsten Varianten sind:

- `@field:` für das Backing Field;
- `@get:` beziehungsweise `@set:` für den Getter oder Setter;
- `@param:` für einen Konstruktorparameter;
- `@property:` für eine Kotlin-Eigenschaft, die kein eigenständiges Java-Element
  darstellt.

Java-Nullability-Annotationen helfen Kotlin dabei, den Ergebnistyp einer Java-API
zu bestimmen:

```java
@NotNull
String getName();
```

Ohne eine korrekte Annotation kann das Ergebnis ein Plattformtyp bleiben. Bei
Reflection- oder Validierungsframeworks muss geprüft werden, welches JVM-Element
sie auslesen.

**Kurz gesagt:** Java-Annotationen werden direkt unterstützt. Entscheidend ist,
für das jeweilige Framework das passende Target wie `@field:`, `@get:` oder
`@param:` auszuwählen.

</details>

<details>
<summary>28. Was ist die Kotlin-REPL?</summary>

#### Kotlin

REPL steht für Read-Eval-Print Loop: Die Umgebung liest den eingegebenen Code,
führt ihn aus, zeigt das Ergebnis an und wartet auf den nächsten Befehl.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

Die REPL eignet sich dazu, Syntax, Standardbibliotheksfunktionen und kleine
Ausdrücke schnell zu überprüfen, ohne ein Projekt anzulegen. Sie kann über die
Kotlin-CLI gestartet werden; für längere Experimente sind Scratch-Dateien oder
der Kotlin Playground komfortabler.

Die REPL bildet weder den Android-Lebenszyklus noch Dependency Injection, die
Gradle-Konfiguration oder eine reale nebenläufige Umgebung ab und ersetzt keine
automatisierten Tests.

**Kurz gesagt:** Die Kotlin-REPL ist eine interaktive Konsole für lokale
Experimente mit der Sprache. Ergebnisse, die für eine Anwendung relevant sind,
müssen durch Tests in der tatsächlichen Umgebung bestätigt werden.

</details>

<details>
<summary>29. Was ist ein Kotlin-Skript (.kts)?</summary>

#### Kotlin

Ein Kotlin-Skript ist eine `.kts`-Datei, in der Anweisungen auf oberster Ebene
ohne `fun main()` ausgeführt werden können:

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

Eine `.kt`-Datei ist eine gewöhnliche Quelldatei einer Anwendung oder Bibliothek,
während eine `.kts`-Datei von einer Skriptumgebung ausgeführt wird. Das
bekannteste Beispiel ist die Gradle Kotlin DSL:

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

Die Dateien `build.gradle.kts` und `settings.gradle.kts` werden von Gradle als
Konfigurationsskripte ausgeführt. Eigenständige Skripte können für kleinere
Automatisierungsaufgaben verwendet werden; ihr Abhängigkeitsmodell und ihre API
hängen jedoch vom jeweiligen Scripting Host ab.

**Kurz gesagt:** `.kts` eignet sich für Konfiguration und kurze
Automatisierungsaufgaben. Umfangreicher werdende Logik mit Abhängigkeiten und
Testbedarf sollte in ein gewöhnliches Kotlin-Modul oder eine CLI-Anwendung
ausgelagert werden.

</details>

<details>
<summary>30. Was ist der Kotlin Style Guide und warum sollte man ihn befolgen?</summary>

#### Kotlin

Der Kotlin Style Guide legt Regeln für Formatierung, Benennung, Dateistruktur und
die Organisation von Imports fest. Dadurch wird der Code eines Teams einheitlich
und vorhersehbar.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Praktische Vorteile:

- weniger Diskussionen über die Formatierung bei Code-Reviews;
- schnelleres Erfassen des Codes und leichtere Einarbeitung neuer Entwickler;
- automatische Überprüfung durch den IDE-Formatter, `ktlint` oder `detekt` in der
  CI-Pipeline.

Abweichungen eines Teams von den offiziellen Regeln sollten dokumentiert und in
IDE sowie CI einheitlich konfiguriert werden. Ein Style Guide regelt die
Gestaltung des Codes, ersetzt jedoch keine Architekturvorgaben.

**Kurz gesagt:** Ein Style Guide reduziert die kognitive Belastung und überträgt
die Prüfung der Formatierung von Menschen auf automatisierte Werkzeuge.

</details>

<details>
<summary>31. Was sind Kotlin-Idiome?</summary>

#### Kotlin

Kotlin-Idiome sind etablierte Vorgehensweisen, die Sprachfunktionen so einsetzen,
dass der Code seine Absicht klar ausdrückt.

Typische Beispiele:

- standardmäßig `val` und `var` nur für notwendige Veränderungen;
- nullable Typen sowie `?.` und `?:` anstelle von `!!`;
- `data class` für Datenmodelle;
- `sealed`-Typen für eine begrenzte Menge von Zuständen;
- `map`, `filter` und `associateBy` anstelle manueller Schleifen für
  Transformationen;
- Guard Clauses zur Verringerung der Verschachtelung.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Scope-Funktionen, Lambdas und Operatorüberladung sind nur dann sinnvoll, wenn sie
die Lesbarkeit verbessern. Der bloße Einsatz einer größeren Anzahl von
Sprachfunktionen macht Code noch nicht idiomatisch.

**Kurz gesagt:** Idiomatisches Kotlin ist sicherer und verständlicher Code, der
die Möglichkeiten der Sprache ihrem Zweck entsprechend nutzt und nicht bloß zur
Demonstration der Syntax.

</details>

<details>
<summary>32. Wie wird Kotlin in der Backend-Entwicklung eingesetzt und warum wird es Java vorgezogen?</summary>

#### Kotlin

Kotlin wird für JVM-Backends eingesetzt, darunter REST- und gRPC-APIs,
Microservices, Worker und ereignisgesteuerte Systeme. Es arbeitet mit Spring
Boot, Ktor und Java-Bibliotheken zusammen.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Die wichtigsten Vorteile gegenüber Java sind:

- Null-Sicherheit;
- weniger wiederkehrender Code durch `data class`, Standardparameter und
  Erweiterungsfunktionen;
- Koroutinen für nicht blockierende Ein- und Ausgabe;
- schrittweise Migration und uneingeschränkter Zugriff auf das Java-Ökosystem.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Zu den Kompromissen zählen eine langsamere Kompilierung, zusätzliche
Compiler- beziehungsweise Gradle-Konfiguration für manche Java-Frameworks und
die Notwendigkeit, das Team in idiomatischem Kotlin und strukturierter
Nebenläufigkeit zu schulen.

**Kurz gesagt:** Kotlin ist eine gute Wahl, wenn ein Team sichereren und
kompakteren JVM-Code wünscht, ohne auf das Java-Ökosystem zu verzichten, und
bereit ist, eine komplexere Toolchain in Kauf zu nehmen.

</details>

<details>
<summary>33. Was sind Funktionen höherer Ordnung (higher-order functions)?</summary>

#### Kotlin

Eine Funktion höherer Ordnung nimmt eine Funktion als Argument entgegen oder
gibt eine Funktion als Ergebnis zurück.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` bezeichnet eine Funktion ohne Parameter und ohne relevantes
Ergebnis. Ein Beispiel für die Rückgabe einer Funktion:

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

Typische Beispiele aus der Standardbibliothek:

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Solche Funktionen werden bei Collections, Callbacks und DSLs eingesetzt. Lambdas
können Objekte erzeugen und äußeren Zustand erfassen; bei kleinen Funktionen
höherer Ordnung beseitigt `inline` diesen Overhead häufig.

**Kurz gesagt:** Funktionen höherer Ordnung ermöglichen es, Verhalten als Wert
zu übergeben. Komplexe oder verschachtelte Lambdas sollten in benannte Funktionen
ausgelagert werden.

</details>

<details>
<summary>34. Was sind Inline-Funktionen und wann sollte man sie verwenden?</summary>

#### Kotlin

`inline` weist den Compiler an, den Funktionsrumpf und die Lambda-Parameter an der
Aufrufstelle einzusetzen. Das Hauptziel besteht darin, den Overhead von Funktionen
höherer Ordnung zu reduzieren:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` ermöglicht außerdem `reified`-Typparameter:

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Zusätzliche Modifikatoren für Lambda-Parameter:

- `noinline` verhindert das Inlining eines Lambdas, sodass es gespeichert oder
  weitergegeben werden kann;
- `crossinline` erlaubt das Inlining, untersagt jedoch ein nicht lokales `return`.

`inline` eignet sich für kleine Funktionen höherer Ordnung, `reified`-Generics
und nachweislich performancekritische Pfade. Bei großen Funktionen kann es den
Bytecode vergrößern. Änderungen an öffentlichen Inline-Funktionen erfordern zudem
besondere Aufmerksamkeit hinsichtlich der Binärkompatibilität.

**Kurz gesagt:** `inline` sollte nicht als universelle Optimierung eingesetzt
werden, sondern zur Verringerung des Lambda-Overheads, für `reified` oder für die
benötigte Semantik eines nicht lokalen `return`.

</details>

<details>
<summary>35. Was bedeutet das Schlüsselwort reified und wo wird es verwendet?</summary>

#### Kotlin

Aufgrund der Typlöschung kann ein gewöhnlicher generischer Parameter `T` weder
für eine Typprüfung noch zum Abrufen von `T::class` verwendet werden:

```kotlin
// fun <T> isOfType(value: Any) = value is T // Kompilierungsfehler
```

`reified` ermöglicht dies innerhalb einer `inline`-Funktion, da der Compiler den
konkreten Typ an der Aufrufstelle einsetzt:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

Auch Klasseninformationen können abgerufen werden:

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

Ein Beispiel aus der Standardbibliothek:

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

Einschränkung: `reified` ist nur für Typparameter von Inline-Funktionen verfügbar
und liefert keine Informationen über verschachtelte generische Argumente wie bei
`List<String>`. Für Nicht-Inline-APIs muss eine `KClass`, `Class` oder ein
Serializer ausdrücklich übergeben werden.

**Kurz gesagt:** `reified` gibt einer Inline-Funktion Zugriff auf den konkreten
Typ `T` für `is`, `T::class`, Reflection und ähnliche APIs, hebt die Typlöschung
jedoch nicht vollständig auf.

</details>

<details>
<summary>36. Was sind Sealed-Klassen und Sealed-Schnittstellen?</summary>

#### Kotlin

`sealed class` und `sealed interface` definieren eine kontrollierte Hierarchie
direkter Untertypen. Sie eignen sich für Zustände, Ergebnisse und Befehle:

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

Der Compiler prüft, ob ein als Ausdruck verwendetes `when` sämtliche Varianten
behandelt:

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Direkte Untertypen müssen im selben Modul und Paket deklariert werden. Eine
`sealed class` kann einen Konstruktor, Zustand und eine gemeinsame Implementierung
besitzen. Ein `sealed interface` hat keinen Konstruktor, dafür kann eine Klasse
mehrere Schnittstellen implementieren.

Anders als bei einem `enum` kann jede Sealed-Variante einen eigenen Datensatz
besitzen. Soll die Hierarchie durch externe Module erweitert werden, ist `sealed`
ungeeignet. Ein `else`-Zweig sollte einem `when` nicht unnötig hinzugefügt werden,
damit der Compiler neue Fälle erkennen kann.

**Kurz gesagt:** Sealed-Typen modellieren eine geschlossene Menge von Varianten
mit unterschiedlichen Daten und ermöglichen eine Prüfung der Vollständigkeit von
`when` zur Kompilierzeit.

</details>

<details>
<summary>37. Was ist ein Objektausdruck und wann wird er verwendet?</summary>

#### Kotlin

Ein `object expression` erzeugt zur Laufzeit an der jeweiligen Stelle ein
anonymes Objekt. Üblicherweise wird er für die einmalige Implementierung einer
Schnittstelle oder Klasse verwendet:

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

Auch von einer Klasse kann geerbt werden:

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

Ein anonymes Objekt ohne Supertyp eignet sich nur für die lokale Verwendung oder
eine `private`-API:

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Sein anonymer Typ kann nicht als stabiler öffentlicher Typ exportiert werden:
Nach außen ist lediglich der deklarierte Supertyp oder `Any` sichtbar. Ein
Objektausdruck wird bei jeder Auswertung erneut erzeugt, während eine
Objektdeklaration ein benanntes Singleton darstellt.

Ein solches Objekt kann äußere Variablen erfassen. Wächst die Implementierung oder
wird sie wiederholt verwendet, sollte sie in eine benannte Klasse ausgelagert
werden.

**Kurz gesagt:** Ein Objektausdruck eignet sich für eine kurze lokale
Implementierung, einen Listener oder einen Test-Stub. Öffentliche oder
wiederverwendbare Logik benötigt einen benannten Typ.

</details>

<details>
<summary>38. Worin besteht der Unterschied zwischen Enum-Klassen und Sealed-Klassen?</summary>

#### Kotlin

Eine `enum class` enthält eine feste Menge von Singleton-Werten desselben Typs:

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

Eine `sealed class` oder ein `sealed interface` definiert eine geschlossene
Hierarchie von Untertypen. Jede Variante kann eigene Daten besitzen:

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

Bei beiden Varianten prüft der Compiler die Vollständigkeit eines `when`:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

Ein `enum` verfügt standardmäßig über `entries`, `name` und `ordinal`. Ein
Sealed-Typ besitzt keine fertige Liste seiner Instanzen, erlaubt dafür aber die
Erzeugung mehrerer Objekte desselben Untertyps mit unterschiedlichen Daten.

**Kurz gesagt:** Ein `enum` eignet sich für einfache Konstanten mit gemeinsamer
Struktur. Ein Sealed-Typ eignet sich für eine geschlossene Menge von Zuständen
oder Ergebnissen mit unterschiedlichen Daten.

</details>

<details>
<summary>39. Worin besteht der Unterschied zwischen lateinit und lazy Initialisierung?</summary>

#### Kotlin

Beide Mechanismen verzögern die Initialisierung, funktionieren jedoch
unterschiedlich.

Mit `lateinit` kann einer nicht-nullbaren `var`-Eigenschaft erst nach der
Erstellung des Objekts ein Wert zugewiesen werden:

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Ein Lesezugriff vor der Zuweisung löst eine
`UninitializedPropertyAccessException` aus. `lateinit` funktioniert nur mit einer
nicht-nullbaren `var`-Eigenschaft eines Referenztyps, bietet keine
Threadsicherheitsgarantie und erlaubt wiederholte Zuweisungen.

Der Initialisierungszustand kann geprüft werden:

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` berechnet den Wert einer `val`-Eigenschaft beim ersten Lesezugriff und
speichert das erfolgreich berechnete Ergebnis:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

Standardmäßig ist `lazy` synchronisiert. Die Modi `PUBLICATION` und `NONE` ändern
diese Garantien; `NONE` ist nur beim Zugriff aus einem einzigen Thread sicher.

- Wird der Wert später durch externen Code oder Dependency Injection zugewiesen,
  eignet sich `lateinit`.
- Soll ein eigener Initialisierer erst bei Bedarf ausgeführt werden, eignet sich
  `lazy`.
- Ist das Fehlen eines Werts ein gültiger Zustand, sollte ein nullable Typ
  verwendet werden.

**Kurz gesagt:** `lateinit` dient der verzögerten externen Zuweisung einer
veränderlichen Eigenschaft; `lazy` einer einmaligen verzögerten Berechnung einer
unveränderlichen Eigenschaft.

</details>

<details>
<summary>40. Was ist der Typ Nothing und wie beeinflusst er den Kontrollfluss?</summary>

#### Kotlin

`Nothing` ist ein Typ ohne einen einzigen möglichen Wert. Eine Funktion mit
diesem Ergebnistyp wird niemals regulär beendet:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` ist ein Untertyp aller Typen und kann daher in jedem Ausdruck verwendet
werden, bei dem die Ausführung abgebrochen wird:

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` und `return` besitzen den Typ `Nothing` und können deshalb Bestandteil
eines Elvis-Ausdrucks sein:

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Der Typ unterstützt den Compiler bei der Analyse des Kontrollflusses und bei
Smart Casts:

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` besitzt mit `null` genau einen möglichen Wert; ohne weiteren Kontext
ist dies der Typ des Ausdrucks `null`. Auch die Standardfunktionen `error()` und
`TODO()` geben `Nothing` zurück.

**Kurz gesagt:** `Nothing` kennzeichnet einen Ausführungspfad, der die Kontrolle
nicht zurückgibt. Dadurch kann der Compiler Typen und den anschließenden
Kontrollfluss präziser bestimmen.

</details>

<details>
<summary>41. Kann Java Kotlin-Funktionen mit Standardparametern aufrufen?</summary>

#### Kotlin

Java unterstützt Kotlin-Standardparameter nicht. Ohne zusätzliche Überladungen
müssen alle Argumente übergeben werden:

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

`@JvmOverloads` generiert Java-freundliche Überladungen:

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

Die Überladungen werden von rechts nach links für aufeinanderfolgende Parameter
mit Standardwerten erzeugt. Die Annotation funktioniert auch bei Konstruktoren:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Für eine reine Kotlin-API ist die Annotation nicht erforderlich. Eine große Zahl
von Parametern erzeugt zu viele Überladungen; in diesem Fall sind ein
Konfigurationsobjekt oder ein Builder besser geeignet.

**Kurz gesagt:** Ohne `@JvmOverloads` kann Java nur die vollständige Signatur
aufrufen. Die Annotation sollte nur APIs hinzugefügt werden, die tatsächlich aus
Java verwendet werden.

</details>

<details>
<summary>42. Was sind endrekursive Funktionen?</summary>

#### Kotlin

Eine endrekursive Funktion ruft sich selbst als letzte Operation auf. Mit dem
Modifikator `tailrec` kann der Compiler einen solchen Aufruf in eine Schleife
umwandeln, ohne dass der Stack wächst:

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

Die folgende Variante ist nicht endrekursiv, weil nach dem rekursiven Aufruf noch
eine Multiplikation ausgeführt wird:

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

Die Optimierung funktioniert nur bei direkter Rekursion und nicht, wenn der
Aufruf in einer Konstruktion steht, die die Umwandlung verhindert, beispielsweise
in `try`/`finally`. Wird `tailrec` falsch eingesetzt, warnt der Compiler, dass
keine Optimierung erfolgt.

**Kurz gesagt:** `tailrec` ermöglicht gut lesbare Rekursion ohne das Risiko eines
`StackOverflowError`. Ist eine gewöhnliche Schleife einfacher, sollte sie
bevorzugt werden.

</details>

<details>
<summary>43. Was sind Scope-Funktionen (let, run, apply, also, with) und wann werden sie verwendet?</summary>

#### Kotlin

Scope-Funktionen führen einen Block im Kontext eines Objekts aus. Sie
unterscheiden sich in zwei Punkten:

- ob das Objekt als `this` oder `it` verfügbar ist;
- ob das Ergebnis des Lambdas oder das Objekt selbst zurückgegeben wird.

| Funktion | Objekt verfügbar als | Rückgabewert       |
| -------- | -------------------- | ------------------ |
| `let`    | `it`                 | Ergebnis des Lambdas |
| `run`    | `this`               | Ergebnis des Lambdas |
| `apply`  | `this`               | das Objekt selbst  |
| `also`   | `it`                 | das Objekt selbst  |
| `with`   | `this`               | Ergebnis des Lambdas |

Typische Verwendungszwecke:

- `let` für Transformationen oder einen Block nach einem sicheren Aufruf;
- `run` zur Berechnung eines Ergebnisses mit Zugriff über `this`;
- `apply` zur Konfiguration eines Objekts;
- `also` für eine Nebenwirkung, ohne die Aufrufkette zu verändern;
- `with` zur Gruppierung von Operationen auf einem übergebenen Objekt.

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

`with` ist keine Erweiterungsfunktion: Das Objekt wird als erstes Argument
übergeben. Tief verschachtelte Scope-Funktionen und eine Vermischung von `this`
und `it` beeinträchtigen die Lesbarkeit; in diesem Fall sind gewöhnlicher Code
oder eine benannte Funktion besser geeignet.

**Kurz gesagt:** Die Scope-Funktion sollte anhand des Empfängers (`this`/`it`)
und des Rückgabewerts (Objekt/Lambda-Ergebnis) gewählt werden, nicht aus
Gewohnheit.

</details>

<details>
<summary>44. Worin besteht der Unterschied zwischen let, run und apply?</summary>

#### Kotlin

Die Funktionen unterscheiden sich darin, wie auf das Objekt zugegriffen wird und
was sie zurückgeben:

| Funktion | Empfänger im Block | Rückgabewert       |
| -------- | ------------------ | ------------------ |
| `let`    | `it`               | Ergebnis des Lambdas |
| `run`    | `this`             | Ergebnis des Lambdas |
| `apply`  | `this`             | das Objekt selbst  |

`let` wird für Transformationen oder die Ausführung nach einem sicheren Aufruf
verwendet:

```kotlin
val length = name?.let { it.length }
```

`run` dient der Berechnung eines Ergebnisses mit mehreren Zugriffen auf das
Objekt:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`apply` dient der Konfiguration und gibt dasselbe Objekt zurück:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

Eine Scope-Funktion sollte nicht verwendet werden, wenn ein gewöhnlicher Aufruf
verständlicher ist. Verschachtelte Blöcke mit mehreren `this` oder `it` sollten
durch eine benannte Funktion ersetzt werden.

**Kurz gesagt:** `let` verwendet `it` und gibt ein neues Ergebnis zurück; `run`
verwendet `this` und gibt ein neues Ergebnis zurück; `apply` verwendet `this` und
gibt das ursprüngliche Objekt zurück.

</details>

<details>
<summary>45. Worin besteht der Unterschied zwischen map und flatMap?</summary>

#### Kotlin

`map` wandelt jedes Element in genau ein Ergebnis um:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

Gibt die Transformation eine Collection zurück, erzeugt `map` eine verschachtelte
Struktur:

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` führt die Transformation aus und vereinigt die verschachtelten
Collections zu einer einzigen:

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

Ein praktisches Beispiel:

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Konzeptionell sind diese Ausdrücke gleichwertig:

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**Kurz gesagt:** `map` entspricht `A -> B`. `flatMap` entspricht
`A -> Iterable<B>` mit anschließender Zusammenführung aller Ergebnisse zu einer
flachen Collection.

</details>

<details>
<summary>46. Was ist eine Sequence und wann sollte man sie verwenden?</summary>

#### Kotlin

Eine `Sequence` ist eine lazy ausgewertete synchrone Folge. Zwischenoperatoren
verarbeiten die Daten nicht sofort, sondern bilden eine Pipeline:

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Die Berechnung wird durch eine Terminaloperation wie `toList`, `first`, `count`,
`fold` oder `any` ausgelöst. Die Elemente durchlaufen die Pipeline einzeln,
wodurch Operationen mit vorzeitigem Abbruch die Verarbeitung früher beenden
können:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Eine `Sequence` eignet sich für große Datenmengen, lange `map`-/`filter`-Ketten
und Operationen mit vorzeitigem Abbruch. Bei einer kleinen Collection oder einer
einzelnen Transformation kann ihr Overhead den Nutzen übersteigen. Operationen
wie `sorted` müssen weiterhin alle Elemente sammeln.

Eine `Sequence` ist weder asynchron noch unterstützt sie Suspension. Für einen
asynchronen Datenstrom wird `Flow` verwendet.

**Kurz gesagt:** Eine `Sequence` reduziert Zwischen-Collections und unnötige
Arbeit in langen synchronen Pipelines, ist aber nicht automatisch schneller als
gewöhnliche Collections.

</details>

<details>
<summary>47. Was bedeutet die lazy Verarbeitung von Collections?</summary>

#### Kotlin

Lazy Verarbeitung bedeutet, dass eine Pipeline zunächst nur beschrieben und erst
durch eine Terminaloperation ausgeführt wird. In Kotlin wird dafür `Sequence`
verwendet:

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Die Elemente durchlaufen die Operatoren üblicherweise einzeln. Dadurch kann die
Verarbeitung früher beendet werden:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Eine lazy Pipeline speichert das Ergebnis nicht zwischen: Eine erneute
Terminaloperation startet die Berechnung in der Regel von vorn. Auch potenziell
unendliche Folgen lassen sich verarbeiten, sofern sie mit `take` begrenzt werden:

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Lazy Operatoren können eigenen Overhead verursachen; `sorted` und ähnliche
Operationen sammeln die Daten weiterhin vollständig. Für asynchrone Verarbeitung
wird `Flow` statt `Sequence` benötigt.

**Kurz gesagt:** Lazy Verarbeitung verschiebt die Arbeit bis zu einer
Terminaloperation und kann unnötige Berechnungen vermeiden. Sie speichert das
Ergebnis nicht zwischen und ist nicht immer schneller als eager ausgewertete
Collections.

</details>

<details>
<summary>48. Was sind Koroutinen und wie unterscheiden sie sich von Threads?</summary>

#### Kotlin

Eine Koroutine ist eine leichtgewichtige Aufgabe, die ihre Ausführung aussetzen
und später fortsetzen kann. Für eine Koroutine wird nicht jeweils ein eigener
Thread des Betriebssystems erstellt:

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Ein Thread ist eine Ressource des Betriebssystems mit eigenem Stack. Viele
Koroutinen können auf einer kleinen Anzahl von Threads ausgeführt werden und nach
einer Suspension zwischen ihnen wechseln.

```kotlin
delay(1_000)        // Setzt die Koroutine aus
Thread.sleep(1_000) // Blockiert den Thread
```

`suspend` allein garantiert keine nicht blockierende Implementierung. Eine
blockierende API blockiert weiterhin den Thread und muss in einem geeigneten
Kontext ausgeführt werden.

Der Dispatcher bestimmt die ausführenden Threads:

- `Dispatchers.Main` für die Benutzeroberfläche;
- `Dispatchers.IO` für blockierende Ein- und Ausgabe;
- `Dispatchers.Default` für CPU-intensive Arbeit.

Strukturierte Nebenläufigkeit bindet Koroutinen an einen `CoroutineScope`: Die
übergeordnete Operation verwaltet Lebenszyklus, Fehler und Abbruch der
untergeordneten Aufgaben.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Koroutinen verhindern keine Race Conditions und beschleunigen CPU-intensive
Arbeit nicht automatisch. Für aufwendige Berechnungen muss der passende
Dispatcher ausdrücklich gewählt werden:

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**Kurz gesagt:** Koroutinen sind Aufgaben auf Threads mit Suspension und
strukturierter Nebenläufigkeit. Sie sind günstiger als das Modell „eine Aufgabe –
ein Thread“, ersetzen jedoch nicht den korrekten Umgang mit Dispatchern und
gemeinsamem Zustand.

</details>

<details>
<summary>49. Was ist eine suspend-Funktion?</summary>

#### Kotlin

`suspend` kennzeichnet eine Funktion, die Aussetzungspunkte enthalten kann. Sie
wird aus einer anderen Suspend-Funktion oder aus einer Koroutine aufgerufen:

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` erstellt keine Koroutine, führt die Funktion nicht parallel aus und
macht blockierenden Code nicht automatisch nicht blockierend:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

Der Compiler wandelt eine Suspend-Funktion in eine Zustandsmaschine mit einer
`Continuation` um, damit ihr Zustand gespeichert und die Ausführung nach einer
Suspension fortgesetzt werden kann.

Der Abbruch erfolgt kooperativ: Die üblichen Aussetzungspunkte prüfen ihn,
während lange CPU-Schleifen dies ausdrücklich tun müssen:

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**Kurz gesagt:** `suspend` ermöglicht einer Funktion, eine Koroutine auszusetzen
und fortzusetzen. Es garantiert keine nicht blockierende Implementierung und
erstellt selbst keine neue Koroutine.

</details>

<details>
<summary>50. Was ist ein CoroutineScope und wofür wird er benötigt?</summary>

#### Kotlin

Ein `CoroutineScope` ist der Besitzer von Koroutinen und enthält einen
`coroutineContext`. Dieser Kontext umfasst üblicherweise einen `Job`, einen
Dispatcher und weitere Elemente:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

Ein `Job` stellt Eltern-Kind-Beziehungen her und verwaltet den Abbruch. Wird der
Scope abgebrochen, werden auch seine untergeordneten Koroutinen abgebrochen:

```kotlin
scope.cancel()
```

Für parallele Arbeit innerhalb einer Suspend-Funktion wird ein Scope-Builder
verwendet:

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` wird erst beendet, nachdem alle untergeordneten Aufgaben
abgeschlossen sind. Unter Android besitzen `viewModelScope`, `lifecycleScope` und
`rememberCoroutineScope()` bereits einen definierten Lebenszyklus.

Ein eigener Scope sollte nur für ein Objekt mit einer eindeutigen
Beendigungsmethode erstellt werden, die `cancel()` aufruft. `GlobalScope` besitzt
keinen solchen Eigentümer und verletzt die strukturierte Nebenläufigkeit; deshalb
sollte er im Anwendungscode vermieden werden.

**Kurz gesagt:** Ein `CoroutineScope` bindet Koroutinen an einen verständlichen
Lebenszyklus und ermöglicht ihren gemeinsamen Abbruch. Jeder Scope sollte einen
Eigentümer besitzen.

</details>

<details>
<summary>51. Worin besteht der Unterschied zwischen launch und async?</summary>

#### Kotlin

Beide Builder starten eine untergeordnete Koroutine in einem `CoroutineScope`.

`launch` gibt einen `Job` zurück und wird verwendet, wenn kein eigenständiges
Ergebnis benötigt wird:

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` gibt ein `Deferred<T>` zurück. Das Ergebnis oder der Fehler wird über
`await()` abgerufen. Ein typisches Szenario ist die parallele Ausführung
unabhängiger Operationen:

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

Standardmäßig startet `async` sofort und nicht erst bei `await()`. Der Fehler
einer untergeordneten Koroutine bricht den gewöhnlichen übergeordneten Scope
unabhängig vom verwendeten Builder ab; bei `async` löst `await()` denselben Fehler
zusätzlich erneut aus. Supervision muss separat konfiguriert werden.

`async` sollte nicht verwendet werden, wenn kein Aufruf von `await()` vorgesehen
ist. Ebenso sollten voneinander abhängige Operationen nicht parallel gestartet
werden.

**Kurz gesagt:** `launch` startet eine Aufgabe ohne Ergebniswert; `async` startet
eine Aufgabe mit einem `Deferred<T>`, deren Ergebnis über `await()` benötigt wird.

</details>

<details>
<summary>52. Was ist runBlocking und wann wird es verwendet?</summary>

#### Kotlin

`runBlocking` erstellt einen `CoroutineScope` und blockiert den aktuellen Thread,
bis der Block und alle seine untergeordneten Koroutinen abgeschlossen sind:

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

Es ist ein Werkzeug an der Grenze zwischen synchronen APIs und Suspend-Code. Es
kann bei einer Legacy-Anbindung oder in einem kurzen Beispiel sinnvoll sein. In
Koroutinentests sollte `runTest` verwendet werden.

Im UI-Code blockiert `runBlocking` den Hauptthread und kann zum Einfrieren, zu
einem ANR oder zu einem Deadlock führen:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Stattdessen wird eine Koroutine in einem Scope mit passendem Lebenszyklus
gestartet:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

Innerhalb einer Suspend-Funktion ist `runBlocking` nicht erforderlich und
blockiert lediglich den Thread:

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**Kurz gesagt:** `runBlocking` ist eine synchrone Brücke zu Suspend-Code. Es
sollte nicht innerhalb von Koroutinen, Suspend-Funktionen oder im UI-Thread
verwendet werden.

</details>

<details>
<summary>53. Was sind Dispatcher (Dispatchers.IO, Default, Main)?</summary>

#### Kotlin

Ein `CoroutineDispatcher` plant die Ausführung einer Koroutine auf den passenden
Threads:

- `Dispatchers.Main` für die Benutzeroberfläche und kurze Operationen im
  Hauptthread;
- `Dispatchers.IO` für blockierende Ein- und Ausgabe;
- `Dispatchers.Default` für CPU-intensive Berechnungen.

Eine blockierende Dateioperation wird auf `IO` verlagert:

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Eine aufwendige Berechnung wird auf `Default` ausgeführt:

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` setzt die aktuelle Koroutine aus, führt den Block im angegebenen
Kontext aus und gibt das Ergebnis zurück. Dabei bleiben strukturierte
Nebenläufigkeit, Abbruch und Fehlerweitergabe erhalten.

Nicht jeder Netzwerk- oder Datenbankaufruf benötigt `Dispatchers.IO`: Eine
Suspend-API kann bereits für den Hauptthread geeignet sein. Der Dispatcher sollte
von der Schicht gewählt werden, die weiß, ob die Implementierung einen Thread
blockiert. Für eine bessere Testbarkeit kann er als Abhängigkeit übergeben werden:

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

Für kleine Aufgaben sollte der Kontext nicht gewechselt werden, da auch dieser
Wechsel Kosten verursacht.

**Kurz gesagt:** `Main` dient der UI, `IO` blockierender Ein- und Ausgabe und
`Default` aufwendigen Berechnungen. `withContext` wechselt den Kontext für eine
bestimmte Operation, ohne eine neue unabhängige Koroutine zu erstellen.

</details>

<details>
<summary>54. Wie werden Koroutinen abgebrochen und Fehler behandelt?</summary>

#### Kotlin

Der Abbruch von Koroutinen erfolgt kooperativ und wird über einen `Job` gesteuert:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Suspend-Funktionen wie `delay()` reagieren automatisch auf einen Abbruch.
CPU-Code muss seinen Zustand regelmäßig prüfen:

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

Ein Abbruch wird durch eine `CancellationException` signalisiert. Wird sie von
einem allgemeinen `catch` abgefangen, muss sie erneut ausgelöst werden:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

Bei gewöhnlicher strukturierter Nebenläufigkeit bricht ein unbehandelter Fehler
einer untergeordneten Koroutine den übergeordneten Scope und die benachbarten
Aufgaben ab. Erwartete Fehler werden lokal behandelt und in ein Domänenergebnis
oder einen UI-Zustand umgewandelt:

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

Für unabhängige Aufgaben werden `supervisorScope` oder `SupervisorJob` verwendet:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

Ein Supervisor behandelt den Fehler nicht, sondern isoliert lediglich die
benachbarten Aufgaben. Ein `CoroutineExceptionHandler` ist für die letzte Ebene
der Behandlung unbehandelter Ausnahmen in einem Root-`launch` vorgesehen,
beispielsweise zur Protokollierung. Fehler aus `async` werden über `await()`
abgerufen.

**Kurz gesagt:** Brich über einen `Job` ab, verschlucke keine
`CancellationException`, behandle erwartete Fehler lokal und isoliere unabhängige
Aufgaben durch Supervision.

</details>

<details>
<summary>55. Was ist strukturierte Nebenläufigkeit (structured concurrency)?</summary>

#### Kotlin

Strukturierte Nebenläufigkeit bedeutet, dass jede Koroutine einen Eigentümer
besitzt und zu einer `Job`-Hierarchie gehört. Der übergeordnete Job wartet auf die
untergeordneten Aufgaben und verwaltet deren Abbruch sowie Fehler.

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

`coroutineScope` wird nicht vor seinen untergeordneten Koroutinen beendet. Ein
Abbruch durch den Aufrufer bricht auch sie ab; ein unbehandelter Fehler einer
Aufgabe bricht den Scope und die benachbarten Aufgaben ab.

Für unabhängige Aufgaben wird Supervision verwendet:

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

Der Fehler eines direkten Kindes bricht die übrigen untergeordneten Aufgaben
nicht ab, muss aber weiterhin behandelt werden. `supervisorScope` wird lokal,
`SupervisorJob` dagegen in einem langlebigen Scope eingesetzt.

`GlobalScope` verletzt dieses Modell: Der Aufrufer kann nicht zuverlässig auf die
Arbeit warten, sie abbrechen oder ihren Fehler erhalten. Ein eigener Scope muss
einen klar definierten Lebenszyklus und eine ausdrückliche Bereinigung besitzen.

**Kurz gesagt:** Strukturierte Nebenläufigkeit verhindert, dass untergeordnete
Arbeit ihren Eigentümer unbemerkt überlebt. Zusammengehörige Aufgaben verwenden
`coroutineScope`, unabhängige Aufgaben Supervision.

</details>

<details>
<summary>56. Können Koroutinen in beliebigen Threads ausgeführt werden?</summary>

#### Kotlin

Eine Koroutine ist nicht an einen eigenen Thread gebunden. Der
`CoroutineDispatcher` bestimmt, wo ihre einzelnen Abschnitte ausgeführt werden:

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

- `Main` dient der Benutzeroberfläche;
- `IO` blockierender Ein- und Ausgabe;
- `Default` CPU-intensiver Arbeit.

Bei einem Dispatcher mit Thread-Pool kann eine Koroutine nach einer Suspension
in einem anderen Thread desselben Dispatchers fortgesetzt werden:

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` wechselt den Kontext vorübergehend und stellt nach Abschluss den
Kontext des Aufrufers wieder her. `Dispatchers.Main` garantiert die Ausführung im
UI-Thread; Pool-Dispatcher garantieren keine bestimmte Thread-ID.

Ein gewöhnliches `ThreadLocal` wechselt nicht zusammen mit der Koroutine zwischen
Threads. Bei Bedarf wird sein Wert dem Koroutinenkontext hinzugefügt:

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

Ein benutzerdefinierter Dispatcher mit eigenen Threads benötigt einen Eigentümer,
der ihn schließt. Ein blockierender Aufruf blockiert den Thread unabhängig davon,
dass er innerhalb einer Koroutine ausgeführt wird.

**Kurz gesagt:** Eine Koroutine wird auf den Threads ihres Dispatchers ausgeführt
und kann nach einer Suspension den physischen Thread wechseln. Die Bindung an
einen bestimmten Thread muss der Dispatcher garantieren.

</details>

<details>
<summary>57. Was ist Flow in Kotlin?</summary>

#### Kotlin

`Flow<T>` ist ein auf Koroutinen basierender asynchroner Strom von Werten. Eine
Suspend-Funktion gibt einen einzelnen Wert zurück, ein `Flow` dagegen eine Folge
von Werten im Zeitverlauf:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Ein Flow wird mit einem Builder erstellt und durch eine Terminaloperation wie
`collect` gestartet:

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

Ein mit `flow {}` erzeugter Flow ist kalt: Der Produzent wird für jeden Collector
separat gestartet. `emit()` ist eine Suspend-Funktion, weshalb der Produzent einen
langsamen Collector ohne ausdrückliche Pufferung nicht überholt.

Operatoren bilden eine Pipeline:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` ändert den Kontext der Upstream-Operatoren:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` behandelt ausschließlich Upstream-Fehler; Fehler des Collectors werden
davon nicht abgefangen. Wird der Collector abgebrochen, werden im Rahmen der
strukturierten Nebenläufigkeit auch das Sammeln und der Produzent abgebrochen.

**Kurz gesagt:** Verwende eine Suspend-Funktion für ein einzelnes Ergebnis und
`Flow` für Werte, die im Zeitverlauf eintreffen und eine asynchrone Pipeline
benötigen.

</details>

<details>
<summary>58. Worin besteht der Unterschied zwischen Flow, StateFlow und SharedFlow?</summary>

#### Kotlin

```text
Flow       -> üblicherweise eine kalte Pipeline
StateFlow  -> heißer Stream mit dem aktuellen Zustand
SharedFlow -> heißer Broadcast mit Replay- und Pufferrichtlinie
```

Ein mit `flow {}` erstellter `Flow` startet den Produzenten für jeden Collector
separat und speichert keinen aktuellen Wert:

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

Ein `StateFlow` besitzt stets einen Anfangswert. Ein neuer Collector erhält sofort
den aktuellen Zustand; gemäß `equals()` identische Werte werden nicht erneut
emittiert:

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

Die veränderliche Variante bleibt `private`; für nebenläufige Aktualisierungen
wird `update { }` verwendet.

Ein `SharedFlow` sendet Werte an alle aktiven Collectors und muss keinen aktuellen
Wert besitzen:

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` legt fest, wie viele der letzten Werte ein neuer Collector erhält;
`extraBufferCapacity` und `onBufferOverflow` steuern den Puffer. Bei `replay = 0`
geht ein Wert ohne Subscriber verloren. Kritische Daten sollten deshalb als
Zustand und nicht als einmaliges Ereignis modelliert werden.

Ein kalter Flow kann in einen gemeinsam genutzten heißen Flow umgewandelt werden:
`stateIn()` erstellt einen `StateFlow`, `shareIn()` einen `SharedFlow`.
`SharingStarted` bestimmt, wann der gemeinsame Upstream gestartet und beendet
wird.

**Kurz gesagt:** `Flow` ist eine unabhängige lazy Pipeline, `StateFlow` bildet den
aktuellen Zustand ab und `SharedFlow` einen gemeinsamen Ereignisstrom mit
konfigurierbarem Replay.

</details>

<details>
<summary>59. Worin besteht der Unterschied zwischen Flow und LiveData?</summary>

#### Kotlin

`LiveData` ist ein Android-spezifischer, lebenszyklusbewusster Datenhalter,
während `Flow` ein von Android unabhängiger Koroutinenstrom ist.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` benachrichtigt nur aktive Observer und liefert nach deren Aktivierung
den letzten Wert erneut. Es eignet sich für eine ältere View-basierte UI, sollte
jedoch nicht in die Domänenschicht gelangen.

`Flow` unterstützt den Abbruch von Koroutinen, Fehler und eine große Auswahl an
Operatoren:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

Ein gewöhnlicher `Flow` ist überwiegend kalt und speichert keinen aktuellen Wert.
Für den UI-Zustand wird `StateFlow` verwendet. Der Lebenszyklus muss beim Sammeln
eines Flow ausdrücklich berücksichtigt werden:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

In Compose steht dafür `collectAsStateWithLifecycle()` zur Verfügung. `LiveData`
ist auf den Hauptthread ausgerichtet, während `Flow` den Koroutinenkontext und
`flowOn` für den Upstream verwendet.

Während einer Migration können die Typen konvertiert werden:

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**Kurz gesagt:** `LiveData` berücksichtigt den Android-Lebenszyklus automatisch.
`Flow` ist von Android unabhängig und besser für die Daten- und Domänenschicht
geeignet; in der UI wird es lebenszyklusbewusst gesammelt, während der aktuelle
Zustand in einem `StateFlow` gespeichert wird.

</details>

<details>
<summary>60. Worin besteht der Unterschied zwischen kalten und heißen Streams?</summary>

#### Kotlin

Der Unterschied liegt in der Beziehung zwischen Produzent und Collectors:

```text
kalt -> für jeden Collector wird ein eigener Produzent gestartet
heiß -> Collectors erhalten Daten aus einer gemeinsamen Quelle
```

Ein mit `flow {}` erstellter Flow ist kalt. Vor `collect` wird er nicht
ausgeführt, und jeder Collector startet den Block separat:

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

Die Operation wird somit zweimal ausgeführt. Jeder Collector besitzt einen
eigenen Lebenszyklus und Abbruch.

`StateFlow` und `SharedFlow` sind heiß: Ihr Objekt und ihre Daten existieren
unabhängig von einem bestimmten Collector.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` speichert den aktuellen Zustand. `SharedFlow` verteilt Werte an aktive
Collectors und kann die letzten Werte mithilfe von `replay` speichern.

Ein kalter Flow wird mit `stateIn()` oder `shareIn()` in einen gemeinsam genutzten
heißen Flow umgewandelt. Die Richtlinie `SharingStarted` legt fest, wann der
gemeinsame Upstream aktiv ist; alle Collectors nutzen dabei weiterhin dieselbe
Ausführung.

**Kurz gesagt:** Ein kalter Stream startet für jeden Collector einen Produzenten;
ein heißer Stream besitzt eine gemeinsame Quelle und verteilt deren Werte an alle
Collectors.

</details>

<details>
<summary>61. Was ist collectLatest und wann sollte man es verwenden?</summary>

#### Kotlin

`collectLatest` ist ein Terminaloperator, der die Verarbeitung des vorherigen
Werts abbricht, sobald ein neuer Wert eintrifft:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Nur 3 wird noch ausgegeben
}
```

Ein typischer Anwendungsfall ist eine Suche, bei der die vorherige Anfrage
hinfällig wird:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Der Abbruch erfolgt kooperativ. Suspend-APIs reagieren darauf, blockierender Code
hingegen nicht:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

`collectLatest` sollte nicht verwendet werden, wenn jeder Wert garantiert
verarbeitet werden muss, etwa bei Zahlungen, Nachrichten, Speichervorgängen oder
Analyseereignissen.

`flatMapLatest` bricht den vorherigen inneren Flow ab, `collectLatest` dagegen den
Block des Collectors.

**Kurz gesagt:** `collect` wird verwendet, wenn jeder Wert wichtig ist;
`collectLatest`, wenn die vorherige Verarbeitung nach Eintreffen eines neuen
Werts nicht mehr benötigt wird.

</details>

<details>
<summary>62. Wie wird eine Callback-basierte API in eine suspend-Funktion umgewandelt?</summary>

#### Kotlin

Ein einmaliger Callback wird mit `suspendCancellableCoroutine` umschlossen:

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

- `resume()` gibt das Ergebnis zurück;
- `resumeWithException()` gibt den Fehler zurück;
- `invokeOnCancellation` bricht die externe Operation ab oder entfernt den
  Listener.

Der Abbruch einer Koroutine beendet eine Callback-API nicht automatisch. Eine
Continuation darf nur einmal abgeschlossen werden. Kann der Callback wiederholt
oder nebenläufig ausgelöst werden, sind `tryResume()`/`completeResume()` oder ein
atomarer Schutz erforderlich.

Für einen Callback mit mehreren Werten wird `callbackFlow` verwendet:

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

`awaitClose` entfernt den Listener beim Abbruch oder Schließen des Flow. Das
Ergebnis von `trySend()` und die Pufferrichtlinie müssen entsprechend den
Zustellungsgarantien behandelt werden.

**Kurz gesagt:** Für ein einzelnes Ergebnis wird
`suspendCancellableCoroutine`, für einen Strom von Werten `callbackFlow`
verwendet. In beiden Fällen muss beim Abbruch eine Bereinigung erfolgen.

</details>

<details>
<summary>63. Worin besteht der Unterschied zwischen Channel und Flow?</summary>

#### Kotlin

Ein `Channel<T>` ist eine asynchrone Warteschlange für die Kommunikation zwischen
Koroutinen:

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

Der Produzent ruft `send()` auf, der Konsument `receive()` oder liest mit `for`.
Jeder Wert wird von genau einem Konsumenten empfangen. Daher eignet sich ein
Channel für eine Worker-Warteschlange und Fan-out. Kapazität, Schließen und
Abbruch müssen ausdrücklich verwaltet werden.

`Flow<T>` ist eine deklarative API für einen asynchronen Datenstrom:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow unterstützt `map`, `filter`, `combine`, Abbruch und strukturierte
Nebenläufigkeit. Ein mit `flow {}` erstellter Flow ist üblicherweise kalt; ein
Channel ist heiß und kann abhängig von seiner Kapazität auch ohne aktiven
Konsumenten Werte aufnehmen.

Der Gegendruck eines Channels wird durch seinen Puffer bestimmt:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Wartet auf einen Empfänger
```

- `RENDEZVOUS` bedeutet ohne Puffer;
- `BUFFERED` verwendet einen begrenzten Puffer;
- `CONFLATED` speichert den letzten Wert;
- `UNLIMITED` verwendet einen unbegrenzten Puffer mit dem Risiko wachsenden
  Speicherverbrauchs.

Bei Flow werden `buffer()`, `conflate()` oder `collectLatest()` verwendet.

Ein Channel kann als Flow dargestellt werden:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` erzeugt keinen Broadcast: Die Collectors teilen die Werte unter
sich auf. Für Zustand wird `StateFlow`, für Broadcast `SharedFlow` und für eine
Callback-Anbindung `callbackFlow` verwendet.

**Kurz gesagt:** Ein `Channel` ist eine Producer-Consumer-Warteschlange, `Flow`
ein deklarativer Datenstrom. Für Anwendungsdatenströme sollte mit Flow begonnen
und Channel der ausdrücklichen Arbeitsübergabe zwischen Koroutinen vorbehalten
werden.

</details>

<details>
<summary>64. Welches sind die wichtigsten Flow-Operatoren (map, filter, combine, zip)?</summary>

#### Kotlin

`map` transformiert jeden Wert:

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` lässt nur Werte passieren, die eine Bedingung erfüllen:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` wartet auf den ersten Wert jedes Flow und emittiert anschließend ein
neues Ergebnis, sobald einer von ihnen aktualisiert wird:

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` verbindet Werte der Reihenfolge nach paarweise:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

Das Ergebnis lautet `1A`, `2B`, `3C`. `zip` wartet auf das jeweils passende Paar,
während `combine` die zuletzt verfügbaren Werte verwendet. Für einen Zustand aus
mehreren Quellen wird daher üblicherweise `combine`, für Wertepaare dagegen `zip`
benötigt.

**Kurz gesagt:** `map` transformiert, `filter` filtert, `combine` reagiert auf die
neuesten Werte aller Streams und `zip` verbindet Werte paarweise.

</details>

<details>
<summary>65. Worin besteht der Unterschied zwischen combine und zip?</summary>

#### Kotlin

Beide Operatoren verbinden Flows, besitzen jedoch eine unterschiedliche Semantik:

```text
combine -> neuer Wert + letzte Werte der übrigen Flows
zip     -> erster mit erstem, zweiter mit zweitem
```

`combine` wartet auf den ersten Wert jedes Upstreams und reagiert anschließend
auf jede Aktualisierung:

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` bildet der Reihenfolge nach Paare:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

Der schnellere Upstream wartet bei `zip` auf sein Gegenstück. Das Ergebnis wird
beendet, sobald keine vollständigen Paare mehr entstehen können. Bei `combine`
kann der letzte Wert eines abgeschlossenen Upstreams weiterverwendet werden,
während die übrigen weiterhin Werte emittieren.

**Kurz gesagt:** `combine` eignet sich für einen Zustand, der von jeder Quelle
aktualisiert wird; `zip` für die paarweise Zuordnung von Folgen.

</details>

<details>
<summary>66. Wofür werden @JvmStatic, @JvmOverloads und @JvmField benötigt?</summary>

#### Kotlin

Diese Annotationen verändern die JVM-API für Java-Code.

- `@JvmStatic` generiert eine statische Brückenmethode für einen Member eines
  `object` oder `companion object`.
- `@JvmOverloads` generiert Überladungen für aufeinanderfolgende Parameter mit
  Standardwerten, beginnend von rechts.
- `@JvmField` macht das Backing Field direkt und ohne Getter beziehungsweise
  Setter zugänglich.

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

Java kann anschließend `UserFactory.create()` aufrufen,
`UserFactory.defaultName` lesen und die Überladung ohne `active` verwenden.

`@JvmField` funktioniert weder mit einer delegierten Eigenschaft noch mit
benutzerdefinierten Accessoren und schwächt die Kapselung. Für zur Kompilierzeit
bekannte Konstanten eines primitiven Typs oder vom Typ `String` ist `const val`
vorzuziehen.

**Kurz gesagt:** Diese Annotationen werden ausschließlich für eine komfortable
Java- oder Framework-API benötigt. In reinem Kotlin-Code sollten sie nicht ohne
konkreten Bedarf hinzugefügt werden.

</details>

<details>
<summary>67. Was ist Delegation?</summary>

#### Kotlin

Bei der Delegation wird eine Implementierung an ein anderes Objekt übertragen,
anstatt Vererbung einzusetzen. Kotlin unterstützt die Delegation von
Schnittstellen mit `by`:

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

Der Compiler generiert weiterleitende Methoden. Einzelne Methoden können
überschrieben werden:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Auch Eigenschaften können einen Delegaten besitzen:

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

Ein benutzerdefinierter Eigenschaftsdelegat implementiert `getValue()` und bei
einer `var`-Eigenschaft zusätzlich `setValue()`.

**Kurz gesagt:** Klassendelegation beseitigt wiederkehrenden Weiterleitungscode
und unterstützt Komposition; Eigenschaftsdelegation lagert die Logik zum Lesen
und Schreiben einer Eigenschaft in ein separates Objekt aus.

</details>

<details>
<summary>68. Welche Testframeworks stehen für Kotlin zur Verfügung?</summary>

#### Kotlin

Die Wahl hängt von der Testebene ab:

- `kotlin.test` bietet Multiplatform-Annotationen und Assertions;
- JUnit eignet sich für JVM-Unittests;
- Kotest bietet eine alternative DSL und Matcher;
- MockK oder Mockito dienen der Erstellung von Test-Doubles;
- `kotlinx-coroutines-test` unterstützt Koroutinen, Test-Dispatcher und virtuelle
  Zeit;
- Turbine prüft Flow-Emissionen;
- AndroidX Test, Espresso und Compose UI Test dienen Android- beziehungsweise
  UI-Tests;
- Robolectric deckt einen Teil der Android-Szenarien auf der JVM ab.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

Für Domänenlogik genügen häufig JUnit oder `kotlin.test` und manuell erstellte
Fakes. Das Werkzeug ersetzt nicht die Wahl der richtigen Testebene:
Gerätespezifisches Verhalten wird mit Instrumentationstests geprüft.

**Kurz gesagt:** Für Unittests eignen sich JUnit oder `kotlin.test`, für
Koroutinen coroutines-test, für Flow Turbine und für Android-UIs Espresso oder
Compose UI Test.

</details>

<details>
<summary>69. Wie werden Abhängigkeiten in Tests gemockt?</summary>

#### Kotlin

Eine Abhängigkeit wird über den Konstruktor übergeben und durch einen Mock, Stub
oder Fake ersetzt.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

MockK für eine Suspend-Funktion:

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

Ein manuell erstellter Fake:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

Ein Fake eignet sich besser zur Verhaltensprüfung, ein Mock dagegen, wenn eine
Interaktion Bestandteil des Vertrags ist. Wertobjekte, reine Funktionen und jeder
interne Aufruf sollten nicht gemockt werden, da solche Tests an die Implementierung
gekoppelt sind.

**Kurz gesagt:** Verwende Konstruktorinjektion und bevorzuge einen einfachen
Fake. Ein Mock sollte für wichtige Interaktionen mit einer externen Abhängigkeit
eingesetzt werden.

</details>

<details>
<summary>70. Wie werden Koroutinen und Flow getestet?</summary>

#### Kotlin

Für Koroutinen werden `kotlinx-coroutines-test` und `runTest` verwendet:

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` stellt virtuelle Zeit und einen `TestCoroutineScheduler` bereit:

- `runCurrent()` führt die zum aktuellen Zeitpunkt anstehenden Aufgaben aus;
- `advanceTimeBy()` verschiebt die virtuelle Zeit;
- `advanceUntilIdle()` verarbeitet die Warteschlange bis zum Leerlauf.

Dispatcher sollten möglichst als Abhängigkeiten übergeben werden.
`Dispatchers.Main` wird in lokalen Unittests durch einen Test-Dispatcher ersetzt
und anschließend mit `resetMain()` zurückgesetzt.

Die Reihenfolge der Flow-Emissionen lässt sich bequem mit Turbine prüfen:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Für den Endzustand eines `StateFlow` genügt es häufig, die anstehenden Aufgaben
auszuführen und `state.value` zu prüfen. `Thread.sleep()`, reale Verzögerungen oder
verschiedene Test-Scheduler innerhalb desselben Tests sollten vermieden werden.

**Kurz gesagt:** `runTest` und ein Test-Dispatcher dienen Koroutinentests, Turbine
prüft die Reihenfolge von Flow-Emissionen und `StateFlow.value` den Endzustand.

</details>

<details>
<summary>71. Welche Best Practices gelten für das Schreiben testbaren Codes?</summary>

#### Kotlin

Testbarer Code besitzt explizite Abhängigkeiten und ein vorhersehbares Verhalten.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

Die wichtigsten Regeln:

- Konstruktorinjektion statt der Erzeugung von Abhängigkeiten innerhalb einer
  Klasse;
- eine klar umrissene Verantwortung pro Klasse;
- reine Funktionen für Geschäftsberechnungen;
- Abstraktionen für Zeit, Ein- und Ausgabe, APIs, Datenbanken und Dispatcher;
- möglichst wenig Android-Framework-Code in der Domänenlogik;
- Fakes statt übermäßigen Mockings;
- Prüfung des öffentlichen Verhaltens statt privater Methoden und interner
  Aufrufe.

Ein ViewModel wird anhand seines beobachtbaren Zustands und seiner Effekte
getestet:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

Es ist nicht erforderlich, für jede Klasse eine Schnittstelle anzulegen. Eine
Abstraktion ist an der Grenze zu einer veränderlichen oder externen Abhängigkeit
gerechtfertigt.

**Kurz gesagt:** Explizite Abhängigkeiten, kontrollierte Nebenwirkungen und kleine
Verantwortungsbereiche machen Code testbar, ohne Tests an die Implementierung zu
koppeln.

</details>

<details>
<summary>72. Wie lassen sich Abhängigkeiten in einem Kotlin-Projekt effektiv verwalten?</summary>

#### Kotlin

In einem Gradle-Projekt müssen Versionen, Konfigurationen und die Richtung der
Abhängigkeiten zwischen Modulen kontrolliert werden.

- Ein Version Catalog zentralisiert Aliase und Versionen in
  `libs.versions.toml`.
- `implementation` verbirgt eine Abhängigkeit vor nachgelagerten Modulen.
- `api` macht sie als Bestandteil der öffentlichen API sichtbar und sollte nur
  bei Bedarf verwendet werden.
- `testImplementation` und `androidTestImplementation` isolieren
  Testabhängigkeiten.
- Eine BOM stimmt die Versionen der Artefakte eines Ökosystems aufeinander ab.
- Convention Plugins beseitigen doppelte Gradle-Konfiguration.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

Ein Modul sollte vom benötigten Vertrag und nicht von der gesamten Implementierung
abhängen. Gradle-Abhängigkeiten und Dependency Injection zur Laufzeit sind
unterschiedliche Ebenen: Gradle bestimmt die Verfügbarkeit des Codes, DI erzeugt
Objekte zur Laufzeit.

Zur Diagnose:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Aktualisierungen sollten in kleinen Schritten erfolgen, wobei Changelog, Build
und Tests geprüft werden.

**Kurz gesagt:** Zentralisiere Versionen, bevorzuge `implementation`, kontrolliere
den Modulgraphen und prüfe den Abhängigkeitsgraphen regelmäßig.

</details>

<details>
<summary>73. Wie arbeitet Kotlin mit Speicher und Garbage Collection?</summary>

#### Kotlin

Das Speichermodell hängt von der Plattform ab. Auf der JVM und unter Android
verwendet Kotlin die Speicherverwaltung von JVM beziehungsweise ART: Objekte
liegen im Heap, und der Garbage Collector gibt diejenigen frei, auf die keine
erreichbaren Referenzen mehr verweisen.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

Nach Abschluss der Funktion kann das Objekt eingesammelt werden, sofern nichts
mehr darauf verweist. Eine Zuweisung von `null` entfernt lediglich eine Referenz;
den Zeitpunkt der Bereinigung bestimmt der Garbage Collector.

Ein Speicherleck entsteht, wenn ein nicht mehr benötigtes Objekt weiterhin
erreichbar bleibt:

```kotlin
object Holder {
    var activity: Activity? = null // Kann eine Activity festhalten
}
```

Typische Ursachen unter Android sind eine `Activity` oder `View` in einem
Singleton, nicht entfernte Listener oder Bindings sowie eine Koroutine mit einem
längeren Lebenszyklus. Häufige Allokationen, Boxing, Closures und
Zwischen-Collections können den Garbage Collector stärker belasten, sollten aber
erst nach einer Profiler-Analyse optimiert werden.

Kotlin/Native und Kotlin/JS verwenden die Speichermechanismen ihrer jeweiligen
Plattform. JVM-spezifische Details lassen sich daher nicht auf alle Targets
übertragen.

**Kurz gesagt:** Auf JVM und Android gibt der Garbage Collector Speicher frei,
nachdem sämtliche erreichbaren Referenzen verschwunden sind. Das Hauptrisiko ist
nicht `null`, sondern eine langlebige Referenz auf ein nicht mehr benötigtes
Objekt.

</details>

<details>
<summary>74. Welche Best Practices gelten für Null-Sicherheit?</summary>

#### Kotlin

Ein nullable Typ sollte die tatsächliche Möglichkeit ausdrücken, dass ein Wert
fehlt, und nicht vorsorglich verwendet werden.

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

Die wichtigsten Regeln:

- Vermeide `!!`; verwende `?.`, `?:` oder eine frühzeitige Rückgabe.
- `requireNotNull` dient einem Eingabevertrag, `checkNotNull` einem internen
  Zustand.
- Prüfe Java-Plattformtypen an der Grenze zwischen Java und Kotlin.
- Wandle nullable DTOs in einem Mapper in strikte Domänenmodelle um.
- Bezeichnet `null` einen eigenen Zustand, modelliere ihn mit einem Sealed-Typ.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

Ein Ersatzwert darf keinen Fehler verbergen. Verletzt das Fehlen eines Werts den
Vertrag, sollte die Ausführung besser ausdrücklich beendet werden.

**Kurz gesagt:** Modelliere das Fehlen eines Werts mit `T?`, behandle es an der
Systemgrenze und verwandle nullable Code nicht in eine Kette aus `!!`.

</details>

<details>
<summary>75. Was ist supervisorScope und worin unterscheidet er sich?</summary>

#### Kotlin

In einem `coroutineScope` bricht ein unbehandelter Fehler einer untergeordneten
Koroutine den Scope und die benachbarten Aufgaben ab. In einem `supervisorScope`
können direkte Kinder unabhängig voneinander fehlschlagen:

```text
coroutineScope  -> Fehler eines Kindes bricht benachbarte Aufgaben ab
supervisorScope -> Fehler eines Kindes bricht benachbarte Aufgaben nicht ab
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

Ein Supervisor behandelt Fehler nicht automatisch. Eine Ausnahme aus `async`
wird durch `await()` ausgelöst, sofern sie nicht zuvor in ein Ergebnis umgewandelt
wurde.

`supervisorScope` wird lokal innerhalb einer Suspend-Funktion eingesetzt,
`SupervisorJob` dagegen für einen langlebigen Scope.

**Kurz gesagt:** Für zusammengehörige Alles-oder-nichts-Aufgaben wird
`coroutineScope` verwendet; für unabhängige Aufgaben, bei denen ein Teilergebnis
zulässig ist, `supervisorScope`.

</details>

<details>
<summary>76. Was ist Backpressure und wie wird sie bei Flow behandelt?</summary>

#### Kotlin

Backpressure entsteht, wenn ein Produzent potenziell schneller ist als ein
Konsument. Bei einem gewöhnlichen Flow ohne Puffer setzt `emit()` die Ausführung
aus, bis der Downstream die Verarbeitung abgeschlossen hat. Dadurch wird der
Produzent auf natürliche Weise verlangsamt.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

Folgende Operatoren verändern dieses Verhalten:

- `buffer(n)` erlaubt dem Produzenten, dem Konsumenten um `n` Werte voraus zu
  sein;
- `conflate()` überspringt Zwischenwerte und behält den aktuellen Wert bei;
- `collectLatest` bricht die vorherige Verarbeitung ab;
- `debounce` wartet auf eine Pause zwischen eingehenden Ereignissen;
- `sample` übernimmt in bestimmten Intervallen den jeweils letzten Wert;
- `flatMapLatest` bricht den vorherigen inneren Flow ab.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

Ist jeder Wert wichtig, darf er weder zusammengeführt noch seine Verarbeitung
abgebrochen werden. Stattdessen muss der Konsument optimiert oder ein
kontrollierter Puffer beziehungsweise eine zuverlässige Warteschlange verwendet
werden. Ein unbegrenzter Puffer kann zu wachsendem Speicherverbrauch führen.

**Kurz gesagt:** Flow erzeugt bereits durch das aussetzende `emit()`
Backpressure. Pufferung, Zusammenführung oder Latest-Operatoren sollten nur dann
hinzugefügt werden, wenn die erforderliche Richtlinie für Datenverlust oder
Wartezeiten bekannt ist.

</details>

<details>
<summary>77. Wie werden Java Streams in Kotlin verwendet?</summary>

#### Kotlin

Kotlin/JVM kann die Java Stream API direkt verwenden:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Für Kotlin-Collections sind die Standardoperatoren üblicherweise kürzer:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Für die lazy synchrone Verarbeitung steht `Sequence` zur Verfügung:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Ein Stream ist sinnvoll, wenn er von einer Java-API zurückgegeben wird, ein
Java-`Collector` benötigt wird oder die Pipeline bereits in Java geschrieben ist.
Ein Stream kann nur einmal verwendet werden.

Ein Ein-/Ausgabe-Stream muss geschlossen werden:

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Java Collectors besitzen häufig ein direktes Kotlin-Gegenstück:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` verwendet den gemeinsamen `ForkJoinPool`, wodurch die
Parallelität implizit und der Abbruch schwieriger wird. In Koroutinencode sollte
Parallelität ausdrücklich gesteuert werden; bei Ein- und Ausgabe ersetzt ein
paralleler Stream keine Suspend-API.

```text
Stream/Sequence -> synchrone, einmal verwendbare Pipeline
Flow            -> asynchroner, aussetzbarer und abbrechbarer Stream
```

**Kurz gesagt:** Verwende Java Stream für Java-Interoperabilität,
Kotlin-Collection-Operatoren für eager Verarbeitung, `Sequence` für eine lazy
synchrone Pipeline und `Flow` für asynchrone Daten.

</details>

<details>
<summary>78. Was ist Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` ist die frühere Bezeichnung für den mobilen Bereich von Kotlin
Multiplatform (`KMP`). Die Idee besteht darin, gemeinsamen Kotlin-Code in ein
Shared-Modul auszulagern und unter Android sowie iOS zu verwenden.

Üblicherweise werden folgende Bestandteile gemeinsam genutzt:

- Domänenmodelle;
- Use Cases;
- Validierung;
- Repository-Verträge;
- Netzwerk- und Datenschicht;
- Geschäftsregeln.

Eine typische Struktur:

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` enthält plattformunabhängigen Code:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Plattformspezifische APIs werden hinter einer Schnittstelle oder `expect`/`actual`
verborgen:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

Die Benutzeroberfläche kann nativ bleiben: Compose oder Views unter Android und
SwiftUI oder UIKit unter iOS. Ist das Team dazu bereit, kann ein Teil der UI mit
Compose Multiplatform gemeinsam genutzt werden.

**Kurz gesagt:** KMP ermöglicht gemeinsame Geschäftslogik für Android und iOS;
Plattform-APIs, UX-Details und Integrationen müssen jedoch isoliert werden. Es
bedeutet nicht „ein Code für alles“, sondern kontrollierte gemeinsame Nutzung
dort, wo sie tatsächlich Duplikation verringert.

</details>

<details>
<summary>79. Welche Einschränkungen hat Kotlin Multiplatform?</summary>

#### Kotlin

Kotlin Multiplatform ist für gemeinsam genutzte Logik nützlich, besitzt jedoch
wichtige Einschränkungen.

1. **Plattform-APIs sind in `commonMain` nicht verfügbar**

Im gemeinsamen Code können `Context`, UIKit sowie ausschließlich für Android
oder iOS verfügbare SDKs nicht direkt verwendet werden. Dafür sind Schnittstellen
oder `expect`/`actual` erforderlich.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **Nicht alle Bibliotheken unterstützen Multiplatform**

Eine ausschließlich für Java oder Android verfügbare Bibliothek kann nicht
einfach `commonMain` hinzugefügt werden. Es muss eine KMP-Alternative gefunden
oder eine plattformspezifische Implementierung erstellt werden.

3. **Die iOS-Interoperabilität weist Besonderheiten auf**

Gemeinsam genutzter Kotlin-Code wird für iOS als Framework exportiert. Dabei sind
die Swift-API, Generics, Ausnahmen, die Anbindung von Koroutinen und Flow sowie das
Debugging zu berücksichtigen.

4. **Eine gemeinsame UI ist nicht immer vorteilhaft**

Häufig werden Domänen- und Datenschicht geteilt, während die Benutzeroberfläche
nativ bleibt. Compose Multiplatform ist möglich, stellt jedoch eine eigenständige
Architekturentscheidung dar.

5. **Komplexerer Build und Zuständigkeiten**

KMP ergänzt Source Sets, Gradle-Konfiguration und CI-Komplexität. Außerdem müssen
sich Android- und iOS-Teams über die Zuständigkeit für das Shared-Modul einigen.

**Kurz gesagt:** KMP eignet sich gut für stabile gemeinsame Geschäftslogik,
beseitigt plattformspezifischen Code jedoch nicht. Die wichtigsten Risiken sind
Interoperabilität, Bibliotheken, Build-Komplexität, Zuständigkeiten und das
übermäßige Auslagern sämtlicher Logik in das Shared-Modul.

</details>

<details>
<summary>80. Wie wird Geschäftslogik in KMM gemeinsam genutzt?</summary>

#### Kotlin

Geschäftslogik wird in ein Shared-Modul ausgelagert, während Android- und
iOS-spezifische Details in den plattformspezifischen Source Sets verbleiben.

Eine typische Struktur:

```text
shared/
  commonMain   -> Domäne, Use Cases und Verträge
  androidMain  -> Android-Implementierung
  iosMain      -> iOS-Implementierung
```

Für `commonMain` eignen sich insbesondere:

- Domänenmodelle;
- Use Cases;
- Validierung;
- Repository-Schnittstellen;
- API-Clients;
- Serialisierung;
- Geschäftsregeln.

Ein Beispiel:

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

Plattformspezifische APIs werden über Dependency Injection, Schnittstellen oder
`expect`/`actual` bereitgestellt:

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

Für Netzwerkzugriffe wird häufig Ktor Client, für JSON
`kotlinx.serialization` und für Zustand Koroutinen beziehungsweise Flow
verwendet. Für iOS sollte von Anfang an eine Anbindung an Swift async/await oder
Combine eingeplant werden.

Nicht sämtliche Bestandteile sollten gemeinsam genutzt werden. Berechtigungen,
Kamera, Benachrichtigungen, Plattformspeicher und UX-spezifische Logik verbleiben
besser auf der jeweiligen Plattform oder werden hinter einem Vertrag verborgen.

**Kurz gesagt:** Das Shared-Modul enthält stabile Geschäftslogik und Verträge;
plattformspezifische Bestandteile werden getrennt implementiert. Ein gutes KMM
zeichnet sich durch eine klare Grenze aus und nicht durch einen gemeinsamen
Monolithen.

</details>

<details>
<summary>81. Wie lässt sich die Performance von Kotlin-Code optimieren?</summary>

#### Kotlin

Optimierung beginnt nicht bei der Syntax, sondern bei der Messung:

```text
messen -> Engpass finden -> beheben -> erneut messen
```

Unter Android werden dafür Profiler, Perfetto/System Trace, Macrobenchmark und
Allocation Tracking verwendet.

Die wichtigsten Bereiche:

1. **Algorithmen und Ein-/Ausgabe**

Den größten Gewinn liefern geeignete Datenstrukturen, weniger Netzwerk- und
Datenbankaufrufe, Pagination, Caching und Datenbankindizes. Eine Komplexität von
`O(n²)` lässt sich nicht durch eleganten Kotlin-Syntax beheben.

2. **Allokationen in performancekritischen Pfaden**

An kritischen Stellen können überflüssige Aufrufe von `map`, `filter` und `copy`
zahlreiche Objekte erzeugen:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

In gewöhnlichem UI-Code ist Lesbarkeit jedoch wichtiger als manuelle
Mikrooptimierung.

3. **Collections und Sequence**

`Sequence` ist für lazy Ketten oder einen vorzeitigen Abbruch nützlich, besitzt
aber eigenen Overhead. Bei kleinen Listen ist sie nicht zwangsläufig schneller.

4. **Koroutinen**

- blockierende Ein- und Ausgabe auf `Dispatchers.IO`;
- CPU-intensive Arbeit auf `Dispatchers.Default`;
- kein `runBlocking` im Hauptthread;
- nicht mehr benötigte Arbeit abbrechen;
- unnötige Kontextwechsel vermeiden.

5. **UI-Performance**

In Compose darf keine aufwendige Arbeit während der Composition ausgeführt
werden. Es sollten stabile Schlüssel verwendet und Zustand möglichst nahe am
Verwendungsort gelesen werden. Bei Views dürfen `onBindViewHolder` und `onDraw`
nicht überlastet und keine unnötigen Layout-Durchläufe erzeugt werden.

6. **Speicher**

Eine `Activity` oder `View` darf nicht in einem Singleton festgehalten werden.
Cache-Größe, Ressourcenlebenszyklen und große Bitmaps müssen kontrolliert werden.
Speicherlecks erhöhen die Belastung des Garbage Collectors und das OOM-Risiko.

**Kurz gesagt:** Zuerst wird profiliert. Anschließend werden Algorithmen,
Ein-/Ausgabe, Allokationen, Nebenläufigkeit, kritische UI-Pfade und Speicherlecks
optimiert. `inline`, `Sequence` und primitive Arrays werden nur dort eingesetzt,
wo Messungen einen Nutzen belegen.

</details>

<details>
<summary>82. Wie wird Retry mit exponentiellem Backoff in Kotlin implementiert?</summary>

#### Kotlin

Exponentieller Backoff wiederholt einen vorübergehenden Fehler mit zunehmend
längerer Pause. Ein Retry benötigt `maxAttempts`, `maxDelay`, Jitter und ein
Timeout und darf den Abbruch nicht beeinträchtigen.

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

Ein Beispiel für das Prädikat:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Für `Flow` kann `retryWhen` verwendet werden:

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

Validierungsfehler, `403`/`404`, Authentifizierungsfehler ohne Refresh-Ablauf und
nicht idempotente Operationen ohne Idempotency Key sollten nicht wiederholt
werden. Bei `429`/`503` sollte möglichst `Retry-After` berücksichtigt werden.

**Kurz gesagt:** Ein Retry muss begrenzt und abbrechbar sein, Jitter verwenden
und eine eindeutige Regel dafür besitzen, welche Fehler wiederholt werden dürfen.
Andernfalls verschleiert er Probleme und erzeugt zusätzliche Last.

</details>

<details>
<summary>83. Welche neuen Kotlin-Funktionen sollte man 2026 kennen?</summary>

#### Kotlin

Bei einem Vorstellungsgespräch im Jahr 2026 ist es nicht entscheidend, jede
kleine Funktion zu kennen, sondern die Entwicklung von Kotlin 2.x und seines
Tooling-Ökosystems zu verstehen.

1. **K2-Compiler**

K2 ist die neue Compilerarchitektur von Kotlin 2.x. Sie ist für schnellere
Analysen und als bessere Grundlage für IDEs, Compiler-Plugins und Multiplatform
wichtig. Vor einem Upgrade muss die Kompatibilität von Kotlin Gradle Plugin, AGP,
Gradle, Compose Compiler, KSP beziehungsweise kapt und Serialization geprüft
werden.

2. **Kontextparameter und Context Receiver**

Sie sind für DSLs und kontextgebundene APIs nützlich:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

Dies ist kein Ersatz für Dependency Injection, sondern eine Möglichkeit, den
Ausführungskontext ausdrücklich zu übergeben.

3. **Explizite Backing Fields**

Sie reduzieren wiederkehrenden Code bei einer schreibgeschützten öffentlichen
API mit intern veränderlichem Zustand:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Weiterentwicklung von Multiplatform**

KMP wird praxistauglicher: Swift Export, die Anbindung von Koroutinen und Flow,
Wasm und Compose Multiplatform entwickeln sich weiter. Produktionsentscheidungen
müssen jedoch weiterhin anhand von Interoperabilität, Tooling, Debugging und
Teamzuständigkeiten bewertet werden.

5. **Standardbibliothek und JVM-Tooling**

Neue APIs der Standardbibliothek, `kotlin.uuid.Uuid`, die Kompatibilität von
JVM-Target und Toolchain, der Gradle Configuration Cache und die Build-Performance
sollten verfolgt werden.

6. **Regel für die Einführung**

Eine neue Funktion sollte nicht allein deshalb in Produktion eingesetzt werden,
weil sie neu ist. Zu prüfen sind:

- stabil oder experimentell;
- Unterstützung durch IDE und CI;
- Plugin-Kompatibilität;
- Lesbarkeit;
- Migrations- und Rollback-Plan.

**Kurz gesagt:** Im Jahr 2026 sollte man Kotlin 2.x und K2, Kontext-APIs,
Backing Fields, die Entwicklung von KMP, Wasm und Compose Multiplatform sowie die
Tooling-Einschränkungen kennen. In der Praxis ist ein sicheres Upgrade des
Ökosystems wichtiger als der Einsatz jeder neuen Syntaxfunktion.

</details>

<details>
<summary>84. Was ist der Lebenszyklus einer Activity?</summary>

#### Kotlin

Der Lebenszyklus einer `Activity` ist eine Abfolge von Callbacks, über die Android
die Erstellung, Sichtbarkeit, den Vorder- beziehungsweise Hintergrundzustand und
die Zerstörung eines Bildschirms verwaltet.

Die grundlegende Reihenfolge:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Bei der Rückkehr aus dem Zustand `STOPPED` kann vor `onStart()` noch `onRestart()`
aufgerufen werden.

- `onCreate()` erstellt UI, Navigation und grundlegende Abhängigkeiten. Hier
  sollte keine aufwendige synchrone Arbeit erfolgen.
- `onStart()` bedeutet, dass die Activity sichtbar geworden ist.
- `onResume()` bedeutet, dass sie sich im Vordergrund befindet und interaktiv ist.
- `onPause()` bedeutet, dass sie den Fokus verloren hat; dieser Callback muss
  schnell ausgeführt werden.
- `onStop()` bedeutet, dass sie nicht mehr sichtbar ist; nur für die Sichtbarkeit
  benötigte Ressourcen sollten beendet werden.
- `onDestroy()` bedeutet, dass die Instanz aufgrund von `finish()` oder einer
  Neuerstellung zerstört wird; bei der Beendigung des Prozesses ist dieser Aufruf
  nicht garantiert.

Eine Konfigurationsänderung kann die Activity neu erstellen:

```text
alt: onPause -> onStop -> onDestroy
neu: onCreate -> onStart -> onResume
```

Ein `ViewModel` übersteht eine Konfigurationsänderung. Ein kleiner UI-Zustand wird
in `SavedStateHandle` beziehungsweise `onSaveInstanceState`, langfristige Daten
werden in einem persistenten Speicher abgelegt.

Das Sammeln eines Flow muss an den Lebenszyklus gebunden werden:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**Kurz gesagt:** `onCreate` erstellt den Bildschirm, `onStart` macht ihn sichtbar,
`onResume` aktiv, `onPause` und `onStop` beenden laufende Arbeit und `onDestroy`
beendet die Instanz. Zustand darf nicht ausschließlich in der Activity-Instanz
gespeichert werden.

</details>

<details>
<summary>85. Was ist ein Fragment und wie unterscheidet es sich von einer Activity?</summary>

#### Kotlin

Eine `Activity` ist eine Android-Komponente mit eigenem Fenster und Task sowie
einem Einstiegspunkt über das Manifest. Ein `Fragment` ist ein UI-Controller
innerhalb einer Activity oder eines anderen Fragments und wird vom
`FragmentManager` verwaltet.

```text
Activity -> Systemkomponente, Fenster, Intents, Task
Fragment -> Teil der UI in einem Host, Back Stack über FragmentManager
```

Ein Activity-Beispiel:

```kotlin
class MainActivity : AppCompatActivity()
```

Ein Fragment-Beispiel:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

Der wichtigste praktische Unterschied besteht darin, dass ein Fragment einen
eigenen Lebenszyklus für seine View besitzt:

```text
onCreateView -> onViewCreated -> onDestroyView
```

Das Fragment-Objekt kann weiterhin existieren, obwohl seine View bereits zerstört
wurde. Deshalb werden Binding, Adapter und UI-Abonnements in `onDestroyView()`
bereinigt:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Flow oder LiveData für die UI müssen an `viewLifecycleOwner` und nicht an den
Lebenszyklus des Fragments selbst gebunden werden.

Navigation:

- zwischen Activities mit einem `Intent`;
- zwischen Fragments mit `FragmentManager` oder der Navigation Component.

In einer vollständig mit Compose entwickelten Anwendung genügt häufig eine
einzige Activity ohne Fragments. In einer hybriden oder View-basierten Anwendung
bleibt ein Fragment eine geeignete Abstraktion für Bildschirm und Navigation.

**Kurz gesagt:** Eine Activity ist eine eigenständige Android-Komponente, ein
Fragment ein Teil der UI innerhalb eines Hosts. Die wichtigste Regel lautet: Die
View eines Fragments lebt kürzer als das Fragment selbst, daher erfolgt die
Bereinigung in `onDestroyView()`.

</details>

<details>
<summary>86. Was ist ein Intent und wie wird er verwendet?</summary>

#### Kotlin

Ein `Intent` ist ein Android-Nachrichtenobjekt, das eine Aktion oder eine konkrete
Komponente beschreibt: eine Activity, einen Service oder einen Broadcast starten
beziehungsweise Daten übergeben.

1. **Expliziter Intent**

Er gibt eine konkrete Komponente an:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

Er wird für die interne Navigation zwischen Activities verwendet.

2. **Impliziter Intent**

Er beschreibt eine Aktion, für deren Ausführung Android eine geeignete Anwendung
sucht:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Beispiele sind Browser, Teilen, Telefon, Karten oder Dateiauswahl.

3. **Extras**

Über Extras werden kleine Datenmengen übergeben:

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Es ist besser, eine ID statt großer Objekte zu übergeben. Für strukturierte
Argumente kann `Parcelable` verwendet werden; Navigation ist jedoch häufig mit
ID und Repository einfacher.

4. **Intent-Filter**

Eine Komponente deklariert, welche Intents sie entgegennimmt:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

Dies wird für Deep Links, Ziele zum Teilen und Systemaktionen benötigt.

5. **Activity Result API**

Anstelle von `startActivityForResult` wird eine lebenszyklusbewusste API
verwendet:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // handle uri
}

launcher.launch("image/*")
```

6. **Sicherheit**

- Keine Geheimnisse in Extras übergeben.
- Interne Komponenten mit einem expliziten Intent starten.
- Externe Eingaben prüfen.
- `android:exported` korrekt setzen.
- Bei einem impliziten Intent berücksichtigen, dass möglicherweise keine
  geeignete Anwendung vorhanden ist.

**Kurz gesagt:** Ein expliziter Intent startet eine konkrete Komponente, ein
impliziter beschreibt eine Aktion für das System. Extras dienen nur kleinen
Datenmengen. Für Ergebnisse wird die Activity Result API verwendet. Externe
Intents erfordern Validierung und Sicherheitsbewusstsein.

</details>

<details>
<summary>87. Was geschieht, wenn super.onCreate() nicht aufgerufen wird?</summary>

#### Kotlin

Wird in `Activity.onCreate()` nicht `super.onCreate(savedInstanceState)`
aufgerufen, führt die Android- beziehungsweise Jetpack-Basisklasse die
erforderliche Lebenszyklusinitialisierung nicht durch. Dies kann zu einem Absturz
oder fehlerhaftem Verhalten der Activity führen.

Korrekt:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` bereitet Folgendes vor:

- den Lebenszykluszustand;
- die Wiederherstellung des gespeicherten Zustands;
- die Fenster- und Decor-Infrastruktur;
- den `FragmentManager` in einer `FragmentActivity`;
- die `SavedStateRegistry`;
- die `ActivityResultRegistry`;
- Integrationen von AppCompat und ComponentActivity.

Ohne `super` können folgende Probleme auftreten:

- `SuperNotCalledException`;
- ein fehlerhafter Lebenszyklus;
- Probleme mit Fragments;
- der gespeicherte Zustand wird nicht wiederhergestellt;
- Probleme mit Activity Result API, Compose, Hilt oder AppCompat.

Für Compose gilt dieselbe Regel:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

Auch in den Lebenszyklus-Callbacks eines Fragments wird `super` üblicherweise
aufgerufen, sofern die Dokumentation nicht ausdrücklich etwas anderes vorgibt.

**Kurz gesagt:** `super.onCreate()` ist keine Formalität, sondern Bestandteil
des Vertrags mit dem Framework. Der Aufruf muss zu Beginn von `onCreate()` vor
UI- oder Framework-abhängiger Logik erfolgen.

</details>

<details>
<summary>88. Was ist View Binding und warum ist es besser als findViewById?</summary>

#### Kotlin

`View Binding` generiert für ein XML-Layout eine Binding-Klasse und ermöglicht
typsicheren Zugriff auf Views ohne manuelles `findViewById`.

Das Problem bei `findViewById`:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Nachteile:

- wiederkehrender Code;
- der Typ kann falsch angegeben werden;
- Fehler treten erst zur Laufzeit auf;
- Refactoring wird schlechter unterstützt.

View Binding in einer `Activity`:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

In einem `Fragment` muss das Binding in `onDestroyView()` freigegeben werden,
weil der Lebenszyklus der View kürzer als der des Fragments ist:

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

Vorteile von View Binding:

- Typsicherheit;
- weniger wiederkehrender Code;
- Prüfung der IDs zur Kompilierzeit;
- komfortables Refactoring;
- nullable Felder, wenn eine View in einer bestimmten Layoutkonfiguration fehlt.

`View Binding` ist nicht dasselbe wie `Data Binding`:

```text
View Binding -> Zugriff auf Views
Data Binding -> Binding-Ausdrücke in XML
```

Für eine vollständig mit Compose erstellte UI wird View Binding üblicherweise
nicht benötigt. Es ist für das XML-/View-System und hybride Bildschirme relevant.

**Kurz gesagt:** View Binding ist ein sicherer und kürzerer Ersatz für
`findViewById` bei XML-Layouts. Der wichtigste Punkt: In einem Fragment muss das
Binding in `onDestroyView()` freigegeben werden.

</details>

<details>
<summary>89. Was ist LiveData?</summary>

#### Kotlin

`LiveData` ist ein lebenszyklusbewusster beobachtbarer Datenhalter aus Android
Jetpack. Er speichert einen Wert und benachrichtigt Observer nur, wenn sich der
`LifecycleOwner` im Zustand `STARTED` oder `RESUMED` befindet.

Ein typisches Beispiel in einem `ViewModel`:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

Nach außen wird `LiveData` und nicht `MutableLiveData` bereitgestellt, damit die
UI den Zustand nicht direkt verändern kann.

Das Abonnement in einem Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` ist wichtig, weil die View eines Fragments kürzer lebt als
das Fragment selbst.

`value` im Vergleich zu `postValue`:

```kotlin
_liveData.value = user      // Hauptthread
_liveData.postValue(user)   // Hintergrundthread, asynchron
```

Vorteile:

- lebenszyklusbewusst;
- einfache API;
- gute Zusammenarbeit mit dem XML-/View-System;
- automatische Abmeldung eines zerstörten Owners;
- komfortabel für älteren Android-Code.

Einschränkungen:

- an Android gebunden;
- schwächere Stream-API als `Flow`;
- für KMP ungeeignet;
- einmalige Ereignisse führen häufig zu Behelfslösungen ähnlich
  `SingleLiveEvent`.

Ein Vergleich:

```text
LiveData  -> lebenszyklusbewusster Android-Datenhalter
StateFlow -> heißer Kotlin-Stream mit Zustandswert
```

In neuem, auf Koroutinen oder Compose basierendem Code werden häufiger
`StateFlow` und `collectAsStateWithLifecycle()` verwendet. `LiveData` ist in einem
älteren View-System oder in Projekten sinnvoll, in denen es bereits Standard ist.

**Kurz gesagt:** LiveData ist ein beobachtbarer Android-Zustandshalter, der den
Lebenszyklus berücksichtigt. Für neuen Kotlin-/Compose-Code ist `StateFlow`
häufig besser geeignet; im XML-/View-System funktioniert LiveData weiterhin gut.

</details>

<details>
<summary>90. Was ist Room und wie funktioniert es?</summary>

#### Kotlin

`Room` ist eine Jetpack-Bibliothek für den Zugriff auf SQLite über eine
typsichere API. Sie ersetzt SQL nicht, bietet jedoch Prüfungen zur Kompilierzeit,
DAOs, Migrationen und eine komfortable Integration mit Koroutinen und Flow.

Die wichtigsten Bestandteile:

```text
@Entity   -> Tabelle
@Dao      -> SQL-Zugriffsmethoden
@Database -> Einstiegspunkt zur Datenbank
```

Entity:

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val age: Int
)
```

DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
}
```

Datenbank:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Erstellung:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room unterstützt `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Ändert sich die Tabelle, invalidiert Room die Abfrage und emittiert neue Daten.

Für Beziehungen ist häufig `@Transaction` erforderlich:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Schemaänderungen werden über Migrationen vorgenommen:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

In Produktion darf keine destruktive Migration unbedacht eingesetzt werden,
wenn Benutzerdaten vorhanden sind.

In der Praxis wird Room hinter einem Repository verborgen:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

Die UI darf nicht direkt von Datenbank-Entities abhängen.

**Kurz gesagt:** Room ist eine typsichere Abstraktion über SQLite mit `Entity`,
`Dao` und `Database`. Es unterstützt Suspend-Funktionen, Flow, Beziehungen und
Migrationen. In einer guten Architektur befindet sich Room in der Datenschicht
hinter einem Repository und nicht direkt in der UI.

</details>

<details>
<summary>91. Was bewirken die Annotationen @Query, @Insert und @Delete?</summary>

#### Kotlin

`@Query`, `@Insert` und `@Delete` sind Room-Annotationen für DAO-Methoden. Room
generiert die Implementierung und prüft SQL sowie Mapping zur Kompilierzeit.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

Führt explizites SQL aus:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` oder `:name` sind Bind-Parameter. Benutzereingaben dürfen nicht durch
String-Verkettung in SQL eingefügt werden.

`@Query` kann nicht nur für `SELECT` verwendet werden:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

`Int` gibt die Anzahl der betroffenen Zeilen an.

### `@Insert`

Fügt eine `@Entity` ein:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` bestimmt das Verhalten bei einem Konflikt des Primär- oder
Eindeutigkeitsschlüssels: `ABORT`, `IGNORE` oder `REPLACE`. Bei `REPLACE` ist
Vorsicht geboten: SQLite kann die alte Zeile tatsächlich löschen und eine neue
einfügen. Für Einfügen-oder-Aktualisieren ist `@Upsert` häufig besser geeignet,
sofern es im Projekt verfügbar ist.

### `@Delete`

Löscht eine Entity anhand ihres Primärschlüssels:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Für das Löschen nach ID oder Bedingung eignet sich `@Query("DELETE ...")` besser.

### `@Transaction`

Müssen mehrere DAO-Operationen atomar ausgeführt werden, wird `@Transaction`
verwendet:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room prüft Tabellen, Spalten, Bind-Parameter und Rückgabemapping. Performance,
Indizes und Geschäftslogik müssen jedoch weiterhin vom Entwickler geprüft werden.

**Kurz gesagt:** `@Query` führt SQL aus, `@Insert` fügt eine Entity ein und
`@Delete` löscht sie anhand ihres Primärschlüssels. Room generiert den DAO-Code
und erkennt viele Fehler zur Kompilierzeit, ersetzt aber keine Analyse der
Abfrageperformance.

</details>

<details>
<summary>92. Was ist WorkManager und wann wird er verwendet?</summary>

#### Kotlin

`WorkManager` ist eine Jetpack-API für dauerhafte, aufschiebbare Hintergrundarbeit.
Eine Aufgabe kann also später gestartet werden, muss aber das Schließen der
Anwendung, das Beenden des Prozesses und bei Bedarf einen Neustart des Geräts
überstehen.

Er wird verwendet für:

- Synchronisierung im Hintergrund;
- erneute Upload- oder Downloadversuche;
- Bereinigung;
- Versand von Protokollen oder Analysedaten;
- regelmäßige Wartung;
- Arbeit mit Bedingungen wie Netzwerk, Laden sowie Batterie- oder Speicherstatus.

Einmalige Arbeit:

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

Ein `CoroutineWorker`:

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

Mögliche Ergebnisse:

- `Result.success()` bedeutet, dass die Aufgabe abgeschlossen wurde;
- `Result.retry()` wiederholt sie gemäß der Backoff-Richtlinie;
- `Result.failure()` bezeichnet einen endgültigen Fehler.

Wichtige Funktionen:

- Bedingungen;
- Wiederholungsversuche und Backoff;
- eindeutige Arbeit mit `enqueueUniqueWork()`;
- Verkettung;
- periodische Arbeit, jedoch ohne exakten Startzeitpunkt.

`Data` für Ein- und Ausgabe muss klein bleiben. Große Nutzdaten werden in einer
Datenbank oder Datei gespeichert; übergeben wird lediglich ihre ID.

WorkManager eignet sich nicht für:

- exakte Alarme;
- eine Echtzeit-Socket-Verbindung;
- Medienwiedergabe;
- dauerhafte Standortverfolgung;
- eine Aktion, die der Benutzer sofort erwartet.

Für fortlaufende, für den Benutzer sichtbare Arbeit ist ein Foreground Service
erforderlich. Für einen exakten Zeitpunkt wird AlarmManager beziehungsweise die
Exact Alarm API verwendet, sofern der Anwendungsfall dies tatsächlich erlaubt.

**Kurz gesagt:** WorkManager dient zuverlässiger aufgeschobener Hintergrundarbeit
mit Bedingungen und Wiederholungsversuchen. Er garantiert keinen exakten
Startzeitpunkt und ersetzt weder einen Foreground Service noch eine
Echtzeitverbindung oder einen exakten Alarm.

</details>

<details>
<summary>93. Worin besteht der Unterschied zwischen CoroutineScope, lifecycleScope und viewModelScope?</summary>

#### Kotlin

Ein `CoroutineScope` definiert Kontext und Lebensdauer von Koroutinen.
`lifecycleScope` und `viewModelScope` sind fertige Android-Scopes mit einem
bereits festgelegten Eigentümer.

```text
CoroutineScope  -> Lebensdauer wird selbst festgelegt
lifecycleScope  -> lebt bis DESTROYED im LifecycleOwner
viewModelScope  -> lebt bis ViewModel.onCleared()
```

### `CoroutineScope`

Ein allgemeiner Scope für Koroutinen:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Wird ein eigener Scope erstellt, muss zum richtigen Zeitpunkt ausdrücklich
`cancel()` aufgerufen werden. Andernfalls entstehen ein Speicherleck oder
unnötige Arbeit nach dem Ende des Eigentümers.

### `lifecycleScope`

Er gehört einer `Activity`, einem `Fragment` oder einem anderen `LifecycleOwner`
und wird bei `DESTROYED` abgebrochen:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`lifecycleScope.launch` selbst wird im Zustand `STOPPED` nicht pausiert. Für das
Sammeln eines Flow ist daher `repeatOnLifecycle` erforderlich.

In einem Fragment sollte für die Arbeit mit View oder Binding
`viewLifecycleOwner.lifecycleScope` verwendet werden:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Der Grund: Das Fragment-Objekt kann länger leben als seine View.

### `viewModelScope`

Er gehört einem `ViewModel`, übersteht Konfigurationsänderungen und wird in
`onCleared()` abgebrochen:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Er wird für Bildschirm-Logik, Ladevorgänge, Geschäftsaufrufe und die Bildung des
UI-Zustands verwendet.

Typische Fehler:

- `GlobalScope` für bildschirmbezogene Arbeit;
- eigener Scope ohne Abbruch;
- Sammeln von Binding oder Zustand im Fragment-Scope statt im
  View-Lebenszyklus-Scope;
- Starten von Geschäftsoperationen aus einer Activity statt aus dem ViewModel;
- Zugriff auf das Binding nach `onDestroyView()`.

**Kurz gesagt:** `viewModelScope` dient der Bildschirm-Logik im ViewModel,
`lifecycleScope` dem Sammeln von UI-Daten und UI-Effekten. Ein eigener
`CoroutineScope` ist nur sinnvoll, wenn er eine klar definierte Lebensdauer und
eine Stelle für `cancel()` besitzt.

</details>

<details>
<summary>94. Wie wird Dependency Injection in Kotlin umgesetzt?</summary>

#### Kotlin

Dependency Injection bedeutet, Abhängigkeiten von außen zu übergeben, anstatt sie
innerhalb einer Klasse zu erzeugen. Der übliche Ansatz in Kotlin ist
Konstruktorinjektion.

Korrekt:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Ungünstig:

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

Die zweite Variante koppelt die Klasse fest an eine Implementierung und erschwert
Tests.

Ein Use Case hängt üblicherweise von einem Vertrag ab:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Manuelle Dependency Injection

Für ein kleines Projekt genügt ein einfacher Container:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Der Vorteil ist die Einfachheit. Ein großer Abhängigkeitsgraph lässt sich jedoch
nur schwer manuell pflegen.

### Hilt/Dagger

Compile-Time-DI ist eine typische Wahl für große Android-Anwendungen:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val getUser: GetUserUseCase
) : ViewModel()
```

Das Binden einer Schnittstelle:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface UserModule {
    @Binds
    fun bindUserRepository(impl: UserRepositoryImpl): UserRepository
}
```

Der Vorteil ist, dass Fehler im Abhängigkeitsgraphen zur Kompilierzeit erkannt
werden. Nachteile sind mehr wiederkehrender Code und eine komplexere
Codegenerierung.

### Koin

Runtime-DI über eine Kotlin-DSL:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

Der Einstieg ist einfacher; ein Teil der Fehler kann jedoch erst zur Laufzeit
auftreten.

DI verwaltet außerdem Lebensdauern wie Singleton, Factory und ViewModel-Scope.
Ein Scope muss sorgfältig gewählt werden, um Speicherlecks oder unnötige
Singletons zu vermeiden.

**Kurz gesagt:** Dependency Injection wird in Kotlin meist über
Konstruktorinjektion umgesetzt. Manuelle DI eignet sich für kleine Graphen,
Hilt/Dagger für große Android-Anwendungen und Koin für einen einfachen Einstieg.
DI verringert Kopplung und vereinfacht Tests erheblich.

</details>

<details>
<summary>95. Was ist Hilt und wie funktioniert es?</summary>

#### Kotlin

`Hilt` ist ein Android-DI-Framework auf Grundlage von Dagger. Es generiert einen
Abhängigkeitsgraphen und eine fertige Komponentenhierarchie für `Application`,
`Activity`, `Fragment`, `ViewModel`, `View` und `Service`.

Grundlegende Einrichtung:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` erstellt den Root-Graphen auf Anwendungsebene.

Eine Android-Klasse, in die Abhängigkeiten injiziert werden sollen:

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Konstruktorinjektion ist die bevorzugte Methode:

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

Ein ViewModel:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Module sind erforderlich, wenn Hilt eine Abhängigkeit nicht selbst erzeugen kann,
beispielsweise eine Schnittstelle, eine Drittanbieterklasse oder einen Builder
beziehungsweise eine Factory.

`@Binds` verbindet eine Schnittstelle mit ihrer Implementierung:

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

`@Provides` dient der Erzeugung über eine Factory oder einen Builder,
beispielsweise bei `OkHttpClient`, `Retrofit` oder `Room`.

Scopes bestimmen die Lebensdauer eines Objekts:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Die wichtigsten Komponenten:

```text
SingletonComponent        -> Anwendung
ActivityRetainedComponent -> übersteht Konfigurationsänderungen
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Eine wichtige Regel: Eine langlebige Komponente darf nicht von einer
kurzlebigen abhängen. Eine mit `@Singleton` versehene Abhängigkeit darf
beispielsweise keinen Activity-Kontext festhalten.

Für Tests ermöglicht Hilt den Austausch von Modulen mit `@TestInstallIn`. In
Unittests ist es jedoch häufig einfacher, einen Fake direkt über den Konstruktor
zu übergeben.

**Kurz gesagt:** Hilt generiert einen Dagger-Graphen für Android. `@Inject`
erzeugt Abhängigkeiten, `@Binds` und `@Provides` beschreiben Bindings,
`@AndroidEntryPoint` aktiviert die Injection in einer Android-Klasse und Scopes
steuern die Lebensdauer.

</details>

<details>
<summary>96. Was ist @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` integriert ein `ViewModel` in Hilt. Hilt generiert eine
`ViewModelProvider.Factory` und übergibt Abhängigkeiten per Konstruktorinjektion,
wodurch eine manuelle Factory üblicherweise entfällt.

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

Regeln:

- `@HiltViewModel` wird an der Klasse angebracht;
- `@Inject constructor` am Konstruktor;
- sämtliche Abhängigkeiten müssen im Hilt-Graphen vorhanden sein;
- `SavedStateHandle` wird von Hilt automatisch bereitgestellt;
- die Host-`Activity` beziehungsweise das Host-`Fragment` benötigt
  `@AndroidEntryPoint`.

Abruf in einem Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

Für ein gemeinsames ViewModel auf Activity-Ebene:

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

Der Scope des ViewModels wird durch den `ViewModelStoreOwner` bestimmt, also etwa
Fragment, Activity oder Navigation-Back-Stack-Eintrag.

Abhängigkeiten des ViewModels leben im `ViewModelComponent`. `@ViewModelScoped`
liefert eine Instanz pro konkretem ViewModel, `@Singleton` eine Instanz pro
Anwendungsgraph.

Typische Fehler:

- `@HiltViewModel` oder `@Inject constructor` vergessen;
- `@AndroidEntryPoint` im Host vergessen;
- das ViewModel manuell über den Konstruktor erzeugen;
- bei unterschiedlichen `ViewModelStoreOwner` dieselbe Instanz erwarten;
- `Activity`, `Fragment`, `View` oder einen kurzlebigen `Context` in ein ViewModel
  injizieren.

**Kurz gesagt:** `@HiltViewModel` ermöglicht Hilt, ein ViewModel über eine
generierte Factory zu erzeugen und Konstruktorabhängigkeiten sowie
`SavedStateHandle` zu injizieren. Die tatsächliche Lebensdauer bestimmt der
`ViewModelStoreOwner`, nicht die Annotation selbst.

</details>

<details>
<summary>97. Wie funktioniert @Inject?</summary>

#### Kotlin

`@Inject` ist eine Annotation, anhand derer Dagger beziehungsweise Hilt erkennt,
wie ein Objekt erzeugt oder wo eine Abhängigkeit eingesetzt werden soll.

Die bevorzugte Variante ist Konstruktorinjektion:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt kann `LoadUserUseCase` erzeugen, wenn für jeden Konstruktorparameter ein
Binding im Graphen vorhanden ist. Fehlt ein Binding oder ist es mehrdeutig,
entsteht ein Fehler zur Kompilierzeit.

Konstruktorinjektion ist für eigene Klassen vorzuziehen:

- Abhängigkeiten sind explizit;
- `val` kann verwendet werden;
- die Klasse ist leicht testbar;
- es gibt keine verborgene Framework-Magie.

Feldinjektion wird überwiegend bei Android-Klassen eingesetzt, die vom Framework
erzeugt werden:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Ein solches Feld darf vor der Injection im Lebenszyklus der Komponente nicht
verwendet werden.

`@Inject constructor` löst das Binding einer Schnittstelle nicht selbstständig.
Für eine Schnittstelle ist `@Binds` erforderlich:

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

Für Drittanbieterklassen oder Builder wird `@Provides` verwendet:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` legt keine Lebensdauer fest. Diese wird durch Scopes wie `@Singleton`,
`@ViewModelScoped` oder `@ActivityScoped` bestimmt. Sind mehrere Bindings
desselben Typs vorhanden, werden Qualifier verwendet.

Hilt und Dagger generieren Factories sowie den Graphen zur Kompilierzeit, ohne
Runtime-Reflection für die Erzeugung von Abhängigkeiten.

**Kurz gesagt:** `@Inject` zeigt Hilt beziehungsweise Dagger, wie eine Klasse
erzeugt oder wo eine Abhängigkeit eingesetzt wird. Konstruktorinjektion ist der
Standard; `@Binds` wird für Schnittstellen, `@Provides` für externe Objekte,
Scopes für die Lebensdauer und Qualifier zur Unterscheidung gleicher Typen
verwendet.

</details>

<details>
<summary>98. Was sind Scopes in Hilt?</summary>

#### Kotlin

Ein Scope in Hilt bestimmt, innerhalb welcher Komponente eine Abhängigkeit
zwischengespeichert wird. Alle Anfragen innerhalb derselben Komponenteninstanz
erhalten somit dasselbe Objekt.

Ohne Scope wird ein Binding üblicherweise für jede Injection-Anfrage neu erzeugt.
Dies ist ein sinnvoller Standard für günstige zustandslose Objekte.

Eine vereinfachte Komponentenhierarchie:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

Ein Kind sieht die Bindings seines Elternteils, der Elternteil darf jedoch nicht
von einem kurzlebigeren Binding des Kindes abhängen.

Die wichtigsten Scopes:

- `@Singleton` liefert eine Instanz pro Anwendungsgraph;
- `@ActivityRetainedScoped` eine Instanz pro logischer Activity und übersteht
  Konfigurationsänderungen;
- `@ViewModelScoped` eine Instanz pro konkretem ViewModel;
- `@ActivityScoped` eine Instanz pro konkreter Activity-Instanz;
- `@FragmentScoped` eine Instanz pro konkretem Fragment;
- `@ViewScoped` eine Instanz pro Hilt-fähiger View;
- `@ServiceScoped` eine Instanz pro konkretem Service.

Der Scope muss zur Komponente in `@InstallIn` passen:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

Eine mit `@ViewModelScoped` versehene Abhängigkeit ist innerhalb eines konkreten
ViewModels identisch, unterscheidet sich jedoch zwischen verschiedenen
ViewModels. Soll ein Objekt mehreren ViewModels derselben Activity gemeinsam
gehören, ist `@ActivityRetainedScoped` erforderlich.

Ein typischer Fehler besteht darin, dass ein langlebiges Objekt einen
kurzlebigen Kontext festhält:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Dies ist unzulässig: Das Singleton hält die Activity nach ihrer Zerstörung fest.
Für `@Singleton` wird Zustand oder Kontext auf Anwendungsebene benötigt.

Nicht jedes Objekt sollte mit einem Scope versehen werden. Ein Scope beschreibt
Eigentums- und Lebenszyklussemantik und ist nicht lediglich eine Optimierung.

**Kurz gesagt:** Ein Hilt-Scope bestimmt Lebensdauer und Wiederverwendung einer
Abhängigkeit innerhalb einer Komponente. Er muss zum tatsächlichen Eigentümer
passen und darf keine Objekte mit kürzerem Lebenszyklus festhalten.

</details>

<details>
<summary>99. Wie lassen sich Speicherlecks unter Android vermeiden?</summary>

#### Kotlin

Ein Speicherleck entsteht, wenn ein Objekt nicht mehr benötigt wird, aber
weiterhin eine starke Referenz darauf existiert. Unter Android sind häufig
`Activity`, `Fragment`, `View`, `Context`, Callbacks oder Koroutinen betroffen.

### Activity und View nicht in langlebigen Objekten festhalten

Ungünstig:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

Handelt es sich um einen Activity-Kontext, wird die Activity nach Rotation oder
`finish` nicht freigegeben. Für Abhängigkeiten auf Anwendungsebene wird
`applicationContext` verwendet.

### View Binding in einem Fragment freigeben

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Das Fragment-Objekt kann länger leben als seine View. Deshalb werden
View-Referenzen in `onDestroyView()` entfernt.

### Adapter, Listener und Callbacks entfernen

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Adapter, Listener und anonyme Callbacks halten häufig eine Referenz auf eine View
oder ein Fragment. Sie müssen im passenden Lebenszyklus-Callback abgemeldet oder
entfernt werden.

### Lebenszyklusbewusste Koroutinen verwenden

Zum Sammeln von UI-Daten in einem Fragment:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

UI-Koroutinen dürfen nicht in einem Scope gestartet werden, der länger als die
View lebt. `GlobalScope` sollte weder für UI- noch für Anwendungsabläufe verwendet
werden.

### Compose

In Compose müssen Nebenwirkungen mit `DisposableEffect` bereinigt werden:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

Eine `Activity` oder `View` sollte nicht mit `remember` festgehalten werden, wenn
es dafür keinen klaren Lebenszyklusgrund gibt.

### Werkzeuge

- LeakCanary;
- Android Studio Memory Profiler;
- Heap Dumps;
- Allocation Tracking.

Eine `WeakReference` ist keine übliche Architekturlösung. Scheint sie erforderlich
zu sein, sollten zunächst Eigentumsverhältnisse und Lebenszyklus geprüft werden.

**Kurz gesagt:** Kurzlebige Android-Objekte dürfen nicht in langlebigen Klassen
festgehalten werden. Fragment-View-Referenzen werden in `onDestroyView()`
freigegeben, lebenszyklusbewusste Koroutinen verwendet, Callbacks abgemeldet und
Speicherlecks mit LeakCanary beziehungsweise dem Profiler untersucht.

</details>

<details>
<summary>100. Wie werden API-Schlüssel in einer Android-Anwendung sicher gespeichert?</summary>

#### Kotlin

Ein Geheimnis lässt sich in einer Android-Anwendung nicht vollständig sicher
speichern. Alles, was in einer APK oder einem AAB enthalten ist, kann potenziell
durch Dekompilierung, einen Proxy, Speicheranalyse oder ein gerootetes Gerät
extrahiert werden.

Die wichtigste Regel: Echte Geheimnisse gehören nicht in den Client.

Ungünstig:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Gewährt ein Schlüssel Zugriff auf Backend-, Administrator-, Zahlungs- oder
Service-Account-Funktionen, darf er ausschließlich auf dem Server liegen.

Das richtige Schema:

```text
Android-Anwendung -> eigenes Backend -> Drittanbieter-API
```

Das Backend ergänzt das Geheimnis, prüft Benutzer und Sitzung, führt Rate
Limiting sowie Auditing durch und gibt nur das benötigte Ergebnis zurück.

Nicht alle Schlüssel sind gleich:

```text
öffentlicher Client-Schlüssel -> darf in der Anwendung liegen, muss aber begrenzt werden
geheimer Admin-Schlüssel      -> darf nicht in der Anwendung liegen
```

Ist ein Client-Schlüssel dennoch in der Anwendung enthalten, muss er begrenzt
werden durch:

- Paketnamen;
- SHA-256-Zertifikatfingerabdruck;
- erlaubte APIs;
- Quoten und Rate Limits;
- Trennung der Umgebungen;
- Überwachung;
- Rotationsplan.

`BuildConfig`, `local.properties` oder CI-Secrets verhindern, dass ein Schlüssel
in Git eingecheckt wird, schützen ihn aber nicht in der kompilierten Anwendung:

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

Nach der Dekompilierung ist ein solcher Schlüssel sichtbar.

NDK, R8/ProGuard und String-Obfuscation erschweren lediglich die Extraktion. Sie
stellen eine Hürde, aber keine Sicherheitsgrenze dar.

Der Android Keystore eignet sich für auf dem Gerät erzeugte Schlüssel und die
Speicherung von Benutzer-Tokens. Er schützt jedoch kein fest eingebautes
API-Geheimnis, da ein Angreifer die Stelle finden kann, an der die Anwendung es
verwendet.

Access- und Refresh-Tokens eines Benutzers müssen sorgfältig gespeichert werden:

- EncryptedSharedPreferences oder ein verschlüsselter DataStore;
- Android Keystore für den kryptografischen Schlüssel;
- kurze Lebensdauer des Access-Tokens;
- Rotation des Refresh-Tokens;
- Löschen des Speichers beim Abmelden;
- Widerrufsmöglichkeit im Backend.

**Kurz gesagt:** Echte API-Geheimnisse müssen im Backend liegen. Im Client dürfen
nur eingeschränkte öffentliche Schlüssel oder Client-Schlüssel enthalten sein.
`BuildConfig`, Obfuscation und NDK garantieren keine Sicherheit; erforderlich
sind Beschränkungen, Überwachung und Rotation.

</details>

<details>
<summary>101. Wie wird eine Offline-First-Architektur umgesetzt?</summary>

#### Kotlin

Offline First bedeutet: Die UI liest aus einer lokalen maßgeblichen Datenquelle,
während das Netzwerk diese lediglich synchronisiert.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Lesepfad

Die UI abonniert Room:

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

Eine Aktualisierung lädt entfernte Daten und führt sie innerhalb einer Transaktion
mit der Datenbank zusammen:

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

Nach dem Schreiben in Room wird die UI über `Flow` aktualisiert. `replaceAll` ist
gefährlich, wenn die Antwort nur einen Teil der Daten enthält oder noch lokale
Änderungen ausstehen.

### Schreibpfad und Outbox

Eine Offline-Änderung wird zunächst lokal gespeichert:

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

Die UI sieht sofort eine optimistische Aktualisierung. Die Outbox-Operation wird
später gesendet und erst nach der Bestätigung durch den Server entfernt.

Wichtig: Lokale Aktualisierung und Eintrag in die Outbox müssen in derselben
Transaktion erfolgen. Andernfalls kann das Beenden des Prozesses die Operation
verloren gehen lassen.

### Synchronisierung

Für Wiederholungsversuche und Widerstandsfähigkeit gegen das Beenden des Prozesses
wird WorkManager verwendet:

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

Die Synchronisierung muss idempotent sein, da ein Wiederholungsversuch dieselbe
Anfrage erneut senden kann. Aktualisierung im Vordergrund und Worker müssen
dieselbe Synchronisierungsimplementierung verwenden.

### Konflikte

Die Konfliktrichtlinie hängt von der Domäne ab:

- Server gewinnt;
- Client gewinnt;
- Revision oder ETag;
- Zusammenführung auf Feldebene;
- manuelle Auflösung.

Ein Client-Zeitstempel ist aufgrund möglicher Zeitabweichungen unzuverlässig. In
kritischen Domänen ist einfaches Last-Write-Wins häufig nicht vertretbar.

### UI-Zustand

Die UI muss unterscheiden zwischen:

- Daten aus dem Cache;
- anfänglichem Laden ohne lokale Daten;
- Aktualisierung mit älteren Daten;
- Offline-Zustand;
- ausstehenden oder fehlgeschlagenen Änderungen;
- letzter erfolgreicher Synchronisierung.

Die Konnektivität ist lediglich ein Hinweis. Maßgeblich ist das Ergebnis einer
tatsächlichen Anfrage.

**Kurz gesagt:** Offline First basiert auf einer lokalen maßgeblichen Datenquelle.
Lesevorgänge erfolgen aus Room, entfernte Daten werden mit der Datenbank
zusammengeführt, Schreibvorgänge laufen über eine transaktionale Outbox,
WorkManager führt die Synchronisierung aus und die Domäne bestimmt die
Konfliktrichtlinie.

</details>

<details>
<summary>102. Was ist die MVI-Architektur und wann sollte sie verwendet werden?</summary>

#### Kotlin

MVI ist ein Architekturansatz mit unidirektionalem Datenfluss:

```text
UI -> Event/Intent -> ViewModel/Reducer -> State -> UI
                                      └-> Effect
```

Die UI verändert den Zustand nicht direkt. Sie rendert einen unveränderlichen
`State` und sendet ein `Event`. Das ViewModel verarbeitet das Ereignis und
erzeugt einen neuen Zustand.

Typische Bestandteile:

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

- `State` ist das dauerhafte Bildschirmmodell;
- `Event` oder `Intent` eine Benutzeraktion oder Systemeingabe;
- `Reducer` beziehungsweise `Handler` der Übergang vom alten zum neuen Zustand;
- `Effect` eine einmalige Aktion wie Navigation, Snackbar oder
  Berechtigungsanfrage.

Das ViewModel:

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

Der Zustand wird mit `copy` beziehungsweise `update` aktualisiert:

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

Die Compose-UI liest ausschließlich den Zustand und sendet Ereignisse:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Vorteile:

- eine maßgebliche Quelle für den Bildschirmzustand;
- vorhersehbare Zustandsübergänge;
- Reducer und ViewModel lassen sich leichter testen;
- gut für Compose geeignet;
- Ereignisse und Zustände lassen sich bequem protokollieren.

Nachteile:

- mehr wiederkehrender Code;
- der Reducer kann zu einem God Object werden;
- einmalige Effekte benötigen eine eindeutige Zustellungsrichtlinie;
- MVI löst Nebenläufigkeit und Race Conditions nicht automatisch;
- für einfaches CRUD kann der Ansatz überdimensioniert sein.

MVI eignet sich für komplexe Bildschirme mit vielen Zustandsübergängen,
Wiederholungsversuchen, optimistischen Aktualisierungen, Validierung oder
mehrstufigen Abläufen. Für einen einfachen Bildschirm genügen
`ViewModel + StateFlow + Methoden`.

**Kurz gesagt:** MVI ist der Zyklus `Event -> Zustandsübergang -> Rendern`. Der
Ansatz ist für komplexe zustandsbehaftete UIs nützlich, erfordert jedoch Disziplin
bei Effekten und Nebenläufigkeit und kann für einfache Bildschirme zu aufwendig
sein.

</details>

<details>
<summary>103. Wie werden skalierbare Android-Anwendungen entworfen?</summary>

#### Kotlin

Eine skalierbare Android-Anwendung ermöglicht es, ein Feature lokal zu ändern,
ohne kaskadenartige Anpassungen in der gesamten Codebasis auszulösen.

Grundlegende Schichten:

```text
UI -> ViewModel -> Domäne -> Repository -> API/DB
```

- Die UI rendert den Zustand.
- Das ViewModel verwaltet Bildschirmzustand und Ereignisse.
- Die Domäne enthält Geschäftsregeln.
- Das Repository verbirgt Datenquellen.
- Die Datenschicht arbeitet mit Retrofit, Room oder SDKs.

Schichten werden nicht mechanisch hinzugefügt. Ein Use Case oder eine
Schnittstelle ist erforderlich, wenn eine Geschäftsregel, eine Systemgrenze, eine
alternative Implementierung oder ein eigenständiger Testbedarf besteht.

### Feature-orientierte Module

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Ein Feature-Modul gruppiert Code, der sich gemeinsam ändert. `core` sollte
tatsächlich gemeinsam genutzte Infrastruktur und keine zufälligen Hilfsfunktionen
enthalten.

Abhängigkeitsregel:

```text
app -> feature
feature -> core
core -X-> feature
```

Ein Feature darf nicht von der Implementierung eines anderen Features abhängen.
Für die Integration eignen sich Verträge, eine Navigations-API oder eine Trennung
in API und Implementierung besser.

### Verträge und Modelle

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Eine Schnittstelle ist an einer tatsächlichen Systemgrenze erforderlich. Es
sollte nicht vorsorglich für jede Klasse eine Schnittstelle erstellt werden.

DTO, Entity, Domänenmodell und UI-Modell werden getrennt, wenn sie unterschiedliche
Semantik oder Lebenszyklen besitzen. Sind die Modelle identisch und haben keine
unterschiedlichen Änderungsgründe, schadet unnötiges Mapping lediglich.

### Zustand, Fehler und Nebenläufigkeit

Die UI sollte einen einzigen beobachtbaren Zustand erhalten und Ereignisse
senden. Das ViewModel muss eine eindeutige Nebenläufigkeitsrichtlinie festlegen:
vorherige Arbeit abbrechen, Duplikate ignorieren, serialisieren oder
zusammenführen.

Ein unverarbeitetes `Throwable` darf kein UI-Vertrag sein. Infrastrukturfehler
müssen an einer Systemgrenze in Domänen- oder UI-Ergebnisse umgewandelt werden.

### Dependency Injection, Tests und Build

- Konstruktorinjektion als Standard;
- Scopes entsprechen dem Lebenszyklus ihres Eigentümers;
- Domänen- und Unittests ohne Android;
- Tests des ViewModel-Zustands;
- Integrationstests für Repositories;
- Convention Plugins und Version Catalog;
- CI für betroffene Module;
- Protokollierung, Metriken und Absturzkontext;
- Migrationsplan für Änderungen an API oder Datenbank.

Modularität ohne Tests, Observability und verständliche Zuständigkeiten skaliert
das Produkt nicht.

**Kurz gesagt:** Skalierbarkeit entsteht durch Feature-Zuständigkeiten, einen
kontrollierten Abhängigkeitsgraphen, stabile Verträge, testbare Systemgrenzen,
expliziten Zustand und Nebenläufigkeit sowie Unterstützung im Produktionsbetrieb.
Module und Schichten werden nur hinzugefügt, wenn sie Änderungen tatsächlich
isolieren.

</details>

<details>
<summary>104. Was ist Jetpack Compose und wie unterscheidet es sich vom View-System?</summary>

#### Kotlin

`Jetpack Compose` ist ein deklaratives UI-Toolkit für Android. Die UI wird mit
Kotlin-`@Composable`-Funktionen als Ergebnis des aktuellen Zustands beschrieben,
ohne XML und ohne manuelle Aktualisierung von Views.

Das View-System arbeitet imperativ:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

Compose arbeitet deklarativ:

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

Ändert sich der Zustand, führt Compose für die erforderlichen Teile der UI eine
Recomposition aus.

Eine Composable-Funktion:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Ein Composable sollte Zustand rendern und Callbacks aufrufen. Geschäftslogik,
Ein- und Ausgabe sowie lang laufende Operationen gehören nicht in UI-Funktionen.

Ein typisches Schema:

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

Das ViewModel bildet den Zustand, Compose stellt ihn dar.

Die wichtigsten Unterschiede:

```text
View-System -> XML + View-Baum + manuelle Aktualisierungen + Listener
Compose     -> Kotlin-Funktionen + zustandsabhängiges Rendering + Recomposition
```

Praktische Besonderheiten von Compose:

- Der Rumpf eines Composables kann mehrfach ausgeführt werden.
- Nebenwirkungen werden über Effect-APIs gestartet.
- Lokaler Zustand und Objekte werden bei Bedarf mit `remember` gespeichert.
- Der Bildschirmzustand wird im ViewModel gehalten.
- Lazy-Listen benötigen stabile Schlüssel.
- Die Performance wird mit einem Profiler geprüft und nicht anhand der Zahl der
  Recompositions beurteilt.

Compose und das View-System können kombiniert werden:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

Umgekehrt kann mit `AndroidView` eine View in Compose eingebettet werden. Eine
Migration kann daher schrittweise erfolgen.

**Kurz gesagt:** Compose beschreibt die UI als Funktion des Zustands und
aktualisiert sie durch Recomposition. Das View-System erfordert XML, einen
View-Baum und manuelle Änderungen an Views. Compose unterstützt zustandsabhängige
UIs besser, verlangt jedoch Disziplin bei Nebenwirkungen und der Zuständigkeit
für den Zustand.

</details>

<details>
<summary>105. Was sind @Composable und Recomposition?</summary>

#### Kotlin

`@Composable` ist eine Annotation für Jetpack-Compose-Funktionen, die eine UI
beschreiben. Recomposition bezeichnet die erneute Ausführung eines Composables,
wenn sich ein Zustand ändert, von dem es abhängt.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Ein Composable ist eine Funktion des Zustands:

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

Ändert sich `state`, aktualisiert Compose den erforderlichen Teil der UI durch
Recomposition.

Ein Beispiel für lokalen Zustand:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

Eine Änderung von `count` löst die Recomposition des UI-Teils aus, der `count`
liest.

Recomposition kann ausgelöst werden durch:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable`;
- geänderte Parameter eines Composables.

Der Rumpf eines Composables kann mehrfach ausgeführt werden. Deshalb dürfen dort
keine Nebenwirkungen stattfinden:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // ungünstig
}
```

Korrekt:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` bewahrt einen Wert zwischen Recompositions auf:

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

Recomposition ist an sich kein Problem. Problematisch sind aufwendige Arbeit in
einem Composable, zu breit gelesener Zustand, instabile Parameter, unnötige
Allokationen oder Lazy-Listen ohne stabile Schlüssel.

Praktische Regeln:

- Ein Composable muss leichtgewichtig und frei von Nebenwirkungen sein.
- Zustand sollte möglichst nahe am Verwendungsort gelesen werden.
- Aufwendig zu erzeugende Objekte werden mit `remember` gespeichert.
- Effekte werden mit `LaunchedEffect`, `DisposableEffect` und ähnlichen APIs
  gestartet.
- Listen verwenden stabile Schlüssel.
- Die Performance wird mit einem Profiler und nicht anhand von Vermutungen
  geprüft.

**Kurz gesagt:** `@Composable` beschreibt die UI als Funktion des Zustands.
Recomposition führt die erforderlichen Composables nach einer Zustandsänderung
erneut aus. Dies ist ein normaler Compose-Mechanismus, sofern UI-Funktionen
leichtgewichtig, frei von Nebenwirkungen und mit sinnvoll platziertem
Zustandszugriff gestaltet sind.

</details>

<details>
<summary>106. Was ist State Hoisting?</summary>

#### Kotlin

State Hoisting bezeichnet das Verschieben eines Zustands aus einem Composable in
ein übergeordnetes Element oder einen State Holder. Das untergeordnete Element
erhält den Wert und einen Callback und wird dadurch zustandslos,
wiederverwendbar und testbar.

Eine zustandsbehaftete Variante:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Nach dem Hoisting:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

Das übergeordnete Element wird zur maßgeblichen Quelle:

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

Bei einem Zustand auf Bildschirmebene ist üblicherweise das `ViewModel` der
Eigentümer:

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

Die Compose-Route liest den Zustand und reicht Ereignisse weiter:

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

Hoisting unterstützt einen unidirektionalen Datenfluss:

```text
Zustand nach unten -> UI
Ereignisse nach oben -> Eigentümer aktualisiert den Zustand
```

Der Zustand sollte bis zum niedrigsten gemeinsamen Eigentümer angehoben werden,
der ihn lesen oder verändern muss. Wird ein Zustand ausschließlich innerhalb
einer Komponente benötigt und beeinflusst keine Geschäftslogik, kann er lokal
bleiben.

Eine typische API eines zustandslosen Composables:

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**Kurz gesagt:** State Hoisting bedeutet „Zustand nach unten, Ereignisse nach
oben“. Das untergeordnete Element besitzt den Zustand nicht; das übergeordnete
Element oder ViewModel wird zur einzigen maßgeblichen Quelle.

</details>

<details>
<summary>107. Worin besteht der Unterschied zwischen remember und rememberSaveable?</summary>

#### Kotlin

`remember` und `rememberSaveable` bewahren Werte zwischen Recompositions auf.
`rememberSaveable` speichert den Wert zusätzlich im Saved State und kann ihn nach
einer Konfigurationsänderung oder Neuerstellung des Prozesses wiederherstellen.

```text
remember         -> übersteht Recomposition
rememberSaveable -> übersteht Recomposition und Neuerstellung
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

Nach einer Neuerstellung der Activity geht der Wert verloren.

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

Unter Android werden Typen automatisch unterstützt, die in einem Saved-State-
`Bundle` abgelegt werden können, darunter primitive Typen, `String`, `Parcelable`
und `Serializable`. Für einen benutzerdefinierten Typ ist ein `Saver` erforderlich:

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

Wann welcher Mechanismus verwendet wird:

- `remember` für vorübergehenden UI-Zustand oder ein zwischengespeichertes Objekt,
  das verloren gehen darf;
- `rememberSaveable` für kleinen UI-Zustand, dessen Erhalt der Benutzer erwartet,
  etwa Eingabetext, ausgewählter Tab oder Filter;
- `ViewModel` für Bildschirm- und Geschäftszustand;
- Speicher, DataStore oder Datenbank für dauerhafte Daten.

Einschränkungen von `rememberSaveable`:

- Der Saved State besitzt eine begrenzte Größe.
- Große Objekte und Collections dürfen darin nicht gespeichert werden.
- Er stellt keine langfristige Persistenz dar.
- Er ersetzt weder `ViewModel` noch Repository.

**Kurz gesagt:** `remember` lebt ausschließlich in der Composition.
`rememberSaveable` stellt kleinen speicherbaren UI-Zustand nach einer
Neuerstellung wieder her. Für Bildschirmzustand wird ein `ViewModel`, für
langfristige Daten die Speicherschicht benötigt.

</details>

<details>
<summary>108. Was ist rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` ist eine Compose-API, die einen an die aktuelle
Composition gebundenen `CoroutineScope` zurückgibt. Der Scope wird abgebrochen,
sobald das Composable die Composition verlässt.

Er wird verwendet, wenn eine aussetzbare UI-Aktion aus einem Callback wie Klick,
Wischgeste, Drawer, Snackbar oder Scrollanimation gestartet werden muss.

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

Eine Suspend-Funktion kann nicht direkt aus `onClick` aufgerufen werden, deshalb
ist ein Scope erforderlich.

Ein weiteres Beispiel:

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

Der Unterschied zu `LaunchedEffect`:

```text
LaunchedEffect         -> Koroutine startet automatisch beim Eintritt oder bei Schlüsseländerung
rememberCoroutineScope -> Koroutine wird manuell aus einem Callback gestartet
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

`rememberCoroutineScope()` sollte nicht für Geschäftslogik verwendet werden:

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

Im ViewModel:

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

`GlobalScope` wird nicht für die UI verwendet, da er nicht an den Lebenszyklus
gebunden ist und nach dem Schließen des Bildschirms weiterarbeiten kann.

**Kurz gesagt:** `rememberCoroutineScope()` dient kurzen aussetzbaren
UI-Operationen aus Callbacks, etwa Snackbar, Drawer oder Scrollen. Für
automatische Effekte wird `LaunchedEffect`, für Bildschirm- und Geschäftslogik
`viewModelScope` verwendet.

</details>

<details>
<summary>109. Was ist collectAsState?</summary>

#### Kotlin

`collectAsState()` ist eine Compose-API, die einen `Flow` sammelt und in einen
Compose-`State` umwandelt. Emittiert der Flow einen neuen Wert, führt Compose an
den Stellen eine Recomposition aus, an denen dieser Zustand gelesen wird.

Das ViewModel:

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

Der Ablauf:

```text
Flow emittiert -> State wird aktualisiert -> Recomposition -> UI wird aktualisiert
```

Für einen gewöhnlichen `Flow` ist ein Anfangswert erforderlich:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

Ein `StateFlow` besitzt bereits einen Anfangswert in `state.value`.

Unter Android sollte die lebenszyklusbewusste Variante verwendet werden:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Der Unterschied:

```text
collectAsState              -> Compose Runtime, ohne Android-Lebenszyklus
collectAsStateWithLifecycle -> sammelt nur in einem aktiven Lebenszykluszustand
```

Ein Flow darf nicht direkt im Rumpf eines Composables gesammelt werden:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // ungünstig
}
```

Für Bildschirmzustand wird `collectAsStateWithLifecycle()` verwendet. Für
einmalige Ereignisse dienen `LaunchedEffect` und ein separater `SharedFlow`:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // Navigation oder Snackbar anzeigen
    }
}
```

Ein Zustand lässt sich wiederholt rendern; ein Ereignis ist eine einmalige Aktion.

Emittiert ein Flow zu häufig, sollte das Rauschen durch `distinctUntilChanged()`,
kleinere Zustandsmodelle und lokalen Zustandszugriff reduziert werden. Aufwendige
Arbeit gehört nicht in ein Composable.

**Kurz gesagt:** `collectAsState()` wandelt einen Flow in Compose State um. Unter
Android wird üblicherweise `collectAsStateWithLifecycle()` verwendet. Einmalige
Navigations- oder Snackbar-Ereignisse benötigen `LaunchedEffect` statt einer
Zustandssammlung.

</details>

<details>
<summary>110. Was ist Modifier in Compose?</summary>

#### Kotlin

Ein `Modifier` beschreibt in Compose Layout, Zeichnen, Eingabe, Semantik und
Verhalten eines Composables, darunter `padding`, `size`, `background`,
`clickable`, Scrollen und Barrierefreiheit.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

Ein `Modifier` ist unveränderlich und wird als Kette aufgebaut. Die Reihenfolge
ist von Bedeutung:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

und:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

erzeugen unterschiedliche Ergebnisse, da der Hintergrund in einer anderen Phase
angewendet wird.

Ein wiederverwendbares Composable sollte einen Modifier von außen annehmen:

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

Dadurch kann das übergeordnete Element Positionierung und äußeres Verhalten des
untergeordneten Elements steuern.

Die wichtigsten Modifier-Arten:

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

Für Barrierefreiheit sind höherstufige Modifier wie `clickable` vorzuziehen, da
sie Semantik hinzufügen. Bei Bedarf kann sie ausdrücklich definiert werden:

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Einige Modifier funktionieren nur im Scope des übergeordneten Elements:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Praktische Regeln:

- `modifier: Modifier = Modifier` als Parameter annehmen;
- das übergeordnete Element ist für die äußere Positionierung zuständig;
- das untergeordnete Element kann dem übergebenen Modifier interne Modifier
  nachstellen;
- die Reihenfolge der Modifier ist wichtig;
- in Modifier-Callbacks darf keine aufwendige Logik ausgeführt werden.

**Kurz gesagt:** Ein `Modifier` ist eine unveränderliche Kette von Einstellungen
für ein Composable. Er steuert Layout, Zeichnen, Eingabe und Semantik. Die
Reihenfolge der Aufrufe ist wichtig, und wiederverwendbare Composables sollten
einen `modifier`-Parameter annehmen.

</details>

<details>
<summary>111. Worin besteht der Unterschied zwischen padding und offset?</summary>

#### Kotlin

`padding` und `offset` verändern die Position einer UI, arbeiten jedoch in
unterschiedlichen Phasen des Layouts.

```text
padding -> verändert Messung und reservierten Platz
offset  -> verändert Positionierung ohne Änderung der gemeldeten Größe
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

`padding` fügt Platz um den Inhalt hinzu und beeinflusst die Größe, die das
übergeordnete Element und benachbarte Elemente sehen.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` wird nach `A` einschließlich dessen Padding platziert.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` verschiebt das Composable bei der Positionierung, während das
übergeordnete Element weiterhin den ursprünglichen Platz reserviert.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` wird nicht verschoben, weshalb sich die Elemente überlappen können. Dies ist
für Badges, Ziehen, Animationen oder dekorative Verschiebungen nützlich.

### RTL

`offset(x, y)` berücksichtigt die Layoutrichtung: Ein positives `x` verschiebt
zum logischen Ende. Werden physische Koordinaten ohne RTL-Spiegelung benötigt,
wird `absoluteOffset()` verwendet.

### Reihenfolge der Modifier

Die Reihenfolge beeinflusst Layout, Zeichnen und Trefferbereich:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

Der Hintergrund umfasst eine größere Fläche.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

Der Hintergrund wird erst nach dem Padding angewendet.

Dasselbe gilt für `clickable`: Steht `clickable` vor `padding`, gehört das Padding
zum anklickbaren Bereich; steht es danach, nicht.

**Kurz gesagt:** `padding` fügt Layoutplatz hinzu und beeinflusst benachbarte
Elemente. `offset` verschiebt ein Composable lediglich, ohne neuen Platz zu
reservieren. Die Reihenfolge der Modifier beeinflusst Darstellung und Hit Testing
entscheidend.

</details>

<details>
<summary>112. Worin besteht der Unterschied zwischen fillMaxSize und matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` und `matchParentSize()` können ein untergeordnetes Element auf die
Größe des übergeordneten Elements bringen, funktionieren jedoch unterschiedlich.

```text
fillMaxSize()     -> übernimmt maximale Constraints und beeinflusst die Messung
matchParentSize() -> übernimmt die bereits bestimmte Größe einer Box
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

`fillMaxSize()` fordert vom übergeordneten Element die maximal verfügbare Größe
an. Sind die Constraints auf den Bildschirm begrenzt, nimmt das Composable den
gesamten Bildschirm ein.

Für eine einzelne Achse stehen zur Verfügung:

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` nimmt an der Messung teil und kann die Größe des übergeordneten
Layouts beeinflussen.

### `matchParentSize()`

`matchParentSize()` funktioniert ausschließlich in einem `BoxScope` und nur für
ein direktes Kind einer `Box`:

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

Dieses Kind übernimmt die bereits berechnete Größe der `Box`, bestimmt sie jedoch
nicht.

Vereinfacht:

1. Die `Box` misst ihre gewöhnlichen Kinder.
2. Sie bestimmt ihre eigene Größe.
3. Kinder mit `matchParentSize()` erhalten diese fertige Größe.

Deshalb eignet sich `matchParentSize()` für Overlays wie Ladeebene, Scrim,
Gradient oder anklickbare Ebene.

Wird das Overlay durch `fillMaxSize()` ersetzt, kann das Kind die maximalen
Constraints anfordern und die `Box` vergrößern. Dies ist ein anderes Verhalten.

Praktische Auswahl:

- Soll ein Root-Element oder Container den gesamten verfügbaren Platz einnehmen,
  wird `fillMaxSize()` verwendet.
- Soll ein Overlay eine vom Inhalt bestimmte `Box` abdecken, wird
  `matchParentSize()` verwendet.
- Darf ein Kind die Größe der `Box` nicht beeinflussen, eignet sich
  `matchParentSize()`.
- Außerhalb eines `BoxScope` ist `matchParentSize()` nicht verfügbar.

**Kurz gesagt:** `fillMaxSize()` fordert den maximal verfügbaren Platz an und
nimmt an der Messung teil. `matchParentSize()` übernimmt in einem `BoxScope`
lediglich die bereits bestimmte Größe der `Box`, ohne sie zu vergrößern.

</details>

<details>
<summary>113. Wie wird die Zurück-Schaltfläche in Compose behandelt?</summary>

#### Kotlin

In Compose wird das Zurück-Ereignis mit `BackHandler` aus
`androidx.activity.compose` behandelt. Dieser fängt das systemseitige
Zurück-Ereignis innerhalb eines Composables ab.

Ein grundlegendes Beispiel:

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // content
}
```

`BackHandler` kann bedingt aktiviert werden:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Bei `enabled = false` wird das Zurück-Ereignis an einen anderen Handler oder an
die Navigation weitergegeben.

Ein typischer Anwendungsfall ist ein Bestätigungsdialog für nicht gespeicherte
Änderungen:

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

Bei einem `NavController` funktioniert das gewöhnliche Zurücknavigieren häufig
automatisch über den Back Stack. Ein eigener `BackHandler` ist erforderlich, wenn
das Standardverhalten geändert werden soll, etwa für Dialog, Bottom Sheet,
Auswahlmodus oder ungespeicherte Änderungen.

Bei lokalem UI-Zustand sollte Zurück zunächst die oberste Ebene schließen:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

Die Priorität lautet:

```text
Dialog/Sheet/Auswahl -> ungespeicherte Änderungen -> Bildschirmnavigation
```

Komplexe Logik sollte als Ereignis an das ViewModel weitergegeben werden:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

Das ViewModel entscheidet, ob ein Dialog angezeigt, ein Entwurf gespeichert, die
Navigation erlaubt oder ein Effekt emittiert wird.

Typische Fehler:

- Zurück immer abfangen;
- `enabled` nicht verwenden;
- Logik zwischen `BackHandler` und `NavController` duplizieren;
- den Zustand von Dialog oder Sheet nicht berücksichtigen;
- Zurück-Logik weit entfernt vom zugehörigen Zustand platzieren.

**Kurz gesagt:** In Compose wird Zurück mit `BackHandler(enabled = ...)`
behandelt. Er eignet sich für obere UI-Ebenen und abweichendes Verhalten, während
die gewöhnliche Navigation häufig dem `NavController` überlassen wird.

</details>

<details>
<summary>114. Wofür wird rememberUpdatedState verwendet?</summary>

#### Kotlin

`rememberUpdatedState()` stellt einem langlebigen Effekt einen aktuellen Wert
oder Callback bereit, ohne den Effekt neu zu starten.

Das Problem:

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` wird bei einer Änderung von `onTimeout` nicht neu
gestartet. Die Koroutine kann daher ein veraltetes Lambda aufrufen.

Die Lösung:

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

Der Timer beginnt nicht erneut, nach der Verzögerung wird jedoch der aktuellste
Callback aufgerufen.

Die wichtigste Regel:

```text
Soll eine Wertänderung den Effekt neu starten?       -> Schlüssel
Soll eine Wertänderung den Effekt nicht neu starten? -> rememberUpdatedState
```

Wird der Callback als Schlüssel verwendet:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

dann wird bei einer Änderung des Lambdas die vorherige Koroutine abgebrochen und
der Timer beginnt von vorn. Dies ist nicht immer erwünscht.

Ein Beispiel mit `DisposableEffect`:

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

Der Observer wird nur bei einer Änderung von `owner` neu registriert, ruft aber
stets den aktuellen Handler auf.

Wann ein Wert ein Schlüssel sein sollte:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Eine Änderung von `userId` bedeutet eine neue Operation. Die alte muss daher
abgebrochen und eine neue gestartet werden.

`rememberUpdatedState()` startet keine Koroutine, ersetzt keine Effektschlüssel
und bewahrt den Wert nach dem Verlassen der Composition nicht auf. Die Funktion
gibt lediglich einen `State<T>` mit dem aktuellen Wert für einen langlebigen
Effekt zurück.

**Kurz gesagt:** `rememberUpdatedState()` sorgt dafür, dass ein Effekt den
aktuellsten Wert oder Callback sieht, ohne neu gestartet zu werden. Bedeutet eine
Wertänderung eine neue Operation, muss der Wert ein Schlüssel sein und darf nicht
durch `rememberUpdatedState` ersetzt werden.

</details>

<details>
<summary>115. Was sind Side Effects in Compose und wie werden sie behandelt?</summary>

#### Kotlin

Ein Side Effect in Compose ist eine Aktion außerhalb des reinen UI-Renderings,
etwa eine Koroutine, Snackbar, Navigation, Analyse, ein Listener,
Lebenszyklus-Observer oder die Synchronisierung mit einer externen API.

Der Rumpf eines Composables kann mehrfach ausgeführt werden. Deshalb darf ein
Side Effect dort nicht direkt gestartet werden:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // ungünstig
}
```

Dafür stehen Effect-APIs zur Verfügung.

### `LaunchedEffect`

Startet eine an die Composition gebundene Koroutine:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Ändert sich `userId`, wird die alte Koroutine abgebrochen und eine neue gestartet.

### `rememberCoroutineScope`

Wird für aussetzbare UI-Aktionen aus einem Callback benötigt:

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

Dies eignet sich für Snackbar, Drawer, Bottom Sheet und Scrollanimation.

### `DisposableEffect`

Für Listener und Observer mit Bereinigung:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

Wurde etwas abonniert, muss es in `onDispose` wieder abgemeldet werden.

### `SideEffect`

Wird nach einer erfolgreichen Recomposition ausgeführt:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Die API eignet sich nicht für aussetzbare Arbeit.

### Weitere APIs

- `rememberUpdatedState` stellt einem langlebigen Effekt den aktuellsten Wert
  oder Callback ohne Neustart bereit;
- `produceState` wandelt eine asynchrone Quelle in Compose-`State` um;
- `snapshotFlow` wandelt Compose-Zustand in einen Flow um.

Eine praktische Zuordnung:

```text
Koroutine der Composition        -> LaunchedEffect
Koroutine aus einem Callback     -> rememberCoroutineScope
Listener mit Bereinigung         -> DisposableEffect
Synchronisierung nach Recompose  -> SideEffect
aktueller Callback ohne Neustart -> rememberUpdatedState
asynchrone Quelle zu State       -> produceState
Compose State zu Flow            -> snapshotFlow
```

**Kurz gesagt:** Side Effects werden nicht im Rumpf eines Composables gestartet.
Effect-APIs machen Lebenszyklus, Neustart, Abbruch und Bereinigung ausdrücklich
kontrollierbar.

</details>

<details>
<summary>116. Was sind Schlüssel in LaunchedEffect und warum sind sie wichtig?</summary>

#### Kotlin

Schlüssel in `LaunchedEffect` bestimmen den Lebenszyklus des Koroutineneffekts:
wann er gestartet, abgebrochen und neu gestartet wird. Ändert sich ein Schlüssel,
bricht Compose die alte Koroutine ab und startet eine neue.

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

Das Verhalten:

- Beim Eintritt des Composables in die Composition startet der Effekt.
- Eine Recomposition ohne Änderung von `userId` startet ihn nicht neu.
- Ändert sich `userId`, wird die alte Koroutine abgebrochen und eine neue
  gestartet.
- Verlässt das Composable die Composition, wird die Koroutine abgebrochen.

Ein typischer Fehler:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Ändert sich `userId`, wird der Effekt nicht neu gestartet und kann veraltete Daten
liefern. Korrekt:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` ist angemessen, wenn ein Effekt beim Eintritt in die
Composition einmal gestartet werden soll:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // Navigation oder Snackbar
    }
}
```

Mehrere Schlüssel:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

Der Effekt wird neu gestartet, sobald sich mindestens ein Schlüssel ändert.
Alles, was das Ergebnis des Side Effects bestimmt, muss ein Schlüssel sein.

Instabile Schlüssel dürfen nicht verwendet werden:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Dadurch würde der Effekt unkontrolliert neu gestartet.

Wird innerhalb des Effekts der neueste Wert benötigt, ohne ihn neu zu starten,
kommt `rememberUpdatedState` zum Einsatz:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Eine praktische Regel:

```text
Wertänderung soll Effekt neu starten -> Schlüssel
aktueller Wert ohne Neustart nötig   -> rememberUpdatedState
```

**Kurz gesagt:** Schlüssel in `LaunchedEffect` steuern Start, Abbruch und Neustart
einer Koroutine. Falsche Schlüssel führen zu veralteten Daten, unnötigen
Neustarts oder instabilem UI-Verhalten.

</details>

<details>
<summary>117. Was ist snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` ist eine Compose-API, die Lesezugriffe auf den
Compose-Snapshot-Zustand in einen kalten `Flow` umwandelt. Sie wird verwendet,
wenn auf Änderungen des Compose-Zustands mit Flow-Operatoren oder einer
Side-Effect-Pipeline reagiert werden soll.

Ein grundlegendes Beispiel:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` verfolgt Lesezugriffe auf Compose-Zustand innerhalb des Lambdas.
Ändert sich der gelesene Wert, emittiert der Flow einen neuen Wert.

Typische Anwendungsfälle:

- Scroll-Analyse;
- Auslösen der Pagination;
- Debounce oder Sampling für UI-Zustand;
- Protokollierung und Debugging;
- Side Effects, die nicht im Rumpf eines Composables gestartet werden dürfen.

Ein Pagination-Beispiel:

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

Da das Ergebnis ein `Flow` ist, können Operatoren verwendet werden:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

`snapshotFlow` wird üblicherweise innerhalb eines `LaunchedEffect` gesammelt. Die
Sammlung darf nicht direkt im Rumpf eines Composables gestartet werden.

Für gewöhnliches UI-Rendering wird `snapshotFlow` nicht benötigt:

```kotlin
Text(text = state.title)
```

Soll ein Zustand lediglich dargestellt werden, wird er direkt gelesen.
`snapshotFlow` dient gezielt dem Übergang von Compose-Zustand zu Flow- oder
Side-Effect-Logik.

Ein wichtiger Punkt: Das Lambda muss einen Compose-Snapshot-Zustand lesen. Ohne
einen solchen Lesezugriff besitzt `snapshotFlow` nichts, was es verfolgen könnte.

**Kurz gesagt:** `snapshotFlow` wandelt Lesezugriffe auf Compose-Zustand in einen
`Flow` um. Es eignet sich für Analyse, Pagination, Debounce und Side Effects in
`LaunchedEffect`, ist aber für einfaches UI-Rendering nicht erforderlich.

</details>

<details>
<summary>118. Wie wird Recomposition in Compose optimiert?</summary>

#### Kotlin

Recomposition zu optimieren bedeutet nicht, sämtliche Recompositions zu
vermeiden, sondern sie günstig und lokal zu halten. Recomposition ist normal;
problematisch sind aufwendige Arbeit oder ein zu großer Aktualisierungsbereich.

### Zustand möglichst nahe am Verwendungsort lesen

Ungünstig:

```kotlin
ProfileScreen(state = hugeState)
```

Besser:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Liest ein Composable einen Zustand, kann es bei dessen Änderung neu ausgeführt
werden. Der gesamte Bildschirmzustand sollte nicht an eine Stelle übergeben
werden, die nur ein einziges Feld benötigt.

### Lazy-Listen: Schlüssel und contentType

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

Stabile Schlüssel helfen Compose, Elemente korrekt zuzuordnen und ihren Zustand
zu bewahren.

Für unterschiedliche Elementtypen:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### Keine aufwendige Arbeit in einem Composable

Ungünstig:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Besser:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Noch besser ist es, formatierten Text in einem Mapper oder ViewModel
vorzubereiten, sofern er Bestandteil des UI-Modells ist.

### `derivedStateOf` bei häufigen Änderungen verwenden

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Dies ist nützlich, wenn sich der Ausgangszustand häufig ändert, die UI aber nur
auf eine abgeleitete Bedingung reagieren soll.

### Stabile und unveränderliche Modelle

Compose arbeitet besser mit unveränderlichen UI-Modellen:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Veränderliche Collections und instabile Objekte können Compose daran hindern,
unnötige Recompositions zu überspringen.

### Side Effects ausschließlich über Effect-APIs

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

Der Rumpf eines Composables kann mehrfach ausgeführt werden. Analyse, Navigation,
Listener und aussetzbare Arbeit dürfen deshalb nicht direkt darin stattfinden.

### Durch Profiling prüfen

- Recomposition-Zähler im Layout Inspector;
- Compose-Compiler-Metriken;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

**Kurz gesagt:** Recomposition wird durch lokalen Zustandszugriff, kleinere
Composables, stabile Schlüssel, `contentType`, unveränderliche UI-Modelle,
`remember`, `derivedStateOf`, korrekte Effekte und Profiling optimiert. Das Ziel
sind günstige lokale Aktualisierungen und nicht null Recompositions.

</details>

<details>
<summary>119. Wie werden Probleme mit Recomposition debuggt?</summary>

#### Kotlin

Probleme mit Recomposition werden durch Messungen und nicht durch Vermutungen
untersucht. Ziel ist es herauszufinden, welcher Zustand sich häufig ändert und
welchen Teil der UI er beeinflusst.

### Layout Inspector

Der Android Studio Layout Inspector zeigt Recomposition-Zähler für Composables.
Die Anzahl allein stellt jedoch kein Problem dar: Ein leichtgewichtiges
Composable kann häufig neu ausgeführt werden, ohne eine sichtbare Auswirkung zu
haben.

### Temporäre Protokolle

Für eine schnelle Prüfung kann ein Log hinzugefügt werden:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Oder mit `SideEffect`:

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

Dies ist ausschließlich ein Debugging-Werkzeug und kein Produktionscode.

### Zustandszugriffe prüfen

Eine häufige Ursache ist, dass ein Composable einen zu umfangreichen Zustand
liest:

```kotlin
UserRow(screenState = state)
```

Besser:

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

Zustand sollte möglichst nahe am Verwendungsort gelesen werden.

### Listen und Parameter prüfen

Eine `LazyColumn` benötigt stabile Schlüssel:

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

Außerdem sind instabile Parameter zu prüfen: veränderliche Collections, unnötig
neu erzeugte Objekte oder Lambdas sowie aufwendige Arbeit im Rumpf eines
Composables.

### Compiler-Metriken und Profiler

Compose-Compiler-Metriken zeigen, welche Composables und Typen instabil,
neustartbar oder überspringbar sind. Android Studio Profiler und Macrobenchmark
sind erforderlich, um die tatsächliche Auswirkung auf die Performance zu
bestätigen.

Eine praktische Checkliste:

- Zähler im Layout Inspector ansehen;
- häufig veränderten Zustand finden;
- Zustandszugriffe eingrenzen;
- stabile Schlüssel in Lazy-Listen prüfen;
- aufwendige Arbeit aus dem Rumpf entfernen;
- instabile Parameter prüfen;
- `remember` und `derivedStateOf` nur mit konkretem Grund einsetzen;
- das Ergebnis mit Profiler und Metriken bestätigen.

**Kurz gesagt:** Recomposition wird mit Layout Inspector, Protokollen,
Compiler-Metriken und Profiler debuggt. Häufige Ursachen sind zu breite
Zustandszugriffe, instabile Parameter, fehlende Schlüssel, aufwendige Arbeit im
Rumpf oder ungeeignete Zustandszuständigkeit.

</details>

<details>
<summary>120. Wie lassen sich Ruckler in Compose, etwa beim Scrollen, vermeiden?</summary>

#### Kotlin

Ruckler beim Scrollen werden meist durch aufwendige Arbeit im Hauptthread,
unnötige Recompositions, Allokationen in Listenelementen, instabile Schlüssel oder
eine zu komplexe Zeilen-UI verursacht.

### Stabile Schlüssel und contentType

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

Ein `key` bewahrt die Identität eines Elements nach Einfügen, Löschen oder
Umsortieren.

Für unterschiedliche Elementtypen:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` hilft Compose, die Composition kompatibler Elemente
wiederzuverwenden.

### Keine aufwendige Arbeit in einem Element

Ungünstig:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Formatierter Text sollte besser in einem Mapper oder ViewModel vorbereitet oder
zumindest das Objekt mit `remember` gespeichert werden. Ein- und Ausgabe,
Bitmap-Decodierung, Sortierung, Filterung und aufwendige Berechnungen dürfen nicht
in einem Composable stattfinden.

### Zustandszugriffe eingrenzen

Ein Element sollte nur seine eigenen Daten erhalten:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Der gesamte Bildschirmzustand sollte nicht an jede Zeile übergeben werden.

### Stabile UI-Modelle

Es sollten unveränderliche UI-Modelle verwendet und nicht bei jeder Recomposition
unnötig neue Collections oder Wrapper erzeugt werden.

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

### Vom Scrollzustand abgeleiteter Zustand

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

`derivedStateOf` ist nützlich, wenn sich der Scrollzustand häufig ändert, die UI
aber nur auf eine abgeleitete Bedingung reagieren soll.

### Bilder und Paging

Für Bilder wird ein Loader mit Speicher- und Festplattencache sowie passender
Zielgröße verwendet. Bitmaps dürfen weder manuell in einem Composable noch im
Hauptthread decodiert werden.

Für große Listen wird Paging eingesetzt:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Profiling

Optimierungen werden bestätigt durch:

- Recomposition-Zähler im Layout Inspector;
- Android Studio Profiler oder System Trace;
- Macrobenchmark in einem releaseähnlichen Build;
- Baseline Profiles.

**Kurz gesagt:** Flüssiges Scrollen in Compose erfordert stabile Schlüssel,
`contentType`, enge Zustandszugriffe, günstige Elemente, unveränderliche Modelle,
gecachete Bilder, Paging und eine Prüfung mit dem Profiler. Entscheidend ist, den
Hauptthread nicht zu blockieren und während der Composition keine aufwendige
Arbeit auszuführen.

</details>

<details>
<summary>121. Worin unterscheiden sich die Annotationen Stable und Immutable?</summary>

#### Kotlin

`@Stable` und `@Immutable` sind in Compose Verträge für Compiler und Runtime, die
bei der Entscheidung helfen, ob eine Recomposition übersprungen werden kann. Sie
sind keine magische Optimierung, sondern das Versprechen eines bestimmten
Typverhaltens.

Der wichtigste Unterschied:

```text
@Immutable -> Objekt ändert sich nach der Erstellung nicht
@Stable    -> Objekt kann sich ändern, Compose erkennt die Änderungen jedoch
```

### `@Immutable`

Eignet sich für Wert- und UI-Modelle:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Voraussetzung: Sämtliche öffentlichen Eigenschaften bleiben nach der Erstellung
unverändert, und auch ihre Felder sind unveränderlich oder stabil.

Ungünstig:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

Eine `MutableList` kann direkt verändert werden, ohne dass Compose die Änderung
zwangsläufig erkennt.

Besser:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Für strengere Garantien können unveränderliche Collections verwendet werden.

### `@Stable`

Eignet sich für State Holder oder Controller-Objekte, deren Änderungen über
beobachtbaren Compose-Zustand erfolgen:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Ein veränderliches Objekt ist zulässig, da Compose die Änderung von `count`
erkennt.

Ungünstig:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Ein gewöhnliches veränderliches Feld meldet seine Änderung nicht an Compose.

Wann welche Annotation verwendet wird:

- `@Immutable` für UI-Modelle, Wertobjekte und Data Classes des
  Bildschirmzustands;
- `@Stable` für benutzerdefinierte State Holder mit `mutableStateOf`, Controller
  und Objekte mit kontrollierter beobachtbarer Mutation.

Typische Fehler:

- ein veränderliches Objekt als `@Immutable` markieren;
- `@Stable` auf eine Klasse mit gewöhnlichen veränderlichen Feldern setzen;
- `MutableList` oder `MutableMap` im UI-Zustand halten;
- alten Zustand verändern, anstatt einen neuen zu erzeugen;
- Annotationen als Ersatz für ungeeignete Zustandszuständigkeit verwenden.

**Kurz gesagt:** `@Immutable` bedeutet, dass ein Objekt nach der Erstellung
unverändert bleibt. `@Stable` bedeutet, dass es sich ändern kann, Compose die
Änderungen aber zuverlässig erkennt. Eine Annotation darf nur gesetzt werden,
wenn der Typ den entsprechenden Vertrag tatsächlich erfüllt.

</details>

<details>
<summary>122. Warum wird ein key an LazyColumn übergeben und was stellt er intern dar?</summary>

#### Kotlin

Ein `key` in einer `LazyColumn` weist einem Element unabhängig von seiner Position
eine stabile Identität zu.

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

Ohne Schlüssel verwendet Compose die Position. Für eine statische Liste ist dies
ausreichend, bei Einfügen, Löschen oder Umsortieren jedoch problematisch.

```text
Vorher:     Nach Einfügen:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

Anhand der Position sieht Compose neuen Inhalt in alten Slots. Anhand von
`user.id` erkennt es, dass die vorhandenen Elemente lediglich ihre Position
geändert haben.

### Gespeicherter Zustand

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

Ohne stabilen Schlüssel kann `expanded` an der Position verbleiben und auf einen
anderen Benutzer übergehen. Der Schlüssel verschiebt gespeicherten und
speicherbaren Zustand gemeinsam mit der Identität des Elements.

### Intern

Der Schlüssel wird zur Identität einer Composition-Gruppe. Die Runtime verwendet
ihn, um:

- alte und neue Gruppen einander zuzuordnen;
- eine Gruppe zu verschieben oder wiederzuverwenden;
- `remember`-Zustand zu bewahren;
- `rememberSaveable`-Zustand wiederherzustellen;
- Elementanimationen korrekt auszuführen.

Dies ist kein `DiffUtil`: Der Schlüssel dient dem Composition- und
Positionierungs-Bookkeeping im Lazy-Layout.

### Anforderungen an den Schlüssel

Ein Schlüssel muss:

- unter benachbarten Elementen eindeutig sein;
- zwischen Recompositions stabil bleiben;
- mit der Domänenidentität zusammenhängen;
- speicherbar sein, wenn das Element `rememberSaveable` verwendet.

Gut:

```kotlin
key = { it.id }
```

Ungünstig:

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Zufall und Zeit erzeugen bei jeder Recomposition eine neue Identität. Ein Index
entspricht der standardmäßigen Positionsidentität und eignet sich nur für
tatsächlich statische Listen.

Ein zusammengesetzter Schlüssel ist zulässig, wenn er stabil ist:

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

Für Paging wird die Item-Key-API verwendet:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**Kurz gesagt:** Ein `key` bindet ein Lazy-Element an eine stabile Identität statt
an einen Index. Er wird für dynamische Listen, lokalen Elementzustand,
Umsortierung, Animationen und Paging benötigt. Der beste Schlüssel ist eine
stabile, vom Server, aus der Datenbank oder vom Client erzeugte ID.

</details>

<details>
<summary>123. Welche weiteren Parameter können an LazyColumn items() übergeben werden und wofür dient contentType?</summary>

#### Kotlin

Die wichtigsten Parameter von `LazyColumn.items()` sind `key` und `contentType`.

Grundlegend:

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Für eine dynamische Liste sollte ein `key` ausdrücklich übergeben werden:

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

Der `key` bestimmt die stabile Identität eines Elements. Er verhindert, dass
Compose gespeicherten Zustand nach Einfügen, Löschen oder Umsortieren falschen
Elementen zuordnet.

Ein guter Schlüssel:

- ist eindeutig;
- bleibt stabil;
- hängt nicht von der Position ab;
- basiert weder auf Zufall noch Zeit;
- beruht auf einer Domänen-ID.

`contentType` beschreibt den UI-Typ eines Elements:

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

Beispiele sind `header`, `post`, `ad` oder `loader`. Dadurch kann Compose die
Composition gleichartiger Elemente in einer gemischten Liste effizienter
wiederverwenden.

Ein Beispiel mit Sealed-Elementen:

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

Wird ein Index benötigt, steht `itemsIndexed` zur Verfügung:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

Für veränderliche oder umsortierbare Listen sollte der Index nicht als Schlüssel
verwendet werden.

`contentType` ist weniger wichtig, wenn sämtliche Elemente dieselbe UI-Struktur
besitzen. Den größten Nutzen bringt er bei gemischten Feeds, Chats und Katalogen.

**Kurz gesagt:** `key` ist die stabile Identität eines Elements. `contentType`
beschreibt seinen UI-Typ beziehungsweise seine UI-Struktur und verbessert die
Wiederverwendung der Composition in gemischten Listen. Bei dynamischen
Produktionslisten ist `key` fast immer erforderlich, `contentType` bei
unterschiedlichen Zeilentypen.

</details>

<details>
<summary>124. Was ist SOLID?</summary>

#### Kotlin

`SOLID` bezeichnet fünf Prinzipien des objektorientierten Designs, die Kopplung
verringern, Testbarkeit verbessern und Änderungen stärker lokalisieren.

### S – Single Responsibility Principle

Eine Klasse sollte nur einen Grund für Änderungen besitzen.

Ungünstig ist es, wenn ein `ViewModel` Netzwerk, Datenbank, Mapping und UI-Zustand
bearbeitet. Besser ist eine Trennung:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

Das `ViewModel` verwaltet den Zustand, der Use Case eine Geschäftsoperation und
das Repository die Daten.

### O – Open/Closed Principle

Code ist offen für Erweiterungen, sollte dafür aber nicht ständig geändert werden
müssen.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Eine neue Implementierung kann hinzugefügt werden, ohne den Client-Code neu zu
schreiben.

### L – Liskov Substitution Principle

Ein Untertyp muss den Basistyp korrekt ersetzen können.

Ungünstig:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Erfüllt eine Implementierung den Vertrag nicht, ist dieser zu umfangreich oder
falsch definiert.

### I – Interface Segregation Principle

Ein Client darf nicht von Methoden abhängen, die er nicht benötigt.

Ungünstig:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Besser getrennt:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D – Dependency Inversion Principle

Höherstufiger Code hängt von Abstraktionen und nicht von konkreten
Implementierungen ab.

Ungünstig:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Besser:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Dadurch lässt sich der Use Case mit einem Fake-Repository leicht testen.

Unter Android sieht dies üblicherweise so aus:

```text
UI -> ViewModel -> Use Case -> Repository-Vertrag -> Repository-Implementierung -> API/DB
```

Eine praktische Regel: SOLID bedeutet nicht, für jede Klasse eine Schnittstelle
zu erstellen. Eine Abstraktion ist dort erforderlich, wo eine tatsächliche
Systemgrenze, eine alternative Implementierung oder ein Test-Seam besteht.

**Kurz gesagt:** SOLID hilft, UI, Geschäftslogik und Datenschicht nicht zu
vermischen, God Classes zu vermeiden, Verträge eng zu halten und Code testbar zu
machen. Übermäßige Abstraktionen ohne konkreten Bedarf sind jedoch ebenfalls
schädlich.

</details>

<details>
<summary>125. Erklären Sie jedes SOLID-Prinzip anhand von Android-Beispielen.</summary>

#### Kotlin

SOLID hilft, Verantwortlichkeiten zu trennen, Verträge sauber zu halten und
Geschäftslogik ohne Android-Framework, Retrofit oder Room zu testen.

### S – Single Responsibility

Eine Klasse sollte nur einen Grund für Änderungen besitzen.

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

Das `ViewModel` verwaltet den UI-Zustand, der Use Case die Geschäftsoperation und
das Repository die Daten. SRP bedeutet nicht „eine Methode pro Klasse“, sondern
verhindert die Vermischung unterschiedlicher Änderungsgründe.

### O – Open/Closed

Code wird durch neue Implementierungen erweitert, anstatt vorhandene Logik
ständig zu verändern.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Ein neuer Handler wird als eigene Klasse hinzugefügt. Eine Abstraktion ist jedoch
nur dort sinnvoll, wo tatsächliche Variabilität besteht.

### L – Liskov Substitution

Ein Untertyp muss den Basistyp korrekt ersetzen können.

Ungünstig ist es, wenn `ReadOnlyStorage` eine `save()`-Methode implementiert und
eine `UnsupportedOperationException` auslöst. Der Vertrag sollte besser getrennt
werden:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP betrifft nicht nur die Signatur, sondern auch Garantien, Fehler,
Nebenwirkungen und Invarianten.

### I – Interface Segregation

Ein Aufrufer darf nicht von Methoden abhängen, die er nicht benötigt.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Ein Use Case erhält einen engen Vertrag anstelle einer God Interface. Zu kleine
Schnittstellen ohne eine tatsächliche Rolle für einen Aufrufer erzeugen jedoch
ebenfalls unnötigen Code.

### D – Dependency Inversion

Höherstufige Geschäftslogik hängt von einer Abstraktion ab, die von der
Datenschicht implementiert wird.

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

Der Use Case erzeugt Retrofit oder Room nicht direkt. Dependency Injection stellt
die reale Implementierung oder einen Fake bereit.

```text
UI -> ViewModel -> Use Case -> Repository-Vertrag
                              ^
                              |
                     Datenimplementierung -> API/DB
```

Typische Verstöße unter Android:

- Ein Fragment ruft API oder DAO direkt auf.
- Ein ViewModel enthält Netzwerkzugriff, Mapping, Cache und Navigation.
- Ein einziges Repository ist für die gesamte Anwendung zuständig.
- Für jede Klasse wird ohne Systemgrenze eine Schnittstelle erstellt.
- Die Domäne hängt von Android- oder Infrastrukturtypen ab.
- Ein Untertyp implementiert formal eine Schnittstelle, verletzt aber ihren
  Vertrag.

**Kurz gesagt:** SRP trennt Verantwortlichkeiten, OCP definiert
Erweiterungspunkte, LSP garantiert den korrekten Ersatz durch Untertypen, ISP hält
Verträge eng und DIP lässt Geschäftslogik von Abstraktionen abhängen. Unter
Android trennt dies UI und Domäne von Framework- und Datendetails.

</details>

<details>
<summary>126. Was ist objektorientierte Programmierung?</summary>

#### Kotlin

Objektorientierte Programmierung ist ein Ansatz, bei dem ein System durch Objekte
modelliert wird, die Zustand und Verhalten verbinden. Eine Klasse beschreibt
einen Typ, ein Objekt eine Instanz dieses Typs.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Die wichtigsten Prinzipien sind Kapselung, Abstraktion, Vererbung und
Polymorphie.

### Kapselung

Eine Klasse verbirgt veränderlichen Zustand und ändert ihn über kontrollierte
Methoden:

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

So schützt die Klasse ihre Invarianten.

### Abstraktion

Code hängt von einem Vertrag und nicht von einer Implementierung ab:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

Das `ViewModel` weiß nicht, ob die Daten von Retrofit, Room oder einem Fake
stammen.

### Vererbung

Klassen sind in Kotlin standardmäßig `final`. Vererbung wird mit `open`
zugelassen:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Vererbung modelliert eine Ist-ein-Beziehung, erzeugt aber häufig starke Kopplung.
Komposition ist üblicherweise sicherer.

### Polymorphie

Verschiedene Implementierungen können über einen gemeinsamen Vertrag eingesetzt
werden:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

Der Aufrufer arbeitet mit `ImageLoader`, wodurch Produktions- und
Testimplementierung austauschbar sind.

In Kotlin lässt sich objektorientierte Programmierung gut mit `data class`,
`sealed class` beziehungsweise `sealed interface`, Erweiterungsfunktionen,
Unveränderlichkeit und Konstruktorinjektion verbinden. Entscheidend sind nicht
die Anzahl der Klassen und Schnittstellen, sondern klare Verantwortlichkeiten und
Systemgrenzen.

**Kurz gesagt:** Objektorientierte Programmierung organisiert Zustand und
Verhalten in Objekten. In Kotlin sind Kapselung, Verträge, Polymorphie und ein
vorsichtiger Einsatz von Vererbung entscheidend; unveränderliche Modelle und
Komposition sind einer tiefen Hierarchie häufig vorzuziehen.

</details>

<details>
<summary>127. Was ist Abstraktion?</summary>

#### Kotlin

Abstraktion bedeutet, Implementierungsdetails hinter einem verständlichen Vertrag
zu verbergen. Sie beschreibt, was Code tut, und nicht, wie er es genau tut.

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Code, der `UserRepository` verwendet, weiß nicht, ob die Daten aus einer API,
Room, einem Cache oder einem Fake-Objekt im Test stammen.

Ein Use Case hängt vom Vertrag ab:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Die Implementierung verbirgt die Details:

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

In Tests kann ein Fake eingesetzt werden:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

Ein typisches Schema unter Android:

```text
ViewModel -> Use Case -> Repository-Vertrag -> Repository-Implementierung -> API/DB
```

Die UI darf nichts über Retrofit, Room oder die Cache-Strategie wissen.

Abstraktion bedeutet nicht zwangsläufig eine Schnittstelle. Sie kann auch durch
eine abstrakte Klasse, eine Sealed-Hierarchie, einen Funktionstyp oder einfach die
öffentliche API einer Klasse entstehen. Abstrakte Basisklassen erzeugen unter
Android jedoch häufig starre Hierarchien, weshalb Komposition meist sicherer ist.

Abstraktion im Vergleich zu Kapselung:

```text
Abstraktion -> zeigt den wesentlichen Vertrag
Kapselung   -> verbirgt internen Zustand und Details
```

Eine schlechte Abstraktion ist eine vorsorgliche Schnittstelle ohne tatsächliche
alternative Implementierung oder Systemgrenze:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

Gibt es nur eine einfache Implementierung und weder Test-Seam noch Systemgrenze,
kann die Schnittstelle überflüssig sein.

Eine gute Abstraktion:

- besitzt einen klaren Vertrag;
- verbirgt instabile Details;
- lässt keine Implementierungsdetails nach außen dringen;
- vereinfacht Tests;
- entspricht einer tatsächlichen Änderungsstelle;
- fügt keinen nutzlosen wiederkehrenden Code hinzu.

**Kurz gesagt:** Abstraktion trennt Vertrag und Implementierung. Unter Android
ist sie an Systemgrenzen wie Netzwerk, Datenbank, Analyse, Zeit, Dispatcher und
Repositories nützlich. Nicht jede Klasse sollte ohne konkreten Grund abstrahiert
werden.

</details>

<details>
<summary>128. Worin besteht der Unterschied zwischen Konstruktoren und Methoden?</summary>

#### Kotlin

Ein Konstruktor erzeugt eine Instanz und versetzt das Objekt in einen anfänglich
gültigen Zustand. Eine Methode beschreibt das Verhalten eines bereits erzeugten
Objekts und kann mehrfach aufgerufen werden.

Ein Primärkonstruktor:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Parameter mit `val` oder `var` werden zu Eigenschaften. Ein Konstruktor besitzt
keinen Rückgabetyp und wird mit `User(...)` aufgerufen.

Eine Methode:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

Die Methode wird an einer Instanz aufgerufen:

```kotlin
user.displayName()
```

Für Validierung oder Initialisierungslogik wird `init` verwendet:

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

Ein Sekundärkonstruktor delegiert mit `this(...)` an den Primärkonstruktor:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

In Kotlin ist ein Standardparameter häufig vorzuziehen:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Ein Konstruktor wird oft für Dependency Injection eingesetzt:

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

Der Konstruktor legt erforderliche Abhängigkeiten fest, die Methode führt eine
Operation aus.

Folgendes gehört nicht in Konstruktor oder `init`:

- Netzwerkanfrage;
- Datenbankabfrage;
- Start einer Koroutine;
- `runBlocking`;
- aufwendige Ein- und Ausgabe;
- vom Android-Lebenszyklus abhängige Logik.

Ungünstig:

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Besser:

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**Kurz gesagt:** Ein Konstruktor erzeugt ein gültiges Objekt und nimmt den
erforderlichen Zustand beziehungsweise erforderliche Abhängigkeiten entgegen.
Eine Methode führt nach der Konstruktion Verhalten aus. Aufwendige, aussetzbare
und lebenszyklusgebundene Arbeit darf nicht im Konstruktor gestartet werden.

</details>

<details>
<summary>129. Was ist eine Methodensignatur?</summary>

#### Kotlin

Eine Methodensignatur umfasst die Merkmale, anhand derer der Compiler Funktionen
unterscheidet. In Kotlin sind dies vor allem Name, Empfänger sowie Typen und
Reihenfolge der Parameter.

```kotlin
fun loadUser(id: String): User
```

Vereinfacht lautet die Signatur `loadUser(String)`.

### Überladung

Überladungen müssen sich in der Parameterliste unterscheiden:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Ein unterschiedlicher Rückgabetyp genügt nicht:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // Konflikt
```

Auch ein anderer Parametername genügt nicht:

```kotlin
fun load(id: String)
fun load(userId: String) // Konflikt
```

Benannte Argumente verbessern die Lesbarkeit an der Aufrufstelle, erzeugen aber
keine neue Signatur.

Die Reihenfolge der Typen ist relevant:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

Dies ist zulässig, führt aufgrund geringer Lesbarkeit jedoch häufig zu einer
schlechten API.

### Standardparameter

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

Dies ist eine einzelne Kotlin-Funktion. Für Java-Überladungen kann
`@JvmOverloads` verwendet werden; zusätzliche manuelle Überladungen sollten
jedoch mit Bedacht erstellt werden.

### Überschreiben

Eine überschreibende Methode muss dem Vertrag des Basistyps entsprechen:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Ein geänderter Parametertyp erzeugt eine andere Funktion und keine Überschreibung.
Der Rückgabetyp darf ein kovarianter Untertyp sein.

### Empfänger und Erweiterungen

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

Der Empfänger einer Erweiterung nimmt an der Kotlin-Auflösung teil. Eine
Erweiterungsfunktion ist jedoch keine virtuelle Überschreibung.

### Besonderheiten der JVM

Aufgrund der Typlöschung können Konflikte entstehen:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // JVM-Konflikt: process(List)
```

Auch Nullability erzeugt keine eigene JVM-Signatur:

```kotlin
fun load(id: String)
fun load(id: String?) // JVM-Konflikt
```

Die Lösung ist ein anderer Name oder bei tatsächlichem Bedarf `@JvmName`.

**Kurz gesagt:** Kotlin unterscheidet Funktionen anhand von Name, Empfänger sowie
Typen und Reihenfolge der Parameter. Rückgabetyp und Parameternamen genügen nicht.
Auf der JVM sind zusätzlich Typlöschung, Nullability und
Interoperabilitätsbesonderheiten zu beachten.

</details>

<details>
<summary>130. Worin besteht der Unterschied zwischen Überladung und Überschreibung?</summary>

#### Kotlin

```text
Überladung       -> gleicher Name, unterschiedliche Parameterlisten
Überschreibung   -> gleiche Signatur, neue Implementierung in einem Untertyp
```

### Überladung

Mehrere Funktionen im selben Gültigkeitsbereich besitzen denselben Namen, aber
unterschiedliche Parameter:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

Der Compiler wählt die Überladung anhand der statischen Typen der Argumente. Der
Rückgabetyp allein kann Überladungen nicht unterscheiden:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // Kompilierungsfehler
```

In Kotlin sollten manche Überladungen besser durch Standardparameter und benannte
Argumente ersetzt werden:

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
)

loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

### Überschreibung

Ein Untertyp implementiert oder verändert eine geerbte Methode mit derselben
Signatur:

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

Ein geänderter Parametertyp erzeugt eine andere Funktion und keine Überschreibung.

Klassen und Methoden sind in Kotlin standardmäßig `final`. Für eine
Überschreibung muss ein Member `open`, `abstract` oder Bestandteil einer
Schnittstelle sein:

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

Überladung verwendet Dispatch zur Kompilierzeit beziehungsweise statischen
Dispatch:

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

Überschreibung verwendet Dispatch zur Laufzeit beziehungsweise virtuellen
Dispatch:

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // Implementierung von CrashlyticsLogger
```

Wichtige Besonderheiten:

- Überladungen mit Standardargumenten können mehrdeutig sein.
- Eine Überschreibung muss den Vertrag des Basistyps bewahren.
- Der Rückgabetyp einer Überschreibung darf kovariant sein.
- Erweiterungsfunktionen werden nicht überschrieben, sondern statisch aufgelöst.
- `final override` verhindert eine weitere Überschreibung.

**Kurz gesagt:** Überladung bedeutet mehrere Funktionen mit demselben Namen und
unterschiedlichen Parametern; die Auswahl erfolgt zur Kompilierzeit.
Überschreibung ersetzt eine geerbte Implementierung mit derselben Signatur; die
Auswahl erfolgt zur Laufzeit durch Polymorphie.

</details>

<details>
<summary>131. Worin besteht der Unterschied zwischen einer Schnittstelle und einer abstrakten Klasse?</summary>

#### Kotlin

Eine Schnittstelle beschreibt einen Vertrag oder eine Rolle. Eine abstrakte
Klasse beschreibt eine gemeinsame Basis mit teilweiser Implementierung,
Konstruktor und Zustand.

```text
Schnittstelle    -> Vertrag, mehrere Rollen
abstrakte Klasse -> Basisimplementierung, eine Elternklasse
```

### Schnittstelle

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Eine Klasse kann mehrere Schnittstellen implementieren:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Eine Schnittstelle eignet sich gut für Dependency Injection, Tests und Dependency
Inversion:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

In Kotlin kann eine Schnittstelle eine Standardmethode besitzen:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Eine Schnittstelle besitzt jedoch keinen Konstruktor und eignet sich nicht für
gemeinsamen veränderlichen Zustand.

### Abstrakte Klasse

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // shared logic
    }

    abstract suspend fun refresh()
}
```

Eine abstrakte Klasse kann besitzen:

- einen Konstruktor;
- Zustand;
- geschützte Methoden;
- eine teilweise Implementierung;
- ein Template-Method-Pattern.

Eine Klasse kann jedoch nur von einer Klasse erben:

```kotlin
class MyRepository : BaseRepository()
```

### Wann welche Variante verwendet wird

Eine Schnittstelle eignet sich, wenn:

- ein Vertrag benötigt wird;
- mehrere Implementierungen existieren;
- ein Fake oder Mock in Tests erforderlich ist;
- eine Klasse mehrere Rollen besitzt;
- eine Abhängigkeit abstrahiert werden muss.

Eine abstrakte Klasse eignet sich, wenn:

- eine gemeinsame Implementierung benötigt wird;
- geschützter Zustand erforderlich ist;
- ein gemeinsamer Lebenszyklus oder ein Template besteht;
- der Konstruktor Bestandteil des Basisvertrags ist.

Unter Android ist bei `BaseActivity`, `BaseFragment` und `BaseViewModel` Vorsicht
geboten: Sie entwickeln sich schnell zu God Base Classes und erzeugen starre
Hierarchien.

Komposition ist häufig besser:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**Kurz gesagt:** Eine Schnittstelle dient Verträgen, Dependency Injection, Tests
und mehreren Rollen. Eine abstrakte Klasse bietet gemeinsame Implementierung und
Zustand, birgt aber das Risiko starrer Vererbung. Im modernen Android ist eine
Schnittstelle mit Komposition häufig besser als tiefe Basisklassenhierarchien.

</details>

<details>
<summary>132. Kann eine Schnittstelle keine Methoden enthalten?</summary>

#### Kotlin

Ja. Eine leere Schnittstelle wird Marker-Schnittstelle genannt. Sie definiert
kein Verhalten, sondern kennzeichnet einen Typ im Typsystem mit einer bestimmten
Rolle.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

`UserProfile` kann nun als `Cacheable` entgegengenommen, mit `is Cacheable`
geprüft oder als generische Obergrenze verwendet werden.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // nur ausdrücklich markierte Typen
}
```

Benötigt `sync` eine ID oder ein bestimmtes Verhalten, genügt der Marker nicht
mehr; dem Vertrag muss eine Eigenschaft oder Methode hinzugefügt werden.

### Sealed Marker

Eine leere `sealed interface` wird häufig als Wurzel einer geschlossenen Menge
von Zuständen, Ereignissen oder Effekten verwendet:

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

Hier handelt es sich nicht nur um einen Marker, sondern um einen Summentyp: Ein
Wert ist eine der definierten Varianten, und der Compiler prüft ein vollständiges
`when`.

Ein klassisches Beispiel einer Marker-Schnittstelle ist `java.io.Serializable`:
Sie enthält keine Methoden, die Runtime prüft jedoch, ob ein Objekt diesen Typ
implementiert.

### Schnittstelle oder Annotation

Eine Marker-Schnittstelle eignet sich, wenn Folgendes benötigt wird:

- eine Untertypbeziehung;
- generische Obergrenzen;
- eine polymorphe API;
- eine Sealed-Hierarchie;
- eine `is`-Prüfung ohne Reflection.

Eine Annotation eignet sich besser für Metadaten:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Sie wird von einem Compiler-Plugin, einem Codegenerator oder durch
Runtime-Reflection verarbeitet und erzeugt keine Untertypbeziehung.

### Wann ein Marker ungeeignet ist

Ein Marker sollte nicht vorsorglich erstellt werden:

```kotlin
interface Important
interface Special
```

Anzeichen für eine schlechte Verwendung:

- Der Marker wird nirgends als Typ, Obergrenze oder Wurzel verwendet.
- Die Logik enthält zahlreiche `is Marker`-Prüfungen.
- Daten oder Methoden werden benötigt, der Code verwendet stattdessen Casts.
- Die Rolle lässt sich besser mit einem Enum oder einer Eigenschaft ausdrücken.
- Metadaten lassen sich besser mit einer Annotation beschreiben.

**Kurz gesagt:** Eine Schnittstelle darf leer sein. Eine Marker-Schnittstelle ist
für Untertypbeziehungen, generische Obergrenzen und Sealed-Hierarchien nützlich.
Für Metadaten wird eine Annotation verwendet; wird Verhalten benötigt, muss die
Schnittstelle Member enthalten.

</details>

<details>
<summary>133. Kann eine Schnittstelle eine weitere Schnittstelle enthalten?</summary>

#### Kotlin

Ja. Eine Schnittstelle kann in Kotlin eine weitere Schnittstelle enthalten. Dies
wird als verschachtelte Schnittstelle bezeichnet.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Verwendung:

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // handle change
    }
}
```

Auf eine verschachtelte Schnittstelle wird mit `Outer.Inner` zugegriffen,
beispielsweise `UserRepository.Listener`.

Dies ist sinnvoll, wenn:

- der Vertrag eng mit der äußeren API verbunden ist;
- der verschachtelte Typ ausschließlich in diesem Kontext verwendet wird;
- es sich um einen Callback oder kleinen verschachtelten Vertrag handelt;
- ein Top-Level-Typ das Paket lediglich unübersichtlicher machen würde.

Ein Beispiel für eine Callback-API:

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

Eine verschachtelte Schnittstelle ist nicht `inner` und besitzt keinen Zugriff
auf den Zustand einer Instanz des äußeren Typs:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` kann `id` nicht direkt lesen.

Dies darf nicht mit Schnittstellenvererbung verwechselt werden:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

Hier handelt es sich nicht um Verschachtelung, sondern um Vererbung von Verträgen.

Eine Schnittstelle sollte nicht verschachtelt werden, wenn sie eigenständig ist
und im gesamten Projekt verwendet wird:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

Dadurch wird die äußere Schnittstelle zu einem künstlichen Namespace. Top-Level-
Verträge sind in diesem Fall vorzuziehen.

**Kurz gesagt:** Eine Schnittstelle darf eine verschachtelte Schnittstelle
enthalten. Dies ist für Callbacks oder Verträge nützlich, die ausschließlich im
Kontext einer äußeren API existieren. Eigenständige Abstraktionen sollten als
Top-Level-Typen definiert werden.

</details>

<details>
<summary>134. Warum sollte eine Schnittstelle nicht Dutzende Methoden enthalten?</summary>

#### Kotlin

Eine große Schnittstelle verletzt üblicherweise das Interface Segregation
Principle: Clients hängen von Methoden ab, die sie nicht benötigen. Dies erhöht
die Kopplung und erschwert Implementierungen sowie Tests.

Ungünstig:

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

Benötigt ein Use Case ausschließlich `getUser`, hängt er dennoch vom gesamten
Vertrag ab.

Ein Problem bei der Implementierung:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Kann eine Implementierung eine Methode nicht ehrlich erfüllen, ist der Vertrag
falsch oder zu umfangreich.

Ein Problem bei Tests:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // unnötiger Ballast
}
```

Fakes und Mocks müssen dadurch nicht benötigte Methoden implementieren.

Die Rollen sollten besser getrennt werden:

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

Ein Use Case hängt nur vom erforderlichen Vertrag ab:

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

Eine große Schnittstelle ist nur zulässig, wenn sie tatsächlich eine einzige
kohärente Abstraktion darstellt. Ein DAO für eine einzelne Tabelle kann
beispielsweise mehrere zusammengehörige Abfragen enthalten. Haben die Methoden
jedoch unterschiedliche Clients und unterschiedliche Änderungsgründe, sollten
sie getrennt werden.

**Kurz gesagt:** Eine Schnittstelle sollte eine Rolle und nicht sämtliche
Fähigkeiten des Systems beschreiben. Verwendet ein Client nur eine oder zwei von
zwanzig Methoden, ist der Vertrag zu umfangreich. Reader, Writer, Uploader und
ähnliche Rollen sollten entsprechend den tatsächlichen Aufgaben getrennt werden.

</details>

<details>
<summary>135. Was ist Komposition und wie unterscheidet sie sich von Vererbung?</summary>

#### Kotlin

Bei Komposition erhält eine Klasse Verhalten über Abhängigkeiten. Bei Vererbung
übernimmt eine Klasse Verhalten von einer Basisklasse.

```text
Vererbung  -> ist ein
Komposition -> hat ein / verwendet ein
```

### Vererbung

Sie ist sinnvoll, wenn ein Untertyp tatsächlich eine Spezialisierung des
Basistyps darstellt:

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

Ein `Dog` ist ein `Animal`, daher ist die Ist-ein-Beziehung logisch.

### Komposition

Eine Klasse verwendet andere Objekte anstelle von Vererbung:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

Ein `Dog` besitzt einen `SoundPlayer`, ist aber kein `SoundPlayer`.

Unter Android und Kotlin sieht Komposition häufig so aus:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

Die Abhängigkeiten sind explizit und können in Tests leicht durch Fakes oder
Mocks ersetzt werden.

### Das Problem von Basisklassen

Große Klassen wie `BaseViewModel`, `BaseRepository` oder `BaseFragment` entwickeln
sich häufig zu God Base Classes:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Unterklassen erhalten Methoden und Zustand, die sie nicht benötigen. Änderungen
an der Basisklasse können zahlreiche voneinander unabhängige Bildschirme
beeinträchtigen.

### Kotlin-Delegation

Kotlin bietet Delegation als komfortable Form der Komposition:

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

`UserService` delegiert `Logger` an ein anderes Objekt, ohne davon zu erben.

Wann welche Variante verwendet wird:

- Vererbung für eine echte Ist-ein-Beziehung und einen stabilen Basisvertrag;
- Komposition, wenn Verhalten eingesetzt, getestet, kombiniert oder ausgetauscht
  werden muss;
- im Android-Anwendungscode ist Komposition üblicherweise tiefer Vererbung
  vorzuziehen.

**Kurz gesagt:** Vererbung modelliert eine Ist-ein-Beziehung, Komposition eine
Hat-ein- beziehungsweise Verwendet-ein-Beziehung. Unter Kotlin und Android ist
Komposition meist besser: explizite Abhängigkeiten, einfachere Tests, geringere
Kopplung und keine fragilen Basisklassenhierarchien.

</details>

<details>
<summary>136. Welche Sichtbarkeitsmodifikatoren gibt es in Kotlin?</summary>

#### Kotlin

Kotlin besitzt vier Sichtbarkeitsmodifikatoren:

```text
public    -> überall zugänglich
internal  -> innerhalb des Moduls zugänglich
protected -> in der Klasse und ihren Unterklassen zugänglich
private   -> im aktuellen Gültigkeitsbereich oder in der Datei zugänglich
```

### `public`

`public` ist die Standardsichtbarkeit:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Alles Öffentliche wird Bestandteil der API. Implementierungsdetails sollten
daher nicht unnötig offengelegt werden.

### `private`

Ein Member ist ausschließlich innerhalb seiner Klasse oder seines Objekts
zugänglich:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Ein Top-Level-`private` ist innerhalb derselben Kotlin-Datei zugänglich:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### `protected`

Ist in der Klasse und ihren Unterklassen zugänglich:

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}
```

Anders als in Java gewährt `protected` in Kotlin keinen Paketzugriff und ist auf
Top-Level-Ebene nicht zulässig. Es ist Bestandteil der Vererbungs-API.

### `internal`

Ist innerhalb eines Kotlin-Moduls zugänglich:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Ein Modul ist eine Kompilierungsgrenze, beispielsweise ein Gradle-Modul, und kein
Paket.

`internal` ist in Android-Projekten mit mehreren Modulen nützlich: Nur die API
des Moduls bleibt öffentlich, die Implementierung wird `internal`.

Wichtig: `internal` ist eine Grenze zur Kompilierzeit und keine
Sicherheitsgrenze. Im Bytecode können Java oder Reflection mehr sehen.

### Sichtbarkeit von Setter und Konstruktor

Eine Eigenschaft kann öffentlich lesbar, aber nur privat schreibbar sein:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set
}
```

Auch ein Konstruktor kann eine Sichtbarkeit besitzen:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Praktische Regeln:

- mit der engsten ausreichenden Sichtbarkeit beginnen;
- Implementierungen `private` oder `internal` halten;
- die öffentliche API minimieren;
- `protected` nur für einen tatsächlichen Vererbungsvertrag verwenden;
- Sichtbarkeit nicht als Sicherheitsgrenze betrachten.

**Kurz gesagt:** `public` gilt überall, `internal` im Modul, `protected` in der
Vererbungshierarchie und `private` in Klasse oder Datei. Eine gute Kotlin-API
legt nur das erforderliche Minimum offen.

</details>

<details>
<summary>137. Was ist ein Singleton?</summary>

#### Kotlin

Ein Singleton ist eine einzelne Instanz innerhalb eines bestimmten Eigentümers,
etwa eines Classloaders, Prozesses oder einer DI-Komponente. Unter Android ist zu
beachten, dass ein Prozess beendet werden kann und eine Multiprozessanwendung in
jedem Prozess ein eigenes Singleton besitzt.

### Kotlin-`object`

```kotlin
object AppLogger {
    fun log(message: String) = println(message)
}
```

Ein `object` wird beim ersten Zugriff lazy und threadsicher initialisiert.
Veränderlicher Zustand darin wird jedoch nicht automatisch threadsicher:

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

Ein Companion ist ein an eine Klasse gebundenes Singleton. Die Klasse selbst kann
viele Instanzen besitzen. Ein Companion Object wird üblicherweise für
Factory-Methoden und Konstanten verwendet.

### Singleton über Dependency Injection

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
)
```

Bei Hilt bedeutet `@Singleton` eine Instanz im `SingletonComponent` und kein
ewiges Objekt. Dependency Injection ist bei vorhandenen Abhängigkeiten einem
globalen Singleton vorzuziehen:

- Konstruktorabhängigkeiten sind explizit;
- die Implementierung kann durch einen Fake ersetzt werden;
- die Komponente steuert die Lebensdauer;
- der Aufrufer hängt von einem Vertrag ab.

Für kürzere Lebensdauern stehen `@ViewModelScoped`, `@ActivityScoped` und weitere
Scopes zur Verfügung.

### Risiken

Ungünstig:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Mögliche Probleme:

- Speicherleck einer Activity;
- globaler veränderlicher Zustand;
- Race Conditions;
- schwierige Tests aufgrund gemeinsamen Zustands;
- unklarer Logout oder Reset;
- Zustandsverlust nach dem Beenden des Prozesses.

Ein Singleton auf Anwendungsebene darf weder `Activity`, `Fragment`, `View` noch
einen Activity-Kontext festhalten. Wird ein Kontext benötigt, darf es nur der
Anwendungskontext sein.

Sitzung und Token sollten nicht ausschließlich in einem In-Memory-Singleton
gespeichert werden. Erforderlich sind sichere Persistenz, Wiederherstellung und
explizites Löschen.

Ein Singleton eignet sich für:

- Room-Datenbank;
- OkHttp-Client;
- Logger und Analyse;
- zustandslose anwendungsweite Dienste;
- unveränderliche Konfiguration.

Es eignet sich nicht für Bildschirmzustand, ViewModel, eine Benutzersitzung ohne
Reset oder eine Abhängigkeit mit kurzem Lebenszyklus.

**Kurz gesagt:** In Kotlin wird ein Singleton häufig mit `object` erstellt;
Android-Abhängigkeiten sollten besser über DI mit einem Scope versehen werden.
Ein Singleton eignet sich für anwendungsweite Dienste, globaler veränderlicher
Zustand und UI-Referenzen sind jedoch fast immer problematisch.

</details>

<details>
<summary>138. Was sind Generics?</summary>

#### Kotlin

Generics ermöglichen es, eine Klasse, ein Interface oder eine Funktion mit einem Typ zu parametrisieren und dabei die Typsicherheit zur Kompilierzeit zu bewahren.

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` ist ein Typparameter, `String` und `Int` sind Typargumente. Der Compiler kennt den Typ von `value`, sodass kein unsicherer Cast erforderlich ist.

Generische Funktion:

```kotlin
fun <T> singleItemList(item: T): List<T> = listOf(item)
```

### Typbeschränkungen

Ein Typparameter kann eingeschränkt werden:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Mehrere Beschränkungen:

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable = Unit
```

### Generisches Ergebnis

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val cause: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

`Nothing` ist ein Untertyp aller Kotlin-Typen. Daher können `Error` und `Loading` als `Result<User>`, `Result<List<Post>>` usw. verwendet werden.

### Varianz

Standardmäßig ist ein generischer Typ invariant. Selbst wenn `Cat : Animal` gilt, ist `MutableList<Cat>` keine `MutableList<Animal>`.

`out T` bezeichnet einen Produzenten beziehungsweise Kovarianz:

```kotlin
interface Producer<out T> {
    fun produce(): T
}

val cats: Producer<Cat> = TODO()
val animals: Producer<Animal> = cats
```

`in T` bezeichnet einen Konsumenten beziehungsweise Kontravarianz:

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}

val animalConsumer: Consumer<Animal> = TODO()
val catConsumer: Consumer<Cat> = animalConsumer
```

Das Modell lautet: Produzent — `out`, Konsument — `in`. Wenn ein Typ `T` sowohl entgegennimmt als auch zurückgibt, ist er in der Regel invariant.

### Typlöschung und `reified`

Auf der JVM werden generische Typargumente zur Laufzeit größtenteils gelöscht:

```kotlin
value is List<*> // möglich
// value is List<String> // nicht direkt möglich
```

Mit `inline reified` kann zur Laufzeit auf den Typparameter zugegriffen werden:

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` ist nur bei Inline-Funktionen möglich.

Praktische Regeln:

- Typbeschränkungen definieren den minimalen Vertrag;
- `out` und `in` beschreiben sichere Untertypbeziehungen;
- veränderliche Container sind häufiger invariant;
- für einen generischen Typ zur Laufzeit wird `reified` oder ein explizites Typ-Token benötigt;
- eine API sollte nicht ohne echten Nutzen durch Generics verkompliziert werden.

**Kurz gesagt:** Generics ermöglichen typsichere, wiederverwendbare APIs. Typbeschränkungen begrenzen die zulässigen Typen, Varianz (`out`/`in`) steuert die Untertypkompatibilität und `reified` umgeht bei Inline-Funktionen teilweise die Typlöschung der JVM.

</details>

<details>
<summary>139. Was ist Type Erasure und warum tritt sie auf?</summary>

#### Kotlin

Type Erasure bedeutet, dass konkrete generische Typargumente auf der JVM in der Regel nicht im Laufzeitobjekt gespeichert werden. `List<String>` und `List<Int>` sind zur Laufzeit lediglich `List`.

Der Grund dafür ist die Abwärtskompatibilität der Java-Generics mit älterem Bytecode. Ein Typparameter wird bis zu seiner oberen Typgrenze oder zu `Object` gelöscht. Kotlin/JVM arbeitet mit demselben JVM-Modell.

Folgende Prüfung ist nicht direkt möglich:

```kotlin
if (value is List<String>) { } // gelöschter Typ
```

Der Container kann hingegen geprüft werden:

```kotlin
if (value is List<*>) {
    // Elementtyp unbekannt
}
```

Mit `List<*>` können Elemente sicher als `Any?` gelesen werden; ihr konkreter Typ wird jedoch nicht garantiert.

Ein ungeprüfter Cast validiert nicht alle Elemente:

```kotlin
val strings = value as List<String> // ungeprüft
```

Ist eine tatsächliche Validierung erforderlich, müssen die Elemente geprüft werden:

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String ?: error("Expected String")
    }
```

### `reified`

Ein gewöhnliches `T` steht für `is T` nicht zur Verfügung. Eine Inline-Funktion kann jedoch einen mit `reified` versehenen Typparameter besitzen:

```kotlin
inline fun <reified T> isType(value: Any): Boolean =
    value is T
```

Dadurch stehen `T::class`, `is T` und APIs mit Klassen-Token zur Verfügung. `reified` stellt verschachtelte Generics jedoch nicht vollständig wieder her: `List<String>` erfordert weiterhin eine Prüfung der Elemente oder einen Serializer beziehungsweise ein Typ-Token.

Ohne `reified` wird der Typ explizit übergeben:

```kotlin
fun <T> decode(json: String, clazz: Class<T>): T = TODO()

val user = decode(json, User::class.java)
```

Für `List<User>` reicht ein einzelnes `Class<List>` nicht aus. Deshalb verwenden Bibliotheken `Type`, `KType`, generierte Adapter oder `KSerializer<T>`.

Type Erasure verursacht außerdem Konflikte bei JVM-Signaturen:

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {} // Konflikt: handle(List)
```

Erforderlich sind unterschiedliche Namen oder ein umsichtiger Einsatz von `@JvmName`.

**Kurz gesagt:** Type Erasure löscht generische Typargumente zur Laufzeit, um die Kompatibilität von JVM und Java zu gewährleisten. Für Laufzeitprüfungen werden `List<*>`, `inline reified`, ein explizites Typ-Token oder ein Serializer verwendet; ein ungeprüfter Cast stellt keine vollständige Typprüfung dar.

</details>

<details>
<summary>140. Welche Arten von Collections gibt es: List, Set, Map, Queue und Stack?</summary>

#### Kotlin

Eine Collection wird anhand ihrer Semantik ausgewählt: Reihenfolge, Eindeutigkeit, Schlüsselsuche, FIFO oder LIFO.

### `List`

Eine geordnete Collection mit Indexzugriff, in der Duplikate zulässig sind:

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

Die typische veränderliche Implementierung auf der JVM ist `ArrayList`: Indexzugriff `O(1)`, Suche `O(n)` sowie Einfügen oder Entfernen in der Mitte `O(n)`.

### `Set`

Eine Collection eindeutiger Elemente:

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* vorhanden */ }
```

`HashSet` bietet für `contains`, `add` und `remove` üblicherweise eine durchschnittliche Laufzeit von `O(1)`, setzt jedoch korrekte Implementierungen von `equals()` und `hashCode()` voraus.

### `Map`

Eine Schlüssel-Wert-Struktur mit eindeutigen Schlüsseln:

```kotlin
val usersById: Map<String, User> = users.associateBy(User::id)
val user = usersById["42"]
```

`HashMap.get()` hat im Durchschnitt eine Laufzeit von `O(1)`. Eine Map eignet sich für Caches, Indizes und Nachschlagetabellen. Ein veränderlicher Schlüssel ist gefährlich, wenn sich Felder ändern, die `hashCode()` beeinflussen.

### `Queue` und `Stack`

Eine Queue arbeitet nach dem FIFO-Prinzip:

```text
first in -> first out
```

Ein Stack arbeitet nach dem LIFO-Prinzip:

```text
last in -> first out
```

In Kotlin lässt sich dafür bequem `ArrayDeque` verwenden:

```kotlin
val deque = ArrayDeque<String>()

// Queue
deque.addLast("A")
deque.addLast("B")
val first = deque.removeFirst() // A

// Stack
deque.addLast("C")
val last = deque.removeLast() // C
```

Operationen an den Enden haben eine amortisierte Laufzeit von `O(1)`. `ArrayDeque` ist dem veralteten Java-`Stack` vorzuziehen.

### Schreibgeschützt und veränderlich

Kotlin trennt die APIs:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` stellt weder `add` noch `remove` bereit. Dies bedeutet jedoch keine tiefe Unveränderlichkeit: Das zugrunde liegende Objekt oder seine Elemente können veränderlich sein. Gemeinsam genutzter veränderlicher Zustand sollte gekapselt werden.

Praktische Auswahl:

- Reihenfolge, Index und Duplikate — `List`;
- Eindeutigkeit und Mitgliedschaftsprüfung — `Set`;
- Suche nach einem Schlüssel — `Map`;
- FIFO — `ArrayDeque.removeFirst()`;
- LIFO — `ArrayDeque.removeLast()`;
- Prioritätsreihenfolge — `PriorityQueue`.

**Kurz gesagt:** `List` steht für Reihenfolge und Duplikate, `Set` für Eindeutigkeit, `Map` für die Suche nach Schlüsseln, Queue für FIFO und Stack für LIFO. In Kotlin genügt für Queue und Stack häufig `ArrayDeque`.

</details>

<details>
<summary>141. Was bedeutet Threadsicherheit bei Collections?</summary>

#### Kotlin

Eine threadsichere Collection behält bei gleichzeitigem Zugriff einen korrekten Zustand bei. Gewöhnliche `MutableList`, `MutableMap` und `MutableSet` sind unter Kotlin/JVM nicht threadsicher.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Gleichzeitige Schreibzugriffe können zu verlorenen Aktualisierungen, einem fehlerhaften internen Zustand oder einer Exception führen.

### Schreibgeschützt ≠ unveränderlich

```kotlin
val users: List<User> = mutableListOf()
```

`List` verhindert Veränderungen lediglich über diese Referenz. Die zugrunde liegende Collection oder ihre Elemente können weiterhin veränderlich sein.

### Grundlegende Strategien

1. **Ein einziger Besitzer beziehungsweise Thread**

Alle Lese- und Schreibzugriffe erfolgen über einen einzigen Besitzer: den Main Thread, einen einzelnen Dispatcher oder eine Actor-ähnliche Komponente. Dies ist das einfachste Ownership-Modell.

2. **`Mutex` für Coroutines**

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

Alle Zugriffe müssen über denselben `Mutex` erfolgen. Innerhalb der Sperre sollten keine lang dauernden Netzwerk- oder Suspend-Operationen ausgeführt werden.

3. **Nebenläufige Collections**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` eignet sich für den gleichzeitigen Zugriff über Schlüssel. Eine zusammengesetzte Operation wird dadurch jedoch nicht automatisch atomar. Dafür werden atomare Methoden wie `computeIfAbsent` oder eine separate Synchronisierung benötigt.

`CopyOnWriteArrayList` eignet sich für Listener: viele Lesezugriffe und seltene Schreibzugriffe.

4. **Unveränderliche Snapshots**

Für den UI-Zustand sollten besser neue Collections erstellt werden:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Die Atomarität wird hier durch `_state.update` gewährleistet, nicht durch `List` selbst.

### Iteration

Eine Veränderung während der Iteration kann selbst in einem einzigen Thread eine `ConcurrentModificationException` auslösen. Muss eine Collection während des Durchlaufens verändert werden, sollte ein Iterator, eine Collection-Operation oder ein Snapshot verwendet werden.

Praktische Auswahl:

- Coroutine-Zustand — `Mutex` oder ein serialisierter Besitzer;
- gleichzeitiger schlüsselbasierter Zugriff — `ConcurrentHashMap`;
- Listener mit überwiegenden Lesezugriffen — `CopyOnWriteArrayList`;
- UI-Zustand — unveränderliche Snapshots und atomares `update`;
- zusammengesetzte Invarianten — ein gemeinsamer kritischer Abschnitt.

**Kurz gesagt:** Veränderliche Collections sind nicht threadsicher. Eine schreibgeschützte API bedeutet keine Unveränderlichkeit. Sicherheit erfordert Ownership, `Mutex`, nebenläufige Collections oder unveränderliche Snapshots; synchronisiert werden muss die gesamte Invariante und nicht nur ein einzelner Aufruf.

</details>

<details>
<summary>142. Worin unterscheiden sich final, finally und finalize?</summary>

#### Kotlin

Es handelt sich um drei unterschiedliche Konzepte:

```text
final    -> verhindert Vererbung beziehungsweise Überschreiben
finally  -> Bereinigungsblock nach try/catch
finalize -> veralteter GC-Callback
```

### `final`

In Java verhindert `final` die Vererbung einer Klasse, das Überschreiben einer Methode oder die Neuzuweisung einer Variablen.

In Kotlin sind Klassen und Methoden standardmäßig `final`. Für die Vererbung wird `open` benötigt:

```kotlin
open class BaseViewModel : ViewModel() {
    open fun load() = Unit
}

class ProfileViewModel : BaseViewModel() {
    final override fun load() {
        // Unterklassen können diese Methode nicht überschreiben
    }
}
```

Mit `final override` kann eine Methode einmal überschrieben werden; weiteres Überschreiben wird verhindert.

Für Variablen und Properties verwendet Kotlin `val`:

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex")
```

`val` verhindert die Neuzuweisung der Referenz, macht das Objekt jedoch nicht unveränderlich.

### `finally`

`finally` wird nach `try` und `catch` zur Bereinigung ausgeführt:

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Typische Anwendungen sind das Schließen einer Ressource, das Aufheben einer Sperre, das Abmelden eines Listeners oder das Zurücksetzen eines temporären Zustands.

Bei Coroutines wird `finally` auch im Fall einer Abbruchanforderung ausgeführt:

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

Wenn die Bereinigung in einer abgebrochenen Coroutine eine Suspend-Funktion benötigt, kann gelegentlich ein kurzer `withContext(NonCancellable)`-Block erforderlich sein. Dies sollte jedoch eine Ausnahme und nicht der Standard sein.

Die Ausführung von `finally` ist beim Beenden des Prozesses oder bei einem JVM-Absturz nicht garantiert.

### `finalize`

`finalize()` ist ein alter Callback von Java-`Object`, den der Garbage Collector vor dem Einsammeln eines Objekts aufrufen konnte. Er sollte nicht zur Bereinigung verwendet werden, weil:

- der Zeitpunkt seines Aufrufs unbekannt ist;
- er möglicherweise vor dem Ende des Prozesses gar nicht aufgerufen wird;
- er die Arbeit des Garbage Collectors erschwert;
- es sich um einen veralteten Legacy-Mechanismus handelt.

Alternativen:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

Unter Android wird die Bereinigung an den Lebenszyklus gebunden:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

In Compose erfolgt dies über `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**Kurz gesagt:** `final` beschränkt Vererbung und Überschreiben, `finally` führt nach `try` und `catch` eine deterministische Bereinigung durch und `finalize()` ist ein veralteter, unzuverlässiger GC-Mechanismus, der nicht verwendet werden sollte.

</details>

<details>
<summary>143. Wie funktioniert try-catch-finally?</summary>

#### Kotlin

`try-catch-finally` trennt eine riskante Operation, die Behandlung einer Exception und die Bereinigung voneinander.

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Reihenfolge:

```text
Erfolg -> try -> finally
Fehler -> try -> passender catch -> finally
```

Gibt es keinen passenden `catch`-Block, wird die Exception nach der Ausführung von `finally` an den Aufrufer weitergegeben.

### `catch`

Unterschiedliche Typen können separat abgefangen werden:

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

`catch`-Blöcke werden von oben nach unten geprüft. Daher müssen spezifischere Typen vor allgemeineren stehen.

Kotlin kennt keine Checked Exceptions, sodass der Compiler nicht zum Schreiben von `try` und `catch` zwingt. Eine Exception sollte dort abgefangen werden, wo der Code sich sinnvoll erholen, den Fehler umwandeln oder zusätzlichen Kontext ergänzen kann.

### `try` als Ausdruck

`try` kann einen Wert zurückgeben:

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

`finally` bestimmt nicht das Ergebnis des Ausdrucks.

### `finally`

`finally` wird zur Bereinigung verwendet:

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

Der Block wird bei Erfolg, einer Exception, `return` und dem Abbruch einer Coroutine ausgeführt. In `finally` sollte kein `return` verwendet werden, da es ein Ergebnis oder eine Exception überdecken kann.

Für `Closeable` und `AutoCloseable` ist `use` vorzuziehen:

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

Eine `CancellationException` darf nicht verschluckt, sondern muss erneut ausgelöst werden. Andernfalls wird der kooperative Abbruch beeinträchtigt.

`finally` wird auch in einer abgebrochenen Coroutine ausgeführt. Eine Suspend-Bereinigung kann jedoch sofort wieder eine Abbruchanforderung erhalten. Für eine kurze, zwingend erforderliche Suspend-Bereinigung wird gelegentlich `withContext(NonCancellable)` verwendet.

**Kurz gesagt:** `try` führt die Operation aus, `catch` behandelt eine passende Exception und `finally` bereinigt unabhängig vom Ergebnis. Bei Coroutines darf eine `CancellationException` nicht verschluckt werden; Ressourcen sollten vorzugsweise mit `use` geschlossen werden.

</details>

<details>
<summary>144. Worin unterscheiden sich Checked und Unchecked Exceptions?</summary>

#### Kotlin

In Java muss eine Checked Exception entweder abgefangen oder mit `throws` deklariert werden. Für Unchecked Exceptions besteht diese Anforderung nicht. Kotlin kennt keine Checked Exceptions; der Compiler erzwingt daher kein `try` und `catch`.

### Java

Checked Exception:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

Der Aufrufer muss `try` und `catch` verwenden oder ebenfalls `throws` deklarieren.

Unchecked Exceptions sind `RuntimeException` und deren Unterklassen:

```java
String value = null;
value.length(); // NullPointerException
```

Der Compiler verlangt keine Behandlung von `NullPointerException`, `IllegalArgumentException` oder `IllegalStateException`.

Auch `Error` ist unchecked, normalerweise jedoch nicht für eine Wiederherstellung vorgesehen.

### Kotlin

Kotlin erlaubt den Aufruf von Java- und Kotlin-APIs, die eine `IOException` auslösen können, ohne einen verpflichtenden `catch`-Block:

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

Die Exception kann dennoch zur Laufzeit auftreten. Deshalb sollte der Fehlervertrag durch die API, die Dokumentation oder den Rückgabetyp ausdrücklich sichtbar gemacht werden.

### Erwartetes Ergebnis und Programmierfehler

In der Praxis sollte zwischen folgenden Fällen unterschieden werden:

- erwartet und behebbar: keine Netzwerkverbindung, ungültige Anmeldedaten, nicht gefunden;
- Programmierfehler: ungültiges Argument, unmöglicher Zustand, Indexfehler.

Bei einer Vertragsverletzung ist eine Exception angemessen:

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

Für erwartete fachliche Ergebnisse eignet sich ein Sealed Result besser:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

So erkennt der Aufrufer die bekannten Zustände über das Typsystem.

### Java-Interoperabilität

Damit ein Java-Aufrufer `throws` sieht, wird in Kotlin `@Throws` verwendet:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

Für einen Kotlin-Aufrufer bleibt `catch` dennoch optional.

### Coroutines

Auch Exceptions in Coroutines sind unchecked. `launch` leitet einen Fehler über die Job-Hierarchie weiter, während `async` die Exception über `await()` zurückgibt.

Eine `CancellationException` ist ein Abbruchsignal und darf nicht verschluckt werden:

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

**Kurz gesagt:** Checked Exceptions sind ein Java-Mechanismus, der `catch` oder `throws` vorschreibt; Unchecked Exceptions werden vom Compiler nicht kontrolliert. Kotlin kennt keine Checked Exceptions. Daher sollten erwartete Fehler vorzugsweise durch Rückgabetypen und Vertragsverletzungen durch Exceptions modelliert werden.

</details>

<details>
<summary>145. Welche Arten von Fehlern gibt es in Kotlin und Java?</summary>

#### Kotlin

Auf der JVM erbt alles, was mit `throw` ausgelöst werden kann, von `Throwable`. Die beiden Hauptzweige sind `Exception` und `Error`.

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

Kotlin verwendet dieselbe JVM-Hierarchie, kennt auf Compilerebene jedoch keine Checked Exceptions.

### `Throwable`

Dies ist der Basistyp für alle Exceptions und Errors. `Throwable` abzufangen ist normalerweise gefährlich, da dadurch ein schwerwiegender `Error` oder der Abbruch einer Coroutine abgefangen werden kann.

### `Exception`

Situationen, die der Anwendungscode potenziell behandeln kann:

- `IOException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

### `RuntimeException`

Eine Unchecked Exception, die häufig auf einen Programmier- oder Vertragsfehler hinweist:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Beispiele sind `NullPointerException`, `IndexOutOfBoundsException`, `ClassCastException` und `NumberFormatException`.

### Checked und Unchecked Exceptions

Java verlangt bei Checked Exceptions wie `IOException` einen `catch`-Block oder eine `throws`-Deklaration. Kotlin stellt diese Anforderung nicht. `RuntimeException` und deren Unterklassen sind in beiden Sprachen unchecked.

### `Error`

Ein schwerwiegendes Problem der JVM beziehungsweise der Laufzeitumgebung:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Der Anwendungscode sollte normalerweise nicht versuchen, sich nach einem `Error` wiederherzustellen.

### Domänenfehler

Erwartete fachliche Fehler sollten besser durch einen eigenen Typ als durch eine Exception modelliert werden:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Technische Exceptions werden an der Grenze der Datenschicht in Domänenfehler umgewandelt:

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

### `CancellationException`

Bei Coroutines ist sie ein Abbruchsignal und kein gewöhnlicher Fehler:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Sie darf in einem allgemeinen `catch`-Block nicht verschluckt werden.

**Kurz gesagt:** `Throwable` gliedert sich in `Exception` und `Error`. Runtime Exceptions weisen häufig auf Vertrags- oder Programmierfehler hin, erwartete fachliche Fehler sollten besser durch Domänentypen modelliert werden und eine `CancellationException` muss bei Coroutines weitergegeben werden.

</details>

<details>
<summary>146. Was ist die Basisklasse für Fehler?</summary>

#### Kotlin

Die Basisklasse für alles, was mit `throw` ausgelöst und mit `catch` abgefangen werden kann, ist `Throwable`.

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

`Throwable` enthält grundlegende Informationen:

- Nachricht;
- Ursache;
- Stacktrace;
- unterdrückte Exceptions.

Nur `Throwable` oder eine seiner Unterklassen kann ausgelöst werden:

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // Kompilierungsfehler
```

`Exception` bezeichnet Probleme, die der Anwendungscode potenziell behandeln kann:

```kotlin
throw IOException("No internet")
throw IllegalStateException("User is not authorized")
```

`Error` bezeichnet schwerwiegende Probleme der JVM beziehungsweise der Laufzeitumgebung:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Sie werden normalerweise nicht abgefangen, um eine Wiederherstellung zu versuchen.

Benutzerdefinierte Exception:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

Für erwartete fachliche Ergebnisse ist ein Sealed Type häufig besser geeignet als eine Exception:

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

`Throwable` sollte nicht ohne Notwendigkeit abgefangen werden:

```kotlin
catch (throwable: Throwable) {
    log(throwable)
}
```

Dadurch können ein `Error` oder eine `CancellationException` abgefangen werden. Bei Coroutines muss das Abbruchsignal weitergegeben werden.

**Kurz gesagt:** Die Basisklasse ist `Throwable`. Von ihr leiten sich `Exception` und `Error` ab. In Produktionscode sollten konkrete, behebbare Exceptions statt des gesamten `Throwable` abgefangen werden.

</details>

<details>
<summary>147. Was bedeutet die Komplexität von Algorithmen?</summary>

#### Kotlin

Die Komplexität eines Algorithmus beschreibt, wie seine Laufzeit oder sein zusätzlicher Speicherbedarf mit der Eingabegröße `n` wächst. Am häufigsten wird die Big-O-Notation verwendet.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

In einem Vorstellungsgespräch sollte präzisiert werden, ob die durchschnittliche, die schlechteste oder die amortisierte Komplexität gemeint ist.

### `O(1)`

Der Aufwand wächst nicht mit `n`:

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

Der Indexzugriff in einer `ArrayList` hat `O(1)`. Die Suche in einer `HashMap` hat durchschnittlich `O(1)`.

### `O(n)`

Ein einzelner Durchlauf durch eine Collection:

```kotlin
fun findUser(users: List<User>, id: String): User? =
    users.firstOrNull { it.id == id }
```

`map`, `filter` und `contains` haben bei einer Liste normalerweise `O(n)`.

### `O(n²)`

Verschachtelter Vergleich zahlreicher Paare:

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

Für Duplikate eignet sich ein `Set` besser:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = HashSet<String>()
    return items.any { !seen.add(it) }
}
```

Die Laufzeit beträgt durchschnittlich `O(n)`, der Speicherbedarf `O(n)`. Dies ist ein Kompromiss, bei dem Speicher zugunsten höherer Geschwindigkeit eingesetzt wird.

### `O(log n)`

Binäre Suche in sortierten Daten:

```kotlin
val index = sortedItems.binarySearch(target)
```

Die Suche hat `O(log n)`, das vorherige Sortieren kann jedoch `O(n log n)` kosten.

### `O(n log n)`

Typische Komplexität vergleichsbasierter Sortierverfahren:

```kotlin
val sorted = users.sortedBy(User::name)
```

### Zeit und Speicher

```kotlin
val copy = users.toList()
```

Die Laufzeit beträgt `O(n)`, der zusätzliche Speicherbedarf ebenfalls `O(n)`. Ein In-Place-Algorithmus kann Speicher sparen, verändert jedoch die Eingabe und erschwert Ownership sowie Nebenläufigkeit.

### Android-Beispiel

Ungünstig:

```kotlin
users.map { user ->
    user.id in selectedIds // selectedIds: List, contains O(m)
}
```

Insgesamt ergibt sich `O(n × m)`.

Besser:

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

Das Erstellen des Sets kostet `O(m)`, das Mapping `O(n)`, insgesamt also durchschnittlich `O(n + m)`.

Big O ist nicht mit der tatsächlichen Performance gleichzusetzen: Konstanten, Allokationen, Ein- und Ausgabe, Cache-Lokalität und das Zeitbudget des Main Threads sind ebenfalls wichtig. Nach der Auswahl einer geeigneten Datenstruktur sollte das Ergebnis mit einem Profiler oder Benchmark überprüft werden.

**Kurz gesagt:** Die Komplexität beschreibt die Skalierung von Laufzeit und Speicherbedarf. `O(n²)` lässt sich häufig durch ein `Set` oder eine `Map` verbessern; dabei müssen jedoch Speicherbedarf, Durchschnitts- und Worst-Case-Verhalten sowie reale Messwerte berücksichtigt werden.

</details>

<details>
<summary>148. Was ist die Tiefensuche (DFS)?</summary>

#### Kotlin

`DFS` (`Depth-First Search`, Tiefensuche) ist ein Algorithmus zum Durchlaufen eines Baums oder Graphen. Er folgt einem Pfad so weit wie möglich in die Tiefe, kehrt anschließend zurück und untersucht die übrigen Verzweigungen.

Beispiel für das Durchlaufen eines Baums:

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Eine mögliche DFS-Reihenfolge:

```text
A -> B -> D -> E -> C -> F
```

### DFS für einen Baum

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

Bei einem Baum wird `visited` normalerweise nicht benötigt, da keine Zyklen vorhanden sind.

### DFS für einen Graphen

Ein Graph kann Zyklen enthalten. Deshalb wird `visited` benötigt:

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

Ohne `visited` kann DFS in eine Endlosschleife geraten.

### Iterative DFS

Die Rekursion kann durch einen Stack ersetzt werden:

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

`ArrayDeque` wird durch die LIFO-Operationen `addLast` und `removeLast` als Stack verwendet.

### Komplexität

Für einen Graphen gilt:

```text
Zeit:    O(V + E)
Speicher: O(V)
```

`V` bezeichnet die Knoten und `E` die Kanten. Der Speicher wird für `visited` sowie den Rekursionsstack oder einen expliziten Stack benötigt.

### DFS und BFS

DFS:

- geht in die Tiefe;
- verwendet Rekursion oder einen Stack;
- eignet sich zur Zyklenerkennung, für Zusammenhangskomponenten und zur topologischen Sortierung;
- garantiert in einem ungewichteten Graphen nicht den kürzesten Pfad.

BFS:

- durchläuft den Graphen Ebene für Ebene;
- verwendet eine Queue;
- findet in einem ungewichteten Graphen den kürzesten Pfad.

Eine rekursive DFS kann bei einer sehr tiefen Struktur einen `StackOverflowError` verursachen. In diesem Fall ist eine iterative DFS vorzuziehen.

**Kurz gesagt:** DFS ist eine Tiefensuche mittels Rekursion oder Stack. Bei einem Graphen wird `visited` benötigt, die Komplexität beträgt `O(V + E)` und für sehr tiefe Strukturen eignet sich die iterative Variante besser.

</details>

<details>
<summary>149. Was ist die Breitensuche (BFS)?</summary>

#### Kotlin

`BFS` (`Breadth-First Search`, Breitensuche) ist ein Algorithmus, der einen Graphen oder Baum Ebene für Ebene durchläuft. Zuerst werden alle Knoten mit der Entfernung 1 vom Startpunkt besucht, danach jene mit der Entfernung 2, dann 3 und so weiter.

BFS verwendet eine Queue mit FIFO-Verhalten:

```text
Start -> Nachbarn -> Nachbarn der Nachbarn -> ...
```

### BFS für einen Graphen

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

`visited` wird benötigt, damit die Suche in einem Graphen mit Zyklen nicht in eine Endlosschleife gerät.

### Kürzester Pfad in einem ungewichteten Graphen

BFS ermittelt die minimale Anzahl von Kanten vom Startpunkt:

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

Für einen gewichteten Graphen wird der Dijkstra-Algorithmus statt BFS benötigt.

### BFS für einen Baum

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

Dies ist ein Durchlauf nach Ebenen.

### Komplexität

```text
Zeit:     O(V + E)
Speicher: O(V)
```

`V` bezeichnet die Knoten und `E` die Kanten. Bei einem Baum beträgt die Komplexität `O(n)`.

### BFS und DFS

```text
BFS -> Queue, Ebene für Ebene, kürzester Pfad in einem ungewichteten Graphen
DFS -> Stack/Rekursion, geht in die Tiefe
```

BFS eignet sich häufig für Aufgaben wie „kleinste Anzahl von Schritten“, „nächstgelegenes Element“ oder „minimale Entfernung in einem ungewichteten Raster“.

**Kurz gesagt:** BFS durchläuft einen Graphen oder Baum mittels Queue in die Breite. Der Algorithmus findet den kürzesten Pfad in einem ungewichteten Graphen, benötigt bei Graphen mit Zyklen `visited` und hat eine Komplexität von `O(V + E)`.

</details>

<details>
<summary>150. Erläutern Sie Clean Architecture.</summary>

#### Kotlin

`Clean Architecture` ist ein Ansatz zur Aufteilung des Codes in Schichten mit einer klaren Abhängigkeitsrichtung. Die zentrale Idee besteht darin, dass die Geschäftslogik weder von der Benutzeroberfläche noch vom Android-Framework, einer Datenbank oder einem Netzwerkclient abhängt.

Typisches Android-Schema:

```text
presentation -> domain <- data
```

- `presentation` — Activity, Fragment oder Compose, ViewModel und UI-Zustand;
- `domain` — Use Cases, Geschäftsregeln, Entitäten und Repository-Verträge;
- `data` — Repository-Implementierungen, API, Room, DataStore und Mapper.

Abhängigkeitsregel:

```text
presentation hängt von domain ab
data hängt von domain ab
domain hängt von nichts Externem ab
```

Die Domain-Schicht darf nichts über Retrofit, Room, `Context`, Compose oder den Android-Lebenszyklus wissen.

### Use Case

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Das ViewModel ruft den Use Case auf und nicht direkt die API oder das DAO.

### Repository-Vertrag

In der Domain-Schicht:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

In der Datenschicht:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Die Datenschicht arbeitet mit DTOs und Entitäten und bildet sie auf das Domänenmodell ab:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

DTOs und Room-Entitäten sollten nicht direkt an die Benutzeroberfläche weitergegeben werden, wenn sie andere Lebenszyklen oder eine andere Semantik besitzen.

### Präsentationsschicht

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

Das ViewModel verwaltet den UI-Zustand und weiß nicht, aus welcher konkreten Quelle die Daten stammen.

Vorteile:

- die Geschäftslogik lässt sich ohne Android testen;
- die Datenquelle kann ausgetauscht werden;
- die Benutzeroberfläche ist nicht an API oder Datenbank gebunden;
- die Grenzen sind für das Team verständlich;
- ein großes Produkt lässt sich leichter pflegen.

Nachteile:

- mehr Dateien und Boilerplate-Code;
- bei einfachem CRUD kann der Ansatz Overengineering sein;
- ungeeignete Abstraktionen verkomplizieren den Code;
- die Abhängigkeitsrichtung erfordert Disziplin.

Praktische Regel: Nicht für jeden trivialen Getter einen Use Case oder ein Interface erstellen. Clean Architecture soll die Kopplung reduzieren und die Testbarkeit verbessern, statt lediglich die Anzahl der Klassen zu erhöhen.

**Kurz gesagt:** Clean Architecture unterteilt Android-Code normalerweise in die Schichten Presentation, Domain und Data. Die Domain-Schicht enthält Geschäftsregeln und Verträge, die Präsentationsschicht stellt den Zustand dar und die Datenschicht implementiert API- und Datenbankzugriffe. Das Hauptziel sind kontrollierte Abhängigkeiten und gute Testbarkeit.

</details>

<details>
<summary>151. Was ist MVVM?</summary>

#### Kotlin

`MVVM` ist das Architekturmuster `Model-View-ViewModel`, das die Benutzeroberfläche von der Zustandsverwaltung und der Geschäftslogik trennt.

```text
View -> ViewModel -> Model
```

- `View` — stellt die Benutzeroberfläche dar und sendet Benutzeraktionen;
- `ViewModel` — hält den Bildschirmzustand, verarbeitet Aktionen und ruft Use Cases auf;
- `Model` — Domain- und Datenschicht mit Use Cases, Repositories, API und Datenbank.

### View

Die Compose-Benutzeroberfläche sollte Zustand und Callbacks erhalten:

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

Die View greift nicht direkt auf API oder Datenbank zu und enthält keine Geschäftslogik.

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

Das ViewModel übersteht Konfigurationsänderungen und hängt nicht von einer konkreten View-Implementierung ab.

### Model-Schicht

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

Das Repository verbirgt API, Room, Cache und DataStore.

### Datenfluss

```text
Benutzeraktion -> ViewModel -> Use Case/Repository -> Zustandsaktualisierung -> View stellt Zustand dar
```

Der Bildschirmzustand sollte explizit modelliert werden:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

Navigation, Snackbar und Toast sind einmalige Effekte und sollten nicht als dauerhafter Zustand gespeichert werden:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Vorteile:

- weniger Logik in der View;
- das ViewModel lässt sich leicht testen;
- der Zustand übersteht eine Bildschirmdrehung;
- die Benutzeroberfläche kann geändert werden, ohne Domain- und Datenschicht neu zu schreiben;
- funktioniert gut mit Compose, XML, StateFlow und LiveData.

Typische Fehler:

- Fragment oder Composable enthält Geschäftslogik;
- das ViewModel arbeitet ohne klare Abgrenzung direkt mit Retrofit oder Room;
- DTOs oder Entitäten werden ohne Grund direkt an die Benutzeroberfläche weitergegeben;
- viele unabhängige veränderliche Zustände statt eines Bildschirmzustands;
- einmalige Ereignisse werden als dauerhafter Zustand gespeichert.

**Kurz gesagt:** Bei MVVM stellt die View den Zustand dar, das ViewModel verwaltet Zustand und Aktionen und Model, Use Cases sowie Repositories führen Geschäftslogik und Datenzugriffe aus. Ein gutes MVVM hält die Benutzeroberfläche schlank und das ViewModel testbar.

</details>

<details>
<summary>152. Worin unterscheiden sich MVVM und MVI?</summary>

#### Kotlin

`MVVM` und `MVI` sind Ansätze zur Verwaltung des UI-Zustands. Beide trennen die Benutzeroberfläche von der Geschäftslogik, organisieren Zustand, Ereignisse und Seiteneffekte jedoch unterschiedlich.

### MVVM

```text
View -> ViewModel -> Model
```

- `View` stellt die Benutzeroberfläche dar und ruft Methoden des ViewModels auf;
- `ViewModel` hält den Zustand und ruft Use Cases oder Repositories auf;
- `Model` bezeichnet die Domain- und Datenschicht.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() { /* Zustand aktualisieren */ }
    fun retry() { /* Zustand aktualisieren */ }
}
```

MVVM ist einfacher und flexibler. Ohne Disziplin kann sich das ViewModel jedoch in eine Sammlung beliebiger Methoden und veränderlicher Zustände verwandeln.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
                                 └-> Effect
```

Die Benutzeroberfläche sendet Ereignisse beziehungsweise Intents, statt viele unterschiedliche Methoden aufzurufen:

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

Üblicherweise gibt es einen einzigen unveränderlichen Bildschirmzustand:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

Häufig existiert nur ein Einstiegspunkt:

```kotlin
fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.LoadClicked -> load()
        ProfileEvent.RetryClicked -> retry()
    }
}
```

### Zustand und Effekte

Bei MVVM kann der Zustand aus einem einzelnen Objekt oder mehreren Streams bestehen. Bei MVI gibt es üblicherweise einen Bildschirmzustand und einen formalen Reducer beziehungsweise Handler.

In beiden Ansätzen sollten Navigation, Snackbar und Toast nicht als dauerhafter Zustand gespeichert werden. Dafür werden Effekte verwendet:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

### Auswahl des Ansatzes

MVVM:

- ist einfacher;
- benötigt weniger Boilerplate-Code;
- eignet sich gut für die meisten Android-Bildschirme;
- erleichtert einem Team den Einstieg.

MVI:

- bietet einen vorhersehbaren unidirektionalen Datenfluss;
- verwendet einen einzigen unveränderlichen Zustand;
- erleichtert das Testen der Ereignis- und Reducer-Logik;
- eignet sich besser für komplexe, zustandsbehaftete Bildschirme;
- ist für große Teams mit einem einheitlichen Datenfluss nützlich.

Ein Nachteil von MVI ist der Boilerplate-Code bei einfachen Bildschirmen. Bei MVVM besteht dagegen das Risiko einer chaotischen Zustandsverwaltung.

**Kurz gesagt:** MVVM ist einfacher: Die View ruft Methoden des ViewModels auf und stellt den Zustand dar. MVI ist formaler: Die Benutzeroberfläche sendet Ereignisse, das ViewModel oder der Reducer erzeugt einen unveränderlichen Zustand und einmalige Aktionen werden als Effekte behandelt. MVVM eignet sich für die meisten Bildschirme, MVI für komplexe zustandsbehaftete Abläufe.

</details>

<details>
<summary>153. Welche Architekturmuster werden unter Android verwendet?</summary>

#### Kotlin

Unter Android werden am häufigsten MVVM, MVI, Clean Architecture, Repository, Use Case, Schichtenarchitektur, Dependency Injection und Feature-Modularisierung eingesetzt. Die Auswahl hängt von der Komplexität der Anwendung, dem Team und den Anforderungen an die Testbarkeit ab.

### MVVM

```text
View -> ViewModel -> Model
```

Die Benutzeroberfläche stellt den Zustand dar, während das ViewModel Benutzeraktionen verarbeitet und die Domain- beziehungsweise Datenschicht aufruft.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

MVVM funktioniert gut mit Compose, XML, StateFlow und LiveData.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
```

Die Benutzeroberfläche sendet Ereignisse, das ViewModel erzeugt einen unveränderlichen Zustand und einmalige Aktionen werden über Effekte behandelt.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

MVI eignet sich für komplexe, zustandsbehaftete Bildschirme.

### Clean Architecture und Schichtenarchitektur

```text
presentation -> domain <- data
```

- `presentation` — Benutzeroberfläche und ViewModel;
- `domain` — Use Cases, Geschäftsregeln und Verträge;
- `data` — Repository-Implementierungen, API und Datenbank.

Die Domain-Schicht darf nicht vom Android-Framework, Retrofit oder Room abhängen.

### Repository-Muster

Ein Repository verbirgt die Datenquellen:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Die Implementierung kann mit API, Room, Cache oder DataStore arbeiten, ohne dass das ViewModel davon Kenntnis hat.

### Use-Case-Muster

Ein Use Case beschreibt eine konkrete Geschäftsaktion:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

Dadurch wird verhindert, dass die Geschäftslogik das ViewModel übermäßig vergrößert.

### Feature-Modularisierung

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Feature-orientierte Module erleichtern die Skalierung der Codebasis und die Zuordnung von Verantwortlichkeiten. Core sollte ausschließlich tatsächlich gemeinsam genutzte Infrastruktur enthalten.

### Dependency Injection

DI ist kein UI-Muster, gehört jedoch zur Architektur:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt, Koin oder manuelle Dependency Injection verbinden Verträge mit ihren Implementierungen.

Praktische Auswahl:

- einfache Anwendung — MVVM und Repository;
- komplexer Bildschirmzustand — MVI;
- große Anwendung — Clean Architecture und Modularisierung;
- Offline-First — Repository und lokale Single Source of Truth;
- viele Teams — Feature-Module und klare Verträge.

**Kurz gesagt:** Eine Android-Architektur kombiniert üblicherweise MVVM oder MVI für den UI-Zustand, Clean Architecture beziehungsweise Schichten für die Abhängigkeitsrichtung, Repository und Use Case für die Grenzen zwischen Domain und Daten, DI für die Verknüpfung sowie Feature-Module zur Skalierung. Ein Muster soll Komplexität reduzieren und keinen unnötigen Boilerplate-Code erzeugen.

</details>

<details>
<summary>154. Wie organisiert man Module in einem Android-Projekt?</summary>

#### Kotlin

Module werden in einem Android-Projekt zur Isolation, zur Verbesserung der Build-Performance, zur Zuordnung von Feature-Verantwortlichkeiten und zur parallelen Zusammenarbeit von Teams eingesetzt. Entscheidend ist nicht die Anzahl der Module, sondern sind geeignete Grenzen und eine klare Abhängigkeitsrichtung.

Typische Struktur:

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

Eine kleine Anwendung benötigt keine Dutzenden Module. Es empfiehlt sich, zunächst mit einer einfacheren Struktur zu beginnen.

### `:app`

Einstiegspunkt der Anwendung:

- `Application`;
- zentrale `Activity`;
- Navigation Host;
- Zusammenstellung der Dependency Injection;
- Build-Flavors und Konfiguration;
- Verknüpfung der Feature- und Datenmodule.

Geschäftslogik gehört nicht in `:app`.

### Feature-Module

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

Ein Feature-Modul enthält die UI- und Präsentationslogik eines konkreten Features. Es sollte über Domain-Verträge und Use Cases arbeiten und nicht direkt mit Retrofit- oder Room-Implementierungen.

### Core-Module

```text
:core:network  -> Konfiguration von Retrofit/OkHttp
:core:database -> Konfiguration von Room
:core:ui       -> Designsystem und Komponenten
:core:common   -> Hilfsfunktionen, Dispatcher und Ergebnistypen
```

Core darf nicht von einem Feature abhängen.

### Domain-Module

Die Domain-Schicht enthält Geschäftsregeln und Verträge:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

Die Domain-Schicht sollte möglichst unabhängig vom Android-Framework sein.

### Datenmodule

Die Datenschicht implementiert die Domain-Verträge:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Hier befinden sich DTOs, Room-Entitäten, Mapper sowie API- und Datenbank-Datenquellen.

### Abhängigkeitsrichtung

```text
feature -> domain
 data   -> domain
 app    -> feature + data
core    -> kennt feature nicht; feature darf core kennen
```

Zyklische Abhängigkeiten sind zu vermeiden. `core` darf kein konkretes Feature kennen.

### Build-Logik

Wiederkehrende Gradle-Konfiguration sollte in Convention Plugins ausgelagert werden:

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

Dadurch werden Duplikate in Build-Skripten reduziert.

Eine Aufteilung ist nicht sinnvoll, wenn:

- keine separate Verantwortlichkeit besteht;
- keine wiederverwendbare Grenze vorhanden ist;
- kein Vorteil bei der Build-Zeit entsteht;
- das Modul lediglich Boilerplate-Code für DI, Navigation oder Gradle hinzufügt.

**Kurz gesagt:** Module sollten um `app`, `feature`, `core`, `domain` und `data` herum aufgebaut werden. Eine Aufteilung ist sinnvoll, wenn eine reale Grenze existiert: Feature-Verantwortlichkeit, wiederverwendbare Infrastruktur, Domain-Vertrag, Datenimplementierung oder ein Vorteil beim Build. Übermäßige Modularisierung ist nachteilig.

</details>

<details>
<summary>155. Was ist ein Token?</summary>

#### Kotlin

Ein Token ist ein Berechtigungsnachweis, der eine Sitzung oder ein Zugriffsrecht bestätigt. In mobilen Anwendungen und Backend-Systemen gibt es üblicherweise ein `Access Token` und ein `Refresh Token`.

### Access Token

Es besitzt eine kurze Gültigkeitsdauer und wird bei Anfragen an eine geschützte API übertragen:

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

### Refresh Token

Es ist länger gültig und wird ausschließlich zur Erneuerung des Access Tokens verwendet:

```text
Anfrage -> 401 -> Erneuerung -> neues Access Token -> Anfrage wiederholen
```

Die Erneuerung muss synchronisiert werden: Mehrere parallele `401`-Antworten dürfen nicht mehrere Erneuerungsanfragen starten. Die Anzahl der Wiederholungsversuche muss begrenzt sein, um eine Endlosschleife zu vermeiden.

### JWT und Bearer

Ein JWT besitzt folgendes Format:

```text
header.payload.signature
```

Die Payload ist lediglich Base64URL-kodiert und nicht verschlüsselt. Deshalb dürfen darin keine geheimen Daten gespeichert werden.

Ein Bearer Token kann von jeder Person verwendet werden, die es besitzt. Wird das Token kompromittiert, ist ein Zugriff bis zum Ablauf oder Widerruf möglich.

### Speicherung unter Android

Für sensible Daten geeignete Speicher:

- Android Keystore;
- EncryptedSharedPreferences;
- verschlüsselter DataStore.

Die Implementierung sollte hinter einem Interface verborgen werden:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Tokens dürfen weder in Protokolle, Absturzberichte, Analysedaten, Screenshots, URL-Abfrageparameter noch in unverschlüsselte Dateien gelangen.

### Ablauf und Scopes

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)
```

Eine lokale Prüfung des Ablaufzeitpunkts ermöglicht eine frühzeitige Erneuerung des Tokens. Die maßgebliche Quelle bleibt jedoch das Backend. Schlägt die Erneuerung fehl, muss die Sitzung gelöscht und der Benutzer zur Anmeldung weitergeleitet werden.

Scopes sollten auf das notwendige Minimum beschränkt sein, beispielsweise `profile:read` oder `payments:create`. Eine mobile Anwendung darf weder Administratorrechte noch Servergeheimnisse erhalten.

Beim Abmelden müssen die lokalen Tokens gelöscht und, sofern das Backend dies unterstützt, das Refresh Token widerrufen werden.

**Kurz gesagt:** Ein Token ist ein Berechtigungsnachweis für den Zugriff. Das Access Token wird bei API-Anfragen übertragen, das Refresh Token erneuert das Access Token. Wichtig sind eine kurze Gültigkeitsdauer, minimale Scopes, sichere Speicherung, eine synchronisierte Erneuerung und das vollständige Fernhalten von Tokens aus Protokollen.

</details>

<details>
<summary>156. Worin unterscheiden sich Authentifizierung und Autorisierung?</summary>

#### Kotlin

```text
Authentifizierung -> Wer bist du?
Autorisierung     -> Was darfst du?
```

Die Authentifizierung stellt die Identität beziehungsweise den Principal fest. Die Autorisierung prüft die Richtlinie für eine konkrete Operation oder Ressource.

### Authentifizierung

Die Identität wird durch Passwort, Einmalpasswort, Passkey oder Biometrie, OAuth beziehungsweise OIDC oder eine gültige Sitzung bestätigt.

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Nach der Anmeldung stellt das Backend ein Sitzungscookie oder Tokens aus. Das Access Token wird API-Anfragen hinzugefügt, während das Refresh Token ausschließlich zum Abrufen eines neuen Access Tokens verwendet wird.

### Autorisierung

Die Autorisierung prüft, ob der Principal eine bestimmte Aktion ausführen darf:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

Das Backend berücksichtigt Rollen, Berechtigungen, den Eigentümer der Ressource, den Mandanten und den Kontext.

Typische Modelle:

- RBAC — rollenbasiert;
- berechtigungsbasiert — granulare Berechtigungen;
- ABAC beziehungsweise richtlinienbasiert — Attribute von Principal, Ressource und Kontext.

### `401` und `403`

```text
401 Unauthorized -> keine gültigen Authentifizierungsdaten vorhanden
403 Forbidden    -> Authentifizierungsdaten vorhanden, Zugriff jedoch verweigert
```

Bei `401` kann die Anwendung einmalig und synchronisiert eine Token-Erneuerung durchführen. Schlägt diese fehl, muss die Sitzung beendet werden.

Bei `403` hilft eine erneute Authentifizierung normalerweise nicht. Die Benutzeroberfläche sollte anzeigen, dass kein Zugriff besteht.

### Android-Client und Backend

Der Client kann Schaltflächen anhand von Berechtigungen ausblenden:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDelete) {
        Text("Delete")
    }
}
```

Dies ist jedoch lediglich eine Maßnahme für die Benutzerfreundlichkeit. Ein modifizierter Client kann die API direkt aufrufen, weshalb das Backend die Autorisierung immer selbst durchsetzen muss.

Aufteilung der Verantwortlichkeiten:

- der Client speichert die Authentifizierungsdaten und verarbeitet `401` sowie `403`;
- der Authentifizierungsdienst stellt Authentifizierungsdaten aus und prüft sie;
- die Richtlinienschicht entscheidet über den Zugriff;
- das Backend vertraut keinen Berechtigungen, die vom Client übermittelt werden;
- Abmeldung und Widerruf beenden die Sitzung entsprechend dem Sicherheitsmodell.

**Kurz gesagt:** Die Authentifizierung bestätigt die Identität, die Autorisierung prüft den Zugriff auf eine Operation oder Ressource. Der Client ist für Authentifizierungsdaten und Benutzerführung verantwortlich, die tatsächliche Durchsetzung der Sicherheit muss jedoch immer im Backend erfolgen.

</details>

<details>
<summary>157. Welche Werkzeuge werden für die Kommunikation mit dem Backend verwendet?</summary>

#### Kotlin

Der Netzwerk-Stack hängt vom Protokoll ab: REST beziehungsweise HTTP, GraphQL, WebSocket, gRPC oder KMP-Networking. Zusätzlich werden Serialisierung, Authentifizierung, Fehlerabbildung sowie eine Cache- und Offline-Strategie benötigt.

### REST/HTTP

Am häufigsten werden Retrofit und OkHttp verwendet.

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

OkHttp führt die Anfragen aus, während Retrofit die API als Kotlin-Interface beschreibt.

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Interceptors ergänzen Header, Authentifizierung, Protokollierung und Tracing. Wiederholungsversuche müssen die Idempotenz berücksichtigen.

### Serialisierung

Für JSON werden Kotlin Serialization, Moshi oder Gson verwendet.

DTOs sollten vom Domänenmodell getrennt werden:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### KMP

Für Kotlin Multiplatform wird häufig Ktor Client eingesetzt. Engine und plattformspezifische Konfiguration werden separat bereitgestellt.

### GraphQL

Apollo Kotlin generiert typsichere Modelle aus GraphQL-Operationen. GraphQL bringt ein Schema und Codegenerierung sowie eigene Besonderheiten bei Caching und Fehlerbehandlung mit sich.

### WebSocket

WebSockets werden für Echtzeitanwendungen wie Chats, Live-Aktualisierungen und Marktdaten eingesetzt. Unter Android unterstützen OkHttp und Ktor WebSockets.

### gRPC

gRPC ist ein Contract-First-RPC-System auf Grundlage von Protocol Buffers und generierten Clients. Es ist sinnvoll, wenn das Backend bereits über eine Protobuf- beziehungsweise gRPC-Infrastruktur verfügt.

### Authentifizierung

Typische Varianten:

- Bearer Access und Refresh Tokens;
- OAuth beziehungsweise OIDC;
- Cookies;
- API-Schlüssel.

Ein Interceptor ergänzt die Authentifizierungsdaten. Ein `Authenticator` oder ein separater Sitzungsmanager koordiniert die Erneuerung nach einer `401`-Antwort. Die Erneuerung muss synchronisiert werden, damit parallele `401`-Antworten nicht mehrere Erneuerungsanfragen starten.

### Fehlerbehandlung und Cache

Die Netzwerkschicht sollte Transport- und HTTP-Fehler auf eigene Domain- oder Datenfehler abbilden. Die Benutzeroberfläche darf nicht von `HttpException` oder einem konkreten Client abhängen.

Offline und Cache:

- HTTP-Cache — Zwischenspeicherung von Antworten;
- Room — strukturierte Single Source of Truth;
- Paging und RemoteMediator — seitenweise lokale und entfernte Synchronisierung;
- Repository — bestimmt, aus welcher Quelle gelesen und wann aktualisiert wird.

Praktische Auswahl:

- REST — Retrofit und OkHttp;
- KMP — Ktor Client;
- GraphQL — Apollo Kotlin;
- Echtzeitkommunikation — WebSocket;
- Protobuf-RPC — gRPC;
- Offline-Single-Source-of-Truth — Room und Repository.

**Kurz gesagt:** Für REST werden üblicherweise Retrofit und OkHttp verwendet, für KMP Ktor, für GraphQL Apollo, für Echtzeitkommunikation WebSockets und für RPC gRPC. Neben den Anfragen sind Authentifizierung, Fehlerabbildung, die Abbildung von DTOs auf Domänenmodelle und eine Offline-Strategie entscheidend.

</details>

<details>
<summary>158. Was ist Retrofit?</summary>

#### Kotlin

`Retrofit` ist eine Bibliothek für die Arbeit mit REST-APIs unter Android und auf der JVM. Sie beschreibt HTTP-Endpunkte durch Kotlin- oder Java-Interfaces und führt die Anfragen über OkHttp aus.

API-Interface:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto

    @POST("users")
    suspend fun createUser(@Body body: CreateUserRequest): UserDto
}
```

Retrofit erzeugt zur Laufzeit eine Implementierung dieses Interfaces.

Konfiguration:

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val api = retrofit.create(UserApi::class.java)
```

Die `baseUrl` muss mit `/` enden.

Retrofit unterstützt Suspend-Funktionen:

```kotlin
val user = api.getUser("123")
```

Annotationen beschreiben die HTTP-Anfrage:

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

### Converter

Retrofit analysiert JSON nicht selbst. Dafür wird ein Converter benötigt:

- Moshi;
- Gson;
- Kotlin Serialization;
- Scalars.

DTOs sollten auf Domänenmodelle abgebildet werden:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### Verarbeitung der Antwort

Eine Methode kann das DTO direkt oder `Response<T>` zurückgeben:

```kotlin
suspend fun getUser(id: String): Response<UserDto>
```

`Response<T>` wird benötigt, wenn Statuscode, Header oder Fehlerinhalt gelesen werden müssen.

### OkHttp-Interceptors

Authentifizierung, Header, Protokollierung und Cache werden über OkHttp hinzugefügt:

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

### Fehlerbehandlung

Retrofit und OkHttp können folgende Fehler liefern:

- `IOException` — Netzwerkfehler;
- `HttpException` oder eine Nicht-2xx-`Response`;
- Serialisierungsfehler.

Im Repository sollten diese auf einen Domain- oder Datenfehler abgebildet werden, statt `HttpException` an die Benutzeroberfläche weiterzugeben.

Schichtengrenzen im Produktivcode:

```text
ViewModel -> Use Case -> Repository -> Retrofit-API
```

**Kurz gesagt:** Retrofit beschreibt REST-Endpunkte durch Interfaces und Annotationen, arbeitet auf Grundlage von OkHttp und unterstützt Coroutines sowie JSON-Converter. Im Produktivcode gehören die Retrofit-API und DTOs hinter ein Repository in die Datenschicht.

</details>

<details>
<summary>159. Was ist Apollo GraphQL?</summary>

#### Kotlin

Apollo Kotlin ist ein typsicherer GraphQL-Client für Kotlin und Android. Er generiert Kotlin-Code aus einem Schema und aus `.graphql`-Operationen und unterstützt Queries, Mutations, Subscriptions sowie einen normalisierten Cache.

Bei GraphQL beschreibt der Client selbst die benötigten Felder:

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

Apollo generiert Kotlin-Typen:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

Entspricht eine Query nicht dem Schema, wird der Fehler bei der Codegenerierung beziehungsweise zur Kompilierzeit erkannt.

Client-Konfiguration:

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

Die Authentifizierung wird mit einem Interceptor ergänzt:

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

Subscription für Echtzeitaktualisierungen:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
    }
```

### Cache und Fehler

Ein normalisierter Cache speichert Entitäten anhand eines Schlüssels, sodass unterschiedliche Queries dieselben Daten wiederverwenden können. Dies setzt stabile IDs im Schema voraus.

Eine GraphQL-Antwort kann gleichzeitig `data` und `errors` enthalten:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()

if (response.hasErrors()) {
    // response.errors auf Domänenfehler abbilden
}

val data = response.data
```

Ein GraphQL-Fehler bedeutet nicht zwingend einen HTTP-Statuscode 4xx oder 5xx. Ein Netzwerkfehler wird normalerweise als Exception übermittelt. Generierte Modelle sollten auf Domänenmodelle abgebildet werden.

Apollo und Retrofit:

- Apollo — GraphQL, der Client bestimmt die Form der Antwort, Modelle werden generiert;
- Retrofit — REST, das Backend bestimmt Endpunkte und Antworten, DTOs werden manuell erstellt.

**Kurz gesagt:** Apollo Kotlin ist ein typsicherer GraphQL-Client mit Codegenerierung, Queries, Mutations, Subscriptions und normalisiertem Cache. Er wird verwendet, wenn das Backend GraphQL statt REST-Endpunkten einsetzt.

</details>

<details>
<summary>160. Wofür wird Firebase verwendet?</summary>

#### Kotlin

`Firebase` ist eine Sammlung von Backend-as-a-Service-Werkzeugen von Google für mobile Anwendungen und Webanwendungen. Unter Android wird Firebase für Analysen, Absturzberichte, Push-Benachrichtigungen, Authentifizierung, Remote-Konfiguration, Datenbanken, Speicher und Backend-Funktionen verwendet.

### Analytics

Erfassung von Benutzerereignissen, Funnels, Nutzerbindung und Feature-Nutzung:

```kotlin
firebaseAnalytics.logEvent(
    "profile_opened",
    bundleOf("source" to "home")
)
```

### Crashlytics

Überwachung von Abstürzen und nicht schwerwiegenden Fehlern:

```kotlin
Firebase.crashlytics.recordException(error)
```

Dies ist ein grundlegendes Produktionswerkzeug für die Stabilität einer Anwendung.

### FCM

Firebase Cloud Messaging dient für Push-Benachrichtigungen und Messaging-Ereignisse:

```kotlin
class AppMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // Push-Nachricht verarbeiten
    }
}
```

### Firebase Auth

Eine fertige Authentifizierungsplattform:

- E-Mail und Passwort;
- Google Sign-In;
- Authentifizierung per Telefonnummer;
- anonyme Authentifizierung;
- benutzerdefinierte Tokens.

Sie muss korrekt in das Backend- und Sicherheitsmodell integriert werden.

### Firestore und Realtime Database

NoSQL-Cloud-Datenbanken mit Echtzeitaktualisierungen:

```kotlin
firestore.collection("users").document(id).get()
```

Sie eignen sich für Chats, Echtzeitsynchronisierung, kollaborative Funktionen und Prototypen. Sicherheitsregeln sind dabei von entscheidender Bedeutung.

### Remote Config

Änderung von Parametern ohne Veröffentlichung einer neuen Anwendungsversion:

```kotlin
val enabled = remoteConfig.getBoolean("new_feature_enabled")
```

Anwendungsfälle sind Feature Flags, schrittweise Einführung, A/B-Tests sowie die Konfiguration von Benutzeroberfläche und Verhalten.

### Storage und Functions

Cloud Storage dient für Benutzer-Uploads wie Bilder, Videos und Dateien.

```kotlin
storage.reference.child("avatars/$userId.jpg")
```

Cloud Functions dienen für serverseitige Logik, beispielsweise Firestore-Trigger, API-Endpunkte, den Versand von Push-Nachrichten, Validierung und Integrationen mit Drittanbietern.

Vorteile:

- schneller Einstieg;
- fertige Lösungen für Authentifizierung, Analysen, Abstürze und Push-Nachrichten;
- gute Android-Integration;
- Echtzeitfunktionen;
- zu Beginn weniger eigene Backend-Infrastruktur.

Risiken:

- Abhängigkeit von einem Anbieter;
- Fehler in Sicherheitsregeln;
- unerwartete Kosten bei zunehmender Nutzung;
- nicht immer für komplexe Geschäftslogik im Backend geeignet;
- clientseitiger Zugriff ersetzt keine Backend-Sicherheit;
- das Offline- und Cache-Verhalten muss gesondert verstanden werden.

**Kurz gesagt:** Firebase beschleunigt die Android-Entwicklung durch fertige Lösungen für Authentifizierung, Analytics, Crashlytics, FCM, Firestore beziehungsweise Realtime Database, Remote Config, Storage und Functions. Erforderlich sind jedoch Sicherheitsregeln, Kostenkontrolle, klare Grenzen und ein Bewusstsein für die Anbieterabhängigkeit.

</details>

<details>
<summary>161. Was ist der Android Keystore?</summary>

#### Kotlin

Der `Android Keystore` ist ein Systemspeicher für kryptografische Schlüssel. Er ermöglicht das Erstellen und Verwenden von Schlüsseln, ohne dass das rohe Schlüsselmaterial den Keystore verlässt. Auf einigen Geräten können die Schlüssel hardwaregestützt sein.

Der Keystore wird verwendet für:

- Verschlüsselung und Entschlüsselung;
- Signierung und Verifizierung;
- lokale Geheimnisse;
- Schlüssel zur Verschlüsselung von EncryptedSharedPreferences oder DataStore;
- durch Biometrie oder Benutzerauthentifizierung geschützte Schlüssel;
- die Verringerung des Risikos, dass rohes kryptografisches Schlüsselmaterial offengelegt wird.

Eine wichtige Einschränkung: Der Keystore macht einen fest im Code hinterlegten API-Schlüssel nicht sicher. Befindet sich ein Geheimnis in der APK, kann ein Angreifer die Stelle finden, an der die Anwendung es verwendet. Der Keystore eignet sich am besten für Schlüssel, die auf dem Gerät erzeugt wurden.

Erzeugen eines AES-Schlüssels:

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

Abrufen des Schlüssels:

```kotlin
val keyStore = KeyStore.getInstance("AndroidKeyStore").apply { load(null) }
val key = keyStore.getKey("secret_key", null) as SecretKey
```

Man erhält einen Verweis auf den Schlüssel und nicht dessen rohe Bytes.

Verschlüsselung:

```kotlin
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)
val iv = cipher.iv
val encrypted = cipher.doFinal(data)
```

Bei AES-GCM muss der `iv` zusammen mit dem Chiffretext gespeichert werden. Derselbe Initialisierungsvektor darf mit demselben Schlüssel nicht wiederverwendet werden.

Ein Schlüssel kann an die Benutzerauthentifizierung gebunden werden:

```kotlin
.setUserAuthenticationRequired(true)
```

Für typische Aufgaben sollte vorzugsweise Jetpack Security verwendet werden:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()
```

Keystore und Jetpack Security eignen sich für Refresh Tokens, lokale Verschlüsselungsschlüssel, benutzerspezifische Geheimnisse und sensible Einstellungen. Server- oder Backend-Geheimnisse dürfen nicht in der Anwendung gespeichert werden; dafür ist ein Backend-Proxy erforderlich.

Risiken:

- ungeeigneter kryptografischer Modus;
- Wiederverwendung des Initialisierungsvektors;
- fehlende Schlüsselrotation;
- Sonderfälle bei Sicherung und Wiederherstellung;
- unterschiedliches Verhalten verschiedener Android-Versionen und Geräte;
- der Versuch, ein fest codiertes Geheimnis statt durch eine geeignete Architektur zu schützen.

**Kurz gesagt:** Der Android Keystore schützt kryptografische Schlüssel auf dem Gerät und gibt kein rohes Schlüsselmaterial heraus. Er eignet sich für lokale Verschlüsselung, Signierung und Benutzertokens, schützt jedoch keine fest in einer APK hinterlegten API-Geheimnisse. Für typische Aufgaben sollte Jetpack Security auf Grundlage des Keystores verwendet werden.

</details>

<details>
<summary>162. Welche Dispatcher gibt es in Kotlin Coroutines und wofür werden sie verwendet?</summary>

#### Kotlin

Ein `CoroutineDispatcher` bestimmt, auf welchem Thread oder Thread-Pool eine Coroutine ausgeführt wird. Der richtige Dispatcher ist entscheidend für eine reaktionsfähige Benutzeroberfläche, gute Performance und die Vermeidung von ANRs.

### `Dispatchers.Main`

Der Hauptthread der Benutzeroberfläche:

```kotlin
withContext(Dispatchers.Main) {
    textView.text = "Loaded"
}
```

Er wird für Aktualisierungen der Benutzeroberfläche, den Compose- oder View-Zustand, Navigation und an den Lebenszyklus gebundene UI-Logik verwendet. Er ist nicht für Netzwerkzugriffe, Datenbanken, Datei-Ein- und -Ausgabe oder aufwendige Berechnungen vorgesehen.

### `Dispatchers.IO`

Für blockierende Ein- und Ausgabe:

```kotlin
withContext(Dispatchers.IO) {
    file.readText()
}
```

Er eignet sich für Dateioperationen, Datenbanken, Netzwerkclients und blockierende SDK-Aufrufe, jedoch nicht für rechenintensive Algorithmen.

### `Dispatchers.Default`

Für rechenintensive Arbeit:

```kotlin
withContext(Dispatchers.Default) {
    largeList.sortedBy { it.score }
}
```

Er eignet sich zum Sortieren, zum Parsen großer Strukturen, zur Komprimierung, zur Berechnung von Unterschieden und für sonstige Berechnungen.

### `Dispatchers.Unconfined`

Dieser Dispatcher bindet eine Coroutine nicht an einen bestimmten Thread. Sie wird dort fortgesetzt, wo die Suspension beendet wurde.

```kotlin
launch(Dispatchers.Unconfined) { }
```

Im produktiven Android-Code wird er kaum benötigt, da sein Verhalten weniger vorhersehbar ist.

### `limitedParallelism`

```kotlin
val limitedIo = Dispatchers.IO.limitedParallelism(4)
```

Dies ist nützlich, um die Anzahl paralleler Uploads, API-Aufrufe, Batch-Aufgaben oder Bildverarbeitungen zu begrenzen.

### `withContext`

Zum Wechseln des Dispatchers innerhalb einer Suspend-Funktion:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

Dies ist besser, als ausschließlich für einen Threadwechsel eine neue Coroutine zu starten.

### Dispatcher injizieren

Für eine gute Testbarkeit sollten Dispatcher über den Konstruktor übergeben werden:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): User = withContext(ioDispatcher) {
        api.getUser()
    }
}
```

In einem Test kann ein `StandardTestDispatcher` übergeben werden.

Typische Fehler:

- Netzwerk- oder Datenbankzugriffe auf `Main`;
- rechenintensive Arbeit auf `IO`;
- `runBlocking` auf dem Main Thread;
- fest codierte Dispatcher in zu testendem Code;
- benutzerdefinierte Thread-Pools ohne Bereinigung;
- `Unconfined` ohne klaren Grund.

**Kurz gesagt:** `Main` dient der Benutzeroberfläche, `IO` blockierender Ein- und Ausgabe, `Default` rechenintensiver Arbeit und `Unconfined` seltenen Spezialfällen. Für die Testbarkeit sollten Dispatcher injiziert und Dispatcherwechsel mit `withContext` durchgeführt werden.

</details>

<details>
<summary>163. Welche Möglichkeiten gibt es, Coroutines zu starten?</summary>

#### Kotlin

Eine Coroutine wird in einem `CoroutineScope` gestartet. Am häufigsten werden `launch`, `async`, `coroutineScope`, `supervisorScope` und Android-spezifische Scopes verwendet.

### `launch`

Startet eine Coroutine ohne fachlichen Rückgabewert und gibt ein `Job` zurück:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

```kotlin
val job = scope.launch { work() }
job.cancel()
```

`launch` eignet sich für Seiteneffekte. Fehler werden über die Scope-Hierarchie oder einen lokalen `try`- und `catch`-Block behandelt.

### `async`

Gibt ein `Deferred<T>` für ein nebenläufig berechnetes Ergebnis zurück:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

Wird ohne Nebenläufigkeit sofort `await()` aufgerufen, sollte stattdessen direkt die Suspend-Funktion ausgeführt werden.

### `coroutineScope`

Erstellt eine Grenze für strukturierte Nebenläufigkeit und wartet auf alle untergeordneten Coroutines:

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

Der Fehler einer untergeordneten Coroutine bricht ihre Geschwister ab und wird an den Aufrufer weitergegeben.

### `supervisorScope`

Isoliert Geschwister-Coroutines vom Fehler eines direkten Kindprozesses:

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

Der Fehler muss weiterhin behandelt oder über `await()` abgerufen werden.

### `runBlocking`

Blockiert den aufrufenden Thread. Es eignet sich für den Einstiegspunkt einer Kommandozeilenanwendung oder eine stark begrenzte synchrone Brücke. In Tests ist `runTest` vorzuziehen; auf dem Android Main Thread darf `runBlocking` wegen des ANR-Risikos nicht verwendet werden.

### Android-Scopes

`viewModelScope` dient für bildschirmbezogene Operationen und wird in `onCleared()` abgebrochen.

`lifecycleScope` gehört einem `LifecycleOwner`. Für Flow in der Benutzeroberfläche wird `repeatOnLifecycle` verwendet:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

In einem Fragment wird für View und Binding `viewLifecycleOwner.lifecycleScope` verwendet.

In Compose dient `rememberCoroutineScope()` für durch Callbacks ausgelöste UI-Aktionen:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

`GlobalScope` ist im Anwendungscode fast immer ungeeignet, da Besitzer, Lebenszyklus und Bereinigung fehlen.

**Kurz gesagt:** `launch` dient für Seiteneffekte, `async` für nebenläufige Ergebnisse, `coroutineScope` als Fail-Fast-Grenze und `supervisorScope` zur Isolation von Geschwister-Coroutines. Der Scope wird nach dem Besitzer der Arbeit ausgewählt; `GlobalScope` und `runBlocking` auf dem Main Thread werden nicht verwendet.

</details>

<details>
<summary>164. Worin unterscheiden sich launch und async?</summary>

#### Kotlin

`launch` und `async` sind Coroutine Builder.

```text
launch -> Job, ohne Rückgabewert
async  -> Deferred<T>, Ergebnis über await()
```

### `launch`

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

Ein `Job` ermöglicht `cancel()`, `join()` und die Prüfung seines Zustands. `launch` wird verwendet, wenn kein Ergebnis nach außen zurückgegeben werden muss, beispielsweise für eine Aktualisierung des UI-Zustands, die Erfassung eines Flows, das Speichern oder die Synchronisierung.

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

`Deferred<T>` ist ein `Job` mit einem Ergebnis oder einer Exception. `async` wird hauptsächlich für mehrere nebenläufig berechnete Ergebnisse benötigt:

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

Ohne `async` würden die Suspend-Aufrufe nacheinander ausgeführt. Die tatsächliche parallele Ausführung hängt jedoch vom Dispatcher ab.

### Exceptions und strukturierte Nebenläufigkeit

In einem `coroutineScope` bricht der Fehler einer untergeordneten Coroutine ihre Geschwister ab und wird an den Aufrufer weitergegeben. Die Exception von `async` wird über `await()` abgerufen; innerhalb eines strukturierten Scopes wirkt sich der Fehler dennoch auf den Parent aus.

Für voneinander unabhängige Fehler werden `supervisorScope` und eine explizite Fehlerstrategie benötigt.

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

Eine `CancellationException` darf nicht verschluckt werden.

### Typische Fehler

Ungünstig:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

Wird kein Wert benötigt, sollte `launch` verwendet werden. Wird ein einzelner Wert ohne Nebenläufigkeit benötigt, sollte die Suspend-Funktion direkt aufgerufen werden. Für einen Dispatcherwechsel dient `withContext` und nicht `async`.

Praktische Auswahl:

- Seiteneffekt ohne Ergebnis — `launch`;
- mehrere nebenläufig berechnete Ergebnisse — `async` und `await`;
- ein einzelnes Ergebnis — Suspend-Funktion;
- Dispatcherwechsel — `withContext`;
- zusammengehörige untergeordnete Coroutines — `coroutineScope`;
- unabhängige Fehler — `supervisorScope`.

**Kurz gesagt:** `launch` gibt einen `Job` für Arbeit ohne Ergebnis zurück. `async` gibt ein `Deferred<T>` zurück und wird für nebenläufig berechnete Ergebnisse verwendet, die über `await()` abgerufen werden. `async` sollte nicht verwendet werden, wenn kein Ergebnis benötigt wird.

</details>

<details>
<summary>165. Wie werden Fehler in Coroutines behandelt?</summary>

#### Kotlin

Die Strategie hängt vom Besitzer der Arbeit ab. Erwartete Fehler werden lokal behandelt und auf ein Ergebnis oder einen Zustand abgebildet. Unbehandelte Exceptions verbreiten sich über die `Job`-Hierarchie. Eine `CancellationException` darf nicht verschluckt werden.

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

Es sollten konkrete Exceptions an einer Grenze abgefangen werden, an der klar ist, wie mit ihnen umzugehen ist.

Im ViewModel wird ein Fehler auf den UI-Zustand abgebildet, statt eine rohe `HttpException` oder `IOException` an die Benutzeroberfläche weiterzugeben.

### `CancellationException`

Bei einem allgemeinen `catch`-Block muss das Abbruchsignal weitergegeben werden:

```kotlin
catch (error: CancellationException) {
    throw error
} catch (error: Exception) {
    handleError(error)
}
```

Andernfalls kann der kooperative Abbruch beeinträchtigt werden.

### `CoroutineExceptionHandler`

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

Ein Handler ist die letzte Instanz für eine nicht abgefangene Exception in einem Root-`launch` und dient üblicherweise der Protokollierung. Er stellt die Coroutine nicht wieder her und ersetzt keine lokale Fehlerbehandlung.

### Exceptions bei `async`

`await()` gibt ein Ergebnis zurück oder löst eine Exception aus:

```kotlin
val deferred = async { api.getUser() }

try {
    val user = deferred.await()
} catch (error: IOException) {
    handleError(error)
}
```

In einem gewöhnlichen `coroutineScope` bricht der Fehler einer untergeordneten Coroutine den Parent und ihre Geschwister ab. Ein `try`- und `catch`-Block um `await()` isoliert parallele Aufgaben nicht immer. Für unabhängige Fehler wird `supervisorScope` benötigt.

### `coroutineScope` und `supervisorScope`

```text
coroutineScope  -> Fail-Fast, Fehler eines Kindes bricht Geschwister ab
supervisorScope -> Fehler eines Kindes bricht Geschwister nicht automatisch ab
```

```kotlin
supervisorScope {
    val a = async { runCatching { requestA() } }
    val b = async { runCatching { requestB() } }
}
```

Der Fehler muss dennoch behandelt werden.

### `Flow.catch`

```kotlin
repository.users()
    .catch { error -> emit(emptyList()) }
    .collect { users -> render(users) }
```

`catch` fängt vorgelagerte Exceptions ab. Ein Abbruch darf nicht in einen Ersatzwert umgewandelt werden.

Typische Fehler:

- eine `CancellationException` verschlucken;
- sich ausschließlich auf `CoroutineExceptionHandler` verlassen;
- einen Fehler aus `await()` ignorieren;
- rohe Exceptions an die Benutzeroberfläche weitergeben;
- Wiederholungsversuche, Fehlerabbildung und Darstellung an derselben Stelle vermischen.

**Kurz gesagt:** Erwartete Fehler werden mit `try` und `catch`, `await()` oder `Flow.catch` auf ein Ergebnis oder einen Zustand abgebildet. `coroutineScope` arbeitet nach dem Fail-Fast-Prinzip, `supervisorScope` isoliert Geschwister-Coroutines und eine `CancellationException` muss immer weitergegeben werden.

</details>

<details>
<summary>166. Wie wartet man auf die Ergebnisse mehrerer paralleler Anfragen?</summary>

#### Kotlin

Unabhängige Suspend-Anfragen werden mit `async` in einem strukturierten Scope gestartet. Auf ihre Ergebnisse wird mit `await()` oder `awaitAll()` gewartet.

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

Beide `async`-Aufrufe werden vor dem ersten `await()` erstellt, sodass die Anfragen nebenläufig ausgeführt werden. Die tatsächliche Parallelität hängt vom Dispatcher und vom HTTP-Client ab.

### `coroutineScope`

`coroutineScope`:

- wartet auf alle untergeordneten Coroutines;
- gibt den Abbruch des Aufrufers an sie weiter;
- arbeitet nach dem Fail-Fast-Prinzip: Der Fehler einer untergeordneten Coroutine bricht ihre Geschwister ab.

Dies ist die richtige Strategie, wenn alle Ergebnisse benötigt werden.

### `awaitAll`

Für eine Liste gleichartiger Anfragen:

```kotlin
val users = coroutineScope {
    ids.map { id ->
        async { api.getUser(id) }
    }.awaitAll()
}
```

`awaitAll()` bewahrt die Reihenfolge der Deferred-Liste und gibt einen Fehler nach dem Fail-Fast-Prinzip zurück.

Voneinander abhängige Aufrufe sollten nicht parallelisiert werden:

```kotlin
val token = api.login()
val profile = api.getProfile(token)
```

### Teilerfolg

Wird ein Teilergebnis benötigt, werden `supervisorScope` und ein typisiertes Ergebnis für jede untergeordnete Coroutine verwendet:

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

`runRequest` muss eine `CancellationException` erneut auslösen und darf nur erwartete Fehler abbilden.

### Zeitüberschreitung

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

Eine Zeitüberschreitung der Coroutine bricht den Scope ab. Der HTTP-Client muss dennoch über eigene Zeitlimits für Verbindungsaufbau, Lesen und den gesamten Aufruf verfügen.

### Anzahl gleichzeitiger Anfragen begrenzen

Bei einem großen Fan-out:

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

Ein `Semaphore` begrenzt die Anzahl gleichzeitig ausgeführter Anfragen. Bei großen Datenmengen wird außerdem Batching eingesetzt.

Praktische Auswahl:

- alle Ergebnisse erforderlich — `coroutineScope` und `async`;
- Liste von Anfragen — `awaitAll()`;
- Teilerfolg — `supervisorScope` und typisierte Fehler;
- großer Fan-out — `Semaphore` oder Batching;
- abhängige Aufrufe — sequenzielle Ausführung.

**Kurz gesagt:** Parallele unabhängige Anfragen werden mit `async` in einem `coroutineScope` gestartet. `awaitAll()` wartet auf eine Liste. Ein gewöhnlicher Scope arbeitet nach dem Fail-Fast-Prinzip, `supervisorScope` ermöglicht Teilergebnisse und ein großer Fan-out wird mit `Semaphore` oder Batching begrenzt.

</details>

<details>
<summary>167. Wofür wurden Fragments entwickelt?</summary>

#### Kotlin

`Fragment` wurde als wiederverwendbarer Bestandteil der Benutzeroberfläche und ihres Verhaltens innerhalb einer `Activity` entwickelt. Die Idee besteht darin, nicht den gesamten Bildschirmablauf in einer großen Activity zu verwalten, sondern die Benutzeroberfläche in kleinere Komponenten mit eigenem Lebenszyklus und Layout aufzuteilen.

Problem ohne Fragment:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Navigation, Liste, Details, Klicks und Zustand
    }
}
```

Eine solche Activity entwickelt sich schnell zu einer God Class.

Fragment als Teil eines Bildschirms:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // UI-Logik der Liste
    }
}
```

Ein Fragment kann eine Liste, Detailansicht, ein Formular, einen mehrstufigen Ablauf oder Tabs kapseln.

Ein historisch wichtiger Anwendungsfall sind unterschiedliche Layouts für Smartphones und Tablets:

```text
Smartphone -> Activity + ListFragment
Tablet     -> Activity + ListFragment + DetailsFragment
```

Eine Activity konnte als Container für mehrere Fragments dienen und sie über den FragmentManager austauschen:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Später vereinfachte die Navigation Component diesen Vorgang.

Ein Fragment besitzt einen eigenen Lebenszyklus und zusätzlich einen separaten View-Lebenszyklus:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Die wichtigste Besonderheit besteht darin, dass das Fragment-Objekt länger leben kann als seine View. Deshalb müssen Binding, Adapter und UI-Subscriptions in `onDestroyView()` bereinigt werden.

Fragments können ein eigenes oder ein gemeinsam genutztes ViewModel besitzen:

```kotlin
private val viewModel: ProfileViewModel by viewModels()
private val shared: SharedViewModel by activityViewModels()
```

Dies ist für Master-Detail-Ansichten, mehrstufige Abläufe oder gemeinsam genutzten UI-Zustand nützlich.

In Compose ist die Rolle von Fragments kleiner: Häufig hostet eine einzige Activity einen `NavHost`, während die Bildschirme Composables sind. Fragments bleiben jedoch für das ältere View-System, hybride Anwendungen, Konfigurationen mit der Navigation Component und schrittweise Migrationen relevant.

**Kurz gesagt:** Fragments wurden für eine modulare Benutzeroberfläche im View-System entwickelt: wiederverwendbare Blöcke, Smartphone- und Tablet-Layouts, Navigation innerhalb einer Activity und Back Stack. Ihre größte Schwierigkeit ist der Lebenszyklus, insbesondere der Unterschied zwischen Fragment- und View-Lebenszyklus.

</details>

<details>
<summary>168. Welche Probleme lösen Fragments?</summary>

#### Kotlin

Fragments lösen Probleme bei der Zerlegung einer View-basierten Benutzeroberfläche innerhalb einer `Activity`: übergroße Activities, Navigation, Back Stack, eigene Lebenszyklen einzelner Bildschirm- oder Containerbestandteile sowie unterschiedliche Layouts für Smartphones und Tablets.

### Große Activities

Ohne Zerlegung wird eine `Activity` schnell zu einer God Class:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Startseite, Details, Einstellungen, Navigation und Zustand
    }
}
```

Fragments unterteilen die Benutzeroberfläche:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

Die `Activity` bleibt der Host, während Zustand und Geschäftslogik im `ViewModel` beziehungsweise in der Domain-Schicht liegen sollten.

### Unterschiedliche Layouts

```text
Smartphone -> Activity + ein Fragment
Tablet     -> Activity + Listen-Fragment + Detail-Fragment
```

Auf dem Smartphone können Liste und Details nacheinander, auf dem Tablet gleichzeitig angezeigt werden.

### Navigation und Back Stack

Fragments ermöglichen den Austausch von Bildschirmbereichen, ohne eine neue Activity zu starten:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Die Navigation Component verwaltet dies kontrollierter über einen Navigationsgraphen.

### UI-Lebenszyklus

Ein Fragment besitzt einen eigenen Lebenszyklus und einen separaten View-Lebenszyklus:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Binding, Adapter und UI-Subscriptions müssen in `onDestroyView()` bereinigt werden, da die View vor dem Fragment-Objekt zerstört werden kann.

### Trennung der Verantwortlichkeiten

Ein Fragment ist für Darstellung, Lebenszyklus und UI-Ereignisse eines bestimmten Bildschirms verantwortlich:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

Ein Fragment darf jedoch keine Geschäftslogik enthalten.

### Was Fragments nicht lösen

- eine ungeeignete Architektur;
- die Vermischung von Benutzeroberfläche und Geschäftslogik;
- fehlerhaftes Ownership des Zustands;
- Speicherlecks nach `onDestroyView()`;
- einen unübersichtlichen Navigationsgraphen;
- übermäßige Verschachtelung der Benutzeroberfläche.

Im View-System ist ein Fragment als Bildschirm oder Container nützlich. In einer vollständig mit Compose erstellten Benutzeroberfläche nimmt seine Bedeutung häufig ab oder entfällt.

**Kurz gesagt:** Fragments ermöglichen die Zerlegung einer View-basierten Benutzeroberfläche, Navigation und Back Stack, Smartphone- und Tablet-Layouts sowie Lebenszyklen einzelner Bildschirmbestandteile. Architektur, Zustandsverwaltung und Speicherlecks beheben sie jedoch nicht automatisch.

</details>

<details>
<summary>169. Welche Besonderheiten besitzen verschachtelte Fragments?</summary>

#### Kotlin

Ein verschachteltes Fragment lebt innerhalb eines anderen Fragments und wird über den `childFragmentManager` verwaltet.

```kotlin
class ParentFragment : Fragment(R.layout.fragment_parent) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        childFragmentManager.beginTransaction()
            .replace(R.id.childContainer, ChildFragment())
            .commit()
    }
}
```

Der `parentFragmentManager` verwaltet Fragments auf der Ebene des übergeordneten Containers und nicht die untergeordneten Fragments des aktuellen Fragments.

### Lebenszyklus und ViewModel-Scope

Ein untergeordnetes Fragment besitzt einen eigenen Lebenszyklus, View-Lebenszyklus, Zustand und ein eigenes ViewModel, kann jedoch nicht länger als sein Parent leben.

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()` — Scope des untergeordneten Fragments;
- `requireParentFragment()` — gemeinsam mit dem Parent genutzter Zustand;
- `activityViewModels()` — Scope der Activity.

### Navigation und Back Stack

Ein untergeordnetes Fragment kann einen eigenen Back Stack oder ein eigenes `NavHostFragment` besitzen. Es muss eindeutig festgelegt sein, wer die Zurück-Navigation verarbeitet:

```kotlin
childFragmentManager.popBackStack()
```

Mehrere verschachtelte Back Stacks verkomplizieren das Verhalten eines Bildschirms schnell. Deshalb sollte verschachtelte Navigation nur bei tatsächlichem Bedarf eingesetzt werden.

### Kommunikation

Direkte Referenzen zwischen Fragments sollten vermieden werden. Besser geeignet sind:

- gemeinsam genutztes ViewModel;
- Fragment Result API;
- Navigationsergebnis.

```kotlin
childFragmentManager.setFragmentResultListener(
    "key",
    viewLifecycleOwner
) { _, bundle ->
    val value = bundle.getString("value")
}
```

### Bereinigung

Parent und Child besitzen getrennte View-Lebenszyklen. View-Referenzen, Adapter und Callbacks werden in `onDestroyView()` bereinigt:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Ein `ViewPager2`-Adapter innerhalb eines Fragments wird mit dem Fragment als Owner erstellt:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment)
```

Verschachtelte Fragments eignen sich für Tabs, einen lokalen mehrstufigen Ablauf oder einen unabhängigen Bereich mit eigenem Lebenszyklus. Für eine einfache Benutzeroberfläche sind eine benutzerdefinierte View oder ein Composable vorzuziehen.

**Kurz gesagt:** Verschachtelte Fragments werden über den `childFragmentManager` verwaltet und besitzen eigene Lebenszyklen, ViewModels und Back Stacks, hängen jedoch vom Parent ab. Die wichtigsten Risiken sind ein falscher FragmentManager, unnötig verschachtelte Navigation und Speicherlecks nach `onDestroyView()`.

</details>

<details>
<summary>170. Welche Callbacks besitzt Application?</summary>

#### Kotlin

`Application` repräsentiert einen Android-Prozess. Jeder Prozess einer Anwendung besitzt eine eigene `Application`-Instanz.

Die wichtigsten Callbacks:

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

Hier wird anwendungsweite Infrastruktur initialisiert: Dependency Injection, Protokollierung, Absturzberichte und SDKs. Der Callback wird auf dem Main Thread ausgeführt und beeinflusst den Kaltstart. Deshalb muss die Initialisierung minimal oder verzögert erfolgen.

Im Manifest:

```xml
<application android:name=".App" />
```

Für Hilt wird die Klasse mit `@HiltAndroidApp` gekennzeichnet.

Eine wichtige Besonderheit: Auf ContentProvidern basierende Initialisierer können vor `Application.onCreate()` gestartet werden.

### Speicher-Callbacks

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

Diese Callbacks signalisieren, dass wiederherstellbarer Speicher freigegeben werden soll, beispielsweise Bild-Caches, Puffer und vorberechnete Daten. Persistenter Zustand darf dabei nicht gelöscht werden.

### Konfiguration

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

Dieser Callback betrifft anwendungsweite Konfigurationsänderungen. Reaktionen der Benutzeroberfläche sollten besser in Activity, Fragment oder Compose behandelt werden, wo ein aktueller Lebenszyklus und ein mit einem Theme versehener Context verfügbar sind.

### `ActivityLifecycleCallbacks`

Die Application kann den Lebenszyklus sämtlicher Activities beobachten:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityStarted(activity: Activity) = Unit
        override fun onActivityResumed(activity: Activity) = Unit
        override fun onActivityStopped(activity: Activity) = Unit
    }
)
```

Anwendungsfälle sind Analysen, Sitzungsverfolgung und Debugging-Werkzeuge. Zur Erkennung von Vorder- und Hintergrundzustand ist häufig `ProcessLifecycleOwner` besser geeignet.

Nach dem Callback darf keine starke Referenz auf eine Activity gespeichert werden.

### `onTerminate()`

`onTerminate()` ist kein zuverlässiger Mechanismus zur Bereinigung im Produktivbetrieb. Android kann den Prozess beenden, ohne diesen Callback aufzurufen. Er darf daher nicht zum Speichern von Daten, Abmelden, Abschließen von Transaktionen oder Freigeben von Ressourcen verwendet werden.

Folgendes sollte vermieden werden:

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

Dies verursacht ein Activity-Speicherleck, globalen veränderlichen Zustand und Datenverlust nach dem Beenden des Prozesses. `Application` darf weder Service Locator noch Sitzungsspeicher sein.

**Kurz gesagt:** `Application.onCreate()` dient einer schnellen anwendungsweiten Initialisierung. `onTrimMemory()` und `onLowMemory()` dienen zum Freigeben wiederherstellbaren Speichers. `onTerminate()` ist nicht garantiert. Der Activity-Lebenszyklus kann über `ActivityLifecycleCallbacks` beobachtet werden, jedoch ohne Activity-Referenzen zu speichern.

</details>

<details>
<summary>171. Welches Singleton erstellt das System beim Start einer Android-Anwendung?</summary>

#### Kotlin

Beim Start eines Android-Prozesses erstellt das System eine Instanz von `Application`. Sie ist ein anwendungsweites Singleton innerhalb des jeweiligen Prozesses.

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
    }
}
```

Im Manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Android erstellt `App` und ruft `onCreate()` auf, bevor `Activity`, `Service`, `BroadcastReceiver` und andere Komponenten in diesem Prozess gestartet werden.

### Singleton nur innerhalb eines Prozesses

`Application` ist weder ein Singleton für das gesamte Gerät noch bei einer Multiprozess-Anwendung zwingend eine einzelne Instanz für die gesamte Anwendung.

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

In diesem Fall gibt es zwei Instanzen:

```text
Hauptprozess  -> App-Instanz Nr. 1
:sync-Prozess -> App-Instanz Nr. 2
```

Deshalb dürfen `Application`, ein Kotlin-`object` und statischer Zustand nicht als gemeinsamer Speicher zwischen Prozessen betrachtet werden.

### Verwendungszwecke von Application

- Konfiguration der Dependency Injection;
- Absturzberichte;
- Protokollierung;
- Konfiguration von Analytics;
- anwendungsweite SDK-Initialisierung;
- `ActivityLifecycleCallbacks`;
- einfache globale Konfiguration.

Für Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

### Application Context

Für langlebige Abhängigkeiten wird `applicationContext` und nicht der Context einer `Activity` verwendet:

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

### Was vermieden werden sollte

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Probleme:

- der Zustand geht nach dem Beenden des Prozesses verloren;
- eine `Activity`-Referenz verursacht ein Speicherleck;
- Geschäftslogik wird zu globalem veränderlichem Zustand;
- Tests werden vom gemeinsam genutzten Zustand abhängig.

Kritischer Zustand muss in einer Datenbank, in DataStore, in Dateien oder im Backend gespeichert werden.

### Application und Kotlin object

Ein Kotlin-`object` wird beim ersten Zugriff verzögert erstellt. `Application` wird vom Android-Framework erstellt und besitzt einen Context auf Prozessebene. Beide existieren nur innerhalb des Prozesses und gehen nach dessen Beendigung verloren.

**Kurz gesagt:** Android erstellt das prozessweite Singleton `Application`. Es eignet sich für eine anwendungsweite Initialisierung, jedoch nicht zum Speichern von Sitzungs- oder Geschäftszustand und darf keine Referenzen auf die Benutzeroberfläche halten.

</details>

<details>
<summary>172. Wie funktioniert der Back Stack?</summary>

#### Kotlin

Der `BackStack` ist ein Stapel des Navigationsverlaufs. Er bestimmt, zu welchem Bildschirm der Benutzer nach einer Zurück-Aktion gelangt. Dabei gilt das LIFO-Prinzip: Der zuletzt hinzugefügte Bildschirm wird zuerst entfernt.

```text
Startseite -> Details -> Einstellungen
```

Zurück:

```text
Einstellungen entfernt -> Details sichtbar
Details entfernt        -> Startseite sichtbar
```

### Activity Back Stack

Eine neue Activity wird dem Back Stack eines Tasks hinzugefügt:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Eine Zurück-Aktion beendet die aktuelle Activity und zeigt die vorherige wieder an.

### Fragment Back Stack

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Wird `addToBackStack` nicht aufgerufen, stellt eine Zurück-Aktion das vorherige Fragment nicht wieder her.

### Navigation Component

Der Back Stack besteht aus Navigationszielen:

```kotlin
navController.navigate("details/$id")
navController.popBackStack()
```

`popBackStack()` entfernt das aktuelle Navigationsziel.

### `popUpTo` und `inclusive`

`popUpTo` entfernt einen Teil des Stacks:

```kotlin
navController.navigate("home") {
    popUpTo("login") { inclusive = true }
}
```

Dies ist nach Anmeldung oder Onboarding nützlich, damit der Benutzer nicht zum Anmeldebildschirm zurückkehren kann.

```text
inclusive = false -> das popUpTo-Ziel bleibt erhalten
inclusive = true  -> das popUpTo-Ziel wird ebenfalls entfernt
```

### Compose Navigation

Das Prinzip ist identisch:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen() }
    composable("details/{id}") { DetailsScreen() }
}
```

Der Back Stack wird vom `NavController` verwaltet.

### `launchSingleTop` und Bottom Navigation

Um doppelte Navigationsziele zu vermeiden:

```kotlin
navController.navigate("home") {
    launchSingleTop = true
}
```

Bei einer Bottom Navigation wird der Zustand der Tabs häufig gespeichert:

```kotlin
navController.navigate(route) {
    popUpTo(navController.graph.startDestinationId) {
        saveState = true
    }
    launchSingleTop = true
    restoreState = true
}
```

Typische Fehler:

- den Stack nach Authentifizierung oder Onboarding nicht bereinigen;
- Navigationsziele duplizieren;
- manuelle Fragment-Transaktionen und `NavController` vermischen;
- den Task Stack einer Activity mit dem Back Stack des `NavController` verwechseln;
- verschachtelte Graphen oder modale Benutzeroberflächen nicht berücksichtigen.

Praktische Regel: Der Back Stack muss dem erwarteten Verlauf des Benutzers entsprechen. Modale Benutzeroberflächen werden zuerst geschlossen, unumkehrbare Abläufe bereinigen den Stack und Tabs bewahren ihren Zustand.

**Kurz gesagt:** Der Back Stack ist ein LIFO-Verlauf von Bildschirmen beziehungsweise Navigationszielen. Android besitzt einen Activity Task Stack, einen Fragment Back Stack und einen NavController Back Stack. Gesteuert werden sie mit `navigate`, `popBackStack`, `popUpTo`, `inclusive`, `launchSingleTop`, `saveState` und `restoreState`.

</details>

<details>
<summary>173. In welchen Fällen kann ein ANR (Application Not Responding) auftreten?</summary>

#### Kotlin

Ein ANR tritt auf, wenn der Anwendungsprozess nicht rechtzeitig auf ein wichtiges Systemereignis reagiert. Meistens ist der Main Thread blockiert, überlastet oder wartet auf einen anderen Thread beziehungsweise Prozess.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // blockiert Eingabe und Darstellung
}
```

Der Main Thread verarbeitet Eingaben, Lebenszyklusereignisse, einen Teil der Binder-Callbacks und die Darstellung. Sowohl eine einzelne lange Operation als auch eine große Warteschlange kurzer Operationen kann einen ANR verursachen.

### Typische Ursachen

- synchrone Netzwerk- oder Festplattenzugriffe beziehungsweise Datenbankabfragen auf dem Main Thread;
- aufwendiges Parsen, Dekodieren von Bitmaps, Sortieren oder kryptografische Berechnungen;
- `runBlocking`, `Thread.sleep()`, `Future.get()` oder `join()`;
- aufwendige Lebenszyklus-, View- oder Compose-Callbacks;
- lang dauernde Arbeit in einem `BroadcastReceiver` oder `Service`;
- Sperren, Deadlocks oder synchrone Binder-Aufrufe;
- aufwendiger Start in `Application`, `ContentProvider`, Dependency Injection oder der ersten Activity.

`suspend` verschiebt blockierenden Code nicht automatisch vom Main Thread.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

Ein- und Ausgabe gehört auf `Dispatchers.IO`, rechenintensive Arbeit auf `Dispatchers.Default`.

Ein `Service` erstellt nicht automatisch einen Worker Thread. Für lang dauernde, garantiert auszuführende Arbeit wird `WorkManager` verwendet. Bei `BroadcastReceiver.goAsync()` muss zwingend `finish()` aufgerufen werden.

### Jank und ANR

Ein langsamer Frame ist Jank und nicht zwingend ein ANR. Eine lange Folge aufwendiger Layout-, Zeichen- oder Recomposition-Operationen kann den Main Thread jedoch lange genug blockieren.

### Diagnose

Die Untersuchung beginnt beim Stack des Main Threads und bei den Threads, auf die er wartet:

- Play Console Android Vitals und ANR-Traces;
- Perfetto beziehungsweise System Trace;
- Android Studio CPU Profiler;
- Logcat und eigene Zeitmessungen;
- StrictMode in einem Debug-Build.

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

StrictMode hilft dabei, Ein- und Ausgabe auf dem Main Thread zu finden, ersetzt jedoch keine Traces.

### Vorbeugung

- den Main Thread nicht blockieren;
- während Ein- und Ausgabe keine Sperre halten;
- Zeitlimits für Netzwerk und IPC festlegen;
- nicht kritische Initialisierung beim Start verzögern;
- lang dauernde CPU-Arbeit aufteilen;
- die Häufigkeit von UI-Ereignissen kontrollieren;
- Start, Scrollen und Massenaktualisierungen profilieren.

**Kurz gesagt:** ANRs werden durch blockierende Ein- und Ausgabe, rechenintensive Arbeit, Sperren, synchrones Warten beziehungsweise IPC, lang dauernde Callbacks oder einen aufwendigen Start auf dem Main Thread verursacht. Die Diagnose beginnt mit dem Stack des Main Threads und einem System Trace.

</details>

<details>
<summary>174. Was darf auf dem Main Thread ausgeführt werden?</summary>

#### Kotlin

Der `Main Thread` ist unter Android für die Benutzeroberfläche verantwortlich: Eingabeereignisse, Lebenszyklus-Callbacks, Darstellung sowie Aktualisierungen von Views und Compose. Auf ihm dürfen nur schnelle Operationen ausgeführt werden, die die Darstellung nicht blockieren und kein ANR-Risiko verursachen.

Erlaubt sind:

- Aktualisierungen der Benutzeroberfläche;
- Arbeit mit der View-Hierarchie;
- Darstellung einer Compose-Benutzeroberfläche;
- Ausführung von Lebenszyklus-Callbacks;
- Verarbeitung von Click-Listenern;
- Navigationsaufrufe;
- Anzeige von Dialogen, Snackbars und Toasts;
- kurze, einfache Logik.

```kotlin
button.setOnClickListener {
    viewModel.onLoginClick()
}
```

Das View-System ist nicht threadsicher. Daher müssen UI-Aktualisierungen auf dem Main Thread erfolgen:

```kotlin
textView.text = "Loaded"
progressBar.isVisible = false
```

In Compose muss ein Composable leichtgewichtig sein:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
ProfileScreen(state)
```

Der Main Thread darf nicht durch Folgendes blockiert werden:

- Netzwerkanfragen;
- Datei-Ein- und -Ausgabe;
- große Datenbankabfragen;
- Dekodierung von Bitmaps;
- umfangreiches JSON-Parsing;
- aufwendiges Sortieren oder Filtern;
- `Thread.sleep`;
- `runBlocking`;
- lang dauernde synchronisierte Blöcke.

Für 60 Bilder pro Sekunde stehen dem Main Thread ungefähr 16 ms pro Frame zur Verfügung. Ist er mit lang dauernder Arbeit beschäftigt, entsteht Jank; bei längerer Blockierung kann ein ANR auftreten.

Hintergrundarbeit:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

Regel:

```text
Benutzeroberfläche und Orchestrierung -> Main
blockierende Ein- und Ausgabe          -> Dispatchers.IO
rechenintensive Arbeit                 -> Dispatchers.Default
```

Eine kurze Validierung auf dem Main Thread ist unproblematisch:

```kotlin
if (email.isBlank()) {
    showEmailError()
    return
}
```

Zum Auffinden unbeabsichtigter Ein- und Ausgabe auf dem Main Thread kann in einem Debug-Build StrictMode aktiviert werden:

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

**Kurz gesagt:** Der Main Thread ist für Benutzeroberfläche, Lebenszyklus, Navigation und kurze Orchestrierungslogik vorgesehen. Alles mit unvorhersehbar langer Ausführungszeit — Netzwerk, Festplatte, Datenbank, Parsing, Bitmaps und rechenintensive Arbeit — wird auf `IO` oder `Default` ausgelagert.

</details>

<details>
<summary>175. Welche grundlegenden Android-Komponenten gibt es?</summary>

#### Kotlin

Die grundlegenden Android-Systemkomponenten sind `Activity`, `Service`, `BroadcastReceiver` und `ContentProvider`. Fragment, ViewModel und WorkManager sind wichtige Jetpack-Abstraktionen, jedoch keine Systemkomponenten.

### Activity

Ein Einstiegspunkt der Benutzeroberfläche mit eigenem Fenster und Lebenszyklus:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Eine Activity empfängt einen `Intent` und kann eine Compose-Benutzeroberfläche oder Fragments hosten.

### Service

Eine Komponente ohne eigene Benutzeroberfläche:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Service-Callbacks werden standardmäßig auf dem Main Thread ausgeführt. Für lang andauernde, für den Benutzer sichtbare Arbeit dient ein Foreground Service. Für aufschiebbare persistente Arbeit ist normalerweise WorkManager besser geeignet.

### BroadcastReceiver

Empfängt Broadcasts des Systems oder der Anwendung:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // Ereignis verarbeiten
    }
}
```

`onReceive()` muss schnell beendet werden. Lang dauernde Arbeit wird an WorkManager oder eine andere geeignete Komponente delegiert.

### ContentProvider

Stellt strukturierte Daten über eine URI bereit:

```text
content://com.example.provider/users/1
```

Ein ContentProvider dient der kontrollierten prozessübergreifenden gemeinsamen Nutzung von Daten. Systembeispiele sind Provider für Kontakte und Medien.

### Manifest und Intent

Systemkomponenten werden in `AndroidManifest.xml` deklariert:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

Das Manifest definiert außerdem Berechtigungen, Intent-Filter und das Verhalten von `android:exported`.

Ein `Intent` startet eine Activity oder einen Service beziehungsweise übermittelt einen Broadcast:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

### Jetpack-Abstraktionen

- Fragment — Bestandteil einer View-basierten Benutzeroberfläche innerhalb einer Activity;
- ViewModel — Besitzer des UI-Zustands, der Konfigurationsänderungen übersteht;
- WorkManager — Scheduler für persistente, aufschiebbare Arbeit.

Praktische Auswahl:

- Einstiegspunkt der Benutzeroberfläche — `Activity`;
- lang andauernde, für den Benutzer sichtbare Arbeit — Foreground-`Service`;
- System- oder Anwendungsereignis — `BroadcastReceiver`;
- prozessübergreifende Daten-API — `ContentProvider`;
- aufschiebbare persistente Arbeit — `WorkManager`.

**Kurz gesagt:** Die grundlegenden Android-Systemkomponenten sind `Activity`, `Service`, `BroadcastReceiver` und `ContentProvider`. Sie werden im Manifest deklariert und kommunizieren über Intent, Binder oder URI. Fragment, ViewModel und WorkManager gehören zur Jetpack-Ebene.

</details>

<details>
<summary>176. Was ist ein Service?</summary>

#### Kotlin

Ein `Service` ist eine Android-Komponente ohne Benutzeroberfläche. Wichtig: Ein Service erstellt nicht selbstständig einen Hintergrundthread. Seine Callbacks werden standardmäßig auf dem Main Thread des Anwendungsprozesses ausgeführt.

Ein Service übersteht nicht garantiert das Beenden des Prozesses, einen erzwungenen Stopp oder einen Neustart des Geräts.

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

Jeder Aufruf von `startService()` löst `onStartCommand()` aus. Blockierende Arbeit muss vom Main Thread ausgelagert werden. `onDestroy()` bietet lediglich eine bestmögliche Bereinigung und wird beim Beenden des Prozesses möglicherweise nicht aufgerufen.

### Bound Service

Ein gebundener Service stellt über `IBinder` eine API bereit:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService = this@PlayerService
    }

    override fun onBind(intent: Intent): IBinder = binder
}
```

Ein lokaler Binder funktioniert innerhalb desselben Prozesses. Für prozessübergreifende IPC werden Messenger oder AIDL verwendet.

Ein ausschließlich gebundener Service lebt, solange Clients verbunden sind. Wurde der Service zusätzlich gestartet, beendet `unbindService()` seinen Started-Lebenszyklus nicht.

### Foreground Service

Ein Foreground Service wird für lang andauernde, für den Benutzer sichtbare Arbeit benötigt, beispielsweise Medienwiedergabe, Navigation, aktive Standortverfolgung oder Dateiübertragung.

```kotlin
startForeground(
    NOTIFICATION_ID,
    createNotification()
)
```

Nach `startForegroundService()` muss zeitnah `startForeground()` aufgerufen werden. Moderne Android-Versionen beschränken Starts im Hintergrund und verlangen eine Benachrichtigung, einen Service-Typ sowie entsprechende Berechtigungen.

### Lebenszyklus

```text
onCreate()       -> Instanz erstellen
onStartCommand() -> jeder Start
onBind()         -> Client-Verbindung
onDestroy()      -> Bereinigung
```

Flags für einen Neustart:

- `START_NOT_STICKY` — nicht wiederherstellen;
- `START_STICKY` — ohne Garantie des Intents wiederherstellen;
- `START_REDELIVER_INTENT` — Intent erneut zustellen.

Eine Neustartstrategie macht einen Service nicht zu einer dauerhaften Warteschlange. Die Operation muss idempotent sein.

Praktische Auswahl:

```text
aktive, sichtbare Arbeit       -> Foreground Service
zuverlässige aufschiebbare Arbeit -> WorkManager
Auslösung zu genauer Zeit      -> AlarmManager
Medienwiedergabe               -> Media3 + FGS
bildschirmgebundene Arbeit     -> ViewModel-Scope
```

**Kurz gesagt:** Ein Service ist eine Lebenszykluskomponente ohne Benutzeroberfläche und kein eigener Thread. Ein Started Service führt Befehle aus, ein Bound Service stellt eine Binder-API bereit und ein Foreground Service dient für fortlaufende, für den Benutzer sichtbare Arbeit. Für zuverlässige aufschiebbare Arbeit wird WorkManager verwendet.

</details>

<details>
<summary>177. Was ist ein BroadcastReceiver?</summary>

#### Kotlin

Ein `BroadcastReceiver` ist eine Android-Komponente zum Empfangen von Broadcast-`Intent`s des Systems, von Anwendungskomponenten oder anderen Anwendungen.

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            scheduleSync(context)
        }
    }
}
```

Ein Receiver speichert keinen Zustand und ist nicht für lang andauernde Arbeit vorgesehen. Nach Abschluss von `onReceive()` kann der Prozess beendet werden.

### Im Manifest deklarierter Receiver

```xml
<receiver
    android:name=".BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

Für `BOOT_COMPLETED` wird die Berechtigung `RECEIVE_BOOT_COMPLETED` benötigt.

Ein im Manifest deklarierter Receiver kann ohne Benutzeroberfläche arbeiten. Moderne Android-Versionen beschränken jedoch zahlreiche implizite Broadcasts. Deshalb müssen der konkrete Action-Typ und die jeweilige API geprüft werden.

`android:exported` ist für die Sicherheit entscheidend. Soll ein Receiver keine Intents anderer Anwendungen empfangen, muss `exported=false` gesetzt werden. Ist er exportiert, müssen Intent-Daten und Berechtigungen validiert werden.

### Dynamischer Receiver

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

Registrierung und Abmeldung müssen symmetrisch sein. Das Lebenszykluspaar richtet sich nach dem Bedarf: für einen sichtbaren Bildschirm `onStart()` und `onStop()`, für die gesamte Lebensdauer einer Activity `onCreate()` und `onDestroy()`.

Auf modernen API-Versionen muss für einen dynamischen Receiver häufig ausdrücklich `RECEIVER_EXPORTED` oder `RECEIVER_NOT_EXPORTED` angegeben werden.

### Lang andauernde Arbeit

`onReceive()` wird normalerweise auf dem Main Thread ausgeführt und muss schnell beendet werden. Für aufschiebbare persistente Arbeit wird WorkManager verwendet:

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

`goAsync()` ermöglicht eine kurze Fortsetzung der Arbeit nach `onReceive()`, erfordert jedoch den Aufruf von `PendingResult.finish()` und ersetzt WorkManager nicht bei lang dauernden Aufgaben.

### Interne Kommunikation

`LocalBroadcastManager` ist veraltet. Innerhalb eines Prozesses sollten stattdessen Flow beziehungsweise StateFlow, Callbacks, Repository-Zustand oder Navigationsergebnisse verwendet werden.

Ein BroadcastReceiver eignet sich für Systemereignisse, Alarme, Änderungen beim Start oder bei Paketen sowie klar definierte Kommunikation zwischen Anwendungen oder Komponenten.

**Kurz gesagt:** Ein BroadcastReceiver empfängt einen Broadcast-Intent und muss `onReceive()` schnell beenden. Bei einem im Manifest deklarierten Receiver erfordern `exported` und Berechtigungen besondere Aufmerksamkeit, ein dynamischer Receiver wird an den Lebenszyklus gebunden und lang andauernde Arbeit an WorkManager delegiert.

</details>

<details>
<summary>178. Was ist ein Context und welche Arten gibt es?</summary>

#### Kotlin

Ein `Context` ist ein Zugangspunkt zur Android-Umgebung: Ressourcen, Systemdienste, Paketinformationen, Dateien, Einstellungen und das Starten von Komponenten. Ein konkreter Context bestimmt außerdem das Theme und die Lebenszyklusgrenze.

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

Er lebt so lange wie der Prozess und besitzt nicht das Theme einer bestimmten Activity. Er eignet sich für langlebige Abhängigkeiten wie Room, DataStore, Repositories und SDKs, die keinen UI-Context benötigen.

### Activity Context

Er besitzt Theme, Fenster und Lebenszyklus einer bestimmten Activity. Er wird für Dialoge, mit einem Theme versehene Views und UI-Operationen benötigt. Er darf nicht in einem Singleton oder einem langlebigen Objekt gespeichert werden.

### Fragment Context

Ein `Fragment` ist selbst kein `Context`. Es bezieht den Context von seinem Host:

```kotlin
val context = requireContext()
val activityContext = requireActivity()
```

Vor dem Anhängen kann der Context `null` sein. Nach dem Trennen löst `requireContext()` eine Exception aus.

### Service Context

Ein `Service` ist ein `Context`, der an den Lebenszyklus des Services gebunden ist. Er eignet sich für Service-Operationen, besitzt jedoch nicht die Fenster- und Theme-Semantik einer Activity.

### ContextWrapper

Ein `ContextWrapper` delegiert an einen Basis-Context und kann dessen Verhalten verändern:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

`ContextThemeWrapper` ist nützlich, um Benutzeroberflächen mit einem bestimmten Theme zu erzeugen.

### Compose

```kotlin
val context = LocalContext.current
```

Dies ist der Context der aktuellen Composition-Umgebung. Er kann für Ressourcen und UI-Aktionen verwendet werden, sollte jedoch nicht ohne Lebenszyklus-Ownership gespeichert werden.

### Speicherlecks

Ungünstig:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

Das Singleton hält die Activity nach einer Neuerstellung weiterhin im Speicher.

Besser für eine langlebige Abhängigkeit:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

Dabei wird `context.applicationContext` übergeben.

Praktische Auswahl:

```text
Application Context -> Datenbank, DataStore und langlebige Abhängigkeiten
Activity Context    -> Benutzeroberfläche, Dialoge und mit Theme versehene Views
Fragment Context    -> Context des Hosts, solange das Fragment angehängt ist
Service Context     -> Service-Operationen
LocalContext        -> Ressourcen und UI-Aktionen in Compose
```

**Kurz gesagt:** Ein `Context` bietet Zugriff auf die Android-Umgebung. Langlebige Abhängigkeiten verwenden den Application Context, die Benutzeroberfläche einen Activity- beziehungsweise Theme-Context. Ein Fragment ist kein Context und ein Activity Context darf nicht in einem Singleton gespeichert werden.

</details>

<details>
<summary>179. Was ist ein Bundle?</summary>

#### Kotlin

Ein `Bundle` ist ein Android-Schlüssel-Wert-Container zur Übertragung kleiner Datenmengen zwischen Komponenten oder zum Speichern eines einfachen UI-Zustands.

```kotlin
val bundle = Bundle().apply {
    putString("user_id", userId)
    putInt("tab", 2)
}

val userId = bundle.getString("user_id")
```

Es wird mit `Intent`, Fragment-Argumenten, `onSaveInstanceState`, Navigation und der Activity Result API verwendet.

### Intent-Extras

```kotlin
val intent = Intent(this, DetailsActivity::class.java).apply {
    putExtra("user_id", userId)
}
startActivity(intent)
```

```kotlin
val userId = intent.getStringExtra("user_id")
```

### Fragment-Argumente

```kotlin
val fragment = DetailsFragment().apply {
    arguments = bundleOf("user_id" to userId)
}
```

```kotlin
val userId = requireArguments().getString("user_id")
```

### Gespeicherter Instanzzustand

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

Dies eignet sich für einen kleinen UI-Zustand nach einer Konfigurationsänderung oder der Neuerstellung des Prozesses.

### Typen

Ein Bundle unterstützt:

- primitive Typen;
- `String`;
- Arrays;
- `Parcelable`;
- `Serializable`;
- `Bundle`;
- `ArrayList` bestimmter Typen.

Für benutzerdefinierte Android-Daten ist `Parcelable` vorzuziehen:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Bei der Navigation ist es häufig besser, nur die `id` zu übergeben und die Daten aus einem Repository zu laden.

### Einschränkungen

Ein Bundle ist nicht für große Datenmengen vorgesehen. Andernfalls kann eine `TransactionTooLargeException` auftreten.

Ungünstig:

```kotlin
bundle.putParcelableArrayList("huge_list", hugeList)
```

Besser ist die Übergabe einer ID oder eines Schlüssels zu einer Datenbank beziehungsweise einem Cache.

### Bundle und andere Zustandsspeicher

```text
Bundle             -> kleiner Navigations- oder UI-Zustand
SavedStateHandle   -> Zustand im ViewModel nach einer Neuerstellung
ViewModel          -> Bildschirm- und Geschäftszustand während des Lebenszyklus
Database/DataStore -> persistente Daten
```

Schlüssel in einem Bundle sind Strings, weshalb Tippfehler leicht möglich sind. Für die Navigation sollten, sofern verfügbar, Safe Args oder typisierte Routenargumente verwendet werden.

**Kurz gesagt:** Ein Bundle ist ein Schlüssel-Wert-Container für kleine Daten wie IDs, Flags sowie einfachen UI- oder Navigationszustand. Große Listen, Bitmaps oder umfangreiche Nutzdaten gehören nicht hinein; stattdessen sollte eine ID übergeben und die Daten aus Repository oder Cache wiederhergestellt werden.

</details>

<details>
<summary>180. Kann ein Bundle in ein anderes Bundle eingebettet werden?</summary>

#### Kotlin

Ja, ein `Bundle` kann mit `putBundle()` in ein anderes `Bundle` eingebettet und mit `getBundle()` ausgelesen werden.

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

Auslesen:

```kotlin
val address = userBundle.getBundle("address")
val city = address?.getString("city")
```

Dies ist für kleine, logisch gruppierte Daten nützlich:

```text
Benutzer
 ├─ ID
 ├─ Name
 └─ Adresse
     ├─ Stadt
     └─ Straße
```

Beispiel für Fragment-Argumente:

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

Für gespeicherten Zustand:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putBundle("filters", bundleOf("query" to query))
    super.onSaveInstanceState(outState)
}
```

Einschränkungen:

- ein Bundle ist nicht für große Datenmengen vorgesehen;
- tiefe Verschachtelung verschlechtert die Lesbarkeit;
- bei String-Schlüsseln entstehen leicht Tippfehler;
- es kann eine `TransactionTooLargeException` auftreten.

Ungünstig:

```kotlin
bundle.putBundle("huge_data", hugeNestedBundle)
```

Besitzt die Struktur ein klares Modell, ist `Parcelable` besser geeignet:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Bei großen Datenmengen sollte nur eine `id` übergeben und die Daten aus Repository, Cache oder Datenbank geladen werden.

**Kurz gesagt:** Ein `Bundle` kann in ein anderes `Bundle` eingebettet werden, jedoch nur für kleine, einfache und logisch gruppierte Argumente oder gespeicherten Zustand. Für komplexe Modelle dient `Parcelable`, für große Datenmengen eine `id` in Verbindung mit einem Repository.

</details>

<details>
<summary>181. Was sind Permissions unter Android?</summary>

#### Kotlin

Permissions sind ein Android-Mechanismus, der den Zugriff einer Anwendung auf Ressourcen außerhalb ihrer Sandbox beschränkt, beispielsweise Kamera, Mikrofon, Standort, Kontakte, Benachrichtigungen sowie Speicher und Medien.

Die wichtigsten Typen:

```text
normal    -> werden automatisch gewährt
dangerous -> Manifest und Laufzeitanfrage erforderlich
special   -> eigener Ablauf über die Einstellungen
signature -> nur Anwendungen mit dem entsprechenden Zertifikat
```

Eine Dangerous Permission muss im Manifest deklariert und zur Laufzeit angefordert werden:

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

Vor dem Zugriff auf eine geschützte API muss stets der aktuelle Berechtigungsstatus geprüft werden:

```kotlin
val granted = ContextCompat.checkSelfPermission(
    context,
    Manifest.permission.CAMERA
) == PackageManager.PERMISSION_GRANTED
```

Die Berechtigung darf nicht dauerhaft als Boolean zwischengespeichert werden: Der Benutzer kann sie in den Einstellungen widerrufen, das System kann sie automatisch zurücksetzen und Berechtigungen für Kamera, Mikrofon oder Standort können nur einmalig gelten.

`shouldShowRequestPermissionRationale()` hilft bei der Entscheidung, ob vor einer erneuten Anfrage eine Erklärung angezeigt werden soll. `false` bedeutet nicht immer eine dauerhafte Ablehnung, weshalb die Anwendung ihre eigene Anfragehistorie berücksichtigen muss.

Praktische Regeln:

- die Berechtigung im Moment der entsprechenden Aktion und nicht beim Start der Anwendung anfordern;
- nur den minimal erforderlichen Zugriff verlangen;
- Ablehnung, Widerruf und einmalige Berechtigungen unterstützen;
- für Dateien und Medien Photo Picker, SAF, MediaStore oder anwendungsspezifischen Speicher verwenden;
- bei speziellen Berechtigungen den Benutzer zu den Einstellungen führen und die Fähigkeit nach der Rückkehr erneut prüfen.

**Kurz gesagt:** Permissions schützen sensible Ressourcen. Dangerous Permissions werden im Manifest deklariert und zur Laufzeit über die Activity Result API angefordert. Eine Berechtigung kann sich jederzeit ändern und muss deshalb vor jeder Verwendung geprüft werden.

</details>

<details>
<summary>182. Was ist setContentView?</summary>

#### Kotlin

`setContentView()` ist eine Activity-API, die die Wurzel der Benutzeroberfläche eines Bildschirms festlegt. Im View-System erzeugt sie aus einem XML-Layout eine View-Hierarchie oder nimmt eine fertige `View` entgegen und fügt sie dem Inhaltsbereich der Activity hinzu.

Grundlegendes Beispiel:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Vereinfacht geschieht intern Folgendes:

```text
XML -> Inflation -> View-Hierarchie -> Inhaltswurzel der Activity
```

Mit ViewBinding wird häufiger folgende Variante verwendet:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

Nach `setContentView()` können Views mit `findViewById` gesucht werden, da das Layout bereits angehängt ist:

```kotlin
val title = findViewById<TextView>(R.id.title)
```

In Compose wird statt `setContentView()` die Funktion `setContent { ... }` verwendet:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { MainScreen() }
    }
}
```

In einem Fragment wird `setContentView()` nicht aufgerufen. Das Layout wird über `Fragment(R.layout...)`, `onCreateView()` oder eine ComposeView erstellt.

Typische Fehler:

- `findViewById` vor `setContentView` aufrufen;
- `setContentView` ohne Notwendigkeit mehrfach aufrufen;
- View-Referenzen einer Activity in einem Singleton speichern;
- XML und Compose ohne klare Verantwortlichkeitsgrenze vermischen.

**Kurz gesagt:** `setContentView()` legt die Wurzel der Benutzeroberfläche einer Activity fest. Für XML sollte ViewBinding mit `setContentView(binding.root)`, für Compose `setContent { ... }` und für Fragments deren eigener View-Erstellungslebenszyklus verwendet werden.

</details>

<details>
<summary>183. Welche Methoden besitzt eine View?</summary>

#### Kotlin

`View` ist die Basisklasse der Android-Benutzeroberfläche. Die zentrale Verarbeitungskette lautet: `measure -> layout -> draw`.

Die wichtigsten Callbacks:

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

`onMeasure()` bestimmt die Größe einer View anhand von `MeasureSpec`: `EXACTLY`, `AT_MOST` und `UNSPECIFIED`. `onLayout()` positioniert untergeordnete Views und ist normalerweise für eine `ViewGroup` von Bedeutung. `onDraw()` zeichnet den Inhalt auf einen `Canvas`.

Zur Aktualisierung der Benutzeroberfläche:

```text
nur Darstellung geändert -> invalidate()
Größe oder Layout geändert -> requestLayout()
```

Eingaben werden mit `onTouchEvent()` verarbeitet:

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

`performClick()` ist für Click-Listener und Barrierefreiheit wichtig.

Lebenszyklus einer View:

- `onAttachedToWindow()` — die View wurde an ein Fenster angehängt;
- `onDetachedFromWindow()` — Animationen, Listener und Callbacks müssen beendet werden;
- `onSaveInstanceState()` und `onRestoreInstanceState()` — Speichern des eigenen Zustands.

Nützliche Methoden und Zustände:

- `VISIBLE`, `INVISIBLE`, `GONE`;
- `requestFocus()` und `clearFocus()`;
- `post { }` — Code in der UI-Warteschlange ausführen;
- `setOnClickListener()` und `setOnLongClickListener()`.

In `onDraw()` dürfen keine Ein- und Ausgabe, keine Bitmap-Dekodierung und keine unnötigen Allokationen stattfinden, da die Methode häufig aufgerufen werden kann.

**Kurz gesagt:** Die wichtigsten Methoden einer `View` sind `onMeasure()`, `onLayout()`, `onDraw()`, `invalidate()`, `requestLayout()`, `onTouchEvent()`, `performClick()` und Lebenszyklus-Callbacks. Sie sind für Größe, Position, Darstellung, Eingaben und Ressourcen verantwortlich.

</details>

<details>
<summary>184. Was ist eine ViewGroup?</summary>

#### Kotlin

Eine `ViewGroup` ist eine `View`, die untergeordnete Views enthält und bestimmt, wie sie gemessen, positioniert und gezeichnet werden sowie wie Touch-Ereignisse an sie weitergegeben werden.

```text
View      -> einzelner UI-Knoten
ViewGroup -> Container + untergeordnete Views + Layoutstrategie
```

Beispiele sind `LinearLayout`, `FrameLayout`, `ConstraintLayout` und `RecyclerView`.

Die zentrale Verarbeitungskette:

```text
onMeasure() -> onLayout() -> dispatchDraw()
```

`onMeasure()` misst die untergeordneten Views und bestimmt die eigene Größe:

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

`onLayout()` legt die endgültigen Grenzen der untergeordneten Views fest:

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

`LayoutParams` beschreiben Regeln für eine untergeordnete View, beispielsweise `weight` in einem `LinearLayout`, Constraints in einem `ConstraintLayout` oder `gravity` in einem `FrameLayout`. Eine benutzerdefinierte `ViewGroup` muss Parameter, Ränder, `GONE`, Padding und `MeasureSpec` korrekt unterstützen.

Ablauf von Touch-Ereignissen:

```text
dispatchTouchEvent()
 -> onInterceptTouchEvent()
 -> child.dispatchTouchEvent()
 -> onTouchEvent()
```

`onInterceptTouchEvent()` ermöglicht einem Container, eine Geste von einer untergeordneten View zu übernehmen, beispielsweise zum Scrollen. Ein fehlerhaftes Abfangen beeinträchtigt Klicks und verschachteltes Scrollen.

Zur Aktualisierung:

```text
invalidate()    -> neu zeichnen
requestLayout() -> Messen und Layout erneut ausführen
```

Eine benutzerdefinierte `ViewGroup` sollte nur erstellt werden, wenn vorhandene Container oder Compose die Anforderungen an Layout, Touch-Verarbeitung oder Performance nicht erfüllen.

**Kurz gesagt:** Eine `ViewGroup` ist ein Container für Views, der Messung, Positionierung, Darstellung und Weiterleitung von Touch-Ereignissen seiner untergeordneten Views verwaltet. Die wichtigsten Methoden sind `onMeasure()`, `onLayout()`, `dispatchDraw()` und `onInterceptTouchEvent()`.

</details>

<details>
<summary>185. Wofür wird die Methode onStart() einer Activity benötigt?</summary>

#### Kotlin

`onStart()` wird aufgerufen, wenn eine `Activity` für den Benutzer sichtbar wird. Die Benutzeroberfläche wurde bereits erstellt, die Activity befindet sich jedoch noch nicht zwingend im Vordergrund und nimmt möglicherweise noch keine Eingaben entgegen.

Lebenszyklus:

```text
erster Start: onCreate -> onStart -> onResume
Hintergrund:  onPause -> onStop
Rückkehr:     onRestart -> onStart -> onResume
```

Unterschied zu `onResume()`:

```text
onStart()  -> Activity ist sichtbar
onResume() -> Activity ist interaktiv und im Vordergrund
```

In `onStart()` sollten Ressourcen gestartet werden, die nur während der Sichtbarkeit benötigt werden:

- Registrierung leichtgewichtiger Listener oder Receiver;
- Analysen zur Sichtbarkeit;
- Subscriptions auf den UI-Zustand;
- Ressourcen, die ausschließlich bei sichtbarem Bildschirm erforderlich sind.

Alles, was in `onStart()` gestartet wird, muss in `onStop()` symmetrisch beendet werden:

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

Für Flow sollten lebenszyklusbewusste APIs verwendet werden:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`repeatOnLifecycle(STARTED)` startet das Sammeln, wenn die Activity sichtbar ist, und bricht es ab, sobald der Lebenszyklus unter `STARTED` fällt.

Folgendes gehört nicht in `onStart()`:

- aufwendige blockierende Ein- und Ausgabe;
- lang dauerndes Parsen oder `Thread.sleep()`;
- einmalige Initialisierung, die in `onCreate()` gehört;
- Hintergrundarbeit, die die Activity überleben muss.

Wird eine Aktualisierung in `onStart()` ausgelöst, müssen ViewModel oder Repository Aktualität und Deduplizierung kontrollieren, da `onStart()` bei jeder Rückkehr zum Bildschirm aufgerufen wird.

**Kurz gesagt:** `onStart()` bedeutet, dass eine Activity sichtbar geworden ist. Hier werden leichtgewichtige, nur während der Sichtbarkeit benötigte Ressourcen und Subscriptions gestartet und in `onStop()` wieder bereinigt. Für Flow wird `repeatOnLifecycle(STARTED)` verwendet.

</details>

<details>
<summary>186. Wie kann onStart() aufgerufen werden, ohne dass onResume() folgt?</summary>

#### Kotlin

Dies ist möglich, wenn eine `Activity` sichtbar geworden ist, jedoch nicht den Fokus im Vordergrund erhalten hat oder ihr Lebenszyklus zwischen `STARTED` und `RESUMED` unterbrochen wurde.

Unterschied zwischen den Zuständen:

```text
onStart()  -> Activity ist sichtbar
onResume() -> Activity ist im Vordergrund und interaktiv
```

Typische Szenarien:

1. **Eine andere Activity oder ein Overlay wurde darüber geöffnet**

Eine transparente oder als Dialog gestaltete Activity kann den vorherigen Bildschirm sichtbar lassen, ihm jedoch den Fokus entziehen.

```text
MainActivity.onStart()
OverlayActivity wird darüber geöffnet
MainActivity erreicht onResume() nicht
```

2. **Weiterleitung in `onStart()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

Der Fokus wechselt zu einer anderen Activity, weshalb die aktuelle möglicherweise `onResume()` nicht erreicht.

3. **`finish()` vor `onResume()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

4. **Unterbrechung durch das System**

Zwischen `onStart()` und `onResume()` können der Benutzer oder das System einen anderen Bildschirm beziehungsweise eine andere Anwendung öffnen, beispielsweise bei einem eingehenden Anruf, durch die Systemoberfläche, im Mehrfenstermodus oder durch einen schnellen Wechsel in den Hintergrund.

Praktische Schlussfolgerung: Lebenszykluscode darf nicht davon ausgehen, dass auf jedes `onStart()` zwingend ein `onResume()` folgt.

Ressourcen müssen an das passende Callback-Paar gebunden werden:

```text
nur bei Sichtbarkeit   -> onStart() / onStop()
nur bei Interaktivität -> onResume() / onPause()
```

Für Flow:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) { /* sichtbare Benutzeroberfläche */ }
repeatOnLifecycle(Lifecycle.State.RESUMED) { /* aktive Interaktion */ }
```

**Kurz gesagt:** `onStart()` ohne anschließendes `onResume()` tritt auf, wenn eine Activity sichtbar, aber nicht interaktiv geworden ist, beispielsweise durch ein Overlay, eine Weiterleitung, `finish()` oder eine Systemunterbrechung. Die Bereinigung muss symmetrisch zu dem Lebenszykluszustand erfolgen, in dem eine Ressource gestartet wird.

</details>

<details>
<summary>187. Was sind Migrationen in Room?</summary>

#### Kotlin

Eine Room-Migration beschreibt den Übergang des Datenbankschemas von einer Version zu einer anderen, ohne Benutzerdaten zu verlieren.

Eine Migration ist erforderlich, wenn sich das Schema ändert:

- eine Tabelle oder Spalte wird hinzugefügt oder entfernt;
- Typ, Einschränkung oder Beziehung wird geändert;
- eine Tabelle oder ein Feld wird umbenannt;
- ein Index oder Fremdschlüssel wird hinzugefügt.

Nach einer Änderung des Entitätsschemas muss die `version` in `@Database` erhöht werden:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase()
```

Manuelle Migration:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Einbindung:

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

Wird für vorhandene Zeilen eine `NOT NULL`-Spalte hinzugefügt, ist ein Standardwert erforderlich:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

Für komplexe Änderungen, die SQLite nicht direkt erlaubt, wird eine Migration durch Kopieren der Tabelle durchgeführt:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

`AutoMigration` eignet sich für einfache, eindeutige Änderungen:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

Für Umbenennungen und Löschungen wird ein `AutoMigrationSpec` benötigt. Transformationen, das Aufteilen oder Zusammenführen von Tabellen und das nachträgliche Befüllen von Daten erfordern eine manuelle Migration.

`fallbackToDestructiveMigration()` löscht die alte Datenbank und erstellt eine neue. Dies ist für Cache- oder Entwicklungsdatenbanken zulässig, jedoch nicht für Benutzerdaten.

Praktische Regeln:

- bei jeder Schemaänderung die `version` erhöhen;
- einen Migrationspfad von allen produktiv eingesetzten Versionen unterstützen;
- das Schema mit `exportSchema = true` exportieren;
- Migrationen mit `MigrationTestHelper` testen;
- für wichtige Daten keinen destruktiven Fallback verwenden.

**Kurz gesagt:** Eine Room-Migration überführt das Datenbankschema unter Erhaltung der Daten in eine neue Version. Einfache Änderungen können mit `AutoMigration`, komplexe Änderungen mit einer manuellen `Migration` umgesetzt werden. Produktive Migrationspfade müssen stets getestet werden.

</details>

<details>
<summary>188. Was bewirkt die Annotation Embedded in Room?</summary>

#### Kotlin

`@Embedded` entfaltet in Room die Felder einer eingebetteten Klasse zu Spalten derselben Tabelle oder desselben Abfrageergebnisses. Das eingebettete Objekt wird nicht zu einer eigenen Tabelle.

Beispiel:

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

Room erstellt folgende Spalten:

```text
id, name, city, street
```

`@Embedded` eignet sich für Wertobjekte, die logisch Bestandteil einer Entität sind:

- Adresse;
- Koordinaten;
- Geldbetrag und Währung;
- Metadaten;
- wiederverwendbare Spaltengruppe.

Bei Konflikten zwischen Spaltennamen wird ein `prefix` verwendet:

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

Spalten:

```text
start_lat, start_lng, end_lat, end_lng
```

`@Embedded` ist außerdem in einem Lese-DTO für Ergebnisse eines `JOIN` nützlich:

```kotlin
data class UserWithStats(
    @Embedded val user: UserEntity,
    @Embedded(prefix = "stats_") val stats: UserStatsEntity
)
```

Unterschied zu `@Relation`:

```text
@Embedded -> Felder befinden sich in derselben Zeile beziehungsweise demselben Ergebnis
@Relation -> Beziehung zwischen unterschiedlichen Tabellen
```

Besitzt ein Objekt einen eigenen Lebenszyklus, wird von vielen Zeilen verwendet oder benötigt einen Fremdschlüssel, sollte keine Einbettung, sondern eine separate Tabelle mit Relation verwendet werden.

Einschränkungen:

- erstellt keinen Fremdschlüssel;
- normalisiert die Daten nicht;
- erstellt nicht automatisch eine Relation;
- kann eine Tabelle durch zahlreiche Spalten vergrößern;
- benötigt ein `prefix`, wenn sich Spaltennamen überschneiden.

**Kurz gesagt:** `@Embedded` bettet die Felder eines Wertobjekts in die aktuelle Tabelle oder das aktuelle Abfrageergebnis ein. Für Beziehungen zwischen Tabellen werden `@Relation`, Fremdschlüssel und separate Entitäten verwendet.

</details>

<details>
<summary>189. Was ist eine DatabaseView in Room?</summary>

#### Kotlin

`@DatabaseView` beschreibt in Room eine SQLite-`VIEW` als Kotlin-Klasse. Eine View speichert keine eigenen Daten, sondern stellt das Ergebnis einer SQL-Abfrage als schreibgeschütztes Modell dar.

Eine SQL-View ist eine benannte Abfrage:

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

Die View muss ausdrücklich in `@Database` aufgenommen werden:

```kotlin
@Database(
    entities = [UserEntity::class],
    views = [ActiveUserView::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase()
```

Lesen über ein DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM ActiveUserView")
    fun observeActiveUsers(): Flow<List<ActiveUserView>>
}
```

Eine `DatabaseView` ist nützlich für:

- komplexe Leseabfragen;
- Projektionen für die Benutzeroberfläche;
- Ergebnisse von `JOIN`s;
- Aggregate;
- die Wiederverwendung derselben SQL-Abfrage in mehreren DAO-Methoden.

Beispiel mit Aggregation:

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

Unterschiede:

```text
Entity       -> reale Tabelle, speichert Daten
DatabaseView -> schreibgeschützte Projektion, Ergebnis einer SQL-Abfrage
@Relation    -> Objektgraph aus miteinander verbundenen Tabellen
```

Einschränkungen:

- direktes `insert`, `update` oder `delete` ist bei einer View nicht möglich;
- die SQL-Abfrage muss bei Schemaänderungen gepflegt werden;
- komplexe Views können die Performance beeinträchtigen;
- Änderungen an zugrunde liegenden Tabellen können eine Migration erfordern.

**Kurz gesagt:** `@DatabaseView` ist ein schreibgeschütztes Modell auf Grundlage einer SQL-View. Es eignet sich für komplexe Lesemodelle, Joins und Aggregate, ersetzt jedoch keine Tabellen und unterstützt keine direkten Schreiboperationen.

</details>

<details>
<summary>190. Worin unterscheiden sich SQL und NoSQL?</summary>

#### Kotlin

`SQL` und `NoSQL` sind unterschiedliche Ansätze zur Modellierung und Speicherung von Daten.

```text
SQL   -> Tabellen, Schema, Beziehungen und SQL-Abfragen
NoSQL -> Dokument-, Schlüssel-Wert-, Wide-Column- oder Graphspeicher
```

SQL-Beispiel:

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
);
```

Beispiel für ein NoSQL-Dokument:

```json
{
  "id": "1",
  "name": "Alex",
  "settings": {
    "theme": "dark"
  }
}
```

Die wichtigsten Unterschiede:

```text
Schema:
SQL   -> strengeres Schema und Migrationen
NoSQL -> flexibleres Schema, mehr Kontrolle in Anwendung und Backend

Beziehungen:
SQL   -> JOIN, Fremdschlüssel und Normalisierung
NoSQL -> häufiger Denormalisierung für bestimmte Lesezugriffe

Transaktionen:
SQL   -> üblicherweise starke ACID-Garantien
NoSQL -> abhängig von der konkreten Datenbank und Konfiguration

Skalierung:
SQL   -> häufig vertikale Skalierung, Replikate und Sharding
NoSQL -> häufig einfachere horizontale Skalierung
```

Ein Android-Beispiel für SQL ist Room auf Grundlage von SQLite:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

Ein Android-Beispiel für NoSQL ist Firebase Firestore:

```kotlin
firestore.collection("users").document(id).get()
```

SQL ist vorzuziehen, wenn:

- die Datenstruktur stabil ist;
- Beziehungen und komplexe Abfragen vorhanden sind;
- Einschränkungen benötigt werden;
- Transaktionen und Konsistenz entscheidend sind.

NoSQL eignet sich besser, wenn:

- die Daten von Natur aus dokument- oder schlüsselwertorientiert sind;
- sich das Schema häufig ändert;
- nur wenige Beziehungen bestehen;
- Denormalisierung für schnelle Lesezugriffe erforderlich ist;
- eine verteilte Last mit hoher Skalierung vorliegt.

Die Auswahl erfolgt nicht nach einem Trend, sondern anhand von Zugriffsmustern, Konsistenzanforderungen, Beziehungen, Skalierung und Erfahrung des Teams.

**Kurz gesagt:** SQL ist ein relationales Modell mit Tabellen, Schema, Joins und starken Transaktionen. NoSQL umfasst nicht relationale Speicher mit flexibleren Datenmodellen, häufig mit Denormalisierung und einfacherer horizontaler Skalierung.

</details>

<details>
<summary>191. Was ist DataStore?</summary>

#### Kotlin

`DataStore` ist ein Jetpack-Speicher zum asynchronen Speichern kleiner persistenter Einstellungen. Er arbeitet mit Coroutines und `Flow` und ist eine moderne Alternative zu `SharedPreferences`.

Typische Daten:

- Theme;
- Sprache;
- Onboarding-Zustand;
- Benutzereinstellungen;
- Feature Flags.

Für große Collections, Beziehungen, Abfragen oder Offline-Caches wird `Room` statt DataStore benötigt.

Es gibt zwei Varianten:

```text
Preferences DataStore -> Schlüssel-Wert ohne Schema
Proto DataStore       -> Protobuf-Schema und typsicheres Modell
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

Proto DataStore verwendet ein `.proto`-Schema:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Unterschied zu `SharedPreferences`:

```text
SharedPreferences -> synchrone Schlüssel-Wert-API
DataStore         -> Suspend-API, Flow und transaktionale Aktualisierungen
```

Im ViewModel wird ein `Flow` üblicherweise in einen `StateFlow` umgewandelt:

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

Das Schreiben mit `edit` ist eine transaktionale Read-Modify-Write-Operation:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

Lesefehler werden ausdrücklich behandelt:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
    }
```

Für die Migration von `SharedPreferences` wird `SharedPreferencesMigration` verwendet.

**Kurz gesagt:** `DataStore` speichert kleine Einstellungen mithilfe von Coroutines und `Flow`. `Preferences DataStore` eignet sich für Schlüssel-Wert-Daten, `Proto DataStore` für typsichere Einstellungen und komplexe relationale Daten gehören in `Room`.

</details>

<details>
<summary>192. Was ist Paging 3?</summary>

#### Kotlin

`Paging 3` ist eine Jetpack-Bibliothek zum abschnittsweisen Laden großer Listen. Sie verwaltet Seitennavigation, Vorabladen, Aktualisierung, Wiederholungsversuche, Abbruch und die Integration in die Benutzeroberfläche.

Die zentrale Verarbeitungskette:

```text
PagingSource -> Pager -> Flow<PagingData<T>> -> Benutzeroberfläche
```

`PagingSource<Key, Value>` beschreibt die Datenquelle und die Regeln zum Laden von Seiten:

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

Ein `Pager` erstellt einen Stream aus `PagingData`:

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

Im ViewModel wird `cachedIn` ergänzt, damit der Paging-Stream im `viewModelScope` lebt und nicht für jeden Collector von vorn beginnt:

```kotlin
val users = repository.users()
    .cachedIn(viewModelScope)
```

Compose-Benutzeroberfläche:

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

`LoadState` stellt getrennte Zustände für `refresh`, `append` und `prepend` bereit. Üblicherweise gilt:

- `refresh` — bildschirmfüllendes Laden oder Fehleranzeige;
- `append` — Laden oder Fehleranzeige in der Fußzeile;
- `retry()` — fehlgeschlagenes Laden wiederholen;
- `refresh()` — eine neue Generation erstellen.

Für einen Offline-First-Ansatz wird `RemoteMediator` verwendet:

```text
Benutzeroberfläche <- PagingSource aus Room
                              ^
RemoteMediator: API -> Room + Remote Keys
```

In diesem Modell ist Room die Single Source of Truth. `RemoteMediator` lädt weitere API-Seiten und schreibt sie transaktional in die Datenbank.

Typische Fehler:

- falsche Seitenschlüssel;
- fehlendes `cachedIn`;
- Fehler bei `append` oder `prepend` ignorieren;
- instabile Elementschlüssel in der Benutzeroberfläche;
- manuelle Seitennavigation parallel zu Paging 3.

**Kurz gesagt:** `Paging 3` lädt große Listen seitenweise. Die grundlegende Kette lautet `PagingSource -> Pager -> PagingData -> UI`, `cachedIn` speichert den Stream im ViewModel zwischen und `RemoteMediator` wird für API und Room in einer Offline-First-Architektur benötigt.

</details>

<details>
<summary>193. Was ist ein RecyclerView.Adapter?</summary>

#### Kotlin

Ein `RecyclerView.Adapter` ist die Brücke zwischen einem Datensatz und einer `RecyclerView`. Er erstellt `ViewHolder`, bindet Daten an die Benutzeroberfläche eines Elements und meldet die Anzahl der Elemente.

Die wichtigsten Methoden:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

Beispiel mit `ListAdapter`:

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

Eine `RecyclerView` verwendet Element-Views wieder. Deshalb muss `bind()` den gesamten UI-Zustand setzen und nicht nur veränderte Felder. Andernfalls kann eine wiederverwendete View den Zustand des vorherigen Elements anzeigen.

`DiffUtil` bestimmt, was sich genau geändert hat:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

```text
areItemsTheSame()    -> Identität
areContentsTheSame() -> Inhalt
```

`ListAdapter` mit `DiffUtil` ist einer veränderlichen Liste und `notifyDataSetChanged()` vorzuziehen, da nur veränderte Elemente aktualisiert werden.

Für unterschiedliche Layouts wird `getItemViewType()` verwendet:

```kotlin
override fun getItemViewType(position: Int): Int = when (getItem(position)) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Typische Fehler:

- Geschäftslogik oder Navigation innerhalb des Adapters;
- unnötiges `notifyDataSetChanged()`;
- unvollständiger Zustand in `bind()`;
- Referenz auf eine `Activity` oder ein `Fragment`;
- Speichern einer alten `position` statt Arbeiten mit dem gebundenen Element.

**Kurz gesagt:** Ein `RecyclerView.Adapter` wandelt Daten in wiederverwendete Element-Views um. Im Produktivcode sind ein vollständiges `bind()`, `ListAdapter` mit `DiffUtil`, eine stabile Weitergabe von Klickereignissen nach außen und das Fernhalten von Geschäftslogik aus dem Adapter entscheidend.

</details>

<details>
<summary>194. Welche Hauptmethoden besitzt RecyclerView.Adapter?</summary>

#### Kotlin

Die wichtigsten Methoden von `RecyclerView.Adapter`:

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

`onCreateViewHolder()` erstellt einen neuen `ViewHolder`, wenn kein wiederverwendbarer Holder verfügbar ist:

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

`onBindViewHolder()` bindet ein Element an den Holder:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}
```

Aufgrund der Wiederverwendung muss `bind()` den gesamten UI-Zustand setzen: `true` und `false`, `null`, leere Werte, Sichtbarkeit und Auswahlzustand.

`getItemCount()` gibt die Größe des Datensatzes zurück:

```kotlin
override fun getItemCount(): Int = items.size
```

Für unterschiedliche Layouts wird `getItemViewType()` verwendet:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

Eine teilweise Aktualisierung kann über den Payload-Overload erfolgen:

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

Zusätzliche Lebenszyklus-Callbacks:

- `onViewRecycled()` — Bereinigung vor der Wiederverwendung eines Holders;
- `onViewAttachedToWindow()` — Element wurde an das Fenster angehängt;
- `onViewDetachedFromWindow()` — Element wurde vom Fenster getrennt.

Aktualisierungen des Datensatzes müssen präzise sein:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

Im Produktivcode werden häufiger `ListAdapter` oder `AsyncListDiffer` mit `DiffUtil` statt einer manuell veränderlichen Liste und `notifyDataSetChanged()` verwendet.

Typische Fehler:

- unvollständiger Zustand in `bind()`;
- aufwendige Arbeit in `onBindViewHolder()`;
- Speichern einer veralteten `position`;
- Veränderung des Datensatzes ohne passende Benachrichtigung oder Diff-Berechnung;
- nicht eindeutige stabile IDs.

**Kurz gesagt:** Die Hauptmethoden eines Adapters sind `onCreateViewHolder()`, `onBindViewHolder()` und `getItemCount()`. Zusätzlich sind `getItemViewType()`, das Binden mit Payloads, Recycling-Callbacks und eine korrekte Aktualisierung des Datensatzes wichtig.

</details>

<details>
<summary>195. Worin unterscheiden sich RecyclerView.Adapter und ListAdapter?</summary>

#### Kotlin

`ListAdapter` ist ein `RecyclerView.Adapter`, der bereits `AsyncListDiffer` und `DiffUtil` enthält. Bei einem gewöhnlichen Adapter muss der Entwickler die Speicherung der Liste und die `notify...`-Aufrufe selbst verwalten.

```text
RecyclerView.Adapter -> manuelle Liste und manuelle Benachrichtigungen
ListAdapter          -> submitList() und asynchrones DiffUtil
```

Gewöhnlicher `RecyclerView.Adapter`:

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

Das Problem besteht darin, dass Veränderungen des Datensatzes und `notify...`-Aufrufe exakt übereinstimmen müssen. `notifyDataSetChanged()` ist einfach, verhindert jedoch granulare Aktualisierungen, Animationen und einen Teil der Optimierungen.

`ListAdapter`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Aktualisierung:

```kotlin
adapter.submitList(users)
```

`ListAdapter` berechnet die Unterschiede außerhalb des Main Threads und wendet präzise `insert`-, `remove`-, `move`- und `change`-Aktualisierungen auf dem Main Thread an.

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
areItemsTheSame()    -> Identität des Elements
areContentsTheSame() -> dargestellter Inhalt
```

Wichtig: An `submitList()` muss ein unveränderlicher Snapshot übergeben werden. Dieselbe Liste oder ihre Elemente dürfen nach der Übergabe nicht verändert werden, da die Diff-Berechnung die Änderung möglicherweise nicht erkennt.

```kotlin
adapter.submitList(oldList + newUser)
```

Auswahl:

- `ListAdapter` — Standard für gewöhnliche dynamische Listen;
- `RecyclerView.Adapter` — wenn Aktualisierungen bereits als präzise Operationen eintreffen oder eine eigene Speicher- beziehungsweise Differ-Architektur benötigt wird;
- `PagingDataAdapter` — für Paging 3.

Typische Fehler:

- fehlerhaftes `areItemsTheSame()`;
- veränderliche Liste nach `submitList()`;
- `notifyDataSetChanged()` statt einer Diff-Berechnung;
- Geschäftslogik im Adapter;
- stabile IDs, die nicht mit der Identität in DiffUtil übereinstimmen.

**Kurz gesagt:** `RecyclerView.Adapter` bietet vollständige manuelle Kontrolle über Liste und Benachrichtigungen. `ListAdapter` nimmt über `submitList()` unveränderliche Snapshots entgegen und wendet selbstständig eine asynchrone Diff-Berechnung an. Daher ist er für die meisten Listen die bessere Standardwahl.

</details>

<details>
<summary>196. Wie funktioniert RecyclerView intern?</summary>

#### Kotlin

Eine `RecyclerView` zeigt große Listen an, ohne für jedes Element eine View zu erstellen. Sie hält sichtbare Views, einen kleinen Cache und einen Pool wiederverwendbarer `ViewHolder`.

```text
10.000 Datenelemente
etwa 10–20 sichtbare Views
Cache + Recycling-Pool
```

Die wichtigsten Komponenten:

- `Adapter` — erstellt `ViewHolder` und bindet Daten;
- `ViewHolder` — hält die Element-View und Referenzen auf ihre Views;
- `LayoutManager` — misst und positioniert Elemente;
- `Recycler` beziehungsweise `RecycledViewPool` — verwendet Holder wieder;
- `ItemAnimator` — animiert Änderungen;
- `ItemDecoration` — ergänzt Trennlinien, Abstände oder benutzerdefinierte Zeichnungen.

Vereinfachter Ablauf:

```text
LayoutManager fordert eine View an
 -> Recycler sucht einen Holder in Cache oder Pool
 -> Adapter erstellt einen Holder, wenn keiner wiederverwendbar ist
 -> Adapter bindet das Element
 -> LayoutManager misst und positioniert die View
```

Beim Scrollen wird eine Element-View, die den Bildschirm verlässt, getrennt und kann für ein anderes Element desselben `viewType` wiederverwendet werden.

Aufgrund der Wiederverwendung muss `bind()` den gesamten UI-Zustand setzen:

```kotlin
fun bind(item: Item) {
    binding.title.text = item.title
    binding.badge.isVisible = item.isImportant
    binding.checkbox.isChecked = item.isSelected
}
```

Werden `false`, `null`, leere Werte oder der Sichtbarkeitszustand nicht gesetzt, kann eine wiederverwendete View den Zustand des vorherigen Elements anzeigen.

Der `LayoutManager` bestimmt die Layoutstrategie:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
// GridLayoutManager(context, 2)
// StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

`getItemViewType()` trennt unterschiedliche Layouts. Der Pool gruppiert Holder nach `viewType`, sodass ein Header nicht als Beitrag wiederverwendet wird.

Vereinfachte Stufen der Wiederverwendung:

```text
attached scrap -> Views des aktuellen Layout-Durchlaufs
cached views    -> kürzlich getrennte Holder
recycled pool   -> Holder nach viewType für erneutes Binden
```

Für Listenaktualisierungen benötigt RecyclerView präzise Benachrichtigungen oder eine Diff-Berechnung:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
```

Im Produktivcode werden üblicherweise `ListAdapter` oder `AsyncListDiffer` mit `DiffUtil` verwendet. `notifyDataSetChanged()` liefert keine präzise Differenz und verschlechtert Animationen sowie Wiederverwendung.

Praktische Regeln:

- `onBindViewHolder()` muss kostengünstig sein;
- keine Datei- oder Netzwerkzugriffe und keine Bitmap-Dekodierung beim Binden;
- Klick-Callbacks müssen mit dem gebundenen Element statt einer alten `position` arbeiten;
- nach Einfügen oder Entfernen kann sich die Position ändern;
- Bilder müssen durch einen Image Loader mit Cache und Abbruchunterstützung geladen werden.

**Kurz gesagt:** Eine `RecyclerView` ist durch die Wiederverwendung von `ViewHolder`, den `LayoutManager`, Cache und Pool sowie punktuelle Aktualisierungen mit `DiffUtil` effizient. Entscheidend ist ein vollständiges und kostengünstiges `bind()`.

</details>

<details>
<summary>197. Was ist DiffUtil?</summary>

#### Kotlin

`DiffUtil` ist ein Hilfswerkzeug für `RecyclerView`, das eine alte und eine neue Liste vergleicht und präzise Änderungen ermittelt: `insert`, `remove`, `move` und `change`.

```text
notifyDataSetChanged() -> alles aktualisieren
DiffUtil               -> nur Änderungen aktualisieren
```

Für `ListAdapter` wird üblicherweise ein `DiffUtil.ItemCallback` erstellt:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

Bedeutung der Methoden:

```text
areItemsTheSame()    -> handelt es sich um dasselbe Element, üblicherweise anhand einer stabilen ID?
areContentsTheSame() -> sind die von der Benutzeroberfläche dargestellten Daten identisch?
```

`ListAdapter` verwendet intern `AsyncListDiffer`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Aktualisieren der Liste:

```kotlin
adapter.submitList(users)
```

Für eine teilweise Aktualisierung wird ein Payload verwendet:

```kotlin
override fun getChangePayload(oldItem: UserUi, newItem: UserUi): Any? {
    return if (oldItem.isOnline != newItem.isOnline) OnlineStatusChanged
    else null
}
```

Ein Payload ermöglicht die Aktualisierung eines Teils der Zeile ohne vollständiges erneutes Binden.

Unveränderlichkeit ist wichtig: An `submitList()` muss ein neuer Snapshot übergeben werden, statt die alte Liste zu verändern.

```kotlin
adapter.submitList(oldList + newUser)
```

Typische Fehler:

- `areItemsTheSame()` vergleicht das gesamte Objekt statt der ID;
- `areContentsTheSame()` gibt immer `true` zurück;
- instabile ID;
- Veränderung der Liste oder ihrer Elemente nach `submitList()`;
- `notifyDataSetChanged()` statt einer Diff-Berechnung;
- zu aufwendiges `equals()` bei großen verschachtelten Modellen.

**Kurz gesagt:** `DiffUtil` berechnet die Unterschiede zwischen einer alten und einer neuen Liste, damit RecyclerView nur veränderte Elemente aktualisiert. `areItemsTheSame()` bestimmt die Identität, `areContentsTheSame()` den Inhalt und `submitList()` muss einen unveränderlichen Snapshot erhalten.

</details>

<details>
<summary>198. Wie werden Listen unter Android dargestellt?</summary>

#### Kotlin

In einer View- beziehungsweise XML-Benutzeroberfläche werden Listen üblicherweise mit `RecyclerView` dargestellt, in Jetpack Compose mit Lazy-Containern wie `LazyColumn`, `LazyRow` und `LazyVerticalGrid`. Für große oder entfernte Listen wird Paging 3 ergänzt.

### View-System: RecyclerView

```kotlin
val adapter = UserAdapter(onClick = viewModel::onUserClicked)

binding.recyclerView.apply {
    layoutManager = LinearLayoutManager(requireContext())
    this.adapter = adapter
}

adapter.submitList(users)
```

Im Produktivcode sollte `ListAdapter` mit `DiffUtil.ItemCallback` statt einer veränderlichen Liste und `notifyDataSetChanged()` verwendet werden.

Der `LayoutManager` bestimmt die Darstellung der Liste:

- `LinearLayoutManager` — vertikale oder horizontale Liste;
- `GridLayoutManager` — Raster;
- `StaggeredGridLayoutManager` — Raster mit unterschiedlich hohen Elementen.

### Paging 3

Für seitenweises Laden wird `PagingDataAdapter` verwendet:

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

`loadState` wird für Laden, Fehler, leere Ergebnisse und Wiederholungsversuche benötigt. `refresh` und `append` müssen getrennt behandelt werden, damit ein Ladeindikator in der Fußzeile nicht den gesamten Bildschirm ersetzt.

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

Ein stabiler `key` bewahrt die Identität eines Elements beim Einfügen, Entfernen oder Umsortieren. Der Schlüssel muss eindeutig und stabil sein und darf nicht die Position darstellen.

Weitere Lazy-Container:

```kotlin
LazyRow {
    items(categories, key = Category::id) { CategoryChip(it) }
}

LazyVerticalGrid(columns = GridCells.Fixed(2)) {
    items(products, key = Product::id) { ProductCard(it) }
}
```

Paging wird in Compose mit `collectAsLazyPagingItems()` eingebunden. Dabei müssen ebenfalls `loadState` und `retry()` behandelt werden.

Praktische Regeln:

- nicht zahlreiche Views manuell zu einem `LinearLayout` hinzufügen;
- kein gewöhnliches `Column` für eine große dynamische Liste verwenden;
- unveränderliche Snapshots übergeben;
- den Listenzustand im ViewModel halten;
- Zustände für Laden, Fehler, leere Ergebnisse und Wiederholungsversuche anzeigen;
- keine aufwendige Arbeit in `bind()` oder während der Composition ausführen.

**Kurz gesagt:** Im View-System wird `RecyclerView` mit `ListAdapter`, in Compose werden `LazyColumn`, `LazyRow` oder `LazyVerticalGrid` verwendet. Große entfernte Listen benötigen zusätzlich Paging 3, stabile Schlüssel beziehungsweise IDs und eine korrekte Behandlung der Ladezustände.

</details>

<details>
<summary>199. Was sind Lazy-Container in Compose?</summary>

#### Kotlin

Lazy-Container in Compose sind Komponenten für Listen und Raster, die nur die für den sichtbaren Bereich benötigten Elemente komponieren und nicht sofort die gesamte Collection.

Die wichtigsten Container:

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

Beispiel für `LazyColumn`:

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

Für eine stabile Identität muss ein `key` übergeben werden:

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

Ein `key` hilft dabei, den Zustand eines Elements nach dem Einfügen, Löschen oder Umsortieren zu bewahren. Der Schlüssel muss stabil und eindeutig sein und darf nicht der Position entsprechen.

Für unterschiedliche Elementtypen wird `contentType` angegeben:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` hilft Compose, die Composition für kompatible Elementtypen effizienter wiederzuverwenden.

Der Scrollzustand wird durch `LazyListState` gesteuert:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

Programmgesteuertes Scrollen:

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

Beispiel für ein Raster:

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

Paging wird über `collectAsLazyPagingItems()` eingebunden:

```kotlin
val users = pager.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user -> UserRow(user) }
    }
}
```

Die `loadState`-Zustände für initiales Laden, weiteres Laden, Fehler, leere Ergebnisse und Wiederholungsversuche werden getrennt behandelt.

Performance-Regeln:

- stabile `key`s verwenden;
- `contentType` für unterschiedliche Elementtypen angeben;
- keine Ein- und Ausgabe oder aufwendigen Berechnungen in einem Element ausführen;
- keine Bitmaps während der Composition dekodieren;
- unnötig verschachtelte Scroll-Container vermeiden;
- unveränderliche und stabile UI-Modelle übergeben.

**Kurz gesagt:** Lazy-Container wie `LazyColumn`, `LazyRow` und Raster stellen große Listen und Raster anhand des sichtbaren Bereichs dar. Im Produktivcode sind stabile Schlüssel, `contentType`, eine leichtgewichtige Element-Benutzeroberfläche, `LazyListState` und korrekt behandelte Paging-Ladezustände wichtig.

</details>

<details>
<summary>200. Wie implementiert man eine zyklische Liste in RecyclerView?</summary>

#### Kotlin

Eine zyklische `RecyclerView` wird üblicherweise durch einen sehr großen `itemCount` simuliert: Eine künstliche Adapterposition wird mittels Modulo auf ein reales Element abgebildet.

Grundlegender Adapter:

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

Damit anfangs in beide Richtungen gescrollt werden kann, wird die Position ungefähr in die Mitte des Wertebereichs gesetzt:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

`items.isEmpty()` muss geprüft werden, da der Modulo andernfalls eine Division durch null verursacht. Auch das anfängliche Scrollen darf nur bei einer nicht leeren Liste erfolgen.

Für ein Karussellverhalten wird Snapping ergänzt:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

Automatisches Scrollen muss den Lebenszyklus berücksichtigen:

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

Stabile IDs bergen ein Risiko. Für alle Wiederholungen darf nicht dieselbe reale ID zurückgegeben werden, da die Adapterpositionen unterschiedlich sind. Werden stabile IDs nicht benötigt, sollten sie bei einem künstlich unendlichen Adapter nicht aktiviert werden.

Bei einer Aktualisierung des Datensatzes muss die reale Liste als unveränderlicher Snapshot gespeichert werden. Nach einer Größenänderung ist die aktuelle künstliche Position an den neuen Modulo anzupassen.

Typische Probleme:

- Modulo bei einer leeren Liste;
- automatisches Scrollen nach `onDestroyView()`;
- nicht eindeutige stabile IDs;
- fehlerhafte Wiederherstellung der Position nach einer Änderung des Datensatzes;
- komplexe Semantik für Barrierefreiheit;
- schlechte Benutzerführung durch unendliches Scrollen, wenn es nicht erforderlich ist.

Alternativen:

- `ViewPager2` für Pager oder Banner;
- Compose `HorizontalPager`;
- endliche Liste ohne künstliche Unendlichkeit;
- duplizierte Randelemente für einen nahtlosen Übergang.

**Kurz gesagt:** Eine zyklische `RecyclerView` wird mit einem großen `itemCount`, `position % items.size` und einem Start in der Mitte des Wertebereichs umgesetzt. Eine leere Liste, lebenszyklusbewusstes automatisches Scrollen, stabile IDs und Aktualisierungen des Datensatzes müssen zwingend korrekt behandelt werden.

</details>

<details>
<summary>201. Was sind Animationen unter Android?</summary>

#### Kotlin

Eine Animation unter Android ist die zeitliche Veränderung einer UI-Eigenschaft wie `alpha`, `translation`, `scale`, `rotation`, Farbe, Größe oder Layoutzustand. Eine gute Animation verdeutlicht eine Zustandsänderung, statt lediglich dekorativ zu wirken.

### View-System

Für einfache Transformationsanimationen:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

`ObjectAnimator` animiert eine bestimmte Eigenschaft:

```kotlin
ObjectAnimator
    .ofFloat(view, View.ALPHA, 1f, 0f)
    .apply {
        duration = 300
        start()
    }
```

`ValueAnimator` erzeugt Werte für benutzerdefinierte Logik:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    addUpdateListener { animation ->
        progressView.progress = animation.animatedValue as Int
    }
    start()
}
```

`AnimatorSet` kombiniert mehrere Animatoren. `MotionLayout` eignet sich für komplexe Übergänge zwischen Layoutzuständen.

### Compose

In Compose müssen Animationen zustandsgesteuert sein:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "contentAlpha"
)

Box(Modifier.alpha(alpha))
```

Die wichtigsten APIs:

- `animate*AsState` — einzelne Eigenschaft;
- `AnimatedVisibility` — Ein- und Ausblenden;
- `AnimatedContent` — Wechsel des Inhalts;
- `updateTransition` — mehrere Eigenschaften desselben Zustands;
- `Animatable` — imperative Suspend-Steuerung;
- `rememberInfiniteTransition` — unendliche dekorative Animationen.

### Performance

Folgende Eigenschaften lassen sich normalerweise kostengünstiger animieren:

```text
alpha, translation, scale, rotation
```

Die Animation von `width`, `height`, `margin` oder Layoutparametern kann in jedem Frame eine Messung und ein Layout auslösen. Sie ist nicht grundsätzlich verboten, erfordert jedoch Profiling.

In Callbacks dürfen keine Ein- und Ausgabe, keine Bitmap-Dekodierung, keine aufwendigen Berechnungen und nicht zahlreiche Allokationen stattfinden.

### Lebenszyklus

View-Animationen müssen abgebrochen und zurückgesetzt werden, insbesondere in einer `RecyclerView`:

```kotlin
override fun onViewRecycled(holder: ItemHolder) {
    holder.itemView.animate().cancel()
    holder.reset()
}
```

Andernfalls kann ein wiederverwendeter Holder `alpha`, `translation` oder den Auswahlzustand des vorherigen Elements anzeigen.

Compose-Effekte und Coroutines werden gemeinsam mit dem Composition-Scope abgebrochen. Externe Ressourcen benötigen dennoch eine Bereinigung.

Praktische Auswahl:

- einfache View-Transformation — `view.animate()`;
- Steuerung einer Eigenschaft oder eines Werts — `ObjectAnimator` beziehungsweise `ValueAnimator`;
- koordinierter Übergang — `AnimatorSet` beziehungsweise `MotionLayout`;
- Compose-Benutzeroberfläche — zustandsgesteuerte Animations-APIs;
- RecyclerView — Animation abbrechen und Zustand zurücksetzen;
- Produktivcode — systemweite Animationsskalierung und reduzierte Bewegung berücksichtigen.

**Kurz gesagt:** Android bietet View-Animatoren, `MotionLayout` und Compose-Animations-APIs. Eine produktionsreife Animation muss zustandsgesteuert, für das Rendern eines Frames kostengünstig und lebenszyklusbewusst sein und darf keinen Restzustand in wiederverwendeten Views hinterlassen.

</details>

<details>
<summary>202. Worin unterscheiden sich ViewBinding und DataBinding?</summary>

#### Kotlin

`ViewBinding` und `DataBinding` ersetzen das manuelle `findViewById`, lösen jedoch unterschiedliche Aufgaben.

```text
ViewBinding -> typsicherer Zugriff auf Views
DataBinding -> Bindung von Daten und Ausdrücken in XML
```

### ViewBinding

Erzeugt eine Binding-Klasse für ein XML-Layout:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Vorteile:

- einfache API;
- typsicherer Zugriff auf Views;
- geringerer Build-Overhead;
- einfacheres Debugging;
- verlagert keine Logik in XML.

### DataBinding

Ermöglicht die Deklaration von Variablen und Ausdrücken direkt in XML:

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

DataBinding unterstützt außerdem Two-Way Binding:

```xml
<EditText
    android:text="@={viewModel.query}" />
```

Dies kann bei einfachen Formularen nützlich sein, erschwert in großen Projekten jedoch häufig das Debugging des Datenflusses.

Die wichtigsten Risiken von DataBinding:

- komplexere Codegenerierung;
- größerer Overhead beim Build und zur Laufzeit;
- weniger offensichtliche Fehler;
- die Versuchung, UI- oder Geschäftslogik in XML zu verlagern;
- aufwendigere Einarbeitung des Teams.

Eine ungünstige Richtung ist komplexe Logik im Layout:

```xml
android:visibility="@{user.isAdmin ? View.VISIBLE : View.GONE}"
```

Einfache Ausdrücke sind zulässig. Komplexe Logik sollte jedoch im ViewModel oder in einem Mapper liegen.

Praktische Auswahl:

- neuer XML- beziehungsweise View-System-Code — üblicherweise `ViewBinding`;
- bestehendes Projekt mit XML-Ausdrücken — kann weiterhin `DataBinding` verwenden;
- neue deklarative Benutzeroberfläche — häufiger Compose.

**Kurz gesagt:** `ViewBinding` bietet einen einfachen, leichtgewichtigen und typsicheren Zugriff auf Views. `DataBinding` ergänzt Variablen, Ausdrücke und Two-Way Binding in XML, verursacht jedoch mehr Overhead und birgt das Risiko, Logik im Layout zu verstecken.

</details>

<details>
<summary>203. Was ist Serializable?</summary>

#### Kotlin

`java.io.Serializable` ist ein Marker-Interface für die binäre Java-Serialisierung. Die JVM kann einen Objektgraphen mit `ObjectOutputStream` in Bytes schreiben und mit `ObjectInputStream` wiederherstellen.

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

Dies ist nicht dasselbe wie `kotlinx.serialization.Serializable`. Dort generiert eine Annotation Serializer für JSON, Proto und andere Formate.

Unter Android kann ein `Serializable` über einen `Intent` oder ein `Bundle` übertragen werden:

```kotlin
val intent = Intent(context, DetailsActivity::class.java)
    .putExtra("user", user)
```

Seit API 33 gibt es einen typisierten Overload:

```kotlin
val user = intent.getSerializableExtra(
    "user",
    User::class.java
)
```

Für unterschiedliche API-Versionen sollten Kompatibilitätshilfen wie `IntentCompat.getSerializableExtra()` verwendet werden.

Vergleich mit `Parcelable`:

```text
Serializable -> JVM-Laufzeitserialisierung, weniger Code, größerer Overhead
Parcelable   -> Android-IPC- und Bundle-Format, schneller und expliziter
```

Für Android-Argumente ist `Parcelable` üblicherweise besser geeignet:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Bei der Navigation ist es noch besser, eine primitive `id` zu übergeben und die aktuellen Daten aus einem Repository zu laden.

Für Versionskompatibilität verwendet die Java-Serialisierung `serialVersionUID`:

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

Eine explizite UID legt die Versionskennung fest, macht die Schemaentwicklung jedoch nicht automatisch sicher.

Risiken:

- Overhead durch Reflection und Metadaten;
- schwache Schemaentwicklung;
- alle verschachtelten Felder müssen ebenfalls serialisierbar sein;
- mögliche `NotSerializableException`;
- große Objekte in `Bundle` oder `Intent` können eine `TransactionTooLargeException` auslösen;
- nicht vertrauenswürdige serialisierte Bytes aus Netzwerk oder externem Speicher dürfen nicht angenommen werden.

```kotlin
// ungünstig
intent.putExtra("response", hugeResponse)

// besser
intent.putExtra("user_id", user.id)
```

Vertretbare Anwendungsfälle:

- kleines internes Argument in Legacy-Code;
- kurzlebiges Objekt innerhalb einer einzelnen Anwendung;
- Performance und Schemaentwicklung sind nicht kritisch.

**Kurz gesagt:** Java-`Serializable` serialisiert einen Objektgraphen mit einem Laufzeitmechanismus der JVM. Unter Android sind für Argumente meist `Parcelable` oder lediglich eine `id` besser geeignet; für Persistenz und Netzwerk sollte ein explizites Schema wie Room, DataStore, JSON oder Proto verwendet werden.

</details>

<details>
<summary>204. Welche Möglichkeiten der Serialisierung gibt es unter Android?</summary>

#### Kotlin

Serialisierung ist die Umwandlung eines Objekts in ein Format zur Übertragung oder Speicherung, beispielsweise JSON, Bytes, `Bundle`, Datenbankzeile oder Protocol Buffers.

Unter Android wird sie am häufigsten benötigt für:

- API und Netzwerk;
- Navigationsargumente;
- `Intent`, `Bundle` und IPC;
- lokalen Speicher;
- Einstellungen.

### JSON

Für APIs wird meist JSON mit Kotlin Serialization, Moshi oder Gson verwendet.

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

Vorteile von Kotlin Serialization sind eine gute Kotlin-Unterstützung, `sealed class`, ein Plugin zur Kompilierzeit sowie die Integration mit Ktor und Multiplatform.

Moshi wird häufig mit Retrofit verwendet. Für Produktivcode ist Codegenerierung der Reflection vorzuziehen.

Gson ist einfach und in Legacy-Code weit verbreitet, arbeitet jedoch schlechter mit Kotlin-Nullbarkeit, verwendet intensiv Reflection und kann unbemerkt ungültige Objekte erzeugen.

### Parcelable

`Parcelable` ist ein Android-Format zur Übertragung von Objekten über `Intent`, `Bundle`, Navigationsargumente oder IPC.

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Bei der Navigation sollte häufig nicht das gesamte Objekt, sondern nur eine primitive `id` übergeben werden:

```kotlin
bundleOf("user_id" to userId)
```

Die aktuellen Daten werden anschließend aus Repository oder ViewModel geladen.

### Serializable

Java-`Serializable` ist einfach, besitzt jedoch Laufzeit-Overhead, eine schwächere Schemaentwicklung und weniger Kontrolle.

```kotlin
data class User(val id: String) : Serializable
```

Für Android-Argumente sind üblicherweise `Parcelable` oder eine `id` besser geeignet.

### Room

Für eine lokale Datenbank werden Room-Entitäten und Mapper verwendet, statt ein Domänenobjekt direkt zu serialisieren:

```kotlin
data class UserEntity(
    val id: String,
    val name: String
)

fun UserEntity.toDomain() = User(id, name)
```

Ein `TypeConverter` kann für einzelne komplexe Felder verwendet werden. Room sollte jedoch nicht ohne Notwendigkeit in einen JSON-Speicher verwandelt werden.

### Proto und DataStore

Für typsichere Einstellungen oder ein kompaktes Binärformat eignet sich Proto DataStore. Es erfordert ein Schema, bietet dafür jedoch ein stabiles typisiertes Modell.

Praktische Auswahl:

- API-JSON — Kotlin Serialization oder Moshi;
- Retrofit-Legacy-Code — je nach Projekt Moshi oder Gson;
- Navigationsargumente — primitive Typen, eine ID oder `Parcelable`;
- Intent und Bundle — kleines `Parcelable`, keine großen Datenmengen;
- lokale Datenbank — Room-Entitäten und Mapper;
- Einstellungen — DataStore oder Proto DataStore;
- Java-`Serializable` — nur für einfache interne Legacy-Fälle.

**Kurz gesagt:** Für Netzwerkzugriffe werden üblicherweise Kotlin Serialization oder Moshi verwendet, für Android-Argumente `Parcelable` oder eine primitive `id`, für Datenbanken Room-Entitäten und für Einstellungen DataStore beziehungsweise Proto. `Serializable` und Gson sollten überwiegend Legacy-Code oder einfachen Szenarien vorbehalten bleiben.

</details>

<details>
<summary>205. Was ist zu tun, wenn ein Feld in einer API-Antwort fehlen kann?</summary>

#### Kotlin

Ein DTO muss den API-Vertrag genau abbilden: Ein fehlendes Feld, ein explizites `null` und ein ungültiges Pflichtfeld sind unterschiedliche Fälle.

```text
fehlendes optionales Feld -> Standardwert
explizites null           -> nullable Typ
fehlendes Pflichtfeld     -> Dekodierungs- oder Mappingfehler
```

Kann das Backend `null` senden, wird das Feld nullable deklariert:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

Ein Ersatzwert sollte besser in einem Mapper festgelegt werden, statt Nullbarkeit durch die gesamte Benutzeroberfläche zu tragen:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name?.takeIf { it.isNotBlank() } ?: "Unknown",
    avatarUrl = avatarUrl
)
```

Kann ein Feld fehlen, besitzt jedoch einen sicheren Standardwert:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Ein fehlendes Feld und ein explizites `null` sind nicht dasselbe:

```json
{}
```

```json
{ "name": null }
```

Bei `kotlinx.serialization` wird ein Standardwert für einen fehlenden Schlüssel verwendet. Eine nullable Property ohne Standardwert kann abhängig von Deklaration und Konfiguration dennoch verpflichtend sein. Das Verhalten von `explicitNulls` muss verstanden und mit Tests abgedeckt werden.

Ein DTO sollte nicht direkt an die Benutzeroberfläche weitergegeben werden. Ein Mapper muss ein Domain- oder UI-Modell mit klaren Garantien erzeugen:

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

Ein Pflichtfeld darf nicht mit einem Ersatzwert verdeckt werden:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

Das Repository kann eine ungültige Antwort in einen typisierten Fehler umwandeln:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

Praktische Regeln:

- fehlendes optionales Feld — Standardwert;
- explizites `null` — nullable Typ;
- Pflichtfeld — Validierung beziehungsweise Fehler;
- DTO → Mapper → Domain- oder UI-Modell;
- Serializer-Verhalten durch Vertragstests für fehlende Werte, `null`, falsche Typen und unbekannte Enum-Werte abdecken.

**Kurz gesagt:** Ein fehlendes Feld wird mit einem Standardwert, ein explizites `null` mit einem nullable Typ modelliert. Ein Pflichtfeld ohne Wert muss einen Validierungsfehler statt eines stillen Ersatzwerts verursachen. DTOs werden auf Domain- oder UI-Modelle abgebildet.

</details>

<details>
<summary>206. Was ist Dependency Injection?</summary>

#### Kotlin

`Dependency Injection` bezeichnet die Übergabe von Abhängigkeiten an eine Klasse von außen, statt sie innerhalb der Klasse zu erstellen. Dies reduziert die Kopplung und vereinfacht Tests sowie die Verwaltung des Lebenszyklus.

Ungünstig ohne DI:

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Probleme:

- die Abhängigkeit lässt sich in einem Test nur schwer ersetzen;
- die Klasse ist selbst für die Erstellung verantwortlich;
- die Implementierung ist fest gekoppelt;
- Lebenszyklus und Konfiguration werden an der falschen Stelle kontrolliert.

Die beste Standardlösung ist Constructor Injection:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

Die Klasse erhält eine fertige Abhängigkeit und weiß nicht, wer sie erstellt hat.

Der Aufrufer hängt üblicherweise von einem Vertrag ab:

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

Unter Android verbindet DI `ViewModel`, Use Cases, Repositories, API-Clients, Room-Datenbank und Dispatcher.

Hilt-Beispiel:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Binden eines Interfaces:

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

Für Objekte von Drittanbietern wird `@Provides` verwendet:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

Scopes bestimmen die Lebensdauer einer Instanz:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

Ein Scope muss dem Besitzer entsprechen. Ein unnötiges Singleton kann ein Objekt länger als erforderlich halten und ein Speicherleck verursachen.

In Tests wird die reale Implementierung durch eine Fake-Implementierung ersetzt:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

DI benötigt nicht zwingend ein Framework. Für einen kleinen Objektgraphen genügt manuelle Dependency Injection:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Praktische Regeln:

- Constructor Injection als Standard;
- der Aufrufer hängt von einem Interface beziehungsweise Vertrag ab;
- Scopes entsprechen dem Lebenszyklus;
- Hilt, Dagger und Koin automatisieren lediglich die Verknüpfung;
- DI ersetzt keine geeignete Architektur.

**Kurz gesagt:** DI übergibt Abhängigkeiten von außen. Dadurch werden Kopplung reduziert, Abhängigkeiten in Tests austauschbar und Lebenszyklen besser verwaltbar. Für einen kleinen Objektgraphen genügt manuelle DI, für einen großen eignen sich Hilt, Dagger oder Koin.

</details>

<details>
<summary>207. Worin unterscheiden sich Dagger/Hilt und Koin?</summary>

#### Kotlin

`Dagger/Hilt` und `Koin` sind DI-Lösungen mit unterschiedlichen Modellen zur Verknüpfung von Abhängigkeiten.

```text
Dagger/Hilt -> generierter Code und Validierung des Graphen zur Kompilierzeit
Koin        -> Laufzeitcontainer und Kotlin-DSL beziehungsweise Annotationen
```

### Dagger und Hilt

`Dagger` generiert Factories und Komponenten und überprüft den Abhängigkeitsgraphen während der Kompilierung.

Vorteile:

- Validierung zur Kompilierzeit;
- generierter Code ohne Laufzeitsuche;
- explizite Scopes und Komponenten;
- gut für große, streng definierte Graphen geeignet.

Nachteile:

- mehr Annotationen und Module;
- komplexere Komponentenhierarchie;
- Compilerfehler sind gelegentlich schwer verständlich;
- kann die Build-Zeit beeinflussen.

`Hilt` ist eine Android-Schicht über Dagger. Es stellt eine standardisierte Hierarchie von Android-Komponenten, Scopes und Jetpack-Integrationen bereit.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

Typische Hilt-Scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

### Koin

Koin beschreibt Abhängigkeiten mit einer Kotlin-DSL oder Annotationen:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Vorteile:

- weniger Formalitäten;
- gut lesbare DSL;
- schneller Einstieg in kleinen und mittleren Projekten;
- praktisch für benutzerdefinierte Verknüpfung und KMP-Anwendungsfälle.

Nachteile:

- die klassische DSL verwendet mehr Laufzeitsuche;
- fehlende oder mehrdeutige Definitionen können ohne Prüfungen zur Kompilierzeit erst zur Laufzeit sichtbar werden;
- Scopes und Ownership des Graphen müssen vom Team diszipliniert modelliert werden.

Modernes Koin bietet Werkzeuge und Annotationen für Prüfungen zur Kompilierzeit. Das Modell bleibt dennoch von Dagger verschieden: Koin ist containerbasiert, während Dagger und Hilt auf einem generierten Graphen basieren.

### Performance und Tests

Dagger und Hilt rufen generierte Factories auf. Koin löst Definitionen über einen Container auf. Der Unterschied sollte am realen Graphen gemessen werden, statt ein Framework anhand eines Mikrobenchmarks auszuwählen.

Für Tests:

- Hilt — Testkomponenten, `@TestInstallIn` und Ersetzen von Modulen;
- Koin — Testmodule und Overrides;
- Unit-Tests — am einfachsten werden Fake-Implementierungen direkt über den Konstruktor übergeben.

### Auswahl

- Hilt — guter Standard für Android mit Jetpack und großem Graphen;
- Dagger — wenn ein sehr expliziter und kontrollierter DI-Graph benötigt wird;
- Koin — wenn eine einfache DSL, schnelles Wiring oder KMP wichtig sind;
- eine vorhandene funktionierende DI-Lösung sollte ohne messbaren Nutzen nicht neu geschrieben werden.

**Kurz gesagt:** Dagger und Hilt generieren DI-Code und prüfen den Graphen zur Kompilierzeit. Koin bietet einen einfacheren DSL- und containerbasierten Ansatz, erfordert jedoch Disziplin bei Scopes und Prüfungen. Die Auswahl hängt von der Größe des Graphen, den Anforderungen an Build und Laufzeit sowie der Erfahrung des Teams ab.

</details>

<details>
<summary>208. Was ist CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` ist ein Mechanismus von Jetpack Compose, mit dem Werte im Composition-Baum nach unten weitergegeben werden, ohne sie ausdrücklich als Parameter durch jedes Composable reichen zu müssen.

Es handelt sich um einen UI-Context mit einem Scope für einen Teilbaum:

```kotlin
CompositionLocalProvider(LocalSpacing provides AppSpacing()) {
    AppContent()
}
```

Deklaration:

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

Verwendung:

```kotlin
@Composable
fun ProfileCard() {
    val spacing = LocalSpacing.current

    Column(Modifier.padding(spacing.medium)) {
        Text("Profile")
    }
}
```

`ProfileCard` liest den nächstgelegenen Wert von `LocalSpacing`, der weiter oben im Baum bereitgestellt wurde.

Der Provider gilt ausschließlich für seinen untergeordneten Teilbaum:

```kotlin
CompositionLocalProvider(
    LocalSpacing provides AppSpacing(medium = 20.dp)
) {
    ProfileScreen()
}
```

Unterschied zwischen den APIs:

```text
compositionLocalOf       -> genauere Nachverfolgung von Lesezugriffen, für veränderliche Werte
staticCompositionLocalOf -> kostengünstigerer Lesezugriff, für nahezu statische Werte
```

Für Tokens eines Designsystems wird häufig `staticCompositionLocalOf` verwendet. Für Werte, die sich öfter ändern, eignet sich `compositionLocalOf`.

Integrierte CompositionLocals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`;
- `LocalLayoutDirection`.

```kotlin
val context = LocalContext.current
```

CompositionLocal eignet sich für:

- Theme-Farben;
- Typografie;
- Abstände;
- Design-Tokens;
- Gebietsschema und Konfiguration;
- UI-Hilfen, die logisch im gesamten Teilbaum verfügbar sind.

Geschäftliche Abhängigkeiten sollten nicht über CompositionLocal verborgen werden:

```kotlin
val repository = LocalUserRepository.current // ungünstig
```

Repositories, Use Cases und Services müssen über DI beziehungsweise ein ViewModel bereitgestellt werden. Andernfalls wird CompositionLocal zu einem Service Locator.

Risiken:

- verborgene Abhängigkeiten;
- schwierigere Tests;
- unklare Herkunft eines Werts;
- unnötige Recompositions bei häufigen Änderungen;
- Missbrauch anstelle expliziter Parameter.

**Kurz gesagt:** `CompositionLocal` übermittelt UI-Werte mit einem Scope im Compose-Baum nach unten, ohne Parameter durch jede Ebene zu reichen. Es eignet sich für Theme, Abstände, Typografie und Context-ähnliche UI-Daten, jedoch nicht für Repositories, Use Cases oder Geschäftslogik.

</details>

<details>
<summary>209. Welche Arten von CompositionLocal gibt es?</summary>

#### Kotlin

In Compose gibt es zwei grundlegende Möglichkeiten, ein `CompositionLocal` zu erstellen:

```text
staticCompositionLocalOf -> kostengünstigerer Lesezugriff, weniger präzise Invalidierung
compositionLocalOf       -> nachverfolgte Lesezugriffe, präzisere Recomposition
```

`staticCompositionLocalOf`:

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

Lesezugriffe werden nicht einzeln nachverfolgt. Ändert sich der Wert, wird der gesamte Inhalt des Providers invalidiert. Dies eignet sich für nahezu statische UI-Werte:

- Theme-Farben;
- Typografie;
- Abstände;
- Tokens des Designsystems.

`compositionLocalOf`:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

Compose verfolgt, welche Composables `.current` gelesen haben, und invalidiert sie bei einer Wertänderung präziser. Dies ist für Werte besser geeignet, die sich häufiger ändern können.

Ein Provider legt den Wert für einen Teilbaum fest:

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

Ein verschachtelter Provider kann den Wert ausschließlich für seinen eigenen Teilbaum überschreiben.

Beispiel für ein CompositionLocal des Designsystems:

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

Verwendung:

```kotlin
Modifier.padding(LocalSpacing.current.medium)
```

Integrierte CompositionLocals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLayoutDirection`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`.

Sie beschreiben die UI-Umgebung des aktuellen Teilbaums.

CompositionLocal sollte nicht als Service Locator verwendet werden:

```kotlin
val repository = LocalRepository.current // ungünstig
```

Repositories, Use Cases und Geschäftsdienste müssen über DI beziehungsweise ein ViewModel bereitgestellt werden. Wichtige Eingaben eines konkreten Composables sollten besser als explizite Parameter übergeben werden.

Risiken:

- verborgene Abhängigkeiten;
- schwierigere Tests;
- unklarer Besitzer eines Werts;
- unnötige Recompositions;
- Ersatz von DI und State Hoisting.

**Kurz gesagt:** `staticCompositionLocalOf` eignet sich für stabile UI-Werte und kostengünstigere Lesezugriffe. `compositionLocalOf` verfolgt Lesezugriffe präziser und eignet sich besser für veränderliche Werte. Beide sollten für den UI-Context und nicht für geschäftliche Abhängigkeiten verwendet werden.

</details>

<details>
<summary>210. Wie funktioniert CompositionLocal intern?</summary>

#### Kotlin

`CompositionLocal` funktioniert als Wert mit einem Scope im Compose-Baum. `CompositionLocalProvider` legt einen Wert für einen Teilbaum fest, und weiter unten liegende Composables lesen über `.current` den nächstgelegenen Wert.

Grundmodell:

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

CompositionLocalProvider(LocalSpacing provides AppSpacing(16.dp)) {
    Content()
}
```

Lesezugriff:

```kotlin
val spacing = LocalSpacing.current
```

Compose sucht in der Composition-Hierarchie den nächstgelegenen Provider für `LocalSpacing`.

Der Provider gilt ausschließlich für seinen Teilbaum:

```text
Root
 ├─ LocalSpacing = 8dp
 │   └─ ScreenA liest 8dp
 └─ LocalSpacing = 16dp
     └─ ScreenB liest 16dp
```

Ein verschachtelter Provider überschreibt den äußeren:

```kotlin
CompositionLocalProvider(LocalSpacing provides smallSpacing) {
    Header()

    CompositionLocalProvider(LocalSpacing provides largeSpacing) {
        Content()
    }
}
```

`Header` erhält `smallSpacing`, `Content` dagegen `largeSpacing`.

Compose verwendet Lesezugriffe zur Invalidierung. Wenn ein Composable Folgendes liest:

```kotlin
val colors = LocalAppColors.current
```

kann Compose diesen Lesezugriff mit dem Wert verknüpfen und bei einer Änderung entscheiden, dass eine Recomposition erforderlich ist.

Unterschied zwischen den Typen:

```text
compositionLocalOf       -> nachverfolgte Lesezugriffe, präzisere Invalidierung
staticCompositionLocalOf -> Lesezugriffe werden nicht einzeln verfolgt, kostengünstiger
```

`compositionLocalOf` eignet sich besser für Werte, die sich häufiger ändern können:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`staticCompositionLocalOf` eignet sich besser für nahezu statische Werte wie Theme, Typografie und Abstände:

```kotlin
val LocalTypography = staticCompositionLocalOf {
    DefaultTypography
}
```

Die Standard-Factory wird aufgerufen, wenn kein Wert bereitgestellt wurde:

```kotlin
val LocalAnalytics = staticCompositionLocalOf<Analytics> {
    error("Analytics not provided")
}
```

Bei verpflichtenden Werten ist ein `error` häufig sinnvoll, damit eine fehlerhafte Konfiguration schnell erkannt wird.

Performance-Besonderheit: Ändert sich ein CompositionLocal häufig und wird es in einem großen Teilbaum gelesen, können unnötige Recompositions entstehen. Hochfrequenter Zustand wie Scrollposition, Texteingabe oder Timer-Ticks gehört nicht hinein.

Praktische Regel: `CompositionLocal` ist keine globale Variable, sondern ein UI-Context mit einem Scope. Es wird für Werte der UI-Umgebung und nicht für verborgenen Zugriff auf Repositories oder Use Cases verwendet.

**Kurz gesagt:** `CompositionLocalProvider` legt einen Wert für einen Teilbaum fest, `.current` liest den nächstgelegenen Wert und Compose nutzt Lesezugriffe zur Invalidierung. `compositionLocalOf` verfolgt Lesezugriffe präziser, während `staticCompositionLocalOf` für nahezu statische Werte kostengünstiger ist.

</details>

<details>
<summary>211. Was sind Side Effects in Compose?</summary>

#### Kotlin

Ein Side Effect in Compose ist eine Aktion außerhalb der reinen Beschreibung der Benutzeroberfläche, beispielsweise eine Coroutine, ein Listener, Navigation, eine Snackbar, Analytics oder die Synchronisierung mit einem externen Objekt.

Der Body eines Composables kann mehrfach ausgeführt werden. Deshalb dürfen Effekte dort nicht direkt gestartet werden:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // ungünstig: wird bei Recomposition wiederholt
    Text(userId)
}
```

### LaunchedEffect

Startet eine an die Composition gebundene Coroutine:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Ändert sich der Schlüssel, wird die vorherige Coroutine abgebrochen und eine neue gestartet. Beim Verlassen der Composition wird die Coroutine ebenfalls abgebrochen. Die Schlüssel müssen sämtliche Abhängigkeiten des Effekts enthalten.

`LaunchedEffect(Unit)` bedeutet einen Start für den aktuellen Eintritt in die Composition und nicht einen einzigen Start während der gesamten Lebensdauer der Anwendung.

### rememberCoroutineScope

Wird benötigt, wenn eine Coroutine aus einem Callback gestartet wird:

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

`LaunchedEffect` startet deklarativ anhand von Schlüsseln, `rememberCoroutineScope` dagegen imperativ nach einem Benutzerereignis.

### DisposableEffect

Wird für den Lebenszyklus von Registrierung und Abmeldung verwendet:

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

Jede Registrierung eines Listeners oder Observers muss eine symmetrische Bereinigung in `onDispose` besitzen.

### SideEffect

Wird nach jeder erfolgreichen Composition ausgeführt:

```kotlin
SideEffect {
    externalController.enabled = isEnabled
}
```

Es eignet sich zum Veröffentlichen von Compose-Zustand in einem Nicht-Compose-Objekt, jedoch nicht für Suspend-Operationen oder aufwendige Arbeit.

### Weitere APIs

`rememberUpdatedState` stellt in einem lang laufenden Effekt eine aktuelle Lambda beziehungsweise einen aktuellen Wert bereit, ohne den Effekt neu zu starten:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

`snapshotFlow` wandelt Lesezugriffe auf Compose-Zustand in einen Flow um:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect(analytics::trackScroll)
}
```

`produceState` adaptiert eine Suspend- oder Callback-Quelle zu einem Compose-`State`. Für einen Flow in der Benutzeroberfläche wird häufiger `collectAsStateWithLifecycle()` verwendet.

`derivedStateOf` ist kein Side Effect, sondern abgeleiteter Zustand zur Optimierung eines berechneten UI-Zustands.

Praktische Regeln:

- Effekte nicht im Body eines Composables starten;
- Schlüssel korrekt festlegen;
- Bereinigung in `onDispose` durchführen;
- für eine Coroutine aus einem Callback `rememberCoroutineScope` verwenden;
- für eine aktuelle Lambda ohne Neustart `rememberUpdatedState` verwenden;
- dauerhafte Daten als Zustand und nicht als einmaligen Effekt speichern.

**Kurz gesagt:** Side Effects in Compose verwalten Aktionen außerhalb der UI-Beschreibung. `LaunchedEffect` startet eine Coroutine anhand von Schlüsseln, `DisposableEffect` verwaltet die Bereinigung, `SideEffect` synchronisiert ein externes Objekt und `rememberCoroutineScope` dient für Coroutines aus Callbacks.

</details>

<details>
<summary>212. Wie lässt sich Recomposition optimieren?</summary>

#### Kotlin

Recomposition sollte nicht „abgeschaltet“ werden. Sie muss lokal und kostengünstig sein. Probleme entstehen durch breit angelegte Lesezugriffe auf Zustand, instabile Parameter oder aufwendige Arbeit im Body eines Composables.

### Zustand auf der richtigen Ebene

Zustand sollte beim niedrigsten gemeinsamen Besitzer liegen, der ihn benötigt.

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

Wird `query` nur im Suchbereich benötigt, sollte es nicht auf der Ebene des gesamten Bildschirms gelesen werden.

### Lesezugriffe eingrenzen

Die Aufteilung in Funktionen allein garantiert keine Optimierung. Ein Composable sollte ausschließlich den benötigten Zustand lesen und stabile Parameter erhalten. Dann kann Compose unveränderte Gruppen überspringen.

### Unveränderliche und stabile Modelle

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

`@Immutable` darf nur verwendet werden, wenn der Vertrag tatsächlich erfüllt ist. Veränderliche Properties und In-Place-Änderungen beeinträchtigen die Vorhersehbarkeit.

Der Zustand wird korrekt mit einem unveränderlichen Snapshot aktualisiert:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

### Lazy-Listen

Lazy-Elemente benötigen einen stabilen `key`:

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

Ein `key` bewahrt gespeicherten Zustand beim Einfügen, Löschen und Umsortieren. Bei gemischten Listen sollte zusätzlich `contentType` angegeben werden.

### Keine aufwendige Arbeit während der Composition

Transformationen werden entweder in einen State Holder ausgelagert oder anhand ihrer Eingabe zwischengespeichert:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

Ein- und Ausgabe, Bitmap-Dekodierung sowie aufwendiges Sortieren oder Filtern ohne Cache dürfen nicht direkt im Body eines Composables erfolgen.

### derivedStateOf

`derivedStateOf` ist nützlich, wenn sich die Quelle häufig ändert, die Benutzeroberfläche jedoch nur auf eine abgeleitete Bedingung reagiert:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

### Profiling

Eine Optimierung muss mit Werkzeugen bestätigt werden:

- Recomposition-Zähler;
- Layout Inspector;
- Berichte des Compose-Compilers;
- System Trace beziehungsweise Perfetto;
- Android Studio Profiler;
- Macrobenchmark in einem releaseähnlichen Build.

Eine hohe Anzahl von Recompositions ist nicht zwangsläufig problematisch, wenn das Composable kostengünstig ist.

**Kurz gesagt:** Recomposition wird durch eingegrenzte Lesezugriffe auf Zustand, unveränderliche und stabile Modelle, unveränderliche Aktualisierungen, stabile Schlüssel in Lazy-Listen, `remember` beziehungsweise `derivedStateOf` sowie das Auslagern aufwendiger Arbeit aus der Composition optimiert. Gemessen wird mit einem Profiler und nicht nach Gefühl.

</details>

<details>
<summary>213. Wie schreibt man einen asynchronen Test ohne Coroutines?</summary>

#### Kotlin

Ein asynchroner Test ohne Coroutines muss auf ein konkretes Ereignis warten: einen Callback, ein Ergebnis, eine Zustandsaktualisierung oder einen Leerlaufzustand. Das Warten muss immer ein Zeitlimit besitzen. `Thread.sleep()` ist ungeeignet.

### CountDownLatch

Für eine Callback-basierte API:

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

Für ein einzelnes Ergebnis oder einen Fehler:

```kotlin
val future = CompletableFuture<User>()

repository.loadUser("42", object : UserCallback {
    override fun onSuccess(user: User) = future.complete(user)
    override fun onError(error: Throwable) = future.completeExceptionally(error)
})

assertEquals("42", future.get(2, TimeUnit.SECONDS).id)
```

### Warten auf einen Zustand

Es kann Awaitility oder eine eigene Polling-Hilfsfunktion verwendet werden:

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

Der Test endet unmittelbar nach Erfüllung der Bedingung, statt eine feste Verzögerung abzuwarten.

### LiveData

Für `LiveData` werden `InstantTaskExecutorRule` und eine Hilfsfunktion wie `getOrAwaitValue()` verwendet:

```kotlin
@get:Rule
val instantTaskExecutorRule = InstantTaskExecutorRule()

@Test
fun `emits user`() {
    val user = viewModel.user.getOrAwaitValue()
    assertEquals("42", user.id)
}
```

Die Hilfsfunktion muss sich anmelden, mit einem Zeitlimit auf einen Wert warten und den Observer in `finally` entfernen.

### UI-Tests

Espresso wird mit einem `IdlingResource` synchronisiert:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

### Fake-Abhängigkeit

Der stabilste Unit-Test verwendet ein steuerbares Fake:

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

Der Test ruft `complete()` selbst auf und hängt deshalb nicht vom Timing des Netzwerks oder Thread-Schedulers ab.

Antipattern:

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

Eine kurze Verzögerung erzeugt einen instabilen Test, eine lange Verzögerung verschwendet Zeit.

**Kurz gesagt:** Ein asynchroner Test ohne Coroutines muss mit einem Zeitlimit auf ein konkretes Ereignis warten. Dafür werden `CountDownLatch`, `CompletableFuture`, Polling beziehungsweise Awaitility, eine LiveData-Hilfsfunktion, Espresso-`IdlingResource` oder ein steuerbares Fake verwendet, jedoch nicht `Thread.sleep()`.

</details>

<details>
<summary>214. Worin unterscheiden sich inline, noinline und crossinline?</summary>

#### Kotlin

`inline`, `noinline` und `crossinline` sind Modifikatoren für Higher-Order Functions. Sie steuern, wie der Compiler Lambda-Parameter verarbeitet.

```text
inline      -> Funktion oder Lambda an der Aufrufstelle einbetten
noinline    -> eine bestimmte Lambda nicht einbetten
crossinline -> Lambda einbetten, aber Non-Local Return verbieten
```

### inline

`inline` weist den Compiler an, den Funktionskörper und die Lambda an der Aufrufstelle einzusetzen:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Bei kleinen Higher-Order Functions reduziert dies den Overhead durch das Erstellen eines Lambda-Objekts und einen Funktionsaufruf.

`inline` wird außerdem für Generics mit `reified` benötigt:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T =
    fromJson(json, T::class.java)
```

Ohne `inline` ist ein `reified`-Typparameter nicht möglich.

### Non-Local Return

In einer Inline-Lambda kann mit `return` die äußere Funktion verlassen werden:

```kotlin
inline fun runBlock(block: () -> Unit) = block()

fun test() {
    runBlock {
        return // verlässt test()
    }
}
```

Dies wird als Non-Local Return bezeichnet.

### noinline

`noinline` deaktiviert das Einbetten einer bestimmten Lambda:

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

`noinline` wird benötigt, wenn eine Lambda in einer Variablen gespeichert, weitergegeben oder als Objekt verwendet werden soll.

### crossinline

`crossinline` behält das Einbetten bei, verbietet jedoch einen Non-Local Return:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

Dies ist erforderlich, wenn die Lambda in einem anderen Kontext ausgeführt wird, beispielsweise innerhalb eines `Runnable`, Callbacks oder einer anderen Lambda.

Gemeinsames Beispiel:

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

Bedeutung:

- `inlined` — vollständig eingebettet;
- `stored` — kann gespeichert oder weitergegeben werden;
- `safe` — eingebettet, jedoch ohne Non-Local Return.

`inline` sollte nicht überall verwendet werden. Bei großen Funktionen oder zahlreichen Aufrufstellen kann es den Bytecode vergrößern. Es eignet sich am besten für kleine Higher-Order-Hilfsfunktionen, DSLs und Generics mit `reified`.

**Kurz gesagt:** `inline` bettet Funktion und Lambda ein und wird für `reified` benötigt. `noinline` erhält ein Lambda-Objekt, damit es gespeichert oder weitergegeben werden kann. `crossinline` verbietet einen Non-Local Return, wenn die Lambda in einem anderen Kontext ausgeführt wird.

</details>

<details>
<summary>215. Welche Vorteile bietet Kotlin gegenüber Java?</summary>

#### Kotlin

Kotlin bietet gegenüber Java praktische Vorteile, insbesondere unter Android: Null Safety, weniger Boilerplate-Code, Coroutines, Extension Functions, Data- und Sealed-Klassen, Smart Casts sowie eine starke Jetpack-Integration.

### Null Safety

Kotlin unterscheidet nullable und nicht nullable Typen:

```kotlin
val name: String = "Alex"
val nickname: String? = null
```

Nullable Werte müssen ausdrücklich behandelt werden:

```kotlin
val title = user.name ?: "Unknown"
```

Dies verhindert nicht jede `NullPointerException`, reduziert ihre Anzahl jedoch erheblich.

### Weniger Boilerplate-Code

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Eine `data class` generiert `equals`, `hashCode`, `toString`, `copy` und Komponentenfunktionen.

### Coroutines

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = State.Success(user)
}
```

Coroutines machen asynchronen Code lesbarer als Callbacks und unterstützen strukturierte Nebenläufigkeit.

### Extension Functions

```kotlin
fun View.show() {
    visibility = View.VISIBLE
}
```

Sie ermöglichen das Ergänzen nützlicher APIs ohne Vererbung oder Wrapper-Klassen.

### Sealed-Klassen und -Interfaces

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}
```

Sie eignen sich gut für UI-Zustände, Ergebnisse, Effekte und endliche Zustandsmodelle.

### Smart Casts

```kotlin
if (state is UiState.Content) {
    Text(state.user.name)
}
```

Nach einer Typprüfung führt Kotlin den Cast automatisch aus.

### Benannte Argumente und Standardargumente

```kotlin
fun createUser(name: String, active: Boolean = true)

createUser(name = "Alex")
```

Dadurch werden weniger Overloads benötigt und Aufrufstellen besser lesbar.

### Java-Interoperabilität

Kotlin arbeitet gut mit Java-Code und Java-Bibliotheken zusammen, sodass ein Projekt schrittweise migriert werden kann.

### Android-Ökosystem

Viele moderne Android-APIs sind Kotlin-first:

- Jetpack Compose;
- Coroutines und Flow;
- KTX-Erweiterungen;
- Suspend- und Flow-APIs von Room;
- Lifecycle-Scopes;
- Kotlin-DSL der Navigation.

Es bestehen auch Kompromisse:

- die Build-Zeit kann schlechter sein;
- Coroutines müssen gründlich verstanden werden;
- Java-Interoperabilität führt zu Plattformtypen;
- komplexe Generics und Typinferenz besitzen Sonderfälle;
- der übermäßige Einsatz von DSLs und Extensions verschlechtert die Lesbarkeit.

**Kurz gesagt:** Kotlin ermöglicht sichereren und ausdrucksstärkeren Code durch Null Safety, Data- und Sealed-Klassen, Coroutines, Extension Functions, Smart Casts, benannte und Standardargumente sowie bessere Android-Werkzeuge. Die Qualität hängt jedoch von Architektur und Disziplin und nicht allein von der Sprache ab.

</details>

<details>
<summary>216. Worin unterscheiden sich val und const val?</summary>

#### Kotlin

`val` und `const val` sind beide schreibgeschützt, bezeichnen jedoch unterschiedliche Konzepte.

```text
val       -> schreibgeschützte Property, Wert kann zur Laufzeit entstehen
const val -> Konstante zur Kompilierzeit
```

`val`:

```kotlin
val name = "Alex"
val createdAt = System.currentTimeMillis()
```

Ein `val` kann nach der Initialisierung nicht neu zugewiesen werden. Sein Wert darf jedoch zur Laufzeit berechnet werden.

`const val`:

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

Der Wert eines `const val` muss dem Compiler bekannt sein und wird an den Verwendungsstellen eingebettet.

`const val` darf an folgenden Stellen deklariert werden:

- auf Top-Level-Ebene;
- in einem `object`;
- in einem `companion object`.

```kotlin
object ApiConfig {
    const val BASE_PATH = "api/v1"
}
```

Ein `const val` darf nicht als lokale Variable innerhalb einer Funktion deklariert werden.

Die Typen für `const val` sind auf Literale zur Kompilierzeit beschränkt:

- `String`;
- primitive Typen wie `Int`, `Long`, `Boolean` und `Double`.

Nicht zulässig:

```kotlin
const val DATE = LocalDate.now() // Fehler
```

`const val` wird benötigt, wenn eine API eine Konstante zur Kompilierzeit verlangt, beispielsweise bei Annotationen:

```kotlin
const val TABLE_USERS = "users"

@Entity(tableName = TABLE_USERS)
data class UserEntity(...)
```

Ein gewöhnliches `val` ist hier ungeeignet, wenn die Annotation eine Konstante erwartet.

Java-Interoperabilität: `const val` wird als `static final`-Feld kompiliert und kann aus Java als Konstante verwendet werden.

Risiken:

- `const val` wird eingebettet, weshalb abhängige Module nach einer Wertänderung in einer Bibliothek möglicherweise neu kompiliert werden müssen;
- `const val` schützt keine Geheimnisse oder API-Schlüssel, da der Wert leicht aus der APK extrahiert werden kann;
- nicht jeder lediglich unveränderliche Wert sollte zu einem `const val` gemacht werden.

Praktische Auswahl:

- Laufzeitwert oder Objekt — `val`;
- literale Konstante zur Kompilierzeit — `const val`;
- Wert für eine Annotation — `const val`;
- Geheimnisse — nicht `const val`, sondern eine geeignete Sicherheits- und Konfigurationsstrategie.

**Kurz gesagt:** `val` ist eine schreibgeschützte Property, deren Wert zur Laufzeit entstehen kann. `const val` ist eine Konstante zur Kompilierzeit für `String` und primitive Typen, darf auf Top-Level-Ebene oder in einem `object` beziehungsweise `companion object` stehen und eignet sich für Annotationen. Geheimnisse dürfen nicht in einem `const val` gespeichert werden.

</details>

<details>
<summary>217. Welche Typen können mit const val verwendet werden?</summary>

#### Kotlin

`const val` bezeichnet eine Konstante zur Kompilierzeit. Der Typ muss `String` oder ein primitiver Typ sein, und der Initialisierer muss dem Compiler ohne Ausführung des Programms bekannt sein.

Zulässige Typen:

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

Typische Beispiele:

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Mögliche Deklarationsorte:

- auf Top-Level-Ebene;
- in einem `object`;
- in einem `companion object`.

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

Ein `const val` darf weder als lokale Variable noch als Instanz-Property deklariert werden.

Nicht zulässig:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val UUID_VALUE = UUID.randomUUID()
const val DEFAULT_THEME = ThemeMode.Light
const val NAME: String? = null
```

Für Objekte, Collections, nullable Werte und Laufzeitausdrücke wird ein gewöhnliches `val` verwendet:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

Ein Initialisierer darf Literale, andere Konstanten und Operationen zur Kompilierzeit enthalten:

```kotlin
const val HOST = "example.com"
const val API_URL = "https://" + HOST
const val TIMEOUT_SECONDS = 5 * 2
```

Funktionsaufrufe, ein `Context`, die aktuelle Zeit, Zufallswerte oder die Erstellung eines Objekts sind keine Konstanten zur Kompilierzeit.

`const val` wird häufig für Argumente von Annotationen benötigt:

```kotlin
const val USERS_TABLE = "users"

@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

**Kurz gesagt:** `const val` unterstützt ausschließlich `String` und primitive Typen mit einem Initialisierer zur Kompilierzeit. Für Objekte, Collections, nullable Werte oder Laufzeitwerte muss ein gewöhnliches `val` verwendet werden.

</details>

<details>
<summary>218. Kann eine data class ohne Parameter erstellt werden?</summary>

#### Kotlin

Nein. Eine `data class` muss einen primären Konstruktor mit mindestens einem Parameter besitzen, und dieser Parameter muss ein `val` oder `var` sein.

Nicht korrekt:

```kotlin
data class Empty
data class Empty()
data class User(id: String)
```

Korrekt:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Der Grund besteht darin, dass der Compiler die Methoden anhand der Properties des primären Konstruktors generiert:

```text
equals()
hashCode()
toString()
copy()
componentN()
```

Eine Property im Klassenkörper wird bei generierter Gleichheit, Hashwert, Kopie und Destrukturierung nicht berücksichtigt:

```kotlin
data class User(
    val id: String
) {
    var cachedLabel: String = ""
}
```

Zwei `User` mit derselben `id`, aber unterschiedlichem `cachedLabel` gelten als gleich. Deshalb müssen wertbestimmende Felder im primären Konstruktor stehen.

Ein Aufruf ohne Argumente ist über Standardwerte möglich:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)

val user = User()
```

Die Klasse besitzt weiterhin Parameter; der Aufrufer verwendet lediglich deren Standardwerte. Standardwerte sollten jedoch nicht nur für ein Framework ein ungültiges Domänenobjekt erzeugen.

Benötigt ein Framework einen parameterlosen Konstruktor, sollten besser folgende Möglichkeiten verwendet werden:

- separates Persistenz- oder DTO-Modell;
- Adapter beziehungsweise Mapper;
- Kotlin-No-Arg-Plugin;
- gültige Standardwerte, sofern sie fachlich sinnvoll sind.

Besitzt ein Zustand keine Nutzdaten, ist ein Singleton besser geeignet:

```kotlin
data object Loading
```

In einer Sealed-Hierarchie:

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

Weitere Einschränkungen: Eine `data class` darf weder `abstract`, `open`, `sealed` noch `inner` sein, kann jedoch Interfaces implementieren.

Praktische Auswahl:

- Wertmodell — `data class`;
- Aufruf ohne Argumente — Standardwerte;
- Zustand ohne Nutzdaten — `object` oder `data object`;
- parameterloser Konstruktor für ein Framework — Adapter oder Plugin;
- wertbestimmende Felder — ausschließlich im primären Konstruktor.

**Kurz gesagt:** Eine leere `data class` lässt sich nicht kompilieren. Im primären Konstruktor wird mindestens ein `val`- oder `var`-Parameter benötigt. Für einen Aufruf ohne Argumente werden Standardwerte, für einen Zustand ohne Nutzdaten `object` oder `data object` verwendet.

</details>

<details>
<summary>219. Worin unterscheiden sich sealed class und enum?</summary>

#### Kotlin

`enum` und `sealed` begrenzen die Menge möglicher Varianten, modellieren jedoch unterschiedliche Konzepte.

```text
enum   -> feste Singleton-Einträge desselben Typs
sealed -> geschlossene Hierarchie unterschiedlicher Untertypen
```

### Enum

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

Ein `enum` eignet sich, wenn alle Fälle dieselbe Struktur besitzen und keine unterschiedlichen Nutzdaten benötigen. Jeder Eintrag ist ein Singleton:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Ein Enum kann gemeinsame Properties besitzen:

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

Alle Einträge besitzen denselben Property-Vertrag. Ein Enum stellt integrierte APIs wie `entries`, `valueOf()`, `name` und `ordinal` bereit.

`ordinal` sollte nicht in einer Datenbank oder API gespeichert werden, da sich die Reihenfolge der Einträge ändern kann. Für Persistenz ist ein expliziter stabiler Code besser geeignet.

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

`sealed` eignet sich, wenn Varianten unterschiedliche Nutzdaten oder unterschiedliches Verhalten besitzen. `Loading` ist ein Singleton, `Content` enthält Daten und `Error` ein anderes Modell.

Typische Anwendungsfälle:

- UI-Zustand;
- Domänenergebnis;
- Fehler;
- Ereignisse;
- endlicher Zustandsautomat.

### Vollständiges when

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

Der Compiler kennt alle Fälle und kann die Vollständigkeit eines `when` prüfen. Ein unnötiges `else` verbirgt häufig den Nutzen dieser Prüfung.

### Sealed Class und Sealed Interface

Eine `sealed class` kann Konstruktorzustand und geschützte Member besitzen:

```kotlin
sealed class AppError(
    open val code: Int
)
```

Ein `sealed interface` besitzt keinen Konstruktorzustand. Eine Implementierung kann jedoch mehrere Interfaces implementieren und von einer anderen Klasse erben. Für Zustands- und Ergebnismodelle genügt häufig ein `sealed interface`.

### Serialisierung

Ein Enum wird üblicherweise als stabiler Text- oder Codewert serialisiert. Eine Sealed-Hierarchie benötigt einen Diskriminator für den Untertyp und ein Schema für die Nutzdaten. Versionsverwaltung und unbekannte Varianten müssen berücksichtigt werden.

Praktische Auswahl:

- einfacher fester Modus oder Status — `enum`;
- Fälle ohne unterschiedliche Nutzdaten — `enum`;
- UI-, Ergebnis- oder Fehlerzustand mit unterschiedlichen Feldern — `sealed`;
- Kombination aus Object- und Data-Class-Fällen in einem Modell — `sealed`;
- ein Enum nicht ohne konkreten Bedarf „für die Zukunft“ durch einen Sealed-Typ ersetzen.

**Kurz gesagt:** `enum` ist eine feste Liste von Singleton-Einträgen mit gemeinsamer Struktur. `sealed class` beziehungsweise `sealed interface` bildet eine geschlossene Hierarchie, in der jeder Fall eigene Nutzdaten besitzen kann. Für einfache Modi dient `enum`, für Zustands- und Ergebnismodelle `sealed`.

</details>

<details>
<summary>220. Was ist WorkManager?</summary>

#### Kotlin

`WorkManager` ist eine Jetpack-API für garantiert auszuführende, aufschiebbare Hintergrundarbeit. Sie wird für Aufgaben verwendet, die auch nach dem Schließen der Anwendung oder dem Beenden des Prozesses ausgeführt werden müssen, jedoch nicht zwingend sofort.

Geeignet für:

- Hintergrundsynchronisierung;
- Uploads und Downloads mit Wiederholungsversuchen;
- Bereinigung alter Daten;
- Versenden von Protokollen und Analysedaten;
- periodische Synchronisierung;
- Aufgaben mit Bedingungen wie Netzwerkverbindung, Ladevorgang oder ausreichend Akkuladung.

Einmalige Aufgabe:

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

Ergebnisse:

```text
Result.success() -> erfolgreich ausgeführt
Result.retry()   -> später erneut versuchen
Result.failure() -> mit Fehler beenden
```

Bedingungen:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()
```

WorkManager startet die Arbeit erst, wenn die Bedingungen erfüllt sind.

Periodische Arbeit:

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodische Arbeit garantiert keinen exakten Startzeitpunkt. Android wählt den Zeitpunkt unter Berücksichtigung des Akkus und der Systemrichtlinien.

Um Duplikate zu vermeiden, wird Unique Work verwendet:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` behält die aktuelle Aufgabe bei, `REPLACE` bricht die alte ab und plant eine neue.

Eingabedaten müssen klein sein:

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Große Objekte sollten in einer Datenbank gespeichert und nur über ihre `id` referenziert werden.

Verkettung:

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

WorkManager eignet sich nicht für:

- zeitgenaue Alarme;
- eine dauerhafte Socket-Verbindung;
- Medienwiedergabe;
- Echtzeitverfolgung;
- Arbeit, die sofort beginnen muss;
- lang andauernde, für den Benutzer sichtbare Vordergrundabläufe ohne Unterbrechung.

**Kurz gesagt:** `WorkManager` dient aufschiebbarer, garantiert auszuführender Hintergrundarbeit mit Bedingungen, Wiederholungsversuchen, periodischer oder eindeutiger Arbeit und Verkettung. Für Echtzeitkommunikation, zeitgenaue Alarme, Medienwiedergabe oder sofortige Vordergrundarbeit werden andere APIs benötigt.

</details>

<details>
<summary>221. Wann sollte WorkManager verwendet werden?</summary>

#### Kotlin

`WorkManager` sollte für persistente, aufschiebbare Arbeit verwendet werden: Eine Aufgabe darf verzögert werden, muss aber nach dem Schließen der Benutzeroberfläche, dem Beenden des Prozesses oder einem Neustart unter Berücksichtigung von Bedingungen zuverlässig ausgeführt werden.

Geeignet für:

- Hintergrundsynchronisierung;
- Wiederholungsversuche bei Uploads und Downloads;
- Bereinigung alter Daten;
- Versenden von Protokollen und Analysedaten;
- periodische Aktualisierung;
- Aufgaben mit Bedingungen für Netzwerk, Ladevorgang oder Akkustand.

Ein exakter Ausführungszeitpunkt wird nicht garantiert. Ein erzwungener Stopp und eine Deinstallation beenden die Arbeit.

Einmalige Arbeit:

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

Beispiel für einen `CoroutineWorker`:

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

Ergebnisse:

```text
success() -> abgeschlossen
retry()   -> vorübergehender Fehler
failure() -> dauerhafter Fehler
```

Ein Worker muss idempotent sein, da das System ihn stoppen und erneut starten kann. Eine `CancellationException` darf nicht in einen Wiederholungsversuch umgewandelt werden.

Bedingungen beschreiben die Voraussetzungen für den Start, jedoch keinen exakten Zeitpunkt:

- Netzwerk;
- Ladevorgang;
- Akkustand nicht niedrig;
- Speicherplatz nicht knapp.

Unique Work verhindert doppelte Aufgaben:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "user-sync",
        ExistingWorkPolicy.KEEP,
        request
    )
```

Periodische Arbeit wird nur ungefähr ausgeführt. Doze, Akkurichtlinien und Bedingungen können den Start verschieben.

Eingabe-`Data` muss klein sein und aus primitiven Typen oder Strings bestehen. Große Objekte sollten in Room oder einer Datei gespeichert und dem Worker ausschließlich über ihre `id` übergeben werden.

Ungeeignete Fälle:

```text
bildschirmgebundene Arbeit -> ViewModel- oder Lifecycle-Scope
zeitgenauer Alarm          -> AlarmManager
Medien oder Tracking       -> Foreground Service
Echtzeit-Socket            -> aktive Laufzeitarchitektur
```

`ExpeditedWorkRequest` unterliegt Kontingenten und macht WorkManager nicht zu einer Echtzeit-API.

**Kurz gesagt:** WorkManager wird für idempotente Hintergrundarbeit benötigt, die aufgeschoben werden darf, jedoch mit Bedingungen und Wiederholungsversuchen zuverlässig ausgeführt werden muss. Für UI-gebundene, Echtzeit-, Medien-, Tracking- oder zeitgenaue Aufgaben werden andere APIs verwendet.

</details>

<details>
<summary>222. Haben Sie mit Android Media3 beziehungsweise ExoPlayer gearbeitet?</summary>

#### Kotlin

Ja. `Media3` ist der moderne Medien-Stack von Jetpack, und `ExoPlayer` ist die Standardimplementierung des `Player`-Interfaces. Er unterstützt lokale und gestreamte Medien, Wiedergabelisten, adaptives Streaming, Untertitel und DRM.

Grundlegende Verwendung:

```kotlin
val player = ExoPlayer.Builder(context).build()

player.setMediaItem(MediaItem.fromUri(videoUrl))
player.prepare()
player.play()
```

In einer View-basierten Benutzeroberfläche:

```kotlin
playerView.player = player
```

Entscheidend sind ein korrekt definierter Besitzer des Players und der garantierte Aufruf von `release()`.

```kotlin
override fun onDestroyView() {
    playerView.player = null
    player.release()
    super.onDestroyView()
}
```

Eine Referenz auf `PlayerView` darf `onDestroyView()` nicht überleben.

In Compose kann die ältere `PlayerView` mit `AndroidView` eingebunden werden:

```kotlin
AndroidView(
    factory = { context ->
        PlayerView(context).apply {
            player = exoPlayer
        }
    }
)
```

Der Player darf nicht bei jeder Recomposition neu erstellt werden. Er benötigt einen stabilen Besitzer und eine Bereinigung über `DisposableEffect`, einen Holder oder einen Service.

Wiedergabeliste:

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

`Player.Listener` stellt Wiedergabezustände und Fehler bereit:

- `BUFFERING`;
- `READY`;
- `ENDED`;
- Wiedergabefehler;
- Wechsel des Medienelements.

Die Benutzeroberfläche bildet diese Ereignisse auf Lade-, Fehler- und Wiederholungszustände ab. Analytics sollte nicht mit der Darstellungslogik vermischt werden.

Bei Hintergrundwiedergabe, Musik oder Podcasts darf der Player nicht einer `Activity` oder einem `Fragment` gehören. Die richtige Architektur lautet:

```text
MediaSessionService besitzt Player und MediaSession
Benutzeroberfläche verbindet sich über MediaController
```

Dies ermöglicht:

- Medienbenachrichtigung;
- Systemsteuerung;
- Integration mit Bluetooth, Wear OS und Android Auto;
- Unabhängigkeit der Wiedergabe vom Bildschirmlebenszyklus.

Für Streaming und Cache wird `CacheDataSource` verwendet. Cache-Größe, Verdrängungsstrategie, Offline-Verhalten und DRM-Bedingungen werden dabei ausdrücklich festgelegt.

Typische Fehler:

- `release()` nicht aufrufen;
- Player während einer Recomposition erstellen;
- `PlayerView` nach `onDestroyView()` halten;
- Hintergrundwiedergabe an den Bildschirmlebenszyklus binden;
- Fehler oder Audio Focus nicht behandeln;
- Player nicht von seinem Application Thread aus aufrufen.

**Kurz gesagt:** Media3 und ExoPlayer benötigen einen klaren Besitzer, lebenszyklusbewusste Bereinigung, `release()`, die Behandlung von Zuständen und Fehlern sowie korrekten Thread-Zugriff. Für Hintergrundwiedergabe wird `MediaSessionService` statt einer `Activity` oder eines `Fragment` verwendet.

</details>

<details>
<summary>223. Wie würden Sie die Komponenten auf Module verteilen, wenn eine Schaltfläche beim Anklicken Daten lädt?</summary>

#### Kotlin

Module sollten um ein Feature beziehungsweise einen Use Case herum aufgebaut werden und nicht um eine Schaltfläche.

```text
Schaltfläche -> ViewModel -> Use Case -> Repository -> API/Datenbank
```

Die Benutzeroberfläche sendet lediglich ein Ereignis und stellt den Zustand dar. Sie weiß nicht, aus welcher Quelle die Daten stammen: Netzwerk, Room oder Cache.

Für eine kleine Anwendung genügt folgende Struktur:

```text
:app
:core:network
:core:database
:feature:profile
```

In `:feature:profile` können folgende Packages liegen:

```text
presentation
domain
data
```

Separate Gradle-Module für jede Schicht sind nicht erforderlich, wenn keine tatsächliche Isolation oder getrennte Verantwortlichkeit besteht.

Bei einem großen Feature können API und Implementierung getrennt werden:

```text
:feature:profile:api
:feature:profile:impl
```

`api` enthält öffentliche Verträge wie Navigation, Fähigkeiten und Interfaces. `impl` enthält Bildschirm, ViewModel, Use Cases und Repository-Implementierung.

### Präsentationsschicht

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

Das ViewModel:

- nimmt das Klickereignis entgegen;
- startet eine Coroutine;
- verwaltet `loading`, `content` und `error`;
- behandelt doppelte Klicks;
- hält den Zustand in einem `StateFlow`.

Retrofit, SQL und Cache-Logik gehören nicht in die Präsentationsschicht.

### Domain-Schicht

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

Ein Use Case ist sinnvoll, wenn eine Geschäftsregel, Orchestrierung oder Wiederverwendung vorhanden ist. Handelt es sich in einem kleinen Feature lediglich um einen einfachen Proxy, kann das ViewModel direkt vom Repository-Vertrag abhängen.

Die Domain-Schicht hängt weder von der Android-Benutzeroberfläche noch von Retrofit, Room oder einem DI-Framework ab.

### Datenschicht

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

Die Datenschicht ist für API, Datenbank, Cache beziehungsweise Single Source of Truth, Mapping und die Abbildung von Infrastrukturfehlern verantwortlich.

Abhängigkeitsrichtung:

```text
app -> Feature-Implementierung
presentation -> Domain-Vertrag
data -> Domain-Vertrag
core -X-> feature
```

Dependency Injection verbindet die Implementierung im Composition Root mit dem Vertrag:

```kotlin
@Binds
abstract fun bindRepository(
    impl: ProfileRepositoryImpl
): ProfileRepository
```

Kriterien für ein separates Gradle-Modul:

- unabhängige Verantwortlichkeit;
- stabile öffentliche API;
- Isolation von Abhängigkeiten;
- wiederverwendbare Fähigkeit;
- Vorteil bei der Build-Zeit.

Liegen diese Gründe nicht vor, sind Packages einfacher. Übermäßige Modularisierung fügt Boilerplate-Code für Gradle, DI und Navigation ohne tatsächlichen Nutzen hinzu.

**Kurz gesagt:** Schaltfläche und Zustand gehören in die Präsentationsschicht, die Geschäftsoperation in Domain beziehungsweise Use Case und API, Datenbank sowie Repository-Implementierung in die Datenschicht. Für eine kleine Anwendung genügen Packages; Gradle-Module werden nur für Isolation, Verantwortlichkeit oder einen Vorteil bei der Build-Zeit ergänzt.

</details>
