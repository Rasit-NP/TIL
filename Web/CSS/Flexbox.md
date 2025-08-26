# CSS Flexbox
> 요소를 행과 열 형태로 배치하는 1차원 레이아웃 방식
```css
.container{
    display: flex;
}
```

## Flexbox 구성 요소
- main axis
- cross axis
- flex container
- flex item

## Flexbox 속성
### Flexbox 속성 목록
- Flex Container 관련 속성
    - display
    - flex-direction
    - flex-wrap
    - justify-content
    - align-items
    - align-content
- Flex Item 관련 속성
    - align-self
    - flex-grow
    - flex-basis
    - order

### Flex Container 지정
- display 속성을 flex로 설정하면, flex Container로 지정됨
- flex item은 기본적으로 행으로 나열
- flex item은 주 축의 시작 선에서 시작
- flex item은 교차 축의 크기를 채우기 위해 늘어남
```css
.container {
    height: 500px;
    border: 1px solid black;
    display: flex;
}
```
## flex-wrap 응용
### 반응형 레이아웃 작성
- 다양한 디바이스 화면 크기에 자동으로 적응하여 콘텐츠를 최적으로 표시하는 웹 레이아웃 방식
- flex-wrap을 사용해 반응형 레이아웃 작성
1. .card 요소를 flex 컨테이너로 설정
2. 컨테이너의 공간이 부족할 경우, 여러 줄로 나뉘어 배치되도록 허용
3. 각 flex item의 기본 너비를 설정
4. 컨테이너에 여유 공간이 있을 때 공간을 차지하며 늘어날 수 있도록 함
    - 두 flex item 모두 값이 1이므로 절반씩 나눠가짐
```css
.card {
    width: 80%;
    border: 1px solid black;
    display: flex;
    flex-wrap: wrap;
}
img {
    width: 100%;
}
.thumbnail {
	flex-basis: 700px;
	flex-grow: 1;
}
.content {
	flex-basis: 350px;
	flex-grow: 1;
}
```
```html
<div class="card">
	<img class="thumbnail" src="#" alt="#">
	<div class="content">
		<h2>Heading</h2>
		<p>...</p>
	</div>
</div>
```