---
title: Azure Media Services v3 を使用できるクラウドとリージョン
description: この記事では、Azure Media Services v3 を使用できる Azure クラウドとリージョンについて説明します。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/21/2020
ms.author: juliako
ms.openlocfilehash: 58b5b749e81aab4d8563d09cbfd139629520531c
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2020
ms.locfileid: "76310572"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Azure Media Services v3 が存在するクラウドとリージョン

Azure Media Services v3 は、Azure Resource Manager マニフェストを通して、グローバル Azure、Azure Government、Azure Germany、Azure China 21Vianet で使用できます。 ただし、すべての Azure クラウドで Media Services のすべての機能を使用できるわけではありません。 このドキュメントでは、Media Services v3 の主要コンポーネントの使用可能性を示します。

## <a name="feature-availability-in-azure-clouds"></a>Azure クラウドで使用できる機能

| 機能|グローバル Azure リージョン | Azure Government|Azure Germany|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | 利用可能 | 使用不可 | 使用不可 | 使用不可 |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  利用可能 | 使用不可 | 使用不可 | 使用不可 |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  利用可能 | 使用不可 | 使用不可 | 使用不可 |
| [StandardEncoderPreset](encoding-concept.md) | 利用可能 | 利用可能 | 利用可能 | 利用可能 |
| [LiveEvents](live-streaming-overview.md) | 利用可能 | 利用可能 | 利用可能 | 利用可能 |
| [StreamingEndpoints](streaming-endpoint-concept.md) | 利用可能 | 利用可能 | 利用可能 | 利用可能 |

## <a name="regionsgeographieslocations"></a>リージョン/地域/場所

[Azure Media Services サービスがデプロイされているリージョン](https://azure.microsoft.com/global-infrastructure/services/?products=media-services)

### <a name="region-code-name"></a>リージョン コード名 

**location** パラメーターを指定する必要がある場合は、**location** 値としてリージョンのコード名を指定する必要があります。 お使いのアカウントが存在し、呼び出しがルーティングされるリージョンのコード名を取得するには、次の行を [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) で実行できます

```bash
az account list-locations
```

上記の行を実行すると、すべての Azure リージョンの一覧が取得されます。 探している *displayName* がある Azure リージョンに移動し、その *name* 値を **location** パラメーターで使用します。

たとえば、Azure リージョンの米国西部 2 の場合 (下に示されています)、"westus2" を **location** パラメーターで使用します。

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="endpoints"></a>エンドポイント  

異なる国の Azure クラウドから Media Services アカウントに接続する際に、次のエンドポイントを知っておくことが重要です。

### <a name="global-azure"></a>グローバル Azure

|エンドポイント ||
| --- | --- | 
| Azure Resource Manager |  `https://management.azure.com/` |
| 認証 | `https://login.microsoftonline.com/` | 
| トークン対象ユーザー | `https://management.core.windows.net/` |

### <a name="azure-government"></a>Azure Government

|エンドポイント||
| --- | --- | 
| Azure Resource Manager |  `https://management.usgovcloudapi.net/` |
| 認証 | `https://login.microsoftonline.us/` | 
| トークン対象ユーザー | `https://management.core.usgovcloudapi.net/` |

### <a name="azure-germany"></a>Azure Germany

| エンドポイント ||
| --- | --- |  
| Azure Resource Manager | `https://management.cloudapi.de/` |
| 認証 | `https://login.microsoftonline.de/` |
| トークン対象ユーザー | `https://management.core.cloudapi.de/`|

### <a name="azure-china-21vianet"></a>Azure China 21Vianet

|エンドポイント||
| --- | --- | 
| Azure Resource Manager | `https://management.chinacloudapi.cn/` |
| 認証 | `https://login.chinacloudapi.cn/` |
| トークン対象ユーザー |  `https://management.core.chinacloudapi.cn/` |

## <a name="see-also"></a>参照

* [Azure リージョン](https://azure.microsoft.com/global-infrastructure/regions/)
* [Azure の地域](https://azure.microsoft.com/global-infrastructure/geographies/)
* [Azure の場所](https://azure.microsoft.com/global-infrastructure/locations/)

## <a name="next-steps"></a>次のステップ

[Media Services v3 の概要](media-services-overview.md)
