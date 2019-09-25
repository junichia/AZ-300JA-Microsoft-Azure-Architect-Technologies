﻿# Azure サブスクリプションとリソースの管理

# ラボ：Azure の監視機能の探索

### シナリオ

アダタムコーポレーションは、Azure の監視機能を探索したいと考えています。

### 目的

このラボを終了すると、下記ができるようになります：

- Azure VM スケール セットのデプロイ

- Azure Monitorを使用して監視と警告を執行します

### ラボのセットアップ

推定時間：45 分間

ユーザー名：**受講者**

パスワード：**Pa55w.rd**

# エクササイズ 1：Azure VM スケール セットのデプロイ

このエクササイズの主なタスクは次のとおりです：

1. Azure クイック スタート テンプレートを使用して Azure VM スケール セットをデプロイします

1. Azure VM スケール セットの自動スケール設定を確認します

#### タスク 1：Azure クイック スタート テンプレートを使用して Azure VM スケール セットをデプロイします

1. ラボの仮想マシンから Microsoft Edge を起動し、http://portal.azure.com　で Azure portalを参照し、ターゲット Azure サブスクリプションの所有者ロールを持つ Microsoft アカウントを使用してサインインします。

1. Azure portalに、Microsoft Edge ウィンドウに、Cloud Shell内で **PowerShell** セッションを開始します。

1. **マウントされたストレージがない** メッセージが表示された場合 は、**高級設定を表示** をクリック し、次の設定を使用してストレージを構成します。

   - サブシプション： ターゲット Azure サブスクリプションの名前

   - Cloud Shell リージョン： サブスクリプションに使用でき、ラボの場所に最も近い Azure リージョンの名前

   - リソース グループ： 新しいリソース グループ **az3000100-LabRG** の名前

   - ストレージ アカウント： 新しいストレージ アカウントの名前 (小文字と数字で構成される 3-24 文字)

   - ファイル共有： 新しいファイル共有の名前： **Cloud Shell**

1. Cloud Shellペインから、次のコマンドを実行して特別な DNS ドメイン名を識別します (文字で始まり、9 文字以下の如何なる英文字・数字からある特別な文字列を有するプレースホルダ`<custom-label>` と、Azure リージョンの名前を有するプレースホルダ`<location>`をこのラボでリソースのデプロイのために置き換えます）：

   ```pwsh
   Test-AzDnsAvailability -DomainNameLabel <custom-label> -Location '<location>'
   ```

1. コマンドが **True** へ戻したことを確認します。そうでない場合は、コマンドが **True** へ戻すまで、`<custom-label>`の異なる値で同じコマンドを再実行します。

1. 成功した結果をもたらした`<custom-label>`の値に注意してください。次のタスクで必要になります。

1. ラボの仮想マシンから Microsoft Edge を起動し、[**https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale**](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale)で　Ubuntu 16.04 に自動スケール デモ アプリをデプロイする Azure クイック スタート テンプレートを参照します。

1. **Azure へデプロイ** をクリックし、プロンプトが表示された場合は、ターゲット Azure サブスクリプションの所有者ロールを持つ Microsoft アカウントを使用してサインインします。

1. Azure Portal　では、**Python ボトル サーバーとオートスケールを使用した VM スケール セットのデプロイ** ブレードで、次の設定を指定し、デプロイを開始します：

   - サブスクリプション：ターゲット　Azure　サブスクリプションの名前

   - リソース グループ： 新しいリソース グループ **az3000101-LabRG** の名前

   - 場所： このタスクの初期に `Test-AzDnsAvailability` を実行したときに参照した Azure リージョンの名前

   - Vm Sku：**Standard_D1_v2**

   - Vmss　名： このタスクの初期に `Test-AzDnsAvailability` を実行したときに識別したカスタム ラベル

   - インスタンス数：**1**

   - 管理者ユーザー名：**受講者**

   - 管理者のパスワード：**Pa55w.rd1234**

1. **上記の条項と条件に同意します** の隣にチェックマークを置き、**購入** を クリックします。

1. デプロイメントが完了するのを待ってください。これにはおよそ5分がかかります。

#### タスク 2：Azure VM スケール セットの自動スケール設定を確認します

1. Azure Portal　で、新しくデプロイされた **Azure VM スケール セット** を表すブレードに移動します。

1. VM スケール セット ブレードから、その **スケーリング** ブレードに移動します。

1. Azure VM スケール セットは、次の標準を使用して、メトリックに基づいて動的にスケーリングするように構成されています。

   - スケールアウト：CPU の平均パーセンテージが　60　以上の場合、インスタンス数を 1 増やします

   - スケールイン：CPU の平均パーセンテージが　30　以下の場合、インスタンス数を 1 減らします

   - インスタンスの最小数：1

   - インスタンスの最大数：10

1. インスタンスの最大数を 3 に変更し、変更内容を **保存** します。

> **結果**：このエクササイズを完了した後、Azure VM スケール セットをデプロイし、その自動スケール設定を確認しました。

## エクササイズ 2：Azure Monitorを使用した監視と警告の執行

このエクササイズの主なタスクは次のとおりです：

1. Azure VM スケール セット メトリックに基づく警告の作成

1. Azure VM スケール セットの自動スケーリングに基づく通知の構成

1. Azure VM スケール セットの監視と警告をテストします。

#### タスク 1：Azure VM スケール セット メトリックに基づく警告の作成

1. Azure portal　で、新しくデプロイされた Azure VM スケール セットを表すブレードに移動し、そこから **監視 - メトリック** ブレードに切り替えます。

1. **監視 - メトリック** ブレードで、フィルタを使用して、このラボの前のエクササイズにプロビジョンしたVM スケール セット リソースの **Avg** アグリゲーション値を持つ **パーセンテージ CPU** メトリックを表示します。

1. 結果のチャートを確認し、最後の数分間以内の平均パーセンテージ CPU をメモします。

1. **監視** セクションの **アラート** ブレードに移動します。

1. **アラート** ブレードから **管理のアクション** ブレードに移動します。

1. **管理のアクション** のセクションで、**アクション グループの追加** をクリックし、アクショングループ名を **az30001 アクション グループ** に設定し、**az30001** という短い名前を設定します。前のエクササイズで使用した Azure サブスクリプションを選択し、**デフォルト-ActivityLogAlerts(作成予定)** というリソースグループのデフォルト名前を受け、**az30001-email** というアクション名を設定します。また、アクションタイプを **メール/SMS/プッシュ/音声** に設定します。 

1. **メール/SMS/プッシュ/音声** ブレードで、このルールによって生成されたアラートを受信するために使用したいメールアドレス、携帯電話番号、または電話番号を設定します。

1. 「アクション グループの追加」ページで、**OK** をクリックします。

1. **アラート** ブレードに移動します。

1. **アラート** ブレードから、**新しいアラート ルール** ブレードまで移動します。
1. **リソース** セクションで、このラボの前のエクササイズにプロビジョンした VM スケール セットを選択します。

1. **条件** セクションで、**条件の追加** をクリックし、**パーセンテージCPU** メトリックを選択し、ディメンション設定と条件タイプを既定値のままにして、条件を **より大きい値** に設定し、時間の集合を **平均** に設定し、しきい値を **60** に設定し、集合の粒度 (期間) を **1 分間** に設定し、頻度を **1 分ごとに** に設定 した後、**完了** をクリック します。

1. **アラートの詳細** セクションで、アラート ルール名を **VM スケール セットのパーセンテージ CPU が 60% を超え** に設定し、その説明を **VM スケール セットのパーセンテージ CPU　が　60% を超えます** に設定し、その重大度を **Sev 3** に設定して、作成時にルールを **有効** にします。

1. **アラートルールの作成** をクリックします。

   > **注記**：メトリック アラート ルールがアクティブになるには、最大　10　分間かかる場合があります。 

#### タスク 2：Azure VM スケール セットの自動スケーリングに基づく通知の構成

1. Azure portalで、新しくデプロイされた Azure VM スケール セットを表すブレードに移動し、そこから **スケーリング** ブレードに切り替えます。

1. **スケーリング** ブレードで、**通知** タブの見出しをクリックし、次の設定を構成し、変更内容を **保存** します。

   - メール管理者： 有効

   - メール共同管理者： 無効

   - 付加の管理者メール： 自動スケーリング イベントに関する通知を受け取るために使用するメールアドレスを追加します

#### タスク 3：Azure VM スケール セットの監視と警告をテストします。

1. Azure portal　で、**ロード バランサー** を検索し、ロード バランサー ブレードに移動します。ロード バランサーは存在する必要があります。このラボの前のエクササイズでテンプレートからデプロイしたスケール セットで作成されたものを代表します。

1. VM スケール セットと関連があるロード バランサーのフロントエンドに割り当てられた **パブリック IP アドレス** の値を識別します。

1. ラボ コンピュータから Microsoft Edge を起動し、**http://*Public IP address\*:9000** を参照してください（**_Public IP address_** は前のステップに識別した IP　アドレスです）。

1. **作業者インターフェイス** ページで、**作業の開始** リンクをクリック します。

1. 「VM スケール セットの概要」ブレードの **CPU (平均)** チャートを使用して、CPU 使用率の変更を監視します。

   > **注記**：または、**メトリック** ブレードに戻り、フィルターを使用して VM スケール セット リソースの **パーセンテージ　CPU** メトリックを表示 し、時間を **最後の30分間** に設定することもできます。

   > **注記**：数分間以内に CPU 使用率の向上に関するアラートを受信する必要があります。

1. インスタンスの数を識別するために、VM スケール セットの **インスタンス** ブレードに切り替えます。

   > **注記**：または、自動スケールが可能なリソースの一覧に **スケーリング** ブレードに戻り、VM スケール セットの名前をクリックし、**自動スケール設定** ブレードで **履歴の実行** をクリック し、自動スケール イベントの一覧を確認することもできます。

   > **注記**：自動スケールは数分間以内にトリガーする必要があります。

1. ワーカー インスタンスページを表示する Microsoft Edge ウィンドウに切り替え、**作業の停止** リンクをクリックします。

1. VM スケール セットのスケーリング アウト時に使用したのと同じ方法を使い、CPU 使用率の低下とイベントのスケーリングを監視します。

> **結果**：このエクササイズを完了した後、Azure Monitor を使用して監視と警告を執行し、テストしました。

## エクササイズ 3：ラボ リソースの削除

### タスク 1：リソース グループの削除

1. Portal　の上部にある **Cloud Shell** アイコンをクリックして、Cloud Shell　ペインを開きます。

1. **Cloud Shell** コマンド プロンプトに下記のコマンドを入力し、**Enter** キーを押して、このラボで作成したすべてのリソース グループを一覧表示します。

   ```
   az group list --query "[?starts_with(name,'az300010')].[name]" --output tsv
   ```

1. 出力に、このラボで作成したリソース グループのみが含まれていることを確認します。これらのグループは、次のタスクで削除されます。

#### タスク 2：リソース グループの削除

1. **Cloud Shell** コマンド プロンプトに下記のコマンドを入力し、**Enter** キーを押してこのラボで作成したリソース グループを削除します。

   ```
   az group list --query "[?starts_with(name,'az300010')].[name]" --output tsv | %{az group delete -n $_ --no-wait -y}
   ```

1. Portal　の下部にある **Cloud Shell** プロンプトを閉じます。

> **結果**：このエクササイズを完了した後、このラボで使用するリソースを削除しました。