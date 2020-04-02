## [CodeDeploy とは](https://docs.aws.amazon.com/ja_jp/codedeploy/latest/userguide/welcome.html)

* CodeDeploy は、Amazon EC2 インスタンスやオンプレミスインスタンス、サーバーレス Lambda 関数、
* または Amazon ECS サービスに対するアプリケーションのデプロイを自動化するデプロイメントサービスです。

* 以下のような、ほぼ無制限の多様なアプリケーションコンテンツをデプロイできます。
	* コード
	* サーバーレス AWS Lambda 関数
	* ウェブファイルおよび設定ファイル
	* 実行可能ファイル
	* packages
	* スクリプト
	* マルチメディアファイル

* CodeDeploy では、サーバーで実行され、Amazon S3 バケット、GitHub リポジトリ、
* または Bitbucket リポジトリに保存されているアプリケーションコンテンツをデプロイできます。
* CodeDeploy では、サーバーレス Lambda 関数をデプロイすることもできます。
* 既存のコードを変更することなく CodeDeploy を使用できます。

### CodeDeploy を使用すると、以下を容易に行うことができます。
* 新機能の迅速なリリース。
* AWS Lambda 関数のバージョンの更新。
* アプリケーションのデプロイメント中のダウンタイム回避。
* アプリケーションの更新に伴う繁雑さを処理。エラーの発生しやすい手動デプロイに伴うリスクの多くを回避できます。


* サービスはインフラストラクチャに合わせてスケールするため、1 つのインスタンスまたは数千のインスタンスに簡単にデプロイできます。

* CodeDeploy は、設定管理、ソース管理、継続的統合、継続的配信、および継続的デプロイのための様々なシステムで動作します。
* 詳細については、「製品の統合」を参照してください。

* また、CodeDeploy コンソールには、レポジトリ、ビルドプロジェクト、デプロイメントアプリケーション、
* パイプラインなど、リソースを迅速に検索するための方法が用意されています。

## AWS CodeDeploy の利点
* サーバーアプリケーション、サーバーレスアプリケーション、コンテナアプリケーション
	* CodeDeploy では、サーバーの従来のアプリケーションと、サーバーレス AWS Lambda 関数バージョン、
	* または Amazon ECS アプリケーションを展開するアプリケーションの両方をデプロイ
* 自動デプロイ
	* 開発、テスト、および本番稼働環境にまたがるアプリケーションのデプロイを完全に自動化

* ダウンタイムの最小化
	* EC2/オンプレミス compute platform を使用している場合、CodeDeploy はアプリケーションの可用性を最大化
	* 複数の Amazon EC2 インスタンスにまたがってローリング更新を実行
	* Blue/Green デプロイの間、最新アプリケーションのリビジョンは、置き換え先インスタンスにインストール

* 停止してロールバック
	* エラーが発生した場合、自動または手動でデプロイを停止してロールバックできる
* コントロールの一元化
	* CodeDeploy コンソールまたは AWS CLI でデプロイを起動し、そのステータスを追跡できます。
	* 各アプリケーションのリビジョンがいつデプロイされ、どの Amazon EC2 インスタンスがリストされているかを示すレポートを受け取ります。

* 導入が簡単
	* CodeDeploy は、プラットフォームに依存せず、すべてのアプリケーションで動作

* 同時デプロイ
	* EC2/オンプレミス compute platform を使用する複数のアプリケーションがある場合、
	* CodeDeploy は同じインスタンスのセットでこれらを同時にデプロイできます。

## CodeDeploy コンピューティングプラットフォームの概要


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




