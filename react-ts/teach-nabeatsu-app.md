# 世界のナベアツを作る

###### tags: `React`, `TyapeScript`

<style>
.markdown-body img{
  display: block;
  width: 80%;
  margin: 0 auto;
}
.markdown-body .code-filename{
  display: block;
  position: relative;
  width: max-content;
  margin: 0 0 -34px auto;
  padding: 0 0.5em;
  right: 0;
  font-family: monospace;
  font-size: 12px;
  background: rgba(0, 0, 0, 0.15);
}
.markdown-body .colorsample{
  display: inline-block;
  width: 1em;
  height: 1em;
  vertical-align: middle;
  border: solid 1px #333;
  margin-right: 0.2em;
}
.markdown-body pre{
  max-height: 80vh;
}

</style>

世界のナベアツさんを作ることを通して、React の State について勉強します。

## 完成図

（写真）

## リポジトリを用意する

### 実装手順

1. [テンプレートリポジトリ](https://github.com/Kosuke2000/nextjs-ts-tailwind-starter) をクローンする

   リポジトリ名は nabeatsu-app とします。

2. VSCode で開き、ローカルホストを立ち上げる

## カウンターを作る

### 実装手順

1. pages/index.tsx ファイルに以下のようにコードを追加してください
   行頭が `+` となっているコードを追加してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Home: NextPage = () => {
   +  const [count, setCount] = useState(0);

   + const countUp = () => setCount((prevCount) => prevCount + 1);

     return (
       <div>
   			<!-- 省略 -->
         <main>
   +        <h1>{count}</h1>
   +        <button onClick={countUp}>Count Up!</button>
         </main>
   			<!-- 省略 -->
       </div>
     );
   };
   ```

2. 表示されている数字を大きくする
   [localhost:3000](https://localhost:3000) を確認し、Count Up! ボタンを押してみてください。

### 解説

#### State

[State](https://beta.reactjs.org/learn/state-a-components-memory)（ステート）とは、サイトの記憶です。ウェブサイトはユーザーの動きに応じて、その見た目を変えています。今回の実装だと、Count Up! ボタンを押すたびに、表示される数字が更新されるようになっていました。これは、ボタンを押した回数が記憶されており、それに応じてサイトの見た目が切り替わっていたと解釈できます。こうした、サイトの挙動に影響を与えるようなユーザーの行動を、React ではステートで管理します。

#### useState

[useState](https://beta.reactjs.org/apis/usestate#usestate)とは、ステートを追加する React Hook（ ≒ React の特別な関数）です。使い方は、以下の通りです。

```typescript
const [state, setState] = useState(initialState);
```

`state` はステートです。`serState` は `state`の更新処理を登録するセッター関数です。重要な点として、セッター関数が呼び出されると、[レンダリング](https://beta.reactjs.org/learn/render-and-commit)（ ≒ サイトの更新）が実行されます。`initailState`は、`state`の初期値になります。今回の実装を確認します。

<span class="code-filename">pages/index.tsx</span>

```typescript
const [count, setCount] = useState(0);
```

初期値`0`の`count`というステートと、`setCount`というセッター関数が追加されていますね。

#### onClick と Event Handler

`onClick` に渡された関数は、クリック時実行されます。今回の実装だと、 Count Up! ボタン をクリックすると、`onClick`に渡された`countUp`関数が実行されるようになっています。

<span class="code-filename">pages/index.tsx</span>

```typescript
<button onClick={countUp}>Count Up!</button>
```

countUp のようなユーザーの動きに応じて実行される関数のことを[Event Handler](https://beta.reactjs.org/learn/responding-to-events#adding-event-handlers)と呼びます。

#### 関数

関数について簡単に説明します。関数は、引数、処理、戻り値の 3 要素で構成されます。関数の書き方はいろいろありますが、とりあえず[アロー関数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)の書き方を紹介します。

```typescript
const f = (<引数>) => {
    <処理>
    return <最後の処理>
}
```

それでは、ボタンクリック時に実行される`countUp`にどんな処理が書かれているか確認しましょう。

<span class="code-filename">pages/index.tsx</span>

```typescript
const countUp = () => setCount((prevCount) => prevCount + 1);
```

`countUp` は引数を持たず、処理が一つなので `return` が省略されています。処理である `setCount((prevCount) => prevCount + 1)`とは、「『`count`に 1 足した数を、新たに`count`とする』という処理をレンダリング時に行う処理として登録する」という意味です。

### まとめ

Count Up! ボタン がクリックされてから、`count`が更新されるまでの流れをまとめます。

1. Count Up! ボタン がクリックされ、`onClick`に渡された`countUp`が実行される
2. `countUp`内の処理中に、`setCount`が実行され、レンダリングが起こる
3. レンダリングの終盤に、『`count`に 1 足した数を、新たに`count`とする』という処理が実行される。

これら 1~3 によって、ユーザーがボタンを押した回数を`count`として管理し、回数に応じて UI を切り替えることができます。

## 3 の倍数か 3 のつく数字でアホになる

### 実装手順

1. count が、3 の倍数または 3 の付く数字なのかを判定する
   以下のコードを Home コンポーネントに追加してください

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Home: NextPage = () => {
   // 省略

   +  const initialState = count === 0;
   +  const countMultipleOfThree = count % 3 === 0;
   +  const countHasThree = Boolean(count.toString().match(/3/));
   +  const isAho = !initialState && (countMultipleOfThree || countHasThree);

   +  console.table({ initialState, countMultipleOfThree, countHasThree, isAho});

   return (
       <!-- 省略 -->
   ```

2. 開発者ツールから真偽値を確認する
   [開発者コンソール](https://ja.javascript.info/devtools)という記事を参考に、開発者ツールの Console タブを開いてください。

### 解説

#### 演算子

演算子とは、「よく利用する演算処理を記号などで表現したもの」です（[演算子 · JavaScript Primer #jsprimer](https://jsprimer.net/basic/operator/#operator)）。今回の実装だと、`%` や`===`、 `!` 、 `&&`、 `||` が演算子でした。

```typescript
1 + 2;
```

この場合、演算子は `+` です。 `1` や `2` のような演算子の対象のことを被演算子（オペラント）と呼びます。

#### 算術演算子

[算術演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators#arithmetic_operators)は、数値 (リテラルまたは値) をオペランドとして取り、1 個の数値を返します。今回使用した[剰余 `%` ](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Remainder)が算術演算子の一つです。`%` は、1 つ目のオペランドが 2 つ目のオペランドで除算されたときに残った剰余を返します。

```typescript
const a = 4 % 3;
```

この場合、 `a` には `1` が代入されます。

#### 等値演算子

[等値演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators#equality_operators) では、評価結果は常に、比較が真（true）か偽（false）かに基づいて Boolean 型の値（= true か false）になります。 今回が使用した[厳密等価 `===`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Strict_equality)は等値演算子の 1 つです。 `===` は、二つのオペランドが等しいことを検査し、論理値（true か false）で結果を返します。また、オペランドの型が異なる場合、常に異なるもの（false）と判断します。

```typescript
const a = 0 === 0;
const b = "0" === 0;
```

この場合、 `a` には true が、`b` には false が代入されます。 `b`が false になるのは、同じ `0` でも、2 つのオペラントの型が異なるからです。左のオペラントは string 型（文字）なのに対して、右のオペラントは number 型（数字）です。

#### Boolean()

[Boolean()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)は、引数に渡された値の真偽を判定し、Boolean 型の値を返します。

```typescript
const a = 0;
const b = "0";

const aIsNumber = Boolean(a === 0);
const bIsNumber = Boolean(b === 0);
```

この場合、`aIsNumber` は true、 `bIsNumber`は false になります。

#### バイナリー論理演算子

[バイナリー論理演算子（以下、論理演算子）](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators#%E3%83%90%E3%82%A4%E3%83%8A%E3%83%AA%E3%83%BC%E8%AB%96%E7%90%86%E6%BC%94%E7%AE%97%E5%AD%90)とは、論理演算をする演算子で、それが置かれた時に真偽値（Boolean 型の値）を返します。今回の実装では、[論理積 `&&`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_AND) と[論理和 `||`](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_OR) を使いました。 `&&` は日本語の「かつ」、
`||` は「または」にあたります。

```typescript
const a = 3;
const b = -2;

const isPositive = a > 0 && b > 0;
const hasPositive = a > 0 || b > 0;
```

`isPositive` は false に、 `hasPositive` は true になります。

#### console

[`console`](https://developer.mozilla.org/en-US/docs/Web/API/console) とは、ブラウザのデバッグコンソールにアクセスする機能を提供してくれるものです。ここで、デバッグコンソールとは、開発者ツールの Console タブのことです。

<span class="code-filename">pages/index.tsx</span>

```typescript
console.table({ initialState, countMultipleOfThree, countHasThree, isAho });
```

今回の実装では、 `console.table` を使いました。 `console.table` によって、 例えば`count` が 3 の倍数のときデバッグコンソールに以下のように表示されます。

```typescript
{
    initialState: true,
    countMultipleOfThree:true,
    countHasThree: true,
    isAho: true
}
```

`console` が役に立つのは、UI 上に現れないデータを確認するときです。例えば、今回の実装だと、`initialState` や `countMultipleOfThree` は UI の中に表示されるわけではなく、UI を生成する処理過程でしか扱われませんでした。そこで、開発者が`initialState` や `countMultipleOfThree`のような UI に表示されていないものがうまく機能しているかを確認するために、 `console` とデバッグコンソールが存在しています。

### まとめ

今回の実装の意味を確認します。3 の倍数か 3 のつく数字で `isAho` が true をとるようになっています。

<span class="code-filename">pages/index.tsx</span>

```typescript
//  初期状態
const initial = count === 0;
//  3の倍数かどうかを判定
const isMultipleOfThree = count % 3 === 0;
//  3がつく数字かどうかを判定
const isNumberWithThree = Boolean(count.toString().match(/3/));
//  0を除き、3の倍数か3のつく数字であればtrueを返す
const isAho = !initial && (isMultipleThree || isNumberWithThree);
```

あわせて、`console.table`の挙動をもう一度確認してみてください。確認できたら、削除しましょう。

<span class="code-filename">pages/index.tsx</span>

```diff
- console.table({ initialState, countMultipleOfThree, countHasThree, isAho});
```

## アホのときに見た目を切り替える

### 実装手順

1. 行頭が `+` のコードを `Home` コンポーネントに追加してください

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Home: NextPage = () => {
     // 省略

   +  const nabeatsu = isAho ? "アホ" : "アホじゃない";

     return (
   	<!-- 省略 -->

   	  <main>
   +		<p>{nabeatsu}</p>
   		<h1>{count}</h1>
   		<button onClick={countUp}>Count Up!</button>
   	  </main>

   	<!-- 省略 -->

     );
   };
   ```

### 解説

#### 三項演算子

[三項演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)とは、3 つのオペランドをとる演算子です。条件に続いて疑問符 (?)、そして条件が真値であった場合に実行する式、コロン (:) が続き、条件が偽値であった場合に実行する式が最後に来ます。

```typescript
条件 ? <条件が true のときの処理> : <条件が false のときの処理>
```

今回の実装だと、`isAho = true` のときに `nabeatsu = "アホ"` 、 `isAho = false`のときに `nabeatsu = "アホじゃない"` となるようになっています。

<span class="code-filename">pages/index.tsx</span>

```typescript
const nabeatsu = isAho ? "アホ" : "アホじゃない";
```

#### JSX の中に JavaScript を書く

[JSX の中に Javascript 書く](https://beta.reactjs.org/learn/javascript-in-jsx-with-curly-braces)とは、以下のような場合です。ここでは、JSX の中に、`nabeatsu` `count` `countUp` という 3 つの JavaScript が書かれています。

<span class="code-filename">pages/index.tsx</span>

```typescript
const Home: NextPage = () => {
  const [count, setCount] = useState(0);

  const countUp = () => setCount((prevCount) => prevCount + 1);

  const nabeatsu = isAho ? "アホ" : "アホじゃない";

  return (
	<!-- 省略 -->

	  <main>
		<p>{nabeatsu}</p>
		<h1>{count}</h1>
		<button onClick={countUp}>Count Up!</button>
	  </main>

	<!-- 省略 -->

  );
};
```

このように、コンポーネントの `return` より上で宣言された定数、変数、関数、ステートを JSX の中で呼び出すときには、`{}`で囲んであげる必要があります。

### まとめ

1. `isAho` の値（true か false）によって、 `nabeatsu` に入る値（文字列）が変わる
2. `nabeatsu` が JSX で呼び出されることで、UI の一部として表示される

これで、世界のナベアツさんは然るべきときにアホになります。ここで完成とすることもできますが、コードの質にまだ改善の予知があります。そこで、次のセクションでは、React のカスタムフックを学んでいきます。

## カスタムフックを作ろう

### 実装手順

1.  以下のコードを `Home` コンポーネントの上に追加してください

    <span class="code-filename">pages/index.tsx</span>

    ```diff
    + function useNabeatsu(): [number, () => void, string] {
    +  return [count, countUp, nabeatsu];
    + }

    const Home:NextPage = () => {
    	<!-- 省略 -->
    ```

2.  行頭が `-` のコードを切り取り、 `useNabeatsu` に貼り付けてください

         <span class="code-filename">pages/index.tsx</span>

    ```diff
    const Home: NextPage = () => {
    - const [count, setCount] = useState(0);

    - const countUp = () => setCount((prevCount) => prevCount + 1);

    - const initialState = count === 0;
    - const countMultipleOfThree = count % 3 === 0;
    - const countHasThree = Boolean(count.toString().match(/3/));
    - const isAho = !initialState && (countMultipleOfThree || countHasThree);

    - const nabeatsu = isAho ? "アホ" : "アホじゃない";

     return (
     <!-- 省略 -->
    ```

         <span class="code-filename">pages/index.tsx</span>

    ```diff
    function useNabeatsu(): [number, () => void, string] {

    + const [count, setCount] = useState(0);

    + const countUp = () => setCount((prevCount) => prevCount + 1);

    + const initialState = count === 0;
    + const countMultipleOfThree = count % 3 === 0;
    + const countHasThree = Boolean(count.toString().match(/3/));
    + const isAho = !initialState && (countMultipleOfThree || countHasThree);

    + const nabeatsu = isAho ? "アホ" : "アホじゃない";

     return [count, countUp, nabeatsu];
     }

    ```

    これで、カスタムフック `useNabeatsu` が完成しました。

3.  カスタムフックを `Home` コンポーネントの中で呼び出します。

    <span class="code-filename">pages/index.tsx</span>

    ```diff
    const Home: NextPage = () => {
    + const [count, countUp, nabeatsu] = useNabeatsu();

     return (
     <!-- 省略 -->
    ```

### 解説

#### Hooks とは

[Hooks](https://reactjs.org/docs/hooks-intro.html)とは、 `use` から始める React 固有の特別な関数です。 `useState` も Hooks の一つです。 `useState` のように、基本的には React から提供されている Hook を使いますが、自分で Hook を作ることも可能です。自作の Hook を、カスタムフックと呼びます。

#### カスタムフックとは

[カスタムフック](https://reactjs.org/docs/hooks-custom.html)は、今回の実装だと `useNabeatsu` がカスタムフックになります。 `useNabeatsu` を作成しても、サイトの挙動は変わりません。では、どこにカスタムフックをを作るメリットがあるのでしょうか？

#### ロジックとビュー

開発において、コードは、ロジックに関わるものとビューに関わるものに分けたほうがいいとされています。ロジックとは演算処理やステートの更新処理のことです。ビューとは、見た目のことで、 React でいうと JSX の部分になります。カスタムフック `useNabeatsu` を作る前のコードを確認してみましょう。

<span class="code-filename">pages/index.tsx</span>

```typescript
const Home: NextPage = () => {
　// ロジック
  const [count, setCount] = useState(0);

  const countUp = () => setCount((count) => count + 1);

  const initialState = count === 0;
  const countMultipleOfThree = count % 3 === 0;
  const countHasThree = Boolean(count.toString().match(/3/));
  const isAho = !initialState && (countMultipleOfThree || countHasThree);

  const nabeatsu = isAho ? "アホ" : "アホじゃない";

  return (
  // ビュー
    <div className={styles.container}>
      <!-- 省略 -->

      <main className={styles.main}>
        <p>{nabeatsu}</p>
        <h1>{count}</h1>
        <button onClick={countUp}>Count Up!</button>
      </main>

    </div>
  );
};
```

ロジックに関わるコードも、ビューに関わるコードも、同じ `Home` コンポーネントに記述されていますね。これだと、ロジックとビューのどちらか一方に関わるコードに不具合が起きたときに、もう片方のコードにも影響を与えてしまう可能性がでてきます。これだと、サイトに不具合が起きてしまう（＝バグが生じる）危険性があります。

次に、 `useNabeatsu` 作成後のコードを確認します。

<span class="code-filename">pages/index.tsx</span>

```typescript
function useNabeatsu(): [number, () => void, string] {
  const [count, setCount] = useState(0);

  const countUp = () => setCount((count) => count + 1);

  const initialState = count === 0;
  const countMultipleOfThree = count % 3 === 0;
  const countHasThree = Boolean(count.toString().match(/3/));
  const isAho = !initialState && (countMultipleOfThree || countHasThree);

  const nabeatsu = isAho ? "アホ" : "アホじゃない";

  return [count, countUp, nabeatsu];
}

const Home: NextPage = () => {
  const [count, countUp, nabeatsu] = useNabeatsu();

  return (
    <div className={styles.container}>
      <!-- 省略 -->

      <main className={styles.main}>
        <p>{nabeatsu}</p>
        <h1>{count}</h1>
        <button onClick={countUp}>Count Up!</button>
      </main>

    </div>
  );
};
```

ロジックに関わるコードは`useNabeatsu` に、ビューに関わるコードは`Home` コンポーネントに分けることができました。

このように、コードの安全性のため、コードの責任（ロジック担当、ビュー担当）を分離することが推奨されています。これは単一責任の原則と言われています。

#### コードの可読性

コードの可読性とは、コードの読みやすさのことです。

::: info
個人開発ではない限り、コードはチームで共有するものです。ですので、誰が読んでもわかるコードを書くことを心がけることが大切です。
:::

カスタムフックは、コードの可読性を上げる手段の一つです。なぜなら、カスタムフックのおかげでコードを意味ごとにまとめ、コードの全貌を把握しやすくなるからです。`Home` コンポーネントの中で呼び出されている `useNabeatsu` を確認します。

<span class="code-filename">pages/index.tsx</span>

```typescript
const [count, countUp, nabeatsu] = useNabeatsu();
```

たった一行のコードに多くの情報が詰まっていることがわかりますか？ まず、`useNabeatus` という Hook 名から、世界のナベアツに関わるコードだということがわかります。そして、 `count` と `countUp` から、何かのカウントに関わるステートと、カウントを更新するセッター関数が返されているという検討がつきます。
つまり、これは、カスタムフックのおかげで、コードの全貌を理解するのに、コードを何行も読む必要がなくなったということです。カスタムフック `useNabeatsu`の導入でコードの可読性があがったと言えるでしょう。

### まとめ

- カスタムフックを作った
- コードの責任を分離する
- コードの可読性

コードの品質に関わる内容でした。ちゃんと動くコードを書くことはもちろん大事ですが、読みやすいコードを書くことも同じくらい重要だということを覚えておいてください。

## 参考文献
