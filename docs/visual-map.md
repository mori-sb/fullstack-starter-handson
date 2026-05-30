# Visual Map

この教材では、文字だけで説明しない。

初心者が迷いやすい概念は、できるだけ図にする。
画像は多めでよい。
ただし、1枚に詰め込みすぎず、段階的に見せる。

## 画像を使う方針

- 1枚の画像で1つの概念だけ説明する
- 左から右に流れる図を基本にする
- 右に行くほどDBに近づく構成にする
- まず基本の流れを見せてから、DTO、Entity、Mapperなどを追加する
- 画像を見せた後に、対応するコードを見る
- 画像の中の用語とコードのクラス名を対応させる

## Day 1: 全体像

### 1. Webアプリ全体構成図

ファイル名:

```text
images/app-overview.png
```

使う場面:

ハンズオンの最初。

伝えること:

- ユーザーはブラウザを使う
- Reactは画面を表示する
- Spring BootはAPIを処理する
- DBはデータを保存する
- ReactはDBを直接触らない

### 2. HTTPリクエスト / レスポンス図

ファイル名:

```text
images/http-json-flow.png
```

使う場面:

APIとは何かを説明するとき。

伝えること:

- ReactからSpring BootへHTTPリクエストが飛ぶ
- Spring BootからReactへJSONレスポンスが返る
- JSONは画面とAPIの間で受け渡すデータ

### 3. 画面操作とAPIの対応図

ファイル名:

```text
images/screen-api-map.png
```

伝えること:

- 一覧を見るときはGET
- 登録するときはPOST
- 編集するときはPUT
- 削除するときはDELETE

生成依頼文:

```text
モダンなSaaS技術資料風に、グルメ管理アプリの画面操作とAPIの対応図を作ってください。

左にReact画面の操作カードを縦に並べ、右に対応するAPIカードを縦に並べてください。

対応は次の通りです。
一覧を見る -> GET /api/restaurants
お店を登録する -> POST /api/restaurants
お店を編集する -> PUT /api/restaurants/{id}
お店を削除する -> DELETE /api/restaurants/{id}
地域で絞り込む -> GET /api/restaurants?area=新宿

左から右へ矢印でつないでください。
HTTPメソッドは小さなカラーバッジで表示してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアン・グリーン・オレンジ・レッドをアクセントに使ってください。
初心者が「画面操作とAPIは対応している」と一目で分かる図にしてください。
```

### 4. 画像URL表示の流れ

ファイル名:

```text
images/image-url-flow.png
```

伝えること:

- DBに保存するのは画像ファイルではなく画像URL
- APIはimageUrlをJSONで返す
- Reactは`img`タグで画像を表示する

## Day 2: Spring Bootの基本

### 5. Spring Boot基本構造図

ファイル名:

```text
images/spring-basic-flow.png
```

作成済み。

伝えること:

- ControllerはAPIの入口
- Serviceは処理を書く場所
- RepositoryはDBとやり取りする場所
- DBはデータを保存する場所

### 6. Spring Bootクラス名対応図

ファイル名:

```text
images/spring-class-map.png
```

伝えること:

- Controllerという概念は`RestaurantController`に対応する
- Serviceという概念は`RestaurantService`に対応する
- Repositoryという概念は`RestaurantRepository`に対応する

### 7. Controllerの役割図

ファイル名:

```text
images/controller-role.png
```

生成依頼文:

```text
初心者向けに、Spring BootのControllerの役割を説明するモダンな図を作ってください。

左にReact、中央にRestaurantController、右にRestaurantServiceを配置してください。
ReactからRestaurantControllerへ「GET /api/restaurants」「POST /api/restaurants」のリクエストが入るようにしてください。
RestaurantControllerからRestaurantServiceへ「処理を依頼」という矢印を描いてください。

Controllerカードには「APIの入口」「URLとHTTPメソッドを受け取る」「細かい処理はServiceに任せる」と表示してください。

この図ではRepository、Entity、DTO、Mapperは出さないでください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアンとグリーンを使ったモダンな教材図にしてください。
```

### 8. Serviceの役割図

ファイル名:

```text
images/service-role.png
```

生成依頼文:

```text
初心者向けに、Spring BootのServiceの役割を説明するモダンな図を作ってください。

左にRestaurantController、中央にRestaurantService、右にRestaurantRepositoryを配置してください。
ControllerからServiceへ「一覧取得を依頼」「登録を依頼」「削除を依頼」という矢印を描いてください。
ServiceからRepositoryへ「DB操作を依頼」という矢印を描いてください。

Serviceカードには「処理を書く場所」「アプリとして何をするかを決める」「ControllerとRepositoryの間に立つ」と表示してください。

この図ではDTO、Entity、Mapperは出さないでください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、グリーンを中心にしたモダンな教材図にしてください。
```

### 9. Repositoryの役割図

ファイル名:

```text
images/repository-role.png
```

生成依頼文:

```text
初心者向けに、Spring BootのRepositoryの役割を説明するモダンな図を作ってください。

左にRestaurantService、中央にRestaurantRepository、右にDatabaseを配置してください。
ServiceからRepositoryへ「保存して」「一覧を取って」「削除して」という依頼が来るようにしてください。
RepositoryからDatabaseへ「SELECT」「INSERT」「UPDATE」「DELETE」の矢印を描いてください。

Repositoryカードには「DBとやり取りする場所」「Serviceから呼ばれる」「基本的なCRUDを担当」と表示してください。

白または薄いグレー背景、角丸カード、細い枠線、控えめな影、グリーンとパープルを使ったモダンな教材図にしてください。
```

## Day 3: DBとCRUD

### 10. CRUDとHTTPメソッド対応図

ファイル名:

```text
images/crud-api-map.png
```

### 11. EntityとDBテーブル対応図

ファイル名:

```text
images/entity-table-map.png
```

関連する作成済み画像:

```text
images/spring-entity-flow.png
```

まず `spring-entity-flow.png` でEntityの位置づけを説明し、その後でEntityとDBテーブルの対応を説明する。

生成依頼文:

```text
初心者向けに、Spring BootのEntityとDBテーブルの対応を説明する図を作ってください。

左にJavaのRestaurant Entityカード、右にDBのrestaurants tableカードを配置してください。
対応する項目を線でつないでください。

Restaurant Entity:
id
name
area
genre
memo
imageUrl
status

restaurants table:
id
name
area
genre
memo
image_url
status

下部に「EntityはDBに保存するデータの形をJavaで表したもの」と表示してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、グリーンとパープルを使ったモダンな教材図にしてください。
```

### 12. 登録APIの流れ

ファイル名:

```text
images/create-api-flow.png
```

### 13. 一覧取得APIの流れ

ファイル名:

```text
images/list-api-flow.png
```

### 14. クエリパラメータの図

ファイル名:

```text
images/query-param-flow.png
```

生成依頼文:

```text
初心者向けに、クエリパラメータで一覧を絞り込む流れを説明する図を作ってください。

左にReactのフィルタUIカードを置き、「地域: 新宿」を選択している状態にしてください。
中央にURLカードとして「GET /api/restaurants?area=新宿」を表示してください。
右にSpring Boot APIカード、さらに右にDatabaseカードを配置してください。

ReactからAPIへ「条件付きで一覧取得」
APIからDBへ「area = 新宿 で検索」
DBからReactへ「新宿のお店だけ返す」
という流れを矢印で表してください。

下部に「?area=新宿 のようにURLに条件を付ける」と補足してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアン・グリーン・パープルを使ったモダンな教材図にしてください。
```

## Day 4: Reactの基本

### 15. Reactコンポーネント分割図

ファイル名:

```text
images/react-components.png
```

### 16. propsの流れ

ファイル名:

```text
images/react-props-flow.png
```

生成依頼文:

```text
初心者向けに、Reactのpropsの流れを説明するモダンな図を作ってください。

上にAppコンポーネントを配置し、下にRestaurantList、さらにその下にRestaurantCardを複数配置してください。
AppからRestaurantListへ「restaurantsを渡す」
RestaurantListからRestaurantCardへ「restaurantを1件ずつ渡す」
という矢印を描いてください。

propsカードには「親から子へ渡すデータ」と表示してください。
この図ではstateやuseEffectは出さず、propsだけに集中してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、Reactらしいシアンを中心にしたモダンな教材図にしてください。
```

### 17. stateの流れ

ファイル名:

```text
images/react-state-flow.png
```

### 18. フォーム入力とstate

ファイル名:

```text
images/form-state-flow.png
```

生成依頼文:

```text
初心者向けに、Reactのフォーム入力とstateの関係を説明する図を作ってください。

左に入力フォームを置き、店名、地域、ジャンル、メモ、画像URLの入力欄を表示してください。
右にform stateカードを置き、name、area、genre、memo、imageUrlが入っている様子を表示してください。

入力欄からform stateへ矢印を描き、「入力するとstateが変わる」と表示してください。
下部に「stateが現在の入力内容を覚えている」と補足してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアンとオレンジを使ったモダンな教材図にしてください。
```

### 19. 画像URLとimgタグ

ファイル名:

```text
images/react-image-tag.png
```

生成依頼文:

```text
初心者向けに、Reactで画像URLをimgタグに渡して画像を表示する流れを説明する図を作ってください。

左にrestaurantデータカードを置き、imageUrl: "https://..." を表示してください。
中央にReactコードカードとして <img src={restaurant.imageUrl} alt={restaurant.name} /> を表示してください。
右にブラウザ上のお店カードUIを置き、画像が表示されている様子にしてください。

矢印には「imageUrlを受け取る」「srcに渡す」「画像が表示される」と表示してください。
下部に「画像ファイルではなく、画像URLを使う」と補足してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアンとコーラルを使ったモダンな教材図にしてください。
```

## Day 5: API連携とデバッグ

### 20. useEffectでAPIを呼ぶ流れ

ファイル名:

```text
images/useeffect-api-flow.png
```

生成依頼文:

```text
初心者向けに、ReactのuseEffectでAPIを呼ぶ流れを説明する図を作ってください。

左から右に、画面を開く、useEffectが動く、fetchでGET /api/restaurantsを呼ぶ、JSONを受け取る、setRestaurantsでstate更新、カード一覧が表示される、の6ステップを並べてください。

各ステップを角丸カードにし、細い矢印でつないでください。
useEffect、fetch、state更新のカードを少し強調してください。
白または薄いグレー背景、シアン・グリーン・オレンジを使ったモダンな教材図にしてください。
```

### 21. fetchの中身

ファイル名:

```text
images/fetch-request-map.png
```

生成依頼文:

```text
初心者向けに、ReactのfetchでAPIを呼ぶときの中身を説明する図を作ってください。

左にfetchコードカード、右にHTTPリクエストカードを配置してください。
fetchコードカードには、URL、method、headers、bodyを表示してください。
HTTPリクエストカードには、POST /api/restaurants、Content-Type: application/json、JSON bodyを表示してください。

対応する要素を線でつないでください。
URL -> APIのURL
method -> HTTPメソッド
headers -> JSONを送る設定
body -> 送信するJSON

白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアンとグリーンを使ったモダンな教材図にしてください。
```

### 22. CORSの図

ファイル名:

```text
images/cors-basic.png
```

生成依頼文:

```text
初心者向けに、CORSの基本を説明する図を作ってください。

左にReact開発サーバー http://localhost:5173、右にSpring Boot API http://localhost:8080 を配置してください。
ReactからSpring BootへAPIリクエストの矢印を描いてください。

中央にブラウザの安全チェックとして「別のオリジンへの通信なので許可が必要」と表示してください。
Spring Boot側に「CORS設定でReactからのアクセスを許可」と表示してください。

難しくしすぎず、「ポートが違うとブラウザが確認する」ということが分かる図にしてください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、シアン・グリーン・オレンジを使ったモダンな教材図にしてください。
```

### 23. エラー切り分けマップ

ファイル名:

```text
images/debugging-map.png
```

### 24. 開発の進め方

ファイル名:

```text
images/development-flow.png
```
