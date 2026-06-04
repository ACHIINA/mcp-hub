# MCP Hub 🧰

**MCPサーバーのディレクトリサイト**

AIエージェント（Claude、GPT等）が使えるMCPサーバーを集めたオープンなカタログです。

## 概要

[MCP (Model Context Protocol)](https://modelcontextprotocol.io/) はAnthropicが策定したAIエージェント用ツール標準規格です。
MCPサーバーは散らばっており、検索しにくい状況です。このサイトはそれを解決します。

## 機能（予定）

- 🔍 MCPサーバーの検索・ブラウジング
- 📂 カテゴリ別分類（リサーチ / コード / データ取得 / ファイル操作 / SNS etc.）
- ⭐ レーティング・レビュー
- 📦 登録フォーム（誰でも投稿可）
- 🆓 基本無料・オープン

## カテゴリ案

| カテゴリ | 例 |
|---------|---|
| 🔍 リサーチ | Exa Search, Brave Search, arXiv |
| 💻 コード | GitHub, GitLab |
| 📁 ファイル | Google Drive, Dropbox |
| 🌐 ウェブ | Playwright, Puppeteer |
| 📊 データ | SQLite, Postgres, CSV |
| 📱 SNS | Twitter/X, Slack, Discord |
| 🧠 AI | OpenAI, Anthropic, Replicate |

## データスキーマ（案）

```json
{
  "id": "exa-search",
  "name": "Exa Search",
  "description": "高精度ウェブ検索API",
  "category": "research",
  "tags": ["search", "web", "free-tier"],
  "github_url": "https://github.com/...",
  "npm_package": "@modelcontextprotocol/server-exa",
  "rating": 4.5,
  "free": true,
  "official": false
}
```

## 技術スタック（予定）

- **フロント**: Next.js + Tailwind CSS
- **DB**: Supabase（無料枠）
- **ホスティング**: Vercel（無料）
- **認証**: GitHub OAuth（投稿者用）

## ロードマップ

- [ ] 静的JSONデータでMVP作成
- [ ] 検索・フィルター実装
- [ ] 投稿フォーム実装
- [ ] Supabase連携
- [ ] レビュー機能

## コントリビュート

MCPサーバーの追加は `data/servers.json` を編集してPRを送ってください！

---

Made with ❤️ by ACHIINA
