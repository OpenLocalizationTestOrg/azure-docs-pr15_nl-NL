<properties
pageTitle="Verbinding met extern bureaublad inschakelen voor een rol in Azure-Cloudservices via PowerShell"
description="Het configureren van uw azure cloud-servicetoepassing PowerShell gebruiken voor het externe bureaublad verbindingen toestaan"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure-Cloudservices via PowerShell

>[AZURE.SELECTOR]
- [Azure klassieke portal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Extern bureaublad kunt u toegang tot het bureaublad van een rol uitgevoerd in Azure wordt aangegeven. U kunt een verbinding met extern bureaublad gebruiken voor het oplossen van problemen met uw toepassing terwijl deze wordt uitgevoerd.

In dit artikel wordt beschreven hoe extern bureaublad op uw Cloud Service rollen via PowerShell inschakelen. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md) op de vereisten die u nodig hebt voor dit artikel. PowerShell maakt gebruik van de externe bureaublad extensie zodat u extern bureaublad inschakelen kunt nadat de toepassing wordt geïmplementeerd.


## <a name="configure-remote-desktop-from-powershell"></a>Extern bureaublad van PowerShell configureren

De cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) kunt u extern bureaublad op opgegeven rollen of alle functies van de implementatie van de service cloud wilt inschakelen. De cmdlet kunt u de gebruikersnaam en wachtwoord opgeven voor de externe bureaublad gebruiker via de *referentie* -parameter die een object PSCredential accepteert.

Als u PowerShell interactief gebruikt, kunt u eenvoudig het PSCredential-object instellen door de ondersteuning voor de cmdlet [Get-referenties](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

Deze opdracht wordt het dialoogvenster zodat u kunt de gebruikersnaam en wachtwoord invoeren voor de externe gebruiker op een veilige manier weergegeven.

Aangezien PowerShell in automatisering scenario's helpt, kunt u ook het object **PSCredential** op een manier die geen interactie van de gebruiker nodig instellen. Eerst moet u een wachtwoord beveiligde instellen. U begint met het opgeven van een wachtwoord tekst zonder opmaak converteren naar een beveiligde tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Vervolgens moet u deze secure tekenreeks converteren naar een versleutelde standaard tekenreeks [ConverterenVan-SecureString](https://technet.microsoft.com/library/hh849814.aspx)gebruiken. Nu kunt u deze versleutelde standaard tekenreeks opslaan naar een bestand met [Set-inhoud](https://technet.microsoft.com/library/ee176959.aspx).

U kunt ook een beveiligde-wachtwoordbestand maken zodat u niet hoeft te typen in het wachtwoord elke keer. Een bestand beveiligd-wachtwoordverificatie is ook beter dan een bestand met tekst zonder opmaak. Het volgende PowerShell gebruiken om een beveiligde-wachtwoordbestand te maken:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Wanneer u het wachtwoord instelt, zorg dat u aan de [complexiteitsvereisten](https://technet.microsoft.com/library/cc786468.aspx)voldoet.

U maakt de referentie-object uit het bestand beveiligd-wachtwoordverificatie, moet u inhoud van het bestand te lezen en ze terug converteren naar een beveiligde tekenreeks [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)met.

De cmdlet [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) accepteert ook een parameter *Verlooptijd* , waarmee een **datum/tijd** waarop het gebruikersaccount verloopt. U kunt bijvoorbeeld de account verloopt een paar dagen vanaf de huidige datum en tijd instellen.

In dit voorbeeld PowerShell ziet u hoe u het externe bureaublad-extensie instellen op een cloudservice:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
U kunt desgewenst ook de implementatie slot en functies die u wilt inschakelen voor extern bureaublad opgeven. Als deze parameters zijn opgegeven, kan de cmdlet extern bureaublad op alle rollen in de **productie** implementatie slot.

De extensie extern bureaublad is gekoppeld aan een implementatie. Als u een nieuwe implementatie voor de service maakt, moet u extern bureaublad in deze implementatie inschakelen. Als u altijd hebben van extern bureaublad is ingeschakeld wilt, klikt u rekening moet houden integratie van de PowerShell-scripts in uw implementatie-werkstroom.


## <a name="remote-desktop-into-a-role-instance"></a>Extern bureaublad in een exemplaar van de rol
De cmdlet [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) wordt gebruikt voor extern bureaublad in een specifieke rol exemplaar van uw cloudservice. U kunt de parameter *LocalPath* lokaal het RDP-bestand wilt downloaden. Of u kunt de parameter *starten* aan het dialoogvenster verbinding met extern bureaublad voor toegang tot het exemplaar van de rol cloud rechtstreeks te starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Controleren of extern bureaublad extensie is ingeschakeld op een service
De cmdlet [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) worden weergegeven die extern bureaublad is ingeschakeld of uitgeschakeld op een service-implementatie. De cmdlet retourneert de gebruikersnaam voor de externe bureaublad gebruiker en de rollen die het externe bureaublad extensie is geconfigureerd voor gebruik. Standaard dit gebeurt op de implementatie-slot en kunt u kiezen om in plaats hiervan het tijdelijk opslaan slot gebruiken.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Extern bureaublad extensie verwijderen uit een service
Als u de externe bureaublad extensie op een implementatie al hebt ingeschakeld en het externe bureaublad instellingen moet bijwerken, moet u eerst de extensie verwijderen. En deze opnieuw inschakelen met de nieuwe instellingen. Stel dat u wilt instellen, een nieuw wachtwoord voor de externe gebruikersaccount of het account is verlopen. Dit is vereist voor bestaande implementaties met het externe bureaublad extensie ingeschakeld. Voor nieuwe implementaties, kunt u de extensie gewoon rechtstreeks toepassen.

Als u wilt het externe bureaublad extensie verwijderen uit de implementatie, kunt u de cmdlet [Verwijderen-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) . U kunt desgewenst ook de implementatie slot en de rol van waaruit u wilt verwijderen van het externe bureaublad extensie opgeven.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Om de configuratie van de extensie volledig te verwijderen, moet u de cmdlet *verwijderen* met de parameter **UninstallConfiguration** bellen.
>
>De parameter **UninstallConfiguration** Hiermee verwijdert u eventuele extensie-configuratie die is toegepast op de service. De configuratie van elke extensie is gekoppeld aan de service te configureren. De extensie verwijderen bellen de cmdlet *verwijderen* zonder **UninstallConfiguration** instelt, scheidt u de <mark>implementatie</mark> van de extensie-configuratie, dus effectief. De configuratie van de extensie blijft echter gekoppeld aan de service.



## <a name="additional-resources"></a>Aanvullende informatie

[Het configureren van Cloudservices](cloud-services-how-to-configure.md)
