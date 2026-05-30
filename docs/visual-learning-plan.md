# Visual Learning Plan

この教材では、画像とコードサンプルをセットで使い、初心者がSpring BootとReactの構造を理解できるようにする。

AIを使うと実装は速く進むため、参加者には「生成されたコードがどの役割なのか」「画面操作からDB更新まで何が起きるのか」を説明できる状態を目指してもらう。

## 画像の使い方

各概念は次のセットで説明する。

```text
1. 図で全体像を見る
2. 役割を日本語で理解する
3. 対応するコードを見る
4. どこを変更すると何が変わるか確認する
```

## 作成済み画像

- Webアプリ全体構成図
- HTTPリクエスト / レスポンスの流れ

## 追加で欲しい画像

### 1. Spring Boot内部の全体構造

目的:

Spring Bootの中にController、Service、Repository、Entity、DTO、Mapperがあり、それぞれ役割が違うことを理解する。

生成依頼文:

```text
モダンなSaaS技術資料風に、Spring Bootバックエンド内部の構造図を作ってください。

左から「HTTP Request」が入り、中央にSpring Bootアプリケーションの大きな枠を置いてください。
その中に上から順番に、Controller、Service、Repository、Entity、Databaseを配置してください。
Controllerの横に「APIの入口」、Serviceの横に「処理の中心」、Repositoryの横に「DBアクセス」、Entityの横に「DBに保存するデータの形」と日本語で説明を入れてください。

さらにControllerとServiceの間、またはControllerの近くにDTOを配置し、「APIで受け渡しするデータの形」と説明してください。
ServiceとDTOまたはEntityの間にMapperを配置し、「DTOとEntityを変換する」と説明してください。

右側には「JSON Response」が返る流れを矢印で表してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、ネイビー・グリーン・シアンを使ったモダンで読みやすい図にしてください。
```

### 2. DTOとEntityの違い

目的:

APIで使うデータの形と、DBに保存するデータの形を分ける理由を理解する。

生成依頼文:

```text
初心者向けに、DTOとEntityの違いを説明するモダンな技術図を作ってください。

左に「RestaurantRequest DTO」、中央に「Mapper」、右に「Restaurant Entity」を配置してください。

RestaurantRequest DTOには、name、area、genre、memo、imageUrl、statusを表示してください。
Restaurant Entityには、id、name、area、genre、memo、imageUrl、status、createdAt、updatedAtを表示してください。

DTOには「APIで受け取る・返すデータ」、Entityには「DBに保存するデータ」と日本語で説明を入れてください。
Mapperには「DTOとEntityを変換する」と表示してください。

下部に「画面/API用の形とDB用の形を分けると、変更に強くなる」という補足を入れてください。
白背景、角丸カード、細い線、控えめな影を使い、SaaSドキュメント風の洗練された図にしてください。
```

### 3. 登録APIの処理の流れ

目的:

Reactで登録ボタンを押してから、DBに保存されるまでの流れを理解する。

生成依頼文:

```text
グルメ管理アプリの「お店登録API」の処理の流れを説明する図を作ってください。

左から右に、React Form、POST /api/restaurants、Controller、Service、Mapper、Repository、Databaseを並べてください。

矢印には次のラベルを付けてください。
React Form -> Controller: JSONを送信
Controller -> Service: DTOを渡す
Service -> Mapper: DTOをEntityに変換
Service -> Repository: 保存を依頼
Repository -> Database: INSERT
Database -> React: 保存結果を返す

各要素は角丸カードにし、Reactはシアン、Spring Boot関連はグリーン、DBはパープル系で色分けしてください。
初心者が「登録ボタンを押すと裏側で何が起きるか」を理解できるモダンな教材図にしてください。
```

### 4. 一覧取得APIの処理の流れ

目的:

画面表示時にReactがAPIを呼び、DBから取ったデータをカード一覧に表示する流れを理解する。

生成依頼文:

```text
グルメ管理アプリの「お店一覧取得API」の処理の流れを説明する図を作ってください。

左から右に、React useEffect、GET /api/restaurants、Controller、Service、Repository、Databaseを並べてください。
戻りの流れとして、DatabaseからRepository、Service、Controller、ReactへJSONが返る矢印も描いてください。

最後にReact側でRestaurantCardが複数表示される小さなカードUIを描いてください。

「画面を開く」「useEffectが動く」「APIを呼ぶ」「DBから取得」「JSONを受け取る」「stateを更新」「カード一覧を表示」という流れが分かるようにしてください。
白または薄いグレー背景、角丸カード、細い線、モダンなSaaS資料風でお願いします。
```

### 5. Reactコンポーネントとデータの流れ

目的:

App、Filter、Form、List、Cardの関係と、props/stateの違いを理解する。

生成依頼文:

```text
Reactのコンポーネント構成とデータの流れを説明する図を作ってください。

一番上にAppコンポーネントを置き、Appの中にstateとして restaurants、selectedArea、selectedGenre、selectedStatus があることを表示してください。

下にRestaurantFilter、RestaurantForm、RestaurantListを並べてください。
RestaurantListの下にRestaurantCardを複数配置してください。

Appから子コンポーネントへpropsが渡る矢印を描いてください。
RestaurantFilterからAppへ「フィルタ変更」、RestaurantFormからAppへ「登録」、RestaurantCardからAppへ「編集・削除」のイベントが戻る矢印を描いてください。

propsは青、stateはオレンジ、イベントはグリーンで色分けしてください。
モダンでおしゃれなSaaSドキュメント風にしてください。
```

### 6. useStateとuseEffectの役割

目的:

React初心者が混乱しやすいstateとEffectを分けて理解する。

生成依頼文:

```text
ReactのuseStateとuseEffectの役割を初心者向けに説明する図を作ってください。

左側にuseStateのエリアを作り、「画面で変わる値を覚える」と説明してください。
例として restaurants、form、selectedArea を表示してください。

右側にuseEffectのエリアを作り、「画面表示時や値の変化時に処理を実行する」と説明してください。
例として「画面表示時にGET /api/restaurantsを呼ぶ」を表示してください。

中央にReact画面の小さなプレビューを置き、stateが変わると画面が更新される流れを矢印で表してください。
白背景、角丸カード、シアン・オレンジ・グリーンのアクセントカラーで、モダンな教材図にしてください。
```

### 7. 開発の進め方

目的:

初心者が「何から作ればよいか」「どの順番で確認するか」を理解する。

生成依頼文:

```text
初心者向けに、フルスタックWebアプリ開発の進め方を説明するロードマップ図を作ってください。

横方向のステップで次の順番を表示してください。
1. 画面を決める
2. データ項目を決める
3. APIを決める
4. Spring BootでAPIを作る
5. APIを単体で確認する
6. Reactで画面を作る
7. ReactからAPIを呼ぶ
8. 動作確認して修正する

各ステップを角丸カードで表示し、フロントエンド、バックエンド、確認作業が色で分かるようにしてください。
モダンなSaaSプロダクト資料風、余白多め、読みやすい日本語ラベルでお願いします。
```

### 8. エラー切り分けマップ

目的:

開発中にエラーが出たとき、どこを見ればよいか分かるようにする。

生成依頼文:

```text
初心者向けに、React + Spring Bootアプリのエラー切り分けマップを作ってください。

左から右に、React画面、ブラウザDevTools、Network、Spring Bootログ、Databaseを並べてください。

それぞれに確認ポイントを表示してください。
React画面: 画面にエラーが出ているか
DevTools: Consoleエラーを見る
Network: APIのURL、ステータスコード、レスポンスを見る
Spring Bootログ: 例外やSQLエラーを見る
Database: データが保存されているか見る

下部に「まずConsole、次にNetwork、次にSpring Bootログを見る」という流れを入れてください。
白背景、角丸カード、警告色は控えめなオレンジ、全体はモダンな技術資料風にしてください。
```

## コードサンプルとセットで説明したい内容

画像だけで終わらせず、必ず短いコードサンプルをセットにする。

### Spring Boot

- Controller: URLとHTTPメソッドを受け取る
- Request DTO: Reactから送られるJSONの形
- Response DTO: Reactへ返すJSONの形
- Service: 登録、編集、削除などの処理を書く
- Mapper: DTOとEntityを変換する
- Entity: DBに保存する形
- Repository: DB操作を担当する

### React

- App: stateを持つ親コンポーネント
- RestaurantForm: 登録・編集フォーム
- RestaurantFilter: 地域・ジャンル・ステータスの絞り込み
- RestaurantList: 一覧を並べる
- RestaurantCard: 1件のお店を表示する
- api client: Spring Boot APIを呼ぶ関数をまとめる

## 説明時間を長めに取るべきポイント

- Day 1: フロントエンド、バックエンド、DB、APIの境界
- Day 2: Controller、Service、Repository、DTO、Entity、Mapperの役割
- Day 3: CRUDとHTTPメソッドの対応
- Day 4: Reactのprops、state、useEffect
- Day 5: 画面操作からAPI、DB更新、画面再描画までの一連の流れ
