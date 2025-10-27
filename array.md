# `배열(Array)`과 `유사 배열(Array-like Object)`의 차이에 대해 설명해주세요.

**핵심키워드** : `배열(Array)`, `유사 배열(Array-like Object)`

프로그래밍에서 `배열(Array)`은 가장 기본적이면서 자주 사용되는 자료구조 중 하나이다. 이와 함께 자주 언급되는 `유사 배열(Live Collection, Array-like Object)`은 겉보기에는 배열과 비슷해 보이지만, 명확한 차이를 가진다.

## 배열(Array)

JS에서 `배열(Array)`은 순서가 있는 값들의 집합으로, `push`, `pop`, `forEach` 등 다양한 메서드를 제공한다. Array 생성자 또는 리터럴(`[]`)을 통해 만들어지며, `length` 속성을 통해 요소의 개수를 추적하고, 인텍스를 기반으로 요소에 접근할 수 있다.

## 유사 배열(Array-like Object)
JS의 느슨한 타입 시스템과 유연한 객체 구조 덕분에, 배열처럼 인덱스로 값을 가지며 `length` 속성이 있는 또 다른 객체가 `유사 배열(Array-like Object)`이다. 브라우저 환경의 `NodeList`, `HTMLCollection` 또는 `arguments` 객체가 있다. 이들은 인덱스와 `length` 속성을 가지고 있지만, 실제 배열이 아니기 때문에 배열 메서드(`map`, `filter`) 등을 직접 사용할 수는 없다. 즉, 배열처럼 생겼으나 `Array.prototype`에 정의된 메서드를 상속받지는 않는다.

> `유사 배열(array-like)`은 컴퓨터 과학(CS)에서 공식적으로 정의된 자료구조가 아닌, JS라는 언어의 특성과 설계 철학에서 비롯된 실용적으로 생겨난 개념
> `Dynamic Array`는 CS에서 공식적으로 정의된 자료 구조로, Java의 ArrayList, JavaScript의 Array는 모두 내부적으로 동적 배열 구조를 가지고 있다.
> JS의 `유사 배열(Array-like Object)`은 자료 구조라기 보다는 '형태'나 '인터페이스' 수준의 분류이다. 아래의 세 조건을 만족하는 객체를 일종의 패턴으로 묶어서 '유사 배열'이라고 부르는 것이다.
>
> 1. `length` 속성이 있다.
> 2. 숫자 형태의 인덱스를 가진 속성들이 있다.
> 3. `Array`의 메서드를 상속받지 않는다.

`유사 배열(Array-like Object)`이라는 개념은 CS의 공식적인 자료구조로 분류되는 것은 아니며, 자바스크립트 언어와 브라우저 API 설계 관행에서 실용적으로 생겨난 개념이다.

### 브라우저의 DOM API 설계 방식
유사 배열이 등장한 주요 배경 중 하나는 브라우저의 DOM API 설계 방식에 있다. 예를 들어 `document.getElementsByTagName()` 이나 `getElementsByClassName()`이 반환하는 `HTMLCollection`은 DOM의 변경 사항을 실시간으로 반영해야하는 Live 구조로, 배열의 정적인 특성과는 다른 동작을 필요로 하너다. 이를 위해 배열이 아닌 별도의 객체로 정의되었고, 이런 객체들이 배열처럼 인덱스로 접근 가능하고, `length` 속성을 가지지만, 진짜 배열은 아닌 상태가 된 것이다. 배열이 아닌 이러한 객체를 배열처럼 다루기 위해서 `Array.prototype.slice.call(obj)` 또는 `Array.from(obj)`와 같은 패턴이 사용되기도 한다.

### JS의 함수 인자 처리 방식
또 하나의 중요한 배경은 JS의 함수 인자 처리 방식이다. JS는 함수의 매개변수 개수를 엄격하게 제한하지 않고, 함수 내부에서 `arguments`라는 내장 객체를 통해 인자를 받을 수 있도록 설계되어 있다. 이 객체 역시 배열처럼 인덱스를 통해 접근할 수 있고, `length`를 가지지만, 실제 배열은 아니기 때문에 배열 메서드를 사용할 수 없다.

```js
function sum () {
  console.log(arguments);
}

const arrowSum = (...args) => {
  console.log(args);
  console.log(arguments); // ReferenceError
}
```

## 유사 배열(Array-like Object)과 이터러블(iterable)
`유사 배열(Array-like Object)`은 이터러블(iterable)이라는 개념과 종종 혼동되지만, 이 둘은 정확히 같은 개념은 아니다. 이터러블은 `for...of` 문이나 전개 연산자(`...`) 등에서 순회 가능한 객체를 말하며, 내부에 `Symbol.iterator`를 구현하고 있어야 한다. 예를 들어 `NodeList`는 유사 배열이면서도 이터러블이기 때문에 `for...of`나 스프레드 문법으로 순회할 수 있지만, `arguments`는 유사 배열이지만 이터러블은 아니기 때문에 직접 순회할 수는 없다.

```js
function showArguments() {
  console.log(arguments); // 유사 배열
  console.log(arguments.map); // undefined (배열 메서드 아님!)

  const argsArray = Array.from(arguments); // 유사 배열을 배열로 변환
  console.log(argsArray.map(arg => arg * 2)); // 배열 메서드 사용 가능
}

showArguments(1, 2, 3);
'''

---

결국 유사 배열은 단순한 타입 구분의 문제가 아니라, 자바스크립트라는 언어와 브라우저 API 설계가 만나는 지점에서 생긴 실용적인 분류라고 할 수 있다. 그리고 이를 제대로 이해하는 것은 DOM 조작, 함수 인자 처리, 라이브러리 구현, 그리고 iterable 프로토콜과 같은 다양한 프론트엔드 개발 맥락에서 매우 중요한 기초가 된다.
