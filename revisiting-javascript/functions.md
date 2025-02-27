---
description: JavaScriptの関数をおさらいします。
---

# JavaScriptの関数

ここでは、JavaScriptの関数にどんな特徴があるのかを復習します。他の言語からJavaScriptにやってきたプログラマが、「JavaScriptの関数ってそんな仕様があったの！？」と意外に感じる部分を中心に解説していきます。このセクションを読むことで、関数のよくある思い込みを解消でき、予期しないバグを未然に回避できるようになります。

## 関数は値

他の言語では関数は特別な立ち位置のことがあります。ある言語では、同じ名前の変数を定義してもエラーにならないのに対し、同じ名前の関数定義はエラーになります。またある言語では、関数を変数に代入できなかったりします。

JavaScriptの関数は値です。つまり、PHPのような他の言語と比べると特別扱いの度合いが少ないです。たとえば、関数を変数に代入することができます。

```javascript
function hello() {
  return "Hello World";
}

const helloWorld = hello; // 関数を変数に代入

helloWorld(); // 関数呼び出しも問題なくできる
```

また、定義済みの関数と同じ名前の関数を宣言することができます。これはエラーにはなりません。これは実質、再代入のような振る舞いになります。

```javascript
function hello() {
  return "HELLO";
}

// これは二度目の関数宣言ですが、実質的には再代入です
function hello() {
  return "KONNICHIWA";
}

hello(); //=> KONNICHIWA
```

このようにJavaScriptの関数は、bool値やstring値などと同じような値としての性質があります。

意図しない再代入はバグの原因になりますが、関数宣言では注意して書く以外に方法はありません。関数の再代入によるバグを未然に回避したい場合は、`const`と関数式を組み合わせます。関数式については後述します。

```javascript
const hello = function () {
  return "HELLO";
};
```

ちなみに、TypeScriptではコンパイラーが重複した関数宣言を警告してくれるので、バグの心配はありません。

## 関数はオブジェクト

関数はオブジェクトです。したがって、関数にプロパティを持たせることができます。

```javascript
function hello() {
  return "Hello World";
}

hello.prop = 123;
```

ただし、これはあまり使われるテクニックではありません。仕様上できるということです。

## 関数のスコープ

関数は値なので、関数名のスコープも変数と同じようにスコープの概念があります。たとえば、関数スコープの中で定義された関数は、そのローカルスコープでのみ使うことができます。

```javascript
function main() {
  // ローカルスコープの関数
  function hello() {
    console.log("hello");
  }

  hello();
}

main(); //=> "hello"

// ローカルスコープで宣言された関数にはアクセスできない
hello(); //=> ReferenceError: hello is not defined
```

## 関数宣言と関数式

JavaScriptの関数の定義方法には、関数宣言と関数式の2つがあります。関数宣言はfunction**構文**を使って関数を定義します。

```javascript
// 関数宣言
function hello() {
  return "hello";
}
```

関数式を用いて関数を定義するには、function**式**を用います。関数式は式なので、関数の終わりにはセミコロンを書きます。

```javascript
// 関数式で関数を定義
let hello = function () {
    return "hello";
}; //←セミコロン
```

この2つは書き方が異なりますが、どちらの書き方をしても関数の呼び出し方は同じです。

上の`hello`関数の例だと、後述する巻き上げを除いて、書き方以外に大きな違いはあまりありません。書き方が違うのでニュアンスの違いが出てきます。関数宣言は専用の構文を使っているので、関数として目を引きますし、関数定義であることが強調されるような感じがあります。一方の関数式は、「関数も値の一種である」という側面が強調され、関数といえども特別扱いされていない感じがあります。どちらで書いても機能的にはほぼ同じという場合、どちらでも構わないということになります。どちらを普段使いにするかは、プログラマのスタンスによるところが大きいです。

## 巻き上げ

関数宣言と関数式の違いが現れるひとつの例は巻き上げ\(hoisting\)です。関数宣言には巻き上げがあり、関数式には巻き上げがありません。

_TODO: **変数とスコープのページの巻き上げを解説する部分にリンクを入れる**_

まずは関数宣言の例を見てみましょう。次のコードは、3行目に`hello`関数の関数宣言があります。そして、その宣言の前で`hello`関数を実行しています。

```javascript
hello();

function hello() {
  console.log("Hello World");
}
```

このコードは、`hello`関数の定義行より前でその関数を呼び出しているのに、エラーにはならず問題なく"Hello World"が出力されます。これは関数宣言には巻き上げがあるためです。

次に関数式の例を見てみましょう。下のコードは`hello`関数を関数式を使って定義するようにしたものです。

```javascript
hello();

const hello = function () {
  console.log("Hello World");
};
```

このコードは実行してみると、1行目で「ReferenceError: Cannot access 'hello' before initialization」というエラーが起こります。関数式で関数を定義した場合は巻き上げがないため、このようなエラーが発生します。

以上のように、関数宣言と関数式には巻き上げの有無の違いがあります。関数式の場合は、関数定義と実行の順番を意識する必要が出てくるので、この点は注意しましょう。

## アロー関数

関数式にはもうひとつの書き方があります。それがアロー関数です。

```javascript
// function式を用いた関数式
const hello = function (name) {
  return `Hello, ${name}!`;
};

// アロー関数の関数式
const hello = (name) => {
  return `Hello, ${name}!`;
};
```

アロー関数は`function`式に比べて短く書けるのが特徴的です。上の`hello`関数はさらに引数のカッコや`return`を省略して書くこともできます。

```javascript
const hello = name => `Hello, ${name}!`;
```

`return` を省略したアロー関数でオブジェクトリテラルを返したい時はそのまま返すことができません。

```typescript
const func = () => {x: 1};

console.log(func());
// -> undefined
```

このときはオブジェクトリテラルを`()` で括ることで返すことができます。

```typescript
const func = () => ({x: 1});

console.log(func());
// -> { x: 1 }
```

アロー関数は関数式をより手軽に書けるものですが、厳密にはアロー関数は書き方が違うだけではありません。が、まずはアロー関数が`function`式の代替えとして、多くの場合、使えることを覚えておくとよいです。それを知った上で、2つの違いを詳しく知りたい方は「[通常の関数とアロー関数の違いは「書き方だけ」ではない。異なる性質が10個ほどある](https://qiita.com/suin/items/a44825d253d023e31e4d)」の解説を御覧ください。

## Generator

Generatorを使用した関数はアロー関数での表記ではなく、必ず`function*() {}`と書く必要があります。次は可能なGeneratorの記述方法です。

```typescript
function* generatorFunction1() {
  // ...
};

const generatorFunction2 = function* () {
  // ...
};

class GeneratorSample {
  public* generatorFunction3() {
    // ...
  }
}
```

Generatorは反復可能\(`Iterable<T>`\)な反復子\(`Iterator<T>`\)であるインターフェース`IterableIterator<T>`を実装したクラスのオブジェクトのことです。条件を満たしたクラスはGenerator関数の中で`yield`キーワードを使えます。`yield`は呼ばれたときに一度その値を呼び出し元へ返却し、次に呼ばれたときはその続きから処理を開始します。

`Promise`が一般化する以前、非同期処理を代わりに担当する目的でGeneratorが使われていたことはありますが、前述の`Promise`に加えて`async / await`が一般的に使われるようになってから非同期処理の目的でGeneratorを使う機会は減りました。現在でも大量のデータを取得したいときに一度ではなく、小出しに取得したいときにGeneratorは使い道があります。

