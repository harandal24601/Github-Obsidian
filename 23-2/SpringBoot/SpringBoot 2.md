## 2.1 스프링과 스프링 부트
	스프링
	 ├─ 자바 기반의 엔터프라이즈 애플리케이션
	 └─ 엔터프라이즈 애플리케이션
	   └─ 대규모 서비스를 뜻함
	스프링 부트
	 └─ 스프링의 복잡한 설정을 쉽게 만들어 출시한 일종의 스핀오프
	   ├─ 빠르게 스프링 프로젝트 설정 가능
	   └─ 스타터를 사용하면 의존성 사용, 관리 용이

> [!quote]  스프링 부트의 주요 특징
> 톰캣, 제티, 언더토우와 같은 웹 애플리케이션(web application server, WAS)가 내장되어 있어서 따로 설치를 하지 않아도 독립적으로 실행할 수 있음.
>  빌드 구성을 단순화하는 스프링 부트 스타터를 제공함.
>  XML 설정을 하지 않고 자바 코드로 모두 작성할 수 있음.
>  JAR를 이용해서 자바 옵션만으로도 배포가 가능함.
>  애플리케이션의 모니터링 및 관리 도구인 스프링 액츄에이터(spring actuator)를 제공함

	차이점 1
	 └─ 구성의 차이 : 스프링은 개발에 필요한 환경을 수동 구성, 스프링 부트는 자동 구성
	 차이점 2
	 └─ 내장 WAS의 유무 : 스프링 부트는 처음부터 WAS를 가지고 있음(tomcat)
	   └─ WAS : 웹 애플리케이션을 실행하기 위한 장치

|                            | 스프링                                                    | 스프링 부트                          |
| -------------------------- | --------------------------------------------------------- | ------------------------------------ |
| 목적                       | 엔터프라이즈 애플리케이션 개발을 더 쉽게 만들기           | 스프링의 개발을 더 빠르고 쉽게 하기  |
| 설정 파일                  | 개발자가 수동으로 구성                                    | 자동 구성                            |
| XML                        | 일부 파일은 XML로 직접 생성하고 관리                      | 사용하지 않음                        |
| 인메모리 데이터베이스 지원 | 지원하지 않음                                             | 인메모리 데이터베이스 자동 설정 지원 |
| 서버                       | 프로젝트를 띄우는 서버(예: 톰캣, 제티)를 별도로 수동 설정 |    내장형 서버를 제공해 별도의 설정이 필요 없음          |

---
## 2.2 스프링 콘셉트 공부하기

	제어의 역전과 의존성 주입
	 ├─ 제어의 역전 = IoC = Inversion of Control
	 │ └─ 객체를 직접 관리하는 것이 아닌 외부에서 관리하는 것
	 ├─ 스프링 컨테이너
	 │ └─ 스프링 안에서 동작하는 빈 생성 및 관리 주체
	 └─ 빈
	   ├─ 스프링 컨테이너가 생성하고 관리하는 객체
	   └─ 빈 등록 방법은 여러가지

![객체 비교|500](https://i.imgur.com/gI0bTLe.png)

### IoC(Inversion of Control) 컨테이너

*IoC*

	 • 결합도와 관련된 개념
	 • 객체의 생성 및 의존 관계에 대한 처리를 소스코드로 처리하지 않고 컨테이너로 처리하는 것
	 • 기존의 자바
	    ├─ 객체를 생성하고 객체들간의 의존관계를 처리하는 것에 대한 책임은 모두 개발자에게 있음
	    └─ 개발자가 어떤 객체를 생성할지 판단하고 객체간의 의존관계를 소스코드로 표현해야 함
	 • 소스에서 객체 생성 및 의존관계에 대한 코드가 사라짐 => 낮은 결합도의 컴포넌트 구현

### 결합도가 높은 프로그램

	결합도
	 ├─ 하나의 클래스가 다른 클래스와 얼마나 많이 연결되었는지를 나타내는 표현
	 └─ 결합도가 높은 프로그램 => 유지보수 힘듬

예)
```Java
// SamsungTV.java

public class SamsungTV {
	public void powerOn() {
		System.out.println("SamsungTV---Power On.");
	}
	public void powerOff() {
		System.out.println("SamsungTV---Power Off.");
	}
	public void volumeUp() {
		System.out.println("SamsungTV---Volume Up.");
	}
	public void volumeDown() {
		System.out.println("SamsungTV---Volume Down");
	}
}
```

``` Java
// LgTV.java

public class LgTV {
	public void turnOn() {
		System.out.println("LgTV---Turn On.");
	}
	public void turnOff() {
		System.out.println("LgTV---Turn Off.");
	}
	public void soundUp() {
		System.out.println("LgTV---Sound Up.");
	}
	public void soundDown() {
		System.out.println("LgTV---Sound Down");
	}
}
```

* 두 TV 클래스를 번갈아 사용하는 TVUser Class
	``` Java
	// TVUser.java

	package polymorphism;

	public class TVUser {
		public static void main(String[] args) {
			SamsungTV tv = new SamsungTV();
			tv.powerOn();
			tv.volumeUp();
			tv.volumeDown();
			tv.powerOff();
		}
	}
	```
	``` Output
	SamsungTV---Turn On.
	SamsungTV---Volume Up.
	SamsungTV---Volume Down.
	SamsungTV---Turn Off.
	```

* LgTV 클래스를 사용하도록 코드 수정 시
	메소드 시그니처가 달라서 TVUser클래스의 대부분 수정 필요
	``` Java
	// TVUser.java
	package polymorphism;

	public class TVUser {
		public static void main(String[] args) {
			LgTV tv = new LgTV();
			tv.turnOn();
			tv.soundUp();
			tv.soundDown();
			tv.turnOff();
		}
	}
	```

### 다형성 이용하기
	다형성(Polymorphism)을 이용한 결합도 낮추기
	 └─ 각 TV 클래스의 interface 제작

![interface|500](https://i.imgur.com/hO3IcO1.png)

``` Java
// TV.java

package polymorphism;

public interface TV {
	public void powerOn();
	public void powerOff();
	public void volumeUp();
	public void volumeDown();
}
```
``` Java
// SamsungTV.java

package polymorphism;

public class SamsungTV implements TV{
	public void powerOn() {
		System.out.println("SamsungTV---Power On.");
	}
	public void powerOff() {
		System.out.println("SamsungTV---Power Off.");
	}
	public void volumeUp() {
		System.out.println("SamsungTV---Volume Up.");
	}
	public void volumeDown() {
		System.out.println("SamsungTV---Volume Down");
	}
}
```
``` Java
// LgTV.java

package polymorphism;

public class LgTV {
	public void powerOn() {
		System.out.println("LgTV---Power On.");
	}
	public void powerOff() {
		System.out.println("LgTV---Power Off.");
	}
	public void volumeUp() {
		System.out.println("LgTV---Volume Up.");
	}
	public void volumeDown() {
		System.out.println("LgTV---Volume Down");
	}
}
```

* TVUser클래스는 interface를 사용하도록 수정
* 그러나 TV 객체를 바꿀 때에는 소스코드를 수정해야 함

``` Java
// TVUser.java

package polymorphism;

public class TVUser {
	public static void main(String[] args) {
		TV tv = new SamsungTV();
		tv.powerOn();
		tv.volumeUp();
		tv.volumeDown();
		tv.powerOff();
	}
}
```

### 디자인 패턴 사용
	factory 패턴 사용
	 ├─ 클라이언트에서 사용할 객체 생성을 캡슐화 함
	 └─ TVUser와 TV 사이를 느슨하게 함


	BeanFactory 클래스
	 └─ 매개 변수에 해당하는 객체를 생성하여 리턴
 ``` Java
// BeanFactory 
package polymorphism;

public class BeanFactory {
	public Object getBean(String beanName) {
		if(beanName.equls("samsung")) { return new SaumsungTV(); }
		else if(beanName.equals("lg")) {return new LgTV(); }
		
		return null;
	}
}
```

``` Java
// TVUser.java

package polymorphism;

public class TVUser {
	public static void main(String[] args) {
		BeanFactory factory = new BeanFactory();
		TV tv = (TV)factory.getBean(args[0]);
		
		tv.powerOn();
		tv.volumeUp();
		tv.volumeDown();
		tv.powerOff();
	}
}
```

* 클라이언트 소스 코드를 수정하지 않고 실행되는 객체 변경
![BeanFactory | 600](https://i.imgur.com/GN8gvdW.png)

---
	의존성 주입 = DI = Dependency Injection
	 ├─ 빈 : 스프링 컨테이너가 관리하는 객체
	 └─ 빈을 스프링 컨테이너로부터 주입 받아 사용(직접 객체를 생성하지 않음)

``` Java
public class A {
	// A에서 B를 주입받음
	@Autowired
	B b;
}
```
![DI | 600](https://i.imgur.com/AhkuoNv.png)

### 스프링의 의존성 관리 방법
	객체의 생성과 의존 관계를 컨테이너가 자동으로 관리 => 스프링 IoC의 핵심원리
	스프링이 지원하는 IoC 형태
	 ├─ Dependency Lookup
	 └─ Dependency Injection

![IoC | 600](https://i.imgur.com/S5ujIoe.png)

	Dependency Lookup
	 ├─ 컨테이너가 애플리케이션 운용에 필요한 객체를 생성하고
	 │  클라이언트는 생성한 객체를 검색(lookup)하여 사용하는 방식
	 ├─ 기존의 컨테이너 사용 방식
	 └─ 실제 응용에서 잘 사용하지 않음

	Dependency Injection
	 ├─ 객체사이의 의존관계를 스프링 설정 파일에 등록된 정보를 바탕으로
	 │  컨테이너가 자동 처리하는 방식
	 ├─ 의존성 설정을 바꾸고 싶을 때 코드를 수정하지 않고 설정 파일을 수정함
	 ├─ Setter Injection
	 │ └─ setter 메소드를 이용
	 └─ Constructor Injection
	   └─ constructor 메소드 사용

### 의존성(Dependency) 관계
	객체와 객체간의 결합 관계
	 └─ 한 객체에서 다른 객체의 변수나 메소드를 사용하고 싶을 떄?
	   └─ 이용하려는 객체에 대한 객체 생성과 생성된 객체에 대한 레퍼런스 정보가 필요

![](https://i.imgur.com/hbagwSa.png)

``` Java
// SonySpeaker.java


package polymorphism;

public class SonySpeaker {
	public SonySpeaker() { System.out.println("===> SonySpeaker Object Created"); }
	public void volumeUp() { System.out.println("SonySpeaker---Volume Up."); }
	public void volumeDown() { System.out.println("SonySpeaker---Volume Down."); }
}
```

``` Java
// SamsungTV.java


package polymorphism;

public class SamasungTV implements TV {
	private SonySpeaker speaker;

	public SamsungTV() { System.out.println("---> SamsungTV Object Created"); }
	
	public void powerOn() { System.out.println("SamsungTV---Power On."); }
	
	public void powerOff() { System.out.println("SamsungTV---Power Off."); }
	
	public void volumeUp() { 
		speaker = new SonySpeker();
		speaker.volumeUp();
	}
	
	public void volumeDown() { 
		speaker = new SonySpeker();
		speaker.volumeDown();
	}
}
```

``` Output
INFO : org.springframework.beans.factory.xml.XmlBeanDefinitionReader -
INFO : org.springframework.context.support.GenericXmlApplicationContext
===> SamsunTV Object Created
SamsungTV---Power On.
===> SonySpeaker Object Created
SonySpeaker---Volume Up.
===> SonySpeaker Object Created
SonySpeaker---Volume Down.
SamsungTV---Power Off.
```

	문제점
	 ├─ SonySpeaker가 쓸데없이 두 개 생성됨
	 └─ SonySpeaker를 AppleSpeaker로 변경 시 volumeIp(), volumeDown() 메소드 수정 필요

### @Component
	@Component만 클래스 위에 선언하면 자동으로 객체 등록 후 생성
	 └─ <context:component-scan/>이 정의되어 있는 경우에 한하여 생성함

	Id 또는 name 설정
	 └─ 클라이언트 프로그램에서 스프링이 만든 객체를 사용하기 위하여 필요함

``` XML
<!-- XML Setting -->
<!-- When set with XML -->

<bean id="tv" class="polymorphism.LgTV"></bean>
```

``` Java
/** Annotation Setting
 *  When set with Annotation
 **/

@Component("tv")
public class LgTV implements TV {
	public LgTV() { System.out.println("===> LgTV Object Create"); }
}
```

``` Output
INFO : org.springframework.beans.factory.xml.XmlBeanDefinitionReader -
INFO : org.springframework.context.support.GenericXmlApplicationContext
===> LgTV Object Created
LgTV---Power On.
LgTV---Volume Up.
LgTV---Volume Down.
LgTV---Power Off.
```

### 의존성 주입 어노테이션

#### 스프링에서 의존성 주입을 지원하는 어노테이션
| 어노테이션 | 설명                                                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------------------- |
| @Autowired | 주로 변수 위에 설정하여 해당 타입의 객체를 찾아서 자동으로 할당<br>org.springframework.beans.factory.annotation.Autowired |
| @Qualifier | 특정 객체의 이름을 이용하여 의존성 주입할 때 사용 <br> org.springframework.beans.factory.annotations.Qualifier            |
| @Inject    | @Autowired와 동일한 기능을 제공 <br> javax.annotation.Resource                                                            |
| @Resource  | @Autowired와 @Qualifier의 기능을 결합한 어노테이션 <br> javax.inject.Inject                                                                  |

	@Autowired와 @Qualifier는 Spring에서 지원

### @Autowired
	생성자, 메소드, 멤버변수 모두 사용 가능
	 └─ 일반적으로 멤버변수에 사용
	기능
	 ├─ 해당 변수의 타입을 검사
	 ├─ 해당 타입의 변수가 메모리에 있는지 확인한 후에 해당 객체를 변수에 주입함
	 └─ 만약 메모리에 없으면 NoSuchBeanDefinitionException 에러발생
``` Output
Casued by : org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [polymorphism.Speaker] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency.
```

``` Java
// LgTV.java

package polymorphism;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component("tv")
public class LgTV implements TV {
	@Autowired
	private Speaker speaker; // Find and assign Speaker-type objects from memory

	public LgTV() { System.out.println("===> LgTV Object Created"); }
	public void powerOn() { System.out.println("LgTV---Power On."); }
	public void powerOff() { System.out.println("LgTV---Power Off."); }

	public void volumeUp() { speaker.volumeUp(); }
	public void volumeDown() { speaker.volumeDown(); }
}
```

	Speaker 객체가 메모리에 있어야 하므로 다음 두 가지 중 하나의 코드가 포함되어 있어야 함.

``` XML
<!-- XML Setting -->
<bean id="sony" class="polymorphism.SonySpeaker"></bean>
```

``` Java
/** Annotation Setting **/

@Component("sony")
public class SonySpeaker implements Speaker {
	public SonySpeaker() { System.out.println("===>SonySpeaker Object Created"); }
}
```

### @Qualifier
	@Autowired와 같이 사용하여 특정 이름의 객체를 주입
	@Autowired를 사용하여 객체 주입 시 클래스 계층 구조에 따라 문제가 발생할 수 있음
	 ├─ 동일한 클래스의 객체가 2개 이상 메모리에 있는 경우
	 └─ 예) SonySpeaker와 AppleSpeaker가 모두 있는 경우

``` Java
// AppleSpeaker.java

package polymorphism;

import org.springframework.stereotype.Component;

@Component("apple")
public class AppleSpeaker implements Speaker {
	public AppleSpeaker() { System.out.println("===> AppleSpeaker Object Created"); }
	// error occurs.

	public void volumeUp() { System.out.println("AppleSpeaker---Volume Up."); }
	public void volumeDown() { System.out.println("AppleSpeaekr---Volume Down."); }
}

```

``` Outputs
Casued by : org.springframework.beans.factory.NoUniqueBeanDefinitionEWxception: No qualifying bean of type [polymorphism.Speaker] is defined: expected single matching bean bit found 2 : apple, sony
```

![@Qualifier|600](https://i.imgur.com/QXOD2Bt.png)

다음과 같이 @Qualifier 사용하여 특정 객체 지정
``` Java
@Component("tv")
public class LgTV implements TV {
	@Autowired
	@Qualifier("apple") // Assign object with ID "apple" among Speaker-type object
	private Speaker speaker;

	public LgTV() { System.out.println("===> LgTV Object Created"); }
	/** 
	omission
	**/
}
```

#### @Resource
	@Resource
	 ├─ @Autowired는 객체의 타입을 기준으로 객체 검색 후 주입 처리
	 └─ @Resource는 객체의 이름을 이용하여 의존성 주입을 처리

``` Java
@Component("tv")
public class LgTV implements TV {
	@Resource(name="apple") // Assign object with ID "apple" among objects of Speaker-type
}
```

### 관점 지향 프로그래밍

	AOP = Aspect Orented Programming
	
	핵심 관점, 부가 관점으로 나누어 프로그래밍하는 것
![AOP | 600](https://i.imgur.com/CHLA1Qc.png)

	AOP 이해하기
	 └─ 엔터프라이즈 애플리케이션 일반적 메서드

``` Java
businessMethod() {
	Logging... // Additional code

	// Core Buisiness Logic(3 ~ 5 Line)

	Exception Handle... // Additional code
	Transaction Handle... // Additional code
	Logging ... // Additional code

	/**
	Additional code => Incresing the complexity of methods => Incresing developer burden
	**/
}
```

	AOP의 핵심 개념 : 관심 분리(Seperation of Concerns)
	
	횡단 관심(Crosscutting Concerns)
	 └─ 메서드마다 공통으로 등장하는 로깅, 예외, 트랜잭션처리 같은 코드
	핵심 관심(Core Concerns)
	 └─ 사용자의 요청에 따라 실제로 수행되는 핵심 비즈니스 로직
	
	기존 OOP언어에서 관심 분리가 어려움

![](https://i.imgur.com/44KhXAO.png)

``` Java
/**
* LogAdvice.java
* Common process code before all business logic runs
* on BoardService components
**/

package com.springbook.biz.common;

public class LogAdvice {
	public void printLog() { 
	System.out.println("[Common Log] Runs before performing business logic.");
	}
}
```

	스프링 AOP
	 ├─ 핵심 관심의 비즈니스 메서드를 호출할 때 횡단 관심에 해당하는 메서드를 적절히 실행
	 └─ 핵심 관심 메서드와 횡단 관심 메서드간의 소스코드 상에서의 결합은 발생하지 않음
	 => AOP를 사용하는 주 목적

#### AOP 용어 정리
	조인포인트(Joinpoint)
	 ├─ 클라이언트가 호출하는 모든 비즈니스 메서드
	 └─ 예) BoardServiceImpl 또는 UserServiceImpl의 모든 메서드

	포인트컷(Pointcut)
	 ├─ 목적에 따라 필터링된 조인포인트
	 ├─ 예) 트랜잭션 처리 공통 기능
	 │  ├─ 등록, 수정, 삭제기능의 비즈니스 메서드에 대해서는 동작 필요
	 │  └─ 검색 기능의 비즈니스 메서드에는 필요없음
	 └─ 개발자가 원하는 특정 메서드에만 횡단관심의 공통기능 수행하기 위하여 필요

![포인트컷 ](https://i.imgur.com/AGOf6VN.png)

	어드바이스(Advice)
	 ├─ 횡단 관심에 해당하는 공통 기능의 코드
	 ├─ 독립된 클래스의 메서드로 작성
	 ├─ 동작 시기 : 스프링 설정 파일을 통해서 지정
	 ├─ 예 : 트랜잭션 관리 기능의 어드바이스
	 │  └─ 비즈니스 로직 수행 후 수행하도록 원함
	 └─ 동작 시점의 지정 : 
	    before, after, after-returning, after-throwing, around 5가지

![](https://i.imgur.com/xZZbgbb.png)

	위빙(Weaving)
	 ├─ 포인트컷으로 지정된 핵심관심 메서드가 호출될 때
	 │  어드바이스에 해당되는 횡단관심 메서드가 삽입되는 과정
	 │  └─ 핵심관심 메서드를 변경하지 않고 횡단관심 메서드를 추가 또는 변경 가능
	 └─ 처리 방식
	    ├─ 컴파일타임위빙, 로딩타임위빙, 런타임위빙
	    └─ 스프링에서는 런타임위빙만 지원함

![](https://i.imgur.com/relXooL.png)

애스팩트(Aspect) 또는 어드바이저(Advisor)

	애스팩트
	 └─ 어떤 포인트컷 메서드에 어떤 어드바이스를 실행할 것인지 결정
	
	<aop:aspect> 엘리먼트 주로 사용
	 └─ 트랜잭션 설정인 경우 : <aop:advisor> 사용

``` Java
// LogAdvice.java

public class LogAdvice {
	public void printLog() {
	System.out.println("[Common Log] Runs before performing business logic.");
	}
}
```

``` XML
<!-- applicationContext.xml -->

<bean id="log" class="com.springbook.biz.common.LogAdvice"></bean>

<aop:config>
	<aop:pointcut id="allPointcut" expression="execution(* com.springbook.biz..*Impl*(..))"/>

	<aop:pointcut id="getPointcut" expression="execution(* com.springbook.biz..*Impl.get*(..))"/>

	<aop:aspect ref="log">
		<aop:before pointcut-ref="getPointcut" method="printLog"/>
	</aop:aspect>
</aop:config>
```


	AOP 용어 종합
	 ├─ 사용자는 여러 비즈니스컴포넌트의 여러 조인포인트를 호출
	 ├─ 특정 포인트컷으로 지정한 메서드가 호출되는 순간
	 ├─ 어드바이스 객체의 어드바이스 메서드가 실행
	 ├─ 어드바이스 메서드의 동작시점을 5가지로 지정할 수 있고 어드바이스 메서드를 삽입하는 설정을 애스팩트라 함
	 └─ 애스팩트 설정에 따라 위빙이 처리

![](https://i.imgur.com/oDqzV7m.png)

### AOP 엘리먼트
	AOP 설정
	 ├─ XML 방식
	 └─ 애너테이션 방식

	<aop:config> 엘리먼트
	 ├─ 루트 엘리먼트
	 ├─ 스프링 설정 파일내에서 여러 번 사용 가능
	 └─ 하위에 <aop:pointcut>, <aop:aspect>가 위치

``` XML
<!-- applicationContext.XML -->

<beans xmlns="http://www.springframework.org/schema/beans" ... >

	<aop:config>
	
		<aop:pointcut .../>
		<aop:aspect ...></aop:aspect>
		
	<aop:config>
	
</beans>
```

#### 스프링에서 제공하는 동작 시점
| 동작 시점 | 설명                                                                                                                                                                                                                                                                                                            |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Before    | - 비즈니스 메서드 실행 전 동작                                                                                                                                                                                                                                                                                  |
| After     | - After Returning : 비즈니스 메서드가 성공적으로 반환되면 동작 <br>  <br> - After Throwing : 비즈니스 메서드 실행 중 예외가 발생하면 동작 <br> &nbsp (try ~ catch 블록에서 catch 블록에 해당) <br>  <br> - After : 비즈니스 메서드가 실행된 후, 무조건 실행 <br> &nbsp (try ~ catch ~ finally 블록에서 finally 블록에 해당) |
| Around    |     Around는 메서드 호출 자체를 가로채 비즈니스 메서드 실행 전후에 처리할 로직을 삽입할 수 있음                                                                                                                                                                                                                                                                                                            |

	Before Advice
	 └─ 포인트컷으로 지정된 메서드 호출 시 메서드가 실행되기 전에 처리될 내용을 기술

![Before Advice | 500](https://i.imgur.com/qqq4vtT.png)

	After return Advice
	 ├─ 포인트 컷으로 지정된 메서드가 정상적으로 실행되고 나서 메서드의 실행 결과가 리턴하는 시점에 동작
	 └─ 비즈니스 메서드의 수행 결과 데이터를 이용한 사후 처리 로직을 추가할 때 사용

![Before return Advice | 500](https://i.imgur.com/Iwl1ylF.png)

	After Throwing Advice
	 ├─ 포인트컷으로 지정한 메서드가 실행하다가 예외가 발생하는 시점에 동작
	 └─ 예외 처리 어드바이스를 설정할 때 사용

![After Throwing Advice | 500](https://i.imgur.com/7cEpcfe.png)

	AOP 관련 어노테이션 코딩
	 └─ 어드바이스 클래스에 코딩
	    └─ 어드바이스 클래스에 선언된 어노테이션을 스프링 컨테이너가 처리하기 위하여
	       반드시 어드바이스 객체가 생성되어야 함
	    1. 스프링 설정 파일에 <bean> 등록 or
	    2. @Service 어노테이션 이용하여 컴포넌트가 검색 가능하게 함
| Annotation 설정 | @Service <br> public class LogAdvice {} |
| --------------- | --------------------------------------- |
|XML 설정                 | `<bean id="log" class="com.springbook.biz.common.LogAdvice"></bean>`       | 


	포인트컷 설정
	 ├─ @Pointcut 사용
	 ├─ 하나의 어드바이스 클래스안에 여러 개의 포인트컷 선언 가능
	 │  └─ 포인트컷 식별을 위한 식별자로 참조메서드 사용
	 └─ 참조 메서드
	    ├─ 메서드 몸체가 비어있는, 즉 구현 로직이 없는 메서드
	    └─ 포인트컷 식별을 위한 이름으로만 사용

``` Java
// LogAdvice.java

import org.aspectj.lang.annotation.Pointcut;

@Service
public class LogAdvice {
	@Pointcut("execution(* com.springbook.biz..*Impl.*(..))")
	public void allPointcut() {}

	@Pointcut("execution(* com.springbook.biz..*Impl.get*(..))")
	public void getPointcut() {}
}
```

	어드바이스 설정
	 ├─ 동작 시기를 결정하는 어노테이션을 메서드 위에 설정
	 └─ 반드시 어노테이션 메서드가 결합될 포인트컷 참조 필요
	    └─ 어드바이스 어노테이션 뒤에 괄호를 추가하고 포인트컷 참조 메서드 지정

``` Java
// LogAdvice.java

import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

@Service
public class LogAdvice {
	@Pointcut("execution(* com.springbook.biz..*Impl.*(..))")
	public void allPointcut() {}

	/** 
	* allPointcut() 참조 메서드로 지정된 비즈니스 메서드가 호출될 때
	* 어드바이스 메서드인 printLog()가 Before 형태로 호출
	**/

	@Before("allPointcut()")
	public void printLog() {
	System.out.println("[Common Log] Runs before performing business logic.");
	}
}
```

---
#### 애스펙트 설정
	애스펙트
	 └─ 포인트컷과 어드바이스의 결합
	@Aspect로 설정
	 └─ 애스펙트 객체에 포인트컷과 어드바이스를 결합하는 설정이 반드시 필요

``` Java
// LogAdvice.java

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

@Service
@Aspect // Aspect = Pointcut + Advice
public class LogAdvice {

	/** 
	Pointcut
	**/
	@Pointcut("execution(* com.springbook.biz..*Impl.*(..))")
	public void allPointcut() {}

	/**
	Advice
	**/
	@Before("allPointcut()")
	public void printLog() {
	System.out.println("[Common Log] Runs before performing business logic.");
	}
}
```

이식 가능한 서비스 추상화

	PSA = Portable Service Abstraction
	스프링에서 제공하는 다양한 기술을 추상화해 개발자가 쉽게 사용하는 인터페이스
	 ├─ 데이터베이스를 위한 PSA : JPA, MyBatisd, JDBC
	 └─ 실행을 위한 PSA : WAS

	한 줄로 정리하는 스프링 핵심 4가지
	 ├─ IoC : 객체의 생성과 관리를 개발자가 하는 것이 아니라 프레임워크가 대신하는 것
	 ├─ DI : 외부에서 객체를 주입받아 사용하는 것
	 ├─ AOP : 프로그래밍을 할 때 핵심 관점과 부가 관점을 나누어서 개발하는 것
	 └─ PSA : 어느 기술을 사용하던 일관된 방식으로 처리하도록 하는 것

![스프링 삼각형 | 400](https://i.imgur.com/QbPHGIA.png)

