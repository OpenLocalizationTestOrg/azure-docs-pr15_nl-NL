<properties 
    pageTitle="Een virtueel netwerk met een netwerk configuratie-bestand configureren" 
    description="Instructies voor het exporteren en importeren in een netwerk configuratiebestand de Azure-beheerportal maken of wijzigen van virtuele netwerken. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Een virtueel netwerk met een netwerk configuratie-bestand configureren

U kunt een virtueel netwerk (VNet) configureren met behulp van de Azure-beheerportal of via een netwerk configuratiebestand.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Maken en wijzigen van een netwerk configuratiebestand 
De eenvoudigste manier om de auteur van een configuratiebestand netwerk is de netwerkinstellingen exporteren vanuit een bestaande virtuele netwerkconfiguratie, wijzigt u het bestand en de instellingen die u wilt configureren voor uw virtuele netwerken bevatten.

Als u wilt bewerken in het configuratiebestand netwerk, kunt u gewoon opent u het bestand, breng de gewenste wijzigingen aan en sla het bestand. U kunt een *XML-* editor gebruiken om het configuratiebestand netwerk te wijzigen. 

U moet nauw volgt u de richtlijnen voor [instellingen van bestand schema netwerkconfiguratie](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure acht een subnet met iets geïmplementeerd in deze als **gebruikt**. Wanneer een subnet gebruikt wordt, kunnen niet worden gewijzigd. Voordat u wijzigt, moet u alles die u hebt geïmplementeerd in het subnet naar een ander subnet dat is niet worden gewijzigd verplaatst.   Zie [een VM of rol exemplaar naar een ander Subnet verplaatsen](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Instellingen van virtueel netwerk met behulp van de Portal Management exporteren en importeren  
U kunt importeren en exporteren van netwerk configuratie van instellingen in het configuratiebestand netwerk via PowerShell of de beheerportal. De onderstaande instructies kunt u exporteren en importeren met behulp van de Portal Management. 

### <a name="to-export-your-network-settings"></a>Uw netwerkinstellingen exporteren
Als u exporteert, worden alle instellingen voor de virtuele netwerken in uw abonnement naar een XML-bestand worden geschreven. 

1. Meld u aan bij de **beheerportal**.
2. Klik in de beheerportal, vanaf de onderkant van de pagina **netwerken** op **exporteren**. 
3. Controleer of u het abonnement waarvoor u wilt exporteren van uw netwerkinstellingen hebt geselecteerd in het venster **netwerkconfiguratie exporteren** . Klik vervolgens op het vinkje op de rechterbenedenhoek. 
4. Wanneer u wordt gevraagd, moet u de *NetworkConfig.xml* -bestand opslaan naar de locatie van uw keuze.


### <a name="to-import-your-network-settings"></a>Uw netwerkinstellingen importeren

1. Klik in de **Beheerportal**in het navigatiedeelvenster links, onderaan op **Nieuw**.
2. Klik op **Netwerkservices** -> **Virtual Network** -> **configuratie importeren**.
3. Klik op de pagina **importeren het configuratiebestand netwerk** bladert u naar uw netwerk configuratie-bestand en klik vervolgens op **de pijl-rechts** .
4. Klik op de pagina **voor het samenstellen van uw netwerk** ziet u informatie over het scherm met welke gedeelten van uw netwerkconfiguratie zijn gewijzigd of gemaakt. Als de wijzigingen wilt juiste voor u opzoeken, klikt u op het vinkje om door te gaan om te werken of maak uw virtuele netwerk. 