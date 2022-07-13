# サーバーレス・アーキテクチャ

## このワークショップはアーカイブ済みで、メンテナンスされていません。コンテンツは読み取り専用です。

Contoso社では料金所管理事業を急速に拡大し、より広い地域で事業を展開しつつあります。この事業は、同社の主要事業であるオンライン決済サービスではないため、クラウドストレージにアップロードされた車両の写真を使用して、多くの新しい料金所からナンバープレート情報を抽出するというプロセスについて、今後の需要に対応するためのスケールアップに苦労しています。現在、同社は多数の画像をバッチ的にサードパーティーに送り、サードパーティーが手動でナンバープレートをCSVファイルに変換し、Contosoに送り返し、同社のオンライン処理システムにアップロードするという手動プロセスを取っています。

彼らは、コスト効率と拡張性の高い方法でこのプロセスを自動化したいと考えています。サーバーレスが最適だと考えていますが、ソリューションを構築するための専門知識は持っていません。

2021年11月

## 対象の読者

アプリケーション開発者

## 要旨

### ワークショップ

このワークショップでは、Azure Functions、Azure Logic Apps、Azure Event Grid、Azure Cosmos DB、およびAzure Data Lake Storageを組み合わせて使用するAzure内のサーバーレスアーキテクチャのセットアップと構成について学習します。このワークショップでの注力ポイントは、プラットフォームからサーバー管理を取り除き、ソリューションを個別に拡張可能な小さなコンポーネントに分解し、顧客が使用した分だけを支払うことを可能にすることです。

このワークショップの終わりには、ビジネスロジックを独立して拡張可能な個別のコンポーネントに分解し、画像アルゴリズムを活用してオブジェクトを検出し、テキストを抽出することができるようになります。 Cosmos DBを高可用性NoSQLデータストアとして活用し、Azure Logic Appsを使ってワークフローを構築し、操作に基づき、条件にあわせてアラートを送信する方法を知ることができます。 最後に、サーバーレスのトポロジーを監視し、変更を自動的に公開するための継続的デプロイメントのDevOpsプロセスを実装するための知識を得ることができます。

### ホワイトボードデザインセッション

このホワイトボード・デザインセッションでは、Azureのサーバーレス・テクノロジーを使用して、データレイクにアップロードされる車両写真をほぼリアルタイムで処理するソリューションをグループで設計します。ナンバープレートデータを取り出し、エクスポート目的で高可用性NoSQLデータストアに格納する必要があります。データエクスポートのプロセスは、新しいナンバープレートデータをファイルストレージにエクスポートし、必要に応じて通知を送信することを調整するサーバーレスAzureコンポーネントがオーケストレーションします。また、Function Apps に新しい変更を自動的に発行するための Continuous Deployment (CD) プロセスを構成します。最後に、処理パイプライン全体を監視し、特に、処理需要に応じたコンポーネントのスケーリングに注意を払う必要があります。

このホワイトボードデザインセッションが終わると、サーバーレスアーキテクチャを活用する最善の方法について、より深い洞察を得ることができます。従来のホスト型 Web アプリケーションやサービスと比較して、わずかなコードと事実上ゼロのインフラで済む、拡張性とコスト効果の高いソリューションを設計する方法をより深く理解できるようになります。

### ハンズオン

このハンズオンでは、Microsoft Azure Functions、Azure Cosmos DB、Azure Event Grid、および関連サービスをベースにしたサンプルが提供されていますので、これらを使用してエンドツーエンドのソリューションを実装します。シナリオでは、Microsoft Azure のさまざまなコンポーネントを使用して、VM、ストレージ、ワークフロー、監視を実装します。ハンズオンは、各自で実施できますが、実体験をモデル化し、ソリューション全体のために各メンバーが専門知識を共有するために、他のメンバーとペアを組んで実施されることを強くお勧めします。

ハンズオンが終了すると、弾力性、拡張性、コスト効率に優れたサーバーレスソリューションの設計、開発、監視に自信を持つことができます。

## Azureサービスや関連製品

- [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)
- [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/what-are-cognitive-services)
- [Azure Computer Vision](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/overview)
- [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview)
- [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)
- [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)
- [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)
- [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)

## Azureソリューション

Cloud-Native Apps

## 参考情報

- [サーバーレス Web アプリケーション 参照アーキテクチャ](https://docs.microsoft.com/azure/architecture/reference-architectures/serverless/web-app)
- [サーバーレスなイベント処理](https://docs.microsoft.com/azure/architecture/reference-architectures/serverless/event-processing)
- [MCW](https://microsoftcloudworkshop.com/)

## サポート

MCW (Microsoft Cloud Workshop) を配信するMicrosoftのSME (Subject Matter Expert) ならびにラーニングパートナーの皆様からのご意見、ご感想をお待ちしております。

**_トラブル時は_**

- まず、記載のすべてのハンズオンの指示 (「実習の前に」のドキュメントを含む) に従っていることを確認します。
- 続いて、問題の詳細を記述したIssueを送信してください。
- プルリクエストは出さないでください。コンテンツ作成者がすべての変更を行い、プルリクエストを出し、承認を得る流れです。

ワークショップの開催を予定している場合は、_早めに資料のレビューとテストを行ってください_。 少なくとも2週間前を推奨します。

### Issueのレビューと解決には5～10営業日ほど要します。
