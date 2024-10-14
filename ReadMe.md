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