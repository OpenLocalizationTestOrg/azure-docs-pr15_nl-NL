<properties
    pageTitle="Synchronisatie van Azure AD Connect: synchronisatie servicebeheer UI | Microsoft Azure"
    description="Het tabblad bewerkingen in het servicebeheer van synchronisatie voor Azure AD Connect te begrijpen."
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
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Synchronisatie van Azure AD Connect: synchronisatie servicebeheer

[Bewerkingen](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Verbindingslijnen](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Het tabblad bewerkingen geeft de resultaten van de meest recente bewerkingen. Dit tabblad is heel belangrijk om te begrijpen en problemen oplossen.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Meer informatie over de informatie die zichtbaar zijn in het tabblad bewerkingen
Het bovenste gedeelte ziet u alle wordt uitgevoerd in chronische volgorde. Standaard is het logboek bewerkingen blijft informatie over de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](active-directory-aadconnectsync-feature-scheduler.md). U wilt bekijken voor een uitvoeren die de status van een success niet wordt weergegeven. U kunt de volgorde door te klikken op de koppen wijzigen.

De kolom **Status** is van de belangrijkste informatie en ziet u het meest ernstige probleem voor een uitvoeren. Hier volgt een beknopt overzicht van de meest voorkomende statussen in volgorde van prioriteit om te kunnen onderzoeken (waar * geven verschillende mogelijke foutberichten).

Status | Opmerking
--- | ---
gestopt-* | De uitvoeren kan niet voltooien. Als bijvoorbeeld het externe systeem is uitgeschakeld en is niet bereikbaar.
gestopt-fout-limiet | Er zijn meer dan 5000 fouten. Het vakje is automatisch vanwege een groot aantal fouten gestopt.
voltooide -\*-fouten | De klaar is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht.
voltooide -\*-waarschuwingen | De uitvoeren die is voltooid, maar sommige gegevens is niet de verwachte status. Als er fouten zijn, klikt u vervolgens is dit bericht gewoonlijk alleen een symptoom. Totdat u fouten hebt verwerkt, moet u niet waarschuwingen onderzoek.
succes | Geen problemen.

Wanneer u een rij selecteert, wordt er onder bijgewerkt om weer te geven van de details van die worden uitgevoerd. Aan de linkerkant van de onderkant, als u wellicht een lijst **stap #**zeggen. Deze lijst wordt alleen weergegeven als u meerdere domeinen hebt in uw bos waarbij elk domein wordt aangeduid met een stap. Klik onder de kop **Partition**vindt u de domeinnaam. Onder **Synchronisatie statistieken**vindt u meer informatie over het aantal wijzigingen die zijn verwerkt. U kunt klikken op de koppelingen voor een overzicht van de gewijzigde objecten. Als u objecten met fouten, weergegeven die fouten onder **Synchronisatiefouten**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Fouten corrigeren in tabblad bewerkingen
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Als er fouten zijn, zijn zowel het object in de fout en de fout zelf koppelingen die vindt u meer informatie.

Begin door te klikken op de fouttekenreeks (de**synchronisatie-regel-fout-functie-geactiveerd** op in de afbeelding). U ziet eerst een goed beeld van het object. Als u wilt de werkelijke fout ziet, klikt u op de knop **Stack Trace**. Deze tracering bevat foutopsporingsgegevens voor de fout.

**TIP:** U kunt met de rechtermuisknop in het vak **bellen van informatie over de stack** , kies **Alles selecteren**en **kopiëren**. U kunt vervolgens kopiëren van de stapel en bekijk de fout in uw favoriete editor, zoals Kladblok.

- Als de fout van **SyncRulesEngine is**, klikt u vervolgens heeft de stapel oproepgegevens eerst een lijst met alle kenmerken van het object. Schuif omlaag totdat u de kop ziet **InnerException = >**.  
![Synchronisatie-servicebeheer](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
De regel na de fout wordt weergegeven. In de bovenstaande afbeelding wordt de fout is van een aangepaste regel-Fabrikam voor synchronisatie is gemaakt.

Als de fout zelf geen weinig gegevens geeft, wordt het tijd om de gegevens zelf te bekijken. U kunt klikken op de koppeling met de object-id en [voert u een object en de gegevens in het systeem](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de configuratie [Azure AD Connect synchroniseren](active-directory-aadconnectsync-whatis.md) .

Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
