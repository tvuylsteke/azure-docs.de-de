---
title: Continuous Integration und Continuous Deployment
description: Professionelle DevOps-Datenbankumgebung für SQL Data Warehouse mit integrierter Unterstützung für Continuous Integration und Continuous Deployment mithilfe von Azure Pipelines
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: integration
ms.date: 08/28/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: a8178e5ff9ff4816ddd422d3c45cfc0e1e0b3d41
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76712984"
---
# <a name="continuous-integration-and-deployment-for-azure-sql-data-warehouse"></a>Continuous Integration und Continuous Deployment für Azure SQL Data Warehouse

In diesem einfachen Tutorial wird erläutert, wie Sie Ihr SSDT-Datenbankprojekt (SQL Server Data Tools) in Azure DevOps integrieren und Azure Pipelines zum Einrichten von Continuous Integration und Continuous Deployment nutzen. Dieses Tutorial ist der zweite Schritt beim Erstellen Ihrer Continuous Integration- und Continuous Deployment-Pipeline mit SQL Data Warehouse. 

## <a name="before-you-begin"></a>Voraussetzungen

- Lesen Sie das Tutorial zur [Integration der Quellcodeverwaltung](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-source-control-integration).

- Einrichten von Azure DevOps und Herstellen einer Verbindung


## <a name="continuous-integration-with-visual-studio-build"></a>Continuous Integration mit Visual Studio Build

1. Navigieren Sie zu Azure Pipelines, und erstellen Sie eine neue Buildpipeline.

      ![Neue Pipeline](media/sql-data-warehouse-continuous-integration-and-deployment/1-new-build-pipeline.png "Neue Pipeline")

2. Wählen Sie Ihr Quellcoderepository (Azure Repos Git) und anschließend die .NET-Desktop-App-Vorlage aus.

      ![Pipelinesetup](media/sql-data-warehouse-continuous-integration-and-deployment/2-pipeline-setup.png "Pipelinesetup") 

3. Bearbeiten Sie Ihre YAML-Datei, damit der richtige Pool Ihres Agents verwendet wird. Ihre YAML-Datei sollte in etwa wie folgt aussehen:

      ![YAML](media/sql-data-warehouse-continuous-integration-and-deployment/3-yaml-file.png "YAML")

Nun verfügen Sie über eine einfache Umgebung, in der jeder Check-In bei Ihrem Quellcoderepository-Masterbranch automatisch einen erfolgreichen Visual Studio-Build Ihres Datenbankprojekts auslösen sollte. Überprüfen Sie, ob die Automatisierung insgesamt funktioniert, indem Sie eine Änderung an Ihrem lokalen Datenbankprojekt vornehmen und diese Änderung beim Masterbranch einchecken.


## <a name="continuous-deployment-with-the-azure-sql-data-warehouse-or-database-deployment-task"></a>Continuous Deployment mit der Bereitstellungsaufgabe für Azure SQL Data Warehouse (oder SQL-Datenbank)

1. Fügen Sie mithilfe der [Azure SQL-Datenbank-Bereitstellungsaufgabe](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/sql-azure-dacpac-deployment?view=azure-devops) eine neue Aufgabe hinzu, und füllen Sie die erforderlichen Felder aus, um eine Verbindung mit dem Ziel-Data Warehouse herzustellen. Wenn diese Aufgabe ausgeführt wird, wird die DACPAC-Datei, die vom vorherigen Buildprozess generiert wurde, auf dem Ziel-Data Warehouse bereitgestellt. Sie können auch die [Azure SQL Data Warehouse-Bereitstellungsaufgabe](https://marketplace.visualstudio.com/items?itemName=ms-sql-dw.SQLDWDeployment) verwenden. 

      ![Bereitstellungsaufgabe](media/sql-data-warehouse-continuous-integration-and-deployment/4-deployment-task.png "Bereitstellungsaufgabe")

2. Stellen Sie bei Verwendung eines selbstgehosteten Agents sicher, dass Sie die Umgebungsvariable so festlegen, dass sie die richtige Version von „SqlPackage.exe“ für SQL Data Warehouse verwendet. Der Pfad sollte in etwa wie folgt aussehen:

      ![Umgebungsvariable](media/sql-data-warehouse-continuous-integration-and-deployment/5-environment-variable-preview.png "Umgebungsvariable")

   C:\Programme (x86)\Microsoft Visual Studio\2019\Preview\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\150  

   Führen Sie Ihre Pipeline aus, und überprüfen Sie sie. Sie können Änderungen lokal vornehmen und Änderungen an der Quellcodeverwaltung einchecken, die eine automatische Erstellung und Bereitstellung generiert.

## <a name="next-steps"></a>Nächste Schritte

- Erkunden der [Azure SQL Data Warehouse-Architektur](massively-parallel-processing-mpp-architecture.md)
- Schnelles [Erstellen einer SQL Data Warehouse-Instanz](create-data-warehouse-portal.md)
- [Laden von Stichprobendaten](sql-data-warehouse-load-sample-databases.md)
- Ansehen von [Videos](/azure/sql-data-warehouse/sql-data-warehouse-videos)
