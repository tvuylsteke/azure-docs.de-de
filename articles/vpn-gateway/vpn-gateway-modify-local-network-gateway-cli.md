---
title: 'VPN Gateway: Ändern der Einstellungen für die Gateway-IP-Adresse: Azure-Befehlszeilenschnittstelle'
description: In diesem Artikel wird erläutert, wie Sie die IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe der Azure CLI ändern.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: bc051a7e0a19dc54431266cfa5f37131868bdc07
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75864042"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe der Azure CLI

Manchmal ändern sich die Einstellungen für das Adresspräfix oder die Gateway-IP-Adresse Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch über eine andere Option aus der folgenden Liste ändern:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Voraussetzungen

Installieren Sie die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zur Installation der CLI-Befehle finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="ipaddprefix"></a>Ändern von IP-Adresspräfixen

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="gwip"></a>Ändern der Gateway-IP-Adresse

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).

