<properties
    pageTitle="Verbinding maken met SQL-Database met SQL Server Management Studio in Azure RemoteApp | Microsoft Azure"
    description="Gebruik van deze zelfstudie om te leren hoe u SQL Server Management Studio in Azure RemoteApp voor beveiliging en prestaties wordt gebruikt wanneer u verbinding maakt met SQL-Database"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>SQL Server Management Studio in Azure RemoteApp met verbinding maken met SQL-Database

## <a name="introduction"></a>Inleiding  
Deze zelfstudie ziet u hoe u SQL Server Management Studio (SSMS) in Azure RemoteApp met verbinding maken met SQL-Database. Deze begeleidt u bij het proces van het instellen van SQL Server Management Studio in Azure RemoteApp, wordt uitgelegd van de voordelen en toont beveiligingsfuncties die u kunt gebruiken in Azure Active Directory.

**Geschatte tijd in beslag:** 45 minuten

## <a name="ssms-in-azure-remoteapp"></a>SSMS in Azure RemoteApp

Azure RemoteApp is een service RDS in Azure wordt aangegeven dat toepassingen levert. U vindt u meer informatie over deze hier: [Wat is RemoteApp?](../remoteapp/remoteapp-whatis.md)

SSMS uitgevoerd in Azure RemoteApp kunt u dezelfde manier als SSMS lokaal uitgevoerd.

![Schermafbeelding van SSMS uitgevoerd in Azure RemoteApp][1]



## <a name="benefits"></a>Voordelen

Er zijn vele voordelen tot het gebruik van SSMS in Azure RemoteApp, waaronder:

- Poort 1433 op Azure SQL Server heeft geen worden blootgesteld extern (buiten Azure).
- Hoeft te behouden toevoegen en verwijderen van IP-adressen in de firewall Azure SQL Server.
- Alle Azure RemoteApp verbindingen werken via HTTPS over het gebruik van poort 443 versleuteld Remote Desktop protocol
- Meerdere gebruikers is en kan schalen.
- Er is een prestatieverbetering SSMS hoeft in hetzelfde gebied, als de SQL-Database.
- Gebruik van Azure RemoteApp met de Premium-editie van Azure Active Directory die gebruiker activiteitsrapporten heeft, kunt u controleren.
- Meervoudige verificatie (MFA), kunt u ook weer inschakelen.
- Toegang SSMS ergens wanneer u een van de ondersteunde Azure RemoteApp-clients, waaronder iOS, Android, Windows Phone, Mac en Windows-PC.


## <a name="create-the-azure-remoteapp-collection"></a>Maak de verzameling Azure RemoteApp

Hier volgen de stappen voor het maken van uw siteverzameling Azure RemoteApp met SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. een nieuwe Windows-VM maken op basis van afbeelding
De afbeelding "Windows Server extern bureaublad sessie Host Windows Server 2012 R2" in de galerie gebruiken om uw nieuwe VM.


### <a name="2-install-ssms-from-sql-express"></a>2. SSMS vanuit SQL Express installeren

Ga naar de nieuwe VM en Ga naar deze downloadpagina: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Er is een optie om alleen downloaden SSMS. Na downloaden, gaat u naar de map installeren en instellen om te kunnen installeren SSMS.

U moet ook SQL Server 2014 Service Pack 1 installeren. U kunt deze hier downloaden: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 bevat essentiële functionaliteit voor het werken met Azure SQL-Database.


### <a name="3-run-validate-script-and-sysprep"></a>3. valideren script en Sysprep uitvoeren

Klik op het bureaublad van de VM heet een PowerShell-script valideren. Deze door te dubbelklikken op uitvoeren. Er wordt gecontroleerd of de VM gereed is voor worden gebruikt voor externe hosting van toepassingen. Als de verificatie is voltooid, wordt u gevraagd naar sysprep uitvoeren: Kies uit te voeren.

Als sysprep is voltooid, wordt deze de VM afgesloten.

Zie voor meer informatie over het maken van een afbeelding van de Azure RemoteApp: [het maken van een afbeelding van de sjabloon RemoteApp in Azure wordt aangegeven](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. afbeelding vastleggen

Wanneer de VM is gestopt, vinden in de huidige portal en deze vastleggen.

Zie voor meer informatie over het opnemen van een afbeelding, [een afbeelding van een Windows Azure virtuele machines die zijn gemaakt met het implementatiemodel klassieke vastleggen](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. toevoegen aan afbeeldingen van Azure RemoteApp-sitesjablonen

Ga naar het tabblad afbeeldingen van sitesjablonen in de sectie Azure RemoteApp van de huidige portal en klikt u op toevoegen. Selecteer 'Een afbeelding van uw bibliotheek virtuele Machines importeren' in het pop- en kiest u op de afbeelding die u zojuist hebt gemaakt.



### <a name="6-create-cloud-collection"></a>6. cloud-verzameling maken

Maak een nieuwe Azure RemoteApp Cloud-verzameling in de huidige-portal. Kies de Sjabloonafbeelding die u net hebt geïmporteerd met SSMS is geïnstalleerd.

![Nieuwe cloud-verzameling maken][2]


### <a name="7-publish-ssms"></a>7. SSMS publiceren

Klik op de publicatie tabblad van de nieuwe cloud-verzameling, worden select publiceren van een toepassing in het Menu Start en kiest u SSMS in de lijst.

![App publiceren][5]

### <a name="8-add-users"></a>8. gebruikers toevoegen

U kunt de gebruikers die toegang heeft tot deze Azure RemoteApp-verzameling waaronder alleen SSMS selecteren op het tabblad van de toegang van gebruikers.

![Gebruiker toevoegen][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. de Azure RemoteApp-clienttoepassing installeren

U kunt downloaden en installeren van een Azure RemoteApp-client: [downloaden | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure SQL Server configureren

De enige configuratie nodig is om ervoor te zorgen dat Azure Services is ingeschakeld voor de firewall. Als u deze oplossing gebruikt, hoeft niet u geen IP-adressen als u wilt openen, de firewall toevoegen. Het netwerkverkeer dat is toegestaan met de SQL Server is afkomstig van andere Azure-services.


![Azure toestaan][4]



## <a name="multi-factor-authentication-mfa"></a>Meervoudige verificatie MFA)

MFA kan specifiek worden ingeschakeld voor deze toepassing. Ga naar het tabblad toepassingen van de Azure Active Directory. U vindt u een vermelding voor Microsoft Azure RemoteApp. Als u van die toepassing en klik vervolgens configureren, ziet u de pagina waar u MFA voor deze toepassing inschakelen kunt.

![MFA inschakelen][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Gebruikersactiviteit met Azure Active Directory Premium controleren

Als u nog geen Azure AD Premium, hebt u deze inschakelen in de sectie licenties van de map. Met de Premium is ingeschakeld, kunt u gebruikers toewijzen aan het niveau van de Premium.

Wanneer u gaat u naar een gebruiker in uw Azure Active Directory, kunt u vervolgens terug naar het tabblad activiteit om foutieve aanmeldinformatie die naar Azure RemoteApp weer te geven.



## <a name="next-steps"></a>Volgende stappen

Na het voltooien van de bovenstaande stappen, zult u kunnen uitvoeren van de Azure RemoteApp-client en aanmelden met een toegewezen gebruiker. U krijgt SSMS als een van uw toepassingen en u het kunt uitvoeren, zoals u zou doen als deze zijn geïnstalleerd op uw computer met toegang tot Azure SQL Server.

Zie voor meer informatie over hoe u de verbinding met SQL-Database, [verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md).


Dat is alles voorlopig. Te rekenen op!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
