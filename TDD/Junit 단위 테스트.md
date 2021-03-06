# 단위 테스트 작성 방법

*망나니 개발자님 블로그 글을 참고하여 정리한 글입니다. *

## 필요 라이브러리


* JUnit5
* AssertJ ; Junit에서 제공하는 메소드가 있지만 ex) assertEquals() 가독성이 떨어진다. 


## given / when / then 패턴

단위테스트에서 사용하는 패턴이다.
* given(준비) : 어떤 데이터가 준비되었을 때,
* when(실행) : 어떤 함수를 실행하면
* then(검증) : 어떤 결과가 나와야 한다. 

   추가적으로 verify로 어떤 메소드가 몇번 호출되었는지 검사하기도 하지만, 실용성이 낮아 메서드 호출 횟수가 중요한 테스트에서만 선택적으로 사용하면 된다고 한다!
   
```
@Displayname("testcode 메서드별 네임을 명시하고 싶을때")
@Test
void testExample(){

//given

//when

//then

}
```
   기본적으로 이러한 구조로 단위 테스트가 진행된다. 


## Mockito 란

Mokito는 **개발자가 동작을 직접 제어할 수 있는 가짜(Mock) 객체를 지원하는 테스트 프레임워크** 이다. Spring으로 웹 애플리케이션을 개발하면서 생기는 여러 의존성이 생기게 되는데, 이는 단위테스트를 어렵게 한다. 이를 해결하기 위해 가짜 객체를 주입시켜주는 Mockito 라이브러리를 사용한다. **Mockito는 가짜 객체에 원하는 결과를 Stub하여 단위테스트를 진행할 수 있게 해준다.**


## Mockito 사용법

### 1. 객체 의존성 주입

* @Mock : Mock 객체를 만들어 반환
* @Spy : Stub 하지 않은 메소드를 원본 그대로 사용
* @InjectMocks " @Mock 또는 @Spy로 생성된 가짜 객체를 자동 주입시켜주는 어노테이션


### 2. Stub로 결과 처리

의존성 있는 객체는 가짜 객체를 주입하여 어떤 결과를 반환하라고 정해진 답을 준비시켜야 한다. Mockito에서는 다음과 같은 stub 메소드를 제공한다. 

* doReturn() : 특정 값 반환
* doNothing() : 아무것도 반환x
* doThrow() : 예외 발생


### 3. Junit과 Mockito 결합

기존의 Junit4에서는 @RunWith(MockitoJUnitRunner.class)를 붙여줘야 했는데, SpringBoot 2.2.0부터 공식적으로 JUnit5를 지원하게 되어 **@ExtenWith(MockitoExtension.class)** 를 명시해주면 된다.













참고 링크:   https://mangkyu.tistory.com/144   
