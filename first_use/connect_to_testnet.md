## テストネットに接続してみる

Gethのインストールが完了したら早速Gethを起動します。Frontier（PoC-9）から、Ethereumの本番ネットワーク（ライブ・ネット）の稼働が開始されましたが、個人的な試験などを行うにはテストネットを立ち上げると便利です。

本節では、Ethereumの様々な動作を見ていくためテスト・ネットに接続していきます。

### Gethの起動し、テストネットへ接続する
事前に任意の場所にGethのデータ用のディレクトリを準備します。
ここでは、ログイン・ユーザー（今回の例ではtest_u）のhomeディレクトリ直下に作成します。

```
$ mkdir /home/test_u/eth_data
```
データ用ディレクトリを作成したら、以下のコマンドでGethを起動します。
```
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/geth_01.log" --olympic console 2>> /home/test_u/geth_e01.log
```
ここで各オプションの意味は以下のとおりです。
* `--networkid "10"` ：`--networkid`オプションで任意の正の整数のIDを指定することで、個人用テストネットを立ち上げることが可能です（ここでは10を指定）。Frontier（PoC-9）から、Ethereumの本番ネットワークの稼働が開始されましたが、個人的な試験などを行うにはテストネットを立ち上げると非常に便利です。自分だけが参加者のネットワークなので、採掘なども非常に短時間で可能です。
* `--datadir "/home/test_u/eth_data"`：データ用ディレクトリとして、事前に作成しておいたディレクトリのパスを指定しています。データ用ディレクトリにブロックチェーン情報やノード情報など各種データが保存されます。
* `--logfile "/home/test_u/geth_01.log"`：ログファイルの指定。
* `--olympic`：テスト・ネットでの採掘を容易にするため、Olympic（PoC-8）のプロトコルを使用することを指定。Frontier（PoC-9）に移行した際に、初期の採掘の難易度（Difficulty）が非常に高く設定されたため、テストネットでも採掘が非常に困難になりました。主な使用はOlympicとFrontierで大きく異なることがないので、個人的な試験ではOlympicプロトコルを使用することで問題はありません。（ライブ・ネットに接続する際には、このフラグは外します。）
* `console`：Gethには採掘やトランザクションの生成などを対話的に進めることができるコンソールが用意されています。`console`サブ・コマンドを指定することで、Gethの起動時に同時にコンソール立ち上げることが可能です。なお、`console`サブ・コマンドを付加せず起動したGethプロセスに対して後からコンソールを起動する事も可能です（下記TIP参照）。

上記コマンドを実行すると、下記の実行結果のように、幾つかの情報の表示の後に「>」のプロンプトが表示され、コンソールが起動されます。次の節ではこのコンソール上で、Ethereumのアカウント（EOA）を生成します。

```
instance: Geth/v1.0.1/linux/go1.4.2
 datadir: /home/test_u/eth_testnet_10
coinbase: null
at block: 0 (1970-01-01 09:00:00)
 modules: admin:1.0 db:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 shh:1.0 txpool:1.0 web3:1.0
>

```


###### ■■ TIP ■■
今後Gethを使用していくなかで、採掘等のためにGethをバックグラウンドで常時起動しておき、必要に応じてそのGethプロセスに対してコンソールを用いて対話的に操作をしたいといった場合が発生します。その際は、下記のように`attach`サブ・コマンドを利用することで、既に起動されたGethプロセスのコンソールを起動することが可能です。

```
$ # gethプロセスをconsoleサブ・コマンドを付加せず、バックグラウンドで起動します。
$ # この場合、起動時にはコンソールは立ち上がりません。
$ geth --networkid "10" --datadir "/home/test_u/eth_data" --logfile "/home/test_u/eth_data/geth_01.log" --olympic 2>> /home/test_u/eth_data/geth_e01.log &
$
$ # attachサブ・コマンドを用いて先に立ち上げたプロセスのコンソールを立ち上げます。
$ # ここで、ipc:以降に先に立ち上げたgethプロセスのデータ用ディレクトリ以下のgeth.ipcファイル（実際はソケット）のパスを指定します。
$ geth --datadir "/home/test_u/eth_data" attach ipc:/home/test_u/eth_data/geth.ipc

```