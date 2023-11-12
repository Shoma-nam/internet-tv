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
このドキュメントでは、インターネットTVサービスのテーブルを構築する手順を説明します。  

以下のステップでデータベースを構築していきます。


1. データベースの構築  

    1-1.データベース管理システム(DBMS)の選定 ← 本ドキュメントで解説します。

    1-2.データベースの作成 ← 本ドキュメントで解説します。


2. ステップ1で設計したテーブルの構築  

    2-1.テーブルの設計と作成 ← テーブル設計はstep1で作成済み。定義したテーブルを基に実際にテーブルを作成する手順を解説します。

    2-2.インデックスの作成 ← step1で作成済み。基本テーブル設計時に考えます。

    2-3.関連付けと外部キー制約 ← step1で作成済み。基本テーブル設計時に考えます。


3. サンプルデータの挿入  

    3-1.データの初期化 ← 本ドキュメントで解説します。  
    3-2.データの削除← 本ドキュメントで解説します。


# 1.データベースの構築

## 1-1.データベース管理システム(DBMS)の選定
 - データベースを構築するために、どのデータベース管理システム（例：MySQL、PostgreSQL、SQLite、Microsoft SQL Serverなど）を使用するか選定します。
 - 選択したDBMSに応じて、そのDBMSに対応したコマンドやツールを使用します。
 - 今回はMySQLを使用します。

## 1-2.データベースの作成
 - 選定したDBMS(今回はMySQL)を使用して、新しいデータベースを作成します。
 - これにより、データベースファイルが作成され、データベースを格納するための物理的な場所が確保されます。

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
    ```

    これで、データベースがアクティブになり、テーブルの作成やデータの操作が可能です。

# 2.ステップ1で設計したテーブルの構築

## 2-1.テーブルの作成手順

MySQLデータベースにstep1のテーブルを作成する手順を説明します。以下の7つのテーブルを作成します。

    1.ジャンルテーブル (genres)  
    2.チャンネルテーブル (channels)  
    3.シリーズテーブル (series)  
    4.番組テーブル (programs)  
    5.エピソードテーブル (episodes)  
    6.ユーザーテーブル (users)  
    7.視聴履歴テーブル (viewing_history)  
    8.放送スケジュールテーブル(broadcast_schedule)  

1. ジャンルテーブル (genres)
    ```bash
    CREATE TABLE genres (
    genre_id INT PRIMARY KEY AUTO_INCREMENT,
    genre_name VARCHAR(255)
    );

2. チャンネルテーブル (channels)
    ```bash
    CREATE TABLE channels (
    channel_id INT PRIMARY KEY AUTO_INCREMENT,
    channel_name VARCHAR(255),
    genre_id INT,
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
    );

3. シリーズテーブル (series)
    ```bash
    CREATE TABLE series (
    series_id INT PRIMARY KEY AUTO_INCREMENT,
    season_number INT,
    series_name VARCHAR(255)
    );

4. 番組テーブル (programs)
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

5. エピソードテーブル (episodes)
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

6. ユーザーテーブル (users)
    ```bash
    CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    password_hash VARCHAR(255)
    );

7. 視聴履歴テーブル (viewing_history)
    ```bash
    CREATE TABLE viewing_history (
    history_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    episode_id INT,
    viewing_datetime DATETIME,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id)
    );

8. 放送スケジュールテーブル(broadcast_schedule)
    ```bash
    CREATE TABLE broadcast_schedule (
    schedule_id INT PRIMARY KEY AUTO_INCREMENT,
    channel_id INT,
    episode_id INT,
    start_time DATETIME,
    end_time DATETIME,
    FOREIGN KEY (channel_id) REFERENCES channels(channel_id),
    FOREIGN KEY (episode_id) REFERENCES episodes(episode_id)
    );


下記コマンドを打つと、internet_tvのテーブルが表示されます。
テーブルが正しく作成されているか確認してください。  
    ```bash
        SHOW TABLES;
    ```  

    ```bash
    mysql> SHOW TABLES;
    +-----------------------+
    | Tables_in_internet_tv |
    +-----------------------+
    | broadcast_schedule    |
    | channels              |
    | episodes              |
    | genres                |
    | programs              |
    | series                |
    | users                 |
    | viewing_history       |
    +-----------------------+
    8 rows in set (0.15 sec)
    ```

# 3.サンプルデータの挿入

## 3-1.データの初期化
テーブルに初期データを挿入します。これには、手動でデータを挿入する作業や、サンプルデータを使用することが含まれます。

1. ジャンルテーブル (genres)
    ```bash
    INSERT INTO genres (genre_name) VALUES
    ('ドラマ'),
    ('アニメ'),
    ('スポーツ'),
    ('映画'),
    ('コメディ'),
    ('ミステリー'),
    ('ホラー'),
    ('SF'),
    ('ドキュメンタリー'),
    ('ファンタジー'),
    ('アクション'),
    ('ロマンス');

2. チャンネルテーブル (channels)
    ```bash
    INSERT INTO channels (channel_name, genre_id) VALUES
    ('ドラマチャンネル', 1),
    ('アニメチャンネル', 2),
    ('スポーツチャンネル', 3),
    ('映画チャンネル', 4),
    ('ミュージカルチャンネル', 5),
    ('コメディチャンネル', 6);

3. シリーズテーブル (series)
    ```bash
    INSERT INTO series (season_number, series_name) VALUES
    (3, '夜明けの街の物語'),
    (2, '宇宙の戦士伝説'),
    (1, '秘密の花園'),
    (4, 'サイバー探偵アドベンチャー'),
    (5, '海を越えて'),
    (2, '宇宙ステーションX'),
    (3, '怪盗の謎'),
    (1, '未来への旅'),
    (6, '歴史の中の影'),
    (4, '幽霊屋敷の秘密');

4. 番組テーブル (programs)
    ```bash
    INSERT INTO programs (title, description, series_id, genre_id) VALUES
    ('夜明けの街', '大都会を背景にした愛と葛藤の物語。...', 1, 1),
    ('宇宙の戦士', '未知の星を探検する若き宇宙飛行士の物語。...', 2, 2),
    ('最後のゴール', 'サッカー界の伝説的な試合と選手たちの情熱を描くドキュメンタリー。', NULL, 3),
    ('笑いの世界', '日常の中に潜むユーモアと笑いを描いたコメディ番組。', NULL, 6),
    ('秘密の花園', '古い屋敷の秘密の花園を舞台に繰り広げられるファンタジー物語。', 3, 1),
    ('サイバー探偵', '未来都市を舞台にしたサイバー犯罪を追う探偵の物語。', 4, 8),
    ('海の向こうの冒険', '海を越えた冒険を通じて成長する若者たちのドラマ。', 5, 1),
    ('恋するキッチン', '料理を通じて恋愛が芽生えるレストランの物語。', NULL, 6),
    ('宇宙ステーションX', '宇宙ステーションでの生活と冒険を描いたSFドラマ。', 6, 2),
    ('歴史の影で', '歴史的な出来事を背景にしたドラマシリーズ。', NULL, 1),
    ('怪盗の謎', '世界を股にかける怪盗と彼を追う探偵の緊張感ある対決を描いたミステリー。', 7, 8),
    ('笑いの学校', '小学校を舞台にした子供たちの日常と冒険を描いたコメディ番組。', NULL, 6),
    ('未来への旅', '時間旅行をテーマにした冒険と家族の絆を描いたドラマ。', 8, 1),
    ('深海の秘密', '深海を探検する海洋学者の冒険と発見を描いたドキュメンタリーシリーズ。', NULL, 3),
    ('幽霊屋敷の謎', '幽霊屋敷の秘密を探る若者たちのスリリングな物語。', NULL, 9);

5. エピソードテーブル (episodes)
    ```bash
    INSERT INTO episodes (series_id, season_number, episode_number, title, description, duration, release_date, viewership) VALUES
    (1, 1, 1, '夜明けの出会い', '新しい都市での生活が始まる主人公の物語。', '00:45:00', '2023-01-01', 500000),
    (1, 1, 2, '過去の影', '主人公が過去の秘密に直面する。', '00:50:00', '2023-01-08', 450000),
    (2, 1, 1, '星への旅立ち', '若き宇宙飛行士が最初のミッションに挑む。', '00:40:00', '2023-02-01', 300000),
    (3, 1, 1, '秘密の花園の発見', '古い屋敷の隠された花園を見つける物語。', '00:30:00', '2023-03-15', 200000),
    (4, 1, 1, 'サイバー犯罪の謎', '高度なサイバー技術を使った犯罪に立ち向かう。', '00:55:00', '2023-04-05', 400000),
    (5, 1, 1, '新しい地平へ', '海を越えた冒険の始まり。', '00:45:00', '2023-05-10', 350000),
    (6, 1, 1, '宇宙ステーションの日常', '宇宙ステーションでの生活を紹介。', '00:40:00', '2023-06-01', 250000),
    (7, 1, 1, '怪盗の挑戦', '巧妙な怪盗による盗みの計画。', '00:50:00', '2023-07-20', 550000),
    (8, 1, 1, '時間を超えて', '時間旅行の不思議と冒険。', '00:55:00', '2023-08-15', 500000),
    (9, 1, 1, '幽霊屋敷の真実', '幽霊屋敷の秘密に迫る。', '01:00:00', '2023-09-10', 600000);

6. ユーザーテーブル (users)
    ```bash
    INSERT INTO users (username, email, password_hash) VALUES
    ('user1', 'user1@example.com', 'hash1'),
    ('user2', 'user2@example.com', 'hash2'),
    ('user3', 'user3@example.com', 'hash3'),
    ('user4', 'user4@example.com', 'hash4'),
    ('user5', 'user5@example.com', 'hash5');

7. 視聴履歴テーブル (viewing_history)
    ```bash
    INSERT INTO viewing_history (user_id, episode_id, viewing_datetime) VALUES
    (1, 11, '2023-01-01 20:00:00'),
    (1, 12, '2023-01-08 20:30:00'),
    (2, 13, '2023-01-02 18:00:00'),
    (2, 14, '2023-01-09 19:45:00'),
    (3, 15, '2023-01-05 21:00:00'),
    (4, 16, '2023-01-10 20:15:00'),
    (5, 17, '2023-01-12 18:30:00'),
    (1, 18, '2023-01-15 22:00:00'),
    (2, 19, '2023-01-18 19:00:00'),
    (3, 20, '2023-01-20 21:30:00');

8. 放送スケジュールテーブル(broadcast_schedule)
    ```bash
    INSERT INTO broadcast_schedule (channel_id, episode_id, start_time, end_time) VALUES
    (1, 11, '2023-11-11 19:00:00', '2023-11-11 19:45:00'),
    (1, 12, '2023-11-11 19:45:00', '2023-11-11 20:30:00'),
    (2, 13, '2023-11-11 20:00:00', '2023-11-11 20:40:00'),
    (3, 14, '2023-11-11 18:30:00', '2023-11-11 19:00:00'),
    (4, 15, '2023-11-11 21:00:00', '2023-11-11 21:55:00'),
    (5, 16, '2023-11-11 17:00:00', '2023-11-11 17:45:00'),
    (1, 17, '2023-11-11 21:00:00', '2023-11-11 21:40:00'),
    (2, 18, '2023-11-11 22:00:00', '2023-11-11 22:50:00'),
    (3, 19, '2023-11-11 23:00:00', '2023-11-11 23:55:00'),
    (4, 20, '2023-11-11 20:00:00', '2023-11-11 21:00:00'),
    (1, 11, '2023-11-12 19:00:00', '2023-11-12 19:45:00'),
    (2, 13, '2023-11-12 20:00:00', '2023-11-12 20:40:00'),
    (3, 14, '2023-11-12 18:30:00', '2023-11-12 19:00:00'),
    (4, 15, '2023-11-12 21:00:00', '2023-11-12 21:55:00'),
    (5, 16, '2023-11-12 17:00:00', '2023-11-12 17:45:00'),
    (1, 17, '2023-11-12 21:00:00', '2023-11-12 21:40:00'),
    (2, 18, '2023-11-12 22:00:00', '2023-11-12 22:50:00'),
    (3, 19, '2023-11-12 23:00:00', '2023-11-12 23:55:00'),
    (4, 20, '2023-11-12 20:00:00', '2023-11-12 21:00:00'),
    (1, 11, '2023-11-12 19:00:00', '2023-11-12 19:45:00'),
    (1, 12, '2023-11-13 20:00:00', '2023-11-13 20:45:00'),
    (1, 13, '2023-11-14 18:30:00', '2023-11-14 19:15:00'),
    (1, 14, '2023-11-15 21:00:00', '2023-11-15 21:45:00'),
    (1, 15, '2023-11-16 20:00:00', '2023-11-16 20:45:00'),
    (1, 16, '2023-11-17 19:30:00', '2023-11-17 20:15:00'),
    (1, 17, '2023-11-18 22:00:00', '2023-11-18 22:45:00'),
    (1, 18, '2023-11-19 20:00:00', '2023-11-19 20:45:00');


## 3-2.データの削除
データの削除に関するドキュメントの作成について、以下に各テーブルごとの削除手順を示します。  
削除する際には、特に外部キー制約が関与するテーブルの順序に注意する必要があります。  

    ```bash
    DELETE FROM genres;
    DELETE FROM channels;
    DELETE FROM series;
    DELETE FROM programs;
    DELETE FROM episodes;
    DELETE FROM users;
    DELETE FROM viewing_history;
    DELETE FROM broadcast_schedule;
    ```  

注意点
 - この手順はテーブル内の全てのデータを削除します。
 - 特定の行のみを削除する場合は、WHERE 句を使用して条件を指定してください。
 - 外部キー制約がある場合、関連するテーブルのデータを先に削除すると、制約違反が発生する可能性があります。
 - 例えば、episodes テーブルのデータは viewing_history テーブルと関連しているため、viewing_history のデータを先に削除する必要があります。
 - データを削除する前に、必要に応じてバックアップを取ることをお勧めします。

