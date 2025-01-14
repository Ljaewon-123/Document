# tsc builer pattern

```typescript
class Person {
  name: string
  age: number | undefined
  address: string | undefined
  constructor(builder: PersonBuilder){
    this.name = builder.name
    this.age = builder.age
    this.address = builder.address
  }

  static Builder(name: string){
    return new PersonBuilder(name)
  }

  hi(){
    console.log("hi")
  }
}

class PersonBuilder{
  name: string
  age: number | undefined
  address: string | undefined
  constructor(name: string){
    if(!name) throw Error('Name is required')
    this.name = name    
  }

  setAge(age: number){
    this.age = age
    return this
  }
  setAddress(address: string){
    this.address = address
    return this
  }

  build(){
    return new Person(this)
  }
}

console.log(
  Person.Builder('jaewon')
    .setAge(30)
    .build()
)
```

최근들어서야 디자인 패턴같은게 좀 와닿기 시작했다.

by zerocho
