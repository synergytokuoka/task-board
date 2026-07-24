# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## デプロイ先

https://github.com/synergytokuoka/task-board/

## 技術スタック

- Vite + React 19 + TypeScript
- Lint: oxlint (`npm run lint`)
- スタイリング: プレーンCSS（`App.css` / `index.css`）。ライト/ダーク対応はCSSカスタムプロパティ（`--text`, `--bg`, `--border`, `--accent` 等）を `index.css` の `:root` で定義し、`@media (prefers-color-scheme: dark)` で上書き
- データ永続化: `localStorage`（バックエンドなし）

### 主なコマンド

```
npm install     # 依存関係インストール
npm run dev     # 開発サーバー起動 (http://localhost:5173)
npm run build   # 型チェック(tsc -b) + 本番ビルド
npm run lint    # oxlintによる静的解析
npm run preview # ビルド成果物をローカルでプレビュー
```

## アーキテクチャ

- `src/App.tsx` に状態・ロジック・UIをまとめた単一コンポーネント構成（現状はタスクボードのみのシンプルなアプリのため分割していない）。機能が増えた場合はコンポーネント分割を検討する。
- タスクは `Task { id, text, done }` の配列としてReactの `useState` で管理し、`useEffect` で変更のたびに `localStorage` へ保存する（キー: `task-board.tasks`）。

## 命名規約

- コンポーネント: ファイル名・関数名ともにPascalCase（例: `App.tsx` の `App`）
- 型・インターフェース: PascalCase（例: `Task`）
- イベントハンドラ（JSXに渡す関数）: `handle` + 対象 + 動作（例: `handleAddTask`）。JSX props自体は `onClick` / `onChange` など標準のon-prefixを使う
- 状態を更新するだけの内部関数: 動詞から始める（例: `toggleTask`, `deleteTask`）
- モジュール定数: SCREAMING_SNAKE_CASE（例: `STORAGE_KEY`）
- CSSクラス: kebab-case（例: `task-form`, `task-list`）。状態を表す場合は基本クラスに修飾クラスを併記する（例: 完了タスクは `class="task done"`）

## Git workflow

- コードを変更したら、そのたびに変更をコミットし、GitHubへプッシュすること（作業を溜め込まず、変更単位でこまめにプッシュする）。
- プッシュ前に `git status` / `git diff` で変更内容を確認すること。
- コミットメッセージは変更内容が分かるように簡潔に書くこと。
