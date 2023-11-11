# ステップ2

実際にテーブルを構築し、データを入れましょう。  
その手順をドキュメントとしてまとめてください（アウトプットは手順のドキュメントです）。

具体的には、以下のことを行う手順のドキュメントを作成してください。

1. データベースを構築します  
2. ステップ1で設計したテーブルを構築します。  
3. サンプルデータを入れます。サンプルデータはご自身で作成ください。  
   （ChatGPTを利用すると比較的簡単に生成できます）

手順のドキュメントは、他の人が見た時にその手順通りに実施すればテーブル作成及びサンプルデータ格納が行えるように記載してください。

なお、ステップ2は以下のことを狙っています。

- データを実際に入れることでステップ3でデータ抽出クエリを試せるようにすること。
- 手順をドキュメントにまとめることで、自身がやり直したい時にすぐやり直せること。
- 手順を人が同じように行えるようにまとめることで、ドキュメントコミュニケーション力を上げること。



# ドキュメント
# 1.データベースを構築します

このドキュメントでは、インターネットTVサービスのテーブルを構築する手順を説明します。
以下のステップでデータベースを構築していきます。

1-1.データベース管理システム(DBMS)の選定: 

1-2.データベースの作成: 
    選定したDBMS(今回はMySQL)を使用して、新しいデータベースを作成します。
    これにより、データベースファイルが作成され、データベースを格納するための物理的な場所が確保されます。

1-3.テーブルの設計と作成: データベース内に必要なテーブルを設計し、それを実際に作成します。
    テーブルの設計には、テーブルの名前、カラム（列）の名前、データ型、制約（PRIMARY KEY、FOREIGN KEY、UNIQUE KEYなど）などが含まれます。

1-4.インデックスの作成: データベースのパフォーマンスを向上させるために、クエリの高速化に役立つインデックスを必要な場所に作成します。
    インデックスは検索処理を高速化します。

1-5.関連付けと外部キー制約: テーブル間の関連を設定し、外部キー制約を定義します。これにより、異なるテーブル間でデータの整合性を保ちます。

1-6.データの初期化: テーブルに初期データを挿入します。これには、手動でデータを挿入する作業や、サンプルデータを使用することが含まれます。

1-7.セキュリティ設定: データベースへのアクセス権やセキュリティ設定を適切に設定し、不正アクセスからデータを保護します。

## 1-1.データベース管理システム(DBMS)の選定
 - データベースを構築するために、どのデータベース管理システム（例：MySQL、PostgreSQL、SQLite、Microsoft SQL Serverなど）を使用するか選定します。
 - 選択したDBMSに応じて、そのDBMSに対応したコマンドやツールを使用します。
 - 今回はMySQLを使用します。

## 1-2.データベースの作成

## 前提条件

- MySQLデータベース管理システムがインストールされていること。
- MySQLサーバーが起動していること。

## 手順

1. MySQLにログインします。コマンドラインで以下のコマンドを実行します。
    ユーザー名には、MySQLにアクセスできるユーザー名を指定します。
    パスワードが必要な場合は、プロンプトが表示されるので、パスワードを入力します。

    ```bash
    mysql -u ユーザー名 -p

2. データベースを作成します。以下のコマンドを実行します。
    ```bash
    CREATE DATABASE internet_tv;

3. データベースを選択します。作成したデータベースに対して操作を行うために、以下のコマンドを実行します。
    ```bash
    USE internet_tv;

これで、データベースがアクティブになり、テーブルの作成やデータの操作が可能です。

# 2.ステップ1で設計したテーブルを構築します

## テーブルの作成手順

MySQLデータベースにstep1のテーブルを作成する手順を説明します。以下の7つのテーブルを作成します。

1. チャンネルテーブル (channels)
2. 番組テーブル (programs)
3. シリーズテーブル (series)
4. エピソードテーブル (episodes)
5. ジャンルテーブル (genres)
6. 視聴履歴テーブル (viewing_history)
7. ユーザーテーブル (users)


1. チャンネルテーブル (channels)
    ```bash
    CREATE TABLE channels (
    channel_id INT PRIMARY KEY AUTO_INCREMENT,
    channel_name VARCHAR(255),
    genre_id INT,
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
    );

2. 番組テーブル (programs)
    ```bash
    CREATE TABLE programs (
    program_id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    description TEXT,
    series_id INT,
    genre_id INT,
    FOREIGN KEY (series_id) REFERENCES series(series_id),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
    );

3. シリーズテーブル (series)
    ```bash
    CREATE TABLE series (
    series_id INT PRIMARY KEY AUTO_INCREMENT,
    season_number INT,
    series_name VARCHAR(255)
    );

4. エピソードテーブル (episodes)
    ```bash
    CREATE TABLE episodes (
    episode_id INT PRIMARY KEY AUTO_INCREMENT,
    series_id INT,
    season_number INT,
    episode_number INT,
    title VARCHAR(255),
    description TEXT,
    duration TIME,
    release_date DATE,
    viewership INT,
    FOREIGN KEY (series_id) REFERENCES series(series_id)
    );

5. ジャンルテーブル (genres)
    ```bash
    CREATE TABLE genres (
    genre_id INT PRIMARY KEY AUTO_INCREMENT,
    genre_name VARCHAR(255)
    );

6. 視聴履歴テーブル (viewing_history)
    ```bash
    CREATE TABLE viewing_history (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    episode_id INT,
    viewing_datetime DATETIME,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id)
    );
7. ユーザーテーブル (users)
    ```bash
    CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    password_hash VARCHAR(255),
    -- その他のテーブルにリンクするカラムを追加
    );

# サンプルデータの挿入手順



















