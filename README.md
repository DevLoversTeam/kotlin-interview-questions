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

Коротко: Kotlin добре інтегрується з Java, бо компілюється в JVM bytecode і
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

Коротко: базові типи Kotlin — це числові типи, `Boolean`, `Char`, `String` і
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

Коротко: `val` — read-only reference, `var` — змінна з можливістю повторного
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

Коротко: type inference прибирає зайвий синтаксис, але не прибирає статичну
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

Коротко: extension functions — це синтаксично зручний спосіб додати поведінку до
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

Коротко: `data class` — це Kotlin-механізм для value-like моделей даних із
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

Коротко: `companion object` — це singleton-обʼєкт, привʼязаний до класу, який
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

Коротко: клас у Kotlin оголошується через `class`, часто з primary constructor
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

Коротко: primary constructor — основний і найчастіший спосіб створення класів у
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

Коротко: наслідування в Kotlin явне й контрольоване. Клас, метод або властивість
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

Коротко: idiomatic singleton у Kotlin — це `object`. Він простий, лінивий і
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

Коротко: `Unit` — це Kotlin-тип для функцій без значимого результату. Він схожий
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

Коротко: smart cast — це автоматичне безпечне приведення типу після перевірки
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

Коротко: Kotlin null safety базується на розділенні `T` і `T?`, safe calls,
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

Коротко: Elvis-оператор `?:` — це компактний спосіб сказати: “використай це
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

Коротко: `!!` вимикає compile-time null safety для конкретного значення і може
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

Коротко: default parameters дають значення за замовчуванням у сигнатурі, а named
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

Коротко: destructuring declarations дозволяють розкласти обʼєкт на кілька
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

Коротко: `typealias` — це псевдонім для існуючого типу, корисний для
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

Коротко: масив — це структура даних, а `vararg` — зручний синтаксис параметра
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

Коротко: lambda в Kotlin — це функція як значення. Вона дозволяє передавати
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

Коротко: `List` зберігає порядок і дублікати, `Set` зберігає унікальні елементи,
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

Коротко: у Kotlin ітерація може бути імперативною (`for`) або функціональною
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

Коротко: список із `listOf()` не можна змінити через Kotlin `List` API — код не
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

Коротко: доступ через `list[index]` поза межами списку кидає
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

Коротко: Kotlin і Java добре сумісні, бо працюють на одній JVM-платформі й
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

Коротко: Java-анотації в Kotlin використовувати можна, але важливо розуміти
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

Коротко: Kotlin REPL — це інтерактивна консоль для швидких експериментів із
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

Коротко: Kotlin script `.kts` — це спосіб виконувати Kotlin як скриптову мову.
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

Коротко: Kotlin style guide — це правила консистентного написання Kotlin-коду.
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

Коротко: Kotlin-ідіоми — це усталені способи писати природний Kotlin-код: `val`
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

Коротко: Kotlin у backend використовують для JVM-сервісів на Spring, Ktor та
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

Коротко: функції вищого порядку — це функції, які приймають або повертають інші
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

Коротко: `inline` каже компілятору підставити тіло функції в місце виклику. Це
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

Коротко: `reified` дозволяє використовувати generic-тип `T` у runtime, але лише
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

Коротко: sealed-класи та sealed-інтерфейси дозволяють моделювати контрольовану
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

Коротко: `object expression` створює анонімний обʼєкт на місці. Його
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

Коротко: `enum` краще для простого фіксованого набору констант без різного
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

Коротко: `lateinit` — це обіцянка “я присвою це `var` до першого використання”.
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

Коротко: `Nothing` — це тип для коду, який ніколи не повертає нормальний
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

Коротко: Java може викликати Kotlin-функції з default parameters тільки через
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

Коротко: tail-recursive функція — це рекурсія, де рекурсивний виклик є останньою
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

Коротко: scope-функції дають короткий scope для роботи з обʼєктом. Вибір між
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

Коротко: `let` бере обʼєкт як `it` і повертає результат lambda. `run` бере
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

Коротко: `map` перетворює `List<A>` у `List<B>`. `flatMap` перетворює `List<A>`
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

Коротко: `Sequence` — це lazy pipeline для синхронної обробки елементів. Його
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

Коротко: lazy-обробка колекцій у Kotlin означає, що pipeline виконується тільки
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

Коротко: корутини — це легковагові асинхронні задачі, які можуть
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

Коротко: `suspend`-функція — це функція, яку можна призупинити й відновити в
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

Коротко: `CoroutineScope` — це власник корутин і їхнього lifecycle. Він задає
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

Коротко: `launch` повертає `Job` і підходить для задач без результату. `async`
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

Коротко: `runBlocking` запускає корутину і блокує поточний thread до її
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

Коротко: dispatcher визначає, де фізично виконується корутина. Правильний вибір
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

Коротко: корутини скасовуються кооперативно через `Job`, а помилки
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

Коротко: structured concurrency гарантує, що корутини не живуть самі по собі.
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

Коротко: корутини можуть виконуватись на різних потоках, але контроль над цим
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

Коротко: `Flow` — це coroutine-native API для асинхронних потоків даних. Він
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

Коротко: `Flow` — cold stream для асинхронних значень, `StateFlow` — hot stream
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

Коротко: `LiveData` привʼязаний до Android lifecycle, а `Flow` — до Kotlin
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

Коротко: cold stream виконується під кожного collector-а, hot stream існує
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

Коротко: `collectLatest` збирає flow, але при новому emission скасовує обробку
попереднього. Це правильний інструмент для UI input, search, preview і
сценаріїв, де потрібен тільки найсвіжіший результат.

</details>
<details>
<summary>62. Як перетворити callback-based API у suspend-функцію?</summary>

#### Kotlin

Callback-based API перетворюють у `suspend`-функцію за допомогою
`suspendCoroutine` або, частіше, `suspendCancellableCoroutine`. Другий варіант
кращий для production-коду, бо він підтримує cancellation і дозволяє коректно
відписатися від callback-а, якщо coroutine була скасована.

1. **Проблема callback API**

Приклад старого API:

```kotlin
interface UserApi {
    fun loadUser(
        id: String,
        callback: Callback<User>
    )
}

interface Callback<T> {
    fun onSuccess(value: T)
    fun onError(error: Throwable)
}
```

Такий API незручно використовувати в coroutine-коді:

```kotlin
viewModelScope.launch {
    // хочеться написати:
    val user = repository.loadUser(id)
}
```

Для цього callback потрібно обгорнути в `suspend`-функцію.

2. **suspendCoroutine**

Базовий варіант:

```kotlin
suspend fun UserApi.awaitUser(id: String): User =
    suspendCoroutine { continuation ->
        loadUser(
            id = id,
            callback = object : Callback<User> {
                override fun onSuccess(value: User) {
                    continuation.resume(value)
                }

                override fun onError(error: Throwable) {
                    continuation.resumeWithException(error)
                }
            }
        )
    }
```

`continuation.resume(value)` повертає результат із suspend-функції.
`resumeWithException(error)` завершує suspend-функцію з помилкою.

3. **suspendCancellableCoroutine**

Для реального коду краще:

```kotlin
suspend fun UserApi.awaitUser(id: String): User =
    suspendCancellableCoroutine { continuation ->
        val request = loadUser(
            id = id,
            callback = object : Callback<User> {
                override fun onSuccess(value: User) {
                    continuation.resume(value)
                }

                override fun onError(error: Throwable) {
                    continuation.resumeWithException(error)
                }
            }
        )

        continuation.invokeOnCancellation {
            request.cancel()
        }
    }
```

Тут важливий `invokeOnCancellation`: якщо coroutine скасували, ми скасовуємо
реальний underlying request.

4. **Якщо API повертає cancellable handle**

Багато callback API повертають обʼєкт, через який можна скасувати операцію:

```kotlin
interface Request {
    fun cancel()
}

fun loadUser(id: String, callback: Callback<User>): Request
```

Тоді bridge має обовʼязково викликати `cancel()`:

```kotlin
continuation.invokeOnCancellation {
    request.cancel()
}
```

Інакше coroutine буде скасована, але network/file/database operation може
продовжити виконуватись у фоні.

5. **Захист від подвійного callback-а**

Погані або legacy API іноді можуть викликати callback більше одного разу.
`Continuation` не можна resume-ити двічі.

Обережний варіант:

```kotlin
suspend fun UserApi.awaitUser(id: String): User =
    suspendCancellableCoroutine { continuation ->
        val request = loadUser(id, object : Callback<User> {
            override fun onSuccess(value: User) {
                if (continuation.isActive) {
                    continuation.resume(value)
                }
            }

            override fun onError(error: Throwable) {
                if (continuation.isActive) {
                    continuation.resumeWithException(error)
                }
            }
        })

        continuation.invokeOnCancellation {
            request.cancel()
        }
    }
```

`isActive` не робить поганий API ідеальним, але захищає від частини проблем,
коли результат приходить після cancellation.

6. **Callback з nullable результатом**

Якщо API може повернути `null`, краще явно визначити контракт:

```kotlin
suspend fun UserApi.awaitUser(id: String): User =
    suspendCancellableCoroutine { continuation ->
        val request = loadUser(id, object : Callback<User?> {
            override fun onSuccess(value: User?) {
                if (value != null) {
                    continuation.resume(value)
                } else {
                    continuation.resumeWithException(
                        NoSuchElementException("User not found")
                    )
                }
            }

            override fun onError(error: Throwable) {
                continuation.resumeWithException(error)
            }
        })

        continuation.invokeOnCancellation {
            request.cancel()
        }
    }
```

Не треба мовчки повертати `null`, якщо доменна модель очікує non-null `User`.

7. **Якщо callback багаторазовий**

Якщо callback повертає багато значень, це вже не `suspend`-функція, а `Flow`.

Погано робити так:

```kotlin
suspend fun observeLocation(): Location
```

Якщо location updates приходять багато разів, краще:

```kotlin
fun observeLocation(): Flow<Location> = callbackFlow {
    val listener = object : LocationListener {
        override fun onLocationChanged(location: Location) {
            trySend(location)
        }
    }

    locationClient.addListener(listener)

    awaitClose {
        locationClient.removeListener(listener)
    }
}
```

Правило: один результат — `suspendCancellableCoroutine`, багато результатів —
`callbackFlow`.

8. **Помилки і cancellation**

Не треба перетворювати cancellation у business error:

```kotlin
continuation.invokeOnCancellation {
    request.cancel()
}
```

Якщо request повернув cancellation-specific exception, варто мапити її обережно.
У coroutine-коді `CancellationException` має залишатися cancellation, а не
перетворюватися на `UiState.Error`.

9. **Практичне правило**

- Для one-shot callback API використовуй `suspendCancellableCoroutine`.
- Обовʼязково підключай `invokeOnCancellation`.
- `resume` — для успіху.
- `resumeWithException` — для помилки.
- Для багаторазових callback-ів використовуй `callbackFlow`.
- Не resume-и continuation більше одного разу.

Коротко: callback-based API перетворюють у suspend-функцію через
`suspendCancellableCoroutine`, де callback викликає `resume` або
`resumeWithException`, а cancellation coroutine скасовує underlying operation.

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

Коротко: `Channel` — це асинхронна черга для coroutine communication. `Flow` —
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

Коротко: оператори `Flow` дозволяють будувати читабельні reactive pipelines без
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

Коротко: `combine` працює з останніми значеннями потоків і реагує на кожне
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

Коротко: ці анотації потрібні не для Kotlin-логіки, а для форми JVM API. Вони
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

Коротко: delegation у Kotlin — це language-level підтримка composition. Вона
дозволяє повторно використовувати поведінку без жорсткого inheritance і робить
код гнучкішим, тестованішим і простішим для заміни реалізацій.

</details>
<details>
<summary>68. Які фреймворки для тестування доступні в Kotlin?</summary>

#### Kotlin

У Kotlin найчастіше використовують не один фреймворк, а набір інструментів:
test runner, assertion library, mocking library, coroutine test utilities і,
для Android, окремі інструменти для JVM/instrumented тестів. Вибір залежить від
типу проєкту: backend, Android, multiplatform або бібліотека.

1. **JUnit 4**

JUnit 4 досі дуже поширений, особливо в Android-проєктах:

```kotlin
class UserValidatorTest {

    @Test
    fun `returns false when email is invalid`() {
        val validator = UserValidator()

        val result = validator.isValidEmail("wrong")

        assertFalse(result)
    }
}
```

Переваги:

- добре підтримується Android tooling;
- багато legacy-проєктів уже на ньому;
- простий API;
- сумісний із багатьма libraries.

Недолік — менш сучасна модель extension-ів порівняно з JUnit 5.

2. **JUnit 5**

JUnit 5 — сучасніший варіант для JVM/Kotlin-проєктів:

```kotlin
class PriceCalculatorTest {

    @Test
    fun `applies discount`() {
        val calculator = PriceCalculator()

        val result = calculator.applyDiscount(100, 10)

        assertEquals(90, result)
    }
}
```

Переваги:

- краща extension model;
- parameterized tests;
- nested tests;
- modern JVM testing API;
- добре підходить для backend і library modules.

Parameterized test:

```kotlin
@ParameterizedTest
@CsvSource(
    "100, 10, 90",
    "200, 50, 150"
)
fun `applies discount`(price: Int, discount: Int, expected: Int) {
    assertEquals(expected, PriceCalculator().applyDiscount(price, discount))
}
```

3. **kotlin.test**

`kotlin.test` — стандартна Kotlin testing abstraction:

```kotlin
import kotlin.test.Test
import kotlin.test.assertEquals

class UserTest {
    @Test
    fun `creates display name`() {
        assertEquals("John Doe", User("John", "Doe").displayName)
    }
}
```

Його часто використовують у Kotlin Multiplatform, бо він абстрагує тестовий API
для різних платформ.

4. **Kotest**

Kotest — Kotlin-first test framework з багатьма стилями специфікацій і сильними
assertions:

```kotlin
class UserValidatorTest : StringSpec({

    "invalid email should return false" {
        val validator = UserValidator()

        validator.isValidEmail("wrong") shouldBe false
    }
})
```

Переваги:

- expressive DSL;
- property-based testing;
- matchers;
- data-driven tests;
- добре читається в Kotlin.

Kotest добре підходить, коли команда хоче більш BDD/spec-like стиль.

5. **MockK**

MockK — популярна mocking library, добре адаптована під Kotlin:

```kotlin
val repository = mockk<UserRepository>()

coEvery { repository.loadUser("1") } returns User("1", "John")

val useCase = LoadUserUseCase(repository)
val result = useCase("1")

coVerify { repository.loadUser("1") }
```

MockK добре працює з:

- final classes;
- Kotlin properties;
- suspend functions;
- extension-like Kotlin patterns;
- relaxed mocks.

Для Kotlin це часто природніший вибір, ніж Mockito, хоча Mockito теж можливий.

6. **Mockito**

Mockito — класична Java mocking library:

```kotlin
val repository = mock(UserRepository::class.java)
whenever(repository.loadUser("1")).thenReturn(User("1", "John"))
```

У Kotlin для Mockito часто використовують wrapper-и типу `mockito-kotlin`, щоб
зменшити проблеми з nullability і синтаксисом.

Mockito має сенс, якщо:

- команда вже використовує його в Java-коді;
- проєкт змішаний Java/Kotlin;
- є існуюча тестова інфраструктура на Mockito.

7. **Turbine для Flow**

Turbine — бібліотека для тестування `Flow`:

```kotlin
repository.observeUser().test {
    assertEquals(UiState.Loading, awaitItem())
    assertEquals(UiState.Success(user), awaitItem())
    awaitComplete()
}
```

Вона робить Flow-тести значно читабельнішими, ніж ручний `collect` у список.

Типові сценарії:

- перевірити порядок emissions;
- перевірити completion;
- перевірити errors;
- тестувати `StateFlow`/`SharedFlow` поведінку.

8. **kotlinx-coroutines-test**

Для корутин використовують `kotlinx-coroutines-test`:

```kotlin
@Test
fun `loads user`() = runTest {
    val repository = FakeUserRepository()
    val useCase = LoadUserUseCase(repository)

    val user = useCase("1")

    assertEquals("John", user.name)
}
```

Основні інструменти:

- `runTest`;
- `TestScope`;
- `StandardTestDispatcher`;
- `UnconfinedTestDispatcher`;
- virtual time через `advanceUntilIdle`, `advanceTimeBy`.

Це must-have для тестування suspend-функцій, ViewModel coroutine logic і Flow.

9. **Android testing**

Для Android є два основні типи тестів:

- local JVM tests — виконуються на JVM без девайса;
- instrumented tests — виконуються на Android device/emulator.

Local tests:

```kotlin
class UserMapperTest {
    @Test
    fun `maps dto to domain`() {
        // pure JVM test
    }
}
```

Instrumented tests:

```kotlin
@RunWith(AndroidJUnit4::class)
class UserDaoTest {
    @Test
    fun insertsUser() {
        // Room/instrumented scenario
    }
}
```

Для UI часто використовують:

- Espresso для View-based UI;
- Compose UI Testing для Jetpack Compose;
- Robolectric для запуску частини Android behavior на JVM.

10. **AssertJ / Truth / Kotest matchers**

Assertion library впливає на читабельність тестів.

Truth:

```kotlin
assertThat(result.name).isEqualTo("John")
```

AssertJ:

```kotlin
assertThat(users).hasSize(2)
```

Kotest matchers:

```kotlin
users shouldHaveSize 2
```

Для Kotlin часто добре заходять Kotest matchers або Truth, особливо в Android.

11. **Практичний вибір**

Для сучасного Kotlin/JVM:

- JUnit 5;
- MockK;
- Kotest assertions або AssertJ;
- kotlinx-coroutines-test;
- Turbine для Flow.

Для Android:

- JUnit 4 або JUnit 5 залежно від setup;
- MockK або Mockito;
- kotlinx-coroutines-test;
- Turbine;
- Espresso або Compose UI Test;
- Robolectric за потреби.

Для Kotlin Multiplatform:

- `kotlin.test`;
- platform-specific test runners;
- обережно з mocking, часто краще fake implementations.

12. **Практичне правило**

Не треба вибирати фреймворк за популярністю. Треба дивитись на тип коду:
pure Kotlin domain logic, Android UI, coroutines, Flow, multiplatform або legacy
Java interop. Найкраща тестова інфраструктура — та, яка робить тести швидкими,
читабельними і стабільними.

Коротко: у Kotlin найчастіше використовують JUnit або Kotest як test framework,
MockK або Mockito для mocking, `kotlinx-coroutines-test` для корутин, Turbine
для Flow, а в Android — Espresso, Compose UI Test, Robolectric та instrumented
tests.

</details>
<details>
<summary>69. Як мокати залежності у тестах?</summary>

#### Kotlin

Мокання залежностей у тестах — це заміна реальних dependency на контрольовані
test doubles: mocks, stubs або fakes. Мета не в тому, щоб "замокати все", а в
тому, щоб ізолювати unit under test, зробити сценарій передбачуваним і
перевірити поведінку без мережі, бази даних, Android framework або часу.

1. **Що саме мокають**

Зазвичай мокають зовнішні залежності:

```kotlin
class LoadUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User {
        return repository.loadUser(id)
    }
}
```

У тесті `UserRepository` можна замінити mock-ом або fake-ом, щоб не ходити в
реальну мережу чи базу.

2. **MockK для Kotlin**

MockK добре підходить для Kotlin, особливо для `suspend`-функцій:

```kotlin
class LoadUserUseCaseTest {

    private val repository = mockk<UserRepository>()
    private val useCase = LoadUserUseCase(repository)

    @Test
    fun `loads user by id`() = runTest {
        coEvery { repository.loadUser("1") } returns User(id = "1", name = "John")

        val result = useCase("1")

        assertEquals("John", result.name)
        coVerify(exactly = 1) { repository.loadUser("1") }
    }
}
```

Для suspend-функцій у MockK використовують:

- `coEvery` — задати поведінку;
- `coVerify` — перевірити виклик;
- `coAnswers` — задати custom відповідь.

3. **Звичайні функції в MockK**

```kotlin
val analytics = mockk<Analytics>()

every { analytics.track("screen_opened") } returns Unit

analytics.track("screen_opened")

verify { analytics.track("screen_opened") }
```

Для `Unit`-методів можна використовувати:

```kotlin
justRun { analytics.track(any()) }
```

або relaxed mock:

```kotlin
val analytics = mockk<Analytics>(relaxed = true)
```

Але relaxed mocks треба використовувати обережно: вони можуть приховати
неочікувані виклики.

4. **Mockito в Kotlin**

Mockito теж можна використовувати, особливо у змішаних Java/Kotlin-проєктах:

```kotlin
val repository = mock<UserRepository>()

whenever(repository.loadUser("1"))
    .thenReturn(User(id = "1", name = "John"))

val result = useCase("1")

verify(repository).loadUser("1")
```

Для Kotlin краще використовувати `mockito-kotlin`, бо чистий Mockito має
незручності з nullability і generic API.

5. **Fake замість mock**

Часто fake кращий за mock:

```kotlin
class FakeUserRepository : UserRepository {
    private val users = mutableMapOf<String, User>()

    fun givenUser(user: User) {
        users[user.id] = user
    }

    override suspend fun loadUser(id: String): User {
        return users.getValue(id)
    }
}
```

Тест:

```kotlin
@Test
fun `loads user`() = runTest {
    val repository = FakeUserRepository()
    repository.givenUser(User("1", "John"))

    val useCase = LoadUserUseCase(repository)

    val result = useCase("1")

    assertEquals("John", result.name)
}
```

Fake часто стабільніший, краще читається і менше привʼязує тест до внутрішніх
викликів.

6. **Mock vs fake**

Mock перевіряє взаємодію:

```kotlin
coVerify { repository.loadUser("1") }
```

Fake перевіряє результат:

```kotlin
assertEquals("John", result.name)
```

Практичне правило: якщо важливий final state/result — краще fake. Якщо важливо,
що конкретний dependency був викликаний із конкретними параметрами — mock.

7. **Не мокати value objects і data classes**

Погано:

```kotlin
val user = mockk<User>()
every { user.name } returns "John"
```

Краще:

```kotlin
val user = User(id = "1", name = "John")
```

Не треба мокати прості моделі, `data class`, DTO, value objects. Їх дешевше і
надійніше створювати напряму.

8. **Не мокати те, що ти не контролюєш**

Поганий сигнал — мокати глибокі Android/framework/static деталі замість того,
щоб винести їх за interface:

```kotlin
interface NetworkStatusProvider {
    fun isOnline(): Boolean
}
```

Production implementation може використовувати Android API, а unit test —
простий fake:

```kotlin
class FakeNetworkStatusProvider(
    private val online: Boolean
) : NetworkStatusProvider {
    override fun isOnline(): Boolean = online
}
```

Це краще, ніж намагатися мокати складні platform classes.

9. **Мокання Flow**

Repository, який повертає `Flow`, можна замокати так:

```kotlin
every { repository.observeUser() } returns flowOf(User("1", "John"))
```

Або для змінного stream-а використати `MutableSharedFlow`:

```kotlin
val userFlow = MutableSharedFlow<User>()

every { repository.observeUser() } returns userFlow

userFlow.emit(User("1", "John"))
```

Для перевірки Flow зручно використовувати Turbine.

10. **Мокання часу і dispatcher-ів**

Не треба напряму викликати `System.currentTimeMillis()` у бізнес-логіці, якщо
код треба тестувати. Краще інжектити clock:

```kotlin
interface Clock {
    fun nowMillis(): Long
}
```

Так само dispatcher-и краще інжектити:

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

У тесті можна передати test dispatcher.

11. **Типові помилки**

- Мокати все підряд.
- Перевіряти implementation details замість поведінки.
- Мокати `data class` замість створення реального обʼєкта.
- Використовувати relaxed mocks без потреби.
- Не перевіряти negative cases.
- Залишати тести залежними від реального часу, мережі або dispatcher-ів.

12. **Практичне правило**

Починай з fake, якщо dependency має просту поведінку і тесту важливий результат.
Використовуй mock, якщо треба перевірити interaction. Для Kotlin suspend-коду
зазвичай зручний MockK (`coEvery`, `coVerify`). Для стабільної архітектури
краще проектувати залежності через interface і dependency injection.

Коротко: мокання — це інструмент ізоляції, а не самоціль. Якісний Kotlin-тест
мінімізує mocks, використовує fakes там, де це простіше, і перевіряє поведінку,
а не випадкові деталі реалізації.

</details>
<details>
<summary>70. Як тестувати корутини та Flow?</summary>

#### Kotlin

Корутини і `Flow` треба тестувати через `kotlinx-coroutines-test`, test
dispatcher-и, virtual time і контрольовані test doubles. Головна мета — зробити
асинхронний код синхронно контрольованим у тесті: без реальних delay, реальних
thread scheduling race conditions і залежності від Android main thread.

1. **runTest**

Для suspend-коду використовують `runTest`:

```kotlin
@Test
fun `loads user`() = runTest {
    val repository = FakeUserRepository()
    repository.givenUser(User("1", "John"))

    val useCase = LoadUserUseCase(repository)

    val result = useCase("1")

    assertEquals("John", result.name)
}
```

`runTest` створює test coroutine scope і дає контроль над virtual time. Це
краще, ніж `runBlocking`, бо `runBlocking` блокує thread і не дає нормального
контролю над delays.

2. **Test dispatcher**

Якщо production-код використовує dispatcher, його треба інжектити:

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

У тесті:

```kotlin
@Test
fun `loads user on test dispatcher`() = runTest {
    val repository = UserRepository(
        api = fakeApi,
        ioDispatcher = StandardTestDispatcher(testScheduler)
    )

    val user = repository.loadUser()

    assertEquals("John", user.name)
}
```

Так тест контролює scheduling і не залежить від реального `Dispatchers.IO`.

3. **advanceUntilIdle і advanceTimeBy**

Для коду з `delay` використовують virtual time:

```kotlin
@Test
fun `emits after delay`() = runTest {
    var completed = false

    launch {
        delay(1_000)
        completed = true
    }

    assertFalse(completed)

    advanceTimeBy(1_000)

    assertTrue(completed)
}
```

`advanceUntilIdle()` виконує всі заплановані задачі до idle state:

```kotlin
advanceUntilIdle()
```

Це корисно для ViewModel, де всередині запускаються корутини.

4. **Тестування ViewModel з корутинами**

Якщо `ViewModel` використовує `viewModelScope`, у JVM-тестах треба підмінити
`Dispatchers.Main`:

```kotlin
@OptIn(ExperimentalCoroutinesApi::class)
class MainDispatcherRule(
    private val dispatcher: TestDispatcher = StandardTestDispatcher()
) : TestWatcher() {

    override fun starting(description: Description) {
        Dispatchers.setMain(dispatcher)
    }

    override fun finished(description: Description) {
        Dispatchers.resetMain()
    }
}
```

У тесті:

```kotlin
@get:Rule
val mainDispatcherRule = MainDispatcherRule()

@Test
fun `loads user and updates state`() = runTest {
    val repository = FakeUserRepository()
    repository.givenUser(User("1", "John"))
    val viewModel = UserViewModel(repository)

    viewModel.load("1")
    advanceUntilIdle()

    assertEquals(UiState.Success(User("1", "John")), viewModel.state.value)
}
```

5. **Тестування Flow через toList**

Для простого finite `Flow` можна зібрати значення в список:

```kotlin
@Test
fun `emits users`() = runTest {
    val result = repository.observeUsers()
        .take(2)
        .toList()

    assertEquals(
        listOf(User("1", "John"), User("2", "Anna")),
        result
    )
}
```

`take(2)` важливий, якщо flow може бути нескінченним. Без обмеження тест може
зависнути.

6. **Turbine для Flow**

Для більшості Flow-тестів зручніше Turbine:

```kotlin
@Test
fun `emits loading and success`() = runTest {
    viewModel.state.test {
        assertEquals(UiState.Loading, awaitItem())

        viewModel.load("1")

        assertEquals(UiState.Success(User("1", "John")), awaitItem())
        cancelAndIgnoreRemainingEvents()
    }
}
```

Turbine дає читабельний API:

- `awaitItem()`;
- `awaitComplete()`;
- `awaitError()`;
- `expectNoEvents()`;
- `cancelAndIgnoreRemainingEvents()`.

7. **Тестування StateFlow**

`StateFlow` завжди має поточне значення:

```kotlin
@Test
fun `initial state is loading`() = runTest {
    val viewModel = UserViewModel(repository)

    assertEquals(UiState.Loading, viewModel.state.value)
}
```

Якщо треба перевірити послідовність emissions:

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load("1")
    advanceUntilIdle()

    assertEquals(UiState.Success(user), awaitItem())
}
```

8. **Тестування SharedFlow events**

Для events через `SharedFlow` важливо спочатку підписатися, а потім викликати
дію:

```kotlin
@Test
fun `emits snackbar event on error`() = runTest {
    viewModel.events.test {
        viewModel.load("wrong-id")
        advanceUntilIdle()

        assertEquals(UiEvent.ShowSnackbar("Load failed"), awaitItem())
    }
}
```

Якщо `SharedFlow` має `replay = 0`, event, який стався до collection, новий
collector не отримає.

9. **Тестування debounce**

Virtual time дозволяє тестувати `debounce` без реального очікування:

```kotlin
@Test
fun `search is triggered after debounce`() = runTest {
    val query = MutableStateFlow("")
    val results = searchUseCase.observe(query)

    results.test {
        query.value = "kot"
        advanceTimeBy(299)
        expectNoEvents()

        advanceTimeBy(1)
        assertEquals(SearchResult("kot"), awaitItem())
    }
}
```

10. **Не тестувати корутини через Thread.sleep**

Погано:

```kotlin
viewModel.load()
Thread.sleep(1_000)
assertEquals(expected, viewModel.state.value)
```

Такі тести повільні й flaky. Краще:

```kotlin
viewModel.load()
advanceUntilIdle()
assertEquals(expected, viewModel.state.value)
```

11. **Типові помилки**

- Використовувати `runBlocking` замість `runTest`.
- Хардкодити `Dispatchers.IO`/`Default` у production-коді без можливості заміни.
- Забувати `Dispatchers.setMain` для ViewModel JVM-тестів.
- Тестувати нескінченний `Flow` без `take`, Turbine або cancellation.
- Використовувати `Thread.sleep`.
- Не викликати `advanceUntilIdle`, коли coroutine запускається асинхронно.
- Очікувати event із `SharedFlow` після того, як він уже був emitted.

12. **Практичне правило**

Для suspend-функцій — `runTest`. Для dispatcher-ів — dependency injection і
`TestDispatcher`. Для часу — `advanceTimeBy`/`advanceUntilIdle`. Для `Flow` —
Turbine або контрольований `toList` із `take`. Для ViewModel — підміна
`Dispatchers.Main`.

Коротко: корутини й `Flow` тестуються стабільно тільки тоді, коли тест контролює
dispatcher, lifecycle collection і virtual time. Хороший тест не спить реальний
час і не залежить від випадкового thread scheduling.

</details>
<details>
<summary>71. Які best practices для написання тестованого коду?</summary>

#### Kotlin

Тестований код — це код, у якому бізнес-логіку можна перевірити швидко,
детерміновано і без реальної мережі, бази, Android framework, часу або
випадкового scheduling. Хороша тестованість зазвичай не додається після факту:
вона є наслідком правильної архітектури, залежностей і меж відповідальності.

1. **Виносити бізнес-логіку з framework-класів**

Погано, коли вся логіка живе в `Activity`, `Fragment` або Android-specific
класах:

```kotlin
class UserFragment : Fragment() {
    fun onSaveClicked() {
        // validation
        // mapping
        // network call
        // navigation
    }
}
```

Краще виносити логіку в ViewModel/use case/domain services:

```kotlin
class SaveUserUseCase(
    private val repository: UserRepository,
    private val validator: UserValidator
) {
    suspend operator fun invoke(input: UserInput): SaveResult {
        if (!validator.isValid(input)) {
            return SaveResult.Invalid
        }

        repository.save(input.toUser())
        return SaveResult.Success
    }
}
```

Такий клас легко тестувати як pure Kotlin code.

2. **Dependency Injection**

Клас не повинен сам створювати важкі залежності:

```kotlin
class UserRepository {
    private val api = Retrofit.Builder()
        .baseUrl("https://example.com")
        .build()
        .create(UserApi::class.java)
}
```

Краще:

```kotlin
class UserRepository(
    private val api: UserApi,
    private val ioDispatcher: CoroutineDispatcher
)
```

У production передаємо real `UserApi`, у тесті — fake або mock.

3. **Залежати від interface, а не concrete implementation**

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
}

class LoadUserUseCase(
    private val repository: UserRepository
)
```

Тест:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun loadUser(id: String): User {
        return User(id, "John")
    }
}
```

Interface дає можливість замінити реалізацію без складного mocking framework.

4. **Уникати глобального стану**

Глобальний mutable state робить тести залежними один від одного:

```kotlin
object SessionManager {
    var token: String? = null
}
```

Краще:

```kotlin
interface SessionStorage {
    fun getToken(): String?
    fun setToken(token: String?)
}
```

Тоді в тесті можна передати isolated fake storage.

5. **Інжектити час**

Погано:

```kotlin
fun isExpired(token: Token): Boolean {
    return token.expiresAt < System.currentTimeMillis()
}
```

Краще:

```kotlin
interface Clock {
    fun nowMillis(): Long
}

class TokenValidator(
    private val clock: Clock
) {
    fun isExpired(token: Token): Boolean {
        return token.expiresAt < clock.nowMillis()
    }
}
```

Тест стає детермінованим:

```kotlin
val clock = FakeClock(nowMillis = 1_000)
```

6. **Інжектити dispatcher-и**

Погано:

```kotlin
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.loadUser()
}
```

Краще:

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

У тестах можна передати `StandardTestDispatcher(testScheduler)` і контролювати
coroutine execution.

7. **Розділяти pure logic і side effects**

Pure function легко тестувати:

```kotlin
fun calculateDiscount(price: Money, user: User): Money {
    return if (user.isPremium) price * 0.9 else price
}
```

Side effects треба ізолювати за interface:

```kotlin
interface PaymentGateway {
    suspend fun charge(amount: Money): PaymentResult
}
```

Чим більше логіки є pure Kotlin-кодом, тим швидші й надійніші тести.

8. **Не тестувати implementation details**

Поганий тест:

```kotlin
verify { mapper.map(dto) }
verify { cache.put(user) }
verify { logger.log(any()) }
```

Якщо тест перевіряє кожен внутрішній виклик, refactoring стає болючим.

Краще перевіряти behavior:

```kotlin
val result = useCase("1")

assertEquals(User("1", "John"), result)
```

Interaction tests потрібні, але не мають заміняти перевірку результату.

9. **Малі класи з однією відповідальністю**

Клас, який робить validation, mapping, network, caching і navigation, важко
тестувати.

Краще розділити:

- `UserValidator`;
- `UserMapper`;
- `UserRepository`;
- `SaveUserUseCase`;
- `UserViewModel`.

Кожен компонент має простий контракт і тестується окремо.

10. **Явні результати замість прихованих exception-flow**

Для domain-level помилок часто краще мати явний result type:

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkError : LoginResult
}
```

Тест:

```kotlin
assertEquals(LoginResult.InvalidCredentials, result)
```

Exception-и краще залишати для unexpected failures або boundary layers.

11. **Не залежати від порядку тестів**

Кожен тест має сам готувати свій стан:

```kotlin
@Before
fun setUp() {
    repository = FakeUserRepository()
    useCase = LoadUserUseCase(repository)
}
```

Не можна покладатися на те, що попередній тест щось створив або очистив.

12. **Fake builders і test data factories**

Щоб тести були читабельні:

```kotlin
fun user(
    id: String = "1",
    name: String = "John",
    premium: Boolean = false
) = User(
    id = id,
    name = name,
    isPremium = premium
)
```

Тоді тест фокусується на суті:

```kotlin
val result = calculator.calculate(user(premium = true))
```

13. **Практичне правило**

- Логіку тримати в pure Kotlin класах.
- Залежності передавати через constructor.
- Час, dispatcher-и, storage, network — інжектити.
- Framework API ховати за interface.
- Перевіряти behavior, а не внутрішню реалізацію.
- Не мокати прості value objects.
- Робити тести детермінованими і незалежними.

Коротко: тестований Kotlin-код — це код із чіткими межами, injected
dependencies, мінімумом глобального стану і відокремленими side effects.
Архітектура має дозволяти перевіряти бізнес-логіку як швидкий pure JVM test, а
не як важкий інтеграційний сценарій.

</details>
<details>
<summary>72. Як ефективно керувати залежностями у Kotlin-проєкті?</summary>

#### Kotlin

Ефективне керування залежностями в Kotlin-проєкті — це не тільки додати library
в Gradle. Це контроль версій, модульність, dependency injection, ізоляція
framework-коду, мінімізація transitive dependencies і стабільна build
інфраструктура. Погане dependency management швидко призводить до конфліктів
версій, повільних build-ів і коду, який важко тестувати.

1. **Використовувати Gradle version catalogs**

У сучасних Gradle-проєктах залежності зручно тримати в `libs.versions.toml`:

```toml
[versions]
kotlin = "2.0.0"
coroutines = "1.9.0"
retrofit = "2.11.0"

[libraries]
kotlinx-coroutines-core = { module = "org.jetbrains.kotlinx:kotlinx-coroutines-core", version.ref = "coroutines" }
retrofit-core = { module = "com.squareup.retrofit2:retrofit", version.ref = "retrofit" }

[plugins]
kotlin-jvm = { id = "org.jetbrains.kotlin.jvm", version.ref = "kotlin" }
```

У `build.gradle.kts`:

```kotlin
dependencies {
    implementation(libs.kotlinx.coroutines.core)
    implementation(libs.retrofit.core)
}
```

Це прибирає дублювання версій і робить upgrades контрольованими.

2. **Не розкидати версії по build files**

Погано:

```kotlin
dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.9.0")
    implementation("com.squareup.retrofit2:retrofit:2.11.0")
}
```

Якщо таких модулів багато, версії починають розʼїжджатися. Краще мати одне
джерело правди: version catalog або convention plugins.

3. **Розділяти api і implementation**

У Gradle важливо правильно вибирати configuration:

```kotlin
dependencies {
    implementation(libs.retrofit.core)
    api(libs.kotlinx.coroutines.core)
}
```

`implementation` — dependency потрібна тільки всередині модуля.
`api` — dependency стає частиною public API модуля.

Правило: за замовчуванням використовувати `implementation`. `api` тільки тоді,
коли типи з цієї dependency реально присутні в public signatures.

4. **Модульність**

Залежності легше контролювати, коли проєкт розбитий на модулі:

```text
:core:domain
:core:data
:feature:user
:feature:orders
:app
```

Domain-модуль не повинен залежати від Android, Retrofit або Room:

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
}
```

Data-модуль реалізує interface:

```kotlin
class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Так залежності йдуть у правильному напрямку: UI -> domain, data -> domain, але
domain не знає про framework.

5. **Dependency Injection**

Залежності краще передавати через constructor:

```kotlin
class LoadUserUseCase(
    private val repository: UserRepository
)
```

Це робить клас:

- тестованим;
- явним;
- незалежним від service locator;
- простим для заміни implementation.

Для Android часто використовують Hilt:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object RepositoryModule {

    @Provides
    fun provideUserRepository(api: UserApi): UserRepository {
        return RealUserRepository(api)
    }
}
```

6. **Не створювати залежності всередині класів**

Погано:

```kotlin
class UserViewModel : ViewModel() {
    private val repository = RealUserRepository(RetrofitUserApi())
}
```

Краще:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel()
```

Перший варіант важко тестувати і важко конфігурувати. Другий дозволяє передати
fake repository у тесті.

7. **Контролювати transitive dependencies**

Бібліотеки часто тягнуть інші бібліотеки transitively. Це може створити
конфлікти версій або зайвий розмір artifact-а.

```kotlin
dependencies {
    implementation(libs.some.library) {
        exclude(group = "com.unwanted", module = "legacy-json")
    }
}
```

Такі exclusions треба робити обережно і документувати причину, бо можна зламати
runtime behavior.

8. **Оновлювати залежності системно**

Не варто оновлювати залежності хаотично. Краще мати процес:

- перевірити changelog;
- оновити одну групу залежностей;
- запустити unit tests;
- запустити integration/UI tests, якщо потрібно;
- перевірити binary/source breaking changes;
- зафіксувати причину оновлення в PR.

Для великих проєктів корисні Renovate або Dependabot, але оновлення все одно
мають проходити через review і CI.

9. **Не тягнути важку бібліотеку заради маленької функції**

Поганий trade-off:

```kotlin
implementation("huge-library")
```

лише щоб використати одну helper-функцію.

Перед додаванням dependency варто оцінити:

- розмір;
- transitive dependencies;
- maintenance status;
- license;
- security history;
- чи можна вирішити задачу простим кодом;
- чи не дублює вона вже наявну dependency.

10. **Версії Kotlin, Gradle і plugins мають бути сумісні**

Kotlin-проєкт сильно залежить від сумісності:

- Kotlin version;
- Gradle version;
- Android Gradle Plugin;
- Compose compiler/plugin;
- KSP/KAPT;
- serialization plugin;
- coroutines/test libraries.

Не можна оновлювати Kotlin compiler, ігноруючи plugins. Часто треба оновлювати
групу повʼязаних залежностей разом.

11. **KAPT vs KSP**

Якщо бібліотека підтримує KSP, у Kotlin-проєктах часто краще KSP:

```kotlin
plugins {
    alias(libs.plugins.ksp)
}
```

KSP зазвичай швидший і краще інтегрується з Kotlin, ніж KAPT. Але міграцію треба
робити поступово і перевіряти generated code.

12. **Test dependencies окремо**

Не треба тягнути test-only бібліотеки в production:

```kotlin
dependencies {
    testImplementation(libs.junit)
    testImplementation(libs.mockk)
    testImplementation(libs.kotlinx.coroutines.test)
}
```

Android:

```kotlin
androidTestImplementation(libs.androidx.test.runner)
androidTestImplementation(libs.espresso.core)
```

Це зменшує production classpath і ризик конфліктів.

13. **Практичне правило**

- Версії тримати централізовано.
- За замовчуванням використовувати `implementation`, не `api`.
- Domain не має залежати від framework.
- Залежності передавати через constructor.
- Transitive dependencies контролювати.
- Оновлення робити через CI і changelog review.
- Не додавати library без оцінки вартості.

Коротко: ефективне керування залежностями — це контроль меж і версій. Хороший
Kotlin-проєкт має централізовані dependency versions, чистий dependency graph,
constructor injection, ізольований domain layer і мінімум випадкових transitive
залежностей.

</details>
<details>
<summary>73. Як Kotlin працює з памʼяттю та garbage collection?</summary>

#### Kotlin

На JVM Kotlin використовує ту саму модель памʼяті й garbage collection, що й
Java. Kotlin-код компілюється в JVM bytecode, обʼєкти живуть у heap, локальні
посилання — у stack frames, а очищенням недосяжних обʼєктів займається JVM GC.
Тобто в Kotlin немає ручного `free()` або `delete`, але це не означає, що можна
ігнорувати памʼять.

1. **Heap і stack**

Обʼєкти зазвичай створюються в heap:

```kotlin
data class User(val id: String, val name: String)

fun createUser(): User {
    return User("1", "John")
}
```

`User` — обʼєкт у heap, а локальна змінна/посилання `user` живе в stack frame
поточного виклику.

```kotlin
fun printUser() {
    val user = User("1", "John")
    println(user.name)
}
```

Коли метод завершується, stack frame зникає. Якщо на `user` більше немає
досяжних посилань, обʼєкт стає кандидатом на garbage collection.

2. **Garbage Collection**

GC очищає обʼєкти, до яких більше неможливо дістатися з GC roots:

- active threads;
- stack frames;
- static fields;
- JNI references;
- class loaders;
- інші runtime roots.

Якщо обʼєкт усе ще reachable, GC його не видалить, навіть якщо він логічно вже
не потрібен.

3. **Memory leaks у JVM/Kotlin**

Memory leak у Kotlin — це зазвичай не "забули звільнити памʼять", а "залишили
непотрібне посилання".

Погано:

```kotlin
object UserCache {
    val users = mutableListOf<User>()
}
```

Якщо список росте без очищення, GC не зможе прибрати `User`, бо вони reachable
через singleton `UserCache`.

Краще:

```kotlin
class UserCache(
    private val maxSize: Int
) {
    private val users = LinkedHashMap<String, User>()

    fun put(user: User) {
        users[user.id] = user
        if (users.size > maxSize) {
            val firstKey = users.keys.first()
            users.remove(firstKey)
        }
    }
}
```

4. **Android memory leaks**

В Android типова проблема — тримати посилання на `Activity`, `View` або
`Context` довше, ніж їхній lifecycle.

Погано:

```kotlin
object AnalyticsHolder {
    var context: Context? = null
}
```

Якщо туди покласти `Activity`, вона може не звільнитися після rotation або
закриття екрана.

Краще:

```kotlin
class Analytics(
    context: Context
) {
    private val appContext = context.applicationContext
}
```

Для довгоживучих обʼєктів треба використовувати `applicationContext`, а не
`Activity context`, якщо UI context не потрібен.

5. **Корутини і памʼять**

Корутина тримає references на все, що захоплено в її lambda:

```kotlin
viewModelScope.launch {
    val result = repository.loadData()
    state.value = result
}
```

Це нормально, якщо scope має правильний lifecycle.

Погано:

```kotlin
GlobalScope.launch {
    activity.doSomething()
}
```

Тут coroutine може пережити `Activity` і утримувати її в памʼяті. Тому
`GlobalScope` у production Android-коді майже завжди поганий сигнал.

6. **Collections і allocation pressure**

Kotlin collection operations зручні, але можуть створювати проміжні колекції:

```kotlin
val result = users
    .map { it.toUiModel() }
    .filter { it.isVisible }
```

Для невеликих списків це нормально. Для великих data sets або hot paths можна
розглянути `asSequence()`:

```kotlin
val result = users
    .asSequence()
    .map { it.toUiModel() }
    .filter { it.isVisible }
    .toList()
```

Але не треба використовувати sequence всюди автоматично. Для малих колекцій
звичайні collection operators часто простіші й достатньо швидкі.

7. **Inline/value classes**

Kotlin має value classes, які можуть зменшити зайві wrapper allocations у
деяких сценаріях:

```kotlin
@JvmInline
value class UserId(val value: String)
```

Це корисно для type safety:

```kotlin
fun loadUser(id: UserId)
```

На JVM value class часто компілюється без окремого wrapper-а в простих випадках,
але boxing усе ще можливий при generics, nullable usage або interface calls.

8. **Data classes і copy**

`data class.copy()` створює новий обʼєкт:

```kotlin
val updated = user.copy(name = "Anna")
```

Це добре для immutable state, але треба розуміти allocation cost у hot paths.
Для UI state це зазвичай нормальний trade-off: immutability дає простоту і
безпечнішу модель.

9. **Strings і boxing**

Kotlin на JVM використовує Java `String`. String immutable, тому масові
конкатенації в циклі краще робити через `StringBuilder`:

```kotlin
val builder = StringBuilder()

items.forEach { item ->
    builder.append(item.name)
}

val result = builder.toString()
```

Також треба памʼятати про boxing primitives у generics:

```kotlin
val numbers: List<Int> = listOf(1, 2, 3)
```

`Int` у generic collection буде boxed як `Integer` на JVM.

10. **WeakReference**

`WeakReference` іноді використовують для cache або listener-сценаріїв:

```kotlin
val reference = WeakReference(activity)
```

Але це не універсальне рішення від leaks. Якщо потрібен `WeakReference`, часто
варто спочатку перевірити lifecycle-дизайн: можливо, обʼєкт просто живе не там,
де має.

11. **Профілювання**

Не треба оптимізувати памʼять "на око". Для JVM/Android треба використовувати:

- Android Studio Memory Profiler;
- heap dumps;
- LeakCanary для Android;
- allocation tracking;
- JVM profilers для backend.

Спочатку виміряти, потім оптимізувати.

12. **Практичне правило**

- Kotlin/JVM використовує JVM heap і garbage collector.
- Leak — це зазвичай зайве reachable reference.
- У Android не тримати `Activity`/`View` довше lifecycle.
- Не використовувати `GlobalScope` для UI-bound роботи.
- Великі collections і hot paths профілювати.
- Immutability часто варта додаткових allocations, але не в критичних loops.
- Оптимізацію робити після вимірювання.

Коротко: Kotlin сам не керує GC — це робить runtime платформи. На JVM і Android
памʼять очищає garbage collector, але якість коду визначає, чи стануть обʼєкти
недосяжними. Правильний lifecycle, контроль references і профілювання важливіші
за мікрооптимізації.

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

Коротко: null safety у Kotlin працює найкраще, коли типи чесно описують домен.
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

Коротко: `supervisorScope` змінює propagation помилок між siblings: одна
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

Коротко: backpressure у `Flow` — це контроль різниці швидкості між producer і
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

Коротко: Java Streams у Kotlin працюють, але не є основним інструментом. У
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

Коротко: KMM — це використання Kotlin Multiplatform для мобільної розробки, де
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

Коротко: головне обмеження Kotlin Multiplatform — він шарить не платформу, а
код, який можна коректно виконати на кількох платформах. Найкращий KMP-дизайн
тримає shared layer чистим і не намагається приховати реальні відмінності
Android та iOS.

</details>
<details>
<summary>80. Як ділитися бізнес-логікою у KMM?</summary>

#### Kotlin

У KMM бізнес-логікою діляться через shared Kotlin module, де живуть domain
models, use cases, validation, repository contracts, networking, serialization
та інша platform-independent логіка. Android і iOS підключають цей shared
module і використовують його з власного native UI.

1. **Що саме варто шарити**

У shared layer зазвичай виносять:

- domain models;
- use cases;
- validation rules;
- repositories або repository interfaces;
- networking client;
- DTO і mappers;
- serialization;
- business algorithms;
- feature flags contracts;
- analytics contracts;
- error mapping;
- state machines.

Приклад domain model:

```kotlin
data class User(
    val id: String,
    val name: String,
    val isPremium: Boolean
)
```

Ця модель може однаково використовуватись на Android і iOS.

2. **Use cases у commonMain**

Use case — хороший кандидат для shared-коду:

```kotlin
class LoadUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User {
        require(id.isNotBlank()) { "User id is required" }

        return repository.loadUser(id)
    }
}
```

Тут немає Android або iOS API, тому код можна тримати в `commonMain`.

3. **Repository contract у commonMain**

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
    fun observeUser(id: String): Flow<User>
}
```

Common-код залежить від contract, а не від platform implementation.

Реалізація може бути повністю shared, якщо використовує KMP-friendly libraries:

```kotlin
class RealUserRepository(
    private val api: UserApi
) : UserRepository {

    override suspend fun loadUser(id: String): User {
        return api.loadUser(id).toDomain()
    }

    override fun observeUser(id: String): Flow<User> {
        return flow {
            emit(loadUser(id))
        }
    }
}
```

4. **Networking у shared**

Для shared networking часто використовують Ktor:

```kotlin
class UserApi(
    private val client: HttpClient
) {
    suspend fun loadUser(id: String): UserDto {
        return client
            .get("https://api.example.com/users/$id")
            .body()
    }
}
```

DTO:

```kotlin
@Serializable
data class UserDto(
    val id: String,
    val name: String,
    val premium: Boolean
)
```

Mapper:

```kotlin
fun UserDto.toDomain(): User =
    User(
        id = id,
        name = name,
        isPremium = premium
    )
```

5. **Platform-specific реалізації через expect/actual**

Якщо логіці потрібна platform-specific можливість, її ховають за abstraction.

У `commonMain`:

```kotlin
expect class DeviceInfoProvider {
    fun deviceName(): String
}
```

В `androidMain`:

```kotlin
actual class DeviceInfoProvider {
    actual fun deviceName(): String {
        return Build.MODEL
    }
}
```

В `iosMain`:

```kotlin
actual class DeviceInfoProvider {
    actual fun deviceName(): String {
        return UIDevice.currentDevice.name
    }
}
```

Common business logic бачить один API, але implementation різна на кожній
платформі.

6. **Dependency injection у shared**

У KMM часто використовують manual DI або KMP-friendly DI:

```kotlin
class SharedDependencies(
    val loadUserUseCase: LoadUserUseCase
)

fun createSharedDependencies(): SharedDependencies {
    val client = createHttpClient()
    val api = UserApi(client)
    val repository = RealUserRepository(api)

    return SharedDependencies(
        loadUserUseCase = LoadUserUseCase(repository)
    )
}
```

Для складніших проєктів можна використовувати Koin або інші KMP-compatible
підходи, але manual DI часто простіший і прозоріший.

7. **Android side**

Android може використовувати shared use case у ViewModel:

```kotlin
class UserViewModel(
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

ViewModel і UI залишаються Android-specific, але business logic приходить зі
shared module.

8. **iOS side**

iOS може викликати shared API зі Swift:

```swift
let useCase = sharedDependencies.loadUserUseCase

Task {
    let user = try await useCase.invoke(id: "1")
    // update SwiftUI state
}
```

На практиці API для iOS часто роблять Swift-friendly через wrapper-и, щоб
сховати Kotlin-specific типи, `Flow` або складні generic types.

9. **Flow і state для iOS**

Якщо shared-код повертає `Flow`, iOS може потребувати adapter:

```kotlin
fun observeUser(id: String): Flow<User>
```

Для Swift можна зробити wrapper, який перетворює flow у callbacks або
AsyncSequence. Це краще, ніж змушувати iOS-команду напряму працювати з
Kotlin-specific stream abstractions.

10. **Не шарити UI state без потреби**

Можна зробити shared state machine:

```kotlin
sealed interface UserState {
    data object Loading : UserState
    data class Content(val user: User) : UserState
    data class Error(val message: String) : UserState
}
```

Але треба бути обережним: Android і iOS UI можуть мати різні UX requirements.
Якщо state model надто UI-specific, її краще тримати на платформі. Якщо це
спільна бізнес-state machine — можна винести в shared.

11. **Тестування shared logic**

Велика перевага KMM — common-тести:

```kotlin
class LoadUserUseCaseTest {

    @Test
    fun `loads user`() = runTest {
        val repository = FakeUserRepository(User("1", "John", false))
        val useCase = LoadUserUseCase(repository)

        val result = useCase("1")

        assertEquals("John", result.name)
    }
}
```

Один тест перевіряє логіку, яка використовується на Android і iOS.

12. **Практичне правило**

- Шарити domain/data/business logic.
- Platform API ховати за interfaces або `expect/actual`.
- UI і navigation залишати platform-specific, якщо немає сильної причини шарити.
- Shared API для iOS проектувати Swift-friendly.
- Не створювати надмірні abstractions заради 100% code sharing.
- Тестувати shared logic у common tests.

Коротко: у KMM бізнес-логіку ділять через shared module, де живе platform-free
Kotlin-код. Android і iOS мають власний UI, але використовують ті самі use cases,
models, validation, repositories і networking logic там, де це справді спільне.

</details>
<details>
<summary>81. Як оптимізувати продуктивність Kotlin-коду?</summary>

#### Kotlin

Оптимізація Kotlin-коду має починатися не з мікрооптимізацій, а з вимірювання.
На JVM і Android більшість проблем продуктивності повʼязані не з самим Kotlin,
а з неправильним lifecycle, зайвими allocation, blocking calls на main thread,
неефективними collection pipelines, неоптимальними coroutine dispatcher-ами або
надто частими оновленнями UI.

1. **Спочатку профілювати**

Не треба оптимізувати "на око":

```kotlin
val result = users.map { it.toUiModel() }
```

Можливо, цей код взагалі не є bottleneck. Спочатку треба виміряти:

- CPU time;
- allocations;
- GC pressure;
- main thread stalls;
- slow database queries;
- network latency;
- recomposition/rendering cost;
- cold start time.

Для Android використовують Android Studio Profiler, System Trace, Macrobenchmark,
Baseline Profiles, LeakCanary, allocation tracking.

2. **Не блокувати main thread**

Погано:

```kotlin
fun onButtonClick() {
    val user = api.loadUserBlocking()
    render(user)
}
```

На Android це може дати freeze або ANR.

Краще:

```kotlin
viewModelScope.launch {
    val user = withContext(Dispatchers.IO) {
        repository.loadUser()
    }

    state.value = UiState.Success(user)
}
```

I/O — на `Dispatchers.IO`, CPU-heavy робота — на `Dispatchers.Default`, UI — на
main thread.

3. **Правильно використовувати dispatcher-и**

Погано:

```kotlin
withContext(Dispatchers.Default) {
    api.loadUser()
}
```

Network або disk I/O краще виконувати на `Dispatchers.IO`:

```kotlin
withContext(Dispatchers.IO) {
    api.loadUser()
}
```

CPU-heavy мапінг, сортування, parsing великих обсягів:

```kotlin
withContext(Dispatchers.Default) {
    users.sortedBy { it.name }
}
```

Неправильний dispatcher може погіршити latency і throughput.

4. **Контролювати allocations у hot paths**

Kotlin collection operators зручні, але можуть створювати проміжні колекції:

```kotlin
val activeNames = users
    .map { it.name }
    .filter { it.isNotBlank() }
```

Для невеликих списків це нормально. Для великих collections або hot paths можна
використати `asSequence()`:

```kotlin
val activeNames = users
    .asSequence()
    .map { it.name }
    .filter { it.isNotBlank() }
    .toList()
```

Але `Sequence` не треба використовувати всюди: для малих списків overhead може
бути більшим за користь.

5. **Уникати зайвих object creation**

У tight loops зайві обʼєкти створюють GC pressure:

```kotlin
repeat(10_000) {
    val formatter = SimpleDateFormat("yyyy-MM-dd", Locale.US)
    formatter.format(Date())
}
```

Краще винести reusable dependency:

```kotlin
private val formatter = SimpleDateFormat("yyyy-MM-dd", Locale.US)
```

Але треба враховувати thread-safety. Наприклад, `SimpleDateFormat` не
thread-safe, тому в багатопоточному коді краще використовувати thread-safe API
або створювати formatter локально свідомо.

6. **inline functions**

`inline` може прибрати overhead lambda allocation і function call:

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}
```

Kotlin standard library активно використовує inline для функцій типу `let`,
`run`, `use`, `synchronized`.

Але не треба робити все `inline`. Це може збільшити bytecode size. `inline`
доречний для small higher-order functions, особливо в hot paths або public DSL.

7. **Value classes**

Value class може дати type safety без зайвого wrapper-а в частині JVM-сценаріїв:

```kotlin
@JvmInline
value class UserId(val value: String)
```

Це краще, ніж передавати всюди raw `String`:

```kotlin
fun loadUser(id: UserId)
```

Але boxing можливий у generics, nullable usage або interface calls. Тому value
classes — не магічне рішення, а інструмент для type safety з потенційним
performance benefit.

8. **String concatenation у циклах**

Погано:

```kotlin
var result = ""
items.forEach {
    result += it.name
}
```

Краще:

```kotlin
val result = buildString {
    items.forEach { item ->
        append(item.name)
    }
}
```

Або:

```kotlin
val result = items.joinToString(separator = "") { it.name }
```

9. **Оптимізувати Flow pipeline**

Якщо `Flow` емітить часто, треба зменшити зайву роботу:

```kotlin
queryFlow
    .debounce(300)
    .distinctUntilChanged()
    .flatMapLatest { query ->
        repository.search(query)
    }
```

Для UI state:

```kotlin
stateFlow
    .distinctUntilChanged()
    .collect { state ->
        render(state)
    }
```

Для повільного collector-а:

```kotlin
flow
    .conflate()
    .collect { latest ->
        render(latest)
    }
```

Вибір між `buffer`, `conflate`, `collectLatest`, `debounce` залежить від того,
чи можна втрачати проміжні значення.

10. **Не робити premature concurrency**

Погано:

```kotlin
coroutineScope {
    users.map { user ->
        async { mapper.map(user) }
    }.awaitAll()
}
```

Якщо `mapper.map(user)` дуже швидкий, overhead корутин буде більшим за користь.
Паралельність має сенс для реально дорогих незалежних задач.

Краще починати просто:

```kotlin
val result = users.map { mapper.map(it) }
```

І паралелити тільки після профілювання.

11. **Кешування**

Кешування може сильно допомогти, якщо результат дорогий і часто повторюється:

```kotlin
class UserMapper {
    private val cache = mutableMapOf<String, UserUiModel>()

    fun map(user: User): UserUiModel {
        return cache.getOrPut(user.id) {
            user.toUiModel()
        }
    }
}
```

Але cache треба обмежувати і чистити. Без eviction cache легко перетворюється на
memory leak.

12. **Android-specific продуктивність**

Для Android важливо:

- не робити I/O на main thread;
- уникати зайвих allocations у scrolling lists;
- використовувати stable keys у lazy lists;
- не передавати нестабільні lambdas/обʼєкти без потреби в Compose;
- оптимізувати startup;
- використовувати Baseline Profiles;
- не тримати зайві references на `Context`/`View`;
- профілювати recomposition.

Kotlin-оптимізація в Android часто насправді є UI/lifecycle/threading
оптимізацією.

13. **Практичне правило**

- Спочатку виміряти, потім оптимізувати.
- Main thread має бути легким.
- Dispatcher-и вибирати за типом роботи.
- У hot paths контролювати allocations.
- Collection pipelines оптимізувати тільки там, де це bottleneck.
- `Sequence`, `inline`, value classes і caching використовувати свідомо.
- Не додавати паралельність без доказу, що вона потрібна.

Коротко: продуктивний Kotlin-код — це не код без `map` чи `data class`, а код із
правильним threading, контрольованими allocations, виміряними bottleneck-ами і
простою архітектурою без зайвої concurrency та framework misuse.

</details>
<details>
<summary>82. Як реалізувати retry з exponential backoff у Kotlin?</summary>

#### Kotlin

Retry з exponential backoff — це повторення операції після помилки з паузою, яка
зростає після кожної невдалої спроби. Такий підхід зменшує навантаження на
сервер, дає системі час відновитись і не створює tight loop із миттєвими
повторами.

1. **Базова ідея**

Замість:

```kotlin
retry after 1 second
retry after 1 second
retry after 1 second
```

робимо:

```text
1s -> 2s -> 4s -> 8s -> max delay
```

Це і є exponential backoff.

2. **Проста suspend-функція retry**

```kotlin
suspend fun <T> retryWithBackoff(
    maxAttempts: Int = 3,
    initialDelayMillis: Long = 1_000,
    maxDelayMillis: Long = 10_000,
    factor: Double = 2.0,
    block: suspend () -> T
): T {
    var currentDelay = initialDelayMillis
    var lastError: Throwable? = null

    repeat(maxAttempts) { attempt ->
        try {
            return block()
        } catch (e: Throwable) {
            lastError = e

            if (attempt == maxAttempts - 1) {
                throw e
            }

            delay(currentDelay)
            currentDelay = (currentDelay * factor)
                .toLong()
                .coerceAtMost(maxDelayMillis)
        }
    }

    throw lastError ?: IllegalStateException("Retry failed")
}
```

Використання:

```kotlin
val user = retryWithBackoff {
    api.loadUser(id)
}
```

3. **Не retry-ити все підряд**

Не всі помилки треба повторювати.

Добрі кандидати для retry:

- timeout;
- temporary network failure;
- HTTP 429;
- HTTP 500/502/503/504;
- transient database/network errors.

Погані кандидати:

- HTTP 400;
- HTTP 401/403;
- validation error;
- business rule error;
- malformed request;
- not found, якщо це стабільний результат.

Тому потрібен predicate:

```kotlin
fun shouldRetry(error: Throwable): Boolean {
    return error is IOException ||
        error is TimeoutCancellationException ||
        error is ServerUnavailableException
}
```

4. **Retry тільки для потрібних помилок**

```kotlin
suspend fun <T> retryWithBackoff(
    maxAttempts: Int = 3,
    initialDelayMillis: Long = 1_000,
    maxDelayMillis: Long = 10_000,
    factor: Double = 2.0,
    shouldRetry: (Throwable) -> Boolean,
    block: suspend () -> T
): T {
    var currentDelay = initialDelayMillis

    repeat(maxAttempts) { attempt ->
        try {
            return block()
        } catch (e: Throwable) {
            if (e is CancellationException) {
                throw e
            }

            val isLastAttempt = attempt == maxAttempts - 1

            if (isLastAttempt || !shouldRetry(e)) {
                throw e
            }

            delay(currentDelay)
            currentDelay = (currentDelay * factor)
                .toLong()
                .coerceAtMost(maxDelayMillis)
        }
    }

    error("Unreachable")
}
```

Важливо: `CancellationException` треба перекидати, а не retry-ити. Cancellation
— це lifecycle signal, а не бізнес-помилка.

5. **Jitter**

Якщо багато клієнтів одночасно retry-ять із однаковими delay, вони можуть знову
одночасно вдарити по серверу. Для цього додають jitter — випадкове відхилення.

```kotlin
fun withJitter(delayMillis: Long, jitterRatio: Double = 0.2): Long {
    val jitter = (delayMillis * jitterRatio).toLong()
    val min = delayMillis - jitter
    val max = delayMillis + jitter

    return Random.nextLong(min, max + 1)
}
```

Використання:

```kotlin
delay(withJitter(currentDelay))
```

Jitter особливо важливий для backend/mobile clients у масштабі.

6. **Retry для Flow**

У `Flow` є оператор `retryWhen`:

```kotlin
repository.observeData()
    .retryWhen { cause, attempt ->
        if (attempt >= 3 || !shouldRetry(cause)) {
            false
        } else {
            val delayMillis = (1_000L * 2.0.pow(attempt.toDouble()))
                .toLong()
                .coerceAtMost(10_000)

            delay(delayMillis)
            true
        }
    }
    .catch { error ->
        emit(DataState.Error(error))
    }
```

`retryWhen` отримує exception і номер attempt. Якщо повернути `true`, flow
повторить upstream.

7. **Flow retry з jitter**

```kotlin
fun <T> Flow<T>.retryWithExponentialBackoff(
    maxAttempts: Long = 3,
    initialDelayMillis: Long = 1_000,
    maxDelayMillis: Long = 10_000,
    factor: Double = 2.0,
    shouldRetry: (Throwable) -> Boolean
): Flow<T> {
    return retryWhen { cause, attempt ->
        if (cause is CancellationException) {
            throw cause
        }

        if (attempt >= maxAttempts || !shouldRetry(cause)) {
            false
        } else {
            val delayMillis = (initialDelayMillis * factor.pow(attempt.toDouble()))
                .toLong()
                .coerceAtMost(maxDelayMillis)

            delay(withJitter(delayMillis))
            true
        }
    }
}
```

8. **Idempotency**

Retry безпечний не для всіх операцій. GET-запит зазвичай retry-friendly.
Payment або create-order операція — ні, якщо немає idempotency key.

Погано:

```kotlin
retryWithBackoff {
    paymentApi.charge(card, amount)
}
```

Якщо перший запит успішно списав гроші, але відповідь загубилася, retry може
списати повторно.

Краще:

```kotlin
paymentApi.charge(
    card = card,
    amount = amount,
    idempotencyKey = requestId
)
```

Retry треба проектувати разом із backend-контрактом.

9. **Timeout**

Retry треба комбінувати з timeout:

```kotlin
withTimeout(30_000) {
    retryWithBackoff(
        maxAttempts = 5,
        shouldRetry = ::shouldRetry
    ) {
        api.loadUser(id)
    }
}
```

Без загального timeout retry може занадто довго тримати operation alive.

10. **Практичне правило**

- Retry тільки transient errors.
- Не retry-ити `CancellationException`.
- Використовувати max attempts і max delay.
- Додавати jitter для масштабних клієнтів.
- Для небезпечних side effects потрібна idempotency.
- Для `Flow` використовувати `retryWhen`.
- Комбінувати retry із timeout і observability/logging.

Коротко: exponential backoff у Kotlin реалізується через `delay` між спробами,
де delay росте після кожної помилки. Якісний retry має обмеження спроб,
фільтрацію помилок, jitter, підтримку cancellation і розуміння idempotency.

</details>
<details>
<summary>83. Які нові можливості Kotlin варто знати у 2026 році?</summary>

#### Kotlin

Станом на червень 2026 року варто орієнтуватися на Kotlin 2.x лінійку: K2
compiler уже є базою сучасного Kotlin, Kotlin Multiplatform активно розвивається,
а релізи 2.3 і 2.4 принесли важливі зміни в language features, JVM, Native,
Wasm, Gradle tooling, standard library і Compose compiler.

1. **Kotlin 2.x і K2 compiler**

Найважливіший фундамент останніх років — K2 compiler. Для розробника це означає:

- кращу основу для майбутніх language features;
- швидшу і точнішу compiler analysis;
- кращу інтеграцію IDE/compiler;
- стабільніший розвиток Kotlin Multiplatform;
- поступове прибирання старих language-version режимів.

Практично: якщо проєкт досі живе на старій Kotlin 1.x лінійці, у 2026 році
треба планувати міграцію на Kotlin 2.x, бо нові можливості, tooling і ecosystem
будуть концентруватися там.

2. **Context parameters**

Context parameters — одна з найважливіших language features для DSL, dependency
passing і declarative APIs.

Ідея: функція може вимагати контекст, який доступний не як явний параметр
кожного виклику, а як contextual dependency.

Концептуально:

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving user $id")
}
```

Це корисно для:

- DSL;
- dependency scopes;
- logging/transaction context;
- typed contextual APIs;
- зменшення boilerplate у ланцюжках викликів.

Тут важливо не перетворити context parameters на прихований service locator.
Якщо dependency є критичною бізнес-залежністю класу, constructor injection усе
ще часто буде зрозумілішим.

3. **Explicit backing fields**

Explicit backing fields дають більше контролю над property storage:

```kotlin
var name: String = ""
    field = value.trim()
```

Це робить роботу з backing field більш явною і гнучкою, особливо для property
API, де треба контролювати читання/запис без зайвого private property.

Практична цінність:

- менше boilerplate;
- чистіші properties;
- краще вираження invariants;
- зручніше писати API з custom getter/setter.

4. **Data-flow-based exhaustiveness for when**

Kotlin продовжує покращувати аналіз `when`. Data-flow-based exhaustiveness
дозволяє compiler-у краще розуміти, які cases уже покриті.

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

Для Android/MVI/Compose це важливо, бо sealed state machines стають ще
безпечнішими й краще перевіряються compiler-ом.

5. **Unused return value checker**

У Kotlin 2.3 зʼявився механізм для виявлення невикористаних return values.
Це корисно, коли функція повертає результат, який не можна безпечно ігнорувати.

Проблема:

```kotlin
repository.saveUser(user)
```

Якщо `saveUser()` повертає `Result`, але caller його ігнорує, це може бути багом.

Практична ідея:

```kotlin
val result = repository.saveUser(user)

if (result.isFailure) {
    showError()
}
```

Цей напрямок важливий для більш безпечного API-дизайну: результат операції має
бути або явно використаний, або явно проігнорований.

6. **Java 25 target на JVM**

Kotlin 2.3 додав підтримку генерації bytecode для Java 25.

Для Android це не завжди прямо релевантно, бо Android має власні constraints.
Але для backend/JVM Kotlin це важливо:

- нові JVM capabilities;
- актуальна сумісність із modern Java;
- поступова еволюція JVM target-ів;
- краще позиціонування Kotlin у backend.

Практично: JVM-проєкти мають синхронно керувати Kotlin version, Java toolchain,
Gradle і runtime deployment target.

7. **Kotlin Multiplatform і Swift export**

У 2026 році KMP стає дедалі практичнішим, особливо для mobile. Важливий напрям —
Swift export і покращення Kotlin/Native interop.

Ціль проста: зробити shared Kotlin API природнішим для Swift.

Це важливо для KMM, бо iOS-команда не повинна відчувати, що працює з чужорідним
Kotlin API. Хороший shared module має мати Swift-friendly boundary:

```kotlin
class UserSdk(
    private val loadUser: LoadUserUseCase
) {
    suspend fun loadUser(id: String): User {
        return loadUser.invoke(id)
    }
}
```

Але складні `Flow`, sealed hierarchies і generics усе ще треба проектувати
обережно для Swift interop.

8. **Kotlin/Wasm**

Kotlin/Wasm помітно просунувся: web target став практичнішим, покращується JS
interop, exception handling, debugging і integration із web tooling.

Це важливо знати, навіть якщо ти Android-розробник, бо Kotlin ecosystem рухається
до ширшої multiplatform story:

```text
common Kotlin logic
    -> Android
    -> iOS
    -> JVM backend
    -> Web/Wasm
```

На співбесіді достатньо розуміти: Wasm — це не заміна Android, а ще один target
для shared Kotlin logic і потенційно UI/runtime сценаріїв.

9. **Standard library: UUID, time, Base64/Hex**

У Kotlin 2.x стабілізуються корисні standard library API:

- UUID у common stdlib;
- time tracking;
- Base64;
- HexFormat;
- покращення для multiplatform utility-коду.

Практична користь: менше platform-specific helper-ів і менше сторонніх
залежностей для базових речей.

Наприклад, у shared KMP-коді UUID/time API у standard library зменшують потребу
власних wrappers.

10. **Gradle і build tooling**

Kotlin 2.x також приносить Gradle-related покращення:

- compatibility з новими Gradle версіями;
- кращі APIs для generated sources;
- binary compatibility validation у Kotlin Gradle plugin;
- покращення KMP dependency declarations;
- швидші Native release tasks.

Для техліда це важливо не менше за language syntax. Поганий build setup може
зʼїсти більше часу команди, ніж відсутність нової мовної фічі.

11. **Compose compiler і diagnostics**

Compose compiler у Kotlin 2.x став частиною Kotlin ecosystem, а не окремою
напіввідокремленою історією.

Важливий напрям — кращі stack traces і diagnostics для minified Android apps.
Для production Compose-додатків це практично корисно: легше розуміти crashes,
які сталися під час composition, measure або draw.

12. **Що реально треба знати Android/Kotlin розробнику**

У 2026 році я б очікував від сильного Kotlin-розробника розуміння:

- Kotlin 2.x/K2 як baseline;
- context parameters і де вони доречні;
- explicit backing fields;
- sealed state + exhaustive `when`;
- KMP/KMM і Swift interop;
- Kotlin/Wasm на концептуальному рівні;
- stdlib improvements для UUID/time/Base64;
- Gradle/version catalog/toolchain discipline;
- Compose compiler changes, якщо розробник працює з Compose;
- migration risks при оновленні Kotlin.

13. **Практичне правило**

Не треба тягнути кожну нову фічу в production одразу. Нові можливості треба
оцінювати через:

- stability level;
- IDE support;
- Gradle/AGP compatibility;
- Android Studio support;
- team readiness;
- library ecosystem;
- migration cost;
- production value.

Коротко: у 2026 році Kotlin — це вже не тільки "зручніший Java для Android".
Це Kotlin 2.x з K2 compiler, активним Multiplatform, покращеним Swift/Wasm
напрямом, сильнішим tooling і новими language features для безпечнішого та
виразнішого API-дизайну.

</details>
<details>
<summary>84. Що таке життєвий цикл Activity?</summary>

#### Kotlin

Життєвий цикл `Activity` — це набір callback-ів, через які Android повідомляє,
що екран створюється, стає видимим, переходить у foreground, втрачає фокус,
зупиняється або знищується. Розуміння lifecycle критичне для Android: від нього
залежать ресурси, підписки, корутини, збереження стану, memory leaks і поведінка
при rotation/configuration changes.

1. **Основні callback-и**

Типовий lifecycle:

```text
onCreate()
onStart()
onResume()
onPause()
onStop()
onDestroy()
```

Також є:

```text
onRestart()
onSaveInstanceState()
onRestoreInstanceState()
```

2. **onCreate**

`onCreate()` викликається, коли `Activity` створюється:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Тут зазвичай роблять:

- initial setup;
- inflate layout або `setContent`;
- init ViewBinding;
- setup toolbar/navigation;
- підписки, які мають жити весь lifecycle Activity;
- restore state з `savedInstanceState`.

Важливо: `onCreate()` може викликатися повторно після rotation або process
recreation.

3. **onStart**

`onStart()` означає, що `Activity` стає видимою:

```kotlin
override fun onStart() {
    super.onStart()
}
```

Тут можна стартувати логіку, яка потрібна, поки екран видимий. Але для більшості
сучасних сценаріїв краще використовувати lifecycle-aware APIs:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

4. **onResume**

`onResume()` означає, що `Activity` у foreground і користувач може взаємодіяти з
нею:

```kotlin
override fun onResume() {
    super.onResume()
}
```

Тут доречно відновлювати речі, які потрібні тільки при активній взаємодії:

- camera preview;
- sensors;
- location updates;
- foreground-only listeners;
- animation/game loop.

5. **onPause**

`onPause()` викликається, коли `Activity` частково втрачає foreground:

```kotlin
override fun onPause() {
    super.onPause()
}
```

Тут треба швидко зупиняти легкі foreground-only операції. Не варто робити довгі
blocking-збереження в `onPause()`, бо це може сповільнити перехід до іншого
екрана.

6. **onStop**

`onStop()` означає, що `Activity` більше не видима:

```kotlin
override fun onStop() {
    super.onStop()
}
```

Тут зупиняють важчі ресурси, які не потрібні, коли екран невидимий:

- receivers/listeners;
- camera;
- location;
- expensive rendering;
- UI-related collection, якщо вона не lifecycle-aware.

7. **onDestroy**

`onDestroy()` викликається перед знищенням `Activity`:

```kotlin
override fun onDestroy() {
    super.onDestroy()
}
```

Але важливий нюанс: `onDestroy()` не завжди означає "користувач закрив екран
назавжди". Він може бути викликаний через configuration change, наприклад
rotation.

```kotlin
if (isFinishing) {
    // Activity реально завершується
}
```

Для бізнес-стану краще використовувати `ViewModel`, а не покладатися на
`onDestroy()`.

8. **Configuration changes**

При rotation Android зазвичай знищує і створює `Activity` заново:

```text
onPause()
onStop()
onDestroy()
onCreate()
onStart()
onResume()
```

Тому не можна зберігати важливий state тільки в полях `Activity`:

```kotlin
private var selectedUserId: String? = null
```

Після recreation це поле буде втрачено, якщо його не зберегти.

Краще:

- `ViewModel` для UI/business state;
- `savedInstanceState` для small transient UI state;
- database/cache для persistent state.

9. **onSaveInstanceState**

`onSaveInstanceState()` використовується для збереження невеликого UI state:

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putString("selected_user_id", selectedUserId)
}
```

Відновлення:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    selectedUserId = savedInstanceState?.getString("selected_user_id")
}
```

Не треба класти в `Bundle` великі обʼєкти, списки або bitmap-и.

10. **ViewModel і lifecycle**

`ViewModel` переживає configuration changes:

```kotlin
class UserViewModel : ViewModel() {
    val state = MutableStateFlow<UiState>(UiState.Loading)
}
```

`Activity` може бути пересоздана, а `ViewModel` залишиться тією самою, доки
екран логічно існує.

Це правильне місце для:

- screen state;
- async loading;
- business interaction;
- coroutine jobs через `viewModelScope`.

11. **Lifecycle-aware collection**

Погано:

```kotlin
lifecycleScope.launch {
    viewModel.state.collect { state ->
        render(state)
    }
}
```

Такий collection може продовжити роботу, навіть коли UI не в потрібному стані.

Краще:

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect { state ->
            render(state)
        }
    }
}
```

`repeatOnLifecycle` автоматично стартує і скасовує collection відповідно до
lifecycle.

12. **Типові помилки**

- Робити heavy work у `onCreate()` або на main thread.
- Не враховувати recreation при rotation.
- Зберігати business state тільки в `Activity`.
- Не відписувати listeners.
- Тримати references на `Activity` у singleton-ах.
- Запускати корутини не в lifecycle-aware scope.
- Покладатися на `onDestroy()` як на гарантований cleanup для всього.

13. **Практичне правило**

- `onCreate` — initial setup.
- `onStart` — Activity видима.
- `onResume` — Activity активна для interaction.
- `onPause` — швидко призупинити foreground-only речі.
- `onStop` — звільнити ресурси, коли екран невидимий.
- `onDestroy` — cleanup, але не місце для критичного business state.
- `ViewModel` — для state, який має пережити configuration changes.

Коротко: lifecycle `Activity` — це контракт між Android system і екраном.
Сильний Android-код не просто перевизначає callback-и, а привʼязує state,
resources, coroutines і subscriptions до правильного lifecycle-рівня.

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

Коротко: Activity — це повноцінний Android component, а Fragment — модульна
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

Коротко: `Intent` — це повідомлення-наміp для Android system або іншого
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

Коротко: якщо не викликати `super.onCreate()`, Android base class не виконає
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

Коротко: View Binding замінює ручний `findViewById` type-safe API, який
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

Коротко: `LiveData` — це Android lifecycle-aware observable holder. Вона
зручна для класичного Android UI, автоматично враховує lifecycle і доставляє
останнє значення active observers, але для сучасної coroutine-first архітектури
часто поступається `Flow` і `StateFlow`.

</details>
<details>
<summary>90. Що таке Room і як він працює?</summary>

#### Kotlin

`Room` — це Jetpack-бібліотека для роботи з SQLite в Android. Вона дає
type-safe abstraction над SQLite: замість ручного `Cursor`, SQL string parsing і
boilerplate, розробник описує `Entity`, `Dao` і `Database`, а Room генерує
реалізацію, перевіряє SQL на compile time і інтегрується з `Flow`, `LiveData`
та coroutines.

1. **Навіщо потрібен Room**

SQLite є в Android з коробки, але raw SQLite API низькорівневий:

- багато boilerplate;
- ручна робота з `Cursor`;
- SQL-помилки часто знаходяться в runtime;
- складніше тестувати;
- легко помилитися з column names/types.

Room вирішує це через compile-time перевірки і annotation-based API.

2. **Основні частини Room**

Room складається з трьох ключових елементів:

```text
Entity   -> таблиця
Dao      -> SQL operations
Database -> точка доступу до БД
```

3. **Entity**

`Entity` описує таблицю:

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val email: String
)
```

Room створить таблицю `users` із колонками `id`, `name`, `email`.

Можна задавати індекси:

```kotlin
@Entity(
    tableName = "users",
    indices = [Index(value = ["email"], unique = true)]
)
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val email: String
)
```

4. **Dao**

DAO описує операції з базою:

```kotlin
@Dao
interface UserDao {

    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Query("SELECT * FROM users")
    fun observeUsers(): Flow<List<UserEntity>>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertUser(user: UserEntity)

    @Delete
    suspend fun deleteUser(user: UserEntity)
}
```

Room генерує implementation цього interface.

5. **Database**

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1,
    exportSchema = true
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Створення:

```kotlin
val database = Room.databaseBuilder(
    context.applicationContext,
    AppDatabase::class.java,
    "app.db"
).build()
```

У production це зазвичай створюється через DI container, наприклад Hilt.

6. **Room і Flow**

Room може повертати `Flow`:

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

Коли таблиця `users` зміниться, Room автоматично invalidates query і емiтить
новий список.

Це дуже зручно для offline-first архітектури:

```kotlin
repository.observeUsers()
    .map { entities -> entities.map { it.toDomain() } }
```

UI отримує updates з локальної БД без ручного polling.

7. **Suspend queries**

Для one-shot операцій використовують `suspend`:

```kotlin
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUser(id: String): UserEntity?
```

Room виконає query асинхронно. Не треба запускати database operation на main
thread вручну.

Важливо: не використовувати `allowMainThreadQueries()` у production.

8. **Relations**

Room підтримує relations через data classes:

```kotlin
data class UserWithOrders(
    @Embedded val user: UserEntity,
    @Relation(
        parentColumn = "id",
        entityColumn = "userId"
    )
    val orders: List<OrderEntity>
)
```

DAO:

```kotlin
@Transaction
@Query("SELECT * FROM users WHERE id = :id")
suspend fun getUserWithOrders(id: String): UserWithOrders
```

`@Transaction` важливий, щоб Room виконав повʼязані queries консистентно.

9. **Migrations**

Коли schema змінюється, треба migration:

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

Без migration користувач може втратити дані або застосунок впаде при оновленні.

10. **TypeConverters**

SQLite не знає складних Kotlin-типів напряму. Для них використовують
`TypeConverter`:

```kotlin
class Converters {

    @TypeConverter
    fun fromInstant(value: Instant?): Long? {
        return value?.toEpochMilli()
    }

    @TypeConverter
    fun toInstant(value: Long?): Instant? {
        return value?.let { Instant.ofEpochMilli(it) }
    }
}
```

Підключення:

```kotlin
@TypeConverters(Converters::class)
@Database(entities = [UserEntity::class], version = 1)
abstract class AppDatabase : RoomDatabase()
```

11. **Repository pattern**

DAO не варто напряму тягнути в UI. Краще мати repository:

```kotlin
class UserRepository(
    private val userDao: UserDao,
    private val api: UserApi
) {
    fun observeUsers(): Flow<List<User>> {
        return userDao.observeUsers()
            .map { entities -> entities.map { it.toDomain() } }
    }

    suspend fun refreshUsers() {
        val users = api.loadUsers()
        userDao.insertUsers(users.map { it.toEntity() })
    }
}
```

Room стає local source of truth, а repository координує network/cache logic.

12. **Типові помилки**

- Використовувати `allowMainThreadQueries()` у production.
- Не писати migrations.
- Зберігати domain models напряму як entities без обдуманої межі.
- Робити надто складні relations без профілювання.
- Не індексувати поля, по яких часто фільтрують.
- Повертати `LiveData`/`Flow` з DAO і трансформувати все прямо в UI.
- Не тестувати migrations.

13. **Практичне правило**

- `Entity` — структура таблиці.
- `Dao` — SQL operations.
- `RoomDatabase` — entry point до БД.
- `Flow` з DAO — для observing changes.
- `suspend` DAO methods — для one-shot operations.
- Migrations — обовʼязкові при зміні schema.
- Repository має ховати Room від UI/domain layer.

Коротко: Room — це type-safe шар над SQLite, який генерує database code,
перевіряє SQL на compile time і добре інтегрується з coroutines та Flow. У
сучасній Android-архітектурі Room часто є локальним source of truth для
offline-first data layer.

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

Коротко: ці анотації описують Room, яку database operation треба згенерувати.
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

Коротко: WorkManager — це стандартний Android-інструмент для надійної
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

Коротко: `CoroutineScope` — загальний механізм керування корутинами,
`lifecycleScope` привʼязаний до Android lifecycle, а `viewModelScope` — до
ViewModel. Правильний вибір scope визначає, коли корутина буде скасована і чи не
створить вона leak або роботу після смерті UI.

</details>
<details>
<summary>94. Як реалізується dependency injection у Kotlin?</summary>

#### Kotlin

Dependency Injection, або DI, — це підхід, коли клас не створює свої залежності
сам, а отримує їх ззовні. У Kotlin це найчастіше реалізують через constructor
injection, manual DI або DI-фреймворки на кшталт Hilt, Dagger чи Koin. Основна
мета — зробити код тестованим, слабко звʼязаним і керованим.

1. **Проблема без DI**

Погано:

```kotlin
class UserRepository {
    private val api = Retrofit.Builder()
        .baseUrl("https://example.com")
        .build()
        .create(UserApi::class.java)
}
```

Клас сам створює dependency. Наслідки:

- важко тестувати;
- важко замінити implementation;
- клас знає забагато про створення обʼєктів;
- складно конфігурувати середовища;
- зʼявляється сильне coupling.

2. **Constructor Injection**

Краще:

```kotlin
class UserRepository(
    private val api: UserApi
) {
    suspend fun loadUser(id: String): User {
        return api.loadUser(id).toDomain()
    }
}
```

Тепер `UserRepository` не знає, як створюється `UserApi`. Він просто отримує
готову залежність.

У тесті:

```kotlin
val repository = UserRepository(
    api = FakeUserApi()
)
```

Це найпростіший і найважливіший вид DI.

3. **Залежати від interface**

```kotlin
interface UserApi {
    suspend fun loadUser(id: String): UserDto
}

class RealUserApi : UserApi {
    override suspend fun loadUser(id: String): UserDto {
        // network call
    }
}
```

Repository:

```kotlin
class UserRepository(
    private val api: UserApi
)
```

У production передаємо `RealUserApi`, у тесті — `FakeUserApi`.

4. **Manual DI**

Для невеликих проєктів manual DI може бути достатнім:

```kotlin
class AppContainer(
    private val context: Context
) {
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://example.com")
        .build()

    private val userApi: UserApi =
        retrofit.create(UserApi::class.java)

    val userRepository: UserRepository =
        UserRepository(userApi)

    val loadUserUseCase: LoadUserUseCase =
        LoadUserUseCase(userRepository)
}
```

Application:

```kotlin
class App : Application() {
    lateinit var container: AppContainer

    override fun onCreate() {
        super.onCreate()
        container = AppContainer(this)
    }
}
```

Manual DI прозорий, але в великих проєктах швидко росте boilerplate.

5. **Hilt/Dagger**

Hilt — стандартний DI-фреймворк для Android поверх Dagger:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity()
```

ViewModel:

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val loadUser: LoadUserUseCase
) : ViewModel()
```

Use case:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
)
```

Hilt генерує dependency graph на compile time і сам створює потрібні обʼєкти.

6. **Modules**

Якщо Hilt не може сам створити dependency, використовують module:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    @Singleton
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://example.com")
            .build()
    }

    @Provides
    fun provideUserApi(retrofit: Retrofit): UserApi {
        return retrofit.create(UserApi::class.java)
    }
}
```

`@Provides` каже Hilt, як створити обʼєкт.

7. **Binding interface до implementation**

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
}

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

`@Binds` використовується, коли треба сказати: "для interface використовуй цю
implementation".

8. **Scopes**

DI також керує lifecycle обʼєктів:

```kotlin
@Singleton
class UserSession @Inject constructor()
```

`@Singleton` означає один instance у межах application graph.

В Android/Hilt є різні scopes:

- `SingletonComponent`;
- `ActivityRetainedComponent`;
- `ViewModelComponent`;
- `ActivityComponent`;
- `FragmentComponent`.

Scope має відповідати lifecycle dependency. Не треба робити все singleton.

9. **Koin**

Koin — runtime DI/service locator style library:

```kotlin
val appModule = module {
    single<UserApi> { RealUserApi() }
    single<UserRepository> { RealUserRepository(get()) }
    factory { LoadUserUseCase(get()) }
}
```

Переваги:

- простіший старт;
- менше generated code;
- Kotlin DSL.

Недоліки:

- помилки dependency graph частіше runtime;
- менше compile-time safety, ніж у Dagger/Hilt.

10. **DI і тестування**

DI дозволяє легко замінити залежності:

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun loadUser(id: String): User {
        return User(id, "John")
    }
}
```

Тест:

```kotlin
val useCase = LoadUserUseCase(
    repository = FakeUserRepository()
)
```

Без DI довелося б мокати внутрішні створення або чіпати framework.

11. **Типові помилки**

- Створювати dependency всередині класу замість constructor injection.
- Використовувати service locator як глобальний singleton без контролю.
- Робити всі обʼєкти `@Singleton`.
- Інжектити Android `Context` там, де достатньо application context або взагалі
  не потрібен context.
- Інжектити implementation замість interface без потреби.
- Ховати business dependencies у context parameters/service locator.
- Будувати занадто складний graph без модульності.

12. **Практичне правило**

- Починати з constructor injection.
- Залежати від interface на межах модулів.
- Для малих проєктів manual DI може бути достатнім.
- Для Android production-проєктів часто використовують Hilt.
- Scope dependency має відповідати її lifecycle.
- DI потрібен для явності й тестованості, а не для магії.

Коротко: dependency injection у Kotlin найкраще починати з простого constructor
injection. DI-фреймворки типу Hilt лише автоматизують створення graph-а, але не
замінюють правильний дизайн залежностей, interface boundaries і lifecycle
мислення.

</details>
<details>
<summary>95. Що таке Hilt і як він працює?</summary>

#### Kotlin

`Hilt` — це офіційний Jetpack DI-фреймворк для Android, побудований поверх
Dagger. Він спрощує dependency injection в Android: створює dependency graph,
інтегрується з `Application`, `Activity`, `Fragment`, `ViewModel`, `Service`,
Worker і автоматично керує scopes відповідно до Android lifecycle.

1. **Навіщо потрібен Hilt**

Без DI Android-код швидко стає жорстко звʼязаним:

```kotlin
class UserViewModel : ViewModel() {
    private val repository = RealUserRepository(RetrofitUserApi())
}
```

Такий код важко тестувати і змінювати.

З Hilt:

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val repository: UserRepository
) : ViewModel()
```

ViewModel отримує dependency з graph-а, а не створює її сама.

2. **@HiltAndroidApp**

Hilt стартує з `Application`:

```kotlin
@HiltAndroidApp
class App : Application()
```

Ця анотація генерує base dependency graph для застосунку. Без неї Hilt не буде
знати, з чого починати створення graph-а.

3. **@AndroidEntryPoint**

Android components, у які Hilt має інжектити dependencies, позначаються:

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity()
```

Fragment:

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user)
```

Це дає Hilt можливість підключити generated component до Android lifecycle.

4. **@Inject constructor**

Якщо клас можна створити напряму, ставимо `@Inject constructor`:

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
)
```

Hilt бачить constructor і розуміє, як створити `LoadUserUseCase`, якщо знає, як
створити `UserRepository`.

5. **Modules і @Provides**

Якщо dependency не можна створити через constructor, використовують module:

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    @Singleton
    fun provideRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl("https://example.com")
            .build()
    }

    @Provides
    fun provideUserApi(retrofit: Retrofit): UserApi {
        return retrofit.create(UserApi::class.java)
    }
}
```

`@Provides` — функція, яка каже Hilt, як створити dependency.

6. **@Binds**

Якщо є interface і implementation:

```kotlin
interface UserRepository {
    suspend fun loadUser(id: String): User
}

class RealUserRepository @Inject constructor(
    private val api: UserApi
) : UserRepository
```

Потрібно сказати Hilt, що для `UserRepository` треба використовувати
`RealUserRepository`:

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

`@Binds` зазвичай краще за `@Provides`, коли треба просто привʼязати interface
до implementation.

7. **Scopes**

Hilt має components, які відповідають Android lifecycle:

```text
SingletonComponent        -> Application
ActivityRetainedComponent -> survives configuration changes
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Приклад singleton:

```kotlin
@Provides
@Singleton
fun provideDatabase(@ApplicationContext context: Context): AppDatabase {
    return Room.databaseBuilder(
        context,
        AppDatabase::class.java,
        "app.db"
    ).build()
}
```

Scope означає, скільки живе instance і де він reused.

8. **ViewModel з Hilt**

```kotlin
@HiltViewModel
class UserViewModel @Inject constructor(
    private val loadUser: LoadUserUseCase
) : ViewModel() {

    fun load(id: String) {
        viewModelScope.launch {
            loadUser(id)
        }
    }
}
```

У Fragment:

```kotlin
private val viewModel: UserViewModel by viewModels()
```

Hilt інтегрується з `ViewModelProvider.Factory` і створює ViewModel з потрібними
dependencies.

9. **Qualifiers**

Якщо є кілька обʼєктів одного типу, використовують qualifiers:

```kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class AuthRetrofit

@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class PublicRetrofit
```

Module:

```kotlin
@Provides
@AuthRetrofit
fun provideAuthRetrofit(): Retrofit = TODO()

@Provides
@PublicRetrofit
fun providePublicRetrofit(): Retrofit = TODO()
```

Інʼєкція:

```kotlin
class AuthApi @Inject constructor(
    @AuthRetrofit private val retrofit: Retrofit
)
```

10. **Context qualifiers**

Для Android context Hilt дає готові qualifiers:

```kotlin
class ImageLoader @Inject constructor(
    @ApplicationContext private val context: Context
)
```

або:

```kotlin
class ScreenTracker @Inject constructor(
    @ActivityContext private val context: Context
)
```

`ApplicationContext` безпечніший для long-lived dependencies. `ActivityContext`
не можна тримати в singleton.

11. **Як Hilt працює під капотом**

На високому рівні:

```text
annotations -> generated Dagger components -> dependency graph -> injection
```

Hilt через annotation processing/code generation створює Dagger graph. Якщо
dependency graph некоректний, помилка зазвичай буде на compile time, а не в
runtime.

Це сильна перевага над runtime service locator-підходами.

12. **Типові помилки**

- Забути `@HiltAndroidApp`.
- Забути `@AndroidEntryPoint` на Activity/Fragment.
- Інжектити `ActivityContext` у singleton.
- Не зробити binding interface до implementation.
- Використовувати `@Singleton` для всього.
- Створювати залежності вручну там, де їх має створювати Hilt.
- Плутати Hilt scopes і Android lifecycle.

13. **Практичне правило**

- `@HiltAndroidApp` — на `Application`.
- `@AndroidEntryPoint` — на Android component, який отримує injection.
- `@Inject constructor` — для класів, які Hilt може створити сам.
- `@Provides` — для third-party або builder-based dependencies.
- `@Binds` — для interface -> implementation.
- Scope має відповідати lifecycle dependency.

Коротко: Hilt — це Android-friendly обгортка над Dagger, яка генерує dependency
graph, інтегрується з Android lifecycle і дозволяє отримувати залежності через
constructor injection замість ручного створення обʼєктів.

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

Коротко: `@HiltViewModel` дозволяє Hilt створювати `ViewModel` через generated
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

Коротко: `@Inject` показує Hilt/Dagger, як створити обʼєкт або куди передати
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

Коротко: scopes у Hilt керують lifecycle і reuse dependencies. Правильний scope
захищає від зайвого створення обʼєктів і memory leaks, неправильний — навпаки
може привʼязати Activity/Fragment до довгоживучого graph-а.

</details>
99. Як уникати витоків памʼяті в Android?
100.  Як безпечно зберігати API-ключі в Android-додатку?
101.  Як реалізувати offline-first архітектуру?
102.  Що таке архітектура MVI і коли її використовувати?
103.  Як проєктувати масштабовані Android-додатки?
104.  Що таке Jetpack Compose і чим він відрізняється від View System?
105.  Що таке @Composable і що таке recomposition?
106.  Що таке state hoisting?
107.  У чому різниця між remember і rememberSaveable?
108.  Що таке rememberCoroutineScope?
109.  Що таке collectAsState?
110.  Що таке Modifier у Compose?
111.  У чому різниця між padding і offset?
112.  У чому різниця між fillMaxSize і matchParentSize?
113.  Як обробляти кнопку “назад” у Compose?
114.  Для чого використовується rememberUpdatedState?
115.  Що таке side-effects у Compose і як їх обробляти?
116.  Що таке ключі (keys) у LaunchedEffect і чому вони важливі?
117.  Що таке snapshotFlow?
118.  Як оптимізувати recomposition у Compose?
119.  Як дебажити проблеми з recomposition?
120.  Які є способи уникнути лагів у Compose, наприклад під час скролу?
121.  Чим відрізняються анотації Stable та Immutable?
122.  Навіщо передавати key у LazyColumn і що він собою являє під капотом?
123.  Які ще параметри можна передати в LazyColumn items() і для чого
      використовується contentType?
124.  Що таке SOLID?
125.  Поясніть кожен принцип SOLID з прикладами з Android.
126.  Що таке ООП?
127.  Що таке абстракція?
128.  У чому різниця між конструкторами та методами?
129.  Що таке сигнатура методу?
130.  У чому різниця між перевантаженням (overloading) та перевизначенням
      (overriding)?
131.  У чому різниця між інтерфейсом та абстрактним класом?
132.  Чи може інтерфейс не містити методів?
133.  Чи може інтерфейс містити інший інтерфейс?
134.  Чому не варто додавати в інтерфейс десятки методів?
135.  Що таке композиція і чим вона відрізняється від наслідування?
136.  Які є модифікатори доступу в Kotlin?
137.  Що таке singleton?
138.  Що таке generics?
139.  Що таке type erasure і чому він виникає?
140.  Які існують колекції: List, Set, Map, Queue, Stack?
141.  Що таке потокобезпечність колекцій?
142.  У чому різниця між final, finally та finalize?
143.  Як працює try-catch-finally?
144.  У чому різниця між checked та unchecked exceptions?
145.  Які існують типи помилок у Kotlin/Java?
146.  Який базовий клас для помилок?
147.  Що таке складність алгоритмів?
148.  Що таке пошук у глибину (DFS)?
149.  Що таке пошук у ширину (BFS)?
150.  Розкажіть про Clean Architecture.
151.  Що таке MVVM?
152.  У чому різниця між MVVM та MVI?
153.  Які архітектурні патерни використовуються в Android?
154.  Як організувати модулі в Android-проєкті?
155.  Що таке токен?
156.  У чому різниця між authentication та authorization?
157.  Які інструменти використовуються для комунікації з бекендом?
158.  Що таке Retrofit?
159.  Що таке Apollo GraphQL?
160.  Для чого використовується Firebase?
161.  Що таке Android Keystore?
162.  Які є dispatchers у Kotlin Coroutines і для чого вони використовуються?
163.  Які є способи запуску корутин?
164.  У чому різниця між launch та async?
165.  Як обробляти помилки в корутинах?
166.  Як дочекатися результату декількох паралельних запитів?
167.  Для чого були придумані Fragment?
168.  Які проблеми вони вирішують?
169.  Які особливості вкладених Fragment?
170.  Які є виклики (callbacks) у Application?
171.  Який singleton створює система під час запуску Android-додатка?
172.  Як працює BackStack?
173.  У яких випадках можна отримати ANR (Application Not Responding)?
174.  Що можна виконувати в main thread?
175.  Які є базові компоненти Android?
176.  Що таке Service?
177.  Що таке BroadcastReceiver?
178.  Що таке Context і які його типи існують?
179.  Що таке Bundle?
180.  Чи можна вкладати Bundle у Bundle?
181.  Що таке Permissions в Android?
182.  Що таке setContentView?
183.  Які методи існують у View?
184.  Що таке ViewGroup?
185.  Для чого потрібен метод onStart() в Activity?
186.  Як можна потрапити в onStart(), але не потрапити в onResume()?
187.  Що таке міграції в Room?
188.  Що робить анотація Embedded у Room?
189.  Що таке DatabaseView у Room?
190.  У чому різниця між SQL та NoSQL?
191.  Що таке DataStore?
192.  Що таке Paging 3?
193.  Що таке RecyclerView.Adapter?
194.  Які основні методи має RecyclerView.Adapter?
195.  У чому різниця між RecyclerView.Adapter та ListAdapter?
196.  Як працює RecyclerView під капотом?
197.  Що таке DiffUtil?
198.  Як відобразити списки в Android?
199.  Що таке lazy-контейнери в Compose?
200.  Як реалізувати циклічний список у RecyclerView?
201.  Що таке анімації в Android?
202.  У чому різниця між ViewBinding та DataBinding?
203.  Що таке Serializable?
204.  Які є способи серіалізації в Android?
205.  Що робити, якщо поле може бути відсутнім у відповіді API?
206.  Що таке Dependency Injection?
207.  У чому різниця між Dagger/Hilt та Koin?
208.  Що таке CompositionLocal?
209.  Які бувають CompositionLocal?
210.  Як працює CompositionLocal під капотом?
211.  Що таке side effects у Compose?
212.  Як оптимізувати recomposition?
213.  Як написати асинхронний тест без використання корутин?
214.  У чому різниця між inline, noinline та crossinline?
215.  Які переваги Kotlin над Java?
216.  У чому різниця між val та const val?
217.  Які типи можна використовувати з const val?
218.  Чи можна створити data class без параметрів?
219.  У чому різниця між sealed class та enum?
220.  Що таке WorkManager?
221.  Коли варто використовувати WorkManager?
222.  Чи працювали ви з Android Media3 / ExoPlayer?
223.  Як би ви розподілили компоненти по модулях, якщо є кнопка, яка по кліку
      завантажує дані?
224.  Які є способи уникнути лагів у Compose, наприклад під час скролу?
225.  Чим відрізняються анотації Stable та Immutable?
226.  Навіщо передавати key у LazyColumn і що він собою являє під капотом?
227.  Які ще параметри можна передати в LazyColumn items() і для чого
      використовується contentType?
