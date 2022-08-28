# 02장 코틀린 개발환경

# 2.1 코틀린 개발환경 - IntelliJ
IntelliJ나 Android Studio 사용이 일반적. 

덧. Android Studio에서는 일반 Java/Kotlin 프로젝트 생성/열기 불가

JVM 기반에서 동작하는 프로그램을 작성하려면 JDK 필요. 코틀린은 JDK 1.6 버전 이상에서 동작. 

IntelliJ의 gradle 버전에서 지원하는 JDK 버전도 고려 필요

덧. 코틀린에서도 main()은 최상위 함수로 실행 진입점이다
```kotlin
fun main(args: Array<String> {
  println("Hello World!")
}
```

코틀린에서 main()함수는 실행 진입점입니다. 자바와 같은 객체 지향 언어에서는 프로그램을 실행하려면 최소한 하나의 클래스와 그 안에 실행할 수 있는main()함수가 있어야 합니다. 하지만 코틀린은 선언된 클래스가 없는데도 불구하고 main()메서드 하나로 println()함수를 통해 콘솔에 문자열 "Hello Kotlin"을 출력 하고 있습니다.

실제로 코틀린 코드는 JVM상에서 실행하기 위해서main 메서드는 파일명을 기준으로 자동으로 클래스가 생성됩니다. 만들어진 파일은 IntelliJ IDEA의 메뉴에 [Tools → Kotlin → Show Kotlin Bytecode]를 누른 후 생성된 화면에서 [Decompile]을 사용해서 어떤 형태로 소스가 해석 되었는지를 확인해 볼 수 있습니다.

출처 : https://m.boostcourse.org/mo132/lecture/59966?isDesc=false

**빌드도구를 사용하지 않았을 경우의 외부 라이브러리 이용**
IntelliJ에서 Gradle 같은 빌드도구를 사용하지 않고 프로젝트를 만들었을 경우
1. 프로젝트 폴더 아래 디렉토리 생성(폴더명을 lib로 예를 들면)
2. 외부 라이브러리를 다운받아 lib 폴더에 넣기
3. [File - Project Structure] 메뉴 선택
4. 좌측 탭 중 Dependencies 선택 - '+' 버튼을 눌러 [JARs or directories] 선택 - lib 안에 넣어둔 .jar 파일 선택

**빌드도구를 이용한 개발환경**
IntelliJ에서 Gradle을 이용한 개발환경 구축 방법

빌드도구는 개발자가 만든 코드를 컴파일하고 패키징하는 일련의 과정 수행. 또한 중요한 역할 중 하나가 라이브러리 의존성 표현. 라이브러리들을 빌드도구 환경파일에 명시하면 해당 라이브러리를 다운로드하고 의존성 설정이 자동으로 완료.

# 2.2 코틀린 개발환경 - Android Studio
(생략)

코틀린 파일은 코틀린 플러그인이 별도로 컴파일하여 적용한다. 그러려면 그레이들 파일에 코틀린 플러그인을 등록해야 한다. 프로젝트 생성 시 include kotlin support 옵션을 선택하면 gradle 파일에 자동으로 설정된다.(plugin 부분을 보면 있다) 선택하지 않았다면 수동으로 gradle 파일들(프로젝트와 모듈)에 추가해줄 수 있다.

# 2.3 코틀린 개발환경 - Eclipse
# 2.4 코틀린 개발환경 - CLI
