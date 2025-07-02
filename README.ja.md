# Gemini Obsidian ナレッジ管理Vault

このリポジトリには、Zettelkastenメソッドに触発された原則に従って、個人のナレッジ管理を効率化するために設計された、カスタムGemini CLIと統合されたObsidian Vaultが含まれています。Obsidianのノート作成と整理の機能を活用し、Gemini CLIを介した自動化されたワークフローによって、情報の処理と洞察の生成を強化します。

## 機能

このVaultには、ナレッジ管理プロセスを自動化および強化するためのカスタムGemini CLIワークフローが装備されています。

-   **`/workflow:create_literature_note_from_daily`**: Daily NotesからURLを自動的に抽出し、Webページコンテンツを取得し、AIを使用して要約し、新しいLiterature Notesを作成し、処理済みのURLをDaily Noteからクリーンアップします。
-   **`/workflow:draft_permanent_note`**: Fleeting NotesとLiterature Notesを分析して関連するテーマやアイデアを特定し、テーマの選択を支援し、関連するノートへのリンクと思考を刺激する質問を含むPermanent Noteの草案を作成します。
-   **`/workflow:update_index_note`**: 新しく作成されたPermanent NotesへのリンクをメインのIndex Noteに自動的に追加することで、ナレッジベースを整理された状態に保ちます。

## ディレクトリ構造

-   `Daily/`: 日々のノート。多くの場合、最初の考えやキャプチャされたURLが含まれます。
-   `FleetingNote/`: 即座のアイデアをキャプチャする、未精製のクイックノート。
-   `LiteratureNote/`: 外部ソース（書籍、記事、Webページ）からの要約と洞察。
-   `PermanentNote/`: コアコンセプトとアイデアを表す、洗練されたアトミックなノート。
-   `IndexNote/`: Permanent Notesとナレッジベースをナビゲートするためのマップとインデックス。
-   `.gemini/`: Gemini CLIの設定ファイル。
-   `.obsidian/`: Obsidianの設定ファイル。

## はじめに

### 前提条件

-   [Obsidian](https://obsidian.md/) (デスクトップアプリケーション)
-   Gemini CLI (このプロジェクトは、Gemini CLIがこのVaultと対話するように設定されていることを前提としています。)

### インストール

1.  **このリポジトリをクローンします。**
    ```bash
    git clone https://github.com/your-username/gemini-obsidian.git
    cd gemini-obsidian
    ```
2.  **ObsidianでVaultを開きます。**
    -   Obsidianを開きます。
    -   「別のVaultを開く」->「フォルダをVaultとして開く」をクリックします。
    -   クローンした`gemini-obsidian`ディレクトリに移動して選択します。
3.  **Gemini CLIを設定します。**
    -   Gemini CLIがこのディレクトリを指していることを確認します。`.gemini/settings.json`ファイルは基本的な設定のためにすでに存在します。`GEMINI.md`で定義されているカスタムワークフローを認識して実行するように、Gemini CLIの環境または設定を調整する必要がある場合があります。

## 使用方法

設定が完了すると、手動でのノート作成にはObsidianを、自動化されたタスクにはGemini CLIを使用して、ナレッジベースと対話できます。

ワークフローをトリガーするには、Gemini CLI環境内でカスタムコマンドを使用します。

-   `workflow:create_literature_note_from_daily`
-   `workflow:draft_permanent_note`
-   `workflow:update_index_note`

（これらのカスタムコマンドの実行方法については、Gemini CLIのドキュメントを参照してください。）

## 貢献

貢献を歓迎します！新しいワークフロー、改善、またはバグ修正のアイデアがある場合は、課題を開くか、プルリクエストを送信してください。

## ライセンス

[オプション：ここにライセンス情報を追加します。例：MITライセンス]
