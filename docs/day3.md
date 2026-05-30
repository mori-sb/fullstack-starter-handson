# Day 3: DB接続とCRUD API

## 今日のゴール

- Entityの役割を理解する
- Repositoryを使ってDBにアクセスできる
- CRUD APIを作れる
- APIの正常系と簡単な異常系を確認できる
- 地域・ジャンル・ステータスで絞り込む考え方を理解する

## 扱う内容

- Entity
- Repository
- CRUD
- バリデーション
- エラーレスポンス
- クエリパラメータ

## 入れたい図

- CRUDとHTTPメソッドの対応図
- DBテーブルとEntityの対応図
- クエリパラメータで一覧を絞り込む流れ

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

## メモ

DBは最初はH2を使うと環境差分が少ない。
