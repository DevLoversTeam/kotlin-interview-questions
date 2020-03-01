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
