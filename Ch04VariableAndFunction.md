# 04 변수와 함수

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
* 단일표현함수는 반환 타입도 생략 가능. 

## 4.2.2 함수 오버로딩
