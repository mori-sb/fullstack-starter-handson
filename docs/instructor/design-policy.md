# Design Policy

このハンズオンは、AIで実装が速く進むことを前提にしつつ、Spring BootとReactの構造を理解できることを重視します。

## 重視すること

- コードを手で打つ量より、構造を説明できること
- 動いたかどうかより、なぜ動くのかを追えること
- エラー時に、フロントエンド側かバックエンド側かを切り分けられること
- AIが生成したコードを読んで、必要な修正箇所を見つけられること
- 「今どこの話をしているか」を見失わないこと

## 各Dayの基本構成

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

## 説明順

説明が長くなっても、次の順番を崩さない。

```text
1. 図で見る
2. 日常的な言葉で説明する
3. 専門用語を出す
4. コードで確認する
5. 動かして確認する
```

例:

```text
Controllerは受付です。
受付は細かい作業を全部自分でやりません。
実際の作業はServiceにお願いします。
Serviceには、登録する、編集する、削除する、というアプリの処理を書きます。
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
DBモデル
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

## 図とコードの対応

図を見せたら、必ず対応するコードを見せる。

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

最初は「動かない」となったときに原因の場所を切り分けにくい。

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

## 今回扱わないこと

次の内容は重要ですが、今回の主題から外れるため扱いません。

- ログイン認証
- 権限管理
- 画像アップロード
- 外部ストレージ連携
- 本番デプロイ
- 複雑な状態管理ライブラリ

まずは、小さなアプリを通してフロントエンド、バックエンド、DB、APIの基本構造を理解します。
