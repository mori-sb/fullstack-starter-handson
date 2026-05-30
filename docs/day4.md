# Day 4: Reactの基本

## 今日のゴール

- Reactコンポーネントの考え方を理解する
- propsとstateの違いを理解する
- useStateとuseEffectを使える
- 画像付きのお店一覧画面を作れる
- お店登録フォームを作れる
- 生成されたReactコードをコンポーネント単位で読める

## 扱う内容

- コンポーネント
- props
- state
- useState
- useEffect
- フォーム入力
- imgタグでの画像表示
- フィルタUI

## 進める順番

```text
1. Reactで作る画面の完成イメージを確認する
2. HTML、CSS、JavaScript、React、Tailwind CSSの関係を確認する
3. Reactのディレクトリ構造とコンポーネント分割を確認する
4. props、state、useStateを図で確認する
5. お店カード、一覧、フォームを段階的に実装する
6. 画像URL表示とフィルタUIを確認する
7. 演習の進め方と確認ポイントを確認する
```

Day4では、API接続を急ぎません。
固定データで画面を作り、Reactのファイル構造、props、state、Tailwind CSSの読み方をつかみます。

## 今日の大事な考え方

Reactでは、画面を部品に分けて考える。

Reactは、HTML、CSS、JavaScriptを組み合わせて画面を作りやすくするものとして見ると分かりやすいです。

```text
HTML        画面の構造
CSS         見た目
JavaScript  動き
React       構造・見た目・動きをコンポーネントとしてまとめる
Tailwind CSS classNameに見た目を書く
```

今回のハンズオンでは、Tailwind CSSを使います。
CSSファイルに細かいスタイルを書き足すよりも、JSXの `className` に見た目の指定を書いていきます。

![HTML/CSS/JavaScriptとReact/Tailwind CSSの関係](../images/react-html-css-js-tailwind.png)

```jsx
<article className="rounded-lg border border-slate-200 bg-white p-4 shadow-sm">
  <h3 className="text-lg font-semibold text-slate-900">
    Cafe Sakura
  </h3>
</article>
```

このように、Reactでは画面構造と見た目の指定が同じコンポーネント内にまとまります。
「この部品の見た目はこのファイルを見れば分かる」と考えると追いやすくなります。

```text
App
  RestaurantFilter
  RestaurantForm
  RestaurantList
    RestaurantCard
```

stateが変わると、Reactが画面を更新する。

```text
入力する
  ↓
stateが変わる
  ↓
画面が再描画される
```

## 最初に伝えること

Reactでは、画面を小さな部品に分けて作る。

最初から1つの大きなファイルに全部書くと、どこが何をしているか分かりにくくなる。
そのため、グルメ管理アプリでは次のように分ける。

```text
App               全体をまとめる
RestaurantFilter  絞り込み条件を選ぶ
RestaurantForm    お店を登録・編集する
RestaurantList    お店カードを並べる
RestaurantCard    お店1件を表示する
```

AIがReactコードを生成した場合も、まずコンポーネント単位で読む。

## Reactのディレクトリ構造

React側は `frontend/` に作ります。

画面を部品ごとに分けるため、`components/` にコンポーネントを置きます。
APIを呼ぶ処理は `api/` にまとめます。
Tailwind CSSを使うため、見た目の多くは各コンポーネントの `className` に書きます。

```text
frontend/
└─ src/
   ├─ main.jsx
   ├─ App.jsx
   ├─ api/
   │  └─ restaurants.js
   ├─ components/
   │  ├─ RestaurantFilter.jsx
   │  ├─ RestaurantForm.jsx
   │  ├─ RestaurantList.jsx
   │  └─ RestaurantCard.jsx
   └─ styles/
      └─ app.css
```

それぞれの役割:

```text
main.jsx                 Reactアプリの起動入口
App.jsx                  画面全体の親コンポーネント
api/restaurants.js       Spring Boot APIを呼ぶ関数
RestaurantFilter.jsx     地域・ジャンル・ステータスの絞り込み
RestaurantForm.jsx       お店の登録・編集フォーム
RestaurantList.jsx       お店カードを並べる
RestaurantCard.jsx       お店1件を表示する
app.css                  全体に共通する最低限のスタイル
```

最初に読む順番:

```text
1. App.jsx
2. RestaurantList.jsx
3. RestaurantCard.jsx
4. RestaurantForm.jsx
5. RestaurantFilter.jsx
6. api/restaurants.js
```

Reactでは、ファイルを分けることで「どの部品が何を担当しているか」を見つけやすくします。

![Reactプロジェクトのディレクトリ構造](../images/react-directory-structure.png)

Tailwind CSSを使う場合、コンポーネント内に次の3つがまとまりやすくなります。

```text
JSXのタグ       画面の構造
className       見た目
イベント処理     ボタンを押したときの動き
```

例:

```jsx
export function RestaurantCard({ restaurant }) {
  return (
    <article className="rounded-lg border bg-white p-4 shadow-sm">
      <img
        className="h-40 w-full rounded object-cover"
        src={restaurant.imageUrl}
        alt={restaurant.name}
      />
      <h3 className="mt-3 text-lg font-semibold">
        {restaurant.name}
      </h3>
      <p className="text-sm text-slate-600">
        {restaurant.area} / {restaurant.genre}
      </p>
    </article>
  );
}
```

見るポイント:

- `<article>` や `<img>` はHTMLに近い画面構造
- `className` はTailwind CSSで見た目を指定している
- `{restaurant.name}` はJavaScriptの値を画面に表示している

![JSXとTailwind CSSの読み方](../images/jsx-tailwind-reading.png)

## propsとstate

Reactで最初に混乱しやすいのが、propsとstate。

### props

親コンポーネントから子コンポーネントへ渡すデータ。

```text
App
  ↓ restaurantを渡す
RestaurantCard
```

子コンポーネントは、受け取ったpropsを使って表示する。

![Reactのpropsの流れ](../images/react-props-flow.png)

### state

画面の中で変化する値。

例えば次のようなもの。

```text
restaurants      お店一覧
form             入力フォームの値
selectedArea     選択中の地域
selectedGenre    選択中のジャンル
selectedStatus   選択中のステータス
```

stateが変わると、Reactは画面を更新する。

## useState

useStateは、画面で変わる値を覚えるために使う。

```jsx
const [selectedArea, setSelectedArea] = useState("すべて");
```

読み方:

```text
selectedArea     現在の値
setSelectedArea  値を変更する関数
"すべて"          最初の値
```

地域フィルタを変更したら、`setSelectedArea` を呼ぶ。
すると `selectedArea` が変わり、画面に表示するお店も変わる。

フォーム入力も同じ考え方です。
入力欄の値をstateに保存しておくことで、登録ボタンを押したときに現在の入力内容をAPIへ渡せるようになります。

![フォーム入力とstateの関係](../images/form-state-flow.png)

## useEffect

useEffectは、画面表示時や値の変化時に処理を実行するために使う。

Day4では固定データで進めるため、useEffectは深追いしすぎない。
Day5でAPI呼び出しとセットで理解する。

```jsx
useEffect(() => {
  // 画面表示時に実行したい処理
}, []);
```

## 画像表示

画像URLは文字列として受け取る。
Reactでは、そのURLを `img` タグの `src` に指定する。

```jsx
<img src={restaurant.imageUrl} alt={restaurant.name} />
```

ここで重要なのは、Reactが画像ファイルを持っているわけではないこと。
ReactはURLを使って、ブラウザに画像を表示させている。

![画像URLをimgタグで表示する流れ](../images/react-image-tag.png)

画像表示の流れ:

```text
restaurants state
  ↓
RestaurantList
  ↓
RestaurantCard
  ↓
<img src={restaurant.imageUrl}>
```

## ハンズオン

固定データを使って一覧画面と登録フォームを作成する。

画面に表示する項目:

- 画像
- 店名
- 地域
- ジャンル
- ステータス
- メモ

フォームで入力する項目:

- 店名
- 地域
- ジャンル
- メモ
- 画像URL
- ステータス

## 実装の進め方

Reactはファイルが分かれるため、次の順番で作ると迷いにくいです。

### 1. 固定データを用意する

まず `App.jsx` にお店データを直接書きます。

```jsx
const initialRestaurants = [
  {
    id: 1,
    name: "新宿うまい店",
    area: "新宿",
    genre: "和食",
    memo: "落ち着いた雰囲気でランチがおすすめです。",
    imageUrl: "https://example.com/images/shinjuku-umai.jpg",
    status: "行きたい",
  },
];
```

1行ずつ読む:

```text
const initialRestaurants = [
  固定のお店データを配列として用意している。
  API接続前は、この配列を画面表示に使う。

{
  ここから1件分のお店データ。

id: 1
  Reactが一覧表示で1件を区別するためのID。

name: "新宿うまい店"
  店名。

area: "新宿"
  地域。

genre: "和食"
  ジャンル。

memo: "落ち着いた雰囲気でランチがおすすめです。"
  カードに表示するメモ。

imageUrl: "https://example.com/images/shinjuku-umai.jpg"
  imgタグで表示する画像URL。

status: "行きたい"
  お店の状態。
```

この時点ではAPIを呼びません。
まず画面に出したいデータの形を確認します。

### 2. RestaurantCardを作る

1件分のお店を表示します。

見るポイント:

- propsで `restaurant` を受け取る
- `restaurant.name` などを画面に表示する
- `restaurant.imageUrl` を `img` の `src` に渡す
- `className` で見た目を整える

### 3. RestaurantListを作る

複数件のお店を並べます。

```jsx
export function RestaurantList({ restaurants }) {
  return (
    <div className="grid gap-4">
      {restaurants.map((restaurant) => (
        <RestaurantCard key={restaurant.id} restaurant={restaurant} />
      ))}
    </div>
  );
}
```

1行ずつ読む:

```text
export function RestaurantList({ restaurants })
  RestaurantListコンポーネントを定義している。
  restaurantsは親コンポーネントからpropsとして受け取る配列。

return (...)
  画面に表示するJSXを返す。

<div className="grid gap-4">
  お店カードを並べる外側の箱。
  classNameはTailwind CSSの見た目指定。

restaurants.map((restaurant) => (...))
  restaurants配列を1件ずつ取り出して、RestaurantCardに変換する。

<RestaurantCard key={restaurant.id} restaurant={restaurant} />
  お店1件分をRestaurantCardへ渡して表示する。
  keyはReactがリストの各要素を区別するために使う。
  restaurant={restaurant} で子コンポーネントへデータを渡している。
```

見るポイント:

- `restaurants` は配列
- `map` で1件ずつ `RestaurantCard` に渡す
- `key` はReactがリストを管理するために必要

### 4. RestaurantFormを作る

フォームの入力値をstateで管理します。

```jsx
const [form, setForm] = useState({
  name: "",
  area: "",
  genre: "",
  memo: "",
  imageUrl: "",
  status: "行きたい",
});
```

1行ずつ読む:

```text
const [form, setForm] = useState(...)
  フォームの入力値をstateとして持つ。
  formは現在の入力値、setFormは入力値を更新する関数。

name: ""
  店名の初期値。まだ入力されていないので空文字。

area: ""
  地域の初期値。

genre: ""
  ジャンルの初期値。

memo: ""
  メモの初期値。

imageUrl: ""
  画像URLの初期値。

status: "行きたい"
  ステータスの初期値。
```

入力欄が変わったら `setForm` でstateを更新します。

```jsx
function handleChange(event) {
  const { name, value } = event.target;
  setForm({ ...form, [name]: value });
}
```

1行ずつ読む:

```text
function handleChange(event)
  入力欄の値が変わったときに実行する関数。

const { name, value } = event.target;
  変更された入力欄のname属性と入力値を取り出す。

setForm({ ...form, [name]: value });
  既存のformをコピーし、変更された項目だけ新しい値に更新する。
  [name] と書くことで、name、area、genreなどを共通の処理で更新できる。
```

### 5. 登録ボタンで一覧に追加する

API接続前は、stateの配列に追加します。

```jsx
setRestaurants([
  ...restaurants,
  {
    id: Date.now(),
    ...form,
  },
]);
```

1行ずつ読む:

```text
setRestaurants(...)
  お店一覧のstateを更新する。
  stateが変わると、Reactが画面を再描画する。

[
  新しい配列を作っている。

...restaurants
  既存のお店一覧をそのまま入れる。

{
  追加する新しいお店データ。

id: Date.now()
  仮のIDを作る。
  API接続後はDBで作られたIDを使う。

...form
  フォームに入力された値を新しいお店データとして展開する。
```

ここで理解したいのは、登録ボタンを押すとstateが変わり、画面が更新されることです。

### 6. RestaurantFilterを作る

地域やジャンルの選択値をstateに持ち、表示する一覧を絞り込みます。

```text
selectedAreaが変わる
  ↓
表示するrestaurantsを絞り込む
  ↓
RestaurantListに渡す配列が変わる
  ↓
画面が更新される
```

## 演習

固定データのReact画面を完成させます。

最初に作るもの:

- お店カード
- お店一覧
- 登録フォーム
- 地域フィルタ

余裕があれば追加するもの:

- ジャンルフィルタ
- ステータスフィルタ
- 画像URLが空のときの代替表示
- 削除ボタン

演習中に説明できるようにすること:

- `App.jsx` が何を管理しているか
- `RestaurantList.jsx` と `RestaurantCard.jsx` の違い
- propsで何を渡しているか
- stateが変わると画面が変わる理由
- Tailwind CSSの `className` がどこに書かれているか

## AIへの依頼例

Day4では、React画面を固定データで作ります。
API接続はまだ入れず、コンポーネント分割、props、stateを確認します。

最初の依頼例:

```text
Reactでグルメ管理アプリのお店カードと一覧を作ってください。

作るファイル:
- frontend/src/components/RestaurantCard.jsx
- frontend/src/components/RestaurantList.jsx

条件:
- RestaurantCardはpropsとしてrestaurantを受け取ってください
- name, area, genre, memo, imageUrl, statusを表示してください
- imageUrlはimgタグのsrcに渡してください
- RestaurantListはrestaurants配列を受け取り、mapでRestaurantCardを表示してください
- Tailwind CSSで見やすいカードUIにしてください

作成後に、propsがどのように渡っているか説明してください。
```

フォームの依頼例:

```text
Reactでグルメ管理アプリのお店登録フォームを作ってください。

作るファイル:
frontend/src/components/RestaurantForm.jsx

入力項目:
name, area, genre, memo, imageUrl, status

条件:
- useStateでフォームの入力値を管理してください
- 入力値が変わったらstateを更新してください
- 送信時にonSubmit propsへフォームの値を渡してください
- API呼び出しはまだ書かないでください

作成後に、form stateがどのように更新されるか説明してください。
```

AIの回答を確認するときのポイント:

- API接続が勝手に入っていないか
- コンポーネントが大きくなりすぎていないか
- propsとstateの役割が分かれているか
- `className` にTailwind CSSの指定が書かれているか
- `img` の `src` に `restaurant.imageUrl` を渡しているか

## コードサンプル

お店1件を表示するコンポーネント。

```jsx
export function RestaurantCard({ restaurant }) {
  return (
    <article className="rounded-lg border bg-white p-4 shadow-sm">
      <img
        className="h-40 w-full rounded object-cover"
        src={restaurant.imageUrl}
        alt={restaurant.name}
      />
      <h3 className="mt-3 text-lg font-semibold">{restaurant.name}</h3>
      <p className="text-sm text-slate-600">
        {restaurant.area} / {restaurant.genre}
      </p>
      <p className="mt-2 text-sm">{restaurant.memo}</p>
      <span className="mt-3 inline-block rounded bg-slate-100 px-2 py-1 text-xs">
        {restaurant.status}
      </span>
    </article>
  );
}
```

見るポイント:

- `restaurant` はpropsとして受け取っている
- `imageUrl` を `img` の `src` に入れている
- `className` にTailwind CSSのクラスを書いて見た目を整えている
- このコンポーネントは1件分の表示だけを担当する

1行ずつ読む:

```text
export function RestaurantCard({ restaurant })
  お店1件分を表示するコンポーネント。
  restaurantは親からpropsとして受け取る。

return (...)
  画面に表示するJSXを返す。

<article className="...">
  お店カード全体の箱。
  classNameで枠線、背景、余白、影を指定している。

<img ... />
  お店画像を表示する。

className="h-40 w-full rounded object-cover"
  画像の高さ、幅、角丸、トリミング方法を指定している。

src={restaurant.imageUrl}
  画像URLをimgタグに渡している。

alt={restaurant.name}
  画像の代替テキスト。店名を入れている。

<h3>{restaurant.name}</h3>
  店名を表示する。

{restaurant.area} / {restaurant.genre}
  地域とジャンルを表示する。

{restaurant.memo}
  メモを表示する。

{restaurant.status}
  ステータスを表示する。
```

## 今日の確認ポイント

- コンポーネントを分ける理由を説明できる
- propsは親から子へ渡すデータだと説明できる
- stateは画面内で変わる値だと説明できる
- `useState` の基本的な読み方が分かる
- 画像URLを `img` タグで表示できる

## よくある混乱

### propsとstateはどちらもデータですか

どちらもデータだが、役割が違う。

propsは外から受け取るデータ。
stateはコンポーネントの中で変化するデータ。

### stateを直接書き換えていいですか

直接書き換えない。

Reactでは、stateを変更する関数を使う。

```jsx
setSelectedArea("新宿");
```

### 画像URLが間違っていたらどうなりますか

画像が表示されない。

APIやReactの処理が正しくても、URL先の画像が存在しなければ表示できない。
そのため、教材では動作確認済みのサンプル画像URLを用意する。

## メモ

最初からAPI接続まで入れると混乱しやすいので、React単体で画面の考え方をつかむ。
