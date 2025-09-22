# 원시 타입과 참조 타입에 대해 설명해주세요.

**핵심키워드** : `원시 타입`, `참조 타입`, `깊은 복사`, `얕은 복사`

## 원시 타입(Primitive Types)

원시 타입은 값 자체를 변수에 저장하며, 값이 변하면 완전히 새로운 값이 됩니다. 원시 타입은 불변(immutable)한 값으로, 자바스크립트에는 총 7가지의 원시 타입이 존재합니다.

| 타입        | 설명                      | 예시                  |
| ----------- | ------------------------- | --------------------- |
| `Number`    | 숫자                      | 1, 3.14, -5           |
| `String`    | 문자열                    | "hello", 'abc'        |
| `Boolean`   | 참/거짓                   | true, false           |
| `Undefined` | 값이 할당되지 않음        | undefined             |
| `Null`      | 아무 값도 없음            | null                  |
| `Symbol`    | 고유하고 변경 불가능한 값 | Symbol('id')          |
| `BigInt`    | 아주 큰 정수              | 12345678901234567890n |

## 참조 타입(Reference Types)

참조 타입은 <u>값이 저장된 메모리 주소(참조)를</u> 변수에 저장합니다. 따라서 변수는 실제 값이 아닌 객체가 저장된 위치를 가리킵니다. 객체, 배열, 함수 등이 참조 타입입니다.

```js
let obj1 = { name: "지민" };
let obj2 = obj1; // obj2는 obj1이 가리키는 주소를 복사

obj2.name = "민수";

console.log(obj1.name); // "민수" (obj1도 변경됨)
```

위의 예시에서 `let obj2 = obj1`의 경우 `obj2`에는 `obj1`의 메모리 주소가 저장됩니다. 따라서 `obj1`의 값이 변경되면, `obj2`의 값도 함께 변경됩니다.

## 복사

코드를 작성하다보면, 변수의 값을 다른 변수에 복사해야하는 경우가 있습니다. 이 경우에 복사가 어느 정도로 이루어지는지를 명확하게 인지해야합니다.

복사는 `값 복사`와 `참조 복사`로 나눌 수 있습니다.

### `값 복사 (Value Copy)`

`원시 타입(Primitive value)`을 복사할 때 일어나는 복사 방식으로, <u>변수에 저장된 값 자체를 복사해서</u> 새로운 메모리 공간에 저장합니다. 따라서 복사한 변수와 원본 변수는 완전히 독립적인 변수입니다.

```js
let a = 10;
let b = a; // 값 복사
b = 20;
console.log(a); // 10 (a는 변하지 않음)
```

### `참조 복사 (Reference Copy)`

참조 복사는 `객체(참조 타입)`를 복사할 때 일어나는 복사 방식으로, 변수에는 객체가 아닌 <u>저장된 메모리 주소(참조)가</u> 저장됩니다. 따라서 두 변수는 같은 객체를 가리키게 되며, 한쪽을 수정하면 다른 쪽도 영향을 받습니다.

```js
let obj1 = { name: "지민" };
let obj2 = obj1; // 참조 복사

obj2.name = "민수";
console.log(obj1.name); // "민수" (obj1도 변경됨)
```

---

<br>

그럼, `참조 타입 객체`를 복사할 때에는 어떻게 해야할까요?
이는 `얕은 복사`와 `깊은 복사`로 나뉩니다.

### `얕은 복사 (Shallow Copy)`

얕은 복사는 최상위의 프로퍼티들만 복사합니다. 따라서 객체 내부에 또 다른 객체가 있으면, 그 내부 객체는 참조가 복사되어 원본과 공유됩니다.

```js
let obj1 = {
  name: "지민",
  address: { city: "서울" }, // 객체 내부에 또 다른 객체가 존재
};

let obj2 = { ...obj1 }; // 얕은 복사 => 최상위의 프로퍼티들만 복사된다.

obj2.name = "민수"; // 독립적으로 변경
obj2.address.city = "부산"; // 얕은 복사로 원본과 공유된다.

console.log(obj1.name); // "지민"
console.log(obj1.address.city); // "부산"
```

name은 복사되어 독립적이지만, address 객체는 참조 복사되어 둘 다 같은 주소를 참조합니다.

### `깊은 복사 (Deep Copy)`

객체 내부의 모든 중첩된 객체들까지 모두 복사해서 완전히 독립적인 새 객체를 생성합니다. 복사된 객체는 원본 객체와는 완전히 분리되기 때문에 서로 영향을 받지 않습니다.

```js
let obj1 = {
  name: "지민",
  address: { city: "서울" },
};

// Json으로 변환하여 깊은 복사
let obj2 = JSON.parse(JSON.stringify(obj1));

obj2.address.city = "부산";

console.log(obj1.address.city); // "서울"
```

JSON 방법은 간단하지만 특정 타입은 복사하지 못한다는 단점이 존재합니다. 따라서 복잡한 객체의 경우 `lodash`와 같은 라이브러리를 사용하기도 합니다.

## 불변성을 유지하는 방법

`불변성(immutability)`이란 한번 생성된 데이터가 변경되지 않는 성질을 의미합니다. 불변 데이터는 변경이 필요하면 기존 데이터를 복사하여 새롭게 만든 후 바꾸는 방식을 사용합니다.

데이터가 변하지 않으면 코드의 흐름을 예측하기가 쉬우며, 상태가 갑자기 바뀌는 버그를 줄임으로써 디버깅에 용이합니다.

JS에서는 기본적으로 객체와 배열이 가변(mutable)한 값으로, 값을 바로 변경할 수 있습니다. 따라서 불변성을 지키기 위해서는 직접 복사하여 새로운 객체 또는 배열을 만들어 사용해야 합니다.

이전에 설명되었던 깊은 복사와 얕은 복사의 개념이 연결되는데요.

객체 및 배열의 불변성을 유지하기 위해서는 값을 복사하거나, 불변성을 강제하는 방법이 있습니다. 값을 복사하는 경우에는 얕은 복사와 깊은 복사 중 적절한 방식을 선택할 수 있습니다.
불변성을 강제하는 경우, `Object.freeze()`를 사용하여 수정이 불가하도록 만들 수 있으나, freeze는 얕은 freeze이기 때문에 중첩 객체는 여전히 변경이 가능합니다.
따라서 깊은 freeze를 요구하는 경우 Immutable.js와 같은 라이브러리를 사용할 수 있습니다.

`+` `Object.freeze()`, `Immer`, `Immutable.js`

1.  `Object.freeze()`  
    자바스크립트 내장 함수로 객체를 얕게 얼려서(동결) 수정할 수 없게 만듭니다. 중첩 객체는 동결되지 않아 직접 재귀 처리해야합니다.

    ```js
    const user = {
      name: "지민",
      address: { city: "서울" },
    };

    Object.freeze(user);

    user.name = "민수"; // 무시됨
    console.log(user.name); // 지민

    user.address.city = "부산"; // 가능! (얕은 freeze라서 내부 객체는 변경 가능)
    console.log(user.address.city); // 부산
    ```

2.  `Immer`  
    가변 객체를 다루듯 작성되며, 내부적으로 불변성을 유지하는 새로운 객체를 생성합니다.
    Proxy로 변경을 감지하여 필요한 부분만 복사하며, 복잡한 깊은 복사 없이 편리하게 불변성을 유지할 수 있습니다.
    React에서 상태를 관리할 때에 자주 사용됩니다.

    ```js
    import produce from "immer";

    const baseState = {
      name: "지민",
      address: { city: "서울" },
    };

    const nextState = produce(baseState, (draft) => {
      draft.name = "민수";
      draft.address.city = "부산";
    });

    console.log(baseState.name); // "지민" (원본 유지)
    console.log(baseState.address.city); // "서울" (원본 유지)

    console.log(nextState.name); // "민수"
    console.log(nextState.address.city); // "부산"
    ```

3.  `Immutable.js`  
    불변 데이터 구조를 제공하는 라이브러리입니다. 데이터가 변경될 때, 내부적으로 새로운 객체를 반환 (persistent data structure)하며 깊은 불변성을 보장합니다.
    중첩 객체까지 복사해야하는 경우, 중첩된 곳마다 별도로 Immutable 자료 구조를 사용해야합니다.
    그럼에도 이 라이브러리를 사용하는 이유는 변경된 부분만 최소한으로 새로 만들고 나머지는 공유하기 때문에 큰 비용 없이 불변성을 유지할 수 있기 때문입니다.

    ```js
    // JS 내장 Map
    const map1 = new Map();
    map1.set("a", 1);
    const map2 = map1.set("a", 2);

    console.log(map1.get("a")); // 2 (원본 map1도 변경됨)
    console.log(map2.get("a")); // 2

    // Immutable.js Map
    import { Map } from "immutable";

    const mapA = Map({ a: 1 });
    const mapB = mapA.set("a", 2);

    console.log(mapA.get("a")); // 1 (원본 유지)
    console.log(mapB.get("a")); // 2 (새 객체)
    ```
