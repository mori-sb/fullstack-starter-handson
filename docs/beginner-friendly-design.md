# Beginner-Friendly Design

この教材を本当に初心者に優しくするための設計メモです。

## 基本方針

初心者向け教材では、情報を正確に詰め込むだけでは足りない。

次の順番を守る。

```text
1. 何のために学ぶのか
2. 全体のどこに位置するのか
3. 何を作るのか
4. どのファイルを見るのか
5. どう動作確認するのか
6. 詰まったら何を見るのか
```

## 一度に出す概念を減らす

Spring Bootでは、最初から全部を出さない。

最初:

```text
Controller
Service
Repository
DB
```

次:

```text
Entity
DTO
Mapper
Validation
Error Response
```

Reactでも、最初から全部を出さない。

最初:

```text
Component
props
state
useState
```

次:

```text
useEffect
fetch
loading
error
filter
```

## 各Dayのテンプレート

各Dayはこの構成に寄せる。

```text
1. 今日のゴール
2. 今日作るもの
3. 今日の全体図
4. 重要な用語
5. コードを見る順番
6. 実装
7. 動作確認
8. よくあるエラー
9. 今日の確認質問
10. 余裕があれば
```

## 説明の粒度

初心者にとって難しい説明:

```text
Service層にビジネスロジックを実装します
```

言い換え:

```text
Controllerは受付です。
Controllerは細かい処理を自分でやらず、Serviceにお願いします。
Serviceには「登録する」「編集する」「削除する」など、アプリとしてやりたい処理を書きます。
```

専門用語を使う場合は、必ず先に日常的な言い換えを置く。

## 図とコードの対応

図を見せたら、必ず対応するコードを見せる。

例:

```text
図: ControllerはAPIの入口
コード: @RestController, @GetMapping, @PostMapping
```

```text
図: ReactからAPIを呼ぶ
コード: fetch("http://localhost:8080/api/restaurants")
```

図だけでも、コードだけでも弱い。
図とコードを往復することで理解しやすくなる。

## 動作確認を細かく分ける

初心者は「動かない」となったときに原因の場所を切り分けにくい。

そのため、動作確認は必ず小さく分ける。

```text
Spring Boot API単体で確認
React固定データで確認
ReactからGET APIを呼ぶ
ReactからPOST APIを呼ぶ
ReactからPUT APIを呼ぶ
ReactからDELETE APIを呼ぶ
フィルタを確認
```

一気に全部つなげない。

## 参加者に求める完成度

初心者向けでは、完璧なコードより次を重視する。

- どの層のコードか説明できる
- どのAPIが呼ばれるか説明できる
- APIのレスポンスJSONを読める
- 画面が変わる理由を説明できる
- エラー時に確認する場所が分かる

## 避けること

- 初日に環境構築だけで終わる
- 最初から認証を入れる
- 最初から画像アップロードを入れる
- 最初からDTO、Entity、Mapperを全部説明する
- エラー処理を本格的に作り込みすぎる
- CSSに時間を使いすぎる
- AI生成コードを読まずに進める

## 入れると良いもの

- 用語集
- ファイル役割一覧
- API一覧
- 画面とAPIの対応表
- よくあるエラー集
- 動作確認チェックリスト
- 各Dayの最後の理解確認

これらは、初心者が迷子になったときの地図になる。
