<properties
    pageTitle="Beleid toepassen op Azure resourcemanager virtuele Machines | Microsoft Azure"
    description="Hoe u een beleid toepast op een Azure resourcemanager virtuele Windows-computer"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Beleid naar Azure resourcemanager virtuele Machines toepassen

Met beleid kan het afdwingen van een organisatie verschillende conventies en regels in de gehele organisatie. Afdwingen van het gewenste gedrag kunt risico terwijl die bijdragen aan het succes van de organisatie. In dit artikel wordt beschreven hoe u Azure resourcemanager beleid kunt gebruiken om te bepalen van het gewenste gedrag voor van uw organisatie virtuele Machines.

Het overzicht voor de stappen hiervoor is als onder

1. Azure resourcemanager beleid 101
2. Een beleid voor uw VM definiëren
3. Het beleid maken
4. Het beleid van toepassing

## <a name="azure-resource-manager-policy-101"></a>Azure resourcemanager beleid 101

Voor het aan de slag met Azure resourcemanager beleidsregels, wordt aangeraden het onderstaande artikel lezen en vervolgens kunt verdergaan met de stappen in dit artikel. De onderstaande artikel worden de eenvoudige definitie en de structuur van een beleid hoe beleid krijgen geëvalueerd en biedt u verschillende voorbeelden van beleidsdefinities.

* [Beleid voor het beheren van resources en toegang gebruiken](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Een beleid voor uw VM definiëren

Een van de gebruikelijke scenario's voor een onderneming mogelijk alleen hun gebruikers toestaan deel te maken van virtuele Machines van specifieke besturingssystemen die zijn getest voor compatibiliteit met een LOB-toepassing. Een beleid Azure resourcemanager met deze taak kan worden uitgevoerd in een paar stappen. In dit voorbeeld beleid gaan we toe te staan dat alleen Windows Server 2012 R2 Datacenter virtuele Machines moet worden gemaakt. De definitie beleid eruit onder

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Het bovenstaande beleid kan eenvoudig worden gewijzigd met een scenario waarin u toestaan van Windows Server Datacenter afbeelding wilt mogelijk moet worden gebruikt voor de implementatie van een virtuele machines met de hieronder wijzigen

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>VM eigenschapsvelden

De onderstaande tabel worden de eigenschappen van het virtuele Machine die kunnen worden gebruikt als velden in de definitie van uw beleid. Zie het artikel hieronder voor meer informatie over velden voor het beleid:

* [Velden en bronnen](../resource-manager-policy.md#fields-and-sources)


| Veldnaam     | Beschrijving                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Hiermee geeft u de uitgever van de afbeelding               |
| imageOffer     | Hiermee geeft u de aanbieding voor de afbeelding van de door u gekozen publisher |
| imageSku       | Hiermee geeft u de SKU voor de door u gekozen aanbieding             |
| imageVersion   | Hiermee wordt de van de afbeeldingsversie voor de gekozen SKU     |

## <a name="create-the-policy"></a>Het beleid maken

Een beleid kan eenvoudig worden gemaakt met de REST API rechtstreeks of de PowerShell-cmdlets. Voor het maken van het beleid, raadpleegt u het artikel hieronder:

* [Een beleid maken](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Het beleid van toepassing

Na het maken van het beleid moet u deze hebt toegepast op een gedefinieerde bereik. Het bereik is een abonnement, resourcegroep of zelfs de resource. Voor het toepassen van het beleid, raadpleegt u het artikel hieronder:

* [Een beleid maken](../resource-manager-policy.md#applying-a-policy)
