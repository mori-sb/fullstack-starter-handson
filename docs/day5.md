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

```jsx
useEffect(() => {
  async function loadMovies() {
    const data = await fetchMovies();
    setMovies(data);
  }

  loadMovies();
}, []);
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

Day5で実装するのは、一覧表示と登録です。
編集、削除はAPIとの対応だけ確認し、実装演習には含めません。

## ハンズオン

React画面とSpring Boot APIを接続し、ミニアプリとして完成させる。

完成ライン:

- お店を登録できる
- お店一覧を見られる
- 画像を表示できる
- 登録後に一覧を更新できる

## 実装の進め方

ReactとSpring Bootをつなぐときは、1つずつ確認します。

### 1. API呼び出し用のファイルを作る

ライブコーディングではMovie題材で確認します。
APIを呼ぶ処理は `frontend/src/api/movies.js` にまとめます。

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

### 2. 一覧取得だけをつなぐ

最初に固定データをやめて、APIから取得したデータをstateに入れます。

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

確認すること:

- Networkで `GET /api/movies` が呼ばれている
- レスポンスJSONに映画データが入っている
- `setMovies(data)` の後に画面が表示される

### 3. 登録APIをつなぐ

フォーム送信時にPOSTします。

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

## 演習

次の順番でReactとAPIを接続します。

```text
1. 一覧取得をAPIにつなぐ
2. 登録をAPIにつなぐ
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

講師と答え合わせする観点:

```text
Movie側で見たもの          Restaurant側で作るもの
api/movies.js              api/restaurants.js
fetchMovies                fetchRestaurants
movies state               restaurants state
setMovies                  setRestaurants
MovieList                  RestaurantList
GET /api/movies            GET /api/restaurants
```

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

AIの回答を確認するときのポイント:

- APIのURLが正しいか
- HTTPメソッドが正しいか
- `Content-Type: application/json` が必要な箇所にあるか
- `JSON.stringify` で送信しているか
- API結果をstateに反映しているか
- Networkでリクエストを確認できるか

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
