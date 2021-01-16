# 이벤트 소개

이벤트(event)는 브라우저 안에서 어떠한 것이 일어났다는 신호입니다. DOM의 모든 노드들은 이러한 신호를 만들어냅니다. 

### 이벤트 종류

이벤트의 종류들에 대해서 한 번 살펴보겠습니다. 이벤트들의 모든 종류를 보시려면 MDN의 [이벤트 참조](https://developer.mozilla.org/ko/docs/Web/Events)를 참고하세요.

#### 마우스 이벤트:

- click – 요소 위에서 마우스 왼쪽 버튼을 눌렀을 때(터치스크린이 있는 장치에선 탭 했을 때) 발생합니다.
- contextmenu – 요소 위에서 마우스 오른쪽 버튼을 눌렀을 때 발생합니다.
- mouseover와 mouseout – 마우스 커서를 요소 위로 움직였을 때, 커서가 요소 밖으로 움직였을 때 발생합니다.

#### 폼 요소 이벤트:
- `submit`: 사용자가 `<form>`을 제출할 때 발생
- `focus`: 사용자가 `<input>`과 같은 요소에 포커스를 줄 때 발생

#### 키보드 이벤트:
- `keydown`: 사용자가 키보드 버튼을 누를 때 발생
- `keyup`: 사용자가 키보드 버튼을 뗄 때 발생

#### 문서 이벤트:
- `DOMContentLoaded`: HTML이 전부 로드 및 처리되어 DOM 생성이 완료되었을 때 발생

### 이벤트 핸들러

보통 어떠한 이벤트가 발생하였을 때 우리는 그것에 맞는 반응을 해주어야 하겠죠. 예를 들어, 쇼핑몰에서 이 상품을 장바구니에 추가 라는 버튼을 눌렀다면 장바구니에 추가가 되어야 할 것입니다. 이렇게 이벤트가 발생했을 때 특정한 반응을 하기 위해 실행시키는 함수를 이벤트 핸들러(Event Handler)라고 합니다.

이벤트 핸들러를 할당하는 방법은 크게 3가지로,

- HTML 속성
- `on<event>` 프로퍼티
- `addEventListener`

가 있습니다.

### HTML 속성

HTML 요소의 `on<event>` 속성에 핸들러를 할당하여 줄 수 있습니다.

```html
<input value="클릭해 주세요." onclick="alert('클릭!')" type="button">
```

버튼을 클릭하면 onclick안의 함수가 실행이 됩니다. 여기서 주의 할 점은, 속성값을 큰 따옴표로 둘러싸는 것입니다.

만약에, 핸들러의 코드가 길다면 함수를 자바스크립트에서 만들고 그 함수를 호출하는 방법이 있습니다.

```javascript
<script>
  function countDogs() {
    for(let i=1; i<=3; i++) {
      alert(`토끼 ${i}마리`);
    }
  }
</script>

<input type="button" onclick="countDogs()" value="강아지를 세봅시다!">
```

하지만 이렇게 HTML을 통해 이벤트에 반응을 하는 방식은 추천되지 않습니다. 컴퓨터 공학에서 사용되는 개념인 [관심사 분리(separation of concerns)](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)의 원칙에 어긋나기 때문입니다.

쉽게 얘기하자면, HTML, CSS, Javascript들이 각자 자기에 주어진 일만 하자는 것입니다.

### `on<Event>` 프로퍼티

`on<Event>` 핸들러는 특정한 DOM 요소의 프로퍼티로서 그 요소가 특정한 이벤트에 어떻게 반응할 것인지 정해줄 수 있습니다. 이것은 자바스크립트에서 다루어주기 때문에 조금 더 나은 방법입니다.

```javascript
<input id="elem" type="button" value="클릭해 주세요.">
<script>
  elem.onclick = function() {
    alert('안녕하세요');
  };
</script>
```

중요한 점은 `on<Event>` 핸들러는 복수의 이벤트 핸들러를 생성할 수 없습니다. 예를 들어서,

```javascript
<input id="elem" type="button" value="클릭해 주세요.">
<script>
  elem.onclick = function() {
    alert('안녕하세요');
  };

  elem.onclick = function() {
    alert('감사합니다');
  };
</script>
```

이렇게 두 번 할당을 하면, 뒤의 핸들러가 앞의 핸들러를 덮어 씌웁니다. 
만약에 핸들러를 제거하고 싶다면 `elem.onclick = null`을 사용해주시면 됩니다.

### 자주하는 실수

`onEvent` 핸들러를 사용할때 자주하는 실수들이 있습니다. 예시를 통해 한 번 알아볼게요.

```javascript
function sayHi() {
    alert("안녕하세요");
}

elem.onclick = sayHi();
```

위의 예시는 `sayHi` 함수를 작동시키지 않을 것입니다. 왜냐하면 onevent 핸들러는 함수의 결과값을 넘겨주는 게 아니라 함수 자체를 넘겨주어야 하기 때문입니다. 함수 자체를 넘겨주면 그 함수를 알아서 실행을 시켜줄 것입니다. 

위의 코드는 이런식으로 바뀌어야 하겠죠.

```javascript
function sayHi() {
    alert("안녕하세요");
}

elem.onclick = sayHi;
```

반대로 HTML 속성에서는, 함수를 호출시켜주어야 합니다.

```html
<input type="button" id="button" onclick="sayHi()">
```

왜냐하면 브라우저가 새로운 함수를 생성하여 onclick의 속성값을 새로운 함수의 본문으로 사용하기 때문이죠. 위의 예시는 이런식으로 바뀔 것입니다.

```javascript
button.onclick = function() {
    sayHi();
}
```

**`setAttribute`로 핸들러를 할당할 수 없습니다.**

```javascript
document.body.setAttribute('onclick', function() { alert("안녕하세요") });
```

아래의 코드는 작동하지 않습니다. 왜냐하면 속성은 문자열이기 때문에 위의 함수가 문자열이 되어버리기 때문입니다.

**onevent 핸들러는 대소문자를 구분합니다**

onevent 핸들러를 사용할 때 예를 들어, `elem.onclick`은 괜찮지만, `elem.ONCLICK`은 안됩니다.

### addEventListener

위에서 살펴본 HTML 속성과 onevent 핸들러는 약간의 문제가 있는데 그것이 무엇일까요? 
그것은 하나의 이벤트에 복수의 핸들러를 할당할 수가 없다는 것입니다.

예를 들어서, 쇼핑몰에서 장바구니 추가를 눌렀다고 생각해볼게요. 그러면

1. 장바구니에 물건을 추가
2. 고객에게 물건이 추가되었다고 알림

이렇게 2개의 반응을 해주고 싶다고 생각해보겠습니다. 기존의 방식이라면 아마 아래의 코드와 같은 방법을 사용할 것입니다.

```javascript
addToCartBtn.onclick = function() { ... } // 장바구니에 추가... 
addToCartBtn.onclick = function() { ... } // 유저에게 메세지 출력 
```

하지만 우리가 알다시피 두번째의 핸들러가 첫번째의 핸들러를 덮어 씌움으로서 장바구니에 물건이 추가되지 않고 물건이 추가되었다는 메세지만 출력될 것입니다. 그러면 아주 큰 문제가 되겠죠.
그래서 `addEventListener`와 `removeEventListener` 라는 새로운 대안이 개발되었습니다. `addEventListener`를 통해서 여러 개의 이벤트 핸들러를 할당하여 줄 수 있습니다.

아래와 같은 문법으로 사용하여 줄 수 있습니다.

```javascript
element.addEventListener(event, handler, [options]);
```

**event**
이벤트 이름(ex: 'click', 'keydown')

**handler**
핸들러 함수

**options**
여러 개의 프로퍼티를 갖는 객체

- `once`: `true`라면 이벤트가 한 번 실행되고 핸들러 함수가 삭제됨
- `capture`: [이벤트 캡처링](https://ko.javascript.info/bubbling-and-capturing#ref-1004)을 사용할 것인지 말 것인지
- `passive`: `true`면 `preventDefault()`를 호출하지 않음.

**핸들러 삭제하기**

`removeEventListener`를 이용하여 이미 할당된 핸들러를 삭제하여 줄 수 있습니다.

```javascript
element.removeEventListener(event, handler, [options])
```

여기서 중요한 점은 삭제는 똑같은 함수만 할 수 있습니다. 아래의 예시를 볼게요.

```javascript
elem.addEventListener('click', () => alert('안녕하세요'));

elem.removeEventListener('click', () => alert('안녕하세요'));
```

이렇게 하면 우리가 할당한 함수가 지워질까요? 정답은 아닙니다. 왜냐하면 둘은 똑같은 함수처럼 보이지만 그냥 같은 행동을 하여주는 다른 함수이기 때문입니다. 그래서 만약에 할당한 핸들러를 지우고 싶다면 이름이 있는 함수를 써주어야 합니다.

```javascript
function handler() {
    alert('안녕하세요');
}

elem.addEventListener('click', handler);
elem.removeEventListener('click', handler);
```

이렇게 작성을 해주면, `handler` 라는 함수는 똑같은 녀석이기 때문에 정확히 삭제가 될 것입니다.

보통 코드를 작성할 때는 onevent 핸들러나 `addEventListener` 둘 중 하나의 방법만을 사용해 코드를 작성하여 주는게 코드의 가독성이나 일관성에 더 좋습니다. 그리고 어떠한 이벤트들은 `addEventListener`를 써주어야만 동작을 합니다. 예시로,

```javascript
document.onDOMContentLoaded = function() {
    alert('DOM이 완성되었습니다.');
}
```

는 작동하지 않습니다.

```javascript
document.addEventListener('DOMContentLoaded', function() {
    alert('DOM이 완성되었습니다.');
}
```

이런 식으로 작성을 해주어야 합니다.

### 이벤트 객체

이벤트를 제대로 다루려면 어떤 일이 일어났는지 상세히 알아야 합니다. 예를 들어, 'click' 이벤트가 발생하였다면 마우스 포인터가 어디에 있는지, 'keydown' 이벤트가 발생하였다면 어떤 키가 눌러졌는지 등 정보가 필요합니다.

이벤트가 발생하면 브라우저는 [이벤트 객체](https://developer.mozilla.org/ko/docs/Web/API/Event)라는 것을 생성합니다. 그리고 이 객체를 핸들러 함수의 인수로 전달을 해줍니다.

```html
<input type="button" value="클릭해 주세요." id="elem">

<script>
  elem.onclick = function(event) {
    alert(event.type + " 이벤트가 " + event.currentTarget + "에서 발생했습니다.");
    alert("이벤트가 발생한 곳의 좌표는 " + event.clientX + ":" + event.clientY +"입니다.");
  };
</script>
```

위의 예시들에서 보이는 `event.type`, `event.currentTarget`, `event.clientX`, 그리고 `event.clientY` 말고도 다양한 프로퍼티들이 있습니다.
이벤트 객체에서 지원하는 프로퍼티는 MDN의 [이벤트 속성](https://developer.mozilla.org/ko/docs/Web/API/Event#%EC%86%8D%EC%84%B1)에서 확인할 수 있습니다.

### 객체 형태의 핸들러 - handleEvent

`addEventListener`를 사용해서 함수뿐만 아니라 객체형태의 이벤트 핸들러를 할당할 수도 있습니다. 객체를 넘겨주면, 객체안에 정의된 `handleEvent` 라는 메쏘드를 실행시킬 것입니다.

객체 리터럴을 사용한 예시를 한 번 보겠습니다.

```html
<button id="elem">클릭해 주세요.</button>

<script>
  let obj = {
    handleEvent(event) {
      alert(event.type + " 이벤트가 " + event.currentTarget + "에서 발생했습니다.");
    }
  };

  elem.addEventListener('click', obj);
</script>
```

위의 예시를 보시면 `obj` 라는 객체가 전달되는데, 이러한 객체가 전달되면 이벤트 발생시 `obj` 객체 안의 `handleEvent` 메쏘드를 찾아 자동으로 실행시킬 것입니다.

클래스도 객체이기 때문에 클래스를 사용하여서도 이벤트를 다루어 줄 수 있습니다.

```html
<button id="elem">클릭해 주세요.</button>

<script>
  class Menu {
    handleEvent(event) {
      // mousedown -> onMousedown
      let method = 'on' + event.type[0].toUpperCase() + event.type.slice(1);
      this[method](event);
    }

    onMousedown() {
      elem.innerHTML = "마우스 버튼을 눌렀습니다.";
    }

    onMouseup() {
      elem.innerHTML += " 그리고 버튼을 뗐습니다.";
    }
  }

  let menu = new Menu();
  elem.addEventListener('mousedown', menu);
  elem.addEventListener('mouseup', menu);
</script>
```

이런 식으로 하면, 각 이벤트 타입에 맞는 핸들러가 작동될 것이기 때문에 코드를 유지보수하기가 좀 더 쉬울 것입니다.

