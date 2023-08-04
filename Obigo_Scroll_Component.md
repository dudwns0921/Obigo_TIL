# Obigo

## 스크롤 컴포넌트 파헤치기

회사 애플리케이션에서 Iscroll을 활용한 공통 스크롤 컴포넌트를 재사용하고 있다. 

```vue
<template>
  <div class="scroll-view">
    <div
      class="scroll-container"
      :style="getSize()"
    >
      <slot />
    </div>
  </div>
</template>
```

스크롤 컴포넌트는 다음과 같은 구조로 이루어져 있다. slot에 들어오는 컴포넌트의 크기에 따라 scroll-container의 크기가 정해지고, 부모 요소인 scroll-view div의 크기를 조절해 overflow된 요소들을 얼마나 보이게 할 것인지 정할 수 있다.

```scss
.scroll-view, .scroll-container {
  overflow: hidden;
  width: 100%;
  height: 100%;
  position: relative;
  box-sizing: border-box;
}
```

둘의 css는 위와 같다. 둘 다 부모 요소의 크기를 상속하기 때문에 scroll view의 크기를 정하기 위해서는 scroll view를 감싸는 또 다른 container를 추가해주어야 한다. 

```js
getSize () {
  let size = ''
  if (this.scrollX) size += 'width: auto;'
  if (this.scrollY) size += 'height: auto;'
  return size
}
```

그리고 scroll-container 요소의 style 속성에 getStyle() 메서드가 바인딩된 것을 확인할 수 있다. getSize 메서드는 스크롤의 방향에 따라 width 혹은 height의 값을 100%에서 auto로 바꿔준다. 그 이유는 scroll-container는 스크롤을 적용시키려는 모든 요소들을 포함할 수 있는 크기를 가져야 하기 때문이다. 

### 커스터마이징

scroll view의 크기나 스크롤바의 위치는 스크롤을 적용시킨 컴포넌트에 따라 조금씩 달라질 수밖에 없다. 그래서 커스터마이징은 필수이다.

```vue
<template>
  <div class="card-list">
    <scroll-view
      ref="scrollView"
      class="scroll-view--override"
    >
      <slot />
    </scroll-view>
  </div>
</template>
```

위 컴포넌트는 카드 형식으로 만들어진 야구 팀 컴포넌트들을 담는 리스트 컴포넌트이다. 

```scss
.card-list {
  height: 478px;
}
```

맨 처음에도 언급했지만, scroll view의 크기는 scroll view의 부모 요소의 크기로 조절할 수 있다. 이렇게 하면 야구 팀을 담은 리스트에서 478px 만큼만 보여지게 된다.

그 다음에는 스크롤바의 위치를 변경해주어야 하는데, 이 때 클래스 오버라이딩 방법을 사용할 수 있다. 실제로 이런 방법이 있는 것은 아니지만 비슷한 개념을 갖고 있기에 이렇게 명명했다.

```scss
.iScrollVerticalScrollbar {
	width: $scrollbar-thickness;
	bottom: $scroll-margin;
	top: $scroll-margin;
	right: $scroll-edge-space;
...
```

위는 스크롤 컴포넌트에 기본적으로 설정된 css이다. 해당 클래스명은 iscroll 라이브러리에서 설정한 것으로 자세한 정보는 공식 문서에서 찾아보도록 하자. 

```scss
.scroll-view--override {
  .iScrollVerticalScrollbar {
    right: 0px;
  }
}
```

중요한 점은 해당 css는 scoped가 아닌 전역으로 설정해야 한다는 점이다. 그래야만 클래스가 오버라이드되어 css 속성 값을 변경할 수 있게 된다. 이 방법은 사실 css가 전역적으로 적용되어 문제가 발생할 수도 있으므로 주의해서 사용하자.