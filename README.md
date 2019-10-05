# ts-practice

[velpert](https://velog.io/@velopert)님의 [TypeScript 강의](https://velog.io/@velopert/series/react-with-typescript) 실습

## 타입스크립트 기초 연습

`tsconfig.json`

- **target:** 컴파일된 코드가 어떤 환경에서 실행될 지 정의합니다. 예를들어서 화살표 함수를 사용하고 target 을 es5 로 한다면 일반 function 키워드를 사용하는 함수로 변환을 해줍니다. 하지만 이를 es6 로 설정한다면 화살표 함수를 그대로 유지해줍니다.
- **module:** 컴파일된 코드가 어던 모듈 시스템을 사용할지 정의합니다. 예를 들어서 이 값을 **common** 으로 하면 `export default Sample` 을 하게 됐을 때 컴파일 된 코드에서는 `exports.default = helloWorld` 로 **변환**해주지만 이 값을 **es2015** 로 하면 `export default Sample` 을 그대로 **유지**하게 됩니다.
- **strict:** 모든 타입 체킹 옵션을 활성화한다는 것을 의미합니다.
- **esModuleInterop:** commonjs 모듈 형태로 이루어진 파일을 es2015 모듈 형태로 불러올 수 있게 해줍니다. [(참고)](https://stackoverflow.com/questions/56238356/understanding-esmoduleinterop-in-tsconfig-file)

_추가_

> **outDir:** 컴파일된 파일들이 저장되는 경로를 지정 할 수 있습니다.

## 기본 타입

```typescript
let count = 0; // 숫자
count += 1;
count = "갑자기 분위기 문자열"; // 이러면 에러가 납니다!

const message: string = "hello world"; // 문자열

const done: boolean = true; // 불리언 값

const numbers: number[] = [1, 2, 3]; // 숫자 배열
const messages: string[] = ["hello", "world"]; // 문자열 배열

messages.push(1); // 숫자 넣으려고 하면.. 안된다!

let mightBeUndefined: string | undefined = undefined; // string 일수도 있고 undefined 일수도 있음
let nullableNumber: number | null = null; // number 일수도 있고 null 일수도 있음

let color: "red" | "orange" | "yellow" = "red"; // red, orange, yellow 중 하나임
color = "yellow";
color = "green"; // 에러 발생!
```

> 기본 타입에 대한 더 자세한 알아보기 [TypeScript-Handbook](https://typescript-kr.github.io/pages/Basic%20Types.html)

## 함수에서 타입 정의하기

```typescript
function sum(x: number, y: number): number {
  return x + y; // 숫자를 반환하지 않으면 에러 발생
}

sum(1, 2); // 함수의 파라미터로 숫자를 넣지 않으면 에러 발생
```

타입스크립트를 사용했을때 참 편리한 점은, 배열의 내장함수를 사용 할 때에도 타입 유추가 매우 잘 이루어진다는 것 입니다.

![image](https://user-images.githubusercontent.com/42956032/66253529-bc38c280-e7a4-11e9-9ddc-d18b92b7dd6b.png)

참고로 함수에서 만약 아무것도 반환하지 않아야 한다면 이를 반환 타입을 `void` 로 설정하면 됩니다.

```typescript
function returnNothing(): void {
  console.log("I am just saying hello world");
}
```

> 함수에 대하여 더 자세히 알아보기 [TypeScript-Handbook](https://typescript-kr.github.io/pages/Functions.html)
