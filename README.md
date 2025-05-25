# Bocchi the Twitter Database

Twitter風SNSアプリケーションのデータベースマイグレーション管理

## セットアップ

### 前提条件

- MySQL 8.0以上

### ローカル環境
1. データベースプロパティ

```properties
url=jdbc:mysql://localhost:50000/bocchi_the_twitter
username=root
password=password
```

2. データベースを作成
```sql
CREATE DATABASE bocchi_the_twitter;
```
3. マイグレーションの実行
```bash
./gradlew update
```

### Dockerを使う場合

以下のコマンドでMySQLコンテナを直接起動できます：

```bash
docker run -d \
  --name bocchi-the-twitter-db \
  -e MYSQL_ROOT_PASSWORD=password \
  -e MYSQL_DATABASE=bocchi_the_twitter \
  -e TZ=Asia/Tokyo \
  -p 50000:3306 \
  --platform linux/amd64 \
  mysql:8.0.34
```

コンテナ起動後、以下のコマンドでマイグレーションを実行してください：

```bash
./gradlew update
```

## 技術スタック

- Liquibase
- MySQL 8.0

## マイグレーションファイル

マイグレーションファイルは `changelog/` ディレクトリに配置されています。

### 構造
- `changelog-master.xml`: メインのチェンジログファイル
- `changelog/const/`: テーブル定義などの定数ファイル

## 初期データの挿入（MySQL Workbench）

以下3つのファイルを **順番に** 実行してください：

1. `init/init-users.sql`
2. `init/init-tweets.sql`
3. `init/init-user-followers.sql`

> 💡 いずれも `USE bocchi_the_twitter;` を含んでいますので、そのまま実行可能です。
> 外部キー制約があるため、必ずこの順序で実行してください。