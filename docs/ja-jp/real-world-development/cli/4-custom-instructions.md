---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント作成の規約を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合、その指針を Copilot が参照できるようにします。[命令ファイル][instruction-files]には、作成するコードの内容だけでなく、その構成方法も記述できます。フィルタリング機能を実装したので、Copilot が参照した命令を確認し、ドキュメント作成の規約を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令ファイルと、パスを対象にした命令ファイルがエージェントに読み込まれる方法を確認します。
- 命令ファイルを更新し、コーディング規約に従うようにします。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードを言い換えるのではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TypeScript Documentation (TSDoc) / JavaScript Documentation (JSDoc) で説明します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードを変更したときにコメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件に関するコンテキストと設定を Copilot に伝えられます。好みの規約、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコード生成に役立ちます。リポジトリ全体に適用する命令と、特定のファイル形式やタスクに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡される単一のファイルです。技術スタック、プロジェクトの概要、一般的なリクエストに関係するベストプラクティスなど、プロジェクト全体に適用する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のプログラミング言語、ファイル形式、タスクに関するガイドラインを記載します。

> [!NOTE]
> ほかの形式の命令ファイルとそのサポート状況は、環境によって異なります。形式に依存する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトにはすでに命令ファイルがあります。変更を加える前に内容を確認し、命令の影響を確かめましょう。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令の適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。命令を小さく再利用しやすいファイルに分け、コード生成の参考例を指定できます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは有用ですが、改善できる点があります。`copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しを探します。およそ 35 行目にあります。
2. そのセクションの最後に、次の箇条書きを追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は、会話の開始時にリポジトリの命令を読み込みます。フィルタリング機能のコンテキストを保ったまま、新しい命令を読み込むため、会話を終了して再開します。

1. フィルタリングの会話をまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにいることを確認し、リポジトリのルートから保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

命令ファイルから Copilot がコンテキストを読み取る方法を確認し、新しい規約を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの命令ファイルとパスを対象にした命令ファイルがエージェントに読み込まれる方法を確認しました。
- 命令ファイルを更新し、コーディング規約に従うようにしました。
- 命令ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令作成のベストプラクティス][custom-instructions-support]
- [カスタム命令作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
