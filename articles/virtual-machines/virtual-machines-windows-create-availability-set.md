<properties
    pageTitle="Maken van een set van de beschikbaarheid van VM | Microsoft Azure"
    description="Informatie over het maken van een beschikbaarheid instellen voor uw virtuele machines via Azure portal of PowerShell met het implementatiemodel resourcemanager."
    keywords="beschikbaarheid instellen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


# <a name="create-an-availability-set"></a>Een set beschikbaarheid maken 

Wanneer u met behulp van de portal, als u wilt dat uw VM wilt toevoegen aan een beschikbaarheid instellen, moet u de beschikbaarheid Bepaal eerst maken.

Zie voor meer informatie over het maken en gebruiken van beschikbaarheid sets [beheren de beschikbaarheid van virtuele machines](virtual-machines-windows-manage-availability.md).


## <a name="use-the-portal-to-create-an-availability-set-before-creating-your-vms"></a>De portal gebruiken om te maken van een beschikbaarheid instellen voordat u uw VMs maakt

1. In het menu hub klikt u op **Bladeren** en selecteer **beschikbaarheid wordt ingesteld**.

2. Klik op de die wordt **beschikbaarheid blade ingesteld**, klikt u op **toevoegen**.

    ![Schermafbeelding met de knop toevoegen voor het maken van een nieuwe beschikbaarheid instellen.](./media/virtual-machines-windows-create-availability-set/add-availability-set.png)

3. Klik op het blad **maken beschikbaarheid instellen** , voert u de gegevens voor een set.

    ![Schermafbeelding met daarin de gegevens waarvan u de naam moet opgeven om de beschikbaarheid van de set te maken.](./media/virtual-machines-windows-create-availability-set/create-availability-set.png)

    - **De naam van** - 1-80 tekens bestaan uit getallen, letters, perioden, onderstrepingstekens en streepjes moet in de naam. Het eerste teken moet een letter of een getal. Het laatste teken moet een letter, getallen of onderstrepingsteken.
    - **Foutenstructuuranalyse domeinen** - foutenstructuuranalyse domeinen de groep van virtuele machines die een gemeenschappelijke power bron- en netwerk schakeloptie delen definiëren. Standaard de VMs gescheiden voor maximaal drie foutenstructuuranalyse domeinen en kunnen worden gewijzigd in tussen 1 en 3.
    - **Domeinen bijwerken** - vijf update domeinen zijn toegewezen standaard en dit kan worden ingesteld op tussen 1 en 20. Update domeinen geven aan groepen van virtuele machines en onderliggende fysieke hardware die kan worden gestart op hetzelfde moment. Bijvoorbeeld als we opgeeft vijf domeinen bijwerken wanneer meer dan vijf virtuele machines zijn geconfigureerd binnen een enkel beschikbaarheid instellen, de zesde virtuele machine wordt geplaatst in het domein bijwerken als de eerste virtuele machine, het zevende in de dezelfde per als de tweede virtuele machine, enzovoort. De volgorde van de computer opnieuw hebt opgestart mogelijk geen opeenvolgende, maar slechts één update domain wordt opnieuw worden gestart tegelijk.
    - **Abonnement** - Selecteer het abonnement kunt gebruiken als u meer dan één.
    - **Resourcegroep** - selecteren van een bestaande resourcegroep door te klikken op de pijl en selecteren van een resourcegroep in de vervolgkeuzelijst omlaag. U kunt ook een nieuwe resourcegroep maken door in een naam te typen. De naam kan een van de volgende tekens bevatten: letters, cijfers, punten, streepjes, onderstrepingstekens en openen of haakje sluiten. De naam mag niet eindigen in een periode. Alle de VMs in de beschikbaarheidsgroep moet worden gemaakt in dezelfde resourcegroep als de beschikbaarheid van de set.
    - **Locatie** - Selecteer een locatie in de vervolgkeuzelijst.

4. Wanneer u klaar bent met invoeren van de gegevens, klikt u op **maken**. Wanneer u de van de beschikbaarheidsgroep hebt gemaakt, kunt u deze in de lijst zien nadat u de portal vernieuwen.

## <a name="use-the-portal-to-create-a-virtual-machine-and-an-availability-set-at-the-same-time"></a>De portal gebruiken om een virtuele machine en een beschikbaarheid instellen op hetzelfde moment maken

Als u een nieuwe VM met behulp van de portal maakt, kunt u ook een nieuwe beschikbaarheid instellen voor de VM terwijl u de eerste VM in de set maken maken.

![Schermafbeelding van die het proces is voor het maken van een nieuwe beschikbaarheid instellen terwijl u de VM maken.](./media/virtual-machines-windows-create-availability-set/new-vm-avail-set.png)


## <a name="add-a-new-vm-to-an-existing-availability-set"></a>Een nieuwe VM toevoegen aan een bestaande beschikbaarheid instellen

Zorg ervoor dat u deze hebt gemaakt in dezelfde **resourcegroep** en selecteer vervolgens de bestaande beschikbaarheid instellen in stap 3 voor elke extra VM die u maakt die in de set moet behoren. 

![Schermafbeelding van het selecteren van een bestaande beschikbaarheid instellen voor uw VM wilt gebruiken.](./media/virtual-machines-windows-create-availability-set/add-vm-to-set.png)



## <a name="use-powershell-to-create-an-availability-set"></a>PowerShell gebruiken om u te maken van een set beschikbaarheid

In dit voorbeeld wordt een beschikbaarheid instellen in de resourcegroep **RMResGroup** op de locatie **West Amerikaans** gemaakt. Dit moet worden uitgevoerd voordat u de eerste VM die weergegeven in de set worden maakt.

    New-AzureRmAvailabilitySet -ResourceGroupName "RMResGroup" -Name "AvailabilitySet03" -Location "West US"
    
Zie [Nieuw AzureRmAvailabilitySet](https://msdn.microsoft.com/library/mt619453.aspx)voor meer informatie.


## <a name="troubleshooting"></a>Problemen oplossen

- Wanneer u een VM, als de gewenste set met beschikbaarheid niet in de vervolgkeuzelijst in de portal maakt u mogelijk deze hebt gemaakt in een andere resource-groep. Als u niet weet de resourcegroep voor uw beschikbaarheid instellen, gaat u naar de hub-menu en klik op Bladeren > beschikbaarheid wordt ingesteld voor een overzicht van uw beschikbaarheid sets en welke resourcegroepen waartoe ze behoren.


## <a name="next-steps"></a>Volgende stappen

Extra opslagruimte wilt toevoegen aan uw VM door het toevoegen van een extra [gegevens Schijfopruiming](virtual-machines-windows-attach-disk-portal.md).
