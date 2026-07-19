# Exercise Answers

このファイルは、演習の模範解答とレビュー観点をまとめたものです。

参加者にそのまま先に配る資料ではなく、説明者が確認したり、AIに採点・コメントを依頼したりするために使います。

## AI採点で使う共通プロンプト

参加者の回答やコードをAIに見てもらうときは、次のプロンプトを使います。

```text
あなたはSpring BootとReactのハンズオン教材のレビュー担当です。
以下の模範解答と採点観点を基準に、参加者の回答をレビューしてください。

レビューでは次の順番でコメントしてください。

1. できている点
2. 不足している点
3. 直すべきファイル
4. 具体的な修正方針
5. 次に確認する動作

厳しすぎる表現は避け、参加者が次に何をすればよいか分かるコメントにしてください。
コードを全面的に書き直すのではなく、まずは現在の実装を読んだ上で必要な修正だけ提案してください。
```

## 共通採点観点

すべての演習で、次の観点を確認します。

- 今日のゴールに関係するファイルを触っている
- 画面、API、DBの責務が混ざっていない
- 不要なファイルや技術を増やしていない
- 仕様にある項目名が揃っている
- 動作確認の方法を説明できる
- エラー時に確認する場所を説明できる

## Day 1: 設計演習

### 演習内容

画面操作、データ項目、APIを整理する。

### 模範解答

画面操作とAPI:

```text
お店一覧を見る      GET    /api/restaurants
お店を登録する      POST   /api/restaurants
お店を編集する      PUT    /api/restaurants/{id}
お店を削除する      DELETE /api/restaurants/{id}
地域で絞り込む      GET    /api/restaurants?area=新宿
ジャンルで絞り込む  GET    /api/restaurants?genre=カフェ
```

データ項目:

```text
id        お店を区別するID
name      店名
area      地域
genre     ジャンル
memo      メモ
imageUrl  画像URL
status    行きたい、行った、お気に入り
```

JSON例:

```json
{
  "id": 1,
  "name": "Cafe Sakura",
  "area": "新宿",
  "genre": "カフェ",
  "memo": "落ち着いて作業できそう",
  "imageUrl": "https://example.com/cafe.jpg",
  "status": "WANT_TO_GO"
}
```

### 採点観点

- React、Spring Boot、DBの役割を分けて説明できている
- ReactがDBを直接操作する説明になっていない
- 画面操作とAPIが対応している
- `imageUrl` を画像ファイル本体ではなくURLとして扱っている

## Day 2: 固定データ一覧API

### 演習内容

固定データを3件返す一覧APIを作る。

### 模範解答

作るファイル:

```text
controller/RestaurantController.java
service/RestaurantService.java
dto/restaurant/RestaurantResponse.java
```

API:

```text
GET /api/restaurants
```

責務:

```text
RestaurantController  GET /api/restaurants を受け取り、Serviceを呼ぶ
RestaurantService     固定のお店データを作って返す
RestaurantResponse    Reactへ返すJSONの形を表す
```

レスポンス例:

```json
[
  {
    "id": 1,
    "name": "Cafe Sakura",
    "area": "新宿",
    "genre": "カフェ",
    "memo": "落ち着いて作業できそう",
    "imageUrl": "https://example.com/cafe.jpg",
    "status": "WANT_TO_GO"
  },
  {
    "id": 2,
    "name": "Ginza Kitchen",
    "area": "銀座",
    "genre": "洋食",
    "memo": "ランチがよかった",
    "imageUrl": "https://example.com/ginza.jpg",
    "status": "VISITED"
  },
  {
    "id": 3,
    "name": "Shibuya Coffee",
    "area": "渋谷",
    "genre": "カフェ",
    "memo": "作業しやすそう",
    "imageUrl": "https://example.com/shibuya.jpg",
    "status": "FAVORITE"
  }
]
```

### 採点観点

- ControllerがURLとHTTPメソッドを担当している
- Serviceにデータ作成処理がある
- Response DTOに必要な項目が揃っている
- APIを呼ぶとJSON配列が返る

## Day 3: CRUD API

### 演習内容

DB保存を使ったCRUD APIを作る。

### 模範解答

作るファイル:

```text
entity/Restaurant.java
repository/RestaurantRepository.java
dto/restaurant/RestaurantRequest.java
dto/restaurant/RestaurantResponse.java
mapper/RestaurantMapper.java
service/RestaurantService.java
controller/RestaurantController.java
```

API:

```text
GET    /api/restaurants
GET    /api/restaurants/{id}
POST   /api/restaurants
PUT    /api/restaurants/{id}
DELETE /api/restaurants/{id}
```

DTO、Entity、Mapperの役割:

```text
RestaurantRequest   Reactから受け取るJSONの形
Restaurant           DBに保存するEntity
RestaurantResponse  Reactへ返すJSONの形
RestaurantMapper    Request/Entity/Responseを変換する
```

登録の流れ:

```text
POST /api/restaurants
  -> Controller
  -> Service
  -> MapperでRequest DTOをEntityへ変換
  -> Repository.save
  -> MapperでEntityをResponse DTOへ変換
  -> JSONレスポンス
```

### 採点観点

- ControllerからRepositoryを直接呼んでいない
- Serviceが処理の中心になっている
- EntityとDTOを分けている
- Mapperで変換している
- 存在しないIDの扱いを考えている
- 登録後のレスポンスに `id` が含まれている

## Day 4: React画面

### 演習内容

固定データでReact画面を作る。

### 模範解答

作るファイル:

```text
App.jsx
components/RestaurantCard.jsx
components/RestaurantList.jsx
components/RestaurantForm.jsx
components/RestaurantFilter.jsx
```

コンポーネントの役割:

```text
App                 restaurants state、form送信、フィルタ条件を管理する
RestaurantCard      お店1件を表示する
RestaurantList      お店カードを一覧表示する
RestaurantForm      お店登録フォームを表示する
RestaurantFilter    地域、ジャンル、ステータスの絞り込み条件を選ぶ
```

propsの流れ:

```text
App
  -> RestaurantList に restaurants を渡す
  -> RestaurantList から RestaurantCard に restaurant を1件ずつ渡す
  -> RestaurantForm に onSubmit を渡す
  -> RestaurantFilter に selectedArea や onChange を渡す
```

画像表示:

```jsx
<img src={restaurant.imageUrl} alt={restaurant.name} />
```

### 採点観点

- コンポーネントが役割ごとに分かれている
- propsで親から子へデータを渡している
- stateで入力値や一覧を管理している
- `imageUrl` を `img` の `src` に渡している
- Tailwind CSSの指定が `className` に書かれている
- API接続をDay4で先に入れすぎていない

## Day 5: ReactとAPI連携

### 演習内容

React画面とSpring Boot APIを接続する。
Day5の演習対象は、一覧取得と登録に絞る。

### 模範解答

API関数:

```text
fetchRestaurants   GET    /api/restaurants
createRestaurant   POST   /api/restaurants
```

一覧取得の流れ:

```text
App.jsx
  -> useEffect
  -> fetchRestaurants
  -> GET /api/restaurants
  -> JSONを受け取る
  -> setRestaurants
  -> RestaurantListへ渡す
```

登録の流れ:

```text
RestaurantForm
  -> onSubmit
  -> createRestaurant
  -> POST /api/restaurants
  -> 登録後に一覧を再取得、またはstateへ追加
```

### 採点観点

- API呼び出し関数を `api/restaurants.js` にまとめている
- Reactコンポーネント内にURLが散らばっていない
- `fetch` のHTTPメソッドが正しい
- JSON送信時に `Content-Type` と `JSON.stringify` を使っている
- API結果をstateに反映している
- Networkタブでリクエストを確認できる
- CORSエラー時にSpring Boot側の設定を確認できる
- 編集、削除、フィルタなど、Day5で説明していない処理を勝手に増やしていない

## AIコメント例

### 良いコメント例

```text
Controller、Service、Repositoryの役割分担はできています。
次に確認するとよいのは、存在しないIDを指定したときの動きです。
PUT /api/restaurants/999 を呼んだときに404として扱えるか確認してください。
```

### 避けたいコメント例

```text
全部違います。作り直してください。
```

### 修正方針を含むコメント例

```text
RestaurantControllerからRepositoryを直接呼んでいるため、責務が混ざっています。
ControllerはRestaurantServiceを呼ぶだけにし、DB操作はServiceからRepositoryを呼ぶ形に直してください。
修正後はGET /api/restaurants と POST /api/restaurants の動作を確認してください。
```
