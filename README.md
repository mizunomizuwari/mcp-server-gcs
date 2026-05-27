# gcs-mcp-server

Google Cloud Storage を操作する MCP サーバーです。

## 提供ツール

| ツール | 説明 |
|---|---|
| `list_buckets` | バケット一覧を取得 |
| `list_files` | バケット内のファイル一覧を取得 |
| `read_file` | テキストファイルの内容を読み込む |
| `read_pptx` | PowerPoint（.pptx）のスライドテキストを読み込む |
| `get_file_info` | ファイルのメタデータを取得 |
| `upload_text` | テキストをファイルとしてアップロード |
| `download_file` | GCSのファイルをローカルパスに保存 |
| `copy_file` | ファイルをコピー |
| `delete_file` | ファイルを削除 |

## Claude Desktop への設定

`~/Library/Application Support/Claude/claude_desktop_config.json` に追記します。

```json
{
  "mcpServers": {
    "gcs": {
      "command": "uvx",
      "args": [
        "--from",
        "git+https://github.com/mizunomizuwari/mcp-server-gcs.git@main",
        "gcs-mcp-server",
        "--project", "your-gcp-project-id",
        "--key-file", "/path/to/service-account.json"
      ]
    }
  }
}
```

`--key-file` を省略すると Application Default Credentials（ADC）を使用します。

## 必要なIAMロール

| ロール | 用途 |
|---|---|
| `roles/storage.objectViewer` | ファイルの読み取りのみ（`list_files` / `read_file` / `get_file_info`） |
| `roles/storage.objectUser` | ファイルの読み取り＋書き込み＋削除（`upload_text` / `download_file` / `copy_file` / `delete_file`） |
| `roles/storage.admin` | バケット一覧取得（`list_buckets`）を含む全操作 |

> **注意1**: `list_buckets` は `storage.buckets.list` 権限を必要とします。この権限は `roles/storage.objectViewer` / `roles/storage.objectUser` には含まれていません。`list_buckets` を使用する場合は `roles/storage.admin`、またはプロジェクトレベルの `roles/viewer` 以上を付与してください。

