---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Supermood | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Supermood konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 12/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: acb3704541e4c573da4d5331ee10a9c7256162e1
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640158"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-supermood"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Supermood

In diesem Tutorial erfahren Sie, wie Sie Supermood in Azure Active Directory (Azure AD) integrieren. Die Integration von Supermood in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Supermood hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Supermood anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Supermood-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Supermood unterstützt **SP- und IDP**-initiiertes einmaliges Anmelden.
* Supermood unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-supermood-from-the-gallery"></a>Hinzufügen von Supermood aus dem Katalog

Zum Konfigurieren der Integration von Supermood in Azure AD müssen Sie Supermood aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Supermood** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Supermood** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-supermood"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Supermood

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Supermood mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Supermood eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Supermood die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Supermood](#configure-supermood-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Supermood-Testbenutzers](#create-supermood-test-user)** , um eine Entsprechung für B. Simon in Supermood zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Supermood** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten** Modus konfigurieren möchten:

    a. Aktivieren Sie **Zusätzliche URLs festlegen**.
    
    b. Geben Sie im Textfeld **Relayzustand** eine URL ein: `https://supermood.co/auth/sso/saml20`

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten** Modus konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://supermood.co/app/#!/loginv2`.

1. Klicken Sie auf **Speichern**.

1. Die Supermood-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt die Liste der Standardattribute.

    ![image](common/default-attributes.png)

1. Darüber hinaus wird von der Supermood-Anwendung erwartet, dass in der SAML-Antwort noch einige weitere Attribute zurückgegeben werden (siehe unten). Diese Attribute werden ebenfalls vorab aufgefüllt, Sie können sie jedoch nach Bedarf überprüfen.

    | Name | Quellattribut|
    | ---------------| ------|
    | firstName | user.givenname |
    | lastName | user.surname |

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Supermood gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Supermood** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen**.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-supermood-sso"></a>Konfigurieren des einmaligen Anmeldens für Supermood

1. Melden Sie sich im Supermood.co-Administratorbereich als Sicherheitsadministrator an.

1. Klicken Sie unten links auf **My account** (Mein Konto) und auf **Single Sign On (SSO)** (Einmaliges Anmelden).

    ![SSO-Zertifikat](./media/supermood-tutorial/tutorial_supermood_single.png)

1. Klicken Sie unter **Your SAML 2.0 configurations** (Meine SAML 2.0-Konfigurationen) auf **Add an SAML 2.0 configuration for an email domain** (E-Mail-Domäne eine SAML 2.0-Konfiguration hinzufügen).

    ![Hinzufügen eines Zertifikats](./media/supermood-tutorial/tutorial_supermood_add.png)

1. Führen Sie im Abschnitt **Add an SAML 2.0 configuration for an email domain** (E-Mail-Domäne eine SAML 2.0-Konfiguration hinzufügen) die folgenden Schritte aus:

    ![SAML-Zertifikat](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. Geben Sie im Textfeld **Email domain for this Identity provider** (E-Mail-Domäne dieses Identitätsanbieters) Ihre Domäne ein.

    b. Fügen Sie im Textfeld **Use a metadata URL** (Metadaten-URL verwenden) die **App-Verbundmetadaten-URL** ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Klicken Sie auf **Hinzufügen**.

### <a name="create-supermood-test-user"></a>Erstellen eines Supermood-Testbenutzers

In diesem Abschnitt wird eine Benutzerin mit dem Namen Britta Simon in Supermood erstellt. Supermood unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Supermood vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt. Wenn Sie einen Benutzer manuell erstellen möchten, wenden Sie sich an das [Supermood-Supportteam](mailto:hello@supermood.fr).

## <a name="test-sso"></a>Testen des einmaligen Anmeldens 

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Supermood“ klicken, sollten Sie automatisch bei der Supermood-Anwendung angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Was ist der bedingte Zugriff in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Supermood mit Azure AD ausprobieren](https://aad.portal.azure.com/)