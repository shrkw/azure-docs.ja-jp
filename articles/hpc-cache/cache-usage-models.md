---
title: Azure HPC Cache の使用モデル
description: さまざまなキャッシュ使用モデルと、それらを選択する方法、および読み取り専用または読み取り/書き込みキャッシュを設定し、その他のキャッシュ設定を制御する方法について説明します。
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 03/15/2021
ms.author: v-erkel
ms.openlocfilehash: 3ad252520ca0cf7acdb3c84ef1da87c8076f3172
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2021
ms.locfileid: "104775716"
---
# <a name="understand-cache-usage-models"></a>キャッシュ使用モデルについて

キャッシュ使用モデルを使用すると、Azure HPC Cache でのファイルの保存方法をカスタマイズして、ワークフローを高速化できます。

## <a name="basic-file-caching-concepts"></a>基本的なファイル キャッシュの概念

ファイル キャッシュとは、Azure HPC Cache によるクライアント要求の迅速化です。 次の基本的な方法を使用します。

* **読み取りキャッシュ** - Azure HPC Cache は、クライアントがストレージ システムから要求したファイルのコピーを保持します。 次にクライアントが同じファイルを要求するときに、HPC キャッシュは、バックエンド ストレージ システムから再びフェッチするのではなく、キャッシュにバージョンを提供します。

* **書き込みキャッシュ** - 必要に応じて、クライアント コンピューターから送信された変更済みのファイルのコピーを Azure HPC Cache に格納できます。 複数のクライアントが短時間に同じファイルに変更を加えた場合、キャッシュは、各変更をバックエンド ストレージ システムに個別に書き込むのではなく、キャッシュ内のすべての変更を収集します。

  変更が行われないまま、指定した時間が経過すると、キャッシュはファイルを長期ストレージ システムに移動します。

  書き込みキャッシュが無効になっている場合、キャッシュは変更されたファイルを保存せず、直ちにバックエンド ストレージ システムに書き込みます。

* **ライトバック遅延** - ライトバック遅延は、書き込みキャッシュが有効になっているキャッシュについて、ファイルをバックエンド ストレージ システムにコピーする前に、キャッシュがファイルの追加変更が行われるまで待機する時間を意味します。

* **バックエンドの検証** - バックエンドの検証の設定では、キャッシュがファイルのローカル コピーをバックエンド ストレージ システムのリモート バージョンと比較する頻度を決定します。 バックエンド コピーがキャッシュされたコピーより新しい場合、キャッシュはリモート コピーをフェッチし、今後の要求のために保存します。

  バックエンド検証の設定は、キャッシュ内のファイルとリモート ストレージ内のソース ファイルとの *自動* 比較がどの時点で行われるかを示します。 ただし、readdirplus 要求を含むディレクトリ操作を実行すると、Azure HPC Cache でファイルを強制的に比較できます。 readdirplus は、ディレクトリ メタデータを返す標準的な NFS API です (拡張読み取りとも呼ばれます)。これにより、キャッシュでファイルが比較され更新されます。

Azure HPC Cache に組み込まれている使用モデルには、それぞれの状況に最適な組み合わせを選択できるように、これらの設定に対して異なる値が設定されています。

## <a name="choose-the-right-usage-model-for-your-workflow"></a>ワークフローに適した使用モデルを選択する

使用する NFS マウント ストレージ ターゲットごとに、使用モデルを選択する必要があります。 Azure Blob Storage ターゲットには、カスタマイズできない組み込みの使用モデルがあります。

HPC キャッシュの使用モデルでは、迅速な応答と古いデータを取得するリスクとのバランスを取る方法を選択できます。 ファイルの読み取り速度を最適化する場合、キャッシュ内のファイルがバックエンド ファイルと照合されるかどうかは重要でないかもしれません。 一方、リモート ストレージでファイルが常に最新の状態になっていることを確認する場合は、頻繁にチェックを行うモデルを選択します。

使用モデルのオプションは次のとおりです。

* **負荷が高く、頻度の低い書き込みを読み取ります** - 静的、またはほとんど変更されないファイルへの読み取りアクセスを速くする場合にこのオプションを使用します。

  このオプションでは、クライアントの読み取りがキャッシュされますが、書き込みはキャッシュされません。 書き込みが直後にバックエンド ストレージに渡されます。
  
  キャッシュに格納されているファイルが NFS ストレージ ボリューム上のファイルと自動的に比較されることはありません (手動で比較する方法については、前述のバックエンド検証の説明を参照してください。)

  最初にキャッシュに書き込まず、ストレージ システムで直接、ファイルが変更されるリスクがある場合、このオプションは使用しないでください。 これが発生すると、キャッシュされたバージョンのファイルは、バックエンド ファイルと非同期になります。

* **書き込みが 15% を超えています** - このオプションを使用すると、書き込みと読み取りの両方が速くなります。 このオプションを使用するとき、クライアントでは、バックエンド ストレージを直接マウントするのではなく、Azure HPC Cache 経由でファイルにアクセスする必要があります。 キャッシュされたファイルには、まだバックエンドにコピーされていない最近の変更があります。

  この使用モデルでは、8 時間ごとに、キャッシュ内のファイルがバックエンド ストレージのファイルと照合されます。 キャッシュされたバージョンのファイルの方が新しいと見なされます。 キャッシュ内の変更後のファイルは、キャッシュに 20 分間留まった後にバックエンド ストレージ システムに書き込まれます。<!-- an hour --> 追加の変更はありません。

* **クライアントは、キャッシュをバイパスして NFS ターゲットに書き込みます** - ワークフローのクライアントで、先にキャッシュに書き込まずにストレージ システムにデータが直接書き込まれる場合、またはデータの整合性を最適化する場合、このオプションを選択します。 クライアントが要求したファイルがキャッシュ (読み取り) されますが、クライアントによるそれらのファイルへの変更 (書き込み) はキャッシュされません。 これらは、バックエンド ストレージ システムに直接渡されます。

  この使用モデルでは、キャッシュ内のファイルについて、更新があるかどうかがバックエンド バージョンに対して頻繁に (30 秒ごとに) 確認されます。 この検証によって、キャッシュの外でファイルを変更することが可能になり、同時にデータの整合性が維持されます。

  > [!TIP]
  > これらの最初の 3 つの基本的な使用モデルを使用して、ほとんどの Azure HPC Cache ワークフローを処理できます。 次のオプションは、あまり一般的ではないシナリオを対象としています。

* **書き込みが 15% を超え、変更がないかについて 30 秒ごとにバッキング サーバーを確認**、および **書き込み 15% を超え、変更がないかについて 60 秒ごとにバッキング サーバーを確認** - これらのオプションは、読み取りと書き込みの両方を高速化するワークフロー用に設計されていますが、別のユーザーがバックエンド ストレージ システムに直接書き込む可能性があります。 たとえば、複数の一連のクライアントが同じファイルに対して別々の場所から作業している場合、これらの使用モデルは、ファイルにすばやくアクセスしながら、古いコンテンツに対するソースからのアクセスの許容度を低くするというバランスの取れた方法です。

* **書き込みが 15% を超え、30 秒ごとにサーバーに書き戻し** - このオプションは、複数のクライアントが同じファイルをアクティブに変更している場合、または一部のクライアントがキャッシュをマウントせずに直接バックエンド ストレージにアクセスする場合に適するように設計されています。

  頻繁なバックエンドの書き込みはキャッシュのパフォーマンスに影響するため、ファイルの競合リスクが低い場合 (複数のクライアントが同じファイル セットの異なる領域で作業していることがわかっている場合など) は、**書き込みが 15% を超えた場合** の使用モデルを検討してください。

* **読み取りの負荷が高く、3 時間ごとにバックアップ サーバーを確認** - このオプションは、クライアント側の高速読み取りを優先しますが、**読み取りの負荷が高く、書き込みの頻度が低い** 使用モデルとは異なり、キャッシュされたファイルをバックエンド ストレージ システムから定期的に更新します。

このテーブルは、使用モデルの違いをまとめたものです。

[!INCLUDE [usage-models-table.md](includes/usage-models-table.md)]

Azure HPC Cache ワークフローの最適な使用モデルについて不明な点がある場合は、Azure の担当者にお問い合わせいただくか、サポート要求をオープンしてください。

## <a name="next-steps"></a>次のステップ

* Azure HPC Cache に[ストレージ ターゲットを追加](hpc-cache-add-storage.md)する
