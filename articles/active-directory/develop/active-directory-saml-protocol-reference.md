---
title: Verwendung des SAML-Protokolls durch Azure AD | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die SAML-Profile für das einmalige Anmelden und Abmelden in Azure Active Directory.
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: dc7771f29fb5d00aedfe5162a98f5f0c14544a7b
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77161170"
---
# <a name="how-azure-ad-uses-the-saml-protocol"></a>Verwendung des SAML-Protokolls durch Azure AD

Azure Active Directory (Azure AD) verwendet das SAML 2.0-Protokoll, um es Anwendungen zu ermöglichen, für Benutzer das einmalige Anmelden bereitzustellen. In den SAML-Profilen zum [einmaligen Anmelden](single-sign-on-saml-protocol.md) und [einmaligen Abmelden](single-sign-out-saml-protocol.md) von Azure AD wird beschrieben, wie SAML-Assertionen, -Protokolle und -Bindungen im Identitätsanbieterdienst verwendet werden.

Für das SAML-Protokoll ist es erforderlich, dass der Identitätsanbieter (Azure AD) und der Dienstanbieter (die Anwendung) Informationen übereinander austauschen.

Wenn eine Anwendung unter Azure AD registriert ist, registriert der App-Entwickler verbundbezogene Informationen unter Azure AD. Dies gilt auch für den **Umleitungs-URI** und den **Metadaten-URI** der Anwendung.

Azure AD nutzt den **Metadaten-URI** des Clouddiensts, um den Signaturschlüssel und den Abmelde-URI abzurufen. Kunden können die App unter **Azure AD > App-Registrierung** öffnen und dann über **Einstellungen > Eigenschaften** die Abmelde-URL aktualisieren. Auf diese Weise kann Azure AD die Antwort an die richtige URL senden. 

Azure Active Directory macht mandantenspezifische und allgemeine (mandantenunabhängige) Endpunkte für das einmalige Anmelden und das einmalige Abmelden verfügbar. Bei diesen URLs handelt es sich um adressierbare Speicherorte (nicht nur um Bezeichner), damit Sie auf den Endpunkt zugreifen können, um die Metadaten zu lesen.

* Der mandantenspezifische Endpunkt befindet sich unter `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`. Der Platzhalter *\<TenantDomainName>* steht für einen registrierten Domänennamen oder die TenantID-GUID eines Azure AD-Mandanten. Die Verbundmetadaten des Mandanten „contoso.com“ befinden sich beispielsweise hier: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml.

* Der mandantenunabhängige Endpunkt befindet sich unter `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Bei dieser Endpunktadresse wird anstelle eines Mandantendomänennamens oder einer ID das Wort **common** verwendet.

Informationen zu den Verbundmetadatendokumenten, die von Azure AD veröffentlicht werden, finden Sie unter [Verbundmetadaten](../azuread-dev/azure-ad-federation-metadata.md).
