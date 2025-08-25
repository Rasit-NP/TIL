# CSS
## CSS 적용 방법
1. Inline
2. Internal
3. External

### Inline 스타일
- HTML 요소 안에 style 속성 값으로 작성
```HTML
<h1 style="color: blue; background-color: yellow;">Hello World!</h1>
```

### Internal 스타일 시트
- head 태그 안의 style 태그에 작성
```HTML
<head>
    ...
    <style>
        h1 {
            color: blue;
            background-color: yellow;
        }
    </style>
</head>
```

### External 스타일 시트
- 별도 CSS 파일 생성 후 HTML link 태그를 사용해 불러오기

## CSS 기본 구조와 문법
- Selector(선택자)
    - 누구를 꾸밀지 지정
- Declaration(선언)
    - 어떻게 꾸밀지에 대한 구체적인 한 줄의 명령
    - 속성과 값이 한 쌍으로 이루어지며, 세미콜론으로 끝남
- Property(속성)
    - 바꾸고 싶은 스타일의 종류를 나타냄
- Value(값)
    - 속성에 적용할 구체적인 설정을 나타냄

## CSS Selector
### CSS Selectors 종류
- 기본 선택자
    - 전체 선택자
    - 요소 선택자
    - 클래스 선택자
    - 아이디 선택자
    - 속성 선택자
- 결합자(Combinators)
    - 자손 결합자
    - 자식 결합자

### CSS Selectors: 기본 선택자
- 전체 선택자
    - HTML 모든 요소를 선택
- 요소 선택자
    - 지정한 모든 태그를 선택
- 클래스 선택자
    - 주어진 클래스 속성을 가진 모든 요소를 선택
- 아이디 선택자
    - 주어진 아이디 속성을 가진 요소 선택
    - 문서에는 주어진 아이디를 가진 요소가 하나만 있어야 함
- 속성 선택자
    - 주어진 속성이나 속성값을 가진 모든 요소 선택
    - 속성의 존재 여부, 값을 일치/포함 등 다양한 조건으로 요소를 정교하게 선택 가능
```CSS
* {
    color: red;
}

h2 {
    color: orange;
}

h3,
h4 {
    color: blue;
}

.green {
    color: green;
}

#purple {
    color: purple;
}

[class^="y"] {
    color: yellow;
}
```
```HTML
<body>
    <h1 class="green">Heading</h1>
    <h2>Practice Selector</h2>
    <h3>Hello</h3>
    <h4>Nice to meet you</h4>
    <p id="purple">목록</p>
    <ul class="green">
        <li>list1</li>
        <li>list2</li>
        <li>list3
            <ol>
                <li>list31</li>
                <li>list32</li>
            </ol>
        </li>
    </ul>
    <p class="green">
        Lorem, <span>ipsum</span> dolor.
    </p>
    <p class="yellow">Test</p>
</body>
```

### CSS Combinators
- 자손 결합자(" ")
    - 첫 번째 요소의 자손 요소들 선택
    - 예) p span은 \<p> 안에 있는 모든 \<span>를 선택(하위 레벨 상관 X)
- 자식 결합자(">")
    - 첫 번째 요소의 직계 자식만 선택
    - 예) ul>li은 \<ul> 안에 있는 모든 \<li>를 선택(한 단계 아래 자식들만)

```CSS
<style>
    ...,
    .green > span{
        font-size: 50px;
    }

    .green li {
        color: brown;
    }
</style>
```

## CSS Declaration
### Property
- 스타일링하고 싶은 기능이나 특성을 의미
- CSS가 미리 정의해 둔 키워드를 사용
- font-size, background-color, width, margin, padding 등

### Value
- 속성에 적용될 구체적인 설정
- 속성이 받을 수 있는 값의 종류는 정해져 있음
- 16px, lightgray, 100%, 10rem 등

### Units
- color: red; 처럼 키워드로 끝나는 값도 있지만, 크기나 간격을 나타낼 때는 단위가 필수적
- 단위는 크게 절대 단위와 상대 단위로 나뉨

### px
- 화면을 구성하는 단위인 pixel을 기준으로 하는 절대 단위
- 직관적이고 예측이 쉬움

### em
- 부모 요소의 font-size를 기준으로 크기가 결정되는 상대 단위
- 만약 부모 요소에 font-size가 없다며니, 그 상위 부모의 font-size를 상속 받음

### rem
- em의 단점을 극복하기 위해 등장
- 최상위 요소인 \<html>의 font-size를 기준으로 크기가 결정
- html의 기본 font-size는 대부분의 브라우저에서 16px
- 요소가 아무리 깊게 중첩되어도 기준은 항상 html이므로, em처럼 계산이 복잡해지지 않음
- \<html>의 font-size만 변경하면 사이트 전체의 레이아웃과 폰트 크기를 일관되게 조절할 수 있음
- 사용자가 브라우저에서 설정한 기본 폰트 크기를 html이 상속받으므로, 사용자의 설정에 맞춰 사이트 전체가 유연하게 확대/축소 됨

## Specificity
> 결과적으로 요소에 적용할 CSS 선언을 결정하기 위한 알고리즘

### 명시도가 높은 순
1. Importance
    - !important
2. Inline 스타일
3. 선택자
    - id 선택자 > class 선택자 > 요소 선택자
4. 소스 코드 선언 순서

## CSS 상속
- 기본적으로 CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속해 재사용성을 높임
- 상속되는 속성
    - Text 관련 요소(font, color, text-align), opacity, visibility 등
- 상속되지 않는 속성
    - Box model 관련 요소(width, height, border, box-sizing ...)
    - position 관련 요소(position, top/right/bottom/left, z-index) 등

## CSS Box Model
> 웹 페이지의 모든 HTML 요소를 감싸는 사각형 상자 모델
>>content, padding, border, margin으로 구성되어 요소의 크기와 배치를 결정함

### Box 구성 요소 예시
```HTML
<body>
    <div class="box1">box1</div>
    <div class="box2">box2</div>
</body>
```
```CSS
.box1 {
    width: 200px;
    padding-left: 25px;
    padding-bottom: 25px;
    margin-left: 25px;
    margin-top: 50px;
    border-width: 3px;
    border-style: solid;
    border-color: black;
}

.box2 {
    width: 200px;
    padding: 25px 50px;
    margin: 25px auto;
    border: 1px dashed black;
}
```

## Shorthand
### border
- border-width, border-style, border-color를 한 번에 설정하기 위한 속성
```CSS
border: 2px solid black;
```

### margin & padding
- 4방향의 속성을 각각 지정하지 않고 한 번에 지정할 수 있는 속성
```CSS
/* 4개 - 상우하좌 */
margin: 10px 20px 30px 40px;
padding: 10px 20px 30px 40px;

/* 3개 - 상/좌우/하 */
margin: 10px 20px 30px;
padding: 10px 20px 30px;

/* 2개 - 상하/좌우 */
margin: 10px 20px;
padding: 10px 20px;

/* 1개 - 공통 */
margin: 10px;
padding: 10px;
```

## Box-sizing
### The standard CSS box model
1. 표준 상자 모델에서 width와 height 속성 값을 설정하면, 이 값은 content box의 크기를 조정하게 됨
2. CSS는 border box가 아닌 content box의 크기를 width 값으로 지정

### The alternative CSS box model
- 대체 상자 모델에서 모든 width와 height는 실제 상자의 너비
- 실제 박스 크기를 정하기 위해 테두리와 패딩을 조정할 필요가 있음