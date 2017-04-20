<properties
   pageTitle="Overzicht van beveiliging in Lake gegevensopslag | Microsoft Azure"
   description="Duidelijk hoe Azure Lake gegevensopslag een veiliger groot gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Beveiliging in Azure Lake gegevensopslag

Veel ondernemingen profiteert van grote gegevens analytics voor bedrijven inzichten te helpen slimme beslissingen te kunnen komen. Een organisatie kan een complexe en geregeld-omgeving, met een toeneemt aantal verschillende gebruikers hebben. Het is belangrijk is voor een onderneming om ervoor te zorgen dat kritieke zakelijke gegevens met de juiste toegangsniveau verleend voor afzonderlijke gebruikers veiliger, is opgeslagen. Azure Lake gegevensopslag is bedoeld om te voldoen aan deze veiligheidsvereisten. In dit artikel wordt meer informatie over de beveiligingsmogelijkheden van gegevensopslag Lake, waaronder:

* Verificatie
* Autorisatie
* Netwerk moeten worden geïsoleerd
* Gegevensbescherming
* Controle

## <a name="authentication-and-identity-management"></a>Verificatie en identiteit beheren

Verificatie is het proces waarmee de identiteit van een gebruiker is geverifieerd wanneer de gebruiker communiceert met Lake gegevensopslag of met elke service die is verbonden met Lake gegevensopslag. Voor identiteitsbeheer en verificatie gebruik Lake gegevensopslag van [Azure Active Directory](../active-directory/active-directory-whatis.md), een volledig identiteit en access cloud beheeroplossing waardoor het eenvoudiger het beheer van gebruikers en groepen wordt.

Elk Azure abonnement kan worden gekoppeld aan een exemplaar van Azure Active Directory. Alleen gebruikers en service-identiteiten die zijn gedefinieerd in uw Azure Active Directory-service hebben toegang tot uw account voor gegevensopslag Lake, met behulp van de Azure portal opdrachtregel hulpmiddelen, of tot en met clienttoepassingen uw organisatie genereert met behulp van de Azure Data Lake Store SDK. Belangrijkste voordelen van het gebruik van Azure Active Directory als gecentraliseerde access besturingselement om zijn:

* Vereenvoudigd beheer van productlevenscyclus identiteit. De identiteit van een gebruiker of een service (een service principal identiteit) worden snel gemaakt en snel door gewoon verwijderen of uitschakelen van het account weergegeven in de map is ingetrokken.

* Meervoudige verificatie. [Meervoudige verificatie](../multi-factor-authentication/multi-factor-authentication.md) biedt een extra beveiliging voor gebruiker aanmeldingen en transacties.

* Verificatie van een client via een standaard geopend protocol, zoals OAuth of OpenID.

* Federatie met adreslijstservices enterprise en cloud-identiteitsprovider.

## <a name="authorization-and-access-control"></a>Verificatie en toegangsbeheer

Nadat een gebruiker wordt geverifieerd door Azure Active Directory zodat de gebruiker toegang heeft tot Azure Lake gegevensopslag, toegangsmachtigingen autorisatie besturingselementen voor gegevensopslag Lake. Gegevensopslag Lake worden gescheiden door autorisatie voor account gerelateerd en gegevens activiteiten op de volgende wijze:

* [Toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) Azure (RBAC) gekregen om uw account te beheren
* POSIX ACL voor toegang tot gegevens in de winkel


### <a name="rbac-for-account-management"></a>RBAC om uw account te beheren

Vier eenvoudige rollen zijn gedefinieerd voor gegevensopslag Lake al dan niet standaard. De rollen vergunningsaanvragen u kunt verschillende bewerkingen op een account voor gegevensopslag Lake via de portal van Azure, PowerShell-cmdlets en REST API's. De rollen eigenaar en Inzender kunnen uitvoeren van tal van functies voor beheer op het account. U kunt de rol van de lezer toewijzen aan gebruikers die alleen met gegevens werken.

![RBAC rollen] (./media/data-lake-store-security-overview/rbac-roles.png "RBAC rollen")

Bedenk dat hoewel rollen voor accountbeheer zijn toegewezen, sommige rollen toegang tot gegevens beïnvloeden. U moet gebruiken ACL's toegang tot de bewerkingen die een gebruiker in het bestandssysteem uitvoeren kan te beheren. De volgende tabel bevat een overzicht van de rights management en gegevens toegangsrechten voor de standaardrollen.

| Rollen                    | Rights management               | Gegevens toegangsrechten | Uitleg |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Geen rol         | Geen                            | Vallende ACL    | De gebruiker niet de Azure beheerportal of Azure PowerShell-cmdlets gebruiken om te bladeren Lake gegevensopslag. De gebruiker kan alleen de opdrachtregel's gebruiken.
| Eigenaar  | Alle  | Alle  | De rol van eigenaar is een beheerder. Deze rol alles kunt beheren en volledige toegang tot gegevens heeft.
| Lezer   | Alleen-lezen  | Vallende ACL    | De rol van de lezer kunt alles met betrekking tot accountbeheer, zoals waarin gebruiker wordt toegewezen aan welke rol weergeven. De rol van de lezer kunt u geen wijzigingen aanbrengen.   |
| Inzender              | Alles behalve rollen toevoegen en verwijderen | Vallende ACL    | De rol Inzender kan sommige aspecten van een account, zoals implementaties en bij het maken en beheren van waarschuwingen beheren. De rol Inzender kan toevoegen of verwijderen van rollen.
| Beheerder van de gebruiker toegang | Toevoegen en verwijderen van rollen            | Vallende ACL    | De gebruiker toegang-beheerdersrol kunt de toegang van gebruikers met accounts beheren. |

Zie [gebruikers of beveiligingsgroepen met Lake gegevensopslag accounts toewijzen](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts)voor instructies.

### <a name="using-acls-for-operations-on-file-systems"></a>Met behulp van ACL's voor bewerkingen op bestandssystemen

Gegevensopslag Lake is een hiërarchische bestandssysteem zoals Hadoop Distributed bestand System (HDFS), en [POSIX ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)wordt ondersteund. Hiermee kunt u lezen (r), schrijven (w) en uitvoeren (x)-machtigingen voor resources voor de rol van eigenaar, de groep eigenaren en voor andere gebruikers en groepen. In de gegevens Lake Store Public Preview (de huidige versie), worden de ACL's zijn ingeschakeld in de hoofdmap, van submappen en afzonderlijke bestanden. De ACL's die u op de hoofdmap toepast zijn ook van toepassing op alle onderliggende mappen en bestanden.

Het is raadzaam dat u ACL's voor meerdere gebruikers wilt definiëren met behulp van [beveiligingsgroepen](../active-directory/active-directory-accessmanagement-manage-groups.md). Gebruikers toevoegen aan een beveiligingsgroep en vervolgens de ACL's voor een bestand of map toewijzen aan die beveiligingsgroep. Dit is handig als u toegang verlenen aangepaste, wilt omdat u beperkt zijn tot maximaal negen items voor aangepaste toegang toevoegen. Zie [gebruikers of beveiligingsgroep als ACL's naar het bestandssysteem Azure Lake gegevensopslag toewijzen](data-lake-store-secure-data.md#filepermissions)voor meer informatie over hoe u beter beveiligde gegevens die zijn opgeslagen in Lake gegevensopslag met behulp van Azure Active Directory-beveiligingsgroepen.

![Lijst met standaardkleuren of aangepaste access] (./media/data-lake-store-security-overview/adl.acl.2.png "Lijst met standaardkleuren of aangepaste access")

## <a name="network-isolation"></a>Netwerk moeten worden geïsoleerd

Gebruik Lake gegevensopslag naar toegang tot uw gegevensopslag op het netwerkniveau van te bepalen. U kunt definiëren firewalls en een IP-adresbereiken voor uw vertrouwde clients definiëren. Met een IP-adresbereiken, worden alleen clients met een IP-adres binnen het gedefinieerde bereik kunnen verbinden Lake gegevensopslag.

![Firewallinstellingen en IP-toegang] (./media/data-lake-store-security-overview/firewall-ip-access.png "Firewallinstellingen en IP-adres")

## <a name="data-protection"></a>Gegevensbescherming

Organisaties wilt ervoor zorgen dat hun cruciale gegevens veilig overal in de levenscyclus van is. Voor gegevens tijdens overdracht Lake gegevensopslag de standaard-beveiliging TLS (Transport Layer)-protocol gebruikt om gegevens die worden verplaatst tussen een client en Lake gegevensopslag te beveiligen.

Gegevensbescherming voor gegevens in rust zijn beschikbaar in toekomstige versies.

## <a name="auditing-and-diagnostic-logs"></a>Controle en diagnostische logboeken

U kunt controleren of de diagnostische logboeken, afhankelijk van of u Logboeken voor management activiteiten of gegevens betrekking activiteiten zoekt gebruiken.

*  Activiteiten leidinggevende Azure resourcemanager-API's gebruiken en via controlelogboeken bijhouden in de portal van Azure zijn opgehaald.
*  Activiteiten gegevens betrekking WebHDFS REST API's gebruiken en in de portal van Azure via diagnostische logboeken zijn opgehaald.

### <a name="auditing-logs"></a>Controlelogboeken

Als u akkoord gaat met regelgeving, wilt mogelijk een organisatie voldoende audit-registratie dat deze moeten verdiepen in specifieke incidenten. Gegevensopslag Lake heeft ingebouwde cmdlets voor controle en controle en registreert alle account management activiteiten.

Voor account management audit-registratie, weergeven en kies de kolommen die u wilt vastleggen. U kunt ook de controlelogboeken bijhouden met Azure Storage exporteren.

![Controlelogboeken bijhouden] (./media/data-lake-store-security-overview/audit-logs.png "Controlelogboeken bijhouden")

### <a name="diagnostic-logs"></a>Diagnostische logboeken

U kunt de toegang tot gegevens audit-registratie instellen in de Azure-portal (in de diagnostische instellingen) en maak een Azure Blob storage account waar de logboeken worden opgeslagen.

![Diagnostische logboeken] (./media/data-lake-store-security-overview/diagnostic-logs.png "Diagnostische logboeken")

Nadat u diagnostische instellingen hebt geconfigureerd, kunt u de logboeken weergeven op het tabblad **Diagnostische logboeken** .

Zie voor meer informatie over het werken met diagnostische logboeken met Azure Lake gegevensopslag [Access diagnostische logboeken voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Overzicht

Zakelijke klanten vraag een cloud-platform voor gegevens analytics veilig en eenvoudig te gebruiken. Azure Lake gegevensopslag is zo ontworpen dat deze vereisten is voldaan via identiteitsbeheer en verificatie via Azure Active Directory-integratie, ACL gebaseerde verificatie, netwerk moeten worden geïsoleerd, gegevensversleuteling tijdens overdracht en op plaats (in de toekomst binnenkort)-mailadres en controle.

Als u zien van de nieuwe functies in Lake gegevensopslag wilt, stuur ons uw feedback in het [forum van gegevens Lake Store UserVoice](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Zie ook

- [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)
- [Aan de slag met Lake gegevensopslag](data-lake-store-get-started-portal.md)
- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
