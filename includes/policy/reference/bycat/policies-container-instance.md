---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 8ca15686659a7fb9bd1390df7d9af041805df7ee
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2021
ms.locfileid: "106091000"
---
|名前<br /><sub>(Azure portal)</sub> |説明 |効果 |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Container Instance コンテナー グループを仮想ネットワークにデプロイする必要がある](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8af8f826-edcb-4178-b35f-851ea6fea615) |Azure 仮想ネットワークを使用して、コンテナー間の通信をセキュリティで保護します。 仮想ネットワークを指定すると、仮想ネットワーク内のリソースが相互に安全かつプライベートに通信できるようになります。 |Audit, Disabled, Deny |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Instance/ContainerInstance_VNET_Audit.json) |
|[Azure Container Instance コンテナー グループでは、暗号化にカスタマー マネージド キーを使用する必要がある](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0aa61e00-0a01-4a3c-9945-e93cffedf0e6) |カスタマー マネージド キーを使用して、コンテナーをより柔軟にセキュリティで保護します。 カスタマー マネージド キーを指定すると、データを暗号化するキーへのアクセスを保護および制御するために、そのキーが使用されます。 カスタマー マネージド キーを使用すると、キー暗号化キーのローテーションを制御したり、データを暗号的に消去したりするための追加機能が提供されます。 |Audit, Disabled, Deny |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Instance/ContainerInstance_CMK_Audit.json) |
