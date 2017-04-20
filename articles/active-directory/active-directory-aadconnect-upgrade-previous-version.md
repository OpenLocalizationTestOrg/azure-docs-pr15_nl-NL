<properties
   pageTitle="Azure AD Connect: Een upgrade uitvoert vanuit een eerdere versie | Microsoft Azure"
   description="Dit artikel wordt uitgelegd de verschillende methoden voor het upgraden naar de meest recente versie van Azure Active Directory verbinding kunt maken, inclusief in-place upgrade en migratie swing."
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
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Upgrade van een eerdere versie naar de meest recente
In dit onderwerp worden de verschillende methoden die u gebruiken kunt voor het bijwerken van uw Azure AD Connect-installatie naar de meest recente versie. Het is raadzaam dat u zelf actueel met de versies van Azure AD Connect houden. De stappen in de [migratie bewegen](#swing-migration) worden ook gebruikt wanneer u een aanzienlijke configuratie wijzigen.

Als u een upgrade uitvoert vanuit DirSync wilt, raadpleegt u [een upgrade uitvoert vanuit Azure AD-synchronisatie (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) in plaats daarvan.

Er zijn een paar andere strategieën Azure AD Connect upgrade uit te voeren.

Methode | Beschrijving
--- | ---
[Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) | Dit is de eenvoudigste methode voor klanten met een snelle installatie.
[In-place upgrade](#in-place-upgrade) | Als u één server hebt, upgrade van de installatie ter plaatse op dezelfde server.
[Migratie bewegen](#swing-migration) | U kunt met twee servers voorbereiden op een van de servers met de nieuwe versie of de configuratie en active server wijzigen wanneer u klaar bent.

Zie [machtigingen vereist voor de upgrade](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)voor de vereiste machtigingen.

## <a name="in-place-upgrade"></a>In-place upgrade
Een in-place upgrade werkt voor het verplaatsen van Azure AD-synchronisatie of Azure AD Connect. Dit werkt niet voor DirSync of voor een oplossing met FIM + Azure AD-Connector.

Deze methode is voorkeur als er één server en minder dan ongeveer 100.000 objecten. Als er wijzigingen van de regels kant-en-klare synchroniseren, wordt een volledige importeren en de volledige synchronisatie optreden na de upgrade. Dit zorgt ervoor dat de nieuwe configuratie wordt toegepast op alle bestaande objecten in het systeem. Dit kan een paar uur al naargelang het aantal objecten in het bereik van de synchronisatie-engine duren. De normale delta synchronisatie scheduler, al dan niet standaard elke 30 minuten, is geschorst maar Wachtwoordsynchronisatie blijft. U overwegen moet de in-place upgrade tijdens een weekend. Als er geen wijzigingen aan de kant-en-klare-configuratie met de nieuwe versie van Azure AD Connect zijn, wordt in plaats daarvan een normale delta importeren/synchronisatie starten.  
![In-Place Upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Als u wijzigingen hebt aangebracht in kant-en-klare synchronisatieregels, wordt deze opnieuw zijn ingesteld op standaard-configuratie op upgrade. Zorg dat de wijzigingen worden aangebracht, zoals wordt beschreven in de [Aanbevolen procedures voor het wijzigen van de standaardconfiguratie](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)om ervoor te zorgen dat uw configuratie wordt bewaard tussen upgrades.

## <a name="swing-migration"></a>Migratie bewegen
Als u een complexe implementatie of zeer groot aantal objecten hebt, is het mogelijk dat deze te doen een in-place upgrade live-mailsysteem. Dit kan voor sommige klanten meerdere dagen duren en tijdens deze periode geen deltawijzigingen worden verwerkt. Deze methode wordt ook gebruikt wanneer u van plan bent belangrijke wijzigingen aanbrengen in uw configuratie en u ze uitprobeert wilt voordat deze worden verplaatst naar de cloud.

De aanbevolen methode voor deze scenario's is via een migratie swing. U moet (minimaal) twee servers, één actief en één tijdelijk opslaan server. De actieve server (ononderbroken blauwe lijn in de onderstaande afbeelding) is verantwoordelijk voor het laden van de actieve productietaken. Het tijdelijk opslaan (paarse stippellijnen in de onderstaande afbeelding)-server is voorbereid met de nieuwe versie of de configuratie en wanneer volledig klaar is, wordt deze server actief gemaakt. De vorige active server, nu met de oude versie of de configuratie is geïnstalleerd, is het tijdelijk opslaan server gemaakt en bijgewerkt.

De twee servers kunnen verschillende versies gebruiken. Bijvoorbeeld de active server die u van plan bent om op te nemen de beschikking over Azure AD-synchronisatie en de nieuwe tijdelijk opslaan server Azure AD Connect kunt gebruiken. Als u swing migratie gebruiken voor het ontwikkelen van een nieuwe configuratie is een goed idee om dezelfde versie op de twee servers hebt.  
![Tijdelijke server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Opmerking: Dit is vastgesteld dat sommige klanten liever drie of vier servers voor dit scenario. Wanneer de server tijdelijk opslaan wordt bijgewerkt, hoeft u niet een back-server voor het geval een [herstel](active-directory-aadconnectsync-operations.md#disaster-recovery). Met drie of vier-servers kunt één set met primaire/stand-by servers met de nieuwe versie opgesteld, zorgen dat er altijd een tijdelijk opslaan server wilt overnemen.

Deze stappen werkt ook als u wilt verplaatsen van Azure AD-synchronisatie of een oplossing met FIM + Azure AD-Connector. Deze stappen niet werken voor DirSync, maar dezelfde migratie (ook wel parallelle implementatie genoemd) deze methode met instructies voor DirSync vindt u in de [Upgrade van Azure Active Directory synchroniseren (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Migratiestappen bewegen

1. Als u Azure AD Connect op beide servers gebruiken en wilt dit alleen doen in een wijziging in de configuratie, zorg dat uw active server en tijdelijke server worden beide met dezelfde versie als. Die vergemakkelijkt verschillen later vergelijken. Als u een van Azure AD-synchronisatie upgrade, hebt met deze servers verschillende versies. Als u een uitvoert voor een oudere versie van Azure AD Connect upgrade, het is een goed idee om te beginnen met de twee servers met dezelfde versie, maar dit is niet vereist.
2. Als u een aangepaste configuratie hebt aangebracht en de server tijdelijk opslaan dit geen heeft, voert u de stappen onder [aangepaste configuratie van actief naar tijdelijk opslaan server verplaatsen](#move-custom-configuration-from-active-to-staging-server).
3. Als u een van een eerdere versie van Azure AD Connect upgrade, moet u het tijdelijk opslaan server bijwerken naar de nieuwste versie. Als u van Azure AD-synchronisatie verplaatst, moet u vervolgens Azure AD Connect installeren op de server tijdelijk opslaan.
4. Laat de synchronisatie-engine volledige importeren en volledige synchronisatie worden uitgevoerd op de server tijdelijk opslaan.
5. Verifiëren dat de nieuwe configuratie er met de stappen onder **verifiëren** in [controleren of de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)onverwachte wijzigingen hebben. Als er iets is niet zoals verwacht, juiste, uitvoeren importeren en synchroniseren en verifiëren totdat de gegevens tevreden bent. Deze stappen vindt u in het onderwerp.
6. Overschakelen van de server tijdelijk opslaan als de actieve server. Dit is de laatste stap **schakelen active server** in [controleren of de configuratie van een server](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Als u een Azure AD Connect upgrade, moet u de server nu in de modus naar de meest recente versie tijdelijke bijwerken. Volg de stappen hierboven om de gegevens en configuratie die zijn bijgewerkt. Als u een upgrade van Azure AD-synchronisatie hebt uitgevoerd, kunt u nu uitschakelen en uw oude server buiten gebruik stellen.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Aangepaste configuratie van actieve naar tijdelijk opslaan server verplaatsen
Als u configuratiewijzigingen hebt aangebracht in de active server, moet u om ervoor te zorgen dat dezelfde wijzigingen zijn toegepast op de server tijdelijk opslaan.

De synchronisatie van aangepaste regels die u hebt gemaakt, kunnen worden verplaatst met PowerShell. Andere wijzigingen moeten worden toegepast als het op dezelfde manier op beide systemen en kunnen niet worden gemigreerd.

Wat u die moet u ervoor zorgen dat op dezelfde manier op beide servers is geconfigureerd:

- Verbinding met de dezelfde forests.
- Een domein en organisatie-eenheid filteren.
- Dezelfde optioneel functies, zoals Wachtwoordsynchronisatie en wachtwoord write-backs.

**Regels van de synchronisatie verplaatsen**  
Ga als volgt te werk als u wilt verplaatsen van een aangepaste synchronisatie-regel:

1. **Synchronisatie regeleditor** openen op uw active server.
2. Selecteer uw aangepaste regel. Klik op **exporteren**. Hiermee opent u een Kladblok-venster. Sla het tijdelijk bestand met de extensie PS1. Hierdoor kunt u een PowerShell-script. Kopieer het bestand ps1 op de server tijdelijk opslaan.  
![Synchronisatie regel exporteren](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. De verbindingslijn-GUID verschilt op de server tijdelijk opslaan en moet worden gewijzigd. Als u de GUID, **Synchronisatie regeleditor**te starten, selecteert u een van de kant-en-klare regels die hetzelfde verbonden systeem en klik op **exporteren**. Vervang de GUID in uw bestand PS1 door de GUID van de server tijdelijk opslaan.
4. Voer in een PowerShell-prompt de PS1-bestand. Dit wordt de synchronisatie van aangepaste regel maken op de server tijdelijk opslaan.
5. Als u meerdere aangepaste regels hebt, herhaalt u voor alle aangepaste regels.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [integreren van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).
