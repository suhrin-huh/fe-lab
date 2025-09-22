# 호이스팅(Hoisting)에 대해 설명해주세요.

**핵심키워드** : `호이스팅`, `변수 선언 키워드`, `함수 선언문`, `함수 표현식`

`호이스팅(Hoisting)`이란 코드 실행 전에 변수 및 함수의 선언이 메모리에 등록되는 것을 의미합니다. <u>선언이 코드의 맨 위로 끌어올려지는 것처럼 동작</u>합니다.

`변수 선언 키워드`나 `함수 선언 방식`에 따라 선언과 초기화 시점에 차이가 있습니다.

- `변수 선언 키워드`에 따른 선언과 초기화 차이

  - `var`는 호이스팅이 발생하며, 선언과 초기화가 동시에 발생합니다.
  - `const`, `let`도 호이스팅이 발생하지만, 선언만 호이스팅이 되고 초기화는 나중에 이루어집니다.

- `함수 선언 방식`에 따른 선언과 초기화 차이

  - `함수 선언문`은 전체 함수가 호이스팅되기 때문에 어디서든 호출이 가능합니다.

  ```js
  sayHi(); // 함수 실행된다.

  function sayHi() {
    console.log("Hello");
  }
  ```

  - `함수 표현식`은 변수만 호이스팅되므로 초기값은 변수 선언 키워드에 맞게 설정됩니다. 따라서 선언 전에 호출할 경우 에러가 발생합니다.

  ```js
  sayHi(); // TypeError: sayHi is not a function

  // var 사용
  var sayHi = function () {
    ///...
  };

  sayBye(); // ReferenceError: Cannot access 'sayBye' before initialization

  // const 사용
  const sayBye = function () {
    // ...
  };
  ```
