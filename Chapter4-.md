Do it! 안드로이드 프로그래밍
====

목차
---
4. [여러 화면 간 전환하기](#4-여러-화면-간-전환하기)    
    1. [레이아웃 인플레이션](#레이아웃-인플레이션)      
    2. [여러 화면 만들고 전환하기](#여러-화면-만들고-화면-간-전환하기)   
    3. [인텐트](#인텐트)      
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

## 인텐트
작업을 수행하기 위해 사용되는 일종의 명령 또는 데이터 전달에 사용       
[ 다른 앱 구성 요소에 인텐트를 전달할 수 있는 대표적인 메서드 ]      

    startActivity() or startActivityForResult()     //액티비티를 화면에 띄울 때 사용
    startService() or bindService()                 //서비스를 시작할 때 사용
    broadcastIntent()                               //객체를 브로드캐스팅 방식으로 전송할 때 사용
    
[ 인텐트의 기본 구성 요소 ]   
    - 액션 : 수행할 기능       
    - 데이터 : 액션이 수행될 대상의 데이터     
[ 대표적인 사용 예 ]   
    - ACTION_DIAL tel:01077881234   //주어진 전화번호를 이용해 전화 걸기 화면 보여줌    
    - ACTION_VIEW tel:01077881234   //위와 동일. URL 유형에 따라 다른 기능 수행 가능     
    - ACTION_EDIT content://contacts/people/2 //전화번호부 데이터베이스 정보 중 ID값이 2인 정보를 편집하기 위한 화면 보여줌    
    - ACTION_VIEW content://contacts/people //전화번호부 데이터베이스의 내용을 보여줌     
인텐트에 포함된 데이터는 그 포맷이 어떤 것인지 시스템이 확인 후 적절한 액티비티를 자동으로 찾아 보여주기도 한다.
Ex) http같은 특정 포맷 사용 - MIME 타입으로 구분 (일반적으로 웹서버에서 사용하는 MIME 타입)   

[ 인텐트의 생성자 ]

    Intent()
    Intent(Intent o)
    Intent(String action [,Uri uri])
    Intent(Context packageContext, Class<?> cls)
    Intent(String action, Uri uri, Context packageContext, Class<?> cls)
- 명시적 인텐트(Explicit Intent)      
    : 인텐트에 클래스 객체나 컴포넌트 이름을 지정하여 호출할 대상을 확실히 알 수 있는 경우      
- 암시적 인텐트(Implicit Intent)      
    : 액션과 데이터를 지정하긴 했지만 호출할 대상이 달라질 수 있는 경우     
    - Category      
        : 액션이 실행되는데 필요한 추가적인 정보 제공      
    - Type      
        : 인텐트에 들어가는 데이터의 MIME 타입을 명시적으로 지정      
    - Component     
        : 인텐트에 사용될 컴포넌트 클래스 이름을 명시적으로 지정    
    - Extra Data    
        : 추가적인 정보를 넣을 수 있는 번들 객체
