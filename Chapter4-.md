Do it! 안드로이드 프로그래밍
====

목차
---
4. [여러 화면 간 전환하기](#4-여러-화면-간-전환하기)    
    1. [레이아웃 인플레이션](#-레이아웃-인플레이션)      
    2. [여러 화면 만들고 전환하기](#-여러-화면-만들고-화면-간-전환하기)   
    3. [인텐트](#-인텐트)      
    4. 플래그와 부가 데이터      
    5. 테스크 관리       
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
화면을 띄우거나 닫는 과정 == 액티비티를 전환하는 것      
[ 안드로이드 앱의 네 가지 구성 요소 ]
    1. 액티비티     
    2. 서비스      
    3. 브로드캐스트 수신자       
    4. 내용 제공자       
    -> AndroidManifest.xml 파일이 정보들을 담고 있다.      
- startActivity() : 액티비티를 소스코드에서 띄울 때 사용    
- startActivityForResult() : 어떤 액티비티를 띄운 건지, 원래의 액티비티로 돌아올 때 사용     

        startActivityForResult(Intent intent, int requestCode)      //인텐트와 정수로 된 코드값을 전달 받음
                                                                    //코드값은 마음대로 해도 되지만 중복 X
        
[ 액티비티를 새로 추가하면 ]

    <activity android:name=".MenuActivity"                      //AndroidManifest.xml에 자동으로 추가
            android:label="메뉴 액티비티"                        //화면의 제목 설정
            android:theme="@style/Theme.AppCompat.Dialog">      //테마 설정 (Dialog : 대화 상자 형태)
    </activity>
    
[ 버튼을 눌러 원래 액티비티로 돌아가기 ]

    Button button = findViewById(R.id.button);              //버튼 객체 참조
    button.setOnClickListener(new View.OnClickListener(){
        public void onClick(View v){
            Intent intent = new Intent();                   //인텐트 객체 생성
            intent.putExtra("name", "mike");                //name의 값을 부가데이터로 넣기
            setResult(RESULT_OK, intent);                   //응답 보내기
            finish();                                       //현재 액티비티 없애기
        }
        
onActivityResult() : 새로 띄웠던 액티비티가 응답을 보내면 그 응답을 처리하는 역할

    protected void onActivityResult(int requestCode, int resultCode, Intent intent)
1. int requestCode : 액티비티를 띄울 때 전달했던 요청 코드      
2. int resultCode : 새 액티비티로부터 전달된 응답 코드 (정상인지 아닌지 구분)   
3. Intent intent : 새 액티비티로부터 전달 받은 인텐트로 새 액티비티의 데이터 전달할 때 사용    
    - putExtra() 메서드 사용. key와 data 값을 쌍으로 전달.   

[ 앱에 새로운 액티비티를 추가하는 과정 ]        
1. 새로운 액티비티 만들기     
2. 새로운 액티비티의 XML 레이아웃 정의하기      
3. 메인 액티비티에서 새로운 액티비티 띄우기   
4. 새로운 액티비티에서 응답 보내기    
5. 응답 처리하기      
