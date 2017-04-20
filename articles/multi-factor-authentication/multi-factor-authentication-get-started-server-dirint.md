<properties 
    pageTitle="Directory-integratie tussen Azure meervoudige verificatie en Active Directory"
    description="Dit is de pagina van de Azure meervoudige verificatie die wordt beschreven hoe u de Server Azure meervoudige verificatie integreren met Active Directory, zodat u de mappen kunt synchroniseren."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Directory-integratie van Azure MFA Server met Active Directory

De sectie Directory-integratie kunt u voor het configureren van de server voor integratie met Active Directory of een andere LDAP-diagram.  Dit kunt u kenmerken overeenkomen met het directory-schema en het instellen van automatische synchronisatie van gebruikers configureren.

## <a name="settings"></a>Instellingen
Standaard worden de Azure meervoudige verificatie-Server is geconfigureerd als u wilt importeren of gebruikers uit Active Directory synchroniseren.  Het tabblad kunt u het standaardgedrag te overschrijven en binding maken met een andere LDAP-map, een ADAM map of specifiek Active Directory-domeincontroller.  Het biedt ook voor het gebruik van de LDAP-verificatie naar proxy LDAP of voor de LDAP binden als RADIUS-doel, verificatie vooraf voor IIS, of primaire verificatie voor gebruiker Portal.  De volgende tabel beschrijft de afzonderlijke instellingen.

![Instellingen](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Functie | Beschrijving |
| ------- | ----------- |
| Active Directory gebruiken | Selecteer de optie Active Directory gebruiken voor het Active Directory gebruiken voor het importeren en synchronisatie.  Dit is de standaardinstelling. <br>Opmerking: De computer moet worden toegevoegd aan een domein en u moet zijn aangemeld met een account voor Active Directory-integratie werkt alleen naar behoren. |
| Vertrouwde domeinen opnemen | Controleren het selectievakje vertrouwde domeinen opnemen om de poging tot het agent verbinding maken met domeinen vertrouwd door het huidige domein, een ander domein in de bos of domeinen betrokken bij een bos vertrouwen.  Wanneer niet importeren of de synchronisatie van gebruikers uit een van de vertrouwde domeinen, schakel het selectievakje in om prestaties te verbeteren.  De standaardinstelling is ingeschakeld. |
| Specifieke LDAP-configuratie uitvoeren | Selecteer de optie LDAP gebruiken voor het gebruik van de opgegeven voor het importeren en synchronisatie LDAP-instellingen. Opmerking: Als Gebruik LDAP is geselecteerd, de gebruikersinterface gewijzigd verwijzingen van Active Directory in LDAP. |
| Knop bewerken | De knop bewerken kunt de huidige LDAP configuratie-instellingen gewijzigd. |
| Kenmerk bereik query's gebruiken | Geeft aan of kenmerk bereik query's moeten worden gebruikt.  Kenmerk bereik query's toestaan voor zoekopdrachten efficiënt directory in aanmerking komende records op basis van de items in een andere record-kenmerk.  De Server Azure meervoudige verificatie wordt kenmerk bereik query's gebruikt om efficiënt query's in de gebruikers die lid van een beveiligingsgroep bent.   <br>Opmerking: Zijn in sommige gevallen waarin kenmerk bereik query's worden ondersteund, maar niet mag worden gebruikt.  Active Directory kan bijvoorbeeld problemen met het kenmerk scope-query's hebben als een beveiligingsgroep leden uit meer dan één domein bevat.  In dit geval moet het selectievakje zijn uitgeschakeld. |

De volgende tabel beschrijft de LDAP-configuratie-instellingen.

| Functie | Beschrijving |
| ------- | ----------- |
| Server | Voer de hostnaam of IP-adres van de server met de LDAP-diagram.  Een back-server kan ook worden opgegeven, gescheiden door een puntkomma. <br>Opmerking: Als Type afhankelijk SSL is, een volledige hostname is gewoonlijk vereist. |
| Base DN | Voer de DN-naam van het grondtal directoryobject waaruit alle directory-query's wordt gestart.  Bijvoorbeeld domeincontroller = abc, domeincontroller = com. |
| Type - query's koppelen | Selecteer het type afhankelijk van de juiste voor gebruik wanneer binden om de LDAP-gebruikerslijst te zoeken.  Dit wordt gebruikt voor invoer, synchronisatie en gebruikersnaam resolutie. <br><br>  Anonieme - een anonieme binding wordt uitgevoerd.  DN binden en binden wachtwoord wordt niet worden gebruikt.  Dit werkt alleen als de LDAP-diagram anonieme binding kunt en machtigingen toestaan de query's uitvoeren van de juiste records en kenmerken.  <br><br> Eenvoudig - binding DN en binden wachtwoord wordt doorgegeven als tekst zonder opmaak binding maken met de LDAP-diagram.  Dit moet alleen worden gebruikt voor testdoeleinden om te bevestigen dat de server kan worden bereikt en dat het account afhankelijk van de juiste toegang heeft.  Het wordt aanbevolen dat SSL moet worden gebruikt in plaats daarvan nadat het juiste certificaat is geïnstalleerd.  <br><br> SSL - binding DN-naam en het wachtwoord binden worden gecodeerd via SSL binding maken met de LDAP-diagram.  Hiervoor is vereist dat een certificaat lokaal zijn geïnstalleerd dat de LDAP-diagram vertrouwt.  <br><br> Windows - binding gebruikersnaam en wachtwoord binden worden veilig verbinding maken met een Active Directory-domeincontroller of ADAM directory gebruikt.  Als gebruikersnaam binden leeg is, wordt de aangemelde gebruikersaccount worden gebruikt voor binding. |
| Binden type - authenticatie | Selecteer het type afhankelijk van de juiste voor gebruik tijdens de LDAP-bindingsverificatie.  Zie de beschrijvingen onder binding type - query's van het type binding.  Bijvoorbeeld: Hiermee kunt voor anonieme binding moet worden gebruikt voor query's terwijl SSL binding wordt gebruikt voor het beveiligen van LDAP binding authenticatie. |
| Binden DN of binding gebruikersnaam | Voer de DN-naam van de gebruikersrecord voor het account wilt gebruiken wanneer u aan de LDAP-diagram.<br><br>De DN-naam voor binding wordt alleen gebruikt indien gebonden Type eenvoudige of SSL.  <br><br>Voer de gebruikersnaam van de Windows-account wilt gebruiken wanneer u naar de LDAP-diagram binden als Type afhankelijk Windows is.  Als leeg laat, wordt het account van de aangemelde gebruiker worden gebruikt voor binding. |
| Afhankelijk van wachtwoord | Voer het wachtwoord binding voor de DN afhankelijk of de gebruikersnaam die wordt gebruikt om verbinding met de LDAP-map.  Voor meer informatie over het configureren van het wachtwoord voor de meervoudige Auth Server-Service met AdSync synchronisatie moet zijn ingeschakeld en de service moet worden uitgevoerd op de lokale computer.  Het wachtwoord worden opgeslagen in de Windows opgeslagen gebruikersnamen en wachtwoorden opnieuw instellen onder de account die de meervoudige Auth Server AdSync-Service wordt uitgevoerd als.  Het wachtwoord worden, ook opgeslagen onder de account die de gebruikersinterface meervoudige Auth Server wordt uitgevoerd als en klik onder de account die de meervoudige Auth Server-Service wordt uitgevoerd als.  <br><br> Opmerking: Aangezien het wachtwoord alleen in de Windows opgeslagen gebruikersnamen en wachtwoorden opnieuw instellen van de lokale server opgeslagen is, deze stap moet worden uitgevoerd op elke meervoudige Auth-Server die toegang tot het wachtwoord nodig heeft. |
| De maximale bestandsgrootte query | Geef de maximale grootte voor het maximum aantal gebruikers die een zoekopdracht directory zullen retourneren.  Deze limiet moet overeenkomen met de configuratie op de LDAP-diagram.  Voor grote zoekopdrachten waar paginering wordt niet ondersteund, probeert importeren en synchronisatie om gebruikers in batches te halen.  Als de groottelimiet overschrijden hier groter is dan de limiet voor de LDAP-diagram hebt geconfigureerd opgegeven, kunt u enkele gebruikers mogelijk gemist. |
| Knop testafbeelding | Klik op de knop testen om te testen binding naar de LDAP-server.  <br><br> Opmerking: De optie Gebruik LDAP hoeft niet te selecteren als u wilt testen binding.  Hierdoor wordt de binding te testen voordat u met de LDAP-configuratie. |

## <a name="filters"></a>Filters
Filters kunt u criteria voor het kwalificeren van records bij het uitvoeren van een zoekopdracht directory instellen.  U kunt de objecten die u wilt synchroniseren beperken door in te stellen van het filter.  

![Filters](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure meervoudige verificatie heeft de volgende 3 opties.

- **Container filter** - Geef de filtercriteria gebruikt om records van de container kwalificeren bij het uitvoeren van een zoekopdracht directory.  Voor Active Directory en ADAM, (| () objectClass=organizationalUnit)(objectClass=container)) wordt meestal gebruikt.  Voor andere mappen LDAP en moeten de filtercriteria die in aanmerking komt elk type containerobject afhankelijk van het directory-schema worden gebruikt.  <br>Opmerking: Als u leeg, ((objectClass=organizationalUnit)(objectClass=container)) wordt standaard worden gebruikt.

- **Beveiliging groepeerfilter** - Geef de filtercriteria gebruikt om de records groeperen beveiliging kwalificeren bij het uitvoeren van een zoekopdracht directory.  Voor Active Directory en ADAM, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) wordt meestal gebruikt.  Voor andere mappen LDAP en moeten de filtercriteria die in aanmerking komt elk type van het groepsobject beveiliging afhankelijk van het directory-schema worden gebruikt.  <br>Opmerking: Als u leeg, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) al dan niet standaard worden gebruikt.

- **Gebruikersfilter** - Geef de filtercriteria gebruikt om kwalificeren gebruikersrecords bij het uitvoeren van een zoekopdracht directory.  Voor Active Directory en ADAM, (en (objectClass=user)(objectCategory=person)) wordt meestal gebruikt.  Voor andere mappen LDAP (objectClass = inetOrgPerson) of een vergelijkbare afhankelijk van het directory-schema moet worden gebruikt. <br>Opmerking: Als u leeg, (en (objectCategory=person)(objectClass=user)) wordt standaard worden gebruikt.

## <a name="attributes"></a>Kenmerken
Kenmerken kunnen worden aangepast zo nodig voor een specifieke map.  Hiermee kunt u aangepaste kenmerken toevoegen en deze af te stellen de synchronisatie met alleen de kenmerken die u nodig hebt.  De waarde voor elk kenmerkveld moet de naam van het kenmerk zoals gedefinieerd in de directory-schema.  Gebruik de onderstaande tabel voor meer informatie.

![Kenmerken](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Functie | Beschrijving |
| ------- | ----------- |
| Unieke id | Voer de naam van het kenmerk van het kenmerk dat als de unieke id van de container beveiligingsgroep gebruikersrecords, en fungeert.  In Active Directory is dit meestal objectGUID.  In andere LDAP-implementaties, kan het zijn entryUUID of een vergelijkbare.  De standaardinstelling is objectGUID. |
| ... (Selecteer kenmerk) knoppen | Elk kenmerkveld heeft een knop '...' ernaast die het kenmerk selecteren dialoogvenster toestaan een kenmerk moet worden geselecteerd in een lijst worden weergegeven. <br><br>Selecteer kenmerk dialoogvenster.<br><br>Opmerking: Kenmerken, kunnen handmatig worden ingevoerd en niet moeten overeenkomen met een kenmerk in de kenmerklijst. |
| Unieke id-type | Selecteer het type van de unieke id-kenmerk.  In Active Directory is het kenmerk objectGUID van het type GUID.  In andere LDAP-implementaties, kan het zijn van het type ASCII-bytematrix of een tekenreeks.  De standaardinstelling is GUID. <br><br>Opmerking: Het is belangrijk om in te stellen dit correct aangezien synchronisatie Items wordt verwezen door hun unieke id en de unieke id-Type wordt gebruikt om het object rechtstreeks te vinden in de adreslijst.  Wanneer de waarde in de map daadwerkelijk worden opgeslagen als een matrix van de bytes van ASCII-tekens voorkomen de synchronisatie dat wordt niet goed werken, moet u dit instelt op tekenreeks. |
| DN-naam | Voer de naam van het kenmerk van het kenmerk met de DN-naam voor elke record.  In Active Directory is dit meestal distinguishedName.  In andere LDAP-implementaties, kan het zijn entryDN of een vergelijkbare.  De standaardinstelling is distinguishedName. <br><br>Opmerking: Als een kenmerk dat alleen de DN-naam niet bestaat, het kenmerk adspath kan worden gebruikt.  De "LDAP: / /<server>/" gedeelte van het pad worden automatisch verwijderd uit alleen de DN-naam van het object te verlaten. |
| De containernaam van de | Voer de naam van het kenmerk van het kenmerk de naam in een container record bevat.  De waarde van dit kenmerk wordt weergegeven in de hiërarchie Container bij het importeren uit Active Directory of synchronisatie items toe te voegen.  Standaard is de naam. <br><br>Opmerking: Als verschillende containers verschillende kenmerken voor hun namen gebruiken, meerdere container naam kenmerken kunnen worden opgegeven gescheiden door puntkomma's.  De eerste container kenmerk name gevonden op een container-object wordt gebruikt om de naam ervan weer te geven. |
| De naam van de groep beveiliging | Voer de naam van het kenmerk van het kenmerk de naam in een record van de groep beveiliging bevat.  De waarde van dit kenmerk wordt weergegeven in de lijst beveiligingsgroep bij het importeren uit Active Directory of synchronisatie items toe te voegen.  Standaard is de naam. |
| Gebruikers | De volgende kenmerken worden gebruikt voor het zoeken naar, weergeven, importeren en synchroniseren gebruikersgegevens uit de adreslijst. |
| Gebruikersnaam | Voer de naam van het kenmerk van het kenmerk met de gebruikersnaam in een gebruikersrecord voor de.  De waarde van dit kenmerk wordt gebruikt als de meervoudige Auth Server-gebruikersnaam.  Een tweede kenmerk kan worden opgegeven als een back-up naar de eerste.  Het tweede kenmerk wordt alleen gebruikt als het eerste kenmerk geen een waarde voor de gebruiker bevat.  De standaardinstellingen zijn userPrincipalName en sAMAccountName. |
| Voornaam | Voer de naam van het kenmerk van het kenmerk de voornaam in de record van een gebruiker bevat.  De standaardinstelling is givenName. |
| Achternaam | Voer de naam van het kenmerk van het kenmerk met de naam van de laatste in de record van een gebruiker.  De standaardinstelling is sn. |
| E-mailadres | Voer de naam van het kenmerk van het kenmerk met het e-mailadres in een gebruikersrecord voor de.  E-mailadres wordt gebruikt om te verzenden Welkom en e-mailberichten aan de gebruiker.  De standaardinstelling is afdruk. |
| Gebruikersgroep | Voer de naam van het kenmerk van het kenmerk met de gebruikersgroep in een gebruikersrecord voor de.  Gebruikersgroep kan worden gebruikt om te bepalen of gebruikers in de agent en rapporten in de beheerportal meervoudige Auth Server. |
| Beschrijving | Voer de naam van het kenmerk van het kenmerk met de beschrijving in een gebruikersrecord voor de.  Beschrijving wordt alleen gebruikt voor het zoeken.  De standaardinstelling is beschrijving. |
| Voicemail bellen taal | Voer de naam van het kenmerk van het kenmerk de korte naam van de taal die u bevat wilt gebruiken voor gesprekken voor de gebruiker. |
| De taal van de SMS-tekst | Voer de naam van het kenmerk van het kenmerk de korte naam van de taal die u bevat wilt gebruiken voor SMS-berichten voor de gebruiker. |
| Telefoon app-taal | Voer de naam van het kenmerk van het kenmerk de korte naam van de taal die u bevat wilt gebruiken voor de telefoon app SMS-berichten voor de gebruiker. |
| EDE token taal | Voer de naam van het kenmerk van het kenmerk de korte naam van de taal die u bevat wilt gebruiken voor EED token SMS-berichten voor de gebruiker. |
| Telefoons | De volgende kenmerken worden gebruikt om te importeren of telefoonnummers van gebruikers te synchroniseren.  Als de kenmerknaam van een is opgegeven voor telefoon, het telefoontype is alleen beschikbaar wanneer importeert uit Active Directory of synchronisatie items toe te voegen. |
| Bedrijven | Voer de naam van het kenmerk van het kenmerk waarin het zakelijke telefoonnummer in een gebruikersrecord voor de.  De standaardinstelling is telephoneNumber. |
| Start | Voer de naam van het kenmerk van het kenmerk het nummer van de telefoon thuis in een gebruikersrecord voor de bevat.  De standaardinstelling is TelefoonPrivé. |
| Pager hebben | Voer de naam van het kenmerk van het kenmerk het nummer pager hebben in de record van een gebruiker bevat.  De standaardinstelling is pager hebben. |
| Mobile | Voer de naam van het kenmerk van het kenmerk met het mobiele telefoonnummer in een gebruikersrecord voor de.  De standaardinstelling is mobiele. |
| Faxvoorblad | Voer de naam van het kenmerk van het kenmerk het faxnummer in een gebruikersrecord voor de bevat.  De standaardinstelling is facsimileTelephoneNumber. |
| IP-telefoon | Voer de naam van het kenmerk van het kenmerk met het IP-telefoonnummer in een gebruikersrecord voor de.  De standaardinstelling is ipPhone. |
| Aangepaste | Voer de naam van het kenmerk van het kenmerk met een aangepaste telefoonnummer in |
|  | een gebruikersrecord.  De standaardinstelling is leeg. |
| Extensie | Voer de naam van het kenmerk van het kenmerk met het toestelnummer in een gebruikersrecord voor de.  De waarde van het veld extensie wordt gebruikt als de extensie voor alleen het telefoonnummer.  De standaardinstelling is leeg. <br><br>Opmerking: Als het kenmerk extensie niet is opgegeven, extensies kunnen worden opgenomen als onderdeel van het telefoon-kenmerk.  De extensie moet worden voorafgegaan door een 'x', zodat deze kan worden geparseerd.  Bijvoorbeeld resultaat 555-123-4567 x890 555-123-4567 als het telefoonnummer en 890 als de extensie. |
| Knop Standaardinstellingen herstellen | Klik op de knop Beginwaarden om terug te keren alle kenmerken terug naar de standaardwaarde.  De standaardinstellingen werkt zonder problemen met de normale Active Directory of ADAM schema. |

Als u wilt bewerken kenmerken, klik op de knop bewerken op het tabblad kenmerken.  U krijgt een windows die u kunt de kenmerken bewerken.

![Bewerken van kenmerken](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronisatie
Synchronisatie blijft de meervoudige Azure-database gesynchroniseerd met de gebruikers in Active Directory of een andere Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol-map.  Het proces is vergelijkbaar met het importeren van gebruikers handmatig uit Active Directory, maar regelmatig worden opgevraagd voor Active Directory-gebruiker en wijzigingen in de groep beveiliging te verwerken.  Het biedt ook voor het uitschakelen of verwijderen van gebruikers verwijderen uit een groep container of beveiligingsgroepen en verwijderen van gebruikers verwijderd uit Active Directory.

De meervoudige Auth ADSync-service is een Windows-service waarmee de periodieke peiling van Active Directory.  Dit is niet te verwarren met Azure AD-synchronisatie of Azure AD Connect.  de meervoudige Auth ADSync, hoort Hoewel gebaseerd op een soortgelijke code base, bij de Server Azure meervoudige verificatie.  Deze status gestopt is geïnstalleerd en is gestart door de meervoudige Auth Server-service wanneer geconfigureerd om uit te voeren.  Als u een meerdere serverconfiguratie meervoudige Auth Server hebt, kan de meervoudige Auth ADSync alleen worden uitgevoerd op één server.

De service meervoudige Auth ADSync gebruikt de DirSync LDAP-server-extensie geleverd door Microsoft worden efficiënt gecontroleerd op wijzigingen.  Deze DirSync besturingselement beller "directory wijzigingen ophalen" rechts hebben en DS-replicatie-Get-wijzigingen uitgebreide toegangsbeheer rechts.  Deze rechten zijn standaard toegewezen aan de beheerder en lokaal systeem accounts op domeincontrollers.  De meervoudige Auth AdSync-service is geconfigureerd om uit te voeren als lokaal systeem al dan niet standaard.  Daarom eenvoudigste voor het uitvoeren van de service op een domeincontroller.  De service kunt uitvoeren als een account met lagere machtigingen als u configureert u deze zo altijd een volledige synchronisatie uitvoeren.  Dit is minder efficiënt, maar de accountbevoegdheden van minder vereist.

Als geconfigureerd voor gebruik LDAP en de LDAP-diagram ondersteunt het besturingselement DirSync, klikt u vervolgens polling voor gebruiker en wijzigingen in de groep beveiliging wordt werken net zoals met Active Directory.  Als de LDAP-diagram biedt geen ondersteuning voor het besturingselement DirSync, wordt een volledige synchronisatie tijdens elke cyclus worden uitgevoerd.

![Synchronisatie](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Gebruik de onderstaande tabel voor meer informatie over elk van de instellingen op het tabblad synchronisatie.

| Functie | Beschrijving |
| ------- | ----------- |
| Synchronisatie met Active Directory inschakelen | Wanneer ingeschakeld, wordt de meervoudige Auth Server-service worden geregeld gecontroleerd Active Directory op wijzigingen worden gestart. <br><br>Opmerking: ten minste één Item van de synchronisatie moet worden toegevoegd en een nu synchroniseren moet worden uitgevoerd voordat de meervoudige Auth Server-service wordt gestart met het verwerken van wijzigingen. |
| Synchroniseren elke | Geef het tijdsinterval dat de meervoudige Auth Server-service wordt gewacht tussen polling en wijzigingen verwerken. <br><br> Opmerking: Het interval dat is opgegeven, is de tijd tussen het begin van elke cyclus.  Als de tijd die wijzigingen verwerken groter is dan het interval, wordt de service direct opnieuw controleren. |
| Niet meer gebruikers verwijderen in Active Directory | Wanneer ingeschakeld, wordt dit door de meervoudige Auth Server-service wordt verwerkt Active Directory verwijderde gebruiker tombstones en de gerelateerde meervoudige Auth Server-gebruiker verwijderen. |
| Altijd een volledige synchronisatie uitvoeren | Wanneer ingeschakeld, de meervoudige Auth Server-service altijd een volledige synchronisatie wordt uitgevoerd.  Wanneer niet inschakelt, wordt de meervoudige Auth Server-service een incrementele synchronisatie uitvoeren door op te vragen alleen gebruikers die zijn gewijzigd.  De standaardinstelling is uitgeschakeld. <br><br> Opmerking: Als deze optie is uitgeschakeld, een incrementele synchronisatie kan alleen worden uitgevoerd wanneer de map het besturingselement DirSync ondersteunt en de account die wordt gebruikt om verbinding met de map de juiste machtigingen heeft voor het uitvoeren van DirSync incrementele query's.  Als het account beschikt niet over de juiste machtigingen of meerdere domeinen bij de synchronisatie betrokken, voert u een volledige synchronisatie wordt aanbevolen. |
| Vereisen van goedkeuring van de beheerder als u meer dan X gebruikers zijn uitgeschakeld of verwijderd | Synchronisatie van items kunnen worden geconfigureerd als u wilt uitschakelen of verwijderen van gebruikers die zich niet langer een lid van de container of de beveiligingsgroep van het item.  Als beveiliging is goedkeuring beheerder vereist als het aantal gebruikers wilt uitschakelen of verwijderen van een drempelwaarde overschrijdt.  Wanneer ingeschakeld, is goedkeuring is vereist voor de opgegeven drempel.  De standaardwaarde is 5 en het bereik is 1 tot en met 999. <br><br> Goedkeuring wordt mogelijk gemaakt door het eerste verzenden per e-mail een melding voor beheerders. De e-mailmelding kunt instructies voor het controleren en goedkeuren van het uitschakelen en verwijderen van gebruikers.  Als de gebruikersinterface meervoudige Auth Server wordt gestart, worden de volgende voor goedkeuring. |

De knop **Nu synchroniseren** , kunt u een volledige synchronisatie voor de synchronisatie-items die is opgegeven.  Een volledige synchronisatie is vereist wanneer synchronisatie items zijn toegevoegd, gewijzigd, verwijderd of nabesteld.  Het is ook vereist voordat de service meervoudige Auth AdSync operationele omdat het beginpunt van waaruit de service wordt gecontroleerd op incrementele wijzigingen wordt ingesteld.  Als wijzigingen zijn aangebracht in items voor synchronisatie en een volledige synchronisatie niet is uitgevoerd, wordt u gevraagd te nu synchroniseren wanneer u navigeert door naar een andere sectie of bij het sluiten van de gebruikersinterface.

De knop **verwijderen** kan de beheerder een of meer synchronisatie-items verwijderen uit de lijst met meervoudige Auth Server synchronisatie items.

>[AZURE.WARNING]Nadat u een record met de synchronisatie is verwijderd, kan niet worden hersteld. U moet de record met de synchronisatie opnieuw toe te voegen als u deze per ongeluk hebt verwijderd.

De synchronisatie-item of de synchronisatie-items zijn verwijderd uit meervoudige Auth Server.  De synchronisatie-items worden niet meer worden verwerkt door de meervoudige Auth Server-service.

De knoppen omhoog en omlaag kunnen de beheerder om de volgorde van de synchronisatie-items te wijzigen.  De volgorde is belangrijk aangezien dezelfde gebruiker mogelijk een lid van meer dan één synchronisatie item (zoals een container en een beveiligingsgroep).  De instellingen die zijn toegepast op de gebruiker tijdens de synchronisatie wordt afkomstig zijn uit het eerste item in de synchronisatie in de lijst die gekoppeld aan de gebruiker is.  Daarom kan de synchronisatie-items moeten worden geplaatst in volgorde van prioriteit.

>[AZURE.TIP]Een volledige synchronisatie moet worden verricht na het verwijderen van items voor synchronisatie.  Een volledige synchronisatie moet worden verricht na het bestellen van synchronisatie.  Klik op de knop Nu synchroniseren om een volledige synchronisatie.

## <a name="multi-factor-auth-servers"></a>Meervoudige verificatie-Servers
Extra meervoudige Auth Servers kan worden ingesteld moet fungeren als een back-RADIUS-proxy, LDAP-proxy, of voor IIS-verificatie. De synchronisatie-configuratie worden gedeeld door alle de agenten. Slechts één van deze agenten hebben echter voor het uitvoeren van de service meervoudige Auth Server. Dit tabblad kunt u de meervoudige Auth-Server die moet worden ingeschakeld voor synchronisatie te selecteren.

![Meerdere Factor Auth servers](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
