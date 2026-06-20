!!! warning "workflowファイルをpushするには `workflow` スコープが必要"
    `.github/workflows/*.yml` をpushする際、`gh` のトークンに `workflow` スコープが
    無いと次のエラーになります。

    ```
    refusing to allow an OAuth App to create or update workflow ... without `workflow` scope
    ```

    次のコマンドでスコープを追加し、ブラウザでワンタイムコードを入力して再認証してください。

    ```bash
    gh auth refresh -h github.com -s workflow
    ```
