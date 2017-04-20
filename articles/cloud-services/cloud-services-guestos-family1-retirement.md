<properties
   pageTitle="Zoals u ziet, Gast OS familie 1 buitengebruikstelling | Microsoft Azure"
   description="Vindt u informatie over wanneer de buitengebruikstelling Azure Gast OS familie 1 is er gebeurd en hoe u bepaalt als u ondervindt"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Gast OS familie 1 buitengebruikstelling kennisgeving

De buitengebruikstelling van OS familie 1 is eerst aangekondigd op 1 juni 2013.

**2 sep 2014** Het besturingssysteem Azure gast (Gast-besturingssysteem) familie 1.x, die is gebaseerd op het Windows Server 2008-besturingssysteem, is officieel buiten gebruik. Alle pogingen implementeren van nieuwe services of het bijwerken van bestaande services met behulp van familie 1, mislukt met een foutbericht waarin wordt gemeld dat de Gast OS familie 1 is teruggetrokken.

**3 november 2014** Uitgebreide ondersteuning voor gast OS familie 1 beëindigd en deze volledig is teruggetrokken. Alle services nog steeds op familie 1 worden beïnvloed. We kunnen deze services op elk gewenst moment stoppen. Er is geen garantie die uw services actief blijft, tenzij u handmatig een upgrade ze zelf uitvoert.

Als u extra vragen hebt, gaat u naar de [Cloud Services-Forums](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) of [Neem contact op met ondersteuning voor Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Worden herkend?

Uw Cloudservices worden beïnvloed als een van de volgende handelingen van toepassing is:

1. U hebt een waarde van "osFamily ="1"expliciet is opgegeven in het bestand ServiceConfiguration.cscfg voor uw Cloudservice.
2. U hoeft niet een waarde voor osFamily expliciet is opgegeven in het bestand ServiceConfiguration.cscfg voor uw Cloudservice. Op dit moment het systeem wordt gebruikt de standaardwaarde van '1' in dit geval.
3. De portal van Azure klassieke bevat uw besturingssysteem Gast family waarde als "Windows Server 2008".

Als u wilt zoeken waarop van uw cloudservices welke familie OS worden uitgevoerd, kunt u het volgende script uitvoeren in Azure PowerShell, hoewel u eerst de volgende [Azure PowerShell instellen](../powershell-install-configure.md) . Zie voor meer informatie over het script, [Azure Gast OS familie 1 einde van levenscyclus: juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Uw cloudservices worden beïnvloed door buitengebruikstelling OS familie 1 als de kolom osFamily in de script-uitvoer leeg is of bevat een '1'.

## <a name="recommendations-if-you-are-affected"></a>Aanbevelingen als u ondervindt

Het is raadzaam om dat u uw rollen Cloudservice migreren naar een van de ondersteunde Gast OS gezinnen:

**Gast OS family 4.x** -Windows Server 2012 R2 *(aanbevolen)*

1. Zorg ervoor dat uw toepassing SDK 2.1 of hoger met .NET framework 4.0, 4.5 of 4.5.1 gebruikt.
2. Stel het kenmerk osFamily naar '4' in het bestand ServiceConfiguration.cscfg en implementeer deze opnieuw uw cloudservice.


**Gast OS family 3.x** -Windows Server 2012

1. Zorg ervoor dat uw toepassing SDK 1,8 of hoger met .NET framework 4.0 of 4.5 gebruikt.
2. Stel het kenmerk osFamily naar '3' in het bestand ServiceConfiguration.cscfg en implementeer deze opnieuw uw cloudservice.


**Gast OS family 2.x** -Windows Server 2008 R2

1. Zorg ervoor dat uw toepassing SDK 1.3 gebruikt en hoger met .NET framework 3.5 of 4.0.
2. Stel het kenmerk osFamily naar '2' in het bestand ServiceConfiguration.cscfg en implementeer deze opnieuw uw cloudservice.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Uitgebreide ondersteuning voor gast OS familie 1 beëindigd 3 november 2014
Cloudservices op Gast OS familie 1 worden niet meer ondersteund. Migreer uitschakelen familie 1 zo snel mogelijk om te voorkomen dat service-onderbreking.  

## <a name="next-steps"></a>Volgende stappen
Bekijk de meest recente [Gast OS loslaat](cloud-services-guestos-update-matrix.md).
