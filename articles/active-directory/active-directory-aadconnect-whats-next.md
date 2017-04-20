<properties
    pageTitle="Azure AD Connect: De volgende stappen en het beheren van Azure AD Connect | Microsoft Azure"
    description="Leer hoe u de standaard-configuratie en operationele taken voor Azure AD Connect uitbreiden."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Volgende stappen en het beheren van Azure AD Connect
De volgende worden operationele onderwerpen waarmee u kunt het aanpassen van de Azure Active Directory verbinding maken om te voldoen aan de behoeften en de vereisten van uw organisatie ingesteld.  

## <a name="add-additional-sync-administrators"></a>Aanvullende synchronisatie-beheerders toevoegen
Standaard kunnen alleen de gebruiker die u de installatie- en lokale beheerders hebt voor het beheren van de geïnstalleerde synchronisatie-engine. Voor andere personen uit voor het kunnen openen en beheren van de synchronisatie-engine, zoek de groep ADSyncAdmins op de lokale server en deze toevoegen aan deze groep.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Licenties toewijzen aan gebruikers van Azure AD Premium en Enterprise-mobiliteit

Nu dat uw gebruikers zijn gesynchroniseerd met de cloud, moet u deze een licentie toewijzen zodat u ze kunnen aan de slag met de cloud-apps zoals Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Een Azure AD Premium of Enterprise mobiliteit Suite-licentie toewijzen
--------------------------------------------------------------------------------
1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, **Active Directory**.
3. Dubbelklik op de pagina Active Directory op de map met de gebruikers die u wilt inschakelen.
4. Selecteer boven aan de pagina directory, **licenties**.
5. Klik op de pagina licenties Selecteer Active Directory-Premium of Enterprise mobiliteit Suite en klik vervolgens op **toewijzen**.
6. In het dialoogvenster Selecteer de gebruikers die u wilt licenties toewijzen en klik vervolgens op het vinkje als de wijzigingen wilt opslaan.


## <a name="verifying-the-scheduled-synchronization-task"></a>De taak geplande synchronisatie verifiëren
Als u wilt controleren of de status van een synchronisatie kunt u dit doen door te schakelen in de portal van Azure.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Om te controleren of de taak geplande synchronisatie
--------------------------------------------------------------------------------
1. Meld u aan bij de Azure-Portal als beheerder.
2. Selecteer aan de linkerkant, **Active Directory**.
3. Dubbelklik op de pagina Active Directory op de map met de gebruikers die u wilt inschakelen.
4. Selecteer boven aan de pagina directory, **Directory-integratie**.
5. Klik onder integratie met lokale active directory-notitie de laatste keer dat synchroniseren.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Een taak geplande synchronisatie starten
Als voor het uitvoeren van een synchronisatietaak kunt u dit doen door nogmaals met de wizard Azure AD Connect uit te voeren.  U moet uw Azure AD-referenties op te geven.  Selecteer de taak **Synchronisatieopties aanpassen** in de wizard en op volgende tot en met de wizard. Aan het einde, moet u ervoor zorgen dat het selectievakje **het code synchronisatie starten zodra de eerste configuratie is voltooid** is ingeschakeld.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Voor meer informatie over het synchroniseren van Azure AD Connect: Scheduler, Zie [Azure AD verbinding Scheduler](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Aanvullende taken beschikbaar in Azure AD Connect
Na de installatie van Azure AD Connect, kunt u altijd de wizard opnieuw starten van de snelkoppeling Azure AD Connect start pagina of het bureaublad.  U ziet dat de wizard opnieuw doorlopen vindt u enkele nieuwe opties in de vorm van andere taken.  

De volgende tabel bevat een overzicht van de volgende taken kunnen uitvoeren en een korte beschrijving op elke.

![Deelnemen aan de regel](./media/active-directory-aadconnect-whats-next/addtasks.png)


Aanvullende taken | Beschrijving
------------- | ------------- |
Het geselecteerde scenario weergeven  |Kunt u uw huidige Azure AD Connect-oplossing weergeven.  Dit geldt ook voor algemene instellingen, gesynchroniseerde mappen, synchronisatie-instellingen, enzovoort.
Opties voor synchronisatie aanpassen | U kunt wijzigen van de huidige configuratie, inclusief het toevoegen van extra Active Directory-forests naar de configuratie of inschakelen Synchronisatieopties zoals door de gebruiker, groep, apparaat of wachtwoord terugschrijven.
Tijdelijke-modus inschakelen |  Hiermee kunt u fase informatie die later worden gesynchroniseerd, maar er niets Azure AD worden geëxporteerd of Active Directory.  Hiermee kunt u de synchronisatie van een voorbeeld bekijken voordat ze voorkomen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
