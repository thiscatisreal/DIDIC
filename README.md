Do it! 안드로이드 프로그래밍
====

목차
--
### 첫째마당
1. [안드로이드란?](#1-안드로이드란)
2. [안드로이드 스튜디오](#2-안드로이드-스튜디오)
### 둘째마당
1. [View](#1-View)
2. [레이아웃](#2-레이아웃)
3. [기본 위젯과 드로어블](#3-기본-위젯과-드로어블)
4.
5.
    
***

# 첫째마당
## 1. 안드로이드란?
  구글에서 만든 스마트폰용 운영체제(OS)이자 앱 플랫폼
  
### 대표적인 특징
- 오픈 소스
- 자바 개발 언어
- 스마트폰을 위한 컴포넌트 제공
- 쉬운 앱 간 연동
- 다양한 기능 지원

### 안드로이드 플랫폼
  버전별로 만들어진 실행 환경으로 PC에선 에뮬레이터, 단말에선 단말의 OS
  안드로이드에선 가상의 플랫폼이라는 의미로 AVD(Android Virtual Device) 용어 사용
  
> 안드로이드 스튜디오 업데이트 :
<br>실행 시 시작화면 아래쪽 Configure - Check for Updates 최신 버전 확인 가능 
<br> 최신 플랫폼 추가 설치시 Configure - SDK Manager 최신 버전 선택 후 설치

***

## 2. 안드로이드 스튜디오
### [ New Project 생성 시 ]
- Name : 영문 대문자로 시작해야 함
    
- Package name : 영문 소문자로 시작해야 함
    - 앱을 구분하는 고유한 값
        > 실무에선 인터넷 사이트 주소처럼 짓는 경우 많음
    - Save location : 프로젝트 저장 위치
    - Language : Java / Kotlin

### [ Android Studio ]
- MainActivity.java : 자바 소스 파일
    - onCreate() : 함수의 시작점 역할 (=main함수)
    - setContentView() : 화면에 뭘 보여줄 지 설정하는 역할
    - R.layout.activity_main : 화면의 구성 요소에 대한 정보

- activity_main.xml : 앱의 첫 화면 자체
    - Design : 실제 스마트폰에 나타날 화면
    - Blue Print : 화면의 구성 요소만 보여주는 화면
    
            화면 안의 요소가 겹쳐 있을 때 투명하게 보고 작업 시에 유용

### [ AVD ]
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

***

# 둘째마당
## 1. View
    일반적으로 컨트롤이나 위젯으로 불리는 UI 구성 요소
    사용자의 눈에 보이는 화면의 구성 요소들
    모든 뷰는 반드시 크기 속성 가짐
    
- 뷰그룹 : 뷰를 여러개 포함하는 것
- Widget : 일반적인 컨트롤의 역할
- Layout : 뷰그룹 중 내부 뷰들을 포함하며 그것들을 배치하는 역할
- wrap_content : 뷰에 들어 있는 내용물의 크기에 자동으로 맞춤
- match_parent : 뷰를 담고 있는 뷰그룹의 여유 공간을 꽉 채움

### [ 제약 레이아웃 ]
- 뷰의 크기와 위치를 결정할 때 제약 조건을 사용
- 제약 조건 : 뷰가 레이아웃 안의 다른 요소와 어떻게 연결되는지 알려줌

### [ activity_main.xml ]
- 1번째 줄 : XML 파일에 일반적으로 추가하는 정보. 파일 형식을 알려줌
- 2번째 줄 : 화면 전체를 감싸는 레이아웃으로 최상위 레이아웃
    > androidx.constraintlayout.widget = 패키지 이름

    - 태그의 속성 
        
            - xmlns:android : 안드로이드 기본 SDK에 포함된 속성 사용
            - xmlns:app : 프로젝트에서 사용하는 외부 라이브러리에 포함된 속성 사용
            - xmlns:tools : 디자이너 도구 등 화면에서 보여줄 때 사용
            - android:id : 뷰를 구분하는 구분자 역할
            
    - 뷰와 뷰를 연결할 때 XML 속성 이름의 규칙
    
            layoutconstraint[소스 뷰의 연결점][타깃 뷰의 연결점] = "[타깃 뷰의 id]"
            
    - id 속성 정의
    
            @+id/아이디 값
            
    - Guideline의 필수 속성
    
            android:orientation 가로/세로 방향을 지정

### [ 크기 단위 ]
    - px : 픽셀
    - dp(dip) : 160dpi 화면을 기준으로 한 픽셀
    - sp(sip) : 텍스트 크기 지정할 때 사용하는 단위
    - in : 인치
    - mm : 밀리미터
    - em : 글꼴과 상관없이 동일한 텍스트 크기 표시
    
    * 뷰에는 dp, 글자 크기는 sp 단위 사용 권장

### 뷰 영역
- 뷰의 영역(Box) : 뷰는 테두리를 기준으로 바깥쪽과 안쪽 공간을 띄움
- Margin : 테두리의 바깥쪽 공간
- Padding : 테두리의 안쪽 공간
- Cell : Margin + Padding
- Content : 내용물

### 뷰의 배경색
    뷰들을 확실하게 구분해서 표시하고 싶을 때 배경색을 설정
    background 속성 사용
    색상 지정 : #ARGB
    이미지도 지정 가능 (res/drawable 폴더에 저장)
    
### View의 필수 속성

    layout_width, layout_height

> ScrollView :
<br> 하나의 뷰나 뷰그룹을 넣을 수 있고 어떤 뷰의 내용물이 넘치면 스크롤을 만들 수 있게 도와줌

***

## 2. 레이아웃
### 안드로이드에서 제공하는 대표적인 레이아웃
#### ConstraintLayout
    제약 조건(Constraint) 기반 모델
    제약 조건을 사용해 화면을 구성하는 방법
    안드로이드 스튜디오에서 자동으로 설정하는 디폴트 레이아웃
#### [LinearLayout](#리니어-레이아웃)
    박스(Box) 모델
    한 쪽 방향으로 차례대로 뷰를 추가하며 화면을 구성하는 방법
    뷰가 차지할 수 있는 사각형 영역을 담당
#### [RelativeLayout](#상대-레이아웃)
     규칙(Role) 기반 모델
     부모 컨테이너나 다른 뷰와의 상대적 위치로 화면을 구성하는 방법
     제약 레이아웃을 사용하게 되면서 상대 레이아웃은 권장하지 않음
#### [FrameLayout](#프레임-레이아웃)
     싱글(Single) 모델
     가장 상위에 있는 하나의 뷰 또는 뷰그룹만 보여주는 방법
     여러 개의 뷰가 들어가면 중첩하여 쌓게 됨
     가장 단순하지만 여러 개의 뷰를 중첩한 후 각 뷰를 전환하여 보여주는 방식으로 자주 사용함
#### [TableLayout](#테이블-레이아웃)
     격자(Grid) 모델
     격자 모양의 배열을 사용하여 화면을 구성하는 방법
     HTML에서 많이 사용하는 정렬 방식과 유사하지만 많이 사용하지는 않음

***

### 리니어 레이아웃
**필수 속성 = 방향**

    orientation 속성 사용
    가로 방향 : horizontal
    세로 방향 : vertical
    
#### 자바 코드에서의 화면 구성

        public class LayoutCodeActivity extends AppCompatActivity {     //하나의 화면을 액티비티라고 부름

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            LinearLayout mainLayout = new LinearLayout(this);           //new 연산자로 LinearLayout 만들고
            mainLayout.setOrientation(LinearLayout.VERTICAL);           //방향 설정

            LinearLayout.LayoutParams params =                          //레이아웃 안에 추가될 뷰들에
                    new LinearLayout.LayoutParams(                      //설정할 파라미터 생성
                            LinearLayout.LayoutParams.MATCH_PARENT,
                            LinearLayout.LayoutParams.WRAP_CONTENT
                    );

            Button button1 = new Button(this);                          //버튼에 파라미터 설정하고
            button1.setText("Button1");
            button1.setLayoutParams(params);
            mainLayout.addView(button1);                                //레이아웃에 추가

            setContentView(mainLayout);                     //새로 만든 레이아웃을 화면에 설정
        }
    }
    
#### 프로젝트를 처음 만들면
    자동으로 MainActivity.java 생성
    AndroidManifest.xml 파일 안에 자동으로 등록
    -> 새로 추가한 액티비티로 변경하려면 
        AndroidManifest.xml 파일에서
        <activity android:name=".MainActivity"> 부분 수정
#### 뷰 정렬
- layout_gravity    
    : 부모 컨테이너의 여유 공간에 뷰가 모두 채워지지 않아 여유 공간이 생겼을 때 여유 공간 안에서 뷰를 정렬    
- gravity   
    : 뷰 안에 표시하는 내용물을 정렬
1. 버튼을 왼쪽으로 정렬  

        <Button
            android:id="@+id/button4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="left"
            android:text="left" />
2. 텍스트뷰 글자를 왼쪽으로 정렬

        <TextView
            android:id="@+id/textView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="left"
            android:textColor="#ffff0000"
            android:textSize="32sp"
            android:text="left"/>
> 버튼이나 텍스트뷰의 크기를 wrap_content로 지정하면 버튼 안에 들어 있는 글자에 맞게 뷰의 크기가 결정되므로
내부의 여유공간이 없어져서 gravity 속성을 지정하는 게 의미가 없음

- baselineAligned   
    : 텍스트와 옆의 텍스트뷰, 버튼에 들어 있는 텍스트들의 높이를 맞춤  
      but, 텍스트 정렬이 우선이기 때문에 뷰의 배치가 이상해질 수 있음    
      
> 제약 레이아웃에선 뷰의 가운데 연결점을 서로 연결하면 텍스트 높이가 맞춰짐

- layout_weight     
    : 부모 컨테이너에 추가한 뷰들의 공간을 제외한 여유 공간을 분할, 할당 가능

### 상대 레이아웃
**상대적인 위치를 이용해 뷰의 위치 결정**
- 부모 레이아웃의 시작 부분에 붙여라   
    : android:layout_alignParentStart="true"
- 버튼이 다른 버튼의 위쪽까지만 차지하도록 배치     
    : android:layout_**above**="@+id/button2"
- 버튼이 다른 버튼의 아래쪽까지만 차지하도록 배치    
    : android:layout_**below**="@+id/button3"

### 테이블 레이아웃
    표나 엑셀 시트와 같은 형태로 화면 구성
    TableRow Tag = 한 행
    행 안에 들어가는 여러 개의 뷰 = 하나의 열
- TableRow의 높이  
    : 항상 wrap_content로 설정되어 화면을 꽉 채울 수 없다.
- TableRow의 폭   
    : 항상 match_parent로 설정되어 가로 공간을 꽉 채운다 .  
        하나의 행으로 표시하기 위함
- stretchColumns    
    : 부모 컨테이너의 여유 공간을 모두 채우기 위해 각 열의 폭을 강제 확장
    
        android:stretchColumns="0,1,2"      //TableLayout 태그에 추가
                                              숫자는 인덱스를 가리키고 적힌 인덱스가 여유 공간을 차지함
- shrinkColumns     
    : 부모 컨테이너의 폭에 맞추도록 각 열의 폭을 강제 축소
- layout_column     
    : 추가한 순서대로 칼럼 인덱스 값을 자동으로 부여받지만 이 속성으로 인덱스 지정 가능    
- layout_span   
    : 뷰가 여러 칼럼에 걸쳐 있도록 만들기 위한 속성으로 숫자로 지정 가능
    
### 프레임 레이아웃
1. Overlay  
    : 뷰는 추가된 순서대로 차곡차곡 쌓인다.
2. Visibility   
    : 가장 위에 있는 뷰만 보이게 한다.
> XML 레이아웃에서 설정한 id는 소스코드에서 참조 가능
<br> - XML 레이아웃 파일에서 id 지정할 때 -> @+id/아이디
<br> - 자바 소스 코드에서 참조할 때 -> R.id.아이디
#### 스크롤뷰
    추가된 뷰의 영역이 한눈에 다 보이지 않을 때 사용
- MainActivity.java

        public class MainActivity extends AppCompatActivity {
        ScrollView scrollView;
        ImageView imageView;
        BitmapDrawable bitmap;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            scrollView = findViewById(R.id.scrollView);                         //레이아웃에서 정의된 뷰 객체 참조
            imageView = findViewById(R.id.imageView);
            scrollView.setHorizontalScrollBarEnabled(true);                     //수평 스크롤바 사용 기능 설정

            Resources res = getResources();                                     //리소스의 이미지 참조
            bitmap = (BitmapDrawable) res.getDrawable(R.drawable.image01);
            int bitmapWidth = bitmap.getIntrinsicWidth();
            int bitmapHeight = bitmap.getIntrinsicHeight();

            imageView.setImageDrawable(bitmap);                                 //이미지 리소스와 크기 설정
            imageView.getLayoutParams().width = bitmapWidth;
            imageView.getLayoutParams().height = bitmapHeight;
        }
        public void onButton1Clicked(View v) {
            changeImage();
        }
        private void changeImage() {                                            //다른 이미지 리소스로 변경
            Resources res = getResources();
            bitmap = (BitmapDrawable) res.getDrawable(R.drawable.image02);
            int bitmapWidth = bitmap.getIntrinsicWidth();                       //이미지의 가로, 세로 길이 알아냄
            int bitmapHeight = bitmap.getIntrinsicHeight();

            imageView.setImageDrawable(bitmap);
            imageView.getLayoutParams().width = bitmapWidth;                    //알아낸 크기를 객체의 속성으로 설정
            imageView.getLayoutParams().height = bitmapHeight;
        }
***
## 3. 기본 위젯과 드로어블
### 텍스트뷰
- **text**      
    : 텍스트뷰의 문자열을 설정하는 필수 속성     
    1. text 속성 값으로 직접 문자열을 넣는 방법    
    2. /app/res/values 폴더에서 strings.xml 파일에 작성한 문자열을 지정하는 방법 (권장)   
        text 속성에서 @string/... 형식으로 참조
- **textColor**     
    : 문자열의 색상 설정. #AARRGGBB 포맷 사용
- **textSize**      
    : 문자열의 크기 설정 (sp 권장)
- **textStyle**     
    : 문자열의 스타일 속성 설정    
      normal, bold 등 | 기호 공백 없이 사용해서 중복 지정 가능
- **typeFace**      
    : 문자열의 폰트 설정
- **maxLines**      
    : 문자열의 최대 줄 수 설정    
      한 줄로만 표시하고 싶을 때 = 1로 설정하면 영역을 넘어가는 부분은 표시되지 않음
- **button**    
    : 사용자가 클릭하면 클릭에 대해 반응하는 위젯으로 텍스트뷰를 상속하여 정의됨
    - CompoundButton
    
            public boolean isChecked()                  //체크박스, 라디오버튼이 선택됐는지 확인
            public void setChecked (boolean checked)    //체크 상태 지정
            public void toggle()
            
            void onCheckedChanged(CompoundButton buttonView, boolean isChecked) //버튼의 상태가 바뀌는 것을 알려줌
    - RadioButton      
        : 라디오 그룹 - 라디오 버튼 생성
        > LinearLayout에서 가운데정렬 : android:gravity = "center_vertical|center_horizontal"
- **EditText**      
    : 사용자에게 값을 입력받을 때 사용
    > inputType : 입력하는 문자의 유형 지정
    
         <EditText
            android:id= "@+id/usernameInput"
            android:layout_width= "match_parent"
            android:layout_height= "wrap_content"
            android:textSize= "24sp"
            android:inputType= "text"                     //입력되는 글자의 유형 정의
            android:hint= "이름을 입력하세요."/>           //기본 안내문의 hint 표시
- **이미지뷰와 이미지버튼**       
    : 이미지를 화면에 표시할 때 사용하는 가장 간단한 위젯
    - 이미지뷰에 이미지를 나타내는 방법
    1. /app/res/drawable 폴더에 저장한 이미지 파일을 복사 & 붙여넣기      
    2. app:srcCompat 속성 값 = @drawable/이미지파일명 (확장자 제외)   
    - android:src or app:srcCompat      
        : 원본 이미지 설정. 이미지뷰는 내용물이 지정되지 않으면 크기 확인 불가해서 필수로 설정해야 함
    - maxWidth, maxHeight       
        : 이미지가 표시되는 최대 폭, 높이로 설정하지 않으면 원본 이미지 그대로 나타남
    - tint      
        : 이미지뷰에 보이는 이미지의 색상 설정
    - scaleType     
        : 이미지뷰의 크기에 맞게 원본 이미지를 자동으로 늘리거나 줄여서 보여줄 때 사용   
          fitXY, centerCrop 등의 값 사용 가능      
> 해상도별 drawable 폴더 
<br> - 초고해상도 : /app/res/drawable-xhdpi, xxhdpi, xxxhdpi
<br> - 고해상도 : /app/res/drawable-hdpi
<br> - 중간해상도 : /app/res/drawable-mdpi
<br> - 저해상도 : /app/res/drawable_ldpi

   - 커서 관련 속성      
       - selectAllOnFocus : true로 설정 시 포커스를 받을 때 문자열 전체가 선택됨       
       - cursorVisible : false로 설정 시 커서 보이지 않음     
         그 외에도
        
             public int getSelectionStart()                      //선택된 영역의 시작점 알려줌
             public int getSelectionEnd()                        //선택된 영역의 끝점 알려줌. 선택 영역 없을 시 현재 위치 알려줌
             public void setSelection(int start, int stop)       //선택 영역 지정
             public void setSelection(int index)                 
             public void selectAll()                             //전체 문자열 선택
             public void extendSelection(int index)              //선택 영역 확장
         
   - 자동 링크 관련 속성   
        : autoLink 속성을 true로 설정하면 링크 색상을 표시하고 접속 가능케 함
   - 줄 간격 조정 관련 속성     
        - lineSpacingMultiplier : 기본 줄 간격 배수로 설정할 때 사용    
        - lineSpacingExtra : 여유 값으로 설정할 때 사용   
   - 대소문자 표시 관련 속성      
        : capitalize에서 characters, words, sentences 등으로 맨 앞글자를 대문자로 지정 가능
   - 줄임 표시 관련 속성    
        : ellipsize 속성으로 입력 내용의 생략 부분 설정 가능     
            - none : default값으로 뒷부분을 생략     
            - start, middle, end : 각각 시작, 중간, 끝부분을 생략       
            - maxLines : 한 줄로 표시    
   - 힌트 표시 관련 속성    
        : 어떤 내용을 입력하라고 안내문 표시. textColorHint로 색상 지정 가능
   - 편집 기능 관련 속성    
        : EditText를 수정하지 못하게 하고 싶을 때 editable 속성을 false로 설정
   - 문자열 변경 처리 관련 속성    
        - getText() : 입력된 문자 확인. Editable 객체를 리턴함   
                      리턴된 객체를 toString() 메서드를 이용하면 확인 가능
        - TextChangedListener : 사용자의 입력에 의해 바뀔 때마다 확인하는 기능
        
                public void addTextChangedListener(TextWatcher watcher)
                public void beforeTextChanged (CharSequence s, int start, int count, int after)
                public void after TextChanged (Editable s)
                public void onTextChanged (CharSequence s, int start, int before, int count)
        - setFilters() : InputFilter 객체를 파라미터로 전달해서 LengthFilter() 메서드로 입력될 문자열의 길이 값 설정 가능
### 뷰의 배경 이미지
Drawable : 상태에 따라 그래픽이나 이미지가 선택적으로 보이게 함    
버튼의 Background에 이미지를 추가해서 버튼의 모양 바꿀 수 있음
### Drawable
뷰에 설정할 수 있는 객체로 그 위에 그래픽을 그릴 수 있다   
    - BitmapDrawable : 이미지 파일을 보여줄 때 사용     
    - StateListDrawable : 상태별로 다른 그래픽을 참조할 수 있음     
    - TransitionDrawable : 두 개의 Drawable이 서로 바뀌도록 만들 수 있음       
    - ShapeDrawable : 색상과 그라데이션을 포함해서 도형 모양 정의 가능   
    - InsetDrawable : 지정한 거리만큼 안쪽으로 들어오게 할 수 있음     
    - Clip-Drawable : 레벨 값을 기준으로 다른 Drawable들을 클리핑      
    - ScaleDrawable : 레벨 값을 기준으로 다른 Drawable의 크기 변경 가능      
#### 상태 드로어블
가장 많이 사용하는 드로어블 중 하나로 뷰의 상태에 따라 뷰에 보여줄 그래픽을 다르게 지정 가능   
Drawable Layout에서 <selector> - <item> tag에서 이미지나다른 그래픽 설정 가능
    
    <item android:state_pressed="true"                  //뷰가 눌렸을 때 finger.png 보여줌
        android:drawable="@drawable/finger_pressed"/>

    <item android:drawable="@drawable/finger"/>
    
    XML파일에서도 버튼의 background 값을 @drawable/finger_drawable로 변경하면 동일한 효과
#### 셰이프 드로어블
많이 쓰이는 드로어블로 XML로 도형을 그릴 수 있게 해주는 드로어블

    <shape xmlns:android="http://schemas.android.com/apk/res/android"   //최상위 태그 <shape>
        android:shape="rectangle">                                      //속성 rectangle (oval = 타원)

        <size android:width="200dp"         //도형의 크기(가로x세로)
            android:height="120dp"/>
        <stroke android:width="1dp"         //테두리 선(굵기, 색상)
            android:color="#0000ff"/>
        <solid android:color="#aaddff"/>    //도형의 안쪽 채움 색상
        <padding android:bottom="1dp"/>     //아래쪽에만 padding 부여

    </shape>
배경색 그라데이션 효과

    <gradient
        android:startColor="#7288DB"        //시작, 가운데, 끝부분 색상 지정
        android:centerColor="3250B4"
        android:endColor="#254095"
        android:angle="90"
        android:centerY="0.5"
        />

    <corners android:radius="2dp"/>
버튼의 배경을 투명하게 만들어 테두리만 있는 버튼     

    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">     //<layout_list> 태그 사용

    <item>
        <shape android:shape="rectangle">
            <stroke android:width="1dp" android:color="#BE55DA"/>
            <solid android:color="#0000000"/>
            <size android:width="200dp" android:height="100dp"/>
        </shape>
    </item>

    <item android:top="1dp" android:bottom="1dp"                    //뷰의 테두리 선으로부터 바깥으로 얼마만큼 띄울지
        android:right="1dp" android:left="1dp">
        <shape android:shape="rectangle">
            <stroke android:width="1dp" android:color="#FF55DA"/>   //테두리 선
            <solid android:color="#0000000"/>                       //안쪽 투명하게
        </shape>
    </item>

    </layer-list>
