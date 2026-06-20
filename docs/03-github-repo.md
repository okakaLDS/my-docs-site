# 3. GitHubリポジトリの作成とpush

## GitHub CLI (gh) のインストール

リポジトリ操作をコマンドから行うため `gh` を導入しました。

```bash
winget install -e --id GitHub.cli --source winget \
  --accept-package-agreements --accept-source-agreements
```

## ログイン

```bash
gh auth login --hostname github.com --git-protocol https --web
```

表示されたワンタイムコードをブラウザの https://github.com/login/device で入力してログインします。

```bash
gh auth status
```

```
✓ Logged in to github.com account okakaLDS
```

## ローカルリポジトリの初期化とコミット

```bash
git init
git add -A
git commit -m "Initial mkdocs site"
```

## GitHubへリポジトリ作成 + push

```bash
gh repo create my-docs-site --public --source=. --remote=origin --push
```

これで `https://github.com/okakaLDS/my-docs-site` が作成され、コードがpushされます。
