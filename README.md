
## 타입스크립트 유래
- 자바스크립트의 동일 연산자 등에서 발생하는 한계를 해결하기 위해
- 자바스크립트의 존재하지 않는 프로 퍼티의 접근 허용을 망지하기 위해

## 핸드북 파트
# 1. 기본타입
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
  
# 2. 내장함수
- substring : string의 index 확인
  ```javascript
    console.log(x[0].substring(1)); // 성공
    console.log(x[1].substring(1)); // 오류, 'number'에는 'substring' 이 없습니다.
  ```

- toString() : number to string


# 3. 인터페이스
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
  
  
# 4. 함수
  - 아래와 같이 외부변수를 참조하는 것을 `capture`한다고 표현함
  ```javascript
  let z = 100;

  function addToZ(x, y) {
    return x + y + z;
  }
  ```
  
  - 여러개의 매개변수를 받은때
  ```javascript
  function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

// employeeName 은 "Joseph Samuel Lucas MacKinzie" 가 될것입니다.
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
  ```
  `...`을 사용하여 리스트 형식의 인자를 받을 수 있음
  
  - `this` 타입에 대한 이해 추가로 필요
  
  
# 5. 리터럴 표기법
  - 리터럴? : 데이터 값 그자체를 의미
  ```javascript
  const a = 1; // 1은 리터럴 , a 는 상수
  ```
  - 문자열 리터럴 타입
  enum과 같은 표기도 가능
  ```javascript
  type Easing = "ease-in" | "ease-out" | "ease-in-out";
  ```
  
# 6. 유니언타입과 교차타입
  - 문자의 타입확인 `typeof {변수명}`
  - 여러가지 타입을 받아야 하는 상황에서 `|`를 이용하여 여러 타입 입력 이를 유니언 타입이라 한다.

  ```javascript
  function padLeft(value: string, padding: string | number) {
  // ...
}
  ```
  
  - 공통으로 정의할 수 있는 타입들이 묶여 있다면 switch를 통해 조건을 걸어 확인 한다. 
  
  
  - 교차타입의 경우 `&`를 사용하여 두개 타입의 합집합인 케이스를 나타낸다. 
  ```javascript
    interface ErrorHandling {
    success: boolean;
    error?: { message: string };
  }

  interface ArtworksData {
    artworks: { title: string }[];
  }

  interface ArtistsData {
    artists: { name: string }[];
  }

  // 이 인터페이스들은
  // 하나의 에러 핸들링과 자체 데이터로 구성됩니다.

  type ArtworksResponse = ArtworksData & ErrorHandling;
  type ArtistsResponse = ArtistsData & ErrorHandling;

  const handleArtistsResponse = (response: ArtistsResponse) => {
    if (response.error) {
      console.error(response.error.message);
      return;
    }

    console.log(response.artists);
  };
  ```
  
  
  - 교차를 통한 믹스드인 패턴 : 사용할일이 있을까.. 

  
# 7. class
  - constructor : python __init__ 과 같은 역할
  ```javascript
    class Greeter {
      greeting: string;  // constructor에서 선언할 변수에 대한 타입정의
      constructor(message: string) {
          this.greeting = message;
      }
      greet() {
          return "Hello, " + this.greeting;
      }
  }

  let greeter = new Greeter("world");
  console.log(greeter.greet())
  ```
  
  - super() 이용해서 부모 class의 프로퍼티를 그대로 사용 할 수 있다.
  
  
- 특이사항
  클래스 선언후 외부에서 새롭게 함수를 지정할 수 있다. 하지만 외부선언 이후 다시 new를 통해 클래스를 정의 할 경우 해당 함수는 사용 할 수 없다.
  ```javascript
  class Person { 
    constructor (name,age, city) { 
  	this.name = name; this.age = age; this.city = city; 
  	} //메서드생성 nextYearAge() { return Number(this.age) + 1; } } 
  let kim = new Person('kim','24','seoul');
  
  kim.eat = function () { 
  		return 'apple' 
  	} 
 console.log(kim.nextYearAge()); 
  console.log(kim.eat());

  
  ```
  
 - `public`, `private`, `protected`    
public : default상태이다. 외부에서 접근가능
private: 클래스 외부에서 접근 하지 못하도록 설정,클래스 상속을 할경우 타입 호환이 가능, 다른 클래스 끼리는 상속 및 호환이 불가능 TypeScript 3.8에서는 비공개 필드 설정법 아래와 같이 추가됨
protected : private와 비슷하지만, 다른 클래스 끼리 호환 가능, private보다 조금 유한느낌
  ```javascript
  //case1 
  class Animal {
      #name: string;
      constructor(theName: string) { this.#name = theName; }
  }

  new Animal("Cat").#name; // 프로퍼티 '#name'은 비공개 식별자이기 때문에 'Animal' 클래스 외부에선 접근할 수 없습니다.

  //case2
  class Animal {
      private name: string;
      constructor(theName: string) { this.name = theName; }
  }

  new Animal("Cat").name; // 오류: 'name'은 비공개로 선언되어 있습니다;
  ```  
- `readonly` : 프로퍼티를 읽기 전용으로 바꿈 변경 불가, api 상에서 const등을 선언할때 사용
- setter , getter : 프로퍼티의 세부조건 초기 세팅을 위해
  
  
- `static`은 클래스가 메모리 올라갈 때 자종으로 생성된다. 클래스내에서 공통으로 변수를 공유하고 싶은 경우에만 사용 (값을 바꿀경우 각기 다른 인스턴스 내에서도 같은 값을 가지는 특징)
인스턴스(객체) 생성없이 바로 사용가능, this의 경우 인스턴스 생성시 사용
 
- `abstract class` : 추상 클래스, 인스턴스화 할 수 없다. 추상 클래스 내에서 표시된 매서드는 파생된 클래스 내에서 구현 되어야 한다. 
  
```javascript
  abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log("Department name: " + this.name);
    }

    abstract printMeeting(): void; // 반드시 파생된 클래스에서 구현되어야 합니다.
}

class AccountingDepartment extends Department {

    constructor() {
        super("Accounting and Auditing"); // 파생된 클래스의 생성자는 반드시 super()를 호출해야 합니다.
    }

    printMeeting(): void {
        console.log("The Accounting Department meets each Monday at 10am.");
    }

    generateReports(): void {
        console.log("Generating accounting reports...");
    }
}

let department: Department; // 추상 타입의 레퍼런스를 생성합니다
department = new Department(); // 오류: 추상 클래스는 인스턴스화 할 수 없습니다
department = new AccountingDepartment(); // 추상이 아닌 하위 클래스를 생성하고 할당합니다
department.printName();
department.printMeeting();
department.generateReports(); // 오류: 선언된 추상 타입에 메서드가 존재하지 않습니다
  ```
  
  - 클래스 자체를 타입으로 인스턴스를 생성 할 수 있다
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
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

  
  ### 3.private, protected 의미는 아는데 사용 이유? 해킹 방지?
  
  ### 4. strictPropertyInitialization :false 설정
  
  ```javascript
  class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
  ```
  - 위코드는 에러가난다. construct를 통해 사전에 변수를 선언해주지 않은게 문제인듯? 해결법으로 ts.config에서 `strictPropertyInitialization :false` 를 해주면됨

  
# 8. 열거형
  ```javascript
  enum Direction {
    Up = 1,
    Down,
    Left,
    Right,
}
  ```
  - Donw =2임 // 초기 숫자 비선언시 default =0
  
  
# 9. 제네릭
- 컴파일러가 타입을 유추할 수 없는 경우엔 명시적인 타입 인수 전달이 필요할 수도 있다. 이런경우 `any`를 사용하는 것을 지양하고 `identity`를 사용하여 넘겨받은 데이터를 타입으로 확정 짓는것이 포인트
```javascript
class Axis<T> {
    perone: T;
    pertwo: T;
    perthree: T;
    perfour: T;
    perfive: T;
}
```
  
