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

Обидва оператори об'єднують `Flow`, але мають різну семантику:

```text
combine -> кожне нове значення + latest values інших flows
zip     -> перше з першим, друге з другим, ...
```

### combine

`combine` чекає, поки кожен upstream емітить хоча б одне значення. Після цього кожен новий emission будь-якого flow створює результат з останніми значеннями інших:

```kotlin
val state: Flow<SearchState> =
    combine(
        queryFlow,
        filtersFlow,
        sortOrderFlow
    ) { query, filters, sortOrder ->
        SearchState(query, filters, sortOrder)
    }
```

Якщо змінився лише `query`, оператор використає новий query та останні filters і sort order. Це потрібна семантика для UI state, form fields, settings та filters.

Швидкий upstream може створити багато результатів, не чекаючи нового значення від повільного після їхніх перших emissions.

### zip

`zip` формує пари за порядком emission-ів:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2, 3)) { letter, number ->
        "$letter$number"
    }
    .collect(::println)
```

Результат:

```text
A1
B2
C3
```

Якщо один flow швидший, його значення чекають відповідної пари з іншого. Якщо один upstream завершився і нових пар більше не буде, `zip` завершується:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1)) { letter, number -> "$letter$number" }
// emits only A1
```

`zip` потрібен, коли елементи двох послідовностей логічно відповідають один одному за індексом, наприклад questions та answers.

### Поведінка завершення

Для `combine` після першого значення кожного upstream завершений flow може продовжувати брати участь своїм останнім значенням, поки інші ще емітять. Результуючий flow завершується після завершення всіх upstream-ів.

Якщо upstream завершився, не емітивши жодного значення, повноцінна комбінація неможлива.

`zip` не зберігає latest value для повторного використання: кожен emission входить максимум в одну пару.

### Типова помилка

Для screen state зазвичай неправильно використовувати `zip`:

```kotlin
val state = userFlow.zip(settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

Якщо settings змінюються рідко, нові users чекатимуть нових settings. Правильніше:

```kotlin
val state = userFlow.combine(settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

### Практичний вибір

- результат має оновлюватися при зміні будь-якого source — `combine`;
- потрібна попарна синхронізація послідовностей — `zip`;
- для reactive UI state майже завжди потрібен `combine`;
- для одноразового паралельного отримання двох результатів краще `async/await`, а не Flow operators.

**Коротко:** `combine` реагує на кожне оновлення та використовує latest values усіх потоків. `zip` споживає emissions попарно, тому швидший потік чекає повільніший і результатів буде стільки, скільки доступно повних пар.

</details>
<details>
<summary>66. Для чого потрібні @JvmStatic, @JvmOverloads, @JvmField?</summary>

#### Kotlin

Ці анотації змінюють JVM API, який бачить Java. Для Kotlin callers вони зазвичай не потрібні.

### @JvmStatic

Member companion object без анотації Java викликає через `Companion`:

```kotlin
class UserFactory {
    companion object {
        fun create(): User = User()
    }
}
```

```java
UserFactory.Companion.create();
```

`@JvmStatic` генерує static bridge:

```kotlin
class UserFactory {
    companion object {
        @JvmStatic
        fun create(): User = User()
    }
}
```

```java
UserFactory.create();
```

Анотацію застосовують до members `companion object` або `object`, коли Java/framework очікує static method/accessor. У Kotlin виклик і без неї має форму `UserFactory.create()`.

### @JvmOverloads

Java не підтримує Kotlin default arguments. Анотація генерує overload-и, послідовно прибираючи default parameters із кінця:

```kotlin
@JvmOverloads
fun showMessage(
    text: String,
    duration: Int = 3,
    important: Boolean = false
)
```

Java отримує еквіваленти:

```java
showMessage(String text);
showMessage(String text, int duration);
showMessage(String text, int duration, boolean important);
```

Якщо default parameter знаходиться перед обов'язковим, генерація зупиняється на ньому, тому Java API треба проектувати явно.

Типовий Android-приклад:

```kotlin
class AvatarView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr)
```

Це генерує constructors для Java та XML inflation. Для public library інколи краще написати overload-и вручну, щоб стабільно контролювати binary API.

### @JvmField

Kotlin property зазвичай представлена getter-ом/setter-ом для Java:

```kotlin
class Config {
    val apiUrl = "https://example.com"
}
```

```java
config.getApiUrl();
```

`@JvmField` відкриває backing field напряму без accessors:

```kotlin
class Config {
    @JvmField
    val apiUrl = "https://example.com"
}
```

```java
config.apiUrl;
```

У companion object:

```kotlin
class Constants {
    companion object {
        @JvmField
        val DEFAULT_TIMEOUT = Duration.ofSeconds(5)
    }
}
```

Java використовує `Constants.DEFAULT_TIMEOUT`.

`@JvmField` не підходить для property із custom getter/setter, delegated property або там, де потрібна інкапсуляція. Вона робить field частиною binary API.

Для compile-time primitive/String constant краще `const val`:

```kotlin
const val API_VERSION = "v1"
```

Top-level `const val` або constant в object/companion компілюється як static final field та може inline-итися в caller bytecode, що важливо при зміні library constants.

### Коли застосовувати

- `@JvmStatic` — Java має бачити static method;
- `@JvmOverloads` — Java callers потребують overload-и для default args;
- `@JvmField` — Java/framework потребує прямий field access;
- `const val` — справжня compile-time constant;
- Kotlin-only API — зазвичай без цих анотацій.

Додавати їх «про всяк випадок» не слід: вони збільшують кількість JVM members і публічну binary surface.

**Коротко:** `@JvmStatic` генерує static bridge, `@JvmOverloads` — Java overload-и для default parameters, `@JvmField` — прямий field без getter/setter. Вони потрібні для Java/framework interoperability, а не для звичайного Kotlin API.

</details>
<details>
<summary>67. Що таке делегування (delegation)?</summary>

#### Kotlin

Делегування передає виконання contract-а іншому object. Це composition: клас має implementation, а не успадковує її. Kotlin підтримує class delegation і delegated properties через `by`.

### Class delegation

```kotlin
interface Logger {
    fun log(message: String)
}

class ConsoleLogger : Logger {
    override fun log(message: String) =
        println(message)
}

class UserService(
    private val logger: Logger
) : Logger by logger {

    fun createUser(name: String) {
        log("Creating user: $name")
    }
}
```

Compiler генерує forwarding methods `Logger`. Окремий method можна змінити у wrapper:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {

    override fun log(message: String) {
        delegate.log("[App] $message")
    }
}
```

Delegation спрощує заміну implementation, тестування та комбінування behaviours без жорсткого зв'язку з base class.

Нюанс: internal calls самого delegate не перенаправляються на overrides wrapper-а. Це forwarding зовнішніх calls, а не inheritance.

### Delegated properties

`lazy` обчислює value при першому доступі:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

Режими `lazy`:

- `SYNCHRONIZED` — thread-safe за замовчуванням;
- `PUBLICATION` — initializer може виконатися кілька разів;
- `NONE` — без synchronization для single-thread access.

`observable` реагує на зміну, `vetoable` може її відхилити:

```kotlin
var age: Int by Delegates.vetoable(0) { _, _, new ->
    new >= 0
}
```

### Custom delegate

Для `val` реалізують `getValue`, для `var` — також `setValue`:

```kotlin
class TrimmedString {
    private var value = ""

    operator fun getValue(
        thisRef: Any?,
        property: KProperty<*>
    ): String = value

    operator fun setValue(
        thisRef: Any?,
        property: KProperty<*>,
        newValue: String
    ) {
        value = newValue.trim()
    }
}

var name: String by TrimmedString()
```

Delegate може централізувати validation або state access, але прихована I/O чи складна бізнес-логіка в property робить API неочевидним.

### Android

```kotlin
private val viewModel: UserViewModel by viewModels()
```

`viewModels()` повертає delegate, який отримує ViewModel з відповідного `ViewModelStoreOwner`.

Delegated value має враховувати lifecycle. Наприклад, Fragment view binding не можна зберігати звичайним `lazy` після `onDestroyView()`; потрібне явне очищення або lifecycle-aware delegate.

### Практичні правила

- delegation — для замінної поведінки та composition;
- inheritance — для стабільного «is-a» contract-а;
- class delegation прибирає forwarding boilerplate;
- property delegate має мати прозору й дешеву семантику;
- враховувати thread-safety `lazy` та lifecycle owner-а;
- не делегувати великий interface без потреби.

**Коротко:** class delegation через `by` передає реалізацію interface іншому object, а property delegates керують читанням і записом властивостей. Це зменшує coupling із inheritance, але потребує явних lifecycle та side-effect semantics.

</details>
<details>
<summary>68. Які фреймворки для тестування доступні в Kotlin?</summary>

#### Kotlin

У Kotlin інструменти вибирають за рівнем тесту: unit, coroutine/Flow, Android integration або UI.

1. **JUnit і kotlin.test**

`JUnit` — типовий runner і framework для JVM unit tests:

```kotlin
class CalculatorTest {
    @Test
    fun `adds numbers`() {
        assertEquals(4, Calculator().sum(2, 2))
    }
}
```

`kotlin.test` надає multiplatform-friendly annotations і assertions, які мапляться на test framework конкретної платформи:

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals

class UserTest {
    @Test
    fun `creates user`() {
        assertEquals("Alex", User("Alex").name)
    }
}
```

2. **Mocking: MockK і Mockito**

`MockK` має Kotlin-friendly API і підтримує suspend-функції:

```kotlin
val repository = mockk<UserRepository>()
coEvery { repository.getUser() } returns User("1")
```

`Mockito` також можна використовувати з Kotlin:

```kotlin
val repository = mock(UserRepository::class.java)
```

Для domain logic часто простіший fake, бо він зменшує coupling тесту до implementation details.

3. **Kotest**

`Kotest` дає DSL-стилі тестів і багато matchers:

```kotlin
class UserSpec : StringSpec({
    "user has name" {
        User("Alex").name shouldBe "Alex"
    }
})
```

4. **Coroutines і Flow**

`kotlinx-coroutines-test` надає `runTest`, test dispatchers і virtual time:

```kotlin
@Test
fun `loads data`() = runTest {
    viewModel.load()
    advanceUntilIdle()
    assertEquals(expected, viewModel.state.value)
}
```

`Turbine` спрощує перевірку послідовності emissions у `Flow`:

```kotlin
flow.test {
    assertEquals(Loading, awaitItem())
    assertEquals(Success, awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

5. **Android tests**

AndroidX Test містить core API, runner, rules і JUnit integration. Instrumentation tests запускаються на device/emulator, коли потрібен реальний Android framework.

`Espresso` тестує View-based UI:

```kotlin
onView(withId(R.id.loginButton)).perform(click())
onView(withText("Success")).check(matches(isDisplayed()))
```

Compose UI Test працює із semantics tree:

```kotlin
composeTestRule
    .onNodeWithText("Login")
    .performClick()
```

6. **Robolectric**

`Robolectric` запускає частину Android tests на JVM без emulator. Це швидше, але device-specific поведінку треба перевіряти instrumentation tests.

7. **Практичний вибір**

- Pure Kotlin logic — JUnit/kotlin.test.
- Kotlin mocks — MockK.
- Coroutines — kotlinx-coroutines-test.
- Flow — Turbine.
- ViewModel — unit tests із fake dependencies.
- View UI — Espresso;
- Compose UI — Compose UI Test.
- Android framework без emulator — Robolectric.
- Device integration — instrumentation tests.

**Коротко:** JUnit або `kotlin.test` покривають unit tests, `kotlinx-coroutines-test` і Turbine — async code, Espresso та Compose UI Test — UI, а Robolectric/AndroidX Test — Android integration.

</details>
<details>
<summary>69. Як мокати залежності у тестах?</summary>

#### Kotlin

Мокання залежностей у тестах — це заміна real dependency на fake/mock/stub, щоб тестувати клас ізольовано. У Kotlin для цього часто використовують MockK, Mockito або manual fakes.

1. **Навіщо мокати залежності**

Якщо use case залежить від API, DB або analytics, тест не має ходити в реальну мережу чи базу:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

У тесті `AuthRepository` можна замінити fake або mock.

2. **MockK**

MockK добре підтримує Kotlin і coroutines:

```kotlin
val repository = mockk<AuthRepository>()

coEvery { repository.login("a@b.com", "123") } returns User("1")

val useCase = LoginUseCase(repository)
val result = useCase("a@b.com", "123")

assertEquals(User("1"), result)
```

Для suspend functions використовують `coEvery`, `coVerify`.

3. **Mockito**

Mockito теж можна використовувати:

```kotlin
val repository = mock<AuthRepository>()
whenever(repository.getUser()).thenReturn(user)
```

У Kotlin часто зручніше брати `mockito-kotlin`, бо чистий Java Mockito менш зручний з nullability і final classes.

4. **Fake замість mock**

Fake — це проста тестова implementation:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(email: String, password: String): Result<User> {
        return result
    }
}
```

Fake часто стабільніший за mock, бо тестує behavior, а не деталі викликів.

5. **Stub vs Mock vs Fake**

```text
Stub -> повертає підготовлені дані
Mock -> перевіряє interactions/calls
Fake -> проста робоча implementation для тестів
```

Для domain/use case тестів fake часто кращий. Для перевірки “чи був викликаний analytics” може бути доречний mock.

6. **Перевірка викликів**

```kotlin
coVerify { repository.login("a@b.com", "123") }
```

Не треба перевіряти кожен внутрішній виклик. Перевіряй interactions тільки там, де це частина очікуваної поведінки.

7. **Мокання dispatchers**

Краще не мокати `Dispatchers`, а інжектити їх:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
)
```

У тесті:

```kotlin
val dispatcher = StandardTestDispatcher(testScheduler)
```

8. **Hilt/Koin у тестах**

Hilt дозволяє заміняти modules через `@TestInstallIn`. Koin — завантажувати test modules:

```kotlin
loadKoinModules(module {
    single<AuthRepository> { FakeAuthRepository() }
})
```

9. **Що не варто мокати**

Не варто мокати data classes, pure functions, прості value objects і те, що легко створити реально. Моки для всього роблять тести крихкими.

10. **Практичне правило**

- Для external dependencies — fake/mock.
- Для suspend functions у MockK — `coEvery`/`coVerify`.
- Для behavior tests — fake часто краще.
- Не перевіряти implementation details без потреби.
- Dependencies мають приходити через constructor injection.

**Коротко:** залежності в тестах мокають через MockK/Mockito або замінюють manual fakes. Mock корисний для перевірки interactions, fake — для стабільних behavior tests. Щоб це працювало, production-код має використовувати constructor injection і залежати від contracts, а не concrete implementations.

</details>
<details>
<summary>70. Як тестувати корутини та Flow?</summary>

#### Kotlin

Coroutine-тести будують на `kotlinx-coroutines-test`; Flow emissions зручно перевіряти через Turbine. Test scheduler керує tasks і virtual time без реального очікування.

### runTest і virtual time

```kotlin
@Test
fun `loads user`() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` створює `TestScope`, пропускає звичайні `delay()` і чекає child coroutines. Scheduler можна контролювати:

```kotlin
service.start()

advanceTimeBy(1_000)
runCurrent()

assertEquals(2, fakeApi.callCount)
```

- `runCurrent()` виконує tasks у поточний virtual time;
- `advanceTimeBy()` пересуває час;
- `advanceUntilIdle()` виконує queue до idle.

Для нескінченного polling `advanceUntilIdle()` не використовують: час рухають обмежено, а job скасовують.

### Dispatcher injection

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher,
    private val api: UserApi
) {
    suspend fun loadUser(): User =
        withContext(ioDispatcher) { api.getUser() }
}
```

У тесті dispatcher створюють зі scheduler-а поточного `runTest`:

```kotlin
val dispatcher = StandardTestDispatcher(testScheduler)
```

Усі test dispatchers одного тесту мають використовувати спільний scheduler. `StandardTestDispatcher` ставить tasks у queue, тому часто потрібен `runCurrent()` або `advanceUntilIdle()`.

`UnconfinedTestDispatcher` запускає нові coroutines eagerly, але може приховати scheduling order, тому не є універсальною заміною.

### Dispatchers.Main

ViewModel через `viewModelScope` залежить від Main. У local unit tests його підміняють JUnit rule-ом:

```kotlin
override fun starting(description: Description) {
    Dispatchers.setMain(testDispatcher)
}

override fun finished(description: Description) {
    Dispatchers.resetMain()
}
```

Action і assertions мають використовувати scheduler цього Main dispatcher-а.

### Flow і Turbine

```kotlin
@Test
fun `emits loading then content`() = runTest {
    viewModel.state.test {
        assertEquals(UiState.Loading, awaitItem())

        viewModel.load()
        assertEquals(UiState.Content(user), awaitItem())

        cancelAndIgnoreRemainingEvents()
    }
}
```

Turbine перевіряє порядок, completion та errors через `awaitItem()`, `awaitComplete()`, `awaitError()`. Нескінченну collection треба скасувати.

Якщо важливий лише фінальний `StateFlow` state:

```kotlin
viewModel.load()
advanceUntilIdle()
assertEquals(UiState.Content(user), viewModel.state.value)
```

Для перевірки проміжних transitions collector треба запустити до action. StateFlow conflates значення, а `SharedFlow(replay = 0)` може втратити event без active subscriber-а.

### Типові помилки

- `Thread.sleep()` або real delay;
- hardcoded `Dispatchers.IO/Main`;
- різні test schedulers в одному тесті;
- assertions до виконання queued tasks;
- змішування real і test dispatchers;
- незавершені jobs або Flow collections;
- тестування лише final state, коли важливий порядок emissions.

**Коротко:** використовуйте `runTest`, shared `TestCoroutineScheduler` і virtual time. Dispatchers інжектяться, Main підміняється, фінальний `StateFlow.value` перевіряється напряму, а послідовність Flow emissions — через Turbine.

</details>
<details>
<summary>71. Які best practices для написання тестованого коду?</summary>

#### Kotlin

Тестований код — це код із чіткими залежностями, маленькими відповідальностями, передбачуваною поведінкою і мінімальною привʼязкою до Android framework. Його можна перевірити unit-тестами без emulator там, де це можливо.

1. **Dependency Injection**

Клас має отримувати залежності ззовні:

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Погано створювати залежності всередині:

```kotlin
class LoginUseCase {
    private val repository = AuthRepositoryImpl()
}
```

DI дозволяє підмінити real dependency на fake/mock у тесті.

2. **Малі відповідальності**

Клас має робити одну зрозумілу річ. Якщо ViewModel одночасно робить network, database, mapping і navigation logic — її важко тестувати.

Краще:

```text
ViewModel -> UseCase -> Repository -> DataSource
```

3. **Interfaces для нестабільних залежностей**

```kotlin
interface Clock {
    fun now(): Instant
}
```

У тесті:

```kotlin
class FakeClock : Clock {
    override fun now(): Instant = fixedInstant
}
```

Так само абстрагують API, DB, dispatchers, analytics, file system.

4. **Не хардкодити Dispatchers**

Погано:

```kotlin
withContext(Dispatchers.IO) { ... }
```

Краще інжектити dispatcher:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher
)
```

У тесті можна передати `StandardTestDispatcher`.

5. **Pure functions**

Чиста функція легше тестується:

```kotlin
fun calculateDiscount(price: BigDecimal, user: User): BigDecimal
```

Вона не залежить від часу, network, database або Android context.

6. **Mapper layer**

DTO/entity краще мапити окремо:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name ?: "Unknown")
```

Mapper легко тестувати окремими unit-тестами.

7. **ViewModel тестувати через state**

ViewModel краще перевіряти через public state/effects:

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()
assertEquals(LoginState.Success, viewModel.state.value)
```

Не треба тестувати private methods напряму. Тест має перевіряти observable behavior.

8. **Fake замість надмірних mocks**

Для repository часто fake краще за mock:

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))
    override suspend fun login(): Result<User> = result
}
```

Fake дає стабільніші тести і менше привʼязки до implementation details.

9. **Android framework ізолювати**

Бізнес-логіка не має напряму залежати від `Context`, `Resources`, `Activity`. Якщо треба resource/string/provider — зробити abstraction.

10. **Практичні правила**

- Constructor injection за замовчуванням.
- Менше static/global state.
- Чіткі contracts для зовнішніх залежностей.
- Coroutines тестувати через `runTest`.
- Flow тестувати через Turbine.
- Domain logic тримати без Android framework.
- Тестувати поведінку, а не implementation details.

**Коротко:** тестований код має явні залежності, малі відповідальності, інжектовані dispatchers/services, чисті функції, mapper layer, fake dependencies і мінімум Android framework у бізнес-логіці. ViewModel тестують через state/effects, а не через private implementation.

</details>
<details>
<summary>72. Як ефективно керувати залежностями у Kotlin-проєкті?</summary>

#### Kotlin

Ефективне керування залежностями — це контроль версій, конфліктів, scopes, модулів і build performance. У Kotlin/Android проєктах зазвичай це роблять через Gradle, version catalogs, convention plugins і чітку модульну структуру.

1. **Version Catalog**

Сучасний підхід — `libs.versions.toml`:

```toml
[versions]
kotlin = "2.0.0"
coroutines = "1.8.1"

[libraries]
coroutines-core = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-core", version.ref = "coroutines" }
```

У Gradle:

```kotlin
dependencies {
    implementation(libs.coroutines.core)
}
```

Це прибирає дублювання версій по модулях.

2. **Scopes залежностей**

Правильно вибирати scope:

```kotlin
implementation(libs.retrofit)
api(libs.coroutines.core)
testImplementation(libs.junit)
androidTestImplementation(libs.espresso)
```

`api` відкриває dependency назовні для downstream modules, тому його треба використовувати обережно. За замовчуванням краще `implementation`.

3. **Не тягнути зайве в модулі**

Feature-модуль не має залежати від усього app stack. Наприклад, якщо модулю потрібен тільки domain contract, не треба підключати Retrofit/Room.

```text
feature -> domain
feature -/-> data implementation
```

Це зменшує coupling і покращує build time.

4. **Convention plugins**

Для повторюваної Gradle-конфігурації краще використовувати convention plugins:

```text
build-logic/
  convention/
    android-library.gradle.kts
    kotlin-library.gradle.kts
```

Так не треба дублювати `compileSdk`, Kotlin options, Compose setup і test dependencies у кожному модулі.

5. **Dependency Injection**

DI допомагає керувати runtime-залежностями:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt/Koin/manual DI зʼєднує contracts з implementations. Важливо не плутати Gradle dependencies і runtime DI: це різні рівні.

6. **Оновлення версій**

Оновлення залежностей має бути контрольованим:

- читати changelog;
- оновлювати по групах;
- запускати tests/build;
- не оновлювати все одразу без причини;
- слідкувати за breaking changes.

Корисні інструменти: Gradle Versions Plugin, Renovate, Dependabot.

7. **Конфлікти версій**

Для аналізу dependency graph:

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

`dependencyInsight` показує, хто притягнув конкретну бібліотеку і яку версію Gradle обрав.

8. **BOM**

Для бібліотек з багатьма artifacts використовують BOM:

```kotlin
implementation(platform(libs.firebase.bom))
implementation(libs.firebase.analytics)
```

BOM вирівнює сумісні версії всередині ecosystem.

9. **Практичні правила**

- Версії тримати централізовано.
- За замовчуванням використовувати `implementation`, не `api`.
- Feature-модулі не мають тягнути concrete data layer без потреби.
- Повторюваний Gradle setup винести в convention plugins.
- Регулярно перевіряти dependency graph.
- Не додавати бібліотеку заради однієї простої функції.

**Коротко:** залежностями в Kotlin-проєкті керують через Gradle version catalogs, правильні scopes (`implementation`/`api`), convention plugins, контроль dependency graph, BOM для ecosystem-бібліотек і DI для runtime-залежностей. Мета — менше coupling, стабільні версії і швидший build.

</details>
<details>
<summary>73. Як Kotlin працює з памʼяттю та garbage collection?</summary>

#### Kotlin

На Android Kotlin працює поверх JVM/ART, тому керування памʼяттю в основному виконує garbage collector. Обʼєкти створюються в heap, посилання на них живуть у stack/heap, а GC звільняє обʼєкти, до яких більше немає reachable references.

1. **Heap і stack**

```text
stack -> локальні references, call frames
heap  -> обʼєкти, колекції, lambdas, data classes
```

Коли ти створюєш object:

```kotlin
val user = User("1", "Alex")
```

сам `User` лежить у heap, а `user` — reference на нього.

2. **Garbage collection**

GC звільняє обʼєкти, які більше недоступні з GC roots: active threads, stack references, static fields, JNI references тощо.

```kotlin
fun loadUser() {
    val user = User("1", "Alex")
}
```

Після виходу з функції, якщо на `user` більше немає посилань, object може бути зібраний GC.

3. **Kotlin null safety не керує памʼяттю**

```kotlin
var user: User? = User("1", "Alex")
user = null
```

`null` тільки прибирає reference. Памʼять звільнить GC, коли обʼєкт стане unreachable.

4. **Memory leaks**

Leak виникає, коли object більше не потрібен, але на нього ще є reference.

Погано:

```kotlin
object Holder {
    var activity: Activity? = null
}
```

Якщо singleton тримає Activity, вона не звільниться після rotation/finish.

5. **Android-типові leaks**

- Activity/Fragment context у singleton;
- adapter/listener не очищено в `onDestroyView()`;
- callback тримає Fragment після lifecycle;
- coroutine живе довше screen;
- static reference на View;
- anonymous inner class тримає outer class.

6. **onDestroyView у Fragment**

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Fragment може жити довше за свою View, тому View references треба очищати.

7. **Coroutines і памʼять**

Coroutine може тримати references через lambda:

```kotlin
lifecycleScope.launch {
    repository.load()
    binding.text.text = "Done"
}
```

Для UI треба використовувати lifecycle-aware scopes (`viewLifecycleOwner.lifecycleScope`, `repeatOnLifecycle`) і не тримати View після destroy.

8. **Allocations і GC pressure**

Часті allocations у hot path можуть викликати часті GC pauses:

```kotlin
items.map { it.toUi() }.filter { it.visible }
```

Для звичайного коду це нормально. Для scroll/render/audio/video hot paths краще уникати зайвих проміжних обʼєктів.

9. **Kotlin-особливості**

Kotlin може створювати додаткові обʼєкти:

- lambdas/closures;
- boxing primitives у generics;
- data class copies;
- collections operations;
- sequences/iterators;
- delegation.

Це не проблема саме по собі, але в hot paths треба профілювати.

10. **Практичне правило**

- Не тримати Activity/View context у singleton.
- Очищати View references у Fragment.
- Використовувати lifecycle-aware coroutines.
- Не робити зайві allocations у rendering/scroll hot paths.
- Шукати leaks через LeakCanary.
- Оптимізувати памʼять тільки після profiler.

**Коротко:** Kotlin на Android покладається на ART garbage collector. Обʼєкти живуть у heap і звільняються, коли стають unreachable. Головні ризики — memory leaks через довгі references на Activity/View/Fragment і зайві allocations у hot paths. Для діагностики використовують profiler і LeakCanary.

</details>
<details>
<summary>74. Які best practices роботи з null safety?</summary>

#### Kotlin

Null safety у Kotlin зменшує ризик `NullPointerException`, але не прибирає його повністю. Хороша практика — моделювати nullable тільки там, де значення реально може бути відсутнім, і явно обробляти цей випадок.

1. **Не робити все nullable**

Погано:

```kotlin
data class User(
    val id: String?,
    val name: String?
)
```

Якщо `id` обовʼязковий за доменом, він має бути non-null:

```kotlin
data class User(
    val id: String,
    val name: String?
)
```

Nullable має означати реальну відсутність значення, а не “про всяк випадок”.

2. **Уникати `!!`**

```kotlin
val name = user!!.name
```

`!!` перетворює nullable у potential crash. Його варто уникати, крім дуже рідкісних випадків, де invariant гарантований і crash справді бажаний.

3. **Safe call і Elvis operator**

```kotlin
val name = user?.name ?: "Unknown"
```

Це нормальний спосіб задати fallback для optional value.

4. **requireNotNull / checkNotNull**

Якщо значення має бути non-null за контрактом:

```kotlin
val userId = requireNotNull(args.userId) { "userId is required" }
```

`requireNotNull` добре підходить для input validation, `checkNotNull` — для internal state invariants.

5. **DTO vs Domain**

API може повертати nullable/відсутні поля:

```kotlin
data class UserDto(
    val id: String?,
    val name: String?
)
```

А domain model має бути чистішою:

```kotlin
fun UserDto.toDomain(): User = User(
    id = requireNotNull(id),
    name = name ?: "Unknown"
)
```

Mapper — правильне місце для fallback або fail-fast logic.

6. **Avoid platform type surprises**

Java APIs можуть повертати platform types (`String!`), які Kotlin не знає як точно nullable/non-null. Їх треба перевіряти або обгортати:

```kotlin
val value: String? = javaApi.getValue()
```

7. **let / run / takeIf**

```kotlin
user?.let { safeUser ->
    render(safeUser)
}
```

Це добре для короткої nullable-гілки. Але не треба будувати надто вкладені chains — іноді простий `if (user == null) return` читабельніший.

8. **Early return**

```kotlin
val user = state.user ?: return
render(user)
```

Early return часто робить код простішим, ніж глибока вкладеність.

9. **Null Object / sealed state**

Якщо `null` означає окремий стан, краще змоделювати його явно:

```kotlin
sealed interface ProfileState {
    data object Loading : ProfileState
    data object Empty : ProfileState
    data class Content(val user: User) : ProfileState
}
```

Це краще, ніж кілька nullable-полів у state.

10. **Практичні правила**

- Nullable тільки там, де значення реально optional.
- Не використовувати `!!` без сильної причини.
- DTO може бути nullable, domain краще робити строгим.
- Fallback/fail-fast робити в mapper/use case.
- Для state краще sealed classes, ніж набір nullable fields.
- Java/platform types перевіряти явно.

**Коротко:** null safety у Kotlin працює найкраще, коли nullable моделює реальну відсутність значення. Уникай `!!`, використовуй safe calls, Elvis, `requireNotNull`, mapper layer для DTO, sealed state для UI і явну перевірку Java platform types.

</details>
<details>
<summary>75. Що таке SupervisorScope і чим він відрізняється?</summary>

#### Kotlin

`supervisorScope` — це coroutine scope, у якому помилка однієї дочірньої корутини не скасовує автоматично інші дочірні корутини. Це відрізняє його від звичайного `coroutineScope`, де failure однієї child coroutine скасовує весь scope.

1. **coroutineScope**

У звичайному `coroutineScope` діє all-or-nothing поведінка:

```kotlin
suspend fun loadAll() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }

    UserScreenData(
        user = user.await(),
        posts = posts.await()
    )
}
```

Якщо `getUser()` впаде, `getPosts()` теж буде скасований. Це правильно, коли результат має бути повним.

2. **supervisorScope**

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(
        user = user.await().getOrNull(),
        posts = posts.await().getOrDefault(emptyList())
    )
}
```

Тут помилка одного запиту не скасовує інший. Це корисно, коли можна показати часткові дані.

3. **Головна різниця**

```text
coroutineScope   -> failure child скасовує siblings
supervisorScope  -> failure child не скасовує siblings автоматично
```

Але exception все одно треба обробити, особливо якщо використовується `async` і `await()`.

4. **SupervisorJob**

`SupervisorJob` дає схожу поведінку для scope:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

В Android `viewModelScope` уже використовує supervisor-поведінку, тому падіння однієї child coroutine не обовʼязково вбиває всі інші jobs у ViewModel.

5. **Коли використовувати supervisorScope**

Підходить для незалежних задач:

- завантажити profile і recommendations;
- кілька widgets на одному екрані;
- паралельні optional API calls;
- частковий UI state;
- незалежні background jobs.

6. **Коли краще coroutineScope**

`coroutineScope` краще, коли всі частини результату обовʼязкові:

- checkout calculation;
- transaction flow;
- dependent requests;
- consistency-critical operations;
- ситуації, де частковий результат некоректний.

7. **Помилки з async**

Навіть у `supervisorScope` exception з `async` буде кинутий при `await()`:

```kotlin
val deferred = async { error("boom") }

try {
    deferred.await()
} catch (e: Exception) {
    // handle
}
```

Тому supervisorScope не замінює error handling.

8. **Практичне правило**

Якщо child tasks незалежні й частковий результат допустимий — `supervisorScope`. Якщо failure однієї задачі робить весь результат невалідним — `coroutineScope`.

**Коротко:** `supervisorScope` дозволяє дочірнім корутинам падати незалежно: помилка однієї не скасовує інші автоматично. `coroutineScope` використовують для all-or-nothing задач, `supervisorScope` — для незалежних задач і часткових результатів. Але exceptions усе одно треба обробляти.

</details>
<details>
<summary>76. Що таке backpressure і як його обробляти у Flow?</summary>

#### Kotlin

`Backpressure` — це ситуація, коли producer створює дані швидше, ніж consumer встигає їх обробляти. У Flow це може призвести до затримок, росту buffer-а, зайвої роботи або застарілих UI updates.

1. **Приклад проблеми**

```kotlin
flow {
    repeat(1000) {
        emit(it)
    }
}.collect { value ->
    delay(100)
    println(value)
}
```

Producer emit-ить швидко, consumer обробляє повільно.

2. **buffer**

`buffer()` дозволяє producer і consumer працювати паралельніше:

```kotlin
flow
    .buffer(capacity = 64)
    .collect { value -> process(value) }
```

Це корисно, якщо consumer тимчасово повільніший. Але buffer не вирішує нескінченний потік, якщо consumer завжди відстає.

3. **conflate**

`conflate()` пропускає проміжні значення і залишає останнє:

```kotlin
scrollEvents
    .conflate()
    .collect { position -> render(position) }
```

Підходить для UI state, де важливий останній стан, а не кожне проміжне значення.

4. **collectLatest**

`collectLatest` скасовує попередню обробку, якщо прийшло нове значення:

```kotlin
queryFlow.collectLatest { query ->
    search(query)
}
```

Добре для search, autocomplete, preview rendering: старий результат уже не потрібен.

5. **debounce**

`debounce` чекає паузу перед emit:

```kotlin
queryFlow
    .debounce(300)
    .collectLatest { query -> search(query) }
```

Корисно для input events, щоб не запускати запит на кожну букву.

6. **sample**

`sample` бере останнє значення з інтервалом:

```kotlin
sensorFlow
    .sample(1_000)
    .collect { value -> render(value) }
```

Підходить для частих events, де не потрібна кожна зміна.

7. **distinctUntilChanged**

```kotlin
stateFlow
    .distinctUntilChanged()
    .collect { state -> render(state) }
```

Не пропускає однакові значення і зменшує зайву роботу.

8. **flatMapLatest**

```kotlin
queryFlow.flatMapLatest { query ->
    repository.search(query)
}
```

Старий inner Flow скасовується, коли зʼявляється новий query.

9. **Channel capacity**

Для `callbackFlow` важливо думати про capacity і overflow:

```kotlin
callbackFlow {
    // trySend(...)
    awaitClose { unregister() }
}.buffer(Channel.CONFLATED)
```

Якщо callback дуже частий, без стратегії buffer/overflow можна отримати проблеми.

10. **Практичне правило**

- Потрібне кожне значення — `buffer` або оптимізувати consumer.
- Потрібне тільки останнє — `conflate`.
- Нова подія скасовує стару роботу — `collectLatest`/`flatMapLatest`.
- Input events — `debounce`.
- Часті sensor/scroll events — `sample`/`conflate`.
- Повторні однакові значення — `distinctUntilChanged`.

**Коротко:** backpressure у Flow виникає, коли producer швидший за consumer. Його обробляють через `buffer`, `conflate`, `collectLatest`, `flatMapLatest`, `debounce`, `sample`, `distinctUntilChanged` і правильну buffer strategy у `callbackFlow`. Вибір залежить від того, чи важливе кожне значення, чи тільки останнє.

</details>
<details>
<summary>77. Як використовувати Java Streams у Kotlin?</summary>

#### Kotlin

Kotlin/JVM напряму працює з Java Stream API:

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Для Kotlin collections зазвичай простіші стандартні operators:

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

### Stream vs Sequence

Collection operators eager і можуть створювати проміжні collections. `Sequence` виконує chain lazily:

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Stream і Sequence потребують terminal operation: `toList`, `count`, `first` або `collect`.

Sequence корисний для довгого chain або early termination. Для малої collection eager operators часто читабельніші й дешевші за lazy overhead.

### Коли Stream доречний

- Java API повертає `Stream<T>`;
- потрібен Java `Collector`;
- codebase змішаний Java/Kotlin;
- pipeline уже реалізований як Stream.

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

I/O stream на кшталт `Files.lines()` закривають через `use`. Stream одноразовий: після terminal operation його не використовують повторно.

На Android доступність окремих Java API залежить від minSdk і desugaring configuration.

### Kotlin alternatives

Java collectors часто мають короткий Kotlin-аналог:

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) =
    users.partition(User::isActive)
```

Також доступні `mapNotNull`, `fold`, `joinToString` і `distinctBy`. Kotlin API краще виражає nullability, ніж Java platform types.

### Parallel streams

```kotlin
val result = users.parallelStream()
    .map(::expensiveMap)
    .collect(Collectors.toList())
```

`parallelStream()` використовує common `ForkJoinPool`, ускладнює cancellation та не має Android lifecycle ownership.

У coroutine code CPU parallelism краще контролювати через `Dispatchers.Default` та обмежену кількість tasks.

Для I/O parallel stream не замінює suspend API.

### Stream vs Flow

```text
Stream/Sequence -> synchronous one-shot pipeline
Flow            -> asynchronous, suspending, cancellable stream
```

Stream не підтримує suspension або lifecycle collection. Для DB observations, UI state чи events використовують Flow.

### Практичний вибір

- eager Kotlin data — collection operators;
- lazy synchronous chain — `Sequence`;
- Java interop — `Stream`;
- asynchronous data — `Flow`;
- I/O stream — `use`;
- `parallelStream` — лише після profiling.

**Коротко:** Java Streams доступні Kotlin/JVM і корисні для Java interop. Для Kotlin data зазвичай використовують collection operators або `Sequence`, для asynchronous data — `Flow`.

</details>
<details>
<summary>78. Що таке Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`KMM` — колишня назва mobile-напряму `Kotlin Multiplatform` (`KMP`). KMP дозволяє повторно використовувати Kotlin-код між Android, iOS та іншими targets: від domain/data layer до shared UI.

1. **Основна ідея**

Команда сама визначає межу sharing:

```text
shared Kotlin code
    -> Android app
    -> iOS app
```

Часто виносять:

- domain models;
- use cases;
- repositories;
- validation;
- networking;
- serialization;
- business rules;
- contracts для platform APIs.

UI можна залишити native (`Compose`/Views та SwiftUI/UIKit) або частково/повністю шарити через Compose Multiplatform.

2. **Типова структура**

```text
:shared
    commonMain
    androidMain
    iosMain

:androidApp
iosApp
```

`commonMain` містить platform-independent code:

```kotlin
data class User(
    val id: String,
    val name: String
)

interface UserRepository {
    suspend fun loadUser(id: String): User
}
```

`androidMain` та `iosMain` містять platform-specific code або actual implementations.

3. **expect/actual**

Для platform API можна використати interface + DI або `expect/actual`:

```kotlin
// commonMain
expect class PlatformLogger {
    fun log(message: String)
}

// androidMain
actual class PlatformLogger {
    actual fun log(message: String) = Log.d("App", message)
}
```

Common code бачить єдиний contract, target компілює відповідний `actual`.

4. **Networking і serialization**

Для shared networking підходить Ktor Client, для serialization — `kotlinx.serialization`:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String
)
```

5. **Що не варто шарити**

Не варто створювати abstraction лише заради відсотка shared code. Platform layer часто залишає:

- Android `Context`;
- iOS `UIViewController`;
- platform integrations: permissions, notifications, camera;
- SDK, доступний лише на одному target;
- UX, який має суттєво відрізнятися між platforms.

Boundary має зменшувати duplication і зберігати простий interop.

6. **Переваги**

- одна domain-логіка та tests;
- shared models, validation і networking;
- поступове впровадження;
- вибір між native і shared UI.

7. **Компроміси**

KMP також додає вартість:

- Gradle та target-specific setup;
- Kotlin/Swift interop;
- різна підтримка targets у libraries;
- складніший cross-platform debugging;
- потреба в KMP expertise.

KMP не зменшує вартість автоматично: вигода залежить від обсягу стабільної спільної логіки/UI.

8. **KMM vs Flutter/React Native**

```text
Flutter / React Native -> shared UI + shared logic
KMM/KMP               -> shared logic, native UI
```

На відміну від Flutter/RN, KMP не вимагає shared UI: можна шарити лише logic або додати Compose Multiplatform поступово.

**Коротко:** KMM тепер називають KMP. Android та iOS можуть ділити business logic, infrastructure і за потреби UI через Compose Multiplatform, зберігаючи platform-specific integrations.

</details>
<details>
<summary>79. Які обмеження має Kotlin Multiplatform?</summary>

#### Kotlin

`Kotlin Multiplatform` дозволяє ділитися кодом між платформами, але не означає “написати один app для всього”. Найкраще KMP працює для shared business logic, networking, validation, data models і частини data layer. UI і platform APIs часто залишаються нативними.

1. **Не весь код можна шарити**

У `commonMain` не можна напряму використовувати Android SDK, UIKit, Java-only APIs або platform-specific бібліотеки.

```kotlin
// commonMain не має знати про Android Context
```

Для platform-specific коду потрібні `expect/actual` або interfaces.

2. **expect/actual додає складність**

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

Android/iOS мають власні `actual` реалізації. Це працює, але збільшує кількість коду і потребує дисципліни.

3. **Бібліотеки не завжди multiplatform**

Не кожна Kotlin/Java бібліотека підтримує KMP. Наприклад, Android-only бібліотеки не можна просто використати в `commonMain`. Потрібно шукати multiplatform alternative або робити abstraction.

4. **iOS integration**

Shared Kotlin код експортується для iOS як framework. Це додає нюанси:

- Swift interop;
- generics виглядають не завжди ідеально;
- Flow треба bridge-ити;
- exceptions/cancellation треба обробляти акуратно;
- debugging може бути складнішим.

5. **UI sharing обмежений**

Compose Multiplatform існує, але в mobile production часто все ще ділять бізнес-логіку, а UI пишуть нативно: Android Compose і SwiftUI/UIKit окремо.

6. **Build complexity**

KMP додає Gradle/source set complexity:

```text
commonMain
androidMain
iosMain
commonTest
androidUnitTest
iosTest
```

CI, dependency management і build time можуть стати складнішими.

7. **Командна взаємодія**

KMP вимагає домовленостей між Android та iOS командами:

- хто owns shared module;
- як ревʼюяться зміни;
- як версіонуються shared APIs;
- як тестується iOS integration;
- як релізяться breaking changes.

8. **Не завжди вигідно**

KMP має сенс, якщо є суттєва спільна логіка. Якщо app простий або платформи мають дуже різну бізнес-логіку/UX, витрати на KMP можуть бути більші за користь.

9. **Що добре шарити**

- domain models;
- validation;
- use cases;
- API clients через Ktor;
- serialization;
- repository contracts;
- business rules;
- частину cache/data layer.

10. **Практичне правило**

Починати краще з shared domain/networking, а не з повного перенесення всього app. KMP має зменшувати дублювання бізнес-логіки, а не створювати складний shared моноліт.

**Коротко:** Kotlin Multiplatform добре підходить для shared бізнес-логіки, моделей, networking і validation, але має обмеження: platform APIs недоступні в common code, потрібні `expect/actual`, не всі бібліотеки multiplatform, iOS interop має нюанси, UI sharing не завжди практичний, а build і командні процеси стають складнішими.

</details>
<details>
<summary>80. Як ділитися бізнес-логікою у KMM?</summary>

#### Kotlin

У KMM/Kotlin Multiplatform бізнес-логіку виносять у shared module, а platform-specific частини залишають в Android/iOS source sets. Shared code містить use cases, domain models, repositories contracts, validation, networking abstractions і частину data layer.

1. **Базова структура**

```text
shared/
  commonMain/   -> спільна логіка
  androidMain/  -> Android-specific реалізації
  iosMain/      -> iOS-specific реалізації
```

`commonMain` не має напряму залежати від Android SDK або iOS frameworks.

2. **Що виносити у shared**

У shared зазвичай виносять:

- domain models;
- use cases;
- validation;
- repository interfaces;
- business rules;
- networking через Ktor;
- serialization;
- local storage abstractions;
- ViewModel/state logic, якщо команда готова.

3. **Що не виносити**

Platform-specific речі краще залишати окремо:

- Android UI / Compose Android;
- SwiftUI/UIKit;
- permissions;
- notifications;
- platform storage details;
- camera/location APIs;
- DI setup, якщо він сильно platform-specific.

4. **Repository contract у commonMain**

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

Use case спільний для Android та iOS. Реалізація repository може бути shared або platform-specific.

5. **expect/actual**

Для platform-specific API використовують `expect/actual`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

Android:

```kotlin
actual class PlatformLogger {
    actual fun log(message: String) {
        Log.d("App", message)
    }
}
```

iOS має свою actual implementation.

6. **Networking**

Ktor часто використовують як multiplatform HTTP client:

```kotlin
class UserApi(private val client: HttpClient) {
    suspend fun getUser(id: String): UserDto =
        client.get("/users/$id").body()
}
```

Для JSON — Kotlin Serialization.

7. **State і Flow**

У shared можна використовувати coroutines і `Flow`:

```kotlin
class ProfileStore(
    private val getUser: GetUserUseCase
) {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

Для iOS треба продумати bridge до Swift/Combine/async-await.

8. **DI**

DI можна робити:

- вручну через shared factory/container;
- Koin Multiplatform;
- platform-specific DI на Android/iOS;
- hybrid approach.

Головне — щоб shared code не тягнув Android-only залежності.

9. **Практичні ризики**

- винести в shared занадто багато platform-specific логіки;
- ускладнити iOS integration;
- погано продумати Flow bridging;
- зробити shared module “god module”;
- не домовитись між Android/iOS командами про ownership.

10. **Практичне правило**

У shared треба виносити стабільну бізнес-логіку, яка однакова для платформ. Якщо логіка залежить від UX або platform APIs, краще лишити її на платформі або сховати за `expect/actual`/interface.

**Коротко:** у KMM бізнес-логіку ділять через shared module: domain models, use cases, contracts, validation, networking і частину data layer. Platform-specific API реалізують через `expect/actual` або interfaces. Важливо не тягнути Android/iOS деталі в `commonMain` і не перетворювати shared module на моноліт.

</details>
<details>
<summary>81. Як оптимізувати продуктивність Kotlin-коду?</summary>

#### Kotlin

Оптимізація починається з вимірювання:

```text
measure -> bottleneck -> change -> measure again
```

Для Android використовують Profiler, Perfetto/System Trace, Macrobenchmark і allocation tracking. Спочатку визначають metric: startup, frame time, memory, battery або throughput.

### Алгоритм і data access

Найбільший виграш дають:

- заміна `O(n²)` на `O(n)`;
- `Set/Map` замість повторного linear search;
- менше network/DB calls;
- DB indexes;
- pagination та cache policy.

Мікрооптимізація syntax не компенсує неправильний algorithm або chatty API.

### Allocations і collections

```kotlin
items.map(Item::toUi)
    .forEach(::draw)
```

Створюється проміжний list. У підтвердженому hot path:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

Для звичайного UI readability важливіша. `Sequence` корисна для lazy chain/early termination, але має overhead і не завжди швидша на малих lists.

Частий `copy()` великого UI state створює allocations. Якщо це bottleneck, state декомпозують за частотою змін, а не переходять до global mutable state.

### Strings, primitives, inline

Для concatenation у loop:

```kotlin
val text = buildString {
    items.forEach {
        appendLine(it.name)
    }
}
```

Для tight numeric loops `IntArray` уникає boxing `List<Int>`, але в типовому UI це рідко критично.

`inline` може прибрати lambda/call overhead у small higher-order functions, але збільшує bytecode. Не inline-ити все без benchmark.

### Coroutines

- Main — коротка UI logic;
- blocking I/O — IO;
- CPU-heavy — Default;
- не викликати `runBlocking` на Main;
- обмежувати parallel tasks;
- скасовувати obsolete work;
- не робити зайві context switches.

Bottleneck зазвичай у blocking, contention, I/O або надмірній concurrency, а не у самій coroutine abstraction.

### Compose і Views

Compose:

- без I/O та важких calculations у composition;
- stable keys у lazy lists;
- state читати ближче до використання;
- не створювати expensive objects на кожну recomposition;
- перевіряти composition/layout/draw profiler-ом.

Не кожна recomposition є проблемою — важлива її вартість.

Views/RecyclerView:

- не робити важкий bind/onDraw;
- reset-ити recycled state;
- image loading делегувати cache-aware loader-у;
- уникати зайвих `requestLayout()`.

### Memory і startup

- не тримати Activity/View у singleton;
- cache має size/eviction policy;
- large bitmap/resources звільняти за lifecycle;
- app startup initialization робити мінімальною/lazy;
- reflection/code generation оцінювати вимірами.

Leaks збільшують GC pressure і ризик OOM, тому memory correctness — частина performance.

### Практичний порядок

1. Відтворити scenario.
2. Записати trace/profile.
3. Знайти hot path.
4. Змінити одну причину.
5. Повторити benchmark.
6. Додати regression metric/test.

**Коротко:** основний виграш дають правильний algorithm, менше I/O та allocations у hot path, контрольована concurrency й дешевий UI pipeline. `Sequence`, `inline`, caching та primitive arrays застосовують лише після вимірювання.

</details>
<details>
<summary>82. Як реалізувати retry з exponential backoff у Kotlin?</summary>

#### Kotlin

Exponential backoff повторює transient failure зі зростаючою паузою. Retry має бути bounded, cancellable, із jitter та чітким predicate.

1. **Базова ідея**

```text
attempt 1 -> delay 1s
attempt 2 -> delay 2s
attempt 3 -> delay 4s
attempt 4 -> delay 8s
```

`maxAttempts` і `maxDelay` обмежують latency, jitter не дає багатьом clients повторювати запит одночасно.

2. **Простий suspend retry**

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
        } catch (error: Exception) {
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

Це full jitter у межах current delay. У production також перевіряють коректність delay/factor parameters.

3. **Використання**

```kotlin
val user = retryWithBackoff(shouldRetry = ::shouldRetry) {
    api.getUser(userId)
}
```

4. **Jitter**

Full/equal/decorrelated jitter розсіюють clients після спільного outage. Фіксовані `+ Random(0..500)` слабко допомагають при великих delays.

5. **Retry predicate**

Краще явно вирішувати, які помилки retry-ити:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in listOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Для `429`/`503` варто поважати server `Retry-After`. `401` іноді обробляється окремим token refresh, а `403`/`404` зазвичай не retry-яться.

6. **Flow retryWhen**

Для Flow є `retryWhen`:

```kotlin
flow {
    emit(api.loadData())
}.retryWhen { cause, attempt ->
    if (attempt < 3 && shouldRetry(cause)) {
        delay(1_000L * (1 shl attempt.toInt()))
        true
    } else {
        false
    }
}
```

`attempt` починається з нуля; delay треба cap-ити, щоб уникнути overflow та надмірного очікування.

7. **Timeout**

Retry не замінює timeout operation/client-а:

```kotlin
withTimeout(15_000) {
    retryWithBackoff {
        api.loadData()
    }
}
```

8. **Cancellation**

`delay()` cancellable. Generic catch/predicate не повинен перетворювати `CancellationException` на retry.

9. **Коли retry не потрібен**

Не retry-ити:

- validation/business errors;
- auth failure без окремого refresh flow;
- 403/404;
- non-idempotent operation без idempotency key;
- input, який має виправити користувач.

**Коротко:** exponential backoff у Kotlin реалізують suspend-loop або `Flow.retryWhen`. Обов’язкові max attempts/delay, jitter, timeout, retry predicate, cancellation propagation та idempotency.

</details>
<details>
<summary>83. Які нові можливості Kotlin варто знати у 2026 році?</summary>

#### Kotlin

Станом на червень 2026 актуальна language release — Kotlin 2.4.0. Для Android важливі language features і сумісність KGP, Gradle, AGP та compiler plugins.

### Kotlin 2.4 і K2

K2 — стандартна compiler architecture Kotlin 2.x: новий frontend для analysis і спільна база JVM, Native, JS та Wasm.

Перед upgrade перевіряють Android Studio, AGP/Gradle/JDK, Compose plugin, KSP/kapt і serialization. Kotlin 2.4.0 підтримує AGP від 8.5.2, тому version update не роблять ізольовано.

### Stable features

Context parameters та explicit backing fields стали stable.

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

Context parameters корисні для DSL, transaction і scoped APIs, але не замінюють DI.

Explicit backing field прибирає pair `_state/state`:

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)

fun update(value: UiState) {
    state.value = value
}
```

Public API бачить read-only `StateFlow`, private scope — concrete mutable field.

Також у лінійці 2.3 stable стали data-flow exhaustiveness для `when` і nested type aliases.

### Experimental features

Kotlin 2.4 додав experimental:

- explicit context arguments;
- collection literals `[]`;
- ширші compile-time constants;
- unused-result checker improvements;
- `@IntroducedAt` для versioned overloads.

```kotlin
val names: List<String> =
    ["Alex", "Kate"]
```

Experimental feature потребує opt-in/compiler flag і може змінитися. У production потрібні конкретна користь, IDE support і migration plan.

### Standard library

`kotlin.uuid.Uuid` став stable, крім частини generation API. Додано `isSorted*()` helpers та точніші fallback operations для nullable Map values.

Перед заміною власного persisted/API format треба перевірити serialization compatibility.

### Multiplatform

У 2.4 Swift export має Alpha status, покращено export coroutines/Flow, додано Swift package import. Kotlin/Wasm incremental compilation стала stable, розвивається Component Model.

Для KMP треба оцінювати Swift API, concurrency/error mapping, binary size та ownership між Android/iOS teams, а не лише можливість компіляції shared code.

### JVM і build

Kotlin 2.4 підтримує Java 26 bytecode та annotations у Kotlin metadata. Android `jvmTarget` визначають AGP, toolchain і runtime/desugaring, а не максимальна можливість compiler-а.

Compose compiler є Kotlin compiler plugin; version alignment і видалення deprecated flags важливіші за adoption кожної нової syntax feature.

### Перед adoption

- stable чи experimental;
- IDE/build/CI support;
- plugin/library compatibility;
- binary/API impact;
- readability benefit;
- migration і rollback plan.

**Коротко:** у 2026 треба знати Kotlin 2.4/K2, stable context parameters та explicit backing fields, UUID API, tooling constraints і KMP/Swift/Wasm розвиток. Experimental features впроваджують лише після перевірки ecosystem compatibility.

</details>
<details>
<summary>84. Що таке життєвий цикл Activity?</summary>

#### Kotlin

Lifecycle `Activity` — послідовність станів і callbacks, через які Android повідомляє про створення, visibility, foreground/background та знищення екрана.

1. **Основні callbacks**

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Після повернення зі `STOPPED` перед `onStart()` викликається `onRestart()`.

2. **onCreate**

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

Тут створюють UI, налаштовують navigation і базові dependencies. Важка synchronous робота затримує startup.

3. **onStart**

Activity стає видимою, але ще не обов’язково має interaction focus. Тут запускають роботу, потрібну видимому UI.

4. **onResume**

Activity у foreground і готова до взаємодії:

```kotlin
override fun onResume() {
    super.onResume()
    // resume camera, sensors, animations
}
```

Тут відновлюють foreground-only resources: camera, sensors або animations.

5. **onPause**

Activity втрачає focus. Callback має бути швидким:

```kotlin
override fun onPause() {
    super.onPause()
    // pause animations, camera preview, sensors
}
```

Довга synchronous операція затримає перехід до іншого екрана.

6. **onStop**

Activity більше не видима. Тут зупиняють visible-only observers та дорогі UI resources.

7. **onDestroy**

Activity знищується через `finish()` або recreation. `onDestroy()` не гарантується при завершенні process, тому важливі дані тут не зберігають.

8. **Configuration change**

При rotation/language/theme change Activity може бути пересоздана:

```text
old Activity -> onPause -> onStop -> onDestroy
new Activity -> onCreate -> onStart -> onResume
```

`ViewModel` переживає configuration change. UI state для system recreation зберігають через Saved State, а довготривалі дані — у persistent storage.

9. **SavedInstanceState**

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

`Bundle` підходить для невеликого UI state, але не для великих objects або business data.

10. **Lifecycle-aware APIs**

Flow collection прив’язують до active state:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state -> render(state) }
    }
}
```

`repeatOnLifecycle` запускає block у `STARTED`, скасовує нижче цього state і запускає знову після повернення.

**Коротко:** `onCreate` створює UI, `onStart` означає visibility, `onResume` — interaction, `onPause`/`onStop` зупиняють відповідну роботу, `onDestroy` завершує instance. State зберігають через ViewModel, Saved State або storage.

</details>
<details>
<summary>85. Що таке Fragment і чим він відрізняється від Activity?</summary>

#### Kotlin

`Activity` — system component і UI entry point із власним window/task. `Fragment` — UI controller усередині Activity або parent Fragment, яким керує `FragmentManager`.

1. **Activity**

Activity реєструється в manifest:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Вона отримує intents, керує window та host-ить Fragment або Compose tree.

2. **Fragment**

Fragment створює/контролює частину View-based UI:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }
}
```

Він не є окремим manifest/system component і залежить від host та `FragmentManager`.

3. **Головна різниця**

```text
Activity -> самостійний Android component, має window/task/intent entry
Fragment -> частина UI всередині Activity, керується FragmentManager
```

4. **Lifecycle**

Activity lifecycle:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Fragment окремо має View lifecycle:

```text
onCreateView -> onViewCreated -> onDestroyView
```

Fragment object може залишатися в back stack після знищення його View.

5. **onDestroyView**

References на View очищають у `onDestroyView()`:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

UI collection також прив’язують до `viewLifecycleOwner`, а не до lifecycle Fragment object.

6. **Navigation**

Між Activities переходять через intents:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Fragment navigation виконує `FragmentManager` або Navigation Component:

```kotlin
findNavController().navigate(R.id.detailsFragment)
```

7. **Composition**

Fragments можна по-різному комбінувати в layouts:

```text
phone  -> Activity + один Fragment
tablet -> Activity + list Fragment + details Fragment
```

Це корисно для master-detail UI, хоча reuse окремих Views/composables часто простіший за reuse цілого Fragment.

8. **ViewModel scope**

State можна scope-ити до Fragment або Activity:

```kotlin
private val ownViewModel: ProfileViewModel by viewModels()
private val sharedViewModel: SharedViewModel by activityViewModels()
```

Scope вибирають за реальним owner-ом state, а не лише для зручного sharing.

9. **Compose контекст**

У повністю Compose app Activity може host-ити один Compose tree без Fragments. У hybrid/View app Fragment залишається screen/navigation abstraction.

**Коротко:** Activity — manifest component із window/task та intents. Fragment — UI controller усередині host, керований FragmentManager. Його View має коротший lifecycle, тому bindings і UI subscriptions очищають у `onDestroyView()`.

</details>
<details>
<summary>86. Що таке Intent і як він використовується?</summary>

#### Kotlin

`Intent` — це обʼєкт повідомлення в Android, який описує дію, яку треба виконати: відкрити Activity, запустити Service, надіслати Broadcast або передати дані між компонентами.

1. **Explicit Intent**

Explicit intent напряму вказує компонент:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
intent.putExtra("user_id", userId)
startActivity(intent)
```

Використовується для переходів між Activity всередині app.

2. **Implicit Intent**

Implicit intent описує дію, а Android шукає app, який може її виконати:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Приклади: відкрити browser, share text, зробити дзвінок, відкрити map.

3. **Extras**

Дані передають через extras:

```kotlin
intent.putExtra("user_id", userId)
```

Отримання:

```kotlin
val userId = intent.getStringExtra("user_id")
```

Краще передавати id або невеликі primitive дані, а не великі обʼєкти.

4. **Parcelable**

Для structured data можна використовувати `Parcelable`:

```kotlin
@Parcelize
data class UserArgs(val id: String, val name: String) : Parcelable
```

Але для navigation часто краще передати тільки `id` і завантажити дані з repository.

5. **Intent action**

Action описує, що треба зробити:

```kotlin
Intent.ACTION_SEND
Intent.ACTION_VIEW
Intent.ACTION_DIAL
```

Приклад share:

```kotlin
val intent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"
    putExtra(Intent.EXTRA_TEXT, "Hello")
}
startActivity(Intent.createChooser(intent, "Share"))
```

6. **Intent filter**

Компонент може оголосити, які intents він приймає:

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

Це використовується для deep links, share targets, system actions.

7. **startActivityForResult replacement**

Старий `startActivityForResult` замінений Activity Result API:

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // handle uri
}

launcher.launch("image/*")
```

Це lifecycle-aware і безпечніше.

8. **Broadcast Intent**

Intent може використовуватись для broadcast:

```kotlin
sendBroadcast(Intent("com.example.ACTION_SYNC"))
```

Але для internal app communication краще часто використовувати Flow/shared state, а не broadcasts.

9. **Security**

З implicit intents і exported components треба бути уважним:

- перевіряти, чи є activity для intent;
- не передавати secrets у extras;
- використовувати explicit intents для internal components;
- правильно налаштовувати `android:exported`;
- валідувати data з external intents.

10. **Практичне правило**

Intent — це механізм комунікації між Android components. Для internal navigation у modern apps часто краще Navigation Component, але intents досі потрібні для Activity запуску, deep links, share, system actions і inter-app communication.

**Коротко:** Intent описує дію або конкретний Android component. Explicit intent відкриває конкретну Activity/Service, implicit intent просить систему знайти app для action. Через extras передають невеликі дані, intent filters оголошують підтримувані дії, а для результатів краще використовувати Activity Result API.

</details>
<details>
<summary>87. Що станеться, якщо не викликати super.onCreate()?</summary>

#### Kotlin

Якщо в `Activity` не викликати `super.onCreate(savedInstanceState)`, базовий Android/Jetpack-клас не виконає свою lifecycle-ініціалізацію. Найчастіше це закінчується crash-ем або некоректною роботою Activity.

1. **Правильний код**

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Для `onCreate` стандартне правило — викликати `super.onCreate()` на початку, до `setContentView`, `setContent`, Fragment setup та іншої framework-dependent логіки.

2. **Що робить `super.onCreate()`**

Базовий клас готує внутрішній стан Activity:

- lifecycle state;
- saved state restoration;
- window/decor infrastructure;
- `FragmentManager` у `FragmentActivity`;
- `SavedStateRegistry` і `ActivityResultRegistry` у Jetpack-класах;
- AppCompat/ComponentActivity-specific інтеграції.

Тобто це не boilerplate, а частина контракту з framework.

3. **Що буде без `super`**

Погано:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        setContentView(R.layout.activity_main)
    }
}
```

Можливі наслідки:

- `SuperNotCalledException` або інший crash;
- некоректний lifecycle state;
- проблеми з fragments;
- не відновиться saved state;
- можуть зламатися Activity Result API, Compose, Hilt або AppCompat;
- lifecycle observers отримають неправильні events.

4. **Fragment-и теж мають `super`**

У Fragment lifecycle callback-ах також зазвичай треба викликати `super`:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
}

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
}
```

Командне правило має бути простим: якщо override-иш lifecycle callback — викликай `super`, якщо документація явно не каже інакше.

5. **Compose і Hilt**

Для Compose:

```kotlin
class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent { App() }
    }
}
```

`ComponentActivity` ініціалізує lifecycle, saved state і activity result інфраструктуру. Для Hilt та інших generated base classes пропуск `super` також може зламати lifecycle/injection setup.

6. **Практичне правило**

- У `Activity.onCreate()` викликати `super.onCreate(savedInstanceState)` на початку.
- Не викликати UI/framework APIs до `super`, якщо немає документованої причини.
- У Fragment/Activity lifecycle callback-ах не пропускати `super`.
- Якщо змінюєш порядок `super`, треба розуміти конкретну причину.

**Коротко:** без `super.onCreate()` Android base class не виконає обовʼязкову lifecycle/framework ініціалізацію. Це може зламати lifecycle, saved state, fragments, Compose, Hilt або одразу привести до crash-а.

</details>
<details>
<summary>88. Що таке View Binding і чому він кращий за findViewById?</summary>

#### Kotlin

`View Binding` — це Android-механізм, який генерує binding-клас для XML layout і дає type-safe доступ до View без ручного `findViewById`. Він зменшує boilerplate і ризик runtime-помилок через неправильний id або тип View.

1. **Проблема findViewById**

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Проблеми:

- багато boilerplate;
- можна вказати неправильний тип;
- помилка проявиться runtime;
- код гірше читається на великих екранах.

2. **View Binding в Activity**

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

`ActivityMainBinding` генерується з `activity_main.xml`.

3. **View Binding у Fragment**

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

У Fragment binding треба очищати в `onDestroyView()`, бо View lifecycle коротший за Fragment lifecycle.

4. **Чому краще за findViewById**

View Binding дає:

- type safety;
- null safety для існуючих View;
- менше boilerplate;
- compile-time перевірку id;
- зручніший refactoring;
- прямий доступ через `binding.someView`.

5. **Nullability**

Якщо View є не в усіх layout configurations, binding поле може бути nullable. Це змушує явно обробити випадок, коли View відсутня.

6. **View Binding vs Data Binding**

```text
View Binding -> type-safe доступ до View
Data Binding -> expressions і binding даних у XML
```

View Binding простіший і легший. Data Binding потужніший, але складніший і може переносити логіку в XML.

7. **View Binding vs Compose**

У Compose View Binding зазвичай не потрібен, бо UI пишеться як composable functions:

```kotlin
setContent {
    ProfileScreen()
}
```

Але View Binding актуальний для XML/View System і hybrid screens.

8. **Практичне правило**

Для XML layout краще використовувати View Binding замість `findViewById`. У Fragment обовʼязково очищати binding у `onDestroyView()`. Для нового повністю Compose UI — використовувати Compose state і composables, а не binding.

**Коротко:** View Binding генерує type-safe binding-класи для XML layout і замінює `findViewById`. Він безпечніший, коротший і краще підтримує refactoring. Головний нюанс — у Fragment binding треба очищати в `onDestroyView()`.

</details>
<details>
<summary>89. Що таке LiveData?</summary>

#### Kotlin

`LiveData` — це lifecycle-aware observable data holder з Android Jetpack. Він зберігає значення і повідомляє active observers, коли значення змінюється. Active означає, що lifecycle owner у стані `STARTED` або `RESUMED`.

1. **Базова ідея**

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

`MutableLiveData` змінюється всередині ViewModel, а назовні віддається read-only `LiveData`.

2. **Observation**

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

Observer автоматично отримує updates тільки коли lifecycle active. Після destroy owner observer очищується.

3. **value vs postValue**

```kotlin
_liveData.value = user      // main thread
_liveData.postValue(user)   // background thread
```

`value` треба викликати з main thread. `postValue` можна з background thread, але воно асинхронне.

4. **Переваги LiveData**

- lifecycle-aware;
- простий API;
- добре інтегрується з XML/View System;
- автоматично не шле updates stopped/destroyed observers;
- історично стандартний Android-підхід.

5. **Обмеження LiveData**

- слабший API для stream transformations, ніж Flow;
- привʼязаний до Android lifecycle;
- менш зручний для Kotlin Multiplatform;
- не такий гнучкий для async pipelines;
- one-shot events часто реалізовувалися костилями типу SingleLiveEvent.

6. **LiveData vs StateFlow**

```text
LiveData  -> Android lifecycle-aware observable holder
StateFlow -> Kotlin Flow API, hot stream зі state value
```

У сучасному Kotlin-коді для ViewModel state частіше використовують `StateFlow`, особливо з Compose.

7. **Compose integration**

LiveData можна читати в Compose:

```kotlin
val user by viewModel.user.observeAsState()
```

Але для Compose + Coroutines зазвичай краще `StateFlow` + `collectAsStateWithLifecycle()`.

8. **Transformations**

LiveData має базові transformations:

```kotlin
val userName = user.map { it.name }
```

Або `switchMap`, якщо залежно від одного LiveData треба підписатись на інше.

9. **Коли використовувати LiveData**

LiveData доречний у legacy Android/View System проектах або там, де команда вже використовує LiveData. Для нового coroutine-first коду частіше варто обрати Flow/StateFlow.

10. **Практичне правило**

- ViewModel expose-ить `LiveData`, не `MutableLiveData`.
- `observe(viewLifecycleOwner)` у Fragment.
- `value` — main thread, `postValue` — background.
- Для one-shot events краще SharedFlow/Channel, а не SingleLiveEvent.
- Для нового Compose-коду частіше StateFlow.

**Коротко:** LiveData — lifecycle-aware observable holder для Android UI. Він добре працює з ViewModel і View System, автоматично враховує lifecycle observers, але в сучасному Kotlin/Compose коді часто замінюється на `StateFlow`/`Flow`, які мають сильніший coroutine API.

</details>
<details>
<summary>90. Що таке Room і як він працює?</summary>

#### Kotlin

`Room` — це Jetpack-бібліотека для роботи з SQLite через типобезпечний API. Вона дає abstraction над SQL, але не ховає саму реляційну модель: ти все одно описуєш таблиці, DAO, queries, migrations і relations.

1. **Основні частини Room**

Room складається з трьох головних елементів:

```text
@Entity  -> таблиця
@Dao     -> методи доступу до даних
@Database -> база даних і entry point
```

2. **Entity**

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val age: Int
)
```

`Entity` описує таблицю SQLite. Поля класу стають колонками.

3. **DAO**

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
}
```

DAO описує SQL-запити. Room перевіряє частину SQL на етапі компіляції.

4. **Database**

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

5. **Flow support**

Room може повертати `Flow`:

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Коли таблиця змінюється, Room emit-ить новий список. Це добре працює з ViewModel і Compose.

6. **Suspend functions**

DAO-методи можуть бути `suspend`:

```kotlin
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUser(id: String): UserEntity?
```

Room виконує такі операції поза main thread. Але все одно треба думати про transaction boundaries і dispatcher strategy в repository.

7. **Relations**

Room підтримує relations через data classes:

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(
        parentColumn = "id",
        entityColumn = "userId"
    )
    val posts: List<PostEntity>
)
```

Для складних relation-запитів часто потрібні `@Transaction`.

8. **Migrations**

Коли schema змінюється, треба підняти version і додати migration:

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

Не варто використовувати destructive migration для user data.

9. **Repository layer**

Room entity краще не віддавати напряму в UI:

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

Repository координує DAO, API, cache і mapping.

10. **Практичне правило**

Room підходить для local relational storage, offline-first cache, structured data і reactive streams через Flow. Але Room — не key-value storage і не заміна backend API.

**Коротко:** Room — типобезпечна обгортка над SQLite. Він працює через `@Entity`, `@Dao`, `@Database`, підтримує suspend functions, Flow, relations і migrations. У production Room зазвичай ховають за repository і маплять entities у domain/UI models.

</details>
<details>
<summary>91. Що роблять анотації @Query, @Insert, @Delete?</summary>

#### Kotlin

Це Room-анотації DAO-методів. Room генерує SQLite-код, перевіряє SQL і mapping result type під час компіляції.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users ORDER BY name")
    fun observeUsers(): Flow<List<UserEntity>>
}
```

### @Query

`@Query` виконує явно заданий SQL:

```kotlin
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUser(id: String): UserEntity?
```

`:id` — bind parameter. Не треба конкатенувати user input у SQL string: Room безпечно передає supported values як parameters.

Query може повертати entity, projection DTO, scalar, `List`, `Flow`, `PagingSource` тощо. Для observable return type Room відстежує таблиці, використані в query, і повторно виконує запит після їх invalidation:

```kotlin
@Query("SELECT id, name FROM users")
fun observeUsers(): Flow<List<UserSummary>>
```

`@Query` застосовується не лише для SELECT:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

`Int` повертає кількість affected rows. Query зручний для partial update або видалення за умовою.

### @Insert

`@Insert` генерує INSERT на основі `@Entity`:

```kotlin
@Insert(onConflict = OnConflictStrategy.ABORT)
suspend fun insert(user: UserEntity): Long

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`Long` — row ID вставленого запису. Conflict strategy визначає поведінку при порушенні unique/primary-key constraint:

- `ABORT` — перервати statement/transaction з exception;
- `IGNORE` — пропустити conflicting row;
- `REPLACE` — SQLite replace semantics.

`REPLACE` може фактично видалити старий row і вставити новий, що важливо для foreign keys, triggers і полів, яких немає в новому entity. Для явного insert-or-update сучасний Room також має `@Upsert`.

### @Delete

`@Delete` генерує видалення entity за його primary key:

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Передавати всі актуальні поля не потрібно — identity визначає primary key. Для видалення лише за ID або складною умовою використовують `@Query("DELETE ...")`.

### @Update і @Transaction

`@Update` оновлює entity за primary key. Для зміни окремих columns `@Query` часто точніший.

Кілька DAO-операцій, які мають бути атомарними, об'єднують `@Transaction`:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

При exception transaction rollback-иться. Для read relation methods `@Transaction` також забезпечує узгоджений snapshot кількох queries.

### Compile-time перевірка

Room перевіряє:

- існування tables і columns;
- SQL syntax;
- відповідність bind parameters;
- mapping result columns у return type;
- допустимість DAO signature.

Це не гарантує правильність бізнес-умови або performance query, тому indexes і query plan все одно треба аналізувати.

**Коротко:** `@Query` виконує та compile-time перевіряє SQL, `@Insert` вставляє entity з conflict strategy, `@Delete` видаляє її за primary key. Room генерує DAO implementation, а `@Transaction` об'єднує операції атомарно.

</details>
<details>
<summary>92. Що таке WorkManager і коли його використовувати?</summary>

#### Kotlin

`WorkManager` планує persistent deferrable work, яка має продовжити виконання після виходу з app, process death або reboot. Він підтримує constraints, retries та chaining, але не гарантує точний час запуску.

1. **Коли використовувати**

Типові задачі:

- background sync та retry;
- deferrable upload/download;
- cleanup, logs або analytics;
- periodic maintenance;
- work із вимогами до network, charging чи battery.

2. **One-time work**

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

Request стане eligible, коли constraint мережі виконаний; фактичний запуск визначає system scheduler.

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

`Result.success()` завершує work, `failure()` позначає terminal failure, `retry()` планує повтор за backoff policy.

4. **Constraints**

Можна вимагати певну network type, charging, достатній battery/storage level. Якщо constraint перестає виконуватися, WorkManager може зупинити work та запланувати її знову.

5. **Periodic work**

Periodic work має мінімальний interval і виконується неточно через system optimizations та constraints.

6. **Unique work**

`enqueueUniqueWork()` запобігає дублюванню. `ExistingWorkPolicy` визначає, залишити, замінити або доповнити existing chain.

7. **Input data**

Input/output `Data` призначений для невеликих values. Великі payloads зберігають у DB/file і передають identifier.

8. **Коли WorkManager не підходить**

WorkManager не підходить для:

- точних alarm-ів;
- realtime socket connection;
- media playback;
- постійного location tracking;
- interactive роботи, яку користувач очікує прямо зараз.

9. **WorkManager vs Service**

Для sync після виходу з app потрібен WorkManager; для playback або active tracking — foreground service. Важливу коротку work можна позначити expedited, але це залежить від quota.

10. **Практичне правило**

WorkManager обирають, якщо work може стартувати пізніше, але повинна бути persistent. Exact alarm, realtime channel і ongoing user-visible operation потребують інших API.

**Коротко:** WorkManager — для persistent deferrable sync/upload/cleanup із constraints і retry. Він не гарантує точний час і не замінює exact alarms, realtime connections або foreground services.

</details>
<details>
<summary>93. У чому різниця між CoroutineScope, lifecycleScope та viewModelScope?</summary>

#### Kotlin

`CoroutineScope` задає context і lifetime корутин. `lifecycleScope` та `viewModelScope` — готові Android scopes із конкретними lifecycle owners.

1. **CoroutineScope**

`CoroutineScope` — загальний owner корутин:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Custom scope потребує власного `Job` і явного `cancel()` від owner-а.

2. **lifecycleScope**

`lifecycleScope` належить `LifecycleOwner`: Activity, Fragment або Fragment View.

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

Він скасовується при `DESTROYED`. Сам `launch` не зупиняє роботу в `STOPPED`, тому для Flow collection використовують `repeatOnLifecycle`.

3. **viewLifecycleOwner.lifecycleScope**

У Fragment роботу з binding/View запускають у scope View lifecycle:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state -> render(state) }
    }
}
```

Fragment живе довше за свою View, тому scope самого Fragment може пережити `onDestroyView()`.

4. **viewModelScope**

`viewModelScope` належить `ViewModel`:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Він переживає configuration change і скасовується в `onCleared()`.

5. **Вибір scope**

```text
CoroutineScope  -> загальний scope, lifecycle визначаєш сам
lifecycleScope  -> живе разом з Activity/Fragment lifecycle
viewModelScope  -> живе разом з ViewModel
```

`viewModelScope` використовують для screen operations і формування UI state. `lifecycleScope` — для UI collection/effects, що належать конкретному owner-у. Custom scope створюють для іншого чіткого lifetime, наприклад application-level work.

6. **repeatOnLifecycle**

Для Flow collection у UI краще:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) {
    flow.collect { value -> render(value) }
}
```

Block запускається в `STARTED`, скасовується нижче цього state і запускається знову при поверненні.

7. **Типові помилки**

- custom scope без cancellation;
- `GlobalScope` для звичайної app-роботи;
- collection для View у scope Fragment;
- screen operation в Activity замість ViewModel;
- reference на binding після `onDestroyView()`.

**Коротко:** custom `CoroutineScope` має власного owner-а; `lifecycleScope` належить UI lifecycle, `viewModelScope` — ViewModel. У Fragment View collection запускають через `viewLifecycleOwner` і `repeatOnLifecycle`.

</details>
<details>
<summary>94. Як реалізується dependency injection у Kotlin?</summary>

#### Kotlin

Dependency Injection у Kotlin реалізується через передачу залежностей ззовні, найчастіше через constructor injection. Для великих проєктів використовують DI frameworks: Hilt/Dagger, Koin або manual DI.

1. **Constructor injection**

Найкращий базовий варіант:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Клас не створює `UserRepository` сам. Він отримує його ззовні, тому його легко тестувати.

2. **Поганий варіант без DI**

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

Тут клас жорстко привʼязаний до implementation. У тестах важко підмінити API, DB або repository.

3. **Interface + implementation**

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository {
    override suspend fun getUser(id: UserId): User = api.getUser(id.value).toDomain()
}
```

Use case залежить від abstraction, а не concrete class.

4. **Manual DI**

Для малого проєкту достатньо manual container:

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Manual DI простий, але при великому dependency graph швидко стає незручним.

5. **Hilt/Dagger**

Hilt використовує compile-time DI:

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

Hilt генерує graph і перевіряє залежності під час компіляції.

6. **Koin**

Koin використовує Kotlin DSL:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Він простіший у старті, але частина помилок може проявитись runtime.

7. **Scopes**

DI також керує lifetime обʼєктів:

```text
Singleton -> один instance на app
Factory   -> новий instance при кожному запиті
ViewModel -> instance в межах ViewModel lifecycle
```

Scope треба вибирати обережно, щоб не створити memory leaks або непотрібні singletons.

8. **Тестування**

DI спрощує заміну dependencies:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

У тесті use case отримує fake repository замість real API implementation.

9. **Практичне правило**

- Constructor injection — default.
- Interface потрібен там, де є реальна заміна/контракт.
- Manual DI підходить для малих проєктів.
- Hilt/Dagger — для великих Android apps.
- Koin — для простоти і швидкого старту.
- DI framework не замінює clean architecture.

**Коротко:** dependency injection у Kotlin реалізують через передачу залежностей ззовні, переважно constructor injection. Для Android часто використовують Hilt/Dagger або Koin, для малих проєктів — manual DI. DI зменшує coupling, спрощує тестування і допомагає керувати lifecycle залежностей.

</details>
<details>
<summary>95. Що таке Hilt і як він працює?</summary>

#### Kotlin

`Hilt` — Android DI framework поверх Dagger. Він генерує component hierarchy, factories та wiring для Application, Activity, Fragment, ViewModel, View і Service.

1. **Навіщо потрібен Hilt**

Hilt знаходить constructor/module bindings у generated graph і створює object без ручного DI container/factory.

2. **Application setup**

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` генерує root `SingletonComponent` і application-level setup.

3. **AndroidEntryPoint**

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

`@AndroidEntryPoint` створює generated component для Android class і дозволяє field/ViewModel injection. Parent components також мають бути Hilt-enabled.

4. **Inject constructor**

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

Constructor injection — default, якщо всі parameters мають bindings.

5. **ViewModel**

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

`@HiltViewModel` інтегрує generated factory з `ViewModelProvider`.

6. **Modules**

Modules потрібні для interface binding або third-party class без injectable constructor.

`@Binds` для interface:

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

`@Provides` method викликає external builder/factory, наприклад для OkHttp, Retrofit або Room.
```

7. **Scopes і components**

Hilt має lifecycle-aware scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Scope кешує binding у відповідному component:

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Child component бачить parent bindings. Довгоживучий parent не може залежати від короткоживучого child. Unscoped binding створюється для кожного request.

8. **Тестування**

`@TestInstallIn` замінює production module у Hilt test component. Для unit test без Android graph dependencies простіше передати fake напряму через constructor.

9. **Практичне правило**

- constructor injection — default;
- `@Binds` — interface → implementation;
- `@Provides` — external factory/builder;
- scope — лише для потрібного shared lifetime;
- Context qualifier і scope мають бути сумісними;
- DI graph не містить business logic.

**Коротко:** Hilt генерує Dagger graph та Android components. Constructor injection створює класи, modules описують external/interface bindings, scopes задають reuse, а test modules замінюють production wiring.

</details>
<details>
<summary>96. Що таке @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` дозволяє Hilt створювати `ViewModel` через generated `ViewModelProvider.Factory` і передавати залежності constructor injection-ом. Ручна factory для constructor parameters не потрібна.

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

Анотація `@HiltViewModel` ставиться на клас, а `@Inject` — на єдиний constructor. Усі його parameters мають бути доступні в Hilt graph. `SavedStateHandle` Hilt надає автоматично; він використовується для navigation arguments і невеликого стану, що відновлюється після process recreation.

### Отримання ViewModel

Host Activity або Fragment має бути Hilt entry point:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

`by viewModels()` використовує owner поточного Fragment. Для спільної ViewModel на рівні Activity застосовують `by activityViewModels()`.

У Compose з Navigation:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

`hiltViewModel()` бере ViewModel із відповідного `ViewModelStoreOwner`, часто navigation back stack entry. Тому scope залежить від місця виклику, а не лише від класу ViewModel.

### Component і scopes

Залежності ViewModel створюються в `ViewModelComponent`:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object ViewModelModule {

    @Provides
    fun provideValidator(): UserInputValidator =
        UserInputValidator()
}
```

`@ViewModelScoped` дає один instance залежності для конкретного instance ViewModel:

```kotlin
@ViewModelScoped
class UserSessionCache @Inject constructor()
```

`@Singleton` живе на рівні application graph. Саму ViewModel не роблять singleton: її lifecycle контролює `ViewModelStoreOwner`.

### Типові помилки

- забути `@HiltViewModel` або `@Inject constructor`;
- не додати `@AndroidEntryPoint` до host Activity/Fragment;
- створювати ViewModel вручну через constructor;
- очікувати спільний instance при різних `ViewModelStoreOwner`;
- інжектити Activity, Fragment або View у ViewModel;
- зберігати короткоживучий `Context` у ViewModel;
- використовувати неправильний scope для залежностей.

Залежності мають приходити через constructor, а Android UI objects не повинні переживати свій lifecycle. Якщо потрібен context, зазвичай інжектять application context у нижчий шар, а не Activity.

**Коротко:** `@HiltViewModel` інтегрує ViewModel з Hilt і `ViewModelProvider`: Hilt генерує factory, інжектить constructor dependencies та `SavedStateHandle`, а lifecycle і scope визначає `ViewModelStoreOwner`.

</details>
<details>
<summary>97. Як працює @Inject?</summary>

#### Kotlin

`@Inject` — JSR-330 анотація, за якою Dagger/Hilt знаходить спосіб створення об'єкта або точку введення залежності. Основний варіант — constructor injection:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt може створити `LoadUserUseCase`, якщо в graph існує binding для кожного constructor parameter. Dependency graph перевіряється під час компіляції:

```text
UserViewModel
 ├── LoadUserUseCase
 │    └── UserRepository
 └── Analytics
```

Якщо binding відсутній, ambiguous або утворює заборонений dependency cycle, build завершується помилкою.

### Constructor і field injection

Constructor injection кращий для власних класів, тому що залежності явні, можуть бути `val`, а клас легко створити в unit test.

Field injection потрібен переважно для Android-компонентів, які створює framework:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Injected field не можна безпечно використовувати до моменту injection у lifecycle компонента. Method injection також підтримується, але в Android-коді використовується рідко.

### Interface і third-party класи

`@Inject constructor` на implementation не визначає, яку реалізацію interface треба вибрати. Для цього використовують `@Binds`:

```kotlin
class RealUserRepository @Inject constructor(
    private val api: UserApi
) : UserRepository

@Module
@InstallIn(SingletonComponent::class)
abstract class RepositoryModule {

    @Binds
    abstract fun bindUserRepository(
        implementation: RealUserRepository
    ): UserRepository
}
```

Для класів, constructor яких не можна анотувати або які створюються builder-ом, застосовують `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

### Scopes і qualifiers

`@Inject` пояснює, як створити об'єкт, але не визначає його lifetime. `@Singleton`, `@ActivityScoped`, `@ViewModelScoped` та інші scopes задають повторне використання instance в межах відповідного component.

Без scope provider вважається unscoped: component може створювати новий instance для кожного запиту.

Якщо bindings мають однаковий тип, їх розрізняють qualifier-ами:

```kotlin
class AuthApi @Inject constructor(
    @AuthRetrofit private val retrofit: Retrofit
)
```

Qualifier має бути присутнім і на provider/binding, і в точці injection.

### Що генерує Hilt

Під час code generation Hilt:

- знаходить `@Inject constructor`, `@Binds` і `@Provides`;
- перевіряє graph та scopes;
- генерує factories і components;
- підключає Android entry points.

Reflection для створення dependency graph у runtime не використовується.

**Коротко:** `@Inject` позначає constructor або injection point для Dagger/Hilt. Для власних класів слід використовувати constructor injection, для interfaces — `@Binds`, для third-party/builder objects — `@Provides`; lifetime задають scopes, а однакові типи розрізняють qualifiers.

</details>
<details>
<summary>98. Що таке області видимості (scopes) у Hilt?</summary>

#### Kotlin

Hilt scope визначає, в межах якого generated component кешується binding. Component задає lifetime, а всі запити в одному його instance отримують той самий object.

Unscoped binding створюється заново для кожного injection request. Це нормальний default для дешевих stateless objects.

### Component hierarchy

Спрощена ієрархія:

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

### Основні scopes

- `@Singleton` — один instance на application graph;
- `@ActivityRetainedScoped` — один instance на logical Activity, переживає configuration changes;
- `@ViewModelScoped` — один instance на конкретну ViewModel;
- `@ActivityScoped` — один instance на конкретний Activity instance;
- `@FragmentScoped` — один instance на конкретний Fragment;
- `@ViewScoped` — один instance на Hilt-enabled View.
- `@ServiceScoped` — один instance на конкретний Service.

`@Singleton` доречний для app-level Room DB, OkHttp client або shared service, якщо справді потрібен один instance на process graph.

`@ViewModelScoped` використовують для stateful dependency, яка належить одній ViewModel:

```kotlin
@ViewModelScoped
class UserDraftCache @Inject constructor()
```

Різні ViewModels отримають різні caches. Для спільного object між ViewModels однієї logical Activity потрібен `@ActivityRetainedScoped`.

### Scope і InstallIn

Scope має відповідати component модуля:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {

    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache =
        UserDraftCache()
}
```

Scope annotation має відповідати component з `@InstallIn`; несумісність Hilt виявляє під час компіляції.

Scope ставлять на injectable class або provider/`@Binds` method. Object створюється при першому запиті та утримується до знищення component.

### Типові помилки

Небезпечна помилка — UI context у довгоживучому scope:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Singleton утримає Activity після destroy. Binding з `ActivityContext` має жити не довше Activity-related component.

Також не слід:

- робити singleton кожен binding;
- плутати Activity instance з retained logical Activity;
- ділити `@ViewModelScoped` object між різними ViewModels;
- scope-ити дешеві stateless helpers;
- зберігати UI objects в app-level service.

Scope — це ownership/lifecycle semantics, а не лише optimization.

**Коротко:** Hilt scope кешує binding у component: app, retained Activity, ViewModel, Activity, Fragment, View або Service. Scope має відповідати реальному owner-у й не утримувати короткоживучий UI object.

</details>
<details>
<summary>99. Як уникати витоків памʼяті в Android?</summary>

#### Kotlin

Витік памʼяті в Android виникає, коли обʼєкт уже не потрібен, але на нього досі є strong reference. Найчастіше це Activity, Fragment, View або Context, які утримуються довше свого lifecycle.

1. **Не тримати Activity Context у singleton**

Погано:

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

Якщо туди потрапить Activity, вона не звільниться після rotation/finish. Для довгоживучих залежностей використовуй `applicationContext`.

2. **Очищати ViewBinding у Fragment**

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Fragment object може жити довше за свою View, тому binding треба очищати в `onDestroyView()`.

3. **Очищати adapter/listeners**

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Adapter, listeners або callbacks можуть тримати View/Fragment reference.

4. **Lifecycle-aware coroutines**

Для Fragment UI collection:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

Не треба запускати UI coroutines у scope, який живе довше View.

5. **Не використовувати GlobalScope**

```kotlin
GlobalScope.launch { }
```

`GlobalScope` не привʼязаний до lifecycle і може тримати references після закриття екрана. Краще `viewModelScope`, `lifecycleScope` або явно керований scope.

6. **WeakReference не є архітектурним рішенням**

`WeakReference` іноді корисний для low-level випадків, але не має замінювати правильний lifecycle management. Якщо потрібен `WeakReference`, часто проблема в ownership.

7. **Anonymous callbacks**

Callback може тримати Fragment/Activity:

```kotlin
api.registerCallback(object : Callback {
    override fun onResult() { render() }
})
```

Треба unregister у відповідному lifecycle callback або використовувати lifecycle-aware API.

8. **Compose**

У Compose не варто тримати Activity/View references у `remember` без потреби. Side effects треба чистити через `DisposableEffect`:

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

9. **Інструменти**

Для пошуку leaks використовують:

- LeakCanary;
- Android Studio Memory Profiler;
- heap dumps;
- allocation tracking.

LeakCanary часто достатній для раннього виявлення типових Activity/Fragment leaks.

10. **Практичне правило**

- Long-living objects не тримають Activity/View.
- Fragment очищає binding/view refs у `onDestroyView()`.
- UI coroutines привʼязані до View lifecycle.
- Listeners/callbacks unregister-яться.
- `GlobalScope` не використовувати для UI/app flow.
- Перевіряти leaks LeakCanary.

**Коротко:** щоб уникати memory leaks в Android, не тримай Activity/View context у singleton, очищай Fragment binding і adapters у `onDestroyView`, використовуй lifecycle-aware coroutines, unregister listeners/callbacks, не використовуй `GlobalScope` для UI і перевіряй leaks через LeakCanary/Profiler.

</details>
<details>
<summary>100. Як безпечно зберігати API-ключі в Android-додатку?</summary>

#### Kotlin

Повністю безпечно зберегти API-ключ у Android-додатку неможливо: усе, що потрапило на client, потенційно можна витягнути через decompile, proxy, memory inspection або rooted device. Тому головна стратегія — не класти справді секретні ключі в app.

1. **Не зберігати backend secrets у клієнті**

Погано:

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Якщо ключ дає доступ до backend/admin/payment/service account — він не має бути в APK/AAB. Такі ключі мають жити на сервері.

2. **Використовувати backend proxy**

Замість прямого виклику third-party API з secret key:

```text
Android app -> your backend -> third-party API
```

Backend додає secret, перевіряє user/session, rate limits і повертає тільки потрібні дані.

3. **Public keys vs secrets**

Не всі ключі однакові:

```text
public/client key -> можна обмежити, не є критичним секретом
secret/admin key  -> не можна класти в app
```

Наприклад, Firebase config не є повноцінним secret, але його треба захищати rules, restrictions і auth.

4. **Обмеження ключів**

Якщо ключ все ж використовується в app, його треба обмежити:

- package name;
- SHA-256 certificate fingerprint;
- allowed APIs;
- quotas;
- rate limits;
- environment separation;
- monitoring і rotation.

Це не робить ключ невидимим, але зменшує шкоду при витоку.

5. **BuildConfig / local.properties**

```kotlin
buildConfigField("String", "API_KEY", ""${apiKey}"")
```

Це зручно, щоб не комітити ключ у Git, але не захищає ключ у зібраному APK. `BuildConfig` легко побачити після decompile.

6. **NDK / obfuscation**

NDK, ProGuard/R8, string obfuscation можуть ускладнити extraction, але не гарантують безпеку. Це лише friction, а не security boundary.

7. **Android Keystore**

Android Keystore корисний для ключів, створених на device, наприклад для encryption/signing. Але він не вирішує проблему hardcoded API secret: якщо secret треба покласти в app, attacker може знайти момент, де app його використовує.

8. **Токени користувача**

Access/refresh tokens треба зберігати обережно:

- EncryptedSharedPreferences або DataStore + encryption;
- Android Keystore для crypto key;
- короткий lifetime access token;
- refresh token rotation;
- logout очищає storage;
- backend може revoke tokens.

9. **CI/CD secrets**

Secrets для build/release мають бути в CI secret storage, а не в repo:

```text
GitHub Actions Secrets / Bitrise Secrets / GitLab CI Variables
```

У repo не має бути `.env` з production secrets.

10. **Практичне правило**

- Справжні secrets — тільки на backend.
- Client keys — обмежувати і моніторити.
- Не комітити ключі в Git.
- Не вважати `BuildConfig`, NDK або obfuscation захистом.
- User tokens — шифрувати і робити revocable.
- Додати rotation plan на випадок витоку.

**Коротко:** справжні API secrets не можна безпечно зберегти в Android app. Їх треба тримати на backend і викликати через proxy. Якщо client key неминучий, його обмежують package/signature/API/quota, не комітять у Git, моніторять і мають план rotation. `BuildConfig` та obfuscation не є security boundary.

</details>
<details>
<summary>101. Як реалізувати offline-first архітектуру?</summary>

#### Kotlin

Offline-first: UI завжди читає локальний source of truth, а network синхронізує його.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

Це дозволяє показувати cached data без мережі та автоматично отримувати updates після sync.

### Read path

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities ->
            entities.map(ArticleEntity::toDomain)
        }

suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(
            remote.map(ArticleDto::toEntity)
        )
    }
}
```

Network response транзакційно merge-иться в DB, а Room Flow оновлює UI. ViewModel не залежить від API/cache details.

`replaceAll` небезпечний, якщо response partial або є pending local edits: потрібен upsert/merge policy.

### Write path та outbox

Offline write спочатку записують локально:

```kotlin
database.withTransaction {
    dao.updateTitle(
        id = id,
        title = title,
        syncStatus = SyncStatus.Pending
    )
    outboxDao.insert(
        PendingOperation.UpdateTitle(id, title)
    )
}
```

UI одразу бачить optimistic change. Worker відправляє outbox operations і видаляє їх лише після server acknowledgement.

Data update та outbox entry мають бути в одній transaction, інакше process death може втратити operation. Для нових objects потрібен stable client ID або mapping до server ID.

### Sync

```kotlin
override suspend fun doWork(): Result =
    try {
        repository.syncPending()
        Result.success()
    } catch (exception: CancellationException) {
        throw exception
    } catch (exception: IOException) {
        Result.retry()
    }
```

WorkManager дає network constraint, backoff та process-death resilience. Sync має бути idempotent, бо request може повторитися.

Foreground refresh і worker повинні використовувати одну sync implementation та не виконувати conflict operations паралельно.

### Conflicts

Стратегія залежить від domain:

- server wins;
- client wins;
- version/revision-based last write;
- field-level merge;
- manual resolution.

Client timestamp ненадійний через clock skew; краще server revision/ETag. Для financial data простий last-write-wins неприйнятний.

Conflict не можна нескінченно retry-ити: operation отримує failed/conflict state, а UI дає користувачу зрозумілий resolution.

### UI state

UI має розрізняти:

- cached data;
- initial loading без local data;
- refreshing зі старими data;
- offline;
- pending/failed changes;
- last successful sync.

Connectivity — лише hint: network може бути формально доступна, але backend недоступний. Реальний результат request-а є джерелом істини.

### Основні ризики

- non-transactional writes;
- дублікати через non-idempotent retry;
- втрата outbox;
- overwrite pending edits remote data;
- неправильний conflict policy;
- stale cache без freshness rules;
- одночасні sync workers.

**Коротко:** Room є source of truth. Remote reads merge-яться в DB, local writes одразу змінюють DB і додаються в transactional outbox, WorkManager повторює idempotent sync, а conflict та freshness policy визначає domain.

</details>
<details>
<summary>102. Що таке архітектура MVI і коли її використовувати?</summary>

#### Kotlin

MVI — підхід з unidirectional data flow:

```text
UI -> Event/Intent -> ViewModel/Reducer -> State -> UI
                                      └-> Effect
```

UI рендерить immutable state і надсилає actions. ViewModel перетворює inputs у новий state, тому transitions явні, а screen має один source of truth.

### Основні частини

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
    data class ShowSnackbar(
        val message: String
    ) : ProfileEffect
}
```

- `State` — durable screen model;
- `Event` — user/system input;
- `Reducer` — `old state + result -> new state`;
- `Effect` — зовнішня одноразова дія.

Не кожна MVI-реалізація має окремий reducer або effect type. Ключова вимога — односпрямований потік та контрольоване оновлення state.

### ViewModel і UI

```kotlin
private val mutableState =
    MutableStateFlow(ProfileState())

val state = mutableState.asStateFlow()

fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.Load,
        ProfileEvent.Retry -> load()

        ProfileEvent.Back -> navigateBack()
    }
}
```

State оновлюють атомарно:

```kotlin
mutableState.update {
    it.copy(
        isLoading = false,
        user = loadedUser,
        error = null
    )
}
```

Compose UI тільки читає state й надсилає events:

```kotlin
val state by viewModel.state
    .collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Однаковий state дає однаковий UI, тому screen легко preview-ити й тестувати.

### Concurrency та effects

MVI не усуває races автоматично. Для повторних requests потрібна policy:

- cancel previous;
- ignore duplicate while loading;
- serialize events;
- дозволити parallel requests і визначити merge order.

Effects через `SharedFlow(replay = 0)` можуть загубитися без active collector-а. Navigation/result, який має пережити recreation, краще моделювати як state з acknowledgement або зберігати окремо.

Cancellation також не можна перетворювати на звичайний error state.

### Переваги

- один immutable screen state;
- передбачуваний data flow;
- явні state transitions;
- проста перевірка reducer/ViewModel;
- зручно для Compose і складних state machines;
- можливість логувати events та states.

### Недоліки

- sealed types і boilerplate;
- reducer/handler може стати god object;
- effect delivery потребує окремої policy;
- великий state може часто копіюватися;
- простий CRUD screen стає складнішим без користі.

### Коли використовувати

MVI доречний для screens із багатьма взаємозалежними states/actions, optimistic updates, retry та складними transitions. Для простого screen достатньо ViewModel із methods і `StateFlow` без формального reducer/effect шару.

**Коротко:** MVI організує UI як цикл `Event -> State transition -> Render`. Він корисний для складних stateful screens, але вимагає явних concurrency та effect-delivery правил і може бути зайвим для простого UI.

</details>
<details>
<summary>103. Як проєктувати масштабовані Android-додатки?</summary>

#### Kotlin

Масштабованість — можливість локально змінювати feature без каскадних змін у всьому app. Вона охоплює код, ownership команди, build time і production support.

### Boundaries

```text
UI -> ViewModel -> domain -> repository -> API/DB
```

- UI рендерить state;
- ViewModel координує screen state;
- domain містить бізнес-правила;
- repository приховує data sources;
- data layer працює з Retrofit/Room.

Шар не додають механічно. Use case або interface потрібен, якщо є business rule, boundary, alternative implementation чи потреба в окремому тестуванні.

Android, DTO та Entity types не повинні витікати через усі шари без причини.

### Feature-first modules

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Feature module групує код, що змінюється разом. Core містить справді shared infrastructure, а не випадкові utilities.

Модуль виправданий, якщо дає ownership, public API, dependency isolation або build benefit. Надто дрібна modularization збільшує Gradle/DI/navigation boilerplate.

### Dependency rules

```text
app -> feature API
feature -> core
core -X-> feature
```

Feature не повинна залежати від implementation іншої feature. Для integration використовують navigation contract, shared capability або API/implementation split.

Cycles і широкі public APIs руйнують isolation. Boundaries контролюють `internal`, convention plugins, dependency analysis та architecture tests.

### Contracts і models

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Interface потрібен на реальному boundary, а не для кожного class.

DTO, Entity, Domain і UiModel розділяють, якщо вони мають різні semantics або lifecycle. Зайвий mapping однакових models лише додає boilerplate.

### State, errors, concurrency

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: UiError? = null
)
```

UI отримує один observable state та надсилає events. ViewModel визначає concurrency policy: cancel previous, serialize або merge requests.

Infrastructure exceptions перетворюють на domain/UI outcomes на boundary; raw `Throwable` не має бути presentation contract-ом.

### DI та tests

Dependencies передають constructor-ом, scopes відповідають owner lifecycle. Hilt/Koin/manual DI — лише спосіб побудови graph-а.

Тести:

- pure domain/unit;
- ViewModel state;
- repository + API/DB integration;
- UI;
- мінімум end-to-end.

Якщо business logic тестується лише на emulator, Android boundary занадто широка.

### Build і production

- convention plugins і version catalog;
- мінімальні feature dependencies;
- affected-module CI;
- DB/API migration plan;
- logging, metrics і crash context;
- feature flags для risky rollout.

Модульна архітектура без observability та migration strategy не масштабується в production.

### Критерій

Хороша структура дозволяє одній команді змінити, протестувати й доставити feature без знання всього codebase. Якщо одна вимога змінює багато unrelated modules, boundaries проведені неправильно.

**Коротко:** масштабований Android-app має feature ownership, контрольований dependency graph, stable contracts, односпрямований state, explicit DI та testable boundaries. Модулі й шари додають лише тоді, коли вони реально ізолюють зміни.

</details>
<details>
<summary>104. Що таке Jetpack Compose і чим він відрізняється від View System?</summary>

#### Kotlin

`Jetpack Compose` — declarative UI toolkit для Android. Замість XML і ручної зміни `View` інтерфейс описують Kotlin-функціями як результат поточного state.

1. **Declarative vs imperative**

У View System UI змінюють імперативно:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

У Compose описують потрібний результат:

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

Після зміни state Compose запускає recomposition потрібних частин UI.

2. **Composable functions**

UI будується через `@Composable` функції:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Composable має рендерити state і надсилати callbacks, а не виконувати business logic або I/O.

3. **State-driven UI**

Типова схема — immutable state із `ViewModel`:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val name: String = ""
)
```

```kotlin
@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsStateWithLifecycle()
    ProfileScreen(state = state)
}
```

ViewModel оновлює state, а UI лише відображає його.

4. **Recomposition**

`Recomposition` — повторне виконання composable після зміни прочитаного state.

Важливо:

- body може виконуватися багато разів;
- side effects запускають через effect APIs;
- локальні objects/state за потреби кешують через `remember`;
- screen state зазвичай належить `ViewModel`.

5. **Compose vs XML/View**

```text
View System -> XML + View classes + manual updates
Compose     -> Kotlin functions + state-driven rendering
```

Compose зменшує XML/adapter boilerplate, спрощує previews, reusable components і unidirectional data flow. View System спирається на mutable view tree, listeners та lifecycle View-об’єктів.

6. **Interoperability**

Compose і View System можна змішувати:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

View також можна вставити в Compose:

```kotlin
AndroidView(factory = { context ->
    TextView(context)
})
```

Тому міграція може бути поступовою, екран за екраном.

7. **Практичні нюанси**

- state reads мають бути локальними й передбачуваними;
- lazy lists потребують stable keys;
- side effects повинні бути lifecycle-aware;
- performance перевіряють profiling і benchmarks, а не кількістю recompositions;
- бізнес-логіку не переносять у composables.

**Коротко:** Compose описує UI як функцію від state та оновлює його через recomposition; View System вимагає вручну змінювати View. Обидва підходи сумісні, тому мігрувати можна поступово.

</details>
<details>
<summary>105. Що таке @Composable і що таке recomposition?</summary>

#### Kotlin

`@Composable` — це анотація для функцій Jetpack Compose, які можуть будувати UI. `Recomposition` — це повторний виклик composable-функцій, коли state, від якого вони залежать, змінився.

1. **Що таке @Composable**

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Composable-функція описує UI для поточного state. Вона не створює View напряму, як XML/View System, а бере участь у Compose runtime tree.

2. **Composable як функція від state**

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

Якщо `state` зміниться, Compose оновить відповідну частину UI.

3. **Що таке recomposition**

Recomposition — це повторний запуск composable, коли змінився observable state:

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

Коли `count` змінюється, Compose recomposition-ить `Text`/частину UI, яка залежить від `count`.

4. **State, який запускає recomposition**

Recomposition можуть викликати:

- `mutableStateOf`;
- `StateFlow.collectAsStateWithLifecycle()`;
- `LiveData.observeAsState()`;
- `rememberSaveable` state;
- derived state;
- зміна parameters composable.

5. **Composable може викликатись багато разів**

Тому в body не можна робити side effects:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // погано
}
```

Краще:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

6. **remember**

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

`remember` зберігає значення між recompositions. Без нього обʼєкт створювався б при кожному повторному виклику composable.

7. **Skipping recomposition**

Compose може пропустити recomposition, якщо parameters стабільні й не змінились. Тому важливо використовувати immutable UI models і не передавати зайві unstable objects.

8. **Recomposition не завжди проблема**

Recomposition — нормальний механізм Compose. Проблема виникає, коли:

- recomposition занадто широка;
- у composable є важка робота;
- створюються зайві обʼєкти;
- state читається не там, де треба;
- списки не мають stable keys.

9. **Практичні правила**

- Composable має бути side-effect free.
- UI має бути функцією від state.
- State читати якнайближче до місця використання.
- Expensive objects кешувати через `remember`.
- Side effects запускати через effect APIs.
- Для списків використовувати stable keys.
- Performance перевіряти profiler/Layout Inspector, а не здогадками.

**Коротко:** `@Composable` позначає функцію, яка описує UI в Compose. `Recomposition` — повторний виклик composable при зміні state. Це нормальний механізм оновлення UI, але composable має бути легким, без side effects у body, з локальним state reading і стабільними параметрами.

</details>
<details>
<summary>106. Що таке state hoisting?</summary>

#### Kotlin

`State hoisting` — перенесення state з composable до його parent або state holder. Дочірній компонент отримує значення та callback, тому стає керованим ззовні, reusable і testable.

1. **Stateful і stateless варіанти**

Stateful-компонент сам володіє значенням:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Після hoisting він стає stateless:

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
    SearchField(query, onQueryChange = { query = it })
}
```

State може жити в `remember`, `rememberSaveable`, `ViewModel` або іншому state holder.

2. **Screen-level state**

State екрана зазвичай піднімають до `ViewModel`:

```kotlin
data class SearchState(
    val query: String = "",
    val results: List<ItemUi> = emptyList()
)

class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

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

`Route` працює з `ViewModel`, а `SearchScreen` лише рендерить state та передає events. Завдяки цьому UI простіше preview-ити й тестувати.

3. **Unidirectional data flow**

Hoisting формує односпрямований цикл:

```text
parent передає state -> child рендерить UI
child надсилає event -> parent оновлює state
```

Один source of truth не дозволяє кільком компонентам зберігати несинхронізовані копії одного значення.

4. **Коли піднімати state**

State варто підняти до найнижчого спільного owner, який має його читати або змінювати:

- він потрібен кільком composables;
- parent має контролювати поведінку child;
- значення впливає на бізнес-логіку або має переживати конфігураційні зміни;
- компонент треба тестувати з різними станами.

Суто UI-деталі — animation progress, ripple або внутрішній expanded state — можна залишити локально, якщо зовнішній код не повинен ними керувати.

5. **Типовий API компонента**

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

Зазвичай API приймає data/state, потім callbacks, а `modifier` — окремим параметром. Callback описує event (`onFollowClick`), а не спосіб зміни state.

**Коротко:** `state hoisting` — передача state вниз, а events угору. Child стає stateless, а parent або `ViewModel` — єдиним source of truth.

</details>
<details>
<summary>107. У чому різниця між remember і rememberSaveable?</summary>

#### Kotlin

Обидва API зберігають state між recompositions. `rememberSaveable` додатково записує saveable value у saved state, тому може відновити його після configuration change або process recreation.

1. **remember**

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

`remember` прив’язаний до поточної composition. Після recreation Activity значення буде втрачено.

2. **rememberSaveable**

```kotlin
@Composable
fun Counter() {
    var count by rememberSaveable { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

3. **Порівняння**

```text
remember         -> survives recomposition
rememberSaveable -> survives recomposition + configuration change/process recreation
```

На Android автоматично підтримуються типи, сумісні із saved-state `Bundle`; для інших потрібен `Saver`.

4. **Input state**

```kotlin
var query by rememberSaveable { mutableStateOf("") }

TextField(
    value = query,
    onValueChange = { query = it }
)
```

Так текст відновиться після rotation.

5. **Custom Saver**

Для custom type описують, як перетворити його на saveable representation:

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

6. **Вибір API**

`remember` підходить для локального transient UI state:

- cached formatter/object;
- animation helper;
- state, який допустимо втратити при recreation.

`rememberSaveable` підходить для UI state, який користувач очікує зберегти:

- текст у полі вводу;
- selected tab/filter;
- simple form або toggle state.

7. **Коли потрібен ViewModel**

Для screen/business state краще ViewModel:

```kotlin
class ProfileViewModel : ViewModel() {
    val state: StateFlow<ProfileState> = TODO()
}
```

`rememberSaveable` не замінює `ViewModel`, repository або persistent storage. Він призначений для невеликого UI state, потрібного для відновлення екрана.

8. **Обмеження**

- saved state має обмежений розмір;
- великі objects і collections зберігати не слід;
- state має бути saveable напряму або через `Saver`;
- відновлення не є довготривалим persistence.

9. **Практичне правило**

- Recomposition only — `remember`.
- Configuration change для простого UI state — `rememberSaveable`.
- Screen/business state — ViewModel.
- Persistent data — repository/database/DataStore.

**Коротко:** `remember` живе в поточній composition. `rememberSaveable` може відновити невеликий saveable UI state після recreation. Screen/business state належить `ViewModel`, persistent data — storage layer.

</details>
<details>
<summary>108. Що таке rememberCoroutineScope?</summary>

#### Kotlin

`rememberCoroutineScope()` — це Compose API, який повертає `CoroutineScope`, привʼязаний до lifecycle поточної composition. Його використовують, коли coroutine треба запускати з callback: click, swipe, snackbar, scroll animation.

1. **Базовий приклад**

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

`showSnackbar()` — suspend function, тому її не можна викликати напряму з `onClick`. Для цього і потрібен scope.

2. **Коли використовувати**

`rememberCoroutineScope()` доречний, коли coroutine стартує не автоматично, а внаслідок user action:

- показати snackbar;
- викликати `animateScrollToItem()`;
- відкрити/закрити drawer;
- запустити bottom sheet animation;
- виконати коротку UI-операцію з suspend API.

3. **Приклад зі scroll**

```kotlin
@Composable
fun UsersList(users: List<UserUi>) {
    val listState = rememberLazyListState()
    val scope = rememberCoroutineScope()

    Button(onClick = {
        scope.launch {
            listState.animateScrollToItem(0)
        }
    }) {
        Text("Scroll to top")
    }

    LazyColumn(state = listState) {
        items(users) { user -> UserRow(user) }
    }
}
```

4. **Lifecycle scope**

Scope автоматично скасовується, коли composable виходить з composition. Це захищає від частини leaks, але не означає, що в ньому треба запускати довгі бізнес-задачі.

5. **LaunchedEffect vs rememberCoroutineScope**

```text
LaunchedEffect            -> coroutine стартує як effect при вході/зміні key
rememberCoroutineScope    -> coroutine стартує вручну з callback
```

Приклад `LaunchedEffect`:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Приклад `rememberCoroutineScope`:

```kotlin
Button(onClick = {
    scope.launch { drawerState.open() }
})
```

6. **Не для бізнес-логіки**

Погано:

```kotlin
Button(onClick = {
    scope.launch {
        repository.saveUser(user)
    }
})
```

Краще передати event у ViewModel:

```kotlin
Button(onClick = { viewModel.onSaveClick() })
```

А вже ViewModel використовує `viewModelScope` для бізнес-операції.

7. **Чому не GlobalScope**

```kotlin
GlobalScope.launch { }
```

`GlobalScope` не привʼязаний до lifecycle UI і може продовжити роботу після закриття екрана. У Compose для UI-coroutines краще `rememberCoroutineScope`, а для screen/business logic — `viewModelScope`.

8. **Практичні ризики**

- запуск довгих jobs із composable;
- дублювання business logic в UI;
- забуте скасування сторонніх listeners;
- використання scope для ефектів, які краще оформити через `LaunchedEffect`;
- запуск кількох однакових coroutine при швидких кліках.

9. **Практичне правило**

Використовуй `rememberCoroutineScope()` для коротких UI suspend actions, які стартують з callback. Для завантаження даних, save, sync, network/database операцій — ViewModel і `viewModelScope`.

**Коротко:** `rememberCoroutineScope()` дає coroutine scope, привʼязаний до composition. Він потрібен для запуску suspend UI-операцій з callbacks, наприклад snackbar, drawer або scroll animation. Для бізнес-логіки краще використовувати ViewModel і `viewModelScope`.

</details>
<details>
<summary>109. Що таке collectAsState?</summary>

#### Kotlin

`collectAsState()` — це Compose API, який collect-ить `Flow` і перетворює його на Compose `State`. Коли Flow emit-ить нове значення, Compose бачить зміну state і запускає recomposition.

1. **Базова ідея**

ViewModel віддає `StateFlow`:

```kotlin
class ProfileViewModel : ViewModel() {
    val state: StateFlow<ProfileState> = MutableStateFlow(ProfileState())
}
```

Compose читає його як state:

```kotlin
@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsState()
    ProfileScreen(state = state)
}
```

2. **Як це працює**

```text
Flow emit -> collectAsState updates State -> recomposition -> UI shows new state
```

Composable не має вручну підписуватись на Flow через coroutine. `collectAsState()` робить це за нього.

3. **Initial value**

Для звичайного `Flow` треба передати початкове значення:

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

Для `StateFlow` initial value вже є в `state.value`, тому окремий `initial` зазвичай не потрібен.

4. **collectAsStateWithLifecycle**

В Android краще використовувати lifecycle-aware версію:

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Вона collect-ить Flow тільки коли lifecycle у відповідному активному стані. Це безпечніше для Android screens і зменшує зайву роботу, коли екран неактивний.

5. **collectAsState vs collectAsStateWithLifecycle**

```text
collectAsState              -> Compose runtime API, без Android lifecycle
collectAsStateWithLifecycle -> Android lifecycle-aware API
```

Для Android app зазвичай краще `collectAsStateWithLifecycle()`.

6. **StateFlow у ViewModel**

Правильний pattern:

```kotlin
private val _state = MutableStateFlow(ProfileState())
val state: StateFlow<ProfileState> = _state.asStateFlow()
```

UI отримує read-only `StateFlow`, а ViewModel змінює `_state`.

7. **Не collect-ити Flow напряму в body**

Погано:

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // не можна напряму в composable body
}
```

Для state використовують `collectAsState`, для one-shot effects — `LaunchedEffect` + `collect`.

8. **One-shot events**

`collectAsState()` не підходить для navigation/toast/snackbar events. Для них краще `SharedFlow` і `LaunchedEffect`:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigate or show snackbar
    }
}
```

State — це те, що можна повторно відрендерити. Event — це одноразова дія.

9. **Performance**

Якщо Flow emit-ить дуже часто, UI може часто recomposition-итись. Треба:

- emit-ити тільки зміни, які потрібні UI;
- використовувати `distinctUntilChanged()`;
- розділяти великий state на менші частини;
- не робити важку роботу в composable.

10. **Практичне правило**

- `StateFlow` для screen state.
- `collectAsStateWithLifecycle()` для Android Compose UI.
- `collectAsState()` для non-Android або простих Compose scenarios.
- `LaunchedEffect` для one-shot effects.
- Не змішувати state і events.

**Коротко:** `collectAsState()` перетворює `Flow`/`StateFlow` у Compose `State`, щоб UI автоматично recomposition-ився при нових значеннях. В Android краще використовувати `collectAsStateWithLifecycle()`, а для одноразових events — `LaunchedEffect` і окремий `SharedFlow`.

</details>
<details>
<summary>110. Що таке Modifier у Compose?</summary>

#### Kotlin

`Modifier` у Jetpack Compose — це обʼєкт, який описує, як composable має виглядати, розміщуватись і поводитись: padding, size, background, click handling, semantics, scroll, layout behavior тощо.

1. **Базовий приклад**

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

Modifier не змінює composable напряму, а додає ланцюжок поведінок/обгорток навколо нього.

2. **Порядок має значення**

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

дадуть різний результат. У першому випадку background буде після padding, у другому — до padding.

3. **Modifier як параметр**

Composable має приймати `modifier` ззовні:

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

Це дозволяє parent-у керувати позиціонуванням і зовнішнім виглядом.

4. **Layout modifiers**

```kotlin
Modifier
    .fillMaxWidth()
    .height(56.dp)
    .padding(horizontal = 16.dp)
```

Ці modifiers впливають на constraints, розмір і розміщення composable.

5. **Drawing modifiers**

```kotlin
Modifier
    .clip(RoundedCornerShape(12.dp))
    .background(Color.LightGray)
    .border(1.dp, Color.Gray)
```

Вони відповідають за візуальне оформлення.

6. **Input modifiers**

```kotlin
Modifier.clickable { onClick() }
```

Також є `pointerInput`, `draggable`, `scrollable`, `combinedClickable`. Для accessibility краще використовувати високорівневі modifiers, наприклад `clickable`, бо вони додають semantics.

7. **Semantics**

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Semantics потрібні для accessibility і UI tests.

8. **Parent data modifiers**

Деякі modifiers працюють тільки в конкретному parent scope:

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

`weight` доступний у `RowScope`/`ColumnScope` і передає дані parent layout-у.

9. **Performance**

Не треба без потреби створювати складні modifier chains у hot path. Якщо modifier великий і не залежить від state, його можна винести:

```kotlin
val itemModifier = Modifier
    .fillMaxWidth()
    .padding(16.dp)
```

Але головне — не робити важку логіку всередині modifier callbacks.

10. **Практичне правило**

- Composable приймає `modifier: Modifier = Modifier`.
- Parent передає зовнішнє позиціонування.
- Child може додавати внутрішні modifiers після переданого modifier.
- Порядок modifier-ів важливий.
- Для accessibility використовувати semantics/clickable правильно.

**Коротко:** `Modifier` у Compose — це ланцюжок налаштувань layout, drawing, input, semantics і поведінки composable. Він immutable, порядок викликів має значення, а reusable composables мають приймати `modifier` параметром, щоб parent міг керувати їхнім розміщенням.

</details>
<details>
<summary>111. У чому різниця між padding і offset?</summary>

#### Kotlin

Обидва modifier-и змінюють розташування content, але на різних етапах layout:

```text
padding -> додає простір і впливає на measurement
offset  -> змінює placement, але повідомляє parent старий розмір
```

### padding

```kotlin
Box(
    modifier = Modifier
        .background(Color.Gray)
        .padding(16.dp)
) {
    Text("Hello")
}
```

Padding зменшує constraints для child і додає відступи до measured size. Parent та siblings бачать цей простір.

У `Row`:

```kotlin
Row {
    Text(
        "A",
        Modifier.padding(start = 20.dp)
    )
    Text("B")
}
```

`B` розміститься після ширини `A` разом із padding. Для spacing між siblings також доречні `Arrangement` або `Spacer`, для lazy container — `contentPadding`.

### offset

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

Offset не змінює measured/reported size, а лише placement. Parent резервує початкове місце:

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` не відсувається, тому items можуть overlap. Це корисно для badge, drag, animation або декоративного shift.

### RTL і dynamic offset

`offset(x, y)` враховує layout direction: positive `x` рухається до logical end. `absoluteOffset()` використовує фізичні координати й не дзеркалиться в RTL.

Для часто змінюваного pixel offset є lambda overload:

```kotlin
Modifier.offset {
    IntOffset(
        x = animatedOffset.roundToPx(),
        y = 0
    )
}
```

Lambda overload читає state під час placement і може уникнути recomposition для часто змінюваної position.

### Порядок modifier-ів

Modifier chain застосовується послідовно. У цьому варіанті background охоплює padding:

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

Тут padding зовнішній, тому background малюється лише всередині нього.

Те саме стосується input:

```kotlin
Modifier
    .clickable(onClick)
    .padding(16.dp)
```

Зовнішній `clickable` включає padding у hit area. Якщо `padding` стоїть першим, цей зовнішній простір не належить clickable node.

Hit testing для `offset` слідує layout placement. Для суто render transform існує `graphicsLayer`, що має іншу семантику.

### Практичний вибір

- layout space, який враховують siblings — `padding`;
- overlap/visual shift без нового reserved space — `offset`;
- draw, input і layout semantics залежать від порядку modifiers.

**Коротко:** `padding` змінює constraints і reported size, тому впливає на siblings. `offset` змінює placement без додаткового reserved space; порядок modifiers впливає на draw та input bounds.

</details>
<details>
<summary>112. У чому різниця між fillMaxSize і matchParentSize?</summary>

#### Kotlin

Обидва modifier-и можуть дати child розмір parent-а, але беруть участь у measurement по-різному:

```text
fillMaxSize()     -> заповнює максимальні constraints і впливає на layout
matchParentSize() -> повторює вже визначений розмір Box
```

### fillMaxSize

`fillMaxSize()` встановлює ширину й висоту composable у максимальні значення, дозволені incoming constraints:

```kotlin
Column(
    modifier = Modifier
        .fillMaxSize()
        .padding(16.dp)
) {
    Text("Home")
}
```

Якщо constraints bounded розміром екрана, `Column` займе весь екран. Для окремої осі є `fillMaxWidth()` і `fillMaxHeight()`.

Fraction визначає частку доступного максимального розміру:

```kotlin
Modifier.fillMaxSize(0.5f)
```

Modifier не може перевищити constraints parent-а. За unbounded constraints на певній осі «заповнити максимум» на цій осі неможливо.

### matchParentSize

`matchParentSize()` — `BoxScope` modifier, доступний лише для direct children `Box`:

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

Overlay отримує точний розмір `Box`, але не визначає цей розмір.

### Measurement Box

Спрощено `Box`:

1. вимірює children без `matchParentSize()`;
2. визначає власний розмір;
3. вимірює matching children fixed constraints цього розміру.

Тому modifier підходить для background, scrim, loading layer, gradient або click overlay.

```kotlin
Box {
    Text("Small content")

    Box(
        Modifier
            .matchParentSize()
            .background(Color.Red.copy(alpha = 0.2f))
    )
}
```

Outer `Box` має розмір Text, а overlay лише покриває його.

Якщо замінити modifier на `fillMaxSize()`, child попросить maximum incoming constraints і може збільшити `Box`. Це інша семантика: child бере участь у визначенні розміру container-а.

### Практичний вибір

- root screen або container має зайняти весь доступний простір — `fillMaxSize()`;
- потрібно заповнити лише одну вісь — `fillMaxWidth/Height()`;
- overlay має повторити content-sized `Box` — `matchParentSize()`;
- child не повинен впливати на розмір `Box` — `matchParentSize()`;
- поза `BoxScope` використовують fill, weight або layout-specific modifier.

Порядок modifier-ів також важливий: `fillMaxSize().padding()` і `padding().fillMaxSize()` створюють різні constraints та області малювання.

**Коротко:** `fillMaxSize()` змушує composable заповнити максимально дозволені constraints і може визначати розмір parent layout. `matchParentSize()` працює лише в `BoxScope`, повторює вже обчислений розмір `Box` і не збільшує його.

</details>
<details>
<summary>113. Як обробляти кнопку “назад” у Compose?</summary>

#### Kotlin

У Compose кнопку “назад” обробляють через `BackHandler` з `androidx.activity.compose`. Він дозволяє перехопити system back press у межах composable і виконати власну логіку.

1. **Базовий BackHandler**

```kotlin
@Composable
fun EditScreen(
    onBack: () -> Unit
) {
    BackHandler {
        onBack()
    }

    // content
}
```

Коли користувач натискає back, виконається `onBack()`.

2. **enabled**

`BackHandler` можна вмикати умовно:

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Якщо `enabled = false`, back event піде далі: до іншого handler або Navigation.

3. **Unsaved changes**

Типовий сценарій — показати confirmation dialog:

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

4. **Navigation Compose**

Якщо використовується `NavController`, звичайний back часто обробляється автоматично:

```kotlin
navController.popBackStack()
```

Custom `BackHandler` потрібен, коли треба змінити стандартну поведінку: закрити dialog, bottom sheet, selection mode або підтвердити вихід.

5. **BottomSheet / Dialog / Modal state**

Back press часто має спочатку закривати локальний UI state:

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

І тільки якщо sheet/dialog не відкритий — виконувати navigation back.

6. **Порядок handlers**

Якщо є кілька `BackHandler`, зазвичай спрацьовує той, який ближчий/пізніше зареєстрований у composition. Тому handlers треба розміщувати близько до UI state, який вони обробляють.

7. **ViewModel і events**

Для складної логіки краще відправити event у ViewModel:

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

А ViewModel вирішує: показати dialog, зберегти draft, дозволити navigation або emit-нути effect.

8. **Predictive back**

Новіші Android версії мають predictive back. Якщо app має складні custom transitions або nested navigation, треба перевіряти поведінку back gesture на реальних API levels. Простий `BackHandler` покриває базовий case, але UX transition може вимагати додаткової інтеграції.

9. **Типові помилки**

- перехопити back завжди і зламати navigation;
- не врахувати dialog/sheet state;
- тримати back logic у random composable;
- не використовувати `enabled`;
- дублювати back handling у NavController і screen одночасно без правил.

10. **Практичне правило**

Back press має обробляти найближчий активний UI layer: dialog, bottom sheet, selection mode, unsaved changes, а вже потім screen navigation. `BackHandler(enabled = ...)` робить цю логіку явною.

**Коротко:** у Compose back press обробляють через `BackHandler`. Його вмикають через `enabled`, використовують для dialog/sheet/unsaved changes/selection mode, а стандартну навігацію часто лишають `NavController`. Для складної логіки back event краще передавати у ViewModel.

</details>
<details>
<summary>114. Для чого використовується rememberUpdatedState?</summary>

#### Kotlin

`rememberUpdatedState()` дає long-lived effect актуальне value/callback без restart. Він потрібен, коли lifecycle effect визначають одні keys, а інший input має оновлюватися всередині нього.

### Проблема

```kotlin
@Composable
fun SplashScreen(
    onTimeout: () -> Unit
) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` не restart-иться після зміни callback, тому coroutine може використати stale lambda.

### Рішення

```kotlin
@Composable
fun SplashScreen(
    onTimeout: () -> Unit
) {
    val currentOnTimeout by
        rememberUpdatedState(onTimeout)

    LaunchedEffect(Unit) {
        delay(2_000)
        currentOnTimeout()
    }
}
```

Timer не починається заново, а після delay викликається latest callback.

```text
effect key               -> визначає lifecycle/restart
rememberUpdatedState(x)  -> оновлює x всередині effect
```

### Чому не зробити value key

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

Після зміни lambda попередня coroutine скасується, а timer почнеться заново. Це правильно лише коли callback визначає нову operation.

Parent може створювати новий function object на recomposition, тому callback key здатен спричинити небажаний restart.

### DisposableEffect

```kotlin
val currentOnEvent by rememberUpdatedState(onEvent)
val owner = LocalLifecycleOwner.current

DisposableEffect(owner) {
    val observer = LifecycleEventObserver { _, event -> currentOnEvent(event) }
    owner.lifecycle.addObserver(observer)
    onDispose { owner.lifecycle.removeObserver(observer) }
}
```

Observer re-register-иться лише при зміні `owner`, але викликає latest handler.

### Коли value має бути key

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Зміна `userId` означає іншу operation, тому старий load треба скасувати. Приховувати ID через `rememberUpdatedState` було б semantic bug.

Питання для вибору:

```text
Зміна value має restart-ити effect?
так -> key
ні  -> rememberUpdatedState
```

### Що API не робить

API не запускає coroutine, не стабілізує lambda identity, не замінює effect keys і не зберігає value після виходу з composition.

Він повертає `State<T>`, яке effect читає в момент використання.

### Типові сценарії

- timeout із latest callback;
- listener без re-registration;
- animation completion handler;
- external subscription із lifecycle, визначеним іншими keys.

**Коротко:** `rememberUpdatedState()` оновлює value всередині effect без restart. Якщо зміна value означає нову operation, воно має бути key, а не `rememberUpdatedState`.

</details>
<details>
<summary>115. Що таке side-effects у Compose і як їх обробляти?</summary>

#### Kotlin

Side-effect у Compose — це дія, яка виходить за межі чистого рендерингу UI: запуск coroutine, snackbar, navigation, analytics, підписка на listener, робота з lifecycle або синхронізація з зовнішнім API.

1. **Чому це важливо**

Composable може recomposition-итись багато разів. Тому side-effect не можна запускати напряму в body:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // погано: може викликатись багато разів
}
```

Для side-effects у Compose є спеціальні APIs.

2. **LaunchedEffect**

Запускає coroutine, привʼязану до composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Якщо `userId` зміниться, попередня coroutine скасується і запуститься нова.

3. **rememberCoroutineScope**

Потрібен для запуску coroutine з callback:

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

Добре підходить для snackbar, drawer, bottom sheet, scroll animation.

4. **DisposableEffect**

Використовується для listeners/observers з cleanup:

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

Якщо щось підписали — треба відписати в `onDispose`.

5. **SideEffect**

Виконується після успішної recomposition:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Підходить для синхронізації Compose state із зовнішнім обʼєктом, але не для suspend work.

6. **rememberUpdatedState**

Дозволяє effect-у бачити актуальну lambda/value без restart:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

Корисно для timers, callbacks і long-running effects.

7. **produceState**

Конвертує async source у Compose `State`:

```kotlin
val user by produceState<User?>(initialValue = null, userId) {
    value = repository.loadUser(userId)
}
```

У Android production часто краще ViewModel + Flow + `collectAsStateWithLifecycle()`.

8. **snapshotFlow**

Перетворює Compose state у Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .collect { index -> analytics.trackScroll(index) }
}
```

Корисно для scroll analytics, pagination triggers і debounce logic.

9. **Практичне правило**

- Auto coroutine on composition — `LaunchedEffect`.
- Coroutine from click/callback — `rememberCoroutineScope`.
- Listener з cleanup — `DisposableEffect`.
- Sync після recomposition — `SideEffect`.
- Актуальна lambda без restart — `rememberUpdatedState`.
- Async source у state — `produceState`.
- Compose state у Flow — `snapshotFlow`.

**Коротко:** side-effects у Compose — це зовнішні дії, які не є чистим UI rendering. Їх треба запускати через effect APIs (`LaunchedEffect`, `DisposableEffect`, `SideEffect`, `rememberCoroutineScope`, `rememberUpdatedState`, `produceState`, `snapshotFlow`), щоб контролювати lifecycle, restart і cleanup.

</details>
<details>
<summary>116. Що таке ключі (keys) у LaunchedEffect і чому вони важливі?</summary>

#### Kotlin

Keys у `LaunchedEffect` визначають lifecycle coroutine-effect-а: коли його запустити, скасувати й перезапустити. Якщо key змінюється, Compose скасовує стару coroutine і запускає нову.

1. **Базовий приклад**

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
- `userId` не змінився — effect не restart-иться при recomposition;
- `userId` змінився — стара coroutine cancel, нова стартує;
- composable вийшов із composition — coroutine cancel.

2. **Помилка з неправильним key**

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Якщо `userId` зміниться, effect не перезапуститься, бо `Unit` не змінюється. Це може дати stale data.

Правильно:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

3. **Коли `LaunchedEffect(Unit)` нормальний**

`LaunchedEffect(Unit)` означає “запусти один раз для цього входу в composition”. Це підходить для initial load без параметрів, collection one-time effects або стартової анімації.

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // handle one-time effect
    }
}
```

Якщо effect залежить від параметра, цей параметр має бути key.

4. **Кілька keys**

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

Effect перезапуститься, якщо зміниться хоча б один key. Практичне правило: усе, що визначає результат side effect-а, має бути key.

5. **Unstable keys**

Погано:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Такі keys змінюються постійно й можуть запускати effect безконтрольно. Ключі мають бути stable і мати коректний `equals`.

6. **Cancellation при зміні key**

```kotlin
LaunchedEffect(query) {
    delay(300)
    viewModel.search(query)
}
```

Якщо `query` зміниться до завершення `delay`, попередня coroutine буде скасована. Це корисно для простих debounce-like сценаріїв, але складний debounce краще робити у ViewModel через `Flow`.

7. **`rememberUpdatedState`**

Якщо значення потрібне всередині effect, але його зміна не має restart-ити effect, використовують `rememberUpdatedState`:

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

Тут callback оновлюється, але timer не стартує заново.

8. **Практичне правило**

- Якщо зміна value має перезапустити effect — value має бути key.
- Якщо потрібна latest value без restart-а — `rememberUpdatedState`.
- Для одноразового effect-а — `LaunchedEffect(Unit)`.
- Для parameter-driven effect-а — `LaunchedEffect(parameter)`.
- Не використовувати random/time-based keys.

**Коротко:** keys у `LaunchedEffect` керують lifecycle coroutine side effect-а. Неправильні keys дають stale data, зайві restart-и або неконтрольовану поведінку UI.

</details>
<details>
<summary>117. Що таке snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` — це Compose API, який перетворює reads із Compose `State` у cold `Flow`. Його використовують, коли треба реагувати на зміни Compose state у coroutine/Flow pipeline: scroll analytics, pagination trigger, debounce, logging, side effects.

1. **Базова ідея**

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` відстежує state reads усередині lambda. Коли прочитане значення змінюється, Flow emit-ить нове значення.

2. **Для чого використовується**

Типові сценарії:

- scroll analytics;
- pagination trigger;
- reaction на Compose state у Flow operators;
- debounce/sample scroll або input state;
- logging/debugging;
- side effects, які не мають бути в composable body.

3. **Приклад pagination trigger**

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

Так можна реагувати на scroll position без прямої логіки в item composables.

4. **distinctUntilChanged**

`snapshotFlow` emit-ить при зміні snapshot state. Часто варто додавати:

```kotlin
.distinctUntilChanged()
```

щоб не обробляти однакові значення.

5. **debounce / sample**

Оскільки результат — Flow, можна використовувати operators:

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

6. **Де запускати**

`snapshotFlow` зазвичай запускають у `LaunchedEffect`:

```kotlin
LaunchedEffect(Unit) {
    snapshotFlow { state.value }
        .collect { /* side effect */ }
}
```

Не треба створювати collection напряму в composable body.

7. **Що не треба робити**

Не варто використовувати `snapshotFlow` для звичайного рендерингу UI. Якщо composable просто має показати state, треба читати state напряму:

```kotlin
Text(text = state.title)
```

`snapshotFlow` потрібен саме для переходу з Compose state у Flow side-effect pipeline.

8. **Важливий нюанс**

У lambda треба читати Compose state:

```kotlin
snapshotFlow { listState.firstVisibleItemIndex }
```

Якщо всередині немає snapshot state read, Flow не матиме що відстежувати.

9. **Практичне правило**

Використовуй `snapshotFlow`, коли потрібно спостерігати Compose state як Flow і застосувати Flow operators або виконати side effect. Не використовуй його як заміну звичайному state reading у UI.

**Коротко:** `snapshotFlow` конвертує Compose snapshot state reads у `Flow`. Він корисний для scroll analytics, pagination, debounce/sample, logging і side effects у `LaunchedEffect`. Для простого UI rendering state треба читати напряму, без `snapshotFlow`.

</details>
<details>
<summary>118. Як оптимізувати recomposition у Compose?</summary>

#### Kotlin

Оптимізація recomposition у Compose — це зменшення зайвих повторних викликів composable і важкої роботи під час rendering. Головна ціль — щоб змінювалась тільки та частина UI, яка реально залежить від зміненого state.

1. **Тримати state ближче до місця використання**

Погано, коли весь екран читає один великий state і передає його всюди:

```kotlin
ProfileScreen(state = hugeState)
```

Краще передавати конкретні частини:

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Так Compose може recomposition-ити менші ділянки UI.

2. **Не читати зайвий state**

Якщо composable читає state, він може recomposition-итись при зміні цього state. Тому item у списку не має читати весь screen state, якщо йому потрібен тільки `user`.

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

3. **Stable keys у LazyColumn**

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

Keys допомагають Compose правильно зберігати state і не плутати items при зміні списку.

4. **contentType для різних item types**

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` допомагає оптимізувати reuse composition для однотипних елементів.

5. **remember для expensive objects**

Погано:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Краще:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Ще краще — підготувати formatted text у mapper/ViewModel, якщо це частина UI model.

6. **derivedStateOf**

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Корисно, коли source state змінюється часто, але UI має реагувати тільки на derived condition.

7. **rememberUpdatedState**

Якщо effect має використовувати актуальну lambda без restart:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

Це допомагає уникнути зайвих перезапусків effects.

8. **Immutable/stable models**

Compose краще працює з immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Mutable collections і нестабільні обʼєкти можуть заважати Compose пропускати recomposition.

9. **Не робити side effects у body**

Погано:

```kotlin
@Composable
fun Screen() {
    analytics.track("opened")
}
```

Краще:

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

Composable body може викликатись багато разів.

10. **Профілювання**

Оптимізацію треба перевіряти:

- Layout Inspector recomposition counts;
- Compose compiler metrics;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles.

11. **Практичне правило**

Не треба боротись із кожною recomposition. Recomposition — нормальна частина Compose. Проблема — коли вона занадто широка, часта або містить важку роботу.

**Коротко:** recomposition оптимізують через локальний state, менші composables, stable keys, `contentType`, immutable UI models, `remember`, `derivedStateOf`, правильні side effects і профілювання. Важливо не прибрати всі recompositions, а зробити їх дешевими й локальними.

</details>
<details>
<summary>119. Як дебажити проблеми з recomposition?</summary>

#### Kotlin

Проблеми з recomposition у Compose дебажать через вимірювання: Layout Inspector, recomposition counters, logs, Compose compiler metrics, profiler і аналіз state reads. Мета — знайти, що саме часто змінюється і яку частину UI це зачіпає.

1. **Layout Inspector**

Android Studio Layout Inspector може показувати recomposition count для composables. Це перший інструмент, коли є підозра на зайві recompositions.

Важливо дивитись не тільки на кількість, а й на контекст: часта recomposition не завжди проблема, якщо composable легкий.

2. **Log у composable**

Для швидкої перевірки можна тимчасово додати log:

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Це грубий метод, його не треба залишати в production.

3. **SideEffect для debug**

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

`SideEffect` виконається після успішної recomposition.

4. **Перевірити state reads**

Часто проблема в тому, що composable читає занадто широкий state:

```kotlin
UserRow(screenState = state)
```

Краще передати тільки потрібне:

```kotlin
UserRow(user = user, isSelected = selectedUserId == user.id)
```

State треба читати якнайближче до місця використання.

5. **Перевірити stable keys у списках**

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

Без stable keys Compose може плутати state items після insert/delete/reorder.

6. **Перевірити unstable parameters**

Якщо composable отримує mutable list, lambda або object, який постійно створюється заново, це може провокувати recomposition.

Погано:

```kotlin
UserList(users = users.toMutableList())
```

Краще передавати immutable UI models і не створювати нові objects без потреби.

7. **remember для expensive objects**

Погано:

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Краще:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Але якщо це UI text, часто краще підготувати його в mapper/ViewModel.

8. **Compose compiler metrics**

Compose compiler може показати, які composables restartable/skippable і які типи unstable. Це корисно, коли проблема не очевидна з Layout Inspector.

9. **derivedStateOf**

Якщо state змінюється часто, а UI залежить від derived condition:

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Це зменшує зайві реакції UI на кожну дрібну зміну source state.

10. **Практичний checklist**

- Подивитись recomposition count у Layout Inspector.
- Знайти, який state змінюється.
- Звузити state reads.
- Додати stable `key` у lazy lists.
- Прибрати важку роботу з composable body.
- Перевірити unstable parameters.
- Використати `remember`/`derivedStateOf`, де це обґрунтовано.
- Підтвердити результат profiler/metrics.

**Коротко:** recomposition дебажать через Layout Inspector, logs, Compose compiler metrics і аналіз state reads. Найчастіші причини проблем — занадто широкий state, unstable parameters, відсутні keys у списках, важка робота в composable body і неправильне використання derived state.

</details>
<details>
<summary>120. Які є способи уникнути лагів у Compose, наприклад під час скролу?</summary>

#### Kotlin

Лаги під час скролу зазвичай спричиняють важка робота на main thread, зайві recompositions, allocations, нестабільні items або складний UI кожного рядка.

1. **Використовувати stable keys**

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

`key` зберігає identity і state item після insert, delete або reorder.

2. **Вказувати contentType**

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` допомагає перевикористовувати composition сумісних типів items.

3. **Не виконувати важку роботу в composition**

Погано:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Краще створити обʼєкт один раз:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Якщо форматований текст є частиною UI model, його краще підготувати в mapper. I/O, bitmap decoding і важкі обчислення не виконують у composable.

4. **Передавати стабільні моделі**

Краще передавати immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Не слід без потреби створювати нові collections або wrappers при кожній recomposition. Stability треба перевіряти, а не припускати лише через `data class`.

5. **Звузити state reads**

Item має отримувати лише власні дані та callback, а не весь screen state:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

6. **Scroll-derived state**

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

`derivedStateOf` доречний, коли scroll змінюється часто, а UI реагує лише на похідну умову.

7. **Зображення**

Використовують image loader із cache та запитують розмір, близький до розміру UI:

```kotlin
AsyncImage(
    model = user.avatarUrl,
    contentDescription = null
)
```

Bitmap не декодують вручну в composable або на main thread.

8. **Paging**

Для великих джерел Paging завантажує дані порціями:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

9. **Профілювання**

Оптимізацію треба підтверджувати:

- recomposition counts і Layout Inspector;
- system traces та Android Studio Profiler;
- Macrobenchmark у release-подібній збірці;
- Baseline Profiles для startup і runtime performance.

**Коротко:** для smooth scroll потрібні stable keys, коректний `contentType`, вузькі state reads, мінімум роботи й allocations у items, cached images та Paging. Оптимізації підтверджують traces і Macrobenchmark.

</details>
<details>
<summary>121. Чим відрізняються анотації Stable та Immutable?</summary>

#### Kotlin

`@Stable` і `@Immutable` у Jetpack Compose допомагають compiler/runtime вирішувати, чи можна безпечно skip-ати recomposition. Це не “оптимізаційні магічні прапорці”, а contract між кодом і Compose.

1. **Головна різниця**

```text
@Immutable -> обʼєкт не змінюється після створення
@Stable    -> обʼєкт може змінюватися, але Compose бачить ці зміни
```

`@Immutable` дає сильнішу гарантію. `@Stable` підходить для state holder-ів із контрольованою mutable поведінкою.

2. **@Immutable**

`@Immutable` означає, що всі public properties не змінюються після створення:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Хороший immutable UI model:

- має `val` properties;
- не має прихованої мутації;
- поля теж immutable/stable;
- зміна state робиться через новий instance.

Погано:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

`MutableList` можна змінити inplace, а Compose може пропустити потрібну recomposition.

Краще:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Для суворішої гарантії можна використовувати immutable collections:

```kotlin
@Immutable
data class FeedUiState(
    val items: ImmutableList<FeedItemUiModel>
)
```

3. **@Stable**

`@Stable` означає, що тип має передбачувану поведінку для Compose:

- `equals` стабільний;
- public properties мають stable типи;
- mutable зміни повідомляються Compose через observable state.

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Клас mutable, але Compose бачить зміну `count`, бо вона йде через Compose state.

4. **Stable state holder**

```kotlin
@Stable
class SearchState {
    var query by mutableStateOf("")
        private set

    fun updateQuery(value: String) {
        query = value
    }
}
```

Погано:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

У другому прикладі Compose не дізнається про зміну `query`, бо це звичайне mutable поле.

5. **Коли що використовувати**

`@Immutable` підходить для:

- UI models;
- value objects;
- screen state data classes;
- sealed state models.

`@Stable` підходить для:

- custom state holders;
- controller objects;
- classes із `mutableStateOf`/`mutableIntStateOf`;
- обʼєктів, які контрольовано повідомляють Compose про зміни.

6. **Типові помилки**

- Анотувати mutable object як `@Immutable`.
- Ставити `@Stable` на клас зі звичайними mutable public fields.
- Використовувати анотації як “ліки” від поганої архітектури state.
- Тримати `MutableList`/`MutableMap` у UI state.
- Мутувати старий state замість створення нового.

**Коротко:** `@Immutable` каже Compose, що обʼєкт не змінюється після створення. `@Stable` каже, що обʼєкт може змінюватися, але Compose може коректно відстежити ці зміни. Обидві анотації треба ставити тільки тоді, коли тип реально відповідає contract-у.

</details>
<details>
<summary>122. Навіщо передавати key у LazyColumn і що він собою являє під капотом?</summary>

#### Kotlin

`key` задає стабільну identity item-а незалежно від position:

```kotlin
LazyColumn {
    items(
        items = users,
        key = User::id
    ) { user ->
        UserItem(user)
    }
}
```

Без key LazyColumn використовує position. Це працює для static list, але створює проблеми при insert, remove або reorder.

### Чому position недостатньо

```text
Було:       Після insert:
0 -> Alice  0 -> Kate
1 -> Bob    1 -> Alice
2 -> Chris  2 -> Bob
            3 -> Chris
```

За position Compose бачить новий content у старих slots. За `user.id` він розуміє, що старі items лише змінили position.

### Remembered state

```kotlin
@Composable
fun UserItem(user: User) {
    var expanded by rememberSaveable {
        mutableStateOf(false)
    }

    Column(
        Modifier.clickable {
            expanded = !expanded
        }
    ) {
        Text(user.name)
        if (expanded) Text(user.details)
    }
}
```

Без stable key state може залишитися біля position і перейти до іншого user. Key переміщує remembered state разом з identity.

Для `rememberSaveable` key має підтримуватися SaveableStateRegistry; на Android зазвичай використовують primitive/String/Bundle-compatible ID.

### Під капотом

Key стає identity composition group. Runtime використовує його, щоб:

- зіставити стару й нову group;
- перемістити/reuse group;
- зберегти `remember` state;
- відновити saveable state;
- визначити item для animations.

Це не `DiffUtil`: lazy layout використовує keys для composition/placement bookkeeping і композить потрібні visible items.

### Вимоги

Key має бути:

- unique серед sibling items;
- stable між recompositions;
- пов'язаний із domain identity;
- saveable для `rememberSaveable`.

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

Random/time створює нову identity: state губиться, groups пересоздаються, animations ламаються.

Index дорівнює default position identity й допустимий лише якщо склад і порядок list не змінюються. Duplicate keys порушують identity contract.

Composite key можливий, якщо стабільний:

```kotlin
key = { item ->
    "${item.type}:${item.id}"
}
```

### Animations і Paging

```kotlin
items(
    items = users,
    key = User::id
) { user ->
    UserItem(
        user = user,
        modifier = Modifier.animateItem()
    )
}
```

Stable key дозволяє розпізнати move замість remove + insert.

Paging:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

Key strategy має відповідати стабільній item identity та placeholder configuration.

### Практичне правило

Key потрібен для dynamic list, local item state, reorder і animations. Найкращий key — stable database/server/client-generated ID. Він описує identity, а не content чи position.

**Коротко:** `key` прив'язує lazy composition group і remembered state до item identity. Без нього identity базується на index; при insert/reorder state та animations можуть перейти до іншого item-а.

</details>
<details>
<summary>123. Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?</summary>

#### Kotlin

У `LazyColumn.items()` окрім самого списку можна передати `key` і `contentType`. Вони потрібні для стабільної identity елементів, збереження state і ефективнішого reuse composition.

1. **Базовий items**

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Це працює, але для списків, які змінюються, краще явно додати `key`.

2. **key**

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

`key` задає стабільний id item-а. Він допомагає Compose не плутати state після insert/delete/reorder.

3. **Навіщо key потрібен**

Без key Compose часто орієнтується на position. Якщо на початок списку додати новий item, local state може “переїхати” не до того елемента.

Хороший key:

- стабільний;
- унікальний;
- не залежить від позиції;
- не генерується випадково;
- зазвичай базується на domain id.

4. **contentType**

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

`contentType` описує тип UI-елемента. Наприклад: header, post, ad, loader.

5. **Для чого contentType**

`contentType` допомагає Compose ефективніше переиспользувати composition для однотипних елементів. Header краще reuse-ити як header, post — як post, loader — як loader.

6. **Приклад sealed items**

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

7. **itemsIndexed**

Якщо потрібен index:

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

Index не варто використовувати як key для mutable/reorderable списків.

8. **Коли contentType можна не давати**

Якщо всі item-и мають однаковий UI shape, `contentType` не критичний. Він найбільш корисний для mixed feed/chat/catalog lists.

9. **Практичне правило**

- Для production list додавай `key`.
- Для mixed list додавай `contentType`.
- `key` — identity item-а.
- `contentType` — форма/тип UI item-а.
- Не використовуй position або random value як key.

**Коротко:** у `LazyColumn.items()` важливі параметри `key` і `contentType`. `key` задає стабільну identity item-а і зберігає state при змінах списку. `contentType` описує тип UI-елемента і допомагає Compose ефективніше reuse-ити composition для mixed lists.

</details>
<details>
<summary>124. Що таке SOLID?</summary>

#### Kotlin

`SOLID` — це пʼять принципів обʼєктно-орієнтованого дизайну, які допомагають робити код менш звʼязаним, більш тестованим і простішим для змін.

1. **S — Single Responsibility Principle**

Клас має мати одну причину для зміни.

Погано, коли ViewModel робить усе:

```kotlin
class ProfileViewModel : ViewModel() {
    // network, database, mapping, UI state
}
```

Краще розділити відповідальності:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

ViewModel керує state, use case — бізнес-дією, repository — даними.

2. **O — Open/Closed Principle**

Код має бути відкритий для розширення, але закритий для постійного редагування.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Новий tracker можна додати новою реалізацією, не переписуючи весь код.

3. **L — Liskov Substitution Principle**

Підтип має коректно замінювати базовий тип.

Погано:

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Якщо реалізація не може виконати контракт, значить контракт неправильний або занадто широкий.

4. **I — Interface Segregation Principle**

Клієнт не має залежати від методів, які йому не потрібні.

Погано:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Краще розділити контракти:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

5. **D — Dependency Inversion Principle**

Високорівневий код має залежати від abstractions, а не concrete implementations.

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

6. **SOLID в Android**

Типова структура:

```text
UI -> ViewModel -> UseCase -> Repository interface -> Repository implementation -> API/DB
```

UI не знає про Retrofit/Room. Domain залежить від contracts. Data реалізує contracts.

7. **Типові порушення**

- Fragment напряму викликає API.
- ViewModel містить network і DB logic.
- Repository робить усе для всього app.
- Interface має надто багато методів.
- Use case залежить від concrete implementation.
- Абстракції створені без реальної потреби.

8. **Практичне правило**

SOLID — це не вимога створювати interface для кожного класу. Це спосіб тримати відповідальності, залежності й контракти чистими, щоб код можна було змінювати локально і тестувати без важкого Android оточення.

**Коротко:** SOLID — це SRP, OCP, LSP, ISP і DIP. В Android ці принципи допомагають не змішувати UI, бізнес-логіку і data layer, будувати код через contracts, уникати god classes і робити ViewModel/use cases/repositories тестованими.

</details>
<details>
<summary>125. Поясніть кожен принцип SOLID з прикладами з Android.</summary>

#### Kotlin

SOLID допомагає локалізувати зміни й тестувати бізнес-логіку окремо від Android framework, Retrofit і Room.

### S — Single Responsibility

Клас має одну узгоджену відповідальність:

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()

class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile =
        repository.loadProfile()
}
```

ViewModel керує UI state, use case — бізнес-операцією, repository — даними. SRP не означає «один метод на клас», а забороняє змішувати незалежні причини для зміни.

### O — Open/Closed

Поведінку розширюють через стабільний contract замість постійного редагування `when/if`:

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Новий handler додається без зміни існуючих. Abstraction потрібна лише навколо реальної точки варіативності.

### L — Liskov Substitution

Subtype має коректно замінювати base type. Якщо implementation не може виконати contract, ієрархія неправильна:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

`ReadOnlyStorage` не повинен реалізовувати `save()` і кидати `UnsupportedOperationException`. LSP охоплює не лише signature, а й guarantees, errors, side effects та invariants.

### I — Interface Segregation

Caller не повинен залежати від непотрібних методів:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Use case отримує вузький contract замість god-interface. Але надмірно дрібні interfaces додають boilerplate — межа має відповідати ролі caller-а.

### D — Dependency Inversion

High-level policy залежить від abstraction, а infrastructure її реалізує:

```kotlin
interface AuthRepository {
    suspend fun login(
        email: String,
        password: String
    ): User
}

class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(
        email: String,
        password: String
    ): User = repository.login(email, password)
}
```

Use case не створює Retrofit/Room напряму. Data layer реалізує contract, а DI підставляє implementation або fake:

```text
UI -> ViewModel -> UseCase -> Repository contract
                              ^
                              |
                     Data implementation -> API/DB
```

### Типові порушення

- Fragment напряму викликає API або DAO;
- ViewModel містить network, DTO mapping, cache і navigation;
- один repository відповідає за весь застосунок;
- subtype формально реалізує interface, але ламає contract;
- domain abstraction залежить від Android/infrastructure type;
- interface створюється для кожного класу без boundary чи альтернативної реалізації.

SOLID — не вимога додавати maximum шарів. Для простого feature зайві interfaces та use cases можуть лише збільшити boilerplate.

**Коротко:** SRP розділяє відповідальності, OCP задає точки розширення, LSP вимагає коректної заміни підтипів, ISP робить contracts вузькими, DIP спрямовує залежності від бізнес-логіки до abstractions. У Android це відділяє UI/domain від framework та data details.

</details>
<details>
<summary>126. Що таке ООП?</summary>

#### Kotlin

ООП моделює систему об’єктами, що поєднують state та behavior. Клас описує тип, object — його instance:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Основи: encapsulation, abstraction, inheritance і polymorphism.

### Інкапсуляція

Клас приховує mutable state та змінює його контрольованими operations:

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

Так клас захищає invariants. Read-only `List` звужує API, але назовні краще повертати snapshot, якщо backing collection може змінюватися.

### Абстракція

Абстракція відділяє contract від implementation:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

ViewModel не знає про Retrofit, Room або fake. Interface доречний, коли ізолює мінливу boundary, а не просто дублює кожен class.

### Наслідування

У Kotlin class і method `final` за замовчуванням; inheritance дозволяють через `open`:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Inheritance моделює «is-a», але сильно зв’язує subclass із base implementation. Композиція часто безпечніша.

### Поліморфізм

Polymorphism дозволяє підставляти різні implementations спільного contract:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader {
    override suspend fun load(url: String): Image = TODO()
}

class FakeImageLoader : ImageLoader {
    override suspend fun load(url: String): Image = TODO()
}
```

Caller працює з `ImageLoader`, тому production і test implementations взаємозамінні.

### ООП у Kotlin/Android

Kotlin доповнює класичне ООП:

- `data class` — value-like models;
- `sealed class/interface` — закриті hierarchies;
- extension functions — behavior без inheritance;
- constructor injection — explicit dependencies.

Композиція виражає «has-a» без base class coupling:

```kotlin
class UserRepository(
    private val errorMapper: ErrorMapper,
    private val logger: Logger
)
```

ООП не суперечить immutability чи functional style. Важливі responsibility boundaries, а не кількість interfaces/classes.

**Коротко:** ООП організовує state і behavior в objects через encapsulation, abstraction, inheritance та polymorphism. У Kotlin часто віддають перевагу immutable models і composition замість глибокого inheritance.

</details>
<details>
<summary>127. Що таке абстракція?</summary>

#### Kotlin

Абстракція — це спосіб приховати деталі реалізації і залишити тільки важливий контракт. У Kotlin/Android вона допомагає відділити “що робить код” від “як саме він це робить”.

1. **Просте визначення**

Абстракція описує поведінку без привʼязки до конкретної implementation:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Код, який використовує `UserRepository`, не знає, чи дані приходять з API, Room, cache або fake object у тесті.

2. **Interface як абстракція**

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Use case залежить від contract, а не від concrete class. Це зменшує coupling і спрощує тестування.

3. **Concrete implementation**

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

Деталі API/DB сховані за repository interface.

4. **Абстрактний клас**

Абстрактний клас може містити частину реалізації:

```kotlin
abstract class BaseViewModel : ViewModel() {
    protected fun logError(error: Throwable) {
        // shared logic
    }
}
```

Але в Android надмірні base classes часто створюють жорстку ієрархію. Часто composition краще за inheritance.

5. **Абстракція в Android архітектурі**

Типовий приклад:

```text
ViewModel -> UseCase -> Repository interface -> Repository implementation -> API/DB
```

UI не має знати про Retrofit, Room або cache strategy. Він працює зі state і events.

6. **Абстракція для тестів**

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

Тест може підставити fake repository без network/database.

7. **Абстракція vs інкапсуляція**

```text
Абстракція   -> показує тільки важливий контракт
Інкапсуляція -> ховає внутрішній стан і деталі реалізації
```

Вони часто працюють разом, але це не одне й те саме.

8. **Погана абстракція**

Погано створювати abstraction “про всяк випадок”:

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

Якщо є одна проста implementation і немає реальної потреби в заміні, interface може бути зайвим.

9. **Ознаки хорошої абстракції**

- має зрозумілий contract;
- приховує нестабільні деталі;
- не протікає implementation details;
- спрощує тестування;
- не додає зайвого boilerplate;
- відповідає реальній точці зміни.

10. **Практичне правило**

Абстракція потрібна там, де є варіативність або залежність від нестабільних деталей: network, database, file system, analytics, time provider, dispatchers. Але не треба абстрагувати кожен клас тільки тому, що “так архітектурно”.

**Коротко:** абстракція приховує деталі реалізації за зрозумілим контрактом. В Android це найчастіше interfaces для repositories, data sources, dispatchers або services. Хороша абстракція зменшує coupling і спрощує тести, погана — додає boilerplate без користі.

</details>
<details>
<summary>128. У чому різниця між конструкторами та методами?</summary>

#### Kotlin

Конструктор створює instance і формує його початковий валідний state. Метод описує поведінку вже створеного object та може викликатися багато разів.

1. **Конструктор**

Primary constructor оголошується в заголовку класу:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Параметри з `val`/`var` стають properties. Constructor не має return type і викликається через `User(...)`.

2. **Метод**

Метод — member function класу:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String {
        return name.trim()
    }
}
```

Метод виконує дію або повертає result через `user.displayName()`, але не створює instance.

3. **init block**

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

`init` blocks виконуються під час construction у порядку оголошення разом із property initializers.

4. **Secondary constructor**

Secondary constructor делегує primary через `this(...)`:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

Для простих overloads зазвичай достатньо default parameters:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

5. **Constructor injection**

В Android constructor часто використовують для DI:

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

Constructor задає required dependencies, а `invoke()` виконує use case.

6. **Що не робити в constructor**

Constructor має бути швидким і без lifecycle-dependent side effects. У ньому не запускають:

- network request;
- database query;
- coroutine;
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

7. **Практичне правило**

- constructor — required state, DI та validation;
- method — repeatable behavior після construction;
- важку, suspend або lifecycle-bound work запускають методом;
- складні способи створення можна винести у factory.

**Коротко:** constructor створює валідний object і приймає dependencies; method виконує його поведінку. Heavy, suspend та lifecycle work не запускають під час construction.

</details>
<details>
<summary>129. Що таке сигнатура методу?</summary>

#### Kotlin

Сигнатура — ознаки, за якими compiler розрізняє functions. У Kotlin overload resolution враховує name, receiver та parameter types/order; JVM має власний descriptor після compilation.

```kotlin
fun loadUser(id: String): User
```

Спрощено: `loadUser(String)`.

### Overloading

Overloads мають відрізнятися parameter list:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Return type або parameter name недостатньо:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflict
fun load(id: String)
fun load(userId: String) // conflict
```

Named arguments змінюють call-site readability, не signature.

Порядок типів має значення:

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

Такий API валідний, але часто нечитабельний.

### Default parameters

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

Це одна source function із synthetic default-argument mechanism. Для Java overloads можна згенерувати через `@JvmOverloads`.

Ручний overload поруч може створити conflict або ambiguous API.

### Override

Override має відповідати contract-у:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User =
        TODO()
}
```

Зміна parameter type створює іншу function. Override return type може бути covariant subtype.

`suspend` є частиною Kotlin contract. На JVM suspend method отримує `Continuation` parameter і object-like return representation.

### Extension receiver

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

Receiver бере участь у Kotlin resolution і компілюється як parameter static method-а. Extension не є virtual override.

### JVM-нюанси

Kotlin source signatures можуть зіткнутися після type erasure:

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // same JVM signature after erasure
```

Після erasure обидва стають `process(List)`. Рішення — інше name або `@JvmName`.

Nullability теж не створює окрему JVM signature:

```kotlin
fun load(id: String)
fun load(id: String?) // JVM clash
```

Тому Kotlin source contract треба відрізняти від JVM descriptor.

### Практичні правила

- overload: name + receiver + parameter types/order;
- return type та parameter names недостатні;
- override зберігає base contract;
- erasure/nullability можуть створити JVM clash;
- API має бути не лише валідним, а й читабельним.

**Коротко:** Kotlin розрізняє functions за name, receiver і parameters. JVM descriptor додає нюанси erasure, відсутності nullability та спеціального представлення suspend/extension functions.

</details>
<details>
<summary>130. У чому різниця між перевантаженням (overloading) та перевизначенням (overriding)?</summary>

#### Kotlin

```text
Overloading -> одна назва, різні parameter lists
Overriding  -> та сама сигнатура, нова реалізація в subtype
```

### Overloading

Перевантажені функції знаходяться в одному scope та відрізняються кількістю, типами або порядком параметрів:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(
        email: String,
        includeDetails: Boolean
    ): User = TODO()
}
```

Compiler вибирає overload на основі static types аргументів. Return type сам по собі не може відрізняти сигнатури:

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // compile error
```

У Kotlin багато overload-ів можна замінити default parameters і named arguments:

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
)

loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

`@JvmOverloads` генерує overload-и для Java callers, але зазвичай його додають лише на Java-facing API.

### Overriding

Subtype реалізує або змінює inherited method:

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

Сигнатура має відповідати contract-у. Зміна `String` на `Long` створить іншу функцію, а не override.

У Kotlin concrete classes і methods `final` за замовчуванням. Для override class member має бути `open` або `abstract`; members interface відкриті для реалізації:

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

### Compile time і runtime

Overloading — static/compile-time dispatch:

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

Вибір визначає declared type `Any`, а не runtime value.

Overriding — virtual/runtime dispatch:

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // CrashlyticsLogger implementation
```

Тому overriding забезпечує subtype polymorphism.

### Важливі нюанси

- Overload-и не повинні бути неоднозначними, особливо з default arguments.
- Override не може звужувати visibility базового member.
- Return type override може бути більш конкретним, якщо він covariant.
- Реалізація має зберігати contract базового типу — принцип підстановки Лісков.
- Extension function не override-иться: вона вибирається статично за declared type receiver-а.
- `final override` забороняє подальше перевизначення.

**Коротко:** overloading створює кілька функцій з однією назвою та різними параметрами й вирішується під час компіляції. Overriding замінює inherited реалізацію з тією самою сигнатурою та забезпечує runtime polymorphism.

</details>
<details>
<summary>131. У чому різниця між інтерфейсом та абстрактним класом?</summary>

#### Kotlin

Інтерфейс описує контракт поведінки, а абстрактний клас описує спільну базу з частковою реалізацією і станом. У Kotlin клас може реалізувати багато інтерфейсів, але наслідувати тільки один клас.

1. **Interface**

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Interface каже, що має вміти обʼєкт, але не зобовʼязує до конкретної реалізації.

2. **Abstract class**

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // shared logic
    }

    abstract suspend fun refresh()
}
```

Абстрактний клас може мати constructor, state, protected methods і часткову реалізацію.

3. **Multiple inheritance**

Клас може реалізувати кілька interfaces:

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Але наслідувати можна тільки один клас:

```kotlin
class MyRepository : BaseRepository()
```

Це важлива різниця для дизайну API.

4. **Default methods в interface**

У Kotlin interface може мати реалізацію методу:

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Але interface не має constructor-а і зазвичай не має власного backing state як клас.

5. **State**

Абстрактний клас може зберігати стан:

```kotlin
abstract class BaseCache {
    protected val memoryCache = mutableMapOf<String, Any>()
}
```

Interface краще використовувати для ролей/контрактів, а не для shared mutable state.

6. **Коли використовувати interface**

Interface підходить, коли потрібно:

- описати контракт;
- зробити dependency inversion;
- мати кілька реалізацій;
- спростити тестування через fake/mock;
- дозволити класу мати кілька ролей.

Приклад:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

7. **Коли використовувати abstract class**

Абстрактний клас підходить, коли потрібно:

- спільна реалізація;
- спільний protected state;
- base lifecycle;
- template method pattern;
- контроль через constructor.

Але в Android надмірні `BaseActivity`, `BaseFragment`, `BaseViewModel` часто стають проблемою.

8. **Composition over inheritance**

Якщо потрібна reusable поведінка, часто краще composition:

```kotlin
class UserRepository(
    private val logger: Logger
)
```

Замість глибокої ієрархії base classes.

9. **Практичне правило**

- Контракт/роль — interface.
- Спільний стан або часткова реалізація — abstract class.
- Для DI і тестів частіше interface.
- Не створювати base class тільки щоб “не дублювати 3 рядки”.
- Уникати глибокого наслідування.

**Коротко:** interface задає контракт і дозволяє багато реалізацій/ролей, тому добре підходить для DI і тестування. Abstract class дає спільну базову реалізацію, constructor і state, але обмежує наслідування одним базовим класом. У modern Android частіше краще interface + composition, ніж глибокі base classes.

</details>
<details>
<summary>132. Чи може інтерфейс не містити методів?</summary>

#### Kotlin

Так. Порожній interface називають marker interface. Він не задає поведінку, а надає типу семантичну роль, яку можна використати в type system.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Тепер `UserProfile` можна приймати як `Cacheable`, перевіряти через `is Cacheable` або використовувати як generic bound.

### Generic constraint

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // accepts only explicitly marked types
}
```

```kotlin
data class DraftMessage(
    val id: String,
    val text: String
) : Syncable
```

Marker робить допустимі types явними на compile time. Але якщо sync потребує `id` або іншої поведінки, property/method треба додати до contract-а замість cast або reflection.

### Sealed root

Порожній sealed interface часто групує закритий набір states/events:

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

Compiler знає direct subclasses у дозволених межах module/package і перевіряє exhaustive `when`.

Це не просто marker для runtime check, а algebraic sum type: значення належить одному з визначених variants.

### Класичний приклад

`java.io.Serializable` — marker interface: сам не має methods, але Java serialization runtime перевіряє, чи object його реалізує.

Marker interface є частиною public type hierarchy, тому його додавання або видалення може впливати на overload resolution, generic bounds і binary/API compatibility.

### Interface чи annotation

Marker interface:

```kotlin
interface Cacheable
```

Annotation:

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Interface краще, коли потрібні:

- compile-time subtype relation;
- generic bounds;
- polymorphic API;
- sealed hierarchy;
- `is` check без reflection metadata lookup.

Annotation краще для metadata, яку обробляє compiler plugin, code generator або runtime reflection, і коли не потрібна subtype relation.

Retention annotation визначає, чи metadata існує лише в source, binary або runtime.

### Коли marker поганий

```kotlin
interface Important
interface Special
interface DataObject
```

Такі types створюють noise, якщо API їх не використовує. Marker також не повинен замінювати реальний behavioural contract.

Погані ознаки:

- marker ніде не є parameter/bound/root type;
- логіка розгалужується на десятки `is Marker`;
- потрібні data/methods, але caller робить casts;
- роль краще виражається enum/property;
- annotation processing точніше відповідає задачі.

### Практичний вибір

- група subtype-ів — marker/sealed interface;
- generic compile-time restriction — marker interface;
- metadata для tooling/runtime — annotation;
- реальна поведінка — methods/properties;
- simple category у моделі — інколи enum/property;
- не створювати marker «на майбутнє».

**Коротко:** interface може не містити methods. Порожній marker дає типову роль для subtype relation, generic bounds або sealed hierarchy. Якщо потрібна metadata — краще annotation, якщо поведінка — явний contract із members.

</details>
<details>
<summary>133. Чи може інтерфейс містити інший інтерфейс?</summary>

#### Kotlin

Так, інтерфейс у Kotlin може містити інший інтерфейс. Це називається nested interface. Його використовують, коли вкладений contract логічно належить зовнішньому API і не має достатнього сенсу як окремий top-level тип.

1. **Базовий приклад**

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

Вкладений інтерфейс звертається через `Outer.Inner`, тобто `UserRepository.Listener`.

2. **Коли nesting доречний**

Nested interface має сенс, якщо:

- contract тісно повʼязаний із зовнішнім типом;
- вкладений тип використовується тільки в межах цього API;
- треба згрупувати callback-и або contract-и;
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

Тут `Callback` логічно належить `DownloadManager`, тому вкладеність виправдана.

3. **Nested interface не є inner**

Вкладений interface не привʼязаний до instance зовнішнього типу і не має доступу до його state:

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` не може напряму читати `id`, бо це окремий type declaration, а не обʼєкт, створений всередині конкретного `Outer` instance.

4. **Наслідування interface — інша річ**

Інтерфейс також може наслідувати інші інтерфейси:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

Це не nested interface, а interface inheritance.

5. **Android contract приклад**

```kotlin
interface ProfileContract {
    interface View {
        fun render(state: ProfileState)
    }

    interface Presenter {
        fun onStart()
        fun onRetryClick()
    }
}
```

Такий стиль часто був у MVP. У сучасному MVVM/MVI частіше роблять окремі `UiState`, `UiEvent`, `UiEffect`, але nested contracts все ще валідні.

6. **Коли не треба вкладати interface**

Не варто вкладати interface, якщо він:

- використовується в багатьох частинах проєкту;
- є самостійним domain contract-ом;
- робить назви занадто довгими;
- перетворює зовнішній interface на штучний namespace.

Погано:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
    interface Navigator
}
```

Краще винести такі абстракції в окремі top-level interfaces.

**Коротко:** інтерфейс у Kotlin може містити інший інтерфейс. Це корисно для callback-ів і contract-ів, які мають сенс тільки в контексті зовнішнього API. Якщо вкладений interface є самостійною абстракцією, краще зробити його top-level.

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
