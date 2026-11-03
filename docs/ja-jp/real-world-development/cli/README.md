---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、GitHub Copilot をターミナルで使えるエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンドラインから作業できるため、グラフィカル エディターに切り替えずに開発を続けられます。

このワークショップでは、まず Copilot CLI をインストールして認証し、小さな変更を加えて最初のプルリクエスト (PR) をマージします。次に、GitHub issue を起点に Plan モードで要件を決め、Autopilot モードでフィルター機能を実装し、Interactive モードでレビューします。その後、リポジトリの命令を更新し、既存の `quality-checks` skill をカスタマイズして、Playwright Model Context Protocol (MCP) サーバーによるブラウザー確認と quality assurance (QA) エージェントによる検証を行います。最後に、変更をまとめて PR を作成し、Agent Merge を使います。コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドも確認します。[Foundry を組み込むオプションの 3 モジュール][foundry]では、モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、準備状況を確認する |
| [2. スター評価を追加する][ex2] | 最初の変更 | 既存の評価と未評価時の表示を確認し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で実装して Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認して更新し、フィルター機能に適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認し、結果のレポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、ブラウザーでフィルター機能を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門のプロファイルを作成して選択し、最終確認の証拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の便利なスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、任意のクラウドへの委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] インストール済みで設定済みの Git

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
