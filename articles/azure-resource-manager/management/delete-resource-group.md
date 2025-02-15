---
title: リソース グループとリソースを削除する
description: リソース グループとリソースを削除する方法について説明します。 ここでは、リソース グループを削除するときに、Azure Resource Manager によってリソースの削除がどのように並べ替えられるかについて説明します。 応答コードと、削除が成功したかを判断するために Resource Manager によって応答コードが処理される方法について説明します。
ms.topic: conceptual
ms.date: 03/18/2021
ms.custom: seodec18
ms.openlocfilehash: 8dd6c3f9ac178a518545a662eb94d33066f7614b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2021
ms.locfileid: "105932728"
---
# <a name="azure-resource-manager-resource-group-and-resource-deletion"></a>Azure Resource Manager のリソース グループとリソースの削除

この記事では、リソース グループとリソースを削除する方法について示します。 ここでは、リソース グループを削除するときに、Azure Resource Manager によってリソースの削除がどのように並べ替えられるかについて説明します。

## <a name="how-order-of-deletion-is-determined"></a>削除の順序が決定される方法

リソース グループを削除する際に、Resource Manager によってリソースを削除する順序が決定されます。 次の順序が使用されます。

1. すべての子 (入れ子) リソースが削除されます。

2. 次に、他のリソースを管理するリソースが削除されます。 リソースには、別のリソースによって管理されていることを示す `managedBy` プロパティが設定されている場合があります。 このプロパティが設定されている場合、他のリソースを管理しているリソースは、その管理対象リソースより前に削除されます。

3. 残りのリソースは、前の 2 つのカテゴリより後に削除されます。

順序が決定された後、Resource Manager によって、各リソースに対して DELETE 操作が発行されます。 先に進む前に、すべての依存関係が完了するまで待ちます。

同期操作の場合、期待される正常な応答コードは次のとおりです。

* 200
* 204
* 404

非同期操作の場合、期待される正常な応答は 202 です。 Resource Manager では、location ヘッダーまたは azure-async 操作ヘッダーが追跡されて、非同期の削除操作の状態が確認されます。
  
### <a name="deletion-errors"></a>削除エラー

削除操作からエラーが返された場合、Resource Manager によって DELETE 呼び出しが再試行されます。 再試行は、5xx、429、408 の状態コードに対して行われます。 既定では、再試行の間隔は 15 分です。

## <a name="after-deletion"></a>削除後

Resource Manager では、削除が試行されるリソースごとに GET 呼び出しが発行されます。 この GET 呼び出しの応答として、404 が期待されます。 Resource Manager が 404 を受け取った場合は、削除が正常に完了したと見なされます。 Resource Manager によって、そのキャッシュからリソースが削除されます。

一方、リソースに対する GET 呼び出しで 200 または 201 が返された場合は、Resource Manager によってリソースが再作成されます。

GET 操作からエラーが返された場合、次のエラー コードであれば Resource Manager によって GET が再試行されます。

* 100 未満
* 408
* 429
* 500 より大きい

その他のエラー コードの場合は、Resource Manager によるリソースの削除が失敗します。

> [!IMPORTANT]
> リソース グループの削除は元に戻すことができません。

## <a name="delete-resource-group"></a>リソース グループの削除

リソース グループを削除するには、次のいずれかのメソッドを使用します。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResourceGroup -Name ExampleResourceGroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group delete --name ExampleResourceGroup
```

# <a name="portal"></a>[ポータル](#tab/azure-portal)

1. [ポータル](https://portal.azure.com)で、削除するリソース グループを選択します。

1. **[リソース グループの削除]** を選択します。

   ![リソース グループの削除](./media/delete-resource-group/delete-group.png)

1. 削除を確認するには、リソース グループの名前を入力します。

---

## <a name="delete-resource"></a>Delete resource

リソースを削除するには、次のいずれかのメソッドを使用します。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResource `
  -ResourceGroupName ExampleResourceGroup `
  -ResourceName ExampleVM `
  -ResourceType Microsoft.Compute/virtualMachines
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az resource delete \
  --resource-group ExampleResourceGroup \
  --name ExampleVM \
  --resource-type "Microsoft.Compute/virtualMachines"
```

# <a name="portal"></a>[ポータル](#tab/azure-portal)

1. [ポータル](https://portal.azure.com)で、削除するリソースを選択します。

1. **[削除]** を選択します。 次のスクリーンショットには、仮想マシンの管理オプションが表示されています。

   ![Delete resource](./media/delete-resource-group/delete-resource.png)

1. メッセージが表示されたら、削除を確定します。

---

## <a name="required-access"></a>必要なアクセス

リソース グループを削除するには、**Microsoft.Resources/subscriptions/resourceGroups** リソースの削除アクションにアクセスできる必要があります。 また、リソース グループ内のすべてのリソースに対して削除を行う必要もあります。

操作の一覧については、「[Azure リソース プロバイダーの操作](../../role-based-access-control/resource-provider-operations.md)」を参照してください。 組み込みロールの一覧については、[Azure の組み込みロール](../../role-based-access-control/built-in-roles.md)に関するページを参照してください。

必要なアクセス権を持っていても、削除要求が失敗した場合は、リソース グループが [ロック](lock-resources.md) されているおそれがあります。

## <a name="next-steps"></a>次のステップ

* Resource Manager の概念を理解するには、「[Azure Resource Manager の概要](overview.md)」をご覧ください。
* 削除コマンドについては、[PowerShell](/powershell/module/az.resources/Remove-AzResourceGroup)、[Azure CLI](/cli/azure/group#az-group-delete)、[REST API](/rest/api/resources/resources/resourcegroups/delete) をご覧ください。
