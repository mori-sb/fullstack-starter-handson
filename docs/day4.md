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

## メモ

最初からAPI接続まで入れると混乱しやすいので、React単体で画面の考え方をつかむ。
