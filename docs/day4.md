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
