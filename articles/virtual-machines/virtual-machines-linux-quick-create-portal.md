<properties
    pageTitle="Maken van een Linux VM met behulp van de Portal Azure | Microsoft Azure"
    description="Maak een Linux VM met behulp van de Azure-Portal."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Maak een VM Linux op Azure met behulp van de Portal


In dit artikel leest u het gebruik van de [Azure-portal](https://portal.azure.com/) een Linux virtuele Machine maken.

De vereisten zijn:

- [een Azure-account](https://azure.microsoft.com/pricing/free-trial/)

- [SSH openbare en persoonlijke sleutels bestanden](virtual-machines-linux-mac-create-ssh-keys.md)


1. Aangemeld bij de Azure-portal met uw identiteit Azure-account, klikt u op **+ Nieuw** in de linkerbovenhoek wordt weergegeven:

    ![scherm1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Klik op **virtuele Machines** in de **Marketplace** **Ubuntu Server 14.04 LTS** vanuit de **Aanbevolen Apps** lijst met afbeeldingen.  Controleer of onder het implementatiemodel is `Resource Manager` en klik op **maken**.

    ![--scherm2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Klik op de pagina **Basisbeginselen** invoeren:
    - een naam voor de VM
    - een gebruikersnaam voor de beheerder
    - het Type verificatie ingesteld op **SSH openbare sleutel**
    - uw openbare sleutel SSH als een tekenreeks (vanuit uw `~/.ssh/` directory)
    - een resource groepsnaam of Selecteer een bestaande groep

    en klik op **OK** om terug te gaan en kies de grootte van de VM; Deze ziet er ongeveer als volgt te werk:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. Kies de grootte **DS1** Ubuntu op een Premium-SSD installeert, en klik op **selecteren** om instellingen te configureren.

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. In **Instellingen**, laat u de standaardinstellingen voor opslag- en -waarden en klik op **OK** om het overzicht weer te geven.  Het type Schijfopruiming kennisgeving is ingesteld op Premium SSD door te kiezen DS1, de **S** notates SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Bevestig de instellingen voor uw nieuwe Ubuntu VM en klik op **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Open van het Dashboard Portal en kies uw NIC in **netwerk-interfaces**

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Open het openbare IP-adressen menu onder de NIC-instellingen

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH in het openbare IP met uw openbare sleutel SSH

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu een VM Linux snel moet worden gebruikt voor testen of demonstratie doeleinden gemaakt. Als u wilt maken van een Linux VM voor de infrastructuur van uw aangepaste, kunt u een van deze artikelen volgen.

- [Maak een VM Linux op Azure sjablonen gebruiken](virtual-machines-linux-cli-deploy-templates.md)
- [Maak een SSH beveiligd Linux VM op Azure sjablonen gebruiken](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Een Linux VM gebruik van de Azure CLI maken](virtual-machines-linux-create-cli-complete.md)
