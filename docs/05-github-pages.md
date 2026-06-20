# 5. GitHub Pagesの公開設定

`mkdocs gh-deploy` は `gh-pages` ブランチにビルド結果をpushするだけで、
GitHub Pages機能そのものはまだ有効になっていません。APIで有効化しました。

## 有効化前の確認

```bash
gh api repos/okakaLDS/my-docs-site/pages
```

```
{"message":"Not Found", ...}
```

→ まだPagesが設定されていない状態。

## 有効化

`gh-pages` ブランチをソースとして指定します。

```bash
gh api repos/okakaLDS/my-docs-site/pages -X POST \
  -f "source[branch]=gh-pages" -f "source[path]=/"
```

成功すると `html_url` が返ります。

```
"html_url": "https://okakalds.github.io/my-docs-site/"
```

## 公開確認

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://okakalds.github.io/my-docs-site/
```

`200` が返れば公開完了です。

---

**公開URL: https://okakalds.github.io/my-docs-site/**

以降は `docs/` 配下のMarkdownを編集して `main` にpushすれば、
Actionsが自動でビルド・公開してくれます。
