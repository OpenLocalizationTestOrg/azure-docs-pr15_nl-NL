<properties
    pageTitle="Zelfstudie: Azure Active Directory-integratie met Evidence.com | Microsoft Azure"
    description="Leer hoe u eenmalige aanmelding tussen Azure Active Directory en Evidence.com configureren."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Zelfstudie: Azure Active Directory-integratie met Evidence.com

Het doel van deze zelfstudie is voor het instellen van eenmalige aanmelding tussen Azure Active Directory (AAD) en Evidence.com. Het overzicht in deze zelfstudie scenario wordt ervan uitgegaan dat u al de volgende items hebt:
    
* Een geldig abonnement op Microsoft Azure
* Een abonnement Evidence.com met eenmalige aanmelding ingeschakeld (e-mail earlyaccess@evidence.com als SAML-gebaseerde eenmalige aanmelding niet is ingeschakeld)

Na het voltooien van deze zelfstudie, kunnen de AAD-gebruikers aan wie u Evidence.com toegang hebt toegewezen één Meld u aan bij de toepassing via het Configuratiescherm AAD-toegang.

## <a name="add-evidencecom-to-your-directory"></a>Evidence.com toevoegen aan uw adreslijst

In dit gedeelte vindt u een overzicht van het toevoegen van Evidence.com als een toepassing integrated in Azure Active Directory.

**De toepassingsintegratie voor bewijs inschakelen:**

1.  Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

2.  Selecteer de map waarvoor u wilt inschakelen directory-integratie in de lijst **Directory** .

3.  De toepassingen om weergave te openen, in de directoryweergave, klikt u in het bovenste menu op **toepassingen** .

4.  Als u wilt openen in de galerie-toepassing, klikt u op **toevoegen**en klik vervolgens op **toevoegen een toepassing in de galerie**.

5.  Typ in het zoekvak **Evidence.com**.

6.  Selecteer **Evidence.com**in het deelvenster met resultaten en klik vervolgens op **voltooid** als de toepassing wilt toevoegen.


## <a name="configuring-single-sign-on"></a>Eenmalige aanmelding configureren

In dit gedeelte vindt u een overzicht van het inschakelen van gebruikers worden geverifieerd bij Evidence.com met hun account in Azure Active Directory, met Federatie op basis van het SAML-protocol.

**Als u wilt configureren eenmalige aanmelding, moet u de volgende stappen uitvoeren:**

1.  Na het toevoegen van Evidence.com in de klassieke Azure-portal, klikt u op **Configureren eenmalige aanmelding**. 
 
2.  Klik op het volgende scherm Selecteer **Azure AD eenmalige aanmelding**en klik vervolgens op **volgende**.

3.  Voer in het scherm configureren App-URL de URL waar gebruikers wordt Meld u aan bij de URL van de tenant Evidence.com (voorbeeld: https://yourtenant.evidence.com en klik op **volgende**. 

4.  Klik op de koppeling **Certificaat downloaden** en sla deze op uw lokale harde schijf. Dit certificaat en de metagegevens URL's (entiteit-ID, Eenmalige aanmelding In URL en aanmelding af URL) wordt gebruikt voor het instellen van eenmalige aanmelding op Evidence.com-site. 

5.  In een afzonderlijk browservenster, meld u aan bij uw Evidence.com tenant als een beheerder en navigeer naar de Tab **beheerder**
      
6.  Klik op **Bureau eenmalige aanmelding**
 
7.  Selecteer **op SAML gebaseerde eenmalige op**
 
8.  Kopieer de **URL van de uitgever**, **Eenmalige aanmelding** en **Één afmelden** waarden weergegeven in de portal van Azure klassieke en naar het overeenkomende veld in Evidence.com.

9.  Open het certificaat gedownload in stap 4 met behulp van een teksteditor zoals Notepad.exe, en kopieer en plak de inhoud in het vak **Beveiligingscertificaat** . 

10. Sla de configuratie in Evidence.com.
 
11. Schakel in de Azure klassieke-portal, **Bevestig dat u hebt geconfigureerd eenmalige aanmelding zoals hierboven wordt beschreven**. Dit controleren, wordt het huidige certificaat aan de slag voor dit selectievakje toepassing inschakelen.
 
12. Klik op de pagina confirmation voor eenmalige aanmelding op **voltooid**.  


## <a name="creating-an-evidencecom-test-user"></a>Maken van een gebruiker Evidence.com testen

Voor Azure AD-gebruikers moeten kunnen aanmelden, moeten ze zijn geconfigureerd voor toegang binnen de toepassing Evidence.com. In deze sectie wordt beschreven hoe u Azure AD-gebruikersaccounts in Evidence.com maakt

**Voor het inrichten van een gebruikersaccount in Evidence.com:**

1.  In een webbrowservenster en meld u aan bij uw bedrijfssite Evidence.com als beheerder.

2.  Navigeer naar de tab **beheerder** .

3.  Klik op **gebruiker toevoegen**.

4.  Klik op de knop **toevoegen** .

5.  Het **E-mailadres** van de toegevoegde gebruiker moet overeenkomen met de gebruikersnaam van de gebruikers in Azure AD die u wilt toegang geven. Als de gebruikersnaam en e-mailadres niet dezelfde waarde in uw organisatie, kunt u de **Evidence.com > kenmerken > eenmalige aanmelding** gedeelte van de Azure klassieke portal de nameidenitifer naar Evidence.com verzonden om te worden van het e-mailadres wijzigen.


## <a name="assigning-users-to-evidencecom"></a>Het toewijzen van gebruikers aan Evidence.com

Voor ingerichte AAD-gebruikers moeten kunnen Evidence.com op hun Configuratiescherm Access, moeten ze toegang binnen de Azure klassieke portal worden toegewezen.

**Gebruikers toewijzen aan Evidence.com:**

1.  Klik op de pagina aan de slag voor Evidence.com in de portal van Azure klassieke op **gebruikers toewijzen aan Evidence.com**.
 
2.  Selecteer in het menu **weergeven** , of u wilt toewijzen aan een gebruiker of een groep Evidence.com en klik op de knop vinkje.
 
3.  Selecteer de gebruikers aan wie u wilt toewijzen Evidence.com groeperen in de lijst met **gebruikers** .
 
4.  Klik in de voettekst van de pagina, op de knop **toewijzen** .

