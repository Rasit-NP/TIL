# CSS position
### CSS Layout
- 각 요소의 위치와 크기를 조정하여 웹 페이지의 디자인을 결정하는 것
- 요소들을 상하좌우로 정렬하고, 간격을 맞추고, 페이지의 뼈대를 구성

### CSS Position
- 요소를 Normal Flow에서 제거하여 다른 위치로 배치하는 것
- 다른 요소 위에 올리기, 화면의 특정 위치에 고정시키기 등

## Position 유형
### static
- 요소를 Normal Flow에 따라 배치

### relative
- 요소를 Normal Flow에 따라 배치
- 자신의 원래 위치를 기준으로 이동
- top, right, bottom, left 속성으로 위치 조절

### absolute
- 요소를 Normal Flow에서 제거
- 가장 가까운 relative 부모 요소를 기준으로 이동
    - 만족하는 부모가 없다면 body 태그를 기준으로 이동
- 문서에서 요소가 차지하는 공간이 없어짐

### fixed
- 요소를 Normal Flow에서 제거
- 현재 화면영역을 기준으로 이동
- 스크롤해도 항상 위치가 유지됨

### sticky
- relative와 fixed의 특성을 결합한 속성
- 스크롤 위치가 임계점에 도달하기 전까지는 relative처럼 동작
- 스크롤 위치가 임계점에 도달하면 fixed처럼 화면에 고정
- 다음 sticky 요소가 나오면 이전 sticky 요소의 자리를 대체

## z-index
> 요소의 쌓임 순서를 정의하는 속성
```css
.index {
    z-index: 1;
}
```
- 정서 값을 사용해 Z축 순서를 지정
- 값이 클 수록 요소가 위에 쌓이게 됨
- static이 아닌 요소에만 적용됨
- 기본값은 auto로 부모 요소의 z-index값에 영향을 받음
- 같은 부모 내에서만 z-index 값을 비교하고, 값이 같으면 HTML 문서 순서대로 쌓임
- 부모의 z-index가 낮으면 자식의 z-index가 아무리 높아도 부모보다 위로 올라갈 수 없음