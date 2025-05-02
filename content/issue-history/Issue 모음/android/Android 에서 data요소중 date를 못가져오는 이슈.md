## 이슈 사항

1. firebase로 얻어온 비철의 데이터중 date를 가져오는것이 있는데 나머지 요소를 모두 가져올 수 있지만 오직 date만 가져오지 못한다..!!!
   1. 작성한 코드
      ![[issue_date_error_write_code.png]]
   2. 결과값
      ![[issue_date_error_result.png]]

## 문제점

- **ios에서는 아무 문제없이 date 값을 가져올 수 있다.**
- android 에서 id가 포함되어있는 + 다른 형식의 createdAt 의 데이터는 date가 불러와지지만 나머지 값들은 date가 불러와지지 않는다
- data 에 date를 불러오려고 하면 다음과같은 결과를 반환한다
  - [`data.date] = undefined`
  - `data[’date’] = undefined`
  - `Object.values = [0.04409162, "XAG", "12321cc-1233223123-29de1a81", 1123213, "2023-08-11"]`
- 각 요소를 탐색 하였을 때 date는 undefined 를 밷지만 배열의 값만 반환하는 Object.values 를 사용 하였을 때 정상적으로 모든 값을 얻을 수 있다.

## 해결

**해결코드 :**

```jsx
if (Platform.OS === 'android') {
  _list.forEach(e => {
    const listData = e.data();

    const {value, createdAt, category, ...prop} = listData;
    const propKey = Object.keys(prop);

		const resultDate =
			propKey.includes('id') ? prop.date : Object.values(prop)[0],
  });
```

- js es6인 디스트럭쳐링을 이용하여 유사하게 data를 필터하여 최대한 date값만 남게 처리하였습니다.
- 필터된 `prop`라는 값에 Object.keys를 이용하여 key값만 분류하여 `propsKey`에 입력합니다.
- `propsKey`에는 `‘id’` 라는 key값이 있을 수 있고 id가 있는 데이터 값들은 .date 또는 [’date’]로 날짜가 추출되기 때문에 그대로 사용합니다.
- 기존에 undefined가 되던 .date와 [’date’] 는 사용하지 않고 Object.values를 이용하여 date값을 정상적으로 얻을 수 있습니다.
