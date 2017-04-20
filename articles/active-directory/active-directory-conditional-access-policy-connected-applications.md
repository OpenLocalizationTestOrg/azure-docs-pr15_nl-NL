<properties
    pageTitle="Voorwaardelijke-apparaat gebaseerde beleid voor Azure Active Directory-toepassingen instellen | Microsoft Azure"
    description="Stel voorwaardelijke-apparaat gebaseerde beleid voor Azure AD-toepassingen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="markvi"/>


# <a name="set-device-based-conditional-access-policy-for-azure-active-directory-connected-applications"></a>Voorwaardelijke-apparaat gebaseerde beleid voor Azure Active Directory-toepassingen instellen


Azure Active Directory (Azure AD) op basis van het apparaat voorwaardelijke access kunt u beveiligen met een organisatie resources uit:

- Er wordt geprobeerd van onbekende of niet-beheerde apparaten.
- Apparaten die niet voldoet aan de beveiligingsbeleid voor apparaten van uw organisatie.

Zie voor een overzicht van voorwaardelijke toegang, [voorwaardelijke Azure Active Directory-toegang](active-directory-conditional-access.md).

Voorwaardelijke-apparaat gebaseerde beleid voor het beveiligen van deze toepassingen, kunt u instellen:

- Office 365 SharePoint Online, om te beveiligen van uw organisatie sites en documenten
- Office 365 Exchange Online, om van uw organisatie e-mail te beschermen
- Software als een service (SaaS)-toepassingen die verbonden zijn met Azure AD voor verificatie
- On-premises implementatie-toepassingen die zijn gepubliceerd via Azure AD-toepassingsproxy services

Als u wilt een voorwaardelijke-apparaat gebaseerde beleid, in de portal van Azure instellen, gaat u naar de specifieke toepassing in de adreslijst.


  ![Lijst met toepassingen in de Azure portal adreslijst] (./media/active-directory-conditional-access-policy-connected-applications/01.png "Toepassingen")


Selecteer de toepassing en klik vervolgens op het tabblad **configureren** om in te stellen van het voorwaardelijke-beleid.  


  ![De toepassing configureren] (./media/active-directory-conditional-access-policy-connected-applications/02.png "Apparaat op basis van regels voor access")




Voor het instellen van een voorwaardelijke-apparaat gebaseerde beleid, in de sectie **apparaat op basis van toegangsregels** in **Toegangsregels inschakelen**, selecteert u **op**.

Een voorwaardelijke-apparaat gebaseerde beleid heeft drie componenten:

- **Toepassen op**. Het bereik van het beleid is van toepassing op gebruikers.

- **Regels voor apparaat**. De voorwaarden die een apparaat moet voldoen aan voordat deze toegang de toepassing tot.

- **Toepassing afdwingen**. De clienttoepassingen (native versus browser) het beleid is van toepassing op.

  ![De drie onderdelen van een apparaat gebaseerde-beleid] (./media/active-directory-conditional-access-policy-connected-applications/03.png "Apparaat op basis van regels voor access")


## <a name="select-the-users-the-policy-applies-to"></a>Selecteer de gebruikers van die het beleid is van toepassing op

U kunt het bereik van dit beleid geldt voor gebruikers selecteren in de sectie **Toepassen op** .

Bij het maken van een bereik van het beleid toegang voor gebruikers hebt u twee opties:

- **Alle gebruikers**. Het beleid van toepassing op alle gebruikers die toegang hebben tot de toepassing.

- **Groepen**. Hiermee kunt u het beleid voor gebruikers die lid zijn van een specifieke groep beperken.

![Beleid toepassen voor alle gebruikers of aan een groep] (./media/active-directory-conditional-access-policy-connected-applications/11.png "Toepassen op")


 Schakel het selectievakje **, behalve** als u een gebruiker uit een beleid wilt. Dit is handig wanneer u moet de machtigingen te geven aan een specifieke gebruiker tijdelijk toegang tot de toepassing. Deze optie selecteert, bijvoorbeeld als sommige gebruikers apparaten die niet gereed voor voorwaardelijke toegang zijn hebt. De apparaten kunnen niet worden geregistreerd of ze zijn worden geen licentie.


## <a name="select-the-conditions-that-devices-must-meet"></a>Selecteer de voorwaarden waaraan apparaten moeten voldoen

**Regels voor apparaat** gebruiken voor het instellen van de voorwaarden die een apparaat moet voldoen om te krijgen tot de toepassing.

U kunt voorwaardelijke toegang op basis van het apparaat instellen voor dit soort apparaat:

- Jubileum-Update voor Windows 10, Windows 8.1 en Windows 7
- Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 en Windows Server 2008 R2
- apparaten met iOS (iPad, iPhone)
- Android-apparaten

Ondersteuning voor Mac is binnenkort beschikbaar.

  ![Beleid toepassen naar apparaten] (./media/active-directory-conditional-access-policy-connected-applications/04.png "Toepassingen")

 >[AZURE.NOTE] Zie voor informatie over de verschillen tussen het domein zijn toegevoegd en Azure AD-die zijn gekoppeld apparaten, [met behulp van Windows 10-apparaten in uw bedrijf](active-directory-azureadjoin-windows10-devices.md).

U hebt twee opties voor apparaat regels:

- **Alle apparaten moet voldoen**. Alle platforms die toegang de toepassing tot moet voldoen. Apparaten die worden uitgevoerd op de platforms die geen ondersteuning voorwaardelijke toegang op basis van het apparaat bieden geen toegang.

- **Alleen geselecteerde apparaten moet voldoen**. Alleen bepaalde apparaat platforms moet voldoen. Andere platforms of andere platforms die toegang heeft tot de toepassing, moet u toegang hebt.

  ![Het bereik voor regels die apparaat instellen] (./media/active-directory-conditional-access-policy-connected-applications/05.png "Toepassingen")

Azure AD-die zijn gekoppeld apparaten zijn compatibel als ze zijn gemarkeerd als **compatibele** in de adreslijst door Intune of door een derde partij mobiele apparaat management-systeem dat wordt geïntegreerd met Azure AD.

Een domein behoren apparaat is compatibel met als:

- Dit is geregistreerd bij Azure AD. Veel organisaties behandelen domein behoren apparaten vertrouwde apparaten.
- Het is gemarkeerd als **compatibele** in Azure AD System Center Configuration Manager-2016.

 ![Een domein behoren apparaten die compatibel zijn] (./media/active-directory-conditional-access-policy-connected-applications/06.png "Regels voor apparaat")

Windows persoonlijke apparaten zijn compatibel als ze zijn gemarkeerd als **compatibele** in de adreslijst door Intune of door een derde partij mobiele apparaat management-systeem dat wordt geïntegreerd met Azure AD.

Niet-Windows-apparaten zijn compatibel als ze door Intune als **compatibele** in de adreslijst zijn gemarkeerd.

 >[AZURE.NOTE] Zie voor meer informatie over het instellen van Azure AD voor apparaat naleving in andere systemen, [voorwaardelijke Azure Active Directory-toegang](active-directory-conditional-access.md).


U kunt een of meerdere apparaat platforms voor een beleid op basis van een apparaat selecteren. Dit geldt ook voor Android, iOS Windows Mobile (Windows 8.1 telefoons en tablets) en Windows (alle andere Windows apparaten, inclusief alle Windows 10-apparaten).
Evaluatie van beleid doet zich alleen op de geselecteerde platforms. Als u een apparaat dat access probeert niet actief is een van de geselecteerde platforms, kan het apparaat dat de toepassing kan openen als de gebruiker toegang heeft. Niet apparaatbeleid wordt geëvalueerd.

![Selecteer platforms voor apparaat regels] (./media/active-directory-conditional-access-policy-connected-applications/07.png "Regels voor apparaat")


## <a name="set-policy-evaluation-for-a-type-of-application"></a>Evaluatie van beleid instellen voor een type van toepassing

Stel het type van het beleid wordt geëvalueerd voor de gebruiker of apparaat access-toepassingen in de sectie **Toepassing afdwingen** .

U hebt twee opties voor het type toepassing om op te nemen:

- Browser en systeemeigen toepassingen
- Alleen systeemeigen toepassingen

![Kies browser of systeemeigen toepassingen] (./media/active-directory-conditional-access-policy-connected-applications/08.png "Toepassingen")

Selecteren om af te dwingen-beleid voor toepassingen, **voor de browser en systeemeigen toepassingen**. Vervolgens kunt u opnemen:

- Browsers (bijvoorbeeld Microsoft Edge in Windows 10) of Safari in iOS.
- Toepassingen die de Active Directory verificatie bibliotheek (ADAL) op een willekeurig platform gebruikt of dat de WebAccountManager (WAM) API in Windows 10.

>[AZURE.NOTE] Zie voor meer informatie over browserondersteuning en andere relevante informatie voor een gebruiker aan wie toegang heeft tot een apparaat gebaseerde, certificaat-toepassing instantie is beveiligd, [voorwaardelijke Azure Active Directory-toegang](active-directory-conditional-access.md).

## <a name="help-protect-email-access-from-exchange-activesync-based-applications"></a>E-mailbericht access beschermd tegen Exchange ActiveSync-toepassingen

In Office 365 Exchange Online-toepassingen, kunt u Exchange ActiveSync e-mail toegang tot de Exchange ActiveSync-e-mailtoepassingen blokkeren.

![Opties voor Exchange ActiveSync-naleving] (./media/active-directory-conditional-access-policy-connected-applications/09.png "Toepassingen")

![Een compatibele apparaat voor toegang tot e-mail vereisen] (./media/active-directory-conditional-access-policy-connected-applications/10.png "Toepassingen")


## <a name="next-steps"></a>Volgende stappen

- [Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access.md)
