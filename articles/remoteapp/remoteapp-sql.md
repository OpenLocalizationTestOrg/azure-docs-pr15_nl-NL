<properties
   pageTitle="SQL Azure wordt aangegeven met Azure RemoteApp | Microsoft Azure"
   description="Leer hoe u SQL Azure wordt aangegeven met Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure wordt aangegeven met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Vaak wanneer klanten kiezen voor het hosten van hun Windows-toepassingen in de cloud met Azure RemoteApp wilt ze ook hun gegevens zoals SQL-servers migreren naar de cloud voor een hele cloud-implementatie. Hiermee hele cloud gehost-oplossing die toegankelijk is op elk gewenst moment voor elk apparaat een willekeurige plaats met Azure RemoteApp. Hieronder vindt u koppelingen en verwijzingen samen met de instructies om u te helpen met dit proces.  

## <a name="migrate-your-sql-data"></a>Migreren van uw SQL-gegevens

Begin met [een SQL Server-database met Azure SQL-Database migreren](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Azure RemoteApp configureren
Uw Windows-toepassing in Azure RemoteApp hosten. Hierna volgt een zeer hoog niveau stapsgewijze:

1.     De [sjabloon Azure RemoteApp VM](remoteapp-imageoptions.md)maken. 
2.     Installeer de vereiste toepassing op de VM.
3.     De toepassing configureren zodat deze verbinding met de SQL-DB maakt en Bevestig dat deze altijd werkt.
4.     Sysprep en afsluiten het VM. Vastleggen dit als een afbeelding voor gebruik met Azure. **Notitie:** U moet ervoor zorgen dat de toepassing kunnen de verbindingsgegevens DB tot en met proces behouden is. Als de toepassing niet kan worden bewaard de verbindingsgegevens DB, wilt u mogelijk de leverancier van de toepassing om te controleren hoe we de verbindingsreeks kunt opgeven deelnemen.
5.     De aangepaste afbeelding importeren in uw Azure RemoteApp-bibliotheek selecteren van de juiste geografische locatie die de SQL Azure-implementatie zich bevindt. 
6.     Een RemoteApp-verzameling in de dezelfde Datacenter als uw SQL Azure-implementatie met de bovenstaande sjabloon implementeert en publiceer de toepassing. Implementatie van Azure RemoteApp in het dezelfde Datacenter als uw SQL Azure wordt aangegeven kunt implementatie zorgen dat de snelste verbindingssnelheid en latentie te verkleinen. 

## <a name="app-and-sql-configuration-considerations"></a>Aandachtspunten voor de configuratie-App en SQL:
Er zijn een paar punten u rekening moet houden wanneer u SQL Azure met RemoteApp:

Informatie over [het configureren van een Azure SQL database-firewall](../sql-database/sql-database-firewall-configure.md). Een uittreksel uit de Staten artikel 'in eerste instantie alle toegang tot uw Azure SQL Database-server wordt geblokkeerd door de firewall. Om te beginnen met uw Azure SQL Database-server, moet u Ga naar de klassieke Portal en een of meer serverniveau firewallregels die u toegang tot uw Azure SQL Database-server opgeven. De firewallregels gebruiken om op te geven welke IP-adresbereiken van Internet zijn toegestaan en al dan niet Azure-toepassingen kunnen probeert te verbinden met uw Azure SQL Database-server."

Ook wanneer een computer probeert te verbinden met uw databaseserver van Internet, de firewall gecontroleerd of het oorspronkelijke IP-adres van de aanvraag ten opzichte van de volledige reeks serverniveau en (indien nodig) database niveau firewallregels. "Als het IP-adres van de aanvraag zich in een van de bereiken in de serverniveau firewallregels, de verbinding is verleend aan uw Azure SQL Database-server." Daarom maakt gebruik van het IP-bereiken gebruikt en niet alleen afzonderlijke bron IP-adressen.

Volg de stapsgewijze instructies in [hoe: firewallinstellingen configureren op SQL-Database met behulp van de Portal Azure](../sql-database/sql-database-configure-firewall-settings.md) om het IP-bereik te geven. Geef het IP-bereik van de subnetten die is opgegeven voor het verzamelen van Azure RemoteApp wanneer u de SQL-firewallregels configureert. Hierdoor moet de servers ARA verbinding maken met de SQL-DB, zelfs als ze hebt dynamisch IP-adressen toegewezen worden.

## <a name="troubleshooting"></a>Problemen oplossen
Als de ervaring van het gebruik van een clienttoepassing gehost in Azure RemoteApp die is verbonden met een SQL-database waarbij die worden gehost op Azure of on-premises implementatie is traag kan er enkele redenen waarom.  

- Netwerklatentie van uw apparaat aan Azure is hoog. Verplaatsen naar de beste en snelste netwerkverbinding u kunt voor de beste prestaties. [Azurespeed.com](http://azurespeed.com/) als een algemene hulpmiddel gebruiken om te testen van uw apparaten latentie Azure Datacenter voor.  
- Client-app gehost in Azure RemoteApp is onder belasting. Een ander Facturering abonnement zoals Premium facturering selecteren, worden de prestaties. Een andere manier is om te controleren van de bronnen voor uw toepassing verbruikt: uitvoeren tijdens een actieve sessie een toetsencombinatie ctrl-alt-end starten van het scherm SA's en selecteer Taakbeheer en het gebruik van de bronnen voor uw app toekijken.
- SQL server is onder belasting of niet worden geoptimaliseerd. Volg de SQL-instructies voor probleemoplossing. 

