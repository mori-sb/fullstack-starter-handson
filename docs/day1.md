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

## メモ

最初は用語を増やしすぎず、「画面」「処理」「データの保存場所」に分けて説明する。
