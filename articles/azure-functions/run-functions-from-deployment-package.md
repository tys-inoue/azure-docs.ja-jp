---
title: Azure Functions をパッケージから実行する
description: 関数アプリのプロジェクト ファイルが含まれたデプロイ パッケージ ファイルをマウントすることで、Azure Functions ランタイムで関数を実行します。
ms.topic: conceptual
ms.date: 07/15/2019
ms.openlocfilehash: a3e11a7c4f3fd91df2fd9dd7a44f3922c4922585
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921115"
---
# <a name="run-your-azure-functions-from-a-package-file"></a>Azure Functions をパッケージ ファイルから実行する

Azure では、関数アプリのデプロイ パッケージ ファイルから、関数を直接実行できます。 他のオプションは、関数アプリの `d:\home\site\wwwroot` ディレクトリ内のファイルをデプロイすることです。

この記事では、パッケージから関数を実行するメリットについて説明します。 関数アプリでこの機能を有効にする方法も示します。

> [!IMPORTANT]
> [Premium プラン](functions-scale.md#premium-plan)で Linux 関数アプリに関数をデプロイするときは、常にパッケージ ファイルから実行し、[Azure Functions Core Tools を使用してアプリを発行](functions-run-local.md#project-file-deployment)する必要があります。

## <a name="benefits-of-running-from-a-package-file"></a>パッケージ ファイルから実行することのメリット
  
パッケージ ファイルからの実行には、さまざまなメリットがあります。

+ ファイル コピー ロック問題のリスクを軽減します。
+ 運用環境のアプリにデプロイできます (再起動が必要です)。
+ アプリで実行されるファイルを確定できます。
+ [Azure Resource Manager デプロイ](functions-infrastructure-as-code.md)のパフォーマンスが向上します。
+ 特に大規模な npm パッケージのツリーの JavaScript 関数の場合、コールド スタート時間を減らすことができます。

詳細については、こちらの[お知らせ](https://github.com/Azure/app-service-announcements/issues/84)をご覧ください。

## <a name="enabling-functions-to-run-from-a-package"></a>パッケージから関数を実行できるようにする

関数アプリをパッケージから実行できるようにするには、`WEBSITE_RUN_FROM_PACKAGE` 設定を関数アプリの設定に追加するだけです。 `WEBSITE_RUN_FROM_PACKAGE` 設定には次のいずれかの値を指定できます。

| Value  | 説明  |
|---------|---------|
| **`1`**  | Windows 上で実行される関数アプリでは、推奨されています。 関数アプリの `d:\home\data\SitePackages` フォルダー内のパッケージ ファイルから実行します。 [Zip 配置を使って配置](#integration-with-zip-deployment)しない場合、このオプションでは、フォルダー内に `packagename.txt` という名前のファイルも含む必要があります。 このファイルには、フォルダー内のパッケージ ファイルの空白なしの名前のみが含まれます。 |
|**`<URL>`**  | 実行する特定のパッケージ ファイルの場所。 Blob Storage を使用する場合は、[Shared Access Signature (SAS)](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) 付きのプライベート コンテナーを使用して、Functions ランタイムがパッケージにアクセスできるようにする必要があります。 [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) を使用して、パッケージ ファイルを Blob Storage アカウントにアップロードします。 また、URL を指定するとき、更新済みのパッケージを発行した後に[トリガーを同期](functions-deployment-technologies.md#trigger-syncing)する必要もあります。 |

> [!CAUTION]
> Windows 上で関数アプリを実行する場合、外部 URL のオプションではコールド スタートのパフォーマンスが悪化します。 関数アプリを Windows にデプロイするときは、`WEBSITE_RUN_FROM_PACKAGE` を `1` に設定して、zip 配置によって発行する必要があります。

Azure Blob torage でホストされている .zip ファイルから実行するように構成された関数アプリを次に示します。

![WEBSITE_RUN_FROM_ZIP アプリの設定](./media/run-functions-from-deployment-package/run-from-zip-app-setting-portal.png)

> [!NOTE]
> 現時点では、.zip パッケージ ファイルのみがサポートされています。

## <a name="integration-with-zip-deployment"></a>zip デプロイとの統合

[Zip デプロイ][Zip deployment for Azure Functions]は、関数アプリ プロジェクトを `wwwroot` ディレクトリに配置することを可能にする Azure App Service の機能です。 プロジェクトは、.zip デプロイ ファイルとしてパッケージ化されます。 同じ API を使用して、パッケージを `d:\home\data\SitePackages` フォルダーに配置できます。 `WEBSITE_RUN_FROM_PACKAGE` アプリ設定が値 `1` の場合、zip デプロイ API は、ファイルを `d:\home\site\wwwroot` 抽出する代わりに、パッケージを `d:\home\data\SitePackages` フォルダーにコピーします。 それは、`packagename.txt` ファイルも作成します。 再起動後、パッケージは読み取り専用のファイルシステムとして `wwwroot` にマウントされます。 zip デプロイの詳細については、[Azure Functions の zip デプロイ](deployment-zip-push.md)に関する記事を参照してください。

## <a name="adding-the-website_run_from_package-setting"></a>WEBSITE_RUN_FROM_PACKAGE 設定の追加

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

### <a name="use-key-vault-references"></a>Key Vault 参照を使用する

セキュリティを強化するために、外部 URL と共に Key Vault 参照を使用できます。 これにより、URL は保存時に暗号化され、Key Vault を利用してシークレットの管理とローテーションを行うことができます。 関連する SAS キーを簡単にローテーションできるように、Azure Blob Storage を使用することをお勧めします。 Azure Blob Storage は保存時に暗号化されます。これにより、App Service にデプロイされていない場合でもアプリケーションのデータを安全に保つことができます。

1. Azure Key Vault を作成します。

    ```azurecli
    az keyvault create --name "Contoso-Vault" --resource-group <group-name> --location eastus
    ```

1. Key Vault に、外部 URL をシークレットとして追加します。

    ```azurecli
    az keyvault secret set --vault-name "Contoso-Vault" --name "external-url" --value "<insert-your-URL>"
    ```

1. `WEBSITE_RUN_FROM_PACKAGE` アプリ設定を作成し、その値を外部 URL への Key Vault 参照として設定します。

    ```azurecli
    az webapp config appsettings set --settings WEBSITE_RUN_FROM_PACKAGE="@Microsoft.KeyVault(SecretUri=https://Contoso-Vault.vault.azure.net/secrets/external-url/<secret-version>"
    ```

詳細については、次の記事を参照してください。

- [App Service の Key Vault 参照](../app-service/app-service-key-vault-references.md)
- [保存データに対する Azure Storage 暗号化](../storage/common/storage-service-encryption.md)

## <a name="troubleshooting"></a>トラブルシューティング

- パッケージから実行すると `wwwroot` が読み取り専用になるので、このディレクトリにファイルを書き込むときにエラーを受け取ります。
- tar および gzip 形式はサポートされていません。
- この機能はローカル キャッシュでは構成されません。
- コールドスタートのパフォーマンスを向上するには、ローカルの Zip オプション (`WEBSITE_RUN_FROM_PACKAGE` = 1) を使用します。

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure Functions の継続的なデプロイ](functions-continuous-deployment.md)

[Zip deployment for Azure Functions]: deployment-zip-push.md
