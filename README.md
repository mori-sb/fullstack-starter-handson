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
- Spring BootのController / Service / Repository / Entityの役割を説明できる
- Reactで一覧・登録・編集画面を作れる
- Reactのコンポーネント / props / state / useEffectの役割を説明できる
- ReactからSpring Boot APIを呼び出せる
- 画像URLを使って画面に画像を表示できる
- AIが生成したコードを読み、どこを修正すればよいか判断できる
- GitHub上で教材とサンプルコードを管理できる

## ハンズオンの考え方

この教材では、コードをすべて手で書くことよりも、アプリの構造を理解することを重視します。

AIを使えば実装は速く進みますが、実務では次の力が必要になります。

- 生成されたコードがどの層のコードなのか分かる
- APIの入口、処理、DBアクセス、画面表示の流れを追える
- エラーが起きたときに、フロントエンド側かバックエンド側かを切り分けられる
- 仕様変更が入ったときに、どのファイルを直すべきか見当をつけられる

そのため、各Dayでは「作る」だけでなく「図で見る」「コードを読む」「少し変える」をセットにします。

## 進め方

| Day | テーマ | 重視する理解 | 成果物 |
| --- | --- | --- | --- |
| Day 1 | Webアプリの全体像、HTTP、JSON、REST API | フロントエンドとバックエンドの境界 | グルメ管理アプリの画面・データ・API設計 |
| Day 2 | Spring Bootの基本、Controller / Service / Repository | リクエストがバックエンド内をどう流れるか | お店一覧API |
| Day 3 | DB接続、Entity、Repository、CRUD API | JavaオブジェクトとDBテーブルの関係 | お店の登録・詳細・編集・削除API |
| Day 4 | Reactの基本、コンポーネント、State、Effect | データが変わると画面が変わる仕組み | 画像付きのお店一覧画面と登録フォーム |
| Day 5 | ReactとAPI連携、CORS、エラー処理 | 画面操作がAPI呼び出しにつながる流れ | 登録・一覧・編集・削除が動くミニアプリ |

## 図で説明する概念

- ブラウザ、React、Spring Boot、DBの全体構成
- HTTPリクエストとHTTPレスポンス
- JSONがAPIと画面の間を流れる様子
- Spring BootのController / Service / Repository / Entity
- DBテーブルとEntityの対応
- CRUDとHTTPメソッドの対応
- Reactのコンポーネント分割
- propsとstateの違い
- useEffectでAPIを呼ぶ流れ
- 画像URLがDB、API、Reactを通って画像表示される流れ

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

## 主要ドキュメント

- [Beginner Guide](docs/beginner-guide.md)
- [Day 1: Webアプリの全体像](docs/day1.md)
- [Day 2: Spring Bootの基本](docs/day2.md)
- [Day 3: DB接続とCRUD API](docs/day3.md)
- [Day 4: Reactの基本](docs/day4.md)
- [Day 5: ReactとAPI連携](docs/day5.md)
- [Glossary](docs/glossary.md)
- [Checklists](docs/checklists.md)
- [Teaching Policy](docs/teaching-policy.md)
- [Beginner-Friendly Design](docs/beginner-friendly-design.md)
- [Visual Learning Plan](docs/visual-learning-plan.md)
- [Visual Map](docs/visual-map.md)
- [Code Sample Plan](docs/code-sample-plan.md)
- [Instructor Notes](docs/instructor-notes.md)

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
