---
title: "3. GitHubリポジトリの作成とpush"
tags:
  - GitHub公開
  - Git
status: "完了"
---

# 3. GitHubリポジトリの作成とpush

=== "本文"

    ## 3-1. 準備するもの

    - GitHubアカウント（無ければ https://github.com で無料登録）
    - Git（Windowsの場合は事前に入っていることが多いです。`git --version` で確認）

    ## 3-2. GitHub CLI (gh) のインストール

    ブラウザでポチポチ操作する代わりに、コマンドでリポジトリ作成〜pushまで一気に行うため
    `gh` コマンドを使います。

    ```bash
    winget install -e --id GitHub.cli --source winget --accept-package-agreements --accept-source-agreements
    ```

    新しいターミナルで確認：

    ```bash
    gh --version
    ```

    ## 3-3. GitHubへログインする

    ```bash
    gh auth login --hostname github.com --git-protocol https --web
    ```

    実行すると、ターミナルに次のような表示が出ます。

    ```
    ! First copy your one-time code: XXXX-XXXX
    Open this URL to continue in your web browser: https://github.com/login/device
    ```

    1. `https://github.com/login/device` を**ブラウザで**開く
    2. 表示された8桁のワンタイムコード（`XXXX-XXXX`）を入力
    3. 「Authorize」を押して連携を許可する

    ターミナルに戻り、ログイン成功を確認します。

    ```bash
    gh auth status
    ```

    ```
    ✓ Logged in to github.com account あなたのユーザー名 (keyring)
    ```

    次の章でGitHub Actionsの設定ファイル（`.github/workflows/*.yml`）をpushしますが、
    その際 `workflow` スコープが必要になるので、先に追加しておくとスムーズです。

    --8<-- "workflow-scope-warning.md"

    ## 3-4. ローカルのプロジェクトをGitリポジトリ化する

    `my-docs-site` フォルダの中で実行します。

    ```bash
    cd my-docs-site
    ```

    ```bash
    git init
    ```

    ```bash
    git add -A
    ```

    ```bash
    git commit -m "Initial mkdocs site"
    ```

    - `git init` : このフォルダをGit管理対象にする（`.git` フォルダが作られる）
    - `git add -A` : 変更した全ファイルをコミット対象に追加
    - `git commit` : その時点の状態を1つの記録（スナップショット）として保存

    ## 3-5. GitHub上にリポジトリを作成してpush

    `gh repo create` 1コマンドで「リポジトリ作成」「リモート登録」「push」を同時に行えます。

    ```bash
    gh repo create my-docs-site --public --source=. --remote=origin --push
    ```

    - `--public` : 誰でも見られる公開リポジトリにする（GitHub Pagesの無料利用条件）
    - `--source=.` : 今いるフォルダの内容を使う
    - `--remote=origin` : このフォルダに `origin` という名前でリモート登録する
    - `--push` : 作成と同時にpushまで行う

    成功すると、リポジトリURLが表示されます。

    ```
    https://github.com/あなたのユーザー名/my-docs-site
    ```

    ブラウザで開いて、ファイルが反映されていることを確認しましょう。

    ## トラブルシューティング

    ??? note "`refusing to allow an OAuth App to create or update workflow ... without workflow scope`"
        `gh` のトークンに `workflow` スコープが無いのが原因です。3-3の最後で案内した
        `gh auth refresh -h github.com -s workflow` を実行し、再度pushしてください。

    ??? note "`gh repo create` で `already exists` と言われる"
        同名のリポジトリが既にGitHub上にある場合です。別名にするか、
        既存リポジトリを削除/利用する必要があります（削除は元に戻せないので要注意）。

=== "改定履歴"

    | 更新日 | 更新者 | 更新内容 |
    |---|---|---|
    | 2026-06-20 | 岡洋介 | 初版作成 |
