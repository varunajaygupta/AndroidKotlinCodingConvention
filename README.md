# Android Coding Conventions
The document enlist code snippets focusing on common mistakes and their suggested solution.

### 1. WHEN


__BAD__:

```kotlin
fun getDefaultLocale(deliveryArea: String): Locale {
    val deliverAreaLower = deliveryArea.toLowerCase()
    if (deliverAreaLower == "germany" || deliverAreaLower == "austria") {
        return Locale.GERMAN
    }
    if (deliverAreaLower == "usa" || deliverAreaLower == "great britain") {
        return Locale.ENGLISH
    }
    if (deliverAreaLower == "france") {
        return Locale.FRENCH
    }
    return Locale.ENGLISH
}
```

__GOOD__:

```kotlin
fun getDefaultLocale2(deliveryArea: String) = when (deliveryArea.toLowerCase()) {
    "germany", "austria" -> Locale.GERMAN
    "usa", "great britain" -> Locale.ENGLISH
    "france" -> Locale.FRENCH
    else -> Locale.ENGLISH
}
```

Every time you write an if consider if it can be replaced with a more concise when expression.


### 2. Top-Level (Extension) Functions for Utility Functions

__BAD__:
```kotlin
object StringUtil {
    fun countNumberOfX(string: String): Int{
        return string.length - string.replace("x", "").length
    }
}
```
```kotlin
StringUtil.countNumberOfX("xFunxWithxKotlinx")
```
__GOOD__:
```kotlin
fun String.countNumberOfX: Int {
    return length - replace("x", "").length
}
```

```kotlin
"xFunxWithxKotlinx".countNumberOfX()
```
Kotlin allows avoiding the unnecessary wrapping util class and use top-level functions instead.


### 3. Don’t Overload for Default Arguments

__BAD__:
```kotlin
fun findString(name: String){
    find(name, true)
}
fun findString(name: String, recursive: Boolean){
}
```
__GOOD__:
```kotlin
fun findString(name: String, recursive: Boolean = true){
}
```
Default arguments remove nearly all use cases for method and constructor overloading in general, because overloading is mainly used to create default arguments.

### 4. Avoid if null Checks

__BAD__:
```kotlin
if (order == null || order.customer == null || order.customer.address == null){
    throw IllegalArgumentException("Invalid Order")
}
val address =order.customer.address.city
```
__GOOD__:
```kotlin
val city = order?.customer?.address?.city ?: throw IllegalArgumentException("Invalid Order")
```
Always make fields as nullable which can hold null values and instead of using if-null checks you can use safe call operator along with the elvis operator.


### 5. Refer to Constructor Parameters in Property Initializers

__BAD__:
```kotlin
class UsersClient(baseUrl: String, appName: String) {
    private val usersUrl: String
    private val httpClient: HttpClient
    init {
        usersUrl = "$baseUrl/users"
        val builder = HttpClientBuilder.create()
        builder.setUserAgent(appName)
        builder.setConnectionTimeToLive(10, TimeUnit.SECONDS)
        httpClient = builder.build()
    }
    fun getUsers(){
        //call service using httpClient and usersUrl
    }
}
```

__GOOD__:
```kotlin
class UsersClient(baseUrl: String, appName: String) {
    private val usersUrl = "$baseUrl/users"
    private val httpClient = HttpClientBuilder.create().apply {
        setUserAgent(appName)
        setConnectionTimeToLive(10, TimeUnit.SECONDS)
    }.build()
    fun getUsers(){
        //call service using httpClient and usersUrl
    }
}
```
* We can refer to the primary constructor parameters in property initializers (and not only in the init block). 
* apply() can help to group initialization code and get along with a single expression.


### 6. Type Inference

__BAD__:
```kotlin
public val something: MyType = MyType()
private val meaningOfLife: Int = 42
```

__GOOD__:
```kotlin
val something = MyType()
private val meaningOfLife = 42
```
* Type inference should be preferred where possible instead of explicitly declaring types.
* Only include visibility modifiers if you need something other than the default of public.


### 7. Immutability

__BAD__:
```kotlin
var eligibleCountries= arrayListOf("Hungary", "Ireland", "Italy", "Latvia", "Lithuania", "Luxembourg")

fun isCountryEligible(countryName: String, countriesEligible: ArrayList<String>):Boolean{
    return countriesEligible.any { it.equals(countryName) }
}
```

__GOOD__:
```kotlin
val eligibleCountries= listOf("Hungary", "Ireland", "Italy", "Latvia", "Lithuania", "Luxembourg")

fun isCountryEligible(countryName: String, countriesEligible: List<String>):Boolean{
    return countriesEligible.any { it.equals(countryName) }
}
```


* Prefer using immutable data to mutable. Always declare local variables and properties as val rather than var if they are not modified after initialization.
* Always use immutable collection interfaces (Collection, List, Set, Map) to declare collections which are not mutated. 

### 8. String templates

__NOT PREFFERED__:
```kotlin
var word ="aaXdsXX"
var count=countOccurrences(word,'X')

fun countOccurrences(s: String, ch: Char): Int {
    return s.length - s.replace(ch.toString(), "").length
}

val message = "No of X found" +count +"in word with length "+ word.length

```
```kotlin
println(message)
// No of X found 3 in word with length 7
```

__PREFFERED__:
```kotlin
var word ="aaXdsXX"
var count=countOccurrences(word,'X')

fun countOccurrences(s: String, ch: Char): Int {
    return s.length - s.replace(ch.toString(), "").length
}

val message = "No of X found $count in word with length ${word.length}"
```
```kotlin
println(message)
// No of X found 3 in word with length 7
```
Here we can use String Templates. A template expression starts with a dollar sign ($) and consists of either a name or an expression in curly braces. 
