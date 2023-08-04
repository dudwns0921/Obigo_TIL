# Obigo

## Sports App

### About Issue

인터넷 연결이 없을 때 무한로딩이 발생, 확인해보니 네트워크가 끊어졌다고 알려주는 팝업창을 띄우는 로직이 존재하지 않음

## How to solve

멘토님께서 showetworkErrorPopup이라는 함수를 추가해주셨다.

```tsx
  async function showNetworkErrorPopup () {
    console.log('showNetworkErrorPopup')
    return new Promise((resolve, reject) => {
      this.confirmPopup({
        title: i18n.t('popup.network-failed.title') as string,
        message: i18n.t('popup.network-failed.contents') as string,
        confirmText: i18n.t('popup.network-failed.button1') as string,
        notConfirmText: i18n.t('popup.network-failed.button2') as string
      }).then((isConfirmed) => {
        if (isConfirmed) {
          // retry
          resolve(true)
        } else {
          resolve(false)
        }
      })
    })
  }
```

먼저 전체 과정을 살펴보겠다.

1. confirmPopup에서 boolean 값이 반환된다.
2. 해당 값을 바탕으로 true일 경우 true를 반환, false일 경우 false를 반환한다.

그렇다면 confirmPopup은 어떤 함수일까?

```tsx
  function confirmPopup (req: {...}): Promise<boolean> {
    return this.popupStack.addConfirmPopup(req)
  }
```

```tsx
 public function addConfirmPopup (req: {...}): Promise<boolean> {
    return new Promise((resolve) => {
      const popupid = this.createPopupId()
      const popup: IConfirmPopupInfo = {
        popupid: popupid,
        title: req.title,
        category: 'confirm',
        message: req.message,
        desc: req.desc,
        icon: req.icon,
        buttons: [{ id: 'id-popup-confirm', name: req.confirmText }],
        callback: (btnId) => {
          // 버튼 클릭되면 목록에서 제거
          const isConfirmed = (btnId === 'id-popup-confirm')
          this.removePopup(popupid)
          resolve(isConfirmed)
        }
      }
      if (req.notConfirmText) {
        if (popup.buttons) {
          popup.buttons.unshift({ id: 'id-popup-not-confirm', name: req.notConfirmText })
        }
      }
      this.addPopup(popup)
    })
  }
```

confirmPopup은 addConfirmPopup 함수의 결과를 반환하는 함수이다. addCofirmPopup은 Promise 객체를 반환하기 때문에 resolve 메서드에서 어떤 값을 반환하는지만 확인해주면 된다. 팝업의 버튼이 클릭된 경우 해당 버튼이 confirm인지 아닌지를 판단한다. 어떤 경우에도 팝업은 stack에서 제거되지만 confirm인 경우에는 true를, 아닌 경우에는 false를 반환한다.

```js
catch (e) {
        console.warn('updateSchedule error:', e)
        await this.$app.showNetworkErrorPopup('Schedule Error').then(async (isRetry) => {
          if (isRetry) {
            await this.updateSchedule()
```

네트워크 에러가 발생할 경우 catch 구문으로 진입해 위 로직이 실행된다. 이번 경우에는 confirm 버튼을 retry로 대신했다. 네트워크 연결 재시도에 어울리는 텍스트로 바꾼 것뿐 기능적인 변화는 없다.  만약 retry 버튼을 누르면 updateSchedule()을 다시 실행시킨다. 이 함수는 vue 컴포넌트의 초기화 함수에서 실행되는 외부 api 함수들 중 가장 상위의 함수이다. 따라서 네트워크가 다시 연결되고 이 함수가 다시금 실행되면, 초기화가 정상적으로 진행된다.

## Wrap up

이번 기회를 통해 stack에 대해 좀 더 생각해볼 기회를 가졌다. popup 객체를 저장하는데 stack을 쓴 이유는, 가장 마지막에 뜬 popup부터 순차적으로 처리해야 하는 LIFO 구조를 가진 자료구조가 필요했기 때문이다. 