a- [들어가면서](#1)

- [의도를 분명히 밝혀라](#2)
- [그릇된 정보를 피하라](#3)
- [의미 있게 구분하라](#4)
- [발음하기 쉬운 이름을 사용하라](#5)
- [검색하기 쉬운 이름을 사용하라](#6)
- [인코딩을 피하라](#7)
- [자신의 기억력을 자랑하지 마라](#8)
- [클래스 이름](#9)
- [메서드 이름](#10)
- [기발한 이름을 피하라](#11)
- [한 개념에 한 단어를 사용하라](#12)
- [말장난을 하지 마라](#13)
- [해법 영역에서 가져온 이름을 사용하라](#14)
- [문제 영역에서 가져온 이름을 사용하라](#15)
- [의미 있는 맥락을 추가하라](#16)
- [불필요한 맥락을 없애라](#17)
- [마치면서](#18)

---

<a name="1"></a>

## 들어가면서

- 소프트웨어에서 이름은 어디나 쓰이므로 이름을 잘 지으면 여러모로 편하다. 이 장에서는 이름을 잘 짓는 간단한 규칙을 몇 가지 소개한다.

<a name="2"></a>

## 의도를 분명히 밝혀라

- `의도가 분명한 이름이 정말로 중요하다`
- (변수나 함수 그리고 클래스) 이름은 `존재 이유, 수행 기능, 사용 방법`에 대한 의미를 내포해야 한다.
- 따로 주석이 필요하다면 의도를 분명히 드러내지 못했다는 말이다.
- 예시 1

```java
//bad
int d; //경과 시간(단위: 날짜)
```

```java
//good
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

- 예시 2

```java
// Bad
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList) {
        if (x[0] == 4) {
            list1.add(x);
        }
    }
    return list1;
}
```

```java
// Good
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
    for (int[] cell : gameBoard) {
        if (cell[STATUS_VALUE] == FLAGGED) {
            flaggedCells.add(cell);
        }
    }
    return flaggedCells;
}
```

```java
// Better
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell : gameBoard) {
        if (cell.isFlagged()) {
            flaggedCells.add(cell);
        }
    }
    return flaggedCells;
}
```

<a name="3"></a>

## 그릇된 정보를 피하라

- 나름대로 널리 쓰이는 의미가 있는 단어를 다른 의미로 사용하면 안된다.
  - hp, aix, sco는 변수 이름으로 적합하지 않다.
- 개발자에게는 특수한 의미를 가지는 단어(List 등)는 실제 컨테이너가 List가 아닌 이상 accountList와 같이 변수명에 붙이지 말자. 차라리 accountGroup, bunchOfAccounts, accounts등으로 명명하자
- 서로 흡사한 이름을 사용하지 않도록 주의한다.

<a name="4"></a>

## 의미 있게 구분하라

- 연속적인 숫자를 덧붙인 이름(a1, a2, ..., aN)은 저자 의도가 전혀 드러나지 않는 이름이다.
- 불용어(noise word)를 추가한 이름은 아무런 정보를 제공하지 못한다.
  - 클래스 이름에 Info, Data를 붙일 경우
    - `ProductInfo, ProductData`
  - 변수명에 `variable` , 테이블 이름에 `table`
  - `Name` VS `NameString`, `money` VS `moneyAmount`, `message` VS `theMessage`
  - `getActiveAccount()` VS `getActiveAccounts()` VS `getActiveAccountInfo()` - 서로의 역할을 구분하기 어려움

<a name="5"></a>

## 발음하기 쉬운 이름을 사용하라

```java
// Bad
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};
```

```java
// Good
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;
    private final String recordId = "102";
    /* ... */
};
```

<a name="6"></a>

## 검색하기 쉬운 이름을 사용하라

- 문자 하나를 사용하는 이름과 상수는 텍스트 코드에서 쉽게 눈에 띄지 않는다.
  - 찾아서 고치지 힘들다.
- 이름의 길이는 범위 크기에 비례해야 한다.

<a name = "7"></a>

## 인코딩을 피해라

#### 접두어를 통해 범위 정보까지 인코딩에 넣지말자.

- 헝가리안 표기법
- 멤버 변수 접두어
- 인터페이스 클래스와 구현 클래스

<a name = "8"></a>

## 자신의 기억력을 자랑하지 마라

- 코드를 읽는 사람이 읽으면서 변수 이름을 자신이 아는 이름으로 변환해야 한다면 좋지 않은 이름이다.
- 루프에서 반복 횟수를 세는 변수 i, j, k는 괜찮다.(단, 루프의 범위가 아주 작고 다른 이름과 충돌하지 않을 경우)
- 이름은 간단하고 명료할수록 좋다.

<a name = "9"></a>

## 클래스 이름

- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다.
- Manager, Processor, Data, Info 와 같은 단어는 피한다.

<a name = "10"></a>

## 메서드 이름

- 메서드 이름은 동사나 동사구가 적합하다.
- 생성자를 중복 정의할 때는 정적 팩토리 메서드를 사용한다.

```java
//Bad
Complex fulcrumPoint = new Complex(23.0);

//Good
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

<a name = "11"></a>

## 기발한 이름은 피하라

- 특정 문화에서만 사용되는 재미있는 이름보다 의도를 분명히 표현하는 이름을 사용하라.
  - whack() => kill()

<a name = "12"></a>

## 한개념에 한 단어를 사용하라

- 추상적인 개념 하나에 단어 하나를 사용하자
  - fetch, retrieve, get
  - controller, manager, driver

<a name = "13"></a>

## 말장난을 하지 마라

- 한 단어를 두 가지 목적으로 사용하지 마라.
- add 라는 메서드를 사용하다가 다른 맥락의 '추가하다' 라는 메서드를 사용하려 한다면 add가 아닌 다른 이름을 사용하여야 한다.

<a name = "14"></a>

## 해법 영역에서 가져온 이름을 사용하라

- 개발자라면 당연히 알고 있을 JobQueue, AccountVisitor(Visitor pattern)등을 사용하지 않을 이유는 없다. 전산용어, 알고리즘 이름, 패턴 이름, 수학 용어 등은 사용하자.

<a name = "15"></a>

## 문제 영역에서 가져온 이름을 사용하라

- 적절한 프로그래머 용어가 없거나 문제 영역과 관련이 깊은 용어의 경우 문제 영역 용어를 사용하자.

<a name = "16"></a>

## 의미 있는 맥락을 추가하라

- 클래스, 함수, namespace 등으로 감싸서 맥락(Context)을 표현하라.
- 그래도 불분명하다면 접두어를 사용하자.

```java
// Bad
private void printGuessStatistics(char candidate, int count) {
    String number;
    String verb;
    String pluralModifier;
    if (count == 0) {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }  else if (count == 1) {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }  else {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }
    String guessMessage = String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );

    print(guessMessage);
}
```

```java
// Good
public class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;

    public String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier );
    }

    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }

    private void thereAreManyLetters(int count) {
        number = Integer.toString(count);
        verb = "are";
        pluralModifier = "s";
    }

    private void thereIsOneLetter() {
        number = "1";
        verb = "is";
        pluralModifier = "";
    }

    private void thereAreNoLetters() {
        number = "no";
        verb = "are";
        pluralModifier = "s";
    }
}
```

<a name = "17"></a>

## 불필요한 맥락을 없애라

- 일반적으로 짧은 이름이 긴 이름보다 좋다. 단, 의미가 분명한 경우에 한해서다. 이름에 불필요한 맥락을 추가하지 않도록 주의한다.

  - Gas Station Delux 라는 어플리케이션의 클래스 이름의 앞에 GSD를 붙이지 말자.

  <a name = "18"></a>

## 마치면서

- 이름을 바꾸는 것에 두려워 하지 말자. 다른 개발자들이 좋은 이름으로 바꿔준다면 감사할 일이다.
- 이름을 개선하면, 단기적인 효과는 물론 장기적인 이익도 보장한다.
