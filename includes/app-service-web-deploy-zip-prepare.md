---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/14/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 6b5aa4f409b8c2f5a9125ab01e8587f4ac9c4ee5
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "75945162"
---
## <a name="create-a-project-zip-file"></a>Erstellen einer ZIP-Datei für das Projekt

>[!NOTE]
> Wenn Sie die Dateien in einer ZIP-Datei heruntergeladen haben, extrahieren Sie diese zunächst. Wenn Sie z.B. eine ZIP-Datei von GitHub heruntergeladen haben, können Sie diese nicht ohne Anpassungen bereitstellen. GitHub fügt zusätzliche geschachtelte Verzeichnisse hinzu, die in App Service nicht funktionieren. 
>

Navigieren Sie in einem lokalen Terminalfenster zum Stammverzeichnis Ihres App-Projekts. 

Dieses Verzeichnis sollte die Eingabedatei für Ihre Web-App enthalten, z.B. _index.html_, _index.php_ oder _app.js_. Sie kann auch Dateien zur Paketverwaltung enthalten, z.B. _project.json_, _composer.json_, _package.json_, _bower.json_ oder _requirements.txt_.

Wenn Sie nicht möchten, dass App Service die Bereitstellungsautomatisierung für Sie ausführt, führen Sie alle Buildaufgaben (z. B. `npm`, `bower`, `gulp`, `composer` und `pip`) aus, und stellen Sie sicher, dass Sie über alle Dateien verfügen, die Sie zum Ausführen der App benötigen. Dieser Schritt ist erforderlich, wenn Sie [Ihr Paket direkt ausführen möchten](../articles/app-service/deploy-run-package.md).

Erstellen Sie ein ZIP-Archiv mit allen Elementen Ihres Projekts. Mit dem folgenden Befehl wird das Standardtool in Ihrem Terminal verwendet:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

