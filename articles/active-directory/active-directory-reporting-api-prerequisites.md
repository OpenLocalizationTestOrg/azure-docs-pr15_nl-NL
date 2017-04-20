<properties
    pageTitle="Vereisten voor toegang tot de Azure AD API rapportage. | Microsoft Azure"
    description="Meer informatie over de vereisten voor toegang tot de Azure AD rapportage-API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Vereisten voor toegang tot de Azure AD rapportage-API 

De [Azure AD rapportage-API's](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) bieden u toegang tot de gegevens via een reeks op basis van een REST API's. U kunt deze API's uit diverse talen en hulpprogramma's voor programming bellen.

De rapportage API gebruikt [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) toegang tot het web API's toe te staan. 

Als u wilt uw toegang tot de rapportage API voorbereiden, moet u het volgende doen:

1. Maken van een toepassing in uw Azure AD-tenant 

2. De juiste machtigingen van toepassing toegang tot de gegevens van Azure AD verlenen

3. Verzamel configuratie-instellingen van uw adreslijst

Voor vragen, problemen of feedback, neemt u contact [AAD rapportage Help](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Een Azure AD-toepassing maken

Als u wilt u de map voor toegang tot de Azure AD rapportage-API configureren, moet u zich aanmeldt bij de portal van Azure klassieke met een beheerdersaccount Azure-abonnement dat is ook een lid van de rol van globale beheerder directory in uw Azure AD-tenant.

> [AZURE.IMPORTANT] Toepassingen die worden uitgevoerd onder referenties met ' beheerdersbevoegdheden "strekking kunnen zijn zeer krachtig, dus zorg ervoor dat u van de toepassing-ID/geheim referenties beveiligen.


1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map in de lijst **active directory** .

3. In het menu aan de bovenkant, klikt u op **toepassingen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Klik op de onderste balk, op **toevoegen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Klik op de **Wat wilt u doen?** dialoogvenster, klikt u op **een toepassing het ontwikkelen van mijn organisatie toevoegen**. 

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/04.png) 


6. Klik in het dialoogvenster **Vertel ons over het gebruik van de toepassing** kunt u de volgende stappen uitvoeren: 

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/05.png) 

    een. Typ een naam in **het tekstvak** (bijvoorbeeld: rapportage-API van toepassing).

    b. Selecteer **webtoepassing en/of web API**.

    c. Klik op **volgende**.


7. Klik in het dialoogvenster **Eigenschappen van de App** kunt u de volgende stappen uitvoeren: 

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/06.png) 

    een. Typ in het tekstvak **URL-aanmelding** `https://localhost`.

    b. Typ in het tekstvak **App-ID-URI** ```https://localhost/ReportingApiApp```.

    c. Klik op **Voltooien**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Uw toepassing toestemming verlenen om te gebruiken van de API

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com/)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map in de lijst **active directory** .

3. In het menu aan de bovenkant, klikt u op **toepassingen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/02.png)


3. Selecteer uw nieuwe toepassing in de lijst met toepassingen.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/08.png)


5. In de sectie **machtigingen voor andere toepassingen** voor de resource **Azure Active Directory** , klikt u op de vervolgkeuzelijst van de **Machtigingen van toepassing** en selecteer vervolgens de **gegevens van de map lezen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/09.png)


5. Klik op de onderste balk, klikt u op **Opslaan**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Verzamel configuratie-instellingen van uw adreslijst

In dit gedeelte ziet u hoe u de volgende instellingen uit uw adreslijst:

- Domeinnaam
- Cliënt-ID
- Geheim client

Tijdens het configureren van oproepen naar de rapportage API moet u deze waarden. 


### <a name="get-your-domain-name"></a>De domeinnaam van uw verkrijgen

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map in de lijst **active directory** .

3. In het menu aan de bovenkant, klikt u op **domeinen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Kopieer uw domeinnaam in de kolom **Naam van het domein** .

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Client-ID van de toepassing ophalen

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map in de lijst **active directory** .

3. In het menu aan de bovenkant, klikt u op **toepassingen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Selecteer uw nieuwe toepassing in de lijst met toepassingen.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopieer uw **klant-ID**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Geheim van de client van de toepassing ophalen

Als u geheim van de client van de toepassing, moet u een nieuwe sleutel maken en opslaan van de waarde bij het opslaan van de nieuwe sleutel omdat het is niet mogelijk om op te halen deze waarde later meer.

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op het linker navigatiedeelvenster, klikt u op **Active Directory**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Selecteer de map in de lijst **active directory** .

3. In het menu aan de bovenkant, klikt u op **toepassingen**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Selecteer uw nieuwe toepassing in de lijst met toepassingen.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/07.png)

4. In het menu aan de bovenkant, klikt u op **configureren**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/08.png)

5. Klik in de sectie **sleutels** moet u de volgende stappen uitvoeren: 

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/14.png)

    een. Selecteer in de lijst duur de duur van een

    b. Klik op de onderste balk, klikt u op **Opslaan**.

    ![Register-toepassing](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Kopieer de sleutelwaarde.

## <a name="next-steps"></a>Volgende stappen

- Wilt u toegang tot de gegevens van de Azure AD API, met een programma en rapportage? Bekijk de [aan de slag met de API in de rapportage Azure Active Directory](active-directory-reporting-api-getting-started.md).

- Als u meer informatie wilt over het rapporteren van Azure Active Directory, raadpleegt u de [Azure Active Directory rapportage Guide](active-directory-reporting-guide.md).  
