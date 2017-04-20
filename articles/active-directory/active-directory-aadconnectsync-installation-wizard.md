<properties
    pageTitle="Synchronisatie van Azure AD Connect: met de installatiewizard een tweede maal | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe de installatiewizard werkt in de tweede keer dat u deze uitvoert."
    keywords="De installatiewizard Azure AD Connect kunt u de tweede keer dat u het uitvoeren van onderhoudsinstellingen configureren"
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Synchronisatie van Azure AD Connect: de installatiewizard een tweede keer uitvoeren
De eerste keer dat u de installatiewizard Azure AD Connect uitvoert begeleidt deze u bij het configureren van uw installatie. Als u de installatiewizard opnieuw uitvoert, biedt deze opties voor het onderhoud.

U kunt de installatiewizard vinden in het startmenu met de naam **Azure AD Connect**.

![Startmenu](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Wanneer u de installatiewizard te starten, ziet u een pagina met de volgende opties:

![Pagina met een lijst met aanvullende taken](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Als u ADFS met Azure AD Connect hebt geïnstalleerd, hebt u nog meer opties. De extra opties die u voor ADFS hebt worden beschreven in [ADFS-management](active-directory-aadconnect-federation-management.md#ad-fs-management).

Selecteer een van de taken en klik op **volgende** om door te gaan.

> [AZURE.IMPORTANT] Terwijl u de installatiewizard open hebt, worden alle bewerkingen in de synchronisatie-engine geschorst. Zorg ervoor dat u de installatiewizard sluit zodra u de van configuratiewijzigingen hebt aangebracht.

## <a name="view-current-configuration"></a>Huidige weergave-configuratie
Deze optie zorgt ervoor dat u een snelle weergave van uw geconfigureerde opties.

![Pagina met een lijst met alle opties en hun status](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Klik op **vorige** om terug te gaan. Als u **Afsluiten**selecteert, kunt u de installatiewizard sluiten.

## <a name="customize-synchronization-options"></a>Synchronisatieopties aanpassen
Deze optie wordt gebruikt om de configuratie van de synchronisatie te wijzigen. U ziet een subset van de opties van het installatiepad van aangepaste configuratie. U ziet deze optie, zelfs als u aangepaste installatie in eerste instantie hebt gebruikt.

- [Meer mappen toevoegen](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Voor het verwijderen van een map, Zie [verwijderen van een verbindingslijn](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Domein wijzigen en organisatie-eenheid filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Groep verwijderen filteren.
- [Optionele onderdelen wijzigen](active-directory-aadconnect-get-started-custom.md#optional-features).

De andere opties uit de eerste installatie kunnen niet worden gewijzigd en zijn niet beschikbaar. Deze opties zijn:

- Wijzig het kenmerk wil gebruiken voor userPrincipalName en sourceAnchor.
- Wijzig de gekoppelde methode voor objecten van verschillende bos.
- Filteren op basis van een groep inschakelen.

## <a name="refresh-directory-schema"></a>Directory-schema vernieuwen
Deze optie wordt gebruikt als u het schema hebt gewijzigd in een van uw on-premises AD DS forests. U mogelijk bijvoorbeeld hebt Exchange geïnstalleerd of bijgewerkt naar een schema Windows Server 2012 met apparaatobjecten. In dit geval moet u opdracht geven Azure AD Connect lezen van het schema opnieuw uit AD DS en bijwerken van de cache. Deze actie wordt ook de regels synchronisatie opnieuw gegenereerd. Als u het schema Exchange als voorbeeld toevoegt, wordt de synchronisatie van regels voor Exchange worden toegevoegd aan de configuratie.

Wanneer u deze optie selecteert, worden alle mappen in uw configuratie worden vermeld. U kunt u de standaardinstelling en alle forests vernieuwen of enkele van deze selectie opheffen.

![Pagina met een lijst met alle mappen in de omgeving](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Tijdelijk opslaan modus configureren
Deze optie kunt u inschakelen en klik op de server tijdelijk opslaan-modus uitschakelen. Meer informatie over het afspelen en hoe deze worden gebruikt tijdelijke vindt u in [bewerkingen](active-directory-aadconnectsync-operations.md#staging-mode).

De optie wordt weergegeven als tijdelijke momenteel is ingeschakeld of uitgeschakeld:  
![De optie ook voor de huidige status van het tijdelijk opslaan modus weergegeven wordt](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Als u wilt wijzigen van de staat, selecteer deze optie en selecteren of deselecteren het selectievakje.  
![De optie ook voor de huidige status van het tijdelijk opslaan modus weergegeven wordt](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Aanmelding wijzigen
Deze optie kunt u wijzigen van Wachtwoordsynchronisatie in Federatie of andersom. U wijzigen niet in **niet is geconfigureerd**.

Zie voor meer informatie over deze optie, [aanmelding](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het configuratiemodel dat is gebruikt door Azure AD Connect synchroniseren in de [Inrichting van declaratieve lidmaatschap](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
