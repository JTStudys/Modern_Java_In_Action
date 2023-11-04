# 3. 람다 표현식

이전 까지 동작 파라미터화를 이용해서 변화하는 client의 요구사항에 효과적으로 대응하는 코드를 구현 및 정의한 코드 블록을 다른 메서드로 전달할 수 있다는 것을 알게 되었다.
anonymous class 로 다양한 동작을 구현할 수 있지만 코드가 아직까지 불편함을 느끼게 만들었다.
이를 개선한 자바8의 람다 표현 식을 3장에서 설명한다.
**전반적인 내용을 다루는 해당 3장이 제일 중요하다.
따라서, 간추리지 않고 책의 내용을 거의 인용 했다고 봐도 무방하다.**

### 람다의 표현식

익명 클래스처럼 이름이 없는 함수이면서 메서드를 인수로 전달할 수 있다. → 익명 클래스와 유사하다.
람다 표현식과 함께 위력을 발휘하는 새로운 기능인 **메서드 참조**를 확인해볼 것이다.

```java
(Integer i) -> return "a" +i;
(String s) -> {"Iron Man";}
```

# 3.1 람다란 무엇인가?

**람다표현식** : 메서드로 전달할 수 있는 익명 함수를 단순화한 것이다.
람다 표현식에는 이름은 없지만, 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트는 가질 수 있다.

### 람다의 특징

1. **익명**
보통의 메서드와 달리 이름이 없으므로 익명이라 표현한다.
2. **함수**
람다는 메서드처럼 특정 클래스에 종속되지 않으므로 함수라고 부른다.
But 메서드처럼 파라미터 리스트, 바디, 반환 형식, 발생할 수 있는 예외 리스트를 포함한다.
3. **전달**
람다 표현식을 메서드 인수로 전달하거나 변수로 저장할 수 있다.
4. **간결성**
익명 클래스처럼 많은 필요없는 코드를 구현할 필요가 없다.

### 람다가 왜 필요할까 ?

→ 2장에서 확인한 것처럼 코드를 전달하는 과정에서 자질구레한 코드가 많이 생긴다. 이를 람다로 해결할수 있다.
즉, 람다를 이용해서 간결한 방식으로 코드를 전달할 수 있게된 것이다.
또한 동작 파라미터를 이용할 때 익명클래스 등 정해진 판에 박힌 코드를 구현할 필요가 없어졌다.
예시로 알아보자.

```java
// 기존
Comparator<Apple> byWeight = new Comparator<Apple>(){
	public int compare(Apple a1, Apple a2){
		return a1.getWeight().compareTo(a2.getWeight());
	}
};

// 람다 이용
Comparator<Apple> byWeight =
	(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

이렇게 구현하면 compare 메서드의 바디를 직접 전달하는 것처럼 코드를 전달할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/5defb0b8-b529-4abf-9468-4596178eb058/Untitled.jpeg)

- 파라미터 리스트
Comparator의 compare 메서드 파라미터(사과 두 개)
- 화살표
화살표 (**→**)는  람다의 파라미터 리스트와 바디를 구분한다.
- 람다 바디
두 사과의 무게를 비교한다. 람다의 반환값에 해당하는 표현식이다.

### 자바 8에서 지원하는 다섯 가지 람다 표현식 예제

```java
// 1 
(String s) -> s.length() // String 형식의 파라미터 하나를 가지며 int를 반환한다.
												// 람다 표현식에는 return이 함축되어 있으므로 return문을 명시적으로 사용하지 않아도 된다.
// 2
(Apple a) -> a.getWeight() > 150 
// Apple 형식의 파라미터 하나를 가지며 boolean(사과150보다 무거운지 결정)을 반환

// 3 
(int x, int y) -> { // int 형식의 파라미터 두 개를 가지며 리턴값이 없다. (void리턴)
	System.out.println("result:");
	System.out.println(x+y);
}
// 3 번의 예제에서 볼 수 있듯이 람다 표현식은 여러행의 문장을 포함할 수 있다.

// 4
() -> 42 // 파라미터가 없으며 int 42를 반환한다.
// 5
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())
// Apple 형식의 파라미터 두 개를 가지며 int(두 사과의 무게 비교 결과)를 반환한다.
```

### 표현식 스타일

람다의 기본 문법

`(parameters) → expression`

또는 블록 스타일로 표현

`(parameters) → { statements; }`

### ✍️ Quiz

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/b62b4e56-e786-4f4d-ac5d-c27e8709a663/Untitled.jpeg)

### 람다 예제

```java
// 불리언 방식
(List<String> list) -> list.isEmpty()

// 객체 생성
() -> new Apple(10)

// 객체에서 소비
(Apple a) -> {
	System.out.println(a.getWeight());
}

// 객체에서 선택/추출
(String s) -> s.length()

// 두 값을 조합
(int a, int b) -> a * b

// 두 객체 비교
(Apple a1, Apple a2) ->
	a1.getWeight().compareTo(a2.getWeight())
```

# 3.2 람다를 어디에서 어떻게 사용하는 것일까?

이전 예제에선 `Comparator<Apple>` 형식의 변수에 람다를 할당 했지만,
아래 예시처럼 필터 메서드에도 람다를 활용할 수 있다

```java
List<Apple> greenApples =
	filter(inventory, **(Apple a) -> GREEN.equals(a.getColor()));**
```

그럼 이제 궁금하지 않은가?
어디에서 람다를 사용할 수 있다는 것인지 ? → 함수형 인터페이스라는 문맥에서 람다 표현식을 사용할 수 있다.

## 3.2.1 함수형 인터페이스

이전에 `Predicate<T>`인터페이스로 필터 메서드를 파라미터화 할 수 있었다.
바로 `Predicate<T>` 가 함수형 인터페이스이며, 오직 한나의 추상 메서드만 지정하기 때문이다.

```java
public interface Predicate<T> {
	boolean test (T t);
}
```

쉽게 말해, 함수형 인터페이스는 정확히 하나의 추상 메서드를 지정하는 인터페이스라는 것이다.
지금까지 살펴본 자바 API의 함수형 인터페이스로 `Comparator`, `Ruunable` 등이 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/853af7a8-16e2-44ac-adda-e7927c99cee9/Untitled.jpeg)

<aside>
💡 NOTE
13 chapter에서 자세히 알아보겠지만, 
인터페이스는 **디폴트 메서드**(인터페이스의 메서드를 구현하지 않은 클래스를 고려해서 기본 구현을 제공하는 바디를 포함하는 메서드)를 포함할 수 있다.
많은 디폴트 메서드가 있더라도 **추상 메서드가 오직 하나**면 함수형 인터페이스다.

</aside>

### ✍️ 퀴즈

함수형 인터페이스는 어느 것인가?

```java
// 1
public interface Adder{
	int add(int a, int b);
}
// 2
public interface SmartAdder extends Adder{
	int add(double a, double b);
}
// 3
public interface Nothing{
}

/*
1번만 함수형 인터페이스다.
2번은 두 추상 add 메서드(하나는 Adder에서 상속받음)를 포함하므로 함수형 인터페이스가 아니다.
3번은 추상메서드가 없으므로 함수형 인터페이스가 아니다.
*/
```

그렇다면 함수형 인터페이스로는 무엇을 할 수 있을까?
→ 람다 표현식으로 함수형 인터페이스의 추상메서드 구현을 직접 전달할 수 있으므로 **전체 표현식을 함수형 인터페이스의 인스턴스로 취급**(기술적으로 따지면 함수형 인터페이스를 **구현한** 클래스의 인스턴스) 할 수 있다.

<aside>
❓ **전체 표현식을 함수형 인터페이스의 인스턴스로 취급 하면 좋은 이유 ?**
이전에 배웠듯이, Java와 같은 언어에서 함수형 프로그래밍 패러다임을 지원하는 데 매우 유용한 기능이다.  함수형 프로그래밍은 함수를 일급 시민으로 취급하고, 함수를 변수에 할당하고 전달하는 것을 중요하게 다루는 프로그래밍 스타일을 강조한다. 함수형 인터페이스는 이러한 패러다임을 지원하기 위해 도입된 개념 중 하나이다.
(설명)

1. **람다 표현식**
함수형 인터페이스를 사용하면 람다 표현식을 통해 간결하고 읽기 쉬운 코드를 작성할 수 있다. 람다 표현식을 사용하면 함수를 익명으로 정의하고 전달할 수 있으므로 코드를 간결하게 유지할 수 있다.
2. **다형성**
함수형 인터페이스를 사용하면 다형성을 활용할 수 있다. 서로 다른 함수형 인터페이스를 구현하는 다양한 람다 표현식을 전달할 수 있으며, 이를 통해 코드의 재사용성과 확장성을 향상시킬 수 있다.
3. **병렬 및 비동기 프로그래밍**
함수형 인터페이스를 사용하면 병렬 및 비동기 프로그래밍을 쉽게 구현할 수 있다. 스레드나 비동기 작업에서 함수형 인터페이스를 사용하여 작업을 분산하고 조합할 수 있으므로 효율적인 병렬 실행이 가능.
4. **코드 가독성**
함수형 인터페이스를 사용하면 코드의 가독성을 향상시킬 수 있다. 함수형 프로그래밍은 부작용을 최소화하고 불변성을 강조하는 경향이 있어서, 코드의 동작을 이해하고 디버그하기 쉽다.
5. **라이브러리 및 프레임워크 지원**
많은 Java 라이브러리와 프레임워크가 함수형 인터페이스를 활용하여 구현되어 있으며, 함수형 프로그래밍 스타일을 채택하고 있다. 이러한 라이브러리를 사용하면 코드 작성과 유지보수가 더 쉬워진다.

**요약하면**,
전체 표현식을 함수형 인터페이스의 인스턴스로 취급하면 코드를 간결하게 작성하고 읽기 쉽게 만들며, 다형성, 병렬성, 가독성 및 라이브러리 지원을 통해 향상할 수 있다.

</aside>

```java
Runnable r1 = () -> System.out.println("Hello World 1"); // uses a lambda

// 익명클래스 사용
Runnable r2 = new Runnable(){ 
	public void run(){
		System.out.println("hello world 2");
	}
};

public static void process(Runnable r){
	r.run();
}
process(r1); // Hello World 1
process(r2); // hello world 2
porcess(() -> System.out.print("Hello world 3")); // 직접 전달된 람다표현임으로 Hello world 3 출력
```

### 3.2.2 함수 디스크립터

람다 표현식이 나타내는 함수의 시그니처 또는 형태를 나타내는 것을 말한다.
함수 디스크립터는 매개변수의 수와 타입, 반환값의 타입으로 이루어져 있다. 
함수 디스크립터는 함수형 인터페이스를 정의할 때 사용되며, 람다 표현식이 해당 인터페이스와 일치하는지 확인|하는 데 중요한 역할을 한다.
`(예시)`

1. `(int, int) -> int`: 이 함수 디스크립터는 두 개의 `int` 매개변수를 가지고 ****`int`를 반환하는 함수를 나타낸다. 이러한 함수는 두 개의 정수를 입력으로 받아 하나의 정수를 반환한다.
2. `(String) -> int`: 이 함수 디스크립터는 하나의 `String` 매개변수를 가지고 ****`int`를 반환하는 함수를 나타내며, 이러한 함수는 문자열을 입력으로 받아 정수를 반환한다.
3. `() -> void` 또는 `() -> {}`: 이 함수 디스크립터는 매개변수가 없고 반환값이 없는 함수,
즉 빈(void) 함수를 나타낸다. 이러한 함수는 아무런 입력도 받지 않고 아무것도 반환하지 않는다.

```java
public void process(Runnable r){
	r.run();
}
porcess() -> System.out.println("Awesome"));
```

`Awesome` 이 출력된다. `porcess() -> System.out.println("Awesome"));` 이 인수가 없으며 void를 반환하는 람다 표현식이기 때문이다. 이는 Runnable 인터페이스의 run 메서드 시그니처와 같은 것이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/01c9d1f3-f79d-4bb5-ba49-2fa8a234e0c5/Untitled.jpeg)

### ✍️ 퀴즈

```java
// 다음 중 람다 표현식을 올바로 사용한 것은?
1. execute(() -> {});
	public void execute(Runnable r){
		r.run();
	}
2. public Callable<String> fetch(){
		return () -> "Tricky exam; -)";
}
3. Predicate<Apple>p = (Apple a) -> a.getWeight();
```

- 정답
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/cedfeaa3-0d6a-4b9b-93c1-a7cf848a4853/Untitled.jpeg)
    

<aside>
📌 **@Functionallnterface**는 무엇인가?
자바 API에 함수형 인터페이스에 @Functionallnterface 어노테이션이 추가되었다.
함수형 인터페이스임을 가리키는 어노테이션이다.
@Functionallnterface 로 인터페이스를 선언했지만 실제로 함수형 인터페이스가 아니면 컴파일러가 에러를 발생시킨다.
예를 들어 추상메서드가 한 개 이상이라면 “Multiple nonoverrindg…(인터페이스 …에 오버라이드 하지 않은 여러 추상 메서드가 있음)”같은 에러가 발생할 수 있다.

</aside>

## 3.3 람다 활용 : 실행 어라운드 패턴

자원처리(예, db의 파일처리)에 사용하는 순환패턴은 자원을 열고, 처리한 다음에, 자원을 닫는 순서로 이루어짐.
설정과 정리 과정은 대부분 비슷하다.
즉, 실제 자원을 처리하는 코드를 설정과 정리 두 과정이 둘러싸는 형태를 갖는다.
[그림 3-2]와 같은 형식의 코드를 **실행 어라운드 패턴**이라 한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/d0bcf4c5-80f5-4012-a0ac-f625c5676fde/Untitled.jpeg)

<aside>
❓ 데이터 베이스에서 **순환패턴이란** ?
두 가지 상황을 가리킬 수 있다.

1. **순환 종속성** (Cyclic Dependency)
데이터베이스에서 순환 종속성은 두 개 이상의 테이블 또는 엔터티 간에 순환 참조가 발생하는 상황을 의미.
예를 들어, 테이블 A가 테이블 B를 참조하고, 동시에 테이블 B가 테이블 A를 참조하는 경우 순환 종속성이 발생한다. 이는 데이터베이스의 무결성을 해칠 수 있고, 쿼리나 데이터 조작 작업을 복잡하게 만들 수 있으며, 데이터 무결성 문제를 일으킬 수 있다. 데이터 모델을 설계할 때 이러한 순환 종속성을 피하는 것이 중요하다.
2. **순환 패턴 검색** (Cycle Detection)
데이터베이스에서 순환 패턴 검색은 그래프 이론의 개념을 사용하여 그래프 또는 네트워크 구조에서 순환 패턴을 찾는 과정을 말한다. 이것은 주로 그래프 데이터베이스나 네트워크 관련 애플리케이션에서 사용됩니다. 순환 패턴 검색은 순환 경로나 루프를 찾아내는 알고리즘을 적용하여 데이터의 구조에서 순환적인 관계를 식별하고 분석하는 데 도움을 준다.
</aside>

### 실행 어라운드 패턴을 적용하는 네 단계의 과정

```java
// 1 
public String porcessFile() throws IOException{
	try (BufferedReader br =
				new BufferedReader(new FileReader("data.txt"))){
		return br.readLine();
	}
}

// 2
public interface BufferedReaderPorcessor{
	String process(BufferedReader b) throws IOException;
}
public String processFile(BufferedReaderPorcessor p) throws IOException{
	...
}

// 3
public String porcessFile(BufferedReaderProcessor p) throws IOException{
	try (BufferedReader br =
				new BufferedReader(new FileReader("data.txt"))){
		return p.process(br);
	}
}

// 4
String oneLine = porcessFile((BufferedReader br) ->
																br.readLine());
String twoLines = porcessFile((BufferedReader br) ->
																br.readLine + br.readLine());
```

<aside>
👉 **1단계 : 동작 파라미터화를 기억하라**
현재 코드는 파일에서 한 번에 한 줄만 읽을 수 있다. 한 번에 두 줄을 읽거나 가장 자주 사용되는 단어를 반환하려면 어떻게 해야 할까? 기존의 설정, 정리 과정은 재사용하고 processFile 메서드만 다른 동작을 수행하도록 명령할 수 있다면 좋을 것이다.
→ 즉, **processFile의 동작을 파라미터화** 하라는 것이다.
processFile 메서드가 한 번에 두 행을 읽게 하려면,
1) BufferedReader를 인수로 받아서 String을 반환하는 람다가 필요
2) BufferedReader에서 두 행을 출력하는 코드
`String result = processFile((BufferedReader br) →`
                                                `br.readLine() + br.readLine());`

</aside>

<aside>
👉 2**단계 : 함수형 인터페이스를 이용해서 동작 전달**
함수형 인터페이스 자리에 람다를 사용할 수 있다.
따라서 `BufferedReader → String` 과 `IOException`을 던질 수 있는 시그니처와 일치하는 함수형 인터페이스를 만들면 된다.  이 인터페이스를 `BufferedReaderPorcessor` 라고 정의하겠다.
(예시)
`@FunctionalInterface`
`public interface **BufferedReaderPorcessor**{`
	`String **process**(BufferedReader b) throws IOException;`
`}`
`public String processFile(BufferedReaderPorcessor p) throws IOException{`
	`...`
`}`

</aside>

<aside>
👉 3**단계 : 동작 실행**
람다 표현식으로 함수형 인터페이스의 추상 메서드 구현을 직접 전달할 수 있으며 전달된 코드는 함수형 인터페이스의 인스턴스로 전달된 코드와 같은 방식으로 처리한다.
따라서 `processFile` 바디 내에서 `BufferedReaderProcessor` 객체의 `process`를 호출할 수 있다.
`public String porcessFile(BufferedReaderProcessor p) throws IOException{
	try (BufferedReader br =
				new BufferedReader(new FileReader("data.txt"))){
		return p.process(br); ← BufferedReader 객체 처리
	}
}`

</aside>

<aside>
👉 4**단계 : 람다 실행**
이제 ****람다를 이용해서 다양한 동작을 processFile 메서드로 전달할 수 있다.
// 한 행을 처리하는 코드
`String oneLine = porcessFile((BufferedReader br) -> br.readLine());`
// 두 행을 처리하는 코드
`String twoLines = porcessFile((BufferedReader br) -> br.readLine + br.readLine());`

</aside>

지금까진 함수형 인터페이스를 이용해서 람다를 전달하는 방법을 확인했다. (이때 인터페이스도 정의함)
이제부턴 다양한 람다를 전달하는 데 재활용할 수 있도록 자바 8에 추가된 새로운 인터페이스를 배울 것이다.

## 3.4 함수형 인터페이스 사용

함수형 인터페이스란? → 오직 하나의 추상 메서드를 지정한다.
함수형 인터페이스의 추상 메서드는 람다 표현식의 시그니처를 묘사한다.
함수형 인터페이스의 추상메서드 시그니처를 **함수 디스크립터**라고 한다.
다양한 람다 표현식을 사용하려면 공통의 함수 디스크립터를 기술하는 함수형 인터페이스 집합이 필요하다3

### 3.4.1 predicate

`java.util.function.Predicate<T>` 인터페이스는 test라는 추상 메서드를 정의하며 test는 제네릭 형식 T의 객체를 인수로 받아 boolean을 return한다.
우리가 만들었던 인터페이스와 같은 형태인데, 따로 정의할 필요 없이 바로 사용할 수 있다는 점이 특징이다.
T 형식의 객체를 사용하는 boolean 표현식이 필요한 상황에서 Predicate 인터페이스를 사용할 수 있다.
다음 예제처럼 String 객체를 인수로 받는 람다를 정의할 수 있다.

```java
@FunctionalInterface
public interface **Predicate<T>**{
	boolean **test**(T t);
}
public <T> List<T> **filter(**List<T> list, Predicate<T> p){
	List<T> results = new ArrayList<>();
	for(T t: list) {
		if(p.test(t)){
			results.add(t);
		}
	}
	return results;
}
**Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();**
List<String> nonEmpty = filter(listOfStrings, **nonEmptyStringPredicate**);
```

### 3.4.2 Consumer

`java.util.function.Consumer<T>` 인터페이스는 제네릭 형식 T 객체를 받아서 void를 반환하는 accept라는 추상 메서드를 정의한다.
T 형식의 객체를 인수로 받아서 어떤 동작을 수행하고 싶을 때, Consumer 인터페이스를 사용할 수 있다.
예를 들어 Integer 리스트를 인수로 받아서 각 항목에 어떤 동작을 수행하는 forEach 메서드를 정의할 때 Consumer를 활용한다.
아래는 forEach와 람다를 이용해서 리스트의 모든 항목을 출력하는 예

```java
@FunctionalInterface
public interface **Consumer<T>**{
	void **accept**(T t);
}

public <T> void **forEach**(List<T> list, Consumer<T> c){
	for(T t: list) {
			c.accept(t);
	}
}
forEach(
	Arrays.asList(1,2,3,4,5),
	(Integer i) -> System.out.print(i) // Consumer의 accept 메서드를 구현하는 람다
);
```

### 3.4.3 Function

`java.util.function<T,R>` 인터페이스는 제네릭 형식 T를 인수로 받아서 제네릭 형식 R 객체를 반환하는 추상 메서드 apply를 정의한다. 입력을 출력으로 매핑하는 람다를 정의할 때 Function 인터페이스를 활용할 수 있다. (예: 사과의 무게 정보를 추출하거나 문자열을 길이와 매핑)
아래 예제는 String 리스트를 인수로 받아 각 String의 길이를 포함하는 Integer 리스트로 반환하는 map 메서드를 정의하는 예제이다.

```java
@FunctionalInterface
public interface **Function<T, R>**{
	R **apply**(T t);
}

public <T,R> List<R> **map**(list<T> list, Function<T,R>f){
	List<R> result = new ArrayList<>();
	for(T t: list){
		result.add(f.apply(t));
	}
	return result;
}

// [7,2,6]
List<Integer> l = map(
					Arrays.asList("lambdas", "in", "action"),
					(String s) -> s.length() // Function의 apply 메서드를 구현하는 람다이다.
);
```

### 기본형 특화

지금까진 세 개의 함수형 인터페이스 Predicate<T>, Consumer<T>, Function<T,R>을 알아봤다.
But 특화된 형식의 함수형 인터페이스도 있다.
자바의 모든 형식은 참조형(reference type, 예를 들면 Byte, Integer, Object, List)아니면 기본형(primitive type, 예를들면 int, double, byte, char)에 해당한다.
하지만 제네릭 파라미터 (예: `Consumer<T>` 의 T)에는 참조형만 사용할 수 있다.
제네릭의 내부구현 때문에 어쩔 수 없기 때문이다.
자바에서는 기본형을 참조형으로 변환하는 기능을 제공한다. 이 기능을 **박싱**이라고 한다.
참조형을 기본형으로 변환하는 반대 동작을 **언박싱**이라고한다.
또한 프로그래머가 편리하게 코드를 구현할 수 있도록 박싱과 언박싱이 자동으로 이루어지는 **오토박싱**이라는 기능도 제공한다.

```java
List<Integer> list = new ArrayList<>();
for (int i= 300; i<400; i++){
	list.add(i);
}
```

위 코드는 유효한 코드다(int가 Integer로 박싱됨). But 이런 변환 과정은 비용이 소모된다.
박싱한 값은 기본형을 감싸는 wrapper이며 heap에 저장된다.
따라서 박싱한 값은 메모리를 더 소비하며 기본형을 가져올 때도 메모리를 탐색하는 과정이 필요하다.

자바 8에선 기본형을 입출력으로 사용하는 상황에서 오토박싱 동작을 피할 수 있도록 특별한 버전의 함수형 인터페이스를 제공한다.
예를 들어 아래 예제에서 IntPredicate는 1000이라는 값을 박싱하지 않지만, `Predicate<Integer>`는 1000이라는 값을 Integer 객체로 박싱한다.

```java
public interface IntPredicate{
	boolean test(int i);
}

IntPredicate evenNumbers = (int i) -> i%2 ==0;
evenNUmbers.test(1000); // true(박싱 없음)

Predicate<Integer> oddNumbers = (Integer i) -> i%2 !=0;
oddNumbers.test(1000); // false (박싱)
```

<aside>
❓ **제네릭의 내부구현**

Java에서 제네릭은 타입 안전성을 제공하면서 컴파일 시에 형식 오류를 잡을 수 있도록 도와준다.
 제네릭 클래스나 인터페이스를 선언할 때 사용되는 타입 매개변수 (예: **`T`**, **`E`**, **`K`** 등)는 참조형 타입만 사용할 수 있다. 이러한 제한은 제네릭의 내부 구현과 관련이 있다.

Java의 제네릭은 타입 소거(type erasure)라는 메커니즘을 사용하여 구현된다. 
이는 컴파일 시에 제네릭 타입 정보가 제거되고, 제네릭 타입의 객체가 실제로는 Object 타입으로 다뤄지도록 한다. 제네릭 타입 매개변수의 실제 타입 정보는 런타임 시에는 알 수 없다.

이러한 타입 소거 메커니즘 때문에 제네릭 클래스나 인터페이스에서 사용되는 타입 매개변수는 참조형 타입으로 제한되게 되는 것이다. 기본 데이터 타입 (예: **`int`**, **`double`**, **`char`** 등)은 객체로 변환될 수 없는 원시 데이터 타입이므로, 제네릭에서 사용될 수 없다.

따라서 제네릭의 내부 구현에서는 타입 소거를 통해 제네릭 타입의 실제 정보를 가리고, 이로 인해 참조형 타입만이 제네릭 클래스 또는 인터페이스에서 사용될 수 있는 제약이 생긴다. 이러한 제약을 통해 제네릭을 사용할 때 안전성을 유지할 수 있기 때문이다.

</aside>

### 자바 API 에서 제공하는 대표적인 함수형 인터페이스와 함수 디스크립터

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/21070006-bd88-4d0b-80a0-6b168fd7abd3/Untitled.jpeg)

### ✍️  3-4 퀴즈

```java
// 함수형 디스크립터(람다 표현식의 시그니처)가 없을 때 어떤 함수형 인터페이스를 사용할 수 있는가?
// 보통 위의 표에서 찾을 수 있다. 또한 이들 함수형 인터페이스에 사용할 수 있는 유효현 람다 표현식은?
1. T -> R
2. (int, int) -> int
3. T -> void
4. () -> T
5. (T, U) -> R
```

- 정답
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/fac85a04-512c-4af7-b3fb-43f86bd73ca5/Untitled.jpeg)
    

### 람다와 함수형 인터페이스 예제

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/e902ff6e-41bc-45ae-b1ab-d5126717c9e8/Untitled.jpeg)

<aside>
👉 **예외, 람다, 함수형 인터페이스의 관계**
함수형 인터페이스는 확인된 예외를 던지는 동작을 허용하지 않는다.
즉, 예외를 던지는 람다 표현식을 만들려면 확인된 예외를 선언하는 함수형 인터페이스를 직접 정의하거나 람다를 `try/catch` 블록으로 감싸야 한다.
(IOException을 명시적으로 선언하는 함수형 인터페이스 BufferedReaderPorcessor)
@FunctionalInterface
public interface BufferedReaderPorcessor{
	String process(BufferedReader b) throws IOException;
}
BufferedReader Processor p = (BufferedReader br) → br.readLine();

그러나 우리는 Function<T, R> 형식의 함수형 인터페이스를 기대하는 API를 사용하고 있으며, 직접 함수형 인터페이스를 만들기 어려운 상황이다.
이런 상황에서는 다음과 같이 명시적으로 확인된 예외를 잡을 수 있다.
Function<BufferedReader, String> f = (BufferedReader ) → {
   try{
       return b.readLine();
   } catch(IOException e) {
           throw new RuntimeException(e);
   }
};

</aside>

지금까진, 람다를 만들고 사용하는 방법을 알아봤다.
이번엔 컴파일러가 람다의 형식을 어떻게 확인하는지, 피해야 할 사항은 무엇인지(예 : 람다 표현식에서 바디 안에 있는 지역 변수를 참조하지 않아야 한다든가 void 호환 람다는 멀리해야한다) 등 더 깊이 있는 내용을 진행한다.

## 3.5 형식 검사, 형식 추론, 제약

(일단 생략) → 나중에 공부할 예정

### 3.6 메서드 참조

쉽게 설명하면, 특정 람다 표현식을 축약한 것이다.
메서드 참조를 이용하면 기존의 메서드 정의를 재활용해서 람다처럼 전달할 수 있다.
때로는 람다 표현식보다 메서드 참조를 사용하는 것이 가독성이 더 좋을 때가 있다.

```java
// 기본 코드
inventory.sort((Apple a1, Apple a2) ->
									a1.getWeight().compareTo(g2.getWeight()));
// 메서드 참조와 활용한 코드
inventory.sort(comparing(Apple::getWeight); // 첫 번째 메서드 참조
```

다시 말해, 메서드 참조는 특정 메서드만을 호출하는 람다의 축약형이며, 메서드 참조를 이용하면 기존 메서드 구현으로 람다 표현식을 만들 수 있다. 이때 명시적으로 **메서드명을 참조함으로써 가독성을 높일 수 있다**.
(예, 람다가 ‘이 메서드를 직접 호출해’ 명령한다면 메서드를 어떻게 호출해야 하는지 설명을 참조하기보단 메서드명을 직접 참조하는 것이 편리하다)
그렇다면 메서드 참조는 어떻게 할까? → 이전에 배웠듯이, `::`를 통해 할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/c297f5b5-ae29-441d-b378-940fbf600d4a/Untitled.jpeg)

### 메서드 참조를 만드는 방법

메서드 참조는 3가지 유형으로 구분할 수 있다.

1. **정적 메서드 참조**
예 : Integer 의 parseInt 메서드는 Intger::parseInt 로 표현 가능
2. **다양한 형식의 인스턴스 메서드 참조**
예 : String 의 length 메서드는 String::length로 표현 가능
3. **기존 객체의 인스턴스 메서드 참조**
예 : Transaction 객체를 할당 받은 expensiveTransaction 지역 변수가 있고, Transaction 객체에는 getValue 메서드가 있다면, 이를 expensiveTransaction::getValue라고 표현 가능

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/36163c79-52e4-48b1-b78f-77f715e15d01/Untitled.jpeg)

컴파일러는 람다 표현식의 형식을 검사하던 방식과 비슷한 과정으로 메서드 참조가 주어진 함수형 인터페이스와 호환하는지 확인한다. → 즉, 메서드 참조는 콘텍스트의 형식과 일치해야한다.

<aside>
❓ **콘텍스트의 형식** ?
람다 표현식 또는 메서드 참조가 사용되는 문맥에서 기대되는 함수형 인터페이스의 형식을 가리킨다.
콘텍스트의 형식은 컴파일러에게 람다 표현식 또는 메서드 참조를 어떻게 해석해야 하는지 알려주는 역할을 한다.

`List<String> names = Arrays.asList("Alice", "Bob", "Charlie");`
`names.forEach(System.out::println);`

위의 코드에서 `names.forEach` 메서드는 `Consumer<T>` 형식을 필요로 한다. `System.out::println`은 메서드 참조로서 `Consumer<T>` 형식과 일치해야 한다. 
이때 콘텍스트의 형식은 `Consumer<T>`이며, `System.out::println`은 이 형식과 일치하므로 컴파일러는 메서드 참조를 올바른 방식으로 해석하게 된다.

람다 표현식도 비슷하게 콘텍스트의 형식을 따라야 하며, 람다 표현식이 사용되는 문맥에서 예상되는 함수형 인터페이스의 형식과 일치해야 한다.

**따라서**
콘텍스트의 형식은 컴파일러에게 람다 표현식 또는 메서드 참조의 타입을 결정하고, 이러한 표현식을 사용하는 메서드나 연산의 매개변수 타입에 따라 함수형 인터페이스를 결정하는데 중요한 역할을 한다.

</aside>

- ✍️ 퀴즈
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/075997f4-35b2-4955-b621-060d6a108880/6bd49d7f-00e9-4643-a0c3-2747fdde83f8/Untitled.jpeg)
    

지금까진 기존의 메서드 구현을 재활용해서 메서드 참조를 만드는 방법을 알아봤다.
이젠 클래스의 생성자를 이용하는 방법을 살펴볼 것이다.

## 3.6.2 생성자 참조

ClassName::new 처럼 클래스명과 new 키워드를 이용해서 기존 생성자의 참조를 만들 수 있다.
→ 정적 메서드의 참조를 만드는 방법과 비슷하다.
예: 인수가 없는 생성자, 즉 Supplier의 ()→ Apple과 같은 시그니처를 갖는 생성자가 있다고 가정.

```java
// 1 (1~2) 같은 코드다.
Supplier<Apple> c1 = Apple::new;
Apple a1 = c1.get(); // supplier의 get메서드를 호출해서 새로운 Apple 객체를 생성할 수 있다.
// 2
Supplier<Apple> c1 = ()-> new Apple(); // 람다 표현식은 default Constructor를 가진 Apple를 생성
Apple a1 = c1.get(); // supplier의 get메서드를 호출해서 새로운 Apple 객체를 만들 수 있다.

// 3  (3~4) 같은 코드
// Apple(Integer weight)라는 시그니처를 갖는 생성자는 Function 인터페이스의 시그니처와 같다.
Function<Integer, Apple> c2 = Apple::new; // Apple(Integer weight)의 생성자 참조
Apple a2 = c2.apply(110); // Function의 apply메서드에 무게를 인수로 호출해서 Apple 객체 생성 가능.
// 4
Function<Integer, Apple> c2 = (weight) -> new Apple(weight); // 특정 무게의 사과를 만드는 람다식
Apple a2 = c2.apply(110); // Function의 apply 메서드에 무게를 인수로 호출해서 새로은 Apple 객체 만들 수 있음.
```

Integer를 포함하는 리스트의 각 요소를 우리가 정의했던 map 같은 메서드를 이용해서 Apple 생성자로 전달한다. 결과적으로 다양한 무게를 포함하는 사과 리스트가 만들어지게 되는 것이다.

```java
List<Integer> weights = Arrays.asList(7,3,4,10);
List<Apple> apples = map(weights, Apple::new); // map 메서드로 생성자 참조 전달
public List<Apple> **map**(List<Integer> list, Function<Integer, Apple> f) {
		List<Apple> result = new ArrayList<>();
		for(Integer i: list){
				result.add(f.apply(i));
		}
		return result;
}
/*
Apple(String color, Integer weight)처럼 두 인수를 갖는 생성자는 BiFunction 인터페이스와 같은 시그니처를 가지므로 다음처럼 할 수 있다.
*/
// 1 (1~2 같은 코드)
BiFunction<Color, Integer, Apple> c3 = Apple::new; // Apple(String color, Integer weight)의 생성자 참조
	Apple a3 = c3.apply(GREEN,110); // 색과 무게를 인수로 제공해서 새로운 Apple 객체 생성
// 2
BiFunction<Color, Integer, Apple> c3 =
		(color, weight) -> new Apple(color, weight); // 특정 색과 무게를 가진 사과를 만드는 람다식
Apple a3 = c3.apply(GREEN, 110); // 색,무게 인수로 제공. 새로운 객체 생성
```

이번엔 인스턴스화하지 않고 생성자에 접근할 수 있는 기능을 다양한 상황에 응용할 수 있는 것을  알아볼 것이다.
예를 들어 Map으로 생성자와 문자열값을 관련시킬 수 있다. 그리고 String과 Integer이 주어졌을 때, 다양한 무게를 갖는 여러 종류의 과일을 만드는 giveMeFruit라는 메서드 생성 가능.

```java
static Map<String, Function<Intger,Fruit>> map = new HashMap<>();
static {
	map.put("apple", Apple::new);
	map.put("orange", Orange::new);
	// etc...
}

public static Fruit **giveMeFruit**(String fruit, Integer weight){
	return map.get(fruit.toLowerCase()) // map에서 Function<Integer,Fruit>를 얻는다.
						.apply(weight);//Function의 apply메서드에 정수 무게 파라미터를 제공해서 Fruit 생성
}
```

### ✍️ 3-7 퀴즈 (생성자 참조)

```java
지금까지 인수가 없거나, 하나 또는 둘인 생성자를 생성자 참조로 바꾸는 방법을 공부했다.
그렇다면, Color(int,int,int)처럼 인수가 세 개인 생성자 참조를 사용하려면 어떻게 해야할까?
```

- Answer :
    
    ```java
    생성자 참조 문법은 ClassName::new 이므로 Color 생성자의 참조는 Color::new가 된다.
    But, 이를 사용하려면 생성자 참조와 일치하는 시그니처를 갖는 함수형 인터페이스가 필요하다.
    현재 이런 시그니처를 갖는 함수형 인터페이스는 제공되지 않으므로 우리가 직접 함수형 인터페이스를 만들어야한다.
    // 이런 식으로 만들어준다.
    public interface TriFunction<T, U, V,R>{
    		R apply(T t, U u, V v);
    } 
    // 이후엔 새로운 생성자 참조를 사용할 수 있다.
    TriFunction<Integer, Integer, Intger, Color> colorFactory = Color::new;
    ```
    

# 3.7 람다, 메서드 참조 활용하기

### 1단계 : 코드 전달

```java
public class AppleComparator implements Comparator<Apple>{
	public int compare(Apple a1, Apple a2){
		return a1.getWeight().compareTo(a2.getWeight());
	}
}
inventory.sort(new AppleComaprator());
```

### 2단계 : 익명 클래스 사용

```java
inventory.sort(new Comparator<Apple>(){
	public int compare(Apple a1, Apple a2){
		return a1.getWeight().compareTo(a2.getWeight());
	}
});
```

### 3단계 : 람다 표현식 사용

```java
inventory.sort((a1,a2)) -> a1.getWeight().compareTo(a2.getWeight()));
//위 코드를 좀더 가독성있게 향상시킴 Comparator는 Comparable키를 추출해서
// Comparator 객체로 만드는 Function 함수를 인수로 받는 정적 메서드 comparing을 포함한다.
Comparator<Apple> c = Comparator.comparing((Apple a) -> a.getWeight());
// 간소화
import static java.util.Comparator.comparing;
inventory.sort(comparing(apple -> apple.getWeight()));
```

### 4단계 : 메서드 참조 사용

```java
inventory.sort(comparing(Apple::getWeight));
```

# 3.8 람다 표현식을 조합할 수 있는 유용한 메서드

# 3.9 비슷한 수학적 개념

### 적분

적분은 함수의 면적 또는 미분된 함수의 원래 함수를 찾는데 사용한다.

### 🤜  적분 **문제**

f(x) = x^2를 [0, 1] 구간에서 수치 적분하는 방법
