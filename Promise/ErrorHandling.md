# Error Handling

### 1️⃣ catch 사용

`Promise`에서 에러가 발생하게 되면 `catch`를 사용해 이를 처리한다.

```javascript
fetch("~~~")
  .then((response) => response.json())
  .then((json) => fetch(`~~~${json}`))
  .then((response) => response.json())
  .then(
    (json) =>
      new Promise((resolve, reject) => {
        if (json.name) {
          setTimeOut(() => {
            alert(json.name);
            resolve("success");
          }, 3000);
        } else reject("failed");
      })
  )
  .catch((error) => alert(error));
```

위 예시를 보면 여러개의 `then`을 거친 후 마지막에 `catch`가 있음을 볼 수 있다. 이때, 위쪽 여러 프로미스 중 하나라도 거부당하게 되면 `catch`가 트리거 되어 해당 오류를 처리하게 된다.

❗`then`이 몇개가 있든 오류가 발생하면 `catch`가 트리거 됨을 숙지하자!

<br>

---

<br>

### 2️⃣ 암시적 try catch

`Promise`는 암묵적인 try catch를 가지고 있다. 굳이 `reject`를 통해 거부하지 않더라도 `Promise`내에서 에러가 발생하면 이를 `reject`처럼 다룬다.

따라서 아래 두 경우 모두 같은 결과를 갖는다.

```javascript
new Promise((resolve, reject) => {
  throw new Error("에러 발생!");
}).catch(alert);
```

```javascript
new Promise((resolve, reject) => {
  reject(new Error("에러 발생!"));
}).catch(alert);
```

<br>

---

<br>

### 3️⃣ 다시 던지기 (catch에서 다른 catch로 던지기)

`catch`에서 에러가 정상적으로 처리되면 그 다음 `then`으로 제어 흐름이 넘어가게 된다.

```javascript
new Promise((resolve, reject) => {
  throw new Error("1번 에러");
})
    .catch((error) => {
        solveError1To3(error);
    })
    .then(()=>{
        에러 처리 후 정상적으로 실행된다.
    });
```

<br>

`Promise`에는 `catch`를 여러개 사용할 수도 있다. 이전 `catch`에서 처리되지 못한 에러는 그 이후 등장하는 `catch`에서 처리된다.

```javascript
new Promise((resolve, reject) => {
  throw new Error("5번 에러");
})
    .catch((error)=>{
        if(error가 1 ~ 3번 에러냐?) {
            solveError1To3(error)
        } else {
            throw error;
        };
    })
    .then(()=>{
        첫번째에 에러가 처리되면 실행되는 함수
    })
    .catch((error)=>{
        solveError4To6(error);
    });
```

<br>

---

<br>

### 4️⃣ 처리되지 못한 거부

만약 여러번의 `catch`에도 처리되지 못한 에러가 있을 수 있다. 이러한 에러를 브라우저 환경에서는 `unhandledrejection` 이벤트를 사용해 전역적으로 처리해줄 수 있다.

```javascript
window.addEventListener("unhandledrejection", function (event) {
  // unhandledrejection 이벤트엔 두 개의 특수 프로퍼티가 있습니다.
  alert(event.promise); // [object Promise] - 에러를 생성하는 프라미스
  alert(event.reason); // Error: 에러 발생! - 처리하지 못한 에러 객체
});

new Promise(function () {
  throw new Error("에러 발생!");
}); // 에러를 처리할 수 있는 .catch 핸들러가 없음
```

<br>

### 🏠[처음으로](https://github.com/kyw0716/modern-javascript-study)
