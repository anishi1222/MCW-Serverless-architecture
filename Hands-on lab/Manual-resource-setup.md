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
  - [Task 3: イベント関数アプリのプロビジョニング](#task-3-イベント関数アプリのプロビジョニング)
  - [Task 4: Event Gridトピックのプロビジョニング](#task-4-Event-Gridトピックのプロビジョニング)
  - [Task 5: Azure Cosmos DBアカウントのプロビジョニングと構成](#task-5-Azure-Cosmos-DBアカウントのプロビジョニングと構成)
  - [Task 6: Computer Vision APIサービスのプロビジョニング](#task-6-Computer-Vision-APIサービスのプロビジョニング)
  - [Task 7: ロジックアプリの作成](#task-7-ロジックアプリの作成)
  - [Task 8: Azure Key Vaultのプロビジョニング](#task-8-Azure-Key-Vaultのプロビジョニング)
  - [Task 9: 各シークレットのURI取得](#task-9-各シークレットのURI取得)
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

1. [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

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

   - **名前**: グローバルで一意な名前にします（例： **appinsights-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **場所**: 関数アプリ用に選択したリージョンと同じAzureリージョンを選択します。

    ![The new Application Insights form is configured as described.](media/new-app-insights.png "Create new Application Insights")

9. **確認および作成**を選択します。

   ![The Review + create button is highlighted in the button bar.](media/review-create-button.png "Review + create")

10. **作成**を選択して新たな関数アプリのプロビジョニングを開始します。

11. 関数アプリのプロビジョニングが完了したら、Azure portalで **hands-on-lab-SUFFIX** リソースグループを開き、**TollBoothFunctions**で始まる名前の関数アプリを選択します。

    > 関数アプリがKey Vaultにアクセスしてシークレットを読み出せるようにするには、関数アプリの [システム割り当て済みマネージドIDを作成](https://docs.microsoft.com/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity) し、そのマネージドIDに対して [Key Vaultのアクセスポリシーを作成する](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault#key-vault-access-policies) 必要があります。

    ![In the hands-on-lab-SUFFIX resource group, the TollBoothFunctions Function App is highlighted.](media/resource-group-toll-booth-functions.png 'hands-on-lab-SUFFIX resource group')

12. 関数アプリの画面で、左側のナビゲーションメニューにある**ID**を選択し、 **システム割り当て済み** タブを開いて（デフォルトで開くはずです）、**状態**を**オン**にして**保存**を選択します。

    ![In the Identity blade, the System assigned tab is selected with the Status set to On, and the Save button is highlighted.](media/function-app-identity.png "Identity")

## Task 3: イベント関数アプリのプロビジョニング

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

   - **関数アプリ名**: グローバルで一意な名前にします（例： "TollBoothEvents-SUFFIX"）
   - **公開**: コードを選択します。
   - **ランタイム スタック**: Node.jsを選択します。
   - **バージョン**: 14 LTSを選択します。
   - **地域**: ハンズオンで利用するリージョンを選択します。

   **オペレーティングシステム**:

   - **オペレーティングシステム**: Windowsを選択します。

   **プラン**:

   - **プランの種類**: 消費量 (サーバーレス) を選択します。

   ![The information above is entered on the Create Function App basics tab.](media/create-events-function-app-basics-tab.png "Create Function App Settings")

5. **次:ホスティング**を選択します。

6. ホスティングタブで、以下の構成を設定します。

   - **ストレージ アカウント**: Task 1で作成した **datalakeSUFFIX** ストレージアカウントを選択します。

   ![The information above is entered on the Create Function App hosting tab.](media/create-function-app-hosting-tab.png "Create Function App Settings")

7. ネットワークタブの設定項目はないので、監視タブに移動し、以下の設定をします。

   - **Application Insightsを有効にする**: はいを選択します。
   - **Application Insights**: TollBoothFunctions関数アプリをプロビジョニングした際に作成したApplication Insightsインスタンスを選択します。

   ![In the Monitoring tab of the Create Function App blade, the form fields are set to the previously defined values.](media/event-function-app-net-monitoring.png "Function App Monitoring blade")

8. **確認および作成**を選択し、**作成**を選択して新たな関数アプリのプロビジョニングを開始します。

## Task 4: Event Gridトピックのプロビジョニング

1. In the [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. マーケットプレースの検索フィールドに"event grid topic"と入力して検索し、その結果から **Event Grid Topic** を選択します。その後、**作成**を選択します。

   ![Event Grid Topic is highlighted in the search box, and the Event Grid Topic panel is highlighted in the results below that.](media/create-resource-event-grid-topic.png "Event Grid Topic")

3. **Event Grid Topic**の画面で**作成**を選択します。

4. **トピックの作成**画面で、以下の構成オプションを指定します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:
   - **名前**: グローバルで一意な名前にします（例： **eventgridtopic-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **地域**: ハンズオンで利用するリージョンを選択します。

   ![In the Create Topic blade, the Name field is set to eventgridtopic, and the Resource Group selected is hands-on-lab-SUFFIX.](media/new-event-grid-topic.png 'Create Topic blade')

5. **確認および作成**を選択し、**作成**を選択します。

6. Event Gridトピックのプロビジョニングが完了したら、Azure portalで **hands-on-lab-SUFFIX**リソースグループを開き、リソースグループ内の利用可能なサービスから**Event Grid Topic**リソースを選択して開きます。
 
7. Event Gridトピックの画面で、左側のナビゲーションメニューにある**概要**を選択し、右側に現れる画面の**トピック エンドポイント**の値をコピーして、メモ帳などのテキストエディターに貼り付けます。この値は後で利用します。

   ![On the Event Grid Topic blade, Overview is selected and highlighted, and the Topic Endpoint value is highlighted.](media/event-grid-topic-endpoint.png 'Event Grid Topic blade')

8. 続いて、Event Gridトピック画面の左側のナビゲーションメニューにある設定の下の**アクセス キー**を選択します。

9. **アクセス キー**の画面で、 **キー 1**の値をコピーして、メモ帳などのテキストエディターに貼り付けます。この値は後で利用します。

   ![In the eventgridtopic blade, in the left menu under Settings, Access keys is selected. In the listing of Access keys, the copy button next to the Key 1 access key is selected.](media/event-grid-access-keys.png 'eventgridtopic - Access keys blade')

## Task 5: Azure Cosmos DBアカウントのプロビジョニングと構成

1. In the [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. カテゴリから**データベース**を選択して、続いて現れる画面で**Azure Cosmos DB**を選択します

   ![In Azure Portal, in the menu, New is selected. Under Azure marketplace, Databases is selected, and under Featured, Azure Cosmos DB is selected.](media/new-databases-cosmos-db.png 'Azure Portal')

3. **API オプションの選択** 画面で、**コア (SQL) - 推奨**を選択し、**作成**をクリックします。

4. **Azure Cosmos DB アカウントの作成 - コア (SQL)** 画面の**Basics**タブで、以下の設定を指定します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **アカウント名**: グローバルで一意な名前にします（例： **cosmosdb-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **場所**: ハンズオンで利用するリージョンを選択します。
   - **容量モード**: **プロビジョニングされたスループット**を選択します。
   - **Free レベル割引の適用**: **適用しない**を選択します。

   ![Fields in the Azure Cosmos DB blade are set to the previously defined settings.](media/new-cosmos-db-basics-tab.png 'Azure Cosmos DB blade')

5. **確認および作成**を選択し、**作成**を選択します。

5. Azure Cosmos DBアカウントのプロビジョニングが完了したら、**hands-on-lab-SUFFIX**リソースグループを開き、リソースグループ内の利用可能なサービスから**Azure Cosmos DB**リソースを選択して開きます。

6. Cosmos DB画面で、左側のナビゲーションメニューにある**データ エクスプローラー**を開き、**New Container**を選択します。

   ![In the Data Explorer blade, the Data Explorer item is selected in the left menu. The New Container button is selected in the Data Explorer pane.](media/data-explorer-new-container.png 'Data Explorer blade')

7. **New Contaier**画面で、以下の設定オプションを指定します。

   - **Database Id**: **Create new**を選択し、名前として**LicensePlates**を指定します。
   - **Container Id**: **Processed**を入力します。
   - **Partition key**: **/licensePlateText**を入力します。
   - **Throughput**: **Autoscale**を選択し、**Max RU/s**のフィールドには**4000**を入力します。

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-processed-collection.png 'Add Container blade')

8. **OK**を選択します。

9. 再度データエクスプローラーの画面で**New Container**を選択し、別のコンテナーを追加します。

10. **New Contaier**画面で、以下の設定オプションを指定します。

    - **Database Id**: **Use existing**を選択し、database Idとして**LicensePlates**を選択します。
    - **Container Id**: **NeedsManualReview**を入力します。
    - **Partition key**: **/fileName**を入力します。
    - **Throughput**: **Autoscale**を選択し、**Max RU/s**のフィールドには**4000**を入力します。

    ![In the Add Container blade, fields are set to the previously defined values.](media/cosmosdb-add-manual-review-collection.png 'Add Collection blade')

11. **OK**を選択します。

12. Cosmos DB画面で、左側のナビゲーションメニューにある**キー**を選択します。

13. **読み取り/書き込みキー**タブの下の**URI**と**プライマリ キー**の値をコピーしておきます。

    ![In the tollbooth - Keys blade, under Settings, Keys is selected. The copy buttons for the URI and Primary Key fields are selected on the Read-write Keys tab.](media/cosmos-db-keys.png 'tollbooth - Keys blade')

14. メモ帳のようなテキストエディターにコピーした値を貼り付けます。これらの値は後ほど利用します。

## Task 6: Computer Vision APIサービスのプロビジョニング

1. [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. **マーケットプレースの検索**フィールドに**Computer Vision**と入力して検索し、その結果から **Computer Vision** を選択します。その後、**作成**を選択します。

3. **Computer Visionを作成する**画面で、以下の構成オプションを指定します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **リージョン**: ハンズオンで利用するリージョンを選択します。
   - **名前**: グローバルで一意な名前にします（例：**computervision-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **価格レベル**: **Standard S1 (10 Class per second)**を選択します。

   ![In the Create Computer Vision blade, fields are set to the previously defined values.](media/create-computer-vision-basics-tab.png 'Create blade')

4. **確認および作成**を選択し、**作成**を選択します。

5. Computer Visionサービスのプロビジョニング完了後、Azure portalで **hands-on-lab-SUFFIX**リソースグループを開き、リソースグループ内の利用可能なサービスから**Computer Vision** Cognitive Serviceリソースを選択して開きます。

6. 左側のナビゲーションメニューにある**リソース管理**の下にある、**キーとエンドポイント**を選択します。

7. **キーとエンドポイント**の画面にある、**エンドポイント**と**キー 1**の値をコピーしておきます。

    ![In the Cognitive Services blade, under Resource Management, Keys and Endpoint is selected. The Copy button next to the Endpoint and Key 1 values are selected.](media/computer-vision-keys-and-endpoint.png 'Keys and Endpoint information')

8. メモ帳などのテキストエディターにコピーした値を貼り付けます。この値は後で利用します。

## Task 7: ロジックアプリの作成

1. [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. **マーケットプレースの検索**フィールドに**ロジック アプリ**と入力して検索し、その結果から **ロジック アプリ** を選択します。

   ![Logic app is entered into the search the marketplace box and highlighted. The Logic App pane is highlighted in the search results.](media/new-logic-app.png "Create new resource")

3. **ロジック アプリ**の画面で、**作成**ボタンをクリックします。

4. **ロジック アプリの作成**画面の基本タブで、以下の構成オプションを指定します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **ロジック アプリ名**: グローバルで一意な名前にします（例：**logicapp-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **地域**: ハンズオンで利用するリージョンを選択します。
   - **ログ分析を有効化**: チェックせずにおきます。

   **プラン**:

   - **プランの種類**: **消費**を選択します。

   **ゾーン冗長 (プレビュー)**

   - **ゾーン冗長**: **無効**を選択します。

   ![The values specified above are entered into the create a logic app basics tab.](media/create-a-logic-app.png "Create a logic app")

5. **確認および作成**を選択し、**作成**を選択します。

## Task 8: Azure Key Vaultのプロビジョニング

1. [Azure portal](https://portal.azure.com/)で、**ポータルメニューの表示**アイコンを選択して、**+リソースの作成**をメニューから選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. **マーケットプレースの検索**フィールドに**キー コンテナー**と入力して検索し、その結果から **key vault** を選択します。

   ![Key vault is entered into the search the marketplace box and highlighted. The Key Vault pane is highlighted in the search results.](media/new-key-vault.png "Create new resource")

3. **Key Vault**の画面で**作成**を選択します。

4. **キー コンテナーの作成**画面で、以下の項目を入力します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **Key vault名**: グローバルで一意な名前にします（例：**keyvault-SUFFIX**）。緑色のチェックマークが出ることを確認してください。
   - **地域**: ハンズオンで利用するリージョンを選択します。
   - **価格レベル**: **標準**を選択します。
   - **削除されたコンテナーを保持する日数**: 90のままで変更せずにおきます。
   - **消去保護**: **消去保護を無効にする**を選択します。

   ![In the Create key vault blade, fields are set to the previously defined values.](media/create-key-vault.png 'Create blade')

5. **次へ: アクセス ポリシー**を選択します。

6. アクセス ポリシータブで、**+ アクセス ポリシーの追加**を選択します。

   ![The Add Access Policy link is highlighted on the Access Policy tab.](media/create-key-vault-access-policy-tab.png "Add access policy")

7. アクセス ポリシーの追加画面で、以下を入力します。

   - **シークレットのアクセス許可**: 利用可能な許可のうち、**取得**を選択します。
   - **プリンシパルの選択**: **選択されていません**をクリックして、**TollBoothFunctions**を検索ボックスに入力して検索し、検索結果からTollBoothFunctions関数アプリのマネージドIDを選択して、**選択**をクリックします。

   ![In the Add access policy form, the Select principal field is highlighted.](media/key-vault-add-access-policy-select-principal.png "Add access policy")

8. **追加**を選択し、新たなアクセスポリシーを追加します。

9. **確認および作成**を選択し、**作成**を選択します。

10. デプロイ完了後、**リソースへ移動**をクリックします。

    ![When the deployment completes, a message is displayed indicating Your deployment is complete. The Go to resource button is highlighted in the next steps section.](media/key-vault-deployment-complete.png "Your deployment is complete")

11. 左側のナビゲーションメニューにある**設定**の下にある、**シークレット**を選択します。

12. **+生成/インポート**を選択して新たなキーを追加します。

    ![The Secrets menu item and the Generate/Import button are highlighted.](media/generate-secret.png "Key Vault - Secrets")

13. これまでにメモ帳などのテキストエディターにコピーしたキー値を使い、下表の名前－値のペアに従って、シークレットを作成します。各シークレットに**名前**と**値**を入れるだけで問題ありません（その他のフィールドはデフォルトでOKです）。

    |                          |                                     |
    | ------------------------ | ----------------------------------- |
    | **Name**                 | **Value**                           |
    | computerVisionApiKey     | Computer Vision API key             |
    | cosmosDBAuthorizationKey | Cosmos DB Primary Key               |
    | dataLakeConnectionString | Data Lake storage connection string |
    | eventGridTopicKey        | Event Grid Topic access key         |

14. シークレット作成が完了したら、以下のような感じになっているはずです。

    ![The listing of secrets is displayed matching the previously defined values.](media/key-vault-keys.png "Key Vault Secrets")

## Task 9: 各シークレットのURI取得

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
