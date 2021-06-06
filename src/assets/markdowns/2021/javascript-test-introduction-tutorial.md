# JavaScript Introduction Tutorial - Unit Test, Integration Tests & e2e Tests

### 参考記事
- [youtube](https://www.youtube.com/watch?v=r9HdJ8P6GQI)  
- [Jest公式](https://jestjs.io/ja/)

## 0. Overview 

### 0-1. テストの種類
- Unit Tests（単体テスト） : Fully Isolated （テスト難易度は優しい、テスト量は多量）
- Integration Tests（組合わせテスト） : With Dependencies
- End-to-End (E2E) Tests（総合テスト） : Full Flow (テスト難易度は高い、テスト量は少量)

### 0-2. JavaScriptテストツール
- Jest
  - Test Runner (Mocha)
  - Assertion Library (Chai)
- Headless Browser (Puppeteer) 

## 1. Unit Test (単体テスト)
### 1-1. Jest のインストール
```bash
$ npm install --save-dev jest
```

### 1-2. util.test.js (Unit Tests)を作成
**Jest** は`.test.js`ファイルを自動で読みとりテストする。  
関数は引数(input)および戻り値(output)があるとUnit Testsがしやすく望ましい？

util.test.js
```js
// テスト対象関数を取得
const { generatieText } = require('./util')

// 第一引数にテストの説明、第二引数はテスト
test('should output name and age', () => {
  
  const text = generateText('Max', 29);  
  
  // expect(テストのinput内容).toBe(outputで期待されるもの)
  expect(text).toBe('Max (29 years old)');
});
```

util.js (テスト対象プログラム抜粋)
```js
exports.generateText = (name, age) => {
  // Returns output text
  return `${name} (${age} years old)`;
};
```

**point** 
- 1つの`test()`に対して複数の`expect().toBe()`が書くことで、テスト精度が上がる
- 単体テストは同じ関数に対して複数の`test()`を書くことで、テスト精度が上がる

### 1-3. package.json を修正
```js
{
  "scripts": {
    "test": "jest --watch"  // --watch をつけると常時起動
  }
}
```

### 1-4. テストの実行
```bash
$ npm run test
```

## 2. Integration Tests (組み合わせテスト)
**Jest** でテストを行う点では単体テストと同じだが、組み合わせテストは、複数の関数の組み合わせのテストを行う。

組み合わせテストも、Input, Outputを明確にすることで、テストを行いやすくする。

**point**
`exports.validateInput = ...`のように記載したままだと、同じファイルから参照できない。対策は下記。
```js
const validateInput = ...

exports.validateInput = validateInput
```

## 3. E2E Tests (総合テスト)

### 3-1. puppeteer install
chrome用テストツールpuppeteerのインストール
```bash
$ npm install --save-dev puppeteer
```

### 3-2. テストの記述
※ WSLだと起動できないのでWindowsで実行した
utils.test.js
```js
const puppeteer = require('puppeteer');

test('should create an element with text and correct class', async () => {
  const browser = await puppeteer.launch({
    headless: false,
    slowMo: 80,
    args: ['--window-size=1920,1080']
  });
  const page = await browser.newPage();
  await page.goto(
    'file:///C:/Users/ktguy/workspace/test/js-testing-introduction/index.html'
  );
  await page.click('input#name');
  await page.type('input#name', 'Anna');
  await page.click('input#age');
  await page.type('input#age', '28');
  await page.click('#btnAddUser');
  const finalText = await page.$eval('.user-item', el => el.textContent);
  expect(finalText).toBe('Anna (28 years old)');
}, 10000);
```