## Typescript 설치

```
yarn global add typescript
```

## tsconfig.json 파일 생성

```
{
	compilerOptions
}
```

## package.json 파일에 다음을 추가

```
// 주석은 제거하세요.
"scripts":{ // yarn start를 하면 자동으로 ts 파일을 js 파일로 컴파일 해줌
	"start":"node index.js",
	"prestart": "tsc" // yarn start 실행하면 prestart 먼저 실행함
}
```



Example

```
{
  "name": "typescript_with_blockchain",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts":{
    "start":"node index.js",
    "prestart": "tsc"
  }
}
```

## tsc-watch 설치

```
yarn add tsc-watch --dev
```

##### * tsc-watch 를 실행할 수 없는 경우

```
yarn global add typescript
```



## package.json 파일 내용 수정

```
// 주석은 제거하세요.
"scripts": {
    "start": "tsc-watch --onSuccess \" node dist/index.js\" "
    // tsc-watch 모드 : source 파일(.ts)가 바뀌면 바로 js 내용도 바꿔줌
  },
```



## 예제 설명

index.ts

```
interface Human2 {
  name: string;
  age: number;
  gender: string;
}

class Human {
  public name: string;
  public age: number;
  public gender: string;
  constructor(name: string, age: number, gender: string) {
    this.name = name;
    this.age = age;
    this.gender = gender;
  }
}

const lynn = new Human("lynn", 18, "female");
const sayHi = (person: Human2): string => {
  return `Hello ${person.name}, you are ${person.age}, you are a ${person.gender}`;
};

console.log(sayHi(lynn));

export {};
```



index.js - 컴파일 결과물

```
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
// interface 는 js로 컴파일 되면 사라진다.
// class는 남아있음
class Human {
    constructor(name, age, gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
}
const lynn = new Human("lynn", 18, "female");
const sayHi = (person) => {
    return `Hello ${person.name}, you are ${person.age}, you are a ${person.gender}`;
};
console.log(sayHi(lynn));
//# sourceMappingURL=index.js.map
```



또다른 예제

```
import * as CryptoJs from "crypto-js";
class Block {
  public index: number;
  public hash: string;
  public previuosHash: string;
  public data: string;
  public timestamp: number;

  static calculateBlockhash = (
    index: number,
    previousHash: string,
    timestamp: number,
    data: string
  ): string =>
    CryptoJs.SHA256(index + previousHash + timestamp + data).toString();

  constructor(
    index: number,
    hash: string,
    previuosHash: string,
    data: string,
    timestamp: number
  ) {
    this.index = index;
    this.hash = hash;
    this.previuosHash = previuosHash;
    this.data = data;
    this.timestamp = timestamp;
  }
}

const genesisBlock: Block = new Block(0, "20202020202020", "", "Hello", 123456);

let blockchain: Block[] = [genesisBlock];

const getBlockchain = (): Block[] => blockchain;

const getLatestBlock = (): Block => blockchain[blockchain.length - 1];

const getNewTimeStamp = (): number => Math.round(new Date().getTime() / 1000);

export {};


const genesisBlock: Block = new Block(0, "20202020202020", "", "Hello", 123456);

// 다음과 같이 [] 배열 형태의 타입도 지정 가능하다.
let blockchain: Block[] = [genesisBlock];

console.log(blockchain);

export {};
```



## Typescript의 import 방식은 조금 다르다

```
import * as CryptoJs from "crypto-js";
```



## 예제코드

```
import * as CryptoJs from "crypto-js";
class Block {
  static calculateBlockhash = (
    index: number,
    previousHash: string,
    timestamp: number,
    data: string
  ): string =>
    CryptoJs.SHA256(index + previousHash + timestamp + data).toString();

  static validateStructure = (aBlock: Block): boolean =>
    typeof aBlock.index === "number" &&
    typeof aBlock.hash == "string" &&
    typeof aBlock.previousHash === "string" &&
    typeof aBlock.timestamp === "number" &&
    typeof aBlock.data === "string";

  public index: number;
  public hash: string;
  public previousHash: string;
  public data: string;
  public timestamp: number;

  constructor(
    index: number,
    hash: string,
    previuosHash: string,
    data: string,
    timestamp: number
  ) {
    this.index = index;
    this.hash = hash;
    this.previousHash = previuosHash;
    this.data = data;
    this.timestamp = timestamp;
  }
}

const genesisBlock: Block = new Block(0, "20202020202020", "", "Hello", 123456);

let blockchain: Block[] = [genesisBlock];

const getBlockchain = (): Block[] => blockchain;
const getLatestBlock = (): Block => blockchain[blockchain.length - 1];
const getNewTimeStamp = (): number => Math.round(new Date().getTime() / 1000);

const createNewBlock = (data: string): Block => {
  const previosBlock: Block = getLatestBlock();
  const newIndex: number = previosBlock.index + 1;
  const newTimestamp: number = getNewTimeStamp();
  const newHash: string = Block.calculateBlockhash(
    newIndex,
    previosBlock.hash,
    newTimestamp,
    data
  );
  const newBlock: Block = new Block(
    newIndex,
    newHash,
    previosBlock.hash,
    data,
    newTimestamp
  );
  addBlock(newBlock);
  return newBlock;
};

const getHashforBlock = (aBlock: Block): string =>
  Block.calculateBlockhash(
    aBlock.index,
    aBlock.previousHash,
    aBlock.timestamp,
    aBlock.data
  );

const isBlockValid = (candidateBlock: Block, previousBlock: Block): boolean => {
  if (!Block.validateStructure(candidateBlock)) {
    return false;
  } else if (previousBlock.index + 1 !== candidateBlock.index) {
    return false;
  } else if (previousBlock.hash !== candidateBlock.previousHash) {
    return false;
  } else if (getHashforBlock(candidateBlock) !== candidateBlock.hash) {
    return false;
  } else {
    return true;
  }
};

const addBlock = (candidateBlock: Block): void => {
  if (isBlockValid(candidateBlock, getLatestBlock())) {
    blockchain.push(candidateBlock);
  }
};

createNewBlock("second block");
createNewBlock("third block");
createNewBlock("fourth block");

console.log(blockchain);

export {};
```

