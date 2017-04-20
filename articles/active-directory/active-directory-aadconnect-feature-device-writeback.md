<properties
    pageTitle="Azure AD Connect: Inschakelen apparaat write-backs | Microsoft Azure"
    description="In dit document een gedetailleerd overzicht van het inschakelen van apparaat write-backs met Azure AD Connect"
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
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Apparaat write-backs inschakelen

>[AZURE.NOTE] Een abonnement op Azure AD Premium is vereist voor apparaat write-backs.

De volgende documentatie vindt u informatie over het inschakelen van de functie apparaat write-backs in Azure AD Connect. Apparaat write-backs wordt gebruikt in de volgende scenario's:

- Voorwaardelijke toegang op basis van apparaten om over te ADFS inschakelen (2012 R2 of hoger) beveiligde toepassingen (gebruikmakende partij vertrouwensrelaties).

Hier vindt u extra beveiliging en assurance die toegang tot toepassingen alleen op vertrouwde apparaten wordt verleend. Zie voor meer informatie over voorwaardelijke toegang, [Risico's beheren met voorwaardelijke toegang](active-directory-conditional-access.md) en het [instellen van On-premises voorwaardelijke toegang tot een apparaatregistratie van Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Apparaten moeten zich bevinden in de dezelfde bos als de gebruikers. Aangezien apparaten moeten terug naar één worden geschreven, ondersteunt deze functie niet momenteel een implementatie met meerdere forests van de gebruiker.</li>
<li>Slechts één apparaat registratie configuratieobject kan worden toegevoegd aan de lokale Active Directory-bos. Deze functie is niet compatibel met een topologie waar de lokale Active Directory wordt gesynchroniseerd met meerdere Azure AD-mappen.</li>

## <a name="part-1-install-azure-ad-connect"></a>Deel 1: Installatiefout Azure AD verbinding maken
1. Azure AD Connect met aangepaste installeren of Express instellingen. Microsoft raadt beginnen met alle gebruikers en groepen gesynchroniseerd vóór het inschakelen van apparaat write-backs.

## <a name="part-2-prepare-active-directory"></a>Deel 2: Active Directory voorbereiden
Gebruik de volgende stappen voor het voorbereiden voor het gebruik van apparaat write-backs.

1.  Vanaf de computer waarop Azure AD Connect is geïnstalleerd, start u PowerShell in de verhoogde modus.

2.  Als de Active Directory-PowerShell-module niet is geïnstalleerd, installeert u deze via de volgende opdracht uit:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. Als de Azure Active Directory PowerShell-module niet is geïnstalleerd, klikt u vervolgens downloaden en installeren vanaf de [Azure Active Directory-Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297). Afhankelijk is dit onderdeel van de aanmeldhulp, die is geïnstalleerd met Azure AD Connect.

4.  Met beheerdersreferenties enterprise, voer de volgende opdrachten en sluit PowerShell.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Beheerdersreferenties voor Enterprise zijn vereist aangezien wijzigingen in de naamruimte configuratie nodig zijn. De domeinbeheerder van een heeft niet voldoende machtigingen.

![PowerShell voor het inschakelen van apparaat write-backs](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Beschrijving:

- Als deze niet al bestaat, wordt gemaakt en nieuwe containers en objecten onder CN configureert = apparaat registratie configuratie, CN = Services, CN = Configuration, [bos-dn].
- Als deze niet al bestaat, wordt gemaakt en nieuwe containers en objecten onder CN configureert = RegisteredDevices, [domein-dn]. Apparaatobjecten wordt gemaakt in deze container.
- Hiermee stelt onvoldoende machtigingen van de Azure AD-Connector-account, apparaten op uw Active Directory beheren.
- Alleen moet worden uitgevoerd op één bos, zelfs als Azure AD Connect op meerdere forests wordt geïnstalleerd.

Parameters:

- Domeinnaam: Active Directory-domein waar apparaatobjecten worden gemaakt. Opmerking: Alle apparaten voor een bepaald Active Directory-bos wordt gemaakt in één domein.
- AdConnectorAccount: Active Directory-account die worden gebruikt door Azure AD Connect objecten in de adreslijst te beheren. Dit is de account die verbinding maken met AD Azure AD Connect synchroniseren gebruikt. Als u met express instellingen hebt geïnstalleerd, is het account voorafgegaan door MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Deel 3: Inschakelen apparaat terugschrijven in Azure AD Connect
Gebruik de volgende procedure om in te schakelen apparaat write-backs in Azure AD Connect.

1.  Voer de installatiewizard opnieuw uit. Selecteert u **Synchronisatieopties aanpassen** op de pagina extra taken en klik op **volgende**.
![Aangepaste installatie Synchronisatieopties aanpassen](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  In de pagina optionele onderdelen wordt apparaat write-backs niet meer worden grijs weergegeven. Neem notitie dat als de Azure AD Connect voorbereidingen stappen zijn niet voltooide apparaat write-backs wordt worden grijs weergegeven, uitfaden in de pagina optionele onderdelen. Schakel het vakje in voor apparaat write-backs en klik op **volgende**. Als het selectievakje nog steeds is uitgeschakeld, raadpleegt u de [sectie Probleemoplossing](#the-writeback-checkbox-is-still-disabled).
![Aangepaste installatie apparaat write-backs optioneel functies](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Klik op de pagina write-backs ziet u het opgegeven domein als het standaard apparaat write-backs bos.
![Aangepaste installatie apparaat write-backs doel bos](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  De installatie van de Wizard met geen aanvullende configuratiewijzigingen voltooien. Als u nodig hebt, raadpleegt u [aangepaste installatie van Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Voorwaardelijke toegang inschakelen
Gedetailleerde instructies voor het inschakelen van dit scenario zijn beschikbaar in [bij het instellen van On-premises voorwaardelijke toegang tot een apparaatregistratie van Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Controleer of dat apparaten worden gesynchroniseerd met Active Directory
Apparaat write-backs moet nu correct werkt. Let erop dat het voor apparaatobjecten worden geschreven terug naar AD tot 3 uur kan duren.  U kunt bevestigen dat uw apparaten correct zijn die worden gesynchroniseerd door als volgt nadat de synchronisatie-regels hebt voltooid:

1.  Active Directory-Beheerderscentrum starten.
2.  Uitvouwen RegisteredDevices, binnen het domein dat is worden gekoppeld.
![Active Directory-beheercentrum geregistreerd apparaten](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Huidige geregistreerde apparaten worden, er vermeld.
![Active Directory-beheercentrum geregistreerd apparatenlijst](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="the-writeback-checkbox-is-still-disabled"></a>Het selectievakje write-backs is nog steeds uitgeschakeld
Als het selectievakje voor apparaat write-backs is niet ingeschakeld, zelfs als u de bovenstaande stappen hebt gevolgd, de volgende stappen begeleidt u bij wat de installatiewizard is verifiëren voordat het vak is ingeschakeld.

Eerste dingen eerste:

- Controleer of minimaal één bos Windows Server 2012R2 heeft. Het type apparaat object moet aanwezig zijn.
- Als de installatiewizard al wordt uitgevoerd, wordt klikt u vervolgens wijzigingen niet gedetecteerd. In dit geval voert u de installatiewizard en voer deze opnieuw.
- Controleer of het account dat u in het script initialisatie opgeeft is dat is wel de juiste gebruiker die worden gebruikt door de Active Directory-Connector. U kunt dit controleren, als volgt te werk:
    - Open de **Synchronisatieservice**in het startmenu.
    - Open het tabblad **verbindingslijnen** .
    - De verbindingslijn vinden met het type Active Directory Domain Services en selecteer deze.
    - Onder **Acties**selecteert u **Eigenschappen**.
    - Ga naar het **verbinding maken met Active Directory bos**. Controleer of de naam van het domein en de gebruiker opgegeven op dit scherm van de zoekwaarde het account voor het script beschikbaar.
![Connector-account in synchronisatie servicebeheer](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Controleer of de configuratie in Active Directory:
- Bevestigen dat de registratie-Service van apparaat bevindt zich in de onderstaande locatie (CN DeviceRegistrationService, CN = apparaat registratie Services, CN = apparaat registratie configuratie, CN = = Services, CN = Configuration) onder configuratie naming context.

![Problemen met, DeviceRegistrationService in configuratie naamruimte](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Controleer of er slechts één configuratieobject is door te zoeken van de naamruimte configuratie. Als er meer dan één, verwijder het duplicaat.

![Problemen met, zoekt u de dubbele objecten](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Controleer op het apparaat registratieservice-object, of het kenmerk msDS-DeviceLocation aanwezig is en een waarde heeft. Opzoeken deze locatie en zorg ervoor dat het presenteren met de objectType msDS-DeviceContainer is.

![Problemen met, msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Problemen met, RegisteredDevices object class](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Controleer of het account dat is gebruikt door de Active Directory-Connector heeft vereiste machtigingen voor de container geregistreerde apparaten door de vorige stap. Dit is de verwachte machtigingen voor deze container:

![Problemen met, Controleer de machtigingen op container](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Controleer of het Active Directory-account machtigingen heeft voor de CN = apparaat registratie Configuration, CN = Services, CN = configuratieobject.

![Problemen met, Controleer de machtigingen op apparaat registratie configuratie](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Aanvullende informatie
- [Risico's met voorwaardelijke toegang beheren](active-directory-conditional-access.md)
- [Bij het instellen van On-premises voorwaardelijke toegang tot een apparaatregistratie van Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
