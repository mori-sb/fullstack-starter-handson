# fullstack-starter-handson

フルスタックWebアプリ開発のハンズオン教材です。

Spring Boot と React を使って、Webアプリケーションの全体像、API、DB、画面実装、フロントエンドとバックエンドの連携を1週間で学びます。

## ゴール

- Webアプリケーションの全体像を説明できる
- HTTP / JSON / REST API の基本を理解する
- Spring BootでCRUD APIを作れる
- Reactで一覧・登録画面を作れる
- ReactからSpring Boot APIを呼び出せる
- GitHub上で教材とサンプルコードを管理できる

## 進め方

| Day | テーマ | 成果物 |
| --- | --- | --- |
| Day 1 | Webアプリの全体像、HTTP、JSON、REST API | 画面とAPIの設計メモ |
| Day 2 | Spring Bootの基本、Controller / Service / Repository | 最初のGET API |
| Day 3 | DB接続、Entity、Repository、CRUD API | 登録・一覧・更新・削除API |
| Day 4 | Reactの基本、コンポーネント、State、Effect | 一覧画面と登録フォーム |
| Day 5 | ReactとAPI連携、CORS、エラー処理 | フルスタックのミニアプリ |

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
