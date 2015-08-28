# cloudpack-ami
## cloudpack-amiとは何か

ISOから素の状態に近いAMIをフルスクラッチで作成します。
まだアルファ版な扱いです。
https://github.com/shiguredo/packer-templates こちらを大いに参考とさせていただきました。

### 基本方針

- 可能な限り公式に提供されているISOをもとにして構成します。
- SR-IOV / irqbalance などは初期状態で組み込みます。
- bashの `SYSLOG_HISTORY` に対応します。

### 取り込んでいるもの(予定含む)

- sysctl / limits まわりの既定値の変更
- 構成時点において `yum -y update` を実施
- ログインユーザーは cloudpack(パスワード同じ)
- HVM対応インスタンスのみ選択可能
- SR-IOV対応カーネルモジュール
- bash SYSLOG_HISTORYへの対応(予定)

## cloudpack-amiの作り方

以下の手続きを行うことでAMI作成手続きを実行します。
- 公式ISOの取得とsha1確認
- kickstartによる構成
- kickstartにて作成されたベースに最低限の構成
- 作成したVMDKをS3へアップロード
- アップロードしたVMDKからAMIへ変換の指示

```
cd %OSNAME%
./iso2ami.sh -B %YOUR_S3_BUCKET_NAME%
```

### 必要な要素

- AWSアカウント
- S3バケット(ImportImageを実施するため)
- [Packer](https://www.packer.io)
- [VirtualBox](https://www.virtualbox.org)
- [Vagrant](https://www.vagrantup.com)
- [AWS CLI](https://github.com/aws/aws-cli)
- 上記が利用できる環境(当方OSXで実装)


## ToDo

- bash `-DSYSLOG_HISTORY` の実装
- ec2-toolsの実装(複数ENIへの自動対応)
- 出力ファイル名がかっこよくないので修正する
- サービス起動の調整