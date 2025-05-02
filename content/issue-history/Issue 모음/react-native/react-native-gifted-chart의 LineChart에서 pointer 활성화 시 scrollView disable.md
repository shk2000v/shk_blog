## 목표 :

- 그래프를 클릭했을 때 나오는 Pointer가 나오는데 해당 이벤트가 진행중 일때 LineChart를 감싸고있는 Scroll이 스크롤 되지 않게 disable 처리 하는것이 목표
  ![[issue_chart_target_explain.jpeg]]

## 이슈 사항:

- react-native-gifted-chart 에 문제가 있었다 바로
  - `ref를 받지 않는다는것`
  - `pointer를 보여주는 컴포넌트에 대하여 pressIn과 out이 따로 없다`

### 라이브러리의 상태?

- 해당 라이브러리에 작성된 LineChart 를 살펴보았다.
- props 중에서 getPointerProps라는것이 있다
- 해당 props는 포인터가 활성화 될때 {pointerIndex, pointerX, pointerY} 값을 받아서 callBack 해주는 Function 타입의 props이다.

  ![[issue_chart_code1.png]]

- 여기서 문제가 생기는데 우리가 목표로 하는 컨트롤이 `pointer가 보여지고 사라질때의 상태를 조절` 인데 해당 이벤트가 pointerX의 값에 의해서만 수정된다.
  ![[issue_chart_code2.png]]
- 때문에 pointerX값의 변화에 따라서만 다른 액션을 취할 수 있는것이다.

## 문제 해결 :

- 해결한 코드
  ```jsx
  <LineChart
  ...
  	getPointerProps={(item, index) => {
  	  const {pointerIndex, pointerX, pointerY} = item;
  	  if (pointerIndex === -1 && pointerX === 0 && pointerY === 0) {
  	    /**
  	     * 거의 모든 랜더링에 불필요한 랜더링중 다음값이 나온다.
  	     *  {"pointerIndex": -1, "pointerX": 0, "pointerY": 0}
  	     * 해당 조건이 걸릴떄는 return void 시켜 불필요한 이벤트 방지
  	     */
  	    return;
  	  } else if (item.pointerX === 0) {
  	    setIsScroll(true);
  	  } else {
  	    setIsScroll(false);
  	  }
  	}}
  ...
  />
  ```
- 설명 :
  1. `getPointerProps` 에는 item과 index 를 파라미터로 받고 item은 `{pointerIndex, pointerX, pointerY}` 을 포함하고 있습니다.
  2. LineChart의 Line을 누르고 있으면 Pointer가 나오고 `getPointerProps` 가 동작 하며 item에 포함된 요소를 받을 수 있습니다.
  3. `getPointerProps` 는 랜더링이 되자마자 실행되며 이때 item의 값은 `{pointerIndex = -1, pointerX = 0, pointerY = 0 }`
  4. 문제는 getPointerProps를 일으키는 이벤트가 실행 될 때(최초 랜더링, pressIn 상태일 때) 수많은 같은 값이 발생이 됩니다.
     ![[issue_chart_result_explain.png]]
  5. 때문에 이벤트 발생시 너무 많은 중복값 발생시에 void return을 하여 불필요한 이벤트 발생을 막고 필요한 때에만 이벤트를 넣었습니다.
