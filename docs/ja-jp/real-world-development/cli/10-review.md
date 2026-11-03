---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローを進めました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、準備済みのフィルタリング Issue を見つけました。
- スター評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` skill をカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- quality assurance (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、skill の結果、ブラウザーでの確認内容を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップでは、更新した `main` からそれぞれ独自のブランチを作成し、2 つの PR を完成させます。

1. **スター評価**: ゲームカードに既存の `starRating` と未評価であることを示す表示を追加します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、命令を更新して機能に適用し、`quality-checks` レポートをカスタマイズして、関連テストと QA プロファイルを追加します。

フィルタリングの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を 1 つの PR にまとめています。

## 異なる方法で検証する

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー確認を行いました。`quality-checks` skill では、ユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA では、その結果に要件とテスト範囲のレビューを加えて PR 作成前に確認しました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行でテストを追加する必要がないと判断される場合もあります。マージを許可する前に、コードと検証の根拠を確認し、変更後は影響を受ける検証を再実行してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、命令を更新し、skill をカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。命令は規約を定め、skill は繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。**Plan** は実装前に方針を考えるとき、**Interactive** は範囲を絞った変更に取り組みながら確認を続けるとき、**Autopilot** は範囲が明確なタスクを進めるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

開発環境と同様に、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力が大きく変わります。

## さらに試す

中心的なワークフローは完了しました。次の CLI 機能も試せます。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けることが大切です。業務のコード、趣味のコード、作りたいと思いながら手をつけていなかった小さなアプリなどで試してください。チームと学びを共有し、互いの経験から学びましょう。必要に応じてドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
