# Docker環境 TravelApp1.5

TravelAppをDocker Composeで構築しました。Visual Studio Code（VSCode）の拡張機能「Dev Container」で開発してください。以下の手順に従ってローカル環境でセットアップできます。なお、SSL認証はv2.0で実装予定です。

## 前提条件

- Docker Desktop
- Visual Studio Code（Dev Container拡張）

## 更新情報

>**2024-09-25**
- Next.jsプロジェクトからReactに変更（必要に応じてNext.jsに戻せます）
- データベース情報を環境変数化（プライベートで配布）
- MySQLで初回にデータベースを作成する必要がなくなりました
- 処理速度の向上
- ホットリロードを有効化
- 簡易サーバーの稼働を手動化（自動化手順は`docker-compose.yml`に記載）
- バックエンドのポートを3001から8000に変更

## ダウンロード

1. リポジトリをクローン

    ```sh
    git clone https://github.com/3YK-Ginga/TravelApp-Docker.git
    cd TravelApp-Docker
    ```

## Visual Studio Codeで開く

1. 拡張機能から「Dev Container」をインストール
1. リポジトリ内のfrontendまたはbackendフォルダにある「`.devcontainer/devcontainer.json`」を実行
1. しばらくすると右下に「コンテナーで再度開く」という通知が出るので押してください
1. ~~「traveldb」の追加~~\
   ~~<http://localhost/phpmyadmin>~~
1. ~~Docker Desktopでbackendコンテナを停止し、再起動~~

>### 【更新】 2024-09-25
>
> 手順4と5は不要です。初期化用SQLを`/initdb.d`に配置することで、traveldbが自動追加されるようになりました。初期化用SQLはプライベートで配布されます。

## WebArenaでの操作

1. コンテナの起動

    ```sh
    docker compose up -d
    ```

1. ~~「traveldb」の追加~~

    ~~<http://ドメイン/phpmyadmin>~~
1. ~~Dockerでbackendコンテナを停止し、再起動~~

## 使用方法

### アクセス

- **nginx**: [http://localhost:8000](http://localhost:8000)
- **frontend**: [http://localhost](http://localhost)
- **backend**: [http://localhost/api](http://localhost/api)
- **phpMyAdmin**: [http://localhost/phpmyadmin](http://localhost/phpmyadmin)

### 停止

```sh
docker compose down
```

※ コンテナの停止はDocker Desktopからでも可能です（VSCode左下の「開発コンテナ」 → 「リモート接続を終了」）。

## コンテナ生成後について

コンテナはDocker Desktopの「▷」ボタンで起動可能です。\
~~backendの「`.devcontainer/devcontainer.json`」も使用できます。~~

>### 【更新】 2024-09-25
>
> frontendまたはbackendのどちらからでも環境構築が始められるようになりました。

## ライブラリ、フレームワークの変更について

### フロントエンド・バックエンド共通

React、Django以外を使う場合(node、Pythonに限る)、`source`ディレクトリ内を削除し、`docker-compose.yml`と`Dockerfile`をプロジェクトの構成に合わせて正しく修正してください。\
プロジェクト名は`workspace`ディレクトリ内で、現在のディレクトリを示す`.`とするのが望ましいです\
なお、`workspace`ディレクトリのサブディレクトリ内にプロジェクトを配置した場合、`docker-compose.yml`と`Dockerfile`を修正する必要があります。

**【修正前】 docker-compose.yml**
```
volumes:
      - ./~end/source:/workspace
```

**【修正後】 docker-compose.yml**
```
volumes:
      - ./~end/source/subdirectory:/workspace
```

**【修正前】 Dockerfile**
```
COPY ./source /workspace
```

**【修正後】 Dockerfile**
```
COPY ./source/subdirectory /workspace
```

### フロントエンド
- モジュールインストールコマンド、簡易サーバー立ち上げコマンドを把握し、適切に`Dockerfile`へ記述する必要があります(使う物に合わせて各自調べてください)
- 以下はプロジェクト作成コマンドの一覧

#### Create React Appを使用する場合
1. **JavaScriptでプロジェクトを作成**:
    ```sh
    npx create-react-app .
    ```

2. **TypeScriptでプロジェクトを作成**:
    ```sh
    npx create-react-app . --template typescript
    ```

#### Viteを使用する場合
1. **JavaScriptでプロジェクトを作成**:
    ```sh
    npm create vite@latest . -- --template react
    ```

2. **TypeScriptでプロジェクトを作成**:
    ```sh
    npm create vite@latest . -- --template react-ts
    ```

#### Next.jsを使用する場合
1. **JavaScriptでプロジェクトを作成**:
    ```sh
    npx create-next-app .
    ```

2. **TypeScriptでプロジェクトを作成**:
    ```sh
    npx create-next-app . --typescript
    ```

### バックエンド
 - ワークスペースの中に`requirements.lock`と`requirements.txt`があるので**必ず**活用する
#### pythonパッケージのインストール手順
1. `requirements.txt`に、インストールしたいライブラリ名を書く
2. `pip install --no-cache-dir -r requirements.txt`を`workspace`ディレクトリで実行
3. `pip freeze > requirements.lock`を実行し、インストール済みパッケージを記録

**<span style="color: red; ">！！！！これをやらないとチームで共有したときにスクリプトが実行できなくなります！！！！</span>**

## トラブルシューティング

### よくある問題

1. **フロントエンドにアクセスできない**
   - プロジェクトの初回インストール時はバックグラウンドで必須パッケージがインストールされます。Docker Desktopで「frontend」を確認し、以下のログが表示されている場合は、インストールが進行中ですので、しばらく待ってください。

    ```
    [1/4] Resolving packages...
    [2/4] Fetching packages...
    info There appears to be trouble with your network connection. Retrying...
    [3/4] Linking dependencies...
    [4/4] Building fresh packages...
    ```

    以下のような表示が出ていればインストールが完了しています。

    ```
    ▲ Next.js 14.2.12
    Local:        http://localhost:3000

    ✓ Starting...
    Attention: Next.js now collects completely anonymous telemetry regarding usage.
    This information is used to shape Next.js' roadmap and prioritize features.
    You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
    https://nextjs.org/telemetry
    ```

1. **それでもフロントエンドにアクセスできない**:

   - ~~Next.jsのビルドには初回時に時間がかかるため、しばらく待ってからアクセスしてください。~~

1. **バックエンドにアクセスできない**:

   - ~~バックエンドはMySQLデータベースへの接続が前提です。データベースが存在しない、またはMySQLが起動していない場合、`manage.py runserver`に失敗し、コンテナが終了することがあります。~~
   - ~~MySQLが起動しても、データベースにアクセスできるまでに少し時間がかかります。backendコンテナを再起動するか、MySQL→phpMyAdmin→frontend→backend→nginxの順に起動すると正常にアクセスできるようになります。~~
   - ~~Djangoの`settings.py`にリトライ機能を追加するとこの問題が解決されます（やる気があれば対応します）。~~

   >### 【更新】 2024-09-25
   >
   > 問題は解消されました。
