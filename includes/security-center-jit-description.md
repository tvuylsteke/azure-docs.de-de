---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 02/24/2020
ms.topic: include
ms.openlocfilehash: c77849b2285283a34e6adf84dc3845a4076407af
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597938"
---
## <a name="attack-scenario"></a>Angriffsszenario

Bei Brute-Force-Angriffen wird im Allgemeinen versucht, über Verwaltungsports Zugriff auf einen virtuellen Computer zu erlangen. Im Erfolgsfall kann ein Angreifer die Kontrolle über den virtuellen Computer erlangen und in Ihrer Umgebung Fuß fassen.

Eine Möglichkeit, die Gefährdung durch Brute-Force-Angriffe zu verringern, ist das Beschränken der Zeitspanne, in der ein Port geöffnet ist. Verwaltungsports müssen nicht jederzeit geöffnet sein. Sie müssen nur geöffnet sein, während eine Verbindung mit dem virtuellen Computer besteht, z.B. zum Ausführen von Verwaltungs- oder Wartungsaufgaben. Wenn Just-In-Time aktiviert ist, verwendet Security Center [Netzwerksicherheitsgruppen](../articles/virtual-network/security-overview.md#security-rules)-Regeln (NSG) und Azure Firewall-Regeln, die den Zugriff auf Verwaltungsports beschränken, um sie vor Angriffen zu schützen.

![Just-In-Time-Szenario](../articles/security-center/media/security-center-just-in-time/just-in-time-scenario.png)

## <a name="how-does-jit-access-work"></a>Wie funktioniert JIT-Zugriff?

Wenn Just-In-Time aktiviert ist, sperrt das Security Center durch das Erstellen einer NSG-Regel eingehenden Datenverkehr auf den Azure-VMs. Sie wählen die Ports auf dem virtuellen Computer aus, für die eingehender Datenverkehr gesperrt wird. Diese Ports werden durch die Just-In-Time-Lösung gesteuert.

Wenn ein Benutzer den Zugriff auf einen virtuellen Computer anfordert, überprüft das Security Center, ob der Benutzer über Berechtigungen der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../articles/role-based-access-control/role-assignments-portal.md) für diesen virtuellen Computer verfügt. Wird die Anforderung genehmigt, konfiguriert Security Center die Netzwerksicherheitsgruppen (NSGs) und Azure Firewall automatisch so, dass für die angegebene Zeitspanne eingehender Datenverkehr an die ausgewählten Ports und angeforderten Quell-IP-Adressen oder -Bereiche zugelassen wird. Nach Ablauf dieser Zeitspanne stellt das Security Center die vorherigen Status der NSGs wieder her. Die bereits eingerichteten Verbindungen werden jedoch nicht unterbrochen.

 > [!NOTE]
 > Wenn eine Just-In-Time-Zugriffsanforderung für einen von einer Azure Firewall geschützten virtuellen Computer genehmigt wird, ändert Security Center automatisch sowohl die Netzwerksicherheitsgruppe als auch Regeln für die Firewallrichtlinie. Für den angegebenen Zeitraum ermöglichen die Regeln eingehenden Datenverkehr an die ausgewählten Ports und angeforderte Quell-IP-Adressen oder -Bereiche. Nach Ablauf dieser Zeitspanne stellt Security Center die Firewall- und NSG-Regeln mit dem zuvor verwendeten Zustand wieder her.


## <a name="permissions-needed-to-configure-and-use-jit"></a>Berechtigungen, die zum Konfigurieren und Verwenden von JIT erforderlich sind

| Optionen, die Benutzern ermöglicht werden können: | Festzulegende Berechtigungen|
| --- | --- |
| Konfigurieren oder Bearbeiten einer JIT-Richtlinie für einen virtuellen Computer | *Weisen Sie der Rolle diese Aktionen zu:*  <ul><li>Im Bereich eines Abonnements oder einer Ressourcengruppe, das oder die dem virtuellen Computer zugeordnet ist:<br/> `Microsoft.Security/locations/jitNetworkAccessPolicies/write` </li><li> Im Bereich eines Abonnements oder einer Ressourcengruppe eines virtuellen Computers: <br/>`Microsoft.Compute/virtualMachines/write`</li></ul> | 
|Anfordern von JIT-Zugriff auf einen virtuellen Computer | *Weisen Sie dem Benutzer diese Aktionen zu:*  <ul><li>Im Bereich eines Abonnements oder einer Ressourcengruppe, das oder die dem virtuellen Computer zugeordnet ist:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action` </li><li>Im Bereich eines Abonnements oder einer Ressourcengruppe, das oder die dem virtuellen Computer zugeordnet ist:<br/>  `Microsoft.Security/locations/jitNetworkAccessPolicies/*/read` </li><li>  Im Bereich eines Abonnements, einer Ressourcengruppe oder eines virtuellen Computers:<br/> `Microsoft.Compute/virtualMachines/read` </li><li>  Im Bereich eines Abonnements, einer Ressourcengruppe oder eines virtuellen Computers:<br/> `Microsoft.Network/networkInterfaces/*/read` </li></ul>|