# tsc 커맨드, 어뎁터 패턴

```typescript
class Command {
  obj: any
  constructor(obj: any){
    this.obj = obj
  }

  run(){
    this.obj.run()
  }
}

class Copy{
  run(){ console.log('copy run') }
}

new Command(new Copy()).run()

class Paste {
  run(){ console.log('paste run') }
}
new Command(new Paste()).run()

class Cut{
  exe() { console.log('cutting run') }
}

class CutAdapter{
  obj:any
  constructor(obj: any){
    this.obj = obj
  }
  run(){
    this.obj.exe()
  }
}

new Command(new CutAdapter(new Cut())).run()
```

with 제로초 자료 
