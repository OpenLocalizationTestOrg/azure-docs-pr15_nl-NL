<properties
    pageTitle="Een domein behoren HDInsight clusters configureren | Microsoft Azure"
    description="Meer informatie over het instellen en configureren van een domein behoren HDInsight clusters"
    services="hdinsight"
    documentationCenter=""
    authors="saurinsh"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/26/2016"
    ms.author="saurinsh"/>

# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Een domein behoren HDInsight clusters (Preview) configureren

Informatie over het instellen van een Azure HDInsight cluster met Azure Active Directory (Azure AD) en [Apache Zwerver](http://hortonworks.com/apache/ranger/) om te profiteren van sterke verificatie en uitgebreide Rolgebaseerd-besturingselement (RBAC) beleid.  Een domein behoren HDInsight kan alleen worden geconfigureerd op Linux gebaseerde clusters. Zie [HDInsight introduceren domein behoren clusters](hdinsight-domain-joined-introduction.md)voor meer informatie.

In dit artikel is bedoeld voor de eerste zelfstudie een reeks:

- Maak een HDInsight cluster verbonden met Azure AD (via de mogelijkheid Azure-Directory Domain Services) met Apache Zwerver is ingeschakeld.
- Maak en component beleid tot en met Apache Zwerver toepassen en gebruikers toestaan (bijvoorbeeld gegevens wetenschappers) te verbinden met component met ODBC-programma's, bijvoorbeeld Excel, Tableau enzovoort. Microsoft werkt samen over het toevoegen van andere werkbelastingen, zoals HBase, elektrische en Storm, met een domein behoren HDInsight binnenkort.

Een voorbeeld van de uiteindelijke topologie ziet er als volgt uit:

![Topologie van een domein behoren HDInsight](.\media\hdinsight-domain-joined-configure\hdinsight-domain-joined-topology.png)

Omdat Azure AD momenteel biedt alleen ondersteuning voor klassieke virtuele netwerken (VNets) en HDInsight Linux gebaseerde clusters alleen ondersteuning resourcemanager Azure op basis van VNets, HDInsight Azure AD-integratie is vereist twee VNets en een peering ertussen. Zie voor de informatie vergelijking tussen de twee implementatiemodellen [Azure resourcemanager versus klassieke implementatie: meer informatie over de implementatiemodellen en de status van uw resources](../resource-manager-deployment-model.md). De twee VNets moeten zich in hetzelfde gebied, als de Azure AD DS.

Azure servicenamen moet uniek zijn. De volgende namen worden gebruikt in deze zelfstudie. Contoso is een factitief naam. Wanneer u de zelfstudie doorlopen, moet u *contoso* vervangen door een andere naam. 
    
**Namen:**

|Eigenschap|Waarde|
|--------|-----|
| Azure AD-VNet|contosoaadvnet|
| Azure AD Virtual Machine (VM)|contosoaadadmin. Deze VM wordt gebruikt voor het configureren van de organisatie-eenheid en omgekeerde DNS-zone.|
| Azure AD-adreslijst|contosoaaddirectory|
| Azure AD-domeinnaam|Contoso (contoso.onmicrosoft.com)|
| HDInsight VNet|contosohdivnet|
| Resourcegroep HDInsight VNet|contosohdirg|
| HDInsight cluster|contosohdicluster|

Deze zelfstudie Stapsgewijze instructies voor het configureren van een cluster HDInsight domein behoren. Elke sectie bevat koppelingen naar andere artikelen met meer achtergrondinformatie.

## <a name="prerequisite"></a>Let op:

- Vertrouwd raken met [Azure AD-domeinservices](https://azure.microsoft.com/services/active-directory-ds/) de structuur [prijzen](https://azure.microsoft.com/pricing/details/active-directory-ds/) .
- Ervoor zorgen dat uw abonnement whitelisted voor deze openbare preview is. U kunt doen door te sturen van een e-mailbericht naar hdipreview@microsoft.com met uw abonnement-ID.
- Een SSL-certificaat dat is ondertekend door een ondertekend instantie voor uw domein. Het certificaat is vereist voor het configureren van veilige LDAP. Zelfondertekende certificaten kunnen niet worden gebruikt.

## <a name="procedures"></a>Procedures

1. Maak een Azure klassieke VNet voor uw Azure AD.  
2. Maken en configureren van Azure AD en Azure AD DS.
3. Een VM toevoegen aan de klassieke VNet voor het maken van de organisatie-eenheid. 
4. Maak een organisatie-eenheid voor Azure AD DS.
5. Maak een HDInsight VNet in de modus van Azure resource management.
6. Omgekeerde DNS-zones voor het Azure AD DS-instelling.
6. Peer de twee VNets.
7. Maak een cluster HDInsight.

> [AZURE.NOTE] Deze zelfstudie wordt ervan uitgegaan dat u geen een Azure AD. Als u een hebt, kunt u het gedeelte in stap 2 overslaan.
    
## <a name="create-an-azure-classic-vnet"></a>Een Azure klassieke VNet maken

In dit gedeelte maakt u een klassieke VNet met behulp van de Azure portal. In de volgende sectie, kunt u de Azure AD DS inschakelen voor uw Azure AD in de klassieke VNet. Zie voor meer informatie over de volgende procedure en het gebruik van andere methoden voor het maken van VNet [maken een virtueel netwerk (klassieke) met behulp van de Azure-portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md).

**Een klassieke VNet maken**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com). 
2. Klik op **nieuwe** > **netwerkproblemen** > **Virtual Network**.
3. Selecteer **klassieke**in **een implementatiemodel selecteren**, en klik vervolgens op **maken**.
4. Typ of Selecteer de volgende waarden:

    - **Naam**: contosoaadvnet
    - **Adresruimte**: 10.1.0.0/16
    - **Subnetnaam**: Subnet1
    - **Subnet adresbereiken**: 10.1.0.0/24
    - **Abonnement**: (Selecteer een abonnement dat is gebruikt voor het maken van deze VNet.)
    - **ResourceGroup**:
    - **Locatie**: (Selecteer een gebied voor uw cluster HDInsight.)

        > [AZURE.IMPORTANT] U kunt een locatie die ondersteuning biedt voor Azure AD DS moet kiezen. Zie voor meer informatie [producten beschikbaar per regio](https://azure.microsoft.com/en-us/regions/services/). 
        >
        > De klassieke VNet zowel de Resource groep VNet moeten zich in hetzelfde gebied, als de Azure AD DS.

5. Klik op **maken** om te maken van de VNet.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Maken en configureren van Azure AD DS voor uw Azure AD

In deze sectie, zal u het volgende doen:

1. Maak een Azure AD.
2. Maak Azure AD-gebruikers. Deze gebruikers zijn Domeingebruikers. U kunt de eerste gebruiker gebruiken voor het configureren van het cluster HDInsight met de Azure AD.  De twee andere gebruikers zijn optioneel voor deze zelfstudie. Deze wordt gebruikt in [configureren component beleidsregels voor een domein behoren HDInsight clusters](hdinsight-domain-joined-run-hive.md) wanneer u Apache Zwerver beleidsregels configureren.
3. De groep beheerders van de domeincontroller AAD hebt gemaakt en de Azure AD-gebruiker toevoegen aan de groep. Deze gebruiker u maakt de organisatie-eenheid.
4. Azure AD-Domain Services (Azure AD DS) voor de Azure AD inschakelen.
7. Leesbaar TEKSTFORMAAT configureren voor de Azure AD. De Lightweight Directory Access Protocol (LDAP) wordt gebruikt om te lezen en schrijven naar Azure AD.

Als u liever een bestaande Azure AD gebruikt, kunt u stap 1 en 2 overslaan.

**Een Azure AD maken**

1. Klik op **Nieuw**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **App Services** > **Active Directory** > **Directory** > **Aangepaste maken**. 
3. Typ of Selecteer de volgende waarden:

    - **Naam**: contosoaaddirectory
    - **Domeinnaam**: contoso.  Deze naam moet uniek zijn.
    - **Land of regio**: Selecteer uw land of regio.
4. Klik op **Voltooien**.


**Een Azure AD-gebruiker maken**

1. Klik op **Active Directory**in de [portal van Azure klassieke](https://manage.windowsazure.com) -> **contosoaaddirectory**. 
3. Klik op **gebruikers** in het bovenste menu.
4. Klik op **gebruiker toevoegen**.
4. Voer de **Gebruikersnaam in te voeren**en klik vervolgens op **volgende**. 
5. Gebruikersprofiel; configureren Selecteer in de **rol** **Globale beheerder**; en klik op **volgende**.  De rol van globale beheerder nodig is om te maken van afdelingen.
6. Klik op **maken** om een tijdelijk wachtwoord.
7. Maak een kopie van het wachtwoord en klik vervolgens op **Voltooien**. Verderop in deze zelfstudie gebruikt u deze gebruiker globale beheerder aanmelden bij de beheerder VM voor het maken van een organisatie-eenheid en configureren van omgekeerde DNS.


Voer dezelfde procedure twee meer om gebruikers te maken met de rol, hiveuser1 en hiveuser2 van **gebruiker** . De volgende gebruikers worden gebruikt in [configureren component beleidsregels voor een domein behoren HDInsight clusters](hdinsight-domain-joined-run-hive.md).

**Domeincontroller beheerders van de AAD-groep maken en een Azure AD-gebruiker toevoegen**

1. Klik op **Active Directory**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **contosoaaddirectory**. 
3. Klik op **groepen** van het bovenste menu.
4. Klik op **een groep toevoegen** of **groep toevoegen**.
5. Typ of Selecteer de volgende waarden:

    - **Naam**: AAD domeincontroller beheerders.  Niet de naam van de groep niet wijzigen.
    - **Groepstype**: beveiliging.
6. Klik op **Voltooien**.
7. Klik op **Beheerders van de domeincontroller AAD** als de groep wilt openen.
8. Klik op **leden toevoegen**.
9. Selecteer de eerste gebruiker die u in de vorige stap hebt gemaakt en klik vervolgens op **Voltooien**.
10. Herhaal deze stappen als u wilt maken van een andere groep **HiveUsers**genoemd en de twee component gebruikers toevoegen aan de groep.

Zie [Azure AD-domeinservices (Preview) - de ' AAD domeincontroller ' beheerdersgroep maken](../active-directory-domain-services/active-directory-ds-getting-started.md)voor meer informatie.

**Azure AD DS inschakelen voor uw Azure AD**

1. Klik op **Active Directory**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **contosoaaddirectory**. 
3. Klik op **configureren** in het bovenste menu.
4. Schuif omlaag naar **Domeinservices**, en stel de volgende waarden:

    - **Domeinservices voor deze map inschakelen**: Ja.
    - **DNS-domeinnaam van domeinservices**: Hiermee worden de standaardnaam van de DNS van de Azure-directory. Bijvoorbeeld contoso.onmicrosoft.com.
    - **Domeinservices verbinden met dit virtuele netwerk**: Selecteer het klassieke virtuele netwerk dat u eerder hebt gemaakt, dat wil zeggen **contosoaadvnet**.
    
6. Klik op **Opslaan** vanaf de onderkant van de pagina. Ziet u de **in behandeling...** naast het **inschakelen van domeinservices voor deze map**.  
7. Wacht totdat **in behandeling...** verdwijnt, en **IP-adres** wordt gevuld. Twee IP-adressen wordt krijgen gevuld. Dit zijn de IP-adressen van de domeincontrollers deze is ingericht door Domain Services. Elk IP-adres zijn zichtbaar nadat de bijbehorende domeincontroller ingerichte en klaar is. Noteer de twee IP-adressen. Moet u deze later.

Zie [Azure AD-domeinservices (Preview) - inschakelen Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md)voor meer informatie.

**Voor het synchroniseren van wachtwoord**

Als u uw eigen domein gebruiken, moet u het wachtwoord synchroniseren. Zie [Wachtwoordsynchronisatie Azure AD domain Services voor een Azure alleen de cloud inschakelen AD directory](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).


**Leesbaar TEKSTFORMAAT configureren voor de Azure AD**

1. Krijg een SSL-certificaat dat is ondertekend door een ondertekend instantie voor uw domein. Zelfondertekende certificaten kunnen niet worden gebruikt. Als u niet een SSL-certificaat, neem bereiken hdipreview@microsoft.com voor een uitzondering.
1. Klik op **Active Directory**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **contosoaaddirectory**. 
3. Klik op **configureren** in het bovenste menu.
4. Schuif naar **domeinservices**.
5. Klik op **configureren certificaat**.
6. Volg de instructies voor het certificaatbestand en het wachtwoord opgeven. Ziet u de **in behandeling...** naast het **inschakelen van domeinservices voor deze map**.  
7. Wacht totdat **in behandeling...** verdwijnt, en **Secure LDAP-certificaat** hebt ingevuld.  Dit duurt maximaal 10 minuten of langer.
 
>[AZURE.NOTE] Als sommige achtergrondtaken worden uitgevoerd op de Azure AD DS, ziet u mogelijk een fout bij het uploaden van certificaat - <i>Er is een bewerking die wordt uitgevoerd voor deze tenant. Probeer het later opnieuw</i>.  Als u deze fout ondervindt, probeer het later opnieuw. De tweede domeincontroller IP duurt maximaal 3 uur worden deze is ingericht.

Zie voor meer informatie [Configureren Secure LDAP (leesbaar TEKSTFORMAAT) voor een domein Azure AD-domeinservices beheerd](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="configure-an-organizational-unit-and-reverse-dns"></a>Een organisatie-eenheid configureren en omgekeerde DNS

In deze sectie, kunt u een virtuele machine toevoegen aan de Azure AD-VNet en beheerprogramma's installeren op deze VM, zodat u kunt een organisatie-eenheid configureren en DNS omgekeerde. Omgekeerde DNS-zoekopdracht is vereist voor Kerberos-verificatie.

**Een virtuele machine in het virtuele netwerk maken**

1. Klik op **Nieuw**in de [portal van Azure klassieke](https://manage.windowsazure.com) > **berekenen** > **VM** > **Uit galerie**.
3. Selecteer een afbeelding en klik vervolgens op **volgende**.  Als u niet welk lettertype weet moet gebruiken, selecteert u de standaard, **Windows Server 2012 R2 Datacenter**.
4. Typ of Selecteer de volgende waarden:

    - De naam van de VM: **contosoaadadmin**
    - **Eenvoudige** fase:
    - Nieuwe gebruikersnaam: (Typ de naam van een gebruiker)
    - Wachtwoord: (Een wachtwoord invoeren)
    
    Houd er rekening mee dat de gebruikersnaam en het wachtwoord is de lokale beheerder.
    
5. Klik op **volgende**
6. In de **Regio/virtuele netwerk**, selecteert u het nieuwe virtuele netwerk dat u hebt gemaakt in de laatste stap (contosoaadvnet) en klik vervolgens op **volgende**.
7. Klik op **Voltooien**.

**Naar RDP voor VM**

1. Klik in de [klassieke Azure-portal](https://manage.windowsazure.com)op **virtuele Machines** > **contosoaadadmin**.
3. Klik op **Dashboard** in het bovenste menu.
4. Klik op **verbinding maken** vanaf de onderkant van de pagina.
5. Volg de instructies en de lokale beheerdersgebruikersnaam en wachtwoord om verbinding te gebruiken.

**VM toevoegen aan de Azure AD-domein**

1. Vanuit de RDP-sessie, klikt u op **Start**en klik op **Serverbeheer**.
2. Klik op **Lokale Server** in het linkermenu.
3. Uit de werkgroep, klikt u op **werkgroep**.
4. Klik op **wijzigen**.
5. Klik op **domein**, voert u **contoso.onmicrosoft.com**en klik vervolgens op **OK**.
6. Voer de referenties van de domein-gebruiker en klik vervolgens op **OK**.
7. Klik op **OK**.
8. Klik op **OK** om de computer opnieuw opstarten te accepteren.
9. Klik op **sluiten**.
10. Klik op nu **opnieuw**.

Zie [deelnemen aan een Windows Server-VM een beheerde domein](../active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm.md)voor meer informatie.

**Active Directory-beheerprogramma's en hulpprogramma's voor DNS-installeren**

1. RDP in **contosoaadadmin** met de Azure AD-gebruikersaccount.
2. Klik op **Start**en klik vervolgens op **Serverbeheer**.
3. Klik op **Dashboard** in het linkermenu.
4. Klik op **beheren**en klik vervolgens op **functies toevoegen en functies**.
5. Klik op **volgende**.
6. Selecteer **op basis van rollen of functies gebaseerde installatie**en klik vervolgens op **volgende**.
7. Selecteer de huidige virtuele machine uit de groep server en klik op **volgende**.
8. Klik op **volgende** als u wilt overslaan rollen.
9. **Externe Server beheerprogramma's**uitvouwen, **beheerprogramma's**voor functies, selecteert u **AD DS en AD-hulpprogramma's** en **Hulpprogramma's voor DNS-Server**en klik vervolgens op **volgende**. 
10. Klik op **volgende**
10. Klik op **installeren**.

Zie [Active Directory installeren beheerprogramma's op de virtuele machine](../active-directory-domain-services/active-directory-ds-admin-guide-administer-domain.md#task-2---install-active-directory-administration-tools-on-the-virtual-machine)voor meer informatie.


**Omgekeerde DNS configureren**

1. RDP naar contosoaadadmin met de Azure AD-gebruikersaccount.
2. Klik op **Start**, klik op **Systeembeheer**en klikt u op **DNS**. 
3. Klik op **geen** als u wilt overslaan ContosoAADAdmin toevoegen.
4. Selecteer **de volgende computer**, voert u het IP-adres van de eerste DNS-server die u eerder hebt geconfigureerd en klik vervolgens op **OK**.  Er wordt dat de domeincontroller/DNS-records wordt toegevoegd aan het linkerdeelvenster.
3. Vouw de domeincontroller/DNS-server, met de rechtermuisknop op **Zones voor Reverse Lookup**en klik vervolgens op **Nieuwe Zone**. De Wizard Nieuwe Zone wordt geopend.
4. Klik op **volgende**.
5. Selecteer de **primaire zone**en klik vervolgens op **volgende**.
6. Selecteren **tot alle DNS-servers op domeincontrollers in dit domein**en klik vervolgens op **volgende**.
6. Selecteer **IPv4 omkeren opzoeken Zone**en klik vervolgens op **volgende**.
7. In **Netwerk-ID**, voert u het voorvoegsel voor het bereik van HDInsight VNET netwerk en klik vervolgens op **volgende**. U maakt de HDInsight VNet in de volgende sectie.
8. Klik op **volgende**.
9. Klik op **volgende**.
10. Klik op **Voltooien**.



De organisatie-eenheid die u maakt worden vervolgens gebruikt bij het maken van het cluster HDInsight. De gebruikers van Hadoop en computeraccounts wordt in deze organisatie-eenheid worden geplaatst.

**Maken van een organisatie-eenheid op een beheerde domeinservices van Azure AD-domein**

1. RDP in **contosoaadadmin** met het domeinaccount dat is in de groep **Beheerders van de domeincontroller AAD** .
2. Klik op **Start**, klikt u op **Systeembeheer**en klik vervolgens op **Beheerderscentrum voor Active Directory**.
5. Klik op de domeinnaam in het linkerdeelvenster. Bijvoorbeeld contoso.
6. Klik op **Nieuw** onder de domeinnaam in **het taakvenster** en klik vervolgens op **Organisatie-eenheid**.
7. Voer een naam, bijvoorbeeld **HDInsightOU**, en klik vervolgens op **OK**. 


Zie [maken een organisatie-eenheid in een domein Azure AD-domeinservices beheerd](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)voor meer informatie.


## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Een Resource Manager VNet voor HDInsight cluster maken

In dit gedeelte maakt u een Azure resourcemanager VNet die wordt gebruikt voor het cluster HDInsight. Zie [maken een virtueel netwerk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) voor meer informatie over het maken van Azure VNET andere methoden gebruiken

Na het maken van de VNet, configureert u de Resource Manager VNet als u wilt dezelfde DNS-servers voor de Azure AD-VNet gebruiken. Als u de stappen in deze zelfstudie om de klassieke VNet en de Azure AD te maken hebt gevolgd, worden de DNS-servers 10.1.0.4 en 10.1.0.5.

**Een Resource Manager VNet maken**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com).
2. Klik op **Nieuw**, **toegang**en klik vervolgens **virtuele netwerk**. 
3. **Resourcemanager**selecteren in **een implementatiemodel selecteren**, en klik vervolgens op **maken**.
4. Typ of Selecteer de volgende waarden:

    - **Naam**: contosohdivnet
    - **Adresruimte**: 10.2.0.0/16. Zorg ervoor dat het adresbereik niet overlappen met het bereik van de IP-adres van de klassieke VNet.
    - **Subnetnaam**: Subnet1
    - **Subnet adresbereiken**: 10.2.0.0/24
    - **Abonnement**: (Selecteer uw Azure abonnement).
    - **Resourcegroep**: contosohdirg
    - **Locatie**: (Selecteer dezelfde locatie als het Azure AD VNet, dat wil zeggen contosoaadvnet.)

5. Klik op **maken**.


**DNS configureren voor de Resource Manager VNet**

1. Klik op **meer services**in de [portal van Azure](https://portal.azure.com) -> **virtuele netwerken**. Ervoor zorgen dat niet **virtuele netwerken (klassieke)**klikt u op.
2. Klik op **contosohdivnet**.
4. Klik op **DNS-servers** vanaf de linkerkant van het nieuwe blad.
6. Klik op **aangepast**en voer de volgende waarden:

    - 10.1.0.4
    - 10.1.0.5

    Deze DNS-server IP-adressen moeten overeenkomen met de DNS-servers in de Azure AD VNet (klassieke VNet).
7. Klik op **Opslaan**.

























## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Peer van de Azure AD-VNet en de HDInsight VNet

**Naar de twee VNet op hetzelfde niveau**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com).
2. Klik op **meer services** in het linkermenu.
3. Klik op **virtuele netwerken**. Klik niet op **virtuele netwerken (klassieke)**.
4. Klik op **contosohdivnet**.  Dit is de HDInsight VNet.
5. Klik op **Peerings** in het linkermenu van het blad.
6. Klik op **toevoegen** in het bovenste menu. Het blad **peering toevoegen** wordt geopend.
7. Klik op het blad **toevoegen peering** instellen of selecteert u de volgende waarden:

    - **Naam**: ContosoAADHDIVNetPeering
    - **Virtuele netwerk implementatiemodel**: klassieke
    - **Abonnement**: Selecteer de naam van uw abonnement wordt gebruikt voor de vnet klassieke (Azure AD).
    - **Virtuele netwerk**: contosoaadvnet.
    - **Het virtuele netwerktoegang toestaan**: (controleren)
    - **Toestaan doorgestuurd verkeer**: (selectievakje). Laat de andere twee selectievakjes uitgeschakeld.

8. Klik op **OK**.



## <a name="create-hdinsight-cluster"></a>HDInsight cluster maken


In deze sectie, kunt u een Hadoop Linux gebaseerde cluster in HDInsight via de portal van Azure of de [resourcemanager Azure-sjabloon](../resource-group-template-deploy.md)maken. De instellingen, Zie voor andere methoden voor het maken van cluster en het begrip [clusters HDInsight maken](hdinsight-hadoop-provision-linux-clusters.md). Zie voor meer informatie over het gebruik van de sjabloon resourcemanager Hadoop clusters in HDInsight maken, [maken Hadoop clusters in HDInsight resourcemanager sjablonen gebruiken](hdinsight-hadoop-create-windows-clusters-arm-templates.md)


**Een domein behoren HDInsight cluster met behulp van de Azure portal maken**

1. Aanmelden bij de [portal van Azure](https://portal.azure.com).
2. Klik op **Nieuw**, **Intelligence + analytics**en vervolgens **HDInsight**.
3. Van het blad **nieuwe HDInsight cluster** , typ of Selecteer de volgende waarden:

    - **De naam van cluster**: Voer een nieuwe clusternaam voor het domein behoren HDInsight cluster.
    - **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van dit cluster.
    - **Configuratie van het cluster**:

        - **Clustertype**: Hadoop. Een domein behoren HDInsight is momenteel alleen ondersteund op Hadoop clusters.
        - **Besturingssysteem**: Linux.  Een domein behoren HDInsight wordt alleen ondersteund op HDInsight Linux gebaseerde clusters.
        - **Versie**: Hadoop punt 2.7.3 (HDI 3.5). Een domein behoren HDInsight wordt alleen ondersteund op HDInsight cluster versie 3.5.
        - **Clustertype**: PREMIUM

        Klik op **selecteren** als de wijzigingen wilt opslaan.

    - **Referenties**: de referenties voor de gebruiker cluster zowel de gebruiker SSH configureren.
    - **Gegevensbron**: Maak een nieuwe opslag-account of een bestaand account voor de opslag gebruiken als het standaardaccount voor de opslag voor het cluster HDInsight. De locatie moet hetzelfde als de twee VNets.  De locatie is ook de locatie van het cluster HDInsight.
    - **Prijzen**: Selecteer het aantal knooppunten werknemer van uw cluster.
    - **Geavanceerde configuraties**: 

        - **Lid worden van een domein en Vnet/Subnet**: 

            - **Domain settings**: 

                - **Domeinnaam**: contoso.onmicrosoft.com
                - **Domein gebruikersnaam in te voeren**: Typ de naam van de gebruiker van een domein. In dit domein moet beschikken over de volgende bevoegdheden: machines toevoegen aan het domein en deze in de organisatie-eenheid die u eerder; geconfigureerd plaatsen Service principes binnen de organisatie-eenheid die u eerder; geconfigureerd maken Maak omgekeerde DNS entries. In dit domeingebruiker wordt ingesteld als de beheerder van deze cluster HDInsight domein behoren.
                - **Domeinwachtwoord**: Voer het wachtwoord van de gebruiker domein.
                - **Organisatie-eenheid**: Voer de DN-naam van de organisatie-eenheid tht die u eerder hebt geconfigureerd. Bijvoorbeeld: OU = HDInsightOU, domeincontroller = contoso, domeincontroller = onmicrosoft, domeincontroller = com
                - **Leesbaar TEKSTFORMAAT URL**: ldaps://contoso.onmicrosoft.com:636
                - **Groep van Access-gebruiker**: de beveiligingsgroep waarvan u wan vereist voor synchronisatie met het cluster gebruikers opgeven. Bijvoorbeeld: HiveUsers.

                Klik op **selecteren** als de wijzigingen wilt opslaan.

                ![Een domein behoren HDInsight portal domein instellen](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
            - **Virtual Network**: contosohdivnet
            - **Subnet**: Subnet1

            Klik op **selecteren** als de wijzigingen wilt opslaan.       
        Klik op **selecteren** als de wijzigingen wilt opslaan.
    - **Resourcegroep**: Selecteer de resourcegroep die wordt gebruikt voor de HDInsight VNet (contosohdirg).

4. Klik op **maken**.  

Er is een andere optie voor het maken van een domein behoren HDInsight cluster Azure resourcebeheer sjabloon wilt gebruiken. De volgende procedure ziet u hoe:

**Een domein behoren HDInsight cluster met een resourcebeheer-sjabloon maken**

1. Klik op de volgende afbeelding om een sjabloon resourcemanager openen in de portal van Azure. De sjabloon resourcemanager bevindt zich in een container openbare blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende waarden van het blad **Parameters** :

    - **Abonnement**: (Selecteer uw Azure abonnement).
    - **Resourcegroep**: klik op **bestaande gebruiken**, en geef de dezelfde resourcegroep die u hebt gebruikt.  Bijvoorbeeld contosohdirg. 
    - **Locatie**: Geef een locatie van de groep resource.
    - **De naam van cluster**: Voer een naam voor het Hadoop-cluster dat u wilt maken. Bijvoorbeeld contosohdicluster.
    - **Cluster Type**: Selecteer een clustertype.  De standaardwaarde is **hadoop**.
    - **Locatie**: Selecteer een locatie voor het cluster.  Het standaardaccount voor de opslag gebruikmaakt van dezelfde locatie.
    - **Cluster werknemer knooppunt telling**: Selecteer het aantal knooppunten werknemer.
    - **Cluster aanmeldingsnaam en wachtwoord**: de standaard-aanmeldingsnaam is **beheerder**.
    - **SSH gebruikersnaam en wachtwoord**: de standaard-gebruikersnaam is **sshuser**.  U kunt dit wijzigen. 
    - **Virtuele netwerk-Id**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
    - **Virtuele netwerk Subnet**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/subnetten/Subnet1
    - **Domeinnaam**: contoso.onmicrosoft.com
    - **Organisatie-eenheid DN**: OU = HDInsightOU, domeincontroller = contoso, domeincontroller = onmicrosoft, domeincontroller = com
    - **Cluster gebruikers groep D Ns**: "\"CN = HiveUsers, OU = AADDC gebruikers, domeincontroller =<DomainName>, domeincontroller = onmicrosoft, domeincontroller = com\""
    - **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
    - **DomainAdminUserName**: (Voer de gebruikersnaam van de domein-beheerder)
    - **DomainAdminPassword**: (Voer het wachtwoord voor het domein beheerder gebruiker)
    - **Ik ga akkoord met de bovenstaande voorwaarden**: (controleren)
    - **Vastmaken aan het dashboard**: (controleren)

6. Klik op **aanschaffen**. Hier ziet u een nieuwe tegel getiteld **implementeert sjabloon-implementatie**. Het duurt over ongeveer 20 minuten een cluster maken. Nadat het cluster is gemaakt, kunt u klikken op het blad cluster in de portal om dit te openen.

Nadat u de zelfstudie hebt voltooid, wilt u mogelijk het cluster verwijderen. Uw gegevens worden opgeslagen in Azure-opslag, met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer deze niet gebruikt wordt. U wordt ook geheven voor een cluster HDInsight, zelfs wanneer deze niet gebruikt wordt. Aangezien de kosten voor het cluster vaak meer dan de kosten voor opslag zijn, relevant dat is economic clusters verwijderen wanneer ze niet gebruikt worden. Zie de instructies voor het verwijderen van een cluster [beheren Hadoop clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-management-portal.md#delete-clusters).





## <a name="next-steps"></a>Volgende stappen

- Voor het configureren van beleidsregels voor component en uitvoeren component-query's, Zie [configureren component beleidsregels voor een domein behoren HDInsight clusters](hdinsight-domain-joined-run-hive.md).
- Voor het uitvoeren van component query's met SSH op domein behoren HDInsight clusters, Zie [Gebruik SSH met Linux gebaseerde Hadoop op HDInsight uit Linux, Unix, of OS X](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-domain-joined-hdinsight-cluster).
