# Obigo

## Y461

### About Issue

AI Player 미리듣기 동작 중 재생이 멈춰버리는 이슈이다.

## How to solve

원인 분석 순서

- 재생이 멈췄기 때문에 Stop 디렉티브가 내려왔는지 확인

- Stop 디렉티브의 dialogRequestId를 확인

  dialogRequestId란 event 전송시 그에 대한 응답으로 전달되는 디렉티브와 연결시켜 확인하기 위한 id

  따라서 해당 dialogRequestId를 확인하면 어떤 Event에 대한 응답인지 확인 가능

- 전송된 event는 ProgressReportPositionPassedEvent

  원래 ProgressReportPositionPassedEvent가 전송되면 미리듣기가 종료되었다는 뜻으로 Play 디렉티브가 내려와 다음 곡이 재생되어야 함.

- 하지만 Stop 디렉티브가 내려오고 있는 상황

이 부분에 대해서는 AI/Mobile2팀에 추가 분석을 요청한 상황. 근본적인 원인이 해결된 상황은 아님.

# :books:참고자료

Obigo 내부 Clova 관련 자료