# HTML
- HyperText Markup Language

## Structure of HTML
- `<!DOCTYPE html>`
    - 해당 문서가 html 문서라는 것을 나타냄
- `<html></html>`
    - 전체 페이지의 콘텐츠를 포함
- `<title></title>`
    - 브라우저 탭 및 즐겨찾기 시 표시되는 제목으로 사용
- `<head></head>`
    - HTML 문서에 관련된 설명, 설정 등 컴퓨터가 식별하는 메타 데이터를 작성
- `<body></body>`
    - HTML 문서의 내용을 나타냄
    - 페이지에 표시되는 모든 콘텐츠를 작성
    - 한 문서에 하나의 body 요소만 존재

## HTML element
- 하나의 요소는 여는 태그와 닫는 태그 그리고 그 안의 내용으로 구성됨
- 닫는 태그는 이름 앞에 슬래시가 포함됨
    - 닫는 태그가 없는 태그도 존재

## HTML Attributes
- 사용자가 원하는 기준에 맞도록 요소를 설정하거나 다양한 방식으로 요소의 동작을 조절하기 위한 값
- 목적
    - 나타내고 싶지 않지만 추가적인 기능, 내용을 담고 싶을 때 사용
    - CSS에서 스타일 적용을 위해 해당 요소를 선택하기 위한 값으로 활용됨
- 작성 규칙
    1. 속성은 요소 이름과 속성 사이에 공백이 있어야 함
    2. 하나 이상의 속성들이 있는 경우엔 속성 사이에 공백으로 구분함
    3. 속성 값은 열고 닫는 따옴표로 감싸야 함
```HTML
<p class="editor-note">Hello</p>
```

## HTML Text Structure
- HTML의 주요 목적 중 하나는 텍스트 구조와 의미를 제공하는 것
- h1 요소는 단순히 텍스트를 크게 만드는 것이 아닌 현재 문서의 최상위 제목이라는 의미를 부여하는 것

### Ex
- Heading & Paragraphs
    - h1~6, p
- Lists
    - ol, ul, li
- Emphasis & Importance
    - em, strong
```HTML
<html>
<body>
    <h1>대제목</h1>
    <p><strong>My</strong> <em>page</em> 문단입니다.</p>
    <a href="https://www.google.co.kr">Google로 이동!</a>
    <img src="images/sample.png" alt="샘플 이미지">
    <img src="https://picsum.photos/200/300" alt="랜덤 이미지">
</body>
</html>
```
