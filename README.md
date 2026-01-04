# project-starter

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-v1.0.33+-blue.svg)](https://code.claude.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/CloudAI-X/claude-workflow/pulls)

[English Version (README_EN.md)](./README_EN.md)

あらゆるソフトウェアプロジェクトに対応する、専用のエージェント、スキル、フック、出力スタイルを備えたユニバーサルな Claude Code ワークフロープラグインです。

---

## クイックスタート

### オプション 1: CLI（セッションごと）

```bash
# プラグインをクローン
git clone https://github.com/CloudAI-X/claude-workflow.git

# プラグインを指定して Claude Code を起動
claude --plugin-dir ./claude-workflow
```

### オプション 2: Agent SDK

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Hello",
  options: {
    plugins: [{ type: "local", path: "./claude-workflow" }]
  }
})) {
  // プラグインのコマンド、エージェント、スキルが利用可能になります
}
```

### オプション 3: 恒久的なインストール

```bash
# マーケットプレイスからインストール（利用可能な場合）
claude plugin install project-starter

# またはローカルディレクトリからインストール
claude plugin install ./claude-workflow
```

### インストールの確認

プラグインを読み込んだ後、正常に動作しているか確認します：

```
> /plugin
```

**Installed** タブに移動し、`project-starter` がリストにあることを確認してください。
**Errors** タブにエラーがないことを確認します。

以下のコマンドが利用可能になります：

```
/project-starter:architect    # アーキテクチャ優先モード
/project-starter:rapid        # 高速開発モード
/project-starter:commit       # コミットメッセージの自動生成
/project-starter:verify-changes  # マルチエージェントによる検証
```

---

## 含まれているもの

| コンポーネント | 数 | 説明 |
|-----------|---|-------------|
| **エージェント** | 7 | コードレビュー、デバッグ、セキュリティ等に特化したサブエージェント |
| **コマンド** | 17 | ワークフローや出力スタイルのためのスラッシュコマンド |
| **スキル** | 6 | Claude が自律的に使用する専門知識ドメイン |
| **フック** | 8 | フォーマット、セキュリティ、通知のための自動化スクリプト |

---

## 使用例

### コマンドの実行

**変更を自動コミットする:**
```
> /project-starter:commit

Looking at staged changes...
✓ Created commit: feat(auth): add JWT refresh token endpoint
```

**Git ワークフローをフルで実行:**
```
> /project-starter:commit-push-pr

✓ Committed: feat: add user dashboard
✓ Pushed to origin/feature/dashboard
✓ Created PR #42: https://github.com/you/repo/pull/42
```

**デプロイ前に検証する:**
```
> /project-starter:verify-changes

Spawning verification agents...
├─ build-validator: ✓ Build passes
├─ test-runner: ✓ 42 tests pass
├─ lint-checker: ⚠ 2 warnings (non-blocking)
└─ security-scanner: ✓ No vulnerabilities

Ready to ship!
```

### エージェントの活用

エージェントは依頼内容に応じて自動的に起動します：

**「ログインが壊れています。ユーザーが 401 エラーになります」と言った場合:**
```
[debugger agent activated]
→ 認証ミドルウェアを確認中... 問題を発見
→ 本番環境でトークンの検証に使用するシークレットが間違っています
→ 修正: .env.production の AUTH_SECRET を更新してください
```

**「変更をレビューして」と言った場合:**
```
[code-reviewer agent activated]
→ 変更された 3 つのファイルを分析中...
✓ ロジックは正しいです
⚠ 42 行目で null チェックが漏れています
⚠ このエンドポイントにレート制限を追加することを検討してください
```

**「API に認証を追加して」と言った場合:**
```
[orchestrator agent activated]
→ サブタスクに分解中:
  1. 認証スキーマを設計 (architect を起動)
  2. JWT ミドルウェアを実装
  3. ログイン/登録エンドポイントを追加
  4. テストを作成 (test-architect を起動)
  5. API ドキュメントを更新 (docs-writer を起動)
```

### スキルの活用

スキルは関連する場面で自動的に適用されます：

**「支払いサービスの構造はどうすべき？」と尋ねた場合:**
```
[designing-architecture skill applied]
→ ヘキサゴナルアーキテクチャを推奨
→ 支払いプロバイダーをアダプターとして実装
→ コアドメインをインフラストラクチャから分離
```

**「このエンドポイントを高速化して」と尋ねた場合:**
```
[optimizing-performance skill applied]
→ データベースインデックスを追加
→ レスポンスキャッシュを実装
→ 大量のリクエストに対してページネーションを使用
```

---

## コマンドリファレンス

すべてのコマンドは `/project-starter:<コマンド>` の形式で使用します。

### 出力スタイル

| コマンド | モード |
|---------|------|
| `/project-starter:architect` | システム設計モード - コードの前に設計を行う |
| `/project-starter:rapid` | 高速開発モード - 素早くリリースし、反復する |
| `/project-starter:mentor` | ティーチングモード - 「なぜ」を説明する |
| `/project-starter:review` | コードレビューモード - 厳格な品質管理 |

### Git ワークフロー

| コマンド | 目的 |
|---------|---------|
| `/project-starter:commit` | Conventional Commits 形式のメッセージを自動的に生成 |
| `/project-starter:commit-push-pr` | コミット → プッシュ → PR作成をワンストップで実行 |
| `/project-starter:quick-fix` | 糸口（Lint/型エラー）を素早く修正 |
| `/project-starter:add-tests` | 最近の変更に対するテストを生成 |
| `/project-starter:lint-fix` | すべての Lint エラーを自動修正 |
| `/project-starter:sync-branch` | メインブランチと同期（リベースまたはマージ） |
| `/project-starter:summarize-changes` | スタンドアップ用または PR 用のサマリーを生成 |

### 検証 (Verification)

| コマンド | 目的 |
|---------|---------|
| `/project-starter:verify-changes` | 複数のサブエージェントによる多角的な検証 |
| `/project-starter:validate-build` | ビルドプロセスの検証 |
| `/project-starter:run-tests` | 階層的なテスト実行 |
| `/project-starter:lint-check` | コード品質のチェック |
| `/project-starter:security-scan` | セキュリティ脆弱性の検出 |
| `/project-starter:code-simplifier` | 実装後のコードクリーンアップ |

---

## エージェント (Agents)

エージェントは、Claude がタスクに応じて自動的に起動する専門のサブエージェントです。

| エージェント | 役割 | 自動起動のトリガー |
|-------|---------|---------------|
| `orchestrator` | 複数ステップのタスクを調整 | "改善", "リファクタリング", 複数モジュールの変更 |
| `code-reviewer` | コード品質のレビュー | コード変更後、コミット前 |
| `debugger` | 系統的なバグ調査 | エラー、テスト失敗、クラッシュ |
| `docs-writer` | テクニカルドキュメント作成 | README, APIドキュメント, ガイド |
| `security-auditor` | 脆弱性検出 | 認証, ユーザー入力, 機密データ |
| `refactorer` | コード構造の改善 | 技術的負債, クリーンアップ |
| `test-architect` | テスト戦略の設計 | テストの追加・改善 |

---

## スキル (Skills)

スキルは、関連するコンテキストにおいて Claude が自律的に使用する知識ドメインです。

| スキル | ドメイン |
|-------|--------|
| `analyzing-projects` | コードベースの構造とパターンの理解 |
| `designing-tests` | ユニット、統合、E2Eテストのアプローチ |
| `designing-architecture` | クリーンアーキテクチャ、ヘキサゴナル等 |
| `optimizing-performance` | アプリケーションの高速化、ボトルネックの特定 |
| `managing-git` | バージョン管理、Conventional Commits |
| `designing-apis` | REST/GraphQL パターンとベストプラクティス |

---

## フック (Hooks)

フックは、特定のイベントで自動的に実行されます。

| フック | トリガー | アクション |
|------|---------|--------|
| セキュリティスキャン | 編集・保存 | 機密情報を含むコミットをブロック |
| ファイル保護 | 編集・保存 | ロックファイル, .env, .git への編集をブロック |
| 自動フォーマット | 編集・保存 | ファイル形式に応じた Prettier/Black/Gofmt の実行 |
| コマンドログ | Bash | `.claude/command-history.log` への記録 |
| 環境チェック | セッション開始 | Node.js, Python, Git のバージョン検証 |
| プロンプト分析 | ユーザープロンプト | 適切なエージェントの提案 |
| 入力通知 | 入力待ち | デスクトップ通知 |
| 完了通知 | タスク完了 | デスクトップ通知 |

---

## 設定 (Configuration)

### プロジェクトへの権限追加

権限テンプレートをプロジェクトにコピーします：

```bash
mkdir -p /path/to/your/project/.claude
cp templates/settings.local.json.template /path/to/your/project/.claude/settings.local.json
```

これにより、一般的な安全なコマンドが事前に許可され、毎回プロンプトが表示されるのを防ぎます。

### チーム規約の追加

CLAUDE.md テンプレートをプロジェクトのルートにコピーします：

```bash
cp templates/CLAUDE.md.template /path/to/your/project/CLAUDE.md
```

その後、以下の項目をカスタマイズしてください：
- パッケージマネージャーのコマンド
- テスト/ビルド/Lint コマンド
- コード規約
- アーキテクチャの方針

### MCP サーバー

一般的な MCP サーバーの設定については [mcp-servers-template.md](./mcp-servers-template.md) を参照してください。

---

## プラグインの拡張

### カスタムコマンドの追加

`commands/` に `.md` ファイルを作成します：

```markdown
---
allowed-tools: Bash(git:*), Read, Write
description: コマンドの説明
argument-hint: [オプション引数]
---

[コマンドの指示をここに記述]
```

---

## プラグインの構造

```
claude-workflow/
├── .claude-plugin/
│   ├── plugin.json           # 必須: プラグインマニフェスト
│   └── marketplace.json      # 任意: マーケットプレイスのメタデータ
├── agents/                   # 7つの専門エージェント
├── commands/                 # 17のスラッシュコマンド
├── skills/                   # 6つの知識ドメイン
├── hooks/
│   ├── hooks.json            # フック設定
│   └── scripts/              # 8つの自動化スクリプト
├── templates/                # テンプレート
├── CLAUDE.md                 # 開発ガイドライン
└── README.md
```

---

## 必要条件

- **Claude Code** v1.0.33 以降
- **Python 3** (フックスクリプト用)
- **Node.js** (任意、npmコマンド用)
- **Git** (バージョン管理機能用)

---

## コントリビューション

貢献をお待ちしております！詳細は [CONTRIBUTING.md](./CONTRIBUTING.md) をご覧ください。

---

## ライセンス

MIT - 詳細は [LICENSE](./LICENSE) をご覧ください。
