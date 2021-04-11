Do it! 안드로이드 프로그래밍
====

목차
---
7. [선택 위젯 만들기](#7-선택-위젯-만들기)    
    1. [나인패치 이미지](#나인-패치-이미지)       
    2. [새로운 뷰 만들기](#새로운-뷰-만들기)      
    3. [레이아웃 정의하고 카드뷰 넣기](#레이아웃-정의하고-카드뷰-넣기)        
    4. [리싸이클러뷰](#리싸이클러뷰)    
    5. [스피너](#스피너)      
8. [애니메이션과 다양한 위젯](#8-애니메이션과-다양한-위젯)        
    1. [애니메이션](#애니메이션)      
    2. [페이지 슬라이딩](#페이지-슬라이딩)        
    3. [앱 화면에 웹브라우저 넣기](#앱-화면에-웹브라우저-넣기)        
    4. [시크바](#시크바)      
    5. [키패드 제어](#키패드-제어)      
9. [스레드와 핸들러 이해하기](#9-스레드와-핸들러-이해하기)
    1. [핸들러](#핸들러)
    2. [일정시간 후에 실행하기](#일정시간-후에-실행하기)
    3. [스레드로 메시지 전송하기](#스레드로-메시지-전송하기)
    4. [AsyncTask](#AsyncTask)
    5. [스레드로 애니메이션 만들기](#스레드로-애니메이션-만들기)

---
# 7. 선택 위젯 만들기
## 나인패치 이미지
이미지뷰를 XML 레이아웃에 추가해서 화면에 보여줄 때 이미지가 나타나는 영역보다 원본 이미지가 작으면 시스템이 이미지 크기를 자동으로 늘려준다. 이 기능은 해상도가 다른 단말에서도 일정한 비율로 이미지의 크기를 지정하면 이미지가 자동으로 그 크기에 맞게 늘어나거나 줄어들게 하므로 유용한 기능 ! BUT, 이 과정에서 이미지의 일부분이 깨져보이거나 왜곡이 발생하는 문제 발생      
=> **나인 패치로 해결 !**      
[ 나인 패치 이미지 만드는 방법 ]        
1. png나 jpg와 같은 이미지보다 가로, 세로 크기가 2px씩 큰 이미지를 새로 만든다.        
2. 원본 이미지를 가운데에 복사한다.   
3. 수정한 이미지 파일을 ~.9.png로 파일 확장자 앞에 '.9'를 붙여 저장한다.    
4. 가장자리 한 픽셀씩을 흰색 또는 검은색으로 변경 가능 !      

[ 뷰의 배경으로 색상과 이미지를 지정하는 메서드 ]       

    void setBackgroundColor (int color)
    void setBackgroundDrawable (Drawable d)
    void setBackgroundResource (int resid)      //XML 레이아웃의 background 속성과 같음
    
## 새로운 뷰 만들기

    public void onMeasure (int widthMeasureSpec, int heightMeasureSpec)      //뷰가 스스로의 크기를 정할 때 자동으로 호출되는 메서드
    public void onDraw (Canvas canvas)                                       //스스로를 레이아웃에 맞게 그릴 때 호출되는 메서드
    
- onMeasure() 메서드의 파라미터로 전달되는 것 : 뷰에게 허용되는 여유 공간과 폭의 넓이에 대한 정보      
- 레이아웃에게 뷰의 크기를 반환하고 싶다면

      void setMeasuredDimension (int measuredWidth, int measuredHeigt)
      
### 새로운 뷰를 만든다는 것은 ...
새로운 뷰를 클래스로 정의하고 그 안에 onDraw() 메서드를 다시 정의한 후 필요한 코드를 넣어 기능을 구현하면 다른 모양으로 보이는 뷰 생성 !     
이때 onDraw() 메서드는 새로 정의한 뷰가 화면에 보이기 전에 호출되므로, 메서드 안에서 원하는 모양의 그래픽을 화면에 그리면 그 모양대로 화면에 표현 가능!     

### 새로운 클래스에서 ...
- 안드로이드 UI 객체를 만들 때 Context 객체를 전달받도록 되어 있으므로 생성자는 항상 Context 객체가 전달되어야 한다.     
- 파라미터 AttributeSet : XML 레이아웃에서 태그에 추가하는 속성을 전달받기 위한 것
- 초기화를 위한 메서드 정의
 
        private void init(Context context){
            setBackgroundColor(Color.CYAN);
            setTextColor(Color.BLACK);

            float textSize = getResources().getDimension(R.dimen.text_size);
            setTextSize(textSize);
        }
- values/dimens.xml : 크기 값 등을 정할 수 있는 파일

        <?xml version="1.0" encoding="utf-8" ?>
        <resources>
            <dimen name="text_size">16sp</dimen>
        </resources>
    
[ 버튼이 눌렸을 때 색깔 바꾸기 ]    

    public boolean onTouchEvent(MotionEvent event) {
        int action = event.getAction();                 //손가락으로 눌린 상태를 전달 받음

        switch(action) {
            case MotionEvent.ACTION_DOWN:               //누르고 있을 때
                setBackgroundColor(Color.BLUE);         //배경 파란색, 글자 빨간색으로 변경
                setTextColor(Color.RED);
                break;                                  //멈춤
                
            case MotionEvent.ACTION_OUTSIDE:            //그 외의 액션들
            case MotionEvent.ACTION_CANCEL:
            case MotionEvent.ACTION_UP:
                setBackgroundColor(Color.CYAN);         //배경 옅은 파란색, 글자 검은색으로 변경
                setTextColor(Color.BLACK);

                break;
        }
        invalidate();                                   //뷰 다시 그림 -> onDraw() 메서드 동작
        return true;
    }
## 레이아웃 정의하고 카드뷰 넣기
카드뷰 : 프로필과 같은 간단 정보를 넣기 위해 각 영역을 구분하는 역할        

[ LinearLayout class를 상속하는 새로운 클래스 정의하기 ]       

- 초기화 : XML 레이아웃 파일을 인플레이션해서 이 소스파일과 매칭될 수 있도록 한다.

        private void init(Context context){
            LayoutInflater inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            inflater.inflate(R.layout.layout, this, true);
            
            imageView = findViewById(R.id.imageView);       //XML 레이아웃에서 정의했던 뷰 참조
            textView = findViewById(R.id.textView);
            textView2 = findViewById(R.id.textView2);
        }
- 새로 정의한 뷰는 메인 레이아웃에 추가되어 사용할 것이므로 소스코드에서 이미지뷰의 이미지나 텍스트뷰의 글자를 바꿀 수 있도록 메서드 정의

        public void setImage(int resId){
            imageView.setImageResource(resId);          //drawable 속 이미지 파일 참조하는 정수 값을 파라미터로 받음
        }

        public void setName(String name){
            textView.setText(name);
        }

        public void setMobile(String mobile){
            textView2.setText(mobile);
        }
- onCreate() 메서드에서

        Layout1 layout1 = findViewById(R.id.layout1);               //XML레이아웃에 추가한 뷰 참조
        
        layout1.setImage(R.drawable.ic_launcher_foreground);        //뷰의 메서드 호출하여 데이터 설정
        layout1.setName("김민수");
        layout1.setMobile("010-1000-1000");

## 리싸이클러뷰
리스트 : 모바일 단말에서 가장 많이 사용되는 UI 모양 중 하나로 여러 아이템 중 하나를 선택할 수 있는 세로 모양으로 된 화면 컨트롤
> 스마트폰의 사용성을 높이기 위해 !
- 안드로이드에서는 위와 같은 리스트를 선택 위젯이라고 부름 !     
- Selection Widget : 원본 데이터를 뷰에 직접 넣지 않고 어댑터라는 클래스를 사용함     
- getView() : 어댑터에서 가장 중요한 메서드로 이 메서드에서 반환하는 뷰가 하나의 아이템으로 디스플레이된다.      
> 반환하는 객체가 텍스트뷰면 선택 위젯의 각 아이템은 텍스트뷰로 표시됨.
<br> 레이아웃처럼 컨테이너 객체라면 하나의 아이템에 여러 정보를 보여줄 수 있음.     

**리싸이클러뷰 : 상하스크롤이나 좌우스크롤 사용 가능. 캐시 매커니즘으로 구현**      

[ 어댑터에서 구현되어야 하는 중요한 메서드 ]      
- getItemCount() : 어댑터에서 관리하는 아이템의 개수 반환    
- onCreateViewHolder() : 각 아이템을 위해 정의한 XML 레이아웃을 이용해 뷰 객체 생성        
- onBindViewHolder() : 뷰홀더가 재사용될 때 호출되며 뷰객체는 기존 것을 그대로 사용하고 데이터만 바꿔줌
> 두 메서드는 뷰홀더 객체가 만들어질 때와 재사용될 때 자동으로 호출된다.
<br> 뷰 객체를 재사용하는 이유 : 메모리를 효율적으로 사용하기 위해

- 어댑터는 리싸이클러뷰 객체에 설정되어야 하고, 어댑터 안에 객체들을 만들어 넣어야 하므로 MainActivity에       

        LinearLayoutManager layoutManager = new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, false);
        recyclerView.setLayoutManager(layoutManager);               //리싸이클러뷰에 레이아웃 매니저 설정
        
        recyclerView.setAdapter(adapter);                           //리싸이클러뷰에 어댑터 설정
        
- 레이아웃 매니저 : 리싸이클러뷰가 보일 기본적이 형태 설정. 세로, 가로, 격자모양을 자주 사용함.       
    - VERTICAL : 세로     
    - HORIZONTAL : 가로   
    - GridLayoutManager : 격자. 칼럼수 지정

            GridLayoutManager layoutManager = new GridLayoutManager(this, 2);      //두 칼럼으로 표시되는 격자 레이아웃
- 사용자가 각 아이템을 클릭했을 때 토스트 메시지가 표시되려면     
 : 뷰홀더 안에서 클릭이벤트를 처리할 수 있도록. 어댑터 객체 밖에서 리스너를 설정하고 설정된 리스너 쪽으로 이벤트 전달받도록.    
 
## 스피너
: 윈도우에선 콤보박스로 불린다. 아이폰이나 안드로이드 단말에서는 쉽게 터치할 수 있도록 별도의 창으로 선택할 수 있는 데이터 아이템들로 표현된다.      

    spinner.setAdapter(adapter);        //스피너에 어댑터 설정하기
     spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {       //스피너에 리스너 설정하기
            public void onItemSelected(AdapterView<?> adapterView, View view, int position, long id){
                textView.setText(items[position]);
            }
            
- 스피너 객체도 선택 위젯이므로 setAdapter() 메서드의 파라미터로 어댑터 객체를 전달해야 한다.     
- API 제공 기본 어댑터 ArrayAdapter를 사용하면 simple_spinner_item이라는 레이아웃 지정(문자열을 아이템으로 보여주는)      
- setDropDownViewResource() : 스피너 항목을 선택하는 창을 위한 레이아웃   

        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_spinner_item, items);
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);

---
    
# 8. 애니메이션과 다양한 위젯  
## 애니메이션
### 트윈 애니메이션
안드로이드에서 간단하게 움직이거나 확대/축소를 가능하게 하는 애니메이션     
### 사용하기
1. 애니메이션이 어떻게 동작할지 XML로 정의      
2. XML 파일을 자바 소스에서 애니메이션 객체로 로딩 -> startAnimation() 메서드를 사용해 애니메이션 동작       
### 대상과 효과
[ 대상 ]      
    - 뷰 : 위젯과 레이아웃 모두 포함    
    - 그리기 객체 : 다양한 드로어블에 적용 가능. ShapeDrawable, BitmapDrawable 등     
[ 효과 ]      
    - 위치 : Translate로 정의하면 대상의 위치 이동        
    - 확대/축소 : Scale로 정의하면 대상의 크기를 키우거나 줄이는 데 사용      
    - 회전 : Rotate로 정의하면 대상을 회전시키는 데 사용      
    - 투명도 : Alpha로 정의하면 대상의 투명도 조절에 사용      

### 1.XML 리소스 정의
애니메이션을 위한 XML파일은 /app/res/anim 폴더 밑에 둬야 한다.     
Animation Set으로 여러 개의 효과를 하나로 묶어 동시 수행되도록 하는 게 가능하다.        
애니메이션이 끝난 후 똑같이 거꾸로 적용하면 자연스럽게 원상복귀 가능하다.       
[ scale.xml ]

    <scale
        android:duration="2500"         //지속시간. startOffset : 애니메이션이 시작되기까지의 시간(없으면 즉시 시작)
        android:pivotX="50%"            //크기를 변경하려는 축의 정보
        android:pivotY="50%"
        android:fromXScale="1.0"        //시작할 때의 확대/축소 비율
        android:fromYScale="1.0"
        android:toXScale="2.0"          //끝날 때의 확대/축소 비율
        android:toYScale="2.0"
        />
        
[ translate.xml ]

    <translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromXDelta = "0%p"      //시작위치
    android:toXDelta="-100%p"       //종료위치 (왼쪽으로)
    android:duration="20000"        //20초
    android:repeatCount="-1"        //무한반복
    android:fillAfter="true"        //애니메이션이 끝난 후 대상이 원래대로 돌아오는 것을 막기 위함
    />
    
[ rotate.xml ]

    <rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"         //회전 각도. 0 -> -360 : 왼쪽으로 한바퀴
    android:toDegrees="360"
    android:pivotX="50%"            //대상의 중심축
    android:pivotY="50%"
    android:duration="10000"        //10초
    >

[ alpha.xml ]

    <alpha
        android:fromAlpha="1.0"
        android:toAlpha="0.0"
        android:duration="10000"
        />
        
### 2.애니메이션 객체에 로딩

    Animation anim = AnimationUtils.loadAnimation(getApplicationContext(),R.anim.scale);
    
### 인터폴레이터
애니메이션 효과가 지속되는 동안 속도 조절     
- accelerate_interpolator : 점점 빠르게      
- decelerate_interpolator : 점점 느리게      
- accelerate_decelerate_interpolator : 점점 빠르다가 느리게      
- anticipate_interpolator : 시작 위치에서 조금 뒤로 당겼다가 시작하도록        
- overshoot_interpolator : 종료 위치에서 조금 지나쳤다가 종료되도록       
- anticipate_overshoot_interpolator : 시작 위치에서 조금 뒤로 당겼다가 시작 & 종료 위치에서 조금 지나쳤다가 종료되도록    
- bounce_interpolator : 종료 위치에서 튀도록     

> 각각의 액션에 설정할 시 shareInterpolator 속성을 false로 설정     

### 애니메이션 호출 시점
- onWindowFocusChanged() : 사용자에게 화면이 표시되는 시점        
- AnimationListener 객체 : 애니메이션이 언제 시작/끝났는지에 대한 정보       
    
        public void onAnimationStart(Animation animation) : 시작되기 전에 호출
        public void onAnimationEnd(Animation animation) : 끝났을 때 호출
        public void onAnimationRepeat(Animation animation) : 반복될 때 호출

## 페이지 슬라이딩
버튼을 눌렀을 때 보이지 않던 뷰가 슬라이딩 방식으로 나타나는 기능       
데이터를 좀 더 효율적으로 보여주기 위한 UI 구성에 사용        

    SlidingPageAnimationListener animationListener = new SlidingPageAnimationListener();        //리스너 설정
        translateLeftAnim.setAnimationListener(animListener);
        translateRightAnim.setAnimationListener(animListener);
        
    public void onAnimationEnd(Animation animation){                            //끝났을 때 호출되는 메서드 안에 코드 넣기
            if(isPageOpen){ page.setVisibility(View.INVISIBLE);
            
## 앱 화면에 웹브라우저 
앱 안에 웹브라우저를 띄우면 더 자연스러운 화면을 보여줄 수 있다.       

[ 매니페스트에 등록해야 할 인터넷 권한 ]

    <user-permission android:name="android.permission.INTERNET" />
    
    <application
        android:usesCleartextTraffic="true"
    
[ MainActivity.java ]

    WebSettings webSettings = webView.getSettings();                        //웹뷰 설정 수정하기
    webSettings.setJavaScriptEnabled(true);                                 //자바 스크립트가 동작할 수 있는 환경

    button.setOnClickListener(new android.view.View.OnClickListener() {     //버튼 클릭 시 사이트 로딩
        @Override
        public void onClick(View v) {
            webView.loadUrl(editText.getText().toString());
        }
    });
    
> 대부분의 웹사이트가 자바 스크립트를 사용하므로 항상 setJavaScriptEnabled(true)로 설정하는 게 좋다.       
- loadUrl() : 웹페이지를 로딩하여 화면에 보여주기 위한 메서드    
- 웹뷰 객체의 goForward()나 goBack() 메서드를 이용하면 앞/뒤 페이지로 이동 가능     
- 화면에 추가된 WebView 객체에 웹페이지를 보여주기 위해서는 WebViewClient를 상속한 객체를 만들어 WebView에 설정해야 한다.     

### 웹뷰 응용
- 컨텐츠가 자주 변하는 경우 서버에서 컨텐츠 관리 가능     
- 앱 가이드나 메시지 중간에 색상이나 링크를 넣고 싶을 때 유용    
- 웹페이지와 앱 사이에 데이터 주고받기 가능       


## 시크바
프로그레스바를 확장하여 만든 상태 표시 위젯으로 사용자가 값을 조정할 수 있다.

    seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {      //시크바에 리스너 설정
        @Override
        public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
            setBrightness(i);
            textView.setText("변경된 값 : " + i);
        }
        
    private void setBrightness(int value){                                         //윈도우 매니저를 이용해 밝기 설정
        if(value < 10){
            value = 10;
        }else if (value > 100){
            value = 100;
        }
        WindowManager.LayoutParams params = getWindow().getAttributes();
        params.screenBrightness = (float)value/100;
        getWindow().setAttributes(params);
    }
    
## 키패드 제어
getSystemService() 메서드 참조 후 InputMethodManager 객체를 사용해 키패드 기능 제어        

[ 대표적인 inputType 속성 ]

    - number : 숫자       
    - numberSigned : 0보다 큰 숫자       
    - numberDecimal : 정수        
    - text : 텍스트        
    - textPassword : 패스워드로 표시   
    - textEmailAddress : 이메일로 표시    
    - phone : 전화번호로 표시      
    - time : 시간     
    - date : 날짜     

[ 키패드 감추기 ]

        if(getCurrentFocus()!=null){
            InputMethodManager inputMethodManager = (InputMethodManager)    //InputMethodManager 객체 참조
            getSystemService(INPUT_METHOD_SERVICE);
            
            inputMethodManager.hideSoftInputFromWindow(getCurrentFocus().getWindowToken(),0);   //키패드 감추기
        }
        
---
# 9.스레드와 핸들러 이해하기
- 스레드 : 동시 수행이 가능한 작업 단위    
- 데드락 : 동시에 두 곳 이상에서 요청이 생겼을 때 어떤 것을 먼저 처리할지 판단할 수 없어 발생하는 시스템 상의 문제        

## 핸들러
- 메인 스레드 : 안드로이드에서 UI를 처리할 때 사용되는 기본 스레드      
- 핸들러 : 새로 생성한 다른 스레드에서 메시지를 전달해 메인 스레드에서 처리할 수 있게 하는 객체        

**안드로이드의 스레드 = 표준 자바의 스레드**     
[ 스레드 사용하기 ]    
new 연산자로 객체 생성 -> start() 메서드 호출     

    button.setOnClickListener(new View.OnClickListener() {              //버튼을 누르면 스레드 객체 생성&시작
            @Override
            public void onClick(View v) {
                BackgroundThread thread = new BackgroundThread();
                thread.start();
            }
        });
        
핸들러 : 실행하려는 특정 기능이 있을 때 핸들러가 포함된 스레드에서 순차적으로 실행시킬 때 사용 !        
> 특정 메시지가 미래의 어떤 시점에 실행되도록 스케쥴링 가능      

    class BackgroundThread extends Thread{
        int value = 0;
        
        public void run(){
            for(int i=0; i<100; i++){
                try {
                    Thread.sleep(1000);
                } catch(Exception e){}

                value += 1;
                Log.d("Thread","value : " + value);
                
                Message message = handler.obtainMessage();      //메시지 객체 참조
                Bundle bundle = new Bundle();
                bundle.putInt("value",value);
                message.setData(bundle);
                
                handler.sendMessage(message);                   //메시지큐로 전송
            }
        }
    }
    
    class MainHandler extends Handler {
        public void handleMessage(Message msg){                 //핸들러 안에서 전달받은 메시지 객체 처리 
            super.handleMessage(msg);
            
            Bundle bundle = msg.getData();
            int value = bundle.getInt("value");
            textView.setText("value 값: " + value);
        }
    }
### Runnable 객체
Runnable 객체를 핸들러의 post() 메서드로 전달해주면 이 객체에 정의된 run() 메서드 안의 코드들 -> 메인 스레드에서 실행됨.     

    Handler handler = new Handler();        //API의 기본 핸들러 객체 생성
    
    handler.post(new Runnable(){            //핸들러의 post() 메서드 호출
        public void run(){
            textView.setText("value 값: " + value);
        }
    });
> 더 간결해서 이 방법을 더 많이 사용      

## 일정시간 후에 실행하기
응답이 늦어질 때 별도의 스레드를 만들어 처리하게 되는데 이때 Thread.sleep()을 사용해 잠시 대기상태로 있다 실행하는 방법도 있지만 핸들러로 지연 시간을 주면 메시지 큐를 통과하며 순차적으로 실행되기 때문에 UI 객체들에 영향을 주지 않는다.       

[ API에 있는 메서드 ]     
    
    public boolean sendMessageAtTime(Message msg, long uptimeMills)     //메시지 전송 시간 지정
    public boolean sendMessageDelayd(Message msg, long delayMillis)     //메시지 딜레이 시간 지정
    
## 스레드로 메시지 전송하기
스레드의 작업 결과물을 메시지로 만들어 전달하는 이유 = 별도의 스레드에서 메인 스레드가 관리하는 UI객체에 직접 접근 불가하기 때문      
**메인 스레드 -> 별도의 스레드로 전달은? 메시지 큐를 이용해 순서대로 접근하도록!**     
루퍼 : 메시지 큐에 들어오는 메시지를 지속적으로 보면서 하나씩 처리한다. 별도의 스레드 생성시 루퍼를 직접 생성해야 한다.      

    button.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v){
                String input = editText.getText().toString();       //텍스트 입력 받아서
                Message message = Message.obtain();                 //Message객체에 문자열 포함시켜
                message.obj = input;                                //obj 변수에 문자열 할당

            thread.processHandler.sendMessage(message);             //별도의 스레드로 전달
         }
     });
     thread = new ProcessThread();                                  //별도의 스레드 생성

    class ProcessHandler extends Handler{                   
        public void handleMessage(Message msg){
            final String output = msg.obj + " from thread.";        //새로운 문자열 생성
            handler.post(new Runnable(){
                public void run(){
                    textView.setText(output);                       //전달
                }
            });
        }
     }

## AsyncTask
AsyncTask 클래스를 상속하면 그 안에 스레드를 위한 코드와 UI 접근 코드를 한 번에 넣을 수 있다. -> 핸들러보다 간편!       

[ 주요 메서드 ]      
- doInBackground        
 : 새로 만든 스레드에서 백그라운드 작업 수행. execute() 메서드 호출할 때 사용된 파라미터를 배열로 전달받음.     
- onPreExecute      
 : 백그라운드 작업 수행 전 호출. 메인 스레드에서 실행되며 초기화 작업에 사용됨.     
- onProgressUpdate      
 : 백그라운드 작업의 진행 상태를 표시하기 위해 호출. 작업 수행 중간에 UI 객체에 접근하는 경우 사용. publishProgress() 메서드 호출해야 사용 가능       
- onPostExecute     
 : 백그라운드 작업 끝난 후 호출. 메인 스레드에서 실행되며 메모리 리소스를 해제하는 등의 작업에 사용됨. 결과는 Result 타입의 파라미터로 전달됨.      
       

    class BackgroundTask extends AsyncTask<Integer, Integer, Integer> {     //백그라운드 작업 수행 클래스
        protected void onPreExecute() {                                     
            value = 0;
            progressBar.setProgress(value);
        }

        protected Integer doInBackground(Integer ... values){               //테스크 객체 안에서 백그라운드 작업 수행하도록
            while (isCancelled() == false){
                value++;
                if(value >= 100){
                    break;
                } else {
                    publishProgress(value);                                 //UI에 진행상태 업데이트
                }
                try{
                    Thread.sleep(100);
                } catch(InterruptedException ex){}
            }
            return value;
        }
        protected void onProgressUpdate(Integer ... values){                //doInBackground() 메서드 호출시마다 자동 호출
            progressBar.setProgress(values[0].intValue());
        }
        protected void onPostExecute(Integer result){
            progressBar.setProgress(0);
        }
        protected void onCancelled() {
            progressBar.setProgress(0);
        }
    }

> < 자료형1, 자료형2, 자료형3 > = 재정의할 새로운 클래스의 메서드가 어떤 자료형의 파라미터를 가질 것인지 명시     
순서대로 doInBackground(), onProgressUpdate(), onPostExecute()의 파라미터 결정

    button2.setOnClickListener(new View.OnClickListener(){                 //버튼 누를 시 테스크 작업 취소
            public void onClick(View v){
                task.cancel(true);
            }
        });
