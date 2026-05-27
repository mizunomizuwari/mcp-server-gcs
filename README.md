# gcs-mcp-server

Google Cloud Storage を操作する MCP サーバーです。

## 提供ツール

| ツール | 説明 |
|---|---|
| `list_buckets` | バケット一覧を取得 |
| `list_files` | バケット内のファイル一覧を取得 |
| `read_file` | テキストファイルの内容を読み込む |
| `get_file_info` | ファイルのメタデータを取得 |
| `upload_text` | テキストをファイルとしてアップロード |
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
| `roles/storage.objectViewer` | 読み取りのみ |
| `roles/storage.objectUser` | 読み取り＋書き込み＋削除 |
