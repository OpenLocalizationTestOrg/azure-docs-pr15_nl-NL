<properties 
   pageTitle="Problemen met netwerk beveiligingsgroepen - Portal | Microsoft Azure"
   description="Informatie over het oplossen van netwerk beveiligingsgroepen in het resourcemanager Azure implementatiemodel met behulp van de Azure-Portal."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Problemen met netwerk beveiligingsgroepen met behulp van de Azure-Portal

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Als u beveiligingsgroepen netwerk (NSGs) voor uw VM (VM) hebt geconfigureerd en VM verbindingsproblemen zich voordoet, overzicht in dit artikel van mogelijkheden voor diagnostische hulpprogramma's voor NSGs om te helpen oplossen verder.

NSGs kunt u bepalen welke typen verkeer stromen en afmelden bij uw virtuele machines (VMs). NSGs kunnen worden toegepast op subnetten die in een Azure Virtual Network (VNet), netwerkinterfaces (NIC) of beide. De effectieve regels toegepast op een NIC zijn een aggregatie van de regels die in de NSGs toegepast op een NIC aanwezig en het is verbonden met subnet. Regels op deze NSGs kunnen soms een conflict veroorzaken met elkaar en van een VM netwerkconnectiviteit van invloed zijn op.  

NIC's van uw VM toegepast, kunt u alle beveiligingsregels voor het effectieve van uw NSGs, weergeven. In dit artikel ziet hoe u problemen met VM verbindingen met behulp van deze regels in het implementatiemodel resourcemanager Azure. Als u niet bekend met VNet en NSG concepten bent, leest u de [virtuele netwerk](virtual-networks-overview.md) - en [netwerk beveiligingsgroepen](virtual-networks-nsg.md) overzicht artikelen.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Effectieve beveiligingsregels gebruiken bij het oplossen van VM verkeersstroom

Het scenario die volgt is een voorbeeld van een algemene verbindingsprobleem:

Een benoemde *VM1* VM maakt deel uit van een subnet met *Subnet1* binnen een VNet *WestUS-VNet1*met de naam de naam. Een poging tot het verbinding maken met de VM RDP met TCP-poort 3389 gebruiken is mislukt. NSGs worden toegepast op zowel de NIC *VM1-NIC1* als het subnet *Subnet1*. TCP-poort 3389-verkeer is toegestaan in de NSG die is gekoppeld aan het netwerkinterface *VM1-NIC1*, maar TCP ping naar VM1 van poort 3389 mislukt.

Terwijl dit voorbeeld wordt TCP-poort 3389 gebruikt, kunnen de volgende stappen uit om te bepalen van binnenkomende en uitgaande verbindingsfouten via een poort worden gebruikt.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Effectieve beveiligingsregels voor een virtuele machine weergeven

Voer de volgende stappen uit om op te lossen NSGs voor een VM:

U kunt de volledige lijst met de effectieve beveiligingsregels bekijken op een NIC, uit de VM zelf. U kunt ook toevoegen, wijzigen en verwijderen van zowel NIC als subnet NSG regels uit het blad effectieve regels als u machtigingen hebt voor deze bewerkingen uitvoeren.

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een VM om op te lossen in de lijst die wordt weergegeven en een blade VM met opties wordt weergegeven.
4. Klik op **Diagnose & oplossen van problemen** en selecteer vervolgens een veelvoorkomend probleem. In dit voorbeeld is **ik kan geen verbinding maken met mijn Windows-VM** geselecteerd. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Stappen die wordt weergegeven onder het probleem, zoals wordt weergegeven in de volgende afbeelding: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Klik op *regels voor effectieve groep* in de lijst met aanbevolen stappen.

6. Het blad **effectieve beveiligingsregels ophalen** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Let op de volgende secties van de afbeelding:

    - **Bereik:** De VM is ingesteld op *VM1*, geselecteerd in stap 3.
    - **Netwerkinterface:** *VM1-NIC1* is geselecteerd. Een VM kan meerdere netwerkinterfaces (NIC) hebben. Elke NIC kunt unieke effectieve Beveiligingsregels hebben. Als wilt oplossen, moet u mogelijk de effectieve beveiligingsregels weergeven voor elke NIC
    - **Die is gekoppeld NSGs:** NSGs kunnen worden toegepast op de NIC zowel het subnet dat de NIC is verbonden met. In de afbeelding, is een NSG toegepast op de NIC zowel het subnet dat is verbonden met. U kunt klikken op de namen NSG regels in de NSGs rechtstreeks wijzigen.
    - **VM1-nsg tabblad:** De lijst met regels die worden weergegeven in de afbeelding is bedoeld voor de NSG toegepast op de NIC Verschillende standaardregels worden gemaakt door Azure wanneer een NSG wordt gemaakt. U kunt de standaardregels niet verwijderen, maar u kunt deze overschrijven met regels van een hogere prioriteit. Meer informatie over standaardregels voor het Lees het artikel [NSG overzicht](virtual-networks-nsg.md#default-rules) .
    - **Doelkolom:** Sommige regels hebt tekst in de kolom terwijl andere adres voorvoegsels voor eenheden een. De tekst is de naam van standaardlabels toegepast op de regel wanneer deze is gemaakt. De codes zijn systeem id's waarmee meerdere voorvoegsels voor eenheden. Selecteren van een regel met een tag, zoals *AllowInternetOutBound*, bevat de voorvoegsels uit het blad **adres voorvoegsels voor eenheden** .
    - **Downloaden:** De lijst met regels kan lang zijn. U kunt een CSV-bestand van de regels voor offline analyse downloaden door te klikken op **downloaden** en het bestand op te slaan.
    - **AllowRDP** Binnenkomende regel: met deze regel RDP-verbindingen voor VM toestaat.
7. Klik op het tabblad **Subnet1-NSG** om de effectieve regels uit de NSG toegepast op het subnet, zoals wordt weergegeven in de volgende afbeelding: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Zoals u ziet de *denyRDP* **Inkomend** regel. Regels voor binnenkomende verbindingen toegepast op het subnet worden geëvalueerd voordat u regels toegepast bij de netwerkadapter. Aangezien deze regel wordt toegepast op het subnet, wordt de aanvraag om deel te verbinden met TCP 3389 mislukt, omdat de regel voor toestaan op de NIC nooit wordt geëvalueerd. 

    De regel *denyRDP* is de reden waarom de RDP-verbinding is verbroken. Verwijderen, moet het probleem oplossen.

    >[AZURE.NOTE]Als de VM die is gekoppeld aan de NIC niet actief is of NSGs zijn toegepast op de NIC of subnet, worden geen regels weergegeven.

8. Als u wilt bewerken NSG regels, klikt u op *Subnet1-NSG* in de sectie **NSGs die is gekoppeld** .
   Hiermee opent u het blad **Subnet1-NSG** . U kunt de regels rechtstreeks bewerken door te klikken op **inkomende beveiligingsregels**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Na het verwijderen van de binnenkomende *denyRDP* -regel in de **Subnet1-NSG** en het toevoegen van een regel voor het *allowRDP* , levert de lijst met effectieve regels op de volgende afbeelding:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Controleer of TCP-poort 3389 door een RDP-verbinding met de VM openen of het hulpprogramma voor het PsPing is. Vindt u meer informatie over PsPing door te lezen de [downloadpagina van PsPing](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Effectieve beveiligingsregels voor de netwerkinterface van een weergeven

Als uw netwerkverkeer VM wordt beïnvloed voor een specifieke NIC, kunt u een volledige lijst met de effectieve regels voor de formule voor het berekenen van de context van de interfaces netwerk weergeven door de volgende stappen uit:

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **netwerk-interfaces** in de lijst die wordt weergegeven.
3. Selecteer een netwerkinterface. In de volgende afbeelding, is een NIC *VM1-NIC1* met de naam ingeschakeld.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    U ziet dat het **bereik** is ingesteld op het netwerkinterface geselecteerd. Lees meer informatie over de aanvullende informatie wordt weergegeven, stap 6 van de sectie **NSGs voor een VM oplossen** in dit artikel.

    >[AZURE.NOTE] Als een NSG wordt verwijderd uit een netwerkinterface, het subnet NSG werkt nog steeds op de opgegeven NIC In dit geval zou de uitvoer alleen regels uit het subnet NSG weergeven. Regels worden alleen weergegeven als de NIC is gekoppeld aan een VM.

4. U kunt regels voor NSGs die hoort bij een NIC en een subnet rechtstreeks bewerken. Voor meer informatie over hoe, lees stap 8 van de sectie **weergave effectieve beveiligingsregels voor een virtuele machine** in dit artikel.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Effectieve beveiligingsregels voor een beveiligingsgroep netwerk (NSG) weergeven

Als u NSG regels wijzigt, kunt u bekijken wat de invloed van de regels die op een bepaalde VM wordt toegevoegd. U kunt een volledige lijst met de effectieve beveiligingsregels weergeven voor alle NIC's die een opgegeven NSG is toegepast, zonder dat u moet de context van het opgegeven NSG blad schakelen. Om op te lossen effectieve regels binnen een NSG, moet u de volgende stappen uitvoeren:

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **netwerk beveiligingsgroepen** in de lijst die wordt weergegeven.
3. Selecteer een NSG. In de volgende afbeelding, is een NSG VM1-nsg met de naam ingeschakeld.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Let op de volgende secties van de vorige afbeelding:

    - **Bereik:** Stel op de NSG geselecteerd.
    - **VM:** Wanneer een NSG wordt toegepast op een subnet, wordt deze toegepast op alle netwerkinterfaces die zijn bijgevoegd bij alle VMs verbonden met het subnet. Deze lijst ziet u alle VMs deze NSG wordt toegepast op. U kunt een VM selecteren in de lijst.

    >[AZURE.NOTE] Als een NSG wordt toegepast op alleen een lege subnet, worden niet VMs weergegeven. Als een NSG wordt toegepast op een NIC die niet gekoppeld aan een VM is, worden deze NIC, ook niet vermeld. 
    - **Netwerk Interface:** Een VM kan meerdere netwerkinterfaces hebben. U kunt een netwerkinterface dat is gekoppeld aan de geselecteerde VM selecteren.
    - **AssociatedNSGs:** Een NIC kan maximaal twee effectieve NSGs, toegepast op de knop presentatie beveiligen en de andere naar het subnet hebben op elk moment. Hoewel het bereik is geselecteerd als VM1-nsg, als de NIC een effectieve subnet NSG heeft, wordt de uitvoer beide NSGs weergegeven.
4. U kunt regels voor NSGs die is gekoppeld aan een NIC of subnet rechtstreeks bewerken. Voor meer informatie over hoe, lees stap 8 van de sectie **weergave effectieve beveiligingsregels voor een virtuele machine** in dit artikel.

Lees meer informatie over de aanvullende informatie wordt weergegeven, stap 6 van de sectie **weergave effectieve beveiligingsregels voor een virtuele machine** van dit artikel.

>[AZURE.NOTE] Hoewel een subnet en NIC elk slechts één die NSG toegepast hebt kunt, is een NSG kan worden gekoppeld aan meerdere NIC's en meerdere subnetten.

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van problemen met de verbinding:

- Standaard NSG regels worden binnenkomende toegang van internet blokkeren en alleen toestaan VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan de binnenkomende toegang vanaf Internet, zoals vereist toestaan.
- Als er geen NSG beveiligingsregels veroorzaakt door van een VM netwerkconnectiviteit mislukt, wordt het probleem mogelijk vanwege:
    - Firewallsoftware in het besturingssysteem van de VM draaien
    - Routes geconfigureerd voor virtual toestellen of on-premises implementatie-verkeer is toegestaan. Internetverkeer kan worden omgeleid naar de on-premises implementatie via gedwongen-tunneling. Een verbinding RDP/SSH van Internet voor uw VM werkt niet met deze instelling, afhankelijk van hoe de netwerkhardware on-premises implementatie dit verkeer verwerkt. Lees het artikel [Problemen oplossen Routes](virtual-network-routes-troubleshoot-powershell.md) om te leren hoe route diagnosticeren die mogelijk worden belemmeren de stroom van verkeer en afmelden bij de VM. 
- Als u hebt VNets, al dan niet standaard peered, wordt automatisch de tag VIRTUAL_NETWORK uitgebreid met voorvoegsels voor peered VNets. U kunt deze voorvoegsels weergeven in de lijst **ExpandedAddressPrefix** eventuele problemen met betrekking tot VNet peering connectivity. 
- Effectieve beveiligingsregels worden alleen weergegeven als er een NSG die is gekoppeld aan van de VM NIC en of subnet is. 
- Als er geen NSGs die is gekoppeld aan de NIC zijn of subnet en u een openbare IP-adres dat is toegewezen aan uw VM, worden alle poorten open voor binnenkomende en uitgaande toegang. Als de VM een openbare IP-adres heeft, wordt NSGs toepassen op de NIC of subnet aanbevolen.