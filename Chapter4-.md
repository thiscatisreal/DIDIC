Do it! 안드로이드 프로그래밍
====

목차
---
4. [여러 화면 간 전환하기](#4-여러-화면-간-전환하기)    
    1. [레이아웃 인플레이션](#레이아웃-인플레이션)      
    2. [여러 화면 만들고 전환하기](#여러-화면-만들고-화면-간-전환하기)   
    3. [인텐트](#인텐트)      
    4. [플래그와 부가 데이터](#플래그와-부가-데이터)      
    5. [태스크 관리](#태스크-관리)       
    6. [액티비티의 수명주기와 SharedPresferences](#액티비티의-수명주기와-SharedPresferences)
5. [프래그먼트](#5-프래그먼트)
    1. [프래그먼트란?](#프래그먼트란)
    2. [프래그먼트로 화면 만들기](#프래그먼트로-화면-만들기)
    3. [액션바와 탭](#액션바와-탭)
    4. [뷰페이저](#뷰페이저)
    5. [바로가기 메뉴](#바로가기-메뉴)
6.    
7.    

****

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
    - ACTION_DIAL tel:01077881234   
        : 주어진 전화번호를 이용해 전화 걸기 화면 보여줌    
    - ACTION_VIEW tel:01077881234   
        : 위와 동일. URL 유형에 따라 다른 기능 수행 가능     
    - ACTION_EDIT content://contacts/people/2   
        : 전화번호부 데이터베이스 정보 중 ID값이 2인 정보를 편집하기 위한 화면 보여줌    
    - ACTION_VIEW content://contacts/people     
        : 전화번호부 데이터베이스의 내용을 보여줌     
            
> 인텐트에 포함된 데이터는 그 포맷이 어떤 것인지 시스템이 확인 후 적절한 액티비티를 자동으로 찾아 보여주기도 한다.
<br> Ex) http같은 특정 포맷 사용 - MIME 타입으로 구분 (일반적으로 웹서버에서 사용하는 MIME 타입)   

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
## 플래그와 부가 데이터
액티비티는 액티비티 매니저라는 객체에 의해 액티비티 스택이라는 것으로 관리된다.    
-> 액티비티를 차곡차곡 쌓아두며 LIFO     
    
BUT 동일한 액티비티를 여러번 실행 - 동일한 액티비티가 여러개 스택에 쌓임 - 문제 발생 !       
    -> 이러한 문제 해결사 = **플래그 !**   
    
        FLAG_ACTIVITY_SINGLE_TOP        //액티비티 생성 시 이미 생성된 액티비티가 존재하면 그거 써라
        FLAG_ACTIVITY_NO_HISTORY        //처음 이후에 실행된 액티비티는 스택에 추가되지 않음
        FLAG_ACTIVITY_CLEAR_TOP         //이 액티비티 위의 다른 액티비티들을 모두 종료
        
### 부가 데이터
번들 객체 안에 넣은 데이터로 시스템에서는 건드리지 않고 다른 앱 구성 요소로 전달      
번들 안에 문자열이나 정수같은 부가 데이터를 넣을 땐 Key, Value를 쌍으로 만들어 넣음    

[ 이진 값을 넣거나 뺄 때 사용하는 대표적인 메서드 ]     

    Intent putExtra(Stirng name, String value)
    Intent putExtra(String name, int value)
    Intent putExtra(String name, boolean value)
    
    String getStringExtra(String name)          //get ~() : 데이터 값이 없으면 디폴트로 설정한 defaultValue로 반환
    int getIntExtra(String name, int defaultValue) 
    boolean getBooleanExtra(String name, boolean defaultValue)
    
- 객체 자료형을 전달하고 싶은 경우    
    1. 바이트 배열로 변환       
    2. Serializable 인터페이스를 구현하는 객체 만들어 직렬화한 다음 전달
      - 안드로이드는 유사한 Parcelable 인터페이스 권장. 다음 메서드를 모두 구현해야 한다.
      
            public abstract int describeContents()      //직렬화하려는 객체의 유형 구분
            public abstract void writeToParcell(Parcel dest, int flags) //객체의 데이터를 Parcel객체로 만들어주는 역할
            
            + CREATOR 상수 정의
[ Parcelable 인터페이스 ]    
- 장점 : 코드가 단순해지고 재사용성 높아짐   
- 단점 : 객체를 일일이 정의해야 하는 번거로움     

## 태스크 관리
Task : 앱이 어떻게 동작할지 결정하는 데 사용     
프로세스처럼 독립적인 실행 단위와 상관없이 어떤 화면들이 같이 동작해야 하는지 흐름 관리

AndroidManifest.xml - <activity> 태그에 launchMode 속성 값을       
singleTask로 설정하면 액티비티가 실행되는 시점에 새로운 태스크를 만들게 되고     
singleInstance로 설정하면 이 액티비티가 실행되는 시점에 새로운 태스크를 만들면서 이후 실행되는 액티비티들은 이 태스크를 공유하지 않도록 한다.      
    
## 액티비티의 수명주기와 SharedPresferences
[ 액티비티의 대표적인 상태 정보 ]    
- 실행(Running)   
    : 화면상에 액티비티가 보이면서 실행되는 상태. 스택의 최상위에 있으며 포커스를 갖고 있음.     
- 일시 정지(Paused)     
    : 사용자에게 보이지만 다른 액티비티가 위에 있어 포커스를 받지 못한 상태. 대화상자 위에 있어 일부가 가려진 경우 해당됨.       
- 중지(Stopped)   
    : 다른 액티비티에 의해 완전히 가려저 보이지 않는 상태.    
    
      수명 주기 : 액티비티의 상태 정보가 변화하는 것   
**onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy**      
- 화면이 보일 때 : onCreat, onStart, onResume 순서로 호출      
- 화면을 없앨 때 : onPause, onStop, onDestroy 순서로 호출      
- 항상 호출되늰 것 : onResume, onPause -> 앱 데이터의 저장과 복원에 필요하기 때문       
    Ex) 게임 중 사용자의 점수가 사라지지 않도록 하려면 onPause()안에 데이터 저장 - onResume()안에서 복원    
- 앱 안에서 간단한 데이터를 저장하거나 복원할 때 = SharedPreferences 사용

****

# 프래그먼트
## 프래그먼트란?
하나의 화면을 여러 부분으로 나눠서 보여주거나 각각의 부분 화면 단위로 바꿔서 보여주고 싶을 때 사용하는 것    
태블릿처럼 큰 화면의 단말을 지원하려고 시작했는데 지금은 단말의 크기와 상관없이 화면 UI를 만들 때 많이 사용      
프래그먼트는 하나의 화면 안에 들어가는 부분 화면과 같아서 하나의 레이아웃처럼 보이지만    
액티비티처럼 독립적으로 동작하는 부분 화면을 만들 때 사용    

[ 프래그먼트의 목적 ]   
- 분할된 화면들을 독립적으로 구성하기 위해 사용     
- 분할된 화면들의 상태를 관리하기 위해 사용   
> 액티비티 화면 = 시스템에서 관리하는 화면
<br> 프래그먼트 화면 = 액티비티 위에 올라가는 화면의 일부, 즉 부분 화면    

[ 프래그먼트 클래스의 주요 메서드 ]   
- public final Activity getActivity()   
    : 이 프래그먼트를 포함하는 액티비티를 반환함.  
- public final FragmentManager getFagmentManager()  
    : 이 프래그먼트를 포함하는 액티비티에서 프래그먼트 객체들과 의사소통하는 프래그먼트 매니저를 반환함.    
- public final Fragment getParentFragment()     
    : 이 프래그먼트를 포함하는 부모가 프래그먼트일 경우 리턴. 액티비티면 null 반환.    
- public final int getid()      
    : 이 프래그먼트의 id 반환.   
    
프래그먼트는 LayoutInflater를 사용해 인플레이션(객체화) 진행해야 한다.      

[ 프래그먼트 매니저 클래스의 주요 메서드 ]   
- public abstract FragmentTransaction beginTransaction()    
    : 프래그먼트 변경을 위한 트랜잭션 시작      
- public abstract Fragment findFragmentById (int id)     
    : ID를 이용해 프래그먼트 객체를 찾음.     
- public abstract Fragment findFragmentByTag (String tag)   
    : 태그 정보를 사용해 프래그먼트 객체를 찾음.      
- public abstract boolean executePendingTransactions()      
    : 트랜잭션은 commit() 메서드 호출시 비동기 방식으로 실행됨.      
      즉시 실행하고 싶다면 이 메서드를 추가로 호출해야 함.    
> getSupportFragmentManager() == getFragmentManager() 같은 기능의 메서드

[ 프래그먼트의 특성 ]       
- 뷰 특성 : 뷰 그룹에 추가되거나 레이아웃의 일부가 될 수 있음   
- 액티비티 특성 : 액티비티처럼 수명주기를 갖고 있음      

[ MainFragment.java 에서 ]    

    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_main, container, false);
    }
    
- 메서드의 파라미터로 LayoutInflater 객체가 전달되므로 inflate()메서드 바로 호출 가능     
- 첫번째 파라미터 : XML 레이아웃 파일    
- 두번째 파라미터 : XML 레이아웃이 설정될 뷰그룹 객체, 프래그먼트의 가장 상위 레이아웃    

[ 프래그먼트를 만들어 사용하는 과정 ]      
1. 프래그먼트를 위한 XML 레이아웃 만들기   
2. 프래그먼트 클래스 만들기    
3. 액티비티를 XML 레이아웃에 추가하기     
    
[ 프래그먼트의 수명주기 메서드 ]     
- onAttach(Activity)    
    : 프래그먼트가 액티비티와 연결될 때 호출됨.   
- onCreate(Bundle)      
    : 프래그먼트가 초기화될 때 호출됨.    
- onCreateView(LayoutInflator, ViewGroup, Bundle)   
    : 프래그먼트와 관련되는 뷰 계층을 만들어서 리턴함.   
- onActivityCreated(Bundle)     
    : 프래그먼트와 연결된 액티비티가 onCreate() 메서드의 작업을 완료했을 때 호출됨.      
- onStart()     
    : 프래그먼트와 연결된 액티비티가 onStart()되어 사용자에게 프래그먼트가 보일 때 호출됨.   
- onResume()    
    : 프래그먼트와 연결된 액티비티가 onResume()되어 사용자와 상호작용할 수 있을 때 호출됨.      
    
[ 프래그먼트가 화면에서 보이지 않을 때 호출되는 상태 메서드 ]    
- onPause()     
    : 프래그먼트와 연결된 액티비티가 onPause()되어 사용자와 상호작용을 중지할 때 호출됨.    
- onStop()      
    : 프래그먼트와 연결된 액티비티가 onStop()되어 화면에서 더이상 보이지 않을 때나 기능이 중지되었을 때 호출됨.   
- onDestroyView()   
    : 프래그먼트와 관련된 뷰 리소스를 해제할 수 있도록 호출됨.      
- onDestroy()       
    : 프래그먼트의 상태를 마지막으로 정리할 수 있도록 호출됨.   
- onDetach()    
    : 프래그먼트가 액티비티와 연결을 끊기 전에 호출됨.   
    
**프래그먼트 객체가 new 연산자로 만들어졌더라도 액티비티에 올라가기 전까지는 동작하지 않음!**

    MyFragment fragment = new MyFragment();     //객체는 만들어졌지만 프래그먼트로 동작하진 X
    getSupportFagmentManager().begin.Transaction().add(fragment).commit();  //동작 O !

## 프래그먼트로 화면 만들기
[ 액티비티를 참조할 때 ]     
- onAttach() 메서드로 전달되는 파라미터 참조      
- getActivity() 메서드를 호출하여 반환되는 객체 참조    
> 액티비티마다 다른 이름의 메서드를 만들면 확인의 번거로움이 있으므로 인터페이스 정의 후 액티비티가 구현하게 하는 것 간편

[ 이미지를 보여주는 프래그먼트 ]     

    public class ViewerFragment extends Fragment {
        ImageView imageView;

        public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle saveInstanceState){
            ViewGroup rootView = (ViewGroup) inflater.inflate(R.layout.fragment_viewer, container, false);

            imageView = rootView.findViewById(R.id.imageView);
            return rootView;
        }

        public void setImage(int resId){
            imageView.setImageResource(resId);
        }
    }
    
1. onCreateView() 메서드 안에서 인플레이션 진행      
2. 이미지뷰 객체를 찾아 변수 할당        
3. setImage() 메서드를 만들어 액티비티에서 설정 가능하게 함     

## 액션바와 탭
### 액션바
- 옵션 메뉴     
    : 시스템 [메뉴] 버튼을 눌렀을 때 나타나는 메뉴로 각 화면마다 설정할 수 있는 주요 메뉴     
- 컨텍스트 메뉴   
    : 화면을 길게 누르면 나타나는 메뉴로 뷰에 설정하여 나타나게 할 수 있다. 텍스트뷰 편집 상태를 바꾸거나 할 때 사용      
> 각각의 액티비티마다 설정 가능

    public boolean onCreateOptionMenu (Menu menu)
    public void onCreateContextMenu (ContextMenu menu, View v, ContextMenu.ContextMenulnfo menuInfo)
    
[ 메뉴 아이템에 추가할 수 있는 대표적인 메서드 ]       
   
    MenuItem add (int groupId, int itemId, int order, CharSequence title)       //groupId : 아이템을 하나의 값으로 묶을 때
    MenuItem add (int groupId, int itemId, int order, int titleRes)             //itemId : 아이템이 갖는 고유 ID 값
    SubMenu addSubMenu (int titleRes)                                           //아이템이 많아서 서브 메뉴로 추가하고 싶을 때
    
[ 아이템 생성 ]  

    <item                                               //하나의 메뉴에 대한 정보 
        android:id="@+id/menu_refresh"
        android:title="새로고침"                        //메뉴에 표시되는 글자
        android:icon="@drawable/menu_refresh"          //icon 표시
        app:showAsAction="always"                      //항상 보이게
        />
        
[ showAsAction 속성 값 ]       
- always : 항상 액션바에 아이템을 추가하여 표시     
- never : 액션바에 아이템을 추가하여 표시하지 않음(default)       
- ifRoom : 액션바에 여유 공간이 있을 때만 아이템 표시     
- withText : title 속성으로 설정된 제목을 같이 표시       
- collapseActionView : 아이템에 설정한 뷰의 아이콘만 표시      

**MainActivity.java에 재정의된 onCreateOptionMenu90 메서드가 액티비티에 만들어질 때 화면에 추가됨**     

[ 기타 메서드 ]      

    boolean onOptionsItemSelected (MenuItem item)       //화면이 띄워진 후 메뉴를 바꾸고 싶을 때 사용
    void Activity.registerForContextMenu (View view)    //컨택스트 메뉴를 특정 뷰에 등록하고 싶을 때 사용
        
