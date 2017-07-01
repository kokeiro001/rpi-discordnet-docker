# rpi-discordnet-docker

Raspberry Pi向けの[Discord.Net](https://github.com/RogueException/Discord.Net)ランタイムです。

[kokeiro001/rpi-discordnet - Docker Hub](https://hub.docker.com/r/kokeiro001/rpi-discordnet/)

## 使用方法

### ホストPCに配置したボットのビルドを利用する方法

予めボットをビルドしておいたデータを任意のディレクトリ(例:```/home/pi/YourBotDir/```)に配置し、次のようなコマンドで起動できます。

```
$ docker run -v /home/pi/<YourBotDir>/:/app -e "BotUserToken=<your bot token>" kokeiro001/rpi-discordnet:1.0.0rc-runtime dotnet YourBot.dll 
```

環境変数BotUserTokenには、Discordの開発者ページから取得したデータを設定してください。


ボットの更新時にイメージをビルドし直す必要がなく手軽に利用できます。


### ボットのビルドと本コンテナを結合して利用する方法

次のようなDockerfileを利用することでランタイムとアプリを結合したイメージを作成することができます。

```
FROM kokeiro001/rpi-discordnet:1.0.0rc-runtime
ENV BotUserToken "<your bot token>"
COPY YourBotDir /app

ENTRYPOINT ["dotnet", "YourBot.dll"]
```

Dockerfileと同じ階層にボットのビルドを入れたディレクトリ(例ではYourBotDir)を用意しておく必要があります。

次のようなコマンドでイメージの作成とボットの起動できます。

```
$ docker build -t kokeiro001/sample-bot .
$ docker run kokeiro001/sample-bot
```

ランタイム、アプリを結合することで、別環境への持ち運びが楽になります。

Dockerfile内でのBotUserToken環境変数の設定は任意です。次のようなコマンドで実行時に指定することもできます。

```
$ docker run -e "BotUserToken=<your bot token>" kokeiro001/sample-bot dotnet YourBot.dll 
```