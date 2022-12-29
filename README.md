movabletypejp-env
===

[MovableType.jp](https://www.movabletype.jp/) の Docker 環境を作成できるリポジトリです。

## PHP の設定変更について

ドキュメントルート配下に
[`.user.ini`](https://www.php.net/manual/ja/configuration.file.per-user.php)
ファイルを置くことで、PHP の設定を一時的に変更することが可能です。
通常は 5 分間のキャッシュ時間がありますが、この環境ではキャッシュを無効にしています。

変更可能な項目の一覧については
[`php.ini` ディレクティブのリスト](https://www.php.net/manual/ja/ini.list.php)
をご覧ください。
