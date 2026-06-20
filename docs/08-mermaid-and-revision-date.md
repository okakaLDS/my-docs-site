# 8. 図解(Mermaid)と更新日表示

=== "本文"

    基本設計書をMarkdownで書く場合、シーケンス図やER図などの「図」が必須になります。
    ここでは、画像ファイルを使わずテキストだけで図を描ける **Mermaid** と、
    各ページの「最終更新日」を自動表示する仕組みを導入します。

    ## 8-1. Mermaidとは

    Markdown内に ` ```mermaid ` というコードブロックを書くだけで、ビルド時に図として描画される機能です。
    画像ではなく**テキストなのでGitで差分管理できる**のが最大の利点です。

    ```yaml title="mkdocs.yml"
    markdown_extensions:
      - pymdownx.superfences:
          custom_fences:
            - name: mermaid
              class: mermaid
              format: !!python/name:pymdownx.superfences.fence_code_format
    ```

    ## 8-2. 書き方の例

    ### シーケンス図

    ````markdown
    ```mermaid
    sequenceDiagram
        participant User
        participant API
        participant DB
        User->>API: リクエスト送信
        API->>DB: クエリ実行
        DB-->>API: 結果返却
        API-->>User: レスポンス
    ```
    ````

    実際の描画結果：

    ```mermaid
    sequenceDiagram
        participant User
        participant API
        participant DB
        User->>API: リクエスト送信
        API->>DB: クエリ実行
        DB-->>API: 結果返却
        API-->>User: レスポンス
    ```

    ### ER図

    ````markdown
    ```mermaid
    erDiagram
        USER ||--o{ ORDER : places
        ORDER ||--|{ ORDER_ITEM : contains
        PRODUCT ||--o{ ORDER_ITEM : includes
    ```
    ````

    ```mermaid
    erDiagram
        USER ||--o{ ORDER : places
        ORDER ||--|{ ORDER_ITEM : contains
        PRODUCT ||--o{ ORDER_ITEM : includes
    ```

    ### アーキテクチャ・フローチャート

    ````markdown
    ```mermaid
    flowchart LR
        A[クライアント] --> B[APIサーバー]
        B --> C[(データベース)]
        B --> D[外部API]
    ```
    ````

    ```mermaid
    flowchart LR
        A[クライアント] --> B[APIサーバー]
        B --> C[(データベース)]
        B --> D[外部API]
    ```

    他にも状態遷移図(`stateDiagram-v2`)・ガントチャート(`gantt`)・クラス図(`classDiagram`)などが書けます。

    ## 8-3. 更新日表示の導入

    設計書は更新頻度が高いため、「このページはいつ更新されたか」が一目でわかると、
    レビューする人や生成AIが「最新の情報かどうか」を判断しやすくなります。

    ```bash
    pip install mkdocs-git-revision-date-localized-plugin
    ```

    ```yaml title="mkdocs.yml"
    plugins:
      - search
      - git-revision-date-localized:
          type: datetime
          timezone: Asia/Tokyo
          locale: ja
          fallback_to_build_date: true
    ```

    - `plugins:` を定義すると `search` プラグインが自動では入らなくなるため、明示的に書く必要があります
    - 各ページの最終更新日は、**そのファイルのGitコミット履歴**から取得されます。ローカルでも
      `git log` の情報があれば動作しますが、GitHub Actions環境ではデフォルトで履歴が浅い（最新1件のみ）
      ため、`fetch-depth: 0` を指定して全履歴を取得する必要があります。

    ```yaml title=".github/workflows/deploy.yml"
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0
    ```

    ```yaml title=".github/workflows/deploy.yml"
    - run: pip install mkdocs mkdocs-material mkdocs-git-revision-date-localized-plugin
    ```

    設定後、各ページの下部に「最終更新: 2026-06-20」のように自動表示されます。

    ## トラブルシューティング

    ??? note "ローカルでは更新日が出るのにGitHub Pagesでは出ない"
        `actions/checkout@v4` に `fetch-depth: 0` を指定し忘れている可能性があります。
        デフォルトでは直近1コミットしか取得しないため、履歴を遡れずに正しい日付が出ません。

    ??? note "Mermaidの図が描画されずコードのまま表示される"
        `mkdocs.yml` の `pymdownx.superfences` に `custom_fences` の設定が入っているか確認してください。
        また、Material for MkDocsのバージョンが古いと対応していない場合があるので
        `pip install -U mkdocs-material` で更新してみてください。

=== "改定履歴"

    | 更新日 | 更新者 | 更新内容 |
    |---|---|---|
    | 2026-06-20 | 岡洋介 | 初版作成 |
