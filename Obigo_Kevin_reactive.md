# Obigo

## Kevin

### reactive

- transform
  - translate3d를 사용하면 GPU를 사용하기 때문에 연산이 더 빠름

- p, span 태그 대체
  - div 태그 사용
  - p 태그 안에 div 태그 사용 시 오류 발생

- inline-block
  - 기본적으로 4px의 크기를 가짐
  - 다른 요소들과 여백 없이 정렬되기 원한다면 font-size 0 적용 필요
- float는 사용하지 말자
- 반응형
  - break 포인트 작업하려면 width만 작업해도 무방
  - box-sizing은 반응형 처리 시 항상 적용하기
  - col, row 컨테이너 구성 시 col, row 컨테이너는 직접적으로 디자인을 적용하지 말자
  - 그 하위에 새로운 div를 생성해 적용하기
  - row
    - gap 작업 시
    - 컨테이너 전체에 margin을 주기
    - 그리고 그 안의 요소에 padding 주기
  - 반응형이란 단순 scale을 줄이는 것이 아닌 화면에 따라 사용자가 잘 인식할 수 있게끔 하는 것

