---
title: IoT プラグ アンド プレイ認定の要件
description: IoT プラグ アンド プレイ認定プログラムの要件
author: cbroad
ms.author: cbroad
ms.topic: conceptual
ms.date: 03/15/2021
ms.custom: IoT Plug and Play Certification Requirements
ms.service: certification
ms.openlocfilehash: bbe9a3f18463285521dde0ee64b369cffcd71d75
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2021
ms.locfileid: "105975744"
---
# <a name="iot-plug-and-play-certification-requirements"></a>IoT プラグ アンド プレイ認定の要件

このドキュメントでは、Azure IoT デバイス カタログで表されるデバイス固有の機能について説明します。 機能とは、ソフトウェアの実装またはソフトウェアとハードウェアの実装の組み合わせである場合がある、単一のデバイス属性です。

## <a name="program-purpose"></a>プログラムの目的

IoT プラグ アンド プレイ プレビューにより、ソリューション ビルダーは、手動で構成することなく、自分のソリューションにスマート デバイスを統合することができます。 IoT プラグ アンド プレイの中核となるのは、デバイスが自身の機能を IoT プラグ アンド プレイ対応アプリケーションに公開するために使用するデバイス モデルです。 このモデルは、一連の要素 (テレメトリ、プロパティ、コマンド) として構成されています。

IoT プラグ アンド プレイ認定では次のことが保証されます。

1.  定義済みのデバイス モデルとインターフェイスは、[デジタル ツイン定義言語](https://github.com/Azure/opendigitaltwins-dtdl)に準拠している  
2.  デバイス プロビジョニング サービスのセキュリティで保護されたプロビジョニングと ID スコープの所有権の転送を容易にする
3.  [デジタル ツイン API](../iot-pnp/concepts-digital-twin.md) を使用した Azure IoT ベースのソリューションとの簡単な統合: Azure IoT Hub と Azure IoT Central
4.  認定デバイスで検証された製品の真実性

## <a name="requirements"></a>必要条件

**[必須] デバイスからクラウドへ: テストの目的は、テレメトリを送信するデバイスが IoT Hub で動作することを確認することです**

| **名前**                | IoTPnP.D2C                                               |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | リーフ デバイス/エッジ デバイス                                      |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | デバイスは IoT Hub にテレメトリ スキーマを送信する必要があります。 Microsoft では、テストを実行するための[ポータル ワークフロー](https://certify.azure.com)を提供しています。 デバイスからクラウドへ (必須): **1.** デバイスが AIC で管理されている IoT Hub にメッセージを送信できることを検証します **2.** ユーザーは、メッセージの数と頻度を指定する必要があります。 **3.** AIC は、ハブ インスタンスによってテレメトリが受信されたことを検証します |
| **リソース**           | [認定手順](./overview.md) (すべての追加リソースを含む) |

**[必須] DPS: テストの目的は、3 つの認証メソッドのいずれかを使用して、デバイスが IoT Hub Device Provisioning Service を実装およびサポートしていることを確認することです**

| **名前**                | IoTPnP.DPS                                               |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | 任意のデバイス                                                   |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | デバイスは、埋め込みコードを再コンパイルする必要なく、DPS ID スコープの所有権の転送を実装する必要があります。 Microsoft では、デバイスが DPS をサポートしていることを検証するために、テストを実行するための [ポータル ワークフロー](https://certify.azure.com)を提供しています **1.** ユーザーは、構成証明メソッド (x.509、TPM、および SAS キー) のいずれかを選択する必要があります **2.** 認証方法に応じて、ユーザーは、**a)** X.509 証明書を AICS マネージド DPS スコープにアップロードする、**b)** SAS キーまたは保証キーをデバイスに実装する、などの対応するアクションを実行する必要があります |
| **リソース**           | **a)** [デバイス プロビジョニング サービスの概要](../iot-dps/about-iot-dps.md)、 **b)** [DPS の ID スコープ転送用のサンプル構成ファイル](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview-pnp/digitaltwin_client/samples/digitaltwin_sample_ll_device/sample_config) |

**[必須] DTDL v2: 定義済みのデバイスモデルとインターフェイスがデジタル ツイン定義言語 v2 に準拠していることを確認するテストの目的。**                                                              

| **名前**                | IoTPnP.DTDL                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | 任意のデバイス                                                   |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | [ポータルのワークフロー](https://certify.azure.com)では、次のことが検証されます。 **1.** モデル ID のアナウンスと、デバイスが MQTT または MQTT over WebSockets プロトコルを使用して接続されていることを確認する **2.** モデルは DTDL v2 に準拠している **3.** テレメトリ、プロパティ、およびコマンドが正しく実装され、デバイス上の IoT Hub デジタル ツインとデバイス ツインの間で対話する |
| **リソース**           | [パブリック プレビュー更新版の更新内容](../iot-pnp/overview-iot-plug-and-play-preview-updates.md) |

**[必須] デバイス モデルはパブリック モデル リポジトリに発行されます**

| **名前**                | IoTPnP.ModelRepo                                             |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | 任意のデバイス                                                   |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | すべてのデバイス モデルは、パブリック リポジトリに発行される必要があります。 デバイス モデルは、パブリック リポジトリで利用可能なモデルを使用して解決されます **1.** ユーザーは、証明書を送信する前に、モデルをパブリック リポジトリに手動で発行する必要があります。 **2.** モデルは一度発行されると変更できないことに注意してください。 モデルと埋め込みデバイス コードが完成した場合にのみ発行することを強くお勧めします。*1 *1 ユーザーは、モデル リポジトリに発行された後に、Microsoft サポートに連絡してモデルを取り消す必要があります **3.** [ポータル ワークフロー](https://certify.azure.com) は、デバイスが証明書サービスに接続されている場合に、パブリック リポジトリ内のモデルの存在を確認します |
| **リソース**           | [モデル リポジトリ](../iot-pnp/overview-iot-plug-and-play-preview-updates.md) |

**[必須] GSG を使用した物理デバイスの検証**

| **名前**                                  | IoTPnP.Physicaldevice                                      |
| ----------------------------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性**                   | 現在利用可能                                                |
| **適用対象**                            | 任意のデバイス                                                   |
| **OS**                                    | 非依存                                                     |
| **検証タイプ**                       | マニュアル                                                       |
| **検証**                            | パートナーは、 物理デバイスに対して追加の検証を実行するための手配を行うために、Microsoft Contact ([iotcert@microsoft.com](mailto:iotcert@microsoft.com)) と提携する必要があります。 COVID-19 の状況のため、デバイスを Microsoft に出荷せずに、物理的なデバイスの検証を実行するさまざまな方法を模索しています。 |
| **リソース**                             | 詳細は後日お知らせします                                 |
| **Azure の推奨事項**       | N/A    |

**[実装されている場合] デバイス情報インターフェイス: テストの目的は、デバイス情報インターフェイスがデバイス コードに適切に実装されていることを検証することです**

| **名前**                | IoTPnP.DeviceInfoInterface                                   |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | 任意のデバイス                                                   |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | [ポータル ワークフロー](https://certify.azure.com) は、デバイス コードが [デバイス情報インターフェイスを実装していることを検証します](https://repo.azureiotrepository.com/Models/dtmi:azure:DeviceManagement:DeviceInformation;1?api-version=2020-05-01-previewureiot:DeviceManagement:DeviceInformation:1) **1.** デバイス コードによって IoT Hub に値が出力されることを確認します **2.** インターフェイスが DCM に実装されていることを確認します (この実装は DTDL v2 で変更されます) **3.** チェック プロパティが書き込み可能ではありません (読み取り専用) **4.** スキーマの種類が文字列または long であり、null ではないことを確認します |
| **リソース**           | [Microsoft によって定義されたインターフェイス](../iot-pnp/overview-iot-plug-and-play-preview-updates.md) |
| **Azure の推奨事項**  | N/A                                                          |

**[実装されている場合] クラウドからデバイス: テストの目的は、メッセージがクラウドからデバイスに送信されることを確認することです**

| **名前**                | IoTPnP.C2D                                               |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | リーフ デバイス/エッジ デバイス                                      |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | デバイスは、IoT Hub のクラウドからデバイスへのメッセージに対応できる必要があります。 Microsoft では、これらのテストを実行するための[ポータル ワークフロー](https://certify.azure.com)を提供しています。 クラウドからデバイス (実装されている場合): **1.** デバイスが IoT Hub からメッセージを受信できることを検証します **2.** AIC は、ランダムなメッセージを送信し、デバイスからのメッセージ ACK を使用して検証します |
| **リソース**           | **1.** [認定手順](./overview.md) (すべての追加リソースを含む)、**2.** [IoT Hub から、クラウドからデバイスへのメッセージを送信する](../iot-hub/iot-hub-devguide-messages-c2d.md) |

**[実装されている場合] ダイレクト メソッド: テストの目的は、デバイスが IoT Hub で動作し、ダイレクト メソッドをサポートしていることを確認することです**

| **名前**                | IoTPnP.DirectMethods                                     |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | リーフ デバイス/エッジ デバイス                                      |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | デバイスは、IoT Hub からのコマンド要求を受信して応答できる必要があります。 Microsoft では、テストを実行するための[ポータル ワークフロー](https://certify.azure.com)を提供しています。 ダイレクト メソッド (実装されている場合): **1.** ユーザーは、ダイレクト メソッドのメソッド ペイロードを指定する必要があります。 **2.** AICS は、指定されたペイロード要求が、デバイスによって受信されたハブと ACK メッセージから送信されたことを検証します |
| **リソース**           | **1.** [認定手順](./overview.md) (すべての追加リソースを含む)、**2.** [IoT Hub からのダイレクト メソッドについて](../iot-hub/iot-hub-devguide-direct-methods.md) |

**[実装されている場合] デバイス ツインのプロパティ: テストの目的は、テレメトリを送信するデバイスが IoT Hub で動作することを確認し、ダイレクト メソッドやデバイス ツインのプロパティなどの IoT Hub 機能の一部をサポートすることです**

| **名前**                | IoTPnP.DeviceTwin                                        |
| ----------------------- | ------------------------------------------------------------ |
| **ターゲットの可用性** | 現在利用可能                                                |
| **適用対象**          | リーフ デバイス/エッジ デバイス                                      |
| **OS**                  | 非依存                                                     |
| **検証タイプ**     | 自動                                                    |
| **検証**          | デバイスは IoT Hub にテレメトリ スキーマを送信する必要があります。 Microsoft では、テストを実行するための[ポータル ワークフロー](https://certify.azure.com)を提供しています。 デバイス ツインのプロパティ (実装されている場合): **1.** AICS は、デバイス ツインの JSON で読み取り/書き込み可能なプロパティを検証します **2.** ユーザーは、変更する JSON ペイロードを指定する必要があります **3.** AICS は、デバイスが受信した IoT Hub メッセージと ACK メッセージから送信された、指定された必要なプロパティを検証します |
| **リソース**           | **1.** [認定手順](./overview.md) (すべての追加リソースを含む)、**2.** [IoT Hub でデバイス ツインを使用する](../iot-hub/iot-hub-devguide-device-twins.md) |