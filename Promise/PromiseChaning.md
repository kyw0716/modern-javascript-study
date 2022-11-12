# Promise Chaning

프로미스 객체는 이를 소비하는 함수 `then`과 `catch`, `finally`를 사용해 반환된 값을 활용한다.

```javascript
const getPromise = () => {
  return new Promoise((resolve, reject) => {
    setTimeout(() => {
      resolve("안녕하세용");
    }, 2000);
  });
};
```

위 코드는 2초 뒤 "안녕하세용"이라는 문구를 반환해주는 함수이다. 이때 `then`을 사용해 함수를 사용하면 다음과 같이 활용이 가능하다.

```javascript
const usePromise = () => {
  getPromise()
    .then((result) => {
      alert(result);
      return result * " 저는";
    })
    .then((result) => {
      alert(result);
      return result * " 김영우입니다.";
    })
    .then((result) => {
      alert(result);
    });
};

// 결과
// 안녕하세용
// 안녕하세용 저는
// 안녕하세용 저는 김영우입니다.
```

`Promise.then`을 사용하면 `Promise`가 반환되므로 위 예시처럼 `then`을 통한 체이닝이 가능해지게 된다. 하지만 만약 매 `alert`마다 2초의 지연 시간을 두고 싶다면 다음과 같이

```javascript
const usePromise = () => {
  getPromise()
    .then((result) => {
      alert(result);

      return new Promise((resolve, reject) => {
        setTimeout(() => {
          return resolve(result + " 저는");
        }, 2000);
      });
    })
    .then((result) => {
      alert(result);

      return new Promise((resolve, reject) => {
        setTimeout(() => {
          return resolve(result + " 김영우입니다.");
        }, 2000);
      });
    })
    .then((result) => {
      alert(result);
    });
};
```

매 `then`마다 `setTimeout`을 통해 2초의 지연시간을 만들어주면 된다.
