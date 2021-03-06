---
title: Custom Speech 用のテスト データを準備する - Speech サービス
titleSuffix: Azure Cognitive Services
description: マイクロソフトの音声認識の正確性をテストする場合、またはカスタム モデルをトレーニングする場合は、オーディオ データとテキスト データが必要です。 このページでは、データ型、使用方法、および管理方法について説明します。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: erhopf
ms.openlocfilehash: 6100ac6a6b01a7d0eac74b0e83539bf4e671cb89
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660411"
---
# <a name="prepare-data-for-custom-speech"></a>Custom Speech 用のテスト データを準備する

マイクロソフトの音声認識の正確性をテストする場合、またはカスタム モデルをトレーニングする場合は、オーディオ データとテキスト データが必要です。 このページでは、データ型、使用方法、および管理方法について説明します。

## <a name="data-types"></a>データ型

この表には、許容されるデータの種類と、それぞれのデータの種類を使用する場合と推奨される数量が一覧表示されています。 モデルを作成するのに、すべてのデータの種類は必要ありません。 データ要件は、テストを作成するのか、またはモデルをトレーニングするのかによって異なります。

| データ型 | テストに使用 | 推奨数量 | トレーニングに使用 | 推奨数量 |
|-----------|-----------------|----------|-------------------|----------|
| [オーディオ](#audio-data-for-testing) | はい<br>目視検査に使用 | 5 つ以上のオーディオ ファイル | いいえ | 該当なし |
| [オーディオ + 人間というラベルが付いたトランスクリプト](#audio--human-labeled-transcript-data-for-testingtraining) | はい<br>精度を評価するために使用 | 0.5-5 時間のオーディオ | はい | 1 - 1,000 時間のオーディオ |
| [関連するテキスト](#related-text-data-for-training) | いいえ | 該当なし | はい | 1 - 200 MB の関連テキスト |

ファイルは、型別にデータセットにグループ化し、ZIP ファイルとしてアップロードする必要があります。 各データセットには、1 つのデータの種類のみを含めることができます。

> [!TIP]
> すぐに始めるには、サンプルデータの使用を検討してください。 <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/sampledata/customspeech" target="_target">サンプル Custom Speech データ <span class="docon docon-navigate-external x-hidden-focus"></span></a>については、こちらの GitHub リポジトリを参照する

## <a name="upload-data"></a>データのアップロード

データをアップロードするには、<a href="https://speech.microsoft.com/customspeech" target="_blank">Custom Speech ポータル <span class="docon docon-navigate-external x-hidden-focus"></span></a>に移動します。 ポータルで、 **[データのアップロード]** をクリックしてウィザードを起動し、最初のデータセットを作成します。 データのアップロードが許可される前に、データセットにより音声データ型を選択するように求められます。

![Speech ポータルからオーディオを選択する](./media/custom-speech/custom-speech-select-audio.png)

アップロードする各データセットでは、選択したデータの種類の要件が満たされている必要があります。 アップロードする前に、データの書式を正しく設定する必要があります。 データが正しくフォーマットされていれば、Custom Speech サービスによって正確に処理されます。 要件は、以降のセクションに示されています。

データセットをアップロードした後は、いくつかの選択肢があります。

* **[テスト]** タブに移動して、オーディオのみまたはオーディオ + 人間というラベルが付いた文字起こしデータを目視で検査できます。
* **[トレーニング]** タブに移動し、オーディオ + 人間の文字起こしデータまたは関連するテキスト データを使用して、カスタム モデルをトレーニングできます。

## <a name="audio-data-for-testing"></a>テスト用のオーディオ データ

オーディオ データは、Microsoft の基準の音声テキスト変換モデルやカスタム モデルの精度をテストするのに最適です。 オーディオ データは、特定のモデルのパフォーマンスに関する音声の精度を検査するために使用されることに注意してください。 モデルの精度の定量化を検討している場合は、[オーディオ + 人間というラベルが付いた文字起こしデータ](#audio--human-labeled-transcript-data-for-testingtraining)を使用します。

次の表を使用して、Custom Speech で使用するためにオーディオ ファイルが確実に正しく書式設定されるようにします。

| プロパティ | 値 |
|----------|-------|
| ファイル形式 | RIFF (WAV) |
| サンプル レート | 8,000 Hz または 16,000 Hz |
| チャンネル | 1 (モノラル) |
| オーディオあたりの最大長 | 2 時間 |
| サンプル形式 | PCM、16 ビット |
| アーカイブ形式 | .zip |
| 最大アーカイブ サイズ | 2 GB |

> [!TIP]
> トレーニング データとテスト データをアップロードする場合、.zip ファイルのサイズは 2 GB を超えることはできません。 トレーニング用にさらに多くのデータが必要な場合は、複数の .zip ファイルに分割し、個別にアップロードします。 その後、*複数* のデータセットからトレーニングを選択できます。 ただし、*単一* のデータセットからのみテストができます。

<a href="http://sox.sourceforge.net" target="_blank" rel="noopener">SoX <span class="docon docon-navigate-external x-hidden-focus"></span></a> を使用して、オーディオのプロパティを確認したり、既存のオーディオを適切な形式に変換したりします。 SoX のコマンドラインから、これらのアクティビティ各々を実行する方法の例を、次にいくつか示します:

| アクティビティ | 説明 | SoX コマンド |
|----------|-------------|-------------|
| オーディオ形式の確認 | このコマンドを使用して確認<br>オーディオ ファイル形式。 | `sox --i <filename>` |
| オーディオの形式の変換 | このコマンドを使用して変換<br>オーディオファイルは、シングルチャネル、16 ビット、16 KHz です。 | `sox <input> -b 16 -e signed-integer -c 1 -r 16k -t wav <output>.wav` |

## <a name="audio--human-labeled-transcript-data-for-testingtraining"></a>テスト/トレーニング用のオーディオ + 人間というラベルが付いた文字起こしデータ

オーディオ ファイルを処理するときに、Microsoft の音声テキスト変換の精度を測定するには、比較のため、人間というラベルが付いた文字起こし (単語単位) を提供する必要があります。 多くの場合、人間というラベルが付いた文字起こしには時間がかかりますが、ユース ケースのモデルの精度を評価およびトレーニングするために必要です。 認識の向上は、提供されたデータによって決まることに注意してください。 そのため、高品質なトランスクリプトのみをアップロードすることが重要です。

| プロパティ | 値 |
|----------|-------|
| ファイル形式 | RIFF (WAV) |
| サンプル レート | 8,000 Hz または 16,000 Hz |
| チャンネル | 1 (モノラル) |
| オーディオあたりの最大長 | 2 時間 (テスト) / 60 分 (トレーニング) |
| サンプル形式 | PCM、16 ビット |
| アーカイブ形式 | .zip |
| Zip の最大サイズ | 2 GB |

> [!NOTE]
> トレーニング データとテスト データをアップロードする場合、.zip ファイルのサイズは 2 GB を超えることはできません。 Uou は、*単一* のデータセットからのみテストできます。確認して、適切なファイルサイズで保持するようにしてください。

単語の削除や置換のような問題に対処するには、認識を向上させるために大量のデータが必要です。 一般に、約 10 から 1,000 時間分のオーディオについて単語単位の文字起こしを提供することをお勧めします。 すべての WAV ファイルの文字起こしは、1 つのプレーン テキスト ファイルに格納されている必要があります。 文字起こしファイルの各行には、いずれかのオーディオ ファイルの名前に続けて、対応する文字起こしが含まれている必要があります。 ファイル名と文字起こしは、タブ (\t) で区切る必要があります。

  次に例を示します。
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```

> [!IMPORTANT]
> 文字起こしは、UTF-8 バイト オーダー マーク (BOM) としてエンコードされている必要があります。

文字起こしに対しては、システムによって処理できるように、テキストの正規化が行われます。 ただし、データを Speech Studio にアップロードする前に実行する必要がある重要な正規化がいくつかあります。 文字起こしを準備する際に使用する適切な言語については、「[How to create a human-labeled transcription](how-to-custom-speech-human-labeled-transcriptions.md)」(人間とラベル付けされた文字起こしの作成方法) を参照してください。

オーディオ ファイルと対応する文字起こしを収集した後、<a href="https://speech.microsoft.com/customspeech" target="_blank">Custom Speech ポータル <span class="docon docon-navigate-external x-hidden-focus"></span></a>にアップロードする前に、それらを1つの .zip ファイルとしてパッケージ化します。 3 つのオーディオ ファイルと、人間とラベル付けされた文字起こしファイルを含むデータセットの例を示します:

> [!div class="mx-imgBorder"]
> ![Speech ポータルからオーディオを選択する](./media/custom-speech/custom-speech-audio-transcript-pairs.png)

## <a name="related-text-data-for-training"></a>トレーニング用の関連するテキスト データ

製品名または固有の機能には、トレーニング用の関連テキストデータが含まれている必要があります。 関連テキストは、正しい認識を保証するのに役立ちます。 認識を向上させるため、次の 2 種類の関連するテキスト データを提供することができます。

| データ型 | このデータによりどのように認識が向上するか |
|-----------|------------------------------------|
| 文 (発話) | 製品名を認識する場合や、文のコンテキスト内で業界固有の語彙を認識する場合の精度を向上させます。 |
| 発音 | 一般的でない用語、略語、またはその他の発音が定義されていない単語の発音を向上させることができます。 |

発話は、1 つまたは複数のテキスト ファイルとして提供できます。 正確性を高めるには、読み上げられる発話に近いテキスト データを使用します。 発音は、1 つのテキスト ファイルとして指定する必要があります。 すべてを 1つの zip ファイルとしてパッケージ化し、<a href="https://speech.microsoft.com/customspeech" target="_blank">Custom Speech ポータル <span class="docon docon-navigate-external x-hidden-focus"></span></a>にアップロードできます。

### <a name="guidelines-to-create-a-sentences-file"></a>発話ファイルを作成するためのガイドライン

関連する文を使用してカスタム モデルを作成するには、サンプルの発話の一覧を提供する必要があります。 発話は、完全または文法的に正しい _必要はありません_ が、運用環境で期待される音声入力を正確に反映する必要があります。 特定の用語の重みを付けるには、これらの特定の用語を含む複数の文を追加します。

一般的なガイダンスとして、モデル適応は、トレーニングテキストが運用環境で予想される実際のテキストにできるだけ近い場合に最も効果的です。 強化する対象として、ドメイン固有の用語や語句をトレーニングテキストに含める必要があります。 可能であれば、1つの文またはキーワードを別の行に制御してみてください。 ユーザーにとって重要なキーワードと語句 (製品名など) は、数回コピーできます。 しかし、コピーしすぎないでください。これは、全体的な認識率に影響する可能性があります。

この表を使用して、発話の関連するデータ ファイルが確実に正しく書式設定されるようにします。

| プロパティ | 値 |
|----------|-------|
| テキストのエンコード | UTF-8 BOM |
| 1 行あたりの発話の数 | 1 |
| ファイルの最大サイズ | 200 MB |

さらに、次の制限を考慮します。

* 文字を 5 回以上繰り返さないようにします。 例: "aaaa" や "uuuu"。
* 特殊文字または 上記 `U+00A1` の UTF-8 文字は使用しないでください。
* URI は拒否されます。

### <a name="guidelines-to-create-a-pronunciation-file"></a>発音ファイルを作成するためのガイドライン

ユーザー側で発生するまたは使用する可能性がある、標準の発音がない一般的でない用語がある場合は、カスタム発音ファイルを提供して認識を向上させることができます。

> [!IMPORTANT]
> カスタムの発音ファイルを使用して、共通単語の発音を変更することはお勧めしません。

これには、音声発話の例と、それぞれのカスタム発音が含まれます。

| 認識される/表示されるフォーム | 音声フォーム |
|--------------|--------------------------|
| 3CPO | three c p o |
| CNTK | c n t k |
| IEEE | i triple e |

音声フォームは、スペル アウトされた表示フォームの音声シーケンスです。これは文字、単語、音節、または 3 つすべての組み合わせで構成できます。

カスタマイズされた発音は、英語 (`en-US`) およびドイツ語 (`de-DE`) で使用できます。 この表に、言語ごとにサポートされている文字を示します。

| 言語 | Locale | 文字 |
|----------|--------|------------|
| English | `en-US` | `a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |
| German | `de-DE` | `ä, ö, ü, a, b, c, d, e, f, g, h, i, j, k, l, m, n, o, p, q, r, s, t, u, v, w, x, y, z` |

次の表を使用して、発音用の関連データ ファイルが正しく書式設定されているか確認します。 発音ファイルは小さいため、数キロバイトしかサイズは必要ありません。

| プロパティ | 値 |
|----------|-------|
| テキストのエンコード | UTF-8 BOM (英語では ANSI もサポートされています) |
| 1 行のあたりの発音数 | 1 |
| ファイルの最大サイズ | 1 MB (Free レベルに 1 KB) |

## <a name="next-steps"></a>次のステップ

* [データを検査する](how-to-custom-speech-inspect-data.md)
* [データを評価する](how-to-custom-speech-evaluate-data.md)
* [モデルをトレーニングする](how-to-custom-speech-train-model.md)
* [モデルをデプロイする](how-to-custom-speech-deploy-model.md)
