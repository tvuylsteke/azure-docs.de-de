---
title: Playbook für den Umgang mit allgemeinen Sicherheitsanforderungen | Microsoft-Dokumentation
titleSuffix: Azure SQL Database
description: Dieser Artikel enthält allgemeine Sicherheitsanforderungen und bewährte Methoden in Azure SQL-Datenbank.
ms.service: sql-database
ms.subservice: security
author: VanMSFT
ms.author: vanto
ms.topic: article
ms.date: 02/20/2020
ms.reviewer: ''
ms.openlocfilehash: c18e1b1a1feba5c528a692b7d63287b3751b62cf
ms.sourcegitcommit: 934776a860e4944f1a0e5e24763bfe3855bc6b60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506214"
---
# <a name="playbook-for-addressing-common-security-requirements-with-azure-sql-database"></a>Playbook für den Umgang mit allgemeinen Sicherheitsanforderungen in Azure SQL-Datenbank

> [!NOTE]
> Dieses Dokument enthält bewährte Methoden für den Umgang mit allgemeinen Sicherheitsanforderungen und entsprechende Lösungen. Nicht alle Anforderungen gelten für alle Umgebungen. Sie sollten mit Ihrem Datenbank- und Sicherheitsteam überlegen, welche Funktionen implementiert werden sollen.

## <a name="solving-common-security-requirements"></a>Lösungen für allgemeine Sicherheitsanforderungen

Dieses Dokument enthält Anleitungen zum Beheben allgemeiner Sicherheitsanforderungen für neue oder vorhandene Anwendungen mit Azure SQL-Datenbank. Es ist nach Sicherheitsbereichen auf hoher Ebene gegliedert. Informationen zur Behandlung bestimmter Bedrohungen finden Sie im Abschnitt [Allgemeine Sicherheitsbedrohungen und mögliche Risikominderungen](#common-security-threats-and-potential-mitigations). Obwohl einige der vorgestellten Empfehlungen auch dann gelten, wenn Sie Anwendungen aus einer lokalen Umgebung zu Azure migrieren, bilden Migrationsszenarien nicht den Schwerpunkt dieses Dokuments.

### <a name="azure-sql-database-deployment-offers-covered-in-this-guide"></a>Bereitstellungsangebote für Azure SQL-Datenbank, die in dieser Anleitung behandelt werden

- [SQL-Datenbanken](https://docs.microsoft.com/azure/sql-database/sql-database-single-index): [Einzelne Datenbanken](sql-database-single-database.md) und [Pools für elastische Datenbanken](sql-database-elastic-pool.md) auf [Azure SQL-Datenbank-Servern](sql-database-servers.md)
- [Verwaltete Instanzen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)

### <a name="sql-deployment-offers-not-covered-in-this-guide"></a>SQL- Bereitstellungsangebote, die in diesem Leitfaden nicht behandelt werden

- Azure SQL Data Warehouse
- Azure SQL-VMs (IaaS)
- SQL Server (lokal)

### <a name="audience"></a>Zielgruppe

Die Zielgruppen für dieses Handbuch sind Kunden, die mit Fragen zum Sichern von Azure SQL-Datenbank konfrontiert sind. Die Rollen, für die dieser Artikel zu bewährten Methoden interessant ist, gehören beispielsweise:

- Sicherheitsarchitekten
- Sicherheits-Manager
- Compliance Officers
- Datenschutzbeauftragte
- Sicherheitstechniker

### <a id="using"></a> Verwenden dieses Leitfadens

Dieses Dokument ist als Begleitmaterial zu unserer vorhandenen Dokumentation zur [Sicherheit von Azure SQL-Datenbank](sql-database-security-overview.md) gedacht.

Sofern nicht anders angegeben, empfehlen wir Ihnen, alle in jedem Abschnitt aufgeführten bewährten Methoden zu befolgen, um das jeweilige Ziel oder die jeweilige Anforderung zu erreichen. Um bestimmte Sicherheitscompliancestandards oder bewährte Methoden zu erfüllen, werden wichtige Kontrollmechanismen zur Einhaltung von Vorschriften im Abschnitt „Anforderungen“ oder „Ziele“ aufgeführt, sofern anwendbar. Dies sind die Sicherheitsstandards und -bestimmungen, auf die in diesem Dokument verwiesen wird:

- [FedRAMP](https://www.fedramp.gov/documents/): AC-04, AC-06
- [SOC](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/sorhome.html): CM-3, SDL-3
- [ISO/IEC 27001](https://www.iso27001security.com/html/27001.html): Zugriffssteuerung, Kryptografie
- [Microsoft OSA-Methoden (Operational Security Assurance)](https://www.microsoft.com/en-us/securityengineering/osa/practices): Methode Nr. 1-6 und Nr. 9
- [NIST-Sonderpublikation 800-53 zu Sicherheitskontrollen](https://nvd.nist.gov/800-53): AC-5, AC-6
- [PCI-DSS](https://www.pcisecuritystandards.org/document_library): 6.3.2, 6.4.2

### <a name="feedback"></a>Feedback

Wir planen die laufende Aktualisierung der hier aufgeführten Empfehlungen und bewährten Methoden. Stellen Sie über den Link **Feedback** am Ende dieses Artikels Anregungen oder Korrekturen für dieses Dokument bereit.

## <a name="authentication"></a>Authentifizierung

Die Authentifizierung ist der Prozess, bei dem bestätigt wird, dass der Benutzer derjenige ist, der er zu sein vorgibt. Azure SQL-Datenbank unterstützt zwei Arten der Authentifizierung:

- SQL-Authentifizierung
- Authentifizierung über Azure Active Directory

> [!NOTE]
> Die Azure Active Directory-Authentifizierung wird möglicherweise nicht für alle Tools und Drittanbieteranwendungen unterstützt.

### <a name="central-management-for-identities"></a>Zentrale Verwaltung für Identitäten

Die zentrale Identitätsverwaltung bietet die folgenden Vorteile:

- Verwalten von Gruppenkonten und Steuern von Benutzerberechtigungen ohne Duplizieren von Anmeldungen bei Azure SQL-Datenbank-Servern und -Datenbanken.
- Vereinfachte und flexible Berechtigungsverwaltung.
- Verwaltung von Anwendungen im richtigen Maßstab.

**Implementierung**:

- Verwenden Sie die Azure Active Directory-Authentifizierung (Azure AD) für die zentralisierte Identitätsverwaltung.

**Bewährte Methoden:**

- Erstellen Sie einen Azure AD-Mandanten, [erstellen Sie Benutzer](../active-directory/fundamentals/add-users-azure-active-directory.md), um menschliche Benutzer darzustellen, und erstellen Sie [Dienstprinzipale](../active-directory/develop/app-objects-and-service-principals.md), die Apps, Dienste und Automatisierungstools darstellen. Dienstprinzipale entsprechen Dienstkonten in Windows und Linux. 

- Weisen Sie Azure AD-Prinzipalen über die Gruppenzuweisung Zugriffsrechte für Ressourcen zu: Erstellen Sie Azure AD-Gruppen, gewähren Sie Zugriff auf Gruppen, und fügen Sie den Gruppen einzelne Mitglieder hinzu. Erstellen in Ihrer Datenbank eigenständige Datenbankbenutzer, die Ihren Azure AD-Gruppen zugeordnet sind. Zum Zuweisen von Berechtigungen innerhalb der Datenbank müssen Sie Benutzern Datenbankrollen mit den entsprechenden Berechtigungen zuordnen.
  - Weitere Informationen finden Sie in den Artikeln [Konfigurieren und Verwalten von Azure Active Directory-Authentifizierung mit SQL](sql-database-aad-authentication-configure.md) und [Verwenden von Azure AD für die Authentifizierung mit SQL](sql-database-aad-authentication.md).
  > [!NOTE]
  > In einer verwalteten Instanz können Sie auch Anmeldungen erstellen, die Azure AD-Prinzipalen in der Masterdatenbank zugeordnet sind. Siehe [CREATE LOGIN (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).

- Die Verwendung von Azure AD-Gruppen vereinfacht die Berechtigungsverwaltung des Gruppenbesitzers, und der Besitzer der Ressource kann der Gruppe Mitglieder hinzufügen bzw. aus ihr entfernen. 

- Erstellen Sie eine separate Gruppe für den Azure AD-Administrator für SQL-Datenbank-Server.

  - Lesen Sie dazu den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

- Überwachen Sie Änderungen der Azure AD-Gruppenmitgliedschaft mithilfe von Azure AD-Überwachungsaktivitätsberichten. 

- Für eine verwaltete Instanz ist ein separater Schritt erforderlich, um einen Azure AD-Administrator zu erstellen. 
  - Lesen Sie dazu den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete Instanz](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance). 

> [!NOTE]
> - Azure AD Authentifizierung wird in Azure SQL-Überwachungsprotokollen aufgezeichnet, aber nicht in Azure AD-Anmeldeprotokollen.
> - In Azure gewährte RBAC-Berechtigungen gelten nicht für Azure SQL-Datenbank-Berechtigungen. Diese Berechtigungen müssen in SQL-Datenbank mithilfe vorhandener SQL-Berechtigungen manuell erstellt/zugeordnet werden.
> - Auf der Clientseite muss die Azure AD-Authentifizierung auf das Internet oder über eine benutzerdefinierte Route (User Defined Route, UDR) auf ein VNET zugreifen können.
> - Das Azure AD-Zugriffstoken wird auf der Clientseite zwischengespeichert, und seine Lebensdauer hängt von der Tokenkonfiguration ab. Lesen Sie dazu den Artikel [Konfigurierbare Tokenlebensdauern in Azure Active Directory](../active-directory/develop/active-directory-configurable-token-lifetimes.md).
> - Eine Anleitung zur Behebung von Problemen bei der Azure AD-Authentifizierung finden Sie im folgenden Blogbeitrag: <https://techcommunity.microsoft.com/t5/azure-sql-database/troubleshooting-problems-related-to-azure-ad-authentication-with/ba-p/1062991>.

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

> Erwähnt in: OSA-Methode Nr. 2, ISO-Zugriffssteuerung (Access Control, AC)

Indem Azure Multi-Factor Authentication (MFA) eine weitere Form der Authentifizierung erfordert, bietet das Verfahren zusätzliche Sicherheit .

**Implementierung**:

- [Aktivieren Sie MFA](../active-directory/authentication/concept-mfa-howitworks.md) in Azure AD mit bedingtem Zugriff, und verwenden Sie interaktive Authentifizierung. 

- Die Alternative besteht darin, MFA für das gesamte Azure AD oder die gesamte AD-Domäne zu aktivieren.

**Bewährte Methoden:**

- Aktivieren Sie bedingten Zugriff in Azure AD (erfordert ein Premium-Abonnement). 
  - Lesen Sie dazu den Artikel [Bedingter Zugriff in Azure AD](../active-directory/conditional-access/overview.md).  

- Erstellen Sie Azure AD-Gruppen, und aktivieren Sie die MFA-Richtlinie für ausgewählte Gruppen mithilfe des bedingten Azure AD-Zugriffs. 
  - Weitere Informationen finden Sie im Artikel [Planen der Bereitstellung bedingten Zugriffs](../active-directory/conditional-access/plan-conditional-access.md). 

- MFA kann für das gesamte Azure AD oder für das gesamte Active Directory im Verbund mit Azure AD aktiviert werden. 

- Verwenden Sie den interaktiven Authentifizierungsmodus von Azure AD für SQL-Datenbank, bei dem ein Kennwort interaktiv angefordert wird, gefolgt von MFA-Authentifizierung:      
  - Verwenden Sie universelle Authentifizierung in SSMS. Lesen Sie dazu den Artikel [Verwenden der mehrstufigen Authentifizierung von AAD mit Azure SQL-Datenbank und Azure SQL Data Warehouse (SSMS-Unterstützung für MFA)](sql-database-ssms-mfa-authentication.md).
  - Verwenden Sie interaktive Authentifizierung, die in SQL Server Data Tools (SSDT) unterstützt wird. Lesen Sie dazu den Artikel [Azure Active Directory-Unterstützung in SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/azure-active-directory?view=azuresqldb-current).
  - Verwenden Sie andere SQL-Tools, die MFA unterstützen. 
    - Unterstützung des SSMS-Assistenten zum Exportieren/Extrahieren/Bereitstellen der Datenbank  
    - [sqlpackage.exe](https://docs.microsoft.com/sql/tools/sqlpackage): Option „/ua“ 
    - [sqlcmd-Hilfsprogramm](https://docs.microsoft.com/sql/tools/sqlcmd-utility): Option -G (interaktiv)
    - [bcp-Hilfsprogramm](https://docs.microsoft.com/sql/tools/bcp-utility): Option -G (interaktiv) 

- Implementieren Sie Ihre Anwendungen zum Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe interaktiver Authentifizierung mit MFA-Unterstützung. 
  - Lesen Sie dazu den Artikel [Herstellen einer Verbindung mit Azure SQL-Datenbank mit Azure Multi-Factor Authentication](active-directory-interactive-connect-azure-sql-db.md). 
  > [!NOTE]
  > Dieser Authentifizierungsmodus erfordert benutzerbasierte Identitäten. In Fällen, in denen ein vertrauenswürdiges Identitätsmodell verwendet wird, das die individuelle Azure AD-Benutzerauthentifizierung umgeht (z.B. durch Verwendung von verwalteten Identitäten für Azure-Ressourcen), gilt MFA nicht.

### <a name="minimize-the-use-of-password-based-authentication-for-users"></a>Minimieren der Verwendung der kennwortbasierten Authentifizierung für Benutzer

> Erwähnt in: OSA-Methode Nr. 4, ISO-Zugriffssteuerung (Access Control, AC)

Kennwortbasierte Authentifizierungsmethoden sind eine schwächere Form der Authentifizierung. Anmeldeinformationen können kompromittiert oder versehentlich offengelegt werden.

**Implementierung**:

- Verwenden Sie in Azure AD integrierte Authentifizierung, bei der die Verwendung von Kennwörtern entfällt.

**Bewährte Methoden:**

- Verwenden Sie SSO-Authentifizierung (Single Sign-On, einmaliges Anmelden) mithilfe von Windows-Anmeldeinformationen. Bilden Sie einen Verbund der lokalen AD-Domäne mit Azure AD, und verwenden Sie integrierte Windows-Authentifizierung (für in die Domäne eingebundene Computer mit Azure AD).
  - Lesen Sie dazu den Artikel [SSMS-Unterstützung für integrierte Azure AD-Authentifizierung](sql-database-aad-authentication-configure.md#active-directory-integrated-authentication).

### <a name="minimize-the-use-of-password-based-authentication-for-applications"></a>Minimieren der Verwendung der kennwortbasierten Authentifizierung für Anwendungen 

> Erwähnt in: OSA-Methode Nr. 4, ISO-Zugriffssteuerung (Access Control, AC)

**Implementierung**:

- Aktivieren Sie eine verwaltete Azure-Identität. Sie können auch integrierte oder zertifikatbasierte Authentifizierung verwenden. 

**Bewährte Methoden:**

- Verwenden Sie [verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).
  - [Systemseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md) 
  - [Benutzerseitig zugewiesene verwaltete Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
  - [Verwenden von Azure SQL-Datenbank aus App Service mit verwalteter Identität (ohne Codeänderungen)](https://github.com/Azure-Samples/app-service-msi-entityframework-dotnet)

- Verwenden Sie zertifikatbasierte Authentifizierung für eine Anwendung. 
  - Weitere Informationen finden Sie in diesem [Codebeispiel](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/token). 

- Verwenden Sie Azure AD-Authentifizierung für die integrierte Verbunddomäne und den in die Domäne eingebundenen Computer (siehe Abschnitt weiter oben).
  - Weitere Informationen finden Sie in der [Beispielanwendung für integrierte Authentifizierung](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/integrated).

### <a name="protect-passwords-and-secrets"></a>Schützen von Kennwörtern und Geheimnissen

Wenn Kennwörter nicht vermeidbar sind, stellen Sie sicher, dass diese geschützt sind.

**Implementierung**:

- Verwenden Sie Azure Key Vault zum Speichern von Kennwörtern und Geheimnissen. Wann immer möglich, verwenden Sie MFA für Azure SQL-Datenbank mit Azure AD-Benutzern.

**Bewährte Methoden:**

- Wenn das Vermeiden von Kennwörtern oder Geheimnissen nicht möglich ist, speichern Sie Benutzerkennwörter und Anwendungsgeheimnisse in Azure Key Vault, und verwalten Sie den Zugriff über Key Vault-Zugriffsrichtlinien. 

- Verschiedene App-Entwicklungsframeworks können auch frameworkspezifische Mechanismen zum Schutz von Geheimnissen in der App zur Verfügung stellen. Beispiel: [ASP.NET Core-App](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows).

### <a name="use-sql-authentication-for-legacy-applications"></a>Verwenden von SQL-Authentifizierung für ältere Anwendungen 

SQL-Authentifizierung bezieht sich auf die Authentifizierung eines Benutzers beim Herstellen einer Verbindung mit Azure SQL-Datenbank mithilfe von Benutzername und Kennwort. Eine Anmeldung muss in jedem SQL-Datenbank-Server oder verwalteten Instanz erstellt werden, und ein Benutzer muss in jeder Datenbank erstellt werden.

**Implementierung**:

- Verwenden Sie SQL-Authentifizierung.

**Bewährte Methoden:**

- Erstellen Sie als Serveradministrator Anmeldungen und Benutzer. Sofern nicht eigenständige Datenbankbenutzer mit Kennwörtern verwendet werden, werden alle Kennwörter in der Masterdatenbank gespeichert.
  - Lesen Sie dazu den Artikel [Steuern und Gewähren des Datenbankzugriffs für SQL-Datenbank und SQL Data Warehouse](sql-database-manage-logins.md).

## <a name="access-management"></a>Zugriffsverwaltung

Zugriffsverwaltung ist der Vorgang der Steuerung und Verwaltung des Zugriffs und der Berechtigungen autorisierter Benutzer auf die Azure SQL-Datenbank.

### <a name="implement-principle-of-least-privilege"></a>Implementieren des Prinzips der geringsten Rechte

> Erwähnt in: FedRamp-Kontrollmechanismen AC-06, NIST: AC-6, OSA-Methode Nr. 3

Das Prinzip der geringsten Rechte besagt, dass Benutzer nicht umfangreichere Berechtigungen besitzen sollten, als für die Ausführung ihrer Aufgaben erforderlich sind. Weitere Informationen finden Sie im Artikel [Minimale Administration](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview).

**Implementierung**:

Weisen Sie nur die notwendigen [Berechtigungen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) zu, um die erforderlichen Aufgaben auszuführen:

- Auf der SQL-Datenebene: 
    - Verwenden Sie granulare Berechtigungen und benutzerdefinierte Datenbankrollen (oder Serverrollen in MI): 
        1. Erstellen der erforderlichen Rollen
            - [CREATE ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql)
            - [CREATE SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/create-server-role-transact-sql)
        1. Erstellen der erforderlichen Benutzer
            - [CREATE USER](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql)
        1. Hinzufügen von Benutzern als Mitglieder zu Rollen 
            - [ALTER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql)
            - [ALTER SERVER ROLE](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql)
        1. Weisen Sie dann Rollen Berechtigungen zu. 
            - [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) 
    - Stellen Sie sicher, dass Benutzer nicht unnötigen Rollen zugewiesen werden.

- In Azure Resource Manager:
  - Verwenden Sie integrierte Rollen (sofern verfügbar) oder benutzerdefinierte RBAC-Rollen, und weisen Sie die erforderlichen Berechtigungen zu.
    - [Integrierte Rollen für Azure](../role-based-access-control/built-in-roles.md) 
    - [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md)

**Bewährte Methoden:**

Die folgenden bewährten Methoden sind optional, führen jedoch zu einer besseren Verwaltbarkeit und Unterstützbarkeit Ihrer Sicherheitsstrategie: 

- Beginnen Sie nach Möglichkeit mit dem geringstmöglichen Satz an Berechtigungen, und fangen Sie an, Berechtigungen einzeln hinzuzufügen, wenn es wirklich notwendig (und gerechtfertigt) ist – im Gegensatz zum umgekehrten Ansatz: Berechtigungen Schritt für Schritt zu entfernen. 

- Verzichten Sie darauf, einzelnen Benutzern Berechtigungen zuzuweisen. Verwenden Sie stattdessen konsistent Rollen (Datenbank- oder Serverrollen). Rollen helfen bei der Berichterstellung und bei der Problembehandlung von Berechtigungen. (Azure RBAC unterstützt nur die Berechtigungszuweisung über Rollen.) 

- Erstellen Sie benutzerdefinierte Rollen mit exakt den Berechtigungen, die benötigt werden, und verwenden Sie diese Rollen. Typische Rollen, die in der Praxis verwendet werden: 
  - Sicherheitsbereitstellung 
  - Administrator 
  - Entwickler 
  - Supportpersonal 
  - Prüfer 
  - Automatisierte Prozesse 
  - Endbenutzer 
  
- Verwenden Sie integrierte Rollen nur dann, wenn die Berechtigungen der Rollen genau den Berechtigungen entsprechen, die für den Benutzer erforderlich sind. Sie können Benutzer mehreren Rollen zuweisen. 

- Beachten Sie, dass die Berechtigungen in SQL Server-Datenbank-Engine auf die folgenden Bereiche angewendet werden können. Je kleiner der Bereich, desto geringer die Auswirkungen der gewährten Berechtigungen: 
  - Azure SQL-Datenbank-Server (spezielle Rollen in der Masterdatenbank) 
  - Datenbank 
  - Schema
      - Es ist eine bewährte Methode, Schemas zum Gewähren von Berechtigungen innerhalb einer Datenbank zu verwenden. (Siehe auch: [Schemaentwurf für SQL Server: Empfehlungen für den Schemaentwurf unter Sicherheitsaspekten](http://andreas-wolter.com/en/schema-design-for-sql-server-recommendations-for-schema-design-with-security-in-mind/))
  - Objekt (Tabelle, Sicht, Prozedur usw.) 
  > [!NOTE]
  > Es wird nicht empfohlen, Berechtigungen auf Objektebene anzuwenden, da diese Ebene der Gesamtimplementierung unnötige Komplexität hinzufügt. Wenn Sie sich dafür entscheiden, Berechtigungen auf Objektebene zu verwenden, sollten diese eindeutig dokumentiert werden. Gleiches gilt für Berechtigungen auf Spaltenebene, die aus denselben Gründen noch weniger empfehlenswert sind. Beachten Sie auch, dass GEWÄHREN auf Spaltenebene standardmäßig nicht durch [VERWEIGERN](https://docs.microsoft.com/sql/t-sql/statements/deny-object-permissions-transact-sql) auf Tabellenebene überschrieben wird. Dazu müsste die [Serverkonfiguration mit Common Criteria-Kompatibilität](https://docs.microsoft.com/sql/database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option) aktiviert werden.

- Führen Sie regelmäßige Überprüfungen mithilfe der [Sicherheitsrisikobewertung (VA)](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) aus, um auf zu umfangreiche Berechtigungen zu testen.

### <a name="implement-separation-of-duties"></a>Implementieren klarer Aufgabentrennung

> Erwähnt in: FedRamp: AC-04, NIST: AC-5, ISO: A.6.1.2, PCI 6.4.2, SOC: CM-3, SDL-3

Bei der Trennung von Aufgaben, auch als „Aufteilung von Aufgaben“ bezeichnet, wird beschrieben, wie Sie vertrauliche Aufgaben in mehrere Unteraufgaben aufteilen, die verschiedenen Benutzern zugewiesen werden. Die Trennung von Aufgaben hilft, Sicherheitsverletzungen im Zusammenhang mit Daten zu verhindern.

**Implementierung**:

- Identifizieren Sie die erforderliche Ebene der Trennung von Aufgaben. Beispiele: 
  - Aufgabentrennung zwischen Entwicklungs-/Test- und Produktionsumgebungen 
  - Sicherheitsbezogene sensible Aufgaben im Vergleich zu Datenbankadministratoraufgaben (DBA) auf Verwaltungsebene und Entwickleraufgaben. 
    - Beispiele: Prüfer, Erstellen einer Sicherheitsrichtlinie für Sicherheit auf Rollenebene (Role-Level Security, RLS), Implementieren von SQL-Datenbank-Objekten mit DDL-Berechtigungen.

- Identifizieren Sie eine umfassende Hierarchie von Benutzern (und automatisierten Prozessen), die auf das System zugreifen.

- Erstellen Sie Rollen entsprechend den erforderlichen Benutzergruppen, und weisen Sie Rollen Berechtigungen zu. 
  - Für Aufgaben auf Verwaltungsebene im Azure-Portal oder über PowerShell-Automation verwenden Sie RBAC-Rollen. Suchen Sie entweder nach einer integrierten Rolle, die mit der Anforderung übereinstimmt, oder erstellen Sie eine benutzerdefinierte RBAC-Rolle mithilfe der verfügbaren Berechtigungen. 
  - Erstellen Sie Serverrollen für serverweite Aufgaben (Erstellen neuer Anmeldungen, Datenbanken) in einer verwalteten Instanz. 
  - Erstellen Sie Datenbankrollen für Aufgaben auf Datenbankebene.

- Für bestimmte vertrauliche Aufgaben sollten Sie die Erstellung spezieller gespeicherter Prozeduren in Erwägung ziehen, die durch ein Zertifikat signiert werden, um Aufgaben im Namen von Benutzern auszuführen. 
  - Beispiel: [Tutorial: Signieren von gespeicherten Prozeduren mit einem Zertifikat](https://docs.microsoft.com/sql/relational-databases/tutorial-signing-stored-procedures-with-a-certificate) 

- Implementieren Sie Transparent Data Encryption (TDE) mit vom Kunden verwalteten Schlüsseln in Azure Key Vault, um die Trennung von Aufgaben zwischen Datenbesitzern und Sicherheitsbesitzern zu ermöglichen. 
  - Lesen Sie dazu den Artikel [Konfigurieren von kundenseitig verwalteten Schlüsseln mit Key Vault für die Azure Storage-Verschlüsselung über das Azure-Portal](../storage/common/storage-encryption-keys-portal.md). 

- Um sicherzustellen, dass ein Datenbankadministrator keine Daten einsehen kann, die als äußerst sensibel eingestuft werden, aber trotzdem Datenbankadministratoraufgaben ausführen kann, können Sie Always Encrypted mit Rollentrennung verwenden. 
  - Weitere Informationen dazu finden Sie in den Artikeln [Übersicht über die Schlüsselverwaltung für Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted), [Schlüsselbereitstellung mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell#KeyProvisionWithRoles) und [Spaltenhauptschlüsselrotation mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell#column-master-key-rotation-with-role-separation). 

- In Fällen, in denen dies nicht durchführbar ist (zumindest nicht ohne große Kosten und Anstrengungen, die das System nahezu unbrauchbar machen können), können Kompromisse durch den Einsatz von kompensierenden Kontrollmechanismen eingegangen werden, etwa: 
  - Menschliches Eingreifen in Prozesse. 
  - Überwachungspfade – Weitere Informationen zur Überwachung finden Sie unter [Überwachen kritischer Sicherheitsereignisse](#audit-critical-security-events).

**Bewährte Methoden:**

- Stellen Sie sicher, dass unterschiedliche Konten für Entwicklungs-/Test- und Produktionsumgebungen verwendet werden. Unterschiedliche Konten helfen dabei, die Trennung von Test- und Produktionssystemen einzuhalten.

- Verzichten Sie darauf, einzelnen Benutzern Berechtigungen zuzuweisen. Verwenden Sie stattdessen konsistent Rollen (Datenbank- oder Serverrollen). Die Verwendung von Rollen hilft bei der Berichterstellung und bei der Problembehandlung von Berechtigungen.

- Verwenden Sie integrierte Rollen, wenn die Berechtigungen genau mit den erforderlichen Berechtigungen übereinstimmen. Wenn die Vereinigungsmenge aller Berechtigungen aus mehreren integrierten Rollen zu einer 100%igen Übereinstimmung führt, können Sie auch mehrere Rollen gleichzeitig zuweisen. 

- Erstellen und verwenden Sie benutzerdefinierte Rollen, wenn integrierte Rollen zu viele oder unzureichende Berechtigungen gewähren. 

- Rollenzuweisungen können auch temporär erfolgen. Dies wird auch als „dynamische Trennung von Aufgaben“ (Dynamic Separation of Duties, DSD) bezeichnet und erfolgt entweder innerhalb von Schritten des SQL Agent-Auftrags in T-SQL oder mithilfe von Azure PIM für RBAC-Rollen. 

- Stellen Sie sicher, dass Datenbankadministratoren keinen Zugriff auf die Verschlüsselungsschlüssel oder Schlüsselspeicher haben, und dass Sicherheitsadministratoren, die Zugriff auf die Schlüssel besitzen, ihrerseits über keinen Zugriff auf die Datenbank verfügen. 

- Stellen Sie immer sicher, dass Sie über einen Überwachungspfad für sicherheitsbezogene Aktionen verfügen. 

- Sie können die Definition der integrierten RBAC-Rollen abrufen, um die verwendeten Berechtigungen anzuzeigen und anhand von Ausschnitten und deren Kumulationen über PowerShell eine benutzerdefinierte Rolle zu erstellen.

- Da jedes Mitglied der db_owner-Datenbankrolle Sicherheitseinstellungen wie Transparent Data Encryption (TDE) oder SLO ändern kann, sollte diese Mitgliedschaft mit Bedacht erteilt werden. Es gibt jedoch viele Aufgaben, die die Berechtigungen von db_owner erfordern. Dazu gehören Aufgaben wie das Ändern von Datenbankeinstellungen. Überwachung spielt in jeder Lösung eine wichtige Rolle.

- Es ist nicht möglich, die Berechtigungen für eine db_owner-Rolle einzuschränken und somit zu verhindern, dass über ein Administratorkonto Benutzerdaten angezeigt werden können. Wenn sich in einer Datenbank hochgradig vertrauliche Daten befinden, kann Always Encrypted verwendet werden, um sicher zu verhindern, dass Benutzer mit db_owner-Berechtigungen oder andere Datenbankadminsistratoren diese anzeigen können.

> [!NOTE]
> Das Erreichen der Trennung von Aufgaben (SoD) stellt für Sicherheits- und Problembehandlungsaufgaben eine Herausforderung dar. Andere Bereiche wie Entwicklungs- und Endbenutzerrollen können einfacher getrennt werden. Die meisten konformitätsbezogenen Kontrollmechanismen ermöglichen die Verwendung alternativer Kontrollfunktionen (z.B. Überwachung), wenn andere Lösungen nicht praktikabel sind.

Für Leser, die sich ausführlicher mit SoD beschäftigen möchten, empfehlen wir die folgenden Ressourcen: 

- Für Azure SQL-Datenbank:  
  - [Steuern und Gewähren des Datenbankzugriffs für SQL-Datenbank und SQL Data Warehouse](sql-database-manage-logins.md)
  - [Engine-Trennung von Aufgaben für den Anwendungsentwickler](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/cc974525(v=sql.100)) 
  - [Trennung von Aufgaben in SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=39269)
  - [Signieren von gespeicherten Prozeduren in SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server)

- Für Azure-Ressourcenverwaltung:
  - [Integrierte Rollen für Azure](../role-based-access-control/built-in-roles.md) 
  - [Benutzerdefinierte Rollen für Azure-Ressourcen](../role-based-access-control/custom-roles.md)
  - [Verwenden von Azure AD Privileged Identity Management für Zugriff mit erhöhten Rechten](https://www.microsoft.com/en-us/itshowcase/using-azure-ad-privileged-identity-management-for-elevated-access)

### <a name="perform-regular-code-reviews"></a>Ausführen von regulären Code Reviews

> Erwähnt in: PCI: 6.3.2, SOC: SDL-3 

Die Trennung von Aufgaben ist nicht auf die Daten in der Datenbank beschränkt, sondern umfasst auch den Anwendungscode. Bösartiger Code kann Sicherheitskontrollen potenziell umgehen. Vor der Bereitstellung von benutzerdefiniertem Code in der Produktion ist es von entscheidender Bedeutung, den Inhalt der Bereitstellung zu überprüfen.

**Implementierung**:

- Verwenden Sie ein Datenbanktool wie Azure Data Studio, das Quellcodeverwaltung unterstützt. 

- Implementieren Sie einen Prozess für die getrennte Codebereitstellung.

- Vor dem Commit in den Hauptbranch muss eine andere Person als der Autor des Codes den Code auf mögliche Risiken durch die Erhöhung von Rechten sowie schädliche Datenänderungen überprüfen, um Schutz vor Betrug und nicht autorisierten Zugriffen sicherzustellen. Dies kann mithilfe von Quellcodeverwaltungsmechanismen erreicht werden.

**Bewährte Methoden:**

- Standardisierung: Es ist hilfreich, eine Standardprozedur zu implementieren, die für alle Codeaktualisierungen befolgt werden muss. 

- Die Sicherheitsrisikobewertung enthält Regeln, die auf übermäßige Berechtigungen, die Verwendung von alten Verschlüsselungsalgorithmen und andere Sicherheitsprobleme innerhalb eines Datenbankschemas prüfen. 

- Weitere Überprüfungen können in einer Qualitätssicherungs- oder Testumgebung mithilfe von Advanced Threat Protection durchgeführt werden. Dabei wird nach Code gesucht, der anfällig für SQL-Einschleusung ist.

- Beispiele dafür, worauf besonders zu achten ist: 
  - Erstellung eines Benutzers oder Ändern von Sicherheitseinstellungen aus einer Bereitstellung einer automatisierten SQL-Codeaktualisierung. 
  - Eine gespeicherte Prozedur, die in Abhängigkeit von den bereitgestellten Parametern einen monetären Wert in einer Zelle auf nicht konforme Weise aktualisiert. 

- Stellen Sie sicher, dass die Person, die die Überprüfung durchführt, eine andere Person als der ursprüngliche Codeautor und mit Code Reviews und sicherer Codierung vertraut ist.

- Stellen Sie sicher, dass alle Quellen für Codeänderungen bekannt sind. Code kann in T-SQL-Skripts enthalten sein. Es kann sich um Ad-hoc-Befehle handeln, die ausgeführt oder in Form von Sichten, Funktionen, Triggern und gespeicherten Prozeduren bereitgestellt werden. Er kann Teil von SQL-Agent-Auftragsdefinitionen (Schritte) sein. Er kann auch aus SSIS-Paketen, Azure Data Factory oder anderen Diensten ausgeführt werden.

## <a name="data-protection"></a>Schutz von Daten

Datenschutz besteht aus einer Reihe von Funktionen zum Schutz wichtiger Informationen vor einer Gefährdung durch Verschlüsselung oder Verschleierung.

> [!NOTE]
> Microsoft bescheinigt Azure SQL-Datenbank die Konformität mit FIPS 140-2 Level 1. Dies erfolgt nach der Verifizierung der strikten Verwendung von akzeptablen FIPS 140-2 Level 1-Algorithmen und validierten FIPS 140-2 Level 1-Instanzen dieser Algorithmen, einschließlich der Konsistenz mit den erforderlichen Schlüssellängen, der Schlüsselverwaltung, der Schlüsselgenerierung und der Schlüsselspeicherung. Diese Bescheinigung soll es unseren Kunden ermöglichen, auf den Bedarf oder die Anforderung für die Verwendung von validierten FIPS 140-2 Level 1-Instanzen bei der Verarbeitung von Daten oder der Bereitstellung von Systemen oder Anwendungen zu reagieren. Wir definieren die Begriffe „mit FIPS 140-2 Level 1 konform“ und „FIPS 140-2 Level 1-Konformität“, die in der obigen Erklärung verwendet werden, um ihre beabsichtigte Anwendbarkeit auf die Verwendung des unterschiedlichen Begriffs „für FIPS 140-2 Level 1 validiert“ durch die US-amerikanische und die kanadische Regierung zu demonstrieren. 


### <a name="encrypt-data-in-transit"></a>Verschlüsseln von Daten während der Übertragung

> Erwähnt in: OSA-Methode Nr. 6, ISO-Kontrollmechanismusfamilie: Kryptografie

Schützt Ihre Daten, während Daten zwischen dem Client und dem Server übertragen werden. Weitere Informationen finden Sie unter [Netzwerksicherheit](#network-security).

### <a name="encrypt-data-at-rest"></a>Verschlüsselung ruhender Daten

> Erwähnt in: OSA-Methode Nr. 6, ISO-Kontrollmechanismusfamilie: Kryptografie

Die Verschlüsselung ruhender Daten ist der kryptografische Schutz von Daten, wenn diese in einer Datenbank, einem Protokoll oder in Sicherungsdateien persistent gespeichert werden.

**Implementierung**:

- [Transparent Database Encryption (TDE)](transparent-data-encryption-azure-sql.md) mit vom Dienst verwalteten Schlüsseln ist standardmäßig für alle Datenbanken aktiviert, die nach 2017 in Azure SQL-Datenbank erstellt wurden.
- Wenn die Datenbank in einer verwalteten Instanz über einen Wiederherstellungsvorgang von einem lokalen Server erstellt wird, wird die TDE-Einstellung der ursprünglichen Datenbank beibehalten. Wenn in der Originaldatenbank TDE nicht aktiviert ist, empfehlen wir, TDE für die verwaltete Instanz manuell zu aktivieren.

**Bewährte Methoden:**

- Speichern Sie keine Daten, für die Verschlüsselung ruhender Daten in der Masterdatenbank erforderlich ist. Die Masterdatenbank kann nicht mit TDE verschlüsselt werden.

- Verwenden Sie von Kunden verwaltete Schlüssel in Azure Key Vault, wenn Sie bessere Transparenz und differenzierte Kontrolle über den TDE-Schutz benötigen. Azure Key Vault bietet die Möglichkeit, Berechtigungen jederzeit zu widerrufen, damit auf die Datenbank nicht mehr zugegriffen werden kann. TDE-Schutzvorrichtungen können zentral mit anderen Schlüsseln verwaltet werden, oder Sie können die TDE-Schutzvorrichtung mithilfe von Azure Key Vault gemäß Ihrem eigenen Zeitplan rotieren.

- Wenn Sie von Kunden verwaltete Schlüssel in Azure Key Vault verwenden, befolgen Sie die Anleitungen in den Artikeln [Richtlinien für das Konfigurieren von TDE mit Azure Key Vault](transparent-data-encryption-byok-azure-sql.md#recommendations-when-configuring-akv) und [Konfigurieren von georedundanter Notfallwiederherstellung mit Azure Key Vault](transparent-data-encryption-byok-azure-sql.md#geo-dr-and-customer-managed-tde).

### <a name="protect-sensitive-data-in-use-from-high-privileged-unauthorized-users"></a>Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern

Daten in Gebrauch sind die Daten, die bei der Ausführung von SQL-Abfragen im Speicher des Datenbanksystems gespeichert werden. Wenn in der Datenbank vertrauliche Daten gespeichert werden, muss Ihre Organisation ggf. sicherstellen, dass Benutzer mit umfassenden Berechtigungen daran gehindert werden, vertrauliche Daten in der Datenbank anzuzeigen. Benutzer mit umfassenden Berechtigungen wie z. B. Microsoft-Operatoren oder DBAs in Ihrer Organisation sollten in der Lage sein, die Datenbank zu verwalten, aber keine vertraulichen Daten anzeigen oder aus dem Arbeitsspeicher des SQL Server-Prozesses oder durch Abfragen der Datenbank extrahieren können.

Die Richtlinien, die festlegen, welche Daten sensibel sind, und welche sensiblen Daten im Speicher verschlüsselt werden müssen und in Klartext für Administratoren nicht zugänglich sind, richten sich nach Ihrer Organisation und den Complianceanforderungen, die Sie erfüllen müssen. Entsprechende Anforderungen finden Sie unter [Identifizieren und Markieren von sensiblen Daten](#identify-and-tag-sensitive-data).

**Implementierung**:

- Verwenden Sie [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine), um sicherzustellen, dass sensible Daten in Azure SQL-Datenbank nicht als Klartext verfügbar gemacht werden (auch dann nicht, wenn sie sich im Arbeitsspeicher bzw. in Gebrauch befinden). Always Encrypted schützt die Daten vor Datenbankadministratoren (DBAs) und Cloudadministratoren (oder böswilligen Benutzern, die sich als hochprivilegierte Benutzer ausgeben können, aber nicht autorisiert sind) und verleiht Ihnen mehr Kontrolle darüber, wer auf Ihre Daten zugreifen kann.

**Bewährte Methoden:**

- Always Encrypted ist kein Ersatz für die Verschlüsselung ruhender Daten (TDE) oder von Daten bei der Übertragung (SSL/TLS). Always Encrypted sollte nicht für nicht vertrauliche Daten verwendet werden, da dies Auswirkungen auf die Leistung und Funktionalität hat. Die Verwendung von Always Encrypted in Verbindung mit TDE und TLS (Transport Layer Security) wird für einen umfassenden Schutz von ruhenden Daten, während der Übertragung und im Gebrauch empfohlen. 

- Bewerten Sie die Auswirkungen einer Verschlüsselung der identifizierten Spalten mit sensiblen Daten, bevor Sie „Always Encrypted“ in einer Produktionsdatenbank bereitstellen. Im Allgemeinen reduziert „Always Encrypted“ die Abfragefunktionalität von verschlüsselten Spalten und bringt weitere Einschränkungen mit sich, die unter [Always Encrypted – Details zur Funktion](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine#feature-details) aufgeführt sind. Daher müssen Sie möglicherweise die Architektur Ihrer Anwendung überarbeiten, um die Funktionalität, die eine Abfrage nicht unterstützt, clientseitig erneut zu implementieren und/oder Ihr Datenbankschema umgestalten (einschließlich der Definition gespeicherter Prozeduren, Vorgänge, Ansichten und Trigger). Vorhandene Anwendungen funktionieren u. U. nicht mit verschlüsselten Spalten, wenn sie den Einschränkungen und Begrenzungen von „Always Encrypted“ nicht entsprechen. Auch wenn das Ökosystem von Microsoft-Tools, -Produkten und -Diensten, die „Always Encrypted“ unterstützen, stetig wächst, funktionieren einige davon nicht mit verschlüsselten Spalten. Das Verschlüsseln einer Spalte kann sich in Abhängigkeit von den Merkmalen Ihrer Workload auch auf die Spaltenabfrageleistung auswirken. 

- Verwalten Sie Always Encrypted-Schlüssel mit Rollentrennung, wenn Sie Always Encrypted verwenden, um Daten vor bösartigen Datenbankadministratoren zu schützen. Mithilfe der Rollentrennung erstellt ein Sicherheitsadministrator die physischen Schlüssel. Der Datenbankadministrator erstellt in der Datenbank Metadatenobjekte für Spaltenhauptschlüssel und Spaltenverschlüsselungsschlüssel, die die physischen Schlüssel beschreiben. Während dieses Vorgangs benötigt der Sicherheitsadministrator keinen Zugriff auf die Datenbank, und der Datenbankadministrator benötigt keinen Zugriff auf die physischen Schlüssel in Klartext. 
  - Weitere Informationen finden Sie im Artikel [Verwalten von Schlüsseln mit Rollentrennung](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted#managing-keys-with-role-separation). 

- Speichern Sie die Spaltenhauptschlüssel in Azure Key Vault, um die Verwaltung zu vereinfachen. Vermeiden Sie die Verwendung des Windows-Zertifikatspeichers (und im Allgemeinen Lösungen mit verteiltem Schlüsselspeicher, im Gegensatz zu Lösungen für die zentrale Schlüsselverwaltung), die die Schlüsselverwaltung erschweren. 

- Berücksichtigen Sie sorgfältig die Vor- und Nachteile der Verwendung mehrerer Schlüssel (Spaltenhauptschlüssel oder Spaltenverschlüsselungsschlüssel). Halten Sie die Anzahl der Schlüssel gering, um die Schlüsselverwaltungskosten zu reduzieren. Ein Spaltenhauptschlüssel und ein Spaltenverschlüsselungsschlüssel pro Datenbank sind in der Regel in Steady-State-Umgebungen (nicht im Zuge einer Schlüsselrotation) ausreichend. Sie benötigen eventuell zusätzliche Schlüssel, wenn Sie über unterschiedliche Benutzergruppen verfügen, die jeweils unterschiedliche Schlüssel verwenden und auf andere Daten zugreifen.  

- Rotieren Sie die Spaltenhauptschlüssel gemäß Ihren Konformitätsanforderungen. Wenn Sie auch Spaltenverschlüsselungsschlüssel rotieren müssen, empfiehlt es sich, Onlineverschlüsselung zu verwenden, um Ausfallzeiten von Anwendungen zu minimieren. 
  - Lesen Sie dazu den Artikel [Überlegungen zu Leistung und Verfügbarkeit](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-column-encryption-using-powershell#performance-and-availability-considerations). 

- Verwenden Sie deterministische Verschlüsselung, wenn Berechnungen (Gleichheit) für Daten unterstützt werden müssen. Verwenden Sie andernfalls Verschlüsselung nach dem Zufallsprinzip. Vermeiden Sie die Verwendung von deterministischer Verschlüsselung für Datasets mit niedriger Entropie oder Datasets mit öffentlich bekannter Verteilung. 

- Wenn Sie Bedenken haben, dass Dritte ohne Ihre Zustimmung legal auf Ihre Daten zugreifen, stellen Sie sicher, dass alle Anwendungen und Tools, die Zugriff auf die Schlüssel und Daten in Klartext besitzen, außerhalb von Microsoft Azure Cloud ausgeführt werden. Ohne Zugriff auf die Schlüssel verfügt der Dritte nicht über die Möglichkeit, die Daten zu entschlüsseln, es sei denn, er umgeht die Verschlüsselung.

- Always Encrypted unterstützt das Erteilen von temporären Zugriffsberechtigungen auf die Schlüssel (und die geschützten Daten) nicht auf einfache Weise. Angenommen, Sie müssen z. B. die Schlüssel für einen Datenbankadministrator freigeben, damit dieser einige Bereinigungsvorgänge für vertrauliche und verschlüsselte Daten durchführen kann. Die einzige Möglichkeit, dem Datenbankadministrator den Zugriff auf die Daten zuverlässig zu entziehen, besteht darin, die Spaltenverschlüsselungsschlüssel und die Spaltenhauptschlüssel, mit denen die Daten geschützt werden, zu rotieren. Dies ist allerdings ein aufwendiger Vorgang. 

- Um auf die Klartextwerte in verschlüsselten Spalten zuzugreifen, muss ein Benutzer über Zugriff auf den CMK verfügen, der die Spalten schützt. Dieser wird im Schlüsselspeicher konfiguriert, der den CMK enthält. Außerdem muss der Benutzer über die Datenbankberechtigungen **VIEW ANY COLUMN MASTER KEY DEFINITION** und **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** verfügen.

### <a name="control-access-of-application-users-to-sensitive-data-through-encryption"></a>Steuern des Zugriffs von Anwendungsbenutzern auf sensible Daten durch Verschlüsselung

Verschlüsselung kann verwendet werden, um sicherzustellen, dass nur bestimmte Anwendungsbenutzer, die Zugriff auf Kryptografieschlüssel besitzen, die Daten anzeigen oder aktualisieren können.

**Implementierung**:

- Verwenden Sie CLE (Column Level Encryption). Weitere Informationen finden Sie im Artikel [Verschlüsseln einer Datenspalte](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data). 
- Verwenden Sie Always Encrypted, aber beachten Sie auch die damit zusammenhängenden Einschränkungen. Die Einschränkungen sind unten aufgeführt.

**bewährten Methoden**

Bei Verwendung von CLE:

- Steuern Sie den Zugriff auf Schlüssel durch SQL-Berechtigungen und -Rollen. 

- Verwenden Sie AES (AES 256 empfohlen) für die Datenverschlüsselung. Algorithmen wie RC4, DES und TripleDES sind veraltet und sollten aufgrund bekannter Sicherheitsrisiken nicht verwendet werden. 

- Schützen Sie symmetrische Schlüssel mit asymmetrischen Schlüsseln/Zertifikaten (nicht mit Kennwörtern), um die Verwendung von 3DES zu vermeiden. 

- Seien Sie vorsichtig, wenn Sie eine Datenbank mithilfe von CLE über Export-/Importfunktionen (bacpac-Dateien) migrieren. 
  - Weitere Informationen dazu, wie Sie den Verlust von Schlüsseln beim Migrieren von Daten verhindern können, sowie weitere Empfehlungen zu bewährten Methoden finden Sie im Artikel [Empfehlungen zur Verwendung von CLE in Azure SQL-Datenbank](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/).

Denken Sie daran, dass Always Encrypted in erster Linie dazu dient, die verwendeten vertraulichen Daten im Gebrauch vor hochprivilegierten Benutzern von Azure SQL-Datenbank (Cloudbetreiber, Datenbankadministratoren) zu schützen. Lesen Sie dazu [Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users). Seien Sie sich der folgenden Herausforderungen bewusst, wenn Sie Always Encrypted verwenden, um Daten vor Anwendungsbenutzern zu schützen:

- Standardmäßig verwalten alle Microsoft-Clienttreiber, die Always Encrypted unterstützen, einen globalen Cache (ein Cache pro Anwendung) von Spaltenverschlüsselungsschlüsseln. Sobald ein Clienttreiber einen Klartext-Spaltenverschlüsselungsschlüssel abruft, indem er einen Schlüsselspeicher kontaktiert, der einen Spaltenhauptschlüssel enthält, wird der Klartext-Spaltenverschlüsselungsschlüssel zwischengespeichert. Dadurch wird die Isolierung von Daten von Benutzern einer Mehrbenutzeranwendung erschwert. Wenn Ihre Anwendung bei der Interaktion mit einem Schlüsselspeicher (z. B. Azure Key Vault) die Identität von Endbenutzern annimmt, wird von einer nachfolgenden Abfrage, die denselben Schlüssel erfordert, aber von einem anderen Benutzer ausgelöst wird, der zwischengespeicherte Schlüssel verwendet, nachdem die Abfrage eines Benutzers den Cache mit einem Spaltenverschlüsselungsschlüssel aufgefüllt hat. Der Treiber ruft den Schlüsselspeicher nicht auf und überprüft nicht, ob der zweite Benutzer über die Berechtigung für den Zugriff auf den Spaltenverschlüsselungsschlüssel verfügt. Folglich kann der Benutzer die verschlüsselten Daten auch dann anzeigen, wenn der Benutzer keinen Zugriff auf die Schlüssel hat. Um Isolation von Benutzern einer Mehrbenutzeranwendung zu erreichen, können Sie das Zwischenspeichern von Spaltenverschlüsselungsschlüsseln deaktivieren. Das Deaktivieren der Zwischenspeicherung führt zu zusätzlichen Leistungseinbußen, da der Treiber den Schlüsselspeicher für jeden Datenverschlüsselungs- oder -entschlüsselungsvorgang kontaktieren muss.

### <a name="protect-data-against-unauthorized-viewing-by-application-users-while-preserving-data-format"></a>Schützen von Daten vor nicht autorisierter Anzeige durch Anwendungsbenutzer unter Beibehaltung des Datenformats
Eine weitere Technik zur Verhinderung der Anzeige von Daten durch unbefugte Benutzer ist die Verschleierung oder Maskierung der Daten unter Beibehaltung der Datentypen und -formate, um sicherzustellen, dass Benutzeranwendungen die Daten weiterhin verarbeiten und anzeigen können.

**Implementierung**:

- Verwenden Sie [dynamische Datenmaskierung](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking), um Tabellenspalten zu verschleiern.

> [!NOTE]
> Always Encrypted funktioniert nicht mit dynamischer Datenmaskierung. Es ist nicht möglich, dieselbe Spalte zu verschlüsseln und zu maskieren. Dies impliziert, dass Sie den Schutz von Daten in Gebrauch im Vergleich mit dem Maskieren der Daten für Ihre App-Benutzer über dynamische Datenmaskierung priorisieren müssen.

**Bewährte Methoden:**

> [!NOTE]
> Dynamische Datenmaskierung kann nicht verwendet werden, um Daten vor Benutzern mit hohen Berechtigungen zu schützen. Maskierungsrichtlinien gelten nicht für Benutzer mit administrativem Zugriff (z.B. db_owner).

- Gestatten Sie App-Benutzern nicht das Ausführen von Ad-hoc-Abfragen (da Sie dynamische Datenmaskierung möglicherweise umgehen können).  
  - Weitere Informationen finden Sie im Artikel [Umgehen der Maskierung mithilfe von Rückschluss- oder Brute-Force-Verfahren](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking#security-note-bypassing-masking-using-inference-or-brute-force-techniques).  

- Verwenden Sie eine geeignete Zugriffssteuerungsrichtlinie (über SQL-Berechtigungen, Rollen, RLS), um die Benutzerberechtigungen für Aktualisierungen in den maskierten Spalten einzuschränken. Das Erstellen einer Maske für eine Spalte verhindert keine Aktualisierungen dieser Spalte. Die Benutzer erhalten beim Abfragen der maskierten Spalte auch maskierte Daten und können die Daten aktualisieren, wenn sie über die entsprechenden Schreibberechtigungen verfügen.    

-  Bei der dynamischen Datenmaskierung werden die statistischen Eigenschaften der maskierten Werte nicht beibehalten. Dies kann sich auf die Abfrageergebnisse auswirken (z. B. auf Abfragen, die eine Filterung der Prädikate oder Joins der maskierten Daten umfassen).

## <a name="network-security"></a>Netzwerksicherheit
Netzwerksicherheit bezieht sich auf Zugriffssteuerungen und bewährte Methoden, um Ihre Daten während der Übertragung in Azure SQL-Datenbank zu sichern.

### <a name="configure-my-client-to-connect-securely-to-azure-sql-database"></a>Konfigurieren des Clients für eine sichere Verbindung mit Azure SQL-Datenbank 
Bewährte Methoden, mit denen Sie verhindern können, dass Clientcomputer und Anwendungen mit bekannten Sicherheitsrisiken (z. B. bei Verwendung älterer TLS-Protokolle und Verschlüsselungssammlungen) eine Verbindung mit Azure SQL-Datenbank herstellen

**Implementierung**:

- Stellen Sie sicher, dass Clientcomputer, die eine Verbindung mit Azure SQL-Datenbank herstellen, [Transport Layer Security (TLS)](sql-database-security-overview.md#transport-layer-security-tls-encryption-in-transit) verwenden.

**Bewährte Methoden:**

- Konfigurieren Sie alle Apps und Tools für das Herstellen einer Verbindung mit SQL-Datenbank mit aktivierter Verschlüsselung. 
  - Encrypt = On, TrustServerCertificate = Off (oder äquivalent mit Nicht-Microsoft-Treibern). 

- Wenn Ihre App einen Treiber verwendet, der TLS nicht unterstützt oder eine ältere Version von TLS unterstützt, ersetzen Sie den Treiber, wenn dies möglich ist. Wenn dies nicht möglich ist, sollten Sie die Sicherheitsrisiken sorgfältig auswerten. 

- Verringern Sie Angriffsvektoren durch Sicherheitsrisiken in SSL 2.0, SSL 3.0, TLS 1.0 und TLS 1.1, indem Sie diese Versionen auf Clientcomputern deaktivieren, die eine Verbindung mit Azure SQL-Datenbank über [TLS-Registrierungseinstellungen (Transport Layer Security)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-10) herstellen. 

- Überprüfen Sie die auf dem Client verfügbaren Verschlüsselungssammlungen: [Verschlüsselungssammlungen in TLS/SSL (Schannel SSP)](https://docs.microsoft.com/windows/desktop/SecAuthN/cipher-suites-in-schannel). Deaktivieren Sie insbesondere 3DES, wie unter [Konfigurieren der Reihenfolge der TLS-Verschlüsselungssammlungen](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order) beschrieben. 

- Bei Azure SQL-Datenbank wird Verschlüsselung für die Verbindungstypen „Proxy“ und „Redirect“ (Umleitung) erzwungen. Wenn Sie eine verwaltete Instanz verwenden, nutzen Sie den Verbindungstyp **Proxy** (Standard), da dies Verschlüsselung auf Serverseite erzwingt. Der Verbindungstyp **Redirect** (Umleitung) unterstützt derzeit keine Verschlüsselungserzwingung und ist nur für private IP-Verbindungen verfügbar. 

- Weitere Informationen finden Sie unter [Verbindungsarchitektur von Azure SQL: Verbindungsrichtlinie](sql-database-connectivity-architecture.md#connection-policy).


### <a name="minimize-attack-surface"></a>Minimieren der Angriffsfläche
Minimieren Sie die Anzahl der Features, die böswillige Benutzer angreifen können. Implementieren Sie Netzwerkzugriffssteuerungen für Azure SQL-Datenbank.

> Erwähnt in: OSA-Methode Nr. 5

**Implementierung**:

Auf einem Azure SQL-Datenbank-Server (mit Singleton-Datenbank oder Pools für elastische Datenbanken):
- Legen Sie den Zugriff auf Azure-Dienste auf OFF (AUS) fest.

- Verwenden Sie VNET-Dienstendpunkte und VNET-Firewallregeln.

- Verwenden Sie Private Link (Vorschau).

In einer verwalteten Instanz:
- Befolgen Sie die Richtlinien unter [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements). 

**Bewährte Methoden:**

- Schränken Sie den Zugriff auf Azure SQL-Datenbank durch Herstellen einer Verbindung mit einem privaten Endpunkt (z.B. mit einem privaten Datenpfad) ein: 
  - Eine verwaltete Instanz kann innerhalb eines VNET isoliert werden, um externen Zugriff zu verhindern. Anwendungen und Tools, die sich im selben VNET oder in einem virtuellen Peernetzwerk in derselben Region befinden, können direkt darauf zugreifen. Anwendungen und Tools in einer anderen Region können VNET-zu-VNET-Verbindungen oder ExpressRoute-Leitungspeering verwenden, um eine Verbindung herzustellen. Der Kunde sollte Netzwerksicherheitsgruppen (NSG) verwenden, um den Zugriff über Port 1433 ausschließlich auf Ressourcen einzuschränken, die Zugriff auf eine verwaltete Instanz benötigen. 
  - Verwenden Sie für einen SQL-Datenbank-Server (mit Singletons oder Pools für elastische Datenbanken) das Feature [Private Link](sql-database-private-endpoint-overview.md), das eine dedizierte private IP-Adresse für den SQL-Datenbank-Server in Ihrem VNET bereitstellt. Sie können auch [VNET-Dienstendpunkte mit VNET-Firewallregeln](sql-database-vnet-service-endpoint-rule-overview.md) verwenden, um den Zugriff auf Ihre SQL-Datenbank-Server einzuschränken.
  - Benutzer von mobilen Geräten sollten Point-to-Site-VPN-Verbindungen verwenden, um eine Verbindung über den Datenpfad herzustellen.
  - Benutzer, die mit Ihrem lokalen Netzwerk verbunden sind, sollten eine Site-to-Site-VPN-Verbindung oder ExpressRoute verwenden, um eine Verbindung über den Datenpfad herzustellen.

- Sie können auf Azure SQL-Datenbank zugreifen, indem Sie eine Verbindung mit einem öffentlichen Endpunkt herstellen (z.B. mithilfe eines öffentlichen Datenpfads). Die folgenden bewährten Methoden sollten in Betracht gezogen werden: 
  - Verwenden Sie für einen SQL-Datenbank-Server [IP-Firewallregeln](sql-database-firewall-configure.md), um den Zugriff nur auf autorisierte IP-Adressen einzuschränken.
  - Verwenden Sie für eine verwaltete Instanz Netzwerksicherheitsgruppen (NSG), um den Zugriff über Port 3342 nur auf die erforderlichen Ressourcen zu beschränken. Weitere Informationen finden Sie unter [Sicheres Verwenden einer verwalteten Azure SQL-Datenbank-Instanz mit öffentlichen Endpunkten](sql-database-managed-instance-public-endpoint-securely.md). 

> [!NOTE]
> Der öffentliche Endpunkt einer verwalteten Instanz ist standardmäßig nicht aktiviert und muss explizit aktiviert werden. Wenn die Unternehmensrichtlinie die Verwendung öffentlicher Endpunkte nicht zulässt, verwenden Sie [Azure Policy](../governance/policy/overview.md), um zu verhindern, dass öffentliche Endpunkte überhaupt aktiviert werden können.

- Einrichten von Azure-Netzwerkkomponenten: 
  - Befolgen Sie die Anleitungen unter [Bewährte Methoden für die Netzwerksicherheit in Azure](../security/fundamentals/network-best-practices.md).
  - Planen Sie die VNET-Konfiguration gemäß den bewährten Methoden, die unter [Azure Virtual Network: Häufig gestellte Fragen (FAQ)](../virtual-network/virtual-networks-faq.md) und im Plan beschrieben werden. 
  - Segmentieren Sie ein VNET in mehrere Subnetze, und weisen Sie dem gleichen Subnetz Ressourcen für eine ähnliche Rolle zu (z.B. Front-End- im Vergleich zu Back-End-Ressourcen).
  - Verwenden Sie [Netzwerksicherheitsgruppen (NSGs)](../virtual-network/security-overview.md), um den Datenverkehr zwischen Subnetzen innerhalb der Azure-VNET-Grenze zu steuern.
  - Aktivieren Sie [Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) für Ihr Abonnement, um den eingehenden und ausgehenden Netzwerkdatenverkehr zu überwachen.

### <a name="configure-power-bi-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von Power BI für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Verwenden Sie für Power BI Desktop nach Möglichkeit den privaten Datenpfad. 

- Stellen Sie sicher, dass Power BI Desktop mithilfe von TLS 1.2 eine Verbindung herstellt, indem Sie den Registrierungsschlüssel auf dem Clientcomputer gemäß den [TLS-Registrierungseinstellungen (Transport Layer Security)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) festlegen. 

- Beschränken Sie den Datenzugriff für bestimmte Benutzer über [Sicherheit auf Zeilenebene (Row-Level Security, RLS) mit Power BI](https://docs.microsoft.com/power-bi/service-admin-rls). 

- Verwenden Sie für den Power BI-Dienst das [lokale Datengateway](https://docs.microsoft.com/power-bi/service-gateway-onprem), und beachten Sie dabei die [Einschränkungen und Überlegungen](https://docs.microsoft.com/power-bi/service-gateway-deployment-guidance#installation-considerations-for-the-on-premises-data-gateway).

### <a name="configure-app-service-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von App Service für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Für eine einfache Web-App muss für das Herstellen einer Verbindung über einen öffentlichen Endpunkt **Allow Azure Services** (Azure-Dienste zulassen) auf ON (Ein) festgelegt werden. 

- [Integrieren Sie Ihre App in ein virtuelles Azure-Netzwerk](../app-service/web-sites-integrate-with-vnet.md), einem Verbindung über einen privaten Datenpfad mit einer verwalteten Instanz bereitzustellen. Optional können Sie eine Web-App auch mit [App Service-Umgebungen (ASE)](../app-service/environment/intro.md) bereitstellen. 

- Für eine Web-App mit ASE oder eine VNET-integrierte Web-App, die eine Verbindung mit einer Datenbank in SQL-Datenbank-Server herstellt, können Sie [VNET-Dienstendpunkte und VNET-Firewallregeln](sql-database-vnet-service-endpoint-rule-overview.md) verwenden, um den Zugriff aus einem bestimmten VNET und Subnetz einzuschränken. Legen Sie dann **Azure-Dienste zulassen** auf „AUS“ fest. Sie können ASE auch über einen privaten Datenpfad mit einer verwalteten Instanz verbinden.  

- Stellen Sie sicher, dass Ihre Web-App wie im Artikel [Bewährte Methoden zum Schützen webbasierter und mobiler PaaS-Anwendungen mit Azure App Service](../security/security-paas-applications-using-app-services.md) beschrieben konfiguriert ist. 

- Installieren Sie die [Web Application Firewall (WAF)](../application-gateway/waf-overview.md), um Ihre Webanwendung vor gängigen Exploits und Sicherheitsrisiken zu schützen.

### <a name="configure-azure-vm-hosting-for-secure-connections-to-azure-sql-database"></a>Konfigurieren von Azure VM-Hosting für sichere Verbindungen mit Azure SQL-Datenbank

**Bewährte Methoden:**

- Verwenden Sie eine Kombination aus Zulassungs- und Ablehnungsregeln für die NSGs von Azure-VMs, um zu steuern, auf welche Regionen von der VM aus zugegriffen werden kann.

- Stellen Sie sicher, dass Ihre VM gemäß dem Artikel [Bewährten Sicherheitsmethoden für IaaS-Workloads in Azure](../security/azure-security-iaas.md) konfiguriert ist.

- Stellen Sie sicher, dass alle VMs einem bestimmten VNET und einem Subnetz zugeordnet sind. 

- Finden Sie heraus, ob Sie die Standardroute 0.0.0.0/Internet gemäß der Anleitung unter [Informationen zu erzwungenem Tunneling](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md#about-forced-tunneling) benötigen. 
  - Wenn dies der Fall ist (z.B. bei einem Front-End-Subnetz), behalten Sie die Standardroute bei.
  - Wenn dies nicht der Fall ist (z.B. mittlere Ebene oder Back-End-Subnetz), aktivieren Sie erzwungenes Tunneling, sodass kein Datenverkehr über das Internet in die lokale Umgebung (also standortübergreifend) gelangt. 

- Implementieren Sie [optionale Standardrouten](../virtual-network/virtual-networks-udr-overview.md#optional-default-routes), wenn Sie Peering verwenden oder eine Verbindung mit einer lokalen Umgebung herstellen. 

- Implementieren Sie [benutzerdefinierte Routen](../virtual-network/virtual-networks-udr-overview.md#user-defined), wenn Sie den gesamten Datenverkehr im VNET für die Paketüberprüfung an ein virtuelles Netzwerkgerät senden müssen. 

- Verwenden Sie [VNET-Dienstendpunkte](sql-database-vnet-service-endpoint-rule-overview.md) für den sicheren Zugriff auf PaaS-Dienste wie Azure Storage über das Azure-Backbonenetzwerk. 

### <a name="protect-against-distributed-denial-of-service-ddos-attacks"></a>Schutz vor DDoS-Angriffen (verteilte Denial-of-Service-Angriffe).
DDoS-Angriffe (verteilte Denial-of-Service-Angriffe) werden von böswilligen Benutzern verwendet, um eine Flut von Netzwerkdatenverkehr an Azure SQL-Datenbank zu senden. Das Ziel besteht dabei darin, die Azure-Infrastruktur zu überlasten und damit gültige Anmeldungen und Workloads abzulehnen.

> Erwähnt in: OSA-Methode Nr. 9

**Implementierung**:

DDoS-Schutz wird im Rahmen der Azure-Plattform automatisch aktiviert. Er umfasst ununterbrochene Datenverkehrsüberwachung sowie Risikominderung in Echtzeit von Angriffen auf öffentliche Endpunkte auf Netzwerkebene. 

- Verwenden Sie [Azure DDoS Protection](../virtual-network/ddos-protection-overview.md), um öffentliche IP-Adressen zu überwachen, die in VNETs bereitgestellten Ressourcen zugeordnet sind.

- Verwenden Sie [Advanced Threat Protection für Azure SQL-Datenbank](sql-database-threat-detection-overview.md), um DOS-Angriffe (Denial-of-Service) auf Datenbanken zu erkennen.

**Bewährte Methoden:**

- Befolgen Sie die unter [Minimieren der Angriffsfläche](#minimize-attack-surface) beschriebenen Verfahren, um Bedrohungen durch DDoS-Angriffe zu minimieren. 

- Die Advanced Threat Protection-Warnung **Brute-Force-SQL-Anmeldeinformationen** hilft, Brute-Force-Angriffe zu erkennen. In einigen Fällen kann die Warnung sogar Workloads für Penetrationstests unterscheiden. 

- Für Azure-VM-Hostinganwendungen, die eine Verbindung mit SQL-Datenbank herstellen: 
  - Befolgen Sie die Empfehlungen zum Einschränken des Zugriffs über Endpunkte mit Internetzugriff in Azure Security Center. 
  - Verwenden Sie VM-Skalierungsgruppen, um mehrere Instanzen der Anwendung auf Azure-VMs auszuführen. 
  - Deaktivieren Sie RDP und SSH aus dem Internet, um Brute-Force-Angriffe zu verhindern. 

## <a name="monitoring-logging-and-auditing"></a>Überwachung, Protokollierung und Überprüfung  
Dieser Abschnitt beschreibt die Möglichkeiten zum Erkennen anomaler Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese missbräuchlich zu nutzen. Außerdem werden bewährte Methoden zum Konfigurieren der Datenbanküberwachung für die Nachverfolgung und Erfassung von Datenbankereignissen beschrieben.

### <a name="protect-databases-against-attacks"></a>Schützen von Datenbanken vor Angriffen 
Mit Advanced Threat Protection können Sie potenzielle Bedrohungen erkennen, sobald diese auftreten, und darauf reagieren, indem Sie Sicherheitswarnungen zu anomalen Aktivitäten einrichten.

**Implementierung**:

- Verwenden Sie [Advanced Threat Protection für SQL](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts), um ungewöhnliche und möglicherweise schädliche Versuchen zu erkennen, auf Datenbanken zuzugreifen oder diese unbefugt zu nutzen, etwa mithilfe von:
  - Angriff mit Einschleusung von SQL-Befehlen.
  - Diebstahl/Offenlegung von Anmeldeinformationen.
  - Missbrauch von Berechtigungen.
  - Datenexfiltration.

**Bewährte Methoden:**

- Konfigurieren Sie [Advanced Data Security (ADS)](sql-database-advanced-data-security.md#getting-started-with-ads) für Azure SQL-Datenbank für einen bestimmten SQL-Datenbank-Server oder eine verwaltete Instanz. Sie können ADS auch für alle SQL-Datenbank-Server und verwalteten Instanzen in einem Abonnement konfigurieren, indem Sie in den [Standard-Tarif von Azure Security Center](../security-center/security-center-pricing.md) wechseln. 

- Für eine vollständige Untersuchung wird empfohlen, die  [SQL-Datenbanküberwachung](sql-database-auditing.md) zu aktivieren. Mit der Überwachung können Sie Datenbankereignisse nachverfolgen und in ein Überwachungsprotokoll in einem Azure Storage-Konto oder Azure Log Analytics-Arbeitsbereich schreiben. 

### <a name="audit-critical-security-events"></a>Überwachen kritischer Sicherheitsereignisse
Das Nachverfolgen von Datenbankereignissen kann Ihnen dabei helfen, die Datenbankaktivität besser zu verstehen. Sie gewinnen Erkenntnisse zu Abweichungen und Anomalien, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können. Außerdem wird die Einhaltung von Compliancestandards ermöglicht und erleichtert. 

**Implementierung**:

- Aktivieren Sie  [SQL-Datenbank-Überwachung](sql-database-auditing.md), um Datenbankereignisse nachzuverfolgen und diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto, Ihrem Log Analytics-Arbeitsbereich (Vorschau) oder in Event Hubs (Vorschau) zu schreiben. 

- Überwachungsprotokolle können in ein Azure Storage-Konto, in einen Log Analytics-Arbeitsbereich für die Nutzung durch Azure Monitor-Protokolle oder in Event Hub für die Nutzung durch einen Event Hub geschrieben werden. Sie können eine beliebige Kombination dieser Optionen konfigurieren, und die Überwachungsprotokolle werden in die jeweils angegebenen Speicherorte geschrieben. 

**Bewährte Methoden:**

- Wenn Sie [SQL-Datenbank-Überwachung](sql-database-auditing.md) auf dem Datenbankserver zum Überwachen von Ereignissen konfigurieren, werden alle vorhandenen und neu erstellten Datenbanken auf diesem Server überwacht.
- Standardmäßig umfasst die Überwachungsrichtlinie alle Aktionen (Abfragen, gespeicherte Prozeduren und erfolgreiche und fehlgeschlagene Anmeldungen) für die Datenbanken, was zu einer großen Anzahl von Überwachungsprotokollen führen kann. Es wird Kunden empfohlen, die [Überwachung für verschiedene Arten von Aktionen und Aktionsgruppen mithilfe von PowerShell zu konfigurieren](sql-database-auditing.md#subheading-7). Durch diese Konfiguration können sie die Anzahl der überwachten Aktionen steuern und das Risiko von Ereignisverlusten minimieren. Das Konfigurieren einer benutzerdefinierten Überwachung ermöglicht Kunden, nur die wirklich benötigten Überwachungsdaten zu erfassen.
- Überwachungsprotokolle können direkt im [Azure-Portal](https://portal.azure.com/) oder aus dem konfigurierten Speicherort genutzt werden. 


> [!NOTE]
> Wenn Sie die Überwachung mit Log Analytics aktivieren, fallen Kosten an, deren Höhe sich nach der Erfassungsrate richtet. Beachten Sie die entsprechenden Kosten bei der Verwendung dieser [Option](https://azure.microsoft.com/pricing/details/monitor/), oder speichern Sie die Überwachungsprotokolle in einem Azure-Speicherkonto. 

**Weitere Ressourcen**:

- [SQL-Datenbanküberwachung](sql-database-auditing.md)
- [SQL Server-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) 

### <a name="secure-audit-logs"></a>Sichern von Überwachungsprotokollen
Beschränken Sie den Zugriff auf das Speicherkonto, um die Trennung von Aufgaben zu fördern und auch DBA und Prüfer voneinander zu trennen. 

**Implementierung**:

- Beim Speichern von Überwachungsprotokollen in Azure Storage müssen Sie sicherstellen, dass der Zugriff auf das Speicherkonto gemäß den minimalen Sicherheitsprinzipien beschränkt wird. Legen Sie genau fest, wer Zugriff auf das Speicherkonto hat.
    - Weitere Informationen finden Sie unter [Autorisierung des Zugriffs auf Azure Storage](../storage/common/storage-auth.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

**Bewährte Methoden:**

- Das Steuern des Zugriffs auf das Überwachungsziel ist ein wesentliches Konzept beim Trennen von DBA von Prüfern. 

- Wenn Sie den Zugriff auf sensible Daten überwachen, sollten Sie die Daten mit Datenverschlüsselung sichern, um die Offenlegung von Informationen für den Prüfer zu vermeiden. Weitere Informationen finden Sie im Abschnitt [Schützen von sensiblen Daten im Gebrauch vor hochprivilegierten, nicht autorisierten Benutzern](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users).

## <a name="security-management"></a>Sicherheitsverwaltung

In diesem Abschnitt werden die verschiedenen Aspekte und bewährten Methoden für die Verwaltung Ihres Datenbanksicherheitsstatus beschrieben. Er enthält bewährte Methoden, um sicherzustellen, dass Ihre Datenbanken so konfiguriert sind, dass sie Sicherheitsstandards erfüllen, um den Zugriff auf potenziell sensible Daten in ihren Datenbanken zu ermitteln, zu klassifizieren und nachzuverfolgen. 

### <a name="ensure-that-the-databases-are-configured-to-meet-security-best-practices"></a>Stellen Sie sicher, dass die Datenbanken für bewährte Sicherheitsmethoden konfiguriert sind. 

Optimieren Sie Ihre Datenbanksicherheit proaktiv, indem Sie potenzielle Datenbanksicherheitsrisiken ermitteln und beheben.

**Implementierung**:

- Aktivieren Sie die [SQL-Sicherheitsrisikobewertung](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) (Vulnerability Assessment, VA), um Ihre Datenbank auf Sicherheitsprobleme zu überprüfen und die Sicherheitsrisikobewertung automatisch regelmäßig für Ihre Datenbanken auszuführen.

**Bewährte Methoden:**

- Führen Sie die Sicherheitsrisikobewertung (VA) zunächst für Ihre Datenbanken aus, und durchlaufen Sie dann die fehlerhaften Überprüfungen, die nicht den bewährten Methoden der Sicherheit entsprechen. Richten Sie eine Baseline für akzeptable Konfigurationen ein, die eine Überprüfung mit _ordnungsgemäßen_ Ergebnissen ermöglichen oder dazu führen, dass alle Überprüfungen erfolgreich abgeschlossen werden.  

- Konfigurieren Sie regelmäßige wiederkehrende Überprüfungen so, dass sie ein Mal pro Woche ausgeführt werden, und legen Sie die relevante Person für den Empfang von Zusammenfassungs-E-Mails fest. 

- Überprüfen Sie die Zusammenfassung der Sicherheitsrisikobewertung nach jedem wöchentlichen Scan. Für alle gefundenen Sicherheitsrisiken sollten Sie die Abweichung vom vorherigen Überprüfungsergebnis auswerten und ermitteln, ob die Überprüfung korrigiert werden soll. Überprüfen Sie, ob es einen legitimen Grund für eine Änderung der Konfiguration gibt.   

- Lösen Sie Überprüfungen auf, und aktualisieren Sie ggf. die Baselines. Erstellen Sie Ticketelemente zum Auflösen von Aktionen, und verfolgen Sie diese, bis sie aufgelöst wurden. 

**Weitere Ressourcen**:

- [Sicherheitsrisikobewertung mit der SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) 
- [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](sql-vulnerability-assessment.md)

### <a name="identify-and-tag-sensitive-data"></a>Identifizieren und Markieren von sensiblen Daten 

Ermitteln Sie Spalten, die potenziell vertrauliche Daten enthalten. Welche Daten als sensibel betrachtet werden, hängt zum Großteil vom Kunden, den Complianceanforderungen usw. ab und muss von dem Benutzer, der für die Daten verantwortlich ist, bewertet werden. Klassifizieren Sie die Spalten, um erweiterte vertraulichkeitsbasierte Überwachungs- und Schutzszenarien zu nutzen. 

**Implementierung**:

- Verwenden Sie die [SQL-Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md), um sensible Daten in Ihren Datenbanken zu ermitteln, zu klassifizieren, zu markieren und zu schützen. 
  - Zeigen Sie die Klassifizierungsempfehlungen an, die bei der automatisierten Ermittlung auf dem Dashboard für die SQL-Datenermittlung und -Klassifizierung erstellt werden. Akzeptieren Sie die relevanten Klassifizierungen, damit Ihre vertraulichen Daten dauerhaft mit Klassifizierungsbezeichnungen versehen werden. 
  - Fügen Sie manuell Klassifizierungen für alle zusätzlichen sensiblen Datenfelder hinzu, die nicht vom automatisierten Mechanismus erkannt wurden. 
- Weitere Informationen finden Sie unter [SQL-Datenermittlung und -klassifizierung](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification).

**Bewährte Methoden:**

- Überwachen Sie das Klassifizierungsdashboard in regelmäßigen Abständen für eine genaue Bewertung des Klassifizierungszustands der Datenbank. Ein Bericht zum Datenbankklassifizierungsstatus kann exportiert oder ausgegeben werden, um für Kompatibilitäts- und Überwachungszwecke zur Verfügung zu stehen.

- Überwachen Sie kontinuierlich den Status der empfohlenen vertraulichen Daten in der SQL-Sicherheitsrisikobewertung. Verfolgen Sie die Ermittlungsregel für vertrauliche Daten, und identifizieren Sie jegliche Abweichung in den empfohlenen Spalten für die Klassifizierung.  

- Verwenden Sie die Klassifizierung so, dass sie auf die spezifischen Anforderungen Ihrer Organisation zugeschnitten ist. Passen Sie Ihre Information Protection-Richtlinie (Vertraulichkeitsbezeichnungen, Informationstypen, Ermittlungslogik) in der [SQL Information Protection](../security-center/security-center-info-protection-policy.md)-Richtlinie in Azure Security Center an. 

### <a name="track-access-to-sensitive-data"></a>Nachverfolgen von Zugriff auf sensible Daten 
Überwachen Sie, wer auf sensible Daten zugreift, und erfassen Sie Abfragen für sensible Daten in Überwachungsprotokollen.

**Implementierung**:

- Verwenden Sie SQL-Überwachung und -Datenklassifizierung in Kombination. 
  - Im Protokoll der [SQL-Datenbanküberwachung](sql-database-auditing.md) können Sie ausdrücklich den Zugriff auf vertrauliche Daten nachverfolgen. Sie können auch Informationen wie die Daten anzeigen, auf die zugegriffen wurde, sowie deren Vertraulichkeitsbezeichnung. Weitere Informationen finden Sie unter [Überwachen des Zugriffs auf vertrauliche Daten](sql-database-data-discovery-and-classification.md#subheading-3). 

**Bewährte Methoden:**

- Weitere Informationen finden Sie unter den bewährten Methoden der Abschnitte zur Überwachung und Datenklassifizierung: 
  - [Überwachen kritischer Sicherheitsereignisse](#audit-critical-security-events) 
  - [Identifizieren und Markieren von sensiblen Daten](#identify-and-tag-sensitive-data) 

### <a name="visualize-security-and-compliance-status"></a>Visualisieren des Sicherheits- und Compliancestatus 

Verwenden Sie ein einheitliches Infrastruktur-Sicherheitsverwaltungssystem, das den Sicherheitsstatus ihrer Rechenzentren (einschließlich der SQL-Datenbanken) stärkt. Zeigen Sie eine Liste mit Empfehlungen für die Sicherheit Ihrer Datenbanken und den Compliancestatus an.

**Implementierung**:

- Überwachen Sie SQL-bezogene Sicherheitsempfehlungen und aktive Bedrohungen in [Azure Security Center](https://azure.microsoft.com/documentation/services/security-center/).

## <a name="common-security-threats-and-potential-mitigations"></a>Allgemeine Sicherheitsbedrohungen und potenzielle Risikominderungen

In diesem Abschnitt finden Sie Sicherheitsmaßnahmen zum Schutz vor bestimmten Angriffsvektoren. Es wird erwartet, dass die meisten Risikominderungen erreicht werden können, indem Sie mindestens eine der oben aufgeführten Sicherheitsrichtlinien befolgen.

### <a name="security-threat-data-exfiltration"></a>Sicherheitsbedrohung: Datenexfiltration

„Datenexfiltration“ bezeichnet das nicht autorisierte Kopieren, Übertragen oder Abrufen von Daten von einem Computer oder Server. Weitere Informationen finden Sie in der Definition von [Datenexfiltration](https://en.wikipedia.org/wiki/Data_exfiltration) auf Wikipedia.

Das Herstellen einer Verbindung mit dem Azure SQL-Datenbank-Server über einen öffentlichen Endpunkt stellt ein Datenexfiltrationsrisiko dar, da Kunden ihre Firewalls für öffentliche IP-Adressen öffnen müssen.  

**Szenario 1**: Eine Anwendung auf einer Azure-VM stellt eine Verbindung mit einer Datenbank auf einem Azure SQL-Datenbank-Server her. Ein nicht autorisierter Akteur erhält Zugriff auf den virtuellen Computer und kompromittiert ihn. In diesem Szenario bedeutet Datenexfiltration, dass eine externe Entität, die die nicht autorisierte VM verwendet, eine Verbindung mit der Datenbank herstellt, personenbezogene Daten kopiert und diese in einem Blobspeicher oder einer anderen SQL-Datenbank in einem anderen Abonnement speichert.

**Szenario 2**: Ein nicht autorisierter DBA. Dieses Szenario wird häufig von sicherheitssensiblen Kunden aus regulierten Branchen ausgelöst. In diesem Szenario kann ein Benutzer mit hohen Berechtigungen Daten aus Azure SQL-Datenbank in ein anderes Abonnement kopieren, das nicht vom Datenbesitzer gesteuert wird.

**Mögliche Risikominderungen**:

Derzeit bietet Azure SQL-Datenbank die folgenden Techniken zum Mindern von Bedrohungen durch Datenexfiltration: 

- Verwenden Sie eine Kombination aus Zulassungs- und Ablehnungsregeln für die NSGs von Azure-VMs, um zu steuern, auf welche Regionen von der VM aus zugegriffen werden kann. 
- Wenn ein Azure SQL-Datenbank-Server (mit Singletons oder Pools für elastische Datenbanken) verwendet wird, legen Sie folgende Optionen fest:
  - „Allow Azure Services“ (Azure-Dienste zulassen) auf OFF (Aus).
  - Erlauben Sie nur Datenverkehr aus dem Subnetz, das Ihre Azure-VM enthält, indem Sie eine VNET-Firewallregel einrichten.
  - Verwenden von [Private Link](sql-database-private-endpoint-overview.md)
- Für eine verwaltete Instanz wird bei Verwendung des privaten IP-Zugriffs standardmäßig das erste Datenexfiltrationsproblem eines nicht autorisierten virtuellen Computers berücksichtigt. Aktivieren Sie die Funktion für Subnetzdelegierung für ein Subnetz so, dass automatische die restriktivste Richtlinie für ein Subnetz einer verwalteten Instanz festgelegt wird.
- Das Problem eines nicht autorisierte DBAs wird mit einer verwalteten Instanz offensichtlicher, da sie über eine größere Oberfläche verfügt und die Netzwerkanforderungen für Kunden sichtbar sind. Die beste Risikominderung besteht unter diesen Umständen im Anwenden aller Methoden in diesem Sicherheitsleitfaden, um das Szenario mit einem nicht autorisierten DBA von vornherein zu verhindern (nicht nur für die Datenexfiltration). Always Encrypted ist eine Methode, um sensible Daten zu schützen, indem diese verschlüsselt werden und der Schlüssel für den Datenbankadministrator nicht zugänglich ist.

## <a name="security-aspects-of-business-continuity-and-availability"></a>Sicherheitsaspekte von Geschäftskontinuität und Verfügbarkeit

Bei den meisten Sicherheitsstandards wird die Verfügbarkeit von Daten im Hinblick auf die betriebliche Kontinuität behandelt, indem Redundanz und Failoverfunktionen implementiert werden, um Single Points of Failure zu vermeiden. In Notfallszenarien ist es üblich, Sicherungen von Daten- und Protokolldateien aufzubewahren. Der folgende Abschnitt enthält eine allgemeine Übersicht über die in Azure integrierten Funktionen. Außerdem werden zusätzliche Optionen angegeben, die für bestimmte Anforderungen konfiguriert werden können: 

- Azure bietet integrierte Hochverfügbarkeit: [Hochverfügbarkeit und Azure SQL-Datenbank](sql-database-high-availability.md) 

- Die unternehmenskritische Ebene umfasst Failovergruppen, mehrere Verfügbarkeitszonen, vollständige und differenzielle Protokollsicherungen und standardmäßig aktivierte Sicherungen für die Point-in-Time-Wiederherstellung:  
  - [Hochverfügbarkeit und Azure SQL-Datenbank: Zonenredundante Konfiguration](sql-database-high-availability.md#zone-redundant-configuration)
  - [Automatisierte Sicherungen](sql-database-automated-backups.md)
  - [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen: Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)

- Zusätzliche Geschäftskontinuitätsfeatures wie automatische Failovergruppen für verschiedene geografische Azure-Standorte können wie folgt konfiguriert werden: [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](sql-database-business-continuity.md)

## <a name="next-steps"></a>Nächste Schritte

- Siehe [Übersicht über die Sicherheitsfunktionen von Azure SQL-Datenbank](sql-database-security-overview.md)
