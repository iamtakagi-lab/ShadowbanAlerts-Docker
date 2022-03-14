# ShadowbanAlerts

See a [example](./example/) directory.

`docker-compose.yml`
```yml
version: '3.9'

services:
  app:
    image: ghcr.io/iamtakagi/shadowban-alerts:latest
    container_name: shadowban-alerts
    volumes:
      - type: bind
        source: './crontab'
        target: '/app/crontab'
      - type: bind
        source: './ShadowbanAlerts.json'
        target: '/app/ShadowbanAlerts.json'
    environment:
      - TZ=Asia/Tokyo
      - SCREEN_NAMES=@yousuck2020,@kskgroup2017
      - WEBHOOK_URL=https://discord.com/api/webhooks/xxx/xxx
```

Twitter アカウントへのシャドウバンが開始・解除された時に Discord に通知するツールです。

## 導入

Windows でも Linux でも動くと思いますが、動作確認は Linux でしかしていません。  
事前に Python 3.9・pip・Git がインストールされていることが前提です。

```Shell
pip3 install requests
git clone https://github.com/tsukumijima/ShadowbanAlerts.git
cd ShadowbanAlerts/
cp ShadowbanAlertsConfig.example.py ShadowbanAlertsConfig.py
```

## 設定

`ShadowbanAlertsConfig.py` は設定ファイルです。

`SCREEN_NAMES` にはシャドウバンをチェックするアカウントのスクリーンネーム (@から始まるID) を指定します。  
Python のリスト形式になっているので、例にある通り複数のアカウントを指定できます。

`WEBHOOK_URL` には Discord の Webhook URL を設定します。  
Discord の Webhook URL は別途取得してください。`https://discord.com/api/webhooks/～` のような URL になります。

## 実行

単独で実行させる場合は、`python3.9 ./ShadowbanAlerts.py` と実行します。  
この時点でいずれかのアカウントがシャドウバンされている場合は、Discord の Webhook を設定したチャンネルに通知が送信されます。

ShadowbanAlerts は常時起動機能を持ちません。継続的に実行させたい場合は、Cron やタスクスケジューラなどに ShadowbanAlerts を登録する必要があります。  
以下に Cron で1分おきに ShadowbanAlerts を実行させる例を示します（`ubuntu` は一般ユーザーです）。

```
*/1 * * * * sudo -u ubuntu /usr/bin/python3.9 /home/ubuntu/ShadowbanAlerts/ShadowbanAlerts.py
```

## License

[MIT License](License.txt)
