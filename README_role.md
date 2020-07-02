# Ansible Role: Zabbix40-Server_install

# Trademarks
-----------
* Linuxは、Linus Torvalds氏の米国およびその他の国における登録商標または商標です。
* RedHat、RHEL、CentOSは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* Windows、PowerShellは、Microsoft Corporation の米国およびその他の国における登録商標または商標です。
* Ansibleは、Red Hat, Inc.の米国およびその他の国における登録商標または商標です。
* pythonは、Python Software Foundationの登録商標または商標です。
* Zabbixは、ラトビア共和国にあるZabbix LLCの商標です。
* Oracleは、Oracle International Corporation の米国およびその他の国における登録商標または商標です。
* NECは、日本電気株式会社の登録商標または商標です。
* その他、本ロールのコード、ファイルに記載されている会社名および製品名は、各社の登録商標または商標です。

## Description

本Ansible Roleは統合監視ソフトウェアである"Zabbix Server"のインストールを行います。
対象バージョンは以下のバージョンです。

- Zabbix 4.0

以下のリポジトリパッケージをインストールします。
- RHEL7の場合: [zabbix-release-4.0-1.el7.noarch.rpm](https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm)  
※roleを実行する前に上記のファイルをダウンロードして、"files"ディレクトリ配下のサイズ0のファイルを上書き置換してください。  

インストールされるパッケージは次の通りです。
 * zabbix-release
 * zabbix-server-mysql
 * zabbix-web
 * zabbix-web-mysql
 * zabbix-web-japanese
 
インストールされないパッケージは次の通りです。
 * zabbix-agent
 * zabbix-get
 * zabbix-java-gatewey
 * zabbix-proxy
 * zabbix-proxy-mysql
 * zabbix-proxy-pgsql
 * zabbix-proxy-sqlite3
 * zabbix-sender
 * zabbix-server-pgsql
 * zabbix-web-pgsql

## Supports

- 管理マシン(Ansibleサーバ)
  * Linux系OS（RHEL/CentOS）
  * Ansible バージョン 2.5以上 (動作確認済み：2.5、2.6)
  * Python バージョン 2.6 または2.7

- 管理対象マシン(構築対象マシン)
  * RHEL7

## Requirements

- 管理マシン(Ansibleサーバ)
  * 管理対象マシンとroot権限でSSH通信可能であること。

- 管理対象マシン(インストール対象マシン)
  - SELinuxが無効に設定されていること。
  - yumコマンドが利用できること
    - RHELレポジトリ（外部サーバ、媒体）が利用できること
    - Zabbixリポジトリにアクセスできること
    - プロキシ設定が適切に行われていること（外部サーバにアクセスできる必要があります）
  - firewalldが適切に設定されていること

## Role Variables

Role の変数値について説明します。

### Mandatory variables

実行時には、必ず指定しないといけない変数はありません。

### Optional variables

以下の変数は任意で指定します。

 - 依存パッケージのインストール設定
  * ''VAR_Zabbix40SV_localpkg_src'': 管理マシン上のパッケージ配置パス(string, default: "/tmp")
    + ダウンロードした依存パッケージは、管理マシン上のこの変数のディレクトリに配置します。
  * ''VAR_Zabbix40SV_localpkg_dst'': 管理対象マシン上のパッケージ配置パス(string, default: "/tmp")
    + 依存パッケージを管理対象マシンでインストールするために、一時的にファイルを配置するディレクトリです。
    + インストール後に、管理対象マシン上のファイルは削除されます。
    + 予め存在するディレクトリを指定します。
  * ''VAR_Zabbix40SV_phpbcmath_src'': PHPパッケージ「php-bcmath」のファイル名(string)
    + ダウンロードしたパッケージ「php-bcmath」のファイル名を指定します。
  * ''VAR_Zabbix40SV_phpmbstring_src'': PHPパッケージ「php-mbstring」のファイル名(string)
    + ダウンロードしたパッケージ「php-mbstring」のファイル名を指定します。
  * ''VAR_Zabbix40SV_trousers_src'': パッケージ「trousers」のファイル名(string)
    + trousers-0.3.11.2-4 以降のパッケージのファイル名を指定します。

## Dependencies

次のソフトウェアに依存します。

 - DB
   * MariaDB(RHEL7)
    + RHELリポジトリに含まれるパッケージが、Role実行時にインストールされます。

 - PHP5.4
   * RHEL7の場合
     + パッケージをダウンロードして、管理マシンに配置する必要があります。
     + 配置したパッケージは、Role実行時にインストールします。
     + 配置するパッケージは次の通りです。
      - php-bcmath
      - php-mbstring
     + 上記以外のphpパッケージは、RHELリポジトリから、Role実行時にインストールします。

 - Apache
   * Apache2.4(RHEL7)
    + RHELリポジトリに含まれるパッケージが、Role実行時にインストールします。

 - trousersパッケージ
  * RHEL7の場合
    + パッケージをダウンロードして、管理マシンに配置する必要があります。
    + 配置したパッケージは、Role実行時にインストールします。
    + 配置するパッケージは次の通りです。
      - trousers
      - パージョン 0.3.11.2-4 以降である必要があります。

## Usage

 - RHEL7 の場合
   1. 本Roleを用いたPlaybookを作成します。
   2. 以下の関連パッケージをダウンロードします。
        - php-bcmath
        - php-mbstring
        - trousers (0.3.11.2-4 以降)
   3. 関連パッケージを、管理対象サーバに配置します。
   4. 任意変数を必要に応じて指定します。
   5. Playbookを実行します。

## Example Playbook

インストールとすべての設定を行う場合は、提供した以下のRoleを"roles"ディレクトリに配置したうえで、
以下のようなPlaybookを作成してください。

- フォルダ構成
~~~
  - group_vars/
    ・ server1
    ・ server2
  - host_vars/
    ・ host1
    ・ host2
  - roles/
    ・ Zabbix40-Server_install/
    ・ Zabbix40-Server_setup/
    ・ Zabbix40-Server_ossetup/
  - Zabbix40-Server_install.yml
  - Zabbix40-Server_setup.yml
  - Zabbix40-Server_ossetup.yml
  - conf.yml
  - hosts
  - site.yml
~~~

- マスターPlaybook サンプル「Zabbix40-Server_install.yml」
~~~
# Zabbix40-Server_install.yml
 - name: install Zabbix40_Server
   hosts: all
   gather_facts: yes
   become: yes
   tags:
    - install
   roles:
     - Zabbix40-Server_install
~~~

## Running Playbook

- extra-varsを利用する場合の実行例
> ansible-playbook site.yml -k -i hosts --extra-vars="@conf.yml"

- group\_varsを利用する場合の実行例   
  group\_varsで指定したグループ名がwebserver1の場合
> ansible-playbook site.yml -k -i hosts -l webserver1

- host\_varsを利用する場合の実行例  
  host\_varsで指定したホスト名がserver1の場合
> ansible-playbook site.yml -k -i hosts -l server1

- 本roleのみを実行する場合は --tags "install"を付け加える
> ansible-playbook site.yml -k -i hosts --extra-vars="@conf.yml" --tags "install"

# Copyright
Copyright (c) 2018 NEC Corporation

# Author Information
NEC Corporation