# Obigo

## U100

### About Issue

AI Player와 달리 팟빵과 지니뮤직에서 재생바 조작시 딜레이가 발생하는 이슈

### How to solve

원인 분석

> Unable to preventDefault inside passive event listener due to target being treated as passive

위 경고창이 콘솔창에서 확인되었고, fastClick 이라는 라이브러리에서 발생하고 있었음. fastClick 라이브러리를 제거하니 딜레이가 사라짐.

하지만 iscroll을 사용하는 컴포넌트에서 click 이벤트가 일어나지 않았고, 이 부분은 iscroll 생성자에 옵션 중 click 옵션을 true로 해서 해결. 

이 부분은 리눅스 AF, 안드로이드 AF에 따라 다르게 설정 필요

- 리눅스 AF
  - click 옵션을 false로 설정, true로 설정할 경우 click 이벤트 두 번 발생
- 안드로이드 AF
  -  click 옵션을 true로 설정

# :books:참고자료
