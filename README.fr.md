**Read in other languages: [English 🇺🇸](README.en.md),
[Polska 🇵🇱](README.pl.md), [German 🇩🇪](README.de.md), [French 🇫🇷](README.fr.md),
[Spanish 🇪🇸](README.es.md), [Українська 🇺🇦](README.md).**

<h1>
  Kotlin <img src="./assets/kotlin.svg" width="40" height="40" alt="Kotlin logo"/>
</h1>

<h2>Questions et réponses les plus fréquentes lors d’un entretien sur Kotlin</h2>

<details>
<summary>1. Qu’est-ce que Kotlin et comment interagit-il avec Java ?</summary>

#### Kotlin

Kotlin est un langage de programmation à typage statique développé par
JetBrains. Il prend en charge la JVM, Android, JavaScript, WebAssembly ainsi que
les plateformes natives.

Sur la JVM, Kotlin est compilé en bytecode JVM et offre donc une interopérabilité
complète avec Java :

- Kotlin peut appeler directement du code Java et utiliser des bibliothèques
  Java.
- Java peut appeler du code Kotlin comme des classes et des méthodes JVM
  ordinaires.
- Les deux langages peuvent être utilisés dans un même projet et un même module.
- Les annotations `@JvmStatic`, `@JvmOverloads`, `@JvmField` et `@JvmName`
  permettent de concevoir une API facile à utiliser depuis Java.

```kotlin
val users = java.util.ArrayList<String>()
users.add("Ivan")
```

Il faut notamment tenir compte des platform types : lorsqu’une API Java ne
possède pas d’annotations de nullabilité correctes, Kotlin ne peut pas toujours
déterminer si la valeur `null` est autorisée. Des contrats explicites sont donc
nécessaires à la frontière entre Java et Kotlin.

**En bref :** Kotlin est interopérable avec Java au niveau de la JVM et peut être
introduit progressivement dans un projet Java existant sans réécriture complète.

</details>

<details>
<summary>2. Quels sont les types de données fondamentaux en Kotlin ?</summary>

#### Kotlin

Les principaux types de Kotlin sont :

- les nombres entiers : `Byte`, `Short`, `Int`, `Long` ;
- les nombres entiers non signés : `UByte`, `UShort`, `UInt`, `ULong` ;
- les nombres à virgule flottante : `Float`, `Double` ;
- le type booléen : `Boolean` ;
- les caractères et les chaînes de caractères : `Char`, `String` ;
- les tableaux : `Array<T>`, `IntArray`, `LongArray`, entre autres.

Certains types du système sont également importants :

- `Any` est le type de base de tous les types non nullables ;
- `Unit` indique qu’une fonction ne renvoie aucun résultat significatif ;
- `Nothing` indique qu’une fonction ne se termine jamais normalement ;
- les types nullables sont indiqués par `?`, par exemple `Int?`.

Sur la JVM, des types tels que `Int` peuvent être compilés en types primitifs
Java. Les types nullables et les types génériques nécessitent généralement une
mise en boîte.

- Kotlin n’effectue aucune conversion numérique implicite : un `Int` n’est pas
  automatiquement converti en `Long` :

```kotlin
val count: Int = 10
val total: Long = count.toLong()
```

**En bref :** Kotlin n’utilise pas de syntaxe distincte pour les types primitifs
et les types objets, mais le compilateur JVM emploie des primitives lorsque cela
est possible.

</details>

<details>
<summary>3. Quelle est la différence entre val et var en Kotlin ?</summary>

#### Kotlin

`val` interdit toute nouvelle affectation, tandis que `var` l’autorise :

```kotlin
val userName = "Ivan"
var retryCount = 0

retryCount += 1
// userName = "Petro" // Erreur de compilation
```

`val` ne rend pas l’objet lui-même immuable : seule la référence ne peut pas être
réaffectée.

```kotlin
val users = mutableListOf("Ivan")
users.add("Petro") // Autorisé
```

Pour les propriétés d’une classe, `val` ne possède qu’un getter, tandis que
`var` possède un getter et un setter.

**En bref :** utilisez `val` par défaut et réservez `var` aux valeurs qui doivent
réellement être modifiées.

</details>

<details>
<summary>4. Qu’est-ce que l’inférence de type (type inference) en Kotlin ?</summary>

#### Kotlin

L’inférence de type est la capacité du compilateur à déterminer un type à partir
d’une expression ou du contexte, sans qu’il soit indiqué explicitement. Kotlin
reste néanmoins un langage à typage statique.

```kotlin
val name = "Kotlin"             // String
val count = 10                  // Int
val names = listOf("A", "B")   // List<String>

val lengths = names.map { it.length } // it est un String
```

Il est préférable d’indiquer explicitement le type lorsqu’il :

- fait partie d’une API publique ;
- améliore la lisibilité d’une expression complexe ;
- doit être différent du type de l’implémentation concrète.

```kotlin
val users: List<User> = mutableListOf()
```

**En bref :** le compilateur déduit automatiquement les types évidents ; les
types explicites restent utiles pour définir les contrats et améliorer la
lisibilité.

</details>

<details>
<summary>5. Que sont les fonctions d’extension (extension functions) ?</summary>

#### Kotlin

Une fonction d’extension ajoute une syntaxe d’appel pratique à un type existant,
sans modifier sa classe et sans recourir à l’héritage.

```kotlin
fun String.lastChar(): Char = last()

val result = "Kotlin".lastChar()
```

`String` est le type receveur. Dans la fonction, son instance est accessible par
le mot-clé `this`.

Principales limitations :

- une extension n’a pas accès aux membres `private` et `protected` de la classe ;
- une méthode de la classe est prioritaire sur une extension possédant la même
  signature ;
- une extension est sélectionnée statiquement selon le type déclaré de la
  variable.

```kotlin
open class Animal
class Dog : Animal()

fun Animal.name() = "animal"
fun Dog.name() = "dog"

val animal: Animal = Dog()
println(animal.name()) // animal
```

Sur la JVM, une telle extension est généralement compilée en une fonction
statique qui reçoit l’objet comme premier argument.

**En bref :** les fonctions d’extension améliorent la lisibilité d’une API, mais
n’ajoutent aucune véritable méthode à la classe et ne prennent pas en charge la
redéfinition polymorphique.

</details>

<details>
<summary>6. Qu’est-ce qu’une data class en Kotlin ?</summary>

#### Kotlin

Une `data class` est destinée aux modèles dont le rôle principal consiste à
conserver des données. Le compilateur génère :

- `equals()` et `hashCode()` ;
- `toString()` ;
- `copy()` ;
- les fonctions `componentN()` utilisées pour la déstructuration.

```kotlin
data class User(
    val id: Long,
    val name: String
)

val user = User(id = 1, name = "Ivan")
val renamed = user.copy(name = "Petro")
```

Règles importantes :

- le constructeur principal doit contenir au moins un paramètre `val` ou `var` ;
- une `data class` ne peut pas être `abstract`, `open`, `sealed` ou `inner` ;
- seules les propriétés du constructeur principal participent aux méthodes
  générées ;
- `copy()` effectue une copie superficielle, et non une copie profonde.

Une propriété déclarée dans le corps de la classe n’est pas prise en compte par
`equals()`, `hashCode()` et `copy()` :

```kotlin
data class User(val id: Long) {
    var lastLoginAt: Long? = null
}
```

**En bref :** une `data class` réduit le code répétitif des DTO, des états
d’interface et des objets-valeurs. Il est préférable de conserver la logique
métier complexe dans des services de domaine ou des classes ordinaires.

</details>

<details>
<summary>7. Qu’est-ce qu’un companion object ?</summary>

#### Kotlin

Un `companion object` est un objet unique associé à une classe. Ses membres
peuvent être appelés par l’intermédiaire du nom de la classe :

```kotlin
class User private constructor(val name: String) {
    companion object {
        const val MAX_NAME_LENGTH = 50

        fun create(name: String): User = User(name.trim())
    }
}

val user = User.create(" Ivan ")
```

Il ne s’agit pas d’un membre `static` de Java : le companion object possède son
propre type, peut porter un nom, implémenter des interfaces et accéder aux membres
`private` de la classe. Une classe ne peut contenir qu’un seul companion object.

Pour rendre l’appel d’une fonction plus pratique depuis Java, on utilise
`@JvmStatic` :

```kotlin
class Config {
    companion object {
        @JvmStatic
        fun default(): Config = Config()
    }
}
```

Sans cette annotation, Java appelle `Config.Companion.default()` ; avec elle,
Java peut appeler `Config.default()`.

**En bref :** un companion object convient aux méthodes de fabrique et aux
constantes associées à une classe. Il est déconseillé d’y conserver un état
global mutable.

</details>

<details>
<summary>8. Comment crée-t-on des classes en Kotlin ?</summary>

#### Kotlin

Une classe se déclare à l’aide du mot-clé `class`. Le constructeur principal est
écrit dans l’en-tête de la classe :

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

- Les paramètres `val` et `var` du constructeur deviennent des propriétés de la
  classe.
- Le bloc `init` est exécuté lors de la création de l’objet et permet de vérifier
  les invariants.
- Les valeurs par défaut évitent de créer plusieurs constructeurs surchargés.
- Un objet est créé sans utiliser le mot-clé `new`.

Les classes et leurs méthodes sont `final` par défaut. Pour autoriser
l’héritage, il faut utiliser `open` :

```kotlin
open class User(val id: Long)

class AdminUser(id: Long) : User(id)
```

**En bref :** Kotlin permet de déclarer le constructeur, les propriétés et les
valeurs par défaut directement dans l’en-tête de la classe ; l’héritage doit être
explicitement autorisé.

</details>

<details>
<summary>9. Expliquez les constructeurs principaux et secondaires.</summary>

#### Kotlin

Le constructeur principal est déclaré dans l’en-tête de la classe :

```kotlin
class User(
    val id: Long,
    val name: String
)
```

Ses paramètres ne deviennent des propriétés que s’ils sont précédés de `val` ou
de `var`. Le corps du constructeur principal est défini à l’aide d’un bloc
`init` :

```kotlin
class User(val name: String) {
    init {
        require(name.isNotBlank())
    }
}
```

Un constructeur secondaire est déclaré dans le corps de la classe avec
`constructor`. Si la classe possède un constructeur principal, le constructeur
secondaire doit lui déléguer l’appel au moyen de `this(...)` :

```kotlin
class User(val id: Long, val name: String) {
    constructor(name: String) : this(id = 0, name = name)
}
```

Les propriétés et les blocs `init` sont exécutés dans leur ordre de déclaration,
puis le corps du constructeur secondaire est exécuté.

Si la seule différence concerne une valeur par défaut, il n’est pas nécessaire
de créer un constructeur distinct :

```kotlin
class User(val name: String, val id: Long = 0)
```

**En bref :** le constructeur principal est la solution privilégiée. Les
constructeurs secondaires sont surtout utiles pour l’interopérabilité avec Java
ou pour répondre aux exigences d’un framework ; des paramètres par défaut ou une
méthode de fabrique suffisent généralement.

</details>

<details>
<summary>10. Comment fonctionne l’héritage en Kotlin ?</summary>

#### Kotlin

En Kotlin, les classes et leurs membres sont `final` par défaut. Une classe doit
être marquée `open` pour autoriser l’héritage, et un membre redéfini doit être
marqué `override` :

```kotlin
open class Animal(val name: String) {
    open fun speak(): String = "..."
}

class Dog(name: String) : Animal(name) {
    override fun speak(): String = "Woof"
}
```

Les propriétés peuvent également être redéfinies :

```kotlin
open class Screen {
    open val title: String = "Base"
}

class HomeScreen : Screen() {
    override val title: String = "Home"
}
```

Principales particularités :

- une classe ne peut hériter que d’une seule classe, mais peut implémenter
  plusieurs interfaces ;
- les membres `abstract` sont déjà ouverts et ne nécessitent pas `open` ;
- un membre redéfini reste ouvert, sauf s’il est déclaré `final override` ;
- l’implémentation de la classe parente est accessible au moyen de `super`.

**En bref :** l’héritage doit être explicitement autorisé en Kotlin. Il est
préférable d’utiliser des interfaces pour les contrats et d’envisager la
composition avant de créer une hiérarchie de classes profonde.

</details>

<details>
<summary>11. Comment implémenter un singleton en Kotlin ?</summary>

#### Kotlin

La manière standard d’implémenter un singleton en Kotlin consiste à utiliser une
déclaration `object` :

```kotlin
object AppConfig {
    const val API_URL = "https://api.example.com"
}

val url = AppConfig.API_URL
```

Sur la JVM, l’objet est créé lors de son premier accès et son initialisation est
thread-safe. Un `object` peut contenir des propriétés, des fonctions et des blocs
`init`, hériter d’une classe et implémenter des interfaces. Son constructeur ne
peut pas être appelé.

Une déclaration `object` convient aux constantes, aux stratégies sans état et à
certains cas de hiérarchies scellées. Pour les services possédant des
dépendances, il est préférable d’utiliser une portée singleton dans un conteneur
d’injection de dépendances.

Un état mutable placé dans un `object` est global, complique les tests et exige
une synchronisation. Sur Android, il ne faut pas y conserver une `Activity` ou
une `View`, car cela peut provoquer une fuite de mémoire.

**En bref :** `object` permet de créer simplement un singleton sans paramètres de
constructeur. Pour les services métier possédant des dépendances et un cycle de
vie contrôlé, il est préférable d’utiliser l’injection de dépendances.

</details>

<details>
<summary>12. À quoi sert le type Unit ?</summary>

#### Kotlin

`Unit` indique qu’une fonction se termine sans produire de résultat significatif :

```kotlin
fun log(message: String): Unit {
    println(message)
}
```

Dans une fonction ordinaire, il n’est pas nécessaire de préciser `: Unit` :

```kotlin
fun log(message: String) {
    println(message)
}
```

Contrairement au `void` de Java, `Unit` est un type à part entière qui possède une
valeur unique, également nommée `Unit`. Il est utilisé dans les types de fonctions
et les API génériques :

```kotlin
val onClick: () -> Unit = { println("Clicked") }
```

Il ne faut pas le confondre avec `Nothing` : `Unit` signifie que la fonction se
termine normalement sans résultat, tandis que `Nothing` indique qu’elle ne se
termine jamais normalement :

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

**En bref :** `Unit` est le type des fonctions qui ne produisent aucun résultat
significatif. Il est généralement indiqué explicitement dans les types des
fonctions de rappel, par exemple `() -> Unit`.

</details>

<details>
<summary>13. Qu’est-ce qu’un smart cast ?</summary>

#### Kotlin

Un smart cast est une conversion automatique vers un type plus précis lorsque le
compilateur peut prouver qu’elle est sûre :

```kotlin
fun printLength(value: Any?) {
    if (value is String) {
        println(value.length) // value est de type String
    }
}
```

Ce mécanisme fonctionne après les vérifications `is`, `!is` et `null`, tout en
tenant compte du flux d’exécution :

```kotlin
fun handle(value: Any?) {
    if (value !is String) return

    println(value.length) // value est de type String
}
```

Le smart cast ne fonctionne pas si la valeur peut changer entre la vérification
et son utilisation. C’est notamment le cas d’une propriété mutable ou dotée d’un
accesseur personnalisé :

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

**En bref :** le smart cast évite les conversions explicites avec `as` après une
vérification de type ou de nullabilité, mais uniquement lorsque le compilateur
garantit que la valeur vérifiée ne peut pas changer.

</details>

<details>
<summary>14. Comment Kotlin garantit-il la null safety ?</summary>

#### Kotlin

En Kotlin, les types nullables et non nullables sont distingués au niveau du
système de types :

```kotlin
val name: String = "Kotlin"
val optionalName: String? = null
```

Pour travailler avec des valeurs nullables, on utilise notamment :

```kotlin
val nullableLength: Int? = optionalName?.length // appel sécurisé
val length: Int = optionalName?.length ?: 0     // opérateur Elvis
```

Après une vérification de `null`, le compilateur peut effectuer un smart cast :

```kotlin
fun printName(value: String?) {
    if (value != null) {
        println(value.uppercase())
    }
}
```

L’opérateur `!!` convertit une valeur nullable en valeur non nullable, mais lève
une `NullPointerException` si cette valeur est `null` :

```kotlin
val length = optionalName!!.length
```

La principale faiblesse réside dans les platform types provenant de Java : sans
annotations de nullabilité, le compilateur ignore si une API Java peut renvoyer
`null`. Ces valeurs doivent être vérifiées à la frontière entre Java et Kotlin.

**En bref :** `T` n’accepte pas `null`, tandis que `T?` l’accepte. Utilisez `?.`,
`?:` et des vérifications ; réservez `!!` aux situations dans lesquelles le
caractère non nul de la valeur est formellement garanti.

</details>

<details>
<summary>15. Qu’est-ce que l’opérateur Elvis (`?:`) ?</summary>

#### Kotlin

L’opérateur Elvis `?:` renvoie l’opérande de gauche s’il n’est pas `null` ;
sinon, il évalue et renvoie celui de droite :

```kotlin
val name: String? = null
val displayName = name ?: "Unknown"
```

Il est souvent utilisé avec un appel sécurisé :

```kotlin
val nameLength = user?.name?.length ?: 0
```

Puisque `return` et `throw` sont des expressions en Kotlin, ils peuvent également
figurer à droite de l’opérateur :

```kotlin
fun handleName(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}

fun requireName(name: String?): String =
    name ?: throw IllegalArgumentException("Name is required")
```

**En bref :** `?:` définit une valeur de remplacement pour `null`. Il ne faut pas
l’utiliser pour masquer une erreur lorsque `null` représente un état invalide ;
dans ce cas, il est préférable d’interrompre explicitement l’exécution.

</details>

<details>
<summary>16. Que fait l’opérateur `!!` et pourquoi est-il dangereux ?</summary>

#### Kotlin

L’opérateur `!!` convertit de force un `T?` en `T`. Si la valeur est `null`, une
`NullPointerException` est levée :

```kotlin
val name: String? = null
val length = name!!.length // NullPointerException
```

Les solutions plus sûres dépendent du comportement souhaité :

```kotlin
val nullableLength = name?.length
val length = name?.length ?: 0
val value = name ?: return
val requiredName = requireNotNull(name) { "Name is required" }
```

`requireNotNull` et `checkNotNull` interrompent également l’exécution par une
erreur, mais décrivent explicitement le contrat qui a été enfreint. Une
utilisation fréquente de `!!` révèle généralement une modélisation incorrecte
de l’état nullable ou des problèmes liés au cycle de vie.

**En bref :** utilisez `!!` uniquement lorsque le caractère non nul est garanti
par le contrat, mais que le compilateur ne peut pas le prouver. Dans tous les
autres cas, traitez explicitement la valeur `null`.

</details>

<details>
<summary>17. Comment fonctionnent les paramètres par défaut et les paramètres nommés ?</summary>

#### Kotlin

La valeur par défaut d’un paramètre est utilisée lorsque l’argument correspondant
n’est pas fourni :

```kotlin
fun createUser(
    name: String,
    isActive: Boolean = true,
    role: String = "user"
) = User(name, isActive, role)

createUser("Ivan")
createUser(name = "Ivan", role = "admin")
```

Les arguments nommés améliorent la lisibilité, notamment lorsque plusieurs
paramètres sont du même type ou de type `Boolean` :

```kotlin
fun connect(host: String, port: Int, useSsl: Boolean, retry: Boolean)

connect(
    host = "api.example.com",
    port = 443,
    useSsl = true,
    retry = false
)
```

Java ne prend pas en charge les paramètres par défaut ni les arguments nommés de
Kotlin. Pour une API destinée à Java, il est possible de générer des surcharges
avec `@JvmOverloads` :

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Le nom des paramètres d’une fonction publique fait partie de son API Kotlin :
le modifier peut rompre les appels utilisant des arguments nommés. Si une
fonction possède trop de paramètres, il est préférable de créer une classe de
configuration distincte.

**En bref :** les valeurs par défaut remplacent la plupart des surcharges, tandis
que les arguments nommés rendent les appels plus explicites. Pour Java, pensez à
utiliser `@JvmOverloads`.

</details>

<details>
<summary>18. Que sont les déclarations de déstructuration ?</summary>

#### Kotlin

Une déclaration de déstructuration décompose un objet en plusieurs variables au
moyen des fonctions `componentN()` :

```kotlin
data class User(val id: Long, val name: String)

val user = User(1, "Ivan")
val (id, name) = user
```

Le code équivalent est le suivant :

```kotlin
val id = user.component1()
val name = user.component2()
```

Pour une `data class`, ces fonctions sont générées selon l’ordre des propriétés
du constructeur principal. La déstructuration fonctionne également dans les
boucles et avec `Map.Entry` :

```kotlin
val scores = mapOf("Ivan" to 10, "Petro" to 20)

for ((name, score) in scores) {
    println("$name -> $score")
}
```

Un composant inutile peut être ignoré à l’aide de `_` :

```kotlin
val (_, name) = user
```

Pour une classe ordinaire, il est possible de définir ses propres fonctions
`operator fun componentN()`. Cependant, la déstructuration dépend de l’ordre des
composants et peut donc nuire à la lisibilité des modèles volumineux.

**En bref :** la déstructuration est une syntaxe pratique reposant sur
`componentN()` pour les petits modèles et les paires de valeurs. L’ordre des
composants fait partie du contrat.

</details>

<details>
<summary>19. Qu’est-ce qu’un alias de type et quand faut-il l’utiliser ?</summary>

#### Kotlin

`typealias` crée un nom plus court ou plus explicite pour un type existant :

```kotlin
typealias UserId = Long
typealias UserCache = Map<UserId, User>
typealias OnUserClick = (User) -> Unit

fun loadUser(id: UserId) = Unit
```

Point important : un alias ne crée pas un nouveau type et n’apporte aucune
sécurité de typage supplémentaire :

```kotlin
typealias UserId = Long
typealias OrderId = Long

val orderId: OrderId = 10
loadUser(orderId) // Valide, car les deux types correspondent à Long
```

Si le compilateur doit distinguer ces valeurs, il faut utiliser une
`value class` :

```kotlin
@JvmInline
value class UserId(val value: Long)

@JvmInline
value class OrderId(val value: Long)
```

**En bref :** `typealias` améliore la lisibilité des types génériques et des
types de fonctions complexes. Pour créer un type métier distinct, utilisez une
`value class`.

</details>

<details>
<summary>20. Quelle est la différence entre `vararg` et un tableau ?</summary>

#### Kotlin

Un tableau est un objet contenant un nombre fixe d’éléments. `vararg` est un
paramètre de fonction qui permet de transmettre un nombre variable d’arguments :

```kotlin
fun printNames(vararg names: String) {
    names.forEach(::println)
}

printNames("Ivan", "Petro")
```

À l’intérieur de la fonction, `names` est un tableau. Un tableau existant est
transmis à l’aide de l’opérateur de décomposition `*` :

```kotlin
val names = arrayOf("Ivan", "Petro")
printNames(*names)
```

Si un autre paramètre suit `vararg`, il doit être passé sous forme d’argument
nommé :

```kotlin
fun createUser(vararg roles: String, isActive: Boolean) = Unit

createUser("admin", "editor", isActive = true)
```

Une fonction ne peut posséder qu’un seul paramètre `vararg`. Pour `vararg Int`,
le compilateur utilise `IntArray`. L’opérateur de décomposition peut créer une
copie du tableau, ce qui doit être pris en compte dans le code sensible aux
performances.

**En bref :** `Array<T>` est une structure de données, tandis que `vararg` permet
d’accepter un nombre quelconque d’arguments. Un tableau est transmis à un
paramètre `vararg` sous la forme `*array`.

</details>

<details>
<summary>21. Comment utilise-t-on les expressions lambda en Kotlin ?</summary>

#### Kotlin

Une lambda est une fonction anonyme qui peut être stockée dans une variable,
transmise comme argument et renvoyée par une fonction :

```kotlin
val sum: (Int, Int) -> Int = { first, second -> first + second }

val result = sum(2, 3)
```

La dernière expression constitue le résultat de la lambda. Lorsqu’elle ne
possède qu’un seul paramètre, celui-ci peut être désigné par `it` :

```kotlin
val numbers = listOf(1, 2, 3)
val evenNumbers = numbers.filter { it % 2 == 0 }
```

Si la lambda est le dernier argument, elle peut être placée hors des parenthèses :

```kotlin
button.setOnClickListener { println("Clicked") }
```

Une lambda peut capturer des variables de la portée extérieure :

```kotlin
var counter = 0
val increment = { counter++ }
increment()
```

Si la fonction nécessaire existe déjà, il est possible d’utiliser une référence
de fonction avec `::` :

```kotlin
fun printName(name: String) = println(name)

listOf("Ivan", "Petro").forEach(::printName)
```

**En bref :** les lambdas permettent de transmettre un comportement à `map`,
`filter`, aux fonctions de rappel et aux DSL. Il est préférable d’extraire les
lambdas longues ou imbriquées dans des fonctions nommées et de contrôler la
capture d’un état mutable, en particulier dans le code concurrent.

</details>

<details>
<summary>22. Comment fonctionnent les collections (`List`, `Set`, `Map`) en Kotlin ?</summary>

#### Kotlin

Les principales collections de Kotlin sont les suivantes :

```kotlin
val names: List<String> = listOf("Ivan", "Petro", "Ivan")
val ids: Set<Int> = setOf(1, 2, 2) // 1, 2
val users: Map<Long, String> = mapOf(1L to "Ivan")
```

- `List` conserve l’ordre, accepte les doublons et permet l’accès par indice.
- `Set` contient des éléments uniques ; leur unicité est déterminée par
  `equals()` et `hashCode()`.
- `Map` contient des paires clé-valeur dont les clés sont uniques. `map[key]`
  renvoie une valeur nullable ; `containsKey()` permet donc de vérifier la
  présence d’une clé.

Les collections possèdent des interfaces en lecture seule (`List`, `Set`, `Map`)
et des interfaces mutables (`MutableList`, `MutableSet`, `MutableMap`) :

```kotlin
val mutable: MutableList<String> = mutableListOf("A", "B")
mutable.add("C")
```

Une interface en lecture seule ne signifie pas que la collection est immuable.
Une autre référence peut modifier le même objet :

```kotlin
val source = mutableListOf("A")
val view: List<String> = source

source.add("B")
println(view) // [A, B]
```

Les opérations `filter`, `map`, `sorted` et similaires renvoient de nouvelles
collections :

```kotlin
val result = names
    .filter { it.length > 4 }
    .map { it.uppercase() }
    .sorted()
```

Les chaînes d’opérations sur un `Iterable` peuvent créer des collections
intermédiaires. Pour les longues chaînes, il peut être utile d’envisager une
`Sequence`, sans toutefois l’utiliser systématiquement.

**En bref :** il est préférable d’exposer des interfaces en lecture seule et de
conserver les collections mutables comme un détail d’implémentation. Lorsqu’une
véritable immuabilité est nécessaire, créez une copie ou utilisez des collections
immuables.

</details>

<details>
<summary>23. Comment parcourir les collections en Kotlin ?</summary>

#### Kotlin

Pour un parcours classique, on utilise `for` :

```kotlin
val names = listOf("Ivan", "Petro", "Oksana")

for (name in names) {
    println(name)
}
```

Pour effectuer une action courte sur chaque élément, `forEach` convient bien :

```kotlin
names.forEach { println(it) }
```

Si l’indice est nécessaire :

```kotlin
for ((index, name) in names.withIndex()) {
    println("$index: $name")
}
```

Une `Map` peut être facilement parcourue au moyen de la déstructuration :

```kotlin
val usersById = mapOf(1L to "Ivan", 2L to "Petro")

for ((id, name) in usersById) {
    println("$id -> $name")
}
```

Pour une transformation ou une agrégation, on utilise l’opérateur approprié :

```kotlin
val longNames = names.filter { it.length > 4 }
val lengths = names.map { it.length }
val totalLength = names.sumOf { it.length }
```

Pour `break`, `continue` et les flux de contrôle complexes, il est préférable
d’utiliser `for`. Pour une recherche, utilisez `firstOrNull`, `find`, `any` ou
`none` plutôt qu’une boucle écrite manuellement.

**En bref :** `for` sert à contrôler la boucle, `forEach` aux effets de bord
simples, et `map` ou `filter` à produire un résultat. Choisissez l’opérateur selon
l’intention du code.

</details>

<details>
<summary>24. Que se passe-t-il si l’on modifie une liste créée avec `listOf()` ?</summary>

#### Kotlin

`listOf()` renvoie une `List<T>`, c’est-à-dire une interface en lecture seule qui
ne possède pas les méthodes `add`, `remove` et `set` :

```kotlin
val names = listOf("Ivan", "Petro")

// names.add("Oksana") // Erreur de compilation
```

Cependant, `List` ne garantit pas une immuabilité complète. Une autre référence
mutable peut modifier la même liste :

```kotlin
val source = mutableListOf("Ivan")
val view: List<String> = source

source.add("Petro")
println(view) // [Ivan, Petro]
```

Convertir le résultat de `listOf()` en `MutableList` par un cast est dangereux :
le code dépend alors de l’implémentation concrète et peut échouer à l’exécution.

Si des modifications sont nécessaires, créez une liste mutable ou une copie :

```kotlin
val mutableNames = names.toMutableList()
mutableNames.add("Oksana")
```

Pour adopter un style immuable, créez une nouvelle liste :

```kotlin
val updated = names + "Oksana"
```

**En bref :** une liste créée avec `listOf()` ne peut pas être modifiée par
l’intermédiaire de l’API `List`. Pour effectuer des modifications, utilisez
`mutableListOf()` ou `toMutableList()`, et non un cast dangereux.

</details>

<details>
<summary>25. Que se passe-t-il lorsqu’on accède à un indice situé hors des limites d’une liste ?</summary>

#### Kotlin

L’accès avec `list[index]` appelle `get(index)`. Si l’indice est négatif ou
supérieur ou égal à `size`, une `IndexOutOfBoundsException` est levée :

```kotlin
val names = listOf("Ivan", "Petro")

println(names[0]) // Ivan
println(names[2]) // IndexOutOfBoundsException
```

Voici des solutions sûres :

```kotlin
val name: String? = names.getOrNull(2)
val fallback = names.getOrElse(2) { "Unknown" }
val isValid = 2 in names.indices
```

Pour une liste susceptible d’être vide, il existe des fonctions sûres
spécifiques :

```kotlin
val first = names.firstOrNull()
val last = names.lastOrNull()
```

**En bref :** utilisez `list[index]` lorsque la validité de l’indice constitue
un invariant. Pour les indices provenant de l’extérieur ou facultatifs, utilisez
`getOrNull`, `getOrElse` ou une vérification avec `indices`.

</details>

<details>
<summary>26. Comment fonctionne l’interopérabilité entre Kotlin et Java ?</summary>

#### Kotlin

Kotlin/JVM est compilé en bytecode JVM, ce qui permet au code Kotlin et au code
Java d’appeler directement leurs classes et méthodes respectives :

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

Le principal risque provient des platform types issus de Java, dont la
nullabilité est inconnue. À la frontière entre les langages, il faut utiliser
`@Nullable` et `@NotNull`, vérifier les valeurs et éviter de transmettre les
platform types aux couches internes de l’application.

Pour créer une API pratique à utiliser depuis Java, on emploie notamment :

- `@JvmStatic` — une méthode statique ;
- `@JvmOverloads` — des surcharges pour les paramètres par défaut ;
- `@JvmField` — un champ sans getter ni setter ;
- `@JvmName` — un autre nom sur la JVM ;
- `@Throws` — la déclaration des exceptions vérifiées pour Java.

```kotlin
@Throws(IOException::class)
fun readFile(path: String): String = File(path).readText()
```

Les interfaces SAM de Java peuvent être représentées par des lambdas :

```kotlin
executor.execute { println("Done") }
```

**En bref :** l’interopérabilité permet une migration progressive entre Java et
Kotlin. La nullabilité, les collections mutables, les paramètres par défaut et
la facilité d’utilisation de l’API depuis Java exigent une attention particulière.

</details>

<details>
<summary>27. Peut-on utiliser des annotations Java en Kotlin ?</summary>

#### Kotlin

Oui. Les annotations Java peuvent être appliquées aux classes Kotlin et à leurs
membres :

```kotlin
class UserTest {
    @org.junit.Test
    fun loadsUser() = Unit
}
```

Sur la JVM, une propriété Kotlin peut être représentée par un champ, un getter,
un setter et un paramètre de constructeur. Pour appliquer l’annotation à
l’élément souhaité, on utilise une cible d’utilisation :

```kotlin
data class User(
    @field:JsonProperty("user_name")
    val name: String
)
```

Les principales variantes sont les suivantes :

- `@field:` — le champ sous-jacent ;
- `@get:` / `@set:` — le getter ou le setter ;
- `@param:` — le paramètre du constructeur ;
- `@property:` — la propriété Kotlin, qui ne constitue pas un élément Java
  distinct.

Les annotations de nullabilité Java aident Kotlin à déterminer le type du
résultat d’une API Java :

```java
@NotNull
String getName();
```

Sans annotation correcte, le résultat peut rester un platform type. Pour les
frameworks de réflexion ou de validation, il faut vérifier quel élément JVM ils
analysent.

**En bref :** les annotations Java sont directement prises en charge. La
principale subtilité consiste à choisir correctement `@field:`, `@get:` ou
`@param:` selon le framework concerné.

</details>

<details>
<summary>28. Qu’est-ce que le REPL de Kotlin ?</summary>

#### Kotlin

REPL signifie Read-Eval-Print Loop : l’environnement lit le code saisi,
l’exécute, affiche le résultat, puis attend la commande suivante.

```kotlin
val numbers = listOf(1, 2, 3)
numbers.map { it * 2 }
// [2, 4, 6]
```

Le REPL convient pour vérifier rapidement la syntaxe, la bibliothèque standard
et de petites expressions sans créer de projet. Il peut être lancé depuis
l’interface en ligne de commande de Kotlin ; pour des expériences plus longues,
les fichiers scratch ou Kotlin Playground sont plus pratiques.

Le REPL ne reproduit ni le cycle de vie Android, ni l’injection de dépendances,
ni la configuration Gradle, ni un véritable environnement concurrent. Il ne
remplace pas non plus les tests automatisés.

**En bref :** le REPL de Kotlin est une console interactive destinée aux
expérimentations locales avec le langage. Tout résultat important pour
l’application doit être confirmé par des tests dans l’environnement réel.

</details>

<details>
<summary>29. Qu’est-ce qu’un script Kotlin (`.kts`) ?</summary>

#### Kotlin

Un script Kotlin est un fichier `.kts` dans lequel des instructions de niveau
supérieur peuvent être exécutées sans `fun main()` :

```kotlin
println("Hello from Kotlin script")

val names = listOf("Ivan", "Petro")
names.forEach(::println)
```

Un fichier `.kt` est un fichier source ordinaire d’une application ou d’une
bibliothèque, tandis qu’un fichier `.kts` est exécuté par un environnement de
script. L’exemple le plus courant est le DSL Kotlin de Gradle :

```kotlin
tasks.register("hello") {
    doLast {
        println("Hello from Gradle")
    }
}
```

Les fichiers `build.gradle.kts` et `settings.gradle.kts` sont exécutés par Gradle
comme des scripts de configuration. Les scripts autonomes peuvent servir à de
petites automatisations, mais leur modèle de dépendances et leur API dépendent
de l’hôte de script.

**En bref :** le format `.kts` convient à la configuration et aux automatisations
courtes. Une logique qui s’étoffe, possède des dépendances et nécessite des tests
devrait être déplacée dans un module Kotlin ordinaire ou une application en ligne
de commande.

</details>

<details>
<summary>30. Qu’est-ce que le guide de style Kotlin et pourquoi faut-il le respecter ?</summary>

#### Kotlin

Le guide de style Kotlin définit les règles de formatage, de nommage, de
structuration des fichiers et d’organisation des importations. Il rend le code
d’une équipe homogène et prévisible.

```kotlin
class UserRepository // PascalCase

fun loadUserById(id: Long): User // camelCase

val isUserActive = true // camelCase

const val DEFAULT_TIMEOUT_SECONDS = 30 // UPPER_SNAKE_CASE
```

Ses avantages pratiques sont les suivants :

- moins de discussions sur le formatage pendant les revues de code ;
- une lecture du code et une intégration des nouveaux développeurs plus rapides ;
- une vérification automatisée grâce au formateur de l’IDE, à `ktlint` ou à
  `detekt` dans la CI.

Les règles internes qui diffèrent des recommandations officielles doivent être
documentées et configurées de manière identique dans l’IDE et la CI. Le guide de
style encadre la présentation du code, mais ne remplace pas les règles
d’architecture.

**En bref :** un guide de style réduit la charge cognitive et confie la
vérification du formatage à des outils automatisés plutôt qu’aux personnes.

</details>

<details>
<summary>31. Que sont les idiomes Kotlin ?</summary>

#### Kotlin

Les idiomes Kotlin sont des manières établies d’utiliser les fonctionnalités du
langage afin que le code exprime clairement son intention.

En voici quelques exemples courants :

- utiliser `val` par défaut et réserver `var` aux mutations nécessaires ;
- préférer les types nullables, `?.` et `?:` à `!!` ;
- employer une `data class` pour les modèles de données ;
- utiliser des types `sealed` pour un ensemble limité d’états ;
- préférer `map`, `filter` et `associateBy` aux boucles écrites manuellement pour
  les transformations ;
- utiliser des clauses de garde afin de réduire l’imbrication.

```kotlin
fun handle(user: User?) {
    val existingUser = user ?: return
    process(existingUser)
}
```

Les fonctions de portée, les lambdas et la surcharge d’opérateurs ne sont
pertinentes que lorsqu’elles améliorent la lisibilité. Employer davantage de
fonctionnalités du langage ne rend pas, à lui seul, le code idiomatique.

**En bref :** un code Kotlin idiomatique est sûr et compréhensible ; il utilise
les fonctionnalités du langage conformément à leur finalité, et non pour faire
étalage de la syntaxe.

</details>

<details>
<summary>32. Comment Kotlin est-il utilisé dans le développement backend et pourquoi le choisir plutôt que Java ?</summary>

#### Kotlin

Kotlin est utilisé pour les backends JVM : API REST ou gRPC, microservices,
workers et systèmes orientés événements. Il fonctionne avec Spring Boot, Ktor
et les bibliothèques Java.

```kotlin
@RestController
class UserController(
    private val userService: UserService
) {
    @GetMapping("/users/{id}")
    fun getUser(@PathVariable id: Long): UserResponse = userService.getUser(id)
}
```

Ses principaux avantages par rapport à Java sont les suivants :

- la null safety ;
- moins de code répétitif grâce aux `data class`, aux paramètres par défaut et
  aux fonctions d’extension ;
- les coroutines pour les entrées-sorties non bloquantes ;
- une migration progressive et l’accès à l’ensemble de l’écosystème Java.

```kotlin
data class UserResponse(
    val id: Long,
    val name: String,
    val email: String?
)
```

Les compromis comprennent une compilation plus lente, des configurations
supplémentaires du compilateur ou de Gradle pour certains frameworks Java, ainsi
que la nécessité de former l’équipe au Kotlin idiomatique et à la concurrence
structurée.

**En bref :** Kotlin constitue un bon choix lorsqu’une équipe souhaite écrire du
code JVM plus sûr et plus concis sans renoncer à l’écosystème Java, et qu’elle
est prête à accepter une chaîne d’outils plus complexe.

</details>

<details>
<summary>33. Que sont les fonctions d’ordre supérieur ?</summary>

#### Kotlin

Une fonction d’ordre supérieur accepte une fonction comme argument ou en renvoie
une comme résultat.

```kotlin
fun repeatAction(times: Int, action: () -> Unit) {
    repeat(times) { action() }
}

repeatAction(3) { println("Hello") }
```

`action: () -> Unit` désigne une fonction sans paramètre et sans résultat
significatif. Voici un exemple de fonction qui en renvoie une autre :

```kotlin
fun multiplier(factor: Int): (Int) -> Int =
    { value -> value * factor }

val double = multiplier(2)
println(double(10)) // 20
```

La bibliothèque standard en fournit des exemples courants :

```kotlin
val result = numbers
    .filter { it > 0 }
    .map { it * 2 }
```

Ces fonctions sont utilisées avec les collections, les fonctions de rappel et
les DSL. Les lambdas peuvent créer des objets et capturer un état extérieur ;
`inline` élimine souvent ce surcoût pour les petites fonctions d’ordre supérieur.

**En bref :** les fonctions d’ordre supérieur permettent de transmettre un
comportement comme une valeur. Il est préférable d’extraire les lambdas complexes
ou imbriquées dans des fonctions nommées.

</details>

<details>
<summary>34. Que sont les fonctions `inline` et quand faut-il les utiliser ?</summary>

#### Kotlin

Le mot-clé `inline` demande au compilateur d’insérer le corps de la fonction et
ses paramètres lambda à l’endroit de l’appel. Son objectif principal est de
réduire le surcoût des fonctions d’ordre supérieur :

```kotlin
inline fun measure(block: () -> Unit) {
    val start = System.nanoTime()
    block()
    println(System.nanoTime() - start)
}

measure { println("Work") }
```

`inline` permet également d’utiliser des paramètres de type `reified` :

```kotlin
inline fun <reified T> Any?.isOfType(): Boolean = this is T
```

Les paramètres lambda peuvent recevoir des modificateurs supplémentaires :

- `noinline` — empêche l’insertion de la lambda, afin de pouvoir la stocker ou la
  transmettre ;
- `crossinline` — insère la lambda, mais interdit les retours non locaux avec
  `return`.

`inline` convient aux petites fonctions d’ordre supérieur, aux génériques
`reified` et aux chemins critiques dont les performances ont été mesurées. Pour
les fonctions volumineuses, il peut augmenter la taille du bytecode. Les
modifications apportées aux fonctions `inline` publiques exigent également une
attention particulière à la compatibilité binaire.

**En bref :** n’utilisez pas `inline` comme une optimisation universelle, mais
pour réduire le surcoût des lambdas, employer `reified` ou obtenir la sémantique
requise des retours non locaux.

</details>

<details>
<summary>35. Que signifie le mot-clé `reified` et où est-il utilisé ?</summary>

#### Kotlin

En raison de l’effacement des types, un paramètre générique ordinaire `T` ne
peut pas servir à vérifier un type ni à obtenir `T::class` :

```kotlin
// fun <T> isOfType(value: Any) = value is T // Erreur de compilation
```

`reified` rend ces opérations possibles à l’intérieur d’une fonction `inline`,
car le compilateur insère le type concret à l’endroit de l’appel :

```kotlin
inline fun <reified T> isOfType(value: Any): Boolean = value is T

val result = isOfType<String>("Kotlin")
```

Il est également possible d’obtenir des informations sur la classe :

```kotlin
inline fun <reified T> typeName(): String = T::class.simpleName.orEmpty()
```

La bibliothèque standard en fournit notamment l’exemple suivant :

```kotlin
val items: List<Any> = listOf("A", 1, "B")
val strings = items.filterIsInstance<String>()
```

Limitation : `reified` n’est disponible que pour les paramètres de type des
fonctions `inline` et ne fournit pas d’informations sur les arguments génériques
imbriqués tels que ceux de `List<String>`. Pour une API non `inline`, il faut
transmettre explicitement une `KClass`, une `Class` ou un sérialiseur.

**En bref :** `reified` donne à une fonction `inline` accès au type concret `T`
pour `is`, `T::class`, la réflexion et des API similaires, mais ne supprime pas
entièrement l’effacement des types.

</details>

<details>
<summary>36. Que sont les classes et les interfaces `sealed` ?</summary>

#### Kotlin

`sealed class` et `sealed interface` définissent une hiérarchie contrôlée de
sous-types directs. Elles conviennent à la représentation d’états, de résultats
et de commandes :

```kotlin
sealed interface UiState

data object Loading : UiState
data class Content(val users: List<User>) : UiState
data class Error(val message: String) : UiState
```

Le compilateur vérifie qu’une expression `when` traite tous les cas possibles :

```kotlin
fun render(state: UiState): String =
    when (state) {
        Loading -> "Loading"
        is Content -> "Users: ${state.users.size}"
        is Error -> "Error: ${state.message}"
    }
```

Les sous-types directs doivent être déclarés dans le même module et le même
package. Une `sealed class` peut posséder un constructeur, un état et une
implémentation commune. Une `sealed interface` ne possède pas de constructeur,
mais une classe peut implémenter plusieurs interfaces.

Contrairement à une `enum`, chaque variante `sealed` peut contenir son propre
ensemble de données. Si des modules externes doivent pouvoir étendre la
hiérarchie, `sealed` ne convient pas. Il est préférable de ne pas ajouter de
branche `else` dans un `when` sans nécessité, afin que le compilateur détecte
les nouveaux cas.

**En bref :** les types `sealed` modélisent un ensemble fermé de variantes
contenant des données différentes et permettent au compilateur de vérifier
l’exhaustivité d’un `when`.

</details>

<details>
<summary>37. Qu’est-ce qu’une expression `object` et quand l’utilise-t-on ?</summary>

#### Kotlin

Une expression `object` crée un objet anonyme au moment de son exécution. Elle
sert généralement à fournir une implémentation ponctuelle d’une interface ou
d’une classe :

```kotlin
val listener = object : ClickListener {
    override fun onClick() {
        println("Clicked")
    }
}
```

Il est également possible d’hériter d’une classe :

```kotlin
val repository = object : BaseRepository() {
    override fun refresh() {
        println("Custom refresh")
    }
}
```

Un objet anonyme sans supertype n’est pratique que localement ou dans une API
`private` :

```kotlin
val config = object {
    val host = "localhost"
    val port = 8080
}

println(config.host)
```

Son type anonyme ne peut pas être exposé comme un type public stable : seul le
supertype déclaré ou `Any` sera visible de l’extérieur. Une expression `object`
crée un nouvel objet à chaque exécution, tandis qu’une déclaration `object`
définit un singleton nommé.

Un tel objet peut capturer des variables extérieures. Si son implémentation
s’étoffe ou se répète, il est préférable de l’extraire dans une classe nommée.

**En bref :** une expression `object` convient à une courte implémentation locale,
à un listener ou à un stub de test. Une logique publique ou réutilisable nécessite
un type nommé.

</details>

<details>
<summary>38. Quelle est la différence entre les classes `enum` et les classes `sealed` ?</summary>

#### Kotlin

Une `enum class` contient un ensemble fixe de valeurs singleton du même type :

```kotlin
enum class OrderStatus(val isFinal: Boolean) {
    Draft(false),
    Paid(false),
    Shipped(true),
    Cancelled(true)
}
```

Une `sealed class` ou une `sealed interface` définit une hiérarchie fermée de
sous-types. Chaque variante peut posséder ses propres données :

```kotlin
sealed interface PaymentResult

data class Success(val transactionId: String) : PaymentResult
data class Failed(val reason: String) : PaymentResult
data object Cancelled : PaymentResult
```

Dans les deux cas, le compilateur vérifie l’exhaustivité d’une expression `when` :

```kotlin
fun render(result: PaymentResult): String =
    when (result) {
        is Success -> result.transactionId
        is Failed -> result.reason
        Cancelled -> "Cancelled"
    }
```

Une `enum` fournit les propriétés standard `entries`, `name` et `ordinal`. Un
type `sealed` ne possède pas de liste prédéfinie d’instances, mais permet de créer
plusieurs objets d’un même sous-type contenant des données différentes.

**En bref :** utilisez une `enum` pour des constantes simples partageant la même
structure, et un type `sealed` pour un ensemble fermé d’états ou de résultats
possédant des données différentes.

</details>

<details>
<summary>39. Quelle est la différence entre les initialisations `lateinit` et `lazy` ?</summary>

#### Kotlin

Ces deux mécanismes retardent l’initialisation, mais fonctionnent différemment.

`lateinit` permet d’attribuer une valeur à une propriété `var` non nullable après
la création de l’objet :

```kotlin
class UserController {
    lateinit var repository: UserRepository
}
```

Une lecture avant l’attribution lève une `UninitializedPropertyAccessException`.
`lateinit` ne fonctionne qu’avec une propriété `var` non nullable de type
référence, ne garantit pas la thread safety et autorise les réaffectations.

Il est possible de vérifier si la propriété a été initialisée :

```kotlin
class UserController {
    lateinit var repository: UserRepository

    fun isReady(): Boolean = ::repository.isInitialized
}
```

`lazy` calcule la valeur d’une propriété `val` lors de sa première lecture et
met en cache le résultat obtenu avec succès :

```kotlin
val config: Config by lazy {
    loadConfig()
}
```

Par défaut, `lazy` est synchronisé. Les modes `PUBLICATION` et `NONE` modifient
les garanties ; `NONE` n’est sûr que lorsque l’accès s’effectue depuis un seul
thread.

- Si du code externe ou un conteneur d’injection de dépendances attribue la
  valeur ultérieurement, utilisez `lateinit`.
- Si un initialiseur interne doit s’exécuter à la demande, utilisez `lazy`.
- Si l’absence de valeur constitue un état valide, utilisez un type nullable.

**En bref :** `lateinit` correspond à l’attribution externe différée d’une
propriété mutable ; `lazy` correspond au calcul différé et unique d’une propriété
immuable.

</details>

<details>
<summary>40. Qu’est-ce que le type `Nothing` et comment influence-t-il le flux d’exécution ?</summary>

#### Kotlin

`Nothing` est un type qui ne possède aucune valeur possible. Une fonction qui
renvoie ce type ne se termine jamais normalement :

```kotlin
fun fail(message: String): Nothing = throw IllegalStateException(message)
```

`Nothing` est un sous-type de tous les types ; il peut donc être utilisé dans
toute expression où l’exécution est interrompue :

```kotlin
val name: String = user.name ?: fail("Name is required")
val value: String = nullableValue
    ?: throw IllegalArgumentException("Value is required")
```

`throw` et `return` ont le type `Nothing` et peuvent donc faire partie d’une
expression Elvis :

```kotlin
fun handle(name: String?) {
    val value = name ?: return
    println(value.uppercase())
}
```

Ce type aide le compilateur à analyser le flux d’exécution et à effectuer des
smart casts :

```kotlin
fun printLength(value: String?) {
    if (value == null) fail("Value is null")
    println(value.length)
}
```

`Nothing?` ne possède qu’une seule valeur possible, `null` ; il s’agit du type
de l’expression `null` lorsqu’aucun autre contexte n’est disponible. Les
fonctions standard `error()` et `TODO()` renvoient également `Nothing`.

**En bref :** `Nothing` représente un chemin qui ne rend jamais le contrôle.
Grâce à lui, le compilateur peut déterminer les types avec davantage de précision
et analyser la suite du flux d’exécution.

</details>

<details>
<summary>41. Java peut-il appeler des fonctions Kotlin possédant des paramètres par défaut ?</summary>

#### Kotlin

Java ne prend pas en charge les paramètres par défaut de Kotlin. Sans surcharges
supplémentaires, tous les arguments doivent être fournis :

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

`@JvmOverloads` génère des surcharges faciles à utiliser depuis Java :

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

Les surcharges sont générées de droite à gauche pour les paramètres consécutifs
qui possèdent une valeur par défaut. L’annotation fonctionne également avec les
constructeurs :

```kotlin
class ApiClient @JvmOverloads constructor(
    val baseUrl: String,
    val timeoutSeconds: Int = 30
)
```

Cette annotation est inutile pour une API exclusivement destinée à Kotlin. Un
grand nombre de paramètres produit trop de surcharges ; dans ce cas, il est
préférable d’utiliser un objet de configuration ou un builder.

**En bref :** sans `@JvmOverloads`, Java ne peut appeler que la signature
complète. Ajoutez cette annotation uniquement aux API réellement utilisées
depuis Java.

</details>

<details>
<summary>42. Que sont les fonctions récursives terminales ?</summary>

#### Kotlin

Une fonction récursive terminale s’appelle elle-même lors de sa dernière
opération. Le modificateur `tailrec` permet au compilateur de transformer cet
appel en boucle sans faire croître la pile :

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

La variante suivante n’est pas récursive terminale, car une multiplication est
effectuée après l’appel récursif :

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L else n * factorial(n - 1)
```

L’optimisation ne fonctionne que pour la récursion directe. Elle ne s’applique
pas non plus lorsque l’appel se trouve dans une construction qui empêche la
transformation, par exemple un bloc `try`/`finally`. Si `tailrec` est utilisé de
manière incorrecte, le compilateur avertit que l’optimisation n’est pas effectuée.

**En bref :** `tailrec` permet d’écrire une récursion lisible sans risque de
`StackOverflowError`. Si une boucle ordinaire est plus simple, il est préférable
de l’utiliser.

</details>

<details>
<summary>43. Que sont les fonctions de portée (`let`, `run`, `apply`, `also`, `with`) et quand les utiliser ?</summary>

#### Kotlin

Les fonctions de portée exécutent un bloc dans le contexte d’un objet. Elles se
distinguent selon deux critères :

- la manière d’accéder à l’objet : `this` ou `it` ;
- la valeur renvoyée : le résultat de la lambda ou l’objet lui-même.

| Fonction | Objet accessible comme | Renvoie                |
| -------- | ---------------------- | ---------------------- |
| `let`    | `it`                   | le résultat de la lambda |
| `run`    | `this`                 | le résultat de la lambda |
| `apply`  | `this`                 | l’objet lui-même       |
| `also`   | `it`                   | l’objet lui-même       |
| `with`   | `this`                 | le résultat de la lambda |

Utilisations courantes :

- `let` — une transformation ou un bloc après un appel sécurisé ;
- `run` — le calcul d’un résultat avec un accès par `this` ;
- `apply` — la configuration d’un objet ;
- `also` — un effet de bord qui ne modifie pas la chaîne ;
- `with` — le regroupement d’opérations sur l’objet transmis.

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

`with` n’est pas une fonction d’extension : l’objet lui est transmis comme
premier argument. Une forte imbrication des fonctions de portée ou le mélange de
`this` et `it` nuit à la lisibilité ; dans ce cas, il est préférable d’utiliser
du code ordinaire ou une fonction nommée.

**En bref :** choisissez une fonction de portée selon le receveur (`this` ou
`it`) et la valeur renvoyée (l’objet ou le résultat de la lambda), et non par
habitude.

</details>

<details>
<summary>44. Quelle est la différence entre `let`, `run` et `apply` ?</summary>

#### Kotlin

Ces fonctions diffèrent par la manière d’accéder à l’objet et par la valeur
qu’elles renvoient :

| Fonction | Receveur dans le bloc | Renvoie                  |
| -------- | --------------------- | ------------------------ |
| `let`    | `it`                  | le résultat de la lambda |
| `run`    | `this`                | le résultat de la lambda |
| `apply`  | `this`                | l’objet lui-même         |

`let` convient à une transformation ou à une opération exécutée après un appel
sécurisé :

```kotlin
val length = name?.let { it.length }
```

`run` sert à calculer un résultat en accédant plusieurs fois à l’objet :

```kotlin
val fullName = user.run {
    "$firstName $lastName"
}
```

`apply` permet de configurer un objet tout en renvoyant ce même objet :

```kotlin
val request = Request().apply {
    method = "GET"
    url = "https://example.com"
}
```

N’utilisez pas de fonction de portée lorsqu’un appel ordinaire est plus lisible.
Il est préférable de remplacer les blocs imbriqués comportant plusieurs `this`
ou `it` par une fonction nommée.

**En bref :** `let` utilise `it` et renvoie un nouveau résultat ; `run` utilise
`this` et renvoie un nouveau résultat ; `apply` utilise `this` et renvoie l’objet
initial.

</details>

<details>
<summary>45. Quelle est la différence entre `map` et `flatMap` ?</summary>

#### Kotlin

`map` transforme chaque élément en un résultat :

```kotlin
val numbers = listOf(1, 2, 3)
val doubled = numbers.map { it * 2 } // [2, 4, 6]
```

Lorsque la transformation renvoie une collection, `map` crée une structure
imbriquée :

```kotlin
val words = listOf("ab", "cd")
val chars = words.map { it.toList() }
// [[a, b], [c, d]]
```

`flatMap` effectue la transformation, puis réunit les collections imbriquées en
une seule :

```kotlin
val chars = words.flatMap { it.toList() }
// [a, b, c, d]
```

Voici un exemple pratique :

```kotlin
data class User(val name: String, val roles: List<String>)

val allRoles = users.flatMap { it.roles }
```

Conceptuellement, les expressions suivantes sont équivalentes :

```kotlin
users.map { it.roles }.flatten()
users.flatMap { it.roles }
```

**En bref :** `map` correspond à `A -> B`. `flatMap` correspond à
`A -> Iterable<B>`, puis réunit tous les résultats dans une collection plate.

</details>

<details>
<summary>46. Qu’est-ce qu’une `Sequence` et quand faut-il l’utiliser ?</summary>

#### Kotlin

Une `Sequence` est une séquence synchrone évaluée paresseusement. Les opérateurs
intermédiaires ne traitent pas immédiatement les données, mais construisent un
pipeline :

```kotlin
val result = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Le calcul est déclenché par une opération terminale telle que `toList`, `first`,
`count`, `fold` ou `any`. Les éléments traversent le pipeline un par un ; les
opérations à court-circuit peuvent donc interrompre le traitement plus tôt :

```kotlin
val firstName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Une `Sequence` convient aux grands ensembles de données, aux longues chaînes de
`map` et `filter`, ainsi qu’aux opérations à court-circuit. Pour une petite
collection ou une transformation unique, son surcoût peut dépasser ses
avantages. Les opérations telles que `sorted` doivent malgré tout accumuler les
éléments.

Une `Sequence` n’est pas asynchrone et ne prend pas en charge la suspension. Pour
un flux de données asynchrone, on utilise `Flow`.

**En bref :** une `Sequence` réduit les collections intermédiaires et le travail
inutile dans les longs pipelines synchrones, mais elle n’est pas
automatiquement plus rapide que les collections ordinaires.

</details>

<details>
<summary>47. Qu’est-ce que le traitement paresseux des collections ?</summary>

#### Kotlin

Le traitement paresseux signifie que le pipeline est seulement décrit et qu’il
n’est exécuté qu’au moment d’une opération terminale. En Kotlin, on utilise une
`Sequence` à cet effet :

```kotlin
val names = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .take(10)
    .toList()
```

Les éléments traversent généralement les opérateurs un par un. Cela permet
d’interrompre le traitement plus tôt :

```kotlin
val firstActiveName = users
    .asSequence()
    .filter { it.isActive }
    .map { it.name }
    .firstOrNull()
```

Un pipeline paresseux ne met pas son résultat en cache : une nouvelle opération
terminale relance généralement le calcul. Il est également possible de travailler
avec des séquences potentiellement infinies si elles sont limitées par `take` :

```kotlin
val powersOfTwo = generateSequence(1) { it * 2 }
    .take(10)
    .toList()
```

Les opérateurs paresseux peuvent introduire leur propre surcoût, tandis que
`sorted` et les opérations similaires doivent malgré tout accumuler les données.
Pour un traitement asynchrone, il faut utiliser `Flow`, et non `Sequence`.

**En bref :** le traitement paresseux reporte le travail jusqu’à une opération
terminale et peut éviter des calculs inutiles. Il ne met pas le résultat en cache
et n’est pas toujours plus rapide que les collections évaluées immédiatement.

</details>

<details>
<summary>48. Que sont les coroutines et en quoi diffèrent-elles des threads ?</summary>

#### Kotlin

Une coroutine est une tâche légère qui peut suspendre son exécution et la
reprendre ultérieurement. Chaque coroutine ne crée pas son propre thread du
système d’exploitation :

```kotlin
scope.launch {
    val user = userRepository.loadUser()
    render(user)
}
```

Un thread est une ressource du système d’exploitation qui possède sa propre pile.
De nombreuses coroutines peuvent s’exécuter sur un petit nombre de threads et
passer de l’un à l’autre après une suspension.

```kotlin
delay(1_000)        // Suspend la coroutine
Thread.sleep(1_000) // Bloque le thread
```

Le mot-clé `suspend` ne garantit pas, à lui seul, une implémentation non
bloquante. Une API bloquante bloque toujours le thread et doit être exécutée dans
un contexte approprié.

Le dispatcher détermine les threads d’exécution :

- `Dispatchers.Main` — l’interface utilisateur ;
- `Dispatchers.IO` — les entrées-sorties bloquantes ;
- `Dispatchers.Default` — les tâches intensives en calcul.

La concurrence structurée rattache les coroutines à un `CoroutineScope` :
l’opération parente gère le cycle de vie, les erreurs et l’annulation des tâches
enfants.

```kotlin
viewModelScope.launch {
    val data = repository.loadData()
}
```

Les coroutines n’éliminent pas les conditions de concurrence et n’accélèrent pas
automatiquement les tâches intensives en calcul. Pour les calculs lourds, il faut
choisir explicitement le dispatcher approprié :

```kotlin
withContext(Dispatchers.Default) {
    heavyCpuWork()
}
```

**En bref :** les coroutines sont des tâches exécutées sur des threads, avec
suspension et concurrence structurée. Elles sont moins coûteuses que le modèle
« une tâche, un thread », mais ne dispensent pas de gérer correctement les
dispatchers et l’état partagé.

</details>

<details>
<summary>49. Qu’est-ce qu’une fonction `suspend` ?</summary>

#### Kotlin

Le mot-clé `suspend` indique qu’une fonction peut contenir des points de
suspension. Elle peut être appelée depuis une autre fonction `suspend` ou depuis
une coroutine :

```kotlin
suspend fun loadUser(id: Long): User = api.getUser(id)

viewModelScope.launch {
    val user = loadUser(1L)
}
```

`suspend` ne crée pas de coroutine, n’exécute pas la fonction en parallèle et ne
rend pas automatiquement non bloquant un code bloquant :

```kotlin
suspend fun readFile(path: String): String =
    withContext(Dispatchers.IO) {
        File(path).readText()
    }
```

Le compilateur transforme une fonction `suspend` en une machine à états utilisant
une `Continuation`, afin de conserver son état et de reprendre l’exécution après
une suspension.

L’annulation est coopérative : les points de suspension standard la vérifient,
tandis que les longues boucles intensives en calcul doivent le faire
explicitement :

```kotlin
while (hasWork) {
    coroutineContext.ensureActive()
    processNextItem()
}
```

**En bref :** `suspend` permet à une fonction de suspendre et de reprendre une
coroutine. Il ne garantit pas une implémentation non bloquante et ne crée pas de
nouvelle coroutine par lui-même.

</details>

<details>
<summary>50. Qu’est-ce qu’un `CoroutineScope` et à quoi sert-il ?</summary>

#### Kotlin

Un `CoroutineScope` est le propriétaire de coroutines et contient un
`coroutineContext`. Ce contexte comprend généralement un `Job`, un dispatcher et
des éléments supplémentaires :

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)
```

Le `Job` établit les relations parent-enfant et gère l’annulation. L’annulation
du scope entraîne celle de ses coroutines enfants :

```kotlin
scope.cancel()
```

Pour effectuer des opérations parallèles à l’intérieur d’une fonction `suspend`,
on utilise un constructeur de scope :

```kotlin
suspend fun loadScreenData() = coroutineScope {
    val user = async { loadUser() }
    val orders = async { loadOrders() }

    ScreenData(user.await(), orders.await())
}
```

`coroutineScope` ne se termine qu’après l’achèvement de toutes les tâches
enfants. Sur Android, `viewModelScope`, `lifecycleScope` et
`rememberCoroutineScope()` possèdent déjà un cycle de vie défini.

Un scope personnalisé ne doit être créé que pour un objet doté d’une méthode de
fermeture explicite qui appelle `cancel()`. `GlobalScope` ne possède aucun
propriétaire de ce type et enfreint la concurrence structurée ; il faut donc
l’éviter dans le code applicatif.

**En bref :** un `CoroutineScope` rattache les coroutines à un cycle de vie
explicite et permet de les annuler collectivement. Chaque scope doit avoir un
propriétaire.

</details>

<details>
<summary>51. Quelle est la différence entre `launch` et `async` ?</summary>

#### Kotlin

Ces deux constructeurs lancent une coroutine enfant dans un `CoroutineScope`.

`launch` renvoie un `Job` et s’utilise lorsqu’aucun résultat distinct n’est
nécessaire :

```kotlin
val job: Job = scope.launch {
    repository.syncData()
}
```

```kotlin
job.cancel()
job.join()
```

`async` renvoie un `Deferred<T>`. Le résultat ou l’erreur est obtenu au moyen de
`await()`. Son cas d’usage courant est l’exécution parallèle d’opérations
indépendantes :

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

Par défaut, `async` démarre immédiatement, et non lors de l’appel à `await()`.
L’erreur d’une coroutine enfant annule le scope parent ordinaire quel que soit le
constructeur utilisé ; avec `async`, `await()` relance également cette erreur.
Le comportement de supervision doit être configuré séparément.

N’utilisez pas `async` si vous ne prévoyez pas d’appeler `await()`, et n’exécutez
pas en parallèle des opérations qui dépendent l’une de l’autre.

**En bref :** `launch` représente une tâche sans valeur de résultat ; `async`
représente une tâche avec un `Deferred<T>`, dont le résultat est obtenu au moyen
de `await()`.

</details>

<details>
<summary>52. Qu’est-ce que `runBlocking` et quand l’utilise-t-on ?</summary>

#### Kotlin

`runBlocking` crée un `CoroutineScope` et bloque le thread courant jusqu’à ce que
le bloc et toutes ses coroutines enfants soient terminés :

```kotlin
fun main() {
    runBlocking {
        val user = repository.loadUser()
        println(user)
    }
}
```

Il s’agit d’un outil de frontière permettant d’appeler du code `suspend` depuis
une API synchrone. Il peut être approprié pour faire le lien avec du code ancien
ou dans un court exemple. Pour les tests de coroutines, il faut utiliser
`runTest`.

Dans le code d’interface utilisateur, `runBlocking` bloque le thread principal
et peut provoquer un gel de l’application, une ANR ou un interblocage :

```kotlin
fun onButtonClick() {
    runBlocking {
        repository.loadData()
    }
}
```

Il faut plutôt lancer une coroutine dans un scope possédant le cycle de vie
approprié :

```kotlin
fun onButtonClick() {
    viewModelScope.launch {
        val data = repository.loadData()
        updateState(data)
    }
}
```

Dans une fonction `suspend`, `runBlocking` est inutile et ne fait que bloquer le
thread :

```kotlin
suspend fun loadUser(): User = repository.loadUser()
```

**En bref :** `runBlocking` est un pont synchrone vers du code `suspend`. Ne
l’utilisez pas à l’intérieur de coroutines, de fonctions `suspend` ou sur le
thread de l’interface utilisateur.

</details>

<details>
<summary>53. Que sont les dispatchers (`Dispatchers.IO`, `Default`, `Main`) ?</summary>

#### Kotlin

Un `CoroutineDispatcher` planifie l’exécution d’une coroutine sur les threads
appropriés :

- `Dispatchers.Main` — l’interface utilisateur et les opérations courtes sur le
  thread principal ;
- `Dispatchers.IO` — les entrées-sorties bloquantes ;
- `Dispatchers.Default` — les calculs intensifs en processeur.

Une opération de fichier bloquante est déplacée vers `IO` :

```kotlin
suspend fun readConfig(): Config =
    withContext(Dispatchers.IO) {
        file.readText().toConfig()
    }
```

Un calcul lourd est déplacé vers `Default` :

```kotlin
suspend fun calculateHash(bytes: ByteArray): String =
    withContext(Dispatchers.Default) {
        expensiveHash(bytes)
    }
```

`withContext` suspend la coroutine courante, exécute le bloc dans le contexte
indiqué, puis renvoie son résultat. Il préserve la concurrence structurée,
l’annulation et la propagation des erreurs.

Tous les appels réseau ou de base de données ne nécessitent pas
`Dispatchers.IO` : une API `suspend` peut déjà être utilisable sans risque sur le
thread principal. Le dispatcher doit être choisi par la couche qui sait si
l’implémentation bloque le thread. Pour faciliter les tests, il peut être fourni
comme dépendance :

```kotlin
class FileRepository(
    private val ioDispatcher: CoroutineDispatcher
) {
    suspend fun load(): Data =
        withContext(ioDispatcher) { blockingLoad() }
}
```

Il est inutile de changer de contexte pour une tâche minime, car cette opération
a également un coût.

**En bref :** `Main` est destiné à l’interface utilisateur, `IO` aux
entrées-sorties bloquantes et `Default` aux calculs lourds. `withContext` change
le contexte d’une opération précise sans créer une nouvelle coroutine
indépendante.

</details>

<details>
<summary>54. Comment annuler des coroutines et gérer les erreurs ?</summary>

#### Kotlin

L’annulation des coroutines est coopérative et se gère au moyen d’un `Job` :

```kotlin
val job = scope.launch {
    repository.sync()
}

job.cancelAndJoin()
```

Les fonctions `suspend` telles que `delay()` réagissent automatiquement à
l’annulation. Le code intensif en calcul doit vérifier régulièrement son état :

```kotlin
scope.launch(Dispatchers.Default) {
    while (hasWork) {
        ensureActive()
        doSmallChunkOfWork()
    }
}
```

L’annulation est signalée par une `CancellationException`. Si un bloc `catch`
trop général l’intercepte, elle doit être relancée :

```kotlin
try {
    repository.loadData()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

Dans le cadre de la concurrence structurée ordinaire, une erreur non gérée dans
une coroutine enfant annule le parent et les tâches sœurs. Les erreurs attendues
sont traitées localement, puis converties en résultat métier ou en état de
l’interface utilisateur :

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

Pour des tâches indépendantes, on utilise `supervisorScope` ou `SupervisorJob` :

```kotlin
supervisorScope {
    launch { loadA() }
    launch { loadB() }
}
```

Un superviseur ne traite pas l’erreur ; il se contente d’isoler les tâches
sœurs. `CoroutineExceptionHandler` est destiné au dernier niveau de traitement
des exceptions non interceptées dans un `launch` racine, par exemple pour les
journaliser. Les erreurs de `async` sont obtenues au moyen de `await()`.

**En bref :** annulez les coroutines au moyen de leur `Job`, n’ignorez pas les
`CancellationException`, traitez localement les erreurs attendues et isolez les
tâches indépendantes grâce à la supervision.

</details>

<details>
<summary>55. Qu’est-ce que la concurrence structurée ?</summary>

#### Kotlin

La concurrence structurée signifie que chaque coroutine possède un propriétaire
et appartient à une hiérarchie de `Job`. Le parent attend ses tâches enfants et
gère leur annulation ainsi que leurs erreurs.

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

`coroutineScope` ne se termine pas avant ses coroutines enfants. L’annulation de
l’appelant les annule également, tandis qu’une erreur non gérée dans l’une des
tâches annule le scope et les tâches sœurs.

Pour des tâches indépendantes, on utilise la supervision :

```kotlin
supervisorScope {
    launch { loadProfile() }
    launch {
        loadRecommendations()
    }
}
```

L’erreur d’un enfant direct n’annule pas les autres tâches enfants, mais elle
doit malgré tout être traitée. `supervisorScope` s’utilise localement, tandis que
`SupervisorJob` convient à un scope de longue durée.

`GlobalScope` enfreint ce modèle : l’appelant ne peut pas attendre correctement
la fin du travail, l’annuler ou recevoir son erreur. Un scope personnalisé doit
posséder un cycle de vie clairement défini et un nettoyage explicite.

**En bref :** la concurrence structurée empêche une tâche enfant de survivre
silencieusement à son propriétaire. Les tâches liées utilisent
`coroutineScope`, tandis que les tâches indépendantes utilisent la supervision.

</details>

<details>
<summary>56. Les coroutines peuvent-elles s’exécuter sur n’importe quel thread ?</summary>

#### Kotlin

Une coroutine ne possède pas son propre thread attitré. Le
`CoroutineDispatcher` détermine où s’exécutent ses différentes parties :

```kotlin
scope.launch(Dispatchers.Main) {
    // Thread de l’interface utilisateur
}

scope.launch(Dispatchers.IO) {
    // Entrées-sorties bloquantes
}

scope.launch(Dispatchers.Default) {
    // Tâche intensive en calcul
}
```

- `Main` — l’interface utilisateur ;
- `IO` — les entrées-sorties bloquantes ;
- `Default` — les tâches intensives en calcul.

Avec un dispatcher reposant sur un pool, une coroutine peut reprendre après une
suspension sur un autre thread du même dispatcher :

```kotlin
withContext(Dispatchers.IO) {
    println(Thread.currentThread().name)
    delay(100)
    println(Thread.currentThread().name)
}
```

`withContext` change temporairement le contexte, puis rétablit celui de
l’appelant à la fin du bloc. `Dispatchers.Main` garantit l’exécution sur le
thread de l’interface utilisateur ; les dispatchers reposant sur un pool ne
garantissent pas un identifiant de thread précis.

Un `ThreadLocal` ordinaire ne suit pas automatiquement une coroutine lorsqu’elle
change de thread. Si nécessaire, sa valeur doit être ajoutée au contexte de la
coroutine :

```kotlin
val requestId = ThreadLocal<String>()

withContext(requestId.asContextElement("request-42")) {
    callApi()
}
```

Un dispatcher personnalisé doté de ses propres threads doit avoir un propriétaire
chargé de le fermer. Un appel bloquant bloque le thread, même s’il est exécuté
dans une coroutine.

**En bref :** une coroutine s’exécute sur les threads de son dispatcher et peut
changer de thread physique après une suspension. Le dispatcher doit garantir
toute éventuelle affinité avec un thread précis.

</details>

<details>
<summary>57. Qu’est-ce qu’un `Flow` en Kotlin ?</summary>

#### Kotlin

Un `Flow<T>` est un flux asynchrone de valeurs reposant sur les coroutines. Une
fonction `suspend` renvoie une seule valeur, tandis qu’un `Flow` renvoie une
séquence de valeurs au fil du temps :

```kotlin
suspend fun loadUser(): User
fun observeUser(): Flow<User>
```

Un Flow est créé au moyen d’un constructeur et démarré par un opérateur terminal,
par exemple `collect` :

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

Un Flow créé avec `flow {}` est froid : le producteur démarre séparément pour
chaque collecteur. `emit()` est une fonction `suspend` ; sans mise en mémoire
tampon explicite, le producteur ne devance donc pas un collecteur lent.

Les opérateurs permettent de construire un pipeline :

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .filter { it.isNotEmpty() }
    .collect { users -> render(users) }
```

`flowOn` modifie le contexte des opérateurs situés en amont :

```kotlin
fun observeData(): Flow<Data> =
    flow {
        emit(blockingApi.loadData())
    }.flowOn(Dispatchers.IO)
```

`catch` ne traite que les erreurs provenant de l’amont ; il n’intercepte pas
celles du collecteur. L’annulation du collecteur annule la collecte et le
producteur dans le cadre de la concurrence structurée.

**En bref :** utilisez une fonction `suspend` pour un résultat unique et un
`Flow` pour des valeurs émises au fil du temps qui nécessitent un pipeline
asynchrone.

</details>

<details>
<summary>58. Quelle est la différence entre `Flow`, `StateFlow` et `SharedFlow` ?</summary>

#### Kotlin

```text
Flow       -> généralement un pipeline froid
StateFlow  -> un flux chaud contenant l’état courant
SharedFlow -> un broadcast chaud avec une politique de replay et de tampon
```

Un `Flow` créé avec `flow {}` démarre le producteur séparément pour chaque
collecteur et ne conserve pas de valeur courante :

```kotlin
fun loadUsers(): Flow<List<User>> = flow {
    emit(api.loadUsers())
}
```

Un `StateFlow` possède toujours une valeur initiale. Un nouveau collecteur reçoit
immédiatement l’état courant, tandis que les valeurs égales selon `equals()` ne
sont pas émises à nouveau :

```kotlin
private val mutableState = MutableStateFlow<UiState>(UiState.Loading)

val state: StateFlow<UiState> = mutableState.asStateFlow()
```

La version mutable reste `private` ; pour une mise à jour concurrente, on utilise
`update { }`.

Un `SharedFlow` transmet les valeurs à tous les collecteurs actifs sans être
obligé de posséder une valeur courante :

```kotlin
private val mutableEvents = MutableSharedFlow<UiEvent>(replay = 0)

val events = mutableEvents.asSharedFlow()
```

`replay` détermine le nombre de valeurs récentes reçues par un nouveau
collecteur ; `extraBufferCapacity` et `onBufferOverflow` contrôlent le tampon.
Avec `replay = 0`, une valeur émise sans abonné est perdue. Les données critiques
doivent donc être modélisées comme un état plutôt que comme un événement ponctuel.

Un Flow froid peut être converti en flux chaud partagé : `stateIn()` crée un
`StateFlow`, tandis que `shareIn()` crée un `SharedFlow`. `SharingStarted`
détermine quand la source partagée démarre et s’arrête.

**En bref :** `Flow` est un pipeline paresseux indépendant, `StateFlow`
représente l’état courant et `SharedFlow` est un flux d’événements partagé avec
un replay configurable.

</details>

<details>
<summary>59. Quelle est la différence entre `Flow` et `LiveData` ?</summary>

#### Kotlin

`LiveData` est un conteneur Android qui tient compte du cycle de vie, tandis que
`Flow` est un flux fondé sur les coroutines et indépendant d’Android.

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`LiveData` ne notifie que les observateurs actifs et leur transmet à nouveau la
dernière valeur lorsqu’ils redeviennent actifs. Il est pratique pour les
interfaces View anciennes, mais ne doit pas être introduit dans la couche métier.

`Flow` prend en charge l’annulation des coroutines, les erreurs et un vaste
ensemble d’opérateurs :

```kotlin
repository.observeUsers()
    .map { users -> users.filter { it.isActive } }
    .combine(settingsFlow, ::buildUiState)
    .catch { emit(UiState.Error) }
```

Un `Flow` ordinaire est généralement froid et ne conserve pas de valeur courante.
Pour l’état de l’interface utilisateur, on utilise `StateFlow`. Le cycle de vie
doit être pris en compte explicitement lors de la collecte d’un Flow :

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Avec Compose, `collectAsStateWithLifecycle()` remplit ce rôle. `LiveData` est
conçu pour le thread principal, tandis que `Flow` utilise le contexte des
coroutines et `flowOn` pour les opérations en amont.

Lors d’une migration, ces types peuvent être convertis :

```kotlin
val liveData = userFlow.asLiveData()
val flow = userLiveData.asFlow()
```

**En bref :** `LiveData` tient automatiquement compte du cycle de vie Android.
`Flow` est indépendant d’Android et convient mieux aux couches données et
métier ; dans l’interface utilisateur, il doit être collecté en tenant compte du
cycle de vie, tandis que l’état courant est conservé dans un `StateFlow`.

</details>

<details>
<summary>60. Quelle est la différence entre les flux froids et les flux chauds ?</summary>

#### Kotlin

La différence réside dans la relation entre le producteur et les collecteurs :

```text
froid -> un producteur distinct démarre pour chaque collecteur
chaud -> les collecteurs reçoivent les données d’une source partagée
```

Un Flow créé avec `flow {}` est froid. Il ne s’exécute pas avant l’appel à
`collect`, et chaque collecteur lance le bloc séparément :

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

L’opération est donc exécutée deux fois. Chaque collecteur possède son propre
cycle de vie et sa propre annulation.

`StateFlow` et `SharedFlow` sont chauds : leur objet et leurs données existent
indépendamment d’un collecteur particulier.

```kotlin
val state = MutableStateFlow<UiState>(UiState.Loading)
val events = MutableSharedFlow<UiEvent>(replay = 0)
```

`StateFlow` conserve l’état courant. `SharedFlow` diffuse les valeurs aux
collecteurs actifs et peut conserver les dernières valeurs au moyen de `replay`.

Un Flow froid est converti en flux chaud partagé avec `stateIn()` ou `shareIn()`.
La politique `SharingStarted` détermine quand la source partagée est active, mais
tous les collecteurs utilisent malgré tout la même exécution.

**En bref :** un flux froid démarre un producteur pour chaque collecteur ; un
flux chaud possède une source partagée et diffuse ses valeurs à tous les
collecteurs.

</details>

<details>
<summary>61. Qu’est-ce que `collectLatest` et quand faut-il l’utiliser ?</summary>

#### Kotlin

`collectLatest` est un opérateur terminal qui annule le traitement de la valeur
précédente lorsqu’une nouvelle valeur arrive :

```kotlin
flowOf(1, 2, 3).collectLatest { value ->
    delay(1_000)
    println(value) // Seul 3 aura le temps d’être affiché
}
```

Un cas d’usage courant est la recherche, où la requête précédente devient
obsolète :

```kotlin
searchQueryFlow
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        val result = repository.search(query)
        state.value = UiState.Success(result)
    }
```

L’annulation est coopérative. Les API `suspend` y réagissent, contrairement au
code bloquant :

```kotlin
collectLatest {
    Thread.sleep(5_000)
}
```

N’utilisez pas `collectLatest` si chaque valeur doit obligatoirement être
traitée, par exemple pour des paiements, des messages, des sauvegardes ou des
événements d’analyse.

`flatMapLatest` annule le Flow interne précédent, tandis que `collectLatest`
annule le bloc du collecteur.

**En bref :** utilisez `collect` lorsque chaque valeur compte, et
`collectLatest` lorsque le traitement précédent devient inutile dès qu’une
nouvelle valeur arrive.

</details>

<details>
<summary>62. Comment convertir une API fondée sur des callbacks en fonction `suspend` ?</summary>

#### Kotlin

Un callback produisant un résultat unique est encapsulé dans
`suspendCancellableCoroutine` :

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

- `resume()` renvoie le résultat ;
- `resumeWithException()` renvoie une erreur ;
- `invokeOnCancellation` annule l’opération externe ou supprime le listener.

L’annulation de la coroutine n’arrête pas automatiquement l’API à callbacks. Une
continuation ne peut être terminée qu’une seule fois ; si le callback peut être
déclenché plusieurs fois ou simultanément, il faut utiliser
`tryResume()`/`completeResume()` ou une protection atomique.

Pour un callback produisant plusieurs valeurs, on utilise `callbackFlow` :

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

`awaitClose` supprime le listener lors de l’annulation ou de la fermeture du
Flow. Le résultat de `trySend()` et la politique du tampon doivent être gérés en
fonction des garanties de livraison requises.

**En bref :** utilisez `suspendCancellableCoroutine` pour un résultat unique et
`callbackFlow` pour un flux de valeurs. Dans les deux cas, implémentez
obligatoirement le nettoyage lors de l’annulation.

</details>

<details>
<summary>63. Quelle est la différence entre `Channel` et `Flow` ?</summary>

#### Kotlin

Un `Channel<T>` est une file asynchrone destinée à la communication entre
coroutines :

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

Le producteur appelle `send()`, tandis que le consommateur appelle `receive()` ou
lit les valeurs avec une boucle `for`. Chaque valeur est reçue par un seul
consommateur ; un Channel convient donc aux files de tâches et à la distribution
du travail. Sa capacité, sa fermeture et son annulation doivent être gérées
explicitement.

Un `Flow<T>` est une API déclarative représentant un flux asynchrone :

```kotlin
fun numbers(): Flow<Int> = flow {
    emit(1)
    emit(2)
}

numbers().collect { value ->
    println(value)
}
```

Flow prend en charge `map`, `filter`, `combine`, l’annulation et la concurrence
structurée. Un Flow créé avec `flow {}` est généralement froid ; un Channel est
chaud et, selon sa capacité, peut recevoir des valeurs sans consommateur actif.

Dans un Channel, la contre-pression dépend du tampon :

```kotlin
val channel = Channel<Int>(capacity = 0)
channel.send(1) // Attend un récepteur
```

- `RENDEZVOUS` — aucun tampon ;
- `BUFFERED` — un tampon limité ;
- `CONFLATED` — seule la dernière valeur est conservée ;
- `UNLIMITED` — un tampon illimité présentant un risque de croissance de la
  mémoire.

Avec Flow, on utilise `buffer()`, `conflate()` ou `collectLatest()`.

Un Channel peut être représenté sous forme de Flow :

```kotlin
val events: Flow<UiEvent> = channel.receiveAsFlow()
```

`receiveAsFlow()` ne crée pas de broadcast : les collecteurs se répartissent les
valeurs. Pour un état, utilisez `StateFlow` ; pour un broadcast, `SharedFlow` ;
et pour adapter une API à callbacks, `callbackFlow`.

**En bref :** `Channel` est une file producteur-consommateur, tandis que `Flow`
est un flux de données déclaratif. Pour les flux applicatifs, commencez par Flow
et réservez Channel au transfert explicite de tâches entre coroutines.

</details>

<details>
<summary>64. Quels sont les principaux opérateurs de Flow (`map`, `filter`, `combine`, `zip`) ?</summary>

#### Kotlin

`map` transforme chaque valeur :

```kotlin
val names: Flow<String> = usersFlow.map { it.name }
```

`filter` ne laisse passer que les valeurs qui satisfont la condition :

```kotlin
val positiveNumbers = numbersFlow.filter { it > 0 }
```

`combine` attend la première valeur de chaque Flow, puis émet un nouveau résultat
dès que l’un d’eux est mis à jour :

```kotlin
val uiState = combine(userFlow, settingsFlow) { user, settings ->
    ProfileState(user = user, theme = settings.theme)
}
```

`zip` associe les valeurs deux à deux dans leur ordre d’arrivée :

```kotlin
flowOf(1, 2, 3)
    .zip(flowOf("A", "B", "C")) { number, letter ->
        "$number$letter"
    }
```

Le résultat est `1A`, `2B`, `3C`. `zip` attend la valeur correspondante pour
former une paire, tandis que `combine` utilise les dernières valeurs disponibles.
On utilise donc généralement `combine` pour construire un état à partir de
plusieurs sources, et `zip` pour associer des éléments par paires.

**En bref :** `map` transforme, `filter` élimine, `combine` réagit aux dernières
valeurs de tous les flux et `zip` associe les valeurs deux à deux.

</details>

<details>
<summary>65. Quelle est la différence entre `combine` et `zip` ?</summary>

#### Kotlin

Ces deux opérateurs réunissent plusieurs Flow, mais leur sémantique diffère :

```text
combine -> une nouvelle valeur + les dernières valeurs des autres Flow
zip     -> la première avec la première, la deuxième avec la deuxième
```

`combine` attend la première valeur de chaque source en amont, puis réagit à
chaque mise à jour :

```kotlin
val state = combine(userFlow, settingsFlow) { user, settings ->
    UiState.Content(user, settings)
}
```

`zip` forme des paires selon l’ordre des valeurs :

```kotlin
flowOf("A", "B", "C")
    .zip(flowOf(1, 2)) { letter, number -> letter + number }
    // A1, B2
```

Avec `zip`, la source la plus rapide attend la valeur correspondante. Le flux
résultant se termine lorsqu’aucune nouvelle paire complète ne peut être formée.
Avec `combine`, la dernière valeur d’une source terminée peut continuer à être
utilisée tant que les autres sources émettent des valeurs.

**En bref :** utilisez `combine` pour un état mis à jour par n’importe quelle
source, et `zip` pour associer deux séquences élément par élément.

</details>

<details>
<summary>66. À quoi servent `@JvmStatic`, `@JvmOverloads` et `@JvmField` ?</summary>

#### Kotlin

Ces annotations modifient l’API JVM exposée au code Java.

- `@JvmStatic` génère un pont statique pour un membre d’un `object` ou d’un
  `companion object`.
- `@JvmOverloads` génère des surcharges pour les paramètres consécutifs possédant
  une valeur par défaut, en commençant par la droite.
- `@JvmField` expose directement le champ sous-jacent, sans getter ni setter.

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

Java pourra appeler `UserFactory.create()`, lire `UserFactory.defaultName` et
utiliser la surcharge sans l’argument `active`.

`@JvmField` ne fonctionne pas avec une propriété déléguée ou des accesseurs
personnalisés, et réduit l’encapsulation. Pour les constantes connues à la
compilation de type primitif ou `String`, il est préférable d’utiliser
`const val`.

**En bref :** ces annotations ne sont nécessaires que pour rendre une API plus
pratique à utiliser depuis Java ou un framework. Il est inutile de les ajouter
sans raison dans du code exclusivement Kotlin.

</details>

<details>
<summary>67. Qu’est-ce que la délégation ?</summary>

#### Kotlin

La délégation confie l’implémentation à un autre objet plutôt que d’utiliser
l’héritage. Kotlin prend en charge la délégation d’interface au moyen de `by` :

```kotlin
interface Logger {
    fun log(message: String)
}

class UserService(
    logger: Logger
) : Logger by logger
```

Le compilateur génère les méthodes de transmission. Une méthode particulière
peut être redéfinie :

```kotlin
class PrefixLogger(
    private val delegate: Logger
) : Logger by delegate {
    override fun log(message: String) {
        delegate.log("[App] " + message)
    }
}
```

Les propriétés peuvent également posséder un délégué :

```kotlin
val config: Config by lazy { loadConfig() }

var age: Int by Delegates.vetoable(0) { _, _, newValue ->
    newValue >= 0
}
```

Un délégué de propriété personnalisé implémente `getValue()` et, pour une
propriété `var`, également `setValue()`.

**En bref :** la délégation de classe élimine le code répétitif de transmission
et favorise la composition ; la délégation de propriété déplace la logique de
lecture et d’écriture d’une propriété dans un objet distinct.

</details>

<details>
<summary>68. Quels frameworks de test sont disponibles pour Kotlin ?</summary>

#### Kotlin

Le choix dépend du niveau de test :

- `kotlin.test` — des annotations et assertions multiplateformes ;
- JUnit — des tests unitaires sur la JVM ;
- Kotest — un DSL et des matchers alternatifs ;
- MockK ou Mockito — des doublures de test ;
- `kotlinx-coroutines-test` — les coroutines, les dispatchers de test et le temps
  virtuel ;
- Turbine — la vérification des émissions d’un Flow ;
- AndroidX Test, Espresso et Compose UI Test — les tests Android et d’interface
  utilisateur ;
- Robolectric — certains scénarios Android exécutés sur la JVM.

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals("Alex", user.name)
}
```

Pour la logique métier, JUnit ou `kotlin.test` et des fakes écrits manuellement
suffisent souvent. L’outil ne remplace pas le choix du bon niveau de test : les
comportements propres à un appareil doivent être vérifiés par des tests
d’instrumentation.

**En bref :** utilisez JUnit ou `kotlin.test` pour les tests unitaires,
coroutines-test pour les coroutines, Turbine pour Flow, et Espresso ou Compose
UI Test pour l’interface Android.

</details>

<details>
<summary>69. Comment simuler des dépendances dans les tests ?</summary>

#### Kotlin

La dépendance est transmise par le constructeur, puis remplacée par un mock, un
stub ou un fake.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Exemple avec MockK pour une fonction `suspend` :

```kotlin
val repository = mockk<AuthRepository>()

coEvery {
    repository.login("a@b.com", "123")
} returns User("1")

coVerify {
    repository.login("a@b.com", "123")
}
```

Exemple de fake écrit manuellement :

```kotlin
class FakeAuthRepository : AuthRepository {
    var result: Result<User> = Result.success(User("1"))

    override suspend fun login(
        email: String,
        password: String
    ): Result<User> = result
}
```

Un fake convient mieux à la vérification du comportement, tandis qu’un mock est
utile lorsqu’une interaction fait partie du contrat. Il est déconseillé de
simuler les objets-valeurs, les fonctions pures et chaque appel interne : ces
tests deviennent trop dépendants de l’implémentation.

**En bref :** utilisez l’injection par constructeur et privilégiez un fake simple.
Réservez les mocks aux interactions importantes avec une dépendance externe.

</details>

<details>
<summary>70. Comment tester les coroutines et les Flow ?</summary>

#### Kotlin

Pour les coroutines, on utilise `kotlinx-coroutines-test` et `runTest` :

```kotlin
@Test
fun loadsUser() = runTest {
    val user = repository.loadUser()
    assertEquals(expectedUser, user)
}
```

`runTest` fournit un temps virtuel et un `TestCoroutineScheduler` :

- `runCurrent()` exécute les tâches prévues à l’instant courant ;
- `advanceTimeBy()` fait avancer le temps virtuel ;
- `advanceUntilIdle()` exécute la file jusqu’à ce qu’elle soit inactive.

Il est préférable de fournir les dispatchers comme dépendances. Dans les tests
unitaires locaux, `Dispatchers.Main` est remplacé par un dispatcher de test,
puis restauré au moyen de `resetMain()`.

Turbine permet de vérifier facilement l’ordre des émissions d’un Flow :

```kotlin
viewModel.state.test {
    assertEquals(UiState.Loading, awaitItem())

    viewModel.load()

    assertEquals(UiState.Content(user), awaitItem())
    cancelAndIgnoreRemainingEvents()
}
```

Pour vérifier uniquement l’état final d’un `StateFlow`, il suffit souvent
d’exécuter les tâches en attente puis de contrôler `state.value`. N’utilisez pas
`Thread.sleep()`, de délais réels ni plusieurs planificateurs de test dans un
même test.

**En bref :** utilisez `runTest` et un dispatcher de test pour les coroutines,
Turbine pour vérifier l’ordre des émissions d’un Flow, et `StateFlow.value` pour
contrôler l’état final.

</details>

<details>
<summary>71. Quelles sont les bonnes pratiques pour écrire du code testable ?</summary>

#### Kotlin

Un code testable possède des dépendances explicites et un comportement
prévisible.

```kotlin
class LoginUseCase(
    private val repository: AuthRepository,
    private val clock: Clock
)
```

Les principales règles sont les suivantes :

- utiliser l’injection par constructeur plutôt que créer les dépendances dans la
  classe ;
- attribuer une responsabilité claire à chaque classe ;
- employer des fonctions pures pour les calculs métier ;
- créer des abstractions pour le temps, les entrées-sorties, les API, les bases
  de données et les dispatchers ;
- limiter l’utilisation du framework Android dans la logique métier ;
- préférer les fakes à un recours excessif aux mocks ;
- vérifier le comportement public plutôt que les méthodes privées et les appels
  internes.

Une ViewModel est testée à travers son état et ses effets observables :

```kotlin
viewModel.onLoginClick()
advanceUntilIdle()

assertEquals(LoginState.Success, viewModel.state.value)
```

Il n’est pas nécessaire de créer une interface pour chaque classe. Une
abstraction est justifiée à la frontière d’une dépendance variable ou externe.

**En bref :** des dépendances explicites, des effets de bord contrôlés et de
petites responsabilités rendent le code testable sans lier les tests aux détails
d’implémentation.

</details>

<details>
<summary>72. Comment gérer efficacement les dépendances dans un projet Kotlin ?</summary>

#### Kotlin

Dans un projet Gradle, il faut contrôler les versions, les portées et le sens des
dépendances entre les modules.

- Un catalogue de versions centralise les alias et les versions dans
  `libs.versions.toml`.
- `implementation` masque la dépendance aux modules en aval.
- `api` l’expose comme une partie de l’API publique et ne doit être utilisé qu’en
  cas de nécessité.
- `testImplementation` et `androidTestImplementation` isolent les dépendances de
  test.
- Une BOM harmonise les versions des artefacts d’un même écosystème.
- Les plugins de convention éliminent la duplication de la configuration Gradle.

```kotlin
dependencies {
    implementation(libs.coroutines.core)
    testImplementation(libs.junit)
}
```

Un module doit dépendre du contrat dont il a besoin, et non de l’ensemble de
l’implémentation. Les dépendances Gradle et l’injection de dépendances à
l’exécution se situent à des niveaux différents : Gradle détermine la
disponibilité du code, tandis que l’injection de dépendances crée les objets à
l’exécution.

Pour établir un diagnostic :

```bash
./gradlew :app:dependencies
./gradlew :app:dependencyInsight --dependency kotlinx-coroutines-core
```

Les mises à jour doivent être effectuées par petites étapes, en vérifiant le
changelog, le build et les tests.

**En bref :** centralisez les versions, privilégiez `implementation`, contrôlez
le graphe des modules et vérifiez régulièrement le graphe des dépendances.

</details>

<details>
<summary>73. Comment Kotlin gère-t-il la mémoire et le ramasse-miettes ?</summary>

#### Kotlin

Le modèle de mémoire dépend de la plateforme. Sur la JVM et Android, Kotlin
utilise la gestion de la mémoire de la JVM ou d’ART : les objets résident dans le
tas, et le ramasse-miettes libère ceux auxquels aucune référence accessible ne
mène plus.

```kotlin
fun createUser() {
    val user = User("1", "Alex")
}
```

Après la fin de la fonction, l’objet peut être collecté si plus rien ne le
conserve. Affecter `null` ne fait que supprimer une référence ; le moment du
nettoyage est déterminé par le ramasse-miettes.

Une fuite de mémoire se produit lorsqu’un objet devenu inutile reste accessible :

```kotlin
object Holder {
    var activity: Activity? = null // Peut conserver une Activity
}
```

Sur Android, les causes courantes sont une Activity ou une View conservée dans
un singleton, des listeners ou un binding non libérés, ainsi qu’une coroutine
dont le cycle de vie est plus long. Les allocations fréquentes, le boxing, les
closures et les collections intermédiaires peuvent augmenter la pression sur le
ramasse-miettes, mais ne doivent être optimisés qu’après un profilage.

Kotlin/Native et Kotlin/JS utilisent les mécanismes de mémoire propres à leur
plateforme ; les particularités de la JVM ne peuvent donc pas être généralisées
à toutes les cibles.

**En bref :** sur la JVM et Android, le ramasse-miettes libère la mémoire après
la disparition de toutes les références accessibles. Le principal risque n’est
pas `null`, mais une référence de longue durée vers un objet devenu inutile.

</details>

<details>
<summary>74. Quelles sont les bonnes pratiques en matière de null safety ?</summary>

#### Kotlin

Un type nullable doit représenter une véritable possibilité d’absence de valeur,
et non être utilisé « au cas où ».

```kotlin
data class User(
    val id: String,
    val middleName: String?
)
```

Les principales règles sont les suivantes :

- éviter `!!` et utiliser `?.`, `?:` ou un retour anticipé ;
- employer `requireNotNull` pour un contrat d’entrée et `checkNotNull` pour un
  état interne ;
- vérifier les platform types Java à la frontière entre Java et Kotlin ;
- convertir les DTO nullables en modèles métier stricts dans un mapper ;
- si `null` représente un état distinct, le modéliser au moyen d’un type
  `sealed`.

```kotlin
val userId = requireNotNull(dto.id) { "User id is required" }
val user = state.user ?: return
```

Une valeur de remplacement ne doit pas masquer une erreur : si l’absence de
valeur enfreint le contrat, il est préférable d’interrompre explicitement
l’exécution.

**En bref :** modélisez l’absence d’une valeur au moyen de `T?`, traitez-la aux
frontières et ne transformez pas le code nullable en une chaîne de `!!`.

</details>

<details>
<summary>75. Qu’est-ce que `supervisorScope` et en quoi diffère-t-il ?</summary>

#### Kotlin

Dans un `coroutineScope`, une erreur non gérée dans une coroutine enfant annule
le scope et les tâches sœurs. Dans un `supervisorScope`, les enfants directs
peuvent échouer indépendamment :

```text
coroutineScope  -> l’échec d’un enfant annule les tâches sœurs
supervisorScope -> l’échec d’un enfant n’annule pas les tâches sœurs
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

Un superviseur ne traite pas automatiquement les erreurs. Une exception
provenant de `async` est relancée par `await()` si elle n’a pas été convertie en
résultat.

`supervisorScope` s’utilise localement dans une fonction `suspend`, tandis que
`SupervisorJob` convient à un scope de longue durée.

**En bref :** utilisez `coroutineScope` pour des tâches liées qui doivent toutes
réussir, et `supervisorScope` pour des tâches indépendantes autorisant un
résultat partiel.

</details>

<details>
<summary>76. Qu’est-ce que la contre-pression et comment la gérer avec Flow ?</summary>

#### Kotlin

La contre-pression apparaît lorsqu’un producteur est potentiellement plus rapide
qu’un consommateur. Dans un Flow ordinaire sans tampon, `emit()` est suspendu
jusqu’à ce que l’aval termine le traitement ; le producteur ralentit donc
naturellement.

```kotlin
flow {
    repeat(1_000) { emit(it) }
}.collect { value ->
    delay(100)
    process(value)
}
```

Les opérateurs suivants modifient ce comportement :

- `buffer(n)` permet au producteur de devancer le consommateur de `n` valeurs ;
- `conflate()` ignore les valeurs intermédiaires et conserve la plus récente ;
- `collectLatest` annule le traitement précédent ;
- `debounce` attend une pause entre les événements entrants ;
- `sample` sélectionne la dernière valeur à intervalles réguliers ;
- `flatMapLatest` annule le Flow interne précédent.

```kotlin
queryFlow
    .debounce(300)
    .flatMapLatest(repository::search)
    .collectLatest(::render)
```

Si chaque valeur est importante, elle ne doit être ni fusionnée ni annulée : il
faut optimiser le consommateur, utiliser un tampon contrôlé ou une file fiable.
Un tampon illimité peut entraîner une croissance de la consommation mémoire.

**En bref :** Flow applique déjà une contre-pression grâce à la suspension de
`emit()`. N’ajoutez un tampon, une fusion ou un opérateur latest que lorsque la
politique nécessaire d’attente ou de perte des données est clairement définie.

</details>

<details>
<summary>77. Comment utiliser les Streams Java en Kotlin ?</summary>

#### Kotlin

Kotlin/JVM peut utiliser directement l’API Stream de Java :

```kotlin
val names = users.stream()
    .filter(User::isActive)
    .map(User::name)
    .collect(Collectors.toList())
```

Pour les collections Kotlin, les opérateurs standard sont généralement plus
concis :

```kotlin
val names = users
    .filter(User::isActive)
    .map(User::name)
```

Pour un traitement synchrone paresseux, Kotlin fournit `Sequence` :

```kotlin
val result = users
    .asSequence()
    .map(User::toUiModel)
    .filter(UserUiModel::isVisible)
    .take(20)
    .toList()
```

Un Stream est pertinent lorsqu’il est renvoyé par une API Java, qu’un
`Collector` Java est nécessaire ou que le pipeline existe déjà en Java. Un
Stream ne peut être consommé qu’une seule fois.

Un Stream d’entrées-sorties doit être fermé :

```kotlin
fun readLines(path: Path): List<String> =
    Files.lines(path).use { stream ->
        stream
            .filter(String::isNotBlank)
            .collect(Collectors.toList())
    }
```

Les Collectors Java possèdent souvent un équivalent Kotlin direct :

```kotlin
val byRole = users.groupBy(User::role)
val byId = users.associateBy(User::id)
val (active, inactive) = users.partition(User::isActive)
```

`parallelStream()` utilise le `ForkJoinPool` commun ; son parallélisme est donc
implicite et son annulation plus complexe. Dans du code fondé sur les coroutines,
il est préférable de contrôler explicitement le parallélisme. Pour les
entrées-sorties, un Stream parallèle ne remplace pas une API `suspend`.

```text
Stream/Sequence -> pipeline synchrone à usage unique
Flow            -> flux asynchrone, suspendable et annulable
```

**En bref :** utilisez les Streams Java pour l’interopérabilité avec Java, les
opérateurs de collections Kotlin pour le traitement immédiat, `Sequence` pour un
pipeline synchrone paresseux et `Flow` pour les données asynchrones.

</details>

<details>
<summary>78. Qu’est-ce que Kotlin Multiplatform Mobile (KMM) ?</summary>

#### Kotlin

`KMM` est l’ancienne appellation de la déclinaison mobile de
`Kotlin Multiplatform` (`KMP`). Le principe consiste à placer le code Kotlin
commun dans un module partagé, puis à l’utiliser sur Android et iOS.

Les éléments généralement partagés sont :

- les modèles métier ;
- les cas d’utilisation ;
- la validation ;
- les contrats des repositories ;
- les couches réseau et données ;
- les règles métier.

Structure courante :

```text
shared/
  commonMain
  androidMain
  iosMain
```

`commonMain` contient le code indépendant de la plateforme :

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Les API propres à une plateforme sont masquées derrière une interface ou
`expect`/`actual` :

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

L’interface utilisateur peut rester native : Compose ou View sur Android, et
SwiftUI ou UIKit sur iOS. Si l’équipe est prête, une partie de l’interface peut
également être partagée avec Compose Multiplatform.

**En bref :** KMP permet de partager la logique métier entre Android et iOS, mais
les API de plateforme, les particularités de l’expérience utilisateur et les
intégrations doivent être isolées. Il ne s’agit pas d’« un seul code pour tout »,
mais d’un partage maîtrisé là où il réduit réellement la duplication.

</details>

<details>
<summary>79. Quelles sont les limites de Kotlin Multiplatform ?</summary>

#### Kotlin

`Kotlin Multiplatform` est utile pour partager la logique, mais présente
plusieurs limites importantes.

1. **Les API de plateforme ne sont pas disponibles dans `commonMain`**

Le code commun ne peut pas utiliser directement `Context`, UIKit ou un SDK
exclusif à Android ou iOS. Il faut recourir à des interfaces ou à
`expect`/`actual`.

```kotlin
expect class PlatformLogger {
    fun log(message: String)
}
```

2. **Toutes les bibliothèques ne sont pas multiplateformes**

Une bibliothèque réservée à Java ou Android ne peut pas simplement être ajoutée
à `commonMain`. Il faut trouver un équivalent KMP ou créer une implémentation
propre à chaque plateforme.

3. **L’interopérabilité avec iOS comporte des subtilités**

Le code Kotlin partagé est exporté vers iOS sous forme de framework. Il faut
tenir compte de l’API Swift, des génériques, des exceptions, de l’adaptation des
coroutines et des Flow, ainsi que du débogage.

4. **Le partage de l’interface utilisateur n’est pas toujours avantageux**

Les couches métier et données sont souvent partagées, tandis que l’interface
reste native. Compose Multiplatform est une possibilité, mais constitue une
décision architecturale distincte.

5. **Le build et la répartition des responsabilités sont plus complexes**

KMP ajoute des source sets, une configuration Gradle et une CI plus complexes.
Il exige également que les équipes Android et iOS s’accordent sur la gestion du
module partagé.

**En bref :** KMP convient à une logique métier commune et stable, mais
n’élimine pas le code propre aux plateformes. Les principaux risques concernent
l’interopérabilité, les bibliothèques, la complexité du build, la répartition
des responsabilités et le déplacement excessif de code vers le module partagé.

</details>

<details>
<summary>80. Comment partager la logique métier avec KMM ?</summary>

#### Kotlin

La logique métier est placée dans un module partagé, tandis que les détails
propres à Android et iOS restent dans les source sets de chaque plateforme.

Structure courante :

```text
shared/
  commonMain   -> domaine, cas d’utilisation et contrats
  androidMain  -> implémentation Android
  iosMain      -> implémentation iOS
```

Il est pertinent de placer dans `commonMain` :

- les modèles métier ;
- les cas d’utilisation ;
- la validation ;
- les interfaces des repositories ;
- les clients API ;
- la sérialisation ;
- les règles métier.

Exemple :

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

Les API propres aux plateformes sont fournies au moyen de l’injection de
dépendances, d’interfaces ou de `expect`/`actual` :

```kotlin
expect class DeviceInfo {
    val platformName: String
}
```

La couche réseau utilise souvent Ktor Client, le JSON
`kotlinx.serialization`, et l’état les coroutines ou Flow. Pour iOS, il faut
prévoir dès le départ une adaptation vers Swift async/await ou Combine.

Il ne faut pas tout partager sans discernement : les permissions, l’appareil
photo, les notifications, le stockage propre à la plateforme et la logique liée
à l’expérience utilisateur doivent rester sur chaque plateforme ou être masqués
derrière un contrat.

**En bref :** le module partagé contient la logique métier stable et les
contrats, tandis que les parties propres aux plateformes sont implémentées
séparément. Un bon projet KMM repose sur une frontière claire, et non sur un
monolithe partagé.

</details>

<details>
<summary>81. Comment optimiser les performances du code Kotlin ?</summary>

#### Kotlin

L’optimisation ne commence pas par la syntaxe, mais par la mesure :

```text
mesurer -> trouver le goulot d’étranglement -> corriger -> mesurer à nouveau
```

Sur Android, on utilise notamment Profiler, Perfetto/System Trace,
Macrobenchmark et le suivi des allocations.

Les principaux axes d’optimisation sont les suivants :

1. **Algorithmes et entrées-sorties**

Les gains les plus importants proviennent de structures de données adaptées, de
la réduction des appels réseau ou de base de données, de la pagination, du cache
et des index de base de données. Une complexité `O(n²)` ne peut pas être corrigée
par une élégante syntaxe Kotlin.

2. **Allocations dans les chemins critiques**

Dans les zones critiques, des appels superflus à `map`, `filter` ou `copy`
peuvent créer de nombreux objets :

```kotlin
for (item in items) {
    draw(item.toUi())
}
```

Cependant, dans une interface utilisateur ordinaire, la lisibilité est plus
importante qu’une micro-optimisation manuelle.

3. **Collections et Sequence**

`Sequence` est utile pour les chaînes paresseuses ou l’arrêt anticipé, mais
introduit un surcoût. Elle n’est pas nécessairement plus rapide sur de petites
listes.

4. **Coroutines**

- utiliser `Dispatchers.IO` pour les entrées-sorties bloquantes ;
- utiliser `Dispatchers.Default` pour les calculs intensifs ;
- ne pas utiliser `runBlocking` sur le thread principal ;
- annuler les tâches devenues obsolètes ;
- éviter les changements de contexte inutiles.

5. **Performances de l’interface utilisateur**

Avec Compose, n’effectuez pas de travail lourd pendant la composition, utilisez
des clés stables et lisez l’état au plus près de son utilisation. Avec les Views,
ne surchargez pas `onBindViewHolder` ou `onDraw` et évitez les passes de layout
inutiles.

6. **Mémoire**

Ne conservez pas une `Activity` ou une `View` dans un singleton, contrôlez la
taille des caches, le cycle de vie des ressources et les grandes bitmaps. Les
fuites augmentent la pression sur le ramasse-miettes et le risque d’OOM.

**En bref :** commencez par profiler, puis corrigez les algorithmes, les
entrées-sorties, les allocations, la concurrence, les chemins critiques de
l’interface utilisateur et les fuites de mémoire. N’utilisez `inline`,
`Sequence` et les tableaux de primitives que lorsque les mesures démontrent leur
utilité.

</details>

<details>
<summary>82. Comment implémenter une nouvelle tentative avec backoff exponentiel en Kotlin ?</summary>

#### Kotlin

Le backoff exponentiel répète une opération après une erreur transitoire en
augmentant progressivement le délai. Le mécanisme doit définir `maxAttempts`,
`maxDelay`, un jitter et un timeout, sans compromettre l’annulation.

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

Exemple de prédicat :

```kotlin
fun shouldRetry(error: Throwable): Boolean = when (error) {
    is IOException -> true
    is HttpException -> error.code() in setOf(408, 429, 500, 502, 503, 504)
    else -> false
}
```

Pour un `Flow`, il est possible d’utiliser `retryWhen` :

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

Il ne faut pas répéter les erreurs de validation, les réponses `403` ou `404`,
les échecs d’authentification sans mécanisme de renouvellement, ni les opérations
non idempotentes sans clé d’idempotence. Pour les réponses `429` et `503`, il est
préférable de respecter l’en-tête `Retry-After`.

**En bref :** les nouvelles tentatives doivent être limitées, annulables,
comporter un jitter et reposer sur une règle explicite déterminant les erreurs
répétables. Dans le cas contraire, elles masquent les problèmes et augmentent la
charge.

</details>

<details>
<summary>83. Quelles nouveautés de Kotlin faut-il connaître en 2026 ?</summary>

#### Kotlin

En 2026, lors d’un entretien, il est plus important de comprendre l’orientation
de Kotlin 2.x et de son écosystème d’outils que de connaître chaque fonctionnalité
mineure.

1. **Compilateur K2**

K2 est la nouvelle architecture du compilateur de Kotlin 2.x. Elle apporte une
analyse plus rapide et une meilleure base pour l’IDE, les plugins de compilation
et le multiplateforme. Avant une mise à niveau, il faut vérifier la compatibilité
du Kotlin Gradle Plugin, d’AGP, de Gradle, du compilateur Compose, de KSP ou kapt,
ainsi que de la sérialisation.

2. **Paramètres et receivers de contexte**

Ils sont utiles pour les DSL et les API limitées à une portée :

```kotlin
context(logger: Logger)
fun User.save() {
    logger.log("Saving $id")
}
```

Ils ne remplacent pas l’injection de dépendances, mais permettent de transmettre
explicitement le contexte d’exécution.

3. **Champs sous-jacents explicites**

Ils réduisent le code répétitif nécessaire pour exposer une API publique en
lecture seule tout en conservant un état interne mutable :

```kotlin
val state: StateFlow<UiState>
    field = MutableStateFlow(UiState.Loading)
```

4. **Évolution du multiplateforme**

KMP devient plus pratique : l’export vers Swift, l’adaptation des coroutines et
des Flow, Wasm et Compose Multiplatform continuent d’évoluer. Les décisions de
production doivent néanmoins toujours être évaluées selon l’interopérabilité,
les outils, le débogage et la répartition des responsabilités dans l’équipe.

5. **Bibliothèque standard et outils JVM**

Il faut suivre les nouvelles API de la bibliothèque standard,
`kotlin.uuid.Uuid`, la compatibilité des cibles et toolchains JVM, le cache de
configuration Gradle et les performances du build.

6. **Règle d’adoption**

Une nouvelle fonctionnalité ne doit pas être introduite en production uniquement
parce qu’elle est récente. Il faut vérifier :

- si elle est stable ou expérimentale ;
- sa prise en charge par l’IDE et la CI ;
- la compatibilité des plugins ;
- sa lisibilité ;
- le plan de migration et de retour en arrière.

**En bref :** en 2026, il faut connaître Kotlin 2.x et K2, les API de contexte,
les champs sous-jacents, l’évolution de KMP, Wasm et Compose Multiplatform, ainsi
que les contraintes liées aux outils. Dans la pratique, une mise à niveau sûre
de l’écosystème importe davantage que l’utilisation de chaque nouveauté
syntaxique.

</details>

<details>
<summary>84. Qu’est-ce que le cycle de vie d’une `Activity` ?</summary>

#### Kotlin

Le cycle de vie d’une `Activity` est une suite de callbacks par lesquels Android
gère la création, la visibilité, le passage au premier plan ou à l’arrière-plan
et la destruction d’un écran.

Ordre principal :

```text
onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
```

Lors du retour depuis l’état `STOPPED`, `onRestart()` peut être appelé avant
`onStart()`.

- `onCreate()` — créer l’interface utilisateur, la navigation et les dépendances
  de base ; ne pas y effectuer de tâche synchrone lourde.
- `onStart()` — l’Activity devient visible.
- `onResume()` — l’Activity est au premier plan et prête à recevoir des
  interactions.
- `onPause()` — elle perd le focus ; ce callback doit rester rapide.
- `onStop()` — elle n’est plus visible ; arrêter les ressources nécessaires
  uniquement lorsqu’elle est visible.
- `onDestroy()` — l’instance est détruite à la suite de `finish()` ou d’une
  recréation ; cet appel n’est pas garanti lorsque le processus est tué.

Un changement de configuration peut recréer l’Activity :

```text
ancienne : onPause -> onStop -> onDestroy
nouvelle : onCreate -> onStart -> onResume
```

Une `ViewModel` survit aux changements de configuration. Un petit état
d’interface est enregistré dans `SavedStateHandle` ou `onSaveInstanceState`,
tandis que les données durables sont conservées dans un stockage persistant.

La collecte d’un Flow doit être liée au cycle de vie :

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

**En bref :** `onCreate` crée l’écran, `onStart` le rend visible, `onResume` le
rend actif, `onPause` et `onStop` interrompent le travail, et `onDestroy` termine
l’instance. L’état ne doit pas dépendre uniquement de l’instance de l’Activity.

</details>

<details>
<summary>85. Qu’est-ce qu’un `Fragment` et en quoi diffère-t-il d’une `Activity` ?</summary>

#### Kotlin

Une `Activity` est un composant Android possédant sa propre fenêtre et sa propre
tâche, ainsi qu’un point d’entrée déclaré dans le manifeste. Un `Fragment` est
un contrôleur d’interface utilisateur placé dans une Activity ou dans un autre
Fragment et géré par un `FragmentManager`.

```text
Activity -> composant système, fenêtre, intents, tâche
Fragment -> partie de l’interface dans un hôte, pile de retour via FragmentManager
```

Exemple d’Activity :

```kotlin
class MainActivity : AppCompatActivity()
```

Exemple de Fragment :

```kotlin
class ProfileFragment : Fragment(R.layout.fragment_profile)
```

La principale différence pratique est qu’un Fragment possède un cycle de vie
distinct pour sa View :

```text
onCreateView -> onViewCreated -> onDestroyView
```

L’objet Fragment peut subsister alors que sa View est déjà détruite. Il faut donc
libérer le binding, l’adapter et les abonnements de l’interface utilisateur dans
`onDestroyView()` :

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    _binding = null
    super.onDestroyView()
}
```

Les Flow et LiveData destinés à l’interface doivent être liés au
`viewLifecycleOwner`, et non au cycle de vie du Fragment lui-même.

Navigation :

- entre des Activities — `Intent` ;
- entre des Fragments — `FragmentManager` ou Navigation Component.

Dans une application entièrement conçue avec Compose, une seule Activity sans
Fragments suffit souvent. Dans une application hybride ou fondée sur les Views,
un Fragment reste une abstraction pertinente pour les écrans et la navigation.

**En bref :** une Activity est un composant Android autonome, tandis qu’un
Fragment représente une partie de l’interface dans un hôte. La règle essentielle
est que la View d’un Fragment vit moins longtemps que le Fragment lui-même ; son
nettoyage doit donc être effectué dans `onDestroyView()`.

</details>

<details>
<summary>86. Qu’est-ce qu’un `Intent` et comment l’utilise-t-on ?</summary>

#### Kotlin

Un `Intent` est un objet de message Android qui décrit une action ou un composant
précis : ouvrir une Activity, un Service ou un Broadcast, ou transmettre des
données.

1. **Intent explicite**

Il désigne un composant précis :

```kotlin
val intent = Intent(this, DetailsActivity::class.java)
    .putExtra("user_id", userId)

startActivity(intent)
```

Il est utilisé pour la navigation interne entre des Activities.

2. **Intent implicite**

Il décrit une action, puis Android recherche une application capable de
l’exécuter :

```kotlin
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://example.com"))
startActivity(intent)
```

Exemples : navigateur, partage, téléphone, cartes et sélecteur de fichiers.

3. **Extras**

Les extras permettent de transmettre de petites quantités de données :

```kotlin
intent.putExtra("user_id", userId)
val userId = intent.getStringExtra("user_id")
```

Il est préférable de transmettre un `id` plutôt que des objets volumineux. Un
`Parcelable` peut servir aux arguments structurés, mais une navigation reposant
sur un identifiant et un repository est souvent plus simple.

4. **Filtre d’Intent**

Un composant déclare les Intents qu’il accepte :

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="https" />
</intent-filter>
```

Cela est nécessaire pour les liens profonds, les cibles de partage et les
actions système.

5. **API Activity Result**

À la place de `startActivityForResult`, on utilise une API qui tient compte du
cycle de vie :

```kotlin
val launcher = registerForActivityResult(
    ActivityResultContracts.GetContent()
) { uri ->
    // traiter l’URI
}

launcher.launch("image/*")
```

6. **Sécurité**

- ne pas transmettre de secrets dans les extras ;
- lancer les composants internes au moyen d’un Intent explicite ;
- valider les données provenant de l’extérieur ;
- configurer correctement `android:exported` ;
- avec un Intent implicite, prévoir l’absence éventuelle d’une application
  capable de le traiter.

**En bref :** un Intent explicite lance un composant précis, tandis qu’un Intent
implicite décrit une action destinée au système. Les extras ne servent qu’aux
petites données. Pour obtenir un résultat, utilisez l’API Activity Result. Les
Intents externes exigent une validation et une attention particulière à la
sécurité.

</details>

<details>
<summary>87. Que se passe-t-il si l’on n’appelle pas `super.onCreate()` ?</summary>

#### Kotlin

Si `super.onCreate(savedInstanceState)` n’est pas appelé dans
`Activity.onCreate()`, la classe Android ou Jetpack de base n’effectue pas
l’initialisation obligatoire du cycle de vie. Cela peut provoquer un crash ou un
fonctionnement incorrect de l’Activity.

Code correct :

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

`super.onCreate()` prépare notamment :

- l’état du cycle de vie ;
- la restauration de l’état sauvegardé ;
- l’infrastructure de la fenêtre et de son décor ;
- le `FragmentManager` dans une `FragmentActivity` ;
- le `SavedStateRegistry` ;
- l’`ActivityResultRegistry` ;
- les intégrations avec AppCompat et ComponentActivity.

Sans cet appel, les problèmes suivants peuvent survenir :

- une `SuperNotCalledException` ;
- un cycle de vie défaillant ;
- des problèmes avec les Fragments ;
- l’absence de restauration de l’état sauvegardé ;
- des dysfonctionnements avec l’API Activity Result, Compose, Hilt ou AppCompat.

La même règle s’applique avec Compose :

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent { App() }
}
```

Dans les callbacks du cycle de vie d’un Fragment, il faut également appeler
`super` dans la plupart des cas, sauf indication contraire explicite de la
documentation.

**En bref :** `super.onCreate()` n’est pas une simple formalité, mais une partie
du contrat avec le framework. Il doit être appelé au début de `onCreate()`, avant
toute logique dépendant de l’interface utilisateur ou du framework.

</details>

<details>
<summary>88. Qu’est-ce que View Binding et pourquoi est-il préférable à `findViewById` ?</summary>

#### Kotlin

View Binding génère une classe de binding pour chaque layout XML et fournit un
accès typé aux Views sans appel manuel à `findViewById`.

Problème de `findViewById` :

```kotlin
val title = findViewById<TextView>(R.id.title)
title.text = "Hello"
```

Inconvénients :

- du code répétitif ;
- un risque d’erreur de type ;
- une erreur détectée uniquement à l’exécution ;
- une prise en charge moins fiable du refactoring.

View Binding dans une `Activity` :

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.title.text = "Hello"
}
```

Dans un `Fragment`, le binding doit être libéré dans `onDestroyView()`, car le
cycle de vie de la View est plus court que celui du Fragment :

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

Avantages de View Binding :

- la sécurité de typage ;
- moins de code répétitif ;
- la vérification des identifiants à la compilation ;
- un refactoring plus pratique ;
- des champs nullables lorsqu’une View est absente d’une certaine configuration
  de layout.

View Binding ne doit pas être confondu avec Data Binding :

```text
View Binding -> accès aux Views
Data Binding -> expressions de liaison dans le XML
```

View Binding est généralement inutile pour une interface entièrement conçue avec
Compose. Il reste pertinent pour le système XML/View et les écrans hybrides.

**En bref :** View Binding est une alternative plus sûre et plus concise à
`findViewById` pour les layouts XML. Dans un Fragment, le point essentiel est de
libérer le binding dans `onDestroyView()`.

</details>

<details>
<summary>89. Qu’est-ce que `LiveData` ?</summary>

#### Kotlin

`LiveData` est un conteneur de données observable d’Android Jetpack qui tient
compte du cycle de vie. Il conserve une valeur et ne notifie les observateurs que
lorsque le `LifecycleOwner` se trouve dans l’état `STARTED` ou `RESUMED`.

Exemple courant dans une `ViewModel` :

```kotlin
class ProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<UserUi>()
    val user: LiveData<UserUi> = _user

    fun load() {
        _user.value = UserUi("Alex")
    }
}
```

On expose `LiveData`, et non `MutableLiveData`, afin que l’interface utilisateur
ne puisse pas modifier directement l’état.

Observation dans un Fragment :

```kotlin
viewModel.user.observe(viewLifecycleOwner) { user ->
    render(user)
}
```

`viewLifecycleOwner` est important, car la View d’un Fragment vit moins longtemps
que le Fragment lui-même.

Différence entre `value` et `postValue` :

```kotlin
_liveData.value = user      // thread principal
_liveData.postValue(user)   // thread d’arrière-plan, asynchrone
```

Avantages :

- tient compte du cycle de vie ;
- possède une API simple ;
- fonctionne bien avec le système XML/View ;
- désabonne automatiquement un propriétaire détruit ;
- convient au code Android ancien.

Limites :

- dépend d’Android ;
- offre une API de flux moins riche que `Flow` ;
- est peu pratique avec KMP ;
- les événements ponctuels conduisent souvent à des solutions de contournement
  semblables à `SingleLiveEvent`.

Comparaison :

```text
LiveData  -> conteneur Android tenant compte du cycle de vie
StateFlow -> flux Kotlin chaud possédant une valeur d’état
```

Dans le nouveau code fondé sur les coroutines ou Compose, on utilise plus souvent
`StateFlow` avec `collectAsStateWithLifecycle()`. `LiveData` reste pertinent dans
le système View ancien ou dans les projets où il constitue déjà la norme.

**En bref :** LiveData est un conteneur d’état observable Android qui tient
compte du cycle de vie. Pour un nouveau projet Kotlin ou Compose, `StateFlow` est
souvent préférable, mais LiveData reste parfaitement utilisable avec le système
XML/View.

</details>

<details>
<summary>90. Qu’est-ce que Room et comment fonctionne-t-il ?</summary>

#### Kotlin

`Room` est une bibliothèque Jetpack permettant d’utiliser SQLite au moyen d’une
API typée. Elle ne supprime pas SQL, mais fournit des vérifications à la
compilation, des DAO, des migrations et une intégration pratique avec les
coroutines et Flow.

Principaux éléments :

```text
@Entity   -> table
@Dao      -> méthodes d’accès SQL
@Database -> point d’entrée de la base de données
```

Entité :

```kotlin
@Entity(tableName = "users")
data class UserEntity(
    @PrimaryKey val id: String,
    val name: String,
    val age: Int
)
```

DAO :

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insert(user: UserEntity)
}
```

Base de données :

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

Création :

```kotlin
val db = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "app.db"
).build()
```

Room prend en charge `Flow` :

```kotlin
@Query("SELECT * FROM users")
fun observeUsers(): Flow<List<UserEntity>>
```

Lorsque la table est modifiée, Room invalide la requête et émet les nouvelles
données.

Les relations nécessitent souvent `@Transaction` :

```kotlin
data class UserWithPosts(
    @Embedded val user: UserEntity,
    @Relation(parentColumn = "id", entityColumn = "userId")
    val posts: List<PostEntity>
)
```

Les modifications du schéma sont gérées au moyen de migrations :

```kotlin
val MIGRATION_1_2 = object : Migration(1, 2) {
    override fun migrate(db: SupportSQLiteDatabase) {
        db.execSQL("ALTER TABLE users ADD COLUMN avatarUrl TEXT")
    }
}
```

En production, une migration destructive ne doit pas être utilisée sans
réflexion lorsque des données utilisateur sont présentes.

Dans la pratique, Room est masqué derrière un repository :

```kotlin
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

L’interface utilisateur ne doit pas dépendre directement des entités de la base
de données.

**En bref :** Room est une surcouche typée de SQLite reposant sur `Entity`, `Dao`
et `Database`. Il prend en charge les fonctions `suspend`, Flow, les relations et
les migrations. Dans une architecture correcte, Room appartient à la couche
données derrière un repository, et non directement à l’interface utilisateur.

</details>

<details>
<summary>91. Que font les annotations `@Query`, `@Insert` et `@Delete` ?</summary>

#### Kotlin

`@Query`, `@Insert` et `@Delete` sont des annotations Room destinées aux méthodes
d’un DAO. Room génère leur implémentation et vérifie le SQL ainsi que le mapping
à la compilation.

```kotlin
@Dao
interface UserDao {
    @Query("SELECT * FROM users WHERE id = :id")
    suspend fun getUser(id: String): UserEntity?
}
```

### `@Query`

Exécute une requête SQL explicite :

```kotlin
@Query("SELECT * FROM users ORDER BY name")
fun observeUsers(): Flow<List<UserEntity>>
```

`:id` et `:name` sont des paramètres liés. Il ne faut pas concaténer les données
de l’utilisateur dans une chaîne SQL.

`@Query` ne se limite pas à `SELECT` :

```kotlin
@Query("UPDATE users SET name = :name WHERE id = :id")
suspend fun updateName(id: String, name: String): Int

@Query("DELETE FROM users WHERE id = :id")
suspend fun deleteById(id: String): Int
```

La valeur `Int` correspond au nombre de lignes affectées.

### `@Insert`

Insère une `@Entity` :

```kotlin
@Insert(onConflict = OnConflictStrategy.REPLACE)
suspend fun insert(user: UserEntity)

@Insert
suspend fun insertAll(users: List<UserEntity>)
```

`onConflict` définit le comportement en cas de conflit avec une clé primaire ou
unique : `ABORT`, `IGNORE` ou `REPLACE`. Il faut utiliser `REPLACE` avec
prudence : SQLite peut supprimer l’ancienne ligne avant d’en insérer une
nouvelle. Pour une insertion ou mise à jour, `@Upsert` est souvent préférable
lorsqu’il est disponible dans le projet.

### `@Delete`

Supprime une entité selon sa clé primaire :

```kotlin
@Delete
suspend fun delete(user: UserEntity): Int
```

Pour supprimer selon un identifiant ou une condition, il est préférable
d’utiliser `@Query("DELETE ...")`.

### `@Transaction`

Lorsque plusieurs opérations DAO doivent être atomiques, on utilise
`@Transaction` :

```kotlin
@Transaction
suspend fun replaceUsers(users: List<UserEntity>) {
    deleteAll()
    insertAll(users)
}
```

Room vérifie les tables, les colonnes, les paramètres liés et le mapping du type
de retour. Le développeur doit néanmoins toujours analyser les performances des
requêtes, les index et la logique métier.

**En bref :** `@Query` exécute du SQL, `@Insert` insère une entité et `@Delete`
supprime une entité selon sa clé primaire. Room génère le code du DAO et détecte
de nombreuses erreurs à la compilation, mais ne remplace pas l’analyse des
performances des requêtes.

</details>

<details>
<summary>92. Qu’est-ce que WorkManager et quand faut-il l’utiliser ?</summary>

#### Kotlin

`WorkManager` est une API Jetpack destinée aux tâches d’arrière-plan persistantes
et différables. Une tâche peut donc démarrer plus tard, mais doit survivre à la
fermeture de l’application, à la mort du processus et, si nécessaire, au
redémarrage de l’appareil.

Il est utilisé notamment pour :

- la synchronisation en arrière-plan ;
- les nouveaux essais de téléversement ou de téléchargement ;
- le nettoyage ;
- l’envoi de journaux ou de données d’analyse ;
- la maintenance périodique ;
- les tâches soumises à des contraintes de réseau, de recharge, de batterie ou
  de stockage.

Tâche unique :

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

`CoroutineWorker` :

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

Résultats :

- `Result.success()` — la tâche est terminée ;
- `Result.retry()` — la tâche doit être répétée selon la politique de backoff ;
- `Result.failure()` — l’échec est définitif.

Fonctionnalités importantes :

- les contraintes ;
- les nouvelles tentatives et le backoff ;
- les tâches uniques avec `enqueueUniqueWork()` ;
- l’enchaînement de tâches ;
- les tâches périodiques, sans heure d’exécution précise.

Les données d’entrée et de sortie transmises avec `Data` doivent rester petites.
Les charges volumineuses doivent être placées dans une base de données ou un
fichier, puis référencées par un identifiant.

WorkManager ne convient pas :

- aux alarmes exactes ;
- à une connexion socket en temps réel ;
- à la lecture multimédia ;
- au suivi permanent de la position ;
- à une action que l’utilisateur attend immédiatement.

Une tâche continue visible par l’utilisateur nécessite un service au premier
plan. Pour une heure précise, utilisez AlarmManager ou l’API Exact Alarm, si ce
cas d’usage est réellement autorisé.

**En bref :** WorkManager convient aux tâches d’arrière-plan différées et fiables
qui nécessitent des contraintes et des nouvelles tentatives. Il ne garantit pas
une heure de démarrage précise et ne remplace ni un service au premier plan, ni
une connexion en temps réel, ni une alarme exacte.

</details>

<details>
<summary>93. Quelle est la différence entre `CoroutineScope`, `lifecycleScope` et `viewModelScope` ?</summary>

#### Kotlin

`CoroutineScope` définit le contexte et la durée de vie des coroutines.
`lifecycleScope` et `viewModelScope` sont des scopes Android prêts à l’emploi,
dont le propriétaire est déjà défini.

```text
CoroutineScope  -> vous définissez vous-même sa durée de vie
lifecycleScope  -> vit jusqu’à l’état DESTROYED du LifecycleOwner
viewModelScope  -> vit jusqu’à ViewModel.onCleared()
```

### `CoroutineScope`

Scope général pour les coroutines :

```kotlin
val scope = CoroutineScope(SupervisorJob() + Dispatchers.Main)

scope.launch {
    repository.load()
}
```

Si vous créez un scope personnalisé, vous devez appeler explicitement `cancel()`
au moment approprié. Sinon, il peut provoquer une fuite ou poursuivre un travail
inutile après la disparition de son propriétaire.

### `lifecycleScope`

Il appartient à une `Activity`, un `Fragment` ou un autre `LifecycleOwner`, et
est annulé à l’état `DESTROYED` :

```kotlin
lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Un simple `lifecycleScope.launch` n’est pas suspendu à l’état `STOPPED`. La
collecte d’un Flow doit donc utiliser `repeatOnLifecycle`.

Dans un Fragment, il est préférable d’utiliser
`viewLifecycleOwner.lifecycleScope` pour travailler avec la View ou le binding :

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

La raison est que l’objet Fragment peut vivre plus longtemps que sa View.

### `viewModelScope`

Il appartient à une `ViewModel`, survit aux changements de configuration et est
annulé dans `onCleared()` :

```kotlin
class ProfileViewModel : ViewModel() {
    fun load() {
        viewModelScope.launch {
            repository.loadProfile()
        }
    }
}
```

Il sert à la logique de l’écran, au chargement, aux appels métier et à la
construction de l’état de l’interface utilisateur.

Erreurs courantes :

- utiliser `GlobalScope` pour le travail d’un écran ;
- créer un scope personnalisé sans annulation ;
- collecter un état lié au binding dans le scope du Fragment plutôt que dans
  celui du cycle de vie de sa View ;
- lancer les opérations métier depuis l’Activity plutôt que depuis la ViewModel ;
- accéder au binding après `onDestroyView()`.

**En bref :** utilisez `viewModelScope` pour la logique d’écran dans une
ViewModel, `lifecycleScope` pour la collecte et les effets de l’interface, et un
`CoroutineScope` personnalisé uniquement lorsqu’il possède une durée de vie
claire et un endroit défini où appeler `cancel()`.

</details>

<details>
<summary>94. Comment implémenter l’injection de dépendances en Kotlin ?</summary>

#### Kotlin

L’injection de dépendances consiste à fournir les dépendances depuis l’extérieur
plutôt qu’à les créer dans la classe. En Kotlin, l’approche par défaut est
l’injection par constructeur.

Correct :

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

Incorrect :

```kotlin
class GetUserUseCase {
    private val repository = UserRepositoryImpl(ApiClient())
}
```

La seconde variante lie fortement la classe à une implémentation et complique
les tests.

Un cas d’utilisation dépend généralement d’un contrat :

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class UserRepositoryImpl(
    private val api: UserApi
) : UserRepository
```

### Injection manuelle

Pour un petit projet, un conteneur simple suffit :

```kotlin
class AppContainer {
    private val api = UserApi()
    val userRepository: UserRepository = UserRepositoryImpl(api)
    val getUserUseCase = GetUserUseCase(userRepository)
}
```

Son avantage est la simplicité ; son inconvénient est qu’un grand graphe devient
difficile à maintenir manuellement.

### Hilt/Dagger

Cette injection vérifiée à la compilation est un choix courant pour les grandes
applications Android :

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val getUser: GetUserUseCase
) : ViewModel()
```

Liaison d’une interface :

```kotlin
@Module
@InstallIn(SingletonComponent::class)
interface UserModule {
    @Binds
    fun bindUserRepository(impl: UserRepositoryImpl): UserRepository
}
```

Les erreurs du graphe sont détectées à la compilation, mais cette solution exige
davantage de code répétitif et une génération plus complexe.

### Koin

Injection à l’exécution au moyen d’un DSL Kotlin :

```kotlin
val appModule = module {
    single<UserRepository> { UserRepositoryImpl(get()) }
    factory { GetUserUseCase(get()) }
}
```

Koin est plus simple à adopter, mais certaines erreurs peuvent n’apparaître qu’à
l’exécution.

L’injection de dépendances gère également la durée de vie : `singleton`,
`factory` ou scope de ViewModel. Le scope doit être choisi avec soin afin
d’éviter les fuites et les singletons inutiles.

**En bref :** en Kotlin, l’injection de dépendances repose le plus souvent sur
l’injection par constructeur. L’injection manuelle convient aux petits graphes,
Hilt/Dagger aux grandes applications Android et Koin à une mise en place simple.
Elle réduit le couplage et facilite considérablement les tests.

</details>

<details>
<summary>95. Qu’est-ce que Hilt et comment fonctionne-t-il ?</summary>

#### Kotlin

`Hilt` est un framework d’injection de dépendances Android construit sur Dagger.
Il génère le graphe des dépendances et une hiérarchie de composants prête à
l’emploi pour `Application`, `Activity`, `Fragment`, `ViewModel`, `View` et
`Service`.

Configuration de base :

```kotlin
@HiltAndroidApp
class App : Application()
```

`@HiltAndroidApp` crée le graphe racine au niveau de l’application.

Classe Android dans laquelle des dépendances doivent être injectées :

```kotlin
@AndroidEntryPoint
class ProfileFragment : Fragment()
```

L’injection par constructeur est la méthode principale :

```kotlin
class LoadProfileUseCase @Inject constructor(
    private val repository: ProfileRepository
)
```

ViewModel :

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Les modules sont nécessaires lorsque Hilt ne peut pas créer lui-même une
dépendance : interface, classe tierce, builder ou factory.

`@Binds` associe une interface à son implémentation :

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

`@Provides` sert à créer un objet au moyen d’une factory ou d’un builder, par
exemple `OkHttpClient`, `Retrofit` ou `Room`.

Les scopes déterminent la durée de vie d’un objet :

```kotlin
@Singleton
@ActivityRetainedScoped
@ViewModelScoped
```

Principaux composants :

```text
SingletonComponent        -> application
ActivityRetainedComponent -> survit aux changements de configuration
ViewModelComponent        -> ViewModel
ActivityComponent         -> Activity
FragmentComponent         -> Fragment
ServiceComponent          -> Service
```

Règle importante : un composant de longue durée ne peut pas dépendre d’un
composant de durée plus courte. Par exemple, une dépendance `@Singleton` ne doit
pas conserver le contexte d’une `Activity`.

Pour les tests, Hilt permet de remplacer des modules au moyen de
`@TestInstallIn`. Dans les tests unitaires, il est toutefois souvent plus simple
de fournir directement un fake par le constructeur.

**En bref :** Hilt génère un graphe Dagger pour Android. `@Inject` crée les
dépendances, `@Binds` et `@Provides` décrivent les liaisons,
`@AndroidEntryPoint` active l’injection dans une classe Android, et les scopes
contrôlent la durée de vie.

</details>

<details>
<summary>96. Qu’est-ce que `@HiltViewModel` ?</summary>

#### Kotlin

`@HiltViewModel` intègre une `ViewModel` à Hilt. Hilt génère une
`ViewModelProvider.Factory` et fournit les dépendances par injection dans le
constructeur ; une factory écrite manuellement est donc généralement inutile.

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

Règles :

- `@HiltViewModel` est placée sur la classe ;
- `@Inject constructor` est placé sur le constructeur ;
- toutes les dépendances doivent appartenir au graphe Hilt ;
- Hilt fournit automatiquement `SavedStateHandle` ;
- l’`Activity` ou le `Fragment` hôte doit posséder `@AndroidEntryPoint`.

Obtention dans un Fragment :

```kotlin
@AndroidEntryPoint
class UserFragment : Fragment(R.layout.fragment_user) {
    private val viewModel: UserViewModel by viewModels()
}
```

Pour une ViewModel partagée au niveau de l’Activity :

```kotlin
private val viewModel: UserViewModel by activityViewModels()
```

Avec Compose :

```kotlin
@Composable
fun UserScreen(
    viewModel: UserViewModel = hiltViewModel()
) {
    val state by viewModel.state.collectAsStateWithLifecycle()
}
```

Le scope de la ViewModel est déterminé par le `ViewModelStoreOwner` : Fragment,
Activity ou entrée de la pile de navigation.

Les dépendances d’une ViewModel vivent dans le `ViewModelComponent`.
`@ViewModelScoped` fournit une instance par ViewModel précise, tandis que
`@Singleton` fournit une instance pour tout le graphe de l’application.

Erreurs courantes :

- oublier `@HiltViewModel` ou `@Inject constructor` ;
- oublier `@AndroidEntryPoint` sur l’hôte ;
- créer manuellement une ViewModel avec son constructeur ;
- attendre la même instance avec des `ViewModelStoreOwner` différents ;
- injecter une `Activity`, un `Fragment`, une `View` ou un `Context` de courte
  durée dans une ViewModel.

**En bref :** `@HiltViewModel` permet à Hilt de créer une ViewModel au moyen
d’une factory générée et d’injecter ses dépendances de constructeur ainsi que
`SavedStateHandle`. Sa durée de vie réelle est déterminée par le
`ViewModelStoreOwner`, et non par l’annotation elle-même.

</details>

<details>
<summary>97. Comment fonctionne `@Inject` ?</summary>

#### Kotlin

`@Inject` est l’annotation qui indique à Dagger ou Hilt comment créer un objet ou
où injecter une dépendance.

L’approche principale est l’injection par constructeur :

```kotlin
class LoadUserUseCase @Inject constructor(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: String): User =
        repository.loadUser(id)
}
```

Hilt peut créer `LoadUserUseCase` si le graphe contient une liaison pour chaque
paramètre du constructeur. Si une liaison est absente ou ambiguë, une erreur de
compilation est produite.

L’injection par constructeur est préférable pour les classes du projet :

- les dépendances sont explicites ;
- elles peuvent être déclarées avec `val` ;
- la classe est facile à tester ;
- aucun mécanisme caché du framework n’est nécessaire.

L’injection de champ est principalement utilisée pour les classes Android créées
par le framework :

```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() {
    @Inject lateinit var analytics: Analytics
}
```

Ce champ ne doit pas être utilisé avant l’injection effectuée dans le cycle de
vie du composant.

`@Inject constructor` ne définit pas à lui seul la liaison d’une interface.
Celle-ci nécessite `@Binds` :

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

Pour les classes tierces ou les builders, on utilise `@Provides` :

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {
    @Provides
    fun provideOkHttpClient(): OkHttpClient =
        OkHttpClient.Builder().build()
}
```

`@Inject` ne définit pas la durée de vie. Celle-ci est déterminée par des scopes
tels que `@Singleton`, `@ViewModelScoped` ou `@ActivityScoped`. Lorsque plusieurs
liaisons possèdent le même type, on utilise des qualifiers.

Hilt et Dagger génèrent les factories et le graphe à la compilation, sans
réflexion à l’exécution pour créer les dépendances.

**En bref :** `@Inject` indique à Hilt ou Dagger comment créer une classe ou où
insérer une dépendance. L’injection par constructeur est l’approche par défaut ;
`@Binds` sert aux interfaces, `@Provides` aux objets externes, les scopes à la
durée de vie et les qualifiers à distinguer des types identiques.

</details>

<details>
<summary>98. Que sont les scopes dans Hilt ?</summary>

#### Kotlin

Dans Hilt, un scope détermine dans quel composant une dépendance est mise en
cache. Toutes les demandes effectuées dans une même instance de composant
reçoivent donc le même objet.

Sans scope, une liaison est généralement recréée à chaque demande d’injection.
C’est un comportement par défaut approprié pour les objets sans état et peu
coûteux.

Hiérarchie simplifiée des composants :

```text
SingletonComponent
 ├── ActivityRetainedComponent
 │    └── ViewModelComponent
 ├── ActivityComponent
 │    └── FragmentComponent
 │         └── ViewComponent
 └── ServiceComponent
```

Un enfant voit les liaisons de son parent, mais un parent ne peut pas dépendre
d’une liaison enfant de durée plus courte.

Principaux scopes :

- `@Singleton` — une instance pour le graphe de l’application ;
- `@ActivityRetainedScoped` — une instance pour une Activity logique, qui survit
  aux changements de configuration ;
- `@ViewModelScoped` — une instance pour une ViewModel précise ;
- `@ActivityScoped` — une instance pour une instance d’Activity ;
- `@FragmentScoped` — une instance pour un Fragment précis ;
- `@ViewScoped` — une instance pour une View compatible avec Hilt ;
- `@ServiceScoped` — une instance pour un Service précis.

Le scope doit correspondre au composant indiqué par `@InstallIn` :

```kotlin
@Module
@InstallIn(ViewModelComponent::class)
object UserModule {
    @Provides
    @ViewModelScoped
    fun provideDraftCache(): UserDraftCache = UserDraftCache()
}
```

Une dépendance `@ViewModelScoped` est unique pour une ViewModel précise, mais
diffère entre plusieurs ViewModels. Pour partager un objet entre les ViewModels
d’une même Activity, il faut utiliser `@ActivityRetainedScoped`.

Une erreur courante consiste à conserver un contexte de courte durée dans un
objet de longue durée :

```kotlin
@Singleton
class ActivityHolder @Inject constructor(
    @ActivityContext private val context: Context
)
```

Cette pratique est incorrecte : le singleton conserverait l’Activity après sa
destruction. Un `@Singleton` doit utiliser un état ou un contexte au niveau de
l’application.

Il ne faut pas attribuer un scope à chaque objet. Un scope représente une
sémantique de propriété et de cycle de vie, et non une simple optimisation.

**En bref :** un scope Hilt définit la durée de vie et la réutilisation d’une
dépendance dans un composant. Il doit correspondre à son véritable propriétaire
et ne jamais conserver d’objets dont le cycle de vie est plus court.

</details>

<details>
<summary>99. Comment éviter les fuites de mémoire sur Android ?</summary>

#### Kotlin

Une fuite de mémoire se produit lorsqu’un objet devenu inutile est toujours
retenu par une référence forte. Sur Android, les fuites concernent le plus
souvent une `Activity`, un `Fragment`, une `View`, un `Context`, des callbacks ou
des coroutines.

### Ne pas conserver une Activity ou une View dans un objet de longue durée

Incorrect :

```kotlin
object SessionHolder {
    lateinit var context: Context
}
```

S’il s’agit du contexte d’une `Activity`, celle-ci ne sera pas libérée après une
rotation ou `finish()`. Les dépendances au niveau de l’application doivent
utiliser `applicationContext`.

### Libérer View Binding dans un Fragment

```kotlin
private var _binding: FragmentProfileBinding? = null
private val binding get() = _binding!!

override fun onDestroyView() {
    _binding = null
    super.onDestroyView()
}
```

L’objet Fragment peut vivre plus longtemps que sa View ; les références aux
Views doivent donc être supprimées dans `onDestroyView()`.

### Libérer les adapters, listeners et callbacks

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Les adapters, listeners et callbacks anonymes conservent souvent une référence
vers une View ou un Fragment. Ils doivent être désinscrits ou libérés dans le
callback approprié du cycle de vie.

### Utiliser des coroutines tenant compte du cycle de vie

Pour collecter un état d’interface dans un Fragment :

```kotlin
viewLifecycleOwner.lifecycleScope.launch {
    repeatOnLifecycle(Lifecycle.State.STARTED) {
        viewModel.state.collect(::render)
    }
}
```

Ne lancez pas de coroutine d’interface dans un scope qui vit plus longtemps que
la View. N’utilisez pas `GlobalScope` pour les flux de l’interface ou de
l’application.

### Compose

Avec Compose, les effets de bord doivent être nettoyés au moyen de
`DisposableEffect` :

```kotlin
DisposableEffect(Unit) {
    registerListener(listener)
    onDispose { unregisterListener(listener) }
}
```

Ne conservez pas une `Activity` ou une `View` dans `remember` sans raison claire
liée au cycle de vie.

### Outils

- LeakCanary ;
- Android Studio Memory Profiler ;
- les heap dumps ;
- le suivi des allocations.

`WeakReference` n’est pas une solution architecturale normale. Si elle semble
nécessaire, il faut d’abord vérifier la propriété des objets et leur cycle de vie.

**En bref :** ne conservez pas d’objets Android de courte durée dans des classes
de longue durée, libérez les références aux Views d’un Fragment dans
`onDestroyView()`, utilisez des coroutines tenant compte du cycle de vie,
désinscrivez les callbacks et recherchez les fuites avec LeakCanary ou Profiler.

</details>

<details>
<summary>100. Comment stocker des clés API de manière sûre dans une application Android ?</summary>

#### Kotlin

Il est impossible de stocker un secret de manière totalement sûre dans une
application Android. Tout élément inclus dans un APK ou un AAB peut
potentiellement être extrait par décompilation, proxy, inspection de la mémoire
ou sur un appareil rooté.

Règle essentielle : les véritables secrets ne doivent pas être placés dans le
client.

Incorrect :

```kotlin
const val SECRET_API_KEY = "sk_live_..."
```

Si une clé donne accès à un backend, à une fonction d’administration, à des
paiements ou à un compte de service, elle doit rester uniquement sur le serveur.

Architecture correcte :

```text
application Android -> votre backend -> API tierce
```

Le backend ajoute le secret, vérifie l’utilisateur ou la session, applique une
limitation du débit et un audit, puis ne renvoie que le résultat nécessaire.

Toutes les clés ne sont pas équivalentes :

```text
clé publique/client -> peut être incluse dans l’application, mais doit être limitée
clé secrète/admin   -> ne doit jamais être incluse dans l’application
```

Lorsqu’une clé client doit malgré tout être présente dans l’application, il faut
la limiter selon :

- le nom du package ;
- l’empreinte SHA-256 du certificat ;
- les API autorisées ;
- les quotas et limites de débit ;
- la séparation des environnements ;
- la surveillance ;
- un plan de rotation.

`BuildConfig`, `local.properties` et les secrets de la CI permettent d’éviter de
commiter une clé dans Git, mais ne la protègent pas dans l’application compilée :

```kotlin
buildConfigField("String", "API_KEY", ""$apiKey"")
```

Après décompilation, cette clé reste visible.

Le NDK, R8/ProGuard et l’obfuscation des chaînes ne font que rendre l’extraction
plus difficile. Ils constituent un obstacle, et non une frontière de sécurité.

Android Keystore est utile pour les clés générées sur l’appareil et le stockage
des jetons utilisateur, mais ne protège pas un secret API codé en dur : un
attaquant peut trouver l’endroit où l’application l’utilise.

Les jetons d’accès et de renouvellement de l’utilisateur doivent être stockés
avec précaution :

- EncryptedSharedPreferences ou un DataStore chiffré ;
- Android Keystore pour la clé cryptographique ;
- une courte durée de vie du jeton d’accès ;
- la rotation du jeton de renouvellement ;
- la suppression du stockage lors de la déconnexion ;
- la prise en charge de la révocation par le backend.

**En bref :** les véritables secrets API doivent rester sur le backend. Seules
des clés publiques ou client limitées peuvent être présentes dans l’application.
`BuildConfig`, l’obfuscation et le NDK ne garantissent pas la sécurité ; il faut
des restrictions, une surveillance et une rotation.

</details>

<details>
<summary>101. Comment implémenter une architecture offline-first ?</summary>

#### Kotlin

Offline-first signifie que l’interface utilisateur lit une source de vérité
locale, tandis que le réseau sert uniquement à la synchroniser.

```text
UI -> ViewModel -> Repository -> Room
                         └----> API
```

### Chemin de lecture

L’interface utilisateur observe Room :

```kotlin
fun observeArticles(): Flow<List<Article>> =
    dao.observeArticles()
        .map { entities -> entities.map(ArticleEntity::toDomain) }
```

Une actualisation récupère les données distantes et les fusionne dans la base au
sein d’une transaction :

```kotlin
suspend fun refresh() {
    val remote = api.getArticles()

    database.withTransaction {
        dao.upsertAll(remote.map(ArticleDto::toEntity))
    }
}
```

Après l’écriture dans Room, l’interface est mise à jour par `Flow`. `replaceAll`
est dangereux si la réponse est partielle ou si des modifications locales sont
en attente.

### Chemin d’écriture et outbox

Une écriture hors ligne est d’abord enregistrée localement :

```kotlin
database.withTransaction {
    dao.updateTitle(id, title, SyncStatus.Pending)
    outboxDao.insert(PendingOperation.UpdateTitle(id, title))
}
```

L’interface affiche immédiatement la mise à jour optimiste. L’opération de
l’outbox est envoyée ultérieurement et n’est supprimée qu’après confirmation du
serveur.

La mise à jour locale et l’insertion dans l’outbox doivent appartenir à la même
transaction ; sinon, la mort du processus peut entraîner la perte de l’opération.

### Synchronisation

WorkManager assure les nouvelles tentatives et la reprise après la mort du
processus :

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

La synchronisation doit être idempotente, car une nouvelle tentative peut répéter
la requête. L’actualisation au premier plan et le worker doivent utiliser la même
implémentation de synchronisation.

### Conflits

La politique de résolution dépend du domaine :

- le serveur l’emporte ;
- le client l’emporte ;
- révision ou ETag ;
- fusion champ par champ ;
- résolution manuelle.

L’horodatage du client n’est pas fiable en raison du décalage des horloges. Dans
les domaines critiques, une simple stratégie du dernier écrit gagnant est
souvent inacceptable.

### État de l’interface utilisateur

L’interface doit distinguer :

- les données en cache ;
- le chargement initial sans données locales ;
- l’actualisation avec d’anciennes données ;
- le mode hors ligne ;
- les modifications en attente ou échouées ;
- la dernière synchronisation réussie.

La connectivité n’est qu’un indice. La véritable source d’information est le
résultat de la requête réelle.

**En bref :** une architecture offline-first repose sur une source de vérité
locale. Les lectures proviennent de Room, les données distantes sont fusionnées
dans la base, les écritures passent par une outbox transactionnelle, WorkManager
assure la synchronisation et le domaine définit la politique de résolution des
conflits.

</details>

<details>
<summary>102. Qu’est-ce que l’architecture MVI et quand faut-il l’utiliser ?</summary>

#### Kotlin

MVI est une approche architecturale reposant sur un flux de données
unidirectionnel :

```text
UI -> Événement/Intent -> ViewModel/Reducer -> État -> UI
                                           └-> Effet
```

L’interface utilisateur ne modifie pas directement l’état. Elle affiche un
`State` immuable et envoie un `Event`. La ViewModel traite l’événement et produit
un nouvel état.

Éléments courants :

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

- `State` — le modèle durable de l’écran ;
- `Event` ou `Intent` — une action de l’utilisateur ou une entrée système ;
- `Reducer` ou handler — la transition de l’ancien état vers le nouveau ;
- `Effect` — une action ponctuelle : navigation, snackbar ou demande
  d’autorisation.

ViewModel :

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

L’état est mis à jour avec `copy` ou `update` :

```kotlin
mutableState.update {
    it.copy(isLoading = false, user = loadedUser, error = null)
}
```

L’interface Compose se contente de lire l’état et d’envoyer des événements :

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()

ProfileScreen(
    state = state,
    onEvent = viewModel::onEvent
)
```

Avantages :

- une seule source de vérité pour l’état de l’écran ;
- des transitions prévisibles ;
- des tests plus simples du reducer ou de la ViewModel ;
- une bonne adéquation avec Compose ;
- une journalisation pratique des événements et des états.

Inconvénients :

- davantage de code répétitif ;
- le reducer peut devenir un objet omnipotent ;
- les effets ponctuels exigent une politique de livraison claire ;
- MVI ne résout pas automatiquement la concurrence et les conditions de course ;
- cette architecture peut être excessive pour un CRUD simple.

MVI convient aux écrans complexes comportant de nombreuses transitions d’état,
des nouvelles tentatives, des mises à jour optimistes, de la validation ou des
parcours en plusieurs étapes. Pour un écran simple, une `ViewModel` avec
`StateFlow` et des méthodes suffit.

**En bref :** MVI suit le cycle `Événement -> transition d’état -> rendu`. Il est
utile pour les interfaces complexes comportant de nombreux états, mais exige une
gestion rigoureuse des effets et de la concurrence, et peut être excessif pour
les écrans simples.

</details>

<details>
<summary>103. Comment concevoir des applications Android évolutives ?</summary>

#### Kotlin

Une application Android évolutive permet de modifier une fonctionnalité
localement, sans provoquer de changements en cascade dans tout le code.

Couches de base :

```text
UI -> ViewModel -> domaine -> repository -> API/DB
```

- l’interface affiche l’état ;
- la ViewModel gère l’état de l’écran et les événements ;
- le domaine contient les règles métier ;
- le repository masque les sources de données ;
- la couche données utilise Retrofit, Room ou des SDK.

Les couches ne doivent pas être ajoutées mécaniquement. Un cas d’utilisation ou
une interface est nécessaire lorsqu’il existe une règle métier, une frontière,
une autre implémentation possible ou un besoin de test distinct.

### Modules organisés par fonctionnalité

```text
:app
:core:network
:core:database
:core:designsystem
:feature:profile
:feature:checkout
```

Un module de fonctionnalité regroupe le code qui évolue ensemble. `core` doit
contenir une infrastructure réellement partagée, et non des utilitaires
disparates.

Règle de dépendance :

```text
app -> feature
feature -> core
core -X-> feature
```

Une fonctionnalité ne doit pas dépendre de l’implémentation d’une autre. Pour
leur intégration, il est préférable d’utiliser des contrats, une API de navigation
ou une séparation entre API et implémentation.

### Contrats et modèles

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Une interface est utile à une véritable frontière. Il ne faut pas en créer une
pour chaque classe « au cas où ».

Les DTO, entités, modèles métier et modèles d’interface sont séparés lorsqu’ils
possèdent des sémantiques ou des cycles de vie différents. S’ils sont identiques
et n’ont pas de raisons distinctes d’évoluer, un mapping supplémentaire est
contre-productif.

### État, erreurs et concurrence

L’interface doit recevoir un état observable unique et envoyer des événements.
La ViewModel doit définir explicitement sa politique de concurrence : annuler la
tâche précédente, ignorer les doublons, sérialiser ou fusionner.

Un `Throwable` brut ne doit pas faire partie du contrat de l’interface. Les
erreurs d’infrastructure doivent être converties en résultats métier ou
d’interface à la frontière appropriée.

### Injection, tests et build

- utiliser l’injection par constructeur par défaut ;
- faire correspondre les scopes au propriétaire du cycle de vie ;
- écrire des tests unitaires et métier sans Android ;
- tester l’état des ViewModels ;
- écrire des tests d’intégration des repositories ;
- utiliser des plugins de convention et un catalogue de versions ;
- exécuter la CI pour les modules affectés ;
- prévoir la journalisation, les métriques et le contexte des crashes ;
- définir un plan de migration pour les changements d’API et de base de données.

La modularité sans tests, observabilité ni répartition claire des responsabilités
ne permet pas au produit de passer à l’échelle.

**En bref :** l’évolutivité repose sur la responsabilité des fonctionnalités, un
graphe de dépendances maîtrisé, des contrats stables, des frontières testables,
une gestion explicite de l’état et de la concurrence, ainsi qu’un support adapté
à la production. Les modules et les couches ne doivent être ajoutés que
lorsqu’ils isolent réellement les changements.

</details>

<details>
<summary>104. Qu’est-ce que Jetpack Compose et en quoi diffère-t-il du système View ?</summary>

#### Kotlin

`Jetpack Compose` est un toolkit déclaratif d’interface utilisateur pour Android.
L’interface est décrite par des fonctions Kotlin `@Composable` comme le résultat
de l’état courant, sans XML ni mise à jour manuelle des `View`.

Le système View est impératif :

```kotlin
textView.text = user.name
progressBar.isVisible = isLoading
```

Compose est déclaratif :

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

Lorsque l’état change, Compose recompose les parties nécessaires de l’interface.

Fonction composable :

```kotlin
@Composable
fun Greeting(name: String) {
    Text(text = "Hello, $name")
}
```

Une fonction composable doit afficher l’état et appeler des callbacks. La logique
métier, les entrées-sorties et les opérations longues ne doivent pas résider dans
les fonctions d’interface.

Structure courante :

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

La ViewModel produit l’état, et Compose l’affiche.

Principales différences :

```text
Système View -> XML + arbre de Views + mises à jour manuelles + listeners
Compose      -> fonctions Kotlin + rendu piloté par l’état + recomposition
```

Points pratiques concernant Compose :

- le corps d’une fonction composable peut s’exécuter de nombreuses fois ;
- les effets de bord doivent être lancés au moyen des API d’effets ;
- l’état local et les objets doivent être mémorisés avec `remember` lorsque cela
  est nécessaire ;
- l’état de l’écran doit être conservé dans une ViewModel ;
- les listes paresseuses doivent posséder des clés stables ;
- les performances doivent être vérifiées avec un profiler, et non en comptant
  simplement les recompositions.

Compose et le système View peuvent être combinés :

```kotlin
ComposeView(context).setContent {
    ProfileScreen(state)
}
```

Une View peut également être intégrée à Compose avec `AndroidView`. La migration
peut donc être progressive.

**En bref :** Compose décrit l’interface comme une fonction de l’état et la met à
jour par recomposition. Le système View repose sur XML, un arbre de Views et des
modifications manuelles. Compose facilite les interfaces pilotées par l’état,
mais exige une gestion rigoureuse des effets de bord et de la propriété de
l’état.

</details>

<details>
<summary>105. Que sont `@Composable` et la recomposition ?</summary>

#### Kotlin

`@Composable` est une annotation destinée aux fonctions Jetpack Compose qui
décrivent l’interface utilisateur. La recomposition est le nouvel appel d’une
fonction composable lorsque l’état dont elle dépend a changé.

```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name")
}
```

Une fonction composable est une fonction de l’état :

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

Lorsque `state` change, Compose met à jour la partie nécessaire de l’interface
par recomposition.

Exemple d’état local :

```kotlin
var count by remember { mutableStateOf(0) }

Button(onClick = { count++ }) {
    Text("Count: $count")
}
```

La modification de `count` déclenche la recomposition de la partie de
l’interface qui lit cette valeur.

La recomposition peut être déclenchée par :

- `mutableStateOf` ;
- `StateFlow.collectAsStateWithLifecycle()` ;
- `LiveData.observeAsState()` ;
- `rememberSaveable` ;
- la modification des paramètres d’une fonction composable.

Le corps d’une fonction composable peut s’exécuter de nombreuses fois ; il ne
doit donc pas contenir d’effets de bord :

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // incorrect
}
```

Code correct :

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

`remember` conserve une valeur entre les recompositions :

```kotlin
val formatter = remember { DateTimeFormatter.ISO_DATE }
```

La recomposition n’est pas un problème en elle-même. Les véritables problèmes
sont le travail lourd dans une fonction composable, une lecture trop large de
l’état, des paramètres instables, des allocations inutiles ou des listes
paresseuses sans clés stables.

Règles pratiques :

- une fonction composable doit être légère et dépourvue d’effets de bord ;
- l’état doit être lu au plus près de son utilisation ;
- les objets coûteux doivent être mis en cache avec `remember` ;
- les effets doivent être lancés avec `LaunchedEffect`, `DisposableEffect` ou
  une API similaire ;
- les listes doivent utiliser des clés stables ;
- les performances doivent être vérifiées avec un profiler, et non estimées.

**En bref :** `@Composable` décrit l’interface comme une fonction de l’état. La
recomposition réexécute les fonctions composables nécessaires lorsque cet état
change. C’est un mécanisme normal de Compose si les fonctions d’interface sont
légères, sans effets de bord et lisent l’état au bon endroit.

</details>

<details>
<summary>106. Qu’est-ce que le state hoisting ?</summary>

#### Kotlin

Le state hoisting consiste à déplacer l’état d’une fonction composable vers son
parent ou vers un détenteur d’état. L’enfant reçoit une valeur et un callback, ce
qui le rend sans état, réutilisable et testable.

Variante avec état interne :

```kotlin
@Composable
fun SearchField() {
    var query by remember { mutableStateOf("") }
    TextField(query, onValueChange = { query = it })
}
```

Après le hoisting :

```kotlin
@Composable
fun SearchField(
    query: String,
    onQueryChange: (String) -> Unit
) {
    TextField(query, onValueChange = onQueryChange)
}
```

Le parent devient la source de vérité :

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

Pour l’état d’un écran, le propriétaire est généralement une `ViewModel` :

```kotlin
class SearchViewModel : ViewModel() {
    private val _state = MutableStateFlow(SearchState())
    val state = _state.asStateFlow()

    fun onQueryChange(query: String) {
        _state.update { it.copy(query = query) }
    }
}
```

La route Compose lit l’état et transmet les événements :

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

Le hoisting favorise un flux de données unidirectionnel :

```text
état vers le bas -> UI
événements vers le haut -> le propriétaire met à jour l’état
```

L’état doit être remonté jusqu’au propriétaire commun le plus proche qui doit le
lire ou le modifier. S’il n’est nécessaire qu’à l’intérieur d’un composant et
n’influence pas la logique métier, il peut rester local.

API courante d’une fonction composable sans état :

```kotlin
@Composable
fun ProfileHeader(
    user: UserUi,
    isFollowing: Boolean,
    onFollowClick: () -> Unit,
    modifier: Modifier = Modifier
)
```

**En bref :** le state hoisting applique le principe « état vers le bas,
événements vers le haut ». L’enfant ne possède pas l’état ; le parent ou la
ViewModel devient l’unique source de vérité.

</details>

<details>
<summary>107. Quelle est la différence entre `remember` et `rememberSaveable` ?</summary>

#### Kotlin

`remember` et `rememberSaveable` conservent une valeur entre les recompositions.
La différence est que `rememberSaveable` l’enregistre également dans l’état
sauvegardé et peut la restaurer après un changement de configuration ou une
recréation du processus.

```text
remember          -> survit aux recompositions
rememberSaveable  -> survit aux recompositions et à la recréation
```

`remember` :

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

Après la recréation de l’Activity, la valeur est perdue.

`rememberSaveable` :

```kotlin
@Composable
fun Counter() {
    var count by rememberSaveable { mutableStateOf(0) }

    Button(onClick = { count++ }) {
        Text("Count: $count")
    }
}
```

Sur Android, les types pouvant être placés dans le `Bundle` de l’état sauvegardé
sont pris en charge automatiquement : primitives, `String`, `Parcelable`,
`Serializable`, etc. Un type personnalisé nécessite un `Saver` :

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

Choix selon le besoin :

- `remember` — un état d’interface transitoire ou un objet mis en cache qui peut
  être perdu ;
- `rememberSaveable` — un petit état que l’utilisateur s’attend à conserver,
  comme un texte saisi, un onglet sélectionné ou un filtre ;
- `ViewModel` — l’état de l’écran ou la logique métier ;
- stockage, DataStore ou base de données — les données persistantes.

Limites de `rememberSaveable` :

- la taille de l’état sauvegardé est limitée ;
- les objets et collections volumineux ne doivent pas y être enregistrés ;
- il ne s’agit pas d’une persistance durable ;
- il ne remplace ni une `ViewModel` ni un repository.

**En bref :** `remember` ne vit que dans la composition. `rememberSaveable`
restaure un petit état d’interface sauvegardable après une recréation. L’état
d’un écran appartient à une `ViewModel`, tandis que les données durables
appartiennent à la couche de stockage.

</details>

<details>
<summary>108. Qu’est-ce que `rememberCoroutineScope` ?</summary>

#### Kotlin

`rememberCoroutineScope()` est une API Compose qui renvoie un `CoroutineScope`
lié à la composition courante. Ce scope est annulé lorsque la fonction composable
quitte la composition.

Il est utilisé lorsqu’une opération d’interface `suspend` doit être lancée depuis
un callback : clic, geste de balayage, drawer, snackbar ou animation de
défilement.

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

Une fonction `suspend` ne peut pas être appelée directement depuis `onClick` ;
un scope est donc nécessaire.

Autre exemple :

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

Différence avec `LaunchedEffect` :

```text
LaunchedEffect         -> la coroutine démarre automatiquement à l’entrée ou au changement de clé
rememberCoroutineScope -> la coroutine est lancée manuellement depuis un callback
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

`rememberCoroutineScope()` ne doit pas être utilisé pour la logique métier :

```kotlin
Button(onClick = {
    viewModel.onSaveClick()
})
```

Dans la ViewModel :

```kotlin
fun onSaveClick() {
    viewModelScope.launch { repository.save() }
}
```

`GlobalScope` ne doit pas être utilisé pour l’interface, car il n’est pas lié au
cycle de vie et peut continuer à travailler après la fermeture de l’écran.

**En bref :** `rememberCoroutineScope()` sert aux courtes opérations d’interface
`suspend` lancées depuis des callbacks, comme un snackbar, un drawer ou un
défilement. Utilisez `LaunchedEffect` pour les effets automatiques et
`viewModelScope` pour la logique d’écran ou métier.

</details>

<details>
<summary>109. Qu’est-ce que `collectAsState` ?</summary>

#### Kotlin

`collectAsState()` est une API Compose qui collecte un `Flow` et le convertit en
`State` Compose. Lorsque le Flow émet une nouvelle valeur, Compose déclenche une
recomposition aux endroits où cet état est lu.

ViewModel :

```kotlin
class ProfileViewModel : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state.asStateFlow()
}
```

Compose :

```kotlin
@Composable
fun ProfileRoute(viewModel: ProfileViewModel) {
    val state by viewModel.state.collectAsState()
    ProfileScreen(state = state)
}
```

Fonctionnement :

```text
émission du Flow -> mise à jour du State -> recomposition -> mise à jour de l’UI
```

Un `Flow` ordinaire nécessite une valeur initiale :

```kotlin
val items by repository.observeItems()
    .collectAsState(initial = emptyList())
```

Un `StateFlow` possède déjà une valeur initiale dans `state.value`.

Sur Android, il est préférable d’utiliser la variante tenant compte du cycle de
vie :

```kotlin
val state by viewModel.state.collectAsStateWithLifecycle()
```

Différence :

```text
collectAsState              -> runtime Compose, sans cycle de vie Android
collectAsStateWithLifecycle -> collecte uniquement dans un état actif du cycle de vie
```

Un Flow ne doit pas être collecté directement dans le corps d’une fonction
composable :

```kotlin
@Composable
fun Screen(flow: Flow<State>) {
    flow.collect { } // incorrect
}
```

Pour l’état d’un écran, utilisez `collectAsStateWithLifecycle()`. Pour les
événements ponctuels, utilisez `LaunchedEffect` et un `SharedFlow` distinct :

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // naviguer ou afficher un snackbar
    }
}
```

Un `State` représente une information pouvant être affichée à nouveau, tandis
qu’un `Event` représente une action ponctuelle.

Si un Flow émet trop fréquemment, il faut réduire le bruit avec
`distinctUntilChanged()`, des modèles d’état plus petits, des lectures locales
de l’état et l’absence de travail lourd dans les fonctions composables.

**En bref :** `collectAsState()` convertit un Flow en State Compose. Sur Android,
on utilise généralement `collectAsStateWithLifecycle()`. Les événements
ponctuels de navigation ou de snackbar doivent être traités avec
`LaunchedEffect`, et non comme un état.

</details>

<details>
<summary>110. Qu’est-ce que `Modifier` dans Compose ?</summary>

#### Kotlin

Dans Compose, `Modifier` décrit la mise en page, le dessin, les interactions, la
sémantique et le comportement d’une fonction composable : `padding`, `size`,
`background`, `clickable`, défilement, accessibilité, etc.

```kotlin
Text(
    text = "Hello",
    modifier = Modifier
        .padding(16.dp)
        .background(Color.Red)
        .clickable { println("Click") }
)
```

`Modifier` est immuable et se construit sous forme de chaîne. L’ordre est
important :

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

et :

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

produisent des résultats différents, car l’arrière-plan est appliqué à une étape
différente.

Une fonction composable réutilisable doit accepter un modifier de l’extérieur :

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

Le parent peut ainsi contrôler le positionnement et le comportement extérieur de
l’enfant.

Principaux types de modifiers :

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

Pour l’accessibilité, il est préférable d’utiliser des modifiers de haut niveau
comme `clickable`, car ils ajoutent une sémantique. Celle-ci peut également être
définie explicitement :

```kotlin
Modifier.semantics {
    contentDescription = "Profile image"
}
```

Certains modifiers ne fonctionnent que dans le scope du parent :

```kotlin
Row {
    Text("A", Modifier.weight(1f))
    Text("B", Modifier.weight(1f))
}
```

Règles pratiques :

- accepter `modifier: Modifier = Modifier` ;
- laisser le parent gérer le positionnement extérieur ;
- permettre à l’enfant d’ajouter ses modifiers internes après le modifier reçu ;
- respecter l’importance de l’ordre des modifiers ;
- ne pas exécuter de logique lourde dans leurs callbacks.

**En bref :** `Modifier` est une chaîne immuable de réglages d’une fonction
composable. Il contrôle la mise en page, le dessin, les interactions et la
sémantique. L’ordre des appels est important, et les fonctions composables
réutilisables doivent accepter un paramètre `modifier`.

</details>

<details>
<summary>111. Quelle est la différence entre `padding` et `offset` ?</summary>

#### Kotlin

`padding` et `offset` modifient la position d’un élément d’interface, mais
interviennent à des étapes différentes de la mise en page.

```text
padding -> modifie la mesure et l’espace réservé
offset  -> modifie le placement sans changer la taille déclarée
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

`padding` ajoute de l’espace autour du contenu et influence la taille vue par le
parent et les éléments voisins.

```kotlin
Row {
    Text("A", Modifier.padding(start = 20.dp))
    Text("B")
}
```

`B` est placé après `A`, espace de padding compris.

### `offset`

```kotlin
Text(
    text = "Hello",
    modifier = Modifier.offset(x = 16.dp, y = 8.dp)
)
```

`offset` déplace la fonction composable lors du placement, mais le parent réserve
son emplacement initial.

```kotlin
Row {
    Text("A", Modifier.offset(x = 20.dp))
    Text("B")
}
```

`B` n’est pas décalé ; les éléments peuvent donc se chevaucher. Ce comportement
est utile pour un badge, un glisser-déposer, une animation ou un décalage
décoratif.

### RTL

`offset(x, y)` tient compte du sens de la mise en page : une valeur `x` positive
déplace l’élément vers la fin logique. Pour utiliser des coordonnées physiques
sans inversion RTL, utilisez `absoluteOffset()`.

### Ordre des modifiers

L’ordre influence la mise en page, le dessin et la zone interactive :

```kotlin
Modifier
    .background(Color.Red)
    .padding(16.dp)
```

L’arrière-plan couvre la zone la plus grande.

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Red)
```

L’arrière-plan ne s’applique qu’après le padding.

Il en va de même pour `clickable` : placé avant `padding`, il inclut le padding
dans la zone cliquable ; placé après, il ne l’inclut pas.

**En bref :** `padding` ajoute de l’espace dans la mise en page et influence les
éléments voisins. `offset` déplace uniquement l’élément sans réserver de nouvel
espace. L’ordre des modifiers a une incidence essentielle sur l’apparence et la
détection des interactions.

</details>

<details>
<summary>112. Quelle est la différence entre `fillMaxSize` et `matchParentSize` ?</summary>

#### Kotlin

`fillMaxSize()` et `matchParentSize()` peuvent donner à un enfant la taille de
son parent, mais fonctionnent différemment.

```text
fillMaxSize()     -> utilise les contraintes maximales et influence la mesure
matchParentSize() -> reprend la taille déjà déterminée de la Box
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

`fillMaxSize()` demande au parent la taille maximale disponible. Si les
contraintes correspondent à l’écran, la fonction composable occupe tout l’écran.

Pour un seul axe, on utilise :

```kotlin
Modifier.fillMaxWidth()
Modifier.fillMaxHeight()
```

`fillMaxSize()` participe à la mesure et peut influencer la taille du layout
parent.

### `matchParentSize()`

`matchParentSize()` fonctionne uniquement dans un `BoxScope` et pour un enfant
direct d’une `Box` :

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

Cet enfant reprend la taille déjà calculée de la `Box`, sans la déterminer.

De manière simplifiée :

1. la `Box` mesure ses enfants ordinaires ;
2. elle détermine sa propre taille ;
3. les enfants utilisant `matchParentSize()` reçoivent cette taille définitive.

`matchParentSize()` convient donc aux superpositions : couche de chargement,
voile, dégradé ou couche cliquable.

Si la superposition utilise plutôt `fillMaxSize()`, l’enfant peut demander les
contraintes maximales et agrandir la `Box`, ce qui produit un comportement
différent.

Choix pratique :

- le conteneur racine doit occuper tout l’espace disponible —
  `fillMaxSize()` ;
- une superposition doit couvrir une `Box` dimensionnée par son contenu —
  `matchParentSize()` ;
- l’enfant ne doit pas influencer la taille de la `Box` —
  `matchParentSize()` ;
- hors d’un `BoxScope`, `matchParentSize()` n’est pas disponible.

**En bref :** `fillMaxSize()` demande tout l’espace disponible et participe à la
mesure. Dans un `BoxScope`, `matchParentSize()` reprend uniquement la taille déjà
déterminée de la `Box` sans l’agrandir.

</details>

<details>
<summary>113. Comment gérer le bouton « Retour » dans Compose ?</summary>

#### Kotlin

Dans Compose, l’appui sur Retour est géré avec `BackHandler` fourni par
`androidx.activity.compose`. Il intercepte le retour système dans la portée d’une
fonction composable.

Exemple de base :

```kotlin
@Composable
fun EditScreen(onBack: () -> Unit) {
    BackHandler {
        onBack()
    }

    // contenu
}
```

`BackHandler` peut être activé conditionnellement :

```kotlin
BackHandler(enabled = state.hasUnsavedChanges) {
    showDiscardDialog = true
}
```

Si `enabled = false`, l’événement Retour est transmis à un autre handler ou au
système de navigation.

Un cas courant est l’affichage d’une boîte de confirmation en présence de
modifications non enregistrées :

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

Avec un `NavController`, le retour standard fonctionne souvent automatiquement
grâce à la pile de navigation. Un `BackHandler` personnalisé est nécessaire
lorsque ce comportement doit être modifié : boîte de dialogue, bottom sheet,
mode de sélection ou modifications non enregistrées.

Pour un état local de l’interface, Retour doit d’abord fermer la couche
supérieure :

```kotlin
BackHandler(enabled = sheetState.isVisible) {
    scope.launch { sheetState.hide() }
}
```

Ordre de priorité :

```text
dialogue/sheet/sélection -> modifications non enregistrées -> navigation de l’écran
```

Pour une logique complexe, il est préférable de transmettre un événement à la
ViewModel :

```kotlin
BackHandler {
    viewModel.onBackClicked()
}
```

La ViewModel décide alors d’afficher une boîte de dialogue, d’enregistrer un
brouillon, d’autoriser la navigation ou d’émettre un effet.

Erreurs courantes :

- intercepter systématiquement le bouton Retour ;
- ne pas utiliser `enabled` ;
- dupliquer la logique entre `BackHandler` et `NavController` ;
- ignorer l’état d’une boîte de dialogue ou d’un sheet ;
- éloigner la logique de retour de l’état qu’elle traite.

**En bref :** dans Compose, le bouton Retour est géré avec
`BackHandler(enabled = ...)`. Il convient aux couches supérieures de l’interface
et aux comportements particuliers, tandis que la navigation ordinaire peut
souvent rester sous la responsabilité du `NavController`.

</details>

<details>
<summary>114. À quoi sert `rememberUpdatedState` ?</summary>

#### Kotlin

`rememberUpdatedState()` fournit à un effet de longue durée la valeur ou le
callback le plus récent sans redémarrer cet effet.

Problème :

```kotlin
@Composable
fun SplashScreen(onTimeout: () -> Unit) {
    LaunchedEffect(Unit) {
        delay(2_000)
        onTimeout()
    }
}
```

`LaunchedEffect(Unit)` ne redémarre pas lorsque `onTimeout` change. La coroutine
peut donc appeler une ancienne lambda.

Solution :

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

Le minuteur ne recommence pas, mais le callback le plus récent est appelé après
le délai.

Règle principale :

```text
la modification de la valeur doit-elle redémarrer l’effet ? -> clé
la modification de la valeur ne doit-elle pas redémarrer l’effet ? -> rememberUpdatedState
```

Si le callback devient une clé :

```kotlin
LaunchedEffect(onTimeout) {
    delay(2_000)
    onTimeout()
}
```

alors toute modification de la lambda annule la coroutine précédente et relance
le minuteur, ce qui n’est pas toujours souhaitable.

Exemple avec `DisposableEffect` :

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

L’observateur n’est réenregistré que lorsque `owner` change, mais il appelle
toujours le handler le plus récent.

Cas où la valeur doit être une clé :

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Une modification de `userId` représente une nouvelle opération ; l’ancienne doit
donc être annulée et une nouvelle lancée.

`rememberUpdatedState()` ne lance pas de coroutine, ne remplace pas les clés des
effets et ne conserve pas la valeur après la sortie de la composition. Il renvoie
simplement un `State<T>` contenant la valeur actuelle pour un effet de longue
durée.

**En bref :** `rememberUpdatedState()` permet à un effet d’utiliser la dernière
valeur ou le dernier callback sans redémarrer. Si une modification représente
une nouvelle opération, la valeur doit être une clé, et non être enveloppée dans
`rememberUpdatedState`.

</details>

<details>
<summary>115. Que sont les effets de bord dans Compose et comment les gérer ?</summary>

#### Kotlin

Dans Compose, un effet de bord est une action extérieure au rendu pur de
l’interface : coroutine, snackbar, navigation, analyse, listener, observateur du
cycle de vie ou synchronisation avec une API externe.

Le corps d’une fonction composable peut s’exécuter de nombreuses fois ; un effet
de bord ne doit donc pas y être lancé directement :

```kotlin
@Composable
fun Screen() {
    analytics.track("opened") // incorrect
}
```

Compose fournit des API d’effets à cet effet.

### `LaunchedEffect`

Lance une coroutine liée à la composition :

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

Si `userId` change, l’ancienne coroutine est annulée et une nouvelle démarre.

### `rememberCoroutineScope`

Sert aux opérations d’interface `suspend` déclenchées depuis un callback :

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

Il convient aux snackbars, drawers, bottom sheets et animations de défilement.

### `DisposableEffect`

Sert aux listeners et observateurs nécessitant un nettoyage :

```kotlin
DisposableEffect(lifecycleOwner) {
    val observer = LifecycleEventObserver { _, event ->
        if (event == Lifecycle.Event.ON_RESUME) viewModel.refresh()
    }

    lifecycleOwner.lifecycle.addObserver(observer)
    onDispose { lifecycleOwner.lifecycle.removeObserver(observer) }
}
```

Tout abonnement doit être annulé dans `onDispose`.

### `SideEffect`

S’exécute après une recomposition réussie :

```kotlin
SideEffect {
    systemUiController.setStatusBarColor(Color.Black)
}
```

Il ne convient pas aux opérations `suspend`.

### Autres API

- `rememberUpdatedState` — fournit la dernière valeur ou le dernier callback dans
  un effet de longue durée sans le redémarrer ;
- `produceState` — convertit une source asynchrone en `State` Compose ;
- `snapshotFlow` — convertit un état Compose en Flow.

Guide pratique :

```text
coroutine de composition       -> LaunchedEffect
coroutine depuis un callback   -> rememberCoroutineScope
listener avec nettoyage        -> DisposableEffect
synchronisation après rendu    -> SideEffect
dernier callback sans restart  -> rememberUpdatedState
source asynchrone vers State   -> produceState
état Compose vers Flow         -> snapshotFlow
```

**En bref :** les effets de bord ne doivent pas être lancés dans le corps d’une
fonction composable. Les API d’effets permettent de contrôler explicitement leur
cycle de vie, leur redémarrage, leur annulation et leur nettoyage.

</details>

<details>
<summary>116. Que sont les clés de `LaunchedEffect` et pourquoi sont-elles importantes ?</summary>

#### Kotlin

Les clés de `LaunchedEffect` déterminent le cycle de vie de la coroutine de
l’effet : quand elle doit démarrer, être annulée et redémarrer. Lorsqu’une clé
change, Compose annule l’ancienne coroutine et en lance une nouvelle.

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

Comportement :

- la fonction composable entre dans la composition — l’effet démarre ;
- une recomposition survient sans modification de `userId` — l’effet ne
  redémarre pas ;
- `userId` change — l’ancienne coroutine est annulée et une nouvelle démarre ;
- la fonction composable quitte la composition — la coroutine est annulée.

Erreur courante :

```kotlin
LaunchedEffect(Unit) {
    viewModel.loadUser(userId)
}
```

Si `userId` change, l’effet ne redémarre pas et peut produire des données
obsolètes. Code correct :

```kotlin
LaunchedEffect(userId) {
    viewModel.loadUser(userId)
}
```

`LaunchedEffect(Unit)` convient lorsqu’un effet doit démarrer une seule fois à
l’entrée dans la composition :

```kotlin
LaunchedEffect(Unit) {
    viewModel.effects.collect { effect ->
        // navigation/snackbar
    }
}
```

Plusieurs clés :

```kotlin
LaunchedEffect(userId, filter) {
    viewModel.loadUserPosts(userId, filter)
}
```

L’effet redémarre si au moins une clé change. Toute valeur qui détermine le
résultat de l’effet de bord doit être une clé.

Il ne faut pas utiliser de clés instables :

```kotlin
LaunchedEffect(System.currentTimeMillis()) {
    viewModel.load()
}
```

L’effet redémarrerait alors de manière incontrôlée.

Pour utiliser la valeur la plus récente dans un effet sans le redémarrer, on
emploie `rememberUpdatedState` :

```kotlin
val currentOnTimeout by rememberUpdatedState(onTimeout)

LaunchedEffect(Unit) {
    delay(2_000)
    currentOnTimeout()
}
```

Règle pratique :

```text
la modification doit redémarrer l’effet -> clé
dernière valeur sans redémarrage         -> rememberUpdatedState
```

**En bref :** les clés de `LaunchedEffect` contrôlent le lancement, l’annulation
et le redémarrage de la coroutine. Des clés incorrectes entraînent des données
obsolètes, des redémarrages inutiles ou un comportement instable de l’interface.

</details>

<details>
<summary>117. Qu’est-ce que `snapshotFlow` ?</summary>

#### Kotlin

`snapshotFlow` est une API Compose qui convertit les lectures d’un état snapshot
Compose en un `Flow` froid. Elle s’utilise lorsque les modifications d’un état
Compose doivent être traitées avec des opérateurs Flow ou dans un pipeline
d’effets de bord.

Exemple de base :

```kotlin
LaunchedEffect(listState) {
    snapshotFlow { listState.firstVisibleItemIndex }
        .distinctUntilChanged()
        .collect { index ->
            println("First visible item: $index")
        }
}
```

`snapshotFlow` observe les lectures d’état Compose effectuées dans sa lambda.
Lorsque la valeur lue change, le Flow émet une nouvelle valeur.

Cas d’usage courants :

- analyse du défilement ;
- déclenchement de la pagination ;
- `debounce` ou `sample` d’un état d’interface ;
- journalisation et débogage ;
- effets de bord qui ne doivent pas être lancés dans le corps d’une fonction
  composable.

Exemple de pagination :

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

Comme le résultat est un `Flow`, ses opérateurs peuvent être utilisés :

```kotlin
snapshotFlow { searchQuery }
    .debounce(300)
    .distinctUntilChanged()
    .collectLatest { query ->
        viewModel.search(query)
    }
```

`snapshotFlow` est généralement collecté dans un `LaunchedEffect`. La collecte
ne doit pas être lancée directement dans le corps d’une fonction composable.

Il ne faut pas utiliser `snapshotFlow` pour le rendu ordinaire de l’interface :

```kotlin
Text(text = state.title)
```

Pour simplement afficher un état, il suffit de le lire directement.
`snapshotFlow` sert précisément à passer d’un état Compose à un Flow ou à une
logique d’effets de bord.

Point important : la lambda doit lire un état snapshot Compose. Sans cette
lecture, `snapshotFlow` ne possède rien à observer.

**En bref :** `snapshotFlow` convertit les lectures d’un état Compose en `Flow`.
Il convient à l’analyse, la pagination, le debounce et les effets de bord dans
un `LaunchedEffect`, mais n’est pas nécessaire pour le simple rendu de
l’interface.

</details>

<details>
<summary>118. Comment optimiser la recomposition dans Compose ?</summary>

#### Kotlin

Optimiser la recomposition ne signifie pas supprimer toutes les recompositions,
mais les rendre peu coûteuses et locales. La recomposition est normale ; le
problème vient d’un travail lourd ou d’une zone de mise à jour trop étendue.

### Lire l’état au plus près de son utilisation

Incorrect :

```kotlin
ProfileScreen(state = hugeState)
```

Préférable :

```kotlin
ProfileHeader(user = state.user)
ProfileStats(stats = state.stats)
```

Une fonction composable qui lit un état peut être recomposée lorsque celui-ci
change. Il ne faut pas transmettre tout l’état de l’écran lorsqu’un seul champ
est nécessaire.

### Listes paresseuses : clés et `contentType`

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

Des clés stables aident Compose à distinguer les éléments et à conserver leur
état.

Pour différents types d’éléments :

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

### Ne pas effectuer de travail lourd dans une fonction composable

Incorrect :

```kotlin
val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
```

Préférable :

```kotlin
val formatter = remember { SimpleDateFormat("dd.MM.yyyy", Locale.getDefault()) }
```

Il est encore préférable de préparer le texte formaté dans un mapper ou une
ViewModel lorsqu’il fait partie du modèle d’interface.

### Utiliser `derivedStateOf` pour les changements fréquents

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

Cette approche est utile lorsque l’état source change fréquemment, mais que
l’interface ne doit réagir qu’à une condition dérivée.

### Modèles stables et immuables

Compose fonctionne mieux avec des modèles d’interface immuables :

```kotlin
data class UserUi(
    val id: String,
    val name: String
)
```

Les collections mutables et les objets instables peuvent empêcher Compose
d’éviter des recompositions inutiles.

### Effets de bord uniquement avec les API d’effets

```kotlin
LaunchedEffect(Unit) {
    analytics.track("opened")
}
```

Le corps d’une fonction composable peut s’exécuter de nombreuses fois ;
l’analyse, la navigation, les listeners et les opérations `suspend` ne doivent
donc pas y être lancés directement.

### Vérifier avec des outils de profilage

- compteurs de recomposition dans Layout Inspector ;
- métriques du compilateur Compose ;
- Android Studio Profiler ;
- Macrobenchmark ;
- Baseline Profiles.

**En bref :** l’optimisation de la recomposition repose sur des lectures locales
de l’état, de petites fonctions composables, des clés stables, `contentType`, des
modèles d’interface immuables, `remember`, `derivedStateOf`, des effets correctement
gérés et le profilage. L’objectif est d’obtenir des mises à jour locales peu
coûteuses, et non de supprimer toute recomposition.

</details>

<details>
<summary>119. Comment déboguer les problèmes de recomposition ?</summary>

#### Kotlin

Les problèmes de recomposition doivent être débogués par la mesure, et non par
des suppositions. L’objectif est d’identifier l’état qui change fréquemment et la
partie de l’interface qu’il affecte.

### Layout Inspector

Layout Inspector d’Android Studio affiche le nombre de recompositions des
fonctions composables. Ce nombre n’est toutefois pas un problème en lui-même :
une fonction légère peut être recomposée fréquemment sans impact visible.

### Logs temporaires

Pour une vérification rapide, il est possible d’ajouter un log :

```kotlin
@Composable
fun UserRow(user: UserUi) {
    Log.d("Recompose", "UserRow ${user.id}")
    Text(user.name)
}
```

Ou d’utiliser `SideEffect` :

```kotlin
@Composable
fun RecomposeLogger(tag: String) {
    SideEffect {
        Log.d("Recompose", tag)
    }
}
```

Il s’agit uniquement d’un outil de débogage, et non de code destiné à la
production.

### Vérifier les lectures de l’état

Une cause fréquente est la lecture d’un état trop large :

```kotlin
UserRow(screenState = state)
```

Préférable :

```kotlin
UserRow(
    user = user,
    isSelected = selectedUserId == user.id
)
```

L’état doit être lu au plus près de son utilisation.

### Vérifier les listes et les paramètres

Une `LazyColumn` doit utiliser des clés stables :

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

Il faut également rechercher les paramètres instables : collections mutables,
objets ou lambdas recréés inutilement, ainsi que le travail lourd exécuté dans le
corps d’une fonction composable.

### Métriques du compilateur et profiler

Les métriques du compilateur Compose permettent d’identifier les fonctions et
types instables, redémarrables ou pouvant être ignorés. Android Studio Profiler
et Macrobenchmark servent à confirmer l’impact réel sur les performances.

Checklist pratique :

- consulter les compteurs dans Layout Inspector ;
- identifier l’état qui change fréquemment ;
- réduire la portée des lectures d’état ;
- vérifier les clés stables des listes paresseuses ;
- retirer le travail lourd du corps des fonctions ;
- rechercher les paramètres instables ;
- utiliser `remember` ou `derivedStateOf` uniquement pour une raison précise ;
- confirmer le résultat avec le profiler et les métriques.

**En bref :** la recomposition se débogue avec Layout Inspector, des logs, les
métriques du compilateur et le profiler. Les causes les plus courantes sont une
lecture trop large de l’état, des paramètres instables, des clés absentes, un
travail lourd dans le corps ou une mauvaise propriété de l’état.

</details>

<details>
<summary>120. Comment éviter les ralentissements dans Compose, notamment pendant le défilement ?</summary>

#### Kotlin

Les ralentissements pendant le défilement sont le plus souvent dus à un travail
lourd sur le thread principal, à des recompositions inutiles, à des allocations
dans les éléments, à des clés instables ou à une interface de ligne trop complexe.

### Clés stables et `contentType`

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

`key` conserve l’identité d’un élément après une insertion, une suppression ou
un changement d’ordre.

Pour différents types d’éléments :

```kotlin
items(
    items = feedItems,
    key = { it.id },
    contentType = { it.type }
) { item ->
    FeedItem(item)
}
```

`contentType` aide Compose à réutiliser la composition d’éléments compatibles.

### Ne pas effectuer de travail lourd dans un élément

Incorrect :

```kotlin
items(users) { user ->
    val formatter = SimpleDateFormat("dd.MM.yyyy", Locale.getDefault())
    Text(formatter.format(user.createdAt))
}
```

Il est préférable de préparer le texte formaté dans un mapper ou une ViewModel,
ou au minimum de mettre l’objet en cache avec `remember`. Les entrées-sorties, le
décodage de bitmaps, le tri, le filtrage et les calculs lourds ne doivent pas être
effectués dans une fonction composable.

### Réduire la portée des lectures d’état

Chaque élément ne doit recevoir que ses propres données :

```kotlin
UserRow(
    user = user,
    onClick = { onUserClick(user.id) }
)
```

Il ne faut pas transmettre tout l’état de l’écran à chaque ligne.

### Modèles d’interface stables

Utilisez des modèles d’interface immuables et évitez de créer sans nécessité de
nouvelles collections ou de nouveaux wrappers à chaque recomposition.

```kotlin
data class UserUi(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

### État dérivé du défilement

```kotlin
val showButton by remember {
    derivedStateOf { listState.firstVisibleItemIndex > 0 }
}
```

`derivedStateOf` est utile lorsque le défilement change fréquemment, mais que
l’interface ne réagit qu’à une condition dérivée.

### Images et pagination

Pour les images, utilisez un chargeur avec cache mémoire et disque ainsi qu’une
taille cible appropriée. Une bitmap ne doit pas être décodée manuellement dans
une fonction composable ou sur le thread principal.

Pour les longues listes, utilisez Paging :

```kotlin
val items = pager.collectAsLazyPagingItems()
```

### Profilage

Les optimisations doivent être confirmées avec :

- les compteurs de recomposition de Layout Inspector ;
- Android Studio Profiler ou System Trace ;
- Macrobenchmark dans une version proche de la production ;
- Baseline Profiles.

**En bref :** un défilement fluide dans Compose exige des clés stables,
`contentType`, des lectures d’état ciblées, des éléments légers, des modèles
immuables, des images mises en cache, la pagination et une vérification avec le
profiler. L’essentiel est de ne pas bloquer le thread principal ni effectuer de
travail lourd pendant la composition.

</details>

<details>
<summary>121. Quelle est la différence entre les annotations `Stable` et `Immutable` ?</summary>

#### Kotlin

Dans Compose, `@Stable` et `@Immutable` constituent un contrat avec le compilateur
et le runtime qui les aide à déterminer si une recomposition peut être ignorée.
Il ne s’agit pas d’une « optimisation magique », mais de la promesse qu’un type
se comporte d’une certaine manière.

Différence principale :

```text
@Immutable -> l’objet ne change pas après sa création
@Stable    -> l’objet peut changer, mais Compose observe ces changements
```

### `@Immutable`

Convient aux modèles de valeur ou d’interface :

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val name: String,
    val avatarUrl: String
)
```

Condition : toutes les propriétés publiques restent inchangées après la création,
et leurs types sont eux-mêmes immuables ou stables.

Incorrect :

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: MutableList<String>
)
```

Une `MutableList` peut être modifiée sur place sans que Compose détecte
nécessairement le changement.

Préférable :

```kotlin
@Immutable
data class UserUiModel(
    val id: String,
    val tags: List<String>
)
```

Pour une garantie plus stricte, il est possible d’utiliser des collections
immuables.

### `@Stable`

Convient aux détenteurs d’état et aux objets contrôleurs dont les modifications
passent par un état Compose observable :

```kotlin
@Stable
class CounterState {
    var count by mutableIntStateOf(0)
}
```

L’objet mutable est acceptable, car Compose observe la modification de `count`.

Incorrect :

```kotlin
@Stable
class SearchState {
    var query: String = ""
}
```

Un champ mutable ordinaire ne signale pas ses modifications à Compose.

Choix selon le besoin :

- `@Immutable` — modèles d’interface, objets-valeurs et data classes représentant
  l’état d’un écran ;
- `@Stable` — détenteurs d’état personnalisés utilisant `mutableStateOf`,
  contrôleurs et objets avec une mutation observable et maîtrisée.

Erreurs courantes :

- annoter un objet mutable avec `@Immutable` ;
- appliquer `@Stable` à une classe possédant des champs mutables ordinaires ;
- conserver une `MutableList` ou une `MutableMap` dans l’état de l’interface ;
- modifier l’ancien état au lieu d’en créer un nouveau ;
- utiliser ces annotations comme remède à une mauvaise propriété de l’état.

**En bref :** `@Immutable` signifie que l’objet ne change pas après sa création.
`@Stable` signifie qu’il peut changer, mais que Compose observe correctement ces
modifications. Une annotation ne doit être appliquée que si le type respecte
réellement ce contrat.

</details>

<details>
<summary>122. Pourquoi fournir une clé à `LazyColumn` et que représente-t-elle en interne ?</summary>

#### Kotlin

Dans une `LazyColumn`, `key` attribue à un élément une identité stable,
indépendante de sa position.

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

Sans clé, Compose utilise la position. Cela convient à une liste statique, mais
pose problème lors d’une insertion, suppression ou réorganisation.

```text
Avant :      Après insertion :
0 -> Alice   0 -> Kate
1 -> Bob     1 -> Alice
2 -> Chris   2 -> Bob
             3 -> Chris
```

Avec la position, Compose voit un nouveau contenu dans les anciens slots. Avec
`user.id`, il comprend que les anciens éléments ont simplement changé de
position.

### État mémorisé

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

Sans clé stable, `expanded` peut rester associé à une position et passer à un
autre utilisateur. La clé déplace l’état mémorisé ou sauvegardé avec l’identité
de l’élément.

### Fonctionnement interne

La clé devient l’identité d’un groupe de composition. Le runtime l’utilise pour :

- associer l’ancien groupe au nouveau ;
- déplacer ou réutiliser le groupe ;
- conserver l’état de `remember` ;
- restaurer l’état de `rememberSaveable` ;
- gérer correctement les animations des éléments.

Ce mécanisme n’est pas `DiffUtil` : la clé sert à la gestion de la composition et
du placement dans un layout paresseux.

### Exigences concernant la clé

Une clé doit être :

- unique parmi les éléments frères ;
- stable entre les recompositions ;
- liée à l’identité métier ;
- sauvegardable si l’élément utilise `rememberSaveable`.

Correct :

```kotlin
key = { it.id }
```

Incorrect :

```kotlin
key = { UUID.randomUUID() }
key = { System.currentTimeMillis() }
key = { index }
```

Une valeur aléatoire ou temporelle crée une nouvelle identité à chaque
recomposition. L’indice correspond à l’identité de position par défaut et ne
convient qu’aux listes réellement statiques.

Une clé composite est acceptable si elle est stable :

```kotlin
key = { item -> "${item.type}:${item.id}" }
```

Avec Paging, on utilise l’API de clé des éléments :

```kotlin
items(
    count = users.itemCount,
    key = users.itemKey { it.id }
) { index ->
    users[index]?.let(::UserItem)
}
```

**En bref :** `key` associe un élément paresseux à une identité stable plutôt
qu’à un indice. Elle est nécessaire pour les listes dynamiques, l’état local des
éléments, la réorganisation, les animations et Paging. La meilleure clé est un
identifiant stable provenant du serveur, de la base de données ou généré côté
client.

</details>

<details>
<summary>123. Quels autres paramètres peut-on transmettre à `LazyColumn.items()` et à quoi sert `contentType` ?</summary>

#### Kotlin

Les paramètres importants de `LazyColumn.items()` sont `key` et `contentType`.

Utilisation de base :

```kotlin
LazyColumn {
    items(users) { user ->
        UserRow(user)
    }
}
```

Pour une liste dynamique, il est préférable de fournir explicitement une clé :

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

`key` définit l’identité stable d’un élément. Elle empêche Compose de confondre
les états mémorisés après une insertion, une suppression ou une réorganisation.

Une bonne clé :

- est unique ;
- reste stable ;
- ne dépend pas de la position ;
- n’est pas aléatoire ni fondée sur l’heure ;
- repose sur un identifiant métier.

`contentType` décrit le type d’interface de l’élément :

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

Par exemple : `header`, `post`, `ad` ou `loader`. Cette information aide Compose
à réutiliser plus efficacement la composition d’éléments du même type dans une
liste hétérogène.

Exemple avec des éléments `sealed` :

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

Si l’indice est nécessaire, on utilise `itemsIndexed` :

```kotlin
itemsIndexed(
    items = users,
    key = { _, user -> user.id }
) { index, user ->
    UserRow(index, user)
}
```

L’indice ne doit pas servir de clé pour une liste mutable ou réorganisable.

`contentType` est moins important lorsque tous les éléments possèdent la même
structure visuelle. Il est particulièrement utile pour les flux, conversations
et catalogues hétérogènes.

**En bref :** `key` représente l’identité stable d’un élément, tandis que
`contentType` décrit son type ou sa structure visuelle afin d’améliorer la
réutilisation des compositions dans les listes hétérogènes. Une clé est presque
toujours nécessaire pour une liste dynamique en production ; `contentType`
devient utile lorsque les lignes sont de types différents.

</details>

<details>
<summary>124. Qu’est-ce que SOLID ?</summary>

#### Kotlin

`SOLID` désigne cinq principes de conception orientée objet qui réduisent le
couplage, améliorent la testabilité et rendent les modifications plus locales.

### S — Principe de responsabilité unique

Une classe ne doit avoir qu’une seule raison de changer.

Il est déconseillé qu’une `ViewModel` gère à la fois le réseau, la base de
données, le mapping et l’état de l’interface. Il est préférable de séparer ces
responsabilités :

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
)
```

La `ViewModel` gère l’état, le cas d’utilisation l’action métier et le repository
les données.

### O — Principe ouvert/fermé

Le code doit être ouvert à l’extension, mais ne pas exiger de modifications
constantes.

```kotlin
interface AnalyticsTracker {
    fun track(event: AnalyticsEvent)
}
```

Une nouvelle implémentation peut être ajoutée sans réécrire le code client.

### L — Principe de substitution de Liskov

Un sous-type doit pouvoir remplacer correctement son type de base.

Incorrect :

```kotlin
class ReadOnlyRepository : UserRepository {
    override suspend fun save(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Si une implémentation ne respecte pas le contrat, celui-ci est trop large ou mal
défini.

### I — Principe de ségrégation des interfaces

Un client ne doit pas dépendre de méthodes dont il n’a pas besoin.

Incorrect :

```kotlin
interface UserRepository {
    suspend fun getUser(): User
    suspend fun uploadAvatar(uri: Uri)
    suspend fun deleteUser()
}
```

Préférable :

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

### D — Principe d’inversion des dépendances

Le code de haut niveau doit dépendre d’abstractions plutôt que
d’implémentations concrètes.

Incorrect :

```kotlin
class LoginUseCase {
    private val api = AuthApi()
}
```

Préférable :

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Le cas d’utilisation peut ainsi être facilement testé avec un fake repository.

Sur Android, cette organisation prend généralement la forme suivante :

```text
UI -> ViewModel -> Cas d’utilisation -> Contrat du repository -> Implémentation -> API/DB
```

Règle pratique : SOLID ne signifie pas qu’il faut créer une interface pour chaque
classe. Une abstraction est nécessaire lorsqu’il existe une véritable frontière,
une autre implémentation possible ou un point de substitution pour les tests.

**En bref :** SOLID aide à séparer l’interface, la logique métier et la couche
données, à éviter les classes omnipotentes, à conserver des contrats étroits et
à rendre le code testable. Des abstractions excessives et injustifiées sont
toutefois également nuisibles.

</details>

<details>
<summary>125. Expliquez chaque principe SOLID avec des exemples Android.</summary>

#### Kotlin

SOLID aide à séparer les responsabilités, à conserver des contrats clairs et à
tester la logique métier sans dépendre du framework Android, de Retrofit ou de
Room.

### S — Responsabilité unique

Une classe ne doit avoir qu’une seule raison de changer.

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

La `ViewModel` gère l’état de l’interface, le cas d’utilisation l’opération métier
et le repository les données. SRP ne signifie pas « une seule méthode par
classe », mais interdit de mélanger plusieurs raisons distinctes de changer.

### O — Ouvert/fermé

Le code est étendu au moyen de nouvelles implémentations plutôt que par la
modification constante de la logique existante.

```kotlin
interface EventHandler {
    fun canHandle(event: Event): Boolean
    fun handle(event: Event)
}
```

Un nouveau handler est ajouté dans une classe distincte. Une abstraction n’est
toutefois justifiée que lorsqu’il existe une véritable variabilité.

### L — Substitution de Liskov

Un sous-type doit pouvoir remplacer correctement son type de base.

Il serait incorrect qu’un `ReadOnlyStorage` implémente `save()` en lançant une
`UnsupportedOperationException`. Il est préférable de séparer le contrat :

```kotlin
interface DataReader {
    suspend fun read(): Data
}

interface DataWriter {
    suspend fun save(data: Data)
}
```

LSP ne concerne pas seulement la signature, mais également les garanties, les
erreurs, les effets de bord et les invariants.

### I — Ségrégation des interfaces

L’appelant ne doit pas dépendre de méthodes dont il n’a pas besoin.

```kotlin
interface UserReader {
    suspend fun getUser(): User
}

interface AvatarUploader {
    suspend fun uploadAvatar(uri: Uri)
}
```

Le cas d’utilisation reçoit un contrat étroit plutôt qu’une interface
omnipotente. Des interfaces trop petites sans rôle clair pour l’appelant
constituent toutefois également du code répétitif.

### D — Inversion des dépendances

La logique métier de haut niveau dépend d’une abstraction, que la couche données
implémente.

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

Le cas d’utilisation ne crée pas directement Retrofit ou Room. L’injection de
dépendances fournit l’implémentation réelle ou un fake.

```text
UI -> ViewModel -> Cas d’utilisation -> Contrat du repository
                                        ^
                                        |
                           Implémentation data -> API/DB
```

Violations courantes sur Android :

- un Fragment appelle directement une API ou un DAO ;
- une ViewModel contient le réseau, le mapping, le cache et la navigation ;
- un seul repository est responsable de toute l’application ;
- une interface est créée pour chaque classe sans véritable frontière ;
- le domaine dépend de types Android ou d’infrastructure ;
- un sous-type implémente formellement une interface, mais enfreint son contrat.

**En bref :** SRP sépare les responsabilités, OCP définit les points d’extension,
LSP garantit la substitution correcte des sous-types, ISP impose des contrats
étroits et DIP fait dépendre la logique métier d’abstractions. Sur Android, ces
principes séparent l’interface et le domaine du framework et des détails de la
couche données.

</details>

<details>
<summary>126. Qu’est-ce que la programmation orientée objet ?</summary>

#### Kotlin

La programmation orientée objet est une approche dans laquelle un système est
modélisé par des objets réunissant un état et un comportement. Une classe décrit
un type, tandis qu’un objet en est une instance.

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}

val user = User(id = "1", name = "Alex")
```

Ses principaux principes sont l’encapsulation, l’abstraction, l’héritage et le
polymorphisme.

### Encapsulation

Une classe masque son état mutable et le modifie au moyen de méthodes contrôlées :

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

La classe protège ainsi ses invariants.

### Abstraction

Le code dépend d’un contrat plutôt que d’une implémentation :

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class ProfileViewModel(
    private val repository: UserRepository
) : ViewModel()
```

La `ViewModel` ignore si les données proviennent de Retrofit, Room ou d’un fake.

### Héritage

En Kotlin, les classes sont `final` par défaut. L’héritage doit être autorisé
avec `open` :

```kotlin
open class BaseProcessor {
    open fun process(value: String): String = value.trim()
}

class UppercaseProcessor : BaseProcessor() {
    override fun process(value: String): String =
        super.process(value).uppercase()
}
```

L’héritage modélise une relation « est un », mais crée souvent un couplage fort.
La composition est généralement plus sûre.

### Polymorphisme

Différentes implémentations peuvent être fournies à travers un contrat commun :

```kotlin
interface ImageLoader {
    suspend fun load(url: String): Image
}

class CoilImageLoader : ImageLoader
class FakeImageLoader : ImageLoader
```

L’appelant travaille avec `ImageLoader` ; les implémentations de production et
de test sont donc interchangeables.

En Kotlin, la programmation orientée objet s’associe bien aux `data class`, aux
`sealed class` ou `sealed interface`, aux fonctions d’extension, à l’immuabilité
et à l’injection par constructeur. L’essentiel n’est pas le nombre de classes et
d’interfaces, mais la clarté des responsabilités et des frontières.

**En bref :** la programmation orientée objet organise l’état et le comportement
dans des objets. En Kotlin, les éléments essentiels sont l’encapsulation, les
contrats, le polymorphisme et un usage prudent de l’héritage ; les modèles
immuables et la composition sont souvent préférables aux hiérarchies profondes.

</details>

<details>
<summary>127. Qu’est-ce que l’abstraction ?</summary>

#### Kotlin

L’abstraction consiste à masquer les détails d’implémentation derrière un contrat
compréhensible. Elle décrit ce que fait le code, et non comment il le fait.

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Le code qui utilise `UserRepository` ignore si les données proviennent d’une API,
de Room, d’un cache ou d’un fake utilisé dans un test.

Le cas d’utilisation dépend du contrat :

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

L’implémentation masque les détails :

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

Un fake peut être fourni pour les tests :

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: UserId): User = User(id, "Test")
}
```

Architecture Android courante :

```text
ViewModel -> Cas d’utilisation -> Contrat du repository -> Implémentation -> API/DB
```

L’interface utilisateur ne doit pas connaître Retrofit, Room ni la stratégie de
cache.

Une abstraction n’est pas nécessairement une interface. Il peut également
s’agir d’une classe abstraite, d’une hiérarchie `sealed`, d’un type de fonction
ou simplement de l’API publique d’une classe. Les classes abstraites ou de base
créent toutefois souvent une hiérarchie rigide sur Android ; la composition est
généralement plus sûre.

Abstraction et encapsulation :

```text
Abstraction   -> expose le contrat important
Encapsulation -> masque l’état et les détails internes
```

Une mauvaise abstraction est une interface créée « au cas où », sans autre
implémentation réelle ni véritable frontière :

```kotlin
interface UserNameFormatter {
    fun format(name: String): String
}
```

S’il n’existe qu’une seule implémentation simple et aucun besoin de substitution
ou de frontière pour les tests, l’interface peut être inutile.

Une bonne abstraction :

- possède un contrat clair ;
- masque les détails instables ;
- ne laisse pas transparaître les détails d’implémentation ;
- facilite les tests ;
- correspond à un véritable point de changement ;
- n’ajoute pas de code répétitif sans bénéfice.

**En bref :** l’abstraction sépare le contrat de l’implémentation. Sur Android,
elle est utile aux frontières telles que le réseau, la base de données,
l’analyse, le temps, les dispatchers et les repositories. Chaque classe ne doit
pas être abstraite sans raison réelle.

</details>

<details>
<summary>128. Quelle est la différence entre les constructeurs et les méthodes ?</summary>

#### Kotlin

Un constructeur crée une instance et place l’objet dans un état initial valide.
Une méthode décrit le comportement d’un objet déjà créé et peut être appelée
plusieurs fois.

Constructeur principal :

```kotlin
class User(
    val id: String,
    val name: String
)
```

Les paramètres déclarés avec `val` ou `var` deviennent des propriétés. Un
constructeur ne possède pas de type de retour et est appelé avec `User(...)`.

Méthode :

```kotlin
class User(
    val id: String,
    val name: String
) {
    fun displayName(): String = name.trim()
}
```

Une méthode est appelée sur une instance :

```kotlin
user.displayName()
```

Pour la validation ou la logique d’initialisation, on utilise un bloc `init` :

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

Un constructeur secondaire délègue au constructeur principal avec `this(...)` :

```kotlin
class User(
    val id: String,
    val name: String
) {
    constructor(id: String) : this(id, "Unknown")
}
```

En Kotlin, un paramètre par défaut est souvent préférable :

```kotlin
class User(
    val id: String,
    val name: String = "Unknown"
)
```

Le constructeur est fréquemment utilisé pour l’injection de dépendances :

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

Le constructeur définit les dépendances requises, tandis que la méthode exécute
l’opération.

Éléments à ne pas exécuter dans un constructeur ou un bloc `init` :

- une requête réseau ;
- une requête de base de données ;
- le lancement d’une coroutine ;
- `runBlocking` ;
- de lourdes entrées-sorties ;
- une logique dépendant du cycle de vie Android.

Incorrect :

```kotlin
class UserRepository(private val api: UserApi) {
    init {
        runBlocking { api.preloadUsers() }
    }
}
```

Préférable :

```kotlin
class UserRepository(private val api: UserApi) {
    suspend fun preloadUsers() {
        api.preloadUsers()
    }
}
```

**En bref :** un constructeur crée un objet valide et reçoit son état ou ses
dépendances indispensables. Une méthode exécute un comportement après la
construction. Les opérations lourdes, `suspend` ou liées au cycle de vie ne
doivent pas être lancées dans un constructeur.

</details>

<details>
<summary>129. Qu’est-ce que la signature d’une méthode ?</summary>

#### Kotlin

La signature d’une méthode regroupe les caractéristiques qui permettent au
compilateur de distinguer une fonction d’une autre. En Kotlin, il s’agit
principalement du nom, du receveur, ainsi que des types et de l’ordre des
paramètres.

```kotlin
fun loadUser(id: String): User
```

Signature simplifiée : `loadUser(String)`.

### Surcharge

Les surcharges doivent posséder des listes de paramètres différentes :

```kotlin
fun search(query: String)
fun search(query: String, limit: Int)
fun search(userId: Long)
```

Le type de retour ne suffit pas :

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // conflit
```

Le nom d’un paramètre ne suffit pas non plus :

```kotlin
fun load(id: String)
fun load(userId: String) // conflit
```

Les arguments nommés améliorent la lisibilité du site d’appel, mais ne créent
pas une nouvelle signature.

L’ordre des types est important :

```kotlin
fun create(name: String, age: Int)
fun create(age: Int, name: String)
```

Ce code est valide, mais constitue souvent une mauvaise API en raison de sa
faible lisibilité.

### Paramètres par défaut

```kotlin
fun loadUser(
    id: String,
    forceRefresh: Boolean = false
)
```

Il s’agit d’une seule fonction Kotlin. `@JvmOverloads` peut générer des
surcharges pour Java, mais il faut éviter d’ajouter sans précaution de nombreuses
surcharges manuelles similaires.

### Redéfinition

Une redéfinition doit respecter le contrat du type de base :

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}

class RealUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = TODO()
}
```

Modifier le type d’un paramètre crée une autre fonction, et non une redéfinition.
Le type de retour peut être un sous-type covariant.

### Receveurs et extensions

```kotlin
fun UserDto.toDomain(): User
fun AdminDto.toDomain(): Admin
```

Le receveur d’une extension participe à la résolution Kotlin. Une fonction
d’extension ne constitue toutefois pas une redéfinition virtuelle.

### Particularités de la JVM

L’effacement des types peut provoquer des conflits :

```kotlin
fun process(items: List<String>)
fun process(items: List<Int>) // conflit JVM : process(List)
```

La nullabilité ne crée pas non plus de signature JVM distincte :

```kotlin
fun load(id: String)
fun load(id: String?) // conflit JVM
```

La solution consiste à choisir un autre nom ou à utiliser `@JvmName` lorsque cela
est réellement nécessaire.

**En bref :** en Kotlin, les fonctions se distinguent par leur nom, leur receveur,
ainsi que les types et l’ordre de leurs paramètres. Le type de retour et le nom
des paramètres ne suffisent pas. Sur la JVM, il faut également tenir compte de
l’effacement des types, de la nullabilité et des particularités
d’interopérabilité.

</details>

<details>
<summary>130. Quelle est la différence entre la surcharge et la redéfinition ?</summary>

#### Kotlin

```text
Surcharge     -> même nom, listes de paramètres différentes
Redéfinition  -> même signature, nouvelle implémentation dans un sous-type
```

### Surcharge

Plusieurs fonctions d’une même portée possèdent le même nom, mais des paramètres
différents :

```kotlin
class UserRepository {
    suspend fun getUser(id: String): User = TODO()
    suspend fun getUser(id: Long): User = TODO()
    suspend fun getUser(email: String, includeDetails: Boolean): User = TODO()
}
```

Le compilateur choisit la surcharge selon les types statiques des arguments. Le
type de retour ne peut pas, à lui seul, distinguer une surcharge :

```kotlin
fun value(): String = "text"
fun value(): Int = 42 // erreur de compilation
```

En Kotlin, les paramètres par défaut et les arguments nommés remplacent
avantageusement certaines surcharges :

```kotlin
fun loadUsers(
    forceRefresh: Boolean = false,
    limit: Int = 50
)

loadUsers()
loadUsers(forceRefresh = true)
loadUsers(limit = 100)
```

### Redéfinition

Un sous-type implémente ou modifie une méthode héritée possédant la même
signature :

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

Modifier le type d’un paramètre crée une autre fonction, et non une redéfinition.

En Kotlin, les classes et les méthodes sont `final` par défaut. Pour autoriser
une redéfinition, le membre doit être `open`, `abstract` ou appartenir à une
interface :

```kotlin
open class BaseTracker {
    open fun track(event: Event) = Unit
}

class FirebaseTracker : BaseTracker() {
    override fun track(event: Event) {
        // implémentation
    }
}
```

### Résolution

La surcharge utilise une résolution statique à la compilation :

```kotlin
fun print(value: Any) = println("Any")
fun print(value: String) = println("String")

val value: Any = "hello"
print(value) // Any
```

La redéfinition utilise une résolution virtuelle à l’exécution :

```kotlin
val logger: Logger = CrashlyticsLogger()
logger.log("Error") // implémentation de CrashlyticsLogger
```

Points importants :

- les surcharges combinées à des arguments par défaut peuvent être ambiguës ;
- une redéfinition doit respecter le contrat du type de base ;
- son type de retour peut être covariant ;
- les fonctions d’extension ne sont pas redéfinies, mais résolues statiquement ;
- `final override` interdit toute redéfinition ultérieure.

**En bref :** la surcharge définit plusieurs fonctions de même nom avec des
paramètres différents, choisies à la compilation. La redéfinition remplace une
implémentation héritée de même signature, choisie à l’exécution grâce au
polymorphisme.

</details>

<details>
<summary>131. Quelle est la différence entre une interface et une classe abstraite ?</summary>

#### Kotlin

Une interface décrit un contrat ou un rôle. Une classe abstraite définit une base
commune avec une implémentation partielle, un constructeur et un état.

```text
interface       -> contrat, plusieurs rôles
classe abstraite -> implémentation de base, une seule classe parente
```

### Interface

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User
}
```

Une classe peut implémenter plusieurs interfaces :

```kotlin
class UserRepositoryImpl : UserRepository, CacheCleaner
```

Une interface convient bien à l’injection de dépendances, aux tests et à
l’inversion des dépendances :

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

En Kotlin, une interface peut posséder une méthode par défaut :

```kotlin
interface Logger {
    fun log(message: String) {
        println(message)
    }
}
```

Elle ne possède toutefois pas de constructeur et ne convient pas à un état
mutable partagé.

### Classe abstraite

```kotlin
abstract class BaseRepository {
    protected fun logError(error: Throwable) {
        // logique partagée
    }

    abstract suspend fun refresh()
}
```

Une classe abstraite peut posséder :

- un constructeur ;
- un état ;
- des méthodes protégées ;
- une implémentation partielle ;
- le pattern Template Method.

Une classe ne peut cependant hériter que d’une seule classe :

```kotlin
class MyRepository : BaseRepository()
```

### Choix selon le besoin

Utilisez une interface lorsque :

- un contrat est nécessaire ;
- plusieurs implémentations existent ;
- un fake ou un mock est requis dans les tests ;
- une classe possède plusieurs rôles ;
- une dépendance doit être abstraite.

Utilisez une classe abstraite lorsque :

- une implémentation partagée est nécessaire ;
- un état protégé est requis ;
- il existe un cycle de vie ou un template commun ;
- le constructeur fait partie du contrat de base.

Sur Android, il faut utiliser avec prudence `BaseActivity`, `BaseFragment` et
`BaseViewModel` : ces classes deviennent rapidement des classes de base
omnipotentes et créent une hiérarchie rigide.

La composition est souvent préférable :

```kotlin
class UserRepository(
    private val logger: Logger
)
```

**En bref :** une interface convient aux contrats, à l’injection, aux tests et
aux rôles multiples. Une classe abstraite fournit une implémentation et un état
communs, mais risque de créer un héritage rigide. Dans le développement Android
moderne, une interface associée à la composition est souvent préférable à de
profondes hiérarchies de classes de base.

</details>

<details>
<summary>132. Une interface peut-elle ne contenir aucune méthode ?</summary>

#### Kotlin

Oui. Une interface vide est appelée interface marqueur. Elle ne définit aucun
comportement, mais attribue un rôle particulier à un type dans le système de
types.

```kotlin
interface Cacheable

data class UserProfile(
    val id: String,
    val name: String
) : Cacheable
```

`UserProfile` peut désormais être accepté comme `Cacheable`, vérifié avec
`is Cacheable` ou utilisé comme borne générique.

```kotlin
interface Syncable

fun <T : Syncable> sync(item: T) {
    // uniquement les types explicitement marqués
}
```

Si `sync` nécessite un identifiant ou un comportement, le marqueur ne suffit
plus : une propriété ou une méthode doit être ajoutée au contrat.

### Marqueur sealed

Une `sealed interface` vide sert souvent de racine à un ensemble fermé d’états,
d’événements ou d’effets :

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

Il ne s’agit pas simplement d’un marqueur, mais d’un type somme : une valeur
appartient à l’une des variantes définies, et le compilateur vérifie
l’exhaustivité du `when`.

`java.io.Serializable` est un exemple classique d’interface marqueur : elle ne
possède aucune méthode, mais le runtime vérifie si un objet implémente ce type.

### Interface ou annotation

Une interface marqueur convient lorsqu’il faut :

- une relation de sous-typage ;
- des bornes génériques ;
- une API polymorphe ;
- une hiérarchie `sealed` ;
- une vérification avec `is` sans réflexion.

Une annotation convient mieux aux métadonnées :

```kotlin
@Target(AnnotationTarget.CLASS)
@Retention(AnnotationRetention.RUNTIME)
annotation class Cacheable
```

Elle est traitée par un plugin de compilation, un générateur de code ou la
réflexion à l’exécution, mais ne crée aucune relation de sous-typage.

### Mauvais usage d’un marqueur

Il ne faut pas créer de marqueurs « pour plus tard » :

```kotlin
interface Important
interface Special
```

Signes d’une mauvaise conception :

- le marqueur n’est jamais utilisé comme type, borne ou racine ;
- la logique contient de nombreuses vérifications `is Marker` ;
- des données ou des méthodes sont nécessaires, mais le code utilise des casts ;
- le rôle serait mieux exprimé par une enum ou une propriété ;
- les métadonnées seraient mieux représentées par une annotation.

**En bref :** une interface peut être vide. Une interface marqueur est utile
pour une relation de sous-typage, des bornes génériques ou des hiérarchies
`sealed`. Utilisez une annotation pour les métadonnées ; si un comportement est
nécessaire, l’interface doit contenir des membres.

</details>

<details>
<summary>133. Une interface peut-elle contenir une autre interface ?</summary>

#### Kotlin

Oui. En Kotlin, une interface peut contenir une autre interface. Il s’agit d’une
interface imbriquée.

```kotlin
interface UserRepository {
    suspend fun getUser(id: String): User

    interface Listener {
        fun onUserChanged(user: User)
    }
}
```

Utilisation :

```kotlin
class UserChangeListener : UserRepository.Listener {
    override fun onUserChanged(user: User) {
        // traiter la modification
    }
}
```

Une interface imbriquée est référencée avec `Outer.Inner`, par exemple
`UserRepository.Listener`.

Cette approche est pertinente lorsque :

- le contrat est étroitement lié à l’API extérieure ;
- le type imbriqué n’est utilisé que dans ce contexte ;
- il représente un callback ou un petit contrat local ;
- un type de premier niveau encombrerait inutilement le package.

Exemple d’API à callback :

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

Une interface imbriquée n’est pas `inner` et n’a pas accès à l’état de l’instance
du type extérieur :

```kotlin
interface Outer {
    val id: String

    interface Inner {
        fun execute()
    }
}
```

`Inner` ne peut pas lire directement `id`.

Il ne faut pas confondre cette notion avec l’héritage d’interfaces :

```kotlin
interface ReadableRepository {
    suspend fun get(id: String): User
}

interface WritableRepository {
    suspend fun save(user: User)
}

interface UserRepository : ReadableRepository, WritableRepository
```

Il s’agit ici d’un héritage de contrats, et non d’une imbrication.

Une interface ne doit pas être imbriquée si elle est autonome et utilisée dans
tout le projet :

```kotlin
interface AppContract {
    interface UserRepository
    interface PaymentRepository
    interface Analytics
}
```

Dans ce cas, l’interface extérieure devient un espace de noms artificiel. Il est
préférable de créer des contrats de premier niveau.

**En bref :** une interface peut contenir une interface imbriquée. Cette
organisation convient aux callbacks et aux contrats qui n’existent que dans le
contexte d’une API extérieure. Les abstractions autonomes doivent plutôt être
déclarées au premier niveau.

</details>

<details>
<summary>134. Pourquoi ne faut-il pas ajouter des dizaines de méthodes à une interface ?</summary>

#### Kotlin

Une interface volumineuse enfreint généralement le principe de ségrégation des
interfaces : les clients dépendent de méthodes dont ils n’ont pas besoin. Cela
augmente le couplage et complique les implémentations ainsi que les tests.

Incorrect :

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

Même si un cas d’utilisation nécessite uniquement `getUser`, il dépend malgré
tout de l’ensemble du contrat.

Problème d’implémentation :

```kotlin
class ReadOnlyUserRepository : UserRepository {
    override suspend fun updateUser(user: User) {
        throw UnsupportedOperationException()
    }
}
```

Si une implémentation ne peut pas honnêtement fournir une méthode, le contrat est
incorrect ou trop large.

Problème dans les tests :

```kotlin
class FakeUserRepository : UserRepository {
    override suspend fun getUser(id: String): User = user
    override suspend fun updateUser(user: User) = Unit
    override suspend fun deleteUser(id: String) = Unit
    // bruit inutile
}
```

Les fakes et les mocks sont contraints d’implémenter des méthodes inutiles.

Il est préférable de séparer les rôles :

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

Le cas d’utilisation ne dépend alors que du contrat nécessaire :

```kotlin
class GetUserUseCase(
    private val userReader: UserReader
) {
    suspend operator fun invoke(id: String): User = userReader.getUser(id)
}
```

Une grande interface reste acceptable lorsqu’elle représente réellement une
abstraction cohérente. Par exemple, le DAO d’une même table peut contenir
plusieurs requêtes liées. Si les méthodes possèdent des clients différents et
des raisons distinctes de changer, il est préférable de les séparer.

**En bref :** une interface doit décrire un rôle, et non tout ce que le système
sait faire. Si un client n’utilise qu’une ou deux méthodes sur vingt, le contrat
est trop large. Il vaut mieux séparer les rôles réels, par exemple lecteur,
rédacteur ou téléverseur.

</details>

<details>
<summary>135. Qu’est-ce que la composition et en quoi diffère-t-elle de l’héritage ?</summary>

#### Kotlin

Avec la composition, une classe obtient un comportement au moyen de ses
dépendances. Avec l’héritage, elle reçoit un comportement d’une classe de base.

```text
Héritage    -> est un
Composition -> possède un / utilise un
```

### Héritage

Il convient lorsqu’une sous-classe est réellement une spécialisation du type de
base :

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

Un `Dog` est un `Animal` ; la relation « est un » est donc logique.

### Composition

Une classe utilise d’autres objets plutôt que l’héritage :

```kotlin
class Dog(
    private val soundPlayer: SoundPlayer
) {
    fun makeSound() {
        soundPlayer.play("Bark")
    }
}
```

Un `Dog` possède un `SoundPlayer`, mais n’est pas un `SoundPlayer`.

Sur Android et en Kotlin, la composition prend souvent la forme suivante :

```kotlin
class LoginViewModel(
    private val login: LoginUseCase,
    private val errorMapper: ErrorMapper,
    private val analytics: AnalyticsTracker
) : ViewModel()
```

Les dépendances sont explicites et peuvent facilement être remplacées par des
fakes ou des mocks dans les tests.

### Problème des classes de base

De grandes classes `BaseViewModel`, `BaseRepository` ou `BaseFragment` deviennent
souvent des classes de base omnipotentes :

```kotlin
open class BaseViewModel : ViewModel() {
    fun showError(error: Throwable) {}
    fun trackScreen(name: String) {}
    fun logout() {}
}
```

Les classes enfants reçoivent alors des méthodes et un état dont elles n’ont pas
besoin. Une modification de la classe de base peut casser de nombreux écrans
sans rapport entre eux.

### Délégation Kotlin

Kotlin fournit la délégation comme forme pratique de composition :

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

`UserService` délègue l’implémentation de `Logger` à un autre objet sans en
hériter.

Choix selon le besoin :

- héritage — véritable relation « est un » et contrat de base stable ;
- composition — comportement devant être substitué, testé, combiné ou modifié ;
- code d’application Android — la composition est généralement préférable à un
  héritage profond.

**En bref :** l’héritage modélise une relation « est un », tandis que la
composition modélise « possède un » ou « utilise un ». En Kotlin et sur Android,
la composition est généralement préférable : dépendances explicites, tests plus
simples, couplage réduit et absence de hiérarchies fragiles de classes de base.

</details>

<details>
<summary>136. Quels sont les modificateurs d’accès en Kotlin ?</summary>

#### Kotlin

Kotlin possède quatre modificateurs de visibilité :

```text
public    -> accessible partout
internal  -> accessible dans le module
protected -> accessible dans la classe et ses sous-classes
private   -> accessible dans la portée ou le fichier courant
```

### `public`

`public` est la visibilité par défaut :

```kotlin
class UserRepository {
    fun getUser(id: String): User = TODO()
}
```

Tout élément public fait partie de l’API ; les détails d’implémentation ne doivent
donc pas être exposés sans nécessité.

### `private`

Un membre privé n’est accessible qu’à l’intérieur de sa classe ou de son objet :

```kotlin
class TokenStorage {
    private var token: String? = null

    fun save(value: String) {
        token = value
    }
}
```

Au premier niveau, `private` limite l’accès à un seul fichier Kotlin :

```kotlin
private const val DEFAULT_TIMEOUT = 30_000L
```

### `protected`

Un membre protégé est accessible dans sa classe et ses sous-classes :

```kotlin
open class BaseViewModel : ViewModel() {
    protected fun handleError(error: Throwable) = Unit
}
```

Contrairement à Java, `protected` ne donne pas d’accès au package en Kotlin et
ne peut pas être utilisé au premier niveau. Il fait partie du contrat d’héritage.

### `internal`

Un élément interne est accessible dans son module Kotlin :

```kotlin
internal class RealUserRepository(
    private val api: UserApi
) : UserRepository
```

Un module est une frontière de compilation, par exemple un module Gradle, et non
un package.

`internal` est utile dans une application Android multimodule : seule l’API du
module reste publique, tandis que l’implémentation est interne.

Point important : `internal` est une frontière de compilation, et non une mesure
de sécurité. Le bytecode Java et la réflexion peuvent exposer davantage
d’éléments.

### Visibilité des setters et constructeurs

Une propriété peut être publique en lecture et privée en écriture :

```kotlin
class SessionManager {
    var isLoggedIn: Boolean = false
        private set
}
```

Un constructeur peut également posséder une visibilité :

```kotlin
class PaymentClient internal constructor(
    private val api: PaymentApi
)
```

Règles pratiques :

- commencer par la visibilité suffisante la plus étroite ;
- conserver l’implémentation en `private` ou `internal` ;
- réduire au minimum l’API publique ;
- réserver `protected` à un véritable contrat d’héritage ;
- ne pas considérer la visibilité comme une frontière de sécurité.

**En bref :** `public` signifie accessible partout, `internal` dans le module,
`protected` dans la hiérarchie d’héritage et `private` dans la classe ou le
fichier. Une bonne API Kotlin n’expose que le strict nécessaire.

</details>

<details>
<summary>137. Qu’est-ce qu’un singleton ?</summary>

#### Kotlin

Un singleton est une instance unique dans la portée d’un propriétaire donné :
classloader, processus ou composant d’injection de dépendances. Sur Android, il
faut se rappeler que le processus peut être tué et qu’une application
multiprocessus possède un singleton distinct dans chaque processus.

### `object` Kotlin

```kotlin
object AppLogger {
    fun log(message: String) = println(message)
}
```

Un `object` est initialisé paresseusement et de manière thread-safe lors de sa
première utilisation. Son état mutable ne devient toutefois pas automatiquement
thread-safe :

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

Un companion est un singleton lié à une classe, mais cette classe peut posséder
plusieurs instances. Il sert généralement aux méthodes de factory et aux
constantes.

### Singleton avec l’injection de dépendances

```kotlin
@Singleton
class AnalyticsTracker @Inject constructor(
    @ApplicationContext private val context: Context
)
```

Dans Hilt, `@Singleton` signifie une instance dans le `SingletonComponent`, et
non un objet éternel. L’injection est préférable à un singleton global lorsqu’un
objet possède des dépendances :

- les dépendances du constructeur sont explicites ;
- l’implémentation peut être remplacée par un fake ;
- la durée de vie est contrôlée par le composant ;
- l’appelant dépend d’un contrat.

Des scopes comme `@ViewModelScoped` ou `@ActivityScoped` conviennent aux durées de
vie plus courtes.

### Risques

Incorrect :

```kotlin
object SessionHolder {
    var activity: Activity? = null
    var token: String? = null
}
```

Problèmes :

- fuite d’Activity ;
- état mutable global ;
- conditions de concurrence ;
- tests difficiles en raison de l’état partagé ;
- déconnexion et réinitialisation mal définies ;
- perte de l’état après la mort du processus.

Un singleton au niveau de l’application ne doit pas conserver une `Activity`, un
`Fragment`, une `View` ou un `ActivityContext`. Si un contexte est nécessaire,
seul le contexte de l’application doit être utilisé.

Une session ou un jeton ne doit pas être conservé uniquement dans un singleton
en mémoire : il faut prévoir une persistance sûre, une restauration et une
suppression explicite.

Un singleton convient notamment :

- à une base de données Room ;
- à un client OkHttp ;
- à un logger ou un service d’analyse ;
- à des services sans état utilisés dans toute l’application ;
- à une configuration immuable.

Il ne convient pas à l’état d’un écran, à une ViewModel, à une session utilisateur
sans réinitialisation ni à une dépendance de courte durée.

**En bref :** en Kotlin, un singleton est souvent créé avec `object`, tandis que
sur Android, il est préférable de gérer les dépendances avec des scopes
d’injection. Un singleton convient aux services de toute l’application, mais
l’état mutable global et les références à l’interface sont presque toujours
problématiques.

</details>

<details>
<summary>138. Que sont les génériques ?</summary>

#### Kotlin

Les génériques permettent de paramétrer une classe, une interface ou une fonction
avec un type tout en conservant la sécurité de typage à la compilation.

```kotlin
class Box<T>(val value: T)

val text: Box<String> = Box("Hello")
val number: Box<Int> = Box(42)
```

`T` est un paramètre de type, tandis que `String` et `Int` sont des arguments de
type. Le compilateur connaît le type de `value` ; aucun cast dangereux n’est donc
nécessaire.

Fonction générique :

```kotlin
fun <T> singleItemList(item: T): List<T> = listOf(item)
```

### Bornes

Un paramètre de type peut être limité :

```kotlin
interface Identifiable {
    val id: String
}

fun <T : Identifiable> findById(
    items: List<T>,
    id: String
): T? = items.firstOrNull { it.id == id }
```

Plusieurs bornes :

```kotlin
fun <T> sync(item: T)
    where T : Identifiable,
          T : Syncable = Unit
```

### Résultat générique

```kotlin
sealed interface Result<out T> {
    data class Success<T>(val data: T) : Result<T>
    data class Error(val cause: Throwable) : Result<Nothing>
    data object Loading : Result<Nothing>
}
```

`Nothing` est un sous-type de tous les types Kotlin ; `Error` et `Loading`
peuvent donc être utilisés comme `Result<User>`, `Result<List<Post>>`, etc.

### Variance

Par défaut, un type générique est invariant. Même si `Cat` hérite d’`Animal`,
`MutableList<Cat>` n’est pas une `MutableList<Animal>`.

`out T` représente un producteur et la covariance :

```kotlin
interface Producer<out T> {
    fun produce(): T
}

val cats: Producer<Cat> = TODO()
val animals: Producer<Animal> = cats
```

`in T` représente un consommateur et la contravariance :

```kotlin
interface Consumer<in T> {
    fun consume(value: T)
}

val animalConsumer: Consumer<Animal> = TODO()
val catConsumer: Consumer<Cat> = animalConsumer
```

Règle mnémotechnique : un producteur utilise `out`, un consommateur utilise `in`.
Lorsqu’un type reçoit et renvoie `T`, il est généralement invariant.

### Effacement des types et `reified`

Sur la JVM, les arguments de types génériques sont généralement effacés à
l’exécution :

```kotlin
value is List<*> // possible
// value is List<String> // impossible directement
```

`inline reified` donne accès au paramètre de type à l’exécution :

```kotlin
inline fun <reified T> Json.decode(raw: String): T =
    decodeFromString<T>(raw)

val user: User = json.decode(raw)
```

`reified` n’est disponible que pour les fonctions `inline`.

Règles pratiques :

- les bornes définissent le contrat minimal ;
- `out` et `in` décrivent les relations de sous-typage sûres ;
- les conteneurs mutables sont généralement invariants ;
- un type générique requis à l’exécution nécessite `reified` ou un token de type
  explicite ;
- ne pas complexifier une API avec des génériques sans avantage réel.

**En bref :** les génériques permettent de créer des API réutilisables et typées.
Les bornes limitent les types autorisés, la variance (`out` et `in`) contrôle la
compatibilité des sous-types, et `reified` contourne partiellement l’effacement
des types de la JVM dans les fonctions `inline`.

</details>

<details>
<summary>139. Qu’est-ce que l’effacement des types et pourquoi existe-t-il ?</summary>

#### Kotlin

L’effacement des types signifie que, sur la JVM, les arguments génériques
concrets ne sont généralement pas conservés dans l’objet à l’exécution.
`List<String>` et `List<Int>` deviennent simplement `List`.

Ce mécanisme existe pour assurer la compatibilité descendante des génériques Java
avec l’ancien bytecode. Un paramètre de type est effacé jusqu’à sa borne
supérieure ou jusqu’à `Object`. Kotlin/JVM utilise le même modèle JVM.

La vérification suivante est impossible :

```kotlin
if (value is List<String>) { } // type effacé
```

Le conteneur peut être vérifié :

```kotlin
if (value is List<*>) {
    // type des éléments inconnu
}
```

`List<*>` permet de lire les éléments en toute sécurité comme `Any?`, mais ne
garantit pas leur type concret.

Un cast non vérifié ne valide pas tous les éléments :

```kotlin
val strings = value as List<String> // non vérifié
```

Pour une véritable validation, il faut vérifier chaque élément :

```kotlin
val strings = (value as? List<*>)
    ?.map { element ->
        element as? String ?: error("Expected String")
    }
```

### `reified`

Un `T` ordinaire ne peut pas être utilisé avec `is T`. Une fonction `inline` peut
toutefois posséder un paramètre de type `reified` :

```kotlin
inline fun <reified T> isType(value: Any): Boolean =
    value is T
```

`T::class`, `is T` et les API utilisant un token de classe deviennent alors
disponibles. `reified` ne restaure cependant pas entièrement les génériques
imbriqués : `List<String>` exige toujours une vérification des éléments, un
sérialiseur ou un token de type.

Sans `reified`, le type est transmis explicitement :

```kotlin
fun <T> decode(json: String, clazz: Class<T>): T = TODO()

val user = decode(json, User::class.java)
```

Pour `List<User>`, un simple `Class<List>` ne suffit pas. Les bibliothèques
utilisent donc `Type`, `KType`, un adapter généré ou `KSerializer<T>`.

L’effacement des types provoque également des conflits de signatures JVM :

```kotlin
fun handle(items: List<String>) {}
fun handle(items: List<Int>) {} // conflit : handle(List)
```

Il faut choisir des noms différents ou utiliser prudemment `@JvmName`.

**En bref :** l’effacement des types supprime les arguments génériques à
l’exécution pour assurer la compatibilité JVM et Java. Les vérifications à
l’exécution utilisent `List<*>`, `inline reified`, un token de type explicite ou
un sérialiseur ; un cast non vérifié ne constitue pas une validation complète
des types.

</details>

<details>
<summary>140. Quelles collections existe-t-il : `List`, `Set`, `Map`, Queue et Stack ?</summary>

#### Kotlin

Le choix d’une collection dépend de sa sémantique : ordre, unicité, recherche par
clé, FIFO ou LIFO.

### `List`

Collection ordonnée permettant l’accès par indice et les doublons :

```kotlin
val names: List<String> = listOf("Alex", "Kate", "Alex")
val first = names[0]
```

Sur la JVM, l’implémentation mutable courante est `ArrayList` : accès par indice
en `O(1)`, recherche en `O(n)`, insertion ou suppression au milieu en `O(n)`.

### `Set`

Collection d’éléments uniques :

```kotlin
val selectedIds: Set<String> = setOf("1", "2")
if ("1" in selectedIds) { /* existe */ }
```

`HashSet` offre généralement une complexité moyenne de `O(1)` pour
`contains`, `add` et `remove`, mais nécessite des implémentations correctes de
`equals()` et `hashCode()`.

### `Map`

Structure clé-valeur dont les clés sont uniques :

```kotlin
val usersById: Map<String, User> = users.associateBy(User::id)
val user = usersById["42"]
```

`HashMap.get()` possède une complexité moyenne de `O(1)`. Une Map convient à un
cache, un index ou une table de recherche. Une clé mutable est dangereuse si les
champs qui influencent `hashCode()` sont modifiés.

### Queue et Stack

Une file utilise l’ordre FIFO :

```text
premier entré -> premier sorti
```

Une pile utilise l’ordre LIFO :

```text
dernier entré -> premier sorti
```

En Kotlin, `ArrayDeque` est une solution pratique :

```kotlin
val deque = ArrayDeque<String>()

// file
deque.addLast("A")
deque.addLast("B")
val first = deque.removeFirst() // A

// pile
deque.addLast("C")
val last = deque.removeLast() // C
```

Les opérations aux extrémités ont une complexité amortie de `O(1)`.
`ArrayDeque` est préférable à l’ancienne classe Java `Stack`.

### Lecture seule et mutable

Kotlin sépare les API :

```kotlin
val users: List<User> = listOf()
val mutableUsers: MutableList<User> = mutableListOf()
```

`List` ne possède pas de méthodes `add` ou `remove`, mais cela ne garantit pas
une immuabilité profonde : l’objet sous-jacent ou ses éléments peuvent être
mutables. Un état mutable partagé doit être encapsulé.

Choix pratique :

- ordre, indice et doublons — `List` ;
- unicité et vérification d’appartenance — `Set` ;
- recherche par clé — `Map` ;
- FIFO — `ArrayDeque.removeFirst()` ;
- LIFO — `ArrayDeque.removeLast()` ;
- ordre de priorité — `PriorityQueue`.

**En bref :** `List` conserve l’ordre et les doublons, `Set` garantit l’unicité,
`Map` permet la recherche par clé, une Queue suit l’ordre FIFO et une Stack
l’ordre LIFO. En Kotlin, `ArrayDeque` suffit souvent pour implémenter une file ou
une pile.

</details>

<details>
<summary>141. Qu’est-ce que la sûreté des collections dans un environnement multithread ?</summary>

#### Kotlin

Une collection thread-safe conserve un état correct lors d’accès concurrents. Les collections ordinaires `MutableList`, `MutableMap` et `MutableSet` de Kotlin/JVM ne sont pas thread-safe.

```kotlin
val users = mutableListOf<User>()

thread { users += User("1", "Alex") }
thread { users += User("2", "Kate") }
```

Des écritures concurrentes peuvent provoquer une perte de mise à jour, un état interne incorrect ou une exception.

### Lecture seule ≠ immuable

```kotlin
val users: List<User> = mutableListOf()
```

`List` interdit les modifications uniquement par l’intermédiaire de cette référence. La collection sous-jacente ou ses éléments peuvent rester modifiables.

### Principales stratégies

1. **Un seul propriétaire/thread**

Toutes les lectures et écritures passent par un seul propriétaire : le thread principal, un dispatcher unique ou un composant de type acteur. Il s’agit du modèle de propriété le plus simple.

2. **`Mutex` pour les coroutines**

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

Tous les accès doivent passer par le même `Mutex`. Il faut éviter d’effectuer de longues opérations réseau ou suspendues à l’intérieur du verrou.

3. **Collections concurrentes**

```kotlin
private val users = ConcurrentHashMap<String, User>()
```

`ConcurrentHashMap` convient aux accès concurrents par clé. Toutefois, une opération composée ne devient pas automatiquement atomique : il faut utiliser des méthodes atomiques telles que `computeIfAbsent` ou une synchronisation distincte.

`CopyOnWriteArrayList` convient aux listeners lorsque les lectures sont nombreuses et les écritures rares.

4. **Instantanés immuables**

Pour l’état de l’interface utilisateur, il est préférable de créer une nouvelle collection :

```kotlin
data class UsersState(val users: List<User>)

_state.update { state ->
    state.copy(users = state.users + newUser)
}
```

Ici, l’atomicité est assurée par `_state.update`, et non par la `List` elle-même.

### Itération

Une modification pendant une itération peut provoquer une `ConcurrentModificationException`, même dans un seul thread. S’il faut modifier une collection pendant son parcours, il convient d’utiliser un itérateur, une opération de collection ou un instantané.

Choix pratiques :

- état géré par des coroutines — `Mutex` ou propriétaire sérialisé ;
- accès concurrent par clé — `ConcurrentHashMap` ;
- listeners principalement consultés — `CopyOnWriteArrayList` ;
- état de l’interface utilisateur — instantanés immuables avec `update` atomique ;
- invariants composés — une seule section critique.

**En bref :** les collections modifiables ne sont pas thread-safe. Une API en lecture seule ne garantit pas l’immuabilité. La sûreté exige un modèle de propriété, un `Mutex`, des collections concurrentes ou des instantanés immuables ; il faut synchroniser l’invariant dans son ensemble, et non un appel isolé.

</details>

<details>
<summary>142. Quelle est la différence entre final, finally et finalize ?</summary>

#### Kotlin

Il s’agit de trois notions différentes :

```text
final    -> interdit l’héritage ou la redéfinition
finally  -> bloc de nettoyage après try/catch
finalize -> ancien callback du ramasse-miettes
```

### `final`

En Java, `final` interdit l’héritage d’une classe, la redéfinition d’une méthode ou la réaffectation d’une variable.

En Kotlin, les classes et les méthodes sont `final` par défaut. Pour permettre l’héritage, il faut utiliser `open` :

```kotlin
open class BaseViewModel : ViewModel() {
    open fun load() = Unit
}

class ProfileViewModel : BaseViewModel() {
    final override fun load() {
        // Les sous-classes ne peuvent pas redéfinir cette méthode
    }
}
```

`final override` permet une première redéfinition, puis interdit toute redéfinition ultérieure.

Pour les variables et les propriétés, Kotlin utilise `val` :

```kotlin
val users = mutableListOf<User>()
users += User("1", "Alex")
```

`val` interdit la réaffectation de la référence, mais ne rend pas l’objet immuable.

### `finally`

Le bloc `finally` s’exécute après `try/catch` afin d’effectuer le nettoyage :

```kotlin
val stream = openStream()
try {
    stream.read()
} finally {
    stream.close()
}
```

Il sert notamment à fermer une ressource, libérer un verrou, désinscrire un listener ou réinitialiser un état temporaire.

Dans les coroutines, `finally` s’exécute également lors d’une annulation :

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

Si le nettoyage doit appeler une fonction suspendue dans une coroutine annulée, un court bloc `withContext(NonCancellable)` peut parfois être nécessaire. Cela doit toutefois rester une exception et non le comportement par défaut.

L’exécution de `finally` n’est pas garantie en cas d’arrêt forcé du processus ou de plantage de la JVM.

### `finalize`

`finalize()` est un ancien callback de la classe Java `Object` que le ramasse-miettes pouvait appeler avant de collecter un objet. Il ne faut pas l’utiliser pour le nettoyage, car :

- le moment de son appel est inconnu ;
- il peut ne jamais être appelé avant la fin du processus ;
- il complique le travail du ramasse-miettes ;
- il s’agit d’un mécanisme obsolète et déprécié.

Solutions de remplacement :

```kotlin
FileInputStream(file).use { stream ->
    stream.readBytes()
}
```

Sous Android, le nettoyage est lié au cycle de vie :

```kotlin
override fun onDestroyView() {
    recyclerView.adapter = null
    super.onDestroyView()
}
```

Dans Compose, il s’effectue avec `DisposableEffect` :

```kotlin
DisposableEffect(Unit) {
    val listener = registerListener()
    onDispose { unregisterListener(listener) }
}
```

**En bref :** `final` limite l’héritage et la redéfinition, `finally` effectue un nettoyage déterministe après `try/catch`, tandis que `finalize()` est un mécanisme obsolète et peu fiable du ramasse-miettes qu’il ne faut pas utiliser.

</details>

<details>
<summary>143. Comment fonctionne try-catch-finally ?</summary>

#### Kotlin

`try-catch-finally` sépare l’opération susceptible d’échouer, le traitement de l’exception et le nettoyage.

```kotlin
try {
    riskyOperation()
} catch (exception: IOException) {
    handleNetworkError(exception)
} finally {
    cleanup()
}
```

Ordre d’exécution :

```text
succès -> try -> finally
erreur -> try -> catch correspondant -> finally
```

S’il n’existe aucun `catch` correspondant, l’exception est transmise à l’appelant après l’exécution de `finally`.

### `catch`

Il est possible d’intercepter séparément différents types d’exceptions :

```kotlin
try {
    repository.load()
} catch (exception: HttpException) {
    showServerError(exception.code())
} catch (exception: IOException) {
    showNetworkError()
}
```

Les blocs `catch` sont examinés de haut en bas ; les types les plus spécifiques doivent donc précéder les types plus généraux.

Kotlin ne possède pas d’exceptions vérifiées. Le compilateur n’impose donc pas l’utilisation de `try/catch`. Une exception doit être interceptée à l’endroit où le code peut réellement reprendre l’exécution, transformer l’erreur ou lui ajouter du contexte.

### `try` comme expression

`try` peut renvoyer une valeur :

```kotlin
val user: User? = try {
    repository.getUser(userId)
} catch (exception: IOException) {
    null
}
```

Le bloc `finally` ne détermine pas le résultat de l’expression.

### `finally`

`finally` sert à effectuer le nettoyage :

```kotlin
val connection = openConnection()
try {
    connection.send()
} finally {
    connection.close()
}
```

Il s’exécute en cas de réussite, d’exception, de `return` et d’annulation d’une coroutine. Il ne faut pas utiliser `return` dans `finally`, car il peut masquer le résultat ou l’exception.

Pour les objets `Closeable` ou `AutoCloseable`, il est préférable d’utiliser `use` :

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

Il ne faut pas absorber une `CancellationException` : elle doit être relancée, sans quoi l’annulation coopérative ne fonctionnera plus correctement.

Le bloc `finally` d’une coroutine annulée s’exécute, mais une opération de nettoyage suspendue peut immédiatement recevoir l’annulation. Pour un nettoyage suspendu court et indispensable, on utilise parfois `withContext(NonCancellable)`.

**En bref :** `try` exécute l’opération, `catch` traite l’exception correspondante et `finally` effectue le nettoyage quel que soit le résultat. Dans les coroutines, il est essentiel de ne pas absorber `CancellationException`, tandis que les ressources doivent de préférence être fermées avec `use`.

</details>

<details>
<summary>144. Quelle est la différence entre les exceptions vérifiées et non vérifiées ?</summary>

#### Kotlin

En Java, une exception vérifiée doit être soit interceptée, soit déclarée avec `throws`. Cette obligation ne s’applique pas aux exceptions non vérifiées. Kotlin ne possède pas d’exceptions vérifiées : le compilateur n’impose pas l’utilisation de `try/catch`.

### Java

Exception vérifiée :

```java
void readFile() throws IOException {
    new FileInputStream("file.txt");
}
```

L’appelant doit utiliser `try/catch` ou déclarer lui aussi l’exception avec `throws`.

Une exception non vérifiée est une `RuntimeException` ou l’une de ses sous-classes :

```java
String value = null;
value.length(); // NullPointerException
```

Le compilateur n’exige pas le traitement de `NullPointerException`, `IllegalArgumentException` ou `IllegalStateException`.

Les classes dérivées de `Error` sont également non vérifiées, mais ne sont généralement pas destinées à permettre une reprise.

### Kotlin

Kotlin permet d’appeler des API Java ou Kotlin susceptibles de lancer une `IOException` sans bloc `catch` obligatoire :

```kotlin
fun readFile(path: String): String =
    File(path).readText()
```

L’exception peut néanmoins se produire à l’exécution. Le contrat d’erreur doit donc être présenté explicitement dans l’API, la documentation ou le type de retour.

### Résultat attendu ou erreur de programmation

Dans la pratique, il est préférable de distinguer :

- les situations attendues et récupérables : mode hors ligne, identifiants incorrects, ressource introuvable ;
- les erreurs de programmation : argument incorrect, état impossible, erreur d’indice.

Une exception convient à une violation de contrat :

```kotlin
fun loadUser(id: String) {
    require(id.isNotBlank()) {
        "User id must not be blank"
    }
}
```

Pour les résultats métier attendus, il est préférable d’utiliser un résultat scellé :

```kotlin
sealed interface LoginResult {
    data object Success : LoginResult
    data object InvalidCredentials : LoginResult
    data object NetworkUnavailable : LoginResult
}
```

L’appelant voit ainsi les états connus par l’intermédiaire du système de types.

### Interopérabilité avec Java

Pour qu’un appelant Java voie la déclaration `throws`, Kotlin utilise `@Throws` :

```kotlin
@Throws(IOException::class)
fun readConfig(path: String): String =
    File(path).readText()
```

Pour un appelant Kotlin, le bloc `catch` reste facultatif.

### Coroutines

Les exceptions des coroutines sont elles aussi non vérifiées. `launch` propage l’échec dans la hiérarchie des jobs, tandis que `async` restitue l’exception lors de l’appel à `await()`.

`CancellationException` est un signal d’annulation qu’il ne faut pas absorber :

```kotlin
try {
    repository.sync()
} catch (exception: CancellationException) {
    throw exception
} catch (exception: IOException) {
    handleNetworkError(exception)
}
```

**En bref :** les exceptions vérifiées constituent un mécanisme Java imposant `catch` ou `throws`, tandis que les exceptions non vérifiées ne sont pas contrôlées par le compilateur. Kotlin ne possède pas d’exceptions vérifiées ; il est donc préférable de modéliser les erreurs attendues avec des types de retour et les violations de contrat avec des exceptions.

</details>

<details>
<summary>145. Quels types d’erreurs existe-t-il en Kotlin/Java ?</summary>

#### Kotlin

Sur la JVM, tout objet pouvant être lancé avec `throw` hérite de `Throwable`. Les deux branches principales sont `Exception` et `Error`.

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

Kotlin utilise la même hiérarchie JVM, mais ne possède pas d’exceptions vérifiées au niveau du compilateur.

### `Throwable`

Il s’agit du type de base de toutes les exceptions et erreurs. Intercepter `Throwable` est généralement dangereux : cela peut capturer une `Error` fatale ou l’annulation d’une coroutine.

### `Exception`

Cette branche représente les situations que le code de l’application peut potentiellement traiter :

- `IOException` ;
- `SQLException` ;
- `ParseException` ;
- `IllegalArgumentException` ;
- `IllegalStateException`.

### `RuntimeException`

Il s’agit d’une exception non vérifiée qui signale souvent une erreur de programmation ou une violation de contrat :

```kotlin
throw IllegalArgumentException("Invalid id")
throw IllegalStateException("User is not logged in")
```

Exemples : `NullPointerException`, `IndexOutOfBoundsException`, `ClassCastException` et `NumberFormatException`.

### Exceptions vérifiées et non vérifiées

Java impose `catch` ou `throws` pour les exceptions vérifiées, telles que `IOException`. Kotlin ne l’impose pas. `RuntimeException` et ses sous-classes sont non vérifiées dans les deux langages.

### `Error`

Cette branche représente un problème grave de la JVM ou de l’environnement d’exécution :

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
AssertionError
```

Le code de l’application ne doit généralement pas tenter de reprendre l’exécution après une `Error`.

### Erreurs métier

Il est préférable de modéliser les échecs métier attendus avec un type distinct plutôt qu’avec une exception :

```kotlin
sealed interface AppError {
    data object Network : AppError
    data object Unauthorized : AppError
    data class Validation(val message: String) : AppError
    data class Unknown(val cause: Throwable) : AppError
}
```

Les exceptions techniques sont converties en erreurs métier à la frontière de la couche de données :

```kotlin
fun Throwable.toAppError(): AppError = when (this) {
    is IOException -> AppError.Network
    is HttpException if code() == 401 -> AppError.Unauthorized
    else -> AppError.Unknown(this)
}
```

### `CancellationException`

Dans les coroutines, il s’agit d’un signal d’annulation et non d’un échec ordinaire :

```kotlin
try {
    repository.sync()
} catch (error: CancellationException) {
    throw error
} catch (error: IOException) {
    handleNetworkError(error)
}
```

Il ne faut pas l’absorber dans un bloc `catch` générique.

**En bref :** `Throwable` se divise en `Exception` et `Error`. Les exceptions d’exécution signalent souvent des erreurs de programmation ou des violations de contrat ; les échecs métier attendus doivent de préférence être modélisés avec des types métier, tandis que `CancellationException` doit être propagée dans les coroutines.

</details>

<details>
<summary>146. Quelle est la classe de base des erreurs ?</summary>

#### Kotlin

La classe de base de tout ce qui peut être lancé avec `throw` et intercepté avec `catch` est `Throwable`.

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

`Throwable` contient les informations fondamentales suivantes :

- le message ;
- la cause ;
- la trace de la pile d’appels ;
- les exceptions supprimées.

Seul un objet de type `Throwable` ou d’une de ses sous-classes peut être lancé :

```kotlin
throw IllegalArgumentException("Bad argument")
// throw "error" // erreur de compilation
```

`Exception` représente les problèmes que le code de l’application peut potentiellement traiter :

```kotlin
throw IOException("No internet")
throw IllegalStateException("User is not authorized")
```

`Error` représente les problèmes graves de la JVM ou de l’environnement d’exécution :

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Ces erreurs ne sont généralement pas interceptées dans le but de reprendre l’exécution.

Exception personnalisée :

```kotlin
class UnauthorizedException(
    message: String = "User is not authorized"
) : RuntimeException(message)
```

Pour les résultats métier attendus, il est souvent préférable d’utiliser un type scellé plutôt qu’une exception :

```kotlin
sealed interface AuthError {
    data object InvalidCredentials : AuthError
    data object Network : AuthError
}
```

Il faut éviter d’intercepter `Throwable` sans nécessité :

```kotlin
catch (throwable: Throwable) {
    log(throwable)
}
```

Cela risque d’intercepter une `Error` ou une `CancellationException`. Dans les coroutines, le signal d’annulation doit être propagé.

**En bref :** la classe de base est `Throwable`, dont dérivent `Exception` et `Error`. En production, il est préférable d’intercepter des exceptions récupérables précises plutôt que l’ensemble des objets `Throwable`.

</details>

<details>
<summary>147. Qu’est-ce que la complexité algorithmique ?</summary>

#### Kotlin

La complexité d’un algorithme décrit la croissance du temps d’exécution ou de la mémoire supplémentaire lorsque la taille de l’entrée `n` augmente. La notation Big O est la plus couramment utilisée.

```text
O(1) < O(log n) < O(n) < O(n log n) < O(n²)
```

Lors d’un entretien, il est important de préciser s’il s’agit de la complexité moyenne, dans le pire cas ou amortie.

### `O(1)`

Le coût n’augmente pas avec `n` :

```kotlin
val first = items.firstOrNull()
val user = usersById[id]
```

L’accès par indice dans une `ArrayList` est en `O(1)`. La recherche dans une `HashMap` est en moyenne en `O(1)`.

### `O(n)`

Un seul parcours de la collection :

```kotlin
fun findUser(users: List<User>, id: String): User? =
    users.firstOrNull { it.id == id }
```

Pour une liste, `map`, `filter` et `contains` sont généralement en `O(n)`.

### `O(n²)`

Comparaison imbriquée d’un grand nombre de paires :

```kotlin
for (i in items.indices) {
    for (j in i + 1 until items.size) {
        if (items[i] == items[j]) return true
    }
}
```

Pour rechercher des doublons, il est préférable d’utiliser un `Set` :

```kotlin
fun hasDuplicates(items: List<String>): Boolean {
    val seen = HashSet<String>()
    return items.any { !seen.add(it) }
}
```

Le temps est en moyenne en `O(n)` et l’espace en `O(n)`. Il s’agit d’un compromis consistant à utiliser davantage de mémoire pour gagner en vitesse.

### `O(log n)`

Recherche binaire dans des données triées :

```kotlin
val index = sortedItems.binarySearch(target)
```

La recherche est en `O(log n)`, mais le tri préalable peut coûter `O(n log n)`.

### `O(n log n)`

Il s’agit de la complexité typique d’un tri par comparaison :

```kotlin
val sorted = users.sortedBy(User::name)
```

### Temps et espace

```kotlin
val copy = users.toList()
```

Le temps est en `O(n)` et la mémoire supplémentaire en `O(n)`. Un algorithme sur place peut économiser de la mémoire, mais il modifie les données d’entrée et complique la gestion de la propriété et de la concurrence.

### Exemple Android

Mauvaise approche :

```kotlin
users.map { user ->
    user.id in selectedIds // selectedIds : List, contains en O(m)
}
```

La complexité totale est en `O(n × m)`.

Meilleure approche :

```kotlin
val selected = selectedIds.toHashSet()

val models = users.map { user ->
    user.toUi(isSelected = user.id in selected)
}
```

La création du `Set` est en `O(m)` et le mapping en `O(n)`, soit une complexité moyenne totale en `O(n + m)`.

La notation Big O ne correspond pas directement aux performances réelles : les constantes, les allocations, les entrées-sorties, la localité du cache et le budget du thread principal comptent également. Après avoir choisi la structure de données appropriée, il faut vérifier le résultat avec un profileur ou un benchmark.

**En bref :** la complexité décrit la manière dont le temps et l’espace nécessaires évoluent. Une complexité en `O(n²)` peut souvent être améliorée avec un `Set` ou une `Map`, mais il faut tenir compte de la mémoire, des cas moyen et défavorable ainsi que des mesures réelles.

</details>

<details>
<summary>148. Qu’est-ce que la recherche en profondeur (DFS) ?</summary>

#### Kotlin

La `DFS` (`Depth-First Search`, ou recherche en profondeur) est un algorithme de parcours d’un arbre ou d’un graphe. Il explore un chemin aussi profondément que possible, puis revient en arrière pour parcourir les autres branches.

Exemple de parcours d’un arbre :

```text
        A
      /   \
     B     C
    / \     \
   D   E     F
```

Un ordre DFS possible est :

```text
A -> B -> D -> E -> C -> F
```

### DFS dans un arbre

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

Dans un arbre, un ensemble `visited` n’est généralement pas nécessaire, car il n’existe pas de cycles.

### DFS dans un graphe

Un graphe peut contenir des cycles ; il faut donc utiliser un ensemble `visited` :

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

Sans `visited`, la DFS peut entrer dans une boucle infinie.

### DFS itérative

La récursion peut être remplacée par une pile :

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

`ArrayDeque` est utilisée comme une pile selon le principe LIFO grâce à `addLast/removeLast`.

### Complexité

Pour un graphe :

```text
Temps :  O(V + E)
Espace : O(V)
```

`V` représente les sommets et `E` les arêtes. L’espace est nécessaire pour l’ensemble `visited` ainsi que pour la pile de récursion ou la pile explicite.

### DFS ou BFS

La DFS :

- explore en profondeur ;
- utilise la récursion ou une pile ;
- est utile pour détecter les cycles, rechercher les composantes connexes et effectuer un tri topologique ;
- ne garantit pas le chemin le plus court dans un graphe non pondéré.

La BFS :

- procède niveau par niveau ;
- utilise une file ;
- trouve le chemin le plus court dans un graphe non pondéré.

Une DFS récursive peut provoquer une `StackOverflowError` sur une structure très profonde. Dans ce cas, il est préférable d’utiliser une DFS itérative.

**En bref :** la DFS est un parcours en profondeur réalisé par récursion ou avec une pile. Un graphe exige un ensemble `visited`, sa complexité est en `O(V + E)` et une version itérative est préférable pour les structures très profondes.

</details>

<details>
<summary>149. Qu’est-ce que la recherche en largeur (BFS) ?</summary>

#### Kotlin

La `BFS` (`Breadth-First Search`, ou recherche en largeur) est un algorithme qui parcourt un graphe ou un arbre niveau par niveau. Elle visite d’abord tous les sommets situés à une distance de 1 du point de départ, puis ceux à une distance de 2, de 3, et ainsi de suite.

La BFS utilise une file suivant le principe FIFO :

```text
départ -> voisins -> voisins des voisins -> ...
```

### BFS dans un graphe

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

L’ensemble `visited` est nécessaire pour éviter une boucle infinie dans un graphe contenant des cycles.

### Plus court chemin dans un graphe non pondéré

La BFS trouve le nombre minimal d’arêtes depuis le sommet de départ :

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

Un graphe pondéré nécessite l’algorithme de Dijkstra plutôt que la BFS.

### BFS dans un arbre

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

Il s’agit d’un parcours par niveaux.

### Complexité

```text
Temps :  O(V + E)
Espace : O(V)
```

`V` représente les sommets et `E` les arêtes. Pour un arbre, la complexité temporelle est en `O(n)`.

### BFS ou DFS

```text
BFS -> file, niveau par niveau, plus court chemin dans un graphe non pondéré
DFS -> pile/récursion, parcours en profondeur
```

La BFS convient souvent aux problèmes demandant « le nombre minimal d’étapes », « l’élément le plus proche » ou « la distance minimale dans une grille non pondérée ».

**En bref :** la BFS parcourt un graphe ou un arbre en largeur à l’aide d’une file. Elle trouve le plus court chemin dans un graphe non pondéré, exige un ensemble `visited` pour les graphes contenant des cycles et possède une complexité en `O(V + E)`.

</details>

<details>
<summary>150. Présentez la Clean Architecture.</summary>

#### Kotlin

La `Clean Architecture` est une approche consistant à diviser le code en couches avec une direction claire des dépendances. Son idée principale est que la logique métier ne dépend ni de l’interface utilisateur, ni du framework Android, ni de la base de données, ni du client réseau.

Architecture Android typique :

```text
présentation -> domaine <- données
```

- `présentation` — Activity/Fragment/Compose, ViewModel et état de l’interface utilisateur ;
- `domaine` — cas d’utilisation, règles métier, entités et contrats des repositories ;
- `données` — implémentations des repositories, API, Room, DataStore et mappers.

Règle de dépendance :

```text
la présentation dépend du domaine
les données dépendent du domaine
le domaine ne dépend d’aucun élément externe
```

Le domaine ne doit rien connaître de Retrofit, Room, `Context`, Compose ou du cycle de vie Android.

### Cas d’utilisation

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
) {
    suspend operator fun invoke(id: UserId): User = repository.getUser(id)
}
```

La ViewModel appelle le cas d’utilisation plutôt que directement l’API ou le DAO.

### Contrat du repository

Dans le domaine :

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Dans la couche de données :

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

La couche de données manipule les DTO et les entités, puis les convertit en objets du domaine :

```kotlin
fun UserDto.toDomain(): User = User(id = id, name = name)
fun UserEntity.toDomain(): User = User(id = id, name = name)
```

Il est préférable de ne pas transmettre directement les DTO et les entités Room à l’interface utilisateur lorsque leur cycle de vie ou leur sémantique diffèrent.

### Présentation

```kotlin
class ProfileViewModel(
    private val getUser: GetUserUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state
}
```

La ViewModel gère l’état de l’interface utilisateur sans savoir précisément d’où proviennent les données.

Avantages :

- la logique métier peut être testée sans Android ;
- la source de données peut être remplacée ;
- l’interface utilisateur n’est pas couplée à l’API ou à la base de données ;
- les frontières architecturales sont claires pour l’équipe ;
- la maintenance d’un produit volumineux est facilitée.

Inconvénients :

- davantage de fichiers et de code répétitif ;
- un risque de surarchitecture pour un CRUD simple ;
- de mauvaises abstractions compliquent le code ;
- la direction des dépendances exige de la discipline.

Règle pratique : il ne faut pas créer un cas d’utilisation ou une interface pour chaque accesseur trivial. La Clean Architecture doit réduire le couplage et améliorer la testabilité, et non simplement multiplier les classes.

**En bref :** sous Android, la Clean Architecture répartit généralement le code entre les couches présentation, domaine et données. Le domaine contient les règles métier et les contrats, la présentation affiche l’état, et la couche de données implémente l’accès à l’API et à la base de données. L’objectif principal est de maîtriser les dépendances et d’améliorer la testabilité.

</details>

<details>
<summary>151. Qu’est-ce que le MVVM ?</summary>

#### Kotlin

Le `MVVM` est le patron architectural `Model-View-ViewModel`, qui sépare l’interface utilisateur de la gestion de l’état et de la logique métier.

```text
Vue -> ViewModel -> Modèle
```

- la `Vue` affiche l’interface utilisateur et transmet les actions de l’utilisateur ;
- la `ViewModel` conserve l’état de l’écran, traite les actions et appelle les cas d’utilisation ;
- le `Modèle` correspond aux couches domaine et données : cas d’utilisation, repositories, API et base de données.

### Vue

Une interface Compose doit recevoir un état et des callbacks :

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

La Vue n’accède pas directement à l’API ou à la base de données et ne contient pas de logique métier.

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

La ViewModel survit aux changements de configuration et ne dépend pas d’une implémentation particulière de la Vue.

### Couche Modèle

```kotlin
class LoadProfileUseCase(
    private val repository: ProfileRepository
) {
    suspend operator fun invoke(): Profile = repository.loadProfile()
}
```

Le repository masque l’API, Room, le cache et DataStore.

### Flux de données

```text
Action utilisateur -> ViewModel -> Cas d’utilisation/Repository -> Mise à jour de l’état -> Affichage par la Vue
```

Il est préférable de représenter explicitement l’état de l’écran :

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val userName: String = "",
    val error: String? = null
)
```

La navigation, les snackbars et les toasts sont des effets ponctuels qui ne doivent pas être conservés comme un état persistant :

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
    data class ShowError(val message: String) : ProfileEffect
}
```

Avantages :

- moins de logique dans la Vue ;
- la ViewModel est facile à tester ;
- l’état survit à la rotation ;
- l’interface utilisateur peut évoluer sans réécriture des couches domaine et données ;
- le patron fonctionne bien avec Compose, XML, StateFlow et LiveData.

Erreurs courantes :

- placer la logique métier dans un Fragment ou un Composable ;
- faire communiquer directement la ViewModel avec Retrofit ou Room sans frontière architecturale ;
- transmettre sans raison des DTO ou des entités directement à l’interface utilisateur ;
- utiliser de nombreux états modifiables indépendants plutôt qu’un état global de l’écran ;
- conserver des événements ponctuels comme un état persistant.

**En bref :** dans le MVVM, la Vue affiche l’état, la ViewModel gère l’état et les actions, tandis que le Modèle, les cas d’utilisation et les repositories exécutent la logique métier et accèdent aux données. Un bon MVVM maintient une interface utilisateur légère et une ViewModel testable.

</details>

<details>
<summary>152. Quelle est la différence entre MVVM et MVI ?</summary>

#### Kotlin

`MVVM` et `MVI` sont deux approches de gestion de l’état de l’interface utilisateur. Toutes deux séparent l’interface de la logique métier, mais organisent différemment l’état, les événements et les effets secondaires.

### MVVM

```text
Vue -> ViewModel -> Modèle
```

- la `Vue` affiche l’interface utilisateur et appelle les méthodes de la ViewModel ;
- la `ViewModel` conserve l’état et appelle les cas d’utilisation ou les repositories ;
- le `Modèle` correspond aux couches domaine et données.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel() {
    private val _state = MutableStateFlow(ProfileState())
    val state: StateFlow<ProfileState> = _state

    fun load() { /* mettre à jour l’état */ }
    fun retry() { /* mettre à jour l’état */ }
}
```

Le MVVM est plus simple et plus flexible. Toutefois, sans discipline, la ViewModel peut devenir un ensemble désordonné de méthodes et d’états modifiables.

### MVI

```text
Événement/Intention -> ViewModel/Reducer -> État -> UI
                                        └-> Effet
```

L’interface utilisateur envoie des événements ou des intentions au lieu d’appeler de nombreuses méthodes différentes :

```kotlin
sealed interface ProfileEvent {
    data object LoadClicked : ProfileEvent
    data object RetryClicked : ProfileEvent
}
```

Il existe généralement un seul état immuable de l’écran :

```kotlin
data class ProfileState(
    val isLoading: Boolean = false,
    val user: UserUi? = null,
    val error: String? = null
)
```

Le point d’entrée est souvent unique :

```kotlin
fun onEvent(event: ProfileEvent) {
    when (event) {
        ProfileEvent.LoadClicked -> load()
        ProfileEvent.RetryClicked -> retry()
    }
}
```

### État et effets

Dans le MVVM, l’état peut être représenté par un objet unique ou par plusieurs flux. Dans le MVI, on utilise généralement un seul état d’écran et un reducer ou gestionnaire formel.

Dans les deux approches, la navigation, les snackbars et les toasts ne doivent pas être conservés comme un état persistant. On utilise pour cela des effets :

```kotlin
sealed interface ProfileEffect {
    data object NavigateBack : ProfileEffect
}
```

### Quand choisir chaque approche

MVVM :

- plus simple ;
- moins de code répétitif ;
- convient bien à la majorité des écrans Android ;
- plus facile à adopter par une équipe.

MVI :

- flux de données unidirectionnel et prévisible ;
- un seul état immuable ;
- logique des événements et du reducer facile à tester ;
- mieux adapté aux écrans complexes possédant beaucoup d’états ;
- utile aux grandes équipes qui souhaitent un flux uniforme.

Le principal inconvénient du MVI est son code répétitif pour les écrans simples. Celui du MVVM est le risque d’une gestion chaotique de l’état.

**En bref :** le MVVM est plus simple : la Vue appelle les méthodes de la ViewModel et affiche l’état. Le MVI est plus formel : l’interface envoie des événements, la ViewModel ou le reducer produit un état immuable, et les actions ponctuelles passent par des effets. Le MVVM convient à la plupart des écrans, tandis que le MVI est adapté aux flux complexes comportant de nombreux états.

</details>

<details>
<summary>153. Quels patrons architecturaux sont utilisés sous Android ?</summary>

#### Kotlin

Sous Android, les approches les plus courantes sont MVVM, MVI, la Clean Architecture, Repository, Use Case, l’architecture en couches, l’injection de dépendances et la modularisation par fonctionnalités. Le choix dépend de la complexité de l’application, de l’équipe et des exigences en matière de testabilité.

### MVVM

```text
Vue -> ViewModel -> Modèle
```

L’interface utilisateur affiche l’état, tandis que la ViewModel traite les actions de l’utilisateur et appelle les couches domaine et données.

```kotlin
class ProfileViewModel(
    private val loadProfile: LoadProfileUseCase
) : ViewModel()
```

Cette approche fonctionne bien avec Compose, XML, StateFlow et LiveData.

### MVI

```text
Événement/Intention -> ViewModel/Reducer -> État -> UI
```

L’interface utilisateur envoie des événements, la ViewModel produit un état immuable et les actions ponctuelles passent par des effets.

```kotlin
sealed interface ProfileEvent {
    data object RetryClicked : ProfileEvent
}
```

Cette approche est utile pour les écrans complexes comportant beaucoup d’états.

### Clean Architecture / Architecture en couches

```text
présentation -> domaine <- données
```

- `présentation` — interface utilisateur et ViewModel ;
- `domaine` — cas d’utilisation, règles métier et contrats ;
- `données` — implémentations des repositories, API et base de données.

Le domaine ne doit dépendre ni du framework Android, ni de Retrofit, ni de Room.

### Patron Repository

Le repository masque les sources de données :

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}
```

Son implémentation peut utiliser une API, Room, un cache ou DataStore sans que la ViewModel en ait connaissance.

### Patron Use Case

Un cas d’utilisation décrit une action métier précise :

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) {
    suspend operator fun invoke(email: String, password: String): User =
        repository.login(email, password)
}
```

Cela évite que la ViewModel ne se charge progressivement de toute la logique métier.

### Modularisation par fonctionnalités

```text
:feature:profile
:feature:search
:core:network
:core:database
:core:ui
```

Les modules organisés par fonctionnalité facilitent la mise à l’échelle de la base de code et la répartition des responsabilités. Les modules Core ne doivent contenir que l’infrastructure réellement partagée.

### Injection de dépendances

L’injection de dépendances n’est pas un patron d’interface utilisateur, mais elle fait partie de l’architecture :

```kotlin
class GetUserUseCase(
    private val repository: UserRepository
)
```

Hilt, Koin ou une injection manuelle relient les contrats à leurs implémentations.

Choix pratiques :

- application simple — MVVM avec Repository ;
- état d’écran complexe — MVI ;
- grande application — Clean Architecture avec modularisation ;
- fonctionnement offline-first — Repository avec une source de vérité locale ;
- nombreuses équipes — modules par fonctionnalité et contrats clairs.

**En bref :** une architecture Android combine généralement MVVM ou MVI pour l’état de l’interface, la Clean Architecture ou les couches pour la direction des dépendances, Repository et Use Case pour les frontières entre domaine et données, l’injection de dépendances pour l’assemblage, et les modules par fonctionnalité pour la mise à l’échelle. Un patron doit réduire la complexité plutôt que produire du code répétitif.

</details>

<details>
<summary>154. Comment organiser les modules dans un projet Android ?</summary>

#### Kotlin

Les modules d’un projet Android servent à isoler le code, améliorer les performances de compilation, répartir la responsabilité des fonctionnalités et permettre aux équipes de travailler en parallèle. Ce n’est pas leur nombre qui importe, mais la pertinence de leurs frontières et la direction de leurs dépendances.

Structure typique :

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

Une petite application n’a pas besoin de dizaines de modules. Il est préférable de commencer par une structure plus simple.

### `:app`

Point d’entrée de l’application :

- `Application` ;
- `Activity` principale ;
- hôte de navigation ;
- assemblage de l’injection de dépendances ;
- variantes et configuration de compilation ;
- liaison des modules de fonctionnalités et de données.

La logique métier ne doit pas résider dans `:app`.

### Modules de fonctionnalités

```text
:feature:profile
  ProfileScreen
  ProfileViewModel
  ProfileNavigation
```

Un module de fonctionnalité contient l’interface utilisateur et la logique de présentation d’une fonctionnalité précise. Il doit utiliser les contrats ou cas d’utilisation du domaine plutôt que les implémentations Retrofit ou Room directement.

### Modules Core

```text
:core:network  -> configuration de Retrofit/OkHttp
:core:database -> configuration de Room
:core:ui       -> système de design et composants
:core:common   -> utilitaires, dispatchers et types de résultats
```

Les modules Core ne doivent pas dépendre des modules de fonctionnalités.

### Modules du domaine

Le domaine contient les règles métier et les contrats :

```kotlin
interface UserRepository {
    suspend fun getUser(id: UserId): User
}

class GetUserUseCase(
    private val repository: UserRepository
)
```

Le domaine doit être aussi indépendant que possible du framework Android.

### Modules de données

La couche de données implémente les contrats du domaine :

```kotlin
class UserRepositoryImpl(
    private val api: UserApi,
    private val dao: UserDao
) : UserRepository
```

Elle contient les DTO, les entités Room, les mappers et les sources de données de l’API et de la base de données.

### Direction des dépendances

```text
feature -> domaine
données -> domaine
app     -> fonctionnalités + données
core    -> ne connaît pas les fonctionnalités ; les fonctionnalités peuvent connaître core
```

Il faut éviter les dépendances cycliques. Un module `core` ne doit connaître aucune fonctionnalité précise.

### Logique de compilation

Il est préférable d’extraire la configuration Gradle répétitive dans des plugins de convention :

```text
build-logic/convention/android-library.gradle.kts
build-logic/convention/compose-library.gradle.kts
```

Cela réduit la duplication dans les scripts de compilation.

Il ne faut pas diviser davantage lorsque :

- aucune équipe n’en assume une responsabilité distincte ;
- il n’existe aucune frontière réutilisable ;
- la compilation n’en tire aucun bénéfice ;
- le module ne fait qu’ajouter du code répétitif pour l’injection de dépendances, la navigation ou Gradle.

**En bref :** les modules peuvent être structurés autour de `app`, `feature`, `core`, `domain` et `data`. Une séparation est justifiée lorsqu’il existe une véritable frontière : responsabilité d’une fonctionnalité, infrastructure réutilisable, contrat du domaine, implémentation des données ou gain de compilation. Une modularisation excessive est nuisible.

</details>
