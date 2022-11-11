# Promise

`Promise` 객체는 어떤 로직이 실행된 이후에 다음 로직이 실행 되도록 할 때 사용된다.(내 생각)

다음 코드는 간단한 `Promise`객체의 사용예시이다.

```javascript
const greatFunction = () => {
  return new Promise((resolve, reject) => {
    if (greatLogic) resolve("성공하셨습니다!");
    reject("에러가 발생했습니다ㅠㅠ");
  });
};
```

위의 예시를 보면 `greatFunction`은 `Promise`객체를 반환한다. 반환되는 객체를 자세히 보면

```javascript
new Promise((resolve, reject) => {});
```

위와 같은 형식으로 쓰여져 있다. 이때 `Promise` 객체에 전달되는 함수를 executor라고 부른다.

<br>

`Promise`객체는 기본적으로 이 executor를 전달 받는다. 이때 executor의 두 인자 `resolve`와 `reject`는 자바스크립트에서 자체 제공하는 콜백이다. 두 콜백의 역할은 다음과 같다.

<br>

> ### `resolve(value)`: 성공적으로 일을 마쳤을 때 그 결과에 해당하는 `value`와 함께 호출
>
> ### `reject(error)`: 에러 발생시 에러 객체를 나타내는 `error`와 함께 호출

<br>

---

<br>

이제 이 `Promise` 객체를 사용하는 예시를 알아보자. 다음 예시는 `greatFunction`을 사용하는 함수이다.

```javascript
const fancyFunction = async () => {
  await greatFunction()
    .then((res) => {
      console.log(res);
    })
    .cath((error) => {
      console.log(error);
    });
};
```

`fancyFunction`을 실행시켰을 때의 결과는 두 가지로 나뉜다. 나뉘는 기준은 다음과 같다.

> ### 1. `Promise` 객체의 executor에서 `resolve`가 호출된다.
>
> ### 2. `Promise` 객체의 executor에서 `reject`가 호출된다.

만약 1번의 경우라면 콘솔에 `"성공하셨습니다!"`가 출력될 것이고, 2번의 경우라면 콘솔에 `"에러가 발생했습니다ㅠㅠ"`가 출력될 것이다.

<br>

간단히 생각해서 `then`함수의 콜백은 `resolve`가 호출됐을 때 실행되고, `catch`함수의 콜백은 `reject`가 호출됐을 때 실행된다. `finally`라는 함수도 있지만 얘는 그냥 `Promise`의 처리가 완료되면 실행되는 녀석이라고만 알아두면 된다.
