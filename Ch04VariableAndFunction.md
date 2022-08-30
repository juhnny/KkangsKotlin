# 04 변수와 함수
# 4.2 함수 사용법
## 4.2.1 함수 선언
* fun 키워드 사용
> fun 함수명(매개변수명 : 타입) : 반환타입 { }
```kotlin
fun sum(a:Int, b:Int): Int{
    return a + b
}
```
* 매개변수는 무조건 val로 적용
* 매개변수에 var, val 선언 불가
* 함수의 반환값이 없을 땐 Unit으로 명시
* Unit일 경우엔 생략 가능
```kotlin
fun sum(text : String){
    println(text)
}
```
* 함수의 반환타입 생략 시 기본적으로 Unit 적용
* 함수 내에 함수 정의 가능
그 함수의 지역변수 비슷한 개념으로 이용됨. 외부에서 호출 불가. 
* 단일표현함수(Single expression function. 말 그대로 한 줄짜리 함수)에서는 중괄호 생략 후 등호 = 사용 가능. 
* 단일표현함수는 반환 타입도 생략 가능. 컴파일러가 유추하여 지정함.
```kotlin
fun sum(a: Int, b: Int) = a + b
```

## 4.2.2 함수 오버로딩
같은 이름의 함수를 매개변수의 타입이나 개수를 다르게 하여 여러 개 정의하는 기법. 함수는 이름뿐 아니라 매개변수까지 판단하여 호출됨.

## 4.2.3 기본 인수와 명명된 인수
함수를 호출하면서 전달하는 값을 인수(argument), 이 인수 값을 전달받는 함수의 변수를 매개변수(parameter)라고 함.
호출 시 인수를 전달하지 않아도 기본으로 적용되는 값이 기본 인수(default argument)
```kotlin
fun some(name : String = "Hong"){
    println("Hello $name")
}
```

파라미터가 여럿이고 일부는 기본값이 주어져 있을 때 어느 파라미터에 대한 인수인지 명시적으로 지정한 것이 명명된 인수(named argument)
```kotlin
fun some(name: String = "Kang", no: Int){
    println("Hello $name")
}

some("Lee", 20)
some(no = 10)
some(name = "Lee", no = 10)
some(15) // 에러. 어느 매개변수에 대입해야 할 지 불명확
```

## 4.2.4 중위 표현식 infix

(추가)

### 자바 vs 코틀린
#### 코틀린에서는 const로 최상위 레벨에 상수변수를 선언한다

자바는 상수변수를 선언할 때 public static final int CONST_VAL = 10; 처럼 선언. static이 추가되어 클래스의 객체 멤버에 포함되지 않게 되며, final로 선언하여 값 변경 방지. 

코틀린에서 같은 코드는 const val CONST_VAL = 10이다. 최상위 레벨로 선언되어 객체 멤버에 포함되지 않으며, const로 선언되어 상수변수가 됨. 또한 코틀린에는 static 예약어가 없음. 

위에서 최상위 레벨이란 무엇?


#### 코틀린의 final은 자바의 final과 다르다

자바에서는 상수변수를 정의할 때 final 키워드 이용. 코틀린에서도 final 키워드를 제공한다. 하지만 final을 변수에 붙여봐야 값 변경이 가능하다. final이 클래스의 상속과 관련이 있다는 점은 같지만, 코틀린에서 상수변수를 만들기 위해 final을 사용하지 않는다.

final var 변수를 만들어봐야 값 수정이 가능하다. 변경할 수 없는 변수를 정의할 때는 val 키워드를 사용한다. 

final은 클래스 다룰 때 다시 다룬다고 함


#### 코틀린은 변수값이 자동으로 초기화되지 않는다

자바에서 클래스의 멤버 변수는 처음에 값을 선언하지 않아도 기본값으로 자동 초기화된다.
```java
public class Test{
    String name;
    int age;
    boolean isVisible;
    
    public static void main(String[] args){
        Test test = new Test();
        system.out.println(test.name + "/" + test.age + "/" + test.isVisible)
    }
}
```
     null/0/false  

객체는 null, 숫자 타입은 0, Boolean은 false로 초기화됨

하지만 코틀린에서는 초기화되지 않음. 초기값은 없더라도 lateinit으로 나중에 명시적으로 값을 대입하여 사용하거나, ?를 사용하여 nullable 타입으로 선언하여 null 값으로 초기화하여 사용해야 함.


#### 코틀린에서는 자바에서 제공하지 않는 함수의 다양한 이용방법을 제공한다

함수 내에 함수를 정의하거나 함수를 중위 표현식으로 이용하거나 기본 인수 등을 제공. 코틀린이 자바에 비해 편한 부분이라고.  


#### 추가할 부분
중위표현식 이후 부분

class에서의 final 사용에 대한 부분 

const val과 그냥 val의 차이점?


