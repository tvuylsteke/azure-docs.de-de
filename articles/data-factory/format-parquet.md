---
title: Parquet-Format in Azure Data Factory
description: In diesem Thema wird der Umgang mit dem Parquet-Format in Azure Data Factory beschrieben.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: 81bbd476cea0472647ca183fb188fc13725d1469
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597625"
---
# <a name="parquet-format-in-azure-data-factory"></a>Parquet-Format in Azure Data Factory

Beachten Sie diesen Artikel, wenn Sie die **Parquet-Dateien analysieren oder die Daten im Parquet-Format schreiben** möchten. 

Das Parquet-Format wird für folgende Connectors unterstützt: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Dateisystem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) und [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der vom Parquet-Dataset unterstützten Eigenschaften.

| Eigenschaft         | Beschreibung                                                  | Erforderlich |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Die „type“-Eigenschaft des Datasets muss auf **Parquet** festgelegt werden. | Ja      |
| location         | Speicherorteinstellungen der Datei(en) Jeder dateibasierte Connector verfügt unter `location` über seinen eigenen Speicherorttyp und unterstützte Eigenschaften. **Informationen hierzu finden Sie im Abschnitt „Dataset-Eigenschaften“ des Artikels über Connectors**. | Ja      |
| compressionCodec | Der Codec für die Komprimierung, der beim Schreiben in Parquet-Dateien verwendet werden soll. Beim Lesen aus Parquet-Dateien bestimmen Data Factorys den Codec für die Komprimierung automatisch anhand der Dateimetadaten.<br>Unterstützte Typen sind „**none**“, „**gzip**“, „**snappy**“ (Standard) und „**lzo**“. Hinweis: LZO wird derzeit von der Kopieraktivität nicht für das Lesen/Schreiben von Parquet-Dateien unterstützt. | Nein       |

> [!NOTE]
> Ein Leerzeichen im Spaltennamen wird für Parquet-Dateien nicht unterstützt.

Nachfolgend sehen Sie das Beispiel eines Parquet-Datasets in Azure Blob Storage:

```json
{
    "name": "ParquetDataset",
    "properties": {
        "type": "Parquet",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Parquet-Quelle und -Senke unterstützt werden.

### <a name="parquet-as-source"></a>Parquet als Quelle

Die folgenden Eigenschaften werden im Abschnitt ***\*source\**** der Kopieraktivität unterstützt.

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **ParquetSource** festgelegt werden. | Ja      |
| storeSettings | Eine Gruppe von Eigenschaften für das Lesen von Daten aus einem Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Leseeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

### <a name="parquet-as-sink"></a>Parquet als Senke

Die folgenden Eigenschaften werden im Abschnitt ***\*sink\**** der Kopieraktivität unterstützt:

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **ParquetSink** festgelegt werden. | Ja      |
| storeSettings | Eine Gruppe von Eigenschaften für das Schreiben von Daten in einen Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Schreibeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Ausführliche Informationen hierzu finden Sie unter [Quellentransformation](data-flow-source.md) und [Senkentransformation](data-flow-sink.md) in Mapping Data Flow.

## <a name="data-type-support"></a>Datentypunterstützung

Komplexe Parquet-Datentypen (wie MAP, LIST und STRUCT) werden derzeit nicht unterstützt.

## <a name="using-self-hosted-integration-runtime"></a>Verwendung der selbstgehosteten Integration Runtime

> [!IMPORTANT]
> Wenn Sie Parquet-Dateien bei Kopiervorgängen mithilfe einer selbstgehosteten Integration Runtime (etwa zwischen lokalen Datenspeichern und der Cloud) nicht **unverändert** kopieren, müssen Sie auf Ihrem IR-Computer die **64-Bit-Version der JRE 8 (Java Runtime Environment) oder OpenJDK** sowie **Microsoft Visual C++ 2010 Redistributable Package** installieren. Weitere Details finden Sie im nächsten Absatz.

Für Kopiervorgänge in der selbstgehosteten Integration Runtime mit Serialisierung/Deserialisierung von Parquet-Dateien sucht ADF die Java Runtime Environment, indem es zunächst die Registrierung *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* auf JRE überprüft. Wird diese nicht gefunden, wird im nächsten Versuch die Systemvariable *`JAVA_HOME`* auf OpenJDK überprüft.

- **Für JRE:** Die 64-Bit-Integration Runtime erfordert die 64-Bit-JRE. Diese steht [hier](https://go.microsoft.com/fwlink/?LinkId=808605) zur Verfügung.
- **Für OpenJDK:** Die Unterstützung ist seit der IR-Version 3.13 verfügbar. Packen Sie die Datei „jvm.dll“ zusammen mit allen anderen erforderlichen OpenJDK-Assemblys in einem selbstgehosteten IR-Computer, und legen Sie die Umgebungsvariable JAVA_HOME des Systems entsprechend fest.
- **So installieren Sie Microsoft Visual C++ 2010 Redistributable Package:** Visual C++ 2010 Redistributable Package ist bei Installationen mit selbstgehosteter Integration Runtime nicht installiert. Diese steht [hier](https://www.microsoft.com/download/details.aspx?id=14632) zur Verfügung.

> [!TIP]
> Wenn Sie Daten mit der selbstgehosteten Integration Runtime in das/aus dem Parquet-Format kopieren und ein Fehler mit dem Text „Fehler beim Aufrufen von Java, Meldung: **java.lang.OutOfMemoryError:Java-Heapspeicher**“ auftritt, können Sie auf dem Computer, auf dem sich die selbstgehosteten IR befindet, eine Umgebungsvariable `_JAVA_OPTIONS` hinzufügen, um die min./max. Heapgröße für JVM anzupassen, sodass eine solche Kopie möglich ist, und dann die Pipeline erneut ausführen.

![JVM-Heapgröße für selbstgehostete IR festlegen](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Beispiel: Legen Sie für die Variable `_JAVA_OPTIONS` den Wert `-Xms256m -Xmx16g` fest. Das Flag `Xms` gibt den anfänglichen Speicherzuweisungspool für eine Java Virtual Machine (JVM) an, während `Xmx` den maximalen Speicherzuweisungspool angibt. Das bedeutet, dass die JVM mit einer Speichergröße von `Xms` gestartet wird und eine maximale Speichergröße von `Xmx` verwenden kann. Standardmäßig verwendet ADF zwischen 64 MB und 1 GB.

## <a name="next-steps"></a>Nächste Schritte

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Mapping Data Flow](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)
