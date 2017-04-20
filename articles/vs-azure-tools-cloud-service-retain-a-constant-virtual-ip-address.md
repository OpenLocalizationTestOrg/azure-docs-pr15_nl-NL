<properties
   pageTitle="Hoe moeten worden bewaard een constant virtuele IP-adres van een cloudservice | Microsoft Azure"
   description="Leer hoe u ervoor zorgen dat het virtuele IP-adres (VIP) van uw Azure cloudservice wordt niet gewijzigd."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Hoe u een constant virtuele IP-adres van een cloudservice behouden

Wanneer u een cloudservice die wordt gehost in Azure bijwerkt, moet u mogelijk om ervoor te zorgen dat het virtuele IP-adres (VIP) van de service wordt niet gewijzigd. Veel domain management-services gebruiken de Domain Name System (DNS) voor het registreren van domeinnamen. DNS werkt alleen als de VIP dezelfde blijft. U kunt de **Wizard publiceren** in hulpmiddelen voor Azure om ervoor te zorgen dat de VIP van uw cloudservice niet wordt gewijzigd wanneer u deze bijwerken. Zie [een aangepaste domeinnaam voor een Azure cloudservice configureren](./cloud-services/cloud-services-custom-domain-name.md)voor meer informatie over het gebruik van DNS-domeinbeheer voor cloudservices.

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Een cloudservice publiceren zonder te wijzigen van de VIP

De VIP van een cloudservice wordt toegewezen wanneer u eerst het dashboard naar Azure in een bepaalde omgeving, zoals de productieomgeving implementeren. De VIP wordt niet gewijzigd tenzij u de implementatie expliciet verwijderen of deze impliciet door het updateproces implementatie is verwijderd. Als u wilt behouden de VIP, moet u uw implementatie niet verwijderen, en u moet ook voor zorgen dat Visual Studio uw implementatie automatisch niet verwijderen. U kunt het gedrag bepalen door het opgeven van de implementatie-instellingen in de **Wizard publiceren**die ondersteuning biedt voor verschillende implementatieopties. U kunt een vers implementatie of een update-implementatie, die incrementele of tegelijk worden kan, opgeven en beide soorten update implementaties behouden de VIP. Zie voor definities van deze verschillende soorten implementatie, [Azure Application Wizard publiceren](vs-azure-tools-publish-azure-application-wizard.md).  Bovendien kunt u bepalen of de vorige implementatie van een cloudservice wordt verwijderd als er een fout optreedt. De VIP mogelijk onverwacht wijzigen als u kunt deze optie niet correct ingesteld.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Naar een cloudservice zonder te wijzigen van de VIP bijwerken

1. Nadat u uw cloudservice ten minste eenmaal implementeert, opent u het snelmenu voor het knooppunt voor uw Azure project en kies vervolgens **publiceren**. De wizard **Publiceren Azure-toepassing** wordt weergegeven.

1. In de lijst met abonnementen, kiest u de presentatie waaraan u wilt implementeren en kies vervolgens de knop **volgende** . De pagina **Instellingen** van de wizard wordt weergegeven.

1. Klik op het tabblad **Algemene instellingen** controleren of de naam van de cloudservice waarop u implementeert bent, de **omgeving**, de **Configuratie maken**en de **Configuratie van de Service** alle juist zijn.

1. Klik op het tabblad **Advanced Settings** Controleer dat het opslag-account en de implementatie-label, dat het selectievakje **implementatie op mislukt verwijderen** is uitgeschakeld en dat het selectievakje bijwerken voor **implementatie** is geselecteerd. U selecteert het selectievakje bijwerken voor **implementatie** , zorgt u ervoor dat uw implementatie niet verwijderd en uw VIP niet verloren wanneer u uw toepassing publiceren. Als u het **verwijderen van de implementatie op het selectievakje mislukt**uitschakelt, zorgt u ervoor dat uw VIP niet verloren is als er een fout optreedt tijdens de implementatie.

1. Als verder wilt opgeven hoe u de rollen moeten worden bijgewerkt, kiest u de koppeling **Instellingen** naast het vak **implementatie bijwerken** en kies vervolgens de incrementele update of de optie tegelijk bijwerken in het dialoogvenster Instellingen voor **implementatie bijwerken** . Als u incrementele update kiest, elk exemplaar wordt bijgewerkt achter elkaar, zodat de toepassing altijd beschikbaar is. Als u gelijktijdig bijwerken kiest, worden alle instanties op hetzelfde moment bijgewerkt. Sneller tegelijk bijwerken is, maar uw service mogelijk niet beschikbaar tijdens het updateproces.

1. Wanneer u tevreden met uw instellingen bent, kiest u de knop **volgende** .

1. Controleer de instellingen op de pagina **Overzicht** van de wizard en kies vervolgens de knop **publiceren** .

  >[AZURE.WARNING] Als de implementatie is mislukt, moet u adres waarom dit is mislukt en implementeer deze opnieuw direct, om te voorkomen dat uw cloudservice verlaten beschadigd raakt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over publiceren naar Azure van Visual Studio, raadpleegt u de [wizard Publiceren Azure-toepassing](vs-azure-tools-publish-azure-application-wizard.md).
