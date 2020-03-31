## [CodeDeploy とは](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/welcome.html)

### CodeDeploy を使用すると、以下を容易に行うことができます。
* 新機能の迅速なリリース。
* AWS Lambda 関数のバージョンの更新。
* アプリケーションのデプロイメント中のダウンタイム回避。
* アプリケーションの更新に伴う繁雑さを処理。エラーの発生しやすい手動デプロイに伴うリスクの多くを回避できます。

## Blue/Green デプロイの概要
* AWS Lambda: Lambda 関数のあるバージョンから、同じ Lambda 関数の新しいバージョンにトラフィックが移行します。
* Amazon ECS: Amazon ECS サービスのタスクセットから、同じ Amazon ECS サービスの最新の置き換えタスクセットにトラフィックが移行します。
* EC2/オンプレミス: 元の環境内の、あるインスタンスセットから、インスタンスの置き換え先のセットにトラフィックが移行します。

## [CodeDeploy レポジトリタイプの選択](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/application-revisions-repository-type.html)

* EC2/オンプレミス
* AWS Lambda および Amazon ECS

### 現在、CodeDeploy は次のリポジトリタイプをサポートしています。
* Amazon S3	AWS Lambda デプロイは Amazon S3 リポジトリでのみ機能します。
* GitHub
* Bitbucket



## [CodeDeploy 用にリビジョンを Amazon S3 にプッシュする (EC2/オンプレミス デプロイのみ)](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/application-revisions-push.html)

```json
{
    "Statement": [
        {
            "Action": [
                "s3:PutObject"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::codedeploydemobucket/*",
            "Principal": {
                "AWS": [
                    "111122223333"
                ]
            }
        }
    ]
}
```


## AWS CLI を使用してリビジョンをプッシュする

```
aws deploy push \
  --application-name WordPress_App \
  --description "This is a revision for the application WordPress_App" \
  --ignore-hidden-files \
  --s3-location s3://codedeploydemobucket/WordPressApp.zip \
  --source .
```


## [CodeDeploy を使用したアプリケーションリビジョンの詳細の表示](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/application-revisions-view-details.html)

* CodeDeploy コンソール、AWS CLI、または CodeDeploy API を使用して、
* 指定したアプリケーションに対して AWS アカウントに登録された
* すべてのアプリケーションリビジョンに関する詳細を表示することができます。

### アプリケーションリビジョンの詳細の表示 (コンソール)

* Sign in to the AWS マネジメントコンソール and open the CodeDeploy console at https://console.aws.amazon.com/codedeploy.

注記「CodeDeploy の使用開始」で使用したのと同じアカウントまたは IAM ユーザー情報でサインインします。

* ナビゲーションペインで [デプロイ] を展開し、[アプリケーション] を選択します。
* 注記 エントリが表示されていない場合は、正しいリージョンが選択されていることを確認します。
* ナビゲーションバーのリージョンセレクターで、AWS General Reference の「リージョンとエンドポイント」に
* 一覧表示されているリージョンの 1 つを選択します。CodeDeploy ではこれらのリージョンのみがサポートされます。
* 表示するリビジョンと関連するアプリケーションの名前を選択します。

* [アプリケーションの詳細] ページで [リビジョン] タブを選択し、アプリケーションに登録されているリビジョンを一覧表示します。
* リビジョンを選択し、[詳細を表示] を選択します。



## アプリケーションリビジョンの詳細の表示 (CLI)

* AWS CLI を使用してアプリケーションのリビジョンを表示するには、
* get-application-revision コマンドまたは list-application-revisions コマンドを呼び出します。



* アプリケーション名.アプリケーション名を取得するには、list-applications コマンドを呼び出します。

* GitHub に保存されたリビジョンの場合、GitHub レポジトリ名と、リポジトリにプッシュされたアプリケーションリビジョンを参照するコミットの ID。

* Amazon S3 に保存されたリビジョンの場合、リビジョンを含む Amazon S3 バケット名、アップロードされたアーカイブファイルの名前とファイル形式。
* さらにオプションで、アーカイブファイルの Amazon S3 バージョン ID と ETag。
* バージョン ID、ETag、または両方が register-application-revision の呼び出し中に指定された場合、ここで指定する必要があります。


## [CodeDeploy を使用した Amazon S3 でのアプリケーションリビジョンの登録](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/application-revisions-register.html)

### Amazon S3 での CodeDeploy を使用したリビジョンの登録 (CLI)
1. リビジョンを Amazon S3 にアップロードします。
2. register-application-revision コマンドを呼び出し、次のように指定します。
	* アプリケーション名.アプリケーション名のリストを表示するには、list-applications コマンドを呼び出します。
	* 登録するリビジョンに関する情報:

```
--s3-location bucket=string,key=string,bundleType=tar|tgz|zip,version=string,eTag=string
--s3-location bucket=string,key=string,bundleType=JSON|YAML,version=string,eTag=string
```


## CodeDeploy による GitHub でのリビジョンの登録 (CLI)

1. GitHub リポジトリにリビジョンをアップロードします。
2. register-application-revision コマンドを呼び出し、次のように指定します。

```
--github-location repository=string,commitId=string
```


## [CodeDeploy でのデプロイの使用](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/deployments.html)

CodeDeploy では、デプロイはプロセスであり、
プロセスには 1 つ以上のインスタンスのコンテンツをインストールするコンポーネントが含まれます。

CodeDeploy は、指定した設定ルールに従って、ソースリポジトリに格納されたコンテンツをデプロイします。

EC2/オンプレミス compute platform を使用する場合、インスタンスの同じセットへの 2 つのデプロイは同時に実行できます。

CodeDeploy は、2 つのデプロイタイプのオプション、インプレースデプロイおよび Blue/Green デプロイを提供します。

* インプレイスデプロイ
* Blue/Green デプロイ
	* AWS Lambda compute platform の Blue/Green:
	* Amazon ECS compute platform の Blue/Green:


## トピック
* デプロイの作成
* デプロイの詳細の表示
* デプロイのログデータの表示
* デプロイの停止
* デプロイを使用した再デプロイおよびロールバック
* 異なる AWS アカウントでアプリケーションをデプロイする
* ローカルマシンのデプロイパッケージの検証


## [CodeDeploy を使用してデプロイを作成する](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/deployments-create.html)

* デプロイの前提条件
AWS Lambda コンピューティングプラットフォームのデプロイ前提条件


## デプロイする Lambda 関数バージョンを指定するアプリケーションリビジョン
```
{
    "Statement": [
        {
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::codedeploydemobucket/*",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::80398EXAMPLE:role/CodeDeployDemo"
                ]
            }
        }
    ]
}
```




