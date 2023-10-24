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
# SamsungTV.java

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
# LgTV.java

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
	# TVUser.java

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
	# TVUser.java
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
# TV.java

package polymorphism;

public interface TV {
	public void powerOn();
	public void powerOff();
	public void volumeUp();
	public void volumeDown();
}
```
``` Java
# SamsungTV.java

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
# LgTV.java

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
# TVUser.java

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
 # BeanFactory 
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
# TVUser.java

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
