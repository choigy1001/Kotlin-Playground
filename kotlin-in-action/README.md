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

# 4장 클래스, 객체, 인터페이스
## 4.1 클래스 계층 정의
### 4.1.1 코틀린 인터페이스
```kotlin
interface Clickable {
  fun click()
}
class Button : Clickable {
  override fun click() = println("clicked")
}
println(Button().click())
// -> clicked
```
- 인터페이스나 상속 모두 하위 클래스에서 콜론(:)을 활용하여 구현할 수 있다.
- 코틀린 인터페이스는 자바 8 인터페이스처럼 추상 메서드뿐만 아니라 디폴트 메서드 구현이 가능하다.
- 다만 아무런 상태(필드)도 들어갈 수 없다.
- 자바와 다르게 override 변경자를 꼭 작성해야 한다. (미작성 시 컴파일 오류)

```kotlin
interface Focusable {
    fun showOff() = println("focusable")
}
interface Clickable {
    fun showOff() = println("clickable")
}
class Show : Focusable, Clickable {
    fun print() = showOff() // 컴파일 오류 발생
}
```
- 동일한 디폴트 메서드를 호출하면 컴파일 오류가 발생한다.
- 코틀린 컴파일러는 이런 상황에서 강제로 하위 클래스에서 메서드를 새로 구현하도록 한다.

```kotlin
class Show : Focusable, Clickable {
    override fun showOff() {
        super<Clickable>.showOff()
        super<Focusable>.showOff()
    }
}
```
- 상위 타입의 메서드는 super와 꺾쇠 안에 타입을 지정하여 호출할 수 있다.

<b>Tip</b>

- 코틀린은 JDK 6과 호환되어 설계 되었다. 그래서 디폴트 메서드가 있는 인터페이스를  
일반 인터페이스와 디폴트 메서드가 정적 메서드로 들어 있는 클래스를 조합해 구현한다.
- 자바에서 코틀린의 인터페이스를 구현하여 디폴트 메서드를 바로 사용할 수 없으므로,   
자바 레벨에서 모두 디폴트 메서드를 포함한 메서드 모두 오버라이딩이 필요하다.


### 4.1.2 open, final, abstract 변경자: 기본적으로 final
```kotlin
open class RichButton: Clickable { //열려 있는 클래스
  fun disable() { //final 메서드
  }
  open fun animate() { //오버라이드 가능
  }
  override fun click() { //기본적으로 오버라이드된 메서드는 열려있다.
  }
  final override fun test() { // 하위 클래스에서 재정의 불가능
  }
}
```
- 코틀린의 클래스와 메서드는 기본적으로 final이다.
- 클래스, 메서드, 프로퍼티 모두 상속을 허용하려면 앞단에 open 변경자를 붙여야 한다.
- 재정의한 메서드 앞에 final을 붙여 하위 클래스에서 재정의 할 수 없게 설정 가능하다.
- final을 사용하면 변경 가능성이 없다는 것을 보장해 스마트 캐스트가 가능하다는 점이다.

```kotlin
abstract class Animated {
    abstract fun animate()
}
```
- 코틀린도 자바와 같이 abstract 클래스를 선언할 수 있다.
- 자바처럼 인스턴스화 할 수 없다.
- 추상 클래스는 기본적으로 open 변경자를 명시할 필요가 없다.
- 본문이 있는 메서드는 기본적으로 final이지만 open으로 재정의가 가능하다.

### 4.1.3 가시성 변경자: 기본적으로 공개
- 코틀린은 기본적으로 가시성이 자바와 다르게 public이다. (자바는 package-private)
- 코틀린의 protected는 자바와 다르게 같은 패키지가 아닌 해당 클래스와 하위 클래스를 뜻한다.   

<b>코틀린 가시성 변경자</b>

  |변경자|클래스 멤버|최상위 선언|
  |------|---|---|
  |public(기본)|모든 곳에서 볼 수 있음|모든 곳에서 볼 수 있음|
  |internal|같은 모듈 안에서만 볼 수 있음|같은 모듈 안에서만 볼 수 있음|
  |protected|하위 클래스 안에서만 볼 수 있음|적용 x|
  |private|같은 클래스 안에서만 볼 수 있음|같은 파일 안에서만 볼 수 있음|

### 4.1.4 내부 클래스와 중첩된 클래스: 기본적으로 중첩 클래스
- 코틀린도 클래스 안에 다른 클래스를 선언할 수 있다.
- 차이점은 중첩 클래스는 명시적으로 요청하지 않는 한 바깥쪽 클래스 인스턴스에 대한 접근 권한이 없다.
```kotlin
interface State: Serializable

interface View{
  fun getCurrentState(): State
  fun restoreState(state: State){}
}

class Button : View{
  override fun getCurrentState(): State = ButtonState()
  
  override fun restoreState(state: State){
    ...
  }
  
  class ButtonState: State{
    ...
  }
  // 내부 클래스
  inner ButtonState: State{
    
  }
}
```
<b>용어 정리</b>

- 중첩 클래스: 바깥쪽 클래스에 대한 참조를 저장하지 않음
- 내부 클래스: 바깥쪽 클래스에 대한 참조를 저장함

<b>자바 기준</b>
```java
public class Outer{
  class Inner { //내부 클래스
  }

  static class Nested { //중첩 클래스
  }
}
```
<b>코틀린 기준</b>
```kotlin
class Outer{
  inner class Inner { //내부 클래스
  }
  class Nested { //중첩 클래스
  }
}
```
- 코틀린은 자바와 달리 내부 클래스를 사용하는 경우에는 바깥쪽 클래스의 참조에 접근하려면  
`this@Outer` 라고 작성해야 한다.
- 생략해서 사용할 수 있지만 같은 이름이 있다면 내부 클래스가 우선권을 가진다.
```kotlin
fun main() {
    val outer = Outer()
    outer.Inner().print()
}
class Outer {
    val cnt: Int = 0
    fun test() = println("123")

    inner class Inner {
        val cnt = 5

        val innerCnt = this@Outer.cnt + 2

        val wrongInnerCnt = cnt + 1
        fun print() {
            println(innerCnt)
            println(wrongInnerCnt)
        }
        fun printSeveral() = this@Outer.test()
    }

}
// 2 6 이 순서대로 출력
```

### 4.1.5 봉인된 클래스: 클래스 계층 정의 시 계층 확장 제한
```kotlin
sealed class Expr {
    class Num(val value: Int): Expr
    class Sum(val left: Expr, val right: Expr): Expr
}

fun eval(e: Expr): Int =
  when (e) {
    is Expr.Num -> e.value
    is Expr.Sum -> eval(e.right) + eval(e.left)
  }
```
- 코틀린에는 sealed Class 라는 개념이 도입되었다.
- sealed Class를 정의하면 하위 클래스 정의를 제한할 수 있다.
- 즉 봉인된 클래스 외부에서 Expr를 구현한 하위 클래스를 정의할 수 없다. (같은 패키지는 가능)
- 장점으로 선언한 메서드처럼 특정 클래스에 대한 분기처리를 컴파일 타임에 잡아준다.
- 즉 하위 클래스에 대한 분기처리가 빠져있다면  
else를 사용하거나 분기처리를 추가를 컴파일 타임에서 확인할 수 있다.

<b>Tip</b>

- 코틀린 1.5부터는 위 코드처럼 봉인된 클래스 안이 아닌   
같은 패키지 내에 어떤 위치에든 상위 클래스를 구현한 하위 클래스를 정의할 수 있다.

## 4.2 뻔하지 않은 생성자와 프로퍼티를 갖는 클래스 선언

### 4.2.1 클래스 초기화: 주 생성자와 초기화 블록
```kotlin
class User(val nickname: String)

class User constructor(_nickname: String) {
  val nickname: String
  init {
    nickname = _nickname
  }
}
// 모두 같은 의미의 코드이다
```
- 코틀린의 생성자는 크게 주 생성자 부 생성자로 나눌 수 있다.
  - 위 코드의 첫 번재 클래스 선언에 괄호로 둘러싸인 코드를 주 생성자라고 한다.
- 또한 초기화 블록을 통해 초기화 로직을 추가할 수 있다.
  - 두 번째 클래스 선언을 보면 init 키워드의 블럭 안에 초기화 코드가 작성되어 있다.
  - 보통 주 생성자가 제한적이기 때문에 주 생성자에 별도의 코드를 포함하기 위해 함께 사용된다.
- `_nickname`은 프로퍼티와 생성자 파라미터를 구분해준다.

```kotlin
class User(val nickname: String, val isSubscribed: Boolean = true)
```
- 생성자 파라미터에도 디폴트 값을 지정할 수 있다.

```kotlin
open class User(val nickname: String) {
    //...
}
class XUser(nickname: String) : User(nickname) {
    //...
}
```
- 클래스에 기반 클래스가 있다면 주 생성자에서 기반 클래스의 생성자를 호출해야 한다.
- 만약 클래스 정의 시 생성자를 정의하지 않으면 자바와 동일하게 컴파일러가 인자가 없는 디폴트 생성자를 만들어준다.
- 인터페이스는 생성자가 없다. 
  - 그래서 클래스의 상위 클래스 목록 중 기반 클래스, 인터페이스를 구분할 때 괄호 유무를 통해 손쉽게 파악할 수 있다.
- 당연하게도 생성자에 접근 제어자를 사용할 수 있다.

### 4.2.2 부 생성자: 상위 클래스를 다른 방식으로 초기화
```kotlin
open class View {
  constructor(ctx: Context) {
      //...
  }
  constructor(ctx: Context, attr: AttributeSet) {
      //...
  }
}
```
- 생성자가 여러 개 필요한 경우도 무조건 있다.
- 위의 코드와 같이 여러 개의 생성자를 사용할 수 있다.

```kotlin
class MyButton: View {
  constructor(ctx: Context): super(ctx) {
      //...
  }
  constructor(ctx: Context): this(ctx, MY_STYLE) {
      //...
  }
}
```
- 자바와 마찬가지로 생성자를 다른 생성자에 위임하거나, 상위 클래스의 생성자를 호출할 수 있다.

### 4.2.3 인터페이스에 선언된 프로퍼티 구현
```kotlin
interface User {
    val nickname: String
}

class PrivateUser(override val nickname: String): User // 주 생성자를 이용한 상태 저장

class SubscribingUser(val email: String) : User { //커스텀 게더 사용
  override val nickname: String
    get() = TODO()
}

class FaceBookUser(val accountId: Int) : User { //프로퍼티 초기화 식
    override val nickname = getFacebookName(accountId)
}
```
- 코틀린의 인터페이스에 추상 프로퍼티 선언을 넣을 수 있다.
- 코틀린의 인터페이스는 상태를 저장할 수 없다.
- 상태 저장을 위해 하위 클래스에서 3가지 방식이 존재한다.
  - 주 생성자, 커스텀 게더, 프로퍼티 초기화 식
  - 커스텀 게더와 프로퍼티 초기화 식에 차이점은 커스텀 게더는 매번 호출될 때마다 계산하고, 프로퍼티는 초기화할 때만 계산된다.  
    즉 커스텀 게더는 저장이 아닌 매번 바뀌고, 프로퍼티 초기화 식은 상태가 뒷받침 필드에 저장되어 똑같이 사용.

### 4.2.4 게터와 세터에서 뒷받침하는 필드에 접근
```kotlin
class User(val name: String) {
    var address: String = "unspecified"
      set(value: String) {
        println("$field -> $value")
        field = value
      }
}
val User = User("Alice")
user.address = "new Address"
```
- 코틀린에서 프로퍼티의 값을 바꿀때는 `user.address = "new Address"` 처럼 필드 설정 구문을 사용한다.
- `field`라는 식별자를 통해 뒷받침하는 필드에 접근할 수 있다. 여기선 `field`를 통해 address에 접근

```kotlin
class LengthCounter {
  var counter: Int = 0
  private set
  fun addWord(word: String) {
    counter += word.length

  }
}
```
- 기본적으로 접근자(여기선 세터)는 기본적으로 프로퍼티의 가시성과 같다.
- 하지만 위 코드처럼 가시성을 변경하여 외부 클래스에서의 접근을 막을 수 있다.

## 4.3 컴파일러가 생성한 메서드: 데이터 클래스와 클래스 위임
### 4.3.1 모든 클래스가 정의해야 하는 메서드
- 코틀린 또한 자바의 Object 클래스 메서드인 `toString(), equals(), hashCode()`를 오버라이드 할 수 있다.
- 코틀린에서 `==` 연산자는 동등성 즉 자바의 `equals()` 와 같은 역할을 `===` 연산자는 객체의 동일성을 체크한다.

### 4.3.2 데이터 클래스: 모든 클래스가 정의해야 하는 메서드 자동 생성
```kotlin
data class Client(val name: String, val postalCode: Int)
```
- 코틀린의 data class는 equals(), hashCode(), toString()에 대한 재정의를 자동으로 해준다.
- 데이터 클래스의 프로퍼티가 꼭 val일 필요는 없지만 불변을 보장하기 위해 val을 사용하길 권장한다.
- 객체의 복사본을 만들 수 있는 copy() 메서드를 제공한다. 깊은 복사로 제공

### 4.3.3 클래스 위임: by 키워드 사용

```kotlin
class DelegatingCollection<T> : Collection<T> {
  private val innerList = arrayListOf<T>()
  override val size: Int get() = innerList.size
  override fun isEmpty(): Boolean = innerList.isEmpty()
  //...
}

// 아래와 같이 변경 가능
class DelegatingCollection<T>(
  innerList: Collection<T> = ArrayList()
) : Collection<T> by innerList()
```
- 상속으로 인해 시스템이 취약하게 될 수 있다. (상위 클래스의 변경으로 인해)
- 상속을 허용하지 않은 클래스에 새로운 동작을 추가해야 할 일이 발생할때 데코레이터 패턴을 사용해서 해결한다.
  - 하지만 준비 코드가 상당하다는 단점이 있는데 코틀린의 by 키워드가 이를 대신해준다.
  - 또한 by 키워드를 통해 그 인터페이스에 대한 구현을 다른 객체에 위임 중이라는 사실을 명시할 수 있다.

```kotlin
class CountingSet<T>(
  val innerSet: MutableCollection<T> = HashSet<>()
): MutableCollection<T> by innerSet {
  var objectsAdded = 0
  override fun add(element: T): Boolean {
    objectsAdded++
    return innerSet.add(element)
  }
}
```
- 위와 같이 오버라이딩 해서 새로운 기능으로 사용가능하다.
- by 키워드는 자바와 비교했을때 필요한 기능에 대해서만 오버라이딩을 하면 된다는 점에서 편리하다고 생각

## 4.4 object 키워드: 클래스 선언과 인스턴스 생성

- object 키워드를 사용하는 경우
  - 객체 선언을 통해 싱글턴을 정의하는 방법
  - 동반 객체
  - 자바의 익명 클래스 대신 사용

### 4.4.1 객체 선언: 싱글턴을 쉽게 만들기
```kotlin
object Payroll {
  val allEmployees = arrayListOf<Person>()
  fun calculateSalary() {
    for (person in allEmployees) {
        //...
    }
  }
}
```
- 위처럼 객체 선언 기능을 통해 인스턴스를 하나만 선언하여 사용하도록 한다.
- 객체 선언 기능은 클래스 선언과 단일 인스턴스 선언을 합친 선언이다.

### 4.4.2 동반 객체: 팩토리 메서드와 정적 멤버가 들어갈 장소

- 코틀린 클래스 안에는 정적인 멤버가 없다. static을 지원하지 않음
- 그 대신 패키지 수준의 최상위 함수와 객체 선언을 활용한다.

```kotlin
class Triangle (val length: Int) {
    private val name: String
        get() {
            TODO()
        }
}
fun test() {
    val triangle = Triangle(12)
    // triangle.name 컴파일 오류
}
```
- 최상위 함수는 기본적으로 비공개 멤버에 접근할 수 없다.

```kotlin
class A {
    companion object {
      fun bar() {
          println("bar!")
      }
    }
}
```
- companion이라는 키워드를 쓰면 동반 객체로 만들 수 있다.
- 동반 객체는 바깥쪽 private 멤버에도 접근할 수 있기에 동반 객체를 통해서 프로퍼티나, 메서드를 참조하면 된다.
```kotlin
class User {
    val nickname: String

  constructor(email: String) {
      nickname = email.substringBefore('@')
  }
  constructor(accountId: Int) {
    nickname = getFacebookName(accountId)
  }
}
// 위 코드를 아래처럼 고쳐서 쓸 수 있음
class User private constructor(val nickname: String) {
    companion object {
      fun newSubscribingUser(email: String) =
        User(email.substringBefore('@'))
      fun newFacebookUser(accountId: Int) =
        User(getFacebookName(accountId))
    }
}
User.newSubscribingUser("123@gmail.com")
User.newFacebookUser(4)
```

### 4.4.3 동반 객체를 일반 객체처럼 사용
```kotlin
class Person(val name: String) {
    companion object Loader {
      fun fromJSON(jsonText: String): Person = TODO()
    }
}
person = Person.Loader.fromJSON("{name: 'JSON'}")
```
- 동반 객체는 클래스 안에 정의된 일반 객체로 이름을 붙이거나 동반 객체가 인터페이스를 상속하거나,  
확장 함수와 프로퍼티를 정의할 수 있다.

<b>동반 객체에서 인터페이스 구현</b>
```kotlin
interface JSONFactory<T> {
    fun fromJSON(jsonText: String): T
}
class Person(val name: String) {
    companion object: JSONFactory<Person> {
      override fun fromJSON(jsonText: String): Person {
        TODO()
      }
    }
}
Person.fromJSON("...")
```
- 위와 같이 인터페이스를 구현하여 사용할 수 있다.

<b>동반 객체 확장</b>
```kotlin
class Person(val firstName: String, val lastName: String) {
    companion object {
        
    }
}
fun Person.Companion.fromJSON(json: String): Person {
    //...
}
```
- 위 코드처럼 동반 객체에 대한 확장 함수를 선언하여 사용할 수도 있다.

### 4.4.4 객체 식: 무명 내부 클래스를 다른 방식으로 작성

```kotlin
interface Move {
  fun left()
  fun right()
}
car.setMoveLogic(
  object : Move() {
    override fun left() {
      TODO("Not yet implemented")
    }
    override fun right() {
      TODO("Not yet implemented")
    }
  }
)
```
- object 키워드는 익명 객체를 정의할 때도 사용된다.
- 위 코드를 보면 매번 다른 인스턴스를 구현한다는 점에서 자바와 유사한 점을 찾을 수 있다.
- 코틀린은 익명 클래스는 자바와 달리 여러 인터페이스를 구현하거나 클래스를 확장하면서 인터페이스를 구현할 수 있다. 

# 5장 람다로 프로그래밍
## 5.1 람다 식과 멤버 참조
### 5.1.1 람다 소개: 코드 블록을 함수 인자로 넘기기
- 이전의 익명 클래스와 달리 함수형 프로그래밍을 통해 함수를 값처럼 다루게 되었다.
- 람다를 사용하면 익명 클래스와 다르게 간결하고 가독성 있는 코드를 작성할 수 있다.

### 5.1.2 람다와 컬렉션
```kotlin
fun findTheOldest(people: List<Person>) {
  var maxAge = 0
  var theOldest: Person? = null
  for (person in people) {
    if (person.age > maxAge) {
      maxAge = person.age
      theOldest = person
    }
  }
}
// 아래는 동일한 코드
person.maxBy { it.age }
people.maxBy(Person::age)
```
- 람다를 사용하면 위처럼 간략하게 작성할 수 있다.
- 또한 멤버 참조를 통해 더 간략하게 코드를 작성할 수도 있다.

### 5.1.3 람다 식의 문법
```kotlin
{x: Int, y: Int -> x + y }
val sum = {x: Int, y: Int -> x + y }
println(sum(1, 2))

run{ println(42) }
```
- 람다식은 기본적으로 위와 같이 작성하여 사용할 수 있다.
- run을 사용하여 인자로 받은 람다를 바로 실행시킬 수도 있따.

```kotlin
people.maxBy({ p: Person -> p.age })
//아래와 같이 개선
people.maxBy() {p: Person -> p.age }
//아래와 같이 개선
people.maxBy { p: Person -> p.age }

//etc) 다른 인자가 있고, 람다가 맨 마지막인 경우는 다음과 같이 호출 가능
people.maxBy("parameter") { p: Person -> p.age}
```
- 함수 호출 시 맨 뒤에 있는 인자가 람다 식이라면 그 람다를 괄호 밖으로 빼낼 수 있다는 관습이 있다.
- 또한 람다가 어떤 함수의 유일한 인자라면 빈 괄호를 없애도 된다.

```kotlin
people.maxBy { p: Person -> p.age } // 파라미터 타입 명시
people.maxBy { p -> p.age } // 파라미터 타입 생략(컴파일러가 추론)
people.maxBy { it.age } // 자동 생성된 파라미터 이름 사용
```
- 컴파일러는 람다 파라미터의 타입도 추론할 수 있다.
- 위 코드는 컬렉션에 Person 타입의 객체가 들어있다는 사실을 알기에 생략이 가능했다.
- 람다의 파라미터가 하나뿐이고 그 타입을 컴파일러가 추론할 수 있다면 it를 사용할 수 있다.
  - it를 사용하면 코드를 간단하게 작성할 수 있지만 람다끼리 중첩되어 있다면 가독성이 떨어지니 주의하자.

### 5.1.4 현재 영역에 있는 변수에 접근
- 자바 람다밖의 지역변수에 접근할 수 있지만 기본적으로 변경은 불가능하다(피할 방법이 존재), 그에 반해 코틀린은 둘 다 가능하다.
- 람다 안에서 사용하는 외부 변수를 `람다가 포획한 변수` 라고 부른다.
- 기본적으로 함수 안에 정의된 로컬 변수의 생명 주기는 함수가 반환되면 끝난다.
  - val 변수는 그 변수의 값이 복사되어 다룬다
  - var 변수는 Ref 클래스로 감싸서 읽기/변경 가능하게 한다.

### 5.1.5 멤버 참조
```kotlin
val getAge = { person: Person -> person.age }
// 아래와 같이 사용
val getAge = Person::age
```
- 멤버 참조는 프로퍼티나 메서드를 단 하나만 호출하는 함수 값을 만들어준다.

```kotlin
fun salute() = println("salute!")
run(::salute)
// -> salute!
```
- 위 코드처럼 최상위에 선언된 함수나 프로퍼티를 참조할 수도 있다.

```kotlin
data class Person(val name: String, val age: Int)
val createPerson = ::Person
val p = createPerson("Alice", 29)
```
- 생성자 참조를 사용하면 클래스 생성 작업을 연기하거나 저장해둘 수 있다.

```kotlin
fun Person.isAdult() = age >= 21
val predicate = Person::isAdult
```
- 확장 함수도 멤버 함수와 똑같은 방식으로 참조할 수 있다.

```kotlin
val p = Person("name", 34)
val personsAgeFunction = Person::age
println(personsAgeFunction(p))

val dmitrysAgeFunction = p::age // 코틀린 1.1 부터 사용할 수 있는 바운드 멤버 참조
println(dmitrysAgeFunction())
```
- 코틀린 1.0에서는 클래스나 메서드나 프로퍼티에 대한 참조를 얻은 다음에 그 참조를 호출할 때 인스턴스 객체를 제공해야 했다.
- 코틀린 1.1 부터는 바운드 멤버 참조를 통해 인스턴스를 함께 저장하여 그 인스턴스에 대한 멤버를 바로 호출할 수 있다.

## 5.2 컬렉션 함수형 API
### 5.2.1 필수적인 함수: filter와 map
```kotlin
val people = listOf(Person("Alice", 29), Person("Bob", 31))
println(people.filter { it.age > 30 })
// -> [Person(name=Bob, age=31)]
val list = listOf(1,2,3,4)
println(list.map { it * it})
// -> [1, 4, 9, 16]
```
- 기본적으로 자바와 동일하게 동작한다.

### 5.2.2 all, any, count, find: 컬렉션에 술어 적용
```kotlin
val canBeInClub27 = { p: Person -> p.age <= 27 }
val people = listOf(Person(age = 27), Person(age = 3))
println(people.all(canBeInClub27))
//true
println(people.any(canBeInClub27))
//true
println(people.count(canBeInClub27))
// 2
println(people.find(canBeInClub27))
// Person(name=Alice, age=27)
```
- all() 메서드는 모두가 조건에 부합하는지, any()는 하나라도 부합하는지 체크한다.  
- count() 조건에 부합하는 원소의 개수를 구하고, find()는 가장 먼저 조건에 만족하는 원소를 반환한다. 

### 5.2.3 groupBy: 리스트를 여러 그룹으로 이뤄진 맵으로 변경
```kotlin
val people = listOf(Person("Alice", 31), Person("Bob", 29), Person("Carol", 31))
println(people.groupBy { it.age })
```
- 위 결과 타입은 Map<Int, List<Person>> 이다. 그리하여 위 결과는 두 개의 리스트로 쪼개진다.

### 5.2.4 flatMap과 flatten: 중첩된 컬렉션 안의 원소 처리
```kotlin
class Book(val title: String, val author: List<String>)

val books = listOf(
  Book("Thuresday Next", listOf("name1", "name2")),
  Book("Monday Next", listOf("name3", "name4")),
  Book("Saturday Next", listOf("name5", "name6")),
)
println(people.flatMap { it.authors } .toSet())
```
- 위 예제를 토대로 flatMap 함수는 모든 책의 작가를 평평한 리스트 하나로 모은다.
- 그리고 toSet()을 통해 중복을 없애고 집합으로 만든다.

## 5.3 지연 계산(lazy) 컬렉션 연산

```kotlin
people.asSequence()
  .map(Person::name)
  .filter { it.startsWith("A") }
  .toList()
```
- map과 filter 같은 함수를 연쇄하면 매 단계마다 임시 컬렉션이 생성된다.
- 시퀀스를 사용해서 임시 컬렉션 만드는 것을 막을 수 있다.
- 시퀀스를 반환해서 사용해도 되지만 인덱스로 접근하는 등 다른 API 메서드가 필요하면 리스트로 반환하는 것을 권장한다.

### 5.3.1 시퀀스 연산 실행: 중간 연산과 최종 연산
```kotlin
people.asSequence()
  .map(Person::name) // 중간 연산
  .filter { it.startsWith("A") } // 중간 연산
  .toList() // 최종 연산
```
- 시퀀스를 사용하면 자바처럼 중간연산, 최종연산이 생기는데 이때 최종연산이 존재해야 중간연산이 수행될 수 있다.

### 5.3.2 시퀀스 만들기
```kotlin
fun SequenceTest() {
  val naturalNumbers = generateSequence(0) { it + 1 }
  val numbersTo100 = naturalNumbers.takeWhile { it <= 100 }
  println(numbersTo100.sum())
}
```
- 시퀀스는 generateSequence()와 같이 만들 수 있다.
- 위 로직은 최종 연산인 sum() 연산이 수행되기 전까지 중간연산이 수행되지 않는다.

## 5.4 자바 함수형 인터페이스 활용
### 5.4.1 자바 메서드에 람다를 인자로 전달
````kotlin
Gogo.postponeComputation(1000) { println(42)}
````
- 자바 메서드에 코틀린 람다를 전달할 수 있다.
- 익명 클래스는 매번 생성되기 때문에 익명 클래스 넘기기보단 람다를 넘기는 것이 효율적이다.
- 다만 람다가 주변 영역의 변수를 사용한다면 같은 인스턴스를 사용할 수 없다.

### 5.4.2 SAM 생성자
```kotlin
fun createAllDoneRunnable(): Runnable {
        return Runnable { println("All done!") }
    }
    fun execute() {
        createAllDoneRunnable().run()
    }
```
- SAM 생성자는 람다를 함수형 인터페이스의 인스턴스로 변환할 수 있게 컴파일러가 자동으로 생성한 함수다.
- 컴파일러가 자동으로 람다를 함수형 인터페이스 익명 클래스로 바꾸지 못하는 경우 사용할 수 있다.
- 위 코드처럼 함수형 인터페이스의 인스턴스를 반환하는 메서드가 있다면 람다를 직접 반환할 수 없고,  
반환할려는 람다를 SAM 생성자로 감싸야 한다.

## 5.5 수신 객체 지정 람다: with와 apply
### 5.5.1 with 함수
```kotlin
fun alphabet(): String {
  val StringBuilder = StringBuilder()
  return with(stringBuilder) { // 수신 객체 지정
    for (letter in 'A'..'Z') { //this를 명시해서 앞으로 지정한 수신 객체의 메서드를 호출
        this.append(letter)
    }
    append("I know the alphabet!") // this를 생략하고 메서드를 호출한다.
    this.toString() // 람다에서 값을 반환한다.
  }
}
```
- 코틀린의 with 함수를 통해서 수신 객체를 지정하여 수신객체의 이름을 반복하지 않고도 다양한 연산을 수행할 수 있다.
- with은 특별한 구문이 아닌 함수로 파라미터를 (사용 객체, 람다) 두개 받는 함수이다.
- this와 점(.)을 사용하지 않고 프로퍼티니 메서드 이름만 사용해도 수신 객체에 접근할 수 있다.

```kotlin
fun alphabet() = with(StringBuilder()) {
  for (letter in 'A'..'Z') {
      append(letter)
  }
  append("I know the alphabet!")
  toString()
}
```
- 위처럼 stringBuilder 인스턴스를 with 파라미터에 바로 선언하여 간략하게 작성할 수 있다.
- 만약에 메서드가 같은 경우에는 this@클래스명.메서드()처럼 어떤 메서드를 참조하는지 확실히 할 수 있다.

### 5.2.2 apply 함수
```kotlin
fun alphabet () = StringBuilder().apply {
  for (letter in 'A'..'Z') {
      append(letter)
  }
  append("I know the alphabet !")
}.toString()
```
- apply() 함수는 with()와 다르게 항상 자신에게 전달된 객체 즉 수신 객체를 반환한다는 점이다.
- 위 코드는 StringBuilder가 수신 객체이고 반환받아 toString() 메서드를 호출하게 된다.

# 6장 코틀린 타입 시스템
## 6.1 널 가능성
###6.1.1 널이 될 수 있는 타입
```kotlin
fun strLenSafe(s: String?): Int = 
    if (s != null) s.length else 0
```
- 어떤 타입이든 상관없이 타입 이름 뒤에 물음표를 붙이면 그 타입의 변수나 프로퍼티에 null 참조를 저장할 수 있다.
- 고로 물음표가 없는 타입은 null 참조를 허용하지 않는다.



### 6.1.3 안전한 호출 연산자: ?.
```kotlin
fun printAllCaps(s: String?) {
  val allCaps: String? = s?.uppercase()
}
```
- 코틀린의 ?. 연산자는 null 검사와 메서드 호출을 한 번의 연산으로 수행한다.

### 6.1.4 엘비스 연산자: ?:
```kotlin
fun foo(s: String?) {
    val t: String = s ?: ""
}
```
- null 대신 사용할 디폴트 값을 지정할 때 엘비스 연산자(?:)를 사용할 수 있다.
- 엘비스 연산자 우항에는 return, throw 등의 연산을 넣을 수 있다.

### 6.1.5 안전한 캐스트: as?
```kotlin
class Person(val firstName: String, val lastName: String) {
    override fun equals(o: Any?): Boolean {
      val otherPerson = o as? Person ?: return false
      return otherPerson.firstName == firstName && otherPerson.lastName == lastName
    }
}
```
- as? 연산자는 어떤 값을 지정한 타입으로 변환할 수 없다면 null을 반환한다.
- 위 처럼 as? 연산자에 엘비스 연산자를 사용하는 패턴이 일반적이다.

### 6.1.6 널 아닌 단언: !!
```kotlin
fun ignoreNull(s: String?) {
  val sNotNull: String = s!!
  println(sNotNull)
}
```
- !!은 널이 무조건적으로 아님을 표시하기 위해 사용한다.
- 대게 컴파일러가 null 체크를 해주지만 중복적으로 null을 검사하는 경우에 사용하게 된다.
- 어떤 값이 null이었는지 확실히 하기 위해 여러 !! 단언문을 한 줄에 사용하는 일을 지양해라.
  - `person.company!!.address!!.country`

### 6.1.7 let 함수
```kotlin
fun sendEmailTo(email: String) {
    println("Sending email to $email")
}
var email: String? = "12@exam.com"
email?.let {sendEmailTo(it) }
// -> Sending email to 12@exam.com
email = null
email?.let { sendEmailTo(it) }
```
- let은 널인지 검사한 다음에 그 결과를 변수에 넣는 작업을 간단한 식을 사용해 처리할 수 있다.
- 그리하여 위처럼 Null이 아니면 Print 구문을 수행하고 아니면 수행하지 않게 작성할 수 있다.

### 6.1.8 나중에 초기화할 프로퍼티
```kotlin
class MyService {
    fun performAction() : String = "foo"
}
class MyTest {
    private lateinit var myService: MyService

  @Before
  fun setUp() {
      myService = MyService()
  }

  @Test
  fun testAction() {
      Assert.assertEquals("foo", myService.performAction())
  }
}
```
- 기본적으로 생성자에서 프로퍼티는 모두 초기화해야 한다.
  - 그렇기에 프로퍼티가 Null을 허용하지 않으면 Null이 아닌 것으로 초기화해야 한다.
  - Null을 허용한다면 모든 프로퍼티 접근에 널 검사, !! 연산자가 필요해지기에 가독성이 매우 떨어지고 불편해진다.
- 인스턴스를 만들고 난 후 나중에 초기화를 할때는 lateinit 변경자를 사용하면 된다.
- val 프로퍼티는 final 필드로 컴파일 되며, 생성자 안에서 반드시 초기화되어야 하기 때문에   
나중에 초기화하는 프로퍼티는 var이어야 한다.

### 6.1.9 널이 될 수 있는 타입 확장
```kotlin
fun verifyUserInput(input: String?) {
  if (input.isNullOrBlank()) {
    println("Null or Blank")
  }
}
```
- 확장함수를 통해 Null에 대한 대비를 할 수 있다.

### 6.1.10 타입 파라미터의 널 가능성
```kotlin
fun <T> printHashCode(t: T) {
    println(t?.hashCode())
}
```
- 위 처럼 타입 파라미터를 써도 파라미터는 null일 수 있다.
- 그렇기에 위 상황에서는 안전한 호출(?.)을 써야 한다.
```kotlin
fun <T: Any> printHashCode(t: T) {
    println(t?.hashCode())
}
printlnHashCode(null)
// -> 컴파일 오류
```
- 이처럼 타입 상한을 지정하면 null이 될 수 없도록 개선할 수 있다.
- 실제로 null을 파라미터로 호출하면 위 코드는 컴파일 에러가 발생한다.

### 6.1.11 널 가능성과 자바

- 자바에는 널 가능성을 지원하지 않는다.
  - 널 가능성에 대한 정보는 자바의 @Nullable, @NotNull과 같은 어노테이션을 통해 코틀린이 확인할 수 있다.
- 만약 널 관련 정보가 없다면 자바의 타입은 코틀린의 플랫폼 타입이 된다.
  - 플랫폼 타입은 널이 될 수 있거나 널이 될 수 없는 타입 모두로 사용할 수 있다.
- 코틀린 컴파일러는 공개(public) 가시성인 코틀린 함수에 있는 널이 아닌 타입인 파라미터와 수신 객체에 대한 널 검사를 추가해준다.

## 6.2 코틀린의 원시 타입
### 6.2.1 원시타입: Int, Boolean 등
```kotlin
fun showProgress(progress: Int) {
  val percent = progress.coerceIn(0, 100)
  println(percent)
}
```
- 코틀린은 자바와 다르게 원시타입과, 래퍼 타입을 구분하지 않고 하나로 사용한다.
- 위처럼 Int 타입에서 메서드를 호출할 수 있다.
- 코틀린이 그렇다고 모든걸 참조 타입으로 표현하지는 않는다.
  - 대부분의 경우는 자바의 int 타입으로 컴파일 된다.
  - 만약 컬렉션이나, 제네릭과 같이 이런 컴파일이 불가능한 상황이라면 Integer 객체가 된다.

### 6.2.2 널이 될 수 있는 원시 타입: Int?, Boolean? 등
```kotlin
data class Person(val name: String, val age: Int? = null) {
    fun isOlderThan(other: Person): Boolean? {
      if (age == null || other.age == null) {
        return null
      }
      return age > other.age
    }
}
```
- 자바의 기본 타입은 당연하게도 null이 들어갈 수 없기에 코틀린의 널이 될 수 있는 타입은 래퍼 타입이 된다.
- 위 코드처럼 코틀린에서 컴파일러는 기본적으로 널 검사를 마친 다음에야 일반적인 값처럼 다루게 허용한다.

### 6.2.3 숫자 변환
```kotlin
val i = 1
val l: Long = i.toLong()
```
- 코틀린은 자동 변환은 불가능하다.
  - 그렇기에 코틀린은 모든 원시 타입에 대한 변환 함수를 제공한다.
  
### 6.2.4 Any, Any?: 최상위 타입

- 자바에서 Object가 클래스 계층의 최상위 타입이듯 코틀린에서는  
Any 타입이 모든 널이 될 수 없는 타입의 조상 타입이다.
- 하지만 자바의 Object는 참조 클래스만 조상이지만,  
Any 타입은 Int 등의 원시타입도 포함하여 조상 타입이 된다.

### 6.2.5 Unit 타입: 코틀린의 void
```kotlin
fun f() : Unit {}
```
- 코틀린의 Unit 타입은 자바의 void와 같은 기능을 한다.
- Unit은 void와 다르게 모든 기능을 가질 수 있는 일반적인 타입이고, 타입 인자로 사용할 수 있다.

```kotlin
interface Processor<T> {
  fun process(): T
}
class NoResultProcessor : Processor<Unit> {
  override fun process() {
    TODO()
  }
}
```
- 인터페이스를 구현하면서 Unit 타입을 반환하도록 명시해서 작성해도 되지만 컴파일러가 묵시적으로 return Unit을 작성해준다.
- 자바에서 Callable, Runnable과 별도로 인터페이스를 분리하는 방법을 코틀린은 Unit으로 해결했다고 볼 수 있다.

### 6.2.6 Nothing 타입: 이 함수는 결코 정상적으로 끝나지 않는다.
```kotlin
fun fail(message: String): Nothing {
    throw IllegalStateException(message)
}
```
- Nothing 타입은 함수가 정상적으로 끝나지 않을 경우를 표현하기 위해 사용하는 타입이다.
- Nothing 타입은 아무 값도 포함하지 않는다. 따라서 반환 타입으로 사용하는 것을 권장한다.

## 6.3 컬렉션과 배열
### 6.3.1 널 가능성과 컬렉션
```kotlin
fun test() {
  val result = ArrayList<Int?>()
}
```
- 위 List는 원소가 Null이 들어올 수 있다는 얘기이다.
- List 자체가 Null인 경우와 구분해서 사용해야 한다.

### 6.3.2 읽기 전용과 변경 가능한 컬렉션

- 코틀린의 Collection은 원소를 추가하거나, 제거하는 메서드가 없다.
- MutableCollection은 Collection 인터페이스를 확장하여 원소를 추가, 삭제 등의 메서드를 추가로 제공한다.
- 컬렉션은 기본적으로 읽기 전용으로만 제공하라.

```kotlin
fun test() {
  val list: Collection<Int> = listOf(3, 5, 7)
  val mutableList: MutableCollection<Int> = arrayListOf(1)
  listOf(list, mutableList)
}
```
- 컬렉션의 요소들이 각자 다른 객체의 참조를 얻고 있는 상황도 있을 것이다.
  - 그렇기에 읽기 전용 클래스가 항상 변경 불가능하다는 것은 아니다.
  - 또한 병렬 실행된다면 읽기 전용 컬렉션이 항상 Thread-safe하지 않기에 주의해서 사용해야 한다.

### 6.3.3 코틀린 컬렉션과 자바

- 모든 코틀린 컬렉션은 그에 상응하는 자바 컬렉션 인터페이스의 인스턴스이다.
- 실제로 자바의 ArrayList가 코틀린의 MutableList를 확장한 구조를 가진다.
- 자바에는 읽기 전용, 변경 가능 컬렉션을 구분하지 않으므로 코틀린의 읽기전용 컬렉션을 자바의 메서드에 넘겨도 이를 막을 수 없다.
  - 그렇기에 개발자가 이 부분을 책임지고 구현해야 한다.

### 6.3.4 컬렉션을 플랫폼 타입으로 다루기


### 6.3.5 객체의 배열과 원시 타입의 배열
```kotlin
fun main(args: Array<String>) {
  for (i in args.indices) {
      println("$i is : ${args[i]}")
  }
}
```
- 코틀린 배열은 타입 파라미터를 받는 클래스다.
- 코틀린에서 원시 타입 배열을, IntArray, CharArray 등과 같이 제공한다.
  - 이 모든 원시 타입 배열은 자바의 int[], char[]와 같이 컴파일 된다.

```kotlin
fun main(args: Array<String>) {
  args.forEachIndexed { index, element ->
      println("$index is : $element")
  }
}
```
- 코틀린은 배열에 기본 연산 이외에도 모든 확장함수를 제공한다.
  - 또한 원시 타입 배열에도 filter,map을 사용할 수 있다.


