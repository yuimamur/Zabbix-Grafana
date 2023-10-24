# Zabbix-Grafana

# Grafana インストール

## レポジトリ追加
```
[admin@bb-amazonlinux2 ~]$ cat /etc/yum.repos.d/grafana.repo
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```

## Grafana インストール
```
[root@bb-amazonlinux2 admin]# yum install -y grafana
```

## Grafana サービス起動
```
[root@bb-amazonlinux2 admin]# systemctl daemon-reload
[root@bb-amazonlinux2 admin]# systemctl start grafana-server
[root@bb-amazonlinux2 admin]# systemctl status grafana-server
● grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; disabled; vendor preset: disabled)
   Active: active (running) since 火 2023-10-24 16:23:30 JST; 7s ago
     Docs: http://docs.grafana.org
 Main PID: 58897 (grafana)
   CGroup: /system.slice/grafana-server.service
           └─58897 /usr/share/grafana/bin/grafana server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana...
[root@bb-amazonlinux2 admin]# systemctl enable grafana-server
Created symlink from /etc/systemd/system/multi-user.target.wants/grafana-server.service to /usr/lib/systemd/system/grafana-server.service.
```

## Grafanaバージョンの確認
```
[root@bb-amazonlinux2 admin]# grafana-cli -v
grafana-cli -v
grafana version 10.1.5
```

## Zabbixプラグインのインストール
```
[root@bb-amazonlinux2 admin]# grafana-cli plugins install alexanderzobnin-zabbix-app
✔ Downloaded and extracted alexanderzobnin-zabbix-app v4.4.3 zip successfully to /var/lib/grafana/plugins/alexanderzobnin-zabbix-app
Please restart Grafana after installing or removing plugins. Refer to Grafana documentation for instructions if necessary.

[root@bb-amazonlinux2 admin]# systemctl restart grafana-server
```

## Grafanaへアクセス

http://localhost:3000 へアクセスする。
<br>
<img width="1207" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/b0d198b0-b2d5-4801-a761-621881d97480">

ログインすると、下記のダッシュボードへアクセス可能になる。
<br>
<img width="1283" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/3cce4a27-ef50-4e5e-af12-007685f8d9e7">

<br>
<br>

# Grafana設定
Administration > Plugins から Zabbixを選択する。

<img width="970" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/751e8901-b27c-4112-afb1-522315cc3d59">

インストールと同時にEnableに設定する。

<img width="1261" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/4ead4ed7-365a-4160-ad08-09b66526b96a">

Connections > Add New Connection へアクセスし、MySQLをクリックする。
<br>
Zabbixインストール時に設定したMySQLの情報を設定する。

<img width="1261" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/eefc3782-7867-4f97-9c5f-ffd077eb25cc">

Data Sources から Addを選択し、Zabbixを追加する。
<br>
Zabbixの項目にて、必要な情報を入力する。

```
HTTP
URL : http://192.168.5.152/zabbix/api_jsonrpc.php
Access : Server

Zabbix API details
Auth type : User and Password
Username : ユーザ名
Password : パスワード
```

<img width="1261" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/ecf8f61e-d9ad-4849-84e2-020b89742a34">

<br>

# ダッシュボードの作成

Build Dashboard からダッシュボードを作成する。

<img width="850" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/421afdc9-1280-4e8e-951c-7f28ba46ff9b">

```
Query type : Text
Group : Zabbix servers
Host : /.*/
Application : Zabbix raw items
Item : 追加したアイテム(test)
```

Zabbix側に追加されたデータがGrafana側で確認できる。

<img width="1100" alt="image" src="https://github.com/yuimamur/Zabbix-Grafana/assets/59761194/d360bfc9-d684-40cb-9ff6-c8d726be6e24">

<br>


