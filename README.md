Do it! 안드로이드 프로그래밍
====

목차
--
### 첫째마당
    1.안드로이드란?
    2.안드로이드 스튜디오
### 둘째마당
    1.
    2.
    3.
    4.
    5.
    
***

# 1.안드로이드란?
  구글에서 만든 스마트폰용 운영체제(OS)이자 앱 플랫폼
  
## 대표적인 특징
- 오픈 소스
- 자바 개발 언어
- 스마트폰을 위한 컴포넌트 제공
- 쉬운 앱 간 연동
- 다양한 기능 지원

## 안드로이드 플랫폼
  버전별로 만들어진 실행 환경으로 PC에선 에뮬레이터, 단말에선 단말의 OS
  안드로이드에선 가상의 플랫폼이라는 의미로 AVD(Android Virtual Device) 용어 사용
  
> 안드로이드 스튜디오 업데이트 :

> 실행 시 시작화면 아래쪽 Configure - Check for Updates 최신 버전 확인 가능 

> 최신 플랫폼 추가 설치시 Configure - SDK Manager 최신 버전 선택 후 설치

# 2.안드로이드 스튜디오
## [ New Project 생성 시 ]
- Name : 영문 대문자로 시작해야 함
    
- Package name : 영문 소문자로 시작해야 함
    - 앱을 구분하는 고유한 값
         > 실무에선 인터넷 사이트 주소처럼 짓는 경우 많음
    - Save location : 프로젝트 저장 위치
    - Language : Java / Kotlin

## [ Android Studio ]
- MainActivity.java : 자바 소스 파일
    - onCreate() : 함수의 시작점 역할 (=main함수)
    - setContentView() : 화면에 뭘 보여줄 지 설정하는 역할
    - R.layout.activity_main : 화면의 구성 요소에 대한 정보

- activity_main.xml : 앱의 첫 화면 자체
    - Design : 실제 스마트폰에 나타날 화면
    - Blue Print : 화면의 구성 요소만 보여주는 화면
    
            화면 안의 요소가 겹쳐 있을 때 투명하게 보고 작업 시에 유용

## [ AVD ]
    - run : Shift + F10
    - Logcat : 로그 확인 가능

* 버튼을 누르는 행위 = 클릭 이벤트로 인식 -> 자바 코드로 이벤트 처리

* 버튼에서 발생한 클릭 이벤트 처리 과정
    - XML 레이아웃 파일의 버튼에 onClick 속성 값 넣기
    
            Attributes - onClick 에서 값 지정 = 버튼 클릭 시 해당 메서드를 찾아 실행하게 됨
    - 소스 파일에 이벤트 처리 함수 추가하기
    
* Toast 클래스 = 작고 간단한 메시지를 잠깐 보여주는 역할

        makeText() , show() 사용

* Intent = 애플리케이션 구성 요소 간에 데이터를 전달하거나 실행하려는 기능을 안드로이드 플랫폼에 알려줌
