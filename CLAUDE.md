# AMAMO Carbon Intelligence — カーボン/エネルギー市場ニュース自動更新システム

## システム概要

カーボンクレジット・エネルギー市場の最新ニュースを自動収集・分析し、5つの業界別Notion DBに構造化されたインテリジェンスレポートとして投稿するシステム。

## 設定ファイル

すべての設定は `config/` ディレクトリのJSONファイルで管理される。設定変更はこれらのファイルを編集すること。

- **`config/databases.json`** — 投稿先Notion DB一覧、DB ID、プロパティスキーマ、各DBの対象トピック
- **`config/sources.json`** — 許可されたニュースソース名リスト、WebFetchブロック対象ドメイン
- **`config/search-queries.json`** — 検索クエリテンプレート

## Notion認証

### Method A: curl + NOTION_API_TOKEN（標準方式）
ローカル実行・クラウドエージェント共通。`.env` ファイルまたは環境変数からAPIトークンを読み取り、curlでNotion APIを直接呼び出す。クラウドエージェント（スケジュール実行）ではプロンプト内でトークンがexportされる。

### Method B: Notion MCP（利用可能な場合のみ）
Notion MCPコネクタの認証が正しく設定されている環境でのみ使用可能。現時点ではクラウド環境のNotion MCPは認証エラー(401)のため使用不可。

### トークン設定:
```
NOTION_API_TOKEN=ntn_xxxxx
```
**セキュリティ注意**: `.env` はgitignoreされている。新しい環境では `.env.example` をコピーして `.env` を作成し、トークンを設定すること。

## 実行方法

### 自動実行（スケジュールエージェント）
毎日自動で実行される。設定変更時はconfigファイルを編集してpushするだけで反映される。

### 手動実行（Claude Code）
```
/carbon-news-update
```

### 手動実行（特定期間）
「前回の稼働から今日までの分を更新しなさい」と指示する。

## インパクト度プロパティの型の違い（重要）

`databases.json` の `impact_property_type` を必ず確認すること:
- `"status"` → `{"status":{"name":"高"}}` （ENERGY, TRADING, AVIATION）
- `"select"` → `{"select":{"name":"高"}}` （BANKING, 制度）

間違えると400エラーになる。

## 分析品質基準

各記事の本文は日本語で以下3セクション:
- **概要**: 2-3箇条書き。具体的な数値（価格・数量・日付）を含める
- **業界への影響**: 2箇条書き。対象業界の事業・コスト・戦略への具体的影響
- **推奨アクション**: 1-2箇条書き。期限がある場合は明記

## 用語集

| 略語 | 正式名称 |
|---|---|
| CORSIA | Carbon Offsetting and Reduction Scheme for International Aviation |
| EEU | Eligible Emissions Unit (CORSIA) |
| ITMO | Internationally Transferred Mitigation Outcome (Article 6.2) |
| LoA | Letter of Authorization (Article 6) |
| CCP | Core Carbon Principles (ICVCM) |
| SAF | Sustainable Aviation Fuel |
| ETS | Emissions Trading System |
| NbS | Nature-based Solutions |
| FPIC | Free, Prior and Informed Consent |
| GX-ETS | Japan's Green Transformation ETS |
| JCM | Joint Crediting Mechanism |
| NDC | Nationally Determined Contribution |
| CBAM | Carbon Border Adjustment Mechanism |

## 設定変更の方法（メンバー向け）

### 新しいDBを追加する場合
1. `config/databases.json` に新しいエントリを追加
2. `impact_property_type` を正しく設定（Notion DBのプロパティ型を確認）
3. `focus` に対象トピックを記述

### 検索クエリを追加・変更する場合
1. `config/search-queries.json` を編集

### ニュースソースを追加する場合
1. `config/sources.json` の `allowed_sources` にソース名を追加
2. WebFetchがブロックされるサイトは `blocked_fetch_domains` にも追加
