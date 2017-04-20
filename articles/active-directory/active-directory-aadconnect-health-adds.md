
<properties
    pageTitle="Gebruik van Azure AD status verbinding met AD DS | Microsoft Azure"
    description="Dit is de status verbinding maken met Azure AD-pagina die wordt besproken hoe om de AD DS te houden."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Gebruik van Azure AD status verbinding met AD DS
De volgende documentatie is specifiek voor Active Directory Domain Services met Azure AD verbinding servicestatus bewaken. De ondersteunde versies van AD DS zijn: Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2.

Zie voor meer informatie over het controleren van AD FS met Azure AD verbinding systeemstatus [Gebruiken Azure AD status verbinding maken met AD FS](active-directory-aadconnect-health-adfs.md). Zie bovendien [Gebruiken Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)voor informatie over het controleren van Azure AD Connect (-synchronisatie) met Azure AD verbinding status.

![Azure AD Connect servicestatus voor AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Waarschuwingen voor Azure AD verbinding servicestatus voor AD DS
De sectie waarschuwingen binnen Azure AD verbinding servicestatus voor AD DS, bevat een lijst met actieve en opgelost waarschuwingen, gerelateerd aan de domeincontroller. Een waarschuwing actief of opgelost selecteren, wordt een nieuwe blade geopend met aanvullende informatie, samen met de stappen voor het oplossen, en koppelingen naar ondersteunende documentatie. Elke waarschuwing typen kunnen een of meer exemplaren die met elke het domein beïnvloed door deze melding voor een bepaalde overeenkomen hebben. Aan de onderkant van het blad waarschuwing, kunt u dubbelklikken op een desbetreffende domeincontroller als u wilt een extra blade met meer informatie over dat waarschuwing exemplaar openen.

In dit blade, kunt u e-mailmeldingen voor waarschuwingen en het tijdsbereik in de weergave wijzigen. Het tijdsbereik uitvouwen, kunt u zien voorafgaande opgelost waarschuwingen.

![Azure AD Connect synchronisatiefout is opgetreden](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Domeincontrollers Dashboard
Dit dashboard biedt een topologische weergave van uw omgeving, samen met belangrijke operationele parameters en status van elk gecontroleerde domeincontroller. De doelstellingen van de gepresenteerde helpen bij het snel identificeren alle domeincontrollers die mogelijk verder onderzoek. Standaard wordt slechts een subset van de kolommen weergegeven. U kunt echter de gehele set beschikbare kolommen vinden door te dubbelklikken op de opdracht kolommen. De kolommen die u meest belangrijk vindt, verandert dit dashboard in een enkele en eenvoudig plaats om weer te geven van de status van uw omgeving AD DS te selecteren.

![Domeincontrollers](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domeincontrollers kunnen worden gegroepeerd op de desbetreffende domein of site, dat wil nuttig zijn zeggen voor informatie over de topologie omgeving. Ten slotte, als u dubbelklikt op de kop blade, gemaximaliseerd het dashboard voor het gebruik van de beschikbare-weergavegebied. Deze grotere weergave is handig wanneer meerdere kolommen worden weergegeven.

## <a name="replication-status-dashboard"></a>Replicatie Status Dashboard
Dit dashboard vindt u een weergave van de status herhaling en herhaling topologie gecontroleerde domeincontroller. De status van de meest recente herhaling poging wordt weergegeven, samen met nuttige documentatie voor elke fout die wordt gevonden. Kunt u dubbelklikken op een domeincontroller een fout bij het openen van een nieuwe blade met informatie, zoals: meer informatie over de fout, aanbevolen resolutie stappen en koppelingen naar de documentatie over probleemoplossing.

![Replicatiestatus](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Cmdlets voor controle
Deze functie biedt grafische trends van van verschillende prestatiemeteritems, die continu uit elk van de gecontroleerde domeincontrollers zijn verzameld. Prestaties van een domeincontroller kan eenvoudig worden vergeleken alle andere gecontroleerde domeincontrollers in uw bos. Bovendien ziet u diverse prestatietellers naast elkaar worden geïnstalleerd, die u kan helpen bij het oplossen van problemen in uw omgeving.

![Cmdlets voor controle](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Standaard hebben we dan vier prestatiemeteritems; geselecteerd u kunt anderen echter opnemen door te klikken op de filteropdracht en selecteren of als u alle gewenste items uitschakelt. Bovendien kunt u een grafiek van de teller prestaties als u wilt openen van een nieuwe blade, waaronder de gegevenspunten voor elk van de gecontroleerde domeincontrollers dubbelklikken.

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect systeemstatus](active-directory-aadconnect-health.md)
* [Azure AD Connect systeemstatus Agent installatie](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect systeemstatus bewerkingen](active-directory-aadconnect-health-operations.md)
* [Gebruik van Azure AD status verbinding met AD FS](active-directory-aadconnect-health-adfs.md)
* [Met behulp van Azure AD verbinding servicestatus voor synchronisatie](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect systeemstatus Veelgestelde vragen](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect systeemstatus versiegeschiedenis](active-directory-aadconnect-health-version-history.md)
