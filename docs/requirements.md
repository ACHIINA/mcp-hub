# MCPHub 要件定義書 v0.2

> **コンセプト**: MCPサーバーのGitHub的なハブ
> Model Context Protocol (MCP) サーバーを誰でも登録・発見できるオープンなディレクトリサイト

## 差別化軸

| 軸 | smithery.ai | mcp.so | **MCPHub** |
|----|------------|--------|-----------|
| 価格 | 有料 | 無料 | **無料・オープン** |
| データのオープン性 | クローズド | 限定的 | **Git公開・PR貢献** |
| 信頼シグナル | △ | △ | **◎ (スター/更新/公式/健全性)** |
| 設定コピー導線 | ◯ | △ | **◎ (クライアント別)** |

## ターゲットユーザー

- **ペルソナA**: ツール探索エンジニア（主要）— Claude Code / Cursor ユーザー
- **ペルソナB**: MCP開発者 — 自作サーバーを公開したい人
- **ペルソナC**: チーム導入リード — 安全なツールセットを選定したい人

## MVP 必須機能

- サーバー一覧 + キーワード検索 + カテゴリ/タグ/属性フィルター + ソート
- GitHubメタ自動同期（スター数・最終更新・ライセンス）
- 公式バッジ・健全性表示（stale/archived 警告）
- クライアント別設定スニペットのコピーボタン（Claude/Cursor/VS Code）
- PRベース登録フロー + テンプレ + CIバリデーション

## データモデル（主要フィールド）

```typescript
type Server = {
  id: string;           // slug, URL使用
  name: string;
  description: string;  // 10〜200文字
  category: Category;
  tags: string[];       // 1〜10個
  github_url?: string;  // github_url か npm_package のどちらか必須
  npm_package?: string;
  runtime: 'local' | 'remote' | 'hybrid';
  auth_required: boolean;
  free: boolean;
  official: boolean;
  install?: {           // クライアント別設定スニペット
    claude?: { command: string; args: string[] };
    cursor?: { command: string; args: string[] };
    env_hint?: string[];
  };
  // 自動同期フィールド
  stars?: number;
  last_commit_at?: string;
  is_archived?: boolean;
  health_status?: 'healthy' | 'stale' | 'archived';
};
```

## ページ構成

| URL | ページ |
|-----|--------|
| `/` | トップ（キュレーション + 検索導線） |
| `/servers` | 一覧（フィルター・検索） |
| `/servers/[id]` | 詳細（メタ + スニペット + README） |
| `/submit` | 登録案内 |
| `/api/servers` | 公開JSON API |

## 技術スタック

- **フレームワーク**: Next.js (App Router) + TypeScript
- **スタイル**: Tailwind CSS + shadcn/ui
- **データソース**: Gitリポジトリ (1サーバー=1YAMLファイル) + Supabase
- **検索**: MVP=Fuse.js(クライアント), V2=Postgres全文検索
- **認証**: GitHub OAuth (Supabase Auth)
- **ホスティング**: Vercel

## データフロー

```
[Git: data/servers/*.yaml] ← 真実の源・PRレビュー対象
        ↓ (ビルド/Cron同期)
[Supabase Postgres] ← 検索・フィルター・派生メタのキャッシュ
        ↓
[Next.js (ISR/RSC)] ← 表示
        ↑
[Supabase] ← レビュー・コレクション・コピーイベント (V2以降)
```

## ロードマップ

### V1 (MVP)
- [ ] データ構造の刷新（1サーバー=1ファイル）
- [ ] Next.js 基本UI（一覧・詳細・検索）
- [ ] GitHub Actions CI バリデーション
- [ ] GitHubメタ自動同期
- [ ] 設定スニペットコピー機能

### V2
- [ ] Web投稿フォーム（GitHub App経由でPR自動生成）
- [ ] レビュー・スター機能
- [ ] コレクション機能
- [ ] バッジ生成

### V3
- [ ] セマンティック検索（pgvector）
- [ ] 開発者アナリティクス
- [ ] ニュースレター
