# Android Coding Conventions
The document enlist certain mistakes and their fixes


### WHEN


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


### Top-Level (Extension) Functions for Utility Functions

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


```kotlin
```
```kotlin
```
```kotlin
```
```kotlin
```
```kotlin
```
```kotlin
```


