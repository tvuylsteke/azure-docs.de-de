---
title: Bewährte Methoden für Azure Functions
description: Enthält Beschreibungen der bewährten Methoden und Muster für Azure Functions.
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.topic: conceptual
ms.date: 12/17/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a41a5828a82d81c5e7e8749fee70cd15e17bb9d0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357998"
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Optimieren der Leistung und Zuverlässigkeit von Azure Functions

Dieser Artikel enthält Informationen zur Verbesserung der Leistung und Zuverlässigkeit Ihrer [serverlosen](https://azure.microsoft.com/solutions/serverless/) Funktionen-Apps.  

## <a name="general-best-practices"></a>Allgemeine bewährte Methoden

Nachfolgend finden Sie bewährte Methoden zum Entwickeln und Erstellen serverloser Lösungen mit Azure Functions.

### <a name="avoid-long-running-functions"></a>Vermeiden von Funktionen mit langer Ausführungsdauer

Umfangreiche Funktionen mit langer Ausführungsdauer können zu unerwarteten Zeitüberschreitungsfehlern führen. Weitere Informationen zu Timeouts für einen bestimmten Hostingplan finden Sie unter [Optimieren der Leistung und Zuverlässigkeit von Azure Functions](functions-scale.md#timeout). 

Eine Funktion kann umfangreich werden, wenn sie über viele Node.js-Abhängigkeiten verfügt. Zudem kann das Importieren von Abhängigkeiten zu längeren Ladezeiten und somit zu unerwarteten Timeouts führen. Abhängigkeiten werden sowohl explizit als auch implizit geladen. Ein einzelnes Modul, das über Ihren Code geladen wird, kann eigene zusätzliche Module laden. 

Nach Möglichkeit sollten Sie umfangreiche Funktionen durch Refactoring immer in kleinere Funktionssätze unterteilen, die zusammenarbeiten und schnelle Reaktionen ermöglichen. Für einen Webhook oder eine HTTP-Triggerfunktion ist unter Umständen eine Bestätigungsantwort innerhalb eines bestimmten Zeitraums erforderlich. Bei Webhooks ist üblicherweise eine unmittelbare Antwort notwendig. Sie können die HTTP-Triggernutzlast an eine Warteschlange übergeben, damit sie von einer Funktion des Warteschlangentriggers verarbeitet wird. Dieser Ansatz ermöglicht es Ihnen, die eigentliche Arbeit zurückzustellen und sofort eine Antwort zurückzugeben.


### <a name="cross-function-communication"></a>Funktionsübergreifende Kommunikation

[Durable Functions](durable/durable-functions-overview.md) und [Azure Logic Apps](../logic-apps/logic-apps-overview.md) dienen zum Verwalten der Statusübergänge und der Kommunikation zwischen mehreren Funktionen.

Wenn weder Durable Functions noch Logic Apps zum Integrieren mit mehreren Funktionen verwendet werden, haben sich Speicherwarteschlangen für die funktionsübergreifende Kommunikation bewährt. Der Hauptgrund ist, dass Speicherwarteschlangen kostengünstiger und deutlich einfacher als andere Speicheroptionen bereitzustellen sind. 

Einzelne Nachrichten in einer Speicherwarteschlange sind auf eine Größe von 64 KB beschränkt. Wenn Sie größere Nachrichten zwischen Funktionen übergeben müssen, kann eine Azure Service Bus-Warteschlange verwendet werden, um Nachrichtengrößen von bis zu 256 KB im Standard-Tarif und bis zu 1 MB im Premium-Tarif zu unterstützen.

Service Bus-Themen sind nützlich, wenn die Nachrichten vor der Verarbeitung gefiltert werden sollen.

Event Hubs sind hilfreich, um die Kommunikation mit hohen Volumina zu unterstützen.


### <a name="write-functions-to-be-stateless"></a>Schreiben von zustandslosen Funktionen 

Funktionen sollten nach Möglichkeit zustandslos und idempotent sein. Ordnen Sie Ihren Daten alle erforderlichen Zustandsinformationen zu. Einer Bestellung, die verarbeitet wird, ist beispielsweise meist ein `state`-Member zugeordnet. Eine Funktion kann eine Bestellung basierend auf diesem Zustand verarbeiten, während die Funktion selbst zustandslos bleibt. 

Idempotente Funktionen sind besonders bei Triggern mit Timer zu empfehlen. Wenn bei Ihnen beispielsweise eine bestimmte Komponente immer ein Mal am Tag ausgeführt werden muss, sollten Sie sie so schreiben, dass sie zu einer beliebigen Tageszeit ausgeführt werden kann und immer die gleichen Ergebnisse liefert. Die Funktion kann beendet werden, wenn für einen bestimmten Tag keine Arbeit vorhanden ist. Falls die letzte Ausführung nicht abgeschlossen werden konnte, sollte die nächste Ausführung am entsprechenden Punkt fortgesetzt werden.


### <a name="write-defensive-functions"></a>Schreiben von defensiven Funktionen

Gehen Sie davon aus, dass es für Ihre Funktion jederzeit zu einer Ausnahme kommen kann. Entwerfen Sie Ihre Funktionen so, dass bei der nächsten Ausführung an einem vorherigen Fehlerpunkt angeknüpft werden kann. Stellen Sie sich ein Szenario mit den folgenden Aktionen vor:

1. Abfrage von 10.000 Zeilen in einer Datenbank.
2. Erstellen Sie eine Warteschlangennachricht für jede Zeile, um die spätere Verarbeitung zu ermöglichen.
 
Je nach Komplexität Ihres Systems verfügen Sie ggf. über Folgendes: fehlerhaftes Verhalten von nachgelagerten Diensten, Netzwerkausfälle, Erreichung von Kontingentgrenzen usw. Alle diese Faktoren können sich jederzeit auf Ihre Funktion auswirken. Sie müssen Ihre Funktionen entsprechend entwerfen, um darauf vorbereitet zu sein.

Wie reagiert Ihr Code, wenn nach dem Einfügen von 5.000 dieser Elemente in eine Warteschlange zur Verarbeitung ein Fehler auftritt? Verfolgen Sie, welche Elemente eines Satzes bereits abgeschlossen sind. Andernfalls kann es ein, dass Sie sie beim nächsten Mal erneut einfügen. Diese doppelte Einfügung kann ernsthafte Auswirkung auf Ihren Workflow haben, sodass Sie [idempotente](functions-idempotent.md) Funktionen verwenden sollten. 

Wenn ein Warteschlangenelement bereits verarbeitet wurde, sollte es möglich sein, dass die Funktion eine No-Op-Funktion ist.

Nutzen Sie Verteidigungsmaßnahmen, die für auf der Azure Functions-Plattform verwendete Komponenten bereits bereitgestellt wurden. Informationen hierzu finden Sie beispielsweise in der [Dokumentation zu Azure Storage-Warteschlangentriggern und -bindungen](functions-bindings-storage-queue-trigger.md#poison-messages) unter **Behandeln von Nachrichten in der Warteschlange für nicht verarbeitbare Nachrichten**. 

## <a name="scalability-best-practices"></a>Skalierbarkeit: Bewährte Methoden

Zahlreiche Faktoren beeinflussen die Skalierung von Instanzen Ihrer Funktions-App. Ausführliche Informationen finden Sie in der Dokumentation zum [Skalieren von Funktionen-Apps](functions-scale.md).  Hier finden Sie bewährten Methoden, um die optimale Skalierbarkeit einer Funktionen-App sicherzustellen.

### <a name="share-and-manage-connections"></a>Freigeben und Verwalten von Verbindungen

Verwenden Sie Verbindungen mit externen Ressourcen nach Möglichkeit wieder. Weitere Informationen finden Sie unter [Verwalten von Verbindungen in Azure Functions](./manage-connections.md).

### <a name="avoid-sharing-storage-accounts"></a>Vermeiden der Freigabe von Speicherkonten

Wenn Sie eine Funktions-App erstellen, müssen Sie sie einem Speicherkonto zuordnen. Die Speicherkontoverbindung wird in der Anwendungseinstellung [AzureWebJobsStorage](./functions-app-settings.md#azurewebjobsstorage) verwaltet. 

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Vermeiden Sie es, Test- und Produktionscodes in der derselben Funktionen-App zu mischen.

Für Funktionen innerhalb einer Funktionen-App werden Ressourcen gemeinsam genutzt. Dies gilt beispielsweise für den Arbeitsspeicher. Wenn Sie eine Funktionen-App in der Produktion verwenden, sollten Sie ihr keine testbezogenen Funktionen und Ressourcen hinzufügen. Bei der Ausführung des Produktionscodes kann dies zu unerwartetem Mehraufwand führen.

Überlegen Sie sich gut, was Sie in Ihre Funktionen-Apps für die Produktion laden. Der Arbeitsspeicher wird gleichmäßig auf die einzelnen Funktionen der App verteilt.

Wenn Sie eine gemeinsame Assembly nutzen, auf die in mehreren .NET-Funktionen verwiesen wird, sollten Sie sie in einem freigegebenen Ordner einfügen. Andernfalls ist es leicht möglich, dass Sie versehentlich mehrere Versionen der gleichen Binärdatei bereitstellen, die sich für die einzelnen Funktionen unterschiedlich verhalten.

Verwenden Sie im Produktionscode keine ausführliche Protokollierung, da sich dies negativ auf die Leistung auswirkt.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Verwenden von asynchronem Code bei Vermeidung von blockierenden Aufrufen

Die asynchrone Programmierung ist eine empfohlene bewährte Vorgehensweise, insbesondere dann, wenn blockierende E/A-Vorgänge beteiligt sind.

Vermeiden Sie in C# stets Verweise auf die `Result`-Eigenschaft oder Aufrufe der `Wait`-Methode für eine `Task`-Instanz. Dieser Ansatz kann zur Threadauslastung führen.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="use-multiple-worker-processes"></a>Verwenden mehrerer Workerprozesse

Standardmäßig verwendet jede Hostinstanz für Functions einen einzelnen Workerprozess. Um die Leistung zu verbessern, insbesondere bei Single Thread-Runtimes wie Python, verwenden Sie [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count), um die Anzahl von Workerprozessen pro Host zu erhöhen (bis zu 10). Azure Functions versucht dann, gleichzeitige Funktionsaufrufe gleichmäßig auf diese Worker zu verteilen. 

Die FUNCTIONS_WORKER_PROCESS_COUNT gilt für jeden Host, der von Functions erstellt wird, wenn Ihre Anwendung horizontal skaliert wird, um die Anforderungen zu erfüllen. 

### <a name="receive-messages-in-batch-whenever-possible"></a>Empfangen von Nachrichten in Batches (sofern möglich)

Einige Trigger wie Event Hub ermöglichen das Erhalten von mehreren Nachrichten in einem einzigen Aufruf.  Die Batchverarbeitung von Nachrichten ermöglicht eine viel bessere Leistung.  Sie können die maximale Batchgröße in der Datei `host.json` wie in der [Dokumentation zur host.json-Referenz](functions-host-json.md) konfigurieren.

Bei C#-Funktionen können Sie den Typ in ein stark typisiertes Array ändern.  Beispielsweise könnte die Methodensignatur `EventData sensorEvent` statt `EventData[] sensorEvent` lauten.  Bei anderen Sprachen müssen Sie die Kardinalitätseigenschaft explizit in Ihrer `function.json` auf `many` festlegen, um die Batchverarbeitung [wie hier gezeigt](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10) zu aktivieren.

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Konfigurieren des Host-Verhaltens zum besseren Verwalten der Parallelität

Die Datei `host.json` in der Funktionen-App ermöglicht die Konfiguration der Host-Laufzeit und des Triggerverhaltens.  Zusätzlich zur Batchverarbeitung von Verhalten können Sie die Parallelität für mehrere Trigger verwalten. Eine häufige Anpassung der Werte in diesen Optionen kann die Skalierung der Instanz an die Anforderungen der aufgerufenen Funktionen vereinfachen.

Die Einstellungen in der Datei „host.json“ gelten für alle Funktionen innerhalb der App in einer *Einzelinstanz* der Funktion. Wenn Sie eine Funktionen-App mit zwei HTTP-Funktionen und [`maxConcurrentRequests`](functions-bindings-http-webhook-output.md#hostjson-settings)-Anforderungen auf 25 festlegen, zählt eine Anforderung für einen der HTTP-Trigger zu den 25 gemeinsamen parallelen Anforderungen.  Beim Skalieren dieser Funktions-App auf 10 Instanzen erlauben die beiden Funktionen effektiv 250 parallele Anforderungen (10 Instanzen * 25 gleichzeitige Anforderungen pro Instanz). 

Weitere Hostkonfigurationsoptionen finden Sie [im Artikel zur host.json-Konfiguration](functions-host-json.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Verwalten von Verbindungen in Azure Functions](manage-connections.md)
* [Azure App Service – bewährte Methoden](../app-service/app-service-best-practices.md)
