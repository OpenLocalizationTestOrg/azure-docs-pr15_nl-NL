<properties
    pageTitle="Azure AD Connect systeemstatus bewerkingen."
    description="In dit artikel worden de extra bewerkingen die kunnen worden uitgevoerd wanneer u Azure AD verbinding systeemstatus hebt geïmplementeerd."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect systeemstatus bewerkingen

Het volgende onderwerp beschrijft de verschillende bewerkingen die kunnen worden uitgevoerd met Azure AD verbinding status.

## <a name="enable-email-notifications"></a>E-mailmeldingen inschakelen
U kunt de Azure AD verbinding systeemstatus Service configureren voor het verzenden van e-mailmeldingen wanneer waarschuwingen worden gegenereerd waarin wordt aangegeven dat de infrastructuur van uw identiteit is beschadigd. Dit gebeurt wanneer een waarschuwing wordt gegenereerd, evenals wanneer deze is gemarkeerd als opgelost. Volg de onderstaande instructies voor het configureren van e-mailmeldingen.

![Azure AD Connect systeemstatus E-mail kennismaken met melding](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] E-mailmeldingen zijn standaard uitgeschakeld.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Azure AD verbinding systeemstatus e-mailmeldingen inschakelen

1. Open het blad waarschuwingen voor de service waarvoor u wilt ontvangen van e-mailmelding.
2. Klik op de knop 'Instellingen voor meldingen' op de actiebalk.
3. Schakel de schakeloptie e-mailmelding in op aan.
4. Schakel het selectievakje voor het configureren van de globale beheerders voor het ontvangen van e-mailmeldingen.
5. Als u ontvangen van e-mailmeldingen op een andere e-mailadressen wilt, kunt u deze in het vak Extra e-mailgeadresseerde opgeven. Als u wilt een e-mailadres verwijderen uit deze lijst, klik met de rechtermuisknop op de vermelding en selecteer verwijderen.
6. Als u wilt voltooien klikt u op de wijzigingen op 'Opslaan'. Alle wijzigingen kost effecten alleen nadat u 'Opslaan' hebt geselecteerd.

## <a name="delete-a-server-or-service-instance"></a>Een exemplaar van de server of service verwijderen

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Een server verwijderen uit Azure AD verbinding systeemstatus-Service
In sommige gevallen wilt u mogelijk een server verwijderen uit het wordt gecontroleerd. Volg de onderstaande instructies voor het verwijderen van een server uit Azure AD verbinding systeemstatus Service.

Wanneer u een server verwijdert, worden op de hoogte van de volgende opties:

- Deze actie wordt gestopt er nog meer gegevens verzamelen van die server. Deze server worden verwijderd uit de monitoring-service. Na deze actie is niet mogelijk om nieuwe waarschuwingen, weer te controleren of gebruik analytics-gegevens voor deze server.
- Deze actie worden niet verwijderen of de systeemstatus-Agent verwijderen uit uw server. Als u niet de systeemstatus-Agent voordat het uitvoeren van deze stap hebt verwijderd, ziet u mogelijk fouten op de server die betrekking hebben op de systeemstatus-Agent.
- Deze actie worden de gegevens van deze server al verzameld niet verwijderd. Aan de hand van het bewaarbeleid voor gegevens van Microsoft Azure die gegevens verwijderd.
- Nadat u deze actie uitvoert, als u wilt starten van de controle van dezelfde server opnieuw, moet u te verwijderen en opnieuw installeren van de servicestatus-agent op deze server.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Een server van Azure AD verbinding systeemstatus Service verwijderen

Azure AD Connect systeemstatus naar AD FS & Azure AD-verbinding (-synchronisatie):

1. Open het blad Server vanuit het blad van de lijst Server door te selecteren van de naam van de server moet worden verwijderd.
2. Klik op het blad Server op de knop "Verwijderen" op de actiebalk.
3. Bevestig de actie als u wilt verwijderen van de server door de servernaam te typen in het bevestigingsvenster.
4. Klik op de knop "Verwijderen".

Azure AD Connect servicestatus voor AD DS:

1. Open het dashboard domeincontrollers.
2. Selecteer de domeincontroller zijn verwijderd.
3. Klik op de knop 'Verwijderen geselecteerd' op de actiebalk.
4. Bevestig de actie als u wilt verwijderen van de server.
5. Klik op de knop "Verwijderen".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Een service-exemplaar van Azure AD verbinding systeemstatus Service verwijderen

In sommige gevallen wilt u mogelijk een exemplaar van service verwijderen. Volg de onderstaande instructies voor het verwijderen van een service-exemplaar van Azure AD verbinding gezondheidszorg.

Wanneer u een exemplaar van service verwijdert, worden op de hoogte van de volgende opties:

- Deze actie worden het huidige exemplaar van de service verwijderd uit de monitoring-service.
- Deze actie worden niet verwijderen of de systeemstatus-Agent verwijderen uit een van de servers die als onderdeel van dit exemplaar van de service zijn gecontroleerd. Als u niet de systeemstatus-Agent voordat het uitvoeren van deze stap hebt verwijderd, ziet u mogelijk fouten op de server (s) die betrekking hebben op de systeemstatus-Agent.
- Alle gegevens van dit exemplaar van de service worden aan de hand van het bewaarbeleid voor Microsoft Azure-gegevens verwijderd.
- Na het uitvoeren van deze actie als u beginnen met de service controleren wilt, verwijder de installatie en opnieuw installeren van de servicestatus-agent op alle servers die wordt gecontroleerd. Nadat u deze actie uitvoert, als u wilt starten van de controle van dezelfde server opnieuw, moet u te verwijderen en opnieuw installeren van de servicestatus-agent op deze server.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Een exemplaar van service verwijderen uit Azure AD verbinding systeemstatus-Service

1. Open het blad Service vanuit het blad van de lijst Service door te selecteren van de service-id (farmnaam) die u wilt verwijderen.
2. Klik op het blad Server op de knop "Verwijderen" op de actiebalk.
3. Controleer de servicenaam van de door dit te typen in het bevestigingsvenster. (bijvoorbeeld: sts.contoso.com)
4. Klik op de knop "Verwijderen".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Beheren van toegang met Rolgebaseerd toegangsbeheer
### <a name="overview"></a>Overzicht
Status verbinding maken met Azure AD- [Rol gebaseerd toegangsbeheer](role-based-access-control-configure.md) biedt toegang status verbinding maken met Azure AD-service met/gebruikers of groepen buiten globale beheerders. Hiermee wordt bereikt door rollen toewijzen aan de beoogde gebruikers en/of-groepen en techniek als u wilt de globale beheerders binnen uw adreslijst te beperken.

#### <a name="roles"></a>Rollen
Azure AD verbinding systeemstatus ondersteunt de volgende ingebouwde functies.

| Rol | Machtigingen |
| ----------- | ---------- |
| Eigenaar | Eigenaren kunnen ***de toegang beheren*** (bijvoorbeeld rol toewijzen aan een gebruiker/groep), ***alle informatie weergeven*** (bijvoorbeeld waarschuwingen bekijken) via de portal en ***-instellingen wijzigen*** (bijvoorbeeld een e-mailmeldingen) binnen Azure AD verbinding status. <br>Standaard Azure AD globale beheerders deze rol zijn toegewezen en dit kan niet worden gewijzigd.  |
|Inzender|  Inzenders kunnen ***alle informatie weergeven*** (bijvoorbeeld waarschuwingen bekijken) via de portal en ***-instellingen wijzigen*** (bijvoorbeeld een e-mailmeldingen) binnen Azure AD verbinding status.|
|Lezer| Lezers kunnen ***alle informatie weergeven*** (bijvoorbeeld waarschuwingen bekijken) in de portal binnen Azure AD verbinding status.|

Alle andere rollen (zoals 'Access Gebruikersbeheerders' of 'DevTest Labs Users'), zelfs als deze functie beschikbaar is in de portal-ervaring, hebben geen gevolgen voor toegang tot binnen Azure AD verbinding status.

#### <a name="access-scope"></a>Access-bereik

Azure AD Connect ondersteunt het beheren van access op twee niveaus:

- ***Alle exemplaren van de service***: dit is de aanbevolen procedure voor de meeste klanten en controleert de toegang voor alle exemplaren van de service (bijvoorbeeld een ADFS-farm) in alle rol typen die zijn worden gecontroleerd door Azure AD verbinding status.

- ***Service-exemplaar***: In sommige gevallen, moet u mogelijk scheiden van access die zijn gebaseerd op rol typen of door een exemplaar van service. In dit geval kunt u access op het niveau van de service-exemplaar beheren.  

Machtiging wordt toegewezen als een eindgebruiker heeft toegang tot op het niveau van de map of exemplaar van de Service.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Hoe u gebruikers of groepen toegang tot Azure AD verbinding systeemstatus toestaan
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Stap 1: Selecteer het juiste-bereik
Als u wilt toestaan dat een gebruikerstoegang op het niveau van de *overal service* binnen Azure AD verbinding maken met status, het belangrijkste blad in Azure AD verbinding systeemstatus te openen.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Stap 2: Gebruikers, groepen toevoegen en toewijzen van rollen
1. Klik op het gedeelte 'Gebruikers' in de sectie met configureren.<br>
![Azure AD Connect systeemstatus RBAC belangrijkste Blade](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Selecteer 'Toevoegen'
3. Selecteer de "rol" zoals "Eigenaar"<br>
![Azure AD Connect systeemstatus RBAC gebruiker toevoegen](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Typ de naam of de id van de gerichte gebruiker of groep. U kunt een of meer gebruikers of groepen selecteren op hetzelfde moment. Klik op "select".
![Azure AD Connect systeemstatus RBAC Selecteer gebruiker](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Selecteer "Ok".<br>

6. Zodra de roltoewijzing voltooid is, worden de gebruikers en/of groepen wordt weergegeven in de lijst.<br>
![Azure AD Connect systeemstatus RBAC gebruikerslijst](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Deze stappen gebruikt, kan de weergegeven gebruikers en de groepstoegang aan de hand van hun rollen.
>[AZURE.NOTE]
- Globale beheerders hebben altijd volledige toegang tot alle bewerkingen maar hoofdbeheerder accounts worden niet in de bovenstaande lijst.
- "Gebruikers uitnodigen" functie wordt niet ondersteund in Azure AD verbinding servicestatus.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Stap 3: De locatie blade delen met gebruikers of groepen
1. Na het toewijzen van machtigingen, een gebruiker heeft toegang tot Azure AD verbinding systeemstatus door naar [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth)te gaan.
2. Eenmaal op het blad, de gebruiker kan het blade of vastmaken verschillende onderdelen aan het dashboard door te klikken op "Pincode naar dashboard"<br>
![Azure AD verbinding systeemstatus RBAC pincode blade](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Een gebruiker met de rol van de toegewezen "Lezer" is niet mogelijk om de bewerking 'maken' als u wilt ophalen van Azure AD verbinding systeemstatus toestelnummer van de Azure Marketplace. Deze gebruiker kan nog bereiken het blad door te gaan met de bovenstaande koppeling. Voor later gebruik, kan de gebruiker het blad naar dashboard vastmaken.

### <a name="remove-users-andor-groups"></a>Gebruikers en/of groepen verwijderen
U kunt verwijderen van een gebruiker of een groep toegevoegd aan een deel van de Azure AD verbinding systeemstatus rol gebaseerd toegangsbeheer met de rechtermuisknop te klikken en verwijderen te selecteren.<br>
![Azure AD Connect systeemstatus RBAC verwijderen gebruiker](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus Agent installatie](active-directory-aadconnect-health-agent-install.md)
* [Gebruik van Azure AD status verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Gebruik van Azure AD status verbinding met AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect systeemstatus Veelgestelde vragen](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
