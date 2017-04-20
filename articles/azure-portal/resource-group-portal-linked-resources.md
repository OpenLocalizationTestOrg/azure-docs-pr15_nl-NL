<properties 
    pageTitle="Gerelateerde en gekoppelde bronnen in de galerie tegel" 
    description="Meer informatie over gerelateerde en gekoppelde resources die worden weergegeven in de galerie tegel van de portal Azure preview." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Gerelateerde en gekoppelde bronnen in de galerie tegel

De galerie tegel kunt u de tegels voor een bepaalde bron zoeken en sleep ze naar uw huidige blade. Via de galerie met tegel, kunt u management weergaven die resources omvatten maken. Voor een opgegeven resource zijn de verwante resources alle bronnen die delen dezelfde resourcegroep als de resource en resources die zijn gekoppeld aan of de resource.

## <a name="linked-resources-in-azure-resource-manager"></a>Gekoppelde bronnen in Azure resourcemanager

Koppelen is een functie van de Azure Resource Manager.  U kunt relaties tussen resources declareren, zelfs als ze zich niet in dezelfde resourcegroep. Koppelen, heeft geen invloed op de runtime van uw bronnen, geen invloed op facturering en geen invloed op rollen gebaseerde access.  Het is eenvoudig een instrument die u gebruiken kunt om aan te geven van relaties zodat hulpprogramma's zoals de tegel galerie kan een uitgebreide beheer bieden optreden.  De hulpmiddelen voor kunnen controleren van de koppelingen met behulp van de koppelingen API en geef aangepaste relatiebeheer ervaringen ook. 

## <a name="how-do-i-link-my-resources"></a>Hoe kan ik mijn resources koppelen?

Wanneer u resources via de portal of met het implementeren van een sjabloon via Azure PowerShell of Azure CLI maakt, worden koppelingen worden automatisch gemaakt voor sommige afhankelijke resources. U kunt ook via programmacode resources koppelen met behulp van de [Gekoppelde Resources REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx) of door de relaties in de sjabloon declareren. Zie voor een beschrijving van het werken met gekoppelde resources, [Linking resources in Azure resourcemanager](../resource-group-link-resources.md).

## <a name="next-steps"></a>Volgende stappen

- Als u een introductie over het schrijven van Azure resourcemanager sjablonen nodig hebt, raadpleegt u [ontwerpfuncties sjablonen](../resource-group-authoring-templates.md).
- Zie [Linking resources in Azure resourcemanager](../resource-group-link-resources.md)om te zoomen op in de meer details over het maken van koppelingen tussen resources.
- Zie informatie over meer over het werken met resourcegroepen tot en met de preview-portal [met behulp van de Azure Preview-Portal als u wilt uw Azure resources beheren](resource-group-portal.md).
