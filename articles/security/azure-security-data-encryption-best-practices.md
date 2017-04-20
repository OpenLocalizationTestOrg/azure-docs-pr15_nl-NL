<properties
   pageTitle="Aanbevolen procedures voor beveiliging van gegevens en versleuteling | Microsoft Azure"
   description="In dit artikel vindt u een set aanbevolen procedures voor beveiliging van gegevens en het gebruik van versleuteling ingebouwd in Azure mogelijkheden."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Azure, gegevensbeveiliging en codering aanbevolen procedures

Een van de toetsen gegevensbescherming in de cloud is accounting voor de mogelijke Staten waarin uw gegevens kunnen optreden, en welke instellingen zijn beschikbaar voor deze staat. Voor de toepassing van Azure-gegevens worden beveiliging en codering aanbevolen procedures de aanbevelingen rond de lidstaten van de volgende gegevens:

- In rust: Dit omvat alle gegevens opslagobjecten, containers en typen die aanwezig statisch op fysieke media, worden deze magnetische of optical schijf.

- In de overdracht: Wanneer gegevens worden overgebracht tussen onderdelen, locaties of -programma's, zoals via het netwerk over een service bus (vanuit een on-premises naar cloud en omgekeerd, met inbegrip van hybride verbindingen zoals ExpressRoute), of tijdens een invoer/uitvoer deze wordt beschouwd als wordt in beweging.

In dit artikel bespreken we een verzameling Azure gegevens beveiliging en codering aanbevolen procedures. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure gegevensbeveiliging en versleuteling en de ervaringen van klanten zoals zelf.

Voor elke aangeraden leggen we:

- Wat de beste manier is
- Waarom u wilt deze aanbevolen procedure inschakelen
- Wat mogelijk het resultaat als u niet het wordt aanbevolen inschakelen
- Mogelijke alternatieven voor het wordt aanbevolen
- Hoe u kunt meer zodat het wordt aanbevolen

In dit artikel Azure gegevensbeveiliging en codering aanbevolen procedures is gebaseerd op een advies consensus en Azure platformmogelijkheden en functiesets, die zijn op de tijd die in dit artikel is geschreven. Meningen en -technologieën wijzigen na verloop van tijd en in dit artikel worden bijgewerkt regelmatig om deze wijzigingen aan te geven.

Azure gegevens beveiliging en codering aanbevolen procedures in dit artikel beschreven opnemen:

- Meervoudige verificatie afdwingen
- Gebruik Rolgebaseerd toegangsbeheer RBAC)
- Azure virtuele machines versleutelen
- Modellen voor macrobeveiliging van hardware gebruiken
- Met Secure Workstations beheren
- SQL-versleuteling voor gegevens inschakelen
- Gegevens tijdens overdracht beschermen
- Bestand niveau gegevensversleuteling afdwingen


## <a name="enforce-multi-factor-authentication"></a>Meervoudige verificatie afdwingen

De eerste stap in gegevenstoegang en besturingselement in Microsoft Azure is om de gebruiker te verifiëren. [Azure meervoudige verificatie MFA ()](../multi-factor-authentication/multi-factor-authentication.md) is een methode om te controleren gebruikersidentiteit met behulp van een andere methode dan alleen een gebruikersnaam en wachtwoord. Deze verificatie methode helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden.

Doordat Azure MFA voor uw gebruikers toevoegt u een tweede laag van beveiliging aan de gebruiker aanmeldingen en transacties. In dit geval een transactie mogelijk worden toegang tot een document in een bestandsserver of in uw SharePoint Online. Azure MFA kunt u er ook IT naar de kans dat een beschadigde referentie toegang tot de gegevens van de organisatie hebben.

Bijvoorbeeld: als u het afdwingen van Azure MFA voor uw gebruikers en configureer deze een telefoongesprek of SMS-bericht als verificatie, gebruiken als referentie van de gebruiker is niet meer veilig, toegang tot alle bronnen omdat hij hebben geen toegang tot de telefoon van de gebruiker de hacker niet mogelijk. Organisaties die deze extra laag van de identiteitsbeveiliging van de niet opgeteld zijn meer vatbaar referentie diefstal aanval, wat tot inbreuk op gegevens leiden kan.

Een alternatief voor organisaties die behouden de verificatie besturingselement wilt on-premises implementatie is via [Azure meervoudige verificatieserver](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), ook wel genoemd MFA on-premises implementatie. Met deze methode is nog steeds mogelijk om te dwingen voor meervoudige verificatie, terwijl de MFA on-premises servers.

Lees het artikel [aan de slag met Azure meervoudige verificatie in de cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)voor meer informatie over Azure MFA.

## <a name="use-role-based-access-control-rbac"></a>Gebruik Rolgebaseerd toegangsbeheer RBAC)
Op basis van de beveiliging [wilt weten](https://en.wikipedia.org/wiki/Need_to_know) en [laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege) beginselen toegang beperken. Dit is dwingende voor organisaties die de wilt afdwingen beveiligingsbeleid voor apparaten voor toegang tot gegevens. Azure Rolgebaseerd Access besturingselement RBAC () kan worden gebruikt voor machtigingen toewijzen aan gebruikers, groepen en toepassingen op een bepaald bereik. Het bereik van een roltoewijzing is een abonnement, een resourcegroep of één resource.

U kunt gebruikmaken van [ingebouwde RBAC rollen](../active-directory/role-based-access-built-in-roles.md) in Azure bevoegdheden toewijzen aan gebruikers. Kunt u *Opslagruimte Account Inzender* bij cloud operatoren die nodig hebt voor het beheren van opslag accounts en de rol *Inzender van de Account klassieke opslag* voor het beheren van klassieke opslag-accounts. Voor cloud operatoren die vereist zijn voor het beheren van VMs en opslag-account, kunt u bovendien deze toe te voegen aan de rol *Inzender VM* .

Organisaties die de gegevens beheren in access niet afdwingen door gebruikmaken van de mogelijkheden zoals RBAC mogelijk meer bevoegdheden dan nodig voor hun gebruikers geven. Dit kan leiden naar gegevens compromissen doordat enkele gebruikers toegang hebben tot gegevens die ze in eerste instantie niet mag hebben.

Vindt u meer informatie over Azure RBAC Lees het artikel [Toegangsbeheer Azure Role-Based](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Azure virtuele Machines versleutelen
Voor veel organisaties is [gegevensversleuteling in rust](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) een verplichte stap naar gegevensprivacy, naleving en gegevens te garanderen. Azure schijfversleuteling kunnen IT-beheerders voor het coderen van Windows en Linux IaaS Virtual Machine (VM) schijven. Azure schijfversleuteling maakt gebruik van de functie standaard BitLocker industrie van Windows en de functie DM-Crypt van Linux te leveren volume-versleuteling voor het besturingssysteem en de gegevensschijven.

U kunt gebruikmaken van Azure schijfversleuteling om u te helpen beveiligen en beschermen van uw gegevens om te voldoen aan uw organisatie-beveiliging en nalevingsvereisten. Organisaties moeten ook rekening houden met behulp van versleuteling risico's gerelateerd aan onbevoegde gegevenstoegang beperken. Het verdient ook versleutelen stations vóór het schrijven van gevoelige gegevens toe.

Zorg ervoor dat van uw VM gegevensvolumes en opstartvolume ter bescherming van gegevens in rust in uw account Azure opslag versleutelen. Bescherming van de toetsen van de versleuteling en geheimen door gebruikmaken van [Azure toets kluis](../key-vault/key-vault-whatis.md).

Voor uw on-premises implementatie Windows-Servers, kunt u bovendien het volgende versleuteling aanbevolen procedures:

- [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) gebruiken om gegevens te coderen
- Herstelinformatie in AD DS opslaan.
- Als er vragen dat is BitLocker toetsen geknoeid, is het raadzaam dat u op het station alle exemplaren van de metagegevens BitLocker verwijderen uit het station opmaken of die u decoderen en coderen van de hele station opnieuw.

Organisaties die niet gegevensversleuteling afdwingen zijn waarschijnlijk meer geneigd worden blootgesteld aan gegevens integriteit problemen, zoals schadelijke of rogue gebruikers gegevens worden gestolen door en is gehackt accounts onbevoegde toegang krijgen tot gegevens in de opmaak wissen. Bedrijven die moeten voldoen aan industriële voorschriften, moeten naast deze risico's bewijzen dat ze doen en de juiste beveiliging besturingselementen gebruikt om gegevensbeveiliging te verbeteren.

Vindt u meer informatie over Azure schijfversleuteling Lees het artikel [Azure schijf versleuteling voor Windows en Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Hardware beveiligingsmodules gebruiken

Geheime sleutels industrie versleuteling oplossingen gebruiken om gegevens te versleutelen. Daarom kritieke dat deze toetsen veilig worden opgeslagen. Key management wordt een integraal onderdeel van de gegevensbeveiliging van, aangezien dit wordt worden gebruikt om op te slaan geheime sleutels die worden gebruikt om gegevens te versleutelen.

[Azure toets kluis](https://azure.microsoft.com/services/key-vault/) Azure schijfversleuteling gebruikt om te bepalen en schijf versleuteling toetsen en geheimen in uw belangrijkste kluis-abonnement en ervoor zorgen dat dat alle gegevens in de virtuele machine schijven worden gecodeerd in rust in Azure opslag beheren. U kunt Azure-toets kluis controle toetsen en het gebruik van beleid.

Zijn er veel risico's gerelateerd aan dat niet nodig beveiliging besturingselementen op hun plaats staan de geheime sleutels die zijn gebruikt voor het coderen van uw gegevens beveiligen. Als onbevoegden toegang tot de geheime sleutels hebt, kunnen ze de gegevens decoderen en mogelijk hebben toegang tot vertrouwelijke informatie.

Vindt u meer informatie over algemene aanbevelingen voor certificaatbeheer in Azure wordt aangegeven door het lezen van het artikel [Certificaatbeheer in Azure wordt aangegeven: tips](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Lees voor meer informatie over Azure-toets kluis, meer [aan de slag met Azure toets kluis](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Met Secure Workstations beheren

Aangezien de meeste van deze aanvallen doelgerichte de eindgebruiker, wordt het eindpunt een van de primaire punten van aanval. Als het eindpunt te onbevoegden storen, kan hij gebruikmaken van de referenties van de gebruiker toegang tot de gegevens van de organisatie. De meeste eindpunt aanvallen zijn profiteren van het feit dat eindgebruikers-beheerders op hun lokale werkplek.

U kunt deze risico's verminderen met behulp van een beveiligde beheerswerkstation. Het is raadzaam dat u een [Bevoegdheden Access werkstations (PAW)](https://technet.microsoft.com/library/mt634654.aspx) gebruiken om te verkleinen aanvallen in werkstations. Deze secure beheerwerkstation kunt u enkele van deze beperken aanvallen zorgen dat uw gegevens beschermen. Controleer of u PAW beveiliging en uw uitgerust met een vergrendelen. Dit is een belangrijk om te garanderen dat een geheime voor gevoelige accounts, taken en gegevensbescherming.

Gebrek aan eindpunt bescherming tegen mogelijk risico voor uw gegevens, controleert u of om af te dwingen beveiligingsbeleid voor apparaten op alle apparaten die worden gebruikt voor het gebruik van gegevens, ongeacht de gegevenslocatie (cloud of on-premises).

Meer informatie over de juiste rechten weer te geven workstation lezen van het artikel [Bevoegdheden Access beveiligen](https://technet.microsoft.com/library/mt631194.aspx), kunt u lezen.

## <a name="enable-sql-data-encryption"></a>SQL-versleuteling voor gegevens inschakelen

[Azure SQL-Database transparante gegevensversleuteling](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) helpt beschermen tegen schadelijke activiteit door realtime coderen en decoderen van de database, gekoppeld back-ups en transactie-logbestanden in rust zonder wijzigingen in de toepassing in te voeren.  TDE versleutelt de opslag van een volledige database met behulp van een symmetric sleutel database versleutelingssleutel genoemd.

Zelfs als u de hele opslag is versleuteld, is het belangrijk dat u uw database zelf ook worden versleuteld. Dit is een implementatie van de beveiliging bij benadering van de diepteas voor gegevensbeveiliging. Als u [Azure SQL-Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) gebruikt en u wilt beveiligen van vertrouwelijke gegevens zoals creditcard of BSN-nummers, kunt u databases met FIPS 140-2-gevalideerd 256 bit AES-versleuteling gebruikt die voldoet aan de vereisten van veel industrienormen (bijvoorbeeld HIPAA, PCI) coderen.

Het is belangrijk om te begrijpen dat bestanden die zijn gerelateerd aan [buffer groep extensie](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) niet worden gecodeerd wanneer een database is versleuteld met TDE. U moet bestand Systeemwerkset niveau versleuteling zoals BitLocker gebruiken of de- [Bestandssysteem coderen](https://technet.microsoft.com/library/cc700811.aspx) (EFS) voor BPE gerelateerde bestanden.

Sinds een geautoriseerde gebruiker, zoals een beveiligingsbeheerder of een databasebeheerder van de toegang heeft tot de gegevens zelfs als de database is versleuteld met TDE, moet u ook de aanbevelingen volgt onderstaande:

- SQL-verificatie op het databaseniveau van de
- Azure AD-verificatie op basis van rollen RBAC
- Gebruikers en toepassingen moeten afzonderlijk accounts gebruiken om te verifiëren. Deze manier kunt u beperkingen instellen voor de machtigingen voor gebruikers en toepassingen en de risico's van schadelijke activiteit te verkleinen
- Beveiliging van database op gebruikersniveau implementeren met behulp van vaste database-functies (zoals db_datareader of db_datawriter), of u kunt aangepaste rollen voor uw toepassing expliciete machtigingen aan geselecteerde databaseobjecten maken

Organisaties die niet werkt met niveau versleuteling van de database zijn mogelijk meer vatbaar voor aanvallen die leidt tot gegevens die zich bevinden in SQL-databases problemen.

U vindt u meer over SQL TDE versleuteling Lees het artikel [Transparante gegevensversleuteling met Azure SQL-Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Gegevens tijdens overdracht beschermen

Gegevens tijdens overdracht beveiligen, moeten essentiële deel uitmaken van uw strategie voor gegevensbescherming. Aangezien gegevens wordt heen en weer vanaf diverse locaties worden verplaatst, is het algemeen wordt aanbevolen dat u altijd SSL/TLS-protocollen gebruiken voor gegevensuitwisseling meerdere locaties. In sommige gevallen wilt u mogelijk de hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuur via een VPN (VPN).

Voor gegevens verplaatsen tussen uw on-premises implementatie-infrastructuur en Azure, passende garanties zoals HTTPS of VPN rekening moet houden.

Voor organisaties die moeten vanaf beveiligde toegang vanuit meerdere werkstations gebruik bevindt on-premises implementatie naar Azure [Azure VPN van site-naar-site](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Voor organisaties die moeten toegang vanuit één werkstation bevinden on-premises tot Azure secure gebruiken [punt-naar-Site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Grotere gegevens sets kunnen worden verplaatst via een speciale snelle WAN-koppeling zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u besluit om ExpressRoute gebruiken, kunt u ook de gegevens van de toepassing niveau coderen met [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor beveiliging toegevoegd.

Als u interactief met Azure Storage via de Portal Azure werken zijn, worden alle transacties plaatsvinden via HTTPS. [Opslag REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kunnen ook worden gebruikt om te communiceren met [Azure opslagruimte](https://azure.microsoft.com/services/storage/) en [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet worden gegevens tijdens overdracht beschermen zijn meer vatbaar voor [man in het midden-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [afluisteren](https://technet.microsoft.com/library/gg195641.aspx) en sessie hackprogramma. Deze aanvallen kunnen de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens zijn.

Vindt u meer informatie over de optie VPN Azure Lees het artikel [Planning en een ontwerp voor VPN Gateway](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Bestand niveau gegevensversleuteling afdwingen

Een andere laag die u kunt het niveau van uw gegevens veilig vergroten, is het bestand zelf, ongeacht de locatie van het coderen.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) wordt versleuteling, identiteit en autorisatie beleidsregels gebruikt om te helpen beveiligen uw bestanden en e-mail. Azure RMS op meerdere apparaten werkt, telefoons, tablets en pc's door te beschermen binnen uw organisatie en buiten uw organisatie. Deze is mogelijk omdat Azure RMS een beschermingsniveau die met de gegevens, blijft zelfs wanneer laat u de grenzen van uw organisatie wordt toegevoegd.

Wanneer u Azure RMS gebruikt voor het beveiligen van uw bestanden, gebruikt u gestandaardiseerde cryptografische met volledige ondersteuning van [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wanneer u gebruikmaken van Azure RMS voor gegevensbescherming, hebt u zelfs als deze wordt gekopieerd naar opslag die niet onder het beheer van de assurance die de beveiliging met het bestand blijft, IT, zoals een cloudopslagservice. Hetzelfde gebeurt voor bestanden die worden gedeeld via e-mail, het bestand is beveiligd als bijlage bij een e-mailbericht met instructies voor het openen van de beveiligde bijlage.

Bij het plannen van Azure RMS adoptie wordt aangeraden het volgende:

- Installeer het [RMS app delen](https://technet.microsoft.com/library/dn339006.aspx). Deze app werkt naadloos samen met Office toepassingen met de installatie van een Office-invoegtoepassing zodat gebruikers kunnen bestanden gemakkelijk rechtstreeks beveiligen.
- Toepassingen en services ter ondersteuning van Azure RMS configureren
- [Aangepaste sjablonen](https://technet.microsoft.com/library/dn642472.aspx) die overeenkomen met uw vereisten voor bedrijven maken. Bijvoorbeeld: een sjabloon voor bovenste geheime gegevens die moet worden toegepast op alle bovenste geheim gerelateerd e-mailberichten.

Organisaties die zwakke op beveiliging tegen [classificatie van gegevens](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestand zijn mogelijk meer onderhevig aan gegevens lekkage. Zonder BEGINLETTERS Bestandsbeveiliging, organisaties niet mogelijk bedrijven inzichten te verkrijgen, abuse controleren en schadelijke toegang tot bestanden voorkomen.

Vindt u meer informatie over Azure RMS Lees het artikel [Aan de slag met Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
