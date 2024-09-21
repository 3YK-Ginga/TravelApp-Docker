# Docker環境 TravelApp

TravelAppをDocker Composeで構築しました。VScodeの拡張機能Dev containerにも対応しています。
以下の手順に従ってローカル環境でセットアップしてください。

## 前提条件

- Docker Desktop
- VisualStudioCode Dev container
## インストール

1. リポジトリをクローン

    ```sh
    git clone https://github.com/3YK-Ginga/TravelApp-Docker.git
    cd TravelApp-Docker
    ```
## Visual Studio Codeで開く

1. 拡張機能からDev containerをインストール
2. リポジトリのfrontend内の「vscode.bat」を実行
3. しばらくすると右下に「コンテナーで再度開く」コンテナーという通知が出るので押す
4. 「traveldb」の追加

    http://localhost/phpmyadmin

5. Docker Desktopでbackendコンテナを停止  → 再起動
### WebArenaでの操作

1. コンテナの起動

    ```sh
    docker compose up -d
    ```

2. 「traveldb」の追加

    http://ドメイン/phpmyadmin

3. Dockerでbackendコンテナを停止 → 再起動

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
※ コンテナの停止はDocker DesktopからでもOK（VScode左下「開発コンテナー」 → リモート接続を終了する）

## トラブルシューティング

### よくある問題

1. **フロントエンドにアクセスできない**:
    - プロジェクトの初回インストール時は、バックグラウンドで必須パッケージをインストールしています。
    - Docker Desktopで、「frontend」を押すとログが見れるので、以下の表示だけの場合はしばらく待っていてください。
	```
	[1/4] Resolving packages...
	[2/4] Fetching packages...
	info There appears to be trouble with your network connection. Retrying...
	[3/4] Linking dependencies...
	[4/4] Building fresh packages...
	```

	- 以下の様な表示が出ていればインストールが完了しています。

	```
	▲ Next.js 14.2.12
	Local:        http://localhost:3000
	 
	✓ Starting...
	Attention: Next.js now collects completely anonymous telemetry regarding usage.
	This information is used to shape Next.js' roadmap and prioritize features.
	You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
	https://nextjs.org/telemetry
	```
2. **それでもフロントエンドにアクセスできない**:
	- 初回はNext.jsのビルド時間もあるので多少時間はかかります。しばらくするとアクセスできるようになっていると思います。