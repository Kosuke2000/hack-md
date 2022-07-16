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

作成したモックデータを見てください。以下の部分は、「`mock0` は Todo の型に従わなければならない」ということを意味しています。このように`mock0`の型を明示したことによって、`mock0`は Todo のモックデータなのだということがひと目で分かるようになっています。（逆に、`: Todo`の記述がなかった場合と比較してみてください。）これが 1 つ目のメリット、コードの可読性が上がると考える理由です。

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

[配列]("https://jsprimer.net/basic/array/")とは、値に順序をつけて格納できるオブジェクトです。 配列に格納したそれぞれの値のことを要素、それぞれの要素の位置のことをインデックス（index）と呼びます。 インデックスは先頭の要素から 0、1、2 のように 0 からはじまる連番となります。配列は、`[]`（リテラルノーテーション）を使って作成することができます。今回の実装の mockTodoList が配列です。

```typescript
const mockTodoList = [mock0, mock1, mock2];
```

### まとめ

- Todo アプリのモデルは Todo 単体
- Todo モデルの型定義
- Todo モデルの型に従ったモックデータ（ダミーデータ）を作成

## Todo リストを表示する

### 実装手順

1. ステート todoList を作る
   コードを追加してください。

```diff
export const Home: VFC = () => {
+  const [todoList, setTodoList] = useState<Todo[]>(mockTodoList)
```

2.  todoList の中身を表示する
    コードを追加してください。

```diff
<main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
+   <ul>
+     {todoList.map((todo) => (
+       <li key={todo.id}>{todo.name}</li>
+     ))}
+   </ul>
</main>
```

### 解説

#### useState の型定義

#### map 関数

### まとめ

## Todo アプリの全体像を理解する

### 解説

#### アプリの仕様

#### ステート todoList の更新

## Todo 登録フォームを作る

### 実装手順

1. 入力フォームの追加

```diff
<main className="flex flex-col justify-center items-center py-4 px-8 m-0 min-h-screen">
  <ul>
    {todoList.map((todo) => (
      <li key={todo.id}>{todo.name}</li>
    ))}
  </ul>
+  <input type="text" className="border"/>
</main>
```

2. フォームにされた内容をステート text で管理する

   ```diff
   const Home: NextPage = () => {
     const [todoList, setTodoList] = useState<Todo[]>(mockTodoList)
   +  const [text, setText] = useState("")

   +  const handleChange: ChangeEventHandler<HTMLInputElement> = (e) => {
   +    setText(e.target.value)
   +  }
   ```

   先程、作成したフォームを以下のように修正してください。

   ```diff
   -  <input type="text" />
   +   <input
   +     type="text"
   +     value={text}
   +     onChange={handleChange}
   +     className="border"
   +   />
   ```

### 解説

#### HTML の`input`

#### JavaScript の GlobalEventHandlers.onchange

#### 実装した `input`

#### 関数の型定義

### まとめ

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

- [参考文献]("hoge")
- [オブジェクト · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/object/")
- [配列 · JavaScript Primer #jsprimer]("https://jsprimer.net/basic/array/")

```

```
