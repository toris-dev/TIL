reduce() 메서드 형식
`array.reduce(reducer, initialValue)`
array.reduce()는 주어진 배열(array)의 모든 요소 각각에 함수를 (reducer)를 적용하고, 각 요소에서 반환된 결과들을 축적하여 하나의 값으로 최종 반환합니다.
이 때 축적이 이러우지기 전의 초기값이 initialValue 입니다.

```javascript
let shoppingCart = [
  {
    product: '자전거',
    qty: 1,
    price: 849.0,
  },
  {
    product: '에어팟',
    qty: 1,
    price: 249.0,
  },
  {
    product: '운동화',
    qty: 1,
    price: 90.99,

  },
  {
    product: '스웨터',
    qty: 2,
    price: 50.99,
  },
  {
    product: '양말',
    qty: 5,
    price: 5.99,
  },
];
const reducer = (acc, {qty, price}) => {
	return acc + qty * price
}

const answer = shoppingCart.reduce(reducer, 0) // 0은 초기값, reducer 는 적용할 함수

```