# TypeScript入門 

[udemy(ja)](https://www.udemy.com/course/typescript-complete/)

[youtube(en)](https://www.youtube.com/watch?v=BwuLxPH8IDs)

[TypeScript Deep Dive](https://typescript-jp.gitbook.io/deep-dive/)

[公式](https://www.typescriptlang.org/)

## 0. 初めに
### 0-1. TypeScriptとは
- JavaScriptにコンパイルされる
- 静的型システムがある
- JavaScriptのSuperset(上位集合：追加機能)である
  - 型検査機能がメイン

### 0-2. セットアップ
**インストール**
```bash
$ npm install -g typescript
```

**実行**
- typescriptの拡張子は`.ts`
```bash
$ tsc index.ts
```

### 0-3. TypeScriptを利用する理由
1. ドキュメントになる
関数を見ると使い方(何を渡して何が取得できるか)がわかるため、読みやすいコードになる。（サードパッケージも読みやすくなる！）
```ts
function add(a: number, b: number): number {
  return a + b;
}
```
2. Linter（コード検証ツール）になる
jsだと実行時にしかわからないエラーが、tsはコンパイル時に確認できる.  
  
3. ES5に対応することができる（トランスパイル(Babel)）
デフォルトだとES3になる
バージョン指定方法は下記
```bash
$ tsc index.ts --target ES2015
$ tsc index.ts --target ES6
```

## 1. TypeScriptの書き方

### 1-1. 型の種類
```ts
// 1. Boolean型
let hasValue: boolean = true; 

// 2. Number型
let count: number = 10;       

// 3. String型
let name: string = 'taro';    

// 4. Object型
const person : {
  name: string;
  age: number
} = {
  name: 'jack',
  age: 21
}

// 5. Array型
const fruits: string[] = ['apple','banana'] //array型
const yy: [string, number] = ['hoge', 123] // string, numberの順かつ2つの場合のみ通す

// 6. Enum（列挙）型
enum CoffeeSize = {   // enum(列挙)型
  SHORT= 'SHORT',     // = xxx をなくすと0から採番
  TALL= 'TALL'
}
const coffee = { 
  hot: true,
  size: CoffeeSize.SHORT
}

// 7. Any型
let anything: any = true; //any型 なるべく使わないように

// 8. 複数の型
let union: number | string = 10; //number or string型
let unions: (number|string)[] = [21, 'hoge'] // 配列の場合

// 9. リテラル型
const apple: 'apple' = 'apple' //リテラル型 'apple'だけ
const apple = 'apple' //型推論でリテラル型になる
let size: 'S' | 'M' | 'L' = 'S' // enum型のように使える

// 10. type arias
type ClothSize = 'S' | 'M' | 'L'
let clothSize: ClothSize = 'L'
const cloth: {
  size: ClothSize
} = {
  size: 'M'
}

// 11. 関数
// 型推論は戻り値は有効, 引数は無効
// 基本的に型注釈をつけるべき
function add(num1: number, num2: number): number {
  return num1 + num2
}

// 12. void型
// return がundefinedの場合
function say(): void{
  console.log('hello')
}

// 13. null型
let n: null;

// 14. 関数の型
const anotherAdd:(n1: number, n2: number) => number = add;
const doubleNumber:(num: number): number = num => num * 2;

// コールバック関数 (number, callback)が引数
function doubleAndHandle(num: number, cb: (num: number) => number): void {
  cb(num * 2)
}

// 15. unknown型
// 値の代入はできるが使用するときは型チェックが必要
let uknInput: unknown;// なんでも入る、
let anyInput: any;    // なんでも入る、なんでも使える
let text: string;
uknInput = 'hello'
text = uknInput // error
text = anyInput // no error
// no error
if(typeof uknInput === 'string') {
  text = uknInput
}

// 16. never型
//何も返さない (returnしない(Errorや無限ループなど))
// ok!
function error(msg: string): never {
  throw new Error(msg)
}
// ok!
function loop(): never {
  while(true) {}
}
// error!!! 
function test(): never {
}


```

### 1-2. 型推論と型注釈
```ts
// 型推論 : 型を明記しない
let y = "型推論"  // 型が自動的にstringになる
y = 12            // errorになる（型がstringのため）

// 型注釈 : 型を明記する
let x :string = "型注釈"    // 型推論ができないものはこちら                
let name: string            // （型推論できない例）初期化しない変数
```

## 2. コンパイラ
### 2-1. コマンド
保存したときにコンパイルを実行する（watchモード）
```bash
$ tsc index --watch
```

tsconfig.json（typescriptの設定ファイル）の作成と実行
```bash
$ tsc --init
$ tsc
```

### 2-2. tsconfig.json
```js

{
  "compilerOptions": {
    "target": "es6",      // コンパイラのバージョン
    "module": "commonjs",
    "lib": ["ES6"],       // 定義ファイルを指定 
    "allowJs": true,      // jsをコンパイル対象に含む (default false)
    "checkJs": true,      // allowJsと一緒に使う。jsファイルのエラーチェック
    "declaration": true,    // 型定義ファイル(使い方のドキュメント)を作る .d.ts
    "declarationMap": true, // 上と同様
    "sourceMap" : true,     // ブラウザがtsを理解できるようにする
    "outDir" : "./dist",    // 一番効率の良い形でtsを指定したディレクトリにコンパイル
    "rootDir": "./src",     // コンパイルするディレクトリのルートを指定
    "noEmitOnError": true,  // errorが起こったらコンパイルしない
    "noImplicitAny" :true   // 暗黙的なAnyを使用したらエラーを表示
    "strictNullCheck": true // stringなどにnullを含むとエラー     
  },
  // コンパイルするものを指定する
  "include": [
    "index.ts"
  ],
  // コンパイルをしない物を指定する (includeで記載があってもコンパイルしない)
  "exclude": [ 
    "main.ts",        // ファイルの指定
    "node_modules",   // ディレクトリの指定
    "*.spec.ts"       // .spec.ts拡張子のコンパイルしない
    "**/compiler.ts"  // すべてのディレクトリ内（サブディレクトリを含む）のcompiler.tsのコンパイルをしない
  ],
  // コンパイルするものを指定する（excludeで記載があってもコンパイル）
  "files": [
    "tmp/compiler.ts"
  ]
}
```


