# 함수를 정의하는 방법에는 무엇이 있나요?

**핵심 키워드** : `함수 선언문`, `함수 표현식`, `화살표 함수`

## 함수를 정의하는 방법

자바스크립트에서 함수를 정의하는 방식이 3가지가 있습니다.

### `함수 선언문(Function Declaration)`

함수 선언문은 `function` 키워드로 이름이 있는 함수를 선언합니다.
코드 최상단으로 호이스팅되며, 스크립트가 실행되기 전에 메모리에 이미 등록되어 있기 때문에 함수를 정의하기 이전에도 호출이 가능합니다.

```js
hello();

function hello() {
  console.log("hello");
}
```

### `함수 표현식(Function Expression)`

함수 표현식은 변수에 익명 함수 또는 이름 있는 함수를 할당하는 것입니다. 변수만 호이스팅되고 함수 자체는 호이스팅되지 않아 `undefined`로 초기화됩니다. 따라서 정의하기 이전에 호출할 경우 오류가 발생합니다.

```js
hello(); // undefined 상태에서 호출 => 오류 발생

const hello = function () {
  console.log("hello");
};
```

### `화살표 함수(Arrow Function)`

ES6부터 도입된 함수 표현식의 축약형으로, 익명 함수로만 사용 가능합니다. 위에서 언급된 함수 선언문, 함수 표현식과는 `this` 바인딩 방식에 차이가 있습니다. `화살표 함수`는 자신만의 `this`, `arguments`, `super`, `new.target`을 가지지 않으며 항상 상위 스코프의 `this`를 캡처합니다. 또한 생성자 함수로 사용이 불가합니다.

```js
const Person = () => {};

const p = new Person(); // TypeError: Person is not a constructor
```

## 방식별 차이점

### 호이스팅

```js
sayhello(); // 정상 동작
sayBye(); // TypeError : undefined
sayGood(); // TypeError : undefined

// 함수 선언식
function sayhello() {
  console.log("hello");
}

// 함수 표현식
const sayBye = function () {
  console.log("Bye");
};

// 화살표 함수
const sayGood = () => {
  console.log("good");
};
```

### `this` 바인딩

함수 선언문과 함수 표현식은 `this` 바인딩이 동적으로 이루어집니다. 그러나 화살표 함수에서는 `this`를 생성하지 않고, <u>상위 스코프에서 캡처</u>하여 그대로 사용합니다.

<p style="fontSize:10px;">보다 더 자세한 설명은 <strong>this</strong>에서 확인 가능합니다.</p>

```js
const obj = {
  value: 100,
  expression: function () {
    // 함수 표현식
    console.log(this.value);
  },
  arrow: () => {
    // 화살표 함수
    console.log(this.value);
  },
};

obj.expression(); // 함수 표현식은 호출한 객체로 this 바인딩
obj.arrow(); // 상위 스코프에서 캡처 => 객체 리터럴 자체는 스코프를 만들지 않으므로 외부 스코프인 전역 객체를 캡처한다.
```

### `arguments` 사용

`arguments`는 함수 내부에서 자동으로 생성되는 유사 배열 객체로, 함수에 전달된 모든 인자를 담고 있습니다.
화살표 함수에서는 ` arguments` 대신 `...args`를 사용해야합니다.

```js
function sum() {
  console.log(arguments);
}

const arrowSum = (...args) => {
  console.log(args);
  console.log(arguments); // ReferenceError
};
```
