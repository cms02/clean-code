## 목차

- [주석은 나쁜 코드를 보완하지 못한다](#1)
- [코드로 의도를 표현하라!](#2)
- [좋은 주석](#3)

  - [법적인 주석](#3-1)
  - [정보를 제공하는 주석](#3-2)
  - [의도를 설명하는 주석](#3-3)
  - [의미를 명료하게 밝히는 주석](#3-4)
  - [결과를 경고하는 주석](#3-5)
  - [TODO 주석](#3-6)
  - [중요성을 강조하는 주석](#3-7)
  - [공개 API에서 Javadocs](#3-8)

- [나쁜 주석](#4)
  - [주절거리는 주석](#4-1)
  - [같은 이야기를 중복하는 주석](#4-2)
  - [오해할 여지가 있는 주석](#4-3)
  - [의무적으로 다는 주석](#4-4)
  - [이력을 기록하는 주석](#4-5)
  - [있으나 마나 한 주석](#4-6)
  - [무서운 잡음](#4-7)
  - [함수나 변수로 표현할 수 있다면 주석을 달지 마라](#4-8)
  - [위치를 표시하는 주석](#4-9)
  - [닫는 괄호에 다는 주석](#4-10)
  - [공로를 돌리거나 저자를 표시하는 주석](#4-11)
  - [주석으로 처리한 코드](#4-12)
  - [HTML주석](#4-13)
  - [전역 정보](#4-14)
  - [너무 많은 정보](#4-15)
  - [모호한 관계](#4-16)
  - [함수 헤더](#4-17)
  - [비공개 코드에서 Javadocs](#4-18)

---

<br>

> 나쁜 코드에 주석을 달지 마라. 새로 짜라. - 브라이언 W. 커니핸, P.J. 플라우거

우리가 프로그래밍 언어를 치밀하게 사용해 의도를 표현할 능력이 있다면 주석은 필요하지 않다. 프로그래머들이 주석을 유지하고 보수하기는 현실적으로 불가능하기 때문에, 주석은 오래될수록 코드에게서 멀어지고, 완전히 그릇될 가능성도 커진다.
우리는 주석을 가능한 줄이도록 꾸준히 노력해야 한다.
<br>
<br>

<a name="1"></a>

## 주석은 나쁜 코드를 보완하지 못한다.

- 코드에 주석을 추가하는 일반적인 이유는 코드의 품질이 나쁘기 때문이다.
- 자신이 저지른 난장판을 주석으로 설명하려 애쓰는 대신에 그 난장판을 깨끗이 치우는 데 시간을 보내라!

<a name="2"></a>

## 코드로 의도를 표현하라!

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((emplotee.flags & HOURLY_FLAG) && (employee.age > 65))
```

```java
if (employee.isEligibleForFullBenefits())
```

- 많은 경우 주석으로 달려는 설명을 함수로 만들어 표현해도 충분하다.

<a name="3"></a>

## 좋은 주석

어떤 주석은 필요하거나 유익하다. 하지만 정말로 좋은 주석은, 주석을 달지 않을 방법을 찾아낸 주석이다.

<a name="3-1"></a>

#### 법적인 주석

- 떄로는 회사가 정립한 구현 표준에 맞춰 법적인 이유로 특정 주석을 넣으라고 명시한다.

```java
// Copyright (C) 2003, 2004, 2005 by Object Montor, Inc All right reserved.
// GNU General Public License
```

<a name="3-2"></a>

#### 정보를 제공하는 주석

```java
// 테스트 중인 Responder 인스턴스를 반환
protected abstract Responder responderInstance();
```

- 위의 코드 또한 함수 이름을 responderBeingTested로 바꾸면 주석이 필요없어 진다.
- 아래는 좀 더 나은 예제이다.

```java
// kk:mm:ss EEE, MMM dd, yyyy 형식이다.
Pattern timeMatcher = Pattern.compile("\\d*:\\d*\\d* \\w*, \\w*, \\d*, \\d*");
```

- 해당 코드 또한 시각과 날짜를 반환하는 클래스를 만들어 코드를 옮기면 주석이 필요 없어진다.

<a name="3-3"></a>

#### 의도를 설명하는 주석

- 떄때로 주석은 구현을 이해하게 도와주는 선을 넘어 결정에 깔린 의도까지 설명한다.

```java
// 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.
for (int i = 0; i > 2500; i++) {
    WidgetBuilderThread widgetBuilderThread =
        new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
    Thread thread = new Thread(widgetBuilderThread);
    thread.start();
}
```

<a name="3-4"></a>

#### 의미를 명료하게 밝히는 주석

- 일반적으로 인수나 반환값 자체를 명확하게 만들면 좋겠지만, 인수나 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속한다면 의미를 명료하게 밝히는 주석이 유용하다.

```java
public void testCompareTo() throws Exception {
    WikipagePath a = PathParser.parse("PageA");
    WikiPagePath ab = PathParser.parse("PageA. PageB");
    Wikipagepath b = PathParser.parse("PageB");
    WikiPagepath aa = PathParser.parse("PageA.PageA");
    WikiPagePath bb = PathParser.parse("PageB. PageB");
    WikipagePath ba = PathParser.parse("PageB. PageA");

    assertTrue(a.compareTo(a) == 0); // a == a
    assertTrue(a.compareTo(b) != 0); // a != b
    assertTrue(ab.compareTo(ab) == 0); // ab == ab
    assertTrue(a.compareTo(b) == -1); // a < b
    assertTrue(aa.compareTo(ab) == -1); // aa < ab
    assertTrue(ba.compareTo(bb) == -1); // ba < bb
    assertTrue(b.compareTo(a) == 1); // b > a
    assertTrue(ab.compareTo(aa) == 1); // ab > aa
    assertTrue(bb.compareTo(ba) == 1); // bb > ba
}
```

- 그릇된 주석을 달아놓을 위험이 상당히 높으므로 더 나은 방법이 없는지 고민하고 정확히 달도록 각별히 주의하자.

<a name="3-5"></a>

#### 결과를 경고하는 주석

```java
// 여유 시간이 충분하지 않다면 실행하지 마십시오.
public void _testWithReallyBigFile() {

}
```

<a name="3-6"></a>

#### TODO 주석

- TODO 주석은 프로그래머가 필요하다 여기지만 당장 구현하기 어려운 업무를 기술한다.
- 그래도 TODO로 떡칠한 코드는 바람직하지 않으므로 주기적으로 TODO 주석을 점검해 없애도 괜찮은 주석은 없애라고 권한다.

```java
// TODO-MdM 현재 필요하지 않다.
// 체크아웃 모델을 도입하면 함수가 필요 없다.
protected VersionInfo makeVersion() throws Exception {
    return null;
}
```

<a name="3-7"></a>

#### 중요성을 강조하는 주석

- 자칫 대수롭지 않다고 여겨질 뭔가의 중요성을 강조하기 위해서도 주석을 사용한다.

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```

<a name="3-8"></a>

#### 공개 API에서 Javadocs

- 설명이 잘 된 공개 API는 참으로 유용하고 만족스럽다. 공개 API를 구현한다면 반드시 훌륭한 Javadocs 작성을 추천한다. 하지만 여느 주석과 마찬가지로 Javadocs 역시 독자를 오도하거나, 잘못 위치하거나, 그릇된 정보를 전달할 가능성이 존재하는 것 역시 잊으면 안 된다.

<a name="4"></a>

## 나쁜주석

<a name="4-1"></a>

#### 주절거리는 주석

```java
public void loadProperties() {
    try {
        String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
        FileInputStream propertiesStream = new FileInputStream(propertiesPath);
        loadedProperties.load(propertiesStream);
    } catch (IOException e) {
        // 속성 파일이 없다면 기본값을 모두 메모리로 읽어 들였다는 의미다.
    }
}
```

- 특별한 이유가 없이 달리는 주석이다.
- catch 블록의 주석을 정확히 이해하려면 다른 코드를 뒤져보는 수 밖에 없다.

<a name="4-2"></a>

#### 같은 이야기를 중복하는 주석

- 헤더에 달린 주석이 같은 코드 내용을 그대로 중복한다면, 코드보다 주석을 읽는 시간이 더 오래 걸릴 수 있다.

```java
// this.closed가 true일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
    if (!closed) {
        wait(timeoutMillis);
        if (!closed) {
            throw new Exception("MockResponseSender could not be closed");
        }
    }
}
```

<a name="4-3"></a>

#### 오해할 여지가 있는 주석

- 위 코드를 다시 보자. 중복이 많으면서도 오해할 여지가 살짝 있다. this.closed가 true로 변하는 순간에 메서드는 반환되지 않는다. this.closed가 true여야 메서드는 반환된다. 아니면 무조건 타임아웃을 기다렸다 this.closed가 그래도 true가 아니면 예외를 던진다. 주석에 담긴 '살짝 잘못된 정보'로 인해 어느 프로그래머가 경솔하게 함수를 호출해 자기 코드가 아주 느려진 이유를 못찾게 되는 것이다.

<a name="4-4"></a>

#### 의무적으로 다는 주석

- 모든 함수에 Javadocs를 달거나 모든 변수에 주석을 달아야 한다는 규칙은 어리석기 그지없다.
- 이러한 주석은 코드를 복잡하게 만들며, 거짓말을 퍼뜨리고, 혼동과 무질서를 초래한다.

```java
/**
 *
 * @param title CD 제목
 * @param author CD 저자
 * @param tracks CD 트랙 숫자
 * @param durationInMinutes CD 길이(단위: 분)
 */
public void addCD(String title, String author, int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = durationInMinutes;
    cdList.add(cd);
}
```

<a name="4-5"></a>

#### 이력을 기록하는 주석

- 현재는 소스 코드 관리 시스템이 있으므로 아래와 같은 주석은 제거하는 것이 바람직하다.

```java
* 변경 이력 (11-Oct-2001부터)
* ------------------------------------------------
* 11-Oct-2001 : 클래스를 다시 정리하고 새로운 패키징
* 05-Nov-2001: getDescription() 메소드 추가
* 이하 생략
```

<a name="4-6"></a>

#### 있으나 마나 한 주석

- 너무 당연한 사실을 언급하며 새로운 정보를 제공하지 못하는 주석이다.

```java
/*
 * 월 중 일자를 반환한다.
 *
 * @return 월 중 일자
 */
public int getDayOfMonth() {
    return dayOfMonth;
}
```

- 위와 같은 주석은 지나친 참견이라 개발자가 주석을 무시하는 습관에 빠지게 한다.

<a name="4-7"></a>

#### 무서운 잡음

- Javadocs도 때로는 잡음이다. 아래의 Javadocs는 어떠한 목적도 수행하지 않는다. 단지 문서를 제공해야 한다는 잘못된 욕심으로 탄생한 잡음이다.
```java
/** The name. */
private String name;

/** The version. */
private String version;

/** The licenceName. */
private String licenceName;

/** The version. */
private String info;
```

<a name="4-8"></a>

#### 함수나 변수로 표현할 수 있다면 주석을 달지 마라.

```java
// 전역 목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if (module.getDependSubsystems().contains(subSysMod.getSubSystem()))
```
- 주석을 제거하면 아래와 같다.
```java
ArrayList moduleDependencies = smodule.getDependSubSystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))
```

<a name="4-9"></a>

#### 위치를 표시하는 주석

```java
// Actions ///////////////////////
```
- 극히 드물지만 위와 같은 배너 아래 특정 기능을 모아놓으면 유용한 경우도 있지만 일반적으로는 이런 주석은 가독성만 낮춘다.
- 특히 슬래시(/)로 이어지는 잡음은 제거하는 것이 좋다.

<a name="4-10"></a>

#### 닫는 괄호에 다는 주석

- 닫는 괄호에 주석을 다는 것은 중첩이 심하고 장황한 함수라면 의미가 있을 수도 있지만 작고 캡슐화된 함수에는 잡음일 뿐이다.
- 닫는 괄호에 주석을 달지 말고 함수를 줄이려 시도하자.
```java
try{
    ...
}//try
catch(){
    ...
}//catch
```

<a name="4-11"></a>

#### 공로를 돌리거나 저자를 표시하는 주석

```java
/* 릭이 추가함 */
```
- 저자 이름으로 코드를 오염시키지 말자.
- 이러한 정보는 소스 코드 관리 시스템에 저장하는 편이 좋다.

<a name="4-12"></a>

#### 주석으로 처리한 코드

- 주석으로 처리한 코드는 다른 사람들이 지우기를 주저한다.
- 소스 코드 관리 시스템이 대신 기억해준다. 그냥 코드를 삭제하자
