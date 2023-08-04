# Obigo

## Y461

### About Issue

바이브에서 제공하는 미리 듣기 60초가 끝나면 다음 곡으로 넘어가는 것이 정상적인 흐름이다. 하지만 재생바를 조작해서 재생 시간이 60초 이상이 되면 현재 곡이 다시 재생되는 이슈이다.

## How to solve

먼저 일반적인 재생으로 60초를 넘기는 경우와 재생바를 통해 60초를 넘기는 경우에 어떤 차이가 있는 알아보겠다.

##### 일반적인 재생

- ProgressReportPositionPassed

- PlayStopped

- StreamRequested

- PlayStarted

##### 재생바 조작

- playstopped
- PlayCommandIssued

- StreamRequested

- PlayStarted

위 이벤트들은 앱에서 Clova, 실제로는 AF 쪽으로 전송하는 이벤트들이다. 여기서 미리듣기가 끝났음을 알려주는 이벤트는 ProgressReportPositionPassed이다. 오디오 객체에 dispatch되는 timeupdate 이벤트의 핸들러에서 미리듣기 시간이 끝나면 AF 쪽으로 ProgressReportPositionPassed 이벤트를 보내고 있다.

![image-20230414092555584](D:\업무문서\Obigo_TIL\md-images\image-20230414092555584.png)	

따라서 해당 이벤트가 발생하기 위해서는 timeupdate 이벤트가 무조건 dispatch 되어야 하는데, 현재 일반적인 재생이 아니면 timeupdate 이벤트가 발생하지 않는다. 

진행바를 옮기게 되면 아래와 같은 순서로 작업이 수행된다.

1. body 요소로 basic-player 이벤트가 dispatch, 이 때 이벤트 속성중 type은 SEEK
2. basic-player 이벤트 핸들러는 Observer 객체에서 등록
3. basic-player 이벤트 핸들러에서 type이 SEEK인 경우 Observer 객체 - media 필드 - seek 메서드를 수행
4. Observer 객체의 media 필드에는 AudioMedia 객체가 할당
5. AudioMedia 객체 - media 필드 - seek 메서드 수행

동일한 구조지만 AudioMedia 객체의 media 필드에는 AudioContentElement 객체가 할당

위와 같은 구조이기 때문에 앱에서 이를 해결하기 위해서는 AudioContentElement의 seek이라는 메서드에서 수정이 이루어지면 된다. 재생바가 옮겨졌을 때 재생시간을 확인하고 강제로 ProgressReportPositionPassed 이벤트를 보내도록 수정할 수 있겠지만 이는 고려해야 할 사항들이 너무 많아지기 때문에 기본적으로는 AF에서 어떤 방식으로든 재생 시간이 바뀐다면 timeupdate 이벤트를 dispatch 해주는 게 가장 좋긴 하다. 팀장님이 이는 AF팀에 요청한 후 해결이 어려운 경우 팀간 협의를 통해 해결해야 한다고 하셨다.

# :books:참고자료

Obigo 내부 Clova 관련 자료