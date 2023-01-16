Tsc에대한 잡다한 정리 

# TypeScript Class

```
https://www.typescriptlang.org/docs/handbook/2/classes.html
```

상속 관련은 그냥 문서한번 더보셈 

private 는 왠만하면 외부접근 하지 않는것이 더 좋은데 

### over*

overloading: 인수 타입이나 수가 바뀜

overriding : 인수는 그대로인데 함수 기능이 바뀜 암튼 그럼 

# 접근제한자

```typescript
class Point {
    privat count = 10
}


const a = new Class()

a['private변수'] 로 접근가능 하지만 컴파일후 공개됨 반면

class Point {
    #count = 10
}
ㄱ같은 경우는 컴파일이 끝나고도 공개되지않으며 []로 escape hatch를 제공하지않아 
hard private 
```

# static

```typescript
static 
class Point{
    static a = 10
}


console.log(Point.a) // static 는 객체생성하지않고 접근가능 물론 맴버함수도
```

# `this`at Runtime in Classes

```typescript
TypeScript는 JavaScript의 런타임 동작을 변경하지 않으며 
JavaScript는 몇 가지 독특한 런타임 동작이 있는 것으로 유명하다는 
점을 기억하는 것이 중요합니다.

이에 대한 JavaScript의 처리는 실제로 이례적입니다

It’s important to remember that TypeScript doesn’t change the runtime behavior of JavaScript, and that JavaScript is somewhat famous for having some peculiar runtime behaviors.

JavaScript’s handling of this is indeed unusual:
```

```typescript
class MyClass {
  name = "MyClass"
  getName(){
    return this.name
  }
}
const c = new MyClass()
const obj = {
  name: 'obj',
  getName : c.getName,
}

// Print 'obj', not 'MyClass'
console.log(obj.getName())
```

위코드는 너무 예외적이라 가져옴 

보면 당연히 MyClass 가 출력되어야하는데 obj가 나옴 

**내가생각한 이유는 Tsc에서 class는 Object생산 축이기 때문에**

**class객체가 object내부로 들어가면 다르게 인식하는거같음**

# Arrow Functions

```typescript
class MyClass {
  name = 'MyClass'
  getName = () => {
    return this.name
  }
}

const c = new MyClass()
const g = c.getName

console.log(g())

// arrow name = () => {}

// console.log(g)   # () => { return this.name; }             
// console.log(g())  # [LOG]: "MyClass"
```

이것은 몇가지 장단점을 교환

- TypeScript로 확인되지 않아도(타입이 불명확해도??) 런타임에 정확함을 보장함

- 클래스 인스턴스가 이러한 방식으로 정의된 각 함수의 고유한 복사본을 갖기 때문에 더 많은 메모리 사용

- 기본 클래스 매서드를 가져올 생성자 체인에 항목이 없어서 파생(자식) 클래스에서 super.getName을 사용할수 없음 

# `this` parameters

- 컴파일중에 지워짐 

- arrow(화살표)함수 대신에 사용해서 매서드가 올바르게 호출되도록 `정적`으로 적용가능

- arrow함수는 `동적`임 아마??

```typescript
class MyClass {
  name = 'MyClass'
  getName(this:MyClass) {
    return this.name
  }
}

const c = new MyClass()
const g = c.getName

console.log(g())


// console.log(g)   
// # [LOG]: getName(){ return this.name }
// console.log(g())  
// # [ERR]: "Executed JavaScript Failed:" 
// # [ERR]: Cannot read properties of undefined (reading 'name') 
```

arrow와 반대의 trade-off를 가짐

- JS 호출자는 인식하지 못하고 잘못사용할수있음 

- 클래스 인스턴스당 하나가 아닌 클래스 정의당 하나만 할당

- **기본매서드는 여전히 super로 호출가능**

**super를 써야하고 정확하게 runtime시켜야할때 해줌**

# `this` Types

클래스 에서 `this`라는 특수 유형은 현재 클래스 `동적 참조` 많이 유용한듯?

```typescript
class Box {
  contents: string = ""
  set(value:string){ // (method) Box.set(value: string): this 함수의 추론을 this로함
    this.contents = value
    return this;
  }
}

class ClearableBox extends Box{
  clear() {
    this.contents = "";
  }
}

const a = new ClearableBox()
const b = a.set('hello') // const b: ClearableBox
```

타입 추론을 `this`로함 그래서 클래스가됨 

```typescript
class Box {
  content: string = "";
  sameAs(other: this) {
    return other.content === this.content;
  }
}

class DerivedBox extends Box {
  otherContent: string = "?";
}

const base = new Box();
const derived = new DerivedBox();
derived.sameAs(base);  // 
```

Box -- 파생 클래스가 있는경우 sameAs메서드는 이제 동일한 파생 클래스의 다른 인스턴스만 허용함 

![](C:\Users\jaewon\AppData\Roaming\marktext\images\2022-09-23-15-31-07-image.png)

### `this`-based type guards

> 특정 필드의 지연 유효성 검사에 쓰임 
> 
> 필요할때 다시봐 그만봄 

클래스 및 interface에서 매서드에 대한 반호나 위치에서 Type을 사용 가능 유형축소 하여 개체 축소가능   `this is Type`

# 제너릭 타입

```
https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Generic-%ED%83%80%EC%9E%85-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0
```

좀 꿀잼 

# ! 와 ?

## Non-null assertion operator

```typescript
접미에 붙는 느낌표(!) 로 해당 연산자가 null,undefined 이 아니라고 안언해
```

## optional chaining

```typescript
접미에 붙는 물음표(?) 로 해당 연산자가 undefined 나 null 이면 평가를 멈추고
undefined 를 return 해주도록함 에러가 나오지는 않게해
```

# TSC ERROR

```
https://bobbyhadz.com/blog/javascript-cannot-set-property-value-of-null
```

property - value -of -null

# Arrow Function Overload

[Typescript Overload Arrow Function - StackBlitz](https://stackblitz.com/edit/typescript-overload-arrow-function?file=index.ts)

굳 예시로 아주좋음 

# tsc 서로다른 변수 한번에 2개 생성

```typescript
const [accessToken, refreshToken] = await Promise.all([
      const a,
      const b,
    ])
```

# TSC 분명 타입이 맞는 데 자꾸 에러날때

https://soopdop.github.io/2020/12/01/index-signatures-in-typescript/

백틱 없는게 더 편할지도...
