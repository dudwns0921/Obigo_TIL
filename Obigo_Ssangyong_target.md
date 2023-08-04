# Obigo

## Ssangyong 타겟 연결하기

기본적으로는 usb를 통해 pc와 연결하며, 현재 대부분 안드로이드 OS이기 때문에 adb를 통해 통신한다. 

## TroubleShooting

### device 연결 안될시

대부분 개발자 모드에서 usb 모드를 device로 변경하지 않아 발생하는 문제이다. 개발자 모드에 진입하기 위해서는 아래 순서를 따른다.

1. 전화 메뉴 진입
2. 번호창에다가 `*#20201030#*` 입력
3. USB 메뉴에서 host를 device로 변경 

### 인포콘 앱 미작동

바이너리 설치 이후에 인포콘 앱을 실행시켜도 아무런 동작이 없는 경우가 갑자기 발생했다. 이 경우 아래 순서에 따라 타겟을 초기화시키고 바이너리를 재설치한다.

1. 설정 > 일반 > 공장출고 상태 초기화 실행

2. uiccid.txt 생성 후 저장

   ```
   // uiccid.txt 내용
   UICCID:
   CTN:
   ```

   각 팀별 uiccid 정보는 아래 링크 확인
   https://confluence.obigo.com/pages/viewpage.action?pageId=90730124

   ```
   adb push uiccid.txt /mnt/sdcard
   ```

3. adb를 통해 AF, SA 재설치

# :books:참고자료





