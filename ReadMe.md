# はじめに

# 事前準備

```
[実行コマンド]
docker network create --subnet=172.10.10.0/24 --gateway=172.10.10.1 devops_net

[確認コマンド]
docker network list
```

```
http://192.168.56.112
http://gitlab.example.com
```

# rootのパスワード
```
root
docker exec -it <コンテナ名> cat /etc/gitlab/initial_root_password
```

ログにパスワードが見つからない場合、次のコマンドで手動でパスワードをリセットできます。
```
docker exec -it <コンテナ名> /bin/bash
gitlab-rails console
user = User.where(id: 1).first
user.password = '新しいパスワード'
user.password_confirmation = '新しいパスワード'
user.save!
exit
```

# トラブルシュート

## メモリ不足

```
21670 ==> /var/log/gitlab/sidekiq/current <==
21671 {"severity":"ERROR","time":"2024-10-14T09:13:13.371Z","message":"Waited 1.0 seconds (unix:///var/opt/gitlab/redis/redis.socket)"}
21672 {"severity":"ERROR","time":"2024-10-14T09:13:13.373Z","message":"Heartbeat thread error: Waited 1.0 seconds (unix:///var/opt/gitlab/re  21672 dis/redis.socket)"}
21673 {"severity":"ERROR","time":"2024-10-14T09:13:16.788Z","message":"Error fetching job: Waited 7.0 seconds"}
21674 {"severity":"ERROR","time":"2024-10-14T09:13:16.413Z","message":"Error fetching job: Waited 7.0 seconds"}
21675 {"severity":"ERROR","time":"2024-10-14T09:13:17.943Z","message":"Error fetching job: Waited 7.0 seconds"}
```

```
free -h
               total        used        free      shared  buff/cache   available
Mem:           1.9Gi       1.7Gi        62Mi        31Mi       196Mi        67Mi
Swap:          2.0Gi       2.0Gi        20Mi
mainte@devops:~/DevOps/02-Add_Jenkins$ cat /proc/meminfo
MemTotal:        2011148 kB
MemFree:           65984 kB
MemAvailable:     110416 kB
Buffers:            7092 kB
Cached:           186744 kB
SwapCached:       184988 kB
Active:           579976 kB
Inactive:        1159880 kB
Active(anon):     472508 kB
Inactive(anon):  1114900 kB
Active(file):     107468 kB
Inactive(file):    44980 kB
Unevictable:       27760 kB
Mlocked:           27760 kB
SwapTotal:       2097148 kB
SwapFree:          18260 kB
Dirty:               112 kB
Writeback:             0 kB
AnonPages:       1516968 kB
Mapped:           127084 kB
...
```
使用中のメモリが 1.7 GiB で、空きメモリが 62 MiB しかないため、メモリがかなり逼迫していることがわかります。Available は 67 MiB ですが、これは直近で使用されていたバッファやキャッシュも含まれているため、実際にはアプリケーションのために使えるメモリが少ない状況です。

メモリ増設する必要がある。
