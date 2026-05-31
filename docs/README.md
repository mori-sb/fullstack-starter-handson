# Hands-on Docs

このディレクトリには、ハンズオンで使う資料を置いています。

まずは、次の順番で読んでください。

```text
1. getting-started.md
2. curriculum.md
3. day1.md
4. day2.md
5. day3.md
6. day4.md
7. day5.md
```

`getting-started.md` には、事前学習項目と参考サイトもまとめています。
Java、JavaScript、TypeScriptに慣れていない場合は、Day1の前にそこだけ軽く読んでおきます。

分からない言葉が出てきたら `glossary.md`、作業確認をしたいときは `checklists.md` を見ます。

## 1日の進め方

各日は、説明、実装、演習の順番で進めます。
いきなり完成コードを書くのではなく、次の順番で理解していきます。

```text
1. 今日作るものを確認する
2. 図で全体の流れをつかむ
3. どのファイルに何を書くか確認する
4. 小さく実装する
5. 動作確認する
6. 演習で同じ考え方を使う
```

コードはAIで生成してもよいですが、生成されたコードをそのまま貼って終わりにはしません。
必ず「どのファイルが、どの役割を持ち、どのデータを受け渡しているか」を確認します。

## ライブコーディングと演習

この教材では、ライブコーディングと演習の題材を分けます。

```text
ライブコーディング  Movie
演習              Restaurant
```

参加者は、Movieで説明された構造を見ながら、Restaurantへ置き換えて実装します。
説明していない内容を演習に出さないようにします。

## 進める順番

目安は次の通りです。

```text
1. 今日作るものとゴール確認
2. 概念説明と図の確認
3. コードの読み方、ファイル構成の確認
4. 段階的に実装する
5. 動作確認、よくあるエラー、演習説明
6. 演習
```

演習では、完成コードを目指すだけでなく、次の説明ができる状態を目指します。

- どの画面操作で、どのAPIが呼ばれるか
- どのファイルに処理が書かれているか
- APIで送るJSONと返るJSONの形
- エラーが出たときに最初に見る場所

## 参加者向け

- [Curriculum](curriculum.md)
- [Getting Started](getting-started.md)
- [Day 1: Webアプリの全体像](day1.md)
- [Day 2: Spring Bootの基本](day2.md)
- [Day 3: DB接続とCRUD API](day3.md)
- [Day 4: Reactの基本](day4.md)
- [Day 5: ReactとAPI連携](day5.md)
- [Glossary](glossary.md)
- [Checklists](checklists.md)

## 講師向け

講師・教材作成者向けのメモは `instructor/` にまとめています。

- [Instructor Notes](instructor/notes.md)
- [Design Policy](instructor/design-policy.md)
- [Visual Map](instructor/visual-map.md)
- [Code Samples](instructor/code-samples.md)
- [Exercise Answers](instructor/exercise-answers.md)
