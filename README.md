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

Kotlin — це статично типізована мова програмування від JetBrains, яка працює
поверх JVM, Android Runtime, JavaScript і Kotlin/Native. У JVM/Android
екосистемі вона компілюється в bytecode, тому може використовувати ті самі
бібліотеки, runtime-модель, build tools і production-інфраструктуру, що й Java.

1. **Що важливо сказати на співбесіді**

- Kotlin не є “заміною синтаксису Java”, а мовою з більш виразною type system:
  null safety, extension functions, data classes, sealed types, корутини, smart
  casts і краща підтримка функціонального стилю.
- Основна ціль Kotlin в Android/backend — зменшити boilerplate, зробити код
  безпечнішим і підвищити читабельність без відриву від Java-екосистеми.
- Код Kotlin і Java може співіснувати в одному проєкті, одному модулі й навіть
  викликати один одного в межах одного call graph.

2. **Як Kotlin взаємодіє з Java**

- Kotlin-класи компілюються у звичайні JVM-класи, які Java може імпортувати й
  викликати.
- Kotlin може напряму використовувати Java-класи, інтерфейси, enum, annotations,
  exceptions, collections і generic API.
- Для кращого Java API Kotlin має спеціальні анотації: `@JvmStatic`,
  `@JvmOverloads`, `@JvmField`, `@JvmName`.
- Java-код бачить Kotlin nullability не завжди ідеально, тому важливо
  використовувати annotations і чіткі контракти на межі Java/Kotlin.

3. **Приклад**

```kotlin
val list = java.util.ArrayList<String>()
list.add("Hello")
```

Це звичайний Java `ArrayList`, який використовується з Kotlin-синтаксисом.
Kotlin не ізолює розробника від Java, а дає більш зручний і безпечний шар над
тією самою платформою.

**Коротко:** Kotlin добре інтегрується з Java, бо компілюється в JVM bytecode і
працює з Java API напряму. Його головна перевага — менше boilerplate, сильніша
безпека типів і краща виразність коду без втрати доступу до Java-екосистеми.

</details>

<details>
<summary>2. Які базові типи даних існують у Kotlin?</summary>

#### Kotlin

У Kotlin базові типи виглядають як звичайні обʼєктні типи, але на JVM компілятор
у багатьох випадках оптимізує їх до Java primitives. Тому розробник пише
безпечний і виразний код, а runtime отримує продуктивне представлення там, де це
можливо.

1. **Основні типи**

- Цілі числа: `Byte`, `Short`, `Int`, `Long`.
- Числа з плаваючою комою: `Float`, `Double`.
- Логічний тип: `Boolean`.
- Символи й рядки: `Char`, `String`.
- Масиви: `Array<T>` і спеціалізовані primitive-масиви: `IntArray`, `LongArray`,
  `BooleanArray` та інші.

2. **Важливі особливості**

- У Kotlin немає неявного розширення числових типів: `Int` не перетворюється в
  `Long` автоматично, треба явно викликати `toLong()`.
- Nullable-версії типів, наприклад `Int?`, можуть boxing-итися, бо мають
  представляти ще й `null`.
- `String` є immutable, як і в Java, але Kotlin додає зручні extension-функції
  та string templates.
- `Any`, `Unit` і `Nothing` не є “базовими типами даних” у вузькому сенсі, але
  важливі для type system: `Any` — корінь non-null типів, `Unit` — відсутність
  значимого результату, `Nothing` — значення, яке ніколи не повертається.

3. **Приклад**

```kotlin
val count: Int = 10
val price: Double = 19.99
val isActive: Boolean = true
val name: String = "Kotlin"

val longCount: Long = count.toLong()
```

На співбесіді важливо не просто перелічити типи, а пояснити модель: Kotlin дає
єдину type system без явного розділення на primitive/reference на рівні мови,
але JVM-компілятор оптимізує представлення під капотом.

**Коротко:** базові типи Kotlin — це числові типи, `Boolean`, `Char`, `String` і
масиви. Вони зручні як обʼєкти на рівні мови, але можуть працювати як primitives
на JVM для продуктивності.

</details>

<details>
<summary>3. У чому різниця між val і var у Kotlin?</summary>

#### Kotlin

`val` і `var` визначають не тип даних, а можливість повторного присвоєння
змінної. Це базова, але дуже важлива відмінність для стилю Kotlin-коду: за
замовчуванням варто обирати `val`, а `var` використовувати тільки там, де
мутація справді є частиною моделі.

1. **Що таке val**

- `val` створює read-only посилання: після ініціалізації змінній не можна
  присвоїти нове значення.
- Це не завжди означає глибоку immutability обʼєкта. Якщо `val` посилається на
  mutable-обʼєкт, сам обʼєкт усе ще може змінювати внутрішній стан.
- `val` робить код простішим для читання, тестування й reasoning, бо зменшує
  кількість місць, де стан може змінитися.

2. **Що таке var**

- `var` дозволяє повторно присвоювати значення змінній.
- Його варто використовувати для локального змінного стану, counters, builders,
  UI state або інших сценаріїв, де зміна значення є очікуваною.
- Надмірне використання `var` ускладнює підтримку коду, особливо в concurrent
  або reactive-сценаріях.

3. **Приклад**

```kotlin
val userName = "Ivan"

var retryCount = 0
retryCount += 1
```

Якщо спробувати змінити `userName`, код не скомпілюється:

```kotlin
// userName = "Petro" // Val cannot be reassigned
```

4. **Важливий нюанс**

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro")
```

Тут `users` не можна переприсвоїти на інший список, але сам список mutable, тому
його вміст можна змінювати. Для справжньої незмінності краще використовувати
read-only інтерфейси (`List`) або immutable-підходи на рівні архітектури.

**Коротко:** `val` — read-only reference, `var` — змінна з можливістю повторного
присвоєння. У production-коді правило просте: починати з `val`, переходити до
`var` тільки коли мутація явно потрібна.

</details>

<details>
<summary>4. Що таке виведення типів (type inference) у Kotlin?</summary>

#### Kotlin

Type inference — це здатність компілятора Kotlin самостійно визначати тип
виразу, змінної, lambda-параметра або generic-результату з контексту. Це одна з
причин, чому Kotlin-код зазвичай коротший за Java-код, але при цьому залишається
статично типізованим.

1. **Як це працює**

- Компілятор аналізує праву частину присвоєння, сигнатуру функції, generic-типи
  й очікуваний тип у конкретному контексті.
- Якщо тип однозначний, його можна не писати явно.
- Якщо контексту недостатньо або є неоднозначність, компілятор попросить вказати
  тип вручну.

2. **Приклади**

```kotlin
val name = "Kotlin"      // String
val count = 10           // Int
val price = 19.99        // Double
val names = listOf("A")  // List<String>
```

У lambda-виразах типи часто беруться з API, яке приймає lambda:

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // it: Int
```

3. **Коли краще писати тип явно**

- Для public API: return type функцій, властивості в інтерфейсах, DTO/contract
  boundaries.
- У складних generic-виразах, де явний тип покращує читабельність.
- Коли тип має бути ширшим за фактичну реалізацію, наприклад `List<User>`
  замість `MutableList<User>`.
- Коли inferred type може випадково “зацементувати” implementation detail.

4. **Типова помилка**

```kotlin
val users = mutableListOf<User>()
```

Тут inferred type буде `MutableList<User>`. Якщо змінна має бути read-only для
користувача цього коду, краще написати явно:

```kotlin
val users: List<User> = mutableListOf()
```

**Коротко:** type inference прибирає зайвий синтаксис, але не прибирає статичну
типізацію. Хороший Kotlin-код використовує inference для локальної простоти, але
явно фіксує типи там, де це частина API, архітектурного контракту або
читабельності.

</details>

<details>
<summary>5. Що таке функції-розширення (extension functions)?</summary>

#### Kotlin

Функції-розширення дозволяють додати нову функцію до вже існуючого типу без
наслідування, wrapper-класів або зміни вихідного коду цього типу. Це один із
ключових механізмів Kotlin для читабельного API, DSL і зменшення utility-класів.

1. **Як це виглядає**

```kotlin
fun String.lastChar(): Char = this[this.length - 1]

val result = "Kotlin".lastChar()
```

`String` тут називається receiver type, а `this` всередині функції посилається
на конкретний обʼєкт, для якого викликали extension.

2. **Що важливо розуміти**

- Extension function не змінює реальний клас і не додає метод у bytecode класу
  receiver.
- На JVM це компілюється приблизно як static-функція, де receiver передається
  першим аргументом.
- Extension не має доступу до private/protected членів receiver-класу.
- Якщо в класі вже є member function з такою самою сигнатурою, member function
  має пріоритет над extension.

3. **Статична диспетчеризація**

Extension functions dispatch-яться статично, за compile-time типом змінної, а не
за runtime типом обʼєкта:

```kotlin
open class Animal
class Dog : Animal()

fun Animal.label() = "animal"
fun Dog.label() = "dog"

val animal: Animal = Dog()
println(animal.label()) // animal
```

Це принципово важливо для співбесіди: extension functions не є polymorphic
override-механізмом.

4. **Коли використовувати**

- Для domain-specific helper API, який природно читається як поведінка типу.
- Для mapping, formatting, validation, conversion.
- Для побудови DSL, наприклад у Gradle, Compose, routing/configuration API.
- Для винесення повторюваної логіки без створення великих `Utils` класів.

5. **Коли не варто використовувати**

- Коли extension приховує складну бізнес-логіку або side effects.
- Коли функція не має природного звʼязку з receiver-типом.
- Коли extension створює конфлікти імен або робить API менш очевидним.
- Коли потрібен runtime polymorphism — тоді краще interface/abstract class.

**Коротко:** extension functions — це синтаксично зручний спосіб додати поведінку до
типу без зміни самого типу. Вони покращують читабельність, але не замінюють
наслідування чи polymorphism, бо dispatch-яться статично.

</details>

<details>
<summary>6. Що таке data class у Kotlin?</summary>

#### Kotlin

`data class` — це клас, основна роль якого полягає у зберіганні даних. Kotlin
автоматично генерує для нього набір стандартних методів, які в Java зазвичай
доводиться писати вручну або генерувати через IDE/Lombok.

1. **Що генерує компілятор**

Для `data class` Kotlin автоматично створює:

- `equals()` — порівняння за значеннями властивостей.
- `hashCode()` — коректний hash для використання в `HashMap`, `HashSet`.
- `toString()` — читабельне представлення обʼєкта.
- `copy()` — створення копії з частковою зміною полів.
- `componentN()` — функції для destructuring declarations.

2. **Приклад**

```kotlin
data class User(
    val id: Long,
    val name: String,
    val email: String
)

val user = User(1, "Ivan", "ivan@example.com")
val renamed = user.copy(name = "Petro")
```

`copy()` особливо корисний для immutable-моделей, UI state, DTO і reducer-style
оновлень стану.

3. **Важливі правила**

- У primary constructor має бути хоча б один параметр.
- Параметри primary constructor мають бути позначені як `val` або `var`.
- `data class` не може бути `open`, `sealed`, `inner` або `abstract`.
- Компілятор бере до `equals/hashCode/toString/copy/componentN` тільки
  властивості з primary constructor.

4. **Нюанс із властивостями в тілі класу**

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

`lastLoginAt` не бере участі в `equals()`, `hashCode()`, `copy()` і
destructuring, бо оголошений не в primary constructor. Це може бути джерелом
тонких багів, якщо команда очікує, що всі поля є частиною identity/value state.

5. **Коли використовувати**

- DTO, API models, database projections.
- Immutable UI state в Android/Compose.
- Value objects у domain layer, якщо поведінка мінімальна.
- Результати мапінгу між шарами застосунку.

**Коротко:** `data class` — це Kotlin-механізм для value-like моделей даних із
автоматично згенерованими `equals`, `hashCode`, `toString`, `copy` і
`componentN`. Його варто використовувати для даних, але не перетворювати на
місце для складної бізнес-логіки.

</details>

<details>
<summary>7. Що таке companion object?</summary>

#### Kotlin

`companion object` — це обʼєкт, оголошений всередині класу, члени якого можна
викликати через імʼя класу. У Kotlin немає `static` members у Java-сенсі, тому
`companion object` є основним способом описати factory methods, константи,
shared helpers або Java-friendly API, повʼязаний із конкретним класом.

1. **Базовий приклад**

```kotlin
class User private constructor(val name: String) {
    companion object {
        fun create(name: String): User {
            return User(name.trim())
        }
    }
}

val user = User.create(" Ivan ")
```

Ззовні це виглядає схоже на static method, але технічно `companion object` — це
singleton-обʼєкт, який має власний тип і може реалізовувати інтерфейси.

2. **Що важливо знати**

- У класі може бути тільки один `companion object`.
- Він може мати імʼя або бути anonymous: `companion object Factory`.
- Його члени викликаються як `ClassName.member`, хоча під капотом це члени
  вкладеного singleton-обʼєкта.
- Він має доступ до private members зовнішнього класу.
- Для Java-викликів іноді потрібні `@JvmStatic` або `@JvmField`.

3. **Java interop**

```kotlin
class Config {
    companion object {
        const val DEFAULT_TIMEOUT = 30

        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Без `@JvmStatic` Java викликатиме метод через `Config.Companion.default()`. З
`@JvmStatic` Java може викликати `Config.default()`, що часто зручніше для
публічного API або legacy Java-коду.

4. **Типові сценарії**

- Factory methods: `User.create(...)`, `Result.success(...)`.
- Константи, тісно повʼязані з класом.
- Parsers/converters: `Money.fromCents(...)`.
- Реалізація interface-based factory.
- Акуратна заміна Java static helpers, коли helper логічно належить класу.

5. **Коли бути обережним**

- Не варто складати в companion object глобальний mutable state.
- Не треба перетворювати companion object на “utility dump”.
- Якщо логіка має залежності, lifecycle або state, краще винести її в окремий
  сервіс і передавати через dependency injection.

**Коротко:** `companion object` — це singleton-обʼєкт, привʼязаний до класу, який
дає Kotlin-альтернативу static members. Його варто використовувати для
factory-методів, констант і API, що природно належить класу, але не для
прихованого глобального стану.

</details>

<details>
<summary>8. Як створюються класи у Kotlin?</summary>

#### Kotlin

Класи в Kotlin створюються ключовим словом `class`. На відміну від Java, Kotlin
дозволяє дуже компактно оголошувати constructor parameters, властивості,
ініціалізацію, default values і visibility прямо в заголовку класу.

1. **Базове оголошення**

```kotlin
class User
```

Це порожній клас без явно оголошених властивостей. Якщо тіло класу порожнє,
фігурні дужки можна не писати.

2. **Клас із primary constructor**

```kotlin
class User(
    val id: Long,
    var name: String
)
```

Тут `id` і `name` одночасно є параметрами конструктора і властивостями класу.
`val` створює read-only властивість, `var` — mutable властивість.

3. **Клас із тілом**

```kotlin
class User(
    val id: Long,
    var name: String
) {
    fun rename(newName: String) {
        name = newName.trim()
    }
}
```

У тілі класу оголошують методи, додаткові властивості, `init`-блоки, secondary
constructors, nested classes, companion object та інші члени.

4. **Ініціалізація**

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank()) { "Name must not be blank" }
    }
}
```

`init`-блок виконується під час створення обʼєкта після обробки параметрів
primary constructor. Це правильне місце для валідації інваріантів обʼєкта.

5. **Visibility і наслідування**

```kotlin
open class BaseUser protected constructor(
    val id: Long
)

class AdminUser(id: Long) : BaseUser(id)
```

За замовчуванням класи в Kotlin `final`, тобто їх не можна наслідувати. Щоб
дозволити наслідування, треба явно написати `open`. Це свідомий дизайн Kotlin:
спочатку композиція й стабільні API, а наслідування — тільки коли воно справді
потрібне.

6. **Практичний підхід**

- Для простих моделей даних використовують `data class`.
- Для сервісів і бізнес-логіки — звичайні `class`.
- Для singleton — `object`.
- Для обмежених ієрархій станів — `sealed class` або `sealed interface`.
- Для контрактів — `interface`.

**Коротко:** клас у Kotlin оголошується через `class`, часто з primary constructor
прямо в заголовку. Kotlin заохочує компактні моделі, явні властивості, валідацію
інваріантів через `init` і обережне використання наслідування через `open`.

</details>

<details>
<summary>9. Поясніть первинні та вторинні конструктори.</summary>

#### Kotlin

У Kotlin є primary constructor і secondary constructors. Primary constructor —
це основний спосіб створення обʼєкта, він оголошується в заголовку класу.
Secondary constructors — додаткові конструктори в тілі класу, які мають
делегувати створення до primary constructor або іншого secondary constructor.

1. **Primary constructor**

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Тут `id` і `name` одночасно є параметрами конструктора і властивостями класу. Це
найтиповіший Kotlin-стиль: коротко, явно і без boilerplate.

2. **init-блок**

Primary constructor не має власного тіла. Для логіки ініціалізації
використовують `init`:

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank()) { "Name must not be blank" }
    }
}
```

`init` — правильне місце для перевірки інваріантів, які мають бути істинними для
кожного створеного обʼєкта.

3. **Secondary constructor**

```kotlin
class User(
    val id: Long,
    val name: String
) {
    constructor(name: String) : this(
        id = 0L,
        name = name
    )
}
```

Secondary constructor оголошується через `constructor` і в цьому прикладі
делегує виклик primary constructor через `: this(...)`.

4. **Порядок ініціалізації**

Під час створення обʼєкта порядок такий:

- Викликається primary constructor.
- Ініціалізуються властивості та `init`-блоки в порядку оголошення в тілі класу.
- Виконується тіло secondary constructor, якщо обʼєкт створювали через нього.

Це важливо, бо логіка в `init` виконається до тіла secondary constructor.

5. **Коли використовувати secondary constructors**

- Для сумісності з Java API або framework requirements.
- Для альтернативних способів створення, які не зручно виразити default
  parameters.
- Для Android/View-класів, де потрібні constructor overloads з `Context`,
  `AttributeSet` тощо.

У звичайному Kotlin-коді часто краще використовувати default parameters або
factory methods у `companion object`, а не плодити багато secondary
constructors.

6. **Практичне правило**

```kotlin
class User(
    val id: Long = 0L,
    val name: String
)
```

Якщо різниця між конструкторами зводиться до дефолтних значень, default
parameters зазвичай простіші й читабельніші.

**Коротко:** primary constructor — основний і найчастіший спосіб створення класів у
Kotlin. Secondary constructors потрібні рідше: для interoperability, framework
вимог або справді альтернативних сценаріїв створення обʼєкта.

</details>

<details>
<summary>10. Як працює наслідування у Kotlin?</summary>

#### Kotlin

Наслідування в Kotlin працює через класи, інтерфейси й перевизначення членів,
але з важливою відмінністю від Java: класи та методи за замовчуванням `final`.
Щоб дозволити наслідування або override, потрібно явно написати `open`.

1. **Базове наслідування**

```kotlin
open class Animal(
    val name: String
) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

`Animal` має бути `open`, і метод `speak()` також має бути `open`, інакше `Dog`
не зможе його перевизначити.

2. **Чому все final за замовчуванням**

- Це зменшує випадкове наслідування там, де клас не проєктувався як base class.
- API стає стабільнішим і безпечнішим для змін.
- Легше підтримувати інваріанти класу.
- Це заохочує композицію замість глибоких ієрархій.

На рівні техліда це важливий сигнал: наслідування має бути свідомим дизайном, а
не побічним ефектом відкритості класів.

3. **Перевизначення властивостей**

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

У Kotlin можна перевизначати не тільки методи, а й властивості, якщо вони
позначені як `open`.

4. **Виклик реалізації батьківського класу**

```kotlin
open class BaseRepository {
    open fun refresh() {
        println("Base refresh")
    }
}

class UserRepository : BaseRepository() {
    override fun refresh() {
        super.refresh()
        println("User refresh")
    }
}
```

`super` використовується для звернення до реалізації базового класу.

5. **Абстрактні класи й інтерфейси**

```kotlin
abstract class UseCase {
    abstract fun execute()
}

interface Logger {
    fun log(message: String)
}
```

`abstract` класи не треба позначати `open`: вони вже призначені для
наслідування. Інтерфейси в Kotlin можуть містити як абстрактні методи, так і
методи з default implementation.

6. **Практичне правило**

- Для shared contract зазвичай краще `interface`.
- Для спільного стану або часткової реалізації — `abstract class`.
- Для обмеженої ієрархії станів — `sealed class` або `sealed interface`.
- Для повторного використання поведінки без жорсткої ієрархії часто краще
  композиція або delegation.

**Коротко:** наслідування в Kotlin явне й контрольоване. Клас, метод або властивість
треба відкрити через `open`, а перевизначення позначити `override`. Це робить
ієрархії більш свідомими й зменшує ризик випадкового розширення API.

</details>

<details>
<summary>11. Як реалізувати singleton у Kotlin?</summary>

#### Kotlin

У Kotlin singleton найчастіше реалізується через `object declaration`. Це
найпростіший і idiomatic спосіб створити один екземпляр класу, який
ініціалізується ліниво й потокобезпечно на рівні JVM.

1. **Object declaration**

```kotlin
object AppConfig {
    val apiUrl: String = "https://api.example.com"

    fun isDebug(): Boolean = false
}

val url = AppConfig.apiUrl
```

`AppConfig` має рівно один екземпляр. Його не потрібно створювати через
constructor, і до нього звертаються напряму через імʼя обʼєкта.

2. **Що важливо знати**

- `object` ініціалізується ліниво: при першому зверненні.
- Ініціалізація потокобезпечна.
- `object` може наслідувати класи й реалізовувати інтерфейси.
- У нього можуть бути властивості, функції, `init`-блоки.
- Він не має public constructor.

3. **Singleton як companion object**

```kotlin
class UserRepository private constructor() {
    companion object {
        val instance: UserRepository by lazy {
            UserRepository()
        }
    }
}
```

Такий підхід можливий, але в Kotlin часто зайвий. Якщо потрібен саме глобальний
singleton без параметрів, `object UserRepository` буде простішим.

4. **Lazy singleton з параметрами**

Якщо обʼєкту потрібні runtime-залежності, глобальний `object` зазвичай поганий
варіант:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

У такому випадку краще створювати singleton через dependency injection
container, наприклад Hilt/Koin/Dagger, де lifecycle і залежності контролюються
явно.

5. **Коли використовувати object**

- Stateless utility або mapper, якщо він справді не має залежностей.
- Глобальні константи, повʼязані з конкретною областю.
- Strategy/Factory без runtime state.
- Sentinel objects або sealed hierarchy cases.

6. **Коли бути обережним**

- Mutable state в `object` стає глобальним станом і ускладнює тести.
- Singleton із прихованими залежностями порушує dependency inversion.
- В Android singleton може випадково тримати `Context`, `Activity` або `View` і
  створити memory leak.
- Для бізнес-сервісів у production-коді краще DI scope, а не ручний singleton.

**Коротко:** idiomatic singleton у Kotlin — це `object`. Він простий, лінивий і
потокобезпечний, але його треба використовувати обережно: для stateless або
справді глобальних речей, а залежні сервіси краще керувати через DI.

</details>

<details>
<summary>12. Для чого використовується тип Unit?</summary>

#### Kotlin

`Unit` у Kotlin означає, що функція не повертає значимого результату. Найближча
аналогія з Java — `void`, але в Kotlin `Unit` є справжнім типом з єдиним
значенням `Unit`, тому він краще вписується в загальну type system.

1. **Базове використання**

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

Якщо функція повертає `Unit`, його зазвичай не пишуть явно:

```kotlin
fun log(message: String) {
    println(message)
}
```

Обидва варіанти еквівалентні.

2. **Чим Unit відрізняється від void**

- `Unit` — це тип, а `void` у Java — відсутність return type.
- `Unit` має значення `Unit`.
- `Unit` можна використовувати в generics і function types.
- Компілятор може неявно повернути `Unit` з функції, якщо немає іншого
  результату.

3. **Unit у function types**

```kotlin
val onClick: () -> Unit = {
    println("Clicked")
}
```

Це дуже поширено в Android, Compose, callback API і higher-order functions. Тип
`() -> Unit` означає “функція без параметрів, яка виконує дію і не повертає
значимого результату”.

4. **Unit у generics**

```kotlin
interface CommandHandler<T> {
    fun handle(command: T): Unit
}
```

Оскільки `Unit` є типом, його можна використовувати там, де Java `void`
використати не може. Це робить generic API більш однорідними.

5. **Коли писати Unit явно**

- У function type: `() -> Unit`.
- У generic-контрактах, де return type має бути частиною сигнатури.
- Для документації intent, якщо це покращує читабельність API.
- У звичайних функціях явно писати `: Unit` найчастіше не потрібно.

6. **Порівняння з Nothing**

`Unit` означає “функція завершилась, але не повернула значимого значення”.
`Nothing` означає “функція ніколи нормально не завершується”, наприклад завжди
кидає exception або має нескінченний цикл.

```kotlin
fun fail(message: String): Nothing {
    throw IllegalStateException(message)
}
```

**Коротко:** `Unit` — це Kotlin-тип для функцій без значимого результату. Він схожий
на Java `void`, але є повноцінним типом, тому природно працює з lambdas,
callbacks, generics і Kotlin type system.

</details>

<details>
<summary>13. Що таке smart cast?</summary>

#### Kotlin

Smart cast — це механізм Kotlin, коли компілятор автоматично приводить змінну до
більш конкретного типу після безпечної перевірки. Завдяки цьому не потрібно
писати явний cast там, де компілятор може довести, що значення вже має потрібний
тип або точно не є `null`.

1. **Smart cast після перевірки типу**

```kotlin
fun printLength(value: Any) {
    if (value is String) {
        println(value.length)
    }
}
```

Після `value is String` компілятор знає, що всередині `if` змінна `value` має
тип `String`, тому дозволяє викликати `length` без `as String`.

2. **Smart cast після null-check**

```kotlin
fun printName(name: String?) {
    if (name != null) {
        println(name.uppercase())
    }
}
```

Усередині блоку `if` тип `name` smart-cast-иться з `String?` до `String`. Це
одна з причин, чому null safety у Kotlin зручна без зайвого boilerplate.

3. **Smart cast із early return**

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length)
}
```

Після `return` компілятор розуміє, що нижче по коду `value` вже точно `String`.
Такий стиль часто робить код простішим і менш вкладеним.

4. **Коли smart cast не спрацює**

Smart cast можливий тільки якщо компілятор впевнений, що значення не змінилось
між перевіркою і використанням.

- Для mutable `var`-властивостей smart cast часто неможливий.
- Для open/custom getter властивостей компілятор не гарантує стабільне значення.
- Для значень, які можуть бути змінені з іншого потоку або через side effects,
  компілятор також буде обережним.

```kotlin
class UserHolder {
    var name: String? = null

    fun printName() {
        if (name != null) {
            // println(name.length) // smart cast may be impossible
        }
    }
}
```

У таких випадках краще зберегти значення в локальну `val`:

```kotlin
val currentName = name
if (currentName != null) {
    println(currentName.length)
}
```

5. **Звʼязок із when**

```kotlin
fun render(value: Any) {
    when (value) {
        is String -> println(value.uppercase())
        is Int -> println(value + 1)
    }
}
```

У кожній гілці `when` Kotlin smart-cast-ить `value` до відповідного типу.
Особливо добре це працює з `sealed class` і `sealed interface`, де компілятор
може перевіряти exhaustive handling.

**Коротко:** smart cast — це автоматичне безпечне приведення типу після перевірки
`is`, `!is` або null-check. Воно робить Kotlin-код коротшим і безпечнішим, але
працює тільки тоді, коли компілятор може гарантувати стабільність значення.

</details>

<details>
<summary>14. Як Kotlin забезпечує null safety?</summary>

#### Kotlin

Kotlin забезпечує null safety через type system: тип, який може містити `null`,
і тип, який не може містити `null`, — це різні типи. Завдяки цьому велика
частина `NullPointerException` переноситься з runtime у compile-time.

1. **Non-null і nullable типи**

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

`String` не може бути `null`, а `String?` може. Якщо змінна nullable, Kotlin не
дозволить напряму викликати методи без перевірки:

```kotlin
val length = optionalName?.length
```

2. **Safe call operator**

```kotlin
val length: Int? = optionalName?.length
```

`?.` викликає метод або властивість тільки якщо значення не `null`. Якщо
значення `null`, результат усього виразу теж буде `null`.

3. **Elvis operator**

```kotlin
val length: Int = optionalName?.length ?: 0
```

`?:` дозволяє задати fallback, якщо ліва частина дорівнює `null`. У production
коді це часто краще, ніж агресивно використовувати `!!`.

4. **Smart cast після null-check**

```kotlin
fun printName(name: String?) {
    if (name != null) {
        println(name.uppercase())
    }
}
```

Після перевірки `name != null` компілятор smart-cast-ить `name` до `String`
всередині блоку.

5. **Not-null assertion**

```kotlin
val length = optionalName!!.length
```

`!!` примусово каже компілятору: “я гарантую, що тут не null”. Якщо значення
насправді `null`, буде `NullPointerException`. Це escape hatch, а не нормальний
інструмент щоденного коду.

6. **Межа з Java**

Найбільший ризик null safety у Kotlin — Java interop. Java-типи можуть
потрапляти в Kotlin як platform types, наприклад `String!`, де компілятор не
знає точно, чи значення nullable.

```kotlin
val name = javaApi.getName()
```

Для таких API важливо:

- використовувати nullability annotations у Java;
- перевіряти значення на межі Java/Kotlin;
- не тягнути platform types глибоко в domain layer;
- нормалізувати API через wrapper або mapper.

7. **Практичний підхід**

- Моделювати nullable тільки там, де `null` є валідним станом домену.
- Не використовувати `String?` як заміну помилкам, якщо потрібен явний result
  або exception.
- Уникати `!!` у production-коді, крім дуже контрольованих місць.
- В Android уважно працювати з lifecycle-driven nullable references.

**Коротко:** Kotlin null safety базується на розділенні `T` і `T?`, safe calls,
Elvis operator, smart casts і compile-time перевірках. Це не магія, а строгий
контракт типів; слабке місце зазвичай на межі з Java, reflection або неакуратним
використанням `!!`.

</details>

<details>
<summary>15. Що таке Elvis-оператор (?:)?</summary>

#### Kotlin

Elvis-оператор `?:` — це оператор для роботи з nullable-значеннями. Він повертає
ліву частину, якщо вона не `null`, або праву частину, якщо ліва частина дорівнює
`null`.

1. **Базовий приклад**

```kotlin
val name: String? = null
val displayName: String = name ?: "Unknown"
```

Якщо `name` не `null`, `displayName` отримає значення `name`. Якщо `name` —
`null`, буде використано `"Unknown"`.

2. **Разом із safe call**

```kotlin
val userNameLength: Int = user?.name?.length ?: 0
```

Тут ланцюжок `user?.name?.length` може повернути `null` на будь-якому етапі.
Elvis-оператор дає безпечне fallback-значення.

3. **return у правій частині**

У Kotlin `return` є expression, тому його можна використовувати справа від
Elvis-оператора:

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Це зручний спосіб early return без зайвої вкладеності.

4. **throw у правій частині**

`throw` також є expression:

```kotlin
fun requireName(name: String?): String {
    return name ?: throw IllegalArgumentException("Name is required")
}
```

Такий підхід добре підходить для валідації обовʼязкових параметрів на межі API.

5. **Тип результату**

Тип Elvis-виразу визначається з урахуванням обох гілок:

```kotlin
val length: Int = name?.length ?: 0
```

Ліва частина має тип `Int?`, права — `Int`, а результат усього виразу — `Int`,
бо fallback прибирає nullable.

6. **Коли використовувати**

- Для fallback-значень.
- Для early return у nullable flow.
- Для fail-fast валідації через `throw`.
- Для зменшення вкладених `if (x != null)` конструкцій.

7. **Коли бути обережним**

- Не варто приховувати важливу помилку дефолтним значенням, якщо `null` означає
  некоректний стан.
- Не треба робити довгі Elvis-ланцюжки, які складно читати.
- Якщо fallback має побічні ефекти, краще винести логіку в окремий блок.

**Коротко:** Elvis-оператор `?:` — це компактний спосіб сказати: “використай це
значення, якщо воно не null, інакше використай fallback”. У Kotlin він особливо
сильний, бо справа можуть бути не тільки значення, а й `return` або `throw`.

</details>

<details>
<summary>16. Що робить оператор !! і чому він небезпечний?</summary>

#### Kotlin

Оператор `!!` називається not-null assertion operator. Він примусово перетворює
nullable-тип `T?` у non-null тип `T`. Якщо значення в момент виконання дорівнює
`null`, програма впаде з `NullPointerException`.

1. **Базовий приклад**

```kotlin
val name: String? = null
val length: Int = name!!.length
```

Цей код скомпілюється, але впаде в runtime, бо `name` насправді `null`.

2. **Що саме робить !!**

```kotlin
val value: String = nullableValue!!
```

Цим оператором розробник фактично каже компілятору: “я беру відповідальність, що
тут точно не `null`”. Компілятор перестає вимагати null-check, але runtime
перевірка все одно залишається.

3. **Чому він небезпечний**

- Повертає ризик `NullPointerException`, від якого Kotlin якраз намагається
  захистити.
- Приховує проблему моделювання: якщо значення може бути `null`, це треба явно
  обробити.
- Ускладнює підтримку, бо майбутня зміна flow може зробити `!!` небезпечним.
- В Android часто призводить до crash через lifecycle: `Fragment`, `View`,
  arguments, binding, async callbacks.

4. **Кращі альтернативи**

Safe call:

```kotlin
val length: Int? = name?.length
```

Elvis fallback:

```kotlin
val length: Int = name?.length ?: 0
```

Early return:

```kotlin
val value = name ?: return
println(value.length)
```

Fail-fast з поясненням:

```kotlin
val value = requireNotNull(name) { "Name is required" }
```

`requireNotNull` або `checkNotNull` часто краще за `!!`, бо crash має зрозуміле
повідомлення й показує, який саме контракт було порушено.

5. **Коли !! може бути допустимим**

- У дуже локальному коді, де non-null гарантія очевидна й недовговічна.
- У тестах, коли падіння є прийнятним і спрощує setup.
- Після framework callback/contract, який компілятор не може довести, але
  команда чітко контролює інваріант.

Навіть у цих випадках краще подумати, чи `requireNotNull`, smart cast або зміна
моделі типів не дадуть чистіший результат.

6. **Техлід-позиція**

У production-коді `!!` має бути винятком, а не звичкою. Якщо в коді багато `!!`,
це сигнал, що команда неправильно моделює nullable state, погано обробляє
framework lifecycle або занадто довіряє зовнішнім API.

**Коротко:** `!!` вимикає compile-time null safety для конкретного значення і може
кинути `NullPointerException`. Його варто уникати, замінюючи на safe calls,
Elvis, early return, `requireNotNull` або кращу модель типів.

</details>

<details>
<summary>17. Як працюють параметри за замовчуванням і іменовані параметри?</summary>

#### Kotlin

Параметри за замовчуванням дозволяють задати default value прямо в сигнатурі
функції або конструктора. Іменовані параметри дозволяють передавати аргументи за
назвою, а не тільки за позицією. Разом вони сильно зменшують кількість overloads
і роблять виклики читабельнішими.

1. **Параметри за замовчуванням**

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) {
    // ...
}

createUser("Ivan")
createUser("Ivan", false)
```

Якщо аргумент не передали, Kotlin використовує значення за замовчуванням. Це
часто замінює кілька перевантажених методів з Java.

2. **Іменовані параметри**

```kotlin
createUser(
    name = "Ivan",
    role = "admin"
)
```

Іменовані аргументи дозволяють пропускати параметри з default values і явно
показують, що саме передається. Це особливо корисно, коли в функції кілька
параметрів одного типу.

3. **Проблема positional arguments**

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect("api.example.com", 443, true, false)
```

Такий виклик читається погано: важко зрозуміти, що означають `true` і `false`.
Краще:

```kotlin
connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

4. **Конструктори**

```kotlin
data class User(
    val id: Long = 0L,
    val name: String,
    val isActive: Boolean = true
)

val user = User(
    name = "Ivan"
)
```

Це робить моделі компактними, але важливо не зловживати default values там, де
значення є обовʼязковим доменним інваріантом.

5. **Java interop**

Java не розуміє Kotlin default parameters напряму. Для Java-коду Kotlin генерує
основну сигнатуру, а default values обробляються спеціальним synthetic
механізмом. Якщо потрібні overloads для Java, використовують `@JvmOverloads`:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Тоді Java зможе викликати кілька згенерованих overloads.

6. **Практичні правила**

- Використовувати default parameters замість великої кількості overloads.
- Для boolean flags у викликах часто використовувати named arguments.
- Не робити занадто довгі сигнатури: якщо параметрів багато, краще винести
  конфігурацію в окремий object/data class.
- Не покладатися на named arguments для Java API: вони працюють тільки на
  Kotlin-рівні.

**Коротко:** default parameters дають значення за замовчуванням у сигнатурі, а named
parameters роблять виклики явними й читабельними. Це idiomatic Kotlin, але на
межі з Java треба памʼятати про `@JvmOverloads` і відмінності interop.

</details>

<details>
<summary>18. Що таке destructuring declarations?</summary>

#### Kotlin

Destructuring declarations — це синтаксис Kotlin, який дозволяє “розкласти”
обʼєкт на кілька локальних змінних через `componentN()` функції. Найчастіше це
використовується з `data class`, `Pair`, `Triple`, `Map.Entry` і результатами,
які природно складаються з кількох частин.

1. **Базовий приклад**

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(1L, "Ivan")
val (id, name) = user
```

Компілятор перетворює це приблизно на:

```kotlin
val id = user.component1()
val name = user.component2()
```

Для `data class` функції `componentN()` генеруються автоматично для властивостей
primary constructor у порядку їх оголошення.

2. **Destructuring у циклах**

```kotlin
val users = listOf(
    User(1L, "Ivan"),
    User(2L, "Petro")
)

for ((id, name) in users) {
    println("$id: $name")
}
```

Це робить ітерацію читабельною, якщо структура обʼєкта проста й очевидна.

3. **Map destructuring**

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Для `Map.Entry` Kotlin має `component1()` для key і `component2()` для value.

4. **Ігнорування непотрібних значень**

```kotlin
val (_, name) = user
```

Underscore `_` означає, що значення не потрібне. Компілятор не створює локальну
змінну для цього компонента.

5. **Custom componentN**

```kotlin
class Point(val x: Int, val y: Int) {
    operator fun component1(): Int = x
    operator fun component2(): Int = y
}

val (x, y) = Point(10, 20)
```

Щоб destructuring працював для звичайного класу, `componentN()` мають бути
позначені як `operator`.

6. **Коли бути обережним**

- Destructuring залежить від порядку `componentN()`, тому зміна порядку полів у
  `data class` може змінити сенс коду.
- Для великих обʼєктів destructuring може погіршити читабельність.
- У public API не варто змушувати читача здогадуватись, що означають
  `component1`, `component2`.
- `Pair` і `Triple` з destructuring зручні локально, але для доменної моделі
  краще названий `data class`.

**Коротко:** destructuring declarations дозволяють розкласти обʼєкт на кілька
змінних через `componentN()`. Це зручно для простих value objects, `Map.Entry` і
локального коду, але в доменній логіці важливо не втрачати читабельність і
семантику назв.

</details>

<details>
<summary>19. Що таке type alias і коли його використовувати?</summary>

#### Kotlin

`typealias` у Kotlin створює альтернативну назву для вже існуючого типу. Він не
створює новий тип на рівні type system, а лише робить складні або доменно
важливі типи коротшими й читабельнішими.

1. **Базовий приклад**

```kotlin
typealias UserId = Long

fun loadUser(id: UserId) {
    // ...
}
```

`UserId` тут є лише інша назва для `Long`. Значення типу `Long` можна передати
туди, де очікується `UserId`, і навпаки.

2. **Для складних generic-типів**

```kotlin
typealias UserCache = Map<UserId, User>
typealias Callback<T> = (Result<T>) -> Unit
```

Це зменшує шум у сигнатурах і робить intent зрозумілішим.

3. **Для function types**

```kotlin
typealias OnUserClick = (User) -> Unit

fun UserList(
    onUserClick: OnUserClick
) {
    // ...
}
```

Такий підхід часто корисний у UI-коді, callbacks, Compose-параметрах і
event-handling.

4. **Для вкладених або platform-specific типів**

```kotlin
typealias AndroidBundle = android.os.Bundle
```

У Kotlin Multiplatform або великих модулях alias може допомогти ізолювати
platform-specific назви або зробити API акуратнішим.

5. **Важливе обмеження**

`typealias` не дає type safety:

```kotlin
typealias UserId = Long
typealias OrderId = Long

fun loadUser(id: UserId) {}

val orderId: OrderId = 10L
loadUser(orderId) // скомпілюється
```

Оскільки обидва alias — це просто `Long`, компілятор не відрізняє `UserId` від
`OrderId`.

6. **Якщо потрібен справді новий тип**

Для сильнішої доменної типізації краще використати value class:

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

Тепер `UserId` і `OrderId` — різні типи для компілятора.

7. **Коли використовувати**

- Для скорочення довгих generic/function types.
- Для покращення читабельності сигнатур.
- Для локального доменного контексту, де не потрібна сильна type safety.
- Для поступового рефакторингу назв без зміни runtime-поведінки.

8. **Коли не використовувати**

- Якщо alias приховує важливу складність API.
- Якщо потрібна реальна типова безпека між схожими значеннями.
- Якщо назва alias вводить в оману і створює враження нового типу.
- Якщо код стає менш зрозумілим через надмірну кількість локальних alias.

**Коротко:** `typealias` — це псевдонім для існуючого типу, корисний для
читабельності й скорочення складних сигнатур. Він не створює новий тип; якщо
потрібна реальна доменна type safety, краще використовувати `value class`.

</details>

<details>
<summary>20. У чому різниця між vararg і масивом?</summary>

#### Kotlin

`vararg` і масив (`Array<T>`) повʼязані, але це не одне й те саме. Масив — це
конкретний обʼєкт-контейнер із фіксованою довжиною. `vararg` — це синтаксис у
сигнатурі функції, який дозволяє передавати змінну кількість аргументів.

1. **Масив**

```kotlin
fun printNames(names: Array<String>) {
    names.forEach(::println)
}

printNames(arrayOf("Ivan", "Petro"))
```

Якщо параметр має тип `Array<String>`, виклик має передати саме масив. Окремі
аргументи передати не можна.

2. **vararg**

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro", "Oksana")
```

`vararg` дозволяє викликати функцію з будь-якою кількістю аргументів. Усередині
функції `names` доступний як масив.

3. **Передача масиву у vararg**

Якщо вже є масив, його треба “розпакувати” через spread operator `*`:

```kotlin
val names = arrayOf("Ivan", "Petro")

printNames(*names)
```

Без `*` Kotlin спробує передати масив як один аргумент, а не як список окремих
аргументів.

4. **vararg і додаткові параметри**

```kotlin
fun log(
    tag: String,
    vararg messages: String
) {
    messages.forEach { println("[$tag] $it") }
}

log("Auth", "Started", "Success")
```

Якщо після `vararg` є інші параметри, їх зазвичай треба передавати як named
arguments, щоб виклик був однозначним.

```kotlin
fun createUser(
    vararg roles: String,
    isActive: Boolean
) {
    // ...
}

createUser("admin", "editor", isActive = true)
```

5. **Обмеження**

- У функції може бути тільки один `vararg` параметр.
- `vararg` у generic API може мати нюанси з type erasure.
- Spread operator `*array` може створювати копію масиву, тому в hot path треба
  думати про продуктивність.
- Для primitive типів є спеціалізовані масиви: `IntArray`, `LongArray`,
  `BooleanArray`, але `vararg Int` усередині працює як `IntArray`.

6. **Коли що використовувати**

- `vararg` — коли API природно приймає “нуль або більше” аргументів.
- `Array<T>` — коли функція має працювати саме з уже сформованим масивом.
- `List<T>` — часто кращий вибір для domain/business API, бо він читабельніший і
  краще виражає колекцію значень.

**Коротко:** масив — це структура даних, а `vararg` — зручний синтаксис параметра
для змінної кількості аргументів. Усередині функції `vararg` представлений як
масив, а готовий масив у `vararg` передають через spread operator `*`.

</details>

<details>
<summary>21. Як використовуються лямбда-вирази у Kotlin?</summary>

#### Kotlin

Лямбда-вирази в Kotlin — це анонімні функції, які можна передавати як значення,
зберігати у змінних і повертати з інших функцій. Вони є основою для
функціонального стилю, collection operators, callbacks, DSL, coroutines API і
Jetpack Compose.

1. **Базовий синтаксис**

```kotlin
val sum: (Int, Int) -> Int = { a, b ->
    a + b
}

val result = sum(2, 3)
```

Тип `(Int, Int) -> Int` означає: функція приймає два `Int` і повертає `Int`.
Останній вираз у lambda є результатом.

2. **Lambda як аргумент функції**

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { number ->
    number * 2
}
```

`map` приймає lambda, яка описує, як перетворити кожен елемент колекції.

3. **Implicit parameter it**

Якщо lambda має один параметр і його тип зрозумілий з контексту, можна
використовувати `it`:

```kotlin
val evenNumbers = numbers.filter { it % 2 == 0 }
```

`it` зручний для коротких lambdas. Якщо логіка довша або вкладена, краще явно
називати параметр.

4. **Trailing lambda syntax**

Якщо останній параметр функції — lambda, її можна винести за дужки:

```kotlin
button.setOnClickListener {
    println("Clicked")
}
```

Цей синтаксис активно використовується в Kotlin DSL, Android callbacks, Compose
і Gradle.

5. **Замикання**

Lambda може захоплювати змінні з зовнішньої області:

```kotlin
var counter = 0

val increment = {
    counter += 1
}

increment()
```

Це зручно, але з mutable state треба бути обережним, особливо в async,
concurrent або UI lifecycle-сценаріях.

6. **Function references**

Іноді замість lambda можна передати посилання на функцію:

```kotlin
fun printName(name: String) {
    println(name)
}

listOf("Ivan", "Petro").forEach(::printName)
```

Function reference корисний, коли вже є готова функція і не потрібно створювати
обгортку `{ value -> function(value) }`.

7. **Практичне використання**

- Collection operations: `map`, `filter`, `fold`, `flatMap`.
- Event handlers і callbacks.
- Higher-order functions у domain/use-case шарі.
- DSL і builder APIs.
- Compose UI, де UI фактично описується через функції й lambdas.

8. **Коли бути обережним**

- Довгі lambdas погіршують читабельність — краще винести в named function.
- Вкладені lambdas з кількома `it` важко читати.
- Захоплення mutable state може створювати приховані side effects.
- У performance-sensitive коді треба розуміти allocation і роль `inline`.

**Коротко:** lambda в Kotlin — це функція як значення. Вона дозволяє передавати
поведінку в API, писати компактні callbacks і будувати DSL, але має залишатися
читабельною: короткі lambdas — inline, складна логіка — в окрему named function.

</details>

<details>
<summary>22. Як працюють колекції (List, Set, Map) у Kotlin?</summary>

#### Kotlin

Колекції в Kotlin поділяються на read-only інтерфейси (`List`, `Set`, `Map`) і
mutable інтерфейси (`MutableList`, `MutableSet`, `MutableMap`). Це не
обовʼязково означає глибоку immutable-структуру, але на рівні API чітко показує,
чи можна змінювати колекцію через конкретне посилання.

1. **List**

`List` — впорядкована колекція, яка дозволяє дублікати й доступ за індексом.

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")

println(names[0])
```

Якщо потрібна зміна списку:

```kotlin
val mutableNames: MutableList<String> = mutableListOf("Ivan")
mutableNames.add("Petro")
```

2. **Set**

`Set` — колекція унікальних елементів. Дублікати не зберігаються.

```kotlin
val ids: Set<Int> = setOf(1, 2, 2, 3)

println(ids) // [1, 2, 3]
```

Важливо, що унікальність залежить від `equals()` і `hashCode()`. Для
`data class` вони генеруються автоматично за primary constructor властивостями.

3. **Map**

`Map` — колекція пар key-value. Ключі унікальні, значення можуть повторюватися.

```kotlin
val usersById: Map<Long, String> = mapOf(
    1L to "Ivan",
    2L to "Petro"
)

println(usersById[1L])
```

Якщо ключа немає, доступ через `map[key]` повертає `null`.

4. **Read-only vs mutable**

```kotlin
val readOnly: List<String> = listOf("A", "B")
val mutable: MutableList<String> = mutableListOf("A", "B")

mutable.add("C")
```

`List` не має методів `add/remove`, а `MutableList` має. Але read-only не завжди
означає справжню immutable collection. Це лише контракт інтерфейсу.

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

5. **Операції над колекціями**

Kotlin має багатий набір extension-функцій:

```kotlin
val result = users
    .filter { it.isActive }
    .map { it.name }
    .sorted()
```

Більшість таких операцій для `Iterable` створюють проміжні колекції. Якщо треба
ліниву обробку, використовують `Sequence`.

6. **Java interop**

На JVM Kotlin-колекції побудовані поверх Java Collections Framework, але Kotlin
розділяє read-only і mutable інтерфейси на рівні типів. При взаємодії з Java
треба памʼятати:

- Java може змінити колекцію, яку Kotlin бачить як `List`.
- Java nullability і mutability не завжди очевидні Kotlin-компілятору.
- На межі API краще робити defensive copy, якщо потрібен стабільний immutable
  contract.

7. **Практичні правила**

- У public API повертати `List/Set/Map`, якщо зовнішній код не має змінювати
  колекцію.
- Використовувати `MutableList/MutableMap` локально або всередині реалізації.
- Не плутати read-only view з immutable data structure.
- Для великих pipeline-операцій розглядати `Sequence`.

**Коротко:** `List` зберігає порядок і дублікати, `Set` зберігає унікальні елементи,
`Map` зберігає пари key-value. Kotlin додає важливий поділ на read-only і
mutable інтерфейси, що робить API безпечнішим і зрозумілішим.

</details>

<details>
<summary>23. Як ітеруватися по колекціях у Kotlin?</summary>

#### Kotlin

У Kotlin по колекціях можна ітеруватися кількома способами: через `for`, через
extension-функції на кшталт `forEach`, через індекси, `withIndex()`,
destructuring для `Map` і функціональні оператори `map/filter/fold`. Вибір
залежить від того, чи потрібен side effect, трансформація, індекс або early
exit.

1. **Звичайний for-loop**

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Це найпростіший і часто найчитабельніший спосіб, особливо коли потрібні `break`,
`continue` або складніший control flow.

2. **forEach**

```kotlin
names.forEach { name ->
    println(name)
}
```

`forEach` добре підходить для коротких side effects. Але якщо потрібен `break`
або `continue`, звичайний `for` зазвичай кращий.

3. **Ітерація з індексом**

```kotlin
for (index in names.indices) {
    println("$index: ${names[index]}")
}
```

`indices` повертає діапазон валідних індексів для списку.

Часто читабельніше використати `withIndex()`:

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

4. **Ітерація по Map**

```kotlin
val usersById = mapOf(
    1L to "Ivan",
    2L to "Petro"
)

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Тут працює destructuring для `Map.Entry`: `component1()` — key, `component2()` —
value.

5. **Функціональні оператори**

Якщо ціль не просто пройтись, а отримати нову колекцію або результат, краще
використовувати відповідний оператор:

```kotlin
val activeNames = users
    .filter { it.isActive }
    .map { it.name }
```

Для агрегації:

```kotlin
val total = orders.fold(0) { sum, order ->
    sum + order.price
}
```

6. **Early exit**

Якщо треба зупинити ітерацію, звичайний `for` часто найпростіший:

```kotlin
for (user in users) {
    if (user.id == targetId) {
        println(user)
        break
    }
}
```

Але для пошуку краще використати готовий оператор:

```kotlin
val user = users.firstOrNull { it.id == targetId }
```

7. **Практичні правила**

- `for` — для складного control flow, `break`, `continue`.
- `forEach` — для коротких side effects.
- `map/filter` — для трансформацій, а не для side effects.
- `withIndex()` — коли потрібні і значення, і індекс.
- Для великих pipeline-операцій розглядати `asSequence()`, щоб уникнути зайвих
  проміжних колекцій.

**Коротко:** у Kotlin ітерація може бути імперативною (`for`) або функціональною
(`map`, `filter`, `fold`, `forEach`). Технічно можна все зробити через
`forEach`, але хороший Kotlin-код обирає інструмент за intent: side effect,
пошук, трансформація чи агрегація.

</details>

<details>
<summary>24. Що станеться, якщо змінити список, створений через listOf()?</summary>

#### Kotlin

`listOf()` повертає `List<T>` — read-only інтерфейс списку. Через таке посилання
не можна викликати `add`, `remove` або інші mutating operations, бо цих методів
немає в `List`. Тому звичайна спроба змінити список, створений через `listOf()`,
не скомпілюється.

1. **Базовий приклад**

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // compile error
```

`names` має тип `List<String>`, а не `MutableList<String>`, тому Kotlin не дає
змінити список через цей API.

2. **Read-only не завжди immutable**

Важливо: `List` у Kotlin означає read-only view, а не обовʼязково глибоко
immutable структуру.

```kotlin
val mutable = mutableListOf("Ivan")
val readOnly: List<String> = mutable

mutable.add("Petro")

println(readOnly) // [Ivan, Petro]
```

Через `readOnly` змінити список не можна, але якщо хтось має mutable-посилання
на той самий backing list, вміст може змінитися.

3. **Небезпечний cast**

Іноді можна побачити спробу привести `List` до `MutableList`:

```kotlin
val names = listOf("Ivan", "Petro")
val mutableNames = names as MutableList<String>
mutableNames.add("Oksana")
```

Так робити не варто. Залежно від конкретної реалізації списку це може впасти з
`UnsupportedOperationException`, `ClassCastException` або створити крихкий код,
який залежить від внутрішньої реалізації стандартної бібліотеки.

4. **Правильний спосіб змінити список**

Якщо потрібен mutable список, треба створити його явно:

```kotlin
val names = mutableListOf("Ivan", "Petro")
names.add("Oksana")
```

Якщо є read-only список і потрібно отримати змінювану копію:

```kotlin
val names = listOf("Ivan", "Petro")
val mutableNames = names.toMutableList()

mutableNames.add("Oksana")
```

5. **Immutable-style оновлення**

У state management, Compose або reducer-style архітектурі часто краще не
змінювати список на місці, а створювати новий:

```kotlin
val names = listOf("Ivan", "Petro")
val updated = names + "Oksana"
```

Оператор `+` повертає новий список, не змінюючи початковий.

6. **Практичне правило**

- `listOf()` — коли колекція не має змінюватися через поточне посилання.
- `mutableListOf()` — коли потрібні `add/remove/set`.
- `toMutableList()` — коли треба змінити копію read-only списку.
- Для public API краще повертати `List`, а mutable деталі тримати всередині
  реалізації.

**Коротко:** список із `listOf()` не можна змінити через Kotlin `List` API — код не
скомпілюється. Але `List` означає read-only view, не абсолютну immutability,
тому для справді контрольованих змін треба явно використовувати `MutableList`,
копії або immutable-style оновлення.

</details>

<details>
<summary>25. Що станеться при зверненні до індексу поза межами списку?</summary>

#### Kotlin

Якщо звернутися до елемента списку за індексом, якого не існує, Kotlin кине
runtime exception — зазвичай `IndexOutOfBoundsException`. Індекси списку
починаються з `0`, а останній валідний індекс дорівнює `size - 1`.

1. **Базовий приклад**

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Для списку з двох елементів валідні індекси — `0` і `1`. Індекс `2` уже поза
межами.

2. **Оператор []**

```kotlin
val name = names[index]
```

Синтаксис `names[index]` у Kotlin викликає `get(index)`. Якщо індекс
некоректний, буде exception.

3. **Безпечний варіант: getOrNull**

```kotlin
val name: String? = names.getOrNull(2)
```

`getOrNull(index)` повертає елемент, якщо індекс валідний, або `null`, якщо
індекс поза межами списку.

```kotlin
val displayName = names.getOrNull(2) ?: "Unknown"
```

Це хороший варіант, коли відсутність елемента є нормальним сценарієм.

4. **Fallback через getOrElse**

```kotlin
val name = names.getOrElse(2) { "Unknown" }
```

`getOrElse` дозволяє задати fallback-значення через lambda. У lambda доступний
індекс, який не знайшли:

```kotlin
val name = names.getOrElse(2) { index ->
    "Missing item at index $index"
}
```

5. **Перевірка індексу**

```kotlin
if (index in names.indices) {
    println(names[index])
}
```

`indices` повертає діапазон валідних індексів. Це читабельніше, ніж вручну
писати `index >= 0 && index < names.size`.

6. **Практичний підхід**

- Якщо індекс має бути валідним за логікою програми, `list[index]` нормальний:
  exception покаже порушення інваріанта.
- Якщо індекс приходить з UI, API, користувацького вводу або зовнішнього стану,
  краще `getOrNull`, `getOrElse` або явна перевірка.
- Не варто ловити `IndexOutOfBoundsException` як нормальний control flow.
- Для першого/останнього елемента краще використовувати `firstOrNull()` і
  `lastOrNull()`, якщо список може бути порожнім.

7. **Повʼязаний приклад**

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

Ці функції безпечні для порожнього списку й повертають `null`, якщо елемента
немає.

**Коротко:** доступ через `list[index]` поза межами списку кидає
`IndexOutOfBoundsException`. Для безпечного доступу використовують `getOrNull`,
`getOrElse`, перевірку `index in list.indices` або спеціальні функції
`firstOrNull/lastOrNull`.

</details>

<details>
<summary>26. Як працює взаємодія Kotlin з Java (interop)?</summary>

#### Kotlin

Kotlin має дуже тісну взаємодію з Java, бо на JVM компілюється у звичайний
bytecode. Це дозволяє Kotlin-коду викликати Java-класи, а Java-коду — викликати
Kotlin-класи. На практиці це означає, що Kotlin можна поступово впроваджувати в
існуючі Java-проєкти без повного переписування.

1. **Kotlin викликає Java**

```kotlin
val list = java.util.ArrayList<String>()
list.add("Kotlin")
```

Kotlin напряму бачить Java-класи, методи, поля, enum, annotations, exceptions,
generics і collections. Більшість Java API можна використовувати без обгорток.

2. **Java викликає Kotlin**

```kotlin
class UserService {
    fun loadUser(id: Long): String {
        return "User $id"
    }
}
```

З Java це буде звичайний JVM-клас:

```java
UserService service = new UserService();
String user = service.loadUser(1L);
```

3. **Platform types**

Головний ризик interop — nullability. Java не має такої строгої null-safety
моделі, тому Kotlin бачить багато Java-типів як platform types, наприклад
`String!`.

```kotlin
val name = javaUser.getName()
```

Компілятор не завжди знає, чи `name` може бути `null`. Тому на межі з Java
важливо перевіряти nullability або використовувати Java annotations:
`@Nullable`, `@NotNull`.

4. **Collections interop**

Kotlin `List`, `MutableList`, `Map` і `Set` на JVM побудовані поверх Java
Collections Framework. Але Kotlin розділяє read-only і mutable інтерфейси на
рівні типів.

```kotlin
fun process(users: List<String>) {
    // read-only Kotlin API
}
```

Java може передати mutable collection, тому read-only в Kotlin не завжди означає
immutable backing structure.

5. **Jvm-анотації**

Kotlin має анотації для кращого Java API:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Найчастіші:

- `@JvmStatic` — генерує static-like метод для Java.
- `@JvmOverloads` — генерує overloads для default parameters.
- `@JvmField` — відкриває поле без getter/setter.
- `@JvmName` — змінює JVM-імʼя функції або property accessor.

6. **Checked exceptions**

Kotlin не має checked exceptions. Якщо Kotlin-функцію має коректно бачити Java з
`throws`, можна використати `@Throws`:

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String {
    // ...
}
```

7. **SAM conversion**

Kotlin зручно працює з Java functional interfaces:

```kotlin
button.setOnClickListener {
    println("Clicked")
}
```

Java-інтерфейс з одним абстрактним методом можна передати як lambda.

8. **Практичні правила**

- На межі Java/Kotlin явно контролювати nullability.
- Для Java-facing Kotlin API використовувати `@JvmStatic`, `@JvmOverloads`,
  `@JvmField`, `@Throws`, якщо це покращує виклик із Java.
- Не тягнути platform types глибоко в domain layer.
- Для міграції Java-проєкту на Kotlin рухатись поступово: новий код на Kotlin,
  стабільні Java contracts, обережний refactor.

**Коротко:** Kotlin і Java добре сумісні, бо працюють на одній JVM-платформі й
можуть викликати один одного напряму. Основні ризики interop — nullability,
mutability collections, default parameters і Java-facing API, які треба
контролювати через явні контракти та `@Jvm*` анотації.

</details>

<details>
<summary>27. Чи можна використовувати Java-анотації у Kotlin?</summary>

#### Kotlin

Так, Java-анотації можна використовувати у Kotlin. Kotlin компілюється в JVM
bytecode і добре інтегрується з Java reflection, annotation processing та
бібліотеками, які очікують Java-анотації: Spring, JPA, Retrofit, Dagger/Hilt,
JUnit, Jackson, Room та іншими.

1. **Базове використання**

```kotlin
@Deprecated("Use newMethod instead")
fun oldMethod() {
    // ...
}
```

Java-анотації застосовуються майже так само, як у Java, але Kotlin має більше
місць, куди може потрапити annotation: property, field, getter, setter,
constructor parameter.

2. **Use-site targets**

У Kotlin одна властивість може відповідати кільком JVM-елементам: field, getter,
setter, constructor parameter. Тому іноді треба явно вказати, куди саме
застосувати annotation.

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Найчастіші targets:

- `@field:` — annotation на backing field.
- `@get:` — annotation на getter.
- `@set:` — annotation на setter.
- `@param:` — annotation на constructor parameter.
- `@property:` — annotation на Kotlin property.

3. **Приклад з validation**

```kotlin
data class CreateUserRequest(
    @field:NotBlank
    val name: String,

    @field:Email
    val email: String
)
```

Для багатьох Java validation/framework annotations потрібен саме `@field:`, бо
framework читає Java field, а не Kotlin property.

4. **Nullability annotations**

Java-анотації `@Nullable` і `@NotNull` допомагають Kotlin правильно бачити
nullability Java API:

```java
@NotNull
String getName();
```

Тоді Kotlin може сприймати результат як `String`, а не як platform type
`String!`.

5. **Annotation processing**

Kotlin підтримує Java annotation processing через `kapt`, а також сучасніший
підхід `ksp` для Kotlin-first процесорів.

- `kapt` — сумісність із Java annotation processors.
- `ksp` — швидший і краще інтегрований з Kotlin symbol model.

Для Android це важливо в контексті Room, Dagger/Hilt, Moshi, serialization та
інших code generation інструментів.

6. **Jvm-анотації Kotlin**

Окремо Kotlin має власні анотації для контролю Java-facing API:

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun create(): Config = Config()
    }
}
```

Приклади: `@JvmStatic`, `@JvmOverloads`, `@JvmField`, `@JvmName`, `@Throws`.

7. **Практичні правила**

- Для Java frameworks перевіряти, чи annotation має бути на `field`, `getter`
  або `constructor parameter`.
- Для DTO/validation у Kotlin часто потрібен `@field:...`.
- На межі Java/Kotlin використовувати nullability annotations.
- Для нового Kotlin codegen надавати перевагу KSP, якщо бібліотека його
  підтримує.

**Коротко:** Java-анотації в Kotlin використовувати можна, але важливо розуміти
use-site targets. Kotlin property не дорівнює одному Java field, тому для
frameworks часто треба явно писати `@field:`, `@get:` або `@param:`.

</details>

<details>
<summary>28. Що таке Kotlin REPL?</summary>

#### Kotlin

Kotlin REPL — це інтерактивне середовище для швидкого виконання Kotlin-коду.
REPL розшифровується як Read-Eval-Print Loop: він читає введений код, виконує
його, друкує результат і чекає наступну команду.

1. **Для чого потрібен REPL**

- Швидко перевірити синтаксис Kotlin.
- Поекспериментувати з функціями стандартної бібліотеки.
- Перевірити поведінку колекцій, null safety, lambdas, scope-функцій.
- Протестувати маленький фрагмент коду без створення повного проєкту.
- Навчатися мові через короткі інтерактивні приклади.

2. **Приклад**

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
```

REPL одразу виконає вираз і покаже результат. Це зручно, коли треба швидко
перевірити гіпотезу без запуску застосунку або тестів.

3. **Як запустити**

Залежно від середовища REPL можна використовувати через:

- IntelliJ IDEA / Android Studio: Kotlin REPL або Scratch files.
- Kotlin command-line compiler.
- Онлайн playground.

У повсякденній Android-розробці частіше використовують Scratch files в IDE, бо
вони зручніші для фрагментів коду, imports і локальних експериментів.

4. **REPL vs Kotlin Scratch**

REPL — інтерактивна консоль: ввів рядок, одразу отримав результат. Scratch file
— файл для експериментів у IDE, який краще підходить для довших прикладів,
кількох функцій або невеликого сценарію.

5. **Обмеження**

- REPL не замінює unit tests.
- Він не показує повну поведінку Android runtime, lifecycle або framework API.
- Для коду з dependency injection, Gradle configuration, coroutines dispatchers
  або Android components потрібне реальне середовище виконання.
- Результат у REPL може бути корисним для експерименту, але production-рішення
  треба підтверджувати тестами.

6. **Практичне використання техлідом**

REPL корисний як швидкий інструмент для перевірки мовної семантики: як працює
`map`, `groupBy`, `Sequence`, destructuring, null-check або конкретна
standard-library функція. Але архітектурні рішення, concurrency-поведінку й
Android lifecycle у REPL перевіряти не варто.

**Коротко:** Kotlin REPL — це інтерактивна консоль для швидких експериментів із
Kotlin-кодом. Він корисний для навчання й перевірки маленьких фрагментів, але не
замінює тести, IDE debugging і запуск коду в реальному runtime.

</details>

<details>
<summary>29. Що таке Kotlin script (.kts)?</summary>

#### Kotlin

Kotlin script — це файл із розширенням `.kts`, який дозволяє виконувати Kotlin
код як скрипт, без створення повноцінного Kotlin-проєкту з класами, пакетами й
звичайною структурою застосунку. Скрипти корисні для автоматизації, build logic,
невеликих утиліт і конфігурацій.

1. **Базовий приклад**

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

У `.kts` можна писати top-level statements, тобто код не обовʼязково загортати в
`fun main()`.

2. **Чим .kts відрізняється від .kt**

- `.kt` — звичайний Kotlin source file, який компілюється як частина програми
  або бібліотеки.
- `.kts` — Kotlin script, який може виконуватись як сценарій.
- У `.kts` дозволені top-level executable statements.
- Скрипт зазвичай використовують для tooling/configuration, а не для основної
  бізнес-логіки застосунку.

3. **Gradle Kotlin DSL**

Найвідоміший приклад `.kts` у production — Gradle Kotlin DSL:

```kotlin
plugins {
    kotlin("jvm") version "1.9.0"
}

repositories {
    mavenCentral()
}
```

Файли `build.gradle.kts` і `settings.gradle.kts` — це Kotlin scripts для
конфігурації Gradle. Вони дають type-safe build configuration на Kotlin замість
Groovy DSL.

4. **Де використовують Kotlin scripts**

- Build configuration: `build.gradle.kts`, `settings.gradle.kts`.
- Локальні automation scripts.
- Невеликі code generation або maintenance tasks.
- Прототипування Kotlin-логіки.
- CI/CD helper scripts, якщо команда вже працює з Kotlin tooling.

5. **Переваги**

- Можна використовувати Kotlin syntax і standard library.
- Краще type safety, ніж у багатьох shell/Groovy сценаріях.
- Зручно для команд, де Kotlin — основна мова.
- Добре інтегрується з Gradle Kotlin DSL.

6. **Обмеження**

- Startup може бути повільнішим, ніж у простих shell scripts.
- Dependency management для standalone `.kts` може бути менш очевидним.
- Не варто переносити складну application logic у scripts.
- IDE support і execution model можуть залежати від конкретного середовища.

7. **Практичне правило**

`.kts` добре підходить для конфігурації та невеликої автоматизації, але якщо
логіка росте, має тести, залежності й domain rules, її краще винести в звичайний
Kotlin module або CLI tool.

**Коротко:** Kotlin script `.kts` — це спосіб виконувати Kotlin як скриптову мову.
Найчастіше він використовується в Gradle Kotlin DSL і tooling-задачах, але не
має замінювати нормальну структуру застосунку для складної логіки.

</details>

<details>
<summary>30. Що таке Kotlin style guide і чому його варто дотримуватись?</summary>

#### Kotlin

Kotlin style guide — це набір правил і рекомендацій щодо форматування,
іменування, структури файлів, організації imports і загального стилю
Kotlin-коду. Його мета — зробити код однаковим, передбачуваним і простим для
читання всією командою.

1. **Що входить у style guide**

- Naming conventions для класів, функцій, змінних, constants.
- Правила форматування: відступи, переноси рядків, пробіли.
- Організація package/imports.
- Рекомендації щодо структури класів і файлів.
- Правила використання expression bodies, lambdas, scope functions.
- Підходи до visibility modifiers і public API.

2. **Приклади naming conventions**

```kotlin
class UserRepository

fun loadUserById(id: UserId): User

val isUserActive: Boolean = true

const val DEFAULT_TIMEOUT_SECONDS = 30
```

Класи зазвичай пишуть у `PascalCase`, функції й змінні — у `camelCase`,
константи — у `UPPER_SNAKE_CASE`.

3. **Чому це важливо для команди**

- Менше часу витрачається на суперечки про стиль у code review.
- Код різних розробників виглядає однаково.
- Легше читати й підтримувати великі модулі.
- Простіше onboarding нових інженерів.
- Автоматичні formatter/linter можуть перевіряти стиль замість людей.

4. **Style guide vs архітектура**

Style guide не замінює архітектурні правила. Він відповідає на питання “як
писати код консистентно”, але не вирішує, де має жити бізнес-логіка, як
розділяти модулі або які залежності дозволені між шарами.

Для production-команди потрібні обидва рівні:

- style guide — локальна читабельність;
- architecture guidelines — структура системи;
- code review standards — якість змін;
- static analysis — автоматична перевірка.

5. **Інструменти**

У Kotlin-проєктах часто використовують:

- IntelliJ IDEA / Android Studio formatter.
- `ktlint` для форматування й style checks.
- `detekt` для static analysis і code smells.
- Gradle tasks у CI, щоб стиль перевірявся автоматично.

6. **Практичні правила**

- Не форматувати вручну те, що може форматувати tool.
- Мати однакові правила в IDE, Gradle і CI.
- Не змішувати style-only changes з великими feature changes.
- У code review не витрачати час на стиль, якщо це може перевірити linter.
- Командні відхилення від офіційного style guide мають бути явно
  задокументовані.

7. **Приклад техлід-позиції**

Style guide важливий не через “красивий код”, а через зниження cognitive load.
Коли весь код виглядає однаково, команда швидше читає зміни, легше знаходить
помилки й менше відволікається на неважливі деталі.

**Коротко:** Kotlin style guide — це правила консистентного написання Kotlin-коду.
Його варто дотримуватись, бо він зменшує шум у code review, покращує
читабельність і дозволяє автоматизувати частину якості через formatter, linter і
CI.

</details>

<details>
<summary>31. Що таке Kotlin-ідіоми?</summary>

#### Kotlin

Kotlin-ідіоми — це типові, природні для Kotlin способи писати код. Це не просто
синтаксичні трюки, а стиль мислення: використовувати можливості мови так, щоб
код був коротшим, безпечнішим, читабельнішим і краще виражав intent.

1. **val за замовчуванням**

```kotlin
val user = loadUser()
```

У Kotlin ідіоматично починати з `val`, а `var` використовувати тільки там, де
мутація справді потрібна. Це зменшує кількість неявного стану.

2. **Null safety замість ручних перевірок**

```kotlin
val displayName = user.name ?: "Unknown"
```

Ідіоматичний Kotlin використовує nullable/non-null типи, safe calls, Elvis
operator, `let`, `requireNotNull`, а не розкидані по коду `if (x == null)` або
`!!`.

3. **Data classes для моделей даних**

```kotlin
data class User(
    val id: Long,
    val name: String
)
```

Замість ручного `equals`, `hashCode`, `toString` і copy-конструкторів Kotlin
використовує `data class`.

4. **Expression style**

```kotlin
fun statusLabel(status: Status): String =
    when (status) {
        Status.Active -> "Active"
        Status.Blocked -> "Blocked"
    }
```

Kotlin часто заохочує expression-oriented код: `when`, `if`, lambdas і функції
можуть повертати значення. Це робить код компактнішим, якщо не втрачати
читабельність.

5. **Standard library operators**

```kotlin
val activeUserNames = users
    .filter { it.isActive }
    .map { it.name }
```

Замість ручних циклів для простих трансформацій Kotlin часто використовує `map`,
`filter`, `flatMap`, `fold`, `groupBy`, `associateBy`.

6. **Sealed types для станів**

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val users: List<User>) : UiState
    data class Error(val message: String) : UiState
}
```

Для обмеженої множини станів `sealed class/interface` часто краще, ніж enum або
набір nullable flags.

7. **Scope functions**

```kotlin
val user = User().apply {
    name = "Ivan"
    isActive = true
}
```

`let`, `run`, `apply`, `also`, `with` — ідіоматичні, але їх треба
використовувати обережно. Якщо scope function приховує intent або створює
плутанину з `this` та `it`, краще написати простіше.

8. **Early return і guard clauses**

```kotlin
fun handle(user: User?) {
    val value = user ?: return
    process(value)
}
```

Такий стиль часто зменшує вкладеність і робить happy path очевидним.

9. **Практична техлід-позиція**

Ідіоматичний Kotlin — це не “використати всі фічі мови”. Це баланс:

- використовувати `val`, null safety і типи для безпеки;
- застосовувати standard library там, де вона покращує intent;
- не зловживати scope functions, operator overloading або надто clever lambdas;
- писати код, який легко читає вся команда, а не тільки автор.

**Коротко:** Kotlin-ідіоми — це усталені способи писати природний Kotlin-код: `val`
by default, null safety, data/sealed classes, expression style, standard-library
operators і акуратне використання scope functions. Ідіома має покращувати
читабельність, а не демонструвати знання синтаксису.

</details>

<details>
<summary>32. Як Kotlin використовується в бекенд-розробці і чому його обирають замість Java?</summary>

#### Kotlin

Kotlin у backend-розробці використовується як JVM-мова для створення REST API,
мікросервісів, event-driven систем, CLI-сервісів і server-side застосунків. Він
працює в тій самій екосистемі, що й Java: JVM, Gradle/Maven, Spring, Hibernate,
Kafka, SQL-драйвери, observability tools і production infrastructure.

1. **Основні backend-фреймворки**

- Spring Boot з Kotlin support.
- Ktor — Kotlin-first framework від JetBrains.
- Micronaut, Quarkus та інші JVM-фреймворки.
- gRPC, Kafka consumers/producers, scheduled workers, CLI tools.

Приклад Spring Boot controller:

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse {
        return userService.getUser(id)
    }
}
```

2. **Чому обирають Kotlin замість Java**

- Менше boilerplate: `data class`, default parameters, extension functions.
- Null safety на рівні type system.
- Коротший і виразніший код для DTO, mapping, validation, configuration.
- Хороша підтримка functional style.
- Корутини для асинхронного коду без callback hell.
- Повна сумісність із Java-бібліотеками.

3. **DTO і моделі**

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

У Java для такого класу часто потрібні getters, constructor, `equals`,
`hashCode`, `toString` або Lombok/records. У Kotlin це природний синтаксис мови.

4. **Корутини в backend**

Kotlin coroutines дозволяють писати асинхронний код у послідовному стилі:

```kotlin
suspend fun loadUserProfile(id: Long): UserProfile {
    val user = userClient.getUser(id)
    val orders = orderClient.getOrders(id)
    return UserProfile(user, orders)
}
```

Це корисно для I/O-bound сервісів, але вимагає розуміння dispatchers, structured
concurrency, cancellation і backpressure.

5. **Java interop як перевага**

Kotlin не змушує відмовлятися від Java-екосистеми. Команда може:

- використовувати existing Java libraries;
- поступово мігрувати Java-код на Kotlin;
- тримати частину модулів на Java;
- викликати Kotlin із Java і Java з Kotlin.

Це важливо для enterprise/backend систем, де повний rewrite майже ніколи не є
раціональним.

6. **Trade-offs**

- Kotlin має власні нюанси компіляції, annotation processing і Gradle setup.
- Java developers потребують часу на idiomatic Kotlin.
- Неакуратне використання scope functions, `!!` або coroutines може погіршити
  якість коду.
- Деякі Java frameworks потребують правильних Kotlin plugins або compiler
  налаштувань, наприклад `all-open`, `no-arg`, `kapt/ksp`.

7. **Техлід-позиція**

Kotlin у backend має сенс, коли команда готова писати саме Kotlin, а не Java з
іншим синтаксисом. Найбільша вигода приходить від null safety, data modeling,
корутин, компактного API і дисципліни в типах. Але потрібні style guide, static
analysis, зрозуміла архітектура й навчання команди.

**Коротко:** Kotlin у backend використовують для JVM-сервісів на Spring, Ktor та
інших фреймворках. Його обирають замість Java через менше boilerplate, null
safety, корутини й виразнішу type system, зберігаючи доступ до всієї
Java-екосистеми.

</details>

<details>
<summary>33. Що таке функції вищого порядку (higher-order functions)?</summary>

#### Kotlin

Функція вищого порядку — це функція, яка приймає іншу функцію як параметр,
повертає функцію як результат або робить обидві речі. У Kotlin це природна
частина мови, бо функції можна передавати як значення через function types і
lambda expressions.

1. **Функція приймає іншу функцію**

```kotlin
fun repeatAction(
    times: Int,
    action: () -> Unit
) {
    repeat(times) {
        action()
    }
}

repeatAction(3) {
    println("Hello")
}
```

`action: () -> Unit` означає: параметр `action` — це функція без аргументів, яка
не повертає значимого результату.

2. **Функція повертає функцію**

```kotlin
fun multiplier(factor: Int): (Int) -> Int {
    return { value -> value * factor }
}

val double = multiplier(2)
println(double(10)) // 20
```

Тут `multiplier` повертає lambda, яка захоплює `factor` із зовнішнього scope.

3. **Типові приклади в стандартній бібліотеці**

```kotlin
val names = users
    .filter { it.isActive }
    .map { it.name }
```

`filter` і `map` — функції вищого порядку. Вони приймають lambdas, які
визначають поведінку для кожного елемента.

4. **Callbacks**

```kotlin
fun loadUser(
    id: Long,
    onSuccess: (User) -> Unit,
    onError: (Throwable) -> Unit
) {
    // ...
}
```

Higher-order functions часто використовуються для callbacks, event handlers,
middleware, DSL і dependency injection дрібної поведінки.

5. **Function references**

```kotlin
fun printUser(user: User) {
    println(user.name)
}

users.forEach(::printUser)
```

Якщо вже є функція з потрібною сигнатурою, її можна передати через reference
`::functionName`.

6. **Переваги**

- Дозволяють параметризувати поведінку без створення зайвих інтерфейсів.
- Роблять APIs гнучкими й компактними.
- Добре підходять для collection pipelines і DSL.
- Зменшують boilerplate у callbacks і event handling.

7. **Ризики**

- Довгі або вкладені lambdas погіршують читабельність.
- Захоплення mutable state може створити приховані side effects.
- Function objects можуть створювати allocations, якщо функція не `inline`.
- Надто абстрактні higher-order APIs важко дебажити й підтримувати.

8. **Звʼязок з inline**

У Kotlin багато стандартних higher-order functions позначені як `inline`, щоб
зменшити overhead від lambdas:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.currentTimeMillis()
    block()
    println(System.currentTimeMillis() - start)
}
```

`inline` підставляє тіло функції й lambda в місце виклику, що може зменшити
allocation overhead і дозволити non-local returns.

**Коротко:** функції вищого порядку — це функції, які приймають або повертають інші
функції. У Kotlin вони лежать в основі lambdas, callbacks, collection operators
і DSL, але мають використовуватись так, щоб код залишався читабельним і
контрольованим.

</details>

<details>
<summary>34. Що таке inline-функції і коли їх варто використовувати?</summary>

#### Kotlin

`inline`-функція — це функція, тіло якої компілятор підставляє безпосередньо в
місце виклику. У Kotlin це особливо важливо для функцій вищого порядку, які
приймають lambdas, бо inlining може прибрати зайві function object allocations і
дозволити деякі мовні можливості, наприклад `reified` generic types.

1. **Базовий приклад**

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.currentTimeMillis()
    block()
    println(System.currentTimeMillis() - start)
}

measure {
    println("Work")
}
```

Компілятор може підставити код `measure` і lambda прямо в місце виклику, замість
створення окремого function object.

2. **Навіщо це потрібно**

- Зменшити overhead від lambdas у hot path.
- Дозволити non-local returns із lambdas.
- Дозволити `reified` type parameters.
- Будувати ефективні DSL і control-flow helpers.

3. **Non-local return**

```kotlin
inline fun runBlock(block: () -> Unit) {
    block()
}

fun test() {
    runBlock {
        return
    }
}
```

Через `inline` `return` всередині lambda може повернути не тільки з lambda, а з
зовнішньої функції `test`. Це потужна можливість, але вона має бути очевидною
для читача.

4. **reified type parameters**

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean {
    return this is T
}
```

Без `inline` і `reified` generic type `T` був би стертий через type erasure.
Inlining дозволяє компілятору підставити реальний тип у місце виклику.

5. **noinline**

Якщо не всі lambda-параметри треба inline-ити, використовують `noinline`:

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

`noinline` lambda можна зберігати в змінну, передавати далі або викликати не
тільки inline-сценарієм.

6. **crossinline**

`crossinline` забороняє non-local return з lambda:

```kotlin
inline fun post(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

Це потрібно, коли lambda виконується в іншому контексті, наприклад всередині
`Runnable`, object expression або callback.

7. **Коли використовувати inline**

- Для маленьких higher-order functions.
- Для performance-sensitive lambdas.
- Для DSL/control-flow helpers.
- Коли потрібен `reified`.
- Коли потрібно дозволити non-local return.

8. **Коли не використовувати**

- Для великих функцій: може збільшити bytecode.
- Для функцій без lambda-параметрів, якщо немає сильної причини.
- Для API, де inlining ускладнює binary compatibility.
- Як “магічну оптимізацію” без вимірювання або реальної потреби.

**Коротко:** `inline` каже компілятору підставити тіло функції в місце виклику. Це
корисно для маленьких higher-order functions, DSL, `reified` generics і
зменшення lambda overhead, але надмірне використання може збільшити bytecode і
ускладнити підтримку API.

</details>

<details>
<summary>35. Що означає ключове слово reified і де воно застосовується?</summary>

#### Kotlin

`reified` означає, що generic-тип доступний у runtime всередині `inline`
функції. Звичайні generic types на JVM стираються через type erasure, тому в
runtime не можна напряму перевірити `T` або отримати `T::class`. `reified`
вирішує це для inline-функцій, бо компілятор підставляє реальний тип у місце
виклику.

1. **Проблема type erasure**

```kotlin
fun <T> isOfType(value: Any): Boolean {
    // return value is T // compile error
    return false
}
```

Звичайний `T` недоступний у runtime. JVM знає, що є generic, але не знає
конкретний тип `T` після компіляції.

2. **Рішення через reified**

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean {
    return value is T
}

val result = isOfType<String>("Kotlin")
```

Тут `T` доступний, бо функція `inline`, і компілятор підставляє конкретний тип у
місце виклику.

3. **Отримання KClass/Class**

```kotlin
inline fun <reified T> typeName(): String {
    return T::class.simpleName.orEmpty()
}
```

Для Java reflection:

```kotlin
inline fun <reified T> javaClassOf(): Class<T> {
    return T::class.java
}
```

Це часто використовують у serialization, DI, navigation, logging і testing
helpers.

4. **Типовий приклад з JSON**

```kotlin
inline fun <reified T> Json.decode(json: String): T {
    return decodeFromString<T>(json)
}
```

Без `reified` часто довелося б передавати `Class<T>`, `KClass<T>` або serializer
окремим параметром.

5. **Фільтрація за типом**

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

`filterIsInstance<T>()` — приклад стандартної бібліотеки, який використовує
reified type parameter.

6. **Обмеження**

- `reified` можна використовувати тільки в `inline` functions.
- Не можна зробити `reified` generic у звичайному класі або non-inline функції.
- `reified` не скасовує всі обмеження type erasure для вкладених generic-типів.
  Наприклад, перевірка `List<String>` у runtime все ще має нюанси.
- Inlining може збільшувати bytecode, якщо функція велика або викликається дуже
  часто.

7. **Коли використовувати**

- Type checks: `value is T`.
- Reflection helpers: `T::class`, `T::class.java`.
- Serialization/deserialization.
- DI/service locator helpers.
- Android helpers, наприклад intent extras або fragment arguments.
- Testing utilities.

8. **Коли краще не використовувати**

- Якщо можна передати явний `KClass<T>` або serializer і це робить API
  прозорішим.
- Якщо функція велика й `inline` створить зайвий bytecode.
- Якщо `reified` використовується лише для clever syntax, а не для реальної
  runtime-інформації про тип.

**Коротко:** `reified` дозволяє використовувати generic-тип `T` у runtime, але лише
в `inline` функціях. Його застосовують для type checks, reflection,
serialization, DI helpers і API, де інакше довелося б вручну передавати
`Class<T>` або `KClass<T>`.

</details>

<details>
<summary>36. Що таке sealed-класи та sealed-інтерфейси?</summary>

#### Kotlin

`sealed class` і `sealed interface` описують обмежену ієрархію типів: компілятор
знає всі прямі підтипи в межах дозволеної області. Це дуже корисно для
моделювання станів, результатів операцій, подій, команд і domain-specific
варіантів, де множина можливих випадків має бути контрольованою.

1. **Базовий приклад sealed class**

```kotlin
sealed class UiState {
    data object Loading : UiState()
    data class Content(val users: List<User>) : UiState()
    data class Error(val message: String) : UiState()
}
```

Тут `UiState` може бути тільки одним із відомих варіантів: `Loading`, `Content`,
`Error`.

2. **Exhaustive when**

```kotlin
fun render(state: UiState): String =
    when (state) {
        UiState.Loading -> "Loading"
        is UiState.Content -> "Users: ${state.users.size}"
        is UiState.Error -> "Error: ${state.message}"
    }
```

Оскільки компілятор знає всі підтипи `UiState`, він може перевірити, що `when`
обробляє всі випадки. Якщо додати новий стан, компілятор покаже місця, які
потрібно оновити.

3. **sealed interface**

```kotlin
sealed interface PaymentResult {
    data object Success : PaymentResult
    data class Failed(val reason: String) : PaymentResult
}
```

`sealed interface` корисний, коли потрібна більш гнучка модель: клас може
реалізовувати кілька інтерфейсів, а sealed interface не несе constructor state.

4. **Sealed class vs sealed interface**

- `sealed class` може мати state, constructor, protected members і спільну
  реалізацію.
- `sealed interface` краще підходить для чистих ієрархій контрактів.
- Клас може наслідувати тільки один class, але реалізовувати кілька interfaces.
- Якщо потрібен shared constructor/state — частіше `sealed class`.
- Якщо потрібна композиція типів — частіше `sealed interface`.

5. **Типові сценарії**

- UI state: `Loading`, `Content`, `Error`.
- Network result: `Success`, `HttpError`, `NetworkError`.
- Domain commands/events.
- Navigation destinations.
- Parser/compiler states.
- Обмежені бізнес-стани, де enum недостатньо.

6. **Чому не просто enum**

`enum` добре підходить для фіксованого набору констант без різного payload.
Sealed types дозволяють кожному варіанту мати власні дані:

```kotlin
sealed interface LoginResult {
    data class Success(val user: User) : LoginResult
    data class InvalidPassword(val attemptsLeft: Int) : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

Це значно точніше, ніж enum плюс nullable fields.

7. **Практичні правила**

- Використовувати sealed types для закритих доменних варіантів.
- Не використовувати sealed, якщо ієрархія має розширюватися зовнішніми модулями
  або plugins.
- Уникати `else` у `when` по sealed type, якщо хочете, щоб компілятор ловив нові
  необроблені випадки.
- Для stateless варіантів використовувати `data object`, для варіантів із даними
  — `data class`.

**Коротко:** sealed-класи та sealed-інтерфейси дозволяють моделювати контрольовану
множину підтипів. Їх головна перевага — type-safe стани й exhaustive `when`,
особливо коли кожен варіант може мати власні дані.

</details>

<details>
<summary>37. Що таке object expression і коли його використовують?</summary>

#### Kotlin

`object expression` — це спосіб створити анонімний обʼєкт прямо в місці
використання. Це Kotlin-аналог anonymous class із Java, але з більш компактним
синтаксисом. Його використовують, коли потрібна одноразова реалізація
інтерфейсу, абстрактного класу або невеликий обʼєкт без окремого іменованого
класу.

1. **Базовий приклад**

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

Тут створюється анонімний обʼєкт, який реалізує `ClickListener`.

2. **Наслідування класу**

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

`object expression` може наслідувати клас, реалізовувати інтерфейс або робити
обидві речі одночасно.

3. **Обʼєкт без supertypes**

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Такий anonymous object зручно використовувати локально, але для public API краще
створити нормальний `data class` або іменований тип.

4. **Доступ до зовнішніх змінних**

```kotlin
var clickCount = 0

val listener = object : ClickListener {
    override fun onClick() {
        clickCount += 1
    }
}
```

Як і lambda, object expression може захоплювати змінні з зовнішнього scope. З
mutable state треба бути обережним, особливо в async або UI lifecycle-коді.

5. **Object expression vs object declaration**

```kotlin
object AppConfig {
    val host = "localhost"
}
```

- `object declaration` створює named singleton.
- `object expression` створює anonymous object у конкретному місці.
- `object declaration` ініціалізується ліниво як singleton.
- `object expression` створюється щоразу, коли виконується відповідний вираз.

6. **Коли використовувати**

- Для одноразової реалізації listener/callback interface.
- Для тестових doubles/stubs без окремого класу.
- Для локальної кастомізації поведінки.
- Для адаптації Java APIs, які очікують object/interface implementation.
- Для коротких anonymous objects, які не варто виносити в окремий тип.

7. **Коли не варто використовувати**

- Якщо обʼєкт має складну логіку або повторно використовується.
- Якщо anonymous object погіршує читабельність.
- Якщо потрібен стабільний public type.
- Якщо залежності й lifecycle краще контролювати через DI або іменований клас.

8. **Практична техлід-позиція**

`object expression` добре підходить для локальної поведінки, але не має ставати
способом ховати архітектурні залежності. Якщо anonymous object росте, має стан,
кілька методів або використовується в кількох місцях, його краще перетворити на
іменований клас.

**Коротко:** `object expression` створює анонімний обʼєкт на місці. Його
використовують для локальних реалізацій інтерфейсів, callbacks, stubs і
одноразової поведінки; для shared або складної логіки краще іменований клас.

</details>

<details>
<summary>38. У чому різниця між enum-класами та sealed-класами?</summary>

#### Kotlin

`enum class` і `sealed class` обидва описують обмежену множину варіантів, але
роблять це по-різному. `enum` — це набір фіксованих singleton-констант одного
типу. `sealed class` — це закрита ієрархія підтипів, де кожен варіант може мати
власну структуру, state і поведінку.

1. **Enum class**

```kotlin
enum class OrderStatus {
    Draft,
    Paid,
    Shipped,
    Cancelled
}
```

`enum` добре підходить, коли варіанти прості, фіксовані й не потребують різних
даних для кожного випадку.

2. **Enum із властивостями**

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

Enum може мати constructor parameters і методи, але кожна enum-константа все
одно є singleton-обʼєктом. Вона не може нести різний runtime payload для кожного
окремого результату.

3. **Sealed class**

```kotlin
sealed class PaymentResult {
    data class Success(val transactionId: String) : PaymentResult()
    data class Failed(val reason: String) : PaymentResult()
    data object Cancelled : PaymentResult()
}
```

Sealed class дозволяє кожному варіанту мати власні дані. Наприклад `Success` має
`transactionId`, `Failed` має `reason`, а `Cancelled` не має payload.

4. **Exhaustive when**

І enum, і sealed class добре працюють із `when`:

```kotlin
fun label(status: OrderStatus): String =
    when (status) {
        OrderStatus.Draft -> "Draft"
        OrderStatus.Paid -> "Paid"
        OrderStatus.Shipped -> "Shipped"
        OrderStatus.Cancelled -> "Cancelled"
    }
```

Для sealed:

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is PaymentResult.Success -> result.transactionId
        is PaymentResult.Failed -> result.reason
        PaymentResult.Cancelled -> "Cancelled"
    }
```

Компілятор може перевірити, що всі варіанти оброблені.

5. **Ключова різниця**

- `enum` — однаковий набір констант одного типу.
- `sealed class` — різні підтипи в межах закритої ієрархії.
- `enum` простіший і легший для статусів без payload.
- `sealed class` сильніший для станів із різними даними.
- `enum` має built-in `values()/entries`, `name`, `ordinal`.
- `sealed class` краще моделює algebraic data types і domain states.

6. **Коли використовувати enum**

- Простий статус: `Active`, `Blocked`, `Deleted`.
- Стабільний набір констант.
- Потрібна інтеграція з persistence/API як string/int status.
- Не потрібен різний payload для кожного варіанту.

7. **Коли використовувати sealed**

- UI state: `Loading`, `Content(data)`, `Error(message)`.
- Result type: `Success(value)`, `Failure(error)`.
- Domain events/commands з різними полями.
- Сценарії, де enum змусив би додати nullable fields або external maps.

8. **Типова помилка**

Поганий сигнал — enum із купою nullable полів або додаткових lookup-структур:

```kotlin
enum class ResultType {
    Success,
    Error
}
```

А десь поруч окремо зберігаються `data`, `errorMessage`, `code`. У такому
випадку sealed type майже завжди точніше моделює домен.

**Коротко:** `enum` краще для простого фіксованого набору констант без різного
payload. `sealed class` краще для закритої ієрархії станів, де кожен варіант
може мати власні дані й поведінку.

</details>

<details>
<summary>39. У чому різниця між lateinit та lazy ініціалізацією?</summary>

#### Kotlin

`lateinit` і `lazy` вирішують схожу проблему — значення буде ініціалізоване
пізніше, не в момент створення обʼєкта. Але це різні механізми з різними
гарантіями: `lateinit` — відкладена ініціалізація mutable `var`, а `lazy` —
відкладене обчислення read-only `val` при першому доступі.

1. **lateinit**

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun loadUser(id: Long): User {
        return repository.load(id)
    }
}
```

`lateinit` дозволяє оголосити non-null `var` без початкового значення. Значення
має бути присвоєне до першого читання.

2. **Що буде, якщо прочитати lateinit до ініціалізації**

```kotlin
lateinit var name: String

println(name) // UninitializedPropertyAccessException
```

Якщо звернутися до `lateinit`-властивості до присвоєння, Kotlin кине
`UninitializedPropertyAccessException`.

3. **Обмеження lateinit**

- Працює тільки з `var`.
- Не працює з primitive types: `Int`, `Boolean`, `Long` тощо.
- Не може бути nullable типом на кшталт `String?`.
- Не дає thread-safety гарантій.
- Добре підходить для dependency injection, тестів або framework lifecycle, але
  потребує дисципліни.

Можна перевірити ініціалізацію:

```kotlin
if (::repository.isInitialized) {
    repository.load(1L)
}
```

4. **lazy**

```kotlin
class UserService {
    val expensiveConfig: Config by lazy {
        loadConfig()
    }
}
```

`lazy` виконає lambda тільки при першому зверненні до `expensiveConfig`, збереже
результат і повертає той самий результат при наступних зверненнях.

5. **Особливості lazy**

- Використовується з `val`.
- Ініціалізується один раз.
- За замовчуванням thread-safe (`LazyThreadSafetyMode.SYNCHRONIZED`).
- Підходить для дорогих обчислень або обʼєктів, які можуть не знадобитися.

```kotlin
val value: String by lazy(LazyThreadSafetyMode.NONE) {
    "computed"
}
```

Режим `NONE` можна використовувати, якщо точно відомо, що доступ буде з одного
потоку.

6. **Ключова різниця**

- `lateinit var` — значення встановлює зовнішній код пізніше.
- `val by lazy` — значення обчислюється автоматично при першому доступі.
- `lateinit` можна переприсвоювати.
- `lazy` після першої ініціалізації не переобчислюється.
- `lateinit` не thread-safe сам по собі.
- `lazy` має configurable thread-safety.

7. **Android-приклади**

`lateinit` часто бачать у DI або tests:

```kotlin
@Inject
lateinit var analytics: Analytics
```

`lazy` часто використовують для обʼєктів, які не завжди потрібні:

```kotlin
private val adapter by lazy {
    UserAdapter()
}
```

Але для View Binding у Fragment треба бути обережним: `lateinit` або nullable
binding може легко створити lifecycle bug або memory leak, якщо не очищати
посилання в `onDestroyView()`.

8. **Практичне правило**

- Якщо значення має бути immutable і може бути створене при першому доступі —
  `lazy`.
- Якщо значення встановлює DI/framework/test setup після створення обʼєкта —
  `lateinit`.
- Якщо значення може бути відсутнім як нормальний стан — краще nullable тип, а
  не `lateinit`.
- Якщо потрібна повторна зміна значення — `lateinit var` або звичайний `var`,
  але з чітким lifecycle.

**Коротко:** `lateinit` — це обіцянка “я присвою це `var` до першого використання”.
`lazy` — це делегат “обчисли це `val` при першому доступі й закешуй”. `lazy`
краще для immutable delayed initialization, `lateinit` — для DI/framework-driven
ініціалізації.

</details>

<details>
<summary>40. Що таке тип Nothing і як він впливає на керування потоком виконання?</summary>

#### Kotlin

`Nothing` — це спеціальний тип Kotlin, який не має жодного значення. Функція з
return type `Nothing` ніколи не завершується нормально: вона або кидає
exception, або входить у нескінченний цикл, або іншим способом не повертає
control flow у місце виклику.

1. **Базовий приклад**

```kotlin
fun fail(message: String): Nothing {
    throw IllegalStateException(message)
}
```

Ця функція не може повернути значення, бо завжди кидає exception. Тому її тип —
`Nothing`.

2. **Nothing як bottom type**

`Nothing` є subtype для всіх типів у Kotlin. Це означає, що вираз типу `Nothing`
можна використовувати там, де очікується будь-який інший тип, бо значення все
одно ніколи не буде повернуте.

```kotlin
val name: String = user.name ?: fail("Name is required")
```

Права частина Elvis-оператора має тип `Nothing`, але весь вираз може мати тип
`String`, бо якщо `user.name` дорівнює `null`, виконання буде перерване.

3. **throw має тип Nothing**

```kotlin
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` у Kotlin є expression з типом `Nothing`, тому його можна використовувати
в Elvis-операторі, `when`, expression body та інших виразах.

4. **return у виразах**

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

`return` теж працює як expression, який не повертає значення в поточний вираз.
Це допомагає компілятору зрозуміти control flow: після `name ?: return` змінна
`value` має non-null тип `String`.

5. **Вплив на smart casts**

```kotlin
fun printLength(value: String?) {
    if (value == null) {
        fail("Value is null")
    }

    println(value.length)
}
```

Після виклику `fail()` компілятор знає, що гілка з `null` не продовжить
виконання. Тому нижче `value` може бути smart-cast до `String`.

6. **Nothing?**

Є також тип `Nothing?`. Він має єдине можливе значення — `null`.

```kotlin
val value = null
```

Якщо компілятор не має додаткового контексту, тип такого виразу може бути
`Nothing?`, бо це “найнижчий” nullable type.

7. **Типові сценарії**

- Функції, які завжди кидають exception: `error()`, `TODO()`, custom `fail()`.
- Exhaustive control flow у `when`.
- Elvis з `throw` або `return`.
- Позначення коду, який не має нормально завершитись.
- Допомога компілятору у smart casts і type inference.

8. **Практичне правило**

`Nothing` не потрібно часто писати вручну, але важливо розуміти його семантику.
Він пояснює, чому `throw`, `return`, `TODO()` і `error()` можуть бути частиною
виразів і як Kotlin робить control flow type-safe.

**Коротко:** `Nothing` — це тип для коду, який ніколи не повертає нормальний
результат. Він допомагає Kotlin точно моделювати `throw`, `return`, fail-fast
функції, smart casts і expression-based control flow.

</details>

<details>
<summary>41. Чи може Java викликати Kotlin-функції з параметрами за замовчуванням?</summary>

#### Kotlin

Java може викликати Kotlin-функції, але Java не розуміє Kotlin default
parameters напряму. Для Java звичайна Kotlin-функція виглядає як метод з повним
набором параметрів. Якщо потрібно, щоб Java мала зручні overloads, у Kotlin
використовують `@JvmOverloads`.

1. **Kotlin-функція з default parameter**

```kotlin
class UserService {
    fun createUser(
        name: String,
        isActive: Boolean = true
    ) {
        // ...
    }
}
```

З Kotlin можна викликати так:

```kotlin
service.createUser("Ivan")
service.createUser("Ivan", isActive = false)
```

2. **Як це бачить Java без @JvmOverloads**

Java не зможе просто викликати:

```java
service.createUser("Ivan");
```

Для Java доступна основна сигнатура з усіма параметрами:

```java
service.createUser("Ivan", true);
```

Kotlin генерує спеціальний synthetic механізм для default arguments, але він не
є нормальним Java API для ручного використання.

3. **Рішення: @JvmOverloads**

```kotlin
class UserService {
    @JvmOverloads
    fun createUser(
        name: String,
        isActive: Boolean = true,
        role: String = "user"
    ) {
        // ...
    }
}
```

Тоді для Java будуть згенеровані overloads:

```java
service.createUser("Ivan");
service.createUser("Ivan", false);
service.createUser("Ivan", false, "admin");
```

4. **Як генеруються overloads**

`@JvmOverloads` генерує overloads справа наліво для параметрів із default
values. Якщо default values є в кінці списку параметрів, Java API буде
найзручнішим.

```kotlin
@JvmOverloads
fun connect(
    host: String,
    port: Int = 443,
    useSsl: Boolean = true
)
```

Java отримає варіанти з `host`, з `host + port`, і з усіма параметрами.

5. **Конструктори**

`@JvmOverloads` працює і з конструкторами:

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Це корисно, якщо клас має використовуватися з Java або Java-based frameworks.

6. **Обмеження й нюанси**

- `@JvmOverloads` може створити багато overloads, якщо default parameters
  багато.
- Named arguments у Java не існують.
- Якщо параметри одного типу, Java overloads можуть бути менш читабельними.
- Для складних конфігурацій краще configuration object або builder.
- Для Kotlin-only API `@JvmOverloads` зазвичай не потрібен.

7. **Практичне правило**

- Якщо API викликається тільки з Kotlin — достатньо default parameters.
- Якщо API є Java-facing — подумати про `@JvmOverloads`.
- Якщо параметрів багато — краще request/config object.
- Якщо це framework constructor/API — перевірити, які overloads очікує
  framework.

**Коротко:** Java може викликати Kotlin-функції з default parameters тільки через
повну сигнатуру, якщо не згенерувати Java-friendly overloads. Для цього
використовують `@JvmOverloads`, але застосовувати його варто лише для API, які
справді викликаються з Java.

</details>

<details>
<summary>42. Що таке tail-recursive функції?</summary>

#### Kotlin

Tail-recursive функція — це рекурсивна функція, у якій рекурсивний виклик є
останньою операцією перед поверненням результату. У Kotlin такі функції можна
позначити модифікатором `tailrec`, і компілятор спробує оптимізувати рекурсію в
цикл, щоб уникнути росту call stack.

1. **Базовий приклад**

```kotlin
tailrec fun factorial(
    n: Int,
    accumulator: Long = 1L
): Long {
    return if (n <= 1) {
        accumulator
    } else {
        factorial(n - 1, accumulator * n)
    }
}
```

Рекурсивний виклик `factorial(...)` є останньою дією в гілці `else`, тому
компілятор може перетворити його на цикл.

2. **Чому це важливо**

Звичайна рекурсія створює новий stack frame для кожного виклику. Для великої
глибини це може закінчитися `StackOverflowError`.

Tail recursion optimization дозволяє виконати такий код як loop без росту стека.

3. **Приклад не tail-recursive**

```kotlin
fun factorial(n: Int): Long {
    return if (n <= 1) {
        1L
    } else {
        n * factorial(n - 1)
    }
}
```

Тут після рекурсивного виклику ще треба виконати множення `n * ...`, тому
рекурсивний виклик не є останньою операцією. Таку функцію `tailrec` оптимізувати
не можна.

4. **Що робить tailrec**

```kotlin
tailrec fun countdown(n: Int) {
    if (n == 0) return
    countdown(n - 1)
}
```

`tailrec` — це не просто документація. Якщо функція не відповідає умовам tail
recursion, компілятор видасть помилку або warning, що оптимізація неможлива.

5. **Умови для tailrec**

- Рекурсивний виклик має бути останньою операцією.
- Не має бути додаткової роботи після рекурсивного виклику.
- Функція має викликати саму себе напряму.
- Не працює для взаємної рекурсії, де `a()` викликає `b()`, а `b()` викликає
  `a()`.
- Не має бути в `try/catch/finally` у формі, яка заважає оптимізації.

6. **Коли використовувати**

- Для алгоритмів, які природно рекурсивні: обходи, парсинг, математичні функції,
  state transitions.
- Коли рекурсивний стиль значно читабельніший за цикл.
- Коли можна легко зробити accumulator-style функцію.

7. **Коли краще цикл**

- Якщо команда читає loop простіше за рекурсію.
- Якщо алгоритм не є природно рекурсивним.
- Якщо потрібна максимальна передбачуваність performance.
- Якщо tailrec вимагає неприродної перебудови коду.

8. **Техлід-позиція**

`tailrec` корисний, але не має бути демонстрацією “функціональності заради
функціональності”. У production-коді головний критерій — читабельність і
коректність. Якщо простий `while` або `for` ясніший, краще використати його.

**Коротко:** tail-recursive функція — це рекурсія, де рекурсивний виклик є останньою
операцією. У Kotlin модифікатор `tailrec` дозволяє компілятору перетворити таку
рекурсію на цикл і уникнути переповнення стека.

</details>

<details>
<summary>43. Що таке scope-функції (let, run, apply, also, with) і коли їх застосовувати?</summary>

#### Kotlin

Scope-функції — це функції стандартної бібліотеки Kotlin, які виконують блок
коду в контексті певного обʼєкта. Вони допомагають компактно писати
ініціалізацію, трансформації, null-safe блоки, side effects і групування логіки
навколо одного receiver.

1. **Ключова ідея**

Усі scope-функції роблять схожу річ: дають тимчасовий scope для обʼєкта. Вони
відрізняються двома речами:

- як доступний обʼєкт: `this` або `it`;
- що повертається: результат lambda або сам обʼєкт.

2. **Коротка таблиця**

| Функція | Обʼєкт доступний як | Повертає         |
| ------- | ------------------- | ---------------- |
| `let`   | `it`                | результат lambda |
| `run`   | `this`              | результат lambda |
| `apply` | `this`              | сам обʼєкт       |
| `also`  | `it`                | сам обʼєкт       |
| `with`  | `this`              | результат lambda |

3. **let**

`let` часто використовують для null-safe виконання або трансформації:

```kotlin
val length = name?.let {
    it.length
}
```

Добре підходить, коли треба працювати з обʼєктом як `it` і повернути новий
результат.

4. **run**

`run` корисний, коли треба виконати кілька операцій у контексті обʼєкта і
повернути результат:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

Receiver доступний як `this`, тому можна не писати `user.` перед кожним полем.

5. **apply**

`apply` використовують для конфігурації обʼєкта:

```kotlin
val user = User().apply {
    name = "Ivan"
    isActive = true
}
```

Повертається сам обʼєкт, тому `apply` добре підходить для builders і
ініціалізації.

6. **also**

`also` використовують для side effects без зміни основного chain:

```kotlin
val user = loadUser()
    .also { logger.info("Loaded user: ${it.id}") }
```

Обʼєкт доступний як `it`, повертається той самий обʼєкт.

7. **with**

`with` не є extension function, обʼєкт передається аргументом:

```kotlin
val summary = with(user) {
    "$firstName $lastName"
}
```

Зручно, коли потрібно виконати кілька операцій над уже існуючим обʼєктом і
повернути результат.

8. **Коли застосовувати**

- `let` — null-safe block або трансформація.
- `run` — обчислення результату в контексті обʼєкта.
- `apply` — налаштування/ініціалізація обʼєкта.
- `also` — logging, debugging, validation side effects у chain.
- `with` — групування операцій над обʼєктом без extension-call стилю.

9. **Коли бути обережним**

- Не вкладати scope-функції глибоко одна в одну.
- Не змішувати `this` і `it`, якщо це погіршує читабельність.
- Не використовувати scope-функцію тільки “бо можна”.
- Якщо блок стає довгим, краще винести named function.
- Для бізнес-логіки важливіша ясність, ніж компактність.

**Коротко:** scope-функції дають короткий scope для роботи з обʼєктом. Вибір між
`let`, `run`, `apply`, `also`, `with` залежить від двох питань: як звертатись до
обʼєкта (`this` чи `it`) і що має повернутись — сам обʼєкт чи результат lambda.

</details>

<details>
<summary>44. У чому різниця між let, run та apply?</summary>

#### Kotlin

`let`, `run` і `apply` — це scope-функції Kotlin. Вони схожі тим, що виконують
блок коду в контексті обʼєкта, але відрізняються двома речами: як доступний
обʼєкт всередині lambda (`it` або `this`) і що повертає функція (результат
lambda або сам обʼєкт).

1. **Коротка різниця**

| Функція | Receiver у блоці | Повертає         |
| ------- | ---------------- | ---------------- |
| `let`   | `it`             | результат lambda |
| `run`   | `this`           | результат lambda |
| `apply` | `this`           | сам обʼєкт       |

2. **let**

`let` використовують, коли треба взяти обʼєкт як `it` і повернути новий
результат:

```kotlin
val length: Int? = name?.let {
    it.length
}
```

Типові сценарії:

- null-safe block;
- трансформація значення;
- обмеження scope локальної змінної;
- коротка операція, де `it` достатньо зрозумілий.

3. **run**

`run` використовують, коли треба працювати з обʼєктом як `this` і повернути
результат:

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`run` зручний, коли всередині блоку кілька звернень до властивостей або методів
одного обʼєкта, і результатом має бути не сам обʼєкт, а обчислене значення.

4. **apply**

`apply` використовують для конфігурації обʼєкта. Він повертає сам обʼєкт:

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

Типові сценарії:

- builders;
- ініціалізація object properties;
- конфігурація UI/component objects;
- setup тестових обʼєктів.

5. **Головне питання при виборі**

Якщо потрібно повернути результат обчислення — дивитися на `let` або `run`. Якщо
потрібно повернути той самий обʼєкт після налаштування — `apply`.

```kotlin
val textLength = text?.let { it.length }

val label = user.run { "$firstName $lastName" }

val user = User().apply { name = "Ivan" }
```

6. **it vs this**

`let` використовує `it`, тому обʼєкт явно видно:

```kotlin
user.let {
    logger.info("User: ${it.id}")
}
```

`run` і `apply` використовують `this`, тому доступ до members коротший:

```kotlin
user.run {
    println(id)
    println(name)
}
```

Але `this` може стати неочевидним у вкладених scopes, особливо в Compose,
builders або DSL.

7. **Коли бути обережним**

- Не вкладати `let/run/apply` один в одного без сильної причини.
- Не використовувати `apply` для складної бізнес-логіки.
- Не використовувати `let`, якщо `it` стає незрозумілим.
- Якщо блок довгий, краще винести named function.

**Коротко:** `let` бере обʼєкт як `it` і повертає результат lambda. `run` бере
обʼєкт як `this` і повертає результат lambda. `apply` бере обʼєкт як `this`, але
повертає сам обʼєкт, тому найкраще підходить для конфігурації.

</details>

<details>
<summary>45. У чому різниця між map і flatMap?</summary>

#### Kotlin

`map` і `flatMap` — це оператори трансформації колекцій. `map` перетворює кожен
елемент у один результат. `flatMap` перетворює кожен елемент у колекцію
результатів, а потім “сплющує” всі ці колекції в одну.

1. **map**

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 }
```

Результат:

```kotlin
listOf(2, 4, 6)
```

`map` — це one-to-one трансформація: один вхідний елемент дає один вихідний
елемент.

2. **map, який повертає списки**

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
```

Результат буде вкладеним:

```kotlin
listOf(
    listOf('a', 'b'),
    listOf('c', 'd')
)
```

Тобто `map` не сплющує вкладені колекції.

3. **flatMap**

```kotlin
val words = listOf("ab", "cd")
val chars = words.flatMap { it.toList() }
```

Результат:

```kotlin
listOf('a', 'b', 'c', 'd')
```

`flatMap` — це one-to-many трансформація з flattening: один елемент може дати
нуль, один або багато елементів у результаті.

4. **Практичний приклад**

```kotlin
data class User(
    val name: String,
    val roles: List<String>
)

val allRoles = users.flatMap { it.roles }
```

Якщо в кожного користувача є список ролей, `flatMap` дозволяє отримати один
загальний список усіх ролей.

5. **map + flatten**

`flatMap` концептуально еквівалентний `map` плюс `flatten`:

```kotlin
val result = users
    .map { it.roles }
    .flatten()
```

Те саме коротше:

```kotlin
val result = users.flatMap { it.roles }
```

6. **Коли використовувати map**

- Коли кожен елемент перетворюється в один елемент.
- DTO mapping: `UserEntity -> User`.
- Formatting: `Int -> String`.
- Projection: `User -> user.name`.

```kotlin
val names = users.map { it.name }
```

7. **Коли використовувати flatMap**

- Коли кожен елемент має вкладену колекцію.
- Коли треба отримати один плоский список із багатьох списків.
- Для one-to-many mapping.
- Для обробки hierarchical/nested data.

```kotlin
val permissions = users.flatMap { it.permissions }
```

8. **Коли бути обережним**

- `flatMap` може швидко збільшити кількість елементів у результаті.
- Для великих pipeline-операцій варто подумати про `Sequence`.
- Якщо intent неочевидний, іноді `map { ... }.flatten()` читається краще для
  менш досвідченої команди.
- Для nullable значень іноді краще `mapNotNull`, а не `flatMap`.

**Коротко:** `map` перетворює `List<A>` у `List<B>`. `flatMap` перетворює `List<A>`
у `List<B>`, коли кожен `A` спочатку дає `Iterable<B>`, а потім результат
сплющується в одну колекцію.

</details>

<details>
<summary>46. Що таке sequence і коли їх використовувати?</summary>

#### Kotlin

`Sequence` у Kotlin — це лінива послідовність елементів. На відміну від
звичайних collection operations, які часто створюють проміжні колекції після
кожного кроку, sequence виконує pipeline поелементно й тільки тоді, коли
потрібен результат terminal operation.

1. **Звичайна колекція**

```kotlin
val result = users
    .filter { it.isActive }
    .map { it.name }
    .take(10)
```

Для `List` кожен крок зазвичай створює проміжну колекцію: після `filter`, після
`map`, і лише потім `take`.

2. **Sequence**

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Тут `filter`, `map`, `take` не виконуються одразу. Вони створюють lazy pipeline.
Реальна обробка починається тільки на terminal operation — у цьому випадку
`toList()`.

3. **Intermediate і terminal operations**

Intermediate operations повертають нову sequence:

- `map`
- `filter`
- `take`
- `drop`
- `flatMap`

Terminal operations запускають обчислення:

- `toList`
- `first`
- `firstOrNull`
- `count`
- `sum`
- `fold`
- `any`
- `all`

4. **Поелементна обробка**

Для sequence pipeline обробка відбувається не “весь filter, потім весь map”, а
по одному елементу через увесь pipeline:

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Як тільки знайдено перший результат, подальші елементи можуть не оброблятися.

5. **Коли використовувати Sequence**

- Великі колекції.
- Довгі chains із кількома `map/filter/flatMap`.
- Коли є short-circuit terminal operation: `first`, `any`, `take`.
- Коли треба уникнути проміжних колекцій.
- Коли дані можна обробляти поелементно.

6. **Коли не варто використовувати**

- Для маленьких колекцій: overhead sequence може бути більшим за вигоду.
- Для простого одного `map` або `filter`.
- Якщо код стає менш читабельним без помітної користі.
- Якщо потрібна асинхронна stream-like обробка — тоді краще `Flow`, а не
  `Sequence`.

7. **Sequence vs Iterable**

- `Iterable` operations на колекціях зазвичай eager.
- `Sequence` operations lazy.
- `Sequence` не є coroutine API.
- `Sequence` підходить для синхронної lazy-обробки.
- `Flow` підходить для асинхронної stream-обробки.

8. **Практична техлід-позиція**

Не треба механічно ставити `asSequence()` всюди. Для маленьких списків звичайний
`List` pipeline часто простіший і достатньо швидкий. `Sequence` виправданий,
коли є велика кількість елементів, довгий pipeline або short-circuit, який
реально економить роботу.

**Коротко:** `Sequence` — це lazy pipeline для синхронної обробки елементів. Його
варто використовувати для великих або складних chains, щоб уникати проміжних
колекцій і зайвої роботи, але не як автоматичну заміну всім collection
operations.

</details>

<details>
<summary>47. Що таке лінива (lazy) обробка колекцій?</summary>

#### Kotlin

Лінива обробка колекцій означає, що елементи обробляються не одразу після
кожного виклику `map`, `filter` чи іншого оператора, а тільки тоді, коли
результат справді потрібен. У Kotlin для цього найчастіше використовують
`Sequence`.

1. **Eager-обробка звичайних колекцій**

```kotlin
val result = users
    .filter { it.isActive }
    .map { it.name }
    .take(10)
```

Для звичайного `List` кожен крок зазвичай виконується одразу й створює проміжну
колекцію: спочатку список активних користувачів, потім список імен, потім перші
10 елементів.

2. **Lazy-обробка через Sequence**

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Після `asSequence()` оператори стають lazy. Pipeline описується одразу, але
реальна обробка починається тільки на terminal operation — тут це `toList()`.

3. **Поелементне виконання**

При lazy-підході Kotlin не обробляє всю колекцію на кожному етапі. Замість
цього один елемент проходить через увесь pipeline:

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Як тільки знайдено перший активний user, решту елементів можна не обробляти.
Це особливо корисно з `first`, `firstOrNull`, `any`, `all`, `take`.

4. **Intermediate і terminal operations**

Lazy intermediate operations:

- `map`
- `filter`
- `flatMap`
- `take`
- `drop`
- `distinct`

Terminal operations, які запускають обчислення:

- `toList`
- `toSet`
- `firstOrNull`
- `count`
- `fold`
- `any`
- `all`

5. **Коли lazy-обробка корисна**

- Велика колекція.
- Довгий chain операторів.
- Є short-circuit operation.
- Треба уникнути проміжних списків.
- Обробка може завершитися раніше, ніж буде переглянуто всі елементи.

6. **Коли lazy-обробка не потрібна**

- Маленькі колекції.
- Один простий `map` або `filter`.
- Код стає складнішим без реальної вигоди.
- Потрібна асинхронна обробка потоку даних — тоді краще `Flow`.

7. **Sequence не дорівнює Flow**

`Sequence` — синхронна lazy-обробка в поточному потоці. `Flow` — асинхронний
stream API на корутинах.

```kotlin
val sequence: Sequence<Int> = sequenceOf(1, 2, 3)
```

Якщо джерело даних асинхронне, наприклад network, database stream або events,
`Sequence` не є правильною абстракцією.

8. **Практична техлід-позиція**

Lazy-обробка — це оптимізація й інструмент читабельного pipeline, але не правило
для кожної колекції. Перед `asSequence()` варто розуміти, яку саме проблему він
вирішує: проміжні allocation, великий dataset або early termination.

**Коротко:** lazy-обробка колекцій у Kotlin означає, що pipeline виконується тільки
на terminal operation і зазвичай поелементно. Для цього використовують
`Sequence`; він корисний для великих або складних chain-операцій, але не
потрібен для кожного маленького списку.

</details>

<details>
<summary>48. Що таке корутини і чим вони відрізняються від потоків (threads)?</summary>

#### Kotlin

Корутини — це легковаговий механізм конкурентного й асинхронного виконання в
Kotlin. Вони дозволяють писати асинхронний код у послідовному стилі, без
callback hell, але при цьому не створюють окремий OS thread для кожної задачі.

1. **Базова ідея**

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Код виглядає послідовним, але `loadUser()` може бути `suspend`-функцією, яка
призупиняє корутину без блокування потоку.

2. **Корутина не дорівнює потік**

Thread — це ресурс операційної системи. Він дорогий: має stack, context switch і
обмежену кількість.

Coroutine — це легковагова задача, якою керує Kotlin coroutine runtime. Багато
корутин можуть виконуватись на невеликій кількості потоків.

3. **Suspension замість blocking**

```kotlin
delay(1000)
```

`delay()` не блокує thread. Вона призупиняє корутину, а thread може виконувати
іншу роботу.

На відміну від цього:

```kotlin
Thread.sleep(1000)
```

`Thread.sleep()` блокує потік повністю.

4. **Dispatchers**

Корутина виконується на dispatcher, який визначає, де саме буде виконуватись
код:

- `Dispatchers.Main` — UI thread в Android.
- `Dispatchers.IO` — blocking I/O: database, files, network clients без native
  suspend API.
- `Dispatchers.Default` — CPU-bound робота.
- custom dispatcher — спеціальні thread pools.

5. **Structured concurrency**

Корутини мають бути привʼязані до `CoroutineScope`. Scope визначає lifecycle,
cancellation і parent-child relationship.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

В Android `viewModelScope` автоматично скасовує корутини, коли `ViewModel`
очищається.

6. **Переваги корутин**

- Менше boilerplate для async-коду.
- Дешевші за threads.
- Природна cancellation model.
- Structured concurrency.
- Добре інтегруються з Flow.
- Послідовний стиль коду для асинхронних операцій.

7. **Що корутини не роблять автоматично**

- Не роблять CPU-bound код швидшим самі по собі.
- Не прибирають потребу правильно вибирати dispatcher.
- Не захищають від race conditions, якщо є shared mutable state.
- Не перетворюють blocking API на non-blocking магічно: blocking код треба
  запускати на правильному dispatcher.

8. **Типова помилка**

```kotlin
viewModelScope.launch {
    heavyCpuWork()
}
```

Якщо це виконується на `Dispatchers.Main`, UI може зависнути. CPU-bound роботу
треба переносити на `Dispatchers.Default`, а blocking I/O — на `Dispatchers.IO`.

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**Коротко:** корутини — це легковагові асинхронні задачі, які можуть
призупинятися без блокування потоку. Потоки — це нижчий і дорожчий OS-рівень
виконання. Корутини працюють поверх потоків і дозволяють ефективніше керувати
асинхронністю, lifecycle і cancellation.

</details>

<details>
<summary>49. Що таке suspend-функція?</summary>

#### Kotlin

`suspend`-функція — це функція, яка може призупинити виконання корутини без
блокування потоку. Вона не обовʼязково завжди призупиняється, але має право
мати suspension points, наприклад network call, database call, `delay()` або
перемикання контексту через `withContext`.

1. **Базовий приклад**

```kotlin
suspend fun loadUser(id: Long): User {
    return api.getUser(id)
}
```

Таку функцію можна викликати тільки з іншої `suspend`-функції або з корутини:

```kotlin
viewModelScope.launch {
    val user = loadUser(1L)
}
```

2. **Suspend не означає thread blocking**

```kotlin
suspend fun waitForData() {
    delay(1000)
}
```

`delay()` призупиняє корутину, але не блокує thread. Потік може виконувати інші
корутини або задачі.

Це відрізняється від:

```kotlin
Thread.sleep(1000)
```

`Thread.sleep()` блокує весь потік.

3. **Що відбувається під капотом**

Компілятор перетворює `suspend`-функцію на state machine і працює з
`Continuation`. Завдяки цьому виконання можна призупинити, зберегти стан і
продовжити пізніше.

На рівні коду це виглядає як звичайна послідовна функція, але runtime може
перервати й відновити її без блокування thread.

4. **Suspend не створює корутину**

Важливий момент: `suspend fun` сама по собі не запускає нову корутину.

```kotlin
suspend fun loadData(): Data {
    // ...
}
```

Це лише функція, яку можна виконувати в coroutine context. Запуск створюють
builders на кшталт `launch`, `async`, `runBlocking`.

5. **withContext**

Якщо всередині `suspend`-функції є blocking або CPU-bound робота, треба явно
вибрати dispatcher:

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

`suspend` не робить blocking API автоматично non-blocking. Воно лише дозволяє
правильно призупиняти й перемикати контекст.

6. **Де використовують suspend-функції**

- Repository methods.
- Network/database calls.
- Use cases/interactors.
- Long-running operations.
- API, які мають підтримувати cancellation.
- Корутинні wrappers над callback-based API.

7. **Cancellation**

Suspend-функції мають бути cancellation-friendly. Багато стандартних suspension
points, наприклад `delay()` або `withContext`, перевіряють cancellation
автоматично.

Для довгих CPU loops треба явно перевіряти стан:

```kotlin
coroutineContext.ensureActive()
```

8. **Практичні правила**

- Не викликати blocking I/O у suspend-функції без `Dispatchers.IO`.
- Не створювати нові scopes всередині suspend-функції без потреби.
- Не плутати `suspend` із “запустити асинхронно”.
- Робити suspend API послідовним і cancellation-friendly.
- Для паралельності всередині suspend-функції використовувати `coroutineScope`
  або `supervisorScope`, а не `GlobalScope`.

**Коротко:** `suspend`-функція — це функція, яку можна призупинити й відновити в
корутині без блокування потоку. Вона не створює корутину сама, а лише може
працювати в coroutine context і містити suspension points.

</details>

<details>
<summary>50. Що таке CoroutineScope і для чого він потрібен?</summary>

#### Kotlin

`CoroutineScope` — це контекст, у межах якого запускаються корутини. Він
визначає lifecycle корутин, їхній `Job`, dispatcher, cancellation і звʼязок
parent-child. Без scope корутина не має зрозумілого власника, а значить її
важко правильно скасувати й контролювати.

1. **Базовий приклад**

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    _state.value = user
}
```

`viewModelScope` — це `CoroutineScope`, привʼязаний до lifecycle `ViewModel`.
Коли `ViewModel` очищається, корутини в цьому scope скасовуються автоматично.

2. **Що містить CoroutineScope**

Концептуально scope має `coroutineContext`:

```kotlin
interface CoroutineScope {
    val coroutineContext: CoroutineContext
}
```

У цьому context можуть бути:

- `Job` або `SupervisorJob`;
- `Dispatcher`;
- `CoroutineName`;
- `CoroutineExceptionHandler`;
- інші context elements.

3. **Job як lifecycle**

`Job` відповідає за cancellation і parent-child relationship:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

Якщо скасувати scope:

```kotlin
scope.cancel()
```

усі дочірні корутини також будуть скасовані.

4. **Structured concurrency**

Scope забезпечує structured concurrency: корутини мають власника, і їхній
lifecycle не губиться.

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` не завершиться, поки не завершаться всі дочірні корутини.

5. **Android scopes**

У Android часто використовують готові scopes:

- `viewModelScope` — живе разом із `ViewModel`.
- `lifecycleScope` — привʼязаний до `LifecycleOwner`.
- `rememberCoroutineScope()` — scope для Compose, привʼязаний до composition.

Ці scopes краще, ніж створювати власні вручну без потреби.

6. **Чому не GlobalScope**

```kotlin
GlobalScope.launch {
    // risky
}
```

`GlobalScope` не привʼязаний до lifecycle екрана, use case або сервісу. Така
корутина може жити довше, ніж обʼєкт, який її запустив, що веде до leaks,
неочікуваних callbacks і складного тестування.

7. **Коли створювати власний scope**

Власний scope має сенс, якщо є чіткий власник lifecycle:

- application-level manager;
- long-running service;
- repository з явно керованим lifecycle;
- background component, який має `close()`/`cancel()`.

```kotlin
class SyncManager {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    fun start() {
        scope.launch {
            syncLoop()
        }
    }

    fun stop() {
        scope.cancel()
    }
}
```

8. **Практичні правила**

- Запускати корутини тільки в scope з понятним lifecycle.
- Не створювати scope всередині функції, якщо його ніхто не скасує.
- Для паралельної роботи всередині suspend-функції використовувати
  `coroutineScope` або `supervisorScope`.
- Уникати `GlobalScope` в application-коді.
- Передавати dispatcher через DI, якщо код треба тестувати.

**Коротко:** `CoroutineScope` — це власник корутин і їхнього lifecycle. Він задає
context, dispatcher, cancellation і parent-child звʼязки. Хороший Kotlin-код не
запускає корутини “в нікуди”, а привʼязує їх до зрозумілого scope.

</details>

<details>
<summary>51. У чому різниця між launch і async?</summary>

#### Kotlin

`launch` і `async` — це coroutine builders, які запускають нову корутину в
межах `CoroutineScope`. Головна різниця: `launch` запускає задачу без результату
і повертає `Job`, а `async` запускає задачу з результатом і повертає
`Deferred<T>`.

1. **launch**

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

`launch` використовують для fire-and-forget задач, де результат не потрібен
напряму. Через `Job` можна контролювати lifecycle:

```kotlin
job.cancel()
job.join()
```

`join()` чекає завершення, але не повертає результат.

2. **async**

```kotlin
val deferred: Deferred<User> = scope.async {
    repository.loadUser()
}

val user = deferred.await()
```

`async` використовують, коли корутина має повернути значення. `Deferred<T>` — це
`Job` із результатом, який отримують через `await()`.

3. **Паралельне виконання**

`async` часто використовують для паралельного завантаження незалежних даних:

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

Тут `loadUser()` і `loadOrders()` стартують паралельно, а `await()` чекає їхні
результати.

4. **Помилки**

У `launch` exception зазвичай одразу поширюється в parent scope:

```kotlin
scope.launch {
    error("Failed")
}
```

В `async` exception зберігається в `Deferred` і буде кинутий при `await()`:

```kotlin
val deferred = scope.async {
    error("Failed")
}

deferred.await() // exception тут
```

Але це не означає, що помилки в `async` можна ігнорувати: у structured
concurrency failure все одно впливає на parent/children відповідно до `Job` або
`SupervisorJob`.

5. **Коли використовувати launch**

- UI side effects.
- Запуск операції без прямого результату.
- Observing/collecting Flow.
- Logging, sync, fire-and-forget у scope з правильним lifecycle.

```kotlin
viewModelScope.launch {
    events.collect { event ->
        handle(event)
    }
}
```

6. **Коли використовувати async**

- Потрібен результат.
- Потрібно виконати кілька незалежних suspend-операцій паралельно.
- Потрібно явно дочекатися значення через `await()`.

7. **Типові помилки**

- Використовувати `async`, але ніколи не викликати `await()`.
- Використовувати `async` для fire-and-forget.
- Запускати `async` у `GlobalScope`.
- Робити паралельність там, де операції мають залежати одна від одної.

8. **Практичне правило**

Якщо результат не потрібен — `launch`. Якщо потрібен результат — `async`.
Якщо всередині `suspend`-функції треба паралельно виконати кілька операцій,
обгортати їх у `coroutineScope`, щоб зберегти structured concurrency.

**Коротко:** `launch` повертає `Job` і підходить для задач без результату. `async`
повертає `Deferred<T>` і підходить для задач із результатом, який отримують
через `await()`.

</details>

<details>
<summary>52. Що таке runBlocking і коли його використовують?</summary>

#### Kotlin

`runBlocking` — це coroutine builder, який запускає корутину і блокує поточний
потік, доки ця корутина не завершиться. Його головне призначення — зробити міст
між звичайним blocking-кодом і suspend-кодом.

1. **Що саме робить runBlocking**

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

Всередині `runBlocking` можна викликати `suspend`-функції. Але важлива деталь:
поточний thread буде заблокований до завершення всього coroutine scope.

Тобто це не просто "запустити корутину". Це "запустити корутину і синхронно
дочекатися її завершення".

2. **Чим runBlocking відрізняється від launch**

```kotlin
runBlocking {
    launch {
        delay(1_000)
        println("Done")
    }
}
```

`launch` створює дочірню корутину, але сам по собі не блокує thread.
`runBlocking` створює scope і блокує caller thread, поки всі дочірні корутини не
завершаться.

3. **Коли runBlocking використовують**

Нормальні сценарії використання:

- `main()` функція в CLI/консольних застосунках.
- Тести, коли потрібно викликати suspend-код із синхронного тестового методу.
- Приклади, документація, playground-код.
- Іноді — інтеграційний bridge у legacy blocking-коді.

```kotlin
@Test
fun loadsUser() = runBlocking {
    val user = repository.loadUser()

    assertEquals("John", user.name)
}
```

У сучасних coroutine-тестах краще використовувати `runTest`, але `runBlocking`
досі зустрічається в старіших тестах або простих інтеграційних сценаріях.

4. **Коли runBlocking не треба використовувати**

У production Android/UI-коді `runBlocking` майже завжди є поганим сигналом:

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Такий код блокує main thread. На Android це може призвести до:

- freeze UI;
- ANR;
- поганого UX;
- deadlock, якщо suspend-код очікує повернення на main dispatcher.

Правильніше:

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

5. **runBlocking і Dispatchers**

За замовчуванням `runBlocking` виконує корутину в поточному потоці:

```kotlin
runBlocking {
    println(Thread.currentThread().name)
}
```

Можна явно передати dispatcher:

```kotlin
runBlocking(Dispatchers.IO) {
    repository.loadFromDisk()
}
```

Але це не скасовує головну властивість: caller thread усе одно чекає завершення
`runBlocking`.

6. **Проблема вкладеного runBlocking**

Не треба викликати `runBlocking` всередині suspend-функцій:

```kotlin
suspend fun loadUser(): User {
    return runBlocking {
        repository.loadUser()
    }
}
```

Це ламає ідею non-blocking suspend-коду. Якщо функція вже `suspend`, їй не
потрібен `runBlocking`:

```kotlin
suspend fun loadUser(): User {
    return repository.loadUser()
}
```

7. **Практичне правило**

`runBlocking` варто сприймати як boundary API. Він потрібен на межі між світом,
де suspend ще не підтримується, і світом корутин. Якщо ти вже всередині
coroutine scope або suspend-функції — `runBlocking` майже точно не потрібен.

**Коротко:** `runBlocking` запускає корутину і блокує поточний thread до її
завершення. Його доречно використовувати в `main`, тестах або bridge-коді, але
не в UI/business production-коді, де потрібна неблокуюча конкурентність.

</details>
<details>
<summary>53. Що таке диспетчери (Dispatchers.IO, Default, Main)?</summary>

#### Kotlin

Диспетчер (`CoroutineDispatcher`) визначає, на якому потоці або пулі потоків
буде виконуватись корутина. Якщо `CoroutineScope` відповідає за lifecycle, то
dispatcher відповідає за execution context.

1. **Навіщо потрібні диспетчери**

Корутини самі по собі не означають "новий thread". Вони легковагові, але їхній
код усе одно має виконуватись на конкретному thread або thread pool.

```kotlin
viewModelScope.launch(Dispatchers.IO) {
    val user = repository.loadUser()
}
```

У цьому прикладі корутина запускається в `viewModelScope`, але її робота
виконується на dispatcher для I/O-операцій.

2. **Dispatchers.Main**

`Dispatchers.Main` використовується для роботи з UI-thread.

```kotlin
viewModelScope.launch(Dispatchers.Main) {
    state.value = UiState.Loading
}
```

На Android це main thread, де можна безпечно оновлювати UI-related state,
LiveData, Compose state або викликати UI API.

Типові сценарії:

- оновлення UI;
- зміна `StateFlow`/`LiveData`, які напряму впливають на екран;
- коротка логіка presentation layer;
- запуск корутин із `viewModelScope`, де `Main` часто є dispatcher за
  замовчуванням.

Важливо: на `Main` не можна виконувати важкі blocking-операції.

3. **Dispatchers.IO**

`Dispatchers.IO` оптимізований для blocking I/O:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}
```

Типові сценарії:

- HTTP-запити;
- робота з базою даних;
- читання/запис файлів;
- blocking SDK/API;
- disk/network I/O.

`Dispatchers.IO` має пул потоків, розрахований на те, що частина потоків буде
простоювати в очікуванні відповіді від мережі, диска або зовнішнього ресурсу.

4. **Dispatchers.Default**

`Dispatchers.Default` використовується для CPU-bound задач:

```kotlin
suspend fun calculateHash(input: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(input)
    }
```

Типові сценарії:

- сортування великих колекцій;
- парсинг великих JSON/XML, якщо це CPU-heavy;
- криптографія;
- image processing;
- складні обчислення;
- мапінг великих обсягів даних.

`Default` зазвичай має кількість потоків, близьку до кількості CPU cores. Його
не треба забивати blocking I/O, бо це погіршить виконання реальних
обчислювальних задач.

5. **withContext для перемикання dispatcher**

Найчастіше dispatcher перемикають через `withContext`:

```kotlin
suspend fun loadProfile(): Profile {
    val dto = withContext(Dispatchers.IO) {
        api.loadProfile()
    }

    return withContext(Dispatchers.Default) {
        mapper.map(dto)
    }
}
```

`withContext` не створює незалежну fire-and-forget корутину. Він перемикає
контекст виконання і повертає результат.

6. **Поганий приклад**

```kotlin
viewModelScope.launch {
    val bitmap = decodeHugeImage(file)
    state.value = UiState.Success(bitmap)
}
```

Якщо `decodeHugeImage()` важка CPU-операція, вона виконається на dispatcher
scope за замовчуванням. Для `viewModelScope` це зазвичай `Main`, тобто UI може
підвиснути.

Краще:

```kotlin
viewModelScope.launch {
    val bitmap = withContext(Dispatchers.Default) {
        decodeHugeImage(file)
    }

    state.value = UiState.Success(bitmap)
}
```

7. **Не хардкодити dispatcher у важкодоступному коді**

Для тестованості dispatcher часто інжектять:

```kotlin
class UserRepository(
    private val api: UserApi,
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun loadUser(): User = withContext(ioDispatcher) {
        api.loadUser()
    }
}
```

У production передають `Dispatchers.IO`, у тестах — test dispatcher. Це робить
код контрольованим і передбачуваним.

8. **Практичне правило**

- `Main` — UI і presentation layer.
- `IO` — мережа, база, файли, blocking I/O.
- `Default` — CPU-heavy обчислення.
- `withContext` — для явного перемикання dispatcher всередині `suspend`-коду.

**Коротко:** dispatcher визначає, де фізично виконується корутина. Правильний вибір
dispatcher захищає UI від блокувань, CPU pool — від I/O-заторів, а код — від
нестабільної продуктивності.

</details>
<details>
<summary>54. Як скасовувати корутини і обробляти помилки?</summary>

#### Kotlin

Скасування і обробка помилок у корутинах базуються на `Job`, structured
concurrency і cooperative cancellation. Хороший Kotlin-код не просто запускає
корутини, а керує їхнім lifecycle: знає, коли їх скасувати, як поширюються
exceptions і де правильно ловити помилки.

1. **Скасування через Job**

Кожна корутина має `Job`. Через нього можна скасувати корутину:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancel()
```

Якщо потрібно дочекатися фактичного завершення після cancel:

```kotlin
job.cancelAndJoin()
```

`cancel()` лише надсилає сигнал скасування. Корутина завершиться тоді, коли
дійде до cancellation point або сама перевірить стан.

2. **Скасування cooperative**

Корутини не вбиваються примусово як thread. Вони скасовуються кооперативно.
Suspending-функції з `kotlinx.coroutines` зазвичай cancellation-aware:

```kotlin
scope.launch {
    while (true) {
        delay(1_000)
        println("Working")
    }
}
```

`delay()` є cancellation point, тому така корутина нормально завершиться після
`cancel()`.

Але CPU-bound цикл треба перевіряти явно:

```kotlin
scope.launch(Dispatchers.Default) {
    while (isActive) {
        doSmallChunkOfWork()
    }
}
```

Або періодично викликати:

```kotlin
ensureActive()
```

3. **CancellationException не треба ковтати**

Скасування реалізоване через `CancellationException`. Її не треба обробляти як
звичайну помилку:

```kotlin
try {
    repository.loadData()
} catch (e: Exception) {
    // погано: тут можна випадково зловити CancellationException
}
```

Краще:

```kotlin
try {
    repository.loadData()
} catch (e: CancellationException) {
    throw e
} catch (e: IOException) {
    handleNetworkError(e)
}
```

Якщо проковтнути `CancellationException`, корутина може продовжити виконання
після скасування, що ламає lifecycle і structured concurrency.

4. **Обробка помилок у launch**

У `launch` exception поширюється в parent scope:

```kotlin
viewModelScope.launch {
    val user = repository.loadUser()
    state.value = UiState.Success(user)
}
```

Якщо `loadUser()` кине exception і його не зловити, корутина завершиться з
помилкою. У UI-коді зазвичай помилки ловлять всередині:

```kotlin
viewModelScope.launch {
    state.value = UiState.Loading

    try {
        val user = repository.loadUser()
        state.value = UiState.Success(user)
    } catch (e: CancellationException) {
        throw e
    } catch (e: Throwable) {
        state.value = UiState.Error(e.message ?: "Unknown error")
    }
}
```

5. **Обробка помилок в async**

`async` повертає `Deferred<T>`. Exception буде кинутий при `await()`:

```kotlin
val deferred = scope.async {
    repository.loadUser()
}

try {
    val user = deferred.await()
} catch (e: IOException) {
    handleNetworkError(e)
}
```

Але в structured concurrency помилка дочірньої корутини все одно може скасувати
parent scope. Тому `async` не треба використовувати як спосіб "сховати" помилку.

6. **CoroutineExceptionHandler**

`CoroutineExceptionHandler` ловить uncaught exceptions у root coroutine:

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.error(throwable)
}

scope.launch(handler) {
    error("Failed")
}
```

Важливий нюанс: handler не є заміною `try/catch` для бізнес-логіки. Він більше
схожий на останній рівень логування/репортингу. Для UI-state помилки краще
обробляти локально там, де є контекст, що показати користувачу.

7. **SupervisorJob і supervisorScope**

За замовчуванням помилка однієї child-корутини скасовує parent і сусідні
children:

```kotlin
coroutineScope {
    launch { loadA() }
    launch { loadB() } // якщо loadA впаде, loadB теж буде скасована
}
```

Якщо задачі незалежні, використовують supervisor-модель:

```kotlin
supervisorScope {
    launch {
        runCatching { loadA() }
            .onFailure { logger.error(it) }
    }

    launch {
        runCatching { loadB() }
            .onFailure { logger.error(it) }
    }
}
```

У supervisor scope failure однієї child-корутини не скасовує автоматично інші
children.

8. **Скасування при lifecycle**

В Android не треба вручну скасовувати `viewModelScope`: він скасовується при
`ViewModel.onCleared()`.

```kotlin
class UserViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadUser()
        }
    }
}
```

Для custom scope треба мати явне місце cleanup:

```kotlin
class SyncManager {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    fun start() {
        scope.launch { syncLoop() }
    }

    fun stop() {
        scope.cancel()
    }
}
```

9. **Практичне правило**

- Скасовуй через `Job.cancel()` або `scope.cancel()`.
- Не ковтай `CancellationException`.
- Для UI-операцій лови помилки локально і перетворюй їх у state.
- Для незалежних задач використовуй `SupervisorJob` або `supervisorScope`.
- Для CPU-heavy коду перевіряй `isActive` або `ensureActive()`.

**Коротко:** корутини скасовуються кооперативно через `Job`, а помилки
поширюються по ієрархії structured concurrency. Надійний код явно розділяє
business errors, cancellation і unexpected failures.

</details>
<details>
<summary>55. Що таке структурована конкурентність (structured concurrency)?</summary>

#### Kotlin

Structured concurrency — це підхід, у якому корутини мають чітку ієрархію:
кожна дочірня корутина привʼязана до parent scope, а її lifecycle, помилки і
скасування керуються через цю ієрархію. Ідея проста: якщо ми запустили async
роботу в певному контексті, вона не повинна жити довше за цей контекст.

1. **Проблема, яку вирішує structured concurrency**

Без структурованої конкурентності легко створити "загублену" роботу:

```kotlin
fun loadUser() {
    GlobalScope.launch {
        repository.loadUser()
    }
}
```

Така корутина не привʼязана до lifecycle екрана, use case або запиту. Якщо екран
закрився, користувач пішов, або parent operation була скасована — ця робота може
продовжити виконуватись.

Це створює ризики:

- витоки ресурсів;
- оновлення вже неактуального state;
- неконтрольовані network/database операції;
- складне тестування;
- помилки, які важко відстежити.

2. **Ієрархія Job**

У structured concurrency кожна корутина має `Job`, і дочірні корутини
підпорядковані parent `Job`.

```kotlin
viewModelScope.launch {
    launch { loadProfile() }
    launch { loadOrders() }
}
```

Тут дві внутрішні корутини є children зовнішньої корутини. Зовнішня корутина не
завершиться, доки її children не завершаться.

3. **Parent чекає children**

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

`coroutineScope` гарантує, що `loadScreen()` завершиться тільки після завершення
всіх дочірніх корутин. Якщо одна задача впаде — scope обробить це як частину
єдиної операції.

4. **Скасування поширюється вниз**

Якщо parent scope скасований, усі child-корутини також скасовуються:

```kotlin
val job = scope.launch {
    launch { syncUser() }
    launch { syncOrders() }
}

job.cancel()
```

Після `job.cancel()` обидві дочірні корутини отримають сигнал скасування. Це
особливо важливо в Android: коли `ViewModel` очищується, `viewModelScope`
скасовує всі активні корутини.

5. **Помилки поширюються вгору**

За замовчуванням exception у child-корутині скасовує parent scope:

```kotlin
coroutineScope {
    launch {
        error("Profile failed")
    }

    launch {
        loadOrders()
    }
}
```

Якщо перша child-корутина впаде, друга буде скасована. Це правильно для задач,
які є частинами однієї бізнес-операції: якщо одна критична частина не виконалась,
немає сенсу продовжувати решту.

6. **supervisorScope для незалежних задач**

Якщо дочірні задачі незалежні, використовують `supervisorScope`:

```kotlin
supervisorScope {
    launch {
        runCatching { loadProfile() }
            .onFailure { logger.error(it) }
    }

    launch {
        runCatching { loadRecommendations() }
            .onFailure { logger.error(it) }
    }
}
```

У `supervisorScope` failure однієї child-корутини не скасовує автоматично інші.
Це корисно для незалежних блоків екрана, де один failed widget не повинен
ламати весь screen.

7. **coroutineScope vs GlobalScope**

Погано:

```kotlin
suspend fun refresh() {
    GlobalScope.launch {
        repository.sync()
    }
}
```

Краще:

```kotlin
suspend fun refresh() = coroutineScope {
    launch {
        repository.sync()
    }
}
```

У другому варіанті робота є частиною виклику `refresh()`. Caller може скасувати
її, дочекатися завершення і отримати помилку, якщо вона сталася.

8. **Structured concurrency в Android**

Типові structured scopes:

- `viewModelScope` — живе стільки, скільки `ViewModel`;
- `lifecycleScope` — привʼязаний до `LifecycleOwner`;
- `coroutineScope` — створює локальний scope всередині suspend-функції;
- `supervisorScope` — локальний scope з незалежними child failures.

```kotlin
class UserViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            val data = repository.loadScreen()
            state.value = UiState.Success(data)
        }
    }
}
```

Коли `ViewModel` буде очищена, запущена робота буде скасована автоматично.

9. **Практичне правило**

- Не запускати корутини без owner-а.
- Уникати `GlobalScope` у production-коді.
- Для паралельної роботи всередині `suspend`-функції використовувати
  `coroutineScope`.
- Для незалежних child-задач використовувати `supervisorScope`.
- Давати кожній async-операції зрозумілий lifecycle.

**Коротко:** structured concurrency гарантує, що корутини не живуть самі по собі.
Вони мають parent scope, правильно скасовуються, передають помилки і не
перетворюються на неконтрольовану фонову роботу.

</details>
<details>
<summary>56. Чи можуть корутини виконуватись у будь-якому потоці?</summary>

#### Kotlin

Так, корутини можуть виконуватись на різних потоках, але не "самі по собі" і не
хаотично. Те, на якому thread або thread pool буде виконуватись корутина,
визначає її `CoroutineContext`, насамперед `CoroutineDispatcher`.

1. **Корутина не дорівнює thread**

Корутина — це легковагова одиниця конкурентності. Thread — це системний ресурс,
на якому реально виконується код.

```kotlin
viewModelScope.launch {
    println(Thread.currentThread().name)
}
```

Цей код виконається на dispatcher scope за замовчуванням. Для `viewModelScope`
це зазвичай `Dispatchers.Main`.

Ключова ідея: корутин може бути багато, а потоків — значно менше. Корутини
плануються на потоки диспетчером.

2. **Dispatcher визначає потік виконання**

```kotlin
scope.launch(Dispatchers.IO) {
    println(Thread.currentThread().name)
}
```

Тут корутина буде виконуватись на пулі потоків `Dispatchers.IO`.

```kotlin
scope.launch(Dispatchers.Default) {
    println(Thread.currentThread().name)
}
```

Тут — на CPU-oriented пулі `Dispatchers.Default`.

```kotlin
scope.launch(Dispatchers.Main) {
    println(Thread.currentThread().name)
}
```

Тут — на main/UI thread.

3. **Корутина може змінювати thread після suspend**

Корутина не гарантує, що весь її код виконається на одному й тому самому thread.
Після suspension вона може продовжитись на іншому thread того ж dispatcher:

```kotlin
withContext(Dispatchers.IO) {
    println("Before: ${Thread.currentThread().name}")
    delay(100)
    println("After: ${Thread.currentThread().name}")
}
```

Для `Dispatchers.IO` або `Dispatchers.Default` `Before` і `After` можуть бути
різними потоками. Це нормальна поведінка.

4. **Main dispatcher як виняток для UI**

На Android UI-код має виконуватись на main thread:

```kotlin
viewModelScope.launch(Dispatchers.Main) {
    state.value = UiState.Success(data)
}
```

Якщо потрібно зробити важку роботу, її треба винести з main thread:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }

    state.value = UiState.Success(result)
}
```

Тут I/O-робота виконується на `Dispatchers.IO`, а оновлення state — після
повернення в контекст `viewModelScope`.

5. **Thread-local дані можуть бути проблемою**

Оскільки корутина може продовжитись на іншому thread, не можна бездумно
покладатися на `ThreadLocal`:

```kotlin
val userIdThreadLocal = ThreadLocal<String>()
```

У звичайному blocking-коді `ThreadLocal` часто працює передбачувано. У
coroutine-коді це може зламатися, якщо не використовувати спеціальну підтримку
через coroutine context.

6. **Обмеження паралельності**

Те, що корутини можуть виконуватись на різних потоках, не означає, що кожна
корутина отримує окремий thread.

```kotlin
repeat(10_000) {
    launch {
        delay(1_000)
    }
}
```

Такий код може створити тисячі корутин без створення тисяч потоків. Саме тому
корутини дешевші за threads для великої кількості I/O-bound задач.

7. **Не блокувати dispatcher**

Погано:

```kotlin
viewModelScope.launch {
    Thread.sleep(2_000)
}
```

Якщо це виконується на `Main`, UI зависне. Якщо на `Default`, буде заблоковано
потік із CPU pool.

Краще:

```kotlin
viewModelScope.launch {
    delay(2_000)
}
```

Для blocking API треба використовувати відповідний dispatcher:

```kotlin
withContext(Dispatchers.IO) {
    blockingSdk.call()
}
```

8. **Практичне правило**

- Корутина виконується не "в повітрі", а на thread, який дає dispatcher.
- `Main` — для UI.
- `IO` — для blocking network/disk/database.
- `Default` — для CPU-heavy задач.
- Після suspension корутина може продовжитись на іншому thread.
- Не треба писати код, який залежить від стабільності конкретного thread, якщо
  це не гарантовано dispatcher-ом.

**Коротко:** корутини можуть виконуватись на різних потоках, але контроль над цим
йде через `CoroutineDispatcher`. Корутина — це не thread, а задача, яку
dispatcher планує на доступні потоки.

</details>
<details>
<summary>57. Що таке Flow у Kotlin?</summary>

#### Kotlin

`Flow` — це асинхронний потік значень у Kotlin Coroutines. Його використовують,
коли потрібно не просто отримати одне значення, а отримувати послідовність
значень у часі: стани екрана, події, результати пошуку, оновлення з бази,
network polling, stream даних.

1. **Базова ідея**

`suspend`-функція повертає одне значення:

```kotlin
suspend fun loadUser(): User
```

`Flow<T>` може повернути багато значень асинхронно:

```kotlin
fun observeUser(): Flow<User>
```

Наприклад, база даних може віддавати новий `User` щоразу, коли запис
оновлюється.

2. **Як створити Flow**

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
    emit(3)
}
```

`emit()` відправляє значення downstream-споживачу. Код всередині `flow {}` є
suspend-контекстом, тому там можна викликати suspend-функції:

```kotlin
fun observeProfile(): Flow<Profile> = flow {
    val profile = api.loadProfile()
    emit(profile)
}
```

3. **Як отримувати значення**

Щоб запустити `Flow`, його треба зібрати через `collect`:

```kotlin
viewModelScope.launch {
    repository.observeUser().collect { user ->
        state.value = UiState.Success(user)
    }
}
```

Без `collect` звичайний cold `Flow` не виконується.

4. **Flow за замовчуванням cold**

Cold flow починає виконуватись окремо для кожного collector-а:

```kotlin
val flow = flow {
    println("Started")
    emit(1)
}

flow.collect { println(it) }
flow.collect { println(it) }
```

`Started` буде надруковано двічі, бо кожен `collect` запускає flow заново.

Це важлива відмінність від hot streams типу `StateFlow` або `SharedFlow`.

5. **Оператори Flow**

`Flow` підтримує declarative pipeline через оператори:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { activeUsers -> activeUsers.isNotEmpty() }
    .collect { activeUsers ->
        state.value = UiState.Success(activeUsers)
    }
```

Поширені оператори:

- `map` — перетворення значення;
- `filter` — фільтрація;
- `onEach` — side effect;
- `catch` — обробка помилок upstream;
- `combine` — обʼєднання кількох flow;
- `flatMapLatest` — перемикання на новий flow при новому input;
- `collectLatest` — скасування попередньої обробки при новому значенні.

6. **Flow і context**

Для перемикання dispatcher у Flow використовують `flowOn`:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(api.loadData())
    }.flowOn(Dispatchers.IO)
```

`flowOn` змінює context для upstream-частини flow. Collector при цьому може
залишатися на іншому dispatcher, наприклад на `Main`.

7. **Обробка помилок**

```kotlin
repository.observeData()
    .catch { throwable ->
        emit(Data.Empty)
    }
    .collect { data ->
        render(data)
    }
```

`catch` ловить exception з upstream. Але помилки, які сталися всередині
`collect`, він не перехоплює:

```kotlin
flow
    .catch { handle(it) }
    .collect {
        error("Collector failed") // catch вище це не зловить
    }
```

8. **Flow в Android**

Типовий приклад у `ViewModel`:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    val state: StateFlow<UiState> =
        repository.observeUser()
            .map<User, UiState> { UiState.Success(it) }
            .catch { emit(UiState.Error) }
            .stateIn(
                scope = viewModelScope,
                started = SharingStarted.WhileSubscribed(5_000),
                initialValue = UiState.Loading
            )
}
```

Тут `Flow<User>` перетворюється у `StateFlow<UiState>`, який зручно спостерігати
з UI.

9. **Коли використовувати Flow**

`Flow` доречний, коли:

- значень може бути більше одного;
- дані змінюються в часі;
- потрібна reactive pipeline;
- потрібна cancellation-aware асинхронна обробка;
- треба комбінувати кілька джерел даних;
- потрібно інтегрувати database/network/domain streams.

Якщо потрібне лише одне значення — достатньо `suspend`-функції.

10. **Практичне правило**

- `suspend fun` — одне асинхронне значення.
- `Flow<T>` — багато асинхронних значень у часі.
- `collect` запускає cold flow.
- `flowOn` змінює dispatcher для upstream.
- `catch` обробляє upstream-помилки.

**Коротко:** `Flow` — це coroutine-native API для асинхронних потоків даних. Він
дозволяє описувати pipeline значень декларативно, без callback hell і з
нормальною підтримкою cancellation та structured concurrency.

</details>
<details>
<summary>58. У чому різниця між Flow, StateFlow та SharedFlow?</summary>

#### Kotlin

`Flow`, `StateFlow` і `SharedFlow` — це API для асинхронних потоків даних, але
вони вирішують різні задачі. Якщо коротко: `Flow` зазвичай cold stream,
`StateFlow` — hot stream зі станом і останнім значенням, `SharedFlow` — hot
stream для подій або broadcast-сценаріїв.

1. **Flow**

`Flow<T>` — це асинхронний потік значень, який за замовчуванням є cold.

```kotlin
fun observeUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

Cold означає, що код всередині flow не виконується, доки хтось не викличе
`collect`:

```kotlin
val usersFlow = repository.observeUsers()

usersFlow.collect { users ->
    println(users)
}
```

Кожен collector зазвичай запускає flow заново. Це добре для pipeline-ів,
запитів, трансформацій і роботи з джерелами, які мають виконуватись під час
collection.

2. **StateFlow**

`StateFlow<T>` — це hot flow, який завжди має поточне значення.

```kotlin
private val _state = MutableStateFlow<UiState>(UiState.Loading)
val state: StateFlow<UiState> = _state.asStateFlow()

fun update(user: User) {
    _state.value = UiState.Success(user)
}
```

Його головна модель — state holder. Новий collector одразу отримує останнє
значення:

```kotlin
viewModel.state.collect { state ->
    render(state)
}
```

`StateFlow` добре підходить для UI-state:

- loading;
- content;
- error;
- selected item;
- filter state;
- screen model.

3. **SharedFlow**

`SharedFlow<T>` — це hot flow для broadcast-подій. Він не зобовʼязаний мати
поточний стан.

```kotlin
private val _events = MutableSharedFlow<UiEvent>()
val events: SharedFlow<UiEvent> = _events.asSharedFlow()

suspend fun showError() {
    _events.emit(UiEvent.ShowSnackbar("Something went wrong"))
}
```

`SharedFlow` корисний для one-time events:

- snackbar;
- navigation;
- toast;
- analytics event;
- одноразові UI-команди;
- broadcast між кількома subscribers.

4. **Hot vs cold**

`Flow` зазвичай cold:

```kotlin
val flow = flow {
    println("Started")
    emit(1)
}
```

Кожен `collect` стартує виконання заново.

`StateFlow` і `SharedFlow` — hot:

```kotlin
private val state = MutableStateFlow(0)
private val events = MutableSharedFlow<String>()
```

Вони існують незалежно від collectors. Значення можна оновлювати навіть тоді,
коли зараз ніхто не слухає.

5. **StateFlow зберігає останнє значення**

```kotlin
val state = MutableStateFlow(0)

state.value = 1
state.value = 2
```

Новий collector одразу отримає `2`.

Це ключова відмінність від звичайного `SharedFlow`, який без replay не
обовʼязково віддасть минулі events новому collector-у.

6. **SharedFlow і replay**

`SharedFlow` можна налаштувати через `replay`:

```kotlin
val shared = MutableSharedFlow<String>(replay = 1)

shared.emit("Last event")
```

Новий collector отримає останнє replay-значення. Але якщо потрібна саме модель
поточного стану, зазвичай краще використовувати `StateFlow`, а не
`SharedFlow(replay = 1)`.

7. **Типовий ViewModel-приклад**

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    private val _state = MutableStateFlow<UiState>(UiState.Loading)
    val state: StateFlow<UiState> = _state.asStateFlow()

    private val _events = MutableSharedFlow<UiEvent>()
    val events: SharedFlow<UiEvent> = _events.asSharedFlow()

    fun load() {
        viewModelScope.launch {
            try {
                val user = repository.loadUser()
                _state.value = UiState.Success(user)
            } catch (e: Throwable) {
                _state.value = UiState.Error
                _events.emit(UiEvent.ShowSnackbar("Load failed"))
            }
        }
    }
}
```

Тут `StateFlow` тримає стан екрана, а `SharedFlow` відправляє одноразову подію.

8. **stateIn і shareIn**

Звичайний `Flow` можна перетворити на `StateFlow`:

```kotlin
val state: StateFlow<UiState> =
    repository.observeUser()
        .map { UiState.Success(it) }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

Або на `SharedFlow`:

```kotlin
val shared: SharedFlow<Data> =
    repository.observeData()
        .shareIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(),
            replay = 0
        )
```

`stateIn` використовують, коли потрібен state. `shareIn` — коли потрібно
поширити один upstream flow між кількома collectors.

9. **Коли що використовувати**

- `Flow` — для cold pipeline, domain streams, repository APIs.
- `StateFlow` — для UI-state і будь-якого "поточного значення".
- `SharedFlow` — для events, broadcast і one-time signals.

Поганий сигнал — використовувати `StateFlow` для navigation event. Якщо
користувач повернеться на екран або UI пересабскрайбиться, старий navigation
state може повторитись. Для таких речей краще `SharedFlow`.

10. **Практичне правило**

Якщо питаєш "який зараз стан?" — це `StateFlow`.
Якщо питаєш "які значення приходять у pipeline?" — це `Flow`.
Якщо питаєш "яку подію треба розіслати subscribers?" — це `SharedFlow`.

**Коротко:** `Flow` — cold stream для асинхронних значень, `StateFlow` — hot stream
із поточним станом, `SharedFlow` — hot stream для подій і broadcast-сценаріїв.

</details>
<details>
<summary>59. У чому різниця між Flow і LiveData?</summary>

#### Kotlin

`Flow` і `LiveData` обидва можуть передавати значення в часі, але це різні
інструменти. `LiveData` — Android lifecycle-aware observable holder. `Flow` —
частина Kotlin Coroutines для асинхронних stream-ів, яка не привʼязана до
Android і краще підходить для domain/data layers.

1. **LiveData**

`LiveData<T>` зберігає значення і повідомляє active observers:

```kotlin
private val _user = MutableLiveData<User>()
val user: LiveData<User> = _user

fun update(user: User) {
    _user.value = user
}
```

Головна перевага `LiveData` — lifecycle awareness:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

Observer отримує оновлення лише коли `LifecycleOwner` у відповідному active
state. Це зручно для класичного Android View-based UI.

2. **Flow**

`Flow<T>` — це асинхронний stream значень:

```kotlin
fun observeUser(): Flow<User> =
    userDao.observeUser()
```

Він не залежить від Android lifecycle і може використовуватись у будь-якому
Kotlin-коді: backend, CLI, shared KMM module, repository, use case.

```kotlin
viewModelScope.launch {
    repository.observeUser().collect { user ->
        render(user)
    }
}
```

3. **Lifecycle**

`LiveData` має lifecycle-awareness вбудовано.

`Flow` сам по собі lifecycle не знає. В Android його треба збирати правильно:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

`repeatOnLifecycle` запускає collection, коли lifecycle входить у потрібний
стан, і скасовує collection, коли виходить.

4. **Threading**

У `LiveData` є обмеження навколо main thread:

```kotlin
_user.value = user      // main thread
_user.postValue(user)   // background thread
```

У `Flow` threading контролюється coroutine context і dispatcher-ами:

```kotlin
repository.observeUser()
    .flowOn(Dispatchers.IO)
    .collect { user ->
        render(user)
    }
```

Це більш гнучко і краще вкладається в coroutine architecture.

5. **Оператори**

`Flow` має багатий набір операторів:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsRepository.observeSettings()) { users, settings ->
        buildUiState(users, settings)
    }
    .catch { emit(UiState.Error) }
```

У `LiveData` теж є трансформації, наприклад `map` і `switchMap`, але вони
обмеженіші й менш зручні для складних asynchronous pipelines.

6. **Cold vs hot**

Звичайний `Flow` зазвичай cold:

```kotlin
val flow = flow {
    emit(api.loadUser())
}
```

Він стартує при `collect`.

`LiveData` більше схожий на hot observable holder: він має останнє значення і
живе як обʼєкт незалежно від конкретного observer-а.

Якщо в `Flow` потрібна модель state holder, зазвичай використовують
`StateFlow`:

```kotlin
val state: StateFlow<UiState> =
    repository.observeUser()
        .map { UiState.Success(it) }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

7. **Data/domain layer**

У сучасній Kotlin/Android архітектурі repository і use case краще не повинні
залежати від Android API:

```kotlin
interface UserRepository {
    fun observeUser(): Flow<User>
}
```

Це краще, ніж:

```kotlin
interface UserRepository {
    fun observeUser(): LiveData<User>
}
```

Причина проста: `Flow` — Kotlin API, а `LiveData` — Android API. Domain/data
layer з `Flow` легше тестувати, переносити і використовувати поза Android UI.

8. **Interop**

Можна конвертувати `Flow` у `LiveData`:

```kotlin
val userLiveData: LiveData<User> =
    repository.observeUser().asLiveData()
```

І `LiveData` у `Flow`:

```kotlin
val userFlow: Flow<User> =
    userLiveData.asFlow()
```

Це корисно під час міграції зі старого коду на coroutine/Flow-based підхід.

9. **Що краще для нового коду**

Для нового Kotlin/Android коду зазвичай краще:

- repository/use case — `Flow`;
- ViewModel state — `StateFlow`;
- one-time events — `SharedFlow`;
- legacy XML/View UI може продовжувати використовувати `LiveData`, якщо проєкт
  уже побудований навколо нього.

У Jetpack Compose `StateFlow` зазвичай природніший вибір, ніж `LiveData`.

10. **Практичне правило**

`LiveData` — хороший lifecycle-aware holder для класичного Android UI.
`Flow` — більш універсальний, coroutine-native stream API для data/domain
логіки і складних asynchronous pipelines.

**Коротко:** `LiveData` привʼязаний до Android lifecycle, а `Flow` — до Kotlin
Coroutines. У сучасній архітектурі `Flow` краще тримати в data/domain layers, а
в UI перетворювати його на `StateFlow`, `SharedFlow` або `LiveData` залежно від
потреб проєкту.

</details>
<details>
<summary>60. У чому різниця між холодними та гарячими потоками?</summary>

#### Kotlin

Cold і hot streams відрізняються тим, коли починається виконання потоку і чи
залежить він від наявності collectors. Cold stream стартує для кожного collector
окремо. Hot stream існує незалежно від collectors і може емiтити значення навіть
тоді, коли його ніхто не слухає.

1. **Cold stream**

Звичайний `Flow` за замовчуванням cold:

```kotlin
val coldFlow = flow {
    println("Started")
    emit(api.loadUser())
}
```

Код всередині `flow {}` не виконається, доки не буде `collect`:

```kotlin
coldFlow.collect { user ->
    println(user)
}
```

Якщо викликати `collect` двічі, flow зазвичай виконається двічі:

```kotlin
coldFlow.collect { println("First: $it") }
coldFlow.collect { println("Second: $it") }
```

Тобто кожен collector отримує власне виконання upstream.

2. **Hot stream**

Hot stream живе незалежно від collectors:

```kotlin
val state = MutableStateFlow(0)

state.value = 1
state.value = 2
```

Значення оновлюються навіть якщо зараз немає активного collector-а. Коли новий
collector підпишеться на `StateFlow`, він одразу отримає останнє значення.

Інший приклад hot stream — `SharedFlow`:

```kotlin
val events = MutableSharedFlow<UiEvent>()

viewModelScope.launch {
    events.emit(UiEvent.ShowSnackbar("Saved"))
}
```

3. **Ключова різниця в execution**

Cold:

```kotlin
fun loadUserFlow(): Flow<User> = flow {
    emit(api.loadUser())
}
```

Кожен collector може викликати `api.loadUser()` заново.

Hot:

```kotlin
private val _state = MutableStateFlow<UiState>(UiState.Loading)
val state: StateFlow<UiState> = _state.asStateFlow()
```

`state` не запускає запит сам по собі при кожному collector-і. Він просто
тримає актуальне значення і віддає його підписникам.

4. **Аналогія**

Cold stream схожий на функцію:

```kotlin
suspend fun loadUser(): User
```

Коли викликаєш — тоді виконується.

Hot stream схожий на радіостанцію або observable state: вона існує незалежно
від того, хто зараз слухає.

5. **StateFlow як hot state**

`StateFlow` завжди має initial value:

```kotlin
private val _state = MutableStateFlow<UiState>(UiState.Loading)
val state: StateFlow<UiState> = _state.asStateFlow()
```

Це правильний вибір для UI-state:

- екран зараз loading;
- дані завантажені;
- сталася помилка;
- користувач вибрав фільтр;
- змінився поточний screen model.

Новий subscriber одразу бачить актуальний стан.

6. **SharedFlow як hot event stream**

`SharedFlow` не зобовʼязаний мати поточне значення:

```kotlin
private val _events = MutableSharedFlow<UiEvent>()
val events: SharedFlow<UiEvent> = _events.asSharedFlow()
```

Це підходить для events:

- navigation;
- snackbar;
- toast;
- analytics;
- one-time command.

Якщо `replay = 0`, новий collector не отримає старі events.

7. **Перетворення cold у hot**

Cold `Flow` можна перетворити в hot через `stateIn`:

```kotlin
val state: StateFlow<UiState> =
    repository.observeUser()
        .map { UiState.Success(it) }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

Або через `shareIn`:

```kotlin
val shared: SharedFlow<Data> =
    repository.observeData()
        .shareIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(),
            replay = 0
        )
```

Це корисно, коли не хочеться запускати expensive upstream окремо для кожного
collector-а.

8. **SharingStarted**

При `stateIn`/`shareIn` важливо вибрати policy:

```kotlin
SharingStarted.WhileSubscribed(5_000)
```

Це означає: upstream активний, поки є subscribers, і ще короткий час після
відписки. Такий режим часто добре підходить для Android UI.

Інші варіанти:

- `Eagerly` — стартувати одразу;
- `Lazily` — стартувати при першому subscriber-і;
- `WhileSubscribed` — працювати, поки є активні subscribers.

9. **Типові помилки**

- Очікувати, що cold `Flow` виконається без `collect`.
- Робити expensive network call у cold flow і випадково запускати його для
  кожного collector-а.
- Використовувати `StateFlow` для one-time events.
- Використовувати `SharedFlow` як state holder без сильної причини.
- Не враховувати `replay` у `SharedFlow`.

10. **Практичне правило**

- Cold `Flow` — pipeline, який стартує при collection.
- Hot `StateFlow` — актуальний state з останнім значенням.
- Hot `SharedFlow` — події або shared broadcast.
- `stateIn`/`shareIn` — перетворення cold flow у hot flow.

**Коротко:** cold stream виконується під кожного collector-а, hot stream існує
незалежно від collectors. У Kotlin звичайний `Flow` зазвичай cold, а
`StateFlow` і `SharedFlow` — hot.

</details>
<details>
<summary>61. Що таке collectLatest і коли його використовувати?</summary>

#### Kotlin

`collectLatest` — це terminal operator для `Flow`, який збирає значення, але
скасовує обробку попереднього значення, якщо приходить нове. Його використовують
там, де актуальним є тільки останній emission, а стару роботу вже немає сенсу
доводити до кінця.

1. **Базова різниця між collect і collectLatest**

`collect` обробляє кожне значення до кінця:

```kotlin
flowOf(1, 2, 3).collect { value ->
    delay(1_000)
    println(value)
}
```

Кожен `value` буде оброблений послідовно.

`collectLatest` скасовує попередній block, якщо приходить нове значення:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value)
}
```

Якщо нові значення приходять швидше, ніж завершується обробка, старі обробки
будуть скасовані.

2. **Навіщо це потрібно**

Типовий приклад — пошук по тексту. Користувач швидко вводить символи:

```text
k
ko
kot
kotl
kotlin
```

Немає сенсу доводити до кінця запит для `ko`, якщо вже є `kotlin`.

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Якщо прийде новий `query`, попередній пошук буде скасований.

3. **Скасування має бути cooperative**

`collectLatest` скасовує попередній block через coroutine cancellation. Це
працює коректно, якщо код всередині cancellation-aware:

```kotlin
collectLatest { query ->
    val result = api.search(query) // suspend, cancellation-aware
    render(result)
}
```

Якщо всередині blocking API, яке не реагує на cancellation, `collectLatest` не
зможе миттєво його зупинити:

```kotlin
collectLatest {
    Thread.sleep(5_000) // погано
}
```

Краще використовувати suspend API або виконувати blocking-операцію на
`Dispatchers.IO`, розуміючи її обмеження.

4. **Приклад з UI rendering**

```kotlin
viewModel.state.collectLatest { state ->
    renderExpensiveState(state)
}
```

Якщо rendering або side-effect довгий, а state оновлюється часто, старий render
можна скасувати і перейти до нового state.

Але якщо кожне значення важливе, `collectLatest` не підходить.

5. **Коли collectLatest підходить**

Добрі сценарії:

- live search;
- autocomplete;
- завантаження preview для останнього input;
- rendering тільки останнього state;
- запити, де старий результат стає неактуальним;
- обробка швидких UI input changes.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest { query ->
        repository.searchFlow(query)
    }
    .collectLatest { result ->
        state.value = UiState.Success(result)
    }
```

6. **Коли collectLatest не підходить**

Не треба використовувати `collectLatest`, якщо важливо обробити кожне значення:

- логування кожної події;
- analytics events;
- фінансові транзакції;
- черга задач;
- збереження кожної зміни;
- message processing.

Погано:

```kotlin
paymentsFlow.collectLatest { payment ->
    paymentRepository.process(payment)
}
```

Якщо прийде новий payment, попередня обробка може бути скасована. Для таких
сценаріїв потрібен `collect`, queue або окрема модель надійної доставки.

7. **collectLatest vs flatMapLatest**

`collectLatest` скасовує block collector-а:

```kotlin
flow.collectLatest { value ->
    process(value)
}
```

`flatMapLatest` перемикає upstream flow:

```kotlin
queryFlow.flatMapLatest { query ->
    repository.searchFlow(query)
}
```

Часто їх використовують у схожих сценаріях, але на різних рівнях pipeline.
`flatMapLatest` краще, коли кожен input створює новий flow. `collectLatest` —
коли треба скасувати саме обробку останнього emitted value.

8. **Обробка помилок**

```kotlin
queryFlow
    .debounce(300)
    .distinctUntilChanged()
    .mapLatest { query ->
        repository.search(query)
    }
    .catch { throwable ->
        emit(emptyList())
    }
    .collectLatest { result ->
        state.value = UiState.Success(result)
    }
```

Для search-сценаріїв часто зручні `mapLatest` або `flatMapLatest`, а
`collectLatest` залишають для останнього UI handling.

9. **Практичне правило**

Використовуй `collect`, коли кожне значення має бути оброблене.
Використовуй `collectLatest`, коли старе значення стає неактуальним після появи
нового.

**Коротко:** `collectLatest` збирає flow, але при новому emission скасовує обробку
попереднього. Це правильний інструмент для UI input, search, preview і
сценаріїв, де потрібен тільки найсвіжіший результат.

</details>
<details>
<summary>62. Як перетворити callback-based API у suspend-функцію?</summary>

#### Kotlin

Callback-based API можна перетворити у suspend-функцію через `suspendCoroutine` або `suspendCancellableCoroutine`. У production частіше потрібен cancellable варіант, щоб корутина могла коректно скасуватись.

1. **Базовий callback API**

```kotlin
interface LocationClient {
    fun getLocation(callback: Callback)

    interface Callback {
        fun onSuccess(location: Location)
        fun onError(error: Throwable)
    }
}
```

Мета — зробити так:

```kotlin
val location = locationClient.awaitLocation()
```

2. **suspendCoroutine**

```kotlin
suspend fun LocationClient.awaitLocation(): Location =
    suspendCoroutine { continuation ->
        getLocation(object : LocationClient.Callback {
            override fun onSuccess(location: Location) {
                continuation.resume(location)
            }

            override fun onError(error: Throwable) {
                continuation.resumeWithException(error)
            }
        })
    }
```

Це працює, але не обробляє cancellation.

3. **suspendCancellableCoroutine**

Краще:

```kotlin
suspend fun LocationClient.awaitLocation(): Location =
    suspendCancellableCoroutine { continuation ->
        val callback = object : LocationClient.Callback {
            override fun onSuccess(location: Location) {
                if (continuation.isActive) {
                    continuation.resume(location)
                }
            }

            override fun onError(error: Throwable) {
                if (continuation.isActive) {
                    continuation.resumeWithException(error)
                }
            }
        }

        getLocation(callback)

        continuation.invokeOnCancellation {
            removeCallback(callback)
        }
    }
```

Якщо coroutine скасували, callback треба відписати або cancel-ити request.

4. **Чому важлива cancellation**

Якщо не прибрати callback після cancellation, можна отримати:

- memory leak;
- callback у вже закритий screen;
- подвійний resume;
- зайву роботу після cancellation.

5. **Не resume двічі**

Continuation можна resume-ити тільки один раз. Якщо callback API може викликати success/error кілька разів, треба захиститись:

```kotlin
if (continuation.isActive) {
    continuation.resume(value)
}
```

Для multi-shot callbacks suspend-функція не підходить — краще `Flow`.

6. **Callback API з listener**

Якщо API має register/unregister:

```kotlin
suspend fun Sensor.awaitFirstValue(): Value =
    suspendCancellableCoroutine { continuation ->
        val listener = object : SensorListener {
            override fun onValue(value: Value) {
                unregister(this)
                continuation.resume(value)
            }
        }

        register(listener)

        continuation.invokeOnCancellation {
            unregister(listener)
        }
    }
```

7. **Multi-shot callback у Flow**

Якщо callback повертає багато значень, треба `callbackFlow`:

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

Suspend-функція — для одного результату. Flow — для потоку результатів.

8. **Error handling**

Callback error треба перетворювати в exception:

```kotlin
continuation.resumeWithException(error)
```

А на рівні use case/repository вже вирішувати: ловити exception, повертати `Result`, sealed result або показувати error state.

9. **Практичне правило**

- Один результат — `suspendCancellableCoroutine`.
- Багато результатів — `callbackFlow`.
- На cancellation — unregister/cancel request.
- Не resume-ити continuation двічі.
- Error callback — `resumeWithException`.
- Не використовувати `GlobalScope` для обгортання callback API.

**Коротко:** callback-based API перетворюють у suspend-функцію через `suspendCancellableCoroutine`: success викликає `resume`, error — `resumeWithException`, а cancellation — unregister/cancel callback. Якщо callback багаторазовий, треба використовувати `callbackFlow`, а не suspend-функцію.

</details>
<details>
<summary>63. У чому різниця між Channel і Flow?</summary>

#### Kotlin

`Channel` і `Flow` обидва можуть передавати значення між корутинами, але мають
різну модель. `Channel` — це coroutine primitive для комунікації між
producer-ом і consumer-ом. `Flow` — це декларативний stream API для асинхронних
послідовностей даних.

1. **Channel**

`Channel<T>` схожий на асинхронну чергу:

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

Producer відправляє значення через `send`, consumer отримує через `receive` або
`for (value in channel)`.

2. **Flow**

`Flow<T>` описує stream значень:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

`Flow` краще підходить для data pipeline: трансформації, фільтрація,
комбінування, обробка помилок, dispatcher control.

3. **Push vs declarative pipeline**

`Channel` — більш низькорівневий push-based механізм:

```kotlin
channel.send(event)
```

Ти явно керуєш відправкою, закриттям, buffering і receiving.

`Flow` — декларативний:

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .catch { emit(emptyList()) }
    .collect { users -> render(users) }
```

Тут ти описуєш, як дані мають пройти через pipeline.

4. **Cold/Hot модель**

Звичайний `Flow` за замовчуванням cold:

```kotlin
val flow = flow {
    emit(loadData())
}
```

Він стартує при `collect`.

`Channel` — hot primitive. Якщо producer відправляє дані, вони йдуть у channel
незалежно від того, як ти описав pipeline. Якщо consumer не читає, поведінка
залежить від buffer capacity.

5. **Backpressure**

У `Channel` backpressure проявляється напряму:

```kotlin
val channel = Channel<Int>(capacity = 0)

channel.send(1) // може suspend-итись, поки consumer не прийме значення
```

Capacity впливає на поведінку:

- `RENDEZVOUS`/`0` — send чекає receive;
- `BUFFERED` — є буфер;
- `CONFLATED` — зберігається тільки останнє значення;
- `UNLIMITED` — необмежений буфер, небезпечно без контролю.

У `Flow` backpressure зазвичай керується операторно:

```kotlin
flow
    .buffer()
    .conflate()
    .collectLatest { value ->
        process(value)
    }
```

6. **Коли використовувати Channel**

`Channel` доречний, коли потрібна coroutine-to-coroutine комунікація:

- worker queue;
- actor-like модель;
- fan-in/fan-out;
- ручне керування producer/consumer;
- одноразова передача задач між корутинами;
- низькорівнева синхронізація.

Приклад worker queue:

```kotlin
val tasks = Channel<Task>(capacity = Channel.BUFFERED)

repeat(4) {
    launch {
        for (task in tasks) {
            process(task)
        }
    }
}

tasks.send(Task.SyncUser)
```

7. **Коли використовувати Flow**

`Flow` доречний для stream-ів даних у application architecture:

- repository streams;
- database observations;
- UI state;
- search queries;
- data transformations;
- combining multiple data sources;
- reactive pipelines.

```kotlin
fun observeScreenState(): Flow<UiState> =
    combine(
        userRepository.observeUser(),
        settingsRepository.observeSettings()
    ) { user, settings ->
        UiState.Content(user, settings)
    }
```

Для більшості application-level stream-ів `Flow` краще, ніж `Channel`, бо API
вище рівнем і безпечніший для composition.

8. **Channel.receiveAsFlow**

Channel можна перетворити у Flow:

```kotlin
val channel = Channel<UiEvent>()
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

Але це треба робити обережно. `receiveAsFlow()` розподіляє значення між
collectors, а не broadcast-ить кожне значення всім collectors. Якщо потрібен
broadcast events, частіше краще `SharedFlow`.

9. **callbackFlow**

`callbackFlow` всередині використовує channel-like API:

```kotlin
fun observeLocation(): Flow<Location> = callbackFlow {
    val listener = object : LocationListener {
        override fun onLocationChanged(location: Location) {
            trySend(location)
        }
    }

    client.addListener(listener)

    awaitClose {
        client.removeListener(listener)
    }
}
```

Ззовні це `Flow`, а всередині є bridge для callback API. Це хороший приклад:
низькорівневу channel-модель ховаємо за зручним Flow API.

10. **Типові помилки**

- Використовувати `Channel` там, де достатньо `Flow`.
- Забути закрити channel, якщо це потрібно.
- Використовувати `Channel.UNLIMITED` без контролю memory growth.
- Очікувати broadcast-поведінку від `receiveAsFlow`.
- Передавати UI state через `Channel`, хоча краще `StateFlow`.

11. **Практичне правило**

Якщо ти будуєш data stream або reactive pipeline — використовуй `Flow`.
Якщо тобі потрібна низькорівнева черга/комунікація між корутинами — використовуй
`Channel`.

**Коротко:** `Channel` — це асинхронна черга для coroutine communication. `Flow` —
це декларативний stream API для значень у часі. У прикладному коді частіше
краще починати з `Flow`, а `Channel` залишати для спеціальних producer-consumer
сценаріїв.

</details>
<details>
<summary>64. Які основні оператори Flow (map, filter, combine, zip)?</summary>

#### Kotlin

Оператори `Flow` дозволяють будувати асинхронний pipeline: перетворювати
значення, фільтрувати їх, комбінувати кілька потоків, обробляти помилки і
контролювати context виконання. Найчастіше в реальному коді використовують
`map`, `filter`, `combine`, `zip`, `catch`, `onEach`, `flatMapLatest`,
`distinctUntilChanged`, `debounce`, `buffer`, `conflate`.

1. **map**

`map` перетворює кожне значення flow:

```kotlin
repository.observeUser()
    .map { user ->
        UserUiModel(
            name = user.name,
            avatarUrl = user.avatarUrl
        )
    }
    .collect { uiModel ->
        render(uiModel)
    }
```

Типовий use case — мапінг DTO/domain model у UI model.

```kotlin
fun observeState(): Flow<UiState> =
    repository.observeUser()
        .map { user -> UiState.Success(user) }
```

2. **filter**

`filter` пропускає тільки значення, які відповідають умові:

```kotlin
repository.observeUsers()
    .filter { users -> users.isNotEmpty() }
    .collect { users ->
        render(users)
    }
```

Або:

```kotlin
eventsFlow
    .filter { event -> event is UiEvent.Refresh }
    .collect {
        refresh()
    }
```

3. **onEach**

`onEach` використовується для side effects без зміни значення:

```kotlin
repository.observeUser()
    .onEach { user -> logger.log("User loaded: ${user.id}") }
    .collect { user -> render(user) }
```

Його часто використовують для logging, analytics або проміжного оновлення
state.

4. **catch**

`catch` обробляє exception з upstream:

```kotlin
repository.observeUser()
    .map { user -> UiState.Success(user) }
    .catch { throwable ->
        emit(UiState.Error(throwable.message))
    }
    .collect { state ->
        render(state)
    }
```

Важливо: `catch` ловить помилки вище себе в pipeline. Помилки всередині
`collect` він не перехоплює.

5. **combine**

`combine` обʼєднує останні значення з кількох flow. Коли будь-який із потоків
емітить нове значення, `combine` бере latest values з усіх потоків:

```kotlin
val state: Flow<UiState> =
    combine(
        userRepository.observeUser(),
        settingsRepository.observeSettings()
    ) { user, settings ->
        UiState.Content(
            user = user,
            theme = settings.theme
        )
    }
```

`combine` дуже часто використовується для побудови screen state з кількох
джерел.

6. **zip**

`zip` обʼєднує значення попарно:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2, 3)) { letter, number ->
        "$letter$number"
    }
    .collect { value ->
        println(value)
    }
```

Результат:

```text
A1
B2
C3
```

`zip` чекає відповідне значення з обох потоків. Він підходить, коли значення
мають логічно оброблятися парами.

7. **distinctUntilChanged**

`distinctUntilChanged` прибирає повторні однакові значення:

```kotlin
searchQueryFlow
    .distinctUntilChanged()
    .collect { query ->
        search(query)
    }
```

Це корисно для UI state і search input, щоб не запускати зайву роботу.

8. **debounce**

`debounce` чекає паузу перед emission:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        search(query)
    }
```

Типовий сценарій — пошук після того, як користувач перестав друкувати.

9. **flatMapLatest**

`flatMapLatest` перетворює кожне значення в новий flow і скасовує попередній,
якщо прийшло нове значення:

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest { query ->
        repository.search(query)
    }
    .collect { result ->
        render(result)
    }
```

Це сильний інструмент для search/autocomplete/filter сценаріїв.

10. **buffer і conflate**

`buffer` дозволяє producer-у і consumer-у працювати більш незалежно:

```kotlin
flow
    .buffer()
    .collect { value ->
        slowProcess(value)
    }
```

`conflate` пропускає проміжні значення, якщо collector не встигає:

```kotlin
flow
    .conflate()
    .collect { latest ->
        render(latest)
    }
```

`conflate` підходить для state-like потоків, де важливий останній стан, а не
кожне проміжне значення.

11. **flowOn**

`flowOn` змінює dispatcher для upstream:

```kotlin
repository.observeData()
    .map { data -> heavyMap(data) }
    .flowOn(Dispatchers.Default)
    .collect { result ->
        render(result)
    }
```

Усе, що вище `flowOn`, буде виконуватись у вказаному context. Collector може
залишатися на іншому dispatcher.

12. **Приклад реального pipeline**

```kotlin
val state: StateFlow<UiState> =
    searchQuery
        .debounce(300)
        .distinctUntilChanged()
        .flatMapLatest { query ->
            repository.search(query)
        }
        .map { results ->
            UiState.Content(results)
        }
        .catch { throwable ->
            emit(UiState.Error(throwable.message))
        }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

Це типовий ViewModel-підхід: input flow перетворюється в screen state.

13. **Практичне правило**

- `map` — перетворити значення.
- `filter` — пропустити тільки потрібне.
- `combine` — зібрати state з кількох потоків.
- `zip` — обʼєднати значення попарно.
- `catch` — обробити upstream-помилки.
- `debounce`/`distinctUntilChanged` — прибрати шум input-у.
- `flatMapLatest` — перемикатись на актуальний inner flow.
- `flowOn` — перенести upstream на потрібний dispatcher.

**Коротко:** оператори `Flow` дозволяють будувати читабельні reactive pipelines без
callback hell. Важливо не просто знати оператори, а розуміти їхню семантику:
що вони скасовують, що буферизують, де ловлять помилки і на якому dispatcher
виконуються.

</details>
<details>
<summary>65. У чому різниця між combine і zip?</summary>

#### Kotlin

`combine` і `zip` обʼєднують кілька `Flow`, але роблять це з різною семантикою.
`combine` реагує на кожне нове значення будь-якого потоку і бере останні
значення з інших потоків. `zip` обʼєднує значення попарно: перше з першим,
друге з другим, третє з третім.

1. **combine**

`combine` працює з latest values:

```kotlin
val screenState: Flow<UiState> =
    combine(
        userRepository.observeUser(),
        settingsRepository.observeSettings()
    ) { user, settings ->
        UiState.Content(
            user = user,
            theme = settings.theme
        )
    }
```

Коли `user` зміниться — `combine` візьме останні `settings` і створить новий
`UiState`. Коли `settings` зміняться — візьме останнього `user` і теж створить
новий `UiState`.

2. **zip**

`zip` працює попарно:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2, 3)) { letter, number ->
        "$letter$number"
    }
    .collect { value ->
        println(value)
    }
```

Результат:

```text
A1
B2
C3
```

`zip` чекає по одному значенню з кожного flow і обʼєднує саме пару.

3. **Поведіка при різній швидкості потоків**

Для `combine` швидший потік може породжувати багато emission-ів:

```kotlin
val names = flowOf("A", "B", "C")
val numbers = flowOf(1)

names.combine(numbers) { name, number ->
    "$name$number"
}
```

Після того як обидва flow дали хоча б одне значення, кожне нове значення з
будь-якого flow створює новий результат.

Для `zip` швидший потік чекатиме повільніший:

```kotlin
names.zip(numbers) { name, number ->
    "$name$number"
}
```

Тут результат буде тільки для доступних пар. Якщо `numbers` має одне значення,
буде одна пара.

4. **Коли combine підходить**

`combine` — типовий вибір для UI state і reactive state composition:

```kotlin
val state: Flow<SearchState> =
    combine(
        queryFlow,
        filtersFlow,
        sortOrderFlow
    ) { query, filters, sortOrder ->
        SearchState(
            query = query,
            filters = filters,
            sortOrder = sortOrder
        )
    }
```

Якщо змінився query, filters або sort order — екран має отримати новий state.
Тут потрібна саме latest-value семантика.

5. **Коли zip підходить**

`zip` підходить, коли значення логічно мають оброблятись парами:

```kotlin
questionsFlow
    .zip(answersFlow) { question, answer ->
        QuestionWithAnswer(question, answer)
    }
```

Або коли два потоки представляють синхронні послідовності, де перший елемент
одного потоку має відповідати першому елементу іншого.

У UI-state задачах `zip` потрібен значно рідше, ніж `combine`.

6. **Поведінка завершення**

`zip` завершується, коли один із потоків завершився і більше немає пар:

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1)) { letter, number ->
        "$letter$number"
    }
```

Результат буде тільки:

```text
A1
```

`combine` працює інакше: після того як кожен потік дав хоча б одне значення,
він може продовжувати емітити при оновленнях будь-якого потоку, доки це
дозволяє lifecycle upstream-ів.

7. **Типова помилка**

Погано використовувати `zip` для screen state:

```kotlin
val state = userFlow.zip(settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

Якщо `settingsFlow` емітить рідше, оновлення `userFlow` можуть чекати пару і не
потрапляти в UI тоді, коли очікується. Для screen state зазвичай потрібно:

```kotlin
val state = userFlow.combine(settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

8. **Ментальна модель**

`combine`:

```text
user changed       -> new state with latest settings
settings changed   -> new state with latest user
```

`zip`:

```text
first user  + first settings  -> first result
second user + second settings -> second result
```

9. **Практичне правило**

- Для UI state, filters, settings, form state, screen model — `combine`.
- Для попарної синхронізації двох послідовностей — `zip`.
- Якщо хочеш "оновлюй результат при зміні будь-чого" — `combine`.
- Якщо хочеш "обʼєднай елементи один до одного" — `zip`.

**Коротко:** `combine` працює з останніми значеннями потоків і реагує на кожне
оновлення будь-якого з них. `zip` обʼєднує значення попарно і підходить для
синхронних послідовностей, а не для більшості UI-state сценаріїв.

</details>
<details>
<summary>66. Для чого потрібні @JvmStatic, @JvmOverloads, @JvmField?</summary>

#### Kotlin

`@JvmStatic`, `@JvmOverloads` і `@JvmField` — це Kotlin-анотації для кращої
сумісності з Java. Вони не потрібні для звичайного Kotlin-коду, але важливі,
коли API має зручно викликатися з Java, Android framework, reflection-based
бібліотек або legacy Java-коду.

1. **Навіщо вони взагалі потрібні**

Kotlin компілюється в JVM bytecode, але не всі Kotlin-конструкції напряму
виглядають у Java так, як очікує Java-розробник.

Наприклад, `companion object` у Kotlin:

```kotlin
class UserFactory {
    companion object {
        fun create(): User = User()
    }
}
```

З Kotlin:

```kotlin
val user = UserFactory.create()
```

А з Java без `@JvmStatic`:

```java
User user = UserFactory.Companion.create();
```

Це не завжди бажаний Java API.

2. **@JvmStatic**

`@JvmStatic` генерує справжній static method або static property accessor для
Java.

```kotlin
class UserFactory {
    companion object {
        @JvmStatic
        fun create(): User = User()
    }
}
```

Тепер з Java:

```java
User user = UserFactory.create();
```

Без `@JvmStatic` Java викликала б метод через `Companion`.

3. **Де використовувати @JvmStatic**

Типові місця:

- `companion object`;
- `object`;
- API, який активно викликається з Java;
- Android framework callbacks або factory methods;
- public library API для Java-користувачів.

```kotlin
object Logger {
    @JvmStatic
    fun log(message: String) {
        println(message)
    }
}
```

З Java:

```java
Logger.log("Hello");
```

4. **@JvmOverloads**

Kotlin має default arguments:

```kotlin
class UserService(
    private val retryCount: Int = 3,
    private val timeoutMs: Long = 5_000
)
```

У Kotlin можна викликати:

```kotlin
UserService()
UserService(retryCount = 5)
UserService(timeoutMs = 10_000)
```

Java не підтримує Kotlin default arguments напряму. `@JvmOverloads` генерує
кілька overload-методів/конструкторів для Java:

```kotlin
class UserService @JvmOverloads constructor(
    private val retryCount: Int = 3,
    private val timeoutMs: Long = 5_000
)
```

З Java:

```java
new UserService();
new UserService(5);
new UserService(5, 10_000L);
```

5. **@JvmOverloads для функцій**

```kotlin
@JvmOverloads
fun showMessage(
    text: String,
    duration: Int = 3,
    important: Boolean = false
) {
    // ...
}
```

Для Java будуть згенеровані overload-и:

```java
showMessage("Hi");
showMessage("Hi", 5);
showMessage("Hi", 5, true);
```

Важливо: overload-и генеруються з кінця параметрів. Тому default parameters
краще ставити після обовʼязкових.

6. **Де використовувати @JvmOverloads**

Добрі сценарії:

- custom Android Views;
- public API, який викликається з Java;
- constructors для Java-friendly використання;
- бібліотеки зі змішаним Kotlin/Java кодом.

Приклад для Android custom view:

```kotlin
class AvatarView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr)
```

Це робить constructor зручним для Android framework і Java interop.

7. **@JvmField**

За замовчуванням Kotlin property компілюється в private field + getter/setter:

```kotlin
class Config {
    val apiUrl: String = "https://example.com"
}
```

З Java:

```java
config.getApiUrl();
```

`@JvmField` відкриває field напряму без getter/setter:

```kotlin
class Config {
    @JvmField
    val apiUrl: String = "https://example.com"
}
```

З Java:

```java
config.apiUrl;
```

8. **@JvmField у companion object**

Без `@JvmField`:

```kotlin
class Constants {
    companion object {
        val DEFAULT_TIMEOUT = 5_000
    }
}
```

Java:

```java
Constants.Companion.getDEFAULT_TIMEOUT();
```

З `@JvmField`:

```kotlin
class Constants {
    companion object {
        @JvmField
        val DEFAULT_TIMEOUT = 5_000
    }
}
```

Java:

```java
Constants.DEFAULT_TIMEOUT;
```

Для compile-time constants краще використовувати `const val`, якщо тип
підходить:

```kotlin
const val API_VERSION = "v1"
```

9. **Коли не треба використовувати ці анотації**

Якщо код використовується тільки з Kotlin, ці анотації часто зайві:

```kotlin
class UserMapper {
    companion object {
        fun map(dto: UserDto): User = User(dto.name)
    }
}
```

У Kotlin і так буде нормальний виклик:

```kotlin
UserMapper.map(dto)
```

Не треба додавати `@JvmStatic` просто "про всяк випадок". Це розширює bytecode
API і може створювати зайві overload-и/entry points.

10. **Практичне правило**

- `@JvmStatic` — коли Java має викликати method/property як static.
- `@JvmOverloads` — коли Java має бачити overload-и замість Kotlin default args.
- `@JvmField` — коли Java має доступатися до field напряму без getter/setter.
- Для чистого Kotlin API ці анотації зазвичай не потрібні.

**Коротко:** ці анотації потрібні не для Kotlin-логіки, а для форми JVM API. Вони
допомагають зробити Kotlin-код природним для Java та Android framework, але їх
варто застосовувати тільки там, де реально є Java interop-потреба.

</details>
<details>
<summary>67. Що таке делегування (delegation)?</summary>

#### Kotlin

Делегування — це підхід, коли обʼєкт передає частину своєї поведінки іншому
обʼєкту. У Kotlin делегування підтримується на рівні мови: є делегування
інтерфейсів через `by` і делеговані властивості (`lazy`, `observable`, custom
delegates).

1. **Ідея делегування**

Замість наслідування можна винести поведінку в окремий обʼєкт і делегувати йому
виклики.

```kotlin
interface Logger {
    fun log(message: String)
}

class ConsoleLogger : Logger {
    override fun log(message: String) {
        println(message)
    }
}
```

Клас може не реалізовувати поведінку сам, а передати її delegate-обʼєкту.

2. **Делегування інтерфейсу через by**

```kotlin
class UserService(
    private val logger: Logger
) : Logger by logger {

    fun createUser(name: String) {
        log("Creating user: $name")
    }
}
```

`UserService` формально реалізує `Logger`, але методи `Logger` делегуються
обʼєкту `logger`.

Це означає, що компілятор згенерує forwarding-код:

```kotlin
override fun log(message: String) {
    logger.log(message)
}
```

Але писати його вручну не потрібно.

3. **Навіщо це краще за inheritance**

Делегування часто краще за наслідування, бо воно базується на composition:

```kotlin
class FileRepository(
    private val cache: Cache,
    private val logger: Logger
)
```

Обʼєкт отримує поведінку через залежності, а не через жорстку ієрархію класів.

Переваги:

- менше coupling;
- легше тестувати;
- легше замінювати реалізації;
- немає проблем fragile base class;
- поведінку можна комбінувати.

4. **Перевизначення делегованого методу**

Можна делегувати інтерфейс, але окремі методи перевизначити:

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {

    override fun log(message: String) {
        delegate.log("[App] $message")
    }
}
```

Тут усі методи `Logger` делегуються, але `log` має custom behavior.

5. **Делеговані властивості**

Kotlin також підтримує property delegation:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

`lazy` ініціалізує значення тільки при першому доступі:

```kotlin
println(config) // тут loadConfig() виконається вперше
```

Це корисно для дорогих або відкладених обчислень.

6. **observable**

```kotlin
var username: String by Delegates.observable("") { property, oldValue, newValue ->
    println("${property.name}: $oldValue -> $newValue")
}
```

`observable` дозволяє реагувати на зміну property.

Є також `vetoable`, який може відхилити нове значення:

```kotlin
var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

7. **Custom property delegate**

Делегат для property має реалізувати `getValue`, а для `var` ще й `setValue`:

```kotlin
class TrimmedString {
    private var value: String = ""

    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return value
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, newValue: String) {
        value = newValue.trim()
    }
}

var name: String by TrimmedString()
```

Тепер:

```kotlin
name = "  Kotlin  "
println(name) // Kotlin
```

8. **by lazy у production-коді**

`lazy` має різні режими thread-safety:

```kotlin
val value by lazy(LazyThreadSafetyMode.SYNCHRONIZED) {
    createValue()
}
```

Режими:

- `SYNCHRONIZED` — thread-safe за замовчуванням;
- `PUBLICATION` — кілька потоків можуть ініціалізувати, але збережеться одне
  значення;
- `NONE` — без thread-safety, швидше, але тільки якщо доступ з одного thread.

Для Android UI-коду часто достатньо `NONE`, якщо property використовується лише
на main thread. Але це треба робити свідомо.

9. **Делегування в Android**

Типові приклади:

```kotlin
private val viewModel: UserViewModel by viewModels()
```

або:

```kotlin
private val binding by lazy {
    ActivityMainBinding.inflate(layoutInflater)
}
```

`by viewModels()` — це delegate, який керує отриманням `ViewModel` з правильним
lifecycle owner-ом.

10. **Типові помилки**

- Використовувати inheritance там, де достатньо delegation.
- Ховати складну бізнес-логіку в property delegate без потреби.
- Використовувати `lazy` для обʼєктів, які треба явно очищати.
- Забувати про thread-safety режим `lazy`.
- Делегувати занадто багато інтерфейсів одному класу і робити його нечитабельним.

11. **Практичне правило**

Делегування використовують, коли обʼєкт має використати поведінку іншого
обʼєкта без наслідування. Для поведінки — interface delegation через `by`. Для
властивостей — property delegates типу `lazy`, `observable` або custom delegate.

**Коротко:** delegation у Kotlin — це language-level підтримка composition. Вона
дозволяє повторно використовувати поведінку без жорсткого inheritance і робить
код гнучкішим, тестованішим і простішим для заміни реалізацій.

</details>
<details>
<summary>68. Які фреймворки для тестування доступні в Kotlin?</summary>

#### Kotlin

У Kotlin доступні різні інструменти для unit, integration, UI, coroutine, Flow і Android-specific тестування. Вибір залежить від рівня тесту і того, що саме треба перевірити.

1. **JUnit**

`JUnit` — базовий framework для unit tests:

```kotlin
class CalculatorTest {
    @Test
    fun `adds numbers`() {
        assertEquals(4, Calculator().sum(2, 2))
    }
}
```

JUnit 4 досі часто використовується в Android. JUnit 5 популярний у JVM/Kotlin backend, але в Android вимагає додаткового setup.

2. **Kotlin test**

`kotlin.test` — multiplatform-friendly assertions API:

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

Корисний для Kotlin Multiplatform.

3. **MockK**

`MockK` — популярний mocking framework для Kotlin:

```kotlin
val repository = mockk<UserRepository>()
coEvery { repository.getUser() } returns User("1")
```

Добре підтримує coroutines, final classes, extension functions.

4. **Mockito**

`Mockito` — класичний Java mocking framework:

```kotlin
val repository = mock(UserRepository::class.java)
```

Може використовуватись у Kotlin, але MockK часто зручніший саме для Kotlin-особливостей.

5. **Kotest**

`Kotest` дає DSL-стилі тестів і багато matchers:

```kotlin
class UserSpec : StringSpec({
    "user has name" {
        User("Alex").name shouldBe "Alex"
    }
})
```

Частіше використовується у JVM/Kotlin backend або multiplatform, в Android — залежить від setup команди.

6. **Turbine**

`Turbine` — бібліотека для тестування `Flow`:

```kotlin
flow.test {
    assertEquals(Loading, awaitItem())
    assertEquals(Success, awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Дуже корисна для ViewModel state/effects.

7. **kotlinx-coroutines-test**

Для coroutine tests:

```kotlin
@Test
fun `loads data`() = runTest {
    viewModel.load()
    advanceUntilIdle()
    assertEquals(expected, viewModel.state.value)
}
```

Дає `runTest`, test dispatchers і virtual time.

8. **AndroidX Test**

Для Android instrumentation tests:

- `androidx.test.core`;
- `androidx.test.ext:junit`;
- `androidx.test.espresso`;
- `androidx.test.runner`;
- `androidx.test.rules`.

Використовується, коли потрібен Android framework, device або emulator.

9. **Espresso**

`Espresso` — UI testing framework для View System:

```kotlin
onView(withId(R.id.loginButton)).perform(click())
onView(withText("Success")).check(matches(isDisplayed()))
```

Підходить для XML/View-based UI.

10. **Compose UI Test**

Для Jetpack Compose:

```kotlin
composeTestRule
    .onNodeWithText("Login")
    .performClick()
```

Compose має власний testing API через semantics tree.

11. **Robolectric**

`Robolectric` дозволяє запускати частину Android tests на JVM без emulator. Корисний для швидких tests, але не завжди точно повторює поведінку реального device.

12. **Практичне правило**

- Pure Kotlin logic — JUnit/kotlin.test.
- Kotlin mocks — MockK.
- Coroutines — kotlinx-coroutines-test.
- Flow — Turbine.
- ViewModel — unit tests + fake dependencies.
- View UI — Espresso.
- Compose UI — Compose UI Test.
- Android framework без emulator — Robolectric.
- Реальна інтеграція з device — instrumentation tests.

**Коротко:** для Kotlin тестування найчастіше використовують JUnit/kotlin.test, MockK/Mockito, kotlinx-coroutines-test, Turbine, Kotest, AndroidX Test, Espresso, Compose UI Test і Robolectric. Вибір залежить від рівня: unit, coroutine/Flow, UI, Android framework або integration tests.

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

Корутини та `Flow` тестують через `kotlinx-coroutines-test`. Головна ідея — не чекати реальний час, а керувати virtual time, test dispatcher і coroutine scheduler.

1. **Базові залежності**

Зазвичай потрібні:

```kotlin
testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test:<version>")
testImplementation("app.cash.turbine:turbine:<version>")
```

`kotlinx-coroutines-test` дає `runTest`, `TestDispatcher`, virtual time. Turbine зручний для перевірки Flow emissions.

2. **runTest**

```kotlin
@Test
fun `loads user`() = runTest {
    val result = repository.loadUser()
    assertEquals(expectedUser, result)
}
```

`runTest` запускає coroutine-тест у контрольованому середовищі і автоматично чекає child coroutines.

3. **Test dispatcher**

Якщо код використовує dispatchers, їх краще інжектити:

```kotlin
class UserRepository(
    private val dispatcher: CoroutineDispatcher,
    private val api: UserApi
) {
    suspend fun loadUser(): User = withContext(dispatcher) {
        api.getUser()
    }
}
```

У тесті передаємо test dispatcher:

```kotlin
@Test
fun `loads user`() = runTest {
    val repository = UserRepository(StandardTestDispatcher(testScheduler), fakeApi)
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

4. **Main dispatcher rule**

Для ViewModel часто треба підмінити `Dispatchers.Main`:

```kotlin
class MainDispatcherRule(
    val dispatcher: TestDispatcher = StandardTestDispatcher()
) : TestWatcher() {
    override fun starting(description: Description) {
        Dispatchers.setMain(dispatcher)
    }

    override fun finished(description: Description) {
        Dispatchers.resetMain()
    }
}
```

```kotlin
@get:Rule
val mainDispatcherRule = MainDispatcherRule()
```

5. **advanceUntilIdle**

```kotlin
@Test
fun `updates state`() = runTest {
    viewModel.load()

    advanceUntilIdle()

    assertEquals(ProfileState.Success(user), viewModel.state.value)
}
```

`advanceUntilIdle()` виконує всі заплановані coroutine tasks у test scheduler.

6. **Тестування delay**

```kotlin
@Test
fun `emits after delay`() = runTest {
    var called = false

    launch {
        delay(1_000)
        called = true
    }

    advanceTimeBy(1_000)
    assertTrue(called)
}
```

Тест не чекає реальну секунду: virtual time перемотується миттєво.

7. **Тестування Flow через Turbine**

```kotlin
@Test
fun `emits states`() = runTest {
    viewModel.state.test {
        assertEquals(ProfileState.Loading, awaitItem())
        assertEquals(ProfileState.Success(user), awaitItem())
        cancelAndIgnoreRemainingEvents()
    }
}
```

Turbine дозволяє читати emissions послідовно і перевіряти, що саме прийшло.

8. **StateFlow**

Для `StateFlow` часто достатньо перевірити `value` після виконання:

```kotlin
viewModel.load()
advanceUntilIdle()
assertEquals(expectedState, viewModel.state.value)
```

Якщо важлива послідовність loading → success/error, краще Turbine.

9. **SharedFlow / effects**

One-shot effects тестують як Flow:

```kotlin
@Test
fun `emits navigation effect`() = runTest {
    viewModel.effects.test {
        viewModel.onBackClick()
        assertEquals(Effect.NavigateBack, awaitItem())
    }
}
```

10. **Типові помилки**

- використовувати `Thread.sleep()` у тестах;
- хардкодити `Dispatchers.IO`/`Main` без можливості підміни;
- не викликати `advanceUntilIdle()`;
- тестувати Flow без timeout/cancel;
- змішувати real dispatchers і test dispatchers;
- не скасовувати нескінченні Flow collection.

11. **Практичне правило**

- Coroutine tests — через `runTest`.
- Dispatchers — інжектити.
- Main — підміняти rule-ом.
- Delay — перевіряти через virtual time.
- Flow emissions — через Turbine.
- StateFlow final state — через `.value`, якщо порядок emissions не важливий.

**Коротко:** корутини тестують через `runTest`, `TestDispatcher`, virtual time і `advanceUntilIdle()`. `Flow` зручно тестувати через Turbine, `StateFlow` — через `.value` або emissions, а dispatchers треба інжектити, щоб тести були швидкими, стабільними й не залежали від real threads.

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

Null safety у Kotlin — це система типів, яка змушує явно розділяти nullable і
non-null значення. Але сама наявність `String?` не гарантує якісний код.
Потрібно правильно моделювати домен, мінімізувати nullable state, не зловживати
`!!` і чітко визначати, де `null` є валідним бізнес-значенням, а де це помилка
даних або API.

1. **Використовувати non-null за замовчуванням**

Якщо значення обовʼязкове — тип має бути non-null:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Не треба робити все nullable "про всяк випадок":

```kotlin
data class User(
    val id: String?,
    val name: String?
)
```

Такий код переносить проблему на всіх callers: кожен тепер має перевіряти
`null`, навіть якщо за бізнес-правилами `id` і `name` завжди потрібні.

2. **Nullable має означати реальну відсутність значення**

Добрий приклад:

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

`middleName` справді може бути відсутнім, тому `String?` тут коректний.

Поганий приклад:

```kotlin
data class Order(
    val totalPrice: Money?
)
```

Якщо order не може існувати без ціни, nullable тип приховує проблему. Краще
зробити поле non-null і валідувати дані на boundary.

3. **Не зловживати `!!`**

`!!` перетворює nullable тип у non-null або кидає `NullPointerException`:

```kotlin
val name = user.name!!
```

Це має бути рідкісний інструмент, а не нормальна практика. Якщо доводиться
часто писати `!!`, значить модель типів або lifecycle організовані погано.

Краще:

```kotlin
val name = user.name ?: return
```

або:

```kotlin
val name = requireNotNull(user.name) {
    "User name is required"
}
```

`requireNotNull` краще, коли null — це порушення контракту і треба впасти з
зрозумілим повідомленням.

4. **Safe call і Elvis operator**

Safe call:

```kotlin
val city = user.address?.city
```

Elvis:

```kotlin
val city = user.address?.city ?: "Unknown"
```

Але default value треба використовувати тільки якщо він бізнесово коректний.
Не треба маскувати проблеми:

```kotlin
val price = order.price ?: Money.ZERO
```

Якщо відсутність ціни означає corrupted data, краще явно обробити error.

5. **Early return для nullable**

Часто найчитабельніший підхід — early return:

```kotlin
fun render(user: User?) {
    user ?: return

    nameView.text = user.name
}
```

Або:

```kotlin
fun submit(email: String?) {
    val normalizedEmail = email?.trim()?.takeIf { it.isNotEmpty() } ?: return

    authService.submit(normalizedEmail)
}
```

Це прибирає вкладені `if` і робить happy path чистішим.

6. **let для локальної роботи з nullable**

```kotlin
user.avatarUrl?.let { url ->
    imageLoader.load(url)
}
```

`let` доречний для короткої локальної операції. Але якщо всередині багато
логіки, краще зробити explicit variable або early return, щоб код не перетворився
на вкладений callback-style.

7. **Null Object або sealed result замість null**

Іноді `null` занадто бідний для доменної моделі. Наприклад:

```kotlin
fun findUser(id: String): User?
```

Це нормально, якщо відповідь тільки "є/немає". Але якщо причин більше:

```kotlin
sealed interface FindUserResult {
    data class Found(val user: User) : FindUserResult
    data object NotFound : FindUserResult
    data object NoPermission : FindUserResult
}
```

Тоді caller не втрачає контекст.

8. **Boundary validation**

Дані з network/database/platform API можуть бути nullable або некоректними. Їх
краще валідувати на межі:

```kotlin
data class UserDto(
    val id: String?,
    val name: String?
)

fun UserDto.toDomain(): User {
    return User(
        id = requireNotNull(id) { "User id is required" },
        name = requireNotNull(name) { "User name is required" }
    )
}
```

Так domain model залишається clean і non-null.

9. **Platform types з Java**

Java interop створює platform types:

```kotlin
val name = javaUser.getName()
```

Kotlin може не знати, чи `name` nullable. Такі значення треба обробляти
обережно:

```kotlin
val name = javaUser.getName() ?: "Unknown"
```

Або одразу нормалізувати на boundary:

```kotlin
fun JavaUser.toDomain(): User =
    User(
        id = requireNotNull(id),
        name = name.orEmpty()
    )
```

Якщо контролюєш Java API, варто додавати nullability annotations.

10. **lateinit використовувати обережно**

`lateinit` дозволяє non-null property без initial value:

```kotlin
private lateinit var repository: UserRepository
```

Але якщо звернутися до неініціалізованої property, буде
`UninitializedPropertyAccessException`.

У production-коді краще constructor injection:

```kotlin
class UserService(
    private val repository: UserRepository
)
```

`lateinit` частіше доречний у тестах або framework-managed lifecycle, але не як
заміна нормальної ініціалізації.

11. **Nullable collections**

Є різниця:

```kotlin
List<User>?
List<User?>
List<User?>?
```

- `List<User>?` — список може бути null.
- `List<User?>` — список є, але елементи можуть бути null.
- `List<User?>?` — nullable і список, і елементи.

Треба вибирати найточніший тип. Часто краще повертати empty list замість null:

```kotlin
fun getUsers(): List<User> = users ?: emptyList()
```

Але тільки якщо empty list і відсутність даних справді означають одне й те саме.

12. **Практичне правило**

- Non-null за замовчуванням.
- Nullable тільки якщо відсутність значення є валідною.
- Не використовувати `!!` як звичку.
- Валідувати nullable DTO/API values на boundary.
- Для складних станів використовувати sealed types, а не `null`.
- `lateinit` — обережно і переважно не для бізнес-логіки.
- Empty value не має маскувати corrupted data.

**Коротко:** null safety у Kotlin працює найкраще, коли типи чесно описують домен.
Nullable має бути явним бізнес-сигналом, а не способом відкласти валідацію або
сховати поганий API-контракт.

</details>
<details>
<summary>75. Що таке SupervisorScope і чим він відрізняється?</summary>

#### Kotlin

`supervisorScope` — це coroutine scope, у якому помилка однієї дочірньої
корутини не скасовує автоматично інші дочірні корутини. Він потрібен для
сценаріїв, де кілька задач виконуються паралельно, але failure однієї задачі не
має ламати всі інші.

1. **Звичайний coroutineScope**

У `coroutineScope` child failure скасовує весь scope:

```kotlin
suspend fun loadScreen() = coroutineScope {
    launch {
        loadProfile()
    }

    launch {
        loadRecommendations()
    }
}
```

Якщо `loadProfile()` впаде з exception, `loadRecommendations()` буде скасована.
Це правильна поведінка, коли всі child-задачі є частинами однієї неподільної
операції.

2. **supervisorScope**

У `supervisorScope` failure однієї child-корутини не скасовує siblings:

```kotlin
suspend fun loadWidgets() = supervisorScope {
    launch {
        loadWeatherWidget()
    }

    launch {
        loadNewsWidget()
    }
}
```

Якщо weather widget впаде, news widget може продовжити виконання. Це корисно,
коли блоки незалежні.

3. **Важливий нюанс: exception треба обробити**

`supervisorScope` не означає "exceptions ігноруються". Якщо child-корутина
падає, exception усе одно треба обробити або залогувати:

```kotlin
supervisorScope {
    launch {
        try {
            loadWeatherWidget()
        } catch (e: Throwable) {
            logger.error(e)
            weatherState.value = WidgetState.Error
        }
    }

    launch {
        try {
            loadNewsWidget()
        } catch (e: Throwable) {
            logger.error(e)
            newsState.value = WidgetState.Error
        }
    }
}
```

Інакше можна отримати uncaught exception у child coroutine.

4. **coroutineScope vs supervisorScope**

`coroutineScope`:

```text
child failed -> parent cancelled -> siblings cancelled
```

`supervisorScope`:

```text
child failed -> siblings continue
```

Але якщо сам `supervisorScope` буде скасований ззовні, усі children теж будуть
скасовані. Supervisor не скасовує propagation зверху вниз.

5. **SupervisorJob**

`SupervisorJob` — це job із supervisor semantics:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)
```

Він часто використовується для довгоживучих scope-ів, де незалежні child tasks
не мають валити одна одну.

Приклад:

```kotlin
class SyncManager {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    fun startUserSync() {
        scope.launch {
            syncUsers()
        }
    }

    fun startOrdersSync() {
        scope.launch {
            syncOrders()
        }
    }

    fun stop() {
        scope.cancel()
    }
}
```

Якщо `syncUsers()` впаде, `syncOrders()` не буде автоматично скасований.

6. **supervisorScope vs SupervisorJob**

`supervisorScope` — локальний scope всередині suspend-функції:

```kotlin
suspend fun load() = supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

`SupervisorJob` — job, який зазвичай використовують для створення custom
`CoroutineScope`:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

Ментально:

- `supervisorScope` — тимчасовий блок;
- `SupervisorJob` — властивість scope-а.

7. **async у supervisorScope**

З `async` важливо не забути `await()`:

```kotlin
supervisorScope {
    val profile = async { loadProfile() }
    val news = async { loadNews() }

    val profileResult = runCatching { profile.await() }
    val newsResult = runCatching { news.await() }
}
```

Exception з `async` буде кинутий при `await()`. Supervisor semantics допомагає
не скасувати siblings, але результат кожної задачі все одно треба обробити.

8. **Коли supervisorScope доречний**

Добрі сценарії:

- незалежні widgets на одному екрані;
- паралельне завантаження optional sections;
- background tasks, які не мають валити одна одну;
- best-effort операції;
- independent sync jobs;
- partial success UI.

Приклад:

```kotlin
suspend fun loadHomeScreen(): HomeState = supervisorScope {
    val profile = async { runCatching { loadProfile() } }
    val banners = async { runCatching { loadBanners() } }
    val recommendations = async { runCatching { loadRecommendations() } }

    HomeState(
        profile = profile.await().getOrNull(),
        banners = banners.await().getOrDefault(emptyList()),
        recommendations = recommendations.await().getOrDefault(emptyList())
    )
}
```

Тут екран може показати частину даних, навіть якщо один блок не завантажився.

9. **Коли supervisorScope не підходить**

Якщо задачі залежні одна від одної, supervisor може приховати неправильну
модель:

```kotlin
supervisorScope {
    launch { authenticate() }
    launch { loadPrivateData() }
}
```

Якщо `authenticate()` впав, `loadPrivateData()` не має продовжуватись. Тут
краще звичайний `coroutineScope` або послідовна логіка.

10. **Практичне правило**

- Якщо failure однієї child-задачі має скасувати всю операцію — `coroutineScope`.
- Якщо child-задачі незалежні й допускається partial success — `supervisorScope`.
- Для довгоживучого custom scope — `SupervisorJob`.
- Exceptions у supervised children усе одно треба обробляти.
- Скасування parent scope все одно скасовує всіх children.

**Коротко:** `supervisorScope` змінює propagation помилок між siblings: одна
дочірня корутина може впасти, не скасовуючи інші. Це інструмент для незалежних
паралельних задач, а не спосіб ігнорувати помилки.

</details>
<details>
<summary>76. Що таке backpressure і як його обробляти у Flow?</summary>

#### Kotlin

Backpressure — це ситуація, коли producer генерує значення швидше, ніж consumer
встигає їх обробляти. У `Flow` це особливо важливо для потоків подій, UI state,
сенсорних даних, progress updates, search input або будь-якого stream-а, де
emissions можуть приходити швидко.

1. **Базова проблема**

Приклад:

```kotlin
fastFlow()
    .collect { value ->
        slowProcess(value)
    }
```

Якщо `fastFlow()` емітить значення швидко, а `slowProcess()` працює довго, треба
вирішити, що робити:

- чекати consumer;
- буферизувати значення;
- пропускати старі значення;
- обробляти тільки останнє;
- скасовувати попередню обробку.

Немає одного правильного рішення — залежить від бізнес-семантики.

2. **Flow за замовчуванням sequential**

Звичайний `Flow` працює послідовно:

```kotlin
flow {
    emit(1)
    emit(2)
    emit(3)
}.collect { value ->
    delay(1_000)
    println(value)
}
```

Producer не буде просто безконтрольно емітити значення вперед. `emit` suspend-иться,
поки downstream не буде готовий приймати наступне значення.

Це вже базова форма backpressure handling.

3. **buffer**

`buffer()` дозволяє producer і consumer працювати більш незалежно:

```kotlin
flow {
    repeat(100) {
        emit(it)
    }
}
    .buffer()
    .collect { value ->
        slowProcess(value)
    }
```

Тепер producer може емітити значення в buffer, поки consumer обробляє попередні.

Це корисно, коли:

- producer і consumer можуть працювати паралельно;
- кожне значення важливе;
- треба зменшити idle time;
- memory growth контрольований.

Але buffer не вирішує проблему нескінченно швидкого producer-а. Він лише дає
обмежений запас.

4. **conflate**

`conflate()` пропускає проміжні значення, якщо consumer не встигає:

```kotlin
progressFlow
    .conflate()
    .collect { progress ->
        renderProgress(progress)
    }
```

Якщо progress змінюється:

```text
1, 2, 3, 4, 5
```

а UI встигає намалювати тільки `1` і `5`, це може бути нормально. Для progress
або state-like потоків часто важливий останній стан, а не кожне проміжне
значення.

5. **collectLatest**

`collectLatest` скасовує обробку попереднього значення, якщо приходить нове:

```kotlin
queryFlow
    .debounce(300)
    .collectLatest { query ->
        val result = repository.search(query)
        render(result)
    }
```

Це підходить для:

- search;
- autocomplete;
- preview;
- rendering останнього state;
- задач, де старий результат після нового input уже неактуальний.

Якщо кожне значення важливе, `collectLatest` не підходить.

6. **debounce**

`debounce` зменшує кількість emissions, чекаючи паузу:

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        search(query)
    }
```

Це класичний варіант для text input. Ми не запускаємо пошук на кожен символ,
якщо користувач ще друкує.

7. **sample**

`sample` бере останнє значення з певним інтервалом:

```kotlin
sensorFlow
    .sample(1_000)
    .collect { value ->
        render(value)
    }
```

Це корисно, коли дані приходять дуже часто, але UI або downstream має
оновлюватись не частіше певного rate.

8. **distinctUntilChanged**

Якщо багато однакових значень:

```kotlin
stateFlow
    .distinctUntilChanged()
    .collect { state ->
        render(state)
    }
```

Це прибирає зайву роботу, коли state фактично не змінився.

Для `StateFlow` базова distinct-поведінка вже є: він не емітить нове значення,
якщо воно `equals` старому. Але в pipeline з мапінгом `distinctUntilChanged`
часто все одно корисний.

9. **buffer capacity і overflow strategy**

Можна явно вказати capacity і strategy:

```kotlin
flow
    .buffer(
        capacity = 64,
        onBufferOverflow = BufferOverflow.DROP_OLDEST
    )
    .collect { value ->
        process(value)
    }
```

Стратегії:

- `SUSPEND` — producer чекає, коли buffer звільниться;
- `DROP_OLDEST` — викидати старі значення;
- `DROP_LATEST` — викидати нові значення.

Вибір залежить від семантики. Для UI state часто можна drop old. Для payments
або audit events — не можна.

10. **Коли не можна втрачати значення**

Не можна використовувати `conflate`, `collectLatest`, `DROP_OLDEST` або
`DROP_LATEST`, якщо кожне значення критичне:

- платежі;
- повідомлення;
- audit logs;
- задачі черги;
- зміни, які треба зберегти;
- commands із side effects.

Для таких сценаріїв потрібні queue, durable storage, retry, transactional
processing або explicit backpressure через suspend.

11. **callbackFlow і trySend**

У `callbackFlow` backpressure треба продумувати окремо:

```kotlin
fun observeEvents(): Flow<Event> = callbackFlow {
    val listener = object : Listener {
        override fun onEvent(event: Event) {
            trySend(event)
        }
    }

    source.addListener(listener)

    awaitClose {
        source.removeListener(listener)
    }
}
```

`trySend` може неуспішно відправити значення, якщо channel закритий або buffer
переповнений. Для критичних events треба явно вирішити, що робити з failure.

12. **Практичне правило**

- Якщо кожне значення важливе — не drop-ати, використовувати backpressure через
  suspension або queue.
- Якщо важливий тільки останній state — `conflate` або `collectLatest`.
- Якщо input шумний — `debounce`, `sample`, `distinctUntilChanged`.
- Якщо producer і consumer можна розділити — `buffer`.
- Якщо значення критичні — не використовувати dropping strategies без гарантій.

**Коротко:** backpressure у `Flow` — це контроль різниці швидкості між producer і
consumer. Kotlin дає інструменти `buffer`, `conflate`, `collectLatest`,
`debounce`, `sample` і overflow strategies, але правильний вибір залежить від
того, чи можна втрачати проміжні значення.

</details>
<details>
<summary>77. Як використовувати Java Streams у Kotlin?</summary>

#### Kotlin

Java Streams можна використовувати в Kotlin, бо Kotlin на JVM має повну
сумісність із Java API. Але в більшості Kotlin-коду замість Java Streams
частіше використовують Kotlin collections, `Sequence` або `Flow`, бо вони краще
інтегруються з мовою, null safety і coroutine-моделлю.

1. **Java Stream напряму**

Якщо є Java collection, можна викликати `stream()`:

```kotlin
val names = users.stream()
    .filter { user -> user.isActive }
    .map { user -> user.name }
    .toList()
```

Це звичайний Java Stream API, тільки з Kotlin lambda syntax.

2. **Kotlin collection operators як основний вибір**

У Kotlin той самий код зазвичай пишуть так:

```kotlin
val names = users
    .filter { user -> user.isActive }
    .map { user -> user.name }
```

Це читабельніше для Kotlin-команди і не потребує переходу в Java Stream API.

Для більшості звичайних collection transformations Kotlin operators — кращий
default.

3. **Java Stream vs Kotlin Sequence**

Kotlin collection operators eager: кожен оператор може створювати проміжну
колекцію.

```kotlin
val result = users
    .map { it.toUiModel() }
    .filter { it.isVisible }
```

Для великих collections або довгих pipelines можна використати `Sequence`:

```kotlin
val result = users
    .asSequence()
    .map { it.toUiModel() }
    .filter { it.isVisible }
    .toList()
```

`Sequence` lazy, як Java Stream, але має Kotlin API.

4. **Коли Java Streams доречні**

Java Streams має сенс використовувати, якщо:

- працюєш із Java API, який уже повертає `Stream<T>`;
- треба interop із Java library;
- команда підтримує спільний Java/Kotlin codebase;
- використовується Java-specific collector;
- потрібно не переписувати існуючий Java Stream pipeline.

Приклад із Java API:

```kotlin
fun readLines(path: Path): List<String> {
    return Files.lines(path).use { stream ->
        stream
            .filter { line -> line.isNotBlank() }
            .toList()
    }
}
```

Тут важливо закрити stream через `use`, бо `Files.lines()` тримає ресурс.

5. **Resource management**

Деякі Java Streams повʼязані з ресурсами:

```kotlin
Files.lines(path)
```

Такий stream треба закривати:

```kotlin
Files.lines(path).use { lines ->
    return lines
        .filter { it.isNotBlank() }
        .count()
}
```

Не всі streams потребують `close`, але якщо stream походить з I/O API, треба
перевірити документацію і використовувати `use`.

6. **Collectors у Kotlin**

Java:

```java
users.stream()
    .collect(Collectors.groupingBy(User::getRole));
```

Kotlin collection API:

```kotlin
val usersByRole = users.groupBy { it.role }
```

У Kotlin часто є коротший і природніший аналог:

- `groupBy`;
- `associateBy`;
- `associateWith`;
- `partition`;
- `sumOf`;
- `joinToString`;
- `fold`;
- `reduce`.

7. **Parallel streams**

Java має `parallelStream()`:

```kotlin
val result = users.parallelStream()
    .map { expensiveMap(it) }
    .toList()
```

У Kotlin/Android це треба використовувати дуже обережно. `parallelStream()`
використовує common ForkJoinPool і може створити проблеми:

- неконтрольована конкуренція;
- складне тестування;
- конфлікти з coroutine dispatchers;
- непередбачувана продуктивність на Android;
- проблеми з blocking operations.

У Kotlin частіше краще явно керувати concurrency через coroutines:

```kotlin
suspend fun mapUsers(users: List<User>): List<UserUiModel> = coroutineScope {
    users.map { user ->
        async(Dispatchers.Default) {
            expensiveMap(user)
        }
    }.awaitAll()
}
```

Але і це треба робити тільки для справді CPU-heavy задач.

8. **Null safety**

Java Stream API не знає Kotlin nullability так добре, як Kotlin collections.
Якщо Java stream може містити null:

```kotlin
val names = javaUsers.stream()
    .map { it.name }
    .toList()
```

Треба бути уважним до platform types. У Kotlin collection API nullable часто
виглядає явніше:

```kotlin
val names = javaUsers
    .mapNotNull { it.name }
```

9. **Stream одноразовий**

Java Stream не можна використати двічі:

```kotlin
val stream = users.stream()

stream.count()
stream.toList() // помилка: stream already operated upon or closed
```

Kotlin collection можна обробляти багато разів:

```kotlin
users.count()
users.map { it.name }
```

`Sequence` теж lazy, але повторне використання залежить від джерела. Якщо
sequence побудована з one-shot джерела, треба бути обережним.

10. **Java Stream vs Flow**

Java Stream — синхронний pipeline для in-memory або blocking джерел.

`Flow` — асинхронний stream із coroutine cancellation:

```kotlin
fun observeUsers(): Flow<List<User>>
```

Не треба використовувати Java Stream для asynchronous data streams у Kotlin.
Для цього є `Flow`.

11. **Практичне правило**

- Для звичайних Kotlin collections — використовуй Kotlin operators.
- Для lazy collection pipeline — `Sequence`.
- Для async stream — `Flow`.
- Java Streams використовуй для Java interop або існуючих Java APIs.
- Streams із I/O закривай через `use`.
- `parallelStream()` використовуй дуже обережно.

**Коротко:** Java Streams у Kotlin працюють, але не є основним інструментом. У
Kotlin зазвичай краще використовувати collection operators, `Sequence` для lazy
обробки і `Flow` для асинхронних потоків.

</details>
<details>
<summary>78. Що таке Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

Kotlin Multiplatform Mobile, або KMM, — це підхід до розробки мобільних
застосунків, де спільна бізнес-логіка пишеться на Kotlin і використовується на
Android та iOS. Сьогодні частіше говорять ширше — Kotlin Multiplatform (KMP),
бо технологія не обмежується тільки mobile: вона може таргетити JVM, Android,
iOS, desktop, web і native-платформи.

1. **Основна ідея**

KMM не означає "один UI для Android і iOS". Основна цінність — спільний
shared-код:

```text
shared Kotlin code
    -> Android app
    -> iOS app
```

У shared module зазвичай виносять:

- business logic;
- use cases;
- repositories;
- validation;
- networking;
- serialization;
- local persistence abstraction;
- domain models;
- analytics contracts.

UI часто залишається native: Android пишеться на Jetpack Compose/View System, а
iOS — на SwiftUI/UIKit.

2. **Типова структура**

```text
:shared
    commonMain
    androidMain
    iosMain

:androidApp
iosApp
```

`commonMain` — код, спільний для всіх платформ:

```kotlin
class LoadUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User {
        return repository.loadUser(id)
    }
}
```

`androidMain` — Android-specific реалізації.

`iosMain` — iOS-specific реалізації.

3. **commonMain**

У `commonMain` пишуть код, який не залежить від Android або iOS API:

```kotlin
data class User(
    val id: String,
    val name: String
)

interface UserRepository {
    suspend fun loadUser(id: String): User
}
```

Цей код може використовуватись і Android, і iOS застосунком.

4. **expect/actual**

Якщо API відрізняється між платформами, використовують `expect/actual`.

У `commonMain`:

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

В `androidMain`:

```kotlin
actual class PlatformLogger {
    actual fun log(message: String) {
        Log.d("App", message)
    }
}
```

В `iosMain`:

```kotlin
actual class PlatformLogger {
    actual fun log(message: String) {
        println(message)
    }
}
```

Так common-код працює з єдиним контрактом, а platform-код дає конкретну
реалізацію.

5. **Networking**

Для shared networking часто використовують Ktor Client:

```kotlin
class UserApi(
    private val client: HttpClient
) {
    suspend fun loadUser(id: String): UserDto {
        return client.get("https://example.com/users/$id").body()
    }
}
```

Ktor має engines для різних платформ, тому common API може залишатися спільним,
а engine підставляється platform-specific.

6. **Serialization**

Для спільної серіалізації часто використовують `kotlinx.serialization`:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String
)
```

Це добре підходить для KMP, бо не привʼязує моделі до Java reflection або
Android-specific runtime.

7. **Coroutines у KMM**

Coroutines підтримуються в Kotlin Multiplatform:

```kotlin
class UserRepository(
    private val api: UserApi
) {
    suspend fun loadUser(id: String): User {
        return api.loadUser(id).toDomain()
    }
}
```

На Android suspend-функції викликаються природно з `viewModelScope`.
На iOS їх треба експортувати у Swift-friendly API або обгорнути в async/await
залежно від setup.

8. **Що не треба виносити в shared**

Не все варто шарити. Зазвичай не варто виносити:

- platform-specific UI;
- Android `Context`;
- iOS `UIViewController`;
- navigation, якщо вона сильно platform-specific;
- permission handling без abstraction;
- SDK, який існує тільки на одній платформі;
- код, який стає складнішим через sharing, ніж через дублювання.

Shared-код має зменшувати складність, а не створювати абстракції заради
абстракцій.

9. **Переваги KMM**

- Менше дублювання бізнес-логіки.
- Однакова domain-логіка на Android та iOS.
- Спільні моделі, validation і networking.
- Kotlin type safety і coroutines у shared layer.
- Native UI і native UX залишаються можливими.
- Bugs у shared business logic фіксяться одразу для двох платформ.

10. **Компроміси**

KMM додає свої витрати:

- складніший build setup;
- треба розуміти Gradle Multiplatform;
- iOS interop має нюанси;
- не всі бібліотеки підтримують KMP;
- debugging між Kotlin/Swift може бути складнішим;
- команда має мати Kotlin expertise не тільки на Android стороні.

Тобто KMM не є автоматичним "дешевше вдвічі". Він корисний, коли є значна
частина спільної логіки і команда готова підтримувати multiplatform stack.

11. **KMM vs cross-platform UI**

KMM відрізняється від Flutter або React Native:

```text
Flutter / React Native -> shared UI + shared logic
KMM/KMP               -> shared logic, native UI
```

Хоча Compose Multiplatform розширює можливості shared UI, класичний KMM-підхід
для mobile — це саме shared business layer і native presentation layer.

12. **Практичне правило**

KMM варто розглядати, якщо:

- Android і iOS мають однакову бізнес-логіку;
- важлива native UI/UX якість;
- команда хоче уникнути дублювання domain/data layer;
- є готовність інвестувати в KMP tooling;
- shared layer можна тримати чистим від platform-specific API.

**Коротко:** KMM — це використання Kotlin Multiplatform для мобільної розробки, де
Android та iOS ділять бізнес-логіку, але можуть мати власний native UI. Це не
"написати весь застосунок один раз", а спосіб винести спільну логіку в один
типобезпечний Kotlin-модуль.

</details>
<details>
<summary>79. Які обмеження має Kotlin Multiplatform?</summary>

#### Kotlin

Kotlin Multiplatform дає можливість шарити код між платформами, але не робить
усі платформи однаковими. Його головне обмеження: спільний код має жити на
перетині можливостей платформ. Усе, що залежить від Android SDK, iOS SDK,
platform lifecycle, UI або специфічних бібліотек, треба або залишати в
platform-specific коді, або ховати за абстракціями.

1. **Не весь код можна шарити**

У `commonMain` не можна напряму використовувати Android API:

```kotlin
class LocationProvider(
    private val context: Context
)
```

`Context` існує тільки на Android, тому такий код не може бути common.

Краще:

```kotlin
interface LocationProvider {
    suspend fun getCurrentLocation(): Location
}
```

А реалізації робити в `androidMain` і `iosMain`.

2. **UI зазвичай platform-specific**

Класичний KMM підхід не шарить UI:

```text
Android UI -> Jetpack Compose / Views
iOS UI     -> SwiftUI / UIKit
shared     -> business/data/domain logic
```

Compose Multiplatform дозволяє шарити UI в частині сценаріїв, але це вже інший
рівень trade-off-ів. Для багатьох production mobile apps native UI досі дає
кращий контроль над platform UX, accessibility і ecosystem integration.

3. **Не всі бібліотеки підтримують KMP**

Java/JVM бібліотека не автоматично працює на iOS:

```kotlin
implementation("some.jvm.only:library")
```

Якщо library залежить від JVM reflection, Java APIs або Android SDK, її не
можна використати в `commonMain`.

Для KMP треба шукати multiplatform libraries:

- Ktor Client для networking;
- kotlinx.serialization для JSON;
- SQLDelight або інші KMP-friendly storage рішення;
- kotlinx-datetime для часу;
- multiplatform settings/storage libraries.

4. **iOS interop має нюанси**

Kotlin-код експортується в Swift/Objective-C не завжди ідеально:

```kotlin
suspend fun loadUser(id: String): User
```

У Swift виклик може вимагати bridging або спеціальної обгортки. Також є нюанси
з:

- sealed classes;
- generics;
- Kotlin collections;
- exceptions;
- coroutines;
- Flow;
- naming;
- binary frameworks.

Тому shared API для iOS треба проектувати свідомо, а не просто експортувати
внутрішній Kotlin API.

5. **Flow на iOS**

`Flow` дуже природний для Kotlin, але Swift не працює з ним нативно так само.
Потрібні wrappers або адаптери:

```kotlin
fun observeUser(): Flow<User>
```

Для iOS може знадобитися API, який конвертує stream у callback, Combine,
AsyncSequence або інший Swift-friendly формат.

Це не blocker, але це додатковий integration layer.

6. **Build complexity**

KMP Gradle setup складніший за single-platform:

```kotlin
kotlin {
    androidTarget()
    iosArm64()
    iosSimulatorArm64()

    sourceSets {
        commonMain.dependencies {
            implementation(libs.ktor.client.core)
        }
    }
}
```

Треба розуміти:

- targets;
- source sets;
- Gradle metadata;
- CocoaPods/SPM integration;
- binary frameworks;
- expect/actual;
- platform-specific dependencies.

Це підвищує вхідний поріг для команди.

7. **Debugging і tooling**

Android debugging Kotlin-коду зазвичай комфортний. На iOS debugging shared
Kotlin-коду може бути менш зручним, особливо коли проблема проходить через
Swift/Kotlin boundary.

Можливі складності:

- stack traces менш очевидні;
- debugging coroutines/Flow на iOS;
- source maps/debug symbols;
- Xcode integration;
- CI для iOS targets.

Tooling стає кращим, але це все ще треба враховувати.

8. **Performance і binary size**

Shared Kotlin framework на iOS додає binary size. Також interop між Swift і
Kotlin має свою вартість.

Зазвичай це не критично для бізнес-логіки, але треба бути обережним із:

- великими object graphs;
- частими cross-boundary викликами;
- performance-critical UI loops;
- масовими callbacks між Swift і Kotlin;
- великими generated models.

Краще робити coarse-grained API між shared і platform layers, а не викликати
Kotlin з Swift тисячі разів у tight loop.

9. **Platform-specific behavior нікуди не зникає**

Android і iOS мають різні:

- lifecycle models;
- permission systems;
- background execution rules;
- push notification APIs;
- storage policies;
- networking constraints;
- UI conventions.

KMP не прибирає ці відмінності. Він дозволяє спільно реалізувати те, що справді
спільне, але platform integration все одно треба робити окремо.

10. **Over-abstraction risk**

Поганий KMP-дизайн часто виглядає так:

```kotlin
interface EverythingPlatformAdapter {
    fun doPlatformThing()
    fun doAnotherPlatformThing()
    fun renderSomething()
}
```

Якщо shared-код починає керувати всім через десятки platform adapters, система
стає складнішою, ніж два окремі native implementations.

KMP має зменшувати дублювання, а не створювати абстрактну платформу поверх
Android і iOS.

11. **Командні обмеження**

KMP вимагає, щоб команда розуміла:

- Kotlin;
- Gradle;
- Android build;
- iOS build/distribution;
- Swift interop;
- multiplatform architecture.

Якщо iOS-команда не готова працювати зі shared Kotlin framework, adoption може
бути організаційно складнішим, ніж технічно.

12. **Практичне правило**

KMP добре підходить для:

- domain logic;
- validation;
- networking;
- serialization;
- repositories;
- use cases;
- shared algorithms.

KMP обережно застосовувати для:

- UI;
- navigation;
- platform lifecycle;
- permission handling;
- platform SDK integrations;
- performance-critical platform boundary calls.

**Коротко:** головне обмеження Kotlin Multiplatform — він шарить не платформу, а
код, який можна коректно виконати на кількох платформах. Найкращий KMP-дизайн
тримає shared layer чистим і не намагається приховати реальні відмінності
Android та iOS.

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

Оптимізація Kotlin-коду починається не з мікрооптимізацій, а з вимірювання. Спочатку треба знайти bottleneck через profiler, traces, benchmarks або logs, і тільки потім міняти код.

1. **Не оптимізувати без вимірювання**

Правильний порядок:

```text
measure -> find bottleneck -> optimize -> measure again
```

Без цього легко “оптимізувати” код, який не впливає на реальну проблему.

2. **Уникати зайвих allocations**

У гарячих місцях зайві обʼєкти створюють GC pressure:

```kotlin
fun render(items: List<Item>) {
    items.map { it.toUi() }.forEach { draw(it) }
}
```

Якщо це hot path, краще уникати проміжних колекцій:

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

3. **Collections API**

`map/filter/flatMap` зручні, але можуть створювати проміжні списки:

```kotlin
val names = users
    .filter { it.isActive }
    .map { it.name }
```

Для невеликих списків це нормально. Для великих або hot path можна використати sequence або простий loop:

```kotlin
val names = users.asSequence()
    .filter { it.isActive }
    .map { it.name }
    .toList()
```

4. **Inline functions**

`inline` зменшує overhead lambda allocation для higher-order functions:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Але `inline` не треба ставити всюди. Він може збільшити bytecode, якщо функція велика.

5. **Coroutines**

Оптимізація coroutine-коду — це правильні dispatchers і відсутність blocking:

```kotlin
withContext(Dispatchers.IO) {
    api.loadData()
}
```

Погано:

```kotlin
runBlocking {
    repository.load()
}
```

`runBlocking` у UI або request path може заблокувати thread і створити ANR/latency.

6. **String operations**

Для багаторазової конкатенації краще `buildString`:

```kotlin
val result = buildString {
    items.forEach { append(it.name).append('
') }
}
```

У циклах `result += value` може створювати багато проміжних рядків.

7. **Data classes і copy**

`copy()` зручний, але у великих state-обʼєктах часті оновлення можуть створювати багато allocations:

```kotlin
_state.update { it.copy(items = newItems) }
```

Це нормально для UI state, але якщо state дуже великий або оновлюється часто, краще декомпозувати state.

8. **Boxing і primitives**

Generic collections типу `List<Int>` можуть мати boxing overhead. У більшості Android-коду це не критично, але для tight loops, audio/video/math обчислень краще використовувати primitive arrays:

```kotlin
val values = IntArray(1000)
```

9. **Compose performance**

Для Compose важливо:

- не створювати важкі обʼєкти в composable без `remember`;
- передавати stable models;
- правильно використовувати keys у lazy lists;
- не робити I/O в composition;
- декомпозувати state, щоб не recomposition-ити весь екран.

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

10. **Android-specific речі**

- не блокувати main thread;
- кешувати expensive objects;
- використовувати Room/Retrofit async APIs;
- оптимізувати bitmap loading через image loader;
- профілювати startup, memory і rendering;
- уникати reflection там, де це hot path.

11. **Практичне правило**

Оптимізація має бути локальною і підтвердженою вимірами. Найчастіше найбільший виграш дають не дрібні Kotlin-трюки, а правильний threading, менше allocations у hot paths, оптимізований UI rendering і нормальна архітектура data flow.

**Коротко:** продуктивність Kotlin-коду оптимізують через вимірювання, усунення зайвих allocations, правильне використання collections, dispatchers, inline, string operations, primitive arrays і Compose best practices. Спочатку profiler, потім зміни, потім повторна перевірка.

</details>
<details>
<summary>82. Як реалізувати retry з exponential backoff у Kotlin?</summary>

#### Kotlin

`Retry with exponential backoff` — це повторна спроба виконати операцію з паузою, яка зростає після кожної невдалої спроби. Це корисно для тимчасових network/server errors, але небезпечно для помилок, які не виправляться повтором.

1. **Базова ідея**

```text
attempt 1 -> delay 1s
attempt 2 -> delay 2s
attempt 3 -> delay 4s
attempt 4 -> delay 8s
```

Зазвичай додають max delay, max attempts і jitter, щоб не створити одночасне навантаження на сервер.

2. **Простий suspend retry**

```kotlin
suspend fun <T> retryWithBackoff(
    maxAttempts: Int = 3,
    initialDelayMillis: Long = 1_000,
    maxDelayMillis: Long = 10_000,
    factor: Double = 2.0,
    block: suspend () -> T
): T {
    var currentDelay = initialDelayMillis

    repeat(maxAttempts - 1) { attempt ->
        try {
            return block()
        } catch (e: IOException) {
            delay(currentDelay)
            currentDelay = (currentDelay * factor)
                .toLong()
                .coerceAtMost(maxDelayMillis)
        }
    }

    return block()
}
```

Цей варіант retry-ить тільки `IOException`. Інші помилки не треба автоматично повторювати без причини.

3. **Використання**

```kotlin
val user = retryWithBackoff {
    api.getUser(userId)
}
```

Якщо API тимчасово недоступне, буде кілька повторних спроб із delay.

4. **Jitter**

Jitter додає випадковість до delay:

```kotlin
val jitter = Random.nextLong(0, 500)
delay(currentDelay + jitter)
```

Це важливо, коли багато clients одночасно retry-ять після помилки. Без jitter вони можуть одночасно вдарити по серверу.

5. **Retry predicate**

Краще явно вирішувати, які помилки retry-ити:

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in listOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Наприклад, `401`, `403`, `404` зазвичай не мають retry-итись.

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

Це зручно для streams, але треба контролювати кількість attempts.

7. **Timeout**

Retry не замінює timeout:

```kotlin
withTimeout(15_000) {
    retryWithBackoff {
        api.loadData()
    }
}
```

Без timeout retry може зробити UX занадто довгим.

8. **Cancellation**

`delay()` cancellable, тому coroutine cancellation працює нормально. Не треба ловити `CancellationException` як звичайну помилку для retry.

```kotlin
catch (e: CancellationException) {
    throw e
}
```

9. **Коли retry не потрібен**

Не retry-ити:

- validation errors;
- auth errors без refresh token logic;
- 404;
- business rule errors;
- non-idempotent operations без idempotency key;
- помилки, які користувач має виправити сам.

10. **Практичне правило**

Retry має бути обмежений, cancellable, з backoff, jitter, timeout і чітким predicate. Інакше він може погіршити UX, навантажити backend або дублювати операції.

**Коротко:** exponential backoff — це retry з паузою, яка зростає після кожної спроби. У Kotlin його роблять через suspend-функцію або `Flow.retryWhen`, додаючи max attempts, max delay, jitter, timeout, cancellation handling і перевірку, які помилки можна retry-ити.

</details>
<details>
<summary>83. Які нові можливості Kotlin варто знати у 2026 році?</summary>

#### Kotlin

У 2026 році сильний Kotlin-розробник має орієнтуватися в Kotlin 2.x, K2 compiler, розвитку Multiplatform, нових language features, tooling і Compose compiler. Не обовʼязково використовувати кожну нову фічу в production, але треба розуміти, що вже стало базою екосистеми і які ризики є при міграції.

1. **Kotlin 2.x і K2 compiler**

K2 — це нова compiler architecture, яка стала фундаментом сучасного Kotlin.

Практично це означає:

- кращу compiler analysis;
- швидший розвиток language features;
- сильнішу основу для IDE/tooling;
- стабільнішу Multiplatform-історію;
- поступовий відхід від Kotlin 1.x як baseline.

Для Android-команди це питання не тільки синтаксису, а й AGP, Gradle, Compose compiler, kapt/ksp і library compatibility.

2. **Context parameters**

Context parameters дозволяють функції вимагати контекст, який доступний у scope:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving user $id")
}
```

Це корисно для DSL, scoped APIs, logging, transactions, typed context. Але на співбесіді важливо сказати: це не заміна DI і не привід робити прихований service locator.

3. **Explicit backing fields**

Explicit backing fields дають більше контролю над property storage:

```kotlin
var name: String = ""
    field = value.trim()
```

Практична користь:

- менше boilerplate;
- чистіші custom getter/setter;
- явніше вираження invariants;
- зручніше проектувати property API.

4. **Покращення exhaustiveness і sealed state**

Kotlin продовжує покращувати аналіз `when`, особливо для sealed hierarchies.

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data class Error(val message: String) : UiState
}

fun render(state: UiState) {
    when (state) {
        UiState.Loading -> showLoading()
        is UiState.Content -> showContent(state.user)
        is UiState.Error -> showError(state.message)
    }
}
```

Для Android, MVI і Compose це важливо, бо UI state machines стають безпечнішими.

5. **Unused return value checks**

У нових Kotlin 2.x релізах розвивається напрям перевірок для результатів, які не можна безпечно ігнорувати.

Проблема:

```kotlin
repository.saveUser(user)
```

Якщо `saveUser()` повертає `Result`, але caller його ігнорує, це потенційний bug.

Краще:

```kotlin
val result = repository.saveUser(user)

if (result.isFailure) {
    showError()
}
```

Це важливо для API-дизайну: результат операції має бути або використаний, або явно проігнорований.

6. **Kotlin Multiplatform і Swift export**

KMP у 2026 році важливо знати хоча б концептуально:

```text
common Kotlin logic
 -> Android
 -> iOS
 -> JVM backend
 -> Web/Wasm
```

Для mobile особливо важливі Swift export і Kotlin/Native interop. Shared Kotlin API має бути зручним для iOS-команди, тому треба обережно проектувати boundaries: `Flow`, sealed types, generics і exceptions не завжди природно виглядають у Swift.

7. **Kotlin/Wasm**

Kotlin/Wasm — це ще один target для Kotlin, не заміна Android. Для Android-розробника достатньо розуміти, що Kotlin рухається до ширшої multiplatform-екосистеми, де частину logic можна шарити між mobile, backend і web/runtime сценаріями.

8. **Standard library improvements**

У Kotlin 2.x розвиваються common stdlib API, корисні для Multiplatform:

- UUID;
- time-related APIs;
- Base64/Hex utilities;
- collection/helper improvements.

Практична цінність: менше platform-specific wrappers і сторонніх залежностей для базових задач у shared code.

9. **Gradle і build tooling**

Для техліда tooling не менш важливий за language features:

- Kotlin Gradle Plugin compatibility;
- Gradle/AGP versions;
- KSP/kapt migration risks;
- generated sources;
- version catalogs;
- binary compatibility checks;
- KMP dependency setup.

Погано налаштований build може коштувати команді більше, ніж відсутність нової мовної фічі.

10. **Compose compiler і diagnostics**

Compose compiler у Kotlin 2.x став тісніше інтегрований з Kotlin tooling. Для Android це важливо через:

- сумісність Kotlin/Compose/AGP;
- recomposition diagnostics;
- stack traces;
- minified app debugging;
- compiler metrics.

Якщо команда пише Compose, оновлення Kotlin треба перевіряти разом із Compose compiler і Android Studio.

11. **Що очікувати на співбесіді**

Я б очікував, що Kotlin-розробник у 2026 році знає:

- Kotlin 2.x/K2 як baseline;
- context parameters і їх ризики;
- explicit backing fields;
- sealed state + exhaustive `when`;
- KMP/KMM і Swift interop на концептуальному рівні;
- Kotlin/Wasm як частину ecosystem;
- stdlib improvements для shared code;
- Gradle/AGP/KSP/Compose compatibility;
- що нові фічі треба оцінювати перед production adoption.

12. **Практичне правило**

Нову Kotlin-фічу не треба тягнути в production автоматично. Оцінювати треба:

- stability level;
- IDE support;
- Android Studio/AGP compatibility;
- Gradle impact;
- library ecosystem;
- team readiness;
- migration cost;
- реальну користь для коду.

**Коротко:** у 2026 році варто знати Kotlin 2.x, K2 compiler, context parameters, explicit backing fields, розвиток KMP/Swift export, Wasm-напрям, stdlib/tooling покращення і Compose compiler compatibility. Але техлід має оцінювати ці фічі прагматично, а не впроваджувати все одразу.

</details>
<details>
<summary>84. Що таке життєвий цикл Activity?</summary>

#### Kotlin

Життєвий цикл `Activity` — це набір callbacks, через які Android повідомляє, що екран створюється, стає видимим, переходить у foreground/background або знищується. Розуміння lifecycle потрібне для коректного керування UI, resources, subscriptions і state.

1. **Основні callbacks**

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Додатково є `onRestart()`, коли Activity повертається після `onStop()`.

2. **onCreate**

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

Тут ініціалізують layout, ViewModel binding, navigation, базовий setup. Важку роботу в `onCreate()` робити не треба.

3. **onStart**

Activity стає видимою користувачу, але ще може бути не в focus. Тут можна стартувати lightweight UI-related observers, якщо вони потрібні саме коли екран видимий.

4. **onResume**

Activity у foreground і готова до взаємодії:

```kotlin
override fun onResume() {
    super.onResume()
    // resume camera, sensors, animations
}
```

Тут часто відновлюють camera, sensors, location updates або UI animations.

5. **onPause**

Activity частково втрачає focus. Callback має бути швидким:

```kotlin
override fun onPause() {
    super.onPause()
    // pause animations, camera preview, sensors
}
```

Не можна робити довгі synchronous operations, бо це затримає перехід до іншої Activity.

6. **onStop**

Activity більше не видима. Тут зупиняють роботу, яка потрібна тільки видимому екрану: observers, location updates, playback preview, expensive UI resources.

7. **onDestroy**

Activity знищується через finish або system recreation:

```kotlin
override fun onDestroy() {
    super.onDestroy()
}
```

Не треба покладатись на `onDestroy()` для гарантованого збереження важливих даних. Процес можуть вбити без нормального завершення.

8. **Configuration change**

При rotation/language/theme change Activity може бути пересоздана:

```text
old Activity -> onPause -> onStop -> onDestroy
new Activity -> onCreate -> onStart -> onResume
```

State, який має пережити recreation, тримають у ViewModel, `savedInstanceState`, `rememberSaveable` або persistent storage.

9. **SavedInstanceState**

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString("query", query)
    super.onSaveInstanceState(outState)
}
```

Підходить для невеликих UI state, але не для великих обʼєктів або бізнес-даних.

10. **Lifecycle-aware APIs**

Замість ручного старт/стоп часто краще використовувати lifecycle-aware підходи:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state -> render(state) }
    }
}
```

`repeatOnLifecycle` автоматично стартує collection при `STARTED` і скасовує при `STOPPED`.

11. **Практичне правило**

- `onCreate` — setup.
- `onStart` — екран видимий.
- `onResume` — активна взаємодія.
- `onPause` — швидко призупинити foreground-only речі.
- `onStop` — зупинити visible-only роботу.
- `onDestroy` — cleanup, але не гарантія збереження даних.
- State — ViewModel/SavedState/persistent storage.

**Коротко:** життєвий цикл Activity описує переходи екрана між created, visible, foreground, background і destroyed станами. Основні callbacks: `onCreate`, `onStart`, `onResume`, `onPause`, `onStop`, `onDestroy`. Для state і observers краще використовувати ViewModel, SavedState і lifecycle-aware APIs.

</details>
<details>
<summary>85. Що таке Fragment і чим він відрізняється від Activity?</summary>

#### Kotlin

`Fragment` — це reusable частина UI і поведінки, яка живе всередині `Activity`.
`Activity` є окремим Android component із власним window, lifecycle і entry
point у системі. `Fragment` не існує повністю самостійно: він привʼязаний до
host `Activity` або іншого `Fragment` і має власний lifecycle, включно з окремим
lifecycle для view.

1. **Activity**

`Activity` — це екран або host для UI:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Вона:

- має власне window;
- реєструється в `AndroidManifest`;
- може запускатися через `Intent`;
- є entry point для системи;
- керує верхнім рівнем navigation/container-ом;
- має lifecycle `onCreate`, `onStart`, `onResume`, `onPause`, `onStop`,
  `onDestroy`.

2. **Fragment**

`Fragment` — це частина UI всередині Activity:

```kotlin
class UserFragment : Fragment(R.layout.fragment_user) {

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }
}
```

Fragment зазвичай використовують для:

- reusable screen sections;
- navigation destinations;
- master-detail layouts;
- tablet/phone adaptive UI;
- розділення великого Activity UI на частини.

3. **Головна різниця**

`Activity` — це системний component.

`Fragment` — це UI/controller component, який живе всередині host-а.

```text
Activity
 └── Fragment
      └── View
```

Activity може існувати без Fragment. Fragment без Activity/host-а нормально
працювати не може.

4. **Lifecycle Fragment**

Fragment має lifecycle:

```text
onAttach()
onCreate()
onCreateView()
onViewCreated()
onStart()
onResume()
onPause()
onStop()
onDestroyView()
onDestroy()
onDetach()
```

Ключова відмінність: у Fragment є lifecycle самого Fragment і lifecycle його
View.

5. **View lifecycle**

View Fragment-а може бути знищена раніше, ніж сам Fragment:

```kotlin
override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```

Це критично для ViewBinding:

```kotlin
private var _binding: FragmentUserBinding? = null
private val binding get() = _binding!!

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    _binding = FragmentUserBinding.bind(view)
}

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Якщо не очистити binding, Fragment може тримати стару View і створити memory
leak.

6. **viewLifecycleOwner**

Для collection у Fragment треба використовувати `viewLifecycleOwner`, а не
сам Fragment lifecycle:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

Погано:

```kotlin
lifecycleScope.launch {
    viewModel.state.collect { render(it) }
}
```

Тут collection може пережити view і звертатися до binding після
`onDestroyView()`.

7. **Navigation**

Activity часто є host-ом для `NavHostFragment`:

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/nav_host"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:navGraph="@navigation/main_graph" />
```

А окремі Fragment-и є destinations у navigation graph:

```kotlin
findNavController().navigate(R.id.userDetailsFragment)
```

У modern Android часто роблять single-activity architecture: одна Activity, а
екрани — Fragment-и або Compose destinations.

8. **Communication**

Fragment не має напряму керувати іншими Fragment-ами через жорсткі references.
Краще використовувати:

- shared ViewModel;
- Fragment Result API;
- navigation arguments;
- interface callbacks у простих legacy-сценаріях.

Shared ViewModel:

```kotlin
private val viewModel: SharedViewModel by activityViewModels()
```

Fragment Result API:

```kotlin
parentFragmentManager.setFragmentResult(
    "user_result",
    bundleOf("user_id" to userId)
)
```

9. **Activity-scoped і Fragment-scoped ViewModel**

Fragment-scoped:

```kotlin
private val viewModel: UserViewModel by viewModels()
```

Activity-scoped:

```kotlin
private val sharedViewModel: SharedViewModel by activityViewModels()
```

Перший живе в межах Fragment. Другий — у межах Activity і може шаритися між
кількома Fragment-ами.

10. **Коли Fragment доречний**

Fragment доречний, якщо:

- проєкт використовує View-based UI;
- потрібна Navigation Component інтеграція;
- є reusable screen sections;
- потрібен adaptive layout;
- legacy app уже побудований на fragments;
- треба поєднувати View system і поступову міграцію.

У повністю Compose-first застосунках Fragment-и часто не потрібні для кожного
екрана, але Activity все одно залишається host-ом.

11. **Типові помилки**

- Тримати binding після `onDestroyView`.
- Використовувати `lifecycleScope` замість `viewLifecycleOwner.lifecycleScope`
  для UI collection.
- Робити Fragment занадто великим.
- Зберігати Fragment instance у singleton.
- Викликати `requireActivity()`/`requireContext()` після detach.
- Напряму звʼязувати Fragment-и між собою.
- Ігнорувати різницю між Fragment lifecycle і View lifecycle.

12. **Практичне правило**

- `Activity` — системний host/window/entry point.
- `Fragment` — частина UI всередині host-а.
- У Fragment завжди думати про `viewLifecycleOwner`.
- Binding очищати в `onDestroyView`.
- State тримати у ViewModel, не у Fragment fields.
- Fragment-и не мають жорстко знати один про одного.

**Коротко:** Activity — це повноцінний Android component, а Fragment — модульна
частина UI всередині Activity. Найважливіша практична різниця для розробника:
у Fragment є окремий lifecycle view, і саме його треба використовувати для
binding, subscriptions і UI-related coroutine collection.

</details>
<details>
<summary>86. Що таке Intent і як він використовується?</summary>

#### Kotlin

`Intent` — це Android-механізм для опису дії, яку потрібно виконати. Через
`Intent` можна запускати `Activity`, стартувати `Service`, відправляти broadcast,
передавати дані між компонентами або попросити інший застосунок виконати дію:
відкрити браузер, подзвонити, поділитися текстом, вибрати файл.

1. **Основна ідея**

`Intent` не виконує дію сам. Він описує намір:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
startActivity(intent)
```

Тут ми кажемо Android system: "запусти `DetailsActivity`".

2. **Explicit Intent**

Explicit intent явно вказує component:

```kotlin
val intent = Intent(this, UserActivity::class.java)
startActivity(intent)
```

Його використовують для переходів усередині свого застосунку, коли точно відомо,
яку `Activity` треба відкрити.

Передача параметрів:

```kotlin
val intent = Intent(this, UserActivity::class.java).apply {
    putExtra("user_id", userId)
}

startActivity(intent)
```

Отримання:

```kotlin
val userId = intent.getStringExtra("user_id")
```

3. **Implicit Intent**

Implicit intent описує дію, але не конкретний component:

```kotlin
val intent = Intent(Intent.ACTION_VIEW).apply {
    data = Uri.parse("https://kotlinlang.org")
}

startActivity(intent)
```

Android знайде застосунок, який може обробити `ACTION_VIEW` для такого URI,
наприклад браузер.

4. **Приклади implicit intents**

Відкрити dialer:

```kotlin
val intent = Intent(Intent.ACTION_DIAL).apply {
    data = Uri.parse("tel:+380501234567")
}
startActivity(intent)
```

Поділитися текстом:

```kotlin
val intent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"
    putExtra(Intent.EXTRA_TEXT, "Hello from app")
}

startActivity(Intent.createChooser(intent, "Share via"))
```

Відкрити email client:

```kotlin
val intent = Intent(Intent.ACTION_SENDTO).apply {
    data = Uri.parse("mailto:")
    putExtra(Intent.EXTRA_EMAIL, arrayOf("support@example.com"))
    putExtra(Intent.EXTRA_SUBJECT, "Support request")
}

startActivity(intent)
```

5. **Intent extras**

Extras — це key-value дані:

```kotlin
intent.putExtra("screen_title", "User details")
intent.putExtra("is_edit_mode", true)
intent.putExtra("user_age", 25)
```

Читання:

```kotlin
val title = intent.getStringExtra("screen_title")
val isEditMode = intent.getBooleanExtra("is_edit_mode", false)
val age = intent.getIntExtra("user_age", 0)
```

Для складних обʼєктів використовують `Parcelable` або `Serializable`, але в
Android зазвичай краще `Parcelable`.

6. **Не передавати великі обʼєкти через Intent**

Погано:

```kotlin
intent.putExtra("large_bitmap", bitmap)
```

Intent extras проходять через Binder transaction, і великі дані можуть
спричинити `TransactionTooLargeException`.

Краще передавати ID:

```kotlin
intent.putExtra("user_id", user.id)
```

А деталі завантажувати з repository/database/cache на цільовому екрані.

7. **Activity Result API**

Старий підхід `startActivityForResult` deprecated. Сучасний підхід —
Activity Result API:

```kotlin
private val pickImage =
    registerForActivityResult(ActivityResultContracts.GetContent()) { uri ->
        if (uri != null) {
            viewModel.onImageSelected(uri)
        }
    }

fun openPicker() {
    pickImage.launch("image/*")
}
```

Це lifecycle-aware і краще працює з configuration changes.

8. **Intent filters**

Щоб Activity могла обробити implicit intent, у manifest задають intent-filter:

```xml
<activity android:name=".DeepLinkActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data
            android:scheme="https"
            android:host="example.com" />
    </intent-filter>
</activity>
```

Так Activity може відкриватися по deep link.

9. **Перевіряти, чи є handler**

Для implicit intent краще перевіряти, чи є застосунок, який його обробить:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))

if (intent.resolveActivity(packageManager) != null) {
    startActivity(intent)
}
```

Інакше можна отримати `ActivityNotFoundException`.

10. **Flags**

Intent flags змінюють поведінку запуску:

```kotlin
val intent = Intent(this, MainActivity::class.java).apply {
    flags = Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP
}
```

Поширені flags:

- `FLAG_ACTIVITY_NEW_TASK`;
- `FLAG_ACTIVITY_CLEAR_TOP`;
- `FLAG_ACTIVITY_SINGLE_TOP`;
- `FLAG_GRANT_READ_URI_PERMISSION`;
- `FLAG_GRANT_WRITE_URI_PERMISSION`.

Flags треба використовувати обережно, бо вони впливають на back stack.

11. **Security**

Не можна сліпо довіряти даним з Intent, особливо якщо Activity exported або
обробляє deep links:

```kotlin
val userId = intent.getStringExtra("user_id")
```

Дані треба валідувати:

```kotlin
val userId = intent.getStringExtra("user_id")
    ?.takeIf { it.isNotBlank() }
    ?: return finish()
```

Також не треба передавати секрети через implicit intents.

12. **Практичне правило**

- Explicit intent — для запуску конкретного component у своєму app.
- Implicit intent — для дії, яку може виконати інший app.
- Через extras передавати малі primitive/Parcelable дані.
- Великі обʼєкти не передавати, краще передати ID або URI.
- Для результатів використовувати Activity Result API.
- Для implicit intents перевіряти handler.
- Дані з Intent завжди валідувати.

**Коротко:** `Intent` — це повідомлення-наміp для Android system або іншого
component-а. Він використовується для navigation, deep links, sharing, запуску
зовнішніх застосунків і передачі невеликих даних між Android components.

</details>
<details>
<summary>87. Що станеться, якщо не викликати super.onCreate()?</summary>

#### Kotlin

Якщо в `Activity` не викликати `super.onCreate(savedInstanceState)`, Android
framework і базові класи не зможуть виконати свою частину ініціалізації lifecycle.
Найчастіше застосунок впаде з exception або буде працювати некоректно. У
`AppCompatActivity`, `ComponentActivity`, `FragmentActivity` і Hilt/Compose
інфраструктурі виклик `super` особливо критичний.

1. **Правильний код**

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

`super.onCreate()` має бути викликаний до більшості framework-dependent
операцій: `setContentView`, Fragment setup, Compose `setContent`, Hilt injection
у частині сценаріїв, lifecycle observers.

2. **Що робить super.onCreate**

Базовий клас Activity виконує внутрішню ініціалізацію:

- створює і переводить lifecycle у правильний стан;
- відновлює saved state;
- готує window/decor infrastructure;
- ініціалізує ActivityResultRegistry/SavedStateRegistry у Jetpack-класах;
- дає базовим класам шанс підключити свою поведінку;
- забезпечує коректну роботу FragmentManager у `FragmentActivity`;
- готує AppCompat-specific delegate у `AppCompatActivity`.

Тобто це не формальність. Це частина контракту з Android framework.

3. **Що буде, якщо не викликати super**

Приклад поганого коду:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        setContentView(R.layout.activity_main)
    }
}
```

Можливі наслідки:

- crash;
- `SuperNotCalledException`;
- некоректний lifecycle state;
- проблеми з fragments;
- не відновиться saved state;
- не працюватимуть Activity Result API;
- проблеми з AppCompat/Compose/Hilt integration;
- lifecycle observers не отримають коректні events.

На практиці Android часто явно перевіряє, чи був викликаний `super`, і кидає
помилку.

4. **Чому порядок важливий**

Зазвичай:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

Спочатку base class ініціалізує framework state, потім твій код працює з UI.

У деяких framework-ах або старих патернах є винятки щодо порядку певних
операцій, але для стандартного Android-коду виклик `super.onCreate()` на
початку — правильний default.

5. **Fragment-и теж мають super**

У Fragment lifecycle callback-ах також треба викликати `super`, якщо метод
цього вимагає:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
}

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
}
```

Деякі Fragment callbacks можуть не мати критичної логіки в `super`, але правило
команди має бути простим: якщо override-иш lifecycle callback — викликай
`super`, якщо документація явно не каже інакше.

6. **onDestroy/onPause/onStop**

У lifecycle cleanup callback-ах порядок може бути предметом командного стилю,
але `super` все одно потрібен:

```kotlin
override fun onDestroy() {
    cleanup()
    super.onDestroy()
}
```

або:

```kotlin
override fun onDestroy() {
    super.onDestroy()
    cleanup()
}
```

Для `onCreate` майже завжди `super` ставлять першим. Для `onDestroy` часто
cleanup роблять до `super`, якщо треба очистити власні ресурси до фінального
destroy. Але важливо не пропускати `super` взагалі.

7. **Hilt і super.onCreate**

Для Hilt:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {

    @Inject lateinit var analytics: Analytics

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        analytics.track("main_opened")
    }
}
```

Hilt інтегрується з Android lifecycle і generated base classes. Пропуск `super`
може зламати injection/lifecycle setup або дати crash.

8. **Compose**

У Compose Activity:

```kotlin
class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {
            App()
        }
    }
}
```

`ComponentActivity` ініціалізує lifecycle, saved state і activity result
інфраструктуру. Без `super.onCreate()` Compose content може працювати
некоректно або застосунок впаде.

9. **Чому це питання часто питають**

Це питання перевіряє, чи розробник розуміє Android lifecycle як framework
contract, а не просто набір методів.

Правильна відповідь не "бо так треба", а:

```text
base class має виконати свою lifecycle/framework ініціалізацію
```

Якщо її пропустити, твій subclass працює поверх неініціалізованого framework
state.

10. **Практичне правило**

- У `Activity.onCreate()` викликати `super.onCreate(savedInstanceState)` на
  початку.
- Не викликати UI/framework APIs до `super.onCreate`, якщо немає документованої
  причини.
- У lifecycle callback-ах Fragment/Activity не пропускати `super`.
- Якщо змінюєш порядок `super`, розуміти чому.
- Не розглядати `super` як boilerplate: це частина lifecycle contract.

**Коротко:** якщо не викликати `super.onCreate()`, Android base class не виконає
обовʼязкову ініціалізацію Activity. Результат — crash, некоректний lifecycle,
зламаний saved state, fragments, Compose, Hilt або інші Jetpack-механізми.

</details>
<details>
<summary>88. Що таке View Binding і чому він кращий за findViewById?</summary>

#### Kotlin

`View Binding` — це Android-механізм, який генерує type-safe binding-клас для
кожного XML layout-файлу. Замість ручного пошуку view через `findViewById`,
розробник отримує обʼєкт binding із властивостями для всіх view, які мають
`android:id`.

1. **Проблема findViewById**

Класичний підхід:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Проблеми:

- можна помилитися з типом;
- можна звернутися до id, якого немає в layout;
- багато boilerplate;
- помилки часто проявляються в runtime;
- код гірше читається на великих екранах.

Наприклад:

```kotlin
val title = findViewById<Button>(R.id.title)
```

Якщо `R.id.title` насправді `TextView`, отримаємо runtime crash.

2. **View Binding**

Якщо є layout `activity_main.xml`, Android генерує клас:

```kotlin
ActivityMainBinding
```

В Activity:

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.title.text = "Hello"
    }
}
```

Тут `binding.title` уже має правильний тип, наприклад `TextView`.

3. **Чому View Binding type-safe**

Якщо в XML:

```xml
<TextView
    android:id="@+id/title"
    ... />
```

У binding-класі буде:

```kotlin
val title: TextView
```

Якщо view видалити або перейменувати id, код не скомпілюється. Це краще, ніж
отримати crash у runtime.

4. **View Binding у Fragment**

У Fragment треба враховувати view lifecycle:

```kotlin
class UserFragment : Fragment(R.layout.fragment_user) {

    private var _binding: FragmentUserBinding? = null
    private val binding get() = _binding!!

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        _binding = FragmentUserBinding.bind(view)

        binding.title.text = "User"
    }

    override fun onDestroyView() {
        _binding = null
        super.onDestroyView()
    }
}
```

Binding треба очищати в `onDestroyView()`, бо view Fragment-а може бути знищена
раніше, ніж сам Fragment.

5. **Чому не можна тримати binding після onDestroyView**

Погано:

```kotlin
private lateinit var binding: FragmentUserBinding
```

і не очищати його.

Fragment може залишитись у back stack, а його view буде destroyed. Якщо binding
тримає reference на стару view hierarchy, виникає memory leak.

Правильний патерн:

```kotlin
private var _binding: FragmentUserBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

6. **Переваги над findViewById**

View Binding кращий за `findViewById`, бо:

- type-safe;
- null-safe для view, які гарантовано є в layout;
- менше boilerplate;
- помилки ловляться на compile time;
- немає ручних casts;
- код краще читається;
- працює без annotation processing.

Порівняння:

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

проти:

```kotlin
binding.title.text = "Hello"
```

7. **View Binding vs Data Binding**

View Binding і Data Binding — не одне й те саме.

View Binding:

- тільки генерує binding до views;
- не має binding expressions;
- не потребує `<layout>` root;
- простіший;
- швидший у build-і;
- менше магії.

Data Binding:

- підтримує expressions у XML;
- може напряму звʼязувати data з layout;
- складніший;
- має більше build-time overhead;
- може ускладнювати debugging.

У більшості сучасних проєктів, якщо потрібен тільки type-safe доступ до views,
достатньо View Binding.

8. **View Binding і include**

Якщо layout має `<include>`, View Binding також генерує доступ:

```xml
<include
    android:id="@+id/header"
    layout="@layout/view_header" />
```

У коді:

```kotlin
binding.header.title.text = "Profile"
```

Це робить роботу з reusable layout blocks зручнішою.

9. **View Binding і nullable views**

Якщо один layout має різні configurations, наприклад `layout` і `layout-land`,
і view існує не в усіх варіантах, binding property може бути nullable:

```kotlin
binding.optionalPanel?.isVisible = true
```

Це ще одна перевага: API відображає реальну структуру layout-ів.

10. **Коли View Binding не потрібен**

Якщо екран повністю написаний на Jetpack Compose, View Binding не потрібен:

```kotlin
setContent {
    AppScreen()
}
```

View Binding актуальний для View-based UI або hybrid screens, де частина UI ще
на XML.

11. **Типові помилки**

- Не очищати binding у Fragment.
- Використовувати binding після `onDestroyView`.
- Тримати reference на binding у ViewModel.
- Передавати binding у domain/data layer.
- Плутати View Binding із Data Binding.
- Використовувати `findViewById` паралельно без потреби.

12. **Практичне правило**

- В Activity binding можна тримати як `lateinit var` на весь lifecycle Activity.
- У Fragment binding має бути nullable backing property і очищатися в
  `onDestroyView`.
- View Binding використовувати для XML/View-based UI.
- Для Compose-only екранів View Binding не потрібен.

**Коротко:** View Binding замінює ручний `findViewById` type-safe API, який
генерується з XML layout. Він зменшує boilerplate, ловить помилки на compile
time і робить роботу з views безпечнішою, особливо в Kotlin-коді.

</details>
<details>
<summary>89. Що таке LiveData?</summary>

#### Kotlin

`LiveData` — це lifecycle-aware observable data holder з Android Jetpack. Вона
зберігає значення і повідомляє observers тільки тоді, коли їхній `LifecycleOwner`
перебуває в активному стані. Історично `LiveData` була основним способом
передавати state з `ViewModel` у XML/View-based UI.

1. **Базова ідея**

`LiveData<T>` — read-only observable:

```kotlin
val user: LiveData<User>
```

`MutableLiveData<T>` — mutable версія, яку зазвичай тримають приватно у
`ViewModel`:

```kotlin
class UserViewModel : ViewModel() {

    private val _user = MutableLiveData<User>()
    val user: LiveData<User> = _user

    fun setUser(user: User) {
        _user.value = user
    }
}
```

UI бачить тільки `LiveData`, а змінювати значення може лише `ViewModel`.

2. **Lifecycle-aware observation**

У Fragment або Activity:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` відправляє updates тільки active observers. Для Fragment важливо
використовувати `viewLifecycleOwner`, а не сам Fragment:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    binding.name.text = user.name
}
```

Так observer автоматично відвʼяжеться, коли view Fragment-а буде destroyed.

3. **Active state**

Observer вважається active, коли lifecycle у стані `STARTED` або `RESUMED`.

Це означає:

- якщо екран неактивний, UI не отримує updates;
- коли екран знову активний, він отримає останнє значення;
- не треба вручну відписуватись у більшості UI-сценаріїв.

Це головна перевага `LiveData` над простим callback/listener.

4. **value vs postValue**

`value` треба встановлювати з main thread:

```kotlin
_user.value = User("1", "John")
```

`postValue` можна викликати з background thread:

```kotlin
_user.postValue(User("1", "John"))
```

Але в сучасному coroutine-коді частіше краще повернутися на main dispatcher або
оновлювати state у `viewModelScope`, ніж хаотично використовувати `postValue`.

5. **LiveData у ViewModel**

Типовий приклад:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    private val _state = MutableLiveData<UiState>(UiState.Loading)
    val state: LiveData<UiState> = _state

    fun loadUser(id: String) {
        viewModelScope.launch {
            try {
                val user = repository.loadUser(id)
                _state.value = UiState.Success(user)
            } catch (e: Throwable) {
                _state.value = UiState.Error
            }
        }
    }
}
```

Fragment:

```kotlin
viewModel.state.observe(viewLifecycleOwner) { state ->
    render(state)
}
```

6. **Transformations**

`LiveData` має трансформації:

```kotlin
val userName: LiveData<String> =
    Transformations.map(user) { it.name }
```

`switchMap` дозволяє перемикатися на інше джерело:

```kotlin
val user: LiveData<User> =
    userId.switchMap { id ->
        repository.observeUser(id)
    }
```

Але для складних reactive pipelines `Flow` зазвичай гнучкіший.

7. **LiveData vs StateFlow**

`LiveData`:

- Android-specific;
- lifecycle-aware з коробки;
- добре підходить для legacy XML/View UI;
- має прості transformations.

`StateFlow`:

- coroutine-native;
- не привʼязаний до Android;
- краще підходить для domain/data layers;
- має багатий Flow API;
- природніший для Compose.

У новому Kotlin-коді часто вибирають `StateFlow` для state, а `LiveData`
залишають у legacy View-based екранах або там, де команда вже побудувала UI на
LiveData.

8. **LiveData і Room**

Room може повертати `LiveData`:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    fun observeUser(id: String): LiveData<UserEntity>
}
```

Коли таблиця зміниться, Room автоматично оновить `LiveData`. Це було дуже
популярним патерном до широкого переходу на `Flow`.

Сучасний варіант часто такий:

```kotlin
@Query("SELECT * FROM users WHERE id = :id")
fun observeUser(id: String): Flow<UserEntity>
```

9. **Interop з Flow**

`Flow` можна перетворити в `LiveData`:

```kotlin
val userLiveData: LiveData<User> =
    repository.observeUser(id).asLiveData()
```

`LiveData` можна перетворити в `Flow`:

```kotlin
val userFlow: Flow<User> =
    userLiveData.asFlow()
```

Це корисно під час поступової міграції з LiveData на Flow/StateFlow.

10. **One-time events**

Поганий сценарій для `LiveData` — одноразові events:

```kotlin
val navigation = MutableLiveData<NavigationEvent>()
```

Після rotation старе значення може бути доставлене повторно. Для one-time events
краще використовувати `SharedFlow`, `Channel` або спеціальний event wrapper,
залежно від архітектури.

11. **Типові помилки**

- Використовувати `MutableLiveData` напряму в UI.
- Спостерігати Fragment LiveData через `this`, а не `viewLifecycleOwner`.
- Використовувати LiveData для складних data pipelines.
- Використовувати LiveData для one-time events без захисту від повторної
  доставки.
- Оновлювати `value` з background thread.
- Тримати business logic у observers замість ViewModel/use case.

12. **Практичне правило**

- `LiveData` добре підходить для lifecycle-aware UI state у View-based Android.
- У `ViewModel` exposed тип має бути `LiveData`, mutable — private.
- У Fragment використовувати `viewLifecycleOwner`.
- Для нового coroutine-first коду частіше вибирати `StateFlow`/`Flow`.
- Для one-time events не використовувати просту `LiveData` без додаткової
  event-семантики.

**Коротко:** `LiveData` — це Android lifecycle-aware observable holder. Вона
зручна для класичного Android UI, автоматично враховує lifecycle і доставляє
останнє значення active observers, але для сучасної coroutine-first архітектури
часто поступається `Flow` і `StateFlow`.

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

`@Query`, `@Insert` і `@Delete` — це Room-анотації для DAO-методів. Вони
описують, яку SQL-операцію має виконати Room: довільний SQL-запит, вставку або
видалення entity. На основі цих анотацій Room генерує implementation DAO і
перевіряє SQL на compile time.

1. **DAO як контракт**

DAO — це interface або abstract class, де описані database operations:

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users")
    fun observeUsers(): Flow<List<UserEntity>>
}
```

Розробник пише контракт, а Room генерує реальний код доступу до SQLite.

2. **@Query**

`@Query` використовується для SQL-запитів:

```kotlin
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUser(id: String): UserEntity?
```

`:id` — це bind parameter. Room підставить значення параметра методу `id` у SQL
безпечним способом, без ручної конкатенації string.

Погано:

```kotlin
@Query("SELECT * FROM users WHERE id = '$id'")
```

Так не працює і концептуально небезпечно. Параметри треба передавати через
`:parameterName`.

3. **@Query для списків**

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

Room може повертати:

- `List<T>`;
- nullable entity;
- `Flow<T>`;
- `LiveData<T>`;
- `PagingSource`;
- `suspend` results.

Якщо повертається `Flow`, Room буде емiтити нові значення при зміні відповідних
таблиць.

4. **@Query для UPDATE/DELETE**

`@Query` можна використовувати не тільки для `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateUserName(id: String, name: String)
```

або:

```kotlin
@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String)
```

Це корисно, коли треба оновити або видалити не весь entity, а конкретні поля або
рядки за умовою.

5. **@Insert**

`@Insert` використовується для вставки entity:

```kotlin
@Insert
suspend fun insertUser(user: UserEntity)
```

Для списку:

```kotlin
@Insert
suspend fun insertUsers(users: List<UserEntity>)
```

Room сам згенерує SQL `INSERT` на основі `@Entity`.

6. **OnConflictStrategy**

При вставці може бути conflict, наприклад однаковий primary key:

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insertUser(user: UserEntity)
```

Поширені стратегії:

- `REPLACE` — замінити старий рядок;
- `IGNORE` — ігнорувати новий рядок при conflict;
- `ABORT` — перервати операцію з помилкою.

Вибір strategy — це бізнес-рішення. `REPLACE` не завжди безпечний, бо фактично
може видалити старий рядок і вставити новий.

7. **@Delete**

`@Delete` видаляє entity:

```kotlin
@Delete
suspend fun deleteUser(user: UserEntity)
```

Room видаляє рядок за primary key entity.

Для списку:

```kotlin
@Delete
suspend fun deleteUsers(users: List<UserEntity>)
```

Якщо треба видалити за `id`, без повного entity, краще `@Query`:

```kotlin
@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteUserById(id: String)
```

8. **Повернення результату**

`@Insert` може повертати row id:

```kotlin
@Insert
suspend fun insertUser(user: UserEntity): Long
```

Для списку:

```kotlin
@Insert
suspend fun insertUsers(users: List<UserEntity>): List<Long>
```

`@Delete` може повертати кількість видалених рядків:

```kotlin
@Delete
suspend fun deleteUser(user: UserEntity): Int
```

`@Query` для update/delete теж може повертати кількість affected rows:

```kotlin
@Query("DELETE FROM users WHERE isArchived = 1")
suspend fun deleteArchivedUsers(): Int
```

9. **@Update**

Хоча питання про `@Query`, `@Insert`, `@Delete`, поруч часто використовують
`@Update`:

```kotlin
@Update
suspend fun updateUser(user: UserEntity)
```

`@Update` оновлює рядок за primary key. Якщо треба оновити тільки одне поле,
часто краще `@Query`.

10. **Compile-time перевірка**

Room перевіряє SQL:

```kotlin
@Query("SELECT wrong_column FROM users")
suspend fun brokenQuery(): List<UserEntity>
```

Якщо column не існує або result не мапиться в return type, Room видасть помилку
під час компіляції. Це одна з головних переваг Room над raw SQLite.

11. **Transaction**

Якщо DAO-метод має виконуватись атомарно:

```kotlin
@Transaction
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUserWithOrders(id: String): UserWithOrders
```

Для кількох write operations можна зробити method у DAO:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertUsers(users)
}
```

12. **Практичне правило**

- `@Query` — довільний SQL: select, update, delete, custom operations.
- `@Insert` — вставка entity/entities.
- `@Delete` — видалення entity/entities за primary key.
- Для delete/update за умовою частіше використовувати `@Query`.
- Для conflict behavior явно задавати `OnConflictStrategy`.
- Для складних multi-step operations використовувати `@Transaction`.

**Коротко:** ці анотації описують Room, яку database operation треба згенерувати.
`@Query` дає контроль над SQL, `@Insert` генерує вставку entity, а `@Delete`
видаляє entity за primary key з type-safe compile-time перевіркою.

</details>
<details>
<summary>92. Що таке WorkManager і коли його використовувати?</summary>

#### Kotlin

`WorkManager` — це Jetpack API для відкладеної, гарантованої background-роботи,
яка має виконатися навіть якщо застосунок закрили або процес був убитий.
Він підходить для deferrable tasks: синхронізація, upload logs, retry network
operation, cleanup, periodic sync. Якщо задача має бути виконана "колись, коли
умови дозволять", WorkManager — правильний інструмент.

1. **Що вирішує WorkManager**

Android має багато обмежень на background execution. Не можна просто запустити
довгу роботу в `Thread` або `GlobalScope` і очікувати, що вона гарантовано
доживе до кінця.

WorkManager дає:

- guaranteed execution;
- constraints;
- retry;
- chaining;
- periodic work;
- збереження роботи в database;
- сумісність з обмеженнями Android background execution.

2. **Коли використовувати**

WorkManager доречний для задач, які:

- не мають виконатися миттєво;
- мають завершитися навіть після restart app/process;
- можуть чекати network/charging/battery constraints;
- можуть retry-итися;
- є background sync/upload/cleanup.

Приклади:

- upload analytics logs;
- sync локальних змін із backend;
- backup файлів;
- periodic refresh;
- cleanup cache;
- відправка queued messages;
- retry failed network operation.

3. **Коли не використовувати**

WorkManager не підходить для:

- негайних UI-bound задач;
- коротких suspend-операцій у ViewModel;
- real-time socket connection;
- точного alarm у конкретну секунду;
- foreground media playback;
- long-running interactive task без foreground service.

Якщо задача потрібна тільки поки екран живий — `viewModelScope`.
Якщо потрібен точний час — `AlarmManager`.
Якщо потрібна постійна foreground робота — Foreground Service.

4. **Worker**

Базовий worker:

```kotlin
class SyncWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params) {

    override suspend fun doWork(): Result {
        return try {
            syncRepository.sync()
            Result.success()
        } catch (e: IOException) {
            Result.retry()
        } catch (e: Throwable) {
            Result.failure()
        }
    }
}
```

Для Kotlin/coroutines краще використовувати `CoroutineWorker`, а не звичайний
`Worker`, якщо всередині є suspend-код.

5. **OneTimeWorkRequest**

Одноразова задача:

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .build()

WorkManager.getInstance(context)
    .enqueue(request)
```

Це поставить роботу в чергу. WorkManager сам вирішить, коли її виконати,
відповідно до system conditions.

6. **Constraints**

Можна задати умови:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.CONNECTED)
    .setRequiresCharging(true)
    .build()

val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .setConstraints(constraints)
    .build()
```

Worker стартує тільки коли умови виконані.

Поширені constraints:

- network connected/unmetered;
- charging;
- battery not low;
- storage not low;
- device idle.

7. **Input data**

Передача невеликих параметрів:

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(
        workDataOf("file_id" to fileId)
    )
    .build()
```

У worker:

```kotlin
val fileId = inputData.getString("file_id")
    ?: return Result.failure()
```

Не треба передавати великі обʼєкти. Краще передати ID, а дані взяти з database
або file storage.

8. **Result.success / retry / failure**

Worker повертає результат:

```kotlin
Result.success()
Result.retry()
Result.failure()
```

Семантика:

- `success` — задача виконалась;
- `retry` — transient failure, можна повторити;
- `failure` — задача не має сенсу повторювати.

Наприклад, timeout або network error — `retry`. Validation/business error —
`failure`.

9. **Backoff policy**

Retry можна налаштувати:

```kotlin
val request = OneTimeWorkRequestBuilder<SyncWorker>()
    .setBackoffCriteria(
        BackoffPolicy.EXPONENTIAL,
        30,
        TimeUnit.SECONDS
    )
    .build()
```

`EXPONENTIAL` збільшує delay між retry. Це правильний default для network sync.

10. **PeriodicWorkRequest**

Періодична робота:

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    6,
    TimeUnit.HOURS
).build()

WorkManager.getInstance(context)
    .enqueueUniquePeriodicWork(
        "user_sync",
        ExistingPeriodicWorkPolicy.KEEP,
        request
    )
```

Periodic work не гарантує точний час запуску. Android оптимізує виконання з
урахуванням battery і system constraints.

11. **Unique work**

Щоб не створити дублікати:

```kotlin
WorkManager.getInstance(context)
    .enqueueUniqueWork(
        "sync_user",
        ExistingWorkPolicy.REPLACE,
        request
    )
```

Policies:

- `KEEP` — залишити існуючу роботу;
- `REPLACE` — замінити;
- `APPEND` — додати в ланцюжок;
- `APPEND_OR_REPLACE` — додати або замінити залежно від стану.

12. **Chaining**

Можна будувати ланцюжки:

```kotlin
WorkManager.getInstance(context)
    .beginWith(downloadWork)
    .then(processWork)
    .then(uploadWork)
    .enqueue()
```

Це корисно, коли задачі мають виконуватись у конкретному порядку.

13. **Практичне правило**

- WorkManager — для гарантованої deferrable background роботи.
- Для короткої роботи на екрані — `viewModelScope`.
- Для точного часу — `AlarmManager`.
- Для постійної foreground роботи — Foreground Service.
- Передавати в worker ID, а не великі payloads.
- Для network failures повертати `Result.retry()`.
- Для повторюваної роботи використовувати unique periodic work.

**Коротко:** WorkManager — це стандартний Android-інструмент для надійної
background-роботи, яка має виконатися пізніше з урахуванням constraints,
retry-політик і системних обмежень Android.

</details>
<details>
<summary>93. У чому різниця між CoroutineScope, lifecycleScope та viewModelScope?</summary>

#### Kotlin

`CoroutineScope`, `lifecycleScope` і `viewModelScope` повʼязані з запуском
корутин, але мають різний рівень абстракції і різний lifecycle. `CoroutineScope`
— загальний Kotlin Coroutines API. `lifecycleScope` — Android scope, привʼязаний
до `LifecycleOwner`. `viewModelScope` — Android scope, привʼязаний до
`ViewModel`.

1. **CoroutineScope**

`CoroutineScope` — це контейнер для корутин, який визначає `CoroutineContext`:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)
```

Він містить:

- `Job` або `SupervisorJob`;
- dispatcher;
- optional context elements;
- lifecycle, якщо ти сам його контролюєш.

Запуск:

```kotlin
scope.launch {
    syncData()
}
```

Якщо створюєш custom scope, ти відповідаєш за його скасування:

```kotlin
scope.cancel()
```

2. **Коли використовувати custom CoroutineScope**

Custom `CoroutineScope` доречний для обʼєктів із власним lifecycle:

```kotlin
class SyncManager {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    fun start() {
        scope.launch {
            syncLoop()
        }
    }

    fun stop() {
        scope.cancel()
    }
}
```

Якщо немає чіткого місця, де scope буде скасований, custom scope створювати не
треба.

3. **lifecycleScope**

`lifecycleScope` доступний у `LifecycleOwner`, наприклад `Activity` або
`Fragment`:

```kotlin
lifecycleScope.launch {
    loadData()
}
```

Цей scope автоматично скасовується, коли lifecycle переходить у `DESTROYED`.

Для Activity:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        lifecycleScope.launch {
            // cancelled when Activity is destroyed
        }
    }
}
```

4. **lifecycleScope у Fragment**

У Fragment треба бути уважним: `lifecycleScope` привʼязаний до lifecycle самого
Fragment, а не його View.

Для UI collection краще:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

Це скасує collection, коли view Fragment-а буде destroyed, і не дасть звертатися
до старого binding.

5. **repeatOnLifecycle**

Для `Flow` у UI важливо не просто запускати collection, а робити це lifecycle-aware:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

`repeatOnLifecycle` запускає block при вході в потрібний state і скасовує при
виході з нього.

6. **viewModelScope**

`viewModelScope` доступний у `ViewModel`:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    fun loadUser(id: String) {
        viewModelScope.launch {
            val user = repository.loadUser(id)
            state.value = UiState.Success(user)
        }
    }
}
```

Він автоматично скасовується в `ViewModel.onCleared()`.

Це правильне місце для:

- завантаження screen data;
- business operations для екрана;
- collecting repository flows;
- update UI state;
- jobs, які мають пережити configuration change.

7. **viewModelScope переживає rotation**

При rotation `Activity` або `Fragment` можуть бути пересоздані, але `ViewModel`
залишається:

```text
Activity destroyed/recreated
ViewModel survives
viewModelScope continues
```

Тому data loading у `viewModelScope` не перезапускається даремно при кожній
rotation, якщо ViewModel той самий.

8. **Головна різниця lifecycle**

```text
CoroutineScope    -> живе стільки, скільки ти сам визначив
lifecycleScope    -> живе до LifecycleOwner DESTROYED
viewModelScope    -> живе до ViewModel.onCleared()
```

Тобто:

- UI-only робота в Activity/Fragment — `lifecycleScope`;
- screen/business state робота — `viewModelScope`;
- custom long-lived component — custom `CoroutineScope` з явним `cancel`.

9. **Типові помилки**

Погано:

```kotlin
GlobalScope.launch {
    repository.sync()
}
```

`GlobalScope` не має нормального owner-а. Така корутина може жити довше, ніж
екран або use case.

Погано:

```kotlin
lifecycleScope.launch {
    viewModel.state.collect { binding.title.text = it.title }
}
```

У Fragment це може пережити view. Краще `viewLifecycleOwner.lifecycleScope` і
`repeatOnLifecycle`.

10. **Коли що використовувати**

`viewModelScope`:

```kotlin
viewModelScope.launch {
    repository.loadUser()
}
```

Для роботи, повʼязаної зі state екрана.

`lifecycleScope`:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Для UI collection і lifecycle-aware UI side effects.

Custom `CoroutineScope`:

```kotlin
private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)
```

Для компонентів, які мають власний explicit lifecycle.

11. **Практичне правило**

- Не запускати корутину без owner-а.
- Для ViewModel-логіки використовувати `viewModelScope`.
- Для Activity/Fragment lifecycle UI-роботи — `lifecycleScope`.
- У Fragment для view-related роботи — `viewLifecycleOwner.lifecycleScope`.
- Для custom scope завжди мати explicit `cancel`.
- `GlobalScope` у production майже завжди поганий сигнал.

**Коротко:** `CoroutineScope` — загальний механізм керування корутинами,
`lifecycleScope` привʼязаний до Android lifecycle, а `viewModelScope` — до
ViewModel. Правильний вибір scope визначає, коли корутина буде скасована і чи не
створить вона leak або роботу після смерті UI.

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

`Hilt` — це dependency injection framework для Android, побудований поверх Dagger. Він спрощує DI setup, генерує Dagger components і дає готову інтеграцію з Android lifecycle: Application, Activity, Fragment, ViewModel, Service.

1. **Навіщо потрібен Hilt**

Hilt допомагає не створювати залежності вручну:

```kotlin
class ProfileViewModel(
    private val repository: ProfileRepository
) : ViewModel()
```

Замість ручного factory/setup Hilt сам надає потрібні objects через generated dependency graph.

2. **Application setup**

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` запускає генерацію root DI component для застосунку.

3. **AndroidEntryPoint**

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Ця анотація дозволяє Hilt inject-ити залежності в Android component і створювати правильний component scope.

4. **Inject constructor**

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

Якщо Hilt знає, як створити всі constructor parameters, він може створити цей клас автоматично.

5. **ViewModel**

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Hilt інтегрується з `ViewModelProvider` і створює ViewModel з потрібними залежностями.

6. **Modules**

Якщо клас не можна створити через constructor injection або треба bind interface, використовують modules.

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

`@Provides` для external classes:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient = OkHttpClient.Builder().build()
}
```

7. **Scopes**

Hilt має lifecycle-aware scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Scope визначає, скільки живе object. Наприклад, `@Singleton` — один instance на application component.

8. **Components**

Основні Hilt components:

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Dependency може залежати тільки від objects із того ж або довшого scope.

9. **Тестування**

Hilt дозволяє замінювати modules у тестах:

```kotlin
@TestInstallIn(
    components = [SingletonComponent::class],
    replaces = [NetworkModule::class]
)
@Module
object FakeNetworkModule
```

Це зручно для integration/instrumentation tests.

10. **Практичне правило**

- Constructor injection — default.
- `@Binds` — для interfaces.
- `@Provides` — для third-party classes/builders.
- Scopes використовувати тільки коли потрібен shared lifetime.
- Не inject-ити Android Context без розуміння scope.
- Hilt — інструмент wiring, не заміна архітектури.

**Коротко:** Hilt — Android DI framework поверх Dagger. Він генерує dependency graph, інтегрується з Android lifecycle, підтримує `@Inject`, `@HiltViewModel`, modules, scopes і test replacements. Його задача — безпечно зʼєднувати залежності, а не переносити бізнес-логіку в DI.

</details>
<details>
<summary>96. Що таке @HiltViewModel?</summary>

#### Kotlin

`@HiltViewModel` — це Hilt-анотація для `ViewModel`, яка дозволяє Hilt створювати
`ViewModel` і передавати в неї залежності через constructor injection. Вона
інтегрує `ViewModel` з Hilt dependency graph і `ViewModelProvider`, щоб
розробник не писав власну factory вручну.

1. **Базовий приклад**

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val loadUser: LoadUserUseCase
) : ViewModel() {

    private val _state = MutableStateFlow<UiState>(UiState.Loading)
    val state: StateFlow<UiState> = _state.asStateFlow()

    fun load(id: String) {
        viewModelScope.launch {
            _state.value = UiState.Success(loadUser(id))
        }
    }
}
```

Тут Hilt сам створить `UserViewModel` і передасть `LoadUserUseCase`.

2. **Навіщо потрібен @HiltViewModel**

Звичайна `ViewModel` створюється через `ViewModelProvider`. Якщо у ViewModel є
constructor parameters, Android не знає, як її створити:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel()
```

Без Hilt довелося б писати custom factory:

```kotlin
class UserViewModelFactory(
    private val repository: UserRepository
) : ViewModelProvider.Factory
```

`@HiltViewModel` прибирає цей boilerplate.

3. **Як отримати Hilt ViewModel у Fragment**

Fragment має бути `@AndroidEntryPoint`:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {

    private val viewModel: UserViewModel by viewModels()
}
```

`by viewModels()` використає Hilt-generated factory і створить ViewModel через
DI graph.

4. **Як отримати Hilt ViewModel в Activity**

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {

    private val viewModel: UserViewModel by viewModels()
}
```

Activity теж має бути entry point, якщо вона отримує Hilt ViewModel або інші
injected dependencies.

5. **Що має бути в constructor**

Constructor має бути позначений `@Inject`:

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val repository: UserRepository,
    private val savedStateHandle: SavedStateHandle
) : ViewModel()
```

Hilt може інжектити:

- use cases;
- repositories;
- dispatchers;
- analytics;
- `SavedStateHandle`;
- інші залежності, які є в graph.

6. **SavedStateHandle**

`SavedStateHandle` підтримується Hilt автоматично:

```kotlin
@HiltViewModel
class DetailsViewModel @Inject constructor(
    private val savedStateHandle: SavedStateHandle,
    private val repository: UserRepository
) : ViewModel() {

    private val userId: String =
        checkNotNull(savedStateHandle["user_id"])
}
```

Це корисно для navigation arguments і state, який має пережити process
recreation.

7. **ViewModelComponent**

Dependencies для Hilt ViewModel живуть у `ViewModelComponent`:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object ViewModelModule {

    @Provides
    fun provideValidator(): UserInputValidator {
        return UserInputValidator()
    }
}
```

Якщо dependency scoped як `@ViewModelScoped`, вона буде одна на конкретний
instance ViewModel.

```kotlin
@ViewModelScoped
class UserSessionCache @Inject constructor()
```

8. **@ViewModelScoped vs @Singleton**

`@Singleton`:

```text
один instance на весь app graph
```

`@ViewModelScoped`:

```text
один instance на конкретну ViewModel
```

Не треба робити все singleton. Якщо обʼєкт потрібен тільки для одного екрана,
його lifecycle має бути ближчим до ViewModel.

9. **Compose і hiltViewModel**

У Compose можна отримати Hilt ViewModel так:

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsState()
}
```

`hiltViewModel()` бере ViewModel із відповідного navigation/back stack owner і
використовує Hilt factory.

10. **Типові помилки**

- Забути `@HiltViewModel`.
- Забути `@Inject constructor`.
- Забути `@AndroidEntryPoint` на Activity/Fragment.
- Намагатися створити Hilt ViewModel вручну через constructor.
- Інжектити Activity/Fragment/View у ViewModel.
- Робити ViewModel singleton.
- Тримати в ViewModel reference на `Context`, якщо це не application context і
  немає чіткої потреби.

11. **Практичне правило**

- `@HiltViewModel` ставиться на ViewModel.
- Constructor ViewModel має бути `@Inject`.
- UI component має бути `@AndroidEntryPoint`.
- Залежності ViewModel мають приходити через constructor.
- Для navigation args використовувати `SavedStateHandle`.
- Для ViewModel-local dependencies використовувати `@ViewModelScoped`, якщо
  потрібен scope.

**Коротко:** `@HiltViewModel` дозволяє Hilt створювати `ViewModel` через generated
factory і constructor injection. Це прибирає ручні factories, робить ViewModel
тестованою і правильно інтегрує її залежності з Android/Hilt lifecycle.

</details>
<details>
<summary>97. Як працює @Inject?</summary>

#### Kotlin

`@Inject` — це анотація з JSR-330, яку використовують Dagger/Hilt, щоб позначити
місце, де dependency треба створити або куди її треба передати. У Android/Hilt
найчастіше `@Inject` ставлять на constructor класу, щоб Hilt знав, як створити
цей клас і які залежності йому потрібні.

1. **Constructor injection**

Найкращий і найпоширеніший варіант:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User {
        return repository.loadUser(id)
    }
}
```

Тут `@Inject constructor` означає: Hilt може створити `LoadUserUseCase`, якщо в
dependency graph є `UserRepository`.

2. **Як Hilt читає constructor**

Для такого класу:

```kotlin
class UserViewModel @Inject constructor(
    private val loadUser: LoadUserUseCase,
    private val analytics: Analytics
) : ViewModel()
```

Hilt будує graph:

```text
UserViewModel
 ├── LoadUserUseCase
 │    └── UserRepository
 └── Analytics
```

Якщо будь-якої залежності немає в graph, компіляція впаде з помилкою.

3. **Field injection**

`@Inject` можна ставити на поле:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {

    @Inject lateinit var analytics: Analytics
}
```

Hilt заповнить `analytics` після створення Activity.

Але для власних класів краще constructor injection. Field injection здебільшого
потрібен для Android framework classes, які система створює сама:

- `Activity`;
- `Fragment`;
- `Service`;
- `BroadcastReceiver`.

4. **Method injection**

Технічно можна інжектити через method:

```kotlin
class Tracker {

    lateinit var analytics: Analytics

    @Inject
    fun setup(analytics: Analytics) {
        this.analytics = analytics
    }
}
```

У production Android-коді це використовується рідко. Constructor injection
зазвичай простіший, явніший і тестованіший.

5. **@Inject не створює interface автоматично**

Якщо є interface:

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
}
```

Hilt не знає, яку реалізацію вибрати. Треба явно привʼязати implementation:

```kotlin
class RealUserRepository @Inject constructor(
    private val api: UserApi
) : UserRepository
```

Module:

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

`@Inject` на constructor implementation недостатньо, якщо dependency запитується
як interface.

6. **@Inject не працює для third-party класів**

Не можна додати `@Inject constructor` у клас, який ти не контролюєш:

```kotlin
Retrofit
OkHttpClient
RoomDatabase
```

Для таких залежностей використовують `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideOkHttpClient(): OkHttpClient {
        return OkHttpClient.Builder().build()
    }
}
```

7. **@Inject і scopes**

`@Inject` каже, як створити клас, але не завжди каже, скільки має жити instance.
Для lifecycle використовують scopes:

```kotlin
@Singleton
class UserSession @Inject constructor()
```

Або:

```kotlin
@ViewModelScoped
class UserDraftCache @Inject constructor()
```

Без scope Hilt може створювати новий instance там, де це потрібно graph-у.

8. **@Inject і qualifiers**

Якщо є кілька залежностей одного типу:

```kotlin
@Provides
@AuthRetrofit
fun provideAuthRetrofit(): Retrofit = TODO()

@Provides
@PublicRetrofit
fun providePublicRetrofit(): Retrofit = TODO()
```

Тоді в constructor треба вказати qualifier:

```kotlin
class AuthApi @Inject constructor(
    @AuthRetrofit private val retrofit: Retrofit
)
```

Без qualifier Hilt не знатиме, який `Retrofit` передати.

9. **Що відбувається на compile time**

Hilt/Dagger через annotation processing/code generation:

- знаходить `@Inject constructor`;
- знаходить `@Module`, `@Provides`, `@Binds`;
- будує dependency graph;
- генерує factories/components;
- перевіряє, чи всі залежності можна створити.

Якщо graph неповний, помилка буде під час компіляції, а не в runtime.

10. **Типові помилки**

- Ставити `@Inject` на implementation, але не зробити `@Binds` для interface.
- Використовувати field injection там, де можна constructor injection.
- Забути qualifier, коли є кілька залежностей одного типу.
- Очікувати, що Hilt сам створить third-party dependency без `@Provides`.
- Інжектити Android `Activity`/`View` у long-lived обʼєкти.
- Додавати scope без розуміння lifecycle.

11. **Практичне правило**

- Для своїх класів — `@Inject constructor`.
- Для Android framework classes — field injection, якщо потрібно.
- Для third-party/builder-based залежностей — `@Provides`.
- Для interface binding — `@Binds`.
- Для кількох однакових типів — qualifiers.
- Для lifecycle/reuse — scopes.

**Коротко:** `@Inject` показує Hilt/Dagger, як створити обʼєкт або куди передати
dependency. Найкраща практика — constructor injection, бо вона робить
залежності явними, compile-time перевіреними і простими для тестування.

</details>
<details>
<summary>98. Що таке області видимості (scopes) у Hilt?</summary>

#### Kotlin

Scopes у Hilt визначають, скільки живе dependency і в межах якого Hilt component
вона перевикористовується. Іншими словами, scope відповідає на питання: "один
instance на весь застосунок, на Activity, на ViewModel, на Fragment чи кожного
разу новий?".

1. **Навіщо потрібні scopes**

Без scope Hilt може створювати новий instance залежності там, де він потрібен:

```kotlin
class UserFormatter @Inject constructor()
```

Це нормально для stateless обʼєктів.

Але для важких або stateful dependencies потрібен контроль lifecycle:

```kotlin
@Singleton
class AppDatabaseProvider @Inject constructor()
```

Scope дозволяє не створювати database, repository cache або session object
щоразу заново.

2. **Hilt components**

Hilt має component hierarchy, привʼязану до Android lifecycle:

```text
SingletonComponent
 └── ActivityRetainedComponent
      └── ViewModelComponent
 └── ActivityComponent
      └── FragmentComponent
           └── ViewComponent
```

Кожен component має свій lifecycle і свої scopes.

3. **@Singleton**

`@Singleton` означає один instance у межах application graph:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object DatabaseModule {

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
}
```

Це правильний scope для:

- Room database;
- OkHttpClient;
- Retrofit;
- application-wide repositories, якщо вони stateless або мають app-wide cache;
- analytics client;
- app-level configuration.

Не треба робити singleton усе підряд.

4. **ActivityRetainedScoped**

`@ActivityRetainedScoped` живе довше за Activity instance і переживає
configuration changes:

```kotlin
@ActivityRetainedScoped
class UserSession @Inject constructor()
```

Цей scope привʼязаний не до конкретної Activity instance, а до retained lifecycle.
Він корисний для обʼєктів, які мають пережити rotation, але не мають бути
singleton на весь app.

5. **@ViewModelScoped**

`@ViewModelScoped` означає один instance на конкретну ViewModel:

```kotlin
@ViewModelScoped
class UserDraftCache @Inject constructor()
```

Module:

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {

    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache {
        return UserDraftCache()
    }
}
```

Це корисно для dependencies, які мають жити стільки ж, скільки ViewModel.

6. **ActivityScoped**

`@ActivityScoped` — один instance на Activity:

```kotlin
@ActivityScoped
class ScreenTracker @Inject constructor(
    @ActivityContext private val context: Context
)
```

Цей scope живе в межах конкретної Activity instance. Після recreation Activity
буде новий instance.

7. **FragmentScoped**

`@FragmentScoped` — один instance на Fragment:

```kotlin
@FragmentScoped
class FragmentAnalyticsTracker @Inject constructor()
```

Його використовують рідше, але він доречний, коли dependency має сенс тільки в
межах конкретного Fragment.

8. **Scope має відповідати component**

Scope і `@InstallIn` мають бути сумісні:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    @Singleton
    fun provideOkHttpClient(): OkHttpClient {
        return OkHttpClient()
    }
}
```

Погано встановлювати Activity-scoped binding у SingletonComponent. Hilt зазвичай
зловить такі помилки на compile time.

9. **Небезпека неправильного scope**

Погано:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Singleton житиме весь app lifecycle, а `ActivityContext` має жити тільки поки
живе Activity. Це прямий шлях до memory leak.

Краще:

```kotlin
@ActivityScoped
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Або взагалі не тримати `ActivityContext`, якщо він не потрібен.

10. **Unscoped dependencies**

Не всі залежності треба scope-ити:

```kotlin
class UserMapper @Inject constructor()
```

Якщо клас stateless, дешевий і не тримає ресурси, unscoped dependency часто
нормальна. Scope додає lifecycle semantics, і його треба застосовувати свідомо.

11. **Практичне правило**

- `@Singleton` — app-wide залежності.
- `@ActivityRetainedScoped` — переживає configuration changes.
- `@ViewModelScoped` — залежність конкретної ViewModel.
- `@ActivityScoped` — залежність конкретної Activity instance.
- `@FragmentScoped` — залежність конкретного Fragment.
- Не scope-ити stateless cheap objects без потреби.
- Не тримати короткоживучий `Context` у довгоживучому scope.

**Коротко:** scopes у Hilt керують lifecycle і reuse dependencies. Правильний scope
захищає від зайвого створення обʼєктів і memory leaks, неправильний — навпаки
може привʼязати Activity/Fragment до довгоживучого graph-а.

</details>
<details>
<summary>99. Як уникати витоків памʼяті в Android?</summary>

#### Kotlin

Витік памʼяті в Android виникає тоді, коли обʼєкт, який уже мав би бути
звільнений, усе ще reachable через якесь посилання. Найчастіше це `Activity`,
`Fragment`, `View`, `Context`, listener, callback або coroutine, які живуть
довше за свій lifecycle.

1. **Розуміти головну причину**

GC видаляє тільки недосяжні обʼєкти. Якщо singleton, static поле, coroutine або
listener тримає `Activity`, вона не буде очищена:

```kotlin
object Holder {
    var activity: Activity? = null
}
```

Це погано, бо `Holder` живе весь процес, а `Activity` може бути знищена після
rotation або navigation.

2. **Не тримати Activity Context у singleton**

Погано:

```kotlin
@Singleton
class Analytics @Inject constructor(
    private val context: Context
)
```

Якщо сюди випадково передали `ActivityContext`, буде leak.

Краще:

```kotlin
@Singleton
class Analytics @Inject constructor(
    @ApplicationContext private val context: Context
)
```

Для довгоживучих dependencies використовувати `applicationContext`.

3. **Очищати ViewBinding у Fragment**

У Fragment view lifecycle коротший за lifecycle самого Fragment:

```kotlin
private var _binding: FragmentUserBinding? = null
private val binding get() = _binding!!

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    _binding = FragmentUserBinding.bind(view)
}

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

Якщо не очистити binding, Fragment може тримати стару view hierarchy після
`onDestroyView()`.

4. **Використовувати viewLifecycleOwner**

Погано:

```kotlin
lifecycleScope.launch {
    viewModel.state.collect { state ->
        binding.title.text = state.title
    }
}
```

У Fragment це може пережити view.

Краще:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            binding.title.text = state.title
        }
    }
}
```

Так collection скасовується разом із view lifecycle.

5. **Скасовувати listeners і callbacks**

Якщо реєструєш listener — маєш місце, де його зняти:

```kotlin
override fun onStart() {
    super.onStart()
    locationManager.addListener(listener)
}

override fun onStop() {
    locationManager.removeListener(listener)
    super.onStop()
}
```

Те саме стосується:

- broadcast receivers;
- text watchers;
- sensor listeners;
- location callbacks;
- custom SDK callbacks;
- observers поза lifecycle-aware API.

6. **Не використовувати GlobalScope для UI**

Погано:

```kotlin
GlobalScope.launch {
    val data = repository.loadData()
    activity.render(data)
}
```

Корутина може пережити `Activity` і тримати її в памʼяті.

Краще:

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
    state.value = UiState.Success(data)
}
```

або для UI lifecycle:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        // collect UI state
    }
}
```

7. **Не тримати View у ViewModel**

Погано:

```kotlin
class UserViewModel : ViewModel() {
    lateinit var titleView: TextView
}
```

`ViewModel` переживає configuration changes, а `View` належить старій
`Activity`/`Fragment`. Це прямий leak.

ViewModel має тримати state:

```kotlin
data class UserState(
    val title: String
)
```

А UI має render-ити цей state.

8. **Обережно з anonymous inner classes**

Anonymous listener може неявно тримати outer class:

```kotlin
button.setOnClickListener {
    presenter.doSomething(this)
}
```

Сам по собі click listener на view нормальний, якщо view живе стільки ж, скільки
екран. Але якщо listener передається в довгоживучий SDK/cache/singleton, він
може утримати Fragment або Activity.

9. **Handler і delayed tasks**

Погано:

```kotlin
Handler(Looper.getMainLooper()).postDelayed({
    binding.title.text = "Done"
}, 10_000)
```

Якщо view знищена раніше, delayed lambda може тримати binding.

Краще використовувати lifecycle-aware coroutine:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    delay(10_000)
    binding.title.text = "Done"
}
```

Вона буде скасована разом із lifecycle.

10. **Використовувати LeakCanary**

LeakCanary — практичний інструмент для знаходження leaks у debug builds.

Він показує reference chain:

```text
Singleton -> Listener -> Fragment -> ViewBinding -> View
```

Це допомагає не гадати, а бачити, хто саме тримає непотрібний обʼєкт.

11. **Hilt scopes і Context**

Scope dependency має відповідати lifecycle:

```kotlin
@Singleton
class ImageLoader @Inject constructor(
    @ApplicationContext private val context: Context
)
```

Погано:

```kotlin
@Singleton
class ScreenTracker @Inject constructor(
    @ActivityContext private val context: Context
)
```

`ActivityContext` не можна тримати в singleton.

12. **Практичне правило**

- Не тримати `Activity`, `Fragment`, `View`, binding у довгоживучих обʼєктах.
- У Fragment очищати binding у `onDestroyView`.
- Для UI collection використовувати `viewLifecycleOwner`.
- Скасовувати listeners/callbacks.
- Не використовувати `GlobalScope` для UI-bound роботи.
- У ViewModel тримати state, а не references на views.
- Для singleton dependencies використовувати `ApplicationContext`.
- Регулярно перевіряти leaks через LeakCanary.

**Коротко:** щоб уникати витоків памʼяті в Android, треба привʼязувати references,
coroutines, listeners і DI scopes до правильного lifecycle. Найчастіше leaks
зʼявляються не через GC, а через довгоживуче посилання на короткоживучий UI
обʼєкт.

</details>
<details>
<summary>100. Як безпечно зберігати API-ключі в Android-додатку?</summary>

#### Kotlin

Коротка відповідь: повністю безпечно зберігати секретні API-ключі всередині Android-додатку неможливо. APK/AAB потрапляє на пристрій користувача, його можна декомпілювати, проаналізувати через runtime tools або перехопити мережеві запити. Тому правильна стратегія — не класти справжні server-side secrets у клієнтський додаток.

1. **Розділяти public keys і secrets**

Не всі ключі однаково критичні:

- public API key для ідентифікації app/client може бути в додатку;
- server secret, private token, signing secret, admin key не мають бути в APK;
- ключі з правом списання коштів, доступом до чужих даних або bypass security мають жити тільки на backend.

Погано:

```kotlin
object ApiKeys {
    const val STRIPE_SECRET_KEY = "sk_live_..."
}
```

Такий ключ буде витягнутий із binary.

2. **Виносити секрети на backend**

Правильна схема:

```text
Android app -> your backend -> third-party API
```

Android-додаток звертається до вашого backend-а, а backend уже використовує секретний ключ.

Наприклад:

```kotlin
interface PaymentsApi {
    @POST("payments/create-intent")
    suspend fun createPaymentIntent(
        @Body request: PaymentRequest
    ): PaymentIntentResponse
}
```

Клієнт не знає secret key. Він отримує тільки обмежений результат, наприклад `clientSecret` для конкретної операції.

3. **Обмежувати ключі на стороні provider-а**

Якщо ключ все ж має бути в додатку, треба максимально обмежити його:

- package name;
- SHA-256 certificate fingerprint;
- allowed APIs;
- allowed domains/backend endpoints;
- rate limits;
- quotas;
- environment: dev/staging/prod;
- мінімальні permissions.

Наприклад для Google APIs ключ має бути привʼязаний до Android app package і signing certificate.

4. **Не зберігати secrets у Git**

Погано:

```kotlin
const val API_KEY = "real_prod_key"
```

Краще підставляти build-time config:

```kotlin
android {
    defaultConfig {
        buildConfigField(
            "String",
            "PUBLIC_API_KEY",
            "\"${project.findProperty("publicApiKey")}\""
        )
    }
}
```

Але важливо: `BuildConfig` не робить ключ секретним. Він лише прибирає значення з repository. У compiled app цей ключ усе одно можна знайти.

5. **Використовувати local.properties або CI secrets для non-secret config**

Для локальної розробки:

```properties
publicApiKey=dev_public_key
```

Для CI/CD — secrets у GitHub Actions, GitLab CI, Bitrise, CircleCI або іншій системі.

Це захищає repository, але не захищає ключ після потрапляння в APK.

6. **Для user tokens використовувати EncryptedSharedPreferences або DataStore + encryption**

Якщо треба зберігати access token користувача:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val preferences = EncryptedSharedPreferences.create(
    context,
    "secure_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)

preferences.edit()
    .putString("access_token", token)
    .apply()
```

Це підходить для user/session tokens, але не для global server secrets.

7. **Краще зберігати короткоживучі токени**

Добра практика:

- access token має короткий TTL;
- refresh token зберігається обережно;
- backend може відкликати токен;
- scopes мінімальні;
- logout очищає локальні токени;
- token rotation підтримується на backend.

Наприклад:

```kotlin
class AuthRepository(
    private val tokenStorage: TokenStorage,
    private val api: AuthApi
) {
    suspend fun logout() {
        tokenStorage.clear()
        api.revokeSession()
    }
}
```

8. **Не покладатися на obfuscation як на security**

R8/ProGuard ускладнює reverse engineering, але не робить ключ секретним.

```proguard
-keep class com.example.BuildConfig { *; }
```

Навіть якщо рядки обфусковані, runtime traffic, memory dump або static analysis можуть показати значення.

Obfuscation — це hardening, не security boundary.

9. **Захищати мережевий шар**

Мінімум:

- HTTPS тільки;
- certificate pinning для high-risk застосунків;
- не логувати tokens/API keys;
- не передавати secrets у query params;
- очищати sensitive headers у crash reports/logs.

Приклад interceptor-а без логування Authorization:

```kotlin
class AuthInterceptor(
    private val tokenProvider: TokenProvider
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenProvider.accessToken()
        val request = chain.request()
            .newBuilder()
            .header("Authorization", "Bearer $token")
            .build()

        return chain.proceed(request)
    }
}
```

10. **Використовувати backend-issued temporary credentials**

Для доступу до S3, Firebase custom flows або third-party services краще видавати тимчасові облікові дані:

```text
Android -> backend -> temporary scoped token -> service
```

Такий токен має:

- короткий TTL;
- обмежений scope;
- привʼязку до user/session;
- можливість revoke.

11. **Практичне правило**

- Server secrets не зберігати в Android-додатку.
- Public keys можна класти в app, але з restrictions.
- `BuildConfig`, `local.properties` і CI secrets захищають repo, не APK.
- User tokens зберігати encrypted і робити їх короткоживучими.
- Секретні операції виконувати через backend.
- Не логувати tokens, headers і sensitive payloads.
- Використовувати rate limits, scopes, quotas і revoke.

**Коротко:** Android-клієнт не є trusted environment. Усе, що потрапило в APK або runtime на пристрої користувача, треба вважати потенційно доступним атакувальнику. Справжня безпека API-ключів будується на backend, обмеженнях ключів, короткоживучих токенах і мінімальних permissions.

</details>
<details>
<summary>101. Як реалізувати offline-first архітектуру?</summary>

#### Kotlin

`Offline-first` — це підхід, де застосунок в першу чергу працює з локальним джерелом даних, а мережа використовується для синхронізації. UI не має напряму залежати від API: він читає state з database/cache, а background layer оновлює ці дані.

1. **Базова схема**

```text
UI -> ViewModel -> Repository -> Local DB
                         └── Remote API
```

Головний принцип: **local database — source of truth**. Якщо немає інтернету, UI все одно показує останні доступні дані.

2. **Repository**

Repository координує local і remote:

```kotlin
class ArticleRepository(
    private val dao: ArticleDao,
    private val api: ArticleApi
) {
    fun observeArticles(): Flow<List<Article>> =
        dao.observeArticles().map { entities -> entities.map { it.toDomain() } }

    suspend fun refresh() {
        val remote = api.getArticles()
        dao.replaceAll(remote.map { it.toEntity() })
    }
}
```

UI підписаний на `Flow` з DB, а `refresh()` тільки оновлює локальні дані.

3. **ViewModel**

```kotlin
class ArticlesViewModel(
    private val repository: ArticleRepository
) : ViewModel() {
    val articles = repository.observeArticles()
        .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5_000), emptyList())

    fun refresh() {
        viewModelScope.launch {
            repository.refresh()
        }
    }
}
```

ViewModel не має знати, чи дані прийшли з cache, DB або network.

4. **Синхронізація**

Для background sync зазвичай використовують `WorkManager`:

```kotlin
class SyncWorker(
    context: Context,
    params: WorkerParameters,
    private val repository: ArticleRepository
) : CoroutineWorker(context, params) {
    override suspend fun doWork(): Result = try {
        repository.refresh()
        Result.success()
    } catch (e: IOException) {
        Result.retry()
    }
}
```

`WorkManager` підходить для задач із constraints: network available, retry, periodic sync.

5. **Write operations**

Для offline-first важливо підтримати локальні зміни:

```kotlin
suspend fun updateTitle(id: ArticleId, title: String) {
    dao.updateTitle(id.value, title, syncStatus = SyncStatus.PENDING)
}
```

Потім sync worker відправляє pending changes на сервер. Якщо API недоступне, зміни лишаються локально.

6. **Conflict resolution**

Конфлікти треба вирішувати явно. Варіанти:

- server wins;
- client wins;
- last write wins;
- merge по полях;
- manual resolution.

Для інтервʼю достатньо пояснити, що стратегія залежить від домену. Для банківських операцій і notes app правила будуть різні.

7. **NetworkBoundResource pattern**

Класичний flow:

```text
emit local data
try fetch remote
save remote into DB
emit updated local data
```

Тобто UI не чекає мережу, а одразу бачить local state.

8. **UI state**

Offline-first UI має показувати не тільки data, а й sync status:

```kotlin
data class ArticlesState(
    val items: List<ArticleUi> = emptyList(),
    val isRefreshing: Boolean = false,
    val isOffline: Boolean = false,
    val pendingChanges: Int = 0
)
```

Користувач має розуміти, що дані можуть бути cached або ще не синхронізовані.

9. **Практичні ризики**

- дублікати після sync;
- неправильні IDs для локально створених обʼєктів;
- конфлікти між client/server;
- втрата pending changes;
- некоректні retry;
- застарілі дані без індикатора.

10. **Практичне правило**

Offline-first реалізація має відповідати на три питання:

- звідки UI читає дані;
- як local state синхронізується з server;
- що робити з pending changes і conflicts.

**Коротко:** offline-first означає, що UI читає дані з локальної DB як source of truth, а remote API використовується для sync. Repository координує local/remote, `WorkManager` виконує background sync, а write operations зберігаються локально як pending changes до успішної синхронізації.

</details>
<details>
<summary>102. Що таке архітектура MVI і коли її використовувати?</summary>

#### Kotlin

`MVI` — це архітектурний підхід, де UI описується через immutable state, а всі дії користувача або системи проходять як events/intents. Основна ідея: один потік даних і один source of truth для екрана.

1. **Базова схема**

```text
UI -> Intent/Event -> ViewModel/Reducer -> State -> UI
                              └── Effect
```

- `State` — поточний стан екрана.
- `Intent/Event` — дія користувача або системи.
- `Reducer` — перетворює old state + event у new state.
- `Effect` — одноразова дія: navigation, toast, snackbar.

2. **State**

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

State має бути immutable. UI не змінює його напряму, а тільки рендерить.

3. **Events / Intents**

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
    data object BackClicked : ProfileEvent
}
```

Event описує, що сталося, а не як це виконати.

4. **Effects**

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowSnackbar(val message: String) : ProfileEffect
}
```

Effect потрібен для одноразових подій. Їх не варто зберігати як звичайний `State`, бо після recomposition або rotation можна випадково повторити navigation/toast.

5. **ViewModel приклад**

```kotlin
class ProfileViewModel(
    private val loadUser: LoadUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    private val _effect = MutableSharedFlow<ProfileEffect>()
    val effect: SharedFlow<ProfileEffect> = _effect

    fun onEvent(event: ProfileEvent) {
        when (event) {
            ProfileEvent.LoadClicked -> load()
            ProfileEvent.RetryClicked -> load()
            ProfileEvent.BackClicked -> emitEffect(ProfileEffect.NavigateBack)
        }
    }

    private fun load() {
        viewModelScope.launch {
            _state.update { it.copy(isLoading = true, error = null) }
            runCatching { loadUser() }
                .onSuccess { user -> _state.update { it.copy(isLoading = false, user = user) } }
                .onFailure { _state.update { it.copy(isLoading = false, error = "Load failed") } }
        }
    }

    private fun emitEffect(effect: ProfileEffect) {
        viewModelScope.launch { _effect.emit(effect) }
    }
}
```

6. **UI у Compose**

```kotlin
@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsStateWithLifecycle()

    LaunchedEffect(Unit) {
        viewModel.effect.collect { effect ->
            when (effect) {
                ProfileEffect.NavigateBack -> { /* navigate */ }
                is ProfileEffect.ShowSnackbar -> { /* show snackbar */ }
            }
        }
    }

    ProfileScreen(
        state = state,
        onEvent = viewModel::onEvent
    )
}
```

UI відправляє events і рендерить state. Він не виконує бізнес-логіку.

7. **Коли MVI корисний**

MVI добре працює, коли:

- екран має багато станів;
- є loading/error/retry/empty/content;
- багато user actions;
- потрібен прогнозований state flow;
- команда хоче однаковий підхід для всіх екранів;
- важлива тестованість ViewModel.

8. **Коли MVI може бути зайвим**

Для простих екранів MVI може додати boilerplate. Якщо екран має одну кнопку і пару полів, достатньо звичайного `StateFlow` у ViewModel без формального reducer/event/effect layer.

9. **Переваги**

- один source of truth;
- predictable state updates;
- простіше тестувати;
- легше відтворити bug через sequence events;
- добре підходить для Compose;
- менше прихованого mutable state.

10. **Ризики**

- багато boilerplate;
- погано спроєктовані events стають “God events”;
- effects можна неправильно змішати зі state;
- reducer може розростися без декомпозиції.

11. **Практичне правило**

MVI варто використовувати для складних екранів, де важлива прозора модель стану. Але не треба перетворювати кожен простий UI-компонент на повну MVI-машину.

**Коротко:** MVI — це unidirectional data flow: UI відправляє events/intents, ViewModel оновлює immutable state, UI рендерить новий state, а одноразові дії йдуть через effects. Його варто використовувати для складних stateful екранів, але для простих сценаріїв він може бути надмірним.

</details>
<details>
<summary>103. Як проєктувати масштабовані Android-додатки?</summary>

#### Kotlin

Масштабований Android-додаток — це не “багато модулів”, а система, де зміни в одній частині мінімально ламають інші. Головні принципи: зрозумілі boundaries, стабільні contracts, однонаправлений data flow, тестованість і контроль залежностей.

1. **Архітектурні шари**

Типова структура:

```text
UI -> ViewModel -> UseCase -> Repository -> DataSource
```

- `UI` показує state і відправляє events.
- `ViewModel` керує screen state.
- `UseCase` містить бізнес-операцію.
- `Repository` приховує джерела даних.
- `DataSource` працює з API, DB, cache.

UI не має знати про Retrofit, Room або конкретну storage-логіку.

2. **Feature-first modularization**

Для великого проєкту краще групувати код навколо features:

```text
:app
:core:network
:core:database
:core:ui
:core:common
:feature:profile
:feature:checkout
:feature:search
```

Feature-модуль має бути максимально самодостатнім. Core-модулі містять спільну інфраструктуру, але не повинні знати про конкретні features.

3. **Контроль залежностей**

Правильний напрямок залежностей:

```text
feature -> domain contracts -> data implementation
```

Не можна, щоб `core` залежав від `feature`, або щоб одна feature напряму тягнула іншу без contract/API модуля. Інакше код швидко перетвориться на циклічний граф.

4. **Stable contracts**

Між шарами краще мати інтерфейси або чіткі моделі:

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Це спрощує тестування, заміну implementation і роботу кількох команд паралельно.

5. **State management**

Екран має мати один зрозумілий state:

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

ViewModel змінює state, UI тільки рендерить. Для Compose це особливо важливо, бо хаотичний state швидко створює зайві recompositions і складні баги.

6. **Error handling**

Помилки треба моделювати явно:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Не варто пробросувати сирий `Throwable` у UI. UI має отримувати зрозумілий стан або подію.

7. **Dependency Injection**

DI потрібен, щоб не створювати залежності вручну всередині класів:

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

У production це зазвичай Hilt/Koin/manual DI. Важливіше не сам framework, а те, щоб залежності були явними.

8. **Тестованість**

Код має тестуватись без Android framework там, де це можливо:

- use cases — unit tests;
- repositories — fake data sources;
- ViewModel — state tests;
- UI — screenshot або Compose UI tests;
- integration — API/DB boundaries.

Якщо бізнес-логіку можна протестувати тільки через emulator, архітектура занадто привʼязана до Android layer.

9. **Performance і build time**

Масштабованість включає не тільки runtime, а й швидкість розробки:

- не тягнути зайві залежності в feature;
- не робити giant-модулі;
- використовувати Gradle convention plugins;
- слідкувати за incremental build;
- уникати глобальних singleton-ів без потреби.

10. **Практичне правило**

Проєкт має масштабуватись по трьох осях:

- **код** — зрозумілі boundaries і модулі;
- **команда** — features можна розробляти незалежно;
- **зміни** — нова вимога не вимагає переписувати половину app.

**Коротко:** масштабований Android-додаток будується навколо чітких шарів, feature-first модулів, стабільних contracts, однонаправленого state flow, DI, тестованості і контрольованих залежностей. Мета — зробити зміни локальними, а не глобальними.

</details>
<details>
<summary>104. Що таке Jetpack Compose і чим він відрізняється від View System?</summary>

#### Kotlin

`Jetpack Compose` — це declarative UI toolkit для Android. Замість XML layout і ручного оновлення `View`, UI описується Kotlin-функціями, які рендерять інтерфейс на основі state.

1. **Declarative vs imperative**

У View System ми зазвичай створюємо layout і потім змінюємо View вручну:

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

У Compose UI є функцією від state:

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

Коли state змінюється, Compose сам оновлює потрібні частини UI.

2. **Composable functions**

UI будується через `@Composable` функції:

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Composable не має бути місцем для бізнес-логіки або I/O. Він має рендерити UI і відправляти callbacks.

3. **State-driven UI**

Compose добре працює з immutable state:

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

ViewModel змінює state, UI його відображає.

4. **Recomposition**

`Recomposition` — це повторний виклик composable, коли state змінився. Compose намагається оновити тільки потрібні частини UI.

Важливо:

- composable може викликатись багато разів;
- не можна робити side effects напряму в body;
- expensive objects треба кешувати через `remember`;
- state має бути стабільним і передбачуваним.

5. **remember**

```kotlin
var query by remember { mutableStateOf("") }
```

`remember` зберігає значення між recompositions, але не переживає process death. Для screen-level state краще ViewModel, для config changes — `rememberSaveable` або ViewModel.

6. **Compose vs XML/View**

```text
View System -> XML + View classes + manual updates
Compose     -> Kotlin functions + state-driven rendering
```

Compose зменшує boilerplate, спрощує previews, state handling і побудову reusable UI-компонентів.

7. **Interoperability**

Compose і View System можна змішувати:

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

Або вставити Android View у Compose:

```kotlin
AndroidView(factory = { context ->
    TextView(context)
})
```

Це дозволяє мігрувати поступово.

8. **Переваги Compose**

- менше XML і adapter boilerplate;
- UI як функція від state;
- сильна інтеграція з Kotlin;
- прості previews;
- легше робити reusable components;
- зручні animation APIs;
- добре підходить для unidirectional data flow.

9. **Ризики Compose**

- неправильний state може викликати зайві recompositions;
- side effects треба запускати через Compose effect APIs;
- lazy lists потребують stable keys;
- performance треба профілювати;
- команда має розуміти lifecycle composition.

10. **Практичне правило**

Compose не просто замінює XML. Він змінює модель мислення: замість “знайди View і зміни її” — “опиши UI для поточного state”. Це робить архітектуру чистішою, якщо state flow спроєктований правильно.

**Коротко:** Jetpack Compose — declarative Kotlin UI toolkit. На відміну від View System, він будує UI як функцію від state, використовує composable functions і recomposition замість ручного оновлення View. Compose дає менше boilerplate і кращий state-driven підхід, але вимагає дисципліни зі state, side effects і performance.

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

`State hoisting` — це підняття state з composable у його parent. Дочірній composable не зберігає власний state, а отримує `value` і callback для зміни. Це робить компонент reusable, testable і контрольованим ззовні.

1. **Ідея**

Замість цього:

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }

    TextField(
        value = query,
        onValueChange = { query = it }
    )
}
```

Краще так:

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(
        value = query,
        onValueChange = onQueryChange
    )
}
```

Тепер `SearchField` не вирішує, де живе state. Він тільки показує value і повідомляє про intent змінити його.

2. **Parent керує state**

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

Parent може зберігати state в `remember`, `rememberSaveable`, `ViewModel`, `StateFlow` або іншому state holder.

3. **State у ViewModel**

Для screen-level state краще використовувати ViewModel:

```kotlin
data class SearchState(
    val query: String = "",
    val results: List<ItemUi> = emptyList()
)

class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state: StateFlow<SearchState> = _state

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

4. **Stateless і stateful composables**

Добра практика — розділяти:

```kotlin
@Composable
fun SearchRoute(viewModel: SearchViewModel) {
    // stateful: бере state з ViewModel
}

@Composable
fun SearchScreen(
    state: SearchState,
    onQueryChange: (String) -> Unit
) {
    // stateless: тільки рендерить state
}
```

`Route` знає про ViewModel. `Screen` не знає про ViewModel і легше тестується/preview-иться.

5. **Single source of truth**

State hoisting створює один source of truth:

```text
state живе зверху -> child рендерить -> child відправляє event -> parent оновлює state
```

Це зменшує баги, коли різні компоненти тримають різні версії одного значення.

6. **Коли не треба hoist-ити все**

Не кожен локальний state треба піднімати. Наприклад, внутрішній animation progress, ripple або тимчасовий expanded state може залишитись всередині компонента, якщо parent не має ним керувати.

Правило:

```text
якщо state потрібен parent-у або іншим composables -> hoist
якщо state суто внутрішній і не впливає на бізнес-логіку -> можна лишити локально
```

7. **Переваги**

- компонент стає reusable;
- легше писати previews;
- легше тестувати UI;
- простіше синхронізувати кілька child components;
- менше прихованого mutable state;
- кращий unidirectional data flow.

8. **Типова структура параметрів**

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

Спочатку data/state, потім callbacks, потім `modifier`.

9. **Практичне правило**

State hoisting — це не просто “винести var нагору”. Це спосіб зробити UI керованим через state і events. У Compose це базовий патерн для чистої архітектури екранів.

**Коротко:** `state hoisting` — це підняття state у parent і передача в child через `value` + `onValueChange`. Child стає stateless, reusable і testable, а parent або ViewModel стає single source of truth.

</details>
<details>
<summary>107. У чому різниця між remember і rememberSaveable?</summary>

#### Kotlin

`remember` і `rememberSaveable` зберігають state між recompositions, але мають різну стійкість. `remember` переживає тільки recomposition, а `rememberSaveable` також може пережити configuration change і process recreation, якщо значення можна зберегти в `Bundle`.

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

`remember` зберігає `count` між recompositions. Але якщо Activity пересоздасться після rotation, значення буде втрачено.

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

`rememberSaveable` використовує `SavedInstanceState` і може відновити значення після configuration change.

3. **Головна різниця**

```text
remember         -> survives recomposition
rememberSaveable -> survives recomposition + configuration change/process recreation
```

Але `rememberSaveable` працює тільки з типами, які можна зберегти: primitives, String, Parcelable, Serializable або custom Saver.

4. **Приклад із TextField**

```kotlin
var query by rememberSaveable { mutableStateOf("") }

TextField(
    value = query,
    onValueChange = { query = it }
)
```

Для input state це часто кращий варіант, бо користувач не втратить текст після rotation.

5. **Custom Saver**

Якщо тип не підтримується напряму, можна описати `Saver`:

```kotlin
data class FilterState(val query: String, val selected: Int)

val FilterSaver = Saver<FilterState, Bundle>(
    save = { state ->
        bundleOf(
            "query" to state.query,
            "selected" to state.selected
        )
    },
    restore = { bundle ->
        FilterState(
            query = bundle.getString("query", ""),
            selected = bundle.getInt("selected")
        )
    }
)
```

```kotlin
var filter by rememberSaveable(stateSaver = FilterSaver) {
    mutableStateOf(FilterState("", 0))
}
```

6. **Коли використовувати remember**

`remember` підходить для локального transient UI state:

- expanded/collapsed у межах composition;
- cached formatter/object;
- animation helper;
- локальний scroll-related helper;
- state, який не шкода втратити при recreation.

7. **Коли використовувати rememberSaveable**

`rememberSaveable` підходить для UI state, який користувач очікує зберегти:

- текст у полі вводу;
- selected tab/filter;
- simple form state;
- toggle state;
- scroll position у простих випадках.

8. **Коли потрібен ViewModel**

Для screen/business state краще ViewModel:

```kotlin
class ProfileViewModel : ViewModel() {
    val state: StateFlow<ProfileState> = TODO()
}
```

`rememberSaveable` не замінює ViewModel, repository або domain state. Він тільки зберігає простий UI state.

9. **Обмеження rememberSaveable**

- не зберігає великі обʼєкти;
- не підходить для repository/data state;
- залежить від saveable типів;
- `Bundle` має обмеження розміру;
- custom Saver треба писати уважно.

10. **Практичне правило**

- Recomposition only — `remember`.
- Configuration change для простого UI state — `rememberSaveable`.
- Screen/business state — ViewModel.
- Persistent data — repository/database/DataStore.

**Коротко:** `remember` зберігає значення тільки між recompositions. `rememberSaveable` додатково зберігає його через `SavedInstanceState`, тому переживає rotation/configuration change для saveable типів. Для складного screen state краще використовувати ViewModel.

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

`Modifier` у Jetpack Compose — це immutable ланцюжок інструкцій, який описує, як composable має поводитися або виглядати: розмір, padding, background, click handling, alignment, semantics, drawing, layout behavior тощо. Це один із ключових механізмів конфігурації UI в Compose.

1. **Базова ідея**

Composable зазвичай приймає `modifier` параметром:

```kotlin
@Composable
fun ProfileAvatar(
    imageUrl: String,
    modifier: Modifier = Modifier
) {
    AsyncImage(
        model = imageUrl,
        contentDescription = null,
        modifier = modifier
            .size(64.dp)
            .clip(CircleShape)
    )
}
```

Caller може додати зовнішню поведінку:

```kotlin
ProfileAvatar(
    imageUrl = user.avatarUrl,
    modifier = Modifier.padding(16.dp)
)
```

2. **Modifier — це chain**

`Modifier` будується як послідовність елементів:

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
    .size(100.dp)
```

Порядок має значення. Це не набір незалежних властивостей, а chain, який Compose застосовує послідовно.

3. **Порядок modifier-ів важливий**

Приклад:

```kotlin
Box(
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .size(100.dp)
)
```

Тут padding застосовується перед background, тому червоний фон буде всередині padded area.

Інший порядок:

```kotlin
Box(
    modifier = Modifier
        .background(Color.Red)
        .padding(16.dp)
        .size(100.dp)
)
```

Тут background буде до padding, тому візуальний результат інший.

4. **Modifier для layout**

Modifier може керувати розміром і розміщенням:

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .fillMaxWidth()
        .padding(horizontal = 16.dp)
)
```

Типові layout modifiers:

- `size`;
- `width`;
- `height`;
- `fillMaxWidth`;
- `fillMaxHeight`;
- `fillMaxSize`;
- `padding`;
- `offset`;
- `weight`;
- `wrapContentSize`.

5. **Modifier для interaction**

Наприклад click:

```kotlin
Text(
    text = "Open profile",
    modifier = Modifier.clickable {
        onProfileClick()
    }
)
```

Або combined click:

```kotlin
Modifier.combinedClickable(
    onClick = onClick,
    onLongClick = onLongClick
)
```

Важливо: interaction logic має бути callback-ом, а не бізнес-логікою всередині composable.

6. **Modifier для drawing**

```kotlin
Box(
    modifier = Modifier
        .size(80.dp)
        .clip(RoundedCornerShape(16.dp))
        .background(Color.Blue)
)
```

Типові drawing modifiers:

- `background`;
- `border`;
- `clip`;
- `alpha`;
- `shadow`;
- `drawBehind`;
- `drawWithContent`.

7. **Modifier для accessibility**

Semantics теж задаються через modifier:

```kotlin
Icon(
    imageVector = Icons.Default.Delete,
    contentDescription = null,
    modifier = Modifier.semantics {
        contentDescription = "Delete item"
    }
)
```

Або для тестів:

```kotlin
Modifier.testTag("profile_avatar")
```

Це важливо для accessibility і UI tests.

8. **Modifier як параметр має бути першим optional параметром**

Стандартний Compose-style:

```kotlin
@Composable
fun PrimaryButton(
    text: String,
    onClick: () -> Unit,
    modifier: Modifier = Modifier
) {
    Button(
        onClick = onClick,
        modifier = modifier
    ) {
        Text(text)
    }
}
```

Зазвичай `modifier` передають у root composable всередині компонента.

9. **Не створювати жорсткі layout constraints всередині reusable component**

Погано:

```kotlin
@Composable
fun UserCard(user: User) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp)
    ) {
        Text(user.name)
    }
}
```

Такий компонент завжди займає всю ширину.

Краще:

```kotlin
@Composable
fun UserCard(
    user: User,
    modifier: Modifier = Modifier
) {
    Row(
        modifier = modifier.padding(16.dp)
    ) {
        Text(user.name)
    }
}
```

Caller вирішує:

```kotlin
UserCard(
    user = user,
    modifier = Modifier.fillMaxWidth()
)
```

10. **Modifier.then**

Можна комбінувати modifiers:

```kotlin
val baseModifier = Modifier
    .fillMaxWidth()
    .padding(16.dp)

Text(
    text = "Title",
    modifier = baseModifier.then(
        Modifier.background(Color.LightGray)
    )
)
```

Але зловживати цим не треба. Часто простий chain читається краще.

11. **Conditional modifiers**

Частий патерн:

```kotlin
fun Modifier.conditional(
    condition: Boolean,
    block: Modifier.() -> Modifier
): Modifier {
    return if (condition) block() else this
}
```

Використання:

```kotlin
Text(
    text = "Premium",
    modifier = Modifier
        .padding(8.dp)
        .conditional(isSelected) {
            background(Color.Yellow)
        }
)
```

Це допомагає не дублювати UI.

12. **Performance**

`Modifier` immutable і lightweight, але не варто без потреби створювати складні modifiers з важкими обчисленнями на кожну recomposition.

Якщо modifier залежить від expensive calculation:

```kotlin
val shape = remember(cornerRadius) {
    RoundedCornerShape(cornerRadius)
}

Box(
    modifier = Modifier.clip(shape)
)
```

Для звичайних простих chains спеціальна оптимізація зазвичай не потрібна.

13. **Практичне правило**

- Кожен reusable composable має приймати `modifier: Modifier = Modifier`.
- Modifier передавати в root element компонента.
- Порядок modifier-ів має значення.
- Не хардкодити зовнішній layout у reusable component.
- Для business logic використовувати callbacks, а не logic всередині modifier.
- Accessibility/test semantics теж задавати через modifier.

**Коротко:** `Modifier` — це спосіб декларативно описати зовнішню поведінку, layout, drawing, interaction і semantics composable. Він immutable, chainable, порядок його елементів важливий, а правильна передача `modifier` робить Compose-компоненти гнучкими й перевикористовуваними.

</details>
<details>
<summary>111. У чому різниця між padding і offset?</summary>

#### Kotlin

У Compose `padding` і `offset` обидва можуть візуально змістити елемент, але працюють на різних рівнях layout pipeline. `padding` змінює constraints і місце, яке елемент займає в layout. `offset` зміщує вже розміщений елемент візуально, зазвичай не змінюючи місце, яке він займає для parent-а.

1. **padding змінює layout**

```kotlin
Box(
    modifier = Modifier
        .background(Color.Gray)
        .padding(16.dp)
) {
    Text("Hello")
}
```

`padding` додає внутрішній або зовнішній простір залежно від порядку modifier-ів. Він впливає на measurement і placement child-а.

Якщо спростити:

```text
parent дає constraints -> padding зменшує доступний простір -> child вимірюється
```

2. **offset зміщує позицію**

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` зміщує елемент від його початкової позиції. Parent зазвичай продовжує вважати, що елемент займає старе місце.

Тобто:

```text
child виміряли -> child розмістили -> offset змістив його візуально
```

3. **Головна різниця**

```text
padding -> впливає на розмір/constraints/місце в layout
offset  -> зміщує елемент після layout
```

Приклад:

```kotlin
Row {
    Text(
        text = "A",
        modifier = Modifier.offset(x = 20.dp)
    )
    Text("B")
}
```

`A` зміститься вправо, але `B` не обовʼязково звільнить для нього додаткове місце. Може бути overlap.

З `padding`:

```kotlin
Row {
    Text(
        text = "A",
        modifier = Modifier.padding(start = 20.dp)
    )
    Text("B")
}
```

`A` займе більше місця в layout, і `B` буде розміщений з урахуванням цього простору.

4. **Порядок із background**

Порядок modifier-ів критичний.

```kotlin
Box(
    modifier = Modifier
        .background(Color.Red)
        .padding(16.dp)
)
```

Фон буде включати область до padding.

```kotlin
Box(
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
)
```

Фон буде після padding, тобто тільки на внутрішньому content area.

З `offset`:

```kotlin
Box(
    modifier = Modifier
        .offset(16.dp, 0.dp)
        .background(Color.Red)
)
```

Елемент буде зміщений, але його layout slot у parent-а лишиться концептуально там, де був.

5. **padding для spacing**

`padding` треба використовувати, коли потрібно:

- додати відступ між компонентами;
- збільшити touch area;
- створити внутрішній простір;
- вплинути на розміщення сусідніх елементів;
- зробити layout передбачуваним.

Наприклад:

```kotlin
Column(
    modifier = Modifier.padding(16.dp)
) {
    Text("Title")
    Text("Subtitle")
}
```

6. **offset для візуального зсуву**

`offset` доречний, коли треба:

- зробити невеликий visual adjustment;
- реалізувати badge;
- змістити елемент в animation;
- зробити overlap;
- не впливати на layout сусідів.

Приклад badge:

```kotlin
Box {
    Icon(
        imageVector = Icons.Default.Notifications,
        contentDescription = null
    )

    Box(
        modifier = Modifier
            .align(Alignment.TopEnd)
            .offset(x = 4.dp, y = (-4).dp)
            .size(8.dp)
            .background(Color.Red, CircleShape)
    )
}
```

Badge зміщується візуально, але не змушує parent перебудовувати spacing.

7. **offset може спричинити overlap**

```kotlin
Row {
    Box(
        modifier = Modifier
            .size(50.dp)
            .offset(x = 30.dp)
            .background(Color.Red)
    )

    Box(
        modifier = Modifier
            .size(50.dp)
            .background(Color.Blue)
    )
}
```

Red box може наїхати на blue box, бо `offset` не резервує додаткове місце.

Якщо overlap не потрібен, треба використовувати `padding`, `Spacer` або layout arrangement.

8. **offset і layout direction**

Є overload:

```kotlin
Modifier.offset(x = 16.dp, y = 0.dp)
```

Він layout-direction aware для horizontal direction.

Також є absolute offset:

```kotlin
Modifier.absoluteOffset(x = 16.dp, y = 0.dp)
```

`absoluteOffset` не враховує RTL/LTR. Його треба використовувати обережно.

9. **offset з lambda**

Для animation або state-based offset краще використовувати lambda version:

```kotlin
Modifier.offset {
    IntOffset(
        x = animatedOffset.roundToPx(),
        y = 0
    )
}
```

Це може бути ефективніше, бо дозволяє уникнути зайвих recomposition у деяких сценаріях і працювати на layout phase.

10. **padding не замінює arrangement**

Для рівномірних відступів між елементами краще:

```kotlin
Column(
    verticalArrangement = Arrangement.spacedBy(8.dp)
) {
    Text("One")
    Text("Two")
    Text("Three")
}
```

Замість того щоб кожному child задавати manual padding.

11. **Практичне правило**

- Потрібен реальний простір у layout — використовувати `padding`, `Spacer`, `Arrangement`.
- Потрібно тільки візуально зсунути елемент — використовувати `offset`.
- Якщо сусідні елементи мають враховувати відступ — це не `offset`.
- Якщо потрібен overlap або badge — `offset` доречний.
- Завжди перевіряти порядок modifier-ів.

**Коротко:** `padding` бере участь у вимірюванні й розміщенні, тому змінює layout. `offset` зміщує елемент після layout і зазвичай не впливає на позицію сусідів. Для spacing використовують `padding`, для visual shift або overlap — `offset`.

</details>
<details>
<summary>112. У чому різниця між fillMaxSize і matchParentSize?</summary>

#### Kotlin

`fillMaxSize()` і `matchParentSize()` обидва можуть зробити елемент розміром із parent, але працюють у різних контекстах. `fillMaxSize()` — загальний modifier, який просить зайняти максимально доступний простір із constraints. `matchParentSize()` — scoped modifier для `BoxScope`, який змушує child відповідати вже визначеному розміру `Box`, не впливаючи на вимірювання самого `Box`.

1. **fillMaxSize**

`fillMaxSize()` каже елементу зайняти весь доступний простір від parent constraints:

```kotlin
Box(
    modifier = Modifier.fillMaxSize()
) {
    Text("Content")
}
```

Якщо parent дає constraints на весь екран, `Box` займе весь екран.

Можна також задати fraction:

```kotlin
Box(
    modifier = Modifier.fillMaxSize(0.5f)
)
```

Це означає зайняти 50% доступної ширини й висоти.

2. **matchParentSize**

`matchParentSize()` доступний тільки всередині `BoxScope`:

```kotlin
Box {
    Image(
        painter = painterResource(R.drawable.background),
        contentDescription = null,
        modifier = Modifier.matchParentSize()
    )

    Text("Content")
}
```

Він робить child таким самим за розміром, як `Box`, але не змушує сам `Box` ставати більшим.

3. **Головна різниця**

```text
fillMaxSize()       -> впливає на розмір самого composable
matchParentSize()   -> підганяє child під уже виміряний Box
```

`fillMaxSize()` бере участь у measurement.  
`matchParentSize()` у `Box` не впливає на те, як `Box` визначає свій розмір.

4. **Приклад, де різниця важлива**

```kotlin
Box {
    Text("Small content")

    Box(
        modifier = Modifier
            .matchParentSize()
            .background(Color.Red.copy(alpha = 0.2f))
    )
}
```

Розмір outer `Box` буде визначений `Text("Small content")`. Overlay через `matchParentSize()` просто покриє цей розмір.

Якщо використати `fillMaxSize()`:

```kotlin
Box {
    Text("Small content")

    Box(
        modifier = Modifier
            .fillMaxSize()
            .background(Color.Red.copy(alpha = 0.2f))
    )
}
```

Child може попросити весь доступний простір і цим вплинути на size behavior parent-а залежно від constraints.

5. **Типовий use case для matchParentSize**

Overlay:

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

Overlay має покрити контент, але не має визначати розмір container-а.

6. **Типовий use case для fillMaxSize**

Screen root:

```kotlin
@Composable
fun HomeScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        Text("Home")
    }
}
```

Тут root layout справді має займати весь доступний екран.

7. **matchParentSize працює тільки в BoxScope**

Це не універсальний modifier:

```kotlin
Box {
    Box(
        modifier = Modifier.matchParentSize()
    )
}
```

Але поза `BoxScope` він недоступний:

```kotlin
Column {
    // Modifier.matchParentSize() тут не працює
}
```

Для `Column`, `Row` та інших layout-ів використовують `fillMaxSize`, `fillMaxWidth`, `fillMaxHeight`, `weight` або custom layout logic.

8. **Box measurement behavior**

`Box` спочатку вимірює дітей, які не мають `matchParentSize`, і на їх основі визначає свій size. Потім діти з `matchParentSize()` вимірюються під розмір `Box`.

Тому `matchParentSize()` добре підходить для:

- background layer;
- overlay layer;
- loading layer;
- clickable scrim;
- gradient поверх content.

9. **Не плутати з Android View match_parent**

У View System:

```xml
android:layout_width="match_parent"
android:layout_height="match_parent"
```

означає зайняти розмір parent-а.

У Compose немає прямого глобального аналога `match_parent`. Найближче за змістом часто `fillMaxSize()`, але це залежить від constraints. А `matchParentSize()` — спеціальний інструмент саме для `BoxScope`.

10. **Практичне правило**

- Для root screen/container — `fillMaxSize()`.
- Для ширини — `fillMaxWidth()`.
- Для висоти — `fillMaxHeight()`.
- Для overlay/background усередині `Box` — `matchParentSize()`.
- Якщо child не має впливати на розмір `Box` — `matchParentSize()`.
- Якщо composable має сам попросити максимум простору — `fillMaxSize()`.

**Коротко:** `fillMaxSize()` змушує composable зайняти максимально доступний простір згідно constraints і може впливати на layout. `matchParentSize()` працює тільки всередині `BoxScope` і підганяє child під уже визначений розмір `Box`, не беручи участі у визначенні цього розміру.

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

`rememberUpdatedState()` використовується, коли long-running effect або callback має бачити найновіше значення параметра/lambda, але сам effect не повинен перезапускатися при кожній зміні цього значення. Це захищає від stale capture у `LaunchedEffect`, `DisposableEffect` та інших side effects.

1. **Проблема stale capture**

Composable може recomposed з новими параметрами, але coroutine або listener, запущений раніше, може тримати стару lambda.

Погано:

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

`LaunchedEffect(Unit)` не перезапускається при зміні `onTimeout`. Якщо lambda змінилася, coroutine може викликати стару версію.

2. **Рішення через rememberUpdatedState**

```kotlin
@Composable
fun SplashScreen(
    onTimeout: () -> Unit
) {
    val currentOnTimeout by rememberUpdatedState(onTimeout)

    LaunchedEffect(Unit) {
        delay(2_000)
        currentOnTimeout()
    }
}
```

Тепер effect не перезапускається, але всередині coroutine буде актуальна lambda.

3. **Ключова ідея**

```text
LaunchedEffect(Unit)        -> effect живе стабільно
rememberUpdatedState(value) -> value всередині effect завжди актуальний
```

Це корисно, коли:

- effect дорогий для restart;
- restart змінить поведінку неправильно;
- треба оновити callback без перезапуску coroutine/listener;
- listener має бути зареєстрований один раз, але викликати актуальну lambda.

4. **Чому не додати lambda в key**

Можна написати:

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

Але тоді при кожній зміні `onTimeout` effect скасується і запуститься заново. Для splash timer це означає, що таймер може починатися спочатку.

Якщо це не бажано, використовують:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

5. **Приклад з lifecycle observer**

```kotlin
@Composable
fun LifecycleEventHandler(
    onStart: () -> Unit,
    onStop: () -> Unit
) {
    val currentOnStart by rememberUpdatedState(onStart)
    val currentOnStop by rememberUpdatedState(onStop)
    val lifecycleOwner = LocalLifecycleOwner.current

    DisposableEffect(lifecycleOwner) {
        val observer = LifecycleEventObserver { _, event ->
            when (event) {
                Lifecycle.Event.ON_START -> currentOnStart()
                Lifecycle.Event.ON_STOP -> currentOnStop()
                else -> Unit
            }
        }

        lifecycleOwner.lifecycle.addObserver(observer)

        onDispose {
            lifecycleOwner.lifecycle.removeObserver(observer)
        }
    }
}
```

Observer реєструється один раз для `lifecycleOwner`, але callbacks завжди актуальні.

6. **Приклад з timeout**

```kotlin
@Composable
fun AutoCloseDialog(
    onClose: () -> Unit
) {
    val currentOnClose by rememberUpdatedState(onClose)

    LaunchedEffect(Unit) {
        delay(5_000)
        currentOnClose()
    }

    AlertDialog(
        onDismissRequest = onClose,
        confirmButton = {}
    )
}
```

Якщо parent recomposed і передав новий `onClose`, timeout викличе саме новий callback.

7. **Коли rememberUpdatedState не потрібен**

Якщо зміна value має перезапустити effect, треба використовувати key:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Тут `userId` має бути key, бо при зміні userId треба завантажити іншого користувача.

`rememberUpdatedState` тут був би помилкою, якщо він приховає необхідний restart.

8. **Правильне питання перед використанням**

Перед `rememberUpdatedState` треба чітко відповісти:

```text
Чи має effect перезапуститися при зміні цього value?
```

Якщо так — value має бути key в `LaunchedEffect`/`DisposableEffect`.

Якщо ні — value можна обгорнути в `rememberUpdatedState`.

9. **Поганий приклад**

```kotlin
@Composable
fun UserScreen(userId: String, viewModel: UserViewModel) {
    val currentUserId by rememberUpdatedState(userId)

    LaunchedEffect(Unit) {
        viewModel.loadUser(currentUserId)
    }
}
```

Це помилка, бо при зміні `userId` треба перезапустити load.

Краще:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

10. **Внутрішньо це State**

`rememberUpdatedState` повертає `State<T>`:

```kotlin
val currentValue by rememberUpdatedState(value)
```

При recomposition Compose оновлює `currentValue`, але сам effect, який читає це значення пізніше, не перезапускається автоматично.

11. **Практичне правило**

Використовувати `rememberUpdatedState`, коли:

- effect має стартувати один раз або за іншими keys;
- callback/value може змінюватися;
- всередині effect треба бачити latest value;
- restart effect небажаний.

Не використовувати, коли:

- зміна value має restart-ити effect;
- value є input для завантаження даних;
- key semantic важливий для correctness;
- це можна простіше вирішити правильними keys.

12. **Коротко**

`rememberUpdatedState()` потрібен для ситуацій, де side effect живе довше за одну recomposition і має використовувати найновішу lambda або value без перезапуску самого effect. Це типовий інструмент проти stale callbacks у `LaunchedEffect` і `DisposableEffect`.

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

Keys у `LaunchedEffect` визначають lifecycle coroutine-effect-а: коли його треба запустити, коли скасувати і коли перезапустити. Якщо key змінюється, Compose скасовує попередню coroutine і запускає нову. Саме тому keys критично важливі для correctness side effects у Compose.

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

Тут `userId` — key.

Поведінка:

- composable увійшов у composition — effect стартує;
- `userId` не змінився — effect не restart-иться при recomposition;
- `userId` змінився — стара coroutine cancel, нова coroutine start;
- composable вийшов із composition — coroutine cancel.

2. **Що буде без правильного key**

Погано:

```kotlin
@Composable
fun UserScreen(
    userId: String,
    viewModel: UserViewModel
) {
    LaunchedEffect(Unit) {
        viewModel.loadUser(userId)
    }
}
```

Якщо `userId` зміниться, effect не перезапуститься, бо key `Unit` не змінився. У результаті екран може показувати старого користувача.

Краще:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

3. **Що означає LaunchedEffect(Unit)**

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadInitialData()
}
```

Це означає: запустити effect один раз для цього входу в composition.

Це нормально для:

- initial load без параметрів;
- collect one-time effects;
- стартової animation;
- одноразового timeout.

Але це помилка, якщо effect залежить від параметра, який може змінитися.

4. **Кілька keys**

Можна передати кілька keys:

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(
        userId = userId,
        filter = filter
    )
}
```

Effect restart-иться, якщо зміниться хоча б один key.

Це корисно, коли side effect залежить від кількох inputs.

5. **Keys мають відповідати dependencies effect-а**

Практичне правило:

```text
Усе, що визначає результат side effect-а, має бути key.
```

Наприклад:

```kotlin
LaunchedEffect(query, sortOrder) {
    viewModel.search(
        query = query,
        sortOrder = sortOrder
    )
}
```

Якщо `sortOrder` не додати в keys, зміна сортування не запустить новий search.

6. **Неправильний unstable key**

Погано:

```kotlin
LaunchedEffect(UserFilter(query, sortOrder)) {
    viewModel.search(query, sortOrder)
}
```

Якщо `UserFilter` створюється заново при кожній recomposition і не має нормального equality, effect може restart-итися зайвий раз.

Краще:

```kotlin
LaunchedEffect(query, sortOrder) {
    viewModel.search(query, sortOrder)
}
```

Або використовувати stable immutable model із коректним `equals`.

7. **Не використовувати random/time як key**

Погано:

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

Або:

```kotlin
LaunchedEffect(UUID.randomUUID()) {
    viewModel.load()
}
```

Такий key змінюватиметься постійно, effect буде restart-итися безконтрольно.

8. **Keys і cancellation**

Коли key змінюється, стара coroutine скасовується:

```kotlin
LaunchedEffect(query) {
    delay(300)
    viewModel.search(query)
}
```

Це можна використати для debounce-like behavior: якщо `query` зміниться до завершення `delay`, попередній effect cancel-иться.

Але для складного search debounce часто краще робити у ViewModel через Flow:

```kotlin
query
    .debounce(300)
    .flatMapLatest { repository.search(it) }
```

9. **Keys і rememberUpdatedState**

Якщо value потрібен всередині effect, але не має restart-ити effect, використовують `rememberUpdatedState`.

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

Тут `onTimeout` не key, бо зміна callback-а не має restart-ити timer.

10. **Collect effects із ViewModel**

Для one-time effects часто використовують:

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        when (effect) {
            LoginEffect.NavigateHome -> navController.navigate("home")
        }
    }
}
```

`Unit` тут доречний, бо collection має стартувати один раз для screen composition.

Якщо змінюється `viewModel`, тоді краще:

```kotlin
LaunchedEffect(viewModel) {
    viewModel.effects.collect { effect ->
        // handle effect
    }
}
```

11. **Типові помилки**

- `LaunchedEffect(Unit)` для effect-а, який залежить від `userId`;
- занадто багато keys, через що effect restart-иться зайво;
- unstable object як key;
- random/time-based key;
- не врахувати cancellation при зміні key;
- використовувати `rememberUpdatedState`, коли насправді потрібен restart.

12. **Практичне правило**

- Якщо зміна value має перезапустити effect — value має бути key.
- Якщо зміна value не має перезапускати effect, але потрібна latest версія — `rememberUpdatedState`.
- Для initial one-time effect — `LaunchedEffect(Unit)`.
- Для parameter-driven effect — `LaunchedEffect(parameter)`.
- Keys мають бути stable і мати коректний equality.

**Коротко:** keys у `LaunchedEffect` — це contract lifecycle-а side effect-а. Вони визначають, коли coroutine запускається, скасовується і перезапускається. Неправильні keys дають або stale data, або зайві restart-и, або непередбачувану поведінку UI.

</details>
<details>
<summary>117. Що таке snapshotFlow?</summary>

#### Kotlin

`snapshotFlow` — це Compose API, який перетворює читання Compose snapshot state у Kotlin `Flow`. Його використовують, коли треба спостерігати за змінами Compose state як за stream-ом і обробляти їх у coroutine: наприклад, логувати scroll position, запускати analytics або реагувати на зміни `LazyListState`.

1. **Базова ідея**

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .collect { index ->
            println("First visible item: $index")
        }
}
```

Всередині lambda `snapshotFlow` читається Compose snapshot state. Коли прочитане значення змінюється, flow emit-ить нове значення.

2. **Типовий приклад зі scroll**

```kotlin
@Composable
fun FeedScreen(
    posts: List<Post>,
    analytics: Analytics
) {
    val listState = rememberLazyListState()

    LaunchedEffect(listState) {
        snapshotFlow { listState.firstVisibleItemIndex }
            .distinctUntilChanged()
            .collect { index ->
                analytics.trackFirstVisiblePost(index)
            }
    }

    LazyColumn(state = listState) {
        items(posts) { post ->
            PostItem(post)
        }
    }
}
```

Тут UI state списку перетворюється у Flow, щоб відправляти analytics не напряму з composition.

3. **Чому не просто читати state у composable**

Погано:

```kotlin
@Composable
fun FeedScreen(analytics: Analytics) {
    val listState = rememberLazyListState()

    analytics.trackFirstVisiblePost(listState.firstVisibleItemIndex)
}
```

Це side effect у body composable. Він може виконуватися на кожну recomposition і давати неконтрольовані виклики.

Краще:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            analytics.trackFirstVisiblePost(index)
        }
}
```

4. **snapshotFlow emit-ить тільки коли результат змінюється**

`snapshotFlow` спостерігає за snapshot reads усередині lambda:

```kotlin
snapshotFlow {
    listState.firstVisibleItemIndex > 0
}
```

Якщо результат був `false` і залишився `false`, нового emission не буде. Але для складніших сценаріїв все одно часто додають `distinctUntilChanged()` для явності.

5. **Комбінація з Flow operators**

Оскільки результат — це звичайний `Flow`, можна використовувати operators:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .map { index -> index > 0 }
        .distinctUntilChanged()
        .filter { showButton -> showButton }
        .collect {
            analytics.trackScrolledFromTop()
        }
}
```

Це корисно для debounce, filter, map, distinctUntilChanged, sample тощо.

6. **Приклад для scroll-to-top button**

Якщо треба тільки показати кнопку, часто краще `derivedStateOf`:

```kotlin
val showScrollToTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

Але якщо треба side effect на зміну scroll state — `snapshotFlow`:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex > 0 }
        .distinctUntilChanged()
        .collect { hasScrolled ->
            analytics.setHasScrolled(hasScrolled)
        }
}
```

7. **snapshotFlow треба запускати в coroutine**

Зазвичай його використовують всередині `LaunchedEffect`:

```kotlin
LaunchedEffect(key1 = listState) {
    snapshotFlow { listState.layoutInfo.visibleItemsInfo }
        .collect { visibleItems ->
            // handle visible items
        }
}
```

Не треба collect-ити його напряму в composable body.

8. **Що можна читати всередині snapshotFlow**

Всередині lambda треба читати Compose snapshot state:

```kotlin
snapshotFlow {
    searchQuery
}
```

Або state обʼєкти Compose:

```kotlin
snapshotFlow {
    pagerState.currentPage
}
```

Якщо всередині не читається snapshot state, `snapshotFlow` не має сенсу.

9. **Не виконувати side effects усередині lambda**

Погано:

```kotlin
snapshotFlow {
    analytics.track(listState.firstVisibleItemIndex)
    listState.firstVisibleItemIndex
}
```

Lambda має тільки читати state і повертати value.

Краще:

```kotlin
snapshotFlow { listState.firstVisibleItemIndex }
    .collect { index ->
        analytics.track(index)
    }
```

10. **snapshotFlow vs collectAsState**

`collectAsState`:

- Flow -> Compose State;
- використовується для rendering UI.

`snapshotFlow`:

- Compose State -> Flow;
- використовується для side effects або Flow pipeline.

```text
Flow -> UI      = collectAsState
UI state -> Flow = snapshotFlow
```

11. **Типові помилки**

- використовувати `snapshotFlow` для простого rendering, де достатньо direct state read;
- робити side effects у lambda;
- забути `LaunchedEffect`;
- читати нестабільний або надто великий object і отримати багато emissions;
- не додати `distinctUntilChanged`, коли потрібні тільки зміни логічного стану;
- використовувати його замість ViewModel Flow без потреби.

12. **Практичне правило**

Використовувати `snapshotFlow`, коли:

- є Compose state;
- треба обробляти його зміни як Flow;
- дія є side effect, а не rendering;
- потрібні Flow operators.

Не використовувати, коли:

- можна просто прочитати state в UI;
- треба показати/сховати UI на основі state — часто краще `derivedStateOf`;
- source state уже є `Flow` у ViewModel.

**Коротко:** `snapshotFlow` конвертує Compose snapshot state у `Flow`. Це правильний інструмент для side effects на основі Compose state, наприклад analytics по scroll, але не заміна звичайному state rendering у composable.

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

Лаги в Compose під час скролу найчастіше виникають через зайві recompositions, важку роботу в item composables, нестабільні keys, allocation під час rendering або надто складний UI в кожному елементі списку.

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

`key` допомагає Compose правильно зберігати state елементів при insert/delete/reorder і зменшує некоректні recompositions.

2. **Вказувати contentType**

Якщо в списку різні типи item-ів:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` допомагає Compose ефективніше переиспользувати composition для однотипних елементів.

3. **Не робити важку роботу в item**

Погано:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Краще:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

А ще краще — підготувати formatted string у mapper/ViewModel, якщо це частина UI model.

4. **Використовувати remember для expensive objects**

```kotlin
val shape = remember { RoundedCornerShape(12.dp) }
```

Не треба створювати важкі обʼєкти на кожній recomposition. Але `remember` не має маскувати бізнес-логіку в UI.

5. **Передавати стабільні моделі**

Краще передавати immutable UI models:

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Якщо модель mutable або щоразу створюється заново без потреби, Compose важче зрозуміти, що реально змінилось.

6. **Декомпозувати item**

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Row {
        Avatar(user.avatarUrl)
        UserInfo(user.name)
    }
}
```

Менші composables дають Compose більше шансів recomposition-ити тільки потрібну частину.

7. **Не читати надто широкий state**

Погано, коли кожен item читає весь screen state. Краще передавати тільки дані конкретного item і callback:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

8. **derivedStateOf для scroll-derived state**

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Це корисно, коли state змінюється дуже часто, але UI має реагувати тільки на derived condition.

9. **Зображення**

Для списків треба використовувати image loader з cache і правильним size:

```kotlin
AsyncImage(
    model = user.avatarUrl,
    contentDescription = null
)
```

Не можна декодувати bitmap вручну в composable або main thread.

10. **Paging і placeholders**

Для великих списків краще Paging 3. Він не вантажить все одразу і краще контролює loading:

```kotlin
val items = pager.collectAsLazyPagingItems()
```

11. **Профілювання**

Оптимізацію треба підтверджувати:

- Layout Inspector recomposition counts;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles;
- system traces.

12. **Практичне правило**

Для smooth scroll важливо: stable `key`, правильний `contentType`, легкі item composables, мінімум allocations, cached images, нечитання зайвого state і відсутність I/O в composition.

**Коротко:** лаги в Compose списках прибирають через stable keys, `contentType`, легкі item composables, `remember` для expensive objects, immutable UI models, `derivedStateOf` для scroll state, cached image loading, Paging для великих списків і профілювання recompositions/rendering.

</details>
<details>
<summary>121. Чим відрізняються анотації Stable та Immutable?</summary>

#### Kotlin

`@Stable` і `@Immutable` у Compose — це анотації, які допомагають Compose compiler/runtime зрозуміти, наскільки безпечно пропускати recomposition для обʼєктів. `@Immutable` означає, що обʼєкт після створення не змінюється. `@Stable` означає, що обʼєкт може мати mutable state, але Compose може коректно відстежувати його зміни.

1. **Навіщо вони потрібні**

Compose намагається пропускати recomposition, якщо параметри composable не змінилися.

```kotlin
@Composable
fun UserCard(user: UserUiModel) {
    Text(user.name)
}
```

Щоб безпечно skip-нути `UserCard`, Compose має розуміти стабільність `UserUiModel`.

Якщо тип unstable, Compose може частіше recomposed composable, бо не може гарантувати, що всередині нічого не змінилося.

2. **@Immutable**

`@Immutable` означає: всі public properties обʼєкта не змінюються після створення.

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Це хороший UI model:

- всі поля `val`;
- типи полів immutable або stable;
- немає прихованої мутації;
- значення змінюється через створення нового instance.

3. **Поганий @Immutable**

Погано:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

Це неправда. `MutableList` можна змінити після створення:

```kotlin
user.tags.add("new")
```

Compose може вважати object immutable, але фактично він змінюється. Це може дати некоректний UI або missed recomposition.

Краще:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

І не мутувати список inplace.

4. **@Stable**

`@Stable` означає, що тип має стабільну поведінку для Compose:

- `equals` дає стабільний результат для тих самих значень;
- зміни public properties будуть повідомлені Compose;
- всі public property types теж stable.

Приклад:

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

Обʼєкт mutable, але Compose бачить зміни через `mutableStateOf`/`mutableIntStateOf`.

5. **Головна різниця**

```text
@Immutable -> обʼєкт не змінюється після створення
@Stable    -> обʼєкт може змінюватися, але Compose знає, як це відстежити
```

`@Immutable` — сильніша гарантія.  
`@Stable` — слабша, але корисна для state holder-ів.

6. **Приклад Stable state holder**

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

Такий клас може бути stable, бо зміна `query` проходить через Compose state.

Але якщо зробити так:

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Compose не буде автоматично знати про зміну `query`. Це погана анотація.

7. **Коли використовувати @Immutable**

Використовувати для:

- UI models;
- value objects;
- screen state data classes;
- sealed state models;
- DTO-to-UI mapped models, якщо вони реально immutable.

Приклад:

```kotlin
@Immutable
data class ProfileUiState(
    val isLoading: Boolean,
    val user: UserUiModel?,
    val errorMessage: String?
)
```

8. **Коли використовувати @Stable**

Використовувати для:

- custom state holders;
- controller objects;
- classes із Compose observable state;
- обʼєктів, які мають mutable behavior, але контрольовано повідомляють Compose.

Приклад:

```kotlin
@Stable
class SnackbarController {
    var message by mutableStateOf<String?>(null)
        private set

    fun show(message: String) {
        this.message = message
    }
}
```

9. **Не використовувати анотації як “лікування” performance**

Погано:

```kotlin
@Stable
class HugeMutableManager {
    val users = mutableListOf<User>()
}
```

Це обман Compose compiler-а. Анотації — це contract. Якщо contract неправдивий, можна отримати missed recompositions і важкі bugs.

Спочатку треба зробити модель реально immutable/stable, а не просто поставити annotation.

10. **Immutable collections**

Звичайний `List<T>` у Kotlin — read-only interface, але не гарантує повну immutability underlying collection.

Для суворішої гарантії можна використовувати persistent immutable collections:

```kotlin
import kotlinx.collections.immutable.ImmutableList

@Immutable
data class FeedUiState(
    val items: ImmutableList<FeedItemUiModel>
)
```

Це особливо корисно у великих Compose-проєктах.

11. **Практичне правило**

- Для data class UI state — переважно `@Immutable`.
- Для state holder з `mutableStateOf` — `@Stable`.
- Не анотувати mutable objects як immutable.
- Не анотувати клас, якщо не можеш гарантувати contract.
- Уникати `MutableList`, `MutableMap`, mutable public fields у UI state.
- Краще створювати новий state object, ніж мутувати старий.

12. **Коротко**

`@Immutable` каже Compose: цей обʼєкт не зміниться після створення. `@Stable` каже: цей обʼєкт може змінюватися, але зміни відстежуються коректно. Обидві анотації впливають на можливість skip recomposition, але їх треба використовувати тільки коли модель реально відповідає contract-у.

</details>
<details>
<summary>122. Навіщо передавати key у LazyColumn і що він собою являє під капотом?</summary>

#### Kotlin

`key` у `LazyColumn` потрібен, щоб Compose міг стабільно ідентифікувати item незалежно від його позиції в списку. Без key Compose переважно орієнтується на index. Якщо item-и додаються, видаляються або міняються місцями, index-based identity може призвести до зайвих recompositions, втрати локального state або “переїзду” state між item-ами.

1. **Базовий приклад**

Погано:

```kotlin
LazyColumn {
    items(users) { user ->
        UserItem(user)
    }
}
```

Краще:

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

`user.id` має бути stable і unique в межах списку.

2. **Проблема без key**

Припустимо, список:

```text
index 0 -> Alice
index 1 -> Bob
index 2 -> Charlie
```

Якщо вставити нового item на початок:

```text
index 0 -> Kate
index 1 -> Alice
index 2 -> Bob
index 3 -> Charlie
```

Без key Compose може сприймати це як “на index 0 був Alice, тепер Kate”, “на index 1 був Bob, тепер Alice”. Тобто identity привʼязана до позиції, а не до item-а.

З key:

```text
id alice -> Alice
id bob -> Bob
id charlie -> Charlie
id kate -> Kate
```

Compose розуміє, що Alice просто змістилася, а не стала новим item-ом.

3. **Локальний state item-а**

Проблема особливо помітна, якщо item має локальний state:

```kotlin
@Composable
fun UserItem(user: User) {
    var expanded by rememberSaveable { mutableStateOf(false) }

    Column(
        modifier = Modifier.clickable { expanded = !expanded }
    ) {
        Text(user.name)
        if (expanded) {
            Text(user.details)
        }
    }
}
```

Без key при insert/delete `expanded` може “переїхати” до іншого item-а, бо Compose асоціював state з позицією.

З key:

```kotlin
items(
    items = users,
    key = { it.id }
) { user ->
    UserItem(user)
}
```

state привʼязується до identity item-а.

4. **Що key собою являє під капотом**

На концептуальному рівні key — це identity token для item-а в composition.

Compose використовує key, щоб:

- зіставити старі й нові item-и між recompositions;
- зберегти remembered state для конкретного item-а;
- коректно обробити reorder/insert/delete;
- оптимізувати reuse composition;
- підтримати item animations.

Це не database key сам по собі, а stable identity для Compose runtime.

5. **Key має бути stable**

Добре:

```kotlin
key = { it.id }
```

Погано:

```kotlin
key = { UUID.randomUUID().toString() }
```

Такий key змінюється на кожну recomposition. Compose вважатиме item новим щоразу.

Також погано:

```kotlin
key = { System.currentTimeMillis() }
```

Key має бути однаковим для того самого item-а між recompositions.

6. **Key має бути unique**

Погано:

```kotlin
key = { it.categoryId }
```

якщо в одній категорії багато item-ів.

Краще:

```kotlin
key = { it.id }
```

Якщо немає одного id, можна зробити composite key:

```kotlin
key = { item -> "${item.type}:${item.localId}" }
```

Але краще мати нормальний stable id у model.

7. **Не використовувати index як key**

Погано:

```kotlin
itemsIndexed(
    items = users,
    key = { index, _ -> index }
) { _, user ->
    UserItem(user)
}
```

Index змінюється при insert/delete/reorder, тому це майже те саме, що не мати key.

Index може бути прийнятним тільки для повністю статичних списків, де порядок і склад не змінюються. Але в реальних списках краще не використовувати index.

8. **Key і animations**

Для item placement animations key критично важливий:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { it.id }
    ) { user ->
        UserItem(
            user = user,
            modifier = Modifier.animateItem()
        )
    }
}
```

Якщо Compose не знає identity item-а, він не може коректно анімувати переміщення.

9. **Key і Paging**

У Paging Compose теж треба задавати key:

```kotlin
val users = viewModel.users.collectAsLazyPagingItems()

LazyColumn {
    items(
        count = users.itemCount,
        key = users.itemKey { it.id }
    ) { index ->
        val user = users[index]
        if (user != null) {
            UserItem(user)
        }
    }
}
```

Це допомагає стабільності item state при підвантаженні сторінок.

10. **Коли key не критичний**

Key менш критичний, якщо:

- список маленький;
- список статичний;
- item-и не мають локального state;
- немає reorder/insert/delete;
- немає animations.

Але як практика для production списків — key краще задавати майже завжди.

11. **Практичне правило**

- Для `LazyColumn` item-ів використовувати stable unique key.
- Найкращий key — server/database id.
- Не використовувати random, time або index як key.
- Якщо item має local state — key обовʼязковий.
- Для reorder/insert/delete/animation — key обовʼязковий.
- Key має представляти identity, а не position.

**Коротко:** `key` у `LazyColumn` — це стабільна identity item-а для Compose runtime. Він потрібен, щоб правильно зберігати state, зіставляти item-и між оновленнями списку, уникати зайвої роботи й коректно обробляти reorder, insert, delete та animations.

</details>
<details>
<summary>123. Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?</summary>

#### Kotlin

У `LazyColumn` функція `items()` приймає не тільки список і item content. Найважливіші параметри — `key` і `contentType`. `key` відповідає за stable identity item-а, а `contentType` допомагає Compose ефективніше reuse-ити composition для item-ів однакового типу.

1. **Типовий signature**

Найчастіший варіант:

```kotlin
LazyColumn {
    items(
        items = users,
        key = { user -> user.id },
        contentType = { "user" }
    ) { user ->
        UserItem(user)
    }
}
```

Основні параметри:

- `items` — список елементів;
- `key` — stable unique identity для item-а;
- `contentType` — тип контенту item-а;
- item lambda — UI для конкретного item-а.

2. **items**

`items` — це data source для lazy list:

```kotlin
items(items = users) { user ->
    UserItem(user)
}
```

Compose не створює UI для всіх елементів одразу. Він compose-ить тільки item-и, потрібні для viewport і невеликого buffer-а.

3. **key**

`key` потрібен для стабільної identity:

```kotlin
items(
    items = users,
    key = { it.id }
) { user ->
    UserItem(user)
}
```

Він важливий для:

- збереження local state item-а;
- коректного reorder/insert/delete;
- item animations;
- меншої кількості зайвої роботи;
- правильного зіставлення item-ів між recompositions.

Не треба використовувати index як key, якщо список може змінюватися.

4. **contentType**

`contentType` описує тип UI, який буде створено для item-а:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { item -> item::class }
) { item ->
    when (item) {
        is FeedItem.Header -> HeaderItem(item)
        is FeedItem.Post -> PostItem(item)
        is FeedItem.Ad -> AdItem(item)
    }
}
```

Compose може ефективніше reuse-ити item composition, якщо знає, що item-и мають однакову структуру.

5. **Навіщо потрібен contentType**

У mixed feed різні item-и мають різний UI:

```text
Header -> один layout
Post   -> інший layout
Ad     -> інший layout
Loader -> інший layout
```

Якщо не передати `contentType`, Compose має менше інформації для reuse. Якщо передати, він розуміє, що `PostItem` краще reuse-ити для інших post item-ів, а не для header або ad.

6. **Приклад із sealed class**

```kotlin
sealed interface FeedItem {
    val id: String

    data class Header(
        override val id: String,
        val title: String
    ) : FeedItem

    data class Post(
        override val id: String,
        val text: String
    ) : FeedItem

    data class Ad(
        override val id: String,
        val imageUrl: String
    ) : FeedItem
}
```

LazyColumn:

```kotlin
LazyColumn {
    items(
        items = feedItems,
        key = { it.id },
        contentType = { item ->
            when (item) {
                is FeedItem.Header -> "header"
                is FeedItem.Post -> "post"
                is FeedItem.Ad -> "ad"
            }
        }
    ) { item ->
        when (item) {
            is FeedItem.Header -> HeaderItem(item)
            is FeedItem.Post -> PostItem(item)
            is FeedItem.Ad -> AdItem(item)
        }
    }
}
```

7. **contentType не є key**

Важливо не плутати:

```text
key         -> хто це?
contentType -> який це тип UI?
```

Наприклад:

```kotlin
key = { it.id }
contentType = { "post" }
```

Усі posts можуть мати однаковий `contentType`, але кожен post має унікальний `key`.

8. **itemsIndexed**

Якщо потрібен index:

```kotlin
LazyColumn {
    itemsIndexed(
        items = users,
        key = { _, user -> user.id },
        contentType = { _, _ -> "user" }
    ) { index, user ->
        UserItem(
            index = index,
            user = user
        )
    }
}
```

Index корисний для UI, але не варто використовувати його як identity key для mutable list.

9. **item() для одиночних елементів**

Окрім `items()`, є `item()`:

```kotlin
LazyColumn {
    item(
        key = "header",
        contentType = "header"
    ) {
        Header()
    }

    items(
        items = users,
        key = { it.id },
        contentType = { "user" }
    ) { user ->
        UserItem(user)
    }
}
```

Це зручно для header, footer, loader, empty state.

10. **Paging**

У Paging Compose є helpers:

```kotlin
val users = viewModel.users.collectAsLazyPagingItems()

LazyColumn {
    items(
        count = users.itemCount,
        key = users.itemKey { it.id },
        contentType = users.itemContentType { "user" }
    ) { index ->
        val user = users[index]
        if (user != null) {
            UserItem(user)
        }
    }
}
```

Це дає stable identity і content reuse для paged data.

11. **Коли contentType особливо корисний**

`contentType` варто задавати, коли:

- список має різні типи item-ів;
- item-и мають суттєво різну структуру;
- є feed із header/post/ad/loader;
- є performance issues під час scroll;
- використовується Paging;
- item-и часто додаються або перемішуються.

Якщо всі item-и одного типу й прості, `contentType` менш критичний, але все одно може бути корисним для явності.

12. **Практичне правило**

- Завжди задавати `key` для production списків.
- Задавати `contentType`, якщо в списку різні UI item types.
- `key` має бути unique і stable.
- `contentType` не має бути unique; він групує item-и за типом layout/content.
- Не використовувати random/time/index як key.
- Для Paging використовувати `itemKey` і `itemContentType`.

**Коротко:** `items()` у `LazyColumn` може приймати `items`, `key`, `contentType` і item content. `key` відповідає за identity конкретного item-а, а `contentType` — за тип його UI, щоб Compose міг ефективніше reuse-ити composition для однакових типів елементів.

</details>
<details>
<summary>124. Що таке SOLID?</summary>

#### Kotlin

SOLID — це набір із пʼяти принципів обʼєктно-орієнтованого дизайну, які допомагають писати код, що легше змінювати, тестувати й розширювати. Це не фреймворк і не набір жорстких правил, а інженерні орієнтири для зменшення coupling і підвищення cohesion.

1. **Що входить у SOLID**

```text
S — Single Responsibility Principle
O — Open/Closed Principle
L — Liskov Substitution Principle
I — Interface Segregation Principle
D — Dependency Inversion Principle
```

Кожен принцип відповідає на окрему проблему дизайну коду.

2. **Single Responsibility Principle**

Клас має мати одну причину для зміни.

Погано:

```kotlin
class UserManager(
    private val api: UserApi,
    private val database: UserDao,
    private val analytics: Analytics
) {
    suspend fun loadUser(id: String): User {
        analytics.track("load_user")
        val user = api.getUser(id)
        database.save(user.toEntity())
        return user.toDomain()
    }
}
```

Тут клас одночасно відповідає за API, cache, analytics і mapping.

Краще розділити:

```kotlin
class UserRepository(
    private val api: UserApi,
    private val dao: UserDao
) {
    suspend fun getUser(id: String): User {
        val dto = api.getUser(id)
        dao.save(dto.toEntity())
        return dto.toDomain()
    }
}
```

Analytics краще викликати на рівні use case або ViewModel залежно від контексту.

3. **Open/Closed Principle**

Код має бути відкритий для розширення, але закритий для постійної модифікації.

Погано:

```kotlin
fun trackEvent(type: String) {
    when (type) {
        "firebase" -> firebaseAnalytics.track()
        "amplitude" -> amplitudeAnalytics.track()
        "console" -> println("event")
    }
}
```

Кожен новий analytics provider змушує змінювати існуючу функцію.

Краще:

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}

class FirebaseTracker : AnalyticsTracker {
    override fun track(event: AnalyticsEvent) {
        // Firebase implementation
    }
}

class AmplitudeTracker : AnalyticsTracker {
    override fun track(event: AnalyticsEvent) {
        // Amplitude implementation
    }
}
```

Нову реалізацію можна додати без переписування клієнтського коду.

4. **Liskov Substitution Principle**

Підтип має коректно замінювати базовий тип без ламання очікуваної поведінки.

Погано:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Bitmap
}

class BrokenImageLoader : ImageLoader {
    override suspend fun load(url: String): Bitmap {
        throw UnsupportedOperationException()
    }
}
```

Якщо код очікує, що `ImageLoader.load()` завантажує зображення, реалізація не має несподівано ламати contract.

Краще contract зробити чесним:

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Result<Bitmap>
}
```

Тоді failure — частина очікуваної поведінки.

5. **Interface Segregation Principle**

Краще мати кілька маленьких інтерфейсів, ніж один великий.

Погано:

```kotlin
interface UserDataSource {
    suspend fun getUser(id: String): User
    suspend fun saveUser(user: User)
    suspend fun deleteUser(id: String)
    suspend fun uploadAvatar(file: File)
    suspend fun observeUsers(): Flow<List<User>>
}
```

Не кожній реалізації потрібні всі методи.

Краще:

```kotlin
interface UserReader {
    suspend fun getUser(id: String): User
}

interface UserWriter {
    suspend fun saveUser(user: User)
}

interface UserObserver {
    fun observeUsers(): Flow<List<User>>
}
```

Клас залежить тільки від того, що йому реально потрібно.

6. **Dependency Inversion Principle**

High-level modules не мають залежати від low-level details. Обидва мають залежати від abstractions.

Погано:

```kotlin
class LoginViewModel : ViewModel() {
    private val api = Retrofit.Builder()
        .baseUrl("https://api.example.com/")
        .build()
        .create(AuthApi::class.java)
}
```

ViewModel напряму створює Retrofit і залежить від low-level detail.

Краще:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase
) : ViewModel()
```

А use case залежить від abstraction:

```kotlin
class LoginUseCase(
    private val authRepository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): LoginResult {
        return authRepository.login(email, password)
    }
}
```

7. **Навіщо SOLID в Android**

SOLID допомагає:

- зменшити залежність UI від framework/details;
- легше тестувати ViewModel/use cases/repositories;
- простіше замінювати API, database, analytics;
- уникати god classes;
- робити модулі незалежнішими;
- підтримувати Clean Architecture/MVVM/MVI.

8. **SOLID не означає overengineering**

Погано створювати abstraction для кожного маленького класу без причини:

```kotlin
interface TextFormatter {
    fun format(text: String): String
}
```

якщо є тільки одна тривіальна реалізація і немає потреби в заміні або тестуванні.

SOLID має зменшувати складність, а не створювати зайві шари.

9. **Практичне правило**

- Якщо клас має багато причин змінюватися — порушено SRP.
- Якщо кожна нова поведінка вимагає редагувати старий `when` — перевірити OCP.
- Якщо subclass/implementation ламає очікування contract-а — проблема LSP.
- Якщо interface змушує реалізувати непотрібні методи — порушено ISP.
- Якщо high-level logic створює Retrofit/Room/SDK напряму — порушено DIP.

**Коротко:** SOLID — це пʼять принципів дизайну, які допомагають будувати код із чіткими відповідальностями, стабільними contract-ами, малими інтерфейсами й залежністю від abstractions. В Android це особливо важливо для тестованості, модульності та підтримки великих codebase-ів.

</details>
<details>
<summary>125. Поясніть кожен принцип SOLID з прикладами з Android.</summary>

#### Kotlin

`SOLID` — це набір принципів, які зменшують coupling, роблять код тестованим і полегшують зміну поведінки без переписування всього модуля.

1. **S — Single Responsibility Principle**

Клас має мати одну причину для зміни.

Погано:

```kotlin
class ProfileViewModel(
    private val api: ProfileApi,
    private val dao: ProfileDao
) : ViewModel() {
    fun loadProfile() {
        // network + db + mapping + UI state
    }
}
```

Краще розділити відповідальності:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}

class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

ViewModel керує state, repository — даними, use case — бізнес-дією.

2. **O — Open/Closed Principle**

Код має бути відкритий для розширення, але закритий для постійної модифікації.

Погано:

```kotlin
fun track(event: Event) {
    if (event.type == "screen") sendScreen(event)
    if (event.type == "click") sendClick(event)
}
```

Краще:

```kotlin
interface EventHandler {
    fun handle(event: Event)
}

class ScreenEventHandler : EventHandler {
    override fun handle(event: Event) {}
}
```

Новий handler додається без переписування центральної функції.

3. **L — Liskov Substitution Principle**

Підтип має коректно замінювати базовий тип без неочікуваної поведінки.

Погано:

```kotlin
interface Storage {
    suspend fun save(data: Data)
}

class ReadOnlyStorage : Storage {
    override suspend fun save(data: Data) {
        throw UnsupportedOperationException()
    }
}
```

Якщо implementation не може виконати контракт, контракт неправильний. Краще розділити:

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

4. **I — Interface Segregation Principle**

Клієнт не має залежати від методів, які йому не потрібні.

Погано:

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun updateUser(user: User)
    suspend fun deleteUser()
    suspend fun uploadAvatar(uri: Uri)
}
```

Краще мати менші контракти:

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Так use case отримує тільки те, що реально використовує.

5. **D — Dependency Inversion Principle**

Високорівневий код має залежати від абстракцій, а не від конкретних реалізацій.

Погано:

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Краще:

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

Тепер use case легко тестувати через fake repository.

6. **Android-приклад структури**

```text
UI -> ViewModel -> UseCase -> Repository interface -> Repository impl -> API/DB
```

- UI не знає про API/DB.
- ViewModel не робить mapping/network.
- Domain залежить від contracts.
- Data реалізує contracts.
- DI зʼєднує abstractions з implementations.

7. **Типові порушення SOLID в Android**

- ViewModel містить network, DB і UI logic одночасно.
- Fragment напряму викликає Retrofit.
- Один repository робить усе для всього app.
- Use case залежить від concrete Room/Retrofit implementation.
- Interface має занадто багато методів.
- Абстракції створені “про всяк випадок”, без реальної потреби.

8. **Практичне правило**

SOLID не означає створювати interface для кожного класу. Він означає, що відповідальності, контракти і залежності мають бути настільки чіткими, щоб код можна було змінити локально і протестувати без важкого Android оточення.

**Коротко:** SOLID в Android допомагає розділяти responsibilities, будувати код через contracts, не змішувати UI/network/database логіку, не створювати “god classes” і робити ViewModel/use cases/repositories тестованими та замінними.

</details>
<details>
<summary>126. Що таке ООП?</summary>

#### Kotlin

ООП — це обʼєктно-орієнтоване програмування, підхід до проєктування коду, де система моделюється через обʼєкти. Обʼєкт поєднує стан і поведінку: дані зберігаються у властивостях, а поведінка описується методами. В Kotlin ООП використовується разом із функціональними можливостями мови.

1. **Базова ідея**

Клас описує тип обʼєкта:

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

Обʼєкт — конкретний instance класу:

```kotlin
val user = User(
    id = "1",
    name = "Alex"
)
```

Тут `User` — клас, `user` — обʼєкт.

2. **Стан і поведінка**

Стан:

```kotlin
val id: String
val name: String
```

Поведінка:

```kotlin
fun displayName(): String
```

Ідея ООП — тримати повʼязані дані й операції поруч, якщо вони належать одній концепції.

3. **Основні принципи ООП**

Зазвичай говорять про чотири базові принципи:

- інкапсуляція;
- абстракція;
- наслідування;
- поліморфізм.

У Kotlin вони реалізуються через класи, інтерфейси, visibility modifiers, inheritance, sealed classes, overriding тощо.

4. **Інкапсуляція**

Інкапсуляція — це приховування деталей реалізації і контроль доступу до стану.

Погано:

```kotlin
class Cart {
    val items = mutableListOf<CartItem>()
}
```

Будь-хто може змінити `items` напряму.

Краще:

```kotlin
class Cart {
    private val _items = mutableListOf<CartItem>()
    val items: List<CartItem> get() = _items

    fun add(item: CartItem) {
        _items.add(item)
    }

    fun remove(item: CartItem) {
        _items.remove(item)
    }
}
```

Клас сам контролює, як змінюється його стан.

5. **Абстракція**

Абстракція — це виділення важливого contract-а без деталей реалізації.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

ViewModel не знає, звідки береться user:

```kotlin
class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

Реалізація може бути через Retrofit, Room, fake repository для тестів або in-memory cache.

6. **Наслідування**

Наслідування дозволяє одному класу розширити інший.

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) {
        // common handling
    }
}

class LoginViewModel : BaseViewModel()
```

В Kotlin класи `final` за замовчуванням. Щоб дозволити inheritance, треба явно вказати `open`.

Але в сучасному Android-коді наслідування треба використовувати обережно. Часто композиція краща.

7. **Поліморфізм**

Поліморфізм дозволяє працювати з різними реалізаціями через спільний contract.

```kotlin
interface ImageLoader {
    suspend fun load(url: String): ImageBitmap
}

class CoilImageLoader : ImageLoader {
    override suspend fun load(url: String): ImageBitmap {
        TODO()
    }
}

class FakeImageLoader : ImageLoader {
    override suspend fun load(url: String): ImageBitmap {
        TODO()
    }
}
```

Код залежить від `ImageLoader`, а не від конкретної реалізації:

```kotlin
class AvatarRepository(
    private val imageLoader: ImageLoader
)
```

8. **ООП в Android**

Типові приклади ООП в Android:

- `Activity`, `Fragment`, `ViewModel` — класи з lifecycle;
- `Repository` — abstraction над data access;
- `UseCase` — клас для business operation;
- `Adapter` — клас для відображення списку;
- `interface` для contracts;
- `sealed class/interface` для state і result models.

Наприклад:

```kotlin
sealed interface LoginState {
    data object Idle : LoginState
    data object Loading : LoginState
    data class Error(val message: String) : LoginState
    data object Success : LoginState
}
```

Це теж частина обʼєктного моделювання станів.

9. **Композиція замість наслідування**

Погано:

```kotlin
open class BaseRepository {
    fun log() {}
    fun handleError() {}
    fun mapResponse() {}
}

class UserRepository : BaseRepository()
```

Клас отримує зайву поведінку через inheritance.

Краще:

```kotlin
class UserRepository(
    private val errorMapper: ErrorMapper,
    private val logger: Logger
)
```

Композиція робить залежності явними і тестованими.

10. **ООП не означає “усе має бути класом”**

Kotlin підтримує:

- top-level functions;
- extension functions;
- data classes;
- sealed interfaces;
- higher-order functions;
- lambdas.

Не треба створювати клас там, де достатньо простої функції:

```kotlin
fun UserDto.toDomain(): User {
    return User(id = id, name = name)
}
```

Це нормально і часто краще, ніж штучний mapper-class без потреби.

11. **Практичне правило**

- Клас має моделювати зрозумілу відповідальність.
- Стан треба інкапсулювати.
- Залежності краще передавати через constructor.
- Для contract-ів використовувати interfaces.
- Наслідування використовувати обережно.
- Для варіантів стану добре підходять sealed classes/interfaces.
- Не створювати abstraction без реальної причини.

**Коротко:** ООП — це підхід, де код організований навколо обʼєктів, які мають стан і поведінку. В Android ООП допомагає моделювати UI state, repositories, use cases, lifecycle components і contracts, але найкращий результат дає в поєднанні з композицією, immutability і функціональними можливостями Kotlin.

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

Конструктор створює та ініціалізує обʼєкт. Метод описує поведінку вже створеного обʼєкта. Головна різниця: constructor викликається під час створення instance, а method викликається після створення обʼєкта для виконання конкретної дії.

1. **Конструктор**

У Kotlin primary constructor оголошується прямо в заголовку класу:

```kotlin
class User(
    val id: String,
    val name: String
)
```

Створення обʼєкта:

```kotlin
val user = User(
    id = "1",
    name = "Alex"
)
```

Конструктор відповідає за те, щоб обʼєкт отримав початковий коректний стан.

2. **Метод**

Метод — це функція всередині класу:

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

Виклик:

```kotlin
val displayName = user.displayName()
```

Метод виконує поведінку, а не створює обʼєкт.

3. **Primary constructor**

```kotlin
class Product(
    val id: String,
    val title: String,
    val price: BigDecimal
)
```

Параметри з `val` або `var` стають властивостями класу.

```kotlin
val product = Product(
    id = "p1",
    title = "Phone",
    price = BigDecimal("999.00")
)
```

4. **init block**

Якщо потрібна додаткова логіка ініціалізації:

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

`init` виконується під час створення обʼєкта.

5. **Secondary constructor**

Kotlin підтримує secondary constructors:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(
        id = id,
        name = "Unknown"
    )
}
```

Але в Kotlin частіше використовують default parameters:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Це простіше й читабельніше.

6. **Методи можуть викликатися багато разів**

```kotlin
class Counter {
    private var value = 0

    fun increment() {
        value++
    }

    fun currentValue(): Int {
        return value
    }
}
```

Обʼєкт створюється один раз:

```kotlin
val counter = Counter()
```

Методи можуть викликатися багато разів:

```kotlin
counter.increment()
counter.increment()
val value = counter.currentValue()
```

7. **Конструктор не має return type**

Метод має return type:

```kotlin
fun fullName(): String {
    return "$firstName $lastName"
}
```

Конструктор не оголошує return type. Він завжди створює instance класу або кидає exception під час ініціалізації.

8. **Constructor injection**

В Android constructor часто використовують для dependency injection:

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

Залежності передаються через constructor, а поведінка реалізується в методі `invoke`.

9. **ViewModel приклад**

```kotlin
class ProfileViewModel(
    private val getProfile: GetProfileUseCase
) : ViewModel() {

    fun load(userId: String) {
        viewModelScope.launch {
            val profile = getProfile(userId)
            // update state
        }
    }
}
```

Constructor отримує залежність `getProfile`.  
Метод `load()` виконує дію.

10. **Що не варто робити в constructor**

Конструктор має бути легким. Не варто запускати:

- network request;
- database query;
- coroutine;
- важку IO-операцію;
- side effects;
- Android lifecycle-dependent logic.

Погано:

```kotlin
class UserRepository(
    private val api: UserApi
) {
    init {
        runBlocking {
            api.preloadUsers()
        }
    }
}
```

Краще:

```kotlin
class UserRepository(
    private val api: UserApi
) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

11. **Практичне правило**

- Constructor — створення обʼєкта і базова валідація state.
- Method — поведінка обʼєкта після створення.
- Constructor викликається при `ClassName(...)`.
- Method викликається через instance: `object.method()`.
- Constructor не має return type.
- Method може повертати значення.
- Важку роботу краще не робити в constructor.

**Коротко:** конструктор відповідає за створення й початкову ініціалізацію обʼєкта, а метод — за дії, які цей обʼєкт виконує після створення. В Android constructor часто використовують для DI, а методи — для business/UI operations.

</details>
<details>
<summary>129. Що таке сигнатура методу?</summary>

#### Kotlin

Сигнатура методу — це набір ознак, за якими компілятор ідентифікує метод і відрізняє його від інших. У Kotlin для функції зазвичай важливі імʼя функції та список параметрів: їх кількість, порядок і типи. Return type сам по собі не використовується для перевантаження функцій.

1. **Базовий приклад**

```kotlin
fun loadUser(id: String): User
```

Сигнатура на практичному рівні:

```text
loadUser(String)
```

Тобто імʼя `loadUser` і параметр типу `String`.

2. **Що входить у сигнатуру**

Для перевантаження в Kotlin важливі:

- імʼя функції;
- кількість параметрів;
- типи параметрів;
- порядок параметрів.

Приклад:

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Це різні overload-и, бо параметри відрізняються.

3. **Return type не достатній для overload**

Погано:

```kotlin
fun getValue(): String = "text"
fun getValue(): Int = 42
```

Так не можна, бо сигнатура для виклику однакова:

```text
getValue()
```

Код:

```kotlin
val value = getValue()
```

не дає компілятору достатньо інформації, яку функцію викликати тільки за return type.

4. **Параметри з різними типами**

Це валідне перевантаження:

```kotlin
fun track(eventName: String) {
    // track by name
}

fun track(event: AnalyticsEvent) {
    // track typed event
}
```

Сигнатури:

```text
track(String)
track(AnalyticsEvent)
```

5. **Порядок параметрів**

Типи і порядок теж мають значення:

```kotlin
fun createUser(name: String, age: Int)
fun createUser(age: Int, name: String)
```

Це різні сигнатури:

```text
createUser(String, Int)
createUser(Int, String)
```

Але такий API часто поганий для читабельності. Краще не створювати overload-и, які легко переплутати.

6. **Named arguments не змінюють JVM signature**

У Kotlin можна викликати:

```kotlin
createUser(
    name = "Alex",
    age = 30
)
```

Але named arguments — це зручність на рівні Kotlin call site. Вони не означають, що можна мати дві функції, які відрізняються тільки іменами параметрів.

Погано:

```kotlin
fun load(id: String)
fun load(userId: String)
```

Тип параметра однаковий, імʼя параметра не створює нову сигнатуру для overload.

7. **Default parameters і overload ambiguity**

```kotlin
fun loadUser(id: String, forceRefresh: Boolean = false)
```

Цю функцію можна викликати так:

```kotlin
loadUser("1")
loadUser("1", true)
```

Не треба додавати зайвий overload:

```kotlin
fun loadUser(id: String)
```

бо він може створити неоднозначність або дублювання API.

8. **Override і сигнатура**

Щоб override спрацював, сигнатура має відповідати contract-у:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User {
        TODO()
    }
}
```

Якщо змінити тип параметра:

```kotlin
override suspend fun getUser(id: Long): User
```

це вже не override, бо сигнатура інша.

9. **Suspend як частина contract-а**

У Kotlin `suspend` важливий для contract-а функції:

```kotlin
suspend fun load(): User
```

і:

```kotlin
fun load(): User
```

це різні способи виклику з точки зору Kotlin. Якщо interface очікує `suspend`, реалізація теж має бути `suspend`.

10. **Extension functions**

Extension function теж має сигнатуру:

```kotlin
fun UserDto.toDomain(): User
```

Практично її можна розуміти як:

```text
toDomain(UserDto)
```

Receiver type (`UserDto`) важливий для resolution.

11. **Практичне правило**

- Сигнатура — це імʼя + параметри.
- Return type не можна використовувати як єдину різницю для overload.
- Імена параметрів не створюють нову overload-сигнатуру.
- Default parameters часто кращі за дублювання overload-ів.
- Для override сигнатура має збігатися з contract-ом.
- Не створювати overload-и, які важко читати або легко переплутати.

**Коротко:** сигнатура методу — це те, як компілятор розпізнає функцію серед інших: насамперед імʼя, кількість, порядок і типи параметрів. Вона важлива для overload, override, API design і читабельності коду.

</details>
<details>
<summary>130. У чому різниця між перевантаженням (overloading) та перевизначенням (overriding)?</summary>

#### Kotlin

Overloading і overriding — це різні механізми. Overloading — це кілька функцій з однаковою назвою, але різними параметрами в одному scope. Overriding — це заміна реалізації функції з базового класу або інтерфейсу в дочірньому класі.

1. **Overloading**

Overloading — це перевантаження функції за параметрами:

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User {
        TODO()
    }

    suspend fun getUser(id: Long): User {
        TODO()
    }

    suspend fun getUser(email: String, includeDetails: Boolean): User {
        TODO()
    }
}
```

У всіх функцій одна назва `getUser`, але різні сигнатури.

2. **Що може відрізнятися при overloading**

Може відрізнятися:

- кількість параметрів;
- типи параметрів;
- порядок параметрів.

Приклад:

```kotlin
fun track(event: String)
fun track(event: AnalyticsEvent)
fun track(event: String, params: Map<String, String>)
```

Це валідне перевантаження.

3. **Return type не створює overload**

Погано:

```kotlin
fun getValue(): String = "text"
fun getValue(): Int = 42
```

Так не можна, бо return type сам по собі не відрізняє сигнатуру для overload.

4. **Overriding**

Overriding — це перевизначення методу базового contract-а:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository(
    private val api: UserApi
) : UserRepository {

    override suspend fun getUser(id: String): User {
        return api.getUser(id).toDomain()
    }
}
```

Тут `RealUserRepository` дає конкретну реалізацію методу з `UserRepository`.

5. **Override в class inheritance**

У Kotlin клас і методи `final` за замовчуванням. Щоб метод можна було override-ити, він має бути `open` або бути частиною interface/abstract class.

```kotlin
open class BaseAnalyticsTracker {
    open fun track(event: AnalyticsEvent) {
        // default implementation
    }
}

class FirebaseAnalyticsTracker : BaseAnalyticsTracker() {
    override fun track(event: AnalyticsEvent) {
        // Firebase implementation
    }
}
```

Без `open` override неможливий.

6. **Головна різниця**

```text
Overloading -> та сама назва, різні параметри
Overriding  -> та сама сигнатура, нова реалізація в subtype
```

Overloading вирішується на етапі compile time за аргументами виклику.  
Overriding працює через polymorphism: конкретна реалізація вибирається за runtime type обʼєкта.

7. **Поліморфізм через overriding**

```kotlin
interface Logger {
    fun log(message: String)
}

class ConsoleLogger : Logger {
    override fun log(message: String) {
        println(message)
    }
}

class CrashlyticsLogger : Logger {
    override fun log(message: String) {
        // send to Crashlytics
    }
}
```

Клієнтський код:

```kotlin
class LoginUseCase(
    private val logger: Logger
) {
    fun execute() {
        logger.log("Login started")
    }
}
```

`LoginUseCase` не знає, яка саме реалізація `Logger` використовується.

8. **Overloading у Kotlin з default parameters**

Kotlin часто дозволяє не створювати багато overload-ів:

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
) {
    TODO()
}
```

Виклики:

```kotlin
loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

Default parameters часто читабельніші за багато overload-ів.

9. **Overloading і @JvmOverloads**

Для Java interop можна використати:

```kotlin
class UserService {
    @JvmOverloads
    fun loadUsers(
        forceRefresh: Boolean = false,
        limit: Int = 50
    ) {
        TODO()
    }
}
```

Kotlin згенерує overload-и для Java-коду. У Kotlin-коді зазвичай достатньо default parameters.

10. **Типові помилки**

Поганий overloading:

```kotlin
fun save(value: String)
fun save(json: String)
```

Це неможливо, бо сигнатури однакові: `save(String)`.

Поганий overriding:

```kotlin
class RealRepository : UserRepository {
    suspend fun getUser(id: Long): User {
        TODO()
    }
}
```

Якщо interface очікує `getUser(id: String)`, це не override. Потрібна така сама сигнатура:

```kotlin
override suspend fun getUser(id: String): User
```

11. **Практичне правило**

- Overloading використовувати, коли одна операція має кілька зручних способів виклику.
- Не створювати overload-и, які легко переплутати.
- У Kotlin часто краще default parameters і named arguments.
- Overriding використовувати для polymorphism і реалізації contract-ів.
- Override має зберігати очікувану поведінку базового contract-а.
- Для Android-архітектури overriding частіше проявляється через interfaces: repositories, providers, trackers.

**Коротко:** overloading — це одна назва функції з різними параметрами. Overriding — це нова реалізація методу з базового класу або інтерфейсу з тією самою сигнатурою. Overloading дає зручність API, overriding дає поліморфізм.

</details>
<details>
<summary>131. У чому різниця між інтерфейсом та абстрактним класом?</summary>

#### Kotlin

Інтерфейс описує contract: що обʼєкт уміє робити. Абстрактний клас описує базову сутність із частковою реалізацією і спільним станом. У Kotlin клас може реалізувати багато інтерфейсів, але наслідувати тільки один клас, у тому числі abstract class.

1. **Interface**

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
    suspend fun saveUser(user: User)
}
```

Інтерфейс каже: “будь-яка реалізація має вміти отримати і зберегти user”.

Реалізація:

```kotlin
class RealUserRepository(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository {

    override suspend fun getUser(id: String): User {
        return dao.getUser(id)?.toDomain()
            ?: api.getUser(id).toDomain()
    }

    override suspend fun saveUser(user: User) {
        dao.save(user.toEntity())
    }
}
```

2. **Abstract class**

```kotlin
abstract class BaseRepository {
    protected fun mapError(error: Throwable): AppError {
        return when (error) {
            is IOException -> AppError.Network
            else -> AppError.Unknown(error)
        }
    }

    abstract suspend fun refresh()
}
```

Абстрактний клас може мати:

- abstract methods;
- concrete methods;
- properties;
- constructor;
- protected state;
- shared implementation.

3. **Головна різниця**

```text
interface       -> contract / capability
abstract class  -> base class / shared partial implementation
```

Інтерфейс відповідає на питання:

```text
Що цей обʼєкт може робити?
```

Абстрактний клас:

```text
Що це за базовий тип і яку спільну реалізацію він дає?
```

4. **Multiple inheritance**

Клас може реалізувати багато інтерфейсів:

```kotlin
class UserRepositoryImpl :
    UserRepository,
    CacheCleaner,
    Syncable {
    // implementation
}
```

Але наслідувати можна тільки один клас:

```kotlin
class ProfileViewModel : BaseViewModel()
```

Не можна:

```kotlin
class MyClass : BaseA(), BaseB()
```

Тому interface гнучкіший для composition contracts.

5. **Default implementation в interface**

У Kotlin interface може мати default methods:

```kotlin
interface Logger {
    fun log(message: String)

    fun logError(error: Throwable) {
        log(error.message ?: "Unknown error")
    }
}
```

Але interface не має зберігати mutable instance state так, як це робить клас.

6. **Properties в interface**

Interface може оголошувати properties:

```kotlin
interface HasUserId {
    val userId: String
}
```

Але він не зберігає backing field для звичайної abstract property. Реалізація має надати значення:

```kotlin
class UserSession(
    override val userId: String
) : HasUserId
```

7. **Constructor**

Abstract class може мати constructor:

```kotlin
abstract class BaseViewModel(
    protected val analytics: AnalyticsTracker
) : ViewModel()
```

Interface constructor не має:

```kotlin
interface AnalyticsOwner {
    val analytics: AnalyticsTracker
}
```

Якщо потрібна обовʼязкова constructor dependency і shared logic, abstract class може бути доречним.

8. **Коли використовувати interface**

Interface краще використовувати для:

- repositories contracts;
- data sources contracts;
- analytics trackers;
- navigators;
- providers;
- validators;
- callback contracts;
- testing/fake implementations.

Приклад:

```kotlin
interface TokenStorage {
    suspend fun save(token: String)
    suspend fun get(): String?
    suspend fun clear()
}
```

Реалізації:

```kotlin
class DataStoreTokenStorage : TokenStorage
class InMemoryTokenStorage : TokenStorage
```

9. **Коли використовувати abstract class**

Abstract class доречний, якщо:

- є спільний стан;
- є спільна реалізація;
- треба constructor;
- є сильний “is-a” звʼязок;
- базовий клас справді моделює спільну сутність.

Приклад:

```kotlin
abstract class BaseMapper<From, To> {
    abstract fun map(value: From): To

    fun mapList(values: List<From>): List<To> {
        return values.map(::map)
    }
}
```

10. **Що краще в Android**

У сучасній Android-архітектурі частіше краще починати з interface:

```kotlin
interface PaymentRepository {
    suspend fun createPayment(amount: Money): PaymentResult
}
```

Це дає:

- менший coupling;
- легші fake implementations;
- простішу DI-конфігурацію;
- кращу модульність.

Abstract base classes варто використовувати обережно, бо вони створюють inheritance hierarchy.

11. **Типова помилка**

Погано:

```kotlin
abstract class BaseFragment : Fragment() {
    fun showError(error: Throwable) {}
    fun showLoading() {}
    fun hideKeyboard() {}
    fun trackScreen() {}
}
```

З часом такий base class стає god object для всіх Fragment.

Краще використовувати composition:

```kotlin
class ErrorRenderer
class KeyboardController
class ScreenAnalyticsTracker
```

і передавати/використовувати тільки те, що реально потрібно.

12. **Практичне правило**

- Якщо потрібен contract — interface.
- Якщо потрібна shared implementation і state — abstract class.
- Якщо потрібні багато capabilities — interfaces.
- Якщо є сильний базовий тип із common behavior — abstract class.
- Не створювати великий base class для всіх випадків.
- В Android для repositories/use cases/providers зазвичай краще interface.

**Коротко:** interface описує можливості й contract, abstract class дає базову часткову реалізацію та може мати стан. Interface гнучкіший і краще підходить для DI, тестів і модульності; abstract class доречний, коли є реальна спільна поведінка, яку треба успадкувати.

</details>
<details>
<summary>132. Чи може інтерфейс не містити методів?</summary>

#### Kotlin

Так, інтерфейс може не містити методів. Такий інтерфейс зазвичай називають marker interface. Він не описує поведінку напряму, а позначає тип як такий, що має певну властивість, роль або contract на рівні дизайну.

1. **Порожній інтерфейс**

```kotlin
interface Cacheable
```

Клас може його реалізувати:

```kotlin
data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

Тепер `UserProfile` позначений як `Cacheable`.

2. **Для чого це потрібно**

Marker interface може використовуватися для:

- позначення типів;
- обмеження generic-параметрів;
- групування моделей;
- runtime/type checks;
- архітектурних contract-ів;
- API design.

Наприклад:

```kotlin
interface UiEvent

data object NavigateBack : UiEvent
data class ShowSnackbar(val message: String) : UiEvent
```

Тут `UiEvent` не має методів, але групує всі UI events.

3. **Приклад із generic constraint**

```kotlin
interface Syncable

class SyncManager {
    fun <T> sync(item: T) where T : Syncable {
        // sync only marked types
    }
}
```

Модель:

```kotlin
data class DraftMessage(
    val id: String,
    val text: String
) : Syncable
```

Тепер `SyncManager` приймає тільки типи, які явно позначені як `Syncable`.

4. **Marker interface у sealed hierarchy**

У Kotlin часто використовують порожні sealed interfaces:

```kotlin
sealed interface LoginEffect

data object NavigateHome : LoginEffect
data class ShowError(val message: String) : LoginEffect
```

`LoginEffect` не має методів, але створює закриту ієрархію можливих effects.

Це зручно для MVI/MVVM:

```kotlin
when (effect) {
    NavigateHome -> navController.navigate("home")
    is ShowError -> snackbarHostState.showSnackbar(effect.message)
}
```

5. **В Android/Java є схожі приклади**

Класичний приклад з Java:

```kotlin
class User : Serializable
```

`Serializable` історично є marker interface. Він позначає, що обʼєкт можна серіалізувати стандартним Java-механізмом.

В Android часто зустрічається `Parcelable`, але він не порожній: там є contract для серіалізації через Parcel.

6. **Коли порожній інтерфейс доречний**

Порожній interface може бути доречний, якщо:

- треба позначити сімейство типів;
- потрібна sealed hierarchy;
- треба generic bound;
- тип має семантичну роль;
- marker реально використовується в API або архітектурі.

Наприклад:

```kotlin
sealed interface FeedItem {
    val id: String
}
```

Це вже не повністю порожній interface, але ідея та сама: він задає типову групу моделей для feed.

7. **Коли це поганий дизайн**

Погано створювати marker interface без реальної потреби:

```kotlin
interface Important
interface Special
interface DataObject
```

Якщо ці marker-и ніде не використовуються, вони тільки створюють шум.

Також погано, якщо marker interface приховує реальний contract, який треба явно описати методом або властивістю.

8. **Альтернатива: annotation**

Іноді замість marker interface краще annotation:

```kotlin
@Target(AnnotationTarget.CLASS)
annotation class Cacheable
```

Використання:

```kotlin
@Cacheable
data class UserProfile(
    val id: String,
    val name: String
)
```

Annotation краще, якщо потрібна metadata для compiler/runtime processing. Interface краще, якщо треба type system і generic constraints.

9. **Практичне правило**

- Інтерфейс може бути без методів.
- Такий інтерфейс має сенс як marker або sealed root.
- Marker має реально використовуватись у type system або API.
- Якщо потрібна поведінка — краще додати метод/властивість.
- Якщо потрібна metadata — можливо, краще annotation.
- Не створювати порожні interfaces “про всяк випадок”.

**Коротко:** інтерфейс без методів можливий і часто використовується як marker interface або sealed root для групи типів. Але він має бути виправданий дизайном: або дає типову ідентичність, або обмежує generic-и, або структурує закриту ієрархію.

</details>
<details>
<summary>133. Чи може інтерфейс містити інший інтерфейс?</summary>

#### Kotlin

Так, інтерфейс може містити інший інтерфейс. У Kotlin це називається nested interface. Такий підхід використовують, коли вкладений contract логічно належить зовнішньому типу або коли треба згрупувати повʼязані API.

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

Вкладений інтерфейс звертається через `UserRepository.Listener`.

2. **Навіщо вкладати interface**

Nested interface може бути доречним, якщо:

- contract тісно повʼязаний із зовнішнім interface/class;
- треба згрупувати типи;
- не хочеться засмічувати package top-level типами;
- вкладений тип не має сенсу сам по собі;
- потрібно зробити API більш структурованим.

3. **Приклад для callback**

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

Реалізація:

```kotlin
class FileDownloadManager : DownloadManager {
    override fun start(
        url: String,
        callback: DownloadManager.Callback
    ) {
        callback.onProgress(0)
        // download
    }
}
```

`Callback` логічно належить `DownloadManager`, тому nesting виправданий.

4. **Nested interface не має доступу до instance state**

Вкладений interface у Kotlin не є inner. Він не має доступу до instance зовнішнього типу.

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` не може напряму читати `id`, бо interface — це type declaration, а не instance-bound object.

5. **Interface може наслідувати інший interface**

Окремий, але схожий випадок:

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

Тут `UserRepository` не містить інші інтерфейси всередині, а наслідує їх.

6. **Приклад для Android UI contract**

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

Такий стиль частіше зустрічався в MVP. У сучасному MVVM/MVI зазвичай краще використовувати окремі `UiState`, `UiEvent`, `UiEffect`, але nested contracts все ще можливі.

7. **Sealed interface всередині contract**

```kotlin
interface LoginContract {
    sealed interface Event {
        data class EmailChanged(val value: String) : Event
        data class PasswordChanged(val value: String) : Event
        data object LoginClicked : Event
    }

    sealed interface Effect {
        data object NavigateHome : Effect
        data class ShowError(val message: String) : Effect
    }
}
```

Використання:

```kotlin
fun onEvent(event: LoginContract.Event) {
    when (event) {
        is LoginContract.Event.EmailChanged -> Unit
        is LoginContract.Event.PasswordChanged -> Unit
        LoginContract.Event.LoginClicked -> Unit
    }
}
```

Це групує типи, але може зробити код більш verbose.

8. **Коли не треба вкладати interface**

Не варто вкладати interface, якщо:

- він використовується в багатьох різних місцях;
- він є самостійним domain contract-ом;
- nesting робить назви занадто довгими;
- тип важко знайти в проєкті;
- зовнішній interface стає контейнером для всього.

Погано:

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
    interface Navigator
}
```

Це штучне групування. Краще окремі top-level interfaces.

9. **Top-level часто простіше**

Замість:

```kotlin
interface UserRepository {
    interface Listener
}
```

можна зробити:

```kotlin
interface UserRepository
interface UserChangeListener
```

Якщо listener використовується не тільки всередині repository API, top-level варіант може бути кращим.

10. **Практичне правило**

- Інтерфейс може містити інший інтерфейс.
- Nested interface доречний, якщо він логічно належить зовнішньому API.
- Якщо contract самостійний — краще top-level interface.
- Не перетворювати interface на namespace для всього.
- У Kotlin nested interface звертається через `Outer.Inner`.
- Nested interface не має доступу до instance state зовнішнього типу.

**Коротко:** інтерфейс у Kotlin може містити інший інтерфейс. Це корисно для callback-ів, contract-ів або згрупованих типів, які мають сенс тільки в контексті зовнішнього API. Але якщо вкладений interface є самостійною абстракцією, краще винести його в top-level тип.

</details>
<details>
<summary>134. Чому не варто додавати в інтерфейс десятки методів?</summary>

#### Kotlin

Не варто додавати в інтерфейс десятки методів, бо такий інтерфейс стає “жирним” contract-ом. Класи змушені реалізовувати методи, які їм не потрібні, код стає важче тестувати, важче змінювати і легше зламати. Це порушує Interface Segregation Principle з SOLID.

1. **Проблема великого інтерфейсу**

Погано:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
    fun observeUsers(): Flow<List<User>>
    suspend fun createUser(user: User)
    suspend fun updateUser(user: User)
    suspend fun deleteUser(id: String)
    suspend fun uploadAvatar(userId: String, file: File)
    suspend fun blockUser(id: String)
    suspend fun unblockUser(id: String)
    suspend fun syncUsers()
    suspend fun clearCache()
}
```

Такий interface покриває забагато сценаріїв одразу.

2. **Класи змушені реалізовувати зайве**

Наприклад, read-only implementation:

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun getUser(id: String): User {
        TODO()
    }

    override fun observeUsers(): Flow<List<User>> {
        TODO()
    }

    override suspend fun createUser(user: User) {
        throw UnsupportedOperationException()
    }

    override suspend fun deleteUser(id: String) {
        throw UnsupportedOperationException()
    }
}
```

Якщо реалізація кидає `UnsupportedOperationException`, це ознака поганого contract-а.

3. **Порушення Interface Segregation Principle**

ISP каже: клієнт не має залежати від методів, які він не використовує.

Погано:

```kotlin
class GetUserProfileUseCase(
    private val repository: UserRepository
)
```

Use case потребує тільки:

```kotlin
suspend fun getUser(id: String): User
```

Але отримує весь великий API.

4. **Краще розділити на малі contracts**

```kotlin
interface UserReader {
    suspend fun getUser(id: String): User
}

interface UserObserver {
    fun observeUsers(): Flow<List<User>>
}

interface UserWriter {
    suspend fun createUser(user: User)
    suspend fun updateUser(user: User)
}

interface UserAvatarUploader {
    suspend fun uploadAvatar(userId: String, file: File): AvatarUrl
}

interface UserCacheCleaner {
    suspend fun clearCache()
}
```

Тепер кожен use case залежить тільки від потрібного contract-а.

5. **UseCase з малим interface**

```kotlin
class GetUserProfileUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(userId: String): User {
        return userReader.getUser(userId)
    }
}
```

Це краще, бо:

- легше тестувати;
- легше підмінити fake;
- менше coupling;
- простіше зрозуміти залежності.

6. **Тестування стає простішим**

Для великого interface fake має реалізувати все:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = testUser
    override fun observeUsers(): Flow<List<User>> = flowOf(emptyList())
    override suspend fun createUser(user: User) = Unit
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    override suspend fun uploadAvatar(userId: String, file: File): AvatarUrl = AvatarUrl("")
    override suspend fun blockUser(id: String) = Unit
    override suspend fun unblockUser(id: String) = Unit
    override suspend fun syncUsers() = Unit
    override suspend fun clearCache() = Unit
}
```

Для малого interface:

```kotlin
class FakeUserReader : UserReader {
    override suspend fun getUser(id: String): User {
        return User(id = id, name = "Test")
    }
}
```

Різниця суттєва.

7. **Великі інтерфейси складніше змінювати**

Якщо додати новий метод у великий interface:

```kotlin
suspend fun exportUsers(): File
```

усі реалізації мають його реалізувати, навіть якщо їм це не потрібно.

Це може зламати:

- production implementations;
- fake implementations;
- test doubles;
- mocks;
- feature modules.

8. **Великі інтерфейси приховують відповідальності**

Назва `UserRepository` може приховувати багато різних ролей:

- read;
- write;
- sync;
- upload;
- cache;
- moderation;
- analytics.

Якщо ролей багато, це сигнал, що contract треба розділити.

9. **Але не треба дробити бездумно**

Погано:

```kotlin
interface GetUserById {
    suspend fun getUser(id: String): User
}

interface GetUserByEmail {
    suspend fun getUserByEmail(email: String): User
}

interface GetUserByPhone {
    suspend fun getUserByPhone(phone: String): User
}
```

Якщо ці методи завжди використовуються разом і належать одному стабільному read contract-у, можна залишити:

```kotlin
interface UserReader {
    suspend fun getUser(id: String): User
    suspend fun getUserByEmail(email: String): User
    suspend fun getUserByPhone(phone: String): User
}
```

Принцип — не “один метод на interface”, а “одна логічна відповідальність”.

10. **Практичне правило**

- Інтерфейс має описувати одну логічну роль.
- Якщо реалізації кидають `UnsupportedOperationException` — contract поганий.
- Якщо use case використовує 1 метод із 20 — interface завеликий.
- Якщо додавання методу ламає багато unrelated класів — interface завеликий.
- Краще кілька малих interfaces, ніж один god interface.
- Але не дробити до абсурду: групувати методи за реальною відповідальністю.

**Коротко:** десятки методів в одному інтерфейсі створюють сильний coupling і змушують клієнтів залежати від зайвого API. Краще розділяти contract-и за ролями: читання, запис, sync, cache, upload тощо. Це робить Android-код простішим для тестування, DI, модульності й підтримки.

</details>
<details>
<summary>135. Що таке композиція і чим вона відрізняється від наслідування?</summary>

#### Kotlin

Композиція — це підхід, коли клас отримує потрібну поведінку через залежності всередині себе. Наслідування — це підхід, коли клас отримує поведінку від базового класу через `is-a` звʼязок. У сучасному Android/Kotlin коді композицію зазвичай варто обирати частіше, бо вона гнучкіша, тестованіша і створює менше жорстких ієрархій.

1. **Наслідування**

Наслідування описує звʼязок “є різновидом”:

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

`Dog` є `Animal`, тому inheritance тут логічний.

2. **Композиція**

Композиція описує звʼязок “має залежність”:

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

`Dog` не наслідує `SoundPlayer`, а використовує його.

3. **Головна різниця**

```text
Inheritance -> is-a
Composition -> has-a / uses-a
```

Наслідування:

```kotlin
class LoginViewModel : BaseViewModel()
```

Композиція:

```kotlin
class LoginViewModel(
    private val errorHandler: ErrorHandler,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

4. **Проблема з великими base classes**

Погано:

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun validateEmail(email: String): Boolean = true
    fun logout() {}
}

class LoginViewModel : BaseViewModel()
class ProfileViewModel : BaseViewModel()
class SettingsViewModel : BaseViewModel()
```

З часом `BaseViewModel` стає god object. Дочірні класи отримують методи, які їм не потрібні.

5. **Композиція замість BaseViewModel**

Краще:

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel() {

    fun onLoginClick(email: String, password: String) {
        analytics.track(LoginClicked)
        // login flow
    }
}
```

Кожна залежність явно показує, що потрібно цьому класу.

6. **Композиція краще тестується**

```kotlin
class CheckoutViewModel(
    private val paymentProcessor: PaymentProcessor,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

У тесті можна підставити fake:

```kotlin
val viewModel = CheckoutViewModel(
    paymentProcessor = FakePaymentProcessor(),
    analytics = FakeAnalyticsTracker()
)
```

З inheritance часто складніше ізолювати поведінку, бо вона схована в base class.

7. **Наслідування створює tight coupling**

Якщо дочірній клас наслідує базовий, він залежить від:

- protected methods;
- protected state;
- lifecycle базового класу;
- неявних припущень parent-а;
- порядку викликів.

Зміна base class може зламати багато subclass-ів.

Композиція локалізує зміни:

```kotlin
class ErrorHandler {
    fun map(error: Throwable): UiError {
        TODO()
    }
}
```

Якщо змінюється `ErrorHandler`, це не ламає ієрархію класів.

8. **Коли наслідування доречне**

Наслідування доречне, якщо:

- є справжній `is-a` звʼязок;
- базовий клас стабільний;
- subclass справді є спеціалізацією parent-а;
- contract parent-а не буде порушено;
- немає потреби в multiple inheritance.

Приклад в Android:

```kotlin
class MainActivity : ComponentActivity()
```

Activity справді є різновидом `ComponentActivity`.

9. **Коли краще композиція**

Композиція краща, якщо:

- треба додати поведінку;
- потрібна тестованість;
- залежність можна замінити;
- є кілька незалежних capabilities;
- не хочеться будувати inheritance hierarchy;
- поведінка не є сутністю класу, а лише використовується ним.

Приклад:

```kotlin
class ProfileRepository(
    private val remoteDataSource: ProfileRemoteDataSource,
    private val localDataSource: ProfileLocalDataSource,
    private val mapper: ProfileMapper
)
```

Repository має data sources і mapper, але не є ними.

10. **Delegation у Kotlin**

Kotlin має зручний механізм delegation:

```kotlin
interface Logger {
    fun log(message: String)
}

class ConsoleLogger : Logger {
    override fun log(message: String) {
        println(message)
    }
}

class UserService(
    private val logger: Logger
) : Logger by logger
```

`UserService` делегує реалізацію `Logger` обʼєкту `logger`. Це форма композиції з мовною підтримкою.

11. **Compose і композиція**

У Jetpack Compose сама назва підказує підхід: UI збирається композицією функцій:

```kotlin
@Composable
fun ProfileScreen(state: ProfileUiState) {
    Column {
        ProfileHeader(state.user)
        ProfileActions()
        ProfileDetails(state.details)
    }
}
```

Замість спадкування custom view hierarchy, UI складається з малих composable-функцій.

12. **Практичне правило**

- Якщо звʼязок `is-a` — можна розглядати inheritance.
- Якщо звʼязок `has-a` або `uses-a` — використовувати composition.
- Не створювати великий `Base*` клас для спільних helper-ів.
- Спільну поведінку краще виносити в окремі залежності.
- Для Android ViewModel/Repository/UseCase частіше краще composition.
- Наслідування має бути коротким, зрозумілим і стабільним.

**Коротко:** наслідування дозволяє класу бути спеціалізацією базового типу, а композиція дозволяє збирати поведінку з окремих залежностей. У Kotlin/Android композиція зазвичай практичніша, бо робить залежності явними, спрощує тести й не створює крихких ієрархій.

</details>
<details>
<summary>136. Які є модифікатори доступу в Kotlin?</summary>

#### Kotlin

У Kotlin є чотири основні модифікатори доступу: `public`, `internal`, `protected`, `private`. Вони визначають, звідки можна бачити клас, функцію, властивість або constructor. На практиці це інструмент для інкапсуляції й контролю public API модуля.

1. **public**

`public` — доступний звідусіль. Це модифікатор за замовчуванням.

```kotlin
class UserRepository {
    fun getUser(id: String): User {
        TODO()
    }
}
```

Те саме, що:

```kotlin
public class UserRepository {
    public fun getUser(id: String): User {
        TODO()
    }
}
```

У Kotlin `public` зазвичай не пишуть явно.

2. **private**

`private` обмежує доступ поточним scope.

У класі:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

`token` доступний тільки всередині `TokenStorage`.

На top-level:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

Top-level `private` доступний тільки в цьому файлі.

3. **protected**

`protected` доступний у класі та його subclasses.

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) {
        // common error handling
    }
}

class LoginViewModel : BaseViewModel() {
    fun login() {
        handleError(IllegalStateException())
    }
}
```

Ззовні `handleError` недоступний.

Важливо: `protected` у Kotlin не працює на top-level declarations.

4. **internal**

`internal` означає доступ у межах одного module.

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Клас доступний у тому самому Gradle module, але не є частиною API для інших modules.

Це дуже корисно в multi-module Android-проєктах.

5. **Що таке module для internal**

Для Kotlin module зазвичай означає:

- Gradle source set/module;
- Maven artifact;
- compilation unit.

Наприклад:

```text
:feature:profile
:core:network
:core:database
```

`internal` у `:feature:profile` не буде доступний напряму з `:feature:settings`.

6. **private constructor**

Можна обмежити створення класу:

```kotlin
class UserId private constructor(
    val value: String
) {
    companion object {
        fun from(value: String): UserId {
            require(value.isNotBlank())
            return UserId(value)
        }
    }
}
```

Тепер обʼєкт можна створити тільки через factory method:

```kotlin
val userId = UserId.from("123")
```

7. **internal constructor**

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Клас може бути public, але constructor — тільки для module. Це корисно, якщо object creation має йти через DI/factory.

8. **Visibility для property setter**

Можна зробити property public для читання, але private для зміни:

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set

    fun login() {
        isLoggedIn = true
    }

    fun logout() {
        isLoggedIn = false
    }
}
```

Зовнішній код може читати:

```kotlin
sessionManager.isLoggedIn
```

але не може напряму змінити.

9. **Практика для Android modules**

У feature module часто роблять public тільки entry point:

```kotlin
public fun NavGraphBuilder.profileGraph() {
    // routes
}
```

А implementation details:

```kotlin
internal class ProfileViewModel : ViewModel()
internal class RealProfileRepository : ProfileRepository
internal fun ProfileScreen()
```

Так інші модулі не залежать від внутрішньої структури feature.

10. **Default visibility і API hygiene**

Оскільки Kotlin за замовчуванням `public`, легко випадково відкрити забагато API.

Погано:

```kotlin
class InternalProfileMapper {
    fun map(dto: ProfileDto): Profile {
        TODO()
    }
}
```

Якщо mapper потрібен тільки в module:

```kotlin
internal class ProfileMapper {
    fun map(dto: ProfileDto): Profile {
        TODO()
    }
}
```

11. **Порівняння**

```text
public    -> видно звідусіль
internal  -> видно в межах module
protected -> видно в класі та subclasses
private   -> видно тільки в поточному scope/file
```

12. **Практичне правило**

- За замовчуванням не відкривати API без потреби.
- Для implementation details у module використовувати `internal`.
- Для helper methods у класі використовувати `private`.
- Для inheritance API використовувати `protected`, але обережно.
- Public API має бути стабільним і навмисним.
- У multi-module Android `internal` — ключовий інструмент архітектурних boundaries.

**Коротко:** модифікатори доступу в Kotlin — це `public`, `internal`, `protected`, `private`. Вони потрібні не тільки для “приховати код”, а для контролю boundaries, інкапсуляції, стабільності API й зменшення coupling між модулями.

</details>
<details>
<summary>137. Що таке singleton?</summary>

#### Kotlin

Singleton — це обʼєкт, який існує в одному екземплярі в межах процесу. Його використовують, коли потрібна одна спільна точка доступу до stateless сервісу, coordinator-а або shared ресурсу. У Kotlin singleton найпростіше створюється через `object`.

1. **Singleton через object**

```kotlin
object AppLogger {
    fun log(message: String) {
        println(message)
    }
}
```

Використання:

```kotlin
AppLogger.log("App started")
```

`AppLogger` буде створений один раз lazy thread-safe способом при першому зверненні.

2. **Навіщо singleton**

Singleton може бути доречним для:

- stateless utility/service;
- app-wide coordinator;
- registry;
- logger;
- feature flags reader;
- shared configuration;
- DI-managed single instance.

Але singleton не має автоматично означати “глобальна змінна для всього”.

3. **Singleton у Kotlin object**

```kotlin
object DateFormatterProvider {
    fun isoDate(): DateTimeFormatter {
        return DateTimeFormatter.ISO_DATE
    }
}
```

Плюси:

- простий синтаксис;
- thread-safe initialization;
- один instance;
- не треба писати manual double-check locking.

4. **Companion object не завжди singleton**

```kotlin
class UserValidator {
    companion object {
        fun isValidEmail(email: String): Boolean {
            return email.contains("@")
        }
    }
}
```

`companion object` — це singleton-обʼєкт, привʼязаний до класу, але сам клас `UserValidator` не стає singleton. Це просто місце для factory/static-like methods або constants.

5. **Singleton через DI scope**

В Android частіше краще керувати singleton-ами через DI:

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
) {
    fun track(event: AnalyticsEvent) {
        // track event
    }
}
```

Hilt створить один instance у `SingletonComponent`.

Перевага DI:

- простіше тестувати;
- залежності явні;
- можна підмінити fake;
- lifecycle контролюється container-ом.

6. **Поганий singleton**

Погано:

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Проблеми:

- memory leak через `Activity`;
- глобальний mutable state;
- складно тестувати;
- складно контролювати lifecycle;
- небезпечно при logout/multi-user сценаріях.

Singleton не має тримати `Activity`, `Fragment`, `View` або `Context`, якщо це не `applicationContext` і немає чіткої потреби.

7. **Application Context**

Якщо singleton потребує context, краще використовувати `applicationContext`:

```kotlin
@Singleton
class ResourceProvider @Inject constructor(
    @ApplicationContext private val context: Context
) {
    fun getString(@StringRes id: Int): String {
        return context.getString(id)
    }
}
```

Не можна зберігати `ActivityContext` у singleton:

```kotlin
@Singleton
class BadTracker @Inject constructor(
    @ActivityContext private val context: Context
)
```

Це може створити memory leak.

8. **Singleton і тестування**

Глобальний `object` складніше підмінити в тестах:

```kotlin
object Analytics {
    fun track(event: String) {}
}
```

Краще залежати від interface:

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Production:

```kotlin
@Singleton
class FirebaseAnalyticsTracker @Inject constructor() : AnalyticsTracker {
    override fun track(event: AnalyticsEvent) {
        // Firebase
    }
}
```

Test:

```kotlin
class FakeAnalyticsTracker : AnalyticsTracker {
    val events = mutableListOf<AnalyticsEvent>()

    override fun track(event: AnalyticsEvent) {
        events.add(event)
    }
}
```

9. **Thread safety**

Kotlin `object` initialization thread-safe. Але внутрішній mutable state — не обовʼязково thread-safe.

Погано:

```kotlin
object Counter {
    var value = 0

    fun increment() {
        value++
    }
}
```

Якщо доступ із різних потоків, потрібна синхронізація:

```kotlin
object Counter {
    private val atomicValue = AtomicInteger(0)

    fun increment(): Int {
        return atomicValue.incrementAndGet()
    }
}
```

10. **Коли singleton не потрібен**

Singleton не потрібен, якщо:

- обʼєкт має screen lifecycle;
- обʼєкт тримає user/session-specific state;
- потрібні різні instances з різними параметрами;
- треба легко ізолювати тести;
- state має очищатися часто;
- залежність краще scoped до ViewModel/Activity/feature.

Наприклад, `ViewModel` не має бути singleton, бо вона привʼязана до lifecycle owner-а.

11. **Практичне правило**

- Для простих stateless Kotlin utilities можна `object`.
- Для app-wide services краще DI singleton.
- Не тримати Android UI references у singleton.
- Уникати глобального mutable state.
- Для тестованості залежати від interface.
- Перевіряти thread safety, якщо singleton має mutable state.
- Scope singleton-а має відповідати реальному lifecycle.

**Коротко:** singleton — це один екземпляр обʼєкта в межах процесу. У Kotlin його легко створити через `object`, але в Android production-коді частіше краще використовувати DI-scoped singleton, щоб мати явні залежності, контроль lifecycle і нормальну тестованість.

</details>
<details>
<summary>138. Що таке generics?</summary>

#### Kotlin

Generics — це механізм параметризації типів. Вони дозволяють писати класи, інтерфейси й функції, які працюють з різними типами, але зберігають type safety на етапі компіляції. У Kotlin generics широко використовуються в колекціях, `Result`, repositories, adapters, use cases, flows.

1. **Базова ідея**

Замість того щоб писати окремий контейнер для кожного типу:

```kotlin
class StringBox(val value: String)
class IntBox(val value: Int)
```

можна написати generic class:

```kotlin
class Box<T>(
    val value: T
)
```

Використання:

```kotlin
val stringBox = Box("Hello")
val intBox = Box(42)
```

Тут `T` — type parameter.

2. **Type safety**

```kotlin
val names: List<String> = listOf("Alex", "Kate")
```

Компілятор знає, що в списку `String`:

```kotlin
val firstName: String = names.first()
```

І не дозволить додати неправильний тип у mutable collection:

```kotlin
val names = mutableListOf<String>()
names.add("Alex")
// names.add(42) // compile error
```

3. **Generic function**

```kotlin
fun <T> singleItemList(item: T): List<T> {
    return listOf(item)
}
```

Використання:

```kotlin
val users: List<User> = singleItemList(User("1", "Alex"))
val numbers: List<Int> = singleItemList(1)
```

Одна функція працює з різними типами.

4. **Generic Result**

Типовий приклад в Android:

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val throwable: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

Використання:

```kotlin
val userResult: Result<User> = Result.Success(user)
val postsResult: Result<List<Post>> = Result.Success(posts)
```

Один `Result` підходить для будь-якого payload.

5. **Generic Repository**

```kotlin
interface CrudRepository<ID, Entity> {
    suspend fun get(id: ID): Entity
    suspend fun save(entity: Entity)
    suspend fun delete(id: ID)
}
```

Реалізація:

```kotlin
class UserRepository : CrudRepository<String, User> {
    override suspend fun get(id: String): User {
        TODO()
    }

    override suspend fun save(entity: User) {
        TODO()
    }

    override suspend fun delete(id: String) {
        TODO()
    }
}
```

Generics дозволяють описати reusable contract.

6. **Upper bounds**

Можна обмежити тип:

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? {
    return items.firstOrNull { it.id == id }
}
```

Тепер `T` має бути типом, який реалізує `Identifiable`.

7. **Multiple bounds**

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable {
    // item has id and is syncable
}
```

Це корисно, коли generic type має відповідати кільком contract-ам.

8. **Variance: out**

`out` означає, що generic type тільки “виробляється” назовні.

```kotlin
interface Producer<out T> {
    fun produce(): T
}
```

Приклад:

```kotlin
val stringProducer: Producer<String> = TODO()
val anyProducer: Producer<Any> = stringProducer
```

Це безпечно, бо producer тільки повертає `T`.

9. **Variance: in**

`in` означає, що generic type тільки “споживається”.

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}
```

Приклад:

```kotlin
val anyConsumer: Consumer<Any> = TODO()
val stringConsumer: Consumer<String> = anyConsumer
```

Це безпечно, бо consumer, який уміє приймати `Any`, точно може прийняти `String`.

10. **Generics у Flow**

```kotlin
fun observeUser(): Flow<User>
fun observePosts(): Flow<List<Post>>
```

`Flow<T>` — generic stream, який emit-ить значення типу `T`.

У ViewModel:

```kotlin
val state: StateFlow<ProfileUiState>
```

Compose знає конкретний тип state.

11. **Reified type parameters**

Через type erasure generic type зазвичай недоступний у runtime. Але inline-функції можуть мати `reified`:

```kotlin
inline fun <reified T> Json.decode(json: String): T {
    return decodeFromString<T>(json)
}
```

Використання:

```kotlin
val user: User = json.decode<User>(rawJson)
```

`reified` дозволяє звертатися до `T::class` всередині inline-функції.

12. **Типові помилки**

Погано:

```kotlin
fun handle(items: List<Any>) {}
```

і очікувати, що можна передати `List<String>` завжди без наслідків. Треба розуміти variance.

Краще:

```kotlin
fun handle(items: List<*>) {}
```

якщо тип елементів не важливий.

Або:

```kotlin
fun <T> handle(items: List<T>) {}
```

якщо треба зберегти тип.

13. **Практичне правило**

- Generics використовують для reusable type-safe API.
- `T` — type parameter, який підставляється конкретним типом.
- Для producer-ів використовувати `out`.
- Для consumer-ів використовувати `in`.
- Для runtime-доступу до типу в Kotlin використовувати `inline reified`.
- Не робити generic API надто абстрактним без потреби.
- Якщо generic ускладнює читання більше, ніж дає користі, краще простіший тип.

**Коротко:** generics дозволяють писати один код для різних типів без втрати type safety. В Kotlin вони особливо важливі для колекцій, `Flow`, `Result`, repositories, mappers і reusable architecture contracts.

</details>
<details>
<summary>139. Що таке type erasure і чому він виникає?</summary>

#### Kotlin

Type erasure — це механізм, через який інформація про generic type parameters здебільшого стирається під час компіляції і недоступна в runtime. Наприклад, `List<String>` і `List<Int>` у runtime виглядають приблизно як просто `List`. Це успадковано з JVM і потрібно для сумісності з Java.

1. **Базовий приклад**

На етапі компіляції Kotlin знає тип:

```kotlin
val names: List<String> = listOf("Alex", "Kate")
val ages: List<Int> = listOf(20, 30)
```

Але в runtime generic information про `String` або `Int` для `List<T>` здебільшого стерта.

Тому не можна написати:

```kotlin
if (value is List<String>) {
    // compile error
}
```

JVM не має повної інформації, чи це саме `List<String>`.

2. **Що залишається після erasure**

Концептуально:

```kotlin
List<String>
List<Int>
List<User>
```

у runtime стають:

```text
List
List
List
```

Generic type parameter `T` стирається або замінюється upper bound-ом.

Наприклад:

```kotlin
class Box<T>(val value: T)
```

у runtime не знає конкретний `T` для кожного instance.

3. **Чому type erasure виникає**

Основна причина — JVM backward compatibility з Java.

Java generics були додані пізніше, ніж сама Java, і реалізовані через erasure, щоб старий bytecode і старі libraries продовжили працювати.

Kotlin на JVM має бути сумісний із JVM model, тому теж має type erasure.

4. **Проблема з is check**

Не можна:

```kotlin
fun handle(value: Any) {
    if (value is List<String>) {
        // impossible check
    }
}
```

Можна перевірити тільки raw/container type:

```kotlin
fun handle(value: Any) {
    if (value is List<*>) {
        // value is some List, element type unknown
    }
}
```

`List<*>` означає list з невідомим типом елементів.

5. **Unchecked cast**

```kotlin
val value: Any = listOf("A", "B")
val strings = value as List<String>
```

Компілятор може попередити про unchecked cast, бо runtime не може повністю перевірити, що всі елементи — `String`.

Безпечніше:

```kotlin
val strings = (value as? List<*>)
    ?.filterIsInstance<String>()
```

Але це вже створює новий список тільки з елементів потрібного типу.

6. **Generic function і T**

Погано:

```kotlin
fun <T> isOfType(value: Any): Boolean {
    return value is T
}
```

Так не можна, бо `T` стертий у runtime.

7. **Reified як рішення для inline functions**

Kotlin має `reified` type parameters для `inline` functions:

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean {
    return value is T
}
```

Використання:

```kotlin
val result = isOfType<String>("hello")
```

Оскільки функція inline, компілятор підставляє конкретний тип у call site.

8. **Приклад з JSON**

Без `reified` треба передавати class/type token:

```kotlin
fun <T> decode(
    json: String,
    clazz: Class<T>
): T {
    TODO()
}
```

Використання:

```kotlin
val user = decode(json, User::class.java)
```

З `reified`:

```kotlin
inline fun <reified T> decode(json: String): T {
    TODO()
}
```

Використання:

```kotlin
val user: User = decode<User>(json)
```

9. **Але reified не вирішує все**

`reified` допомагає з самим типом `T`, але nested generics усе ще можуть бути складними:

```kotlin
decode<List<User>>(json)
```

Для повної інформації про вкладені generic types часто потрібні serializers або type tokens, наприклад у kotlinx.serialization, Moshi, Gson.

10. **Type erasure і overload**

Через erasure не можна мати такі overload-и на JVM:

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {}
```

Після erasure обидві функції виглядають як:

```text
handle(List)
handle(List)
```

Це JVM signature clash.

Краще:

```kotlin
fun handleStrings(items: List<String>) {}
fun handleInts(items: List<Int>) {}
```

або додати додатковий параметр/інший contract.

11. **Star projection**

Якщо тип елементів невідомий:

```kotlin
fun printList(items: List<*>) {
    items.forEach { item ->
        println(item)
    }
}
```

`List<*>` безпечніше, ніж raw `List`, бо Kotlin не дозволить додати туди елемент невідомого типу.

12. **Практичне правило**

- Не перевіряти `value is List<String>` — використовувати `List<*>`.
- Обережно з `as List<T>` — це unchecked cast.
- Для runtime type checks generic `T` використовувати `inline reified`.
- Для JSON і nested generics використовувати serializers/type tokens.
- Не створювати overload-и, які після erasure мають однакову JVM signature.
- Памʼятати, що compile-time type safety не означає повну runtime type information.

**Коротко:** type erasure — це стирання generic type information у runtime на JVM. Він виникає через сумісність із Java/JVM. Kotlin компенсує частину обмежень через `reified`, star projections і type-safe compile-time checks, але generic типи в runtime все одно треба обробляти обережно.

</details>
<details>
<summary>140. Які існують колекції: List, Set, Map, Queue, Stack?</summary>

#### Kotlin

Колекції — це структури даних для зберігання групи елементів. У Kotlin найчастіше використовують `List`, `Set`, `Map`, а для черг і стеків — Java/Kotlin-compatible структури на кшталт `ArrayDeque`. Вибір колекції залежить від того, чи потрібен порядок, унікальність, доступ за ключем, FIFO або LIFO поведінка.

1. **List**

`List` — впорядкована колекція, яка дозволяє дублікати.

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
```

Особливості:

- порядок зберігається;
- доступ за index;
- дублікати дозволені;
- read-only interface у Kotlin.

```kotlin
val first = names[0]
```

Mutable version:

```kotlin
val mutableNames = mutableListOf<String>()
mutableNames.add("Alex")
mutableNames.remove("Alex")
```

2. **Set**

`Set` — колекція унікальних елементів.

```kotlin
val ids: Set<String> = setOf("1", "2", "1")
```

Результат міститиме тільки:

```text
1, 2
```

Особливості:

- дублікати не зберігаються;
- добре підходить для membership check;
- порядок залежить від конкретної реалізації.

```kotlin
if ("1" in ids) {
    // exists
}
```

3. **Map**

`Map` — колекція пар key-value.

```kotlin
val usersById: Map<String, User> = mapOf(
    "1" to User("1", "Alex"),
    "2" to User("2", "Kate")
)
```

Доступ за ключем:

```kotlin
val user = usersById["1"]
```

Особливості:

- key має бути унікальним;
- value може повторюватися;
- зручно для lookup за id;
- порядок залежить від реалізації.

Mutable version:

```kotlin
val cache = mutableMapOf<String, User>()
cache["1"] = User("1", "Alex")
```

4. **Queue**

Queue — структура з поведінкою FIFO:

```text
first in -> first out
```

У Kotlin/JVM часто використовують `ArrayDeque`:

```kotlin
val queue = ArrayDeque<String>()

queue.addLast("A")
queue.addLast("B")

val first = queue.removeFirst() // A
```

Queue корисна для:

- черги задач;
- BFS;
- event processing;
- buffering.

5. **Stack**

Stack — структура з поведінкою LIFO:

```text
last in -> first out
```

Через `ArrayDeque`:

```kotlin
val stack = ArrayDeque<String>()

stack.addLast("A")
stack.addLast("B")

val last = stack.removeLast() // B
```

Stack корисний для:

- DFS;
- undo/redo;
- back stack;
- parsing;
- навігаційних сценаріїв.

6. **ArrayDeque**

`ArrayDeque` може працювати і як queue, і як stack:

Queue:

```kotlin
arrayDeque.addLast(item)
arrayDeque.removeFirst()
```

Stack:

```kotlin
arrayDeque.addLast(item)
arrayDeque.removeLast()
```

У Kotlin це часто кращий вибір, ніж застарілий Java `Stack`.

7. **Read-only vs Mutable**

Kotlin розділяє read-only і mutable interfaces:

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` не має методів `add/remove`.  
`MutableList` має.

Це не завжди означає глибоку immutability underlying object-а, але API обмежує можливість мутації.

8. **Типові реалізації**

На JVM часто використовуються:

- `ArrayList` для `MutableList`;
- `LinkedHashSet` для `Set`;
- `LinkedHashMap` для `Map`;
- `ArrayDeque` для queue/stack behavior.

У більшості Android-коду достатньо працювати через interfaces:

```kotlin
List<User>
Set<String>
Map<String, User>
```

А не привʼязуватися до конкретної реалізації без потреби.

9. **Коли що використовувати**

`List`:

- потрібен порядок;
- потрібен доступ за index;
- дублікати допустимі.

`Set`:

- потрібна унікальність;
- треба швидко перевіряти наявність.

`Map`:

- потрібен доступ за ключем;
- треба кеш або lookup table.

`Queue`:

- треба обробляти в порядку надходження.

`Stack`:

- треба обробляти останній доданий елемент першим.

10. **Приклад в Android**

Список для UI:

```kotlin
data class UsersUiState(
    val users: List<UserUiModel>
)
```

Set для selected ids:

```kotlin
data class UsersUiState(
    val selectedUserIds: Set<String>
)
```

Map для cache:

```kotlin
class UserMemoryCache {
    private val usersById = mutableMapOf<String, User>()

    fun get(id: String): User? = usersById[id]

    fun put(user: User) {
        usersById[user.id] = user
    }
}
```

11. **Performance на практиці**

Типові складності:

- `List` access by index: O(1) для ArrayList;
- `List.contains`: O(n);
- `Set.contains`: зазвичай O(1) для hash-based set;
- `Map.get`: зазвичай O(1) для hash-based map;
- `Queue add/remove` на кінцях `ArrayDeque`: O(1) amortized.

Якщо часто перевіряєш membership, `Set` краще за `List`.

12. **Практичне правило**

- Для UI списків — `List`.
- Для унікальних id — `Set`.
- Для lookup/cache за id — `Map`.
- Для FIFO — `ArrayDeque` як queue.
- Для LIFO — `ArrayDeque` як stack.
- За замовчуванням віддавати read-only interfaces.
- Mutable collections тримати всередині класу й не віддавати назовні напряму.

**Коротко:** `List` зберігає порядок і дублікати, `Set` гарантує унікальність, `Map` дає доступ за ключем, `Queue` працює за FIFO, `Stack` — за LIFO. У Kotlin/Android важливо вибирати колекцію під конкретний сценарій і не відкривати mutable state без потреби.

</details>
<details>
<summary>141. Що таке потокобезпечність колекцій?</summary>

#### Kotlin

Потокобезпечність колекцій означає, що колекцію можна безпечно використовувати з кількох потоків одночасно без race conditions, corrupted state або `ConcurrentModificationException`. Звичайні `MutableList`, `MutableMap`, `MutableSet` у Kotlin/JVM не є потокобезпечними за замовчуванням.

1. **Проблема**

Погано:

```kotlin
val users = mutableListOf<User>()

thread {
    users.add(User("1", "Alex"))
}

thread {
    users.add(User("2", "Kate"))
}
```

Якщо кілька потоків одночасно змінюють `MutableList`, результат може бути непередбачуваним.

2. **Read-only не завжди immutable**

```kotlin
val users: List<User> = mutableListOf()
```

Тип `List` не дозволяє викликати `add/remove` через це посилання, але underlying object може бути mutable.

Тобто `List` — це read-only interface, а не гарантія повної immutability або thread safety.

3. **Race condition**

```kotlin
class UserCache {
    private val users = mutableMapOf<String, User>()

    fun put(user: User) {
        users[user.id] = user
    }

    fun get(id: String): User? {
        return users[id]
    }
}
```

Якщо `put` і `get` викликаються з різних потоків, `MutableMap` може поводитися некоректно, бо вона не синхронізована.

4. **ConcurrentModificationException**

```kotlin
val items = mutableListOf("A", "B", "C")

for (item in items) {
    if (item == "B") {
        items.remove(item)
    }
}
```

Це може кинути `ConcurrentModificationException`, бо колекція змінюється під час iteration.

Правильно:

```kotlin
items.removeAll { it == "B" }
```

Або працювати з копією:

```kotlin
for (item in items.toList()) {
    if (item == "B") {
        items.remove(item)
    }
}
```

5. **Синхронізація через Mutex**

У coroutines можна використовувати `Mutex`:

```kotlin
class UserCache {
    private val mutex = Mutex()
    private val users = mutableMapOf<String, User>()

    suspend fun put(user: User) {
        mutex.withLock {
            users[user.id] = user
        }
    }

    suspend fun get(id: String): User? {
        return mutex.withLock {
            users[id]
        }
    }
}
```

Це робить доступ послідовним і безпечним для coroutine concurrency.

6. **ConcurrentHashMap**

Для JVM можна використовувати concurrent collections:

```kotlin
class UserCache {
    private val users = ConcurrentHashMap<String, User>()

    fun put(user: User) {
        users[user.id] = user
    }

    fun get(id: String): User? {
        return users[id]
    }
}
```

`ConcurrentHashMap` безпечніший для concurrent read/write, ніж `mutableMapOf`.

7. **CopyOnWriteArrayList**

```kotlin
val listeners = CopyOnWriteArrayList<Listener>()
```

Корисно для listeners, коли:

- читання/iteration часті;
- додавання/видалення рідкісні;
- треба безпечно ітерувати під час змін.

Але для частих writes це дорого, бо при зміні створюється копія.

8. **Immutable state як альтернатива**

У UI state краще не мутувати колекцію inplace:

Погано:

```kotlin
data class UsersState(
    val users: MutableList<User>
)
```

Краще:

```kotlin
data class UsersState(
    val users: List<User>
)
```

Оновлення:

```kotlin
_state.update { state ->
    state.copy(
        users = state.users + newUser
    )
}
```

Це простіше для Compose, Flow і reasoning про state.

9. **StateFlow і атомарне оновлення**

```kotlin
private val _state = MutableStateFlow(UsersState())
val state = _state.asStateFlow()

fun addUser(user: User) {
    _state.update { current ->
        current.copy(
            users = current.users + user
        )
    }
}
```

`update` атомарно бере поточне значення і створює нове. Це краще, ніж мутувати список всередині state.

10. **Android main thread**

Іноді кажуть: “у нас усе на main thread, тому безпечно”. Це частково правда тільки якщо всі read/write справді відбуваються на main thread.

Проблеми починаються, коли:

- repository працює на `Dispatchers.IO`;
- callback приходить з background thread;
- cache використовується з кількох coroutine dispatchers;
- listener-и викликаються з різних потоків;
- collection передається між шарами.

Краще не покладатися на припущення, а зробити ownership і threading явними.

11. **Практичне правило**

- Звичайні mutable collections не thread-safe.
- Read-only `List` не гарантує deep immutability.
- Для shared mutable state потрібні `Mutex`, `synchronized`, concurrent collections або immutable copies.
- Для UI state краще immutable list/set/map і `copy`.
- Не мутувати collection під час iteration.
- Не віддавати mutable collection назовні напряму.
- Обирати concurrent structure під конкретний сценарій.

**Коротко:** потокобезпечність колекцій — це гарантія коректної роботи при доступі з кількох потоків. У Kotlin/Android звичайні mutable collections такої гарантії не дають, тому для shared state треба використовувати синхронізацію, concurrent collections або immutable state updates.

</details>
<details>
<summary>142. У чому різниця між final, finally та finalize?</summary>

#### Kotlin

`final`, `finally` та `finalize` — це різні речі. `final` повʼязаний із забороною наслідування або перевизначення. `finally` — блок коду, який виконується після `try/catch`. `finalize` — застарілий механізм Java, який колись викликався GC перед знищенням обʼєкта, але в сучасному коді його не використовують.

1. **final**

У Java `final` може означати:

- клас не можна наслідувати;
- метод не можна override-ити;
- змінну не можна переприсвоїти.

У Kotlin класи й методи `final` за замовчуванням:

```kotlin
class UserRepository
```

Цей клас не можна наслідувати, якщо не написати `open`:

```kotlin
open class BaseRepository
```

2. **final у Kotlin**

```kotlin
open class BaseViewModel : ViewModel() {
    open fun load() {}
}

class ProfileViewModel : BaseViewModel() {
    final override fun load() {
        // cannot be overridden further
    }
}
```

`final override` забороняє наступним subclass-ам перевизначати метод.

3. **final для змінних**

У Kotlin аналогом Java `final` variable є `val`:

```kotlin
val userId = "123"
```

Не можна переприсвоїти:

```kotlin
// userId = "456" // compile error
```

Але важливо: `val` забороняє переприсвоєння reference, а не обовʼязково робить обʼєкт immutable.

```kotlin
val users = mutableListOf<User>()
users.add(User("1", "Alex")) // можна
```

4. **finally**

`finally` — це блок, який виконується після `try`/`catch` незалежно від того, була exception чи ні.

```kotlin
try {
    riskyOperation()
} catch (error: IOException) {
    handleError(error)
} finally {
    closeResources()
}
```

Його використовують для cleanup:

- закрити stream;
- release resource;
- скинути loading state;
- unlock;
- cleanup temporary state.

5. **finally виконується і при exception**

```kotlin
fun readFile() {
    val stream = openStream()
    try {
        stream.read()
    } finally {
        stream.close()
    }
}
```

Навіть якщо `read()` кине exception, `close()` все одно виконається.

6. **finally у coroutines**

У coroutines `finally` теж часто використовують:

```kotlin
viewModelScope.launch {
    _state.update { it.copy(isLoading = true) }

    try {
        repository.sync()
    } catch (error: Throwable) {
        _state.update { it.copy(error = error.message) }
    } finally {
        _state.update { it.copy(isLoading = false) }
    }
}
```

Так loading state гарантовано скинеться.

7. **finalize**

`finalize()` — метод з Java `Object`, який історично міг бути викликаний garbage collector-ом перед знищенням обʼєкта.

```java
@Override
protected void finalize() throws Throwable {
    try {
        // cleanup
    } finally {
        super.finalize();
    }
}
```

У сучасному Java/Kotlin коді `finalize` не використовують.

8. **Чому finalize поганий**

Проблеми `finalize`:

- немає гарантії, коли він виконається;
- може не виконатися взагалі до завершення process;
- створює performance overhead;
- ускладнює GC;
- ненадійний для resource cleanup;
- deprecated у Java.

Для cleanup треба використовувати явне закриття ресурсів.

9. **Що використовувати замість finalize**

Для ресурсів:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

`use` автоматично закриває `Closeable` після виконання block-а.

Для lifecycle resources в Android:

```kotlin
override fun onDestroyView() {
    binding = null
    super.onDestroyView()
}
```

Для Compose:

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()

    onDispose {
        unregisterListener(listener)
    }
}
```

10. **Порівняння**

```text
final    -> заборона inheritance/override/reassignment
finally  -> cleanup block після try/catch
finalize -> застарілий GC callback, не використовувати
```

11. **Практичне правило**

- У Kotlin класи final за замовчуванням.
- Для дозволу наслідування використовувати `open`.
- Для незмінного reference використовувати `val`.
- Для cleanup після exception використовувати `finally` або `use`.
- Для Android lifecycle cleanup використовувати lifecycle callbacks.
- `finalize` не використовувати в production-коді.

**Коротко:** `final` — про заборону зміни/наслідування, `finally` — про гарантований cleanup після `try/catch`, `finalize` — застарілий і ненадійний механізм GC cleanup, який у сучасному Kotlin/Android коді треба уникати.

</details>
<details>
<summary>143. Як працює try-catch-finally?</summary>

#### Kotlin

`try-catch-finally` — це механізм обробки помилок. Код у `try` виконується першим. Якщо виникає exception, керування переходить у відповідний `catch`. Блок `finally` виконується після `try/catch` майже завжди і зазвичай використовується для cleanup.

1. **Базова структура**

```kotlin
try {
    riskyOperation()
} catch (error: IOException) {
    handleNetworkError(error)
} finally {
    cleanup()
}
```

Порядок:

```text
try -> catch якщо була помилка -> finally
```

Якщо помилки не було:

```text
try -> finally
```

2. **try**

У `try` кладуть код, який може кинути exception:

```kotlin
try {
    val response = api.getUser(userId)
    saveUser(response.toDomain())
} catch (error: IOException) {
    showError("Network error")
}
```

Якщо `api.getUser()` кине `IOException`, виконання всередині `try` зупиниться і перейде в `catch`.

3. **catch**

`catch` обробляє конкретний тип exception:

```kotlin
try {
    repository.load()
} catch (error: IOException) {
    showError("No internet")
} catch (error: HttpException) {
    showError("Server error")
}
```

Порядок `catch` важливий: спочатку конкретніші exceptions, потім загальніші.

4. **finally**

`finally` виконується незалежно від того, була помилка чи ні:

```kotlin
var connection: Connection? = null

try {
    connection = openConnection()
    connection.send()
} catch (error: IOException) {
    log(error)
} finally {
    connection?.close()
}
```

Його використовують для:

- закриття ресурсів;
- скидання loading state;
- unlock;
- unregister listener;
- cleanup temporary state.

5. **try як expression**

У Kotlin `try` може повертати значення:

```kotlin
val result = try {
    repository.getUser(userId)
} catch (error: IOException) {
    null
}
```

Тип `result` буде `User?`.

Ще приклад:

```kotlin
val message = try {
    loadMessage()
} catch (error: Throwable) {
    "Fallback message"
}
```

6. **finally не змінює результат зазвичай**

```kotlin
val value = try {
    "success"
} catch (error: Exception) {
    "error"
} finally {
    println("cleanup")
}
```

`finally` виконається, але значенням буде `"success"` або `"error"`.

Не варто робити `return` із `finally`, бо це може приховати exception або результат.

Погано:

```kotlin
fun load(): String {
    return try {
        "success"
    } finally {
        return "from finally"
    }
}
```

Такий код важко читати і він може ламати очікувану поведінку.

7. **runCatching як альтернатива**

У Kotlin часто використовують `runCatching`:

```kotlin
val result = runCatching {
    repository.getUser(userId)
}
```

Обробка:

```kotlin
result
    .onSuccess { user ->
        render(user)
    }
    .onFailure { error ->
        showError(error.message)
    }
```

Але `try-catch` часто читабельніший, коли потрібна різна обробка різних exception types.

8. **try-catch у coroutines**

```kotlin
viewModelScope.launch {
    try {
        val user = repository.getUser(userId)
        _state.value = ProfileState.Content(user)
    } catch (error: IOException) {
        _state.value = ProfileState.Error("Network error")
    }
}
```

Важливо: `try-catch` ловить exception всередині цієї coroutine. Якщо exception виникла в іншій coroutine, потрібна окрема обробка.

9. **CancellationException**

У coroutines треба обережно ловити `Throwable` або `Exception`, бо можна випадково перехопити cancellation.

Погано:

```kotlin
try {
    repository.sync()
} catch (error: Exception) {
    // may catch CancellationException
}
```

Краще:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Cancellation має поширюватися далі.

10. **finally при coroutine cancellation**

`finally` виконується і при cancellation:

```kotlin
val job = viewModelScope.launch {
    try {
        repository.longRunningSync()
    } finally {
        _state.update { it.copy(isSyncing = false) }
    }
}

job.cancel()
```

`finally` допомагає гарантовано скинути state.

11. **use замість manual finally для Closeable**

Для `Closeable` ресурсів краще:

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

`use` автоматично закриє stream навіть при exception.

Це краще, ніж manual:

```kotlin
val stream = FileInputStream(file)
try {
    stream.readBytes()
} finally {
    stream.close()
}
```

12. **Практичне правило**

- У `try` — потенційно небезпечний код.
- У `catch` — обробка конкретних exceptions.
- У `finally` — cleanup, який має виконатися завжди.
- Не ловити занадто загальний `Throwable` без потреби.
- У coroutines не ковтати `CancellationException`.
- Для ресурсів використовувати `use`.
- Не робити `return` із `finally`.

**Коротко:** `try-catch-finally` дозволяє виконати ризикований код, обробити помилки і гарантовано виконати cleanup. У Kotlin `try` може бути expression, а в coroutines треба окремо памʼятати про cancellation.

</details>
<details>
<summary>144. У чому різниця між checked та unchecked exceptions?</summary>

#### Kotlin

Checked exceptions — це exceptions, які Java змушує явно обробити через `try-catch` або оголосити через `throws`. Unchecked exceptions — це exceptions, які компілятор не змушує обробляти. У Kotlin усі exceptions фактично unchecked: мова не вимагає ні `catch`, ні `throws`.

1. **Checked exceptions у Java**

У Java checked exception треба обробити:

```java
void readFile() throws IOException {
    FileInputStream stream = new FileInputStream("file.txt");
}
```

Або:

```java
try {
    readFile();
} catch (IOException error) {
    error.printStackTrace();
}
```

Якщо не зробити ні `throws`, ні `catch`, Java-код не скомпілюється.

2. **Unchecked exceptions у Java**

Unchecked exceptions — це нащадки `RuntimeException`.

```java
String value = null;
value.length(); // NullPointerException
```

Компілятор Java не змушує обробляти `NullPointerException`, `IllegalArgumentException`, `IllegalStateException`, `IndexOutOfBoundsException`.

3. **Kotlin не має checked exceptions**

У Kotlin можна викликати Java-метод, який кидає checked exception, без `try-catch`:

```kotlin
fun readFile(path: String): String {
    return File(path).readText()
}
```

Якщо виникне `IOException`, вона впаде в runtime, але компілятор Kotlin не змусить її обробити.

4. **Чому Kotlin так зробив**

Checked exceptions часто створюють boilerplate і погано працюють із functional style, lambdas, coroutines та API composition.

Kotlin обрав підхід:

```text
exceptions існують, але compiler не примушує їх обробляти
```

Відповідальність переноситься на дизайн API й домовленості в команді.

5. **Приклади unchecked exceptions**

```kotlin
throw IllegalArgumentException("Invalid user id")
throw IllegalStateException("User is not logged in")
throw IndexOutOfBoundsException()
```

Це зазвичай programming errors або порушення contract-а.

Наприклад:

```kotlin
fun getUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

`require` кидає `IllegalArgumentException`.

6. **Recoverable vs programming errors**

Практично важливіше не checked/unchecked, а чи можна відновитися.

Recoverable:

- немає інтернету;
- server повернув 500;
- token expired;
- файл не знайдено;
- permission denied.

Programming errors:

- передали invalid argument;
- порушили state machine;
- unexpected null;
- index out of bounds;
- неправильно використали API.

Recoverable errors краще моделювати явно.

7. **Result замість exceptions для очікуваних помилок**

Для domain/API результатів часто краще sealed result:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkError : LoginResult
    data class UnknownError(val cause: Throwable) : LoginResult
}
```

Use case:

```kotlin
suspend fun login(email: String, password: String): LoginResult {
    return try {
        api.login(email, password)
        LoginResult.Success
    } catch (error: IOException) {
        LoginResult.NetworkError
    } catch (error: Throwable) {
        LoginResult.UnknownError(error)
    }
}
```

Так caller бачить усі очікувані стани через type system.

8. **Exceptions для contract violations**

Для неправильного використання API exceptions нормальні:

```kotlin
class UserId private constructor(val value: String) {
    companion object {
        fun from(value: String): UserId {
            require(value.isNotBlank()) {
                "UserId must not be blank"
            }
            return UserId(value)
        }
    }
}
```

Порожній id — це помилка програміста, а не recoverable user scenario.

9. **Java interop і @Throws**

Якщо Kotlin-функцію викликає Java-код і треба показати checked exception, можна використати `@Throws`:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String {
    return File(path).readText()
}
```

Для Java це виглядатиме як метод із `throws IOException`.

У чистому Kotlin-коді `@Throws` зазвичай не потрібен.

10. **Coroutines**

У coroutines exceptions теж unchecked з точки зору компілятора:

```kotlin
viewModelScope.launch {
    val user = repository.getUser(userId)
}
```

Якщо `getUser` кине exception і її не обробити, coroutine завершиться з помилкою.

Краще:

```kotlin
viewModelScope.launch {
    try {
        val user = repository.getUser(userId)
        _state.value = ProfileState.Content(user)
    } catch (error: IOException) {
        _state.value = ProfileState.Error("Network error")
    }
}
```

11. **Практичне правило**

- У Kotlin немає checked exceptions на рівні compiler enforcement.
- Очікувані recoverable помилки краще повертати як `Result`/sealed class.
- Programming errors можна кидати через `require`, `check`, `error`.
- Java interop за потреби оформлювати через `@Throws`.
- Не ловити всі exceptions без розуміння, що з ними робити.
- У coroutines не ковтати `CancellationException`.

**Коротко:** checked exceptions — це Java-механізм примусової обробки помилок компілятором. Unchecked exceptions не потребують явної обробки. У Kotlin усі exceptions unchecked, тому хороший API має явно моделювати очікувані помилки через типи, а exceptions залишати для неочікуваних або contract-violating ситуацій.

</details>
<details>
<summary>145. Які існують типи помилок у Kotlin/Java?</summary>

#### Kotlin

У Kotlin/Java всі помилки й exceptions у JVM-ієрархії походять від `Throwable`. На верхньому рівні є дві великі гілки: `Exception` і `Error`. `Exception` зазвичай означає ситуації, які application code потенційно може обробити. `Error` — серйозні проблеми JVM або runtime, які зазвичай не треба ловити.

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

У Kotlin ця JVM-ієрархія така сама, але Kotlin не має checked exceptions на рівні compiler enforcement.

2. **Throwable**

`Throwable` — базовий тип для всього, що можна `throw` і `catch`.

```kotlin
try {
    riskyOperation()
} catch (throwable: Throwable) {
    // catches almost everything
}
```

Але ловити `Throwable` у production-коді треба дуже обережно, бо можна перехопити `Error` або `CancellationException`.

3. **Exception**

`Exception` — це помилки, які application code часто може обробити:

```kotlin
try {
    repository.loadUser()
} catch (error: Exception) {
    showError(error.message)
}
```

Приклади:

- `IOException`;
- `HttpException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

4. **RuntimeException**

`RuntimeException` — unchecked exceptions, які часто означають programming error або неправильне використання API.

Приклади:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Також:

- `NullPointerException`;
- `IndexOutOfBoundsException`;
- `ClassCastException`;
- `NumberFormatException`.

5. **Checked exceptions**

У Java checked exceptions — це exceptions, які треба явно обробляти або оголошувати:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

Приклади:

- `IOException`;
- `SQLException`;
- `FileNotFoundException`;
- `ParseException`.

У Kotlin вони не checked на рівні компілятора:

```kotlin
fun readFile(path: String): String {
    return File(path).readText()
}
```

6. **Error**

`Error` — серйозні проблеми runtime/JVM:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Зазвичай application code не має ловити `Error`, бо після таких станів програма може бути нестабільною.

Погано:

```kotlin
try {
    runApp()
} catch (error: Error) {
    // usually bad idea
}
```

7. **Domain errors**

У хорошій архітектурі очікувані business/data помилки часто моделюють не exceptions, а sealed types:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data object NotFound : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Тоді use case може повернути:

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Failure(val error: AppError) : Result<Nothing>
}
```

8. **Network/API errors**

В Android типові помилки:

- немає інтернету — `IOException`;
- timeout — `SocketTimeoutException`;
- HTTP 401/404/500 — часто `HttpException` або custom API error;
- invalid JSON — serialization exception;
- SSL/certificate issues — SSL exceptions.

Приклад mapping:

```kotlin
fun Throwable.toAppError(): AppError {
    return when (this) {
        is IOException -> AppError.Network
        is HttpException -> when (code()) {
            401 -> AppError.Unauthorized
            404 -> AppError.NotFound
            else -> AppError.Unknown(this)
        }
        else -> AppError.Unknown(this)
    }
}
```

9. **Validation errors**

Validation error не завжди має бути exception.

Краще:

```kotlin
sealed interface EmailValidationResult {
    data object Valid : EmailValidationResult
    data object Empty : EmailValidationResult
    data object InvalidFormat : EmailValidationResult
}
```

ніж:

```kotlin
throw IllegalArgumentException("Invalid email")
```

якщо invalid email — це нормальний user input scenario.

10. **CancellationException**

У coroutines `CancellationException` означає cancellation, а не звичайну помилку.

Погано:

```kotlin
try {
    repository.sync()
} catch (error: Exception) {
    // accidentally catches CancellationException
}
```

Краще:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Cancellation треба пропускати далі.

11. **Практичне правило**

- `Throwable` — базовий тип для всіх throwables.
- `Exception` — потенційно recoverable application-level problems.
- `RuntimeException` — часто programming/API contract errors.
- `Error` — серйозні JVM/runtime problems, зазвичай не ловити.
- Очікувані domain errors краще моделювати sealed classes.
- Network/data exceptions мапити в domain-level errors.
- У coroutines не ковтати `CancellationException`.

**Коротко:** у Kotlin/Java є `Throwable`, від якого походять `Exception` і `Error`. Для application-коду важливо відрізняти recoverable помилки, programming errors, runtime/JVM errors і domain errors. Хороший Android-код не просто ловить усе підряд, а мапить технічні exceptions у зрозумілі domain/UI стани.

</details>
<details>
<summary>146. Який базовий клас для помилок?</summary>

#### Kotlin

Базовий клас для всіх помилок і exceptions у Kotlin/JVM — `Throwable`. Саме від `Throwable` наслідуються дві основні гілки: `Exception` і `Error`. Усе, що можна кинути через `throw` і перехопити через `catch`, має бути `Throwable` або його нащадком.

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

У Kotlin:

```kotlin
throw IllegalArgumentException("Invalid id")
```

`IllegalArgumentException` є нащадком `RuntimeException`, `RuntimeException` — нащадок `Exception`, а `Exception` — нащадок `Throwable`.

2. **Throwable**

`Throwable` має базову інформацію про помилку:

- message;
- cause;
- stack trace;
- suppressed exceptions.

Приклад:

```kotlin
val error = Throwable(
    message = "Something went wrong"
)
```

Але напряму `Throwable` у application-коді кидають рідко. Частіше використовують конкретніші типи.

3. **Exception**

`Exception` зазвичай означає проблему, яку application code потенційно може обробити:

```kotlin
throw IOException("No internet")
```

або:

```kotlin
throw IllegalStateException("User is not authorized")
```

У Kotlin compiler не змушує обробляти exceptions, навіть якщо це Java checked exception.

4. **Error**

`Error` означає серйозну проблему runtime/JVM:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Зазвичай їх не ловлять у звичайному application-коді:

```kotlin
try {
    runApp()
} catch (error: Error) {
    // майже завжди погана ідея
}
```

Після `Error` application може бути в нестабільному стані.

5. **Що можна throw**

У Kotlin можна кинути тільки `Throwable`:

```kotlin
throw IllegalArgumentException("Bad argument")
```

Не можна:

```kotlin
// throw "error" // compile error
```

Рядок не є `Throwable`.

6. **Custom exception**

Можна створити власний exception:

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

Використання:

```kotlin
if (!isLoggedIn) {
    throw UnauthorizedException()
}
```

Але для очікуваних domain states часто краще sealed class/result, а не exception.

7. **Custom domain error без exception**

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
    data object Unknown : AuthError
}
```

Result:

```kotlin
sealed interface AuthResult {
    data object Success : AuthResult
    data class Failure(val error: AuthError) : AuthResult
}
```

Це не `Throwable`, а domain model для очікуваних помилок.

8. **catch Throwable**

Технічно можна:

```kotlin
try {
    riskyOperation()
} catch (throwable: Throwable) {
    log(throwable)
}
```

Але це небезпечно, бо ловить також `Error` і `CancellationException`.

У coroutines краще не ковтати cancellation:

```kotlin
catch (error: CancellationException) {
    throw error
}
```

9. **Практичне правило**

- Базовий клас для всіх throwables — `Throwable`.
- Для recoverable application errors частіше ловити конкретні `Exception`.
- `Error` зазвичай не ловити.
- Не ловити `Throwable` без дуже чіткої причини.
- Custom exceptions створювати від `Exception` або `RuntimeException`.
- Очікувані business errors краще моделювати sealed types.

**Коротко:** базовий клас для помилок у Kotlin/Java — `Throwable`. Від нього походять `Exception` і `Error`. У production-коді важливо не просто знати базовий тип, а правильно розділяти recoverable exceptions, серйозні runtime errors і domain-level помилки.

</details>
<details>
<summary>147. Що таке складність алгоритмів?</summary>

#### Kotlin

Складність алгоритмів — це оцінка того, як ростуть час виконання або використання памʼяті алгоритму зі збільшенням розміру вхідних даних. Зазвичай її описують через Big O notation: `O(1)`, `O(log n)`, `O(n)`, `O(n log n)`, `O(n²)` тощо.

1. **Навіщо це потрібно**

Складність допомагає зрозуміти, чи буде рішення працювати на великих даних.

Наприклад:

```kotlin
fun containsUser(users: List<User>, id: String): Boolean {
    return users.any { it.id == id }
}
```

Це `O(n)`, бо в гіршому випадку треба пройти весь список.

Якщо користувачів 10 — не проблема. Якщо 1 000 000 — вже важливо.

2. **O(1) — константна складність**

Час не залежить від розміру input.

```kotlin
fun firstItem(items: List<String>): String? {
    return items.firstOrNull()
}
```

Доступ до першого елемента — `O(1)`.

Також:

```kotlin
val user = usersById[id]
```

Для `HashMap` lookup зазвичай `O(1)` average.

3. **O(n) — лінійна складність**

Час росте пропорційно кількості елементів.

```kotlin
fun findUser(users: List<User>, id: String): User? {
    for (user in users) {
        if (user.id == id) {
            return user
        }
    }
    return null
}
```

Якщо список у 10 разів більший, роботи приблизно у 10 разів більше.

4. **O(n²) — квадратична складність**

Зазвичай виникає при вкладених циклах.

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    for (i in items.indices) {
        for (j in i + 1 until items.size) {
            if (items[i] == items[j]) {
                return true
            }
        }
    }
    return false
}
```

Для 10 000 елементів це може бути дуже дорого.

Краще через `Set`:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = mutableSetOf<String>()
    for (item in items) {
        if (!seen.add(item)) {
            return true
        }
    }
    return false
}
```

Це `O(n)` average.

5. **O(log n) — логарифмічна складність**

Типовий приклад — binary search у відсортованому списку:

```kotlin
fun binarySearch(items: List<Int>, target: Int): Int {
    var left = 0
    var right = items.lastIndex

    while (left <= right) {
        val mid = (left + right) / 2
        when {
            items[mid] == target -> return mid
            items[mid] < target -> left = mid + 1
            else -> right = mid - 1
        }
    }

    return -1
}
```

На кожному кроці search space ділиться навпіл.

6. **O(n log n)**

Типова складність ефективного сортування:

```kotlin
val sorted = users.sortedBy { it.name }
```

Більшість загальних алгоритмів сортування працюють близько до `O(n log n)`.

7. **Time complexity vs Space complexity**

Time complexity — скільки часу/операцій потрібно.

Space complexity — скільки додаткової памʼяті потрібно.

Наприклад:

```kotlin
fun copyUsers(users: List<User>): List<User> {
    return users.toList()
}
```

Time: `O(n)`  
Space: `O(n)`, бо створюється копія списку.

8. **Приклад List vs Set**

```kotlin
val selectedIds: List<String> = ...
val isSelected = selectedIds.contains(user.id)
```

`List.contains` — `O(n)`.

Якщо перевірка виконується для кожного item у великому списку:

```kotlin
users.map { user ->
    user.copy(isSelected = selectedIds.contains(user.id))
}
```

це може стати `O(n * m)`.

Краще:

```kotlin
val selectedIdSet = selectedIds.toSet()

users.map { user ->
    user.copy(isSelected = user.id in selectedIdSet)
}
```

Lookup у `Set` зазвичай `O(1)` average.

9. **Big O і constants**

Big O ігнорує constants:

```text
O(2n) -> O(n)
O(100n) -> O(n)
```

Але в реальному Android performance constants теж можуть мати значення, особливо на main thread.

Наприклад, `O(n)` з важким image processing у кожному item може лагати сильніше, ніж `O(n log n)` легкого сортування.

10. **Android приклади**

Погано для Compose item:

```kotlin
@Composable
fun UserItem(user: User, selectedIds: List<String>) {
    val selected = selectedIds.contains(user.id)
    Text(user.name)
}
```

Якщо item-ів багато, кожен item робить лінійний пошук.

Краще:

```kotlin
@Composable
fun UserItem(user: User, selectedIds: Set<String>) {
    val selected = user.id in selectedIds
    Text(user.name)
}
```

11. **Практичне правило**

- Один прохід по списку — зазвичай `O(n)`.
- Вкладений цикл по тому самому списку — часто `O(n²)`.
- HashMap/HashSet lookup — зазвичай `O(1)` average.
- Binary search — `O(log n)`, але список має бути sorted.
- Sorting — часто `O(n log n)`.
- Для UI/main thread важливі не тільки Big O, а й реальна вартість операцій.

**Коротко:** складність алгоритмів показує, як швидко росте вартість виконання або памʼяті при збільшенні input. У Android це напряму впливає на плавність UI, scroll performance, роботу з великими списками, кешами й пошуком.

</details>
<details>
<summary>148. Що таке пошук у глибину (DFS)?</summary>

#### Kotlin

DFS або Depth-First Search — це алгоритм обходу графа або дерева, який іде “вглиб” по одному шляху настільки далеко, наскільки можливо, а потім повертається назад і пробує інші гілки. Його використовують для обходу дерев, графів, пошуку шляхів, cycle detection, topological sort, connected components.

1. **Ідея DFS**

Приклад дерева:

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

DFS може пройти так:

```text
A -> B -> D -> E -> C -> F
```

Алгоритм спочатку йде вглиб до кінця гілки, потім повертається.

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

Виклик:

```kotlin
dfs(root)
```

Це простий recursive DFS для дерева.

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

Без `visited` можна потрапити в нескінченну рекурсію.

4. **DFS iterative через stack**

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

`Space O(V)` потрібен для `visited` і stack/recursion call stack.

6. **DFS vs BFS**

DFS:

- іде вглиб;
- використовує stack або recursion;
- добре підходить для topological sort, cycle detection;
- може швидко знайти глибокий target;
- не гарантує найкоротший шлях у unweighted graph.

BFS:

- іде рівнями;
- використовує queue;
- знаходить найкоротший шлях у unweighted graph;
- часто потребує більше памʼяті для широких графів.

7. **Cycle detection**

DFS часто використовують для пошуку циклів:

```kotlin
fun hasCycle(
    graph: Map<String, List<String>>
): Boolean {
    val visited = mutableSetOf<String>()
    val recursionStack = mutableSetOf<String>()

    fun visit(node: String): Boolean {
        if (node in recursionStack) return true
        if (node in visited) return false

        visited.add(node)
        recursionStack.add(node)

        for (neighbor in graph[node].orEmpty()) {
            if (visit(neighbor)) return true
        }

        recursionStack.remove(node)
        return false
    }

    return graph.keys.any { visit(it) }
}
```

Це корисно для dependency graph-ів, build modules, navigation graphs.

8. **DFS для connected components**

```kotlin
fun connectedComponents(
    graph: Map<String, List<String>>
): List<Set<String>> {
    val visited = mutableSetOf<String>()
    val components = mutableListOf<Set<String>>()

    fun visit(node: String, component: MutableSet<String>) {
        if (!visited.add(node)) return

        component.add(node)

        for (neighbor in graph[node].orEmpty()) {
            visit(neighbor, component)
        }
    }

    for (node in graph.keys) {
        if (node !in visited) {
            val component = mutableSetOf<String>()
            visit(node, component)
            components.add(component)
        }
    }

    return components
}
```

9. **Ризик StackOverflow**

Recursive DFS може впасти зі `StackOverflowError`, якщо граф/дерево дуже глибоке:

```text
A -> B -> C -> D -> ... -> 100000 nodes
```

Для дуже глибоких структур краще iterative DFS через `ArrayDeque`.

10. **Android-приклади**

DFS може знадобитися для:

- обходу tree-like UI/data structures;
- dependency graph analysis;
- navigation graph validation;
- file tree traversal;
- comments/replies tree;
- category tree;
- build/module graph tools.

Наприклад, обхід коментарів:

```kotlin
data class Comment(
    val id: String,
    val replies: List<Comment>
)

fun flattenComments(root: Comment): List<Comment> {
    val result = mutableListOf<Comment>()

    fun visit(comment: Comment) {
        result.add(comment)
        comment.replies.forEach(::visit)
    }

    visit(root)
    return result
}
```

11. **Практичне правило**

- DFS іде вглиб, BFS — рівнями.
- Для дерева можна recursive DFS без `visited`.
- Для графа майже завжди потрібен `visited`.
- Для дуже глибоких структур краще iterative DFS.
- DFS не гарантує shortest path у unweighted graph.
- Складність DFS — `O(V + E)`.

**Коротко:** DFS — це алгоритм обходу графа або дерева вглиб через recursion або stack. Він простий, ефективний і дуже корисний для задач із графами, деревами, залежностями, циклами й connected components.

</details>
<details>
<summary>149. Що таке пошук у ширину (BFS)?</summary>

#### Kotlin

BFS або Breadth-First Search — це алгоритм обходу графа або дерева рівнями. Він спочатку відвідує всі сусідні вершини поточного вузла, потім сусідів наступного рівня і так далі. BFS використовує queue і добре підходить для пошуку найкоротшого шляху в unweighted graph.

1. **Ідея BFS**

Приклад дерева:

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

BFS пройде так:

```text
A -> B -> C -> D -> E -> F
```

Тобто рівень за рівнем.

2. **BFS через queue**

```kotlin
data class TreeNode(
    val value: String,
    val children: List<TreeNode> = emptyList()
)

fun bfs(root: TreeNode) {
    val queue = ArrayDeque<TreeNode>()
    queue.addLast(root)

    while (queue.isNotEmpty()) {
        val node = queue.removeFirst()
        println(node.value)

        for (child in node.children) {
            queue.addLast(child)
        }
    }
}
```

Queue працює за FIFO:

```text
first in -> first out
```

3. **BFS для графа**

У графі потрібен `visited`, щоб не ходити по колу:

```kotlin
fun bfs(
    graph: Map<String, List<String>>,
    start: String
) {
    val visited = mutableSetOf<String>()
    val queue = ArrayDeque<String>()

    visited.add(start)
    queue.addLast(start)

    while (queue.isNotEmpty()) {
        val node = queue.removeFirst()
        println(node)

        for (neighbor in graph[node].orEmpty()) {
            if (visited.add(neighbor)) {
                queue.addLast(neighbor)
            }
        }
    }
}
```

`visited.add(neighbor)` повертає `true`, якщо елемента ще не було в set.

4. **Складність**

Для графа:

```text
V — кількість вершин
E — кількість ребер
```

BFS:

```text
Time:  O(V + E)
Space: O(V)
```

Памʼять потрібна для `visited` і queue.

5. **Найкоротший шлях в unweighted graph**

BFS знаходить найкоротший шлях за кількістю ребер у graph без ваг.

```kotlin
fun shortestPath(
    graph: Map<String, List<String>>,
    start: String,
    target: String
): List<String>? {
    val visited = mutableSetOf<String>()
    val queue = ArrayDeque<String>()
    val previous = mutableMapOf<String, String?>()

    visited.add(start)
    queue.addLast(start)
    previous[start] = null

    while (queue.isNotEmpty()) {
        val node = queue.removeFirst()

        if (node == target) {
            break
        }

        for (neighbor in graph[node].orEmpty()) {
            if (visited.add(neighbor)) {
                previous[neighbor] = node
                queue.addLast(neighbor)
            }
        }
    }

    if (target !in previous) return null

    val path = mutableListOf<String>()
    var current: String? = target

    while (current != null) {
        path.add(current)
        current = previous[current]
    }

    return path.asReversed()
}
```

Це корисно для route/search/navigation-like задач без ваг.

6. **DFS vs BFS**

DFS:

- використовує stack або recursion;
- іде вглиб;
- корисний для cycle detection, topological sort;
- не гарантує shortest path.

BFS:

- використовує queue;
- іде рівнями;
- гарантує shortest path в unweighted graph;
- може споживати багато памʼяті на широких графах.

7. **BFS для рівнів дерева**

```kotlin
fun levels(root: TreeNode): List<List<String>> {
    val result = mutableListOf<List<String>>()
    val queue = ArrayDeque<TreeNode>()

    queue.addLast(root)

    while (queue.isNotEmpty()) {
        val levelSize = queue.size
        val level = mutableListOf<String>()

        repeat(levelSize) {
            val node = queue.removeFirst()
            level.add(node.value)

            for (child in node.children) {
                queue.addLast(child)
            }
        }

        result.add(level)
    }

    return result
}
```

Результат:

```text
[
  [A],
  [B, C],
  [D, E, F]
]
```

8. **BFS у grid**

Пошук по сітці — частий приклад BFS:

```kotlin
data class Cell(val row: Int, val col: Int)

fun neighbors(cell: Cell): List<Cell> {
    return listOf(
        Cell(cell.row - 1, cell.col),
        Cell(cell.row + 1, cell.col),
        Cell(cell.row, cell.col - 1),
        Cell(cell.row, cell.col + 1)
    )
}
```

BFS у grid використовується для:

- shortest path;
- flood fill;
- пошуку найближчого елемента;
- maze solving.

9. **Android-приклади**

BFS може знадобитися для:

- пошуку shortest path у navigation graph;
- обходу dependency graph рівнями;
- аналізу дерева категорій по рівнях;
- пошуку найближчого доступного node;
- задач із grid/map;
- побудови рівнів comments/replies.

10. **Типові помилки**

- забути `visited` у графі з циклами;
- використовувати stack замість queue і випадково отримати DFS;
- очікувати shortest path у weighted graph;
- не врахувати памʼять для широкого graph;
- додавати в `visited` після вилучення з queue, через що node може потрапити в queue кілька разів.

Краще додавати в `visited` у момент enqueue:

```kotlin
if (visited.add(neighbor)) {
    queue.addLast(neighbor)
}
```

11. **Практичне правило**

- BFS іде рівнями.
- Для BFS потрібна queue.
- Для graph із циклами потрібен `visited`.
- BFS знаходить shortest path тільки в unweighted graph.
- Складність BFS — `O(V + E)`.
- Для weighted graph потрібні інші алгоритми, наприклад Dijkstra.

**Коротко:** BFS — це обхід графа або дерева в ширину, рівень за рівнем. Він використовує queue і особливо корисний, коли треба знайти найкоротший шлях у графі без ваг або обробити nodes по рівнях.

</details>
<details>
<summary>150. Розкажіть про Clean Architecture.</summary>

#### Kotlin

Clean Architecture — це підхід до побудови системи, де business logic ізольована від UI, database, network, frameworks і platform details. Головна ідея: залежності мають іти всередину — до domain layer, а не навпаки. Domain не має знати про Android, Retrofit, Room або Firebase.

1. **Основні шари**

Типова Android-структура:

```text
presentation -> domain <- data
```

Або детальніше:

```text
UI / ViewModel
     |
   UseCase
     |
Repository interface
     ^
     |
Repository implementation -> API / DB / DataStore
```

2. **Presentation layer**

Presentation відповідає за UI і screen state:

- `Activity`;
- `Fragment`;
- `Composable`;
- `ViewModel`;
- `UiState`;
- `UiEvent`;
- `UiEffect`.

Приклад:

```kotlin
class ProfileViewModel(
    private val getProfile: GetProfileUseCase
) : ViewModel() {

    private val _state = MutableStateFlow(ProfileUiState())
    val state = _state.asStateFlow()

    fun load(userId: String) {
        viewModelScope.launch {
            _state.value = ProfileUiState(isLoading = true)
            val profile = getProfile(UserId(userId))
            _state.value = ProfileUiState(profile = profile)
        }
    }
}
```

ViewModel не знає, чи дані прийшли з network або database.

3. **Domain layer**

Domain — ядро системи:

- business models;
- use cases;
- repository interfaces;
- domain errors;
- business rules.

Domain не має залежати від Android framework.

```kotlin
data class UserProfile(
    val id: UserId,
    val name: String,
    val email: Email
)
```

Use case:

```kotlin
class GetProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(userId: UserId): UserProfile {
        return repository.getProfile(userId)
    }
}
```

4. **Repository interface у domain**

```kotlin
interface ProfileRepository {
    suspend fun getProfile(userId: UserId): UserProfile
}
```

Це contract. Domain знає тільки, що profile можна отримати. Він не знає, як саме.

5. **Data layer**

Data layer містить реалізацію:

- Retrofit API;
- Room DAO;
- DataStore;
- DTO;
- Entity;
- mappers;
- repository implementation.

```kotlin
class RealProfileRepository(
    private val api: ProfileApi,
    private val dao: ProfileDao
) : ProfileRepository {

    override suspend fun getProfile(userId: UserId): UserProfile {
        val cached = dao.getProfile(userId.value)
        if (cached != null) {
            return cached.toDomain()
        }

        val remote = api.getProfile(userId.value)
        dao.save(remote.toEntity())
        return remote.toDomain()
    }
}
```

Data layer залежить від domain contract-а, а не навпаки.

6. **Dependency Rule**

Головне правило:

```text
Outer layers can depend on inner layers.
Inner layers must not depend on outer layers.
```

Тобто:

- presentation може знати domain;
- data може знати domain;
- domain не знає presentation;
- domain не знає data;
- domain не знає Android framework.

7. **DTO, Entity, Domain model**

Не треба використовувати одну модель всюди.

```kotlin
data class ProfileDto(
    val id: String,
    val full_name: String
)

@Entity(tableName = "profiles")
data class ProfileEntity(
    @PrimaryKey val id: String,
    val fullName: String
)

data class UserProfile(
    val id: UserId,
    val fullName: String
)
```

Mappers:

```kotlin
fun ProfileDto.toEntity(): ProfileEntity {
    return ProfileEntity(
        id = id,
        fullName = full_name
    )
}

fun ProfileEntity.toDomain(): UserProfile {
    return UserProfile(
        id = UserId(id),
        fullName = fullName
    )
}
```

Це захищає domain від змін API або database schema.

8. **Use cases**

Use case описує конкретну business operation:

```kotlin
class LoginUseCase(
    private val authRepository: AuthRepository,
    private val analytics: AnalyticsTracker
) {
    suspend operator fun invoke(
        email: Email,
        password: Password
    ): LoginResult {
        val result = authRepository.login(email, password)

        if (result is LoginResult.Success) {
            analytics.track(LoginSucceeded)
        }

        return result
    }
}
```

Use case не має містити UI logic.

9. **Переваги Clean Architecture**

- легше тестувати business logic;
- domain не залежить від Android;
- можна замінити Retrofit/Room/Firebase;
- краща модульність;
- чіткі boundaries;
- менше god classes;
- легше підтримувати великий codebase;
- простіше паралельно працювати команді.

10. **Недоліки**

Clean Architecture може додати:

- більше файлів;
- більше mappers;
- більше interfaces;
- складнішу навігацію по коду;
- overhead для маленьких features.

Тому її треба застосовувати прагматично. Для простого CRUD-екрана не завжди потрібна максимальна кількість шарів.

11. **Типова структура модулів**

```text
:app
:core:network
:core:database
:core:common
:feature:profile:presentation
:feature:profile:domain
:feature:profile:data
```

Або простіше:

```text
:feature:profile
  /presentation
  /domain
  /data
```

Рівень деталізації залежить від масштабу проєкту.

12. **Практичне правило**

- Domain не має імпортувати Android classes.
- ViewModel залежить від use cases, а не від Retrofit/DAO.
- Repository interface ближче до domain.
- Repository implementation у data layer.
- DTO/Entity не мають витікати в UI.
- Use case має одну зрозумілу business operation.
- Не створювати layers без користі для маленьких задач.

**Коротко:** Clean Architecture ізолює business logic від деталей UI, network, database і framework. В Android це зазвичай означає `presentation`, `domain`, `data`, dependency inversion через repository interfaces і чітке розділення моделей між API, DB та domain.

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

Модулі в Android-проєкті треба організовувати так, щоб зменшити coupling, прискорити build, ізолювати фічі й зробити залежності зрозумілими. Хороша модульність — це не “чим більше модулів, тим краще”, а чіткі boundaries, стабільні contracts і контроль напрямку залежностей.

1. **Типова структура**

Для середнього/великого проєкту:

```text
:app
:core:common
:core:network
:core:database
:core:datastore
:core:designsystem
:core:analytics
:feature:auth
:feature:profile
:feature:settings
:feature:payments
```

`:app` збирає весь application graph, а feature/core модулі містять реалізацію.

2. **Напрямок залежностей**

Базове правило:

```text
app -> feature -> core
```

Feature modules можуть залежати від core modules:

```kotlin
dependencies {
    implementation(project(":core:network"))
    implementation(project(":core:designsystem"))
}
```

Але core modules не мають залежати від feature modules.

Погано:

```text
:core:network -> :feature:profile
```

Це ламає архітектурний напрямок.

3. **app module**

`:app` відповідає за:

- application class;
- root navigation;
- DI graph composition;
- build flavors;
- app-level config;
- wiring feature modules.

`:app` не має містити всю business logic.

```kotlin
@HiltAndroidApp
class MainApplication : Application()
```

4. **core modules**

Core modules містять reusable infrastructure:

```text
:core:network       -> Retrofit/OkHttp/API infrastructure
:core:database      -> Room database/shared DB setup
:core:datastore     -> DataStore setup
:core:designsystem  -> UI components/theme
:core:analytics     -> analytics contracts/impl
:core:common        -> common utilities/result models
:core:testing       -> test utilities/fakes
```

Core module має мати одну зрозумілу відповідальність.

5. **feature modules**

Feature module містить код конкретної фічі:

```text
:feature:profile
  /presentation
  /domain
  /data
```

Або розділено на підмодулі:

```text
:feature:profile:presentation
:feature:profile:domain
:feature:profile:data
```

Другий варіант доречний у великих командах/фічах, але може бути overengineering для малого проєкту.

6. **Feature API / implementation split**

Для великих проєктів можна розділяти contract і implementation:

```text
:feature:profile:api
:feature:profile:impl
```

API:

```kotlin
interface ProfileFeatureApi {
    fun profileRoute(userId: String): String
}
```

Implementation:

```kotlin
internal fun NavGraphBuilder.profileGraph() {
    // profile screens
}
```

Це дозволяє іншим модулям залежати тільки від contract-а.

7. **Navigation між features**

Feature не має напряму знати internals іншої feature.

Погано:

```kotlin
navController.navigate("payments/details/$paymentId")
```

зсередини будь-якого модуля без contract-а.

Краще:

```kotlin
interface PaymentsNavigator {
    fun openPaymentDetails(paymentId: String)
}
```

Або централізовані route contracts:

```kotlin
object PaymentsRoutes {
    fun details(paymentId: String): String {
        return "payments/details/$paymentId"
    }
}
```

8. **Domain/data/presentation boundaries**

У feature module:

```text
presentation -> domain <- data
```

Presentation:

- ViewModel;
- Composable/Fragment;
- UiState;
- UiEvent.

Domain:

- UseCase;
- domain models;
- repository interfaces.

Data:

- repository implementations;
- DTO;
- Entity;
- API;
- DAO;
- mappers.

9. **Не створювати циклічні залежності**

Погано:

```text
:feature:profile -> :feature:settings
:feature:settings -> :feature:profile
```

Цикли ускладнюють build, тестування й ownership.

Якщо дві feature залежать одна від одної, треба винести спільний contract:

```text
:feature:profile:api
:feature:settings:api
```

або:

```text
:core:navigation
```

10. **api vs implementation**

У Gradle:

```kotlin
dependencies {
    implementation(project(":core:network"))
}
```

`implementation` приховує dependency від consumers модуля.

`api` відкриває dependency як частину public API:

```kotlin
dependencies {
    api(project(":core:model"))
}
```

Практичне правило: за замовчуванням використовувати `implementation`, `api` — тільки якщо типи реально є частиною public contract-а.

11. **Build time**

Модульність може прискорити build, якщо:

- залежності спрямовані правильно;
- модулі не залежать один від одного без потреби;
- зміни локалізовані;
- annotation processing не розмазаний всюди;
- core API стабільний.

Але надмірна кількість маленьких модулів може ускладнити Gradle config і navigation по коду.

12. **Практична стартова схема**

Для нового не надто великого проєкту:

```text
:app
:core:network
:core:database
:core:designsystem
:core:common
:feature:auth
:feature:home
:feature:profile
```

Всередині feature:

```text
data/
domain/
presentation/
```

Коли feature виростає — тоді можна розбити на `:api`, `:impl`, `:domain`, `:data`.

13. **Практичне правило**

- Модулі мають мати чітку відповідальність.
- Напрямок залежностей: `app -> feature -> core`.
- Core не залежить від feature.
- Feature не лізе у internals іншої feature.
- Public API модуля має бути мінімальним.
- За замовчуванням `implementation`, не `api`.
- Не дробити модулі без реальної потреби.
- Модульність має покращувати build, ownership і boundaries.

**Коротко:** Android-модулі треба організовувати навколо core infrastructure і feature boundaries. Хороша структура контролює залежності, ховає implementation details, не допускає циклів і дозволяє масштабувати codebase без перетворення `:app` або `:core:common` на dump усього коду.

</details>
<details>
<summary>155. Що таке токен?</summary>

#### Kotlin

Токен — це рядок або структуроване значення, яке підтверджує певний факт: автентифікацію користувача, право доступу, сесію, одноразову дію або дозвіл на виклик API. У mobile/backend контексті найчастіше мають на увазі access token і refresh token.

1. **Access token**

Access token використовується для доступу до захищених ресурсів.

Приклад HTTP-запиту:

```http
GET /profile
Authorization: Bearer eyJhbGciOi...
```

В Android:

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

2. **Refresh token**

Refresh token використовується для отримання нового access token, коли старий expired.

Типова схема:

```text
access token  -> короткий TTL
refresh token -> довший TTL
```

Flow:

```text
API request -> 401 Unauthorized -> refresh token -> new access token -> retry request
```

3. **JWT**

JWT — популярний формат токена:

```text
header.payload.signature
```

Payload може містити claims:

```json
{
  "sub": "user-id",
  "exp": 1710000000,
  "role": "user"
}
```

Важливо: JWT payload зазвичай не encrypted, а тільки Base64Url encoded. Не треба зберігати там секретні дані.

4. **Токен не дорівнює пароль**

Пароль — secret, який користувач знає.

Токен — credential, який система видала після login або authorization flow.

Пароль не треба зберігати в app. Токени теж треба зберігати обережно, але їх можна відкликати, обмежувати TTL і scope.

5. **Де зберігати токен в Android**

Для sensitive token storage:

- Android Keystore;
- EncryptedSharedPreferences;
- DataStore + encryption;
- secure storage abstraction.

Приклад:

```kotlin
interface TokenStorage {
    fun getAccessToken(): String?
    fun saveTokens(tokens: AuthTokens)
    fun clear()
}
```

Implementation details краще сховати за interface.

6. **Не логувати токени**

Погано:

```kotlin
Log.d("Auth", "Access token: $token")
```

Токени не мають потрапляти в:

- logs;
- crash reports;
- analytics;
- screenshots;
- query parameters;
- plain text files.

7. **Token expiration**

Access token зазвичай має expiration time:

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String,
    val expiresAtMillis: Long
)
```

Перевірка:

```kotlin
fun AuthTokens.isExpired(clock: Clock): Boolean {
    return clock.nowMillis() >= expiresAtMillis
}
```

Але остаточне рішення про валідність токена зазвичай приймає backend.

8. **Token refresh**

```kotlin
class AuthRepository(
    private val api: AuthApi,
    private val tokenStorage: TokenStorage
) {
    suspend fun refreshTokens(): AuthTokens {
        val refreshToken = tokenStorage.getRefreshToken()
            ?: throw UnauthorizedException()

        val newTokens = api.refreshToken(
            RefreshTokenRequest(refreshToken)
        ).toDomain()

        tokenStorage.saveTokens(newTokens)
        return newTokens
    }
}
```

Refresh logic треба робити акуратно, щоб не запустити 10 refresh-запитів паралельно.

9. **Bearer token**

Bearer token означає: хто володіє токеном, той може його використати.

```http
Authorization: Bearer <token>
```

Тому якщо токен вкрали, атакувальник може діяти від імені користувача до expiration або revoke.

10. **Scope**

Токен може мати scope — набір дозволів:

```text
profile:read
profile:write
payments:create
```

Добра практика — мінімальні permissions:

```text
least privilege
```

Не видавати mobile app токен із admin-level permissions.

11. **Практичне правило**

- Access token використовувати для API-запитів.
- Refresh token використовувати для оновлення access token.
- Токени зберігати безпечно.
- Не логувати й не передавати токени в URL query.
- Робити короткий TTL для access token.
- Підтримувати revoke/logout.
- Не класти server secrets у mobile app.
- JWT payload не вважати секретним.

**Коротко:** токен — це credential для підтвердження доступу або сесії. В Android найчастіше працюють з access/refresh tokens: перший додається в API-запити, другий використовується для оновлення. Головне — безпечне зберігання, короткий lifetime, мінімальні scopes і відсутність токенів у логах.

</details>
<details>
<summary>156. У чому різниця між authentication та authorization?</summary>

#### Kotlin

Authentication і authorization — це різні частини security flow. Authentication відповідає на питання “хто ти?”, а authorization — “що тобі дозволено робити?”. Спочатку система ідентифікує користувача, потім перевіряє його права доступу.

1. **Authentication**

Authentication — це процес підтвердження особи користувача.

Приклади:

- login/password;
- biometric login;
- SMS/OTP code;
- OAuth login через Google/Apple;
- refresh session;
- перевірка access token.

Приклад login API:

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Після успішної authentication backend видає tokens:

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String
)
```

2. **Authorization**

Authorization — це перевірка прав доступу після authentication.

Наприклад, користувач authenticated, але не має права видаляти payment:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

Backend може відповісти:

```http
403 Forbidden
```

Тобто користувача впізнали, але дія заборонена.

3. **Коротка різниця**

```text
Authentication -> хто користувач?
Authorization  -> що цьому користувачу дозволено?
```

Приклад:

```text
401 Unauthorized -> користувач не authenticated або token невалідний
403 Forbidden    -> користувач authenticated, але не має permission
```

Назва `401 Unauthorized` історично трохи плутає, але на практиці це саме проблема authentication.

4. **Приклад в Android**

Authentication:

```kotlin
class LoginUseCase(
    private val authRepository: AuthRepository
) {
    suspend operator fun invoke(
        email: String,
        password: String
    ): LoginResult {
        return authRepository.login(email, password)
    }
}
```

Authorization:

```kotlin
class DeleteUserUseCase(
    private val userRepository: UserRepository,
    private val permissionChecker: PermissionChecker
) {
    suspend operator fun invoke(userId: String): DeleteUserResult {
        if (!permissionChecker.canDeleteUsers()) {
            return DeleteUserResult.Forbidden
        }

        userRepository.deleteUser(userId)
        return DeleteUserResult.Success
    }
}
```

5. **Access token і authentication**

Access token часто підтверджує, що користувач authenticated:

```kotlin
class AuthInterceptor(
    private val tokenStorage: TokenStorage
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenStorage.getAccessToken()

        val request = chain.request()
            .newBuilder()
            .apply {
                if (token != null) {
                    header("Authorization", "Bearer $token")
                }
            }
            .build()

        return chain.proceed(request)
    }
}
```

Backend перевіряє token і розуміє, хто робить request.

6. **Roles і permissions**

Authorization часто базується на ролях або permissions:

```kotlin
enum class Role {
    USER,
    MODERATOR,
    ADMIN
}

enum class Permission {
    READ_PROFILE,
    EDIT_PROFILE,
    DELETE_USER
}
```

Роль може давати набір permissions:

```text
ADMIN -> READ_PROFILE, EDIT_PROFILE, DELETE_USER
USER  -> READ_PROFILE, EDIT_PROFILE
```

7. **Не довіряти тільки клієнту**

Android app може приховати кнопку:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDeleteClick) {
        Text("Delete")
    }
}
```

Але це тільки UX. Справжню authorization має перевіряти backend.

Причина: mobile app можна модифікувати, reverse engineer-ити або викликати API напряму.

8. **401 vs 403**

Обробка в app:

```kotlin
fun Throwable.toAppError(): AppError {
    return when (this) {
        is HttpException -> when (code()) {
            401 -> AppError.Unauthenticated
            403 -> AppError.Forbidden
            else -> AppError.Unknown(this)
        }
        else -> AppError.Unknown(this)
    }
}
```

UI:

```kotlin
when (error) {
    AppError.Unauthenticated -> showLoginScreen()
    AppError.Forbidden -> showNoAccessMessage()
    else -> showGenericError()
}
```

9. **Session expiration**

Якщо access token expired:

```text
API -> 401 -> refresh token -> retry request
```

Якщо refresh token теж invalid:

```text
logout -> clear tokens -> navigate to login
```

Це authentication problem, не authorization.

10. **Приклади**

Authentication examples:

- користувач ввів password;
- app отримав OAuth code;
- backend видав JWT;
- app refresh-нув access token.

Authorization examples:

- user може бачити тільки свої orders;
- moderator може блокувати comments;
- admin може видаляти users;
- free user не має доступу до premium feature.

11. **Практичне правило**

- Authentication завжди перед authorization.
- Access token підтверджує identity/session.
- Permissions/roles визначають доступ.
- `401` — треба login/refresh token.
- `403` — authenticated, але доступ заборонено.
- UI може ховати недоступні дії, але backend має перевіряти права.
- Не зберігати permissions як єдине джерело правди тільки на клієнті.

**Коротко:** authentication підтверджує особу користувача, authorization перевіряє його права. В Android app ми зберігаємо й додаємо токен до запитів, але остаточна перевірка доступу має бути на backend.

</details>
<details>
<summary>157. Які інструменти використовуються для комунікації з бекендом?</summary>

#### Kotlin

Для комунікації з бекендом в Android/Kotlin найчастіше використовують HTTP client, serialization, authentication, error handling, caching і інструменти для realtime-звʼязку. Конкретний стек залежить від API: REST, GraphQL, WebSocket або gRPC.

1. **Retrofit**

Retrofit — стандартний вибір для REST API:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(@Path("id") id: String): UserDto
}
```

Він добре працює з coroutines, OkHttp і JSON converters.

2. **OkHttp**

OkHttp — HTTP client під Retrofit або напряму:

```kotlin
val client = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenProvider))
    .build()
```

Через interceptors додають headers, auth token, logging, retry, cache або request tracing.

3. **Serialization**

Для JSON використовують:

- Kotlin Serialization;
- Moshi;
- Gson.

DTO краще відділяти від domain models:

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
```

4. **Ktor Client**

Ktor Client часто використовують у Kotlin Multiplatform:

```kotlin
val user: UserDto = client.get("/users/$id").body()
```

Він підходить, якщо треба спільний networking code для Android/iOS.

5. **GraphQL**

Для GraphQL в Android часто використовують Apollo Kotlin:

```kotlin
val response = apolloClient.query(UserQuery(id)).execute()
```

GraphQL корисний, коли клієнт хоче явно контролювати форму даних, але додає complexity: schema, codegen, caching, query management.

6. **WebSocket**

WebSocket потрібен для realtime:

- chat;
- live updates;
- multiplayer;
- trading/market data;
- notifications у реальному часі.

В Android можна використовувати OkHttp WebSocket або Ktor WebSockets.

7. **gRPC**

gRPC використовують для contract-first API через protobuf. Він швидкий і strict, але складніший для Android setup, ніж REST. Частіше зустрічається в backend-heavy або internal systems.

8. **Authentication**

Типові механізми:

- Bearer token;
- refresh token;
- OAuth/OpenID Connect;
- session cookies;
- API keys.

Auth зазвичай реалізують через OkHttp interceptor + authenticator:

```kotlin
class AuthInterceptor : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val request = chain.request().newBuilder()
            .addHeader("Authorization", "Bearer $token")
            .build()
        return chain.proceed(request)
    }
}
```

9. **Error handling**

Network layer має перетворювати raw errors у зрозумілу модель:

```kotlin
sealed interface ApiError {
    data object Network : ApiError
    data object Unauthorized : ApiError
    data class Server(val code: Int) : ApiError
}
```

Не варто прокидувати сирий `HttpException` напряму в UI.

10. **Caching і offline**

Для cache/offline-first часто використовують:

- OkHttp cache;
- Room;
- DataStore для settings/tokens;
- Paging 3 + RemoteMediator;
- repository як coordinator local/remote.

11. **Практичне правило**

- REST — Retrofit + OkHttp.
- KMM — Ktor Client.
- GraphQL — Apollo Kotlin.
- Realtime — WebSocket.
- Strong contract/binary — gRPC.
- JSON — Kotlin Serialization або Moshi.
- Cache/offline — Room + repository.

**Коротко:** для backend-комунікації в Android використовують Retrofit/OkHttp для REST, Ktor для KMM, Apollo для GraphQL, WebSocket для realtime, gRPC для strict contract API, JSON serialization для DTO, interceptors для auth/logging і Room/Paging/Repository для cache та offline-first.

</details>
<details>
<summary>158. Що таке Retrofit?</summary>

#### Kotlin

Retrofit — це type-safe HTTP client для Android/JVM, який дозволяє описувати REST API як Kotlin interface з анотаціями. Він працює поверх OkHttp і бере на себе створення HTTP-запитів, обробку response body через converters і інтеграцію з coroutines.

1. **Базова ідея**

Замість ручного створення HTTP request:

```kotlin
val request = Request.Builder()
    .url("https://api.example.com/users/1")
    .build()
```

у Retrofit описують API contract:

```kotlin
interface UserApi {
    @GET("users/{id}")
    suspend fun getUser(
        @Path("id") id: String
    ): UserDto
}
```

Retrofit сам згенерує implementation цього interface.

2. **Створення Retrofit**

```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://api.example.com/")
    .client(okHttpClient)
    .addConverterFactory(json.asConverterFactory("application/json".toMediaType()))
    .build()

val userApi = retrofit.create(UserApi::class.java)
```

`baseUrl` має закінчуватись `/`.

3. **HTTP methods**

Retrofit підтримує HTTP анотації:

```kotlin
interface UserApi {
    @GET("users")
    suspend fun getUsers(): List<UserDto>

    @POST("users")
    suspend fun createUser(
        @Body request: CreateUserRequest
    ): UserDto

    @PUT("users/{id}")
    suspend fun updateUser(
        @Path("id") id: String,
        @Body request: UpdateUserRequest
    ): UserDto

    @DELETE("users/{id}")
    suspend fun deleteUser(
        @Path("id") id: String
    )
}
```

4. **Path і Query parameters**

`@Path` підставляє значення в URL:

```kotlin
@GET("users/{id}")
suspend fun getUser(@Path("id") id: String): UserDto
```

`@Query` додає query parameter:

```kotlin
@GET("users")
suspend fun searchUsers(
    @Query("query") query: String,
    @Query("page") page: Int
): List<UserDto>
```

Запит:

```text
GET /users?query=alex&page=1
```

5. **Headers**

Static header:

```kotlin
@Headers("Accept: application/json")
@GET("users")
suspend fun getUsers(): List<UserDto>
```

Dynamic header:

```kotlin
@GET("users")
suspend fun getUsers(
    @Header("Authorization") token: String
): List<UserDto>
```

Але auth token зазвичай краще додавати через OkHttp interceptor.

6. **Coroutines**

Retrofit добре працює з `suspend`:

```kotlin
suspend fun getUser(id: String): UserDto
```

Виклик:

```kotlin
val user = userApi.getUser("1")
```

Якщо response неуспішний або є network problem, може бути exception:

- `IOException`;
- `HttpException`;
- serialization exception.

7. **Response wrapper**

Можна отримати повний HTTP response:

```kotlin
@GET("users/{id}")
suspend fun getUserResponse(
    @Path("id") id: String
): Response<UserDto>
```

Обробка:

```kotlin
val response = api.getUserResponse(id)

if (response.isSuccessful) {
    val body = response.body()
} else {
    val code = response.code()
}
```

Це корисно, якщо треба вручну обробляти status code, headers або empty body.

8. **Converters**

Retrofit сам не знає, як перетворити JSON у Kotlin object. Для цього потрібен converter:

- Kotlin Serialization converter;
- Moshi converter;
- Gson converter.

Kotlin Serialization:

```kotlin
val json = Json {
    ignoreUnknownKeys = true
}

Retrofit.Builder()
    .addConverterFactory(json.asConverterFactory("application/json".toMediaType()))
```

9. **OkHttp interceptors**

Retrofit використовує OkHttp client, тому можна додавати interceptors:

```kotlin
val okHttpClient = OkHttpClient.Builder()
    .addInterceptor(AuthInterceptor(tokenStorage))
    .addInterceptor(loggingInterceptor)
    .build()
```

Auth:

```kotlin
class AuthInterceptor(
    private val tokenStorage: TokenStorage
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenStorage.getAccessToken()

        val request = chain.request()
            .newBuilder()
            .apply {
                if (token != null) {
                    header("Authorization", "Bearer $token")
                }
            }
            .build()

        return chain.proceed(request)
    }
}
```

10. **Repository layer**

Retrofit interface не має напряму використовуватись у UI:

```kotlin
class UserRepository(
    private val api: UserApi
) {
    suspend fun getUser(id: String): User {
        return api.getUser(id).toDomain()
    }
}
```

ViewModel залежить від use case/repository, а не від Retrofit:

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel()
```

11. **Error mapping**

```kotlin
class UserRepository(
    private val api: UserApi
) {
    suspend fun getUser(id: String): Result<User> {
        return try {
            Result.success(api.getUser(id).toDomain())
        } catch (error: IOException) {
            Result.failure(NetworkException(error))
        } catch (error: HttpException) {
            Result.failure(ApiException(error.code(), error))
        }
    }
}
```

Краще не пропускати технічні exceptions прямо в UI без mapping.

12. **Практичне правило**

- Retrofit — для REST API.
- OkHttp — transport layer під Retrofit.
- API описувати через interfaces.
- DTO не віддавати напряму в UI.
- Auth/logging/retry — через OkHttp interceptors.
- JSON parsing — через converter.
- Errors мапити в domain/UI-friendly result.
- Для GraphQL краще Apollo, не Retrofit.

**Коротко:** Retrofit — це бібліотека, яка перетворює annotated Kotlin interface у HTTP client. В Android її зазвичай використовують разом з OkHttp, Kotlin Serialization/Moshi, interceptors і Repository layer.

</details>
<details>
<summary>159. Що таке Apollo GraphQL?</summary>

#### Kotlin

Apollo GraphQL, точніше Apollo Kotlin, — це GraphQL client для Kotlin/Android. Він генерує type-safe Kotlin-код із GraphQL schema та `.graphql` queries, дозволяє виконувати queries, mutations, subscriptions, працювати з cache і мінімізує ручний parsing JSON.

1. **Що таке GraphQL**

GraphQL — це API-підхід, де client сам описує, які поля йому потрібні.

REST:

```text
GET /users/1
GET /users/1/posts
GET /users/1/followers
```

GraphQL:

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

Client отримує саме ті поля, які описав у query.

2. **Apollo Kotlin**

Apollo Kotlin бере `.graphql` файли:

```graphql
query GetUser($id: ID!) {
  user(id: $id) {
    id
    name
    avatarUrl
  }
}
```

і генерує Kotlin classes:

```kotlin
GetUserQuery
GetUserQuery.Data
GetUserQuery.User
```

Це дає type safety на етапі компіляції.

3. **Створення ApolloClient**

```kotlin
val apolloClient = ApolloClient.Builder()
    .serverUrl("https://api.example.com/graphql")
    .build()
```

З auth header:

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

4. **Query**

```kotlin
class UserRepository(
    private val apolloClient: ApolloClient
) {
    suspend fun getUser(id: String): User {
        val response = apolloClient
            .query(GetUserQuery(id))
            .execute()

        val user = response.data?.user
            ?: throw IllegalStateException("User not found")

        return user.toDomain()
    }
}
```

Query використовується для читання даних.

5. **Mutation**

Mutation використовується для зміни даних:

```graphql
mutation UpdateUserName($id: ID!, $name: String!) {
  updateUserName(id: $id, name: $name) {
    id
    name
  }
}
```

Kotlin:

```kotlin
val response = apolloClient
    .mutation(UpdateUserNameMutation(id, name))
    .execute()
```

6. **Subscription**

Subscription використовується для realtime updates через WebSocket:

```graphql
subscription OnMessageAdded($chatId: ID!) {
  messageAdded(chatId: $chatId) {
    id
    text
    senderName
  }
}
```

Kotlin:

```kotlin
apolloClient
    .subscription(OnMessageAddedSubscription(chatId))
    .toFlow()
    .collect { response ->
        val message = response.data?.messageAdded
        // handle message
    }
```

7. **Apollo vs Retrofit**

Retrofit:

- REST API;
- endpoint-based;
- DTO пишемо самі;
- response shape визначає backend endpoint.

Apollo:

- GraphQL API;
- query-based;
- models генеруються з schema/query;
- response shape визначає client query.

Якщо backend REST — використовувати Retrofit.  
Якщо backend GraphQL — використовувати Apollo Kotlin.

8. **Type safety**

Якщо query просить:

```graphql
user {
  id
  name
}
```

у generated model будуть тільки ці поля. Якщо backend schema змінилася і query стала невалідною, build може впасти на етапі codegen/compile, а не в runtime.

Це одна з головних переваг Apollo.

9. **Cache**

Apollo підтримує normalized cache. Це корисно, коли:

- одна й та сама entity приходить із різних queries;
- треба offline/cache behavior;
- треба уникати зайвих network запитів;
- треба оновлювати UI після mutation.

Концептуально:

```text
User(id=1) зберігається як entity
різні queries можуть посилатися на неї
```

Cache треба проєктувати обережно, особливо для складних offline сценаріїв.

10. **Error handling**

GraphQL response може мати:

- `data`;
- `errors`;
- network exception;
- partial data with errors.

Приклад:

```kotlin
val response = apolloClient
    .query(GetUserQuery(id))
    .execute()

if (response.hasErrors()) {
    val errors = response.errors
    // map GraphQL errors
}

val data = response.data
```

Важливо: GraphQL error не завжди означає, що HTTP status буде 4xx/5xx.

11. **Коли Apollo GraphQL доречний**

Apollo Kotlin доречний, якщо:

- backend використовує GraphQL;
- потрібні type-safe queries;
- client має контролювати shape response;
- є багато різних screen-specific data needs;
- потрібні subscriptions;
- важлива інтеграція зі schema/codegen.

Не доречний, якщо backend звичайний REST і немає GraphQL endpoint.

12. **Практичне правило**

- `.graphql` файли зберігати поруч із feature/data layer.
- Generated models не тягнути напряму в UI без mapping.
- GraphQL errors мапити в domain-level errors.
- Auth додавати через interceptor.
- Для REST — Retrofit, для GraphQL — Apollo.
- Cache strategy узгоджувати з backend schema і normalized IDs.

**Коротко:** Apollo Kotlin — це type-safe GraphQL client для Android/Kotlin. Він генерує Kotlin-код із GraphQL schema і queries, підтримує queries, mutations, subscriptions, cache і добре підходить для проєктів, де backend API побудований на GraphQL.

</details>
<details>
<summary>160. Для чого використовується Firebase?</summary>

#### Kotlin

Firebase — це платформа від Google для швидкої розробки mobile/web застосунків. В Android її використовують для authentication, push notifications, analytics, crash reporting, remote config, realtime database, Firestore, cloud functions, app distribution, performance monitoring та інших backend-like сервісів без повного власного backend-а.

1. **Firebase Authentication**

Firebase Auth використовується для login/signup:

- email/password;
- phone auth;
- Google Sign-In;
- Apple Sign-In;
- anonymous auth;
- custom token auth.

Приклад:

```kotlin
class FirebaseAuthRepository(
    private val firebaseAuth: FirebaseAuth
) {
    suspend fun signIn(
        email: String,
        password: String
    ): FirebaseUser {
        val result = firebaseAuth
            .signInWithEmailAndPassword(email, password)
            .await()

        return result.user ?: error("User is null")
    }
}
```

Firebase Auth зручно використовувати, коли не хочеться самостійно будувати повний auth backend.

2. **Firebase Cloud Messaging**

FCM використовується для push notifications.

```kotlin
class AppFirebaseMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        val title = message.notification?.title
        val body = message.notification?.body

        // show notification
    }

    override fun onNewToken(token: String) {
        // send token to backend
    }
}
```

FCM token треба відправляти на backend, щоб backend міг надсилати push конкретному device/user.

3. **Firebase Analytics**

Firebase Analytics використовується для трекінгу подій:

```kotlin
class FirebaseAnalyticsTracker(
    private val analytics: FirebaseAnalytics
) : AnalyticsTracker {
    override fun track(event: AnalyticsEvent) {
        analytics.logEvent(event.name, event.toBundle())
    }
}
```

Приклади events:

- screen opened;
- login clicked;
- purchase completed;
- onboarding finished.

Важливо не логувати sensitive data.

4. **Crashlytics**

Firebase Crashlytics використовується для crash reporting:

```kotlin
FirebaseCrashlytics.getInstance()
    .recordException(error)
```

Можна додавати custom keys:

```kotlin
FirebaseCrashlytics.getInstance()
    .setCustomKey("user_type", "premium")
```

Не треба записувати туди токени, email-и або персональні дані без потреби.

5. **Remote Config**

Remote Config дозволяє змінювати конфігурацію без нового релізу app.

Приклади:

- feature flags;
- A/B test values;
- мінімальна підтримувана версія;
- тексти/ліміти;
- rollout configuration.

```kotlin
val remoteConfig = FirebaseRemoteConfig.getInstance()

val isNewCheckoutEnabled = remoteConfig.getBoolean("new_checkout_enabled")
```

Remote Config не має бути єдиним security mechanism. Для доступу до premium/admin features backend все одно має перевіряти права.

6. **Cloud Firestore**

Firestore — document database з realtime updates.

```kotlin
data class UserDocument(
    val name: String = "",
    val email: String = ""
)
```

Читання:

```kotlin
val snapshot = firestore
    .collection("users")
    .document(userId)
    .get()
    .await()

val user = snapshot.toObject(UserDocument::class.java)
```

Firestore добре підходить для швидких MVP, realtime data і простих backend-less сценаріїв.

7. **Realtime Database**

Realtime Database — старіша Firebase database, орієнтована на realtime JSON tree.

Підходить для:

- простих realtime updates;
- presence;
- live counters;
- legacy Firebase apps.

Для більшості нових складніших структур часто обирають Firestore.

8. **Cloud Functions**

Cloud Functions дозволяє запускати backend logic у відповідь на events:

- auth user created;
- Firestore document changed;
- HTTP request;
- scheduled job;
- FCM-related logic.

Наприклад:

```text
User created -> Cloud Function -> create profile document
```

Це корисно, коли client не має виконувати trusted server-side logic.

9. **Performance Monitoring**

Firebase Performance Monitoring допомагає бачити:

- app startup time;
- network request duration;
- slow screens;
- custom traces.

Приклад custom trace:

```kotlin
val trace = Firebase.performance.newTrace("load_profile")
trace.start()

repository.loadProfile()

trace.stop()
```

10. **App Distribution**

Firebase App Distribution використовують для розповсюдження test builds QA-команді або beta users.

Це зручно для:

- internal testing;
- pre-release builds;
- швидкого sharing APK/AAB;
- release candidate validation.

11. **Коли Firebase доречний**

Firebase доречний, якщо потрібно швидко отримати:

- auth;
- push notifications;
- analytics;
- crash reporting;
- remote config;
- realtime database;
- beta distribution;
- performance monitoring.

Але для складної domain logic, складних транзакцій, strict backend ownership або enterprise security часто потрібен власний backend.

12. **Практичне правило**

- Firebase Auth — для authentication.
- FCM — для push notifications.
- Analytics — для product events.
- Crashlytics — для crashes/non-fatal errors.
- Remote Config — для feature flags/config.
- Firestore/Realtime DB — для realtime data.
- Cloud Functions — для server-side trusted logic.
- Sensitive logic і permissions не покладати тільки на client.
- Firebase SDK краще ховати за interfaces/repositories, щоб не привʼязувати весь app до SDK.

**Коротко:** Firebase використовується як набір backend/mobile services для Android: auth, push, analytics, crashes, remote config, realtime data, functions і distribution. Він прискорює розробку, але його треба інтегрувати через архітектурні abstraction-и й не переносити security-critical logic у клієнт.

</details>
<details>
<summary>161. Що таке Android Keystore?</summary>

#### Kotlin

Android Keystore — це системний механізм для безпечного створення, зберігання й використання криптографічних ключів на Android-пристрої. Головна ідея: ключі можна використовувати для шифрування, дешифрування або підпису, але сам key material не має напряму покидати захищене сховище.

1. **Для чого потрібен Android Keystore**

Keystore використовують для:

- зберігання encryption keys;
- шифрування sensitive data;
- захисту access/refresh tokens;
- підпису даних;
- інтеграції з biometric authentication;
- hardware-backed security, якщо пристрій підтримує.

Важливо: Keystore зазвичай зберігає ключі, а не великі дані. Дані шифрують ключем, а encrypted payload зберігають у DataStore, SharedPreferences або файлі.

2. **Чого не треба робити**

Погано:

```kotlin
const val SECRET_KEY = "hardcoded-secret"
```

Такий ключ можна витягнути з APK.

Краще створити ключ у Android Keystore:

```kotlin
val keyGenerator = KeyGenerator.getInstance(
    KeyProperties.KEY_ALGORITHM_AES,
    "AndroidKeyStore"
)
```

3. **Створення AES ключа**

```kotlin
fun createSecretKey(alias: String): SecretKey {
    val keyGenerator = KeyGenerator.getInstance(
        KeyProperties.KEY_ALGORITHM_AES,
        "AndroidKeyStore"
    )

    val keyGenParameterSpec = KeyGenParameterSpec.Builder(
        alias,
        KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT
    )
        .setBlockModes(KeyProperties.BLOCK_MODE_GCM)
        .setEncryptionPaddings(KeyProperties.ENCRYPTION_PADDING_NONE)
        .setKeySize(256)
        .build()

    keyGenerator.init(keyGenParameterSpec)
    return keyGenerator.generateKey()
}
```

Цей ключ створюється всередині Android Keystore.

4. **Отримання ключа**

```kotlin
fun getSecretKey(alias: String): SecretKey {
    val keyStore = KeyStore.getInstance("AndroidKeyStore").apply {
        load(null)
    }

    return keyStore.getKey(alias, null) as SecretKey
}
```

Зазвичай flow такий:

```text
якщо ключ існує -> взяти
якщо не існує -> створити
```

5. **Шифрування**

```kotlin
fun encrypt(
    plainText: ByteArray,
    secretKey: SecretKey
): Pair<ByteArray, ByteArray> {
    val cipher = Cipher.getInstance("AES/GCM/NoPadding")
    cipher.init(Cipher.ENCRYPT_MODE, secretKey)

    val cipherText = cipher.doFinal(plainText)
    val iv = cipher.iv

    return cipherText to iv
}
```

Для AES-GCM треба зберігати `iv` разом із encrypted data. `iv` не є секретом, але потрібен для дешифрування.

6. **Дешифрування**

```kotlin
fun decrypt(
    cipherText: ByteArray,
    iv: ByteArray,
    secretKey: SecretKey
): ByteArray {
    val cipher = Cipher.getInstance("AES/GCM/NoPadding")
    val spec = GCMParameterSpec(128, iv)

    cipher.init(Cipher.DECRYPT_MODE, secretKey, spec)
    return cipher.doFinal(cipherText)
}
```

Якщо `iv`, key або encrypted data неправильні, дешифрування впаде exception-ом.

7. **EncryptedSharedPreferences**

На практиці часто використовують готовий Jetpack Security wrapper:

```kotlin
val masterKey = MasterKey.Builder(context)
    .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
    .build()

val preferences = EncryptedSharedPreferences.create(
    context,
    "secure_prefs",
    masterKey,
    EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
    EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)
```

Збереження token:

```kotlin
preferences.edit()
    .putString("access_token", token)
    .apply()
```

Це простіше, ніж вручну працювати з `Cipher`.

8. **Biometric-bound keys**

Ключ можна привʼязати до user authentication:

```kotlin
KeyGenParameterSpec.Builder(
    alias,
    KeyProperties.PURPOSE_ENCRYPT or KeyProperties.PURPOSE_DECRYPT
)
    .setUserAuthenticationRequired(true)
    .setUserAuthenticationParameters(
        30,
        KeyProperties.AUTH_BIOMETRIC_STRONG or KeyProperties.AUTH_DEVICE_CREDENTIAL
    )
```

Тоді для використання ключа може знадобитися biometrics або device credential.

9. **Hardware-backed keys**

На деяких пристроях ключі можуть бути hardware-backed:

- TEE;
- StrongBox;
- secure hardware.

Але не всі пристрої це підтримують. Код має бути готовий до fallback.

StrongBox:

```kotlin
.setIsStrongBoxBacked(true)
```

може кинути exception, якщо пристрій не підтримує StrongBox.

10. **Обмеження Android Keystore**

Keystore не вирішує всі security problems:

- root device може зменшити гарантії;
- malware/accessibility attacks можливі;
- token у runtime памʼяті може бути перехоплений на скомпрометованому пристрої;
- server secrets не можна безпечно ховати в app;
- backup/restore може створити edge cases для encrypted data.

Keystore — сильний захист, але не абсолютна безпека.

11. **Що зберігати через Keystore**

Добре:

- ключ для шифрування local tokens;
- private key для signing;
- symmetric key для local encrypted storage;
- credentials, які мають бути захищені на device.

Не варто:

- hardcoded API secrets;
- великі blobs напряму;
- server private keys;
- admin tokens;
- дані, які краще взагалі не зберігати локально.

12. **Практичне правило**

- Keystore зберігає ключі, не бізнес-дані.
- Дані шифрувати ключем і зберігати encrypted payload окремо.
- Для типових token use cases можна використовувати EncryptedSharedPreferences або encrypted DataStore wrapper.
- Не класти server secrets у APK.
- Обробляти key invalidation, backup/restore і biometric changes.
- Для high-risk flows розглядати biometric-bound keys.

**Коротко:** Android Keystore — це системне захищене сховище криптографічних ключів. Його використовують, щоб безпечно шифрувати локальні sensitive дані, наприклад токени, але він не замінює backend security і не робить клієнтський додаток повністю trusted environment.

</details>
<details>
<summary>162. Які є dispatchers у Kotlin Coroutines і для чого вони використовуються?</summary>

#### Kotlin

Dispatcher у Kotlin Coroutines визначає, на якому потоці або пулі потоків буде виконуватися coroutine. Найчастіше використовують `Dispatchers.Main`, `Dispatchers.IO`, `Dispatchers.Default` і в тестах `TestDispatcher`. Правильний dispatcher важливий, щоб не блокувати UI thread і не робити важку роботу не там.

1. **Dispatchers.Main**

`Dispatchers.Main` — головний Android thread.

Використовується для:

- оновлення UI state;
- взаємодії з View;
- Compose state updates;
- LiveData/StateFlow updates, які впливають на UI;
- запуску coroutines у `viewModelScope`.

Приклад:

```kotlin
viewModelScope.launch {
    _state.value = ProfileUiState(isLoading = true)
}
```

`viewModelScope.launch {}` за замовчуванням працює на Main dispatcher.

2. **Що не можна робити на Main**

Не можна виконувати blocking або важку роботу:

```kotlin
viewModelScope.launch {
    Thread.sleep(5_000) // погано
    val bytes = file.readBytes() // потенційно погано
}
```

Це може спричинити лаги або ANR.

Для IO треба перейти на `Dispatchers.IO`.

3. **Dispatchers.IO**

`Dispatchers.IO` призначений для blocking IO operations:

- network;
- file read/write;
- database operations, якщо API blocking;
- SharedPreferences/DataStore access;
- content resolver;
- sockets.

Приклад:

```kotlin
suspend fun loadUser(id: String): User {
    return withContext(Dispatchers.IO) {
        api.getUser(id).toDomain()
    }
}
```

Якщо бібліотека вже suspend-friendly і сама перемикає dispatcher, додатковий `withContext(IO)` може бути не потрібен, але для blocking IO він потрібен.

4. **Dispatchers.Default**

`Dispatchers.Default` використовується для CPU-heavy задач:

- сортування великих списків;
- обчислення;
- parsing великих структур;
- diff/calculation;
- compression;
- image processing, якщо це CPU-bound.

Приклад:

```kotlin
suspend fun sortUsers(users: List<User>): List<User> {
    return withContext(Dispatchers.Default) {
        users.sortedBy { it.name }
    }
}
```

`Default` має пул потоків, орієнтований на кількість CPU cores.

5. **Dispatchers.Unconfined**

`Dispatchers.Unconfined` не привʼязаний до конкретного потоку й продовжує виконання там, де resume-иться coroutine.

```kotlin
launch(Dispatchers.Unconfined) {
    // not recommended for regular Android app code
}
```

У production Android-коді його майже ніколи не варто використовувати. Він корисний у специфічних низькорівневих або тестових сценаріях, але легко створює непередбачувану поведінку.

6. **withContext**

`withContext` перемикає dispatcher для конкретного блоку:

```kotlin
suspend fun loadProfile(): UserProfile {
    val dto = withContext(Dispatchers.IO) {
        api.getProfile()
    }

    return withContext(Dispatchers.Default) {
        dto.toDomain()
    }
}
```

Після `withContext` coroutine повертається в попередній context.

7. **Repository і dispatcher**

Погано хардкодити dispatchers всюди без контролю:

```kotlin
class UserRepository {
    suspend fun getUser(): User {
        return withContext(Dispatchers.IO) {
            TODO()
        }
    }
}
```

Краще інжектити dispatcher:

```kotlin
class UserRepository(
    private val ioDispatcher: CoroutineDispatcher,
    private val api: UserApi
) {
    suspend fun getUser(id: String): User {
        return withContext(ioDispatcher) {
            api.getUser(id).toDomain()
        }
    }
}
```

Так простіше тестувати.

8. **DispatcherProvider**

У великих проєктах роблять abstraction:

```kotlin
interface DispatcherProvider {
    val main: CoroutineDispatcher
    val io: CoroutineDispatcher
    val default: CoroutineDispatcher
}

class AppDispatcherProvider : DispatcherProvider {
    override val main = Dispatchers.Main
    override val io = Dispatchers.IO
    override val default = Dispatchers.Default
}
```

У тестах:

```kotlin
class TestDispatcherProvider(
    private val dispatcher: CoroutineDispatcher
) : DispatcherProvider {
    override val main = dispatcher
    override val io = dispatcher
    override val default = dispatcher
}
```

9. **TestDispatcher**

Для unit-тестів coroutines використовують:

```kotlin
val testDispatcher = StandardTestDispatcher()
```

або:

```kotlin
val testDispatcher = UnconfinedTestDispatcher()
```

Це дозволяє контролювати execution і virtual time:

```kotlin
runTest {
    val viewModel = ProfileViewModel(
        dispatcherProvider = TestDispatcherProvider(testDispatcher)
    )

    viewModel.load()
    advanceUntilIdle()
}
```

10. **Main dispatcher у tests**

Якщо код використовує `Dispatchers.Main`, у unit tests треба підмінити Main:

```kotlin
Dispatchers.setMain(testDispatcher)
```

і після тесту:

```kotlin
Dispatchers.resetMain()
```

Часто це ховають у JUnit rule.

11. **Типові помилки**

- робити network/file work на Main;
- робити CPU-heavy sorting/parsing на Main;
- використовувати `Dispatchers.IO` для CPU-heavy задач;
- використовувати `Dispatchers.Default` для blocking IO;
- хардкодити dispatchers так, що тести стають складними;
- використовувати `GlobalScope`;
- використовувати `Unconfined` без чіткої причини.

12. **Практичне правило**

- UI/state update — `Dispatchers.Main`.
- Network/file/database blocking work — `Dispatchers.IO`.
- CPU-heavy work — `Dispatchers.Default`.
- Tests — `TestDispatcher`.
- Dispatcher-и краще інжектити, якщо код треба якісно тестувати.
- Не блокувати Main thread.
- Не використовувати `Unconfined` у звичайному app code.

**Коротко:** dispatcher визначає, де виконується coroutine. `Main` — для UI, `IO` — для input/output, `Default` — для CPU-heavy роботи, test dispatchers — для контрольованих тестів. Правильний dispatcher напряму впливає на responsiveness Android-додатку.

</details>
<details>
<summary>163. Які є способи запуску корутин?</summary>

#### Kotlin

Корутини в Kotlin запускають через coroutine builders: `launch`, `async`, `runBlocking`, а також через lifecycle-aware scopes в Android: `viewModelScope`, `lifecycleScope`, `rememberCoroutineScope`, `LaunchedEffect`. Вибір способу залежить від того, чи потрібен результат, який lifecycle має мати задача і чи можна блокувати потік.

1. **launch**

`launch` запускає coroutine без повернення результату. Повертає `Job`.

```kotlin
val job = viewModelScope.launch {
    repository.sync()
}
```

Підходить для fire-and-forget задач:

- оновити state;
- виконати sync;
- записати analytics;
- запустити side effect;
- виконати операцію без прямого return value.

2. **async**

`async` запускає coroutine, яка повертає результат через `Deferred<T>`.

```kotlin
val deferredUser = viewModelScope.async {
    repository.getUser(userId)
}

val user = deferredUser.await()
```

`async` використовують, коли треба паралельно виконати кілька задач і дочекатися результатів.

3. **async для паралельних запитів**

```kotlin
viewModelScope.launch {
    val userDeferred = async { userRepository.getUser(userId) }
    val postsDeferred = async { postRepository.getPosts(userId) }

    val user = userDeferred.await()
    val posts = postsDeferred.await()

    _state.value = ProfileState.Content(user, posts)
}
```

Так `getUser` і `getPosts` виконуються паралельно в межах structured concurrency.

4. **runBlocking**

`runBlocking` блокує поточний потік, поки coroutine не завершиться.

```kotlin
fun main() = runBlocking {
    val user = repository.getUser("1")
    println(user)
}
```

В Android production UI-коді `runBlocking` майже ніколи не треба використовувати, бо він може заблокувати main thread і спричинити ANR.

Доречні сценарії:

- `main` function;
- tests;
- bridge між blocking і suspend code у non-Android контексті.

5. **viewModelScope**

`viewModelScope` привʼязаний до `ViewModel`.

```kotlin
class ProfileViewModel(
    private val repository: ProfileRepository
) : ViewModel() {

    fun load(userId: String) {
        viewModelScope.launch {
            val profile = repository.getProfile(userId)
            _state.value = ProfileState.Content(profile)
        }
    }
}
```

Корутини автоматично скасовуються, коли `ViewModel` очищається.

6. **lifecycleScope**

`lifecycleScope` привʼязаний до `LifecycleOwner`, наприклад Activity або Fragment.

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

У Fragment для UI collection краще використовувати `viewLifecycleOwner.lifecycleScope`, а не `fragment.lifecycleScope`, щоб не тримати стару View після `onDestroyView`.

7. **rememberCoroutineScope**

У Compose `rememberCoroutineScope` дає scope для запуску coroutine з callback-а:

```kotlin
@Composable
fun SnackbarButton(
    snackbarHostState: SnackbarHostState
) {
    val scope = rememberCoroutineScope()

    Button(
        onClick = {
            scope.launch {
                snackbarHostState.showSnackbar("Saved")
            }
        }
    ) {
        Text("Show snackbar")
    }
}
```

Це підходить для UI-bound suspend operations: snackbar, drawer, scroll animation.

8. **LaunchedEffect**

`LaunchedEffect` запускає coroutine в Compose при вході в composition або зміні key.

```kotlin
@Composable
fun ProfileRoute(
    userId: String,
    viewModel: ProfileViewModel
) {
    LaunchedEffect(userId) {
        viewModel.load(userId)
    }
}
```

Якщо `userId` зміниться, стара coroutine скасується і запуститься нова.

9. **coroutineScope**

`coroutineScope` створює scope всередині suspend-функції й чекає завершення всіх child coroutines.

```kotlin
suspend fun loadProfile(userId: String): ProfileData = coroutineScope {
    val user = async { userRepository.getUser(userId) }
    val posts = async { postRepository.getPosts(userId) }

    ProfileData(
        user = user.await(),
        posts = posts.await()
    )
}
```

Якщо одна child coroutine впаде, весь scope скасується.

10. **supervisorScope**

`supervisorScope` схожий на `coroutineScope`, але failure однієї child coroutine не скасовує автоматично інші.

```kotlin
suspend fun loadDashboard(): DashboardData = supervisorScope {
    val profile = async { profileRepository.getProfile() }
    val recommendations = async { recommendationRepository.getRecommendations() }

    DashboardData(
        profile = runCatching { profile.await() }.getOrNull(),
        recommendations = runCatching { recommendations.await() }.getOrDefault(emptyList())
    )
}
```

Корисно, коли частина даних може не завантажитися, але екран все одно має показати решту.

11. **GlobalScope**

`GlobalScope` запускає coroutine, яка живе майже як process-level task.

Погано:

```kotlin
GlobalScope.launch {
    repository.sync()
}
```

Проблеми:

- не привʼязаний до lifecycle;
- складно скасувати;
- складно тестувати;
- ризик leaks;
- порушує structured concurrency.

У звичайному Android app code `GlobalScope` майже завжди треба уникати.

12. **Практичне правило**

- Потрібна задача без результату — `launch`.
- Потрібен результат — `async` + `await`.
- У ViewModel — `viewModelScope`.
- У Fragment/Activity — `lifecycleScope` або `viewLifecycleOwner.lifecycleScope`.
- У Compose side effect — `LaunchedEffect`.
- У Compose callback — `rememberCoroutineScope`.
- У suspend-функції для child coroutines — `coroutineScope` або `supervisorScope`.
- `runBlocking` не використовувати на main thread.
- `GlobalScope` уникати.

**Коротко:** корутини запускають через builders і lifecycle-aware scopes. Найчастіше в Android це `viewModelScope.launch`, `async` для паралельних результатів, `lifecycleScope` для lifecycle collection і `LaunchedEffect`/`rememberCoroutineScope` у Compose.

</details>
<details>
<summary>164. У чому різниця між launch та async?</summary>

#### Kotlin

`launch` і `async` — це coroutine builders. `launch` запускає coroutine без результату і повертає `Job`. `async` запускає coroutine з результатом і повертає `Deferred<T>`, з якого результат отримують через `await()`. Практично: `launch` — для side effects, `async` — для паралельних обчислень або запитів із результатом.

1. **launch**

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

`launch` повертає `Job`, який дозволяє:

- скасувати coroutine;
- перевірити стан;
- дочекатися завершення через `join()`.

```kotlin
job.cancel()
job.join()
```

Але `launch` не повертає значення з coroutine.

2. **async**

```kotlin
val deferred: Deferred<User> = viewModelScope.async {
    repository.getUser(userId)
}

val user: User = deferred.await()
```

`async` повертає `Deferred<T>`, який є `Job` + майбутній результат.

Результат отримують через:

```kotlin
deferred.await()
```

3. **Головна різниця**

```text
launch -> Job, без результату
async  -> Deferred<T>, з результатом
```

`launch`:

```kotlin
viewModelScope.launch {
    analytics.track(Event.ScreenOpened)
}
```

`async`:

```kotlin
val user = async { userRepository.getUser(id) }
val posts = async { postRepository.getPosts(id) }
```

4. **Коли використовувати launch**

`launch` підходить для:

- оновлення UI state;
- запису в database;
- sync без прямого результату;
- analytics;
- navigation effects;
- collect Flow;
- fire-and-forget у межах lifecycle scope.

Приклад:

```kotlin
fun load(userId: String) {
    viewModelScope.launch {
        _state.value = ProfileState.Loading

        val profile = repository.getProfile(userId)

        _state.value = ProfileState.Content(profile)
    }
}
```

5. **Коли використовувати async**

`async` підходить, коли треба:

- отримати результат;
- запустити кілька незалежних задач паралельно;
- обʼєднати результати;
- прискорити loading, якщо запити не залежать один від одного.

Приклад:

```kotlin
suspend fun loadDashboard(): Dashboard = coroutineScope {
    val userDeferred = async { userRepository.getUser() }
    val statsDeferred = async { statsRepository.getStats() }
    val feedDeferred = async { feedRepository.getFeed() }

    Dashboard(
        user = userDeferred.await(),
        stats = statsDeferred.await(),
        feed = feedDeferred.await()
    )
}
```

6. **async без await — поганий сигнал**

Погано:

```kotlin
viewModelScope.async {
    repository.sync()
}
```

Якщо результат не потрібен, треба `launch`:

```kotlin
viewModelScope.launch {
    repository.sync()
}
```

`async` без `await()` часто означає неправильний builder.

7. **Exception handling у launch**

Exception у `launch` поводиться як uncaught exception у coroutine scope:

```kotlin
viewModelScope.launch {
    throw RuntimeException("Failed")
}
```

Її треба обробити:

```kotlin
viewModelScope.launch {
    try {
        repository.sync()
    } catch (error: IOException) {
        _state.value = State.Error
    }
}
```

8. **Exception handling у async**

Exception з `async` зазвичай проявляється при `await()`:

```kotlin
val deferred = async {
    repository.getUser()
}

try {
    val user = deferred.await()
} catch (error: IOException) {
    // handle error
}
```

Але в structured concurrency failure child coroutine все одно впливає на parent scope.

9. **Structured concurrency**

Правильно запускати `async` всередині `coroutineScope` або lifecycle scope:

```kotlin
suspend fun loadData(): Data = coroutineScope {
    val a = async { loadA() }
    val b = async { loadB() }

    Data(a.await(), b.await())
}
```

Так parent чекає children, а cancellation працює передбачувано.

10. **supervisorScope для незалежних задач**

Якщо одна задача може впасти, але інші мають продовжити:

```kotlin
suspend fun loadHome(): HomeData = supervisorScope {
    val profile = async { profileRepository.getProfile() }
    val recommendations = async { recommendationRepository.getRecommendations() }

    HomeData(
        profile = runCatching { profile.await() }.getOrNull(),
        recommendations = runCatching { recommendations.await() }.getOrDefault(emptyList())
    )
}
```

11. **Практичне правило**

- Немає результату — `launch`.
- Є результат — `async`.
- Потрібно паралельно отримати кілька результатів — `async + await`.
- `async` без `await` майже завжди помилка.
- Exceptions у `launch` обробляти всередині coroutine або через handler.
- Exceptions у `async` обробляти навколо `await`, але памʼятати про parent cancellation.
- Для паралельних suspend-функцій використовувати `coroutineScope`.

**Коротко:** `launch` запускає coroutine для роботи без результату і повертає `Job`; `async` запускає coroutine з результатом і повертає `Deferred<T>`. У Android `launch` частіше використовують у ViewModel для state updates, а `async` — для паралельних незалежних запитів.

</details>
<details>
<summary>165. Як обробляти помилки в корутинах?</summary>

#### Kotlin

Помилки в корутинах треба обробляти з урахуванням structured concurrency: exception у дочірній coroutine зазвичай скасовує parent scope і сусідні child coroutines. Тому важливо розуміти, де саме ловити помилку, який builder використовується (`launch` чи `async`) і чи потрібна ізоляція через `supervisorScope` або `SupervisorJob`.

1. **Базовий try/catch**

Найпростіший варіант — обгорнути suspend-виклик у `try/catch` всередині coroutine:

```kotlin
viewModelScope.launch {
    try {
        val user = repository.getUser(userId)
        _state.value = UserState.Content(user)
    } catch (error: IOException) {
        _state.value = UserState.NetworkError
    } catch (error: Throwable) {
        _state.value = UserState.UnknownError
    }
}
```

Це нормальний підхід для ViewModel, коли помилку треба перетворити в UI state.

2. **Не ловити CancellationException як звичайну помилку**

`CancellationException` означає нормальне скасування coroutine. Її зазвичай треба пробросити далі:

```kotlin
viewModelScope.launch {
    try {
        repository.sync()
    } catch (error: CancellationException) {
        throw error
    } catch (error: Exception) {
        _state.value = SyncState.Error
    }
}
```

Якщо випадково “проковтнути” cancellation, coroutine може поводитися некоректно і продовжити роботу після скасування.

3. **launch кидає exception у parent**

У `launch` необроблена exception завершує coroutine і передається в parent:

```kotlin
viewModelScope.launch {
    repository.sync()
}
```

Якщо `sync()` впаде, ця coroutine завершиться з помилкою. У звичайному `Job` це також може скасувати parent scope і сусідні child coroutines.

4. **async кидає exception при await**

В `async` exception зберігається в `Deferred` і буде кинута при `await()`:

```kotlin
viewModelScope.launch {
    val userDeferred = async {
        repository.getUser(userId)
    }

    try {
        val user = userDeferred.await()
        _state.value = UserState.Content(user)
    } catch (error: IOException) {
        _state.value = UserState.NetworkError
    }
}
```

Але це не означає, що `async` повністю ізолює помилку. У structured concurrency failed child все одно впливає на parent, якщо не використовується supervisor-поведінка.

5. **CoroutineExceptionHandler**

`CoroutineExceptionHandler` — це fallback handler для необроблених exceptions у root coroutine:

```kotlin
val handler = CoroutineExceptionHandler { _, error ->
    logError(error)
}

CoroutineScope(SupervisorJob() + Dispatchers.Main + handler).launch {
    repository.sync()
}
```

Важливо: він не замінює `try/catch` для бізнес-логіки. Для UI state краще явно ловити помилки там, де є контекст, що з ними робити.

6. **CoroutineExceptionHandler не ловить async до await**

```kotlin
val handler = CoroutineExceptionHandler { _, error ->
    println("Caught: $error")
}

CoroutineScope(Dispatchers.Default + handler).launch {
    val deferred = async {
        error("Failed")
    }

    deferred.await()
}
```

Помилка з `async` стане видимою на `await()`. Якщо потрібна обробка результату, ловити треба навколо `await()`.

7. **supervisorScope для незалежних задач**

Якщо кілька паралельних задач незалежні і падіння однієї не повинно скасувати інші, використовують `supervisorScope`:

```kotlin
suspend fun loadDashboard(): DashboardState = supervisorScope {
    val profile = async { profileRepository.getProfile() }
    val banners = async { bannerRepository.getBanners() }

    DashboardState(
        profile = runCatching { profile.await() }.getOrNull(),
        banners = runCatching { banners.await() }.getOrDefault(emptyList())
    )
}
```

Тут падіння `profile` не скасовує `banners`.

8. **Result як частина контракту repository**

Іноді зручно, щоб repository повертав доменний результат замість сирої exception:

```kotlin
sealed interface UserResult {
    data class Success(val user: User) : UserResult
    data object NetworkError : UserResult
    data object UnknownError : UserResult
}

suspend fun getUser(userId: String): UserResult {
    return try {
        UserResult.Success(api.getUser(userId))
    } catch (error: IOException) {
        UserResult.NetworkError
    } catch (error: CancellationException) {
        throw error
    } catch (error: Exception) {
        UserResult.UnknownError
    }
}
```

Так ViewModel працює з очікуваним доменним результатом, а не з transport-level exceptions.

9. **runCatching і cancellation**

`runCatching` ловить `Throwable`, тому з ним треба бути обережним:

```kotlin
suspend fun loadUser(userId: String): Result<User> {
    return try {
        Result.success(repository.getUser(userId))
    } catch (error: CancellationException) {
        throw error
    } catch (error: Exception) {
        Result.failure(error)
    }
}
```

Для production-коду це часто безпечніше, ніж сліпо писати `runCatching { ... }`.

10. **Практичне правило**

- Для UI state — `try/catch` всередині `viewModelScope.launch`.
- Для `async` — ловити помилки навколо `await()`.
- `CancellationException` не проковтувати.
- Для незалежних паралельних задач — `supervisorScope` або `SupervisorJob`.
- `CoroutineExceptionHandler` використовувати як fallback logging, а не як основний control flow.
- На рівні repository часто краще повертати доменний result/error model.

**Коротко:** помилки в корутинах обробляють локальним `try/catch`, навколо `await()` для `async`, через `supervisorScope` для ізоляції паралельних задач і з обовʼязковим пробросом `CancellationException`.

</details>
<details>
<summary>166. Як дочекатися результату декількох паралельних запитів?</summary>

#### Kotlin

У Kotlin для паралельних запитів зазвичай використовують structured concurrency: `coroutineScope`, `async` і `await`. Це дозволяє запустити кілька suspend-операцій паралельно і дочекатися їх результатів.

1. **async / await**

```kotlin
suspend fun loadScreen(): ScreenData = coroutineScope {
    val userDeferred = async { api.getUser() }
    val postsDeferred = async { api.getPosts() }
    val settingsDeferred = async { api.getSettings() }

    ScreenData(
        user = userDeferred.await(),
        posts = postsDeferred.await(),
        settings = settingsDeferred.await()
    )
}
```

Усі три запити стартують паралельно, а `await()` чекає результат.

2. **Чому потрібен coroutineScope**

`coroutineScope` створює structured concurrency boundary:

```kotlin
suspend fun load() = coroutineScope {
    async { requestA() }
    async { requestB() }
}
```

Якщо одна child coroutine падає, scope скасовує інші. Це безпечніше, ніж запускати запити в `GlobalScope`.

3. **awaitAll**

Якщо запитів багато й вони одного типу:

```kotlin
val users = ids.map { id ->
    async { api.getUser(id) }
}.awaitAll()
```

`awaitAll()` чекає всі deferred і повертає список результатів.

4. **Обробка помилок**

За замовчуванням помилка в одному `async` скасовує весь `coroutineScope`:

```kotlin
suspend fun loadAll() = coroutineScope {
    val a = async { requestA() }
    val b = async { requestB() }
    a.await() to b.await()
}
```

Це правильно, якщо екран не може працювати без усіх даних.

5. **supervisorScope**

Якщо частковий результат допустимий, можна використати `supervisorScope`:

```kotlin
suspend fun loadPartial(): PartialData = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(
        user = user.await().getOrNull(),
        posts = posts.await().getOrDefault(emptyList())
    )
}
```

Тут помилка одного запиту не скасовує інший.

6. **Timeout**

```kotlin
val data = withTimeout(10_000) {
    loadScreen()
}
```

Timeout потрібен, щоб не чекати нескінченно повільний backend.

7. **Обмеження паралельності**

Якщо треба зробити багато запитів, не варто запускати сотні одночасно:

```kotlin
val dispatcher = Dispatchers.IO.limitedParallelism(5)

val results = ids.map { id ->
    async(dispatcher) { api.getUser(id) }
}.awaitAll()
```

Це захищає app і backend від надмірного навантаження.

8. **У ViewModel**

```kotlin
fun load() {
    viewModelScope.launch {
        _state.value = State.Loading
        runCatching { repository.loadScreen() }
            .onSuccess { _state.value = State.Success(it) }
            .onFailure { _state.value = State.Error }
    }
}
```

ViewModel запускає одну coroutine, а repository/use case вже вирішує, які запити паралелити.

9. **Типові помилки**

- використовувати `GlobalScope`;
- робити запити послідовно, хоча вони незалежні;
- запускати сотні `async` без limit;
- не обробляти cancellation;
- ловити `CancellationException` як звичайну помилку;
- використовувати `async`, коли результат не потрібен — тоді краще `launch`.

10. **Практичне правило**

Незалежні запити — `async` у `coroutineScope`. Якщо потрібні всі результати — звичайний `coroutineScope`. Якщо можна показати часткові дані — `supervisorScope` + `runCatching`. Для великої кількості запитів — обмежити паралельність.

**Коротко:** кілька паралельних запитів у Kotlin роблять через `coroutineScope { async { ... } }` і `await()`/`awaitAll()`. Звичайний `coroutineScope` скасовує все при помилці одного запиту, `supervisorScope` дозволяє часткові результати. Для production потрібні timeout, error handling і limit паралельності.

</details>
<details>
<summary>167. Для чого були придумані Fragment?</summary>

#### Kotlin

`Fragment` були придумані як reusable частини UI та поведінки всередині `Activity`. Основна ідея: не робити одну велику `Activity` на весь екран, а розбивати інтерфейс на менші незалежні блоки, які мають власний lifecycle, layout, state і можуть комбінуватися по-різному залежно від екрана, планшета, телефону або навігаційного сценарію.

1. **Проблема великих Activity**

До активного використання Fragment логіка екранів часто концентрувалася в `Activity`:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // navigation
        // list rendering
        // details rendering
        // click handling
        // state restoration
        // lifecycle logic
    }
}
```

Такі `Activity` швидко ставали занадто великими: багато UI-логіки, багато callbacks, складна навігація і важке тестування.

2. **Fragment як частина екрана**

`Fragment` дозволяє винести частину UI в окремий компонент:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        // list UI logic
    }
}
```

`Activity` при цьому може бути контейнером:

```kotlin
class MainActivity : AppCompatActivity(R.layout.activity_main)
```

Тобто `Activity` відповідає за high-level container, а `Fragment` — за конкретний екран або частину екрана.

3. **Повторне використання UI**

Один і той самий Fragment можна використати в різних місцях:

```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.container, UserDetailsFragment.newInstance(userId))
    .commit()
```

Наприклад:

- на телефоні `UserListFragment` і `UserDetailsFragment` можуть відкриватися як окремі екрани;
- на планшеті вони можуть бути показані одночасно поруч;
- у master-detail layout один Fragment показує список, інший — деталі.

4. **Адаптація під різні розміри екрана**

Fragment були особливо корисні для tablet UI:

```text
Phone:
Activity
 └── UserListFragment

Tablet:
Activity
 ├── UserListFragment
 └── UserDetailsFragment
```

Це дозволяло будувати гнучкі layout-и без дублювання всієї `Activity`.

5. **Власний lifecycle**

Fragment має власний lifecycle:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
}

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    return inflater.inflate(R.layout.fragment_user_list, container, false)
}

override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
}

override fun onDestroyView() {
    super.onDestroyView()
}
```

Це важливо, бо lifecycle Fragment і lifecycle його View — не одне й те саме.

6. **Навігація між екранами**

Fragment стали основою для navigation всередині однієї `Activity`:

```kotlin
findNavController().navigate(
    UserListFragmentDirections.openUserDetails(userId)
)
```

У сучасному Android XML/View-підході часто використовується single-activity architecture:

```text
MainActivity
 └── NavHostFragment
      ├── HomeFragment
      ├── DetailsFragment
      └── SettingsFragment
```

Тут `Activity` одна, а екрани представлені Fragment-ами.

7. **Back stack**

FragmentTransaction може додаватися в back stack:

```kotlin
supportFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Це дозволяє повертатися назад між Fragment-екранами без створення нової `Activity` для кожного переходу.

8. **Інкапсуляція логіки екрана**

Fragment допомагає тримати UI-логіку ближче до конкретного layout:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        observeState()
        setupClicks()
    }
}
```

Це краще, ніж зберігати всю логіку профілю, списку, налаштувань і деталей в одній `Activity`.

9. **Що Fragment не вирішують автоматично**

Fragment не роблять архітектуру хорошою самі по собі. Вони також додають складність:

- окремий lifecycle Fragment;
- окремий lifecycle View;
- back stack edge cases;
- child fragments;
- state restoration;
- ризик memory leaks через binding після `onDestroyView()`.

Наприклад, binding треба очищати:

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = requireNotNull(_binding)

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

10. **Compose-контекст**

У Jetpack Compose роль Fragment стала менш центральною. Compose дозволяє будувати екрани як composable-функції:

```kotlin
@Composable
fun ProfileScreen(
    state: ProfileState,
    onAction: (ProfileAction) -> Unit
) {
    // UI
}
```

Але Fragment досі актуальні у проектах з XML/View, legacy-кодом, Navigation Component, mixed View + Compose підходом або SDK/feature-модулями, які очікують Fragment API.

11. **Практичне правило**

- `Activity` — host/container для application-level або navigation-level логіки.
- `Fragment` — reusable screen або частина screen.
- Для XML/View navigation Fragment досі нормальний стандарт.
- У Compose-first застосунках часто можна обходитися без Fragment.
- Fragment корисні, але потребують уважного lifecycle management.

**Коротко:** Fragment придумали, щоб розбивати UI на reusable lifecycle-aware частини, комбінувати їх у різних layout-ах, підтримувати navigation/back stack всередині Activity і не перетворювати `Activity` на моноліт.

</details>
<details>
<summary>168. Які проблеми вони вирішують?</summary>

#### Kotlin

Fragment вирішують проблеми, які зʼявляються, коли UI в Android-додатку росте: занадто великі `Activity`, дублювання екранів, складна навігація, різні layouts для phone/tablet і потреба повторно використовувати частини UI.

1. **Занадто великі Activity**

Без Fragment одна `Activity` може містити всю логіку екранів:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // home, details, settings, navigation, state
    }
}
```

Fragment дозволяють розділити UI на окремі частини:

```kotlin
class HomeFragment : Fragment(R.layout.fragment_home)
class DetailsFragment : Fragment(R.layout.fragment_details)
class SettingsFragment : Fragment(R.layout.fragment_settings)
```

`Activity` стає контейнером, а не місцем для всієї UI-логіки.

2. **Повторне використання UI**

Один Fragment можна використовувати в різних сценаріях:

```kotlin
class UserListFragment : Fragment(R.layout.fragment_user_list)
```

Наприклад, список користувачів може бути частиною phone screen, tablet master-detail layout або вкладеного flow.

3. **Адаптація під різні екрани**

Fragment історично добре підходили для phone/tablet layouts:

```text
phone  -> Activity + один Fragment
tablet -> Activity + list Fragment + details Fragment
```

Це дозволяє по-різному комбінувати одні й ті самі UI-блоки.

4. **Навігація і back stack**

Fragment дозволяють міняти частини екрана без запуску нової Activity:

```kotlin
parentFragmentManager.beginTransaction()
    .replace(R.id.container, DetailsFragment())
    .addToBackStack(null)
    .commit()
```

Navigation Component зараз робить це зручніше, але під капотом часто все одно працює з Fragment transactions.

5. **Lifecycle окремих частин UI**

Fragment має власний lifecycle:

```text
onCreate -> onCreateView -> onViewCreated -> onDestroyView -> onDestroy
```

Це дозволяє окремо керувати ресурсами, subscriptions, adapter-ами і View lifecycle конкретної частини екрана.

6. **Композиція екрана**

Fragment допомагають будувати екран з кількох незалежних блоків:

```text
Activity
 ├─ HeaderFragment
 ├─ ContentFragment
 └─ BottomSheetFragment
```

Але надмірна вкладеність Fragment може ускладнити lifecycle і back stack.

7. **Ізоляція відповідальності**

Fragment може відповідати за конкретний screen або UI-flow, а бізнес-логіка залишається у ViewModel/use cases:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private val viewModel: ProfileViewModel by viewModels()
}
```

Це краще, ніж тримати всю логіку в одній Activity.

8. **Які проблеми Fragment не вирішують**

Fragment самі по собі не вирішують:

- погану архітектуру;
- змішування UI і бізнес-логіки;
- неправильний state management;
- memory leaks після `onDestroyView()`;
- складну navigation без правил.

Для цього потрібні ViewModel, lifecycle-aware APIs, clean boundaries і дисципліна з state.

9. **Практичне правило**

Fragment корисні як screen/container abstraction у View System. Вони вирішують композицію UI, reuse, navigation і lifecycle частин екрана. Але якщо UI повністю на Compose, роль Fragment часто зменшується до host/container або може взагалі зникати.

**Коротко:** Fragment вирішують проблеми великих Activity, повторного використання UI, адаптації під різні екрани, навігації всередині Activity і lifecycle окремих частин UI. Але вони не замінюють архітектуру і самі можуть створити складність, якщо зловживати вкладеністю та back stack.

</details>
<details>
<summary>169. Які особливості вкладених Fragment?</summary>

#### Kotlin

Вкладені Fragment — це Fragment, які додаються всередину іншого Fragment через `childFragmentManager`. Вони корисні, коли частина екрана має власний lifecycle, navigation або внутрішню структуру.

1. **childFragmentManager**

Для вкладених Fragment треба використовувати саме `childFragmentManager`, а не `parentFragmentManager`:

```kotlin
class ParentFragment : Fragment(R.layout.fragment_parent) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        childFragmentManager.beginTransaction()
            .replace(R.id.childContainer, ChildFragment())
            .commit()
    }
}
```

`parentFragmentManager` працює з Fragment на рівні Activity або parent container. `childFragmentManager` — з Fragment всередині поточного Fragment.

2. **Lifecycle**

Child Fragment живе всередині parent Fragment. Якщо parent знищується, child Fragment теж буде знищений. Але child має власні callbacks:

```text
ParentFragment
  └── ChildFragment
```

Це означає, що child може мати власний `onCreateView`, `onViewCreated`, `onDestroyView`, ViewModel і state.

3. **ViewModel scope**

Scope ViewModel треба обирати явно:

```kotlin
val ownViewModel: ChildViewModel by viewModels()
val parentViewModel: ParentViewModel by viewModels({ requireParentFragment() })
val activityViewModel: SharedViewModel by activityViewModels()
```

- `viewModels()` — ViewModel тільки для child Fragment;
- `requireParentFragment()` — shared state з parent;
- `activityViewModels()` — shared state на рівні Activity.

4. **Navigation**

Якщо child Fragment має власну навігацію, для нього можна мати окремий `NavHostFragment`:

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/childNavHost"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:navGraph="@navigation/child_graph" />
```

Але вкладена navigation швидко ускладнює back stack, тому її варто використовувати тільки коли є реальна локальна навігація всередині частини екрана.

5. **Back stack**

У parent і child можуть бути різні back stacks. Це часта причина багів:

```kotlin
childFragmentManager.popBackStack()
```

Якщо натискання Back має впливати на child stack, це треба обробляти явно через Navigation Component або `OnBackPressedDispatcher`.

6. **Communication**

Для комунікації краще не тримати прямі references між Fragment. Варіанти:

- shared ViewModel;
- Fragment Result API;
- callbacks через interface, якщо це локальний простий випадок;
- navigation result.

Приклад Fragment Result API:

```kotlin
childFragmentManager.setFragmentResultListener("key", viewLifecycleOwner) { _, bundle ->
    val value = bundle.getString("value")
}
```

7. **onDestroyView і leaks**

Parent і child мають окремі View lifecycle. Якщо зберігати reference на child view або adapter після `onDestroyView()`, можна отримати leak.

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Особливо це актуально для ViewPager, RecyclerView adapters і custom callbacks.

8. **ViewPager2**

Вкладені Fragment часто зустрічаються у `ViewPager2`:

```kotlin
class TabsAdapter(fragment: Fragment) : FragmentStateAdapter(fragment) {
    override fun getItemCount(): Int = 3
    override fun createFragment(position: Int): Fragment = TabFragment.newInstance(position)
}
```

Якщо adapter створюється всередині Fragment, треба передавати `fragment`, а не `activity`, щоб lifecycle був правильним.

9. **Коли використовувати**

Вкладені Fragment доречні для:

- tabs всередині екрана;
- локального wizard flow;
- незалежної частини екрана зі своїм lifecycle;
- reusable fragment-блоку.

Не варто використовувати вкладені Fragment тільки для дрібного UI. Для цього краще custom View або composable.

10. **Практичне правило**

- Для child Fragment — `childFragmentManager`.
- Для shared state — правильно вибрати ViewModel scope.
- Для back stack — явно розуміти, який FragmentManager використовується.
- Для communication — shared ViewModel або Fragment Result API.
- Для cleanup — враховувати окремий View lifecycle.

**Коротко:** вкладені Fragment живуть всередині parent Fragment і керуються через `childFragmentManager`. Вони мають власний lifecycle, ViewModel scope і back stack, тому головні ризики — неправильний FragmentManager, заплутана navigation, leaks після `onDestroyView()` і нечітка communication-схема.

</details>
<details>
<summary>170. Які є виклики (callbacks) у Application?</summary>

#### Kotlin

`Application` — це singleton-обʼєкт процесу Android-додатка. Він створюється раніше за `Activity`, `Service` або `BroadcastReceiver` у цьому процесі й живе, поки живе процес. Основні callbacks: `onCreate()`, `onTerminate()`, `onLowMemory()`, `onTrimMemory()`, `onConfigurationChanged()`. Також через `Application` часто реєструють `ActivityLifecycleCallbacks`.

1. **onCreate**

Викликається при старті процесу додатка:

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        // DI
        // logging
        // crash reporting
        // analytics setup
    }
}
```

Типові задачі:

- ініціалізація Hilt/DI;
- logging;
- crash reporting;
- app-wide SDK setup;
- реєстрація lifecycle callbacks.

Важливо: `onCreate()` має бути швидким. Важка робота на main thread погіршує cold start.

2. **Manifest**

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme">

</application>
```

Якщо `android:name` не вказати, Android створить стандартний `Application`.

3. **onTerminate**

```kotlin
override fun onTerminate() {
    super.onTerminate()
}
```

У production на реальних Android-девайсах на нього покладатися не можна. Система може просто вбити процес без виклику `onTerminate()`.

Практичне правило: не зберігати важливі дані й не робити critical cleanup в `onTerminate()`.

4. **onLowMemory**

```kotlin
override fun onLowMemory() {
    super.onLowMemory()
    imageCache.clear()
}
```

Викликається при критично низькій памʼяті. Тут можна звільнити кеші або некритичні ресурси.

5. **onTrimMemory**

Дає точніший сигнал про memory pressure:

```kotlin
override fun onTrimMemory(level: Int) {
    super.onTrimMemory(level)

    when (level) {
        TRIM_MEMORY_RUNNING_LOW,
        TRIM_MEMORY_RUNNING_CRITICAL -> imageCache.clear()

        TRIM_MEMORY_UI_HIDDEN -> screenCache.clear()
    }
}
```

`TRIM_MEMORY_UI_HIDDEN` означає, що UI додатка більше не видимий. Це хороший момент звільнити UI-related ресурси, але не critical state.

6. **onConfigurationChanged**

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

Application може отримувати configuration callbacks, але UI-specific configuration handling краще робити на рівні Activity/Fragment/Compose.

7. **ActivityLifecycleCallbacks**

`Application` може слухати lifecycle усіх Activity:

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        registerActivityLifecycleCallbacks(
            object : ActivityLifecycleCallbacks {
                override fun onActivityStarted(activity: Activity) = Unit
                override fun onActivityResumed(activity: Activity) = Unit
                override fun onActivityPaused(activity: Activity) = Unit
                override fun onActivityStopped(activity: Activity) = Unit
                override fun onActivityDestroyed(activity: Activity) = Unit

                override fun onActivityCreated(
                    activity: Activity,
                    savedInstanceState: Bundle?
                ) = Unit

                override fun onActivitySaveInstanceState(
                    activity: Activity,
                    outState: Bundle
                ) = Unit
            }
        )
    }
}
```

Використання:

- analytics screen tracking;
- foreground/background detection;
- session tracking;
- debug tooling;
- global activity monitoring.

8. **Foreground/background detection**

Спрощена ідея:

```kotlin
private var startedActivities = 0

override fun onActivityStarted(activity: Activity) {
    startedActivities++
    if (startedActivities == 1) onAppForeground()
}

override fun onActivityStopped(activity: Activity) {
    startedActivities--
    if (startedActivities == 0) onAppBackground()
}
```

У production краще враховувати configuration changes і edge cases або використовувати `ProcessLifecycleOwner`.

9. **Чого не робити в Application**

Погано:

```kotlin
class App : Application() {
    var currentActivity: Activity? = null
    var currentUser: User? = null
}
```

Проблеми:

- `Activity` reference може створити memory leak;
- process може бути вбитий, і state зникне;
- business logic стає глобальною;
- тестування ускладнюється.

`Application` не має бути global mutable storage.

10. **Hilt**

Для Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

Це запускає генерацію application-level dependency graph.

11. **Практичне правило**

- `onCreate()` — lightweight global initialization.
- `onTrimMemory()` — реакція на memory pressure.
- `onLowMemory()` — fallback для критичної нестачі памʼяті.
- `onTerminate()` — не використовувати для production cleanup.
- `ActivityLifecycleCallbacks` — глобальне спостереження за Activity lifecycle.
- Не тримати `Activity`, `Fragment` або `View` у `Application`.
- Не використовувати `Application` як сховище business state.

**Коротко:** головний callback `Application` — `onCreate()`. Для памʼяті є `onTrimMemory()` і `onLowMemory()`, `onTerminate()` не гарантується, а для lifecycle усіх Activity використовується `registerActivityLifecycleCallbacks()`.

</details>
<details>
<summary>171. Який singleton створює система під час запуску Android-додатка?</summary>

#### Kotlin

Під час запуску Android-додатка система створює один екземпляр `Application` для процесу додатка. Це application-level singleton, який існує протягом життя процесу і створюється до `Activity`, `Service`, `BroadcastReceiver` та інших компонентів у цьому процесі.

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
    android:theme="@style/AppTheme">

</application>
```

Коли процес додатка стартує, Android створює екземпляр `App`.

2. **Важливе уточнення: singleton не на весь пристрій**

`Application` — singleton не глобально для всієї системи, а тільки в межах конкретного процесу:

```text
app process
 └── Application instance
```

Якщо додаток має кілька процесів, у кожному процесі буде свій `Application` instance.

3. **Multi-process випадок**

Наприклад:

```xml
<service
    android:name=".SyncService"
    android:process=":sync" />
```

Тоді може бути така структура:

```text
main process
 └── App instance #1

:sync process
 └── App instance #2
```

Тому не можна вважати `Application`, `object` або static state єдиним shared storage між процесами.

4. **Коли викликається Application.onCreate**

`Application.onCreate()` викликається при створенні процесу:

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()

        initLogger()
        initCrashReporter()
    }
}
```

Це відбувається перед створенням першого component у цьому процесі.

5. **Для чого використовують Application**

Типові задачі:

- ініціалізація dependency injection;
- crash reporting;
- logging;
- analytics setup;
- app-wide конфігурація;
- реєстрація `ActivityLifecycleCallbacks`;
- lazy initialization глобальних SDK.

Наприклад з Hilt:

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` створює application-level dependency graph.

6. **Application context**

З `Application` можна отримати `applicationContext`:

```kotlin
class UserRepository(
    private val context: Context
)
```

Для long-lived залежностей треба передавати саме application context, а не `Activity`:

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

7. **Application не має бути global state dump**

Погано:

```kotlin
class App : Application() {
    var currentUser: User? = null
    var currentActivity: Activity? = null
}
```

Проблеми:

- state може зникнути після process death;
- `Activity` reference створює memory leak;
- business logic стає глобальною і неконтрольованою;
- тестування ускладнюється.

8. **Application не замінює DI container**

Іноді початківці роблять так:

```kotlin
class App : Application() {
    lateinit var repository: UserRepository

    override fun onCreate() {
        super.onCreate()
        repository = UserRepository()
    }
}
```

Для маленького pet-проєкту це може працювати, але в production краще використовувати DI:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    @Provides
    @Singleton
    fun provideUserRepository(api: UserApi): UserRepository {
        return UserRepository(api)
    }
}
```

9. **Application і process death**

Якщо система вбила процес, `Application` буде створений заново при наступному старті:

```text
process killed
user opens app
new process
new Application instance
```

Тому все критичне треба зберігати в persistent storage:

- database;
- DataStore;
- SharedPreferences;
- files;
- backend.

Не можна покладатися на поля `Application` як на надійне сховище.

10. **Чим Application відрізняється від Kotlin object**

Kotlin `object`:

```kotlin
object SessionHolder {
    var token: String? = null
}
```

`Application`:

```kotlin
class App : Application()
```

Обидва можуть виглядати як singleton у процесі, але:

- `Application` створюється Android framework;
- `Application` має lifecycle callbacks;
- `Application` має context;
- `object` створюється lazy при першому зверненні;
- обидва будуть втрачені при process death.

11. **Практичне правило**

- Система створює singleton `Application` на процес.
- `Application.onCreate()` — місце для легкої глобальної ініціалізації.
- Не тримати там `Activity`, `Fragment`, `View`.
- Не використовувати `Application` як storage для business state.
- Для long-lived залежностей використовувати application context.
- У multi-process додатку буде кілька `Application` instances.

**Коротко:** Android при старті процесу створює singleton `Application`. Він один у межах процесу, живе поки живе процес, підходить для глобальної ініціалізації, але не є надійним сховищем стану і не повинен тримати UI-обʼєкти.

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

`ANR` виникає, коли main thread Android-додатка занадто довго не відповідає на системні події. Система вважає застосунок “завислим” і показує діалог Application Not Responding.

1. **Головна причина**

ANR майже завжди означає, що main thread заблокований:

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // погано: блокує main thread
}
```

Main thread має швидко обробляти input, lifecycle, rendering і callbacks. Якщо він зайнятий довгою роботою, UI перестає відповідати.

2. **Довгі операції на main thread**

Типові помилки:

- network request на main thread;
- читання/запис великих файлів;
- важкий JSON parsing;
- bitmap decoding;
- database query без background dispatcher;
- складні цикли або сортування великих колекцій.

Правильно переносити роботу в `Dispatchers.IO` або `Dispatchers.Default`:

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        repository.loadData()
    }
    _state.value = State.Success(result)
}
```

3. **BroadcastReceiver**

ANR можна отримати, якщо `BroadcastReceiver.onReceive()` виконується занадто довго:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    // погано: довга синхронна робота
}
```

`onReceive()` має швидко завершуватись. Для довшої роботи краще запускати `WorkManager` або використовувати `goAsync()` дуже обережно.

4. **Service**

`Service` теж працює на main thread, якщо явно не винести роботу в background:

```kotlin
override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
    scope.launch(Dispatchers.IO) {
        syncData()
        stopSelf(startId)
    }
    return START_NOT_STICKY
}
```

Помилка — думати, що Service автоматично дає окремий потік.

5. **Deadlock і lock contention**

ANR може зʼявитись через блокування locks:

```kotlin
synchronized(lock) {
    // main thread чекає ресурс, який тримає background thread
}
```

Особливо небезпечно, коли main thread чекає результат background thread, а background thread чекає main thread.

6. **Binder calls**

Синхронні IPC/Binder calls можуть заблокувати main thread, якщо інший процес повільний або завис:

```kotlin
val result = remoteService.getData() // ризик, якщо виклик синхронний
```

Такі виклики краще не робити напряму з UI thread.

7. **Повільний rendering**

Формально це частіше jank, але дуже важкий UI може привести до зависання:

- надто складний layout;
- дорогі custom `onDraw()`;
- великі allocations під час rendering;
- блокуючі операції в Compose/View callbacks.

Custom drawing і Compose recomposition мають бути легкими.

8. **Як діагностувати ANR**

Основні інструменти:

- Android Studio Profiler;
- Logcat;
- ANR traces;
- Play Console ANR reports;
- StrictMode;
- Perfetto/System Trace.

`StrictMode` допомагає рано ловити disk/network роботу на main thread:

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

9. **Як уникати ANR**

Практичні правила:

- не блокувати main thread;
- переносити I/O в `Dispatchers.IO`;
- CPU-heavy роботу — в `Dispatchers.Default`;
- не робити synchronous wait типу `runBlocking` у UI;
- не тримати locks на main thread;
- не виконувати довгу роботу в `BroadcastReceiver`;
- профілювати складні екрани;
- додавати timeouts для зовнішніх викликів.

10. **Типовий поганий приклад**

```kotlin
fun loadProfile() = runBlocking {
    repository.fetchProfile()
}
```

`runBlocking` на main thread блокує UI і може привести до ANR.

Краще:

```kotlin
fun loadProfile() {
    viewModelScope.launch {
        val profile = withContext(Dispatchers.IO) {
            repository.fetchProfile()
        }
        _state.value = ProfileState(profile)
    }
}
```

**Коротко:** ANR виникає, коли main thread довго не відповідає. Основні причини — blocking I/O, важкі CPU-операції, довгі `BroadcastReceiver`/`Service` callbacks, locks, synchronous IPC і неправильне використання `runBlocking`. Профілактика — не блокувати UI thread, переносити роботу на правильні dispatchers і профілювати проблемні місця.

</details>
<details>
<summary>174. Що можна виконувати в main thread?</summary>

#### Kotlin

У main thread можна виконувати тільки швидку UI-роботу і легку coordination logic: оновлення View/Compose UI, обробку кліків, lifecycle callbacks, запуск корутин, просту валідацію, читання вже підготовленого state. Не можна виконувати довгий network, database, file I/O, важкі обчислення, blocking waits або будь-що, що може заблокувати UI і привести до ANR.

1. **Для чого main thread**

Main thread в Android відповідає за:

- rendering UI;
- обробку touch/input events;
- lifecycle callbacks;
- View hierarchy operations;
- Compose recomposition/application of changes;
- dispatch повідомлень через `Looper`.

Тому UI-операції мають виконуватися саме на main thread:

```kotlin
binding.title.text = "Profile"
binding.progressBar.isVisible = false
```

2. **Оновлення UI**

View-система не thread-safe. Оновлювати View треба з main thread:

```kotlin
viewModelScope.launch {
    val user = repository.getUser(userId)
    _state.value = ProfileState.Content(user)
}
```

У Fragment:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewModel.state.collect { state ->
        binding.progressBar.isVisible = state is ProfileState.Loading
    }
}
```

Collector тут працює на main thread, якщо не змінено dispatcher.

3. **Обробка кліків**

Click listener виконується на main thread, але має бути швидким:

```kotlin
binding.retryButton.setOnClickListener {
    viewModel.reload()
}
```

Добре: делегувати дію у ViewModel.  
Погано: робити важку роботу прямо в listener.

```kotlin
binding.retryButton.setOnClickListener {
    Thread.sleep(5_000)
}
```

Це блокує UI.

4. **Lifecycle callbacks**

`onCreate`, `onStart`, `onResume`, `onPause`, `onStop` викликаються на main thread:

```kotlin
override fun onResume() {
    super.onResume()
    analytics.trackScreen("Profile")
}
```

Тут можна робити швидкі операції. Довгі задачі треба запускати асинхронно:

```kotlin
override fun onStart() {
    super.onStart()
    viewModel.load()
}
```

5. **Запуск корутин**

Запустити coroutine з main thread нормально:

```kotlin
viewModelScope.launch {
    val user = repository.getUser(userId)
    _state.value = ProfileState.Content(user)
}
```

Але всередині repository blocking I/O має бути перенесений на `Dispatchers.IO`:

```kotlin
suspend fun getUser(userId: String): User = withContext(Dispatchers.IO) {
    api.getUserBlocking(userId)
}
```

6. **Легка валідація**

На main thread можна робити просту синхронну валідацію:

```kotlin
fun onEmailChanged(email: String) {
    val isValid = email.contains("@")
    _state.value = state.value.copy(isEmailValid = isValid)
}
```

Але якщо валідація важка або працює з великими даними, її треба винести:

```kotlin
val result = withContext(Dispatchers.Default) {
    heavyValidator.validate(input)
}
```

7. **Compose UI logic**

Composable-функції виконуються на main thread:

```kotlin
@Composable
fun ProfileScreen(state: ProfileState) {
    Text(text = state.name)
}
```

У composable не можна робити blocking work:

```kotlin
@Composable
fun BadScreen() {
    Thread.sleep(1_000)
    Text("Loaded")
}
```

Side effects треба виконувати через Compose APIs:

```kotlin
LaunchedEffect(userId) {
    viewModel.load(userId)
}
```

8. **Що не можна робити на main thread**

Не можна:

- network calls;
- database queries, якщо вони blocking;
- file reads/writes;
- image decoding великих bitmap;
- JSON parsing великих payload;
- сортування/фільтрацію великих списків;
- криптографію;
- compression/decompression;
- `Thread.sleep`;
- `runBlocking`;
- `Future.get`;
- `CountDownLatch.await`;
- довгі synchronized blocks.

Погано:

```kotlin
val json = File(path).readText()
val result = Json.decodeFromString<BigResponse>(json)
```

Краще:

```kotlin
val result = withContext(Dispatchers.IO) {
    val json = File(path).readText()
    Json.decodeFromString<BigResponse>(json)
}
```

9. **Dispatchers.Main**

`Dispatchers.Main` використовується для UI:

```kotlin
withContext(Dispatchers.Main) {
    binding.progressBar.isVisible = false
}
```

Але якщо ти вже у `viewModelScope.launch { ... }`, за замовчуванням це часто вже Main dispatcher:

```kotlin
viewModelScope.launch {
    _state.value = ProfileState.Loading
}
```

10. **Dispatchers.IO**

Для blocking I/O:

```kotlin
suspend fun readConfig(): Config = withContext(Dispatchers.IO) {
    File("config.json").readText()
        .let { Json.decodeFromString<Config>(it) }
}
```

Сюди належать file, database, network blocking APIs.

11. **Dispatchers.Default**

Для CPU-heavy роботи:

```kotlin
suspend fun calculate(items: List<Item>): List<Result> {
    return withContext(Dispatchers.Default) {
        items.map { heavyTransform(it) }
    }
}
```

Це не I/O, а computation, тому `Default`, не `IO`.

12. **Room і main thread**

Room зазвичай забороняє queries на main thread:

```kotlin
@Query("SELECT * FROM users")
suspend fun getUsers(): List<UserEntity>
```

Якщо DAO метод `suspend`, Room виконає його без блокування main thread. Не треба вмикати:

```kotlin
.allowMainThreadQueries()
```

У production це майже завжди погана ідея.

13. **Практичне правило**

- UI updates — main thread.
- Click/lifecycle callbacks — main thread, але тільки швидка логіка.
- Запуск coroutine — main thread, важку роботу переносити dispatcher-ами.
- Blocking I/O — `Dispatchers.IO`.
- CPU-heavy — `Dispatchers.Default`.
- Не блокувати main thread очікуванням, sleep або synchronized-heavy кодом.
- Якщо операція може зайняти більше кількох мілісекунд — подумати про background dispatcher.

**Коротко:** в main thread можна виконувати UI-оновлення, lifecycle/click handling і легку coordination logic. Усе важке або blocking треба переносити з main thread, інакше будуть frame drops, freezes або ANR.

</details>
<details>
<summary>175. Які є базові компоненти Android?</summary>

#### Kotlin

Базові компоненти Android — це `Activity`, `Service`, `BroadcastReceiver` і `ContentProvider`. Це framework-level entry points, через які система запускає частини додатка. Окремо важливі `Application`, `Intent`, `Context` і `Manifest`, але класична відповідь на питання про core components — саме чотири компоненти.

1. **Activity**

`Activity` представляє екран або container для UI:

```kotlin
class MainActivity : AppCompatActivity(R.layout.activity_main)
```

Типові задачі:

- показати UI;
- приймати user input;
- бути host для Fragment або Compose;
- обробляти lifecycle екрана;
- стартувати navigation flow.

У сучасній архітектурі часто використовують single-activity підхід:

```text
MainActivity
 └── NavHostFragment / Compose Navigation
```

Тобто `Activity` — це не обовʼязково кожен екран, а часто головний host додатка.

2. **Service**

`Service` — компонент для роботи без прямого UI:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Важливо: `Service` не означає автоматично background thread. За замовчуванням lifecycle methods сервісу викликаються на main thread.

Типи:

- started service;
- bound service;
- foreground service.

Для більшості відкладених background tasks краще використовувати `WorkManager`, а не ручний `Service`.

3. **BroadcastReceiver**

`BroadcastReceiver` приймає broadcast-події від системи або додатків:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            // schedule work
        }
    }
}
```

Типові сценарії:

- system events;
- connectivity/power changes;
- alarms;
- app-internal broadcasts, хоча зараз це менш поширено.

`onReceive()` має бути швидким. Для довгої роботи треба делегувати в `WorkManager`.

4. **ContentProvider**

`ContentProvider` дає стандартизований доступ до даних через URI:

```kotlin
class UserProvider : ContentProvider() {
    override fun onCreate(): Boolean = true

    override fun query(
        uri: Uri,
        projection: Array<out String>?,
        selection: String?,
        selectionArgs: Array<out String>?,
        sortOrder: String?
    ): Cursor? {
        return null
    }
}
```

Використовується для:

- sharing data між додатками;
- доступу до contacts/media/calendar;
- file sharing через `FileProvider`;
- ініціалізації бібліотек через provider startup pattern.

У звичайному app-коді власний `ContentProvider` пишуть нечасто, але `FileProvider` використовується регулярно.

5. **Application**

`Application` не входить у класичну четвірку компонентів, але це важливий process-level singleton:

```kotlin
@HiltAndroidApp
class App : Application()
```

Використовується для:

- DI initialization;
- logging;
- crash reporting;
- global SDK setup;
- Activity lifecycle callbacks.

Не треба використовувати `Application` як storage для business state або тримати там `Activity`.

6. **Intent**

`Intent` — механізм комунікації між Android components:

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
intent.putExtra("user_id", userId)
startActivity(intent)
```

Implicit intent:

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Через `Intent` система розуміє, який component треба запустити і з якими даними.

7. **Context**

`Context` дає доступ до app/system resources:

```kotlin
val appName = context.getString(R.string.app_name)
```

Через `Context` можна:

- отримати resources;
- стартувати Activity/Service;
- отримати system services;
- доступитися до files/preferences/database;
- створити UI-related обʼєкти.

Важливо розрізняти `Activity context` і `Application context`, щоб не створювати memory leaks.

8. **AndroidManifest**

Компоненти оголошуються в `AndroidManifest.xml`:

```xml
<application
    android:name=".App">

    <activity
        android:name=".MainActivity"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>

    <receiver android:name=".BootReceiver" />

</application>
```

Manifest описує system-level entry points, permissions, exported state і intent filters.

9. **Життєві цикли різні**

Кожен компонент має свій lifecycle:

```text
Activity          -> onCreate/onStart/onResume/onPause/onStop/onDestroy
Service           -> onCreate/onStartCommand/onBind/onDestroy
BroadcastReceiver -> onReceive
ContentProvider   -> onCreate/query/insert/update/delete
Application       -> onCreate/onTrimMemory
```

Не можна переносити припущення з одного lifecycle на інший.

10. **Практичне правило**

- `Activity` — UI entry point або host.
- `Service` — робота без UI, але не автоматично background thread.
- `BroadcastReceiver` — швидка реакція на broadcast events.
- `ContentProvider` — structured data sharing через URI.
- `Application` — singleton процесу для легкої глобальної ініціалізації.
- `Intent` — запуск і комунікація між components.
- `Context` — доступ до Android environment.
- `Manifest` — декларація components і permissions.

**Коротко:** базові Android components — `Activity`, `Service`, `BroadcastReceiver` і `ContentProvider`. Вони є entry points для системи, мають різний lifecycle і взаємодіють через `Intent`, `Context` та декларації в `AndroidManifest.xml`.

</details>
<details>
<summary>176. Що таке Service?</summary>

#### Kotlin

`Service` — це Android component для роботи у фоні без власного UI. Він живе окремо від `Activity`, але все одно працює в процесі застосунку і за замовчуванням на main thread. Тому довгі операції всередині `Service` треба переносити в coroutine, thread, executor або інший async-механізм.

1. **Для чого потрібен Service**

Service використовують, коли робота має продовжуватись без активного екрана:

- програвання музики;
- синхронізація даних;
- завантаження файлів;
- foreground tracking;
- робота з long-running operation;
- взаємодія з іншим компонентом через binding.

Але Service не означає “окремий потік”. Це часта помилка на співбесідах.

2. **Started Service**

Started service запускається через `startService()` або `ContextCompat.startForegroundService()`:

```kotlin
class SyncService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        CoroutineScope(Dispatchers.IO).launch {
            syncData()
            stopSelf(startId)
        }

        return START_NOT_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? = null
}
```

Такий Service виконує задачу і сам себе зупиняє через `stopSelf()`.

3. **Bound Service**

Bound service дозволяє іншому компоненту підʼєднатись і викликати методи сервісу:

```kotlin
class PlayerService : Service() {
    private val binder = LocalBinder()

    inner class LocalBinder : Binder() {
        fun service(): PlayerService = this@PlayerService
    }

    override fun onBind(intent: Intent): IBinder = binder

    fun play() {
        // start playback
    }
}
```

Він живе, поки є bound clients, або довше, якщо паралельно був запущений як started service.

4. **Foreground Service**

Foreground service потрібен для помітної для користувача довгої роботи. Він має показувати notification:

```kotlin
class PlayerService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        startForeground(NOTIFICATION_ID, createNotification())
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? = null
}
```

Приклади: media playback, location tracking, active file upload/download. Якщо задача не є user-visible, foreground service часто буде неправильним вибором.

5. **Lifecycle Service**

Основні callbacks:

```kotlin
override fun onCreate() {
    super.onCreate()
}

override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
    return START_NOT_STICKY
}

override fun onBind(intent: Intent?): IBinder? = null

override fun onDestroy() {
    super.onDestroy()
}
```

`onCreate()` викликається один раз на створення. `onStartCommand()` — на кожен start request. `onDestroy()` — для cleanup.

6. **START_STICKY / NOT_STICKY / REDELIVER_INTENT**

```text
START_STICKY           -> система може пересоздати Service без старого Intent
START_NOT_STICKY       -> не пересоздавати автоматично
START_REDELIVER_INTENT -> пересоздати і повторно доставити останній Intent
```

Вибір залежить від задачі. Для sync job часто достатньо `START_NOT_STICKY`. Для player — частіше `START_STICKY`.

7. **Service і threading**

Service працює на main thread:

```kotlin
class SyncService : Service() {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        scope.launch {
            syncData()
            stopSelf(startId)
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

Головне — не блокувати main thread і скасовувати роботу в `onDestroy()`.

8. **Коли Service не потрібен**

Не варто використовувати Service для будь-якої фонової задачі. Часто кращі варіанти:

- `WorkManager` — deferrable/background work;
- `AlarmManager` — точний запуск у часі;
- `JobScheduler` — системно керовані jobs;
- `DownloadManager` — прості завантаження;
- `MediaSession` + Media3 — media playback.

9. **Практичне правило**

Service потрібен, коли робота має бути привʼязана до Android component lifecycle і може тривати без екрана. Якщо задача має пережити process death, network constraints або reboot, частіше потрібен `WorkManager`, а не Service.

**Коротко:** `Service` — Android component для роботи без UI. Він не створює окремий thread автоматично. Є started, bound і foreground services. Для довгих задач потрібен async-код, cleanup і правильний вибір між `Service`, `ForegroundService` та `WorkManager`.

</details>
<details>
<summary>177. Що таке BroadcastReceiver?</summary>

#### Kotlin

`BroadcastReceiver` — це Android component, який отримує broadcast events від системи або інших застосунків. Він використовується для реакції на події типу boot completed, connectivity changes, package changes, alarm events або власні app broadcasts.

1. **Базова ідея**

Receiver має один головний callback:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        if (intent.action == Intent.ACTION_BOOT_COMPLETED) {
            // handle boot
        }
    }
}
```

`onReceive()` має виконуватись швидко. Не можна робити довгу роботу напряму в цьому методі.

2. **Manifest receiver**

Receiver можна оголосити в manifest:

```xml
<receiver
    android:name=".BootReceiver"
    android:exported="false">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

Для деяких system broadcasts потрібні permissions, наприклад `RECEIVE_BOOT_COMPLETED`.

3. **Dynamic receiver**

Receiver можна зареєструвати в runtime:

```kotlin
val receiver = object : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // handle event
    }
}

context.registerReceiver(receiver, IntentFilter(Intent.ACTION_BATTERY_LOW))
```

І обовʼязково unregister:

```kotlin
context.unregisterReceiver(receiver)
```

Інакше можна отримати leak.

4. **Exported**

`android:exported` визначає, чи можуть інші apps надсилати broadcasts цьому receiver:

```text
exported=false -> тільки всередині app / system за правилами
exported=true  -> доступний зовні, потрібна обережність
```

Якщо receiver exported, треба думати про security і permissions.

5. **Implicit broadcast restrictions**

Починаючи з Android 8, багато implicit broadcasts не можна просто слухати через manifest receiver. Це зроблено для економії батареї. Часто треба використовувати dynamic receiver, `WorkManager` або інший API.

6. **Довга робота**

Погано:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    Thread.sleep(10_000)
}
```

Краще передати роботу в `WorkManager`:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    val request = OneTimeWorkRequestBuilder<SyncWorker>().build()
    WorkManager.getInstance(context).enqueue(request)
}
```

`onReceive()` має тільки прийняти event і делегувати роботу.

7. **goAsync**

`goAsync()` дозволяє завершити роботу асинхронно, але його треба використовувати обережно:

```kotlin
override fun onReceive(context: Context, intent: Intent) {
    val pendingResult = goAsync()
    CoroutineScope(Dispatchers.IO).launch {
        try {
            // short async work
        } finally {
            pendingResult.finish()
        }
    }
}
```

Якщо забути `finish()`, можна отримати проблеми з lifecycle receiver.

8. **Local broadcasts**

`LocalBroadcastManager` deprecated. Для communication всередині app краще використовувати Flow, shared ViewModel, callbacks, repository state або event bus тільки якщо є чітка причина.

9. **Коли використовувати**

BroadcastReceiver доречний для:

- system events;
- alarm callbacks;
- boot completed;
- package changes;
- app-specific broadcasts між компонентами, якщо це справді потрібно.

Не варто використовувати receiver як універсальний механізм комунікації всередині app.

10. **Практичне правило**

- `onReceive()` має бути коротким.
- Для довгої роботи — `WorkManager` або foreground service.
- Dynamic receiver треба unregister.
- Manifest receiver має коректний `exported`.
- Не покладатися на restricted implicit broadcasts.
- Не використовувати receiver замість нормального app state flow.

**Коротко:** `BroadcastReceiver` отримує system або app broadcasts. Його `onReceive()` має швидко завершуватись, довгу роботу треба делегувати в `WorkManager`/service, dynamic receivers треба unregister, а manifest receivers мають бути безпечно налаштовані через `exported` і permissions.

</details>
<details>
<summary>178. Що таке Context і які його типи існують?</summary>

#### Kotlin

`Context` в Android — це обʼєкт, який дає доступ до ресурсів, system services, application environment, package info, запуску Activity/Service, доступу до assets, files, preferences тощо. Він є “точкою входу” до Android runtime для component-а.

1. **Для чого потрібен Context**

Через `Context` можна:

```kotlin
context.getString(R.string.app_name)
context.getSystemService(Context.CONNECTIVITY_SERVICE)
context.startActivity(intent)
context.getSharedPreferences("settings", Context.MODE_PRIVATE)
```

Але неправильне зберігання `Context` легко створює memory leak.

2. **Application Context**

```kotlin
val appContext = context.applicationContext
```

Application context живе стільки, скільки процес app. Його можна використовувати для dependencies, які не потребують UI theme або Activity lifecycle: database, DataStore, WorkManager, app-level services.

3. **Activity Context**

Activity context привʼязаний до конкретної Activity і має theme/window/lifecycle:

```kotlin
class MainActivity : AppCompatActivity() {
    fun openScreen() {
        startActivity(Intent(this, DetailsActivity::class.java))
    }
}
```

Його треба використовувати для UI-related дій: dialogs, views, themed resources, starting Activity з normal task behavior.

4. **Fragment Context**

У Fragment context доступний через:

```kotlin
val context = requireContext()
val activity = requireActivity()
```

Не можна зберігати Fragment context у long-living object. Після `onDestroyView()` або detach це може привести до leak/crash.

5. **Service Context**

Service теж є Context:

```kotlin
class SyncService : Service() {
    override fun onCreate() {
        val notificationManager = getSystemService(NotificationManager::class.java)
    }
}
```

Service context підходить для service-related operations, але не має Activity UI.

6. **ContextWrapper**

`ContextWrapper` обгортає інший Context і може змінювати поведінку:

```kotlin
val themedContext = ContextThemeWrapper(context, R.style.AppTheme)
```

Часто використовується для theme override.

7. **Context у Compose**

У Compose context отримують так:

```kotlin
val context = LocalContext.current
```

Його можна використовувати для resources або запуску intent, але не треба перетворювати composable на місце бізнес-логіки.

8. **Memory leaks**

Погано:

```kotlin
object ImageLoaderHolder {
    lateinit var context: Activity
}
```

Якщо singleton тримає Activity context, Activity не буде звільнена після rotation/close.

Краще:

```kotlin
class AppDatabaseFactory(
    private val appContext: Context
)
```

і передавати `applicationContext`.

9. **Який Context використовувати**

```text
Application context -> long-living dependencies, DB, DataStore
Activity context    -> UI, dialogs, themed views, start activity
Fragment context    -> коротко в межах lifecycle Fragment
Service context     -> service operations
Compose LocalContext -> UI/resource actions у composable
```

10. **Практичне правило**

Якщо обʼєкт живе довше Activity/Fragment — не передавати йому Activity context. Для singleton/repository/database використовувати application context. Для UI — Activity/Fragment/themed context.

**Коротко:** `Context` дає доступ до Android resources, services і runtime operations. Основні типи — Application, Activity, Fragment/Service context і ContextWrapper. Головне правило: long-living objects мають використовувати application context, а UI-related операції — Activity/Fragment context.

</details>
<details>
<summary>179. Що таке Bundle?</summary>

#### Kotlin

`Bundle` — це контейнер key-value даних в Android. Його використовують для передачі невеликих обсягів даних між components, збереження instance state, передачі arguments у Fragment, extras в Intent і state через `SavedStateHandle`. Значення в `Bundle` мають бути типами, які Android може серіалізувати/парселізувати.

1. **Bundle як key-value контейнер**

```kotlin
val bundle = Bundle().apply {
    putString("user_id", "42")
    putInt("page", 1)
    putBoolean("is_admin", false)
}
```

Читання:

```kotlin
val userId = bundle.getString("user_id")
val page = bundle.getInt("page")
val isAdmin = bundle.getBoolean("is_admin")
```

Ключі — `String`, значення — підтримувані Android типи.

2. **Для чого використовується Bundle**

Типові сценарії:

- `Activity` extras через `Intent`;
- Fragment arguments;
- `onSaveInstanceState`;
- Navigation arguments;
- `SavedStateHandle`;
- передача результатів через Fragment Result API;
- IPC/Binder interaction.

3. **Bundle в Intent extras**

```kotlin
val intent = Intent(context, DetailsActivity::class.java).apply {
    putExtra("user_id", userId)
}

context.startActivity(intent)
```

У Activity:

```kotlin
val userId = intent.getStringExtra("user_id")
```

Під капотом extras зберігаються в `Bundle`.

4. **Bundle як Fragment arguments**

Передача arguments:

```kotlin
class DetailsFragment : Fragment(R.layout.fragment_details) {
    companion object {
        private const val ARG_USER_ID = "user_id"

        fun newInstance(userId: String): DetailsFragment {
            return DetailsFragment().apply {
                arguments = bundleOf(ARG_USER_ID to userId)
            }
        }
    }
}
```

Читання:

```kotlin
private val userId: String by lazy {
    requireArguments().getString(ARG_USER_ID)
        ?: error("user_id is required")
}
```

Arguments треба встановлювати до того, як Fragment буде доданий у FragmentManager.

5. **Bundle в onSaveInstanceState**

`Bundle` використовується для збереження transient UI state:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("query", binding.searchInput.text.toString())
}
```

Відновлення:

```kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    val query = savedInstanceState?.getString("query").orEmpty()
    binding.searchInput.setText(query)
}
```

Тут треба зберігати невеликі дані, а не великі обʼєкти або списки.

6. **Підтримувані типи**

`Bundle` підтримує:

- primitives: `Int`, `Long`, `Boolean`, `Float`, `Double`;
- `String`;
- arrays;
- `ArrayList<String>` та деякі typed lists;
- `Parcelable`;
- `Serializable`;
- інші `Bundle`;
- `IBinder`;
- `Size`, `SizeF` тощо.

Приклад з Parcelable:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

```kotlin
val bundle = bundleOf("user" to UserArgs("42", "Alex"))
```

7. **Parcelable краще за Serializable**

В Android для передачі обʼєктів через `Bundle` зазвичай краще `Parcelable`:

```kotlin
@Parcelize
data class ProductArgs(
    val id: String,
    val title: String
) : Parcelable
```

`Serializable` простіший, але повільніший і менш Android-орієнтований.

8. **Bundle не для великих даних**

Погано:

```kotlin
bundle.putParcelableArrayList("items", hugeList)
```

Проблеми:

- Binder transaction limit;
- performance overhead;
- crash `TransactionTooLargeException`;
- складне відновлення після process death.

Краще передавати id:

```kotlin
bundle.putString("user_id", userId)
```

А дані завантажувати з repository/database/cache.

9. **TransactionTooLargeException**

Якщо передати занадто багато даних через `Intent`/`Bundle`, можна отримати:

```text
TransactionTooLargeException
```

Це часто трапляється, коли в `onSaveInstanceState` кладуть великі списки, bitmap або response models.

Правило: у `Bundle` передавати тільки мінімальний state або identifiers.

10. **Bundle і Navigation Component**

З Navigation Component arguments теж передаються через `Bundle`, але краще використовувати Safe Args:

```kotlin
findNavController().navigate(
    UserListFragmentDirections.openUserDetails(userId)
)
```

У destination:

```kotlin
private val args: UserDetailsFragmentArgs by navArgs()
```

Safe Args дає type-safety і зменшує ризик помилок у string keys.

11. **Bundle і SavedStateHandle**

`SavedStateHandle` працює поверх saved state mechanism:

```kotlin
class DetailsViewModel(
    savedStateHandle: SavedStateHandle
) : ViewModel() {
    private val userId: String = checkNotNull(savedStateHandle["user_id"])
}
```

Це зручно для ViewModel, бо arguments і saved state доступні без прямої залежності від Fragment.

12. **Ключі краще централізувати**

Погано:

```kotlin
bundle.putString("userId", userId)
val id = bundle.getString("user_id")
```

Через різні ключі буде bug. Краще:

```kotlin
private const val ARG_USER_ID = "user_id"
```

або Safe Args/navigation typed routes, якщо доступно.

13. **Практичне правило**

- `Bundle` — для невеликих key-value даних.
- Передавати id, flags, small args, simple state.
- Не передавати великі списки, bitmap, response models.
- Для custom objects використовувати `Parcelable`.
- Для Fragment arguments краще Safe Args або централізовані keys.
- Для process death важливі дані мають бути в persistent storage, не тільки в Bundle.

**Коротко:** `Bundle` — Android key-value контейнер для передачі і збереження невеликих даних між components та lifecycle callbacks. Він зручний для arguments/state, але не призначений для великих обʼєктів або довготривалого зберігання даних.

</details>
<details>
<summary>180. Чи можна вкладати Bundle у Bundle?</summary>

#### Kotlin

Так, `Bundle` можна вкладати в інший `Bundle`. Для цього є методи `putBundle()` і `getBundle()`. Але практично це треба робити обережно: вкладені Bundle ускладнюють контракт, збільшують ризик помилок у ключах і можуть привести до занадто великого payload, якщо зберігати багато даних.

1. **Приклад вкладеного Bundle**

```kotlin
val filtersBundle = Bundle().apply {
    putString("sort", "date")
    putBoolean("only_favorites", true)
}

val screenBundle = Bundle().apply {
    putString("user_id", "42")
    putBundle("filters", filtersBundle)
}
```

Читання:

```kotlin
val filters = screenBundle.getBundle("filters")
val sort = filters?.getString("sort")
val onlyFavorites = filters?.getBoolean("only_favorites") ?: false
```

2. **Коли це може бути доречно**

Вкладений `Bundle` може бути доречним, коли треба логічно згрупувати дані:

```kotlin
val analyticsBundle = bundleOf(
    "source" to "push",
    "campaign" to "winter_sale"
)

val args = bundleOf(
    "product_id" to productId,
    "analytics" to analyticsBundle
)
```

Тут `analytics` — окремий блок metadata.

3. **Fragment arguments**

```kotlin
class ProductFragment : Fragment(R.layout.fragment_product) {
    companion object {
        private const val ARG_PRODUCT_ID = "product_id"
        private const val ARG_ANALYTICS = "analytics"

        fun newInstance(
            productId: String,
            analytics: Bundle
        ): ProductFragment {
            return ProductFragment().apply {
                arguments = bundleOf(
                    ARG_PRODUCT_ID to productId,
                    ARG_ANALYTICS to analytics
                )
            }
        }
    }
}
```

Читання:

```kotlin
val analytics = requireArguments().getBundle(ARG_ANALYTICS)
```

Це працює, але контракт стає менш очевидним, ніж typed arguments.

4. **Проблема string keys**

Вкладені Bundle збільшують кількість string keys:

```kotlin
val sort = args
    .getBundle("filters")
    ?.getString("sort")
```

Ризики:

- typo в ключі;
- неправильний тип;
- nullable result;
- складніше refactoring;
- складніше зрозуміти contract destination.

Краще централізувати keys:

```kotlin
private const val ARG_FILTERS = "filters"
private const val KEY_SORT = "sort"
```

5. **Bundle vs Parcelable data class**

Якщо структура має чіткий контракт, часто краще зробити `Parcelable`:

```kotlin
@Parcelize
data class FilterArgs(
    val sort: String,
    val onlyFavorites: Boolean
) : Parcelable
```

Передача:

```kotlin
val args = bundleOf(
    "user_id" to userId,
    "filters" to FilterArgs(
        sort = "date",
        onlyFavorites = true
    )
)
```

Читання:

```kotlin
val filters = requireArguments()
    .getParcelable<FilterArgs>("filters")
```

Typed model зрозуміліший і безпечніший за вкладену key-value структуру.

6. **Bundle vs Navigation Safe Args**

Якщо використовується Navigation Component, краще Safe Args:

```kotlin
findNavController().navigate(
    ProductFragmentDirections.openProduct(
        productId = productId,
        source = "push"
    )
)
```

Safe Args дає compile-time перевірку типів і ключів. Вкладені Bundle такої перевірки не дають.

7. **Обмеження розміру**

Вкладення Bundle не обходить Binder transaction limit:

```kotlin
val parent = Bundle().apply {
    putBundle("large_data", hugeBundle)
}
```

Якщо передати занадто багато даних через `Intent`, `Fragment arguments` або saved state, можна отримати:

```text
TransactionTooLargeException
```

Тому не треба вкладати великі списки, bitmap або response models.

8. **Bundle у savedInstanceState**

Вкладений Bundle можна зберегти:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)

    outState.putBundle(
        "search_state",
        bundleOf(
            "query" to binding.searchInput.text.toString(),
            "page" to currentPage
        )
    )
}
```

Але це має бути невеликий transient UI state. Для важливих даних потрібна база, DataStore або repository.

9. **Nullable handling**

`getBundle()` повертає nullable:

```kotlin
val filters = requireArguments().getBundle("filters")
```

Якщо Bundle обовʼязковий:

```kotlin
val filters = requireNotNull(requireArguments().getBundle("filters")) {
    "filters argument is required"
}
```

Для optional block:

```kotlin
val filters = requireArguments().getBundle("filters") ?: Bundle.EMPTY
```

10. **Коли краще не вкладати Bundle**

Краще не вкладати `Bundle`, якщо:

- структура складна;
- є багато рівнів вкладеності;
- дані мають чітку модель;
- потрібна type-safety;
- дані великі;
- цей контракт буде використовуватися в багатьох місцях.

Погано:

```kotlin
args.getBundle("user")
    ?.getBundle("profile")
    ?.getBundle("settings")
    ?.getString("theme")
```

Це важко підтримувати.

11. **Практичне правило**

- Технічно вкладати `Bundle` у `Bundle` можна.
- Для простих grouped metadata — нормально.
- Для чітких моделей краще `Parcelable`.
- Для Navigation — краще Safe Args або typed routes.
- Не передавати великі дані.
- Не будувати глибокі nested key-value структури.
- Завжди централізувати keys або використовувати typed API.

**Коротко:** `Bundle` можна вкладати в `Bundle` через `putBundle()`/`getBundle()`, але це варто використовувати тільки для невеликих згрупованих даних. Для складних структур краще `Parcelable`, Safe Args або інший typed contract.

</details>
<details>
<summary>181. Що таке Permissions в Android?</summary>

#### Kotlin

`Permissions` в Android — це механізм контролю доступу застосунку до чутливих ресурсів: камера, мікрофон, геолокація, контакти, storage, notifications тощо. Мета — захистити користувача і дати йому контроль над даними.

1. **Типи permissions**

Основні категорії:

```text
normal      -> видаються автоматично
runtime     -> треба запитувати у користувача
special     -> потребують окремого system screen
signature   -> тільки для app з тим самим signing certificate
```

Наприклад, internet — normal permission, а camera/location — runtime permissions.

2. **Manifest**

Permission треба оголосити в `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

Але для runtime permissions цього недостатньо: їх ще треба запросити в runtime.

3. **Runtime permission flow**

Базовий flow:

```text
check permission -> request permission -> handle result -> use feature
```

Не можна просто викликати camera/location API без перевірки permission.

4. **Запит permission у Compose/Activity**

Через Activity Result API:

```kotlin
val launcher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.RequestPermission()
) { granted ->
    if (granted) {
        viewModel.onCameraPermissionGranted()
    } else {
        viewModel.onCameraPermissionDenied()
    }
}

Button(onClick = { launcher.launch(Manifest.permission.CAMERA) }) {
    Text("Open camera")
}
```

Для кількох permissions використовують `RequestMultiplePermissions()`.

5. **Перевірка permission**

```kotlin
val granted = ContextCompat.checkSelfPermission(
    context,
    Manifest.permission.CAMERA
) == PackageManager.PERMISSION_GRANTED
```

Перевірка потрібна перед доступом до protected API.

6. **Rationale**

Якщо користувач відмовив, можна показати пояснення:

```kotlin
ActivityCompat.shouldShowRequestPermissionRationale(
    activity,
    Manifest.permission.CAMERA
)
```

Rationale має пояснювати користь для користувача, а не тиснути на нього.

7. **Permanent denial**

Якщо користувач вибрав “Don’t ask again”, повторний request не покаже dialog. У такому випадку треба запропонувати перейти в Settings:

```kotlin
val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {
    data = Uri.fromParts("package", context.packageName, null)
}
context.startActivity(intent)
```

8. **Location permissions**

Location має додаткові нюанси:

- fine vs coarse location;
- foreground vs background location;
- approximate location;
- окремий flow для background location.

Background location не варто запитувати одразу, якщо feature може працювати foreground-only.

9. **Notifications**

На Android 13+ notifications потребують runtime permission:

```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

Її треба запитувати перед показом notifications, якщо app target SDK це вимагає.

10. **Storage permissions**

Storage permissions сильно змінились. Для більшості сценаріїв краще використовувати:

- Photo Picker;
- Storage Access Framework;
- app-specific storage;
- MediaStore.

Не варто запитувати broad storage access без реальної потреби.

11. **Практичне правило**

- Запитувати permission тільки перед реальною дією.
- Не просити всі permissions на старті app.
- Пояснювати користувачу причину.
- Обробляти denial і permanent denial.
- Завжди перевіряти permission перед protected API.
- Мінімізувати набір permissions.

**Коротко:** permissions в Android контролюють доступ до чутливих ресурсів. Їх треба оголошувати в manifest, runtime permissions — запитувати через Activity Result API, обробляти grant/deny/permanent deny і просити тільки ті permissions, які реально потрібні конкретній feature.

</details>
<details>
<summary>182. Що таке setContentView?</summary>

#### Kotlin

`setContentView()` — це метод `Activity`, який встановлює UI-контент для вікна Activity. У класичній View/XML системі він inflate-ить layout resource або приймає вже створений `View` і робить його root content view. Після `setContentView()` можна знаходити View через `findViewById` або працювати з ViewBinding.

1. **Базове використання з XML layout**

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

`R.layout.activity_main` — XML layout, який буде inflate-нутий і встановлений як content view Activity.

2. **Що відбувається під капотом**

Спрощено:

```text
Activity.onCreate
 -> setContentView(layoutRes)
 -> LayoutInflater inflate XML
 -> View hierarchy створюється в памʼяті
 -> root View додається у content area Window
```

Після цього View hierarchy стає частиною UI Activity.

3. **setContentView з View**

Можна передати не resource id, а готовий `View`:

```kotlin
val textView = TextView(this).apply {
    text = "Hello"
}

setContentView(textView)
```

Це рідше використовується в production, але корисно для простих custom views або dynamic UI.

4. **setContentView з layout params**

```kotlin
val view = TextView(this).apply {
    text = "Hello"
}

setContentView(
    view,
    ViewGroup.LayoutParams(
        ViewGroup.LayoutParams.MATCH_PARENT,
        ViewGroup.LayoutParams.WRAP_CONTENT
    )
)
```

Так можна явно задати layout params для root view.

5. **findViewById після setContentView**

`findViewById` працює після того, як layout встановлений:

```kotlin
setContentView(R.layout.activity_main)

val title = findViewById<TextView>(R.id.title)
title.text = "Profile"
```

Якщо викликати `findViewById` до `setContentView`, потрібної View ще не буде в hierarchy.

6. **ViewBinding**

З ViewBinding `setContentView()` зазвичай отримує `binding.root`:

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.title.text = "Profile"
    }
}
```

Це type-safe альтернатива `findViewById`.

7. **DataBinding**

З DataBinding:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val binding = DataBindingUtil.setContentView<ActivityMainBinding>(
            this,
            R.layout.activity_main
        )

        binding.lifecycleOwner = this
        binding.viewModel = viewModel
    }
}
```

Тут `DataBindingUtil.setContentView()` одночасно inflate-ить layout, встановлює content view і повертає binding.

8. **Compose аналог**

У Jetpack Compose для Activity використовують Compose `setContent`:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {
            AppTheme {
                ProfileScreen()
            }
        }
    }
}
```

Це не те саме, що XML `setContentView`, але концептуально також встановлює content UI для Activity.

9. **Fragment не використовує setContentView**

У Fragment не викликають `setContentView()`:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

або:

```kotlin
override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View {
    return FragmentProfileBinding.inflate(inflater, container, false).root
}
```

Fragment створює і повертає свою View, а не встановлює content view Activity.

10. **AppCompatActivity constructor layout**

Можна передати layout у constructor:

```kotlin
class MainActivity : AppCompatActivity(R.layout.activity_main)
```

У такому випадку layout буде встановлений framework/appcompat механізмом. Але якщо потрібен ViewBinding, часто все одно використовують явний inflate + `setContentView(binding.root)`.

11. **Повторний setContentView**

Технічно можна викликати `setContentView()` повторно:

```kotlin
setContentView(R.layout.screen_loading)
setContentView(R.layout.screen_content)
```

Але в production це зазвичай погана ідея для зміни станів. Краще мати один layout і перемикати state:

```kotlin
binding.progress.isVisible = state is Loading
binding.content.isVisible = state is Content
```

або використовувати navigation.

12. **Типові помилки**

- Викликати `findViewById` до `setContentView`.
- Викликати `setContentView` у Fragment.
- Повторно встановлювати layout замість state rendering.
- Забути `setContentView(binding.root)` при ViewBinding.
- Використати неправильний layout resource.
- Робити важку логіку під час inflation великих layout-ів.

13. **Практичне правило**

- В XML/View Activity `setContentView(R.layout...)` встановлює root UI.
- З ViewBinding використовувати `setContentView(binding.root)`.
- У Fragment використовувати `onCreateView`, constructor layout або binding inflate.
- У Compose використовувати `setContent { ... }`.
- Після `setContentView` можна безпечно працювати з View hierarchy Activity.

**Коротко:** `setContentView()` встановлює layout або View як основний UI Activity. Це точка, після якої створена View hierarchy доступна для `findViewById`, ViewBinding і подальшої UI-логіки.

</details>
<details>
<summary>183. Які методи існують у View?</summary>

#### Kotlin

`View` — базовий клас Android UI. Його методи відповідають за lifecycle, вимірювання, layout, малювання, input events, state, visibility, focus і accessibility. Для custom View найважливіші: `onMeasure()`, `onDraw()`, `invalidate()`, `requestLayout()`, `onTouchEvent()` і `performClick()`.

1. **Lifecycle attachment**

```kotlin
override fun onAttachedToWindow() {
    super.onAttachedToWindow()
}

override fun onDetachedFromWindow() {
    super.onDetachedFromWindow()
}
```

`onAttachedToWindow()` викликається, коли View додали до window. `onDetachedFromWindow()` — коли прибрали. Тут зазвичай стартують/зупиняють listeners, animations або звільняють ресурси.

2. **onMeasure**

`onMeasure()` визначає розмір View:

```kotlin
override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    val width = resolveSize(200, widthMeasureSpec)
    val height = resolveSize(100, heightMeasureSpec)
    setMeasuredDimension(width, height)
}
```

`MeasureSpec` приходить від parent і задає constraints: `EXACTLY`, `AT_MOST`, `UNSPECIFIED`.

3. **onLayout**

Для простого `View` майже не потрібен. Для `ViewGroup` це метод, який розміщує child views:

```kotlin
override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
    val child = getChildAt(0)
    child.layout(0, 0, child.measuredWidth, child.measuredHeight)
}
```

4. **onDraw**

`onDraw()` малює View на `Canvas`:

```kotlin
class CircleView(context: Context) : View(context) {
    private val paint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.Red
    }

    override fun onDraw(canvas: Canvas) {
        canvas.drawCircle(width / 2f, height / 2f, 50f, paint)
    }
}
```

У `onDraw()` не можна робити важку роботу: allocation великих обʼєктів, bitmap decoding, file/network I/O.

5. **invalidate і requestLayout**

```kotlin
fun setProgress(value: Float) {
    progress = value
    invalidate()
}

fun setItems(items: List<Item>) {
    this.items = items
    requestLayout()
}
```

Правило просте:

```text
змінився тільки вигляд -> invalidate()
змінився розмір/layout -> requestLayout()
```

6. **onTouchEvent і performClick**

```kotlin
override fun onTouchEvent(event: MotionEvent): Boolean {
    return when (event.action) {
        MotionEvent.ACTION_DOWN -> true
        MotionEvent.ACTION_UP -> {
            performClick()
            true
        }
        else -> super.onTouchEvent(event)
    }
}

override fun performClick(): Boolean {
    super.performClick()
    return true
}
```

Якщо View обробляє touch, `ACTION_DOWN` має повернути `true`. Для accessibility click треба проводити через `performClick()`.

7. **Listeners**

```kotlin
view.setOnClickListener {
    viewModel.onItemClicked()
}

view.setOnLongClickListener {
    showContextMenu()
    true
}
```

Listeners виконуються на main thread, тому довгу роботу треба виносити за межі UI callback.

8. **Visibility і state**

```kotlin
view.visibility = View.VISIBLE
view.visibility = View.INVISIBLE
view.visibility = View.GONE
```

```text
VISIBLE   -> видно і займає місце
INVISIBLE -> не видно, але займає місце
GONE      -> не видно і не займає місце
```

Також часто використовуються `isEnabled`, `isSelected`, `isActivated`, `isPressed`.

9. **Focus, state, accessibility**

```kotlin
editText.requestFocus()
view.clearFocus()
view.contentDescription = "Profile image"
```

Focus важливий для input fields, keyboard navigation, TV і accessibility. Custom View також може перевизначати `onSaveInstanceState()` / `onRestoreInstanceState()`, якщо має власний state.

10. **post**

```kotlin
view.post {
    val measuredWidth = view.width
}
```

`post {}` корисний, коли треба виконати код після layout pass, але ним не варто маскувати проблеми архітектури.

11. **Практичне правило**

- Розмір View — `onMeasure()`.
- Розміщення child views — `onLayout()`.
- Малювання — `onDraw()`.
- Перемалювання — `invalidate()`.
- Повний layout pass — `requestLayout()`.
- Touch/click — `onTouchEvent()` + `performClick()`.
- Cleanup — `onDetachedFromWindow()`.
- Accessibility — `contentDescription`, focus, click actions.

**Коротко:** ключові методи `View` відповідають за lifecycle, measure/layout/draw pipeline, input events, state, visibility, focus і accessibility. Для custom View найважливіші `onMeasure`, `onDraw`, `invalidate`, `requestLayout`, `onTouchEvent` і `performClick`.

</details>
<details>
<summary>184. Що таке ViewGroup?</summary>

#### Kotlin

`ViewGroup` — це спеціальний `View`, який може містити інші `View`. Він відповідає за вимірювання, розміщення і керування child views. Усі класичні Android layouts (`LinearLayout`, `FrameLayout`, `ConstraintLayout`, `RecyclerView`) є різними реалізаціями `ViewGroup`.

1. **View vs ViewGroup**

```text
View      -> малює себе
ViewGroup -> містить і розкладає children
```

Наприклад, `TextView` — це `View`, а `LinearLayout` — `ViewGroup`.

2. **Основна відповідальність**

`ViewGroup` має:

- зберігати список child views;
- вимірювати children;
- розміщувати children;
- передавати touch events;
- керувати layout params;
- брати участь у draw/layout pipeline.

3. **onMeasure**

Custom `ViewGroup` має виміряти children і себе:

```kotlin
override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    var totalHeight = paddingTop + paddingBottom
    var maxWidth = 0

    for (i in 0 until childCount) {
        val child = getChildAt(i)
        measureChild(child, widthMeasureSpec, heightMeasureSpec)
        totalHeight += child.measuredHeight
        maxWidth = maxOf(maxWidth, child.measuredWidth)
    }

    setMeasuredDimension(
        resolveSize(maxWidth + paddingLeft + paddingRight, widthMeasureSpec),
        resolveSize(totalHeight, heightMeasureSpec)
    )
}
```

`onMeasure()` визначає, скільки місця потрібно контейнеру.

4. **onLayout**

`onLayout()` розміщує children:

```kotlin
override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
    var top = paddingTop

    for (i in 0 until childCount) {
        val child = getChildAt(i)
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

Якщо не викликати `child.layout(...)`, child не буде правильно показаний.

5. **LayoutParams**

`ViewGroup` може мати власні layout params:

```kotlin
class CustomLayoutParams(
    width: Int,
    height: Int
) : ViewGroup.LayoutParams(width, height)
```

Наприклад, `LinearLayout.LayoutParams` має `weight`, а `ConstraintLayout.LayoutParams` — constraints.

6. **Touch dispatch**

`ViewGroup` бере участь у routing touch events:

```text
dispatchTouchEvent -> onInterceptTouchEvent -> child/onTouchEvent
```

`onInterceptTouchEvent()` дозволяє контейнеру перехопити event у child. Це важливо для scroll containers, `RecyclerView`, `ViewPager`.

7. **requestLayout і invalidate**

```text
requestLayout() -> треба перерахувати measure/layout
invalidate()    -> треба тільки перемалювати
```

Якщо змінився розмір або позиції children — потрібен `requestLayout()`. Якщо змінився тільки вигляд — `invalidate()`.

8. **Приклади ViewGroup**

- `FrameLayout` — накладає children один на одного;
- `LinearLayout` — розкладає по вертикалі або горизонталі;
- `ConstraintLayout` — розкладає за constraints;
- `RecyclerView` — складний ViewGroup для списків;
- `CoordinatorLayout` — координує поведінку child views.

9. **Практичні ризики**

- неправильний `onMeasure()`;
- child не викликає `layout()`;
- ігнорування padding/margins;
- зайві layout passes;
- важка логіка в layout pipeline;
- неправильне touch interception.

10. **Практичне правило**

Custom `ViewGroup` пишуть рідко. У більшості випадків краще використовувати `ConstraintLayout`, `RecyclerView`, Compose або готові containers. Custom layout потрібен, коли стандартні layouts не дають потрібної поведінки або performance.

**Коротко:** `ViewGroup` — це контейнер для інших `View`. Він вимірює children в `onMeasure()`, розміщує їх в `onLayout()`, керує `LayoutParams`, touch dispatch і layout pipeline. Основний ризик — неправильно реалізувати measure/layout або створити зайві layout passes.

</details>
<details>
<summary>185. Для чого потрібен метод onStart() в Activity?</summary>

#### Kotlin

`onStart()` — це lifecycle callback `Activity`, який викликається, коли Activity стає видимою користувачу, але ще не обовʼязково готова до взаємодії. Він іде після `onCreate()` або після повернення з `onStop()`, і перед `onResume()`. Практично `onStart()` використовують для запуску роботи, яка потрібна, поки екран видимий.

1. **Місце onStart у lifecycle**

Типовий старт Activity:

```text
onCreate()
onStart()
onResume()
```

Коли Activity йде в background:

```text
onPause()
onStop()
```

Коли повертається:

```text
onRestart()
onStart()
onResume()
```

Тобто `onStart()` може викликатися багато разів за життя однієї Activity.

2. **Activity стає видимою**

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onStart() {
        super.onStart()
        // Activity is visible
    }
}
```

У цей момент UI вже створений після `onCreate()`, і Activity переходить у visible state.

3. **onStart vs onResume**

```text
onStart  -> Activity видима
onResume -> Activity на передньому плані і готова до interaction
```

Activity може бути visible, але ще не resumed. Наприклад, коли поверх неї є partially transparent Activity або system transition.

4. **Що доречно робити в onStart**

У `onStart()` доречно:

- підписуватися на visible-only listeners;
- стартувати lightweight UI updates;
- реєструвати receivers, які потрібні тільки коли екран видимий;
- запускати lifecycle-aware collection;
- повідомляти analytics, що screen став visible.

Приклад:

```kotlin
override fun onStart() {
    super.onStart()
    analytics.onScreenVisible("Profile")
}
```

5. **Реєстрація receiver**

Якщо receiver потрібен тільки для видимого екрана:

```kotlin
override fun onStart() {
    super.onStart()
    ContextCompat.registerReceiver(
        this,
        syncReceiver,
        IntentFilter(ACTION_SYNC_FINISHED),
        ContextCompat.RECEIVER_NOT_EXPORTED
    )
}

override fun onStop() {
    unregisterReceiver(syncReceiver)
    super.onStop()
}
```

Симетрія важлива: зареєстрували в `onStart()` — зняли в `onStop()`.

6. **Lifecycle-aware collection**

Для Flow краще не вручну стартувати/зупиняти collect, а використовувати `repeatOnLifecycle`:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    lifecycleScope.launch {
        repeatOnLifecycle(Lifecycle.State.STARTED) {
            viewModel.state.collect { state ->
                render(state)
            }
        }
    }
}
```

Цей блок буде активний між `onStart()` і `onStop()`.

7. **Чому STARTED часто краще за RESUMED**

Для UI state collection часто достатньо `STARTED`:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) {
    viewModel.state.collect(::render)
}
```

Екран уже видимий, тому state можна рендерити. `RESUMED` потрібен, коли робота має йти тільки при активній взаємодії користувача.

8. **Що не треба робити в onStart**

Не треба виконувати важку синхронну роботу:

```kotlin
override fun onStart() {
    super.onStart()
    Thread.sleep(5_000)
}
```

`onStart()` виконується на main thread. Довга робота може заблокувати UI і привести до ANR.

Краще:

```kotlin
override fun onStart() {
    super.onStart()
    viewModel.loadIfNeeded()
}
```

А у ViewModel:

```kotlin
fun loadIfNeeded() {
    viewModelScope.launch {
        repository.load()
    }
}
```

9. **onStart після configuration change**

При rotation Activity може бути перестворена:

```text
old Activity: onPause -> onStop -> onDestroy
new Activity: onCreate -> onStart -> onResume
```

Тому код в `onStart()` має бути idempotent: повторний виклик не має ламати стан або дублювати підписки.

10. **onStart і resources**

Якщо ресурс потрібен тільки поки Activity видима, стартувати його можна в `onStart()`, а зупиняти в `onStop()`:

```kotlin
override fun onStart() {
    super.onStart()
    cameraPreview.start()
}

override fun onStop() {
    cameraPreview.stop()
    super.onStop()
}
```

Якщо ресурс потрібен тільки при активній взаємодії, краще `onResume()`/`onPause()`.

11. **onStart у Fragment**

У Fragment теж є `onStart()`, але треба памʼятати про view lifecycle:

```kotlin
override fun onStart() {
    super.onStart()
}
```

Для UI Flow collection у Fragment краще:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Не `lifecycleScope`, а саме `viewLifecycleOwner.lifecycleScope`, якщо робота привʼязана до View.

12. **Практичне правило**

- `onStart()` — Activity стала видимою.
- Парний callback — `onStop()`.
- Реєструвати visible-only ресурси в `onStart()`.
- Звільняти їх у `onStop()`.
- Для Flow використовувати `repeatOnLifecycle(STARTED)`.
- Не виконувати blocking work у `onStart()`.
- Код має бути безпечний до повторних викликів.

**Коротко:** `onStart()` потрібен для запуску логіки, актуальної поки Activity видима. Це правильне місце для visible lifecycle subscriptions і lightweight setup, але не для довгої синхронної роботи.

</details>
<details>
<summary>186. Як можна потрапити в onStart(), але не потрапити в onResume()?</summary>

#### Kotlin

У `onStart()` можна потрапити, але не дійти до `onResume()`, якщо Activity стала видимою або майже видимою, але не отримала foreground focus для взаємодії. Практично це трапляється через overlay/transparent Activity, dialog-like Activity, швидкий перехід у background, finish до resume або коли система перериває lifecycle між `STARTED` і `RESUMED`.

1. **Різниця між STARTED і RESUMED**

```text
onStart()  -> Activity видима
onResume() -> Activity у foreground і готова до interaction
```

`STARTED` не гарантує, що користувач вже може взаємодіяти з Activity. Це лише означає, що Activity стала visible.

2. **Transparent або dialog Activity поверх**

Сценарій:

```text
MainActivity.onStart()
TransparentActivity відкривається поверх
MainActivity не переходить у onResume()
```

Якщо поверх Activity одразу зʼявляється інша Activity, яка забирає focus, нижня Activity може бути visible/started, але не resumed.

Приклад transparent Activity у manifest:

```xml
<activity
    android:name=".OverlayActivity"
    android:theme="@style/Theme.App.Transparent" />
```

Нижній екран може залишатися видимим, але не активним для input.

3. **Dialog-themed Activity**

Activity з dialog theme може не повністю закривати попередню:

```xml
<activity
    android:name=".ConfirmActivity"
    android:theme="@style/Theme.App.Dialog" />
```

Попередня Activity може бути в `STARTED`, бо вона видима позаду, але не `RESUMED`, бо focus у dialog Activity.

4. **Швидкий запуск іншої Activity в onStart**

Якщо в `onStart()` одразу запустити іншу Activity:

```kotlin
override fun onStart() {
    super.onStart()

    if (shouldRedirectToLogin()) {
        startActivity(Intent(this, LoginActivity::class.java))
    }
}
```

Поточна Activity може не дійти до `onResume()`, бо navigation одразу переводить focus на іншу Activity.

5. **finish до onResume**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

Якщо Activity завершується в `onStart()`, `onResume()` може не бути викликаний.

6. **Перехід у background між onStart і onResume**

Система або користувач може перервати lifecycle:

```text
onCreate()
onStart()
user/system opens another app
onStop()
```

Наприклад, приходить system overlay, permission dialog, incoming call, або Activity launch flow швидко змінюється.

7. **Permission/system dialog**

Системний dialog може вплинути на focus:

```kotlin
requestPermissionLauncher.launch(Manifest.permission.CAMERA)
```

Залежно від моменту виклику і transition, Activity може бути started, але foreground interaction тимчасово забирає system UI.

Запит permissions краще робити після зрозумілої user action або коли lifecycle стабільний, а не агресивно в ранніх callbacks.

8. **Multi-window / PiP / focus edge cases**

У multi-window Activity може бути visible, але не resumed/focused. На новіших Android версіях lifecycle behavior для multi-resume став складнішим, але базова ідея лишається: видимість і активний focus — різні речі.

```text
visible != resumed in all cases
```

Тому логіку, яка потребує саме active interaction, не треба вішати тільки на `onStart()`.

9. **Чому це важливо для ресурсів**

Якщо ресурс потрібен, поки екран видимий:

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

Якщо ресурс потрібен тільки коли Activity активна:

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

10. **Flow collection**

Якщо UI має оновлюватися, поки екран видимий:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Якщо потрібна робота тільки в активному foreground:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.RESUMED) {
        viewModel.activeEvents.collect(::handleEvent)
    }
}
```

11. **Не покладатися на ідеальну послідовність**

Не треба писати код з припущенням:

```text
onStart завжди веде до onResume
```

Краще думати так:

```text
onStart означає visible
onResume означає interactive foreground
```

І будувати cleanup симетрично:

```text
onStart  <-> onStop
onResume <-> onPause
```

12. **Практичне правило**

- `onStart()` може викликатися без подальшого `onResume()`.
- Причини: overlay, transparent/dialog Activity, redirect, finish, background interruption, system UI, multi-window.
- Visible-only логіка — `onStart()`/`onStop()`.
- Interaction-only логіка — `onResume()`/`onPause()`.
- Не запускати важку або ризиковану navigation logic без контролю повторних викликів.
- Lifecycle-код має бути стійкий до переривань.

**Коротко:** в `onStart()` можна потрапити без `onResume()`, коли Activity стала видимою, але не отримала foreground focus або була перервана до resume. Найтиповіші сценарії — transparent/dialog Activity поверх, redirect/finish у `onStart()`, системний overlay або швидкий перехід у background.

</details>
<details>
<summary>187. Що таке міграції в Room?</summary>

#### Kotlin

Міграції в Room — це контрольована зміна схеми бази даних між версіями застосунку. Вони потрібні, щоб користувач не втрачав дані після оновлення app.

1. **Коли потрібна міграція**

Міграція потрібна, коли змінюється schema:

- додали таблицю;
- додали/видалили колонку;
- змінили тип колонки;
- перейменували таблицю або поле;
- додали index/foreign key;
- змінили relation між таблицями.

Якщо schema змінилась, треба збільшити `version` у `@Database`.

2. **Версія бази**

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Room порівнює стару і нову version. Якщо немає migration path, app впаде з exception.

3. **Manual migration**

Приклад додавання nullable колонки:

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

4. **Non-null column**

Якщо додається non-null колонка, треба вказати default value:

```kotlin
db.execSQL(
    "ALTER TABLE users ADD COLUMN isActive INTEGER NOT NULL DEFAULT 1"
)
```

Інакше старі rows не матимуть значення, і migration буде некоректною.

5. **Складна міграція**

SQLite не завжди дозволяє напряму змінити column type або constraints. Тоді роблять через temporary table:

```sql
CREATE TABLE users_new (...)
INSERT INTO users_new SELECT ... FROM users
DROP TABLE users
ALTER TABLE users_new RENAME TO users
```

Це типовий підхід для складних schema changes.

6. **AutoMigration**

Room підтримує auto migrations для простих змін:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    autoMigrations = [AutoMigration(from = 1, to = 2)]
)
abstract class AppDatabase : RoomDatabase()
```

AutoMigration підходить не завжди. Для перейменувань або складних змін можуть знадобитись specs або manual migration.

7. **fallbackToDestructiveMigration**

```kotlin
Room.databaseBuilder(context, AppDatabase::class.java, "app.db")
    .fallbackToDestructiveMigration()
    .build()
```

Це видаляє стару DB і створює нову. У production для користувацьких даних це майже завжди погано, бо дані будуть втрачені. Можна використовувати тільки для cache або dev/debug сценаріїв.

8. **Тестування міграцій**

Міграції треба тестувати:

```kotlin
@get:Rule
val helper = MigrationTestHelper(
    InstrumentationRegistry.getInstrumentation(),
    AppDatabase::class.java
)
```

Тест має створити стару schema, вставити дані, виконати migration і перевірити, що дані збережені.

9. **Schema export**

Для нормального тестування треба export schema:

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 2,
    exportSchema = true
)
```

Schema files допомагають Room перевіряти зміни між версіями.

10. **Практичне правило**

- Змінив schema — підняв database version.
- Додав migration path для всіх старих версій.
- Non-null поля додаються з default value.
- Не використовувати destructive migration для user data.
- Міграції тестувати на реальних старих схемах.

**Коротко:** міграції в Room — це опис переходу schema DB між версіями без втрати даних. Вони реалізуються через `Migration`, іноді через `AutoMigration`, підключаються в `Room.databaseBuilder()` і мають тестуватись, особливо якщо дані користувача важливі.

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

`DataStore` — це Jetpack-бібліотека для асинхронного збереження невеликих даних. Вона прийшла на заміну `SharedPreferences` і працює через Kotlin Coroutines та `Flow`.

1. **Для чого потрібен DataStore**

DataStore використовують для app settings і невеликих persistent values:

- theme mode;
- language;
- auth/session flags;
- onboarding completed;
- feature toggles;
- user preferences.

Для великих структур, списків, relational data або offline cache краще використовувати Room.

2. **Preferences DataStore**

Preferences DataStore зберігає key-value дані без schema:

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

Це найпростіший варіант для settings.

3. **Proto DataStore**

Proto DataStore використовує protobuf schema і дає type-safe model:

```proto
message UserSettings {
  bool dark_mode = 1;
  string language = 2;
}
```

Він краще підходить, коли settings мають структуру і потрібна типобезпека.

4. **DataStore vs SharedPreferences**

```text
SharedPreferences -> sync API, може блокувати main thread
DataStore         -> async API, Flow, transactional updates
```

DataStore безпечніший для main thread і краще інтегрується з coroutine-based architecture.

5. **Читання даних**

DataStore повертає `Flow`, тому UI/ViewModel може підписуватись:

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

6. **Запис даних**

Запис виконується через `edit`:

```kotlin
suspend fun saveLanguage(language: String) {
    context.dataStore.edit { preferences ->
        preferences[stringPreferencesKey("language")] = language
    }
}
```

`edit` виконує atomic update, тому не треба вручну синхронізувати доступ.

7. **Error handling**

При читанні можна обробляти `IOException`:

```kotlin
val settings = context.dataStore.data
    .catch { exception ->
        if (exception is IOException) emit(emptyPreferences())
        else throw exception
    }
```

Не варто мовчки ковтати всі exceptions.

8. **Migration з SharedPreferences**

DataStore підтримує міграцію:

```kotlin
val Context.dataStore by preferencesDataStore(
    name = "settings",
    produceMigrations = { context ->
        listOf(SharedPreferencesMigration(context, "old_settings"))
    }
)
```

Це дозволяє перенести старі values без ручного коду на кожному запуску.

9. **Обмеження**

DataStore не підходить для:

- великих таблиць;
- списків доменних обʼєктів;
- складних queries;
- offline-first cache;
- частих масових записів.

Для цього потрібен Room або інше storage-рішення.

10. **Практичне правило**

- Simple key-value settings — Preferences DataStore.
- Type-safe structured settings — Proto DataStore.
- Complex data/cache — Room.
- UI читає через `Flow`.
- Запис робиться через suspend functions.

**Коротко:** DataStore — асинхронна заміна `SharedPreferences` для невеликих persistent settings. Є Preferences DataStore для key-value і Proto DataStore для type-safe schema. Він працює через Coroutines/Flow, підтримує atomic updates і міграцію зі `SharedPreferences`.

</details>
<details>
<summary>192. Що таке Paging 3?</summary>

#### Kotlin

`Paging 3` — це Jetpack-бібліотека для поступового завантаження великих списків даних. Вона допомагає не вантажити весь список одразу, а підвантажувати сторінки за потреби, наприклад під час скролу.

1. **Для чого потрібен Paging 3**

Paging використовують, коли:

- даних багато;
- API повертає сторінки;
- потрібен infinite scroll;
- треба обробляти loading/error/retry;
- список має працювати з cache/DB;
- важлива інтеграція з RecyclerView або Compose.

2. **PagingSource**

`PagingSource` описує, як завантажити одну сторінку:

```kotlin
class UsersPagingSource(
    private val api: UsersApi
) : PagingSource<Int, UserDto>() {
    override suspend fun load(params: LoadParams<Int>): LoadResult<Int, UserDto> {
        val page = params.key ?: 1
        return try {
            val response = api.getUsers(page = page, size = params.loadSize)
            LoadResult.Page(
                data = response.users,
                prevKey = if (page == 1) null else page - 1,
                nextKey = if (response.users.isEmpty()) null else page + 1
            )
        } catch (e: Exception) {
            LoadResult.Error(e)
        }
    }

    override fun getRefreshKey(state: PagingState<Int, UserDto>): Int? =
        state.anchorPosition?.let { position ->
            state.closestPageToPosition(position)?.prevKey?.plus(1)
                ?: state.closestPageToPosition(position)?.nextKey?.minus(1)
        }
}
```

3. **Pager**

`Pager` створює `Flow<PagingData<T>>`:

```kotlin
class UsersRepository(private val api: UsersApi) {
    fun users(): Flow<PagingData<UserDto>> = Pager(
        config = PagingConfig(pageSize = 20),
        pagingSourceFactory = { UsersPagingSource(api) }
    ).flow
}
```

4. **ViewModel**

```kotlin
class UsersViewModel(
    repository: UsersRepository
) : ViewModel() {
    val users = repository.users()
        .cachedIn(viewModelScope)
}
```

`cachedIn(viewModelScope)` кешує paging stream у межах ViewModel і не перезапускає завантаження після rotation.

5. **Compose integration**

```kotlin
val users = viewModel.users.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user ->
            UserRow(user)
        }
    }
}
```

Для production треба додати обробку `loadState`.

6. **LoadState**

Paging має стани loading/error для refresh, append і prepend:

```kotlin
when (val state = users.loadState.refresh) {
    is LoadState.Loading -> LoadingScreen()
    is LoadState.Error -> ErrorScreen(onRetry = { users.retry() })
    is LoadState.NotLoading -> Unit
}
```

`refresh` — початкове завантаження, `append` — підвантаження в кінець списку.

7. **RemoteMediator**

`RemoteMediator` використовують для offline-first paging: API + Room cache.

```text
UI -> PagingData from Room
RemoteMediator -> loads from API -> saves into Room
```

UI читає дані з DB, а mediator синхронізує сторінки з backend.

8. **PagingConfig**

```kotlin
PagingConfig(
    pageSize = 20,
    prefetchDistance = 5,
    enablePlaceholders = false
)
```

- `pageSize` — розмір сторінки;
- `prefetchDistance` — коли починати підвантаження;
- `enablePlaceholders` — чи показувати placeholders для ще не завантажених items.

9. **Типові помилки**

- не використовувати `cachedIn`;
- неправильно рахувати `nextKey`;
- не обробляти `LoadState.Error`;
- змішувати paging і ручний pagination;
- не робити stable keys у Compose/RecyclerView;
- використовувати Paging для маленьких статичних списків.

10. **Практичне правило**

Paging 3 потрібен для великих або потенційно нескінченних списків. Якщо список маленький і вантажиться одним запитом, Paging може бути зайвим.

**Коротко:** Paging 3 — Jetpack-бібліотека для посторінкового завантаження списків. Основні частини: `PagingSource`, `Pager`, `PagingData`, `cachedIn`, `LoadState`, а для API + Room cache — `RemoteMediator`. Вона зменшує ручну pagination-логіку і добре працює з RecyclerView та Compose.

</details>
<details>
<summary>193. Що таке RecyclerView.Adapter?</summary>

#### Kotlin

`RecyclerView.Adapter` — це міст між списком даних і `RecyclerView`. Він відповідає за створення item UI, привʼязку даних до `ViewHolder` і повідомлення `RecyclerView` про зміни. Adapter не має містити business logic або navigation logic; його задача — ефективно показати колекцію моделей.

1. **Основна роль**

```text
Data list -> Adapter -> ViewHolder -> RecyclerView item UI
```

Adapter відповідає на три питання:

- скільки item-ів у списку;
- як створити ViewHolder;
- як заповнити ViewHolder даними.

2. **Мінімальний Adapter**

```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
) : RecyclerView.Adapter<UserViewHolder>() {

    private val items = mutableListOf<User>()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        val binding = ItemUserBinding.inflate(
            LayoutInflater.from(parent.context),
            parent,
            false
        )
        return UserViewHolder(binding, onUserClick)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(items[position])
    }

    override fun getItemCount(): Int = items.size
}
```

Це базова форма, але для production частіше використовують `ListAdapter`.

3. **ViewHolder**

`ViewHolder` тримає item view і вміє bind-ити модель:

```kotlin
class UserViewHolder(
    private val binding: ItemUserBinding,
    private val onUserClick: (User) -> Unit
) : RecyclerView.ViewHolder(binding.root) {

    fun bind(user: User) {
        binding.name.text = user.name
        binding.email.text = user.email
        binding.root.setOnClickListener {
            onUserClick(user)
        }
    }
}
```

ViewHolder потрібен, щоб RecyclerView міг перевикористовувати item views під час scroll.

4. **Recycling**

RecyclerView не створює View для кожного елемента списку. Він тримає тільки видимі item-и і невеликий cache:

```text
visible ViewHolders + cache -> reuse on scroll
```

Коли item виходить за межі екрана, його ViewHolder може бути використаний для іншої позиції. Тому `bind()` має повністю виставляти state.

Погано:

```kotlin
if (user.isAdmin) {
    binding.badge.isVisible = true
}
```

Краще:

```kotlin
binding.badge.isVisible = user.isAdmin
```

Інакше старий state може “протекти” в новий item.

5. **ListAdapter як production default**

```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
) : ListAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        return UserViewHolder(createBinding(parent), onUserClick)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Diff:

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean {
        return oldItem.id == newItem.id
    }

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean {
        return oldItem == newItem
    }
}
```

`ListAdapter` краще за ручний `notifyDataSetChanged()`, бо оновлює тільки змінені item-и.

6. **Кілька типів item**

Для feed/header/loader item-ів використовують `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int {
    return when (items[position]) {
        is FeedItem.Header -> VIEW_TYPE_HEADER
        is FeedItem.Post -> VIEW_TYPE_POST
    }
}
```

Потім у `onCreateViewHolder` створюється відповідний ViewHolder для конкретного типу.

7. **Click handling**

Adapter краще робити dumb:

```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
)
```

Погано, коли adapter сам робить navigation:

```kotlin
navController.navigate(...)
```

Navigation має бути на рівні screen/ViewModel/route callback, а не всередині adapter.

8. **PagingDataAdapter**

Для Paging 3 використовують окремий adapter:

```kotlin
class UserPagingAdapter :
    PagingDataAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        getItem(position)?.let(holder::bind)
    }
}
```

Він працює з `PagingData`, placeholders і load states.

9. **Типові помилки**

- Робити business logic в adapter.
- Використовувати `notifyDataSetChanged()` для всіх змін.
- Не скидати state у `bind()`.
- Тримати `Activity`/`Fragment` reference без потреби.
- Використовувати застарілий `position` у click listener.
- Не використовувати DiffUtil для великих або часто змінних списків.

10. **Практичне правило**

- Adapter — це glue між даними і item UI.
- ViewHolder відповідає за конкретний item layout.
- `onCreateViewHolder` створює item view.
- `onBindViewHolder` заповнює item даними.
- `getItemCount` повертає розмір списку.
- Для production списків краще `ListAdapter`/`DiffUtil`.
- Adapter не має знати про domain logic, navigation або DI graph.

**Коротко:** `RecyclerView.Adapter` керує тим, як дані перетворюються на item views у `RecyclerView`. Він створює `ViewHolder`, bind-ить дані і дозволяє RecyclerView ефективно перевикористовувати views під час scroll.

</details>
<details>
<summary>194. Які основні методи має RecyclerView.Adapter?</summary>

#### Kotlin

Основні методи `RecyclerView.Adapter` — це `onCreateViewHolder()`, `onBindViewHolder()` і `getItemCount()`. Вони визначають, як створюється item UI, як дані bind-яться до `ViewHolder` і скільки item-ів має список. Додатково часто використовують `getItemViewType()`, `getItemId()`, lifecycle callbacks ViewHolder-а і notify-методи.

1. **onCreateViewHolder**

Створює новий `ViewHolder` для конкретного `viewType`:

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

Викликається тоді, коли RecyclerView потрібен новий ViewHolder, а не на кожен bind.

2. **onBindViewHolder**

Привʼязує item data до ViewHolder:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    holder.bind(items[position])
}
```

`bind()` має повністю виставляти state item-а, бо ViewHolder перевикористовується:

```kotlin
fun bind(user: User) {
    binding.name.text = user.name
    binding.badge.isVisible = user.isAdmin
}
```

3. **getItemCount**

Повертає кількість елементів:

```kotlin
override fun getItemCount(): Int = items.size
```

RecyclerView використовує це для layout, scroll range і позицій.

4. **getItemViewType**

Потрібен для різних типів item-ів:

```kotlin
override fun getItemViewType(position: Int): Int {
    return when (items[position]) {
        is FeedItem.Header -> VIEW_TYPE_HEADER
        is FeedItem.Post -> VIEW_TYPE_POST
        is FeedItem.Loader -> VIEW_TYPE_LOADER
    }
}
```

Після цього `onCreateViewHolder()` створює різні ViewHolder-и залежно від `viewType`.

5. **onBindViewHolder з payloads**

Overload для часткових оновлень:

```kotlin
override fun onBindViewHolder(
    holder: UserViewHolder,
    position: Int,
    payloads: MutableList<Any>
) {
    if (payloads.isEmpty()) {
        holder.bind(items[position])
    } else {
        holder.updatePartial(payloads)
    }
}
```

Це корисно, коли змінилася тільки частина item-а, наприклад selected state.

6. **getItemId**

Для stable ids:

```kotlin
init {
    setHasStableIds(true)
}

override fun getItemId(position: Int): Long {
    return items[position].id.hashCode().toLong()
}
```

Stable ids допомагають RecyclerView краще розуміти item identity, але id має бути справді стабільним.

7. **onViewRecycled**

Викликається перед reuse ViewHolder-а:

```kotlin
override fun onViewRecycled(holder: UserViewHolder) {
    holder.clear()
    super.onViewRecycled(holder)
}
```

Корисно для cleanup: скасувати animation, очистити image, прибрати listener, зупинити playback preview.

8. **onViewAttachedToWindow / onViewDetachedFromWindow**

```kotlin
override fun onViewAttachedToWindow(holder: UserViewHolder) {
    super.onViewAttachedToWindow(holder)
    holder.onAttached()
}

override fun onViewDetachedFromWindow(holder: UserViewHolder) {
    holder.onDetached()
    super.onViewDetachedFromWindow(holder)
}
```

Використовується для item-level ресурсів, які мають жити тільки поки item attached.

9. **notify-методи**

Грубий варіант:

```kotlin
notifyDataSetChanged()
```

Краще точкові оновлення:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position)
notifyItemMoved(fromPosition, toPosition)
```

Для range:

```kotlin
notifyItemRangeInserted(start, count)
notifyItemRangeRemoved(start, count)
notifyItemRangeChanged(start, count)
```

У production для звичайних списків краще `ListAdapter`/`DiffUtil`, а не ручні notify-виклики.

10. **ListAdapter submitList**

Якщо використовується `ListAdapter`, `getItemCount()` і diff updates уже реалізовані:

```kotlin
class UserAdapter : ListAdapter<User, UserViewHolder>(UserDiffCallback) {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        return UserViewHolder(createBinding(parent))
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Оновлення:

```kotlin
adapter.submitList(users)
```

11. **Типові помилки**

- Використовувати `notifyDataSetChanged()` замість DiffUtil.
- Не скидати старий state у `bind()`.
- Використовувати нестабільний `getItemId()`.
- Неправильно обробляти `viewType`.
- Робити важку роботу в `onBindViewHolder()`.
- Зберігати старий `position` у click listener.

12. **Практичне правило**

- Обовʼязкові методи: `onCreateViewHolder`, `onBindViewHolder`, `getItemCount`.
- Для різних item layouts — `getItemViewType`.
- Для stable identity — `getItemId` + `setHasStableIds(true)`.
- Для cleanup — `onViewRecycled`.
- Для attach/detach ресурсів — `onViewAttachedToWindow`/`onViewDetachedFromWindow`.
- Для оновлень — `ListAdapter`/`DiffUtil` або точкові notify-методи.

**Коротко:** основні методи `RecyclerView.Adapter` створюють ViewHolder, bind-ять дані й повертають кількість item-ів. Додаткові методи керують типами item-ів, stable ids, recycling lifecycle і точковими оновленнями списку.

</details>
<details>
<summary>195. У чому різниця між RecyclerView.Adapter та ListAdapter?</summary>

#### Kotlin

`RecyclerView.Adapter` — базовий adapter, де ти сам керуєш списком, оновленнями й викликами `notify...`. `ListAdapter` — готовий adapter поверх `AsyncListDiffer`, який сам рахує diff між старим і новим списком через `DiffUtil`.

1. **RecyclerView.Adapter**

```kotlin
class UserAdapter : RecyclerView.Adapter<UserViewHolder>() {
    private val items = mutableListOf<UserUi>()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        return UserViewHolder.create(parent)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(items[position])
    }

    override fun getItemCount(): Int = items.size

    fun setItems(newItems: List<UserUi>) {
        items.clear()
        items.addAll(newItems)
        notifyDataSetChanged()
    }
}
```

Це працює, але `notifyDataSetChanged()` перемальовує весь список і не дає нормальних item animations.

2. **ListAdapter**

```kotlin
class UserAdapter : ListAdapter<UserUi, UserViewHolder>(UserDiffCallback) {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder {
        return UserViewHolder.create(parent)
    }

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}
```

Оновлення списку:

```kotlin
adapter.submitList(users)
```

`ListAdapter` сам порівнює старий і новий список та викликає точні updates.

3. **DiffUtil.ItemCallback**

```kotlin
object UserDiffCallback : DiffUtil.ItemCallback<UserUi>() {
    override fun areItemsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: UserUi, newItem: UserUi): Boolean =
        oldItem == newItem
}
```

`areItemsTheSame` перевіряє identity, а `areContentsTheSame` — чи змінився content.

4. **Головна різниця**

```text
RecyclerView.Adapter -> manual list updates
ListAdapter          -> submitList + DiffUtil + async diff calculation
```

`ListAdapter` зменшує boilerplate і помилки при оновленні списків.

5. **Performance**

`ListAdapter` рахує diff не на main thread, тому краще підходить для динамічних списків. Він оновлює тільки змінені items, а не весь RecyclerView.

6. **Immutability списку**

З `ListAdapter` важливо передавати новий list object:

```kotlin
adapter.submitList(oldList + newUser)
```

Погано мутувати старий список і передавати той самий reference, бо diff може не спрацювати очікувано.

7. **Коли використовувати Adapter**

Звичайний `RecyclerView.Adapter` доречний, коли:

- список дуже специфічний;
- потрібен повний контроль над updates;
- дані майже не змінюються;
- використовується custom differ;
- adapter складний і не вписується в `ListAdapter`.

8. **Коли використовувати ListAdapter**

`ListAdapter` — дефолтний вибір для більшості списків:

- список часто оновлюється;
- потрібні item animations;
- дані приходять з Flow/LiveData;
- треба менше manual notify logic;
- важлива простота і predictability.

9. **Типові помилки**

- використовувати `notifyDataSetChanged()` замість точних updates;
- неправильно реалізувати `areItemsTheSame`;
- мутувати list після `submitList`;
- плутати identity і content;
- не використовувати stable ids, коли вони реально потрібні.

10. **Практичне правило**

Якщо немає сильної причини писати manual update logic, краще брати `ListAdapter`. Він простіший, безпечніший і краще працює з modern Android data flow.

**Коротко:** `RecyclerView.Adapter` дає повний ручний контроль, але ти сам відповідаєш за оновлення списку. `ListAdapter` приймає новий список через `submitList()`, рахує diff через `DiffUtil` у background і оновлює тільки змінені items. Для більшості production-списків краще `ListAdapter`.

</details>
<details>
<summary>196. Як працює RecyclerView під капотом?</summary>

#### Kotlin

`RecyclerView` працює як оптимізований контейнер для великих списків: він не створює View для всіх елементів, а тримає тільки видимі item views і невеликий cache. Під час скролу `ViewHolder`-и перевикористовуються, а `Adapter` заново bind-ить їх до інших позицій.

1. **Головна ідея recycling**

```text
10 000 data items
~10-20 visible item views
small cache/recycled pool
```

RecyclerView створює обмежену кількість `ViewHolder`-ів і перевикористовує їх замість створення нового View для кожного item-а.

2. **Основні учасники**

```text
RecyclerView
 ├── Adapter
 ├── ViewHolder
 ├── LayoutManager
 ├── Recycler / RecycledViewPool
 ├── ItemAnimator
 └── ItemDecoration
```

- `Adapter` створює і bind-ить item UI.
- `ViewHolder` тримає item view.
- `LayoutManager` розміщує items.
- `Recycler` керує reuse/cache.
- `ItemAnimator` анімує зміни.
- `ItemDecoration` додає spacing/dividers/custom drawing.

3. **Перший layout**

Спрощено:

```text
RecyclerView measure/layout
 -> LayoutManager asks Recycler for views
 -> Recycler asks Adapter for ViewHolder if needed
 -> Adapter creates/binds ViewHolder
 -> LayoutManager positions child views
 -> RecyclerView draws children
```

Якщо ViewHolder ще немає — викликається `onCreateViewHolder()` і `onBindViewHolder()`. Якщо ViewHolder можна reuse-ити — тільки `onBindViewHolder()`.

4. **Що відбувається при scroll**

```text
item leaves screen
 -> ViewHolder goes to cache/pool
new item enters screen
 -> old ViewHolder reused
 -> Adapter binds new data
```

Тому `bind()` має повністю виставляти state:

```kotlin
binding.title.text = item.title
binding.badge.isVisible = item.isImportant
binding.checkbox.isChecked = item.isSelected
```

Не можна покладатися на старий стан View, бо ViewHolder міг належати іншому item-у.

5. **LayoutManager**

RecyclerView сам не знає, як розкладати item-и. Це робить `LayoutManager`:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
```

Інші варіанти:

```kotlin
GridLayoutManager(context, spanCount = 2)
StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

6. **View types**

Для різних item layouts:

```kotlin
override fun getItemViewType(position: Int): Int {
    return when (items[position]) {
        is FeedItem.Header -> VIEW_TYPE_HEADER
        is FeedItem.Post -> VIEW_TYPE_POST
    }
}
```

RecyclerView не буде reuse-ити `HeaderViewHolder` як `PostViewHolder`, бо `viewType` різний.

7. **Cache і RecycledViewPool**

У RecyclerView є кілька рівнів reuse:

```text
attached scrap -> тимчасові views під час layout
view cache     -> recent detached views
recycled pool  -> ViewHolders за viewType
```

Це зменшує allocations і робить scroll дешевшим.

8. **Оновлення списку**

Adapter повідомляє RecyclerView про зміни:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position)
```

Краще використовувати `DiffUtil`/`ListAdapter`, щоб RecyclerView отримував точні зміни: insert, remove, move, change. `notifyDataSetChanged()` змушує вважати, що змінився весь список.

9. **ItemAnimator**

```kotlin
recyclerView.itemAnimator = DefaultItemAnimator()
```

Анімує додавання, видалення, переміщення і зміну item-ів. Якщо change animations дають flicker:

```kotlin
(recyclerView.itemAnimator as? SimpleItemAnimator)
    ?.supportsChangeAnimations = false
```

10. **ItemDecoration**

```kotlin
recyclerView.addItemDecoration(
    DividerItemDecoration(context, RecyclerView.VERTICAL)
)
```

Використовується для dividers, spacing, sticky headers або custom drawing без зміни item layout.

11. **Чому onBindViewHolder має бути легким**

Погано:

```kotlin
override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
    val bitmap = BitmapFactory.decodeFile(items[position].imagePath)
    holder.bind(bitmap)
}
```

Це може лагати під час scroll. Краще передати модель, а картинки вантажити через image loader із cache/cancellation.

12. **Position pitfalls**

Position може змінюватися через insert/remove/move. У click listener краще перевіряти актуальну позицію:

```kotlin
binding.root.setOnClickListener {
    val position = bindingAdapterPosition
    if (position != RecyclerView.NO_POSITION) {
        onClick(getItem(position))
    }
}
```

Або передавати конкретний item у `bind()`, якщо adapter працює з immutable snapshots.

13. **Практичне правило**

- RecyclerView створює тільки потрібні ViewHolder-и.
- ViewHolder-и перевикористовуються при scroll.
- Adapter створює і bind-ить item UI.
- LayoutManager відповідає за розміщення.
- DiffUtil/ListAdapter дає точкові оновлення.
- `bind()` має бути швидким і повністю встановлювати state.
- Для різних item layouts використовувати `viewType`.
- Для великих списків не використовувати ручний `LinearLayout.addView()`.

**Коротко:** RecyclerView під капотом працює через reuse `ViewHolder`-ів, layout delegation у `LayoutManager`, cache/pool механізми і точкові adapter updates. Саме recycling і DiffUtil/ListAdapter роблять його ефективним для великих списків.

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

Списки в Android відображають через `RecyclerView` у XML/View-системі або через lazy-контейнери в Jetpack Compose: `LazyColumn`, `LazyRow`, `LazyVerticalGrid`. Для великих або paged списків використовують Paging 3. Вибір залежить від UI stack, обсягу даних і вимог до loading/error/empty states.

1. **RecyclerView у View/XML**

XML:

```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

Fragment:

```kotlin
val adapter = UserAdapter(
    onUserClick = viewModel::onUserClicked
)

binding.recyclerView.layoutManager = LinearLayoutManager(requireContext())
binding.recyclerView.adapter = adapter
```

Оновлення списку:

```kotlin
adapter.submitList(users)
```

Для звичайних списків краще `ListAdapter` + `DiffUtil`, а не ручний `notifyDataSetChanged()`.

2. **Основні layout-и RecyclerView**

Вертикальний список:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(context)
```

Горизонтальний:

```kotlin
recyclerView.layoutManager = LinearLayoutManager(
    context,
    RecyclerView.HORIZONTAL,
    false
)
```

Grid:

```kotlin
recyclerView.layoutManager = GridLayoutManager(context, 2)
```

3. **Paging 3 для великих списків**

Якщо дані приходять сторінками або їх дуже багато:

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
    repository.getUsers()
        .cachedIn(viewModelScope)
```

Fragment:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.users.collectLatest { pagingData ->
            adapter.submitData(pagingData)
        }
    }
}
```

4. **Load/Error/Retry states**

```kotlin
adapter.addLoadStateListener { loadState ->
    binding.progressBar.isVisible = loadState.refresh is LoadState.Loading
    binding.errorView.isVisible = loadState.refresh is LoadState.Error
}

binding.retryButton.setOnClickListener {
    adapter.retry()
}
```

Список без loading/error/empty state — неповний production UI.

5. **Compose LazyColumn**

```kotlin
@Composable
fun UserListScreen(
    users: List<User>,
    onUserClick: (User) -> Unit
) {
    LazyColumn {
        items(
            items = users,
            key = { user -> user.id }
        ) { user ->
            UserItem(
                user = user,
                onClick = { onUserClick(user) }
            )
        }
    }
}
```

`key` важливий для стабільної identity item-а при insert/delete/sort/filter.

6. **Compose LazyRow і Grid**

Горизонтальний список:

```kotlin
LazyRow {
    items(categories, key = { it.id }) { category ->
        CategoryChip(category)
    }
}
```

Grid:

```kotlin
LazyVerticalGrid(
    columns = GridCells.Fixed(2)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

7. **Paging у Compose**

```kotlin
@Composable
fun UserPagingScreen(viewModel: UserViewModel) {
    val users = viewModel.users.collectAsLazyPagingItems()

    LazyColumn {
        items(users.itemCount) { index ->
            users[index]?.let { user ->
                UserItem(user)
            }
        }
    }
}
```

LoadState:

```kotlin
when (users.loadState.refresh) {
    is LoadState.Loading -> LoadingContent()
    is LoadState.Error -> ErrorContent(onRetry = { users.retry() })
    is LoadState.NotLoading -> Unit
}
```

8. **Empty state**

View/XML:

```kotlin
binding.emptyView.isVisible = users.isEmpty()
binding.recyclerView.isVisible = users.isNotEmpty()
```

Compose:

```kotlin
if (users.isEmpty()) {
    EmptyContent()
} else {
    LazyColumn {
        items(users, key = { it.id }) { user ->
            UserItem(user)
        }
    }
}
```

9. **Що не робити**

Погано у View-системі:

```kotlin
items.forEach { item ->
    linearLayout.addView(createItemView(item))
}
```

Погано в Compose для великих списків:

```kotlin
Column {
    users.forEach { user ->
        UserItem(user)
    }
}
```

Для великих списків потрібні `RecyclerView` або lazy-контейнери.

10. **Практичне правило**

- XML/View список — `RecyclerView`.
- Простий RecyclerView список — `ListAdapter`.
- Великий/paged список — Paging 3.
- Compose список — `LazyColumn` або `LazyRow`.
- Compose grid — `LazyVerticalGrid`.
- Для dynamic lists задавати stable `key`.
- Loading, error, empty і retry states мають бути частиною UI.

**Коротко:** у View-системі списки відображають через `RecyclerView` з `Adapter`/`ListAdapter`/`PagingDataAdapter`. У Compose — через `LazyColumn`, `LazyRow` або lazy grids. Для великих або remote списків використовують Paging 3.

</details>
<details>
<summary>199. Що таке lazy-контейнери в Compose?</summary>

#### Kotlin

`Lazy`-контейнери в Jetpack Compose — це компоненти для списків і сіток, які compose-ять тільки видимі елементи та невеликий buffer навколо них. Це аналог ідеї `RecyclerView`, але в declarative Compose API.

1. **Основні lazy-контейнери**

Найчастіше використовують:

```kotlin
LazyColumn { }
LazyRow { }
LazyVerticalGrid { }
LazyHorizontalGrid { }
LazyVerticalStaggeredGrid { }
```

`LazyColumn` — вертикальний список, `LazyRow` — горизонтальний, grid-контейнери — для сіток.

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

Compose не створює всі `UserRow` одразу. Він створює тільки ті, які потрібні для viewport.

3. **Keys**

Для стабільності треба передавати key:

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

Без key Compose може неправильно переиспользувати state елементів після insert/delete/reorder.

4. **contentType**

Якщо в списку різні типи item-ів, варто задавати `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

Це допомагає Compose ефективніше переиспользувати composition для схожих елементів.

5. **LazyListState**

Для контролю scroll position використовують state:

```kotlin
val listState = rememberLazyListState()

LazyColumn(state = listState) {
    items(users, key = { it.id }) { user ->
        UserRow(user)
    }
}
```

Можна читати `firstVisibleItemIndex`, робити scroll або animate scroll:

```kotlin
LaunchedEffect(Unit) {
    listState.animateScrollToItem(0)
}
```

6. **Sticky headers**

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

`stickyHeader` корисний для grouped lists, але треба слідкувати за performance, якщо header складний.

7. **Grids**

```kotlin
LazyVerticalGrid(
    columns = GridCells.Adaptive(minSize = 128.dp)
) {
    items(products, key = { it.id }) { product ->
        ProductCard(product)
    }
}
```

`Adaptive` автоматично підбирає кількість колонок залежно від ширини.

8. **Performance правила**

- використовувати stable keys;
- не створювати важкі обʼєкти всередині item;
- не робити I/O або bitmap decoding у composable;
- виносити expensive calculations у `remember`;
- уникати вкладених scroll containers без потреби;
- передавати immutable/stable models, якщо можливо.

Погано:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Краще:

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

9. **Paging 3**

Lazy-контейнери добре працюють з Paging:

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

У production також обробляють `loadState`: loading, error, retry.

10. **Практичне правило**

Lazy-контейнер потрібен, коли елементів багато або їх кількість динамічна. Якщо елементів 3–5 і вони завжди статичні, звичайний `Column` може бути простішим.

**Коротко:** lazy-контейнери Compose (`LazyColumn`, `LazyRow`, grids) рендерять тільки видимі елементи. Для production важливо використовувати stable `key`, `contentType`, `LazyListState`, уникати важкої роботи в item composables і правильно інтегрувати Paging/loading states.

</details>
<details>
<summary>200. Як реалізувати циклічний список у RecyclerView?</summary>

#### Kotlin

Циклічний список у `RecyclerView` — це список, який імітує нескінченне прокручування: після останнього елемента знову йде перший. Найчастіше це потрібно для carousel/banner UI.

1. **Базова ідея**

Adapter повертає дуже великий `itemCount`, а реальний елемент береться через modulo:

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

Так користувач може скролити дуже довго, а дані повторюються.

2. **Старт із середини**

Щоб можна було скролити і вперед, і назад, список ставлять ближче до середини:

```kotlin
val startPosition = Int.MAX_VALUE / 2
val alignedStart = startPosition - startPosition % items.size
recyclerView.scrollToPosition(alignedStart)
```

Це зменшує шанс дійти до початку або кінця fake range.

3. **Обробка пустого списку**

Обовʼязково перевіряти `items.isEmpty()`, інакше буде ділення на нуль:

```kotlin
override fun getItemCount(): Int =
    if (items.isEmpty()) 0 else Int.MAX_VALUE
```

4. **Stable ids**

Якщо потрібні stable ids:

```kotlin
override fun getItemId(position: Int): Long {
    val realPosition = position % items.size
    return items[realPosition].id
}
```

При цьому в adapter треба викликати:

```kotlin
setHasStableIds(true)
```

5. **SnapHelper для carousel**

Для banner/carousel часто додають snapping:

```kotlin
val snapHelper = PagerSnapHelper()
snapHelper.attachToRecyclerView(recyclerView)
```

`PagerSnapHelper` робить поведінку схожою на pager: один item за раз.

6. **Auto-scroll**

Для автоматичного прокручування можна використовувати coroutine:

```kotlin
lifecycleScope.launch {
    while (isActive) {
        delay(3_000)
        recyclerView.smoothScrollToPosition(
            (recyclerView.layoutManager as LinearLayoutManager)
                .findFirstVisibleItemPosition() + 1
        )
    }
}
```

У production треба зупиняти auto-scroll у `onPause()` / `onDestroyView()` або через lifecycle-aware coroutine.

7. **DiffUtil і оновлення даних**

Якщо дані змінюються, треба перерахувати start position і не забути про modulo. Для складних списків краще мати окремий real list і adapter, який тільки мапить fake position у real position.

8. **Ризики**

- `items.size == 0` і crash через modulo;
- неправильний старт position;
- auto-scroll leak після `onDestroyView()`;
- проблеми зі stable ids;
- accessibility: користувач може не розуміти нескінченний список;
- погана UX-поведінка для великих/важких items.

9. **Альтернативи**

Для деяких задач краще використати:

- `ViewPager2` для pager/banner;
- Compose `HorizontalPager`;
- звичайний finite list без fake infinity;
- duplicated edges, якщо треба тільки seamless transition.

10. **Практичне правило**

Циклічний RecyclerView варто робити тільки там, де це реально покращує UX: банери, carousel, picker. Для звичайних списків нескінченність часто плутає користувача і ускладнює accessibility.

**Коротко:** циклічний список у RecyclerView зазвичай роблять через великий `itemCount` і `position % items.size`. Стартують із середини `Int.MAX_VALUE`, обробляють empty list, додають `SnapHelper` для carousel і уважно керують auto-scroll lifecycle.

</details>
<details>
<summary>201. Що таке анімації в Android?</summary>

#### Kotlin

Анімації в Android — це механізми зміни UI з часом: позиції, прозорості, масштабу, кольору, розміру або стану компонента. Їх використовують, щоб зробити інтерфейс зрозумілішим, а не просто “красивішим”.

1. **Основні типи анімацій**

В Android найчастіше використовують:

- `ViewPropertyAnimator` — прості анімації View;
- `ObjectAnimator` / `ValueAnimator` — property/value animations;
- `AnimatorSet` — комбінація кількох анімацій;
- `MotionLayout` — складні transition між layout states;
- Compose animations — `animate*AsState`, `AnimatedVisibility`, `updateTransition`;
- `RecyclerView.ItemAnimator` — анімації змін у списку.

2. **ViewPropertyAnimator**

Найпростіший спосіб анімувати View:

```kotlin
view.animate()
    .alpha(0f)
    .translationY(100f)
    .setDuration(300)
    .start()
```

Підходить для простих змін: `alpha`, `translationX/Y`, `scaleX/Y`, `rotation`.

3. **ObjectAnimator**

`ObjectAnimator` змінює конкретну property обʼєкта:

```kotlin
ObjectAnimator.ofFloat(view, View.ALPHA, 1f, 0f).apply {
    duration = 300
    start()
}
```

Корисний, коли треба більше контролю або працювати з `AnimatorSet`.

4. **ValueAnimator**

`ValueAnimator` генерує значення, а ти сам вирішуєш, як його застосувати:

```kotlin
ValueAnimator.ofInt(0, 100).apply {
    duration = 300
    addUpdateListener { animator ->
        val value = animator.animatedValue as Int
        view.translationX = value.toFloat()
    }
    start()
}
```

Підходить для custom View, кольорів, progress або нестандартної логіки.

5. **AnimatorSet**

```kotlin
val fade = ObjectAnimator.ofFloat(view, View.ALPHA, 0f, 1f)
val move = ObjectAnimator.ofFloat(view, View.TRANSLATION_Y, 100f, 0f)

AnimatorSet().apply {
    playTogether(fade, move)
    duration = 300
    start()
}
```

Дозволяє запускати анімації разом або послідовно.

6. **MotionLayout**

`MotionLayout` використовують для складних transitions між станами layout. Наприклад: collapsible header, onboarding animation, complex screen transition. Його плюс — декларативний опис start/end states і переходів.

7. **Compose animations**

У Compose анімації привʼязані до state:

```kotlin
val alpha by animateFloatAsState(
    targetValue = if (visible) 1f else 0f,
    label = "alpha"
)

Box(
    Modifier.alpha(alpha)
)
```

Для появи/зникнення зручно використовувати:

```kotlin
AnimatedVisibility(visible = visible) {
    Text("Content")
}
```

Compose-анімація має бути результатом зміни state, а не ручного керування View.

8. **Interpolators / Easing**

Interpolator визначає, як змінюється значення з часом:

- linear — рівномірно;
- accelerate — прискорення;
- decelerate — сповільнення;
- overshoot — вихід за межу й повернення;
- spring — пружинна поведінка.

Правильний easing часто важливіший за тривалість.

9. **Performance**

Безпечніше анімувати properties, які не викликають повний layout pass:

- добре: `alpha`, `translationX/Y`, `scale`, `rotation`;
- обережно: `width`, `height`, margins, layout params.

Погано робити allocations, bitmap decoding або важкі обчислення в animation frame callbacks.

10. **Lifecycle і cancel**

Анімації треба зупиняти, якщо View більше не актуальна:

```kotlin
override fun onDestroyView() {
    view.animate().cancel()
    super.onDestroyView()
}
```

У списках і Fragment це важливо, щоб не отримати візуальні баги або leaks.

11. **Практичне правило**

- Для простих View-анімацій — `view.animate()`.
- Для property animation — `ObjectAnimator`.
- Для custom values — `ValueAnimator`.
- Для складних layout transitions — `MotionLayout`.
- Для Compose — state-driven animation APIs.
- Для performance — анімувати transform/alpha, не layout.

**Коротко:** анімації в Android змінюють UI state з часом. Основні інструменти — `ViewPropertyAnimator`, `ObjectAnimator`, `ValueAnimator`, `AnimatorSet`, `MotionLayout` і Compose animation APIs. Хороша анімація має бути state-driven, короткою, cancellable і не блокувати rendering.

</details>
<details>
<summary>202. У чому різниця між ViewBinding та DataBinding?</summary>

#### Kotlin

`ViewBinding` і `DataBinding` прибирають ручний `findViewById`, але мають різну мету. `ViewBinding` просто генерує type-safe доступ до View з XML. `DataBinding` додатково дозволяє писати expressions у XML, привʼязувати data/ViewModel до layout і використовувати `BindingAdapter`. Для нового XML/View-коду ViewBinding зазвичай кращий default: він простіший, легший і прозоріший.

1. **ViewBinding**

XML:

```xml
<TextView
    android:id="@+id/title"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Activity:

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.title.text = "Hello"
    }
}
```

ViewBinding дає type-safe references на View і не додає logic у XML.

2. **DataBinding**

DataBinding layout має root `<layout>` і `<data>`:

```xml
<layout>
    <data>
        <variable
            name="user"
            type="com.example.User" />
    </data>

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@{user.name}" />
</layout>
```

Activity:

```kotlin
val binding = DataBindingUtil.setContentView<ActivityMainBinding>(
    this,
    R.layout.activity_main
)

binding.user = user
```

Тут XML сам читає `user.name`.

3. **Головна різниця**

```text
ViewBinding:
  - type-safe доступ до View
  - немає XML expressions
  - менше generated complexity
  - простіше debug-ити

DataBinding:
  - XML expressions
  - variables у layout
  - BindingAdapters
  - lifecycle-aware binding з LiveData
  - більше build/runtime complexity
```

4. **ViewBinding у Fragment**

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    private var _binding: FragmentProfileBinding? = null
    private val binding get() = requireNotNull(_binding)

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        _binding = FragmentProfileBinding.bind(view)
        binding.title.text = "Profile"
    }

    override fun onDestroyView() {
        _binding = null
        super.onDestroyView()
    }
}
```

У Fragment binding треба очищати в `onDestroyView()`, бо View lifecycle коротший за Fragment lifecycle.

5. **DataBinding у Fragment**

```kotlin
class ProfileFragment : Fragment() {
    private var _binding: FragmentProfileBinding? = null
    private val binding get() = requireNotNull(_binding)

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentProfileBinding.inflate(inflater, container, false)
        binding.lifecycleOwner = viewLifecycleOwner
        binding.viewModel = viewModel
        return binding.root
    }

    override fun onDestroyView() {
        _binding = null
        super.onDestroyView()
    }
}
```

`lifecycleOwner` потрібен, щоб binding міг lifecycle-aware працювати з observable data.

6. **BindingAdapter**

DataBinding підтримує custom XML attributes:

```kotlin
@BindingAdapter("avatarUrl")
fun ImageView.bindAvatar(url: String?) {
    load(url)
}
```

XML:

```xml
<ImageView
    android:layout_width="48dp"
    android:layout_height="48dp"
    app:avatarUrl="@{user.avatarUrl}" />
```

Це зручно, але може приховувати логіку, якщо зловживати.

7. **Чому ViewBinding частіше кращий default**

ViewBinding:

- менше boilerplate, ніж `findViewById`;
- не змішує UI logic з XML;
- легше читати flow даних;
- менше generated-code проблем;
- зазвичай швидший і простіший для build.

Типовий render:

```kotlin
fun render(state: ProfileState) {
    binding.name.text = state.name
    binding.progress.isVisible = state.isLoading
}
```

8. **Коли DataBinding має сенс**

DataBinding доречний, якщо:

- проєкт уже побудований на DataBinding;
- команда активно використовує XML expressions;
- є legacy MVVM з LiveData в XML;
- потрібні `BindingAdapter`-и;
- є чіткі правила, що не можна писати складну business logic у XML.

9. **Compose-контекст**

У Compose ці інструменти не потрібні для UI:

```kotlin
@Composable
fun ProfileScreen(state: ProfileState) {
    Text(text = state.name)
}
```

У mixed проєктах ViewBinding може лишатися для XML screens, а новий UI поступово йти в Compose.

10. **Типові помилки**

ViewBinding:

- не очищати binding у Fragment;
- використовувати binding після `onDestroyView()`;
- тримати binding у ViewModel.

DataBinding:

- писати складну логіку в XML;
- зловживати `BindingAdapter`;
- забути `lifecycleOwner`;
- отримувати важкі для читання generated-code errors.

11. **Практичне правило**

- Потрібен тільки доступ до View — ViewBinding.
- Потрібні XML expressions/BindingAdapters — DataBinding.
- Для нового XML/View коду ViewBinding зазвичай кращий default.
- У Fragment завжди чистити binding у `onDestroyView()`.
- Не писати business logic у XML.
- У Compose ViewBinding/DataBinding для composable UI не потрібні.

**Коротко:** ViewBinding — простий type-safe доступ до View. DataBinding — ширший binding framework з XML expressions і BindingAdapters. Для більшості нового Android XML-коду ViewBinding простіший, безпечніший і дешевший у підтримці.

</details>
<details>
<summary>203. Що таке Serializable?</summary>

#### Kotlin

`Serializable` — це Java-механізм серіалізації, який дозволяє перетворити обʼєкт у послідовність байтів і потім відновити його назад. В Android `Serializable` можна використовувати для передачі обʼєктів через `Intent`/`Bundle`, але для Android-specific передачі даних зазвичай краще `Parcelable`, бо він швидший і оптимізований під Android.

1. **Базове визначення**

Клас роблять serializable так:

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

Тепер обʼєкт можна покласти в `Bundle` або `Intent`:

```kotlin
intent.putExtra("user", user)
```

2. **Отримання Serializable**

```kotlin
val user = intent.getSerializableExtra("user") as? User
```

На новіших Android API краще використовувати typed overload, якщо доступний:

```kotlin
val user = intent.getSerializableExtra("user", User::class.java)
```

Але часто треба враховувати version-specific API і робити compatibility wrapper.

3. **Як це працює концептуально**

```text
Object -> serialization -> bytes
bytes -> deserialization -> Object
```

Серіалізація потрібна, коли обʼєкт треба:

- передати;
- зберегти;
- відновити;
- перенести між процесами або компонентами.

4. **Serializable в Android Bundle**

```kotlin
val bundle = Bundle().apply {
    putSerializable("user", user)
}
```

Читання:

```kotlin
val user = bundle.getSerializable("user") as? User
```

Це працює, але для Android navigation/arguments краще частіше використовувати `Parcelable` або передавати тільки id.

5. **Serializable vs Parcelable**

```text
Serializable:
  - Java standard
  - простіше підключити
  - reflection-based
  - повільніший
  - більше overhead

Parcelable:
  - Android-specific
  - швидший
  - оптимізований для IPC/Bundle
  - потребує опису parceling
  - з @Parcelize дуже простий
```

Parcelable:

```kotlin
@Parcelize
data class UserArgs(
    val id: String,
    val name: String
) : Parcelable
```

Для Android `Parcelable` зазвичай кращий вибір.

6. **serialVersionUID**

У Java Serializable є `serialVersionUID`, який контролює сумісність версій класу:

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

Якщо структура класу змінилась, deserialization старих даних може стати проблемною.

7. **Проблема зміни моделі**

Було:

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

Стало:

```kotlin
data class User(
    val id: String,
    val name: String,
    val email: String
) : Serializable
```

Якщо старий serialized object відновлюється в нову модель, можуть бути compatibility issues. Тому Serializable погано підходить для довготривалого persistent storage без контрольованої schema.

8. **Не передавати великі обʼєкти**

Погано:

```kotlin
intent.putExtra("response", hugeResponse as Serializable)
```

Проблеми:

- повільно;
- великий memory overhead;
- ризик `TransactionTooLargeException`;
- складна сумісність;
- передача зайвих даних між screens.

Краще:

```kotlin
intent.putExtra("user_id", user.id)
```

А дані отримати з repository/cache/database.

9. **Serializable не для domain persistence**

Не треба використовувати Serializable як основний формат збереження domain data:

```kotlin
file.writeBytes(serialize(user))
```

Для storage краще:

- Room;
- DataStore;
- JSON/Proto з явною schema;
- backend;
- files з контрольованим форматом.

Serializable слабко контрольований для довгого життя даних.

10. **Serializable і безпека**

Deserialization загалом може бути небезпечною, якщо дані приходять з недовіреного джерела. В Android screen arguments зазвичай internal, але все одно не треба deserializе-ити довільні зовнішні дані без контролю.

11. **Коли Serializable допустимий**

Serializable може бути допустимий:

- для простих тимчасових аргументів;
- у legacy-коді;
- для швидкого прототипу;
- коли performance не критична;
- коли обʼєкт маленький і internal.

Але для production Android navigation краще:

- primitive args;
- ids;
- `Parcelable`;
- Safe Args;
- typed routes.

12. **Приклад кращого підходу**

Замість:

```kotlin
intent.putExtra("user", user)
```

Краще:

```kotlin
intent.putExtra("user_id", user.id)
```

У destination:

```kotlin
val userId = requireNotNull(intent.getStringExtra("user_id"))
viewModel.loadUser(userId)
```

Це зменшує payload і робить screen більш стійким до process death.

13. **Практичне правило**

- `Serializable` — Java serialization marker interface.
- В Android може передаватися через `Intent`/`Bundle`.
- Для Android IPC/navigation краще `Parcelable`.
- Не передавати великі objects.
- Не використовувати як довготривалий storage format.
- Найкраще між screens передавати ids і завантажувати дані з repository.

**Коротко:** `Serializable` дозволяє перетворити обʼєкт у байти і передати/відновити його, але в Android це зазвичай менш ефективний варіант, ніж `Parcelable`. Для navigation краще передавати мінімальні arguments, найчастіше id.

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

Якщо поле може бути відсутнім у відповіді API, це треба явно відобразити в DTO-моделі: зробити поле nullable, задати default value або обробити custom serialization. Головне — не припускати, що backend завжди поверне ідеальний JSON.

1. **Nullable поле**

Якщо поле справді опціональне:

```kotlin
data class UserDto(
    val id: String,
    val name: String?,
    val avatarUrl: String?
)
```

Потім у mapper треба перетворити DTO у domain/UI model:

```kotlin
fun UserDto.toDomain(): User = User(
    id = id,
    name = name ?: "Unknown",
    avatarUrl = avatarUrl
)
```

2. **Default value**

Якщо відсутнє поле має мати дефолтне значення:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String = "Unknown",
    val isActive: Boolean = true
)
```

Default value корисний, коли backend може не прислати поле, але app має стабільну fallback-поведінку.

3. **Відсутнє поле vs null**

Це різні випадки:

```json
{}
```

і:

```json
{ "name": null }
```

У DTO треба розуміти, чи backend не присилає поле взагалі, чи присилає `null`. Різні serializers можуть обробляти це по-різному.

4. **Kotlin Serialization**

```kotlin
@Serializable
data class ProfileDto(
    val id: String,
    val displayName: String? = null
)
```

Якщо поле має default value, Kotlin Serialization може нормально декодувати JSON без цього поля. Для nullable без default поведінка залежить від налаштувань і схеми.

5. **Moshi/Gson**

З Moshi/Gson nullable поля зазвичай стають `null`, якщо поле відсутнє. Але для non-null Kotlin properties без default можна отримати exception або некоректну модель, залежно від adapter/codegen.

Тому для API DTO краще не робити non-null поле, якщо backend не гарантує його на 100%.

6. **Mapper layer**

DTO не треба напряму використовувати в UI. Краще мати mapper:

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

Mapper — місце, де вирішується fallback logic.

7. **Required fields**

Якщо поле критичне для роботи, краще не маскувати проблему:

```kotlin
fun UserDto.toDomain(): User {
    val safeId = requireNotNull(id) { "User id is required" }
    return User(id = safeId)
}
```

Але це рішення залежить від домену: іноді треба fallback, іноді — fail fast.

8. **Sealed result / error handling**

Якщо API може бути неповним, repository може повертати результат:

```kotlin
sealed interface LoadUserResult {
    data class Success(val user: User) : LoadUserResult
    data class InvalidResponse(val reason: String) : LoadUserResult
}
```

Так UI не працює з “напіввалідною” моделлю без розуміння проблеми.

9. **Практичне правило**

- Optional field — nullable або default value.
- Required field — non-null і fail fast при відсутності.
- DTO не показувати напряму в UI.
- Fallback logic тримати в mapper/domain layer.
- Перевірити поведінку serializer для missing vs null.
- Контракт API бажано зафіксувати в документації або tests.

**Коротко:** якщо поле може бути відсутнім у API, його треба моделювати як nullable або з default value, а fallback обробляти в mapper layer. Для критичних полів краще fail fast або явно повертати error result. DTO не варто напряму використовувати в UI.

</details>
<details>
<summary>206. Що таке Dependency Injection?</summary>

#### Kotlin

`Dependency Injection` — це підхід, коли клас не створює свої залежності сам, а отримує їх ззовні. Це робить код слабше звʼязаним, тестованим і простішим для заміни implementation.

1. **Проблема без DI**

```kotlin
class UserRepository {
    private val api = UserApi()
}
```

Тут `UserRepository` сам створює `UserApi`. Це погано, бо:

- складно підмінити API в тестах;
- клас жорстко привʼязаний до implementation;
- важче контролювати lifecycle залежностей;
- складніше змінювати конфігурацію.

2. **Constructor injection**

Краще передавати залежність через constructor:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

Тепер клас залежить не від способу створення `UserApi`, а тільки від готового обʼєкта.

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

UI/domain можуть залежати від `UserRepository`, а не від `UserRepositoryImpl`.

4. **DI в Android**

В Android DI потрібен для ViewModel, repositories, use cases, API clients, databases, dispatchers та інших shared dependencies.

Приклад з Hilt:

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
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

Так DI container знає, яку implementation використовувати для interface.

6. **Providing external dependencies**

Для обʼєктів, які не можна створити через constructor injection, використовують `@Provides`:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient = OkHttpClient.Builder().build()
}
```

Це типово для Retrofit, Room, OkHttp, Gson/Moshi.

7. **Scopes**

DI дозволяє контролювати lifecycle:

```kotlin
@Singleton
class SessionManager @Inject constructor()
```

У Hilt є scopes рівня application, activity, fragment, ViewModel. Scope треба вибирати обережно, щоб не створити memory leak або непотрібний singleton.

8. **Тестування**

DI спрощує тести:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

У тесті можна передати fake замість real API implementation.

9. **Manual DI**

DI не обовʼязково означає Hilt/Dagger/Koin. Малий проєкт може мати manual DI:

```kotlin
class AppContainer {
    val api = UserApi()
    val repository = UserRepositoryImpl(api)
}
```

Framework потрібен тоді, коли dependency graph стає великим.

10. **Практичне правило**

- Клас не має створювати складні залежності сам.
- Залежності краще передавати через constructor.
- Domain/UI мають залежати від contracts, а не concrete implementations.
- DI framework — інструмент, а не архітектура.
- Scopes мають відповідати lifecycle.

**Коротко:** Dependency Injection — це передача залежностей ззовні замість створення їх всередині класу. В Android DI використовують для ViewModel, use cases, repositories, API, DB і shared services. Він зменшує coupling, спрощує тестування і допомагає контролювати lifecycle обʼєктів.

</details>
<details>
<summary>207. У чому різниця між Dagger/Hilt та Koin?</summary>

#### Kotlin

Dagger/Hilt і Koin — це DI-рішення для Kotlin/Android, але вони працюють по-різному. Dagger/Hilt генерують dependency graph на compile time, а Koin будує graph переважно в runtime через Kotlin DSL.

1. **Dagger**

`Dagger` — compile-time DI framework. Він генерує код і перевіряє graph під час компіляції.

Плюси:

- compile-time validation;
- висока performance;
- явний dependency graph;
- добре підходить для великих enterprise Android apps.

Мінуси:

- більше boilerplate;
- складніший learning curve;
- помилки компіляції можуть бути важкими для читання.

2. **Hilt**

`Hilt` — надбудова над Dagger для Android. Він спрощує DI setup і дає готові Android scopes/components.

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val useCase: LoadProfileUseCase
) : ViewModel()
```

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

Hilt зменшує boilerplate Dagger і краще інтегрується з Android lifecycle.

3. **Koin**

`Koin` — Kotlin DSL для dependency injection/service location style:

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { LoadUserUseCase(get()) }
    viewModel { ProfileViewModel(get()) }
}
```

Плюси:

- простий старт;
- мало boilerplate;
- Kotlin DSL;
- зручно для малих/середніх проєктів.

Мінуси:

- частина помилок проявляється runtime;
- нижча compile-time safety;
- graph може бути менш явним у великих apps.

4. **Compile-time vs runtime**

```text
Dagger/Hilt -> graph перевіряється при компіляції
Koin        -> багато помилок видно тільки під час запуску
```

Наприклад, якщо залежність не оголошена, Dagger/Hilt зламає build, а Koin може впасти runtime exception.

5. **Performance**

Dagger/Hilt зазвичай швидший, бо використовує generated code. Koin робить lookup через runtime container. Для більшості середніх apps Koin може бути достатнім, але у великих проєктах Dagger/Hilt має кращу predictability.

6. **Scopes**

Hilt має Android-aware scopes:

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Koin теж підтримує scopes, але їх треба організовувати через Koin DSL і lifecycle integration.

7. **Тестування**

Обидва дозволяють підміняти залежності в тестах.

Hilt:

```kotlin
@TestInstallIn(...)
@Module
object FakeModule
```

Koin:

```kotlin
loadKoinModules(module {
    single<UserRepository> { FakeUserRepository() }
})
```

Koin часто простіший у тестовому setup, Hilt — більш строгий і формальний.

8. **Що вибрати**

- Великий Android-проєкт, багато команд, потрібна строгість — Hilt.
- Legacy Dagger project — лишатись на Dagger/Hilt.
- Малий/середній проєкт, швидкий старт — Koin.
- Потрібна максимальна compile-time safety — Hilt/Dagger.
- Потрібна простота і мінімум ceremony — Koin.

9. **Практичне правило**

Для production Android у великій команді я б частіше обрав Hilt, бо compile-time validation і стандартна Android integration зменшують ризики. Koin нормальний вибір, якщо команда розуміє runtime graph risks і проєкт не має надмірно складного dependency graph.

**Коротко:** Dagger/Hilt — compile-time DI з generated code, високою строгістю і кращою predictability для великих Android apps. Koin — runtime DI через Kotlin DSL, простіший у старті, але менш compile-time safe. Hilt зазвичай краще для великих production-проєктів, Koin — для простоти й швидкого розвитку.

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

`CompositionLocal` — це механізм Compose для неявної передачі значень вниз по composition tree. Його використовують для dependency-like значень, які потрібні багатьом composables: theme, density, configuration, localization, permissions helpers, design system tokens.

1. **Навіщо потрібен CompositionLocal**

Замість передачі параметра через багато рівнів:

```kotlin
App(theme, user, analytics) {
    Screen(theme, user, analytics)
}
```

можна надати значення через composition:

```kotlin
CompositionLocalProvider(LocalAnalytics provides analytics) {
    AppContent()
}
```

А нижче прочитати:

```kotlin
val analytics = LocalAnalytics.current
```

2. **staticCompositionLocalOf**

```kotlin
val LocalAppColors = staticCompositionLocalOf<AppColors> {
    error("No AppColors provided")
}
```

`staticCompositionLocalOf` використовують, коли значення рідко змінюється або зміна має recomposition-ити широку частину tree. Типовий приклад — design system/theme values.

3. **compositionLocalOf**

```kotlin
val LocalUser = compositionLocalOf<User?> { null }
```

`compositionLocalOf` відстежує reads точніше. Якщо значення зміниться, Compose може recomposition-ити тільки ті composables, які читали `LocalUser.current`.

4. **Різниця між ними**

```text
staticCompositionLocalOf -> дешевший read, менш точне invalidation
compositionLocalOf       -> точніше invalidation, корисно для змінних значень
```

Для теми часто підходить `staticCompositionLocalOf`. Для значень, які можуть змінюватись частіше, краще `compositionLocalOf`.

5. **CompositionLocalProvider**

```kotlin
CompositionLocalProvider(
    LocalAppColors provides darkColors
) {
    Content()
}
```

Provider задає значення для всіх composables нижче в tree.

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

```kotlin
@Composable
fun CardContent() {
    val spacing = LocalSpacing.current
    Column(Modifier.padding(spacing.medium)) {
        Text("Content")
    }
}
```

7. **Built-in CompositionLocals**

Compose вже має багато built-in locals:

- `LocalContext`;
- `LocalConfiguration`;
- `LocalDensity`;
- `LocalLayoutDirection`;
- `LocalLifecycleOwner`;
- `LocalFocusManager`;
- `LocalSoftwareKeyboardController`.

Вони дають доступ до context/configuration/lifecycle без ручного прокидування параметрів.

8. **Коли не використовувати**

CompositionLocal не має замінювати явні параметри всюди. Погано ховати важливі screen dependencies:

```kotlin
val repository = LocalRepository.current // погано для бізнес-логіки UI
```

Для ViewModel/use cases/repositories краще DI і явні залежності.

9. **Ризики**

- приховані залежності;
- складніше тестувати;
- незрозуміло, звідки береться value;
- випадкова recomposition великого subtree;
- використання як service locator.

10. **Практичне правило**

CompositionLocal доречний для контекстних UI-значень, які логічно доступні всьому subtree. Якщо залежність важлива для бізнес-логіки або конкретного компонента — краще передати її явно параметром або через ViewModel/DI.

**Коротко:** `CompositionLocal` передає значення вниз по Compose tree без явних параметрів. Є `staticCompositionLocalOf` для рідко змінних/глобальних UI values і `compositionLocalOf` для значень з точнішим invalidation. Використовувати варто для UI context/theme/design tokens, але не як service locator для бізнес-залежностей.

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

`Side effects` у Compose — це дії, які виходять за межі чистого рендерингу UI: запуск coroutine, підписка на listener, navigation, snackbar, analytics, робота з lifecycle або синхронізація зовнішнього state. У Compose composable може викликатись багато разів, тому side effects треба виконувати через спеціальні APIs, а не напряму в тілі composable.

1. **Чому не можна робити side effect напряму**

Погано:

```kotlin
@Composable
fun ProfileScreen(userId: String) {
    analytics.track("profile_opened") // може викликатись багато разів
    Text(userId)
}
```

Composable може recomposition-итись часто. Якщо side effect стоїть напряму в body, він може повторюватись неконтрольовано.

2. **LaunchedEffect**

`LaunchedEffect` запускає coroutine, привʼязану до composition:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Якщо `userId` зміниться, попередня coroutine скасується і запуститься нова. Це підходить для loading, collecting one-shot effects, scroll actions.

3. **rememberCoroutineScope**

Використовується, коли coroutine треба запускати з callback:

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

На відміну від `LaunchedEffect`, coroutine стартує не автоматично, а в реакції на дію.

4. **DisposableEffect**

`DisposableEffect` потрібен для підписок і cleanup:

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

Якщо є listener/observer, майже завжди потрібен cleanup у `onDispose`.

5. **SideEffect**

`SideEffect` виконується після успішної recomposition:

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Підходить для синхронізації Compose state з обʼєктами, які не є Compose state. Але для довгих задач або coroutine його не використовують.

6. **rememberUpdatedState**

Допомагає не перезапускати effect, але мати актуальну lambda/value:

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(1_000)
    currentOnTimeout()
}
```

Корисно для timers, callbacks і long-running effects.

7. **produceState**

`produceState` конвертує async source у Compose `State`:

```kotlin
val user by produceState<User?>(initialValue = null, userId) {
    value = repository.loadUser(userId)
}
```

Часто в production замість цього використовують `Flow` + `collectAsStateWithLifecycle()`.

8. **derivedStateOf**

`derivedStateOf` потрібен для derived state, щоб не перераховувати значення зайвий раз:

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Це не “звичайний if”, а optimization для state, який часто змінюється.

9. **snapshotFlow**

`snapshotFlow` перетворює Compose state у Flow:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .collect { index -> analytics.trackScroll(index) }
}
```

Корисно для analytics, pagination triggers або реакції на scroll state.

10. **Практичне правило**

- Coroutine при вході в composition — `LaunchedEffect`.
- Coroutine з click/callback — `rememberCoroutineScope`.
- Listener з cleanup — `DisposableEffect`.
- Синхронізація після recomposition — `SideEffect`.
- Актуальна lambda без restart — `rememberUpdatedState`.
- Async data у State — `produceState`.
- Derived value — `derivedStateOf`.
- Compose state у Flow — `snapshotFlow`.

**Коротко:** side effects у Compose — це зовнішні дії, які не є чистим рендерингом UI. Їх треба запускати через `LaunchedEffect`, `DisposableEffect`, `SideEffect`, `rememberCoroutineScope`, `rememberUpdatedState`, `produceState`, `derivedStateOf` або `snapshotFlow`, щоб контролювати lifecycle, restart і cleanup.

</details>
<details>
<summary>212. Як оптимізувати recomposition?</summary>

#### Kotlin

Оптимізація recomposition у Compose — це не спроба “вимкнути” recomposition, а зробити її локальною, дешевою і передбачуваною. Нормальна модель: state змінюється → Compose перемальовує тільки залежні частини UI. Проблеми починаються, коли state лежить занадто високо, моделі нестабільні або в composable виконується важка робота.

1. **Тримати state на правильному рівні**

State треба hoist-ити тільки настільки високо, наскільки потрібно.

Погано, коли зміна тексту в search field змушує recompose-ити великий екран:

```kotlin
@Composable
fun Screen() {
    var query by remember { mutableStateOf("") }

    Header()
    SearchField(query, onQueryChange = { query = it })
    HugeContent()
}
```

Краще ізолювати локальний state:

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

2. **Розбивати UI на менші composable**

```kotlin
@Composable
fun ProfileScreen(state: ProfileState) {
    ProfileHeader(state.user)
    ProfileTabs(state.selectedTab)
    ProfileContent(state.content)
    ProfileDialogs(state.dialogState)
}
```

Так Compose легше skip-ає частини UI, параметри яких не змінилися.

3. **Використовувати immutable/stable моделі**

Погано:

```kotlin
class UserUiModel(
    var name: String,
    var isSelected: Boolean
)
```

Краще:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

Такі моделі простіше порівнювати, безпечніше передавати в composable і легше використовувати для skip recomposition.

4. **Не мутувати списки in-place**

Погано:

```kotlin
users.add(newUser)
```

Краще створювати новий snapshot:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

Composable API краще приймає `List<T>`, а не `MutableList<T>`.

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

`key` привʼязує remembered state і item identity до конкретного елемента, а не до позиції. Це критично при insert/delete/sort/filter.

6. **Використовувати contentType для mixed lists**

```kotlin
LazyColumn {
    items(
        items = feedItems,
        key = { it.id },
        contentType = { it::class }
    ) { item ->
        FeedItem(item)
    }
}
```

`contentType` допомагає Compose ефективніше перевикористовувати item compositions одного типу.

7. **Не робити важку роботу в composable body**

Погано:

```kotlin
@Composable
fun UserList(users: List<User>) {
    val sorted = users.sortedBy { it.name }
    LazyColumn {
        items(sorted) { UserItem(it) }
    }
}
```

Краще підготувати дані у ViewModel або кешувати обчислення:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

Але якщо це business/UI transformation для всього екрана — краще винести у ViewModel/use case mapper.

8. **remember для дорогих обʼєктів**

```kotlin
val formatter = remember {
    DateTimeFormatter.ofPattern("dd.MM.yyyy")
}
```

Якщо значення залежить від параметрів, додати keys:

```kotlin
val filteredUsers = remember(users, query) {
    users.filter { it.name.contains(query, ignoreCase = true) }
}
```

9. **derivedStateOf для часто змінного state**

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

Scroll state змінюється часто, але button visibility змінюється рідко. `derivedStateOf` зменшує зайві invalidations.

10. **Lifecycle-aware collection**

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Це краще для Android, ніж collect без lifecycle, бо UI не буде оновлюватися, коли screen неактивний.

11. **Не створювати state без remember**

Погано:

```kotlin
@Composable
fun Counter() {
    val count = mutableStateOf(0)
}
```

Правильно:

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }
}
```

12. **Профілювати, а не гадати**

Для реальної оптимізації дивитися:

- Layout Inspector recomposition counters;
- Compose compiler reports;
- Android Studio Profiler;
- System Trace;
- Macrobenchmark;
- baseline profiles.

Високий recomposition count не завжди проблема, якщо composable дешевий. Проблема — дорога робота на main thread, зайвий layout/draw або нестабільні параметри.

13. **Практичне правило**

- State тримати на найнижчому достатньому рівні.
- UI розбивати на маленькі composable.
- Передавати immutable/stable UI models.
- Не мутувати списки in-place.
- У lazy lists використовувати `key`, а для mixed lists ще й `contentType`.
- Heavy calculations робити у ViewModel або кешувати через `remember`.
- Scroll-derived state обгортати в `derivedStateOf`.
- Side effects запускати тільки через effect APIs.
- Спочатку міряти, потім оптимізувати.

**Коротко:** recomposition оптимізують через правильну модель state, immutable параметри, локалізацію state, stable keys у lazy lists, кешування дорогих обчислень і профілювання. Recomposition сама по собі нормальна; проблема — дорога або занадто широка recomposition.

</details>
<details>
<summary>213. Як написати асинхронний тест без використання корутин?</summary>

#### Kotlin

Асинхронний тест без корутин пишуть через контрольоване очікування результату: callback, `CountDownLatch`, `CompletableFuture`, Awaitility, LiveData helper або Espresso `IdlingResource`. Ключове правило: тест має чекати конкретну подію з timeout, а не робити `Thread.sleep()`.

1. **CountDownLatch**

Найпростіший callback-based підхід:

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

    val completed = latch.await(2, TimeUnit.SECONDS)

    assertTrue(completed)
    assertNull(error.get())
    assertEquals("42", result.get()?.id)
}
```

Timeout обовʼязковий. Без нього тест може зависнути назавжди.

2. **Error scenario**

```kotlin
@Test
fun `returns error when api fails`() {
    val latch = CountDownLatch(1)
    val error = AtomicReference<Throwable?>()

    repository.loadUser("invalid", object : UserCallback {
        override fun onSuccess(user: User) {
            latch.countDown()
        }

        override fun onError(throwable: Throwable) {
            error.set(throwable)
            latch.countDown()
        }
    })

    assertTrue(latch.await(2, TimeUnit.SECONDS))
    assertTrue(error.get() is IOException)
}
```

Хороший async-тест перевіряє не тільки success path, а й failure path.

3. **CompletableFuture**

Зручно, коли хочеться отримати результат як value:

```kotlin
@Test
fun `loads user with future`() {
    val future = CompletableFuture<User>()

    repository.loadUser("42", object : UserCallback {
        override fun onSuccess(user: User) {
            future.complete(user)
        }

        override fun onError(throwable: Throwable) {
            future.completeExceptionally(throwable)
        }
    })

    val user = future.get(2, TimeUnit.SECONDS)

    assertEquals("42", user.id)
}
```

Це чистіше за окремі mutable змінні, якщо callback має один результат.

4. **Awaitility або polling helper**

Для state, який змінюється асинхронно:

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

Це краще за:

```kotlin
Thread.sleep(1000)
```

`sleep` робить тести повільними і flaky.

5. **LiveData helper**

Для LiveData часто використовують `getOrAwaitValue()`:

```kotlin
fun <T> LiveData<T>.getOrAwaitValue(
    timeout: Long = 2,
    timeUnit: TimeUnit = TimeUnit.SECONDS
): T {
    var data: T? = null
    val latch = CountDownLatch(1)

    val observer = object : Observer<T> {
        override fun onChanged(value: T) {
            data = value
            latch.countDown()
            removeObserver(this)
        }
    }

    observeForever(observer)

    if (!latch.await(timeout, timeUnit)) {
        removeObserver(observer)
        throw AssertionError("LiveData value was never set")
    }

    @Suppress("UNCHECKED_CAST")
    return data as T
}
```

У тестах LiveData також потрібен:

```kotlin
@get:Rule
val instantTaskExecutorRule = InstantTaskExecutorRule()
```

6. **Espresso IdlingResource**

Для UI-тестів краще не чекати руками, а повідомити Espresso, коли app busy/idle:

```kotlin
val idlingResource = CountingIdlingResource("app")

fun loadData() {
    idlingResource.increment()
    api.loadData {
        idlingResource.decrement()
    }
}
```

Espresso дочекається idle state перед assertion.

7. **Fake dependency замість реального async**

Найкращий тест часто не чекає реальний час, а контролює fake:

```kotlin
class FakeUserApi : UserApi {
    var callback: UserCallback? = null

    override fun loadUser(userId: String, callback: UserCallback) {
        this.callback = callback
    }

    fun complete(user: User) {
        callback?.onSuccess(user)
    }
}
```

Тест сам вирішує, коли async operation завершується:

```kotlin
repository.loadUser("42", callback)
fakeApi.complete(User(id = "42", name = "Alex"))
```

Це deterministic і швидше за реальні delays.

8. **Що не робити**

Погано:

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

Проблеми:

- тест повільний;
- flaky на слабших машинах;
- не гарантує завершення async work;
- приховує race conditions;
- збільшує час test suite.

9. **Практичне правило**

- Callback async — `CountDownLatch` або `CompletableFuture`.
- State polling — Awaitility/helper з timeout.
- LiveData — `InstantTaskExecutorRule` + `getOrAwaitValue()`.
- UI tests — Espresso `IdlingResource`.
- Не використовувати `Thread.sleep()` як механізм синхронізації.
- Краще fake dependency, ніж реальний network/time.
- У кожного async test має бути timeout.

**Коротко:** асинхронний тест без корутин пишеться через очікування конкретного callback/result/state з timeout. Найчастіше це `CountDownLatch`, `CompletableFuture`, Awaitility, LiveData helper або Espresso `IdlingResource`; `Thread.sleep()` краще не використовувати.

</details>
<details>
<summary>214. У чому різниця між inline, noinline та crossinline?</summary>

#### Kotlin

`inline`, `noinline` і `crossinline` — це модифікатори Kotlin для функцій із lambda-параметрами. `inline` просить компілятор вставити тіло функції і lambdas у місце виклику. `noinline` забороняє inline для конкретної lambda всередині inline-функції. `crossinline` забороняє non-local return з lambda, коли lambda буде викликана не напряму або в іншому execution context.

1. **inline**

Приклад:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.currentTimeMillis()
    block()
    val end = System.currentTimeMillis()
    println("Time: ${end - start}")
}
```

Виклик:

```kotlin
measure {
    println("Work")
}
```

Концептуально компілятор може перетворити це на:

```kotlin
val start = System.currentTimeMillis()
println("Work")
val end = System.currentTimeMillis()
println("Time: ${end - start}")
```

Тобто немає окремого lambda object і зайвого function call overhead.

2. **Навіщо inline**

`inline` корисний для:

- higher-order functions;
- зменшення allocation для lambdas;
- reified generics;
- non-local returns;
- DSL;
- маленьких utility functions.

Приклад зі standard library:

```kotlin
list.forEach { item ->
    println(item)
}
```

Багато Kotlin collection functions є inline.

3. **reified type parameters**

`reified` можливий тільки в inline-функціях:

```kotlin
inline fun <reified T> Gson.fromJson(json: String): T {
    return fromJson(json, T::class.java)
}
```

Виклик:

```kotlin
val user: User = gson.fromJson(json)
```

Без `inline` generic type `T` був би erased у runtime.

4. **Non-local return**

В inline lambda можна зробити return із зовнішньої функції:

```kotlin
fun hasAdmin(users: List<User>): Boolean {
    users.forEach { user ->
        if (user.isAdmin) {
            return true
        }
    }
    return false
}
```

`return true` повертає з `hasAdmin`, не тільки з lambda, бо `forEach` inline.

5. **noinline**

Якщо inline-функція має кілька lambda-параметрів, можна заборонити inline для конкретної lambda:

```kotlin
inline fun execute(
    action: () -> Unit,
    noinline onError: (Throwable) -> Unit
) {
    try {
        action()
    } catch (error: Throwable) {
        onError(error)
    }
}
```

`action` буде inline, а `onError` — ні.

6. **Навіщо noinline**

Lambda, яка inline, не існує як normal object. Її не можна:

- зберегти в змінну;
- передати в іншу non-inline функцію;
- повернути з функції;
- використати як object reference.

Потрібен `noinline`, якщо lambda треба передати далі:

```kotlin
inline fun setup(
    action: () -> Unit,
    noinline listener: () -> Unit
) {
    button.setOnClickListener {
        listener()
    }

    action()
}
```

`listener` має бути обʼєктом, бо зберігається в click listener.

7. **crossinline**

`crossinline` забороняє non-local return:

```kotlin
inline fun runLater(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

Тут `block` викликається всередині іншого object (`Runnable`), тому non-local return був би небезпечний/неможливий.

8. **Приклад проблеми без crossinline**

Якщо lambda викликається не прямо:

```kotlin
inline fun executeAsync(block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
}
```

Kotlin не дозволить таку конструкцію без `crossinline`, бо inline lambda з potential non-local return не може безпечно викликатися в іншому context.

Правильно:

```kotlin
inline fun executeAsync(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
}
```

9. **crossinline і return**

З `crossinline` не можна:

```kotlin
fun test() {
    executeAsync {
        return
    }
}
```

Тому що `return` намагається повернути з `test`, але lambda може виконатися пізніше або в іншому context.

Можна local return:

```kotlin
executeAsync {
    return@executeAsync
}
```

10. **inline vs noinline vs crossinline**

```text
inline:
  - lambda inlined
  - можна non-local return
  - менше allocation

noinline:
  - lambda не inlined
  - можна передати/зберегти як object
  - non-local return не можна

crossinline:
  - lambda inlined
  - non-local return заборонений
  - можна викликати в іншому object/context
```

11. **Приклад разом**

```kotlin
inline fun perform(
    action: () -> Unit,
    noinline onError: (Throwable) -> Unit,
    crossinline onComplete: () -> Unit
) {
    try {
        action()

        val runnable = Runnable {
            onComplete()
        }
        runnable.run()
    } catch (error: Throwable) {
        onError(error)
    }
}
```

`action` — inline і може non-local return.  
`onError` — object lambda, можна передати далі.  
`onComplete` — inline, але без non-local return.

12. **inline не завжди добре**

Не треба робити inline для великих функцій:

```kotlin
inline fun hugeFunction(block: () -> Unit) {
    // 200 lines of logic
}
```

Мінуси:

- збільшення bytecode;
- складніший debugging;
- potential binary compatibility concerns;
- не завжди є performance benefit.

Inline має сенс для малих higher-order functions.

13. **Практичне правило**

- `inline` — коли маленька higher-order function і треба прибрати lambda overhead або використати `reified`.
- `noinline` — коли lambda треба зберегти, передати або використати як object.
- `crossinline` — коли lambda inlined, але буде викликана не напряму, тому треба заборонити non-local return.
- Не ставити `inline` всюди.
- Для великих функцій inline може нашкодити.

**Коротко:** `inline` вбудовує lambda в місце виклику, `noinline` залишає конкретну lambda звичайним object, а `crossinline` залишає lambda inline, але забороняє non-local return для безпечного виклику в іншому context.

</details>
<details>
<summary>215. Які переваги Kotlin над Java?</summary>

#### Kotlin

Kotlin має переваги над Java насамперед у безпеці, лаконічності та сучасних language features. Для Android головні плюси: null-safety, data classes, extension functions, coroutines, sealed classes, smart casts, default/named arguments, більш виразні collections API і повна interoperable робота з Java.

1. **Null-safety**

У Java `null` може бути майже всюди:

```java
String name = user.getName();
int length = name.length();
```

Якщо `name == null`, буде `NullPointerException`.

У Kotlin nullability є частиною типу:

```kotlin
val name: String = "Alex"
val optionalName: String? = null
```

Щоб працювати з nullable:

```kotlin
val length = optionalName?.length ?: 0
```

Це різко зменшує кількість випадкових NPE.

2. **Data classes**

У Java для model-класу потрібно багато boilerplate:

```java
class User {
    private final String id;
    private final String name;

    // constructor, getters, equals, hashCode, toString
}
```

У Kotlin:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Компілятор генерує:

- `equals`;
- `hashCode`;
- `toString`;
- `copy`;
- `componentN`.

3. **Extension functions**

Kotlin дозволяє додавати функції до існуючих типів без inheritance:

```kotlin
fun String.capitalizeFirst(): String {
    return replaceFirstChar { it.uppercase() }
}
```

Виклик:

```kotlin
val title = "profile".capitalizeFirst()
```

Це дуже корисно для utility API, Android View extensions, mappers.

4. **Coroutines**

У Java async-код часто callback/future-heavy:

```java
api.getUser(id, new Callback<User>() {
    @Override
    public void onSuccess(User user) { }

    @Override
    public void onError(Throwable error) { }
});
```

У Kotlin:

```kotlin
val user = repository.getUser(userId)
```

Якщо `getUser` — suspend-функція:

```kotlin
suspend fun getUser(userId: String): User
```

Код читається послідовно, але не блокує thread.

5. **Sealed classes**

Для обмежених станів:

```kotlin
sealed interface ProfileState {
    data object Loading : ProfileState
    data class Content(val user: User) : ProfileState
    data class Error(val message: String) : ProfileState
}
```

`when` може бути exhaustive:

```kotlin
when (state) {
    ProfileState.Loading -> showLoading()
    is ProfileState.Content -> showUser(state.user)
    is ProfileState.Error -> showError(state.message)
}
```

Це дуже зручно для UI state, navigation state, domain result.

6. **Smart casts**

Kotlin автоматично cast-ить після перевірки:

```kotlin
fun render(state: ProfileState) {
    if (state is ProfileState.Content) {
        println(state.user.name)
    }
}
```

У Java треба явний cast:

```java
if (state instanceof Content) {
    User user = ((Content) state).getUser();
}
```

7. **Default і named arguments**

```kotlin
fun createUser(
    name: String,
    age: Int = 0,
    isAdmin: Boolean = false
)
```

Виклик:

```kotlin
createUser(
    name = "Alex",
    isAdmin = true
)
```

Це зменшує кількість overloads і робить виклики читабельнішими.

8. **Collections API**

```kotlin
val activeUsers = users
    .filter { it.isActive }
    .map { it.name }
    .sorted()
```

Kotlin collection API лаконічний і добре працює з lambdas.

9. **Immutability by default**

Kotlin розділяє `val` і `var`:

```kotlin
val id = "42"
var name = "Alex"
```

`val` не можна переприсвоїти. Це підштовхує до immutable стилю.

Також є read-only collection interfaces:

```kotlin
val users: List<User> = listOf()
```

10. **Scope functions**

Kotlin має `let`, `run`, `with`, `apply`, `also`:

```kotlin
val user = UserBuilder()
    .apply {
        setName("Alex")
        setAge(30)
    }
    .build()
```

Або nullable handling:

```kotlin
user?.let {
    showUser(it)
}
```

Вони роблять код компактнішим, але ними не треба зловживати.

11. **Interoperability з Java**

Kotlin повністю працює з Java:

```kotlin
val date = java.time.LocalDate.now()
```

Можна:

- викликати Java-код з Kotlin;
- викликати Kotlin-код з Java;
- поступово мігрувати проєкт;
- використовувати Java libraries.

Це критично для Android, де багато SDK написані на Java.

12. **Android-specific переваги**

Для Android Kotlin став фактичним default:

- coroutines для async;
- Flow для reactive streams;
- Jetpack Compose написаний для Kotlin;
- extension APIs у Android KTX;
- Hilt/Room/Paging мають Kotlin-friendly API;
- менше boilerplate у ViewModel/use cases/mappers.

Приклад:

```kotlin
viewModelScope.launch {
    repository.getUsers()
        .collect { users ->
            _state.value = UserState.Content(users)
        }
}
```

13. **Kotlin не ідеальний**

Переваги не означають, що Kotlin без мінусів:

- повільніший compile time у деяких проєктах;
- треба розуміти nullability/platform types;
- coroutines вимагають дисципліни;
- scope functions можуть погіршити читабельність;
- неправильне використання inline/reified/generics може ускладнити код.

Але для Android плюси зазвичай переважають.

14. **Практичне правило**

- Kotlin безпечніший через null-safety.
- Kotlin лаконічніший через data classes, properties, default args.
- Kotlin виразніший через sealed classes, smart casts, extension functions.
- Kotlin краще підходить для modern Android через coroutines, Flow, Compose.
- Java interoperability дозволяє мігрувати поступово.
- Переваги працюють тільки якщо писати idiomatic Kotlin, а не Java-style Kotlin.

**Коротко:** Kotlin дає менше boilerplate, кращу null-safety, сучасні async primitives, сильніші моделі стану через sealed/data classes і зручний Android-first ecosystem. Його головна перевага над Java — безпечніший і виразніший код при повній сумісності з Java.

</details>
<details>
<summary>216. У чому різниця між val та const val?</summary>

#### Kotlin

`val` і `const val` обидва означають значення, яке не можна переприсвоїти, але вони працюють на різних рівнях. `val` — це read-only property, значення якої може бути обчислене під час runtime. `const val` — це compile-time constant, значення якої відоме компілятору ще до запуску програми.

1. **val**

`val` не можна переприсвоїти після ініціалізації:

```kotlin
val name = "Alex"
// name = "Bob" // compile error
```

Але значення може бути обчислене в runtime:

```kotlin
val createdAt = System.currentTimeMillis()
```

Тут `createdAt` read-only, але не compile-time constant.

2. **const val**

`const val` — це константа часу компіляції:

```kotlin
const val API_VERSION = "v1"
const val MAX_RETRY_COUNT = 3
```

Її значення має бути відоме на compile time.

Не можна:

```kotlin
const val CREATED_AT = System.currentTimeMillis()
```

Бо `System.currentTimeMillis()` виконується тільки в runtime.

3. **Де можна оголошувати const val**

`const val` можна оголошувати тільки:

- на top-level;
- в `object`;
- у `companion object`.

Top-level:

```kotlin
const val BASE_URL = "https://api.example.com"
```

Object:

```kotlin
object ApiConfig {
    const val TIMEOUT_SECONDS = 30
}
```

Companion object:

```kotlin
class UserRepository {
    companion object {
        const val DEFAULT_PAGE_SIZE = 20
    }
}
```

4. **Де не можна const val**

Не можна всередині function:

```kotlin
fun test() {
    const val value = 10 // compile error
}
```

Не можна як instance property:

```kotlin
class Config {
    const val version = "1" // compile error
}
```

Бо instance property залежить від обʼєкта, а `const val` має бути compile-time/static-like.

5. **Типи для const val**

`const val` підтримує тільки primitive/string типи:

```kotlin
const val NAME = "app"
const val COUNT = 10
const val ENABLED = true
const val PI = 3.14
```

Не можна:

```kotlin
const val USERS = listOf("Alex") // compile error
const val DATE = LocalDate.now() // compile error
```

6. **val може бути будь-якого типу**

```kotlin
val users = listOf("Alex", "Bob")
val formatter = DateTimeFormatter.ISO_DATE
val repository = UserRepository()
```

`val` означає тільки, що property reference не можна переприсвоїти. Це не означає, що сам object immutable.

7. **val не гарантує глибоку immutable**

```kotlin
val users = mutableListOf<String>()
users.add("Alex")
```

`users` не можна переприсвоїти:

```kotlin
// users = mutableListOf() // compile error
```

Але сам список mutable, тому його можна змінити.

Для immutable API краще:

```kotlin
val users: List<String> = listOf("Alex")
```

8. **Compile-time inlining**

`const val` значення inline-иться в bytecode у місце використання:

```kotlin
const val APP_VERSION = "1.0"
```

Використання:

```kotlin
println(APP_VERSION)
```

Компілятор може підставити `"1.0"` напряму. Це важливо для annotations і Java interop.

9. **const val в annotations**

Annotation arguments мають бути compile-time constants:

```kotlin
const val TABLE_USERS = "users"
```

```kotlin
@Entity(tableName = TABLE_USERS)
data class UserEntity(
    @PrimaryKey val id: String
)
```

Звичайний `val` тут не завжди підійде:

```kotlin
val tableName = "users"

@Entity(tableName = tableName) // compile error
```

10. **Java interop**

`const val` у `object`/top-level виглядає для Java як static final constant.

```kotlin
object Constants {
    const val API_VERSION = "v1"
}
```

Java:

```java
String version = Constants.API_VERSION;
```

Для звичайного `val` у object Java часто бачить getter:

```kotlin
object Constants {
    val apiVersion = "v1"
}
```

Java:

```java
String version = Constants.INSTANCE.getApiVersion();
```

11. **lateinit і const val**

`lateinit` працює тільки з `var`, не з `val` або `const val`:

```kotlin
lateinit var repository: UserRepository
```

`const val` завжди має бути ініціалізований одразу:

```kotlin
const val DEFAULT_LANGUAGE = "en"
```

12. **Коли використовувати val**

Використовувати `val`, коли:

- значення обчислюється в runtime;
- тип не primitive/string constant;
- це instance property;
- потрібен object/list/date/repository;
- значення read-only, але не compile-time constant.

```kotlin
val userId = savedStateHandle.get<String>("user_id")
val createdAt = clock.now()
```

13. **Коли використовувати const val**

Використовувати `const val`, коли:

- значення відоме на compile time;
- потрібне в annotation;
- це string/int/boolean/etc constant;
- це protocol/key/table/route constant;
- треба Java static final style constant.

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DATABASE_NAME = "app.db"
```

14. **Типові помилки**

- Думати, що `val` означає повну immutable.
- Намагатися зробити `const val` зі списком або object.
- Оголошувати `const val` всередині function або class instance.
- Використовувати runtime expression для `const val`.
- Використовувати `val` там, де annotation потребує compile-time constant.

15. **Практичне правило**

- `val` — read-only reference/property.
- `const val` — compile-time constant.
- `val` може бути runtime value.
- `const val` має бути primitive або `String`.
- `const val` доступний тільки top-level/object/companion object.
- Для annotation arguments потрібен `const val`.

**Коротко:** `val` означає, що змінну не можна переприсвоїти після ініціалізації, але значення може бути runtime. `const val` — це справжня compile-time константа, яку компілятор знає наперед і може inline-ити.

</details>
<details>
<summary>217. Які типи можна використовувати з const val?</summary>

#### Kotlin

З `const val` у Kotlin можна використовувати тільки типи, значення яких компілятор може знати на етапі компіляції: `String` і primitive types. Це означає, що `const val` підходить для compile-time constants: ключів, route names, table names, числових лімітів, flags. Для списків, обʼєктів, дат, enum або результатів функцій `const val` використовувати не можна.

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

Тобто дозволені `String` і primitive-like Kotlin types.

2. **String**

Найчастіший випадок:

```kotlin
const val ARG_USER_ID = "user_id"
const val ROUTE_PROFILE = "profile"
const val DATABASE_NAME = "app.db"
```

Такі константи зручно використовувати для:

- Bundle keys;
- Intent extras;
- navigation routes;
- database names;
- table names;
- API constants.

3. **Numbers**

```kotlin
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
```

Це доречно для простих numeric constants, які не залежать від runtime.

4. **Boolean**

```kotlin
const val LOGGING_ENABLED = true
```

Але для build-specific flags краще часто використовувати `BuildConfig` або product flavors:

```kotlin
if (BuildConfig.DEBUG) {
    enableDebugLogging()
}
```

5. **Char**

```kotlin
const val CSV_SEPARATOR = ','
```

Рідше використовується, але теж дозволено.

6. **Де можна оголошувати const val**

Top-level:

```kotlin
const val API_VERSION = "v1"
```

В `object`:

```kotlin
object ApiConstants {
    const val BASE_PATH = "/api"
}
```

У `companion object`:

```kotlin
class UserFragment {
    companion object {
        const val ARG_USER_ID = "user_id"
    }
}
```

7. **Недозволені типи**

Не можна:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val FORMATTER = DateTimeFormatter.ISO_DATE
const val UUID_VALUE = UUID.randomUUID()
```

Причина: це runtime objects або results of function calls, а не compile-time constants.

8. **Enum не можна**

```kotlin
enum class ThemeMode {
    Light,
    Dark
}
```

Не можна:

```kotlin
const val DEFAULT_THEME = ThemeMode.Light
```

Краще:

```kotlin
val DEFAULT_THEME = ThemeMode.Light
```

Або якщо потрібне string значення:

```kotlin
const val DEFAULT_THEME_VALUE = "light"
```

9. **List/Map/Set не можна**

```kotlin
const val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

Так не можна. Треба:

```kotlin
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

Але памʼятати: `val` не робить mutable object immutable, якщо сам object mutable.

10. **Функції і runtime expressions не можна**

Не можна:

```kotlin
const val CURRENT_TIME = System.currentTimeMillis()
const val RANDOM_ID = UUID.randomUUID().toString()
const val PACKAGE_NAME = context.packageName
```

`const val` має бути обчислений компілятором, а не під час виконання програми.

11. **const val для annotations**

Annotation arguments мають бути compile-time constants:

```kotlin
const val USERS_TABLE = "users"
```

```kotlin
@Entity(tableName = USERS_TABLE)
data class UserEntity(
    @PrimaryKey val id: String
)
```

Звичайний `val` тут не підходить:

```kotlin
val usersTable = "users"
```

```kotlin
@Entity(tableName = usersTable) // compile error
```

12. **const val і nullable**

`const val` не може бути nullable:

```kotlin
const val NAME: String? = null // compile error
```

Compile-time constant має мати конкретне non-null значення.

13. **const val і custom typealias**

Якщо є typealias до дозволеного типу:

```kotlin
typealias UserIdKey = String

const val ARG_USER_ID: UserIdKey = "user_id"
```

Це може працювати, бо underlying type — `String`. Але в реальному коді краще не ускладнювати constants typealias-ами без потреби.

14. **Практичне правило**

- `const val` — тільки `String` і primitive types.
- Значення має бути відоме на compile time.
- Не можна objects, lists, maps, dates, enums, function calls.
- Для annotation arguments використовувати `const val`.
- Для runtime values використовувати звичайний `val`.
- Для складних constants краще `val` в `object`.

**Коротко:** з `const val` можна використовувати тільки compile-time типи: `String`, числа, `Boolean`, `Char` та інші primitive types. Якщо значення створюється в runtime або є обʼєктом, треба використовувати звичайний `val`.

</details>
<details>
<summary>218. Чи можна створити data class без параметрів?</summary>

#### Kotlin

Ні, у Kotlin `data class` не можна створити без параметрів у primary constructor. За правилами мови, `data class` має мати щонайменше один параметр у primary constructor, і цей параметр має бути позначений як `val` або `var`.

1. **Некоректний data class**

Так не можна:

```kotlin
data class Empty
```

І так теж не можна:

```kotlin
data class Empty()
```

Компілятор очікує хоча б один property-параметр у primary constructor.

2. **Коректний data class**

```kotlin
data class User(
    val id: String
)
```

Або:

```kotlin
data class Point(
    val x: Int,
    val y: Int
)
```

Саме constructor properties використовуються для генерації `equals`, `hashCode`, `toString`, `copy` і `componentN`.

3. **Чому потрібен хоча б один параметр**

`data class` призначений для зберігання даних. Компілятор генерує methods на основі properties primary constructor:

```kotlin
data class User(
    val id: String,
    val name: String
)
```

Генерується логіка приблизно для:

```text
equals()
hashCode()
toString()
copy()
component1()
component2()
```

Якщо параметрів немає, немає даних, на основі яких генерувати data-behavior.

4. **Параметр має бути val або var**

Так не можна:

```kotlin
data class User(id: String)
```

Потрібно:

```kotlin
data class User(
    val id: String
)
```

Бо data class працює з properties, а не просто constructor arguments.

5. **Параметри в тілі класу не рахуються**

Так не можна:

```kotlin
data class User() {
    val id: String = "1"
}
```

Бо primary constructor порожній.

Так можна:

```kotlin
data class User(
    val id: String
) {
    val type: String = "regular"
}
```

Але `type` з тіла класу не братиме участі в `equals`, `hashCode`, `copy` і destructuring.

6. **Default values не означають “без параметрів”**

Можна зробити data class, який можна створити без arguments, якщо всі parameters мають default values:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)
```

Виклик:

```kotlin
val user = User()
```

Але це не data class без параметрів. Параметри є, просто вони мають default values.

7. **No-arg constructor для Java/serialization**

Якщо потрібен no-arg constructor для framework:

```kotlin
data class User(
    val id: String = "",
    val name: String = ""
)
```

Java може викликати:

```java
User user = new User();
```

за певних умов генерації overloads/default constructor. Для деяких persistence/serialization frameworks також використовують Kotlin noarg plugin, але для Android DTO частіше достатньо default values або конкретної serialization library.

8. **Якщо треба marker/state без даних**

Не треба використовувати data class:

```kotlin
object Loading
```

Або sealed state:

```kotlin
sealed interface UiState {
    data object Loading : UiState
    data class Content(val user: User) : UiState
    data object Error : UiState
}
```

Для стану без payload краще `object` або `data object`, а не порожній data class.

9. **data object**

У Kotlin є `data object` для singleton-станів:

```kotlin
data object Loading
```

Це корисно в sealed hierarchies:

```kotlin
sealed interface ScreenState {
    data object Loading : ScreenState
    data class Content(val items: List<Item>) : ScreenState
}
```

Якщо даних немає і instance має бути один — це кращий варіант.

10. **Практичне правило**

- `data class` має мати хоча б один `val` або `var` параметр у primary constructor.
- `data class Empty()` не компілюється.
- Якщо потрібен no-arg виклик — додати default values.
- Якщо стан без даних — використовувати `object` або `data object`.
- Properties у body не беруть участі в data class generated methods.

**Коротко:** створити `data class` без параметрів не можна. Але можна створити data class з параметрами за замовчуванням і викликати його як `User()`. Якщо даних немає взагалі, краще використовувати `object` або `data object`.

</details>
<details>
<summary>219. У чому різниця між sealed class та enum?</summary>

#### Kotlin

`enum` і `sealed class` обидва використовують для обмеженого набору варіантів, але вони моделюють різні речі. `enum` — це фіксований набір однакових singleton-констант. `sealed class` або `sealed interface` — це закрита ієрархія типів, де кожен варіант може мати власну структуру, поля, поведінку і навіть бути `data class`.

1. **Enum**

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

Enum добре підходить, коли є простий фіксований набір значень без різних payload:

```kotlin
val mode = ThemeMode.Dark
```

2. **Sealed class**

```kotlin
sealed class UiState {
    data object Loading : UiState()
    data class Content(val user: User) : UiState()
    data class Error(val message: String) : UiState()
}
```

Тут варіанти різні:

- `Loading` не має даних;
- `Content` має `User`;
- `Error` має `message`.

Enum таку модель нормально не виразить.

3. **Головна різниця**

```text
enum:
  - фіксований набір singleton values
  - всі елементи одного типу
  - не має різного payload на кожен case

sealed:
  - закрита type hierarchy
  - кожен case може мати свій тип
  - кожен case може мати власні поля
```

4. **when exhaustive**

І enum, і sealed дають exhaustive `when`:

```kotlin
fun title(mode: ThemeMode): String {
    return when (mode) {
        ThemeMode.Light -> "Light"
        ThemeMode.Dark -> "Dark"
        ThemeMode.System -> "System"
    }
}
```

Sealed:

```kotlin
fun render(state: UiState) {
    when (state) {
        UiState.Loading -> showLoading()
        is UiState.Content -> showUser(state.user)
        is UiState.Error -> showError(state.message)
    }
}
```

Компілятор знає всі можливі subclasses.

5. **Enum з properties**

Enum може мати properties:

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

Але структура всіх enum entries однакова: кожен має `allowsBody`.

6. **Sealed з різними payload**

```kotlin
sealed interface PaymentResult {
    data class Success(
        val transactionId: String,
        val amount: Long
    ) : PaymentResult

    data class Failed(
        val reason: String
    ) : PaymentResult

    data object Cancelled : PaymentResult
}
```

Кожен варіант має власні поля. Це головна сила sealed types.

7. **Enum entries — singleton**

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Кожне enum значення існує як один singleton instance.

8. **Sealed subclasses можуть бути singleton або data**

```kotlin
sealed interface ScreenState {
    data object Loading : ScreenState
    data class Content(val items: List<Item>) : ScreenState
}
```

`Loading` — singleton.  
`Content` — багато instances з різними даними.

9. **Enum має built-in API**

```kotlin
ThemeMode.entries
ThemeMode.valueOf("Dark")
ThemeMode.Dark.name
ThemeMode.Dark.ordinal
```

Це зручно для простих списків значень, dropdown, settings.

Але `ordinal` краще не зберігати в database/API, бо порядок enum може змінитися.

10. **Sealed не має valueOf**

У sealed hierarchy немає автоматичного:

```kotlin
PaymentResult.valueOf(...)
```

Якщо потрібна serialization/deserialization, треба явно описувати mapping або використовувати serialization library.

11. **Коли використовувати enum**

Enum добре підходить для:

- fixed simple constants;
- modes;
- sort type;
- theme;
- tabs;
- permissions state без payload;
- HTTP methods;
- simple status values.

Приклад:

```kotlin
enum class SortOrder {
    Ascending,
    Descending
}
```

12. **Коли використовувати sealed**

Sealed підходить для:

- UI state;
- result state;
- events;
- navigation commands;
- domain outcomes;
- error hierarchy;
- cases з різними payload.

Приклад:

```kotlin
sealed interface NetworkResult<out T> {
    data class Success<T>(val data: T) : NetworkResult<T>
    data class Error(val throwable: Throwable) : NetworkResult<Nothing>
    data object Loading : NetworkResult<Nothing>
}
```

13. **Sealed class vs sealed interface**

`sealed class` може мати state/constructor:

```kotlin
sealed class ErrorState(
    val code: Int
)
```

`sealed interface` гнучкіший для множинної реалізації:

```kotlin
sealed interface UiEvent
```

У сучасному Kotlin для state hierarchies часто використовують `sealed interface` + `data class`/`data object`.

14. **Serialization**

Enum легко серіалізувати як string:

```kotlin
ThemeMode.Dark.name
```

Sealed serialization складніша, бо треба зберігати type discriminator і payload:

```json
{
  "type": "Error",
  "message": "Network error"
}
```

З Kotlinx Serialization це можна зробити, але контракт складніший.

15. **Практичне правило**

- Якщо всі варіанти однакові і без payload — `enum`.
- Якщо варіанти мають різні дані або поведінку — `sealed`.
- Для UI state майже завжди краще `sealed`.
- Для settings/dropdown/simple constants — `enum`.
- Не зберігати `enum.ordinal` у persistent storage.
- Для sealed hierarchy використовувати exhaustive `when`.

**Коротко:** `enum` — це фіксований список простих singleton-значень. `sealed class/interface` — це закрита ієрархія типів, де кожен варіант може мати власні дані. Для простих режимів — enum, для UI/result/domain state з payload — sealed.

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

`WorkManager` варто використовувати для deferrable background work — задач, які мають гарантовано виконатись, але не обовʼязково прямо зараз. Він враховує constraints, retry, process death і системні обмеження Android.

1. **Типові сценарії**

WorkManager підходить для:

- синхронізації даних;
- upload/download, який можна відкласти;
- відправки analytics/logs;
- cleanup старих даних;
- periodic background sync;
- retry після network error;
- задач, які мають пережити закриття app.

Якщо робота критична і user-visible прямо зараз, частіше потрібен foreground service.

2. **OneTimeWorkRequest**

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

Це одноразова background-задача з умовою наявності мережі.

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

`Result.retry()` каже WorkManager повторити задачу з backoff policy. `Result.failure()` — задача завершилась остаточно.

4. **Constraints**

WorkManager може чекати потрібних умов:

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.UNMETERED)
    .setRequiresCharging(true)
    .setRequiresBatteryNotLow(true)
    .build()
```

Це корисно для задач, які не треба виконувати на мобільному інтернеті або при низькій батареї.

5. **Periodic work**

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodic work не гарантує точний час запуску. Система сама вибирає оптимальний момент. Якщо потрібен точний час — це не WorkManager, а інший механізм, наприклад `AlarmManager`.

6. **Unique work**

Щоб не запускати дублікати:

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` залишає існуючу задачу. `REPLACE` скасовує стару і ставить нову.

7. **Input / output data**

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Data підходить тільки для невеликих primitive/string values. Великі обʼєкти краще передавати через id і читати з DB.

8. **Коли WorkManager не підходить**

Не варто використовувати WorkManager для:

- точних alarm-ів;
- роботи, яку треба виконати негайно;
- довгої user-visible операції без notification;
- постійного socket connection;
- streaming/audio playback;
- задач, які мають виконуватись щосекунди.

9. **WorkManager vs Service**

```text
WorkManager -> гарантована відкладена робота з constraints/retry
Service     -> робота, повʼязана з активним component або foreground user-visible task
```

Для sync після закриття app — WorkManager. Для програвання музики — foreground service.

10. **Практичне правило**

Питай себе: “чи можна цю задачу відкласти і виконати пізніше, але гарантовано?”. Якщо так — WorkManager хороший вибір. Якщо потрібне точне виконання прямо зараз або постійна активність — ні.

**Коротко:** WorkManager використовують для гарантованої deferrable background work: sync, upload, cleanup, retry і periodic tasks. Він підтримує constraints, retry, unique work і process-death resilience, але не підходить для точних alarm-ів, realtime-задач, постійних connection або foreground media/location сценаріїв.

</details>
<details>
<summary>222. Чи працювали ви з Android Media3 / ExoPlayer?</summary>

#### Kotlin

Так, Media3 / ExoPlayer використовують для програвання audio/video в Android: streaming, local media, playlists, adaptive formats, subtitles, background playback і кастомний player UI. Media3 — сучасний Jetpack-набір бібліотек, який включає ExoPlayer, PlayerView, MediaSession та інші компоненти.

1. **Що таке ExoPlayer / Media3**

`ExoPlayer` — гнучкий media player, який підтримує більше сценаріїв, ніж стандартний `MediaPlayer`:

- HLS/DASH/SmoothStreaming;
- progressive MP4/MP3;
- playlists;
- subtitles;
- DRM;
- buffering control;
- custom renderers;
- background playback через MediaSession.

2. **Базове створення player**

```kotlin
val player = ExoPlayer.Builder(context).build()

player.setMediaItem(
    MediaItem.fromUri(videoUrl)
)

player.prepare()
player.play()
```

У View System його часто підключають до `PlayerView`:

```kotlin
playerView.player = player
```

3. **Lifecycle**

Player треба коректно звільняти:

```kotlin
override fun onStop() {
    player.pause()
    super.onStop()
}

override fun onDestroy() {
    player.release()
    super.onDestroy()
}
```

Якщо player живе у Fragment, важливо не тримати reference на View після `onDestroyView()`.

4. **Compose інтеграція**

У Compose можна використати `AndroidView`:

```kotlin
AndroidView(
    factory = { context ->
        PlayerView(context).apply {
            player = exoPlayer
        }
    }
)
```

Player краще створювати не напряму в composable body, а через `remember` або ViewModel/holder з чітким lifecycle.

5. **MediaItem і playlist**

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

ExoPlayer може працювати з playlist і перемикати media items.

6. **Player.Listener**

```kotlin
player.addListener(object : Player.Listener {
    override fun onPlaybackStateChanged(state: Int) {
        when (state) {
            Player.STATE_BUFFERING -> showLoading()
            Player.STATE_READY -> hideLoading()
            Player.STATE_ENDED -> showReplay()
        }
    }

    override fun onPlayerError(error: PlaybackException) {
        showError(error)
    }
})
```

Listener потрібен для buffering, errors, ended state, tracking і UI updates.

7. **Background playback**

Для музики/podcast потрібні:

- foreground service;
- MediaSession;
- notification controls;
- audio focus;
- handling noisy intent;
- playback state для system UI.

Не варто тримати довге audio playback тільки в Activity/Fragment.

8. **Caching**

Для streaming можна налаштувати cache через `CacheDataSource`, але це треба робити обережно: розмір cache, eviction policy, offline behavior, DRM і storage limits.

9. **Типові проблеми**

- player не release-иться і тече memory;
- створення player на кожну recomposition;
- неправильний lifecycle у Fragment/Compose;
- відсутній audio focus;
- background playback без foreground service;
- погана обробка buffering/error;
- важкі operations на main thread під час playback.

10. **Практичне правило**

Для простого video screen достатньо `ExoPlayer + PlayerView + lifecycle cleanup`. Для production audio/video app потрібні MediaSession, foreground service, notification, audio focus, error handling, analytics і тестування lifecycle/background сценаріїв.

**Коротко:** Media3 / ExoPlayer — сучасний Android media stack для audio/video playback. Він підтримує streaming, playlists, subtitles, DRM, listeners, MediaSession і background playback. Головне в production — правильно керувати lifecycle, release player, обробляти buffering/errors і не створювати player напряму в recomposition.

</details>
<details>
<summary>223. Як би ви розподілили компоненти по модулях, якщо є кнопка, яка по кліку завантажує дані?</summary>

#### Kotlin

Я б розподіляв не “кнопку”, а весь use case: UI event → ViewModel → domain use case → repository → data source. Кнопка — це тільки trigger, тому вона має жити в UI-модулі, а логіка завантаження — нижче по шарах.

1. **Приклад модулів**

```text
:app
:core:network
:core:database
:core:common
:core:ui
:feature:profile
:domain:profile
:data:profile
```

Можна мати менше модулів, якщо проєкт малий. Важливо не кількість, а правильний напрямок залежностей.

2. **UI / feature module**

У `:feature:profile` лежить екран, кнопка і ViewModel:

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

UI не знає, звідки беруться дані: з API, DB чи cache.

3. **ViewModel**

ViewModel приймає event від UI і викликає use case:

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun onLoadClick() {
        viewModelScope.launch {
            _state.update { it.copy(isLoading = true) }
            val result = loadProfile()
            _state.update { it.copy(isLoading = false, profile = result) }
        }
    }
}
```

ViewModel керує screen state, але не містить network/database деталей.

4. **Domain module**

У `:domain:profile` лежить use case і contract repository:

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}

interface ProfileRepository {
    suspend fun loadProfile(): Profile
}
```

Domain не залежить від Retrofit, Room, Android UI або DI framework.

5. **Data module**

У `:data:profile` лежить implementation repository:

```kotlin
class ProfileRepositoryImpl(
    private val api: ProfileApi,
    private val dao: ProfileDao
) : ProfileRepository {
    override suspend fun loadProfile(): Profile {
        val remote = api.getProfile()
        dao.save(remote.toEntity())
        return remote.toDomain()
    }
}
```

Data module знає про API, DB, DTO/entity mappings і cache strategy.

6. **Core modules**

`core` містить спільну інфраструктуру:

```text
:core:network  -> Retrofit/OkHttp setup
:core:database -> Room setup
:core:ui       -> shared UI components/theme
:core:common   -> utilities, dispatchers, result types
```

Core не має залежати від конкретної feature.

7. **Dependency direction**

Правильний напрямок:

```text
feature -> domain
 data   -> domain
 app    -> feature + data + DI wiring
```

`feature` може знати contract з domain, але не конкретну data implementation. Binding робиться в `app` або DI module.

8. **DI wiring**

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

DI зʼєднує interface з implementation, але бізнес-логіка від цього не залежить.

9. **Як відповісти на інтервʼю**

Я б сказав так: кнопка живе в UI, ViewModel обробляє click event, use case описує бізнес-дію, repository приховує джерело даних, data source працює з API/DB. Модулі будую так, щоб feature не залежала від concrete data implementation.

**Коротко:** кнопку треба залишити в feature/UI модулі. Click іде у ViewModel, ViewModel викликає use case з domain, use case працює через repository interface, а implementation repository і API/DB лежать у data/core модулях. Головне — правильний dependency direction і відсутність network/database логіки в UI.

</details>
<details>
<summary>224. Які є способи уникнути лагів у Compose, наприклад під час скролу?</summary>

#### Kotlin

Лаги під час скролу в Compose зазвичай виникають не через сам `LazyColumn`, а через важкий item content, нестабільні моделі, зайві recompositions, великі images, allocation у composable body або blocking роботу на main thread. Оптимізація має починатися з профілювання, а не з випадкового додавання `remember` всюди.

1. **Використовувати LazyColumn, а не Column**

Погано для великих списків:

```kotlin
Column {
    users.forEach { user ->
        UserItem(user)
    }
}
```

Краще:

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

`LazyColumn` створює тільки item-и, потрібні для viewport.

2. **Ставити stable key**

```kotlin
items(
    items = users,
    key = { user -> user.id }
) { user ->
    UserItem(user)
}
```

`key` не дає remembered state і composition identity привʼязатися до позиції. Це особливо важливо при insert/delete/filter/sort.

3. **Використовувати contentType для mixed list**

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it::class }
) { item ->
    FeedItem(item)
}
```

`contentType` допомагає Compose ефективніше reuse-ити item compositions одного типу: header, post, ad, loader.

4. **Не робити важкі обчислення в item**

Погано:

```kotlin
LazyColumn {
    items(users) { user ->
        val formattedDate = expensiveFormat(user.createdAt)
        UserItem(user, formattedDate)
    }
}
```

Краще підготувати UI model у ViewModel:

```kotlin
data class UserUiModel(
    val id: String,
    val name: String,
    val formattedDate: String
)
```

Або мінімум кешувати:

```kotlin
val formattedDate = remember(user.createdAt) {
    expensiveFormat(user.createdAt)
}
```

5. **Передавати immutable UI models**

Погано:

```kotlin
class UserUiModel(
    var name: String,
    var isSelected: Boolean
)
```

Краще:

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val isSelected: Boolean
)
```

Так Compose легше визначає, що параметри стабільні, і може skip-ати зайві recompositions.

6. **Не мутувати списки in-place**

Погано:

```kotlin
users.add(newUser)
```

Краще:

```kotlin
state = state.copy(
    users = state.users + newUser
)
```

Для Compose важливі стабільні snapshots, а не прихована мутація mutable collection.

7. **Оптимізувати images**

Частий source jank — картинки. Правила:

- не декодувати bitmap вручну в composable;
- задавати стабільний розмір;
- використовувати Coil/інший loader із cache;
- не робити crossfade без потреби у великих списках;
- не завантажувати оригінал 4000px для avatar 48dp.

Приклад:

```kotlin
AsyncImage(
    model = user.avatarUrl,
    contentDescription = null,
    modifier = Modifier.size(48.dp)
)
```

8. **Фіксувати розміри item, де це можливо**

Якщо image/content змінює розмір після завантаження, scroll може смикатися.

```kotlin
AsyncImage(
    model = imageUrl,
    contentDescription = null,
    modifier = Modifier
        .fillMaxWidth()
        .height(180.dp)
)
```

Стабільний layout зменшує remeasure/re-layout cost.

9. **derivedStateOf для scroll-derived UI**

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

Scroll state змінюється часто, але UI-прапорець може змінюватися рідко.

10. **snapshotFlow для scroll side effects**

Analytics або side effects від scroll краще робити через `snapshotFlow`:

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            analytics.trackScrollIndex(index)
        }
}
```

Не треба виконувати side effects прямо в body composable.

11. **Уникати nested vertical lazy lists**

Погано:

```kotlin
LazyColumn {
    item {
        LazyColumn {
            items(innerItems) { ... }
        }
    }
}
```

Краще flatten-ити структуру в один список:

```kotlin
LazyColumn {
    items(feedItems, key = { it.id }) { item ->
        FeedItem(item)
    }
}
```

Горизонтальний `LazyRow` всередині вертикального `LazyColumn` — нормальний сценарій, але vertical-in-vertical часто створює проблеми.

12. **Paging для великих remote списків**

```kotlin
val users = viewModel.users.collectAsLazyPagingItems()

LazyColumn {
    items(users.itemCount) { index ->
        users[index]?.let { user ->
            UserItem(user)
        }
    }
}
```

Paging зменшує memory/network pressure і не тримає весь dataset одразу.

13. **Профілювати конкретну причину jank**

Дивитися:

- Layout Inspector recomposition counters;
- Android Studio Profiler;
- System Trace;
- Macrobenchmark;
- image loading/cache metrics;
- allocations на scroll.

Причина може бути не в recomposition, а в image decode, layout, draw, GC або blocking work на main thread.

14. **Практичне правило**

- `LazyColumn`/`LazyRow` замість `Column` для великих списків.
- `key` для dynamic lists.
- `contentType` для mixed lists.
- Immutable UI models і snapshots.
- Heavy work — у ViewModel або `remember` з правильними keys.
- Images — з cache і фіксованими розмірами.
- Scroll-derived state — через `derivedStateOf`.
- Scroll side effects — через `snapshotFlow`.
- Спочатку профілювати, потім оптимізувати.

**Коротко:** щоб уникнути лагів у Compose під час скролу, item має бути легким, дані — stable/immutable, lazy list — з `key` і за потреби `contentType`, images — оптимізовані, а scroll-derived логіка — винесена в `derivedStateOf` або `snapshotFlow`.

</details>
<details>
<summary>225. Чим відрізняються анотації Stable та Immutable?</summary>

#### Kotlin

У Jetpack Compose `@Stable` і `@Immutable` — це підказки Compose compiler/runtime про стабільність типів. Вони допомагають Compose вирішувати, чи можна пропустити recomposition, якщо параметри composable не змінились.

1. **Навіщо потрібна стабільність**

Compose часто викликає composable повторно. Якщо параметри стабільні й не змінились, Compose може skip-нути частину recomposition.

```kotlin
@Composable
fun UserCard(user: UserUi) {
    Text(user.name)
}
```

Якщо `UserUi` стабільний, Compose краще розуміє, чи треба перемальовувати `UserCard`.

2. **@Immutable**

`@Immutable` означає, що обʼєкт після створення не змінюється:

```kotlin
@Immutable
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Усі public properties мають бути immutable і самі мати stable/immutable типи. `val` — необхідна, але не завжди достатня умова.

3. **@Stable**

`@Stable` означає, що тип може змінюватись, але Compose може відстежити ці зміни або має гарантії стабільної поведінки:

```kotlin
@Stable
class CounterState {
    var count by mutableStateOf(0)
}
```

Тут `count` mutable, але це Compose state, тому зміни observable для Compose.

4. **Головна різниця**

```text
@Immutable -> обʼєкт не змінюється після створення
@Stable    -> обʼєкт може змінюватись, але зміни observable/передбачувані
```

`@Immutable` сильніша гарантія. `@Stable` гнучкіша, але її легше використати неправильно.

5. **Поганий приклад @Immutable**

```kotlin
@Immutable
data class FeedUi(
    val items: MutableList<PostUi>
)
```

Це неправильно: `MutableList` можна змінити без створення нового object, і Compose може не побачити зміну.

Краще:

```kotlin
@Immutable
data class FeedUi(
    val items: List<PostUi>
)
```

І не мутувати список після створення.

6. **Поганий приклад @Stable**

```kotlin
@Stable
class UserState {
    var name: String = ""
}
```

Якщо `name` не є `mutableStateOf`, Compose не дізнається про зміну. Анотація тут тільки обманює compiler/runtime.

Краще:

```kotlin
@Stable
class UserState {
    var name by mutableStateOf("")
}
```

7. **Коли використовувати**

`@Immutable` добре підходить для UI models:

```kotlin
@Immutable
data class ProfileState(
    val isLoading: Boolean,
    val user: UserUi?
)
```

`@Stable` підходить для state holder classes, які мають observable mutable state.

8. **Коли не використовувати**

Не треба ставити ці анотації “щоб було швидше”. Якщо гарантії неправильні, можна отримати stale UI або складні bugs. Спочатку треба зробити модель реально immutable/stable, і тільки потім анотувати.

9. **Практичне правило**

- Immutable data class з `val` і immutable fields — `@Immutable`.
- Mutable state holder з `mutableStateOf` — `@Stable`.
- Mutable collections усередині — небезпечно.
- Анотації не виправляють погану модель state.
- Перевіряти ефект краще через Compose metrics/profiling.

**Коротко:** `@Immutable` каже Compose, що обʼєкт не змінюється після створення. `@Stable` каже, що обʼєкт має стабільну поведінку, а зміни observable для Compose. `@Immutable` підходить для UI data models, `@Stable` — для state holders. Неправильне використання може привести до stale UI.

</details>
<details>
<summary>226. Навіщо передавати key у LazyColumn і що він собою являє під капотом?</summary>

#### Kotlin

`key` у `LazyColumn` задає стабільну identity елемента списку. Він потрібен, щоб Compose розумів, який item є тим самим item-ом після insert, delete або reorder, і міг правильно зберегти state та переиспользати composition.

1. **Базовий приклад**

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

`user.id` має бути стабільним і унікальним для item-а.

2. **Що буде без key**

Якщо key не передати, Compose фактично орієнтується на позицію item-а. Це може зламати local state, якщо список змінюється.

Наприклад, був список:

```text
0 -> Alice
1 -> Bob
2 -> Kate
```

Потім на початок додали нового item-а:

```text
0 -> John
1 -> Alice
2 -> Bob
3 -> Kate
```

Без stable key state, який був привʼязаний до позиції `0`, може помилково перейти від Alice до John.

3. **Для чого потрібен key**

`key` допомагає:

- зберігати local state item-а;
- коректно працювати з `remember` всередині item;
- уникати плутанини після reorder;
- покращити item animations;
- зменшити зайву роботу при зміні списку.

4. **Поганий key**

Не варто використовувати index:

```kotlin
itemsIndexed(users, key = { index, _ -> index }) { _, user ->
    UserRow(user)
}
```

Index не є стабільною identity, якщо список може змінювати порядок або отримувати insert/delete.

5. **Що key собою являє під капотом**

Під капотом Compose використовує key як ідентифікатор group/item у composition. Це дозволяє зіставити стару composition group з новими даними після зміни списку.

Спрощено:

```text
old composition: key=user_1 -> remembered state A
new list:        key=user_1 -> reuse remembered state A
```

Тобто key допомагає Compose не плутати state між різними item-ами.

6. **key і remember**

```kotlin
items(users, key = { it.id }) { user ->
    var expanded by remember { mutableStateOf(false) }
    UserRow(user, expanded)
}
```

Якщо item переїде на іншу позицію, `expanded` залишиться з тим самим `user.id`, а не з позицією.

7. **key і contentType**

`key` відповідає на питання “який це item?”.

`contentType` відповідає на питання “який це тип UI?”.

```kotlin
items(
    items = feed,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

Для mixed lists краще використовувати обидва.

8. **Яким має бути key**

Хороший key:

- стабільний;
- унікальний у межах списку;
- не залежить від позиції;
- не генерується випадково;
- базується на domain id або stable local id.

Погано:

```kotlin
key = { UUID.randomUUID() }
```

Такий key змінюється при кожній recomposition і ламає reuse.

9. **Практичне правило**

Для production `LazyColumn` майже завжди передавай `key`, якщо список може змінюватись. Якщо список статичний і маленький, це менш критично, але stable key все одно робить поведінку передбачуванішою.

**Коротко:** `key` у `LazyColumn` — це стабільний ідентифікатор item-а. Він дозволяє Compose зіставити старий і новий item після змін списку, зберегти local state, не плутати `remember` між позиціями і краще переиспользати composition. Найкращий key — stable domain id.

</details>
<details>
<summary>227. Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?</summary>

#### Kotlin

У `LazyColumn` функція `items()` приймає не тільки список і item content. Важливі параметри — `key` і `contentType`. Вони допомагають Compose правильно зберігати state елементів і ефективніше переиспользувати composition.

1. **Базовий приклад**

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Це працює, але для production-списків краще явно передавати `key`, а для списків з різними типами item-ів — ще й `contentType`.

2. **key**

`key` задає стабільну identity item-а:

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

Без key Compose орієнтується на позицію. Якщо елементи додаються, видаляються або міняються місцями, state може привʼязатись не до того item-а.

3. **Навіщо потрібен key**

`key` допомагає:

- зберегти local state item-а;
- правильно працювати з animations;
- уникнути плутанини при reorder;
- зменшити зайві recompositions;
- стабільніше працювати зі списками після insert/delete.

4. **contentType**

`contentType` описує тип UI-елемента:

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

Якщо список містить header, post, ad, loader — це різні content types.

5. **Для чого використовується contentType**

`contentType` допомагає Compose ефективніше переиспользувати composition для однотипних елементів. Наприклад, post item краще переиспользувати як post item, а header — як header, а не змішувати їх.

6. **Приклад sealed model**

```kotlin
sealed interface FeedItem {
    val id: String

    data class Header(override val id: String, val title: String) : FeedItem
    data class Post(override val id: String, val text: String) : FeedItem
    data class Ad(override val id: String) : FeedItem
}
```

```kotlin
LazyColumn {
    items(
        items = feedItems,
        key = { it.id },
        contentType = { item -> item::class }
    ) { item ->
        when (item) {
            is FeedItem.Header -> HeaderRow(item)
            is FeedItem.Post -> PostRow(item)
            is FeedItem.Ad -> AdRow(item)
        }
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
    UserRow(index = index, user = user)
}
```

Але не варто використовувати index як key, якщо порядок списку може змінюватись.

8. **Поганий key**

Погано:

```kotlin
key = { index }
```

або:

```kotlin
key = { Random.nextInt() }
```

Key має бути стабільним і унікальним для item-а, зазвичай `id`.

9. **Коли contentType не потрібен**

Якщо всі items мають однакову структуру UI, `contentType` можна не вказувати. Найбільше користі він дає для mixed lists: feed, chat, catalog з headers/loaders/ads.

10. **Практичне правило**

- Для production list майже завжди додавай `key`.
- Для mixed item types додавай `contentType`.
- Не використовуй position як key для mutable lists.
- `contentType` має описувати UI shape, а не конкретний id.

**Коротко:** у `LazyColumn.items()` важливі параметри `key` і `contentType`. `key` задає стабільну identity item-а, щоб Compose правильно зберігав state. `contentType` описує тип UI-елемента і допомагає ефективніше переиспользувати composition для mixed lists.

</details>
