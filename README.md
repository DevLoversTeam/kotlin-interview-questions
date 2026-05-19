Kotlin / Android / Compose — питання для співбесід по рівнях

⸻

1. Kotlin Core

Junior

Основи мови

* **Що таке Kotlin і як він взаємодіє з Java?**
  Kotlin — сучасна statically typed мова від JetBrains, яка найчастіше використовується для Android, backend-розробки та Multiplatform-проєктів. Вона компілюється в JVM bytecode, тому добре працює в екосистемі Java.

  Kotlin повністю сумісний з Java: можна викликати Java-класи з Kotlin і навпаки. Завдяки цьому Kotlin легко додавати в існуючі Java-проєкти без повного переписування коду.

  ```kotlin
  val list = java.util.ArrayList<String>()
  list.add("Hello")
  ```
* **Які базові типи даних існують у Kotlin?**
  Основні типи в Kotlin: `Int`, `Long`, `Short`, `Byte`, `Float`, `Double`, `Boolean`, `Char`, `String`. Вони використовуються для зберігання чисел, логічних значень, символів і тексту.

  На рівні мови Kotlin працює з ними як з повноцінними типами, а на JVM компілятор оптимізує їх до primitive type там, де це можливо. Це дає зручний синтаксис без втрати продуктивності.
* **У чому різниця між val і var у Kotlin?**
  `val` означає, що посилання не можна переприсвоїти після ініціалізації. Це аналог read-only змінної, і його варто використовувати за замовчуванням.

  `var` дозволяє змінювати значення змінної в процесі виконання програми. Тобто `val` — для незмінних посилань, `var` — для змінних даних.

  ```kotlin
  val name = "Ivan"
  var age = 20
  age = 21
  ```
* **Що таке виведення типів (type inference) у Kotlin?**
  Type inference — це механізм, коли компілятор сам визначає тип змінної, параметра або результату виразу з контексту. Через це код у Kotlin коротший і читабельніший.

  Якщо тип і так очевидний, його не потрібно писати вручну. Але в складних місцях явне зазначення типу все одно може покращити зрозумілість коду.

  ```kotlin
  val count = 10 // Int
  ```
* **Що таке функції-розширення (extension functions)?**
  Функції-розширення дозволяють додати нову поведінку до існуючого класу без наслідування і без зміни його вихідного коду. Це зручно для утиліт, форматування, мапінгу та покращення читабельності.

  Важливо розуміти, що extension function не змінює сам клас реально. Це лише синтаксичний цукор: виклик виглядає як метод обʼєкта, але насправді це звичайна функція.

  ```kotlin
  fun String.lastChar(): Char = this[this.length - 1]
  ```
* Що таке data class у Kotlin?
* Що таке companion object?
* Як створюються класи у Kotlin?
* Поясніть первинні та вторинні конструктори.
* Як працює наслідування у Kotlin?
* Як реалізувати singleton у Kotlin?
* Для чого використовується тип Unit?
* Що таке smart cast?
* Як Kotlin забезпечує null safety?
* Що таке Elvis-оператор (?:)?
* Що робить оператор !! і чому він небезпечний?
* Як працюють параметри за замовчуванням і іменовані параметри?
* Що таке destructuring declarations?
* Що таке type alias і коли його використовувати?
* У чому різниця між vararg і масивом?
* Як використовуються лямбда-вирази у Kotlin?

Колекції

* Як працюють колекції (List, Set, Map) у Kotlin?
* Як ітеруватися по колекціях у Kotlin?
* Що станеться, якщо змінити список, створений через listOf()?
* Що станеться при зверненні до індексу поза межами списку?

Java Interop

* Як працює взаємодія Kotlin з Java (interop)?
* Чи можна використовувати Java-анотації у Kotlin?

Інструменти та стиль

* Що таке Kotlin REPL?
* Що таке Kotlin script (.kts)?
* Що таке Kotlin style guide і чому його варто дотримуватись?
* Що таке Kotlin-ідіоми?

⸻

Middle

Функції та ООП

* Як Kotlin використовується в бекенд-розробці і чому його обирають замість Java?
* Що таке функції вищого порядку (higher-order functions)?
* Що таке inline-функції і коли їх варто використовувати?
* Що означає ключове слово reified і де воно застосовується?
* Що таке sealed-класи та sealed-інтерфейси?
* Що таке object expression і коли його використовують?
* У чому різниця між enum-класами та sealed-класами?
* У чому різниця між lateinit та lazy ініціалізацією?
* Що таке тип Nothing і як він впливає на керування потоком виконання?
* Чи може Java викликати Kotlin-функції з параметрами за замовчуванням?
* Що таке tail-recursive функції?

Scope-функції та колекції

* Що таке scope-функції (let, run, apply, also, with) і коли їх застосовувати?
* У чому різниця між let, run та apply?
* У чому різниця між map і flatMap?
* Що таке sequence і коли їх використовувати?
* Що таке лінива (lazy) обробка колекцій?

Корутини та Flow

* Що таке корутини і чим вони відрізняються від потоків (threads)?
* Що таке suspend-функція?
* Що таке CoroutineScope і для чого він потрібен?
* У чому різниця між launch і async?
* Що таке runBlocking і коли його використовують?
* Що таке диспетчери (Dispatchers.IO, Default, Main)?
* Як скасовувати корутини і обробляти помилки?
* Що таке структурована конкурентність (structured concurrency)?
* Чи можуть корутини виконуватись у будь-якому потоці?
* Що таке Flow у Kotlin?
* У чому різниця між Flow, StateFlow та SharedFlow?
* У чому різниця між Flow і LiveData?
* У чому різниця між холодними та гарячими потоками?
* Що таке collectLatest і коли його використовувати?
* Як перетворити callback-based API у suspend-функцію?
* У чому різниця між Channel і Flow?
* Які основні оператори Flow (map, filter, combine, zip)?
* У чому різниця між combine і zip?

JVM та тестування

* Для чого потрібні @JvmStatic, @JvmOverloads, @JvmField?
* Що таке делегування (delegation)?
* Які фреймворки для тестування доступні в Kotlin?
* Як мокати залежності у тестах?
* Як тестувати корутини та Flow?
* Які best practices для написання тестованого коду?
* Як ефективно керувати залежностями у Kotlin-проєкті?
* Як Kotlin працює з памʼяттю та garbage collection?
* Які best practices роботи з null safety?

⸻

Senior

Просунуті теми

* Що таке SupervisorScope і чим він відрізняється?
* Що таке backpressure і як його обробляти у Flow?
* Як використовувати Java Streams у Kotlin?
* Що таке Kotlin Multiplatform Mobile (KMM)?
* Які обмеження має Kotlin Multiplatform?
* Як ділитися бізнес-логікою у KMM?
* Як оптимізувати продуктивність Kotlin-коду?
* Як реалізувати retry з exponential backoff у Kotlin?
* Які нові можливості Kotlin варто знати у 2026 році?

⸻

2. Android Core

Junior

Android Basics

* Що таке життєвий цикл Activity?
* Що таке Fragment і чим він відрізняється від Activity?
* Що таке Intent і як він використовується?
* Що станеться, якщо не викликати super.onCreate()?
* Що таке View Binding і чому він кращий за findViewById?
* Що таке LiveData?
* Що таке Room і як він працює?
* Що роблять анотації @Query, @Insert, @Delete?
* Що таке WorkManager і коли його використовувати?

⸻

Middle

Architecture & DI

* У чому різниця між CoroutineScope, lifecycleScope та viewModelScope?
* Як реалізується dependency injection у Kotlin?
* Що таке Hilt і як він працює?
* Що таке @HiltViewModel?
* Як працює @Inject?
* Що таке області видимості (scopes) у Hilt?
* Як уникати витоків памʼяті в Android?
* Як безпечно зберігати API-ключі в Android-додатку?
* Як реалізувати offline-first архітектуру?

⸻

Senior

System Design

* Що таке архітектура MVI і коли її використовувати?
* Як проєктувати масштабовані Android-додатки?

⸻

3. Jetpack Compose

Junior

Compose Basics

* Що таке Jetpack Compose і чим він відрізняється від View System?
* Що таке @Composable і що таке recomposition?
* Що таке state hoisting?
* У чому різниця між remember і rememberSaveable?
* Що таке rememberCoroutineScope?
* Що таке collectAsState?
* Що таке Modifier у Compose?
* У чому різниця між padding і offset?
* У чому різниця між fillMaxSize і matchParentSize?
* Як обробляти кнопку “назад” у Compose?

⸻

Middle

State & Side Effects

* Для чого використовується rememberUpdatedState?
* Що таке side-effects у Compose і як їх обробляти?
* Що таке ключі (keys) у LaunchedEffect і чому вони важливі?
* Що таке snapshotFlow?
* Як оптимізувати recomposition у Compose?

⸻

Senior

Performance & Debugging

* Як дебажити проблеми з recomposition?
* Які є способи уникнути лагів у Compose, наприклад під час скролу?
* Чим відрізняються анотації Stable та Immutable?
* Навіщо передавати key у LazyColumn і що він собою являє під капотом?
* Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?

⸻

4. Computer Science / OOP / Архітектура

Junior

SOLID та ООП

* Що таке SOLID?
* Поясніть кожен принцип SOLID з прикладами з Android.
* Що таке ООП?
* Що таке абстракція?
* У чому різниця між конструкторами та методами?
* Що таке сигнатура методу?
* У чому різниця між перевантаженням (overloading) та перевизначенням (overriding)?
* У чому різниця між інтерфейсом та абстрактним класом?
* Чи може інтерфейс не містити методів?
* Чи може інтерфейс містити інший інтерфейс?
* Чому не варто додавати в інтерфейс десятки методів?
* Що таке композиція і чим вона відрізняється від наслідування?
* Які є модифікатори доступу в Kotlin?
* Що таке singleton?

Generics та колекції

* Що таке generics?
* Що таке type erasure і чому він виникає?
* Які існують колекції: List, Set, Map, Queue, Stack?
* Що таке потокобезпечність колекцій?

Exceptions

* У чому різниця між final, finally та finalize?
* Як працює try-catch-finally?
* У чому різниця між checked та unchecked exceptions?
* Які існують типи помилок у Kotlin/Java?
* Який базовий клас для помилок?

Алгоритми

* Що таке складність алгоритмів?
* Що таке пошук у глибину (DFS)?
* Що таке пошук у ширину (BFS)?

Архітектура

* Розкажіть про Clean Architecture.
* Що таке MVVM?
* У чому різниця між MVVM та MVI?
* Які архітектурні патерни використовуються в Android?
* Як організувати модулі в Android-проєкті?

Безпека та мережа

* Що таке токен?
* У чому різниця між authentication та authorization?
* Які інструменти використовуються для комунікації з бекендом?
* Що таке Retrofit?
* Що таке Apollo GraphQL?
* Для чого використовується Firebase?
* Що таке Android Keystore?

Корутини та асинхронність

* Які є dispatchers у Kotlin Coroutines і для чого вони використовуються?
* Які є способи запуску корутин?
* У чому різниця між launch та async?
* Як обробляти помилки в корутинах?
* Як дочекатися результату декількох паралельних запитів?

Android Components

* Для чого були придумані Fragment?
* Які проблеми вони вирішують?
* Які особливості вкладених Fragment?
* Які є виклики (callbacks) у Application?
* Який singleton створює система під час запуску Android-додатка?
* Як працює BackStack?
* У яких випадках можна отримати ANR (Application Not Responding)?
* Що можна виконувати в main thread?
* Які є базові компоненти Android?
* Що таке Service?
* Що таке BroadcastReceiver?
* Що таке Context і які його типи існують?
* Що таке Bundle?
* Чи можна вкладати Bundle у Bundle?
* Що таке Permissions в Android?
* Що таке setContentView?
* Які методи існують у View?
* Що таке ViewGroup?
* Для чого потрібен метод onStart() в Activity?
* Як можна потрапити в onStart(), але не потрапити в onResume()?

Data Layer

* Що таке міграції в Room?
* Що робить анотація Embedded у Room?
* Що таке DatabaseView у Room?
* У чому різниця між SQL та NoSQL?
* Що таке DataStore?
* Що таке Paging 3?

Android UI

* Що таке RecyclerView.Adapter?
* Які основні методи має RecyclerView.Adapter?
* У чому різниця між RecyclerView.Adapter та ListAdapter?
* Як працює RecyclerView під капотом?
* Що таке DiffUtil?
* Як відобразити списки в Android?
* Що таке lazy-контейнери в Compose?
* Як реалізувати циклічний список у RecyclerView?
* Що таке анімації в Android?
* У чому різниця між ViewBinding та DataBinding?

Serialization

* Що таке Serializable?
* Які є способи серіалізації в Android?
* Що робити, якщо поле може бути відсутнім у відповіді API?

Dependency Injection

* Що таке Dependency Injection?
* У чому різниця між Dagger/Hilt та Koin?

⸻

Middle

Compose Advanced

* Що таке CompositionLocal?
* Які бувають CompositionLocal?
* Як працює CompositionLocal під капотом?
* Що таке side effects у Compose?
* Як оптимізувати recomposition?
* Як написати асинхронний тест без використання корутин?

Kotlin Advanced

* У чому різниця між inline, noinline та crossinline?
* Які переваги Kotlin над Java?
* У чому різниця між val та const val?
* Які типи можна використовувати з const val?
* Чи можна створити data class без параметрів?
* У чому різниця між sealed class та enum?

Android Libraries

* Що таке WorkManager?
* Коли варто використовувати WorkManager?
* Чи працювали ви з Android Media3 / ExoPlayer?

⸻

Senior

Performance & Scaling

* Як би ви розподілили компоненти по модулях, якщо є кнопка, яка по кліку завантажує дані?
* Які є способи уникнути лагів у Compose, наприклад під час скролу?
* Чим відрізняються анотації Stable та Immutable?
* Навіщо передавати key у LazyColumn і що він собою являє під капотом?
* Які ще параметри можна передати в LazyColumn items() і для чого використовується contentType?
