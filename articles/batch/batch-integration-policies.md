---
title: Azure ポリシーとの統合 - Azure Batch | Microsoft Docs
description: ''
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/24/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 9a306457f838fc79d12be3217d96cc8fb25c9c1b
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617867"
---
# <a name="integration-with-azure-policy"></a>Azure Policy との統合

Azure Policy は、リソースにルールを適用するポリシーを作成、割り当て、管理するために使用する Azure のサービスであり、これらのリソースが企業の基準やサービスレベル契約に準拠したままになるようにします。 Azure Policy では、割り当てられているポリシーへの非準拠がないか、リソースを評価します。 

Azure Batch には、ポリシー コンプライアンスを管理するための拡張機能が 2 つ用意されています。 

|**名前**...|   **説明**|**効果**|  **Version**|    **ソース**
|----------------|----------|----------|----------------|---------------|
|Batch アカウントで診断ログを有効にする必要がある|   診断ログが有効になっていることを監査します。 これにより、セキュリティ インシデントが発生した場合やお使いのネットワークが侵害された場合に、調査目的で使用するアクティビティ証跡を再作成できます|AuditIfNotExists、Disabled|  2.0.0|  GitHub|
|Batch アカウントにおけるメトリック アラート ルールを構成する必要がある| 必須メトリックを有効にするための Batch アカウントにおけるメトリック アラート ルールの構成を監査します|   AuditIfNotExists、Disabled| 1.0.0|  GitHub|

ポリシーの定義には、満たす必要がある条件が記述されています。 条件によって、リソース プロパティが必要な値と比較されます。 リソース プロパティ フィールドには、事前定義されたエイリアスを使用してアクセスします。リソースの種類の特定のプロパティにアクセスするには、プロパティの別名を使用します。 エイリアスを使用すると、リソースのプロパティで許可される値または条件を制限できます。 各エイリアスは、特定のリソースの種類について異なる API バージョンのパスにマップされます。 ポリシーの評価時に、ポリシー エンジンはその API バージョンのプロパティ パスを取得します。

Batch で必要なリソースには、アカウント、計算ノード、プール、ジョブ、タスクなどがあります。 そのため、プロパティの別名を使用して、これらのリソースの特定のプロパティにアクセスします。 [エイリアス](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)の詳細について学習します

現在のエイリアスを把握し、リソースとポリシーを確認するには、 Visual Studio Code 用の Azure policy extension を使用します。 Visual Studio Code でサポートされているすべてのプラットフォームにインストールできます。 このサポートには、Windows、Linux、macOS が含まれます。 [インストール ガイドライン](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode)をご覧ください。



