![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
サーバーレス・アーキテクチャ
</div>

<div class="MCWHeader2">
手作業によるリソースデプロイ・セットアップガイド
</div>

<div class="MCWHeader3">
2021年11月
</div>

本書に記載されている情報は、URLやその他のインターネット上のウェブサイトを含め、予告なく変更されることがあります。特に断りのない限り、ここに記載されている会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントの例は架空のものであり、実在の会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントとの関連を意図したり推論したりするものではありません。適用されるすべての著作権法を遵守することは、ユーザーの責任です。著作権に基づく権利を制限することなく、マイクロソフトの書面による明示的な許可がない限り、本書のいかなる部分も、いかなる形式または手段 (電子的、機械的、複写、記録、その他) で、あるいはいかなる目的においても、複製、検索システムへの保存または導入、転送することを禁じます。

マイクロソフトは、本書の主題を対象とする特許、特許出願、商標、著作権、またはその他の知的財産権を有している場合があります。マイクロソフトの書面によるライセンス契約において明示的に規定されている場合を除き、本書の提供は、これらの特許、商標、著作権、またはその他の知的財産に対するいかなるライセンスもお客様に与えるものではありません。

メーカー名、製品名、URL は情報提供のみを目的としており、マイクロソフトは、これらのメーカーまたはマイクロソフトのテクノロジーによる製品の使用に関して、明示的、黙示的、法定的を問わず、いかなる表明および保証も行いません。メーカーや製品を掲載することは、マイクロソフトがそのメーカーや製品を推奨していることを意味するものではありません。また、第三者のサイトへのリンクが張られていることがあります。そのようなサイトはマイクロソフトの管理下にはなく、マイクロソフトは、リンク先のサイトのコンテンツ、リンク先のサイトに含まれるリンク、またはそのようなサイトの変更もしくは更新について、一切の責任を負いません。マイクロソフトは、リンク先サイトから受信するウェブキャスティングまたはその他の形式の伝送について責任を負いません。マイクロソフトは、これらのリンクをお客様の便宜のためにのみ提供しており、リンクを掲載することは、マイクロソフトが当該サイトまたはそこに含まれる製品を推奨していることを意味するものではありません。

© 2021 Microsoft Corporation. All rights reserved.

マイクロソフトと <https://www.microsoft.com/legal/intellectualproperty/Trademarks/Usage/General.aspx> に記載の商標は、マイクロソフトグループの商標です。その他の商標は各所有者に帰属します。

<!-- TOC -->
**Contents**:

- [手作業によるリソースデプロイ・セットアップガイド](#手作業によるリソースデプロイ・セットアップガイド)
  - [必要なもの](#必要なもの)
  - [Task 1: Azure Data Lake Storage Gen2 アカウントのプロビジョニング](#task-1-Azure-Data-Lake-Storage-Gen2-アカウントのプロビジョニング)
  - [Task 2: 料金所Function Appのプロビジョニング](#task-2-料金所Function-Appのプロビジョニング)
  - [Task 3: Provision the Events Function App](#task-3-provision-the-events-function-app)
  - [Task 4: Provision an Event Grid topic](#task-4-provision-an-event-grid-topic)
  - [Task 5: Provision and configure an Azure Cosmos DB account](#task-5-provision-and-configure-an-azure-cosmos-db-account)
  - [Task 6: Provision the Computer Vision API service](#task-6-provision-the-computer-vision-api-service)
  - [Task 7: Create a Logic App](#task-7-create-a-logic-app)
  - [Task 8: Provision Azure Key Vault](#task-8-provision-azure-key-vault)
  - [Task 9: Retrieve the URI for each secret](#task-9-retrieve-the-uri-for-each-secret)
  - [Task 10: Configure application settings for the TollBoothFunctions Function App](#task-10-configure-application-settings-for-the-tollboothfunctions-function-app)
  - [Task 11: Set up a development virtual machine](#task-11-set-up-a-development-virtual-machine)
  - [Task 12: Connect to the Lab VM](#task-12-connect-to-the-lab-vm)
  - [Task 13: Disable Internet Explorer Enhanced Security](#task-13-disable-internet-explorer-enhanced-security)
  - [Task 14: Install required software on the LabVM](#task-14-install-required-software-on-the-labvm)

<!-- /TOC -->

# 手作業によるリソースデプロイ・セットアップガイド (約30分)

このガイドでは、before the hands-on labガイドのタスク 2 (ARMテンプレートを使ったハンズオンリソースのプロビジョニング) を手動で実行する手順を説明します。以下のステップバイステップの手順で、ARM テンプレートが作成するソースを手動でプロビジョニングおよび設定していきます。

クリーンアップを容易にするために、すべてのリソースで同じリソース グループを使用していることを確認します。

> **重要**: 多くのAzureリソースでは、グローバルで一意な名前を必要とします。これらの手順全体で、"SUFFIX"という文字がリソース名の中に出てきますが、このSUFFIXを使い、一意な名前のリソースになるようにしてください（例：社員番号、エイリアスなど）。

## 必要なもの

- Microsoft Azureのサブスクリプション
- ローカルマシンもしくは構成済みの仮想マシン (**ハンズオン当日までに完了しておいてください!**):
  - Visual Studio Community 2019 もしくはそれ以後のVisual Studio
    - <https://www.visualstudio.com/vs/>
  - Azure development workload for Visual Studio 2019
    - <https://docs.microsoft.com/azure/azure-functions/functions-develop-vs#prerequisites>
  - .NET Core 3.1
    - <https://www.microsoft.com/net/download/windows>
- Office 365のアカウント。お持ちでないなら、 [トライアル環境のサインアップ](https://portal.office.com/Signup/MainSignup15.aspx?Dap=False&QuoteId=79a957e9-ad59-4d82-b787-a46955934171&ali=1) が可能です。
- GitHubのアカウント。 [無料のアカウントを作成](https://github.com/join) できます。

## Task 1: Azure Data Lake Storage Gen2 アカウントのプロビジョニング

1. In the [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. マーケットプレースの検索フィールドに"ストレージ アカウント"と入力して検索し、その結果から **ストレージ アカウント** を選択します。その後、**作成**を選択します。

   !["Storage account" is entered into the Search the Marketplace box. Storage account is selected in the results.](media/create-resource-storage-account.png "Create Storage account")

3. ストレージ アカウント作成の**基本**タブで、以下を入力します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **ストレージ アカウント名**: datalakeSUFFIXを入力します。
   - **地域**: ハンズオンで利用するリージョンを選択します。
   - **パフォーマンス**: **Standard**（汎用v2アカウント）を選択します。
   - **冗長性**: **ローカル冗長ストレージ (LRS)**を選択します。

   ![On the Create storage account blade, the values specified above are entered into the appropriate fields.](media/storage-create-account-basics.png "Create storage account")

4. 続いて、 **詳細設定** タブに移動します。

   ![The Advanced tab is highlighted in the tabs on the Create storage account blade.](media/storage-create-account-tabs.png "Create storage account")

5. 詳細設定タブでは、Data Lake Storage Gen2の下にある**階層型名前空間**を**有効**にします。

   ![The Advanced tab of the Create storage account blade is highlighted and Hierarchical namespace and its enabled setting are selected and highlighted.](media/storage-create-account-advanced.png "Create storage account")

6. **確認および作成**を選択します。

7. **確認および作成**の画面では、検証が完了したメッセージが表示されたことを確認してから、**作成**を選択します。

8. ストレージ アカウントのプロビジョニングが完了してから、**リソースへ移動**を選択してストレージ アカウントを開きます。

    ![In the Azure Portal, once the storage account has completed provisioning, a status message is displayed saying Your deployment is complete. Beneath the next steps section, The Go to resource button is highlighted.](media/storage-go-to-resource.png "Go to resource")

9. **ストレージ アカウント**の画面で、左側のナビゲーションメニューにある、**データストレージ**の下の**コンテナー**を選択し、**+ コンテナー**ボタンをクリックして新しいコンテナーを作成します。

    ![The Containers menu item is selected and highlighted in the Storage account blade's left-hand menu, and + Container is highlighted on the Containers blade.](media/data-lake-containers.png "Containers")

10. 新しいコンテナーの画面で、**名前**のフィールドに**images**と入力し、パブリックアクセスレベルとして **プライベート（匿名アクセスはありません）**を選択します。それから**作成**をクリックしてコンテナーを保存します。

    ![In the New container dialog, images is entered into the Name field and highlighted. Private (no anonymous access) is selected for the public access level. The **Create** button is highlighted.](media/data-lake-new-container-images.png 'Containers blade')

11. 手順10を繰り返して**export**という別のコンテナーを作成します。

    ![In the New container dialog, images is entered into the Name field and highlighted. Private (no anonymous access) is selected for the public access level. The **Create** button is highlighted.](media/data-lake-new-container-export.png 'Storage and Containers blade')

12. 続いて、左側のナビゲーションメニューの設定内にある**アクセス キー**を選択します。**アクセス キー**の画面で、**キーの表示**を選択し、 **key1の接続文字列**の値表示フィールドに**クリップボードにコピー**ボタンがあるので、これを選択して**key1の接続文字列**の値をコピーします。

    ![In the Storage account blade, under Settings, Access keys is selected. Under Default keys, the copy button next to the key1 connection string is selected.](media/data-lake-access-keys.png 'Storage account blade')

13. クリップボードにコピーした値をメモ帳などのテキストエディターに貼り付け、以後のKey Vaultの手順で利用できるようにします。

## Task 2: 料金所Function Appのプロビジョニング

1. [Azure portal](https://portal.azure.com/)で、**ポータルメニューの表示**アイコンを選択して、**+リソースの作成**をメニューから選択します。

    ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. **マーケットプレースの検索**フィールドに**"**関数アプリ**と入力して検索し、その結果から **関数アプリ** を選択します。

    ![Function app is highlighted in the search box, and the Function App row is highlighted in the results below that.](media/create-resource-function-app.png "Azure Portal")

3. **関数アプリ**の画面で**作成**を選択します。

4. **関数アプリの作成**画面で、以下の項目を入力します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **関数アプリ名**: グローバルで一意な名前にします（例： "TollBoothFunctions-SUFFIX"）
   - **公開**: コードを選択します。
   - **ランタイム スタック**: .NETを選択します。
   - **バージョン**: 3.1を選択します。
   - **地域**: ハンズオンで利用するリージョンを選択します。

   **オペレーティングシステム**:

   - **オペレーティングシステム**: Windowsを選択します。

   **プラン**:

   - **プランの種類**: 消費量 (サーバーレス) を選択します。

   ![The information above is entered on the Create Function App basics tab.](media/create-function-app-basics-tab.png "Create Function App Settings")

5. **次: ホスティング**を選択します。

6. ホスティングタブで、以下の構成を設定します。

   - **ストレージ アカウント**: Task 1で作成した **datalakeSUFFIX** ストレージアカウントを選択します。

   ![The information above is entered on the Create Function App hosting tab.](media/create-function-app-hosting-tab.png "Create Function App Settings")

7. ネットワークタブの設定項目はないので、監視タブに移動し、以下の設定をします。

   - **Application Insightsを有効にする**: はいを選択します。
   - **Application Insights**: 新規作成を選択します。

   ![In the Monitoring tab of the Create Function App blade, the form fields are set to the previously defined values.](media/new-functionapp-net-monitoring.png "Function App Monitoring blade")

8. **Application Insights の新規作成**画面で、以下の情報を入力して**OK**を選択します。

   - **名前**: グローバルで一意な名前にします（例： **appinsights-SUFFIX** ）。緑色のチェックマークが出ることを確認してください。
   - **場所**: 関数アプリ用に選択したリージョンと同じAzureリージョンを選択します。

    ![The new Application Insights form is configured as described.](media/new-app-insights.png "Create new Application Insights")

9. **確認および作成**を選択します。

   ![The Review + create button is highlighted in the button bar.](media/review-create-button.png "Review + create")

10. **作成**を選択して新たな関数アプリのプロビジョニングを開始します。

11. 関数アプリのプロビジョニングが完了したら、Azure portalで **hands-on-lab-SUFFIX** リソースグループを開き、**TollBoothFunctions**で始まる名前の関数アプリを選択します。

    > 関数アプリがKey Vaultにアクセスしてシークレットを読み出せるようにするには、関数アプリの [システム割り当て済みマネージドIDを作成](https://docs.microsoft.com/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity) し、そのマネージドIDに対して [Key Vaultのアクセスポリシーを作成する](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault#key-vault-access-policies) 必要があります。

    ![In the hands-on-lab-SUFFIX resource group, the TollBoothFunctions Function App is highlighted.](media/resource-group-toll-booth-functions.png 'hands-on-lab-SUFFIX resource group')

12. On the Function App blade, select **Identity** in the left-hand navigation menu, and within the **System assigned** tab, switch **Status** to **On** and select **Save**.

    ![In the Identity blade, the System assigned tab is selected with the Status set to On, and the Save button is highlighted.](media/function-app-identity.png "Identity")

## Task 3: Provision the Events Function App

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

    ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "function app" into the **Search the marketplace** box and select **Function App** from the results.

    ![Function app is highlighted in the search box, and the Function App row is highlighted in the results below that.](media/create-resource-function-app.png "Azure Portal")

3. On the **Function App** blade, select **Create**.

4. On the **Create Function App** blade, enter the following:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Function App name:** Enter a globally unique name, such as "TollBoothEvents-SUFFIX".
   - **Publish:** Select Code.
   - **Runtime Stack** Select Node.js.
   - **Version**: Choose 14 LTS.
   - **Region:** Select the region you have been using for resources in this hands-on lab.

   ![The information above is entered on the Create Function App basics tab.](media/create-events-function-app-basics-tab.png "Create Function App Settings")

5. Select **Next: Hosting**.

6. On the Hosting tab, set the following configuration:

   - **Storage account:** Select the **datalakeSUFFIX** storage account you created in Task 1 above.
   - **Operating System**: Select Windows.
   - **Plan type:** Choose Consumption (Serverless).

   ![The information above is entered on the Create Function App hosting tab.](media/create-function-app-hosting-tab.png "Create Function App Settings")

7. Select **Next: Monitoring**, and on the Monitoring tab, enter the following:

   - **Enable Application Insights**: Select Yes.
   - **Application Insights**: Select the Application Insights instance you created earlier when provisioning the TollBoothFunctions Function App.

   ![In the Monitoring tab of the Create Function App blade, the form fields are set to the previously defined values.](media/event-function-app-net-monitoring.png "Function App Monitoring blade")

8. Select **Review + create** and then select **Create** to provision the Function App.

## Task 4: Provision an Event Grid topic

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "event grid topic" into the **Search the marketplace** box and select **Event Grid Topic** from the results.

   ![Event Grid Topic is highlighted in the search box, and the Event Grid Topic panel is highlighted in the results below that.](media/create-resource-event-grid-topic.png "Event Grid Topic")

3. On the **Event Grid Topic** blade, select **Create**.

4. On the **Create Topic** blade, specify the following configuration options:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Name:** Enter a globally unique value, such as **eventgridtopic-SUFFIX** (ensure the green checkmark appears).
   - **Location:** Select the region you have been using for resources in this hands-on lab.

   ![In the Create Topic blade, the Name field is set to eventgridtopic, and the Resource Group selected is hands-on-lab-SUFFIX.](media/new-event-grid-topic.png 'Create Topic blade')

5. Select **Review + Create**, then select **Create** in the screen that follows.

6. After the Event Grid topic has completed provisioning, open the account by opening the **hands-on-lab-SUFFIX** resource group and then selecting the **Event Grid Topic** resource from the list of available services.

7. On the Event Grid Topic blade, select **Overview** in the left-hand navigation menu and then copy the **Topic Endpoint** value and paste the value into a text editor, such as Notepad, for later reference.

   ![On the Event Grid Topic blade, Overview is selected and highlighted, and the Topic Endpoint value is highlighted.](media/event-grid-topic-endpoint.png 'Event Grid Topic blade')

8. Next, select **Access Keys** under Settings in the left-hand navigation menu on the Event Grid Topic blade.

9. Within the **Access Keys** blade, copy the **Key 1** value and paste it into a text editor, such as Notepad, for later reference.

   ![In the eventgridtopic blade, in the left menu under Settings, Access keys is selected. In the listing of Access keys, the copy button next to the Key 1 access key is selected.](media/event-grid-access-keys.png 'eventgridtopic - Access keys blade')

## Task 5: Provision and configure an Azure Cosmos DB account

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. On the **New** screen, select **Databases** then select **Azure Cosmos DB**.

   ![In Azure Portal, in the menu, New is selected. Under Azure marketplace, Databases is selected, and under Featured, Azure Cosmos DB is selected.](media/new-databases-cosmos-db.png 'Azure Portal')

3. On the **Create new Azure Cosmos DB** **account** blade, specify the following configuration options:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Account Name:** Enter a globally unique value, such as **cosmosdb-SUFFIX** (ensure the green check mark appears).
   - **API**: Select the **Core (SQL)** API.
   - **Location:** Select the region you have been using for resources in this hands-on lab.
   - **Capacity mode**: Select **Provisioned throughput**.
   - **Apply Free Tier Discount**: Select **Do Not Apply**.
   - **Account Type**: Select **Production**.
   - **Geo-Redundancy**: Choose **Disable**.
   - **Multi-region writes**: Select **Disable**.

   ![Fields in the Azure Cosmos DB blade are set to the previously defined settings.](media/new-cosmos-db-basics-tab.png 'Azure Cosmos DB blade')

4. Select **Review + create**, then select **Create**.

5. After the Azure Cosmos DB account has completed provisioning, open the account by opening the **hands-on-lab-SUFFIX** resource group and then selecting the **Azure Cosmos DB** resource from the list of available services in the resource group.

6. On the Cosmos DB blade, select **Data Explorer** in the left-hand navigation menu and then select **New Container**.

   ![In the Data Explorer blade, the Data Explorer item is selected in the left menu. The New Container button is selected in the Data Explorer pane.](media/data-explorer-new-container.png 'Data Explorer blade')

7. On the **Add Container** blade, specify the following configuration options:

   - **Database Id**: Choose **Create new** and enter **LicensePlates** for the name.
   - **Container Id**: Enter **Processed**.
   - **Partition key**: Enter **/licensePlateText**.
   - **Throughput**: Select **Autoscale** and enter **4000** into the **Max RU/s** box.

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-processed-collection.png 'Add Container blade')

8. Select **OK**.

9. Select **New Container** again on the Data Explorer blade to add another container.

10. On the **Add Container** blade, specify the following configuration options:

    - **Database Id**: Choose **Use existing** and select the **LicensePlates** database Id.
    - **Container Id**: Enter **NeedsManualReview**.
    - **Partition key**: Enter **/fileName**.
    - **Throughput**: Select **Autoscale** and enter **4000** into the **Max RU/s** box.

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-manual-review-collection.png 'Add Collection blade')

11. Select **OK**.

12. Select **Keys** under Settings in the left-hand navigation menu on the Cosmos DB blade.

13. Underneath the **Read-write Keys** tab within the Keys blade, copy the **URI** and **Primary Key** values.

    ![In the tollbooth - Keys blade, under Settings, Keys is selected. The copy buttons for the URI and Primary Key fields are selected on the Read-write Keys tab.](media/cosmos-db-keys.png 'tollbooth - Keys blade')

14. Paste the values into a text editor, such as Notepad, for later reference.

## Task 6: Provision the Computer Vision API service

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "computer vision" into the **Search the marketplace** box, select **Computer Vision** from the results, and then select the **Create** button on the **Computer Vision** blade.

3. On the **Create Computer Vision** blade, specify the following configuration options:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Region:** Select the region you have been using for resources in this hands-on lab.
   - **Name**: Enter a globally unique value, such as **computervision-SUFFIX** (ensure the green checkmark appears).
   - **Pricing tier**: Select **Standard S1 (10 Class per second)**.

   ![In the Create Computer Vision blade, fields are set to the previously defined values.](media/create-computer-vision-basics-tab.png 'Create blade')

4. Select **Review + create** and then select **Create**.

5. After the Computer Vision service has completed provisioning, open the service by opening the **hands-on-lab-SUFFIX** resource group and then selecting the **Computer Vision** Cognitive Service resource from the list of available services in the resource group.

6. Under Resource Management in the left-hand navigation menu, select **Keys and Endpoint**.

7. Within the **Keys and Endpoint** blade, copy the **Endpoint** and **KEY 1** values.

    ![In the Cognitive Services blade, under Resource Management, Keys and Endpoint is selected. The Copy button next to the Endpoint and Key 1 values are selected.](media/computer-vision-keys-and-endpoint.png 'Keys and Endpoint information')

8. Paste the values into a text editor, such as Notepad, for later reference.

## Task 7: Create a Logic App

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "logic app" into the **Search the marketplace** box and select **Logic App** from the results.

   ![Logic app is entered into the search the marketplace box and highlighted. The Logic App pane is highlighted in the search results.](media/new-logic-app.png "Create new resource")

3. Select the **Create** button on the **Logic App** blade.

4. On the **Create a logic app** blade Basics tab, specify the following configuration options:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Logic app name**: Enter a globally unique value, such as **logicapp-SUFFIX** (ensure the green check mark appears).
   - **Region:** Select the region you have been using for resources in this hands-on lab.
   - **Associate with integration service environment**: Leave unchecked.
   - **Enable log analysis**: Leave unchecked.

   ![The values specified above are entered into the create a logic app basics tab.](media/create-a-logic-app.png "Create a logic app")

5. Select **Review + create**, then select **Create**.

## Task 8: Provision Azure Key Vault

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "key vault" into the **Search the marketplace** box and select **Key Vault** from the results.

   ![Key vault is entered into the search the marketplace box and highlighted. The Key Vault pane is highlighted in the search results.](media/new-key-vault.png "Create new resource")

3. Select the **Create** button on the **Key Vault** blade.

4. On the **Create key vault** blade Basics tab, specify the following configuration options:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this hands-on lab.
   - **Resource Group**: Select the hands-on-lab-SUFFIX resource group from the list of existing resource groups.

   **Instance Details**:

   - **Key vault name**: Enter a globally unique value, such as **keyvault-SUFFIX** (ensure the green check mark appears).
   - **Region:** Select the region you have been using for resources in this hands-on lab.
   - **Pricing tier**: Select **Standard**.
   - **Days to retain deleted vaults**: Leave at 90.
   - **Purge protection**: Select the **Disable** option.

   ![In the Create key vault blade, fields are set to the previously defined values.](media/create-key-vault.png 'Create blade')

5. Select **Next: Access policy**.

6. On the Access policy tab, select **Add Access Policy**.

   ![The Add Access Policy link is highlighted on the Access Policy tab.](media/create-key-vault-access-policy-tab.png "Add access policy")

7. On the Add access policy dialog, enter the following:

   - **Secret permissions**: Select **Get** from the list of available permissions.
   - **Select principal**: Select *None selected** and in the dialog, enter **TollBoothFunctions** into the search box, select the TollBoothFunctions Function App's managed service identity from the results and then select **Select**.

   ![In the Add access policy form, the Select principal field is highlighted.](media/key-vault-add-access-policy-select-principal.png "Add access policy")

8. Select **Add** to add the new access policy.

9. Select **Review + create**, then select **Create**.

10. After the deployment completes, select **Go to resource** in the deployment complete notification.

    ![When the deployment completes, a message is displayed indicating Your deployment is complete. The Go to resource button is highlighted in the next steps section.](media/key-vault-deployment-complete.png "Your deployment is complete")

11. Select **Secrets** under Settings in the left-hand navigation menu.

12. Select **Generate/Import** to add a new key.

    ![The Secrets menu item and the Generate/Import button are highlighted.](media/generate-secret.png "Key Vault - Secrets")

13. Using the key values you copied into a text editor, such as Notepad, in the tasks above, use the table below for the name-value pairs to use when creating the secrets. You only need to populate the **Name** and **Value** fields for each secret and leave the other fields at their default values.

    |                          |                                     |
    | ------------------------ | ----------------------------------- |
    | **Name**                 | **Value**                           |
    | computerVisionApiKey     | Computer Vision API key             |
    | cosmosDBAuthorizationKey | Cosmos DB Primary Key               |
    | dataLakeConnectionString | Data Lake storage connection string |
    | eventGridTopicKey        | Event Grid Topic access key         |

14. When you are finished creating the secrets, your list should look similar to the following:

    ![The listing of secrets is displayed matching the previously defined values.](media/key-vault-keys.png "Key Vault Secrets")

## Task 9: Retrieve the URI for each secret

1. Open your Key Vault instance in the portal.

2. Still on the **Secrets** page in Key Vault, select the `computerVisionApiKey` secret.

   ![The computerVisionApiKey secret is highlighted in the secrets list.](media/key-vault-computer-vision-api-key.png "Secrets")

3. On the computerVisionApiKey blade, select the **Current Version** of the secret.

    ![The secret's current version is selected.](media/key-vault-secret-current-version.png "Current Version")

4. On the secret version blade, copy the **Secret Identifier** value by selecting the **Copy to clipboard** button.

    ![The Secret Identifier field is highlighted. Next to this field is a copy button.](media/key-vault-secret-identifier.png "Secret Identifier")

5. Paste the secret identifier value into a text editor, such as Notepad, for later reference.

6. Close the `computerVisionApiKey` blades and return to the **Secrets** page of your Key Vault.

7. Repeat steps 2 through 6 above for the other three secrets listed.

8. When you are done copying and pasting the secret identifier values, you should have a list similar to the following in your text editor document:

   ```text
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/computerVisionApiKey/ce228a43f40140dd8a9ffb9a25d042ee)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/cosmosDBAuthorizationKey/1f9a0d16ad22409b85970b3c794a218c)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/dataLakeConnectionString/771aa40adac64af0b2aefbd741bd46ef)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/eventGridTopicKey/e310bcd71a72489f89b6112234fed815)
   ```

## Task 10: Configure application settings for the TollBoothFunctions Function App

1. Navigate to the **TollBoothFunctions** Function App resource in the Azure portal by opening the **hands-on-lab-SUFFIX** resource group and then select the Azure Function App resource whose name begins with **TollBoothFunctions**.

   ![In the hands-on-lab-SUFFIX resource group, the TollBoothFunctions Function App is highlighted.](media/resource-group-toll-booth-functions.png 'hands-on-lab-SUFFIX resource group')

2. On the Function App blade, select **Configuration** in the left-hand navigation menu.

    ![In the TollBoothFunctionApp blade on the Overview tab, under Configured features, the Configuration item is selected.](media/function-app-configuration-menu.png 'TollBoothFunctionApp blade')

3. Scroll to the **Application settings** section. Use the **+ New application setting** link to create the following additional key-value pairs (the key names must exactly match those found in the table below). **Be sure to remove the curly braces (`{}`)**.

    |                          |           |
    | ------------------------ | --------- |
    | **Application Key**      | **Value** |
    | computerVisionApiKey     | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **computerVisionApiKey** Key Vault secret |
    | computerVisionApiUrl     | Computer Vision API endpoint you copied earlier with **vision/v3.0/ocr** appended to the end. Example: `https://eastus.api.cognitive.microsoft.com/vision/v3.0/ocr` |
    | cosmosDBAuthorizationKey | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **cosmosDBAuthorizationKey** Key Vault secret |
    | cosmosDBCollectionId     | Cosmos DB processed collection id (**Processed**) |
    | cosmosDBDatabaseId       | Cosmos DB database id (**LicensePlates**) |
    | cosmosDBEndpointUrl      | Cosmos DB URI |
    | dataLakeConnection | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **dataLakeConnectionString** Key Vault secret |
    | eventGridTopicEndpoint   | Event Grid Topic endpoint |
    | eventGridTopicKey        | Enter `@Microsoft.KeyVault(SecretUri={referenceString})`, where `{referenceString}` is the URI for the **eventGridTopicKey** Key Vault secret |
    | exportCsvContainerName   | Data lake CSV export container name (**export**) |

    ![In the Application Settings section, the previously defined key/value pairs are displayed.](media/application-settings.png 'Application Settings')

4. Select **Save** on the Configuration toolbar to save the application settings changes.

## Task 11: Set up a development virtual machine

In this task, you provision a virtual machine (VM) in Azure. The VM image used has the latest version of Visual Studio Community 2019 installed.

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "visual studio 2019" into the Search the Marketplace box and then select **Visual Studio 2019 Latest** from the results.

   !["Visual studio 2019" is entered into the Search the Marketplace box. Visual Studio 2019 latest is selected in the results.](media/create-resource-visual-studio-vm.png "Visual Studio 2019 Latest")

3. On the Visual Studio 2019 Latest blade, select **Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64)** from the Select a software plan drop-down list, and then select **Create**.

   ![On the Visual Studio 2019 Latest blade, Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64) is highlighted in the Select a software plan drop-down list.](media/visual-studio-create.png "Visual Studio 2019 Latest")

4. On the Create a virtual machine **Basics** tab, set the following configuration:

   - Project Details:

     - **Subscription**: Select the subscription you are using for this hands-on lab.
     - **Resource Group**: Select the **hands-on-lab-SUFFIX** resource group from the list of existing resource groups.

   - Instance Details:

     - **Virtual machine name**: Enter LabVM.
     - **Region**: Select the region you are using for resources in this hands-on lab.
     - **Availability options**: Select no infrastructure redundancy required.
     - **Image**: Leave Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64) selected.
     - **Azure Spot instance**: Select No.
     - **Size**: Accept the default size of Standard_D4s_v3.

   - Administrator Account:

     - **Username**: Enter **demouser**.
     - **Password**: Enter **Password.1!!**

   - Inbound Port Rules:

     - **Public inbound ports**: Choose Allow selected ports.
     - **Select inbound ports**: Select RDP (3389) in the list.

   ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/lab-virtual-machine-basics-tab.png "Create a virtual machine Basics tab")

   > **Note**: Default settings are used for the remaining tabs so that they can be skipped.

5. Select **Review + create** to validate the configuration.

6. On the **Review + create** tab, ensure the Validation passed message is displayed, and then select **Create** to provision the virtual machine.

   ![The Review + create tab is displayed, with a Validation passed message.](media/lab-virtual-machine-review-create-tab.png "Create a virtual machine Review + create tab")

7. It takes approximately 5 minutes for the VM to finish provisioning.

## Task 12: Connect to the Lab VM

In this task, you create an RDP connection to your Lab virtual machine (VM).

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** from the Azure services list.

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. Select the **hands-on-lab-SUFFIX** resource group from the list.

   ![The "hands-on-lab-SUFFIX" resource group is highlighted.](media/resource-groups.png "Resource groups list")

3. In the list of resources within your resource group, select the **LabVM Virtual machine** resource.

   ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and LabVM is highlighted.](media/resource-group-resources-labvm.png "LabVM in resource group list")

4. On your LabVM blade, select **Connect** and **RDP** from the top menu.

   ![The LabVM blade is displayed, with the Connect button highlighted in the top menu.](media/connect-vm-rdp.png "Connect to Lab VM")

5. On the Connect to virtual machine blade, select **Download RDP File**, then open the downloaded RDP file.

   ![The Connect to virtual machine blade is displayed, and the Download RDP File button is highlighted.](media/connect-to-virtual-machine.png "Connect to virtual machine")

6. Select **Connect** on the Remote Desktop Connection dialog.

   ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](media/remote-desktop-connection.png "Remote Desktop Connection dialog")

7. Enter the following credentials when prompted, and then select **OK**:

   - **Username**: demouser
   - **Password**: Password.1!!

   ![The credentials specified above are entered into the Enter your credentials dialog.](media/rdc-credentials.png "Enter your credentials")

8. Select **Yes** to connect if prompted that the remote computer's identity cannot be verified.

   ![In the Remote Desktop Connection dialog box, a warning states that the remote computer's identity cannot be verified and asks if you want to continue anyway. At the bottom, the Yes button is highlighted.](media/remote-desktop-connection-identity-verification-labvm.png "Remote Desktop Connection dialog")

## Task 13: Disable Internet Explorer Enhanced Security

In this task, you disable Internet Explorer Enhanced Security Configuration (IE ESC) on the LabVM.

> > **Note**: Sometimes this Visual Studio 2019 Latest image has IE ESC disabled already, and sometimes it does not.

1. Once logged in, launch the **Server Manager**. This should start automatically, but you can access it via the Start menu if it does not.

2. Select **Local Server**, then select **On** next to **IE Enhanced Security Configuration**.

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](media/windows-server-manager-ie-enhanced-security-configuration.png "Server Manager")

3. In the Internet Explorer Enhanced Security Configuration dialog, select **Off** under both Administrators and Users, and then select **OK**.

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](media/internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

4. You can close the Server Manager but leave the connection to the LabVM open for the next task.

## Task 14: Install required software on the LabVM

In this task, you configure the LabVM with the required software and downloads. First, you download and install the Microsoft Edge web browser. Next, you download a copy of the Visual Studio starter solution and unzip it into a folder named `C:\ServerlessMCW`.

> **Note**: Some aspects of this lab require using the new Microsoft Edge (Chromium edition) browser. You may find yourself blocked if using Internet Explorer later in the lab, as Internet Explorer is not supported for some specific activities.

1. To install Microsoft Edge, launch Internet Explorer on the VM and download [Microsoft Edge](https://msedge.sf.dl.delivery.mp.microsoft.com/filestreamingservice/files/0a4291f0-226e-4d0a-a702-7aa901f20ff4/MicrosoftEdgeEnterpriseX64.msi).

2. Run the downloaded installer and follow the setup instruction.

3. Next, [download a copy of the Serverless Architecture MCW GitHub repo](https://github.com/microsoft/MCW-Serverless-architecture/archive/main.zip).

4. Extract the download ZIP file to `C:\ServerlessMCW`.

   ![The Extract Compressed Folders dialog is displayed, with `C:\ServerlessMCW` entered into the extraction location.](media/zip-extract.png "Extract Compressed ZIP")
