# typescript-study
타입스크립트 기초 공부

# OVERVIEW OF TYPESCRIPT

> 객체 관련
> 
- optional parameter (선택적 변수)
    - ?를 뒤에 붙여주면 타입을 선택적으로 쓸 수 있게 된다.
        
        ```tsx
        const player : {
            name: string,   
            age?: number   // age type => number | undefined
        } = {
            name: "nico"
        }
        
        if(player.age && player.age < 10) {
        	
        }
        ```
        
- Alias type
    - 반복적인 코드를 줄일 수 있게 해준다
    - 첫번째 글자 → 대문자
        
        ```tsx
        type Player = { // Alias type
            name: string,
            age?: number
        }
        
        const nico: Player = {
            name: "nico"
        }
        
        const sangjun : Player = {
            name: "sangjun",
            age: 25
        }
        ```
        
- 함수에서의 활용
    
    ```tsx
    type Player = {
        name: string,
        age?: number
    }
    
    function playerMaker(name: string): Player {
        return {
            name
        }
    }
    
    // 화살표 함수
    // const playerMaker = (name: string): Player => ({name})
    // 화살표 함수2
    // const playerMaker = (name: string): Player => {
    //    return {
    //        name
    //    }
    // }
    
    const nico = playerMaker("nico");
    nico.age = 13;
    
    console.log("nico", nico);
    ```

- readonly
    
    ```tsx
    type Player = {
        readonly name: string,
        age?: number
    }
    
    // 화살표 함수
    const playerMaker = (name: string): Player => ({name})
    
    const nico = playerMaker("nico");
    nico.age = 13;
    // nico.name = "nico2"  readonly로 인해 수정 불가능
    
    console.log("nico", nico);
    ```
    
    ```tsx
    const names: readonly string[] = ["1", "2"];
    
    // names.push("3"); readonly로 인해 error
    // map, filter는 가능
    ```

> Tuple
> 
- array 생성
- 최소한의 길이를 가져야 한다.
- 특정 위치에 특정 타입이 있어야 한다.

```tsx
const player : [number, string, boolean] = [25, "sangjun", true];

// readonly도 같이 사용 가능
const player : readonly[number, string, boolean] = [25, "sangjun", true];
```

> any
> 
- 모든 타입 가능
- Typescript의 보호장치로부터 벗어나기 위해 사용
- 권장 X, 필요때가 있긴 하다.

> unknown
> 
- 변수의 타입을 미리 알지 못할 때 사용
- 타입을 확인해 줘야 한다.

```tsx
let a:unknown;

let b = a + 1       // 오류

if(typeof a === 'number') {
    let b = a + 1;
}

if(typeof a === 'string') {
    let b = a.toUpperCase();
}
```

> void
> 
- 아무것도 return 하지 않는 함수를 대상으로 사용

```tsx
function a(): void {
    console.log("hi");
}

a();
```

> return
> 
- 함수가 절대 return 하지 않을 때 사용

```tsx
function a(): void {
    console.log("hi");
}

a();
```

> never
> 
- 항상 오류를 출력하거나 리턴 값을 절대로 내보내지 않는다.

# FUNCTIONS

> call signature
> 

```tsx
type Add = (a: number, b: number) => number; // call signature 

const add: Add = (a, b) => a + b
```

> Overloading
> 
- 함수가 여러개의 call signatures를 가지고 있을때 발생
- 동일한 이름에 매개 변수와 매개 변수 타입 도는 리턴 타입이 다른 여러 버전의 함수를 만드는 것을 말한다.

```tsx
type Add = {
    (a: number, b: number): number;
    (a: number, b: number, c:number): number;
}

const add: Add = (a, b, c?:number) => {
    if(c) {
        return a + b + c;
    } else {
        return a + b;
    }
}

add(1,2);
add(1,2,3);
```

> Polymorphism
> 
- 다형성

> generic
> 
- call signature를 작성할 때 generic을 사용

```tsx
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]): void
}

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i));
}

superPrint([1,2,3,4]);
superPrint([true, false, true]]);
superPrint([1,2,3,4]                             );
superPrint([1,2,3,4]);
```

# CLASSES AND INTERFACES

> 추상클래스
> 
- 다른 클래스가 상속받을 수 있는 클래스
- 직접 새로운 인스턴트를 만들 수는 없다.

```tsx
abstract class User { // 추상 클래스
    constructor (
        protected firstName: string,
        protected lastName: string,
        protected nickname: string
    ) {}
    abstract getNickName(): void // 추상 메소드
    getFullName() {
        return `${this.firstName} ${this.lastName}`
    }
} 

class Player extends User {
    getNickName() {
        console.log(this.nickname);
    }
}

const nico = new Player("nico", "las", "니꼬");

nico.getFullName();
```

> 예제 (사전에 단어 추가 및 설명을 나타내는 기능)
> 

```tsx
type Words = {
    [key: string]: string // 제한된 양의 property 혹은 key를 가지는 타입을 정의해 주는 방법 (property의 일므은 모르지만 타입만을 알때 이걸 쓴다.)
}

class Dict {
    private words: Words
    constructor() {
        this.words = {}
    }
    add(word: Word) {
        if(this.words[word.term] === undefined) {
            this.words[word.term] = word.def;
        }
    }
    def(term:string) {
        return this.words[term];
    }
}

class Word {
    constructor(public term: string, public def: string) {} 
}

const kimchi = new Word("kimchi", "한국의 음식");

const dict = new Dict()

dict.add(kimchi);
dict.def("kimchi"); // 결과 : 한국의 음식
```

> 코드 첼린지 (업데이트, 삭제 기능 추가해보기)
> 

```tsx
type Words = {
    [key: string]: string // 제한된 양의 property 혹은 key를 가지는 타입을 정의해 주는 방법 (property의 일므은 모르지만 타입만을 알때 이걸 쓴다.)
}

class Dict {
    private words: Words
    constructor() {
        this.words = {}
    }
    add(word: Word) {
        if(this.words[word.term] === undefined) {
            this.words[word.term] = word.def;
        }
    }
    remove(word: Word) {
        if(this.words[word.term] !== undefined) {
            delete this.words[word.term];
        }
    }
    update(word: Word, newWord: Word) { // 전체 수정으로 구현
        if(this.words[word.term] !== undefined) {
            delete this.words[word.term];
            this.words[newWord.term] = newWord.def;
        }
    }
    def(term: string) {
        return this.words[term];
    }
}

class Word {
    constructor(public term: string, public def: string) {} 
}

const kimchi = new Word("kimchi", "한국의 음식");
const pizza = new Word("pizza", "이탈리아의 음식");

const dict = new Dict()

dict.add(kimchi);
dict.add(pizza);
dict.update(pizza, new Word("spagetti", "이탈리아의 음식2"));
console.log(dict);
```


> Type의 용도
> 
1. 객체의 모양을 설명할 수 있다.

```tsx
type Player = {
    nickname: string,
    healthBar: number
}

const nico: Player = {
    nickname: "nico",
    healthBar: 10
}
```

1. 타입을 정할 수 있다.

```tsx
type Food = string;

const kimchi: Food = "delicious";
```

1. 지정된 옵션으로 사용할 수 있다.

```tsx
type Team = "red" | "blue" | "yellow" // 특정 옵션
type Health = 1 | 5 | 10 // 특정 옵션

type Player = {
    nickname: string,
    team: Team,
    health: Health
}

const nico: Player = {
    nickname: "nico",
    team: "yellow",
    health : 10
}
```

> interface의 용도
> 
- 오브젝트의 모양을 특정해주기 위해서만 쓰인다.

```tsx
type Team = "red" | "blue" | "yellow"
type Health = 1 | 5 | 10

interface Player {
    nickname: string,
    team: Team, 
    health: Health
}

const nico: Player = {
    nickname: "nico",
    team: "yellow",
    health : 10
}
```