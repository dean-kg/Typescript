# Typescript 한글 핸드북 정리


## 타입스크립트 유래
- 자바스크립트의 동일 연산자 등에서 발생하는 한계를 해결하기 위해
- 자바스크립트의 존재하지 않는 프로 퍼티의 접근 허용을 망지하기 위해

## 핸드북 파트
### 기본타입
- number : 부동소수(flaot) , 16진수, 10진수
- string : 문자열 , ${ expr } 방식의 formating
- Array : eg. number[] , Array<number>
- Tuple : eg. let x: [string,number]   // 타입이 같을 필요는 없음
- Enum : 열거 (기본적으로 0부터 시작, 처음숫자 바꾸면 그숫자부터 시작, 전체를 임의 지정가능)     
  ```javascript
  enum Color {Red = 1, Green, Blue}
  let colorName: string = Color[2];

  console.log(colorName); 
  ```
- any : 타입스크립트의 본질과 벗어나는 타입, 컴파일용이라 생각하기
- void : any와 반대 타입이 없음을 의미 , 함수의 반환값이 없는 경우 사용,  null , undefined만 선언 가능
- null , undefined : 타입으로 존재는 하지만 잘 안씀
- never : 오류발생용 혹은 반환하지 않는 타입을 정의 할때 사용
  ```javascript
    // never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
  function error(message: string): never {
      throw new Error(message);
  }

  // 반환 타입이 never로 추론된다.
  function fail() {
      return error("Something failed");
  }

  // never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
  function infiniteLoop(): never {
      while (true) {
      }
  }
  ```
  
- object : 원시 타입이 아닌 타입 의미, 여기서 원시 타입은 number, string, boolean ,bigint, symbol, null, undefined 
  
  ```javascript
  declare function create(o: object | null): void;

  create({ prop: 0 }); // 성공
  create(null); // 성공

  create(42); // 오류
  create("string"); // 오류
  create(false); // 오류
  create(undefined); // 오류
  ```
- 타입 단언(타입 강조를 위해)
  as , angle-bracket 방식으로 사용 JSX를 사용할 경우 as 스타일만 가능
  
- let : ES2015소개된 이후 var을 대체
  
### 내장함수
- substring : string의 index 확인
  ```javascript
    console.log(x[0].substring(1)); // 성공
    console.log(x[1].substring(1)); // 오류, 'number'에는 'substring' 이 없습니다.
  ```

- toString() : number to string


### 인터페이스
- 타입스크립트의 핵심원칙 : 덕타이핑, 구조적 서브타이핑 
  ```javascript
  interface LabeledValue {
    label: string;
  }

  function printLabel(labeledObj: LabeledValue) {
    console.log(labeledObj.label);
  }

  let myObj = { size: 10, label: "Size 10 Object" };
  printLabel(myObj);

  ```javascript
  객체가 실제로는 더 많은 프로퍼티를 갖고 있지만, 컴파일러느 ㄴ최소한 필요한 프로퍼티가 있는지만 확인, 이외로 타입스크립트가 관대하지않은 케이스가 있음

- `?` 를 이용하여 선택적 프로퍼티 가능
- `readonly` 를 이용하여 읽기 전용 프로퍼티 표현, 선언 후 수정 불가
  Array에서도 ReadonlyArray<T>와 같은 표현으로 배열 변경을 유지할수 있음
  `const`는 변수 `readonly`는 프로퍼티
  ```javascript
  interface Point {
    readonly x: number;
    readonly y: number;
  }
  ```
  
- 추가 프로퍼티가 존재 할 경우
  ```javascript
  interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
  }
  ```
  
  
### 함수
  아래와 같이 외부변수를 참조하는 것을 `capture`한다고 표현함
  ```javascript
  let z = 100;

  function addToZ(x, y) {
    return x + y + z;
  }
  ```
  
  여러개의 매개변수를 받은때
  ```javascript
  function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

// employeeName 은 "Joseph Samuel Lucas MacKinzie" 가 될것입니다.
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
  ```
  `...`을 사용하여 리스트 형식의 인자를 받을 수 있음
  
  - `this` 타입에 대한 이해 추가로 필요
  
  
### 리터럴 표기법
  - 리터럴? : 데이터 값 그자체를 의미
  ```javascript
  const a = 1; // 1은 리터럴 , a 는 상수
  ```
  - 문자열 리터럴 타입
  enum과 같은 표기도 가능
  ```javascript
  type Easing = "ease-in" | "ease-out" | "ease-in-out";
  ```
  
  
## 질문
### 1. Interfaces
명시적인 implements 절 없이, 인터페이스가 요구하는 형태를 사용하는 것만으로도 인터페이스를 구현할 수 있습니다. https://typescript-kr.github.io/pages/tutorials/typescript-in-5-minutes.html
  
### 2. 초과 프로퍼티 검사
  ```javascript
  interface LabeledValue {
    label: string;
  }

  function printLabel(labeledObj: LabeledValue) {
    console.log(labeledObj.label);
  }

  printLabel({ size: 10, label: "Size 10 Object" }); 

  
  
  let myObj = { size: 10, label: "Size 10 Object" }; 
  printLabel(myObj);
  
  
  
  printLabel({ size: 10, label: "Size 10 Object" } as LabeledValue); 
  
  
  
  
  let myObj :  LabeledValue = { size: 10, label: "Size 10 Object" }; 
  printLabel(myObj);
  네가지 케이스의 차이.?

  ```
