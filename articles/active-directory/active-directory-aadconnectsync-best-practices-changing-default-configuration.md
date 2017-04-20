<properties
    pageTitle="Synchronisatie van Azure AD Connect: aanbevolen procedures voor het wijzigen van de standaardconfiguratie | Microsoft Azure"
    description="Bevat aanbevolen procedures voor het wijzigen van de standaardconfiguratie van Azure AD Connect synchroniseren."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Synchronisatie van Azure AD Connect: aanbevolen procedures voor het wijzigen van de standaardconfiguratie
Het doel van dit onderwerp wordt beschreven ondersteunde en niet-ondersteunde wijzigingen in Azure AD Connect synchroniseren.

De configuratie gemaakt door Azure AD Connect werkt "is" voor de meeste omgevingen die lokale Active Directory met Azure AD synchroniseren. In sommige gevallen is het echter nodig voor sommige wijzigingen toepassen op een configuratie om te voldoen aan een bepaald nodig of de vereiste.

## <a name="changes-to-the-service-account"></a>Wijzigingen in de serviceaccount
Azure AD Connect-synchronisatie wordt uitgevoerd met een serviceaccount gemaakt door de installatiewizard. Deze serviceaccount bevat de toetsen versleuteling met de database die wordt gebruikt door synchroniseren. Deze is gemaakt met een 127 tekens lang wachtwoord en het wachtwoord is ingesteld op niet verlopen.

- Het is **niet-ondersteunde** wijzigen of het wachtwoord van de serviceaccount opnieuw instellen. Hierdoor gaan verloren de toetsen versleuteling en de service is geen toegang tot de database en is niet kan worden gestart.

## <a name="changes-to-the-scheduler"></a>Wijzigingen in de planner
Beginnen met de versies van versie 1.1 (februari 2016) kunt u de [scheduler](active-directory-aadconnectsync-feature-scheduler.md) om een andere synchronisatie cyclus dan het standaardgeluid 30 minuten.

## <a name="changes-to-synchronization-rules"></a>Wijzigingen in de van synchronisatieregels
De installatiewizard biedt een configuratie die moet werken voor de meeste gevallen. In het geval u wijzigingen aanbrengen in de configuratie wilt, moet u deze regels om nog een ondersteunde configuratie volgen.

- U kunt [wijzigen van kenmerk loopt](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) als de standaard directe kenmerk loopt niet geschikt voor uw organisatie zijn.
- Als u [niet stroom een kenmerk wilt](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) en verwijder alle bestaande kenmerkwaarden in Azure AD, moet u een regel maken voor dit scenario.
- [Een ongewenste synchronisatie regel uitschakelen](#disable-an-unwanted-sync-rule) in plaats van deze te verwijderen. Een verwijderde regel wordt opnieuw gemaakt tijdens een upgrade.
- [Een kant-en-klare regel wijzigen](#change-an-out-of-box-rule), moet u maakt u een kopie van de oorspronkelijke regel en de regel kant-en-klare uitschakelen. De synchronisatie-regel-Editor wordt gevraagd en helpt u.
- Exporteer uw aangepaste synchronisatieregels met de synchronisatie regels-Editor. De editor biedt u een PowerShell-script die kunt u ze gemakkelijk opnieuw maken in het herstellen van fouten.

>[AZURE.WARNING] De synchronisatie van kant-en-klare regels hebben een vingerafdruk. Als u een wijziging in deze regels aanbrengt, is niet langer de vingerafdruk overeenkomende. U wellicht problemen in de toekomst wanneer u probeert een nieuwe versie van Azure AD Connect toepassen. Alleen Breng wijzigingen aan de manier die wordt beschreven in dit artikel.

### <a name="disable-an-unwanted-sync-rule"></a>Een ongewenste synchronisatie regel uitschakelen
Een regel voor het synchroniseren van kant-en-klare niet verwijderen. Dit is een opnieuw gemaakt tijdens de volgende upgrade.

In sommige gevallen heeft de installatiewizard een configuratie die niet voor uw topologie werkt geproduceerd. Bijvoorbeeld als u een account-resource bos topologie hebt, maar u het schema in de bos rekening met de Exchange-schema uitgebreid, worden klikt u vervolgens regels voor Exchange gemaakt voor het account en het forest resource. In dit geval moet u de synchronisatie-regel voor Exchange uitschakelen.

![Synchronisatie van uitgeschakelde regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

In de bovenstaande afbeelding heeft de installatiewizard een oude Exchange 2003-schema gevonden in de bos account. Dit toestel schema is toegevoegd voordat de resources bos is geïntroduceerd in van Fabrikam-omgeving. Om ervoor te zorgen dat er geen kenmerken van de oude implementatie van Exchange worden gesynchroniseerd, kan de synchronisatie-regel moet worden uitgeschakeld zoals wordt weergegeven.

### <a name="change-an-out-of-box-rule"></a>Een kant-en-klare regel wijzigen
Als u nodig hebt om een regel voor het kant-en-klare te wijzigen, moet u een kopie maken van de regel kant-en-klare en de oorspronkelijke regel uitschakelen. Breng de wijzigingen op de gekloonde regel. De synchronisatie regel Editor helpt u met deze stappen. Wanneer u een regel voor het kant-en-klare opent, wordt dit dialoogvenster weergegeven:  
![Waarschuwing af bij vak regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Selecteer **Ja** om een kopie van de regel te maken. De gekloonde regel wordt geopend.  
![Gekloonde regel](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Klik op deze regel gekloonde nodig wijzigingen aanbrengt bereik, bijwonen en transformaties.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
