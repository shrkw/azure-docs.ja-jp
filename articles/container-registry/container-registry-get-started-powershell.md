---
title: クイック スタート - レジストリを作成する - PowerShell
description: Azure Container Registry で PowerShell を使用してプライベート Docker レジストリを作成する方法を簡単に説明します
ms.topic: quickstart
ms.date: 01/22/2019
ms.custom: seodec18, mvc, devx-track-azurepowershell
ms.openlocfilehash: b6928f1c45cdac93b70797daf41205b4c5db27e0
ms.sourcegitcommit: b8995b7dafe6ee4b8c3c2b0c759b874dff74d96f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106283820"
---
# <a name="quickstart-create-a-private-container-registry-using-azure-powershell"></a>クイック スタート:Azure PowerShell を使用してプライベート コンテナー レジストリを作成する

Azure Container Registry は、Docker コンテナー イメージのビルド、保管、サポートをするための、管理されたプライベート Docker コンテナー レジストリ サービスです。 このクイックスタートでは、PowerShell を使用して Azure Container Registry を作成する方法を簡単に説明します。 次に、Docker コマンドを使用してコンテナー イメージをレジストリにプッシュし、最後にレジストリからイメージをプルして実行します。

## <a name="prerequisites"></a>前提条件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

このクイック スタートには、Azure PowerShell モジュールが必要です。 `Get-Module -ListAvailable Az` を実行して、インストールされたバージョンを判断します。 インストールまたはアップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/install-az-ps)に関するページを参照してください。

Docker もローカルにインストールする必要があります。 Docker には [macOS][docker-mac]、[Windows][docker-windows]、[Linux][docker-linux] システム用のパッケージがあります。

Azure Cloud Shell には、必要な Docker コンポーネント (`dockerd` デーモン) すべてが含まれていないため、このクイックスタートで Cloud Shell を使用することはできません。

## <a name="sign-in-to-azure"></a>Azure へのサインイン

[Connect-AzAccount][Connect-AzAccount] コマンドで Azure サブスクリプションにサインインし、画面上の指示に従います。

```powershell
Connect-AzAccount
```

## <a name="create-resource-group"></a>リソース グループの作成

Azure での認証が済んだら、[New-AzResourceGroup][New-AzResourceGroup] を使用してリソース グループを作成します。 リソース グループとは、Azure リソースのデプロイと管理に使用する論理コンテナーです。

```powershell
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-container-registry"></a>コンテナー レジストリを作成する

次に、[New-AzContainerRegistry][New-AzContainerRegistry] コマンドを使用して新しいリソース グループにコンテナー レジストリを作成します。

レジストリの名前は Azure 内で一意にする必要があります。また、5 ～ 50 文字の英数字を含める必要があります。 次の例では、「myContainerRegistry007」という名前のレジストリを作成しています。 以下のコマンドで *myContainerRegistry007* を置換後実行して、レジストリを作成します。

```powershell
$registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

このクイック スタートでは、*Basic* レジストリを作成します。これは、Azure Container Registry について学習している開発者にとって、コストが最適なオプションです。 利用可能なサービス レベルの詳細については、[コンテナー レジストリのサービス レベル][container-registry-skus]に関するページを参照してください。

## <a name="log-in-to-registry"></a>レジストリへのログイン

コンテナー イメージをプッシュしたりプルしたりするには、あらかじめレジストリにログインしておく必要があります。 このクイックスタートでは、作業を簡略化するために、[Get-AzContainerRegistryCredential][Get-AzContainerRegistryCredential] コマンドを使用して、レジストリ上の管理者ユーザーを有効にします。 運用環境のシナリオでは、レジストリ アクセス用に別の[認証方法](container-registry-authentication.md) (サービス プリンシパルなど) を使用する必要があります。 

```powershell
$creds = Get-AzContainerRegistryCredential -Registry $registry
```

次に、[docker login][docker-login] を実行してログインします。

```powershell
$creds.Password | docker login $registry.LoginServer -u $creds.Username --password-stdin
```

このコマンドは、完了すると `Login Succeeded` を返します。

> [!TIP]
> Azure CLI には、`az acr login` コマンドが用意されています。このコマンドは、docker 資格情報を渡さずに[個別の ID](container-registry-authentication.md#individual-login-with-azure-ad) を使用してコンテナー レジストリにログインするための便利な方法です。


[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>リソースをクリーンアップする

このクイック スタートで作成したリソースでの作業が完了したら、[Remove-AzResourceGroup][Remove-AzResourceGroup] コマンドを使用して、リソース グループ、コンテナー レジストリ、そこに格納されているコンテナー イメージを削除します。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>次のステップ

このクイック スタートでは、Azure PowerShell を使って Azure Container Registry を作成し、コンテナー イメージをプッシュしてから、レジストリからイメージをプルして実行しました。 Azure Container Registry のチュートリアルに進んで、ACR についての理解を深めましょう。

> [!div class="nextstepaction"]
> [Azure Container Registry のチュートリアル][container-registry-tutorial-prepare-registry]

> [!div class="nextstepaction"]
> [Azure Container Registry タスクのチュートリアル][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Get-AzContainerRegistryCredential]: /powershell/module/az.containerregistry/get-azcontainerregistrycredential
[Get-Module]: /powershell/module/microsoft.powershell.core/get-module
[New-AzContainerRegistry]: /powershell/module/az.containerregistry/New-AzContainerRegistry
[New-AzResourceGroup]: /powershell/module/az.resources/new-azresourcegroup
[Remove-AzResourceGroup]: /powershell/module/az.resources/remove-azresourcegroup
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[container-registry-tutorial-prepare-registry]: container-registry-tutorial-prepare-registry.md
