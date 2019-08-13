# これは何

CentOS/7上にDockerとConcourse CIを構築する。(fly-cliのインストールも行う)

`vagrant up` 後に `http://localhost:38080` に `test:test` でログイン可能。

# 環境構築

* VirtualBox 6.0
* Vagrant 2.2.4(1.9以降なら多分動く)

## vagrant plugin

|プラグイン名|用途|備考|
|-|-|-|
|vagrant-reload|カーネルのバージョンアップ後の再起動||
|vagrant-proxyconf|VMへのプロキシ設定|プロキシの設定を行わないのであれば不要|

### plugin installコマンド例

```sh
$ vagrant plugin install vagrant-reload
```

## proxy環境下での利用

以下の環境変数を設定した状態で `vagrant up` してください。

設定された環境変数はVM内のdockerにも使用されます。

* http_proxy
* no_proxy

## windowsで動かすときの注意

### `Encoding::InvalidByteSequenceError` とかエラーが出る。

コマンドプロンプトかPSの文字コードをUTF-8変更した上で実行する。

* `chcp 65001`

### `Encoding::CompatibilityError` とかエラーが出る。

Vagrantfileを置いているパスに日本語が含まれているはずなので全て英語にする。


# 補足

## 基本的にやってること

* 使っているOSはCentOS7
* プロビジョナはシェルを書くのが辛いのでansible_local

## ポートフォワード

8080 (guest:concourse) => 8080 (guest) => 38080 (host) 

## カーネル更新後の再起動について

vagrant(2.2.4)ではansible_local（ansible 2.8）で `reboot` モジュールを使用するとエラーになる。（接続が切れるのでprovision失敗扱いその後の処理もできない）

# 参考
## ansible
* https://docs.ansible.com/ansible/latest/modules/modules_by_category.html
## Concourse CI
* https://concourse-ci.org/