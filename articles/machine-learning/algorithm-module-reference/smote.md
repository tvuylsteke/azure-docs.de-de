---
title: SMOTE
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das SMOTE-Modul in Azure Machine Learning verwenden, um die Anzahl von Beispielen mit geringem Vorkommen in einem Dataset mithilfe von Überquotierung (Oversampling) zu erhöhen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 4b6944f7703500a2c3859e8e3111eceefbd5ff10
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2020
ms.locfileid: "76311409"
---
# <a name="smote"></a>SMOTE

In diesem Artikel wird beschrieben, wie das SMOTE-Modul im Azure Machine Learning-Designer verwendet werden kann, um die Anzahl von unterrepräsentierten Fällen in einem Dataset zu erhöhen, das für maschinelles Lernen verwendet wird. SMOTE bietet eine bessere Möglichkeit, die Anzahl von seltenen Fälle zu erhöhen, statt einfach vorhandene Fälle zu duplizieren.  

Sie verbinden das SMOTE-Modul mit einem Dataset, das *unausgeglichen* ist. Es gibt viele Gründe, warum ein Dataset unausgeglichen sein kann. Beispielsweise kann es sein, dass die gewählte Kategorie in der Population selten oder die Erfassung der Daten schwierig ist. In der Regel verwenden Sie SMOTE, wenn die *Klasse*, die Sie analysieren möchten, unterrepräsentiert ist. 
  
Das Modul gibt ein Dataset mit den ursprünglichen Beispielen zurück. Je nachdem, welchen Prozentsatz Sie angeben, wird auch eine Reihe von „synthetischen Minderheitsstichproben“ zurückgegeben.  
  
## <a name="more-about-smote"></a>Weitere Informationen zu SMOTE

Synthetic Minority Oversampling Technique (SMOTE) ist ein statistisches Verfahren, um die Anzahl von Fällen in Ihrem Dataset auf ausgeglichene Weise zu erhöhen. Das Modul geht so vor, dass es neue Instanzen aus vorhandenen Minderheitsfällen erstellt, die Sie als Eingabe bereitstellen. Bei dieser Implementierung von SMOTE werden *keine* Änderungen an der Anzahl von Mehrheitsfällen vorgenommen.

Die neuen Instanzen sind nicht einfach Kopien von vorhandenen Minderheitsfällen. Stattdessen werden im Algorithmus Stichproben des *Merkmalsraums* für jede Zielklasse und deren nächste Nachbarn verwendet. Anschließend werden vom Algorithmus neue Beispiele generiert, in denen Merkmale des Zielfalls mit Merkmalen seiner Nachbarn kombiniert sind. Dieser Ansatz erhöht die für jede Klasse verfügbaren Merkmale und macht die Stichproben allgemeiner.
  
SMOTE übernimmt das gesamte Dataset als Eingabe, erhöht aber nur den Prozentsatz der Minderheitsfälle. Nehmen Sie beispielsweise an, Sie haben ein unausgewogenes Dataset, in dem nur 1 Prozent der Fälle den Zielwert A (die Minderheitsklasse) und 99 Prozent der Fälle den Wert B haben. Wenn Sie den Prozentsatz der Minderheitsfälle auf das Doppelte des vorherigen Prozentsatzes erhöhen möchten, geben Sie in den Eigenschaften des Moduls den Wert **200** unter **SMOTE percentage** (SMOTE-Prozentsatz) ein.  
  
## <a name="examples"></a>Beispiele  

Wir empfehlen Ihnen, SMOTE zu Testzwecken mit einem kleinen Dataset zu verwenden, um die Funktionsweise zu testen. Im folgenden Beispiel wird das Blood Donation-Dataset (Blutspende-Dataset) verwendet, das in Azure Machine Learning-Designer verfügbar ist.
  
Wenn Sie das Dataset einer Pipeline hinzufügen und in der Ausgabe des Datasets die Option **Visualisieren** auswählen, können Sie sehen, dass von den 748 Zeilen oder Fällen, die im Dataset enthalten sind, 570 Fälle (76 Prozent) zur Klasse 0 und 178 Fälle (24 Prozent) zur Klasse 1 gehören. Obwohl dies kein Ergebnis mit übermäßiger Unausgeglichenheit ist, entspricht Klasse 1 den Personen, die Blut gespendet haben. Daher enthalten diese Zeilen den *Merkmalsraum*, den Sie modellieren möchten.
 
Um die Anzahl von Fällen zu erhöhen, können Sie den Wert von **SMOTE percentage** (SMOTE-Prozentsatz) wie folgt mit Vielfachen von 100 festlegen:

||Klasse 0|Klasse 1|total|  
|-|-------------|-------------|-----------|  
|Ursprüngliches Dataset<br /><br /> (entspricht **SMOTE percentage** = **0**)|570<br /><br /> 76 %|178<br /><br /> 24 %|748|  
|**SMOTE percentage** = **100**|570<br /><br /> 62 %|356<br /><br /> 38 %|926|  
|**SMOTE percentage** = **200**|570<br /><br /> 52 %|534<br /><br /> 48 %|1\.104|  
|**SMOTE percentage** = **300**|570<br /><br /> 44 %|712<br /><br /> 56 %|1\.282|  
  
> [!WARNING]
> Das Erhöhen der Anzahl von Fällen über SMOTE garantiert nicht, dass genauere Modelle erzeugt werden. Versuchen Sie, die Pipelineverarbeitung mit unterschiedlichen Prozentsätzen, unterschiedlichen Merkmalssätzen und unterschiedlich vielen nächsten Nachbarn auszuprobieren, um zu ermitteln, wie sich das Hinzufügen von Fällen auf Ihr Modell auswirkt.  
  
## <a name="how-to-configure-smote"></a>Konfigurieren von SMOTE
  
1.  Fügen Sie das SMOTE-Modul zu Ihrer Pipeline hinzu. Sie finden das Modul in der Kategorie **Manipulation** (Bearbeitung) in den zu **Data Transformation** (Datentransformation) gehörenden Modulen.

2. Stellen Sie eine Verbindung mit dem Dataset her, für das Sie eine Erhöhung vornehmen möchten. Wenn Sie den Merkmalsraum für das Erstellen der neuen Fälle angeben möchten – entweder indem nur bestimmte Spalten verwendet oder einige ausgeschlossen werden –, verwenden Sie das Modul [Select Columns in Dataset](select-columns-in-dataset.md) (Spalten im Datensatz auswählen). Anschließend können Sie die Spalten isolieren, die Sie vor der Nutzung von SMOTE verwenden möchten.
  
    Andernfalls erfolgt die Erstellung neuer Fälle über SMOTE basierend auf *allen* Spalten, die Sie als Eingaben bereitstellen. Mindestens eine Spalte der Merkmalsspalten (Featurespalten) ist numerisch.
  
3.  Wählen Sie die Spalte aus, die die Bezeichnung oder Zielklasse enthält. Für SMOTE werden nur binäre Bezeichnungen akzeptiert.
  
4.  Das SMOTE-Modul erkennt automatisch die Minderheitsklasse in der Bezeichnungsspalte und ruft dann alle Fälle für die Minderheitsklasse ab. Nicht alle Spalten dürfen NaN-Werte enthalten.
  
5.  Geben Sie für die Option **SMOTE percentage** (SMOTE-Prozentsatz) eine ganze Zahl ein, die den Zielprozentsatz der Minderheitsfälle im Ausgabedataset angibt. Beispiel:  
  
    - Sie geben **0** ein. Das SMOTE-Modul gibt genau das Dataset zurück, das Sie als Eingabe bereitgestellt haben. Es werden keine neuen Minderheitsfälle hinzugefügt. In diesem Dataset hat sich das Verhältnis der Klassen nicht geändert.  
  
    - Sie geben **100** ein. Das SMOTE-Modul generiert neue Minderheitsfälle. Es wird die gleiche Anzahl von Minderheitsfällen wie im ursprünglichen Dataset hinzugefügt. Weil SMOTE die Anzahl von Mehrheitsfällen nicht erhöht hat, hat sich das Verhältnis der Fälle jeder Klasse geändert.  
  
    - Sie geben **200** ein. Das Modul verdoppelt den Prozentsatz der Minderheitsfälle im Vergleich zum ursprünglichen Dataset. Dies führt *nicht* dazu, dass nun die doppelte Anzahl von Minderheitsfällen vorliegt. Stattdessen wird die Größe des Datasets so angehoben, dass die Anzahl von Mehrheitsfällen unverändert bleibt. Die Anzahl von Minderheitsfällen wird angehoben, bis der gewünschte Prozentwert erreicht ist.  
  
    > [!NOTE]
    > Verwenden Sie für den SMOTE-Prozentsatz nur Vielfache von 100.

6.  Verwenden Sie die Option **Number of nearest neighbors** (Anzahl der nächsten Nachbarn), um die Größe des Merkmalsraums zu bestimmen, den der SMOTE-Algorithmus für das Erstellen neuer Fälle verwenden soll. Ein nächster Nachbar ist eine Zeile mit Daten (ein Fall), die einem Zielfall ähnelt. Der Abstand zwischen zwei Fällen wird gemessen, indem die gewichteten Vektoren aller Merkmale (Features) kombiniert werden.  
  
    + Durch Erhöhen der Anzahl der nächsten Nachbarn erhalten Sie Merkmale von mehr Fällen.
    + Belassen Sie es bei einer kleinen Anzahl der nächsten Nachbarn, verwenden Sie Merkmale, die eher denen in der ursprünglichen Stichprobe ähneln.  
  
7. Geben Sie einen Wert in das Feld **Random seed** (Zufälliger Ausgangswert) ein, wenn Sie dieselben Ergebnisse über Ausführungen derselben Pipeline mit denselben Daten sicherstellen möchten. Andernfalls generiert das Modul einen zufälligen Ausgangswert anhand von Prozessortaktwerten, wenn die Pipeline bereitgestellt wird. Dies kann zu geringfügig unterschiedlichen Ergebnissen zwischen Ausführungen führen.

8. Ausführen der Pipeline.  
  
   Die Ausgabe des Moduls ist ein Dataset, das die ursprünglichen Zeilen sowie eine Reihe von hinzugefügten Zeilen mit Minderheitsfällen enthält.  

## <a name="technical-notes"></a>Technische Hinweise

+ Wenn Sie ein Modell veröffentlichen, in dem das **SMOTE**-Modul verwendet wird, entfernen Sie **SMOTE** aus der Vorhersagepipeline, bevor die Veröffentlichung als Webdienst durchgeführt wird. Der Grund dafür ist, dass SMOTE zum Verbessern eines Modells während des Trainings und nicht für Bewertungen vorgesehen ist. Möglicherweise erhalten Sie einen Fehler, wenn eine veröffentlichte Vorhersagepipeline das SMOTE-Modul enthält.

+ Sie können häufig bessere Ergebnisse erzielen, indem Sie die Bereinigung fehlender Werte oder andere Transformationen anwenden, um Daten vor dem Anwenden von SMOTE zu korrigieren. 

+ Einige Forscher haben untersucht, ob SMOTE für Daten mit vielen Dimensionen oder mit geringer Dichte geeignet ist, z. B. in Textklassifizierungs- oder Genomikdatasets verwendete Daten. Der folgende Artikel enthält eine gute Zusammenfassung der Auswirkungen und der theoretischen Gültigkeit des Anwendens von SMOTE in solchen Fällen: [Blagus und Lusa: SMOTE für hochdimensionale Daten mit Unausgeglichenheit bei Klassen](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-14-106).

+ Falls Sie mit SMOTE für Ihr Dataset keine gut Wirksamkeit erzielen, können Sie beispielsweise die folgenden anderen Ansätze in Erwägung ziehen:
  + Methoden zur Überquotierung von Minderheitsfällen oder zur Unterquotierung von Mehrheitsfällen
  + Kombinationstechniken, die dem Lernenden direkt helfen, indem Clustering, Bagging oder adaptives Boosting verwendet wird


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 

