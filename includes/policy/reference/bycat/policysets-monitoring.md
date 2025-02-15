---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1dab7150da65708512872facc9e972811c78cb5d
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2021
ms.locfileid: "106090056"
---
|名前 |説明 |ポリシー |Version |
|---|---|---|---|
|[\[プレビュー\]: デプロイ - 仮想マシン上の Azure Monitor と Azure Security エージェントを有効にするために前提条件を構成する](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Azure Monitor および Azure セキュリティ エージェントを自動的にインストールするようにマシンを構成します。 Security Center では、エージェントからイベントを収集し、それらを使用してセキュリティ アラートと調整されたセキュリティ強化タスク (推奨事項) を提供します。 監査レコードを保存するマシンと同じリージョンに、リソース グループと Log Analytics ワークスペースを作成します。 このポリシーは、少数のリージョンの VM にのみ適用されます。 |5 |1.0.0-preview |
|[仮想マシン スケール セットに対して Azure Monitor を有効にする](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |指定されたスコープ (管理グループ、サブスクリプション、またはリソース グループ) 内の仮想マシン スケール セットに対して Azure Monitor を有効にします。 Log Analytics ワークスペースをパラメーターとして取得します。 注: お使いのスケール セット upgradePolicy が手動に設定されている場合、セット内のすべての VM でアップグレードを呼び出して拡張機能を適用することが必要になります。 CLI では、これは az vmss update-instances になります。 |6 |1.0.1 |
|[VM 用 Azure Monitor を有効にする](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |指定されたスコープ (管理グループ、サブスクリプション、リソース グループ) 内の仮想マシン (VM) に対して Azure Monitor を有効にします。 Log Analytics ワークスペースをパラメーターとして取得します。 |10 |2.0.0 |
