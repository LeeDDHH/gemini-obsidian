## Gemini CLI ワークフロー定義

私が定義したカスタムコマンドで、特定のワークローを実行させるためのルールです。

### `/workflow:create_literature_note_from_daily`

**目的**: Daily Noteに記載されたURLから情報を取得し、要約付きの新しいLiteratureNoteを作成します。

**アクション**:
1. `Daily/`フォルダ内のすべての`.md`ファイルを検索します。
2. ファイル内からURLを正規表現で抽出します。
3. 抽出したURLのWebページ内容を取得し、AIがその内容を日本語で要約します。
4. 取得・要約した内容を元に、`LiteratureNote/`に新しいノートを作成します。
5. 元のDaily Noteから処理済みのURLを削除します。

### `/workflow:draft_permanent_note`

**目的**: `FleetingNote`と`LiteratureNote`を横断的に分析し、PermanentNoteの草案を作成することで、知的生産を加速させます。

**アクション**:
1. `FleetingNote/`と`LiteratureNote/`内のすべての`.md`ファイルを読み込みます。
2. 内容をAIが横断的に分析し、関連性の高いテーマやアイデアのクラスター（集合）を特定します。
3. 私（ユーザー）に、どのテーマについて深掘りしたいか、選択を促します。
4. 選択されたテーマに基づき、関連ノートへのリンクや発展的な問いを含んだPermanentNoteの草案を`PermanentNote/`に作成します。

### `/workflow:update_index_note`

**目的**: 新しく作成されたPermanentNoteを関連する`IndexNote`に追記し、知識体系の鮮度を保ちます。

**アクション**:
1. `IndexNote/`フォルダ内にあるメインの索引ノート（例: `Index.md`）を読み込みます。
2. 新しいPermanentNoteへのリンクを、文脈に応じて適切な場所に追加します。