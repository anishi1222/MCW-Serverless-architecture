![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
サーバーレス・アーキテクチャ
</div>

<div class="MCWHeader2">
ハンズオン手順
</div>

<div class="MCWHeader3">
2021年11月
</div>

本書に記載されている情報は、URLやその他のインターネット上のウェブサイトを含め、予告なく変更されることがあります。特に断りのない限り、ここに記載されている会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントの例は架空のものであり、実在の会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントとの関連を意図したり推論したりするものではありません。適用されるすべての著作権法を遵守することは、ユーザーの責任です。著作権に基づく権利を制限することなく、マイクロソフトの書面による明示的な許可がない限り、本書のいかなる部分も、いかなる形式または手段 (電子的、機械的、複写、記録、その他) で、あるいはいかなる目的においても、複製、検索システムへの保存または導入、転送することを禁じます。

マイクロソフトは、本書の主題を対象とする特許、特許出願、商標、著作権、またはその他の知的財産権を有している場合があります。マイクロソフトの書面によるライセンス契約において明示的に規定されている場合を除き、本書の提供は、これらの特許、商標、著作権、またはその他の知的財産に対するいかなるライセンスもお客様に与えるものではありません。

メーカー名、製品名、URL は情報提供のみを目的としており、マイクロソフトは、これらのメーカーまたはマイクロソフトのテクノロジーによる製品の使用に関して、明示的、黙示的、法定的を問わず、いかなる表明および保証も行いません。メーカーや製品を掲載することは、マイクロソフトがそのメーカーや製品を推奨していることを意味するものではありません。また、第三者のサイトへのリンクが張られていることがあります。そのようなサイトはマイクロソフトの管理下にはなく、マイクロソフトは、リンク先のサイトのコンテンツ、リンク先のサイトに含まれるリンク、またはそのようなサイトの変更もしくは更新について、一切の責任を負いません。マイクロソフトは、リンク先サイトから受信するウェブキャスティングまたはその他の形式の伝送について責任を負いません。マイクロソフトは、これらのリンクをお客様の便宜のためにのみ提供しており、リンクを掲載することは、マイクロソフトが当該サイトまたはそこに含まれる製品を推奨していることを意味するものではありません。

© 2021 Microsoft Corporation. All rights reserved.

マイクロソフトと <https://www.microsoft.com/legal/intellectualproperty/Trademarks/Usage/General.aspx> に記載の商標は、マイクロソフトグループの商標です。その他の商標は各所有者に帰属します。

**目次**

<!-- TOC -->

- [サーバーレス・アーキテクチャ ハンズオン手順](#サーバーレス・アーキテクチャ-ハンズオン手順)
  - [ハンズオンの目的](#ハンズオンの目的)
  - [概要](#概要)
  - [ソリューション アーキテクチャ](#ソリューション-アーキテクチャ)
  - [必要なもの](#必要なもの)
  - [演習 1: 画像処理関数アプリとデータエクスポート関数アプリを開発し発行する](#演習-1-画像処理関数アプリとデータエクスポート関数アプリを開発し発行する)
    - [Task 1: ハンズオンVMに接続する](#task-1-ハンズオンVMに接続する)
    - [Task 2: Visual Studioでスターターソリューションを開く](#task-2-Visual-Studioでスターターソリューションを開く)
    - [Task 3: ProcessImage関数アプリを完成させる](#task-3-ProcessImage関数アプリを完成させる)
    - [Task 4: Visual Studioから関数アプリを公開する](#task-4-Visual-Studioから関数アプリを公開する)
  - [演習 2: Azure Portalで関数を作成する](#演習-2-Azure-Portalで関数を作成する)
    - [Task 1: ナンバープレートデータをAzure Cosmos DBに保存する関数を作成する](#task-1-ナンバープレートデータをAzure-Cosmos-DBに保存する関数を作成する)
    - [Task 2: Event GridサブスクリプションをSavePlateData関数に追加する](#task-2-Event-GridサブスクリプションをSavePlateData関数に追加する)
    - [Task 3: Azure Cosmos DB出力バインドをSavePlateData関数に追加する](#task-3-Azure-Cosmos-DB出力バインドをSavePlateData関数に追加する)
    - [Task 4: 手作業による検証情報をAzure Cosmos DBに保存する関数を作成する](#task-4-手作業による検証情報をAzure-Cosmos-DBに保存する関数を作成する)
    - [Task 5: Event GridサブスクリプションをQueuePlateForManualCheckup関数に追加する](#task-5-Event-GridサブスクリプションをQueuePlateForManualCheckup関数に追加する)
    - [Task 6: Azure Cosmos DB出力バインドをQueuePlateForManualCheckup関数に追加する](#task-6-Azure-Cosmos-DB出力バインドをQueuePlateForManualCheckup関数に追加する)
  - [演習 3: Application Insightsで関数アプリを監視する](#演習-3-Application-Insightsで関数アプリを監視する)
    - [Task 1: ライブ メトリックを使ってリアルタイムに関数アプリを監視する](#task-1-ライブ-メトリックを使ってリアルタイムに関数アプリを監視する)
    - [Task 2: リソース制約がある場合に関数アプリが動的にスケールする様子を観察する](#task-2-リソース制約がある場合に関数アプリが動的にスケールする様子を観察する)
  - [演習 4: Azure Cosmos DBでデータを調査する](#演習-4-Azure-Cosmos-DBでデータを調査する)
    - [Task 1: Azure Cosmos DB データエクスプローラーを使用する](#task-1-Azure-Cosmos-DB-データエクスプローラーを使用する)
  - [演習 5: データエクスポートワークフローを作成する](#演習-5-データエクスポートワークフローを作成する)
    - [Task 1: ロジックアプリを作成する](#task-1-ロジックアプリを作成する)
  - [演習 6: 関数アプリの継続的デプロイを構成する](#演習-6-関数アプリの継続的デプロイを構成する)
    - [Task 1: gitリポジトリにVisual Studioのソリューションを追加し、GitHubに展開する](#task-1-gitリポジトリにVisual-Studioのソリューションを追加し、GitHubに展開する)
    - [Task 2: 関数アプリがGitHubリポジトリを使用して継続的にデプロイされるように設定する](#task-2-関数アプリがGitHubリポジトリを使用して継続的にデプロイされるように設定する)
    - [Task 3: ExportLicensePlates 関数のコードを完成させ、変更を GitHub にプッシュしてデプロイを開始する](#task-3-ExportLicensePlates-関数のコードを完成させ、変更を-GitHub-にプッシュしてデプロイを開始する)
  - [演習 7: ワークフローを再実行し、データエクスポートを確認する](#演習-7-ワークフローを再実行し、データエクスポートを確認する)
    - [Task 1: 画像のアップロードを再実行する](#task-1-画像のアップロードを再実行する)
    - [Task 2: ロジックアプリを実行する](#task-2-ロジックアプリを実行する)
    - [Task 3: エクスポートされたCSVファイルを見る](#task-3-エクスポートされたCSVファイルを見る)
  - [ハンズオン終了後](#ハンズオン終了後)
    - [Task 1: Azureリソースを配置したリソースグループを削除する](#task-1-Azureリソースを配置したリソースグループを削除する)
    - [Task 2: GitHubリポジトリを削除する](#task-2-GitHubリポジトリを削除する)

<!-- /TOC -->

# サーバーレス・アーキテクチャ ハンズオン手順

## ハンズオンの目的

このハンズオンでは、Microsoft Azure Functions、Azure Cosmos DB、Azure Event Grid、および関連サービスをベースに、提供されたサンプルを使用してエンドツーエンドのソリューションを実装します。シナリオでは、Microsoft Azure のさまざまなコンポーネントを使用して、コンピュート、ストレージ、ワークフロー、およびモニタリングを実装する予定です。このハンズオンは各自のペースで実施できますが、ラボで他のメンバーとペアを組んで、実体験をモデル化し、ソリューション全体のために各メンバーが専門知識を共有することを強くお勧めします。

ハンズオンが終了すると、弾力性、拡張性、コスト効率の高いサーバーレスソリューションの設計、開発、監視に自信を持つことができます。

## 概要

Contoso社では料金所管理事業を急速に拡大し、より広い地域で事業を展開しつつあります。この事業は、同社の主要事業であるオンライン決済サービスではないため、クラウドストレージにアップロードされた車両の写真を使用して、多くの新しい料金所からナンバープレート情報を抽出するというプロセスについて、今後の需要に対応するためのスケールアップに苦労しています。現在、同社は多数の画像をバッチ的にサードパーティーに送り、サードパーティーが手動でナンバープレートをCSVファイルに変換し、Contosoに送り返し、同社のオンライン処理システムにアップロードするという手動プロセスを取っています。

彼らは、コスト効率と拡張性の高い方法でこのプロセスを自動化したいと考えています。サーバーレスが最適だと考えていますが、ソリューションを構築するための専門知識は持っていません。

## ソリューション アーキテクチャ

以下は、このハンズオンで構築するソリューションアーキテクチャの図です。これをよく読んで、さまざまなコンポーネントに取り組みながら、ソリューションの全体像を理解してください。

![The Solution diagram is described in the text following this diagram.](media/preferred-solution.png 'Solution diagram')

このソリューションでは、撮影された車両の写真を、**Azure Data Lake Storage Gen2**コンテナにアップロードします。データレイクストレージコンテナに対して、**Azure Event Grid** サブスクリプションを作成します。新しい blob が作成されると、写真処理の **Azure Function** エンドポイントを呼び出すイベントが呼び出され、次に写真が **Computer Vision API** サービスに送信されてナンバープレートデータを抽出します。処理が成功し、ナンバープレート番号が取得できた場合、関数アプリはデータと共に新しいEvent Gridイベントを、イベントタイプ`savePlateData`で待機しているEvent Gridトピックに発行します。しかしながらナンバープレート番号の取得ができなかった場合、関数アプリは`queuePlateForManualCheckup`と呼ばれるイベントタイプで待機しているEvent Gridトピックに対し、新たなEvent Gridイベントを発行します。新しいイベントがイベントグリッドトピックに追加されたときに、2つの別々の関数がトリガーされるように設定されています。特定のイベントタイプで各々フィルタリングすると、Cosmos DB 出力バインディングを使用して、出力に合わせた**Azure Cosmos DB** コレクションに関連するデータが保存されます。15分間隔で実行される**Logic App**は、HTTPトリガーでAzure Functionを実行し、Cosmos DBから新しいナンバープレートデータを取得し、Blobストレージに保存された新しいCSVファイルにエクスポートする役割を担います。エクスポート対象の新しいナンバープレートレコードが見つからない場合、Logic AppはOffice 365サブスクリプションを使ってカスタマーサービス部門にメール通知を送信します。**Application Insights** は、サーバーレスアーキテクチャでデータを処理しているときに、すべての Azure Functions をリアルタイム監視するために使用します。このリアルタイム監視により、動的なスケールを直接観察し、特定のイベントが発生したときにアラートを構成できます。**Azure Key Vault** は、接続文字列やアクセスキーなどのシークレットを安全に保管するために使用します。Key Vaultは、各機能アプリのシステムに割り当てられたマネージドIDに対して設定したKey Vault内のアクセスポリシーを通じて、関数アプリからアクセスします。

## 必要なもの

- Microsoft Azureのサブスクリプション
- 以下のソフトウェアが構成済みのローカルマシンもしくは仮想マシン (VM) (**ハンズオン前日には構成しておいてください!**):
  - Visual Studio Community 2019 もしくはそれ以後
    - <https://www.visualstudio.com/vs/>
  - Visual Studio用のAzure 開発ワークロード
    - <https://docs.microsoft.com/azure/azure-functions/functions-develop-vs#prerequisites>
  - .NET Core 3.1 SDK
    - <https://www.microsoft.com/net/download/windows>
- Office 365のアカウント。お持ちでないなら、 [トライアル環境のサインアップ](https://portal.office.com/Signup/MainSignup15.aspx?Dap=False&QuoteId=79a957e9-ad59-4d82-b787-a46955934171&ali=1) が可能です。  
- GitHubのアカウント。 [無料のアカウントを作成](https://github.com/join) できます。

## 演習 1: 画像処理関数アプリとデータエクスポート関数アプリを開発し発行する

**所要時間**: 45分

Visual Studio と統合された Azure Functions ツールを使用して、ローカルで関数アプリを開発およびデバッグし、Azure に公開します。スタータープロジェクトソリューションであるTollBoothsには、必要なコードのほとんどが含まれています。Azureにデプロイする前に、不足しているコードを追加します。

### 参考情報

|                                       |                                                                        |
| ------------------------------------- | ---------------------------------------------------------------------- |
| **Description**                       | **Link**                                                              |
| Azure Functions をローカルでコーディングしてテストする | <https://docs.microsoft.com/azure/azure-functions/functions-develop-local> |

### Task 1: ハンズオンVMに接続する

このタスクでは、ハンズオンVMへのRDP接続を作成します。

1. [Azure portal](https://portal.azure.com) で、  **リソースグループ** をAzureサービスリストから選択します。

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. リストから**hands-on-lab-SUFFIX**リソースグループを選択します。

   ![The "hands-on-lab-SUFFIX" resource group is highlighted.](media/resource-groups.png "Resource groups list")

3. リソースグループ内のリソースリストで、**LabVM仮想マシン**リソースを選択します。

   ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and LabVM is highlighted.](media/resource-group-resources-labvm.png "LabVM in resource group list")

4. LabVMの画面で、トップメニューから**接続**を選択し、続いて**RDP**を選択します。

   ![The LabVM blade is displayed, with the Connect button highlighted in the top menu.](media/connect-vm-rdp.png "Connect to Lab VM")

5. 接続画面で、**RDPファイルのダウンロード**を選択し、ダウンロードしたRDPファイルを開きます。

   ![The Connect to virtual machine blade is displayed, and the Download RDP File button is highlighted.](media/connect-to-virtual-machine.png "Connect to virtual machine")

6. リモートデスクトップの画面で、**接続**をクリックします。

   ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](media/remote-desktop-connection.png "Remote Desktop Connection dialog")

7. 以下の資格情報を入力し、**OK**を選択します。

   - **ユーザー名**: demouser
   - **パスワード**: Password.1!!

   ![The credentials specified above are entered into the Enter your credentials dialog.](media/rdc-credentials.png "Enter your credentials")

8. リモートコンピューターのIDを確認できない旨のメッセージが表示された場合、**はい**を選択して接続します。

   ![In the Remote Desktop Connection dialog box, a warning states that the remote computer's identity cannot be verified and asks if you want to continue anyway. At the bottom, the Yes button is highlighted.](media/remote-desktop-connection-identity-verification-labvm.png "Remote Desktop Connection dialog")

### Task 2: Visual Studioでスターターソリューションを開く

1. 接続したLabVMで、ファイルエクスプローラを開いて、`C:\ServerlessMCW\MCW-Serverless-architecture-main\Hands-on lab\lab-files\src\TollBooth`に移動します。

    > **注意**: ファイルが `C:\ServerlessMCW\` 以下にあることを確認してください。ファイルが長いルートパスの配下にある場合（例： `C:\Users\workshop\Downloads\`) 後の作業で `The specified path, file name, or both are too long. The fully qualified file name must be less than 260 characters, and the directory name must be less than 248 characters`　という問題がビルド時に発生します。

2. 手順1で開いた **TollBooth**フォルダーの `TollBooth.sln` ファイルをダブルクリックして、Visual Studio Solutionを開きます。

    ![In the TollBooth folder in File Explorer, TollBooth.sln is highlighted.](media/file-explorer-toll-booth-sln.png "File Explorer")

3. ファイイルの開き方に関するプロンプトが現れたら、 **Visual Studio 2019** を選択し、**OK**をクリックします。

   ![Visual Studio 2019 is highlighted in the How do you want to open this file? dialog.](media/solution-file-open-with.png "Visual Studio 2019")

4. ご自身のAzureアカウント資格情報でVisual Studioにサインインします。

   ![The Sign in button is highlighted on the Visual Studio Welcome screen.](media/visual-studio-sign-in.png "Visual Studio 2019")

5. セキュリティ警告が出る場合、 **Ask me for every project in this solution** のチェックを外して **OK**を選択します。

6. ソリューションに以下の2プロジェクトが含まれていることを確認します。

   - `TollBooth`
   - `UploadImages`

   > **注意**: UploadImagesプロジェクトは、クルマの写真をアップロードし、サーバーレスアーキテクチャの拡張をテストするために利用します。

   ![The two projects listed above are highlighted in Solution Explorer.](media/visual-studio-solution-explorer-projects.png 'Solution Explorer')

7. Visual StudioからAzureサブスクリプションへの接続を確認するため、**Cloud Explorer**を**View**メニューから開き、Azureサブスクリプションに接続できることを確認します。

   ![In Cloud Explorer, the list of Azure subscriptions is shown. A single subscription is highlighted and expanded in the list.](media/vs-cloud-explorer.png 'Cloud Explorer')

   > **注意**: Azureサブスクリプションのリソースを確認する前に、アカウントアイコンを選択してAzureアカウントでログインする必要がある可能性があります。

8. ファイルエクスプローラで **src** サブフォルダに移動します。ここで、 **license plates** サブフォルダを開きます。このフォルダにはソリューションのテスト用にサンプルのナンバープレートの写真が入っています。このうち1枚の写真はOCR処理に失敗するもので、このような障害を扱うようにワークロードがどのように設計されているかを示すためのものです。UploadImagesプロジェクトは、拡張性をテストするために、1,000枚の写真をアップロードするために**copyfrom**フォルダを使います。

### Task 3: ProcessImage関数アプリを完成させる

スタータープロジェクトの一部のコンポーネントを完成させる必要があります。これらはコード内で `TODO` としてマークされています。最初の `TODO` アイテムは `ProcessImage` 関数の中にあります。Computer Vision サービスを呼び出す `FindLicensePlateText` クラスと、処理結果を先に作成した Event Grid トピックに送信するための `SendToEventGrid` クラスをアップデートします。

> **注意**: NuGet パッケージのバージョンは**更新しないでください**。このソリューションは、現在定義されている NuGet パッケージのバージョンで機能するように構築されています。これらのパッケージを新しいバージョンに更新すると、予期しない結果を引き起こす可能性があります。

1. Visual Studioの **View** メニューから **Task List** を選択します。

    ![The Visual Studio Menu displays, with View and Task List selected.](media/vs-task-list-link.png 'Visual Studio Menu')

2. `TODO`タスクのリストが表示されます。各タスクは、完了する必要があるコードの1行を表しています。

    ![A list of TODO tasks, including their description, project, file, and line number are displayed.](media/vs-task-list.png 'TODO tasks')

    > **注意**: TODOがソートされていない場合は、**Description**を選択して並べ替えてください。

3. Visual Studio ソリューションエクスプローラーで、**TollBooth** プロジェクトを展開し、`ProcessImage.cs`をダブルクリックして、ファイルを開きます。

    > Run メソッドには **FunctionName** 属性が付加されており、Azure Function の名前が `ProcessImage` に設定されていることに注意してください。この関数アプリは、Event Grid サービスから送られてくる HTTP リクエストをトリガーとして実行されます。イベントサブスクリプションを作成することで、関数アプリのURLで通知を受け取りたいことをEvent Gridに伝えます。この設定は後のタスクで行います（Blob作成イベントをサブスクライブする）。この関数アプリのトリガーは、「Before the hands-on lab guide」のARMテンプレートで作成されたデータレイクストレージアカウントのimagesコンテナーに追加される新しいBlobを監視します。Event Grid通知から関数アプリに渡されるデータには、BlobのURLが含まれています。このURLを入力バインディングに渡し、データレイクストレージからアップロードされた画像を取得します。

    ![The ProcessImage.cs file is highlighted within the TollBooth project in the Visual Studio Solution Explorer.](media/visual-studio-solution-explorer-process-image.png "Solution Explorer")

4. Visual Studioの下部にある**Task List**ペインで、`TODO 1`をダブルクリックします。これで最初の`TODO`タスクに移動できます。

    ![TODO 1 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-1.png "Task List")

5. `TODO 1` のコメントの下の行のコードを、以下のコードで更新します。

    ```csharp
    // **TODO 1: Set the licensePlateText value by awaiting a new FindLicensePlateText.GetLicensePlate method.**
    licensePlateText = await new FindLicensePlateText(log, _client).GetLicensePlate(licensePlateImage);
    ```

6. タスクリストの `TODO 2` をダブルクリックし、 `FindLicensePlateText.cs` ファイルを開きます。

    > このクラスは、Computer VisionサービスのRead APIに接続して、OCRを使用して写真からナンバープレートテキストを検索・抽出する役割を担っています。このクラスは、過渡的なエラーを処理するのに役立つオープンソースの.NETライブラリである[Polly](https://github.com/App-vNext/Polly)を使用して、resilienceパターンを実装する方法を示していることにも注目してください。これは、下流のサービス（この場合はComputer Visionサービス）に過負荷をかけないようにするために有用です。このことは、後でFunctionのスケーラビリティを可視化するときに実証します。
    
    ![TODO 2 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-2.png "Task List")

7. 以下のコードは、FindLicensePlateText.csのタスクが完了したことを表しています。

    ```csharp
    // TODO 2: Populate the below two variables with the correct AppSettings properties.
    var uriBase = Environment.GetEnvironmentVariable("computerVisionApiUrl");
    var apiKey = Environment.GetEnvironmentVariable("computerVisionApiKey");
    ```

8. タスクリストの `TODO 3` をダブルクリックし、 `SendToEventGrid.cs`を開きます。

    > このクラスは、イベントタイプとナンバープレートデータを含むEventをEvent Gridトピックに送信する役割を担います。イベントリスナーは、イベントタイプを使用して、処理する必要のあるイベントをフィルタリングして処理します。ここで定義されたイベントタイプ（Send メソッドに渡される最初のパラメータ）は、後でプロビジョニングした 2 つ目の Function App で新しい関数を作成するときに使用されるので、メモしておいてください。

    ![TODO 3 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-3.png "Task List")

9. 以下のコードを使って `SendToEventGrid.cs`の`TODO 3`を完成させます。

    ```csharp
    // TODO 3: Modify send method to include the proper eventType name value for saving plate data.
    await Send("savePlateData", "TollBooth/CustomerService", data);
    ```

10. `TODO 4` は、`SendLicensePlateData(LicensePlateData data)` の `else` ブロックです。 次のコードで `SendToEventGrid.cs` の `TODO 4` を完成させてください。

    ```csharp
    // TODO 4: Modify send method to include the proper eventType name value for queuing plate for manual review.
    await Send("queuePlateForManualCheckup", "TollBooth/CustomerService", data);
    ```

    > **注意**: `TODO` 5、6、7はこのガイドの後で完成させます。

### Task 4: Visual Studioから関数アプリを公開する

このタスクでは、Visual Studio のスターター プロジェクトから、Azure にプロビジョニングした既存の Function App に Function App を公開します。

1. Visual Studioのソリューションエクスプローラーを使用して、**TollBooth**プロジェクトに移動します。

2. **TollBooth** プロジェクトを右クリックし、コンテキストメニューから **Publish** を選択します。

    ![In Solution Explorer, the TollBooth is selected, and within its context menu, the Publish item is selected.](media/image39.png "Solution Explorer")

3. Publishウィンドウで、**Azure**を選択し、**Next**を選択します。

    ![In the Pick a publish target window, the Azure Functions Consumption Plan is selected in the left pane. The Select Existing radio button is selected in the right pane, and the Run from package file (recommended) checkbox is unchecked. The Create Profile button is also selected.](media/vs-publish-function.png 'Publish window')

4. ターゲットとして **Azure Function App (Windows)** を選択し、**Next**を選択します。

    ![The specific target screen of the Publish dialog is shown with the Azure Function App (Windows) item selected and the Next button highlighted.](media/vs-publish-specific-target.png "Publish specific target")

5. **Publish** ダイアログでは

    - ご利用の**Subscription**を選択 ①
    - **View**の下の**Resource Group**を選択②
    - **Function Apps** ③で、ご自身の**hands-on-lab-SUFFIX**リソースグループを開き、**TollBoothFunctions-SUFFIX**の関数アプリを選択
    - **`Run from package file` ④のチェックを外す**

    ![In the App Service form, Resource Group displays in the View field, and in the tree-view below, the hands-on-lab-SUFFIX folder is expanded, and TollBoothFunctionApp is selected.](media/vs-publish-function2.png 'Publish window')

    > **重要**: パッケージファイルから実行しないのは、後でGitHubからデプロイする際、Function AppがZIPデプロイ用に設定されているとビルドがスキップされるからです。

6. **Finish** を選択します。 Azure Function App の公開用 XML ファイル（拡張子は `.pubxml`）が作成されます。

7. **Publish** を選択し、処理を開始します。Visual Studio の Output ウィンドウを見ながら、Function App が公開されるのを確認します。完了すると、`========================================= Publish: 1 成功、0 失敗、0 スキップ ==========` のようなメッセージが確認できるはずです。

    > **注意**: 注**：Azure 上の関数のバージョンを更新するように促された場合は、**Yes** を選択します。

    ![The Publish button is selected.](media/vs-publish-function3.png "Publish")

8. ブラウザの新しいタブまたはインスタンスを使用して、 [Azure portal](https://portal.azure.com) を開きます。

9. **hands-on-lab-SUFFIX** リソースグループを開き、先ほど公開した **TollBoothFunctions** 関数アプリを選択します。

10. 左側のナビゲーションメニューから **Functions** ①を選択します。Visual Studioソリューションから公開した両方の関数が表示されます②。

    ![In the Function Apps blade, in the left tree-view, both TollBoothFunctionApp and Functions (Read Only) are expanded. Beneath Functions (Read Only), two functions ExportLicensePlates and ProcessImage are highlighted.](media/dotnet-functions.png 'TollBoothFunctionApp blade')

11. ここで、ProcessImage 関数に Event Grid サブスクリプションを追加し、新しい画像がデータレイクストレージコンテナに追加されたときにこの関数がトリガーされるようにする必要があります。

    - **ProcessImage** 関数を選択
    - 左側のメニューで **統合** を選択 ①
    - **Event Grid Trigger (eventGridEvent)** ②を選択
    - **Event Grid サブスクリプションの作成** ③を選択

    ![In the TollboothFunctionApp tree-view, the ProcessImage function is selected. In the code window pane, the Add Event Grid subscription link is highlighted.](media/processimage-add-eg-sub.png 'ProcessImage function')

12. **イベント サブスクリプションの作成**の画面で、以下の設定を指定します。

    - **名前**: **processimagesub**のような一意な名前を入力します (緑色のチェックマークが現れることを確認します)。
    - **イベントスキーマ**: **イベント グリッド スキーマ**を選択します。
    - **Topic Type**: **Storage Accounts (Blob & GPv2)**を選択します。
    - **Subscription**: このハンズオンで利用しているサブスクリプションを選択します。
    - **Resource Group**: 既存リソースグループから**hands-on-lab-SUFFIX** リソースグループを選択します。
    - **Resource**: データレイクストレージアカウントを選択します。`datalake`で始まるアカウント1個のみのはずです。
    - **システムトピック名**: **processimagesubtopic**と入力します。
    - **イベントの種類のフィルター**: イベントの種類のドロップダウンリストから、**Blob Created**だけを選択します。
    - **エンドポイントのタイプ**: エンドポイントは `Azure 関数` のままにします。
    - **エンドポイント**: `ProcessImage`のままにしておきます。

    ![In the Create event subscription form, the fields are set to the previously defined values.](media/process-image-sub-topic.png)

13. **作成**を選択します。

## 演習 2: Azure Portalで関数を作成する

**所要時間**: 45分

この演習では、Azure Portalから2個のAzure関数アプリをNode.jsで作成します。これらはEvent Gridでトリガーされ、ProcessImage関数で処理したナンバープレートの結果をAzure Cosmos DBに格納します。

### 参考情報

|                  |          |
| ---------------- | -------- |
| **Description**  | **Link** |
| Azure Portal で初めての関数を作成する | <https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal> |
| Azure Functions と Azure Cosmos DB を使用して非構造化データを格納する | <https://docs.microsoft.com/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb> |

### Task 1: ナンバープレートデータをAzure Cosmos DBに保存する関数の作成

このタスクでは、Event Grid をトリガーとして、正常に処理されたナンバープレートデータを Azure Cosmos DB に出力する Node.js 関数を新規に作成します。

1. ブラウザの新しいタブまたはインスタンスを使用して、[Azure Portal](https://portal.azure.com) に移動します。

2. **hands-on-lab-SUFFIX** リソースグループを開き、**TollBoothEvents** で始まる名前の Azure関数アプリを選択します。

3. 左側のメニューから **関数** を選択し、**+作成** を選択します。

    ![In the Function Apps blade, the TollBoothEvents application is selected. In the Overview tab, the + Create function button is selected.](media/functions-new.png 'TollBoothEvents blade')

4. **関数の作成**フォームで以下のように設定します。

   - **テンプレートの選択**フィルターに、`event grid` を入力 ①
   - **Azure Event Grid trigger** テンプレートを選択 ②
   - **新しい関数** の名前フィールドに `SavePlateData` という名前を入力 ③
   - **作成** ボタンを選択 ④

    ![In the Create Function form, event grid is entered into the filter box, the Azure Event Grid trigger template is selected and highlighted, and SavePlateData is entered in the Name field and highlighted.](media/new-function-save-plate-data.png "Create Function form")

5. **SavePlateData** 関数の画面で、左側のメニューから **コードとテスト** を選択し、新しい `SavePlateData` 関数の `index.js` ファイル内のコードを以下のものに置き換えます。

    ```javascript
    module.exports = function(context, eventGridEvent) {
        context.log(typeof eventGridEvent);
        context.log(eventGridEvent);

        context.bindings.outputDocument = {
            fileName: eventGridEvent.data['fileName'],
            licensePlateText: eventGridEvent.data['licensePlateText'],
            timeStamp: eventGridEvent.data['timeStamp'],
            exported: false
        };

        context.done();
    };
    ```

    ![The function code is displayed.](media/saveplatedata-code.png "SavePlateData Code + Test")

6. **保存**を選択します。

### Task 2: Event GridサブスクリプションをSavePlateData関数に追加する

このタスクでは、SavePlateData関数にEvent Gridサブスクリプションを追加します。これにより、savePlateDataイベントタイプを含む Event Gridトピックに送信されたイベントが、この関数にルーティングされるようになります。

1. SavePlateData関数を開いた状態で、左メニューの**統合**を選択した後、**Event Grid Trigger（eventGridEvent）**を選択し、続いて**Event Gridサブスクリプションの作成**を選択します。

    ![In the SavePlateData blade code window, the Add Event Grid subscription link is selected.](media/saveplatedata-add-eg-sub.png 'SavePlateData blade')

2. **イベント サブスクリプションの作成**の画面で、以下の構成を指定します。

    - **名前**: `saveplatedatasub`のような一意の名前を指定します（緑色のチェックマークが現れることを確認します）。
    - **イベントスキーマ**: **イベントグリッドスキーマ**を選択します。
    - **Topic Type**: **Event Grid Topics**を選択します。
    - **Subscription**: このハンズオンで利用しているサブスクリプションを選択します。
    - **Resource Group**: 既存リソースグループから**hands-on-lab-SUFFIX** リソースグループを選択します。
    - **Resource**: Event Grid Topicを選択します。`eventgridtopic-`で始まるサービス1個のみのはずです。
    - **イベントの種類**: **イベントの種類の追加**を選択して、新たなイベントタイプとして`savePlateData` を入力します。これにより、このEventの種類でのみこの関数が呼び出されるようになります。
    - **エンドポイントのタイプ**: `Azure関数`のままにします。
    - **エンドポイント**: Leave as `SavePlateData`のままにします。

    ![In the Create Event Subscription blade, fields are set to the previously defined values.](media/saveplatedata-eg-sub.png "Create Event Subscription")

3. **作成**を選択し、トリガーの編集画面を閉じます。

### Task 3: Azure Cosmos DB出力バインドをSavePlateData関数に追加する

このタスクでは、SavePlateData 関数に Azure Cosmos DB 出力バインディングを追加し、Processed コレクションへのデータ保存を可能にします。

1. **SavePlateData**関数の統合画面で、**出力**の下の **+出力の追加** を選択します。

2. **出力の作成** 画面で、以下の設定をします。

    - **バインドの種類**として `Azure Cosmos DB` を選択 ①
    - **Cosmos DB account connection**のドロップダウンリストの下にある、**New**のリンクを選択 ②
    - `cosmosdb-`で始まる名前の接続を選択 ③  
    - **OK**を選択 ④

    ![The Add Output link is highlighted with an arrow pointing to the highlighted binding type in the Create Output blade.](media/function-output-binding-type.png "Create Output")

3. 以下の追加設定項目を出力の作成画面で指定します。

    - **Document parameter name**: `outputDocument`のままにしておきます。
    - **Database name**: `LicensePlates`と入力します。
    - **Collection name**: `Processed`と入力します。

4. **OK**を選択します。

    ![Under Azure Cosmos DB output the following field values display: Document parameter name, outputDocument; Collection name, Processed; Database name, LicensePlates; Azure Cosmos DB account connection, cosmosdb_DOCUMENTDB.](media/saveplatedata-cosmos-integration.png 'Azure Cosmos DB output section')

5. **SavePlateData**関数を閉じます。

### Task 4: 手作業による検証情報をAzure Cosmos DBに保存する関数を作成する

このタスクでは、Event Grid をトリガーとして、手作業での確認が必要な写真の情報を Azure Cosmos DB に出力する別の関数を新規作成します。 これは、**TollBoothEvents** で始まる Azure Function App 内にあります。

1. 左側のメニューから **関数** を選択し、**+作成** を選択します。

    ![In the Function Apps blade, the TollBoothEvents application is selected. In the Overview tab, the + Create button is selected.](media/functions-new.png 'TollBoothEvents blade')

2. **関数の作成**フォームで以下のように設定します。

    - **テンプレートの選択**フィルターに、`event grid` を入力 ①
    - **Azure Event Grid trigger** テンプレートを選択 ②
    - **新しい関数** の名前フィールドに`QueuePlateForManualCheckup`を入力 ③
    - **作成** ボタンを選択 ④

    ![In the Create function form, event grid is entered into the filter box. The Azure Event Grid trigger template is selected and highlighted, and QueuePlateForManualCheckup is entered in the Name field and highlighted.](media/new-function-manual-checkup.png "Create function form")

3. **QueuePlateForManualCheckup** 関数の画面で、左側のメニューから **コードとテスト** を選択し、新しい `QueuePlateForManualCheckup` 関数の `index.js` ファイル内のコードを以下のものに置き換えます。

    ```javascript
    module.exports = async function(context, eventGridEvent) {
        context.log(typeof eventGridEvent);
        context.log(eventGridEvent);

        context.bindings.outputDocument = {
            fileName: eventGridEvent.data['fileName'],
            licensePlateText: '',
            timeStamp: eventGridEvent.data['timeStamp'],
            resolved: false
        };

        context.done();
    };
    ```

4. **保存**を選択します。

### Task 5: Event GridサブスクリプションをQueuePlateForManualCheckup関数に追加する

このタスクでは、QueuePlateForManualCheckup関数にEvent Gridサブスクリプションを追加します。これにより、queuePlateForManualCheckupイベントタイプを含む Event Gridトピックに送信されたイベントが、この関数にルーティングされるようになります。

1. **QueuePlateForManualCheckup**関数を開いた状態で、①左メニューの**統合**を選択し、②**Event Grid Trigger（eventGridEvent）**を選択して、③続いて**Event Gridサブスクリプションの作成**を選択します。

    ![In the QueuePlateForManualCheckup Integration blade, the Create Event Grid subscription link is selected.](media/queueplateformanualcheckup-add-eg-sub.png "QueuePlateForManualCheckup blade")

2. **イベント サブスクリプションの作成**の画面で、以下の構成を指定します。

    - **名前**: `queueplateformanualcheckupsub`のような一意の名前を指定します（緑色のチェックマークが現れることを確認します）。
    - **イベントスキーマ**: **イベントグリッドスキーマ**を選択します。
    - **Topic Type**: **Event Grid Topics**を選択します。
    - **Subscription**: このハンズオンで利用しているサブスクリプションを選択します。
    - **Resource Group**: 既存リソースグループから**hands-on-lab-SUFFIX** リソースグループを選択します。
    - **Resource**: Event Grid Topicを選択します。`eventgridtopic-`で始まるサービス1個のみのはずです。
    - **イベントの種類**: **イベントの種類の追加**を選択して、新たなイベントタイプとして`queuePlateForManualCheckup` を入力します。これにより、このEventの種類でのみこの関数が呼び出されるようになります。
    - **エンドポイントのタイプ**: `Azure関数`のままにします。
    - **エンドポイント**: Leave as `QueuePlateForManualCheckup`のままにします。

    ![In the Create Event Subscription blade, fields are set to the previously defined values.](media/manualcheckup-eg-sub.png)

3. **作成**を選択し、トリガーの編集画面を閉じます。

### Task 6: Azure Cosmos DB出力バインドをQueuePlateForManualCheckup関数に追加する

このタスクでは、QueuePlateForManualCheckup関数に Azure Cosmos DB 出力バインディングを追加し、NeedsManualReviewコレクションへのデータ保存を可能にします。

1. **QueuePlateForManualCheckup**関数の統合画面で、**出力**の下の **+出力の追加** を選択します。

2. **出力の作成** 画面で、以下の設定をします。

   - **バインドの種類**として `Azure Cosmos DB` を選択
   - **Cosmos DB account connection**は先ほど作成した**Azure Cosmos DB account connection**を選択
   - **Document parameter name**: `outputDocument`のままにしておきます。
   - **Database name**: `LicensePlates`と入力します。
   - **Collection name**: `NeedsManualReview`と入力します。

3. **OK**を選択します。

    ![In the Azure Cosmos DB output form, the following field values display: Document parameter name, outputDocument; Collection name, NeedsManualReview; Database name, LicensePlates; Azure Cosmos DB account connection, cosmosdb-SUFFIX.](media/manual-checkup-cosmos-integration.png 'Azure Cosmos DB output form')

4. **QueuePlateForManualCheckup**関数を閉じます。

## 演習 3: Application Insightsで関数アプリを監視する

**所要時間**: 15分

Application Insights は、Azure Function Apps と統合することで、機能に対する強固な監視を提供することができます。この演習では、Function Apps のプロビジョニング時に作成した Application Insights アカウントのテレメトリーを確認します。Function Apps の作成時に Application Insights アカウントを関連付けたため、Application Insights テレメトリーキーが Function App の設定に追加されています。

### 参考情報

|                 |          |
| --------------- | -------- |
| **Description** | **Link** |
| Azure Functions を監視する | <https://docs.microsoft.com/azure/azure-functions/functions-monitoring> |
| Live Metrics: 1 秒の待機時間での監視と診断 | <https://docs.microsoft.com/azure/application-insights/app-insights-live-stream> |

### Task 1: ライブ メトリックを使ってリアルタイムに関数アプリを監視する

1. ハンズオンのリソースグループから **appinsights-SUFFIX**のApplication Insights リソースを開きます。

    ![The Application Insights instance is highlighted in the resource group.](media/resource-group-application-insights.png "Application Insights")

2. Application Insightsの画面で、左側のナビゲーションメニューの**調査**の下にある**ライブ メトリック**を選択します。

    ![In the TollBoothMonitor blade, in the pane under Investigate, Live Metrics Stream is selected. ](media/live-metrics-link.png 'TollBoothMonitor blade')

3. ライブ メトリックを開いたままで、LabVMのVisual Studioでスターターアプリソリューションに戻ります。

4. Visual Studioのソリューションエクスプローラで**UploadImages**プロジェクトに移動し、**UploadImages**プロジェクトを右クリックして**Properties**を選択します。

    ![In Solution Explorer, the UploadImages project is expanded, and Properties is selected from the right-click context menu.](media/vs-uploadimages.png 'Solution Explorer')

5. 左側のメニューの **Debug** を選択し、Azure Data Lake Storage Gen2アカウントの接続文字列を **Application arguments** テキストフィールドに貼り付けます。

   > **注意**: 接続文字列を取得する方法は以下からどうぞ。
   >
   > - Azure portalで、**datalake{SUFFIX}**ストレージアカウントに移動
   > - 左側のメニューから**アクセス キー**を選択
   > - **key1**の**接続文字列**の値をコピー
   >  

   この値を指定すると、アプリケーションを実行するたびに必要な接続文字列が引数として追加されるようになります。さらに、この値を追加して `.gitignore` ファイルをプロジェクトディレクトリに含めることで、後のステップでソースコードリポジトリに機密性の高い接続文字列が追加されるのを防ぐことができます。

    ![The Debug menu item and the command line arguments text field are highlighted.](media/vs-command-line-arguments.png "Properties - Debug")

6. Visual Studioのツールバーにある**Save**アイコンを選択して、変更を保存します。

7. Visual Studioのソリューションエクスプローラで**UploadImages**を右クリックし、**Debug**を選択後、コンテキストメニューから**Start New Instance**を選択します。

    ![In Solution Explorer, the UploadImages project is selected. From the context menu, Debug then Start New Instance is selected.](media/vs-debug-uploadimages.png 'Solution Explorer')

    >**注意**: ファイルが `C:\ServerlessMCW` の下に配置されていることを確認します。もしファイルが `C:\Users\workshop\Downloads\` のような長いルートパスの下にある場合、後のステップで `The specified path, file name, or both are too long. The fully qualified file name must be less than 260 characters, and the directory name must be less than 248 characters. (指定されたパス、ファイル名、またはその両方が長すぎます。指定されたパス、ファイル名、またはその両方が長すぎます。完全修飾ファイル名は260文字未満、ディレクトリ名は248文字未満でなければなりません)` というビルドの問題に遭遇します。

8. コンソールウィンドウが表示されたら、**1**と入力し、**ENTER**を押してください。この操作により、Blobストレージアカウントの画像コンテナにクルマの写真がアップロードされます。

    ![A Command prompt window displays, showing images being uploaded.](media/image69.png 'Command prompt window')

9. Application Insights内でライブメトリックを開いた状態のブラウザウィンドウに戻ります。オンラインのサーバーの個数、Request Rate、CPUプロセス量などを示す新しいテレメトリーが到着し始めるはずです。横のリストでサンプルのテレメトリをいくつか選択して、出力データを表示できます。

    ![The Live Metrics Stream window displays information for the two online servers. Displaying line and point graphs including incoming requests, outgoing requests, and overall health. To the side is a list of Sample Telemetry information. ](media/image70.png 'Live Metrics Stream window')

10. ライブメトリックスストリームウィンドウを再度開いたままにして、画像アップロードのコンソールウィンドウを閉じます。UploadImagesプロジェクトを再度Debugで開き、**2**を入力し、**ENTER**を押してください。これで、1,000枚の写真が新たにアップロードされます。

    ![The Command prompt window displays with image uploading information.](media/image71.png 'Command prompt window')

11. ライブメトリックの画面に戻り、写真がアップロードされる様子を観察してください。オンラインになっているサーバーの数、つまり両方の関数アプリで実行されている関数アプリインスタンスの数が表示されます。また、Request Rateモニターが一定の周期で推移し、Request Durationが "秒"未満で推移し、Incoming RequestsがOutgoing Requestsとほぼ同じであることに気づくはずです。

    ![In the Live Metrics Stream window, two servers are online under Incoming Requests. The Request Rate heartbeat line graph is selected, as is the Request Duration dot graph. Under Overall Health, the Process CPU heartbeat line graph is also selected, the similarities between this graph and the Request Rate graph under Incoming Requests are highlighted for comparison.](media/image72.png 'Live Metrics Stream window')

12. しばらくの間この操作を実行した後に、写真アップロードのコンソールウィンドウを再度閉じます。なお、ライブメトリックの画面はそのまま開いておきます。

### Task 2: リソース制約がある場合に関数アプリが動的にスケールする様子を観察する

このタスクでは、Computer Vision APIの価格レベルをFreeに変更します。これにより、OCR サービスへのリクエスト数は 1 分あたり 10 件に制限されます。変更後、UploadImagesコンソールアプリを実行し、1,000枚の画像を再度アップロードします。ProcessImage関数のFindLicensePlateText.MakeOCRRequestメソッドにプログラムされたresiliencyポリシーが、Computer Vision APIへのリクエストを指数関数的にバックオフし始め、その結果、レート制限が収まり影響を受けなくすることができるようになります。この意図的な遅延により、関数の応答時間が大幅に増加し、Consumptionプランの動的スケーリングが作動し、さらにいくつかのサーバーが割り当てられます。ライブメトリックビューを使用して、このすべてがリアルタイムで起こる様子を確認します。

1. **hands-on-lab-SUFFIX**リソースグループを開き、**computervision-**で始まるリソースを選択して、Computer Vision APIサービスを開きます。

    ![The computervision Computer vision resource is highlighted in the list of services in the resource group.](media/resource-group-computer-vision-resource.png "Resource group")

2. メニューの**リソース 管理**にある**価格レベル**を選択します。**F0 Free**価格レベルを選択し**適用**を選択します。

    > **注意**: すでに**F0 Free**価格レベルインスタンスを有している場合、別のものを作成できません。

    ![In the Cognitive Services blade, under Resource Management, the Pricing tier item is selected. In the Choose your pricing tier blade, the F0 Free option is selected.](media/image73.png 'Choose your pricing tier blade')

3. Visual Studioに切り替えて、**UploadImages**プロジェクトを再度Debugで開き、**2**を入力して**ENTER**を押します。これで1,000枚の新たな写真がアップロードされます。

    ![The Command prompt window displays image uploading information.](media/image71.png 'Command Prompt window')

4. ライブメトリックの画面に戻り、写真がアップロードされる様子を観察してください。数分間実行した後、いくつかのことに気がつくはずです。リクエストの処理時間は時間経過とともに増加し始めます。この現象が発生すると、オンラインになるサーバーの数が増えていることに気づくはずです。サーバーがオンラインになるたびに、Sample Telemetry に "Generating 2 job function(s)" というメッセージが表示され、その後に Starting Host というメッセージが表示されるはずです。また、Computer Vision API サーバーがリクエストを制限していることを示す、resiliencyポリシーが記録するメッセージも表示されるはずです。これは、サービスから送り返されるレスポンスコード（429）でわかります。メッセージのサンプルは、"Computer Vision API server is throttling our requests. Automatically delaying for 16000ms" です。

    > **注意**: サンプルテレメトリを選択してその詳細を確認できない場合、リストの下にあるサイズ変更バーを動かして、詳細ペインのサイズを変更してください。

    ![In the Live Metrics Stream window, 11 servers are now online.](media/image74.png 'Live Metrics Stream window')

5. しばらく実行した後、UploadImagesコンソールを閉じて、写真のアップロードを停止します。

6. Azure Portalの **Computer Vision** リソースに戻り、価格階層を **S1 Standard** に設定します。

## 演習 4: Azure Cosmos DBでデータを調査する

**所要時間**: 15分

この演習では、ポータルのAzure Cosmos DBデータエクスプローラーを使用して、保存されたナンバープレートデータを表示します。

> **注意**: Azure Cosmos DB アカウントの **ファイアウォール設定** の IP リストに自分の IP アドレスが追加されていることを確認します。そうでない場合、Azure Cosmos DB 内の License Plates データは表示されません。この作業は、ハンズオンガイドの Before the hands で完了しているはずです。

### 参考情報

|                       |                                                           |
| --------------------- | :-------------------------------------------------------: |
| **Description**       |                         **Links**                         |
| Azure Cosmos DB の概要 | <https://docs.microsoft.com/azure/cosmos-db/introduction> |

### Task 1: Azure Cosmos DB データエクスプローラーを使用する

1. [Azure portal](https://portal.azure.com)で、 **hands-on-lab-SUFFIX**リソースグループに移動します。

   > Azure Portalホームページの**Azureサービス**の下にある**リソースグループ**を選択し、リストからリソースグループを選択すると、リソースグループに移動できます。Azureアカウントに多くのリソースグループがある場合、**hands-on-lab**のリストをフィルタリングして、リストされるリソースグループをフィルタリングできます。

2. リソースグループの画面で、リソースグループ内で利用可能なサービス一覧から **cosmosdb** Azure Cosmos DB アカウントのリソースを選択します。

   ![The Azure Cosmos DB account resource is highlighted in the list of services in the resource group.](media/resource-group-cosmos-db-account.png "Resources")

3. Cosmos DBの画面で、左側のナビゲーションメニューから**データ エクスプローラー**を選択します。

    ![In the Data Explorer blade, Data Explorer is selected from the left menu.](media/data-explorer-link.png 'Data Explorer')

4. **LicensePlates** データベースを展開し、次に **Processed** コレクションを展開し、**Items** を選択します。これにより、コレクションに追加された各JSONドキュメントが一覧表示されます。

5. ドキュメントを1つ選択し、その内容を表示します。作成した関数アプリが、最初の4つのプロパティを追加しました。残りのプロパティは標準的なもので、Cosmos DBが割り当てています。

    ![In the tree-view beneath the LicensePlates Cosmos DB, the Processed collection is expanded with the Items item selected. On the Items tab, a document is selected, and its JSON data is displayed. The first four properties of the document (fileName, licensePlateText, timeStamp, and exported) are displayed along with the standard Cosmos DB properties.](media/data-explorer-processed.png 'Data Explorer')

6. 次に**NeedsManualReview**コレクションを展開し、**Items**を選択します。

7. ドキュメントを1つ選択し、内容を表示します。ファイル名と、"resolved"という名前のプロパティが追加されていることに着目してください。このハンズオンの範囲外ですが、これらのプロパティを使用して、写真のレビューとナンバープレートの入力のための手動プロセスを提供できます。

    ![In the tree-view beneath the LicensePlates Cosmos DB, the NeedsManualReview collection is expanded, and the Items item is selected. On the Items tab, a document is selected, and its JSON data is displayed. The first four properties of the document (fileName, licensePlateText, timeStamp, and resolved) are shown along with the standard Cosmos DB properties.](media/data-explorer-needsreview.png 'Data Explorer')

8. **Processed**コレクションの横にある楕円 ... を選択し、**New SQL Query**を選択します。

    ![In the tree-view beneath the LicensePlates Cosmos DB, the Processed collection is selected. From its right-click context menu, New SQL Query is selected.](media/data-explorer-new-sql-query.png 'Data Explorer')

9. 以下のSQLクエリをクエリウィンドウに貼り付けます。このクエリは、エクスポートされていない処理済み文書の件数をカウントします。

    ```sql
    SELECT VALUE COUNT(1) FROM c WHERE c.exported = false
    ```

10. このクエリを実行し、結果を観察してみましょう。この例では、エクスポート対象の処理済み文書が669件あります。

    ![In the Query window, the previously defined SQL query displays. Under Results, the number 669 is highlighted.](media/cosmos-query-results.png 'Query 1 tab')

## 演習 5: データエクスポートワークフローを作成する

**所要時間**: 30分

この演習では、データエクスポートワークフローのための新しいロジックアプリを作成します。このロジックアプリは定期的に実行されるもので、ExportLicensePlates関数アプリを呼び出し、エクスポートするレコードがない場合、条件付きで電子メールを送信します。

### 参考情報

|                 |           |
| --------------- |---------- |
| **Description** | **Links** |
| Azure Logic Apps とは | <https://docs.microsoft.com/azure/logic-apps/logic-apps-overview> |
| Azure Functions を使用してコードを作成し、Azure Logic Apps のワークフローから呼び出す | <https://docs.microsoft.com/azure/logic-apps/logic-apps-azure-functions> |

### Task 1: ロジックアプリを作成する

1. [Azure portal](https://portal.azure.com)で、 **hands-on-lab-SUFFIX**リソースグループに移動します。

   > Azure Portalホームページの**Azureサービス**の下にある**リソースグループ**を選択し、リストからリソースグループを選択すると、リソースグループに移動できます。Azureアカウントに多くのリソースグループがある場合、**hands-on-lab**のリストをフィルタリングして、リストされるリソースグループをフィルタリングできます。

2. リソースグループの画面で、リソースグループ内で利用可能なサービス一覧から **logicapp** Logic Appリソースを選択します。

3. **Logic Apps デザイナー**で、_一般的なトリガーで開始する_を見つけるまでページをスクロールし、**繰り返し**トリガーを選択します。

    ![The Recurrence tile is selected in the Logic App Designer.](media/logic-app-designer.png 'Logic App Designer')

4. **間隔**に **15** と入力し、**頻度**が **分** に設定されていることを確認します。これは、ビジネス要件に応じて、1時間または他の間隔に設定することができます。

5. **+新しいステップ**を選択します。

    ![Under Recurrence, the Interval field is set to 15, and the + New step button is selected.](media/image83.png 'Logic App Designer Recurrence section')

6. 検索フィールドに`関数`と入力し、**Azure Functions**コネクターを選択します。

    ![Under Choose an action, Functions is typed in the search box. Under Connectors, Azure Functions is selected.](media/logic-app-choose-operation-functions.png 'Logic App Designer Choose an action section')

7. **TollBoothFunctions**関数アプリを選択します。

    ![Under Azure Functions, in the search results list, Azure Functions (TollBoothFunctions) is selected.](media/logic-app-function-app-action.png 'Logic App Designer Azure Functions section')

8. **ExportLicensePlates**関数をリストから選択します。

    ![Under Azure Functions, under Actions (2), Azure Functions (ExportLicensePlates) is selected.](media/logic-app-select-export-function.png 'Logic App Designer Azure Functions section')

    > This function does not require any parameters that need to be sent when it gets called.

9. **+新しいステップ**を選択し、 `条件`でコネクターを選択します。アクションの検索結果から、**条件**制御を選択します。

    ![In the logic app designer, in the ExportLicensePlates section, the parameter field is left blank. In the Choose an action box, condition is entered as the search term, and the Condition Control item is selected from the Actions list.](media/logicapp-add-condition.png 'Logic App Designer ExportLicensePlates section')

10. **値の選択**フィールドで、**状態コード**パラメーターを選択します。オペレーターが**次の値に等しい**であることを確認した上で、2番目の値の選択フィールドで、**200**を指定します。

    > **Note**: ExportLicensePlates関数から返されたステータスコードを評価します。これは、ナンバープレートが見つかり、エクスポートされた場合に200コードを返します。そうでない場合、つまりエクスポート対象のナンバープレートが発見されなかったときに、204（NoContent）ステータスコードを送信します。200以外のレスポンスが返された場合は、条件付きで電子メールを送信するようにします。

    ![The first Condition field displays Status code. The second, drop-down menu field displays is equal to, and the third field is set to 200.](media/logicapp-condition.png 'Condition fields')

11. ナンバープレートのエクスポートに成功した場合はアクションを実行する必要がないため、**If true**条件を無視します。**If false** 条件ブロック内で **アクションの追加** を選択します。

    ![Under the Conditions field is an If true (green checkmark) section and an if false (red x) section. In the If false section, the Add an action button is selected.](media/logicapp-condition-false-add.png 'Logic App Designer Condition fields if true/false ')

12. `メールの送信 (V2)` を検索フィールドに入力し、Office 365 Outlookの**メールの送信 (V2)**を選択します。

    ![In the Choose an action box, send an email is entered as the search term. From the Actions list, Office 365 Outlook (end an email (V2) item is selected.](media/logicapp-send-email.png 'Office 365 Outlook Actions list')

13. **サインイン**を選択し、ご自身のOffice 365 Outlookアカウントでサインインします。

    ![In the Office 365 Outlook - Send an email box, the Sign in button is selected.](media/image93.png 'Office 365 Outlook Sign in prompt')

14. メールの送信フォームで、以下の値を指定します。

     - **宛先** : ご自身のメールアドレスを入力
     - **件名** : `Toll Booth license plate data export check` (料金所ナンバープレートデータのエクスポート) のような件名を入力 (日本語可)
     - **本文** : ExportLicensePlates関数から**状態コード**を選択してメール本文に追加

     >**注意**: ステータスコード**204**のメールを受信した場合、すべてのデータが処理されており、エラー状態ではありません。ステータスコード**200**を出すには、処理を遅くしてキューイングされた状態を作成する必要があります。

     ![In the Send an email box, fields are set to the previously defined values.](media/logicapp-send-email-form.png 'Logic App Designer, Send an email fields')

15. ツールバーの **保存** を選択し、Logic App を保存します。

16. **トリガーの実行**を選択して、ロジックアプリを実行します。ナンバープレートデータがエクスポートされないため、電子メールアラートを受信し始めるはずです。これは、Azure Cosmos DB からナンバープレートデータを抽出し、CSV ファイルを生成して Blob ストレージにアップロードするよう、ExportLicensePlates 関数の変更を完了する必要があるためです。

    ![The Run button is selected on the Logic Apps Designer blade toolbar.](media/logicapp-start.png 'Logic Apps Designer blade')

17. ロジック アプリ デザイナーでは、ワークフローの各ステップの実行結果が表示されます。正常に実行された各ステップの横に緑色のチェックマークが付き、完了までの実行時間が表示されます。これを使って、各ステップがどのように動作しているかを確認でき、実行されたステップを選択して出力のRAWデータを確認できます。

    ![In the Logic App Designer, green check marks display next to Recurrence, ExportLicensePlates, Condition, and Send an email steps of the logic app.](media/image96.png 'Logic App Designer ')

18. ロジックアプリは、無効にするまでバックグラウンドで実行され続け、15分ごと（または設定した間隔ごと）に実行されます。アプリを無効にするには、Logic アプリの **概要**に移動し、タスクバーの **無効** ボタンを選択します。

    ![The Disable button is selected on the TollBoothLogic Logic app blade toolbar menu.](media/image97.png 'TollBoothLogic blade')

## 演習 6: 関数アプリの継続的デプロイを構成する

**所要時間**: 40分

この演習では、ProcessImage 関数を含む関数アプリを継続的にデプロイできるように設定します。まず、GitHub のソースコードリポジトリを作成し、Function App のデプロイ元として設定します。

### 参考情報

|                 |           |
| --------------- | ----------|
| **Description** | **Links** |
| 新しいリポジトリの作成 | <https://help.github.com/articles/creating-a-new-repository/> |
| Azure Functions の継続的なデプロイ | <https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment> |

### Task 1: gitリポジトリにVisual Studioのソリューションを追加し、GitHubに展開する

1. LabVMに戻り、Visual Studioで**Git**メニュー項目を選択し、**Settings**を選択します。

2. **Options** ダイアログで、Source Control の **Git Global Settings** タブが表示されていることを確認し、`User name` と `Email` フィールドに GitHub ユーザー名とメールアドレスを入力し、**OK** を選択します。

    ![The Visual Studio Git Global Settings page is displayed with the user name and email fields highlighted.](media/git-global-settings.png "Git Global Settings")

3. 次に、デフォルトブランチを `main` に設定します。 メニューの **View** を選択し、**Terminal** を選択します。

4. ターミナルで、以下のコマンドを実行します。これは新しいGitリポジトリのデフォルトブランチをすべて`main`に設定するものです。

    ```bash
    git config --global init.defaultbranch main
    ```

5. 次に、Solution Explorerのタブの横にある **Git Changes** タブを選択します。

    ![The Git Changes tab is highlighted below the Solution Explorer pane in Visual Studio.](media/git-changes-tab.png "Git Changes")

6. Git Changesパネルで、**Create Git Repository...**を選択します。

    ![In Solution Explorer, TollBooth solution is selected. From its right-click context menu, the Create Git Repository item is selected.](media/vs-create-git-repo.png 'Solution Explorer')

7. `Create a new GitHub repository`の下にある、Accountの隣の**Sign in...**を選択し、GitHubアカウントを選択します。

    ![The sign in button is highlighted.](media/vs-create-git-repo-sign-in.png "Create a Git repository")

8. 表示されたウェブページで、**Authorize github** を選択して、Visual Studio にて GitHub アカウントで作業するための追加権限を付与します。

    > **注意**: Microsoft Edge を LabVM のデフォルトブラウザにしなかった場合、Authorize github ボタンは無効になります。Windows の検索バーに **Default apps** と入力し、デフォルトの Web ブラウザを Microsoft Edge に変更する必要があります。

    ![The authorize github button is highlighted.](media/vs-create-git-repo-allow.png "Allow additional permissions")

9. GitHubのアカウントにサインインします。しばらくすると、認証が成功したことを示す Success ページが表示されます。これが表示されたら、Visual Studioに戻りましょう。

    ![The success page is displayed.](media/vs-github-auth-successful.png "Your authorization was successful")

10. Git リポジトリの作成ダイアログで、**Local path** フィールドの横にあるbrowseボタンを選択して、ディレクトリを変更します。

    ![The browse button is highlighted next to the local path for the GitHub repo.](media/create-a-git-repo-browse.png "Change local path")

11. Browseダイアログで、`TollBooth`フォルダーの `TollBooth` フォルダーを選択します。これで、ソース管理に追加する **TollBooth** プロジェクトのみが選択され、 **UploadImages** プロジェクトを除外できます。

    ![In the Browse dialog, the TollBooth/TollBooth folder is highlighted.](media/browse-tollbooth.png "Browse")

12. 以下の情報をフォームに入力します。

    - **Repository Name**: `serverless-architecture-lab`
    - **Private**: チェックしないでおきます (Public repoとして作成)

    ![The form is completed as described.](media/vs-create-git-repo-create.png "Create a Git repository")

13. **Create and Push**を選択します。

14. ブラウザでGitHubのリポジトリページを更新します。プロジェクトファイルが追加されていることを確認できるはずです。リポジトリ内の **TollBooth** フォルダに移動し、local.settings.jsonファイルがアップロードされていないことに着目してください。これは、TollBoothプロジェクトの.gitignoreファイルが、そのファイルをリポジトリから明示的に除外しているためで、誤ってアプリケーションの秘密を共有することがないようにしています。

    ![On the GitHub Repository webpage for serverless-architecture-lab, on the Code tab, the project files are displayed.](media/github-repo-page.png 'GitHub Repository page')

### Task 2: 関数アプリがGitHubリポジトリを使用して継続的にデプロイされるように設定する

1. [Azure Portal](https://portal.azure.com)で、**hands-on-lab-SUFFIX**のリソースグループに移動します。

   > Azure Portalホームページの**Azureサービス**の下にある**リソースグループ**を選択し、リストからリソースグループを選択すると、リソースグループに移動できます。Azureアカウントに多くのリソースグループがある場合、**hands-on-lab**のリストをフィルタリングして、リストされるリソースグループをフィルタリングできます。

2. リソースグループの画面で、リソースグループ内で利用可能なサービス一覧から **TollBoothFunctions** 関数アプリリソースを選択します。

3. 左側のナビゲーションメニューで、**デプロイメント**の下にある**デプロイ センター**を選択します。

    ![The Platform features tab is displayed, under Code Deployment, Container settings is selected.](media/functionapp-menu-deployment-center-link.png 'TollBoothFunctions blade')

4. Select the **ソース**ドロップダウンリストを選択して、リストから **GitHub** を選択します、

    ![GitHub is highlighted in the select code source drop-down list.](media/deployment-center-select-code-source.png "Select code source")

5. **認証**を選択して、ご自身のGitHubの資格情報を入力します。

    ![The Authorize button is highlighted under GitHub in the Deployment Center.](media/deployment-center-github-authorize.png "GitHub Authorize")

6. Authorize Azure App Serviceページで、 **Authorize AzureAppService**を選択します（パスワード入力が求められる場合には、パスワードを入力します）。

    ![The Authorize Azure App Service button is highlighted.](media/authorize-azure-app-service.png "Authorize Azure App Service")

7. アカウントの認証後、GitHubリポジトリに接続するための以下の情報を構成します。

    - **組織**: リポジトリを作成したGitHubアカウント組織を指定
    - **リポジトリ**: **serverless-architecture-lab**もしくはリポジトリ名として指定した名前を選択
    - **ブランチ**: **main**を選択

    ![The GitHub settings specified above are entered into the Settings dialog.](media/deployment-center-github-settings.png "GitHub settings")

    > **注意**: 現時点では、Build settingsが編集不可能で、.NETバージョン4.0に設定されているという問題があります。今後のステップで、これを適切なフレームワークのバージョンに変更していきます。

8. 一番上のツールバーの**保存**を選択します。

9. Web ブラウザで GitHub ウェブサイトの **serverless-architecture-lab** リポジトリに戻ります。トップメニューから、**Actions** を選択します。

    ![The serverless-architecture-lab repository displays with the Actions menu item highlighted.](media/githubrepo_actionsmenu.png "GitHub repository screen")

10. **All workflows** の下にある **Build and deploy dotnet core app to Azure Function App - TollBoothFunctions-{SUFFIX}** を選択します。タイトルの直下にある **main_TollBoothFunctions-{SUFFIX}** リンクを選択します。

    ![The Build and deploy workflow screen is shown with the yml file link highlighted beneath the workflow title.](media/buildanddeployworkflow_landingpage.png "Workflow landing page")

    > **注意**: 最初のワークフローが失敗するのは想定されていることです。適切でないフレームワークがYAMLドキュメントで指定されていることが原因で発生しているためです。

11. YAMLファイルの画面で、鉛筆アイコンを選択し、ドキュメントをインラインで編集します。

    ![The YML file screen displays with the pencil icon highlighted.](media/edit_yml_file_menu_main.png "YML file")

12. 14行目で、**DOTNET_VERSION**の値を **'3.1.x'** に変更します。ファイルの構造は変えず、**値**だけを変更してください。変更後、**Start commit**を選択します。

    ![The YML file screen displays an editor with the DOTNET_VERSION value changed to 3.1.x, the Start commit button is highlighted.](media/yml_edit_dotnetversion_main.png "Editing a YML file")

13. **Commit changes**ダイアログで、**Changed .NET version**のコメントを入れて、**Commit changes**を選択します。

    ![The Commit changes dialog displays with the Changed .NET version comment and the Commit changes button highlighted.](media/yml_commit_changes_main.png "Commit changes dialog")

14. YAMLファイルの更新をコミットすると、新しいデプロイメントが成功します。現在実行中のワークフローや過去のワークフローの状況は、リポジトリのActionsタブで確認できます。

    ![A successful deployment workflow displays.](media/successful_workflow_execution_main.png "Successful workflow run")

### Task 3: ExportLicensePlates 関数のコードを完成させ、変更を GitHub にプッシュしてデプロイを開始する

1. LabVMに戻り、Visual Studioのソリューションエクスプローラーを使用して、**TollBooth**プロジェクトに移動します。

2. Visual Studioの**View**メニューから、**Task List**を選択します。

    ![Task List is selected from the Visual Studio View menu.](media/vs-task-list.png 'Visual Studio View menu')

3. Visual Studioウィンドウの下部にある**Task List**ペインで、`TODO 5`をダブルクリックすると、関連する`TODO`タスクが表示されます。

    ![TODO 5 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-5.png "Task List")

4. 開いた **DatabaseMethods.cs** ファイルで、`TODO 5` コメントの下の行のコードを、以下のコードで更新してください。

    ```csharp
    // TODO 5: Retrieve a List of LicensePlateDataDocument objects from the collectionLink where the exported value is false.
    licensePlates = _client.CreateDocumentQuery<LicensePlateDataDocument>(collectionLink,
            new FeedOptions() { EnableCrossPartitionQuery=true,MaxItemCount = 100 })
        .Where(l => l.exported == false)
        .ToList();
    ```

5. 続いて`TODO`リストに戻り、`TODO 6`をダブルクリックします。

    ![TODO 6 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-6.png "Task List")

6. これは、先ほど更新した `TODO 5` のコードのすぐ下にあります。`//TODO 6`のコメントの下にあるコードの行を削除してください。

    ```csharp
    // TODO 6: Remove the line below.
    ```

7. `TODO 6`の下にある以下の行を削除したことを確認してください。 

    ```csharp
    licensePlates = new List<LicensePlateDataDocument>();
    ```

8. 変更を **DatabaseMethods.cs** ファイルに保存します。

9. `TODO`リストに戻って、`TODO 7`をダブルクリックします。

    ![TODO 7 is highlighted in the Visual Studio Task List.](media/visual-studio-task-list-todo-7.png "Task List")

10. 開いた **FileMethods.cs** ファイルで、`TODO 7` のコメントの下の行のコードを、以下のコードで更新してください。

    ```csharp
    // TODO 7: Asynchronously upload the blob from the memory stream.
    await blob.UploadFromStreamAsync(stream);
    ```

11. 変更を **FileMethods.cs** ファイルに保存します。

12. Visual Studio で **Git** メニューを選択し、**Commit or Stash...** を選択します。

    ![The Git menu is displayed.](media/vs-commit-or-stash.png "Commit or Stash")

13. コミットメッセージを入力し、**Commit All**を選択します。

    ![In the Team Explorer - Changes window, "Finished the ExportLicensePlates function" displays in the message box, and the Commit All button is selected.](media/vs-git-commit-all-main.png 'Team Explorer - Changes window')

14. コミット後、**Push**ボタンを選択し、GitHub リポジトリに変更内容をプッシュします。

    ![The Push button is highlighted.](media/vs-git-push-main.png "Push changes")

    > **注意**: ローカルブランチがリモートブランチより遅れているというメッセージが表示されることがあります。
    > ![Git - Push failed dialog appears.](media/git-push-failed.png)
    >
    > このメッセージが表示された場合は、**Pull then Push** を選択してリポジトリを同期し、その後変更をコミットしてください。

15. GitHub リポジトリへのプッシュが成功した旨のメッセージが表示されるはずです。

    ![The message is displayed.](media/vs-git-push-success-main.png "Successfully pushed")

16. Azure Portalの関数アプリのデプロイ センターに戻ります。この最後のコミットによって動作したデプロイのエントリが表示されるはずです。メッセージのタイムスタンプを確認し、最新のデプロイをチェックしていることを確認します。**デプロイメントが完了したことを確認してから続けてください**。

    ![The latest deployment is displayed in the Deployment Center.](media/functionapp-dc-latest.png 'Deployment Center')

## 演習 7: ワークフローを再実行し、データエクスポートを確認する

**所要時間**: 10分

最新のコード変更を行った上でロジックアプリを実行し、ファイルが正常にエクスポートされることを確認します。

### Task 1: 画像のアップロードを再実行する

1. Visual StudioのSolution Explorerで、**UploadImages**プロジェクトを右クリックし、**Debug**を選択後、コンテキストメニューから **Start New Instance** を選択します。

2. コンソールウィンドウが表示されたら、`2`を入力し、**ENTER**を押します。このアクションは、車の写真をBlobストレージアカウントのimagesコンテナにアップロードします。これにより、ExportLicensePlates関数をトリガーするためのデータが得られるはずです。

### Task 2: ロジックアプリを実行する

1. Azure Portalで**hands-on-lab-SUFFIX**リソースグループを開き、リストから**logicapp**で始まるロジックアプリのリソースを選択します。

2. (以前このロジックアプリを無効化していた場合は)**概要**画面で、**有効**を選択します。

    ![In the TollBoothLogic Logic app blade, Overview is selected in the left menu, and the Enable enable button is selected in the right pane.](media/image113.png 'TollBoothLogic blade')

3. Now select **トリガーの実行**を選択して、**実行**を選択し、ワークフローを即時起動します。

    ![In the TollBoothLogic Logic app blade, Run Trigger and Run are selected.](media/image114.png 'TollBoothLogic blade')

4. トリガーの実行の隣にある**最新の情報に更新**ボタンを選択し、実行履歴を更新します。最新の実行履歴を選択します。状態の表示結果が **成功**であれば、CSVファイルがデータレイクストレージにエクスポートされているはずです。ロジックアプリを無効化するのをお忘れ無く。そうでないと、15分ごとにメールが届いてしまいます。場合によっては、起動に予想以上の時間がかかることがありますので、ご注意ください。

    ![In Logic App Designer, in the Condition section, under Inputs, true is highlighted.](media/image115.png 'Logic App Designer ')

### Task 3: エクスポートされたCSVファイルを見る

1. Azure Portalで**hands-on-lab-SUFFIX**リソースグループを開き、アップロードされた写真とエクスポートされたCSVファイルを保存するためにプロビジョニングした、**datalake**で始まるストレージアカウントリソースを選択します。

2. ストレージアカウントの左側のメニューから、**コンテナー**を選択し、その中の **export** コンテナーを選択します。

    ![The Containers option is selected on the left menu and the export container is highlighted from the container listing.](media/storage-containers.png 'Storage container listing')

3. 少なくとも一つ、直近でアップロードされたCSVファイルを確認できるはずです。ファイル名を選択して、そのファイルのプロパティを確認します。

    ![In the Export blade, under Name, a .csv file is selected.](media/blob-export.png 'Export blade')

4. Blobプロパティの画面で、**ダウンロード**を選択します。

    ![In the Blob properties blade, the Download button is selected.](media/blob-download.png 'Blob properties blade')

5. CSVファイルは以下のような感じになっているはずです。

    ![A CSV file displays with the following columns: FileName, LicensePlateText, TimeStamp, and LicensePlateFound.](media/csv.png 'CSV file')

6. ExportLicensePlates 関数は、exportedの値をtrueに設定することで、エクスポートしたすべてのレコードを更新します。これにより、前回のエクスポート以降の新しいレコードのみが、次回のエクスポートに含まれるようになります。Azure Cosmos DB で、Processed コレクション内の exported が false のドキュメント数をカウントするスクリプトを再実行することで、これを確認します。その後、新しい写真をアップロードしていない限り、0が返されるはずです。

## ハンズオン終了後

**所要時間**: 10分

この演習では、演習で作成したAzureリソースをすべて削除します。

### Task 1: Azureリソースを配置したリソースグループを削除する

1. Azure Portalで**hands-on-lab-SUFFIX**リソース グループに移動し、上部にあるツールバーで **リソースグループの削除**を選択します。

2. 削除の確認のため、**リソースグループ名**を入力し､**削除**を選択します。このとき、**選択した仮想マシンと仮想マシン スケール セットに対して強制削除を適用する**にチェックを入れておいてください。

3. 仮想マシンに別のリソースグループを作成していた場合、同様にそのリソースグループも削除してください。この場合も、**選択した仮想マシンと仮想マシン スケール セットに対して強制削除を適用する**にチェックを入れておいてください。

### Task 2: GitHubリポジトリを削除する

[Optional] このタスクでは、このハンズオンのために作成したGitHubリポジトリを削除します。

1. <https://www.github.com>を開き、ご自身のProfileアイコンを選択して、**Your repositories**を選択します。

2. 削除対象のリポジトリを選択します。

3. **Settings**タブを選択し、スクロールダウンして、**Delete this repository**を選択します。

ハンズオン終了後、この手順をすべて実施されることを推奨します。