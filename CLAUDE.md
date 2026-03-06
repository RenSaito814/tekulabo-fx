# テクラボFX - 開発ガイド

## サイト概要

| 項目 | 値 |
|------|-----|
| サイト名 | テクラボFX |
| URL | https://tekulabo-fx.com |
| SSG | Hugo v0.157.0 extended |
| テーマ | PaperMod (git submodule) |
| ホスティング | Cloudflare Pages (GitHub連携) |
| リポジトリ | RenSaito814/tekulabo-fx |

---

## デプロイ

### 自動デプロイ（通常）

`main`ブランチにpushすると、Cloudflare Pagesが自動でビルド・デプロイする。

```bash
cd /mnt/c/Users/perot/Documents/kane儲け.com/tekulabo-fx
git add <files>
git commit -m "メッセージ"
git push
```

### 手動デプロイ（緊急時）

```bash
OP="/mnt/c/Users/perot/AppData/Local/Microsoft/WinGet/Packages/AgileBits.1Password.CLI_Microsoft.Winget.Source_8wekyb3d8bbwe/op.exe"
CF_TOKEN=$("$OP" read "op://Personal/Cloudflare API/api_token" 2>/dev/null | tr -d '\r\n')
export CLOUDFLARE_API_TOKEN="$CF_TOKEN"
export CLOUDFLARE_ACCOUNT_ID="2b581d249dfe32509080589d3e8ccc70"
cd /mnt/c/Users/perot/Documents/kane儲け.com/tekulabo-fx && hugo && wrangler pages deploy public --project-name=tekulabo-fx --branch=main
```

### ローカルプレビュー

```bash
cd /mnt/c/Users/perot/Documents/kane儲け.com/tekulabo-fx && hugo server -D
```
`-D` でドラフト記事も含めてプレビュー。http://localhost:1313 でアクセス。

---

## Cloudflare

| 項目 | 値 |
|------|-----|
| アカウントID | 2b581d249dfe32509080589d3e8ccc70 |
| ゾーンID | 125d8797c1cc53da247d078126c9089d |
| 1Passwordアイテム | Cloudflare API / api_token |
| ビルド環境変数 | HUGO_VERSION=0.157.0 |

### Cloudflare API操作の共通パターン

```bash
OP="/mnt/c/Users/perot/AppData/Local/Microsoft/WinGet/Packages/AgileBits.1Password.CLI_Microsoft.Winget.Source_8wekyb3d8bbwe/op.exe"
CF_TOKEN=$("$OP" read "op://Personal/Cloudflare API/api_token" 2>/dev/null | tr -d '\r\n')
CF_ACCOUNT="2b581d249dfe32509080589d3e8ccc70"
ZONE_ID="125d8797c1cc53da247d078126c9089d"
```

---

## ファイル構成

```
tekulabo-fx/
├── hugo.yaml              # サイト設定
├── CLAUDE.md              # このファイル
├── .gitignore
├── .gitmodules            # PaperModテーマのsubmodule定義
├── archetypes/
│   └── default.md         # 新規記事のテンプレート
├── content/
│   ├── about.md           # /about/
│   ├── privacy.md         # /privacy/
│   ├── disclaimer.md      # /disclaimer/
│   ├── search.md          # /search/ (PaperMod検索機能)
│   └── posts/             # 記事フォルダ
│       └── *.md
├── plan/                  # 計画・進捗管理
│   ├── requirements.md    # 要件定義
│   ├── progress.md        # 進捗状況
│   ├── tasks.md           # タスクリスト
│   ├── decisions.md       # 設計判断の記録
│   └── affiliate-links.md # アフィリエイトリンク管理
├── static/                # 静的ファイル（画像等）
└── themes/
    └── PaperMod/          # git submodule
```

---

## 記事作成

### front matterテンプレート

```yaml
---
title: "記事タイトル"
date: YYYY-MM-DD
draft: false
summary: "検索結果やトップページに表示される要約文。SEOのdescriptionにもなる。"
categories:
  - カテゴリ名
tags:
  - タグ1
  - タグ2
ShowToc: true
# cover:
#   image: "images/記事スラッグ/cover.jpg"
#   alt: "カバー画像の説明"
---
```

### カテゴリ案

- テクニカル分析
- ファンダメンタルズ
- 自動売買
- リスク管理
- 相場分析
- FX基礎知識

### タグ例

- 初心者向け / 中級者向け / 上級者向け
- ローソク足 / 移動平均線 / RSI / MACD / ボリンジャーバンド
- トレンド分析 / サポート・レジスタンス
- Python / バックテスト
- ドル円 / ユーロドル

### 記事のルール

- ファイル名: 英語ケバブケース（例: `fx-technical-analysis-basics.md`）
- 記事末尾に免責事項の注記を入れる:
  ```
  ※本記事は情報提供を目的としており、特定の金融商品の売買を推奨するものではありません。投資判断はご自身の責任において行ってください。
  ```
- `draft: true` で下書き保存可能（デプロイされない）

---

## hugo.yaml 設定要点

- 言語: ja
- テーマ: PaperMod
- ページネーション: 10記事/ページ
- 目次(ToC): 有効（記事単位で制御可）
- 検索: Fuse.js (JSON出力有効)
- RSS: 有効
- ダークモード: auto（OS設定に追従）
- メニュー: ホーム / カテゴリ / タグ / 検索 / About

---

## planフォルダの管理

プロジェクトの計画・進捗・判断記録を `plan/` フォルダで管理する。

### フォルダ構成

```
plan/
├── requirements.md       # 要件定義（サイト目的、対象読者、収益化要件）
├── progress.md           # 進捗状況（フェーズ別の完了・未完了）
├── tasks.md              # タスクリスト（優先度別）
├── decisions.md          # 設計判断の記録（技術選定、戦略決定等）
└── affiliate-links.md    # アフィリエイトリンク管理（テンプレート、配置状況）
```

### 更新タイミング

- 記事追加時: progress.md の記事リストを更新
- アフィリエイトリンク配置時: affiliate-links.md の配置済み記事を更新
- タスク完了時: tasks.md, progress.md を更新
- 技術・戦略の判断時: decisions.md に記録
- マイルストーン達成時: 全ドキュメントを見直し

### コミットルール

- planフォルダの更新は独立したコミットとする
- コミットメッセージのスコープは「計画」を使用
- 例: `文書(計画): 進捗・タスクを最新状態に更新`

---

## コミットルール

kane儲け.comプロジェクトのCLAUDE.mdに準拠。日本語コミットメッセージ。

```
機能(ブログ): 新記事を追加
文書(ブログ): Aboutページを更新
修正(ブログ): OGP設定を修正
文書(計画): 進捗・タスクを更新
```
