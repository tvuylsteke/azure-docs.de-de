---
title: Übersicht über die Durable Functions-Versionen – Azure Functions
description: Informationen über Durable Functions-Versionen
author: cgillum
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: azfuncdf
ms.openlocfilehash: 4a117e7f69647af3ad82f9013bfa40556ccc0dbd
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152889"
---
# <a name="durable-functions-versions-overview"></a>Übersicht über die Durable Functions-Versionen

*Durable Functions* ist eine Erweiterung von [Azure Functions](../functions-overview.md) und [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Umgebung schreiben können. Die Erweiterung verwaltet Status, Prüfpunkte und Neustarts für Sie. Wenn Sie nicht bereits mit Durable Functions vertraut sind, lesen Sie die [Übersichtsdokumentation](durable-functions-overview.md).

## <a name="new-features-in-2x"></a>Neue Features in 2.x

In diesem Abschnitt werden die Features von Durable Functions beschrieben, die in Version 2.x hinzugefügt wurden.

### <a name="durable-entities"></a>Dauerhafte Entitäten

In Durable Functions 2.x haben wir ein neues Konzept für [Entitätsfunktionen](durable-functions-entities.md) eingeführt.

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsteile, bekannt als *dauerhafte Entitäten*. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Im Gegensatz zu Orchestratorfunktionen müssen Entitätsfunktionen keine spezifischen Codeeinschränkungen besitzen. Entitätsfunktionen verwalten Zustände auch explizit, statt Zustände implizit durch die Ablaufsteuerung darzustellen.

Weitere Informationen finden Sie im Artikel [Dauerhafte Entitäten](durable-functions-entities.md).

### <a name="durable-http"></a>Durable HTTP

In Durable Functions 2.x haben wir ein neues Feature [Durable HTTP](durable-functions-http-features.md#consuming-http-apis) eingeführt, das Folgendes ermöglicht:

* Direktes Aufrufen von HTTP-APIs aus Orchestrierungsfunktionen (es gelten einige dokumentierte Einschränkungen)
* Implementieren automatischer clientseitiger HTTP 202-Statusabfragen
* Integrierte Unterstützung für [verwaltete Azure-Identitäten](../../active-directory/managed-identities-azure-resources/overview.md)

Weitere Informationen finden Sie im Artikel zu den [HTTP-Funktionen](durable-functions-http-features.md#consuming-http-apis).

## <a name="migrate-from-1x-to-2x"></a>Migration von 1.x zu 2.x

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene Durable Functions-Version 1.x zu Version 2.x migrieren, um die neuen Funktionen zu nutzen.

### <a name="upgrade-the-extension"></a>Upgrade der Erweiterung

Installieren Sie Version 2.x der [Durable Functions-Bindungserweiterung](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) in Ihrem Projekt. Siehe hierzu [Registrieren von Bindungserweiterungen von Azure Functions](../functions-bindings-register.md).

### <a name="update-your-code"></a>Aktualisieren Ihres Codes

In Durable Functions 2.x werden verschiedene Breaking Changes eingeführt. Vorhandene Durable Functions 1.x-Anwendungen sind ohne Codeänderungen nicht mit Durable Functions 2.x kompatibel. In diesem Abschnitt werden einige der Änderungen aufgeführt, die Sie beim Upgrade Ihrer Funktionen der Version 1.x auf die Version 2.x vornehmen müssen.

#### <a name="hostjson-schema"></a>host.json-Schema

Durable Functions 2.x verwendet ein neues host.json-Schema. Zu den wichtigsten Änderungen gegenüber 1.x gehören:

* `"storageProvider"` (und Unterabschnitt `"azureStorage"`) für die speicherspezifische Konfiguration
* `"tracing"` für die Konfiguration von Nachverfolgung und Protokollierung
* `"notifications"` (und Unterabschnitt `"eventGrid"`) für die Konfiguration von Event Grid-Benachrichtigungen

Ausführliche Informationen finden Sie in der [Referenzdokumentation für die host.json-Datei von Durable Functions](durable-functions-bindings.md#durable-functions-2-0-host-json).

#### <a name="default-taskhub-name-changes"></a>Standardnamensänderungen für Aufgabenhubs

Wenn in Version 1.x in der Datei „host.json“ kein Aufgabenhubname angegeben wurde, wurde standardmäßig „DurableFunctionsHub“ verwendet. In Version 2.x wird der Standardname für Aufgabenhubs jetzt vom Namen der Funktions-App abgeleitet. Aus diesem Grund gilt Folgendes: Wenn Sie beim Upgrade auf 2.x keinen Aufgabenhubnamen angegeben haben, wird für Ihren Code ein neuer Aufgabenhub verwendet, und alle ausgeführten Orchestrierungen verfügen nicht mehr über eine Anwendung für die Verarbeitung. Als Problemumgehung können Sie entweder Ihren Aufgabenhubnamen explizit auf den Standardnamen „DurableFunctionsHub“ aus v1.x festlegen, oder Sie können sich an die Anleitung in unserem [Leitfaden zur Bereitstellung ohne Ausfallzeit](durable-functions-zero-downtime-deployment.md) halten, um wichtige Änderungen für ausgeführte Orchestrierungen zu verarbeiten.

#### <a name="public-interface-changes-net-only"></a>Änderungen an der öffentlichen Schnittstelle (nur .NET)

In Version 1.x umfassen die verschiedenen _context_-Objekte, die von Durable Functions unterstützt werden, abstrakte Basisklassen für die Verwendung bei Komponententests. In Durable Functions 2.x werden diese abstrakten Basisklassen durch Schnittstellen ersetzt.

In der folgenden Tabelle werden die Hauptänderungen dargestellt:

| 1.x | 2.x |
|----------|----------|
| `DurableOrchestrationClientBase` | `IDurableOrchestrationClient` oder `IDurableClient` |
| `DurableOrchestrationContext` oder `DurableOrchestrationContextBase` | `IDurableOrchestrationContext` |
| `DurableActivityContext` oder `DurableActivityContextBase` | `IDurableActivityContext` |
| `OrchestrationClientAttribute` | `DurableClientAttribute` |

In dem Fall, wo eine abstrakte Basisklasse virtuelle Methoden enthielt, wurden diese virtuellen Methoden durch Erweiterungsmethoden ersetzt, die in `DurableContextExtensions` definiert sind.

#### <a name="functionjson-changes-javascript-and-c-script"></a>function.json-Änderungen (JavaScript und C#-Skript)

In Durable Functions 1.x verwendet die Clientorchestrierungsbindung einen `type` von `orchestrationClient`. Version 2.x verwendet stattdessen `durableClient`.
