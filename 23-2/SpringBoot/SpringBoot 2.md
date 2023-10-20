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
		System.out.println("SamsungTV---Turn On.");
	}
	public void turnOff() {
		System.out.println("SamsungTV---Turn Off.");
	}
	public void soundUp() {
		System.out.println("SamsungTV---Sound Up.");
	}
	public void soundDown() {
		System.out.println("SamsungTV---Sound Down");
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