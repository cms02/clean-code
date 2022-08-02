- [들어가면서](#1)
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
