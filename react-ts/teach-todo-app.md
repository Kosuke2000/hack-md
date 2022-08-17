---
title: Todo リストを作る
description: React TypeScript を習得するためのノート
image: https://i.imgur.com/Ml6wuFm.png
---

# Todo リストを作る

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

React + TypeScript で、Todo リストを作ります。

## 完成図

<video src="https://drive.google.com/uc?id=1WcqTK72iIcgDrA8daXaKIoBNP_Kd8qwg" type="video/mp4" autoplay controls playsinline  width="640"></video>

## リポジトリを用意する

### 実装手順

1. [テンプレートリポジトリ](https://github.com/Kosuke2000/nextjs-ts-tailwind-starter) をクローンする

   リポジトリ名は todo-app とします。

2. VSCode で開き、ローカルホストを立ち上げる

## Todo モデルの作成

### 実装手順

1. Todo モデルの作成
   行頭 `+` で始まるコードを追加してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   import Head from "next/head";
   import type { NextPage } from "next";

   + type Todo = {
   +   id: number
   +   name: string
   +   isDone: boolean
   + }

   const Home: NextPage = () => {
   ```

1. モックデータ（ダミーデータ）の作成
   コードを追加してください。
   <span class="code-filename">pages/index.tsx</span>

   ```diff
   export type Todo = {
    id: number
    name: string
    isDone: boolean
   }

   + const mockTodo0: Todo = {
   +   id: 0,
   +   name: "髪を切りに行く",
   +   isDone: false,
   + }
   + const mockTodo1: Todo = {
   +   id: 1,
   +   name: "プレゼントを選ぶ",
   +   isDone: false,
   + }
   + const mockTodo2: Todo = {
   +   id: 2,
   +   name: "映画館デートする",
   +   isDone: false,
   + }

   + const mockTodoList = [mockTodo0, mockTodo1, mockTodo2]

   const Home: NextPage = () => {
   ```

### 解説

#### モデルとは？

モデルとは、「ユーザーの目当て」です。今回作成する Todo アプリのモデルは、Todo 単体です。なぜなら、アプリの使用中、ユーザーの関心は Todo に向かっているからです。「Todo を登録」「Todo を削除」「Todo を編集」「Todo を Done にする」など、これらのタスクを実行するとき、ユーザーの関心は Todo にあると言えます。

#### モデルの型定義

TypeScript では、モデルの型定義ができます。今回の実装の以下の箇所で、Todo モデルの型定義が行われています。

<span class="code-filename">pages/index.tsx</span>

```typescript
type Todo = {
  id: number;
  name: string;
  isDone: boolean;
};
```

ここで Todo の型は、number 型の`id`プロパティ、string 型の`name`プロパティ、boolean 型の`isDone`プロパティをもったオブジェクトと定義されています。

#### プロパティとオブジェクト

[オブジェクト]("https://jsprimer.net/basic/object/")とは、プロパティの集合です。プロパティとは名前（キー）と値（バリュー）が対になったものです。オブジェクトは、`{}`（カーリーブレイス）で作成します。

```typescript
const sampleObj = {
  // キー: 値
  key: "value",
};
```

今回の実装だと、モックデータとして mock0、mock1、mock2 の 3 つのオブジェクトを作成しました。

<span class="code-filename">pages/index.tsx</span>

```typescript
const mock0: Todo = {
  id: 0,
  name: "髪を切りに行く",
  isDone: false,
};
const mock1: Todo = {
  id: 1,
  name: "プレゼントを選ぶ",
  isDone: false,
};
const mock2: Todo = {
  id: 2,
  name: "映画館デートする",
  isDone: false,
};
```

#### 型定義をする理由

なぜ、型を定義するのでしょうか？筆者は、型定義には 2 つのメリットがあると考えています。

1. コードの可読性があがる
2. 予期せぬバグが起こるの防ぐ

今回の実装から理解できる範囲で説明します。作成したモックデータを見てください。以下の部分は、「`mock0` は Todo の型に従わなければならない」ということを意味しています。このように`mock0`の型を明示したことによって、`mock0`は Todo のモックデータなのだということがひと目で分かるようになっています。（逆に、`: Todo`の記述がなかった場合と比較してみてください。）これが 1 つ目のメリット、コードの可読性が上がると考える理由です。

<span class="code-filename">pages/index.tsx</span>

```typescript
const mock0: Todo = {
  // 省略
};
```

次に、作成したモックデータを以下のように壊してみます。するとエラー文が出てくるはずです。

<span class="code-filename">pages/index.tsx</span>

```diff
const mock0: Todo = {
  id: 0,
  name: "髪を切りに行く",
+  place: "バーバー井上",
  isDone: false,
};
```

<span class="code-filename">エラー文</span>

```
Type '{ id: number; name: string; place: string; isDone: false; }' is not assignable to type 'Todo'.
```

TypeScript は指定した型を破ると即座にエラーを出してくれます。型定義を利用することでコードの振る舞いを限定し、予期せぬバグを防ぐことができます。これが 2 つ目のメリット、予期せぬバグが起こるの防げると考える理由です。

#### 配列

[配列]("https://jsprimer.net/basic/array/")とは、値に順序をつけて格納できるオブジェクトです。 配列に格納したそれぞれの値のことを要素、それぞれの要素の位置のことをインデックス（index）と呼びます。 インデックスは先頭の要素から 0、1、2 のように 0 からはじまる連番となります。配列は、`[]`（ブラケット）を使って作成することができます。今回の実装の `mockTodoList` が配列です。

<span class="code-filename">pages/index.tsx</span>

```typescript
const mockTodoList = [mock0, mock1, mock2];
```

配列にマウスを当てると、その型を確認できます。mockTodoList の型を確認してみましょう。

```typescript
const mockTodoList: Todo[];
```

これは、「`mockTodoList`の型 は Todo 型の要素の配列型」ということです。mockTodoList の型は、コードで指定したものではなく、TypeScript の型推論によって自動的に指定されたものです。

### まとめ

- モデルと型定義を理解する
- Todo モデルの型定義
- Todo モデルの型に従ったモックデータ（ダミーデータ）を作成

## Todo リストを表示する

### 実装手順

1. ステート todoList を作る
   コードを追加してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   export const Home: VFC = () => {
   +  const [todoList, setTodoList] = useState(mockTodoList)
   ```

2. todoList の中身を表示するコードを追加してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
   + <table className="text-sm text-left text-gray-500 table-auto">
   +   <thead className="text-xs text-gray-700 uppercase bg-gray-50 ">
   +     <tr>
   +       <th className="py-4 px-6">ステータス</th>
   +       <th className="py-4 px-6">名前</th>
   +       <th></th>
   +       <th className="py-4 px-6"></th>
   +     </tr>
   +   </thead>
   +   <tbody>
   +     {todoList.map((todo) => (
   +       <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
   +        <td></td>
   +         <td className="py-4 px-6">
   +           <p
   +             className="w-40"
   +             style={{ textDecoration: todo.isDone ? "line-through" : "" }}
   +           >
   +             {todo.name}
   +           </p>
   +         </td>
   +         <td></td>
   +         <td></td>
   +       </tr>
   +     ))}
   +   </tbody>
   + </table>
   </main>
   ```

### 解説

#### ステート の型

[useState]("https://hackmd.io/@Kosuke2000/nabeatsu-app#useState") で生成されたステートにも型があります。ステートの型は、ステートにマウスをホバーすることで確認できます。`todoList` の型を確認しましょう。

```typescript
const todoList: Todo[];
```

ステートの初期値 `mockTodoList` から型推論によって、 `Todo[]` が指定されています。

#### オブジェクト内のプロパティの値を取得する

オブジェクト内のプロパティの値を取得する方法を学びましょう。例えば、`idInMock0` に `mock0`オブジェクトの `id`の値 0 を代入する場合、以下のようになります。

```typescript
const mock0 = {
  id: 0,
  name: "髪を切りに行く",
  isDone: false,
};

const idInMock0 = mock0.id;
```

#### map 関数

map 関数とは、配列の各要素に対して指定された関数を実行し、その結果から新しい配列を作成する関数です。今回の実装を確認します。

<span class="code-filename">pages/index.tsx</span>

```typescript
{
  todoList.map((todo) => (
    <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
      <td></td>
      <td className="py-4 px-6">
        <p
          className="w-40"
          style={{ textDecoration: todo.isDone ? "line-through" : "" }}
        >
          {todo.name}
        </p>
      </td>
      <td></td>
      <td></td>
    </tr>
  ));
}
```

`todoList` の各要素に対して、`map`関数の引数に渡された関数 `(todo) => <li key={todo.id}>{todo.name}</li>` を実行し、新しい配列を返しています。 `todo` とは、`todoList` の各要素のことだと考えてください。`todoList` が初期値の場合、上記コードの処理結果は以下のようになります。

```typescript
{[
  <tr key={0}>
      <td></td>
      <td className="py-4 px-6">
        <p
          className="w-40"
          style={{ textDecoration: todo.isDone ? "line-through" : "" }}
        >
          髪を切りに行く
        </p>
      </td>
      <td></td>
      <td></td>
  </tr>,
  <tr key={1}>
      <td></td>
      <td className="py-4 px-6">
        <p
          className="w-40"
          style={{ textDecoration: todo.isDone ? "line-through" : "" }}
        >
          プレゼントを選ぶ
        </p>
      </td>
      <td></td>
      <td></td>
  </tr>,
  <tr key={2}>
      <td></td>
      <td className="py-4 px-6">
        <p
          className="w-40"
          style={{ textDecoration: todo.isDone ? "line-through" : "" }}
        >
          映画館デートする
        </p>
      </td>
      <td></td>
      <td></td>
  </tr>
]}
```

#### 複数行の JSX だけを返す関数の書き方

今回の実装で map 関数に渡されていた関数は、複数行の JSX だけを返す関数です。複数行の JSX だけを返す関数は 2 つの書き方があります。今回の実装では、省略形が採用されています。 `=>` に続く括弧が異なっていることに注意してください。

```typescript
const function0 = (todo) => {
  return (
    <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
      // 省略
    </tr>
  );
};

// 省略形
const function1 = (todo) => (
  <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
    // 省略
  </tr>
);
```

### まとめ

- ステート（`todoList`）を作成
- todoList の中身を、map 関数で表示

## Todo アプリの全体像を理解する

### Todo アプリの仕様

今回の Todo アプリには以下の 6 つの機能を実装します。「Todo を表示する」は、前のセクションで実装済みです。

- Todo を表示
- 新しい Todo を作成
- Todo の削除
- 既存の Todo の編集
- Todo のステータスを Done（完了）にする
- Done になった Todo を削除する

### ステート todoList の更新

上記 6 つの機能を、`todoList` を使って実装します。ここでは、どのような実装になるかの概要を見ていきます。

#### Todo を表示

配列 `todoList` 内の要素を表示する。

#### 新しい Todo を作成

配列 `todoList` に、要素を追加する。追加される要素は、Todo モデルの型に従っている。

#### Todo の削除

配列 `todoList` から、任意の `id` を持った要素を削除する。

#### 既存の Todo の編集

配列 `todoList` 内の任意の `id` を持った要素の `name` を変更する。

#### Todo のステータスを Done（完了）にする

配列 `todoList` 内の任意の `id` を持った要素の `isDone` を `false` から　`true` に変更する。

#### Done になった Todo を削除する

配列 `todoList` から、 `isDone` が　`true` の要素を削除する。

## Todo 登録フォームを作る

このセクションでは、新しい Todo を登録するためのフォームを実装します。

### 実装手順

1. 入力フォームの追加

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
   +  <input type="text" className="border" />
     <table>
       {todoList.map((todo) => (
        // 省略
       ))}
     </table>
   </main>
   ```

2. フォームにされた内容をステート text で管理する

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Home: NextPage = () => {
     const [todoList, setTodoList] = useState(mockTodoList)
   +  const [text, setText] = useState("")

   +  const handleChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
   +    setText(e.target.value)
   +  }
   ```

   先程、作成したフォームを以下のように修正してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   -  <input type="text" className="border"/>
   +   <input
   +     type="text"
   +     value={text}
   +     onChange={handleChangeInput}
   +     className="border"
   +   />
   ```

### 解説

#### 実装した `input`

[こちらの記事]("https://upmostly.com/tutorials/react-onchange-events-with-examples#:~:text=An%20onChange%20event%20handler%20returns,target.")を参考に解説します。

#### `input` の `value`

`input` は、`value` を持っています。フォームに入力された内容は `value` に保存されます。

#### イベント

[イベント]("https://developer.mozilla.org/ja/docs/Learn/JavaScript/Building_blocks/Events")とは、アプリ内のユーザーの動作、出来事を指します。必要であれば、イベントに対して、何らかの反応を返す事ができます。その際用いるのがイベントプロパティです。イベントプロパティはいろいろあります。 例えば `onClick` は「ユーザーがボタンをクリックしたとき」に何らかの処理を実行します。

```typescript
<button onClick={＜クリックされたときに行う処理＞}>ボタン</button>
```

また、イベントプロパティには型があります。TypeScript では、以下のように表現されます。使い方は後述します。

```typescript
// イベントプロパティ: イベントプロパティの型
type Props = {
  onClick: (event: React.MouseEvent<HTMLInputElement>) => void;
  onChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
  onkeypress: (event: React.KeyboardEvent<HTMLInputElement>) => void;
};
```

#### `onChage`

`onChage` はイベントプロパティの一つです。今回の実装では、「ユーザーがフォームに入力するとき」、つまり`input` の `value` に変更が起こったときに、onChange に渡された関数が実行されます。このとき実行される関数の引数には、[イベントオブジェクト]("https://uhyohyo.net/javascript/3_5.html")が渡されます。イベントオブジェクトから、イベントの様々な情報を得ることができます。

具体例を見ます。下のコードをコンポーネントの `return` 以下に追加してください。ローカルホストで開発者ツールの Console を開き、作成したフォームに入力します。Console に入力中の内容が表示されるはずです。

<span class="code-filename">pages/index.tsx</span>

```typescript
<input
  className="border-2 border-red-500"
  onChange={(e) => console.log(e.target.value)}
/>
```

一連の流れを解説します。

1. ユーザーがフォームに入力
2. `input` の `value`が変化する
3. onChange に渡された関数が呼び出される
   今回の例だと、`(e) => console.log(e.target.value)`が呼び出される。

今回の実装だと、onChange から渡されるイベントオブジェクトは `e`と呼ばれています。 `e.target.value` で フォームに入力された内容、つまり`input` の `value`を取得することができます。

#### フォームの入力内容をステートとして管理する

Home コンポーネント内の`return`より上の箇所で、以下のコードを追加してください。

<span class="code-filename">pages/index.tsx</span>

```diff
const Home: NextPage = () => {
  const [todoList, setTodoList] = useState(mockTodoList)
  const [text, setText] = useState("")
+  const [demo, setDemo] = useState("")

+ console.log(demo)
```

先程のコードを以下のように変更します。

<span class="code-filename">pages/index.tsx</span>

```diff
<input
+  value={demo}
  className="border-2 border-red-500"
-  onChange={(e) => console.log(e.target.value)}
+  onChange={(e) => setDemo(e.target.value)}
/>
```

開発者ツールの Console を確認してみてください。 フォームの入力内容にあわせて `demo` が変更されていますね。これで、フォームの入力内容をステートとして管理することができました。一連の流れを説明します。

1. ユーザーがフォームに入力
2. `input` の `value`が変化する
3. `onChange` に渡された関数が呼び出される
4. ステート `demo` が更新される

##### 実装箇所の解説

今回の実装を観察します。先程までの解説とほとんど同じですが、`onChange` に直接関数を書いて渡すのではなく、`handleChangeInput`という関数名を渡しています。`onChange` に渡される引数の型が `e: React.ChangeEvent<HTMLInputElement>` と指定されているところも確認してください。

<span class="code-filename">pages/index.tsx</span>

```typescript
const [text, setText] = useState("");
const handleChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
  setText(e.target.value);
};

// 再掲: onChange の型
type Props = { onChange: (event: React.ChangeEvent<HTMLInputElement>) => void };
```

<span class="code-filename">pages/index.tsx</span>

```typescript
<input
  type="text"
  value={text}
  onChange={handleChangeInput}
  className="border"
/>
```

最後に、デモで使用したコードを次に進んでください。

<span class="code-filename">pages/index.tsx</span>

```diff
const Home: NextPage = () => {
  const [todoList, setTodoList] = useState(mockTodoList)
  const [text, setText] = useState("")
-  const [demo, setDemo] = useState("")

- console.log(demo)
```

<span class="code-filename">pages/index.tsx</span>

```diff
- <input
-   value={demo}
-  className="border-2 border-red-500"
-   onChange={(e) => console.log(e.target.value)}
-   onChange={(e) => setDemo(e.target.value)}
- />
```

## Todo を登録できるようにする

このセクションでは、フォームの入力内容をもとに新しい Todo を作成できるようにします。

### 実装手順

1. 登録ボタンの追加

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <main>
   + <div className="flex flex-col gap-2 py-10">
      <input
        type="text"
        value={text}
        onChange={handleChangeInput}
        className="border"
      />
   + <button
   +   className="py-2 px-4 font-bold text-white bg-blue-500 hover:bg-blue-400 rounded border-b-4 border-blue-700 hover:border-blue-500"
   +    disabled={text === ""}
   +   >
   +     Todo登録
   +   </button>
   + </div>
   ```

2. 以下のコードを追加してください

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   + let nextId = 3

   const Home: NextPage = () => {
   ```

3. 入力内容を Todo に登録する

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Home: NextPage = () => {
     const [todoList, setTodoList] = useState<Todo[]>(mockTodoList)
     const [text, setText] = useState("")

     const handleChangeInput: ChangeEventHandler<HTMLInputElement> = (e) => {
       setText(e.target.value)
     }

   + const register = () => {
   +   const newTodo: Todo = {
   +     id: nextId,
   +     name: text,
   +     isDone: false,
   +   }
   +   setTodoList([...todoList, newTodo])
   +
   +   setText("")
   +   nextId++
   + }
   ```

   先程作成したボタンを以下のように修正してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <button
   +  onClick={register}
     className="py-2 px-4 font-bold text-white bg-blue-500 hover:bg-blue-400 rounded border-b-4 border-blue-700 hover:border-blue-500"
     disabled={text === ""}
   >
     Todo登録
   </button>
   ```

### 解説

#### `const`

[`const`]("https://jsprimer.net/basic/variables/#const")とは、再代入できない変数の宣言とその変数が参照する値（初期値）を定義できます。

```typescript
const 変数名 = 初期値;
```

再代入しようとするとエラーがでます。

```typescript
const name = "Kosuke";
name = "Takashi"; // => エラー：Cannot assign to 'name' because it is a constant.
```

#### `let`

[`let`]("https://jsprimer.net/basic/variables/#let")とは、値の再代入が可能な変数を宣言できます。使い方は `const` とほぼ同じです。

```typescript
let name = "Kosuke";
name = "Takashi";
```

今回の実装では、唯一 `nextID`が`let`で宣言されています。

```typescript
let nextId = 3;
```

#### インクリメント演算子`++`

[インクリメント演算子`++`]("https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Increment")は、オペランド（被演算子）をインクリメント (1 を加算) して値を返します。

```typescript
let x = 3;
y = x++;

// y = 3
// x = 4
```

今回の実装を確認してみましょう。変数`nextID`がインクリメントされています。

<span class="code-filename">pages/index.tsx</span>

```typescript
const register = () => {
  // 省略

  setText("");
  nextId++;
};
```

`nextId++` は、以下のコードの省略形です。 `nextID` に `nextID` をインクリメントした値が再代入されています。

```ts
nextId = nextId++;
```

#### 配列 TodoList に追加する要素を作成する

`register` 内の処理を確認します。まず、`todoList` に追加する`newTodo` を作成します。ポイントは 2 つです。

- `newTodo`は Todo モデルの型に従っている
- `name` の値は `text`

<span class="code-filename">pages/index.tsx</span>

```typescript
const register = () => {
  const newTodo: Todo = {
    id: nextId,
    name: text,
    isDone: false,
  };
  // 省略
};
```

#### spread syntax `...`

[spread syntax `...`]("https://typescriptbook.jp/reference/values-types-variables/array/spread-syntax-for-array")を使うことで、要素を展開することができます。

```typescript
const arr = [1, 2, 3];
const arr2 = [...arr, 4];

console.log(arr2); // => expected output is [1, 2, 3, 4]
```

今回の実装では、 `register` の中で使用されています。

<span class="code-filename">pages/index.tsx</span>

```typescript
const register = () => {
  const newTodo: Todo = {
    id: nextId,
    name: text,
    isDone: false,
  };
  setTodoList([...todoList, newTodo]);

  setText("");
  nextId++;
};
```

`todoList` が初期値の場合、spread syntax では以下のように展開されます。

```typescript
setTodoList([mock0, mock1, mock2, newTodo]);
```

### まとめ

`register` の処理を確認します。

1. フォームに新たに追加したい Todo が入力される
2. 「Todo に登録」ボタンをクリックされる
3. フォームに入力内容を `name`に持った `newTodo`を作成
4. `todoList`内の要素に `newTodo`を追加した配列を、新たに `todoList` にする
5. フォームがリセット
6. `nextId` がインクリメント

#### 補足

`id`とは、各 Todo の識別子の役割を果たしています。Todo が作られるたびに、`nextId` をインクリメントさせることで、各 Todo が固有の `id` を持つようにしています。今回の実装では、すでにモックデータで 0、1、2 の `id` が使われていたので、 `let nextId = 3` としました。最初に作られる Todo の id は 3、それ以降、Todo の `id` は 1 ずつ大きくなります。

## Todo 削除ボタンを作る

このセクションでは、Todo を削除する機能を実装します。

### 実装手順

1. 削除ボタンの作成

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   {todoList.map((todo) => (
      <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
        <td></td>
        <td className="py-4 px-6">
          <p
            className="w-40"
            style={{ textDecoration: todo.isDone ? "line-through" : "" }}
          >
            {todo.name}
          </p>
        </td>
        <td></td>
        <td className="py-4 px-6">
   +     <button className="py-2 px-4 font-bold text-white bg-red-500 rounded">
   +       削除
   +     </button>
   +   </td>
      </tr>
    ))}
   ```

2. remove 関数の用意

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const register = () => {
     // 省略
     }

   + const remove = (id: number) => {
   +     const newState = todoList.filter((todo) => todo.id !== id)
   +     setTodoList(newState)
   +   }
   ```

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   - <button className="py-2 px-4 font-bold text-white bg-red-500 rounded">
   + <button
   +    onClick={() => remove(todo.id)}
   +    className="py-2 px-4 font-bold text-white bg-red-500 rounded"
   + >
      削除
    </button>
   ```

### 解説

#### map 関数のおさらい

現在のコードで、map 関数のおさらいをしましょう。

<span class="code-filename">pages/index.tsx</span>

```typescript
{
  todoList.map((todo) => (
    <button
      onClick={() => remove(todo.id)}
      className="py-2 px-4 font-bold text-white bg-red-500 rounded"
    >
      {todo.name}
    </button>
  ));
}
```

`todoList` が初期状態の場合、上記コードを map を使わないで書くと、以下のようになります。各 Todo が削除ボタンを持ちます。 `onClick` 内の `remove` は Todo の `id` を引数として受け取っています。

```typescript
<button
  onClick={() => remove(0)}
  className="py-2 px-4 font-bold text-white bg-red-500 rounded"
>
  髪を切りに行く
</button>
<button
  onClick={() => remove(1)}
  className="py-2 px-4 font-bold text-white bg-red-500 rounded"
>
  プレゼントを選ぶ
</button>
<button
  onClick={() => remove(2)}
  className="py-2 px-4 font-bold text-white bg-red-500 rounded"
>
  映画館デートする
</button>
```

#### filter 関数

[filter 関数]("https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter")とは、与えられた関数によって実装されたテストに合格したすべての配列からなる新しい配列を生成します。

```typescript
const words = [
  "spray",
  "limit",
  "elite",
  "exuberant",
  "destruction",
  "present",
];

const result = words.filter((word) => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```

今回実装した `remove` を確認します。`remove` は、Todo の `id` を引数として受け取ります。 `newState` は、 `todoList` から、 `remove` の引数と一致する `id` を持った Todo が取り除かれた配列です。

<span class="code-filename">pages/index.tsx</span>

```typescript
const remove = (id: number) => {
  const newState = todoList.filter((todo) => todo.id !== id);
  setTodoList(newState);
};
```

### まとめ

1. 削除ボタンが押され、その `onClick`に渡されている `remove` が実行される
2. `todoList` から、削除したい Todo が削除される

## Todo 完了ボタンを作る

このセクションでは、Todo を完了にする機能を実装します。

### 実装手順

1. 完了チェックボックスの用意

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   {todoList.map((todo) => (
      <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
   -    <td></td>
   +    <td className="py-4 px-6">
   +      <input
   +        type="checkbox"
   +        checked={todo.isDone}
   +      />
   +    </td>
        <td className="py-4 px-6">
          <p
            className="w-40"
            style={{ textDecoration: todo.isDone ? "line-through" : "" }}
          >
            {todo.name}
          </p>
        </td>
   ```

2. toggle 関数の作成

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const remove = (id: number) => {
     // 省略
   };

   + const toggle = (id: number) => {
   +   const newState = todoList.map((todo) => {
   +     if (todo.id !== id) return todo;
   +     return { ...todo, isDone: !todo.isDone };
   +   });
   +
   +   setTodoList(newState);
   + };
   ```

   <span class="code-filename">pages/index.tsx</span>

   ```diff
     <input
       type="checkbox"
       checked={todo.isDone}
   +    onChange={() => toggle(todo.id)}
     />
   ```

### 解説

#### input type="checkbox"

[`<input type=“checkbox”>`]("https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox")でチェックボックスが作れます。checked 属性を使うことで、チェックボックスの挙動をコントロールできます。
今回の実装を確認します。 `isDone` は Boolean 型でした。`isDone` が `true` ならチェックが入り、`false` なら空欄になります。

<span class="code-filename">pages/index.tsx</span>

```typescript
<input type="checkbox" checked={todo.isDone} onChange={() => toggle(todo.id)} />
```

mock0 の `isDone` を `false` から `true` に変えて、ローカルホストを確認してみましょう。

<span class="code-filename">pages/index.tsx</span>

```diff
const mock0: Todo = {
  id: 0,
  name: "髪を切りに行く",
-  isDone: false,
+  isDone: true,
};
```

#### 論理否定 (!)

[論理否定 (!)]("https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_NOT") 演算子 (論理反転、否定) は、真値を取ると偽値になり、偽値になると真値になります。
今回の実装を確認します。todo.isDone が true のとき isDone は false になり、todo.isDone が false のとき isDone は true になります。

```typescript
isDone: !todo.isDone;
```

#### 厳密不等価 (!==)

[厳密不等価演算子 (!==)]("https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Strict_inequality") は、2 つのオペランド（被演算子）が等しくないことを検査し、`true` か `false` で結果を返します。

```typescript
console.log(1 !== 1);
// expected output: false
console.log("1" !== 1);
// expected output: true
```

#### if 文

[if 文]("https://jsprimer.net/basic/condition/#if-statement")を使うことで、プログラム内に条件分岐を書けます。if 文は次のような構文が基本形となります。 条件式の評価結果が `true` であるならば、実行する文が実行されます。

```typescript
if (条件式) {
  実行する文;
}
```

今回の実装を確認します。`togggle` の引数と `todo.id` が異なる場合は、`todo` がそのまま返されます。逆に、`togggle` の引数と `todo.id` が同じだった場合は、if 文に続く処理 `return { ...todo, isDone: !todo.isDone }` に移動します。

<span class="code-filename">pages/index.tsx</span>

```typescript
const toggle = (id: number) => {
  const newState = todoList.map((todo) => {
    if (todo.id !== id) {
      return todo;
    }
    return { ...todo, isDone: !todo.isDone };
  });

  setTodoList(newState);
};
```

上記で書かれたコードは、以下のようにリファクタリングできます。

<span class="code-filename">pages/index.tsx</span>

```typescript
const toggle = (id: number) => {
  const newState = todoList.map((todo) => {
    if (todo.id !== id) return todo;

    return { ...todo, isDone: !todo.isDone };
  });

  setTodoList(newState);
};
```

### まとめ

1. 完了した Todo にチェックが入れられる
2. その Todo の `isDone` が `true` になる

## 完了した Todo を一掃するボタンを作る

このセクションでは、完了した Todo を一掃する機能を実装します。

### 実装手順

1. クリアボタンの作成

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <thead className="text-xs text-gray-700 uppercase bg-gray-50 ">
    <tr>
      <th className="py-4 px-6">ステータス</th>
      <th className="py-4 px-6">名前</th>
      <th></th>
   +   <th className="py-4 px-6">
   +     <button
   +       className="py-2 px-4 font-bold text-white bg-gray-700 hover:bg-gray-900 rounded"
   +     >
   +       一掃
   +     </button>
      </th>
    </tr>
   </thead>
   ```

2. clear 関数の作成

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const toggle = () => {
     // 省略
   };

   + const clear = () => {
   +   const newState = todoList.filter((todo) => !todo.isDone);
   +   setTodoList(newState);
   + };
   ```

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <button
   +   onClick={clear}
      className="py-2 px-4 font-bold text-white bg-gray-700 hover:bg-gray-900 rounded"
    >
      一掃
    </button>
   ```

## Todo コンポーネントの作成

このセクションでは、Todo コンポーネントを作成します。

### 実装手順

1. Todo コンポーネントを作成する
   まず、Home コンポーネントの上に、Todo コンポーネントを作ります。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   + const Todo = () => {
   +   return (
   +   );
   + };

   const Home: NextPage = () => {
   ```

   次に、Home コンポーネントの以下の箇所(行頭が `-` のコード)を切り取ります。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <tbody>
     {todoList.map((todo) => (
   -   <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
   -           <td className="py-4 px-6">
   -             <input
   -               type="checkbox"
   -               checked={todo.isDone}
   -               onChange={() => toggle(todo.id)}
   -             />
   -           </td>
   -           <td className="py-4 px-6">
   -             <p
   -               className="w-40"
   -               style={{ textDecoration: todo.isDone ? "line-through" : "" }}
   -             >
   -               {todo.name}
   -             </p>
   -           </td>
   -           <td></td>
   -           <td className="py-4 px-6">
   -             <button
   -               onClick={() => remove(todo.id)}
   -               className="py-2 px-4 font-bold text-white bg-red-500 rounded"
   -             >
   -               削除
   -             </button>
   -           </td>
   -         </tr>
     ))}
   </tbody>
   ```

   切り取った部分を、Todo コンポーネントに貼り付けます。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Todo = () => {
     return (
   +   <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
   +     <td className="py-4 px-6">
   +       <input
   +         type="checkbox"
   +         checked={todo.isDone}
   +         onChange={() => toggle(todo.id)}
   +       />
   +     </td>
   +     <td className="py-4 px-6">
   +       <p
   +         className="w-40"
   +         style={{ textDecoration: todo.isDone ? "line-through" : "" }}
   +       >
   +         {todo.name}
   +       </p>
   +     </td>
   +     <td></td>
   +     <td className="py-4 px-6">
   +       <button
   +         onClick={() => remove(todo.id)}
   +         className="py-2 px-4 font-bold text-white bg-red-500 rounded"
   +       >
   +         削除
   +       </button>
   +     </td>
   +   </tr>
     );
   };
   ```

   `todo` `remove` `toggle` の部分にでエラーが出ていたら正解です。

   エラーが出ているのは、Todo コンポーネントの中で`todo` `remove` `toggle`を呼び出しても、Todo コンポーネント内にこれらに対応するものがないからです。今から、親コンポーネント（Home）から子コンポーネント（Todo）へ、`todo` `remove` `toggle` が渡せるようにします。（詳しくは解説で）

2. Todo コンポーネントのインターフェイスを作る
   TodoProps という名前のインターフェイスを作ります。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   + interface Props {
   + todo: Todo
   + onToggle: (id: number) => void
   + onRemove: (id: number) => void
   + }

   const Todo = () => {
   ```

   次に、以下のように修正してください。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   - const Todo = () => {
   + const Todo = ({ todo, onToggle, onRemove }: TodoProps) => {
   ```

   `return` の中も、以下のように修正してください。先程までのエラーが消えるはずです。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   return (
     <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
       <td className="py-4 px-6">
   -      <input type="checkbox" checked={todo.isDone} onChange={() => toggle(todo.id)} />
   +      <input type="checkbox" checked={todo.isDone} onChange={() => onToggle(todo.id)} />
       </td>
       <td className="py-4 px-6">
         <p
           className="w-40"
           style={{ textDecoration: todo.isDone ? "line-through" : "" }}
         >
           {todo.name}
         </p>
       </td>
       <td className="py-4 px-6"></td>
       <td className="py-4 px-6">
         <button
   -        onClick={() => remove(todo.id)}
   +        onClick={() => onRemove(todo.id)}
           className="py-2 px-4 font-bold text-white bg-red-500 hover:bg-red-700 rounded"
         >
           削除
         </button>
       </td>
     </tr>
   );
   ```

3. Home コンポーネントの中で Todo コンポーネントを呼び出す
   ローカルホストで挙動を確認して下しさい。
   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <tbody>
     {todoList.map((todo) => (
   +    <Todo
   +      key={todo.id}
   +      todo={todo}
   +      onRemove={remove}
   +      onToggle={toggle}
   +    />
     ))}
   </tbody>
   ```

### 解説

#### コンポーネント

[コンポーネント]("https://beta.reactjs.org/learn/your-first-component#defining-a-component")は、特別な JavaScript の関数です。特別な点は 2 つです。

1. 関数名が大文字から始まる
2. JSX を返す

```typescript
const 大文字で始める関数名 = (引数) => {

  return (
    <p>JSXを返す</p>;
  )
};
```

#### 関数のスコープ

[スコープ]("https://jsprimer.net/basic/function-scope/#what-is-scope")とは、変数の名前や関数などの参照できる範囲を決めるものです。 スコープの中で定義された変数はスコープの内側でのみ参照でき、スコープの外側からは参照できません。例えば、関数の中で定義された変数は、その関数の中でしか参照することしかできないことになっています。

今回の実装を振り返ります。実装手順 1 の終わりでエラーが出ていたのは、Home コンポーネントの変数がスコープ外である Todo コンポーネントから呼び出されていたからです。コンポーネントは関数でしたね。

<img src="https://drive.google.com/uc?id=1ktZMP8zHjOKv5UUiNyFkpU7aY84peOVE" width="800" allow="autoplay"/>

#### 親コンポーネントから子コンポーネントに Props を渡す

[コンポーネントは親から子へ Props として変数や関数、JSX などあらゆる情報をわたすことができます。]("https://beta.reactjs.org/learn/passing-props-to-a-component")今回の実装を確認します。Home コンポーネントから Todo コンポーネントを呼び出しました。このとき、Home コンポーネントが親、Todo コンポーネントが子になります。

<span class="code-filename">pages/index.tsx</span>

```typescript
<tbody>
  {todoList.map((todo) => (
    <Todo key={todo.id} todo={todo} onRemove={remove} onToggle={toggle} />
  ))}
</tbody>
```

Props は、`todo` や `onRemove`、`onToggle`になります。例えば、 Home コンポーネントから Todo コンポーネントの `onRemove` に渡された `remove` は、Todo コンポーネントの中で`onRemove`として呼び出すことが可能になります。

Todo コンポーネントを確認します。削除ボタンの `onClick` 内の処理で`onRemove` が呼び出されています。

<span class="code-filename">pages/index.tsx</span>

```typescript
const Todo = ({ todo, onToggle, onRemove }: Props) => {
  s;
  return (
    <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
      <td className="py-4 px-6">
        <input
          type="checkbox"
          checked={todo.isDone}
          onChange={() => onToggle(todo.id)}
        />
      </td>
      <td className="py-4 px-6">
        <p
          className="w-40"
          style={{ textDecoration: todo.isDone ? "line-through" : "" }}
        >
          {todo.name}
        </p>
      </td>
      <td className="py-4 px-6">
        <button
          onClick={() => onRemove(todo.id)}
          className="py-2 px-4 font-bold text-white bg-red-500 hover:bg-red-700 rounded"
        >
          削除
        </button>
      </td>
    </tr>
  );
};
```

#### インターフェイス

インターフェイスとは、Props の型の集合です。インターフェイスを定義することで、子コンポーネントが親から受け取る Props の型を定義できます。実装を確認します。`TodoProps` は、「`todo` は Todo」「`onToggle` は number 型の引数を受け取り void を返す関数」「`onRemove` は number 型の引数を受け取り void を返す関数」という Props の型定義の集合です。

<span class="code-filename">pages/index.tsx</span>

```typescript
interface TodoProps {
  todo: Todo;
  onToggle: (id: number) => void
  onRemove: (id: number) => void
}

const Todo = ({ todo, onToggle, onRemove }: TodoProps) => {
  return (
    // 省略
  );
};
```

## Todo を編集モードに切り替えられるようにする

このセクションと次のセクションで、既存の Todo を編集する機能を実装します。このセクションでは、既存の Todo を編集モードに切り替えられるようにします。

<video src="https://drive.google.com/uc?id=1hHb3e7kTpuDWcnsx390w-ls2ghkYAFNd" type="video/mp4" autoplay controls playsinline  width="600"></video>

### 実装手順

1. 編集モードを作成する

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Todo = ({ todo, onToggle, onRemove, onEdit }: TodoProps) => {
   +  const [isEditing, setIsEditing] = useState(false)

   +  const startEditing = () => setIsEditing(true)
   +  const finishEditing = () => setIsEditing(false)
   ```

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   return (
   + <>
   +   {isEditing ? (
   +     <tr className="bg-white dark:bg-gray-800 border-b">
   +       <td></td>
   +       <td className="py-4 px-6">
   +         <input
   +           type="text"
   +           className="border"
   +           value={todo.name}
   +         />
   +       </td>
   +       <td className="py-4 px-6">
   +         <button
   +           onClick={finishEditing}
   +           className="py-2 px-4 font-bold text-white bg-green-500 hover:bg-green-700 rounded"
   +         >
   +           編集終了
   +         </button>
   +       </td>
   +       <td className="py-4 px-6">
   +         <button
   +           onClick={() => onRemove(todo.id)}
   +           className="py-2 px-4 font-bold text-white bg-red-500 rounded opacity-50 cursor-not-allowed"
   +           disabled
   +         >
   +           削除
   +         </button>
   +       </td>
   +     </tr>
   +   ) : (
        <tr key={todo.id} className="bg-white dark:bg-gray-800 border-b">
          <td className="py-4 px-6">
            <input type="checkbox" checked={todo.isDone} onChange={() => onToggle(todo.id)} />
          </td>
          <td className="py-4 px-6">
            <p
              className="w-40"
              style={{ textDecoration: todo.isDone ? "line-through" : "" }}
            >
              {todo.name}
            </p>
          </td>
          <!-- 省略 -->
        </tr>
   +   )}
   + </>
   )
   ```

### 解説

#### UI の条件分岐

[三項演算子]("https://hackmd.io/@Kosuke2000/nabeatsu-app#%E4%B8%89%E9%A0%85%E6%BC%94%E7%AE%97%E5%AD%90")を使って、UI を分岐させることができます。実装を確認します。

<span class="code-filename">pages/index.tsx</span>

```typescript
return (
  <>
    {isEditing ? (
      <li>Todo編集モード＝isEditingがtrueのときのUI</li>
    ) : (
      <li>Todo表示モード＝isEditingがfalseのときのUI</li>
    )}
  </>
);
```

### まとめ

1. 編集ボタンを押すと、編集モードに見た目が切り替わる
2. 編集終了ボタンを押すと、表示モードに切り替わる

## Todo を編集できるようにする

このセクションでは、編集モードで入力した内容がアプリに反映されるようにします。

### 実装手順

1. 反映する関数を作り、Prop として Todo コンポーネントにわたす
   まず、 Home コンポーネント内に `edit` を作ります。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const clear = () => {
     // 省略
   };

   + const edit = (id: number) => (name: string) => {
   +   const newState = todoList.map((todo) => {
   +     if (todo.id !== id) return todo;
   +     return { ...todo, name: name };
   +   });
   +
   +   setTodoList(newState);
   + };
   ```

   `edit` を Todo コンポーネントに渡せるように、インターフェイスを変更します。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   interface TodoProps {
     todo: Todo
     onToggle: (id: number) => void
     onRemove: (id: number) => void
   +  onEdit: (id: number, name: string) => void
   }

   - const Todo = ({ todo, onToggle, onRemove }: TodoProps) => {
   + const Todo = ({ todo, onToggle, onRemove, onEdit }: TodoProps) => {
   ```

   `edit` を Todo コンポーネントの `onEdit` に渡します。

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   <Todo
     key={todo.id}
     todo={todo}
     onRemove={remove}
     onToggle={toggle}
   +  onEdit={edit}
   />
   ```

2. 編集フォームへの入力内容が反映されるようにする

   <span class="code-filename">pages/index.tsx</span>

   ```diff
   const Todo = ({ todo, onToggle, onRemove, onEdit }: TodoProps) => {
     const [isEditing, setIsEditing] = useState(false)

   +  const handleChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
   +    onEdit(e.target.value)
   +  }
   ```

   <span class="code-filename">pages/index.tsx</span>

   ```
    {isEditing ? (
    <tr className="bg-white dark:bg-gray-800 border-b">
      <td className="py-4 px-6"></td>
      <td className="py-4 px-6">
        <input
          type="text"
    +      onChange={handleChangeInput}
          className="border"
          value={todo.name}
        />
      </td>
   ```

## 参考文献

- [オブジェクト · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/object/")
- [配列 · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/array/")
- [input type="text" - HTML: HyperText Markup Language | MDN ]("https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/text")
- [React onChange Events (With Examples) - Upmostly]("https://upmostly.com/tutorials/react-onchange-events-with-examples#:~:text=An%20onChange%20event%20handler%20returns,target.")
- [イベントへの入門 - ウェブ開発を学ぶ | MDN]("https://developer.mozilla.org/ja/docs/Learn/JavaScript/Building_blocks/Events")
- [三章第五回　イベントオブジェクト — JavaScript 初級者から中級者になろう — uhyohyo.net]("https://uhyohyo.net/javascript/3_5.html")
- [any 型で諦めない React.EventCallback - Qiita]("https://qiita.com/Takepepe/items/f1ba99a7ca7e66290f24")
- [変数と宣言 · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/variables/")
- [Array.prototype.filter() - JavaScript | MDN]("https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter")
- [input type="checkbox" - HTML: HyperText Markup Language | MDN]("https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox")
- [論理否定 (!) - JavaScript | MDN]("https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_NOT")
- [厳密不等価 (!==) - JavaScript | MDN]("https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Strict_inequality")
- [条件分岐 · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/condition")
- [Your First Component]("https://beta.reactjs.org/learn/your-first-component")
- [関数とスコープ · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/function-scope")
- [Passing Props to a Component]("https://beta.reactjs.org/learn/passing-props-to-a-component")
- [サルでもわかるカリー化とそのメリット - These Walls]("https://kazchimo.com/2021/03/29/monkey_curry/")
