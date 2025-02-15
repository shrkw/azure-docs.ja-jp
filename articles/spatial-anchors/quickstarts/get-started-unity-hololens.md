---
title: クイック スタート:Unity を使用して HoloLens アプリを作成する
description: このクイック スタートでは、Spatial Anchors を使用して HoloLens アプリを Unity でビルドする方法について説明します。
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 03/18/2021
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: b99c2efcb746a056adc76fdc161c8d8a8e2799e1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670122"
---
# <a name="quickstart-create-a-unity-hololens-app-that-uses-azure-spatial-anchors"></a>クイック スタート:Azure Spatial Anchors を使用する Unity HoloLens アプリを作成する

このクイックスタートでは、[Azure Spatial Anchors](../overview.md) を使用する Unity HoloLens アプリを作成します。 Spatial Anchors は、クロスプラットフォーム対応の開発者向けサービスです。このサービスを使用すると、複数のデバイス間でその位置を長期にわたって保持するオブジェクトを使用して複合現実エクスペリエンスを作成できます。 完了すると、Unity でビルドされた、空間アンカーを保存して再呼び出しできる HoloLens アプリが作成されます。

学習内容は次のとおりです。

- Spatial Anchors アカウントを作成する。
- Unity のビルド設定を準備する。
- Spatial Anchors アカウント識別子とアカウント キーを構成する。
- HoloLens Visual Studio プロジェクトをエクスポートする。
- HoloLens デバイスにアプリをデプロイして実行する。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>前提条件

このクイック スタートを完了するには、以下が必要です。

- <a href="https://unity3d.com/get-unity/download" target="_blank">Unity (LTS)</a> および <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> 以降がインストールされている Windows コンピューターが必要です。 **Unity 2020 LTS** を ASA SDK バージョン 2.9 以降 ([Unity XR Plug-in Framework](https://docs.unity3d.com/Manual/XRPluginArchitecture.html) が使用される) で、または **Unity 2019 LTS** を ASA SDK バージョン 2.8 以前で使用します。 Visual Studio インストールには、**ユニバーサル Windows プラットフォーム開発** ワークロードと **Windows 10 SDK (10.0.18362.0 以降)** コンポーネントが含まれている必要があります。 <a href="https://git-scm.com/download/win" target="_blank">Git for Windows</a> および <a href="https://git-lfs.github.com/">Git LFS</a> もインストールする必要があります。
- [開発者モード](/windows/mixed-reality/using-visual-studio)が有効になっている HoloLens デバイスが必要です。 [Windows 10 May 2020 Update](/windows/mixed-reality/whats-new/release-notes-may-2020) がデバイスにインストールされている必要があります。 HoloLens を最新のリリースに更新するには、 **[設定]** アプリを開き、 **[更新とセキュリティ]** を選択し、 **[更新プログラムの確認]** を選択します。
- アプリで **SpatialPerception** 機能を有効にする必要があります。 この設定は、 **[ビルド設定]**  >  **[プレーヤーの設定]**  >  **[発行の設定]**  >  **[機能]** にあります。
- アプリで **[Virtual Reality Supported]\(サポートされている仮想現実\)** を **Windows Mixed Reality SDK** と共に有効にする必要があります。 この設定は、 **[ビルド設定]**  >  **[プレーヤーの設定]**  >  **[XR Settings]\(XR 設定\)** にあります。

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-and-open-the-unity-sample-project"></a>Unity のサンプル プロジェクトをダウンロードして開く

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

[!INCLUDE [Open Unity Project](../../../includes/spatial-anchors-open-unity-project.md)]

**[ファイル]**  >  **[ビルド設定]** を選択して、 **[ビルド設定]** を開きます。

**[プラットフォーム]** セクションで、 **[ユニバーサル Windows プラットフォーム]** を選択します。 **[Target Device]\(ターゲット デバイス\)** を **[HoloLens]** に変更します。

**[Switch Platform]\(プラットフォームの切り替え\)** を選択して、プラットフォームを **[ユニバーサル Windows プラットフォーム]** に変更します。 UWP をサポートするコンポーネントが不足している場合は、Unity によってそれらをインストールすることを要求される場合があります。

![Unity の [Build Settings]\(ビルド設定\) ウィンドウ](./media/get-started-unity-hololens/unity-build-settings.png)

**[ビルド設定]** ウィンドウを閉じます。

[!INCLUDE [Configure Unity Scene](../../../includes/spatial-anchors-unity-configure-scene.md)]

## <a name="export-the-hololens-visual-studio-project"></a>HoloLens Visual Studio プロジェクトをエクスポートする

[!INCLUDE [Export Unity Project](../../../includes/spatial-anchors-unity-export-project-snip.md)]

**[Build]\(ビルド\)** を選択します。 ダイアログ ボックスで、HoloLens Visual Studio プロジェクトのエクスポート先となるフォルダーを選択します。

エクスポートが完了すると、エクスポートされた HoloLens プロジェクトを含むフォルダーが表示されます。

## <a name="deploy-the-hololens-application"></a>HoloLens アプリケーションをデプロイする

フォルダー内の **HelloAR U3D.sln** をダブルクリックして、Visual Studio でプロジェクトを開きます。

**[ソリューション構成]** を **[リリース]** に変更し、 **[ソリューション プラットフォーム]** を **[x86]** に変更し、デプロイ ターゲット オプションから **[デバイス]** を選択します。

HoloLens 2 を使用している場合は、**x86** の代わりに **ARM64** を **[ソリューション プラットフォーム]** として使用します。

   ![Visual Studio の構成](./media/get-started-unity-hololens/visual-studio-configuration.png)

HoloLens デバイスの電源をオンにしてサインインし、USB ケーブルを使用してデバイスを PC に接続します。

**[デバッグ]**  >  **[デバッグの開始]** を選択して、アプリのデプロイとデバッグを開始します。

アプリで矢印を使用して **[BasicDemo]** を選択し、 **[実行]**  ボタンを 押してデモを実行します。 指示に従って、アンカーを配置し、呼び戻します。

![スクリーンショット 1](./media/get-started-unity-hololens/screenshot-1.jpg)
![スクリーンショット 2](./media/get-started-unity-hololens/screenshot-2.jpg)
![スクリーンショット 3](./media/get-started-unity-hololens/screenshot-3.jpg)
![スクリーンショット 4](./media/get-started-unity-hololens/screenshot-4.jpg)

Visual Studio で、 **[デバッグの停止]** を選択するか、Shift + F5 キーを押して、アプリを停止します。

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [チュートリアル:デバイス間で空間アンカーを共有する](../tutorials/tutorial-share-anchors-across-devices.md)

> [!div class="nextstepaction"]
> [方法: Unity プロジェクトで Azure Spatial Anchors を構成する](../how-tos/setup-unity-project.md)