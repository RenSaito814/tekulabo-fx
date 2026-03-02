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
cd /mnt/c/Users/perot/Documents/tekulabo-fx
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
cd /mnt/c/Users/perot/Documents/tekulabo-fx && hugo && wrangler pages deploy public --project-name=tekulabo-fx --branch=main
```

### ローカルプレビュー

```bash
cd /mnt/c/Users/perot/Documents/tekulabo-fx && hugo server -D
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

## コミットルール

kane儲け.comプロジェクトのCLAUDE.mdに準拠。日本語コミットメッセージ。

```
機能(ブログ): 新記事を追加
文書(ブログ): Aboutページを更新
修正(ブログ): OGP設定を修正
```
