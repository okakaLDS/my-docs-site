# 6. ローカルでのプレビュー方法

公開前に変更を確認したい場合は `mkdocs serve` を使います。

```bash
cd my-docs-site
mkdocs serve
```

実行するとローカルサーバーが立ち上がります。

```
INFO    -  Building documentation...
INFO    -  Documentation built in 0.xx seconds
INFO    -  [hh:mm:ss] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO    -  [hh:mm:ss] Serving on http://127.0.0.1:8000/
```

ブラウザで `http://127.0.0.1:8000/` を開くとサイトが見られます。
`docs/` 内のMarkdownファイルを編集すると自動で再ビルドされ、ブラウザも自動更新されます（ライブリロード）。

終了するときは、コマンドを実行しているターミナルで `Ctrl + C` を押します。

## ポートが使われている場合

別のポートを指定できます。

```bash
mkdocs serve -a 127.0.0.1:8001
```
