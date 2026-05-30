# fullstack-starter-handson

フルスタックWebアプリ開発のハンズオン教材です。

Spring Boot と React を使って、グルメ管理アプリを作りながら、Webアプリケーションの全体像、API、DB、画面実装、フロントエンドとバックエンドの連携を1週間で学びます。

## 作るアプリ

行きたいお店や行ったお店を登録し、地域・ジャンル・ステータスで探しやすくするグルメ管理アプリを作ります。

扱う主な項目:

- 店名
- 地域
- ジャンル
- メモ
- 画像URL
- ステータス

画像はファイルアップロードではなく、画像URLを登録してReactで表示します。

## ゴール

- Webアプリケーションの全体像を説明できる
- HTTP / JSON / REST API の基本を理解する
- Spring BootでCRUD APIを作れる
- Reactで一覧・登録・編集画面を作れる
- ReactからSpring Boot APIを呼び出せる
- 画像URLを使って画面に画像を表示できる
- GitHub上で教材とサンプルコードを管理できる

## 進め方

| Day | テーマ | 成果物 |
| --- | --- | --- |
| Day 1 | Webアプリの全体像、HTTP、JSON、REST API | グルメ管理アプリの画面・データ・API設計 |
| Day 2 | Spring Bootの基本、Controller / Service / Repository | お店一覧API |
| Day 3 | DB接続、Entity、Repository、CRUD API | お店の登録・詳細・編集・削除API |
| Day 4 | Reactの基本、コンポーネント、State、Effect | 画像付きのお店一覧画面と登録フォーム |
| Day 5 | ReactとAPI連携、CORS、エラー処理 | 登録・一覧・編集・削除が動くミニアプリ |

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

ステータス:

```text
WANT_TO_GO  行きたい
VISITED     行った
FAVORITE    お気に入り
```

## ディレクトリ構成

```text
.
├─ README.md
├─ docs/       # ハンズオン資料
├─ images/     # 概念図・スクリーンショット
├─ backend/    # Spring Boot
└─ frontend/   # React
```

## ブランチ運用

```text
main      安定版・共有用
develop   教材作成中の最新版
feature/* 個別作業用
```

まずは `develop` に教材とサンプルコードを追加していきます。

## 教材作成メモ

- 概念説明は `docs/` に書く
- 図やスクリーンショットは `images/` に置く
- Spring Bootのコードは `backend/` に置く
- Reactのコードは `frontend/` に置く
- 各Dayの完成状態はブランチまたはタグで残す
- 画像アップロードは扱わず、画像URLを保存して表示する
