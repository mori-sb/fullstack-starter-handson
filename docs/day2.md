# Day 2: Spring Bootの基本

## 今日のゴール

- Spring Bootプロジェクトの構成を理解する
- Controllerの役割を理解する
- Serviceの役割を理解する
- Repositoryの役割を理解する
- お店一覧APIを作ってJSONで返せる

## 扱う内容

- Spring Bootのプロジェクト構成
- Controller / Service / Repository
- DTO
- APIの動作確認
- 画像URLを含むレスポンス

## 入れたい図

- Controller / Service / Repository の役割図
- リクエストがControllerに届いてレスポンスが返るまでの流れ
- お店データがJSONとして返る流れ

## ハンズオン

固定のお店データを返す一覧APIを作成する。

```text
GET /api/restaurants
```

返すJSONの例:

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
  }
]
```

## メモ

Springの用語が多くなるので、最初は役割を日常的な言葉に置き換えて説明する。
