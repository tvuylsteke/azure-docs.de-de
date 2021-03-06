---
title: Verwalten von Arbeitsbereichen
titleSuffix: ML Studio (classic) - Azure
description: Verwalten des Zugriffs auf Azure Machine Learning Studio-Arbeitsbereiche (klassisch) und Bereitstellen sowie Verwalten von Machine Learning-API-Webdiensten
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: 996cf4451811b46a99e7c0b64ca0fa21c1eba5f9
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77168479"
---
# <a name="manage-an-azure-machine-learning-studio-classic-workspace"></a>Verwalten eines Azure Machine Learning Studio-Arbeitsbereichs (klassisch)

> [!NOTE]
> Informationen zum Verwalten von Webdiensten im Machine Learning Web Services-Portal finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).
> 
> 

Sie können Machine Learning Studio-Arbeitsbereiche (klassisch) im Azure-Portal verwalten.



## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

So verwalten Sie einen Studio-Arbeitsbereich (klassisch) im Azure-Portal

1. Melden Sie sich über das Administratorkonto eines Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.
2. Geben Sie oben auf der Seite im Suchfeld „Machine Learning Studio-Arbeitsbereiche (klassisch)“ ein, und wählen Sie dann **Machine Learning Studio-Arbeitsbereiche (klassisch)** aus.
3. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Neben den Standardinformationen und -optionen zur Ressourcenverwaltung stehen folgende Optionen zur Verfügung:

- **Eigenschaften anzeigen:** Auf dieser Seite werden Informationen zum Arbeitsbereich und der Ressource angezeigt. Zudem können Sie hier das Abonnement und die Ressourcengruppe ändern, mit denen der Arbeitsbereich verbunden ist.
- **Speicherschlüssel neu synchronisieren:** Im Arbeitsbereich werden die Schlüssel für das Speicherkonto verwaltet. Wenn sich die Schlüssel für das Speicherkonto ändern, können Sie auf **Schlüssel neu synchronisieren** klicken, um die Schlüssel mit dem Arbeitsbereich zu synchronisieren.

Verwenden Sie zum Verwalten der diesem Studio-Arbeitsbereich (klassisch) zugeordneten Webdienste das Machine Learning Web Services-Portal. Ausführliche Informationen finden Sie unter [Verwalten eines Webdiensts im Azure Machine Learning Web Services-Portal](manage-new-webservice.md).

> [!NOTE]
> Zum Bereitstellen oder Verwalten neuer Webdienste muss Ihnen in dem Abonnement, für das der Webdienst bereitgestellt wird, eine Teilnehmer- oder Administratorrolle zugewiesen sein. Wenn Sie andere Benutzer in einen Machine Learning Studio-Arbeitsbereich (klassisch) einladen, müssen Sie ihnen eine Teilnehmer- oder Administratorrolle im Abonnement zuweisen, damit sie Webdienste bereitstellen oder verwalten können. 
> 
>Weitere Informationen zum Festlegen von Zugriffsberechtigungen finden Sie unter [Verwalten des Zugriffs mit RBAC und dem Azure-Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Bereitstellen von Machine Learning mithilfe von Azure Resource Manager-Vorlagen](deploy-with-resource-manager-template.md). 
