---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針に合意し、Autopilot で Issue に基づくフィルタリング機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えます。これから複数のレッスンにわたり、Issue を起点に機能を作成し、コードが有効で期待どおりに動作することを確認して、最終的にマージする一般的なエージェント駆動型の開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、このワークショップでは中心的な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能開発を続けながら、リポジトリの命令を更新し、既存の `quality-checks` skill をカスタマイズし、Model Context Protocol (MCP) による検証を追加して品質保証 (QA) エージェントを作成し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリーや発行元で絞り込めるようにする必要があります。バックログの Issue には機能の説明がありますが、カテゴリーの組み合わせなど、実装前に合意しておきたい詳細もあります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本的な手順は変わりません。むしろ、重要性は増します。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した Issue を起票します。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの開発フローはこの手順を応用したものです。

この標準的なアプローチに従うことで、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューと検証のプロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが一緒に作業します。エージェントは変更案を提示し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成し、プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了したと判断するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot に切り替えて実装します。

## Issue から作業を始める

フィルタリング機能に取り組む前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動してから続けます。
2. スター評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能実装やタスクの実行を Copilot に依頼する前に、そのアプローチを決められます。複雑な作業では、少し時間をかけて計画を立てると効果的です。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後ろに、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、Autopilot を使って実装します。

Autopilot では、Copilot が問題を解決したと判断するまで、作業を反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示される同様の名前の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。事前に計画をレビューしてください。依存関係が不足している場合やポートが競合している場合は、チェック完了と判断する前に問題を解決します。

## 実装をレビューして検証する

コードの生成後は、ほかのコードと同様に、マージする前にレビューします。実装を確認し、サイトを起動して問題がないかを確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 変更内容を Issue と計画時の決定事項に照らし合わせます。
4. コードレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードが適切でも、実際に動くかどうかを確認します。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリー、発行元、およびその組み合わせで絞り込みます。
4. リセット時と結果がない場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って、機能を構築してレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、生成されたコードが文書化された規約に従うよう、[カスタム命令を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
