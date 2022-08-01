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
