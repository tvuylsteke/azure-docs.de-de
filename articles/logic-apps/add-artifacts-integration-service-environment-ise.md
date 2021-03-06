---
title: Hinzufügen von Artefakten zu Integrationsdienstumgebungen
description: Hinzufügen von Logik-Apps, Integrationskonten, benutzerdefinierten Connectors und verwalteten Connectors zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE)
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: e2505d8ee8b8539f158c0a549bedfcd69a954e24
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77191670"
---
# <a name="add-artifacts-to-your-integration-service-environment-ise-in-azure-logic-apps"></a>Hinzufügen von Artefakten zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps

Fügen Sie nach der Erstellung einer [Integrationsdienstumgebung (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) Artefakte wie Logik-Apps, Integrationskonten und Connectors hinzu, damit diese auf die Ressourcen in Ihrem virtuellen Azure-Netzwerk zugreifen können. Beispielsweise werden verwaltete ISE-Connectors, die nach dem Erstellen der ISE verfügbar werden, nicht automatisch im Logik-App-Designer angezeigt. Bevor Sie diese ISE-Connectors verwenden können, müssen Sie [diese Connectors manuell Ihrer ISE hinzufügen und bereitstellen](#add-ise-connectors-environment), damit Sie im Logik-App-Designer angezeigt werden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die ISE, die Sie für die Ausführung Ihrer Logik-Apps erstellt haben. Besitzen Sie keine ISE, [erstellen Sie zuerst eine](../logic-apps/connect-virtual-network-vnet-isolated-environment.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-apps"></a>Erstellen von Logik-Apps

Führen Sie die folgenden Schritte aus, um Logik-Apps zu erstellen, die in Ihrer Integrationsdienstumgebung (ISE) ausgeführt werden:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Logik-Apps** > **Hinzufügen** aus.

   ![Hinzufügen einer neuen Logik-App zur ISE](./media/add-artifacts-integration-service-environment-ise/add-logic-app-to-ise.png)

1. Stellen Sie Informationen zu der Logik-App bereit, die Sie erstellen möchten, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-logic-app-integration-service-environment.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name** | Ja | Der Name der zu erstellenden Logik-App. |
   | **Abonnement** | Ja | Der Name des zu verwendenden Azure-Abonnements |
   | **Ressourcengruppe** | Ja | Der Name der (neuen oder vorhandenen) zu verwendenden Ressourcengruppe in Azure. |
   | **Location** | Ja | Wählen Sie unter **Integrationsdienstumgebungen** die zu verwendende ISE aus, sofern diese nicht bereits ausgewählt ist. <p><p> **Wichtig**: Um Ihre Logik-Apps mit einem Integrationskonto zu verwenden, müssen beide dieselbe ISE verwenden. |
   ||||

1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

1. Fahren Sie wie gewohnt mit der [Erstellung Ihrer Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) fort.

   Informationen zu den Unterschieden, wie Trigger und Aktionen funktionieren und wie sie bezeichnet werden, wenn Sie eine Integrationsdienstumgebung verwenden, im Vergleich zum globalen Logic Apps-Dienst, finden Sie unter [„Isoliert“ gegenüber „Global“ in der Übersicht zur Integrationsdienstumgebung](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#difference).

1. Informationen zum Verwalten von Logik-Apps und API-Verbindungen in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-accounts"></a>Erstellen von Integrationskonten

Basierend auf der während der Erstellung gewählten [ISE-SKU](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level) umfasst Ihre ISE die Nutzung des jeweiligen Integrationskontos ohne weitere Kosten. Logik-Apps, die in einer Integrationsdienstumgebung (ISE) enthalten sind, können nur auf Integrationskonten verweisen, die sich in derselben ISE befinden. Damit ein Integrationskonto mit Logik-Apps in einer ISE funktioniert, muss sowohl für das Integrationskonto als auch für die Logik-Apps *dieselbe Umgebung* als Speicherort verwendet werden. Weitere Informationen zu Integrationskonten und ISEs finden Sie unter [Integrationskonten mit ISE](connect-virtual-network-vnet-isolated-environment-overview.md#create-integration-account-environment).

Führen Sie die folgenden Schritte aus, um ein Integrationskonto zu erstellen, das eine ISE verwendet:

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Integrationskonten** > **Hinzufügen** aus.

   ![Hinzufügen eines neuen Integrationskontos zur ISE](./media/add-artifacts-integration-service-environment-ise/add-integration-account-to-ise.png)

1. Stellen Sie Informationen zu der Logik-App bereit, die Sie erstellen möchten, z. B.:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-integration-account-integration-service-environment.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name** | Ja | Der Name des zu erstellenden Integrationskontos. |
   | **Abonnement** | Ja | Der Name des zu verwendenden Azure-Abonnements. |
   | **Ressourcengruppe** | Ja | Der Name der (neuen oder vorhandenen) zu verwendenden Ressourcengruppe in Azure. |
   | **Preisstufe** | Ja | Der für das Integrationskonto zu verwendende Tarif. |
   | **Location** | Ja | Wählen Sie unter **Integrationsdienstumgebungen** dieselbe ISE aus, die Ihre Logik-Apps verwenden, sofern diese nicht bereits ausgewählt ist. <p><p> **Wichtig**: Um Ihr Integrationskonto mit Logik-Apps zu verwenden, müssen beide dieselbe ISE verwenden. |
   ||||

1. Wählen Sie **Erstellen**, wenn Sie fertig sind.

1. [Verknüpfen Sie Ihre Logik-App wie gewohnt mit Ihrem Integrationskonto.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. Fügen Sie Ihrem Integrationskonto anschließend Artefakte hinzu, etwa [Parteien](../logic-apps/logic-apps-enterprise-integration-partners.md) und [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md).

1. Informationen zum Verwalten von Integrationskonten in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

<a name="add-ise-connectors-environment"></a>

## <a name="add-ise-connectors"></a>Hinzufügen von ISE-Connectors

Von Microsoft verwaltete ISE-Connectors, die nach dem Erstellen der ISE verfügbar werden, werden nicht automatisch in der Connectorauswahl im Logik-App-Designer angezeigt. Bevor Sie diese ISE-Connectors verwenden können, müssen Sie diese Connectors manuell Ihrer ISE hinzufügen und bereitstellen, damit Sie im Logik-App-Designer angezeigt werden.

1. Wählen Sie im ISE-Menü unter **Einstellungen** die Option **Verwaltete Connectors** aus. Wählen Sie auf der Symbolleiste **Hinzufügen** aus.

   ![Anzeigen verwalteter Connectors](./media/add-artifacts-integration-service-environment-ise/ise-view-managed-connectors.png)

1. Öffnen Sie im Bereich **Neuen verwalteten Connector hinzufügen** die Liste **Connector suchen**. Wählen Sie den ISE-Connector aus, den Sie verwenden möchten, der aber noch nicht in Ihrer ISE bereitgestellt ist. Klicken Sie auf **Erstellen**.

   ![Wählen Sie den ISE-Connector aus, den Sie in Ihrer ISE bereitstellen möchten.](./media/add-artifacts-integration-service-environment-ise/add-managed-connector.png)

   Nur ISE-Connectors, die berechtigt, aber noch nicht in Ihrer ISE bereitgestellt sind, werden Ihnen zur Auswahl angezeigt. Connectors, die bereits in der ISE bereitgestellt wurden, sind für die Auswahl nicht verfügbar.

<a name="create-custom-connectors-environment"></a>

## <a name="create-custom-connectors"></a>Erstellen benutzerdefinierter Connectors

Wenn Sie benutzerdefinierte Connectors in ihrer ISE verwenden möchten, erstellen Sie diese direkt in der ISE.

1. Suchen Sie nach Ihrer ISE, und öffnen Sie sie, falls dies erforderlich ist. Wählen Sie im ISE-Menü unter **Einstellungen** die Optionen **Benutzerdefinierte Connectors** > **Hinzufügen** aus.

   ![Erstellen eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/add-custom-connector-to-ise.png)

1. Geben Sie den Namen, das Azure-Abonnement und die Azure-Ressourcengruppe (neu oder vorhanden) an, die Sie für den benutzerdefinierten Connector verwenden möchten.

1. Wählen Sie in der Liste **Speicherort** im Abschnitt **Integrationsdienstumgebungen** dieselbe ISE aus, die von Ihren Logik-Apps verwendet wird, und wählen Sie **Erstellen** aus. Beispiel:

   ![Auswählen der Integrationsdienstumgebung](./media/add-artifacts-integration-service-environment-ise/create-custom-connector-integration-service-environment.png)

1. Wählen Sie den neuen benutzerdefinierten Connector und anschließend **Bearbeiten** aus. Beispiel:

   ![Auswählen und Bearbeiten eines benutzerdefinierten Connectors](./media/add-artifacts-integration-service-environment-ise/edit-custom-connectors.png)

1. Erstellen Sie anschließend den Connector wie gewohnt auf der Grundlage einer [OpenAPI-Definition](https://docs.microsoft.com/connectors/custom-connectors/define-openapi-definition#import-the-openapi-definition) oder eines [SOAP](https://docs.microsoft.com/connectors/custom-connectors/create-register-logic-apps-soap-connector#2-define-your-connector).

1. Informationen zum Verwalten von benutzerdefinierten Connectors in Ihrer ISE finden Sie unter [Manage your integration service environment (ISE) in Azure Logic Apps](../logic-apps/ise-manage-integration-service-environment.md) (Verwalten Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps.)

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Integrationsdienstumgebungen](../logic-apps/ise-manage-integration-service-environment.md)
