# Google Java Style Guide
> 이 문서는 우아한형제들의 테크코스를 진행하면서 참고할 문서인 Google Java Style Guide를 한 문장씩 번역하며 공부한 문서입니다. 약간의 의역이 섞여 있을 수 있지만 본 의도는 해치지 않습니다.

원문 주소 : https://google.github.io/styleguide/javaguide.html

## 1. 개요

이 문서는 구글의 Java언의 코딩 표준의 완벽한 정의 문서이다. 자바 소스 파일은 이 문서의 규칙을 고수해야만 Google Style이라 칭할 수 있다.

다른 프로그래밍 스타일 가이드 처럼, 주제들은 포매팅같은 심미적인 주제뿐만 아니라, 다른 관습들 혹은 코딩 규칙들도 포함한다. 그러나 이 문서는 우리가 보편적으로 따르는 엄격한 규칙에 초점을 맞추고 있으며 명백히 실행할 수 없는 조언들을 주는 것을 지양한다. (인간 이든 기계이든)

### 1.1 용어 노트

이 문서에는 

1. class라는 용어는 "일반적인" 클래스, enum 클래스, 인터페이스 혹은 애노테이션 타입을 포괄하여 쓰인다.
2. member(혹은 class)라는 용어는 중첩 클래스, 필드, 메소드 혹은 생성자 즉, 초기화들과 주석들을 제외한 클래스의 모든 최상위 내용들을 포괄하여 쓰인다.
3. comment라는 용어는 항상 구현 주석을 나타낸다. 우리는 "documentation comments"라는 용어 대신에 "Javadoc"이라는 용어를 쓴다.

다른 "용어 노트"는 문서 전체에 가끔씩 나온다.

### 1.2 가이드 노트

이 문서의 예제 코드는 비표준이다. 즉, 예제는 Google Style이지만, 우아한 코드를 나타내지 않을 수 있다. 예제에서 나오는 추가적인 포매팅 선택들은 규칙으로써 제정되면 안된다. 

## 2. 소스파일들의 기본

### 2.1 파일 명

소스 파일들의 이름은 최상위 클래스들을 포함하는 대소문자를 구별하는 이름들로 되어 있고 .java 확장자를 가지고 있다.

### 2.2 파일 인코딩: UTF-8

소스 파일들은 UTF-8로 인코딩 되어 있다.

### 2.3 특수 문자들

#### 2.3.1 공백 문자

줄 끝내기 문자를 제외하고, ASCII horizontal space character (0x20)은 소스파일에 나오는 유일한 공백 문자이다. 이것은 다음을 암시한다:

1. 문자열과 문자들의 다른 모든 공백 문자들은 이스케이프 처리가 된다.
2. Tab은 들여쓰기에 사용되지 않는다.

#### 2.3.2. 특수 이스케이프 문자들

특수 이스케이프 문자들 (`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'` and `\\`)은 해당 옥텟(\012) 이나 유니코드(\u000a) 이스케이프 대신에  해당 문자가 사용된다.

#### 2.3.3 Non-ASCII 문자들

남은 non-ASCII 문자들중, 실제로 유니코드 문자 (e.g. `∞`) 인 혹은 유니코드와 동등한 이스케이프 문자 (e.g. `\u221e`) 이 사용된다. 비록, 유니 코드는 문자열 리터럴 밖에서 이스케이프되며 주석은 권장되지 않지만 이러한 선택은 코드를 쉽게 읽고 이해하기 위해 만들어졌다. 

예시:

| Example                                                  | Discussion                                                   |
| -------------------------------------------------------- | :----------------------------------------------------------- |
| `String unitAbbrev = "μs";`                              | 최적: 주석없이도 완벽하다.                                   |
| `String unitAbbrev = "\u03bcs"; // "μs"`                 | 허용, 하지만 이럴 이유는 없음.                               |
| `String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"` | 허용, 하지만 이상하고 실수할 수 있음.                        |
| `String unitAbbrev = "\u03bcs";`                         | 형편없음: 읽는 사람이 알 수가 없음.                          |
| `return '\ufeff' + content; // byte order mark`          | 좋음: 프린트할 수 없는 문자들에 대해 이스케이프를 썼으며 주석이 필요하다면 만든다. |

팁: 어떤 프로그램이 non-ASCII 문자를 제대로 다루지 못한다고 해서 읽이 어렵게 코드를 만들지 말라. 그렇다면 프로그램들은 부서지고 고쳐야 한다.

## 소스 파일 구조

소스 파일들은 이렇게 순서대로 구성되야 한다:

1. 라이센스나 저작권 정보 (만약 있다면)
2. 패키지(Package) 구문
3. 임포트(Import) 구문
4. 정확히 하나의 최상위 클래스

### 3.1 라이센스나 저작권 (만약 존제한다면)

만약 라이센스나 저작권이 있다면 여기에 속해야 한다.

### 3.2 패키지 구문

패키지 서술은 줄바꿈 하지 않는다. 열 제한은 (4.4 참고, 열제한: 100) 패키지 구문에 적용되지 않는다.

### 3.3 임포트 구문

#### 3.3.1 와일드 카드 임포트 하지 않기

💡`와일드 카드 임포트, static 같은 것들은 사용하지 않는다.`

#### 3.3.2 줄바꿈 하지 않기

임포트 구문은 줄바꿈 하지 않는다. 열 제한은 (4.4 참고, 열제한: 100) 임포트 구문에 적용되지 않는다.

#### 3.3.3 순서와 공간

임포트는 다음과 같은 단계를 따른다:

1. 하나의 블럭안에 static 임포트 포함
2. 하나의 블럭안에 non-static 임포트 포함

만약에 static과 non-static이 둘 다 있다면, 개행을 하고 두 개의 블럭으로 나눈다. 그 이외에는 개행이 있으면 안된다.

각각의 블럭에는 ASCII 순서로 정렬되어 있어야 한다. (참고: '.'가 ';'전에 오기 때문에 임포트 구문에는 ASCII 순서가 아니다)

#### 3.3.4 클래스에는 static 임포트를 하지 않는다.

static 임포트는 static 중처버 클래스에 사용되지 않는다. 그것들은 일반적인 임포트를 사용한다.

### 3.4 클래스 정의

#### 3.4.1 정확히 최상위 클래스 하나를 정의

💡`소스 파일마다 각자의 최상위 클래스가 존재한다.` 

#### 3.4.2 클래스 본문 순서 정하기

클래스 안에 멤버들과 초기화들의 순서는 학습용이성에 많은 영향을 미친다. 그러나 하나의 맞는 방법은 없다. 다양한 클래스들은 각자의 방법대로 본문들을 포함하고 있다.

중요한 것은 💡`논리적 순서`를 따라야 한다는 것인데 그것은 유지보수자가 질문을 받았을 때 설명할 수 있어야 한다. 예를들어, 새로운 메소드들이 클래스 끝에 습관적으로 붙었다고 생각해보면 그것은 "연대기적 순서"이지 논리적 순서는 아니기 때문이다.

##### 3.4.2.1 오버로드: 나누지 마라

클래스가 여러개의 생성자들 혹은 💡`같은 이름의 함수들을 가지고 있다면 이것들은 가운데 다른 코드들 없이 차례로` 나타나야 한다. (private 멤버라 할 지라도)

## 4. 포매팅

용어 노트: block-like construct (블럭과 같은 구조, 괄호로 나타내어지는 구조를 의미)는 클래스, 함수, 생성자의 몸체를 나타낸다. 4.8.3.1에 나와있는 배열 초기화블럭은 블럭과 같은 구조로 간주될 수 있다.

#### 4.1 괄호

#### 4.1.1 괄호는 선택사항에서도 쓰인다.

괄호는 if, else, for, do, while 구문에 쓰이는데 몸체가 없거나 💡`한 줄의 구문에도 괄호`가 쓰인다.

#### 4.1.2 비어있지 않은 블럭: K & R 스타일

괄호는 비어있지 않은 블럭과 block-like construct에서 Kernighan과 Ritchie 스타일(Egyptian brackets)을 따른다. 

- 💡`여는 괄호 앞에는 줄 바꿈이 없음`
- 💡`여는 괄호 다음에 줄 바꿈`
- 💡`닫는 괄호 전에 줄 바꿈`
- 💡`닫는 괄호 다음에 줄 바꿈`, 그런데 이것은 오직 구문이 끝나거나 메소드,  생성자, 클래스가 끝났을 때 적용된다. 예를들어 else나 콤마뒤에 나오는 부분은 줄 바꿈을 하지 않는다.

예:

```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```

Enum 클래스에는 예외가 있다. 4.8.1

#### 4.1.4 빈 블럭들: 아마 간결하게

💡`빈 블럭이나 block-like construct 에서는 K & R 스타일을 따를 수 있다. 대안으로 { } 괄호 안에 문자가 없거나 줄바꿈이라면 열자마자 끝날 수 있다. 하지만 멀티 블럭 구문에서는 할 수 없다.`

예:

```java
  // 허용
  void doNothing() {}

  // 마찬가지로 허용
  void doNothingElse() {
  }
```

```java
 // 허용되지 않음: 멀티 블럭 구문에서는 간결한 빈 블럭을 사용할 수 없음
  try {
    doSomething();
  } catch (Exception e) {}
```

### 4.2 블럭 들여쓰기: +4 스페이스 ⚠️

💡`새 블록 또는 블록과 유사한 구조(block-like construct)가 열릴 때마다 들여쓰기가 네 칸씩 증가합니다.` 블록이 끝나면 들여쓰기는 이전 들여쓰기 단계로 돌아갑니다. 들여쓰기 단계는 블록 전체의 코드와 주석 모두에 적용됩니다. 

### 4.3 줄 당 하나의 서술

💡`하나의 구문은 줄바꿈이 뒤따른다.`

### 4.4 열 제한: 120 ⚠️
💡`Java 코드의 열 제한은 120자입니다.` "문자"는 유니코드 코드 포인트를 의미합니다. 밑에 서술된 예외들을 제외하고 이 숫자를 넘어간다면 줄 바꿈 (4.5 참조)을 해야한다.

하나의 유니코드 포인트는 화면상 길든 짧든 하나의 문자를 의미한다. 예를들어, full-width 문자(예를들어, 중국어, 일본어, 한국어)를 쓴다면 규칙보다 일찍 개행하는게 좋다.

예외:

1. 💡`개행이 불가능한 경우 (예를들어,  Javadoc의 긴 URL 혹은 긴 JSNI 메서드 레페런스)`
2. package 나 import 구문들 (3.2  패키지 구문, 3.3 임포트 구문 참조)
3. 쉘에 복사 붙여넣기 되는 커멘드 라인에 대한 주석

### 4.5 줄 바꿈

용어 노트: 💡`코드가 하나의 줄에서 여러 줄러 바뀐다면 그것을 줄 바꿈(개행)` 이라고 부른다.

매 상황별 어떻게 줄 바꿈하는지에 대한 정확한 방법은 없다. 주로 같은 조각의 코드에서 줄 바꿈을 한다.

참고: 줄 바꿈의 전형적 의도는 행 제한을 넘지 않기 위해서다. 심지어는 줄 제한에 걸리지 않더라도 저자의 재량에 따라 줄 바꿈이 될 수 있다.

팁: 함수나 변수를 빼내는 방법이 줄 바꿈을 대신할 수 있는 방법이 될 수 있다.

#### 4.5.1 언제 바꾸는가

줄 바꿈의 원칙은: 높은 문법 레벨에서 바꾸는 것이다. 또한:

1. 💡`non-assignment 연산자에서 줄 바꿈이 일어날 경우 바꿈은 기호 이전에 위치한다.` (Google Style Guide의 C++, JavaScript 와는 다름) 이것은 "operator-like" 기호에 적용된다. 
   - dot (.)
   - 2개의 콜론 (::)
   - 타입 바운딩의 앰퍼센드 기호 (<T extends Foo & Bar>)
   - catch 블럭의 파이프 ( catch (FooException | BarException e) ).
2. 줄 바꿈이 assignment 연산자에서 일어나면 기호 다음에 위치하지만 바뀌어도 상관없다.
   - 이것은 향상된 for문의 "assignment-operator-like" 콜론에도 적용된다.
3. 함수나 생성자의 이름에 여는 괄호가 있을 때.
4. 콤마 앞에 오는 토큰에 연결되어 있을 때.
5. 줄은 람다식의 인접한 화살표에서는 바뀌지 않는다. 하지만 람다의 몸체가 한 줄로 되어 있다면 바꿔도 된다. 예를들면:

```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```

참고: 줄 바꿈의 목적은 깨끗한 코드를 만듦에 있지, 줄 수를 줄이는데 있지 않다.

#### 4.5.2 들여쓰기 지속은 최소 +8 스페이스 ⚠️

줄 바꿈 시 그 다음 줄은 원래 줄에서 +8 이상 들여씁니다.

여러 줄 바꿈 줄이 있을 때, 들여쓰기는 +4 이상으로 변동 가능하다. 일반적으로, 두 개의 연속된 줄은 같은 들여쓰기 레벨을 갖고 구문적으로 병렬인 요소일때만 적용된다.

4.6.3에 수평 일직선은, 가변적인 수의 공백을 사용하여 특정 토큰을 이전 행과 정렬하는 불편함을 언급한다.

### 4.6 공백

#### 4.6.1 수직 공백 ⚠️

하나의 공백 줄은 항상 이럴 때 나타난다:

1. 💡` 연속적인 멤버나 클래스의 초기화: 필드, 생성자, 메소드, 중첩 클래스, 정적 초기화 그리고 인스턴스 초기화`
   - 예외: 두 개의 연속된 필드의 공백은 선택적이다. 그러한 공백은 필드의 논리적 그룹을 형성하는데 필요하다.
   - 예외: enum 상수의 공백 줄은 4.8.1 절에서 다룬다.
2. 문서의 다른 부분에서도 필요하다. ( 섹션3 이나 3.3)

💡` 하나의 공백 줄은 어디에서나 등장할 수 있고, 가독성을 높인다. 빈 줄은 가독성을 향상시키기 위해서라면 어디든(예를 들면 논리적으로 코드를 구분하기 위해 문장 사이) 사용 될 수 있습니다.` 클래스의 첫 번째 멤버나 초기화(initializer) 또는 마지막 멤버 또는 초기화( initializer) 뒤의 빈 줄은 권장되지도 비권장하지도 않습니다.

클래스의 첫 번째 멤버나 초기화(initializer) 앞에 있는 빈줄을 강제하지 않습니다.

다수의 공백 줄은 허용은 되나 요구되지는 않는다. (지양)

#### 4.6.1 수평 공백

리터럴, 주석 및 Javadoc을 제외하고 언어 또는 기타 스타일 규칙이 필요한 곳을 넘어서는 경우 단일 ASCII 공간이 다음 장소에만 나타난다.

1. 💡`예약어를 나누는 경우, if, for, catch 같은 예약어 이후 나오는 여는 괄호에서 사용`
2. 💡`예약어를 나누는 경우, else 나 catch 같은 예약어 이후 나오는 닫는 중괄호에서 사용`
3. 중괄호는 여는 모든 경우에 사용, 예외 두가지
   - @SomeAnnotation({a, b})
   - String[][] x = {{"foo"}};
4. 임의의 이항 또는 삼항 연산자 양쪽에 사용. 이것은 "operator-like" 기호에도 적용된다.
   - <T extends Foo & Bar>  인접한 타입 바운딩의 앰퍼센드 연산자에서
   - catch (FooException | BarException e) 예외처리의 파이프 에서
   - `for` ("foreach") statement 향상된 for 문에서
   - `(String str) -> str.length()` 람다의 화살표에서
   - 하지만 두개의 :: 콜론에서는 띄우면 안된다. Object::toString
   - 그리고 dot(.) 연산자에서도 띄우면 안된다. object.toString()
5. , : ; 혹은 캐스팅 할때 닫는 괄호 뒤에서 사용
6. 💡`// 더블 슬래시에서 양쪽에 사용. 여러 수의 공백이 허용되지만 필수사항은 아니다.`
7. 💡`변수의 선언문 사이에서 List<String> list`
8. 💡`배열 선언문 사이에서 공백 선택`
   - new int[] {5, 6}` and `new int[] { 5, 6 } 둘 다 가능
9. 타입 애너테이션이나 대괄호에서 사용

이 규칙은 라인의 시작이나 끝에서 추가 공간을 요구하거나 금지하는 것은 아니다. 그것은 내부 공간만을 다룬다.

#### 4.6.3 수평 일직선 혹은 가로맞춤: 요구되지 않음

용어 노트: 가로 맞춤은 이전 줄의 특정 토큰 아래에 특정 토큰을 직접 표시하려는 목적으로 코드에 가변 개수의 추가 공간을 추가하는 방법이다. 

이것은 허용은 되지만 Google Style에서는 필수사항이 아니다. 심지어 이미 쓰고있는 상황에서도 💡`가로맞춤을 유지하는게 필수가 아니라고 말한다.

```java
private int x; // 괜찮음
private Color color; // 이것도 괜찮음

private int   x;      // 허용된다, 나중에 고쳐야 함
private Color color;  // 맞춰지지 않은 상태로 둘 수도 있다
```

팁: 맞춤은 가독성을 높여준다. 하지만 미래 유지보수에서 문제를 일으킨다. 나중에 한 줄만 수정한다고 가정해보자. 이 변경으로 인해 이전에 맞춰놓은 서식이 엉망으로 남을 수 있지만 허용은 된다. 제작자에게 수정하는 것을 촉구하며, 일련의 재 포매팅을 유발할 수 있다. 그 한줄짜리 변경은 이제 "폭발 반경"을 갖는다. 이것은 최악의 경우 혼잡하지 않은 상황이 될 수 있지만 근본적으로 버전 히스토리 정보가 손상되고 검토자를 느리게하고 머지 충돌이 일어난다.

### 4.7 소괄호 그룹: 추천

선택적 그룹 괄호는 작성자와 검토자가 코드가 없으면 잘못 해석 될 가능성이 없으며 코드를 읽기 쉽게 만든다는 데 동의하지 않는 경우에만 생략된다. 모든 독자가 전체 Java 연산자 우선 순위 테이블을 가지고 있다고 가정하는 것은 합리적이지 않습니다. 즉, 우선순위 연산자가 명확하더라도 소괄호로 감싸는것을 추천한다는 말이다.

### 4.8 특별한 구조

#### 4.8.1  Enum 클래스

💡`enum 상수의 각 컴마 다음에 개행은 선택적이다. 추가의 개행(주로 한개)도 허용된다.`

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

💡`메소드와 documentaation이 없는 enum 클래스는 배열 초기화와 같은 포맷으로 작성될 수 있다.` (4.8.3.1 배열 초기화 참조)

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

enum 클래스들은 클래스 이므로 클래스 포맷팅 형식이 적용된다.

#### 4.8.2 변수 선언

##### 4.8.2.1 정의당 하나의 변수

💡`매 변수 초기화는 (필드 혹은 지역) 하나의 변수만 초기화한다`: int a, b; 는 쓰이지 않는다.

예외: for 루프의 헤더에는 여러 변수선언이 쓰일 수 있다.

##### 4.8.2.2 필요하면 정의

지역변수는 블럭이나 block-like construct가 시작될 때 습관적으로 쓰일 필요는 없다. 대신에, 💡`지역변수는 범위를 좁히기 위해 그것들이 처음 사용될 때 (이유가 있어야 함) 가까운 위치에 선언한다. 지역변수는 전형적으로 초기화를 시키거나 선언과 동시에 초기화를 시킨다.

#### 4.8.3 배열

##### 4.8.3.1 배열 초기화는 "block-like"

💡`배열 초기화는 "block-like construct"처럼 포매팅 될 수 있다.` 예를들어 다음과 같은 경우는 모두 가능하다.

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

##### 4.8.3.2 C 스타일의 배열 선언문은 쓰지 말것

💡`대괄호는 타입에 붙는다. 변수에 붙으면 안된다.` `String[] args 로 사용`String args[ ] 이것은 안됨

#### 4.8.4 Switch 구문

용어 노트: switch 블럭 안에는 한개 혹은 여러개의 구문 그룹들이 들어간다. 각각의 그룹은 구문 이전에 한개 이상의 switch 라벨이 붙는다. (case 혹은 default) (마지막 부분은 0개 이상의 구문)

##### 4.8.4.1 들여쓰기

다른 블럭과 마찬가지로, switch 블럭의 들여쓰기는 +4 이다.

switch 라벨 이후, 개행이 온다. 그리고 들여쓰기 레벨은 블럭이 열렸을 때와 같이 +4로 증가한다. 뒤 이어 오는 switch 라벨은 블럭이 닫힌 것 처럼 이전의 들여쓰기 레벨과 같이 한다. 

##### 4.8.4.2 실패 혹은 불발: 주석

switch 블럭 이내에 각 구문들은 갑자기 종료될 수 있다. (break, continue, return 혹은 예외) 혹은 다음 구문으로 실행되게 넘어가는것을 표시할 수 있다. 💡`이 불발 상황에 대해 주석을 달 수 있다.` 이 특별한 주석은 switch의 마지막에는 올 필요가 없다. 예를들어,

```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```

case 1 에서는 주석이 없는 것을 보라. 오직 마지막 구문 그룹에만 쓰인다.

##### 4.8.4.3 default 케이스가 존재한다

💡`각 switch 구문은 default 그룹에 존재한다. 그 그룹이 코드를 포함하고 있지 않더라도 말이다.

예외: 💡`enum 타입의 switch 구문은 다른 모든 경우의 처리를 다 했다면 default를 생략 가능하다. `이것은 IDE나 다른 정적인 분석 툴들에게 빠진 것들을 경고하게 한다.

#### 4.8.5 애노테이션

애노테이션은 documentation 블럭 바로 이후에 클래스나 함수 혹은 생성자에 적용된다. 그리고 💡`각 애노테이션들은 그들 만의 줄을 가지고 있다.` (즉 하나의 애노테이션은 한 줄에) 이러한 개행들은 줄 바꿈에 해당되지 않는다. (4.5 절 줄바꿈) 그래서 들여쓰기 레벨이 증가되지 않는다. 예를들어:

```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

예외: 💡`파라미터가 없는 단일 애노테이션은 한줄에 쓸 수 있다.` 예를들어,

```java
@Override public int hashCode() { ... }
```

필드에 적용되는 애노테이션들은 (파라미터가 있을 수 있음) 한 줄에 쓸 수 있다. 예를들어, 

```java
@Partial @Mock DataLoader loader;
```

파라미터나 지역변수 혹은 타입에 적용되는 애노테이션의 특정한 포맷팅 규칙은 없다.

#### 4.8.6 주석

이 부분은 구현 주석에 대한 내용이다. Javadoc은 7절에 나와 있다.

💡`임의의 줄 바꿈 앞에는 임의의 공백 문자와 그 뒤에 구현 주석이 올 수 있다. 이러한 주석은 행을 비어 있지 않게 만든다.`

##### 4.8.6.1 블럭 주석 스타일

💡`블럭 주석 스타일은 둘러 샇인 코드와 같은 들여쓰기 레벨을 가진다.` /* ... */ 이나 // ... 의 스타일을 가진다. / * ... */ 여러 줄이면 뒤이어 오는 줄은 *로 시작해야 하는데 그 이전 *과 맞아야 한다.

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

주석은 별표 또는 기타 문자로 그려진 박스에 넣지 않는다.

팁: 💡`여러 줄의 주석을 작성할 때 문단 형식으로 re-wrap을 하고 싶으면 /* ... */ 의 형식으로 작성한다.` 대부분 포매터들은 // ... 의 re-wrap을 지원하지 않는다.

#### 4.8.7 접근 제한자

클래스와 멤버의 접근 제한자 목록이다.

```java
public protected private abstract default static final transient volatile synchronized native strictfp
```

#### 4.8.8 숫자 리터럴

💡`long의 값을 가지는 정수 리터럴은 대문자 L의 접미사를 가진다.` (소문자가 아닌 이유는 숫자 1과 헷갈리기 때문). 예를들어 3000000000L을 3000000000l 대신에 쓴다.

## 5. 네이밍

### 5.1 모든 식별자에 대한 공통 내용

식별자는 ASCII 숫자와 문자만을 쓴다. 그리고 어떤 일부 경우 언더 스코어( _ )를 쓰기도한다. 그러므로 유효한 식별자 이름은 정규식 \w+와 매칭된다. 

구글 스타일에서는 💡`특별한 접미사나 접두사는 쓰이지 않는다.` 예를들어 name_, mName, s_name, kName은 구글 스타일이 아니다. 

### 5.2 식별자 타입에 대한 룰

#### 5.2.1 패키지 이름

패키지명은 전부 소문자로 단순히 서로 뭍여서 연속된 단어로 이루어져 있다. (언더스코어 없음) 예를들어 com.example.deepspace같은 형식이다. com.example.deepSpace` 혹은 `com.example.deep_space 는 잘못되었다.

#### 5.2.2 클래스 이름

💡`클래스 이름은 UpperCamelCase 이다.` 

💡`클래스 이름은 전형적으로 명사나 명사 구이다.` 예를들어, Character 혹은 ImmutableList 처럼 말이다. 💡`인터페이스의 이름은 명사나 명사구가 될 수 있다.` 예를들어 List. 그러나 가끔은 💡`형용사나 형용사구가 대신 쓰이기도 한다` (예를들어 Readable)

애노테이션 타입에 대한 잘만들어진 규칙같은것은 없다.

💡`테스트 클래스들은 테스트하려는 클래스의 이름이 앞에오고 끝에 Test를 붙여준다.` 예를들어 HashTest 혹은 HashIntegrationTest

#### 5.2.3 함수 이름

💡`함수 이름은 lowerCamelCase 이다.`

💡`함수 이름은 전형적으로 동사 혹은 동사 구이다.` 예를들어, sendMessage 나 stop이다. 언더스코어는 JUnit 테스트에서 논리적 컴포넌트를 분리시키기 위해 각각을 lowerCamelCase로 변경시켜 나올수 있다. 하나의 전형적인 패턴은<methodUnderTest>_<state> 이다. 예를들어 pop_emptyStack. 💡`테스트 메소드를 작성하는 하나의 정확한 방법은 없다.

#### 5.2.4 상수 이름

💡`상수는 CONSTANT_CASE를 사용한다`: 모두 대문자이고 각 단어는 하나의 언더스코어로 구분하는 형식. 하지만 정확히 상수는 무엇인가?

💡`상수는 static final 필드 인데 그것은 변경될 수 없고 그것들의 메소드는 부작용이 보여서는 안된다.` 이것은 원시타입, 문자열 그리고 불변 타입, 불변타입의 불변 컬렉션을 포함한다. 만약 어떤 인스턴스의 상태가 바뀐다면 그것은 상수가아니다. 단지 객체의 상태를 변화시키지 않는 것이 목적은 아니다. 예를들어,

```java
// 상수
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // Joiner가 불변이기 때문
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// 상수 아님
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```

💡`이 이름들은 전형적으로 명사나 명사구이다.`

#### 5.2.5 상수가 아닌 필드의 이름

💡`상수가 아닌 필드 이름은 (static 같은) lowerCamelCase로 작성한다.`

이러한 이름들은 전형적으로 명사나 명사구이다. 예를들어, computedValues 혹은 index.

#### 5.2.6 파라미터 이름

💡`파라미터 이름은 lowerCamelCase 이다.`

💡`public 메서드에서 한개의 문자를 가진 파라미터는 피해야 한다.`

#### 5.2.7 지역변수 이름

💡`지역변수는 lowerCamelCase dlek.`

💡`심지어 final 이나 불변, 지역변수는 상수로 간주되어서는 안되고 상수 스타일로 기술해서도 안된다.`

#### 5.2.8 타입 변수 이름

각 타입들은 두 스타일중 하나를 따른다.

- 💡`하나의 대문자, 혹은 뒤에 하나의 숫자가 따라올 수 있다.` (예를들어, E, T, X, T2)
- 💡`클래스를 위해서 사용되는 (5.2.2절 클래스 이름 참조) 이름의 형식에 T 대문자가 따라오는 형식` (예를들어, RequestT, FooBarT)

### 5.3 캐멀 케이스

가끔 💡`영어 구를 캐멀 케이스로 바꾸는` 이유가 하나 이상 존재한다. 예를들어 두문자어 또는 "IPv6"또는 "iOS"와 같은 비정상적인 구성이있는 경우가 있다. 예측 가능성을 높이기 위해 구글 스타일은 다음과 같은 (거의) 결정 론적 계획을 지정한다.

산문 형태의 이름으로 시작:

1. 구를 일반적인 ASCII로 변환하고 어포스트로피를 없앤다. 예를들어 Müller's algorithm은 Muellers algorithm로 변환.
2. 결과를 단어로 나누고 남은 공백과 구두점으로 나눈다. (일반적으로 하이픈)
   - 추천: 어떤 단어가 이미 전통적인 캐멀 케이스방식이 쓰인다면 이것을 구성하는 부분들로 나눈다. (예를들어 "AdWords"를 "ad words"로). "IOS"는 캐멀케이스가 아니다. 이 부분은 어떠한 약속에도 위배되므로 적용되지 않는다.
3. 이제 모두 lowercase로 바꾸고 (심지어 두음문자도) 그리고 첫 번째 글자를 대문자로 바꾸는데 그 바꾸는 문자들은:
   - 각 단어
   - 각 단어지만 첫 번째를 제외, 첫 번째는 lowercase
4. 이제 하나로 조합한다.

| 산문 형태               | 맞음                                 | 틀림                |
| ----------------------- | ------------------------------------ | ------------------- |
| "XML HTTP request"      | `XmlHttpRequest`                     | `XMLHTTPRequest`    |
| "new customer ID"       | `newCustomerId`                      | `newCustomerID`     |
| "inner stopwatch"       | `innerStopwatch`                     | `innerStopWatch`    |
| "supports IPv6 on iOS?" | `supportsIpv6OnIos`                  | `supportsIPv6OnIOS` |
| "YouTube importer"      | `YouTubeImporter` `YoutubeImporter`* |                     |

*는 권장되지 않음

참고: 영어에서는 모호하게 하이픈이 있는 단어가 몇개 있따. 예를들어, "nonempty" 나 "non-empty"는 둘 다 맞다. 그래서 메소드 이름이 checkNonempty나 checkNonEmpty 둘다 맞다.

## 6. 프로그래밍 연습

### 6.1 @Override: 항상 사용한다

💡`@Override가 사용가능할 때 이 애노테이션을 붙인다.` 이것은 클래스가 슈퍼 클래스의 메서드를 오버라이딩을 하는 것을 나타내기도하고 인터페이스의 메서드를 구현하는 것을 나타내기도 하고 슈퍼 인터페이스의 메서드를 재 구현하는 것을 나타낼 수도 있다.

예외: 부모 함수가 @Deprecated가 되면 @Override를 생략할 수 있다.

### 6.2 💡`예외 잡기: 생략 하지 말것`

아래 명시되있는 것말고 예외를 잡고 아무것도 안하는 것은 거의 있을 수 없다. (전형적인 반응은 로그를 남기는 것 혹은 불가능하다고 간주되면 AssertionError로 다시 던져준다)

정말로 캐치블럭에서 아무것도 하지 않는것이 정당하다면 주석을 남기는것으로 정당화한다.

```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // 숫자가 아니다; 괜찮으니 그냥 넘어간다
}
return handleTextResponse(response);
```

예외: 테스트에서 예외를 잡는 부분은 expected, 혹은 expected로 시작하는 이름을 지으면서 무시할 수 있다. 다음 예제는 테스트에서 예외가 나오는게 확실한 상황에서 사용되는 대중적인 형식으로 주석이 필요가 없다.

```java
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
```

### 6.3 정적 멤버: 클래스를 사용할 수 있음

💡`static 클래스 멤버 참조는 그에 대한 자격이 있어야 하는데 그것은 클래스 타입이 아니라 이름이다.`

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // 좋음
aFoo.aStaticMethod(); // 나쁨
somethingThatYieldsAFoo().aStaticMethod(); // 아주 나쁨
```

## 7. Javadoc

### 7.1 포매팅

#### 7.1.1 일반 형식

Javadoc의 일반적인 형태는 다음과 같다. 

```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```

혹은 한줄의 예는:

```java
/** An especially short bit of Javadoc. */
```

기본 폼은 항상 수용가능하다. 한줄은 Javadoc 블럭이 한 줄에 맞는다면 그렇게 쓸 수 있다. 이것은 @return 같은 블럭 태그가 없을 때 적용가능하다.

#### 7.1.2 문단

한 개의 빈줄 - 그것은 하나의 공백 행 (즉, 정렬 된 선행 별표 (*) 만 포함하는 행)은 단락 사이에, 그리고 블록 태그 그룹 앞에 표시 된다. 첫 번째 단락을 제외한 각 단락은 첫 단어 바로 앞에 <p>가 표시되며 뒤에 공백이 없다.

#### 7.1.3 블럭 태그

표준 "block tag"는 @param, @return, @throws, @deprecated로 나타내지는데 이 네 가지의 타입은 빈 서술이 있으면 안된다. 블럭 태그가 한줄에 입력이 되지 않는다면 다음 줄은 @위치에서 들여쓰기를 4번을 한다.

### 7.2 요약 조각(단편)

각 Javadoc 블록은 간단한 요약 조각으로 시작한다. 이 단편은 매우 중요하다. 클래스 및 메소드 색인과 같은 특정 컨텍스트에 나타나는 텍스트의 유일한 부분이다. 완전한 명사가 아닌 명사구 또는 동사구이다. A {@code Foo} is a ... 또는 This method returns...로 시작하지 않고 또한 Save the record과 같은 완전한 명령형을 구성하지도 않는다. 그러나 조각은 대문자로 표기되고 완전한 문장이다.

팁: 주로 하는 실수: /** @return the customer ID */ 이것은 잘못 되었고 /** Returns the customer ID. */로 바뀌어야 한다.

### 7.3 Javadoc이 어디에 쓰이는지

최소한 Javadoc은 매 public class에 쓰이고 그러한 클래스안에 public, protected 멤버에 쓰인다. 몇몇의 예외는 아래와 같다.

추가 Javadoc 본문은 있을 수 있다. (7.3.4 Javadoc이 필요없는 경우 참조)

#### 7.3.1 예외: 자가-설명 메소드

Javadoc은 간단하고 명료한 메소드, 예를들어 getFoo같은 경우 선택적이다. 그런 경우는 foo를 반환한다 이외에는 도저히 설명할 길이 없는 경우이다.

중요: 그렇다고 해서 독자가 알아야할 정보를 빠트리는 것은 안된다. 예를들어 getCanonicalName의 경우 문서화를 빠트리지 말자. 왜냐하면 일반적 독자는 canonical name이 무슨 뜻인지 모르기 때문이다.

#### 7.3.2 예외: 오버라이드

Javadoc은 슈퍼 타입 메소드를 오버라이드를 한다면 항상 존재하지는 않는다.

#### 7.3.4 Javadoc이 필요없는 경우

다른 클래스 및 멤버는 필요에 따라 또는 원하는대로 Javadoc을 가진다. 

구현 주석이 클래스 또는 멤버의 전반적인 용도 또는 동작을 정의하는 데 사용될 때마다 해당 주석은 대신 Javadoc으로 작성된다.(/ ** 사용).

불필요한 Javadoc은 7.1.2, 7.1.3, 7.2절의 포맷 규칙을 엄격히 준수 할 것을 요구하지는 않지만, 물론 권장된다.


