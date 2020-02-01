**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Najpopularniejsze pytania i odpowiedzi na rozmowie kwalifikacyjnej z Kotlin</h2>

<details>
<summary>1. Czym jest Kotlin i jak współpracuje z Javą?</summary>

#### Kotlin

Kotlin to statycznie typowany język programowania stworzony przez JetBrains.
Obsługuje JVM, Androida, JavaScript, WebAssembly oraz platformy Native.

Na JVM Kotlin kompiluje się do bajtkodu JVM, dlatego jest w pełni zgodny z Javą:

- Kotlin może bezpośrednio wywoływać kod Javy i korzystać z bibliotek Javy.
- Java może wywoływać kod Kotlin jako zwykłe klasy i metody JVM.
- Oba języki mogą być używane w tym samym projekcie i module.
- Adnotacje `@JvmStatic`, `@JvmOverloads`, `@JvmField` i `@JvmName` pomagają
  tworzyć API wygodne dla Javy.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

Ważny niuans to typy platformowe: jeśli API Javy nie ma poprawnych adnotacji
nullability, Kotlin nie zawsze może określić, czy `null` jest dozwolone. Dlatego
na granicy Java/Kotlin potrzebne są jawne kontrakty.

**Krótko:** Kotlin jest zgodny z Javą na poziomie JVM i pozwala stopniowo
dodawać Kotlin do istniejącego projektu Java bez pełnego przepisywania.

</details>

<details>
<summary>2. Jakie podstawowe typy danych istnieją w Kotlin?</summary>

#### Kotlin

Podstawowe typy Kotlin:

- liczby całkowite: `Byte`, `Short`, `Int`, `Long`;
- bez znakowe liczby całkowite: `UByte`, `UShort`, `UInt`, `ULong`;
- liczby zmiennoprzecinkowe: `Float`, `Double`;
- typ logiczny: `Boolean`;
- znaki i łańcuchy znaków: `Char`, `String`;
- tablice: `Array<T>`, `IntArray`, `LongArray` i inne.

Ważne są też typy systemowe:

- `Any` — typ bazowy dla wszystkich typów non-null;
- `Unit` — funkcja nie zwraca znaczącej wartości;
- `Nothing` — funkcja nigdy nie kończy się normalnie;
- typy nullable oznacza się znakiem `?`, na przykład `Int?`.

Na JVM typy takie jak `Int` mogą być kompilowane do prymitywów Javy. Typy
nullable i typy generyczne zwykle wymagają boxingu.

- W Kotlin nie ma niejawnego rozszerzania typów liczbowych: `Int` nie zamienia
  się automatycznie w `Long`:

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**Krótko:** w Kotlin nie ma osobnej składni dla typów prymitywnych i obiektowych,
ale kompilator JVM używa prymitywów tam, gdzie jest to możliwe.

</details>

<details>
<summary>3. Jaka jest różnica między val i var w Kotlin?</summary>

#### Kotlin

`val` zabrania ponownego przypisania, a `var` na nie pozwala:

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Błąd kompilacji
```

`val` nie sprawia, że sam obiekt jest niezmienny — niezmienna jest tylko
referencja:

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Dozwolone
```

Dla właściwości klasy `val` ma tylko getter, a `var` — getter i setter.

**Krótko:** domyślnie używaj `val`, a `var` tylko wtedy, gdy wartość rzeczywiście
trzeba zmieniać.

</details>

<details>
<summary>4. Czym jest wnioskowanie typów (type inference) w Kotlin?</summary>

#### Kotlin

Wnioskowanie typów to zdolność kompilatora do określenia typu na podstawie
wyrażenia lub kontekstu bez jego jawnego podawania. Kotlin nadal pozostaje
językiem statycznie typowanym.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it — String
```

Typ jawny warto podać, gdy:

- jest częścią publicznego API;
- poprawia czytelność złożonego wyrażenia;
- ma różnić się od typu konkretnej implementacji.

```kotlin
val users: List<User> = mutableListOf()
```

**Krótko:** kompilator automatycznie wyprowadza oczywiste typy; jawne typy są
potrzebne dla kontraktów i czytelności.

</details>

<details>
<summary>5. Czym są funkcje rozszerzające (extension functions)?</summary>

#### Kotlin

Funkcja rozszerzająca dodaje wygodną składnię wywołania dla istniejącego typu bez
zmiany samej klasy i bez dziedziczenia.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` to typ odbiorcy. Wewnątrz funkcji jego obiekt jest dostępny przez
`this`.

Ważne ograniczenia:

- rozszerzenie nie ma dostępu do `private` i `protected` członków klasy;
- metoda klasy ma pierwszeństwo przed rozszerzeniem o takiej samej sygnaturze;
- rozszerzenie wybierane jest statycznie według zadeklarowanego typu zmiennej.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

Na JVM takie rozszerzenie zwykle kompiluje się do funkcji statycznej, do której
obiekt przekazywany jest jako pierwszy argument.

**Krótko:** funkcje rozszerzające poprawiają czytelność API, ale nie dodają
rzeczywistych metod klasy i nie obsługują polimorficznego nadpisywania.

</details>

<details>
<summary>6. Czym jest data class w Kotlin?</summary>

#### Kotlin

`data class` jest przeznaczona dla modeli, których główną rolą jest
przechowywanie danych. Kompilator generuje:

- `equals()` i `hashCode()`;
- `toString()`;
- `copy()`;
- `componentN()` do destrukturyzacji.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Ważne zasady:

- konstruktor główny musi zawierać co najmniej jeden parametr `val` lub `var`;
- `data class` nie może być `abstract`, `open`, `sealed` ani `inner`;
- w wygenerowanych metodach biorą udział tylko właściwości konstruktora
  głównego;
- `copy()` wykonuje kopię płytką, a nie głęboką.

Właściwość w ciele klasy nie jest uwzględniana w `equals()`, `hashCode()` ani
`copy()`:

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**Krótko:** `data class` zmniejsza ilość kodu szablonowego dla DTO, stanu UI i
value objects. Złożoną logikę biznesową lepiej trzymać w serwisach domenowych
albo zwykłych klasach.

</details>

<details>
<summary>7. Czym jest companion object?</summary>

#### Kotlin

`companion object` to pojedynczy obiekt powiązany z klasą. Jego członków można
wywoływać przez nazwę klasy:

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

To nie jest Java `static`: companion object ma własny typ, może mieć nazwę,
implementować interfejsy i mieć dostęp do `private` członków klasy. W klasie może
być tylko jeden companion object.

Do wygodnego wywoływania funkcji z Javy używa się `@JvmStatic`:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Bez adnotacji Java wywołuje `Config.Companion.default()`, a z nią —
`Config.default()`.

**Krótko:** companion object nadaje się do metod fabrycznych i stałych
powiązanych z klasą. Nie warto przechowywać w nim globalnego zmiennego stanu.

</details>

<details>
<summary>8. Jak tworzy się klasy w Kotlin?</summary>

#### Kotlin

Klasę deklaruje się słowem kluczowym `class`. Konstruktor główny zapisuje się w
nagłówku klasy:

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

- `val` i `var` w konstruktorze tworzą właściwości klasy.
- `init` wykonuje się podczas tworzenia obiektu i nadaje się do sprawdzania
  inwariantów.
- Wartości domyślne pozwalają uniknąć przeciążonych konstruktorów.
- Obiekt tworzy się bez słowa kluczowego `new`.

Klasy i ich metody są domyślnie `final`. Do dziedziczenia potrzebne jest `open`:

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**Krótko:** Kotlin pozwala deklarować konstruktor, właściwości i wartości
domyślne bezpośrednio w nagłówku klasy; dziedziczenie trzeba dopuścić jawnie.

</details>

<details>
<summary>9. Wyjaśnij konstruktory główne i wtórne.</summary>

#### Kotlin

Konstruktor główny deklaruje się w nagłówku klasy:

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Jego parametry stają się właściwościami tylko wtedy, gdy mają `val` albo `var`.
Ciało konstruktora głównego definiuje się przez `init`:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

Konstruktor wtórny deklaruje się w ciele klasy przez `constructor`. Jeśli istnieje
konstruktor główny, konstruktor wtórny musi delegować do niego wywołanie przez
`this(...)`:

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Właściwości i bloki `init` wykonują się w kolejności deklaracji, a ciało
konstruktora wtórnego — po nich.

Jeśli różnica dotyczy tylko wartości domyślnej, osobny konstruktor nie jest
potrzebny:

```kotlin
class User(val name: String, val id: Long = 0)
```

**Krótko:** konstruktor główny jest podstawowym wariantem. Konstruktory wtórne są
potrzebne głównie dla zgodności z Javą albo wymagań frameworka; zwykle wystarczą
parametry domyślne albo metoda fabryczna.

</details>

<details>
<summary>10. Jak działa dziedziczenie w Kotlin?</summary>

#### Kotlin

Klasy i ich członkowie są w Kotlin domyślnie `final`. Do dziedziczenia potrzebne
jest `open`, a do nadpisania — `override`:

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

Nadpisywać można także właściwości:

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Ważne niuanse:

- klasa może dziedziczyć tylko po jednej klasie, ale implementować wiele
  interfejsów;
- członkowie `abstract` są już otwarci i nie wymagają `open`;
- nadpisany członek pozostaje otwarty, jeśli nie podamy `final override`;
- do implementacji rodzica odwołujemy się przez `super`.

**Krótko:** dziedziczenie w Kotlin trzeba dopuścić jawnie. Do kontraktów lepiej
używać interfejsów, a kompozycję zwykle warto rozważyć wcześniej niż tworzenie
głębokiej hierarchii klas.

</details>

<details>
<summary>11. Jak zaimplementować singleton w Kotlin?</summary>

#### Kotlin

Standardowy sposób implementacji singletona w Kotlin to deklaracja `object`:

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

Na JVM obiekt tworzony jest przy pierwszym dostępie, a jego inicjalizacja jest
bezpieczna wątkowo. `object` może zawierać właściwości, funkcje i bloki `init`,
dziedziczyć po klasie oraz implementować interfejsy. Nie można wywołać jego
konstruktora.

`object` nadaje się do stałych, bezstanowych strategii i przypadków w hierarchii
sealed. Dla serwisów z zależnościami lepiej używać zakresu singleton w
kontenerze DI.

Zmienny stan w `object` jest globalny, utrudnia testy i wymaga synchronizacji. W
Androidzie nie wolno przechowywać w nim `Activity` ani `View`, ponieważ może to
spowodować wyciek pamięci.

**Krótko:** `object` to prosty singleton bez parametrów konstruktora. Dla
serwisów biznesowych z zależnościami i kontrolowanym cyklem życia lepsze jest DI.

</details>

<details>
<summary>12. Do czego używa się typu Unit?</summary>

#### Kotlin

`Unit` oznacza, że funkcja kończy się bez znaczącego wyniku:

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

W zwykłej funkcji `: Unit` można pominąć:

```kotlin
fun log(message: String) {
    println(message)
}
```

W przeciwieństwie do Java `void`, `Unit` jest pełnoprawnym typem z jedyną
wartością `Unit`. Używa się go w typach funkcji i API generycznych:

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

Nie należy mylić go z `Nothing`: `Unit` oznacza normalne zakończenie bez wyniku,
a `Nothing` — że funkcja nigdy nie kończy się normalnie:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**Krótko:** `Unit` to typ dla funkcji bez znaczącego wyniku. Jawnie zapisuje się
go zwykle w typach funkcji callback, na przykład `() -> Unit`.

</details>

<details>
<summary>13. Czym jest smart cast?</summary>

#### Kotlin

Smart cast to automatyczne rzutowanie do bardziej konkretnego typu, gdy
kompilator może udowodnić jego bezpieczeństwo:

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value ma typ String
    }
}
```

Mechanizm działa po sprawdzeniach `is`, `!is`, `null` i z uwzględnieniem
przepływu wykonania:

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value ma typ String
}
```

Smart cast nie działa, jeśli wartość może zmienić się między sprawdzeniem a
użyciem. Typowy przypadek to zmienna właściwość albo custom getter:

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

**Krótko:** smart cast usuwa potrzebę jawnego `as` po sprawdzeniu typu albo
`null`, ale tylko wtedy, gdy kompilator gwarantuje, że sprawdzona wartość się nie
zmieni.

</details>

<details>
<summary>14. Jak Kotlin zapewnia null safety?</summary>

#### Kotlin

W Kotlin typy nullable i non-null są rozdzielone na poziomie systemu typów:

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

Do pracy z wartościami nullable używa się:

```kotlin
val nullableLength: Int? = optionalName?.length // safe call
val length: Int = optionalName?.length ?: 0     // Elvis operator
```

Po sprawdzeniu `null` kompilator może wykonać smart cast:

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

Operator `!!` zamienia wartość nullable na non-null, ale rzuca
`NullPointerException`, jeśli wartość jest równa `null`:

```kotlin
val length = optionalName!!.length
```

Główne słabe miejsce to typy platformowe Javy: bez adnotacji nullability
kompilator nie wie, czy API Javy może zwrócić `null`. Takie wartości trzeba
sprawdzać na granicy Java/Kotlin.

**Krótko:** `T` nie dopuszcza `null`, a `T?` dopuszcza. Używaj `?.`, `?:` i
sprawdzeń; `!!` zostawiaj tylko dla przypadków z udowodnioną gwarancją non-null.

</details>

<details>
<summary>15. Czym jest operator Elvis (?:)?</summary>

#### Kotlin

Operator Elvis `?:` zwraca lewy operand, jeśli nie jest `null`; w przeciwnym
razie oblicza i zwraca prawy:

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

Często używa się go razem z safe call:

```kotlin
val nameLength = user?.name?.length ?: 0
```

Ponieważ `return` i `throw` w Kotlin są wyrażeniami, mogą też znajdować się po
prawej stronie:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**Krótko:** `?:` definiuje fallback dla `null`. Nie maskuj nim błędów, jeśli
`null` oznacza niepoprawny stan — wtedy lepiej jawnie zakończyć wykonanie.

</details>

<details>
<summary>16. Co robi operator !! i dlaczego jest niebezpieczny?</summary>

#### Kotlin

Operator `!!` wymusza konwersję `T?` na `T`. Jeśli wartość jest równa `null`,
powstaje `NullPointerException`:

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Bezpieczniejsze warianty zależą od oczekiwanego zachowania:

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

`requireNotNull` i `checkNotNull` również kończą wykonanie błędem, ale jawnie
opisują naruszony kontrakt. Częste używanie `!!` zwykle sygnalizuje niepoprawny
model stanu nullable albo problemy z cyklem życia.

**Krótko:** używaj `!!` tylko wtedy, gdy non-null jest gwarantowany kontraktem,
ale kompilator nie może tego udowodnić. W pozostałych przypadkach obsługuj
`null` jawnie.

</details>

<details>
<summary>17. Jak działają parametry domyślne i nazwane?</summary>

#### Kotlin

Parametr domyślny jest używany, jeśli odpowiadający mu argument nie został
przekazany:

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Argumenty nazwane poprawiają czytelność, szczególnie przy kilku parametrach tego
samego typu albo `Boolean`:

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java nie obsługuje parametrów domyślnych Kotlin ani argumentów nazwanych. Dla API
używanego z Javy można wygenerować przeciążenia przez `@JvmOverloads`:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Nazwy parametrów funkcji publicznej są częścią API Kotlin: ich zmiana może
zepsuć wywołania z argumentami nazwanymi. Jeśli parametrów jest zbyt dużo,
lepiej stworzyć osobną klasę konfiguracyjną.

**Krótko:** wartości domyślne zastępują większość przeciążeń, a argumenty
nazwane czynią wywołanie czytelniejszym. Dla Javy uwzględniaj `@JvmOverloads`.

</details>

<details>
<summary>18. Czym są deklaracje destrukturyzujące?</summary>

#### Kotlin

Deklaracja destrukturyzująca rozkłada obiekt na zmienne przez funkcje
`componentN()`:

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

Kod równoważny:

```kotlin
val id = user.component1()
val name = user.component2()
```

Dla `data class` te funkcje generowane są według kolejności właściwości
konstruktora głównego. Destrukturyzacja działa też w pętlach i z `Map.Entry`:

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Niepotrzebny komponent można pominąć przez `_`:

```kotlin
val (_, name) = user
```

Dla zwykłej klasy można zdefiniować własne `operator fun componentN()`. Jednak
destrukturyzacja zależy od kolejności komponentów, więc dla dużych modeli może
pogarszać czytelność.

**Krótko:** destrukturyzacja to wygodna składnia nad `componentN()` dla małych
modeli i par wartości. Kolejność komponentów jest częścią kontraktu.

</details>

<details>
<summary>19. Czym jest type alias i kiedy go używać?</summary>

#### Kotlin

`typealias` tworzy krótszą albo bardziej zrozumiałą nazwę dla istniejącego typu:

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Ważne: alias nie tworzy nowego typu i nie dodaje bezpieczeństwa typów:

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Poprawne, ponieważ oba typy to Long
```

Jeśli kompilator ma rozróżniać wartości, potrzebna jest `value class`:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**Krótko:** `typealias` używa się dla czytelności złożonych typów generycznych i
funkcyjnych. Dla osobnego typu domenowego używaj `value class`.

</details>

<details>
<summary>20. Jaka jest różnica między vararg a tablicą?</summary>

#### Kotlin

Tablica to obiekt o stałej liczbie elementów. `vararg` to parametr funkcji, który
pozwala przekazać zmienną liczbę argumentów:

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

Wewnątrz funkcji `names` jest tablicą. Gotową tablicę przekazuje się przez
operator spread `*`:

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

Jeśli po `vararg` występuje inny parametr, przekazuje się go po nazwie:

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

W funkcji może być tylko jeden `vararg`. Dla `vararg Int` kompilator używa
`IntArray`. Spread może tworzyć kopię tablicy, co ma znaczenie w kodzie
krytycznym wydajnościowo.

**Krótko:** `Array<T>` to struktura danych, a `vararg` to sposób przyjęcia
dowolnej liczby argumentów. Tablicę do `vararg` przekazuje się jako `*array`.

</details>
