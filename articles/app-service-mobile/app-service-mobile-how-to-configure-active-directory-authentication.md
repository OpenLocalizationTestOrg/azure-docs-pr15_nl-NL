<properties
    pageTitle="Het configureren van Azure Active Directory-verificatie voor uw App Services-toepassing"
    description="Informatie over het configureren van Azure Active Directory-verificatie voor uw App Services-toepassing."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Het configureren van uw App-servicetoepassing Azure Active Directory-aanmelding gebruiken

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In dit onderwerp ziet u hoe u het configureren van Azure App Services Azure Active Directory gebruik als een verificatieprovider.

## <a name="express"> </a>Configureren Azure Active Directory met express-instellingen

13. Ga in uw toepassing in de [portal van Azure]. Klik op **Instellingen**en klik op **Verificatie**.

14. Als de verificatie / autorisatie functie niet is ingeschakeld, schakelt u het overschakelen naar **op**.

15. **Azure Active Directory**op en klik vervolgens op **Express** onder **Management modus**.

16. Klik op **OK** om te registreren van de toepassing in Azure Active Directory. Hiermee maakt u een nieuwe registratie. Als u een bestaande registratie in plaats daarvan kiezen wilt, klikt u op **een bestaande app selecteren** en zoek vervolgens naar de naam van de registratie van een eerder gemaakte binnen uw tenant.
Klik op de registratie te selecteren en klik op **OK**. Klik vervolgens op **OK** in het blad Azure Active Directory-instellingen.

    ![][0]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

17. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen de gebruikers die zijn geverifieerd door Azure Active Directory, stelt u **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** op **aanmelden met de Azure Active Directory**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.

17. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw app.

## <a name="advanced"> </a>(Alternatieve methode) Azure Active Directory handmatig configureren met geavanceerde instellingen
U kunt er ook voor kiezen om te leveren configuratie-instellingen handmatig. Dit is de beste oplossing als u de gewenste AAD-tenant verschilt van de tenant waarmee u zich bij Azure aanmelden. U voltooit de configuratie, moet u eerst een registratie maken in Azure Active Directory en moet u opgeven enkele van de registratiedetails naar App-Service.

### <a name="register"> </a>Uw toepassing registreren met Azure Active Directory

1. Meld u aan bij de [Azure-portal]en Ga in uw toepassing. Kopieer de **URL**. U kunt dit wilt gebruiken voor het configureren van de Azure Active Directory-app.

3. Meld u aan bij de [portal van Azure klassieke] en Ga naar **Active Directory**.

    ![][2]

4. Selecteer de map en selecteer vervolgens het tabblad **toepassingen** aan de bovenkant. Klik op **toevoegen** aan het scherm om een nieuwe app-registratie maken.

5. Klik op **een toepassing het ontwikkelen van mijn organisatie toevoegen**.

6. In de Wizard van de toepassing toevoegen een **naam** voor uw toepassing en klik op het **Web Application en/of Web API** -type. Klik vervolgens op als u wilt doorgaan.

7. Plak de URL van de toepassing die u eerder hebt gekopieerd in het vak **AANMELDINGSADRES-op-URL** . Voer die dezelfde URL in het vak **App-ID-URI** . Klik vervolgens op als u wilt doorgaan.

8. Wanneer u de toepassing hebt toegevoegd, klikt u op het tabblad **configureren** . Bewerk de **URL van het antwoord** onder **eenmalige aanmelding** moeten de URL van uw toepassing toegevoegd door het pad, _/.auth/login/aad/callback_. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Zorg ervoor dat u het schema HTTPS worden gebruikt.

    ![][3]

9. Klik op **Opslaan**. Kopieer de **Client-ID** voor de app. U stelt uw toepassing dit later gebruik.

10. In de opdrachtenbalk onder op **View eindpunten**, en kopieer de URL **Federatie metagegevens Document** en dat document downloaden of Ga naar deze in een browser.

11. In het hoofdelement **EntityDescriptor** , moet er een **id van de entiteit** -kenmerk van het formulier `https://sts.windows.net/` gevolgd door een specifieke GUID aan bij uw tenant (ook wel een "tenant-ID" genoemd). Kopieer deze waarde: deze fungeert als de **URL van de uitgever**. U stelt uw toepassing dit later gebruik.

### <a name="secrets"> </a>Toevoegen Azure Active Directory-informatie in uw toepassing

13. Ga in uw toepassing terug in de [portal van Azure]. Klik op **Instellingen**en klik op **Verificatie**.

14. Als de verificatie-functie is niet ingeschakeld, schakelt u het overschakelen naar **op**.

15. Klik op **Azure Active Directory**en klik vervolgens op **Geavanceerd** onder **Management modus**. Plak de waarde in Client-ID en de URL van de uitgever die u eerder hebt verkregen. Klik vervolgens op **OK**.

    ![][1]

    Standaard is App Service biedt verificatie, maar blokkeert niet de gemachtigde toegang tot uw site-inhoud en API's. U moet gebruikers toestaan in uw app-code.

17. (Optioneel) Als u wilt beperken van toegang aan uw site om alleen de gebruikers die zijn geverifieerd door Azure Active Directory, stelt u **actie moet uitvoeren wanneer aan aanvraag niet is geverifieerd** op **aanmelden met de Azure Active Directory**. Hiervoor is vereist dat alle aanvragen worden geverifieerd, en alle niet-geverifieerde aanvragen worden omgeleid naar Azure Active Directory voor verificatie.

17. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Azure Active Directory voor verificatie in uw app.

## <a name="optional-configure-a-native-client-application"></a>(Optioneel) Een native client configureren

Azure Active Directory kunt u ook systeemeigen clients, registreren waarmee betere controle over machtigingen toewijzen. U moet dit als u wilt uitvoeren aanmeldingen met behulp van een bibliotheek, zoals de **Bibliotheek van Active Directory-verificatie**.

1. Navigeer naar **Active Directory** in de [klassieke Azure-portal].

2. Selecteer de map en selecteer vervolgens het tabblad **toepassingen** aan de bovenkant. Klik op **toevoegen** aan het scherm om een nieuwe app-registratie maken.

3. Klik op **een toepassing het ontwikkelen van mijn organisatie toevoegen**.

4. In de Wizard van de toepassing toevoegen een **naam** voor uw toepassing en klik op het type **Systeemeigen clienttoepassing** . Klik vervolgens op als u wilt doorgaan.

5. Voer in het vak **Omleiden URI** van uw site _/.auth/login/done_ eindpunt, met het schema HTTPS. Deze waarde moet er ongeveer als _https://contoso.azurewebsites.net/.auth/login/done_. Als u een Windows-toepassing maakt, kunt u in plaats daarvan het [pakket beveiligings-id](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als de URI gebruiken.

6. Nadat de bijbehorende toepassing zijn toegevoegd, klikt u op het tabblad **configureren** . Zoek de **Client-ID** en noteer deze waarde.

7. Blader omlaag naar de sectie **machtigingen voor andere toepassingen** en klik op **toevoegen-toepassing**.

8. Zoeken naar de webtoepassing die u eerder hebt geregistreerd en klik op het pluspictogram. Klik vervolgens op het selectievakje om het dialoogvenster te sluiten. Als de webtoepassing niet kan worden gevonden, gaat u naar de registratie en een nieuwe antwoord-URL (bijvoorbeeld de HTTP-versie van de URL van uw huidige) toevoegen, klikt u op Opslaan en herhaalt u deze stappen: de toepassing moet worden weergegeven in de lijst.

9. Op de nieuwe vermelding die u zojuist hebt toegevoegd, opent u de **Machtigingen gedelegeerde** vervolgkeuzelijst en selecteer **Access (toepassingsnaam)**. Klik vervolgens op **Opslaan**.

U hebt nu een systeemeigen clienttoepassing die toegang uw App-servicetoepassing tot hebben geconfigureerd.

## <a name="related-content"> </a>Verwante inhoud

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-portal]: https://portal.azure.com/
[Azure klassieke portal]: https://manage.windowsazure.com/
[alternative method]:#advanced
