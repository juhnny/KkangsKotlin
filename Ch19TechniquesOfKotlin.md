# 19. 코틀린의 다양한 기법

# 19.1 델리게이션

코틀린은 Delegation(위임) 디자인 패턴에 맞는 프로그램을 작성하기 위한 유용한 기법들을 제공

## 19.1.1 델리게이션 클래스

위임 패턴은 위임자(delegator)가 해야 할 작업을 대행자(delegatee)에게 맡기는 것. 

예를 들어, 데이터 저장 업무를 해야 하는 위임자가 있다. 
데이터 저장을 하나의 방식으로만 하는 것이 아니라 상황에 따라 파일, 데이터베이스, 네트워크 등에 저장할 수 있다고 가정하면, 
위임자는 각각의 데이터 저장 로직을 구현한 대행자(파일에 저장하는 대행자, DB에 저장하는 대행자, 네트워크에 저장하는 대행자)들을 만들어 이들을 이용해 일을 처리하는 구조.

```kotlin
class MyDelegatee {
  // 로직을 모두 이쪽에 몰아서 구현
  fun show(str:String){
    println("I'm delegatee : $str")
  }
}

class MyDelegator {
  val delegatee = MyDelegatee()
  
  fun show(str:String){
    delegatee.show(str)
  }
}

fun main(args: Array<String>) {
  val delegator = MyDelegator()
  delegator.show("Hello world")
}
```

    I'm delegatee : Hello world

'음.. 그냥 API 이용하는 느낌인데? 어차피 우리는 항상 Java system api라는 누가 이미 구현해놓은 클래스의 기능들을 쓰는 거니까..'

'아, 하지만 위임 패턴에서는 대행자 쪽에 모든 로직을 몰아주고 위임자 쪽에서는 호출만 하는 정도인 게 차이구나!'

### by를 이용한 위임 패턴 구현

by를 이용하면 인터페이스의 구현을 같은 인터페이스를 구현한 다른 객체로 대신할 수 있음

```kotlin
interface Print {
  fun show(str: String)
}

class MyDelegator : Print {
  override fun show(str : String) {
    println("I'm delegatee : $str")
  }
}

class MyDelegator(obj : MyDelegatee) : Print by obj

fun main(args : Array<String>) {
  val delegatee = MyDelegatee()
  MyDelegator(delegatee).show("Hello world")
}
``` 

인터페이스를 사용하긴 했지만 대행자 부분(주요 로직 구현)은 이전 예제와 차이가 없다. 

이를 이용하는 위임자의 구현과 그 메소드 호출 시 차이가 있다.
* MyDelegator 선언 시 중괄호 영역이 없다.
* 인터페이스의 추상 함수를 재정의하지 않았지만 에러가 아님
* MyDelegator에서 MyDelegatee의 show()를 이용한 show()를 구현하지 않았음에도 MyDelegatee의 show()가 호출됨

인터페이스의 구현(추상메소드의 재정의override)을 by 오른쪽에 전달된 객체로 대신하게 되는 것. 즉, 위임자의 구현을 'by 대행자'로 대신할 수 있는 것

이를 위해서는 **위임자와 대행자가 같은 Interface를 구현하고 있어야 함**

'X라는 업무를 할 줄 아는 A와 B가 있을 때, A의 업무를 B가 대신하게 한다는 느낌'

한 위임자가 여러 대행자를 동시에 이용할 때는 어떡하지?
  
  
### 위임 패턴과 Gof 디자인 패턴 중 어댑터 패턴과의 차이점
두 패턴은 구조적으로 매우 동일. 목적에 자체에 차이가 있음. 

어댑터 패턴은 다른 여러가지(?)를 같은 API로 사용하려는 게 주목적. 위임 패턴은 원래 본인이 수행해야 할 업무를 다른 곳에 위임하기 위한 것이 주목적.

그래서 일반적으로 위임 패턴은 위임자와 대행자가 같은 API를 가진다. 위임자가 처리할 내용을 대행자에게 맡기는 개념이므로 둘의 API를 동일하게 작성하는 게 일반적. 그러나 어댑터 패턴은 어댑터의 목적 자체가 무언가를 구현할 목적이 아니라 다른 여러 가지를 동일하게 이용하기 위한 목적이므로 Adapter와 Adaptee(작업자)의 API가 다른 경우가 많다. 

## 19.1.2 델리게이션 프로퍼티
by를 이용해 클래스 위임을 대행하는 것과 같은 개념으로 프로퍼티 위임도 제공됨.
프로퍼티의 get(), set() 함수에 들어가는 로직이 여러 프로퍼티에 중복된다면, 매번 구현하는 대신 중복되는 로직을 구현하는 클래스를 만들어서 처리를 위임하는 개념 

역시 'by 대행객체' 문법을 이용한다.

### 프로퍼티 대행 클래스 작성 규칙
getValue(), setValue() 함수를 포함해야 한다. 프로퍼티를 이용할 때 이 함수들이 자동 호출되며 프로퍼티의 get(), set() 함수에 매핑된다. 즉, get()을 호출하면 getValue()가, set()을 호출하면 setValue()가 호출된다.
getValue(), setValue() 함수는 operator 키워드를 추가해야 한다.

```kotlin
class MyPropDelegatee {
  var prop : Int = 0
  
  operator fun getValue(thisRef: Any?, property: KProperty<*>): Int {
  // 두번째 매개변수를 이용해 적용된 프로퍼티의 이름을 얻을 수 있다.(setValue()와 공통)
    println("getValue call... ref : $thisRef, property : '${property.name}'")
    return prop
  }

  operator fun setValue(thisRef: Any?, property: KProperty<*>, value: Int) {
  // 프로퍼티에 대입된 값은 setValue() 함수에 세 번째 매개변수로 전달된다.
    println("setValue call... property : '${property.name}', value : Int")
    // 필요한 작업
    // ...
    //
    val = value
  }
}

class Test {
  var a : Int by MyPropDelegatee() 
}

fun main(args: Array<String>) {
  val obj = Test()
  obj.a = 10
  println("a : ${obj.a}")
}
```

# 19.2. SAM 전환

SAM(Single Abstract Method)는 자바 API를 코틀린에서 이용할 때 lambda를 이용해 쉽게 작성할 수 있게 해주는 기법. 하나의 추상 함수를 갖는 인터페이스의 활용을 목적으로 함.

## 19.2.1. 자바에서의 인터페이스 활용

Interface의 주목적은 주목적은 인터페이스를 구현하는 곳에서 추상 함수를 재정의하도록 강제하기 위함. 

```java
// 자바에서 인터페이스 선언
// onClickListner를 상상해보자
public interface MyInterface{
  fun callback();
}

// 위 인터페이스를 사용하는 클래스
// onClickListener 등록이 가능한 View를 상상해보자
// 인터페이스 구현 객체를 등록하기 위한 setter 함수 정의
public class Test {
  MyInterface myInterface;
  
  public void setMyInterface(MyInterface myInterface) {
    this.myInterface = myInterface;
  }
  // 참고로 코틀린에서는 MyInterface에 대해 자동으로 
  setMyInterface, getMyInterface라는 이름으로 setter, getter 메소드가 만들어지므로 
  개발자가 setMyInterface라는 이름의 메소드를 만들면 이름이 같아서 충돌함. 
  그냥 test.myInterface = MyInterface{ //내용 } 같은 식으로 써야 하는 듯.
}
```

이를 Java에서 사용할 때의 모습

```java
// setter 함수를 통해 인터페이스 구현체 등록
// View에 onClickListener를 등록하는 모습과 같다.
Test test = new Test()
test.setMyInterface(new MyInterface(){
  @override
  public void callback(){
    system.out.println("Hello Java");
  }
});
```

이를 Kotlin에서 사용하는 모습. 익명 클래스를 만들어 이용

```kotlin
val test = Test()
test.setMyInterface(object : MyInterface{
  override fun callback() {
    println("Hello Kotlin")
  }
})
test.myInterface.callback()
```

SAM 전환을 이용하면 익명 클래스 대신 lambda로 더 짧게 만들 수 있다.

```kotlin
val test = Test()
test.setMyInterface{ println("Hello Kotlin") }
test.myInterface.callback()
```

람다만 전달하더라도 컴파일러가 MyInterface를 구현한 익명 클래스 객체를 만들어 줌

setMyInterface()는 MyInterface를 구현한 객체를 매개변수로 받는다고 명시적으로 선언한 함수이고, MyInterface 안에는 구현할 추상 함수가 하나 뿐이므로 컴파일러가 명확히 인식할 수 있다.

# 19.3. 타입 에일리어스 type alias

클래스나 인터페이스의 타입 이름을 변경해 사용할 수 있다.

> typealias 새 이름 = 원래 이름

```kotlin
// typealias로 타입 별명 지정
typealias MyInt = Int
typealias MList<T> = MutableList<T>
typealias MI = MyInterface
typealias MC = MyClass

interface MyInterface // 왜 MI를 안 쓰고? 기존 이름도 여전히 쓸 수 있다는 걸 보여주려고? 그리고 이게 도대체 무슨 문법이지? 
class MyClass : MI // 여기도 MC를 안 썼네. 그리고 왜 body 영역이 없지?

fun main(args: Array<String>) {
  val no: MyInt = 10
  val list: MList<String> = mutableListOf("Hello", "Kotlin")
  val mc: MC = MC()
}
```

함수 타입에도 사용이 가능하다.

```kotlin
typealias MyType = (Int) -> Boolean
val myFunction: MyType = {it > 10} // 이게 무슨 식이지? 그리고 리턴타입은 Boolean이어야 하는데?
```

inner class를 반복해 사용할 때 typealias를 사용할 수 있다. (덧. 다른 개발자에게 혼돈을 줄 수 있을지도 모르겠다)

```kotlin
class Super {
  inner class Sub // 이렇게 body 없이도 정의가 가능했던가?
  
  fun getSubInstance: MySub { // MySub으로 재정의하기 전에 여기서 벌써 MySub을 쓸 수 있다고?
    return Sub()
  }
}

typealias MySub = Super.Sub
```

