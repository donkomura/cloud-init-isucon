# cloud-init-isucon/isucon10q

## Overview

isucon9予選とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 18.04 LTSを用意してください。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。

## Usage

### Multipassでの利用方法

* [Multipass](https://multipass.run/)実行環境を用意します
* このリポジトリ内の `isucon9q.cfg` を手元に用意します
* 以下を実行します
  ```sh
  multipass launch --name isucon10q --cpus 2 --disk 16G --mem 4G --cloud-init isucon9q.cfg 18.04
  ```
  * cpus, disk, memoryは必要に応じて増減させてください
  * cloud-initは時間がかかるためタイムアウトとなるもののバックグラウンドで構築は行われています
* ログインします
  ```sh
  multipass shell isucon9q
  ```
* 進捗確認は以下のコマンドで確認できます
  ```sh
  sudo tail -f /var/log/cloud-init-output.log
  ```

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  cd isucari/bench
  ./benchmarker
  ```

## 本来の設定と異なるところ

* Denoは構築に失敗するため除外しています
* 本来のサーバのDisk I/O制限(IOスループット100Mbps、IOPS 200)が再現できていません
* 本来ののベンチマーカのNIC制限(100Mbps)が再現できていません

## FAQ

### 途中でエラーが発生した

スリープモードなどの影響で失敗した場合は以下で再試行ができます。

```sh
sudo /var/lib/cloud/instance/scripts/runcmd
```

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON9 予選当日マニュアル](https://github.com/isucon/isucon9-qualify/blob/master/docs/manual.md)
* [ISUCON9 予選リポジトリ](https://github.com/isucon/isucon9-qualify)

### Multipassで動作確認ができない

以下のコマンドでIPアドレスの確認ができます。

```sh
multipass info isucon9q
```

ブラウザから `https://表示されたIPアドレス/` にアクセスしてみてください。

### Multipassで作成した環境を削除したい

```sh
multipass stop isucon9q
multipass delete isucon9q
multipass purge
```

### cloud-init以外の環境で試したい

[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をご利用ください。
