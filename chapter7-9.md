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
    
    
    
