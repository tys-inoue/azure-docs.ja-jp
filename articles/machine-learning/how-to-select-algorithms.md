---
title: 機械学習アルゴリズムの選択方法
titleSuffix: Azure Machine Learning
description: クラスタリング、分類、または回帰の実験で教師あり学習と教師なし学習用の Azure Machine Learning のアルゴリズムを選択する方法。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: FrancescaLazzeri
ms.author: lazzeri
ms.reviewer: cgronlun
ms.date: 03/05/2020
ms.openlocfilehash: e0482bac9569a834adf3e1cdef2b3f702980eac0
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2020
ms.locfileid: "78328665"
---
# <a name="how-to-select-algorithms-for-azure-machine-learning"></a>Azure Machine Learning のアルゴリズムの選択方法

よくある質問は、「どの機械学習アルゴリズムを使用すればよいか」ということです。 選択するアルゴリズムは、主として、データ サイエンス シナリオの次の 2 つの異なる側面によって決まります。

 - **データを使って何をしたいか?** 具体的には、過去のデータから学習することによって回答を得たいビジネス上の質問は何かということです。

 - **データ サイエンス シナリオの要件は何か?** 具体的には、ソリューションでサポートする精度、トレーニング時間、線形性、パラメーターの数、特徴の数はどのくらいかということです。

 ![アルゴリズムの選択に関する考慮事項:何を知りたいのか? シナリオの要件は何か?](./media/how-to-select-algorithms/how-to-select-algorithms.png)

## <a name="business-scenarios-and-the-machine-learning-algorithm-cheat-sheet"></a>ビジネス シナリオと機械学習アルゴリズム チート シート

[Azure Machine Learning アルゴリズム チート シート](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri)は、最初の考慮事項である、「**データを使って何をしたいか**」について検討するのに役立ちます。 Machine Learning アルゴリズム チート シートで、目的のタスクを探して、予測分析ソリューション用の [Azure Machine Learning デザイナー](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri)のアルゴリズムを見つけます。 

Machine Learning デザイナーは、[多クラス デシジョン フォレスト](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri)、[レコメンデーション システム](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-recommender?WT.mc_id=docs-article-lazzeri)、[ニューラル ネットワーク回帰](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri)、[多クラス ニューラル ネットワーク](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri)、[K-Means クラスタリング](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri)などのアルゴリズムの包括的なポートフォリオを提供します。 各アルゴリズムは、機械学習の異なる種類の問題に対処するように設計されています。 完全な一覧、各アルゴリズムのしくみとパラメーターを調整してアルゴリズムを最適化する方法に関するドキュメントについては、「[Azure Machine Learning デザイナーのアルゴリズムとモジュールのリファレンス](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri)」を参照してください。

> [!NOTE]
> 機械学習アルゴリズム チート シートをダウンロードするには、「[Azure Machine Learning デザイナーの機械学習アルゴリズム チート シート](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri)」にアクセスします。
> 
> 

ソリューションの機械学習アルゴリズムを選択する際には、Azure Machine Learning アルゴリズム チート シートのガイダンスと合わせて、他の要件にも留意してください。 精度、トレーニング時間、線形性、パラメーターの数、特徴の数など、考慮する必要がある追加の要素を次に示します。

## <a name="additional-requirements-for-a-data-science-scenario"></a>データ サイエンス シナリオの追加要件

データを使って何をやりたいかがわかったら、ソリューションの追加要件を決定する必要があります。 

次の要件を選択します。これらの間にはトレードオフが生じる可能性があります。

- 精度
- トレーニング時間
- 線形性
- パラメーターの数
- 特徴の数

## <a name="accuracy"></a>精度

機械学習の精度では、すべてのケースに対する真の結果の割合として、モデルの有効性を測定します。 Machine Learning デザイナーでは、[モデルの評価モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri)で業界標準の一連の評価メトリックを計算します。 このモジュールを使用して、トレーニング済みのモデルの精度を測定します。

可能な限り最も正確な回答を得ることが常に必要であるとは限りません。 使用目的によっては、近似で十分な場合があります。 その場合は、より近似的な方法を使用することで、処理時間を大幅に短縮できる場合があります。 さらに、近似的な方法には、当然ながらオーバーフィットを回避する傾向があります。

モデルの評価モジュールを使用するには、次の 3 つの方法があります。

- モデルを評価するために、トレーニング データのスコアを生成する
- モデルでスコアを生成するが、これらのスコアを予約済みのテスト セットでのスコアと比較する
- 同じデータ セットを使用して、2 つの異なるが関連するモデルのスコアを比較する

機械学習モデルの精度を評価するために使用できるメトリックとアプローチの完全な一覧については、「[モデルの評価モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri)」を参照してください。

## <a name="training-time"></a>トレーニング時間

教師あり学習の場合、トレーニングは、履歴データを使用してエラーを最小限に抑える機械学習モデルを構築することを意味します。 モデルのトレーニングに必要な分数または時間数は、アルゴリズムによって大きく異なります。 多くの場合、トレーニング時間は精度と密接に関係しており、通常、片方があれば他方も伴います。 

さらに、一部のアルゴリズムは他よりデータ ポイントの数に大きく影響を受けます。 時間に制限があるため、特にデータ セットが大きい場合、特定のアルゴリズムを選択することもできます。

Machine Learning デザイナーでは、機械学習モデルの作成と使用は、通常、3 つのステップから成るプロセスです。

1.  特定の種類のアルゴリズムを選択し、そのパラメーターまたはハイパーパラメーターを定義してモデルを構成します。 

2.  ラベル付けされていて、かつアルゴリズムに適合したデータを含んだデータセットを指定します。 データとモデルの両方を[モデルのトレーニング モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/train-model?WT.mc_id=docs-article-lazzeri)に接続します。

3.  トレーニングの完了後、いずれかの[スコアリング モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/score-model?WT.mc_id=docs-article-lazzeri)でトレーニング済みのモデルを使用し、新しいデータについて予測を行います。

## <a name="linearity"></a>線形性

統計および機械学習では、線形性は、データ セット内の変数と定数の間に線形関係があることを意味します。 たとえば、線形分類アルゴリズムは、クラスを直線 (またはその高次元版) で分離できることを想定しています。

機械学習アルゴリズムの多くは線形性を使用します。 Azure Machine Learning デザイナーでは、このようなアルゴリズムとして次のものがあります。 

- [多クラスのロジスティック回帰](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [T2 クラスのロジスティック回帰](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [サポート ベクター マシン](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri)  

線形回帰アルゴリズムは、データの傾向が直線に従うことを想定しています。 問題によって、この想定が適切な場合もありますが、精度が低下することもあります。 線形回帰アルゴリズムは、欠点はあっても、最初の戦略として一般的です。 アルゴリズムが簡単で、速くトレーニングできる傾向があります。

![非線形クラス境界](./media/how-to-select-algorithms/nonlinear-class-boundary.png)

***非線形クラス境界***: *線形分類アルゴリズムに従うと、精度が低下します。*

![非線形傾向のデータ](./media/how-to-select-algorithms/nonlinear-trend.png)

***非線形傾向のデータ***: *線形回帰法を使用すると、必要以上に多くのエラーが生成されます。*

## <a name="number-of-parameters"></a>パラメーターの数

パラメーターは、アルゴリズムを設定するときに使用します。 エラーの許容誤差や反復回数などのアルゴリズムの動作に影響を与える数値、またはアルゴリズムの動作方法のバリエーション間で設定するオプションです。 アルゴリズムのトレーニング時間と精度は、適切な設定を行うかどうかによって影響を受ける場合があります。 通常、パラメーター数の多いアルゴリズムは、適切な組み合わせを見つけるのに多くの試行錯誤が必要です。

このため、Machine Learning デザイナーには、[モデルのハイパーパラメーターの調整モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/tune-model-hyperparameters?WT.mc_id=docs-article-lazzeri)があります。このモジュールの目標は、機械学習モデルに最適なハイパーパラメーターを特定することです。 このモジュールでは、さまざまに組み合わせた設定を使って複数のモデルをビルドし、テストします。 その後、すべてのモデルについてメトリックを比較し、設定の組み合わせを求めます。 

これは、パラメーター空間を確実に網羅する優れた方法ですが、パラメーターの数が増えるとモデルのトレーニングに必要な時間が指数関数的に増加します。 利点として、通常、パラメーターの数の多さはアルゴリズムがより柔軟であることを示します。 適切なパラメーター設定の組み合わせを見つけられる場合に、非常に高い精度を示すことが多くあります。

## <a name="number-of-features"></a>特徴の数

機械学習の場合、特徴は、分析しようとしている現象の定量化可能な変数です。 特定の種類のデータでは、特徴の数がデータ ポイントの数と比較して非常に大きくなる可能性があります。 遺伝学やテキスト データの場合によくあります。 

特徴の数が多いと、一部の学習アルゴリズムは処理が遅くなり、実行不可能なほどトレーニング時間が長くなります。 [サポート ベクトル マシン](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri)は、特徴の数が多いシナリオに特に適しています。 このため、情報の取得からテキストおよびイメージの分類まで、数多くのアプリケーションで使用されています。 サポート ベクトル マシンは、分類タスクと回帰タスクの両方に使用できます。

特徴選択とは、指定された出力を前提として、統計的テストを入力に適用するプロセスを指します。 この目標は、出力の予測能力が高い列を特定することです。 Machine Learning デザイナーの[フィルターに基づく特徴選択モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/filter-based-feature-selection?WT.mc_id=docs-article-lazzeri)には、選択肢となる複数の特徴選択アルゴリズムが用意されています。 このモジュールには、ピアソンの相関やカイ二乗値などの相関法が含まれています。

[順列の特徴量の重要度モジュール](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/permutation-feature-importance?WT.mc_id=docs-article-lazzeri)を使用して、データ セットの一連の特徴量の重要度スコアを計算することもできます。 これらのスコアを利用すると、モデルで使用する最適な特徴を決定するのに役立ちます。


## <a name="next-steps"></a>次のステップ

 - [Azure Machine Learning デザイナーの詳細](https://docs.microsoft.com/azure/machine-learning/service/concept-designer?WT.mc_id=docs-article-lazzeri)
 - Azure Machine Learning デザイナーで使用可能なすべての機械学習アルゴリズムの詳細については、「[Machine Learning デザイナーのアルゴリズムとモジュールのリファレンス](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri)」を参照してください
 - ディープ ラーニング、機械学習、AI の関係を調べるには、「[ディープ ラーニングと機械学習](https://docs.microsoft.com/azure/machine-learning/service/concept-deep-learning-vs-machine-learning?WT.mc_id=docs-article-lazzeri)」を参照してください
