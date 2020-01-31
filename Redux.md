# Redux

#### 컴포넌트의 스토어 구독

G가 listener 함수를 스토어에 등록

스토어에서 state가 변경되면 listener 함수를 호출함

![컴포넌트의 스토어 구독](.\img\컴포넌트의 스토어 구독.png)



#### 스토어에 상태 변경하라고 알려주기

컴포넌트에서 특정 state의 값을 변경하려고 하는 경우

![컴포넌트의 스토어 구독](.\img\스토어에 상태 변경하라고 알려주기.png)

#### 

### 리덕스 3가지 규칙

#### 1. 하나의 애플리케이션에는 스토어는 하나.

#### 2. 상태는 읽기 전용 입니다.

```javascript
var person = {
  name: "jane",
  age: 16,
  family: [
    {
      name: "jack",
      age: 15
    },
    {
      name: "tom",
      age: 40
    },
  ]
};
// object-rest-spread를 사용해서 person을 수정하지 않고 newPerson을 만든다
var newPerson = {
  ...person,
  name: "mike"
};
```

불변성을 유지해야 하는 이유 : 

#### 3. 변화를 일으키는 함수, 리듀서는 순수한 함수여야 합니다.



#### Immutable 을 사용 할 때는 다음 규칙들을 기억하세요:

1. 객체는 Map
2. 배열은 List
3. 설정할땐 set
4. 읽을땐 get
5. 읽은다음에 설정 할 땐 update
6. 내부에 있는걸 ~ 할땐 뒤에 In 을 붙인다: setIn, getIn, updateIn
7. 일반 자바스크립트 객체로 변환 할 땐 toJS
8. List 엔 배열 내장함수와 비슷한 함수들이 있다 – push, slice, filter, sort, concat… 전부 불변함을 유지함
9. 특정 key 를 지울때 (혹은 List 에서 원소를 지울 때) delete 사용