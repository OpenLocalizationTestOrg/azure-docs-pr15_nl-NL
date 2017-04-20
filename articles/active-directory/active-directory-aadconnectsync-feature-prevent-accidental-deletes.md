<properties
   pageTitle="Synchronisatie van Azure AD Connect: niet per ongeluk verwijderen | Microsoft Azure"
   description="In dit onderwerp worden de functie voorkomen dat per ongeluk verwijdert (voorkomen onbedoeld verwijderen) in Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Synchronisatie van Azure AD Connect: niet per ongeluk verwijderen
In dit onderwerp worden de functie voorkomen dat per ongeluk verwijdert (voorkomen onbedoeld verwijderen) in Azure AD Connect.

Bij het installeren van Azure AD Connect, te voorkomen dat per ongeluk wordt verwijderd standaard ingeschakeld en geconfigureerd voor een exporteren met meer dan 500 verwijderen niet toestaan. Deze functie is ontworpen voor bescherming tegen ongeluk configuratiewijzigingen en wijzigingen aan uw on-premises adreslijst invloed heeft op veel gebruikers en andere objecten.

Gebruikelijke scenario's, wanneer u veel verwijderen opnemen ziet:

- Wijzigingen in [filteren](active-directory-aadconnectsync-configure-filtering.md) waar een hele [organisatie-eenheid](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) of [domein](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) niet ingeschakeld is.
- Alle objecten in een organisatie-eenheid worden verwijderd.
- De naam van een organisatie-eenheid wordt gewijzigd zodat alle objecten in deze worden beschouwd als buiten het bereik voor synchronisatie.

De standaardwaarde van 500 objecten kan worden gewijzigd met PowerShell met `Enable-ADSyncExportDeletionThreshold`. U kunt deze waarde om aan te passen aan de grootte van uw organisatie moet configureren. Aangezien de synchronisatie-planner elke 30 minuten wordt uitgevoerd, is de waarde van het aantal verwijderen gezien binnen 30 minuten.

Als er te veel verwijderen om te worden geëxporteerd naar Azure AD gefaseerde, klikt u vervolgens de export stopt en ontvangt u een e-mailbericht als volgt:

![Voorkomen dat per ongeluk verwijdert e](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hallo (technische contactpersoon). (Tijd) de synchronisatieservice identiteit gedetecteerd dat het aantal verwijderingen hoger dan de drempelwaarde voor geconfigureerde verwijdering (naam van organisatie). Een totaal van (getal) objecten zijn verzonden voor verwijdering in deze identiteit synchronisatie uitvoeren. Dit voldaan of de drempelwaarde geconfigureerde verwijdering van (getal) objecten overschreden. Moeten we u ter bevestiging dat deze verwijderingen moeten worden verwerkt voordat we wordt verdergaat. Raadpleeg de voorkomen onbedoeld verwijderen voor meer informatie over de fout die wordt weergegeven in deze e-mailbericht.*

U ziet ook de status `stopped-deletion-threshold-exceeded` wanneer u zoeken in het **Servicebeheer van synchronisatie** gebruikersinterface voor het profiel exporteren.
![Voorkomen dat per ongeluk verwijdert synchronisatie servicebeheer UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Als dit onverwachte was, onderzoeken en corrigerende acties uitvoeren. Ga als volgt te werk om te zien welke objecten zijn verwijderd:

1. **Synchronisatieservice** starten vanuit het startmenu.
2. Ga naar de **verbindingslijnen**.
3. Selecteer de verbindingslijn met type **Azure Active Directory**.
4. Selecteer onder **Acties** aan de rechterkant **Zoeken verbindingslijn ruimte**.
5. Selecteer **Verbinding is verbroken aangezien** in het pop-upvenster onder **bereik**en selecteert u een tijd in het verleden. Klik op **Zoeken**. Deze pagina vindt een weergave van alle objecten worden verwijderd. Door te klikken op elk item, krijgt u meer informatie over het object. U kunt ook klikken op **Kolominstelling** als u wilt toevoegen van extra kenmerken om te zien in het raster.

![Zoeken verbindingslijn ruimte](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Als deze verwijdert u aanbrengen wilt, klikt u vervolgens als volgt:

1. Tijdelijk uitschakelen van deze bescherming en deze verwijderen gaat doorlopen, voert u de PowerShell-cmdlet laten: `Disable-ADSyncExportDeletionThreshold`. Geef een globale beheerder van Azure AD-account en wachtwoord.
![Referenties](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. Met de Azure Active Directory Connector nog steeds is geselecteerd, selecteert u de actie **uitvoeren** en selecteer **exporteren**.
3. Als u wilt de beveiliging opnieuw inschakelen, voert u de PowerShell-cmdlet: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Volgende stappen

**Van overzichtsonderwerpen**

- [Synchronisatie van Azure AD Connect: begrijpen en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
- [Uw on-premises implementatie identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
