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

Скасування та помилки в корутинах базуються на `Job`, structured concurrency і cooperative cancellation.

### Скасування

Кожна корутина має `Job`:

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancel()
job.join()
// або job.cancelAndJoin()
```

`cancel()` лише надсилає сигнал. Корутина завершується на cancellation point, наприклад `delay()`, або після явної перевірки стану. CPU-bound код має перевіряти `isActive` чи викликати `ensureActive()`:

```kotlin
scope.launch(Dispatchers.Default) {
    while (isActive) {
        doSmallChunkOfWork()
    }
}
```

### CancellationException

Скасування реалізоване через `CancellationException`, тому її не можна ковтати як звичайну помилку:

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

Широкий `catch (Exception)` або `runCatching` потребує обережності: якщо перехоплено `CancellationException`, її треба повторно кинути.

### launch і async

У `launch` необроблений exception поширюється до parent і зазвичай скасовує його children. У UI помилку краще перетворити на state локально:

```kotlin
viewModelScope.launch {
    state.value = UiState.Loading

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

`async` повертає `Deferred<T>`, а результат або exception отримують через `await()`:

```kotlin
val user = coroutineScope {
    async { repository.loadUser() }.await()
}
```

Exception із child `async` усе одно підпорядковується structured concurrency і може скасувати parent.

### Supervisor і handler

За звичайної ієрархії failure однієї child-корутини скасовує parent та сусідні children. Якщо задачі незалежні, використовують `supervisorScope` або scope з `SupervisorJob`:

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

У supervisor failure однієї child не скасовує інші, але її exception усе одно треба обробити.

`CoroutineExceptionHandler` отримує uncaught exception кореневої `launch`-корутини. Це останній рівень логування або crash reporting, а не заміна `try/catch` для бізнес-логіки. Для `async` exception обробляють біля `await()`.

### Lifecycle

`viewModelScope` автоматично скасовується в `ViewModel.onCleared()`. Custom scope повинен мати явний owner і cleanup:

```kotlin
class SyncManager {
    private val scope = CoroutineScope(SupervisorJob() + Dispatchers.IO)

    fun start() = scope.launch { syncLoop() }
    fun stop() = scope.cancel()
}
```

Не слід створювати scope без зрозумілого lifecycle або використовувати `GlobalScope`.

**Коротко:** корутини скасовуються кооперативно через `Job`. Не можна ковтати `CancellationException`; помилки треба обробляти на відповідному рівні, а для незалежних задач використовувати `SupervisorJob` або `supervisorScope`.

</details>
<details>
<summary>55. Що таке структурована конкурентність (structured concurrency)?</summary>

#### Kotlin

Structured concurrency означає, що async робота має owner-а та входить в ієрархію `Job`. Parent керує lifecycle children, чекає їх завершення та визначає поширення cancellation і failures.

Неструктурований запуск:

```kotlin
fun loadUser() {
    GlobalScope.launch {
        repository.loadUser()
    }
}
```

Caller не може нормально дочекатися, скасувати або отримати помилку цієї роботи. Вона може пережити екран чи бізнес-операцію.

### Parent і children

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

`coroutineScope`:

- створює child scope поточної coroutine;
- не завершується, поки не завершаться всі children;
- повертає result або кидає exception caller-у;
- скасовує children, якщо caller скасований.

Тому lifecycle паралельної роботи обмежений викликом `loadScreen()`.

### Cancellation і failure

Cancellation parent-а поширюється вниз:

```kotlin
val parent = scope.launch {
    launch { syncUser() }
    launch { syncOrders() }
}

parent.cancel()
```

Корутини завершуються кооперативно на suspension points або після `ensureActive()`/`isActive` у CPU-bound коді.

У звичайному `coroutineScope` необроблений failure одного child скасовує parent і sibling children. Це fail-fast семантика для задач, які разом формують одну операцію.

`CancellationException` не слід перетворювати на звичайну помилку: її треба пропускати далі.

### supervisorScope

Для незалежних children використовують supervisor:

```kotlin
supervisorScope {
    launch {
        try {
            loadProfile()
        } catch (exception: IOException) {
            logger.error(exception)
        }
    }

    launch {
        loadRecommendations()
    }
}
```

Failure одного direct child не скасовує siblings і parent supervisor-а. Але exception не зникає: його треба обробити всередині child або передати caller-у відповідним способом.

`SupervisorJob` застосовують для довгоживучого owner scope, `supervisorScope` — локально всередині suspend-функції.

### Android ownership

Типові owners:

- `viewModelScope` — до `ViewModel.onCleared()`;
- `lifecycleScope` — до знищення `LifecycleOwner`;
- `repeatOnLifecycle` — запускає та скасовує collection відповідно до active state;
- application-level custom scope — для роботи, яка справді має пережити screen.

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    fun load() {
        viewModelScope.launch {
            state.value =
                UiState.Success(repository.loadScreen())
        }
    }
}
```

Custom scope має отримувати `Job`, dispatcher та явний метод cleanup від owner-а. Repository зазвичай не повинен приховано запускати fire-and-forget coroutine, якщо lifetime операції належить caller-у.

### Практичні правила

- кожна coroutine має мати чіткий owner;
- suspend API повертає result лише після завершення своєї child-роботи;
- пов'язані задачі — `coroutineScope` із fail-fast;
- незалежні задачі — supervisor із явною обробкою failures;
- не використовувати `GlobalScope` для звичайної app-роботи;
- cancellation має доходити до network, database та callback adapters.

**Коротко:** structured concurrency прив'язує корутини до parent scope. Parent чекає children, cancellation і failures поширюються передбачувано, а async робота не живе довше за свого owner-а; для незалежних children використовують supervisor-семантику.

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

`Flow<T>` — coroutine-native асинхронний потік значень. `suspend`-функція повертає одне значення, а `Flow` може віддавати послідовність значень у часі:

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Flow доречний для стану екрана, змін у базі, пошуку, polling або комбінування джерел даних.

### Створення та collect

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    delay(100)
    emit(2)
}
```

Builder `flow { }` має suspend-контекст, а `emit()` передає значення downstream. Значення отримують термінальним оператором:

```kotlin
viewModelScope.launch {
    numbers().collect { value ->
        println(value)
    }
}
```

Звичайний `Flow` є cold: він не виконується без collector-а й запускає producer заново для кожного `collect`. `StateFlow` і `SharedFlow` — hot streams зі спільним producer-ом.

### Оператори

```kotlin
repository.observeUsers()
    .map { users -> users.filter(User::isActive) }
    .filter(List<User>::isNotEmpty)
    .collect { users -> render(users) }
```

Основні оператори:

- `map`, `filter` — трансформація та фільтрація;
- `combine` — поєднання останніх значень кількох flows;
- `flatMapLatest` — перемикання на новий inner flow зі скасуванням попереднього;
- `collectLatest` — скасування попередньої обробки при новому значенні;
- `onEach` — side effect;
- `catch` — обробка upstream-помилок.

### Context і помилки

`flowOn` змінює dispatcher лише для upstream-частини:

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(api.loadData())
    }.flowOn(Dispatchers.IO)
```

Collector може виконуватися в іншому context, наприклад на Main. Усередині звичайного `flow { }` не можна довільно змінювати context для `emit`; для паралельних producers існує `channelFlow`.

```kotlin
repository.observeData()
    .catch { exception ->
        emit(Data.Empty)
    }
    .collect(::render)
```

`catch` бачить exceptions лише з upstream. Помилка всередині `collect` ним не перехоплюється. Cancellation не треба перетворювати на звичайну помилку.

### Flow в Android

У ViewModel cold flow часто перетворюють на `StateFlow`:

```kotlin
val state: StateFlow<UiState> =
    repository.observeUser()
        .map<User, UiState>(UiState::Success)
        .catch { emit(UiState.Error) }
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

У View-based UI збір прив'язують до lifecycle через `repeatOnLifecycle`; у Compose використовують lifecycle-aware `collectAsStateWithLifecycle()`. Це запобігає зайвій роботі, коли UI неактивний.

Якщо потрібен один результат — краще `suspend fun`. Якщо дані змінюються в часі або потрібна reactive pipeline — `Flow`.

**Коротко:** `Flow` — cold асинхронний потік, який запускається через `collect`. Він підтримує декларативні оператори, cancellation та structured concurrency; для стану UI його часто перетворюють на `StateFlow`.

</details>
<details>
<summary>58. У чому різниця між Flow, StateFlow та SharedFlow?</summary>

#### Kotlin

`Flow`, `StateFlow` і `SharedFlow` — це API для асинхронних потоків даних, але вони вирішують різні задачі. Коротко: `Flow` зазвичай cold stream, `StateFlow` — hot stream зі станом і останнім значенням, `SharedFlow` — hot stream для подій або broadcast.

1. **Flow**

`Flow<T>` — асинхронний потік значень, який за замовчуванням є cold:

```kotlin
fun observeUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

Cold означає, що код всередині flow не виконується, доки хтось не викличе `collect`:

```kotlin
repository.observeUsers().collect { users ->
    println(users)
}
```

Кожен collector зазвичай запускає flow заново. Це добре для pipeline-ів, repository APIs, трансформацій і асинхронних джерел даних.

2. **StateFlow**

`StateFlow<T>` — hot flow, який завжди має поточне значення:

```kotlin
private val _state = MutableStateFlow<UiState>(UiState.Loading)
val state: StateFlow<UiState> = _state.asStateFlow()

fun update(user: User) {
    _state.value = UiState.Success(user)
}
```

Новий collector одразу отримує останнє значення. Тому `StateFlow` добре підходить для UI-state: loading, content, error, selected item, filters, screen model.

3. **SharedFlow**

`SharedFlow<T>` — hot flow для подій або broadcast-сценаріїв. Він не зобовʼязаний мати поточний стан:

```kotlin
private val _events = MutableSharedFlow<UiEvent>()
val events: SharedFlow<UiEvent> = _events.asSharedFlow()

suspend fun showError() {
    _events.emit(UiEvent.ShowSnackbar("Something went wrong"))
}
```

Типові сценарії: snackbar, navigation, toast, analytics event, одноразові UI-команди.

4. **Hot vs cold**

`Flow` зазвичай cold: кожен `collect` стартує виконання заново.

`StateFlow` і `SharedFlow` — hot: вони існують незалежно від collectors, і значення можна оновлювати навіть коли зараз ніхто не слухає.

5. **Останнє значення і replay**

`StateFlow` завжди зберігає останній state:

```kotlin
val state = MutableStateFlow(0)
state.value = 1
state.value = 2
```

Новий collector одразу отримає `2`.

`SharedFlow` може мати `replay`:

```kotlin
val shared = MutableSharedFlow<String>(replay = 1)
```

Але якщо потрібна саме модель поточного стану, краще використовувати `StateFlow`, а не `SharedFlow(replay = 1)`.

6. **Типовий ViewModel-приклад**

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

`StateFlow` тримає стан екрана, `SharedFlow` відправляє одноразову подію.

7. **stateIn і shareIn**

Звичайний `Flow` можна перетворити на `StateFlow` або `SharedFlow`:

```kotlin
val state = repository.observeUser()
    .map { UiState.Success(it) }
    .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5_000), UiState.Loading)
```

```kotlin
val shared = repository.observeData()
    .shareIn(viewModelScope, SharingStarted.WhileSubscribed(), replay = 0)
```

`stateIn` — коли потрібен state. `shareIn` — коли треба поширити один upstream між кількома collectors.

8. **Практичне правило**

- `Flow` — cold pipeline або repository/domain stream.
- `StateFlow` — поточний UI-state.
- `SharedFlow` — one-time events або broadcast.
- Не використовувати `StateFlow` для navigation/snackbar events, щоб не повторити стару подію після resubscribe.

**Коротко:** `Flow` — cold stream для асинхронних значень, `StateFlow` — hot stream із поточним станом, `SharedFlow` — hot stream для подій і broadcast-сценаріїв.

</details>
<details>
<summary>59. У чому різниця між Flow і LiveData?</summary>

#### Kotlin

`Flow` і `LiveData` обидва передають значення в часі, але це різні інструменти. `LiveData` — Android lifecycle-aware observable holder. `Flow` — coroutine-native stream API, не привʼязаний до Android і краще підходить для data/domain layers.

1. **LiveData**

`LiveData<T>` зберігає значення і повідомляє active observers:

```kotlin
private val _user = MutableLiveData<User>()
val user: LiveData<User> = _user

fun update(user: User) {
    _user.value = user
}
```

Головна перевага — lifecycle awareness:

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

Observer отримує оновлення лише коли `LifecycleOwner` у відповідному active state.

2. **Flow**

`Flow<T>` — асинхронний stream значень:

```kotlin
fun observeUser(): Flow<User> =
    userDao.observeUser()
```

Він не залежить від Android lifecycle і може використовуватись у repository, use case, KMM shared module, backend або будь-якому Kotlin-коді.

3. **Lifecycle**

`LiveData` має lifecycle-awareness вбудовано.

`Flow` сам по собі lifecycle не знає. В Android його треба збирати через `repeatOnLifecycle`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

`repeatOnLifecycle` запускає collection у потрібному state і скасовує її при виході з нього.

4. **Threading**

У `LiveData` є main-thread модель:

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

Це гнучкіше для coroutine architecture.

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

У `LiveData` є `map` і `switchMap`, але вони обмеженіші для складних async pipelines.

6. **Cold vs hot**

Звичайний `Flow` зазвичай cold:

```kotlin
val flow = flow {
    emit(api.loadUser())
}
```

Він стартує при `collect`.

`LiveData` більше схожий на hot observable holder: має останнє значення і живе як обʼєкт незалежно від конкретного observer-а.

Якщо у Flow потрібна модель state holder, використовують `StateFlow`:

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

У сучасній архітектурі repository і use case краще не повинні залежати від Android API:

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

`Flow` легше тестувати, переносити і використовувати поза Android UI.

8. **Interop**

Flow у LiveData:

```kotlin
val userLiveData: LiveData<User> =
    repository.observeUser().asLiveData()
```

LiveData у Flow:

```kotlin
val userFlow: Flow<User> =
    userLiveData.asFlow()
```

Це корисно під час міграції legacy-коду.

9. **Що краще для нового коду**

Для нового Kotlin/Android коду зазвичай:

- repository/use case — `Flow`;
- ViewModel state — `StateFlow`;
- one-time events — `SharedFlow`;
- legacy XML/View UI може залишатися на `LiveData`, якщо проєкт уже так побудований.

У Jetpack Compose `StateFlow` зазвичай природніший вибір, ніж `LiveData`.

**Коротко:** `LiveData` привʼязаний до Android lifecycle, а `Flow` — до Kotlin Coroutines. У сучасній архітектурі `Flow` краще тримати в data/domain layers, а в UI перетворювати його на `StateFlow`, `SharedFlow` або `LiveData` залежно від потреб проєкту.

</details>
<details>
<summary>60. У чому різниця між холодними та гарячими потоками?</summary>

#### Kotlin

Різниця — у lifecycle producer-а та зв'язку з collectors:

```text
cold -> окремий producer запускається для кожного collector-а
hot  -> producer/state існує незалежно від конкретного collector-а
```

### Cold Flow

Звичайний `flow { }` є cold:

```kotlin
val userFlow = flow {
    println("Started")
    emit(api.loadUser())
}
```

До `collect` код не виконується. Два collectors запускають upstream двічі:

```kotlin
userFlow.collect(::renderFirst)
userFlow.collect(::renderSecond)
```

Отже, expensive network або database operation може повторитися для кожної підписки. Cold Flow підходить для lazy pipeline, де кожен collector має власне виконання та cancellation.

### StateFlow

`StateFlow` — hot state holder:

```kotlin
private val mutableState =
    MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> =
    mutableState.asStateFlow()
```

Він:

- завжди має current value;
- одразу віддає її новому collector-у;
- зберігає лише останній стан;
- conflates однакові значення за `equals()`;
- не завершується сам через відсутність collectors.

Це основний вибір для screen state. Оновлення `value` відбувається навіть без subscriber-а, і наступний subscriber отримає останній стан.

### SharedFlow

`SharedFlow` — hot broadcast stream із налаштовуваними `replay`, buffer та overflow policy:

```kotlin
private val mutableEvents =
    MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

При `replay = 0` новий collector не отримує старі emissions. Якщо subscribers відсутні, event не зберігається для майбутнього collector-а. `replay > 0` кешує задану кількість останніх значень.

Для одноразових UI events SharedFlow треба використовувати обережно: event може загубитися без active collector-а. Якщо подія має пережити recreation або процес, її краще моделювати як state або зберігати надійніше.

### Перетворення cold у shared hot flow

`stateIn` створює `StateFlow`, `shareIn` — `SharedFlow`:

```kotlin
val state: StateFlow<UiState> =
    repository.observeUser()
        .map<User, UiState>(UiState::Content)
        .stateIn(
            scope = viewModelScope,
            started = SharingStarted.WhileSubscribed(5_000),
            initialValue = UiState.Loading
        )
```

Усі collectors використовують один shared upstream у заданому `scope`.

`SharingStarted` керує upstream:

- `Eagerly` — стартує одразу;
- `Lazily` — після першого subscriber-а;
- `WhileSubscribed` — працює за наявності subscribers і може зупинитися після timeout.

Hot flow не обов'язково означає, що upstream завжди активний: `WhileSubscribed` може його запускати й зупиняти, але collectors усе одно ділять одне shared виконання.

### Практичний вибір

- lazy незалежна operation/pipeline — cold `Flow`;
- актуальний UI state — `StateFlow`;
- broadcast із контрольованим replay/buffer — `SharedFlow`;
- дорогий cold upstream для кількох collectors — `stateIn` або `shareIn`;
- lifecycle UI collector-а — `repeatOnLifecycle` або `collectAsStateWithLifecycle`.

**Коротко:** cold Flow запускає окремий upstream для кожного collector-а. `StateFlow` і `SharedFlow` — hot: вони мають спільний lifecycle та broadcast-ять значення subscribers; `stateIn/shareIn` перетворюють cold pipeline на shared hot flow.

</details>
<details>
<summary>61. Що таке collectLatest і коли його використовувати?</summary>

#### Kotlin

`collectLatest` — це terminal operator для `Flow`, який збирає значення, але скасовує обробку попереднього emission, якщо приходить новий. Його використовують там, де актуальним є тільки останнє значення.

1. **collect vs collectLatest**

`collect` обробляє кожне значення до кінця:

```kotlin
flowOf(1, 2, 3).collect { value ->
    delay(1_000)
    println(value)
}
```

`collectLatest` скасовує попередній block, якщо приходить нове значення:

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value)
}
```

Якщо emissions приходять швидше, ніж завершується обробка, старі обробки не доходять до кінця.

2. **Типовий сценарій — search**

Користувач швидко вводить текст:

```text
k -> ko -> kot -> kotl -> kotlin
```

Немає сенсу завершувати запит для `ko`, якщо вже є `kotlin`.

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

Новий `query` скасує попередній пошук, якщо той ще виконується.

3. **Cancellation має бути cooperative**

`collectLatest` працює через coroutine cancellation. Це добре працює з suspend API:

```kotlin
collectLatest { query ->
    val result = api.search(query)
    render(result)
}
```

Погано:

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

Blocking code не реагує на cancellation миттєво. Для такого коду потрібен suspend API або контрольоване виконання на `Dispatchers.IO`.

4. **Коли collectLatest підходить**

Добрі сценарії:

- live search;
- autocomplete;
- preview для останнього input;
- rendering тільки останнього state;
- запити, де старий результат стає неактуальним;
- швидкі UI input changes.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest { query -> repository.searchFlow(query) }
    .collectLatest { result ->
        state.value = UiState.Success(result)
    }
```

5. **Коли collectLatest не підходить**

Не використовуй `collectLatest`, якщо треба обробити кожне значення:

- analytics events;
- логування кожної події;
- фінансові операції;
- message processing;
- черга задач;
- збереження кожної зміни.

Погано:

```kotlin
paymentsFlow.collectLatest { payment ->
    paymentRepository.process(payment)
}
```

Новий payment може скасувати попередню обробку. Тут потрібен `collect`, queue або інший механізм гарантованої доставки.

6. **collectLatest vs flatMapLatest**

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

`flatMapLatest` краще, коли кожен input створює новий flow. `collectLatest` — коли треба скасувати саме обробку останнього value.

7. **Практичне правило**

- `collect` — коли важливе кожне значення.
- `collectLatest` — коли старе значення стає неактуальним після нового.
- `mapLatest`/`flatMapLatest` — коли cancellation потрібен вище в pipeline.

**Коротко:** `collectLatest` збирає `Flow`, але при новому emission скасовує обробку попереднього. Це правильний інструмент для search, autocomplete, preview і UI-сценаріїв, де потрібен тільки найсвіжіший результат.

</details>
<details>
<summary>62. Як перетворити callback-based API у suspend-функцію?</summary>

#### Kotlin

Single-shot callback API обгортають у `suspendCancellableCoroutine`. Вона призупиняє coroutine без блокування thread і дозволяє скасувати underlying operation.

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

- success перетворюється на return value через `resume()`;
- error — на exception через `resumeWithException()`;
- cancellation — на `cancel` або `unregister` зовнішнього API.

Cancellation coroutine не зупиняє callback API автоматично, тому cleanup у `invokeOnCancellation` обов'язковий, якщо API його підтримує.

### Listener API

Якщо callback реєструється як listener, його треба видалити після результату та при cancellation:

```kotlin
suspend fun Sensor.awaitFirstValue(): Value =
    suspendCancellableCoroutine { continuation ->
        val listener = object : SensorListener {
            override fun onValue(value: Value) {
                unregister(this)
                if (continuation.isActive) {
                    continuation.resume(value)
                }
            }
        }

        register(listener)

        continuation.invokeOnCancellation {
            unregister(listener)
        }
    }
```

Continuation можна завершити лише один раз. Якщо source здатний одночасно викликати success/error кілька разів, `isActive` недостатньо через race condition — потрібна гарантія single callback, atomic guard або `tryResume`/`completeResume`.

Також adapter має коректно працювати, якщо callback викликається синхронно прямо з `register()`.

### suspendCoroutine

`suspendCoroutine` не має cancellation hook. Її використовують лише коли operation неможливо скасувати й callback гарантовано одноразовий. Для Android API зазвичай безпечніше `suspendCancellableCoroutine`.

Не треба створювати `GlobalScope`, blocking latch або busy loop: callback уже можна напряму перетворити на continuation.

### Multi-shot callback

Якщо callback повертає багато значень, потрібен `callbackFlow`, а не suspend-функція:

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

`awaitClose` виконує cleanup під час cancellation або закриття flow. Якщо події не можна втрачати, треба явно вибрати buffer policy та обробляти результат `trySend()`.

**Коротко:** один callback/result обгортають у `suspendCancellableCoroutine`: `resume` для success, `resumeWithException` для error, cancel/unregister для cancellation. Багато callback-значень адаптують через `callbackFlow` і `awaitClose`.

</details>
<details>
<summary>63. У чому різниця між Channel і Flow?</summary>

#### Kotlin

`Channel` і `Flow` обидва передають значення між корутинами, але це різні абстракції. `Channel` — низькорівнева асинхронна черга для producer-consumer комунікації. `Flow` — декларативний stream API для асинхронних послідовностей даних.

1. **Channel**

`Channel<T>` схожий на queue:

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

Producer явно викликає `send`, consumer читає через `receive` або `for`. Також потрібно думати про closing, buffer capacity і cancellation.

2. **Flow**

`Flow<T>` описує pipeline значень:

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

`Flow` краще підходить для repository streams, UI state, database observations, search queries і трансформацій даних.

3. **Cold vs hot**

Звичайний `Flow` — cold: він стартує тільки при `collect`.

```kotlin
val users = flow {
    emit(loadUsers())
}
```

`Channel` — hot primitive: producer може відправляти значення незалежно від collector-а. Якщо consumer не читає, поведінка залежить від capacity.

4. **Backpressure**

У `Channel` backpressure задається buffer capacity:

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // suspend until receiver receives value
```

Типові режими:

- `RENDEZVOUS`/`0` — `send` чекає `receive`;
- `BUFFERED` — є буфер;
- `CONFLATED` — зберігається останнє значення;
- `UNLIMITED` — може привести до memory growth.

У `Flow` для цього частіше використовують оператори:

```kotlin
flow
    .buffer()
    .conflate()
    .collectLatest { value -> process(value) }
```

5. **Коли використовувати Channel**

`Channel` доречний, коли потрібна явна coroutine-to-coroutine комунікація:

- worker queue;
- actor-like модель;
- fan-in/fan-out;
- ручне producer-consumer керування;
- передача задач між корутинами.

```kotlin
val tasks = Channel<Task>(capacity = Channel.BUFFERED)

repeat(4) {
    launch {
        for (task in tasks) {
            process(task)
        }
    }
}
```

6. **Коли використовувати Flow**

`Flow` краще для application-level streams:

```kotlin
fun observeScreenState(): Flow<UiState> =
    combine(
        userRepository.observeUser(),
        settingsRepository.observeSettings()
    ) { user, settings ->
        UiState.Content(user, settings)
    }
```

Для UI state зазвичай беруть `StateFlow`, для broadcast events — `SharedFlow`.

7. **receiveAsFlow і callbackFlow**

Channel можна віддати як Flow:

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

Але `receiveAsFlow()` розподіляє значення між collectors, а не broadcast-ить кожне значення всім.

`callbackFlow` використовують як bridge з callback API:

```kotlin
fun observeLocation(): Flow<Location> = callbackFlow {
    val listener = LocationListener { location ->
        trySend(location)
    }

    client.addListener(listener)
    awaitClose { client.removeListener(listener) }
}
```

Ззовні це `Flow`, всередині — channel-like bridge.

**Коротко:** `Channel` — асинхронна черга для низькорівневої coroutine communication. `Flow` — декларативний stream API для даних у часі. У прикладному коді частіше починають з `Flow`, а `Channel` залишають для спеціальних producer-consumer сценаріїв.

</details>
<details>
<summary>64. Які основні оператори Flow (map, filter, combine, zip)?</summary>

#### Kotlin

Оператори `Flow` дозволяють трансформувати, фільтрувати і комбінувати асинхронні потоки даних. Найчастіше використовують `map`, `filter`, `combine`, `zip`, `flatMapLatest`, `catch`, `onEach`, `stateIn`, `shareIn`.

1. **map**

`map` перетворює кожне значення:

```kotlin
val names: Flow<List<String>> = usersFlow.map { users ->
    users.map { it.name }
}
```

Типовий use case — mapping DTO/entity/domain у UI model.

2. **filter**

`filter` пропускає тільки значення, які відповідають умові:

```kotlin
val activeUsers = usersFlow.map { users ->
    users.filter { it.isActive }
}
```

Для самого Flow:

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

3. **combine**

`combine` обʼєднує останні значення кількох Flow. Emit відбувається, коли будь-який з них дає нове значення після того, як усі вже щось emit-нули.

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

Добре підходить для UI state, який залежить від кількох джерел.

4. **zip**

`zip` обʼєднує значення попарно:

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

Результат: `1A`, `2B`, `3C`. Для UI state `combine` зазвичай корисніший, ніж `zip`.

5. **flatMapLatest**

`flatMapLatest` перемикається на новий inner Flow і скасовує попередній:

```kotlin
val results = queryFlow.flatMapLatest { query ->
    repository.search(query)
}
```

Корисно для search: якщо користувач ввів новий query, старий запит більше не потрібен.

6. **onEach**

`onEach` виконує side action для кожного emission:

```kotlin
flow.onEach { value ->
    logger.log(value)
}
```

Але не варто класти важку бізнес-логіку в `onEach`.

7. **catch**

`catch` обробляє exceptions upstream:

```kotlin
repository.users()
    .catch { emit(emptyList()) }
```

Важливо: `catch` не ловить помилки, які сталися після нього downstream.

8. **stateIn**

`stateIn` перетворює cold Flow у hot `StateFlow`:

```kotlin
val state = repository.users()
    .map { UsersState.Content(it) }
    .stateIn(viewModelScope, SharingStarted.WhileSubscribed(5_000), UsersState.Loading)
```

Це типовий pattern у ViewModel.

9. **shareIn**

`shareIn` робить shared hot Flow без обовʼязкового current value:

```kotlin
val events = source.shareIn(
    scope = viewModelScope,
    started = SharingStarted.WhileSubscribed(),
    replay = 0
)
```

Корисно для shared streams/events.

10. **Практичне правило**

- `map` — трансформація.
- `filter` — відсіювання.
- `combine` — UI state з кількох потоків.
- `zip` — попарне обʼєднання.
- `flatMapLatest` — search/latest request.
- `catch` — error handling upstream.
- `stateIn` — Flow у `StateFlow` для UI.
- `shareIn` — shared hot stream.

**Коротко:** основні оператори Flow: `map` трансформує, `filter` відсіює, `combine` обʼєднує останні значення потоків, `zip` обʼєднує попарно, `flatMapLatest` скасовує старий inner Flow, `catch` ловить помилки, `stateIn` і `shareIn` роблять hot streams.

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

`@JvmStatic`, `@JvmOverloads` і `@JvmField` — це Kotlin-анотації для сумісності з Java. Вони не потрібні для звичайного Kotlin-коду, але важливі, коли API має зручно викликатися з Java, Android framework або legacy Java-коду.

1. **Навіщо вони потрібні**

Kotlin компілюється в JVM bytecode, але не всі Kotlin-конструкції виглядають у Java так, як очікує Java-розробник.

Наприклад, `companion object`:

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

З Java без `@JvmStatic`:

```java
User user = UserFactory.Companion.create();
```

2. **@JvmStatic**

`@JvmStatic` генерує справжній static method або static property accessor для Java.

```kotlin
class UserFactory {
    companion object {
        @JvmStatic
        fun create(): User = User()
    }
}
```

З Java:

```java
User user = UserFactory.create();
```

Типові місця використання: `companion object`, `object`, Java-facing API, factory methods, public library API.

3. **@JvmOverloads**

Kotlin має default arguments:

```kotlin
class UserService(
    private val retryCount: Int = 3,
    private val timeoutMs: Long = 5_000
)
```

Java не підтримує Kotlin default arguments напряму. `@JvmOverloads` генерує кілька overload-конструкторів або методів:

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

Для функцій:

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

Overload-и генеруються з кінця параметрів, тому default parameters краще ставити після обовʼязкових.

4. **@JvmOverloads в Android Views**

Типовий Android-сценарій — custom view:

```kotlin
class AvatarView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr)
```

Це робить constructor зручним для Android framework і Java interop.

5. **@JvmField**

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

6. **@JvmField у companion object**

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

Для compile-time constants краще `const val`, якщо тип підходить:

```kotlin
const val API_VERSION = "v1"
```

7. **Коли не треба використовувати**

Якщо код використовується тільки з Kotlin, ці анотації часто зайві:

```kotlin
class UserMapper {
    companion object {
        fun map(dto: UserDto): User = User(dto.name)
    }
}
```

У Kotlin і так нормальний виклик:

```kotlin
UserMapper.map(dto)
```

Не треба додавати `@JvmStatic` або `@JvmOverloads` “про всяк випадок”, бо це розширює bytecode API і створює зайві entry points.

8. **Практичне правило**

- `@JvmStatic` — коли Java має викликати method/property як static.
- `@JvmOverloads` — коли Java має бачити overload-и замість Kotlin default args.
- `@JvmField` — коли Java має доступатися до field напряму без getter/setter.
- Для чистого Kotlin API ці анотації зазвичай не потрібні.

**Коротко:** ці анотації потрібні не для Kotlin-логіки, а для форми JVM API. Вони роблять Kotlin-код природнішим для Java та Android framework, але застосовувати їх варто тільки там, де реально є Java interop-потреба.

</details>
<details>
<summary>67. Що таке делегування (delegation)?</summary>

#### Kotlin

Делегування — це підхід, коли обʼєкт передає частину своєї поведінки іншому обʼєкту. У Kotlin делегування підтримується на рівні мови: є делегування інтерфейсів через `by` і делеговані властивості (`lazy`, `observable`, custom delegates).

1. **Ідея делегування**

Замість наслідування можна винести поведінку в окремий обʼєкт і делегувати йому виклики.

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

`UserService` формально реалізує `Logger`, але методи `Logger` делегуються обʼєкту `logger`.

Компілятор фактично генерує forwarding-код:

```kotlin
override fun log(message: String) {
    logger.log(message)
}
```

3. **Чому це краще за inheritance**

Делегування базується на composition:

```kotlin
class FileRepository(
    private val cache: Cache,
    private val logger: Logger
)
```

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

Тут `Logger` делегується, але `log` має custom behavior.

5. **Делеговані властивості**

Kotlin також підтримує property delegation:

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

`lazy` ініціалізує значення тільки при першому доступі.

6. **observable і vetoable**

```kotlin
var username: String by Delegates.observable("") { property, oldValue, newValue ->
    println("${property.name}: $oldValue -> $newValue")
}
```

`observable` дозволяє реагувати на зміну property.

`vetoable` може відхилити нове значення:

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

```kotlin
name = "  Kotlin  "
println(name) // Kotlin
```

8. **by lazy і thread-safety**

`lazy` має різні режими:

```kotlin
val value by lazy(LazyThreadSafetyMode.SYNCHRONIZED) {
    createValue()
}
```

- `SYNCHRONIZED` — thread-safe за замовчуванням;
- `PUBLICATION` — кілька потоків можуть ініціалізувати, але збережеться одне значення;
- `NONE` — без thread-safety, швидше, але тільки для одного thread.

9. **Делегування в Android**

Типові приклади:

```kotlin
private val viewModel: UserViewModel by viewModels()
```

```kotlin
private val binding by lazy {
    ActivityMainBinding.inflate(layoutInflater)
}
```

`by viewModels()` — delegate, який отримує `ViewModel` з правильним lifecycle owner-ом.

10. **Типові помилки**

- Використовувати inheritance там, де достатньо delegation.
- Ховати складну бізнес-логіку в property delegate без потреби.
- Використовувати `lazy` для обʼєктів, які треба явно очищати.
- Забувати про thread-safety режим `lazy`.
- Делегувати занадто багато інтерфейсів одному класу.

**Коротко:** delegation у Kotlin — це language-level підтримка composition. Вона дозволяє повторно використовувати поведінку без жорсткого inheritance і робить код гнучкішим, тестованішим і простішим для заміни реалізацій.

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

Java Streams можна використовувати в Kotlin, бо Kotlin на JVM сумісний із Java API. Але в більшості Kotlin-коду частіше використовують Kotlin collections, `Sequence` або `Flow`, бо вони краще інтегруються з мовою, null safety і coroutines.

1. **Java Stream напряму**

Якщо є Java collection, можна викликати `stream()`:

```kotlin
val names = users.stream()
    .filter { user -> user.isActive }
    .map { user -> user.name }
    .toList()
```

Це звичайний Java Stream API з Kotlin lambda syntax.

2. **Kotlin collection operators як default**

У Kotlin той самий код зазвичай пишуть так:

```kotlin
val names = users
    .filter { user -> user.isActive }
    .map { user -> user.name }
```

Для більшості collection transformations Kotlin operators читабельніші й природніші.

3. **Java Stream vs Kotlin Sequence**

Kotlin collection operators eager: кожен оператор може створювати проміжну колекцію.

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

- Java API вже повертає `Stream<T>`;
- потрібен interop із Java library;
- є існуючий Java Stream pipeline;
- використовується Java-specific collector;
- codebase змішаний Java/Kotlin.

Приклад:

```kotlin
fun readLines(path: Path): List<String> {
    return Files.lines(path).use { stream ->
        stream
            .filter { line -> line.isNotBlank() }
            .toList()
    }
}
```

`Files.lines()` тримає ресурс, тому stream треба закривати через `use`.

5. **Collectors у Kotlin**

Java:

```java
users.stream()
    .collect(Collectors.groupingBy(User::getRole));
```

Kotlin:

```kotlin
val usersByRole = users.groupBy { it.role }
```

У Kotlin часто є коротші аналоги: `groupBy`, `associateBy`, `partition`, `sumOf`, `joinToString`, `fold`, `reduce`.

6. **Parallel streams**

Java має `parallelStream()`:

```kotlin
val result = users.parallelStream()
    .map { expensiveMap(it) }
    .toList()
```

На Android це треба використовувати обережно: `parallelStream()` працює через common `ForkJoinPool`, може конфліктувати з coroutine dispatchers і давати непередбачувану продуктивність.

У Kotlin зазвичай краще явно керувати concurrency через coroutines:

```kotlin
suspend fun mapUsers(users: List<User>): List<UserUiModel> = coroutineScope {
    users.map { user ->
        async(Dispatchers.Default) {
            expensiveMap(user)
        }
    }.awaitAll()
}
```

І тільки для справді CPU-heavy задач.

7. **Null safety і one-shot behavior**

Java Stream API гірше відображає Kotlin nullability, особливо з platform types. Для nullable values Kotlin API часто явніший:

```kotlin
val names = javaUsers.mapNotNull { it.name }
```

Також Java Stream одноразовий:

```kotlin
val stream = users.stream()

stream.count()
stream.toList() // error: stream already operated upon or closed
```

Kotlin collection можна обробляти багато разів.

8. **Java Stream vs Flow**

Java Stream — синхронний pipeline для in-memory або blocking джерел.

`Flow` — асинхронний stream із coroutine cancellation:

```kotlin
fun observeUsers(): Flow<List<User>>
```

Для asynchronous data streams у Kotlin треба використовувати `Flow`, а не Java Stream.

9. **Практичне правило**

- Для звичайних Kotlin collections — collection operators.
- Для lazy pipeline — `Sequence`.
- Для async stream — `Flow`.
- Java Streams — для Java interop або існуючих Java APIs.
- Streams із I/O закривати через `use`.
- `parallelStream()` на Android використовувати дуже обережно.

**Коротко:** Java Streams у Kotlin працюють, але не є основним інструментом. У Kotlin зазвичай краще використовувати collection operators, `Sequence` для lazy обробки і `Flow` для асинхронних потоків.

</details>
<details>
<summary>78. Що таке Kotlin Multiplatform Mobile (KMM)?</summary>

#### Kotlin

`Kotlin Multiplatform Mobile` або `KMM` — це підхід, де спільна бізнес-логіка пишеться на Kotlin і використовується в Android та iOS застосунках. Зараз частіше говорять ширше — `Kotlin Multiplatform` (`KMP`), бо технологія не обмежується тільки mobile.

1. **Основна ідея**

KMM не означає “один UI для Android і iOS”. Найчастіше шарять domain/data layer, а UI залишають native:

```text
shared Kotlin code
    -> Android app
    -> iOS app
```

У shared module зазвичай виносять:

- domain models;
- use cases;
- repositories;
- validation;
- networking;
- serialization;
- business rules;
- contracts для analytics/storage/platform APIs.

Android UI може бути на Jetpack Compose/View System, iOS UI — на SwiftUI/UIKit.

2. **Типова структура**

```text
:shared
    commonMain
    androidMain
    iosMain

:androidApp
iosApp
```

`commonMain` містить спільний код:

```kotlin
data class User(
    val id: String,
    val name: String
)

interface UserRepository {
    suspend fun loadUser(id: String): User
}
```

`androidMain` і `iosMain` містять platform-specific реалізації.

3. **expect/actual**

Якщо API різний на платформах, використовують `expect/actual`.

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

Common-код працює з єдиним контрактом, а конкретна реалізація підставляється для платформи.

4. **Networking і serialization**

Для shared networking часто використовують `Ktor Client`, а для моделей — `kotlinx.serialization`:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String
)
```

Ці бібліотеки добре підходять для KMP, бо не привʼязані до Android runtime або Java reflection.

5. **Що не варто шарити**

Не все треба переносити в shared module. Зазвичай не варто шарити:

- platform-specific UI;
- Android `Context`;
- iOS `UIViewController`;
- navigation без чіткої abstraction;
- permission handling без abstraction;
- SDK, який існує тільки на одній платформі.

Shared-код має зменшувати складність, а не створювати абстракції заради абстракцій.

6. **Переваги**

- Менше дублювання бізнес-логіки.
- Однакова domain-логіка на Android та iOS.
- Спільні моделі, validation і networking.
- Kotlin type safety і coroutines у shared layer.
- Native UI і native UX залишаються можливими.

7. **Компроміси**

KMM додає вартість:

- складніший Gradle/setup;
- нюанси Kotlin/Swift interop;
- не всі бібліотеки підтримують KMP;
- debugging між платформами складніший;
- команді потрібна KMP-експертиза.

Тому KMM не означає “дешевше вдвічі”. Він корисний, коли справді є багато спільної логіки.

8. **KMM vs Flutter/React Native**

```text
Flutter / React Native -> shared UI + shared logic
KMM/KMP               -> shared logic, native UI
```

Compose Multiplatform розширює можливості shared UI, але класичний KMM-підхід — це shared business layer і native presentation layer.

**Коротко:** KMM — це використання Kotlin Multiplatform для мобільної розробки, де Android та iOS ділять бізнес-логіку, але можуть мати власний native UI. Це не “написати весь застосунок один раз”, а спосіб винести спільний domain/data layer у Kotlin-модуль.

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

У 2026 році сильний Kotlin-розробник має орієнтуватися в Kotlin 2.x, K2 compiler, розвитку Multiplatform, tooling і Compose compiler. Не треба тягнути кожну нову фічу в production, але треба розуміти користь, ризики й сумісність.

1. **Kotlin 2.x і K2 compiler**

K2 — це нова compiler architecture, яка стала фундаментом сучасного Kotlin.

Практично це означає:

- кращу compiler analysis;
- швидший розвиток language features;
- сильнішу основу для IDE/tooling;
- стабільнішу Multiplatform-історію;
- поступовий відхід від Kotlin 1.x як baseline.

Для Android-команди важлива не тільки мова, а й сумісність із AGP, Gradle, Compose compiler, KSP/kapt і бібліотеками.

2. **Context parameters**

Context parameters дозволяють функції вимагати контекст, доступний у scope:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving user $id")
}
```

Це корисно для DSL, scoped APIs, logging або transactions. Але це не заміна DI і не привід будувати прихований service locator.

3. **Explicit backing fields**

Explicit backing fields дають більше контролю над property storage:

```kotlin
var name: String = ""
    field = value.trim()
```

Користь — менше boilerplate, чистіші getter/setter і явніше вираження invariants.

4. **Sealed state і exhaustive `when`**

Kotlin продовжує покращувати аналіз `when` для sealed hierarchies:

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

Розвивається напрям перевірок для результатів, які не можна безпечно ігнорувати:

```kotlin
val result = repository.saveUser(user)

if (result.isFailure) {
    showError()
}
```

Для API-дизайну це означає: якщо функція повертає важливий результат, caller має або використати його, або явно проігнорувати.

6. **Kotlin Multiplatform і Swift export**

KMP важливо знати хоча б концептуально:

```text
common Kotlin logic
 -> Android
 -> iOS
 -> JVM backend
 -> Web/Wasm
```

Для mobile особливо важливі Swift export і Kotlin/Native interop. Shared API треба проектувати так, щоб він був зручний не тільки Kotlin, а й iOS-команді.

7. **Kotlin/Wasm і stdlib**

Kotlin/Wasm — це ще один target, не заміна Android. Його варто знати як частину ширшої multiplatform-екосистеми.

Також розвиваються common stdlib API: UUID, time APIs, Base64/Hex utilities, collection/helper improvements. Це зменшує кількість platform-specific wrappers у shared code.

8. **Gradle, Compose compiler і tooling**

Для техліда tooling не менш важливий за language features:

- Kotlin Gradle Plugin compatibility;
- Gradle/AGP versions;
- KSP/kapt migration risks;
- Compose compiler compatibility;
- generated sources;
- version catalogs;
- binary compatibility checks.

Погано налаштований build може коштувати команді більше, ніж відсутність нової мовної фічі.

9. **Практичне правило**

Перед adoption нової Kotlin-фічі треба оцінити:

- stability level;
- IDE support;
- Android Studio/AGP compatibility;
- Gradle impact;
- library ecosystem;
- team readiness;
- migration cost;
- реальну користь для коду.

**Коротко:** у 2026 році варто знати Kotlin 2.x, K2 compiler, context parameters, explicit backing fields, sealed/exhaustive state, KMP/Swift export, Wasm-напрям, stdlib/tooling покращення і Compose compiler compatibility. Техлід має впроваджувати це прагматично, а не гнатися за фічами.

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

`Activity` — це Android component, який представляє окремий екран або entry point UI. `Fragment` — це reusable частина UI і поведінки, яка живе всередині Activity або іншого Fragment.

1. **Activity**

Activity має власний lifecycle і реєструється в manifest:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Вона може бути entry point-ом app, отримувати intents, керувати window і бути host-ом для Fragment/Compose UI.

2. **Fragment**

Fragment — частина UI всередині Activity:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile) {
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }
}
```

Fragment має lifecycle, але залежить від host Activity і FragmentManager.

3. **Головна різниця**

```text
Activity -> самостійний Android component, має window/task/intent entry
Fragment -> частина UI всередині Activity, керується FragmentManager
```

Fragment не існує як окремий system component на рівні manifest.

4. **Lifecycle**

Activity lifecycle:

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Fragment має ще lifecycle своєї View:

```text
onCreateView -> onViewCreated -> onDestroyView
```

Це важливо: Fragment object може жити довше, ніж його View.

5. **onDestroyView**

У Fragment треба очищати View references:

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Інакше можна отримати memory leak.

6. **Navigation**

Activity navigation часто працює через intents:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Fragment navigation — через FragmentManager або Navigation Component:

```kotlin
findNavController().navigate(R.id.detailsFragment)
```

7. **Reuse і composition**

Fragment можна перевикористати як частину різних layouts:

```text
phone  -> Activity + один Fragment
tablet -> Activity + list Fragment + details Fragment
```

Activity більше підходить як host/container, Fragment — як screen або reusable UI block у View System.

8. **ViewModel scope**

Fragment може мати власну ViewModel або shared ViewModel з Activity:

```kotlin
private val ownViewModel: ProfileViewModel by viewModels()
private val sharedViewModel: SharedViewModel by activityViewModels()
```

Scope треба вибирати залежно від того, кому належить state.

9. **Compose контекст**

У сучасних Compose apps Activity часто host-ить один Compose tree, а Fragment може не використовуватись. У hybrid apps Fragment все ще корисний для navigation, legacy UI або поступової міграції.

10. **Практичне правило**

Activity — це top-level container і Android entry point. Fragment — це частина UI з власним lifecycle, navigation і state scope. У View System Fragment часто представляє screen, але треба уважно працювати з `onDestroyView()` і back stack.

**Коротко:** Activity — самостійний Android component із window, task, intents і manifest. Fragment — reusable UI component всередині Activity, керований FragmentManager. Fragment має окремий View lifecycle, тому головні ризики — leaks після `onDestroyView()`, неправильний scope ViewModel і складний back stack.

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

`@Query`, `@Insert` і `@Delete` — це Room-анотації для DAO-методів. Вони описують, яку SQL-операцію має виконати Room: довільний SQL-запит, вставку або видалення entity. Room генерує implementation DAO і перевіряє SQL на compile time.

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

`:id` — це bind parameter. Room підставить значення безпечно, без ручної конкатенації string.

Погано:

```kotlin
@Query("SELECT * FROM users WHERE id = '$id'")
```

Параметри треба передавати через `:parameterName`.

3. **@Query для списків і Flow**

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

Room може повертати `List<T>`, nullable entity, `Flow<T>`, `LiveData<T>`, `PagingSource`, `suspend` results. Якщо повертається `Flow`, Room emit-ить нові значення при зміні відповідних таблиць.

4. **@Query для UPDATE/DELETE**

`@Query` можна використовувати не тільки для `SELECT`:

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateUserName(id: String, name: String)
```

```kotlin
@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String)
```

Це корисно, коли треба оновити або видалити конкретні поля/рядки за умовою.

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

`REPLACE` не завжди безпечний, бо може фактично видалити старий рядок і вставити новий.

7. **@Delete**

`@Delete` видаляє entity за primary key:

```kotlin
@Delete
suspend fun deleteUser(user: UserEntity)
```

Для списку:

```kotlin
@Delete
suspend fun deleteUsers(users: List<UserEntity>)
```

Якщо треба видалити за `id` без повного entity, краще `@Query`:

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

`@Delete` може повертати кількість видалених рядків:

```kotlin
@Delete
suspend fun deleteUser(user: UserEntity): Int
```

`@Query` для update/delete теж може повертати affected rows:

```kotlin
@Query("DELETE FROM users WHERE isArchived = 1")
suspend fun deleteArchivedUsers(): Int
```

9. **@Update і @Transaction**

Поруч часто використовують `@Update`:

```kotlin
@Update
suspend fun updateUser(user: UserEntity)
```

`@Update` оновлює рядок за primary key. Якщо треба оновити одне поле, частіше краще `@Query`.

Для атомарних multi-step operations використовують `@Transaction`:

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertUsers(users)
}
```

10. **Compile-time перевірка**

Room перевіряє SQL під час компіляції:

```kotlin
@Query("SELECT wrong_column FROM users")
suspend fun brokenQuery(): List<UserEntity>
```

Якщо column не існує або result не мапиться в return type, Room видасть compile-time помилку.

**Коротко:** `@Query` дає контроль над SQL, `@Insert` генерує вставку entity, а `@Delete` видаляє entity за primary key. Room на основі цих анотацій генерує DAO implementation і перевіряє SQL на compile time.

</details>
<details>
<summary>92. Що таке WorkManager і коли його використовувати?</summary>

#### Kotlin

`WorkManager` — це Jetpack API для гарантованої відкладеної background work. Його використовують для задач, які не обовʼязково мають стартувати миттєво, але мають надійно виконатись з урахуванням constraints, retry і process death.

1. **Коли використовувати**

WorkManager підходить для:

- background sync;
- upload/download, який можна відкласти;
- cleanup старих даних;
- відправки logs/analytics;
- retry після network error;
- periodic sync;
- задач із умовами: network, charging, battery not low.

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

Це одноразова задача, яка запуститься, коли буде мережа.

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

`Result.retry()` каже WorkManager повторити задачу пізніше з backoff policy.

4. **Constraints**

```kotlin
val constraints = Constraints.Builder()
    .setRequiredNetworkType(NetworkType.UNMETERED)
    .setRequiresCharging(true)
    .setRequiresBatteryNotLow(true)
    .build()
```

Constraints дозволяють не виконувати задачу в поганих умовах для battery/network.

5. **Periodic work**

```kotlin
val request = PeriodicWorkRequestBuilder<SyncWorker>(
    12, TimeUnit.HOURS
).build()
```

Periodic work не гарантує точний час запуску. Android сам вибирає зручний момент.

6. **Unique work**

```kotlin
WorkManager.getInstance(context).enqueueUniqueWork(
    "sync",
    ExistingWorkPolicy.KEEP,
    request
)
```

`KEEP` не додає дубль, якщо задача вже є. `REPLACE` скасовує стару і ставить нову.

7. **Input data**

```kotlin
val request = OneTimeWorkRequestBuilder<UploadWorker>()
    .setInputData(workDataOf("file_id" to fileId))
    .build()
```

Передавати треба невеликі primitive/string values. Великі обʼєкти краще зберігати в DB і передавати id.

8. **Коли WorkManager не підходить**

Не використовуй WorkManager для:

- точних alarm-ів;
- realtime socket connection;
- media playback;
- постійного location tracking;
- задач, які мають виконатись прямо зараз;
- foreground user-visible роботи без notification.

9. **WorkManager vs Service**

```text
WorkManager -> deferrable guaranteed work з constraints/retry
Service     -> active foreground/user-visible робота або component-bound task
```

Для sync після закриття app — WorkManager. Для музики або active tracking — foreground service.

10. **Практичне правило**

Якщо задачу можна відкласти, але треба виконати надійно — WorkManager. Якщо задача точна, realtime або активна прямо зараз для користувача — треба інший механізм.

**Коротко:** WorkManager використовують для гарантованої відкладеної background work: sync, upload, cleanup, retry і periodic jobs. Він підтримує constraints, retry, unique work і process-death resilience, але не підходить для точних alarm-ів, realtime-задач і foreground media/location сценаріїв.

</details>
<details>
<summary>93. У чому різниця між CoroutineScope, lifecycleScope та viewModelScope?</summary>

#### Kotlin

`CoroutineScope` визначає lifecycle корутин: де вони запускаються, коли скасовуються і як обробляють child jobs. `lifecycleScope` і `viewModelScope` — готові Android scopes, привʼязані до lifecycle конкретних компонентів.

1. **CoroutineScope**

`CoroutineScope` — загальний інтерфейс для запуску корутин:

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Якщо створюєш scope вручну, ти сам відповідаєш за `cancel()`.

2. **lifecycleScope**

`lifecycleScope` доступний у `LifecycleOwner`: Activity, Fragment, viewLifecycleOwner.

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

Scope скасовується, коли lifecycle owner знищується.

3. **viewLifecycleOwner.lifecycleScope**

У Fragment для роботи з View краще використовувати саме `viewLifecycleOwner.lifecycleScope`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state -> render(state) }
    }
}
```

Fragment object може жити довше за View, тому `lifecycleScope` Fragment-а може бути неправильним для binding/View operations.

4. **viewModelScope**

`viewModelScope` привʼязаний до ViewModel:

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Він скасовується в `ViewModel.onCleared()`, тобто переживає configuration changes, але завершується, коли ViewModel більше не потрібна.

5. **Головна різниця**

```text
CoroutineScope  -> загальний scope, lifecycle визначаєш сам
lifecycleScope  -> живе разом з Activity/Fragment lifecycle
viewModelScope  -> живе разом з ViewModel
```

6. **Коли використовувати viewModelScope**

Для бізнес-операцій екрана:

- load data;
- save form;
- call use case;
- update UI state;
- collect repository streams у ViewModel.

ViewModel не має залежати від Activity/Fragment lifecycle напряму.

7. **Коли використовувати lifecycleScope**

Для UI/lifecycle-bound роботи:

- collect state у Fragment/Activity;
- animation tied to UI lifecycle;
- одноразові UI effects;
- робота, яка має зупинитись при destroy конкретного UI owner.

8. **repeatOnLifecycle**

Для Flow collection у UI краще:

```kotlin
repeatOnLifecycle(Lifecycle.State.STARTED) {
    flow.collect { value -> render(value) }
}
```

Це автоматично зупиняє collection, коли екран неактивний, і запускає знову.

9. **Типові помилки**

- створити custom `CoroutineScope` і не cancel-ити його;
- використовувати `GlobalScope`;
- collect Flow у Fragment lifecycle замість viewLifecycleOwner;
- запускати бізнес-логіку в Activity scope замість ViewModel;
- тримати View/binding у coroutine після `onDestroyView()`.

10. **Практичне правило**

- Business/screen work — `viewModelScope`.
- UI collection/effects — `viewLifecycleOwner.lifecycleScope` або `lifecycleScope`.
- Custom scope — тільки якщо є чіткий owner і явний cancel.
- `GlobalScope` у app-коді майже завжди не потрібен.

**Коротко:** `CoroutineScope` — загальний механізм запуску корутин. `lifecycleScope` привʼязаний до Activity/Fragment lifecycle, а `viewModelScope` — до ViewModel. Для бізнес-логіки екрана використовують `viewModelScope`, для UI collection — lifecycle-aware scope, у Fragment бажано `viewLifecycleOwner.lifecycleScope`.

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

Scope у Hilt визначає, в межах якого component зберігається та повторно використовується instance dependency. Lifetime задає component, а scope гарантує один instance на конкретний instance цього component.

Без scope binding є unscoped: Hilt може створювати новий об'єкт для кожного запиту. Це нормально для дешевих stateless mapper-ів і formatter-ів.

### Component hierarchy

Спрощена ієрархія:

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 └── ActivityComponent
      └── FragmentComponent
           └── ViewComponent
```

Parent component не може залежати від binding-а child component, бо child живе менше. Натомість child бачить bindings своїх parents.

### Основні scopes

- `@Singleton` — один instance на application graph;
- `@ActivityRetainedScoped` — один instance на logical Activity, переживає configuration changes;
- `@ViewModelScoped` — один instance на конкретну ViewModel;
- `@ActivityScoped` — один instance на конкретний Activity instance;
- `@FragmentScoped` — один instance на конкретний Fragment;
- `@ViewScoped` — один instance на Hilt-enabled View.

`@Singleton` підходить для Room database, OkHttpClient, Retrofit та app-level services:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object DatabaseModule {

    @Provides
    @Singleton
    fun provideDatabase(
        @ApplicationContext context: Context
    ): AppDatabase =
        Room.databaseBuilder(
            context,
            AppDatabase::class.java,
            "app.db"
        ).build()
}
```

`@ViewModelScoped` використовують для stateful dependency, яка належить одній ViewModel:

```kotlin
@ViewModelScoped
class UserDraftCache @Inject constructor()
```

Дві різні ViewModel отримають різні `UserDraftCache`. Якщо dependency має бути спільною між ViewModel однієї Activity, більше підходить `@ActivityRetainedScoped`.

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

`@ViewModelScoped` не можна встановити в `SingletonComponent`, а `@ActivityScoped` — у `FragmentComponent`. Несумісність Hilt виявляє під час компіляції.

Scope застосовують до binding-а: constructor class або provider/`@Binds` method. Scoped binding кешується component-ом лише після першого запиту.

### Типові помилки

Найнебезпечніша помилка — короткоживучий об'єкт у довгоживучому scope:

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Singleton утримає Activity після її знищення. Binding із `ActivityContext` має жити максимум у Activity-related component.

Також не слід:

- робити singleton кожну dependency;
- плутати `ActivityScoped` з `ActivityRetainedScoped`;
- очікувати один `@ViewModelScoped` instance для різних ViewModel;
- scope-ити immutable дешеві helpers без потреби;
- зберігати Fragment, View або Activity в app-level service.

Scope — це не лише optimization, а ownership і lifecycle semantics.

**Коротко:** Hilt scope задає reuse instance в межах component. `@Singleton` живе на рівні app, retained scope переживає rotation, а ViewModel/Activity/Fragment scopes відповідають своїм owners. Довгоживуча dependency не повинна утримувати короткоживучий Context або UI object.

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

`MVI` — це архітектурний підхід з unidirectional data flow. UI рендерить immutable state, відправляє events/intents, ViewModel обробляє їх і створює новий state. Для одноразових дій використовують effects.

1. **Базова схема**

```text
UI -> Intent/Event -> ViewModel/Reducer -> State -> UI
                              └── Effect
```

- `State` — поточний стан екрана.
- `Intent/Event` — дія користувача або системи.
- `Reducer` — перетворює old state + event у new state.
- `Effect` — одноразова дія: navigation, toast, snackbar.

2. **State, Event, Effect**

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)

sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
    data object BackClicked : ProfileEvent
}

sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowSnackbar(val message: String) : ProfileEffect
}
```

`State` має бути immutable. `Event` описує, що сталося. `Effect` потрібен для одноразових дій, які не можна безпечно тримати як звичайний state.

3. **ViewModel приклад**

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
                .onSuccess { user ->
                    _state.update { it.copy(isLoading = false, user = user) }
                }
                .onFailure {
                    _state.update { it.copy(isLoading = false, error = "Load failed") }
                }
        }
    }

    private fun emitEffect(effect: ProfileEffect) {
        viewModelScope.launch { _effect.emit(effect) }
    }
}
```

4. **Compose UI**

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

UI тільки рендерить `state` і відправляє `event`. Бізнес-логіка лишається у ViewModel/use cases.

5. **Коли MVI корисний**

MVI добре працює, коли:

- екран має багато станів: loading/error/empty/content;
- є багато user actions;
- потрібен один source of truth;
- важлива тестованість ViewModel;
- команда хоче однаковий state-management pattern;
- UI написаний на Compose.

6. **Коли MVI зайвий**

Для простих екранів MVI може дати зайвий boilerplate. Якщо є одна кнопка і кілька полів, часто достатньо `StateFlow` у ViewModel без формального event/reducer/effect шару.

7. **Ризики**

- Забагато boilerplate.
- Events перетворюються на “God events”.
- Effects змішуються зі state.
- Reducer розростається без декомпозиції.
- Простий UI стає надмірно складним.

**Коротко:** MVI — це unidirectional data flow: UI відправляє events, ViewModel оновлює immutable state, UI рендерить новий state, а одноразові дії йдуть через effects. Його варто використовувати для складних stateful екранів, але не для кожного простого UI.

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

У Compose `padding` і `offset` обидва можуть візуально змістити елемент, але працюють на різних етапах layout pipeline. `padding` змінює constraints і місце елемента в layout. `offset` зміщує вже розміщений елемент візуально і зазвичай не змінює місце, яке він займає для parent-а.

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

`padding` впливає на measurement і placement child-а:

```text
parent дає constraints -> padding зменшує доступний простір -> child вимірюється
```

Тобто `padding` бере участь у layout.

2. **offset зміщує позицію**

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` зміщує елемент від його початкової позиції:

```text
child виміряли -> child розмістили -> offset змістив його візуально
```

Parent зазвичай продовжує вважати, що елемент займає старе місце.

3. **Головна різниця**

```text
padding -> впливає на розмір/constraints/місце в layout
offset  -> зміщує елемент після layout
```

Приклад з `offset`:

```kotlin
Row {
    Text(
        text = "A",
        modifier = Modifier.offset(x = 20.dp)
    )
    Text("B")
}
```

`A` зміститься вправо, але `B` не зарезервує для цього додаткове місце. Може бути overlap.

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

4. **Порядок modifier-ів**

Порядок важливий:

```kotlin
Box(
    modifier = Modifier
        .background(Color.Red)
        .padding(16.dp)
)
```

Фон включає область padding.

```kotlin
Box(
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
)
```

Фон застосовується вже після padding, тобто до внутрішньої області.

5. **Коли використовувати padding**

`padding` потрібен, коли треба:

- додати реальний відступ;
- збільшити touch area;
- створити внутрішній простір;
- вплинути на розміщення сусідніх елементів;
- зробити layout передбачуваним.

```kotlin
Column(
    modifier = Modifier.padding(16.dp)
) {
    Text("Title")
    Text("Subtitle")
}
```

6. **Коли використовувати offset**

`offset` доречний, коли треба:

- зробити visual adjustment;
- реалізувати badge;
- змістити елемент в animation;
- зробити overlap;
- не впливати на layout сусідів.

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

7. **Offset і overlap**

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

8. **Додаткові нюанси**

- `Modifier.offset(x, y)` враховує layout direction для horizontal direction.
- `Modifier.absoluteOffset(x, y)` не враховує RTL/LTR.
- Для animation/state-based offset можна використовувати lambda version:

```kotlin
Modifier.offset {
    IntOffset(
        x = animatedOffset.roundToPx(),
        y = 0
    )
}
```

9. **Практичне правило**

- Потрібен реальний простір у layout — `padding`, `Spacer`, `Arrangement`.
- Потрібен тільки візуальний зсув — `offset`.
- Якщо сусідні елементи мають враховувати відступ — це не `offset`.
- Якщо потрібен overlap або badge — `offset` доречний.
- Завжди перевіряти порядок modifier-ів.

**Коротко:** `padding` бере участь у вимірюванні й розміщенні, тому змінює layout. `offset` зміщує елемент після layout і зазвичай не впливає на позицію сусідів. Для spacing — `padding`, для visual shift або overlap — `offset`.

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

`rememberUpdatedState()` використовують, коли long-running effect або callback має бачити найновіше значення, але сам effect не повинен перезапускатися при кожній зміні цього значення. Це захищає від stale capture у `LaunchedEffect`, `DisposableEffect` та інших side effects.

1. **Проблема stale capture**

Composable може recomposed з новими параметрами, але coroutine, запущена раніше, може тримати стару lambda.

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

`LaunchedEffect(Unit)` не перезапускається при зміні `onTimeout`, тому coroutine може викликати стару версію callback-а.

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

Effect не перезапускається, але всередині coroutine буде актуальна lambda.

3. **Ключова ідея**

```text
LaunchedEffect(Unit)        -> effect живе стабільно
rememberUpdatedState(value) -> value всередині effect завжди актуальний
```

Це корисно, коли restart effect-а небажаний: наприклад, timer не має стартувати заново, але callback після timeout має бути найновіший.

4. **Чому не додати lambda в key**

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

Такий код перезапустить effect при кожній зміні `onTimeout`. Для splash timer це може означати, що таймер щоразу починається спочатку.

Якщо restart не потрібен, краще:

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

Observer реєструється для `lifecycleOwner`, але callbacks завжди актуальні.

6. **Коли не використовувати**

Якщо зміна value має перезапустити effect, value має бути key:

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Погано:

```kotlin
val currentUserId by rememberUpdatedState(userId)

LaunchedEffect(Unit) {
    viewModel.loadUser(currentUserId)
}
```

Це помилка, бо при зміні `userId` треба завантажити іншого користувача.

7. **Практичне питання**

Перед використанням треба відповісти:

```text
Чи має effect перезапуститися при зміні цього value?
```

Якщо так — value має бути key. Якщо ні — можна використовувати `rememberUpdatedState`.

8. **Практичне правило**

Використовувати `rememberUpdatedState`, коли:

- effect стартує один раз або за іншими keys;
- callback/value може змінюватися;
- всередині effect потрібна latest value;
- restart effect-а небажаний.

Не використовувати, коли value є input-ом для завантаження даних або key semantic важливий для correctness.

**Коротко:** `rememberUpdatedState()` потрібен, щоб long-running side effect бачив найновішу lambda або value без перезапуску самого effect. Якщо зміна value має restart-ити effect — треба використовувати key, а не `rememberUpdatedState`.

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

`key` у `LazyColumn` потрібен, щоб Compose міг стабільно ідентифікувати item незалежно від його позиції. Без key Compose переважно орієнтується на index, що може ламати local state при insert/delete/reorder.

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

Було:

```text
index 0 -> Alice
index 1 -> Bob
index 2 -> Charlie
```

Вставили item на початок:

```text
index 0 -> Kate
index 1 -> Alice
index 2 -> Bob
index 3 -> Charlie
```

Без key Compose може сприймати це як зміну item-ів на позиціях. З key Compose розуміє, що `Alice`, `Bob` і `Charlie` просто змістилися.

3. **Локальний state item-а**

Проблема особливо помітна, якщо item має local state:

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

Без key після insert/delete `expanded` може “переїхати” до іншого item-а, бо state був привʼязаний до позиції.

4. **Що key означає під капотом**

Концептуально key — це identity token для item-а в composition.

Compose використовує key, щоб:

- зіставити старі й нові item-и між recompositions;
- зберегти remembered state для конкретного item-а;
- коректно обробити reorder/insert/delete;
- оптимізувати reuse composition;
- підтримати item animations.

Це не обовʼязково database key, але він має стабільно представляти identity item-а.

5. **Key має бути stable**

Добре:

```kotlin
key = { it.id }
```

Погано:

```kotlin
key = { UUID.randomUUID().toString() }
```

Або:

```kotlin
key = { System.currentTimeMillis() }
```

Такий key змінюється на кожну recomposition, тому Compose вважатиме item новим щоразу.

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

Index змінюється при insert/delete/reorder, тому це майже те саме, що не мати key. Index прийнятний тільки для повністю статичних списків.

8. **Key і animations/Paging**

Для item animations key критично важливий:

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

У Paging Compose також задають key:

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let { UserItem(it) }
}
```

9. **Практичне правило**

- Для `LazyColumn` використовувати stable unique key.
- Найкращий key — server/database id.
- Не використовувати random, time або index як key.
- Якщо item має local state — key обовʼязковий.
- Для reorder/insert/delete/animation — key обовʼязковий.
- Key має представляти identity, а не position.

**Коротко:** `key` у `LazyColumn` — це стабільна identity item-а для Compose runtime. Він потрібен, щоб правильно зберігати state, зіставляти item-и між оновленнями списку й коректно обробляти reorder, insert, delete та animations.

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

ООП — підхід, у якому система моделюється об'єктами, що поєднують стан і поведінку. Клас описує тип, а об'єкт є його instance:

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Основні принципи ООП — інкапсуляція, абстракція, наслідування та поліморфізм.

### Інкапсуляція

Клас приховує mutable state і дозволяє змінювати його лише контрольованими операціями:

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

Це захищає invariants класу. Повернення `List` замість `MutableList` обмежує API, але для повного захисту треба не віддавати назовні mutable implementation.

### Абстракція

Абстракція описує потрібний contract без деталей реалізації:

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

ViewModel працює з contract і не залежить від Retrofit, Room, cache або fake implementation. Абстракція виправдана, коли ізолює мінливу деталь чи має кілька реалізацій, а не просто додає interface до кожного класу.

### Наслідування

У Kotlin класи й методи `final` за замовчуванням. Наслідування треба дозволити явно:

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

Наслідування моделює відношення «is-a», але створює сильний зв'язок із base class. У прикладному Android-коді композиція часто простіша й безпечніша.

### Поліморфізм

Різні реалізації можна використовувати через спільний contract:

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

Caller залежить від `ImageLoader`, тому production і test implementations взаємозамінні.

### ООП у Kotlin/Android

Типові застосування:

- ViewModel, Repository, UseCase — об'єкти з чіткою відповідальністю;
- interfaces — межі між шарами та зовнішніми системами;
- sealed class/interface — закрита ієрархія станів;
- data class — value-like моделі;
- dependency injection — передача залежностей через constructor.

```kotlin
sealed interface LoginState {
    data object Idle : LoginState
    data object Loading : LoginState
    data class Error(val message: String) : LoginState
    data object Success : LoginState
}
```

Композиція виражає можливість «has-a» і зазвичай краща за base classes:

```kotlin
class UserRepository(
    private val errorMapper: ErrorMapper,
    private val logger: Logger
)
```

ООП не виключає immutability, extension functions і функціональний стиль Kotlin. Важливіша модель відповідальностей, ніж кількість класів.

**Коротко:** ООП організовує код навколо об'єктів зі станом і поведінкою. Його основи — інкапсуляція, абстракція, наслідування та поліморфізм; у Kotlin перевагу часто віддають immutable моделям і композиції замість глибокої ієрархії наслідування.

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

Конструктор створює та ініціалізує обʼєкт. Метод описує поведінку вже створеного обʼєкта. Тобто constructor викликається під час `ClassName(...)`, а method — після створення instance через `object.method()`.

1. **Конструктор**

Primary constructor у Kotlin оголошується в заголовку класу:

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

Параметри з `val` або `var` стають властивостями класу. Constructor відповідає за початковий валідний state обʼєкта.

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

Метод виконує дію або повертає результат, але не створює сам обʼєкт.

3. **init block**

Якщо потрібна валідація або проста ініціалізація, використовують `init`:

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

`init` виконується під час створення instance.

4. **Secondary constructor**

Kotlin підтримує secondary constructors:

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

Але частіше краще default parameters:

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Це простіше й читабельніше.

5. **Методи можна викликати багато разів**

```kotlin
class Counter {
    private var value = 0

    fun increment() {
        value++
    }

    fun currentValue(): Int = value
}
```

```kotlin
val counter = Counter()
counter.increment()
counter.increment()
val value = counter.currentValue()
```

Обʼєкт створюється один раз, а методи можуть викликатися багато разів.

6. **Constructor injection**

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

Constructor отримує залежності, а метод `invoke()` виконує поведінку.

7. **Що не варто робити в constructor**

Конструктор має бути легким. Не варто запускати там:

- network request;
- database query;
- coroutine;
- важку IO-операцію;
- side effects;
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

8. **Практичне правило**

- Constructor — створення object-а, DI і базова валідація state.
- Method — поведінка після створення object-а.
- Constructor не має return type.
- Method може мати return type.
- Важку або lifecycle-dependent роботу краще запускати з методів, не з constructor-а.

**Коротко:** конструктор створює й ініціалізує обʼєкт, а метод виконує дії вже створеного обʼєкта. В Android constructor часто використовують для dependency injection, а методи — для business/UI operations.

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

`public` — visibility за замовчуванням, тому його зазвичай не пишуть:

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Public declaration формує API, від якого можуть залежати інші модулі або callers. Відкривати implementation details без потреби не слід.

### private

Member із `private` доступний лише всередині свого class/object:

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Top-level `private` declaration доступний у межах поточного Kotlin-файлу:

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

Private constructor дозволяє контролювати створення через factory:

```kotlin
class UserId private constructor(val value: String) {
    companion object {
        fun from(value: String): UserId {
            require(value.isNotBlank())
            return UserId(value)
        }
    }
}
```

### protected

`protected` member доступний у class та його subclasses:

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

На відміну від Java, Kotlin `protected` не дає package access. Top-level declaration не може бути protected. Цей modifier створює inheritance API, тому його варто використовувати обережно.

### internal

`internal` обмежує доступ Kotlin module-ом:

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Module — це compilation boundary: наприклад Gradle module, Maven artifact або окремий compiler invocation. Це не package visibility.

У multi-module Android `internal` приховує реалізацію feature:

```kotlin
fun NavGraphBuilder.profileGraph() { /* public entry point */ }

internal class ProfileViewModel : ViewModel()
internal fun ProfileScreen() { }
```

JVM bytecode не забезпечує `internal` як security boundary: це compile-time обмеження Kotlin API, а Java/reflection можуть бачити згенеровані declarations.

### Visibility setter-а та constructor-а

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

Visibility constructor-а задається окремо:

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Клас public, але створювати його напряму можуть лише callers того самого module.

Visibility override не можна звузити порівняно з base member. Також declaration не може відкривати назовні менш видимий тип у своїй сигнатурі.

### Практичні правила

- починати з найвужчої достатньої visibility;
- implementation details залишати `private` або `internal`;
- public API робити мінімальним і стабільним;
- `protected` використовувати лише для навмисного inheritance contract;
- package не є visibility boundary у Kotlin;
- modifier контролює доступ, але не замінює security механізми.

**Коротко:** `public` відкриває API всім, `internal` — одному module, `protected` — class hierarchy, `private` — локальному scope або файлу. Найвужча достатня visibility зменшує coupling і захищає boundaries коду.

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

2. **Коли singleton доречний**

Singleton може бути нормальним для:

- stateless utility/service;
- app-wide coordinator;
- registry;
- logger;
- feature flags reader;
- shared configuration;
- DI-managed single instance.

Але singleton не має автоматично означати “глобальна змінна для всього”.

3. **Companion object**

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

4. **Singleton через DI scope**

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

Переваги DI:

- простіше тестувати;
- залежності явні;
- можна підмінити fake;
- lifecycle контролюється container-ом.

5. **Поганий singleton**

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

Singleton не має тримати `Activity`, `Fragment`, `View` або `ActivityContext`.

6. **Application Context**

Якщо singleton потребує context, використовувати `applicationContext`:

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

Не можна зберігати `ActivityContext` у singleton, бо це може створити memory leak.

7. **Тестування**

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

У production можна дати реальну реалізацію, у тестах — fake.

8. **Thread safety**

Kotlin `object` ініціалізується thread-safe. Але внутрішній mutable state — не обовʼязково thread-safe.

Погано:

```kotlin
object Counter {
    var value = 0

    fun increment() {
        value++
    }
}
```

Для доступу з різних потоків потрібна синхронізація або atomic types:

```kotlin
object Counter {
    private val atomicValue = AtomicInteger(0)

    fun increment(): Int {
        return atomicValue.incrementAndGet()
    }
}
```

9. **Коли singleton не потрібен**

Singleton не підходить, якщо:

- обʼєкт має screen lifecycle;
- обʼєкт тримає user/session-specific state;
- потрібні різні instances з різними параметрами;
- state має очищатися часто;
- залежність краще scoped до ViewModel/Activity/feature.

Наприклад, `ViewModel` не має бути singleton, бо вона привʼязана до lifecycle owner-а.

**Коротко:** singleton — це один екземпляр обʼєкта в межах процесу. У Kotlin його легко створити через `object`, але в Android production-коді частіше краще використовувати DI-scoped singleton, щоб мати явні залежності, контроль lifecycle і нормальну тестованість.

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

Type erasure — це механізм, через який інформація про generic type parameters здебільшого стирається під час компіляції і недоступна в runtime. Наприклад, `List<String>` і `List<Int>` у runtime виглядають приблизно як просто `List`.

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

3. **Чому type erasure виникає**

Основна причина — JVM backward compatibility з Java.

Java generics були додані пізніше, ніж сама Java, і реалізовані через erasure, щоб старий bytecode і старі libraries продовжили працювати. Kotlin на JVM має бути сумісний із цією model.

4. **Проблема з is check**

Не можна:

```kotlin
fun handle(value: Any) {
    if (value is List<String>) {
        // impossible check
    }
}
```

Можна перевірити тільки container type:

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

6. **Generic function і T**

Погано:

```kotlin
fun <T> isOfType(value: Any): Boolean {
    return value is T
}
```

Так не можна, бо `T` стертий у runtime.

7. **Reified як рішення**

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

```kotlin
val user = decode(json, User::class.java)
```

З `reified`:

```kotlin
inline fun <reified T> decode(json: String): T {
    TODO()
}
```

```kotlin
val user: User = decode<User>(json)
```

9. **Reified не вирішує все**

`reified` допомагає з самим типом `T`, але nested generics можуть бути складними:

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

11. **Практичне правило**

- Не перевіряти `value is List<String>` — використовувати `List<*>`.
- Обережно з `as List<T>` — це unchecked cast.
- Для runtime type checks generic `T` використовувати `inline reified`.
- Для JSON і nested generics використовувати serializers/type tokens.
- Не створювати overload-и, які після erasure мають однакову JVM signature.

**Коротко:** type erasure — це стирання generic type information у runtime на JVM. Він виникає через сумісність із Java/JVM. Kotlin частково компенсує це через `reified`, star projections і compile-time type safety, але generic типи в runtime треба обробляти обережно.

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

Потокобезпечна колекція зберігає коректний стан при одночасному доступі з кількох потоків. Звичайні `MutableList`, `MutableMap` і `MutableSet` у Kotlin/JVM не дають такої гарантії.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Concurrent writes можуть призвести до lost updates, пошкодженого внутрішнього стану або непередбачуваних результатів.

### Read-only не означає immutable

```kotlin
val users: List<User> = mutableListOf()
```

`List` забороняє mutation лише через це посилання. Underlying object може залишатися mutable та змінюватися через інший reference. Також незмінна структура не гарантує immutability її елементів.

### Основні стратегії

**1. Один owner/thread**

Найпростіший підхід — усі read/write виконуються на одному dispatcher або actor-like owner. Твердження «все на Main» коректне лише якщо жоден background callback чи repository не звертається до колекції напряму.

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

Гарантія діє лише якщо кожен доступ проходить через той самий `Mutex`. Lock треба тримати коротко й не виконувати всередині network або довгі suspend-операції.

**3. JVM concurrent collections**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` підтримує concurrent read/write. Але послідовність із кількох операцій не стає атомарною автоматично:

```kotlin
users.computeIfAbsent(id) { loadUser(it) }
```

Для listeners із частими iterations і рідкісними змінами підходить `CopyOnWriteArrayList`; кожен write копіює backing array, тому для write-heavy workload вона дорога.

**4. Immutable snapshots**

Для UI state зручніше створювати нову колекцію замість mutation shared instance:

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Це спрощує state observation у Flow/Compose, але атомарність залежить від механізму update. Immutable snapshot не захищає сам контейнер state від неатомарного read-modify-write.

### Iteration

Зміна звичайної колекції під час iteration може кинути `ConcurrentModificationException`, навіть в одному потоці:

```kotlin
items.removeAll { it == "B" }
```

Використовують iterator із `remove()`, спеціальну collection operation або працюють із snapshot-копією. Відсутність exception не означає відсутність race condition.

### Практичний вибір

- coroutine-only shared state — `Mutex` або actor/serialized owner;
- багато concurrent reads/writes за ключем — `ConcurrentHashMap`;
- listeners: багато reads, мало writes — `CopyOnWriteArrayList`;
- UI state — immutable snapshots і atomic `update`;
- складні invariants між кількома значеннями — одна спільна критична секція.

Mutable collection не слід віддавати назовні: API має повертати snapshot або read-only view з чітко визначеним ownership.

**Коротко:** звичайні mutable collections не є thread-safe. Коректна стратегія залежить від access pattern: один owner, `Mutex`/`synchronized`, concurrent collection або immutable snapshots. Важливо синхронізувати не лише окремі операції, а весь compound invariant.

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

`Checked exceptions` — це exceptions, які Java змушує явно обробити через `try-catch` або оголосити через `throws`. `Unchecked exceptions` — це exceptions, які компілятор не змушує обробляти. У Kotlin усі exceptions фактично unchecked.

1. **Checked exceptions у Java**

У Java checked exception треба обробити або оголосити:

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

Unchecked exceptions — це зазвичай нащадки `RuntimeException`:

```java
String value = null;
value.length(); // NullPointerException
```

Компілятор не змушує обробляти `NullPointerException`, `IllegalArgumentException`, `IllegalStateException`, `IndexOutOfBoundsException`.

3. **Kotlin не має checked exceptions**

У Kotlin можна викликати Java-метод, який кидає checked exception, без `try-catch`:

```kotlin
fun readFile(path: String): String {
    return File(path).readText()
}
```

Якщо виникне `IOException`, вона впаде в runtime, але компілятор Kotlin не змусить її обробити.

4. **Чому Kotlin так зробив**

Checked exceptions часто створюють boilerplate і погано комбінуються з lambdas, coroutines, functional style та API composition.

Kotlin обрав підхід:

```text
exceptions існують, але compiler не примушує їх обробляти
```

Відповідальність переноситься на дизайн API й домовленості в команді.

5. **Recoverable vs programming errors**

Практично важливіше не checked/unchecked, а чи можна відновитися.

Recoverable errors:

- немає інтернету;
- server повернув 500;
- token expired;
- файл не знайдено;
- permission denied.

Programming errors:

- invalid argument;
- неправильний state;
- unexpected null;
- index out of bounds;
- неправильне використання API.

Recoverable errors краще моделювати явно, а programming errors можна кидати як exceptions.

6. **Result/sealed class замість exceptions**

Для очікуваних domain/API помилок часто краще sealed result:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkError : LoginResult
    data class UnknownError(val cause: Throwable) : LoginResult
}
```

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

Так caller бачить очікувані стани через type system.

7. **Exceptions для contract violations**

Для неправильного використання API exceptions нормальні:

```kotlin
fun getUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

`require` кидає `IllegalArgumentException`. Це сигнал, що caller порушив контракт.

8. **Java interop і @Throws**

Якщо Kotlin-функцію викликає Java-код і треба показати checked exception, використовують `@Throws`:

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String {
    return File(path).readText()
}
```

Для Java це виглядатиме як метод із `throws IOException`. У чистому Kotlin-коді `@Throws` зазвичай не потрібен.

9. **Coroutines**

У coroutines exceptions теж unchecked для компілятора:

```kotlin
viewModelScope.launch {
    val user = repository.getUser(userId)
}
```

Якщо exception не обробити, coroutine завершиться з помилкою. Також важливо не ковтати `CancellationException`, бо це механізм cancellation.

**Коротко:** checked exceptions — це Java-механізм примусової обробки помилок компілятором. У Kotlin усі exceptions unchecked, тому очікувані recoverable помилки краще моделювати типами (`Result`, sealed classes), а exceptions залишати для неочікуваних або contract-violating ситуацій.

</details>
<details>
<summary>145. Які існують типи помилок у Kotlin/Java?</summary>

#### Kotlin

У Kotlin/Java всі помилки й exceptions у JVM-ієрархії походять від `Throwable`. На верхньому рівні є дві головні гілки: `Exception` і `Error`.

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

У Kotlin ієрархія JVM така сама, але checked exceptions не enforced компілятором.

2. **Throwable**

`Throwable` — базовий тип для всього, що можна `throw` і `catch`:

```kotlin
try {
    riskyOperation()
} catch (throwable: Throwable) {
    // catches almost everything
}
```

У production-коді ловити `Throwable` небезпечно, бо можна випадково перехопити `Error` або `CancellationException`.

3. **Exception**

`Exception` — це помилки, які application code потенційно може обробити:

```kotlin
try {
    repository.loadUser()
} catch (error: Exception) {
    showError(error.message)
}
```

Приклади:

- `IOException`;
- `SQLException`;
- `ParseException`;
- `IllegalArgumentException`;
- `IllegalStateException`.

4. **RuntimeException**

`RuntimeException` — unchecked exceptions, які часто означають programming error або порушення контракту API:

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Типові приклади: `NullPointerException`, `IndexOutOfBoundsException`, `ClassCastException`, `NumberFormatException`.

5. **Checked exceptions**

У Java checked exceptions треба явно обробляти або оголошувати:

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

У Kotlin compiler не змушує їх ловити:

```kotlin
fun readFile(path: String): String {
    return File(path).readText()
}
```

6. **Error**

`Error` — серйозні JVM/runtime проблеми:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Зазвичай application code не має ловити `Error`, бо після таких станів процес може бути нестабільним.

7. **Domain errors**

Очікувані business-помилки часто краще моделювати sealed types, а не exceptions:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data object NotFound : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Наприклад, invalid email — це нормальний user input scenario, а не обовʼязково exception.

8. **Network/API errors**

У Android технічні exceptions часто маплять у domain/UI errors:

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

Так UI працює з зрозумілими станами, а не з деталями transport layer.

9. **CancellationException**

У coroutines `CancellationException` означає cancellation, а не звичайну помилку:

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Cancellation треба пропускати далі, а не ковтати в generic `catch`.

**Коротко:** у Kotlin/Java всі помилки походять від `Throwable`; головні гілки — `Exception` і `Error`. У хорошому Kotlin/Android коді recoverable exceptions мапляться в domain/UI errors, `Error` зазвичай не ловлять, а `CancellationException` у coroutines не ковтають.

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

Складність алгоритмів — це оцінка того, як ростуть час виконання або використання памʼяті алгоритму зі збільшенням розміру вхідних даних. Зазвичай її описують через Big O notation: `O(1)`, `O(log n)`, `O(n)`, `O(n log n)`, `O(n²)`.

1. **Навіщо це потрібно**

Складність допомагає зрозуміти, чи буде рішення працювати на великих даних.

```kotlin
fun containsUser(users: List<User>, id: String): Boolean {
    return users.any { it.id == id }
}
```

Це `O(n)`, бо в гіршому випадку треба пройти весь список.

2. **O(1) — константна складність**

Час не залежить від розміру input:

```kotlin
fun firstItem(items: List<String>): String? {
    return items.firstOrNull()
}
```

Також:

```kotlin
val user = usersById[id]
```

Для `HashMap` lookup зазвичай `O(1)` average.

3. **O(n) — лінійна складність**

Час росте пропорційно кількості елементів:

```kotlin
fun findUser(users: List<User>, id: String): User? {
    for (user in users) {
        if (user.id == id) return user
    }
    return null
}
```

Якщо список у 10 разів більший, роботи приблизно у 10 разів більше.

4. **O(n²) — квадратична складність**

Зазвичай виникає при вкладених циклах:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    for (i in items.indices) {
        for (j in i + 1 until items.size) {
            if (items[i] == items[j]) return true
        }
    }
    return false
}
```

Краще через `Set`:

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = mutableSetOf<String>()
    for (item in items) {
        if (!seen.add(item)) return true
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

7. **Time vs Space complexity**

Time complexity — скільки часу/операцій потрібно.

Space complexity — скільки додаткової памʼяті потрібно.

```kotlin
fun copyUsers(users: List<User>): List<User> {
    return users.toList()
}
```

Time: `O(n)`  
Space: `O(n)`, бо створюється копія списку.

8. **List vs Set**

```kotlin
val selectedIds: List<String> = ...
val isSelected = selectedIds.contains(user.id)
```

`List.contains` — `O(n)`.

Якщо перевірка виконується для кожного item у великому списку, це може стати `O(n * m)`.

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

Але в реальному Android performance constants теж важливі, особливо на main thread.

10. **Практичне правило**

- Один прохід по списку — зазвичай `O(n)`.
- Вкладений цикл по тому самому списку — часто `O(n²)`.
- HashMap/HashSet lookup — зазвичай `O(1)` average.
- Binary search — `O(log n)`, але список має бути sorted.
- Sorting — часто `O(n log n)`.
- Для UI/main thread важливі не тільки Big O, а й реальна вартість операцій.

**Коротко:** складність алгоритмів показує, як швидко росте вартість виконання або памʼяті при збільшенні input. В Android це впливає на плавність UI, scroll performance, роботу з великими списками, кешами й пошуком.

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

`Authentication` і `authorization` — це різні частини security flow. Authentication відповідає на питання “хто ти?”, authorization — “що тобі дозволено робити?”. Спочатку система ідентифікує користувача, потім перевіряє його права.

1. **Authentication**

Authentication — це підтвердження особи користувача.

Приклади:

- login/password;
- biometric login;
- SMS/OTP code;
- OAuth login через Google/Apple;
- перевірка access token;
- refresh session.

Приклад login API:

```kotlin
interface AuthApi {
    @POST("auth/login")
    suspend fun login(
        @Body request: LoginRequest
    ): AuthResponse
}
```

Після успішної authentication backend зазвичай видає tokens:

```kotlin
data class AuthTokens(
    val accessToken: String,
    val refreshToken: String
)
```

2. **Authorization**

Authorization — це перевірка прав доступу після authentication.

Наприклад:

```http
DELETE /payments/123
Authorization: Bearer <token>
```

Backend може відповісти:

```http
403 Forbidden
```

Користувача впізнали, але дія для нього заборонена.

3. **Коротка різниця**

```text
Authentication -> хто користувач?
Authorization  -> що цьому користувачу дозволено?
```

HTTP-коди:

```text
401 Unauthorized -> користувач не authenticated або token невалідний
403 Forbidden    -> користувач authenticated, але не має permission
```

Назва `401 Unauthorized` історично плутає, але на практиці це authentication problem.

4. **Приклад в Android**

Authentication:

```kotlin
class LoginUseCase(
    private val authRepository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): LoginResult {
        return authRepository.login(email, password)
    }
}
```

Authorization:

```kotlin
class DeleteUserUseCase(
    private val permissionChecker: PermissionChecker,
    private val userRepository: UserRepository
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

5. **Access token**

Access token зазвичай додають до request через interceptor:

```kotlin
class AuthInterceptor(
    private val tokenStorage: TokenStorage
) : Interceptor {
    override fun intercept(chain: Interceptor.Chain): Response {
        val token = tokenStorage.getAccessToken()
        val request = chain.request().newBuilder()
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

Backend перевіряє token, визначає identity/session і потім застосовує authorization rules.

6. **Roles, permissions і backend enforcement**

Authorization часто базується на ролях або permissions:

```kotlin
enum class Role { USER, MODERATOR, ADMIN }
enum class Permission { READ_PROFILE, EDIT_PROFILE, DELETE_USER }
```

Android app може приховати кнопку:

```kotlin
if (state.canDeleteUser) {
    Button(onClick = onDeleteClick) {
        Text("Delete")
    }
}
```

Але це тільки UX. Справжню перевірку прав має робити backend, бо mobile app можна модифікувати або викликати API напряму.

7. **Обробка 401 і 403**

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

Для `401` app зазвичай робить refresh token або веде на login. Для `403` показує “немає доступу”.

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

Корутини запускають через coroutine builders: `launch`, `async`, `runBlocking`, а також через lifecycle-aware scopes в Android: `viewModelScope`, `lifecycleScope`, `rememberCoroutineScope`. Вибір залежить від того, чи потрібен результат, який lifecycle у задачі і чи можна блокувати thread.

1. **launch**

`launch` запускає coroutine без повернення результату:

```kotlin
viewModelScope.launch {
    repository.syncData()
}
```

Повертає `Job`, який можна cancel-ити:

```kotlin
val job = scope.launch { work() }
job.cancel()
```

Підходить для fire-and-forget задач у межах scope.

2. **async**

`async` запускає coroutine і повертає `Deferred<T>`:

```kotlin
val userDeferred = async { api.getUser() }
val postsDeferred = async { api.getPosts() }

val user = userDeferred.await()
val posts = postsDeferred.await()
```

Використовується, коли потрібен результат, особливо для паралельних незалежних операцій.

3. **coroutineScope**

```kotlin
suspend fun loadScreen() = coroutineScope {
    val user = async { api.getUser() }
    val posts = async { api.getPosts() }
    ScreenData(user.await(), posts.await())
}
```

`coroutineScope` створює structured concurrency boundary: якщо одна child coroutine падає, інші скасовуються.

4. **supervisorScope**

```kotlin
suspend fun loadPartial() = supervisorScope {
    val user = async { runCatching { api.getUser() } }
    val posts = async { runCatching { api.getPosts() } }

    PartialData(user.await().getOrNull(), posts.await().getOrNull())
}
```

`supervisorScope` дозволяє одній child coroutine впасти без автоматичного скасування інших.

5. **runBlocking**

```kotlin
fun main() = runBlocking {
    repository.load()
}
```

`runBlocking` блокує thread. Його можна використовувати в `main`, tests або bridging legacy sync code. В Android UI коді його майже ніколи не можна використовувати, бо він може спричинити ANR.

6. **viewModelScope**

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Scope автоматично скасовується в `ViewModel.onCleared()`.

7. **lifecycleScope**

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { render(it) }
    }
}
```

Підходить для Activity/Fragment lifecycle-aware роботи.

8. **rememberCoroutineScope**

У Compose:

```kotlin
val scope = rememberCoroutineScope()

Button(onClick = {
    scope.launch { snackbarHostState.showSnackbar("Saved") }
}) {
    Text("Save")
}
```

Підходить для UI suspend actions з callbacks: snackbar, drawer, scroll animation.

9. **GlobalScope**

```kotlin
GlobalScope.launch { }
```

Майже завжди погана ідея в app-коді, бо coroutine не привʼязана до lifecycle і може жити довше потрібного.

10. **Практичне правило**

- Немає результату — `launch`.
- Потрібен результат — `async` + `await`.
- Паралельні child tasks — `coroutineScope`.
- Часткові результати — `supervisorScope`.
- ViewModel — `viewModelScope`.
- Activity/Fragment — `lifecycleScope`.
- Compose callback — `rememberCoroutineScope`.
- Не використовувати `runBlocking` на main thread.

**Коротко:** корутини запускають через `launch`, `async`, `coroutineScope`, `supervisorScope`, `runBlocking` і Android scopes (`viewModelScope`, `lifecycleScope`, `rememberCoroutineScope`). Вибір залежить від результату, lifecycle, error handling і того, чи можна блокувати thread.

</details>
<details>
<summary>164. У чому різниця між launch та async?</summary>

#### Kotlin

`launch` і `async` — це coroutine builders. `launch` запускає coroutine без результату і повертає `Job`. `async` запускає coroutine з результатом і повертає `Deferred<T>`, з якого результат отримують через `await()`.

1. **launch**

```kotlin
val job: Job = viewModelScope.launch {
    repository.sync()
}
```

`launch` повертає `Job`, який дозволяє скасувати coroutine, перевірити стан або дочекатися завершення через `join()`:

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

`async` повертає `Deferred<T>`, тобто `Job` + майбутній результат.

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

`launch` підходить для side effects:

- оновлення UI state;
- запису в database;
- sync без прямого результату;
- analytics;
- navigation effects;
- collect Flow.

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

`async` підходить, коли треба отримати результат або запустити кілька незалежних задач паралельно:

```kotlin
suspend fun loadDashboard(): Dashboard = coroutineScope {
    val user = async { userRepository.getUser() }
    val stats = async { statsRepository.getStats() }
    val feed = async { feedRepository.getFeed() }

    Dashboard(
        user = user.await(),
        stats = stats.await(),
        feed = feed.await()
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

`async` без `await()` майже завжди означає неправильний builder.

7. **Exception handling**

У `launch` exception поводиться як uncaught exception у coroutine scope, тому в UI-коді її зазвичай ловлять всередині coroutine:

```kotlin
viewModelScope.launch {
    try {
        repository.sync()
    } catch (error: IOException) {
        _state.value = State.Error
    }
}
```

У `async` exception проявляється при `await()`:

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

Але в structured concurrency failure child coroutine все одно може скасувати parent scope.

8. **Structured concurrency**

Правильно запускати `async` всередині `coroutineScope` або lifecycle scope:

```kotlin
suspend fun loadData(): Data = coroutineScope {
    val a = async { loadA() }
    val b = async { loadB() }

    Data(a.await(), b.await())
}
```

Так parent чекає children, а cancellation працює передбачувано.

Якщо задачі незалежні і одна може впасти без скасування інших, використовують `supervisorScope`.

9. **Практичне правило**

- Немає результату — `launch`.
- Є результат — `async`.
- Паралельно отримати кілька результатів — `async + await`.
- `async` без `await` майже завжди помилка.
- Exceptions у `launch` обробляти всередині coroutine або через handler.
- Exceptions у `async` обробляти навколо `await`.
- Для паралельних suspend-функцій використовувати `coroutineScope`.

**Коротко:** `launch` запускає coroutine для роботи без результату і повертає `Job`; `async` запускає coroutine з результатом і повертає `Deferred<T>`. В Android `launch` частіше використовують для state updates, а `async` — для паралельних незалежних запитів.

</details>
<details>
<summary>165. Як обробляти помилки в корутинах?</summary>

#### Kotlin

Помилки в корутинах обробляють через `try/catch`, `CoroutineExceptionHandler`, `supervisorScope`, `SupervisorJob`, `runCatching` і правильне розділення responsibility між repository/use case/ViewModel. Важливо не ковтати `CancellationException`.

1. **try/catch у suspend code**

Найпростіший спосіб:

```kotlin
suspend fun loadUser(): Result<User> {
    return try {
        Result.success(api.getUser())
    } catch (e: IOException) {
        Result.failure(e)
    }
}
```

`try/catch` добре працює всередині suspend-функцій і `launch` blocks.

2. **У ViewModel**

```kotlin
viewModelScope.launch {
    _state.value = State.Loading

    try {
        val user = repository.loadUser()
        _state.value = State.Success(user)
    } catch (e: IOException) {
        _state.value = State.Error("Network error")
    }
}
```

UI має отримувати зрозумілий state, а не сирий exception.

3. **CancellationException**

`CancellationException` не треба обробляти як помилку:

```kotlin
catch (e: CancellationException) {
    throw e
} catch (e: Exception) {
    handleError(e)
}
```

Якщо її проковтнути, cancellation може працювати некоректно.

4. **CoroutineExceptionHandler**

```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    logger.log(throwable)
}

scope.launch(handler) {
    error("Boom")
}
```

Handler ловить uncaught exceptions у root coroutine. Він не замінює локальний `try/catch`, якщо треба показати error state.

5. **async exceptions**

У `async` exception зберігається в `Deferred` і кидається при `await()`:

```kotlin
val deferred = async { api.getUser() }
try {
    val user = deferred.await()
} catch (e: Exception) {
    handleError(e)
}
```

6. **coroutineScope vs supervisorScope**

`coroutineScope`: помилка однієї child coroutine скасовує весь scope.

```kotlin
coroutineScope {
    async { requestA() }
    async { requestB() }
}
```

`supervisorScope`: помилка однієї child не скасовує інші автоматично.

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

`catch` ловить exceptions upstream, але не downstream після `catch`.

8. **Result / sealed errors**

Для domain layer часто краще мапити exceptions у зрозумілі помилки:

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

UI не має знати деталі Retrofit/OkHttp/SQL exceptions.

9. **Типові помилки**

- ловити `Exception` і ковтати cancellation;
- покладатися тільки на `CoroutineExceptionHandler`;
- не обробляти `await()`;
- прокидувати raw exceptions у UI;
- змішувати retry, mapping і UI state в одному місці;
- не логувати unexpected errors.

10. **Практичне правило**

Очікувані помилки обробляти локально і перетворювати в state/result. Неочікувані — логувати. Cancellation — не ковтати. Для незалежних child tasks — `supervisorScope`, для all-or-nothing — `coroutineScope`.

**Коротко:** помилки в корутинах обробляють через `try/catch`, `await()` handling, `Flow.catch`, `supervisorScope`/`SupervisorJob` і мапінг exceptions у domain/UI errors. `CoroutineExceptionHandler` ловить uncaught root errors, але не замінює нормальну локальну обробку. `CancellationException` треба перевикидати.

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

`Application` — це singleton Android-процесу. Він створюється раніше за `Activity`, `Service` або `BroadcastReceiver` у цьому процесі й живе, поки живе процес. Основні callbacks: `onCreate()`, `onTerminate()`, `onLowMemory()`, `onTrimMemory()`, `onConfigurationChanged()`. Також через `Application` можна реєструвати `ActivityLifecycleCallbacks`.

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

Типові задачі: DI setup, crash reporting, logging, analytics, app-wide SDK setup. `onCreate()` має бути швидким, бо він впливає на cold start.

У manifest:

```xml
<application
    android:name=".App"
    android:theme="@style/AppTheme" />
```

Якщо `android:name` не вказати, Android створить стандартний `Application`.

2. **onTerminate**

```kotlin
override fun onTerminate() {
    super.onTerminate()
}
```

На реальних Android-девайсах на нього не можна покладатися. Система може просто вбити процес без виклику `onTerminate()`. Тому critical cleanup або збереження даних тут робити не можна.

3. **onLowMemory і onTrimMemory**

`onLowMemory()` викликається при критично низькій памʼяті:

```kotlin
override fun onLowMemory() {
    super.onLowMemory()
    imageCache.clear()
}
```

`onTrimMemory()` дає точніший сигнал:

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

`TRIM_MEMORY_UI_HIDDEN` означає, що UI додатка вже не видимий, тому можна звільняти UI-related ресурси.

4. **onConfigurationChanged**

```kotlin
override fun onConfigurationChanged(newConfig: Configuration) {
    super.onConfigurationChanged(newConfig)
}
```

`Application` може отримувати configuration changes, але UI-specific handling краще тримати на рівні `Activity`, `Fragment` або Compose.

5. **ActivityLifecycleCallbacks**

`Application` може слухати lifecycle усіх Activity:

```kotlin
registerActivityLifecycleCallbacks(
    object : ActivityLifecycleCallbacks {
        override fun onActivityCreated(activity: Activity, state: Bundle?) = Unit
        override fun onActivityStarted(activity: Activity) = Unit
        override fun onActivityResumed(activity: Activity) = Unit
        override fun onActivityPaused(activity: Activity) = Unit
        override fun onActivityStopped(activity: Activity) = Unit
        override fun onActivitySaveInstanceState(activity: Activity, outState: Bundle) = Unit
        override fun onActivityDestroyed(activity: Activity) = Unit
    }
)
```

Типові задачі: analytics screen tracking, foreground/background detection, session tracking, debug tooling.

Для foreground/background detection у production частіше простіше використовувати `ProcessLifecycleOwner`, а не вручну рахувати Activity.

6. **Чого не робити в Application**

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

7. **Hilt**

Для Hilt application class позначають так:

```kotlin
@HiltAndroidApp
class App : Application()
```

Це запускає генерацію application-level dependency graph.

**Коротко:** головний callback `Application` — `onCreate()`. Для memory pressure є `onTrimMemory()` і `onLowMemory()`, `onTerminate()` не гарантується, а lifecycle усіх Activity можна слухати через `registerActivityLifecycleCallbacks()`.

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

ANR виникає, коли application process не обробляє важливі системні події в очікуваний час. Найчастіше причина — заблокований або перевантажений main thread.

```kotlin
button.setOnClickListener {
    Thread.sleep(10_000) // blocks input and rendering
}
```

Main thread обробляє input, lifecycle callbacks, Binder callbacks і rendering. Навіть якщо кожна операція окремо коротка, велика черга роботи також може зробити UI не responsive.

### Типові причини

**Blocking або важка робота на Main:**

- synchronous network або disk I/O;
- великі database queries;
- JSON parsing і bitmap decoding;
- складні цикли, sorting або cryptography;
- `runBlocking`, `Thread.sleep()`, `Future.get()` чи `join()`;
- довга робота в View/Compose callbacks.

I/O переносять на `Dispatchers.IO`, CPU-bound роботу — на `Dispatchers.Default`. Suspend-функція не гарантує background execution сама по собі: важливо, щоб blocking implementation явно змінила dispatcher або була викликана у правильному context.

```kotlin
viewModelScope.launch {
    val result = withContext(Dispatchers.IO) {
        blockingRepository.loadData()
    }
    _state.value = UiState.Success(result)
}
```

**Компоненти Android:**

`BroadcastReceiver.onReceive()`, `Service.onCreate()` і `onStartCommand()` за замовчуванням виконуються на main thread. Service не створює background thread автоматично. Receiver має швидко завершитись; тривалу гарантовану роботу зазвичай передають у WorkManager. `goAsync()` лише продовжує lifetime receiver-а на обмежений час і вимагає `PendingResult.finish()`.

**Locks і IPC:**

Main thread може чекати monitor, mutex, latch або synchronous Binder call. Особливо небезпечний deadlock: background thread чекає Main, а Main чекає цей background thread.

**Повільний startup:**

Важка ініціалізація в `Application.onCreate()`, ContentProvider, DI graph або першому Activity може заблокувати process ще до появи інтерактивного UI.

### Jank та ANR

Повільний frame спричиняє jank, але не обов'язково ANR. Серія дуже дорогих layout/draw/recomposition операцій або нескінченний layout loop може заблокувати Main настільки, що система зафіксує ANR.

### Діагностика

Насамперед аналізують stack main thread у момент ANR:

- Play Console Android vitals та ANR clusters;
- ANR traces і tombstones;
- Perfetto/System Trace;
- Android Studio CPU Profiler;
- Logcat і власні timing metrics;
- StrictMode у debug builds.

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

StrictMode знаходить частину порушень, але не замінює traces і profiling. В ANR report важливо відрізняти реальну причину від місця, де Main лише чекає lock, який утримує інший thread.

### Профілактика

- не блокувати Main синхронним очікуванням;
- робити важкі операції cancellable та розбивати CPU work на частини;
- мінімізувати critical sections і не тримати lock під час I/O;
- додавати timeouts до network/IPC;
- відкладати некритичну startup-ініціалізацію;
- профілювати startup, scrolling і масові updates;
- обмежувати частоту подій через debounce/conflation, якщо producer швидший за UI.

**Коротко:** ANR — це наслідок того, що process не відповідає системі вчасно. Основні причини: blocking I/O, CPU-heavy робота, synchronous waits/IPC, lock contention, довгі component callbacks і важкий startup. Діагноз починають зі stack trace main thread та системного trace.

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

Базові компоненти Android — це building blocks, з яких складається застосунок: `Activity`, `Service`, `BroadcastReceiver`, `ContentProvider`. Також у сучасній Android-архітектурі часто говорять про Fragment, ViewModel, WorkManager, але класичні system components саме ці чотири.

1. **Activity**

`Activity` представляє екран або entry point UI:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Вона має lifecycle, window, отримує intents і може host-ити Fragment або Compose UI.

2. **Service**

`Service` виконує роботу без власного UI:

```kotlin
class SyncService : Service() {
    override fun onBind(intent: Intent?): IBinder? = null
}
```

Service не означає автоматично background thread. Довгу роботу треба переносити в coroutine/thread. Для user-visible long-running задач використовують foreground service.

3. **BroadcastReceiver**

`BroadcastReceiver` отримує system або app broadcasts:

```kotlin
class BootReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        // handle event
    }
}
```

`onReceive()` має бути коротким. Для довгої роботи краще делегувати в WorkManager або Service.

4. **ContentProvider**

`ContentProvider` дає іншим apps або компонентам доступ до структурованих даних через URI:

```text
content://com.example.provider/users/1
```

Сьогодні у звичайних apps його пишуть рідше, але він важливий для sharing data, contacts, media, files, search suggestions.

5. **Intent**

Компоненти часто взаємодіють через `Intent`:

```kotlin
startActivity(Intent(this, DetailsActivity::class.java))
```

Intent може запускати Activity/Service або надсилати broadcast.

6. **Manifest**

System components оголошуються в `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity" />
<service android:name=".SyncService" />
<receiver android:name=".BootReceiver" />
<provider android:name=".AppProvider" />
```

Manifest також визначає permissions, intent filters і exported behavior.

7. **Fragment**

Fragment не є базовим system component, але часто використовується як частина UI всередині Activity:

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

Він має власний lifecycle і керується FragmentManager.

8. **ViewModel**

ViewModel теж не system component, але це стандартний Jetpack-компонент для state management:

```kotlin
class ProfileViewModel : ViewModel()
```

Він переживає configuration changes і тримає UI state.

9. **WorkManager**

WorkManager не є класичним component, але використовується для deferrable background work:

```kotlin
class SyncWorker(...) : CoroutineWorker(...)
```

Для sync/upload/retry він часто кращий за Service.

10. **Практичне правило**

- UI entry — Activity.
- Частина UI у View System — Fragment.
- Long-running user-visible робота — foreground Service.
- Deferrable background work — WorkManager.
- System/app events — BroadcastReceiver.
- Data sharing між apps — ContentProvider.

**Коротко:** класичні базові Android components — `Activity`, `Service`, `BroadcastReceiver`, `ContentProvider`. Вони оголошуються в manifest і взаємодіють через intents/URI. У modern Android також часто використовують Fragment, ViewModel і WorkManager, але це вже Jetpack/architecture components, не всі з них є system components.

</details>
<details>
<summary>176. Що таке Service?</summary>

#### Kotlin

`Service` — це Android component для роботи без власного UI. Він може продовжувати виконання, коли `Activity` вже не на екрані, але важливий нюанс: `Service` за замовчуванням працює в процесі застосунку і на main thread. Він не створює окремий потік автоматично.

1. **Для чого потрібен Service**

Service використовують для задач, які мають бути привʼязані до Android component lifecycle, але не до конкретного екрана:

- media playback;
- foreground location tracking;
- активне завантаження або upload;
- синхронізація, якщо вона справді має йти як service;
- IPC або локальна взаємодія через binding.

Головна помилка — вважати Service універсальним рішенням для будь-якої background work.

2. **Started Service**

Started service запускається через `startService()` або `ContextCompat.startForegroundService()` і зазвичай сам себе зупиняє:

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

Довгу роботу треба переносити з main thread у coroutine/thread/executor і скасовувати в `onDestroy()`.

3. **Bound Service**

Bound service дозволяє клієнту підʼєднатись і викликати API сервісу:

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

Такий сервіс живе, поки є bound clients, якщо він не був додатково запущений як started service.

4. **Foreground Service**

Foreground service потрібен для довгої user-visible роботи й обовʼязково показує notification:

```kotlin
override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
    startForeground(NOTIFICATION_ID, createNotification())
    return START_STICKY
}
```

Типові приклади: музика, навігація, location tracking, активний upload/download. Якщо користувач не має бачити цю роботу як активну — часто краще не використовувати foreground service.

5. **Lifecycle callbacks**

Основні callbacks:

```kotlin
override fun onCreate()
override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int
override fun onBind(intent: Intent?): IBinder?
override fun onDestroy()
```

`onCreate()` викликається один раз на створення сервісу. `onStartCommand()` — на кожен start request. `onDestroy()` — місце для cleanup: cancel coroutine scope, unregister receivers, release resources.

6. **START flags**

```text
START_STICKY           -> система може пересоздати Service без старого Intent
START_NOT_STICKY       -> не пересоздавати автоматично
START_REDELIVER_INTENT -> пересоздати і повторно доставити останній Intent
```

Для одноразової sync-задачі частіше підходить `START_NOT_STICKY`. Для player або довгого foreground-сценарію — частіше `START_STICKY`.

7. **Service vs WorkManager**

Service не гарантує переживання process death, reboot або constraints. Якщо задача deferrable і має виконатися надійно, частіше потрібен `WorkManager`:

```text
user-visible long-running work -> Foreground Service
reliable deferred background work -> WorkManager
exact time trigger -> AlarmManager
media playback -> Media3 + foreground service
```

**Коротко:** `Service` — Android component для роботи без UI, але не окремий thread. Є started, bound і foreground services. Для довгої роботи потрібні async-виконання, cleanup і правильний вибір між `Service`, `ForegroundService` та `WorkManager`.

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

`onStart()` — це lifecycle callback `Activity`, який викликається, коли Activity стає видимою користувачу, але ще не обовʼязково готова до взаємодії. Він іде після `onCreate()` або після повернення з `onStop()`, і перед `onResume()`.

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

`onStart()` може викликатися багато разів за життя однієї Activity.

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

Activity може бути visible, але ще не resumed, наприклад коли поверх є partially transparent Activity або system transition.

4. **Що доречно робити в onStart**

У `onStart()` доречно:

- підписуватися на visible-only listeners;
- стартувати lightweight UI updates;
- реєструвати receivers, потрібні тільки коли екран видимий;
- запускати lifecycle-aware collection;
- повідомляти analytics, що screen став visible.

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

Для `Flow` краще використовувати `repeatOnLifecycle`:

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

Цей блок активний між `onStart()` і `onStop()`.

7. **STARTED vs RESUMED**

Для UI state collection часто достатньо `STARTED`, бо екран уже видимий. `RESUMED` потрібен, коли робота має йти тільки при активній взаємодії користувача.

8. **Що не треба робити в onStart**

Не треба виконувати важку синхронну роботу:

```kotlin
override fun onStart() {
    super.onStart()
    Thread.sleep(5_000)
}
```

`onStart()` виконується на main thread. Довга робота може заблокувати UI і привести до ANR.

Краще делегувати роботу у ViewModel:

```kotlin
override fun onStart() {
    super.onStart()
    viewModel.loadIfNeeded()
}
```

9. **Resources і повторні виклики**

Якщо ресурс потрібен, поки Activity видима, стартувати його можна в `onStart()`, а зупиняти в `onStop()`:

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

Код в `onStart()` має бути idempotent: повторний виклик не має дублювати підписки або ламати стан.

10. **Fragment нюанс**

Для UI Flow collection у Fragment краще використовувати `viewLifecycleOwner`:

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    viewLifecycleOwner.repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Якщо робота привʼязана до View, не треба використовувати Fragment `lifecycleScope`.

**Коротко:** `onStart()` потрібен для запуску логіки, актуальної поки Activity видима. Парний cleanup робиться в `onStop()`. Це хороше місце для visible subscriptions і lightweight setup, але не для довгої синхронної роботи.

</details>
<details>
<summary>186. Як можна потрапити в onStart(), але не потрапити в onResume()?</summary>

#### Kotlin

У `onStart()` можна потрапити, але не дійти до `onResume()`, якщо Activity стала видимою або майже видимою, але не отримала foreground focus для взаємодії. Це може статися через overlay, dialog-like Activity, redirect, `finish()` або переривання lifecycle між `STARTED` і `RESUMED`.

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

Якщо інша Activity забирає focus, нижня Activity може бути visible/started, але не resumed.

Приклад:

```xml
<activity
    android:name=".OverlayActivity"
    android:theme="@style/Theme.App.Transparent" />
```

Dialog-themed Activity працює схоже:

```xml
<activity
    android:name=".ConfirmActivity"
    android:theme="@style/Theme.App.Dialog" />
```

Попередня Activity може бути видимою позаду, але focus буде у dialog Activity.

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

Поточна Activity може не дійти до `onResume()`, бо focus одразу переходить на іншу Activity.

4. **finish до onResume**

```kotlin
override fun onStart() {
    super.onStart()

    if (!isUserAllowed()) {
        finish()
    }
}
```

Якщо Activity завершується в `onStart()`, `onResume()` може не викликатися.

5. **Background або system interruption**

Lifecycle може бути перерваний між `onStart()` і `onResume()`:

```text
onCreate()
onStart()
user/system opens another app
onStop()
```

Причини: system overlay, permission dialog, incoming call, multi-window/PiP або швидкий перехід в інший app.

6. **Visible vs interactive resources**

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

Якщо ресурс потрібен тільки коли Activity активна для взаємодії:

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

7. **Flow collection**

Оновлювати UI, поки екран видимий:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Працювати тільки в active foreground:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.RESUMED) {
        viewModel.activeEvents.collect(::handleEvent)
    }
}
```

8. **Практичне правило**

- `onStart()` може викликатися без подальшого `onResume()`.
- `onStart()` означає visible, `onResume()` — interactive foreground.
- Visible-only логіка — `onStart()`/`onStop()`.
- Interaction-only логіка — `onResume()`/`onPause()`.
- Причини: overlay, dialog Activity, redirect, finish, system UI, background interruption, multi-window.
- Lifecycle-код має бути стійкий до переривань.

**Коротко:** в `onStart()` можна потрапити без `onResume()`, коли Activity стала видимою, але не отримала foreground focus або була перервана до resume. Типові сценарії — transparent/dialog Activity поверх, redirect/finish у `onStart()`, системний overlay або швидкий перехід у background.

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

`RecyclerView.Adapter` — це міст між даними і `RecyclerView`. Він каже, скільки item-ів треба показати, створює `ViewHolder`, bind-ить дані в item UI і повідомляє список про зміни. Adapter має бути thin layer, без business logic і navigation logic.

1. **Основна роль**

```text
Data list -> Adapter -> ViewHolder -> RecyclerView item UI
```

Три ключові методи:

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UserViewHolder
override fun onBindViewHolder(holder: UserViewHolder, position: Int)
override fun getItemCount(): Int
```

2. **Приклад Adapter**

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

У production частіше використовують `ListAdapter`, а не ручний `mutableList` + `notifyDataSetChanged()`.

3. **ViewHolder**

`ViewHolder` тримає item view і заповнює її даними:

```kotlin
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

Саме через `ViewHolder` `RecyclerView` перевикористовує item views під час scroll.

4. **Recycling і state**

`RecyclerView` не створює View для кожного елемента. Він перевикористовує видимі item-и і cache. Тому `bind()` має повністю виставляти state.

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

Інакше старий state може потрапити в інший item.

5. **ListAdapter і DiffUtil**

```kotlin
class UserAdapter(
    private val onUserClick: (User) -> Unit
) : ListAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}

object UserDiffCallback : DiffUtil.ItemCallback<User>() {
    override fun areItemsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem.id == newItem.id

    override fun areContentsTheSame(oldItem: User, newItem: User): Boolean =
        oldItem == newItem
}
```

`DiffUtil` дозволяє оновлювати тільки змінені item-и, а не весь список.

6. **Кілька типів item**

Для різних layout-ів використовують `getItemViewType()`:

```kotlin
override fun getItemViewType(position: Int): Int = when (items[position]) {
    is FeedItem.Header -> VIEW_TYPE_HEADER
    is FeedItem.Post -> VIEW_TYPE_POST
}
```

Після цього `onCreateViewHolder()` створює відповідний `ViewHolder`.

7. **Типові помилки**

- Тримати business logic в adapter.
- Викликати navigation напряму з adapter.
- Використовувати `notifyDataSetChanged()` для всіх змін.
- Не скидати повний state у `bind()`.
- Тримати зайве посилання на `Activity` або `Fragment`.
- Не використовувати `DiffUtil` для великих списків.

**Коротко:** `RecyclerView.Adapter` перетворює список даних на item views. Він створює `ViewHolder`, bind-ить дані, підтримує recycling і має залишатися простим UI binding шаром без бізнес-логіки.

</details>
<details>
<summary>194. Які основні методи має RecyclerView.Adapter?</summary>

#### Kotlin

Три обов'язкові методи `RecyclerView.Adapter` визначають створення item UI, binding даних і розмір списку:

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

`onCreateViewHolder()` викликається лише коли потрібен новий holder. `onBindViewHolder()` викликається при показі або оновленні item. Через recycling `bind()` має повністю встановлювати стан View, включно з `false`/порожніми значеннями.

### Різні типи item

`getItemViewType()` повертає тип елемента, за яким `onCreateViewHolder()` створює відповідний holder:

```kotlin
override fun getItemViewType(position: Int): Int =
    when (items[position]) {
        is FeedItem.Header -> TYPE_HEADER
        is FeedItem.Post -> TYPE_POST
        is FeedItem.Loader -> TYPE_LOADER
    }
```

### Часткове оновлення

Payload дозволяє оновити лише змінену частину item:

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

Якщо payload відсутній, завжди потрібен повний bind.

### Recycling lifecycle

- `onViewRecycled()` — cleanup перед повторним використанням holder-а;
- `onViewAttachedToWindow()` — item приєднаний до window;
- `onViewDetachedFromWindow()` — item більше не attached.

У cleanup скасовують item-specific animations або jobs і прибирають callbacks. Image loader зазвичай сам працює з lifecycle View, але custom resources треба звільняти явно.

### Оновлення даних

Ручні методи мають точно відповідати зміні колекції:

```kotlin
notifyItemInserted(position)
notifyItemRemoved(position)
notifyItemChanged(position, payload)
notifyItemMoved(fromPosition, toPosition)
```

`notifyDataSetChanged()` перемальовує весь список і не дає RecyclerView інформації про конкретні зміни. Для звичайних production-списків краще `ListAdapter` або `AsyncListDiffer` з `DiffUtil.ItemCallback`:

```kotlin
class UserAdapter :
    ListAdapter<User, UserViewHolder>(UserDiffCallback) {

    override fun onBindViewHolder(holder: UserViewHolder, position: Int) {
        holder.bind(getItem(position))
    }
}

adapter.submitList(users)
```

Для stable IDs викликають `setHasStableIds(true)` і перевизначають `getItemId()`. ID має стабільно представляти identity item-а, а не його позицію.

### Типові помилки

- не скидати старий state під час bind;
- виконувати важку роботу в `onBindViewHolder()`;
- зберігати переданий `position` у click listener замість актуального `bindingAdapterPosition`;
- використовувати нестабільні IDs;
- змінювати список без відповідного notify або diff;
- передавати mutable list і змінювати його після `submitList()`.

**Коротко:** основні методи Adapter — `onCreateViewHolder()`, `onBindViewHolder()` і `getItemCount()`. `getItemViewType()`, payloads та recycling callbacks покривають складніші сценарії, а оновлення списку краще делегувати `ListAdapter` і `DiffUtil`.

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

`Serializable` — це Java-механізм серіалізації, який дозволяє перетворити обʼєкт у послідовність байтів і потім відновити його назад. В Android його можна використовувати для `Intent`/`Bundle`, але зазвичай краще `Parcelable`, бо він швидший і оптимізований під Android.

1. **Базове визначення**

Клас роблять serializable так:

```kotlin
data class User(
    val id: String,
    val name: String
) : Serializable
```

Тепер обʼєкт можна покласти в `Intent`:

```kotlin
intent.putExtra("user", user)
```

2. **Отримання Serializable**

```kotlin
val user = intent.getSerializableExtra("user") as? User
```

На новіших Android API є typed overload:

```kotlin
val user = intent.getSerializableExtra("user", User::class.java)
```

На практиці часто потрібен compatibility wrapper через різні API levels.

3. **Як це працює концептуально**

```text
Object -> serialization -> bytes
bytes -> deserialization -> Object
```

Серіалізація потрібна, коли обʼєкт треба передати, зберегти або відновити.

4. **Serializable в Bundle**

```kotlin
val bundle = Bundle().apply {
    putSerializable("user", user)
}
```

Читання:

```kotlin
val user = bundle.getSerializable("user") as? User
```

Це працює, але для Android navigation/arguments зазвичай краще `Parcelable` або передача тільки id.

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
  - з @Parcelize простий у використанні
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

Старі serialized objects можуть не відновитися коректно. Тому Serializable погано підходить для довготривалого persistent storage без контрольованої schema.

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

9. **Не для domain persistence**

Не варто використовувати `Serializable` як основний формат збереження domain data. Для storage краще:

- Room;
- DataStore;
- JSON/Proto з явною schema;
- backend;
- files з контрольованим форматом.

10. **Коли Serializable допустимий**

Serializable може бути допустимий:

- для простих тимчасових arguments;
- у legacy-коді;
- для швидкого прототипу;
- коли performance не критична;
- коли обʼєкт маленький і internal.

Для production navigation краще primitive args, ids, `Parcelable`, Safe Args або typed routes.

11. **Практичне правило**

- `Serializable` — Java serialization marker interface.
- В Android може передаватися через `Intent`/`Bundle`.
- Для Android IPC/navigation краще `Parcelable`.
- Не передавати великі objects.
- Не використовувати як довготривалий storage format.
- Між screens краще передавати ids і завантажувати дані з repository.

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

Оптимізація recomposition у Compose — це не спроба “вимкнути” recomposition, а зробити її локальною, дешевою і передбачуваною. Проблема не в самому recomposition, а в широкому state scope, нестабільних моделях або важкій роботі в composable body.

1. **Тримати state на правильному рівні**

State треба hoist-ити тільки настільки високо, наскільки потрібно.

Погано, якщо зміна search text recompose-ить увесь екран:

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

Immutable UI models безпечніше передавати в composable і простіше skip-ати при recomposition.

4. **Не мутувати списки in-place**

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

`key` привʼязує remembered state до item identity, а не до позиції. Це важливо при insert/delete/sort/filter.

Для mixed lists корисний ще й `contentType`:

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it::class }
) { item ->
    FeedItem(item)
}
```

6. **Не робити важку роботу в composable body**

Погано:

```kotlin
@Composable
fun UserList(users: List<User>) {
    val sorted = users.sortedBy { it.name }
    LazyColumn { items(sorted) { UserItem(it) } }
}
```

Краще винести transformation у ViewModel або кешувати:

```kotlin
val sorted = remember(users) {
    users.sortedBy { it.name }
}
```

7. **remember і derivedStateOf**

`remember` підходить для дорогих обʼєктів або обчислень:

```kotlin
val formatter = remember {
    DateTimeFormatter.ofPattern("dd.MM.yyyy")
}
```

`derivedStateOf` корисний, коли source state змінюється часто, а derived result — рідко:

```kotlin
val listState = rememberLazyListState()

val showScrollTop by remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```

8. **Профілювати, а не гадати**

Для реальної оптимізації дивляться:

- Layout Inspector recomposition counters;
- Compose compiler reports;
- Android Studio Profiler;
- System Trace;
- Macrobenchmark;
- baseline profiles.

Високий recomposition count не завжди проблема, якщо composable дешевий.

**Коротко:** recomposition оптимізують через локальний state, маленькі composable, immutable UI models, stable `key` у lazy lists, кешування важких обчислень і профілювання. Recomposition нормальна; проблема — коли вона широка або дорога.

</details>
<details>
<summary>213. Як написати асинхронний тест без використання корутин?</summary>

#### Kotlin

Асинхронний тест без корутин пишуть через контрольоване очікування конкретної події: callback, result, state update або idle state. Головне правило — чекати з timeout, а не використовувати `Thread.sleep()`.

1. **CountDownLatch**

Для callback-based API часто достатньо `CountDownLatch`:

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

Timeout обовʼязковий, інакше тест може зависнути назавжди.

2. **CompletableFuture**

Якщо callback повертає один результат, `CompletableFuture` часто чистіший:

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

3. **Awaitility або polling helper**

Для асинхронної зміни state краще чекати assertion з timeout:

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

Це краще за `Thread.sleep()`, бо тест завершується одразу після виконання умови й не чекає фіксований час.

4. **LiveData**

Для `LiveData` зазвичай використовують `InstantTaskExecutorRule` і helper на кшталт `getOrAwaitValue()`:

```kotlin
@get:Rule
val instantTaskExecutorRule = InstantTaskExecutorRule()

@Test
fun `emits user`() {
    val user = viewModel.user.getOrAwaitValue()
    assertEquals("42", user.id)
}
```

Helper всередині підписується на `LiveData`, чекає значення з timeout і відписує observer.

5. **UI-тести**

Для Espresso краще використовувати `IdlingResource`, а не ручні затримки:

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

6. **Fake dependency**

Найстабільніший варіант — замінити реальний async fake-ом, який тест контролює сам:

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

Так тест стає deterministic і не залежить від реального network/time.

7. **Що не робити**

```kotlin
repository.refresh()
Thread.sleep(2_000)
assertEquals(expected, repository.currentValue)
```

`Thread.sleep()` робить тести повільними, flaky і не гарантує, що async work реально завершився.

**Коротко:** асинхронний тест без корутин має чекати конкретний callback/result/state з timeout. Для цього використовують `CountDownLatch`, `CompletableFuture`, Awaitility, LiveData helper або Espresso `IdlingResource`; `Thread.sleep()` — поганий механізм синхронізації.

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

З `const val` у Kotlin можна використовувати тільки типи, значення яких компілятор знає на етапі компіляції: `String` і primitive types. Це підходить для compile-time constants: ключів, route names, table names, числових лімітів, flags.

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

Такі константи зручно використовувати для Bundle keys, Intent extras, navigation routes, database names, table names, API constants.

3. **Numbers, Boolean, Char**

```kotlin
const val DEFAULT_PAGE_SIZE = 20
const val MAX_RETRY_COUNT = 3
const val CACHE_TTL_SECONDS = 60L
const val LOGGING_ENABLED = true
const val CSV_SEPARATOR = ','
```

Це доречно для простих значень, які не залежать від runtime.

4. **Де можна оголошувати const val**

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

5. **Недозволені типи**

Не можна:

```kotlin
const val USERS = listOf("Alex", "Bob")
const val DATE = LocalDate.now()
const val FORMATTER = DateTimeFormatter.ISO_DATE
const val UUID_VALUE = UUID.randomUUID()
```

Це runtime objects або results of function calls, а не compile-time constants.

6. **Enum не можна**

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

7. **List/Map/Set не можна**

```kotlin
const val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

Так не можна. Треба:

```kotlin
val SUPPORTED_LANGUAGES = listOf("en", "uk")
```

8. **Runtime expressions не можна**

Не можна:

```kotlin
const val CURRENT_TIME = System.currentTimeMillis()
const val RANDOM_ID = UUID.randomUUID().toString()
const val PACKAGE_NAME = context.packageName
```

`const val` має бути обчислений компілятором, а не під час виконання програми.

9. **const val для annotations**

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

Звичайний `val` тут не підходить.

10. **Nullable не можна**

```kotlin
const val NAME: String? = null // compile error
```

Compile-time constant має мати конкретне non-null значення.

11. **Практичне правило**

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

`enum` і `sealed class` використовують для обмеженого набору варіантів, але вони моделюють різні речі. `enum` — це фіксований набір singleton-констант. `sealed class` або `sealed interface` — це закрита ієрархія типів, де кожен варіант може мати власні дані й поведінку.

1. **Enum**

```kotlin
enum class ThemeMode {
    Light,
    Dark,
    System
}
```

Enum добре підходить, коли є простий фіксований набір значень без різного payload:

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

Тут варіанти різні: `Loading` без даних, `Content` має `User`, `Error` має `message`. Enum таку модель нормально не виразить.

3. **Головна різниця**

```text
enum:
  - фіксований набір singleton values
  - всі entries мають однакову структуру
  - зручно для простих constants

sealed:
  - закрита type hierarchy
  - кожен case може мати свій тип
  - кожен case може мати власні поля
```

4. **Exhaustive when**

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

```kotlin
fun render(state: UiState) {
    when (state) {
        UiState.Loading -> showLoading()
        is UiState.Content -> showUser(state.user)
        is UiState.Error -> showError(state.message)
    }
}
```

Компілятор знає всі можливі варіанти.

5. **Enum properties**

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

Але структура всіх entries однакова: кожен має `allowsBody`.

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

7. **Singleton vs instances**

Enum entries завжди singleton:

```kotlin
ThemeMode.Dark === ThemeMode.Dark // true
```

Sealed subclasses можуть бути singleton або data instances:

```kotlin
sealed interface ScreenState {
    data object Loading : ScreenState
    data class Content(val items: List<Item>) : ScreenState
}
```

`Loading` — singleton, `Content` — багато instances з різними даними.

8. **Built-in API**

Enum має готові API:

```kotlin
ThemeMode.entries
ThemeMode.valueOf("Dark")
ThemeMode.Dark.name
ThemeMode.Dark.ordinal
```

Це зручно для dropdown/settings/simple lists. Але `ordinal` не варто зберігати в database/API, бо порядок enum може змінитися.

У sealed hierarchy немає автоматичного `valueOf`, тому serialization/deserialization треба проектувати окремо.

9. **Коли що використовувати**

Enum підходить для:

- modes;
- sort order;
- theme;
- tabs;
- HTTP methods;
- simple status values без payload.

Sealed підходить для:

- UI state;
- result state;
- events;
- navigation commands;
- domain outcomes;
- error hierarchy;
- cases з різними payload.

10. **Sealed class vs sealed interface**

`sealed class` може мати constructor/state:

```kotlin
sealed class ErrorState(
    val code: Int
)
```

`sealed interface` гнучкіший, бо дозволяє реалізовувати кілька interfaces. У сучасному Kotlin для state hierarchies часто використовують `sealed interface` + `data class`/`data object`.

**Коротко:** `enum` — це фіксований список простих singleton-значень. `sealed class/interface` — це закрита ієрархія типів, де кожен варіант може мати власні дані. Для простих режимів — `enum`, для UI/result/domain state з payload — `sealed`.

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

Лаги в Compose під час скролу зазвичай виникають через зайві recompositions, важку роботу в item composables, нестабільні keys, великі allocations або погану роботу із зображеннями. Оптимізація має починатися з профілювання, а не з припущень.

1. **Stable key у LazyColumn**

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

`key` допомагає Compose правильно зберігати state item-а після insert/delete/reorder і не плутати елементи між позиціями.

2. **contentType для mixed lists**

```kotlin
items(
    items = feed,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` корисний, коли в списку є різні типи рядків: header, post, ad, loader. Compose може ефективніше reuse-ити composition для однотипних елементів.

3. **Не робити важку роботу в item**

Погано:

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Краще підготувати formatted text у mapper/ViewModel або хоча б використати `remember`.

4. **remember для expensive objects**

```kotlin
val shape = remember { RoundedCornerShape(12.dp) }
```

Не треба створювати важкі обʼєкти при кожній recomposition. Але `remember` не має приховувати бізнес-логіку в UI.

5. **Immutable UI models**

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Краще передавати стабільні immutable моделі. Mutable collections або обʼєкти, які постійно створюються заново, можуть провокувати зайві recompositions.

6. **Звузити state reads**

Не передавати весь screen state у кожен item:

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Item має отримувати тільки ті дані, які йому реально потрібні.

7. **derivedStateOf для scroll state**

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Корисно, коли source state змінюється часто, але UI має реагувати тільки на derived condition.

8. **Зображення**

Для картинок використовувати image loader із cache і правильним розміром:

```kotlin
AsyncImage(
    model = user.avatarUrl,
    contentDescription = null
)
```

Не можна декодувати bitmap вручну в composable або main thread.

9. **Paging для великих списків**

```kotlin
val items = pager.collectAsLazyPagingItems()
```

Paging 3 допомагає не вантажити весь список одразу і нормально обробляти loading/error/retry.

10. **Профілювання**

Перевіряти треба через:

- Layout Inspector recomposition counts;
- Android Studio Profiler;
- Macrobenchmark;
- Baseline Profiles;
- system traces.

**Коротко:** щоб уникати лагів у Compose списках, потрібні stable `key`, `contentType` для mixed lists, легкі item composables, immutable UI models, мінімум allocations, cached image loading, `derivedStateOf` для scroll-derived state, Paging для великих списків і обовʼязкове профілювання.

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
