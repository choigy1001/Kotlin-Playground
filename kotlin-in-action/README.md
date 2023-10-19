# 코틀린 인 액션
# 1장 코틀린이란 무엇이고 왜 필요한가?

## 1.1 코틀린 맛보기
```kotlin
data class Person(val name: String, val age: Int? = null)

fun main(args: Array<String>) {
    val persons = listOf(Person("영희"), Person("철수", age = 29))
    val oldest = persons.maxByOrNull { it.age ?: 0 }
    println("나이가 가장 많은 사람: $oldest")
}
```

해당 코드를 보면 자바와 다른 점을 발견할 수 있다.  
위 코드에서 차례대로 차이점에 대해 알아보면 다음과 같다.
1. 데이터 클래스의 등장 및 중괄호의 부재
2. 널이 될 수 있는 타입(Int?)와 파라미터 디폴트 값(null)
3. new Person -> Person
4. age = 29 와 같이 이름 붙인 파라미터
5. 람다의 it, 엘비스 연산자 ?:
6. 문자열 탬플릿($oldest)  

이와 같이 새로운 것들을 참고해서 코틀린을 적극 사용해보자.

## 1.2 코틀린의 주요 특성
1. 자바가 실행되는 곳이라면 어디든 사용 가능
2. 정적 타입 지정 언어
3. 함수형 프로그래밍, 객체지향 프로그래밍
4. 무료 오픈 소스


# 2장 코틀린 기초

## 2.1 기본 요소: 함수와 변수

### 2.1.1 Hello, World!
```kotlin
fun main(args: Array<String>) {
    println("Hello, world!")
}
```
 위 코드를 토대로 우리는 코트린의 특성과 문법을 몇가지 파악할 수 있다.

- 함수 선언시에는 fun 키워드를 사용
- 파라미터 이름 뒤에 그 파라미터의 타입을 사용
- 함수를 최상위 수준에 정의할 수 있다. 꼭 클래스 안에 함수를 넣을 필요가 없음.
- 배열은 일반적인 클래스와 같다.
- 코틀린 표준 라이브러리는 자바 표준 라이브러리 함수를 간결하게 사용하도록 래퍼를 제공한다.  
  println도 그 중 하나이다.
- 세미콜론 생략 가능

### 2.1.2 함수
```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```
반환 타입을 설정하고 싶다면 매개변수 목록 뒤에 콜론과 함께 반환 타입을 명시하면 된다.  
if는 문이 아닌 식이다. 값을 만들어낸다면 식, 아니면 문이라고 할 수 있다.   

추가로 대입문은 식에서 문으로 바뀌었다. 그래서 아래와 같은 코드는 컴파일 오류가 발생한다.
```kotlin
fun test() {
    val a = 5
    println(a = 7) //컴파일 오류
}
```
<b>식이 본문인 함수 </b>
```kotlin
fun max(a: Int, b: Int): Int = if (a > b) a else b
```
if 식하나로 함수 본문이 이루어져 있다면 중괄호와 return을 생략할 수 있다.

함수가 중괄호로 둘러싸이면 블록이 본문인 함수, 등호와 식으로 이루어지면 식이 본문인 함수라 한다.  
식이 본문인 함수는 컴파일러가 타입 추론을 할 수 있기에 반환타입을 생략할 수 있다.

### 2.1.3 변수
```kotlin
val question = "이게 뭘까요?"
val answer = 42
```
- 코틀린은 위처럼 타입을 생략하여 선언할 수 있다.  
- 원한다면 타입 명시가 가능하다.

하지만 위처럼 초기화식을 사용하지 않고 변수를 선언하면 컴파일 에러가 발생하니 그럴땐 타입을 선언하자. 

<br>

<b>변경 가능한 변수와 변경 불가능한 변수</b>  
변수 선언 시 사용되는 키워드는 2가지이다.
- val : 변경 불가능한 참조를 저장하는 변수이다. 초기화하고 나면 재대입이 불가능하다.  
final 변수와 유사함.
- var : 변경 가능한 참조다. 자바로 따지면 일반 변수에 해당한다.

코틀린은 val 방식으로 변수를 선언하도록 권장하고 있다.

```kotlin
var answer = 42
answer = "no answer"
```
- 위 코드처럼 var이 가변이라고 하여 다른 타입으로 변경한다면 컴파일 오류가 발생하니 주의해서 사용하자.
- 다른 타입으로 변경하고 싶다면 강제 형변환을 사용해야 한다.

### 2.1.4 더 쉽게 문자열 형식 지정: 문자열 템플릿
```kotlin
fun main(args: Array<String>) {
    val name = "홍길동"
    println("Hello, $name")
    println("Hello, ${args[0]}")
}
```
- 문자열 템플릿을 이용하여 위 코드와 같이 변수를 문자열 안에 사용할 수 있다.
- 변수 이외의 식 또한 중괄호로 둘러싸 템플릿 안에 넣을 수 있다.
- 변수명만 사용하는 것도 좋지만 중괄호를 사용하는 것이 더 식별하기 쉽기에 중괄호 사용을 습관화하자.


## 2.2 클래스와 프로퍼티

```java
public class Person {
    private final String name;
    
    public Person(String name) {
        this.name = name;
    }

  public String getName() {
        return name;
  }
}
```
위 자바 코드는 아래와 같은 코틀린으로 변환할 수 있다.
```kotlin
class Person(val name: String)
```

- 코틀린의 클래스는 기본적으로 public 접근제어자를 가진다.

### 2.2.1 프로퍼티
```kotlin
class Person (
    val name: String,
    var isMarried: Boolean
)
```
- val은 final과 같다고 했기에 getter를 제공한다.
- var은 가변이기에 getter, setter 둘다 제공한다.

자바에서 lombok을 사용할때 isOpen과 같이 boolean으로 선언된 필드는  
getter를 isOpen 그대로 사용했듯  코틀린도 동일한 규칙으로 getter를 사용한다.
```kotlin
val person = Person("Bob", true)
println(person.name)
println(person.isMarried)
```
- getter를 호출하는 대신 프로퍼티를 직접 사용해도 코틀린이 자동으로 게터를 호출한다.

### 2.2.2 커스텀 접근자
```kotlin
class Rectangle(val height: Int, val width: Int) {
    val isSquare: Boolean
    get() {
        return height==width
    }
}
val rectangle = Rectangle(10, 10)
println(rectangle.isSquare)
```
- 위와 같이 커스텀 접근자를 선언할 수도 있다.

### 2.2.3 코틀린 소스코드 구조: 디렉터리와 패키지
<b> package</b>  

- 코틑린에도 패키지 개념이 존재한다.
- 자바에서는 디렉터리 구조가 패키지 구조를 그대로 따랐으나, 코틀린은 따르지 않아도 된다.
- 하지만 자바와 같이 패키지별로 디렉터리를 구성하는 편을 권장하고 있다. 
- 그렇다고 여러 클래스를 한 파일에 넣는 것을 두려워하지 말자.

<b> import </b>

- 다른 패키지에 정의한 선언을 사용하려면 Import를 통해 사용할 수 있다.
- 패키지 이름 뒤에 .*를 추가하면 패키지 안의 모든 선언을 임포트 할 수 있다는 점도 동일하다.

## 2.3 선택 표현과 처리: enum과 when
### 2.3.1 enum 클래스 정의
```kotlin
enum class Color {
    RED, ORANGE, YELLOW
}
```
- 코틀린은 자바와 다르게 enum class를 사용한다.
- 자바와 마찬가지로 프로퍼티와 메서드를 정의할 수 있다.

```kotlin
enum class Color(
  val r: Int, val g: Int, val b: Int
) {
   RED(255,0,0), ORANGE(255,165, 0);

  fun rgb() = (r * 256 + g) * 256 + b
}
```
- enum class 안에서 메서드를 정의하는 경우에는 위와 같이 세미콜론이 필수이다.

### 2.3.2 when으로 enum 클래스 다루기
```kotlin
fun getMnemonic(color: Color) =
    when (color) {
        Color.RED -> "Richard"
        Color.ORANGE -> "Of"
        Color.BLUE, Color.INDIGO -> "Cold"
    }
println(getMnemonic(Color.RED))
-> Richard
```
- when은 자바의 switch를 대신하면서 더 다양하게 사용할 수 있다.
- 한 분기안에 여러 값을 사용하려면 콤마를 통해 구분할 수 있다.
- 또한 Import를 통해서 enum 클래스 수식자인 Color를 생략할 수 있다.

### 2.3.3 when과 임의의 객체를 함께 사용

- 코틀린의 when은 자바의 switch와 달리 분기 조건에 객체를 허용한다.
```kotlin
fun mix(c1: Color, c2: Color) =
    when (setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE
        setOf(YELLOW, BLUE) -> GREEN
    }
println(mix(RED, YELLOW))
-> ORANGE
```
- 위 코드처럼 set 컬렉션을 만들어 비교하는 것이 가능하다.
- 조건을 비교하는 기준은 동등성이다.

```kotlin
fun Request.getBody() =
  when (val response = executeRequest()) {
     is Success-> response.body 
     is HttpError -> throw HttpException(response.status)
  }
```
- 더 나아가 when 절에도 변수에 식의 결과값을 대입하고 사용할 수 있다.
- 이를 통해 when 밖의 네임스페이스가 더럽혀지는 일을 줄일 수 있다.

### 2.3.4 인자 없는 when 사용
```kotlin
fun mixOptimized(c1: Color, c2: Color) =
  when {
    (c1 == RED && c2 == YELLOW) || (c1 == YELLOW && c2 == RED) -> ORANGE
      ...
  }
```
- when에 인자없이 사용할 수 있음을 확인할 수 있다.
- 2.3.3에서 봤던 코드와 달리 set 인스턴스를 생성하지 않아 불필요한 객체 생성을 줄인다.
- 하지만 이러한 경우는 가독성이 떨어진다는 장점이 있다.

### 2.3.5 스마트 캐스트: 타입 검사와 타입 캐스트를 조합
```kotlin
interface Expr
class Num(val value: Int): Expr
class Sum(val left: Expr, val right: Expr): Expr

fun eval(e: Expr): Int {
  if (e is Num) {
    val n = e as Num
    return n.value
  }
  if (e is Sum) {
      return eval(e.right) + eval(e.left)
  }
}
```
- 자바 스타일대로 작성된 코드이다.
- 특징으로 is를 사용해 변수 타입을 검사한다.
- 타입 검사 후 타입 캐스팅을 하는데 이는 불필요한 일이다.   
코틀린은 타입 검사 후 캐스팅하지 않아도 컴파일러가 캐스팅을 수행해준다. 이를 <b>스마트 캐스트</b>라고 부른다.

<b>스마트 캐스트 </b>
- 스마트 캐스트는 그 값이 바뀔 수 없는 경우에만 작동한다.
- 만약 변수가 var이거나, val을 사용하고 있으면서 커스텀 접근자를 사용한다면 변경 가능성이 있기 때문이다.

```kotlin
fun eval(e: Expr): Int =
  if (e is Num) {
    e.value
  } else if (e is Sum) {
    eval(e.right) + eval(e.left)
  }
```
- 코틀린스럽게 코드를 위와 같이 작성할 수 있다.
- if 분기에 식이 하나밖에 없다면 중괄호를 생략할 수 있다.
- 또한 if 분기에 블록을 사용하는 경우 마지막 식이 그 분기의 결과값이다.

```kotlin
fun eval(e: Expr): Int =
    when(e) {
        is Num ->
          e.value
        is Sum ->
          eval(e.right) + eval(e.left)
    }
    
```
- 더 나아가 if 대신 when을 사용하여 위와 같이 작성할 수 있다.

## 2.4  대상을 이터레이션: while과 for 루프
### 2.4.1 while 루프
```kotlin
while(조건) {
  ...
}
do {
  ...
} while (조건)
```
- 코틀린과 자바 동일하게 while 문을 사용할 수 있다.

### 2.4.2 수에 대한 이터레이션: 범위와 수열
```kotlin
fun fizzBuzz(i: Int) = when {
  i % 15 == 0 -> "Fizzbuzz"
  i % 3 == 0 -> "Fizz"
  else -> "$i"
}
for (i in 1..100) {
    print(fizzBuzz(i))
}
for (i in 100 downTo 1 step 2) {
    print(fizzBuzz(i))
}
```
- 코틀린은 1..100처럼 범위를 설정해서 사용한다.
- 또한 downTo를 사용하여 역방향 수열을 만들 수 있고, Step을 사용하여 증가값을 변경할 수 있다.
- 만약 100까지가 아니라 99까지만 탐색하고 싶은 경우가 있을 수 있는데 그럴땐 until을 사용하면 된다.  
`for (x in 0 until size)`  `for (x in 0..size-1)` 모두 동일하다.

### 2.4.3 맵에 대한 이터레이션

```kotlin
val binaryReps = TreeMap<Char, String>()
for (c in 'A'..'F') {
  val binary = Integer.toBinaryString(c.toInt())
  binaryReps[c] = binary
}
for ((letter, binary) in binaryReps) {
    println("$letter = $binary")
}
```
- `for (c in 'A'..'F')`처럼 문자 타입의 값에도 .. 연산자를 사용할 수 있다.
- 코틀린에서 Map 순회는 자바 Map 내부 클래스인 entry를 사용하는 것 보다 훨씬 간단하게 이용할 수 있다.
- 또한 map의 get, put을 사용하는 대신 `map[key]`, `map[key]= value`를 사용해 값을 획득하거나 설정할 수 있다.

```kotlin
val list = arrayListOf("10", "11")
for((index, element) in list.withIndex()) {
    println("$index: $element")
}
```
- 또한 맵이 아닌 컬렉션에도 맵에 사용했던 구조 분해구문을 적용할 수 있다.
- 그리하여 인덱스를 저장된 변수, 데이터가 저장된 변수를 동시에 사용할 수 있다.

### 2.4.4 in으로 컬렉션이나 범위의 원소 검사
```kotlin
fun isLetter(c: Char) = c in 'a'..'z' || c in 'A'..'Z'
println(isLetter('A'))
-> true
```
- in 연산자를 사용해 특정 값이 범위에 속하는지 검사할 수 있다.
- 반대로 !in을 사용하면 속하지 않는지 검사할 수 있다.

```kotlin
fun recognize(c: Char) = when (c) {
  in '0'..'9' -> "It's a digit"
  else -> "NO"
}
println(recognize('3'))
-> It's a digit
```
- in 연산자를 when 식에서도 사용할 수 있다.

범위는 숫자, 문자에만 국한되지 않는다.

- 비교가 가능한 클래스(Comparable 인터페이스를 구현한)라면 범위를 만들 수 있다.
- 예를 들어 String과 같이 Comparable 인터페이스를 구현한 클래스는 다음과 같이 범위를 정할 수 있다.  
`println("kotlin" in "java".."scala")`
- 더 나아가 컬렉션에 속하는지도 다음과 같이 확인할 수 있다.  
`println("kotlin" in setOf("Java", "Scala"))`

## 2.5 코틀린의 예외 처리
```kotlin
val percentage =
  if (number in 0..100) 
    number
  else
    throw IllegalArgumentException()
```
- 코틀린의 예외 처리 방식은 자바와 동일하다.
- 다른 점이 있다면 예외 인스턴스를 만들 때도 new를 붙일 필요가 없다.

### 2.5.1 try, catch, finally
```kotlin
fun readNumber(reader: BufferedReader) : Int? {
  try {
    val line = reader.readLine()
    return Integer.parseInt(line)
  } catch (e: NumberFormatException) {
    return null
  }
  finally {
    reader.close()
  }
}
```
- 자바 코드와 차이점으로는 입출력임에도 불구하고 IOException 예외에 대한 처리가 없다.
- 즉 코틀린은 자바처럼 Checked Exception, Unchecked Exception을 구분하지 않는다.

### 2.5.2 try를 식으로 사용
```kotlin
fun readNumber(reader: BufferedReader) {
  val number = try {
    Integer.parseInt(reader.readLine())
  } catch (e: NumberFormatException) {
      return
  }
}
```
- 코틀린의 try 키워드는 if나 when과 마찬가지로 식이다. 즉 try의 값을 변수에 대입할 수 있다.
- 주의사항으로는 if와 달리 try의 본문을 반드시 중괄호로 둘러싸야 한다.
- try 본문도 내부에 여러 문장이 있으면 마지막 식의 값이 리턴값이다.


# 3장 함수 정의와 호출
## 3.1 코틀린에서 컬렉션 만들기

```kotlin
val list = arrayListOf(1, 7, 35)
val map = hashMapOf(1 to "one", 7 to "seven")

println(list.javaClass)  
println(map.javaClass)
// -> class java.util.ArrayList
// -> class java.util.HashMap
```
- 위와 같이 컬렉션을 만들어 사용할 수 있다.
- 코틀린은 출력문의 결과처럼 자신만의 컬렉션을 제공하지 않는다.

## 3.2 함수를 호출하기 쉽게 만들기
### 3.2.1 이름 붙인 인자
```kotlin
fun <T> joinToString(
  collection: Collection<T>,
  separator: String,
  prefix: String,
  postfix: String
): String {
  val result = StringBuilder(prefix)
  for ((index, element) in collection.withIndex()) {
    if (index > 0) result.append(separator)
    result.append(element)
  }
  result.append(postfix)
  return result.toString()
}
```
- 위 함수는 IDE의 도움을 받으면 파라미터를 식별하기 쉬울 수 있으나 기본적으론 그렇지 않다.
- 코틀린은 함수를 호출할 때 파라미터 이름을 명시하여 호출할 수 있다.  
`joinToString(collection, separator = " ", prefix = " ", postfix = ".")`
- 주의할 점은 자바에서 함수 파라미터 정보를 넣는 것은 JDK 8 부터인데 코틀린은 JDK 6와 호환되기에  
자바 메서드를 호출할 때 파라미터 이름을 같이 못 넣는 경우가 발생할 수 있다.

### 3.2.2 디폴트 파라미터 값
```kotlin
fun <T> joinToString(
  collection: Collection<T>,
  separator: String = " ",
  prefix: String = "",
  postfix: String = ""
): String
```
- 코틀린의 함수 파라미터에 디폴트 값을 설정할 수 있다.
- 그래서 디폴트 값을 통해 상당수의 오버로딩을 줄일 수 있다는 장점이 있다.
- 기본적으로 함수를 호출할 때 파라미터 순서를 지키면서 사용하지만 이름을 붙인다면 순서 상관없이 호출할 수 있다.  
`joinToString(list, postfix=";", prefix="# ")`


주의사항
- 자바에는 디폴트 파라미터가 없기에 코틀린 함수를 자바에서 호출 시 모든 파라미터를 명시해서 사용해야 한다.
- 만약 자바에서 자주 코틀린 함수를 호출한다면   
`@JvmOverloads` 어노테이션을 통해 컴파일러가 자동으로 오버로딩 코드를 추가해준다.

### 3.2.3 정적인 유틸리티 클래스 없애기: 최상위 함수와 프로퍼티
```kotlin
package strings
fun joinToString(...): String {...}
```
- 코틀린은 이처럼 클래스 밖에 함수를 선언할 수 있다.
- JVM은 클래스 내부에 있는 코드만 동작할 수 있기에, 컴파일러는 이런 파일을 컴파일 할 때 새로운 클래스를 정의해준다.
- 클래스명은 파일의 이름과 동일하고 만약 생성되는 클래스의 이름을 바꾸고 싶다면 `@JvmName` 어노테이션을 이용할 수 있다.

<b>최상위 프로퍼티 </b>
```kotlin
var opCount = 0
fun performOperation() {
  opCount++
}
```
- 프로퍼티 또한 최상위에 둘 수 있다.
- 이런 프로퍼티 값은 정적 필드에 저장된다. 다만 프로퍼티를 접근하기 위해 게터, 세터를 사용해야 한다.
- 상수 자체로 이용하고 싶다면 `const val NAME = "홍길동"`으로 선언해서  
`public static final String NAME = "홍길동"` 자바 코드처럼 이용할 수 있다.

##3.3 메서드를 다른 클래스에 추가: 확장 함수와 확장 프로퍼티
```kotlin
package strings

fun String.lastChar(): Char = this.get(this.length - 1)
```
- 코틀린은 확장함수 개념을 통해서 기존의 API를 재작성하지 않고도 위 예제처럼 API를 추가할 수 있다.
- 확장할 클래스 이름을 <b>수신 객체 타입</b>, 호출하는 대상을 <b>수신 객체</b>라고 부른다.
- 확장 함수는 캡슐화를 깨지 않는다. 확장함수는 클래스 내부에서만 사용할 수 있는 함수(private)를 사용할 수 없다.

###3.3.1 임포트와 확장 함수
- 확장 함수를 사용하기 위해서는 똑같이 import를 해서 사용해야 한다.
- 만약 사용하는 클래스에서 import한 함수와 확장함수의 이름이 같다면 치환하여 충돌을 막을 수 있다.  
`import strings.lastChar as last`

### 3.3.2 자바에서 확장 함수 호출
- 자바에서 확장함수는 수신 객체를 첫 번째로 받는 정적 메서드이다. 
- 확장 함수를 StringUtil.kt 파일에 정의하면 다음과 같이 사용할 수 있다.   
`char c = StringUtilKt.lastChar("Java")`

### 3.3.3 확장 함수로 유틸리티 함수 정의
```kotlin
fun <T> Collection<T>.joinToString(
  separator: String = ", ",
  prefix: String = "",
  postfix: String = ""
): String {
  val result = StringBuilder(prefix)
  for ((index, element) in this.withIndex()) {
    if (index > 0) result.append(separator)
    result.append(element)
  }
  result.append(postfix)
  return result.toString()
}
```
- 확장함수를 적용하여 3.2.1에서 보았던 함수를 유틸리티 클래스의 확장함수로 적용하여 변경하였다.

### 3.3.4 확장 함수는 오버라이드할 수 없다.
```kotlin
open class View {
    open fun click() = println("View clicked")
}
class Button : View() {
    override fun click() = println("Button clicked")
}

val view: View = Button()
view.click()
-> Button clicked
```
- 코틀린의 메서드 오버라이드도 일반적인 메서드 오버라이드와 동잃하게 동작한다.

```kotlin
fun View.showOff() = println("I'm a view!")
fun Button.showOff() = println("I'm a button!")
val view: View = Button()
view.showOff()
-> I'm a view!
```
- 확장 함수는 오버라이드에 비해 정적으로 결정된다.
- 실제 타입은 Button임에도 View의 확장함수가 호출된다.

<b>Tip </b> 확장 함수와 멤버 함수 모두 이름과 시그니처가 같다면?

- 기본적으로 멤버 함수가 우선권을 가지고 호출된다.
- 그렇기에 기존에 확장 함수를 쓰고 있더라도 클래스에 멤버함수가 추가되면 재컴파일 하는 순간부터 멤버 함수를 사용한다.

### 3.3.5 확장 프로퍼티
- 확장함수처럼 확장 프로퍼티도 사용할 수 있다.
- 하지만 상태를 저장할 방법이 없기에 확장함수를 조금 더 짧게 사용할 수 있는 효과정도를 가진다.
```kotlin
val String.lastChar: Char
  get() = get(length - 1)
```

- 상태를 저장할 방법이 없기 때문에 기본 게터 구현을 제공할 수 없기에 최소한 게터는 꼭 정의를 해야 한다.

## 3.4 컬렉션 처리: 가변 길이 인자, 중위 함수 호출, 라이브러리 지원
### 3.4.1 자바 컬렉션 API 확장
- 이전에 컬렉션의 last, max 와 같은 함수는 모두 확장함수였다는 것을 3.3을 보고 알 수 있었다.
- 실제로 구현을 확인해보면 다음과 같다.
```kotlin
fun <T> List<T>.last(): T {
    // ...
}
fun Collection<Int>.max(): Int {
    // ...
}
```

### 3.4.2 가변 인자 함수: 인자의 개수가 달라질 수 있는 함수 정의
- 코틀린의 가변 인자는 자바의 가변 인자와 개념이 같다.
- 차이점은 타입 뒤 ... 대신 파라미터 앞에 vararg 변경자를 붙인다.
- 또한 기존에 자바에서 가변 인자에 배열을 그냥 넘겼다면 코틀린은 스프레드 연산자와 함께 넘겨야 한다.
```kotlin
fun main(args: Array<String>) {
  val list = listOf("args:", *args)
  println(list)
}
```

### 3.4.3 값의 쌍 다루기: 중위 호출과 구조 분해 선언
```kotlin
val map = mapOf(1 to "one", 7 to "seven")
```
- 위 코드에서 to는 키워드가 아닌 메서드이다.
- 이 코드에서는 중위 호출이라는 개념이 적용된다.
- 인자가 하나만 있는 일반 메서드나 인자가 하나뿐인 확장 함수에 중위 호출을 사용할 수 있다.

```kotlin
infix fun Any.to(other: Any) = Pair(this, other)
```
- 함수를 중위 호출에 사용하게 허용하고 싶으면 infix를 함수 선언 앞에 추가하며 된다.

<b>Pair</b>  
```kotlin
val (number, name) = 1 to "one"
```
- Pair는 말 그대로 두 원소로 이루어진 순서쌍을 반환한다.
- 위와 같이 코드를 구조 분해 선언이라고 부른다.
- Pair 인스턴스 외에 다른 객체에도 구조 분해를 적용할 수 있다.

## 3.5 문자열과 정규식 다루기
### 3.5.1 문자열 나누기
```kotlin
println("12.345-6.A".split("\\.|-".toRegex()))
```
- 코틀린은 정규식 문법은 자바와 동일하다.
- 코틀린은 자바의 split과는 다르게 확장함수를 제공하여 사용법에 대한 혼동을 줄였다.

```kotlin
prtinln("12.345-6.A".split(".", "-"))
```
- 이처럼 여러 구분 문자열을 받아 문자열을 나눌 수도 있다.

## 3.6 코드 다듬기: 로컬 함수와 확장
```kotlin
class User(val id: Int, val name: String, val address: String)

fun saveUser(user: User) {
  fun validate(user: User, value: String, fieldName: String) {
    if (value.isEmpty()) {
      if (user.name.isEmpty()) {
        throw IllegalArgumentException("Can't Save user ${user.id}: empty $fieldName")
      }
    }
  }
  validate(user, user.name, "Name")
  validate(user, user.address, "Address")
}
```

- 코틀린은 로컬 함수 즉 선언한 함수 내에 다시 함수를 작성할 수 있다.
- 또한 로컬 함수에서 바깥쪽의 파라미터, 변수를 사용할 수 있다.
- 중첩된 함수가 많을수록 코드는 읽기 어려워지니 일반적으로 한단계만 중첩하길 권장한다.