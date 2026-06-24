**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Найпопулярніші запитання та відповіді на співбесіді з Kotlin</h2>

<details>
<summary>1. Що таке Kotlin і як він взаємодіє з Java?</summary>

#### Kotlin

Kotlin — статично типізована мова програмування від JetBrains. Вона підтримує
JVM, Android, JavaScript, WebAssembly і Native-платформи.

На JVM Kotlin компілюється у JVM-байткод, тому повністю сумісний із Java:

- Kotlin може безпосередньо викликати Java-код і використовувати Java-бібліотеки.
- Java може викликати Kotlin-код як звичайні JVM-класи та методи.
- Обидві мови можуть використовуватися в одному проєкті й модулі.
- Анотації `@JvmStatic`, `@JvmOverloads`, `@JvmField` і `@JvmName` допомагають
  створювати зручний API для Java.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

Важливий нюанс — platform types: якщо Java API не має коректних nullability-
анотацій, Kotlin не завжди може визначити, чи допускається `null`. Тому на межі
Java/Kotlin потрібні явні контракти.

**Коротко:** Kotlin сумісний із Java на рівні JVM і дозволяє поступово додавати
Kotlin у наявний Java-проєкт без повного переписування.

</details>

<details>
<summary>2. Які базові типи даних існують у Kotlin?</summary>

#### Kotlin

Основні типи Kotlin:

- цілі числа: `Byte`, `Short`, `Int`, `Long`;
- беззнакові цілі числа: `UByte`, `UShort`, `UInt`, `ULong`;
- числа з плаваючою крапкою: `Float`, `Double`;
- логічний тип: `Boolean`;
- символи та рядки: `Char`, `String`;
- масиви: `Array<T>`, `IntArray`, `LongArray` та інші.

Також важливі типи системи:

- `Any` — базовий тип для всіх non-null типів;
- `Unit` — функція не повертає значущого результату;
- `Nothing` — функція ніколи не завершується нормально;
- nullable-типи позначаються через `?`, наприклад `Int?`.

На JVM типи на кшталт `Int` можуть компілюватися у примітиви Java. Nullable-типи
та generics зазвичай потребують boxing.

- У Kotlin немає неявного розширення числових типів: `Int` не перетворюється в
  `Long` автоматично:

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**Коротко:** у Kotlin немає окремого синтаксису для примітивних і обʼєктних
типів, але JVM-компілятор використовує примітиви там, де це можливо.

</details>

<details>
<summary>3. У чому різниця між val і var у Kotlin?</summary>

#### Kotlin

`val` забороняє повторне присвоєння, а `var` дозволяє його:

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Помилка компіляції
```

`val` не робить сам обʼєкт незмінним — незмінним є лише посилання:

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Дозволено
```

Для властивостей класу `val` має лише getter, а `var` — getter і setter.

**Коротко:** за замовчуванням використовуй `val`, а `var` — лише коли значення
справді потрібно змінювати.

</details>

<details>
<summary>4. Що таке виведення типів (type inference) у Kotlin?</summary>

#### Kotlin

Виведення типів — це здатність компілятора визначити тип із виразу або контексту
без його явного зазначення. Kotlin залишається статично типізованою мовою.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it — String
```

Явний тип варто вказувати, коли він:

- є частиною публічного API;
- покращує читабельність складного виразу;
- має відрізнятися від типу конкретної реалізації.

```kotlin
val users: List<User> = mutableListOf()
```

**Коротко:** компілятор виводить очевидні типи автоматично; явні типи потрібні
для контрактів і читабельності.

</details>

<details>
<summary>5. Що таке функції-розширення (extension functions)?</summary>

#### Kotlin

Функція-розширення додає зручний синтаксис виклику для наявного типу без зміни
самого класу та без наслідування.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` — тип-отримувач. Усередині функції його обʼєкт доступний через `this`.

Важливі обмеження:

- розширення не має доступу до `private` і `protected` членів класу;
- метод класу має пріоритет над розширенням із такою самою сигнатурою;
- розширення вибирається статично за оголошеним типом змінної.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

На JVM таке розширення зазвичай компілюється у статичну функцію, якій обʼєкт
передається першим аргументом.

**Коротко:** extension-функції покращують читабельність API, але не додають
реальних методів класу й не підтримують поліморфне перевизначення.

</details>

<details>
<summary>6. Що таке data class у Kotlin?</summary>

#### Kotlin

`data class` призначений для моделей, основна роль яких — зберігати дані.
Компілятор генерує:

- `equals()` і `hashCode()`;
- `toString()`;
- `copy()`;
- `componentN()` для деструктуризації.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Важливі правила:

- первинний конструктор має містити хоча б один параметр `val` або `var`;
- `data class` не може бути `abstract`, `open`, `sealed` або `inner`;
- у згенерованих методах беруть участь лише властивості первинного конструктора;
- `copy()` виконує поверхневе, а не глибоке копіювання.

Властивість у тілі класу не враховується в `equals()`, `hashCode()` і `copy()`:

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**Коротко:** `data class` зменшує шаблонний код для DTO, UI state і value objects.
Складну бізнес-логіку краще тримати в доменних сервісах або звичайних класах.

</details>
<details>
<summary>7. Що таке companion object?</summary>

#### Kotlin

`companion object` — це єдиний обʼєкт, повʼязаний із класом. Його члени можна
викликати через імʼя класу:

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

Це не Java `static`: companion object має власний тип, може мати імʼя,
реалізовувати інтерфейси та отримувати доступ до `private` членів класу. У класі
може бути лише один companion object.

Для зручного виклику функції з Java використовують `@JvmStatic`:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Без анотації Java викликає `Config.Companion.default()`, з нею —
`Config.default()`.

**Коротко:** companion object підходить для фабричних методів і констант,
повʼязаних із класом. Глобальний змінюваний стан у ньому зберігати не варто.

</details>

<details>
<summary>8. Як створюються класи у Kotlin?</summary>

#### Kotlin

Клас оголошується ключовим словом `class`. Первинний конструктор записується в
заголовку класу:

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

- `val` і `var` у конструкторі створюють властивості класу.
- `init` виконується під час створення обʼєкта і підходить для перевірки
  інваріантів.
- Значення за замовчуванням дозволяють не створювати перевантажені конструктори.
- Обʼєкт створюється без ключового слова `new`.

Класи та їхні методи за замовчуванням `final`. Для наслідування потрібен `open`:

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**Коротко:** Kotlin дозволяє оголосити конструктор, властивості та значення за
замовчуванням прямо в заголовку класу; наслідування потрібно дозволяти явно.

</details>

<details>
<summary>9. Поясніть первинні та вторинні конструктори.</summary>

#### Kotlin

Первинний конструктор оголошується в заголовку класу:

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Його параметри стають властивостями лише з `val` або `var`. Тіло первинного
конструктора задається через `init`:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

Вторинний конструктор оголошується в тілі класу через `constructor`. Якщо є
первинний конструктор, вторинний має делегувати йому виклик через `this(...)`:

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Властивості та `init`-блоки виконуються в порядку оголошення, а тіло вторинного
конструктора — після них.

Якщо відмінність лише у значенні за замовчуванням, окремий конструктор не потрібен:

```kotlin
class User(val name: String, val id: Long = 0)
```

**Коротко:** первинний конструктор — основний варіант. Вторинні потрібні переважно
для Java-сумісності або вимог фреймворку; зазвичай достатньо параметрів за
замовчуванням чи фабричного методу.

</details>

<details>
<summary>10. Як працює наслідування у Kotlin?</summary>

#### Kotlin

Класи та їхні члени в Kotlin за замовчуванням `final`. Для наслідування потрібен
`open`, для перевизначення — `override`:

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

Перевизначати можна і властивості:

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Важливі нюанси:

- клас може успадкувати лише один клас, але реалізувати кілька інтерфейсів;
- `abstract` члени вже відкриті й не потребують `open`;
- перевизначений член залишається відкритим, якщо не вказати `final override`;
- до батьківської реалізації звертаються через `super`.

**Коротко:** наслідування в Kotlin потрібно дозволяти явно. Для контрактів краще
використовувати інтерфейси, а композицію зазвичай варто розглядати раніше за
створення глибокої ієрархії класів.

</details>

<details>
<summary>11. Як реалізувати singleton у Kotlin?</summary>

#### Kotlin

Стандартний спосіб реалізувати singleton у Kotlin — оголошення `object`:

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

На JVM обʼєкт створюється при першому зверненні, а його ініціалізація є
потокобезпечною. `object` може містити властивості, функції та `init`-блоки,
успадковувати клас і реалізовувати інтерфейси. Викликати його конструктор не можна.

`object` підходить для констант, stateless-стратегій і випадків sealed-ієрархії.
Для сервісів із залежностями краще використовувати singleton scope у DI-контейнері.

Змінюваний стан в `object` є глобальним, ускладнює тести й потребує синхронізації.
В Android не можна зберігати в ньому `Activity` або `View`, оскільки це може
спричинити витік памʼяті.

**Коротко:** `object` — простий singleton без параметрів конструктора. Для
бізнес-сервісів із залежностями та керованим життєвим циклом краще DI.

</details>

<details>
<summary>12. Для чого використовується тип Unit?</summary>

#### Kotlin

`Unit` означає, що функція завершується без значущого результату:

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

У звичайній функції `: Unit` можна не вказувати:

```kotlin
fun log(message: String) {
    println(message)
}
```

На відміну від Java `void`, `Unit` — повноцінний тип з єдиним значенням `Unit`.
Він використовується в типах функцій і generic API:

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

Не слід плутати з `Nothing`: `Unit` означає нормальне завершення без результату,
а `Nothing` — функція ніколи не завершується нормально:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**Коротко:** `Unit` — тип для функцій без значущого результату. Явно його зазвичай
пишуть у типах callback-функцій, наприклад `() -> Unit`.

</details>

<details>
<summary>13. Що таке smart cast?</summary>

#### Kotlin

Smart cast — автоматичне приведення до конкретнішого типу, коли компілятор може
довести його безпечність:

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value має тип String
    }
}
```

Механізм працює після перевірок `is`, `!is`, `null` і з урахуванням потоку
виконання:

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value має тип String
}
```

Smart cast не працює, якщо значення може змінитися між перевіркою та
використанням. Типовий випадок — змінювана властивість або custom getter:

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

**Коротко:** smart cast прибирає явні `as` після перевірки типу або `null`, але
лише коли компілятор гарантує, що перевірене значення не зміниться.

</details>

<details>
<summary>14. Як Kotlin забезпечує null safety?</summary>

#### Kotlin

У Kotlin nullable і non-null типи розділені на рівні системи типів:

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

Для роботи з nullable-значеннями використовують:

```kotlin
val nullableLength: Int? = optionalName?.length // safe call
val length: Int = optionalName?.length ?: 0     // Elvis operator
```

Після перевірки на `null` компілятор може виконати smart cast:

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

Оператор `!!` перетворює nullable-значення на non-null, але кидає
`NullPointerException`, якщо значення дорівнює `null`:

```kotlin
val length = optionalName!!.length
```

Основне слабке місце — Java platform types: без nullability-анотацій компілятор
не знає, чи може Java API повернути `null`. Такі значення потрібно перевіряти на
межі Java/Kotlin.

**Коротко:** `T` не допускає `null`, а `T?` допускає. Використовуй `?.`, `?:` і
перевірки; `!!` залишай лише для випадків із доведеною гарантією non-null.

</details>

<details>
<summary>15. Що таке Elvis-оператор (?:)?</summary>

#### Kotlin

Elvis-оператор `?:` повертає лівий операнд, якщо він не `null`, інакше обчислює
та повертає правий:

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

Його часто використовують разом із safe call:

```kotlin
val nameLength = user?.name?.length ?: 0
```

Оскільки `return` і `throw` у Kotlin є виразами, вони також можуть бути праворуч:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**Коротко:** `?:` задає fallback для `null`. Не маскуй ним помилки, якщо `null`
означає некоректний стан — у такому разі краще явно завершити виконання.

</details>

<details>
<summary>16. Що робить оператор !! і чому він небезпечний?</summary>

#### Kotlin

Оператор `!!` примусово перетворює `T?` на `T`. Якщо значення дорівнює `null`,
виникає `NullPointerException`:

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Безпечніші варіанти залежать від потрібної поведінки:

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

`requireNotNull` і `checkNotNull` теж завершують виконання з помилкою, але явно
описують порушений контракт. Часте використання `!!` зазвичай сигналізує про
неправильну модель nullable-стану або проблеми з життєвим циклом.

**Коротко:** використовуй `!!` лише коли non-null гарантований контрактом, але
компілятор не може цього довести. У решті випадків обробляй `null` явно.

</details>

<details>
<summary>17. Як працюють параметри за замовчуванням і іменовані параметри?</summary>

#### Kotlin

Параметр за замовчуванням використовується, якщо відповідний аргумент не передано:

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Іменовані аргументи покращують читабельність, особливо для кількох параметрів
одного типу або `Boolean`:

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java не підтримує Kotlin-параметри за замовчуванням та іменовані аргументи. Для
Java API можна згенерувати перевантаження через `@JvmOverloads`:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Назви параметрів публічної функції є частиною Kotlin API: їх зміна може зламати
виклики з іменованими аргументами. Якщо параметрів забагато, краще створити
окремий конфігураційний клас.

**Коротко:** значення за замовчуванням замінюють більшість перевантажень, а
іменовані аргументи роблять виклик зрозумілішим. Для Java враховуй `@JvmOverloads`.

</details>

<details>
<summary>18. Що таке destructuring declarations?</summary>

#### Kotlin

Destructuring declaration розкладає обʼєкт на змінні через функції
`componentN()`:

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

Еквівалентний код:

```kotlin
val id = user.component1()
val name = user.component2()
```

Для `data class` ці функції генеруються за порядком властивостей первинного
конструктора. Destructuring також працює в циклах і з `Map.Entry`:

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Непотрібний компонент можна пропустити через `_`:

```kotlin
val (_, name) = user
```

Для звичайного класу можна визначити власні `operator fun componentN()`. Однак
destructuring залежить від порядку компонентів, тому для великих моделей може
погіршувати читабельність.

**Коротко:** destructuring — зручний синтаксис над `componentN()` для невеликих
моделей і пар значень. Порядок компонентів є частиною контракту.

</details>

<details>
<summary>19. Що таке type alias і коли його використовувати?</summary>

#### Kotlin

`typealias` створює коротшу або зрозумілішу назву для наявного типу:

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Важливо: псевдонім не створює нового типу й не додає типової безпеки:

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Валідно, оскільки обидва типи — Long
```

Якщо компілятор має розрізняти значення, потрібен `value class`:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**Коротко:** `typealias` використовують для читабельності складних generic- і
function types. Для окремого доменного типу використовуй `value class`.

</details>

<details>
<summary>20. У чому різниця між vararg і масивом?</summary>

#### Kotlin

Масив — обʼєкт із фіксованою кількістю елементів. `vararg` — параметр функції,
який дозволяє передати змінну кількість аргументів:

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

Усередині функції `names` є масивом. Готовий масив передають через spread
operator `*`:

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

Якщо після `vararg` є інший параметр, його передають за назвою:

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

У функції може бути лише один `vararg`. Для `vararg Int` компілятор використовує
`IntArray`. Spread може створювати копію масиву, що важливо у критичному до
продуктивності коді.

**Коротко:** `Array<T>` — структура даних, `vararg` — спосіб прийняти довільну
кількість аргументів. Масив у `vararg` передається як `*array`.

</details>

<details>
<summary>21. Як використовуються лямбда-вирази у Kotlin?</summary>

#### Kotlin

Лямбда — анонімна функція, яку можна зберігати у змінній, передавати як аргумент
і повертати з функції:

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

Останній вираз є результатом лямбди. Якщо параметр один, його можна позначити
через `it`:

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

Якщо лямбда є останнім аргументом, її можна винести за круглі дужки:

```kotlin
button.setOnClickListener { println("Clicked") }
```

Лямбда може захоплювати змінні із зовнішньої області:

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

Якщо потрібна функція вже існує, можна використати посилання `::`:

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**Коротко:** лямбди передають поведінку в `map`, `filter`, callbacks і DSL. Довгі
або вкладені лямбди краще виносити в іменовані функції, а захоплення змінюваного
стану контролювати, особливо в конкурентному коді.

</details>

<details>
<summary>22. Як працюють колекції (List, Set, Map) у Kotlin?</summary>

#### Kotlin

Основні колекції Kotlin:

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- `List` зберігає порядок, допускає дублікати й доступ за індексом.
- `Set` зберігає унікальні елементи; унікальність визначається через
  `equals()` і `hashCode()`.
- `Map` зберігає пари ключ-значення з унікальними ключами. `map[key]` повертає
  nullable-значення, тому для перевірки наявності ключа є `containsKey()`.

Колекції мають read-only (`List`, `Set`, `Map`) і mutable-інтерфейси
(`MutableList`, `MutableSet`, `MutableMap`):

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Read-only не означає незмінну колекцію. Інше посилання може змінити той самий
обʼєкт:

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Операції `filter`, `map`, `sorted` та інші повертають нові колекції:

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Ланцюжки операцій над `Iterable` можуть створювати проміжні колекції. Для
великих ланцюжків варто оцінити `Sequence`, але не використовувати його без
потреби.

**Коротко:** назовні краще віддавати read-only інтерфейси, а mutable-колекції
залишати деталлю реалізації. За потреби справжньої незмінності роби копію або
використовуй immutable-колекції.

</details>

<details>
<summary>23. Як ітеруватися по колекціях у Kotlin?</summary>

#### Kotlin

Для звичайного обходу використовують `for`:

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Для короткої дії над кожним елементом підійде `forEach`:

```kotlin
names.forEach { println(it) }
```

Якщо потрібен індекс:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

`Map` зручно обходити через destructuring:

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Для трансформації або агрегації використовують відповідний оператор:

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Для `break`, `continue` і складного control flow краще `for`. Для пошуку варто
використовувати `firstOrNull`, `find`, `any` або `none`, а не ручний цикл.

**Коротко:** `for` — для керування циклом, `forEach` — для коротких побічних дій,
`map/filter` — для створення результату. Обирай оператор за наміром коду.

</details>

<details>
<summary>24. Що станеться, якщо змінити список, створений через listOf()?</summary>

#### Kotlin

`listOf()` повертає `List<T>` — read-only інтерфейс без методів `add`, `remove`
і `set`:

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Помилка компіляції
```

Однак `List` не гарантує повної незмінності. Інше mutable-посилання може змінити
той самий список:

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Приводити результат `listOf()` до `MutableList` небезпечно: код залежить від
конкретної реалізації та може впасти під час виконання.

Якщо потрібні зміни, створи mutable-список або копію:

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

Для незмінюваного стилю створи новий список:

```kotlin
val updated = names + "Oksana"
```

**Коротко:** `listOf()` не можна змінювати через `List` API. Для змін використовуй
`mutableListOf()` або `toMutableList()`, не небезпечний cast.

</details>

<details>
<summary>25. Що станеться при зверненні до індексу поза межами списку?</summary>

#### Kotlin

Доступ через `list[index]` викликає `get(index)`. Якщо індекс відʼємний або
більший чи дорівнює `size`, виникає `IndexOutOfBoundsException`:

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Безпечні варіанти:

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

Для потенційно порожнього списку є окремі безпечні функції:

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**Коротко:** використовуй `list[index]`, коли валідність індексу є інваріантом.
Для зовнішніх або необовʼязкових індексів використовуй `getOrNull`, `getOrElse`
чи перевірку через `indices`.

</details>

<details>
<summary>26. Як працює взаємодія Kotlin з Java (interop)?</summary>

#### Kotlin

Kotlin/JVM компілюється у JVM-байткод, тому Kotlin і Java можуть напряму
викликати класи та методи одне одного:

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

Основний ризик — platform types із Java, для яких nullability невідома. На межі
мов потрібно використовувати `@Nullable`/`@NotNull`, перевіряти значення й не
передавати platform types у внутрішні шари застосунку.

Для зручного Java-facing API використовують:

- `@JvmStatic` — статичний метод;
- `@JvmOverloads` — перевантаження для параметрів за замовчуванням;
- `@JvmField` — поле без getter/setter;
- `@JvmName` — інше JVM-імʼя;
- `@Throws` — оголошення checked exceptions для Java.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Java SAM-інтерфейси можна передавати як лямбди:

```kotlin
executor.execute { println("Done") }
```

**Коротко:** interop дозволяє поступову міграцію між Java і Kotlin. Найбільше
уваги потребують nullability, mutable-колекції, параметри за замовчуванням і
зручність API для Java-коду.

</details>

<details>
<summary>27. Чи можна використовувати Java-анотації у Kotlin?</summary>

#### Kotlin

Так. Java-анотації можна застосовувати до Kotlin-класів і їхніх членів:

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

Властивість Kotlin може бути представлена на JVM полем, getter, setter і
параметром конструктора. Щоб анотація потрапила в потрібне місце, використовують
use-site target:

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Основні варіанти:

- `@field:` — backing field;
- `@get:` / `@set:` — getter або setter;
- `@param:` — параметр конструктора;
- `@property:` — Kotlin-властивість, яка не є окремим Java-елементом.

Java-анотації nullability допомагають Kotlin визначити тип результату Java API:

```java
@NotNull
String getName();
```

Без коректної анотації результат може залишитися platform type. Для reflection-
або validation-фреймворків потрібно перевіряти, який JVM-елемент вони читають.

**Коротко:** Java-анотації підтримуються напряму. Головний нюанс — правильно
вибрати `@field:`, `@get:` або `@param:` для конкретного фреймворку.

</details>

<details>
<summary>28. Що таке Kotlin REPL?</summary>

#### Kotlin

REPL означає Read-Eval-Print Loop: середовище читає введений код, виконує його,
показує результат і чекає наступну команду.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

REPL підходить для швидкої перевірки синтаксису, стандартної бібліотеки та
невеликих виразів без створення проєкту. Запустити його можна через Kotlin CLI;
для довших експериментів зручніші scratch-файли або Kotlin Playground.

REPL не відтворює Android lifecycle, DI, конфігурацію Gradle чи реальне
конкурентне середовище й не замінює автоматизовані тести.

**Коротко:** Kotlin REPL — інтерактивна консоль для локальних експериментів із
мовою. Результати, важливі для застосунку, потрібно підтверджувати тестами у
реальному середовищі.

</details>

<details>
<summary>29. Що таке Kotlin script (.kts)?</summary>

#### Kotlin

Kotlin script — файл `.kts`, у якому можна виконувати інструкції верхнього рівня
без `fun main()`:

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

`.kt` є звичайним вихідним файлом програми або бібліотеки, а `.kts` виконується
скриптовим середовищем. Найпоширеніший приклад — Gradle Kotlin DSL:

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

Файли `build.gradle.kts` і `settings.gradle.kts` виконуються Gradle як
конфігураційні скрипти. Standalone-скрипти можна застосовувати для невеликої
автоматизації, але їхня модель залежностей і API залежать від scripting host.

**Коротко:** `.kts` підходить для конфігурації та короткої автоматизації. Логіку,
яка росте, має залежності й потребує тестів, краще винести у звичайний Kotlin-
модуль або CLI-застосунок.

</details>

<details>
<summary>30. Що таке Kotlin style guide і чому його варто дотримуватись?</summary>

#### Kotlin

Kotlin style guide визначає правила форматування, іменування, структури файлів
та організації імпортів. Він робить код команди однорідним і передбачуваним.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Практична користь:

- менше суперечок про форматування під час code review;
- швидше читання коду та адаптація нових розробників;
- автоматична перевірка через IDE formatter, `ktlint` або `detekt` у CI.

Командні відхилення від офіційних правил потрібно документувати й однаково
налаштовувати в IDE та CI. Style guide регулює оформлення коду, але не замінює
архітектурні правила.

**Коротко:** style guide зменшує когнітивне навантаження й переносить перевірку
форматування з людей на автоматичні інструменти.

</details>

<details>
<summary>31. Що таке Kotlin-ідіоми?</summary>

#### Kotlin

Kotlin-ідіоми — усталені способи використовувати можливості мови так, щоб код
чітко передавав намір.

Типові приклади:

- `val` за замовчуванням, `var` лише для необхідної мутації;
- nullable-типи, `?.` і `?:` замість `!!`;
- `data class` для моделей даних;
- `sealed`-типи для обмеженої множини станів;
- `map`, `filter`, `associateBy` замість ручних циклів для трансформацій;
- guard clauses для зменшення вкладеності.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Scope-функції, лямбди й operator overloading доречні лише тоді, коли покращують
читабельність. Використання більшої кількості можливостей мови саме по собі не
робить код ідіоматичним.

**Коротко:** ідіоматичний Kotlin — безпечний і зрозумілий код, який використовує
можливості мови за призначенням, а не заради демонстрації синтаксису.

</details>

<details>
<summary>32. Як Kotlin використовується в бекенд-розробці і чому його обирають замість Java?</summary>

#### Kotlin

Kotlin використовують для JVM-бекендів: REST/gRPC API, мікросервісів, workers і
event-driven систем. Він працює зі Spring Boot, Ktor та Java-бібліотеками.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Основні переваги порівняно з Java:

- null safety;
- менше шаблонного коду завдяки `data class`, параметрам за замовчуванням і
  extension-функціям;
- корутини для неблокуючого I/O;
- поступова міграція та повна доступність Java-екосистеми.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Компроміси: повільніша компіляція, додаткові compiler/Gradle-налаштування для
деяких Java-фреймворків і необхідність навчити команду ідіоматичного Kotlin та
structured concurrency.

**Коротко:** Kotlin варто обирати, коли команда хоче безпечніший і компактніший
JVM-код без відмови від Java-екосистеми та готова прийняти складніший toolchain.

</details>

<details>
<summary>33. Що таке функції вищого порядку (higher-order functions)?</summary>

#### Kotlin

Функція вищого порядку приймає функцію як аргумент або повертає її як результат.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` — функція без параметрів і без значущого результату.
Приклад повернення функції:

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

Типові приклади зі стандартної бібліотеки:

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Такі функції використовують у колекціях, callbacks і DSL. Лямбди можуть
створювати обʼєкти та захоплювати зовнішній стан; `inline` часто прибирає цей
overhead для невеликих функцій вищого порядку.

**Коротко:** функції вищого порядку дозволяють передавати поведінку як значення.
Складні або вкладені лямбди краще виносити в іменовані функції.

</details>

<details>
<summary>34. Що таке inline-функції і коли їх варто використовувати?</summary>

#### Kotlin

`inline` просить компілятор підставити тіло функції та її lambda-параметри в
місце виклику. Основна мета — зменшити overhead функцій вищого порядку:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` також дозволяє `reified` type parameters:

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Додаткові модифікатори lambda-параметрів:

- `noinline` — не вбудовує лямбду, тому її можна зберігати або передавати далі;
- `crossinline` — вбудовує, але забороняє non-local `return`.

`inline` доречний для невеликих функцій вищого порядку, `reified` generics і
виміряних hot paths. Для великих функцій він може збільшити bytecode. Зміни в
публічних inline-функціях також потребують уваги до бінарної сумісності.

**Коротко:** використовуй `inline` не як універсальну оптимізацію, а для
lambda-overhead, `reified` або потрібної семантики non-local return.

</details>

<details>
<summary>35. Що означає ключове слово reified і де воно застосовується?</summary>

#### Kotlin

Через type erasure звичайний generic-параметр `T` не можна використати для
перевірки типу або отримання `T::class`:

```kotlin
// fun <T> isOfType(value: Any) = value is T // Помилка компіляції
```

`reified` дозволяє це всередині `inline`-функції, бо компілятор підставляє
конкретний тип у місце виклику:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

Також можна отримати інформацію про клас:

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

Приклад зі стандартної бібліотеки:

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

Обмеження: `reified` доступний лише для type parameters inline-функцій і не
повертає інформацію про вкладені generic-аргументи на кшталт `List<String>`. Для
non-inline API потрібно явно передавати `KClass`, `Class` або serializer.

**Коротко:** `reified` дає inline-функції доступ до конкретного `T` для `is`,
`T::class`, reflection і подібних API, але не скасовує type erasure повністю.

</details>

<details>
<summary>36. Що таке sealed-класи та sealed-інтерфейси?</summary>

#### Kotlin

`sealed class` і `sealed interface` задають контрольовану ієрархію прямих
підтипів. Вони підходять для станів, результатів і команд:

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

Компілятор перевіряє, що `when` як вираз обробляє всі варіанти:

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Прямі підтипи мають бути оголошені в тому самому модулі й пакеті. `sealed class`
може мати конструктор, стан і спільну реалізацію. `sealed interface` не має
конструктора, зате клас може реалізувати кілька інтерфейсів.

На відміну від `enum`, кожен sealed-варіант може мати власний набір даних. Якщо
ієрархію мають розширювати зовнішні модулі, `sealed` не підходить. `else` у
`when` краще не додавати без потреби, щоб компілятор знаходив нові випадки.

**Коротко:** sealed-типи моделюють закритий набір варіантів із різними даними та
дають compile-time перевірку повноти `when`.

</details>

<details>
<summary>37. Що таке object expression і коли його використовують?</summary>

#### Kotlin

`object expression` створює анонімний обʼєкт у місці виконання. Зазвичай його
використовують для одноразової реалізації інтерфейсу або класу:

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

Також можна успадкувати клас:

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

Анонімний обʼєкт без supertype зручний лише локально або в `private` API:

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Його анонімний тип не можна експортувати як стабільний public type: назовні буде
видно оголошений supertype або `Any`. Object expression створюється щоразу під
час виконання виразу, тоді як object declaration є іменованим singleton.

Такий обʼєкт може захоплювати зовнішні змінні. Якщо реалізація росте або
повторюється, її краще винести в іменований клас.

**Коротко:** object expression підходить для короткої локальної реалізації,
listener або test stub. Для публічної чи повторно використовуваної логіки потрібен
іменований тип.

</details>

<details>
<summary>38. У чому різниця між enum-класами та sealed-класами?</summary>

#### Kotlin

`enum class` містить фіксований набір singleton-значень одного типу:

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

`sealed class` або `sealed interface` задає закриту ієрархію підтипів. Кожен
варіант може мати власні дані:

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

Для обох варіантів компілятор перевіряє повноту `when`:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

`enum` має стандартні `entries`, `name` та `ordinal`. Sealed-тип не має готового
списку екземплярів, зате дозволяє створювати багато обʼєктів одного підтипу з
різними даними.

**Коротко:** `enum` — для простих констант зі спільною структурою. Sealed-тип —
для закритого набору станів або результатів із різними даними.

</details>

<details>
<summary>39. У чому різниця між lateinit та lazy ініціалізацією?</summary>

#### Kotlin

Обидва механізми відкладають ініціалізацію, але працюють по-різному.

`lateinit` дозволяє присвоїти non-null `var` після створення обʼєкта:

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Читання до присвоєння кидає `UninitializedPropertyAccessException`. `lateinit`
працює лише з non-null `var` посилального типу, не гарантує thread safety і
дозволяє повторне присвоєння.

Можна перевірити ініціалізацію:

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` обчислює `val` при першому читанні й кешує успішний результат:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

За замовчуванням `lazy` синхронізований. Режими `PUBLICATION` і `NONE` змінюють
гарантії; `NONE` безпечний лише за доступу з одного потоку.

- Зовнішній код або DI присвоює значення пізніше — `lateinit`.
- Власний initializer має виконатися за потреби — `lazy`.
- Відсутність значення є валідним станом — nullable-тип.

**Коротко:** `lateinit` — відкладене зовнішнє присвоєння змінюваної властивості;
`lazy` — одноразове відкладене обчислення незмінної властивості.

</details>

<details>
<summary>40. Що таке тип Nothing і як він впливає на керування потоком виконання?</summary>

#### Kotlin

`Nothing` — тип без жодного можливого значення. Функція з таким результатом
ніколи не завершується нормально:

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` є підтипом усіх типів, тому його можна використовувати в будь-якому
виразі, де виконання переривається:

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` і `return` мають тип `Nothing`, тому можуть бути частиною Elvis-виразу:

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Тип допомагає компілятору аналізувати control flow і виконувати smart cast:

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` має єдине можливе значення `null`; це тип виразу `null`, якщо немає
іншого контексту. Стандартні `error()` і `TODO()` також повертають `Nothing`.

**Коротко:** `Nothing` позначає шлях, який не повертає керування. Завдяки цьому
компілятор точніше визначає типи та аналізує подальший потік виконання.

</details>

<details>
<summary>41. Чи може Java викликати Kotlin-функції з параметрами за замовчуванням?</summary>

#### Kotlin

Java не підтримує Kotlin-параметри за замовчуванням. Без додаткових overloads
вона має передати всі аргументи:

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

`@JvmOverloads` генерує Java-friendly перевантаження:

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

Перевантаження генеруються справа наліво для послідовних параметрів зі значеннями
за замовчуванням. Анотація працює і з конструкторами:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Для Kotlin-only API анотація не потрібна. Велика кількість параметрів створює
забагато overloads — у такому випадку краще config object або builder.

**Коротко:** без `@JvmOverloads` Java викликає лише повну сигнатуру. Додавай
анотацію тільки до API, які справді використовуються з Java.

</details>

<details>
<summary>42. Що таке tail-recursive функції?</summary>

#### Kotlin

Tail-recursive функція викликає саму себе останньою операцією. Модифікатор
`tailrec` дозволяє компілятору перетворити такий виклик на цикл без росту стека:

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

Цей варіант не є tail-recursive, бо після рекурсивного виклику виконується
множення:

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

Оптимізація працює лише для прямої рекурсії та не працює, якщо виклик розміщений
у конструкції, що заважає перетворенню, наприклад у `try`/`finally`. Якщо
`tailrec` застосований некоректно, компілятор попередить, що оптимізація не
виконується.

**Коротко:** `tailrec` потрібен для читабельної рекурсії без ризику
`StackOverflowError`. Якщо звичайний цикл простіший, краще використати цикл.

</details>

<details>
<summary>43. Що таке scope-функції (let, run, apply, also, with) і коли їх застосовувати?</summary>

#### Kotlin

Scope-функції виконують блок у контексті обʼєкта. Вони відрізняються двома
ознаками:

- як доступний обʼєкт: `this` або `it`;
- що повертається: результат лямбди або сам обʼєкт.

| Функція | Обʼєкт доступний як | Повертає         |
| ------- | ------------------- | ---------------- |
| `let`   | `it`                | результат лямбди |
| `run`   | `this`              | результат лямбди |
| `apply` | `this`              | сам обʼєкт       |
| `also`  | `it`                | сам обʼєкт       |
| `with`  | `this`              | результат лямбди |

Типове використання:

- `let` — трансформація або блок після safe call;
- `run` — обчислення результату з доступом через `this`;
- `apply` — конфігурація обʼєкта;
- `also` — побічна дія без зміни ланцюжка;
- `with` — групування операцій над переданим обʼєктом.

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

`with` — не extension-функція: обʼєкт передається першим аргументом. Глибоко
вкладені scope-функції та змішування `this`/`it` погіршують читабельність; у
такому разі краще звичайний код або іменована функція.

**Коротко:** вибирай scope-функцію за receiver (`this`/`it`) і результатом
(обʼєкт/результат лямбди), а не за звичкою.

</details>

<details>
<summary>44. У чому різниця між let, run та apply?</summary>

#### Kotlin

Функції відрізняються способом доступу до обʼєкта та результатом:

| Функція | Receiver у блоці | Повертає         |
| ------- | ---------------- | ---------------- |
| `let`   | `it`             | результат лямбди |
| `run`   | `this`           | результат лямбди |
| `apply` | `this`           | сам обʼєкт       |

`let` — для трансформації або виконання після safe call:

```kotlin
val length = name?.let { it.length }
```

`run` — для обчислення результату з кількома зверненнями до обʼєкта:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`apply` — для конфігурації з поверненням того самого обʼєкта:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

Не використовуй scope-функцію, якщо звичайний виклик читабельніший. Вкладені
блоки з кількома `this` або `it` краще замінити іменованою функцією.

**Коротко:** `let` — `it` і новий результат; `run` — `this` і новий результат;
`apply` — `this` і початковий обʼєкт.

</details>

<details>
<summary>45. У чому різниця між map і flatMap?</summary>

#### Kotlin

`map` перетворює кожен елемент на один результат:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

Якщо трансформація повертає колекцію, `map` створює вкладену структуру:

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` виконує трансформацію та обʼєднує вкладені колекції в одну:

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

Практичний приклад:

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Концептуально ці вирази еквівалентні:

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**Коротко:** `map`: `A -> B`. `flatMap`: `A -> Iterable<B>` із подальшим
обʼєднанням усіх результатів у плоску колекцію.

</details>

<details>
<summary>46. Що таке Sequence і коли його використовувати?</summary>

#### Kotlin

`Sequence` — лінива синхронна послідовність. Проміжні оператори не обробляють
дані одразу, а формують pipeline:

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Обчислення запускає terminal operation: `toList`, `first`, `count`, `fold`,
`any` тощо. Елементи проходять pipeline по одному, тому short-circuit операції
можуть зупинити роботу раніше:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

`Sequence` доречний для великих наборів, довгих ланцюжків `map`/`filter` та
short-circuit операцій. Для маленької колекції або одного перетворення його
overhead може бути більшим за користь. Операції на кшталт `sorted` все одно
потребують накопичити елементи.

`Sequence` не є асинхронним і не підтримує suspension. Для асинхронного потоку
даних використовують `Flow`.

**Коротко:** `Sequence` зменшує проміжні колекції та зайву роботу в довгих
синхронних pipeline, але не є автоматично швидшим за звичайні колекції.

</details>

<details>
<summary>47. Що таке лінива (lazy) обробка колекцій?</summary>

#### Kotlin

Лінива обробка означає, що pipeline лише описується, а виконується після
terminal operation. У Kotlin для цього використовують `Sequence`:

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Елементи зазвичай проходять оператори по одному. Це дозволяє завершити обробку
раніше:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Лінивий pipeline не кешує результат: повторна terminal operation зазвичай
запускає обчислення знову. Також можна працювати з потенційно нескінченними
послідовностями, якщо обмежити їх через `take`:

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Ліниві оператори можуть мати власний overhead, а `sorted` та подібні операції
все одно накопичують дані. Для асинхронної обробки потрібен `Flow`, не `Sequence`.

**Коротко:** lazy-обробка відкладає роботу до terminal operation і може уникнути
зайвих обчислень. Вона не кешує результат і не завжди швидша за eager-колекції.

</details>

<details>
<summary>48. Що таке корутини і чим вони відрізняються від потоків (threads)?</summary>

#### Kotlin

Корутина — легковагова задача, яка може призупинити виконання та продовжити його
пізніше. Для кожної корутини не створюється окремий OS thread:

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Потік — ресурс операційної системи зі власним стеком. Багато корутин можуть
виконуватися на невеликій кількості потоків і переходити між ними після
suspension.

```kotlin
delay(1_000)        // Призупиняє корутину
Thread.sleep(1_000) // Блокує потік
```

`suspend` сам по собі не гарантує неблокуючу реалізацію. Blocking API все одно
блокує потік і має виконуватися у відповідному контексті.

Dispatcher визначає потоки виконання:

- `Dispatchers.Main` — UI;
- `Dispatchers.IO` — blocking I/O;
- `Dispatchers.Default` — CPU-bound робота.

Structured concurrency привʼязує корутини до `CoroutineScope`: батьківська
операція керує життєвим циклом, помилками й скасуванням дочірніх задач.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Корутини не усувають race conditions і не роблять CPU-bound роботу швидшою
автоматично. Для важких обчислень потрібно явно вибрати правильний dispatcher:

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**Коротко:** корутини — задачі поверх потоків із suspension і structured
concurrency. Вони дешевші за модель «одна задача — один потік», але не замінюють
правильну роботу з dispatchers і shared state.

</details>

<details>
<summary>49. Що таке suspend-функція?</summary>

#### Kotlin

`suspend` позначає функцію, яка може мати точки призупинення. Вона викликається з
іншої suspend-функції або з корутини:

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` не створює корутину, не запускає функцію паралельно й не робить
blocking-код неблокуючим автоматично:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

Компілятор перетворює suspend-функцію на state machine з `Continuation`, щоб
зберегти стан і продовжити виконання після suspension.

Скасування кооперативне: стандартні suspension points перевіряють його, а довгі
CPU-цикли мають робити це явно:

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**Коротко:** `suspend` дозволяє функції призупиняти й відновлювати корутину. Він
не гарантує неблокуючу реалізацію та не створює нову корутину самостійно.

</details>
<details>
<summary>50. Що таке CoroutineScope і для чого він потрібен?</summary>

#### Kotlin

`CoroutineScope` є власником корутин і містить `coroutineContext`. Зазвичай у
контексті є `Job`, dispatcher та додаткові елементи:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

`Job` формує parent-child звʼязки та керує скасуванням. Скасування scope скасовує
його дочірні корутини:

```kotlin
scope.cancel()
```

Для паралельної роботи всередині suspend-функції використовують scope builder:

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` завершується лише після всіх дочірніх задач. В Android готові
`viewModelScope`, `lifecycleScope` і `rememberCoroutineScope()` вже мають
визначений lifecycle.

Власний scope створюють лише для обʼєкта з явним методом завершення, який
викликає `cancel()`. `GlobalScope` не має такого власника й порушує structured
concurrency, тому в application-коді його слід уникати.

**Коротко:** `CoroutineScope` привʼязує корутини до зрозумілого lifecycle та
забезпечує їх спільне скасування. Кожен scope повинен мати власника.

</details>

<details>
<summary>51. У чому різниця між launch і async?</summary>

#### Kotlin

Обидва builders запускають дочірню корутину в `CoroutineScope`.

`launch` повертає `Job` і використовується, коли окремий результат не потрібен:

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` повертає `Deferred<T>`. Результат або помилку отримують через `await()`.
Типовий сценарій — паралельне виконання незалежних операцій:

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

За замовчуванням `async` стартує одразу, а не під час `await()`. Помилка дочірньої
корутини скасовує звичайний parent scope незалежно від builder; для `async`
`await()` також повторно кидає цю помилку. Поведінку supervision потрібно
налаштовувати окремо.

Не використовуй `async`, якщо не плануєш викликати `await()`, і не запускай
паралельно операції, які залежать одна від одної.

**Коротко:** `launch` — задача без значення результату; `async` — задача з
`Deferred<T>`, результат якої потрібен через `await()`.

</details>

<details>
<summary>52. Що таке runBlocking і коли його використовують?</summary>

#### Kotlin

`runBlocking` створює `CoroutineScope` і блокує поточний потік, доки блок та всі
його дочірні корутини не завершаться:

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

Це boundary-інструмент для виклику suspend-коду із синхронного API. Він може
бути доречним у legacy bridge або короткому прикладі. У coroutine-тестах слід
використовувати `runTest`.

У UI-коді `runBlocking` блокує main thread і може спричинити зависання, ANR або
deadlock:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Замість цього корутину запускають у scope з відповідним lifecycle:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

У suspend-функції `runBlocking` не потрібен і лише блокує потік:

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**Коротко:** `runBlocking` — синхронний міст до suspend-коду. Не використовуй
його всередині корутин, suspend-функцій або UI-потоку.

</details>
<details>
<summary>53. Що таке диспетчери (Dispatchers.IO, Default, Main)?</summary>

#### Kotlin

`CoroutineDispatcher` планує виконання корутини на відповідних потоках:

- `Dispatchers.Main` — UI та короткі операції на головному потоці;
- `Dispatchers.IO` — blocking I/O;
- `Dispatchers.Default` — CPU-intensive обчислення.

Blocking-файлову операцію переносять на `IO`:

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Важке обчислення — на `Default`:

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` призупиняє поточну корутину, виконує блок у заданому контексті та
повертає результат. Він зберігає structured concurrency, cancellation і
поширення помилок.

Не кожен network або database виклик потребує `Dispatchers.IO`: suspend API може
вже бути main-safe. Dispatcher має вибирати шар, який знає, чи реалізація блокує
потік. Для тестованості dispatcher можна передати як залежність:

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

Перемикати контекст для дрібної роботи не варто: воно також має вартість.

**Коротко:** `Main` — UI, `IO` — blocking I/O, `Default` — важкі обчислення.
`withContext` змінює контекст для конкретної операції без створення нової
незалежної корутини.

</details>
<details>
<summary>54. Як скасовувати корутини і обробляти помилки?</summary>

#### Kotlin

Скасування корутин кооперативне й керується через `Job`:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Suspend-функції на кшталт `delay()` реагують на скасування автоматично. CPU-код
має регулярно перевіряти стан:

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

Скасування сигналізується через `CancellationException`. Якщо її перехопив
широкий `catch`, потрібно повторно кинути:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

За звичайної structured concurrency необроблена помилка дочірньої корутини
скасовує parent і сусідні задачі. Очікувані помилки обробляють локально та
перетворюють на доменний результат або UI state:

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

Для незалежних задач використовують `supervisorScope` або `SupervisorJob`:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

Supervisor не обробляє помилку, а лише ізолює siblings. `CoroutineExceptionHandler`
призначений для останнього рівня обробки uncaught exceptions у root `launch`,
наприклад логування. Помилки `async` отримують через `await()`.

**Коротко:** скасовуй через `Job`, не ковтай `CancellationException`, очікувані
помилки обробляй локально, а незалежні задачі ізолюй через supervision.

</details>
<details>
<summary>55. Що таке структурована конкурентність (structured concurrency)?</summary>

#### Kotlin

Structured concurrency означає, що кожна корутина має owner-а та належить до
ієрархії `Job`. Parent чекає дочірні задачі й керує їхнім скасуванням та
помилками.

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

`coroutineScope` не завершиться раніше дочірніх корутин. Скасування caller-а
скасує їх, а необроблена помилка однієї задачі скасує scope та siblings.

Для незалежних задач використовують supervision:

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

Помилка direct child не скасовує інші дочірні задачі, але її все одно потрібно
обробити. `supervisorScope` застосовують локально, `SupervisorJob` — у
довгоживучому scope.

`GlobalScope` порушує цю модель: caller не може нормально дочекатися роботи,
скасувати її чи отримати помилку. Власний scope повинен мати чіткий lifecycle та
явний cleanup.

**Коротко:** structured concurrency не дозволяє дочірній роботі непомітно
пережити свого owner-а. Повʼязані задачі використовують `coroutineScope`,
незалежні — supervision.

</details>
<details>
<summary>56. Чи можуть корутини виконуватись у будь-якому потоці?</summary>

#### Kotlin

Корутина не привʼязана до власного потоку. `CoroutineDispatcher` визначає, де
виконуються її частини:

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
- `Default` — CPU-bound робота.

На dispatcher із пулом корутина після suspension може продовжитися на іншому
потоці того самого dispatcher-а:

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` тимчасово змінює контекст, а після завершення відновлює контекст
caller-а. `Dispatchers.Main` гарантує виконання на UI-потоці; pool dispatchers не
гарантують конкретний thread ID.

Звичайний `ThreadLocal` не переходить між потоками разом із корутиною. За
потреби його значення додають до coroutine context:

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

Custom dispatcher із власними потоками повинен мати owner-а, який закриє його.
Blocking-виклик блокує потік незалежно від того, що він виконується в корутині.

**Коротко:** корутина виконується на потоках свого dispatcher-а й після
suspension може змінити фізичний потік. Привʼязку до конкретного потоку має
гарантувати dispatcher.

</details>
<details>
<summary>57. Що таке Flow у Kotlin?</summary>

#### Kotlin

`Flow<T>` — асинхронний потік значень на базі корутин. Suspend-функція повертає
одне значення, а `Flow` — послідовність значень у часі:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Flow створюють через builder і запускають terminal-оператором, наприклад
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

Flow, створений через `flow {}`, є cold: producer запускається окремо для кожного
collector-а. `emit()` є suspend-функцією, тому producer не випереджає повільний
collector без явного buffering.

Оператори формують pipeline:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` змінює контекст upstream-операторів:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` обробляє лише upstream-помилки; помилки collector-а ним не
перехоплюються. Скасування collector-а скасовує збір і producer у межах
structured concurrency.

**Коротко:** використовуй suspend-функцію для одного результату, а `Flow` — для
значень, що надходять у часі та потребують асинхронного pipeline.

</details>
<details>
<summary>58. У чому різниця між Flow, StateFlow та SharedFlow?</summary>

#### Kotlin

```text
Flow       -> зазвичай cold pipeline
StateFlow  -> hot-потік із поточним станом
SharedFlow -> hot broadcast із replay і buffer policy
```

`Flow`, створений через `flow {}`, запускає producer окремо для кожного
collector-а й не зберігає поточне значення:

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

`StateFlow` завжди має initial value. Новий collector одразу отримує поточний
стан, а рівні за `equals()` значення не emit-яться повторно:

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

Mutable-версію залишають `private`; для конкурентного оновлення використовують
`update { }`.

`SharedFlow` надсилає значення всім активним collectors і не зобовʼязаний мати
поточне значення:

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` визначає, скільки останніх значень отримає новий collector;
`extraBufferCapacity` і `onBufferOverflow` керують буфером. За `replay = 0`
значення без subscribers втрачається, тому критичні дані потрібно моделювати як
state, а не одноразову подію.

Cold Flow можна перетворити на shared hot flow: `stateIn()` створює `StateFlow`,
а `shareIn()` — `SharedFlow`. `SharingStarted` визначає, коли спільний upstream
запускається та зупиняється.

**Коротко:** `Flow` — незалежний lazy pipeline, `StateFlow` — поточний стан,
`SharedFlow` — спільний потік подій із налаштовуваним replay.

</details>
<details>
<summary>59. У чому різниця між Flow і LiveData?</summary>

#### Kotlin

`LiveData` — Android lifecycle-aware holder, а `Flow` — coroutine stream без
залежності від Android.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` повідомляє лише активних observers і повторно віддає останнє значення
після активації. Він зручний для legacy View UI, але не повинен потрапляти в
domain layer.

`Flow` підтримує coroutine cancellation, помилки та широкий набір операторів:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

Звичайний `Flow` переважно cold і не зберігає current value. Для UI state
використовують `StateFlow`. Lifecycle під час збору Flow потрібно врахувати явно:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

У Compose для цього є `collectAsStateWithLifecycle()`. LiveData орієнтований на
Main thread, а Flow використовує coroutine context і `flowOn` для upstream.

Під час міграції типи можна конвертувати:

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**Коротко:** `LiveData` автоматично враховує Android lifecycle. `Flow` не залежить
від Android і краще підходить для data/domain; у UI його збирають lifecycle-aware,
а поточний стан зберігають у `StateFlow`.

</details>
<details>
<summary>60. У чому різниця між холодними та гарячими потоками?</summary>

#### Kotlin

Різниця — у звʼязку producer-а з collectors:

```text
cold -> окремий producer запускається для кожного collector-а
hot  -> collectors отримують дані зі спільного джерела
```

Flow, створений через `flow {}`, є cold. До `collect` він не виконується, а кожен
collector запускає блок окремо:

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

Отже, операція виконається двічі. Кожен collector має власний lifecycle і
cancellation.

`StateFlow` і `SharedFlow` є hot: їхній обʼєкт і дані існують незалежно від
конкретного collector-а.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` зберігає поточний стан. `SharedFlow` розсилає значення активним
collectors і може зберігати останні значення через `replay`.

Cold Flow перетворюють на shared hot flow через `stateIn()` або `shareIn()`.
Політика `SharingStarted` визначає, коли спільний upstream активний, але всі
collectors усе одно використовують одне виконання.

**Коротко:** cold stream запускає producer для кожного collector-а; hot stream
має спільне джерело й розсилає його значення всім collectors.

</details>
<details>
<summary>61. Що таке collectLatest і коли його використовувати?</summary>

#### Kotlin

`collectLatest` — terminal operator, який скасовує обробку попереднього значення,
коли надходить нове:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Встигне надрукуватися лише 3
}
```

Типовий сценарій — пошук, де попередній запит стає неактуальним:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Скасування кооперативне. Suspend API реагує на нього, а blocking-код — ні:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

Не використовуй `collectLatest`, якщо кожне значення має бути гарантовано
оброблене: платежі, повідомлення, збереження або analytics events.

`flatMapLatest` скасовує попередній inner Flow, а `collectLatest` — блок
collector-а.

**Коротко:** `collect` — коли важливе кожне значення; `collectLatest` — коли після
нового значення попередня обробка більше не потрібна.

</details>
<details>
<summary>62. Як перетворити callback-based API у suspend-функцію?</summary>

#### Kotlin

Одноразовий callback обгортають у `suspendCancellableCoroutine`:

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

- `resume()` повертає результат;
- `resumeWithException()` повертає помилку;
- `invokeOnCancellation` скасовує зовнішню операцію або видаляє listener.

Скасування корутини не зупиняє callback API автоматично. Continuation можна
завершити лише один раз; якщо callback може спрацювати повторно або конкурентно,
потрібні `tryResume()`/`completeResume()` чи атомарний захист.

Для callback із багатьма значеннями використовують `callbackFlow`:

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

`awaitClose` видаляє listener під час скасування або закриття Flow. Результат
`trySend()` і buffer policy потрібно обробляти відповідно до гарантій доставки.

**Коротко:** один результат — `suspendCancellableCoroutine`; потік значень —
`callbackFlow`. В обох випадках обовʼязково реалізуй cleanup при cancellation.

</details>
<details>
<summary>63. У чому різниця між Channel і Flow?</summary>

#### Kotlin

`Channel<T>` — асинхронна черга для комунікації між корутинами:

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

Producer викликає `send()`, consumer — `receive()` або читає через `for`. Кожне
значення отримує один consumer, тому Channel підходить для worker queue і
fan-out. Потрібно явно керувати capacity, закриттям і cancellation.

`Flow<T>` — декларативний API асинхронного потоку:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow підтримує `map`, `filter`, `combine`, cancellation і structured
concurrency. Flow, створений через `flow {}`, зазвичай cold; Channel є hot і може
приймати значення без активного consumer-а залежно від capacity.

Backpressure Channel визначається буфером:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Чекає receiver-а
```

- `RENDEZVOUS` — без буфера;
- `BUFFERED` — обмежений буфер;
- `CONFLATED` — зберігається останнє значення;
- `UNLIMITED` — необмежений буфер із ризиком росту памʼяті.

У Flow використовують `buffer()`, `conflate()` або `collectLatest()`.

Channel можна представити як Flow:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` не робить broadcast: collectors розподіляють значення між
собою. Для стану використовують `StateFlow`, для broadcast — `SharedFlow`, а для
callback bridge — `callbackFlow`.

**Коротко:** `Channel` — producer-consumer черга; `Flow` — декларативний потік
даних. Для application streams починай із Flow, Channel залишай для явної
передачі роботи між корутинами.

</details>
<details>
<summary>64. Які основні оператори Flow (map, filter, combine, zip)?</summary>

#### Kotlin

`map` перетворює кожне значення:

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` пропускає лише значення, які відповідають умові:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` чекає перше значення від кожного Flow, а потім emit-ить новий результат
після оновлення будь-якого з них:

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` обʼєднує значення попарно за порядком:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

Результат: `1A`, `2B`, `3C`. `zip` чекає відповідну пару, тоді як `combine`
використовує останні доступні значення. Тому для стану з кількох джерел зазвичай
потрібен `combine`, а для пар елементів — `zip`.

**Коротко:** `map` трансформує, `filter` відсіює, `combine` реагує на останні
значення всіх потоків, `zip` обʼєднує значення попарно.

</details>
<details>
<summary>65. У чому різниця між combine і zip?</summary>

#### Kotlin

Обидва оператори обʼєднують Flow, але мають різну семантику:

```text
combine -> нове значення + останні значення інших Flow
zip     -> перше з першим, друге з другим
```

`combine` чекає перше значення від кожного upstream, а потім реагує на кожне
оновлення:

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` формує пари за порядком:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

Швидший upstream у `zip` чекає пару. Результат завершується, коли повних пар
більше не буде. У `combine` завершений upstream може використовуватися своїм
останнім значенням, доки інші продовжують emit.

**Коротко:** `combine` — для стану, що оновлюється від будь-якого джерела;
`zip` — для попарного зіставлення послідовностей.

</details>
<details>
<summary>66. Для чого потрібні @JvmStatic, @JvmOverloads, @JvmField?</summary>

#### Kotlin

Ці анотації змінюють JVM API для Java-коду.

- `@JvmStatic` генерує статичний bridge для члена `object` або
  `companion object`.
- `@JvmOverloads` генерує overloads для послідовних параметрів зі значеннями
  за замовчуванням, починаючи справа.
- `@JvmField` відкриває backing field напряму, без getter/setter.

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

Java зможе викликати `UserFactory.create()`, читати
`UserFactory.defaultName` і використовувати overload без `active`.

`@JvmField` не працює з delegated property або custom accessors і послаблює
інкапсуляцію. Для compile-time констант примітивного або `String` типу краще
`const val`.

**Коротко:** ці анотації потрібні лише для зручного Java/framework API. У
Kotlin-only коді додавати їх без потреби не варто.

</details>
<details>
<summary>67. Що таке делегування (delegation)?</summary>

#### Kotlin

Делегування передає реалізацію іншому обʼєкту замість наслідування. Kotlin
підтримує делегування інтерфейсу через `by`:

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

Компілятор генерує forwarding methods. Окремий метод можна перевизначити:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Властивості також можуть мати delegate:

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

Custom property delegate реалізує `getValue()`, а для `var` — також
`setValue()`.

**Коротко:** class delegation прибирає forwarding boilerplate і підтримує
композицію; property delegation виносить логіку читання та запису властивості в
окремий обʼєкт.

</details>
<details>
<summary>68. Які фреймворки для тестування доступні в Kotlin?</summary>

#### Kotlin

Вибір залежить від рівня тесту:

- `kotlin.test` — multiplatform annotations та assertions;
- JUnit — JVM unit tests;
- Kotest — альтернативний DSL і matchers;
- MockK або Mockito — test doubles;
- `kotlinx-coroutines-test` — корутини, test dispatchers і virtual time;
- Turbine — перевірка emissions Flow;
- AndroidX Test, Espresso та Compose UI Test — Android/UI;
- Robolectric — частина Android-сценаріїв на JVM.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

Для domain logic часто достатньо JUnit або `kotlin.test` і ручних fakes.
Інструмент не замінює правильний рівень тесту: device-specific поведінку
перевіряють instrumentation tests.

**Коротко:** unit — JUnit/`kotlin.test`, корутини — coroutines-test, Flow —
Turbine, Android UI — Espresso або Compose UI Test.

</details>
<details>
<summary>69. Як мокати залежності у тестах?</summary>

#### Kotlin

Залежність передають через конструктор і замінюють mock, stub або fake.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

MockK для suspend-функції:

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

Ручний fake:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

Fake краще підходить для перевірки поведінки, mock — коли interaction є частиною
контракту. Не варто мокати value objects, pure functions і кожен внутрішній
виклик: такі тести привʼязуються до реалізації.

**Коротко:** використовуй constructor injection; віддавай перевагу простому fake,
а mock застосовуй для важливих interactions із зовнішньою залежністю.

</details>
<details>
<summary>70. Як тестувати корутини та Flow?</summary>

#### Kotlin

Для корутин використовують `kotlinx-coroutines-test` і `runTest`:

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` надає virtual time і `TestCoroutineScheduler`:

- `runCurrent()` виконує задачі в поточний момент;
- `advanceTimeBy()` пересуває virtual time;
- `advanceUntilIdle()` виконує чергу до простою.

Dispatchers краще передавати як залежності. `Dispatchers.Main` у local unit
tests підміняють test dispatcher-ом і повертають через `resetMain()`.

Послідовність Flow emissions зручно перевіряти через Turbine:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Для фінального стану `StateFlow` часто достатньо виконати queued tasks і
перевірити `state.value`. Не використовуй `Thread.sleep()`, real delays або
різні test schedulers в одному тесті.

**Коротко:** `runTest` і test dispatcher — для корутин; Turbine — для порядку
Flow emissions; `StateFlow.value` — для фінального стану.

</details>
<details>
<summary>71. Які best practices для написання тестованого коду?</summary>

#### Kotlin

Тестований код має явні залежності та передбачувану поведінку.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

Основні правила:

- constructor injection замість створення залежностей усередині класу;
- одна чітка відповідальність на клас;
- pure functions для бізнес-обчислень;
- абстракції для часу, I/O, API, БД та dispatchers;
- мінімум Android framework у domain logic;
- fakes замість надмірного мокання;
- перевірка public behavior, а не private methods та внутрішніх викликів.

ViewModel тестують через observable state/effects:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

Не потрібно створювати interface для кожного класу. Абстракція виправдана на
межі зі змінною або зовнішньою залежністю.

**Коротко:** явні залежності, контрольовані side effects і маленькі
відповідальності роблять код тестованим без привʼязки тестів до реалізації.

</details>
<details>
<summary>72. Як ефективно керувати залежностями у Kotlin-проєкті?</summary>

#### Kotlin

У Gradle-проєкті потрібно контролювати версії, scopes і напрямок залежностей між
модулями.

- Version catalog централізує aliases і версії в `libs.versions.toml`.
- `implementation` приховує dependency від downstream modules.
- `api` відкриває її як частину public API й використовується лише за потреби.
- `testImplementation` та `androidTestImplementation` ізолюють тестові
  залежності.
- BOM узгоджує версії артефактів однієї екосистеми.
- Convention plugins прибирають дублювання Gradle-конфігурації.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

Модуль має залежати від потрібного contract, а не від усієї implementation.
Gradle dependencies і runtime DI — різні рівні: Gradle визначає доступність коду,
DI створює обʼєкти під час виконання.

Для діагностики:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Оновлення роблять малими кроками з перевіркою changelog, build і tests.

**Коротко:** централізуй версії, віддавай перевагу `implementation`, контролюй
module graph і регулярно перевіряй dependency graph.

</details>
<details>
<summary>73. Як Kotlin працює з памʼяттю та garbage collection?</summary>

#### Kotlin

Модель памʼяті залежить від платформи. На JVM та Android Kotlin використовує
керування памʼяттю JVM/ART: обʼєкти живуть у heap, а GC звільняє ті, до яких
більше немає досяжних посилань.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

Після завершення функції обʼєкт може бути зібраний, якщо його ніщо не утримує.
Присвоєння `null` лише прибирає посилання — момент очищення визначає GC.

Memory leak виникає, коли непотрібний обʼєкт залишається reachable:

```kotlin
object Holder {
    var activity: Activity? = null // Може утримувати Activity
}
```

На Android типові причини — Activity/View у singleton, неочищені listeners або
binding, а також корутина з довшим lifecycle. Часті allocations, boxing,
closures і проміжні колекції можуть збільшувати GC pressure, але оптимізувати їх
потрібно лише після профілювання.

Kotlin/Native і Kotlin/JS використовують механізми памʼяті своєї платформи, тому
JVM-деталі не можна переносити на всі targets.

**Коротко:** на JVM/Android памʼять звільняє GC після втрати всіх досяжних
посилань. Головний ризик — не `null`, а довгоживуче посилання на непотрібний
обʼєкт.

</details>
<details>
<summary>74. Які best practices роботи з null safety?</summary>

#### Kotlin

Nullable-тип має означати реальну можливість відсутності значення, а не
використовуватися «про всяк випадок».

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

Основні правила:

- уникай `!!`; використовуй `?.`, `?:` або early return;
- `requireNotNull` — для вхідного контракту, `checkNotNull` — для внутрішнього
  стану;
- перевіряй Java platform types на межі Java/Kotlin;
- перетворюй nullable DTO на строгі domain models у mapper;
- якщо `null` означає окремий стан, моделюй його sealed-типом.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

Fallback не повинен приховувати помилку: якщо відсутність значення порушує
контракт, краще завершити виконання явно.

**Коротко:** моделюй відсутність значення типом `T?`, обробляй її на межі та не
перетворюй nullable-код на ланцюжок `!!`.

</details>
<details>
<summary>75. Що таке supervisorScope і чим він відрізняється?</summary>

#### Kotlin

У `coroutineScope` необроблена помилка дочірньої корутини скасовує scope та
siblings. У `supervisorScope` direct children можуть падати незалежно:

```text
coroutineScope  -> failure child скасовує siblings
supervisorScope -> failure child не скасовує siblings
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

Supervisor не обробляє помилки автоматично. Exception з `async` буде кинутий
через `await()`, якщо його не перетворити на результат.

`supervisorScope` використовують локально в suspend-функції, а
`SupervisorJob` — для довгоживучого scope.

**Коротко:** повʼязані all-or-nothing задачі — `coroutineScope`; незалежні
задачі з допустимим частковим результатом — `supervisorScope`.

</details>
<details>
<summary>76. Що таке backpressure і як його обробляти у Flow?</summary>

#### Kotlin

Backpressure виникає, коли producer потенційно швидший за consumer. У звичайному
Flow без buffer `emit()` призупиняється, доки downstream не завершить обробку,
тому producer природно сповільнюється.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

Оператори змінюють цю поведінку:

- `buffer(n)` дозволяє producer випередити consumer на `n` значень;
- `conflate()` пропускає проміжні значення, зберігаючи актуальне;
- `collectLatest` скасовує попередню обробку;
- `debounce` чекає паузу між вхідними подіями;
- `sample` бере останнє значення з інтервалом;
- `flatMapLatest` скасовує попередній inner Flow.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

Якщо важливе кожне значення, його не можна conflated або скасовувати: потрібно
оптимізувати consumer, використати контрольований buffer чи надійну чергу.
Необмежений buffer може спричинити ріст памʼяті.

**Коротко:** Flow уже створює backpressure через suspend `emit()`. Buffering,
conflation або latest-оператори додають лише тоді, коли відома потрібна політика
втрати чи очікування даних.

</details>
<details>
<summary>77. Як використовувати Java Streams у Kotlin?</summary>

#### Kotlin

Kotlin/JVM може напряму використовувати Java Stream API:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Для Kotlin-колекцій стандартні оператори зазвичай коротші:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Для лінивої синхронної обробки є `Sequence`:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Stream доречний, коли його повертає Java API, потрібен Java `Collector` або
pipeline уже написаний на Java. Stream є одноразовим.

I/O Stream потрібно закривати:

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Java Collectors часто мають прямий Kotlin-аналог:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` використовує common `ForkJoinPool`, тому має неявний
parallelism і складніше скасовується. У coroutine-коді паралельність краще
контролювати явно; для I/O parallel Stream не замінює suspend API.

```text
Stream/Sequence -> synchronous one-shot pipeline
Flow            -> asynchronous, suspending, cancellable stream
```

**Коротко:** Java Stream використовуй для Java interop; Kotlin collection
operators — для eager-обробки; `Sequence` — для lazy synchronous pipeline;
`Flow` — для асинхронних даних.

</details>
<details>
<summary>78. Що таке Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` — стара назва mobile-напряму `Kotlin Multiplatform` (`KMP`). Ідея: винести спільний Kotlin-код у shared module і використовувати його на Android та iOS.

Зазвичай шарять:

- domain models;
- use cases;
- validation;
- repository contracts;
- networking/data layer;
- business rules.

Типова структура:

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` містить платформонезалежний код:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Platform-specific API ховають за interface або `expect/actual`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

UI можна залишити нативним: Android Compose/View та SwiftUI/UIKit. Якщо команда готова, частину UI можна шарити через Compose Multiplatform.

**Коротко:** KMP дає спільну бізнес-логіку для Android/iOS, але platform API, UX-деталі та інтеграції треба ізолювати. Це не “один код для всього”, а контрольований sharing там, де він реально зменшує дублювання.

</details>

<details>
<summary>79. Які обмеження має Kotlin Multiplatform?</summary>

#### Kotlin

`Kotlin Multiplatform` корисний для shared logic, але має важливі обмеження.

1. **Platform API недоступні в `commonMain`**

У спільному коді не можна напряму використовувати `Context`, UIKit, Android-only або iOS-only SDK. Потрібні interfaces або `expect/actual`.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **Не всі бібліотеки multiplatform**

Java/Android-only бібліотеку не можна просто додати в `commonMain`. Треба шукати KMP-аналог або робити platform implementation.

3. **iOS interop має нюанси**

Shared Kotlin код експортується в iOS як framework. Треба враховувати Swift API, generics, exceptions, coroutines/Flow bridging і debugging.

4. **UI sharing не завжди вигідний**

Часто шарять domain/data layer, а UI залишають нативним. Compose Multiplatform можливий, але це окреме архітектурне рішення.

5. **Складніший build і ownership**

KMP додає source sets, Gradle setup, CI-складність і потребує домовленостей між Android/iOS командами щодо shared module.

**Коротко:** KMP добре підходить для стабільної спільної бізнес-логіки, але не прибирає platform-specific код. Основні ризики: interop, бібліотеки, build complexity, ownership і надмірне винесення всього в shared.

</details>

<details>
<summary>80. Як ділитися бізнес-логікою у KMM?</summary>

#### Kotlin

Бізнес-логіку виносять у shared module, а Android/iOS деталі залишають у platform source sets.

Типова структура:

```text
shared/
  commonMain   -> domain/use cases/contracts
  androidMain  -> Android implementation
  iosMain      -> iOS implementation
```

У `commonMain` добре тримати:

- domain models;
- use cases;
- validation;
- repository interfaces;
- API clients;
- serialization;
- business rules.

Приклад:

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

Platform-specific API підставляють через DI, interfaces або `expect/actual`:

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

Networking часто роблять через Ktor Client, JSON — через `kotlinx.serialization`, state — через coroutines/Flow. Для iOS одразу треба продумати bridge до Swift async/await або Combine.

Не варто шарити все підряд: permissions, camera, notifications, platform storage і UX-specific логіку краще залишати на платформі або ховати за контрактом.

**Коротко:** у shared module виносять стабільну бізнес-логіку та contracts. Platform-specific частини реалізують окремо. Хороший KMM — це чітка межа, а не shared-моноліт.

</details>

<details>
<summary>81. Як оптимізувати продуктивність Kotlin-коду?</summary>

#### Kotlin

Оптимізація починається не з синтаксису, а з вимірювання:

```text
measure -> find bottleneck -> fix -> measure again
```

На Android використовують Profiler, Perfetto/System Trace, Macrobenchmark і allocation tracking.

Основні напрямки:

1. **Алгоритми та I/O**

Найбільший виграш дають правильні data structures, менше network/DB calls, pagination, cache і DB indexes. `O(n²)` не виправляється красивим Kotlin-синтаксисом.

2. **Allocations у hot path**

У критичних місцях зайві `map/filter/copy` можуть створювати багато обʼєктів:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

Але в звичайному UI readability важливіша за ручну мікрооптимізацію.

3. **Collections і Sequence**

`Sequence` корисна для lazy chains або early termination, але має overhead. На малих списках вона не обовʼязково швидша.

4. **Coroutines**

- blocking I/O — `Dispatchers.IO`;
- CPU work — `Dispatchers.Default`;
- не використовувати `runBlocking` на Main;
- скасовувати obsolete work;
- не робити зайві context switches.

5. **UI performance**

У Compose не робити важку роботу в composition, використовувати stable keys, читати state ближче до місця використання. У Views не перевантажувати `onBindViewHolder`, `onDraw` і не створювати зайві layout passes.

6. **Memory**

Не тримати `Activity/View` у singleton, контролювати cache size, lifecycle resources і large bitmaps. Leaks збільшують GC pressure і ризик OOM.

**Коротко:** спочатку профілюємо. Потім виправляємо алгоритми, I/O, allocations, concurrency, UI hot paths і memory leaks. `inline`, `Sequence` та primitive arrays застосовують тільки там, де вимірювання показало користь.

</details>

<details>
<summary>82. Як реалізувати retry з exponential backoff у Kotlin?</summary>

#### Kotlin

Exponential backoff повторює transient error зі зростаючою паузою. Retry має мати `maxAttempts`, `maxDelay`, jitter, timeout і не ламати cancellation.

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

Приклад predicate:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Для `Flow` можна використати `retryWhen`:

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

Не retry-ити validation errors, `403/404`, auth failure без refresh flow і non-idempotent operations без idempotency key. Для `429/503` бажано враховувати `Retry-After`.

**Коротко:** retry має бути обмежений, cancellable, з jitter і чітким правилом, які помилки можна повторювати. Інакше він маскує проблеми та створює додаткове навантаження.

</details>

<details>
<summary>83. Які нові можливості Kotlin варто знати у 2026 році?</summary>

#### Kotlin

У 2026 році на співбесіді важливо орієнтуватися не в кожній дрібній фічі, а в напрямку Kotlin 2.x і tooling ecosystem.

1. **K2 compiler**

K2 — нова compiler architecture Kotlin 2.x. Вона важлива для швидшого analysis, кращої бази для IDE, compiler plugins і Multiplatform. Перед upgrade перевіряють сумісність Kotlin Gradle Plugin, AGP, Gradle, Compose compiler, KSP/kapt і serialization.

2. **Context parameters / receivers**

Корисні для DSL і scoped APIs:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

Це не заміна DI, а спосіб явно передати контекст виконання.

3. **Explicit backing fields**

Дозволяють менше boilerplate для read-only public API та mutable internal state:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Multiplatform розвиток**

KMP стає практичнішим: Swift export, coroutines/Flow bridging, Wasm і Compose Multiplatform розвиваються. Але production-рішення все одно треба оцінювати за interop, tooling, debugging і team ownership.

5. **Standard library та JVM tooling**

Варто слідкувати за новими stdlib API, `kotlin.uuid.Uuid`, JVM target/toolchain compatibility, Gradle configuration cache і build performance.

6. **Правило adoption**

Нову фічу не треба тягнути в production лише тому, що вона нова. Перевіряємо:

- stable чи experimental;
- IDE/CI підтримку;
- plugin compatibility;
- readability;
- migration/rollback plan.

**Коротко:** у 2026 треба знати Kotlin 2.x/K2, context APIs, backing fields, розвиток KMP/Wasm/Compose Multiplatform і tooling constraints. На практиці важливіше безпечне оновлення ecosystem, ніж використання кожної нової синтаксичної фічі.

</details>

<details>
<summary>84. Що таке життєвий цикл Activity?</summary>

#### Kotlin

Lifecycle `Activity` — це послідовність callbacks, через які Android керує створенням, видимістю, foreground/background станом і знищенням екрана.

Основний порядок:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Після повернення зі `STOPPED` може бути `onRestart()` перед `onStart()`.

- `onCreate()` — створити UI, navigation, базові dependencies. Не робити важку sync-роботу.
- `onStart()` — Activity стала видимою.
- `onResume()` — Activity у foreground і готова до взаємодії.
- `onPause()` — втратила focus; callback має бути швидким.
- `onStop()` — більше не видима; зупинити visible-only resources.
- `onDestroy()` — instance знищується через `finish()` або recreation; не гарантується при kill process.

Configuration change може пересоздати Activity:

```text
old: onPause -> onStop -> onDestroy
new: onCreate -> onStart -> onResume
```

`ViewModel` переживає configuration change. Невеликий UI state кладуть у `SavedStateHandle`/`onSaveInstanceState`, довготривалі дані — у storage.

Flow collection треба привʼязувати до lifecycle:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**Коротко:** `onCreate` створює екран, `onStart` робить його видимим, `onResume` активним, `onPause/onStop` зупиняють роботу, `onDestroy` завершує instance. State не можна покладати тільки на Activity instance.

</details>

<details>
<summary>85. Що таке Fragment і чим він відрізняється від Activity?</summary>

#### Kotlin

`Activity` — Android component із власним window/task і entry point через manifest. `Fragment` — UI controller всередині Activity або іншого Fragment, яким керує `FragmentManager`.

```text
Activity -> system component, window, intents, task
Fragment -> частина UI всередині host, back stack через FragmentManager
```

Activity приклад:

```kotlin
class MainActivity : AppCompatActivity()
```

Fragment приклад:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

Головна практична різниця — Fragment має окремий lifecycle для View:

```text
onCreateView -> onViewCreated -> onDestroyView
```

Fragment object може залишатися, а його View вже знищена. Тому binding, adapter і UI subscriptions очищають у `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Flow/LiveData для UI треба привʼязувати до `viewLifecycleOwner`, а не до lifecycle самого Fragment.

Navigation:

- між Activities — `Intent`;
- між Fragments — `FragmentManager` або Navigation Component.

У повністю Compose app часто достатньо однієї Activity без Fragments. У hybrid/View app Fragment все ще нормальна screen/navigation abstraction.

**Коротко:** Activity — самостійний Android component. Fragment — частина UI всередині host. Найважливіше правило: View у Fragment живе менше, ніж сам Fragment, тому cleanup робиться в `onDestroyView()`.

</details>

<details>
<summary>86. Що таке Intent і як він використовується?</summary>

#### Kotlin

`Intent` — Android message object, який описує дію або конкретний component: відкрити Activity, Service, Broadcast чи передати дані.

1. **Explicit Intent**

Вказує конкретний component:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

Використовується для internal navigation між Activities.

2. **Implicit Intent**

Описує дію, а Android шукає app, який її виконає:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Приклади: browser, share, dialer, maps, file picker.

3. **Extras**

Через extras передають невеликі дані:

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Краще передавати `id`, а не великі objects. Для structured args можна `Parcelable`, але navigation часто простіша через id + repository.

4. **Intent filter**

Компонент оголошує, які intents приймає:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

Це потрібно для deep links, share targets і system actions.

5. **Activity Result API**

Замість `startActivityForResult` використовують lifecycle-aware API:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // handle uri
}

launcher.launch("image/*")
```

6. **Security**

- secrets не передавати в extras;
- internal components запускати explicit intent;
- перевіряти external input;
- правильно ставити `android:exported`;
- для implicit intent враховувати, що handler може бути відсутній.

**Коротко:** explicit intent запускає конкретний component, implicit intent описує дію для системи. Extras — тільки для невеликих даних. Для результатів — Activity Result API. Для external intents потрібна валідація і security awareness.

</details>

<details>
<summary>87. Що станеться, якщо не викликати super.onCreate()?</summary>

#### Kotlin

Якщо в `Activity.onCreate()` не викликати `super.onCreate(savedInstanceState)`, базовий Android/Jetpack-клас не виконає обовʼязкову lifecycle-ініціалізацію. Це може дати crash або некоректну роботу Activity.

Правильно:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` готує:

- lifecycle state;
- saved state restoration;
- window/decor infrastructure;
- `FragmentManager` у `FragmentActivity`;
- `SavedStateRegistry`;
- `ActivityResultRegistry`;
- AppCompat/ComponentActivity інтеграції.

Без `super` можливі:

- `SuperNotCalledException`;
- зламаний lifecycle;
- проблеми з fragments;
- не відновиться saved state;
- проблеми з Activity Result API, Compose, Hilt або AppCompat.

Для Compose правило те саме:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

У Fragment lifecycle callback-ах також зазвичай викликають `super`, якщо документація явно не каже інакше.

**Коротко:** `super.onCreate()` — не формальність, а частина контракту з framework. Його треба викликати на початку `onCreate()` перед UI/framework-залежною логікою.

</details>

<details>
<summary>88. Що таке View Binding і чому він кращий за findViewById?</summary>

#### Kotlin

`View Binding` генерує binding-клас для XML layout і дає type-safe доступ до View без ручного `findViewById`.

Проблема `findViewById`:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Мінуси:

- boilerplate;
- можна помилитися з типом;
- помилка буде runtime;
- refactoring слабший.

View Binding в `Activity`:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

У `Fragment` binding треба очищати в `onDestroyView()`, бо View lifecycle коротший за lifecycle Fragment:

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

Переваги View Binding:

- type safety;
- менше boilerplate;
- compile-time перевірка id;
- зручний refactoring;
- nullable поля, якщо View відсутня в певній layout configuration.

`View Binding` не те саме, що `Data Binding`:

```text
View Binding -> доступ до View
Data Binding -> binding expressions у XML
```

Для повністю Compose UI View Binding зазвичай не потрібен. Він актуальний для XML/View System і hybrid screens.

**Коротко:** View Binding — безпечніша й коротша заміна `findViewById` для XML layouts. Головний нюанс: у Fragment очищати binding у `onDestroyView()`.

</details>

<details>
<summary>89. Що таке LiveData?</summary>

#### Kotlin

`LiveData` — lifecycle-aware observable data holder з Android Jetpack. Він зберігає значення і повідомляє observer-и тільки коли `LifecycleOwner` у стані `STARTED` або `RESUMED`.

Типовий приклад у `ViewModel`:

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

Назовні віддаємо `LiveData`, а не `MutableLiveData`, щоб UI не міг змінювати state напряму.

Підписка у Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` важливий, бо View Fragment живе менше, ніж сам Fragment.

`value` vs `postValue`:

```kotlin
_liveData.value = user      // main thread
_liveData.postValue(user)   // background thread, async
```

Переваги:

- lifecycle-aware;
- простий API;
- добре працює з XML/View System;
- автоматично відписує destroyed owner;
- зручний для legacy Android-коду.

Обмеження:

- привʼязаний до Android;
- слабший API для streams, ніж `Flow`;
- незручний для KMP;
- one-shot events часто ведуть до `SingleLiveEvent`-подібних костилів.

Порівняння:

```text
LiveData  -> Android lifecycle-aware holder
StateFlow -> Kotlin hot stream зі state value
```

У новому coroutine-first або Compose-коді частіше використовують `StateFlow` + `collectAsStateWithLifecycle()`. `LiveData` доречний у legacy View System або проектах, де він уже є стандартом.

**Коротко:** LiveData — Android observable state holder, який враховує lifecycle. Для нового Kotlin/Compose коду частіше краще `StateFlow`, але в XML/View System LiveData досі нормально працює.

</details>

<details>
<summary>90. Що таке Room і як він працює?</summary>

#### Kotlin

`Room` — Jetpack-бібліотека для роботи з SQLite через типобезпечний API. Вона не прибирає SQL, а дає compile-time перевірки, DAO, migrations і зручну інтеграцію з coroutines/Flow.

Основні частини:

```text
@Entity   -> таблиця
@Dao      -> SQL access methods
@Database -> entry point до бази
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

Створення:

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room підтримує `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Коли таблиця змінюється, Room invalidates query і emit-ить нові дані.

Для relations часто потрібен `@Transaction`:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Schema changes роблять через migrations:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

У production не можна бездумно використовувати destructive migration, якщо є user data.

Практично Room ховають за repository:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

UI не має напряму залежати від database entities.

**Коротко:** Room — типобезпечна обгортка над SQLite через `Entity`, `Dao` і `Database`. Він підтримує suspend/Flow, relations і migrations. У нормальній архітектурі Room живе в data layer за repository, а не напряму в UI.

</details>

<details>
<summary>91. Що роблять анотації @Query, @Insert, @Delete?</summary>

#### Kotlin

`@Query`, `@Insert`, `@Delete` — це Room-анотації для DAO-методів. Room генерує implementation і перевіряє SQL/mapping на етапі компіляції.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

Виконує явний SQL:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` або `:name` — bind parameters. Не треба конкатенувати user input у SQL string.

`@Query` може бути не тільки `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

`Int` — кількість affected rows.

### `@Insert`

Вставляє `@Entity`:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` визначає поведінку при конфлікті primary/unique key: `ABORT`, `IGNORE`, `REPLACE`. З `REPLACE` треба бути обережним: SQLite може фактично видалити старий row і вставити новий. Для insert-or-update часто краще `@Upsert`, якщо він доступний у проекті.

### `@Delete`

Видаляє entity за primary key:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Для видалення за id або умовою краще `@Query("DELETE ...")`.

### `@Transaction`

Якщо кілька DAO-операцій мають бути атомарними, використовують `@Transaction`:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room перевіряє tables, columns, bind parameters і return mapping. Але performance, indexes і бізнес-логіку все одно має перевіряти розробник.

**Коротко:** `@Query` виконує SQL, `@Insert` вставляє entity, `@Delete` видаляє entity за primary key. Room генерує DAO-код і ловить багато помилок на compile time, але не замінює аналіз query performance.

</details>

<details>
<summary>92. Що таке WorkManager і коли його використовувати?</summary>

#### Kotlin

`WorkManager` — Jetpack API для persistent deferrable background work. Тобто задача може стартувати пізніше, але має пережити закриття app, process death і за потреби reboot.

Використовують для:

- background sync;
- retry upload/download;
- cleanup;
- logs/analytics dispatch;
- periodic maintenance;
- роботи з constraints: network, charging, battery/storage.

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

Результати:

- `Result.success()` — задача виконана;
- `Result.retry()` — повторити з backoff policy;
- `Result.failure()` — terminal failure.

Важливі можливості:

- constraints;
- retries/backoff;
- unique work через `enqueueUniqueWork()`;
- chaining;
- periodic work, але без точного часу запуску.

`Data` для input/output має бути малим. Великі payloads треба класти в DB/file і передавати id.

WorkManager не підходить для:

- exact alarms;
- realtime socket connection;
- media playback;
- постійного location tracking;
- дії, яку користувач очікує негайно.

Для ongoing user-visible роботи потрібен foreground service. Для точного часу — AlarmManager/Exact Alarm API, якщо це справді дозволений use case.

**Коротко:** WorkManager — для надійної відкладеної background роботи з constraints і retry. Він не гарантує точний час запуску і не замінює foreground service, realtime connection або exact alarm.

</details>

<details>
<summary>93. У чому різниця між CoroutineScope, lifecycleScope та viewModelScope?</summary>

#### Kotlin

`CoroutineScope` задає context і lifetime корутин. `lifecycleScope` та `viewModelScope` — готові Android scopes з уже визначеним owner-ом.

```text
CoroutineScope  -> lifetime визначаєш сам
lifecycleScope  -> живе до DESTROYED у LifecycleOwner
viewModelScope  -> живе до ViewModel.onCleared()
```

### `CoroutineScope`

Загальний scope для корутин:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Якщо створив custom scope — маєш явно викликати `cancel()` у правильний момент. Інакше буде leak або зайва робота після смерті owner-а.

### `lifecycleScope`

Належить `Activity`, `Fragment` або іншому `LifecycleOwner` і скасовується при `DESTROYED`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Сам `lifecycleScope.launch` не паузиться в `STOPPED`, тому для Flow collection потрібен `repeatOnLifecycle`.

У Fragment для роботи з View/binding краще використовувати `viewLifecycleOwner.lifecycleScope`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Причина: Fragment object може жити довше, ніж його View.

### `viewModelScope`

Належить `ViewModel`, переживає configuration change і скасовується в `onCleared()`:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Використовується для screen logic, loading, business calls і формування UI state.

Типові помилки:

- `GlobalScope` для screen work;
- custom scope без cancellation;
- collection binding/state у Fragment scope замість View lifecycle scope;
- запуск бізнес-операцій з Activity замість ViewModel;
- доступ до binding після `onDestroyView()`.

**Коротко:** `viewModelScope` — для логіки екрана у ViewModel, `lifecycleScope` — для UI collection/effects, custom `CoroutineScope` — тільки коли є власний чіткий lifetime і місце для `cancel()`.

</details>

<details>
<summary>94. Як реалізується dependency injection у Kotlin?</summary>

#### Kotlin

Dependency Injection — це передача залежностей ззовні, а не створення їх усередині класу. У Kotlin default-підхід — constructor injection.

Правильно:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Погано:

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

Другий варіант жорстко привʼязує клас до implementation і ускладнює тести.

Зазвичай use case залежить від contract:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Manual DI

Для малого проєкту достатньо простого container-а:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Плюс — простота. Мінус — великий graph важко підтримувати вручну.

### Hilt/Dagger

Compile-time DI, типовий вибір для великих Android apps:

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

Плюс — помилки graph ловляться на compile time. Мінус — більше boilerplate і складніша генерація.

### Koin

Runtime DI через Kotlin DSL:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

Плюс — простіше стартувати. Мінус — частина помилок може проявитись runtime.

DI також керує lifetime: `singleton`, `factory`, `ViewModel scope`. Scope треба вибирати обережно, щоб не отримати leaks або зайві singletons.

**Коротко:** у Kotlin DI найчастіше роблять через constructor injection. Manual DI підходить для малого graph, Hilt/Dagger — для великих Android apps, Koin — для простоти. DI зменшує coupling і сильно спрощує тестування.

</details>

<details>
<summary>95. Що таке Hilt і як він працює?</summary>

#### Kotlin

`Hilt` — Android DI framework поверх Dagger. Він генерує dependency graph і готову component hierarchy для `Application`, `Activity`, `Fragment`, `ViewModel`, `View` і `Service`.

Базовий setup:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` створює root graph на рівні application.

Android class, куди треба inject dependencies:

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Constructor injection — основний спосіб:

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

Modules потрібні, коли Hilt не може сам створити dependency: interface, third-party class, builder/factory.

`@Binds` — для interface → implementation:

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

`@Provides` — для створення через factory/builder, наприклад `OkHttpClient`, `Retrofit`, `Room`.

Scopes визначають lifetime object-а:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Основні components:

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Важливе правило: довгоживучий component не може залежати від короткоживучого. Наприклад, `@Singleton` dependency не має тримати `Activity` context.

Для тестів Hilt дозволяє замінювати modules через `@TestInstallIn`. Але в unit-тестах часто простіше передати fake напряму через constructor.

**Коротко:** Hilt генерує Dagger graph для Android. `@Inject` створює залежності, `@Binds/@Provides` описують bindings, `@AndroidEntryPoint` вмикає injection у Android class, а scopes контролюють lifetime.

</details>

<details>
<summary>96. Що таке @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` інтегрує `ViewModel` з Hilt. Hilt генерує `ViewModelProvider.Factory` і передає dependencies через constructor injection, тому ручна factory зазвичай не потрібна.

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

Правила:

- `@HiltViewModel` ставиться на клас;
- `@Inject constructor` — на constructor;
- усі dependencies мають бути в Hilt graph;
- `SavedStateHandle` Hilt надає автоматично;
- host `Activity/Fragment` має мати `@AndroidEntryPoint`.

Отримання у Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

Для shared ViewModel на рівні Activity:

```kotlin
private val viewModel: UserViewModel by activityViewModels()
```

У Compose:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

Scope ViewModel визначає `ViewModelStoreOwner`: Fragment, Activity або navigation back stack entry.

Залежності ViewModel живуть у `ViewModelComponent`. `@ViewModelScoped` дає один instance на конкретну ViewModel. `@Singleton` — один instance на application graph.

Типові помилки:

- забути `@HiltViewModel` або `@Inject constructor`;
- забути `@AndroidEntryPoint` у host;
- створювати ViewModel вручну через constructor;
- очікувати один instance при різних `ViewModelStoreOwner`;
- інжектити `Activity`, `Fragment`, `View` або короткоживучий `Context` у ViewModel.

**Коротко:** `@HiltViewModel` дозволяє Hilt створювати ViewModel через generated factory, інжектити constructor dependencies і `SavedStateHandle`. Реальний lifetime визначає `ViewModelStoreOwner`, а не сама анотація.

</details>

<details>
<summary>97. Як працює @Inject?</summary>

#### Kotlin

`@Inject` — анотація, за якою Dagger/Hilt розуміє, як створити обʼєкт або куди вставити dependency.

Основний варіант — constructor injection:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt зможе створити `LoadUserUseCase`, якщо в graph є binding для кожного constructor parameter. Якщо binding відсутній або неоднозначний — буде compile-time error.

Constructor injection кращий для власних класів:

- dependencies явні;
- можна використовувати `val`;
- клас легко тестувати;
- немає прихованого framework magic.

Field injection використовують переважно для Android classes, які створює framework:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Такий field не можна використовувати до injection у lifecycle компонента.

`@Inject constructor` не вирішує interface binding сам по собі. Для interface потрібен `@Binds`:

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

Для third-party classes або builder-ів використовують `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` не задає lifetime. Lifetime задають scopes: `@Singleton`, `@ViewModelScoped`, `@ActivityScoped`. Якщо є кілька bindings одного типу, використовують qualifiers.

Hilt/Dagger генерує factories і graph на compile time, без runtime reflection для створення залежностей.

**Коротко:** `@Inject` показує Hilt/Dagger, як створити class або куди вставити dependency. Constructor injection — default; `@Binds` потрібен для interfaces, `@Provides` — для external objects, scopes — для lifetime, qualifiers — для розрізнення однакових типів.

</details>

<details>
<summary>98. Що таке області видимості (scopes) у Hilt?</summary>

#### Kotlin

Scope у Hilt визначає, в межах якого component кешується dependency. Тобто всі запити в одному component instance отримують той самий object.

Без scope binding зазвичай створюється заново для кожного injection request. Це нормальний default для дешевих stateless objects.

Спрощена ієрархія components:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

Child бачить parent bindings, але parent не може залежати від короткоживучого child binding.

Основні scopes:

- `@Singleton` — один instance на application graph;
- `@ActivityRetainedScoped` — один instance на logical Activity, переживає configuration changes;
- `@ViewModelScoped` — один instance на конкретну ViewModel;
- `@ActivityScoped` — один instance на конкретний Activity instance;
- `@FragmentScoped` — один instance на конкретний Fragment;
- `@ViewScoped` — один instance на Hilt-enabled View;
- `@ServiceScoped` — один instance на конкретний Service.

Scope має відповідати `@InstallIn` component:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

`@ViewModelScoped` dependency буде одна для конкретної ViewModel, але різна для різних ViewModels. Якщо object має бути спільним між ViewModels однієї Activity — потрібен `@ActivityRetainedScoped`.

Типова помилка — довгоживучий object тримає короткоживучий context:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Так робити не можна: singleton утримає Activity після destroy. Для `@Singleton` потрібен application-level state/context.

Не треба scope-ити все підряд. Scope — це ownership/lifecycle semantics, а не просто optimization.

**Коротко:** Hilt scope визначає lifetime і reuse dependency в межах component. Scope має відповідати реальному owner-у й не повинен утримувати objects з коротшим lifecycle.

</details>

<details>
<summary>99. Як уникати витоків памʼяті в Android?</summary>

#### Kotlin

Memory leak виникає, коли обʼєкт уже не потрібен, але на нього досі є strong reference. В Android найчастіше витікають `Activity`, `Fragment`, `View`, `Context`, callbacks або coroutines.

### Не тримати Activity/View у довгоживучих обʼєктах

Погано:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

Якщо це `Activity context`, Activity не звільниться після rotation/finish. Для app-level залежностей використовують `applicationContext`.

### Очищати ViewBinding у Fragment

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Fragment object може жити довше за свою View, тому View references очищають у `onDestroyView()`.

### Очищати adapters/listeners/callbacks

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Adapters, listeners і anonymous callbacks часто тримають View або Fragment reference. Їх треба unregister/clear у правильному lifecycle callback.

### Використовувати lifecycle-aware coroutines

Для Fragment UI collection:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Не запускати UI coroutines у scope, який живе довше View. `GlobalScope` для UI/app flow не використовувати.

### Compose

У Compose side effects треба чистити через `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

Не тримати `Activity` або `View` у `remember`, якщо немає чіткої lifecycle-причини.

### Інструменти

- LeakCanary;
- Android Studio Memory Profiler;
- heap dumps;
- allocation tracking.

`WeakReference` не є нормальним архітектурним рішенням. Якщо він “потрібен”, спочатку треба перевірити ownership і lifecycle.

**Коротко:** не тримати короткоживучі Android objects у довгоживучих класах, очищати Fragment View references у `onDestroyView()`, використовувати lifecycle-aware coroutines, unregister callbacks і перевіряти leaks через LeakCanary/Profiler.

</details>

<details>
<summary>100. Як безпечно зберігати API-ключі в Android-додатку?</summary>

#### Kotlin

Повністю безпечно зберегти secret у Android app неможливо. Все, що потрапило в APK/AAB, можна потенційно витягнути через decompile, proxy, memory inspection або rooted device.

Головне правило: справжні secrets не кладуть у клієнт.

Погано:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Якщо ключ дає доступ до backend/admin/payment/service account — він має бути тільки на сервері.

Правильна схема:

```text
Android app -> your backend -> third-party API
```

Backend додає secret, перевіряє user/session, робить rate limit, audit і повертає тільки потрібний результат.

Не всі ключі однакові:

```text
public/client key -> можна класти в app, але треба обмежувати
secret/admin key  -> не можна класти в app
```

Якщо client key все ж є в app, його треба обмежити:

- package name;
- SHA-256 certificate fingerprint;
- allowed APIs;
- quotas/rate limits;
- environment separation;
- monitoring;
- rotation plan.

`BuildConfig`, `local.properties` або CI secrets допомагають не комітити ключ у Git, але не захищають його в зібраному app:

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

Після decompile такий ключ можна побачити.

NDK, R8/ProGuard і string obfuscation лише ускладнюють extraction. Це friction, не security boundary.

Android Keystore корисний для device-generated keys і зберігання user tokens, але не рятує hardcoded API secret: attacker може знайти місце, де app використовує secret.

User access/refresh tokens треба зберігати обережно:

- EncryptedSharedPreferences або encrypted DataStore;
- Android Keystore для crypto key;
- короткий lifetime access token;
- refresh token rotation;
- logout очищає storage;
- backend підтримує revoke.

**Коротко:** справжні API secrets мають жити на backend. У клієнті можуть бути тільки обмежені public/client keys. `BuildConfig`, obfuscation і NDK не гарантують безпеку; потрібні restrictions, monitoring і rotation.

</details>

<details>
<summary>101. Як реалізувати offline-first архітектуру?</summary>

#### Kotlin

Offline-first означає: UI читає локальний source of truth, а network лише синхронізує його.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Read path

UI підписується на Room:

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

Refresh бере remote data і транзакційно merge-ить у DB:

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

Після запису в Room UI оновиться через `Flow`. `replaceAll` небезпечний, якщо response partial або є pending local edits.

### Write path / outbox

Offline write спочатку пишеться локально:

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

UI одразу бачить optimistic update. Outbox operation відправляється пізніше і видаляється тільки після server acknowledgement.

Важливо: local update і outbox insert мають бути в одній transaction, інакше process death може втратити operation.

### Sync

Для retry/process-death resilience використовують WorkManager:

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

Sync має бути idempotent, бо retry може повторити request. Foreground refresh і worker мають використовувати одну sync implementation.

### Conflicts

Conflict policy залежить від domain:

- server wins;
- client wins;
- revision/ETag;
- field-level merge;
- manual resolution.

Client timestamp ненадійний через clock skew. Для критичних доменів простий last-write-wins часто неприйнятний.

### UI state

UI має розрізняти:

- cached data;
- initial loading без local data;
- refreshing зі старими data;
- offline;
- pending/failed changes;
- last successful sync.

Connectivity — лише hint. Джерело істини — результат реального request-а.

**Коротко:** offline-first будується навколо локального source of truth. Reads ідуть з Room, remote data merge-иться в DB, writes йдуть через transactional outbox, sync виконує WorkManager, а conflict policy визначає domain.

</details>

<details>
<summary>102. Що таке архітектура MVI і коли її використовувати?</summary>

#### Kotlin

MVI — архітектурний підхід з unidirectional data flow:

```text
UI -> Event/Intent -> ViewModel/Reducer -> State -> UI
                                      └-> Effect
```

UI не змінює state напряму. Він рендерить immutable `State` і відправляє `Event`. ViewModel обробляє event і створює новий state.

Типові частини:

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
- `Event/Intent` — дія користувача або system input;
- `Reducer/Handler` — перехід `old state -> new state`;
- `Effect` — одноразова дія: navigation, snackbar, permission request.

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

State оновлюють через `copy/update`:

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

Compose UI тільки читає state і відправляє events:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Переваги:

- один source of truth для screen state;
- передбачувані transitions;
- легше тестувати reducer/ViewModel;
- добре підходить для Compose;
- зручно логувати events/states.

Недоліки:

- більше boilerplate;
- reducer може стати god object;
- one-shot effects потребують чіткої delivery policy;
- concurrency/races MVI не вирішує автоматично;
- для простого CRUD може бути зайвим.

MVI варто використовувати для складних screens: багато state transitions, retry, optimistic updates, validation, multi-step flows. Для простого screen достатньо `ViewModel + StateFlow + methods`.

**Коротко:** MVI — це цикл `Event -> State transition -> Render`. Він корисний для складного UI зі станами, але потребує дисципліни з effects і concurrency та може бути overkill для простих екранів.

</details>

<details>
<summary>103. Як проєктувати масштабовані Android-додатки?</summary>

#### Kotlin

Масштабований Android-додаток — це додаток, де feature можна змінювати локально, без каскадних правок по всьому codebase.

Базові шари:

```text
UI -> ViewModel -> domain -> repository -> API/DB
```

- UI рендерить state;
- ViewModel керує screen state і events;
- domain містить бізнес-правила;
- repository ховає data sources;
- data layer працює з Retrofit/Room/SDK.

Шари не додають механічно. Use case або interface потрібні, коли є business rule, boundary, alternative implementation або окрема потреба в тестуванні.

### Feature-first modules

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Feature module групує код, що змінюється разом. `core` має містити справді shared infrastructure, а не випадкові utilities.

Dependency rule:

```text
app -> feature
feature -> core
core -X-> feature
```

Feature не повинна залежати від implementation іншої feature. Для інтеграції краще contracts, navigation API або API/implementation split.

### Contracts і models

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Interface потрібен на реальному boundary. Не треба створювати interface для кожного class “про всяк випадок”.

DTO, Entity, Domain і UiModel розділяють, якщо вони мають різні semantics або lifecycle. Якщо models однакові й не мають різних причин змінюватися — зайвий mapping тільки шкодить.

### State, errors, concurrency

UI має отримувати один observable state і надсилати events. ViewModel має явно визначити concurrency policy: cancel previous, ignore duplicate, serialize або merge.

Raw `Throwable` не має бути UI contract. Infrastructure errors треба перетворювати на domain/UI outcomes на boundary.

### DI, tests, build

- constructor injection як default;
- scopes відповідають lifecycle owner-у;
- domain/unit tests без Android;
- ViewModel state tests;
- repository integration tests;
- convention plugins і version catalog;
- affected-module CI;
- logging, metrics, crash context;
- migration plan для API/DB changes.

Модульність без тестів, observability і зрозумілого ownership не масштабує продукт.

**Коротко:** масштабованість дають feature ownership, контрольований dependency graph, stable contracts, testable boundaries, explicit state/concurrency і production support. Модулі та шари додають лише тоді, коли вони реально ізолюють зміни.

</details>

<details>
<summary>104. Що таке Jetpack Compose і чим він відрізняється від View System?</summary>

#### Kotlin

`Jetpack Compose` — declarative UI toolkit для Android. UI описують Kotlin `@Composable` функціями як результат поточного state, без XML і ручного оновлення `View`.

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

Коли state змінюється, Compose запускає recomposition потрібних частин UI.

Composable function:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Composable має рендерити state і викликати callbacks. Business logic, I/O і довгі операції не повинні жити в UI-функціях.

Типова схема:

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

ViewModel формує state, Compose його відображає.

Головні відмінності:

```text
View System -> XML + View tree + manual updates + listeners
Compose     -> Kotlin functions + state-driven rendering + recomposition
```

Практичні нюанси Compose:

- composable body може виконуватися багато разів;
- side effects запускати через effect APIs;
- локальний state/objects кешувати через `remember`, коли потрібно;
- screen state тримати у ViewModel;
- lazy lists мають мати stable keys;
- performance перевіряти profiler-ом, а не рахувати recompositions.

Compose і View System можна змішувати:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

Або вставити View у Compose через `AndroidView`. Тому міграція може бути поступовою.

**Коротко:** Compose описує UI як функцію від state і оновлює його через recomposition. View System вимагає XML/View tree та ручних змін View. Compose краще підтримує state-driven UI, але вимагає дисципліни з side effects і state ownership.

</details>

<details>
<summary>105. Що таке @Composable і що таке recomposition?</summary>

#### Kotlin

`@Composable` — анотація для функцій Jetpack Compose, які описують UI. `Recomposition` — повторний виклик composable, коли state, від якого він залежить, змінився.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Composable — це функція від state:

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

Коли `state` змінюється, Compose оновлює потрібну частину UI через recomposition.

Приклад local state:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

Зміна `count` запускає recomposition частини UI, яка читає `count`.

Recomposition можуть запускати:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable`;
- зміна parameters composable.

Composable body може виконуватись багато разів, тому там не можна робити side effects:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // погано
}
```

Правильно:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` зберігає значення між recompositions:

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

Recomposition сама по собі не проблема. Проблема — важка робота в composable, широке читання state, нестабільні parameters, зайві allocations або lazy lists без stable keys.

Практичні правила:

- composable має бути легким і side-effect free;
- state читати ближче до місця використання;
- expensive objects кешувати через `remember`;
- effects запускати через `LaunchedEffect`, `DisposableEffect` тощо;
- для списків використовувати stable keys;
- performance перевіряти profiler-ом, не здогадками.

**Коротко:** `@Composable` описує UI як функцію від state. `Recomposition` повторно виконує потрібні composable при зміні state. Це нормальний механізм Compose, якщо UI-функції легкі, без side effects і з правильно розміщеним state reading.

</details>

<details>
<summary>106. Що таке state hoisting?</summary>

#### Kotlin

`State hoisting` — це винесення state з composable до parent або state holder. Child отримує значення і callback, тому стає stateless, reusable і testable.

Stateful варіант:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Після hoisting:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

Parent стає source of truth:

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

Для screen-level state owner-ом зазвичай є `ViewModel`:

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

Compose route читає state і передає events:

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

Hoisting підтримує unidirectional data flow:

```text
state вниз -> UI
 events вгору -> owner оновлює state
```

State треба піднімати до найнижчого спільного owner-а, який має його читати або змінювати. Якщо state потрібен тільки всередині компонента і не впливає на бізнес-логіку, його можна залишити локально.

Типовий API stateless composable:

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**Коротко:** state hoisting — це “state вниз, events вгору”. Child не володіє state, а parent або ViewModel стає єдиним source of truth.

</details>

<details>
<summary>107. У чому різниця між remember і rememberSaveable?</summary>

#### Kotlin

`remember` і `rememberSaveable` зберігають значення між recompositions. Різниця в тому, що `rememberSaveable` додатково зберігає value у saved state і може відновити його після configuration change або process recreation.

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

Після recreation Activity значення буде втрачено.

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

На Android автоматично підтримуються типи, які можна покласти в saved-state `Bundle`: primitives, `String`, `Parcelable`, `Serializable` тощо. Для custom type потрібен `Saver`:

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

Коли що використовувати:

- `remember` — transient UI state або кешований object, який можна втратити;
- `rememberSaveable` — невеликий UI state, який користувач очікує зберегти: input text, selected tab, filter;
- `ViewModel` — screen/business state;
- storage/DataStore/DB — persistent data.

Обмеження `rememberSaveable`:

- saved state має обмежений розмір;
- великі objects/collections не зберігають;
- це не довготривале persistence;
- не замінює `ViewModel` або repository.

**Коротко:** `remember` живе тільки в composition. `rememberSaveable` відновлює невеликий saveable UI state після recreation. Для screen state потрібен `ViewModel`, для довготривалих даних — storage layer.

</details>

<details>
<summary>108. Що таке rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` — Compose API, який повертає `CoroutineScope`, привʼязаний до поточної composition. Scope скасовується, коли composable виходить з composition.

Використовується, коли suspend UI-дію треба запустити з callback: click, swipe, drawer, snackbar, scroll animation.

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

`suspend` function не можна напряму викликати з `onClick`, тому потрібен scope.

Ще приклад:

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

Різниця з `LaunchedEffect`:

```text
LaunchedEffect         -> coroutine стартує автоматично при вході/зміні key
rememberCoroutineScope -> coroutine стартує вручну з callback
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

Не треба використовувати `rememberCoroutineScope()` для бізнес-логіки:

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

А у ViewModel:

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

`GlobalScope` для UI не використовують, бо він не привʼязаний до lifecycle і може працювати після закриття екрана.

**Коротко:** `rememberCoroutineScope()` потрібен для коротких suspend UI-операцій з callbacks: snackbar, drawer, scroll. Для автоматичних effects — `LaunchedEffect`, для screen/business logic — `viewModelScope`.

</details>

<details>
<summary>109. Що таке collectAsState?</summary>

#### Kotlin

`collectAsState()` — Compose API, який collect-ить `Flow` і перетворює його на Compose `State`. Коли Flow emit-ить нове значення, Compose запускає recomposition там, де це state читається.

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

Механіка:

```text
Flow emit -> State update -> recomposition -> UI update
```

Для звичайного `Flow` потрібен initial value:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

Для `StateFlow` initial value вже є в `state.value`.

В Android краще використовувати lifecycle-aware варіант:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Різниця:

```text
collectAsState              -> Compose runtime, без Android lifecycle
collectAsStateWithLifecycle -> collect тільки в активному lifecycle state
```

У composable body не можна напряму collect-ити Flow:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // погано
}
```

Для screen state використовують `collectAsStateWithLifecycle()`. Для one-shot events — `LaunchedEffect` і окремий `SharedFlow`:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigate або show snackbar
    }
}
```

`State` — це те, що можна повторно відрендерити. `Event` — одноразова дія.

Якщо Flow emit-ить занадто часто, треба зменшити noise: `distinctUntilChanged()`, менші state-моделі, локальні state reads, без важкої роботи в composable.

**Коротко:** `collectAsState()` перетворює Flow у Compose State. В Android зазвичай використовують `collectAsStateWithLifecycle()`. Для one-shot navigation/snackbar events потрібен `LaunchedEffect`, а не state collection.

</details>

<details>
<summary>110. Що таке Modifier у Compose?</summary>

#### Kotlin

`Modifier` у Compose описує layout, drawing, input, semantics і поведінку composable: `padding`, `size`, `background`, `clickable`, `scroll`, accessibility тощо.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

`Modifier` immutable і будується як chain. Порядок має значення:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

і:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

дадуть різний результат, бо background застосовується на різному етапі.

Reusable composable має приймати modifier ззовні:

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

Так parent може контролювати позиціонування і зовнішню поведінку child.

Основні типи modifiers:

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

Для accessibility краще використовувати високорівневі modifiers типу `clickable`, бо вони додають semantics. За потреби semantics задають явно:

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Деякі modifiers працюють тільки в scope parent-а:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Практичні правила:

- приймати `modifier: Modifier = Modifier`;
- parent відповідає за зовнішнє позиціонування;
- child може додати внутрішні modifiers після переданого modifier;
- порядок modifier-ів важливий;
- не робити важку логіку в modifier callbacks.

**Коротко:** `Modifier` — immutable chain налаштувань composable. Він керує layout, drawing, input і semantics. Порядок викликів важливий, а reusable composables мають приймати `modifier` параметром.

</details>

<details>
<summary>111. У чому різниця між padding і offset?</summary>

#### Kotlin

`padding` і `offset` змінюють розташування UI, але працюють на різних етапах layout.

```text
padding -> змінює measurement і reserved space
offset  -> змінює placement без зміни reported size
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

`padding` додає простір навколо content і впливає на розмір, який бачать parent та siblings.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` буде розміщений після `A` разом із padding.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` пересуває composable при placement, але parent резервує старе місце.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` не відсунеться, тому елементи можуть overlap. Це корисно для badge, drag, animation або декоративного shift.

### RTL

`offset(x, y)` враховує layout direction: positive `x` рухає до logical end. Якщо потрібні фізичні координати без RTL-дзеркалення — `absoluteOffset()`.

### Порядок modifiers

Порядок впливає на layout, drawing і hit area:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

Background охопить більшу область.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

Background буде тільки після padding.

Так само з `clickable`: якщо `clickable` стоїть до `padding`, padding входить у click area; якщо після — ні.

**Коротко:** `padding` додає layout space і впливає на siblings. `offset` лише пересуває composable без нового reserved space. Порядок modifiers критично впливає на вигляд і hit testing.

</details>

<details>
<summary>112. У чому різниця між fillMaxSize і matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` і `matchParentSize()` можуть зробити child розміром з parent, але працюють по-різному.

```text
fillMaxSize()     -> бере maximum constraints і впливає на measurement
matchParentSize() -> повторює вже визначений розмір Box
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

`fillMaxSize()` просить максимально доступний розмір від parent-а. Якщо constraints обмежені екраном, composable займе весь екран.

Для однієї осі є:

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` бере участь у measurement і може вплинути на розмір parent layout.

### `matchParentSize()`

`matchParentSize()` працює тільки в `BoxScope` і тільки для direct child `Box`:

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

Такий child повторює вже обчислений розмір `Box`, але не визначає його.

Спрощено:

1. `Box` вимірює normal children;
2. визначає власний розмір;
3. children з `matchParentSize()` отримують цей готовий розмір.

Тому `matchParentSize()` добре підходить для overlay: loading layer, scrim, gradient, clickable layer.

Якщо замінити overlay на `fillMaxSize()`, child може попросити максимум constraints і збільшити `Box`. Це вже інша поведінка.

Практичний вибір:

- root/container має зайняти весь доступний простір — `fillMaxSize()`;
- overlay має покрити content-sized `Box` — `matchParentSize()`;
- child не має впливати на розмір `Box` — `matchParentSize()`;
- поза `BoxScope` `matchParentSize()` недоступний.

**Коротко:** `fillMaxSize()` просить максимум доступного простору і бере участь у measurement. `matchParentSize()` у `BoxScope` лише повторює вже визначений розмір `Box` і не збільшує його.

</details>

<details>
<summary>113. Як обробляти кнопку “назад” у Compose?</summary>

#### Kotlin

У Compose back press обробляють через `BackHandler` з `androidx.activity.compose`. Він перехоплює system back у межах composable.

Базовий приклад:

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // content
}
```

`BackHandler` можна вмикати умовно:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Якщо `enabled = false`, back event піде далі: до іншого handler або Navigation.

Типовий сценарій — confirmation dialog для unsaved changes:

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

З `NavController` стандартний back часто працює автоматично через back stack. Custom `BackHandler` потрібен, коли стандартну поведінку треба змінити: dialog, bottom sheet, selection mode, unsaved changes.

Для локального UI state back має спочатку закривати верхній шар:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

Пріоритет такий:

```text
dialog/sheet/selection -> unsaved changes -> screen navigation
```

Для складної логіки краще передати event у ViewModel:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

ViewModel вирішує: показати dialog, зберегти draft, дозволити navigation або emit-нути effect.

Типові помилки:

- перехоплювати back завжди;
- не використовувати `enabled`;
- дублювати logic між `BackHandler` і `NavController`;
- не враховувати dialog/sheet state;
- тримати back logic далеко від state, який вона обробляє.

**Коротко:** у Compose back press обробляють через `BackHandler(enabled = ...)`. Його варто використовувати для верхніх UI-шарів і нестандартної поведінки, а звичайну навігацію часто залишати `NavController`.

</details>

<details>
<summary>114. Для чого використовується rememberUpdatedState?</summary>

#### Kotlin

`rememberUpdatedState()` дає long-lived effect актуальне value або callback без restart effect-а.

Проблема:

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` не restart-иться, якщо зміниться `onTimeout`. Coroutine може викликати stale lambda.

Рішення:

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

Timer не починається заново, але після delay викликається latest callback.

Головне правило:

```text
зміна value має restart-ити effect? -> key
зміна value не має restart-ити effect? -> rememberUpdatedState
```

Якщо зробити callback key:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

то при зміні lambda попередня coroutine скасується, а timer почнеться заново. Це не завжди бажано.

Приклад із `DisposableEffect`:

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

Observer re-register-иться тільки при зміні `owner`, але викликає актуальний handler.

Коли value має бути key:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Зміна `userId` означає нову operation, тому стару треба скасувати і запустити нову.

`rememberUpdatedState()` не запускає coroutine, не замінює effect keys і не зберігає value після виходу з composition. Він просто повертає `State<T>` з актуальним значенням для long-lived effect.

**Коротко:** `rememberUpdatedState()` потрібен, щоб effect бачив latest value/callback без restart. Якщо зміна value означає нову operation — це має бути key, а не `rememberUpdatedState`.

</details>

<details>
<summary>115. Що таке side-effects у Compose і як їх обробляти?</summary>

#### Kotlin

Side-effect у Compose — дія поза чистим рендерингом UI: coroutine, snackbar, navigation, analytics, listener, lifecycle observer, sync із зовнішнім API.

Composable body може виконуватись багато разів, тому side-effect не можна запускати напряму:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // погано
}
```

Для цього є effect APIs.

### `LaunchedEffect`

Запускає coroutine, привʼязану до composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Якщо `userId` зміниться, стара coroutine скасується і стартує нова.

### `rememberCoroutineScope`

Потрібен для suspend UI-дій з callback:

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

Підходить для snackbar, drawer, bottom sheet, scroll animation.

### `DisposableEffect`

Для listeners/observers з cleanup:

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

Якщо підписались — відписуємось у `onDispose`.

### `SideEffect`

Виконується після успішної recomposition:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Не підходить для suspend work.

### Інші API

- `rememberUpdatedState` — latest value/callback всередині long-lived effect без restart;
- `produceState` — async source у Compose `State`;
- `snapshotFlow` — Compose state у Flow.

Практична карта:

```text
composition coroutine      -> LaunchedEffect
coroutine from callback    -> rememberCoroutineScope
listener with cleanup      -> DisposableEffect
after recomposition sync   -> SideEffect
latest callback no restart -> rememberUpdatedState
async source to State      -> produceState
Compose state to Flow      -> snapshotFlow
```

**Коротко:** side-effects не запускають у body composable. Для них використовують effect APIs, щоб явно контролювати lifecycle, restart, cancellation і cleanup.

</details>

<details>
<summary>116. Що таке ключі (keys) у LaunchedEffect і чому вони важливі?</summary>

#### Kotlin

Keys у `LaunchedEffect` визначають lifecycle coroutine-effect-а: коли його запустити, скасувати й перезапустити. Якщо key змінюється, Compose скасовує стару coroutine і запускає нову.

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

Поведінка:

- composable увійшов у composition — effect стартує;
- recomposition без зміни `userId` — effect не restart-иться;
- `userId` змінився — стара coroutine cancel, нова стартує;
- composable вийшов із composition — coroutine cancel.

Типова помилка:

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Якщо `userId` зміниться, effect не перезапуститься і можна отримати stale data. Правильно:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` нормальний, коли effect має стартувати один раз для входу в composition:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigation/snackbar
    }
}
```

Кілька keys:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

Effect перезапуститься, якщо зміниться хоча б один key. Усе, що визначає результат side effect-а, має бути key.

Не використовувати нестабільні keys:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Так effect буде restart-итись неконтрольовано.

Якщо потрібне latest value всередині effect, але без restart-а, використовують `rememberUpdatedState`:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Практичне правило:

```text
зміна value має restart-ити effect -> key
потрібне latest value без restart   -> rememberUpdatedState
```

**Коротко:** keys у `LaunchedEffect` керують запуском, скасуванням і перезапуском coroutine. Неправильні keys дають stale data, зайві restart-и або нестабільну поведінку UI.

</details>

<details>
<summary>117. Що таке snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` — Compose API, який перетворює reads із Compose snapshot state у cold `Flow`. Його використовують, коли на зміну Compose state треба реагувати через Flow operators або side-effect pipeline.

Базовий приклад:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` відстежує Compose state reads усередині lambda. Коли прочитане значення змінюється, Flow emit-ить нове значення.

Типові сценарії:

- scroll analytics;
- pagination trigger;
- debounce/sample для UI state;
- logging/debugging;
- side effects, які не можна запускати в composable body.

Pagination приклад:

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

Оскільки результат — `Flow`, можна використовувати operators:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

`snapshotFlow` зазвичай collect-ять у `LaunchedEffect`. Не треба запускати collection напряму в composable body.

Не використовувати `snapshotFlow` для звичайного UI rendering:

```kotlin
Text(text = state.title)
```

Якщо треба просто показати state — читаємо state напряму. `snapshotFlow` потрібен саме для переходу з Compose state у Flow/side-effect logic.

Важливий нюанс: у lambda має бути Compose snapshot state read. Якщо його немає, `snapshotFlow` не має що відстежувати.

**Коротко:** `snapshotFlow` конвертує Compose state reads у `Flow`. Він корисний для analytics, pagination, debounce і side effects у `LaunchedEffect`, але не потрібен для простого рендерингу UI.

</details>

<details>
<summary>118. Як оптимізувати recomposition у Compose?</summary>

#### Kotlin

Оптимізація recomposition — це не “прибрати всі recompositions”, а зробити їх дешевими й локальними. Recomposition нормальна; проблема — важка робота або занадто широка область оновлення.

### Читати state ближче до використання

Погано:

```kotlin
ProfileScreen(state = hugeState)
```

Краще:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Якщо composable читає state, він може recomposition-итись при зміні цього state. Не передавати весь screen state туди, де потрібне одне поле.

### Lazy lists: keys і contentType

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

Stable keys допомагають Compose не плутати items і зберігати item state.

Для різних типів елементів:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### Не робити важку роботу в composable

Погано:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Краще:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

А ще краще — підготувати formatted text у mapper/ViewModel, якщо це частина UI model.

### Використовувати `derivedStateOf` для частих змін

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Корисно, коли source state змінюється часто, але UI має реагувати тільки на derived condition.

### Stable/immutable models

Compose краще працює з immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Mutable collections і нестабільні objects можуть заважати Compose пропускати непотрібні recompositions.

### Side effects — тільки через effect APIs

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

Composable body може виконуватись багато разів, тому analytics, navigation, listeners і suspend work не мають бути напряму в body.

### Перевіряти профілюванням

- Layout Inspector recomposition counts;
- Compose compiler metrics;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

**Коротко:** оптимізація recomposition — це локальні state reads, менші composables, stable keys, `contentType`, immutable UI models, `remember`, `derivedStateOf`, правильні effects і профілювання. Ціль — дешеві локальні оновлення, а не нуль recompositions.

</details>

<details>
<summary>119. Як дебажити проблеми з recomposition?</summary>

#### Kotlin

Проблеми з recomposition дебажать через вимірювання, а не здогадки. Ціль — знайти, який state часто змінюється і яку частину UI він зачіпає.

### Layout Inspector

Android Studio Layout Inspector показує recomposition counts для composables. Але кількість сама по собі не проблема: легкий composable може recomposition-итись часто без видимого impact.

### Тимчасові logs

Для швидкої перевірки можна додати log:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Або через `SideEffect`:

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

Це тільки debug-інструмент, не production код.

### Перевірити state reads

Часта причина — composable читає занадто широкий state:

```kotlin
UserRow(screenState = state)
```

Краще:

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

State треба читати ближче до місця використання.

### Перевірити lists і parameters

Для `LazyColumn` мають бути stable keys:

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

Також перевірити unstable parameters: mutable collections, objects/lambdas, які створюються заново без потреби, або важка робота в composable body.

### Compiler metrics і profiler

Compose compiler metrics допомагають побачити, які composables/types unstable, restartable або skippable. Android Studio Profiler і Macrobenchmark потрібні, щоб підтвердити реальний performance impact.

Практичний checklist:

- подивитись counts у Layout Inspector;
- знайти state, який часто змінюється;
- звузити state reads;
- перевірити stable keys у lazy lists;
- прибрати важку роботу з body;
- перевірити unstable parameters;
- застосувати `remember`/`derivedStateOf` тільки там, де є причина;
- підтвердити результат profiler/metrics.

**Коротко:** recomposition дебажать через Layout Inspector, logs, compiler metrics і profiler. Найчастіші причини — широкий state read, unstable parameters, відсутні keys, важка робота в body або неправильний state ownership.

</details>

<details>
<summary>120. Які є способи уникнути лагів у Compose, наприклад під час скролу?</summary>

#### Kotlin

Лаги під час скролу найчастіше спричиняють важка робота на main thread, зайві recompositions, allocations у items, нестабільні keys або надто складний UI рядка.

### Stable keys і contentType

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

`key` зберігає identity item після insert/delete/reorder.

Для різних типів елементів:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` допомагає Compose перевикористовувати composition сумісних items.

### Не робити важку роботу в item

Погано:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Краще підготувати formatted text у mapper/ViewModel або хоча б кешувати object через `remember`. I/O, bitmap decoding, sorting/filtering і важкі обчислення не мають виконуватись у composable.

### Звузити state reads

Item має отримувати тільки власні дані:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Не передавати весь screen state у кожен рядок.

### Stable UI models

Використовувати immutable UI models і не створювати нові collections/wrappers при кожній recomposition без потреби.

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

`derivedStateOf` корисний, коли scroll змінюється часто, а UI реагує тільки на похідну умову.

### Images і paging

Для images використовувати loader з memory/disk cache і правильним target size. Bitmap не декодувати вручну в composable або на main thread.

Для великих списків використовувати Paging:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Профілювання

Підтверджувати оптимізації через:

- Layout Inspector recomposition counts;
- Android Studio Profiler/System Trace;
- Macrobenchmark у release-like збірці;
- Baseline Profiles.

**Коротко:** smooth scroll у Compose потребує stable keys, `contentType`, вузьких state reads, дешевих items, immutable models, cached images, paging і перевірки profiler-ом. Головне — не блокувати main thread і не робити важку роботу під час composition.

</details>

<details>
<summary>121. Чим відрізняються анотації Stable та Immutable?</summary>

#### Kotlin

`@Stable` і `@Immutable` у Compose — це contract для compiler/runtime, який допомагає вирішувати, чи можна skip-ати recomposition. Це не “магічна оптимізація”, а обіцянка, що тип поводиться певним чином.

Головна різниця:

```text
@Immutable -> object не змінюється після створення
@Stable    -> object може змінюватися, але Compose бачить ці зміни
```

### `@Immutable`

Підходить для value/UI models:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Умова: усі public properties не змінюються після створення, а поля теж immutable/stable.

Погано:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

`MutableList` можна змінити inplace, а Compose може не побачити зміну.

Краще:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Для суворішої гарантії можна використовувати immutable collections.

### `@Stable`

Підходить для state holders або controller objects, де зміни проходять через observable Compose state:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Mutable object допустимий, бо Compose бачить зміну `count`.

Погано:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Звичайне mutable поле не повідомляє Compose про зміну.

Коли що використовувати:

- `@Immutable` — UI models, value objects, screen state data classes;
- `@Stable` — custom state holders із `mutableStateOf`, controllers, objects із контрольованою observable мутацією.

Типові помилки:

- анотувати mutable object як `@Immutable`;
- ставити `@Stable` на клас зі звичайними mutable fields;
- тримати `MutableList/MutableMap` у UI state;
- мутувати старий state замість створення нового;
- використовувати анотації як “ліки” від поганого state ownership.

**Коротко:** `@Immutable` означає “не змінюється після створення”. `@Stable` означає “може змінюватися, але Compose коректно бачить зміни”. Анотацію ставлять тільки якщо тип реально виконує цей contract.

</details>

<details>
<summary>122. Навіщо передавати key у LazyColumn і що він собою являє під капотом?</summary>

#### Kotlin

`key` у `LazyColumn` задає стабільну identity item-а незалежно від його position.

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

Без key Compose використовує position. Це нормально для статичного списку, але ламається при insert/remove/reorder.

```text
Було:       Після insert:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

За position Compose бачить новий content у старих slots. За `user.id` він розуміє, що старі items просто змінили position.

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

Без stable key `expanded` може залишитись на position і перейти до іншого user. Key переносить remembered/saveable state разом з item identity.

### Під капотом

Key стає identity composition group. Runtime використовує його, щоб:

- зіставити стару й нову group;
- перемістити/reuse group;
- зберегти `remember` state;
- відновити `rememberSaveable` state;
- коректно працювати з item animations.

Це не `DiffUtil`: key потрібен для composition/placement bookkeeping у lazy layout.

### Вимоги до key

Key має бути:

- unique серед sibling items;
- stable між recompositions;
- повʼязаний із domain identity;
- saveable, якщо item має `rememberSaveable`.

Добре:

```kotlin
key = { it.id }
```

Погано:

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Random/time створює нову identity на кожну recomposition. Index дорівнює default position identity і годиться тільки для справді статичних списків.

Composite key допустимий, якщо стабільний:

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

Для Paging використовують item key API:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**Коротко:** `key` привʼязує lazy item до стабільної identity, а не до index. Він потрібен для dynamic lists, local item state, reorder, animations і Paging. Найкращий key — стабільний server/database/client-generated ID.

</details>

<details>
<summary>123. Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?</summary>

#### Kotlin

У `LazyColumn.items()` важливі параметри — `key` і `contentType`.

Базово:

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Для dynamic list краще явно передати `key`:

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

`key` задає стабільну identity item-а. Він потрібен, щоб Compose не плутав remembered state після insert/delete/reorder.

Хороший key:

- унікальний;
- стабільний;
- не залежить від position;
- не random/time-based;
- базується на domain id.

`contentType` описує тип UI item-а:

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

Наприклад: `header`, `post`, `ad`, `loader`. Це допомагає Compose ефективніше reuse-ити composition для однотипних елементів у mixed list.

Приклад із sealed items:

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

Якщо потрібен index, є `itemsIndexed`:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

Index не варто використовувати як key для mutable/reorderable списків.

Коли `contentType` не критичний: якщо всі items мають однаковий UI shape. Найбільша користь — у mixed feeds, chats, catalogs.

**Коротко:** `key` — це stable identity item-а. `contentType` — тип/форма UI item-а для кращого reuse composition у mixed lists. Для production dynamic lists `key` майже завжди потрібен, `contentType` — коли є різні типи рядків.

</details>

<details>
<summary>124. Що таке SOLID?</summary>

#### Kotlin

`SOLID` — пʼять принципів ООП-дизайну, які зменшують coupling, покращують testability і роблять зміни локальнішими.

### S — Single Responsibility Principle

Клас має мати одну причину для зміни.

Погано, коли `ViewModel` робить network, DB, mapping і UI state. Краще розділити:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

`ViewModel` керує state, use case — бізнес-дією, repository — даними.

### O — Open/Closed Principle

Код відкритий для розширення, але не потребує постійного редагування.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Нову реалізацію можна додати без переписування клієнтського коду.

### L — Liskov Substitution Principle

Підтип має коректно замінювати базовий тип.

Погано:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Якщо реалізація не виконує контракт, контракт занадто широкий або неправильний.

### I — Interface Segregation Principle

Клієнт не має залежати від методів, які йому не потрібні.

Погано:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Краще розділити:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D — Dependency Inversion Principle

Високорівневий код залежить від abstractions, а не concrete implementations.

Погано:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Краще:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Так use case легко тестувати через fake repository.

В Android це зазвичай виглядає так:

```text
UI -> ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

Практичне правило: SOLID не означає interface для кожного class. Абстракція потрібна там, де є реальний boundary, alternative implementation або test seam.

**Коротко:** SOLID допомагає не змішувати UI, business logic і data layer, уникати god classes, тримати contracts вузькими та робити код тестованим. Але надмірні абстракції без потреби теж шкодять.

</details>

<details>
<summary>125. Поясніть кожен принцип SOLID з прикладами з Android.</summary>

#### Kotlin

SOLID допомагає розділяти відповідальності, тримати contracts чистими й тестувати бізнес-логіку без Android framework, Retrofit або Room.

### S — Single Responsibility

Клас має одну причину для зміни.

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

`ViewModel` керує UI state, use case — бізнес-операцією, repository — даними. SRP не означає “один метод на клас”, а не дозволяє змішувати різні причини для зміни.

### O — Open/Closed

Код розширюється новими реалізаціями, а не постійним редагуванням existing logic.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Новий handler додається окремим класом. Але abstraction потрібна лише там, де є реальна варіативність.

### L — Liskov Substitution

Підтип має коректно замінювати базовий тип.

Погано, коли `ReadOnlyStorage` реалізує `save()` і кидає `UnsupportedOperationException`. Краще розділити contract:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP — це не тільки signature, а й guarantees, errors, side effects та invariants.

### I — Interface Segregation

Caller не має залежати від методів, які йому не потрібні.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Use case отримує вузький contract замість god-interface. Але надто дрібні interfaces без ролі caller-а — теж boilerplate.

### D — Dependency Inversion

High-level business logic залежить від abstraction, а data layer її реалізує.

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

Use case не створює Retrofit/Room напряму. DI підставляє real implementation або fake.

```text
UI -> ViewModel -> UseCase -> Repository contract
                              ^
                              |
                     Data implementation -> API/DB
```

Типові порушення в Android:

- Fragment напряму викликає API/DAO;
- ViewModel містить network, mapping, cache і navigation;
- один repository відповідає за весь app;
- interface створюється для кожного class без boundary;
- domain залежить від Android/infrastructure types;
- subtype формально реалізує interface, але ламає contract.

**Коротко:** SRP розділяє відповідальності, OCP — точки розширення, LSP — коректну заміну підтипів, ISP — вузькі contracts, DIP — залежність business logic від abstractions. У Android це відділяє UI/domain від framework і data details.

</details>

<details>
<summary>126. Що таке ООП?</summary>

#### Kotlin

ООП — підхід, де система моделюється обʼєктами, які поєднують state і behavior. Клас описує тип, object — його instance.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Основні принципи: encapsulation, abstraction, inheritance, polymorphism.

### Encapsulation

Клас ховає mutable state і змінює його через контрольовані methods:

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

Так клас захищає invariants.

### Abstraction

Код залежить від contract, а не implementation:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

`ViewModel` не знає, чи дані прийшли з Retrofit, Room або fake.

### Inheritance

У Kotlin класи `final` за замовчуванням. Наслідування дозволяють через `open`:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Inheritance моделює `is-a`, але часто створює сильний coupling. Композиція зазвичай безпечніша.

### Polymorphism

Різні implementations можна підставляти через спільний contract:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

Caller працює з `ImageLoader`, тому production і test implementations взаємозамінні.

У Kotlin ООП добре поєднується з `data class`, `sealed class/interface`, extension functions, immutability і constructor injection. Важлива не кількість classes/interfaces, а чіткі responsibilities і boundaries.

**Коротко:** ООП організовує state і behavior в objects. У Kotlin головне — encapsulation, contracts, polymorphism і обережне inheritance; часто краще immutable models та composition замість глибокої ієрархії.

</details>

<details>
<summary>127. Що таке абстракція?</summary>

#### Kotlin

Абстракція — це приховування деталей реалізації за зрозумілим contract-ом. Вона описує “що робить код”, а не “як саме”.

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Код, який використовує `UserRepository`, не знає, чи дані приходять з API, Room, cache або fake object у тесті.

Use case залежить від contract:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Implementation ховає деталі:

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

Для тестів можна підставити fake:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

В Android типова схема:

```text
ViewModel -> UseCase -> Repository contract -> Repository implementation -> API/DB
```

UI не має знати про Retrofit, Room або cache strategy.

Абстракція не обовʼязково означає interface. Це може бути також abstract class, sealed hierarchy, function type або просто public API класу. Але abstract/base classes в Android часто створюють жорстку ієрархію, тому composition зазвичай безпечніша.

Абстракція vs інкапсуляція:

```text
Абстракція   -> показує важливий contract
Інкапсуляція -> ховає internal state/details
```

Погана абстракція — interface “про всяк випадок”, без реальної альтернативної implementation або boundary:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

Якщо є одна проста реалізація і немає test seam/boundary, interface може бути зайвим.

Хороша абстракція:

- має чіткий contract;
- приховує нестабільні деталі;
- не протікає implementation details;
- спрощує тестування;
- відповідає реальній точці зміни;
- не додає boilerplate без користі.

**Коротко:** абстракція відділяє contract від implementation. В Android вона корисна на boundaries: network, database, analytics, time, dispatchers, repositories. Не треба абстрагувати кожен клас без реальної причини.

</details>

<details>
<summary>128. У чому різниця між конструкторами та методами?</summary>

#### Kotlin

Constructor створює instance і приводить object у початковий валідний state. Method описує поведінку вже створеного object і може викликатися багато разів.

Primary constructor:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Параметри з `val`/`var` стають properties. Constructor не має return type і викликається через `User(...)`.

Method:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

Метод викликається на instance:

```kotlin
user.displayName()
```

Для validation або initialization logic використовують `init`:

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

Secondary constructor делегує primary через `this(...)`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

У Kotlin часто краще default parameter:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Constructor часто використовують для DI:

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

Constructor задає required dependencies, method виконує operation.

Що не робити в constructor/init:

- network request;
- database query;
- coroutine launch;
- `runBlocking`;
- важкий I/O;
- Android lifecycle-dependent logic.

Погано:

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Краще:

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**Коротко:** constructor створює валідний object і приймає required state/dependencies. Method виконує поведінку після construction. Heavy, suspend і lifecycle-bound work не запускають у constructor.

</details>

<details>
<summary>129. Що таке сигнатура методу?</summary>

#### Kotlin

Сигнатура методу — це ознаки, за якими compiler відрізняє одну function від іншої. У Kotlin це насамперед name, receiver і parameter types/order.

```kotlin
fun loadUser(id: String): User
```

Спрощено сигнатура: `loadUser(String)`.

### Overloading

Overloads мають відрізнятися parameter list:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Return type недостатній:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflict
```

Parameter name теж недостатній:

```kotlin
fun load(id: String)
fun load(userId: String) // conflict
```

Named arguments покращують читабельність call-site, але не створюють нову signature.

Порядок типів має значення:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

Це валідно, але часто поганий API через низьку читабельність.

### Default parameters

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

Це одна Kotlin function. Для Java overloads можна використати `@JvmOverloads`, але вручну плодити overloads поруч треба обережно.

### Override

Override має відповідати contract-у base type:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Зміна parameter type — це вже інша function, не override. Return type може бути covariant subtype.

### Receiver і extensions

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

Extension receiver бере участь у Kotlin resolution. Але extension function не є virtual override.

### JVM нюанси

Через type erasure можуть бути clashes:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // JVM clash: process(List)
```

Nullability теж не створює окрему JVM signature:

```kotlin
fun load(id: String)
fun load(id: String?) // JVM clash
```

Рішення — інша назва або `@JvmName`, якщо це справді потрібно.

**Коротко:** у Kotlin functions розрізняються за name, receiver і parameter types/order. Return type і parameter names не достатні. На JVM додатково треба памʼятати про type erasure, nullability і interop нюанси.

</details>

<details>
<summary>130. У чому різниця між перевантаженням (overloading) та перевизначенням (overriding)?</summary>

#### Kotlin

```text
Overloading -> одна назва, різні parameter lists
Overriding  -> та сама сигнатура, нова реалізація в subtype
```

### Overloading

Кілька functions в одному scope мають одну назву, але різні параметри:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

Compiler вибирає overload за static types аргументів. Return type не може сам відрізняти overload:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // compile error
```

У Kotlin частину overload-ів краще замінювати default parameters і named arguments:

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

Subtype реалізує або змінює inherited method із тією самою сигнатурою:

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

Зміна parameter type створить іншу function, а не override.

У Kotlin classes і methods `final` за замовчуванням. Для override member має бути `open`, `abstract` або member interface:

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

Важливі нюанси:

- overload-и з default arguments можуть бути ambiguous;
- override має зберігати contract базового типу;
- return type в override може бути covariant;
- extension functions не override-яться, вони resolve-яться статично;
- `final override` забороняє подальший override.

**Коротко:** overloading — кілька functions з однією назвою й різними параметрами, вибір на compile time. Overriding — заміна inherited реалізації з тією самою сигнатурою, вибір на runtime через polymorphism.

</details>

<details>
<summary>131. У чому різниця між інтерфейсом та абстрактним класом?</summary>

#### Kotlin

Interface описує contract/роль. Abstract class описує спільну базу з частковою реалізацією, constructor-ом і state.

```text
interface      -> contract, багато ролей
abstract class -> base implementation, один parent class
```

### Interface

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Клас може реалізувати багато interfaces:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Interface добре підходить для DI, тестів і dependency inversion:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

У Kotlin interface може мати default method:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Але interface не має constructor-а і не підходить для shared mutable state.

### Abstract class

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // shared logic
    }

    abstract suspend fun refresh()
}
```

Абстрактний клас може мати:

- constructor;
- state;
- protected methods;
- часткову реалізацію;
- template method pattern.

Але клас може наслідувати тільки один class:

```kotlin
class MyRepository : BaseRepository()
```

### Коли що використовувати

Interface:

- потрібен contract;
- є кілька implementations;
- потрібен fake/mock у тестах;
- class має кілька ролей;
- dependency має бути абстрагована.

Abstract class:

- потрібна shared implementation;
- потрібен protected state;
- є base lifecycle/template;
- constructor частина base contract.

В Android треба бути обережним із `BaseActivity`, `BaseFragment`, `BaseViewModel`: вони швидко стають god-base-class і створюють жорстку ієрархію.

Часто краще composition:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**Коротко:** interface — для контрактів, DI, тестів і кількох ролей. Abstract class — для спільної реалізації та state, але з ризиком жорсткого inheritance. У modern Android частіше краще interface + composition, ніж глибокі base classes.

</details>

<details>
<summary>132. Чи може інтерфейс не містити методів?</summary>

#### Kotlin

Так. Порожній interface називають marker interface. Він не задає поведінку, а позначає тип певною роллю в type system.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Тепер `UserProfile` можна приймати як `Cacheable`, перевіряти через `is Cacheable` або використовувати як generic bound.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // only explicitly marked types
}
```

Якщо для `sync` потрібен `id` або поведінка, marker вже недостатній — треба додати property/method у contract.

### Sealed marker

Порожній `sealed interface` часто використовують як root для закритого набору states/events/effects:

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

Тут це не просто marker, а sum type: значення може бути одним із визначених variants, а compiler перевіряє exhaustive `when`.

Класичний приклад marker interface — `java.io.Serializable`: methods немає, але runtime перевіряє, чи object реалізує цей type.

### Interface чи annotation

Marker interface краще, коли потрібні:

- subtype relation;
- generic bounds;
- polymorphic API;
- sealed hierarchy;
- `is` check без reflection.

Annotation краще для metadata:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Її обробляє compiler plugin, code generator або runtime reflection. Вона не створює subtype relation.

### Коли marker поганий

Не варто створювати marker “на майбутнє”:

```kotlin
interface Important
interface Special
```

Погані ознаки:

- marker ніде не використовується як type/bound/root;
- логіка має багато `is Marker`;
- потрібні data/methods, але код робить casts;
- роль краще виразити enum/property;
- metadata краще описується annotation.

**Коротко:** interface може бути порожнім. Marker interface корисний для subtype relation, generic bounds або sealed hierarchies. Якщо потрібна metadata — annotation; якщо потрібна поведінка — interface має містити members.

</details>

<details>
<summary>133. Чи може інтерфейс містити інший інтерфейс?</summary>

#### Kotlin

Так. Інтерфейс у Kotlin може містити інший інтерфейс. Це nested interface.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Використання:

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // handle change
    }
}
```

Nested interface звертається через `Outer.Inner`, наприклад `UserRepository.Listener`.

Коли це доречно:

- contract тісно повʼязаний із зовнішнім API;
- вкладений тип використовується тільки в цьому контексті;
- це callback або small nested contract;
- top-level тип лише засмічував би package.

Приклад callback API:

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

Nested interface не є `inner` і не має доступу до state instance зовнішнього типу:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` не може напряму читати `id`.

Не плутати з interface inheritance:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

Це не вкладеність, а наслідування contracts.

Не варто вкладати interface, якщо він самостійний і використовується по всьому проєкту:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

Так зовнішній interface стає штучним namespace. Краще top-level contracts.

**Коротко:** interface може містити nested interface. Це корисно для callback-ів або contracts, які існують тільки в контексті зовнішнього API. Самостійні abstractions краще робити top-level.

</details>

<details>
<summary>134. Чому не варто додавати в інтерфейс десятки методів?</summary>

#### Kotlin

Не варто додавати в інтерфейс десятки методів, бо це порушує Interface Segregation Principle: клієнти починають залежати від методів, які їм не потрібні. Такий інтерфейс стає важким для реалізації, тестування і підтримки.

1. **Проблема великого інтерфейсу**

Погано:

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

Якщо use case потребує тільки `getUser`, він все одно залежить від усього контракту.

2. **Складні реалізації**

Кожна implementation має реалізувати всі методи, навіть якщо частина не потрібна:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Це ознака неправильного контракту.

3. **Складні тести**

У тестах fake/mock теж має знати про всі методи:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // зайвий шум
}
```

Чим ширший інтерфейс, тим більше boilerplate в тестах.

4. **Краще розділити контракти**

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

Тепер кожен use case залежить тільки від того, що реально використовує.

5. **Приклад use case**

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

Use case не знає про update/delete/upload, бо вони йому не потрібні.

6. **God interface**

Великий interface часто стає “god interface” — централізованим контрактом для всього модуля. Це збільшує coupling: будь-яка зміна в одному методі може зачепити багато класів.

7. **Коли великий інтерфейс допустимий**

Іноді широкий API виправданий, якщо це справді один cohesive abstraction. Наприклад, DAO для конкретної таблиці може мати кілька related queries. Але якщо методи належать різним use cases або reasons to change — краще розділити.

8. **Практичне правило**

Інтерфейс має описувати роль, а не “все, що вміє система”. Якщо клієнт використовує 1–2 методи з 20, інтерфейс занадто широкий.

**Коротко:** великі інтерфейси створюють зайві залежності, складні fake/mock реалізації, порушують ISP і часто перетворюються на god interfaces. Краще розділяти контракти за ролями: reader, writer, uploader, deleter — щоб кожен клієнт залежав тільки від потрібних методів.

</details>
<details>
<summary>135. Що таке композиція і чим вона відрізняється від наслідування?</summary>

#### Kotlin

`Composition` — це підхід, коли клас отримує поведінку через залежності. `Inheritance` — це підхід, коли клас успадковує поведінку від базового класу.

Головна різниця:

```text
Inheritance -> is-a
Composition -> has-a / uses-a
```

1. **Наслідування**

Наслідування доречне, коли дочірній клас справді є спеціалізацією базового типу:

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

`Dog` є `Animal`, тому `is-a` звʼязок тут логічний.

2. **Композиція**

Композиція означає, що клас не успадковує поведінку, а використовує окремі обʼєкти:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

`Dog` має `SoundPlayer`, але не є `SoundPlayer`.

3. **Чому композиція часто краща**

Композиція робить залежності явними:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

З такого конструктора одразу видно, що потрібно класу. Це легше тестувати, бо залежності можна замінити fake/mock реалізаціями.

4. **Проблема великих base classes**

Поганий сигнал — коли зʼявляється великий `BaseViewModel`, `BaseRepository` або `BaseFragment`, який містить багато helper-ів:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Такі класи швидко стають `god object`. Дочірні класи отримують методи й стан, які їм не потрібні, а зміна базового класу може зламати багато місць.

5. **Kotlin delegation**

Kotlin підтримує delegation як зручну форму композиції:

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

`UserService` делегує реалізацію `Logger` окремому обʼєкту, не наслідуючи його.

6. **Коли що використовувати**

Наслідування підходить, якщо:

- є реальний `is-a` звʼязок;
- базовий клас стабільний;
- subclass не порушує контракт parent-а.

Композиція краща, якщо:

- треба додати поведінку;
- залежність треба замінювати в тестах;
- є кілька незалежних capabilities;
- не хочеться будувати крихку ієрархію.

7. **Практичне правило**

В Android/Kotlin для `ViewModel`, `Repository`, `UseCase`, mapper-ів і service-класів зазвичай краще композиція. Наслідування варто залишати для стабільних framework-типів або очевидних доменних `is-a` моделей.

**Коротко:** наслідування — це `is-a`, композиція — це `has-a` або `uses-a`. У Kotlin/Android композиція зазвичай практичніша: вона робить залежності явними, спрощує тести й не створює крихких base-class ієрархій.

</details>
<details>
<summary>136. Які є модифікатори доступу в Kotlin?</summary>

#### Kotlin

Kotlin має чотири visibility modifiers:

```text
public    -> доступний звідусіль
internal  -> доступний у межах module
protected -> доступний у класі та subclasses
private   -> доступний у поточному scope або файлі
```

### public

`public` — visibility за замовчуванням:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Public declaration формує зовнішній API, тому implementation details без потреби не відкривають.

### private

Private member доступний лише всередині class/object:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Top-level `private` доступний у поточному Kotlin-файлі:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### protected

`protected` доступний у class та subclasses:

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}

class LoginViewModel : BaseViewModel() {
    fun login() {
        handleError(IllegalStateException())
    }
}
```

На відміну від Java, Kotlin `protected` не дає package access і не дозволений top-level. Це частина inheritance API.

### internal

`internal` обмежує доступ Kotlin module-ом:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Module — compilation boundary, наприклад Gradle module, а не package.

У multi-module Android public API може містити лише entry point feature, а її screens, ViewModels та реалізації залишатися `internal`.

`internal` — compile-time API boundary, а не security-механізм: Java або reflection можуть бачити bytecode declarations.

### Setter і constructor

Property може бути public для читання та private для запису:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set

    fun login() {
        isLoggedIn = true
    }
}
```

Constructor також має окрему visibility:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Клас public, але constructor доступний лише в module. Override не можна зробити менш видимим за base member; public API також не може розкривати менш видимий тип у signature.

### Практичні правила

- починати з найвужчої достатньої visibility;
- implementation залишати `private` або `internal`;
- public API робити мінімальним і стабільним;
- `protected` використовувати лише для inheritance contract;
- не сприймати visibility як security boundary.

**Коротко:** `public` відкриває API всім, `internal` — module, `protected` — class hierarchy, `private` — class scope або файл. Варто обирати найвужчу достатню visibility.

</details>
<details>
<summary>137. Що таке singleton?</summary>

#### Kotlin

Singleton — один instance у межах owner-а: classloader/process або DI component. Android process може бути знищений, а multiprocess app матиме окремий instance у кожному process.

### Kotlin object

```kotlin
object AppLogger {
    fun log(message: String) =
        println(message)
}
```

`object` thread-safe ініціалізується JVM при першому використанні. Але mutable state усередині не стає thread-safe автоматично:

```kotlin
object Counter {
    private val value = AtomicInteger()

    fun increment(): Int =
        value.incrementAndGet()
}
```

### Companion object

```kotlin
class UserId private constructor(
    val value: String
) {
    companion object {
        fun from(raw: String): UserId =
            UserId(raw.trim())
    }
}
```

Companion — singleton, прив'язаний до класу. Сам `UserId` може мати багато instances. Companion використовують для factory methods і constants.

### Singleton через DI

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext
    private val context: Context
)
```

У Hilt це один instance на `SingletonComponent`. DI краще за прямий global object, коли є dependencies:

- constructor dependencies явні;
- implementation можна замінити fake;
- lifetime визначає component;
- caller залежить від interface.

Для коротшого lifecycle використовують `@ViewModelScoped`, `@ActivityScoped` тощо. Scope має відповідати owner-у state/resource.

### Ризики

Погано:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Це створює:

- Activity leak;
- global mutable state;
- race conditions;
- залежність tests від порядку;
- нечіткий logout/reset;
- втрату state при process death.

App-scoped object не повинен утримувати `Activity`, `Fragment`, `View` або `ActivityContext`. Якщо Context справді потрібен, використовують application context.

Token/session не можна зберігати лише в memory singleton: потрібні безпечне persistence, відновлення та explicit clear.

### Коли доречний

- Room database, OkHttp client;
- logger/analytics/configuration;
- stateless app-wide service;
- immutable registry;
- coordinator з application lifecycle.

Не доречний:

- screen state або ViewModel;
- object із різними parameters;
- user state без reset;
- dependency з коротким lifecycle;
- utility, яку простіше зробити top-level function.

### Тестованість

Lifetime не повинен бути зашитий у caller:

```kotlin
class LoginUseCase(
    private val analytics: AnalyticsTracker
)
```

DI вирішує, чи concrete implementation singleton. Caller лише отримує contract, тому його можна тестувати з fake.

### Практичні правила

- явно визначити owner singleton-а;
- не зберігати UI references;
- синхронізувати mutable state;
- враховувати process death;
- app-wide dependencies scope-ити через DI;
- не використовувати singleton як service locator;
- short-lived state тримати в short-lived scope.

**Коротко:** Kotlin `object` створює singleton на рівні classloader, Hilt `@Singleton` — на рівні component. Він підходить для app-wide services, але global mutable state, UI references і припущення про переживання process death є помилками.

</details>
<details>
<summary>138. Що таке generics?</summary>

#### Kotlin

Generics параметризують клас, interface або функцію типом. Це дозволяє повторно використовувати реалізацію, зберігаючи compile-time type safety:

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` — type parameter, а `String` і `Int` — type arguments. Compiler знає точний тип `value` і не потребує unsafe cast.

### Generic functions та bounds

```kotlin
fun <T> singleItemList(item: T): List<T> =
    listOf(item)
```

Type parameter можна обмежити upper bound:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Кілька bounds задаються через `where`:

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable = Unit
```

### Generic models

Типовий result type:

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val cause: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

`Nothing` є subtype усіх Kotlin-типів, тому `Error` і `Loading` можна використовувати як `Result<User>`, `Result<List<Post>>` тощо.

Generics також лежать в основі `List<T>`, `Flow<T>`, serializers, adapters і repository contracts.

### Variance

За замовчуванням generic type invariant: навіть якщо `Cat : Animal`, `MutableList<Cat>` не є `MutableList<Animal>`, бо через останній reference можна було б додати `Dog`.

`out T` означає covariance — type виробляє `T`:

```kotlin
interface Producer<out T> {
    fun produce(): T
}

val cats: Producer<Cat> = TODO()
val animals: Producer<Animal> = cats
```

`in T` означає contravariance — type споживає `T`:

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}

val animalConsumer: Consumer<Animal> = TODO()
val catConsumer: Consumer<Cat> = animalConsumer
```

Ментальна модель: producer — `out`, consumer — `in`. Якщо type одночасно приймає та повертає `T`, він зазвичай invariant.

Use-site projections дозволяють обмежити конкретне використання: `List<out Animal>`, `Comparator<in Cat>`. Star projection `List<*>` означає список невідомого типу з безпечним читанням як `Any?`.

### Type erasure та reified

На JVM більшість generic type arguments стираються в runtime:

```kotlin
// value is List<String> // impossible direct runtime check
value is List<*>
```

Inline-функція може зберегти type argument через `reified`:

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` дозволяє використовувати `T::class`, `is T` та API, яким потрібен runtime type token. Він можливий лише для inline functions.

### Практичні правила

- generic API має виражати реальну спільну операцію;
- bounds фіксують мінімальний contract;
- variance задає безпечні subtype-відносини;
- mutable containers переважно invariant;
- для runtime generic metadata потрібен `reified` або явний type token;
- надмірна параметризація погіршує читабельність.

**Коротко:** generics дозволяють одному API працювати з різними типами без unsafe casts. Bounds обмежують допустимі типи, `out/in` керують variance, а `inline reified` дає доступ до type parameter у runtime попри JVM type erasure.

</details>
<details>
<summary>139. Що таке type erasure і чому він виникає?</summary>

#### Kotlin

Type erasure означає, що JVM object зазвичай не зберігає concrete generic arguments. `List<String>` і `List<Int>` мають runtime class `List`; різницю перевіряє compiler у source code.

### Чому виникає

Java generics проєктували для backward compatibility зі старим bytecode. Type parameter стирається до upper bound або `Object`.

Kotlin/JVM дотримується тієї самої JVM model.

Class-file signature може містити metadata declaration, але runtime object не несе повного concrete generic type.

### Runtime checks

Не можна перевірити:

```kotlin
if (value is List<String>) { } // erased type
```

Можна перевірити лише container:

```kotlin
if (value is List<*>) {
    // element type is unknown
}
```

`List<*>` дозволяє безпечно читати `Any?`, але не додавати довільні values.

Cast теж не перевіряє всі elements:

```kotlin
val strings = value as List<String> // unchecked
```

Для validation перевіряють кожен element:

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String
            ?: error("Expected String")
    }
```

`filterIsInstance<String>()` лише відкидає invalid elements і не валідовує весь list.

### Reified

Звичайний `T` недоступний для `is T`. Inline function може мати `reified` parameter:

```kotlin
inline fun <reified T> isType(
    value: Any
): Boolean = value is T
```

Compiler спеціалізує call site, тому доступні `T::class`, `is T` та class-token APIs.

`reified` не відновлює nested arguments: `List<String>` усе одно потребує перевірки elements або serializer-а.

### Type tokens і serializers

Без reified type передають явно:

```kotlin
fun <T> decode(
    json: String,
    clazz: Class<T>
): T = TODO()

val user = decode(json, User::class.java)
```

Для `List<User>` одного `Class<List>` недостатньо. Libraries використовують `Type`, `KType`, generated adapter або `KSerializer<T>`.

### JVM signature clash

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {}
```

Після erasure обидві signatures стають `handle(List)`, тому виникає platform clash. Потрібні різні names або обережний `@JvmName`.

### Практичні правила

- container check — `List<*>`;
- unchecked cast не є validation;
- runtime `T` — `inline reified`;
- nested generic — serializer/type token;
- overloads мають відрізнятися після erasure.

**Коротко:** JVM стирає generic arguments заради Java compatibility. Runtime checks використовують star projection, `reified` для доступного `T` або serializer/type token для nested generics.

</details>
<details>
<summary>140. Які існують колекції: List, Set, Map, Queue, Stack?</summary>

#### Kotlin

Колекцію вибирають за потрібною семантикою: порядок, унікальність, lookup за ключем, FIFO або LIFO.

### List

`List` — впорядкована колекція з доступом за індексом і дозволеними дублікатами:

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

Типова реалізація mutable list на JVM — `ArrayList`: доступ за index — `O(1)`, пошук — `O(n)`, insert/remove у середині — `O(n)`.

### Set

`Set` зберігає унікальні елементи:

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* exists */ }
```

`HashSet` зазвичай має `O(1)` для `contains/add/remove`, але коректність залежить від узгоджених `equals()` і `hashCode()`. Для передбачуваного iteration order використовують linked implementation, для sorted order — tree-based set.

### Map

`Map<K, V>` зберігає пари key-value; keys унікальні:

```kotlin
val usersById: Map<String, User> =
    users.associateBy(User::id)

val user = usersById["42"]
```

`HashMap.get()` у середньому працює за `O(1)`. Map підходить для cache, index і lookup table. Mutable key небезпечний: зміна полів, що беруть участь у `hashCode()`, може зробити entry недоступним.

### Queue і Stack

Queue має FIFO-семантику:

```text
first in -> first out
```

Stack має LIFO-семантику:

```text
last in -> first out
```

У Kotlin для обох сценаріїв використовують `ArrayDeque`:

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

Операції на обох кінцях мають amortized `O(1)`. `ArrayDeque` кращий за legacy Java `Stack`, який успадковує synchronized `Vector`.

Queue використовують для BFS, buffering і task processing; stack — для DFS, undo/redo, parsing і navigation history.

### Read-only та mutable API

Kotlin розділяє interfaces:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` не має `add/remove`, але не гарантує deep immutability: underlying object або його елементи можуть бути mutable. Mutable state краще інкапсулювати:

```kotlin
class UserCache {
    private val mutableUsers = mutableMapOf<String, User>()

    fun get(id: String): User? = mutableUsers[id]
    fun snapshot(): Map<String, User> = mutableUsers.toMap()
}
```

### Практичний вибір

- порядок, index, дублікати — `List`;
- унікальність і membership check — `Set`;
- lookup за key — `Map`;
- FIFO — `ArrayDeque.removeFirst()`;
- LIFO — `ArrayDeque.removeLast()`;
- priority order — `PriorityQueue`, а не звичайна Queue.

Hash-based complexity є average-case, а колекції не стають thread-safe автоматично. Для shared mutable state потрібні ownership або синхронізація.

**Коротко:** `List` зберігає порядок і дублікати, `Set` — унікальні елементи, `Map` — значення за ключем. Queue працює за FIFO, Stack — за LIFO; у Kotlin обидва сценарії зручно реалізовувати через `ArrayDeque`.

</details>
<details>
<summary>141. Що таке потокобезпечність колекцій?</summary>

#### Kotlin

Потокобезпечна колекція зберігає invariants при concurrent access. Звичайні `MutableList`, `MutableMap` і `MutableSet` у Kotlin/JVM такої гарантії не дають.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Concurrent writes можуть спричинити lost update, некоректний state або exception.

### Read-only не означає immutable

```kotlin
val users: List<User> = mutableListOf()
```

`List` забороняє mutation лише через цей reference. Underlying object та його elements можуть залишатися mutable.

### Основні стратегії

**1. Один owner/thread**

Найпростіше серіалізувати всі read/write одним dispatcher або actor-like owner. «Все на Main» працює лише без background access.

**2. Mutex для корутин**

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

Кожен доступ має проходити через той самий `Mutex`. Усередині lock не виконують network або довгу suspend work.

**3. JVM concurrent collections**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` підтримує concurrent read/write, але compound sequence не стає атомарною автоматично:

```kotlin
users.computeIfAbsent(id) { loadUser(it) }
```

`CopyOnWriteArrayList` підходить для listeners із частими reads і рідкісними writes; кожен write копіює array.

**4. Immutable snapshots**

Для UI state зручніше створювати нову колекцію замість mutation shared instance:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Це зручно для Flow/Compose, але атомарність забезпечує саме `_state.update`, а не `List`.

### Iteration

Mutation під час iteration може кинути `ConcurrentModificationException` навіть в одному thread:

```kotlin
items.removeAll { it == "B" }
```

Використовують iterator, collection operation або snapshot. Відсутність exception не доводить thread safety.

### Практичний вибір

- coroutine state — `Mutex` або serialized owner;
- concurrent keyed access — `ConcurrentHashMap`;
- read-heavy listeners — `CopyOnWriteArrayList`;
- UI state — immutable snapshots + atomic `update`;
- compound invariants — одна critical section.

Mutable collection не віддають назовні: API повертає snapshot або view із чітким ownership.

**Коротко:** mutable collections не thread-safe. Обирають serialized owner, `Mutex`, concurrent collection або immutable snapshots; синхронізувати треба весь compound invariant, а не окремий виклик.

</details>
<details>
<summary>142. У чому різниця між final, finally та finalize?</summary>

#### Kotlin

Це три незалежні поняття:

```text
final    -> заборона inheritance або override
finally  -> cleanup-блок після try/catch
finalize -> застарілий callback GC
```

### final

У Java `final` забороняє наслідування класу, override методу або повторне присвоєння змінної. У Kotlin класи й методи `final` за замовчуванням; для розширення потрібен `open`:

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

`final override` дозволяє перевизначити метод один раз і закриває його для наступних subclasses.

Для local variables та properties Kotlin використовує `val`. Він забороняє переприсвоєння reference, але не гарантує глибоку immutability:

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex") // object remains mutable
```

### finally

`finally` виконується після `try`/`catch` незалежно від нормального завершення або exception:

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Його використовують для детермінованого cleanup: закриття ресурсів, unlock, unregister listener або скидання тимчасового state.

У корутинах `finally` також виконується під час cancellation:

```kotlin
viewModelScope.launch {
    _state.update { it.copy(isLoading = true) }

    try {
        repository.sync()
    } catch (exception: CancellationException) {
        throw exception
    } catch (exception: IOException) {
        showError(exception)
    } finally {
        _state.update { it.copy(isLoading = false) }
    }
}
```

Якщо cleanup містить suspend-функцію, cancelled coroutine не зможе нормально її виконати. Для справді обов'язкового suspend-cleanup інколи використовують `withContext(NonCancellable)`, але цей блок має бути коротким.

`finally` не є абсолютною гарантією: він може не виконатися при примусовому завершенні process або JVM.

### finalize

`finalize()` — історичний метод Java `Object`, який GC міг викликати перед збиранням об'єкта. Його не можна використовувати для важливого cleanup, оскільки:

- час виклику невідомий;
- метод може не виконатися до завершення process;
- він ускладнює та сповільнює GC;
- finalization deprecated і позначена для видалення у Java.

### Чим замінити finalize

Ресурси треба закривати явно. Для `Closeable`/`AutoCloseable` у Kotlin використовують `use`:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

В Android cleanup прив'язують до lifecycle owner-а:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

У Compose side effect очищають через `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**Коротко:** `final` обмежує наслідування та override, `finally` виконує детермінований cleanup після `try/catch`, а `finalize()` — deprecated і ненадійний GC-механізм, який не слід використовувати.

</details>
<details>
<summary>143. Як працює try-catch-finally?</summary>

#### Kotlin

`try-catch-finally` розділяє ризикову операцію, обробку exception і cleanup:

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Порядок:

```text
успіх   -> try -> finally
помилка -> try -> matching catch -> finally
```

Якщо matching `catch` немає, exception поширюється caller-у після `finally`.

### catch

Можна обробляти різні типи окремо:

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

Catch blocks перевіряються зверху вниз, тому конкретніші типи мають стояти перед загальнішими. Не слід ловити `Throwable` без чіткої причини: він включає серйозні JVM errors та coroutine cancellation.

Kotlin не має checked exceptions, тому compiler не вимагає `try/catch`. Catch потрібен там, де поточний рівень може осмислено відновитися, перетворити помилку або додати context.

### try як expression

`try` повертає значення останнього expression у виконаній гілці:

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

Тип результату є спільним supertype гілок `try` і `catch`. `finally` не визначає значення expression.

### finally

`finally` використовують для детермінованого cleanup:

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

Він виконується при success, exception, `return` і coroutine cancellation. Не слід робити `return` у `finally`: він перекриє попередній result або exception. Exception із `finally` також може замінити original exception, тому cleanup має бути мінімальним і надійним.

Абсолютної гарантії немає при примусовому завершенні process/JVM.

Для `Closeable`/`AutoCloseable` краще `use`:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

Вона закриває ресурс і коректніше працює з suppressed exception, якщо одночасно падають operation та `close()`.

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

`CancellationException` треба пропускати далі. `runCatching` теж ловить її як звичайний `Throwable`, тому в cancellable code потребує явного rethrow.

Звичайний `finally` виконується в уже cancelled coroutine. Non-suspending cleanup працює, але suspend-cleanup може одразу отримати cancellation. Якщо він обов'язковий, інколи застосовують короткий `withContext(NonCancellable)`.

`try/catch` ловить exceptions лише з execution, яке входить у його dynamic scope. Failure окремо запущеної coroutine може оброблятися її parent-ом, handler-ом або біля `await()`.

**Коротко:** `try` виконує операцію й може повертати значення, `catch` обробляє matching exception, `finally` виконує cleanup незалежно від результату. У корутинах не можна ковтати `CancellationException`, а ресурси краще закривати через `use`.

</details>
<details>
<summary>144. У чому різниця між checked та unchecked exceptions?</summary>

#### Kotlin

У Java checked exception треба перехопити або оголосити через `throws`; для unchecked такої вимоги немає. Kotlin не має compile-time перевірки exceptions — усі вони поводяться як unchecked.

### Java

Checked — subclasses `Exception`, крім `RuntimeException` та її descendants:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

Caller має додати `try/catch` або `throws`.

Unchecked — `RuntimeException` та її subclasses:

```java
String value = null;
value.length(); // NullPointerException
```

Compiler не вимагає обробляти `NullPointerException`, `IllegalArgumentException` чи `IllegalStateException`.

`Error` теж не перевіряється compiler-ом і зазвичай не призначений для recovery.

### Kotlin

Kotlin дозволяє викликати API з `throws IOException` без обов’язкового catch:

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

`IOException` усе одно може виникнути в runtime, тому error contract має бути зрозумілим із API, документації або return type.

### Очікувані та програмні помилки

На практиці розділяють:

- **expected/recoverable:** offline, invalid credentials, permission denied, not found;
- **programming/contract violation:** invalid argument, impossible state, index error.

Для порушення контракту доречний exception:

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

Очікувані domain outcomes краще повертати явним типом:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

Так caller бачить відомі стани через type system. `Result<T>` підходить для success/failure boundary, але сам не моделює окремі domain cases.

### Java interoperability

Щоб Java caller бачив checked declaration Kotlin-функції, використовують `@Throws`:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

У Java signature з’явиться `throws IOException`, але для Kotlin caller-а catch залишиться необов’язковим.

### Coroutines

У корутинах exceptions також unchecked. `launch` передає failure через job hierarchy, `async` повертає його з `await()`.

`CancellationException` — сигнал cancellation, який не слід перетворювати на звичайний error:

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

Широкий `catch (Throwable)` потребує уваги до cancellation і JVM errors.

**Коротко:** checked exceptions — Java-механізм обов’язкового `catch/throws`; unchecked compiler не контролює. Kotlin не має checked exceptions, тому очікувані outcomes краще виражати return types.

</details>
<details>
<summary>145. Які існують типи помилок у Kotlin/Java?</summary>

#### Kotlin

У JVM усе, що можна кинути через `throw`, походить від `Throwable`. Дві головні гілки — `Exception` і `Error`.

1. **Базова ієрархія**

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

Kotlin використовує ту саму JVM-ієрархію, але не примушує обробляти checked exceptions.

2. **Throwable**

`Throwable` — базовий тип усіх exceptions і errors. Ловити його зазвичай небезпечно: так можна перехопити фатальний `Error` або coroutine cancellation.

3. **Exception**

`Exception` представляє ситуації, які application code потенційно може обробити: `IOException`, `SQLException`, `ParseException`, `IllegalArgumentException` або `IllegalStateException`.

4. **RuntimeException**

`RuntimeException` — unchecked exception, що часто означає programming error або порушення контракту:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

До неї належать `NullPointerException`, `IndexOutOfBoundsException`, `ClassCastException` і `NumberFormatException`.

5. **Checked та unchecked**

Java вимагає catch або `throws` для checked exceptions, наприклад `IOException`. Kotlin не перевіряє їх на рівні компілятора. `RuntimeException` та її subclasses є unchecked в обох мовах.

6. **Error**

`Error` означає серйозну JVM/runtime проблему:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Application code зазвичай не відновлюється після таких станів, тому `Error` не ловлять.

7. **Domain errors**

Очікувані business-сценарії краще моделювати sealed-типом, а не exceptions:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Наприклад, invalid email — очікуваний результат валідації.

8. **Network/API errors**

Технічні exceptions маплять у domain errors на межі data layer:

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

UI отримує зрозумілий стан, не знаючи деталей HTTP або storage.

9. **CancellationException**

У coroutines `CancellationException` — сигнал скасування, а не звичайна failure:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Її треба передавати далі, якщо generic `catch` може її перехопити.

**Коротко:** `Throwable` ділиться на recoverable `Exception` і переважно фатальний `Error`. Очікувані business failures моделюють domain-типами, а `CancellationException` не ковтають.

</details>
<details>
<summary>146. Який базовий клас для помилок?</summary>

#### Kotlin

Базовий клас для всього, що можна кинути через `throw` і перехопити через `catch`, — `Throwable`. Його основні гілки в Kotlin/JVM: `Exception` і `Error`.

1. **Базова ієрархія**

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

2. **Що містить Throwable**

`Throwable` має базову інформацію про помилку:

- message;
- cause;
- stack trace;
- suppressed exceptions.

Напряму `Throwable` кидають рідко: конкретний subtype краще описує failure і дозволяє точкову обробку.

3. **Exception**

`Exception` зазвичай представляє проблему, яку application code потенційно може обробити:

```kotlin
throw IOException("No internet")
```

```kotlin
throw IllegalStateException("User is not authorized")
```

Kotlin compiler не вимагає обробляти checked exceptions.

4. **Error**

`Error` означає серйозну JVM/runtime проблему:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Їх зазвичай не ловлять: після такого failure процес може бути нестабільним.

5. **Що дозволяє throw**

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // compile error: String не є Throwable
```

6. **Custom exception**

Custom exception наслідують від конкретної гілки:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

```kotlin
if (!isLoggedIn) {
    throw UnauthorizedException()
}
```

7. **Domain error**

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

Це не `Throwable`, а явна модель очікуваного business outcome. Exceptions доречніші для неочікуваних failures або порушень контракту.

8. **catch Throwable**

```kotlin
try {
    riskyOperation()
} catch (throwable: Throwable) {
    log(throwable)
}
```

Такий catch перехопить також `Error` і `CancellationException`. У coroutines cancellation треба передати далі:

```kotlin
catch (error: CancellationException) {
    throw error
}
```

**Коротко:** базовий клас — `Throwable`; від нього походять `Exception` і `Error`. Ловити слід конкретні recoverable exceptions, а очікувані business outcomes краще моделювати окремими типами.

</details>
<details>
<summary>147. Що таке складність алгоритмів?</summary>

#### Kotlin

Складність описує, як ростуть час або додаткова пам'ять алгоритму зі збільшенням input `n`. Використовують asymptotic notation, найчастіше Big O.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

Big O зазвичай задає upper bound; на співбесіді важливо уточнювати average, worst-case або amortized complexity.

### O(1)

Вартість не росте з `n`:

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

Доступ за index у `ArrayList` — `O(1)`. Lookup у `HashMap` — `O(1)` average, але не математично гарантований worst-case.

### O(n)

Один прохід:

```kotlin
fun findUser(
    users: List<User>,
    id: String
): User? =
    users.firstOrNull { it.id == id }
```

У worst case перевіряються всі elements. `List.contains`, `filter`, `map` — зазвичай `O(n)`.

Кілька послідовних проходів `O(n) + O(n)` залишаються `O(n)`, хоча constants у runtime зростають.

### O(n²)

Вкладене порівняння всіх пар:

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

Для duplicate check можна використати Set:

```kotlin
fun hasDuplicates(
    items: List<String>
): Boolean {
    val seen = HashSet<String>()

    return items.any { !seen.add(it) }
}
```

Time — `O(n)` average, додаткова space — `O(n)`. Це trade-off пам'яті за швидкість.

### O(log n)

Binary search щоразу ділить sorted range навпіл:

```kotlin
val index = sortedItems.binarySearch(target)
```

Search — `O(log n)`, але підготовче sorting може коштувати `O(n log n)`. Для одного пошуку сортування може бути дорожчим за linear scan; вигода з'являється при багатьох queries або вже sorted data.

### O(n log n)

Типова складність comparison sorting:

```kotlin
val sorted = users.sortedBy(User::name)
```

Конкретний algorithm і worst-case залежать від implementation та data type, тому не слід механічно вважати будь-який `sorted*` однаковим без контексту.

### Time і space

```kotlin
val copy = users.toList()
```

Time — `O(n)`, додаткова space — `O(n)`.

In-place algorithm може зменшити memory, але змінює input і ускладнює concurrency/state reasoning. Для UI immutable copies часто виправдані, поки profiling не показує bottleneck.

### Практичний Android-приклад

Погано: для кожного user робити linear search selected IDs:

```kotlin
users.map { user ->
    user.id in selectedIds // List.contains O(m)
}
```

Разом `O(n × m)`.

Краще один раз створити Set:

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

Побудова Set — `O(m)`, mapping — `O(n)`, разом `O(n + m)` average.

### Big O не дорівнює performance

Big O ігнорує constants, allocations, cache locality, I/O, device та main-thread budget. `O(n)` із network call усередині може бути значно гіршим за CPU `O(n²)` на малому input.

Спочатку обирають коректну структуру даних, потім вимірюють profiler/benchmark.

**Коротко:** складність показує масштабування часу й пам'яті. Один прохід — `O(n)`, binary search — `O(log n)`, sorting — часто `O(n log n)`, вкладені повні проходи — `O(n²)`. Потрібно враховувати average/worst case та space trade-offs.

</details>
<details>
<summary>148. Що таке пошук у глибину (DFS)?</summary>

#### Kotlin

`DFS` або `Depth-First Search` — це алгоритм обходу дерева або графа, який іде “вглиб” по одному шляху настільки далеко, наскільки можливо, а потім повертається назад і пробує інші гілки.

1. **Ідея DFS**

Приклад дерева:

```text
        A
      /        B     C
    / \        D   E     F
```

DFS може пройти так:

```text
A -> B -> D -> E -> C -> F
```

Алгоритм спочатку заглиблюється до кінця гілки, потім повертається.

2. **DFS через рекурсію**

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

Для дерева цього зазвичай достатньо, бо там немає циклів.

3. **DFS для графа**

У графі можуть бути цикли, тому потрібен `visited`:

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

Без `visited` DFS може зациклитися.

4. **Ітеративний DFS через stack**

Рекурсію можна замінити stack-ом:

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

Stack дає LIFO-поведінку, тому алгоритм іде вглиб.

5. **Складність**

Для графа:

```text
V — кількість вершин
E — кількість ребер
```

DFS має:

```text
Time:  O(V + E)
Space: O(V)
```

Памʼять потрібна для `visited` і recursion stack або явного stack.

6. **DFS vs BFS**

DFS:

- іде вглиб;
- використовує recursion або stack;
- добре підходить для cycle detection, topological sort, connected components;
- не гарантує shortest path у unweighted graph.

BFS:

- іде рівнями;
- використовує queue;
- знаходить найкоротший шлях у unweighted graph;
- може потребувати більше памʼяті для широких графів.

7. **Де використовується**

DFS часто застосовують для:

- обходу дерев;
- пошуку циклів у графі;
- connected components;
- topological sort;
- file tree traversal;
- navigation/dependency graph validation;
- flatten comments/replies tree.

8. **Ризик StackOverflow**

Recursive DFS може впасти зі `StackOverflowError`, якщо структура дуже глибока:

```text
A -> B -> C -> D -> ... -> 100000 nodes
```

Для таких випадків краще використовувати iterative DFS через `ArrayDeque`.

**Коротко:** DFS — це обхід графа або дерева вглиб через recursion або stack. Для дерева часто достатньо рекурсії, для графа потрібен `visited`, а складність алгоритму — `O(V + E)`.

</details>
<details>
<summary>149. Що таке пошук у ширину (BFS)?</summary>

#### Kotlin

`BFS` або пошук у ширину — це алгоритм обходу графа або дерева рівень за рівнем. Він спочатку відвідує всі вершини на відстані 1 від старту, потім на відстані 2 і так далі.

1. **Основна ідея**

BFS використовує чергу (`Queue`):

```text
start -> neighbors -> neighbors of neighbors -> ...
```

Черга гарантує порядок FIFO: хто перший доданий, той перший оброблений.

2. **Коли використовувати BFS**

BFS підходить для:

- пошуку найкоротшого шляху в unweighted graph;
- обходу дерева по рівнях;
- пошуку найближчого елемента;
- задач на grid/maze;
- social graph типу “друзі друзів”.

3. **BFS для графа**

```kotlin
fun bfs(graph: Map<Int, List<Int>>, start: Int): List<Int> {
    val visited = mutableSetOf<Int>()
    val queue = ArrayDeque<Int>()
    val result = mutableListOf<Int>()

    visited.add(start)
    queue.add(start)

    while (queue.isNotEmpty()) {
        val current = queue.removeFirst()
        result.add(current)

        for (neighbor in graph[current].orEmpty()) {
            if (neighbor !in visited) {
                visited.add(neighbor)
                queue.add(neighbor)
            }
        }
    }

    return result
}
```

`visited` потрібен, щоб не зациклитись у графі з циклами.

4. **Найкоротший шлях в unweighted graph**

BFS знаходить найкоротшу кількість ребер від старту до інших вершин:

```kotlin
fun shortestDistance(graph: Map<Int, List<Int>>, start: Int): Map<Int, Int> {
    val distance = mutableMapOf(start to 0)
    val queue = ArrayDeque<Int>()
    queue.add(start)

    while (queue.isNotEmpty()) {
        val current = queue.removeFirst()
        for (neighbor in graph[current].orEmpty()) {
            if (neighbor !in distance) {
                distance[neighbor] = distance[current]!! + 1
                queue.add(neighbor)
            }
        }
    }

    return distance
}
```

Для weighted graph потрібен Dijkstra, а не BFS.

5. **BFS для дерева**

```kotlin
fun levelOrder(root: Node?): List<Int> {
    if (root == null) return emptyList()

    val result = mutableListOf<Int>()
    val queue = ArrayDeque<Node>()
    queue.add(root)

    while (queue.isNotEmpty()) {
        val node = queue.removeFirst()
        result.add(node.value)

        node.left?.let(queue::add)
        node.right?.let(queue::add)
    }

    return result
}
```

Це обхід дерева по рівнях.

6. **Складність**

```text
Time:  O(V + E)
Space: O(V)
```

де `V` — кількість вершин, `E` — кількість ребер. У дереві це `O(n)`.

7. **BFS vs DFS**

```text
BFS -> черга, рівень за рівнем, shortest path в unweighted graph
DFS -> stack/recursion, йде вглиб
```

BFS часто краще, коли треба знайти найближче рішення. DFS — коли треба пройти всі варіанти вглиб або простіше рекурсивно обійти структуру.

8. **Практичне правило**

Якщо задача звучить як “найменша кількість кроків”, “найближчий елемент”, “мінімальна відстань у grid без ваг” — часто це BFS.

**Коротко:** BFS — алгоритм обходу графа/дерева в ширину через чергу. Він проходить вершини рівень за рівнем і знаходить найкоротший шлях у графі без ваг. Складність — `O(V + E)` за часом і `O(V)` за памʼяттю.

</details>
<details>
<summary>150. Розкажіть про Clean Architecture.</summary>

#### Kotlin

`Clean Architecture` — це підхід до розділення коду на шари з чіткими залежностями. Головна ідея: бізнес-логіка не має залежати від UI, framework-ів, бази даних або network-клієнтів.

1. **Основні шари**

Типова Android-структура:

```text
presentation -> domain -> data
```

- `presentation` — Activity/Fragment/Compose, ViewModel, UI state;
- `domain` — use cases, business rules, entities, repository contracts;
- `data` — repository implementations, API, Room, DataStore, mappers.

2. **Dependency rule**

Залежності мають іти всередину:

```text
presentation depends on domain
data depends on domain
domain depends on nothing external
```

Domain не має знати про Retrofit, Room, Android Context або Compose.

3. **Use Case**

Use case описує конкретну бізнес-дію:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

ViewModel викликає use case, а не напряму API/DAO.

4. **Repository contract**

У domain лежить interface:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Data layer дає implementation:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

5. **Presentation layer**

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

ViewModel керує UI state і не знає, звідки саме приходять дані.

6. **Data layer**

Data layer працює з DTO/entity і мапить їх у domain:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

DTO і Room entities не треба віддавати напряму в UI.

7. **Переваги**

- бізнес-логіка тестується без Android framework;
- UI можна змінити без переписування domain;
- data source можна замінити;
- код має чіткі boundaries;
- великій команді простіше працювати паралельно.

8. **Недоліки**

- більше файлів і boilerplate;
- для простих CRUD-екранів може бути overengineering;
- погані abstractions можуть ускладнити код;
- потрібна дисципліна з dependency direction.

9. **Коли використовувати**

Clean Architecture особливо корисна для великих apps, складної бізнес-логіки, offline-first, довгого lifecycle продукту і командної розробки. Для маленького prototype повна схема може бути зайвою.

10. **Практичне правило**

Не треба створювати use case для кожного trivial getter. Clean Architecture має зменшувати coupling і підвищувати тестованість, а не просто множити класи.

**Коротко:** Clean Architecture розділяє Android app на presentation, domain і data. Domain містить бізнес-логіку та contracts і не залежить від framework-ів. Presentation рендерить state, data реалізує доступ до API/DB. Головна ціль — тестованість, слабке звʼязування і контроль залежностей.

</details>
<details>
<summary>151. Що таке MVVM?</summary>

#### Kotlin

`MVVM` — це архітектурний патерн `Model-View-ViewModel`, який відділяє UI від бізнес-логіки і state management. В Android він зазвичай реалізується через `Activity/Fragment/Compose UI`, `ViewModel`, `StateFlow/LiveData`, use cases і repositories.

1. **Основні частини MVVM**

```text
View -> ViewModel -> Model
```

- `View` — рендерить UI і відправляє user actions.
- `ViewModel` — тримає screen state і викликає бізнес-логіку.
- `Model` — domain/data layer: use cases, repositories, API, DB.

2. **View**

У Compose View виглядає так:

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

View не має напряму ходити в API або DB. Вона тільки показує state і відправляє events.

3. **ViewModel**

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

ViewModel переживає configuration changes і не залежить від конкретної View implementation.

4. **Model layer**

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

Repository приховує джерела даних: API, Room, cache, DataStore.

5. **Data flow**

Типовий flow:

```text
User action -> ViewModel -> UseCase/Repository -> State update -> View renders state
```

Це робить поведінку екрана передбачуваною.

6. **State**

Краще мати явний screen state:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

UI має рендерити state, а не самостійно вирішувати бізнес-умови.

7. **Side effects**

Navigation, snackbar, toast — це одноразові події. Їх краще відокремлювати від state:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Для effects часто використовують `SharedFlow` або Channel.

8. **Переваги MVVM**

- менше логіки у View;
- ViewModel легко тестувати;
- state переживає rotation;
- UI можна замінити без зміни domain/data layer;
- добре працює з Compose, XML, StateFlow і LiveData.

9. **Типові помилки**

- ViewModel напряму працює з Retrofit/Room без repository/use case;
- Fragment містить бізнес-логіку;
- DTO/entity віддаються напряму в UI;
- багато незалежних mutable states без єдиного screen state;
- navigation events зберігаються як persistent state.

10. **Практичне правило**

MVVM працює добре, коли ViewModel відповідає за UI state, View тільки рендерить, а бізнес-логіка винесена в use cases/repositories. Якщо ViewModel стає “god class”, патерн використаний неправильно.

**Коротко:** MVVM розділяє UI, state management і data/domain layer. View рендерить state, ViewModel обробляє user actions і оновлює state, Model/use cases/repositories виконують бізнес-логіку та роботу з даними. Це робить Android-код тестованішим і простішим для підтримки.

</details>
<details>
<summary>152. У чому різниця між MVVM та MVI?</summary>

#### Kotlin

`MVVM` і `MVI` — це архітектурні підходи для UI state management. Обидва відділяють UI від бізнес-логіки, але по-різному організовують state, events і side effects.

1. **MVVM**

MVVM складається з:

```text
Model -> ViewModel -> View
```

- `View` показує UI і відправляє user actions.
- `ViewModel` зберігає state і викликає use cases/repositories.
- `Model` — domain/data layer.

Приклад:

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() { /* update state */ }
}
```

2. **MVI**

MVI має більш формальний unidirectional flow:

```text
Intent/Event -> Reducer/ViewModel -> State -> UI
                         └── Effect
```

UI не викликає довільні методи, а відправляє events/intents:

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

ViewModel обробляє event і створює новий immutable state.

3. **State**

У MVVM state може бути кількома потоками або одним state object:

```kotlin
val isLoading: StateFlow<Boolean>
val user: StateFlow<UserUi?>
```

У MVI зазвичай один screen state:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

4. **Events**

MVVM часто має окремі methods:

```kotlin
viewModel.load()
viewModel.retry()
viewModel.onBackClick()
```

MVI частіше має один entry point:

```kotlin
viewModel.onEvent(ProfileEvent.RetryClicked)
```

Це робить flow більш стандартизованим, але додає boilerplate.

5. **Side effects**

В обох підходах navigation/snackbar/toast краще не тримати як звичайний state. Зазвичай використовують `SharedFlow`/Channel для effects:

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

6. **Переваги MVVM**

- простіший;
- менше boilerplate;
- добре підходить для більшості Android screens;
- нативно лягає на ViewModel + StateFlow/LiveData;
- легше стартувати команді.

7. **Переваги MVI**

- predictable unidirectional data flow;
- один immutable screen state;
- легко відтворювати bug через sequence events;
- краще для складних екранів із багатьма станами;
- простіше тестувати reducer/event logic.

8. **Недоліки**

MVVM може стати хаотичним, якщо ViewModel має багато mutable states і methods без єдиного flow.

MVI може бути надмірним для простих екранів через events, reducers, effects і boilerplate.

9. **Коли що обрати**

```text
простий/середній екран -> MVVM
складний stateful екран -> MVI
велика команда з однаковим flow -> MVI може бути корисним
```

10. **Практичне правило**

Важливіше не назва патерна, а чіткий data flow: UI рендерить state, user actions йдуть у ViewModel, бізнес-логіка в use cases, side effects не змішуються зі state.

**Коротко:** MVVM простіший і гнучкіший: View викликає methods ViewModel і рендерить state. MVI більш формальний: UI відправляє intents/events, ViewModel/reducer створює один immutable state, а одноразові дії йдуть через effects. MVVM краще для простих екранів, MVI — для складних stateful flows.

</details>
<details>
<summary>153. Які архітектурні патерни використовуються в Android?</summary>

#### Kotlin

В Android використовують кілька архітектурних підходів: MVVM, MVI, Clean Architecture, Repository pattern, Use Case pattern, layered architecture і feature modularization. Вибір залежить від складності app, команди і вимог до тестованості.

1. **MVVM**

```text
View -> ViewModel -> Model
```

MVVM — найпоширеніший Android-патерн. UI рендерить state, ViewModel обробляє user actions і викликає domain/data layer.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Добре працює з Compose, XML, StateFlow і LiveData.

2. **MVI**

```text
Intent/Event -> Reducer/ViewModel -> State -> UI
```

MVI робить data flow більш формальним: UI відправляє events, ViewModel створює immutable state, UI його рендерить.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

Корисний для складних екранів із багатьма станами.

3. **Clean Architecture**

Типова структура:

```text
presentation -> domain -> data
```

- `presentation` — UI/ViewModel;
- `domain` — use cases, business rules, contracts;
- `data` — repositories implementations, API, DB.

Domain не має залежати від Android framework, Retrofit або Room.

4. **Repository pattern**

Repository приховує джерела даних:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Implementation може працювати з API, Room, cache або DataStore, але ViewModel про це не знає.

5. **Use Case pattern**

Use case описує конкретну бізнес-дію:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

Це допомагає не роздувати ViewModel бізнес-логікою.

6. **Layered architecture**

```text
UI -> ViewModel -> UseCase -> Repository -> DataSource
```

Це практична форма Clean Architecture. Важливо, щоб залежності йшли в один бік і UI не знав про API/DB details.

7. **Feature modularization**

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Feature-first модулі допомагають масштабувати проєкт і командну роботу. Core-модулі містять спільну інфраструктуру.

8. **Dependency Injection**

DI — не архітектурний патерн UI, але важлива частина архітектури:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt/Koin/manual DI зʼєднує contracts з implementations.

9. **Коли що використовувати**

- Простий app — MVVM + Repository.
- Складний screen state — MVI.
- Великий app — Clean Architecture + modularization.
- Offline-first — Repository + local source of truth.
- Багато команд — feature modules + чіткі contracts.

10. **Практичне правило**

Патерн має зменшувати складність, а не додавати її. Для малого екрана повний Clean + MVI може бути надмірним. Для великого app відсутність шарів і boundaries швидко призводить до хаосу.

**Коротко:** в Android найчастіше використовують MVVM, MVI, Clean Architecture, Repository, Use Cases, layered architecture, DI і feature modularization. Головна ціль — розділити UI, state, бізнес-логіку і data layer так, щоб код було легко змінювати, тестувати й масштабувати.

</details>
<details>
<summary>154. Як організувати модулі в Android-проєкті?</summary>

#### Kotlin

Модулі в Android-проєкті організовують так, щоб зменшити coupling, прискорити build, ізолювати features і дати командам працювати паралельно. Головне — не кількість модулів, а правильні boundaries і напрямок залежностей.

1. **Типова структура**

```text
:app
:core:network
:core:database
:core:ui
:core:common
:feature:profile
:feature:search
:feature:settings
:domain:user
:data:user
```

Структура залежить від розміру проєкту. Малому app не потрібні десятки модулів.

2. **app module**

`:app` — entry point застосунку:

- Application class;
- main Activity;
- navigation host;
- DI wiring;
- feature dependency assembly;
- build flavors/config.

`:app` може знати про feature/data modules, але бізнес-логіка не має жити в `app`.

3. **feature modules**

Feature-модулі містять UI і presentation logic конкретної фічі:

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

Feature має залежати від domain contracts/use cases, а не від concrete Retrofit/Room implementation.

4. **core modules**

Core-модулі містять спільну інфраструктуру:

```text
:core:network  -> Retrofit/OkHttp setup
:core:database -> Room setup
:core:ui       -> design system/components
:core:common   -> utils, dispatchers, result types
```

Core не має залежати від feature.

5. **domain modules**

Domain містить business rules і contracts:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

Domain має бути максимально незалежним від Android framework.

6. **data modules**

Data реалізує repository contracts:

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Тут живуть DTO, entities, mappers, API/DB data sources.

7. **Dependency direction**

Правильний напрямок:

```text
feature -> domain
 data   -> domain
 app    -> feature + data
core    -> не залежить від feature
```

Не можна допускати циклічних залежностей і ситуації, коли core знає про конкретну feature.

8. **Convention plugins**

Повторювану Gradle-конфігурацію краще винести в convention plugins:

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

Це зменшує дублювання build scripts.

9. **Коли не дробити**

Не треба створювати модуль для кожного класу. Надмірна модульність ускладнює navigation, DI, Gradle setup і refactoring. Модуль має мати чітку відповідальність і реальну причину існувати.

10. **Практичне правило**

Починай з простої структури. Діли на модулі, коли є реальний boundary: feature ownership, reusable core, domain contract, data implementation або build-time benefit.

**Коротко:** Android-модулі варто організовувати навколо `app`, `feature`, `core`, `domain` і `data`. Головне — правильний напрямок залежностей: feature працює через domain contracts, data реалізує contracts, core не залежить від features, а app збирає усе разом через DI/navigation.

</details>
<details>
<summary>155. Що таке токен?</summary>

#### Kotlin

Токен — це credential, який підтверджує сесію або право доступу. У mobile/backend системах зазвичай використовують access token і refresh token.

### Access і refresh token

Access token має короткий TTL і передається із запитами до захищеного API:

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

Refresh token живе довше й використовується лише для оновлення access token:

```text
API request -> 401 Unauthorized -> refresh token -> new access token -> retry request
```

Паралельні `401` не повинні запускати кілька refresh-запитів: оновлення треба синхронізувати, а повтор запиту обмежити, щоб уникнути циклу.

### JWT і Bearer

JWT має формат `header.payload.signature`. Payload може містити claims:

```json
{
  "sub": "user-id",
  "exp": 1710000000,
  "role": "user"
}
```

Payload лише Base64Url encoded, а не зашифрований, тому секретні дані в ньому зберігати не можна. Підпис перевіряє цілісність, але остаточну валідність токена визначає backend.

Bearer token може використати будь-хто, хто ним володіє. Викрадений токен дає доступ до expiration або revoke.

Токен не є паролем: система видає його після login/authorization flow, може обмежити TTL і scope та відкликати.

### Зберігання в Android

Для sensitive storage використовують Android Keystore, EncryptedSharedPreferences або DataStore з encryption. Деталі реалізації краще приховати за interface:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Токени не повинні потрапляти в logs, crash reports, analytics, screenshots, URL query parameters або незашифровані файли.

### Expiration і scope

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)

fun AuthTokens.isExpired(clock: Clock): Boolean =
    clock.nowMillis() >= expiresAtMillis
```

Локальна перевірка expiration допомагає завчасно оновити токен, але backend залишається джерелом істини. Невдалий refresh має очистити сесію та перевести користувача на login.

Scope обмежує дозволи токена за принципом least privilege, наприклад `profile:read` або `payments:create`. Mobile app не повинен отримувати admin permissions чи server secrets. Logout має видаляти локальні токени й, за можливості, відкликати refresh token на backend.

**Коротко:** токен — це credential для підтвердження доступу або сесії. Access token додається в API-запити, refresh token використовується для його оновлення. Ключові вимоги — безпечне зберігання, короткий TTL, мінімальні scopes і відсутність токенів у логах.

</details>
<details>
<summary>156. У чому різниця між authentication та authorization?</summary>

#### Kotlin

```text
Authentication -> хто ти?
Authorization  -> що тобі дозволено?
```

Authentication встановлює principal, authorization оцінює policy для конкретної operation/resource.

### Authentication

Identity підтверджують password, OTP, passkey/biometrics, OAuth/OIDC або чинна session:

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Після login backend видає session cookie або tokens. Access token авторизує API request; refresh token використовується лише для отримання нового access token.

### Authorization

Authorization перевіряє право principal виконати operation:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

Backend враховує role, permissions, resource ownership, tenant і context. Role сама по собі часто недостатня.

Типові моделі:

- RBAC — roles;
- permission-based — granular permissions;
- ABAC/policy-based — attributes principal/resource/context.

### 401 і 403

```text
401 Unauthorized -> request не має valid authentication credentials
403 Forbidden    -> server зрозумів request, але відмовляє
```

При `401` app може виконати один синхронізований token refresh. Після failure session завершується; нескінченний retry або паралельні refresh requests створюють loops/races.

При `403` re-authentication зазвичай не змінює policy; UI показує відсутність доступу.

### Android client

Client додає access token interceptor-ом, координує refresh через authenticator/session manager і зберігає credentials відповідно до threat model.

App може використовувати permissions для UX:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDelete) {
        Text("Delete")
    }
}
```

Прихована кнопка — лише UX. Modified client може викликати API напряму, тому backend завжди enforce-ить authorization.

### Розділення відповідальностей

- auth service видає/перевіряє credentials;
- policy вирішує access до operation;
- client обробляє credentials та `401/403`;
- backend не довіряє role/permission від client-а;
- logout/revoke завершує session за security model.

Це різні security decisions, навіть якщо виконуються одним middleware.

**Коротко:** authentication встановлює identity, authorization перевіряє її право на operation/resource. Client керує credentials і UX, але security enforcement завжди виконує backend.

</details>
<details>
<summary>157. Які інструменти використовуються для комунікації з бекендом?</summary>

#### Kotlin

Networking stack залежить від протоколу: REST/HTTP, GraphQL, WebSocket або gRPC. Окремо потрібні serialization, authentication, error mapping і cache.

1. **Retrofit**

Retrofit описує REST API Kotlin interface-ом:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

Він підтримує suspend-функції та працює поверх OkHttp із converter-ами.

2. **OkHttp**

OkHttp виконує HTTP-запити під Retrofit або напряму:

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Interceptors додають headers, auth, logging і tracing. Retry має враховувати idempotency запиту.

3. **Serialization**

Для JSON використовують Kotlin Serialization, Moshi або Gson.

DTO краще відділяти від domain models:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

4. **Ktor Client / KMP**

Ktor Client дозволяє винести networking у Kotlin Multiplatform:
Engine і platform-specific configuration можна підставляти окремо.

5. **GraphQL**

Apollo Kotlin генерує type-safe models із GraphQL operations:
GraphQL додає schema/codegen та окремі правила caching і error handling.

6. **WebSocket**

WebSocket забезпечує двосторонній realtime channel для chat, live updates або market data. На Android його підтримують OkHttp і Ktor.

7. **gRPC**

gRPC — contract-first RPC через protobuf із generated clients. Його обирають, якщо backend уже має відповідний contract та infrastructure.

8. **Authentication**

Типові механізми: Bearer/refresh tokens, OAuth/OIDC, cookies або API keys.

Interceptor додає credentials до request, а `Authenticator` може координувати refresh після authentication challenge. Refresh має бути синхронізований, щоб паралельні 401 не запускали багато оновлень token.

9. **Error handling**

Network layer мапить transport/HTTP failures у власний `ApiError`; UI не повинен залежати від `HttpException` або конкретного client-а.

10. **Caching і offline**

HTTP cache зберігає responses, Room — structured offline data, `Paging + RemoteMediator` координує paged local/remote source, а repository визначає source of truth. Secrets не слід зберігати у звичайному DataStore без відповідної security model.

11. **Практичне правило**

- REST — Retrofit + OkHttp;
- KMP — Ktor Client;
- GraphQL — Apollo Kotlin.
- Realtime — WebSocket.
- Protobuf RPC — gRPC;
- offline source of truth — Room + repository.

**Коротко:** Retrofit/OkHttp підходять для REST, Ktor Client — для KMP, Apollo Kotlin — для GraphQL, WebSocket — для realtime, gRPC — для protobuf RPC. Repository із Room координує offline data.

</details>
<details>
<summary>158. Що таке Retrofit?</summary>

#### Kotlin

`Retrofit` — це HTTP client library для Android/JVM, яка спрощує роботу з REST API. Вона описує endpoints через Kotlin/Java interfaces, а реальні HTTP-запити виконує через OkHttp.

1. **Базовий API interface**

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto

    @POST("users")
    suspend fun createUser(@Body body: CreateUserRequest): UserDto
}
```

Retrofit генерує implementation цього interface runtime-ом.

2. **Створення Retrofit**

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(MoshiConverterFactory.create())
    .build()

val api = retrofit.create(UserApi::class.java)
```

`baseUrl` має закінчуватись `/`.

3. **Coroutines support**

Retrofit підтримує `suspend` functions:

```kotlin
val user = api.getUser("123")
```

Не треба вручну працювати з callbacks для звичайних запитів.

4. **Path, Query, Body**

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

Annotations описують, як параметри перетворюються в HTTP request.

5. **Response handling**

Можна повертати DTO напряму або `Response<T>`:

```kotlin
suspend fun getUser(id: String): Response<UserDto>
```

`Response<T>` потрібен, якщо треба читати status code, headers або error body.

6. **Converters**

Retrofit сам не парсить JSON. Для цього підключають converter:

- Moshi;
- Gson;
- Kotlin Serialization;
- Scalars.

DTO краще мапити в domain model окремо:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

7. **OkHttp interceptors**

Через OkHttp додають auth, logs, headers, cache:

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

8. **Error handling**

Retrofit може кинути:

- `IOException` для network errors;
- `HttpException` для non-2xx, якщо повертаєш DTO напряму;
- serialization exception для некоректного JSON.

У repository краще мапити це в domain error:

```kotlin
sealed interface NetworkError {
    data object NoInternet : NetworkError
    data object Unauthorized : NetworkError
    data class Server(val code: Int) : NetworkError
}
```

9. **Куди класти Retrofit**

Retrofit API interfaces і DTO — це data/network layer. ViewModel не має напряму викликати Retrofit. Краще:

```text
ViewModel -> UseCase -> Repository -> Retrofit API
```

10. **Практичне правило**

Retrofit — хороший default для REST API в Android. Але важливо не протягувати DTO/HttpException у UI, а мати repository, mappers, error model, interceptors і тестовану boundary між network та domain.

**Коротко:** Retrofit — бібліотека для REST API, яка описує HTTP endpoints через interfaces і annotations (`@GET`, `@POST`, `@Path`, `@Query`, `@Body`). Вона працює поверх OkHttp, підтримує coroutines і converters для JSON. У production її ховають за repository layer.

</details>
<details>
<summary>159. Що таке Apollo GraphQL?</summary>

#### Kotlin

Apollo Kotlin — це type-safe GraphQL client для Kotlin/Android. Він генерує Kotlin-код зі schema та `.graphql` операцій, підтримує queries, mutations, subscriptions і normalized cache.

У GraphQL клієнт сам визначає потрібні поля:

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

Apollo генерує для операції Kotlin-класи:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

Несумісність schema та query виявляється під час code generation або компіляції.

### Налаштування клієнта

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

Auth та інші загальні параметри додаються interceptor-ом:

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

### Операції

Query читає дані:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()
val user = response.data?.user ?: error("User not found")
```

Mutation змінює дані:

```kotlin
apolloClient.mutation(UpdateUserNameMutation(id, name)).execute()
```

Subscription використовується для realtime updates через WebSocket:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
        // handle message
    }
```

### Cache та помилки

Normalized cache зберігає сутності за ключем, тому різні queries можуть використовувати ті самі дані. Це корисно для offline-сценаріїв, зменшення network-запитів та оновлення UI після mutation. Cache strategy залежить від стабільних normalized IDs у schema.

GraphQL response може одночасно містити `data` та `errors`, а network failure приходить exception-ом:

```kotlin
val response = apolloClient.query(GetUserQuery(id)).execute()

if (response.hasErrors()) {
    // map response.errors to domain errors
}

val data = response.data
```

GraphQL error не обов'язково означає HTTP 4xx/5xx. Generated models краще мапити в domain-моделі, а API-помилки — у domain-level errors.

### Apollo vs Retrofit

- Apollo використовують для GraphQL: клієнт задає response shape, моделі генеруються.
- Retrofit використовують для REST: backend задає endpoints і форму response, DTO описуються вручну.

**Коротко:** Apollo Kotlin — це type-safe GraphQL client для Android/Kotlin. Він генерує Kotlin-код із schema і queries, підтримує queries, mutations, subscriptions, cache і підходить для проєктів із GraphQL backend.

</details>
<details>
<summary>160. Для чого використовується Firebase?</summary>

#### Kotlin

`Firebase` — це набір backend-as-a-service інструментів від Google для мобільних і web-додатків. В Android його використовують для аналітики, crash reporting, push notifications, auth, remote config, database/storage і швидкого запуску backend-функцій без власної інфраструктури.

1. **Firebase Analytics**

Використовується для збору подій користувача:

```kotlin
firebaseAnalytics.logEvent("profile_opened", bundleOf("source" to "home"))
```

Допомагає аналізувати funnels, retention, engagement, feature usage.

2. **Crashlytics**

Crashlytics збирає crashes і non-fatal errors:

```kotlin
Firebase.crashlytics.recordException(error)
```

У production це один із базових інструментів для моніторингу стабільності app.

3. **Firebase Cloud Messaging**

FCM використовується для push notifications:

```kotlin
class AppMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // handle push
    }
}
```

Підходить для notifications, silent sync triggers, messaging сценаріїв.

4. **Firebase Auth**

Firebase Auth дає готову authentication platform:

- email/password;
- Google Sign-In;
- phone auth;
- anonymous auth;
- custom tokens.

Корисно для швидкого старту, але треба правильно інтегрувати з власним backend/security model.

5. **Firestore / Realtime Database**

Firestore і Realtime Database — NoSQL cloud databases з realtime updates:

```kotlin
firestore.collection("users").document(id).get()
```

Підходять для chat, realtime sync, collaborative features, prototypes. Але правила безпеки треба писати дуже уважно.

6. **Remote Config**

Remote Config дозволяє змінювати параметри без релізу app:

```kotlin
val enabled = remoteConfig.getBoolean("new_feature_enabled")
```

Використовується для feature flags, rollout, A/B тестів, конфігурації UI/behavior.

7. **Cloud Storage**

Firebase Storage зберігає файли: images, videos, user uploads.

```kotlin
storage.reference.child("avatars/$userId.jpg")
```

Часто використовується разом з Auth і Firestore rules.

8. **Cloud Functions**

Cloud Functions дозволяють виконувати backend logic:

- trigger на Firestore changes;
- custom API endpoint;
- push notification dispatch;
- server-side validation;
- integration з third-party APIs.

9. **Переваги Firebase**

- швидкий старт;
- готова auth/analytics/crashes/push;
- хороша Android інтеграція;
- realtime capabilities;
- менше власної backend-інфраструктури на старті.

10. **Ризики Firebase**

- vendor lock-in;
- security rules mistakes;
- складний pricing при рості;
- не завжди підходить для складної backend-доменіки;
- offline/cache behavior треба розуміти окремо;
- client-side access не має замінювати backend security.

**Коротко:** Firebase використовують для analytics, Crashlytics, push notifications, auth, Firestore/Realtime Database, Remote Config, Storage і Cloud Functions. Це прискорює розробку Android app, але вимагає уважних security rules, контролю вартості, архітектурних boundaries і розуміння vendor lock-in.

</details>
<details>
<summary>161. Що таке Android Keystore?</summary>

#### Kotlin

`Android Keystore` — це системне сховище криптографічних ключів. Воно дозволяє створювати й використовувати ключі так, щоб raw key material не покидав Keystore. У деяких пристроях ключі можуть бути захищені hardware-backed security.

1. **Для чого потрібен Keystore**

Keystore використовують для:

- encryption/decryption;
- signing/verification;
- захисту локальних secrets;
- ключів для EncryptedSharedPreferences/DataStore encryption;
- biometric/user-auth protected keys;
- зменшення ризику витоку raw crypto key.

2. **Важливе обмеження**

Keystore не робить hardcoded API key безпечним. Якщо secret вже лежить в APK, attacker може знайти місце, де app його використовує. Keystore краще підходить для ключів, які генеруються на пристрої.

3. **Генерація ключа**

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

Ключ створюється всередині Android Keystore.

4. **Отримання ключа**

```kotlin
val keyStore = KeyStore.getInstance("AndroidKeyStore").apply { load(null) }
val key = keyStore.getKey("secret_key", null) as SecretKey
```

Ти отримуєш handle до ключа, але не raw bytes ключа.

5. **Шифрування**

```kotlin
val cipher = Cipher.getInstance("AES/GCM/NoPadding")
cipher.init(Cipher.ENCRYPT_MODE, key)
val iv = cipher.iv
val encrypted = cipher.doFinal(data)
```

Для AES-GCM важливо зберігати `iv` поруч із ciphertext і не перевикористовувати IV з тим самим ключем.

6. **User authentication**

Ключ можна зробити доступним тільки після автентифікації користувача:

```kotlin
.setUserAuthenticationRequired(true)
```

Це корисно для чутливих операцій, але UX і lifecycle треба продумувати окремо.

7. **EncryptedSharedPreferences**

У багатьох випадках краще використовувати готові Jetpack Security API:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()
```

Це простіше і менш ризиковано, ніж писати crypto вручну.

8. **Що зберігати**

Keystore/Jetpack Security підходить для:

- refresh tokens;
- local encryption keys;
- user-specific secrets;
- sensitive preferences.

Але backend/service secrets краще не класти в app взагалі.

9. **Практичні ризики**

- неправильний crypto mode;
- reuse IV;
- відсутність key rotation;
- backup/restore edge cases;
- різна поведінка на різних Android versions/devices;
- плутанина між “захистити local token” і “сховати hardcoded API secret”.

10. **Практичне правило**

Для local sensitive data використовуй Keystore через Jetpack Security, якщо можливо. Для server secrets — backend proxy. Для crypto вручну — тільки якщо команда розуміє algorithms, IV, authentication tags, rotation і threat model.

**Коротко:** Android Keystore — системне сховище криптографічних ключів, де raw key material не має покидати Keystore. Він корисний для local encryption/signing і захисту user tokens, але не вирішує проблему hardcoded API secrets у APK. Для типових задач краще використовувати Jetpack Security поверх Keystore.

</details>
<details>
<summary>162. Які є dispatchers у Kotlin Coroutines і для чого вони використовуються?</summary>

#### Kotlin

`CoroutineDispatcher` визначає, на якому thread pool або thread буде виконуватись coroutine. Правильний dispatcher важливий для performance, UI responsiveness і уникнення ANR.

1. **Dispatchers.Main**

`Dispatchers.Main` — головний UI thread:

```kotlin
withContext(Dispatchers.Main) {
    textView.text = "Loaded"
}
```

Використовується для UI updates, Compose/View state, navigation, lifecycle-bound UI logic. Не підходить для network, DB, file I/O або важких обчислень.

2. **Dispatchers.IO**

`Dispatchers.IO` — для blocking I/O:

```kotlin
withContext(Dispatchers.IO) {
    file.readText()
}
```

Підходить для file operations, database, network clients, blocking SDK calls. Не треба використовувати для CPU-heavy алгоритмів.

3. **Dispatchers.Default**

`Dispatchers.Default` — для CPU-heavy роботи:

```kotlin
withContext(Dispatchers.Default) {
    largeList.sortedBy { it.score }
}
```

Підходить для сортування, parsing великих структур, обчислень, compression, diff calculation. Використовує pool, орієнтований на CPU cores.

4. **Dispatchers.Unconfined**

`Dispatchers.Unconfined` не привʼязує coroutine до конкретного thread і продовжує виконання там, де resume-нулась suspension.

```kotlin
launch(Dispatchers.Unconfined) { }
```

У production Android-коді майже не використовується, бо поведінка менш передбачувана.

5. **limitedParallelism**

```kotlin
val limitedIo = Dispatchers.IO.limitedParallelism(4)
```

Корисно, коли треба обмежити кількість паралельних задач: uploads, API calls, image processing, batch jobs.

6. **Custom dispatcher**

Іноді створюють dispatcher з executor:

```kotlin
val dispatcher = Executors.newSingleThreadExecutor().asCoroutineDispatcher()
```

Його треба закривати, якщо він створений вручну. У більшості Android-коду достатньо стандартних dispatchers.

7. **withContext**

`withContext` перемикає dispatcher для частини suspend-функції:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

Це краще, ніж запускати нову coroutine тільки для зміни thread.

8. **Інжектити dispatchers**

Для тестованості dispatchers краще передавати через constructor:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): User = withContext(ioDispatcher) {
        api.getUser()
    }
}
```

У тесті можна передати `StandardTestDispatcher`.

9. **Типові помилки**

- network або DB на `Main`;
- CPU-heavy робота на `IO`;
- `runBlocking` на main thread;
- hardcoded dispatchers без можливості тестування;
- створення власних thread pools без cleanup;
- використання `Unconfined` без розуміння наслідків.

10. **Практичне правило**

```text
UI work       -> Main
blocking I/O  -> IO
CPU-heavy     -> Default
tests         -> TestDispatcher
limit needed  -> limitedParallelism
```

**Коротко:** dispatchers визначають, де виконується coroutine. `Main` — для UI, `IO` — для blocking I/O, `Default` — для CPU-heavy задач, `Unconfined` — рідкісний спеціальний випадок. Для тестованості dispatchers краще інжектити, а не хардкодити.

</details>
<details>
<summary>163. Які є способи запуску корутин?</summary>

#### Kotlin

`launch` і `async` запускають coroutine у вибраному scope. `coroutineScope`/`supervisorScope` створюють structured boundary, а Android scopes визначають owner і lifetime роботи.

1. **launch**

`launch` повертає `Job`, але не business result:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

```kotlin
val job = scope.launch { work() }
job.cancel()
```

Підходить для side effect, failure якого обробляється через scope hierarchy або локальний `try/catch`.

2. **async**

`async` повертає `Deferred<T>` для concurrent result:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

Якщо виклик одразу `await`-иться без паралельної роботи, простіше викликати suspend-функцію напряму.

3. **coroutineScope**

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

`coroutineScope` чекає всіх children; failure одного скасовує siblings і передається caller-у.

4. **supervisorScope**

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

`supervisorScope` не скасовує siblings через failure direct child. Failure все одно треба отримати через `await()` або обробити в child.

5. **runBlocking**

`runBlocking` блокує caller thread. Він доречний для CLI entry point або обмеженого sync bridge; у tests краще `runTest`, а на Android Main він може спричинити ANR.

6. **Android scope owners**

`viewModelScope` скасовується в `ViewModel.onCleared()` і підходить для screen operations.

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

`lifecycleScope` скасовується при destroy owner-а. Flow для UI збирають через `repeatOnLifecycle`; у Fragment View використовують `viewLifecycleOwner.lifecycleScope`.

7. **rememberCoroutineScope**

У Compose:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

Scope належить місцю виклику в Composition і підходить для callback-driven UI actions: snackbar, drawer або scroll animation.

8. **GlobalScope**

У app-коді він зазвичай неправильний, бо робота не має application owner-а та явного cleanup.

**Коротко:** `launch` запускає side effect, `async` — concurrent result. `coroutineScope` дає fail-fast semantics, `supervisorScope` ізолює siblings. Scope завжди обирають за owner-ом; `runBlocking` на Main не використовують.

</details>
<details>
<summary>164. У чому різниця між launch та async?</summary>

#### Kotlin

Обидва — coroutine builders:

```text
launch -> Job, без result value
async  -> Deferred<T>, result через await()
```

### launch

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

`Job` дозволяє `cancel()`, `join()` і перевірку state. Launch використовують, коли result споживається всередині coroutine: UI-state update, Flow collection, save або sync.

```kotlin
fun load(id: String) {
    viewModelScope.launch {
        state.value = UiState.Loading
        state.value =
            UiState.Content(repository.getUser(id))
    }
}
```

### async

```kotlin
val deferred: Deferred<User> = async {
    repository.getUser()
}

val user = deferred.await()
```

`Deferred<T>` є `Job` із result або exception. Async потрібен переважно для кількох concurrent results:

```kotlin
suspend fun loadDashboard(): Dashboard =
    coroutineScope {
        val user = async {
            userRepository.getUser()
        }
        val feed = async {
            feedRepository.getFeed()
        }

        Dashboard(
            user = user.await(),
            feed = feed.await()
        )
    }
```

Без `async` два suspend calls виконувалися б послідовно. Concurrency не гарантує parallel execution: це залежить від dispatcher-а.

### Structured concurrency

`coroutineScope` чекає всіх children. Cancellation caller-а скасовує обидва `Deferred`.

У звичайному scope failure одного child скасовує parent і siblings. Failure `async` у structured scope поширюється parent-у одразу; `await()` лише повертає або повторно кидає result.

Для незалежних children використовують supervisor semantics та окремо визначають error policy для кожного result.

### Exceptions

У root `launch` uncaught exception передається `CoroutineExceptionHandler`; у child — parent-у.

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

Для `async` exception отримують через `await()`, але catch має бути в scope owner-а, який розуміє, чи failure має скасувати інші tasks.

### Типові помилки

`async` без `await()`:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

Якщо value не потрібне — `launch`. Якщо потрібен один value без паралельності — звичайна suspend-функція.

Не слід повертати `Deferred` із випадкового зовнішнього scope. Structured suspend API простіше скасувати, тестувати й комбінувати.

Також не треба використовувати `async` лише для перемикання dispatcher-а — для цього існує `withContext`.

### Практичний вибір

- робота без зовнішнього result — `launch`;
- кілька concurrent results — `async + await`;
- один result — suspend-функція;
- пов'язані children — `coroutineScope`;
- незалежні failures — supervisor з явною policy;
- cancellation не ковтати.

**Коротко:** `launch` повертає `Job` для роботи без result, `async` — `Deferred<T>` для value через `await()`. Async використовують для concurrent results усередині structured scope, а не як універсальну заміну launch.

</details>
<details>
<summary>165. Як обробляти помилки в корутинах?</summary>

#### Kotlin

Стратегія залежить від ownership: очікувані failures обробляють локально й маплять у result/state, а необроблені поширюються через `Job` hierarchy. `CancellationException` не можна ковтати.

1. **try/catch у suspend code**

```kotlin
suspend fun loadUser(): Result<User> {
    return try {
        Result.success(api.getUser())
    } catch (e: IOException) {
        Result.failure(e)
    }
}
```

Ловити краще конкретні exceptions на boundary, де зрозуміло, як їх обробити.

2. **У ViewModel**

`viewModelScope.launch` переводить UI у `Loading`, викликає use case і мапить result у `Success` або `Error`. UI отримує state, а не transport/storage exception.

3. **CancellationException**

При широкому catch cancellation передають далі:

```kotlin
catch (e: CancellationException) {
    throw e
} catch (e: Exception) {
    handleError(e)
}
```

4. **CoroutineExceptionHandler**

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

Handler — last-resort для uncaught exception root `launch`, зазвичай для logging. Він не відновлює coroutine і не замінює локальний state handling.

5. **async exceptions**

`await()` повертає result або кидає failure `Deferred`:

```kotlin
val deferred = async { api.getUser() }
try {
    val user = deferred.await()
} catch (e: Exception) {
    handleError(e)
}
```

Але в звичайному structured scope child `async` також скасує parent одразу після failure. Локальний `try/catch` навколо `await()` не ізолює sibling tasks; для незалежності потрібна supervisor semantics.

6. **coroutineScope vs supervisorScope**

У `coroutineScope` failure child скасовує scope та siblings. У `supervisorScope` failure direct child не скасовує siblings, але її все одно треба обробити:

```kotlin
supervisorScope {
    val a = async { runCatching { requestA() } }
    val b = async { runCatching { requestB() } }
}
```

7. **Flow catch**

```kotlin
repository.users()
    .catch { e -> emit(emptyList()) }
    .collect { users -> render(users) }
```

`catch` бачить лише upstream exceptions і не має перетворювати cancellation на fallback.

8. **Result / sealed errors**

На data/domain boundary exceptions маплять у `Result` або власний `AppError`, щоб UI не знав деталі Retrofit, OkHttp чи SQL.

9. **Типові помилки**

- ковтати cancellation у generic catch/runCatching;
- покладатися лише на `CoroutineExceptionHandler`;
- ігнорувати failure `await()`;
- передавати raw exceptions у UI;
- змішувати retry, mapping і rendering;
- не логувати unexpected failure.

**Коротко:** очікувані failures обробляють через `try/catch`, `await()` або `Flow.catch` і маплять у state/error type. `coroutineScope` працює fail-fast, supervisor ізолює siblings, а `CancellationException` передають далі.

</details>
<details>
<summary>166. Як дочекатися результату декількох паралельних запитів?</summary>

#### Kotlin

Незалежні suspend-запити запускають через `async` у structured scope:

```kotlin
suspend fun loadScreen(): ScreenData =
    coroutineScope {
        val user = async {
            api.getUser()
        }
        val posts = async {
            api.getPosts()
        }

        ScreenData(
            user = user.await(),
            posts = posts.await()
        )
    }
```

Обидва `async` створені до першого `await()`, тому calls виконуються concurrently. Реальний parallelism залежить від dispatcher та API.

### coroutineScope

`coroutineScope` чекає children, поширює cancellation caller-а вниз і працює fail-fast: failure child скасовує siblings та повертається caller-у.

Це правильна policy, якщо потрібні всі results. Operation має належати caller-у, а не `GlobalScope`.

### awaitAll

Для однотипних requests:

```kotlin
val users = coroutineScope {
    ids.map { id ->
        async {
            api.getUser(id)
        }
    }.awaitAll()
}
```

`awaitAll()` зберігає порядок deferred list і fail-fast повертає failure.

Залежні calls виконують послідовно:

```kotlin
val token = api.login()
val profile = api.getProfile(token)
```

### Partial success

Для partial success використовують `supervisorScope` та typed result кожного child:

```kotlin
supervisorScope {
    val user = async {
        runRequest { api.getUser() }
    }
    val posts = async {
        runRequest { api.getPosts() }
    }

    PartialData(
        user = user.await(),
        posts = posts.await()
    )
}
```

`runRequest` має передавати `CancellationException` далі та мапити лише очікувані failures. `null` зазвичай гірший за typed partial result.

### Timeout

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

Coroutine timeout скасовує scope. HTTP client окремо потребує connect/read/call timeouts.

### Limit in-flight requests

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

`Semaphore` обмежує in-flight requests. `limitedParallelism()` обмежує dispatcher execution, але не еквівалентний network concurrency limit.

Для великих sets також використовують batching та limits HTTP client-а. Parallelization зазвичай інкапсулює repository/use case, а ViewModel запускає одну screen operation.

### Практичний вибір

- усі results — `coroutineScope + async`;
- список requests — `awaitAll`;
- partial success — supervisor + typed errors;
- великий fan-out — `Semaphore`/batching;
- dependent calls — sequential execution.

**Коротко:** independent requests запускають через `async` у `coroutineScope` і чекають `awaitAll`. Звичайний scope fail-fast, supervisor дає partial results, а fan-out обмежують `Semaphore` або batching.

</details>
<details>
<summary>167. Для чого були придумані Fragment?</summary>

#### Kotlin

`Fragment` були придумані як reusable частини UI і поведінки всередині `Activity`. Вони дозволяють не робити одну величезну Activity, а розбивати екран на менші компоненти з власним lifecycle, layout і state.

1. **Проблема великих Activity**

Без Fragment вся UI-логіка часто жила в Activity:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // navigation, list, details, clicks, state
    }
}
```

З часом така Activity ставала важкою для підтримки й тестування.

2. **Fragment як частина екрана**

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // list UI logic
    }
}
```

Fragment інкапсулює конкретну частину UI: список, деталі, форму, step flow або tabs.

3. **Reuse UI**

Один Fragment можна використати в різних layouts:

```text
phone  -> Activity + ListFragment
tablet -> Activity + ListFragment + DetailsFragment
```

Це було особливо важливо для підтримки phone/tablet UI.

4. **Композиція екрана**

Activity може бути контейнером для кількох Fragment:

```text
Activity
 ├─ HeaderFragment
 ├─ ContentFragment
 └─ DetailsFragment
```

Це дозволяє збирати складні екрани з окремих блоків.

5. **Navigation всередині Activity**

Fragment дозволяють міняти екрани без запуску нової Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Пізніше Navigation Component зробив це зручніше.

6. **Власний lifecycle**

Fragment має lifecycle і окремий View lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Це дозволяє керувати ресурсами конкретної частини UI, але також створює ризики leaks, якщо не очищати View references.

7. **Shared state**

Fragment можуть мати власну ViewModel або shared ViewModel з Activity/parent Fragment:

```kotlin
private val viewModel: ProfileViewModel by viewModels()
private val shared: SharedViewModel by activityViewModels()
```

Це корисно для master-detail, wizard flow або shared UI state.

8. **Чому зараз їх менше в Compose**

У Compose UI можна будувати як composable tree без Fragment для кожного screen. Часто Activity host-ить `NavHost`, а destinations — composables. Але Fragment досі потрібні в legacy/hybrid apps, Navigation Component setups, View System і поступових міграціях.

9. **Практичне правило**

Fragment були створені для modular UI у View System: reuse, адаптація під різні екрани, back stack і розділення великої Activity. Але вони не замінюють чисту архітектуру і вимагають дисципліни з lifecycle.

**Коротко:** Fragment придумали, щоб розбивати Activity на reusable UI-блоки з власним lifecycle, підтримувати phone/tablet layouts, navigation всередині Activity і back stack. У сучасному Compose їх роль менша, але у View System і hybrid apps вони досі важливі.

</details>
<details>
<summary>168. Які проблеми вони вирішують?</summary>

#### Kotlin

Fragment дають screen/container abstraction усередині `Activity`: допомагають розділити View-based UI, керувати navigation і lifecycle окремих частин екрана.

1. **Занадто великі Activity**

Без декомпозиції `Activity` швидко стає контейнером для кількох екранів:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // home, details, settings, navigation, state
    }
}
```

Fragment розділяють їх:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

`Activity` лишається host-ом, а business logic належить `ViewModel`/domain layer.

2. **Композиція та різні layouts**

Одні UI-блоки можна по-різному комбінувати:

```text
phone  -> Activity + один Fragment
tablet -> Activity + list Fragment + details Fragment
```

Наприклад, phone показує list і details послідовно, а tablet — одночасно. Водночас reuse цілого Fragment часто створює сильнішу залежність, ніж reuse окремого View або composable.

3. **Навігація і back stack**

Fragment дозволяють міняти частини екрана без запуску нової Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Navigation Component керує такими transitions і back stack декларативніше.

4. **Lifecycle UI**

Fragment має власний lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Fragment і його View мають різні lifecycles. View bindings, adapters і UI subscriptions треба очищати в `onDestroyView()`, а не чекати `onDestroy()` Fragment.

5. **Вкладені UI-блоки**

Child fragments можуть складати один екран:

```text
Activity
 ├─ HeaderFragment
 ├─ ContentFragment
 └─ BottomSheetFragment
```

Надмірна вкладеність ускладнює lifecycle, state restoration і back stack.

6. **Ізоляція відповідальності**

Fragment відповідає за rendering, lifecycle та UI events конкретного screen, а state і business logic — за `ViewModel`/use cases:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

7. **Що Fragment не вирішують**

Fragment самі по собі не вирішують:

- погану архітектуру;
- змішування UI і бізнес-логіки;
- неправильний state management;
- memory leaks після `onDestroyView()`;
- заплутаний navigation graph.

Для цього потрібні state ownership, lifecycle-aware APIs та чіткі architectural boundaries.

8. **Практичне правило**

У View System Fragment зручний як screen/container. У повністю Compose UI його роль часто зменшується до navigation host або зникає.

**Коротко:** Fragment розділяють View-based UI на lifecycle-aware screens, підтримують navigation і різні layouts. Вони не замінюють state management чи архітектуру, а надмірна вкладеність додає складності.

</details>
<details>
<summary>169. Які особливості вкладених Fragment?</summary>

#### Kotlin

Вкладений Fragment розміщується всередині іншого Fragment і керується через `childFragmentManager`:

```kotlin
class ParentFragment : Fragment(R.layout.fragment_parent) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        childFragmentManager.beginTransaction()
            .replace(R.id.childContainer, ChildFragment())
            .commit()
    }
}
```

`parentFragmentManager` керує Fragment на рівні батьківського контейнера, а не дочірніми Fragment поточного екрана.

### Lifecycle і ViewModel

Child Fragment має власні lifecycle callbacks, view lifecycle, state та ViewModel, але не може існувати довше за parent. Scope ViewModel залежить від потрібного власника:

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()` — scope child Fragment;
- `requireParentFragment()` — спільний стан із parent;
- `activityViewModels()` — scope Activity.

### Navigation і back stack

Child Fragment може мати власний `NavHostFragment` і back stack. Тому треба явно визначити, який стек обробляє Back:

```kotlin
childFragmentManager.popBackStack()
```

Для складних сценаріїв використовують Navigation Component або `OnBackPressedDispatcher`. Вкладену навігацію не варто додавати без реальної потреби: кілька незалежних стеків ускладнюють поведінку екрана.

### Комунікація та cleanup

Не слід зберігати прямі references між Fragment. Для передачі даних підходять shared ViewModel, Fragment Result API або navigation result:

```kotlin
childFragmentManager.setFragmentResultListener("key", viewLifecycleOwner) { _, bundle ->
    val value = bundle.getString("value")
}
```

Оскільки parent і child мають окремі view lifecycle, references на view, adapter і callbacks треба очищати в `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

У `ViewPager2` адаптер усередині Fragment треба прив'язувати до Fragment, а не Activity:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment) {
    override fun getItemCount(): Int = 3
    override fun createFragment(position: Int): Fragment = TabFragment.newInstance(position)
}
```

Вкладені Fragment доречні для tabs, локального wizard flow або незалежного блоку зі своїм lifecycle. Для простого UI краще custom View чи composable.

**Коротко:** вкладені Fragment керуються через `childFragmentManager` і мають власні lifecycle, ViewModel scope та back stack. Основні ризики — неправильний FragmentManager, зайва вкладена навігація та leaks після `onDestroyView()`.

</details>
<details>
<summary>170. Які є виклики (callbacks) у Application?</summary>

#### Kotlin

`Application` представляє Android process. Кожен process застосунку має власний instance.

Основні callbacks:

```text
onCreate()
onTrimMemory()
onLowMemory()
onConfigurationChanged()
onTerminate()
```

### onCreate

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        initLogging()
        initCrashReporting()
    }
}
```

Тут запускають app-wide infrastructure: DI, logging, crash reporting і SDK. Callback виконується на main thread та впливає на cold start, тому initialization має бути мінімальною або lazy.

`Application.onCreate()` не завжди найперший callback: ContentProvider-based initializers можуть запуститися раніше.

У manifest:

```xml
<application android:name=".App" />
```

Для Hilt class позначають `@HiltAndroidApp`.

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

Це signals звільнити recreatable memory: image cache, precomputed data, buffers. Critical persisted state видаляти не можна. Business logic не повинна залежати від memory callback.

### Configuration

```kotlin
override fun onConfigurationChanged(
    newConfig: Configuration
) {
    super.onConfigurationChanged(newConfig)
}
```

Callback повідомляє про app-wide configuration changes. UI reaction краще залишити Activity/Fragment/Compose, де є актуальний lifecycle та themed Context.

### ActivityLifecycleCallbacks

Application може слухати lifecycle усіх Activity:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityStarted(
            activity: Activity
        ) = Unit

        override fun onActivityResumed(
            activity: Activity
        ) = Unit

        override fun onActivityStopped(
            activity: Activity
        ) = Unit

        // other callbacks omitted
    }
)
```

Застосування: analytics, session tracking і debug tooling. Для foreground/background state часто використовують `ProcessLifecycleOwner`.

Не можна зберігати strong reference на Activity після callback-а.

### onTerminate

`onTerminate()` не є production cleanup: Android може завершити process без його виклику.

Тут не можна покладатися на:

- збереження data;
- завершення transaction;
- logout;
- guaranteed resource release.

State зберігають у момент зміни, а resources очищають у lifecycle їхнього owner-а.

### Чого не робити

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

Це створює Activity leak, global mutable state та втрату data після process death. Application не повинен бути service locator або session storage.

Application Context використовують лише для dependencies без UI theme/window lifecycle.

**Коротко:** `onCreate()` виконує швидку app-wide initialization. `onTrimMemory/onLowMemory` звільняють recreatable memory, `onConfigurationChanged` повідомляє про config, `onTerminate` не гарантується. Lifecycle Activity слухають через `ActivityLifecycleCallbacks`.

</details>
<details>
<summary>171. Який singleton створює система під час запуску Android-додатка?</summary>

#### Kotlin

Під час запуску Android-додатка система створює екземпляр `Application` для процесу додатка. Це application-level singleton, який живе протягом життя процесу і створюється до `Activity`, `Service`, `BroadcastReceiver` та інших компонентів у цьому процесі.

1. **Application як singleton процесу**

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
    }
}
```

У manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Коли процес стартує, Android створює екземпляр `App` і викликає `onCreate()`.

2. **Singleton тільки в межах процесу**

`Application` не є singleton на весь пристрій або навіть на весь app у multi-process сценарії. Він один лише в конкретному процесі:

```text
app process
 └── Application instance
```

Якщо є окремий процес:

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

то буде два instances:

```text
main process  -> App instance #1
:sync process -> App instance #2
```

Тому `Application`, Kotlin `object` і static state не можна вважати shared storage між процесами.

3. **Для чого використовують Application**

Типові задачі:

- dependency injection setup;
- crash reporting;
- logging;
- analytics setup;
- app-wide SDK initialization;
- реєстрація `ActivityLifecycleCallbacks`;
- легка глобальна конфігурація.

Для Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` запускає генерацію application-level dependency graph.

4. **Application context**

Для long-lived залежностей треба використовувати `applicationContext`, а не `Activity` context:

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

Це зменшує ризик memory leak.

5. **Чого не робити**

Погано:

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Проблеми:

- state зникне після process death;
- `Activity` reference може створити memory leak;
- business logic стає глобальною;
- тестування ускладнюється.

`Application` не має бути global mutable storage і не замінює DI container.

6. **Process death**

Якщо система вбила процес, `Application` буде створений заново при наступному старті:

```text
process killed -> user opens app -> new process -> new Application instance
```

Критичний state треба зберігати в database, DataStore, SharedPreferences, files або backend.

7. **Application vs Kotlin object**

```kotlin
object SessionHolder {
    var token: String? = null
}
```

Kotlin `object` створюється lazy при першому зверненні. `Application` створюється Android framework і має `Context`/lifecycle процесу. Обидва живуть тільки в межах процесу й обидва втрачаються при process death.

**Коротко:** Android при старті процесу створює singleton `Application`. Він один у межах процесу, підходить для легкої глобальної ініціалізації, але не є надійним сховищем стану і не повинен тримати UI-обʼєкти.

</details>
<details>
<summary>172. Як працює BackStack?</summary>

#### Kotlin

`BackStack` — це стек екранів або navigation destinations, який визначає, куди користувач повернеться при натисканні Back. Принцип простий: останній доданий екран виходить першим — LIFO.

1. **Базова ідея**

```text
Home -> Details -> Settings
```

Якщо натиснути Back:

```text
Settings removed -> Details visible
Details removed  -> Home visible
```

2. **Activity back stack**

Коли запускається нова Activity, вона додається в task back stack:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Back завершує поточну Activity і повертає попередню.

3. **Fragment back stack**

Fragment transaction можна додати в back stack:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Якщо не викликати `addToBackStack`, Back не поверне попередній Fragment, а transaction просто замінить UI.

4. **Navigation Component**

У Navigation Component back stack складається з destinations:

```kotlin
navController.navigate("details/$id")
navController.popBackStack()
```

`popBackStack()` прибирає поточну destination і повертає попередню.

5. **popUpTo**

`popUpTo` дозволяє прибрати частину back stack:

```kotlin
navController.navigate("home") {
    popUpTo("login") { inclusive = true }
}
```

Це корисно після login: користувач не має повернутись на login screen через Back.

6. **inclusive**

```text
inclusive = false -> destination popUpTo залишається
inclusive = true  -> destination popUpTo теж видаляється
```

Це важливо для auth flow, onboarding, checkout success screen.

7. **Compose Navigation**

У Compose Navigation принцип той самий:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen() }
    composable("details/{id}") { DetailsScreen() }
}
```

Back stack керується `NavController`.

8. **SingleTop**

Щоб не створювати дублікати destination:

```kotlin
navController.navigate("home") {
    launchSingleTop = true
}
```

Це корисно для bottom navigation або повторних кліків по тому самому tab.

9. **Bottom navigation**

Для bottom navigation часто потрібно зберігати state кожного tab:

```kotlin
navController.navigate(route) {
    popUpTo(navController.graph.startDestinationId) {
        saveState = true
    }
    launchSingleTop = true
    restoreState = true
}
```

Так користувач повертається в tab із попереднім scroll/state.

10. **Типові помилки**

- не використовувати `popUpTo` після auth/onboarding;
- дублювати destinations у stack;
- змішувати manual Fragment transactions і NavController;
- неправильно обробляти Back у nested graphs;
- не розуміти різницю між Activity task stack і NavController back stack.

11. **Практичне правило**

BackStack має відображати очікувану історію користувача. Після irreversible flow — чистити stack. Для tabs — зберігати state. Для modal UI — спочатку закривати modal, потім робити navigation back.

**Коротко:** BackStack — це стек navigation history. Back прибирає поточний екран і повертає попередній. В Android є Activity task back stack, Fragment back stack і NavController back stack. Керують ним через `navigate`, `popBackStack`, `popUpTo`, `inclusive`, `launchSingleTop`, `saveState` і `restoreState`.

</details>
<details>
<summary>173. У яких випадках можна отримати ANR (Application Not Responding)?</summary>

#### Kotlin

ANR виникає, коли process не відповідає на важливу системну подію вчасно. Найчастіше main thread заблокований, перевантажений або чекає інший thread/process.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // blocks input and rendering
}
```

Main thread обробляє input, lifecycle, частину Binder callbacks і rendering. ANR може спричинити як одна довга операція, так і велика черга коротких.

### Типові причини

**Важка робота на Main:**

- synchronous network/disk I/O або database query;
- parsing, bitmap decoding, sorting чи cryptography;
- `runBlocking`, `Thread.sleep()`, `Future.get()` або `join()`;
- важкі lifecycle, View чи Compose callbacks.

I/O виконують на `Dispatchers.IO`, CPU-bound роботу — на `Dispatchers.Default`. Позначка `suspend` сама по собі не переносить blocking code з main thread.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

**Компоненти Android:**

`BroadcastReceiver.onReceive()`, `Service.onCreate()` і `onStartCommand()` за замовчуванням працюють на main thread; `Service` не створює worker thread. Довгу гарантовану роботу передають у `WorkManager`. `goAsync()` лише тимчасово продовжує lifetime receiver-а й вимагає `finish()`.

**Locks і IPC:**

Main може чекати lock, latch або synchronous Binder call. Класичний deadlock: worker чекає Main, а Main — worker.

**Повільний startup:**

Важка ініціалізація в `Application`, `ContentProvider`, DI graph або першій Activity може заблокувати startup.

### Jank та ANR

Повільний frame — це jank, не обов’язково ANR. Але довга серія дорогих layout/draw/recomposition операцій може зрештою заблокувати Main достатньо надовго.

### Діагностика

Спочатку аналізують stack main thread і threads, на які він чекає:

- Play Console Android vitals та ANR traces;
- Perfetto/System Trace і CPU Profiler;
- Logcat та власні timing metrics;
- StrictMode у debug build.

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

StrictMode знаходить частину main-thread I/O, але не замінює traces. Stack Main може показувати лише очікування lock, тоді реальну причину шукають у його owner thread.

### Профілактика

- не блокувати Main і не тримати lock під час I/O;
- додавати timeouts до network та IPC;
- відкладати некритичну startup-ініціалізацію;
- розбивати довгу CPU work і контролювати частоту UI events;
- профілювати startup, scrolling і масові updates.

**Коротко:** ANR спричиняють blocking I/O, CPU-heavy work, synchronous waits/IPC, locks, довгі callbacks або startup. Діагностику починають зі stack main thread та system trace.

</details>
<details>
<summary>174. Що можна виконувати в main thread?</summary>

#### Kotlin

`Main thread` в Android відповідає за UI: input events, lifecycle callbacks, rendering, View/Compose updates. На ньому можна виконувати тільки швидкі операції, які не блокують frame rendering і не створюють ANR.

1. **Що має виконуватись на main thread**

На main thread виконують:

- оновлення UI;
- роботу з View hierarchy;
- Compose state reads/writes для UI;
- lifecycle callbacks;
- click listeners;
- navigation calls;
- показ dialog/snackbar/toast;
- коротку lightweight logic.

Приклад:

```kotlin
button.setOnClickListener {
    viewModel.onLoginClick()
}
```

2. **UI updates**

```kotlin
textView.text = "Loaded"
progressBar.isVisible = false
```

View system не thread-safe, тому View треба оновлювати з main thread.

3. **Compose**

У Compose UI state зазвичай collect-иться і рендериться на main thread:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
ProfileScreen(state)
```

Composable має бути легким і не виконувати I/O або важкі обчислення.

4. **Що не можна робити на main thread**

Не можна блокувати main thread такими операціями:

- network requests;
- database queries великого обсягу;
- file I/O;
- bitmap decoding;
- JSON parsing великих payloads;
- heavy sorting/filtering;
- `Thread.sleep`;
- `runBlocking`;
- довгі synchronized blocks.

5. **Чому це небезпечно**

Main thread має вкладатися приблизно в 16ms на frame для 60fps. Якщо він зайнятий довгою операцією, UI лагає. Якщо не відповідає занадто довго — система може показати ANR.

6. **Перенесення роботи в background**

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

I/O — в `Dispatchers.IO`, CPU-heavy — в `Dispatchers.Default`.

7. **Коротка логіка допустима**

На main thread нормально робити прості перевірки:

```kotlin
if (email.isBlank()) {
    showEmailError()
    return
}
```

Головне — щоб ця логіка була швидкою і не блокувала UI.

8. **StrictMode**

Для пошуку випадкового I/O на main thread можна увімкнути StrictMode:

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

9. **Практичне правило**

На main thread має бути тільки UI і коротка orchestration logic. Усе, що може зайняти непередбачувано довго, треба винести в background dispatcher або окремий async API.

**Коротко:** в main thread можна виконувати UI updates, lifecycle callbacks, navigation і коротку lightweight logic. Не можна робити network, важкий I/O, великі DB/JSON/bitmap операції, `runBlocking`, `Thread.sleep` або довгі CPU tasks. Для цього використовують `Dispatchers.IO` або `Dispatchers.Default`.

</details>
<details>
<summary>175. Які є базові компоненти Android?</summary>

#### Kotlin

Чотири application components, які знає Android system: `Activity`, `Service`, `BroadcastReceiver` і `ContentProvider`. Fragment, ViewModel та WorkManager — важливі Jetpack abstractions, але не базові system components.

1. **Activity**

`Activity` — UI entry point із власними window та lifecycle:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Вона отримує `Intent` і може host-ити Compose UI або Fragment.

2. **Service**

`Service` — component без власного UI:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Service callbacks за замовчуванням працюють на main thread. Для ongoing user-visible work потрібен foreground service; deferrable persistent work зазвичай належить WorkManager.

3. **BroadcastReceiver**

`BroadcastReceiver` отримує system/app broadcasts:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // handle event
    }
}
```

`onReceive()` має швидко завершитися; тривалу persistent work делегують WorkManager.

4. **ContentProvider**

`ContentProvider` надає структуровані дані через URI:

```text
content://com.example.provider/users/1
```

Його використовують для controlled inter-process data sharing; системні приклади — contacts і media providers.

5. **Intent**

`Intent` запускає Activity/Service або доставляє broadcast:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

6. **Manifest**

System components оголошуються в `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

Manifest також задає permissions, intent filters та `exported` behavior. Динамічний receiver можна реєструвати runtime.

7. **Jetpack abstractions**

- Fragment — частина View-based UI всередині Activity;
- ViewModel — owner UI state, який переживає configuration changes;
- WorkManager — scheduler persistent deferrable work.

8. **Практичний вибір**

- UI entry — Activity;
- ongoing user-visible work — foreground Service;
- system/app event — BroadcastReceiver;
- inter-process data API — ContentProvider;
- deferrable persistent work — WorkManager.

**Коротко:** system components — `Activity`, `Service`, `BroadcastReceiver` і `ContentProvider`. Вони оголошуються в manifest та взаємодіють через Intent/Binder/URI; Fragment, ViewModel і WorkManager належать Jetpack-рівню.

</details>
<details>
<summary>176. Що таке Service?</summary>

#### Kotlin

`Service` — Android component без UI. Він не створює background thread: callbacks за замовчуванням виконуються на main thread у process застосунку.

Service не гарантує переживання process death, force stop або reboot.

### Started Service

```kotlin
class SyncService : Service() {
    private val scope =
        CoroutineScope(
            SupervisorJob() + Dispatchers.IO
        )

    override fun onStartCommand(
        intent: Intent?,
        flags: Int,
        startId: Int
    ): Int {
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

    override fun onBind(
        intent: Intent?
    ): IBinder? = null
}
```

Кожен `startService()` викликає `onStartCommand()`. Service зупиняється через `stopSelf(startId)` або `stopService()`. Blocking роботу треба винести з Main.

`onDestroy()` — best-effort cleanup, але при kill process він може не викликатися.

### Bound Service

Bound service надає API через `IBinder`:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService =
            this@PlayerService
    }

    override fun onBind(
        intent: Intent
    ): IBinder = binder
}
```

Local Binder працює в одному process. Для cross-process IPC використовують Messenger/AIDL.

Bound-only service живе, поки є clients. Якщо він також started, `unbindService()` не зупиняє started lifecycle.

### Foreground Service

Потрібен для довгої user-visible роботи: media playback, navigation, active location або transfer.

```kotlin
startForeground(
    NOTIFICATION_ID,
    createNotification()
)
```

Після `startForegroundService()` треба швидко викликати `startForeground()`. Сучасний Android обмежує background start і вимагає notification, service type та відповідні permissions.

Якщо робота не помітна користувачу, foreground service зазвичай не потрібен.

### Lifecycle і restart

```text
onCreate()       -> створення instance
onStartCommand() -> кожен start
onBind()         -> connection client-а
onDestroy()      -> cleanup
```

Restart flags:

- `START_NOT_STICKY` — не відновлювати;
- `START_STICKY` — відновити без гарантії Intent;
- `START_REDELIVER_INTENT` — повторно доставити Intent.

Operation має бути idempotent: restart policy не робить Service durable queue.

### Що вибрати

```text
user-visible active work -> Foreground Service
reliable deferred work   -> WorkManager
exact-time trigger       -> AlarmManager
media playback           -> Media3 + FGS
screen-bound work        -> ViewModel scope
```

Звичайний Service не є універсальним background executor. Для WorkManager не потрібен Service, а для короткої screen operation — component scope.

**Коротко:** Service — lifecycle component без UI, а не окремий thread. Started service виконує commands, bound service надає Binder API, foreground service показує notification. Для гарантованої відкладеної роботи використовують WorkManager.

</details>
<details>
<summary>177. Що таке BroadcastReceiver?</summary>

#### Kotlin

`BroadcastReceiver` — Android component для отримання broadcast `Intent` від системи або інших components/apps.

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(
        context: Context,
        intent: Intent
    ) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            scheduleSync(context)
        }
    }
}
```

Receiver не зберігає state і не призначений для довгої роботи. Після завершення `onReceive()` process може бути зупинений.

### Manifest receiver

```xml
<receiver
    android:name=".BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name=
            "android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

Для `BOOT_COMPLETED` потрібен permission `RECEIVE_BOOT_COMPLETED`. Manifest receiver може працювати без запущеного UI, але сучасний Android обмежує багато implicit broadcasts, тому правила треба перевіряти для конкретного action/API.

`android:exported` визначає, чи доступний component іншим apps. Зовнішній receiver потребує validation Intent data та permissions; internal receiver не слід export-ити.

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

Registration і unregister мають бути симетричними. Lifecycle pair залежить від потреби: visible screen — `onStart/onStop`, lifetime Activity — `onCreate/onDestroy`.

На сучасних API для dynamic receiver часто треба явно вказати `RECEIVER_EXPORTED` або `RECEIVER_NOT_EXPORTED`.

### Довга робота

`onReceive()` зазвичай виконується на main thread і має швидко завершитися. Для гарантованої deferrable роботи використовують WorkManager:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    val request =
        OneTimeWorkRequestBuilder<SyncWorker>().build()

    WorkManager.getInstance(context)
        .enqueueUniqueWork(
            "sync",
            ExistingWorkPolicy.KEEP,
            request
        )
}
```

Для негайної user-visible роботи може знадобитися foreground service з урахуванням background-start restrictions.

`goAsync()` дозволяє коротко продовжити обробку поза `onReceive()`, але вимагає обов'язкового `PendingResult.finish()`. Він не замінює WorkManager і не гарантує process survival для довгої operation.

### Внутрішня комунікація

`LocalBroadcastManager` deprecated. Усередині process зазвичай краще Flow/StateFlow, callbacks, repository state або navigation result.

BroadcastReceiver доречний для:

- system events;
- alarms;
- boot/package changes;
- чіткої міжкомпонентної або міжзастосункової взаємодії.

**Коротко:** BroadcastReceiver приймає broadcast Intent і має швидко завершити `onReceive()`. Dynamic registration прив'язують до lifecycle, manifest receiver враховує implicit restrictions та `exported`, а гарантовану довгу роботу передають WorkManager.

</details>
<details>
<summary>178. Що таке Context і які його типи існують?</summary>

#### Kotlin

`Context` — точка доступу до Android environment: resources, system services, package info, files і запуску components. Конкретний context також визначає theme та lifecycle boundary.

1. **Для чого потрібен Context**

```kotlin
context.getString(R.string.app_name)
context.getSystemService(Context.CONNECTIVITY_SERVICE)
context.startActivity(intent)
context.getSharedPreferences("settings", Context.MODE_PRIVATE)
```

2. **Application Context**

```kotlin
val appContext = context.applicationContext
```

Application context живе разом із process і не має theme конкретної Activity. Він підходить для DB, DataStore та інших long-lived dependencies, яким не потрібен UI context.

3. **Activity Context**

Activity context має theme, window і lifecycle конкретної Activity. Він потрібен для dialogs, themed views та UI operations; long-lived object не повинен його зберігати.

4. **Context у Fragment**

`Fragment` не є `Context`; він повертає context свого host-а:

```kotlin
val context = requireContext()
val activityContext = requireActivity()
```

До attach `context` може бути `null`, а після detach `requireContext()` кине exception. View references треба очищати в `onDestroyView()`.

5. **Service Context**

`Service` є `Context`, прив’язаним до lifetime service. Він підходить для service operations, але не має Activity window/theme semantics.

6. **ContextWrapper**

`ContextWrapper` делегує base context і може змінювати окрему поведінку:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

`ContextThemeWrapper` додає theme override для UI inflation.

7. **Context у Compose**

У Compose context отримують так:

```kotlin
val context = LocalContext.current
```

Це context поточного composition environment. Його використовують для UI/resource actions, але не зберігають без урахування lifecycle.

8. **Memory leaks**

Погано:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

Singleton із reference на Activity не дасть звільнити її після recreation.

Краще:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

У factory передають `context.applicationContext`.

9. **Вибір context**

```text
Application context -> long-living dependencies, DB, DataStore
Activity context    -> UI, dialogs, themed views, start activity
Fragment context    -> host context, поки Fragment attached
Service context     -> service operations
Compose LocalContext -> UI/resource actions у composable
```

**Коротко:** `Context` дає доступ до Android environment. Long-lived dependencies зазвичай використовують application context, UI — Activity/themed context, service operations — Service context. Fragment сам не є `Context`.

</details>
<details>
<summary>179. Що таке Bundle?</summary>

#### Kotlin

`Bundle` — це key-value контейнер Android для передачі невеликих даних між компонентами або для збереження UI state. Він часто використовується з `Intent`, Fragment arguments, `onSaveInstanceState`, Navigation і Activity Result API.

1. **Базова ідея**

```kotlin
val bundle = Bundle().apply {
    putString("user_id", userId)
    putInt("tab", 2)
}
```

Дані дістають за тим самим ключем:

```kotlin
val userId = bundle.getString("user_id")
```

2. **Intent extras**

```kotlin
val intent = Intent(this, DetailsActivity::class.java).apply {
    putExtra("user_id", userId)
}
startActivity(intent)
```

У Activity:

```kotlin
val userId = intent.getStringExtra("user_id")
```

3. **Fragment arguments**

```kotlin
class DetailsFragment : Fragment(R.layout.fragment_details) {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val userId = requireArguments().getString("user_id")
    }
}
```

Створення:

```kotlin
val fragment = DetailsFragment().apply {
    arguments = bundleOf("user_id" to userId)
}
```

4. **onSaveInstanceState**

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

Підходить для невеликого UI state, який треба відновити після configuration change або process recreation.

5. **Які типи можна зберігати**

Bundle підтримує:

- primitives;
- String;
- arrays;
- Parcelable;
- Serializable;
- Bundle;
- ArrayList деяких типів.

Для Android краще `Parcelable`, ніж Java `Serializable`, бо він ефективніший.

6. **Parcelable приклад**

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

```kotlin
bundle.putParcelable("user", userArgs)
```

Але в navigation часто краще передавати тільки `id`, а дані завантажувати з repository.

7. **Обмеження розміру**

Bundle не призначений для великих обʼєктів. Якщо передати занадто багато даних, можна отримати `TransactionTooLargeException`.

Погано:

```kotlin
bundle.putParcelableArrayList("huge_list", hugeList)
```

Краще передати id або key до даних у DB/cache.

8. **Bundle vs ViewModel**

```text
Bundle    -> маленький serializable UI/navigation state
ViewModel -> screen state і business state під час lifecycle
Database  -> persistent data
```

Bundle не має бути основним сховищем даних.

9. **Type safety**

Ключі у Bundle — строки, тому легко помилитись. Для Navigation краще Safe Args або typed route arguments, якщо доступно.

10. **Практичне правило**

- Передавати через Bundle тільки невеликі дані.
- Для screen navigation краще передавати id.
- Не класти великі списки/bitmap/payloads.
- Для custom object — `Parcelable`.
- Для state екрана — ViewModel/SavedStateHandle.

**Коротко:** Bundle — Android key-value контейнер для невеликих даних між components і для збереження простого UI state. Він використовується з Intent, Fragment arguments і saved instance state. Не треба класти в Bundle великі обʼєкти; краще передавати id і відновлювати дані з repository/cache.

</details>
<details>
<summary>180. Чи можна вкладати Bundle у Bundle?</summary>

#### Kotlin

Так, `Bundle` можна вкладати в інший `Bundle`. Android `Bundle` підтримує збереження вкладених `Bundle` через `putBundle()` і читання через `getBundle()`.

1. **Базовий приклад**

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

Отримання:

```kotlin
val address = userBundle.getBundle("address")
val city = address?.getString("city")
```

2. **Коли це корисно**

Вкладений `Bundle` може бути корисним, коли дані логічно згруповані:

```text
user
 ├─ id
 ├─ name
 └─ address
     ├─ city
     └─ street
```

Але для складних структур часто краще `Parcelable` або передача id.

3. **Fragment arguments**

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

Це працює, але може стати менш читабельним, якщо структура глибока.

4. **onSaveInstanceState**

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putBundle("filters", bundleOf("query" to query))
    super.onSaveInstanceState(outState)
}
```

Підходить для невеликого UI state.

5. **Обмеження розміру**

Bundle передається через Binder у багатьох сценаріях. Якщо покласти занадто багато даних, можна отримати `TransactionTooLargeException`.

Погано:

```kotlin
bundle.putBundle("huge_data", hugeNestedBundle)
```

Краще передати id і відновити дані з repository/cache.

6. **Type safety**

Bundle працює зі string keys, тому легко помилитися:

```kotlin
getBundle("adress") // typo
```

Для navigation краще використовувати typed arguments, Safe Args або власний args class.

7. **Bundle vs Parcelable**

Якщо структура має чітку модель, краще:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

`Parcelable` читабельніший і типобезпечніший за глибоко вкладені Bundle.

8. **Практичне правило**

Вкладати `Bundle` у `Bundle` можна, але тільки для невеликих і простих структур. Для складних обʼєктів — `Parcelable`, для великих даних — id + repository/database.

**Коротко:** так, `Bundle` можна вкладати в `Bundle` через `putBundle()`/`getBundle()`. Це нормально для невеликих grouped arguments або saved state, але не варто передавати великі чи глибоко вкладені структури. Для складних моделей краще `Parcelable`, для великих даних — передавати id.

</details>
<details>
<summary>181. Що таке Permissions в Android?</summary>

#### Kotlin

Permissions контролюють доступ app до ресурсів поза sandbox: camera, microphone, location, contacts і notifications.

```text
normal    -> автоматично
dangerous -> manifest + runtime request
special   -> окремий Settings flow
signature -> apps із відповідним certificate
```

### Runtime flow

```xml
<uses-permission
    android:name="android.permission.CAMERA" />
```

Dangerous permission треба ще запросити runtime:

```kotlin
val cameraPermission =
    rememberLauncherForActivityResult(
        ActivityResultContracts.RequestPermission()
    ) { granted ->
        if (granted) openCamera()
        else showPermissionDenied()
    }
```

Перед protected API перевіряють поточний grant:

```kotlin
val granted =
    ContextCompat.checkSelfPermission(
        context,
        Manifest.permission.CAMERA
    ) == PackageManager.PERMISSION_GRANTED
```

Permission може бути відкликаний у Settings або auto-reset, тому його не зберігають як постійний boolean.

### Rationale і denial

`shouldShowRequestPermissionRationale()` допомагає вирішити, чи показати пояснення перед повторним request.

Rationale має пояснювати користь feature, а не тиснути. `false` не завжди означає permanent denial: це також стан до першого request. App має враховувати власну історію запиту.

Denial — нормальний сценарій. Optional feature повинна мати fallback; якщо permission обов'язковий, треба пояснити наслідок і за потреби запропонувати Settings.

### One-time та location

Camera, microphone і location можуть бути надані одноразово. Після revoke доступ зникає, а process може бути завершений. Resource відкривають лише після check і звільняють за lifecycle.

Location має окремі рівні:

- coarse/fine;
- approximate/precise;
- foreground/background.

Запитують мінімальний foreground access. Background location потребує окремого flow і має бути core use case-ом.

### Notifications

Android 13+ використовує runtime permission:

```xml
<uses-permission
    android:name=
        "android.permission.POST_NOTIFICATIONS" />
```

Запитують перед реальною notification feature. Foreground service можна запустити без grant, але notification створити все одно потрібно; system визначає її видимість.

### Storage без broad permission

Для більшості scenarios краще:

- Photo Picker — вибрані media;
- Storage Access Framework — document/directory;
- MediaStore — shared media;
- app-specific storage — файли app.

Це дає least privilege і не відкриває всю media library.

### Special permissions

Overlay, exact alarm, all-files access та інші special access використовують окремий Settings screen. Після повернення capability перевіряють знову.

Play policy може обмежувати sensitive permission навіть якщо platform API технічно його підтримує.

### Практичні правила

- запитувати access у момент feature action;
- просити мінімальний permission;
- не запитувати все на startup;
- перевіряти grant перед кожним використанням;
- підтримувати denial, revoke та one-time;
- використовувати Picker/SAF замість broad storage;
- тестувати approximate location і process recreation.

**Коротко:** dangerous permissions оголошують у manifest і запитують runtime через Activity Result API. Grant може бути одноразовим або відкликаним. Запитуйте мінімальний доступ у контексті feature, а для media/files використовуйте Picker, SAF або MediaStore.

</details>
<details>
<summary>182. Що таке setContentView?</summary>

#### Kotlin

`setContentView()` — це метод Activity, який встановлює UI layout для екрана. У View System він зазвичай inflate-ить XML layout і привʼязує його до window Activity.

1. **Базовий приклад**

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Після цього Activity має root View hierarchy з `activity_main.xml`.

2. **Що відбувається під капотом**

Спрощено:

```text
XML layout -> inflate -> View hierarchy -> attach to Activity content root
```

Android читає XML, створює відповідні `View` обʼєкти і додає їх у content container Activity.

3. **setContentView з View**

Можна передати готовий View:

```kotlin
val textView = TextView(this).apply {
    text = "Hello"
}
setContentView(textView)
```

Але в production частіше використовують XML, ViewBinding або Compose.

4. **ViewBinding**

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)
}
```

ViewBinding дає type-safe доступ до View без `findViewById`.

5. **findViewById після setContentView**

```kotlin
setContentView(R.layout.activity_main)
val title = findViewById<TextView>(R.id.title)
```

`findViewById` працює після того, як layout уже встановлений.

6. **Compose альтернатива**

У Compose замість XML `setContentView` часто використовують `setContent`:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            AppTheme {
                MainScreen()
            }
        }
    }
}
```

Це створює Compose UI tree замість XML View hierarchy.

7. **Fragment відмінність**

У Fragment зазвичай не викликають `setContentView`. Fragment має свій layout через constructor або `onCreateView`:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

`setContentView` — це Activity-level API.

8. **Типові помилки**

- викликати `findViewById` до `setContentView`;
- викликати `setContentView` кілька разів без потреби;
- змішувати XML і Compose без чіткої структури;
- тримати Activity View references у singleton;
- робити важку логіку в `onCreate` після inflate.

9. **Практичне правило**

У View System Activity встановлює layout через `setContentView`. У сучасному Android з XML краще використовувати ViewBinding. У Compose app — `setContent`. Fragment layout керується через Fragment lifecycle, а не `setContentView`.

**Коротко:** `setContentView()` встановлює root UI для Activity: inflate-ить XML або приймає готовий View і додає його в content area. Для XML часто використовують ViewBinding + `setContentView(binding.root)`, а в Compose замість цього застосовують `setContent { ... }`.

</details>
<details>
<summary>183. Які методи існують у View?</summary>

#### Kotlin

`View` — базовий клас Android UI. Для custom View важливо розуміти lifecycle та pipeline `measure → layout → draw`.

### Lifecycle

```kotlin
override fun onAttachedToWindow() {
    super.onAttachedToWindow()
}

override fun onDetachedFromWindow() {
    stopAnimation()
    removeListeners()
    super.onDetachedFromWindow()
}
```

`onAttachedToWindow()` викликається після приєднання до window, `onDetachedFromWindow()` — під час від'єднання. Тут запускають і зупиняють ресурси, прив'язані до UI lifecycle.

### Measure, layout, draw

`onMeasure()` визначає розмір View відповідно до `MeasureSpec` від parent:

```kotlin
override fun onMeasure(widthSpec: Int, heightSpec: Int) {
    val width = resolveSize(200, widthSpec)
    val height = resolveSize(100, heightSpec)
    setMeasuredDimension(width, height)
}
```

Режими `MeasureSpec`: `EXACTLY`, `AT_MOST`, `UNSPECIFIED`.

`onLayout()` у `ViewGroup` задає позиції child views. Для звичайного `View` його рідко перевизначають.

`onDraw()` малює вміст на `Canvas`:

```kotlin
override fun onDraw(canvas: Canvas) {
    canvas.drawCircle(width / 2f, height / 2f, radius, paint)
}
```

У `onDraw()` не можна виконувати network/file I/O, bitmap decoding чи створювати багато об'єктів: метод може викликатися часто.

### invalidate і requestLayout

```text
змінився лише вигляд       -> invalidate()
змінився розмір або layout -> requestLayout()
```

`invalidate()` планує перемальовування, а `requestLayout()` запускає новий measure/layout pass і зазвичай подальший draw.

### Touch і click

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

Щоб отримувати наступні події gesture, View зазвичай має прийняти `ACTION_DOWN`. Click треба проводити через `performClick()`, оскільки це важливо для listeners та accessibility.

### Стан і допоміжні методи

- `VISIBLE` — видно й займає місце;
- `INVISIBLE` — не видно, але займає місце;
- `GONE` — не видно й не займає місце;
- `requestFocus()` / `clearFocus()` — керування focus;
- `onSaveInstanceState()` / `onRestoreInstanceState()` — власний state;
- `post { }` — виконання коду в UI queue, часто після поточного layout pass;
- `setOnClickListener()` / `setOnLongClickListener()` — події користувача.

Listeners виконуються на main thread, тому довгу роботу в callback робити не можна. Для accessibility треба задавати коректні `contentDescription`, focus і click actions.

**Коротко:** ключові методи `View` — `onMeasure()`, `onLayout()`, `onDraw()`, `invalidate()`, `requestLayout()`, `onTouchEvent()`, `performClick()` та lifecycle callbacks. Вони керують розміром, розміщенням, малюванням, input і ресурсами View.

</details>
<details>
<summary>184. Що таке ViewGroup?</summary>

#### Kotlin

`ViewGroup` — `View`, який містить children і визначає їх measurement, placement, drawing order та touch routing.

```text
View      -> один UI node
ViewGroup -> View + children + layout policy
```

`LinearLayout`, `FrameLayout`, `ConstraintLayout`, `RecyclerView` — ViewGroup implementations.

### onMeasure

ViewGroup отримує constraints від parent, вимірює visible children та визначає власний size:

```kotlin
override fun onMeasure(
    widthSpec: Int,
    heightSpec: Int
) {
    var totalHeight =
        paddingTop + paddingBottom
    var maxWidth = 0

    for (index in 0 until childCount) {
        val child = getChildAt(index)
        if (child.visibility == GONE) continue

        measureChild(
            child,
            widthSpec,
            heightSpec
        )

        totalHeight += child.measuredHeight
        maxWidth =
            maxOf(maxWidth, child.measuredWidth)
    }

    setMeasuredDimension(
        resolveSize(
            maxWidth + paddingLeft + paddingRight,
            widthSpec
        ),
        resolveSize(totalHeight, heightSpec)
    )
}
```

Production implementation також враховує margins, minimum size, measured state та `MeasureSpec` modes. Child size не може порушувати constraints parent-а.

### onLayout

```kotlin
override fun onLayout(
    changed: Boolean,
    l: Int,
    t: Int,
    r: Int,
    b: Int
) {
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

`child.layout()` задає final bounds. Coordinates локальні відносно ViewGroup.

### LayoutParams

ViewGroup визначає supported per-child parameters. Наприклад:

- LinearLayout — `weight`;
- ConstraintLayout — constraints;
- FrameLayout — `gravity`.

Custom ViewGroup із власними LayoutParams має підтримати їх generation, validation, margins і XML parsing.

### Touch

```text
dispatchTouchEvent()
 -> onInterceptTouchEvent()
 -> child.dispatchTouchEvent()
 -> onTouchEvent()
```

`onInterceptTouchEvent()` дозволяє контейнеру забрати gesture у child, наприклад для scroll. Неправильне interception ламає clicks, scrolling та nested scrolling.

### Drawing та invalidation

ViewGroup малює себе й children через `dispatchDraw()`. Drawing order, clipping та elevation впливають на overlap.

```text
invalidate()    -> redraw
requestLayout() -> новий measure/layout
```

Якщо змінився лише color — `invalidate()`; якщо child size/position — `requestLayout()`. Зайві layout passes спричиняють jank.

### Коли custom ViewGroup

Готові containers або Compose достатні для більшості UI. Custom ViewGroup потрібен для специфічного placement, touch behavior або виміряної performance-причини.

Основні помилки: ігнорувати margins/GONE, вимірювати child кілька разів без потреби, створювати objects у layout, неправильно intercept-ити touch і не підтримувати accessibility.

**Коротко:** ViewGroup — контейнер із layout policy. Він вимірює children в `onMeasure()`, задає bounds в `onLayout()`, керує LayoutParams, drawing і touch routing. Custom implementation має коректно враховувати constraints, margins та layout passes.

</details>
<details>
<summary>185. Для чого потрібен метод onStart() в Activity?</summary>

#### Kotlin

`onStart()` викликається при переході Activity у visible state. UI вже створений, але Activity ще не обов’язково має interaction focus.

```text
first launch: onCreate -> onStart -> onResume
background:   onPause -> onStop
return:       onRestart -> onStart -> onResume
```

Один instance проходить `onStart()` після першого launch і кожного повернення зі `STOPPED`.

### onStart vs onResume

```text
STARTED -> visible
RESUMED -> foreground та interactive
```

Під partially transparent Activity поточна може залишатися `STARTED`, але бути `PAUSED`.

Visible-only resources прив’язують до `onStart/onStop`, interaction-only — до `onResume/onPause`.

### Симетричний lifecycle

Listener/receiver, зареєстрований у `onStart()`, знімають у `onStop()`. Setup має бути idempotent, щоб повторний start не створював duplicate registration.

Тут доречні:

- visible-only listeners/resources;
- visibility analytics;
- preview/sensor із visibility semantics;
- connection до UI-related controller.

Одноразову ініціалізацію Activity роблять у `onCreate()`, а не в `onStart()`.

### Lifecycle-aware Flow

Замість ручного запуску collection у callback використовують lifecycle API:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Block запускається у `STARTED`, скасовується нижче цього state та створюється знову після повернення.

Кілька нескінченних flows усередині block збирають у паралельних child coroutines.

У Fragment collection прив’язують до `viewLifecycleOwner`, щоб завершити її в `onDestroyView()`.

### Чого не робити

`onStart()` працює на main thread: blocking I/O, parsing і `Thread.sleep()` тут неприпустимі.

```kotlin
override fun onStart() {
    super.onStart()
    viewModel.refreshIfNeeded()
}
```

Callback може ініціювати async work. Refresh при кожному start має бути навмисним: recreation або повернення з іншого screen запускає його знову, тому ViewModel/repository контролює freshness і deduplication.

Work, що має пережити Activity, належить WorkManager, foreground service або application owner залежно від вимог.

**Коротко:** `onStart()` означає visibility. Тут запускають lightweight visible-only resources із cleanup у `onStop()`. Flow збирають через `repeatOnLifecycle(STARTED)`, а heavy/persistent work виносять.

</details>
<details>
<summary>186. Як можна потрапити в onStart(), але не потрапити в onResume()?</summary>

#### Kotlin

`onStart()` може викликатися без наступного `onResume()`, якщо Activity стала видимою, але не отримала foreground focus або її lifecycle перервався між станами `STARTED` і `RESUMED`.

1. **Різниця між STARTED і RESUMED**

```text
onStart()  -> Activity видима
onResume() -> Activity у foreground і готова до interaction
```

`STARTED` означає visibility, але не гарантує можливість взаємодії.

2. **Інша Activity або overlay**

Сценарій:

```text
MainActivity.onStart()
TransparentActivity відкривається поверх
MainActivity не переходить у onResume()
```

Якщо інша Activity забирає focus, нижня може залишитися visible у стані `STARTED`.

```xml
<activity
    android:name=".OverlayActivity"
    android:theme="@style/Theme.App.Transparent" />
```

Transparent або dialog-themed Activity може залишити попередній екран видимим, але забрати interaction focus.

3. **Redirect у onStart**

Якщо в `onStart()` одразу запустити іншу Activity:

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

Focus одразу переходить до `LoginActivity`, тому поточна Activity може не resume-итися.

4. **finish до onResume**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

5. **System interruption**

Lifecycle може бути перерваний між `onStart()` і `onResume()`:

```text
onCreate()
onStart()
user/system opens another app
onStop()
```

Причиною може бути system UI, incoming call, multi-window/PiP або швидкий перехід в інший app.

6. **Розміщення lifecycle-логіки**

Ресурс, потрібний під час visibility, прив’язують до `onStart()`/`onStop()`:

```kotlin
override fun onStart() {
    super.onStart()
    startVisibleResource()
}

override fun onStop() {
    stopVisibleResource()
    super.onStop()
}
```

Ресурс тільки для активної взаємодії — до `onResume()`/`onPause()`:

```kotlin
override fun onResume() {
    super.onResume()
    startInteractiveResource()
}

override fun onPause() {
    stopInteractiveResource()
    super.onPause()
}
```

7. **Lifecycle-aware collection**

State, потрібний видимому UI:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Events лише для interactive foreground:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.RESUMED) {
        viewModel.activeEvents.collect(::handleEvent)
    }
}
```

Lifecycle-код не повинен припускати, що після кожного `onStart()` обов’язково буде `onResume()`. Setup і cleanup треба робити симетрично для відповідної пари callbacks.

**Коротко:** `onStart()` без `onResume()` можливий, коли Activity стала видимою, але не отримала focus або була перервана. Типові причини: overlay, redirect, `finish()`, system UI чи перехід у background.

</details>
<details>
<summary>187. Що таке міграції в Room?</summary>

#### Kotlin

Міграція Room описує перехід schema між версіями DB зі збереженням даних користувача.

1. **Коли потрібна міграція**

Вона потрібна при зміні schema:

- додали/видалили таблицю або колонку;
- змінили type, constraint чи relation;
- перейменували таблицю/поле;
- додали index або foreign key.

Якщо schema змінилась, треба збільшити `version` у `@Database`.

2. **Версія бази**

Після зміни entity schema підвищують `version` у `@Database`. Room шукає migration path від установленої version до нової; без нього відкриття DB завершиться exception, якщо не налаштований destructive fallback.

3. **Manual migration**

Додавання nullable колонки:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Підключення:

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .addMigrations(MIGRATION_1_2)
    .build()
```

4. **Non-null колонка**

Для існуючих rows потрібне default value:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

5. **Складна міграція**

Якщо SQLite не дозволяє напряму змінити type/constraints, створюють нову таблицю та переносять дані:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

6. **AutoMigration**

Для простих однозначних змін Room може згенерувати migration:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

Для rename/delete потрібен `AutoMigrationSpec`, а для transformations або split/merge tables — manual migration.

7. **fallbackToDestructiveMigration**

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .fallbackToDestructiveMigration()
    .build()
```

Fallback видаляє стару DB разом із даними. Він допустимий для відновлюваного cache або dev-сценарію, але не для user data.

8. **Тестування міграцій**

`MigrationTestHelper` відкриває стару schema і запускає migrations. Тест вставляє representative data, проходить supported paths і перевіряє schema та значення після переходу.

9. **Schema export**

Room schema export потрібен для auto migrations і перевірки історичних schemas:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    exportSchema = true
)
```

10. **Практичні правила**

- піднімати version при зміні schema;
- підтримувати paths від усіх актуальних старих versions;
- не застосовувати destructive fallback до user data;
- тестувати schema і збереження даних.

**Коротко:** Room migration переводить schema на нову version без втрати даних. Прості зміни підтримує `AutoMigration`, складні — manual `Migration`; усі production paths треба тестувати.

</details>
<details>
<summary>188. Що робить анотація Embedded у Room?</summary>

#### Kotlin

`@Embedded` у Room дозволяє вбудувати поля одного класу в таблицю або результат query іншого класу. Room “розгортає” embedded object у набір колонок.

1. **Базовий приклад**

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

Room створить колонки:

```text
id, name, city, street
```

Тобто `Address` не стає окремою таблицею. Його поля зберігаються в тій самій таблиці `users`.

2. **Для чого це потрібно**

`@Embedded` корисний, коли кілька полів логічно утворюють value object:

- address;
- coordinates;
- money amount + currency;
- user profile info;
- metadata;
- reusable group of columns.

Це допомагає не тримати всі поля плоско в одному великому entity-класі.

3. **Prefix**

Якщо в entity є кілька embedded objects з однаковими назвами полів, треба використати `prefix`:

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

Колонки будуть:

```text
start_lat, start_lng, end_lat, end_lng
```

Без prefix Room отримає конфлікт назв колонок.

4. **Embedded у query result**

`@Embedded` часто використовують у DTO для результату `JOIN`:

```kotlin
data class UserWithStats(
    @Embedded val user: UserEntity,
    @Embedded(prefix = "stats_") val stats: UserStatsEntity
)
```

Це зручно для read models, коли результат query складається з кількох частин.

5. **Embedded vs Relation**

```text
@Embedded -> поля обʼєкта лежать у тій самій row/table
@Relation -> звʼязок між різними таблицями
```

Якщо `Address` — просто частина `UserEntity`, можна `@Embedded`. Якщо address має власний lifecycle або багато users можуть посилатись на одну address row — потрібна окрема table + relation.

6. **Nullable embedded object**

Якщо всі колонки embedded object nullable і в результаті всі `null`, Room може створити `null` object для nullable embedded field:

```kotlin
@Embedded val address: Address?
```

Це важливо для LEFT JOIN або optional data.

7. **Обмеження**

- `@Embedded` не створює relation автоматично.
- Не додає foreign keys.
- Не нормалізує дані.
- Може створити багато колонок у таблиці.
- Потрібен `prefix`, якщо назви колонок конфліктують.

8. **Практичне правило**

Використовуй `@Embedded` для value objects, які логічно є частиною entity і не мають окремого lifecycle. Для окремих сутностей і звʼязків використовуй `@Relation`, foreign keys і окремі таблиці.

**Коротко:** `@Embedded` у Room розгортає поля вкладеного класу в колонки тієї самої таблиці або query result. Він підходить для value objects, підтримує `prefix` для уникнення конфліктів назв, але не створює relation чи foreign key.

</details>
<details>
<summary>189. Що таке DatabaseView у Room?</summary>

#### Kotlin

`@DatabaseView` у Room — це спосіб описати SQLite VIEW як Kotlin-клас. View не зберігає дані окремо, а представляє результат SQL-запиту як read-only модель.

1. **Що таке SQL View**

SQL view — це іменований query:

```sql
CREATE VIEW active_users AS
SELECT id, name FROM users WHERE active = 1;
```

У Room це можна описати через `@DatabaseView`.

2. **Приклад DatabaseView**

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

Room створить SQLite view, а клас буде використовуватись як read model.

3. **Підключення до Database**

```kotlin
@Database(
    entities = [UserEntity::class],
    views = [ActiveUserView::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

`views` треба явно додати в `@Database`.

4. **DAO для View**

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM ActiveUserView")
    fun observeActiveUsers(): Flow<List<ActiveUserView>>
}
```

View можна читати як звичайну таблицю, але не можна insert/update/delete напряму в неї.

5. **Для чого використовувати**

`DatabaseView` корисний для:

- складних read queries;
- projection models;
- join результатів;
- агрегованих даних;
- read-only екранів;
- уникнення дублювання одного SQL у кількох DAO методах.

6. **View з JOIN**

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

Це зручно для екранів, де потрібна готова read model.

7. **DatabaseView vs Entity**

```text
Entity       -> реальна таблиця, зберігає дані
DatabaseView -> read-only view, результат SQL query
```

`DatabaseView` не є окремим джерелом даних. Він залежить від таблиць, на яких побудований.

8. **DatabaseView vs @Relation**

`@Relation` будує object graph із кількох запитів, а `DatabaseView` описує SQL projection/view. Якщо потрібна read model із join/aggregation — часто простіше `DatabaseView`.

9. **Обмеження**

- read-only;
- не можна напряму insert/update/delete;
- треба підтримувати SQL при зміні schema;
- складні views можуть бути важкими для performance;
- зміни underlying tables можуть потребувати migration.

10. **Практичне правило**

Використовуй `DatabaseView`, коли один складний read query потрібен як стабільна модель для DAO/UI. Не використовуй його як заміну нормальним таблицям або для write operations.

**Коротко:** `@DatabaseView` у Room описує SQLite VIEW як read-only Kotlin-модель. Він корисний для складних read queries, joins, projections і aggregates. View читається через DAO, але не зберігає дані окремо і не підтримує прямі insert/update/delete.

</details>
<details>
<summary>190. У чому різниця між SQL та NoSQL?</summary>

#### Kotlin

`SQL` і `NoSQL` — це різні підходи до зберігання даних. SQL бази зазвичай реляційні, зі схемою, таблицями і запитами SQL. NoSQL — ширша категорія нереляційних сховищ: document, key-value, wide-column, graph.

1. **SQL**

SQL бази зберігають дані в таблицях:

```sql
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    age INTEGER
);
```

Дані мають schema, relations, constraints і підтримують транзакції.

2. **NoSQL**

NoSQL може зберігати дані як documents:

```json
{
  "id": "1",
  "name": "Alex",
  "settings": {
    "theme": "dark"
  }
}
```

Типові приклади: MongoDB, Redis, Cassandra, DynamoDB, Firebase Firestore.

3. **Schema**

```text
SQL   -> жорсткіша schema, migrations
NoSQL -> гнучкіша schema, але більше відповідальності на app/backend
```

SQL краще контролює структуру даних. NoSQL простіше адаптується до різних форм документів, але може накопичувати неузгоджені дані.

4. **Relations**

SQL добре працює з relations:

```sql
SELECT * FROM orders
JOIN users ON orders.user_id = users.id;
```

NoSQL часто денормалізує дані, щоб читати їх швидше без join-ів.

5. **Transactions**

SQL бази зазвичай мають сильну транзакційну модель ACID. Це важливо для фінансів, orders, inventory, booking.

NoSQL теж може мати транзакції, але гарантії залежать від конкретної бази і конфігурації.

6. **Scaling**

```text
SQL   -> часто vertical scaling + read replicas/sharding
NoSQL -> часто простіше horizontal scaling
```

Це не абсолютне правило, але NoSQL системи часто проєктувались для distributed workloads.

7. **Android приклади**

В Android найчастіший SQL-інструмент — Room поверх SQLite:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

NoSQL-приклад — Firebase Firestore:

```kotlin
firestore.collection("users").document(id).get()
```

8. **Коли обирати SQL**

SQL підходить, коли:

- структура даних чітка;
- є relations;
- потрібні constraints;
- важливі транзакції;
- потрібні складні queries;
- дані мають бути консистентними.

9. **Коли обирати NoSQL**

NoSQL підходить, коли:

- дані природно документні;
- schema часто змінюється;
- треба швидко масштабувати reads/writes;
- потрібен key-value cache;
- relations мінімальні;
- важлива простота зберігання nested structure.

10. **Практичне правило**

Не треба вибирати NoSQL тільки тому, що “гнучко”, або SQL тільки тому, що “класика”. Вибір залежить від access patterns, consistency requirements, relations, scalability і команди.

**Коротко:** SQL — реляційна модель з таблицями, schema, relations, constraints і сильними транзакціями. NoSQL — нереляційні сховища з гнучкішою моделлю даних і часто кращою горизонтальною масштабованістю. SQL краще для структурованих relation-heavy даних, NoSQL — для document/key-value/distributed сценаріїв.

</details>
<details>
<summary>191. Що таке DataStore?</summary>

#### Kotlin

`DataStore` — Jetpack-рішення для асинхронного збереження невеликих persistent settings. Воно працює з Coroutines і `Flow` та є сучасною альтернативою `SharedPreferences`.

1. **Коли використовувати**

Типові дані: theme, language, onboarding state, feature flags і user preferences. Для великих колекцій, зв’язків, queries або offline cache потрібен `Room`.

2. **Preferences DataStore**

Зберігає key-value дані без schema:

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

3. **Proto DataStore**

Використовує protobuf schema і дає type-safe model:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Цей варіант доречний для структурованих settings, де важлива типобезпека.

4. **Відмінність від SharedPreferences**

```text
SharedPreferences -> synchronous API, mutable key-value access
DataStore         -> suspend API, Flow, transactional updates
```

DataStore не надає synchronous API, тому краще інтегрується з coroutine-based architecture.

5. **Читання в UI**

Repository повертає `Flow`, а `ViewModel` за потреби перетворює його на `StateFlow`:

```kotlin
class SettingsViewModel(
    repository: SettingsRepository
) : ViewModel() {
    val darkMode = repository.darkMode
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5_000), false)
}
```

У Compose:

```kotlin
val darkMode by viewModel.darkMode.collectAsStateWithLifecycle()
```

6. **Запис**

Preferences оновлюють через suspend-функцію `edit`:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

`edit` виконує transactional read-modify-write, тому зовнішня синхронізація не потрібна.

7. **Error handling**

Помилки читання обробляють вибірково:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
}
```

Очікуваний `IOException` можна замінити default preferences, але інші exceptions треба передати далі.

8. **Migration з SharedPreferences**

```kotlin
val Context.dataStore by preferencesDataStore(
    name = "settings",
    produceMigrations = { context ->
        listOf(SharedPreferencesMigration(context, "old_settings"))
    }
)
```

Міграція переносить старі values при першому доступі до DataStore.

9. **Вибір storage**

- Simple key-value settings — Preferences DataStore.
- Type-safe structured settings — Proto DataStore.
- Relational data, складні queries або cache — Room.

**Коротко:** DataStore зберігає невеликі settings через Coroutines і `Flow`. Preferences DataStore підходить для key-value, Proto DataStore — для type-safe schema, а складні дані слід зберігати в Room.

</details>
<details>
<summary>192. Що таке Paging 3?</summary>

#### Kotlin

Paging 3 завантажує великі datasets сторінками та керує prefetch, deduplication, refresh/retry, cancellation і UI integration.

### PagingSource

`PagingSource<Key, Value>` описує single source та key pagination:

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

`load()` повертає `Page`/`Error`; cancellation не перехоплюють. Keys беруть із backend metadata, а `getRefreshKey()` визначає позицію нового generation після refresh.

### Pager і ViewModel

```kotlin
fun users(): Flow<PagingData<UserDto>> =
    Pager(
        config = PagingConfig(
            pageSize = 20,
            prefetchDistance = 5
        ),
        pagingSourceFactory = {
            UsersPagingSource(api)
        }
    ).flow
```

```kotlin
val users = repository.users()
    .cachedIn(viewModelScope)
```

`cachedIn(viewModelScope)` кешує paging stream у scope ViewModel, щоб новий UI collector не запускав pipeline з нуля.

### UI і LoadState

Compose:

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

`loadState.refresh`, `append` і `prepend` окремо представляють initial/refresh та завантаження в обидва боки. UI показує full-screen state для refresh і footer/header для append/prepend. `retry()` повторює failed load, `refresh()` створює нове generation. Для RecyclerView є `PagingDataAdapter`.

### RemoteMediator

Для offline-first:

```text
UI <- PagingSource from Room
          ^
RemoteMediator: API -> Room + remote keys
```

UI читає Room, а `RemoteMediator` завантажує API pages і транзакційно записує data з remote keys. DB залишається source of truth.

### Коли використовувати

Paging доречний для великих remote/DB datasets, infinite scroll та offline cache. Для малого bounded list простіший один request.

Типові помилки: неправильні keys, відсутній `cachedIn`, ignored append error, unstable item ID та паралельна manual pagination.

**Коротко:** pipeline — `PagingSource -> Pager -> PagingData -> UI`; `cachedIn` прив’язує stream до ViewModel. Для API + Room `RemoteMediator` оновлює DB, яка лишається source of truth.

</details>
<details>
<summary>193. Що таке RecyclerView.Adapter?</summary>

#### Kotlin

`RecyclerView.Adapter` — міст між даними та `RecyclerView`: створює `ViewHolder`, прив’язує дані до item UI і повертає кількість елементів. Це тонкий UI-шар без business logic і navigation.

1. **Основні методи**

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

2. **Adapter і ViewHolder**

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
        binding.root.setOnClickListener { onUserClick(user) }
    }
}
```

`ViewHolder` зберігає references на views, а click event передає назовні callback-ом.

3. **Recycling і повний bind**

`RecyclerView` перевикористовує views під час scroll, тому `bind()` має встановлювати весь state:

```kotlin
binding.badge.isVisible = user.isAdmin
```

Умова лише для `true` залишила б у recycled view state попереднього item.

4. **ListAdapter і DiffUtil**

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

`ListAdapter` обчислює diff і оновлює лише змінені items. `areItemsTheSame()` порівнює identity, `areContentsTheSame()` — content. Це краще за mutable list і `notifyDataSetChanged()`.

5. **Кілька типів item**

Для різних layout-ів повертають `viewType`:

```kotlin
override fun getItemViewType(position: Int): Int = when (items[position]) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Після цього `onCreateViewHolder()` створює відповідний holder. Для складних списків доречні delegates або `ConcatAdapter`.

6. **Типові помилки**

- business logic або navigation в adapter;
- `notifyDataSetChanged()` замість diff;
- неповний UI state у `bind()`;
- reference на `Activity` або `Fragment`;
- збережена під час bind position замість актуальної `bindingAdapterPosition` або bound item.

**Коротко:** `RecyclerView.Adapter` перетворює дані на recycled item views. У production важливі повний `bind()`, `ListAdapter` із `DiffUtil` та відсутність business logic усередині adapter.

</details>
<details>
<summary>194. Які основні методи має RecyclerView.Adapter?</summary>

#### Kotlin

Три основні abstract methods створюють holder, bind-ять item та повідомляють розмір dataset:

```kotlin
override fun onCreateViewHolder(
    parent: ViewGroup,
    viewType: Int
): UserViewHolder {
    val binding = ItemUserBinding.inflate(
        LayoutInflater.from(parent.context),
        parent,
        false
    )
    return UserViewHolder(binding)
}

override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}

override fun getItemCount(): Int = items.size
```

`onCreateViewHolder()` викликається, коли немає reusable holder. `onBindViewHolder()` — при показі/оновленні item. Через recycling повний bind встановлює весь View state, включно з `false`, `null` і empty values.

### Різні типи item

`getItemViewType()` визначає, який holder створити:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

### Часткове оновлення

Payload дає виконати partial bind:

```kotlin
override fun onBindViewHolder(
    holder: UserViewHolder,
    position: Int,
    payloads: MutableList<Any>
) {
    if (payloads.isEmpty()) {
        holder.bind(items[position])
    } else {
        holder.bindPayload(items[position], payloads)
    }
}
```

Порожній `payloads` завжди означає full bind; delivery payload не гарантується для detached item.

### Recycling lifecycle

- `onViewRecycled()` — cleanup перед повторним використанням holder-а;
- `onViewAttachedToWindow()` — item приєднаний до window;
- `onViewDetachedFromWindow()` — item більше не attached.

У `onViewRecycled()` скасовують item-specific animations/jobs і callbacks. Attach/detach не слід плутати зі створенням/знищенням holder-а.

### Оновлення даних

Manual notifications мають точно відповідати mutation dataset:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

`notifyDataSetChanged()` не описує конкретні changes. Для snapshot lists зазвичай використовують `ListAdapter`/`AsyncListDiffer` із `DiffUtil.ItemCallback`.

Stable IDs мають бути унікальними та представляти identity, не position.

### Типові помилки

- неповний state у bind;
- важка робота в `onBindViewHolder()`;
- captured `position` замість `bindingAdapterPosition` або bound item;
- mutation без відповідного notify/diff;
- mutable list після `submitList()`;
- неунікальні stable IDs.

**Коротко:** основні methods — `onCreateViewHolder()`, `onBindViewHolder()` і `getItemCount()`. Додатково важливі `getItemViewType()`, payload bind, recycling callbacks і точна синхронізація dataset updates.

</details>
<details>
<summary>195. У чому різниця між RecyclerView.Adapter та ListAdapter?</summary>

#### Kotlin

`ListAdapter` — subclass `RecyclerView.Adapter` із `AsyncListDiffer` і `DiffUtil`. У звичайному Adapter storage та notifications контролює розробник.

```text
Adapter     -> manual list + notify methods
ListAdapter -> submitList + async DiffUtil
```

### RecyclerView.Adapter

```kotlin
class UserAdapter : RecyclerView.Adapter<UserViewHolder>() {
    private val items = mutableListOf<UserUi>()

    override fun getItemCount() = items.size

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(items[position])
    }

    fun replace(items: List<UserUi>) {
        this.items.clear()
        this.items.addAll(items)
        notifyDataSetChanged()
    }
}
```

Adapter дає повний контроль, але mutation і `notify...` мають збігатися. Помилковий index або notification спричинить некоректний UI чи exception.

`notifyDataSetChanged()` не описує конкретні changes, тому погіршує animations та reuse.

### ListAdapter

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Оновлення:

```kotlin
adapter.submitList(users)
```

Diff обчислюється поза main thread, а точні insert/remove/move/change updates застосовуються на Main. Differ також відкидає результат застарілої generation.

### DiffUtil.ItemCallback

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(old: UserUi, new: UserUi) =
        old.id == new.id

    override fun areContentsTheSame(old: UserUi, new: UserUi) =
        old == new
}
```

`areItemsTheSame` порівнює identity, `areContentsTheSame` — rendered content.

Помилкова identity ламає moves/animations, а неправильне content comparison пропускає rebind.

Для partial bind перевизначають `getChangePayload()`.

### Immutable snapshots

```kotlin
adapter.submitList(oldList + newUser)
```

List та items після `submitList()` не мутують: diff порівнює snapshots, а той самий mutable reference може приховати change.

`currentList` — read-only snapshot differ-а, не mutable storage.

### Коли що вибрати

`ListAdapter` — default для більшості dynamic snapshot lists.

Звичайний Adapter доречний, коли:

- storage зовнішній;
- updates уже приходять точними operations;
- потрібна custom differ architecture;
- adapter не представляє простий snapshot list.

Для paging використовують `PagingDataAdapter`, який має подібну DiffUtil model.

Stable IDs вмикають лише коли вони справді унікальні, стабільні та узгоджені з DiffUtil identity.

**Коротко:** Adapter вимагає ручних storage і notifications. ListAdapter приймає immutable snapshots через `submitList()` та застосовує розрахований diff; для звичайного dynamic list це кращий default.

</details>
<details>
<summary>196. Як працює RecyclerView під капотом?</summary>

#### Kotlin

`RecyclerView` показує великі списки без створення View для кожного елемента. Він тримає лише видимі item views, невеликий cache і pool перевикористовуваних `ViewHolder`-ів.

```text
10 000 data items
~10-20 visible views
cache + recycled pool
```

### Основні компоненти

- `Adapter` — створює holder-и та bind-ить дані;
- `ViewHolder` — зберігає item view і посилання на її елементи;
- `LayoutManager` — вимірює та розміщує items;
- `Recycler` / `RecycledViewPool` — шукає holder для повторного використання;
- `ItemAnimator` — анімує структурні зміни;
- `ItemDecoration` — додає spacing, dividers або custom drawing.

### Layout і scroll

Під час layout `LayoutManager` запитує views у `Recycler`. Якщо відповідного holder-а немає, Adapter викликає `onCreateViewHolder()`; перед показом дані встановлюються через `onBindViewHolder()`.

```text
LayoutManager requests view
 -> cache/pool lookup
 -> create holder if absent
 -> bind current item
 -> measure and position view
```

Під час scroll View, що залишила екран, від'єднується та може потрапити в cache або pool. Для нового item використовується сумісний holder того самого `viewType`, після чого виконується новий bind.

Через recycling `bind()` має встановлювати весь стан:

```kotlin
fun bind(item: Item) {
    binding.title.text = item.title
    binding.badge.isVisible = item.isImportant
    binding.checkbox.isChecked = item.isSelected
}
```

Інакше item може успадкувати visibility, checked state або listener від попередніх даних.

### LayoutManager і view types

RecyclerView делегує розміщення елементів:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
// GridLayoutManager(context, 2)
// StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

`getItemViewType()` розділяє різні layouts. Pool групує holder-и за `viewType`, тому header не буде використаний як post.

### Cache і pool

Спрощені рівні reuse:

```text
attached scrap -> views поточного layout pass
cached views    -> нещодавно від'єднані holder-и
recycled pool   -> очищені holder-и за viewType
```

Cache може зберегти holder із прив'язкою до позиції, тоді як holder із pool зазвичай потребує повторного bind. Спільний `RecycledViewPool` можна використовувати для вкладених RecyclerView з однаковими типами items.

### Оновлення списку

Точні adapter notifications дозволяють RecyclerView зберегти identity, запустити animations і перебіндити лише потрібні елементи:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
```

Зазвичай використовують `ListAdapter`/`AsyncListDiffer` з `DiffUtil`. `notifyDataSetChanged()` повідомляє лише про загальну зміну й позбавляє RecyclerView точного diff.

`onBindViewHolder()` має бути дешевим: без file/network I/O та bitmap decoding. Зображення завантажує image loader із cache і cancellation.

Position може змінитися після insert/remove. У callback треба передавати поточний item або перевіряти `bindingAdapterPosition != NO_POSITION`, а не зберігати старе значення `position`.

**Коротко:** RecyclerView ефективний завдяки повторному використанню `ViewHolder`-ів, делегуванню layout у `LayoutManager`, cache/pool механізмам і точковим updates через `DiffUtil`. Коректний повний bind критично важливий через recycling.

</details>
<details>
<summary>197. Що таке DiffUtil?</summary>

#### Kotlin

`DiffUtil` — це utility з RecyclerView, яка порівнює старий і новий список та визначає мінімальний набір змін: insert, remove, move, change. Це дозволяє оновлювати список ефективніше, ніж через `notifyDataSetChanged()`.

1. **Навіщо потрібен DiffUtil**

Погано:

```kotlin
adapter.notifyDataSetChanged()
```

Це каже RecyclerView перемалювати весь список. Немає точних animations, втрачається частина оптимізацій.

Краще — порахувати diff і оновити тільки змінені items.

2. **DiffUtil.ItemCallback**

Для `ListAdapter` зазвичай пишуть `ItemCallback`:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

3. **areItemsTheSame**

Цей метод перевіряє identity:

```kotlin
oldItem.id == newItem.id
```

Тобто це той самий item чи ні. Зазвичай порівнюють stable id.

4. **areContentsTheSame**

Цей метод перевіряє, чи змінився content:

```kotlin
oldItem == newItem
```

Якщо item той самий, але content змінився, RecyclerView оновить саме цей item.

5. **ListAdapter**

`ListAdapter` уже використовує DiffUtil всередині:

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Оновлення:

```kotlin
adapter.submitList(users)
```

Diff рахується асинхронно через `AsyncListDiffer`.

6. **Payloads**

Якщо треба оновити тільки частину item-а:

```kotlin
override fun getChangePayload(oldItem: UserUi, newItem: UserUi): Any? {
    return if (oldItem.isOnline != newItem.isOnline) OnlineStatusChanged
    else null
}
```

Payload дозволяє не rebinding-ити весь row.

7. **Immutability**

Для коректної роботи DiffUtil важливо передавати новий immutable list:

```kotlin
adapter.submitList(oldList + newUser)
```

Погано мутувати старий список і передавати той самий reference.

8. **Типові помилки**

- `areItemsTheSame` порівнює весь object замість id;
- `areContentsTheSame` завжди повертає `true`;
- використання нестабільного id;
- мутація list після `submitList`;
- `notifyDataSetChanged()` замість diff;
- важкий `equals()` для великих nested models.

9. **Коли DiffUtil не потрібен**

Якщо список маленький, статичний і майже не оновлюється, складна diff-логіка може бути зайвою. Але для більшості production списків `ListAdapter` + `DiffUtil.ItemCallback` — хороший default.

10. **Практичне правило**

`areItemsTheSame` відповідає на питання “це той самий item?”, а `areContentsTheSame` — “чи змінились його дані?”. Якщо це розуміти правильно, більшість багів з DiffUtil зникає.

**Коротко:** DiffUtil порівнює старий і новий список та оновлює тільки змінені items. У production зазвичай використовують `ListAdapter` з `DiffUtil.ItemCallback`: `areItemsTheSame` для identity, `areContentsTheSame` для content, а `submitList()` для передачі нового immutable списку.

</details>
<details>
<summary>198. Як відобразити списки в Android?</summary>

#### Kotlin

У View/XML списки відображають через `RecyclerView`, у Jetpack Compose — через `LazyColumn`, `LazyRow` або lazy grids. Для великих remote-списків використовують Paging 3.

### RecyclerView

```kotlin
val adapter = UserAdapter(onClick = viewModel::onUserClicked)

binding.recyclerView.apply {
    layoutManager = LinearLayoutManager(requireContext())
    this.adapter = adapter
}

adapter.submitList(users)
```

Для production-коду зазвичай використовують `ListAdapter` з `DiffUtil.ItemCallback`: він обчислює точкові зміни й не перемальовує весь список через `notifyDataSetChanged()`.

Layout визначає `LayoutManager`:

- `LinearLayoutManager` — вертикальний або горизонтальний список;
- `GridLayoutManager` — сітка;
- `StaggeredGridLayoutManager` — сітка з елементами різної висоти.

### Paging 3

Для даних, що завантажуються сторінками, Adapter наслідує `PagingDataAdapter`:

```kotlin
class UserPagingAdapter :
    PagingDataAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        getItem(position)?.let(holder::bind)
    }
}
```

ViewModel кешує paging stream у своєму scope:

```kotlin
val users: Flow<PagingData<User>> =
    repository.getUsers().cachedIn(viewModelScope)
```

UI збирає його lifecycle-aware:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.users.collectLatest(adapter::submitData)
    }
}
```

`loadState` використовується для loading, error, empty та retry UI. Треба окремо враховувати initial refresh і append/prepend, щоб footer loading не замінював увесь екран.

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

Стабільний `key` зберігає identity item-а при insert, remove, reorder і допомагає Compose коректно утримувати item state. Ключ має бути унікальним і стабільним, а не позицією.

Інші контейнери:

```kotlin
LazyRow {
    items(categories, key = Category::id) { CategoryChip(it) }
}

LazyVerticalGrid(columns = GridCells.Fixed(2)) {
    items(products, key = Product::id) { ProductCard(it) }
}
```

Paging у Compose підключають через `collectAsLazyPagingItems()`, рендерять за `itemCount` і обробляють `loadState` та `retry()`.

### Практичні правила

- не додавати тисячі View вручну в `LinearLayout`;
- не використовувати звичайний `Column` для великого dynamic list;
- передавати immutable snapshots у Adapter/Compose;
- виносити стан списку у ViewModel;
- показувати loading, error, empty та retry states;
- використовувати stable IDs/keys і не виконувати важку роботу під час bind/composition.

**Коротко:** для View UI використовують `RecyclerView` з `ListAdapter`, для Compose — lazy-контейнери. Paging 3 додають для великих або посторінкових джерел; stable identity та коректні load states є обов'язковими для production-списку.

</details>
<details>
<summary>199. Що таке lazy-контейнери в Compose?</summary>

#### Kotlin

`Lazy`-контейнери — компоненти Compose для списків і сіток. Вони compose-ять елементи за потреби для viewport, тому підходять для великих або динамічних колекцій.

1. **Основні lazy-контейнери**

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

`LazyColumn` і `LazyRow` створюють списки, `LazyVerticalGrid` та інші grid-компоненти — сітки.

2. **LazyColumn**

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

3. **Keys**

Для елементів зі стабільною identity передають `key`:

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

Це допомагає зберегти item state після insert, delete або reorder.

4. **contentType**

Для різних типів item-ів задають `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

Compose зможе ефективніше перевикористовувати composition сумісних елементів.

5. **Scroll state**

Scroll position і програмний scroll контролює `LazyListState`:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

Також доступні `firstVisibleItemIndex`, `scrollToItem()` та layout info.

6. **Headers і grids**

```kotlin
LazyColumn {
    stickyHeader {
        Text("A")
    }

    items(users) { user ->
        UserRow(user)
    }
}
```

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

`GridCells.Adaptive` підбирає кількість колонок за доступною шириною.

7. **Performance правила**

- використовувати stable `key` і коректний `contentType`;
- не виконувати I/O, bitmap decoding або важкі обчислення в item;
- не створювати однакові обʼєкти при кожній recomposition;
- уникати зайвих вкладених scroll-контейнерів;
- передавати immutable/stable UI models.

8. **Paging**

Paging інтегрується через `collectAsLazyPagingItems()`:

```kotlin
val items = pager.collectAsLazyPagingItems()

LazyColumn {
    items(items.itemCount) { index ->
        items[index]?.let { item ->
            UserRow(item)
        }
    }
}
```

Окремо обробляють `loadState`: initial loading, append loading, error і retry.

9. **Практичне правило**

Lazy-контейнер потрібен для великої або динамічної колекції. Для кількох статичних елементів звичайний `Column` або `Row` простіший.

**Коротко:** `LazyColumn`, `LazyRow` і grids compose-ять items за потреби. Для production важливі stable keys, `contentType`, легкий item UI, scroll state і обробка Paging load states.

</details>
<details>
<summary>200. Як реалізувати циклічний список у RecyclerView?</summary>

#### Kotlin

Циклічний `RecyclerView` імітує нескінченний carousel: fake adapter position мапиться на real item через modulo.

1. **Базова ідея**

Adapter повертає великий range:

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

2. **Старт із середини**

Початкову position вирівнюють у середині range, щоб скролити в обидва боки:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

3. **Обробка пустого списку**

Перевірка `items.isEmpty()` в `itemCount` обов’язкова, інакше modulo спричинить ділення на нуль. Initial scroll також виконують лише для non-empty list.

4. **Stable IDs**

Не можна повертати той самий real ID для кожного повтору: stable ID має бути унікальним для adapter item. Якщо IDs справді потрібні, fake position включають в ID:

```kotlin
override fun getItemId(position: Int): Long {
    return position.toLong()
}
```

Проте при зміні dataset такі IDs не описують стабільну logical identity, тому часто безпечніше не вмикати stable IDs для fake-infinite adapter.

5. **SnapHelper для carousel**

Для поведінки «один item за раз» додають snapping:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

6. **Auto-scroll**

Auto-scroll запускають lifecycle-aware:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.RESUMED) {
        while (isActive) {
            delay(3_000)
            val manager = recyclerView.layoutManager as LinearLayoutManager
            recyclerView.smoothScrollToPosition(
                manager.findFirstVisibleItemPosition() + 1
            )
        }
    }
}
```

У Fragment coroutine має належати `viewLifecycleOwner`.

7. **DiffUtil і оновлення даних**

Adapter зберігає окремий immutable real list. Після зміни size треба вирівняти current fake position відносно нового modulo.

8. **Ризики**

- modulo для empty list;
- auto-scroll після `onDestroyView()`;
- неунікальні stable IDs;
- складні accessibility semantics;
- важкі items і нескінченна UX-поведінка.

9. **Альтернативи**

Часто простіше використати:

- `ViewPager2` для pager/banner;
- Compose `HorizontalPager`;
- звичайний finite list без fake infinity;
- duplicated edges, якщо треба тільки seamless transition.

**Коротко:** fake-infinite RecyclerView використовує великий `itemCount`, modulo та старт із середини. Треба обробити empty list, lifecycle auto-scroll і unique IDs; для pager часто простіший `ViewPager2`.

</details>
<details>
<summary>201. Що таке анімації в Android?</summary>

#### Kotlin

Анімація змінює UI-властивість у часі: position, alpha, scale, rotation, color або size. Вона має пояснювати зміну стану, а не лише додавати декор.

### View API

Для простих transforms:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

`ViewPropertyAnimator` зручний для `alpha`, `translation`, `scale`, `rotation`.

`ObjectAnimator` анімує property:

```kotlin
ObjectAnimator
    .ofFloat(view, View.ALPHA, 1f, 0f)
    .apply {
        duration = 300
        start()
    }
```

`ValueAnimator` генерує values для custom logic:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    addUpdateListener { animation ->
        progressView.progress =
            animation.animatedValue as Int
    }
    start()
}
```

`AnimatorSet` комбінує animator-и разом або послідовно. `MotionLayout` підходить для складних переходів між layout states.

### Compose

Compose-анімації мають бути state-driven:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "contentAlpha"
)

Box(Modifier.alpha(alpha))
```

Основні API:

- `animate*AsState` — одна property;
- `AnimatedVisibility` — enter/exit;
- `AnimatedContent` — зміна content;
- `updateTransition` — кілька properties одного state;
- `Animatable` — imperative suspend control;
- `rememberInfiniteTransition` — нескінченний decorative loop.

### Timing і performance

Duration задає час, easing/interpolator — характер руху. Spring підходить для фізичної поведінки, linear — для рівномірного progress.

Зазвичай дешевше анімувати:

```text
alpha, translation, scale, rotation
```

Width, height, margin та layout params можуть запускати measure/layout кожен frame. Вони допустимі, але потребують profiling на реальному hierarchy.

Animation callbacks не повинні виконувати I/O, bitmap decoding, складні calculations або створювати багато objects.

У Compose треба уникати зайвих recompositions; коли можливо, state читають на placement/draw phase. Performance перевіряють через System Trace/Perfetto та frame timeline.

### Lifecycle і recycling

View animation треба скасувати й reset-нути, коли View recycled або знищена:

```kotlin
override fun onViewRecycled(holder: ItemHolder) {
    holder.itemView.animate().cancel()
    holder.reset()
}
```

Інакше RecyclerView holder може показати alpha/translation попереднього item-а. Listeners також не повинні утримувати Fragment/View після `onDestroyView()`.

Compose effect/coroutine скасовується при виході з відповідного composition scope, але custom external resources усе одно потребують cleanup.

### Практичний вибір

- проста View transform — `view.animate()`;
- property/value control — `ObjectAnimator/ValueAnimator`;
- coordinated transition — `AnimatorSet/MotionLayout`;
- Compose — state-driven animation API;
- layout animation — лише після оцінки вартості;
- RecyclerView/lifecycle UI — cancel і reset state;
- враховувати системне зменшення/вимкнення анімацій.

**Коротко:** Android має View animators, MotionLayout і Compose animation APIs. Хороша анімація прив'язана до state, не робить важку роботу кожен frame, коректно скасовується та не залишає recycled View у проміжному стані.

</details>
<details>
<summary>202. У чому різниця між ViewBinding та DataBinding?</summary>

#### Kotlin

`ViewBinding` і `DataBinding` — це Android-механізми для роботи з XML layout без ручного `findViewById`, але вони мають різну ціль. `ViewBinding` дає type-safe доступ до View. `DataBinding` додатково дозволяє звʼязувати дані та expressions прямо в XML.

1. **ViewBinding**

ViewBinding генерує binding-клас для кожного XML layout:

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Він простий, швидкий і не додає логіку в XML.

2. **DataBinding**

DataBinding дозволяє писати binding expressions у XML:

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

У Kotlin:

```kotlin
binding.user = user
binding.executePendingBindings()
```

3. **Головна різниця**

```text
ViewBinding -> type-safe доступ до View
DataBinding -> binding даних/expressions у XML
```

ViewBinding не змінює архітектуру. DataBinding дозволяє частину UI-логіки перенести в XML.

4. **Compile-time safety**

Обидва генерують класи і прибирають ризик неправильного `findViewById`. Але DataBinding має складніший code generation і може давати менш очевидні build/runtime помилки.

5. **Performance і build time**

ViewBinding легший і зазвичай швидший для build. DataBinding важчий, бо парсить expressions, генерує більше коду і має складніший runtime.

6. **Двосторонній binding**

DataBinding підтримує two-way binding:

```xml
<EditText
    android:text="@={viewModel.query}" />
```

Це може бути зручно, але у великих проєктах часто ускладнює debug data flow.

7. **Архітектурний ризик DataBinding**

Погано, коли XML починає містити складну логіку:

```xml
android:visibility="@{user.isAdmin ? View.VISIBLE : View.GONE}"
```

Прості expressions допустимі, але бізнес-логіку краще тримати у ViewModel/mapper.

8. **Коли використовувати ViewBinding**

ViewBinding — хороший default для XML/View System:

- простий доступ до View;
- мінімум магії;
- менший build overhead;
- легше debug;
- не переносить логіку в XML.

9. **Коли використовувати DataBinding**

DataBinding доречний, якщо проєкт уже побудований на binding expressions або потрібна declarative XML-binding модель. Для нового коду частіше обирають ViewBinding або Compose.

10. **Практичне правило**

Для modern Android XML екранів я б за замовчуванням обрав ViewBinding. DataBinding варто брати тільки якщо команда свідомо використовує XML expressions і готова контролювати складність.

**Коротко:** ViewBinding генерує type-safe доступ до View і є простішим default. DataBinding додає binding expressions, variables і two-way binding у XML, але має більший build/runtime overhead і ризик перенести забагато логіки в layout.

</details>
<details>
<summary>203. Що таке Serializable?</summary>

#### Kotlin

`java.io.Serializable` — marker interface Java binary serialization. `ObjectOutputStream` записує object graph у bytes, `ObjectInputStream` відновлює його.

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

Це не `kotlinx.serialization.Serializable`: однойменна annotation генерує serializers для JSON, Proto та інших formats.

### Android Intent і Bundle

```kotlin
val intent = Intent(context, DetailsActivity::class.java)
    .putExtra("user", user)
```

З API 33 доступний typed overload:

```kotlin
val user = intent.getSerializableExtra(
    "user",
    User::class.java
)
```

Для єдиного виклику на різних API levels використовують `IntentCompat.getSerializableExtra()`.

### Serializable vs Parcelable

`Serializable` — JVM runtime mechanism із мінімумом коду, але reflection/metadata overhead і слабкою schema evolution. `Parcelable` — Android format для Bundle/Intent/IPC; `@Parcelize` генерує реалізацію.

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Для navigation argument часто достатньо primitive ID, за яким актуальні дані завантажуються з repository.

### Version compatibility

Java serialization перевіряє compatibility через `serialVersionUID`:

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

Явний UID фіксує version identifier, але зміна fields, hierarchy або types усе одно може порушити semantics чи compatibility.

Для persistence краще Room, DataStore або explicit versioned JSON/Proto schema.

### Ризики в Android

Binder transaction має обмежений розмір. Великий graph створює allocations і ризик `TransactionTooLargeException`.

```kotlin
// bad
intent.putExtra("response", hugeResponse)

// better
intent.putExtra("user_id", user.id)
```

Недовірені Java serialized bytes не можна приймати як network/external format через ризики object deserialization.

Усі reachable fields теж мають бути serializable, інакше виникне `NotSerializableException`; непотрібні поля позначають `transient` на JVM field.

### Коли допустимо

- невеликий internal argument у legacy-коді;
- короткоживучий object під контролем одного app;
- performance і schema evolution не критичні.

Не підходить для великих screen models, domain persistence, network protocol або довготривалого cache.

**Коротко:** Java `Serializable` перетворює object graph у bytes runtime-механізмом JVM. Для Android arguments зазвичай кращі `Parcelable` або ID, для persistence — формат із явною schema.

</details>
<details>
<summary>204. Які є способи серіалізації в Android?</summary>

#### Kotlin

Серіалізація — це перетворення обʼєкта у формат, який можна передати або зберегти: JSON, XML, bytes, `Bundle`, database entity тощо. В Android найчастіше серіалізація потрібна для API, navigation arguments, local storage і IPC.

1. **JSON serialization**

Для network/API найчастіше використовують JSON:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String
)
```

З Kotlin Serialization:

```kotlin
val json = Json.encodeToString(UserDto("1", "Alex"))
val user = Json.decodeFromString<UserDto>(json)
```

Популярні варіанти: Kotlin Serialization, Moshi, Gson.

2. **Kotlin Serialization**

Плюси:

- добре працює з Kotlin;
- підтримує `sealed class`;
- compile-time plugin;
- зручна для Ktor і multiplatform.

```kotlin
@Serializable
sealed interface FeedItemDto {
    @Serializable
    data class Post(val id: String) : FeedItemDto
}
```

3. **Moshi**

Moshi часто використовують з Retrofit:

```kotlin
data class UserDto(
    val id: String,
    val name: String
)
```

Плюси: стабільна Android-практика, codegen, добра інтеграція з Retrofit. Важливо використовувати codegen, а не reflection, якщо потрібна краща performance.

4. **Gson**

Gson старіший і простий, але має мінуси:

- reflection;
- слабша Kotlin nullability підтримка;
- може мовчки створювати некоректні обʼєкти;
- гірше для modern Kotlin моделей.

У legacy Android його ще часто можна зустріти.

5. **Parcelable**

`Parcelable` — Android-спосіб передавати обʼєкти між components:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Використовується для `Intent`, `Bundle`, Navigation args. Для великих обʼєктів краще передавати id, а не весь payload.

6. **Serializable**

Java `Serializable` простий, але повільніший і менш контрольований:

```kotlin
data class User(val id: String) : Serializable
```

В Android зазвичай краще `Parcelable`, особливо для navigation/IPC.

7. **Bundle / primitives**

Для простих аргументів достатньо primitives:

```kotlin
bundleOf("user_id" to userId)
```

Це найкращий варіант для navigation: передати `id`, а дані завантажити з repository/ViewModel.

8. **Room entities**

Room не серіалізує domain model напряму. Зазвичай роблять mapping:

```kotlin
data class UserEntity(
    val id: String,
    val name: String
)

fun UserEntity.toDomain() = User(id, name)
```

Для складних полів можна використовувати `TypeConverter`, але не варто перетворювати Room на JSON-storage без потреби.

9. **Proto / binary formats**

Для structured settings або compact storage можна використовувати protobuf, наприклад у Proto DataStore. Це type-safe, але потребує schema.

10. **Практичне правило**

- API JSON — Kotlin Serialization або Moshi.
- Navigation args — primitives або `Parcelable`.
- Intent/Bundle — `Parcelable`, не великі обʼєкти.
- Local DB — Room entities + mappers.
- Settings — DataStore, за потреби Proto DataStore.
- Legacy/simple cases — Gson або Java Serializable, але обережно.

**Коротко:** в Android серіалізація буває для API, navigation, storage і IPC. Для JSON краще Kotlin Serialization або Moshi, для Android arguments — `Parcelable`, для простих navigation args — primitives/id, для DB — Room entities, для type-safe settings — Proto DataStore.

</details>
<details>
<summary>205. Що робити, якщо поле може бути відсутнім у відповіді API?</summary>

#### Kotlin

DTO має відображати API contract: optional field отримує default, explicit `null` потребує nullable type, а invalid required field має завершити decoding/mapping помилкою.

1. **Nullable поле**

Якщо backend може надіслати `null`:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

Mapper визначає domain fallback:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name ?: "Unknown",
    avatarUrl = avatarUrl
)
```

2. **Default value**

Для missing field задають default:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Default має відповідати contract, а не приховувати backend regression.

3. **Відсутнє поле vs null**

Це різні випадки:

```json
{}
```

і:

```json
{ "name": null }
```

Missing означає «ключа немає», explicit null — «ключ є без value». Їхня semantics може відрізнятися, наприклад у PATCH response.

4. **Kotlin Serialization**

```kotlin
@Serializable
data class ProfileDto(
    val id: String,
    val displayName: String? = null
)
```

Default `kotlinx.serialization` використовує property default для missing key. Nullable property без default лишається required і за відсутності кидає `MissingFieldException`; `explicitNulls = false` змінює цю поведінку.

5. **Moshi/Gson**

Поведінка Moshi/Gson залежить від Kotlin adapter/codegen та configuration. Її треба зафіксувати tests для missing, explicit null, wrong type та unknown enum.

6. **Mapper layer**

DTO не передають напряму в UI:

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

Mapper валідовує transport model і створює domain/UI model зі зрозумілими guarantees.

7. **Required fields**

Критичне поле не маскують fallback-ом:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

Repository може перетворити invalid response на typed error та logging/metric.

8. **Typed error**

Невалідний response можна виразити явно:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

UI не отримує напіввалідний `User`.

9. **Практичне правило**

- missing optional field — default;
- explicit null — nullable type;
- required field — validation/failure;
- DTO → mapper → domain/UI model;
- serializer behavior фіксують contract tests.

**Коротко:** missing field моделюють default value, explicit null — nullable type. Mapper перетворює DTO на валідну domain model, а відсутність required field — на explicit error.

</details>
<details>
<summary>206. Що таке Dependency Injection?</summary>

#### Kotlin

`Dependency Injection` — передача залежностей класу ззовні замість їх створення всередині. Це зменшує coupling, спрощує тестування та керування lifecycle.

1. **Проблема без DI**

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Такий клас жорстко прив’язаний до concrete API:

- залежність складно замінити в тесті;
- creation і business responsibilities змішані;
- lifecycle та configuration контролює сам repository.

2. **Constructor injection**

Найкращий default — constructor injection:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

Клас отримує готовий object і не знає, хто та як його створив.

3. **Interface + implementation**

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

Caller залежить від contract `UserRepository`, а implementation можна замінити.

4. **DI в Android**

В Android DI з’єднує ViewModels, use cases, repositories, API clients, databases і dispatchers.

Приклад з Hilt:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

5. **Binding interface**

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

`@Binds` повідомляє container-у implementation interface.

6. **Providing external dependencies**

Для third-party objects без injectable constructor використовують `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient = OkHttpClient.Builder().build()
}
```

Це типово для Retrofit, Room або OkHttp.

7. **Scopes**

Scope визначає lifetime та reuse instance:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

Scope має відповідати owner-у. Зайвий singleton утримує object довше, ніж потрібно, і може спричинити leak.

8. **Тестування**

У тесті real implementation замінюють fake:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

9. **Manual DI**

DI не вимагає framework. Для малого graph достатньо manual container:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Hilt/Dagger/Koin автоматизують wiring, коли graph і кількість scopes зростають.

10. **Практичне правило**

- constructor injection — default;
- callers залежать від contracts;
- scopes відповідають lifecycle owners;
- framework автоматизує graph, але не замінює архітектуру.

**Коротко:** DI передає залежності ззовні, зменшуючи coupling і спрощуючи testing та lifecycle management. Для малого graph достатньо manual DI, для великого — container на кшталт Hilt.

</details>
<details>
<summary>207. У чому різниця між Dagger/Hilt та Koin?</summary>

#### Kotlin

Dagger/Hilt і Koin — DI-рішення з різною моделлю wiring. Dagger генерує factories/components; Hilt додає Android component hierarchy. Koin використовує container та Kotlin DSL/annotations, а сучасний compiler plugin може перевіряти graph під час compilation.

1. **Dagger**

`Dagger` генерує DI code та валідує graph під час compilation.

Плюси:

- compile-time graph validation;
- direct generated factories без runtime lookup;
- explicit components/scopes.

Мінуси:

- більше annotations/modules;
- складніша component hierarchy;
- іноді важкі compiler diagnostics/build impact.

2. **Hilt**

`Hilt` — Android layer над Dagger з generated components, стандартними scopes та Jetpack integrations.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

Він зменшує manual Dagger setup і задає стандартний Android graph.

3. **Koin**

Koin описує definitions через Kotlin DSL або annotations:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

DSL дає мало ceremony та легко читається. Без compiler validation missing/ambiguous definitions можуть проявитися runtime; compiler plugin Koin 4.2 перевіряє modules, qualifiers і call sites під час compilation.

4. **Graph validation**

```text
Dagger/Hilt         -> generated graph + compile-time validation
Koin classic runtime -> runtime definitions/lookups
Koin compiler plugin -> compile-time validation + runtime container
```

Тому твердження «Koin завжди знаходить помилки лише runtime» вже застаріле; усе залежить від обраного setup.

5. **Performance**

Dagger/Hilt викликає generated factories. Koin resolve-ить definitions через runtime container, тому startup/runtime profile відрізняється. Різницю слід вимірювати на реальному graph, а не вибирати framework лише за microbenchmark.

6. **Scopes**

Hilt має predefined Android scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Koin також підтримує scopes та Android integrations, але ownership graph команда моделює у Koin configuration.

7. **Тестування**

Hilt замінює modules через test components/`@TestInstallIn`, Koin — test modules та definition overrides. В unit tests найпростіше передавати fake напряму через constructor.

8. **Що вибрати**

- Hilt — standard Android component hierarchy та Dagger ecosystem;
- Koin — DSL/annotations, простіше custom wiring і KMP use cases;
- existing Dagger graph зазвичай не варто переписувати без вимірюваної користі;
- оцінюють compile safety setup, build time, startup, scopes і expertise команди.

**Коротко:** Dagger/Hilt генерує DI code та дає стандартний Android graph. Koin використовує runtime container із DSL/annotations, але compiler plugin уже додає compile-time validation. Вибір залежить від architecture, tooling і досвіду команди.

</details>
<details>
<summary>208. Що таке CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` — це механізм Jetpack Compose для передачі значень вниз по composition tree без явного прокидування параметрів через кожен composable. Це scoped context для UI-значень.

1. **Навіщо потрібен**

Без CompositionLocal довелось би передавати однаковий параметр через багато рівнів:

```kotlin
App(theme) {
    Screen(theme) {
        Card(theme) {
            Text(theme)
        }
    }
}
```

З CompositionLocal значення задається один раз для subtree:

```kotlin
CompositionLocalProvider(LocalSpacing provides AppSpacing()) {
    AppContent()
}
```

2. **Оголошення**

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

3. **Використання**

```kotlin
@Composable
fun ProfileCard() {
    val spacing = LocalSpacing.current

    Column(Modifier.padding(spacing.medium)) {
        Text("Profile")
    }
}
```

Composable читає найближче значення `LocalSpacing`, яке було provided вище в tree.

4. **CompositionLocalProvider**

```kotlin
CompositionLocalProvider(
    LocalSpacing provides AppSpacing(medium = 20.dp)
) {
    ProfileScreen()
}
```

Provider діє тільки на дочірній subtree. Зовні значення не змінюється.

5. **compositionLocalOf vs staticCompositionLocalOf**

```text
compositionLocalOf       -> точніше відстежує reads, краще для змінних values
staticCompositionLocalOf -> дешевший read, краще для майже статичних values
```

Для theme/design tokens зазвичай використовують `staticCompositionLocalOf`. Для values, які можуть змінюватись частіше, — `compositionLocalOf`.

6. **Built-in CompositionLocals**

Compose вже має готові locals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`;
- `LocalLayoutDirection`.

Наприклад:

```kotlin
val context = LocalContext.current
```

7. **Коли використовувати**

CompositionLocal доречний для:

- theme colors;
- typography;
- spacing;
- design system tokens;
- locale/configuration;
- UI helpers, які логічно доступні всьому subtree.

8. **Коли не використовувати**

Не варто ховати через CompositionLocal бізнес-залежності:

```kotlin
val repository = LocalUserRepository.current // погана ідея
```

Repositories, use cases і services краще передавати через DI/ViewModel. Інакше CompositionLocal перетворюється на service locator.

9. **Ризики**

- приховані залежності;
- складніше тестування;
- незрозуміле походження value;
- зайві recompositions при частих змінах;
- зловживання замість явних параметрів.

10. **Практичне правило**

Якщо значення є частиною UI context і потрібне багатьом composables — CompositionLocal доречний. Якщо це конкретні дані екрана або бізнес-логіка — краще явні parameters, state hoisting або ViewModel.

**Коротко:** `CompositionLocal` передає scoped UI-значення вниз по Compose tree без parameter drilling. Його добре використовувати для theme, spacing, typography, context-like UI values, але не для прихованого доступу до repositories або бізнес-логіки.

</details>
<details>
<summary>209. Які бувають CompositionLocal?</summary>

#### Kotlin

`CompositionLocal` передає контекстне UI-значення вниз по composition tree без параметра на кожному рівні. Типові приклади: theme, density, localization і design-system tokens.

1. **Навіщо потрібен CompositionLocal**

Замість parameter drilling:

```kotlin
App(theme, user, analytics) {
    Screen(theme, user, analytics)
}
```

значення надають subtree:

```kotlin
CompositionLocalProvider(LocalAnalytics provides analytics) {
    AppContent()
}
```

і читають нижче:

```kotlin
val analytics = LocalAnalytics.current
```

2. **staticCompositionLocalOf**

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

Reads `staticCompositionLocalOf` не відстежуються окремо. При зміні value invalidated весь content provider-а, тому цей варіант підходить для стабільних theme/design-system values.

3. **compositionLocalOf**

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`compositionLocalOf` відстежує reads. Після зміни value Compose invalidates composables, які читали `.current`.

4. **Вибір factory**

```text
staticCompositionLocalOf -> дешевший read, менш точне invalidation
compositionLocalOf       -> точніше invalidation, корисно для змінних значень
```

Отже, static-варіант використовують для рідко змінних values, tracked-варіант — коли важлива точніша recomposition.

5. **CompositionLocalProvider**

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

Вкладений provider може тимчасово перевизначити value для свого subtree.

6. **Приклад design system**

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

Використання: `Modifier.padding(LocalSpacing.current.medium)`.

7. **Built-in CompositionLocals**

Compose надає built-in locals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLayoutDirection`;
- `LocalLifecycleOwner` і `LocalFocusManager`.

Вони представляють UI environment поточного subtree.

8. **Коли не використовувати**

Не слід ховати business dependency:

```kotlin
val repository = LocalRepository.current // погано для бізнес-логіки UI
```

Repositories/use cases передають через DI, а важливі component inputs — явними параметрами.

9. **Ризики**

- приховані залежності та складніші тести;
- незрозумілий owner value;
- широка invalidation для static local;
- перетворення на service locator.

10. **Практичне правило**

CompositionLocal доречний, коли value логічно належить UI environment усього subtree. Залежність конкретного компонента краще передати параметром.

**Коротко:** `staticCompositionLocalOf` підходить для стабільних UI values і ширше invalidates subtree; `compositionLocalOf` відстежує reads точніше. Обидва не повинні замінювати DI чи явні component inputs.

</details>
<details>
<summary>210. Як працює CompositionLocal під капотом?</summary>

#### Kotlin

`CompositionLocal` працює як контекстне значення в Compose tree. Provider записує значення в composition, а composables нижче читають його через `.current`. Compose використовує ці reads, щоб знати, кого треба recomposition-ити при зміні value.

1. **Базова модель**

```kotlin
val LocalSpacing = staticCompositionLocalOf {
    AppSpacing()
}

CompositionLocalProvider(LocalSpacing provides AppSpacing(16.dp)) {
    Content()
}
```

Усередині `Content()` і нижче:

```kotlin
val spacing = LocalSpacing.current
```

Compose шукає найближче значення `LocalSpacing` у composition hierarchy.

2. **Provider scope**

`CompositionLocalProvider` задає value тільки для свого subtree:

```text
Root
 ├─ LocalSpacing = 8dp
 │   └─ ScreenA reads 8dp
 └─ LocalSpacing = 16dp
     └─ ScreenB reads 16dp
```

Різні частини UI можуть мати різні значення одного CompositionLocal.

3. **current read**

Коли composable читає:

```kotlin
val colors = LocalAppColors.current
```

Compose фіксує dependency між цим composable і конкретним CompositionLocal value. Якщо value зміниться, відповідний subtree або readers можуть бути invalidated.

4. **compositionLocalOf**

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`compositionLocalOf` відстежує reads точніше. Якщо значення змінюється, Compose може recomposition-ити тільки ті місця, які читали `.current`.

5. **staticCompositionLocalOf**

```kotlin
val LocalTypography = staticCompositionLocalOf { DefaultTypography }
```

`staticCompositionLocalOf` не відстежує individual reads так точно. При зміні provider value Compose invalidates ширшу область provider. Це дешевше для read і добре для значень, які майже не змінюються: theme, typography, spacing.

6. **Різниця коротко**

```text
compositionLocalOf       -> точніше invalidation, краще для змінних values
staticCompositionLocalOf -> дешевший read, краще для статичних/theme values
```

7. **Nested providers**

Внутрішній provider перекриває зовнішній:

```kotlin
CompositionLocalProvider(LocalSpacing provides smallSpacing) {
    Header()

    CompositionLocalProvider(LocalSpacing provides largeSpacing) {
        Content()
    }
}
```

`Header` отримає `smallSpacing`, `Content` — `largeSpacing`.

8. **Default value**

CompositionLocal має default factory:

```kotlin
val LocalAnalytics = staticCompositionLocalOf<Analytics> {
    error("Analytics not provided")
}
```

Якщо value не provided, буде використаний default. Для обовʼязкових залежностей часто краще кидати `error`, щоб швидко знайти проблему.

9. **Performance нюанс**

Якщо CompositionLocal часто змінюється і читається у великому subtree, можна отримати зайві recompositions. Тому не варто класти туди high-frequency state типу scroll position або text input value.

10. **Практичне правило**

CompositionLocal — це не глобальна змінна, а scoped value в composition. Його треба використовувати для контекстних UI-значень, а не для прихованого доступу до repositories/use cases.

**Коротко:** під капотом `CompositionLocal` зберігає scoped value у composition tree. `CompositionLocalProvider` задає значення для subtree, `.current` читає найближче value, а Compose використовує ці reads для invalidation. `compositionLocalOf` точніше відстежує зміни, `staticCompositionLocalOf` дешевший для майже статичних values.

</details>
<details>
<summary>211. Що таке side effects у Compose?</summary>

#### Kotlin

Side effect — дія поза чистим описом UI: coroutine, listener, navigation, snackbar, analytics або синхронізація із зовнішнім object. Composable body може виконуватися багато разів, тому effect не запускають напряму:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // repeated on recomposition
    Text(userId)
}
```

### LaunchedEffect

Запускає coroutine, прив'язану до composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

При зміні key попередня coroutine скасовується й запускається нова; при виході з composition вона скасовується. Keys мають містити dependencies effect-а. `LaunchedEffect(Unit)` виконується один раз для поточного входу в composition, а не один раз за життя process.

### rememberCoroutineScope

Потрібен для coroutine, що стартує з callback:

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

Scope скасовується при виході з composition. `LaunchedEffect` запускається декларативно через keys, `rememberCoroutineScope` — імперативно після події.

### DisposableEffect

Керує register/unregister lifecycle:

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

При зміні key виконується cleanup і створюється новий effect. Кожна реєстрація listener-а повинна мати симетричний `onDispose`.

### SideEffect

```kotlin
SideEffect {
    externalController.enabled = isEnabled
}
```

Виконується після кожної успішної composition для публікації Compose state у non-Compose object. Не підходить для suspend або важкої роботи.

### Допоміжні API

`rememberUpdatedState` передає актуальну lambda/value у long-running effect без його restart:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

`produceState` адаптує suspend/callback source у Compose `State`. Для Flow у UI зазвичай використовують `collectAsStateWithLifecycle()`.

`snapshotFlow` перетворює reads Compose state у Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect(analytics::trackScroll)
}
```

`derivedStateOf` не є side effect. Це derived state для випадку, коли input змінюється частіше, ніж потрібен новий UI result.

### Правила

- не виконувати effects у composable body;
- правильно задавати keys;
- очищати listeners у `onDispose`;
- використовувати `rememberUpdatedState` для актуальної callback без restart;
- не переносити UI effect у довгоживучий application scope;
- зберігати durable data як state, а не як одноразовий effect.

**Коротко:** `LaunchedEffect` запускає coroutine за keys, `rememberCoroutineScope` — із callback, `DisposableEffect` керує listener lifecycle, `SideEffect` синхронізує state після composition. Правильні keys і cleanup запобігають повторним діям та leaks.

</details>
<details>
<summary>212. Як оптимізувати recomposition?</summary>

#### Kotlin

Recomposition не треба «вимикати» — вона має бути локальною та дешевою. Проблеми виникають через широкі state reads, нестабільні параметри або важку роботу в composable body.

1. **Тримати state на правильному рівні**

State піднімають до найнижчого спільного owner-а, якому він потрібен.

Якщо search query більше нікому не потрібен, його читають у локальній секції:

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

2. **Звужувати state reads**

Сам поділ на функції не гарантує оптимізацію. Кожна частина має читати лише потрібний state і отримувати стабільні параметри — тоді Compose може skip-нути незмінені groups.

3. **Використовувати immutable/stable моделі**

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

`@Immutable` ставлять лише коли контракт справді виконується. Mutable properties роблять зміни непередбачуваними для Compose.

4. **Не мутувати state in-place**

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

Треба створити новий observable state value; проста мутація звичайного списку може не запустити recomposition.

5. **Ставити key у LazyColumn**

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

`key` прив’язує remembered state до item identity під час insert, delete або reorder.

Для mixed lists також задають `contentType`, щоб перевикористовувати сумісні item compositions.

6. **Прибирати важку роботу з composition**

Transformation виносять у state holder або кешують за input:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

7. **Кешування і derived state**

`derivedStateOf` доречний, коли source змінюється часто, а UI реагує лише на похідну умову:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

8. **Профілювати, а не гадати**

Оптимізацію підтверджують інструментами:

- recomposition counters і Layout Inspector;
- Compose compiler reports;
- System Trace та Android Studio Profiler;
- Macrobenchmark у release-подібній збірці.

Високий recomposition count не завжди проблема, якщо composable дешевий.

**Коротко:** recomposition оптимізують через вузькі state reads, стабільні моделі, immutable updates, lazy-list keys і винесення важкої роботи. Високий count не проблема, якщо робота дешева.

</details>
<details>
<summary>213. Як написати асинхронний тест без використання корутин?</summary>

#### Kotlin

Асинхронний тест без корутин синхронізують із конкретною подією: callback, result, state update або idle state. Очікування завжди має timeout; `Thread.sleep()` для цього не підходить.

1. **CountDownLatch**

Для callback-based API підійде `CountDownLatch`:

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

2. **CompletableFuture**

Для одного result/error callback можна адаптувати до `CompletableFuture`:

```kotlin
val future = CompletableFuture<User>()
repository.loadUser("42", object : UserCallback {
    override fun onSuccess(user: User) = future.complete(user)
    override fun onError(error: Throwable) = future.completeExceptionally(error)
})
assertEquals("42", future.get(2, TimeUnit.SECONDS).id)
```

3. **Очікування state**

Awaitility або власний polling helper може чекати, доки assertion стане істинним:

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

Тест завершується одразу після виконання умови, а не чекає фіксовану затримку.

4. **LiveData**

Для `LiveData` використовують `InstantTaskExecutorRule` і helper на кшталт `getOrAwaitValue()`:

```kotlin
@get:Rule
val instantTaskExecutorRule = InstantTaskExecutorRule()

@Test
fun `emits user`() {
    val user = viewModel.user.getOrAwaitValue()
    assertEquals("42", user.id)
}
```

Helper має підписатися, дочекатися значення з timeout і видалити observer у `finally`.

5. **UI-тести**

Espresso синхронізують через `IdlingResource`:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

6. **Fake dependency**

Найстабільніший unit test використовує fake, завершення якого контролює сам тест:

```kotlin
class FakeUserApi : UserApi {
    var callback: UserCallback? = null

    override fun loadUser(id: String, callback: UserCallback) {
        this.callback = callback
    }

    fun complete(user: User) = callback?.onSuccess(user)
}
```

Тест сам викликає `complete()`, тому не залежить від network timing.

7. **Антипатерн**

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

Коротка затримка дає випадковий failure, довга — марнує час.

**Коротко:** тест має чекати конкретну подію з timeout. Для цього використовують `CountDownLatch`, `CompletableFuture`, Awaitility, LiveData helper, Espresso `IdlingResource` або керований fake — але не `Thread.sleep()`.

</details>
<details>
<summary>214. У чому різниця між inline, noinline та crossinline?</summary>

#### Kotlin

`inline`, `noinline` і `crossinline` — це модифікатори для функцій вищого порядку. Вони впливають на те, як компілятор працює з lambda-параметрами: вбудовує їх у call site, дозволяє передавати як object або забороняє non-local return.

1. **inline**

`inline` просить компілятор вставити тіло функції і lambda в місце виклику:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Це зменшує overhead на створення lambda object і виклик функції, особливо для маленьких higher-order functions.

2. **Навіщо inline потрібен**

`inline` корисний для:

- маленьких utility functions;
- DSL;
- functions з lambda-параметрами;
- reified generics;
- performance-sensitive higher-order calls.

Приклад reified:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T =
    fromJson(json, T::class.java)
```

Без `inline` reified type parameter неможливий.

3. **Non-local return**

У inline lambda можна зробити return з зовнішньої функції:

```kotlin
inline fun runBlock(block: () -> Unit) = block()

fun test() {
    runBlock {
        return // вихід з test()
    }
}
```

Це називається non-local return.

4. **noinline**

`noinline` каже: цей lambda-параметр не треба inline-ити:

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

`noinline` потрібен, якщо lambda треба зберегти в змінну, передати далі або використати як object.

5. **crossinline**

`crossinline` забороняє non-local return з lambda:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

Lambda виконується в іншому контексті (`Runnable`), тому `return` із зовнішньої функції був би небезпечним.

6. **Різниця коротко**

```text
inline      -> вбудувати function/lambda в call site
noinline    -> не inline-ити конкретну lambda
crossinline -> inline-ити, але заборонити non-local return
```

7. **Приклад різниці**

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

- `inlined` можна inline-ити повністю;
- `stored` можна зберігати/передавати;
- `safe` можна викликати в іншому lambda/context без non-local return.

8. **Ризики inline**

`inline` не треба ставити всюди. Якщо функція велика або викликається багато разів у коді, bytecode може роздутись. Inline має сенс переважно для маленьких functions із lambdas або reified generics.

9. **Практичне правило**

- Хочеш прибрати lambda overhead або зробити reified — `inline`.
- Треба зберегти lambda або передати далі — `noinline`.
- Lambda виконується в іншому context і не має робити non-local return — `crossinline`.

**Коротко:** `inline` вбудовує функцію і lambda в місце виклику, `noinline` вимикає inline для конкретної lambda, а `crossinline` залишає inline, але забороняє non-local return. Найчастіше це потрібно для DSL, reified generics і оптимізації маленьких higher-order functions.

</details>
<details>
<summary>215. Які переваги Kotlin над Java?</summary>

#### Kotlin

Kotlin має кілька практичних переваг над Java, особливо в Android: null safety, менше boilerplate, coroutines, extension functions, data classes, sealed classes, smart casts і кращу інтеграцію з modern Android APIs.

1. **Null safety**

Kotlin розділяє nullable і non-null типи:

```kotlin
val name: String = "Alex"
val nickname: String? = null
```

Це зменшує кількість `NullPointerException`, бо nullable треба обробляти явно:

```kotlin
val title = user.name ?: "Unknown"
```

2. **Менше boilerplate**

Data class замінює багато Java-коду:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Kotlin автоматично генерує `equals`, `hashCode`, `toString`, `copy`, component functions.

3. **Coroutines**

Kotlin coroutines дають зручний async-код:

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = State.Success(user)
}
```

Це простіше за callbacks і часто читабельніше за складні chains.

4. **Extension functions**

```kotlin
fun View.show() {
    visibility = View.VISIBLE
}
```

Можна додавати utility-поведінку без наслідування і wrapper classes.

5. **Sealed classes/interfaces**

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}
```

Добре підходить для моделювання state, results, effects, navigation events.

6. **Smart casts**

```kotlin
if (state is UiState.Content) {
    Text(state.user.name)
}
```

Після перевірки типу Kotlin автоматично cast-ить значення.

7. **Default і named arguments**

```kotlin
fun createUser(name: String, active: Boolean = true)

createUser(name = "Alex")
```

Код стає читабельнішим і не треба створювати багато overloads.

8. **Interoperability з Java**

Kotlin добре працює з Java-кодом і бібліотеками:

```kotlin
val list = ArrayList<String>()
```

Це дозволяє мігрувати поступово, а не переписувати весь проєкт одразу.

9. **Android-first tooling**

Багато сучасних Android API орієнтовані на Kotlin:

- Jetpack Compose;
- Coroutines/Flow;
- KTX extensions;
- Room suspend/Flow APIs;
- Lifecycle scopes;
- Navigation Kotlin DSL.

10. **Недоліки теж є**

Kotlin не “магічно кращий” у всьому:

- повільніша компіляція в деяких проєктах;
- складні generics/type inference edge cases;
- треба розуміти coroutines;
- Java interop може давати platform types;
- надмірне використання DSL/extensions може погіршити читабельність.

**Коротко:** Kotlin дає null safety, менше boilerplate, data/sealed classes, extension functions, coroutines, smart casts, named/default arguments і сильну інтеграцію з modern Android. Його головна перевага — виразніший і безпечніший код, але якість все одно залежить від архітектури й дисципліни команди.

</details>
<details>
<summary>216. У чому різниця між val та const val?</summary>

#### Kotlin

`val` і `const val` обидва означають read-only значення, але працюють на різних рівнях. `val` — звичайна read-only property, значення якої може бути обчислене runtime. `const val` — compile-time constant, відоме під час компіляції.

1. **val**

```kotlin
val name = "Alex"
val createdAt = System.currentTimeMillis()
```

`val` не можна перевизначити після ініціалізації, але значення може бути результатом runtime-обчислення.

2. **const val**

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

`const val` має бути відоме на етапі компіляції. Воно inline-иться в місця використання як константа.

3. **Де можна оголошувати const val**

`const val` можна оголошувати тільки:

- top-level;
- в `object`;
- в `companion object`.

```kotlin
object ApiConfig {
    const val BASE_PATH = "api/v1"
}
```

Не можна оголосити `const val` як local variable всередині функції.

4. **Які типи підтримує const val**

`const val` підтримує тільки primitive-like compile-time типи:

- `String`;
- primitives: `Int`, `Long`, `Boolean`, `Double` тощо.

Не можна:

```kotlin
const val DATE = LocalDate.now() // помилка
```

5. **Runtime vs compile-time**

```kotlin
val runtimeValue = UUID.randomUUID().toString()
const val compileTimeValue = "fixed"
```

`runtimeValue` рахується під час виконання. `compileTimeValue` відомий компілятору.

6. **const val і annotations**

`const val` можна використовувати там, де потрібна compile-time constant:

```kotlin
const val TABLE_USERS = "users"

@Entity(tableName = TABLE_USERS)
data class UserEntity(...)
```

Звичайний `val` для такого не підійде, якщо annotation вимагає константу.

7. **Java interop**

`const val` компілюється як static final field. Java-код може використовувати його як константу.

8. **Ризик inline constants**

Оскільки `const val` inline-иться, при зміні значення в library інші модулі можуть потребувати recompilation. Інакше вони можуть бачити старе значення.

9. **Коли що використовувати**

- `val` — майже завжди для read-only значень.
- `const val` — тільки для справжніх compile-time constants.
- Для secrets/API keys `const val` не дає безпеки: значення легко витягнути з APK.

10. **Практичне правило**

Якщо значення обчислюється runtime або є object — використовуй `val`. Якщо це простий literal, потрібний у annotations або як stable constant — `const val`.

**Коротко:** `val` — read-only property, яка може бути runtime-значенням. `const val` — compile-time constant для `String`/primitives, доступна top-level або в object/companion object і може використовуватись в annotations. `const val` не є способом захисту secrets.

</details>
<details>
<summary>217. Які типи можна використовувати з const val?</summary>

#### Kotlin

`const val` оголошує compile-time constant. Його типом може бути `String` або primitive type, а значення має бути відоме компілятору без виконання програми.

1. **Дозволені типи**

З `const val` можна використовувати:

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

Тип можна вказати явно або дозволити inference.

2. **Типові приклади**

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Так зберігають keys, route names, table names, limits і flags, якщо вони не залежать від runtime.

3. **Де оголошувати**

`const val` дозволений top-level, в `object` або `companion object`:

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

Він не може бути local variable або instance property класу.

4. **Що не дозволено**

Objects, collections, nullable types і runtime-вирази не є compile-time constants:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val UUID_VALUE = UUID.randomUUID()
const val DEFAULT_THEME = ThemeMode.Light
const val NAME: String? = null
```

Для таких значень використовують звичайний `val`:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

5. **Constant expressions**

Initializer може містити literals, інші constants і дозволені compile-time operations:

```kotlin
const val HOST = "example.com"
const val API_URL = "https://" + HOST
const val TIMEOUT_SECONDS = 5 * 2
```

Function calls, доступ до `context` або поточного часу не дозволені.

6. **Використання в annotations**

Annotation arguments часто вимагають compile-time constant:

```kotlin
const val USERS_TABLE = "users"
```

```kotlin
@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

Звичайний `val` не підходить, якщо annotation очікує compile-time value.

**Коротко:** `const val` підтримує `String` і primitive types із compile-time initializer. Для objects, collections, nullable або runtime values використовують звичайний `val`.

</details>
<details>
<summary>218. Чи можна створити data class без параметрів?</summary>

#### Kotlin

Ні. Primary constructor `data class` повинен містити щонайменше один parameter, і кожен parameter має бути `val` або `var`.

Некоректно:

```kotlin
data class Empty
data class Empty()
data class User(id: String)
```

Коректно:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

### Чому constructor обов'язковий

Compiler генерує methods лише з primary-constructor properties:

```text
equals()
hashCode()
toString()
copy()
componentN()
```

Саме вони визначають value semantics.

Property у body не входить у generated equality, hash, copy і destructuring:

```kotlin
data class User(
    val id: String
) {
    var cachedLabel: String = ""
}
```

Objects з однаковим `id`, але різним `cachedLabel`, рівні. `copy()` створить body property заново через initializer.

Value-defining fields мають бути в primary constructor.

### Виклик без arguments

Можна задати defaults:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)

val user = User()
```

Class усе одно має два parameters, але caller використовує defaults. На JVM, якщо defaults є для всіх parameters, генерується parameterless constructor.

Defaults на кшталт `id = ""` не повинні створювати invalid domain object лише заради framework.

### Frameworks

Для framework із no-arg requirement використовують adapter, окрему persistence model або Kotlin no-arg plugin.

### Стан без payload

Якщо case не має data, потрібен singleton:

```kotlin
data object Loading
```

У закритій hierarchy:

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

`object` створює singleton. `data object` додає data-like `toString()`/equality semantics та симетрично виглядає в sealed hierarchy.

Для state без payload singleton зазвичай точніший за пустий regular class.

### Інші обмеження data class

Data class не може бути `abstract`, `open`, `sealed` або `inner`, але може реалізовувати interfaces.

### Практичний вибір

- value model — `data class`;
- no-arg call — valid defaults;
- state без payload — `data object`;
- framework no-arg — adapter/plugin;
- value property — primary constructor.

**Коротко:** empty `data class` не компілюється: потрібен хоча б один `val/var` parameter. Defaults дозволяють виклик без arguments, а state без payload моделюють `object`/`data object`.

</details>
<details>
<summary>219. У чому різниця між sealed class та enum?</summary>

#### Kotlin

Обидва задають обмежений набір варіантів, але моделюють різне:

```text
enum   -> фіксовані singleton entries одного типу
sealed -> закрита ієрархія різних subtype-ів
```

### Enum

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

Enum доречний, коли cases мають однакову структуру й не потребують різного payload. Кожен entry — єдиний instance:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Enum може мати спільні properties і methods:

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

Усі entries мають той самий property contract. Enum також має built-in API: `entries`, `valueOf()`, `name`, `ordinal`.

`ordinal` не слід зберігати в DB/API: зміна порядку entries змінить значення. Для persistence краще стабільний explicit code.

### Sealed type

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

Кожен subtype має власні fields і behavior. `Loading` — singleton, а `Content` має багато instances із різними users.

Це підходить для UI state, domain result, events та errors, де variants мають різний payload.

Direct subclasses sealed type відомі compiler-у та мають бути оголошені в дозволених межах Kotlin module/package. Ієрархію не можна довільно розширити з іншого module, тому compiler може перевіряти повноту cases.

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

Якщо додати новий case, compiler покаже місця, де exhaustive `when` треба оновити. Зайвий `else` може приховати цю користь.

### Sealed class vs sealed interface

`sealed class` може мати constructor, stored state і protected members:

```kotlin
sealed class AppError(
    open val code: Int
)
```

`sealed interface` не має constructor state, але implementation може реалізувати кілька interfaces і успадкувати інший class. Для state/result hierarchies часто достатньо `sealed interface`.

### Serialization

Enum зазвичай серіалізується як стабільне textual/code value. Sealed hierarchy потребує discriminator, щоб визначити subtype, та окремої schema для payload. Library має бути явно налаштована на versioning і unknown variants.

### Практичний вибір

Enum:

- theme, sort order, fixed mode;
- simple status без різного payload;
- потрібні `entries/valueOf`;
- один instance на case.

Sealed:

- UI/result state;
- domain outcomes та errors;
- variants із різними fields;
- object і data class cases в одній hierarchy.

Не потрібно замінювати enum sealed-типом лише «на майбутнє»: вибір визначає поточна model.

**Коротко:** enum — фіксований список singleton entries зі спільною структурою. Sealed class/interface — закрита subtype hierarchy, де кожен case може мати власний payload. Для простих modes — enum, для state/result models — sealed.

</details>
<details>
<summary>220. Що таке WorkManager?</summary>

#### Kotlin

`WorkManager` — це Jetpack API для гарантованої відкладеної background work. Його використовують для задач, які мають виконатись навіть якщо застосунок закрили або процес був убитий, але задача не обовʼязково має стартувати миттєво.

1. **Для чого потрібен WorkManager**

WorkManager підходить для:

- background sync;
- upload/download з retry;
- cleanup старих даних;
- відправки logs/analytics;
- periodic sync;
- задач із constraints: network, charging, battery not low.

Якщо задача user-visible і має виконуватись прямо зараз, може знадобитись foreground service.

2. **OneTimeWorkRequest**

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .build()

WorkManager.getInstance(context).enqueue(request)
```

Це одноразова задача.

3. **CoroutineWorker**

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

`Result.success()` — задача виконана. `Result.retry()` — повторити пізніше. `Result.failure()` — завершити з помилкою.

4. **Constraints**

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresBatteryNotLow(true)
    .build()
```

WorkManager запустить задачу тільки коли constraints виконані.

5. **PeriodicWorkRequest**

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodic work не гарантує точний час запуску. Android сам вибирає оптимальний момент з урахуванням battery і system policy.

6. **Unique work**

Щоб не запускати дублікати:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` залишає поточну задачу, `REPLACE` скасовує стару і додає нову.

7. **Input data**

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Передавати треба невеликі primitive/string values. Великі обʼєкти краще зберігати в DB і передавати id.

8. **Work chaining**

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

Так можна будувати послідовність background-задач.

9. **Коли не підходить**

WorkManager не підходить для:

- точних alarm-ів;
- постійного socket connection;
- media playback;
- realtime tracking;
- задач, які мають виконуватись негайно;
- роботи, яка має йти постійно у foreground без перерв.

10. **Практичне правило**

WorkManager — для deferrable, guaranteed background work. Якщо задачу можна відкласти, але треба надійно виконати з constraints/retry, це хороший кандидат для WorkManager.

**Коротко:** WorkManager — Jetpack API для гарантованої відкладеної background work. Він підтримує constraints, retry, periodic work, unique work і chaining. Його використовують для sync/upload/cleanup, але не для точних alarm-ів, realtime-задач, media playback або постійних foreground-сценаріїв.

</details>
<details>
<summary>221. Коли варто використовувати WorkManager?</summary>

#### Kotlin

WorkManager обирають для persistent deferrable work: operation може стартувати пізніше, але має продовжити планування після process death або reboot та виконатися за constraints.

Сценарії: sync, retry upload, cleanup, logs і periodic refresh. Exact time не гарантується; force stop та uninstall припиняють work.

### One-time work

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .setConstraints(
        Constraints.Builder()
            .setRequiredNetworkType(NetworkType.CONNECTED)
            .build()
    )
    .build()

WorkManager.getInstance(context)
    .enqueue(request)
```

Request persist-иться та передається system scheduler-у.

### CoroutineWorker

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

- `success()` — завершено;
- `retry()` — transient failure;
- `failure()` — permanent failure.

Worker має бути idempotent: його можуть зупинити та запустити повторно. Cancellation не перетворюють на retry.

### Constraints та unique work

Constraints описують network, charging, battery/storage conditions, але не момент запуску. Для retries налаштовують backoff.

Unique work запобігає duplicate sync:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "user-sync",
        ExistingWorkPolicy.KEEP,
        request
    )
```

Policy залишає, замінює або продовжує existing work/chain.

### Periodic work

Periodic work виконується приблизно: Doze, battery policy та constraints зміщують запуск. Exact user alarm потребує `AlarmManager`, якщо use case має право на точність.

### Input data

`Data` використовують для невеликих primitive/String values. Великі objects зберігають у Room/file, а Worker отримує ID і читає актуальні дані після restart.

### Коли не підходить

```text
screen-bound work -> ViewModel/lifecycle scope
exact alarm       -> AlarmManager
media/tracking    -> Foreground Service
realtime socket   -> active runtime architecture
```

Expedited work має quotas і не перетворює WorkManager на realtime API.

**Коротко:** WorkManager потрібен, якщо idempotent operation можна відкласти, але важливо виконати після закриття UI з constraints/retry. Для UI work, realtime, media та exact alarms використовують інші механізми.

</details>
<details>
<summary>222. Чи працювали ви з Android Media3 / ExoPlayer?</summary>

#### Kotlin

Так. `Media3` — Jetpack media stack, а `ExoPlayer` — default implementation інтерфейсу `Player`. Він підтримує local/streaming media, playlists, adaptive formats, subtitles та DRM.

1. **Базове використання**

```kotlin
val player = ExoPlayer.Builder(context).build()
player.setMediaItem(MediaItem.fromUri(videoUrl))
player.prepare()
player.play()
```

У View UI player задають через `playerView.player = player`.

2. **Lifecycle і ресурси**

Owner має гарантовано викликати `release()`. Для короткого video це може бути screen/holder, для background playback — service. Reference на `PlayerView` не повинен переживати `onDestroyView()`.

3. **Compose**

Media3 має Compose UI-модулі. Legacy `PlayerView` можна обгорнути:

```kotlin
AndroidView(
    factory = { PlayerView(it).apply { player = exoPlayer } }
)
```

Player не створюють на recomposition: потрібен стабільний owner та cleanup через `DisposableEffect`, holder або service.

4. **Playlist і стан**

```kotlin
val items = videos.map {
    MediaItem.Builder()
        .setUri(it.url)
        .setMediaId(it.id)
        .build()
}
player.setMediaItems(items)
player.prepare()
```

`Player.Listener` повідомляє про `BUFFERING`, `READY`, `ENDED`, errors та інші events. UI мапить їх у loading/error/replay state; analytics не змішують із rendering logic.

5. **Background playback**

Для music/podcast `Player` і `MediaSession` розміщують у `MediaSessionService`, а UI підключається через `MediaController`. Це забезпечує:

- media notification і system controls;
- Bluetooth/Wear OS/Android Auto integration;
- незалежність від lifecycle screen.

Довге відтворення не повинно належати `Activity` або `Fragment`.

6. **Caching**

Для streaming використовують `CacheDataSource`, явно визначаючи size, eviction, offline behavior та DRM constraints.

7. **Типові помилки**

- відсутній `release()` або player створюється на recomposition;
- UI reference переживає `onDestroyView()`;
- background playback належить screen;
- не обробляються errors/audio focus;
- player викликається не з його application thread.

**Коротко:** Media3/ExoPlayer потребує чіткого owner-а, `release()`, обробки states/errors та application-thread access. Background playback реалізують через `MediaSessionService`, а не screen lifecycle.

</details>
<details>
<summary>223. Як би ви розподілили компоненти по модулях, якщо є кнопка, яка по кліку завантажує дані?</summary>

#### Kotlin

Модулі будують навколо feature/use case, а не кнопки:

```text
Button -> ViewModel -> UseCase -> Repository -> API/DB
```

UI лише надсилає event і рендерить state. Він не знає, чи data прийшла з network, Room або cache.

### Структура

Для невеликого app:

```text
:app
:core:network
:core:database
:feature:profile
```

У `:feature:profile` достатньо packages `presentation/domain/data`. Окремі Gradle modules для кожного шару не потрібні.

Для великої feature з окремою ownership:

```text
:feature:profile:api
:feature:profile:impl
```

`api` містить лише public navigation/capability contracts, `impl` — screen, ViewModel, domain і data implementation.

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

- обробляє click;
- запускає coroutine;
- керує loading/content/error;
- визначає policy duplicate clicks;
- переживає rotation через StateFlow.

Retrofit і SQL у presentation бути не повинні.

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

Use case потрібен, якщо є business rule, orchestration або reuse. Якщо це лише proxy одного method у маленькій feature, ViewModel може залежати від repository contract напряму.

Domain не залежить від Android UI, Retrofit, Room або DI.

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

Data layer володіє API, DB, cache/source-of-truth, mappings та infrastructure error mapping.

Core network/database надають reusable clients, але не містять profile business logic.

### Dependency direction

```text
app -> feature implementation
presentation -> domain contract
data -> domain contract
core -X-> feature
```

DI зв'язує implementation із contract у composition root:

```kotlin
@Binds
abstract fun bindRepository(
    impl: ProfileRepositoryImpl
): ProfileRepository
```

Одна feature не повинна залежати від internal classes іншої. Для взаємодії використовують narrow API/navigation contract.

### Критерій модульності

Окремий module виправданий, якщо дає:

- independent ownership;
- stable public API;
- dependency isolation;
- reusable capability;
- build benefit.

Інакше packages простіші. Надмірна modularization додає Gradle, DI та navigation boilerplate.

**Коротко:** кнопка й state живуть у feature presentation, business operation — у domain/use case, API/DB та repository implementation — у data. Для малого app достатньо packages; Gradle modules додають лише для реальної isolation та ownership.

</details>
