![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshops')

<div class="MCWHeader1">
サーバーレス・アーキテクチャ
</div>

<div class="MCWHeader2">
ハンズオン環境構成ガイド
</div>

<div class="MCWHeader3">
2021年11月
</div>

本書に記載されている情報は、URLやその他のインターネット上のウェブサイトを含め、予告なく変更されることがあります。特に断りのない限り、ここに記載されている会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントの例は架空のものであり、実在の会社、組織、製品、ドメイン名、電子メールアドレス、ロゴ、人物、場所、イベントとの関連を意図したり推論したりするものではありません。適用されるすべての著作権法を遵守することは、ユーザーの責任です。著作権に基づく権利を制限することなく、マイクロソフトの書面による明示的な許可がない限り、本書のいかなる部分も、いかなる形式または手段 (電子的、機械的、複写、記録、その他) で、あるいはいかなる目的においても、複製、検索システムへの保存または導入、転送することを禁じます。

マイクロソフトは、本書の主題を対象とする特許、特許出願、商標、著作権、またはその他の知的財産権を有している場合があります。マイクロソフトの書面によるライセンス契約において明示的に規定されている場合を除き、本書の提供は、これらの特許、商標、著作権、またはその他の知的財産に対するいかなるライセンスもお客様に与えるものではありません。

メーカー名、製品名、URL は情報提供のみを目的としており、マイクロソフトは、これらのメーカーまたはマイクロソフトのテクノロジーによる製品の使用に関して、明示的、黙示的、法定的を問わず、いかなる表明および保証も行いません。メーカーや製品を掲載することは、マイクロソフトがそのメーカーや製品を推奨していることを意味するものではありません。また、第三者のサイトへのリンクが張られていることがあります。そのようなサイトはマイクロソフトの管理下にはなく、マイクロソフトは、リンク先のサイトのコンテンツ、リンク先のサイトに含まれるリンク、またはそのようなサイトの変更もしくは更新について、一切の責任を負いません。マイクロソフトは、リンク先サイトから受信するウェブキャスティングまたはその他の形式の伝送について責任を負いません。マイクロソフトは、これらのリンクをお客様の便宜のためにのみ提供しており、リンクを掲載することは、マイクロソフトが当該サイトまたはそこに含まれる製品を推奨していることを意味するものではありません。

© 2021 Microsoft Corporation. All rights reserved.

**目次**

- [ハンズオン環境構成ガイド](#ハンズオン環境構成ガイド)
  - [必要なもの](#必要なもの)
  - [ハンズオンの前に](#ハンズオンの前に)
    - [Task 1: リソースグループの作成](#task-1-リソースグループの作成)
    - [Task 2: ARMテンプレートを使ったハンズオンリソースのプロビジョニング](#task-2-ARMテンプレートを使ったハンズオンリソースのプロビジョニング)
    - [Task 3: Cosmos DBのFirewallにIPアドレスを追加](#task-3-Cosmos-DBのFirewallにIPアドレスを追加)
    - [Task 4: ハンズオンで利用するVMのデフォルトWebブラウザをMicrosoft Edgeに設定](#task-4-ハンズオンで利用するVMのデフォルトWebブラウザをMicrosoft-Edgeに設定)

# ハンズオン環境構成ガイド

## 必要なもの

- Microsoft Azureのサブスクリプション
- Office 365のアカウント。お持ちでないなら、 [トライアル環境のサインアップ](https://portal.office.com/Signup/MainSignup15.aspx?Dap=False&QuoteId=79a957e9-ad59-4d82-b787-a46955934171&ali=1) が可能です。  
- GitHubのアカウント。 [無料のアカウントを作成](https://github.com/join) できます。

## ハンズオンの前に

この演習では、ハンズオンで使う環境のセットアップをします。ハンズオン参加<b>前</b>に必ず以下に示す手順で環境を構成しておいてください (20分) 。

> **重要**: 多くのAzureリソースでは、グローバルで一意な名前を必要とします。これらの手順全体で、"SUFFIX"という文字がリソース名の中に出てきますが、このSUFFIXを使い、一意な名前のリソースになるようにしてください（例：社員番号、エイリアスなど）。

### Task 1: リソースグループの作成

1. [Azure portal](https://portal.azure.com) で、  **リソースグループ** をAzureサービスリストから選択します。

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. リソースグループ・ブレードで、 **+作成** を選択します。

   ![+Add is highlighted in the toolbar on Resource groups blade.](media/resource-groups-add.png "Resource groups")

3. リソースグループ作成の **基本** タブで以下の項目を入力します。

   - **サブスクリプション**: このハンズオンで利用するサブスクリプションを選択
   - **リソースグループ**: 新規作成するリソースグループとして `hands-on-lab-SUFFIX` を入力
   - **リージョン**: このハンズオンｄ選りようするリージョンを選択

   ![The values specified above are entered into the Create a resource group Basics tab.](media/create-resource-group.png "Create resource group")

4. **確認および作成**を選択します。

5. **確認および作成**タブで、検証完了のメッセージが出たことを確認してから、 **作成** を選択します。

### Task 2: ARMテンプレートを使ったハンズオンリソースのプロビジョニング

このタスクでは、Azure Resource Manager (ARM) テンプレートを実行して、ハンズオンのリソースを作成します。リソース作成に加えて、ARMテンプレートはPowerShellスクリプトを `LabVM` で実行し、ソフトウェアのインストールとサーバーの構成を実施します。ARMテンプレートが作成するりソースは以下の通りです。

- Azure Data Lake Storage Gen2アカウント
  - BlobおよびFileサービス
  - `images` と `export` という名前のコンテナー
- Azure Cosmos DB
  - `LicensePlates` という名前のデータベース
  - `Processed` と `NeedsManualReview` という名前のコンテナー
- `default` サブネットを含む仮想ネットワーク (VNet)
- Visual Studio 2019 Community Edition イメージを使った仮想マシン
  - カスタムスクリプト拡張を使って以下を実行しています
    - Microsoft Edgeブラウザーのインストール
    - Serverless architecture MCW GitHubリポジトリからのスターターソリューションのダウンロード
    - **IE Enhanced Security Configuration** の無効化
- VM用ネットワークセキュリティグループ
- VM用ネットワークインターフェース (NIC)
- VM用パブリックIPアドレス
- Azure Function Apps
  - `TollBoothFunctions`
  - `TollBoothEvents`
- Application Insights
- Azure Computer Visionサービス
- Azure Event Gridトピック
- Azure Logic App
- Azure Key Vaultおよび以下のシークレット
  - `computerVisionApiKey`
  - `cosmosDBAuthorizationKey`
  - `dataLakeConnectionString`
  - `eventGridTopicKey`

> **注意**: [手作業によるリソースデプロイ・セットアップガイド](Manual-resource-setup.md)の手作業でのプロビジョニング手順を確認し、ハンズオンリソースを手作業で構成することもできます。

1. ARMテンプレートのデプロイ開始の準備ができたら、Azure Portalのカスタムデプロイ画面を開き、以下のDeploy to Azureのボタンを選択します。

   [![Deploy to azure](media/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2FMCW-Serverless-architecture%2Fmain%2FHands-on%20lab%2Flab-files%2Farm-template%2Fazure-deploy.json)

2. カスタムデプロイ画面で、最初の入力パラメータはAzure Portalにログインするために利用しているアカウントに紐付く `ObjectId` です。この値を確認するために、Azure Portalツールバーの **Cloud Shell** アイコンを選択します。すると、ブラウザ画面の下部にAzure Command Line Interface (CLI) ターミナル画面が現れます。

   ![The Cloud Shell icon is highlighted on the Azure portal toolbar.](media/azure-toolbar-cloud-shell.png "Azure toolbar")

3. Azure PortalでPowerShellターミナル画面が開いたら、以下のコマンドを入力します。

   ```powershell
   az ad signed-in-user show --query id -o tsv
   ```

   ![At the cloud shell prompt, the az ad signed-in-user show command is entered and highlighted.](media/azure-cli-az-ad-signed-in-user-show.png "Azure CLI")

4. コマンドを実行し、出力結果をコピーします。

   ![In the cloud shell, the output from the az ad signed-in-user show command is highlighted.](media/azure-cli-az-ad-signed-in-user-show-output.png "Azure CLI")

5. ここまでできたら、Azure Portalのカスタムデプロイ画面にて以下の内容を入力します。

   - **Subscription**: このハンズオンで使うサブスクリプションを選択します。
   - **Resource group**: hands-on-lab-SUFFIXリソースグループをドロップダウンリストから選択します。
   - **Signed In User Object Id**: 先ほどCloud Shellターミナルからコピーした値を貼り付けます。
   - **Region**: Japan Eastを選択します。
   - **Vm Username**: デフォルト値 **demouser** のままでかまいません。
   - **Vm Password**: デフォルト値 **Password.1!!** のままでかまいません。

   ![The Custom deployment blade is displayed, and the information above is entered on the Custom deployment blade.](media/azure-custom-deployment.png "Custom deployment blade")

6. **確認および作成**を選択して、カスタムデプロイを最終確認します。

   > **注意**: ARMテンプレートはリソース名の後ろにハイフンに続く13桁の文字列を追加します。この文字列はリソース名のグローバルでの一意性を保証するために付加しています。このハンズオンでは、リソース参照時には、この付加される文字列を無視してください。

7. 確認および作成の画面で、 _検証が完了しました_ のメッセージを確認してから、**作成**を選択してカスタムデプロイを開始します。

   > **注意**: カスタムARMテンプレートのデプロイにはおよそ5分ほどかかります。

   ![On the Review + create blade for the custom deployment, the Validation passed message is highlighted, and the Create button is highlighted.](media/azure-custom-deployment-review-create.png "Review + create custom deployment")

8. ARMテンプレートのデプロイを開始した際に開く **デプロイ** 画面でデプロイの進捗を確認できます。blade that opens when you start the ARM template deployment.

### Task 3: Cosmos DBのFirewallにIPアドレスを追加

1. [Azure portal](https://portal.azure.com) で、先ほど作成した **hands-on-lab-SUFFIX** リソースグループに移動します。

   > Azure Portalのホームページで、 **Azureサービス** の下にある **リソースグループ** を選択し、表示されるリストから選択しても、リソースグループに到達できます。Azureアカウントに多数のリソースグループがある場合、 **hands-on-lab** でフィルタすれば、表示されているリソースグループを減らすことができます。

2. リソースグループの画面で、サービスが利用可能なリソースグループのリストにある **cosmosdb** Azure Cosmos DBアカウントリソースを選択します。

   ![The Azure Cosmos DB account resource is highlighted in the list of services in the resource group.](media/resource-group-cosmos-db-account.png "Resources")

3. 続いて、Cosmos DBの画面の左側にあるナビゲーションメニューから、 **ファイアウォールと仮想ネットワーク** を選択します。

4. **選択したネットワーク (2)** を選択し、 **+ 現在のIPを追加する (3)** を選択して、ファイアウォールの下にあるIPリストにご利用環境のPublic IPアドレスを追加します。その後、 **パブリック Azure データセンター内からの接続を受け入れる (4)** の隣のチェックボックスを選択してください。このチェックボックスを選択すると、Function AppsなどのAzureサービスがAzure Cosmos DBアカウントにアクセスできるようになります。

    ![The checkbox is highlighted.](media/cosmos-db-firewall.png "Firewall and virtual networks")

5. **保存** を選択します。

### Task 4: ハンズオンで利用するVMのデフォルトWebブラウザをMicrosoft Edgeに設定

このタスクでは、ハンズオンで利用する仮想マシン (VM) へのRDP接続を作成し、デフォルトのWebブラウザをMicrosoft Edgeに変更します。この作業により、Visual StudioからWebブラウザを立ち上げた際にMicrosoft Edgeが開き、Internet Explorerを使った場合に発生する機能上の問題を回避できます。

1. [Azure portal](https://portal.azure.com) で、Azureサービスリストから **リソースグループ** を選択します。

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. **hands-on-lab-SUFFIX** リソースグループをリストから選択します。

   ![The "hands-on-lab-SUFFIX" resource group is highlighted.](media/resource-groups.png "Resource groups list")

3. リソースグループ内のリソースリストで、 **LabVM 仮想マシン** リソースを選択します。

   ![The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and LabVM is highlighted.](media/resource-group-resources-labvm.png "LabVM in resource group list")

4. LabVM 画面で、上部のメニューから **接続** > **RDP** を選択します。

   ![The LabVM blade is displayed, with the Connect button highlighted in the top menu.](media/connect-vm-rdp.png "Connect to Lab VM")

5. 仮想マシンへの接続画面で、 **RDPファイルのダウンロード** を選択します。ダウンロードされたRDPファイルを開きます。

   ![The Connect to virtual machine blade is displayed, and the Download RDP File button is highlighted.](media/connect-to-virtual-machine.png "Connect to virtual machine")

6. リモートデスクトップ接続の画面で、 **接続** を選択します。

   ![In the Remote Desktop Connection Dialog Box, the Connect button is highlighted.](media/remote-desktop-connection.png "Remote Desktop Connection dialog")

7. 以下の資格証明を入力し、 **OK** を選択します。

   - **Username**: demouser
   - **Password**: Password.1!!

   ![The credentials specified above are entered into the Enter your credentials dialog.](media/rdc-credentials.png "Enter your credentials")

8. リモートコンピューターのIDを検証できない旨の表示が現れた場合には、 **Yes** をクリックします。

   ![In the Remote Desktop Connection dialog box, a warning states that the remote computer's identity cannot be verified and asks if you want to continue anyway. At the bottom, the Yes button is highlighted.](media/remote-desktop-connection-identity-verification-labvm.png "Remote Desktop Connection dialog")

9. ログインした後、「スタート」バー **Search** アイコンを選択し、 **default apps** を検索ボックスに入力し、検索結果の **Default apps** を選択します。

    ![The search icon is highlighted on the Windows start bar. In the search dialog, "default apps" is entered into the search box and highlighted. In the search results, Default apps is highlighted.](media/search-default-apps.png "Windows Search")

10. Default apps画面で、**Web browser**の下の**Internet Explorer**を選択します。

    ![In the Default apps dialog, Internet Explorer is highlighted under Web browser.](media/default-apps-web-browser.png "Default apps")

11. **Choose an app**画面で、**Microsoft Edge**を選択します。

    ![In the Choose an App dialog, Microsoft Edge is highlighted.](media/default-apps-web-browser-choose-an-app.png "Choose an app")

12. **Default apps** 画面を閉じます。

ハンズオン*前*に実施すべきすべての作業を実施しておいてください。

> **Microsoft Edgeへの切り替え時に選択できない場合**<br>
> Internet Explorerを開くと、Microsoft Edgeのダウンロード画面が表示されます。<br>その際に、**Windows Server 2019**用のEdgeを選択してインストールしてください。