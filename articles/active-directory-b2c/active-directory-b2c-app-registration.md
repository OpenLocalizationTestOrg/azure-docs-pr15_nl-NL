<properties
    pageTitle="Azure Active Directory-B2C: Registratie toepassing | Microsoft Azure"
    description="Hoe u uw toepassing registreert met Azure Active Directory B2C"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory-B2C: Uw toepassing registreren

## <a name="prerequisite"></a>Vereiste

Als u wilt maken van een toepassing die consumenten aanmeldings- en aanmeldingsproblemen accepteert, moet u eerst de toepassing met een tenant Azure Active Directory B2C registreren. Uw eigen tenant krijgen via de stappen in [een tenant Azure AD B2C maken](active-directory-b2c-get-started.md). Nadat u de stappen in dit artikel hebt gevolgd, hebt u het B2C functies blad vastgemaakt aan uw Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Ga naar het blad B2C-functies

Als u het B2C functies blad vastgemaakt aan uw Startboard hebt, ziet u het blad zodra u zich aanmeldt bij de [Azure-portal](https://portal.azure.com/) als globale beheerder van de tenant B2C.

U kunt ook het blad door te klikken op **Bladeren** en klik vervolgens op **Azure AD B2C** in het linkernavigatiedeelvenster op de [Azure-portal](https://portal.azure.com/)openen.

> [AZURE.IMPORTANT] U moet een globale beheerder van de tenant B2C voor toegang tot het blad B2C-functies kunnen zijn. Een globale beheerder van een andere tenant of een gebruiker van een tenant geen toegang kunnen krijgen.  U kunt uw tenant B2C overstappen met behulp van de tenant-overschakeling in de rechterbovenhoek van de Azure-Portal.

## <a name="register-an-application"></a>Een toepassing registreren

1. Klik op het blad van de functies B2C op de Azure-portal op **toepassingen**.
2. Klik op **+ toevoegen** aan de bovenkant van het blad.
3. Voer een **naam** voor de toepassing die wordt uw toepassing op consumenten beschrijven. U kunt bijvoorbeeld "Contoso B2C app" invoeren.
4. Schakelen tussen de schakeloptie **opnemen in de browser / web API** op **Ja**als u een webtoepassing schrijft. Het **Antwoord URL's** zijn eindpunten waar Azure AD B2C alle tokens die uw toepassing aanvraagt zullen retourneren. Voer bijvoorbeeld `https://localhost:44321/`. Als uw webtoepassing, ook van enkele web API die zijn beveiligd met Azure AD B2C bellen wordt, wilt u een **Toepassing geheim** ook maken door te klikken op de knop **Sleutel genereren** .

    > [AZURE.NOTE] Een **Toepassing geheim** is een belangrijk beveiliging en correct moet worden beveiligd.

5. Schakelen tussen de schakeloptie **native client opnemen** op **Ja**als u een mobiele toepassing schrijft. Kopieer omlaag standaard **URI omleiden** die automatisch voor u is gemaakt.
6. Klik op **maken** om te registreren van uw toepassing.
7. Klik op de toepassing die u zojuist hebt gemaakt en kopieer omlaag de globaal unieke **ID van Client** die u later in uw code gebruikt.

> [AZURE.IMPORTANT] Toepassingen die zijn gemaakt in het blad van de functies B2C moeten beheerd op dezelfde locatie. Als u B2C bewerken toepassingen via PowerShell of een ander portal ze worden niet-ondersteunde en waarschijnlijk niet werkt met Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Maken van een toepassing voor snel aan de slag.

Nu dat u een toepassing geregistreerd bij Azure AD B2C hebt, kunt u een van onze zelfstudies werkbalk Snel starten aan de slag te voltooien. Hier volgen een paar aanbevelingen:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
