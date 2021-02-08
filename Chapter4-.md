Do it! 안드로이드 프로그래밍
====

목차
---
4. [여러 화면 간 전환하기](#4-여러-화면-간-전환하기)    
5.    
6.    
7.    

====

# 4. 여러 화면 간 전환하기
## 레이아웃 인플레이션
안드로이드 앱은 화면 배치를 알려주는 XML 레이아웃 파일 + 화면의 기능을 담당하는 소스코드 파일   
소스파일(MainActivity.java)에서 setContentView() 메서드가 XML 레이아웃 파일을 연결

    setContentView(R.layout.파일명)    //R = res폴더 의미
    
**인플레이션 : XML 레이아웃 내용이 메모리에 객체화되는 과정**    
XML 레이아웃은 앱이 실행되는 시점에 메모리에 객체화된다.   
setContentView() 메서드는 액티비티 화면 전체(main)를 설정하는 역할만 수행할 뿐    
부분 화면(부분 레이아웃)을 메모리에 객체화할 수 없다. -> 인플레이터 사용     

    getSystemService(Context.LAYOUT_INFLATER_SERVICE)           //LayoutInflater 객체 참조 후 사용
    static LayoutInflater LayoutInflater.from (Context context) //or from() 메서드 호출하여 사용
    
    LayoutInflater inflater = (LayoutInflater)                  //Inflater의 inflate() 메서드
            getSystemService(Context.LAYOUT_INFLATER_SERVICE);  
    inflater.inflate(R.layout.sub1, container, true);           //container라는 레이아웃 객체에 sub1 레이아웃 설정 - 객체화
    CheckBox checkBox = container.findViewById(R.id.checkBox);  //객체화 되었으므로 sub1의 뷰 또한 같은 방법으로 참조
    
[ inflate() 메서드 ]

    View inflate (int resource, ViewGroup root) //XML 레이아웃 리소스와 부모 컨테이너를 파라미터로 지정

## 여러 화면 만들고 화면 간 전환하기
