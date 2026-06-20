---
title: "7. サイトデザインのカスタマイズ"
tags:
  - デザイン
  - ナビゲーション
status: "完了"
---

# 7. サイトデザインのカスタマイズ

=== "本文"

    ここまでで「公開する仕組み」は完成しました。最後に、GitHub Pagesで公開した
    サイトを実際に読みやすく・使いやすくするために行ったカスタマイズをまとめます。
    すべて `mkdocs.yml` の設定だけで実現できます。

    ## 7-1. テーマ・配色・ロゴ

    ```yaml title="mkdocs.yml"
    theme:
      name: material
      language: ja
      logo: images/logo.png
      favicon: images/favicon.png
      palette:
        - media: "(prefers-color-scheme: light)"
          scheme: default
          primary: indigo
          accent: indigo
          toggle:
            icon: material/weather-night
            name: ダークモードに切り替え
        - media: "(prefers-color-scheme: dark)"
          scheme: slate
          primary: indigo
          accent: indigo
          toggle:
            icon: material/weather-sunny
            name: ライトモードに切り替え
    ```

    - `palette` を2つ並べると、OSのダーク/ライト設定に自動追従しつつ、
      読者がヘッダーのアイコンで手動切り替えもできるようになります
    - `logo` / `favicon` はリポジトリ内の画像ファイルを指定するだけで反映されます
      （`docs/images/` に置いて相対パスで指定）

    ## 7-2. ナビゲーション構成

    最初はヘッダーに章タブを表示する `navigation.tabs` を試しましたが、
    **章が多いと読みづらい** という理由でやめ、左サイドバー中心の構成に変更しました。

    ```yaml title="mkdocs.yml"
    theme:
      features:
        - navigation.top       # スクロール時に上に戻るボタン
        - navigation.expand    # サイドバーを最初から展開表示
        - navigation.sections  # トップレベルをセクション見出しとして表示
        - navigation.tracking  # URLにスクロール位置を反映
        - navigation.indexes   # セクションにトップページを持たせられる
        - navigation.path      # ブレッドクラム（現在位置）を表示
        - navigation.footer    # ページ下部に「前へ/次へ」リンクを表示
        - navigation.instant           # ページ遷移をその場で差し替え（高速化）
        - navigation.instant.progress  # 遷移中に進捗バーを表示
    ```

    `nav:` 側もフラットな一覧から、章をグループ化した構成に変更しています。

    ```yaml title="mkdocs.yml"
    nav:
      - ホーム: index.md
      - セットアップ:
          - 1. Python環境の準備: 01-python-setup.md
          - 2. mkdocsのインストールとサイト作成: 02-mkdocs-install.md
      - GitHub公開:
          - 3. GitHubリポジトリの作成とpush: 03-github-repo.md
          - 4. GitHub Actionsによる自動デプロイ: 04-github-actions.md
          - 5. GitHub Pagesの公開設定: 05-github-pages.md
      - ローカル開発:
          - 6. ローカルでのプレビュー方法: 06-local-preview.md
    ```

    グループ化のメリット：

    - サイドバーに見出しが出て、どのフェーズの作業か一目でわかる
    - 章が増えても、グループごとに整理されるので散らからない
    - ブレッドクラムと組み合わせると「ホーム / GitHub公開 / 5. GitHub Pages...」のように
      現在地が常にわかる

    ## 7-3. コードブロックの分割ルール

    複数コマンドを1つのブロックにまとめていた箇所は、**1コマンド = 1ブロック** に分割しました。

    ```bash title="変更前（コピーすると3行まとめて貼り付けられてしまう）"
    git add -A
    git commit -m "更新内容のメモ"
    git push
    ```

    ```bash title="変更後（1行ずつコピーできる）"
    git add -A
    ```

    ```bash
    git commit -m "更新内容のメモ"
    ```

    ```bash
    git push
    ```

    これは `content.code.copy` 機能（各コードブロック右上に表示されるコピー
    アイコン）と組み合わせて初めて効果を発揮します。1ブロックにまとめたままでは、
    読者が「1行だけ実行したい」場合でも3行まとめてコピーされてしまうためです。

    ただし、PowerShellのパイプライン処理のように **本質的に1つの処理** のものは、
    あえて分割せずそのまま1ブロックにしています。分割の基準は
    「読者が実行するタイミングが分かれるかどうか」です。

    ## 7-4. 変数・再利用コンテンツ（macros / snippets）

    GitHubユーザー名やリポジトリ名のような「サイト内で何度も出てくる値」は、
    `mkdocs.yml` の `extra:` に変数として1か所だけ定義し、各ページからは
    `{% raw %}{{ extra.github_user }}{% endraw %}` のように参照しています
    （`mkdocs-macros-plugin` が必要です）。

    ```yaml title="mkdocs.yml"
    extra:
      github_user: okakaLDS
      repo_name: my-docs-site
      repo_url: https://github.com/okakaLDS/my-docs-site
      site_base_url: https://okakalds.github.io/my-docs-site/

    plugins:
      - macros
    ```

    また、複数ページで同じ注意書き（例: `workflow` スコープが必要という警告）を
    繰り返し書いていた箇所は、`pymdownx.snippets` で1つのファイルに切り出し、
    各ページから読み込む形に変更しました。

    ```yaml title="mkdocs.yml"
    markdown_extensions:
      - pymdownx.snippets:
          base_path:
            - snippets
          check_paths: true
    ```

    ```markdown title="呼び出し側のページ"
    --8<-- "workflow-scope-warning.md"
    ```

    どちらも「同じ内容を複数箇所に書かない」ための仕組みです。文章を直すときに
    1か所直し忘れて内容がズレる、という事故を防げます。

    ## 7-5. まとめ：GitHub Pages活用の観点で意識したこと

    | 工夫 | 効果 |
    |---|---|
    | ダーク/ライト切り替え | 読者の環境・好みに合わせて見やすさを確保 |
    | ロゴ・favicon | ブックマークやタブで見分けやすくなる |
    | セクション分けしたナビゲーション | 章が増えても迷わない設計に |
    | ブレッドクラム・前後リンク | 読者が「今どこにいるか」を見失わない |
    | コードブロックの分割 | コピペ作業時のミス・余計な手間を減らす |
    | macros / snippets | 同じ値・同じ注意書きを1か所で管理し、修正漏れを防ぐ |

    これらはすべて `mkdocs.yml` を編集して `git push` するだけで反映されます
    （[4. GitHub Actionsによる自動デプロイ](../02-github-publish/04-github-actions.md)の仕組みがそのまま使えます）。

=== "改定履歴"

    | 更新日 | 更新者 | 更新内容 |
    |---|---|---|
    | 2026-06-20 | 岡洋介 | 初版作成 |
