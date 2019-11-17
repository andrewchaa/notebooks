# Kotlin

## Class

#### Creating instances of classes

```kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```

## Extensions

Translated as Java static function and it doesn't hide member function

```kotlin
fun String?.isEmptyOrNull(): Boolean {
    return this == "" || this == null
}

infix fun <T> T.eq(other: T) {
    if (this == other) println("OK")
    else println("Error: $this != $other")
}
```

