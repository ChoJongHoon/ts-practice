# ts-practice

[velpert](https://velog.io/@velopert)님의 [TypeScript 강의](https://velog.io/@velopert/series/react-with-typescript) 실습

# 타입스크립트 기초 연습

`tsconfig.json`

- **target:** 컴파일된 코드가 어떤 환경에서 실행될 지 정의합니다. 예를들어서 화살표 함수를 사용하고 `target` 을 `es5` 로 한다면 일반 `function` 키워드를 사용하는 함수로 변환을 해줍니다. 하지만 이를 `es6` 로 설정한다면 화살표 함수를 그대로 유지해줍니다.
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

## interface 사용해보기

`interface`는 클래스 또는 객체를 위한 타입을 지정 할 때 사용되는 문법입니다.

### 클래스에서 interface 를 implements 하기

```typescript
// Shape 라는 interface 를 선언합니다.
interface Shape {
  getArea(): number; // Shape interface 에는 getArea 라는 함수가 꼭 있어야 하며 해당 함수의 반환값은 숫자입니다.
}

class Circle implements Shape {
  // `implements` 키워드를 사용하여 해당 클래스가 Shape interface 의 조건을 충족하겠다는 것을 명시합니다.
  constructor(public radius: number) {
    this.radius = radius;
  }

  // 너비를 가져오는 함수를 구현합니다.
  getArea() {
    return this.radius * this.radius * Math.PI;
  }
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {
    this.width = width;
    this.height = height;
  }
  getArea() {
    return this.width * this.height;
  }
}

const circle = new Circle(5);
const rectangle = new Rectangle(10, 5);

console.log(circle.radius); // 에러 없이 작동
console.log(rectangle.width); // width 가 private 이기 때문에 에러 발생!

const shapes: Shape[] = [new Circle(5), new Rectangle(10, 5)];

shapes.forEach(shape => {
  console.log(shape.getArea());
});
```

### 일반 객체를 interface 로 타입 설정하기

`interface`를 선언 할 때 다른 `interface`를 `extends`해서 상속받을 수 있습니다.

```typescript
interface Person {
  name: string;
  age?: number; // 물음표가 들어갔다는 것은, 설정을 해도 되고 안해도 되는 값이라는 것을 의미합니다.
}
interface Developer extends Person {
  skills: string[];
}

const person: Person = {
  name: "김사람",
  age: 20
};

const expert: Developer = {
  name: "김개발",
  skills: ["javascript", "react"]
};

const people: Person[] = [person, expert];
```

> interface에 대한 더 자세한 내용 확인하기 [TypeScript-Handbook](https://typescript-kr.github.io/pages/Interfaces.html)

## Type Alias 사용하기

`type`은 특정 타입에 별칭을 붙이는 용도로 사용합니다. 이를 사용하여 객체를 위한 타입을 설정할 수도 있고, 배열, 또는 그 어떤 타입이던 별칭을 지어줄 수 있습니다.

```typescript
type Person = {
  name: string;
  age?: number; // 물음표가 들어갔다는 것은, 설정을 해도 되고 안해도 되는 값이라는 것을 의미합니다.
};

// & 는 Intersection 으로서 두개 이상의 타입들을 합쳐줍니다.
// 참고: https://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types
type Developer = Person & {
  skills: string[];
};

const person: Person = {
  name: "김사람"
};

const expert: Developer = {
  name: "김개발",
  skills: ["javascript", "react"]
};

type People = Person[]; // Person[] 를 이제 앞으로 People 이라는 타입으로 사용 할 수 있습니다.
const people: People = [person, expert];

type Color = "red" | "orange" | "yellow";
const color: Color = "red";
const colors: Color[] = ["red", "orange"];
```

### `type`과 `interface`어떨 때 사용해야 할까?

무엇이든 써도 상관 없는데 일관성 있게만 쓰면 됩니다.
구버전의 타입스크립트에서는 `type`과 `interface`의 차이가 많이 존재했었는데 이제는 큰 차이는 없습니다.
다만 라이브러리를 작성하거나 다른 라이브러리를 위한 타입 지원 파일을 작성하게 될 때는 `interface`를 사용하는것이 권장되고 있습니다.

#### 관련글

- [Interface vs Type alias in TypeScript 2.7](https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c)
- [TypeScript에서 Type을 기술하는 두 가지 방법, Interface와 Type Alias](https://joonsungum.github.io/post/2019-02-25-typescript-interface-and-type-alias/)

## Generics

제네릭(Generics)은 타입스크립트에서 함수, 클래스, `interface`, `type`을 사용하게 될 때 여러 종류의 타입에 대하여 호환을 맞춰야 하는 상황에서 사용하는 문법입니다.

### 함수에서 Generics 사용하기

```typescript
function merge<A, B>(a: A, b: B): A & B {
  return {
    ...a,
    ...b
  };
}

const merged = merge({ foo: 1 }, { bar: 1 });
```

제네릭을 사용 할 때는 이렇게 `<T>`처럼 꺽쇠 안에 타입의 이름을 넣어서 사용하며, 이렇게 설정을 해주면 제네릭에 해당하는 타입에는 뭐든지 들어올 수 있게 되면서도, 사용 할 때 타입이 깨지지 않게 됩니다. 만약 함수에 이렇게 제네릭을 사용하게 된다면 함수의 파라미터로 넣은 실제 값의 타입을 활용하게 됩니다.

```typescript
function wrap<T>(param: T) {
  return {
    param
  };
}

const wrapped = wrap(10);
```

이렇게 함수에서 제네릭을 사용하면 파라미터로 다양한 타입을 넣을 수도 있고 타입 지원을 지켜낼 수 있습니다.

### interface 에서 Generics 사용하기

```typescript
interface Items<T> {
  list: T[];
}

const items: Items<string> = {
  list: ["a", "b", "c"]
};
```

만약 `Items<string>`라는 타입을 사용하게 된다면, `Items`타입을 지닌 객체의 `list`배열은 `string[]`타입을 지니고 있게 됩니다. 이렇게 함으로써, `list`가 숫자 배열인 경우, 문자열 배열인 경우, 객체배열, 또는 그 어떤 배열인 경우에도 하나의 `interface`만을 사용하여 타입을 설정할 수 있습니다.

### Type alias 에서 Generics 사용하기

```typescript
type Items<T> = {
  list: T[];
};

const items: Items<string> = {
  list: ["a", "b", "c"]
};
```

### 클래스에서 Generics 사용하기

Generics 를 사용하여 Queue를 구현해봅시다.

```typescript
class Queue<T> {
  list: T[] = [];
  get length() {
    return this.list.length;
  }

  enqueue(item: T) {
    this.list.push(item);
  }

  dequeue() {
    return this.list.shift();
  }
}

const queue = new Queue<number>();

queue.enqueue(0);
queue.enqueue(1);
queue.enqueue(2);
queue.enqueue(3);
console.log(queue.dequeue());
console.log(queue.dequeue());
console.log(queue.dequeue());
console.log(queue.dequeue());
```

![image](https://user-images.githubusercontent.com/42956032/66895259-4dbdf500-f02d-11e9-82b3-51efb699f496.png)

> Generic 에 대해서 더 자세히 알아보기[TypeScript-Handbook](https://typescript-kr.github.io/pages/Generics.html)
