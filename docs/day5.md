# Day 5: ReactとAPI連携

## 今日のゴール

- ReactからSpring Boot APIを呼び出せる
- 一覧表示、登録、更新、削除の流れをつなげられる
- フロントエンドとバックエンドを分けて考えられる
- 地域・ジャンル・ステータスで絞り込める
- 画像URLを使って画像を表示できる
- 1週間の内容を自分の言葉で説明できる
- AIが作った実装を見て、画面からAPIまでの流れを追える

## 扱う内容

- fetchまたはaxios
- API呼び出し
- ローディング状態
- エラー表示
- CORS
- フィルタ
- 編集
- 削除
- まとめ

## 今日の大事な考え方

フロントエンドとバックエンドは、APIでつながる。

```text
Reactのボタンを押す
  ↓
fetchでAPIを呼ぶ
  ↓
Spring Bootが処理する
  ↓
DBを更新する
  ↓
JSONを返す
  ↓
Reactがstateを更新する
  ↓
画面が変わる
```

## 最初に伝えること

Day5では、これまで別々に作ってきたReactとSpring Bootをつなげる。

ここで重要なのは、ReactとSpring Bootの責務を混ぜないこと。

```text
React
  画面を表示する
  入力を受け取る
  APIを呼ぶ
  APIの結果で画面を更新する

Spring Boot
  APIを受け取る
  処理する
  DBへ保存・取得する
  JSONを返す
```

ReactはDBを直接触らない。
Spring Bootは画面を直接描画しない。
両者はAPIでつながる。

## API呼び出しの基本

ReactからAPIを呼ぶには `fetch` を使う。

一覧取得の例:

```jsx
export async function fetchRestaurants() {
  const response = await fetch("http://localhost:8080/api/restaurants");
  return response.json();
}
```

登録の例:

```jsx
export async function createRestaurant(restaurant) {
  const response = await fetch("http://localhost:8080/api/restaurants", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(restaurant),
  });

  return response.json();
}
```

見るポイント:

- `method` でHTTPメソッドを指定する
- JSONを送るときは `Content-Type` を指定する
- JavaScriptのオブジェクトは `JSON.stringify` でJSON文字列にする
- APIの結果は `response.json()` で取り出す

## useEffectで一覧を取得する

画面を開いたときに一覧を取得する。

```jsx
useEffect(() => {
  async function loadRestaurants() {
    const data = await fetchRestaurants();
    setRestaurants(data);
  }

  loadRestaurants();
}, []);
```

流れ:

```text
画面を開く
  ↓
useEffectが動く
  ↓
GET /api/restaurants を呼ぶ
  ↓
JSONを受け取る
  ↓
setRestaurantsでstateを更新する
  ↓
お店一覧が表示される
```

## CORS

ReactとSpring Bootを別々のポートで動かすと、ブラウザが通信を止めることがある。

例:

```text
React        http://localhost:5173
Spring Boot  http://localhost:8080
```

このようにオリジンが違う場合、Spring Boot側でReactからのアクセスを許可する必要がある。

初心者には、CORSは「ブラウザの安全機能」と説明する。
エラーが出たら、ReactのコードだけでなくSpring Boot側の設定も確認する。

## 入れたい図

- ReactからAPIを呼び出して画面が更新される流れ
- フロントエンドとバックエンドの責務分担
- 登録、編集、削除でどのAPIが呼ばれるかの対応図

## 図の説明メモ

```text
一覧表示  GET    /api/restaurants
登録      POST   /api/restaurants
編集      PUT    /api/restaurants/{id}
削除      DELETE /api/restaurants/{id}
```

## ハンズオン

React画面とSpring Boot APIを接続し、ミニアプリとして完成させる。

完成ライン:

- お店を登録できる
- お店一覧を見られる
- 画像を表示できる
- お店を編集できる
- お店を削除できる
- 地域で絞り込める
- ジャンルで絞り込める
- ステータスで絞り込める

## 動作確認の順番

フルスタック開発では、いきなり全部つなげて確認しない。

次の順番で確認すると、エラーの場所を見つけやすい。

```text
1. Spring Boot単体でAPIを確認する
2. React単体で固定データ表示を確認する
3. Reactから一覧APIを呼ぶ
4. 登録APIをつなぐ
5. 編集APIをつなぐ
6. 削除APIをつなぐ
7. フィルタをつなぐ
```

一気に全部やると、どこで壊れているか分からなくなる。

## エラー切り分け

画面がうまく動かないときは、次の順番で見る。

```text
1. ブラウザのConsole
   JavaScriptエラーが出ていないか

2. ブラウザのNetwork
   APIが呼ばれているか
   URLは正しいか
   ステータスコードは何か
   レスポンスは何か

3. Spring Bootのログ
   例外が出ていないか
   Controllerに届いているか

4. DB
   データが保存されているか
```

初心者には「まずNetworkを見る」習慣をつけてもらうとよい。

## 今日の確認ポイント

- ReactからSpring Boot APIを呼ぶ流れを説明できる
- `GET`、`POST`、`PUT`、`DELETE` がReactのどの操作に対応するか説明できる
- APIの結果でstateを更新し、画面が変わることを説明できる
- CORSエラーが出たときに、何が起きているか大まかに説明できる
- エラー時にConsole、Network、Spring Bootログを見る順番が分かる

## よくある混乱

### APIは呼べているのに画面が変わりません

APIの結果をstateに入れていない可能性がある。

Reactでは、データを取得しただけでは画面は変わらない。
`setRestaurants` のようなstate更新が必要。

### Spring Bootでは成功しているのにReactでエラーになります

CORS、URL間違い、JSONの形の違い、React側の処理ミスが考えられる。

Networkタブで、実際にどんなレスポンスが返っているか確認する。

### 画像だけ表示されません

`imageUrl` の値を確認する。

APIレスポンスに `imageUrl` が含まれているか、URL先の画像が存在するかを見る。

## メモ

最後は「自分で1項目追加してみる」など、小さな応用課題を入れる。
