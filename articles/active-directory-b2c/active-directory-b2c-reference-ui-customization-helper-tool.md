<properties
    pageTitle="Azure Active Directory-B2C: Page UI aanpassing helper hulpmiddel | Microsoft Azure"
    description="Een hulpprogramma voor helper om te laten zien van de functie pagina UI aanpassen in Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory-B2C: Een helper hulpmiddel gebruikt om te laten zien van de functie pagina gebruiker gebruikersinterface (UI) aanpassen

In dit artikel is een aanvulling op de [gebruikersinterface aanpassing hoofdartikel](active-directory-b2c-reference-ui-customization.md) in B2C Azure Active Directory (Azure AD). De volgende stappen wordt beschreven hoe bij het gebruikmaken van de pagina-functie voor het aanpassen van UI met behulp van de steekproef HTML en CSS inhoud die we hebt opgegeven.

## <a name="get-an-azure-ad-b2c-tenant"></a>Een B2C van Azure AD-tenant ophalen

Voordat u aanpassen kunt, moet u om [een B2C van Azure AD-tenant](active-directory-b2c-get-started.md) als u er nog geen hebt.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Inschrijfformulier of aanmeldingsproblemen-beleid maken

De inhoud van de steekproef we hebt opgegeven kan worden gebruikt om customze twee pagina's in een [Inschrijfformulier of aanmeldingsproblemen beleid](active-directory-b2c-reference-policies.md): de [collectieve aanmeldingspagina](active-directory-b2c-reference-ui-customization.md) en de [selfservice krachtens kenmerken pagina](active-directory-b2c-reference-ui-customization.md). Wanneer [uw inschrijfformulier of aanmeldingsproblemen-beleid maken](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), lokale Account (e-mailadres), Facebook, Google en Microsoft toevoegen als **identiteitsprovider**. Dit zijn de enige IDPs die ons voorbeeld HTML-inhoud worden geaccepteerd.  Als u wilt, kunt u ook een subset van deze IDPs toevoegen.

## <a name="register-an-application"></a>Een toepassing registreren

U moet [een toepassing registreren](active-directory-b2c-app-registration.md) in uw tenant B2C die kan worden gebruikt om uw beleid uitvoeren. Na het registreren van uw toepassing, hebt u een paar opties waarmee u kunt uw aanmelding beleid uitvoeren:

- Maken van een van de Azure AD B2C toepassingen van de werkbalk Snel starten in de sectie 'Aan de slag' van weergegeven [Meld u aan omhoog en meld u aan consumenten in uw toepassingen](active-directory-b2c-overview.md#getting-started).
- Gebruik de vooraf gedefinieerde [Azure AD B2C Speelplaats](https://aadb2cplayground.azurewebsites.net) -toepassing. Als u besluit om de Speelplaats gebruiken, moet u een toepassing registreren in uw B2C-tenant met behulp van de **URI omleiden** `https://aadb2cplayground.azurewebsites.net/`.
- Gebruik de knop **Nu uitvoeren** op uw beleid in de [portal van Azure](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Uw beleid aanpassen

Als u wilt aanpassen van het uiterlijk van uw beleid, moet u eerst HTML en CSS-bestanden met de specifieke conventies van Azure AD B2C maken. U kunt uw statische inhoud vervolgens uploaden naar een openbaar locatie zodat Azure AD B2C deze kunnen openen. Dit kan zijn op uw eigen speciale webserver, Azure-blobopslag, Azure inhoud bezorging netwerk of een andere statische resource-hostingprovider. De enige vereisten zijn dat de inhoud beschikbaar via HTTPS is en kan worden geopend met behulp van CORS. Nadat u uw statische inhoud op het web hebt weergegeven, kunt u uw beleid om te verwijzen naar deze locatie en die inhoud presenteren aan uw klanten kunt bewerken. De [gebruikersinterface aanpassing hoofdartikel](active-directory-b2c-reference-ui-customization.md) wordt beschreven in detail de werking van de functie voor het aanpassen van Azure AD B2C.

Voor de toepassing van deze zelfstudie wordt hebt er al gemaakt van bepaalde inhoud van de steekproef en deze die worden gehost op Azure-blobopslag. De inhoud van de steekproef is een zeer eenvoudige aanpassing in het thema van onze fictief bedrijf, "Speel". Als u wilt uitproberen in uw eigen beleid, als volgt te werk:

1. Meld u aan bij uw tenant op de [Azure-portal](https://portal.azure.com/) en Ga naar het blad B2C-functies.
2. **Beleid voor het registreren of aanmelden** op en klik op het beleid (bijvoorbeeld "b2c\_1\_Aanmeldingsadres\_omhoog\_Aanmeldingsadres\_in").
3. Op **pagina UI-aanpassing** en klik vervolgens op **geïntegreerde inschrijfformulier of aanmeldingsproblemen pagina**.
4. Schakelen tussen de schakeloptie **Gebruik aangepaste pagina** op **Ja**. Voer in het veld **aangepaste pagina URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klik op **OK**.
5. Klik op **de aanmeldingspagina lokale account**. Schakelen tussen de schakeloptie **aangepaste sjabloon gebruiken** op **Ja**. Voer in het veld **aangepaste pagina URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Herhaal dezelfde stap voor de **aanmeldingspagina van sociale-account**.
 Klik op **OK** tweemaal om te sluiten van de bladen van de aanpassing UI.
6. Klik op **Opslaan**.

Nu kunt u uw aangepaste beleid uitproberen. U kunt uw eigen toepassing of de Speelplaats Azure AD B2C gebruiken als u wilt, maar u kunt ook klikken op de opdracht **Nu uitvoeren** in het blad beleid. Selecteer uw toepassing in de vervolgkeuzelijst en kies de juiste omleiding URI. Klik op de knop **nu uitvoeren** . Een nieuw browsertabblad wordt geopend en u kunt uitvoeren via de gebruikerservaring van zich registreert voor uw toepassing met de nieuwe inhoud op hun plaats staan.

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>De inhoud van de steekproef uploaden naar Azure-blobopslag

Als u Azure-blobopslag gebruiken wilt voor het hosten van uw pagina-inhoud, kunt u uw eigen opslag-account maken en gebruiken van onze B2C helper hulpmiddel om uw bestanden te uploaden.

### <a name="create-a-storage-account"></a>Een opslagruimte-account maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ Nieuw** > **gegevens + opslagruimte** > **opslag-account**. Moet u een Azure-abonnement om een Azure-blobopslag-account te maken. U kunt een gratis proefversie op de [website van Azure](https://azure.microsoft.com/pricing/free-trial/)aanmelden.
3. Geef een **naam** voor de opslag-account (bijvoorbeeld "contoso") en kies de gewenste selecties voor **prijzen laag**, **resourcegroep** en **abonnement**. Zorg ervoor dat u de optie **vastmaken aan Startboard** is ingeschakeld hebt. Klik op **maken**.
4. Ga terug naar de Startboard en klik op de opslagruimte-account dat u zojuist hebt gemaakt.
5. Klik in de sectie **Overzicht** **Containers**op en klik vervolgens op **+ toevoegen**.
6. Geef een **naam** voor de container (bijvoorbeeld "b2c") en **Blob** selecteert als het **type toegang**. Klik op **OK**.
7. De container die u hebt gemaakt, wordt weergegeven in de lijst op het blad **BLOB's** . Noteer de URL van de container; bijvoorbeeld, moet er ongeveer `https://contoso.blob.core.windows.net/b2c`. Sluit het blad **BLOB's** .
8. Klik op het blad opslag-account op **toetsen** en noteer de waarden van de velden **Opslagaccountnaam** en **Primaire sleutel van Access** .

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ Nieuw** > **gegevens + opslagruimte** > **opslag-account**. Moet u een Azure-abonnement om een Azure-blobopslag-account te maken. U kunt een gratis proefversie op de [website van Azure](https://azure.microsoft.com/pricing/free-trial/)aanmelden.
3. Selecteer **Blob Storage** onder **Type Account**en laat de andere waarden als standaard.  U kunt de resourcegroep & locatie als u wilt bewerken.  Klik op **maken**.
4. Ga terug naar de Startboard en klik op de opslag-account dat u zojuist hebt gemaakt.
5. Klik op **+ Container**in de sectie **Overzicht** .
6. Geef een **naam** voor de container (bijvoorbeeld "b2c") en **Blob** selecteert als het **type toegang**. Klik op **OK**.
7. Open de container- **Eigenschappen**en noteer de URL van de container; bijvoorbeeld, moet er ongeveer `https://contoso.blob.core.windows.net/b2c`. Sluit het blad container.
8. Klik op het **Pictogram van de toets** op het blad opslagruimte-account en noteer de waarden van de velden **Opslagaccountnaam** en **Primaire sleutel van Access** .

> [AZURE.NOTE]
    **Primaire sleutel van Access** is een belangrijk beveiliging.

### <a name="download-the-helper-tool-and-sample-files"></a>De helper hulpmiddel en het voorbeeld bestanden downloaden

U kunt downloaden van de [Azure-blobopslag helper hulpmiddel en het voorbeeld-bestanden als een ZIP-bestand](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) of deze uit GitHub klonen:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Deze bibliotheek bevat een `sample_templates\wingtip` directory, waarin voorbeeld HTML, CSS en afbeeldingen. Voor deze sjablonen om te verwijzen naar uw eigen Azure-blobopslag-account, moet u de HTML-bestanden bewerken. Open `unified.html` en `selfasserted.html` en vervangen van alle exemplaren van `https://localhost` met de URL van uw eigen container die u in de vorige stappen hebt genoteerd. U moet het volledige pad van de HTML-bestanden gebruiken omdat in dit geval de HTML-code moet worden verzonden door Azure AD, onder het domein `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Upload de bestanden van de steekproef

In de dezelfde opslagplaats pak `B2CAzureStorageClient.zip` en voer de `B2CAzureStorageClient.exe` bestand in. Dit programma wordt gewoon upload de bestanden in de map die u bij uw account voor opslagruimte en CORS toegang inschakelen voor deze bestanden. Als u de bovenstaande stappen hebt gevolgd, worden nu de HTML en CSS-bestanden verwijzen naar uw account opslag. Houd er rekening mee dat de naam van uw account opslag is het gedeelte die voorafgaat aan `blob.core.windows.net`; bijvoorbeeld `contoso`. U kunt controleren of dat de inhoud juist is geüpload door bij het openen van `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in een browser. [Http://test-cors.org/](http://test-cors.org/) ook gebruiken om ervoor te zorgen dat de inhoud nu CORS ingeschakeld is. (Zoekt u ' XHR status: 200 ' in het resultaat.)

### <a name="customize-your-policy-again"></a>Het beleid, opnieuw aanpassen

Nu dat u de inhoud van de steekproef hebt geüpload naar uw eigen opslag-account, moet u uw aanmelding bewerken ernaar verwijst. Herhaal de stappen in de sectie ["Aanpassen uw beleid"](#customize-your-policy) hierboven, met behulp van URL's van uw eigen opslag-account. Bijvoorbeeld, de locatie van uw `unified.html` bestand zou `<url-of-your-container>/wingtip/unified.html`.

Nu kunt u de knop **Nu uitvoeren** of uw eigen applicatie uw beleid opnieuw uitvoeren. Het resultaat moet er bijna helemaal hetzelfde--u gebruikt hetzelfde voorbeeld van HTML en CSS in beide gevallen. Echter uw beleid worden nu verwijst naar uw eigen exemplaar van Azure-blobopslag en werkt u gratis om te bewerken en upload de bestanden opnieuw als u neemt. Raadpleeg het [hoofdartikel in UI aanpassen](active-directory-b2c-reference-ui-customization.md)voor meer informatie over het aanpassen van de HTML en CSS.
