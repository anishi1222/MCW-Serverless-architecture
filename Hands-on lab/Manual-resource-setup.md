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
  - [Task 10: TollBoothFunctions関数アプリのためのアプリケーション設定の構成](#task-10-TollBoothFunctions関数アプリのためのアプリケーション設定の構成)
  - [Task 11: 開発用VMのセットアップ](#task-11-開発用VMのセットアップ)
  - [Task 12: ハンズオンVMへの接続](#task-12-ハンズオンVMへの接続)
  - [Task 13: Internet Explorerのセキュリティ強化の無効化](#task-13-Internet-Explorerのセキュリティ強化の無効化)
  - [Task 14: ハンズオンVMへの必要なソフトウェアのインストール](#task-14-ハンズオンVMへの必要なソフトウェアのインストール)

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

1. Azure Portalでキーコンテナー (Key Vault) インスタンスを開きます。

2. **シークレット**ページで、`computerVisionApiKey`シークレットを開きます。

   ![The computerVisionApiKey secret is highlighted in the secrets list.](media/key-vault-computer-vision-api-key.png "Secrets")

3. `computerVisionApiKey`の画面で、シークレットの**現在のバージョン**を選択します。

    ![The secret's current version is selected.](media/key-vault-secret-current-version.png "Current Version")

4. シークレットのバージョン画面で**クリップボードにコピー**ボタンを選択し、**シークレット識別子**の値をコピーします。

    ![The Secret Identifier field is highlighted. Next to this field is a copy button.](media/key-vault-secret-identifier.png "Secret Identifier")

5. シークレット識別子の値をメモ帳などのテキストエディターにコピーします。この値は後で利用します。

6. `computerVisionApiKey`の画面を閉じ、Key vaultの**シークレット**のページに戻ります。

7. 2から6の手順を繰り返して別の3個のシークレットも同様にコピーして、テキストエディターにコピーします。

8. シークレット識別子のコピー＆ペーストが終わると、テキストエディターでは以下のような感じになているはずです。

   ```text
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/computerVisionApiKey/ce228a43f40140dd8a9ffb9a25d042ee)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/cosmosDBAuthorizationKey/1f9a0d16ad22409b85970b3c794a218c)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/dataLakeConnectionString/771aa40adac64af0b2aefbd741bd46ef)
   @Microsoft.KeyVault(SecretUri=https://tollboothvault.vault.azure.net/secrets/eventGridTopicKey/e310bcd71a72489f89b6112234fed815)
   ```

## Task 10: TollBoothFunctions関数アプリのためのアプリケーション設定の構成

1. Azure Portalで**hands-on-lab-SUFFIX**リソースグループを開いて、**TollBoothFunctions**で始まる名前のAzure関数アプリリソースを選択して、**TollBoothFunctions**関数アプリリソースへと移動します。

   ![In the hands-on-lab-SUFFIX resource group, the TollBoothFunctions Function App is highlighted.](media/resource-group-toll-booth-functions.png 'hands-on-lab-SUFFIX resource group')

2. 関数アプリ画面で、左側のナビゲーションメニューにある**構成**を選択します。

    ![In the TollBoothFunctionApp blade on the Overview tab, under Configured features, the Configuration item is selected.](media/function-app-configuration-menu.png 'TollBoothFunctionApp blade')

3. **アプリケーション設定**へと移動します。**+新しいアプリケーション設定**のリンクを使って以下のKey-Valueペアを追加します (キー名は以下の表の通りである必要があります）。なお、**中括弧(`{}`)は除去してください**。

    |                          |           |
    | ------------------------ | --------- |
    | **アプリケーション キー**      | **値** |
    | computerVisionApiKey     | `@Microsoft.KeyVault(SecretUri={referenceString})` ここで `{referenceString}` は**computerVisionApiKey** Key VaultシークレットのURIです。 |
    | computerVisionApiUrl     | Computer Vision APIエンドポイントに、**vision/v3.0/ocr**を付加してください。【例】 `https://eastus.api.cognitive.microsoft.com/vision/v3.0/ocr` |
    | cosmosDBAuthorizationKey | `@Microsoft.KeyVault(SecretUri={referenceString})` ここで `{referenceString}` は**cosmosDBAuthorizationKey** Key VaultシークレットのURIです。 |
    | cosmosDBCollectionId     | Cosmos DBの処理対象コレクションID (**Processed**) |
    | cosmosDBDatabaseId       | Cosmos DB データベースID (**LicensePlates**) |
    | cosmosDBEndpointUrl      | Cosmos DB URI |
    | dataLakeConnection | `@Microsoft.KeyVault(SecretUri={referenceString})` ここで `{referenceString}` は**dataLakeConnectionString** Key VaultシークレットのURIです。 |
    | eventGridTopicEndpoint   | Event Grid Topicエンドポイント |
    | eventGridTopicKey        | `@Microsoft.KeyVault(SecretUri={referenceString})` ここで `{referenceString}` は**eventGridTopicKey** Key VaultシークレットのURIです。 |
    | exportCsvContainerName   | Data lakeのCSVエクスポート先のコンテナー名 (**export**) |

    ![In the Application Settings section, the previously defined key/value pairs are displayed.](media/application-settings.png 'Application Settings')

4. 構成ページのツールバーにある**保存**を選択して、アプリケーション設定の変更を保存します。

## Task 11: 開発用VMのセットアップ

このタスクでは、Azureで仮想マシン (VM) をプロビジョニングします。利用するVMイメージには、最新のVisual Studio Community 2019がインストール済みです。

1. In the [Azure portal](https://portal.azure.com/) で、 **ポータルメニューの表示** アイコンを選択し、メニューから **+リソースの作成** を選択します。

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. マーケットプレースの検索フィールドに"visual studio 2019"と入力して検索し、その結果から **visual studio 2019 Latest** を選択します。

   !["Visual studio 2019" is entered into the Search the Marketplace box. Visual Studio 2019 latest is selected in the results.](media/create-resource-visual-studio-vm.png "Visual Studio 2019 Latest")

3. Visual Studio 2019 Latestの画面で、**Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64)**をプランのドロップダウンリストから選択し、**作成**を選択します。

   ![On the Visual Studio 2019 Latest blade, Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64) is highlighted in the Select a software plan drop-down list.](media/visual-studio-create.png "Visual Studio 2019 Latest")

4. 仮想マシンの作成画面の**基本**タブで、 以下の構成を設定します。

   **プロジェクトの詳細**:

   - **サブスクリプション**: このハンズオンで使うサブスクリプションを選択します。
   - **リソースグループ**: 既存のリソースグループのリストから、**hands-on-lab-SUFFIX**リソースグループを選択します。

   **インスタンスの詳細**:

   - **仮想マシン名**: LabVMと入力します。
   - **地域**: ハンズオンで利用するリージョンを選択します。
   - **可用性オプション**: "インフラストラクチャ冗長は必要ありません"を選択します。
   - **イメージ**: デフォルトのままで変更しません（Visual Studio 2019 Community (latest release) on Windows Server 2019 (x64)）
   - **Azureスポットインスタンス**: デフォルトのままで選択しません。
   - **サイズ**: デフォルトのサイズ（Standard_D4s_v3）のままにします。

   **管理者アカウント**:

   - **ユーザー名**: **demouser**を入力します。
   - **パスワード**: **Password.1!!**を入力します。

   **受信ポートの規則**:

   - **パブリック受信ポート**: "選択したポートを許可する"を選択します。
   - **受信ポートを選択**: リストからRDP (3389)を選択します。

   ![Screenshot of the Basics tab, with fields set to the previously mentioned settings.](media/lab-virtual-machine-basics-tab.png "Create a virtual machine Basics tab")

   > **注意**: 残りのタブはデフォルト設定で問題ないので、スキップできます。
   
5. **確認および作成**を選択し、構成を検証します。

6. **確認および作成**タブで、検証が完了した旨のメッセージが表示されたことを確認してから、**作成**を選択して、仮想マシンのプロビジョニングを開始します。

   ![The Review + create tab is displayed, with a Validation passed message.](media/lab-virtual-machine-review-create-tab.png "Create a virtual machine Review + create tab")

7. VMのプロビジョニング完了までおよそ5分ほどかかります。

## Task 12: ハンズオンVMへの接続

このタスクでは、ハンズオンVMへのRDP接続を作成します。

1. [Azure portal](https://portal.azure.com)で、Azureサービスリストから**リソースグループ**を選択します。

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

## Task 13: Internet Explorerのセキュリティ強化の無効化

このタスクでは、LabVMでInternet Explorerのセキュリティ強化 (IE ESC) の設定を無効化します。

> > **注意**: このVisual Studio 2019 Latestイメージでは、すでにIE ESCが無効化されている場合があります。

1. VMにログインし**Server Manager**を起動します。これは自動的に開始するはずですが、起動しない場合にはスタートメニューからアクセスできます。

2. **Local Server**を選択し、**IE Enhanced Security Configuration**の隣の**On**を選択します。

    ![Screenshot of the Server Manager. In the left pane, Local Server is selected. In the right, Properties (For LabVM) pane, the IE Enhanced Security Configuration, which is set to On, is highlighted.](media/windows-server-manager-ie-enhanced-security-configuration.png "Server Manager")

3. Internet Explorer Enhanced Security Configurationの画面で、AdministratorsとUsersの両方の設定で**Off**を選択し、**OK**を選択します。

    ![Screenshot of the Internet Explorer Enhanced Security Configuration dialog box, with Administrators set to Off.](media/internet-explorer-enhanced-security-configuration-dialog.png "Internet Explorer Enhanced Security Configuration dialog box")

4. Server Managerを閉じてかまいませんが、LabVMへの接続は閉じないでおきます（引き続き次のタスクで利用します）。

## Task 14: ハンズオンVMへの必要なソフトウェアのインストール

このタスクでは、LabVMに必要なソフトウェアをダウンロードし、構成します。まず、Microsoft Edge Webブラウザをダウンロード、インストールします。続いて、Visual Studioスターターソリューションをダウンロードし、`C:\ServerlessMCW`というディレクトリに展開します。

> **注意**: このハンズオンでは、この新しいMicrosoft Edge (Chromium版) Webブラウザを使う必要があります。Internet Explorerは特定のアクティビティではサポートされていないため、ハンズオンの後半でInternet Explorerを使用するとブロックされる可能性があります。

1. Microsoft Edgeをインストールするために、VMでInternet Explorerを起動し、[Microsoft Edge](https://msedge.sf.dl.delivery.mp.microsoft.com/filestreamingservice/files/0a4291f0-226e-4d0a-a702-7aa901f20ff4/MicrosoftEdgeEnterpriseX64.msi)をダウンロードします。

2. ダウンロードしたインストーラーを実行して、セットアップ手順に従います。

3. 続いて[Serverless Architecture MCW GitHubリポジトリのコピーをダウンロードします。](https://github.com/microsoft/MCW-Serverless-architecture/archive/main.zip)

4. ダウンロードしたZIPファイルを`C:\ServerlessMCW`に展開します。

   ![The Extract Compressed Folders dialog is displayed, with `C:\ServerlessMCW` entered into the extraction location.](media/zip-extract.png "Extract Compressed ZIP")
