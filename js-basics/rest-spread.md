# Rest 연산자와 Spread 연산자의 차이에 대해서 설명해주세요.

**핵심키워드** : `Rest 연산자`, `Spread 연산자`, `구조 분해 할당`

## Rest 연산자

나머지를 모아서 하나로 만드는 연산자로, 함수 매개변수에서 여러 개의 값을 하나로 묶을 때 사용됩니다.

```js
function printNumber(...numbers) {
  console.log(numbers);
}

printNumber(1, 2, 3, 4); // [1,2,3,4]
```

```js
function introduce(name, age, ...hobies) {
  console.log(name);
  console.log(age);
  console.log(hobies);
}

introduce("길동", 100, "운동", "영화"); // 길동 100 ["운동", "영화"]
```

```js
const arr = [1, 2, 3, 4];

const [first, ...rest] = arr;

console.log(`첫번째 숫자는 ${first}입니다.`); // 첫번째 숫자는 1입니다.
```

## Spread 연산자

묶인 걸 풀어서 펼쳐주는 연산자로, 배열이나 객체를 복사, 결합 및 개별 요소를 하나씩 전달할 때에 사용됩니다.

- 배열

  ```js
  const arr1 = [1, 2];
  const arr2 = [3, 4];

  // 두 배열에 있는 요소를 모두 가지고 있는 새로운 배열을 만들고 싶은 경우
  const arr3 = [...arr1, ...arr2]; // [ 1,2,3,4 ]
  ```

- 객체
  ```js
  const user = { name: "길동", age: 100 };
  const updatedUser = { ...user, name: "홍길동" }; // {{name: '홍길동', age: 100}}
  ```
  > 객체에서는 동일한 키가 중복되면, 마지막에 나오는 값이 덮어쓰기(override)됩니다.

<br>

> `+` `리터럴(literal)`이란 값 그 자체를 코드에 직접 쓰는 것을 의미합니다. 즉, 프로그래밍 언어에서 변수나 계산 없이, 있는 그대로 적는 값을 말합니다.

## 구조 분해 할당

구조 분해 할당이란 배열이나 객체 안의 값을 간단하게 꺼내서 변수로 만들 수 있는 문법입니다.

- 배열에서의 구조 분해 할당

  ```js
  // 전체 할당
  const fruits = ["사과", "바나나", "딸기"];

  const [a, b, c] = fruits;

  console.log(a); // "사과"
  console.log(b); // "바나나"
  console.log(c); // "딸기"

  // 일부만 할당
  const [first, , third] = ["A", "B", "C"];

  console.log(first); // "A"
  console.log(third); // "C"
  ```

- 객체에서의 구조 분해 할당

  - 객체의 경우 이름이 정확히 일치하는 key가 있어야 변수에 할당할 수 있습니다.

  ```js
  const user = { name: "지민", age: 25 };

  const { name, age } = user;

  console.log(name); // "지민"
  console.log(age); // 25

  // 변수명을 바꿀 수도 있습니다.
  // {원래 key : 새로 선언할 변수명}
  const user = { name: "지민", age: 25 };

  const { name: userName, age: userAge } = user;

  console.log(userName); // "지민"
  console.log(userAge); // 25
  ```

- 함수 매개변수에서의 구조 분해 할당

  ```js
  function greet({ name, age }) {
    // 매개변수로 객체가 전달되었을 경우
    console.log(`${name}은 ${age}살입니다.`);
  }

  greet({ name: "지민", age: 25 }); // 지민은 25살입니다.
  ```
