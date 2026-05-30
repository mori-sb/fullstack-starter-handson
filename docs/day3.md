# Day 3: DB接続とCRUD API

## 今日のゴール

- Entityの役割を理解する
- Repositoryを使ってDBにアクセスできる
- CRUD APIを作れる
- APIの正常系と簡単な異常系を確認できる
- 地域・ジャンル・ステータスで絞り込む考え方を理解する
- 仕様変更時にどの層を直すか判断できる

## 扱う内容

- Entity
- Repository
- CRUD
- バリデーション
- エラーレスポンス
- クエリパラメータ

## 今日の大事な考え方

CRUDは多くの業務アプリの基本になる。

```text
Create  登録する
Read    一覧・詳細を見る
Update  編集する
Delete  削除する
```

この4つを一度作ると、申請管理、台帳管理、レビュー管理など多くのアプリに応用できる。

## 最初に伝えること

Day2では固定データを返した。
Day3では、データをDBに保存する。

DBを使うと、アプリを再起動してもデータを残せる。
登録、編集、削除の結果が保存されるため、アプリらしくなる。

```text
固定データ:
コードに書いてあるだけのデータ

DB保存:
アプリの外に保存され、後から取得できるデータ
```

## Entityとは

Entityは、DBに保存するデータの形をJavaで表したもの。

![Spring BootにおけるEntityの位置づけ](../images/spring-entity-flow.png)

グルメ管理アプリでは、`Restaurant` Entityを作る。

```text
Restaurant Entity
  id
  name
  area
  genre
  memo
  imageUrl
  status
```

DBのテーブルに近い考え方。

```text
JavaのEntity  <->  DBのテーブル
Restaurant    <->  restaurants
```

## Repositoryとは

Repositoryは、EntityをDBに保存したり、DBから取得したりするための入口。

Spring Data JPAを使うと、次のような操作を自分で細かくSQLを書かなくても使える。

```text
save       登録・更新
findAll    一覧取得
findById   詳細取得
delete     削除
```

まずは、Repositoryは「DB操作をまとめたもの」と理解すればよい。

## Mapperとは

Mapperは、DTOとEntityを変換する役割です。

![DTO、Mapper、Entityの関係](../images/spring-mapper-flow.png)

ReactとAPIの間ではDTOを使い、DBに保存するときはEntityを使います。
この2つは目的が違うため、変換する場所が必要になります。

```text
Request DTO -> Mapper -> Entity
Entity -> Mapper -> Response DTO
```

最初は「MapperはDTOとEntityの変換係」と理解すれば十分です。

## CRUDとHTTPメソッド

画面の操作、HTTPメソッド、APIは対応している。

```text
お店を登録する
  -> POST /api/restaurants

お店一覧を見る
  -> GET /api/restaurants

お店詳細を見る
  -> GET /api/restaurants/{id}

お店を編集する
  -> PUT /api/restaurants/{id}

お店を削除する
  -> DELETE /api/restaurants/{id}
```

この対応が分かると、React側でどのAPIを呼べばよいか判断しやすくなる。

## 使用する図

- CRUDとHTTPメソッドの対応図
- DBテーブルとEntityの対応図
- クエリパラメータで一覧を絞り込む流れ

## 図の説明メモ

```text
POST   /api/restaurants       登録
GET    /api/restaurants       一覧
GET    /api/restaurants/{id}  詳細
PUT    /api/restaurants/{id}  編集
DELETE /api/restaurants/{id}  削除
```

## ハンズオン

登録、一覧、詳細、更新、削除のAPIを作成する。

```text
GET    /api/restaurants
GET    /api/restaurants/{id}
POST   /api/restaurants
PUT    /api/restaurants/{id}
DELETE /api/restaurants/{id}
```

余裕があれば、一覧APIにフィルタを追加する。

```text
GET /api/restaurants?area=新宿
GET /api/restaurants?genre=カフェ
GET /api/restaurants?status=WANT_TO_GO
```

## リクエストとレスポンスの例

登録APIでは、ReactからSpring BootへJSONを送る。

```http
POST /api/restaurants
Content-Type: application/json
```

```json
{
  "name": "Cafe Sakura",
  "area": "新宿",
  "genre": "カフェ",
  "memo": "落ち着いて作業できそう",
  "imageUrl": "https://example.com/cafe.jpg",
  "status": "WANT_TO_GO"
}
```

Spring BootはDBに保存し、保存結果をJSONで返す。

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

ポイントは、登録前のリクエストには `id` がなく、保存後のレスポンスには `id` があること。
`id` はDBに保存されたデータを区別するために使う。

## バリデーションの考え方

入力値が空のまま登録されると、使いにくいアプリになる。

最低限、次の項目は必須にする。

- 店名
- 地域
- ジャンル
- ステータス

メモと画像URLは任意でもよい。

最初から厳密に作り込みすぎない。
まずは「必須項目が空ならエラーにする」程度で十分。

## エラーレスポンスの考え方

APIは成功するときだけでなく、失敗するときもある。

例:

```text
存在しないIDを指定した
必須項目が空だった
URLが間違っていた
```

最初は次の違いを理解する。

```text
200 OK       成功
201 Created  登録成功
400 Bad Request  入力が間違っている
404 Not Found    データが見つからない
500 Internal Server Error  サーバー側の想定外エラー
```

## 今日の確認ポイント

- EntityはDBに保存するデータの形だと説明できる
- RepositoryはDB操作を担当すると説明できる
- CRUDとHTTPメソッドの対応を説明できる
- 登録前のJSONと保存後のJSONの違いを説明できる
- API単体で登録、一覧、詳細、編集、削除を確認できる

## よくある混乱

### EntityとDTOは同じですか

同じではない。

EntityはDBに保存する形。
DTOはAPIで受け渡しする形。

最初は似ていても、目的が違う。

### PUTとPOSTの違いは何ですか

POSTは新しく作る。
PUTは既にあるものを更新する。

```text
POST /api/restaurants
  -> 新しいお店を作る

PUT /api/restaurants/1
  -> id=1のお店を更新する
```

### フィルタは別APIにするべきですか

今回は一覧APIにクエリパラメータを付ける。

```text
GET /api/restaurants?area=新宿
```

一覧の条件違いなので、同じURLに条件を足す考え方でよい。

## メモ

DBは最初はH2を使うと環境差分が少ない。
