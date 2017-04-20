<properties
   pageTitle="Stapsgewijze instructies voor het gebruik van de Azure Active Directory B2B samenwerking preview | Microsoft Azure"
   description="Azure Active Directory-B2B samenwerking ondersteunt uw relaties intern doordat zakenpartners selectief toegang krijgen tot uw zakelijke toepassingen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Voorbeeld van de samenwerking Azure AD B2B: Stapsgewijze instructies

In dit scenario vindt u een overzicht van het gebruik van Azure AD B2B samenwerking. Als de IT-beheerder van Contoso wilt we toepassingen delen met werknemers van drie partnerbedrijven. Geen van de partnerbedrijven die moet Azure AD zijn.

- Lisa van eenvoudige Partner organigram
- Stefan, van gemiddeld Partner-organisatie, moet toegang tot een reeks apps
- Carol van complexe Partner-organisatie, moet toegang tot een reeks apps en lidmaatschap van groepen bij Contoso

Nadat uitnodigingen worden verzonden naar partner gebruikers, kunnen we ze in Azure AD om te verlenen van toegang tot de apps en lidmaatschap van groepen via de portal van Azure configureren. Laten we beginnen door toe te voegen Lisa.

## <a name="adding-alice-to-the-contoso-directory"></a>Lisa toevoegen aan de map Contoso
1. Maak een CSV-bestand met de koppen zoals u ziet, alleen Lisa van **E-mail**, **weergavenaam**en **InviteContactUsUrl**vullen. **Weergavenaam** is de naam die wordt weergegeven in de uitnodiging en ook de naam die wordt weergegeven in de directory van Contoso Azure AD. **InviteContactUsUrl** is een manier om Lisa Contoso contact op te nemen. In het volgende voorbeeld geeft InviteContactUsUrl het LinkedIn-profiel van Contoso. Het is belangrijk de spelling van de etiketten in de eerste rij van het CSV-bestand precies zoals opgegeven in de [verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md).  
![Voorbeeld van een CSV-bestand voor Lisa](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Klik in de portal Azure kunt u een gebruiker toevoegen in de directory van Contoso (Active Directory > Contoso > Gebruikers > gebruiker toevoegen). Selecteer in de 'Type van gebruiker' vervolgkeuzelijst "Gebruikers in partnerbedrijven". Upload het CSV-bestand. Zorg ervoor dat het CSV-bestand wordt gesloten voordat u uploadt.  
![CSV-bestand uploaden voor Lisa](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Lisa wordt nu weergegeven als een externe gebruiker in de directory van Contoso Azure AD.  
![Lisa wordt weergegeven in Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Lisa ontvangt over het volgende e-mailbericht.  
![E-mail met uitnodiging voor Lisa](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Lisa op de koppeling klikt en zij wordt gevraagd om de uitnodiging te accepteren en aan te melden met de referenties van haar werk. Als Lisa niet in de adreslijst Azure AD is, gevraagd Lisa te registreren.  
![Na de uitnodiging voor Lisa registreren](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Lisa omgeleid naar het deelvenster App Access, lege totdat ze toegang tot de apps is verleend.  
![Access-venster voor het Lisa](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Deze procedure kunt de eenvoudigste van B2B samenwerking. Als een gebruiker in de directory van Contoso Azure AD, kunt Lisa toegang krijgen tot toepassingen en -groepen via de portal van Azure. Nu we toevoegen Stefan, die toegang krijgen tot de Moodle en Salesforce-toepassingen moet.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Stefan toevoegen naar de map Contoso en het verlenen van toegang tot de apps
1. Windows PowerShell gebruiken met de Azure AD-Module geïnstalleerd als de toepassing-id's van Moodle en Salesforce wilt zoeken. De id's kunnen worden opgehaald met de cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` Hiermee opent u een lijst met alle beschikbare toepassingen in Contoso en hun AppPrincialIds.  
![Id's voor Stefan terughalen](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Maak een CSV-bestand met van Stefan e en weergavenaam, **InviteAppID**, **InviteAppResources**en InviteContactUsUrl. Vullen **InviteAppResources** met de AppPrincipalIds van Moodle en Salesforce gevonden via PowerShell, gescheiden door een spatie. **InviteAppId** met de dezelfde AppPrincipalId van Moodle naar uw huisstijl toepassen op het e-mailbericht en meld u aan de pagina's met een logo Moodle vullen.  
![Voorbeeld van een CSV-bestand voor Stefan](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Upload het CSV-bestand via de Portal Azure net zoals deze voor Lisa werden geëffend. Stefan is nu een externe gebruiker in de directory van Contoso Azure AD.

4. Stefan ontvangt over het volgende e-mailbericht.  
![E-mail met uitnodiging voor Stefan](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Stefan op de koppeling klikt, en wordt gevraagd om de uitnodiging te accepteren. Nadat hij is aangemeld, hij is doorgestuurd naar het deelvenster Access en al de beschikking over Moodle en Salesforce.  
![Access-venster voor het Stefan](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

We wordt toegevoegd Carol vervolgens die nodig toegang tot toepassingen en lidmaatschap van groepen in de directory van Contoso heeft.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Carol toevoegen aan de directory van Contoso en groepslidmaatschap geeft toegang verlenen tot de apps

1. Windows PowerShell gebruiken met de Azure AD-Module geïnstalleerd om de toepassings-id's en groeps-id's binnen Contoso vinden.
 - AppPrincipalId met cmdlet ophalen `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, hetzelfde geldt voor Stefan
 - Object-id ophalen voor groepen met cmdlet `Get-MsolGroup | fl DisplayName, ObjectId`. Hiermee opent u een lijst met alle groepen in Contoso en hun extensies. Groep-id's kunnen ook worden opgehaald als de Object-ID in het tabblad Eigenschappen van de groep in de portal van Azure.  
![Id's en groepen voor Carol terughalen](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. CSV-bestand, vullen van Carol E-mail, weergavenaam, InviteAppID, InviteAppResources, **InviteGroupResources**en InviteContactUsUrl maken. **InviteGroupResources** wordt gevuld met de extensies van de groepen MyGroup1 en externe sleutels, gescheiden door een spatie.  
![Voorbeeld van een CSV-bestand voor Carol](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Upload het CSV-bestand via de portal van Azure.

4. Carol is een gebruiker in de directory van Contoso en is ook een lid van de groepen MyGroup1 en externe sleutels, zoals gezien in de portal van Azure.  
![Carol wordt weergegeven in een groep in Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol ontvangt een e-mail met een koppeling om de uitnodiging te accepteren. Nadat ze zich aanmeldt, zij omgeleid naar het deelvenster van de Access-App toegang heeft tot Moodle en Salesforce.  

Dat is alles er is op het toevoegen van gebruikers van partner bedrijven Azure AD B2B samenwerken. In dit scenario blijkt hoe gebruikers Lisa, Stefan en Carol toevoegen aan de directory van Contoso met drie afzonderlijke .csv-bestanden. Dit proces kan gemakkelijker worden aangebracht door de afzonderlijke .csv-bestanden in een afzonderlijk bestand condenserend.  
![Voorbeeld van een CSV-bestand voor Lisa, Stefan en Carol](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Verwante artikelen
Onze andere artikelen op Azure AD B2B samenwerking bladeren:

- [Wat is Azure AD B2B samenwerking?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Werkwijze](active-directory-b2b-how-it-works.md)
- [Verwijzing naar een CSV-bestand opmaken](active-directory-b2b-references-csv-file-format.md)
- [Externe gebruiker token opmaken](active-directory-b2b-references-external-user-token-format.md)
- [Wijzigingen van de externe gebruiker object kenmerken](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Huidige preview beperkingen](active-directory-b2b-current-preview-limitations.md)
- [Artikel Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
