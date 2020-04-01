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

<details>
<summary>21. Jak używa się wyrażeń lambda w Kotlin?</summary>

#### Kotlin

Lambda to funkcja anonimowa, którą można przechowywać w zmiennej, przekazywać
jako argument i zwracać z funkcji:

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

Ostatnie wyrażenie jest wynikiem lambdy. Jeśli parametr jest jeden, można
oznaczyć go przez `it`:

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

Jeśli lambda jest ostatnim argumentem, można wynieść ją poza nawiasy okrągłe:

```kotlin
button.setOnClickListener { println("Clicked") }
```

Lambda może przechwytywać zmienne z zewnętrznego zakresu:

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

Jeśli potrzebna funkcja już istnieje, można użyć referencji `::`:

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**Krótko:** lambdy przekazują zachowanie do `map`, `filter`, callbacków i DSL.
Długie albo zagnieżdżone lambdy lepiej wynosić do nazwanych funkcji, a
przechwytywanie zmiennego stanu kontrolować, szczególnie w kodzie współbieżnym.

</details>

<details>
<summary>22. Jak działają kolekcje (List, Set, Map) w Kotlin?</summary>

#### Kotlin

Podstawowe kolekcje Kotlin:

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- `List` zachowuje kolejność, dopuszcza duplikaty i dostęp po indeksie.
- `Set` przechowuje unikalne elementy; unikalność określa się przez `equals()` i
  `hashCode()`.
- `Map` przechowuje pary klucz-wartość z unikalnymi kluczami. `map[key]` zwraca
  wartość nullable, dlatego do sprawdzania obecności klucza służy
  `containsKey()`.

Kolekcje mają interfejsy read-only (`List`, `Set`, `Map`) oraz mutable
(`MutableList`, `MutableSet`, `MutableMap`):

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Read-only nie oznacza kolekcji niezmiennej. Inna referencja może zmienić ten sam
obiekt:

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Operacje `filter`, `map`, `sorted` i inne zwracają nowe kolekcje:

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Łańcuchy operacji na `Iterable` mogą tworzyć kolekcje pośrednie. Dla długich
łańcuchów warto rozważyć `Sequence`, ale nie należy używać go bez potrzeby.

**Krótko:** na zewnątrz lepiej zwracać interfejsy read-only, a kolekcje mutable
zostawiać jako szczegół implementacji. Jeśli potrzebna jest prawdziwa
niezmienność, zrób kopię albo użyj kolekcji immutable.

</details>

<details>
<summary>23. Jak iterować po kolekcjach w Kotlin?</summary>

#### Kotlin

Do zwykłego przejścia używa się `for`:

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Do krótkiej akcji na każdym elemencie nadaje się `forEach`:

```kotlin
names.forEach { println(it) }
```

Jeśli potrzebny jest indeks:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

`Map` wygodnie przechodzi się przez destrukturyzację:

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Do transformacji albo agregacji używa się odpowiedniego operatora:

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Dla `break`, `continue` i złożonego control flow lepszy jest `for`. Do
wyszukiwania warto używać `firstOrNull`, `find`, `any` albo `none`, a nie
ręcznej pętli.

**Krótko:** `for` jest do sterowania pętlą, `forEach` do krótkich działań
ubocznych, a `map/filter` do tworzenia wyniku. Wybieraj operator według intencji
kodu.

</details>

<details>
<summary>24. Co się stanie, jeśli zmienimy listę utworzoną przez listOf()?</summary>

#### Kotlin

`listOf()` zwraca `List<T>` — interfejs read-only bez metod `add`, `remove` i
`set`:

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Błąd kompilacji
```

Jednak `List` nie gwarantuje pełnej niezmienności. Inna referencja mutable może
zmienić tę samą listę:

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Rzutowanie wyniku `listOf()` na `MutableList` jest niebezpieczne: kod zależy od
konkretnej implementacji i może upaść w czasie wykonania.

Jeśli potrzebne są zmiany, utwórz listę mutable albo kopię:

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

Dla stylu niezmiennego utwórz nową listę:

```kotlin
val updated = names + "Oksana"
```

**Krótko:** `listOf()` nie da się zmieniać przez API `List`. Do zmian używaj
`mutableListOf()` albo `toMutableList()`, a nie niebezpiecznego castu.

</details>

<details>
<summary>25. Co się stanie przy dostępie do indeksu poza zakresem listy?</summary>

#### Kotlin

Dostęp przez `list[index]` wywołuje `get(index)`. Jeśli indeks jest ujemny albo
większy bądź równy `size`, powstaje `IndexOutOfBoundsException`:

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Bezpieczniejsze warianty:

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

Dla potencjalnie pustej listy istnieją osobne bezpieczne funkcje:

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**Krótko:** używaj `list[index]`, gdy poprawność indeksu jest inwariantem. Dla
indeksów zewnętrznych albo opcjonalnych używaj `getOrNull`, `getOrElse` albo
sprawdzenia przez `indices`.

</details>

<details>
<summary>26. Jak działa współpraca Kotlin z Javą (interop)?</summary>

#### Kotlin

Kotlin/JVM kompiluje się do bajtkodu JVM, dlatego Kotlin i Java mogą bezpośrednio
wywoływać swoje klasy i metody:

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

Główne ryzyko to typy platformowe z Javy, dla których nullability jest nieznane.
Na granicy języków trzeba używać `@Nullable`/`@NotNull`, sprawdzać wartości i nie
przekazywać typów platformowych do wewnętrznych warstw aplikacji.

Do wygodnego API skierowanego do Javy używa się:

- `@JvmStatic` — metoda statyczna;
- `@JvmOverloads` — przeciążenia dla parametrów domyślnych;
- `@JvmField` — pole bez getter/setter;
- `@JvmName` — inna nazwa JVM;
- `@Throws` — deklaracja checked exceptions dla Javy.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Interfejsy SAM z Javy można przekazywać jako lambdy:

```kotlin
executor.execute { println("Done") }
```

**Krótko:** interop pozwala na stopniową migrację między Javą i Kotlin. Najwięcej
uwagi wymagają nullability, kolekcje mutable, parametry domyślne i wygoda API dla
kodu Java.

</details>

<details>
<summary>27. Czy można używać adnotacji Javy w Kotlin?</summary>

#### Kotlin

Tak. Adnotacje Javy można stosować do klas Kotlin i ich członków:

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

Właściwość Kotlin może być reprezentowana na JVM jako pole, getter, setter i
parametr konstruktora. Aby adnotacja trafiła we właściwe miejsce, używa się
use-site target:

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Główne warianty:

- `@field:` — backing field;
- `@get:` / `@set:` — getter albo setter;
- `@param:` — parametr konstruktora;
- `@property:` — właściwość Kotlin, która nie jest osobnym elementem Javy.

Adnotacje nullability z Javy pomagają Kotlin określić typ wyniku API Javy:

```java
@NotNull
String getName();
```

Bez poprawnej adnotacji wynik może pozostać typem platformowym. Dla frameworków
reflection albo validation trzeba sprawdzać, który element JVM odczytują.

**Krótko:** adnotacje Javy są obsługiwane bezpośrednio. Główny niuans to poprawny
wybór `@field:`, `@get:` albo `@param:` dla konkretnego frameworka.

</details>

<details>
<summary>28. Czym jest Kotlin REPL?</summary>

#### Kotlin

REPL oznacza Read-Eval-Print Loop: środowisko czyta wprowadzony kod, wykonuje go,
pokazuje wynik i czeka na kolejne polecenie.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

REPL nadaje się do szybkiego sprawdzania składni, biblioteki standardowej i
małych wyrażeń bez tworzenia projektu. Można uruchomić go przez Kotlin CLI; do
dłuższych eksperymentów wygodniejsze są scratch files albo Kotlin Playground.

REPL nie odtwarza Android lifecycle, DI, konfiguracji Gradle ani rzeczywistego
środowiska współbieżnego i nie zastępuje testów automatycznych.

**Krótko:** Kotlin REPL to interaktywna konsola do lokalnych eksperymentów z
językiem. Wyniki ważne dla aplikacji trzeba potwierdzać testami w rzeczywistym
środowisku.

</details>

<details>
<summary>29. Czym jest Kotlin script (.kts)?</summary>

#### Kotlin

Kotlin script to plik `.kts`, w którym można wykonywać instrukcje najwyższego
poziomu bez `fun main()`:

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

`.kt` jest zwykłym plikiem źródłowym programu albo biblioteki, a `.kts` jest
wykonywany przez środowisko skryptowe. Najczęstszy przykład to Gradle Kotlin DSL:

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

Pliki `build.gradle.kts` i `settings.gradle.kts` są wykonywane przez Gradle jako
skrypty konfiguracyjne. Samodzielne skrypty można stosować do niewielkiej
automatyzacji, ale ich model zależności i API zależą od scripting host.

**Krótko:** `.kts` nadaje się do konfiguracji i krótkiej automatyzacji. Logikę,
która rośnie, ma zależności i wymaga testów, lepiej wynieść do zwykłego modułu
Kotlin albo aplikacji CLI.

</details>

<details>
<summary>30. Czym jest Kotlin style guide i dlaczego warto go przestrzegać?</summary>

#### Kotlin

Kotlin style guide określa zasady formatowania, nazewnictwa, struktury plików
oraz organizacji importów. Sprawia, że kod zespołu jest jednolity i
przewidywalny.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Praktyczne korzyści:

- mniej sporów o formatowanie podczas code review;
- szybsze czytanie kodu i wdrażanie nowych programistów;
- automatyczna kontrola przez formatter IDE, `ktlint` albo `detekt` w CI.

Zespołowe odstępstwa od oficjalnych zasad trzeba dokumentować i jednakowo
konfigurować w IDE oraz CI. Style guide reguluje wygląd kodu, ale nie zastępuje
reguł architektonicznych.

**Krótko:** style guide zmniejsza obciążenie poznawcze i przenosi sprawdzanie
formatowania z ludzi na narzędzia automatyczne.

</details>

<details>
<summary>31. Czym są idiomy Kotlin?</summary>

#### Kotlin

Idiomy Kotlin to utrwalone sposoby używania możliwości języka tak, aby kod jasno
przekazywał intencję.

Typowe przykłady:

- `val` domyślnie, `var` tylko dla koniecznej mutacji;
- typy nullable, `?.` i `?:` zamiast `!!`;
- `data class` dla modeli danych;
- typy `sealed` dla ograniczonego zbioru stanów;
- `map`, `filter`, `associateBy` zamiast ręcznych pętli do transformacji;
- guard clauses dla zmniejszenia zagnieżdżenia.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Scope functions, lambdy i operator overloading są właściwe tylko wtedy, gdy
poprawiają czytelność. Samo używanie większej liczby możliwości języka nie czyni
kodu idiomatycznym.

**Krótko:** idiomatyczny Kotlin to bezpieczny i zrozumiały kod, który używa
możliwości języka zgodnie z przeznaczeniem, a nie po to, by demonstrować
składnię.

</details>

<details>
<summary>32. Jak Kotlin jest używany w backendzie i dlaczego wybiera się go zamiast Javy?</summary>

#### Kotlin

Kotlin jest używany do backendów JVM: REST/gRPC API, mikroserwisów, workerów i
systemów event-driven. Współpracuje ze Spring Boot, Ktor i bibliotekami Javy.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Główne przewagi względem Javy:

- null safety;
- mniej kodu szablonowego dzięki `data class`, parametrom domyślnym i funkcjom
  rozszerzającym;
- coroutines dla nieblokującego I/O;
- stopniowa migracja i pełna dostępność ekosystemu Javy.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Kompromisy: wolniejsza kompilacja, dodatkowe ustawienia compiler/Gradle dla
niektórych frameworków Javy oraz konieczność nauczenia zespołu idiomatycznego
Kotlin i structured concurrency.

**Krótko:** Kotlin warto wybierać, gdy zespół chce bezpieczniejszego i bardziej
zwięzłego kodu JVM bez rezygnacji z ekosystemu Javy oraz jest gotowy zaakceptować
bardziej złożony toolchain.

</details>

<details>
<summary>33. Czym są funkcje wyższego rzędu (higher-order functions)?</summary>

#### Kotlin

Funkcja wyższego rzędu przyjmuje funkcję jako argument albo zwraca ją jako wynik.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` to funkcja bez parametrów i bez znaczącego wyniku. Przykład
zwracania funkcji:

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

Typowe przykłady z biblioteki standardowej:

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Takie funkcje używane są w kolekcjach, callbackach i DSL. Lambdy mogą tworzyć
obiekty i przechwytywać zewnętrzny stan; `inline` często usuwa ten narzut dla
małych funkcji wyższego rzędu.

**Krótko:** funkcje wyższego rzędu pozwalają przekazywać zachowanie jako wartość.
Złożone albo zagnieżdżone lambdy lepiej wynosić do nazwanych funkcji.

</details>

<details>
<summary>34. Czym są funkcje inline i kiedy warto ich używać?</summary>

#### Kotlin

`inline` prosi kompilator, aby wstawił ciało funkcji i jej parametry lambda w
miejsce wywołania. Główny cel to zmniejszenie narzutu funkcji wyższego rzędu:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` pozwala też używać `reified` type parameters:

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Dodatkowe modyfikatory parametrów lambda:

- `noinline` — nie inline'uje lambdy, więc można ją przechowywać albo przekazać
  dalej;
- `crossinline` — inline'uje, ale zabrania non-local `return`.

`inline` jest właściwy dla małych funkcji wyższego rzędu, `reified` generics i
zmierzonych hot paths. Dla dużych funkcji może zwiększyć bytecode. Zmiany w
publicznych funkcjach inline wymagają też uwagi względem zgodności binarnej.

**Krótko:** używaj `inline` nie jako uniwersalnej optymalizacji, lecz dla
lambda-overhead, `reified` albo potrzebnej semantyki non-local return.

</details>

<details>
<summary>35. Co oznacza słowo kluczowe reified i gdzie się je stosuje?</summary>

#### Kotlin

Z powodu type erasure zwykłego parametru generycznego `T` nie można użyć do
sprawdzenia typu ani pobrania `T::class`:

```kotlin
// fun <T> isOfType(value: Any) = value is T // Błąd kompilacji
```

`reified` pozwala na to wewnątrz funkcji `inline`, ponieważ kompilator podstawia
konkretny typ w miejscu wywołania:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

Można też uzyskać informację o klasie:

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

Przykład z biblioteki standardowej:

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

Ograniczenie: `reified` jest dostępne tylko dla parametrów typu funkcji inline i
nie zwraca informacji o zagnieżdżonych argumentach generycznych, takich jak
`List<String>`. Dla API non-inline trzeba jawnie przekazywać `KClass`, `Class`
albo serializer.

**Krótko:** `reified` daje funkcji inline dostęp do konkretnego `T` dla `is`,
`T::class`, reflection i podobnych API, ale nie usuwa type erasure całkowicie.

</details>

<details>
<summary>36. Czym są klasy sealed i interfejsy sealed?</summary>

#### Kotlin

`sealed class` i `sealed interface` definiują kontrolowaną hierarchię
bezpośrednich podtypów. Nadają się do stanów, wyników i komend:

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

Kompilator sprawdza, czy `when` jako wyrażenie obsługuje wszystkie warianty:

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Bezpośrednie podtypy muszą być zadeklarowane w tym samym module i pakiecie.
`sealed class` może mieć konstruktor, stan i wspólną implementację.
`sealed interface` nie ma konstruktora, ale klasa może implementować kilka
interfejsów.

W odróżnieniu od `enum`, każdy wariant sealed może mieć własny zestaw danych.
Jeśli hierarchię mają rozszerzać zewnętrzne moduły, `sealed` nie jest właściwe.
`else` w `when` lepiej nie dodawać bez potrzeby, aby kompilator znajdował nowe
przypadki.

**Krótko:** typy sealed modelują zamknięty zestaw wariantów z różnymi danymi i
dają compile-time sprawdzenie kompletności `when`.

</details>

<details>
<summary>37. Czym jest object expression i kiedy się go używa?</summary>

#### Kotlin

`object expression` tworzy obiekt anonimowy w miejscu wykonania. Zwykle używa
się go do jednorazowej implementacji interfejsu albo klasy:

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

Można też dziedziczyć po klasie:

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

Obiekt anonimowy bez supertype jest wygodny tylko lokalnie albo w `private` API:

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Jego anonimowego typu nie da się eksportować jako stabilnego typu publicznego:
na zewnątrz będzie widoczny zadeklarowany supertype albo `Any`. Object expression
tworzony jest za każdym razem podczas wykonania wyrażenia, natomiast object
declaration jest nazwanym singletonem.

Taki obiekt może przechwytywać zmienne zewnętrzne. Jeśli implementacja rośnie
albo się powtarza, lepiej wynieść ją do nazwanej klasy.

**Krótko:** object expression nadaje się do krótkiej lokalnej implementacji,
listenera albo test stub. Dla logiki publicznej lub wielokrotnego użytku
potrzebny jest nazwany typ.

</details>

<details>
<summary>38. Jaka jest różnica między enum class a sealed class?</summary>

#### Kotlin

`enum class` zawiera stały zestaw wartości singleton jednego typu:

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

`sealed class` albo `sealed interface` definiuje zamkniętą hierarchię podtypów.
Każdy wariant może mieć własne dane:

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

Dla obu wariantów kompilator sprawdza kompletność `when`:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

`enum` ma standardowe `entries`, `name` i `ordinal`. Typ sealed nie ma gotowej
listy instancji, ale pozwala tworzyć wiele obiektów jednego podtypu z różnymi
danymi.

**Krótko:** `enum` jest dla prostych stałych ze wspólną strukturą. Typ sealed
jest dla zamkniętego zestawu stanów albo wyników z różnymi danymi.

</details>

<details>
<summary>39. Jaka jest różnica między lateinit a inicjalizacją lazy?</summary>

#### Kotlin

Oba mechanizmy odkładają inicjalizację, ale działają inaczej.

`lateinit` pozwala przypisać non-null `var` po utworzeniu obiektu:

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Odczyt przed przypisaniem rzuca `UninitializedPropertyAccessException`.
`lateinit` działa tylko z non-null `var` typu referencyjnego, nie gwarantuje
thread safety i pozwala na ponowne przypisanie.

Można sprawdzić inicjalizację:

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` oblicza `val` przy pierwszym odczycie i cache'uje udany wynik:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

Domyślnie `lazy` jest synchronizowany. Tryby `PUBLICATION` i `NONE` zmieniają
gwarancje; `NONE` jest bezpieczny tylko przy dostępie z jednego wątku.

- Kod zewnętrzny albo DI przypisuje wartość później — `lateinit`.
- Własny initializer ma wykonać się na żądanie — `lazy`.
- Brak wartości jest poprawnym stanem — typ nullable.

**Krótko:** `lateinit` to odłożone zewnętrzne przypisanie zmiennej właściwości;
`lazy` to jednorazowe odłożone obliczenie niezmiennej właściwości.

</details>

<details>
<summary>40. Czym jest typ Nothing i jak wpływa na sterowanie przepływem wykonania?</summary>

#### Kotlin

`Nothing` to typ bez żadnej możliwej wartości. Funkcja z takim wynikiem nigdy
nie kończy się normalnie:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` jest podtypem wszystkich typów, dlatego można go używać w dowolnym
wyrażeniu, w którym wykonanie jest przerywane:

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` i `return` mają typ `Nothing`, dlatego mogą być częścią wyrażenia Elvis:

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Ten typ pomaga kompilatorowi analizować control flow i wykonywać smart cast:

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` ma jedyną możliwą wartość `null`; jest to typ wyrażenia `null`, jeśli
nie ma innego kontekstu. Standardowe `error()` i `TODO()` również zwracają
`Nothing`.

**Krótko:** `Nothing` oznacza ścieżkę, która nie zwraca sterowania. Dzięki temu
kompilator dokładniej określa typy i analizuje dalszy przepływ wykonania.

</details>

<details>
<summary>41. Czy Java może wywoływać funkcje Kotlin z parametrami domyślnymi?</summary>

#### Kotlin

Java nie obsługuje parametrów domyślnych Kotlin. Bez dodatkowych overloads musi
przekazać wszystkie argumenty:

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

`@JvmOverloads` generuje przeciążenia przyjazne dla Javy:

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

Przeciążenia generowane są od prawej do lewej dla kolejnych parametrów z
wartościami domyślnymi. Adnotacja działa także z konstruktorami:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Dla API używanego tylko z Kotlin adnotacja nie jest potrzebna. Duża liczba
parametrów tworzy zbyt wiele overloads — w takim przypadku lepszy jest config
object albo builder.

**Krótko:** bez `@JvmOverloads` Java wywołuje tylko pełną sygnaturę. Dodawaj
adnotację tylko do API, które rzeczywiście są używane z Javy.

</details>

<details>
<summary>42. Czym są funkcje tail-recursive?</summary>

#### Kotlin

Funkcja tail-recursive wywołuje samą siebie jako ostatnią operację. Modyfikator
`tailrec` pozwala kompilatorowi zamienić takie wywołanie na pętlę bez wzrostu
stosu:

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

Ten wariant nie jest tail-recursive, ponieważ po wywołaniu rekurencyjnym
wykonywane jest mnożenie:

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

Optymalizacja działa tylko dla rekursji bezpośredniej i nie działa, jeśli
wywołanie znajduje się w konstrukcji przeszkadzającej transformacji, na przykład
w `try`/`finally`. Jeśli `tailrec` zastosowano niepoprawnie, kompilator ostrzeże,
że optymalizacja nie jest wykonywana.

**Krótko:** `tailrec` służy do czytelnej rekursji bez ryzyka
`StackOverflowError`. Jeśli zwykła pętla jest prostsza, lepiej użyć pętli.

</details>

<details>
<summary>43. Czym są scope functions (let, run, apply, also, with) i kiedy ich używać?</summary>

#### Kotlin

Scope functions wykonują blok w kontekście obiektu. Różnią się dwiema cechami:

- jak dostępny jest obiekt: `this` albo `it`;
- co jest zwracane: wynik lambdy albo sam obiekt.

| Funkcja | Obiekt dostępny jako | Zwraca       |
| ------- | -------------------- | ------------ |
| `let`   | `it`                 | wynik lambdy |
| `run`   | `this`               | wynik lambdy |
| `apply` | `this`               | sam obiekt   |
| `also`  | `it`                 | sam obiekt   |
| `with`  | `this`               | wynik lambdy |

Typowe użycie:

- `let` — transformacja albo blok po safe call;
- `run` — obliczenie wyniku z dostępem przez `this`;
- `apply` — konfiguracja obiektu;
- `also` — działanie uboczne bez zmiany łańcucha;
- `with` — grupowanie operacji na przekazanym obiekcie.

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

`with` nie jest funkcją rozszerzającą: obiekt przekazywany jest jako pierwszy
argument. Głęboko zagnieżdżone scope functions i mieszanie `this`/`it` pogarsza
czytelność; w takim przypadku lepszy jest zwykły kod albo nazwana funkcja.

**Krótko:** wybieraj scope function według receivera (`this`/`it`) i wyniku
(obiekt/wynik lambdy), a nie z przyzwyczajenia.

</details>

<details>
<summary>44. Jaka jest różnica między let, run i apply?</summary>

#### Kotlin

Funkcje różnią się sposobem dostępu do obiektu i wynikiem:

| Funkcja | Receiver w bloku | Zwraca       |
| ------- | ---------------- | ------------ |
| `let`   | `it`             | wynik lambdy |
| `run`   | `this`           | wynik lambdy |
| `apply` | `this`           | sam obiekt   |

`let` — do transformacji albo wykonania po safe call:

```kotlin
val length = name?.let { it.length }
```

`run` — do obliczenia wyniku z kilkoma odwołaniami do obiektu:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`apply` — do konfiguracji ze zwróceniem tego samego obiektu:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

Nie używaj scope function, jeśli zwykłe wywołanie jest czytelniejsze.
Zagnieżdżone bloki z kilkoma `this` albo `it` lepiej zastąpić nazwaną funkcją.

**Krótko:** `let` — `it` i nowy wynik; `run` — `this` i nowy wynik; `apply` —
`this` i początkowy obiekt.

</details>

<details>
<summary>45. Jaka jest różnica między map i flatMap?</summary>

#### Kotlin

`map` przekształca każdy element w jeden wynik:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

Jeśli transformacja zwraca kolekcję, `map` tworzy strukturę zagnieżdżoną:

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` wykonuje transformację i łączy zagnieżdżone kolekcje w jedną:

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

Praktyczny przykład:

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Koncepcyjnie te wyrażenia są równoważne:

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**Krótko:** `map`: `A -> B`. `flatMap`: `A -> Iterable<B>` z późniejszym
połączeniem wszystkich wyników w płaską kolekcję.

</details>

<details>
<summary>46. Czym jest Sequence i kiedy go używać?</summary>

#### Kotlin

`Sequence` to leniwa synchroniczna sekwencja. Operatory pośrednie nie przetwarzają
danych od razu, lecz budują pipeline:

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Obliczenia uruchamia terminal operation: `toList`, `first`, `count`, `fold`,
`any` i podobne. Elementy przechodzą przez pipeline pojedynczo, dlatego operacje
short-circuit mogą zatrzymać pracę wcześniej:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

`Sequence` jest właściwy dla dużych zbiorów, długich łańcuchów `map`/`filter` i
operacji short-circuit. Dla małej kolekcji albo jednej transformacji jego narzut
może być większy niż korzyść. Operacje takie jak `sorted` i tak wymagają
zgromadzenia elementów.

`Sequence` nie jest asynchroniczny i nie obsługuje suspension. Do asynchronicznego
strumienia danych używa się `Flow`.

**Krótko:** `Sequence` zmniejsza liczbę kolekcji pośrednich i zbędną pracę w
długich synchronicznych pipeline, ale nie jest automatycznie szybszy od zwykłych
kolekcji.

</details>

<details>
<summary>47. Czym jest leniwe (lazy) przetwarzanie kolekcji?</summary>

#### Kotlin

Leniwe przetwarzanie oznacza, że pipeline jest tylko opisywany, a wykonywany
dopiero po terminal operation. W Kotlin używa się do tego `Sequence`:

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Elementy zwykle przechodzą przez operatory pojedynczo. Pozwala to zakończyć
przetwarzanie wcześniej:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Leniwy pipeline nie cache'uje wyniku: ponowna terminal operation zwykle uruchamia
obliczenia od nowa. Można też pracować z potencjalnie nieskończonymi sekwencjami,
jeśli ograniczy się je przez `take`:

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Operatory leniwe mogą mieć własny narzut, a `sorted` i podobne operacje i tak
gromadzą dane. Do przetwarzania asynchronicznego potrzebny jest `Flow`, nie
`Sequence`.

**Krótko:** lazy processing odkłada pracę do terminal operation i może uniknąć
zbędnych obliczeń. Nie cache'uje wyniku i nie zawsze jest szybsze od eager
collections.

</details>

<details>
<summary>48. Czym są coroutines i czym różnią się od wątków (threads)?</summary>

#### Kotlin

Coroutine to lekka jednostka pracy, która może wstrzymać wykonanie i kontynuować
je później. Dla każdej coroutine nie tworzy się osobnego OS thread:

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Wątek to zasób systemu operacyjnego z własnym stosem. Wiele coroutines może
wykonywać się na niewielkiej liczbie wątków i przechodzić między nimi po
suspension.

```kotlin
delay(1_000)        // Wstrzymuje coroutine
Thread.sleep(1_000) // Blokuje wątek
```

`suspend` samo w sobie nie gwarantuje implementacji nieblokującej. Blocking API
nadal blokuje wątek i powinno być wykonywane w odpowiednim kontekście.

Dispatcher określa wątki wykonania:

- `Dispatchers.Main` — UI;
- `Dispatchers.IO` — blocking I/O;
- `Dispatchers.Default` — praca CPU-bound.

Structured concurrency wiąże coroutines z `CoroutineScope`: operacja rodzica
zarządza cyklem życia, błędami i anulowaniem zadań potomnych.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Coroutines nie usuwają race conditions i nie przyspieszają automatycznie pracy
CPU-bound. Dla ciężkich obliczeń trzeba jawnie wybrać właściwy dispatcher:

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**Krótko:** coroutines to zadania działające nad wątkami, z suspension i
structured concurrency. Są tańsze niż model „jedno zadanie — jeden wątek”, ale
nie zastępują poprawnej pracy z dispatchers i shared state.

</details>

<details>
<summary>49. Czym jest funkcja suspend?</summary>

#### Kotlin

`suspend` oznacza funkcję, która może mieć punkty wstrzymania. Wywołuje się ją z
innej funkcji suspend albo z coroutine:

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` nie tworzy coroutine, nie uruchamia funkcji równolegle i nie czyni
kodu blokującego automatycznie nieblokującym:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

Kompilator przekształca funkcję suspend w state machine z `Continuation`, aby
zachować stan i kontynuować wykonanie po suspension.

Anulowanie jest kooperatywne: standardowe suspension points je sprawdzają, a
długie pętle CPU powinny robić to jawnie:

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**Krótko:** `suspend` pozwala funkcji wstrzymywać i wznawiać coroutine. Nie
gwarantuje implementacji nieblokującej i samodzielnie nie tworzy nowej coroutine.

</details>

<details>
<summary>50. Czym jest CoroutineScope i do czego służy?</summary>

#### Kotlin

`CoroutineScope` jest właścicielem coroutines i zawiera `coroutineContext`.
Zwykle w kontekście znajduje się `Job`, dispatcher oraz dodatkowe elementy:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

`Job` tworzy relacje parent-child i zarządza anulowaniem. Anulowanie scope
anuluje jego coroutines potomne:

```kotlin
scope.cancel()
```

Do pracy równoległej wewnątrz funkcji suspend używa się scope builder:

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` kończy się dopiero po wszystkich zadaniach potomnych. W
Androidzie gotowe `viewModelScope`, `lifecycleScope` i `rememberCoroutineScope()`
mają już określony lifecycle.

Własny scope tworzy się tylko dla obiektu z jawną metodą zakończenia, która
wywołuje `cancel()`. `GlobalScope` nie ma takiego właściciela i narusza
structured concurrency, dlatego w kodzie aplikacji należy go unikać.

**Krótko:** `CoroutineScope` wiąże coroutines ze zrozumiałym lifecycle i zapewnia
ich wspólne anulowanie. Każdy scope powinien mieć właściciela.

</details>

<details>
<summary>51. Jaka jest różnica między launch i async?</summary>

#### Kotlin

Oba builders uruchamiają coroutine potomną w `CoroutineScope`.

`launch` zwraca `Job` i jest używany, gdy osobny wynik nie jest potrzebny:

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` zwraca `Deferred<T>`. Wynik albo błąd pobiera się przez `await()`.
Typowy scenariusz to równoległe wykonanie niezależnych operacji:

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

Domyślnie `async` startuje od razu, a nie dopiero przy `await()`. Błąd coroutine
potomnej anuluje zwykły parent scope niezależnie od buildera; dla `async`
`await()` dodatkowo ponownie rzuca ten błąd. Zachowanie supervision trzeba
konfigurować osobno.

Nie używaj `async`, jeśli nie planujesz wywołać `await()`, i nie uruchamiaj
równolegle operacji, które zależą od siebie.

**Krótko:** `launch` to zadanie bez wartości wyniku; `async` to zadanie z
`Deferred<T>`, którego wynik jest potrzebny przez `await()`.

</details>

<details>
<summary>52. Czym jest runBlocking i kiedy się go używa?</summary>

#### Kotlin

`runBlocking` tworzy `CoroutineScope` i blokuje bieżący wątek, dopóki blok oraz
wszystkie jego coroutines potomne się nie zakończą:

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

To narzędzie graniczne do wywoływania kodu suspend z API synchronicznego. Może
być właściwe w legacy bridge albo krótkim przykładzie. W testach coroutines
należy używać `runTest`.

W kodzie UI `runBlocking` blokuje main thread i może spowodować zawieszenie, ANR
albo deadlock:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Zamiast tego coroutine uruchamia się w scope z odpowiednim lifecycle:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

W funkcji suspend `runBlocking` nie jest potrzebny i tylko blokuje wątek:

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**Krótko:** `runBlocking` to synchroniczny most do kodu suspend. Nie używaj go
wewnątrz coroutines, funkcji suspend ani wątku UI.

</details>

<details>
<summary>53. Czym są dispatchers (Dispatchers.IO, Default, Main)?</summary>

#### Kotlin

`CoroutineDispatcher` planuje wykonanie coroutine na odpowiednich wątkach:

- `Dispatchers.Main` — UI i krótkie operacje na głównym wątku;
- `Dispatchers.IO` — blocking I/O;
- `Dispatchers.Default` — obliczenia CPU-intensive.

Blokującą operację plikową przenosi się na `IO`:

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Ciężkie obliczenie — na `Default`:

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` wstrzymuje bieżącą coroutine, wykonuje blok w podanym kontekście i
zwraca wynik. Zachowuje structured concurrency, cancellation i propagację błędów.

Nie każde wywołanie network albo database potrzebuje `Dispatchers.IO`: suspend
API może już być main-safe. Dispatcher powinna wybierać warstwa, która wie, czy
implementacja blokuje wątek. Dla testowalności dispatcher można przekazać jako
zależność:

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

Nie warto przełączać kontekstu dla drobnej pracy: to też ma koszt.

**Krótko:** `Main` — UI, `IO` — blocking I/O, `Default` — ciężkie obliczenia.
`withContext` zmienia kontekst dla konkretnej operacji bez tworzenia nowej
niezależnej coroutine.

</details>

<details>
<summary>54. Jak anulować coroutines i obsługiwać błędy?</summary>

#### Kotlin

Anulowanie coroutines jest kooperatywne i zarządzane przez `Job`:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Funkcje suspend takie jak `delay()` reagują na anulowanie automatycznie. Kod CPU
powinien regularnie sprawdzać stan:

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

Anulowanie sygnalizowane jest przez `CancellationException`. Jeśli przechwycił ją
szeroki `catch`, trzeba rzucić ją ponownie:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

W zwykłej structured concurrency nieobsłużony błąd coroutine potomnej anuluje
parent i sąsiednie zadania. Oczekiwane błędy obsługuje się lokalnie i zamienia na
wynik domenowy albo UI state:

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

Dla niezależnych zadań używa się `supervisorScope` albo `SupervisorJob`:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

Supervisor nie obsługuje błędu, tylko izoluje siblings.
`CoroutineExceptionHandler` jest przeznaczony do ostatniego poziomu obsługi
uncaught exceptions w root `launch`, na przykład logowania. Błędy `async`
odbiera się przez `await()`.

**Krótko:** anuluj przez `Job`, nie połykaj `CancellationException`, oczekiwane
błędy obsługuj lokalnie, a niezależne zadania izoluj przez supervision.

</details>

<details>
<summary>55. Czym jest structured concurrency?</summary>

#### Kotlin

Structured concurrency oznacza, że każda coroutine ma właściciela i należy do
hierarchii `Job`. Parent czeka na zadania potomne i zarządza ich anulowaniem oraz
błędami.

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

`coroutineScope` nie zakończy się przed swoimi coroutines potomnymi. Anulowanie
callera anuluje je, a nieobsłużony błąd jednego zadania anuluje scope i siblings.

Dla niezależnych zadań używa się supervision:

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

Błąd direct child nie anuluje innych zadań potomnych, ale nadal trzeba go
obsłużyć. `supervisorScope` stosuje się lokalnie, a `SupervisorJob` — w
długowiecznym scope.

`GlobalScope` narusza ten model: caller nie może normalnie poczekać na pracę,
anulować jej ani otrzymać błędu. Własny scope powinien mieć jasny lifecycle i
jawny cleanup.

**Krótko:** structured concurrency nie pozwala, aby praca potomna niepostrzeżenie
przeżyła swojego właściciela. Powiązane zadania używają `coroutineScope`,
niezależne — supervision.

</details>

<details>
<summary>56. Czy coroutines mogą wykonywać się w dowolnym wątku?</summary>

#### Kotlin

Coroutine nie jest przywiązana do własnego wątku. `CoroutineDispatcher` określa,
gdzie wykonywane są jej części:

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

- `Main` — UI;
- `IO` — blocking I/O;
- `Default` — praca CPU-bound.

Na dispatcherze z pulą coroutine po suspension może kontynuować na innym wątku
tego samego dispatchera:

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` tymczasowo zmienia kontekst, a po zakończeniu przywraca kontekst
callera. `Dispatchers.Main` gwarantuje wykonanie na wątku UI; dispatchery z pulą
nie gwarantują konkretnego thread ID.

Zwykły `ThreadLocal` nie przechodzi między wątkami razem z coroutine. W razie
potrzeby jego wartość dodaje się do coroutine context:

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

Custom dispatcher z własnymi wątkami powinien mieć właściciela, który go zamknie.
Wywołanie blocking blokuje wątek niezależnie od tego, że wykonuje się w
coroutine.

**Krótko:** coroutine wykonuje się na wątkach swojego dispatchera i po suspension
może zmienić fizyczny wątek. Przywiązanie do konkretnego wątku musi gwarantować
dispatcher.

</details>

<details>
<summary>57. Czym jest Flow w Kotlin?</summary>

#### Kotlin

`Flow<T>` to asynchroniczny strumień wartości oparty na coroutines. Funkcja
suspend zwraca jedną wartość, a `Flow` — sekwencję wartości w czasie:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Flow tworzy się przez builder i uruchamia terminal operator, na przykład
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

Flow utworzony przez `flow {}` jest cold: producer uruchamia się osobno dla
każdego collectora. `emit()` jest funkcją suspend, więc producer nie wyprzedza
wolnego collectora bez jawnego buffering.

Operatory tworzą pipeline:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` zmienia kontekst operatorów upstream:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` obsługuje tylko błędy upstream; błędów collectora nie przechwytuje.
Anulowanie collectora anuluje zbieranie i producer w ramach structured
concurrency.

**Krótko:** używaj funkcji suspend dla jednego wyniku, a `Flow` dla wartości,
które przychodzą w czasie i wymagają asynchronicznego pipeline.

</details>

<details>
<summary>58. Jaka jest różnica między Flow, StateFlow i SharedFlow?</summary>

#### Kotlin

```text
Flow       -> zwykle cold pipeline
StateFlow  -> hot stream z bieżącym stanem
SharedFlow -> hot broadcast z replay i buffer policy
```

`Flow` utworzony przez `flow {}` uruchamia producer osobno dla każdego collectora
i nie przechowuje bieżącej wartości:

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

`StateFlow` zawsze ma initial value. Nowy collector od razu otrzymuje bieżący
stan, a wartości równe według `equals()` nie są emitowane ponownie:

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

Wersję mutable zostawia się jako `private`; do aktualizacji współbieżnych używa
się `update { }`.

`SharedFlow` wysyła wartości do wszystkich aktywnych collectors i nie musi mieć
bieżącej wartości:

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` określa, ile ostatnich wartości otrzyma nowy collector;
`extraBufferCapacity` i `onBufferOverflow` zarządzają buforem. Przy `replay = 0`
wartość bez subscribers zostaje utracona, dlatego dane krytyczne trzeba
modelować jako state, a nie jednorazowe zdarzenie.

Cold Flow można przekształcić we współdzielony hot flow: `stateIn()` tworzy
`StateFlow`, a `shareIn()` — `SharedFlow`. `SharingStarted` określa, kiedy
wspólny upstream startuje i zatrzymuje się.

**Krótko:** `Flow` to niezależny lazy pipeline, `StateFlow` to bieżący stan, a
`SharedFlow` to współdzielony strumień zdarzeń z konfigurowalnym replay.

</details>

<details>
<summary>59. Jaka jest różnica między Flow i LiveData?</summary>

#### Kotlin

`LiveData` to Android lifecycle-aware holder, a `Flow` to coroutine stream bez
zależności od Androida.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` powiadamia tylko aktywnych observers i ponownie oddaje ostatnią
wartość po aktywacji. Jest wygodne dla legacy View UI, ale nie powinno trafiać do
domain layer.

`Flow` obsługuje coroutine cancellation, błędy i szeroki zestaw operatorów:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

Zwykły `Flow` jest przeważnie cold i nie przechowuje current value. Dla UI state
używa się `StateFlow`. Lifecycle podczas zbierania Flow trzeba uwzględnić jawnie:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

W Compose służy do tego `collectAsStateWithLifecycle()`. LiveData jest
zorientowane na Main thread, a Flow używa coroutine context i `flowOn` dla
upstream.

Podczas migracji typy można konwertować:

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**Krótko:** `LiveData` automatycznie uwzględnia Android lifecycle. `Flow` nie
zależy od Androida i lepiej nadaje się do data/domain; w UI zbiera się go
lifecycle-aware, a bieżący stan przechowuje w `StateFlow`.

</details>

<details>
<summary>60. Jaka jest różnica między cold i hot streams?</summary>

#### Kotlin

Różnica polega na relacji producera z collectors:

```text
cold -> osobny producer uruchamia się dla każdego collectora
hot  -> collectors otrzymują dane ze wspólnego źródła
```

Flow utworzony przez `flow {}` jest cold. Przed `collect` się nie wykonuje, a
każdy collector uruchamia blok osobno:

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

Zatem operacja wykona się dwa razy. Każdy collector ma własny lifecycle i
cancellation.

`StateFlow` i `SharedFlow` są hot: ich obiekt i dane istnieją niezależnie od
konkretnego collectora.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` przechowuje bieżący stan. `SharedFlow` rozsyła wartości aktywnym
collectors i może przechowywać ostatnie wartości przez `replay`.

Cold Flow przekształca się we współdzielony hot flow przez `stateIn()` albo
`shareIn()`. Polityka `SharingStarted` określa, kiedy wspólny upstream jest
aktywny, ale wszyscy collectors i tak używają jednego wykonania.

**Krótko:** cold stream uruchamia producer dla każdego collectora; hot stream ma
wspólne źródło i rozsyła jego wartości wszystkim collectors.

</details>

<details>
<summary>61. Czym jest collectLatest i kiedy go używać?</summary>

#### Kotlin

`collectLatest` to terminal operator, który anuluje obsługę poprzedniej wartości,
gdy pojawia się nowa:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Zdąży wydrukować się tylko 3
}
```

Typowy scenariusz to wyszukiwanie, gdzie poprzednie zapytanie staje się
nieaktualne:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Anulowanie jest kooperatywne. Suspend API na nie reaguje, a kod blocking — nie:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

Nie używaj `collectLatest`, jeśli każda wartość musi być gwarantowanie
obsłużona: płatności, wiadomości, zapisywanie albo analytics events.

`flatMapLatest` anuluje poprzedni inner Flow, a `collectLatest` — blok collectora.

**Krótko:** `collect` — gdy ważna jest każda wartość; `collectLatest` — gdy po
nowej wartości poprzednia obsługa nie jest już potrzebna.

</details>

<details>
<summary>62. Jak przekształcić callback-based API w funkcję suspend?</summary>

#### Kotlin

Jednorazowy callback opakowuje się w `suspendCancellableCoroutine`:

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

- `resume()` zwraca wynik;
- `resumeWithException()` zwraca błąd;
- `invokeOnCancellation` anuluje operację zewnętrzną albo usuwa listener.

Anulowanie coroutine nie zatrzymuje callback API automatycznie. Continuation
można zakończyć tylko raz; jeśli callback może wywołać się ponownie albo
równolegle, potrzebne są `tryResume()`/`completeResume()` albo ochrona atomowa.

Dla callbacka z wieloma wartościami używa się `callbackFlow`:

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

`awaitClose` usuwa listener podczas anulowania albo zamknięcia Flow. Wynik
`trySend()` i buffer policy trzeba obsługiwać zgodnie z gwarancjami dostarczenia.

**Krótko:** jeden wynik — `suspendCancellableCoroutine`; strumień wartości —
`callbackFlow`. W obu przypadkach obowiązkowo zaimplementuj cleanup przy
cancellation.

</details>

<details>
<summary>63. Jaka jest różnica między Channel i Flow?</summary>

#### Kotlin

`Channel<T>` to asynchroniczna kolejka do komunikacji między coroutines:

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

Producer wywołuje `send()`, consumer — `receive()` albo czyta przez `for`. Każdą
wartość otrzymuje jeden consumer, dlatego Channel nadaje się do worker queue i
fan-out. Trzeba jawnie zarządzać capacity, zamknięciem i cancellation.

`Flow<T>` to deklaratywne API asynchronicznego strumienia:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow obsługuje `map`, `filter`, `combine`, cancellation i structured
concurrency. Flow utworzony przez `flow {}` jest zwykle cold; Channel jest hot i
może przyjmować wartości bez aktywnego consumera, zależnie od capacity.

Backpressure Channel zależy od bufora:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Czeka na receivera
```

- `RENDEZVOUS` — bez bufora;
- `BUFFERED` — ograniczony bufor;
- `CONFLATED` — przechowywana jest ostatnia wartość;
- `UNLIMITED` — nieograniczony bufor z ryzykiem wzrostu pamięci.

W Flow używa się `buffer()`, `conflate()` albo `collectLatest()`.

Channel można przedstawić jako Flow:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` nie robi broadcast: collectors dzielą wartości między sobą. Do
stanu używa się `StateFlow`, do broadcast — `SharedFlow`, a do callback bridge —
`callbackFlow`.

**Krótko:** `Channel` to kolejka producer-consumer; `Flow` to deklaratywny
strumień danych. Dla application streams zaczynaj od Flow, a Channel zostaw do
jawnego przekazywania pracy między coroutines.

</details>
<details>
<summary>64. Jakie są podstawowe operatory Flow (map, filter, combine, zip)?</summary>

#### Kotlin

`map` przekształca każdą wartość:

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` przepuszcza tylko wartości spełniające warunek:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` czeka na pierwszą wartość z każdego Flow, a następnie emituje nowy wynik
po aktualizacji któregokolwiek z nich:

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` łączy wartości parami według kolejności:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

Wynik: `1A`, `2B`, `3C`. `zip` czeka na odpowiednią parę, natomiast `combine`
używa ostatnich dostępnych wartości. Dlatego dla stanu z kilku źródeł zwykle
potrzebny jest `combine`, a dla par elementów — `zip`.

**Krótko:** `map` transformuje, `filter` odfiltrowuje, `combine` reaguje na
ostatnie wartości wszystkich strumieni, `zip` łączy wartości parami.

</details>
<details>
<summary>65. Jaka jest różnica między combine i zip?</summary>

#### Kotlin

Oba operatory łączą Flow, ale mają różną semantykę:

```text
combine -> nowa wartość + ostatnie wartości innych Flow
zip     -> pierwsze z pierwszym, drugie z drugim
```

`combine` czeka na pierwszą wartość z każdego upstream, a potem reaguje na każdą
aktualizację:

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` tworzy pary według kolejności:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

Szybszy upstream w `zip` czeka na parę. Wynik kończy się, gdy nie będzie już
pełnych par. W `combine` zakończony upstream może być używany ze swoją ostatnią
wartością, dopóki inne nadal emitują.

**Krótko:** `combine` służy do stanu aktualizowanego przez dowolne źródło;
`zip` — do parowania sekwencji element po elemencie.

</details>
<details>
<summary>66. Do czego służą @JvmStatic, @JvmOverloads, @JvmField?</summary>

#### Kotlin

Te adnotacje zmieniają JVM API dla kodu Java.

- `@JvmStatic` generuje statyczny bridge dla członka `object` albo
  `companion object`.
- `@JvmOverloads` generuje overloads dla kolejnych parametrów z wartościami
  domyślnymi, zaczynając od prawej strony.
- `@JvmField` udostępnia backing field bezpośrednio, bez getter/setter.

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

Java będzie mogła wywołać `UserFactory.create()`, odczytać
`UserFactory.defaultName` i użyć overload bez `active`.

`@JvmField` nie działa z delegated property ani custom accessors i osłabia
enkapsulację. Dla compile-time constants typu prymitywnego albo `String` lepsze
jest `const val`.

**Krótko:** te adnotacje są potrzebne tylko dla wygodnego Java/framework API. W
kodzie Kotlin-only nie warto dodawać ich bez potrzeby.

</details>
<details>
<summary>67. Czym jest delegowanie (delegation)?</summary>

#### Kotlin

Delegowanie przekazuje implementację innemu obiektowi zamiast używać dziedziczenia. Kotlin
obsługuje delegowanie interfejsu przez `by`:

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

Kompilator generuje forwarding methods. Osobną metodę można nadpisać:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Właściwości również mogą mieć delegate:

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

Custom property delegate implementuje `getValue()`, a dla `var` — także
`setValue()`.

**Krótko:** class delegation usuwa forwarding boilerplate i wspiera kompozycję;
property delegation przenosi logikę odczytu oraz zapisu właściwości do osobnego
obiektu.

</details>
<details>
<summary>68. Jakie frameworki testowe są dostępne w Kotlin?</summary>

#### Kotlin

Wybór zależy od poziomu testu:

- `kotlin.test` — multiplatform annotations i assertions;
- JUnit — JVM unit tests;
- Kotest — alternatywny DSL i matchers;
- MockK albo Mockito — test doubles;
- `kotlinx-coroutines-test` — coroutines, test dispatchers i virtual time;
- Turbine — sprawdzanie emissions Flow;
- AndroidX Test, Espresso i Compose UI Test — Android/UI;
- Robolectric — część scenariuszy Android na JVM.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

Dla domain logic często wystarczą JUnit albo `kotlin.test` i ręczne fakes.
Narzędzie nie zastępuje właściwego poziomu testu: zachowanie device-specific
sprawdza się w instrumentation tests.

**Krótko:** unit — JUnit/`kotlin.test`, coroutines — coroutines-test, Flow —
Turbine, Android UI — Espresso albo Compose UI Test.

</details>
<details>
<summary>69. Jak mockować zależności w testach?</summary>

#### Kotlin

Zależność przekazuje się przez konstruktor i zastępuje mock, stub albo fake.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

MockK dla funkcji suspend:

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

Ręczny fake:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

Fake lepiej nadaje się do sprawdzania zachowania, mock — gdy interaction jest
częścią kontraktu. Nie warto mockować value objects, pure functions ani każdego
wewnętrznego wywołania: takie testy wiążą się z implementacją.

**Krótko:** używaj constructor injection; preferuj prosty fake, a mock stosuj dla
ważnych interactions z zewnętrzną zależnością.

</details>
<details>
<summary>70. Jak testować coroutines i Flow?</summary>

#### Kotlin

Do coroutines używa się `kotlinx-coroutines-test` i `runTest`:

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` udostępnia virtual time i `TestCoroutineScheduler`:

- `runCurrent()` wykonuje zadania w bieżącym momencie;
- `advanceTimeBy()` przesuwa virtual time;
- `advanceUntilIdle()` wykonuje kolejkę do bezczynności.

Dispatchers lepiej przekazywać jako zależności. `Dispatchers.Main` w local unit
tests podmienia się test dispatcherem i przywraca przez `resetMain()`.

Sekwencję Flow emissions wygodnie sprawdzać przez Turbine:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Dla finalnego stanu `StateFlow` często wystarczy wykonać queued tasks i
sprawdzić `state.value`. Nie używaj `Thread.sleep()`, real delays ani różnych
test schedulers w jednym teście.

**Krótko:** `runTest` i test dispatcher — dla coroutines; Turbine — dla kolejności
Flow emissions; `StateFlow.value` — dla finalnego stanu.

</details>
<details>
<summary>71. Jakie są best practices pisania testowalnego kodu?</summary>

#### Kotlin

Testowalny kod ma jawne zależności i przewidywalne zachowanie.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

Podstawowe zasady:

- constructor injection zamiast tworzenia zależności wewnątrz klasy;
- jedna wyraźna odpowiedzialność na klasę;
- pure functions do obliczeń biznesowych;
- abstrakcje dla czasu, I/O, API, bazy danych i dispatchers;
- minimum Android framework w domain logic;
- fakes zamiast nadmiernego mockowania;
- sprawdzanie public behavior, a nie private methods i wewnętrznych wywołań.

ViewModel testuje się przez observable state/effects:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

Nie trzeba tworzyć interface dla każdej klasy. Abstrakcja jest uzasadniona na
granicy ze zmienną albo zewnętrzną zależnością.

**Krótko:** jawne zależności, kontrolowane side effects i małe odpowiedzialności
sprawiają, że kod jest testowalny bez wiązania testów z implementacją.

</details>
<details>
<summary>72. Jak efektywnie zarządzać zależnościami w projekcie Kotlin?</summary>

#### Kotlin

W projekcie Gradle trzeba kontrolować wersje, scopes i kierunek zależności między
modułami.

- Version catalog centralizuje aliases i wersje w `libs.versions.toml`.
- `implementation` ukrywa dependency przed downstream modules.
- `api` ujawnia ją jako część public API i jest używane tylko wtedy, gdy trzeba.
- `testImplementation` i `androidTestImplementation` izolują zależności testowe.
- BOM uzgadnia wersje artefaktów jednej ekosystemu.
- Convention plugins usuwają duplikację konfiguracji Gradle.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

Moduł powinien zależeć od potrzebnego contract, a nie od całej implementation.
Gradle dependencies i runtime DI to różne poziomy: Gradle określa dostępność
kodu, DI tworzy obiekty w czasie wykonania.

Do diagnostyki:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Aktualizacje robi się małymi krokami, sprawdzając changelog, build i tests.

**Krótko:** centralizuj wersje, preferuj `implementation`, kontroluj module graph
i regularnie sprawdzaj dependency graph.

</details>
<details>
<summary>73. Jak Kotlin działa z pamięcią i garbage collection?</summary>

#### Kotlin

Model pamięci zależy od platformy. Na JVM i Androidzie Kotlin używa zarządzania
pamięcią JVM/ART: obiekty żyją w heap, a GC zwalnia te, do których nie ma już
osiągalnych referencji.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

Po zakończeniu funkcji obiekt może zostać zebrany, jeśli nic go nie utrzymuje.
Przypisanie `null` tylko usuwa referencję — moment oczyszczenia określa GC.

Memory leak powstaje, gdy niepotrzebny obiekt pozostaje reachable:

```kotlin
object Holder {
    var activity: Activity? = null // Może utrzymywać Activity
}
```

Na Androidzie typowe przyczyny to Activity/View w singletonie, nieoczyszczone
listeners albo binding, a także coroutine z dłuższym lifecycle. Częste
allocations, boxing, closures i pośrednie kolekcje mogą zwiększać GC pressure,
ale optymalizować je trzeba dopiero po profilowaniu.

Kotlin/Native i Kotlin/JS używają mechanizmów pamięci swojej platformy, dlatego
szczegółów JVM nie można przenosić na wszystkie targets.

**Krótko:** na JVM/Androidzie pamięć zwalnia GC po utracie wszystkich osiągalnych
referencji. Główne ryzyko to nie `null`, lecz długowieczna referencja do
niepotrzebnego obiektu.

</details>
<details>
<summary>74. Jakie są best practices pracy z null safety?</summary>

#### Kotlin

Typ nullable powinien oznaczać realną możliwość braku wartości, a nie być używany
„na wszelki wypadek”.

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

Podstawowe zasady:

- unikaj `!!`; używaj `?.`, `?:` albo early return;
- `requireNotNull` — dla kontraktu wejściowego, `checkNotNull` — dla stanu
  wewnętrznego;
- sprawdzaj Java platform types na granicy Java/Kotlin;
- przekształcaj nullable DTO na ścisłe domain models w mapperze;
- jeśli `null` oznacza osobny stan, modeluj go typem sealed.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

Fallback nie powinien ukrywać błędu: jeśli brak wartości narusza kontrakt, lepiej
zakończyć wykonanie jawnie.

**Krótko:** modeluj brak wartości typem `T?`, obsługuj go na granicy i nie
zamieniaj kodu nullable w łańcuch `!!`.

</details>
<details>
<summary>75. Czym jest supervisorScope i czym się różni?</summary>

#### Kotlin

W `coroutineScope` nieobsłużony błąd coroutine potomnej anuluje scope i
siblings. W `supervisorScope` direct children mogą upadać niezależnie:

```text
coroutineScope  -> failure child anuluje siblings
supervisorScope -> failure child nie anuluje siblings
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

Supervisor nie obsługuje błędów automatycznie. Exception z `async` zostanie
rzucony przez `await()`, jeśli nie zostanie przekształcony w wynik.

`supervisorScope` używa się lokalnie w funkcji suspend, a `SupervisorJob` — dla
długowiecznego scope.

**Krótko:** powiązane zadania all-or-nothing — `coroutineScope`; niezależne
zadania z dopuszczalnym częściowym wynikiem — `supervisorScope`.

</details>
<details>
<summary>76. Czym jest backpressure i jak obsługiwać go w Flow?</summary>

#### Kotlin

Backpressure powstaje, gdy producer jest potencjalnie szybszy niż consumer. W
zwykłym Flow bez buffer `emit()` wstrzymuje się, dopóki downstream nie zakończy
przetwarzania, więc producer naturalnie zwalnia.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

Operatory zmieniają to zachowanie:

- `buffer(n)` pozwala producerowi wyprzedzić consumera o `n` wartości;
- `conflate()` pomija wartości pośrednie, zachowując aktualną;
- `collectLatest` anuluje poprzednie przetwarzanie;
- `debounce` czeka na pauzę między zdarzeniami wejściowymi;
- `sample` bierze ostatnią wartość w interwale;
- `flatMapLatest` anuluje poprzedni inner Flow.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

Jeśli każda wartość jest ważna, nie można jej conflated ani anulować: trzeba
zoptymalizować consumer, użyć kontrolowanego buffer albo niezawodnej kolejki.
Nieograniczony buffer może spowodować wzrost pamięci.

**Krótko:** Flow już tworzy backpressure przez suspend `emit()`. Buffering,
conflation albo operatory latest dodaje się tylko wtedy, gdy znana jest potrzebna
polityka utraty lub oczekiwania na dane.

</details>
<details>
<summary>77. Jak używać Java Streams w Kotlin?</summary>

#### Kotlin

Kotlin/JVM może bezpośrednio używać Java Stream API:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Dla kolekcji Kotlin standardowe operatory są zwykle krótsze:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Do leniwego synchronicznego przetwarzania służy `Sequence`:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Stream jest właściwy, gdy zwraca go Java API, potrzebny jest Java `Collector`
albo pipeline jest już napisany w Javie. Stream jest jednorazowy.

I/O Stream trzeba zamykać:

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Java Collectors często mają bezpośredni odpowiednik w Kotlin:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` używa common `ForkJoinPool`, dlatego ma niejawny parallelism i
trudniej się anuluje. W kodzie coroutine lepiej kontrolować równoległość jawnie;
dla I/O parallel Stream nie zastępuje suspend API.

```text
Stream/Sequence -> synchronous one-shot pipeline
Flow            -> asynchronous, suspending, cancellable stream
```

**Krótko:** Java Stream używaj do Java interop; Kotlin collection operators — do
eager-przetwarzania; `Sequence` — do lazy synchronous pipeline; `Flow` — do
danych asynchronicznych.

</details>
<details>
<summary>78. Czym jest Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` to stara nazwa mobilnego kierunku `Kotlin Multiplatform` (`KMP`). Idea:
wynieść wspólny kod Kotlin do shared module i używać go na Androidzie oraz iOS.

Zwykle współdzieli się:

- domain models;
- use cases;
- validation;
- repository contracts;
- networking/data layer;
- business rules.

Typowa struktura:

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` zawiera kod niezależny od platformy:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Platform-specific API ukrywa się za interface albo `expect/actual`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

UI można zostawić natywne: Android Compose/View oraz SwiftUI/UIKit. Jeśli zespół
jest gotowy, część UI można współdzielić przez Compose Multiplatform.

**Krótko:** KMP daje wspólną logikę biznesową dla Android/iOS, ale platform API,
szczegóły UX i integracje trzeba izolować. To nie „jeden kod do wszystkiego”,
lecz kontrolowany sharing tam, gdzie realnie zmniejsza duplikację.

</details>
<details>
<summary>79. Jakie ograniczenia ma Kotlin Multiplatform?</summary>

#### Kotlin

`Kotlin Multiplatform` jest użyteczny dla shared logic, ale ma ważne ograniczenia.

1. **Platform API są niedostępne w `commonMain`**

We wspólnym kodzie nie można bezpośrednio używać `Context`, UIKit, Android-only
ani iOS-only SDK. Potrzebne są interfaces albo `expect/actual`.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **Nie wszystkie biblioteki są multiplatform**

Biblioteki Java/Android-only nie można po prostu dodać do `commonMain`. Trzeba
szukać odpowiednika KMP albo zrobić platform implementation.

3. **iOS interop ma niuanse**

Shared Kotlin kod eksportuje się do iOS jako framework. Trzeba uwzględniać Swift
API, generics, exceptions, coroutines/Flow bridging i debugging.

4. **UI sharing nie zawsze się opłaca**

Często współdzieli się domain/data layer, a UI zostawia natywne. Compose
Multiplatform jest możliwy, ale to osobna decyzja architektoniczna.

5. **Bardziej złożony build i ownership**

KMP dodaje source sets, Gradle setup, złożoność CI i wymaga ustaleń między
zespołami Android/iOS dotyczących shared module.

**Krótko:** KMP dobrze pasuje do stabilnej wspólnej logiki biznesowej, ale nie
usuwa kodu platform-specific. Główne ryzyka: interop, biblioteki, build
complexity, ownership i nadmierne wynoszenie wszystkiego do shared.

</details>
<details>
<summary>80. Jak współdzielić logikę biznesową w KMM?</summary>

#### Kotlin

Logikę biznesową wynosi się do shared module, a szczegóły Android/iOS zostawia w
platform source sets.

Typowa struktura:

```text
shared/
  commonMain   -> domain/use cases/contracts
  androidMain  -> Android implementation
  iosMain      -> iOS implementation
```

W `commonMain` dobrze trzymać:

- domain models;
- use cases;
- validation;
- repository interfaces;
- API clients;
- serialization;
- business rules.

Przykład:

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

Platform-specific API podstawia się przez DI, interfaces albo `expect/actual`:

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

Networking często robi się przez Ktor Client, JSON — przez
`kotlinx.serialization`, state — przez coroutines/Flow. Dla iOS od razu trzeba
przemyśleć bridge do Swift async/await albo Combine.

Nie warto współdzielić wszystkiego bez wyboru: permissions, camera,
notifications, platform storage i logikę UX-specific lepiej zostawiać na
platformie albo ukrywać za kontraktem.

**Krótko:** do shared module wynosi się stabilną logikę biznesową i contracts.
Części platform-specific implementuje się osobno. Dobry KMM to wyraźna granica,
a nie shared-monolit.

</details>
<details>
<summary>81. Jak optymalizować wydajność kodu Kotlin?</summary>

#### Kotlin

Optymalizacja zaczyna się nie od składni, lecz od pomiaru:

```text
measure -> find bottleneck -> fix -> measure again
```

Na Androidzie używa się Profiler, Perfetto/System Trace, Macrobenchmark i
allocation tracking.

Główne kierunki:

1. **Algorytmy i I/O**

Największy zysk dają właściwe data structures, mniej network/DB calls,
pagination, cache i DB indexes. `O(n²)` nie naprawia się elegancką składnią
Kotlin.

2. **Allocations w hot path**

W krytycznych miejscach zbędne `map/filter/copy` mogą tworzyć wiele obiektów:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

Ale w zwykłym UI readability jest ważniejsza niż ręczna mikrooptymalizacja.

3. **Collections i Sequence**

`Sequence` jest przydatna dla lazy chains albo early termination, ale ma overhead.
Na małych listach nie musi być szybsza.

4. **Coroutines**

- blocking I/O — `Dispatchers.IO`;
- CPU work — `Dispatchers.Default`;
- nie używać `runBlocking` na Main;
- anulować obsolete work;
- nie robić zbędnych context switches.

5. **UI performance**

W Compose nie wykonywać ciężkiej pracy w composition, używać stable keys, czytać
state bliżej miejsca użycia. W Views nie przeciążać `onBindViewHolder`, `onDraw`
i nie tworzyć zbędnych layout passes.

6. **Memory**

Nie trzymać `Activity/View` w singletonie, kontrolować cache size, lifecycle
resources i large bitmaps. Leaks zwiększają GC pressure i ryzyko OOM.

**Krótko:** najpierw profilujemy. Potem poprawiamy algorytmy, I/O, allocations,
concurrency, UI hot paths i memory leaks. `inline`, `Sequence` oraz primitive
arrays stosuje się tylko tam, gdzie pomiar pokazał korzyść.

</details>
<details>
<summary>82. Jak zaimplementować retry z exponential backoff w Kotlin?</summary>

#### Kotlin

Exponential backoff powtarza transient error z rosnącą pauzą. Retry powinien mieć
`maxAttempts`, `maxDelay`, jitter, timeout i nie psuć cancellation.

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

Przykład predicate:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Dla `Flow` można użyć `retryWhen`:

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

Nie należy retry-ować validation errors, `403/404`, auth failure bez refresh flow
ani non-idempotent operations bez idempotency key. Dla `429/503` warto uwzględniać
`Retry-After`.

**Krótko:** retry powinien być ograniczony, cancellable, z jitter i jasną zasadą,
które błędy można powtarzać. W przeciwnym razie maskuje problemy i tworzy
dodatkowe obciążenie.

</details>
<details>
<summary>83. Jakie nowe możliwości Kotlin warto znać w 2026 roku?</summary>

#### Kotlin

W 2026 roku na rozmowie kwalifikacyjnej ważne jest, aby orientować się nie w
każdej drobnej funkcji, lecz w kierunku Kotlin 2.x i tooling ecosystem.

1. **K2 compiler**

K2 to nowa compiler architecture Kotlin 2.x. Jest ważna dla szybszego analysis,
lepszej bazy dla IDE, compiler plugins i Multiplatform. Przed upgrade sprawdza
się kompatybilność Kotlin Gradle Plugin, AGP, Gradle, Compose compiler, KSP/kapt
i serialization.

2. **Context parameters / receivers**

Przydatne dla DSL i scoped APIs:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

To nie jest zamiennik DI, lecz sposób jawnego przekazania kontekstu wykonania.

3. **Explicit backing fields**

Pozwalają ograniczyć boilerplate dla read-only public API i mutable internal
state:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Rozwój Multiplatform**

KMP staje się praktyczniejsze: Swift export, coroutines/Flow bridging, Wasm i
Compose Multiplatform się rozwijają. Jednak production-decyzje nadal trzeba
oceniać pod kątem interop, tooling, debugging i team ownership.

5. **Standard library i JVM tooling**

Warto śledzić nowe stdlib API, `kotlin.uuid.Uuid`, JVM target/toolchain
compatibility, Gradle configuration cache i build performance.

6. **Zasada adoption**

Nowej funkcji nie trzeba wprowadzać do production tylko dlatego, że jest nowa.
Sprawdzamy:

- stable czy experimental;
- wsparcie IDE/CI;
- plugin compatibility;
- readability;
- migration/rollback plan.

**Krótko:** w 2026 trzeba znać Kotlin 2.x/K2, context APIs, backing fields,
rozwój KMP/Wasm/Compose Multiplatform i tooling constraints. W praktyce
ważniejsza jest bezpieczna aktualizacja ecosystem niż użycie każdej nowej
funkcji składniowej.

</details>
<details>
<summary>84. Czym jest cykl życia Activity?</summary>

#### Kotlin

Lifecycle `Activity` to sekwencja callbacks, przez które Android zarządza
tworzeniem, widocznością, stanem foreground/background i zniszczeniem ekranu.

Podstawowa kolejność:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Po powrocie ze stanu `STOPPED` może wystąpić `onRestart()` przed `onStart()`.

- `onCreate()` — utworzyć UI, navigation, podstawowe dependencies. Nie wykonywać
  ciężkiej pracy sync.
- `onStart()` — Activity stała się widoczna.
- `onResume()` — Activity jest w foreground i gotowa do interakcji.
- `onPause()` — straciła focus; callback powinien być szybki.
- `onStop()` — nie jest już widoczna; zatrzymać visible-only resources.
- `onDestroy()` — instance jest niszczona przez `finish()` albo recreation; nie
  jest gwarantowane przy kill process.

Configuration change może odtworzyć Activity:

```text
old: onPause -> onStop -> onDestroy
new: onCreate -> onStart -> onResume
```

`ViewModel` przeżywa configuration change. Mały UI state umieszcza się w
`SavedStateHandle`/`onSaveInstanceState`, długotrwałe dane — w storage.

Flow collection trzeba powiązać z lifecycle:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**Krótko:** `onCreate` tworzy ekran, `onStart` czyni go widocznym, `onResume`
aktywnym, `onPause/onStop` zatrzymują pracę, `onDestroy` kończy instance. State
nie można opierać wyłącznie na Activity instance.

</details>
<details>
<summary>85. Czym jest Fragment i czym różni się od Activity?</summary>

#### Kotlin

`Activity` to Android component z własnym window/task i entry point przez
manifest. `Fragment` to UI controller wewnątrz Activity albo innego Fragment,
którym zarządza `FragmentManager`.

```text
Activity -> system component, window, intents, task
Fragment -> część UI wewnątrz hosta, back stack przez FragmentManager
```

Przykład Activity:

```kotlin
class MainActivity : AppCompatActivity()
```

Przykład Fragment:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

Główna praktyczna różnica: Fragment ma osobny lifecycle dla View:

```text
onCreateView -> onViewCreated -> onDestroyView
```

Fragment object może pozostać, a jego View jest już zniszczona. Dlatego binding,
adapter i UI subscriptions czyści się w `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Flow/LiveData dla UI trzeba wiązać z `viewLifecycleOwner`, a nie z lifecycle
samego Fragment.

Navigation:

- między Activities — `Intent`;
- między Fragments — `FragmentManager` albo Navigation Component.

W pełni Compose app często wystarczy jedna Activity bez Fragments. W hybrid/View
app Fragment nadal jest normalną abstrakcją screen/navigation.

**Krótko:** Activity to samodzielny Android component. Fragment to część UI
wewnątrz hosta. Najważniejsza zasada: View we Fragment żyje krócej niż sam
Fragment, dlatego cleanup robi się w `onDestroyView()`.

</details>
<details>
<summary>86. Czym jest Intent i jak się go używa?</summary>

#### Kotlin

`Intent` to Android message object, który opisuje akcję albo konkretny component:
otworzyć Activity, Service, Broadcast albo przekazać dane.

1. **Explicit Intent**

Wskazuje konkretny component:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

Używa się go do internal navigation między Activities.

2. **Implicit Intent**

Opisuje akcję, a Android szuka app, która ją wykona:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Przykłady: browser, share, dialer, maps, file picker.

3. **Extras**

Przez extras przekazuje się niewielkie dane:

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Lepiej przekazywać `id`, a nie duże objects. Dla structured args można użyć
`Parcelable`, ale navigation często jest prostsza przez id + repository.

4. **Intent filter**

Komponent deklaruje, jakie intents przyjmuje:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

To jest potrzebne dla deep links, share targets i system actions.

5. **Activity Result API**

Zamiast `startActivityForResult` używa się lifecycle-aware API:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // handle uri
}

launcher.launch("image/*")
```

6. **Security**

- secrets nie przekazywać w extras;
- internal components uruchamiać explicit intent;
- sprawdzać external input;
- poprawnie ustawiać `android:exported`;
- dla implicit intent uwzględniać, że handler może być nieobecny.

**Krótko:** explicit intent uruchamia konkretny component, implicit intent
opisuje akcję dla systemu. Extras służą tylko do niewielkich danych. Dla wyników
— Activity Result API. Dla external intents potrzebne są walidacja i security
awareness.

</details>
<details>
<summary>87. Co się stanie, jeśli nie wywołać super.onCreate()?</summary>

#### Kotlin

Jeśli w `Activity.onCreate()` nie wywoła się `super.onCreate(savedInstanceState)`,
bazowa klasa Android/Jetpack nie wykona obowiązkowej inicjalizacji lifecycle. Może
to spowodować crash albo niepoprawne działanie Activity.

Poprawnie:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` przygotowuje:

- lifecycle state;
- saved state restoration;
- window/decor infrastructure;
- `FragmentManager` w `FragmentActivity`;
- `SavedStateRegistry`;
- `ActivityResultRegistry`;
- integracje AppCompat/ComponentActivity.

Bez `super` możliwe są:

- `SuperNotCalledException`;
- zepsuty lifecycle;
- problemy z fragments;
- brak odtworzenia saved state;
- problemy z Activity Result API, Compose, Hilt albo AppCompat.

Dla Compose zasada jest taka sama:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

W lifecycle callback-ach Fragment również zwykle wywołuje się `super`, jeśli
dokumentacja wyraźnie nie mówi inaczej.

**Krótko:** `super.onCreate()` to nie formalność, lecz część kontraktu z
framework. Trzeba go wywołać na początku `onCreate()` przed logiką zależną od
UI/framework.

</details>
<details>
<summary>88. Czym jest View Binding i dlaczego jest lepszy niż findViewById?</summary>

#### Kotlin

`View Binding` generuje klasę binding dla XML layout i daje type-safe dostęp do
View bez ręcznego `findViewById`.

Problem `findViewById`:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Minusy:

- boilerplate;
- można pomylić typ;
- błąd będzie runtime;
- słabszy refactoring.

View Binding w `Activity`:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

We `Fragment` binding trzeba czyścić w `onDestroyView()`, bo View lifecycle jest
krótszy niż lifecycle Fragment:

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

Zalety View Binding:

- type safety;
- mniej boilerplate;
- compile-time sprawdzanie id;
- wygodny refactoring;
- nullable pola, jeśli View nie istnieje w danej layout configuration.

`View Binding` to nie to samo co `Data Binding`:

```text
View Binding -> dostęp do View
Data Binding -> binding expressions w XML
```

Dla w pełni Compose UI View Binding zwykle nie jest potrzebny. Jest aktualny dla
XML/View System i hybrid screens.

**Krótko:** View Binding to bezpieczniejsza i krótsza zamiana `findViewById` dla
XML layouts. Główny niuans: we Fragment czyścić binding w `onDestroyView()`.

</details>
<details>
<summary>89. Czym jest LiveData?</summary>

#### Kotlin

`LiveData` to lifecycle-aware observable data holder z Android Jetpack.
Przechowuje wartość i powiadamia observerów tylko wtedy, gdy `LifecycleOwner` jest
w stanie `STARTED` albo `RESUMED`.

Typowy przykład w `ViewModel`:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

Na zewnątrz zwracamy `LiveData`, a nie `MutableLiveData`, aby UI nie mogło
zmieniać state bezpośrednio.

Subskrypcja we Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` jest ważny, bo View Fragment żyje krócej niż sam Fragment.

`value` vs `postValue`:

```kotlin
_liveData.value = user      // main thread
_liveData.postValue(user)   // background thread, async
```

Zalety:

- lifecycle-aware;
- prosty API;
- dobrze działa z XML/View System;
- automatycznie odsubskrybowuje destroyed owner;
- wygodny dla legacy Android-kodu.

Ograniczenia:

- powiązany z Androidem;
- słabszy API dla streams niż `Flow`;
- niewygodny dla KMP;
- one-shot events często prowadzą do obejść typu `SingleLiveEvent`.

Porównanie:

```text
LiveData  -> Android lifecycle-aware holder
StateFlow -> Kotlin hot stream ze state value
```

W nowym coroutine-first albo Compose-kodzie częściej używa się `StateFlow` +
`collectAsStateWithLifecycle()`. `LiveData` jest właściwy w legacy View System
albo projektach, w których jest już standardem.

**Krótko:** LiveData to Android observable state holder, który uwzględnia
lifecycle. Dla nowego Kotlin/Compose kodu częściej lepszy jest `StateFlow`, ale w
XML/View System LiveData nadal działa poprawnie.

</details>
<details>
<summary>90. Czym jest Room i jak działa?</summary>

#### Kotlin

`Room` to biblioteka Jetpack do pracy z SQLite przez typobezpieczne API. Nie
usuwa SQL, lecz daje compile-time checks, DAO, migrations i wygodną integrację z
coroutines/Flow.

Główne części:

```text
@Entity   -> tabela
@Dao      -> SQL access methods
@Database -> entry point do bazy
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

Database:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Tworzenie:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room obsługuje `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Gdy tabela się zmienia, Room invalidates query i emituje nowe dane.

Dla relations często potrzebny jest `@Transaction`:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Schema changes robi się przez migrations:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

W production nie można bezmyślnie używać destructive migration, jeśli istnieją
user data.

Praktycznie Room ukrywa się za repository:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

UI nie powinno bezpośrednio zależeć od database entities.

**Krótko:** Room to typobezpieczna obudowa nad SQLite przez `Entity`, `Dao` i
`Database`. Obsługuje suspend/Flow, relations i migrations. W normalnej
architekturze Room żyje w data layer za repository, a nie bezpośrednio w UI.

</details>
<details>
<summary>91. Co robią adnotacje @Query, @Insert, @Delete?</summary>

#### Kotlin

`@Query`, `@Insert`, `@Delete` to adnotacje Room dla metod DAO. Room generuje
implementation i sprawdza SQL/mapping na etapie kompilacji.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

Wykonuje jawny SQL:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` albo `:name` to bind parameters. Nie trzeba konkatenować user input do SQL
string.

`@Query` może być nie tylko `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

`Int` to liczba affected rows.

### `@Insert`

Wstawia `@Entity`:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` określa zachowanie przy konflikcie primary/unique key: `ABORT`,
`IGNORE`, `REPLACE`. Z `REPLACE` trzeba uważać: SQLite może faktycznie usunąć
stary row i wstawić nowy. Dla insert-or-update często lepsze jest `@Upsert`,
jeśli jest dostępne w projekcie.

### `@Delete`

Usuwa entity po primary key:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Do usuwania po id albo warunku lepsze jest `@Query("DELETE ...")`.

### `@Transaction`

Jeśli kilka operacji DAO ma być atomowych, używa się `@Transaction`:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room sprawdza tables, columns, bind parameters i return mapping. Ale performance,
indexes i logikę biznesową nadal musi sprawdzać developer.

**Krótko:** `@Query` wykonuje SQL, `@Insert` wstawia entity, `@Delete` usuwa
entity po primary key. Room generuje kod DAO i łapie wiele błędów w compile time,
ale nie zastępuje analizy query performance.

</details>
<details>
<summary>92. Czym jest WorkManager i kiedy go używać?</summary>

#### Kotlin

`WorkManager` to Jetpack API do persistent deferrable background work. Oznacza to,
że zadanie może wystartować później, ale powinno przeżyć zamknięcie app, process
death i w razie potrzeby reboot.

Używa się go do:

- background sync;
- retry upload/download;
- cleanup;
- logs/analytics dispatch;
- periodic maintenance;
- pracy z constraints: network, charging, battery/storage.

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

`CoroutineWorker`:

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

Wyniki:

- `Result.success()` — zadanie wykonane;
- `Result.retry()` — powtórzyć z backoff policy;
- `Result.failure()` — terminal failure.

Ważne możliwości:

- constraints;
- retries/backoff;
- unique work przez `enqueueUniqueWork()`;
- chaining;
- periodic work, ale bez dokładnego czasu uruchomienia.

`Data` dla input/output powinno być małe. Duże payloads trzeba umieszczać w
DB/file i przekazywać id.

WorkManager nie nadaje się do:

- exact alarms;
- realtime socket connection;
- media playback;
- stałego location tracking;
- akcji, której użytkownik oczekuje natychmiast.

Dla ongoing user-visible pracy potrzebny jest foreground service. Dla dokładnego
czasu — AlarmManager/Exact Alarm API, jeśli to rzeczywiście dozwolony use case.

**Krótko:** WorkManager służy do niezawodnej odłożonej pracy background z
constraints i retry. Nie gwarantuje dokładnego czasu uruchomienia i nie zastępuje
foreground service, realtime connection ani exact alarm.

</details>
<details>
<summary>93. Jaka jest różnica między CoroutineScope, lifecycleScope i viewModelScope?</summary>

#### Kotlin

`CoroutineScope` określa context i lifetime coroutines. `lifecycleScope` oraz
`viewModelScope` to gotowe Android scopes z już określonym ownerem.

```text
CoroutineScope  -> lifetime określasz sam
lifecycleScope  -> żyje do DESTROYED w LifecycleOwner
viewModelScope  -> żyje do ViewModel.onCleared()
```

### `CoroutineScope`

Ogólny scope dla coroutines:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Jeśli utworzyłeś custom scope, musisz jawnie wywołać `cancel()` we właściwym
momencie. Inaczej powstanie leak albo zbędna praca po śmierci ownera.

### `lifecycleScope`

Należy do `Activity`, `Fragment` albo innego `LifecycleOwner` i anuluje się przy
`DESTROYED`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Samo `lifecycleScope.launch` nie pauzuje się w `STOPPED`, dlatego dla Flow
collection potrzebny jest `repeatOnLifecycle`.

We Fragment do pracy z View/binding lepiej używać
`viewLifecycleOwner.lifecycleScope`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Powód: Fragment object może żyć dłużej niż jego View.

### `viewModelScope`

Należy do `ViewModel`, przeżywa configuration change i anuluje się w
`onCleared()`:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Używa się go do screen logic, loading, business calls i formowania UI state.

Typowe błędy:

- `GlobalScope` dla screen work;
- custom scope bez cancellation;
- collection binding/state w Fragment scope zamiast View lifecycle scope;
- uruchamianie operacji biznesowych z Activity zamiast ViewModel;
- dostęp do binding po `onDestroyView()`.

**Krótko:** `viewModelScope` — do logiki ekranu w ViewModel, `lifecycleScope` —
do UI collection/effects, custom `CoroutineScope` — tylko gdy istnieje własny
jasny lifetime i miejsce na `cancel()`.

</details>
<details>
<summary>94. Jak implementuje się dependency injection w Kotlin?</summary>

#### Kotlin

Dependency Injection to przekazywanie zależności z zewnątrz, a nie tworzenie ich
wewnątrz klasy. W Kotlin defaultowym podejściem jest constructor injection.

Poprawnie:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Źle:

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

Drugi wariant sztywno wiąże klasę z implementation i utrudnia testy.

Zwykle use case zależy od contract:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Manual DI

Dla małego projektu wystarczy prosty container:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Plus — prostota. Minus — duży graph trudno utrzymywać ręcznie.

### Hilt/Dagger

Compile-time DI, typowy wybór dla dużych Android apps:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val getUser: GetUserUseCase
) : ViewModel()
```

Binding interface:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface UserModule {
    @Binds
    fun bindUserRepository(impl: UserRepositoryImpl): UserRepository
}
```

Plus — błędy graph łapane są w compile time. Minus — więcej boilerplate i
trudniejsza generacja.

### Koin

Runtime DI przez Kotlin DSL:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

Plus — łatwiej zacząć. Minus — część błędów może ujawnić się runtime.

DI zarządza też lifetime: `singleton`, `factory`, `ViewModel scope`. Scope trzeba
dobierać ostrożnie, aby nie dostać leaks albo zbędnych singletons.

**Krótko:** w Kotlin DI najczęściej robi się przez constructor injection. Manual
DI pasuje do małego graph, Hilt/Dagger — do dużych Android apps, Koin — dla
prostoty. DI zmniejsza coupling i mocno upraszcza testowanie.

</details>
<details>
<summary>95. Czym jest Hilt i jak działa?</summary>

#### Kotlin

`Hilt` to Android DI framework zbudowany na Dagger. Generuje dependency graph i
gotową component hierarchy dla `Application`, `Activity`, `Fragment`, `ViewModel`,
`View` i `Service`.

Podstawowy setup:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` tworzy root graph na poziomie application.

Android class, do której trzeba inject dependencies:

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Constructor injection to główny sposób:

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

ViewModel:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Modules są potrzebne, gdy Hilt nie może sam utworzyć dependency: interface,
third-party class, builder/factory.

`@Binds` — dla interface → implementation:

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

`@Provides` — do tworzenia przez factory/builder, na przykład `OkHttpClient`,
`Retrofit`, `Room`.

Scopes określają lifetime obiektu:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Główne components:

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Ważna zasada: długowieczny component nie może zależeć od krótkowiecznego. Na
przykład `@Singleton` dependency nie powinno trzymać `Activity` context.

Dla testów Hilt pozwala zastępować modules przez `@TestInstallIn`. Ale w unit
tests często prościej przekazać fake bezpośrednio przez constructor.

**Krótko:** Hilt generuje Dagger graph dla Androida. `@Inject` tworzy zależności,
`@Binds/@Provides` opisują bindings, `@AndroidEntryPoint` włącza injection w
Android class, a scopes kontrolują lifetime.

</details>
<details>
<summary>96. Czym jest @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` integruje `ViewModel` z Hilt. Hilt generuje
`ViewModelProvider.Factory` i przekazuje dependencies przez constructor
injection, dlatego ręczna factory zwykle nie jest potrzebna.

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

Zasady:

- `@HiltViewModel` umieszcza się na klasie;
- `@Inject constructor` — na constructor;
- wszystkie dependencies muszą być w Hilt graph;
- `SavedStateHandle` Hilt dostarcza automatycznie;
- host `Activity/Fragment` musi mieć `@AndroidEntryPoint`.

Pobranie we Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

Dla shared ViewModel na poziomie Activity:

```kotlin
private val viewModel: UserViewModel by activityViewModels()
```

W Compose:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

Scope ViewModel określa `ViewModelStoreOwner`: Fragment, Activity albo navigation
back stack entry.

Zależności ViewModel żyją w `ViewModelComponent`. `@ViewModelScoped` daje jeden
instance na konkretną ViewModel. `@Singleton` — jeden instance na application
graph.

Typowe błędy:

- zapomnieć `@HiltViewModel` albo `@Inject constructor`;
- zapomnieć `@AndroidEntryPoint` w hoście;
- tworzyć ViewModel ręcznie przez constructor;
- oczekiwać jednego instance przy różnych `ViewModelStoreOwner`;
- wstrzykiwać `Activity`, `Fragment`, `View` albo krótkowieczny `Context` do
  ViewModel.

**Krótko:** `@HiltViewModel` pozwala Hilt tworzyć ViewModel przez generated
factory, wstrzykiwać constructor dependencies i `SavedStateHandle`. Rzeczywisty
lifetime określa `ViewModelStoreOwner`, a nie sama adnotacja.

</details>
<details>
<summary>97. Jak działa @Inject?</summary>

#### Kotlin

`@Inject` to adnotacja, dzięki której Dagger/Hilt rozumie, jak utworzyć obiekt
albo gdzie wstawić dependency.

Podstawowy wariant to constructor injection:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt będzie mógł utworzyć `LoadUserUseCase`, jeśli w graph istnieje binding dla
każdego constructor parameter. Jeśli bindingu brakuje albo jest niejednoznaczny —
pojawi się compile-time error.

Constructor injection jest lepszy dla własnych klas:

- dependencies są jawne;
- można używać `val`;
- klasę łatwo testować;
- nie ma ukrytego framework magic.

Field injection używa się głównie dla Android classes, które tworzy framework:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Takiego field nie można używać przed injection w lifecycle komponentu.

`@Inject constructor` nie rozwiązuje interface binding sam z siebie. Dla interface
potrzebny jest `@Binds`:

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

Dla third-party classes albo builderów używa się `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` nie określa lifetime. Lifetime określają scopes: `@Singleton`,
`@ViewModelScoped`, `@ActivityScoped`. Jeśli istnieje kilka bindings tego samego
typu, używa się qualifiers.

Hilt/Dagger generuje factories i graph w compile time, bez runtime reflection do
tworzenia zależności.

**Krótko:** `@Inject` pokazuje Hilt/Dagger, jak utworzyć class albo gdzie wstawić
dependency. Constructor injection — default; `@Binds` jest potrzebny dla
interfaces, `@Provides` — dla external objects, scopes — dla lifetime, qualifiers
— do rozróżniania tych samych typów.

</details>
<details>
<summary>98. Czym są obszary widoczności (scopes) w Hilt?</summary>

#### Kotlin

Scope w Hilt określa, w ramach którego component cache'owana jest dependency.
Oznacza to, że wszystkie żądania w jednym component instance otrzymują ten sam
object.

Bez scope binding zwykle tworzony jest od nowa dla każdego injection request. To
normalny default dla tanich stateless objects.

Uproszczona hierarchia components:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

Child widzi parent bindings, ale parent nie może zależeć od krótkowiecznego child
binding.

Główne scopes:

- `@Singleton` — jeden instance na application graph;
- `@ActivityRetainedScoped` — jeden instance na logical Activity, przeżywa
  configuration changes;
- `@ViewModelScoped` — jeden instance na konkretną ViewModel;
- `@ActivityScoped` — jeden instance na konkretny Activity instance;
- `@FragmentScoped` — jeden instance na konkretny Fragment;
- `@ViewScoped` — jeden instance na Hilt-enabled View;
- `@ServiceScoped` — jeden instance na konkretny Service.

Scope musi odpowiadać component `@InstallIn`:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

`@ViewModelScoped` dependency będzie jedna dla konkretnej ViewModel, ale różna
dla różnych ViewModels. Jeśli object ma być wspólny między ViewModels jednej
Activity, potrzebny jest `@ActivityRetainedScoped`.

Typowy błąd: długowieczny object trzyma krótkowieczny context:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Tak nie wolno robić: singleton utrzyma Activity po destroy. Dla `@Singleton`
potrzebny jest application-level state/context.

Nie trzeba scope'ować wszystkiego bez wyboru. Scope to ownership/lifecycle
semantics, a nie tylko optimization.

**Krótko:** Hilt scope określa lifetime i reuse dependency w ramach component.
Scope powinien odpowiadać realnemu ownerowi i nie powinien utrzymywać objects z
krótszym lifecycle.

</details>
<details>
<summary>99. Jak unikać wycieków pamięci w Androidzie?</summary>

#### Kotlin

Memory leak powstaje, gdy obiekt nie jest już potrzebny, ale nadal istnieje do
niego strong reference. W Androidzie najczęściej wyciekają `Activity`,
`Fragment`, `View`, `Context`, callbacks albo coroutines.

### Nie trzymać Activity/View w długowiecznych obiektach

Źle:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

Jeśli to `Activity context`, Activity nie zostanie zwolniona po rotation/finish.
Dla app-level zależności używa się `applicationContext`.

### Czyścić ViewBinding we Fragment

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Fragment object może żyć dłużej niż jego View, dlatego View references czyści się
w `onDestroyView()`.

### Czyścić adapters/listeners/callbacks

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Adapters, listeners i anonymous callbacks często trzymają View albo Fragment
reference. Trzeba je unregister/clear we właściwym lifecycle callback.

### Używać lifecycle-aware coroutines

Dla Fragment UI collection:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Nie uruchamiać UI coroutines w scope, który żyje dłużej niż View. `GlobalScope`
dla UI/app flow nie używać.

### Compose

W Compose side effects trzeba czyścić przez `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

Nie trzymać `Activity` ani `View` w `remember`, jeśli nie ma jasnej lifecycle
przyczyny.

### Narzędzia

- LeakCanary;
- Android Studio Memory Profiler;
- heap dumps;
- allocation tracking.

`WeakReference` nie jest normalnym rozwiązaniem architektonicznym. Jeśli jest
„potrzebny”, najpierw trzeba sprawdzić ownership i lifecycle.

**Krótko:** nie trzymać krótkowiecznych Android objects w długowiecznych klasach,
czyścić Fragment View references w `onDestroyView()`, używać lifecycle-aware
coroutines, unregister callbacks i sprawdzać leaks przez LeakCanary/Profiler.

</details>
<details>
<summary>100. Jak bezpiecznie przechowywać klucze API w aplikacji Android?</summary>

#### Kotlin

Całkowicie bezpieczne przechowanie secret w Android app jest niemożliwe.
Wszystko, co trafiło do APK/AAB, można potencjalnie wyciągnąć przez decompile,
proxy, memory inspection albo rooted device.

Główna zasada: prawdziwych secrets nie umieszcza się w kliencie.

Źle:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Jeśli klucz daje dostęp do backend/admin/payment/service account, powinien być
tylko na serwerze.

Poprawny schemat:

```text
Android app -> your backend -> third-party API
```

Backend dodaje secret, sprawdza user/session, robi rate limit, audit i zwraca
tylko potrzebny wynik.

Nie wszystkie klucze są takie same:

```text
public/client key -> można umieścić w app, ale trzeba ograniczać
secret/admin key  -> nie wolno umieszczać w app
```

Jeśli client key jednak znajduje się w app, trzeba go ograniczyć:

- package name;
- SHA-256 certificate fingerprint;
- allowed APIs;
- quotas/rate limits;
- environment separation;
- monitoring;
- rotation plan.

`BuildConfig`, `local.properties` albo CI secrets pomagają nie commitować klucza
do Git, ale nie chronią go w zbudowanej app:

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

Po decompile taki klucz można zobaczyć.

NDK, R8/ProGuard i string obfuscation tylko utrudniają extraction. To friction, a
nie security boundary.

Android Keystore jest użyteczny dla device-generated keys i przechowywania user
tokens, ale nie ratuje hardcoded API secret: attacker może znaleźć miejsce, gdzie
app używa secret.

User access/refresh tokens trzeba przechowywać ostrożnie:

- EncryptedSharedPreferences albo encrypted DataStore;
- Android Keystore dla crypto key;
- krótki lifetime access token;
- refresh token rotation;
- logout czyści storage;
- backend obsługuje revoke.

**Krótko:** prawdziwe API secrets powinny żyć na backend. W kliencie mogą być
tylko ograniczone public/client keys. `BuildConfig`, obfuscation i NDK nie
gwarantują bezpieczeństwa; potrzebne są restrictions, monitoring i rotation.

</details>
<details>
<summary>101. Jak zaimplementować architekturę offline-first?</summary>

#### Kotlin

Offline-first oznacza: UI czyta lokalny source of truth, a network tylko go
synchronizuje.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Read path

UI subskrybuje Room:

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

Refresh pobiera remote data i transakcyjnie merge'uje je do DB:

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

Po zapisie w Room UI zaktualizuje się przez `Flow`. `replaceAll` jest
niebezpieczne, jeśli response jest partial albo istnieją pending local edits.

### Write path / outbox

Offline write najpierw zapisuje się lokalnie:

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

UI od razu widzi optimistic update. Outbox operation wysyła się później i usuwa
dopiero po server acknowledgement.

Ważne: local update i outbox insert muszą być w jednej transaction, inaczej
process death może zgubić operation.

### Sync

Dla retry/process-death resilience używa się WorkManager:

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

Sync musi być idempotent, bo retry może powtórzyć request. Foreground refresh i
worker powinny używać tej samej sync implementation.

### Conflicts

Conflict policy zależy od domain:

- server wins;
- client wins;
- revision/ETag;
- field-level merge;
- manual resolution.

Client timestamp jest niewiarygodny przez clock skew. Dla krytycznych domen
prosty last-write-wins często jest nieakceptowalny.

### UI state

UI powinno rozróżniać:

- cached data;
- initial loading bez local data;
- refreshing ze starymi data;
- offline;
- pending/failed changes;
- last successful sync.

Connectivity to tylko hint. Źródłem prawdy jest wynik realnego requestu.

**Krótko:** offline-first buduje się wokół lokalnego source of truth. Reads idą z
Room, remote data merge'uje się do DB, writes idą przez transactional outbox, sync
wykonuje WorkManager, a conflict policy określa domain.

</details>
<details>
<summary>102. Czym jest architektura MVI i kiedy jej używać?</summary>

#### Kotlin

MVI to podejście architektoniczne z unidirectional data flow:

```text
UI -> Event/Intent -> ViewModel/Reducer -> State -> UI
                                      └-> Effect
```

UI nie zmienia state bezpośrednio. Renderuje immutable `State` i wysyła `Event`.
ViewModel obsługuje event i tworzy nowy state.

Typowe części:

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

- `State` — durable screen model;
- `Event/Intent` — akcja użytkownika albo system input;
- `Reducer/Handler` — przejście `old state -> new state`;
- `Effect` — jednorazowa akcja: navigation, snackbar, permission request.

ViewModel:

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

State aktualizuje się przez `copy/update`:

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

Compose UI tylko czyta state i wysyła events:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Zalety:

- jeden source of truth dla screen state;
- przewidywalne transitions;
- łatwiej testować reducer/ViewModel;
- dobrze pasuje do Compose;
- wygodnie logować events/states.

Wady:

- więcej boilerplate;
- reducer może stać się god object;
- one-shot effects wymagają jasnej delivery policy;
- concurrency/races MVI nie rozwiązuje automatycznie;
- dla prostego CRUD może być zbędne.

MVI warto używać dla złożonych screens: wiele state transitions, retry,
optimistic updates, validation, multi-step flows. Dla prostego screen wystarczy
`ViewModel + StateFlow + methods`.

**Krótko:** MVI to cykl `Event -> State transition -> Render`. Jest użyteczne dla
złożonego UI ze stanami, ale wymaga dyscypliny z effects i concurrency oraz może
być overkill dla prostych ekranów.

</details>
<details>
<summary>103. Jak projektować skalowalne aplikacje Android?</summary>

#### Kotlin

Skalowalna aplikacja Android to aplikacja, w której feature można zmieniać
lokalnie, bez kaskadowych zmian w całym codebase.

Podstawowe warstwy:

```text
UI -> ViewModel -> domain -> repository -> API/DB
```

- UI renderuje state;
- ViewModel zarządza screen state i events;
- domain zawiera reguły biznesowe;
- repository ukrywa data sources;
- data layer pracuje z Retrofit/Room/SDK.

Warstw nie dodaje się mechanicznie. Use case albo interface są potrzebne, gdy
istnieje business rule, boundary, alternative implementation albo osobna potrzeba
testowania.

### Feature-first modules

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Feature module grupuje kod, który zmienia się razem. `core` powinien zawierać
rzeczywiście shared infrastructure, a nie przypadkowe utilities.

Dependency rule:

```text
app -> feature
feature -> core
core -X-> feature
```

Feature nie powinna zależeć od implementation innej feature. Do integracji lepsze
są contracts, navigation API albo API/implementation split.

### Contracts i models

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Interface jest potrzebny na realnym boundary. Nie trzeba tworzyć interface dla
każdego class „na wszelki wypadek”.

DTO, Entity, Domain i UiModel rozdziela się, jeśli mają różne semantics albo
lifecycle. Jeśli models są identyczne i nie mają różnych powodów do zmian,
zbędny mapping tylko szkodzi.

### State, errors, concurrency

UI powinno otrzymywać jeden observable state i wysyłać events. ViewModel powinna
jawnie określić concurrency policy: cancel previous, ignore duplicate, serialize
albo merge.

Raw `Throwable` nie powinien być UI contract. Infrastructure errors trzeba
przekształcać w domain/UI outcomes na boundary.

### DI, tests, build

- constructor injection jako default;
- scopes odpowiadają lifecycle ownerowi;
- domain/unit tests bez Androida;
- ViewModel state tests;
- repository integration tests;
- convention plugins i version catalog;
- affected-module CI;
- logging, metrics, crash context;
- migration plan dla API/DB changes.

Modularność bez testów, observability i zrozumiałego ownership nie skaluje
produktu.

**Krótko:** skalowalność dają feature ownership, kontrolowany dependency graph,
stable contracts, testable boundaries, explicit state/concurrency i production
support. Moduły i warstwy dodaje się tylko wtedy, gdy realnie izolują zmiany.

</details>
<details>
<summary>104. Czym jest Jetpack Compose i czym różni się od View System?</summary>

#### Kotlin

`Jetpack Compose` to declarative UI toolkit dla Androida. UI opisuje się
funkcjami Kotlin `@Composable` jako wynik bieżącego state, bez XML i ręcznego
aktualizowania `View`.

View System — imperative:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

Compose — declarative:

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

Gdy state się zmienia, Compose uruchamia recomposition potrzebnych części UI.

Composable function:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Composable powinien renderować state i wywoływać callbacks. Business logic, I/O i
długie operacje nie powinny żyć w UI-funkcjach.

Typowy schemat:

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

ViewModel formuje state, Compose go wyświetla.

Główne różnice:

```text
View System -> XML + View tree + manual updates + listeners
Compose     -> Kotlin functions + state-driven rendering + recomposition
```

Praktyczne niuanse Compose:

- composable body może wykonywać się wiele razy;
- side effects uruchamiać przez effect APIs;
- lokalny state/objects cache'ować przez `remember`, gdy trzeba;
- screen state trzymać w ViewModel;
- lazy lists powinny mieć stable keys;
- performance sprawdzać profilerem, a nie liczyć recompositions.

Compose i View System można mieszać:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

Albo wstawić View do Compose przez `AndroidView`. Dlatego migracja może być
stopniowa.

**Krótko:** Compose opisuje UI jako funkcję od state i aktualizuje go przez
recomposition. View System wymaga XML/View tree i ręcznych zmian View. Compose
lepiej wspiera state-driven UI, ale wymaga dyscypliny z side effects i state
ownership.

</details>
<details>
<summary>105. Czym jest @Composable i czym jest recomposition?</summary>

#### Kotlin

`@Composable` to adnotacja dla funkcji Jetpack Compose, które opisują UI.
`Recomposition` to ponowne wywołanie composable, gdy state, od którego zależy,
uległ zmianie.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Composable to funkcja od state:

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

Gdy `state` się zmienia, Compose aktualizuje potrzebną część UI przez
recomposition.

Przykład local state:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

Zmiana `count` uruchamia recomposition części UI, która czyta `count`.

Recomposition mogą uruchamiać:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable`;
- zmiana parameters composable.

Composable body może wykonywać się wiele razy, dlatego nie wolno robić tam side
effects:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // źle
}
```

Poprawnie:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` zachowuje wartość między recompositions:

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

Recomposition sama w sobie nie jest problemem. Problemem są ciężka praca w
composable, szerokie czytanie state, niestabilne parameters, zbędne allocations
albo lazy lists bez stable keys.

Praktyczne zasady:

- composable powinien być lekki i side-effect free;
- state czytać bliżej miejsca użycia;
- expensive objects cache'ować przez `remember`;
- effects uruchamiać przez `LaunchedEffect`, `DisposableEffect` itd.;
- dla list używać stable keys;
- performance sprawdzać profilerem, nie zgadywaniem.

**Krótko:** `@Composable` opisuje UI jako funkcję od state. `Recomposition`
ponownie wykonuje potrzebne composable przy zmianie state. To normalny mechanizm
Compose, jeśli UI-funkcje są lekkie, bez side effects i z poprawnie umieszczonym
state reading.

</details>
<details>
<summary>106. Czym jest state hoisting?</summary>

#### Kotlin

`State hoisting` to wyniesienie state z composable do parent albo state holder.
Child otrzymuje wartość i callback, dzięki czemu staje się stateless, reusable i
testable.

Wariant stateful:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Po hoisting:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

Parent staje się source of truth:

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

Dla screen-level state ownerem zwykle jest `ViewModel`:

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

Compose route czyta state i przekazuje events:

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

Hoisting wspiera unidirectional data flow:

```text
state w dół -> UI
 events w górę -> owner aktualizuje state
```

State trzeba podnosić do najniższego wspólnego ownera, który ma go czytać albo
zmieniać. Jeśli state jest potrzebny tylko wewnątrz komponentu i nie wpływa na
logikę biznesową, można zostawić go lokalnie.

Typowy API stateless composable:

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**Krótko:** state hoisting to „state w dół, events w górę”. Child nie posiada
state, a parent albo ViewModel staje się jedynym source of truth.

</details>
<details>
<summary>107. Jaka jest różnica między remember i rememberSaveable?</summary>

#### Kotlin

`remember` i `rememberSaveable` przechowują wartości między recompositions.
Różnica polega na tym, że `rememberSaveable` dodatkowo zapisuje value w saved
state i może odtworzyć je po configuration change albo process recreation.

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

Po recreation Activity wartość zostanie utracona.

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

Na Androidzie automatycznie obsługiwane są typy, które można umieścić w
saved-state `Bundle`: primitives, `String`, `Parcelable`, `Serializable` itd.
Dla custom type potrzebny jest `Saver`:

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

Kiedy czego używać:

- `remember` — transient UI state albo cache'owany object, który można utracić;
- `rememberSaveable` — mały UI state, który użytkownik oczekuje zachować: input
  text, selected tab, filter;
- `ViewModel` — screen/business state;
- storage/DataStore/DB — persistent data.

Ograniczenia `rememberSaveable`:

- saved state ma ograniczony rozmiar;
- dużych objects/collections się nie zapisuje;
- to nie jest długoterminowe persistence;
- nie zastępuje `ViewModel` ani repository.

**Krótko:** `remember` żyje tylko w composition. `rememberSaveable` odtwarza mały
saveable UI state po recreation. Dla screen state potrzebny jest `ViewModel`, dla
długotrwałych danych — storage layer.

</details>
<details>
<summary>108. Czym jest rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` to Compose API, które zwraca `CoroutineScope` powiązany
z bieżącą composition. Scope jest anulowany, gdy composable wychodzi z
composition.

Używa się go, gdy suspend UI-akcję trzeba uruchomić z callback: click, swipe,
drawer, snackbar, scroll animation.

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

Funkcji `suspend` nie można wywołać bezpośrednio z `onClick`, dlatego potrzebny
jest scope.

Jeszcze jeden przykład:

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

Różnica względem `LaunchedEffect`:

```text
LaunchedEffect         -> coroutine startuje automatycznie przy wejściu/zmianie key
rememberCoroutineScope -> coroutine startuje ręcznie z callback
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

Nie trzeba używać `rememberCoroutineScope()` do logiki biznesowej:

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

A w ViewModel:

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

`GlobalScope` dla UI się nie używa, bo nie jest powiązany z lifecycle i może
działać po zamknięciu ekranu.

**Krótko:** `rememberCoroutineScope()` jest potrzebny do krótkich suspend
UI-operacji z callbacks: snackbar, drawer, scroll. Dla automatycznych effects —
`LaunchedEffect`, dla screen/business logic — `viewModelScope`.

</details>
<details>
<summary>109. Czym jest collectAsState?</summary>

#### Kotlin

`collectAsState()` to Compose API, które collectuje `Flow` i przekształca go w
Compose `State`. Gdy Flow emituje nową wartość, Compose uruchamia recomposition
tam, gdzie ten state jest czytany.

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

Mechanika:

```text
Flow emit -> State update -> recomposition -> UI update
```

Dla zwykłego `Flow` potrzebna jest initial value:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

Dla `StateFlow` initial value już istnieje w `state.value`.

W Androidzie lepiej używać wariantu lifecycle-aware:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Różnica:

```text
collectAsState              -> Compose runtime, bez Android lifecycle
collectAsStateWithLifecycle -> collect tylko w aktywnym lifecycle state
```

W composable body nie można bezpośrednio collectować Flow:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // źle
}
```

Dla screen state używa się `collectAsStateWithLifecycle()`. Dla one-shot events —
`LaunchedEffect` i osobny `SharedFlow`:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigate albo show snackbar
    }
}
```

`State` to coś, co można ponownie wyrenderować. `Event` to jednorazowa akcja.

Jeśli Flow emituje zbyt często, trzeba zmniejszyć noise: `distinctUntilChanged()`,
mniejsze modele state, lokalne state reads, bez ciężkiej pracy w composable.

**Krótko:** `collectAsState()` przekształca Flow w Compose State. W Androidzie
zwykle używa się `collectAsStateWithLifecycle()`. Dla one-shot navigation/snackbar
events potrzebny jest `LaunchedEffect`, a nie state collection.

</details>
<details>
<summary>110. Czym jest Modifier w Compose?</summary>

#### Kotlin

`Modifier` w Compose opisuje layout, drawing, input, semantics i zachowanie
composable: `padding`, `size`, `background`, `clickable`, `scroll`,
accessibility itd.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

`Modifier` jest immutable i buduje się go jako chain. Kolejność ma znaczenie:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

i:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

dadzą różny rezultat, bo background stosuje się na innym etapie.

Reusable composable powinien przyjmować modifier z zewnątrz:

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

Dzięki temu parent może kontrolować pozycjonowanie i zewnętrzne zachowanie child.

Główne typy modifiers:

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

Dla accessibility lepiej używać wysokopoziomowych modifiers typu `clickable`, bo
dodają semantics. W razie potrzeby semantics ustawia się jawnie:

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Niektóre modifiers działają tylko w scope parenta:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Praktyczne zasady:

- przyjmować `modifier: Modifier = Modifier`;
- parent odpowiada za zewnętrzne pozycjonowanie;
- child może dodać wewnętrzne modifiers po przekazanym modifier;
- kolejność modifiers jest ważna;
- nie robić ciężkiej logiki w modifier callbacks.

**Krótko:** `Modifier` to immutable chain ustawień composable. Zarządza layout,
drawing, input i semantics. Kolejność wywołań jest ważna, a reusable composables
powinny przyjmować `modifier` jako parametr.

</details>
<details>
<summary>111. Jaka jest różnica między padding i offset?</summary>

#### Kotlin

`padding` i `offset` zmieniają położenie UI, ale działają na różnych etapach
layout.

```text
padding -> zmienia measurement i reserved space
offset  -> zmienia placement bez zmiany reported size
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

`padding` dodaje przestrzeń wokół content i wpływa na rozmiar, który widzą parent
oraz siblings.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` zostanie umieszczony po `A` razem z padding.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` przesuwa composable przy placement, ale parent rezerwuje stare miejsce.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` się nie odsunie, więc elementy mogą overlap. To przydatne dla badge, drag,
animation albo dekoracyjnego shift.

### RTL

`offset(x, y)` uwzględnia layout direction: dodatnie `x` przesuwa do logical end.
Jeśli potrzebne są fizyczne współrzędne bez RTL-mirroring — `absoluteOffset()`.

### Kolejność modifiers

Kolejność wpływa na layout, drawing i hit area:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

Background obejmie większy obszar.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

Background będzie tylko po padding.

Tak samo z `clickable`: jeśli `clickable` stoi przed `padding`, padding wchodzi w
click area; jeśli po — nie.

**Krótko:** `padding` dodaje layout space i wpływa na siblings. `offset` tylko
przesuwa composable bez nowego reserved space. Kolejność modifiers krytycznie
wpływa na wygląd i hit testing.

</details>
<details>
<summary>112. Jaka jest różnica między fillMaxSize i matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` i `matchParentSize()` mogą sprawić, że child będzie rozmiaru
parenta, ale działają inaczej.

```text
fillMaxSize()     -> bierze maximum constraints i wpływa na measurement
matchParentSize() -> powtarza już określony rozmiar Box
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

`fillMaxSize()` prosi parent o maksymalnie dostępny rozmiar. Jeśli constraints są
ograniczone ekranem, composable zajmie cały ekran.

Dla jednej osi są:

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` bierze udział w measurement i może wpłynąć na rozmiar parent
layout.

### `matchParentSize()`

`matchParentSize()` działa tylko w `BoxScope` i tylko dla direct child `Box`:

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

Taki child powtarza już obliczony rozmiar `Box`, ale go nie definiuje.

W uproszczeniu:

1. `Box` mierzy normal children;
2. określa własny rozmiar;
3. children z `matchParentSize()` otrzymują ten gotowy rozmiar.

Dlatego `matchParentSize()` dobrze pasuje do overlay: loading layer, scrim,
gradient, clickable layer.

Jeśli zamienić overlay na `fillMaxSize()`, child może poprosić o maksimum
constraints i powiększyć `Box`. To już inne zachowanie.

Praktyczny wybór:

- root/container ma zająć całą dostępną przestrzeń — `fillMaxSize()`;
- overlay ma przykryć content-sized `Box` — `matchParentSize()`;
- child nie ma wpływać na rozmiar `Box` — `matchParentSize()`;
- poza `BoxScope` `matchParentSize()` jest niedostępny.

**Krótko:** `fillMaxSize()` prosi o maksimum dostępnej przestrzeni i bierze udział
w measurement. `matchParentSize()` w `BoxScope` tylko powtarza już określony
rozmiar `Box` i go nie powiększa.

</details>
<details>
<summary>113. Jak obsługiwać przycisk „wstecz” w Compose?</summary>

#### Kotlin

W Compose back press obsługuje się przez `BackHandler` z
`androidx.activity.compose`. Przechwytuje on system back w ramach composable.

Podstawowy przykład:

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // content
}
```

`BackHandler` można włączać warunkowo:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Jeśli `enabled = false`, back event przejdzie dalej: do innego handlera albo
Navigation.

Typowy scenariusz to confirmation dialog dla unsaved changes:

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

Z `NavController` standardowy back często działa automatycznie przez back stack.
Custom `BackHandler` jest potrzebny, gdy standardowe zachowanie trzeba zmienić:
dialog, bottom sheet, selection mode, unsaved changes.

Dla lokalnego UI state back powinien najpierw zamykać górną warstwę:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

Priorytet jest taki:

```text
dialog/sheet/selection -> unsaved changes -> screen navigation
```

Dla złożonej logiki lepiej przekazać event do ViewModel:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

ViewModel decyduje: pokazać dialog, zapisać draft, pozwolić na navigation albo
wyemitować effect.

Typowe błędy:

- przechwytywać back zawsze;
- nie używać `enabled`;
- duplikować logic między `BackHandler` i `NavController`;
- nie uwzględniać dialog/sheet state;
- trzymać back logic daleko od state, który obsługuje.

**Krótko:** w Compose back press obsługuje się przez
`BackHandler(enabled = ...)`. Warto go używać dla górnych warstw UI i
niestandardowego zachowania, a zwykłą nawigację często zostawiać `NavController`.

</details>
<details>
<summary>114. Do czego używa się rememberUpdatedState?</summary>

#### Kotlin

`rememberUpdatedState()` daje long-lived effect aktualną value albo callback bez
restartu effectu.

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

`LaunchedEffect(Unit)` nie restartuje się, jeśli zmieni się `onTimeout`. Coroutine
może wywołać stale lambda.

Rozwiązanie:

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

Timer nie zaczyna się od nowa, ale po delay wywoływany jest latest callback.

Główna zasada:

```text
zmiana value ma restartować effect? -> key
zmiana value nie ma restartować effect? -> rememberUpdatedState
```

Jeśli zrobić callback key:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

to przy zmianie lambda poprzednia coroutine zostanie anulowana, a timer zacznie
się od nowa. Nie zawsze jest to pożądane.

Przykład z `DisposableEffect`:

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

Observer rejestruje się ponownie tylko przy zmianie `owner`, ale wywołuje
aktualny handler.

Kiedy value powinna być key:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Zmiana `userId` oznacza nową operation, więc starą trzeba anulować i uruchomić
nową.

`rememberUpdatedState()` nie uruchamia coroutine, nie zastępuje effect keys i nie
zachowuje value po wyjściu z composition. Po prostu zwraca `State<T>` z aktualną
wartością dla long-lived effect.

**Krótko:** `rememberUpdatedState()` jest potrzebny, aby effect widział latest
value/callback bez restartu. Jeśli zmiana value oznacza nową operation, powinna
być key, a nie `rememberUpdatedState`.

</details>
<details>
<summary>115. Czym są side-effects w Compose i jak je obsługiwać?</summary>

#### Kotlin

Side-effect w Compose to działanie poza czystym renderowaniem UI: coroutine,
snackbar, navigation, analytics, listener, lifecycle observer, sync z zewnętrznym
API.

Composable body może wykonywać się wiele razy, dlatego side-effect nie wolno
uruchamiać bezpośrednio:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // źle
}
```

Do tego służą effect APIs.

### `LaunchedEffect`

Uruchamia coroutine powiązaną z composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Jeśli `userId` się zmieni, stara coroutine zostanie anulowana i wystartuje nowa.

### `rememberCoroutineScope`

Potrzebny do suspend UI-akcji z callback:

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

Pasuje do snackbar, drawer, bottom sheet, scroll animation.

### `DisposableEffect`

Dla listeners/observers z cleanup:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

Jeśli subskrybujemy, odsubskrybowujemy w `onDispose`.

### `SideEffect`

Wykonuje się po udanej recomposition:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Nie nadaje się do suspend work.

### Inne API

- `rememberUpdatedState` — latest value/callback wewnątrz long-lived effect bez
  restart;
- `produceState` — async source do Compose `State`;
- `snapshotFlow` — Compose state do Flow.

Praktyczna mapa:

```text
composition coroutine      -> LaunchedEffect
coroutine from callback    -> rememberCoroutineScope
listener with cleanup      -> DisposableEffect
after recomposition sync   -> SideEffect
latest callback no restart -> rememberUpdatedState
async source to State      -> produceState
Compose state to Flow      -> snapshotFlow
```

**Krótko:** side-effects nie uruchamia się w body composable. Używa się do nich
effect APIs, aby jawnie kontrolować lifecycle, restart, cancellation i cleanup.

</details>
<details>
<summary>116. Czym są klucze (keys) w LaunchedEffect i dlaczego są ważne?</summary>

#### Kotlin

Keys w `LaunchedEffect` określają lifecycle coroutine-effectu: kiedy go uruchomić,
anulować i uruchomić ponownie. Jeśli key się zmienia, Compose anuluje starą
coroutine i uruchamia nową.

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

Zachowanie:

- composable weszło do composition — effect startuje;
- recomposition bez zmiany `userId` — effect nie restartuje się;
- `userId` zmienił się — stara coroutine cancel, nowa startuje;
- composable wyszło z composition — coroutine cancel.

Typowy błąd:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Jeśli `userId` się zmieni, effect się nie uruchomi ponownie i można dostać stale
data. Poprawnie:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` jest normalny, gdy effect ma wystartować raz przy wejściu
do composition:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigation/snackbar
    }
}
```

Kilka keys:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

Effect uruchomi się ponownie, jeśli zmieni się choć jeden key. Wszystko, co
określa wynik side effectu, powinno być key.

Nie używać niestabilnych keys:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Wtedy effect będzie restartował się niekontrolowanie.

Jeśli potrzebne jest latest value wewnątrz effectu, ale bez restartu, używa się
`rememberUpdatedState`:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Praktyczna zasada:

```text
zmiana value ma restartować effect -> key
potrzebne latest value bez restart  -> rememberUpdatedState
```

**Krótko:** keys w `LaunchedEffect` kontrolują uruchomienie, anulowanie i restart
coroutine. Niepoprawne keys dają stale data, zbędne restarty albo niestabilne
zachowanie UI.

</details>
<details>
<summary>117. Czym jest snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` to Compose API, które przekształca reads z Compose snapshot state w
cold `Flow`. Używa się go, gdy na zmianę Compose state trzeba reagować przez Flow
operators albo side-effect pipeline.

Podstawowy przykład:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` śledzi Compose state reads wewnątrz lambda. Gdy odczytana wartość
się zmienia, Flow emituje nową wartość.

Typowe scenariusze:

- scroll analytics;
- pagination trigger;
- debounce/sample dla UI state;
- logging/debugging;
- side effects, których nie wolno uruchamiać w composable body.

Przykład pagination:

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

Ponieważ wynik to `Flow`, można używać operators:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

`snapshotFlow` zwykle collectuje się w `LaunchedEffect`. Nie trzeba uruchamiać
collection bezpośrednio w composable body.

Nie używać `snapshotFlow` do zwykłego UI rendering:

```kotlin
Text(text = state.title)
```

Jeśli trzeba po prostu pokazać state, czytamy state bezpośrednio. `snapshotFlow`
jest potrzebny właśnie do przejścia z Compose state do Flow/side-effect logic.

Ważny niuans: w lambda musi być Compose snapshot state read. Jeśli go nie ma,
`snapshotFlow` nie ma czego śledzić.

**Krótko:** `snapshotFlow` konwertuje Compose state reads na `Flow`. Jest
przydatny do analytics, pagination, debounce i side effects w `LaunchedEffect`,
ale nie jest potrzebny do prostego renderowania UI.

</details>
<details>
<summary>118. Jak optymalizować recomposition w Compose?</summary>

#### Kotlin

Optymalizacja recomposition to nie „usunięcie wszystkich recompositions”, lecz
sprawienie, aby były tanie i lokalne. Recomposition jest normalna; problemem jest
ciężka praca albo zbyt szeroki obszar aktualizacji.

### Czytać state bliżej użycia

Źle:

```kotlin
ProfileScreen(state = hugeState)
```

Lepiej:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Jeśli composable czyta state, może recompositionować się przy zmianie tego state.
Nie przekazywać całego screen state tam, gdzie potrzebne jest jedno pole.

### Lazy lists: keys i contentType

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

Stable keys pomagają Compose nie mylić items i zachowywać item state.

Dla różnych typów elementów:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### Nie robić ciężkiej pracy w composable

Źle:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Lepiej:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

A jeszcze lepiej — przygotować formatted text w mapper/ViewModel, jeśli to część
UI model.

### Używać `derivedStateOf` dla częstych zmian

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Przydatne, gdy source state zmienia się często, ale UI ma reagować tylko na
derived condition.

### Stable/immutable models

Compose lepiej pracuje z immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Mutable collections i niestabilne objects mogą przeszkadzać Compose w pomijaniu
niepotrzebnych recompositions.

### Side effects — tylko przez effect APIs

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

Composable body może wykonywać się wiele razy, dlatego analytics, navigation,
listeners i suspend work nie powinny być bezpośrednio w body.

### Sprawdzać profilowaniem

- Layout Inspector recomposition counts;
- Compose compiler metrics;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

**Krótko:** optymalizacja recomposition to lokalne state reads, mniejsze
composables, stable keys, `contentType`, immutable UI models, `remember`,
`derivedStateOf`, poprawne effects i profilowanie. Celem są tanie lokalne
aktualizacje, a nie zero recompositions.

</details>
<details>
<summary>119. Jak debugować problemy z recomposition?</summary>

#### Kotlin

Problemy z recomposition debugguje się przez pomiar, a nie zgadywanie. Celem jest
znalezienie, który state często się zmienia i jaką część UI dotyka.

### Layout Inspector

Android Studio Layout Inspector pokazuje recomposition counts dla composables.
Ale liczba sama w sobie nie jest problemem: lekki composable może
recompositionować się często bez widocznego impact.

### Tymczasowe logs

Do szybkiego sprawdzenia można dodać log:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Albo przez `SideEffect`:

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

To tylko debug-narzędzie, nie production kod.

### Sprawdzić state reads

Częsta przyczyna: composable czyta zbyt szeroki state:

```kotlin
UserRow(screenState = state)
```

Lepiej:

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

State trzeba czytać bliżej miejsca użycia.

### Sprawdzić lists i parameters

Dla `LazyColumn` powinny być stable keys:

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

Sprawdzić też unstable parameters: mutable collections, objects/lambdas tworzone
od nowa bez potrzeby albo ciężką pracę w composable body.

### Compiler metrics i profiler

Compose compiler metrics pomagają zobaczyć, które composables/types są unstable,
restartable albo skippable. Android Studio Profiler i Macrobenchmark są potrzebne,
aby potwierdzić realny performance impact.

Praktyczny checklist:

- sprawdzić counts w Layout Inspector;
- znaleźć state, który często się zmienia;
- zawęzić state reads;
- sprawdzić stable keys w lazy lists;
- usunąć ciężką pracę z body;
- sprawdzić unstable parameters;
- zastosować `remember`/`derivedStateOf` tylko tam, gdzie jest powód;
- potwierdzić wynik profiler/metrics.

**Krótko:** recomposition debugguje się przez Layout Inspector, logs, compiler
metrics i profiler. Najczęstsze przyczyny to szeroki state read, unstable
parameters, brak keys, ciężka praca w body albo niepoprawny state ownership.

</details>
<details>
<summary>120. Jakie są sposoby uniknięcia lagów w Compose, na przykład podczas scrollowania?</summary>

#### Kotlin

Lagi podczas scrollowania najczęściej powodują ciężka praca na main thread,
zbędne recompositions, allocations w items, niestabilne keys albo zbyt złożone UI
wiersza.

### Stable keys i contentType

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

`key` zachowuje identity item po insert/delete/reorder.

Dla różnych typów elementów:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` pomaga Compose ponownie używać composition kompatybilnych items.

### Nie robić ciężkiej pracy w item

Źle:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Lepiej przygotować formatted text w mapper/ViewModel albo przynajmniej cache'ować
object przez `remember`. I/O, bitmap decoding, sorting/filtering i ciężkie
obliczenia nie powinny wykonywać się w composable.

### Zawęzić state reads

Item powinien otrzymywać tylko własne dane:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Nie przekazywać całego screen state do każdego wiersza.

### Stable UI models

Używać immutable UI models i nie tworzyć nowych collections/wrappers przy każdej
recomposition bez potrzeby.

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

`derivedStateOf` jest przydatne, gdy scroll zmienia się często, a UI reaguje
tylko na pochodny warunek.

### Images i paging

Dla images używać loadera z memory/disk cache i właściwym target size. Bitmap nie
dekodować ręcznie w composable ani na main thread.

Dla dużych list używać Paging:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Profilowanie

Potwierdzać optymalizacje przez:

- Layout Inspector recomposition counts;
- Android Studio Profiler/System Trace;
- Macrobenchmark w release-like buildzie;
- Baseline Profiles.

**Krótko:** smooth scroll w Compose wymaga stable keys, `contentType`, wąskich
state reads, tanich items, immutable models, cached images, paging i sprawdzania
profilerem. Najważniejsze — nie blokować main thread i nie robić ciężkiej pracy
podczas composition.

</details>
<details>
<summary>121. Czym różnią się adnotacje Stable i Immutable?</summary>

#### Kotlin

`@Stable` i `@Immutable` w Compose to contract dla compiler/runtime, który pomaga
zdecydować, czy można skipować recomposition. To nie „magiczna optymalizacja”,
lecz obietnica, że typ zachowuje się w określony sposób.

Główna różnica:

```text
@Immutable -> object nie zmienia się po utworzeniu
@Stable    -> object może się zmieniać, ale Compose widzi te zmiany
```

### `@Immutable`

Pasuje do value/UI models:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Warunek: wszystkie public properties nie zmieniają się po utworzeniu, a pola też
są immutable/stable.

Źle:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

`MutableList` można zmienić inplace, a Compose może nie zobaczyć zmiany.

Lepiej:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Dla silniejszej gwarancji można używać immutable collections.

### `@Stable`

Pasuje do state holders albo controller objects, gdzie zmiany przechodzą przez
observable Compose state:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Mutable object jest dopuszczalny, bo Compose widzi zmianę `count`.

Źle:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Zwykłe mutable pole nie informuje Compose o zmianie.

Kiedy czego używać:

- `@Immutable` — UI models, value objects, screen state data classes;
- `@Stable` — custom state holders z `mutableStateOf`, controllers, objects z
  kontrolowaną observable mutacją.

Typowe błędy:

- anotować mutable object jako `@Immutable`;
- stawiać `@Stable` na klasie ze zwykłymi mutable fields;
- trzymać `MutableList/MutableMap` w UI state;
- mutować stary state zamiast tworzyć nowy;
- używać adnotacji jako „leku” na zły state ownership.

**Krótko:** `@Immutable` oznacza „nie zmienia się po utworzeniu”. `@Stable`
oznacza „może się zmieniać, ale Compose poprawnie widzi zmiany”. Adnotację stawia
się tylko wtedy, gdy typ realnie spełnia ten contract.

</details>
<details>
<summary>122. Po co przekazywać key w LazyColumn i czym jest pod spodem?</summary>

#### Kotlin

`key` w `LazyColumn` określa stabilną identity itema niezależnie od jego position.

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

Bez key Compose używa position. To jest normalne dla statycznej listy, ale psuje
się przy insert/remove/reorder.

```text
Było:       Po insert:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

Według position Compose widzi nowy content w starych slots. Według `user.id`
rozumie, że stare items po prostu zmieniły position.

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

Bez stable key `expanded` może zostać na position i przejść do innego user. Key
przenosi remembered/saveable state razem z item identity.

### Pod spodem

Key staje się identity composition group. Runtime używa go, aby:

- dopasować starą i nową group;
- przenieść/reuse group;
- zachować `remember` state;
- odtworzyć `rememberSaveable` state;
- poprawnie działać z item animations.

To nie jest `DiffUtil`: key jest potrzebny do composition/placement bookkeeping w
lazy layout.

### Wymagania dla key

Key powinien być:

- unique wśród sibling items;
- stable między recompositions;
- powiązany z domain identity;
- saveable, jeśli item ma `rememberSaveable`.

Dobrze:

```kotlin
key = { it.id }
```

Źle:

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Random/time tworzy nową identity przy każdej recomposition. Index jest równy
default position identity i nadaje się tylko do naprawdę statycznych list.

Composite key jest dopuszczalny, jeśli stabilny:

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

Dla Paging używa się item key API:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**Krótko:** `key` wiąże lazy item ze stabilną identity, a nie z index. Jest
potrzebny dla dynamic lists, local item state, reorder, animations i Paging.
Najlepszy key to stabilny server/database/client-generated ID.

</details>
<details>
<summary>123. Jakie jeszcze parametry można przekazać do LazyColumn items() i do czego służy contentType?</summary>

#### Kotlin

W `LazyColumn.items()` ważne parametry to `key` i `contentType`.

Podstawowo:

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Dla dynamic list lepiej jawnie przekazać `key`:

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

`key` określa stabilną identity itema. Jest potrzebny, aby Compose nie mylił
remembered state po insert/delete/reorder.

Dobry key:

- unikalny;
- stabilny;
- nie zależy od position;
- nie jest random/time-based;
- bazuje na domain id.

`contentType` opisuje typ UI itema:

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

Na przykład: `header`, `post`, `ad`, `loader`. To pomaga Compose efektywniej
reuse'ować composition dla elementów tego samego typu w mixed list.

Przykład z sealed items:

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

Jeśli potrzebny jest index, istnieje `itemsIndexed`:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

Indexu nie warto używać jako key dla mutable/reorderable list.

Kiedy `contentType` nie jest krytyczny: gdy wszystkie items mają ten sam UI
shape. Największa korzyść jest w mixed feeds, chats, catalogs.

**Krótko:** `key` to stable identity itema. `contentType` to typ/forma UI itema
dla lepszego reuse composition w mixed lists. Dla production dynamic lists `key`
jest prawie zawsze potrzebny, `contentType` — gdy są różne typy wierszy.

</details>
<details>
<summary>124. Czym jest SOLID?</summary>

#### Kotlin

`SOLID` to pięć zasad projektowania OOP, które zmniejszają coupling, poprawiają
testability i sprawiają, że zmiany są bardziej lokalne.

### S — Single Responsibility Principle

Klasa powinna mieć jeden powód do zmiany.

Źle, gdy `ViewModel` robi network, DB, mapping i UI state. Lepiej rozdzielić:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

`ViewModel` zarządza state, use case — akcją biznesową, repository — danymi.

### O — Open/Closed Principle

Kod jest otwarty na rozszerzanie, ale nie wymaga ciągłego edytowania.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Nową implementację można dodać bez przepisywania kodu klienckiego.

### L — Liskov Substitution Principle

Podtyp powinien poprawnie zastępować typ bazowy.

Źle:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Jeśli implementacja nie spełnia kontraktu, kontrakt jest zbyt szeroki albo
niepoprawny.

### I — Interface Segregation Principle

Klient nie powinien zależeć od metod, których nie potrzebuje.

Źle:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Lepiej rozdzielić:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D — Dependency Inversion Principle

Kod wysokiego poziomu zależy od abstractions, a nie concrete implementations.

Źle:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Lepiej:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Wtedy use case łatwo testować przez fake repository.

W Androidzie zwykle wygląda to tak:

```text
UI -> ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

Praktyczna zasada: SOLID nie oznacza interface dla każdej class. Abstrakcja jest
potrzebna tam, gdzie istnieje realny boundary, alternative implementation albo
test seam.

**Krótko:** SOLID pomaga nie mieszać UI, business logic i data layer, unikać god
classes, utrzymywać wąskie contracts i robić kod testowalnym. Ale nadmierne
abstrakcje bez potrzeby też szkodzą.

</details>
<details>
<summary>125. Wyjaśnij każdy принцип SOLID na przykładach z Androida.</summary>

#### Kotlin

SOLID pomaga rozdzielać odpowiedzialności, utrzymywać czyste contracts i testować
logikę biznesową bez Android framework, Retrofit albo Room.

### S — Single Responsibility

Klasa ma jeden powód do zmiany.

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

`ViewModel` zarządza UI state, use case — operacją biznesową, repository —
danymi. SRP nie oznacza „jedna metoda na klasę”, lecz zakaz mieszania różnych
powodów do zmiany.

### O — Open/Closed

Kod rozszerza się nowymi implementacjami, a nie przez ciągłe edytowanie existing
logic.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Nowy handler dodaje się osobną klasą. Ale abstraction jest potrzebna tylko tam,
gdzie istnieje realna wariantowość.

### L — Liskov Substitution

Podtyp powinien poprawnie zastępować typ bazowy.

Źle, gdy `ReadOnlyStorage` implementuje `save()` i rzuca
`UnsupportedOperationException`. Lepiej rozdzielić contract:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP to nie tylko signature, lecz także guarantees, errors, side effects i
invariants.

### I — Interface Segregation

Caller nie powinien zależeć od metod, których nie potrzebuje.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Use case otrzymuje wąski contract zamiast god-interface. Ale zbyt drobne
interfaces bez roli callera to też boilerplate.

### D — Dependency Inversion

High-level business logic zależy od abstraction, a data layer ją implementuje.

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

Use case nie tworzy Retrofit/Room bezpośrednio. DI podstawia real implementation
albo fake.

```text
UI -> ViewModel -> UseCase -> Repository contract
                              ^
                              |
                     Data implementation -> API/DB
```

Typowe naruszenia w Androidzie:

- Fragment bezpośrednio wywołuje API/DAO;
- ViewModel zawiera network, mapping, cache i navigation;
- jedno repository odpowiada za całą app;
- interface tworzy się dla każdej class bez boundary;
- domain zależy od Android/infrastructure types;
- subtype formalnie implementuje interface, ale łamie contract.

**Krótko:** SRP rozdziela odpowiedzialności, OCP — punkty rozszerzeń, LSP —
poprawne zastępowanie podtypów, ISP — wąskie contracts, DIP — zależność business
logic od abstractions. W Androidzie oddziela to UI/domain od framework i data
details.

</details>
<details>
<summary>126. Czym jest OOP?</summary>

#### Kotlin

OOP to podejście, w którym system modeluje się obiektami łączącymi state i
behavior. Klasa opisuje typ, object — jego instance.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Główne zasady: encapsulation, abstraction, inheritance, polymorphism.

### Encapsulation

Klasa ukrywa mutable state i zmienia go przez kontrolowane methods:

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

W ten sposób klasa chroni invariants.

### Abstraction

Kod zależy od contract, a nie implementation:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

`ViewModel` nie wie, czy dane przyszły z Retrofit, Room czy fake.

### Inheritance

W Kotlin klasy są domyślnie `final`. Dziedziczenie dopuszcza się przez `open`:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Inheritance modeluje `is-a`, ale często tworzy silny coupling. Kompozycja jest
zwykle bezpieczniejsza.

### Polymorphism

Różne implementations można podstawiać przez wspólny contract:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

Caller pracuje z `ImageLoader`, dlatego production i test implementations są
wymienne.

W Kotlin OOP dobrze łączy się z `data class`, `sealed class/interface`, extension
functions, immutability i constructor injection. Ważna jest nie liczba
classes/interfaces, lecz jasne responsibilities i boundaries.

**Krótko:** OOP organizuje state i behavior w objects. W Kotlin najważniejsze są
encapsulation, contracts, polymorphism i ostrożne inheritance; często lepsze są
immutable models i composition zamiast głębokiej hierarchii.

</details>
<details>
<summary>127. Czym jest abstrakcja?</summary>

#### Kotlin

Abstrakcja to ukrywanie szczegółów implementacji za zrozumiałym contractem.
Opisuje „co robi kod”, a nie „jak dokładnie”.

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Kod, który używa `UserRepository`, nie wie, czy dane przychodzą z API, Room, cache
albo fake object w teście.

Use case zależy od contract:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Implementation ukrywa szczegóły:

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

Do testów można podstawić fake:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

W Androidzie typowy schemat:

```text
ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

UI nie powinno wiedzieć o Retrofit, Room ani cache strategy.

Abstrakcja nie musi oznaczać interface. Może to być też abstract class, sealed
hierarchy, function type albo po prostu public API klasy. Ale abstract/base
classes w Androidzie często tworzą sztywną hierarchię, dlatego composition jest
zwykle bezpieczniejsza.

Abstrakcja vs enkapsulacja:

```text
Abstrakcja   -> pokazuje ważny contract
Enkapsulacja -> ukrywa internal state/details
```

Zła abstrakcja to interface „na wszelki wypadek”, bez realnej alternatywnej
implementation albo boundary:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

Jeśli istnieje jedna prosta implementacja i nie ma test seam/boundary, interface
może być zbędny.

Dobra abstrakcja:

- ma jasny contract;
- ukrywa niestabilne szczegóły;
- nie przecieka implementation details;
- upraszcza testowanie;
- odpowiada realnemu punktowi zmiany;
- nie dodaje boilerplate bez korzyści.

**Krótko:** abstrakcja oddziela contract od implementation. W Androidzie jest
przydatna na boundaries: network, database, analytics, time, dispatchers,
repositories. Nie trzeba abstrahować każdej klasy bez realnego powodu.

</details>
<details>
<summary>128. Jaka jest różnica między konstruktorami a metodami?</summary>

#### Kotlin

Constructor tworzy instance i doprowadza object do początkowego poprawnego state.
Method opisuje zachowanie już utworzonego object i może być wywoływana wiele razy.

Primary constructor:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Parametry z `val`/`var` stają się properties. Constructor nie ma return type i
wywołuje się go przez `User(...)`.

Method:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

Metodę wywołuje się na instance:

```kotlin
user.displayName()
```

Do validation albo initialization logic używa się `init`:

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

Secondary constructor deleguje do primary przez `this(...)`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

W Kotlin często lepszy jest default parameter:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Constructor często używa się do DI:

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

Constructor określa required dependencies, method wykonuje operation.

Czego nie robić w constructor/init:

- network request;
- database query;
- coroutine launch;
- `runBlocking`;
- ciężki I/O;
- Android lifecycle-dependent logic.

Źle:

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Lepiej:

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**Krótko:** constructor tworzy poprawny object i przyjmuje required
state/dependencies. Method wykonuje zachowanie po construction. Heavy, suspend i
lifecycle-bound work nie uruchamia się w constructor.

</details>
<details>
<summary>129. Czym jest sygnatura metody?</summary>

#### Kotlin

Sygnatura metody to cechy, po których compiler odróżnia jedną function od
drugiej. W Kotlin to przede wszystkim name, receiver i parameter types/order.

```kotlin
fun loadUser(id: String): User
```

W uproszczeniu sygnatura: `loadUser(String)`.

### Overloading

Overloads muszą różnić się parameter list:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Return type nie wystarcza:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflict
```

Parameter name też nie wystarcza:

```kotlin
fun load(id: String)
fun load(userId: String) // conflict
```

Named arguments poprawiają czytelność call-site, ale nie tworzą nowej signature.

Kolejność typów ma znaczenie:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

To jest poprawne, ale często jest złym API przez niską czytelność.

### Default parameters

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

To jedna Kotlin function. Dla Java overloads można użyć `@JvmOverloads`, ale
ręczne mnożenie overloads obok siebie wymaga ostrożności.

### Override

Override musi odpowiadać contractowi base type:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Zmiana parameter type to już inna function, nie override. Return type może być
covariant subtype.

### Receiver i extensions

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

Extension receiver bierze udział w Kotlin resolution. Ale extension function nie
jest virtual override.

### Niuanse JVM

Przez type erasure mogą pojawić się clashes:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // JVM clash: process(List)
```

Nullability też nie tworzy osobnej JVM signature:

```kotlin
fun load(id: String)
fun load(id: String?) // JVM clash
```

Rozwiązanie to inna nazwa albo `@JvmName`, jeśli jest to naprawdę potrzebne.

**Krótko:** w Kotlin functions rozróżnia się po name, receiver i parameter
types/order. Return type i parameter names nie wystarczają. Na JVM dodatkowo
trzeba pamiętać o type erasure, nullability i interop niuansach.

</details>
<details>
<summary>130. Jaka jest różnica między przeciążeniem (overloading) a nadpisaniem (overriding)?</summary>

#### Kotlin

```text
Overloading -> jedna nazwa, różne parameter lists
Overriding  -> ta sama sygnatura, nowa implementacja w subtype
```

### Overloading

Kilka functions w jednym scope ma jedną nazwę, ale różne parametry:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

Compiler wybiera overload według static types argumentów. Return type sam nie może
odróżnić overload:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // compile error
```

W Kotlin część overloadów lepiej zastępować default parameters i named arguments:

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

Subtype implementuje albo zmienia inherited method z tą samą sygnaturą:

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

Zmiana parameter type utworzy inną function, a nie override.

W Kotlin classes i methods są domyślnie `final`. Aby zrobić override, member musi
być `open`, `abstract` albo member interface:

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

Overloading — compile-time/static dispatch:

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

Overriding — runtime/virtual dispatch:

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // CrashlyticsLogger implementation
```

Ważne niuanse:

- overloady z default arguments mogą być ambiguous;
- override musi zachowywać contract typu bazowego;
- return type w override może być covariant;
- extension functions nie są override'owane, resolve'ują się statycznie;
- `final override` zabrania dalszego override.

**Krótko:** overloading to kilka functions z jedną nazwą i różnymi parametrami,
wybór na compile time. Overriding to zastąpienie inherited implementacji z tą
samą sygnaturą, wybór na runtime przez polymorphism.

</details>
<details>
<summary>131. Jaka jest różnica między interfejsem a klasą abstrakcyjną?</summary>

#### Kotlin

Interface opisuje contract/rolę. Abstract class opisuje wspólną bazę z częściową
implementacją, constructorem i state.

```text
interface      -> contract, wiele ról
abstract class -> base implementation, jeden parent class
```

### Interface

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Klasa może implementować wiele interfaces:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Interface dobrze pasuje do DI, testów i dependency inversion:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

W Kotlin interface może mieć default method:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Ale interface nie ma constructora i nie nadaje się do shared mutable state.

### Abstract class

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // shared logic
    }

    abstract suspend fun refresh()
}
```

Klasa abstrakcyjna może mieć:

- constructor;
- state;
- protected methods;
- częściową implementację;
- template method pattern.

Ale klasa może dziedziczyć tylko po jednej class:

```kotlin
class MyRepository : BaseRepository()
```

### Kiedy czego używać

Interface:

- potrzebny jest contract;
- istnieje kilka implementations;
- potrzebny jest fake/mock w testach;
- class ma kilka ról;
- dependency ma być abstrahowana.

Abstract class:

- potrzebna jest shared implementation;
- potrzebny jest protected state;
- istnieje base lifecycle/template;
- constructor jest częścią base contract.

W Androidzie trzeba uważać na `BaseActivity`, `BaseFragment`, `BaseViewModel`:
szybko stają się god-base-class i tworzą sztywną hierarchię.

Często lepsza jest composition:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**Krótko:** interface — do contracts, DI, testów i wielu ról. Abstract class — do
wspólnej implementacji i state, ale z ryzykiem sztywnego inheritance. We
współczesnym Androidzie częściej lepsze jest interface + composition niż głębokie
base classes.

</details>
<details>
<summary>132. Czy interfejs może nie zawierać metod?</summary>

#### Kotlin

Tak. Pusty interface nazywa się marker interface. Nie określa zachowania, lecz
oznacza typ pewną rolą w type system.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Teraz `UserProfile` można przyjmować jako `Cacheable`, sprawdzać przez
`is Cacheable` albo używać jako generic bound.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // only explicitly marked types
}
```

Jeśli do `sync` potrzebny jest `id` albo zachowanie, marker już nie wystarcza —
trzeba dodać property/method do contract.

### Sealed marker

Pusty `sealed interface` często używa się jako root dla zamkniętego zestawu
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

Tutaj to nie tylko marker, lecz sum type: wartość może być jednym z określonych
variants, a compiler sprawdza exhaustive `when`.

Klasyczny przykład marker interface to `java.io.Serializable`: nie ma methods,
ale runtime sprawdza, czy object implementuje ten type.

### Interface czy annotation

Marker interface jest lepszy, gdy potrzebne są:

- subtype relation;
- generic bounds;
- polymorphic API;
- sealed hierarchy;
- `is` check bez reflection.

Annotation jest lepsza dla metadata:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Przetwarza ją compiler plugin, code generator albo runtime reflection. Nie tworzy
subtype relation.

### Kiedy marker jest zły

Nie warto tworzyć marker „na przyszłość”:

```kotlin
interface Important
interface Special
```

Złe oznaki:

- marker nigdzie nie jest używany jako type/bound/root;
- logika ma dużo `is Marker`;
- potrzebne są data/methods, ale kod robi casts;
- rolę lepiej wyrazić enum/property;
- metadata lepiej opisuje annotation.

**Krótko:** interface może być pusty. Marker interface jest przydatny dla subtype
relation, generic bounds albo sealed hierarchies. Jeśli potrzebna jest metadata —
annotation; jeśli potrzebne jest zachowanie — interface powinien mieć members.

</details>
<details>
<summary>133. Czy interfejs może zawierać inny interfejs?</summary>

#### Kotlin

Tak. Interfejs w Kotlin może zawierać inny interfejs. To nested interface.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Użycie:

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // handle change
    }
}
```

Do nested interface odwołujemy się przez `Outer.Inner`, na przykład
`UserRepository.Listener`.

Kiedy jest to właściwe:

- contract jest ściśle powiązany z zewnętrznym API;
- typ zagnieżdżony jest używany tylko w tym kontekście;
- to callback albo mały nested contract;
- top-level typ tylko zaśmiecałby package.

Przykład callback API:

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

Nested interface nie jest `inner` i nie ma dostępu do state instance typu
zewnętrznego:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` nie może bezpośrednio czytać `id`.

Nie mylić z interface inheritance:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

To nie zagnieżdżenie, lecz dziedziczenie contracts.

Nie warto zagnieżdżać interface, jeśli jest samodzielny i używany w całym
projekcie:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

Wtedy zewnętrzny interface staje się sztucznym namespace. Lepsze są top-level
contracts.

**Krótko:** interface może zawierać nested interface. To przydatne dla callbacks
albo contracts istniejących tylko w kontekście zewnętrznego API. Samodzielne
abstractions lepiej robić jako top-level.

</details>
<details>
<summary>134. Dlaczego nie warto dodawać do interfejsu dziesiątek metod?</summary>

#### Kotlin

Duży interface zwykle narusza Interface Segregation Principle: klienci zależą od
metod, których nie potrzebują. Zwiększa to coupling, komplikuje implementations i
testy.

Źle:

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

Jeśli use case potrzebuje tylko `getUser`, nadal zależy od całego contractu.

Problem z implementation:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Jeśli implementation nie może uczciwie zaimplementować metody, contract jest
niepoprawny albo zbyt szeroki.

Problem z testami:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // zbędny szum
}
```

Fake/mock są zmuszone implementować niepotrzebne metody.

Lepiej rozdzielić role:

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

Use case zależy tylko od potrzebnego contractu:

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

Duży interface jest dopuszczalny tylko wtedy, gdy to naprawdę jedna cohesive
abstraction. Na przykład DAO dla jednej tabeli może mieć kilka related queries.
Ale jeśli metody mają różnych clients i różne reasons to change, lepiej je
rozdzielić.

**Krótko:** interface powinien opisywać rolę, a nie „wszystko, co umie system”.
Jeśli klient używa 1–2 metod z 20, contract jest zbyt szeroki. Lepiej rozdzielać
reader/writer/uploader/etc według realnych ról.

</details>

<details>
<summary>135. Czym jest kompozycja i czym różni się od dziedziczenia?</summary>

#### Kotlin

Composition — klasa otrzymuje zachowanie przez zależności. Inheritance — klasa
dziedziczy zachowanie po klasie bazowej.

```text
Inheritance -> is-a
Composition -> has-a / uses-a
```

### Inheritance

Jest właściwe, gdy subclass naprawdę jest specjalizacją base type:

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

`Dog` jest `Animal`, więc relacja `is-a` jest logiczna.

### Composition

Klasa używa innych objects zamiast inheritance:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

`Dog` ma `SoundPlayer`, ale nie jest `SoundPlayer`.

W Android/Kotlin composition często wygląda tak:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

Zależności są jawne i łatwo zastąpić je fake/mock w testach.

### Problem base classes

Duże `BaseViewModel`, `BaseRepository`, `BaseFragment` często stają się god base
classes:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Klasy potomne otrzymują methods/state, których nie potrzebują. Zmiana base class
może zepsuć wiele unrelated screens.

### Kotlin delegation

Kotlin ma delegation jako wygodną formę composition:

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

`UserService` deleguje `Logger` innemu objectowi, nie dziedzicząc po nim.

Kiedy czego używać:

- inheritance — realne `is-a`, stabilny base contract;
- composition — behavior trzeba podstawiać, testować, łączyć albo zmieniać;
- Android app code — zwykle composition jest lepsza niż deep inheritance.

**Krótko:** inheritance modeluje `is-a`, composition — `has-a/uses-a`. W
Kotlin/Android composition zwykle jest lepsza: jawne dependencies, prostsze
testy, mniej coupling i brak kruchych hierarchii base-class.

</details>

<details>
<summary>136. Jakie są modyfikatory dostępu w Kotlinie?</summary>

#### Kotlin

W Kotlinie są cztery visibility modifiers:

```text
public    -> dostępny zewsząd
internal  -> dostępny w obrębie module
protected -> dostępny w class i subclasses
private   -> dostępny w bieżącym scope albo pliku
```

### `public`

`public` — domyślna widoczność:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Wszystko, co public, staje się częścią API, dlatego implementation details nie
powinno się bez potrzeby ujawniać.

### `private`

Member jest dostępny tylko wewnątrz class/object:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Top-level `private` jest dostępny w obrębie jednego pliku Kotlin:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### `protected`

Dostępny w class i subclasses:

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}
```

W Kotlinie `protected` nie daje package access, w przeciwieństwie do Javy, i nie
jest dozwolony na top-level. To część inheritance API.

### `internal`

Dostępny w obrębie Kotlin module:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Module — to compilation boundary, na przykład Gradle module, a nie package.

`internal` jest przydatny w multi-module Android: jako public zostawiamy tylko
API modułu, a implementation — `internal`.

Ważne: `internal` to compile-time boundary, nie security. W bytecode Java/reflection
mogą zobaczyć więcej.

### Widoczność settera i konstruktora

Property może być public do odczytu i private do zapisu:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set
}
```

Constructor również może mieć visibility:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Praktyczne zasady:

- zaczynać od najwęższej wystarczającej visibility;
- implementation trzymać jako `private` albo `internal`;
- public API robić minimalnym;
- `protected` używać tylko dla realnego inheritance contract;
- visibility nie uważać za security boundary.

**Krótko:** `public` — dla wszystkich, `internal` — module, `protected` —
inheritance hierarchy, `private` — class/file scope. Dobre Kotlin API ujawnia
minimum tego, co potrzebne.

</details>

<details>
<summary>137. Czym jest singleton?</summary>

#### Kotlin

Singleton — to jeden instance w obrębie określonego ownera: classloader/process
albo DI component. W Androidzie ważne jest, aby pamiętać: process może zostać
zabity, a multiprocess app będzie mieć osobny singleton w każdym process.

### Kotlin `object`

```kotlin
object AppLogger {
    fun log(message: String) = println(message)
}
```

`object` inicjalizuje się lazy i thread-safe przy pierwszym użyciu. Ale mutable
state wewnątrz nie staje się automatycznie thread-safe:

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

Companion — singleton powiązany z klasą. Sama klasa może mieć wiele instances.
Zwykle companion używa się do factory methods i constants.

### Singleton przez DI

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
)
```

W Hilt `@Singleton` oznacza jeden instance w `SingletonComponent`, a nie
„wieczny object”. DI jest lepsze niż global singleton, gdy są dependencies:

- constructor dependencies są jawne;
- implementation można zastąpić fake;
- lifetime kontroluje component;
- caller zależy od contract.

Dla krótszego lifecycle istnieją `@ViewModelScoped`, `@ActivityScoped` itd.

### Ryzyka

Źle:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Problemy:

- Activity leak;
- global mutable state;
- race conditions;
- trudne testy przez shared state;
- niejasny logout/reset;
- utrata state po process death.

App-level singleton nie powinien trzymać `Activity`, `Fragment`, `View` ani
`ActivityContext`. Jeśli potrzebny jest context — tylko application context.

Session/token nie warto przechowywać tylko w memory singleton: potrzebne są
bezpieczne persistence, restore i explicit clear.

Singleton jest właściwy dla:

- Room database;
- OkHttp client;
- logger/analytics;
- stateless app-wide services;
- immutable configuration.

Nie jest właściwy dla screen state, ViewModel, user session bez resetu ani
dependency o krótkim lifecycle.

**Krótko:** w Kotlinie singleton często robi się przez `object`, a w Androidzie
dependencies lepiej scope'ować przez DI. Singleton pasuje do app-wide services,
ale global mutable state i UI references prawie zawsze są problemem.

</details>

<details>
<summary>138. Czym są generics?</summary>

#### Kotlin

Generics pozwalają parametryzować class, interface albo function typem i
zachować compile-time type safety.

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` — type parameter, `String`/`Int` — type arguments. Compiler zna typ `value`,
więc unsafe cast nie jest potrzebny.

Generic function:

```kotlin
fun <T> singleItemList(item: T): List<T> = listOf(item)
```

### Bounds

Type parameter można ograniczyć:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Kilka bounds:

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

`Nothing` jest subtype wszystkich typów Kotlin, dlatego `Error` i `Loading`
można używać jako `Result<User>`, `Result<List<Post>>` itd.

### Variance

Domyślnie generic type jest invariant. Nawet jeśli `Cat : Animal`,
`MutableList<Cat>` nie jest `MutableList<Animal>`.

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

Model: producer — `out`, consumer — `in`. Jeśli type zarówno przyjmuje, jak i
zwraca `T`, zwykle jest invariant.

### Type erasure i reified

Na JVM generic type arguments w większości są usuwane w runtime:

```kotlin
value is List<*> // można
// value is List<String> // nie można bezpośrednio
```

`inline reified` daje dostęp do type parameter w runtime:

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` jest możliwy tylko dla inline functions.

Praktyczne zasady:

- bounds określają minimalny contract;
- `out/in` opisują bezpieczne subtype-relacje;
- mutable containers częściej są invariant;
- dla runtime generic type potrzebny jest `reified` albo explicit type token;
- nie komplikować API generics bez realnej korzyści.

**Krótko:** generics dają type-safe reusable API. Bounds ograniczają dopuszczalne
typy, variance (`out/in`) steruje subtype-zgodnością, a `reified` częściowo
omija JVM type erasure dla inline functions.

</details>

<details>
<summary>139. Czym jest type erasure i dlaczego powstaje?</summary>

#### Kotlin

Type erasure oznacza, że na JVM concrete generic arguments zwykle nie są
przechowywane w runtime object. `List<String>` i `List<Int>` w runtime to po
prostu `List`.

Powód to backward compatibility Java generics ze starym bytecode. Type parameter
jest usuwany do upper bound albo `Object`. Kotlin/JVM działa w tym samym JVM
model.

Nie można bezpośrednio sprawdzić:

```kotlin
if (value is List<String>) { } // erased type
```

Można sprawdzić container:

```kotlin
if (value is List<*>) {
    // element type unknown
}
```

`List<*>` pozwala bezpiecznie czytać elements jako `Any?`, ale nie gwarantuje
ich konkretnego typu.

Unchecked cast nie waliduje wszystkich elements:

```kotlin
val strings = value as List<String> // unchecked
```

Jeśli potrzebna jest realna validation, trzeba sprawdzać elements:

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String ?: error("Expected String")
    }
```

### `reified`

Zwykłe `T` nie jest dostępne dla `is T`. Ale inline function może mieć `reified`
type parameter:

```kotlin
inline fun <reified T> isType(value: Any): Boolean =
    value is T
```

Wtedy dostępne są `T::class`, `is T` i class-token APIs. Ale `reified` nie
odtwarza nested generics w pełni: `List<String>` nadal wymaga element check albo
serializer/type token.

Bez `reified` type przekazuje się jawnie:

```kotlin
fun <T> decode(json: String, clazz: Class<T>): T = TODO()

val user = decode(json, User::class.java)
```

Dla `List<User>` samo `Class<List>` nie wystarczy, dlatego libraries używają
`Type`, `KType`, generated adapter albo `KSerializer<T>`.

Type erasure tworzy też JVM signature clashes:

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {} // clash: handle(List)
```

Potrzebne są różne names albo ostrożne `@JvmName`.

**Krótko:** type erasure usuwa generic arguments w runtime przez JVM/Java
compatibility. Do runtime checks używa się `List<*>`, `inline reified`, explicit
type token albo serializer; unchecked cast nie jest pełną weryfikacją typów.

</details>

<details>
<summary>140. Jakie istnieją kolekcje: List, Set, Map, Queue, Stack?</summary>

#### Kotlin

Kolekcję wybiera się według semantyki: kolejność, unikalność, lookup po kluczu,
FIFO albo LIFO.

### `List`

Uporządkowana kolekcja z dostępem przez index i dozwolonymi duplikatami:

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

Typowa mutable implementacja na JVM to `ArrayList`: index access `O(1)`, search
`O(n)`, insert/remove w środku `O(n)`.

### `Set`

Kolekcja unikalnych elementów:

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* exists */ }
```

`HashSet` zwykle ma average `O(1)` dla `contains/add/remove`, ale wymaga
poprawnych `equals()` i `hashCode()`.

### `Map`

Struktura key-value, w której keys są unikalne:

```kotlin
val usersById: Map<String, User> = users.associateBy(User::id)
val user = usersById["42"]
```

`HashMap.get()` średnio ma `O(1)`. Map nadaje się do cache, index, lookup table.
Mutable key jest niebezpieczny, jeśli zmieniają się pola wpływające na
`hashCode()`.

### `Queue` i `Stack`

Queue — FIFO:

```text
first in -> first out
```

Stack — LIFO:

```text
last in -> first out
```

W Kotlinie wygodnie używać `ArrayDeque`:

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

Operacje na końcach mają amortized `O(1)`. `ArrayDeque` jest lepsze niż legacy
Java `Stack`.

### Read-only vs mutable

Kotlin rozdziela API:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` nie ma `add/remove`, ale to nie jest deep immutability: underlying object
albo elements mogą być mutable. Shared mutable state trzeba enkapsulować.

Praktyczny wybór:

- kolejność, index, duplikaty — `List`;
- unikalność i membership check — `Set`;
- lookup po key — `Map`;
- FIFO — `ArrayDeque.removeFirst()`;
- LIFO — `ArrayDeque.removeLast()`;
- priority order — `PriorityQueue`.

**Krótko:** `List` — kolejność i duplikaty, `Set` — unikalność, `Map` — lookup
po kluczu, Queue — FIFO, Stack — LIFO. W Kotlinie dla Queue/Stack często
wystarczy `ArrayDeque`.

</details>

<details>
<summary>141. Czym jest thread-safety kolekcji?</summary>

#### Kotlin

Thread-safe collection zachowuje poprawny state przy concurrent access. Zwykłe
`MutableList`, `MutableMap`, `MutableSet` w Kotlin/JVM nie są thread-safe.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Concurrent writes mogą dać lost update, niepoprawny internal state albo exception.

### Read-only ≠ immutable

```kotlin
val users: List<User> = mutableListOf()
```

`List` zabrania mutation tylko przez ten reference. Underlying collection albo
elements mogą być mutable.

### Główne strategie

1. **Jeden owner/thread**

Wszystkie read/write przechodzą przez jednego ownera: Main thread, single
dispatcher, actor-like component. To najprostszy model ownership.

2. **`Mutex` dla coroutines**

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

Wszystkie accesses muszą przechodzić przez ten sam `Mutex`. Wewnątrz lock nie
robi się długiej network/suspend work.

3. **Concurrent collections**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` nadaje się do concurrent keyed access. Ale compound operation
nie staje się automatycznie atomowa — do tego potrzebne są atomic methods typu
`computeIfAbsent` albo osobna synchronizacja.

`CopyOnWriteArrayList` nadaje się do listeners: dużo reads, rzadkie writes.

4. **Immutable snapshots**

Dla UI state lepiej tworzyć nową collection:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Tutaj atomowość daje `_state.update`, a nie sama `List`.

### Iteration

Mutation podczas iteration może dać `ConcurrentModificationException` nawet w
jednym thread. Jeśli trzeba zmienić collection podczas przechodzenia — należy
użyć iteratora, collection operation albo snapshot.

Praktyczny wybór:

- coroutine state — `Mutex` albo serialized owner;
- concurrent keyed access — `ConcurrentHashMap`;
- read-heavy listeners — `CopyOnWriteArrayList`;
- UI state — immutable snapshots + atomic `update`;
- compound invariants — jedna critical section.

**Krótko:** mutable collections nie są thread-safe. Read-only API nie oznacza
immutability. Dla bezpieczeństwa potrzebne są ownership, `Mutex`, concurrent
collections albo immutable snapshots; synchronizować trzeba cały invariant, a
nie pojedyncze wywołanie.

</details>

<details>
<summary>142. Jaka jest różnica między final, finally i finalize?</summary>

#### Kotlin

To trzy różne pojęcia:

```text
final    -> zakaz inheritance/override
finally  -> blok cleanup po try/catch
finalize -> przestarzały GC callback
```

### `final`

W Javie `final` zabrania dziedziczenia class, override method albo ponownego
przypisania variable.

W Kotlinie classes i methods są domyślnie `final`. Do inheritance potrzebne jest
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

`final override` pozwala override zrobić raz i zabrania dalszego nadpisywania.

Dla variables/properties Kotlin używa `val`:

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex")
```

`val` zabrania ponownego przypisania reference, ale nie robi object immutable.

### `finally`

`finally` wykonuje się po `try/catch` dla cleanup:

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Zastosowanie: zamknąć resource, unlock, unregister listener, zresetować
tymczasowy state.

W coroutines `finally` wykonuje się również przy cancellation:

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

Jeśli cleanup ma suspend-funkcję w cancelled coroutine, czasem potrzebny jest
krótki `withContext(NonCancellable)`. Ale to powinien być wyjątek, nie default.

`finally` nie jest gwarantowane przy kill process/JVM crash.

### `finalize`

`finalize()` — stary Java `Object` callback, który GC mógł wywołać przed
zebraniem objectu. Nie używa się go do cleanup, ponieważ:

- nie wiadomo, kiedy zostanie wywołany;
- może nie zostać wywołany przed zakończeniem process;
- komplikuje GC;
- jest deprecated/legacy mechanism.

Zamienniki:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

W Androidzie cleanup wiąże się z lifecycle:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

W Compose — przez `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**Krótko:** `final` ogranicza inheritance/override, `finally` robi
deterministyczny cleanup po `try/catch`, `finalize()` to deprecated i
niewiarygodny GC-mechanizm, którego nie należy używać.

</details>

<details>
<summary>143. Jak działa try-catch-finally?</summary>

#### Kotlin

`try-catch-finally` rozdziela ryzykowną operację, obsługę exception i cleanup.

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Kolejność:

```text
success -> try -> finally
error   -> try -> matching catch -> finally
```

Jeśli nie ma matching `catch`, exception pójdzie do callera po wykonaniu
`finally`.

### `catch`

Można łapać różne typy osobno:

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

`catch` są sprawdzane od góry do dołu, dlatego bardziej konkretne typy powinny
być przed ogólniejszymi.

Kotlin nie ma checked exceptions, dlatego compiler nie zmusza do pisania
`try/catch`. Exception należy łapać tam, gdzie kod może sensownie się odtworzyć,
przekształcić błąd albo dodać context.

### `try` jako expression

`try` może zwracać wartość:

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

`finally` nie definiuje result expression.

### `finally`

`finally` używa się do cleanup:

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

Wykonuje się przy success, exception, `return` i coroutine cancellation. Nie
należy robić `return` w `finally`, bo może nadpisać result albo exception.

Dla `Closeable`/`AutoCloseable` lepsze jest `use`:

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

`CancellationException` nie wolno połykać — trzeba ją rethrow, inaczej zepsuje
się cooperative cancellation.

`finally` w cancelled coroutine wykona się, ale suspend-cleanup może od razu
dostać cancellation. Dla krótkiego obowiązkowego suspend cleanup czasem używa
się `withContext(NonCancellable)`.

**Krótko:** `try` wykonuje operację, `catch` obsługuje matching exception,
`finally` robi cleanup niezależnie od wyniku. W coroutines ważne jest, aby nie
połykać `CancellationException`, a resources lepiej zamykać przez `use`.

</details>

<details>
<summary>144. Jaka jest różnica między checked i unchecked exceptions?</summary>

#### Kotlin

W Javie checked exception trzeba albo przechwycić, albo zadeklarować przez
`throws`. Dla unchecked takiego wymagania nie ma. W Kotlinie checked exceptions
nie ma — compiler nie zmusza do pisania `try/catch`.

### Java

Checked exception:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

Caller musi zrobić `try/catch` albo również zadeklarować `throws`.

Unchecked exception — to `RuntimeException` i subclasses:

```java
String value = null;
value.length(); // NullPointerException
```

Compiler nie wymaga obsługi `NullPointerException`, `IllegalArgumentException`,
`IllegalStateException`.

`Error` też jest unchecked, ale zwykle nie jest przeznaczony do recovery.

### Kotlin

Kotlin pozwala wywoływać Java/Kotlin API, które mogą rzucić `IOException`, bez
obowiązkowego catch:

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

Exception nadal może wystąpić w runtime, dlatego error contract trzeba jasno
pokazywać przez API, dokumentację albo return type.

### Expected outcome vs programming error

Praktycznie lepiej rozdzielać:

- expected/recoverable: offline, invalid credentials, not found;
- programming error: invalid argument, impossible state, index error.

Dla contract violation właściwy jest exception:

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

Dla oczekiwanych domain outcomes lepszy jest sealed result:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

Wtedy caller widzi znane stany przez type system.

### Java interop

Aby Java caller widział `throws`, w Kotlinie używa się `@Throws`:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

Dla Kotlin callera catch nadal jest nieobowiązkowy.

### Coroutines

Exceptions w coroutines też są unchecked. `launch` przekazuje failure przez job
hierarchy, `async` zwraca exception przez `await()`.

`CancellationException` — to cancellation signal, którego nie należy połykać:

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

**Krótko:** checked exceptions to javowy mechanizm obowiązkowego `catch/throws`;
unchecked compiler nie kontroluje. W Kotlinie checked exceptions nie ma, dlatego
oczekiwane błędy lepiej modelować przez return types, a contract violations —
przez exceptions.

</details>

<details>
<summary>145. Jakie istnieją typy błędów w Kotlin/Java?</summary>

#### Kotlin

Na JVM wszystko, co można rzucić przez `throw`, dziedziczy po `Throwable`. Dwie
główne gałęzie to `Exception` i `Error`.

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

Kotlin używa tej samej hierarchii JVM, ale nie ma checked exceptions na poziomie
compilera.

### `Throwable`

Typ bazowy dla wszystkich exceptions/errors. Łapanie `Throwable` zwykle jest
niebezpieczne: można przechwycić fatalny `Error` albo coroutine cancellation.

### `Exception`

Sytuacje, które application code potencjalnie może obsłużyć:

- `IOException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

### `RuntimeException`

Unchecked exception, często oznacza programming/contract error:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Przykłady: `NullPointerException`, `IndexOutOfBoundsException`,
`ClassCastException`, `NumberFormatException`.

### Checked vs unchecked

Java wymaga `catch` albo `throws` dla checked exceptions, na przykład
`IOException`. Kotlin tego nie wymaga. `RuntimeException` i subclasses są
unchecked w obu językach.

### `Error`

Poważny problem JVM/runtime:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Zwykle application code nie powinien próbować recover po `Error`.

### Domain errors

Oczekiwane business failures lepiej modelować osobnym typem, a nie exception:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Techniczne exceptions mapuje się na domain errors na granicy data layer:

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

### `CancellationException`

W coroutines to sygnał cancellation, a nie zwykła failure:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Nie należy jej połykać w generic `catch`.

**Krótko:** `Throwable` dzieli się na `Exception` i `Error`. Runtime exceptions
często oznaczają contract/programming errors, expected business failures lepiej
modelować domain-typami, a `CancellationException` w coroutines trzeba
przepuszczać dalej.

</details>

<details>
<summary>146. Jaka jest klasa bazowa dla błędów?</summary>

#### Kotlin

Klasą bazową dla wszystkiego, co można rzucić przez `throw` i złapać przez
`catch`, jest `Throwable`.

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

`Throwable` zawiera podstawowe informacje:

- message;
- cause;
- stack trace;
- suppressed exceptions.

Rzucać można tylko `Throwable` albo jego subclass:

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // compile error
```

`Exception` — problemy, które application code potencjalnie może obsłużyć:

```kotlin
throw IOException("No internet")
throw IllegalStateException("User is not authorized")
```

`Error` — poważne problemy JVM/runtime:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Zwykle nie łapie się ich do recovery.

Custom exception:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

Dla oczekiwanych business outcomes często lepszy jest sealed type, a nie
exception:

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

Nie warto bez potrzeby łapać `Throwable`:

```kotlin
catch (throwable: Throwable) {
    log(throwable)
}
```

Tak można przechwycić `Error` albo `CancellationException`. W coroutines
cancellation trzeba przepuszczać dalej.

**Krótko:** klasa bazowa to `Throwable`. Od niej pochodzą `Exception` i `Error`.
W production lepiej łapać konkretne recoverable exceptions, a nie cały
`Throwable`.

</details>

<details>
<summary>147. Czym jest złożoność algorytmów?</summary>

#### Kotlin

Złożoność algorytmu opisuje, jak rośnie czas wykonania albo dodatkowa pamięć
wraz ze wzrostem input `n`. Najczęściej używa się Big O.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

Na rozmowie kwalifikacyjnej ważne jest doprecyzowanie: average, worst-case albo
amortized complexity.

### `O(1)`

Koszt nie rośnie wraz z `n`:

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

Index access w `ArrayList` — `O(1)`. Lookup w `HashMap` — `O(1)` average.

### `O(n)`

Jeden przebieg po collection:

```kotlin
fun findUser(users: List<User>, id: String): User? =
    users.firstOrNull { it.id == id }
```

`map`, `filter`, `contains` dla list — zwykle `O(n)`.

### `O(n²)`

Zagnieżdżone porównywanie wielu par:

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

Dla duplicates lepszy jest `Set`:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = HashSet<String>()
    return items.any { !seen.add(it) }
}
```

Time — `O(n)` average, space — `O(n)`. To trade-off pamięci za szybkość.

### `O(log n)`

Binary search po sorted data:

```kotlin
val index = sortedItems.binarySearch(target)
```

Search — `O(log n)`, ale sorting wcześniej może kosztować `O(n log n)`.

### `O(n log n)`

Typowa złożoność comparison sorting:

```kotlin
val sorted = users.sortedBy(User::name)
```

### Time vs space

```kotlin
val copy = users.toList()
```

Time — `O(n)`, dodatkowa memory — `O(n)`. In-place algorithm może oszczędzać
memory, ale zmienia input i komplikuje ownership/concurrency.

### Przykład z Androida

Źle:

```kotlin
users.map { user ->
    user.id in selectedIds // selectedIds: List, contains O(m)
}
```

Razem `O(n × m)`.

Lepiej:

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

Budowa Set — `O(m)`, mapping — `O(n)`, razem `O(n + m)` average.

Big O nie równa się realnej performance: constants, allocations, I/O, cache
locality i main-thread budget też są ważne. Po wyborze właściwej struktury
danych wynik trzeba sprawdzić profilerem/benchmarkiem.

**Krótko:** złożoność pokazuje skalowanie time/space. `O(n²)` często można
poprawić przez `Set/Map`, ale trzeba uwzględniać pamięć, average/worst case i
realne pomiary.

</details>

<details>
<summary>148. Czym jest przeszukiwanie w głąb (DFS)?</summary>

#### Kotlin

`DFS` (`Depth-First Search`) — algorytm przechodzenia drzewa albo grafu, który
idzie w głąb jedną ścieżką tak długo, jak może, potem cofa się i próbuje innych
gałęzi.

Przykład przejścia drzewa:

```text
        A
      /        B     C
    / \        D   E     F
```

Możliwa kolejność DFS:

```text
A -> B -> D -> E -> C -> F
```

### DFS dla drzewa

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

Dla drzewa `visited` zwykle nie jest potrzebny, bo nie ma cykli.

### DFS dla grafu

W grafie mogą być cykle, dlatego potrzebny jest `visited`:

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

Bez `visited` DFS może się zapętlić.

### Iteracyjny DFS

Rekursję można zastąpić stackiem:

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

`ArrayDeque` jest używany jako stack przez LIFO: `addLast/removeLast`.

### Złożoność

Dla grafu:

```text
Time:  O(V + E)
Space: O(V)
```

`V` — vertices, `E` — edges. Space jest potrzebny dla `visited` i recursion
stack albo explicit stack.

### DFS vs BFS

DFS:

- idzie w głąb;
- używa recursion albo stack;
- jest przydatny do cycle detection, connected components, topological sort;
- nie gwarantuje shortest path w unweighted graph.

BFS:

- idzie poziomami;
- używa queue;
- znajduje shortest path w unweighted graph.

Recursive DFS może dać `StackOverflowError` na bardzo głębokiej strukturze.
Wtedy lepszy jest iterative DFS.

**Krótko:** DFS to przechodzenie w głąb przez recursion albo stack. Dla grafu
potrzebny jest `visited`, złożoność to `O(V + E)`, a dla bardzo głębokich
struktur lepszy jest wariant iterative.

</details>

<details>
<summary>149. Czym jest przeszukiwanie wszerz (BFS)?</summary>

#### Kotlin

`BFS` (`Breadth-First Search`) — algorytm przechodzenia grafu albo drzewa poziom
po poziomie. Najpierw odwiedza wszystkie wierzchołki w odległości 1 od startu,
potem 2, potem 3 i tak dalej.

BFS używa queue z zachowaniem FIFO:

```text
start -> neighbors -> neighbors of neighbors -> ...
```

### BFS dla grafu

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

`visited` jest potrzebny, aby nie zapętlić się w grafie z cycles.

### Shortest path w unweighted graph

BFS znajduje minimalną liczbę krawędzi od startu:

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

Dla weighted graph potrzebny jest Dijkstra, a nie BFS.

### BFS dla drzewa

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

To przejście po poziomach.

### Złożoność

```text
Time:  O(V + E)
Space: O(V)
```

`V` — vertices, `E` — edges. Dla drzewa — `O(n)`.

### BFS vs DFS

```text
BFS -> queue, poziom po poziomie, shortest path w unweighted graph
DFS -> stack/recursion, idzie w głąb
```

BFS często pasuje do zadań: „najmniejsza liczba kroków”, „najbliższy element”,
„minimalna odległość w grid bez wag”.

**Krótko:** BFS przechodzi graf/drzewo wszerz przez queue. Znajduje shortest
path w unweighted graph, wymaga `visited` dla grafów z cycles i ma złożoność
`O(V + E)`.

</details>

<details>
<summary>150. Opowiedz o Clean Architecture.</summary>

#### Kotlin

`Clean Architecture` — podejście do dzielenia kodu na warstwy z jasnym
dependency direction. Główna idea: logika biznesowa nie zależy od UI, Android
framework, database ani network client.

Typowy schemat Android:

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

Domain nie powinien wiedzieć o Retrofit, Room, `Context`, Compose ani Android
lifecycle.

### Use case

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

ViewModel wywołuje use case, a nie bezpośrednio API/DAO.

### Repository contract

W domain:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

W data:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Data layer pracuje z DTO/entities i mapuje je na domain:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

DTO i Room entities nie warto oddawać bezpośrednio do UI, jeśli mają inne
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

ViewModel zarządza UI state i nie wie, skąd dokładnie przychodzą dane.

Zalety:

- business logic testuje się bez Androida;
- data source można zastąpić;
- UI nie jest przywiązany do API/DB;
- boundaries są zrozumiałe dla zespołu;
- łatwiej utrzymywać duży produkt.

Wady:

- więcej plików i boilerplate;
- dla prostego CRUD może być overengineering;
- złe abstractions komplikują kod;
- potrzebna jest dyscyplina w dependency direction.

Praktyczna zasada: nie tworzyć use case/interface dla każdego trivial getter.
Clean Architecture ma zmniejszać coupling i poprawiać testowalność, a nie tylko
mnożyć klasy.

**Krótko:** Clean Architecture w Androidzie zwykle dzieli kod na presentation,
domain i data. Domain zawiera business rules i contracts, presentation renderuje
state, data implementuje API/DB. Główny cel to kontrola zależności i
testowalność.

</details>

<details>
<summary>151. Czym jest MVVM?</summary>

#### Kotlin

`MVVM` — architektoniczny pattern `Model-View-ViewModel`, który oddziela UI od
state management i logiki biznesowej.

```text
View -> ViewModel -> Model
```

- `View` — renderuje UI i wysyła user actions;
- `ViewModel` — trzyma screen state, obsługuje actions, wywołuje use cases;
- `Model` — domain/data layer: use cases, repositories, API, DB.

### View

Compose UI powinno otrzymywać state i callbacks:

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

View nie chodzi bezpośrednio do API/DB i nie zawiera logiki biznesowej.

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

ViewModel przeżywa configuration changes i nie zależy od konkretnej View
implementation.

### Model layer

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

Repository ukrywa API, Room, cache, DataStore.

### Data flow

```text
User action -> ViewModel -> UseCase/Repository -> State update -> View renders state
```

Screen state lepiej robić jawnie:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

Navigation, snackbar, toast — to one-shot effects, nie warto ich przechowywać
jako persistent state:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Zalety:

- mniej logiki w View;
- ViewModel łatwo testować;
- state przeżywa rotation;
- UI można zmieniać bez przepisywania domain/data;
- dobrze działa z Compose, XML, StateFlow/LiveData.

Typowe błędy:

- Fragment/Composable zawiera logikę biznesową;
- ViewModel bezpośrednio pracuje z Retrofit/Room bez boundary;
- DTO/entity są oddawane bezpośrednio do UI bez powodu;
- dużo niezależnych mutable states zamiast screen state;
- one-shot events są przechowywane jako persistent state.

**Krótko:** MVVM: View renderuje state, ViewModel zarządza state/actions,
Model/use cases/repositories wykonują logikę biznesową i data access. Dobry
MVVM robi UI cienkim, a ViewModel testowalną.

</details>

<details>
<summary>152. Jaka jest różnica między MVVM i MVI?</summary>

#### Kotlin

`MVVM` i `MVI` — podejścia do UI state management. Oba oddzielają UI od logiki
biznesowej, ale inaczej organizują state, events i side effects.

### MVVM

```text
View -> ViewModel -> Model
```

- `View` renderuje UI i wywołuje methods ViewModel;
- `ViewModel` trzyma state i wywołuje use cases/repositories;
- `Model` — domain/data layer.

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

MVVM jest prostszy i bardziej elastyczny. Ale bez dyscypliny ViewModel może
zamienić się w zestaw przypadkowych methods i mutable states.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
                                 └-> Effect
```

UI wysyła events/intents, a nie wywołuje wielu różnych methods:

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

Zwykle istnieje jeden immutable screen state:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

Entry point często jest jeden:

```kotlin
fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.LoadClicked -> load()
        ProfileEvent.RetryClicked -> retry()
    }
}
```

### State i effects

W MVVM state może być jednym object albo kilkoma streams. W MVI zwykle jest
jeden screen state i formalny reducer/handler.

W obu podejściach navigation/snackbar/toast nie należy przechowywać jako
persistent state. Do tego używa się effects:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

### Kiedy co wybrać

MVVM:

- prostszy;
- mniej boilerplate;
- dobrze pasuje do większości Android screens;
- łatwiejszy start dla zespołu.

MVI:

- predictable unidirectional data flow;
- jeden immutable state;
- wygodnie testować event/reducer logic;
- lepszy dla złożonych stateful screens;
- przydatny dla dużych zespołów z jednolitym flow.

Wadą MVI jest boilerplate dla prostych ekranów. Wadą MVVM jest ryzyko
chaotycznego state management.

**Krótko:** MVVM jest prostszy: View wywołuje methods ViewModel i renderuje
state. MVI jest bardziej formalny: UI wysyła events, ViewModel/reducer tworzy
immutable state, one-shot akcje idą przez effects. MVVM — dla większości ekranów,
MVI — dla złożonych stateful flows.

</details>

<details>
<summary>153. Jakie wzorce architektoniczne są używane w Androidzie?</summary>

#### Kotlin

W Androidzie najczęściej używa się MVVM, MVI, Clean Architecture, Repository,
Use Case, layered architecture, DI i feature modularization. Wybór zależy od
złożoności app, zespołu i wymagań dotyczących testowalności.

### MVVM

```text
View -> ViewModel -> Model
```

UI renderuje state, ViewModel obsługuje user actions i wywołuje domain/data
layer.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Dobrze działa z Compose, XML, StateFlow i LiveData.

### MVI

```text
Event/Intent -> ViewModel/Reducer -> State -> UI
```

UI wysyła events, ViewModel tworzy immutable state, one-shot akcje idą przez
effects.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

Przydatny dla złożonych stateful screens.

### Clean Architecture / Layered Architecture

```text
presentation -> domain <- data
```

- `presentation` — UI/ViewModel;
- `domain` — use cases, business rules, contracts;
- `data` — repository implementations, API, DB.

Domain nie powinien zależeć od Android framework, Retrofit ani Room.

### Repository pattern

Repository ukrywa data sources:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Implementation może pracować z API, Room, cache albo DataStore, ale ViewModel
tego nie wie.

### Use Case pattern

Use case opisuje konkretną akcję biznesową:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

To nie pozwala ViewModel rozrosnąć się logiką biznesową.

### Feature modularization

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Feature-first modules pomagają skalować codebase i ownership. Core powinien
zawierać naprawdę shared infrastructure.

### Dependency Injection

DI nie jest UI pattern, ale jest częścią architektury:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt/Koin/manual DI łączy contracts z implementations.

Praktyczny wybór:

- prosty app — MVVM + Repository;
- złożony screen state — MVI;
- duży app — Clean Architecture + modularization;
- offline-first — Repository + local source of truth;
- dużo zespołów — feature modules + jasne contracts.

**Krótko:** Android architecture zwykle łączy MVVM/MVI dla UI state,
Clean/layers dla dependency direction, Repository/UseCase dla domain/data
boundaries, DI dla wiring i feature modules dla skalowania. Pattern ma
zmniejszać złożoność, a nie tworzyć boilerplate.

</details>

<details>
<summary>154. Jak zorganizować moduły w projekcie Android?</summary>

#### Kotlin

Moduły w projekcie Android są potrzebne do isolation, build performance, feature
ownership i równoległej pracy zespołów. Ważna jest nie liczba modułów, lecz
właściwe boundaries i dependency direction.

Typowa struktura:

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

Dla małej app nie potrzeba dziesiątek modułów. Warto zaczynać prościej.

### `:app`

Entry point aplikacji:

- `Application`;
- main `Activity`;
- navigation host;
- DI assembly;
- build flavors/config;
- łączenie feature/data modules.

Logika biznesowa nie powinna mieszkać w `:app`.

### Feature modules

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

Feature zawiera UI/presentation logic konkretnej funkcji. Powinna pracować przez
domain contracts/use cases, a nie bezpośrednio przez Retrofit/Room implementation.

### Core modules

```text
:core:network  -> Retrofit/OkHttp setup
:core:database -> Room setup
:core:ui       -> design system/components
:core:common   -> utils, dispatchers, result types
```

Core nie powinien zależeć od feature.

### Domain modules

Domain zawiera business rules i contracts:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

Domain powinien być maksymalnie niezależny od Android framework.

### Data modules

Data implementuje domain contracts:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Tutaj mieszkają DTO, Room entities, mappers, API/DB data sources.

### Dependency direction

```text
feature -> domain
 data   -> domain
 app    -> feature + data
core    -> feature nie wie; feature może znać core
```

Nie dopuszczać cycles. `core` nie powinien wiedzieć o konkretnej feature.

### Build logic

Powtarzalną konfigurację Gradle lepiej wynieść do convention plugins:

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

To zmniejsza duplikację build scripts.

Kiedy nie dzielić:

- nie ma osobnego ownership;
- nie ma reusable boundary;
- nie ma build-time benefit;
- moduł tylko dodaje DI/navigation/Gradle boilerplate.

**Krótko:** moduły warto budować wokół `app`, `feature`, `core`, `domain`,
`data`. Dzielić trzeba wtedy, gdy istnieje realny boundary: feature ownership,
reusable infrastructure, domain contract, data implementation albo build benefit.
Nadmierna modularność szkodzi.

</details>

<details>
<summary>155. Czym jest token?</summary>

#### Kotlin

Token — credential, który potwierdza sesję albo prawo dostępu. W systemach
mobile/backend zwykle istnieją `access token` i `refresh token`.

### Access token

Ma krótki TTL i jest przekazywany w żądaniach do chronionego API:

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

Żyje dłużej i jest używany tylko do odświeżania access token:

```text
request -> 401 -> refresh -> new access token -> retry request
```

Refresh trzeba synchronizować: kilka równoległych `401` nie powinno uruchamiać
kilku refresh-żądań. Retry powinien być ograniczony, aby nie dostać infinite
loop.

### JWT i Bearer

JWT ma format:

```text
header.payload.signature
```

Payload jest tylko Base64Url encoded, nie encrypted. Dlatego sekretne dane nie
powinny trafiać do JWT payload.

Bearer token może wykorzystać każdy, kto go posiada. Jeśli token wycieknie,
dostęp jest możliwy do expiration albo revoke.

### Storage w Androidzie

Sensitive storage:

- Android Keystore;
- EncryptedSharedPreferences;
- encrypted DataStore.

Lepiej ukryć implementację za interface:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Tokeny nie powinny trafiać do logs, crash reports, analytics, screenshots, URL
query parameters ani plain files.

### Expiration i scopes

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)
```

Lokalna weryfikacja expiration pomaga odświeżyć token wcześniej, ale backend
jest źródłem prawdy. Jeśli refresh się nie uda — trzeba wyczyścić session i
odesłać użytkownika do login.

Scopes powinny być minimalne: `profile:read`, `payments:create`. Mobile app nie
powinna dostawać admin permissions ani server secrets.

Logout powinien usunąć lokalne tokens i, jeśli backend to wspiera, revoke
refresh token.

**Krótko:** token to credential dostępu. Access token idzie w API-żądania,
refresh token odnawia access token. Ważne: krótki TTL, minimalne scopes,
bezpieczne storage, zsynchronizowany refresh i żadnych tokens w logach.

</details>

<details>
<summary>156. Jaka jest różnica między authentication i authorization?</summary>

#### Kotlin

```text
Authentication -> kim jesteś?
Authorization  -> co wolno ci zrobić?
```

Authentication ustala identity/principal. Authorization sprawdza policy dla
konkretnej operation albo resource.

### Authentication

Identity potwierdzają password, OTP, passkey/biometrics, OAuth/OIDC albo ważna
session.

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Po login backend wydaje session cookie albo tokens. Access token dodaje się do
API requests, refresh token używa się tylko do uzyskania nowego access token.

### Authorization

Authorization sprawdza, czy principal ma prawo wykonać akcję:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

Backend uwzględnia roles, permissions, resource ownership, tenant i context.

Typowe modele:

- RBAC — roles;
- permission-based — granular permissions;
- ABAC/policy-based — attributes principal/resource/context.

### `401` vs `403`

```text
401 Unauthorized -> brak valid authentication credentials
403 Forbidden    -> credentials są, ale dostęp zabroniony
```

Przy `401` app może jeden raz synchronicznie zrobić token refresh. Jeśli refresh
się nie uda — zakończyć session.

Przy `403` re-authentication zwykle nie pomoże: UI powinno pokazać, że nie ma
dostępu.

### Android client vs backend

Client może ukrywać przyciski według permissions:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDelete) {
        Text("Delete")
    }
}
```

Ale to tylko UX. Modified client może wywołać API bezpośrednio, dlatego backend
zawsze musi enforce authorization.

Podział odpowiedzialności:

- client przechowuje credentials i obsługuje `401/403`;
- auth service wydaje/sprawdza credentials;
- policy layer decyduje o access;
- backend nie ufa permissions, które przyszły od clienta;
- logout/revoke kończy session zgodnie z security model.

**Krótko:** authentication potwierdza identity, authorization sprawdza dostęp do
operation/resource. Client odpowiada za credentials i UX, ale realne security
enforcement zawsze musi być na backendzie.

</details>

<details>
<summary>157. Jakich narzędzi używa się do komunikacji z backendem?</summary>

#### Kotlin

Networking stack zależy od protokołu: REST/HTTP, GraphQL, WebSocket, gRPC albo
KMP networking. Osobno potrzebne są serialization, auth, error mapping i
cache/offline strategy.

### REST/HTTP

Najczęściej: Retrofit + OkHttp.

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

OkHttp wykonuje żądania, Retrofit opisuje API Kotlin interface.

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Interceptors dodają headers, auth, logging, tracing. Retry powinien uwzględniać
idempotency.

### Serialization

Do JSON używa się Kotlin Serialization, Moshi albo Gson.

DTO lepiej oddzielać od domain:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### KMP

Dla Kotlin Multiplatform często używa się Ktor Client. Engine i platform-specific
config podstawia się osobno.

### GraphQL

Apollo Kotlin generuje type-safe models z GraphQL operations. GraphQL dodaje
schema/codegen, własne caching i specyfikę error handling.

### WebSocket

Dla realtime: chat, live updates, market data. Na Androidzie WebSocket wspierają
OkHttp i Ktor.

### gRPC

gRPC — contract-first RPC przez protobuf i generated clients. Jest właściwy,
jeśli backend ma już protobuf/gRPC infrastructure.

### Authentication

Typowe warianty:

- Bearer access/refresh tokens;
- OAuth/OIDC;
- cookies;
- API keys.

Interceptor dodaje credentials. `Authenticator` albo osobny session manager
koordynuje refresh po `401`. Refresh musi być zsynchronizowany, aby równoległe
`401` nie uruchamiały wielu refresh-żądań.

### Error handling i cache

Network layer powinien mapować transport/HTTP errors na własny domain/data error.
UI nie powinno zależeć od `HttpException` ani konkretnego clienta.

Offline/cache:

- HTTP cache — response cache;
- Room — structured source of truth;
- Paging + RemoteMediator — paged local/remote sync;
- Repository — określa, skąd czytać i kiedy refresh.

Praktyczny wybór:

- REST — Retrofit + OkHttp;
- KMP — Ktor Client;
- GraphQL — Apollo Kotlin;
- realtime — WebSocket;
- protobuf RPC — gRPC;
- offline source of truth — Room + Repository.

**Krótko:** dla REST zwykle wybiera się Retrofit/OkHttp, dla KMP — Ktor, dla
GraphQL — Apollo, dla realtime — WebSocket, dla RPC — gRPC. Ważne są nie tylko
żądania, ale też auth, error mapping, DTO/domain mapping i offline strategy.

</details>

<details>
<summary>158. Czym jest Retrofit?</summary>

#### Kotlin

`Retrofit` — biblioteka do pracy z REST API na Android/JVM. Opisuje HTTP
endpoints przez Kotlin/Java interfaces, a żądania wykonuje przez OkHttp.

API interface:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto

    @POST("users")
    suspend fun createUser(@Body body: CreateUserRequest): UserDto
}
```

Retrofit tworzy implementation tego interface w runtime.

Setup:

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val api = retrofit.create(UserApi::class.java)
```

`baseUrl` musi kończyć się `/`.

Retrofit wspiera `suspend` functions:

```kotlin
val user = api.getUser("123")
```

Annotations opisują HTTP request:

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

Retrofit sam nie parsuje JSON. Potrzebny jest converter:

- Moshi;
- Gson;
- Kotlin Serialization;
- Scalars.

DTO lepiej mapować na domain model:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

### Response handling

Można zwracać DTO bezpośrednio albo `Response<T>`:

```kotlin
suspend fun getUser(id: String): Response<UserDto>
```

`Response<T>` jest potrzebny, jeśli trzeba czytać status code, headers albo error
body.

### OkHttp interceptors

Auth, headers, logs i cache dodaje się przez OkHttp:

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

Retrofit/OkHttp mogą dać:

- `IOException` — network error;
- `HttpException` albo non-2xx `Response`;
- serialization exception.

W repository lepiej mapować to na domain/data error, a nie oddawać
`HttpException` do UI.

Production boundary:

```text
ViewModel -> UseCase -> Repository -> Retrofit API
```

**Krótko:** Retrofit opisuje REST endpoints przez interfaces i annotations,
działa nad OkHttp, wspiera coroutines i JSON converters. W production Retrofit
API i DTO powinny mieszkać w data layer za repository.

</details>

<details>
<summary>159. Czym jest Apollo GraphQL?</summary>

#### Kotlin

Apollo Kotlin — type-safe GraphQL client dla Kotlin/Android. Generuje Kotlin-kod
ze schema i `.graphql` operations oraz wspiera queries, mutations, subscriptions
i normalized cache.

W GraphQL client sam opisuje potrzebne pola:

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

Apollo generuje Kotlin types:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

Jeśli query nie pasuje do schema, błąd pojawi się na etapie code generation albo
compile time.

Client setup:

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

Auth dodaje się interceptorem:

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

Subscription dla realtime updates:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
    }
```

### Cache i errors

Normalized cache przechowuje entities według klucza, aby różne queries mogły
reuse tych samych danych. To zależy od stabilnych IDs w schema.

GraphQL response może jednocześnie zawierać `data` i `errors`:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()

if (response.hasErrors()) {
    // map response.errors to domain errors
}

val data = response.data
```

GraphQL error nie musi oznaczać HTTP 4xx/5xx. Network failure zwykle przychodzi
jako exception. Generated models lepiej mapować na domain models.

Apollo vs Retrofit:

- Apollo — GraphQL, client definiuje response shape, models generated;
- Retrofit — REST, backend definiuje endpoints/response, DTO pisze się ręcznie.

**Krótko:** Apollo Kotlin to type-safe GraphQL client z codegen, queries,
mutations, subscriptions i normalized cache. Używa się go, gdy backend działa
przez GraphQL, a nie REST endpoints.

</details>

<details>
<summary>160. Do czego używa się Firebase?</summary>

#### Kotlin

`Firebase` — zestaw backend-as-a-service narzędzi od Google dla mobile/web apps.
W Androidzie używa się go do analytics, crash reporting, push notifications,
auth, remote config, database/storage i backend functions.

### Analytics

Zbieranie user events, funnels, retention, feature usage:

```kotlin
firebaseAnalytics.logEvent(
    "profile_opened",
    bundleOf("source" to "home")
)
```

### Crashlytics

Crash i non-fatal monitoring:

```kotlin
Firebase.crashlytics.recordException(error)
```

Podstawowe production-narzędzie dla stabilności app.

### FCM

Firebase Cloud Messaging — push notifications i messaging events:

```kotlin
class AppMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // handle push
    }
}
```

### Firebase Auth

Gotowa auth platform:

- email/password;
- Google Sign-In;
- phone auth;
- anonymous auth;
- custom tokens.

Trzeba poprawnie zintegrować ją z backend/security model.

### Firestore / Realtime Database

NoSQL cloud databases z realtime updates:

```kotlin
firestore.collection("users").document(id).get()
```

Pasują do chat, realtime sync, collaborative features, prototypes. Security
rules są krytycznie ważne.

### Remote Config

Zmiana parametrów bez release app:

```kotlin
val enabled = remoteConfig.getBoolean("new_feature_enabled")
```

Use cases: feature flags, rollout, A/B testing, config UI/behavior.

### Storage i Functions

Cloud Storage — user uploads: images, videos, files.

```kotlin
storage.reference.child("avatars/$userId.jpg")
```

Cloud Functions — server-side logic: Firestore triggers, API endpoints, push
dispatch, validation, third-party integrations.

Zalety:

- szybki start;
- gotowe auth/analytics/crashes/push;
- dobra integracja z Androidem;
- realtime features;
- mniej własnej backend infrastructure na starcie.

Ryzyka:

- vendor lock-in;
- błędy security rules;
- pricing surprises przy wzroście;
- nie zawsze pasuje do złożonej backend domain logic;
- client-side access nie zastępuje backend security;
- offline/cache behavior trzeba rozumieć osobno.

**Krótko:** Firebase przyspiesza Android development przez gotowe auth,
analytics, Crashlytics, FCM, Firestore/Realtime DB, Remote Config, Storage i
Functions. Ale potrzebne są security rules, cost control, boundaries i
zrozumienie vendor lock-in.

</details>

<details>
<summary>161. Czym jest Android Keystore?</summary>

#### Kotlin

`Android Keystore` — systemowe przechowywanie kluczy kryptograficznych. Pozwala
tworzyć i używać kluczy tak, aby raw key material nie opuszczał Keystore. Na
części urządzeń klucze mogą być hardware-backed.

Keystore używa się do:

- encryption/decryption;
- signing/verification;
- local secrets;
- kluczy dla EncryptedSharedPreferences/DataStore encryption;
- biometric/user-auth protected keys;
- zmniejszenia ryzyka wycieku raw crypto key.

Ważne ograniczenie: Keystore nie robi hardcoded API key bezpiecznym. Jeśli secret
leży w APK, attacker może znaleźć miejsce, w którym app go używa. Keystore
najlepiej pasuje do kluczy wygenerowanych na device.

Generowanie AES key:

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

Pobranie klucza:

```kotlin
val keyStore = KeyStore.getInstance("AndroidKeyStore").apply { load(null) }
val key = keyStore.getKey("secret_key", null) as SecretKey
```

Otrzymujesz handle do key, a nie raw bytes.

Szyfrowanie:

```kotlin
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)
val iv = cipher.iv
val encrypted = cipher.doFinal(data)
```

Dla AES-GCM trzeba przechowywać `iv` obok ciphertext i nie używać ponownie IV z
tym samym key.

Klucz można powiązać z user authentication:

```kotlin
.setUserAuthenticationRequired(true)
```

Dla typowych zadań lepiej używać Jetpack Security:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()
```

Keystore/Jetpack Security pasuje do refresh tokens, local encryption keys,
user-specific secrets i sensitive preferences. Server/backend secrets nie wolno
wkładać do app — potrzebny jest backend proxy.

Ryzyka:

- niepoprawny crypto mode;
- reuse IV;
- brak key rotation;
- backup/restore edge cases;
- różne zachowanie Android versions/devices;
- próba ukrycia hardcoded secret zamiast poprawnej architektury.

**Krótko:** Android Keystore chroni klucze kryptograficzne na device i nie
oddaje raw key material. Jest przydatny do local encryption/signing i user
tokens, ale nie chroni hardcoded API secrets w APK. Dla typowych zadań lepszy
jest Jetpack Security nad Keystore.

</details>

<details>
<summary>162. Jakie są dispatchers w Kotlin Coroutines i do czego służą?</summary>

#### Kotlin

`CoroutineDispatcher` określa, na którym thread albo thread pool wykonuje się
coroutine. Właściwy dispatcher jest ważny dla UI responsiveness, performance i
unikania ANR.

### `Dispatchers.Main`

Główny UI thread:

```kotlin
withContext(Dispatchers.Main) {
    textView.text = "Loaded"
}
```

Do UI updates, Compose/View state, navigation, lifecycle-bound UI logic. Nie do
network, DB, file I/O ani ciężkich obliczeń.

### `Dispatchers.IO`

Do blocking I/O:

```kotlin
withContext(Dispatchers.IO) {
    file.readText()
}
```

Pasuje do file operations, database, network clients, blocking SDK calls. Nie do
CPU-heavy algorytmów.

### `Dispatchers.Default`

Do CPU-heavy pracy:

```kotlin
withContext(Dispatchers.Default) {
    largeList.sortedBy { it.score }
}
```

Pasuje do sortowania, parsing dużych struktur, compression, diff calculation,
obliczeń.

### `Dispatchers.Unconfined`

Nie przywiązuje coroutine do konkretnego thread i resume odbywa się tam, gdzie
zakończyła się suspension.

```kotlin
launch(Dispatchers.Unconfined) { }
```

W production Android code prawie nie jest potrzebny, bo zachowanie jest mniej
przewidywalne.

### `limitedParallelism`

```kotlin
val limitedIo = Dispatchers.IO.limitedParallelism(4)
```

Przydatne do ograniczenia równoległych uploads, API calls, batch jobs, image
processing.

### `withContext`

Do przełączania dispatcher wewnątrz suspend function:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

To lepsze niż uruchamianie nowej coroutine tylko po to, aby zmienić thread.

### Wstrzykiwać dispatchers

Dla testowalności dispatchers lepiej przekazywać przez constructor:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): User = withContext(ioDispatcher) {
        api.getUser()
    }
}
```

W teście można przekazać `StandardTestDispatcher`.

Typowe błędy:

- network/DB na `Main`;
- CPU-heavy praca na `IO`;
- `runBlocking` na main thread;
- hardcoded dispatchers w kodzie, który trzeba testować;
- custom thread pools bez cleanup;
- `Unconfined` bez jasnego powodu.

**Krótko:** `Main` — UI, `IO` — blocking I/O, `Default` — CPU-heavy praca,
`Unconfined` — rzadki specjalny przypadek. Dla testowalności dispatchers lepiej
wstrzykiwać, a dispatcher switch robić przez `withContext`.

</details>

<details>
<summary>163. Jakie są sposoby uruchamiania coroutines?</summary>

#### Kotlin

Coroutine uruchamia się w `CoroutineScope`. Najczęściej używa się `launch`,
`async`, `coroutineScope`, `supervisorScope` i Android-specific scopes.

### `launch`

Uruchamia coroutine bez business result i zwraca `Job`:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

```kotlin
val job = scope.launch { work() }
job.cancel()
```

Pasuje do side effects. Failure obsługuje się przez scope hierarchy albo lokalny
`try/catch`.

### `async`

Zwraca `Deferred<T>` dla concurrent result:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

Jeśli od razu robisz `await()` bez równoległości — lepiej po prostu wywołać
suspend-funkcję.

### `coroutineScope`

Tworzy structured boundary i czeka na wszystkie children:

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

Failure jednego child anuluje siblings i przekazuje się do callera.

### `supervisorScope`

Izoluje siblings od failure direct child:

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

Failure nadal trzeba obsłużyć albo odebrać przez `await()`.

### `runBlocking`

Blokuje caller thread. Jest właściwy dla CLI entry point albo bardzo ograniczonego
sync bridge. W tests lepszy jest `runTest`, na Android Main — nie używać przez
ryzyko ANR.

### Android scopes

`viewModelScope` — dla screen operations, anuluje się w `onCleared()`.

`lifecycleScope` — należy do `LifecycleOwner`; dla Flow w UI używa się
`repeatOnLifecycle`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

W Fragment dla View/binding — `viewLifecycleOwner.lifecycleScope`.

W Compose `rememberCoroutineScope()` — dla callback-driven UI actions:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

`GlobalScope` w app-code prawie zawsze jest błędny: nie ma ownera, lifecycle ani
cleanup.

**Krótko:** `launch` — side effect, `async` — concurrent result,
`coroutineScope` — fail-fast boundary, `supervisorScope` — izolacja siblings.
Scope wybiera się według ownera pracy; `GlobalScope` i `runBlocking` na Main nie
używa się.

</details>

<details>
<summary>164. Jaka jest różnica między launch i async?</summary>

#### Kotlin

`launch` i `async` — coroutine builders.

```text
launch -> Job, bez result value
async  -> Deferred<T>, result przez await()
```

### `launch`

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

`Job` pozwala na `cancel()`, `join()` i sprawdzenie state. `launch` używa się,
gdy result nie trzeba zwracać na zewnątrz: UI state update, Flow collection,
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

`Deferred<T>` to `Job` z result albo exception. `async` jest potrzebny głównie
dla kilku concurrent results:

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

Bez `async` suspend calls wykonywałyby się sekwencyjnie. Ale realna parallel
execution zależy od dispatchera.

### Exceptions i structured concurrency

W `coroutineScope` failure jednego child anuluje siblings i przekazuje się do
callera. `async` exception zostanie odebrany przez `await()`, ale w structured
scope failure i tak wpływa na parent.

Dla niezależnych failures potrzebny jest `supervisorScope` i jawna error policy.

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

`CancellationException` nie połykamy.

### Typowe błędy

Źle:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

Jeśli value nie jest potrzebne — używaj `launch`. Jeśli potrzebny jest jeden
value bez równoległości — wywołuj suspend function bezpośrednio. Do przełączania
dispatchera używaj `withContext`, nie `async`.

Praktyczny wybór:

- side effect bez result — `launch`;
- kilka concurrent results — `async + await`;
- jeden result — suspend function;
- dispatcher switch — `withContext`;
- powiązane children — `coroutineScope`;
- niezależne failures — `supervisorScope`.

**Krótko:** `launch` zwraca `Job` dla pracy bez result. `async` zwraca
`Deferred<T>` i jest potrzebny do concurrent results przez `await()`. Nie używaj
`async`, jeśli wynik nie jest potrzebny.

</details>

<details>
<summary>165. Jak obsługiwać błędy w coroutines?</summary>

#### Kotlin

Strategia zależy od ownership. Oczekiwane failures obsługuje się lokalnie i
mapuje na result/state. Nieobsłużone exceptions rozchodzą się przez `Job`
hierarchy. `CancellationException` nie wolno połykać.

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

Lepiej łapać konkretne exceptions na boundary, gdzie wiadomo, co z nimi zrobić.

W ViewModel błąd mapuje się na UI state, a nie oddaje raw
`HttpException`/`IOException` do UI.

### `CancellationException`

Przy broad catch cancellation trzeba przekazać dalej:

```kotlin
catch (error: CancellationException) {
    throw error
} catch (error: Exception) {
    handleError(error)
}
```

Inaczej można zepsuć cooperative cancellation.

### `CoroutineExceptionHandler`

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

Handler to last-resort dla uncaught exception w root `launch`, zwykle do logging.
Nie odtwarza coroutine i nie zastępuje lokalnego error handling.

### `async` exceptions

`await()` zwraca result albo rzuca exception:

```kotlin
val deferred = async { api.getUser() }

try {
    val user = deferred.await()
} catch (error: IOException) {
    handleError(error)
}
```

W zwykłym `coroutineScope` failure child anuluje parent i siblings. `try/catch`
wokół `await()` nie zawsze izoluje równoległe zadania. Dla niezależnych failures
potrzebny jest `supervisorScope`.

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

Failure nadal trzeba obsłużyć.

### `Flow.catch`

```kotlin
repository.users()
    .catch { error -> emit(emptyList()) }
    .collect { users -> render(users) }
```

`catch` łapie upstream exceptions. Nie należy zamieniać cancellation na fallback.

Typowe błędy:

- połykać `CancellationException`;
- polegać tylko na `CoroutineExceptionHandler`;
- ignorować `await()` failure;
- przekazywać raw exceptions do UI;
- mieszać retry, mapping i rendering w jednym miejscu.

**Krótko:** expected failures mapuje się na result/state przez `try/catch`,
`await()` albo `Flow.catch`. `coroutineScope` jest fail-fast, `supervisorScope`
izoluje siblings, `CancellationException` zawsze przepuszczamy dalej.

</details>

<details>
<summary>166. Jak zaczekać na wynik kilku równoległych żądań?</summary>

#### Kotlin

Niezależne suspend-żądania uruchamia się przez `async` w structured scope i czeka
przez `await()` albo `awaitAll()`.

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

Oba `async` są utworzone przed pierwszym `await()`, dlatego żądania wykonują się
concurrently. Realny parallelism zależy od dispatchera i HTTP clienta.

### `coroutineScope`

`coroutineScope`:

- czeka na wszystkie children;
- propaguje cancellation callera w dół;
- działa fail-fast: failure jednego child anuluje siblings.

To właściwa policy, jeśli potrzebne są wszystkie results.

### `awaitAll`

Dla listy requestów tego samego typu:

```kotlin
val users = coroutineScope {
    ids.map { id ->
        async { api.getUser(id) }
    }.awaitAll()
}
```

`awaitAll()` zachowuje kolejność deferred list i fail-fast zwraca failure.

Zależnych calls nie należy parallelizować:

```kotlin
val token = api.login()
val profile = api.getProfile(token)
```

### Partial success

Jeśli potrzebny jest partial result, używa się `supervisorScope` i typed result
dla każdego child:

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

`runRequest` powinien rethrow `CancellationException` i mapować tylko oczekiwane
failures.

### Timeout

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

Coroutine timeout anuluje scope. HTTP client nadal powinien mieć własne
connect/read/call timeouts.

### Limit in-flight requests

Dla dużego fan-out:

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

`Semaphore` ogranicza liczbę jednoczesnych requests. Dla dużych zbiorów używa się
też batching.

Praktyczny wybór:

- potrzebne są wszystkie results — `coroutineScope + async`;
- lista requests — `awaitAll()`;
- partial success — `supervisorScope + typed errors`;
- duży fan-out — `Semaphore` albo batching;
- zależne calls — sekwencyjnie.

**Krótko:** równoległe niezależne żądania uruchamia się przez `async` w
`coroutineScope`. `awaitAll()` czeka na listę. Zwykły scope jest fail-fast,
`supervisorScope` daje partial results, fan-out ogranicza się przez
`Semaphore`/batching.

</details>

<details>
<summary>167. Po co wymyślono Fragment?</summary>

#### Kotlin

`Fragment` wymyślono jako reusable części UI i zachowania wewnątrz `Activity`.
Idea polegała na tym, aby nie trzymać całego screen flow w jednej dużej Activity,
tylko dzielić UI na mniejsze komponenty z własnym lifecycle i layout.

Problem bez Fragment:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // navigation, list, details, clicks, state
    }
}
```

Taka Activity szybko staje się god class.

Fragment jako część ekranu:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // list UI logic
    }
}
```

Fragment może enkapsulować listę, szczegóły, formularz, step flow albo tabs.

Historycznie ważny use case — phone/tablet layouts:

```text
phone  -> Activity + ListFragment
tablet -> Activity + ListFragment + DetailsFragment
```

Activity mogła być kontenerem dla kilku Fragment i zmieniać je przez
FragmentManager:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Później Navigation Component zrobił to wygodniejszym.

Fragment ma własny lifecycle i osobny View lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Główny niuans: Fragment object może żyć dłużej niż View. Dlatego binding,
adapters i UI subscriptions trzeba czyścić w `onDestroyView()`.

Fragment może mieć własny albo shared ViewModel:

```kotlin
private val viewModel: ProfileViewModel by viewModels()
private val shared: SharedViewModel by activityViewModels()
```

To przydatne dla master-detail, wizard flow albo shared UI state.

W Compose rola Fragment jest mniejsza: często jedna Activity hostuje `NavHost`,
a screens są composables. Ale Fragment nadal są aktualne dla legacy View System,
hybrid apps, Navigation Component setups i stopniowych migracji.

**Krótko:** Fragment stworzono dla modular UI w View System: reusable blocks,
phone/tablet layouts, navigation wewnątrz Activity i back stack. Ich główna
trudność to lifecycle, szczególnie różnica między Fragment lifecycle i View
lifecycle.

</details>

<details>
<summary>168. Jakie problemy rozwiązują?</summary>

#### Kotlin

Fragment rozwiązują problemy dekompozycji View-based UI wewnątrz `Activity`:
duże Activity, navigation, back stack, lifecycle osobnych części screen/container
i różne layouts dla phone/tablet.

### Duże Activity

Bez dekompozycji `Activity` szybko staje się god class:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // home, details, settings, navigation, state
    }
}
```

Fragment dzielą UI:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

`Activity` zostaje hostem, a state/business logic powinny mieszkać w
`ViewModel`/domain layer.

### Różne layouts

```text
phone  -> Activity + jeden Fragment
tablet -> Activity + list Fragment + details Fragment
```

Phone może pokazywać list/details sekwencyjnie, tablet — jednocześnie.

### Navigation i back stack

Fragment pozwalają zmieniać części ekranu bez uruchamiania nowej Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Navigation Component robi to bardziej kontrolowanie przez navigation graph.

### Lifecycle UI

Fragment ma lifecycle i osobny View lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Binding, adapters i UI subscriptions trzeba czyścić w `onDestroyView()`, bo View
może zostać zniszczona wcześniej niż Fragment object.

### Izolacja odpowiedzialności

Fragment odpowiada za rendering, lifecycle i UI events konkretnego screen:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

Ale Fragment nie powinien zawierać logiki biznesowej.

### Czego Fragment nie rozwiązują

- złej architektury;
- mieszania UI i business logic;
- niepoprawnego state ownership;
- memory leaks po `onDestroyView()`;
- zagmatwanego navigation graph;
- nadmiernego zagnieżdżenia UI.

W View System Fragment jest wygodny jako screen/container. W pełnym Compose UI
jego rola często się zmniejsza albo znika.

**Krótko:** Fragment rozwiązują dekompozycję View-based UI, navigation/back
stack, phone/tablet layouts i lifecycle screen parts. Ale architektury, state
management i memory leaks nie naprawiają automatycznie.

</details>

<details>
<summary>169. Jakie są cechy zagnieżdżonych Fragment?</summary>

#### Kotlin

Zagnieżdżony Fragment żyje wewnątrz innego Fragment i jest zarządzany przez
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

`parentFragmentManager` zarządza Fragment na poziomie parent container, a nie
child Fragment bieżącego Fragment.

### Lifecycle i ViewModel scope

Child Fragment ma własny lifecycle, view lifecycle, state i ViewModel, ale nie
może żyć dłużej niż parent.

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()` — scope child Fragment;
- `requireParentFragment()` — shared state z parent;
- `activityViewModels()` — scope Activity.

### Navigation i back stack

Child Fragment może mieć własny back stack albo `NavHostFragment`. Trzeba jasno
rozumieć, kto obsługuje Back:

```kotlin
childFragmentManager.popBackStack()
```

Kilka zagnieżdżonych back stacks szybko komplikuje screen behavior, dlatego
nested navigation dodaje się tylko wtedy, gdy jest potrzebna.

### Komunikacja

Nie warto trzymać bezpośrednich references między Fragment. Lepiej:

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

Parent i child mają osobne view lifecycles. View references, adapters i callbacks
czyści się w `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

W `ViewPager2` adapter wewnątrz Fragment tworzy się z Fragment owner:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment)
```

Zagnieżdżone Fragment są właściwe dla tabs, lokalnego wizard flow albo
niezależnego bloku z własnym lifecycle. Dla prostego UI lepszy jest custom View
albo composable.

**Krótko:** nested Fragment zarządza się przez `childFragmentManager`, mają
własne lifecycle/ViewModel/back stack, ale zależą od parent. Główne ryzyka to
zły manager, zbędna nested navigation i leaks po `onDestroyView()`.

</details>

<details>
<summary>170. Jakie są wywołania (callbacks) w Application?</summary>

#### Kotlin

`Application` reprezentuje Android process. Każdy process app ma własny
`Application` instance.

Główne callbacks:

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

Tutaj uruchamia się app-wide infrastructure: DI, logging, crash reporting, SDK.
Callback wykonuje się na main thread i wpływa na cold start, dlatego
initialization powinna być minimalna albo lazy.

W manifest:

```xml
<application android:name=".App" />
```

Dla Hilt class oznacza się `@HiltAndroidApp`.

Ważny niuans: ContentProvider-based initializers mogą wystartować wcześniej niż
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

To signals, aby zwolnić recreatable memory: image cache, buffers, precomputed
data. Persisted state nie wolno usuwać.

### Configuration

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

App-wide config changes. UI reaction lepiej trzymać w Activity/Fragment/Compose,
gdzie jest aktualny lifecycle i themed Context.

### `ActivityLifecycleCallbacks`

Application może słuchać lifecycle wszystkich Activity:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityStarted(activity: Activity) = Unit
        override fun onActivityResumed(activity: Activity) = Unit
        override fun onActivityStopped(activity: Activity) = Unit
    }
)
```

Use cases: analytics, session tracking, debug tooling. Dla foreground/background
często lepszy jest `ProcessLifecycleOwner`.

Nie wolno przechowywać strong reference do Activity po callbacku.

### `onTerminate()`

`onTerminate()` nie jest production cleanup. Android może zabić process bez jego
wywołania. Nie należy polegać na nim przy saving data, logout, transactions ani
resource release.

Czego nie robić:

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

To Activity leak, global mutable state i utrata data po process death.
`Application` nie powinien być service locator ani session storage.

**Krótko:** `Application.onCreate()` to szybka app-wide initialization.
`onTrimMemory/onLowMemory` — zwalnianie recreatable memory. `onTerminate()` nie
jest gwarantowane. Activity lifecycle można słuchać przez
`ActivityLifecycleCallbacks`, ale bez przechowywania Activity references.

</details>

<details>
<summary>171. Jaki singleton tworzy system podczas uruchamiania aplikacji Android?</summary>

#### Kotlin

Podczas startu Android process system tworzy instancję `Application`. To
application-level singleton w obrębie konkretnego process.

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
    }
}
```

W manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Android tworzy `App` i wywołuje `onCreate()` przed uruchomieniem `Activity`,
`Service`, `BroadcastReceiver` i innych components w tym process.

### Singleton tylko w obrębie process

`Application` nie jest singletonem na całym device i nie zawsze jest jeden na
całą app, jeśli istnieje multiprocess.

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

Wtedy będą dwie instances:

```text
main process  -> App instance #1
:sync process -> App instance #2
```

Dlatego `Application`, Kotlin `object` i static state nie można uważać za shared
storage między processes.

### Do czego używa się Application

- DI setup;
- crash reporting;
- logging;
- analytics setup;
- app-wide SDK initialization;
- `ActivityLifecycleCallbacks`;
- lekka globalna konfiguracja.

Dla Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

### Application context

Dla long-lived dependencies używa się `applicationContext`, a nie `Activity`
context:

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

### Czego nie robić

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Problemy:

- state zniknie po process death;
- reference do `Activity` tworzy memory leak;
- business logic staje się global mutable state;
- testy stają się zależne od shared state.

Krytyczny state trzeba przechowywać w DB, DataStore, files albo backend.

### Application vs Kotlin object

Kotlin `object` tworzy się lazy przy pierwszym odwołaniu. `Application` tworzy
Android framework i ma process-level Context. Oba żyją tylko w obrębie process i
znikają po process death.

**Krótko:** Android tworzy process-level singleton `Application`. Nadaje się do
app-wide initialization, ale nie do przechowywania session/business state i nie
powinien trzymać UI references.

</details>

<details>
<summary>172. Jak działa BackStack?</summary>

#### Kotlin

`BackStack` — stos navigation history. Określa, dokąd użytkownik wróci po Back.
Zasada to LIFO: ostatnio dodany ekran wychodzi pierwszy.

```text
Home -> Details -> Settings
```

Back:

```text
Settings removed -> Details visible
Details removed  -> Home visible
```

### Activity back stack

Nowa Activity dodaje się do task back stack:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Back kończy bieżącą Activity i wraca do poprzedniej.

### Fragment back stack

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Jeśli nie wywołać `addToBackStack`, Back nie przywróci poprzedniego Fragment.

### Navigation Component

Back stack składa się z destinations:

```kotlin
navController.navigate("details/$id")
navController.popBackStack()
```

`popBackStack()` usuwa bieżącą destination.

### `popUpTo` i `inclusive`

`popUpTo` czyści część stack:

```kotlin
navController.navigate("home") {
    popUpTo("login") { inclusive = true }
}
```

Przydatne po login/onboarding, aby użytkownik nie wrócił na login.

```text
inclusive = false -> popUpTo destination zostaje
inclusive = true  -> popUpTo destination też jest usuwana
```

### Compose Navigation

Zasada jest taka sama:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen() }
    composable("details/{id}") { DetailsScreen() }
}
```

Back stack jest zarządzany przez `NavController`.

### `launchSingleTop` i bottom navigation

Aby nie tworzyć duplikatów destination:

```kotlin
navController.navigate("home") {
    launchSingleTop = true
}
```

Dla bottom navigation często zachowuje się state tabs:

```kotlin
navController.navigate(route) {
    popUpTo(navController.graph.startDestinationId) {
        saveState = true
    }
    launchSingleTop = true
    restoreState = true
}
```

Typowe błędy:

- nie czyścić stack po auth/onboarding;
- duplikować destinations;
- mieszać manual Fragment transactions i NavController;
- mylić Activity task stack z NavController back stack;
- nie uwzględniać nested graphs albo modal UI.

Praktyczna zasada: BackStack powinien odpowiadać oczekiwanej historii
użytkownika. Modal UI zamyka się pierwsze, irreversible flows czyszczą stack,
tabs zachowują swój state.

**Krótko:** BackStack to LIFO history ekranów/destinations. W Androidzie istnieją
Activity task stack, Fragment back stack i NavController back stack. Zarządza się
nim przez `navigate`, `popBackStack`, `popUpTo`, `inclusive`, `launchSingleTop`,
`saveState`, `restoreState`.

</details>

<details>
<summary>173. W jakich przypadkach można dostać ANR (Application Not Responding)?</summary>

#### Kotlin

ANR powstaje, gdy app process nie odpowiada na ważne zdarzenie systemowe na
czas. Najczęściej przyczyną jest main thread zablokowany, przeciążony albo
czekający na inny thread/process.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // blocks input and rendering
}
```

Main thread obsługuje input, lifecycle, część Binder callbacks i rendering. ANR
może spowodować jedna długa operacja albo duża kolejka krótkich.

### Typowe przyczyny

- synchronous network/disk I/O albo DB query na Main;
- ciężki parsing, bitmap decoding, sorting, crypto;
- `runBlocking`, `Thread.sleep()`, `Future.get()`, `join()`;
- ciężkie lifecycle/View/Compose callbacks;
- długa praca w `BroadcastReceiver` albo `Service`;
- locks/deadlocks/synchronous Binder calls;
- ciężki startup w `Application`, `ContentProvider`, DI albo pierwszej Activity.

`suspend` nie przenosi blocking code z main thread automatycznie.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

I/O — `Dispatchers.IO`, CPU-heavy — `Dispatchers.Default`.

`Service` sam nie tworzy worker thread. Dla długiej gwarantowanej pracy —
`WorkManager`. Dla `BroadcastReceiver.goAsync()` trzeba obowiązkowo wywołać
`finish()`.

### Jank vs ANR

Powolny frame to jank, nie zawsze ANR. Ale długa seria kosztownych
layout/draw/recomposition operacji może zablokować Main wystarczająco długo.

### Diagnostyka

Zaczynać od stack main thread i threads, na które czeka:

- Play Console Android vitals / ANR traces;
- Perfetto/System Trace;
- Android Studio CPU Profiler;
- Logcat i custom timing metrics;
- StrictMode w debug build.

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

StrictMode pomaga znaleźć main-thread I/O, ale nie zastępuje traces.

### Profilaktyka

- nie blokować Main;
- nie trzymać lock podczas I/O;
- dodawać timeouts do network/IPC;
- odkładać niekrytyczną startup initialization;
- dzielić długą CPU pracę;
- kontrolować częstotliwość UI events;
- profilować startup, scrolling i mass updates.

**Krótko:** ANR powodują blocking I/O, CPU-heavy work, locks, synchronous
waits/IPC, długie callbacks albo ciężki startup na Main. Diagnostykę zaczyna się
od stack main thread i system trace.

</details>

<details>
<summary>174. Co można wykonywać na main thread?</summary>

#### Kotlin

`Main thread` w Androidzie odpowiada za UI: input events, lifecycle callbacks,
rendering, View/Compose updates. Można na nim wykonywać tylko szybkie operacje,
które nie blokują rendering i nie grożą ANR.

Można:

- aktualizować UI;
- pracować z View hierarchy;
- renderować Compose UI;
- wykonywać lifecycle callbacks;
- obsługiwać click listeners;
- robić navigation calls;
- pokazywać dialog/snackbar/toast;
- wykonywać krótką lightweight logic.

```kotlin
button.setOnClickListener {
    viewModel.onLoginClick()
}
```

View system nie jest thread-safe, dlatego UI updates muszą być na main thread:

```kotlin
textView.text = "Loaded"
progressBar.isVisible = false
```

W Compose composable powinien być lekki:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
ProfileScreen(state)
```

Nie wolno blokować main thread:

- network requests;
- file I/O;
- duże DB queries;
- bitmap decoding;
- duży JSON parsing;
- heavy sorting/filtering;
- `Thread.sleep`;
- `runBlocking`;
- długie synchronized blocks.

Main thread powinien mieścić się mniej więcej w 16 ms na frame dla 60 fps. Jeśli
jest zajęty długą pracą — będzie jank, a przy długim blokowaniu — ANR.

Background work:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

Zasada:

```text
UI/orchestration -> Main
blocking I/O     -> Dispatchers.IO
CPU-heavy work   -> Dispatchers.Default
```

Krótka validation na main thread jest normalna:

```kotlin
if (email.isBlank()) {
    showEmailError()
    return
}
```

Aby znaleźć przypadkowe I/O na main thread w debug, można włączyć StrictMode:

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

**Krótko:** main thread jest dla UI, lifecycle, navigation i krótkiej
orchestration logic. Wszystko, co nieprzewidywalnie długie: network, disk, DB,
parsing, bitmap, CPU-heavy work — przenosimy do `IO` albo `Default`.

</details>

<details>
<summary>175. Jakie są podstawowe komponenty Androida?</summary>

#### Kotlin

Podstawowe Android system components: `Activity`, `Service`, `BroadcastReceiver`,
`ContentProvider`. Fragment, ViewModel i WorkManager — to ważne Jetpack
abstractions, ale nie system components.

### Activity

UI entry point z własnym window i lifecycle:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Activity otrzymuje `Intent` i może hostować Compose UI albo Fragment.

### Service

Component bez własnego UI:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Service callbacks domyślnie wykonują się na main thread. Dla ongoing user-visible
work — foreground service. Dla deferrable persistent work zwykle lepszy jest
WorkManager.

### BroadcastReceiver

Odbiera system/app broadcasts:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // handle event
    }
}
```

`onReceive()` powinno szybko się zakończyć. Długą pracę deleguje się do
WorkManager albo innego właściwego component.

### ContentProvider

Udostępnia strukturalne dane przez URI:

```text
content://com.example.provider/users/1
```

Używa się go do controlled inter-process data sharing. Systemowe przykłady to
contacts/media providers.

### Manifest i Intent

System components deklaruje się w `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

Manifest określa też permissions, intent filters i `android:exported` behavior.

`Intent` uruchamia Activity/Service albo dostarcza broadcast:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

### Jetpack abstractions

- Fragment — część View-based UI wewnątrz Activity;
- ViewModel — owner UI state, który przeżywa configuration changes;
- WorkManager — scheduler persistent deferrable work.

Praktyczny wybór:

- UI entry — `Activity`;
- ongoing user-visible work — foreground `Service`;
- system/app event — `BroadcastReceiver`;
- inter-process data API — `ContentProvider`;
- deferrable persistent work — `WorkManager`.

**Krótko:** podstawowe Android system components to `Activity`, `Service`,
`BroadcastReceiver`, `ContentProvider`. Deklaruje się je w manifest i
współpracują przez Intent/Binder/URI. Fragment, ViewModel i WorkManager to
poziom Jetpack.

</details>

<details>
<summary>176. Czym jest Service?</summary>

#### Kotlin

`Service` — Android component bez UI. Ważne: Service sam nie tworzy background
thread. Jego callbacks domyślnie wykonują się na main thread w process app.

Service nie gwarantuje przetrwania process death, force stop ani reboot.

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

Każde `startService()` wywołuje `onStartCommand()`. Blocking pracę trzeba wynieść
z Main. `onDestroy()` to best-effort cleanup, przy kill process może się nie
wywołać.

### Bound Service

Bound service udostępnia API przez `IBinder`:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService = this@PlayerService
    }

    override fun onBind(intent: Intent): IBinder = binder
}
```

Local Binder działa w jednym process. Dla cross-process IPC — Messenger/AIDL.

Bound-only service żyje, dopóki ma clients. Jeśli service jest też started,
`unbindService()` nie zatrzymuje started lifecycle.

### Foreground Service

Potrzebny dla długiej user-visible pracy: media playback, navigation, active
location, file transfer.

```kotlin
startForeground(
    NOTIFICATION_ID,
    createNotification()
)
```

Po `startForegroundService()` trzeba szybko wywołać `startForeground()`.
Współczesny Android ogranicza background start i wymaga notification, service
type oraz permissions.

### Lifecycle

```text
onCreate()       -> utworzenie instance
onStartCommand() -> każdy start
onBind()         -> client connection
onDestroy()      -> cleanup
```

Restart flags:

- `START_NOT_STICKY` — nie odtwarzać;
- `START_STICKY` — odtworzyć bez gwarancji Intent;
- `START_REDELIVER_INTENT` — ponownie dostarczyć Intent.

Restart policy nie robi z Service durable queue, operation musi być idempotent.

Praktyczny wybór:

```text
user-visible active work -> Foreground Service
reliable deferred work   -> WorkManager
exact-time trigger       -> AlarmManager
media playback           -> Media3 + FGS
screen-bound work        -> ViewModel scope
```

**Krótko:** Service to lifecycle component bez UI, nie osobny thread. Started
service wykonuje commands, bound service daje Binder API, foreground service jest
potrzebny dla ongoing user-visible work. Dla niezawodnej odłożonej pracy —
WorkManager.

</details>

<details>
<summary>177. Czym jest BroadcastReceiver?</summary>

#### Kotlin

`BroadcastReceiver` — Android component do odbierania broadcast `Intent` od
systemu, app components albo innych apps.

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            scheduleSync(context)
        }
    }
}
```

Receiver nie przechowuje state i nie jest przeznaczony do długiej pracy. Po
zakończeniu `onReceive()` process może zostać zatrzymany.

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

Dla `BOOT_COMPLETED` potrzebny jest permission `RECEIVE_BOOT_COMPLETED`.

Manifest receiver może działać bez UI, ale współczesny Android ogranicza wiele
implicit broadcasts. Trzeba sprawdzać konkretny action/API.

`android:exported` jest krytyczny dla security. Jeśli receiver nie ma przyjmować
intents od innych apps — `exported=false`. Jeśli jest exported — trzeba validate
intent data i permissions.

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

Register/unregister muszą być symetryczne. Lifecycle pair zależy od potrzeby:
visible screen — `onStart/onStop`, Activity lifetime — `onCreate/onDestroy`.

Na współczesnych API dla dynamic receiver często trzeba jawnie wskazywać
`RECEIVER_EXPORTED` albo `RECEIVER_NOT_EXPORTED`.

### Długa praca

`onReceive()` zwykle wykonuje się na main thread i powinno szybko się zakończyć.
Dla deferrable persistent work używa się WorkManager:

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

`goAsync()` pozwala krótko kontynuować pracę po `onReceive()`, ale wymaga
`PendingResult.finish()` i nie zastępuje WorkManager dla długich zadań.

### Komunikacja wewnętrzna

`LocalBroadcastManager` jest deprecated. Wewnątrz process lepiej używać
Flow/StateFlow, callbacks, repository state albo navigation result.

BroadcastReceiver jest właściwy dla system events, alarms, boot/package changes
i jasnej inter-app/component communication.

**Krótko:** BroadcastReceiver przyjmuje broadcast Intent i powinien szybko
zakończyć `onReceive()`. Manifest receiver wymaga uwagi na `exported`/permissions,
dynamic receiver wiąże się z lifecycle, długą pracę deleguje się do WorkManager.

</details>

<details>
<summary>178. Czym jest Context i jakie istnieją jego typy?</summary>

#### Kotlin

`Context` — punkt dostępu do Android environment: resources, system services,
package info, files, preferences i uruchamiania components. Konkretny context
określa też theme i lifecycle boundary.

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

Żyje razem z process i nie ma theme konkretnej Activity. Nadaje się do long-lived
dependencies: Room, DataStore, repositories, SDK, którym nie jest potrzebny UI
context.

### Activity Context

Ma theme, window i lifecycle konkretnej Activity. Jest potrzebny do dialogs,
themed views, UI operations. Nie wolno przechowywać go w singleton ani long-lived
object.

### Fragment Context

`Fragment` sam nie jest `Context`. Bierze context z hosta:

```kotlin
val context = requireContext()
val activityContext = requireActivity()
```

Przed attach context może być `null`, po detach `requireContext()` rzuci
exception.

### Service Context

`Service` jest `Context`, powiązanym z lifecycle service. Nadaje się do service
operations, ale nie ma Activity window/theme semantics.

### ContextWrapper

`ContextWrapper` deleguje base context i może zmieniać zachowanie:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

`ContextThemeWrapper` jest przydatny do themed UI inflation.

### Compose

```kotlin
val context = LocalContext.current
```

To context bieżącego composition environment. Można go używać do resources/UI
actions, ale nie należy przechowywać go bez lifecycle ownership.

### Memory leaks

Źle:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

Singleton zatrzyma Activity po recreation.

Lepiej dla long-lived dependency:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

Przekazywać `context.applicationContext`.

Praktyczny wybór:

```text
Application context -> DB, DataStore, long-lived dependencies
Activity context    -> UI, dialogs, themed views
Fragment context    -> host context while attached
Service context     -> service operations
LocalContext        -> Compose resources/UI actions
```

**Krótko:** `Context` daje dostęp do Android environment. Long-lived zależności —
application context; UI — Activity/themed context; Fragment nie jest Context;
Activity context nie wolno trzymać w singleton.

</details>

<details>
<summary>179. Czym jest Bundle?</summary>

#### Kotlin

`Bundle` — Android key-value container do przekazywania małych danych między
components albo do zapisywania prostego UI state.

```kotlin
val bundle = Bundle().apply {
    putString("user_id", userId)
    putInt("tab", 2)
}

val userId = bundle.getString("user_id")
```

Używa się go z `Intent`, Fragment arguments, `onSaveInstanceState`, Navigation i
Activity Result API.

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

Pasuje do małego UI state po configuration change albo process recreation.

### Typy

Bundle wspiera:

- primitives;
- `String`;
- arrays;
- `Parcelable`;
- `Serializable`;
- `Bundle`;
- `ArrayList` niektórych typów.

Dla Android custom data lepszy jest `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Ale dla navigation często lepiej przekazywać tylko `id`, a dane ładować z
repository.

### Ograniczenia

Bundle nie jest do dużych payloads. Można dostać `TransactionTooLargeException`.

Źle:

```kotlin
bundle.putParcelableArrayList("huge_list", hugeList)
```

Lepiej przekazać id/key do DB/cache.

### Bundle vs inne state holders

```text
Bundle            -> mały navigation/UI state
SavedStateHandle  -> state w ViewModel po recreation
ViewModel         -> screen/business state during lifecycle
Database/DataStore -> persistent data
```

Klucze w Bundle to strings, dlatego łatwo się pomylić. Dla Navigation lepsze są
Safe Args albo typed route arguments, jeśli są dostępne.

**Krótko:** Bundle to key-value container dla małych danych: ids, flags, prosty
UI/navigation state. Nie wkładać dużych list, bitmap ani payloads; lepiej
przekazać id i odtworzyć dane z repository/cache.

</details>

<details>
<summary>180. Czy można zagnieżdżać Bundle w Bundle?</summary>

#### Kotlin

Tak, `Bundle` można zagnieżdżać w innym `Bundle` przez `putBundle()` i czytać
przez `getBundle()`.

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

Odczyt:

```kotlin
val address = userBundle.getBundle("address")
val city = address?.getString("city")
```

To przydatne dla małych logicznie pogrupowanych danych:

```text
user
 ├─ id
 ├─ name
 └─ address
     ├─ city
     └─ street
```

Przykład we Fragment arguments:

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

Dla saved state:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putBundle("filters", bundleOf("query" to query))
    super.onSaveInstanceState(outState)
}
```

Ograniczenia:

- Bundle nie jest do dużych payloads;
- głębokie zagnieżdżenie pogarsza czytelność;
- string keys łatwo typo-nąć;
- można dostać `TransactionTooLargeException`.

Źle:

```kotlin
bundle.putBundle("huge_data", hugeNestedBundle)
```

Jeśli struktura ma jasny model, lepszy jest `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Dla dużych danych lepiej przekazać `id` i załadować dane z
repository/cache/database.

**Krótko:** można zagnieżdżać `Bundle` w `Bundle`, ale tylko dla małych prostych
grouped arguments albo saved state. Dla złożonych modeli — `Parcelable`, dla
dużych danych — `id + repository`.

</details>

<details>
<summary>181. Czym są Permissions w Androidzie?</summary>

#### Kotlin

Permissions — to mechanizm Androida, który ogranicza dostęp app do zasobów poza
sandbox: camera, microphone, location, contacts, notifications, storage/media.

Główne typy:

```text
normal    -> przyznawane automatycznie
dangerous -> manifest + runtime request
special   -> osobny Settings flow
signature -> tylko apps z odpowiednim certificate
```

Dangerous permission trzeba zadeklarować w manifest i poprosić o nie runtime:

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

Przed protected API zawsze sprawdza się aktualny grant:

```kotlin
val granted = ContextCompat.checkSelfPermission(
    context,
    Manifest.permission.CAMERA
) == PackageManager.PERMISSION_GRANTED
```

Grant nie może być cache'owany jako stały boolean: użytkownik może cofnąć
permission w Settings, system może zrobić auto-reset, a camera/microphone/location
mogą być one-time.

`shouldShowRequestPermissionRationale()` pomaga zdecydować, czy pokazać
wyjaśnienie przed ponownym request. `false` nie zawsze oznacza permanent denial,
dlatego app musi uwzględniać własną historię request.

Praktyczne zasady:

- prosić o permission w momencie działania, a nie na starcie app;
- prosić o minimalny dostęp;
- wspierać denial, revoke i one-time permissions;
- dla files/media używać Photo Picker, SAF, MediaStore albo app-specific storage;
- dla special permissions prowadzić użytkownika do Settings i po powrocie znów
  sprawdzać capability.

**Krótko:** Permissions chronią sensitive resources. Dangerous permissions
deklaruje się w manifest i prosi o nie runtime przez Activity Result API. Grant
może zmienić się w każdej chwili, dlatego trzeba go sprawdzać przed użyciem.

</details>

<details>
<summary>182. Czym jest setContentView?</summary>

#### Kotlin

`setContentView()` — to Activity API, które ustawia root UI dla ekranu. W View
System inflate'uje XML layout albo przyjmuje gotowy `View` i dodaje go do content
area Activity.

Podstawowy przykład:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

W uproszczeniu pod spodem:

```text
XML -> inflate -> View hierarchy -> Activity content root
```

Z ViewBinding częściej pisze się tak:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

Po `setContentView()` można szukać View przez `findViewById`, bo layout jest już
podłączony:

```kotlin
val title = findViewById<TextView>(R.id.title)
```

W Compose zamiast `setContentView()` używa się `setContent { ... }`:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { MainScreen() }
    }
}
```

We Fragment `setContentView()` się nie wywołuje: layout tworzy się przez
`Fragment(R.layout...)`, `onCreateView()` albo ComposeView.

Typowe błędy:

- wywołać `findViewById` przed `setContentView`;
- wywołać `setContentView` kilka razy bez potrzeby;
- trzymać Activity View references w singleton;
- mieszać XML i Compose bez jasnej granicy odpowiedzialności.

**Krótko:** `setContentView()` ustawia root UI dla Activity. Dla XML lepiej
używać ViewBinding + `setContentView(binding.root)`, dla Compose —
`setContent { ... }`, dla Fragment — własny lifecycle tworzenia view.

</details>

<details>
<summary>183. Jakie metody istnieją w View?</summary>

#### Kotlin

`View` — klasa bazowa Android UI. Kluczowy pipeline: `measure -> layout -> draw`.

Główne callbacks:

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

`onMeasure()` określa rozmiar View według `MeasureSpec`: `EXACTLY`, `AT_MOST`,
`UNSPECIFIED`. `onLayout()` rozmieszcza children i zwykle jest ważny dla
`ViewGroup`. `onDraw()` rysuje zawartość na `Canvas`.

Do aktualizacji UI:

```text
zmienił się tylko wygląd       -> invalidate()
zmienił się rozmiar/layout     -> requestLayout()
```

Input obsługuje się przez `onTouchEvent()`:

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

`performClick()` jest ważny dla click listeners i accessibility.

Lifecycle View:

- `onAttachedToWindow()` — View jest podłączona do window;
- `onDetachedFromWindow()` — trzeba zatrzymać animation, listeners, callbacks;
- `onSaveInstanceState()` / `onRestoreInstanceState()` — zapis własnego state.

Przydatne metody i stany:

- `VISIBLE`, `INVISIBLE`, `GONE`;
- `requestFocus()` / `clearFocus()`;
- `post { }` — wykonać kod w UI queue;
- `setOnClickListener()` / `setOnLongClickListener()`.

W `onDraw()` nie robi się I/O, bitmap decoding ani zbędnych allocation, bo metoda
może być wywoływana często.

**Krótko:** główne metody `View` to `onMeasure()`, `onLayout()`, `onDraw()`,
`invalidate()`, `requestLayout()`, `onTouchEvent()`, `performClick()` i lifecycle
callbacks. Odpowiadają za rozmiar, pozycję, rysowanie, input i zasoby.

</details>

<details>
<summary>184. Czym jest ViewGroup?</summary>

#### Kotlin

`ViewGroup` — to `View`, który zawiera child views i określa, jak je mierzyć,
rozmieszczać, rysować oraz przekazywać touch events.

```text
View      -> jeden UI node
ViewGroup -> kontener + children + layout policy
```

Przykłady: `LinearLayout`, `FrameLayout`, `ConstraintLayout`, `RecyclerView`.

Główny pipeline:

```text
onMeasure() -> onLayout() -> dispatchDraw()
```

`onMeasure()` mierzy children i określa własny rozmiar:

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

`onLayout()` ustawia final bounds dla children:

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

`LayoutParams` opisują zasady dla child: `weight` w `LinearLayout`, constraints w
`ConstraintLayout`, `gravity` w `FrameLayout`. Custom `ViewGroup` musi poprawnie
wspierać params, margins, `GONE`, padding i `MeasureSpec`.

Touch flow:

```text
dispatchTouchEvent()
 -> onInterceptTouchEvent()
 -> child.dispatchTouchEvent()
 -> onTouchEvent()
```

`onInterceptTouchEvent()` pozwala kontenerowi przejąć gesture od child, na
przykład dla scroll. Niepoprawny intercept psuje clicks i nested scrolling.

Do aktualizacji:

```text
invalidate()    -> przerysować
requestLayout() -> powtórzyć measure/layout
```

Custom `ViewGroup` pisze się tylko wtedy, gdy gotowe containers albo Compose nie
pokrywają layout/touch/performance requirement.

**Krótko:** `ViewGroup` to kontener dla `View`, który zarządza mierzeniem,
pozycjonowaniem, rysowaniem i touch routing dzieci. Kluczowe metody to
`onMeasure()`, `onLayout()`, `dispatchDraw()`, `onInterceptTouchEvent()`.

</details>

<details>
<summary>185. Do czego służy metoda onStart() w Activity?</summary>

#### Kotlin

`onStart()` jest wywoływane, gdy `Activity` staje się widoczna dla użytkownika.
UI jest już utworzone, ale Activity niekoniecznie jest jeszcze w foreground i
niekoniecznie przyjmuje input.

Lifecycle:

```text
first launch: onCreate -> onStart -> onResume
background:   onPause -> onStop
return:       onRestart -> onStart -> onResume
```

Różnica względem `onResume()`:

```text
onStart()  -> Activity visible
onResume() -> Activity interactive / foreground
```

W `onStart()` właściwe jest uruchamianie visible-only resources:

- rejestrowanie lightweight listeners/receivers;
- visibility analytics;
- subskrypcje UI state;
- resources potrzebne tylko wtedy, gdy ekran jest widoczny.

Wszystko, co startuje w `onStart()`, trzeba symetrycznie zatrzymać w `onStop()`:

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

Dla Flow lepiej używać lifecycle-aware API:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

`repeatOnLifecycle(STARTED)` uruchamia collection, gdy Activity jest widoczna, i
anuluje ją po przejściu poniżej `STARTED`.

Czego nie robić w `onStart()`:

- ciężkiego blocking I/O;
- długiego parsing albo `Thread.sleep()`;
- jednorazowej inicjalizacji, której miejsce jest w `onCreate()`;
- background work, który ma przeżyć Activity.

Jeśli refresh startuje w `onStart()`, ViewModel albo repository powinny
kontrolować freshness i deduplication, bo `onStart()` wywołuje się przy każdym
powrocie na ekran.

**Krótko:** `onStart()` oznacza, że Activity stała się widoczna. Tutaj uruchamia
się lekkie visible-only resources i subskrypcje, a czyszczenie robi się w
`onStop()`. Dla Flow używa się `repeatOnLifecycle(STARTED)`.

</details>

<details>
<summary>186. Jak można trafić do onStart(), ale nie trafić do onResume()?</summary>

#### Kotlin

Jest to możliwe, jeśli `Activity` stała się widoczna, ale nie dostała foreground
focus albo lifecycle został przerwany między `STARTED` i `RESUMED`.

Różnica stanów:

```text
onStart()  -> Activity visible
onResume() -> Activity foreground + interactive
```

Typowe scenariusze:

1. **Na wierzchu otworzyła się inna Activity albo overlay**

Transparent/dialog-themed Activity może zostawić poprzedni ekran widoczny, ale
zabrać focus.

```text
MainActivity.onStart()
OverlayActivity otworzyła się na wierzchu
MainActivity nie przechodzi do onResume()
```

2. **Redirect w `onStart()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

Focus przechodzi do innej Activity, dlatego bieżąca może nie dojść do
`onResume()`.

3. **`finish()` przed `onResume()`**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

4. **System interruption**

Między `onStart()` i `onResume()` użytkownik albo system mogą otworzyć inny
ekran/app: incoming call, system UI, multi-window, szybkie przejście do
background.

Praktyczny wniosek: nie wolno pisać lifecycle-code z założeniem, że po każdym
`onStart()` obowiązkowo będzie `onResume()`.

Resources trzeba wiązać z właściwą parą callbacks:

```text
visible-only     -> onStart() / onStop()
interactive-only -> onResume() / onPause()
```

Dla Flow:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) { /* visible UI */ }
repeatOnLifecycle(Lifecycle.State.RESUMED) { /* active interaction */ }
```

**Krótko:** `onStart()` bez `onResume()` zdarza się, gdy Activity jest widoczna,
ale nie stała się interactive: overlay, redirect, `finish()`, system
interruption. Cleanup trzeba robić symetrycznie do tego lifecycle state, w którym
resource startuje.

</details>

<details>
<summary>187. Czym są migracje w Room?</summary>

#### Kotlin

Migracja Room — to opis przejścia schema bazy danych z jednej version na inną
bez utraty danych użytkownika.

Migracja jest potrzebna, gdy zmienia się schema:

- dodano albo usunięto tabelę/kolumnę;
- zmieniono type, constraint albo relation;
- zmieniono nazwę tabeli albo pola;
- dodano index albo foreign key.

Po zmianie entity schema trzeba podnieść `version` w `@Database`:

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

Podłączenie:

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

Jeśli dodaje się kolumnę `NOT NULL` dla istniejących rows, potrzebna jest
default value:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

Dla złożonych zmian, których SQLite nie pozwala wykonać bezpośrednio, robi się
copy-table migration:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

`AutoMigration` pasuje do prostych jednoznacznych zmian:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

Dla rename/delete potrzebny jest `AutoMigrationSpec`, a dla transformations,
split/merge tables albo data backfill — manual migration.

`fallbackToDestructiveMigration()` usuwa starą DB i tworzy nową. To dopuszczalne
dla cache/dev DB, ale nie dla user data.

Praktyczne zasady:

- podnosić `version` przy każdej schema change;
- wspierać migration path ze wszystkich production versions;
- eksportować schema przez `exportSchema = true`;
- testować migracje przez `MigrationTestHelper`;
- nie używać destructive fallback dla ważnych danych.

**Krótko:** Room migration przenosi DB schema na nową version z zachowaniem
danych. Proste zmiany może pokryć `AutoMigration`, złożone — manual `Migration`,
a production paths obowiązkowo się testuje.

</details>

<details>
<summary>188. Co robi adnotacja Embedded w Room?</summary>

#### Kotlin

`@Embedded` w Room rozwija pola zagnieżdżonej klasy do kolumn tej samej tabeli
albo query result. Embedded object nie staje się osobną tabelą.

Przykład:

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

Room utworzy kolumny:

```text
id, name, city, street
```

`@Embedded` pasuje do value objects, które logicznie są częścią entity:

- address;
- coordinates;
- money amount + currency;
- metadata;
- reusable group of columns.

Jeśli występuje konflikt nazw kolumn, używa się `prefix`:

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

Kolumny:

```text
start_lat, start_lng, end_lat, end_lng
```

`@Embedded` jest też wygodny w read DTO dla wyników `JOIN`:

```kotlin
data class UserWithStats(
    @Embedded val user: UserEntity,
    @Embedded(prefix = "stats_") val stats: UserStatsEntity
)
```

Różnica względem `@Relation`:

```text
@Embedded -> pola leżą w tej samej row/result
@Relation -> relacja między różnymi tabelami
```

Jeśli object ma własny lifecycle, może być używany przez wiele rows albo wymaga
foreign key — to nie `@Embedded`, tylko osobna table + relation.

Ograniczenia:

- nie tworzy foreign key;
- nie normalizuje danych;
- nie tworzy relation automatycznie;
- może rozdmuchać tabelę liczbą kolumn;
- wymaga `prefix`, jeśli nazwy kolumn się przecinają.

**Krótko:** `@Embedded` wbudowuje pola value object w bieżącą tabelę albo query
result. Dla relacji między tabelami używa się `@Relation`, foreign keys i
osobnych entity.

</details>

<details>
<summary>189. Czym jest DatabaseView w Room?</summary>

#### Kotlin

`@DatabaseView` w Room opisuje SQLite `VIEW` jako Kotlin-klasę. View nie
przechowuje danych osobno, tylko przedstawia wynik SQL query jako read-only
model.

SQL view to nazwane zapytanie:

```sql
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE active = 1;
```

W Room:

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

View trzeba jawnie dodać w `@Database`:

```kotlin
@Database(
    entities = [UserEntity::class],
    views = [ActiveUserView::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase()
```

Odczyt przez DAO:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM ActiveUserView")
    fun observeActiveUsers(): Flow<List<ActiveUserView>>
}
```

`DatabaseView` jest przydatny do:

- złożonych read queries;
- projections dla UI;
- wyników `JOIN`;
- aggregates;
- ponownego użycia jednego SQL w kilku DAO methods.

Przykład z aggregation:

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

Różnica:

```text
Entity       -> realna tabela, przechowuje dane
DatabaseView -> read-only projection, wynik SQL query
@Relation    -> object graph z powiązanych tabel
```

Ograniczenia:

- nie można bezpośrednio `insert/update/delete` w view;
- SQL trzeba utrzymywać przy zmianie schema;
- złożone views mogą być kosztowne dla performance;
- zmiany underlying tables mogą wymagać migration.

**Krótko:** `@DatabaseView` to read-only model nad SQL view. Jest wygodna dla
złożonych read models, joins i aggregates, ale nie zastępuje tabel i nie wspiera
bezpośrednich write operations.

</details>

<details>
<summary>190. Jaka jest różnica między SQL i NoSQL?</summary>

#### Kotlin

`SQL` i `NoSQL` — to różne podejścia do modelowania i przechowywania danych.

```text
SQL   -> tabele, schema, relations, SQL queries
NoSQL -> document/key-value/wide-column/graph storage
```

Przykład SQL:

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
);
```

Przykład NoSQL document:

```json
{
  "id": "1",
  "name": "Alex",
  "settings": {
    "theme": "dark"
  }
}
```

Kluczowe różnice:

```text
Schema:
SQL   -> sztywniejsza schema + migrations
NoSQL -> elastyczniejsza schema, więcej kontroli w app/backend

Relations:
SQL   -> JOIN, foreign keys, normalization
NoSQL -> częściej denormalization pod konkretne reads

Transactions:
SQL   -> zwykle silne ACID guarantees
NoSQL -> zależy od konkretnej DB i konfiguracji

Scaling:
SQL   -> często vertical scaling, replicas, sharding
NoSQL -> często prostszy horizontal scaling
```

Przykład SQL w Androidzie — Room nad SQLite:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

Przykład NoSQL w Androidzie — Firebase Firestore:

```kotlin
firestore.collection("users").document(id).get()
```

SQL lepiej wybrać, gdy:

- struktura danych jest stabilna;
- istnieją relations i złożone queries;
- potrzebne są constraints;
- krytyczne są transakcje i spójność.

NoSQL lepiej pasuje, gdy:

- dane są naturalnie document/key-value;
- schema często się zmienia;
- relations są minimalne;
- potrzebna jest denormalization pod szybkie reads;
- istnieje distributed/high-scale workload.

Wybór robi się nie według mody, ale według access patterns, consistency
requirements, relations, skalowania i doświadczenia zespołu.

**Krótko:** SQL to model relacyjny z tabelami, schema, joins i silnymi
transakcjami. NoSQL to nierelacyjne storage z elastyczniejszym modelem danych,
często z denormalization i prostszym skalowaniem poziomym.

</details>

<details>
<summary>191. Czym jest DataStore?</summary>

#### Kotlin

`DataStore` — Jetpack storage do asynchronicznego zapisywania małych persistent
settings. Działa z Coroutines i `Flow`, jest nowocześniejszą alternatywą dla
`SharedPreferences`.

Typowe dane:

- theme;
- language;
- onboarding state;
- user preferences;
- feature flags.

Dla dużych kolekcji, relations, queries albo offline cache potrzebny jest `Room`,
a nie DataStore.

Istnieją dwa warianty:

```text
Preferences DataStore -> key-value bez schema
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

Proto DataStore używa `.proto` schema:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Różnica względem `SharedPreferences`:

```text
SharedPreferences -> synchronous key-value API
DataStore         -> suspend API, Flow, transactional updates
```

W ViewModel zwykle przekształca się `Flow` na `StateFlow`:

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

Zapis przez `edit` jest transactional read-modify-write:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

Błędy odczytu obsługuje się jawnie:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
    }
```

Do migration ze `SharedPreferences` używa się `SharedPreferencesMigration`.

**Krótko:** `DataStore` zapisuje małe settings przez Coroutines i `Flow`.
`Preferences DataStore` pasuje do key-value, `Proto DataStore` — do type-safe
settings, a złożone relational dane trzeba przechowywać w `Room`.

</details>

<details>
<summary>192. Czym jest Paging 3?</summary>

#### Kotlin

`Paging 3` — Jetpack library do ładowania dużych list częściami. Zarządza
pagination, prefetch, refresh, retry, cancellation i integracją z UI.

Główny pipeline:

```text
PagingSource -> Pager -> Flow<PagingData<T>> -> UI
```

`PagingSource<Key, Value>` opisuje źródło danych i zasady ładowania stron:

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

`Pager` tworzy stream `PagingData`:

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

W ViewModel dodaje się `cachedIn`, aby paging stream żył w `viewModelScope` i nie
startował od zera dla każdego collector:

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

`LoadState` daje osobne stany dla `refresh`, `append`, `prepend`. Zwykle:

- `refresh` — full-screen loading/error;
- `append` — footer loading/error;
- `retry()` — powtórzyć failed load;
- `refresh()` — utworzyć nowe generation.

Dla offline-first używa się `RemoteMediator`:

```text
UI <- PagingSource from Room
          ^
RemoteMediator: API -> Room + remote keys
```

W takim schemacie Room jest source of truth, a `RemoteMediator` doładowuje API
pages i transakcyjnie zapisuje je do DB.

Typowe błędy:

- niepoprawne pagination keys;
- brak `cachedIn`;
- ignorowanie append/prepend errors;
- niestabilne item keys w UI;
- ręczna pagination równolegle z Paging 3.

**Krótko:** `Paging 3` ładuje duże listy stronami. Bazowy łańcuch to
`PagingSource -> Pager -> PagingData -> UI`, `cachedIn` cache'uje stream w
ViewModel, a `RemoteMediator` jest potrzebny dla API + Room offline-first.

</details>

<details>
<summary>193. Czym jest RecyclerView.Adapter?</summary>

#### Kotlin

`RecyclerView.Adapter` — to most między data set i `RecyclerView`. Tworzy
`ViewHolder`, wiąże dane z item UI i informuje o liczbie elementów.

Główne metody:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

Przykład z `ListAdapter`:

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

`RecyclerView` ponownie wykorzystuje item views, dlatego `bind()` musi ustawiać
cały UI state, a nie tylko zmienione pola. Inaczej recycled view może pokazać
state poprzedniego item.

`DiffUtil` określa, co dokładnie się zmieniło:

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

`ListAdapter` + `DiffUtil` jest lepszy niż mutable list i `notifyDataSetChanged()`,
bo aktualizuje tylko zmienione items.

Dla różnych layoutów używa się `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int = when (getItem(position)) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Typowe błędy:

- business logic albo navigation wewnątrz adapter;
- `notifyDataSetChanged()` bez potrzeby;
- niepełny state w `bind()`;
- reference do `Activity` albo `Fragment`;
- przechowywanie starej `position` zamiast pracy z bound item.

**Krótko:** `RecyclerView.Adapter` zamienia dane na recycled item views. W
production ważne są pełny `bind()`, `ListAdapter` z `DiffUtil`, stabilne
przekazywanie click events na zewnątrz i brak business logic w adapter.

</details>

<details>
<summary>194. Jakie główne metody ma RecyclerView.Adapter?</summary>

#### Kotlin

Główne metody `RecyclerView.Adapter`:

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

`onCreateViewHolder()` tworzy nowy `ViewHolder`, gdy nie ma reusable holder:

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

`onBindViewHolder()` wiąże item z holder:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}
```

Przez recycling `bind()` musi ustawiać cały UI state: `true/false`, `null`, empty
values, visibility, selected state.

`getItemCount()` zwraca rozmiar dataset:

```kotlin
override fun getItemCount(): Int = items.size
```

Dla różnych layoutów używa się `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

Partial update można zrobić przez payload overload:

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

Dodatkowe lifecycle callbacks:

- `onViewRecycled()` — cleanup przed ponownym użyciem holder;
- `onViewAttachedToWindow()` — item attached do window;
- `onViewDetachedFromWindow()` — item detached od window.

Aktualizacje dataset muszą być precyzyjne:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

W production częściej używa się `ListAdapter` albo `AsyncListDiffer` z `DiffUtil`,
a nie ręcznego mutable list + `notifyDataSetChanged()`.

Typowe błędy:

- niepełny state w `bind()`;
- ciężka praca w `onBindViewHolder()`;
- przechowywanie starej `position`;
- mutation dataset bez odpowiedniego notify/diff;
- nieunikalne stable IDs.

**Krótko:** główne metody adapter to `onCreateViewHolder()`,
`onBindViewHolder()` i `getItemCount()`. Dodatkowo ważne są `getItemViewType()`,
payload bind, recycling callbacks i poprawne aktualizowanie dataset.

</details>

<details>
<summary>195. Jaka jest różnica między RecyclerView.Adapter i ListAdapter?</summary>

#### Kotlin

`ListAdapter` — to `RecyclerView.Adapter`, który ma już `AsyncListDiffer` i
`DiffUtil`. Zwykły adapter zmusza developera do samodzielnego zarządzania list
storage i `notify...` calls.

```text
RecyclerView.Adapter -> manual list + manual notify
ListAdapter          -> submitList() + async DiffUtil
```

Zwykły `RecyclerView.Adapter`:

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

Problem: mutation dataset i `notify...` muszą dokładnie się zgadzać.
`notifyDataSetChanged()` jest proste, ale zabija granular updates, animations i
część optymalizacji.

`ListAdapter`:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Aktualizacja:

```kotlin
adapter.submitList(users)
```

`ListAdapter` liczy diff poza main thread i stosuje precyzyjne
`insert/remove/move/change` updates na main thread.

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
areItemsTheSame()    -> identity item
areContentsTheSame() -> rendered content
```

Ważne: do `submitList()` trzeba przekazywać immutable snapshot. Nie wolno mutować
tej samej listy ani items po submit, bo diff może nie zobaczyć zmiany.

```kotlin
adapter.submitList(oldList + newUser)
```

Kiedy co wybrać:

- `ListAdapter` — default dla zwykłych dynamic lists;
- `RecyclerView.Adapter` — gdy updates już przychodzą jako precyzyjne operations
  albo potrzebna jest custom storage/differ architecture;
- `PagingDataAdapter` — dla Paging 3.

Typowe błędy:

- niepoprawny `areItemsTheSame()`;
- mutable list po `submitList()`;
- `notifyDataSetChanged()` zamiast diff;
- business logic w adapter;
- stable IDs, które nie zgadzają się z DiffUtil identity.

**Krótko:** `RecyclerView.Adapter` daje pełną ręczną kontrolę nad list i
notifications. `ListAdapter` przyjmuje immutable snapshots przez `submitList()` i
sam stosuje async diff, dlatego jest lepszym defaultem dla większości list.

</details>

<details>
<summary>196. Jak działa RecyclerView pod spodem?</summary>

#### Kotlin

`RecyclerView` pokazuje duże listy bez tworzenia View dla każdego item. Trzyma
widoczne views, mały cache i pool ponownie używanych `ViewHolder`.

```text
10 000 data items
~10-20 visible views
cache + recycled pool
```

Główne komponenty:

- `Adapter` — tworzy `ViewHolder` i binduje dane;
- `ViewHolder` — trzyma item view i references do jej views;
- `LayoutManager` — mierzy i rozmieszcza items;
- `Recycler` / `RecycledViewPool` — ponownie używa holders;
- `ItemAnimator` — animuje zmiany;
- `ItemDecoration` — dodaje dividers, spacing albo custom drawing.

Uproszczony flow:

```text
LayoutManager requests view
 -> Recycler szuka holder w cache/pool
 -> Adapter tworzy holder, jeśli nie ma reuse
 -> Adapter binduje item
 -> LayoutManager measure/layout view
```

Podczas scroll item view, która wyszła poza ekran, odłącza się i może zostać
użyta dla innego item tego samego `viewType`.

Przez recycling `bind()` musi ustawiać cały UI state:

```kotlin
fun bind(item: Item) {
    binding.title.text = item.title
    binding.badge.isVisible = item.isImportant
    binding.checkbox.isChecked = item.isSelected
}
```

Jeśli nie ustawić `false`, `null`, empty albo visibility state, recycled view może
pokazać stan poprzedniego item.

`LayoutManager` określa layout strategy:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
// GridLayoutManager(context, 2)
// StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

`getItemViewType()` rozdziela różne layouty. Pool grupuje holders według
`viewType`, dlatego header nie zostanie reused jako post.

Poziomy reuse w uproszczeniu:

```text
attached scrap -> views bieżącego layout pass
cached views    -> niedawno detached holders
recycled pool   -> holders według viewType do ponownego bind
```

Do aktualizacji listy RecyclerView potrzebuje precyzyjnych notifications albo
diff:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
```

W production zwykle używa się `ListAdapter` / `AsyncListDiffer` z `DiffUtil`.
`notifyDataSetChanged()` nie daje precyzyjnego diff i pogarsza animations/reuse.

Praktyczne zasady:

- `onBindViewHolder()` powinien być tani;
- nie robić file/network I/O ani bitmap decoding w bind;
- click callbacks powinny pracować z bound item, a nie ze starą `position`;
- po insert/remove position może się zmienić;
- images powinien ładować image loader z cache i cancellation.

**Krótko:** `RecyclerView` jest efektywny dzięki recycling `ViewHolder`,
`LayoutManager`, cache/pool i punktowym updates przez `DiffUtil`. Najważniejsze
to pełny i tani `bind()`.

</details>

<details>
<summary>197. Czym jest DiffUtil?</summary>

#### Kotlin

`DiffUtil` — utility dla `RecyclerView`, które porównuje starą i nową listę oraz
znajduje dokładne zmiany: `insert`, `remove`, `move`, `change`.

```text
notifyDataSetChanged() -> odświeżyć wszystko
DiffUtil               -> odświeżyć tylko zmienione
```

Dla `ListAdapter` zwykle pisze się `DiffUtil.ItemCallback`:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

Znaczenie metod:

```text
areItemsTheSame()    -> czy to ten sam item? zwykle stable id
areContentsTheSame() -> czy dane renderowane przez UI są takie same?
```

`ListAdapter` używa `AsyncListDiffer` wewnątrz:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Aktualizacja listy:

```kotlin
adapter.submitList(users)
```

Dla partial update używa się payload:

```kotlin
override fun getChangePayload(oldItem: UserUi, newItem: UserUi): Any? {
    return if (oldItem.isOnline != newItem.isOnline) OnlineStatusChanged
    else null
}
```

Payload pozwala zaktualizować część row bez pełnego rebind.

Ważna jest immutability: do `submitList()` trzeba przekazywać nowy snapshot, a
nie mutować starą listę.

```kotlin
adapter.submitList(oldList + newUser)
```

Typowe błędy:

- `areItemsTheSame()` porównuje cały object zamiast id;
- `areContentsTheSame()` zawsze zwraca `true`;
- niestabilny id;
- mutation list/items po `submitList()`;
- `notifyDataSetChanged()` zamiast diff;
- zbyt ciężki `equals()` dla dużych nested models.

**Krótko:** `DiffUtil` liczy różnicę między starą i nową listą, aby RecyclerView
odświeżył tylko zmienione items. `areItemsTheSame()` odpowiada za identity,
`areContentsTheSame()` — za content, a `submitList()` powinien dostać immutable
snapshot.

</details>

<details>
<summary>198. Jak wyświetlać listy w Androidzie?</summary>

#### Kotlin

W View/XML listy zwykle wyświetla się przez `RecyclerView`, w Jetpack Compose —
przez lazy-kontenery: `LazyColumn`, `LazyRow`, `LazyVerticalGrid`. Dla dużych albo
remote list dodaje się Paging 3.

### View System: RecyclerView

```kotlin
val adapter = UserAdapter(onClick = viewModel::onUserClicked)

binding.recyclerView.apply {
    layoutManager = LinearLayoutManager(requireContext())
    this.adapter = adapter
}

adapter.submitList(users)
```

Dla production lepiej używać `ListAdapter` + `DiffUtil.ItemCallback`, a nie
mutable list + `notifyDataSetChanged()`.

`LayoutManager` określa wygląd listy:

- `LinearLayoutManager` — pionowa albo pozioma lista;
- `GridLayoutManager` — siatka;
- `StaggeredGridLayoutManager` — siatka z różną wysokością items.

### Paging 3

Do ładowania stronami używa się `PagingDataAdapter`:

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

`loadState` jest potrzebny dla `loading`, `error`, `empty`, `retry`. Ważne, aby
osobno obsługiwać `refresh` i `append`, żeby footer loading nie zastępował całego
ekranu.

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

Stabilny `key` zachowuje identity item podczas insert/remove/reorder. Klucz musi
być unikalny i stabilny, nie pozycją.

Inne lazy-kontenery:

```kotlin
LazyRow {
    items(categories, key = Category::id) { CategoryChip(it) }
}

LazyVerticalGrid(columns = GridCells.Fixed(2)) {
    items(products, key = Product::id) { ProductCard(it) }
}
```

Paging w Compose podłącza się przez `collectAsLazyPagingItems()` i również
obsługuje `loadState` oraz `retry()`.

Praktyczne zasady:

- nie dodawać wielu views ręcznie do `LinearLayout`;
- nie używać zwykłego `Column` dla dużej dynamic list;
- przekazywać immutable snapshots;
- trzymać list state w ViewModel;
- pokazywać loading/error/empty/retry states;
- nie robić ciężkiej pracy w `bind()` ani composition.

**Krótko:** w View UI używa się `RecyclerView` z `ListAdapter`, w Compose —
`LazyColumn/LazyRow/LazyVerticalGrid`. Dla dużych remote list dodaje się Paging 3,
stable keys/ids i poprawną obsługę load states.

</details>

<details>
<summary>199. Czym są lazy-kontenery w Compose?</summary>

#### Kotlin

Lazy-kontenery w Compose — to komponenty dla list i siatek, które compose'ują
tylko elementy potrzebne dla viewport, a nie całą kolekcję od razu.

Główne kontenery:

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

Przykład `LazyColumn`:

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

Dla stable identity trzeba przekazać `key`:

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

`key` pomaga zachować item state po insert, delete albo reorder. Klucz musi być
stabilny i unikalny, nie pozycją.

Dla różnych typów items ustawia się `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` pomaga Compose efektywniej ponownie używać composition dla
kompatybilnych item types.

Scroll kontroluje `LazyListState`:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

Programowy scroll:

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

Przykład grid:

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

Paging integruje się przez `collectAsLazyPagingItems()`:

```kotlin
val users = pager.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user -> UserRow(user) }
    }
}
```

Osobno obsługuje się `loadState`: initial loading, append loading, error, empty i
retry.

Zasady performance:

- używać stable `key`;
- ustawiać `contentType` dla różnych item types;
- nie robić I/O ani ciężkich obliczeń w item;
- nie dekodować bitmap w composition;
- unikać zbędnych nested scroll containers;
- przekazywać immutable/stable UI models.

**Krótko:** lazy-kontenery (`LazyColumn`, `LazyRow`, grids) renderują duże listy i
siatki według viewport. Dla production ważne są stable keys, `contentType`, lekki
item UI, `LazyListState` i poprawne Paging load states.

</details>

<details>
<summary>200. Jak zaimplementować cykliczną listę w RecyclerView?</summary>

#### Kotlin

Cykliczny `RecyclerView` zwykle imituje się przez duży `itemCount`: fake adapter
position mapuje się na real item przez modulo.

Podstawowy adapter:

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

Aby na starcie dało się scrollować w obie strony, pozycję ustawia się blisko
środka range:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

Ważne, aby sprawdzać `items.isEmpty()`, inaczej modulo da dzielenie przez zero.
Initial scroll też robi się tylko dla non-empty list.

Dla zachowania carousel dodaje się snapping:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

Auto-scroll powinien być lifecycle-aware:

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

Stable IDs — ryzyko. Nie można zwracać jednego real ID dla wszystkich powtórzeń,
bo adapter positions będą różne. Jeśli stable IDs nie są potrzebne, lepiej ich
nie włączać dla fake-infinite adapter.

Przy aktualizacji dataset trzeba zachowywać immutable real list i po zmianie size
wyrównać current fake position pod nowe modulo.

Typowe problemy:

- modulo dla empty list;
- auto-scroll po `onDestroyView()`;
- nieunikalne stable IDs;
- niepoprawny restore position po zmianie dataset;
- złożona accessibility semantics;
- słaby UX od infinite scroll tam, gdzie nie jest potrzebny.

Alternatywy:

- `ViewPager2` dla pager/banner;
- Compose `HorizontalPager`;
- finite list bez fake infinity;
- duplicated edge items dla seamless transition.

**Krótko:** cykliczny `RecyclerView` robi się przez duży `itemCount`,
`position % items.size` i start ze środka range. Obowiązkowo trzeba obsłużyć
empty list, lifecycle auto-scroll, stable IDs i aktualizacje dataset.

</details>

<details>
<summary>201. Czym są animacje w Androidzie?</summary>

#### Kotlin

Animacja w Androidzie — to zmiana UI-property w czasie: `alpha`, `translation`,
`scale`, `rotation`, color, size albo layout state. Dobra animacja wyjaśnia
zmianę stanu, a nie tylko dodaje dekorację.

### View System

Dla prostych transform-animacji:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

`ObjectAnimator` animuje konkretną property:

```kotlin
ObjectAnimator
    .ofFloat(view, View.ALPHA, 1f, 0f)
    .apply {
        duration = 300
        start()
    }
```

`ValueAnimator` generuje wartości dla custom logic:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    addUpdateListener { animation ->
        progressView.progress = animation.animatedValue as Int
    }
    start()
}
```

`AnimatorSet` łączy kilka animatorów, a `MotionLayout` pasuje do złożonych
transitions między layout states.

### Compose

W Compose animacje powinny być state-driven:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "contentAlpha"
)

Box(Modifier.alpha(alpha))
```

Główne API:

- `animate*AsState` — jedna property;
- `AnimatedVisibility` — enter/exit;
- `AnimatedContent` — zmiana content;
- `updateTransition` — kilka properties jednego state;
- `Animatable` — imperative suspend control;
- `rememberInfiniteTransition` — nieskończone decorative animations.

### Performance

Zwykle taniej animować:

```text
alpha, translation, scale, rotation
```

Animacja `width`, `height`, `margin`, layout params może uruchamiać
measure/layout na każdym frame. To nie jest zakazane, ale wymaga profiling.

W callbacks nie wolno robić I/O, bitmap decoding, ciężkich calculations ani wielu
allocations.

### Lifecycle

View animations trzeba anulować i resetować, szczególnie w `RecyclerView`:

```kotlin
override fun onViewRecycled(holder: ItemHolder) {
    holder.itemView.animate().cancel()
    holder.reset()
}
```

Inaczej recycled holder może pokazać `alpha`, `translation` albo selected state z
poprzedniego item.

Compose effects/coroutines anulują się razem z composition scope, ale external
resources nadal wymagają cleanup.

Praktyczny wybór:

- prosta View transform — `view.animate()`;
- property/value control — `ObjectAnimator` / `ValueAnimator`;
- coordinated transition — `AnimatorSet` / `MotionLayout`;
- Compose UI — state-driven animation APIs;
- RecyclerView — cancel + reset state;
- production — uwzględniać system animation scale/reduced motion.

**Krótko:** Android ma View animators, `MotionLayout` i Compose animation APIs.
Production-animation powinna być state-driven, tania dla frame rendering,
lifecycle-aware i bez zostawionego stanu w recycled views.

</details>

<details>
<summary>202. Jaka jest różnica między ViewBinding i DataBinding?</summary>

#### Kotlin

`ViewBinding` i `DataBinding` usuwają ręczne `findViewById`, ale rozwiązują różne
zadania.

```text
ViewBinding -> type-safe dostęp do View
DataBinding -> binding danych i expressions w XML
```

### ViewBinding

Generuje binding-klasę dla XML layout:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Plusy:

- prosty API;
- type-safe dostęp do views;
- mniejszy build overhead;
- łatwiejszy debug;
- nie przenosi logiki do XML.

### DataBinding

Pozwala deklarować variables i expressions bezpośrednio w XML:

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

W Kotlin:

```kotlin
binding.user = user
binding.executePendingBindings()
```

DataBinding wspiera też two-way binding:

```xml
<EditText
    android:text="@={viewModel.query}" />
```

To może być wygodne dla prostych formularzy, ale w dużych projektach często
komplikuje debug data flow.

Główne ryzyka DataBinding:

- bardziej złożony code generation;
- większy build/runtime overhead;
- mniej oczywiste błędy;
- pokusa przenoszenia UI/business logic do XML;
- trudniejszy onboarding dla zespołu.

Zły kierunek — złożona logika w layout:

```xml
android:visibility="@{user.isAdmin ? View.VISIBLE : View.GONE}"
```

Proste expressions są dopuszczalne, ale złożoną logikę lepiej trzymać w
ViewModel albo mapper.

Praktyczny wybór:

- nowy XML/View System code — zwykle `ViewBinding`;
- legacy projekt z XML expressions — może zostać `DataBinding`;
- nowy declarative UI — częściej Compose.

**Krótko:** `ViewBinding` to prosty i lekki type-safe dostęp do views.
`DataBinding` dodaje variables, expressions i two-way binding w XML, ale ma
większy overhead i ryzyko ukrycia logiki w layout.

</details>

<details>
<summary>203. Czym jest Serializable?</summary>

#### Kotlin

`java.io.Serializable` — marker interface dla Java binary serialization. JVM może
zapisać object graph do bytes przez `ObjectOutputStream` i odtworzyć przez
`ObjectInputStream`.

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

To nie to samo co `kotlinx.serialization.Serializable`: tam annotation generuje
serializers dla JSON, Proto i innych formatów.

W Androidzie `Serializable` można przekazywać przez `Intent` albo `Bundle`:

```kotlin
val intent = Intent(context, DetailsActivity::class.java)
    .putExtra("user", user)
```

Od API 33 istnieje typed overload:

```kotlin
val user = intent.getSerializableExtra(
    "user",
    User::class.java
)
```

Dla różnych API levels lepiej używać compatibility helpers, na przykład
`IntentCompat.getSerializableExtra()`.

Porównanie z `Parcelable`:

```text
Serializable -> JVM runtime serialization, mniej kodu, większy overhead
Parcelable   -> Android IPC/Bundle format, szybszy i bardziej jawny
```

Dla Android arguments zwykle lepszy jest `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Jeszcze lepiej dla navigation — przekazywać primitive `id`, a aktualne dane
ładować z repository.

Dla version compatibility Java serialization używa `serialVersionUID`:

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

Jawny UID utrwala version identifier, ale nie robi schema evolution bezpieczną
automatycznie.

Ryzyka:

- reflection/metadata overhead;
- słaba schema evolution;
- wszystkie nested fields też muszą być serializable;
- możliwy `NotSerializableException`;
- duże objects w `Bundle/Intent` mogą dać `TransactionTooLargeException`;
- nie wolno przyjmować niezaufanych serialized bytes z network/external storage.

```kotlin
// bad
intent.putExtra("response", hugeResponse)

// better
intent.putExtra("user_id", user.id)
```

Kiedy dopuszczalne:

- mały internal argument w legacy-code;
- krótkotrwały object w obrębie jednej app;
- performance i schema evolution nie są krytyczne.

**Krótko:** Java `Serializable` serializuje object graph runtime-mechanizmem JVM.
W Androidzie dla arguments częściej lepszy jest `Parcelable` albo samo `id`, a
dla persistence/network — jawna schema: Room, DataStore, JSON albo Proto.

</details>

<details>
<summary>204. Jakie są sposoby serializacji w Androidzie?</summary>

#### Kotlin

Serializacja — to przekształcenie object w format do przesyłania albo
przechowywania: JSON, bytes, `Bundle`, database row, protobuf itd.

W Androidzie najczęściej jest potrzebna do:

- API/network;
- navigation arguments;
- `Intent` / `Bundle` / IPC;
- local storage;
- settings.

### JSON

Dla API najczęściej używa się JSON: Kotlin Serialization, Moshi albo Gson.

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

Plusy Kotlin Serialization: dobra obsługa Kotlin, `sealed class`, compile-time
plugin, Ktor, multiplatform.

Moshi często używa się z Retrofit. Dla production lepszy jest codegen, a nie
reflection.

Gson jest prosty i popularny w legacy, ale gorzej działa z Kotlin nullability,
jest reflection-heavy i może po cichu tworzyć niepoprawne objecty.

### Parcelable

`Parcelable` — Android format do przekazywania objects przez `Intent`, `Bundle`,
Navigation args albo IPC.

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Dla navigation często lepiej przekazywać nie cały object, tylko primitive `id`:

```kotlin
bundleOf("user_id" to userId)
```

Potem aktualne dane ładuje się z repository/ViewModel.

### Serializable

Java `Serializable` jest prosty, ale ma runtime overhead, słabszą schema
evolution i gorszą kontrolę.

```kotlin
data class User(val id: String) : Serializable
```

W Androidzie dla arguments zwykle lepszy jest `Parcelable` albo `id`.

### Room

Dla local DB używa się Room entities i mappers, a nie serializacji domain object
bezpośrednio:

```kotlin
data class UserEntity(
    val id: String,
    val name: String
)

fun UserEntity.toDomain() = User(id, name)
```

`TypeConverter` można używać dla pojedynczych złożonych pól, ale nie warto
zamieniać Room w JSON-storage bez potrzeby.

### Proto / DataStore

Dla type-safe settings albo compact binary format pasuje Proto DataStore. Wymaga
schema, ale daje stabilny typed model.

Praktyczny wybór:

- API JSON — Kotlin Serialization albo Moshi;
- Retrofit legacy — Moshi/Gson zależnie od projektu;
- navigation args — primitives/id albo `Parcelable`;
- Intent/Bundle — mały `Parcelable`, nie duży payload;
- local DB — Room entities + mappers;
- settings — DataStore albo Proto DataStore;
- Java `Serializable` — tylko dla legacy/simple internal cases.

**Krótko:** dla network zwykle wybiera się Kotlin Serialization albo Moshi, dla
Android arguments — `Parcelable` albo primitive `id`, dla DB — Room entities, dla
settings — DataStore/Proto. `Serializable` i Gson lepiej zostawić dla legacy albo
prostych scenariuszy.

</details>

<details>
<summary>205. Co zrobić, jeśli pola może nie być w odpowiedzi API?</summary>

#### Kotlin

DTO powinien dokładnie odzwierciedlać API contract: missing field, explicit
`null` i invalid required field to różne przypadki.

```text
missing optional field -> default value
explicit null          -> nullable type
missing required field -> decoding/mapping error
```

Jeśli backend może wysłać `null`, pole robi się nullable:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

Fallback lepiej ustawiać w mapper, a nie ciągnąć nullable wszędzie do UI:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name?.takeIf { it.isNotBlank() } ?: "Unknown",
    avatarUrl = avatarUrl
)
```

Jeśli pola może brakować, ale ma bezpieczną wartość domyślną:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Missing i explicit `null` to nie to samo:

```json
{}
```

```json
{ "name": null }
```

Dla `kotlinx.serialization` default value używa się dla missing key. Nullable
property bez default nadal może być required, zależnie od declaration/config.
Zachowanie `explicitNulls` trzeba rozumieć i pokrywać testami.

DTO nie warto przekazywać bezpośrednio do UI. Mapper powinien tworzyć domain/UI
model z jasnymi guarantees:

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

Required field nie należy maskować fallbackiem:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

Repository może przekształcić invalid response na typed error:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

Praktyczne zasady:

- optional missing field — default;
- explicit `null` — nullable type;
- required field — validation/failure;
- DTO → mapper → domain/UI;
- serializer behavior pokrywać contract tests: missing, null, wrong type, unknown
  enum.

**Krótko:** brakujące pole modeluje się default value, explicit `null` — nullable
type. Required field bez wartości powinien dawać validation error, a nie cichy
fallback. DTO trzeba mapować na domain/UI model.

</details>

<details>
<summary>206. Czym jest Dependency Injection?</summary>

#### Kotlin

`Dependency Injection` — to przekazywanie zależności klasy z zewnątrz zamiast
tworzenia ich wewnątrz klasy. Zmniejsza to coupling, upraszcza testowanie i
zarządzanie lifecycle.

Źle bez DI:

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Problemy:

- trudno zastąpić dependency w teście;
- klasa sama odpowiada za creation;
- implementation jest sztywno powiązana;
- lifecycle/configuration są kontrolowane nie tam, gdzie trzeba.

Lepszy default — constructor injection:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

Klasa otrzymuje gotową dependency i nie wie, kto ją utworzył.

Zwykle caller zależy od contract:

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

W Androidzie DI łączy `ViewModel`, use cases, repositories, API clients, Room DB,
dispatchers.

Przykład Hilt:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Binding interface:

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

Dla third-party objects używa się `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

Scopes określają lifetime instance:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

Scope powinien odpowiadać ownerowi. Zbędny singleton może trzymać object dłużej,
niż trzeba, i stworzyć leak.

W testach real implementation zastępuje się fake:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

DI nie musi wymagać framework. Dla małego graph wystarczy manual DI:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Praktyczne zasady:

- constructor injection — default;
- caller zależy od interface/contract;
- scopes odpowiadają lifecycle;
- Hilt/Dagger/Koin tylko automatyzują wiring;
- DI nie zastępuje normalnej architektury.

**Krótko:** DI przekazuje zależności z zewnątrz. Zmniejsza coupling, pozwala
zastępować dependencies w testach i pomaga zarządzać lifecycle. Dla małego graph
wystarczy manual DI, dla dużego — Hilt/Dagger/Koin.

</details>

<details>
<summary>207. Jaka jest różnica między Dagger/Hilt i Koin?</summary>

#### Kotlin

`Dagger/Hilt` i `Koin` — rozwiązania DI z różnym modelem wiring.

```text
Dagger/Hilt -> generated code + compile-time graph validation
Koin        -> runtime container + Kotlin DSL/annotations
```

### Dagger / Hilt

`Dagger` generuje factories/components i sprawdza dependency graph podczas
compilation.

Plusy:

- compile-time validation;
- generated code bez runtime lookup;
- jawne scopes/components;
- dobrze pasuje do dużych strict graphs.

Minusy:

- więcej annotations/modules;
- bardziej złożona component hierarchy;
- compiler errors czasem trudno czytać;
- może wpływać na build time.

`Hilt` — Android layer nad Dagger. Daje standardową Android component hierarchy,
scopes i Jetpack integrations.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

Typowe scopes Hilt:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

### Koin

Koin opisuje dependencies przez Kotlin DSL albo annotations:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Plusy:

- mniej ceremony;
- DSL łatwo czytać;
- szybki start w małych/średnich projektach;
- wygodny dla custom wiring i KMP use cases.

Minusy:

- classic DSL ma więcej runtime lookup;
- missing/ambiguous definitions mogą ujawnić się runtime, jeśli nie ma
  compile-time checks;
- scopes i ownership graph zespół musi modelować zdyscyplinowanie.

Współczesny Koin ma tooling/annotations dla compile-time checks, ale model nadal
różni się od Dagger: Koin pozostaje container-based, a Dagger/Hilt —
generated graph-based.

### Performance i testing

Dagger/Hilt wywołuje generated factories. Koin resolve'uje definitions przez
container. Różnicę warto mierzyć na realnym graph, a nie wybierać framework po
microbenchmark.

Dla testów:

- Hilt — test components, `@TestInstallIn`, module replacement;
- Koin — test modules, overrides;
- unit tests — najprościej przekazywać fake bezpośrednio przez constructor.

### Co wybrać

- Hilt — dobry default dla Androida z Jetpack i dużym graph;
- Dagger — gdy potrzebny jest bardzo jawny i kontrolowany DI graph;
- Koin — gdy ważne są prostota DSL, szybkość wiring albo KMP;
- existing working DI nie warto przepisywać bez mierzalnej korzyści.

**Krótko:** Dagger/Hilt generuje DI code i sprawdza graph na compile time. Koin
daje prostszy DSL/container-based approach, ale wymaga dyscypliny w scopes i
checks. Wybór zależy od size graph, build/runtime wymagań i doświadczenia
zespołu.

</details>

<details>
<summary>208. Czym jest CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` — mechanizm Jetpack Compose do przekazywania wartości w dół
composition tree bez jawnego przepychania parametrów przez każdy composable.

To scoped UI context dla subtree:

```kotlin
CompositionLocalProvider(LocalSpacing provides AppSpacing()) {
    AppContent()
}
```

Deklaracja:

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

Użycie:

```kotlin
@Composable
fun ProfileCard() {
    val spacing = LocalSpacing.current

    Column(Modifier.padding(spacing.medium)) {
        Text("Profile")
    }
}
```

`ProfileCard` czyta najbliższą wartość `LocalSpacing`, provided wyżej w tree.

Provider działa tylko na child subtree:

```kotlin
CompositionLocalProvider(
    LocalSpacing provides AppSpacing(medium = 20.dp)
) {
    ProfileScreen()
}
```

Różnica API:

```text
compositionLocalOf       -> dokładniejsze tracking reads, dla zmiennych values
staticCompositionLocalOf -> tańszy read, dla prawie statycznych values
```

Dla design system tokens często używa się `staticCompositionLocalOf`. Dla values,
które zmieniają się częściej, — `compositionLocalOf`.

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

CompositionLocal jest właściwy dla:

- theme colors;
- typography;
- spacing;
- design tokens;
- locale/configuration;
- UI helpers, które logicznie są dostępne całemu subtree.

Nie warto ukrywać przez CompositionLocal biznesowych zależności:

```kotlin
val repository = LocalUserRepository.current // źle
```

Repositories, use cases i services powinny przychodzić przez DI/ViewModel.
Inaczej CompositionLocal zamienia się w service locator.

Ryzyka:

- ukryte zależności;
- trudniejsze testowanie;
- niejasne pochodzenie value;
- zbędne recompositions przy częstych zmianach;
- nadużywanie zamiast jawnych parameters.

**Krótko:** `CompositionLocal` przekazuje scoped UI-values w dół Compose tree bez
parameter drilling. To dobre dla theme, spacing, typography i context-like UI
data, ale nie dla repositories, use cases ani logiki biznesowej.

</details>

<details>
<summary>209. Jakie są rodzaje CompositionLocal?</summary>

#### Kotlin

W Compose istnieją dwa główne sposoby utworzenia `CompositionLocal`:

```text
staticCompositionLocalOf -> tańszy read, mniej precyzyjne invalidation
compositionLocalOf       -> tracked reads, precyzyjniejsza recomposition
```

`staticCompositionLocalOf`:

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

Reads nie są śledzone osobno. Jeśli value się zmieni, invalidated będzie cały
content providera. To dobre dla prawie statycznych UI values:

- theme colors;
- typography;
- spacing;
- design-system tokens.

`compositionLocalOf`:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

Compose śledzi, które composables czytały `.current`, i przy zmianie value
invalidates precyzyjniej. To lepsze dla values, które mogą zmieniać się częściej.

Provider ustawia value dla subtree:

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

Zagnieżdżony provider może nadpisać value tylko dla swojego subtree.

Przykład design system local:

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

Użycie:

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

Opisują UI environment bieżącego subtree.

Nie warto używać CompositionLocal jako service locator:

```kotlin
val repository = LocalRepository.current // źle
```

Repositories, use cases i business services powinny przychodzić przez
DI/ViewModel. Ważne inputs konkretnego composable lepiej przekazywać jawnymi
parameters.

Ryzyka:

- ukryte zależności;
- trudniejsze testy;
- niejasny owner value;
- zbędne recompositions;
- zastępowanie DI i state hoisting.

**Krótko:** `staticCompositionLocalOf` pasuje do stabilnych UI values i tańszych
reads. `compositionLocalOf` śledzi reads precyzyjniej i jest lepszy dla zmiennych
values. Obu należy używać dla UI context, a nie dla biznesowych zależności.

</details>

<details>
<summary>210. Jak działa CompositionLocal pod spodem?</summary>

#### Kotlin

`CompositionLocal` działa jak scoped value w Compose tree. `CompositionLocalProvider`
zapisuje value dla subtree, a composables niżej czytają najbliższą wartość przez
`.current`.

Podstawowy model:

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

CompositionLocalProvider(LocalSpacing provides AppSpacing(16.dp)) {
    Content()
}
```

Odczyt:

```kotlin
val spacing = LocalSpacing.current
```

Compose szuka najbliższego providera dla `LocalSpacing` w composition hierarchy.

Provider działa tylko na subtree:

```text
Root
 ├─ LocalSpacing = 8dp
 │   └─ ScreenA reads 8dp
 └─ LocalSpacing = 16dp
     └─ ScreenB reads 16dp
```

Nested provider przesłania zewnętrzny:

```kotlin
CompositionLocalProvider(LocalSpacing provides smallSpacing) {
    Header()

    CompositionLocalProvider(LocalSpacing provides largeSpacing) {
        Content()
    }
}
```

`Header` dostanie `smallSpacing`, `Content` — `largeSpacing`.

Compose używa reads do invalidation. Gdy composable czyta:

```kotlin
val colors = LocalAppColors.current
```

Compose może powiązać ten read z value i przy zmianie zdecydować, co trzeba
recomposition.

Różnica między typami:

```text
compositionLocalOf       -> tracked reads, dokładniejsze invalidation
staticCompositionLocalOf -> reads nie są trackowane osobno, tańszy read
```

`compositionLocalOf` jest lepszy dla values, które mogą zmieniać się częściej:

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`staticCompositionLocalOf` jest lepszy dla prawie statycznych values: theme,
typography, spacing.

```kotlin
val LocalTypography = staticCompositionLocalOf {
    DefaultTypography
}
```

Default factory wywołuje się, jeśli value nie zostało provided:

```kotlin
val LocalAnalytics = staticCompositionLocalOf<Analytics> {
    error("Analytics not provided")
}
```

Dla obowiązkowych values często lepiej rzucać `error`, aby szybko znaleźć
niepoprawny setup.

Niuans performance: jeśli CompositionLocal często się zmienia i jest czytany w
dużym subtree, można dostać zbędne recompositions. Nie warto wkładać tam
high-frequency state: scroll position, text input value, timer ticks.

Praktyczna zasada: `CompositionLocal` to nie globalna zmienna, tylko scoped UI
context. Używa się go dla UI environment values, a nie dla ukrytego dostępu do
repositories/use cases.

**Krótko:** `CompositionLocalProvider` ustawia value dla subtree, `.current`
czyta najbliższe value, a Compose używa reads do invalidation.
`compositionLocalOf` dokładniej trackuje reads, `staticCompositionLocalOf` jest
tańszy dla prawie statycznych values.

</details>

<details>
<summary>211. Czym są side effects w Compose?</summary>

#### Kotlin

Side effect w Compose — to działanie poza czystym opisem UI: coroutine, listener,
navigation, snackbar, analytics albo synchronizacja z external object.

Composable body może wykonywać się wiele razy, dlatego effects nie uruchamia się
bezpośrednio:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // źle: powtórzy się przy recomposition
    Text(userId)
}
```

### LaunchedEffect

Uruchamia coroutine powiązaną z composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Przy zmianie key poprzednia coroutine jest anulowana i startuje nowa. Przy wyjściu
z composition coroutine też jest anulowana. Keys powinny zawierać dependencies
effect.

`LaunchedEffect(Unit)` — jeden start dla bieżącego wejścia do composition, nie raz
na całe życie app.

### rememberCoroutineScope

Potrzebny, gdy coroutine startuje z callback:

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

`LaunchedEffect` startuje deklaratywnie według keys, `rememberCoroutineScope` —
imperatywnie po zdarzeniu użytkownika.

### DisposableEffect

Używany do register/unregister lifecycle:

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

Każda rejestracja listener/observer powinna mieć symetryczny cleanup w
`onDispose`.

### SideEffect

Wykonuje się po każdej udanej composition:

```kotlin
SideEffect {
    externalController.enabled = isEnabled
}
```

Nadaje się do publikowania Compose state do non-Compose object. Nie nadaje się do
suspend ani ciężkiej pracy.

### Inne API

`rememberUpdatedState` daje aktualną lambda/value w long-running effect bez
restart:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

`snapshotFlow` przekształca reads Compose state w Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect(analytics::trackScroll)
}
```

`produceState` adaptuje suspend/callback source do Compose `State`. Dla Flow w UI
częściej używa się `collectAsStateWithLifecycle()`.

`derivedStateOf` — to nie side effect, tylko derived state do optymalizacji
obliczonego UI state.

Praktyczne zasady:

- nie uruchamiać effects w composable body;
- poprawnie ustawiać keys;
- cleanup robić w `onDispose`;
- dla callback coroutine używać `rememberCoroutineScope`;
- dla aktualnej lambda bez restart — `rememberUpdatedState`;
- durable data przechowywać jako state, a nie jako jednorazowy effect.

**Krótko:** side effects w Compose zarządzają działaniami poza opisem UI.
`LaunchedEffect` uruchamia coroutine według keys, `DisposableEffect` zarządza
cleanup, `SideEffect` synchronizuje external object, a `rememberCoroutineScope`
jest potrzebny dla coroutine z callback.

</details>

<details>
<summary>212. Jak optymalizować recomposition?</summary>

#### Kotlin

Recomposition nie trzeba „wyłączać”. Powinna być lokalna i tania. Problemy
pojawiają się przez szerokie state reads, niestabilne parametry albo ciężką pracę
w composable body.

### State na właściwym poziomie

State trzeba trzymać w najniższym wspólnym ownerze, który go potrzebuje.

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

Jeśli `query` jest potrzebny tylko search-blokowi, nie trzeba czytać go na
poziomie całego screen.

### Zawężać reads

Sam podział na funkcje nie gwarantuje optymalizacji. Composable powinien czytać
tylko potrzebny state i otrzymywać stabilne parameters — wtedy Compose może
skipnąć niezmienione groups.

### Immutable/stable modele

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

`@Immutable` ustawia się tylko wtedy, gdy contract naprawdę jest spełniony.
Mutable properties i in-place mutations psują przewidywalność.

Poprawnie aktualizować state immutable snapshotem:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

### Lazy lists

Dla lazy items potrzebny jest stable `key`:

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

`key` zachowuje remembered state przy insert/delete/reorder. Dla mixed lists
dodatkowo ustawia się `contentType`.

### Nie robić ciężkiej pracy w composition

Transformation albo wynosi się do state holder, albo cache'uje według input:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

Nie należy robić I/O, bitmap decoding, heavy sorting/filtering bez cache
bezpośrednio w composable body.

### derivedStateOf

`derivedStateOf` jest przydatny, gdy source zmienia się często, a UI reaguje
tylko na pochodny warunek:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

### Profilowanie

Optymalizację trzeba potwierdzać narzędziami:

- recomposition counters;
- Layout Inspector;
- Compose compiler reports;
- System Trace / Perfetto;
- Android Studio Profiler;
- Macrobenchmark w release-like build.

Wysoki recomposition count sam w sobie nie zawsze jest problemem, jeśli
composable jest tani.

**Krótko:** recomposition optymalizuje się przez wąskie state reads,
immutable/stable models, immutable updates, stable keys w lazy lists,
`remember/derivedStateOf` i wynoszenie ciężkiej pracy poza composition. Mierzyć
trzeba profilerem, a nie na oko.

</details>

<details>
<summary>213. Jak napisać test asynchroniczny bez użycia coroutines?</summary>

#### Kotlin

Test asynchroniczny bez coroutines powinien czekać na konkretne zdarzenie:
callback, result, state update albo idle state. Oczekiwanie zawsze musi mieć
timeout. `Thread.sleep()` to zły wariant.

### CountDownLatch

Dla callback-based API:

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

Dla jednego result/error:

```kotlin
val future = CompletableFuture<User>()

repository.loadUser("42", object : UserCallback {
    override fun onSuccess(user: User) = future.complete(user)
    override fun onError(error: Throwable) = future.completeExceptionally(error)
})

assertEquals("42", future.get(2, TimeUnit.SECONDS).id)
```

### Oczekiwanie na state

Można używać Awaitility albo własnego polling helper:

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

Test kończy się od razu po spełnieniu warunku, a nie czeka stałej zwłoki.

### LiveData

Dla `LiveData` używa się `InstantTaskExecutorRule` i helper typu
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

Helper powinien zasubskrybować, czekać na value z timeout i usunąć observer w
`finally`.

### UI tests

Espresso synchronizuje się przez `IdlingResource`:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

### Fake dependency

Najstabilniejszy unit test to kontrolowany fake:

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

Test sam wywołuje `complete()`, dlatego nie zależy od timing network/thread
scheduler.

Antywzorzec:

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

Krótka zwłoka daje flaky test, długa — marnuje czas.

**Krótko:** test asynchroniczny bez coroutines powinien czekać na konkretne
zdarzenie z timeout. Do tego używa się `CountDownLatch`, `CompletableFuture`,
polling/Awaitility, LiveData helper, Espresso `IdlingResource` albo
kontrolowanego fake, ale nie `Thread.sleep()`.

</details>

<details>
<summary>214. Jaka jest różnica między inline, noinline i crossinline?</summary>

#### Kotlin

`inline`, `noinline` i `crossinline` — to modyfikatory dla higher-order functions.
Sterują tym, jak compiler pracuje z lambda-parametrami.

```text
inline      -> wbudować function/lambda w call site
noinline    -> nie inline'ować konkretnej lambda
crossinline -> inline'ować, ale zabronić non-local return
```

### inline

`inline` prosi compiler, aby wstawił ciało funkcji i lambda w miejsce wywołania:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Zmniejsza to overhead tworzenia lambda object i function call dla małych
higher-order functions.

`inline` jest też potrzebny dla `reified` generics:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T =
    fromJson(json, T::class.java)
```

Bez `inline` `reified` type parameter nie jest możliwy.

### Non-local return

W inline lambda można zrobić `return` z zewnętrznej funkcji:

```kotlin
inline fun runBlock(block: () -> Unit) = block()

fun test() {
    runBlock {
        return // wyjście z test()
    }
}
```

To nazywa się non-local return.

### noinline

`noinline` wyłącza inline dla konkretnej lambda:

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

`noinline` jest potrzebne, jeśli lambda trzeba zapisać do zmiennej, przekazać
dalej albo użyć jako object.

### crossinline

`crossinline` zostawia inline, ale zabrania non-local return:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

To potrzebne, gdy lambda wykonuje się w innym context, na przykład wewnątrz
`Runnable`, callback albo innej lambda.

Przykład razem:

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

Sens:

- `inlined` — w pełni inline;
- `stored` — można zapisywać/przekazywać;
- `safe` — inline, ale bez non-local return.

`inline` nie należy dodawać wszędzie. Dla dużych functions albo częstych call
sites może rozdąć bytecode. Najlepiej pasuje do małych higher-order utilities,
DSL i `reified` generics.

**Krótko:** `inline` wbudowuje function/lambda i jest potrzebny dla `reified`;
`noinline` zostawia lambda object, aby można było ją zapisywać albo przekazywać;
`crossinline` zabrania non-local return, gdy lambda wykonuje się w innym context.

</details>

<details>
<summary>215. Jakie są przewagi Kotlina nad Javą?</summary>

#### Kotlin

Kotlin ma praktyczne przewagi nad Javą, szczególnie w Androidzie: null safety,
mniej boilerplate, coroutines, extension functions, data/sealed classes, smart
casts i silną integrację z Jetpack.

### Null safety

Kotlin rozdziela typy nullable i non-null:

```kotlin
val name: String = "Alex"
val nickname: String? = null
```

Nullable values trzeba obsługiwać jawnie:

```kotlin
val title = user.name ?: "Unknown"
```

To nie usuwa wszystkich `NullPointerException`, ale mocno zmniejsza ich liczbę.

### Mniej boilerplate

```kotlin
data class User(
    val id: String,
    val name: String
)
```

`data class` generuje `equals`, `hashCode`, `toString`, `copy`, component
functions.

### Coroutines

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = State.Success(user)
}
```

Coroutines sprawiają, że async-code jest czytelniejszy niż callbacki i wygodny
dla structured concurrency.

### Extension functions

```kotlin
fun View.show() {
    visibility = View.VISIBLE
}
```

Pozwalają dodawać utility API bez inheritance i wrapper classes.

### Sealed classes/interfaces

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}
```

Dobrze pasują do UI state, results, effects i finite state models.

### Smart casts

```kotlin
if (state is UiState.Content) {
    Text(state.user.name)
}
```

Po type check Kotlin sam robi cast.

### Named/default arguments

```kotlin
fun createUser(name: String, active: Boolean = true)

createUser(name = "Alex")
```

Mniej overloads, czytelniejsze call sites.

### Java interoperability

Kotlin dobrze działa z Java-code i libraries, dlatego projekt można migrować
stopniowo.

### Android ecosystem

Wiele modern Android API jest Kotlin-first:

- Jetpack Compose;
- Coroutines/Flow;
- KTX extensions;
- Room suspend/Flow APIs;
- Lifecycle scopes;
- Navigation Kotlin DSL.

Tradeoffs też istnieją:

- build time może być gorszy;
- coroutines trzeba rozumieć głęboko;
- Java interop daje platform types;
- złożone generics/type inference edge cases;
- nadużywanie DSL/extensions pogarsza czytelność.

**Krótko:** Kotlin daje bezpieczniejszy i bardziej ekspresyjny kod: null safety,
data/sealed classes, coroutines, extension functions, smart casts, named/default
arguments i lepszy Android tooling. Ale jakość zależy od architektury i
dyscypliny, a nie tylko od języka.

</details>

<details>
<summary>216. Jaka jest różnica między val i const val?</summary>

#### Kotlin

`val` i `const val` są read-only, ale to różne rzeczy.

```text
val       -> read-only property, wartość może być runtime
const val -> compile-time constant
```

`val`:

```kotlin
val name = "Alex"
val createdAt = System.currentTimeMillis()
```

`val` nie można ponownie przypisać po inicjalizacji, ale wartość może zostać
obliczona podczas runtime.

`const val`:

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

`const val` musi być znane compilerowi i jest inline'owane w miejsca użycia.

Gdzie można deklarować `const val`:

- top-level;
- w `object`;
- w `companion object`.

```kotlin
object ApiConfig {
    const val BASE_PATH = "api/v1"
}
```

Nie można zadeklarować `const val` jako local variable wewnątrz funkcji.

Typy dla `const val` są ograniczone do compile-time literals:

- `String`;
- primitives: `Int`, `Long`, `Boolean`, `Double` itd.

Nie można:

```kotlin
const val DATE = LocalDate.now() // błąd
```

`const val` jest potrzebne tam, gdzie API wymaga compile-time constant, na
przykład annotations:

```kotlin
const val TABLE_USERS = "users"

@Entity(tableName = TABLE_USERS)
data class UserEntity(...)
```

Zwykły `val` tutaj nie zadziała, jeśli annotation oczekuje constant.

Java interop: `const val` kompiluje się jako `static final` field i może być
używane z Javy jako stała.

Ryzyka:

- `const val` jest inline'owane, dlatego po zmianie wartości w library zależne
  moduły mogą wymagać recompilation;
- `const val` nie chroni secrets/API keys — wartość łatwo wydobyć z APK;
- nie trzeba robić `const val` ze wszystkiego, co po prostu „się nie zmienia”.

Praktyczny wybór:

- runtime value albo object — `val`;
- literal compile-time constant — `const val`;
- wartość dla annotation — `const val`;
- secrets — nie `const val`, tylko normalna security/config strategy.

**Krótko:** `val` to read-only property, która może być runtime value.
`const val` to compile-time constant dla `String`/primitives, dozwolona top-level
albo w `object/companion object`, i pasuje do annotations. Secrets nie wolno
przechowywać w `const val`.

</details>

<details>
<summary>217. Jakich typów można używać z const val?</summary>

#### Kotlin

`const val` — to compile-time constant. Typ musi być `String` albo primitive, a
initializer musi być znany compilerowi bez wykonywania programu.

Dozwolone typy:

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

Typowe przykłady:

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Gdzie można deklarować:

- top-level;
- w `object`;
- w `companion object`.

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

Nie można zadeklarować `const val` jako local variable ani instance property.

Czego nie wolno:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val UUID_VALUE = UUID.randomUUID()
const val DEFAULT_THEME = ThemeMode.Light
const val NAME: String? = null
```

Dla objects, collections, nullable values i runtime expressions używa się
zwykłego `val`:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

Initializer może zawierać literals, inne constants i compile-time operations:

```kotlin
const val HOST = "example.com"
const val API_URL = "https://" + HOST
const val TIMEOUT_SECONDS = 5 * 2
```

Function calls, `Context`, current time, random values albo object creation nie
są compile-time constants.

`const val` często jest potrzebne dla annotation arguments:

```kotlin
const val USERS_TABLE = "users"

@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

**Krótko:** `const val` wspiera tylko `String` i primitive types z compile-time
initializer. Dla objects, collections, nullable albo runtime values trzeba używać
zwykłego `val`.

</details>

<details>
<summary>218. Czy można utworzyć data class bez parametrów?</summary>

#### Kotlin

Nie. `data class` musi mieć primary constructor z co najmniej jednym parameter, i
ten parameter musi być `val` albo `var`.

Niepoprawnie:

```kotlin
data class Empty
data class Empty()
data class User(id: String)
```

Poprawnie:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Powód: compiler generuje methods właśnie z primary-constructor properties:

```text
equals()
hashCode()
toString()
copy()
componentN()
```

Property w body nie wchodzi do generated equality, hash, copy i destructuring:

```kotlin
data class User(
    val id: String
) {
    var cachedLabel: String = ""
}
```

Dwa `User` z tym samym `id`, ale różnym `cachedLabel`, będą równe. Dlatego
value-defining fields powinny być w primary constructor.

Wywołanie bez arguments jest możliwe przez default values:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)

val user = User()
```

Klasa nadal ma parameters, tylko caller używa defaults. Ale defaults nie powinny
tworzyć invalid domain object tylko dla framework.

Jeśli framework potrzebuje no-arg constructor, lepiej użyć:

- osobnej persistence/DTO model;
- adapter/mapper;
- Kotlin no-arg plugin;
- valid defaults, jeśli naprawdę mają sens.

Jeśli state nie ma payload, lepiej użyć singleton:

```kotlin
data object Loading
```

W sealed hierarchy:

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

Inne ograniczenia: `data class` nie może być `abstract`, `open`, `sealed` ani
`inner`, ale może implementować interfaces.

Praktyczny wybór:

- value model — `data class`;
- no-arg call — default values;
- state bez payload — `object` / `data object`;
- framework no-arg — adapter/plugin;
- value fields — tylko primary constructor.

**Krótko:** pusty `data class` się nie kompiluje. Potrzebny jest co najmniej
jeden `val/var` parameter w primary constructor. Dla no-arg wywołania używa się
defaults, a dla stanu bez payload — `object` albo `data object`.

</details>

<details>
<summary>219. Jaka jest różnica między sealed class i enum?</summary>

#### Kotlin

`enum` i `sealed` ograniczają zestaw wariantów, ale modelują różne rzeczy.

```text
enum   -> stałe singleton entries jednego typu
sealed -> zamknięta hierarchy różnych subtype
```

### Enum

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

`enum` pasuje, gdy wszystkie cases mają taką samą strukturę i nie potrzebują
różnego payload. Każdy entry to singleton:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Enum może mieć wspólne properties:

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

Wszystkie entries mają ten sam property contract. Enum ma built-in API:
`entries`, `valueOf()`, `name`, `ordinal`.

`ordinal` nie należy zapisywać w DB/API: kolejność entries może się zmienić. Dla
persistence lepszy jest explicit stable code.

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

`sealed` pasuje, gdy variants mają różny payload albo behavior. `Loading` to
singleton, `Content` ma dane, `Error` ma inną model.

Typowe use cases:

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

Compiler zna wszystkie cases i może sprawdzić exhaustive `when`. Zbędne `else`
często ukrywa korzyść z tej weryfikacji.

### Sealed class vs sealed interface

`sealed class` może mieć constructor state i protected members:

```kotlin
sealed class AppError(
    open val code: Int
)
```

`sealed interface` nie ma constructor state, ale implementation może realizować
kilka interfaces i dziedziczyć po innej class. Dla state/result models często
wystarczy `sealed interface`.

### Serialization

Enum zwykle serializuje się jako stable textual/code value. Sealed hierarchy
wymaga discriminator dla subtype i schema dla payload. Trzeba myśleć o versioning
i unknown variants.

Praktyczny wybór:

- simple fixed mode/status — `enum`;
- cases bez różnego payload — `enum`;
- UI/result/error state z różnymi fields — `sealed`;
- object + data class cases w jednej model — `sealed`;
- nie zastępować enum sealed-typem „na przyszłość” bez potrzeby.

**Krótko:** `enum` to stała lista singleton entries ze wspólną strukturą.
`sealed class/interface` to zamknięta hierarchy, gdzie każdy case może mieć
własny payload. Dla prostych modes — `enum`, dla state/result models — `sealed`.

</details>

<details>
<summary>220. Czym jest WorkManager?</summary>

#### Kotlin

`WorkManager` — Jetpack API dla gwarantowanej odłożonej background work. Używa
się go do zadań, które mają wykonać się nawet po zamknięciu app albo kill
process, ale niekoniecznie natychmiast.

Pasuje do:

- background sync;
- upload/download z retry;
- cleanup starych danych;
- sending logs/analytics;
- periodic sync;
- zadań z constraints: network, charging, battery not low.

Jednorazowe zadanie:

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

Wyniki:

```text
Result.success() -> wykonano
Result.retry()   -> powtórzyć później
Result.failure() -> zakończyć błędem
```

Constraints:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()
```

WorkManager uruchomi pracę tylko wtedy, gdy constraints są spełnione.

Periodic work:

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodic work nie gwarantuje dokładnego czasu startu: Android wybiera moment z
uwzględnieniem battery i system policy.

Aby nie tworzyć duplikatów, używa się unique work:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` zostawia bieżące zadanie, `REPLACE` anuluje stare i ustawia nowe.

Input data powinno być małe:

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Duże objects lepiej przechowywać w DB i przekazywać tylko `id`.

Chaining:

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

WorkManager nie pasuje do:

- dokładnych alarmów;
- stałego socket connection;
- media playback;
- realtime tracking;
- pracy, która ma wystartować natychmiast;
- długiego user-visible foreground scenariusza bez przerw.

**Krótko:** `WorkManager` jest do deferrable, guaranteed background work z
constraints, retry, periodic/unique work i chaining. Dla realtime, exact alarms,
media playback albo natychmiastowej foreground pracy potrzebne są inne API.

</details>

<details>
<summary>221. Kiedy warto używać WorkManager?</summary>

#### Kotlin

`WorkManager` warto używać do persistent deferrable work: zadanie można odłożyć,
ale powinno zostać niezawodnie wykonane po zamknięciu UI, process death albo
reboot, z uwzględnieniem constraints.

Pasuje do:

- background sync;
- retry upload/download;
- cleanup starych danych;
- sending logs/analytics;
- periodic refresh;
- zadań z network/charging/battery constraints.

Exact time nie jest gwarantowany. Force stop i uninstall zatrzymują work.

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

Przykład `CoroutineWorker`:

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

Wyniki:

```text
success() -> zakończono
retry()   -> transient failure
failure() -> permanent failure
```

Worker powinien być idempotent: system może go zatrzymać i uruchomić ponownie.
`CancellationException` nie należy zamieniać na retry.

Constraints opisują warunki uruchomienia, ale nie dokładny czas:

- network;
- charging;
- battery not low;
- storage not low.

Unique work zapobiega duplicate jobs:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "user-sync",
        ExistingWorkPolicy.KEEP,
        request
    )
```

Periodic work wykonuje się orientacyjnie: Doze, battery policy i constraints
mogą przesunąć uruchomienie.

Input `Data` powinno być małe: primitives/String. Duże objects lepiej trzymać w
Room/file i przekazywać workerowi tylko `id`.

Kiedy nie pasuje:

```text
screen-bound work -> ViewModel/lifecycle scope
exact alarm       -> AlarmManager
media/tracking    -> Foreground Service
realtime socket   -> active runtime architecture
```

`ExpeditedWorkRequest` ma quotas i nie robi z WorkManager realtime API.

**Krótko:** WorkManager jest potrzebny dla idempotent background work, którą
można odłożyć, ale trzeba niezawodnie wykonać z constraints i retry. Dla
UI-bound, realtime, media, tracking albo exact-time zadań potrzebne są inne API.

</details>

<details>
<summary>222. Czy pracowałeś z Android Media3 / ExoPlayer?</summary>

#### Kotlin

Tak. `Media3` — to nowoczesny Jetpack media stack, a `ExoPlayer` — domyślna
implementation interfejsu `Player`. Wspiera local/streaming media, playlists,
adaptive streaming, subtitles i DRM.

Podstawowe użycie:

```kotlin
val player = ExoPlayer.Builder(context).build()

player.setMediaItem(MediaItem.fromUri(videoUrl))
player.prepare()
player.play()
```

W View UI:

```kotlin
playerView.player = player
```

Najważniejsze to poprawnie określić owner playera i gwarantowanie wywołać
`release()`.

```kotlin
override fun onDestroyView() {
    playerView.player = null
    player.release()
    super.onDestroyView()
}
```

Reference do `PlayerView` nie powinno przeżyć `onDestroyView()`.

W Compose legacy `PlayerView` można opakować przez `AndroidView`:

```kotlin
AndroidView(
    factory = { context ->
        PlayerView(context).apply {
            player = exoPlayer
        }
    }
)
```

Playera nie wolno tworzyć przy każdej recomposition. Potrzebny jest stabilny
owner i cleanup przez `DisposableEffect`, holder albo service.

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

`Player.Listener` daje playback states i errors:

- `BUFFERING`;
- `READY`;
- `ENDED`;
- playback error;
- media item transition.

UI mapuje te events na loading/error/replay state. Analytics lepiej nie mieszać z
rendering logic.

Dla background playback, music albo podcasts player nie powinien należeć do
`Activity/Fragment`. Poprawny schemat:

```text
MediaSessionService owns Player + MediaSession
UI connects via MediaController
```

To daje:

- media notification;
- system controls;
- Bluetooth/Wear OS/Android Auto integration;
- niezależność playback od screen lifecycle.

Dla streaming/cache używa się `CacheDataSource`, jawnie ustawiając cache size,
eviction strategy, offline behavior i DRM constraints.

Typowe błędy:

- nie wywołać `release()`;
- tworzyć player podczas recomposition;
- trzymać `PlayerView` po `onDestroyView()`;
- robić background playback w screen lifecycle;
- nie obsługiwać errors/audio focus;
- wywoływać player nie z jego application thread.

**Krótko:** Media3/ExoPlayer wymaga jasnego ownera, lifecycle cleanup,
`release()`, obsługi states/errors i poprawnego thread access. Dla background
playback używa się `MediaSessionService`, a nie `Activity` albo `Fragment`.

</details>

<details>
<summary>223. Jak rozdzieliłbyś komponenty po modułach, jeśli jest przycisk, który po kliknięciu ładuje dane?</summary>

#### Kotlin

Moduły trzeba budować wokół feature/use case, a nie wokół przycisku.

```text
Button -> ViewModel -> UseCase -> Repository -> API/DB
```

UI tylko wysyła event i renderuje state. Nie wie, skąd przyszły dane: network,
Room albo cache.

Dla małej app wystarczy taka struktura:

```text
:app
:core:network
:core:database
:feature:profile
```

W `:feature:profile` mogą być packages:

```text
presentation
domain
data
```

Osobne Gradle modules dla każdej warstwy nie są potrzebne, jeśli nie ma realnej
isolation/ownership.

Dla dużej feature można rozdzielić API i implementation:

```text
:feature:profile:api
:feature:profile:impl
```

`api` zawiera public contracts: navigation, capabilities, interfaces. `impl`
zawiera screen, ViewModel, use cases, repository implementation.

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

- przyjmuje click event;
- uruchamia coroutine;
- zarządza `loading/content/error`;
- obsługuje duplicate clicks;
- trzyma state w `StateFlow`.

Retrofit, SQL i cache logic nie powinny być w presentation.

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

Use case jest potrzebny, jeśli istnieje business rule, orchestration albo reuse.
Jeśli to prosty proxy w małej feature, ViewModel może zależeć bezpośrednio od
repository contract.

Domain nie zależy od Android UI, Retrofit, Room ani DI framework.

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

Data layer odpowiada za API, DB, cache/source of truth, mapping i infrastructure
error mapping.

Dependency direction:

```text
app -> feature impl
presentation -> domain contract
data -> domain contract
core -X-> feature
```

DI łączy implementation z contract w composition root:

```kotlin
@Binds
abstract fun bindRepository(
    impl: ProfileRepositoryImpl
): ProfileRepository
```

Kryteria dla osobnego Gradle module:

- independent ownership;
- stable public API;
- dependency isolation;
- reusable capability;
- build-time benefit.

Jeśli tych powodów nie ma, packages są prostsze. Nadmierna modularization dodaje
Gradle, DI i navigation boilerplate bez realnej korzyści.

**Krótko:** przycisk i state żyją w `presentation`, business operation — w
`domain/use case`, API/DB i repository implementation — w `data`. Dla małej app
wystarczą packages; Gradle modules dodaje się tylko dla isolation, ownership albo
build benefit.

</details>
