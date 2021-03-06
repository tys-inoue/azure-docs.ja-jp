---
title: 削除されたストレージ アカウントを回復させる方法
description: 削除されたストレージ アカウントを回復させる方法について説明します
author: todmccoy
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: rogarana
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: 05465d4a03335ac607ba8981116c66fd6dac9416
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252631"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>削除されたストレージ アカウントを回復させる方法

Azure Storage では、自動レプリカによるデータの回復性が提供されますが、ユーザーやアプリケーション コードによるデータの破損を防ぐことはできません。それが不注意によるものか、故意によるものかは関係ありません。 アプリケーション エラーやユーザー エラーが発生したときにデータの忠実性を維持するには、監査ログと共にセカンダリ ストレージの場所にデータをコピーするなどの高度な手法が必要です。

次の表には、レプリケーション戦略に応じて、ストレージ アカウントの回復のスコープの概要が示されています。

| |LRS|ZRS|GRS|RA - GRS|
|---|---|---|---|---|
|Azure Resource Manager ストレージ アカウント|はい|はい|はい|はい|
|クラシック ストレージ アカウント|はい|はい|はい|はい|

次の情報を収集し、Microsoft サポートでサポート リクエストを提出します。

* ストレージ アカウント名
* 削除日
* ストレージ アカウントのリージョン
* ストレージ アカウントはどのように削除されましたか?
* ストレージ アカウントをどのような方法で削除しましたか? (ポータル、PowerShell など)

重要なポイント

* 通常、ストレージ サービスが削除されてからガベージ コレクションが実行されるまで、最大で 15 日かかることがあります。そのため、SLA ではストレージ アカウントが回復されない場合があります。
* Microsoft サポートでは、ベストエフォート方式でコンテナー/アカウントの回復を試み、回復を保証することはできません。

> [!NOTE]
> アカウントが再作成されている場合、回復が正常に行われない可能性があります。 アカウントを既に再作成している場合は、回復を試みる前に、まず、削除する必要があります。
