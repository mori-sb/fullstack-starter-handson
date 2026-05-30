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

## propsとstate

Reactで初心者が混乱しやすいのが、propsとstate。

### props

親コンポーネントから子コンポーネントへ渡すデータ。

```text
App
  ↓ restaurantを渡す
RestaurantCard
```

子コンポーネントは、受け取ったpropsを使って表示する。

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

## 入れたい図

- Stateが変わると画面が更新される流れ
- 親コンポーネントと子コンポーネントの関係
- APIから受け取った画像URLをimgタグで表示する流れ

## 図の説明メモ

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
    <article className="restaurant-card">
      <img src={restaurant.imageUrl} alt={restaurant.name} />
      <h3>{restaurant.name}</h3>
      <p>{restaurant.area} / {restaurant.genre}</p>
      <p>{restaurant.memo}</p>
      <span>{restaurant.status}</span>
    </article>
  );
}
```

見るポイント:

- `restaurant` はpropsとして受け取っている
- `imageUrl` を `img` の `src` に入れている
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
