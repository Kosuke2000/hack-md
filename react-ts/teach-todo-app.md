---
title: Todoリストを作る
description: React TypeScriptを習得するためのノート
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

<video src="https://drive.google.com/uc?id=1cn9As-pb9ZlAZzVYqb7X0RUArR9zff8x" type="video/mp4" autoplay controls loop playsinline  width="480"></video>

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

```typescript
type Todo = {
  id: number;
  name: string;
  isDone: boolean;
};
```

ここで Todo の型は、number 型の`id`プロパティ、string 型の`name`プロパティ、boolean 型の`isDone`プロパティをもったオブジェクトと定義されています。

#### プロパティとオブジェクト

[オブジェクト]("https://jsprimer.net/basic/object/")とは、プロパティの集合です。プロパティとは名前（キー）と値（バリュー）が対になったものです。オブジェクトは、`{}`（オブジェクトリテラル）で作成します。

```typescript
const sampleObj = {
  // キー: 値
  key: "value",
};
```

今回の実装だと、モックデータとして mock0、mock1、mock2 の 3 つのオブジェクトを作成しました。

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

```typescript
const mock0: Todo = {
  // 省略
};
```

次に、作成したモックデータを以下のように壊してみます。するとエラー文が出てくるはずです。

```diff
const mock0: Todo = {
  id: 0,
  name: "髪を切りに行く",
+  place: "バーバー井上",
  isDone: false,
};
```

```
Type '{ id: number; name: string; place: string; isDone: false; }' is not assignable to type 'Todo'.
```

TypeScript は指定した型を破ると即座にエラーを出してくれます。型定義を利用することでコードの振る舞いを限定し、予期せぬバグを防ぐことができます。これが 2 つ目のメリット、予期せぬバグが起こるの防げると考える理由です。

#### 配列

[配列]("https://jsprimer.net/basic/array/")とは、値に順序をつけて格納できるオブジェクトです。 配列に格納したそれぞれの値のことを要素、それぞれの要素の位置のことをインデックス（index）と呼びます。 インデックスは先頭の要素から 0、1、2 のように 0 からはじまる連番となります。配列は、`[]`（リテラルノーテーション）を使って作成することができます。今回の実装の `mockTodoList` が配列です。

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

```diff
export const Home: VFC = () => {
+  const [todoList, setTodoList] = useState(mockTodoList)
```

2.  todoList の中身を表示する
    コードを追加してください。

```diff
<main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
+   <ul>
+     {todoList.map((todo) => (
+        <li key={todo.id} className="flex gap-2">
+          {todo.name}
+        </li>
+      ))}
+   </ul>
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

```ts
const mock0 = {
  id: 0,
  name: "髪を切りに行く",
  isDone: false,
};

const idInMock0 = mock0.id;
```

#### map 関数

map 関数とは、配列の各要素に対して指定された関数を実行し、その結果から新しい配列を作成する関数です。今回の実装を確認します。

```ts
{
  todoList.map((todo) => <li key={todo.id}>{todo.name}</li>);
}
```

`todoList` の各要素に対して、`map`関数の引数に渡された関数 `(todo) => <li key={todo.id}>{todo.name}</li>` を実行し、新しい配列を返しています。 `todo` とは、`todoList` の各要素のことだと考えてください。`todoList` が初期値の場合、上記コードの処理結果は以下のようになります。

```ts
{[
  <li key={mock0.id}>{mock0.name}</li>
  <li key={mock1.id}>{mock1.name}</li>
  <li key={mock2.id}>{mock2.name}</li>
]}
```

#### 複数行の JSX だけを返す関数の書き方

今回の実装で map 関数に渡されていた関数は、複数行の JSX だけを返す関数です。複数行の JSX だけを返す関数は 2 つの書き方があります。今回の実装では、省略形が採用されています。 `=>` に続く括弧が異なっていることに注意してください。

```ts
const function0 = (todo) => {
  return (
    <li key={todo.id} className="flex gap-2">
      {todo.name}
    </li>
  );
};

// 省略形
const function1 = (todo) => (
  <li key={todo.id} className="flex gap-2">
    {todo.name}
  </li>
);
```

### まとめ

- ステート（`todoList`）を作成
- todoList の中身を、map 関数で表示

## Todo アプリの全体像を理解する

### 解説

#### Todo アプリの仕様

今回の Todo アプリには以下の 6 つの機能を実装します。「Todo を表示する」は、前のセクションで実装済みです。

- Todo を表示
- 新しい Todo を作成
- Todo の削除
- 既存の Todo の編集
- Todo のステータスを Done（完了）にする
- Done になった Todo を削除する

#### ステート todoList の更新

上記 6 つの機能を、`todoList` を使って実装します。ここでは、どのような実装になるかの概要を見ていきます。

##### Todo を表示

配列 `todoList` 内の要素を表示する。

##### 新しい Todo を作成

配列 `todoList` に、要素を追加する。追加される要素は、Todo モデルの型に従っている。

##### Todo の削除

配列 `todoList` から、任意の `id` を持った要素を削除する。

##### 既存の Todo の編集

配列 `todoList` 内の任意の `id` を持った要素の `name` を変更する。

##### Todo のステータスを Done（完了）にする

配列 `todoList` 内の任意の `id` を持った要素の `isDone` を `false` から　`true` に変更する。

##### Done になった Todo を削除する

配列 `todoList` から、 `isDone` が　`true` の要素を削除する。

## Todo 登録フォームを作る

このセクションでは、新しい Todo を登録するためのフォームを実装します。

### 実装手順

1. 入力フォームの追加

```diff
<main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
  <ul>
    {todoList.map((todo) => (
      <li key={todo.id} className="flex gap-2">
        {todo.name}
      </li>
    ))}
  </ul>
+  <input type="text" className="border"/>
</main>
```

2. フォームにされた内容をステート text で管理する

   ```diff
   const Home: NextPage = () => {
     const [todoList, setTodoList] = useState(mockTodoList)
   +  const [text, setText] = useState("")

   +  const handleChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
   +    setText(e.target.value)
   +  }
   ```

   先程、作成したフォームを以下のように修正してください。

   ```diff
   -  <input type="text" />
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

###### `input` の `value`

`input` は、`value` を持っています。フォームに入力された内容は `value` に保存されます。

##### `onChage`

`onChage` に渡された関数は、`input` の `value` が変更したときに呼び出されます。

##### 例： input に onChange を追加する

下のコードをコンポーネントの `return` 以下に追加してください。ローカルホストで開発者ツールの Console を開き、作成したフォームに入力します。Console に入力中の内容が表示されるはずです。

```ts
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

onChage は、渡された関数の引数に Synthetic Event を渡します。 Synthetic Event とは、input 関連のメタ情報のことです。今回の実装だと、onChange から渡される Synthetic Event は `e`と呼ばれています。 `e.target.value` で `input` に入力された内容、つまり`input` の `value`を取得することができます。

##### フォームの入力内容をステートとして管理する

Home コンポーネント内の`return`より上の箇所で、以下のコードを追加してください。

```diff
const Home: NextPage = () => {
  const [todoList, setTodoList] = useState(mockTodoList)
  const [text, setText] = useState("")
+  const [demo, setDemo] = useState("")

+ console.log(demo)
```

先程のコードを以下のように変更します。

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

今回の実装を観察します。先程までの解説とほとんど同じですが、`onChange` に直接関数を書いて渡すのではなく、`handleChangeInput`という関数名を渡しています。この場合、関数、または、その引数に適当な型を指定する必要があります。 `e: React.ChangeEvent<HTMLInputElement>` とは、「引数は`e`。`e`の型は、`input`内の`onChange`に渡す関数の型。」という意味を表します。 `: React.ChangeEvent<HTMLInputElement>`を削除してみてください。onChange の箇所で型エラーが出るはずです。

```ts
const [text, setText] = useState("");
const handleChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
  setText(e.target.value);
};
```

```ts
<input
  type="text"
  value={text}
  onChange={handleChangeInput}
  className="border"
/>
```

## Todo を登録できるようにする

### 実装手順

1. 登録ボタンの追加

```diff
  <input type="text" className="border"/>
+  <button>Todoに登録</button>

</main>
```

2. ファイル末尾に以下のコードを追加してください

   ```diff
   + let nextId = 3

   export default Home
   ```

3. 入力内容を Todo に登録する

   ```diff
   const Home: NextPage = () => {
     const [todoList, setTodoList] = useState<Todo[]>(mockTodoList)
     const [text, setText] = useState("")

     const handleChange: ChangeEventHandler<HTMLInputElement> = (e) => {
       setText(e.target.value)
     }

   + const register = (text: string) => {
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

   ```diff
   - <button>Todoに登録</button>
   + <button onClick={() => register(text)}>Todoに登録</button>
   ```

### 解説

#### `const` と `let`

#### インクリメント演算子`++`

#### 配列に要素を追加する

#### `register` 内の処理

#### spread syntax `...`

### まとめ

## Todo 削除ボタンを作る

### 実装手順

1. 削除ボタンの作成

```diff
<ul>
{todoList.map((todo) => (
  <li key={todo.id} className="flex gap-2">
    <p>{todo.name}</p>
+    <button onClick={() => remove(todo.id)}>削除</button>
  </li>
))}
</ul>
```

2. remove 関数の用意

```diff
const register = () => {
  // 省略
  }

+ const remove = (id: number) => {
+     const newState = todoList.filter((todo) => todo.id !== id)
+     setTodoList(newState)
+   }
```

### 解説

#### 配列から要素を削除する

#### filter 関数

### まとめ

## Section

### 実装手順

1. 行頭が `+` のコードを `Home` コンポーネントに追加してください
2.

### 解説

#### 解説項目

[解説項目](https://hoge)とは、...。

### まとめ

## Section

### 実装手順

1. 行頭が `+` のコードを `Home` コンポーネントに追加してください
2.

### 解説

#### 解説項目

[解説項目](https://hoge)とは、...。

### まとめ

## Section

### 実装手順

1. 行頭が `+` のコードを `Home` コンポーネントに追加してください
2.

### 解説

#### 解説項目

[解説項目](https://hoge)とは、...。

### まとめ

## 参考文献

- [オブジェクト · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/object/")
- [配列 · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/array/")
- [<input type="text"> - HTML: HyperText Markup Language | MDN ]("https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/text")

```

```
