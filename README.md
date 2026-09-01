# AMAMO Carbon Intelligence

カーボンクレジット・エネルギー市場の最新ニュースを自動収集・分析し、Notionデータベースに投稿するシステム。

## 対象業界（5つのNotion DB）

| DB | 対象 | 主なトピック |
|---|---|---|
| ENERGY | 電力/ガス/エネルギー調達 | Article 6, JCM, ETS価格, サプライチェーンリスク |
| TRADING | 商社 | クックストーブ事業者, クレジット創出案件, アフリカ地政学 |
| AVIATION | 航空会社 | CORSIA, ICAO, SAF規制 |
| BANKING | 銀行 | カーボンインシュアランス, ICVCM CCP, ESG投融資 |
| 制度 | 制度・規制 | EU ETS, CBAM, GX-ETS, Article 6運用規則, NDC |

## セットアップ

1. `.env.example` を `.env` にコピーし、Notion APIトークンを設定
2. Claude Codeでこのディレクトリを開く

## 使い方

### 自動実行
スケジュールエージェントにより毎日自動で実行される。

### 手動実行
Claude Codeで `/carbon-news-update` を実行。

## 設定変更

**GitHubのWeb UI上で誰でも編集可能:**

- `config/databases.json` — DB追加・変更（DB ID、プロパティ型、対象トピック）
- `config/sources.json` — ニュースソースの追加・削除
- `config/search-queries.json` — 検索クエリの追加・変更

### DB追加手順
1. Notionで新しいDBを作成（既存DBと同じプロパティスキーマで）
2. 「プロトタイプ最新ニュース自動更新」インテグレーションをDBに接続
3. `config/databases.json` にエントリを追加
4. `impact_property_type` を確認して正しく設定（`"status"` or `"select"`）

### ニュースソース追加手順
1. `config/sources.json` の `allowed_sources` にソース名を追加
2. WebFetchがブロックされる場合は `blocked_fetch_domains` にも追加
