# Typescript 한글 핸드북 


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
  ```
  enum Color {Red = 1, Green, Blue}
  let colorName: string = Color[2];

  console.log(colorName); 
  ```
  
### 내장함수
- substring : string의 index 확인
  ```
    console.log(x[0].substring(1)); // 성공
    console.log(x[1].substring(1)); // 오류, 'number'에는 'substring' 이 없습니다.
  ```

- toString() : number to string





  
## 질문
### 1. Interfaces
명시적인 implements 절 없이, 인터페이스가 요구하는 형태를 사용하는 것만으로도 인터페이스를 구현할 수 있습니다. https://typescript-kr.github.io/pages/tutorials/typescript-in-5-minutes.html
