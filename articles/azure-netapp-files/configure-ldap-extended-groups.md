---
title: Azure NetApp Files NFS ボリューム アクセスに拡張グループで ADDS LDAP を構成する | Microsoft Docs
description: Azure NetApp Files を使用して NFS ボリュームを作成するときに、拡張グループで LDAP を有効にする際の考慮事項と手順について説明します。
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 04/05/2021
ms.author: b-juche
ms.openlocfilehash: 2031cbf07d700307ae1e11c516f9fc736bce5080
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498877"
---
# <a name="configure-adds-ldap-with-extended-groups-for-nfs-volume-access"></a>NFS ボリューム アクセスに拡張グループで ADDS LDAP を構成する

[NFS ボリュームを作成する](azure-netapp-files-create-volumes.md)ときに、ボリュームの拡張グループ機能 (**LDAP** オプション) を使用して LDAP を有効にするオプションがあります。 この機能により、Active Directory LDAP ユーザーおよび拡張グループ (最大1024 グループ) がボリュームにアクセスできるようになります。  

この記事では、NFS ボリュームの作成時に拡張グループで LDAP を有効にする際の考慮事項と手順について説明します。  

## <a name="considerations"></a>考慮事項

* Azure Active Directory Domain Services (AADDS) を使用している場合は、TLS 経由の LDAP を有効に *しない* でください。  

* 拡張グループ機能を使用して LDAP を有効にすると、LDAP が有効になっている [Kerberos ボリューム](configure-kerberos-encryption.md)では、LDAP ユーザーのファイル所有権が正しく表示されません。 LDAP ユーザーによって作成されたファイルまたはディレクトリは、実際の LDAP ユーザーではなく、既定で `root` が所有者として設定されます。 ただし、`root` アカウントでは、コマンド `chown <username> <filename>` を使用してファイルの所有権を手動で変更できます。 

* ボリュームを作成した後で、LDAP オプションの設定 (有効または無効) を変更することはできません。  

* 次の表では、LDAP キャッシュの Time to Live (TTL) 設定について説明します。 クライアントを使用してファイルまたはディレクトリにアクセスしようとする前に、キャッシュが更新されるまで待機する必要があります。 そうでないと、アクセス拒否メッセージがクライアントに表示されます。 

    |     エラー状態    |     解像度    |
    |-|-|
    | キャッシュ |  [Default Timeout]\(既定のタイムアウト\) |
    | グループ メンバーシップの一覧  | 24 時間の TTL  |
    | Unix グループ  | 24 時間の TTL、1 分の負の TTL  |
    | Unix ユーザー  | 24 時間の TTL、1 分の負の TTL  |

    キャッシュには、*Time to Live* と呼ばれる特定のタイムアウト期間があります。 タイムアウト期間が経過すると、古いエントリが残らないようにエントリが期限切れになります。 *負の TTL* の値は、存在しない可能性があるオブジェクトの LDAP クエリに起因するパフォーマンスの問題を回避するために、失敗した参照が存在する場所です。        

## <a name="steps"></a>手順

1. 拡張グループ機能を使用する LDAP は現在プレビューの段階です。 この機能を初めて使用する前に、機能を登録する必要があります。  

    1. 機能を登録します。   

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```

    2. 機能の登録の状態を確認します。 

        > [!NOTE]
        > **RegistrationState** が `Registering` 状態から `Registered` に変化するまでに最大 60 分間かかる場合があります。 この状態が `Registered` になってから続行してください。

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapExtendedGroups
        ```
        
    また、[Azure CLI のコマンド](/cli/azure/feature) `az feature register` と `az feature show` を使用して、機能を登録し、登録状態を表示することもできます。 

2. LDAP ボリュームには、LDAP サーバー設定の Active Directory 構成が必要です。 「[Active Directory 接続の要件](create-active-directory-connections.md#requirements-for-active-directory-connections)」と「[Active Directory 接続を作成する](create-active-directory-connections.md#create-an-active-directory-connection)」の手順に従い、Azure portal で Active Directory 接続を構成します。  

3. Active Directory で Active Directory LDAP サーバーが稼働していることを確認します。 そのためには、AD マシンで [Active Directory ライトウェイト ディレクトリ サービス (AD LDS)](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831593(v=ws.11)) ロールをインストールして構成します。

4. LDAP NFS ユーザーは、LDAP サーバー上で特定の POSIX 属性を持っている必要があります。 「[LDAP POSIX 属性を管理する](create-volumes-dual-protocol.md#manage-ldap-posix-attributes)」に従って、必要な属性を設定します。  

5. LDAP 統合 Linux クライアントを構成する場合は、「[Azure NetApp Files 用に NFS クライアントを構成する](configure-nfs-clients.md)」をご覧ください。

6.  「[Azure NetApp Files の NFS ボリュームを作成する](azure-netapp-files-create-volumes.md)」の手順に従って、NFS ボリュームを作成します。 ボリュームの作成処理中に、 **[プロトコル]** タブで **[LDAP]** オプションを有効にします。   

    ![[LDAP オプションを使用してボリューム ページを作成する] を示すスクリーンショット。](../media/azure-netapp-files/create-nfs-ldap.png)  

7. 省略可能 - Windows LDAP サーバー上に存在しないローカル NFS クライアント ユーザーが、拡張グループが有効になっている LDAP を持つ NFS ボリュームにアクセスできるようにすることができます。 これを行うには、次のように、 **[LDAP を使用するローカル NFS ユーザーを許可する]** オプションを有効にします。
    1. **[Active Directory 接続]** をクリックします。  既存の Active Directory 接続で、コンテキスト メニュー (3 つのドット `…`) をクリックし、 **[編集]** を選択します。  
    2. 表示される **[Active Directory 設定の編集]** ウィンドウで、 **[LDAP を使用するローカル NFS ユーザーを許可する]** オプションを選択します。  

    ![[LDAP を使用するローカル NFS ユーザーを許可する] オプションを示すスクリーンショット](../media/azure-netapp-files/allow-local-nfs-users-with-ldap.png)  

## <a name="next-steps"></a>次のステップ  

* [Azure NetApp Files の NFS ボリュームを作成する](azure-netapp-files-create-volumes.md)
* [Active Directory 接続の作成と管理](create-active-directory-connections.md)
* [LDAP ボリュームに関する問題のトラブルシューティング](troubleshoot-ldap-volumes.md)
