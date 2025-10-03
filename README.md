# marp2cliptext

marpファイルのクリップ差し込み用スライドを特定し、そのノートをクリップのシナリオとして書き出す。

## インストール

```shell
npm install -g .
```

## 使い方

```shell
$ marp2cliptext --help
Usage: marp2cliptext [options] <marp_file_path>

marpファイルのクリップ差し込み用スライドを特定し、そのノートをクリップのシナリオとして書き出す。

Arguments:
  marp_file_path             原稿となるMarpファイルパス

Options:
  -V, --version              output the version number
  -o, --output <output_dir>  テキストファイルを出力するディレクトリパス
  --pdf                      出力したテキストファイルをcupsfilterを使ってPDFに変換する(macOS用)
  --delete-org-file          PDFに変換時にオリジナルのテキストファイルを削除する
  -h, --help                 display help for command
```

## 例

[サンプルのスライド](./sample/slide.md)を例にツールの使い方を紹介します。

Marpのスライド内に後からクリップを差し込みたい場合があります。

その場合、`<div class="wm" />` を追加し、スライド内にクリップを挿入するためのプレースホルダーを記載するようにしています。

```markdown
---
## 正式版リリースは 2025/7/15 !!

<!-- 正式版リリースは 2025年7月15日だよ -->

<div class="wm" title="動画差し込み用のウォーターマーク" />
---
```

ちなみに、プレースホルダーのスタイルは、Marpのfrontmatterのstyleや、テーマの拡張により、定義しています。

```markdown
---
marp: true
theme: gaia
style: |
  section {
    > .wm {
        border-radius: 40px;
        background-color: #ddd;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        overflow-wrap: normal;
        position: absolute;
        max-width: 18rem;
        max-height: 10rem;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
        font-size: 10cqmin;
        font-weight: bold;
        color: red;
        opacity: 0.25;

        &::before {
          content: attr(title);
        }
    }
  }
---
```

[サンプルのスライド](./sample/slide.md)の場合、3枚目のスライドにクリップ用のプレースホルダーが記載されています。
そして、操作画面のキャプチャなどのシンプルな差し込み用のクリップの場合は、スライド内のノートにそのままナレーションも記載します。

あとから、差し込み用の操作画面のキャプチャを作成する場合、操作手順書として、そのクリップのナレーション原稿があると便利です。

このキャプチャ動画の操作手順書を作成するのが本ツールになります。

以下を実行した場合、`./sample/clips`内に、クリップのプレースホルダを持つスライド番号のファイルが作成され、そのナレーション原稿がテキストファイルに保存されます。

```shell
marp2cliptext ./sample/slide.md -o ./sample/clips
```

また、PDF形式でも出力できます。(ただし、`/usr/sbin/cupsfilter`を利用するため、macOS専用)

```shell
marp2cliptext ./sample/slide.md -o ./sample/clips --pdf --delete-org-file
```

## 更新履歴

### v1.0.1

- `-n, --no <slide_numbers>`オプションで処理対象のスライド番号を指定可能に

### v1.0.0

- [kantas-spike/my-marp-utils: marpファイルから動画を作成するためのユーティリティツールを提供する。](https://github.com/kantas-spike/my-marp-utils)から、`marp2cliptext`を移動
