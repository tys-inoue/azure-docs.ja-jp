---
title: Linux VM のために Azure Key Vault を設定する
description: Azure CLI を使用して Azure Resource Manager 仮想マシン用に Key Vault を設定する方法。
author: singhkays
manager: gwallace
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/24/2017
ms.author: kasing
ms.openlocfilehash: c9659c38e349bd62f22ed0d0839d97651b1b3b66
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2020
ms.locfileid: "78944819"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Azure CLI を使用して仮想マシン用に Key Vault を設定する方法

Azure Resource Manager スタックでは、Key Vault により提供されるリソースとしてシークレット/証明書がモデル化されます。 Azure Key Vault の詳細については、「 [Azure Key Vault とは](../../key-vault/key-vault-overview.md) Azure Resource Manager VM と共に Key Vault を使用するには、Key Vault の *EnabledForDeployment* プロパティを True に設定する必要があります。 この記事では、Azure CLI を使用して Azure 仮想マシン (VM) で Key Vault を設定する方法について説明します。 

これらの手順を実行するには、[Azure CLI](/cli/azure/install-az-cli2) の最新版をインストールし、[az login](/cli/azure/reference-index) を使用して Azure アカウントにログインする必要があります。

## <a name="create-a-key-vault"></a>Key Vault の作成
[az keyvault create](/cli/azure/keyvault) で Key Vault を作成し、デプロイメント ポリシーを割り当てます。 次の例では、`myKeyVault` という名前の Key Vault を `myResourceGroup` リソース グループに作成します。

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>VM で使用するように Key Vault を更新する
[az keyvault update](/cli/azure/keyvault) で、デプロイメント ポリシーを既存の Key Vault に設定します。 次の例では、`myResourceGroup` リソース グループにある `myKeyVault` という名前の Key Vault を更新します。

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>テンプレートを使用して Key Vault を設定する
テンプレートを使用する場合、次のように、Key Vault リソースの `enabledForDeployment` プロパティを `true` に設定する必要があります。

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>次のステップ
テンプレートを使用して、Key Vault の作成時に構成できるその他のオプションについては、「[Key Vault の作成](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)」を参照してください。
