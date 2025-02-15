---
title: Read API を呼び出す方法
titleSuffix: Azure Cognitive Services
description: Read API を呼び出して、その動作を詳細に構成する方法について説明します。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: pafarley
ms.openlocfilehash: 54ec669984488d5fe429050faefa0ff2d2e8eb9b
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106287511"
---
# <a name="call-the-read-api"></a>Read API を呼び出す

このガイドでは、Read API を呼び出して画像からテキストを抽出する方法について説明します。 この API の動作をニーズに合わせて構成できるさまざまな方法を学習します。

## <a name="submit-data-to-the-service"></a>サービスにデータを送信する

Read API の [Read 呼び出し](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)は、画像または PDF ドキュメントを入力として受け取り、非同期でテキストを抽出します。

`https://{endpoint}/vision/v3.2-preview.3/read/analyze[?language][&pages][&readingOrder]`

この呼び出しにより、`Operation-Location` という応答ヘッダー フィールドが返されます。 `Operation-Location` 値は、次の手順で使用する操作 ID を含む URL です。

|応答ヘッダー| 値の例 |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/vision/v3.1/read/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

> [!NOTE]
> **Billing** 
>
> 「[Computer Vision の価格](https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/)」ページには、Read の価格レベルが含まれています。 分析された画像またはページはそれぞれ 1 つのトランザクションです。 100 ページを含む PDF または TIFF ドキュメントを使用して操作を呼び出した場合、Read 操作ではそれは 100 トランザクションとしてカウントされ、100 トランザクションに対して課金されます。 操作に対して 50 回の呼び出しを行い、各呼び出しで 100 ページのドキュメントが送信された場合、50 X 100 = 5000 トランザクションに対して課金されます。

## <a name="determine-how-to-process-the-data"></a>データの処理方法を定義する

### <a name="language-specification"></a>言語仕様

[Read](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d986960601faab4bf452005) 呼び出しには、言語に対する省略可能な要求パラメーターがあります。 読み取りでは、言語の自動識別と多言語ドキュメントがサポートされるため、言語コードの指定は、その特定の言語としてドキュメントを処理するように強制する場合にのみ行ってください。

### <a name="natural-reading-order-output-latin-languages-only"></a>自然な読み取り順序の出力 (ラテン言語のみ)
[Read 3.2 プレビュー API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) では、`readingOrder` クエリ パラメーターを使用してテキスト行の出力順序を指定できます。 次の例に示すように、`natural` を使用して、よりわかりやすい読み取り順序の出力を行います。 この機能は、ラテン語系の言語でのみサポートされています。

:::image border type="content" source="../Images/ocr-reading-order-example.png" alt-text="OCR の読み取り順序の例":::



### <a name="select-pages-or-page-ranges-for-text-extraction"></a>テキスト抽出のページまたはページ範囲の選択
[Read 3.2 プレビュー API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) では、大規模なマルチページ ドキュメントに対し、`pages` クエリ パラメーターを使用して、ページ番号またはページ範囲を指定し、それらのページからのみテキストを抽出します。 次の例は、10 ページを含むドキュメントを示しています。ここには、すべてのページ (1 から 10) と選択したページ (3 から 6) の両方に対して抽出されたテキストがあります。

:::image border type="content" source="../Images/ocr-select-pages.png" alt-text="選択されたページの出力":::

## <a name="get-results-from-the-service"></a>サービスから結果を取得する

2 番目のステップでは、[読み取り結果の取得](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/5d9869604be85dee480c8750)操作を呼び出します。 この操作は、読み取り操作によって作成された操作 ID を入力として受け取ります。 

`https://{endpoint}/vision/v3.2-preview.3/read/analyzeResults/{operationId}`

これにより、次の設定可能な値を持つ **status** フィールドが含まれた JSON 応答が返されます。 

|値 | 説明 |
|:-----|:----|
| `notStarted`| 操作は開始されていません。 |
| `running`| 操作を処理しています。 |
| `failed`| 操作は失敗しました。 |
| `succeeded`| 操作は成功しました。 |

**succeeded** の値が返されるまで、この操作を対話形式で呼び出します。 1 秒あたりの要求 (RPS) レートを超えないようにするために、1 秒から 2 秒の間隔を使用してください。

> [!NOTE]
> Free レベルでは、要求レートは 1 分あたり 20 回の呼び出しに制限されます。 有料レベルでは、10 RPS (1 秒あたりの要求) が許可されており、これはリクエストに応じて増加できます。 1 秒あたりの要求 (RPS) レートの引き上げをリクエストするには、Azure サポート チャネルまたはお客様のアカウント チームを使用してください。

**status** フィールドに `succeeded` 値が指定されている場合、JSON 応答には、画像またはドキュメントから抽出されたテキスト コンテンツが含まれます。 JSON の応答では、認識された単語の元の行グループが維持されます。 抽出されたテキスト行とその境界ボックスの座標が含まれます。 各テキスト行には、抽出されたすべての単語と、その座標および信頼度スコアが含まれています。

> [!NOTE]
> `Read` 操作に送信されたデータは一時的に暗号化され、しばらくの間保存されてから削除されます。 これにより、アプリケーションからは、サービス応答の一部として抽出されたテキストを取得できます。

### <a name="sample-json-output"></a>サンプル JSON 出力

成功した JSON 応答の例を次に示します。

```json
{
  "status": "succeeded",
  "createdDateTime": "2020-05-28T05:13:21Z",
  "lastUpdatedDateTime": "2020-05-28T05:13:22Z",
  "analyzeResult": {
    "version": "3.1.0",
    "readResults": [
      {
        "page": 1,
        "angle": 0.8551,
        "width": 2661,
        "height": 1901,
        "unit": "pixel",
        "lines": [
          {
            "boundingBox": [
              67,
              646,
              2582,
              713,
              2580,
              876,
              67,
              821
            ],
            "text": "The quick brown fox jumps",
            "words": [
              {
                "boundingBox": [
                  143,
                  650,
                  435,
                  661,
                  436,
                  823,
                  144,
                  824
                ],
                "text": "The",
                "confidence": 0.958
              }
            ]
          }
        ]
      }
    ]
  }
}
```

### <a name="handwritten-classification-for-text-lines-latin-languages-only"></a>テキスト行の手書き分類 (ラテン言語のみ)
[Read 3.2 プレビュー API](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005) 応答には、各テキスト行が手書きスタイルであるかどうかと、信頼度スコアが分類されます。 この機能は、ラテン語系の言語でのみサポートされています。 次の例は、画像内のテキストの手書き分類を示しています。

:::image border type="content" source="../Images/ocr-handwriting-classification.png" alt-text="OCR の手書き分類の例":::

## <a name="next-steps"></a>次の手順

REST API を使用する場合は、[Read API リファレンス](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-preview-3/operations/5d986960601faab4bf452005)を参照してください。