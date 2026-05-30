# Day 1: Webアプリの全体像

## 今日のゴール

- ブラウザ、React、API、Spring Boot、DBの関係を理解する
- HTTPリクエストとHTTPレスポンスの流れを説明できる
- JSONがどこで使われるかを理解する
- REST APIの基本を知る
- グルメ管理アプリの画面、データ、APIをざっくり設計できる
- AIで実装する前に、何を作るのかを言葉と図で説明できる

## 扱う内容

- Webアプリケーションの全体像
- フロントエンドとバックエンドの役割
- 今回作るグルメ管理アプリの完成イメージ
- HTTPメソッド
- URLとエンドポイント
- JSON
- REST API
- 画像URLを使った画像表示の考え方

## 今日の大事な考え方

AIを使うとコードはすぐに作れるが、次のことが分からないと実務では詰まりやすい。

- 画面の処理なのか、APIの処理なのか
- データはどこから来て、どこに保存されるのか
- ボタンを押したときに、どのAPIが呼ばれるのか
- APIが返したJSONをReactがどう表示するのか

Day 1では、コードを書き始める前にこの地図を作る。

## 最初に伝えること

Webアプリ開発では、いきなりコードを書き始めるより先に「役割分担」を理解することが大事。

今回のアプリは、画面、API、DBが分かれている。

```text
React        ユーザーが見る画面
Spring Boot  画面から呼ばれるAPI
DB           お店データを保存する場所
```

この3つは別々の役割を持っている。
初心者が混乱しやすいのは、画面の問題なのか、APIの問題なのか、DBの問題なのかが分からなくなること。

Day 1では、まず「どこで何が起きているか」を見分ける力をつける。

## 用語の整理

### フロントエンド

ユーザーが直接見る部分。

今回でいうとReactで作る画面。
ボタン、入力フォーム、お店カード、フィルタなどがここに入る。

```text
ユーザーが操作する場所 = フロントエンド
```

### バックエンド

画面から呼ばれて、データの登録・取得・更新・削除を行う部分。

今回でいうとSpring Bootで作るAPI。
Reactから送られてきたリクエストを受け取り、必要に応じてDBへアクセスする。

```text
画面の裏側で処理する場所 = バックエンド
```

### API

フロントエンドとバックエンドの窓口。

ReactはDBを直接触らない。
ReactはSpring BootのAPIを呼び、Spring BootがDBとやり取りする。

```text
React -> API -> Spring Boot -> DB
```

### JSON

フロントエンドとバックエンドの間でデータをやり取りする形式。

JavaScriptのオブジェクトに似た見た目をしている。
ReactもSpring Bootも、このJSONを使ってデータを受け渡しする。

```json
{
  "name": "Cafe Sakura",
  "area": "新宿",
  "genre": "カフェ"
}
```

### REST API

URLとHTTPメソッドを使って、データに対する操作を表すAPIの考え方。

```text
GET     データを見る
POST    データを作る
PUT     データを更新する
DELETE  データを削除する
```

まずはこの4つだけ分かれば十分。

## 作るアプリ

行きたいお店や行ったお店を登録し、地域・ジャンル・ステータスで探しやすくするグルメ管理アプリを作る。

最初に扱う項目:

- 店名
- 地域
- ジャンル
- メモ
- 画像URL
- ステータス

ステータス:

- 行きたい
- 行った
- お気に入り

画像はファイルをアップロードせず、画像URLを文字列として保存する。

```text
DBに画像URLを保存する
APIで画像URLを返す
Reactが <img> で画像を表示する
```

## 画面の完成イメージ

```text
[地域: すべて] [ジャンル: すべて] [ステータス: すべて]

+-------------------------------+
| 画像                          |
| Cafe Sakura                   |
| 新宿 / カフェ / 行きたい       |
| 落ち着いて作業できそう         |
+-------------------------------+

+-------------------------------+
| 画像                          |
| Ginza Kitchen                 |
| 銀座 / 洋食 / お気に入り       |
| ランチがよかった               |
+-------------------------------+
```

## データの完成イメージ

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

## APIの完成イメージ

```text
GET    /api/restaurants
GET    /api/restaurants/{id}
POST   /api/restaurants
PUT    /api/restaurants/{id}
DELETE /api/restaurants/{id}
```

フィルタ:

```text
GET /api/restaurants?area=新宿
GET /api/restaurants?genre=カフェ
GET /api/restaurants?status=WANT_TO_GO
```

## API設計の考え方

画面でやりたい操作からAPIを考える。

```text
お店一覧を見たい
  -> GET /api/restaurants

お店を1件詳しく見たい
  -> GET /api/restaurants/{id}

お店を登録したい
  -> POST /api/restaurants

お店を編集したい
  -> PUT /api/restaurants/{id}

お店を削除したい
  -> DELETE /api/restaurants/{id}
```

初心者はURLを暗記する必要はない。
大事なのは「画面操作」と「API」が対応していることを理解すること。

## 画像URL方式の考え方

今回、画像アップロードは扱わない。

画像アップロードを入れると、ファイル保存、容量制限、形式チェック、保存先、セキュリティなど考えることが急に増える。

そのため今回は、画像そのものではなく画像URLを保存する。

```text
保存するもの:
https://example.com/cafe.jpg

保存しないもの:
画像ファイル本体
```

ReactはこのURLを使って画像を表示する。

```jsx
<img src={restaurant.imageUrl} alt={restaurant.name} />
```

この方式でも、一覧画面に画像付きカードを表示できるため、アプリらしさは十分出せる。

## 入れたい図

- ブラウザ -> React -> API -> Spring Boot -> DB の全体図
- HTTPリクエスト / レスポンスの往復図
- JSONデータが画面に表示されるまでの流れ
- 画像URLが画面に表示されるまでの流れ

## 図の説明メモ

全体図:

```text
ユーザー
  ↓ 操作する
ブラウザ
  ↓ 画面を表示する
React
  ↓ HTTPでAPIを呼ぶ
Spring Boot
  ↓ 必要に応じてDBへアクセスする
DB
```

画像URLの流れ:

```text
DB: image_url = "https://example.com/cafe.jpg"
  ↓
Spring Boot API: imageUrlとしてJSONに入れて返す
  ↓
React: restaurant.imageUrlを受け取る
  ↓
imgタグ: <img src={restaurant.imageUrl}>
  ↓
ブラウザ: 画像を表示する
```

## ハンズオン

まだコードを書き始めず、完成するアプリの画面、データ、APIを設計する。

やること:

- 画面に表示したい項目を決める
- 登録フォームに必要な項目を決める
- APIの一覧を確認する
- JSONの形を読む
- フロントエンドとバックエンドの境界を確認する
- AIに実装を依頼するとしたら、どんな指示を出すか考える

## 今日の確認ポイント

- ReactはDBを直接触らない、と説明できる
- Spring BootはAPIを受け取り、必要に応じてDBへアクセスする、と説明できる
- JSONがReactとSpring Bootの間を流れるデータ形式だと説明できる
- `GET`、`POST`、`PUT`、`DELETE` の大まかな意味を説明できる
- 画像URL方式で画像が表示される流れを説明できる

## よくある混乱

### Reactとブラウザは同じものですか

同じではない。

ブラウザはWebページを表示するアプリ。
Reactはブラウザ上で動く画面を作るためのライブラリ。

### APIとSpring Bootは同じものですか

同じではない。

Spring Bootはバックエンドを作るためのフレームワーク。
APIはReactから呼ばれる入口。
Spring Bootを使ってAPIを作る、という関係。

### JSONはDBですか

DBではない。

JSONはデータの受け渡し形式。
DBはデータを保存する場所。

## メモ

最初は用語を増やしすぎず、「画面」「処理」「データの保存場所」に分けて説明する。
