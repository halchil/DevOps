# はじめに

# 事前準備

# GitLab連携

手順
Step 1: GitLabプラグインのインストール

    Jenkinsのダッシュボードにアクセスします。
    左メニューから「Manage Jenkins」を選択し、その後「Manage Plugins」を選択します。
    「Available」タブを選択し、検索ボックスに「GitLab Plugin」と入力。
    GitLab Plugin をチェックして、「Install without restart（再起動せずにインストール）」をクリック。
        このプラグインにより、GitLabからのWebhook通知を受け取ってビルドを開始する機能が追加されます。

Step 2: ジョブのビルドトリガ設定

    Jenkinsダッシュボードに戻り、新しいジョブを作成するか、既存のジョブを編集します。
    「ビルドトリガ（Build Triggers）」タブで「Build when a change is pushed to GitLab（GitLabへのプッシュでビルド）」を選択します。

Step 3: GitLab Webhookの設定

    GitLabのプロジェクトにアクセスし、左メニューの「Settings」 > 「Webhooks」をクリックします。
    「URL」にJenkinsのGitLab Webhook URLを入力します：

    plaintext

    http://<JenkinsのドメインまたはIP>:8080/gitlab-webhook/

    「Push events」にチェックを入れて、Webhookがプッシュイベントでトリガされるように設定します。
    Webhookを保存します。


Step 3: ジョブを実行して確認

    GitLabのリポジトリで任意の変更（例: README.md にコメントを追加）を行い、コミット＆プッシュします。
    GitLabがJenkinsにWebhookを送り、Jenkinsで自動的にジョブが実行されます。
    Jenkinsのジョブが成功したかどうかは、Jenkinsのダッシュボードから確認できます。
    コンソールログを開いて、次の出力を確認します：

    plaintext

    Hello from Jenkins!

これで、GitLabからプッシュイベントがJenkinsに通知され、最も簡単なジョブ（プリントのみ）が動作することを確認できます。

Step 4: ジョブのテスト

    GitLabのリポジトリに適当な変更を加えてプッシュします。
    Jenkinsが自動的にビルドを開始することを確認します。ジョブのコンソールログで「Hello from Jenkins!」のような出力が確認できれば成功です。


