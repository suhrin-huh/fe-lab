# 클로저(Closure)에 대해 설명해주세요.

**핵심키워드** : `클로저`,

`클로저(Closure)`란 함수가 선언될 당시의 외부 변수(스코프)를 기억하고, 그 함수가 그 스코프 밖에서 실행되더라도 그 환경을 그대로 기억해 해당 변수에 계속 접근할 수 있게 해주는 기능입니다.
렉시컬 환경을 캡처함으로써 함수가 선언 당시의 문맥을 보존할 수 있게 합니다. 이로 인해 상태 보존 및 캡슐화가 가능합니다.

## 클로저 함수가 만들어지는 조건

1. 중첩 함수가 있다.
2. 그 내부 함수가 외부 함수의 변수에 접근하고 있다.
3. 그 내부 함수가 외부 함수 밖에서 사용된다.

```js
function outer() {
  const string = "외부 변수";

  function inner() {
    // inner 함수는 outer의 렉시컬 환경을 캡처(closed over)한다.
    console.log(string);
  }

  return inner;
}

const closureFtn = outer(); // => outer() 함수는 inner 함수를 반환!
```

```js
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const countUp = counter(); // 익명 함수가 반환된다.
countUp(); // count = 1
countUp(); // count = 2 => 예상으로는 1이 되어야할 것 같지만, 그렇지 않다!
```

- 반복문과 클로저

  - `var`는 `for` 블록 안에 국한되지 않고, 그 바깥의 함수(또는 전역) 레벨에서 하나만 존재합니다. 따라서 setTimeout이 등록될 때마다 같은 i를 참조합니다.

  ```js
  // 내부상으로는 var i;
  for (var i = 0; i < 3; i++) {
    setTimeout(() => {
      console.log(i); // 3,3,3
    }, 100);
  }
  ```

  - `let`은 블록 스코프를 가지기 때문에, 각 반복마다 `i`가 새롭게 바인딩됩니다.

  ```js
  for (let i = 0; i < 3; i++) {
    setTimeout(() => {
      console.log(i); // 1,2,3
    }, 100);
  }
  ```
