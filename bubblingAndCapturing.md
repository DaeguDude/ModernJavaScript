# 버블링과 캡처링

이벤트가 발생할 때 저희가 알아야 하는 개념에 버블링과 캡처링 이라는 것이 있습니다. 먼저 버블링부터 알아보죠.

## 버블링(bubbling)

버블링이란 한 요소에 이벤트가 발생하면, 그 요소에 해당된 핸들러가 동작되고, 이어서 그 부모 노드의 핸들러, 그리고 계속 이어지며 최상단의 조상 요소를 만날 때까지 동작하는 것입니다.

예시를 한 번 보면서 이해를 하면 좋습니다.

```html
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form onclick="alert('form')">FORM
  <div onclick="alert('div')">DIV
    <p onclick="alert('p')">P</p>
  </div>
</form>
```

만약에 가장 안쪽의 `<p>` 태그를 클릭하면 어떤 일이 벌어질까요? `<p>` 요소에 할당된 핸들러를 동작시키고, `<div>`의 핸들러를 동작 시키고, 그리고 `<form>`의 핸들러도 동작을 시킵니다. 그리고 최상단 노드인 document를 만날 때 까지, 각 요소에 할당된 `onclick` 핸들러가 있다면 모두 동작 시킬 것입니다.

이것을 이벤트 버블링이라고 부르는데, 이유는 제일 깊은 곳에서 올라가며 발생하는 모양이 물속 거품 같기 때문입니다.

이런식으로 만약에 동작을 한다면 어디에서 이벤트가 시작되었는지 알지 못하는 상황이 발생합니다. 그래서 `event.target` 이라는 녀석을 통해서, 이벤트가 어디서 발생이 되었는지 알아 볼 수 있습니다.

```html
<form>FORM
  <div>DIV
    <p>P</p>
  </div>
</form>
```

```javascript
const form = document.querySelector('form');
form.addEventListener('click', (e) => {
    alert(event.target.tagName);
})
```

위의 코드를 동작시키면 만약 `<p>`를 누르면 p, `<div>` 를 누르면 div, `<form>` 을 누르면 form을 출력시켜 줄 것입니다.

### 버블링 중단하기

**거의** 모든 이벤트가 버블링이 되는데 이 버블링을 멈추도록 할 수도 있습니다.

```html
<body onclick="alert('저는 body의 onclick 이벤트 핸들러입니다')">
    <button onclick="event.stopPropagation()">클릭</button>
</body>
```

### 캡처링(Capturing)

이벤트에는 캡처링(Capturing)이라는 흐름도 있습니다.

보통 이벤트가 발생하면 3 가지의 단계를 거칩니다.

1. 캡처링 단계 - 이벤트가 하위 요소로 전파됨
2. 타깃 단계 - 이벤트가 실제 타깃 요소에 전달됨
3. 버블링 단계 - 이벤트가 상위 요소로 전파됨

보통의 코드에서 캡처링을 쓰지는 않습니다. 하지만 그러한 것이 있다는 것을 알아두는 것은 의미가 있을 겁니다. 아래의 그림을 보며 캡쳐링을 이해해보죠.

<!-- 캡쳐링 그림 넣기 -->

만약에 `<td>`를 클릭하면 이벤트가 최상위 조상인 Window 부터 시작해서 `<td>` 까지 내려올 것입니다. 그 다음, 이벤트가 타겟에 전달이 된 후, 버블링 단계를 거쳐 다시 올라가겠죠.

여기서 중요한 점은 onevent 핸들러나 HTML 속성, addEventListener의 핸들러들은 캡처링에 대해서 알고 있지 않습니다. 이것들은 타깃, 버블링 단계에 속하기 때문이죠.

캡처링에서 이벤트를 잡아내려면 `addEventListener`의 `capture` 옵션을 `true`로 주면 됩니다. 기본값은 `false` 입니다.

```html
<style>
  body * {
    margin: 10px;
    border: 1px solid blue;
  }
</style>

<form>FORM
  <div>DIV
    <p>P</p>
  </div>
</form>

<script>
  for(let elem of document.querySelectorAll('*')) {
    elem.addEventListener("click", e => alert(`캡쳐링: ${elem.tagName}`), true);
    elem.addEventListener("click", e => alert(`버블링: ${elem.tagName}`));
  }
</script>
```

### 요약

캡처링은 보통 코드에서 잘 쓰이지 않는다. 버블링을 현실 세계에서 빗대어 비유를 하자면, 사건이 일어났을 때 먼저 지역 파출소(이벤트가 발생한 타겟요소)에서 먼저 연락을 받는다. 그리고 지역 파출소에서 일 처리를 한다(타겟요소의 핸들러). 그리고 상부에 보고를 한다(버블링). 

이 패턴은 나중에 이벤트 위임 패턴의 토대가 되는데 아주 유용하다.