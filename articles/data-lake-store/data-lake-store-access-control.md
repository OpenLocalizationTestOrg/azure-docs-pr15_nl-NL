<properties
   pageTitle="Overzicht van toegangsbeheer in Lake gegevensopslag | Microsoft Azure"
   description="Meer informatie over hoe toegangsbeheer in Azure Lake gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Toegangsbeheer in Azure Lake gegevensopslag

Gegevensopslag Lake implementeert een model voor toegangsbeheer dat is afgeleid uit HDFS en op zijn beurt uit het model voor toegangsbeheer POSIX. In dit artikel bevat een overzicht van de basisbeginselen van het model voor toegangsbeheer voor gegevensopslag Lake. Meer informatie over de HDFS toegangsbeheer Zie model [HDFS machtigingen Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Toegangsbeheerlijsten voor bestanden en mappen

Er zijn twee soorten toegang toegangsbeheerlijsten (ACL's) - **Access ACL's** en **Standaard ACL's**.

* **Access ACL's** – ActiveX-besturingselementen toegang tot een object. Bestanden en mappen hebt toegang ACL's.

* **Standaard ACL's** – een 'sjabloon' van ACL's die zijn gekoppeld aan een map die de Access-ACL's voor alle onderliggende items gemaakt onder die map bepalen. Bestanden hebben geen standaard ACL's.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Zowel Access ACL's als standaard ACL's hebben dezelfde structuur.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Wijzigen van de ACL standaard in een bovenliggend item heeft geen invloed op de ACL Access of standaard ACL van de onderliggende items die al aanwezig.

## <a name="users-and-identities"></a>Gebruikers en identiteiten

Elke bestanden en mappen heeft unieke machtigingen voor deze identiteiten:

* De gebruiker die eigenaar van het bestand
* De eigenaar groep
* Benoemde gebruikers
* Benoemde groepen
* Alle andere gebruikers

De identiteit van gebruikers en groepen worden Azure Active Directory (AAD) identiteiten dus tenzij anders vermeld een 'gebruiker', in de context van gegevensopslag Lake, kan een betekenen een AAD-gebruiker of een AAD-beveiligingsgroep.

## <a name="permissions"></a>Machtigingen

De machtigingen voor een bestandssysteemobject zijn **lezen**, **schrijven**, en **uitvoeren** en ze kunnen worden gebruikt voor bestanden en mappen, zoals wordt weergegeven in de onderstaande tabel.

|            |    Bestand     |   Map |
|------------|-------------|----------|
| **Read (R)** | De inhoud van een bestand kunt lezen | Vereist **lezen** en **uitvoeren** voor een overzicht van de inhoud van de map.|
| **Schrijven (S)** | Kan schrijven of toevoegen aan een bestand | Vereist **schrijven en uitvoeren** voor het maken van de onderliggende items in een map. |
| **Uitvoeren (X)** | Niet betekent niets in de context van Lake gegevensopslag | Vereist om de onderliggende items van een map bladeren. |

### <a name="short-forms-for-permissions"></a>Korte formulieren voor machtigingen

**RWX**wordt gebruikt om aan te geven **lezen + schrijven + uitvoeren**. Een formulier voor meer verkorte numerieke bestaat waarin **gelezen = 4**, **schrijven = 2**, en **Execute = 1** en hun som Hiermee geeft u de machtigingen. Hieronder volgen enkele voorbeelden.

| Numerieke formulier | Korte formulier |      Wat betekent dit?     |
|--------------|------------|------------------------|
| 7            | RWX        | Lezen + schrijven + uitvoeren |
| 5            | R-X        | Lezen + uitvoeren         |
| 4            | R--        | Lezen                   |
| 0            | ---        | Geen machtigingen         |


### <a name="permissions-do-not-inherit"></a>Machtigingen overnemen niet

Machtigingen voor een item worden opgeslagen in het model POSIX-stijl is gebruikt door Lake gegevensopslag, klik op het item zelf. Machtigingen voor een item kunnen niet met andere woorden, worden overgenomen van de bovenliggende items.

## <a name="common-scenarios-related-to-permissions"></a>Gebruikelijke scenario's die zijn gerelateerd aan machtigingen

Hier volgen enkele veelvoorkomende scenario's voor meer informatie over welke machtigingen vereist zijn om bepaalde acties op een account voor gegevensopslag Lake uitvoert.

### <a name="permissions-needed-to-read-a-file"></a>Machtigingen vereist om een bestand te lezen

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Voor het bestand om te worden gelezen - moet de beller **Leesmachtigingen**
* Voor alle mappen in de mapstructuur van de waarin het bestand - moet de beller machtiging **uitvoeren**

### <a name="permissions-needed-to-append-to-a-file"></a>Machtigingen vereist toevoegen aan een bestand

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Voor het bestand moet worden toegevoegd aan - moet de beller **schrijfmachtigingen**
* Voor alle mappen waarin het bestand - moet de beller machtigingen voor **uitvoeren**

### <a name="permissions-needed-to-delete-a-file"></a>Machtigingen vereist voor het verwijderen van een bestand

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Voor de bovenliggende map - moet de beller **schrijven + uitvoeren** machtigingen
* Voor alle andere mappen in het bestandspad - moet de beller machtiging **uitvoeren**

>[AZURE.NOTE] Schrijf machtigingen voor het bestand is niet verplicht voor het verwijderen van het bestand zo lang maken als de bovenstaande twee voorwaarden voldaan wordt.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Machtigingen vereist voor een map opsommen

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Voor de map opsommen - moet de beller machtigingen van **lezen + uitvoeren**
* Voor alle bovenliggende mappen - moet de beller machtiging **uitvoeren**

## <a name="viewing-permissions-in-the-azure-portal"></a>Machtigingen weergeven in de portal van Azure

Uit van de account Lake gegevensopslag **Data Explorer** blade, klikt u op **toegang** als u wilt zien van de ACL's voor een bestand of map. In de onderstaande schermafbeelding, klikt u op toegang als u wilt zien van de ACL's voor de map **catalogus** onder het **mydatastore** -account.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Klik op **Eenvoudige weergave** voor een overzicht van de eenvoudiger weergave van het blad **Access** daarna.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klik op **Geavanceerde weergave** om de meer geavanceerde weergave te zien.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>De supergebruiker

Een super gebruiker heeft de meeste rechten van alle gebruikers in de Lake gegevensopslag. Een supergebruiker:

* RWX machtigingen heeft tot **alle** bestanden en mappen
* kan de machtigingen voor een bestand of map wijzigen.
* de eigenaar gebruiker of de eigenaar groep van een bestand of map is kunt wijzigen.

In Azure wordt aangegeven heeft een account voor gegevensopslag Lake verschillende Azure rollen:

* Eigenaren
* Inzenders
* Lezers
* Enzovoort.

Iedereen in de rol **eigenaren** voor een account voor gegevensopslag Lake wordt automatisch een controle gebruiker voor dat account. Meer informatie over Azure rol op basis van Access besturingselement RBAC () Zie [Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md)meer informatie.

## <a name="the-owning-user"></a>De eigenaar gebruiker

De gebruiker die het item heeft gemaakt, wordt automatisch de gebruiker die eigenaar van het item. Een eigenaar gebruiker kunt doen:

* De machtigingen van een bestand dat is eigendom wijzigen
* Wijzig de eigenaar groep van een bestand dat eigendom is, zo lang maken als de gebruiker die eigenaar ook een lid van de doelgroep is.

>[AZURE.NOTE] De eigenaar gebruiker **kan het niet** wijzigen de eigenaar gebruikers van een ander eigendom bestand. Alleen controle gebruikers kunnen de eigenaar gebruiker van een bestand of map wijzigen.

## <a name="the-owning-group"></a>De eigenaar groep

Elke gebruiker is de POSIX ACL's, gekoppeld aan een 'primaire groep". Gebruiker "Lisa" kan bijvoorbeeld behoren tot de groep "Financiën". Lisa mogelijk deel uitmaakt van meerdere groepen, maar één groep is altijd aangewezen als haar primaire groep. In POSIX, dat wanneer Lisa Hiermee maakt u een bestand, de groep eigenaar van het bestand is ingesteld op haar primaire groep, die in dit geval is "Financiën".
 
Als u een nieuw bestandssysteem item is gemaakt, wordt een waarde in Lake gegevensopslag toegewezen aan de eigenaar groep. 

* **Voorbeeld 1** : de hoofdmap "/". Deze map wordt gemaakt wanneer u een account voor gegevensopslag Lake is gemaakt. In dit geval is de eigenaar groep ingesteld op de gebruiker van wie de account hebt gemaakt.
* **Voorbeeld 2** (alle andere gevallen) - wanneer een nieuw item is gemaakt, de eigenaar groep wordt gekopieerd van de bovenliggende map.

De eigenaar groep kan worden gewijzigd door:
* Controle gebruikers
* De eigenaar gebruiker, als de gebruiker die eigenaar ook een lid van de doelgroep is.

## <a name="access-check-algorithm"></a>Selectievakje algoritme van Access

De volgende afbeelding vertegenwoordigt de algoritme van het selectievakje toegang voor gegevensopslag Lake-accounts.

![Gegevensopslag Lake ACL's algoritme](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Het masker en 'de effectieve machtigingen'

Het **invoermasker** is een RWX-waarde die wordt gebruikt voor het beperken van toegang voor **gebruikers met de naam**, de **eigenaar van de groep**en **benoemde groepen** bij het uitvoeren van de algoritme van de Access controleren. Hier vindt u de basisprincipes voor het masker. 

* Het masker maakt 'de effectieve machtigingen', dat wil zeggen, wijzigt de machtigingen op het moment van Access controleren.
* Het masker kan rechtstreeks worden bewerkt door de bestandseigenaar van het en eventuele controle gebruikers.
* Het masker heeft de mogelijkheid om machtigingen te maken van de effectieve machtigingen te verwijderen. Machtigingen voor het invoermasker **, kunnen niet** worden toevoegen naar de effectieve machtigingen. 

Laat ons ziet u enkele voorbeelden. Hieronder is het masker ingesteld op **RWX**, wat betekent dat het masker verwijdert niet alle machtigingen. Zoals u ziet dat de effectieve machtigingen voor benoemde gebruiker, groep eigenaar en benoemde groep niet worden gewijzigd bij de controle van access.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

Klik in het onderstaande voorbeeld is het masker ingesteld op **R-X**. Ja, deze **worden uitgeschakeld schrijftoegang** voor **naam van de gebruiker**, **groep eigenaar**en **groep** op het moment van access controleren.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Voor verwijzing, moet u hier is waar het masker voor een bestand of map wordt weergegeven in de Portal Azure.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] Het masker voor de ACL Access en standaard ACL van de hoofdmap ("/") zijn voor een nieuw account voor gegevensopslag Lake, standaard ingesteld op RWX.

## <a name="permissions-on-new-files-and-folders"></a>Machtigingen voor nieuwe bestanden en mappen

Wanneer u een nieuw bestand of map onder een bestaande map maakt, bepaalt de standaard ACL in de bovenliggende map:

* De standaard ACL en ACL Access een onderliggende-map
* Een bestand van de onderliggende Access ACL (bestanden hebben geen een standaard-ACL)

### <a name="a-child-file-or-folders-access-acl"></a>Een onderliggend-bestand of map van Access ACL

Als een onderliggende bestand of map is gemaakt, gekopieerd van de bovenliggende standaard ACL als het onderliggende-bestand of map van Access ACL. Ook als **andere** gebruiker RWX machtigingen in van de bovenliggende standaard ACL heeft, wordt deze volledig verwijderd uit het onderliggend item van Access ACL.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

De bovenstaande gegevens zijn in de meeste gevallen hoeft u moet weten over hoe een onderliggend item van Access ACL wordt bepaald. Als u bekend met POSIX-systemen bent en wilt u uitgebreidere begrijpen hoe deze transformatie wordt bereikt, raadpleegt u echter de sectie [Umask van rol bij het maken van de Access-ACL voor nieuwe bestanden en mappen](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) verderop in dit artikel.
 

### <a name="a-child-folders-default-acl"></a>Een onderliggende-map standaard ACL

Wanneer u een onderliggende map maakt onder een bovenliggende map, gekopieerd van de bovenliggende map standaard ACL om, als dat zo, aan van de onderliggende map standaard ACL is.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Geavanceerde onderwerpen voor informatie over ACL's in Lake gegevensopslag

Hier volgen een aantal geavanceerde onderwerpen voor hulp bij het begrijpen hoe ACL's voor gegevensopslag Lake bestanden of mappen worden bepaald.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>De rol van Umask bij het maken van de Access-ACL voor nieuwe bestanden en mappen

In een POSIX-compatibele systeem is het algemene concept dat die umask is een 9-bits-waarde van de bovenliggende map gebruikt om te transformeren van de machtiging voor **eigendom van gebruiker**, **groep eigenaar**en **andere** op een nieuwe onderliggende-bestand of map van Access ACL. De bits van een umask bepalen welke bits uitschakelen in het onderliggend item van Access ACL. Het is dus gebruikte om selectief te voorkomen dat het doorgeven van machtigingen voor het eigendom van gebruiker, groep eigenaar en andere.
  
In een HDFS-systeem is de umask meestal een hele site configuratieoptie dat wordt beheerd door beheerders. Gegevensopslag Lake maakt gebruik van een **account hele umask** die niet worden gewijzigd. De volgende tabel ziet u Lake gegevensopslag van umask.

| Gebruikersgroep  | Instelling | Invloed op nieuw onderliggend item van Access ACL |
|------------ |---------|---------------------------------------|
| Eigendom van gebruiker | ---     | Geen effect                             |
| Eigenaar van de groep| ---     | Geen effect                             |
| Andere       | RWX     | Verwijderen lezen + schrijven + uitvoeren         | 

De volgende afbeelding ziet deze umask in actie. Het uiteindelijke resultaat is om te **lezen + schrijven + uitvoeren** voor de **andere** gebruiker verwijderen. Aangezien de umask geen bits voor de **eigenaar van de gebruiker** en **eigenaar van de groep**hebt opgegeven, worden deze machtigingen niet getransformeerd.

![Gegevensopslag Lake ACL 's](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>De Plaktoetsen bits

De Plaktoetsen bit is een geavanceerdere functie van een bestandssysteem POSIX. In de context van Lake gegevensopslag is het niet waarschijnlijk dat de Plaktoetsen bits nodig zijn.

De volgende tabel ziet de werking van de Plaktoetsen bit in Lake gegevensopslag.

| Gebruikersgroep         | Bestand    | Map |
|--------------------|---------|-------------------------|
| Plaktoetsen bits **uit** | Geen effect   | Geen effect           |
| Plaktoetsen bits **aan**  | Geen effect   | Hiermee voorkomt u dat iedereen behalve **controle gebruikers** en de **gebruiker die eigenaar is** van een onderliggend item de naam van dat item onderliggende wijzigen of verwijderen.               |

De Plaktoetsen bit wordt niet weergegeven in de Portal Azure.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Veelgestelde vragen voor ACL's in Lake gegevensopslag

Hier vindt u enkele vragen die vaak met betrekking tot ACL's in Lake gegevensopslag voordoen.

### <a name="do-i-have-to-enable-support-for-acls"></a>Heb ik ondersteuning voor ACL's inschakelen?

Nee. Toegangsbeheer via ACL's is altijd ingeschakeld voor een account voor gegevensopslag Lake.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Welke machtigingen zijn vereist om te recursief het verwijderen van een map en de inhoud ervan?

* De bovenliggende map moet **schrijven + uitvoeren**.
* De map zijn verwijderd, en elke erin, is vereist **lezen + schrijven + uitvoeren**.
>[AZURE.NOTE] De bestanden in mappen verwijderen is, wordt schrijven niet aan deze bestanden vereist. Bovendien de hoofdmap "/" **nooit** kan worden verwijderd.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Wie is ingesteld als de eigenaar van een bestand of map?

De maker van een bestand of map wordt de eigenaar.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Wie is ingesteld als de eigenaar groep van een bestand of map bij maken?

Dit hebt gekopieerd uit de eigenaar van de bovenliggende map waaronder de nieuwe bestand of map is gemaakt.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ik ben de eigenaar gebruiker van een bestand, maar ik heb ik nodig heb RWX machtigingen. Wat moet ik doen?

De eigenaar gebruiker kunt gewoon wijzigen voor de machtigingen van het bestand om te machtigen zelf een RWX die zij nodig hebben.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Ondersteunt Lake gegevensopslag overname van ACL's?

Nee.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Wat is het verschil tussen invoermasker en umask?

| Invoermasker | umask|
|------|------|
| De eigenschap **invoermasker** is beschikbaar op elke bestanden en mappen. | De **umask** is een eigenschap van het account voor gegevensopslag Lake. Er is dus alleen een enkele umask in de Lake gegevensopslag.    |
| De eigenschap Invoermasker van een bestand of map kan worden gewijzigd door de eigenaar gebruiker of de eigenaar van de groep van een bestand of een controle gebruiker. | De eigenschap umask kan niet worden gewijzigd door een gebruiker, zelfs een supergebruiker. Dit is een waarde niet te wijzigen, constante.|
| De eigenschap Invoermasker wordt gebruikt om tijdens het controleren van Access algoritme gedurende runtime om te bepalen of een gebruiker het recht om uit te voeren op bewerking op een bestand of map heeft. De rol van het masker is het opzetten van 'de effectieve machtigingen' op het moment van access controleren. | De umask wordt niet helemaal tijdens het controleren van Access gebruikt. De umask wordt gebruikt om te bepalen de Access-ACL van nieuwe onderliggende items van een map. |
| Het masker is de waarde van een 3-bits-RWX die van toepassing op benoemde gebruiker, groep naam en eigenaar gebruiker op het moment van access controleren.| De umask is een 9 bits waarde die is van toepassing op de eigenaar gebruiker, groep eigenaar en andere van een nieuwe onderliggende.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Waar vind ik meer informatie over het model voor toegangsbeheer POSIX?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [HDFS machtiging handleiding](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [VEELGESTELDE VRAGEN OVER POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL op Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [ACL met toegangsbeheerlijsten op Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Zie ook

* [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md)

* [Aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





