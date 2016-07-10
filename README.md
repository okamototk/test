ALMifdsfnium Ansible
================

はじめに
-------

ALMinium Ansibleはチケット管理とバージョン管理を提供する統合管理ツールです。
チケットシステムにRedmineを、バージョン管理にGit/Subversionを利用することができます。

ALMinium AnsibleはALMiniumを構成管理ツールのAnsibleを利用して書き直したものです。
ユーザの方は、Ansibleの知識がなくてもご利用いただけます。
Ansibleの知識があれば、従来のALMiniumに比べ簡単にカスタマイズすることができます。

業務で自由にお使い頂いて構いませんが、自己責任でご利用お願いします。

特徴
----

* Redmineによるチケット管理を行えることができます
* Git/Subversionによるバージョン管理を行えます
* RedmineとGit/Subversionは統合されているので、Redmineからリポジトリを作成したりリポジトリのアクセス権を設定することができます。
* データベースサーバを分離したり、クラスタ構成のデータベースを別途作成して構築するマルチホストに対応しています(New)
* Ansibleにより簡単にカスタマイズすることができます
* 現在CentOS7のみ対応しています(Ubuntuなど他のOSはサポートしていません)

使い化
-----

# git clone https://github.com/alminium/alminium-ansible
# cd alminium-ansible
# bash smelt.sh

プロキシの設定が必要な場合は、group_vars/allファイルのproxy_host/proxy_portにプロキシ情報を記述してください。

正常にインストールが完了すると、https://<ホスト名>/　にアクセスすると、ALMiniumのトップページが表示されます。


### 例

    proxy_host: proxy.mycompany.com
    proxy_port: 8080

マルチホストへのインストール
----------------------------

Redmineサーバとは別にデータベースサーバを独立させるためには、hostsファイルを編集してください。
例えば、次のように記述します。

#### hosts

    [redmine]
    192.168.1.1

    [database]
    192.168.1.2

また、SSHでパスワードなしでログインできるようにしておきます。

### SSH鍵の作成

```
# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
[以下省略]
```

### SSH鍵のコピー


```
# ssh-copy-id -i ~/.ssh/id_rsa  root@192.168.1.1
# ssh-copy-id -i ~/.ssh/id_rsa  root@192.168.1.2
```
 
### Ansibleのインストール、実行

    # yum install epel-release
    # ansible-playbook site.yml -i hosts



RDSなど既にあるデータベースを利用
--------------------------------

予め、group_vars/allに記載してあるデータベースとRedmineホストからアクセス可能なユーザを作成しておきます。
デフォルトでは、下記の通りです。

|--------------|------------|
|データベース名|alminium    |
|--------------|------------|
|ユーザ名      |alminium    |
|--------------|------------|
|パスワード    |dbpass      |
|--------------|------------|
