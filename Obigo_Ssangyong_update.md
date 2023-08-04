# Obigo

## Ssangyong 펌웨어 업데이트

먼저 업데이트에 필요한 파일을 ftp를 사용해 다운로드한다. 현재 ftp 클라이언트인 Filezilla를 사용해서 파일들을 다운로드하고 있다. 서버 정보는 아래 링크를 확인해보자.

https://confluence.obigo.com/pages/viewpage.action?pageId=102710691

## U100

### 내비게이션 & MCU 업데이트

*EMC-U100_HQX-V000000543 버전부터 가능*

- MCU
  - /ftp_digen12/U100/SW Release/MCU
  - *.bin 파일만 필요
- Map Data
  - /ftp_digen12/U100/Thinkware_MAPDATA
  - SY_Data 폴더 전체 필요

- USB root 폴더에 업데이트 파일 넣기
  - SY_Data
  - *.bin
- USB와 타겟 연결
- 엔지니어링 모드에서 USB host 로 설정
- 설정 > 시스템 설정 > 업데이트 실행

### SOC 업데이트

*EMC-U100_HQX-V000000548 버전부터 가능*

- USB /UpdatePackage/HUROM 경로에 업데이트 파일 넣기

  - update.md5

  - update.zip

    가이드에서는 파일명을 변경하라고 되어있지만, 실제 ftp를 통해 받으면 파일명이 위와 같음

- USB와 타겟 연결

- 엔지니어링 모드에서 USB host 로 설정

- 설정 > 시스템 설정

- 시스템 설정 문구 5회 터치

  ![image-20230613140016632](D:\업무문서\Obigo_TIL\md-images\image-20230613140016632.png)	

- 오른쪽에 뜨는 탭들중 FOTA 터치

- 오른쪽 화면의 제목 부분 8번 터치

  ![image-20230613140044904](D:\업무문서\Obigo_TIL\md-images\image-20230613140044904.png)	

  이 때 8번 터치해도 아무런 반응이 없다면 상단 바를 내렸다가 올린 후에 다시 시도

  그리고 8번 터치 후 click 8 이라는 창이 뜨는데, 이 창이 뜬 이후에 다시 상단 바를 내렸다가 올린 후 제목 터치

- 3개 체크 박스 체크 후 아래 버튼 터치

  ![image-20230613140101868](D:\업무문서\Obigo_TIL\md-images\image-20230613140101868.png)	

- 설치 팝업 뜨면 Install 버튼 누르기

  ![image-20230613140208433](D:\업무문서\Obigo_TIL\md-images\image-20230613140208433.png)	

- 이후 체크박스 설정하는 화면에서 진행률 확인 가능
