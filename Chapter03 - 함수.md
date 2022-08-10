## 목차

- [작게 만들어라!](#1)
  - [블록과 들여쓰기](#1-1)
- [한 가지만 해라!](#2)
  - [함수 내 섹션](#2-2)
- [함수 당 추상화 수준은 하나로!](#3)
  - [위에서 아래로 코드 읽기 : 내려가기 규칙](#3-1)
- [Switch문](#4)
- [서술적인 이름을 사용하라!](#5)
- [함수 인수](#6)
  - [많이 쓰는 단항 형식](#6-1)
  - [플래그 인수](#6-2)
  - [이항 함수](#6-3)
  - [삼항 함수](#6-4)
  - [인수 객체](#6-5)
  - [인수 목록](#6-6)
  - [동사와 키워드](#6-7)
- [부수 효과를 일으키지 마라!](#7)
  - [출력 인수](#7-1)
- [명령과 조회를 분리하라!](#8)
- [오류코드보다 예외를 사용하라!](#9)
  - [Try/Catch 블록 뽑아내기](#9-1)
  - [오류 처리도 한 가지 직업이다](#9-2)
  - [Error.java의 의존성 자석](#9-3)
- [반복하지마라!](#10)
- [구조적 프로그래밍](#11)
- [함수를 어떻게 짜죠?](#12)

---

<a name="1"></a>

## 작게 만들어라!

- 함수를 최대한 `작게!` 만들어라!

<a name="1-1"></a>

#### 블록과 들여쓰기

- 중첩 구조(if/else, while 문 등)에 들어가는 블록은 한 줄이어야 한다.
- 각 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안된다.
- 각 함수가 명백하다면 더욱 읽고 이해하기 쉬워진다.

<a name="2"></a>

## 한 가지만 해라!

- `함수는 한 가지를 해야 한다. 그 한 가지를 잘 해야 한다. 그 한 가지만을 해야 한다.`
- 지정된 함수 이름 아래에서 추상화 수준이 하나인 단계만 수행한다면 그 함수는 한 가지 작업만 하는 것이다.

<a name="2-1"></a>

#### 함수 내 섹션

- 함수를 여러 섹션으로 나눌 수 있다면 그 함수는 여러 작업을 하는 셈이다.

<a name="3"></a>

## 함수 당 추상화 수준은 하나로!

- 함수가 '한가지' 작업만 하려면 함수 내 모든 문장의 추상화 수준이 동일해야 한다.
- 만약 한 함수 내에 추상화 수준이 섞이게 된다면 읽는 사람이 헷갈린다.

<a name="3-1"></a>

#### 위에서 아래로 코드 읽기: 내려가기 규칙

- 코드는 위에서 아래로 이야기처럼 읽혀야 좋다.
- 위에서 아래로 읽으면서 함수 추상화 부분이 한번에 한단계씩 낮아지는 것이 이상적이다.(내려가기 규칙)

<a name="4"></a>

## Switch 문

```java
public Money calculatePay(Employee e) throws InvalidEmployeeType {
	switch (e.type) {
		case COMMISSIONED:
			return calculateCommissionedPay(e);
		case HOURLY:
			return calculateHourlyPay(e);
		case SALARIED:
			return calculateSalariedPay(e);
		default:
			throw new InvalidEmployeeType(e.type);
	}
}
```

- Switch 문은 작게 만들기 어렵다(if/else도 마찬가지).
- 다형성을 이용하여 Switch문을 추상 팩토리(Abstract Factory)에 숨겨 다형적 객체를 생성하는 코드 안에서만 Switch 문을 사용하도록 하자.

```java
public abstract class Employee {
	public abstract boolean isPayday();
	public abstract Money calculatePay();
	public abstract void deliverPay(Money pay);
}
-----------------
public interface EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType;
}
-----------------
public class EmployeeFactoryImpl implements EmployeeFactory {
	public Employee makeEmployee(EmployeeRecord r) throws InvalidEmployeeType {
		switch (r.type) {
			case COMMISSIONED:
				return new CommissionedEmployee(r) ;
			case HOURLY:
				return new HourlyEmployee(r);
			case SALARIED:
				return new SalariedEmploye(r);
			default:
				throw new InvalidEmployeeType(r.type);
		}
	}
}
```

<a name="5"></a>

## 서술적인 이름을 사용하라!

- 함수가 작고 단순할수록 서술적인 이름을 고르기 쉬워진다.
- 서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해진다.
- 단, 이름을 붙일 때는 일관성이 있어야 한다.

<a name="6"></a>

## 함수 인수

- 함수에서 이상적인 인수 개수는 0개(무항)이다. 차선은 1개뿐인 경우이다.

<a name="6-1"></a>

#### 많이 쓰는 단항 형식

- 인수에 질문을 던지는 경우
  - `boolean fileExists("MyFile");`
- 인수를 뭔가로 변환해 결과를 반환하는 경우
  - `InputStream fileOpen(“MyFile”);`
- 이벤트 함수일 경우

위의 경우들이 아니라면 단항 함수는 가급적 피하는 것이 좋다.

<a name="6-2"></a>

#### 플래그 인수

- `플래그 인수란 함수가 실행하는 로직을 선택하기 위해 전달하는 인수이다`
- 플래그 인수는 `절대 쓰지말자`. 추하다.
- 함수가 한꺼번에 여거 가지를 처리한다고 대놓고 공표하는 셈이다.

<a name="6-3"></a>

#### 이항 함수

- 단항 함수보다 이해하기 어렵다.
- 물론 이항 함수가 적절한 경우도 있다.
  - `Point p = new Point(0,0)`
- 가능한 단항 함수로 바꾸도록 노력하자.

<a name="6-4"></a>

#### 삼항 함수

- 이항 함수보다 훨씬 더 이해하기 어렵다.
- 삼항 함수를 만들 때는 신중이 고려하자.

<a name="6-5"></a>

#### 인수 객체

- 인수가 많이 필요할 경우, 일부 인수를 독자적인 클래스 변수로 선언할 가능성을 살펴보자.
  - `Circle makeCircle(double x, double y, double radius)`
  - `Circle makeCircle(Point center, double radius)`

<a name="6-6"></a>

#### 인수 목록

- 때로는 String.format 같은 가변적인 함수도 필요하다.
- `String.format("%s worked %.2f" hours., name, hours);`

<a name="6-5"></a>

#### 동사와 키워드

- 단항 함수는 함수와 인수가 동사/명사 쌍을 이뤄야한다.
  - `writeField(name)`
- 함수 이름에 `키워드(인수 이름)`를 추가하면 인수 순서를 기억하지 않아도 된다.
  - `assertExpectedEqualsActual(expected, actual);`
