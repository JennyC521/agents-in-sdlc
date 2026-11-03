---
title: "レッスン 7 - 品質保証エージェントを作成して使う"
description: "Issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、これらの機能を明確な品質保証 (quality assurance、QA) の手順を持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが命令、skill、MCP ツールと連携する方法を確認します。
- 再利用可能な quality assurance プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルにはエージェントの目的、命令、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの規約を説明します。`quality-checks` skill は繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー操作ツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告するよう Copilot に指示します。QA プロファイルは既存の機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を作成する前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。このプロファイルでは、QA が実施するチェックと、その作業上の制約を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトでカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

QA エージェントを使う前に、意図した QA の手順と制約がプロファイルに含まれていることを確認します。確認を依頼しているのに、プロファイルの範囲が不完全だったり広すぎたりして、機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略できますが、設定するとエージェントの表示名が明確になります。
3. プロファイルの命令を読み、QA が要件を確認し、リポジトリの命令に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットせず PR も作成しないことを確認します。
5. 必要な役割や制約が不足している場合は、先に既定のエージェントに修正を依頼します。

## Issue を QA に確認させる

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開して QA を選択すると、それまでの Issue と計画の内容を利用できます。

1. 次のコマンドを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントが選択肢に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 作業が終わったら、結果のレポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その作業結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが命令、skill、MCP ツールと連携する方法を確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートがそろいました。次は、これらをまとめて[機能 PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
