# 4. GitHub Actionsによる自動デプロイ

`main` ブランチにpushするたびに自動でビルド・公開されるよう、
GitHub Actionsのワークフローを用意しました。

`.github/workflows/deploy.yml`:

```yaml
name: Deploy MkDocs site

on:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - run: pip install mkdocs mkdocs-material
      - run: mkdocs gh-deploy --force
```

## つまずいた点：workflowスコープ不足

最初のpush時、以下のエラーが出ました。

```
! [remote rejected] HEAD -> main (refusing to allow an OAuth App to create or
update workflow `.github/workflows/deploy.yml` without `workflow` scope)
```

`gh` のトークンに `workflow` スコープが無いことが原因でした。スコープを追加して再認証します。

```bash
gh auth refresh -h github.com -s workflow
```

再度ブラウザでワンタイムコードを入力して承認すれば、pushできるようになります。

```bash
git push -u origin main
```

## 実行結果の確認

```bash
gh run list --limit 1
```

`conclusion: success` になればデプロイ成功です。
