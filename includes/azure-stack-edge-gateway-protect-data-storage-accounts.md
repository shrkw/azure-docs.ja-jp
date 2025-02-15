---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: 2e0a7ca8fb9eaafbc50c0ce60799dd68d83b2fa3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/29/2021
ms.locfileid: "98901033"
---
デバイスは、Azure 内のデータの宛先として使用されるストレージ アカウントに関連付けられています。 ストレージ アカウントへのアクセスは、各ストレージ アカウントに関連付けられたサブスクリプションと 2 つの 512 ビット ストレージ アクセス キーによって制御されます。

Data Box Edge デバイスがストレージ アカウントにアクセスするときに、いずれかのキーが認証に使用されます。 もう一方のキーは予備で保持されているため、それらのキーを定期的にローテーションできます。

セキュリティ上の理由から、多くのデータ センターでキーのローテーションが義務化されています。 キーのローテーションに関しては、以下のベスト プラクティスに従うようお勧めします。

- ストレージ アカウント キーは、ストレージ アカウントの root パスワードに似ています。 アカウント キーは慎重に保護してください。 このパスワードを他のユーザーに配布したり、ハード コードしたり、他のユーザーからアクセスできるプレーンテキストで保存したりしないでください。
- 侵害される可能性があると考えられる場合は、Azure Portal 経由で[アカウント キーを再生成](../articles/storage/common/storage-account-keys-manage.md#manually-rotate-access-keys)します。
- Azure 管理者は、Azure Portal の [ストレージ] セクションを使用してストレージ アカウントに直接アクセスすることにより、プライマリまたはセカンダリ キーを定期的に変更または再生成する必要があります。
