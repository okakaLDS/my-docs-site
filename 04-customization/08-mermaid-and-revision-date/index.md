# 8. 図解(Mermaid)と改定履歴

基本設計書をMarkdownで書く場合、シーケンス図やER図などの「図」が必須になります。 ここでは、画像ファイルを使わずテキストだけで図を描ける **Mermaid** の使い方と、 各ページの「本文」「改定履歴」をタブで分けて管理する方法を説明します。

## 8-1. Mermaidとは

Markdown内に ```` ```mermaid ```` というコードブロックを書くだけで、ビルド時に図として描画される機能です。 画像ではなく**テキストなのでGitで差分管理できる**のが最大の利点です。

mkdocs.yml

```
markdown_extensions:
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
```

## 8-2. 書き方の例

### シーケンス図

````
```mermaid
sequenceDiagram
    participant User
    participant API
    participant DB
    User->>API: リクエスト送信
    API->>DB: クエリ実行
    DB-->>API: 結果返却
    API-->>User: レスポンス
````

```

実際の描画結果：

```

sequenceDiagram participant User participant API participant DB User->>API: リクエスト送信 API->>DB: クエリ実行 DB-->>API: 結果返却 API-->>User: レスポンス

```

### ER図

```

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    ORDER ||--|{ ORDER_ITEM : contains
    PRODUCT ||--o{ ORDER_ITEM : includes
```

```

```

erDiagram USER ||--o{ ORDER : places ORDER ||--|{ ORDER_ITEM : contains PRODUCT ||--o{ ORDER_ITEM : includes

```

### アーキテクチャ・フローチャート

```

```mermaid
flowchart LR
    A[クライアント] --> B[APIサーバー]
    B --> C[(データベース)]
    B --> D[外部API]
```

```

```

flowchart LR A[クライアント] --> B[APIサーバー] B --> C[(データベース)] B --> D[外部API]

```

他にも状態遷移図(`stateDiagram-v2`)・ガントチャート(`gantt`)・クラス図(`classDiagram`)などが書けます。

## 8-3. 本文と改定履歴をタブで分ける

最初はGitの履歴から「最終更新日」を自動表示する仕組み
（`mkdocs-git-revision-date-localized-plugin`）を試しましたが、
**誰が・何を変えたか** まではGitの履歴だけでは一覧しにくいため、
各ページに「更新日・更新者・更新内容」を手動で記録する **改定履歴表** に変更しました。

`pymdownx.tabbed` を使い、各ページを「本文」「改定履歴」の2タブで構成します。

mkdocs.yml

```

markdown_extensions:

- pymdownx.tabbed: alternate_style: true

```

ページの基本構成

```

# ページタイトル

=== "本文"

```
本文の内容...
```

=== "改定履歴"

```
| 更新日 | 更新者 | 更新内容 |
|---|---|---|
| 2026-06-20 | 岡洋介 | 初版作成 |
```

````

タブの中はすべて4スペースでインデントする必要があります。コードブロック（フェンス）や
admonition（`???`/`!!!`）もそのまま入れ子にできます。

## トラブルシューティング

タブの中の見出しやコードブロックが認識されない

タブの中身（`=== "本文"` の次の行から）はすべて4スペースのインデントが必要です。
見出し・コードブロック・表など、インデントが揃っていないと、タブの外側の
通常のMarkdownとして解釈されてしまいます。


Mermaidの図が描画されずコードのまま表示される

`mkdocs.yml` の `pymdownx.superfences` に `custom_fences` の設定が入っているか確認してください。
また、Material for MkDocsのバージョンが古いと対応していない場合があるので
`pip install -U mkdocs-material` で更新してみてください。

| 更新日 | 更新者 | 更新内容 |
| --- | --- | --- |
| 2026-06-20 | 岡洋介 | 初版作成 |```
````
