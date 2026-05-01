```kotlin
fun alphabet(): String = with(StringBuilder()) {
    for (letter in 'A'..'Z') {
        this.append(letter)
    }
    // append("\nNow I know the alphabet!")
    this.append("\nNow I know the alphabet!")
    this.toString()
}
```

`with`는 첫 번째 인자로 받은 객체를 두 번째 인자로 받은 람다의 수신 객체로 만든다. 
