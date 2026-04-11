**言語:** [English](../en/README.md) | [中文](../../README.md) | [한국어](../ko/README.md) | 日本語 | [Español](../es/README.md)

# ikiw

LLM ベースの個人ナレッジベース skill。フレームワーク、データベース、ベクトル検索に一切依存せず、純粋に prompt 駆動で動作し、ファイルの読み書きができる LLM agent であればどれでも利用可能。

[Karpathy の LLM Wiki パターン](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)にインスピレーションを受けて開発。

## コアアイデア

記事をディレクトリに放り込むと、LLM が各記事の要約を生成し、インデックスファイルにまとめる。クエリ時には LLM がインデックスを読んで該当記事を特定し、原文を読んで総合的に回答する。それだけ。

タグ付けなし、分類なし、データベースなし。要約そのものが最も立体的な「タグ」になる。

## ナレッジベースの構造

```
my-wiki/
├── raw/              # 原文記事（読み取り専用）
├── summaries.md      # 要約インデックス（システムの中核）
├── wiki/             # 必要に応じて生成されるナレッジページ
└── SCHEMA.md         # ナレッジベース設定（カスタム prompt）
```

## 機能

| コマンド | 説明 |
|------|------|
| 要約生成 | 記事を読み、要約を生成して summaries.md に追記 |
| 一括要約 | 未処理の記事を検出し、一括で生成 |
| クエリ | 要約インデックスを読む → 記事を特定 → 原文を読む → 総合回答 |
| Wiki 生成 | テーマごとの知識を構造化ページにまとめ、必要に応じて作成 |
| スタイル付き執筆 | コンテンツ + 文体テンプレートに基づいて記事を出力 |
| スタイル出力 | design-md でスタイル付き HTML にレンダリング |
| 新規記事の登録 | 要約を生成し、wiki の更新をチェック |
| 初期化 | ナレッジベースを作成し、対話を通じて SCHEMA.md を生成 |
| 要約アシスタント | 対話を通じてユーザーの要約 prompt の定義をサポート |

## 3 層出力

同じコンテンツに対して、異なるテンプレートを段階的に重ねることができる：

1. **コンテンツ層** — SCHEMA.md の wiki prompt が「何を書くか」を決定
2. **文体層** — `styles/` ディレクトリの文体テンプレートが「どう書くか」を決定
3. **ビジュアル層** — `designs/` ディレクトリの design-md が「どう見せるか」を決定

## クイックスタート

1. 本リポジトリを skill として LLM agent（Claude Code、Codex、OpenCode など）に読み込ませる
2. agent に「ナレッジベースを作って」と伝える
3. agent が対話を通じて記事の種類や関心事を把握し、自動で SCHEMA.md を生成
4. 記事を `raw/` ディレクトリに配置
5. agent に「要約を生成して」と伝える
6. クエリ開始

## プロジェクト構成

```
ikiw/
├── SKILL.md              # skill 定義（agent のエントリポイント）
├── SCHEMA.template.md    # ナレッジベース設定テンプレート
├── designs/              # ビジュアルスタイル（design-md）
│   ├── notion.md
│   ├── claude.md
│   ├── linear.app.md
│   ├── stripe.md
│   ├── vercel.md
│   └── mintlify.md
└── styles/               # 文体テンプレート（ユーザーが自由に追加）
```

## スケール

- 1 万記事以内：summaries.md を一括読み込み可能、追加インフラ不要
- 1 万記事超：ベクトル検索で粗いフィルタリングを導入、要約データはそのまま embedding に利用可能

## 設計原則

- **コードを書かない** — システム全体が prompt の組み合わせであり、プログラムではない
- **原文に手を加えない** — raw/ ディレクトリは読み取り専用
- **事前生成しない** — wiki は必要に応じて作成、無駄なし
- **プラットフォームに縛られない** — ファイルの読み書きができる LLM agent ならどれでも利用可能
- **過度に設計しない** — 必要になった時に機能を追加

## ビジュアルスタイルの追加

```bash
npx getdesign@latest add <style-name>
# 生成された DESIGN.md をリネームして designs/ ディレクトリに配置
```

60 以上のスタイルが利用可能：https://github.com/VoltAgent/awesome-design-md

## 謝辞

- [Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — LLM Wiki パターン
- [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) — ビジュアルスタイルライブラリ
