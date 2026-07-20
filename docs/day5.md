# Day 5: ReactとAPI連携

## 今日のゴール

- ReactからSpring Boot APIを呼び出せる
- 一覧表示、登録の流れをつなげられる
- フロントエンドとバックエンドを分けて考えられる
- 画像URLを使って画像を表示できる
- 1週間の内容を自分の言葉で説明できる
- AIが作った実装を見て、画面からAPIまでの流れを追える

## 扱う内容

- fetchまたはaxios
- API呼び出し
- ローディング状態
- エラー表示
- CORS
- まとめ

## 今日の重点

| ラベル | 重点 | 今日できるようにすること |
| --- | --- | --- |
| 🟦 概念 | ReactとSpring Bootの接続 | 画面操作からAPI、JSON、state更新まで追える |
| 🟩 実装 | `api/movies.js` | API通信をコンポーネントから分離できる |
| 🟩 実装 | `useEffect` と `fetch` | 初回表示で一覧APIを呼べる |
| 🟨 確認 | Networkタブ | GET、POST、レスポンスJSONを確認できる |
| 🟥 注意 | ReactはDBを直接触らない | API経由でデータを取得・保存する |

## ライブコーディングと演習

```text
ライブコーディング  Movie API連携
演習              Restaurant API連携
```

Movieで一覧取得と登録のAPI連携を作り、同じ構造をRestaurantへ置き換えます。
Day5の演習では、説明した一覧取得と登録だけを扱います。
編集、削除、フィルタは、説明してから別の演習で扱います。

## 進める順番

```text
1. Day3のAPIとDay4のReact画面を復習する
2. fetchの中身とHTTPリクエストの対応を確認する
3. useEffectで一覧APIを呼ぶ流れを確認する
4. CORSとNetworkタブの見方を確認する
5. 一覧、登録のAPI接続を段階的に実装する
6. エラー表示の考え方を確認する
7. 演習の進め方と完成チェックを確認する
```

Day5では、画面とAPIを一気に全部つなげません。
まず一覧取得だけをつなぎ、Networkで確認してから登録へ進みます。

## 今日の大事な考え方

フロントエンドとバックエンドは、APIでつながる。

```text
Reactのボタンを押す
  ↓
fetchでAPIを呼ぶ
  ↓
Spring Bootが処理する
  ↓
DBを更新する
  ↓
JSONを返す
  ↓
Reactがstateを更新する
  ↓
画面が変わる
```

## Day5で使うReact重要事項

Day5では、Day4で学んだReactの基本をAPI連携に使います。
特に次の対応を意識します。

| 色 | 覚えること | Day5での役割 |
| --- | --- | --- |
| 🟦 | `fetch` | Spring Boot APIを呼ぶ |
| 🟪 | `useEffect` | 画面を開いたあとに一覧APIを呼ぶ |
| 🟥 | `useState` | APIから取得した一覧、読み込み中、エラーをstateとして持つ |
| 🟨 | `props` | `App.jsx` から `MovieList` や `MovieForm` へデータや関数を渡す |
| ⬛ | `event handler` | フォーム送信やボタンクリックでAPIを呼ぶ |
| 🟩 | `JSX` | stateの値を画面に表示する |

Day5で覚える流れ:

```text
画面を開く
  useEffect
  ↓
APIを呼ぶ
  fetch
  ↓
JSONを受け取る
  response.json()
  ↓
stateを更新する
  setMovies(data)
  ↓
画面が変わる
  JSXが再描画される
```

登録の流れ:

```text
フォームに入力する
  useState / onChange
  ↓
登録ボタンを押す
  onSubmit
  ↓
APIへ送る
  fetch + POST + JSON.stringify
  ↓
一覧を更新する
  setMovies または loadMovies()
```

## 最初に伝えること

Day5では、これまで別々に作ってきたReactとSpring Bootをつなげる。

ここで重要なのは、ReactとSpring Bootの責務を混ぜないこと。

```text
React
  画面を表示する
  入力を受け取る
  APIを呼ぶ
  APIの結果で画面を更新する

Spring Boot
  APIを受け取る
  処理する
  DBへ保存・取得する
  JSONを返す
```

ReactはDBを直接触らない。
Spring Bootは画面を直接描画しない。
両者はAPIでつながる。

## API呼び出しの基本

ReactからAPIを呼ぶには `fetch` を使う。

一覧取得の例:

```jsx
export async function fetchMovies() {
  const response = await fetch("http://localhost:8080/api/movies");
  return response.json();
}
```

登録の例:

```jsx
export async function createMovie(movie) {
  const response = await fetch("http://localhost:8080/api/movies", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(movie),
  });

  return response.json();
}
```

見るポイント:

- `method` でHTTPメソッドを指定する
- JSONを送るときは `Content-Type` を指定する
- JavaScriptのオブジェクトは `JSON.stringify` でJSON文字列にする
- APIの結果は `response.json()` で取り出す

![fetchでAPIを呼ぶときの中身](../images/fetch-request-map.png)

## useEffectで一覧を取得する

画面を開いたときに一覧を取得する。

Reactでは、コンポーネントの表示中にAPIを呼びたい場面があります。
ただし、画面を描画する処理の中で直接 `fetchMovies()` を呼ぶと、再描画のたびにAPIを呼んでしまう可能性があります。

そこで `useEffect` を使います。
`useEffect` は、「画面に表示されたあとで実行したい処理」を書く場所です。

今回の使い方:

```text
画面を初めて表示したあとに、映画一覧APIを1回だけ呼ぶ
```

```jsx
useEffect(() => {
  async function loadMovies() {
    const data = await fetchMovies();
    setMovies(data);
  }

  loadMovies();
}, []);
```

読み方:

```text
useEffect(() => { ... }, [])
  画面が表示されたあとに中の処理を実行する。

() => { ... }
  useEffectに渡している関数。
  ここに「あとで実行したい処理」を書く。

[]
  依存配列。
  空配列にすると、初回表示後に1回だけ実行する。
```

流れ:

```text
画面を開く
  ↓
useEffectが動く
  ↓
GET /api/movies を呼ぶ
  ↓
JSONを受け取る
  ↓
setMoviesでstateを更新する
  ↓
映画一覧が表示される
```

なぜ `async function loadMovies()` を中に作るのか:

```text
API通信ではawaitを使いたい。
ただし、useEffectに渡す関数そのものをasyncにする書き方は避ける。
そのため、useEffectの中でasync関数を定義し、その関数を呼び出す。
```

つまり、次の2段階で考えます。

```text
1. loadMoviesを定義する
   APIを呼んで、結果をstateへ入れる関数。

2. loadMovies()を実行する
   画面表示後に実際にAPIを呼ぶ。
```

![useEffectでAPIを呼ぶ流れ](../images/useeffect-api-flow.png)

## CORS

ReactとSpring Bootを別々のポートで動かすと、ブラウザが通信を止めることがある。

例:

```text
React        http://localhost:5173
Spring Boot  http://localhost:8080
```

このようにオリジンが違う場合、Spring Boot側でReactからのアクセスを許可する必要がある。

CORSは、まず「ブラウザの安全機能」と考える。
エラーが出たら、ReactのコードだけでなくSpring Boot側の設定も確認する。

![CORSの基本](../images/cors-basic.png)

## 画面操作とAPIの対応

```text
一覧表示  GET    /api/restaurants
登録      POST   /api/restaurants
編集      PUT    /api/restaurants/{id}
削除      DELETE /api/restaurants/{id}
```

Day5では、まず一覧表示と登録をつなぎます。
その後、同じ考え方で編集、削除、フィルタをつなぎます。

## ライブコーディング: Movie API連携

ここからは、説明者がMovie題材でReact画面とSpring Boot APIを接続します。
参加者は、APIを呼ぶ処理をどのファイルに置くか、APIの結果がどこでstateに入るかを見ながら確認します。

Day4で作ったMovie画面を使い、固定データをAPIから取得する形へ変更します。

まずつなぐAPI:

- 一覧取得
- 登録

確認する場所:

- Bruno
- ブラウザのNetwork
- Reactの画面表示
- Spring Bootのログ

## 説明者用: ライブコーディングで作るもの

Day5のライブコーディングでは、Movie題材でReactとSpring Boot APIを接続します。
完成コードを一気に貼るのではなく、次の順番で小さくつなぎます。
各ステップでブラウザとNetworkを確認し、「今どのファイルが何を担当しているか」を言葉にします。

```text
1. BrunoでGET /api/moviesを確認する
   Spring Boot APIが先に動いていることを確認する。
   画面につなぐ前に、API単体でJSONが返ることを見る。

2. api/movies.js
   APIを呼ぶ関数を作る。
   fetchMoviesで一覧取得、createMovieで登録を行う。
   ReactコンポーネントにURLを直接書かないことを説明する。

3. App.jsx
   movies、loading、errorのstateを持つ。
   useEffectでfetchMoviesを呼び、取得したJSONをmovies stateへ入れる。

4. ブラウザのNetworkを確認する
   GET /api/movies が呼ばれているか見る。
   レスポンスJSONが画面に表示されるか見る。

5. MovieForm.jsx
   入力値をform stateで管理する。
   onSubmitで親へformを渡す。
   MovieFormからAPIを直接呼ばないことを説明する。

6. App.jsx
   MovieFormから受け取ったmovieをcreateMovieへ渡す。
   登録後にloadMoviesを呼び、一覧を再取得する。

7. もう一度Networkを確認する
   POST /api/movies が呼ばれているか見る。
   その後GET /api/moviesで一覧が更新されるか見る。

8. loading / error
   通信中と失敗時の表示をstateで管理する。
```

余裕があれば扱う範囲:

```text
9. PUT /api/movies/{id}
   編集をAPIにつなぐ。

10. DELETE /api/movies/{id}
   削除をAPIにつなぐ。

11. GET /api/movies?genre=SF
   クエリパラメータで絞り込む。
```

作るファイル:

```text
frontend/src/api/movies.js
frontend/src/App.jsx
frontend/src/components/MovieForm.jsx
frontend/src/components/MovieList.jsx
frontend/src/components/MovieCard.jsx
frontend/src/components/MovieFilter.jsx  余裕があれば
```

説明者が各ステップで確認すること:

```text
BrunoでGET /api/moviesを確認したあと
  Spring Boot APIが起動しているか。
  JSON配列が返っているか。

api/movies.jsを作ったあと
  fetchMoviesでGET /api/moviesを呼べているか。
  createMovieでPOST /api/moviesを呼べているか。
  APIのURLがコンポーネント側に散らばっていないか。

App.jsxで一覧取得をつないだあと
  useEffectで初回表示時にfetchMoviesを呼んでいるか。
  取得したJSONをsetMoviesでstateへ入れているか。
  MovieListへmoviesをpropsで渡しているか。

MovieFormを登録APIにつないだあと
  MovieFormがAPIを直接呼んでいないか。
  onSubmitで親へ入力値を渡しているか。
  App.jsxでcreateMovieを呼んでいるか。

登録後に一覧を更新するとき
  loadMoviesをもう一度呼んでいるか。
  POSTのあとにGETが呼ばれているか。
  画面とDBの状態が揃っているか。
```

説明者が口頭で補足するとよいこと:

```text
ReactはAPIを呼ぶ。
APIの結果をstateに入れる。
stateが変わると画面が変わる。

フォームは入力を集める部品。
APIを呼ぶ中心はApp.jsxに置く。

まずGETだけをつなぐ。
GETが動いたらPOSTをつなぐ。
POSTが動いたら一覧を再取得する。

この順番にすると、どこで壊れたか分かりやすい。
```

## ライブコーディング用コピペコード

ライブコーディングでは、まず貼って動かしてから読みます。
その後で、どのファイルが何を担当しているか、APIの結果がどこでstateに入るかを確認します。
貼るときも、できれば次の順番で小さく確認します。

```text
1. api/movies.js
   Spring Boot APIを呼ぶ関数を確認する

2. App.jsx
   useEffectで一覧を取得する流れを確認する

3. App.jsx
   登録後にAPIを呼び、一覧を再取得する流れを確認する

4. MovieForm.jsx
   フォームは入力値を親へ渡すだけにする
```

### api/movies.js

```jsx
const API_BASE_URL = "http://localhost:8080/api/movies";

export async function fetchMovies() {
  const response = await fetch(API_BASE_URL);

  if (!response.ok) {
    throw new Error("Failed to fetch movies");
  }

  return response.json();
}

export async function createMovie(movie) {
  const response = await fetch(API_BASE_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(movie),
  });

  if (!response.ok) {
    throw new Error("Failed to create movie");
  }

  return response.json();
}
```

貼った後に見るポイント:

```text
API_BASE_URL
  APIのURLを1か所にまとめている。

fetchMovies
  GET /api/movies を呼ぶ。
  一覧取得で使う。

createMovie
  POST /api/movies を呼ぶ。
  登録で使う。

response.ok
  HTTPステータスが成功かどうかを確認している。

JSON.stringify(movie)
  JavaScriptのオブジェクトをJSON文字列に変換している。
```

動作確認:

```text
この時点では、画面はまだ変わらない。
api/movies.js は関数を用意しただけ。
次にApp.jsxから呼んで、Networkで確認する。
```

### App.jsx 一覧取得

Day4で作った固定データ表示を、APIから取得する形に変更します。

```jsx
import { useEffect, useState } from "react";
import { fetchMovies, createMovie } from "./api/movies";
import { MovieForm } from "./components/MovieForm";
import { MovieList } from "./components/MovieList";

export default function App() {
  const [movies, setMovies] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  async function loadMovies() {
    try {
      setLoading(true);
      setError("");
      const data = await fetchMovies();
      setMovies(data);
    } catch (error) {
      setError("映画一覧の取得に失敗しました");
    } finally {
      setLoading(false);
    }
  }

  useEffect(() => {
    loadMovies();
  }, []);

  async function handleAddMovie(movie) {
    try {
      setError("");
      await createMovie(movie);
      await loadMovies();
    } catch (error) {
      setError("映画の登録に失敗しました");
    }
  }

  return (
    <main className="min-h-screen bg-slate-50 p-6">
      <div className="mx-auto grid max-w-5xl gap-6">
        <h1 className="text-2xl font-bold text-slate-900">映画メモ</h1>

        <MovieForm onAddMovie={handleAddMovie} />

        {loading && <p>読み込み中です</p>}
        {error && <p className="text-red-600">{error}</p>}

        <MovieList movies={movies} />
      </div>
    </main>
  );
}
```

貼った後に見るポイント:

```text
useEffect
  画面を開いたあとにloadMoviesを呼ぶ。

loadMovies
  fetchMoviesでAPIを呼び、setMoviesでstateへ入れる。

movies state
  APIから取得した一覧データを持つ。

loading state
  通信中かどうかを持つ。

error state
  API通信に失敗したときのメッセージを持つ。

handleAddMovie
  createMovieで登録し、その後loadMoviesで一覧を取り直す。
  登録に失敗した場合はerror stateを更新する。
```

動作確認:

```text
1. Spring Bootを起動する
2. Reactを起動する
3. ブラウザで画面を開く
4. 開発者ツールのNetworkを開く
5. GET /api/movies が呼ばれているか確認する
6. レスポンスJSONがMovieListに表示されているか確認する
```

うまくいかないときに見る場所:

```text
Console
  JavaScriptエラーを見る。

Network
  APIのURL、HTTPメソッド、ステータスコード、レスポンスを見る。

Spring Bootログ
  Controllerまで届いているか、例外が出ていないかを見る。
```

### MovieForm.jsx

MovieFormはAPIを直接呼びません。
入力値を集めて、親の `App.jsx` に渡します。

```jsx
import { useState } from "react";

const initialForm = {
  title: "",
  genre: "",
  memo: "",
  imageUrl: "",
  status: "WATCHED",
};

export function MovieForm({ onAddMovie }) {
  const [form, setForm] = useState(initialForm);

  function handleChange(event) {
    const { name, value } = event.target;
    setForm({ ...form, [name]: value });
  }

  async function handleSubmit(event) {
    event.preventDefault();
    await onAddMovie(form);
    setForm(initialForm);
  }

  return (
    <form onSubmit={handleSubmit} className="grid gap-3 rounded-lg border border-slate-200 bg-white p-4">
      <input name="title" value={form.title} onChange={handleChange} placeholder="タイトル" className="rounded border p-2" />
      <input name="genre" value={form.genre} onChange={handleChange} placeholder="ジャンル" className="rounded border p-2" />
      <input name="imageUrl" value={form.imageUrl} onChange={handleChange} placeholder="画像URL" className="rounded border p-2" />
      <textarea name="memo" value={form.memo} onChange={handleChange} placeholder="メモ" className="rounded border p-2" />
      <select name="status" value={form.status} onChange={handleChange} className="rounded border p-2">
        <option value="WATCHED">見た</option>
        <option value="WANT_TO_WATCH">見たい</option>
        <option value="FAVORITE">お気に入り</option>
      </select>
      <button className="rounded bg-cyan-600 px-4 py-2 font-semibold text-white" type="submit">
        登録する
      </button>
    </form>
  );
}
```

貼った後に見るポイント:

```text
form state
  入力中の値を持つ。

handleChange
  入力欄が変わるたびにform stateを更新する。

handleSubmit
  フォーム送信時に動く。
  onAddMovie(form) で親へ入力値を渡す。
  登録が終わってからフォームを空に戻す。

MovieForm
  APIは呼ばない。
  APIを呼ぶのはApp.jsxのhandleAddMovie。
```

動作確認:

```text
1. フォームに入力する
2. 登録ボタンを押す
3. NetworkでPOST /api/movies が呼ばれているか確認する
4. Request Payloadに入力した値が入っているか確認する
5. POST後にGET /api/movies が呼ばれているか確認する
6. 一覧に登録したMovieが表示されるか確認する
```

### ライブコーディング後に必ず確認すること

```text
GET /api/movies
  画面を開いたときに呼ばれる。

POST /api/movies
  登録ボタンを押したときに呼ばれる。

POST後のGET /api/movies
  登録後に一覧を最新化するために呼ばれる。
```

最後に、次の流れを声に出して確認します。

```text
画面を開く
  ↓
useEffect
  ↓
fetchMovies
  ↓
GET /api/movies
  ↓
setMovies
  ↓
MovieListに表示
```

```text
フォームに入力する
  ↓
onSubmit
  ↓
onAddMovie
  ↓
createMovie
  ↓
POST /api/movies
  ↓
loadMovies
  ↓
一覧を再取得
```

## ハンズオン

React画面とSpring Boot APIを接続し、ミニアプリとして完成させる。

基本ライン:

- お店を登録できる
- お店一覧を見られる
- 登録後に一覧を更新できる
- 画像を表示できる
- NetworkでGETとPOSTを確認できる

余裕があれば追加すること:

- お店を編集できる
- お店を削除できる
- 地域、ジャンル、ステータスで絞り込める
- 操作後に一覧を再取得できる

## 実装の進め方

ReactとSpring Bootをつなぐときは、1つずつ確認します。

### 1. API呼び出し用のファイルを作る

ライブコーディングではMovie題材で確認します。
APIを呼ぶ処理は `frontend/src/api/movies.js` にまとめます。

🟩 このステップで新しく作るファイル:

```text
frontend/src/api/movies.js
```

作る場所:

```text
frontend/
└─ src/
   ├─ App.jsx
   ├─ api/
   │  └─ movies.js        ここにAPIを呼ぶ関数を書く
   └─ components/
      ├─ MovieForm.jsx
      ├─ MovieList.jsx
      └─ MovieCard.jsx
```

それぞれの役割:

```text
frontend/src/api/movies.js
  Spring Boot APIを呼ぶ関数を書く。
  fetchMovies、createMovie などを置く。
  JSXは書かない。
  画面の見た目も書かない。

frontend/src/App.jsx
  movies stateを持つ。
  api/movies.js の関数をimportして呼ぶ。
  取得したデータをMovieListへ渡す。

frontend/src/components/MovieList.jsx
  movies配列を受け取り、MovieCardを並べる。

frontend/src/components/MovieCard.jsx
  Movie 1件分を表示する。

frontend/src/components/MovieForm.jsx
  入力値を管理し、登録したいMovieを親へ渡す。
```

まず `frontend/src/api/` ディレクトリがなければ作ります。
その中に `movies.js` を作ります。

```text
frontend/src/api/movies.js
```

このファイルには、画面部品ではなく「APIを呼ぶ関数」だけを書きます。
Reactコンポーネントから見ると、API呼び出しの細かい書き方を `movies.js` に隠せます。

```jsx
const API_BASE_URL = "http://localhost:8080/api/movies";

export async function fetchMovies() {
  const response = await fetch(API_BASE_URL);
  return response.json();
}
```

1行ずつ読む:

```text
const API_BASE_URL = "http://localhost:8080/api/movies";
  APIのURLを定数としてまとめている。
  URLを毎回直接書くより、後で変更しやすい。

export async function fetchMovies()
  映画一覧を取得する関数。
  exportしているので、App.jsxなど別ファイルから呼び出せる。
  asyncは、API通信のような時間がかかる処理を書くために使う。

const response = await fetch(API_BASE_URL);
  fetchでSpring Boot APIへGETリクエストを送る。
  awaitは、レスポンスが返ってくるまで待つという意味。

return response.json();
  レスポンスのJSONをJavaScriptのデータとして取り出す。
  この結果がmovies stateに入る。
```

見るポイント:

- Reactコンポーネントの中にURLを何度も書かない
- APIを呼ぶ関数は `api/` にまとめる
- 画面側は `fetchMovies()` を呼ぶだけにする

`App.jsx` から使うときは、次のようにimportします。

```jsx
import { fetchMovies, createMovie } from "./api/movies";
```

読み方:

```text
import { fetchMovies, createMovie } from "./api/movies";
  api/movies.js でexportした関数をApp.jsxで使えるようにしている。

./api/movies
  App.jsxから見た相対パス。
  frontend/src/api/movies.js を指している。
```

API通信では、成功だけでなく失敗も考えます。
最初は次のように、HTTPステータスを見てエラーにできます。

```jsx
export async function fetchMovies() {
  const response = await fetch(API_BASE_URL);

  if (!response.ok) {
    throw new Error("Failed to fetch movies");
  }

  return response.json();
}
```

1行ずつ読む:

```text
response.ok
  HTTPステータスが成功かどうかを表す。
  200番台ならtrueになる。

throw new Error(...)
  API呼び出しに失敗したことを、呼び出し元へ伝える。
```

### 2. 一覧取得だけをつなぐ

最初に固定データをやめて、APIから取得したデータをstateに入れます。
ここからは `frontend/src/App.jsx` を編集します。

🟦 このステップで編集するファイル:

```text
frontend/src/App.jsx
```

`App.jsx` でやること:

```text
1. fetchMoviesをimportする
2. movies stateを用意する
3. useEffectでfetchMoviesを呼ぶ
4. setMoviesでAPIレスポンスをstateに入れる
5. MovieListへmoviesを渡す
```

```jsx
useEffect(() => {
  async function loadMovies() {
    const data = await fetchMovies();
    setMovies(data);
  }

  loadMovies();
}, []);
```

1行ずつ読む:

```text
useEffect(() => { ... }, [])
  画面が表示された後に処理を実行する。
  最後の [] は、初回表示時だけ実行するという意味。
  ここでは、画面を開いたときに一覧を1回だけ取得したいので [] にする。

async function loadMovies()
  APIから映画一覧を読み込むための関数。
  useEffectの中でasync処理を扱うために関数として分けている。

const data = await fetchMovies();
  api/movies.js の fetchMovies を呼び、映画一覧を取得する。
  dataにはAPIレスポンスのJSONが入る。

setMovies(data);
  取得したデータをmovies stateに入れる。
  stateが変わると、画面の映画一覧が更新される。

loadMovies();
  定義した読み込み関数を実行する。
```

ここで説明者が補足すること:

```text
useEffectは、画面表示後に外部とのやり取りをするために使う。
今回の外部とのやり取りは、Spring Boot APIを呼ぶこと。

[] を付けないと、再描画のたびに実行される可能性がある。
今回の一覧取得は初回だけでよいので、[] を付ける。

useEffectの中でasync関数を作ってから呼ぶのは、
awaitを使ってAPI結果を待ちたいから。
```

登録後に一覧をもう一度取得したい場合は、`loadMovies` を再利用します。
このあと登録APIをつなぐときに、登録後に `loadMovies()` を呼ぶと、DBに保存された最新の一覧を画面へ反映できます。

画面では、読み込み中とエラーもstateで持ちます。

```jsx
const [loading, setLoading] = useState(false);
const [error, setError] = useState("");
```

```jsx
async function loadMovies() {
  try {
    setLoading(true);
    setError("");
    const data = await fetchMovies();
    setMovies(data);
  } catch (error) {
    setError("映画一覧の取得に失敗しました");
  } finally {
    setLoading(false);
  }
}
```

1行ずつ読む:

```text
loading
  API通信中かどうかを表すstate。
  trueなら「読み込み中」と表示できる。

error
  エラーメッセージを入れるstate。
  空文字ならエラーなしとして扱える。

try
  成功するかもしれない処理を書く。

catch
  API通信に失敗したときの処理を書く。

finally
  成功しても失敗しても最後に実行する。
  読み込み中表示を止める処理に使いやすい。
```

確認すること:

- Networkで `GET /api/movies` が呼ばれている
- レスポンスJSONに映画データが入っている
- `setMovies(data)` の後に画面が表示される
- APIが失敗したときにエラー表示へ切り替えられる

### 3. 登録APIをつなぐ

フォーム送信時にPOSTします。
`createMovie` も `frontend/src/api/movies.js` に書きます。
一覧取得と同じように、API通信の処理は `api/` にまとめます。

🟩 このステップで追記するファイル:

```text
frontend/src/api/movies.js
```

🟦 このステップで編集するファイル:

```text
frontend/src/App.jsx
frontend/src/components/MovieForm.jsx
```

```jsx
export async function createMovie(movie) {
  const response = await fetch(API_BASE_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(movie),
  });

  return response.json();
}
```

1行ずつ読む:

```text
export async function createMovie(movie)
  映画を新規登録する関数。
  movieにはフォームで入力した値が入る。

const response = await fetch(API_BASE_URL, { ... })
  Spring Boot APIへリクエストを送る。
  第2引数にmethod、headers、bodyなどの設定を書く。

method: "POST"
  新しくデータを作るAPIなのでPOSTを指定する。

headers: { "Content-Type": "application/json" }
  送るデータがJSONであることをSpring Bootへ伝える。

body: JSON.stringify(movie)
  JavaScriptのオブジェクトをJSON文字列に変換して送る。
  fetchのbodyには、そのままオブジェクトを渡せない。

return response.json();
  登録後にAPIから返ってきたJSONを取り出す。
```

画面側では、登録後に一覧を再取得するか、返ってきたデータをstateに追加します。
最初は分かりやすさを優先して、登録後に一覧を再取得してもよいです。

### 4. 編集APIをつなぐ

編集では、URLにidを入れてPUTします。

🟩 このステップで追記するファイル:

```text
frontend/src/api/movies.js
```

🟦 このステップで編集するファイル:

```text
frontend/src/App.jsx
frontend/src/components/MovieForm.jsx
frontend/src/components/MovieCard.jsx
```

このステップでやること:

```text
1. api/movies.js に updateMovie(id, movie) を追加する
2. App.jsx に編集中のMovieを持つstateを追加する
3. MovieCard.jsx の編集ボタンから、編集対象をApp.jsxへ渡す
4. MovieForm.jsx に編集対象の値を表示する
5. 保存時に updateMovie(id, movie) を呼ぶ
6. 保存後に loadMovies() で一覧を再取得する
```

```jsx
export async function updateMovie(id, movie) {
  const response = await fetch(`${API_BASE_URL}/${id}`, {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(movie),
  });

  return response.json();
}
```

1行ずつ読む:

```text
updateMovie(id, movie)
  更新したいidと、フォームで入力した値を受け取る。

`${API_BASE_URL}/${id}`
  /api/movies/1 のようなURLを作る。
  どのデータを更新するかをURLで指定する。

method: "PUT"
  既存データを更新するため、PUTを指定する。

body: JSON.stringify(movie)
  更新後の内容をJSONとして送る。
```

画面側では、編集保存後に一覧を再取得すると流れを理解しやすいです。

### 5. 削除APIをつなぐ

削除では、URLにidを入れてDELETEします。

🟩 このステップで追記するファイル:

```text
frontend/src/api/movies.js
```

🟦 このステップで編集するファイル:

```text
frontend/src/App.jsx
frontend/src/components/MovieCard.jsx
```

このステップでやること:

```text
1. api/movies.js に deleteMovie(id) を追加する
2. App.jsx に handleDeleteMovie(id) を追加する
3. MovieCard.jsx の削除ボタンから、削除したいidを親へ渡す
4. App.jsx で deleteMovie(id) を呼ぶ
5. 削除後に loadMovies() で一覧を再取得する
```

```jsx
export async function deleteMovie(id) {
  await fetch(`${API_BASE_URL}/${id}`, {
    method: "DELETE",
  });
}
```

1行ずつ読む:

```text
deleteMovie(id)
  削除したいデータのidを受け取る。

method: "DELETE"
  データを削除するため、DELETEを指定する。

レスポンスJSONを受け取らない
  削除APIは、返すデータがない設計にすることがある。
```

削除後も、一覧を再取得すると画面とDBの状態を揃えやすいです。

### 6. フィルタ条件をAPIへ渡す

地域やジャンルで絞り込む場合は、クエリパラメータをURLにつけます。

🟩 このステップで追記するファイル:

```text
frontend/src/api/movies.js
```

🟦 このステップで編集するファイル:

```text
frontend/src/App.jsx
frontend/src/components/MovieFilter.jsx
```

MovieFilter.jsx がまだない場合は、このタイミングで作ります。

```text
frontend/
└─ src/
   └─ components/
      └─ MovieFilter.jsx
```

このステップでやること:

```text
1. MovieFilter.jsx で絞り込み条件を選べるようにする
2. App.jsx に filters stateを追加する
3. MovieFilter.jsx から、変更された条件をApp.jsxへ渡す
4. App.jsx から fetchMovies(filters) を呼ぶ
5. api/movies.js で filters をクエリパラメータに変換する
6. Networkで /api/movies?genre=SF のようなURLになっているか確認する
```

```jsx
export async function fetchMovies(filters = {}) {
  const params = new URLSearchParams(filters);
  const response = await fetch(`${API_BASE_URL}?${params.toString()}`);
  return response.json();
}
```

1行ずつ読む:

```text
filters = {}
  絞り込み条件をオブジェクトで受け取る。
  条件がない場合は空のオブジェクトにする。

new URLSearchParams(filters)
  { genre: "SF" } のような条件を、genre=SF というURL用の文字列に変換する。

`${API_BASE_URL}?${params.toString()}`
  /api/movies?genre=SF のようなURLを作る。
```

空の条件を送ると `?` だけが付くことがあります。
実装では、条件があるときだけクエリパラメータを付ける書き方にしてもよいです。

最初からこの形で書いてもよいですが、理解を優先するなら次の順番で育てます。

```text
1. fetchMovies() で全件取得する
2. fetchMovies(filters) に変更する
3. 画面のフィルタstateをfiltersとして渡す
```

## 演習

次の順番でReactとAPIを接続します。

```text
1. 一覧取得をAPIにつなぐ
2. 登録をAPIにつなぐ
3. 編集をAPIにつなぐ
4. 削除をAPIにつなぐ
5. フィルタ条件をAPIにつなぐ
```

各機能で確認すること:

- どのボタンや画面操作から始まるか
- どのAPI関数を呼んでいるか
- HTTPメソッドは何か
- Networkでリクエストを確認できるか
- レスポンスJSONをstateに反映しているか
- 画面表示が変わっているか

## AIへの依頼例

Day5では、ReactとSpring Boot APIを接続します。
一度に全部つなげず、一覧取得から始めます。

一覧取得の依頼例は、まず穴埋めしてから使います。

```text
MovieのAPI連携を参考にして、Restaurantの一覧取得をAPIへつなぎたいです。
まず、下の穴埋めが正しいか確認してください。

対象ファイル:
- frontend/src/api/__________.js
- frontend/src/__________.jsx

API:
______ http://localhost:8080/api/__________

条件:
- MovieのfetchMoviesに対応する関数名: ______
- 初回表示時に使うReactの機能: ______
- 取得したJSONを入れるstate: ______
- 一覧表示へ渡すコンポーネント: ______

コードを出す前に、useEffect、fetch、setRestaurantsの流れを説明してください。
```

参考にするMovie側の例:

```text
API file        api/movies.js
Fetch function  fetchMovies
Create function createMovie
State name      movies
Setter name     setMovies
List component  MovieList
API path        /api/movies
```

Restaurant側のファイル名、関数名、state名、APIパスは、Movieの例を見ながら自分で埋めます。

登録APIの依頼例:

```text
Movieの登録API連携を参考にして、Restaurantの登録フォームからAPIを呼べるようにしたいです。
まず、下の穴埋めが正しいか確認してください。

API:
______ http://localhost:8080/api/__________

対象ファイル:
- frontend/src/api/__________.js
- frontend/src/__________.jsx
- frontend/src/components/__________.jsx

条件:
- MovieのcreateMovieに対応する関数名: ______
- フォーム送信時に呼ぶ関数: ______
- 登録後に何をするか: ______
- 送信するJSONの項目: ______

コードを出す前に、送信するJSONとレスポンスJSONの違いを説明してください。
```

編集、削除、フィルタの依頼例:

```text
MovieのAPI連携を参考にして、Restaurantの編集、削除、フィルタを追加したいです。
まず、下の穴埋めが正しいか確認してください。

編集API:
______ http://localhost:8080/api/__________/{id}

削除API:
______ http://localhost:8080/api/__________/{id}

フィルタAPI:
______ http://localhost:8080/api/__________?______=______

条件:
- MovieのupdateMovieに対応する関数名: ______
- MovieのdeleteMovieに対応する関数名: ______
- URLにidを入れる理由: ______
- フィルタ条件をURLに入れる方法: ______

コードを出す前に、PUT、DELETE、クエリパラメータの違いを説明してください。
```

AIの回答を確認するときのポイント:

- APIのURLが正しいか
- HTTPメソッドが正しいか
- `Content-Type: application/json` が必要な箇所にあるか
- `JSON.stringify` で送信しているか
- API結果をstateに反映しているか
- Networkでリクエストを確認できるか
- 編集、削除、フィルタを一度に入れず、1つずつ確認しているか
- loading、errorのstateが必要な箇所にあるか

## 動作確認の順番

フルスタック開発では、いきなり全部つなげて確認しない。

次の順番で確認すると、エラーの場所を見つけやすい。

```text
1. Spring Boot単体でAPIを確認する
2. React単体で固定データ表示を確認する
3. Reactから一覧APIを呼ぶ
4. 登録APIをつなぐ
5. 編集APIをつなぐ
6. 削除APIをつなぐ
7. フィルタをつなぐ
```

一気に全部やると、どこで壊れているか分からなくなる。

## エラー切り分け

画面がうまく動かないときは、次の順番で見る。

```text
1. ブラウザのConsole
   JavaScriptエラーが出ていないか

2. ブラウザのNetwork
   APIが呼ばれているか
   URLは正しいか
   ステータスコードは何か
   レスポンスは何か

3. Spring Bootのログ
   例外が出ていないか
   Controllerに届いているか

4. DB
   データが保存されているか
```

最初は「まずNetworkを見る」習慣をつけてもらうとよい。

## 今日の確認ポイント

- ReactからSpring Boot APIを呼ぶ流れを説明できる
- `GET`、`POST`、`PUT`、`DELETE` がReactのどの操作に対応するか説明できる
- APIの結果でstateを更新し、画面が変わることを説明できる
- CORSエラーが出たときに、何が起きているか大まかに説明できる
- エラー時にConsole、Network、Spring Bootログを見る順番が分かる

## よくある混乱

### APIは呼べているのに画面が変わりません

APIの結果をstateに入れていない可能性がある。

Reactでは、データを取得しただけでは画面は変わらない。
`setRestaurants` のようなstate更新が必要。

### Spring Bootでは成功しているのにReactでエラーになります

CORS、URL間違い、JSONの形の違い、React側の処理ミスが考えられる。

Networkタブで、実際にどんなレスポンスが返っているか確認する。

### 画像だけ表示されません

`imageUrl` の値を確認する。

APIレスポンスに `imageUrl` が含まれているか、URL先の画像が存在するかを見る。

## メモ

最後は「自分で1項目追加してみる」など、小さな応用課題を入れる。
