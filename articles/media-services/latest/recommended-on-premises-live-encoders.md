---
title: Von Media Services empfohlene Livestreaming-Encoder – Azure | Microsoft-Dokumentation
description: Erfahren Sie etwas über die von Media Services empfohlenen lokalen Livestreaming-Encoder.
services: media-services
keywords: Codierung; Encoder; Medien
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 02/10/2020
ms.topic: article
ms.service: media-services
ms.openlocfilehash: aa9cd3f642e3d8047e8b64afb023fffb7bd2c4f6
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484909"
---
# <a name="recommended-live-streaming-encoders"></a>Empfohlene Livestreaming-Encoder

In Azure Media Services stellt ein [Liveereignis](https://docs.microsoft.com/rest/api/media/liveevents) (Kanal) eine Pipeline zum Verarbeiten von Livestreaminginhalten dar. Es gibt zwei Arten, auf die Live-Eingabestreams vom Liveereignis empfangen werden können.

* Von einem lokalen Liveencoder wird ein RTMP- oder Smooth Streaming-Datenstrom (fragmentiertes MP4) mit Mehrfachbitrate an das Liveereignis gesendet, das nicht für die Livecodierung mit Media Services aktiviert ist. Die erfassten Streams durchlaufen Liveereignisse ohne weitere Verarbeitung. Diese Methode wird als **Pass-Through-Methode** bezeichnet. Um auf Clientseite ein Streaming mit adaptiver Bitrate zu ermöglichen, sollte der Liveencoder anstelle eines Einzelbitraten-Datenstroms Mehrfachbitraten-Datenströme an ein Pass-Through-Liveereignis senden. 

    Bei Verwendung von Mehrfachbitraten-Datenströmen für das Pass-Through-Liveereignis müssen die Video-GOP-Größe und die Videofragmente unterschiedlicher Bitraten synchronisiert werden, um ein unerwartetes Verhalten bei der Wiedergabe zu vermeiden.

  > [!NOTE]
  > Die Verwendung der Pass-Through-Methode ist die wirtschaftlichste Form des Livestreamings.
 
* Ein lokaler Liveencoder sendet einen Single-Bitrate-Datenstrom an das Liveereignis, das zum Ausführen der Livecodierung mit Media Services in einem der folgenden Formate aktiviert wurde: RTMP oder Smooth Streaming (fragmentiertes MP4). Vom Liveereignis wird dann eine Livecodierung des Eingabestreams mit Einzelbitrate in einen (adaptiven) Videostream mit Mehrfachbitrate ausgeführt.

Ausführliche Informationen zur Livecodierung mit Media Services finden Sie unter [Livestreaming mit Media Services v3](live-streaming-overview.md).

## <a name="encoder-requirements"></a>Anforderungen für Encoder

Encoder müssen TLS 1.2 unterstützen, wenn HTTPS- oder RTMPS-Protokolle verwendet werden.

## <a name="live-encoders-that-output-rtmp"></a>Liveencoder mit RTMP-Ausgabe

Media Services empfiehlt die Verwendung eines der nachfolgenden Liveencoder mit RTMP-Ausgabe. Die unterstützten URL-Schemas sind `rtmp://` und `rtmps://`.

Überprüfen Sie beim Streamen per RTMP in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden TCP-Ports 1935 und 1936 geöffnet sind.<br/><br/>
Überprüfen Sie beim Streamen per RTMPS in den Firewall- und/oder Proxyeinstellungen, ob die ausgehenden TCP-Ports 2935 und 2936 geöffnet sind.

> [!NOTE]
> Bei Verwendung von RTMPS-Protokollen müssen Encoder TLS 1.2 unterstützen.

- Adobe Flash Media Live Encoder 3.2
- [Cambria Live 4.3](https://www.capellasystems.net/products/cambria-live/)
- Elemental Live (Version 2.14.15 und höher)
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast (Version 13.0.2 oder höher aufgrund der TLS 1.2-Anforderung)
- Telestream Wirecast S (nur RTMP wird unterstützt)
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream
- [Ffmpeg](https://www.ffmpeg.org)
- [GoPro](https://gopro.com/help/articles/block/getting-started-with-live-streaming) Hero 7 und Hero 8
- [Restream.io](https://restream.io/)

## <a name="live-encoders-that-output-fragmented-mp4"></a>Liveencoder mit Ausgabe im fragmentierten MP4-Format

Media Services empfiehlt den Einsatz eines der nachfolgenden Liveencoder, der Smooth Streaming mit Mehrfachbitrate (fragmentiertes MP4) ausgibt. Die unterstützten URL-Schemas sind `http://` und `https://`.

> [!NOTE]
> Bei Verwendung von HTTPS-Protokollen müssen Encoder TLS 1.2 unterstützen.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live (Version 2.14.15 und höher aufgrund der TLS 1.2-Anforderung)
- Envivio-4Caster C4 Gen III 
- Imagine Communications Selenio MCP3
- Media Excel Hero Live und Hero 4K (UHD/HEVC)
- [Ffmpeg](https://www.ffmpeg.org)

> [!TIP]
>  Wenn Sie Liveveranstaltungen in mehreren Sprachen streamen (z.B. eine englische Audiospur und eine spanische Audiospur), können Sie dies mit dem Media Excel Live Encoder erreichen, der so konfiguriert ist, dass er den Livefeed an ein Pass-Through-Liveereignis sendet.

## <a name="configuring-on-premises-live-encoder-settings"></a>Konfigurieren von lokalen Liveencodereinstellungen

Informationen über die gültigen Einstellungen für Ihren Liveereignistyp finden Sie unter [Vergleich von Liveereignistypen](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Wiedergabeanforderungen

Damit Inhalte wiedergegeben werden können, müssen sowohl ein Audio- als auch ein Videostream vorhanden sein. Die Wiedergabe nur des Videostreams wird nicht unterstützt.

### <a name="configuration-tips"></a>Konfigurationstipps

- Verwenden Sie nach Möglichkeit eine Kabelverbindung zum Internet.
- Zum Bestimmen der erforderlichen Bandbreite müssen die Streamingbitraten verdoppelt werden. Wenngleich nicht bindend, ist diese einfache Regel hilfreich, um Netzwerküberlastungen zu verhindern.
- Bei der Verwendung softwarebasierter Encoder schließen Sie alle nicht benötigten Programme.
- Das Ändern der Encoderkonfiguration, nachdem die Pushübertragung begonnen hat, führt zu negativen Auswirkungen auf das Ereignis. Durch Konfigurationsänderungen kann das Ereignis instabil werden. 
- Nehmen Sie sich zum Einrichten Ihres Ereignisses unbedingt ausreichend Zeit. Für Großereignisse empfiehlt es sich, eine Stunde vor dem Ereignis mit dem Setup zu beginnen.

## <a name="becoming-an-on-premises-encoder-partner"></a>Partner für lokale Encoder werden

Wenn Sie Azure Media Services-Partner für lokale Encoder werden, unterstützt Media Services Ihr Produkt, indem es Ihren Encoder Unternehmenskunden empfiehlt. Um Partner für lokale Encoder zu werden, müssen Sie die Kompatibilität Ihres lokalen Encoders mit Media Services bestätigen. Führen Sie hierzu die folgenden Überprüfungsschritte aus.

### <a name="pass-through-live-event-verification"></a>Überprüfung von Pass-Through-Liveereignissen

1. Vergewissern Sie sich in Ihrem Media Services-Konto, dass der **Streamingendpunkt** ausgeführt wird. 
2. Erstellen und starten Sie das **Pass-Through**-Liveereignis. <br/> Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing.md).
3. Rufen Sie die Erfassungs-URLs ab, und konfigurieren Sie Ihren lokalen Encoder für die Verwendung der URL, um einen Livedatenstrom mit Mehrfachbitrate an Media Services zu senden.
4. Rufen Sie die Vorschau-URL ab und verwenden Sie sie, um sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.
5. Erstellen Sie ein neues **Medienobjekt**.
6. Erstellen Sie eine **Liveausgabe**, und verwenden Sie den Namen des erstellten Medienobjekts.
7. Erstellen Sie einen **Streaminglocator** mit den integrierten Arten von **Streamingrichtlinien**.
8. Listen Sie die Pfade auf dem **Streaminglocator** auf, um die zu verwendenden URLs zurückzugeben.
9. Rufen Sie den Hostnamen für den **Streamingendpunkt** ab, von dem aus Sie streamen möchten.
10. Kombinieren Sie die URL aus Schritt 8 mit dem Hostnamen aus Schritt 9, um die vollständige URL zu erhalten.
11. Lassen Sie Ihren Liveencoder ungefähr 10 Minuten lang laufen.
12. Beenden Sie das Liveereignis. 
13. Verwenden Sie einen Player wie [Azure Media Player](https://aka.ms/azuremediaplayer), um die archivierten Medienobjekte zu beobachten, um sicherzustellen, dass die Wiedergabe auf allen Qualitätsebenen keine sichtbaren Störungen aufweist. Zum Beobachten und Überprüfen können Sie während der Livesitzung auch die Vorschau-URL verwenden.
14. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Liveencoders.
15. Setzen Sie den Status des Liveereignisses nach dem Erstellen der einzelnen Stichproben zurück.
16. Wiederholen Sie die Schritte 5 bis 15 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

### <a name="live-encoding-live-event-verification"></a>Überprüfung von Liveereignissen mit Livecodierung

1. Vergewissern Sie sich in Ihrem Media Services-Konto, dass der **Streamingendpunkt** ausgeführt wird. 
2. Erstellen und starten Sie ein **Livecodierungs**-Liveereignis. <br/> Weitere Informationen finden Sie im Abschnitt [LiveEvent-Zustandswerte und Abrechnung](live-event-states-billing.md).
3. Rufen Sie die Erfassungs-URLs ab, und konfigurieren Sie Ihren Encoder so, dass er einen Livedatenstrom mit Einzelbitrate per Push an Media Services überträgt.
4. Rufen Sie die Vorschau-URL ab und verwenden Sie sie, um sich zu vergewissern, dass die Eingabe des Encoders auch tatsächlich empfangen wird.
5. Erstellen Sie ein neues **Medienobjekt**.
6. Erstellen Sie eine **Liveausgabe**, und verwenden Sie den Namen des erstellten Medienobjekts.
7. Erstellen Sie einen **Streaminglocator** mit den integrierten Arten von **Streamingrichtlinien**.
8. Listen Sie die Pfade auf dem **Streaminglocator** auf, um die zu verwendenden URLs zurückzugeben.
9. Rufen Sie den Hostnamen für den **Streamingendpunkt** ab, von dem aus Sie streamen möchten.
10. Kombinieren Sie die URL aus Schritt 8 mit dem Hostnamen aus Schritt 9, um die vollständige URL zu erhalten.
11. Lassen Sie Ihren Liveencoder ungefähr 10 Minuten lang laufen.
12. Beenden Sie das Liveereignis.
13. Verwenden Sie einen Player wie [Azure Media Player](https://aka.ms/azuremediaplayer), um die archivierten Medienobjekte zu beobachten, um sicherzustellen, dass die Wiedergabe auf allen Qualitätsebenen keine sichtbaren Störungen aufweist. Zum Beobachten und Überprüfen können Sie während der Livesitzung auch die Vorschau-URL verwenden.
14. Vermerken Sie die Medienobjekt-ID, die veröffentlichte Streaming-URL für das Livearchiv und die Einstellungen sowie die verwendete Version Ihres Liveencoders.
15. Setzen Sie den Status des Liveereignisses nach dem Erstellen der einzelnen Stichproben zurück.
16. Wiederholen Sie die Schritte 5 bis 15 für alle Konfigurationen, die von Ihrem Encoder unterstützt werden (mit und ohne Werbesignalisierung oder Untertiteln sowie in verschiedenen Codiergeschwindigkeiten).

### <a name="longevity-verification"></a>Überprüfung der Lebensdauer

Fügen Sie die gleichen Schritte wie bei der [Überprüfung von Pass-Through-Liveereignissen](#pass-through-live-event-verification) mit Ausnahme von Schritt 11 aus. <br/>Lassen Sie Ihren Liveencoder statt für 10 Minuten mindestens eine Woche lang laufen. Verwenden Sie einen Player wie [Azure Media Player](https://aka.ms/azuremediaplayer), um das Livestreaming (oder ein archiviertes Medienobjekt) von Zeit zu Zeit zu beobachten, um sicherzustellen, dass die Wiedergabe keine sichtbaren Störungen aufweist.

### <a name="email-your-recorded-settings"></a>Senden Ihrer aufgezeichneten Einstellungen per E-Mail

Senden Sie schließlich eine E-Mail mit Ihren aufgezeichneten Einstellungen und Livearchivparametern als Benachrichtigung darüber, dass alle automatischen Überprüfungen bestanden wurden, unter amshelp@microsoft.com an Azure Media Services. Schließen Sie auch Ihre Kontaktinformationen für nachfolgende Kontaktaufnahmen ein. Setzen Sie sich bei Fragen zu dieser Vorgehensweise mit dem Azure Media Services-Team in Verbindung.

## <a name="next-steps"></a>Nächste Schritte

[Livestreaming mit Media Services v3](live-streaming-overview.md)
