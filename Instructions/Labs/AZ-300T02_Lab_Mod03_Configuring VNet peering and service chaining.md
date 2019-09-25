﻿# 仮想ネットワークの作成と管理
# ラボ: VNet ピアリングとサービス チェーンの構成
  
### シナリオ
  
ADatum コーポレーションは、Azure サブスクリプションに Azure 仮想ネットワーク間のサービス チェーンを実装したいと考えています。 


### 目的
  
このラボを終了すると、次のことができるようになります:

- Azure リソース マネージャー テンプレートを使用して Azure VM をデプロイします。

- VNet ピアリングの構成

- ルーティングの実装

- サービス チェーンの検証


### ラボの設定
  
推定時間: 45 分間

ユーザー名: **受講者**

パスワード: **Pa55w.rd**



## エクササイズ 1: デプロイメント テンプレートを使用した Azure ラボ環境の作成
  
このエクササイズの主なタスクは次のとおりです。

1. Azure リソース マネージャー テンプレートを使用して最初の Azure 仮想ネットワーク環境を作成します

1. Azure リソース マネージャー テンプレートを使用して2 番目の Azure 仮想ネットワーク環境を作成します


#### タスク 1: Azure リソース マネージャー テンプレートを使用して最初の Azure 仮想ネットワーク環境を作成します
  
1. ラボの仮想マシンから Microsoft Edge を起動し、[**http://portal.azure.com**](http://portal.azure.com) に Azure ポータルを参照 し、ターゲット Azure サブスクリプションの所有者ロールを持つ Microsoft アカウントを使用してサインインします。

1. Azure portal には、Microsoft Edge ウィンドウに、**Cloud Shell** 内の **Bash** セッションを開始します。 

1. **ストレージにマウントされたメッセージがない** というメッセージが表示された場合は、次の設定を使用してストレージを構成します。

    - サブシプション: ターゲット Azure サブスクリプションの名前

    - Cloud Shellリージョン: サブスクリプションで使用でき、ラボの場所に最も近い Azure リージョンの名前

    - リソース グループ: 新しいリソース グループ **az3000400-LabRG** の名前

    - ストレージ アカウント: 新しいストレージ アカウントの名前

    - ファイル共有: 新しいファイル共有の名前

1. Cloud Shell ペインから、下記を実行して2つのリソース グループを作成します (`<Azure region>`プレースホルダを、サブスクリプションに使用でき、ラボの場所に最も近い Azure リージョンの名前に置き換えます)。

   ```
   az group create --resource-group az3000401-LabRG --location <Azure region>
   az group create --resource-group az3000402-LabRG --location <Azure region>
   ```

1. Cloud Shell ペインから、最初の Azure リソース マネージャー テンプレート **\allfile\AZ-300T02\Module_03\azuredeploy0401.json** をホーム ディレクトリにアップロードします。

1. Cloud Shell ペインから、パラメータ ファイル **\\allfile\\AZ-300T02\\Module_03\\azuredeploy04.parameters.json** をホーム ディレクトリにアップロードします。

1. Cloud Shell ペインから、下記を実行して、Windows Server 2016 データセンターをホストする 2 つの Azure VM を最初の仮想ネットワークにデプロイします。

   ```
   az group deployment create --resource-group az3000401-LabRG --template-file azuredeploy0401.json --parameters @azuredeploy04.parameters.json
   ```

    > **注意**: デプロイメントが完了するのを待たず、次のタスクに進みます。


#### タスク 1: Azure リソース マネージャー テンプレートを使用して2 番目の Azure 仮想ネットワーク環境を作成します

1. Cloud Shell ペインから、2 番目の Azure リソース マネージャー テンプレート **\allfile\AZ-300T02\Module_03\azuredeploy0402.json** をホーム ディレクトリにアップロードします。

1. Cloud Shell ペインから、下記を実行して、Windows Server 2016 データセンターをホストする Azure VM を 2 番目の仮想ネットワークにデプロイします。

   ```
   az group deployment create --resource-group az3000402-LabRG --template-file azuredeploy0402.json --parameters @azuredeploy04.parameters.json
   ```

    > **注意**: 2 番目のテンプレートは、同じパラメータ ファイルを使用します。 

    > **注意**: デプロイメントが完了するのを待たず、次のエクササイズに進みます。

> **結果**: このエクササイズを完了したら、Windows Server 2016 データセンターを実行する Azure VM をホストする 2 つの Azure 仮想ネットワークを作成する必要があります。 


## エクササイズ 2: VNet ピアリングの構成 
  
このエクササイズのタスクは次のとおりです。

1. 両方の仮想ネットワークの VNet ピアリングを構成します

#### タスク 1: 両方の仮想ネットワークの VNet ピアリングを構成します
  
1. Azure portal を表示する Microsoft Edge ウィンドウに、**az3000401-vnet** 仮想ネットワーク ブレードに移動します。

1. **az3000401-vnet** ブレードから、次の設定で VNet ピアリングを作成します。

    - 最初の仮想ネットワークから 2 番目の仮想ネットワークへのピアリングの名前: **az3000401-vnet-to-az3000402-vnet**

    - 仮想ネットワーク デプロイメント モデル: **リソース マネージャー**

    - サブスクリプション: このラボのために使用するAzureサブスクリプションの名前

    - 仮想ネットワーク: **az3000402-vnet**
    
    - 最初の仮想ネットワークから 2 番目の仮想ネットワークへのピアリングの名前: **az3000402-vnet-to-az3000401-vnet**    
    
    - 最初の仮想ネットワークから 2 番目の仮想ネットワークへの仮想ネットワーク アクセスを許可します。**有効にする**
    
    - 2 番目の仮想ネットワークから最初の仮想ネットワークへの仮想ネットワーク アクセスを許可します。**有効にする**    

    - 最初の仮想ネットワークから 2 番目の仮想ネットワークへの転送トラフィックを許可します。**無効にする**
    
    - 2 番目の仮想ネットワークから最初の仮想ネットワークへの転送トラフィックを許可します。**無効にする**

    - ゲートウェイ トランジットの許可: 無効

## エクササイズ 3: ルーティングの実装
  
このエクササイズの主なタスクは次のとおりです。

1. IP 転送を有効にします

1. ユーザー定義ルーティングの構成 

1. Windows Server 2016 を実行している Azure VM にルーティングを構成します


#### タスク 1: IP 転送を有効にします 
  
1. Microsoft Edge に **az3000401-nic2** ブレード (**az3000401-vm2** の NIC)に移動します。

1. **az3000401-nic2** ブレードに、**IP 転送** を **有効** に設定して **IP 構成** を変更します。


#### タスク 2: ユーザー定義ルーティングの構成 

1. Azure portalから、次の設定をもって新しいルート テーブルを作成します:

    - 名前: **az3000402-rt1**

    - サブスクリプション: このラボのために使用するAzureサブスクリプションの名前

    - リソース グループ: **az3000402-LabRG**

    - 場所: 仮想ネットワークを作成したのと同じ Azure リージョン
  
    - BGP ルート普及: **無効にする**

1. Azure portal に、次の設定でルートをルート テーブルに追加します。 

    - ルルート名: **custom-route-to-az3000401-vnet**

    - アドレス プレフィックス: **10.0.0.0/22**

    - 次のホップの種類: **仮想アプライアンス**

    - 次のホップのアドレス: **10.0.1.4**

1. Azure portal に、ルート テーブルを **az3000402-vnet** の **サブネット-1** に関連付けます。


#### タスク 3: Windows Server 2016 を実行している Azure VM にルーティングを構成します

1. ラボ コンピュータに、Azure portal から **az3000401-vm2** Azure VM にリモート デスクトップ セッションを開始します。 

1. 認証を求める場合は、次の資格情報を指定します。

    - ユーザー名: **受講者**

    - パスワード: **Pa55w.rd1234**

1. **サーバー マネージャ** からリモート デスクトップ セッションを通して az3000401-vm2 に接続されたら、**ルーティング** ロール サービスとすべての必要な機能を使用して **リモート アクセス** サーバーのロールをインストールします。 

1. az3000401-vm2 へのリモート デスクトップ セッションに、 **ルーティングおよびリモート アクセス** コンソールを起動します。 

1. **ルーティングおよびリモート アクセス** コンソールに、**ルーティングおよびリモート アクセス サーバー設定ウィザード** を実行し、**LAN ルーティング** を有効にします。 

1. **ルーティングサービスとリモート アクセス** サービスを開始します。

1. az3000401-vm2 へのリモート デスクトップ セッションに、**高度なセキュリティ コンソールを使用して Windows ファイアウォール** を起動し、すべてのプロファイルに対して **ファイルとプリンタの共有 (エコー要求 - ICMPv4-In)** の受信ルールを有効にします。

> **結果**: ここのエクササイズを完了したら、2 番目の仮想ネットワーク内にカスタム ルーティングを構成する必要があります。


## エクササイズ 4: サービス チェーンの検証
  
このエクササイズの主なタスクは次のとおりです。

1. Azure VM に高度なセキュリティを使用して Windows ファイアウォールを構成します。

1. ピアリングされた仮想ネットワーク間のサービス チェーンのテストします


#### タスク 1: ターゲット Azure VM に高度なセキュリティを使用して Windows ファイアウォールを構成します。
  
1. ラボ コンピュータに、Azure portal から **az3000401-vm1** Azure VM にリモート デスクトップ セッションを開始します。 

1. 認証を求める場合は、次の資格情報を指定します。

    - ユーザー名: **受講者**

    - パスワード: **Pa55w.rd1234**

1. az3000401-vm1 へのリモート デスクトップ セッションに、**高度なセキュリティ コンソールを使用して Windows ファイアウォール** を起動し、すべてのプロファイルに対して **ファイルとプリンタの共有 (エコー要求 - ICMPv4-In)** の受信ルールを有効にします。


#### タスク 2: ピアリングされた仮想ネットワーク間のサービス チェーンのテストします
  
1. ラボ コンピュータに、Azure portal から **az3000402-vm1** Azure VM にリモート デスクトップ セッションを開始します。 

1. 認証を求める場合は、次の資格情報を指定します。

    - ユーザー名: **受講者**

    - パスワード: **Pa55w.rd1234**

1. リモート デスクトップ セッションを通して az3000402-vm1 に接続されたら、**Windows PowerShell** を起動します。

1. **Windows PowerShell** ウィンドウに、次を実行します。

   ```pwsh
   Test-NetConnection -ComputerName 10.0.0.4 -TraceRoute
   ```

1. テストが成功したことを確認し、接続が 10.0.1.4 を超えてルーティングされたことを注意します。


>  **結果**: このエクササイズを完了したら、ピアリングされた仮想ネットワーク間のサービス チェーンを検証する必要があります。