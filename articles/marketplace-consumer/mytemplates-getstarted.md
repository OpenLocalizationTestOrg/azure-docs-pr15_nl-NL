<properties
   pageTitle="Aan de slag met persoonlijke sjablonen | Microsoft Azure"
   description="Toevoegen, beheren en delen van uw persoonlijke sjablonen met de portal van Azure, de Azure CLI of PowerShell."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Aan de slag met persoonlijke sjablonen op de Azure-Portal

Een sjabloon [Azure resourcemanager](../resource-group-authoring-templates.md) is een declaratieve sjabloon gebruikt om te definiëren, uw implementatie. U kunt de bronnen voor het implementeren voor een oplossing definiëren en geef parameters en variabelen waarmee u kunt invoerwaarden voor verschillende omgevingen. De sjabloon bestaat uit JSON en expressies die u gebruiken kunt om samen te stellen van de waarden voor de implementatie.

U kunt de nieuwe **sjablonen** mogelijkheid in de [Portal van Azure](https://portal.azure.com) samen met de provider van de resource **Microsoft.Gallery** gebruiken als een uitbreiding van de [Azure Marketplace](https://azure.microsoft.com/marketplace/) zodat gebruikers kunnen maken, beheren en implementeren van persoonlijke sjablonen van een persoonlijke bibliotheek.

In dit document begeleidt u door toe te voegen, beheren en delen van een persoonlijke **sjabloon** met behulp van de Azure-Portal.

## <a name="guidance"></a>Richtlijnen

De volgende suggesties kunt u profiteren van **sjablonen** tijdens het werken met uw oplossingen:

- Een **sjabloon** is een encapsulating bron met een resourcemanager sjabloon en een extra metagegevens. Het lijkt veel op dezelfde manier zeer naar een item in de Marketplace. Het belangrijkste verschil is dat dit een privé-item in plaats van de openbare Marketplace-items is.
- De bibliotheek **sjablonen** werkt ook voor gebruikers die u nodig hebt om aan te passen hun implementaties.
- **Sjablonen** goed werkt voor gebruikers die een eenvoudige opslagplaats binnen Azure nodig hebben.
- Begin met een bestaande resourcemanager-sjabloon. Sjablonen voor zoeken in [github](https://github.com/Azure/azure-quickstart-templates) of [sjabloon exporteren](../resource-manager-export-template.md) uit een bestaande resourcegroep.
- **Sjablonen** zijn gekoppeld aan de gebruiker die ze publiceert. Naam van de uitgever is zichtbaar voor iedereen die toegang gelezen heeft.
- **Sjablonen** resourcemanager resources zijn en kunnen niet worden gewijzigd eenmaal gepubliceerd.

## <a name="add-a-template-resource"></a>Een resource sjabloon toevoegen

Er zijn twee manieren een bron **sjabloon** maken in de portal van Azure.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Methode 1: Een nieuwe sjabloon resource uit een actieve resourcegroep maken

1. Navigeer naar een bestaande resourcegroep op de Azure-Portal. Selecteer de **sjabloon exporteren** in **Instellingen**.
2. Nadat de sjabloon resourcemanager is geëxporteerd, gebruikt u de knop **Opslaan sjabloon** opslaan naar de bibliotheek **sjablonen** . Meer volledige informatie over het exporteren sjabloon [hier](../resource-manager-export-template.md).
<br /><br />
![Resource groep exporteren](media/rg-export-portal1.PNG)  <br />

3. Selecteer de knop **opslaan op de sjabloon** .
<br /><br />

4. Voer de volgende gegevens:

    - Naam: de naam van het sjabloonobject (NOTITIE: dit is de naam van een Azure-resourcemanager die is gebaseerd. Alle naming beperkingen zijn van toepassing en kan niet worden gewijzigd nadat gemaakt).
    - Omschrijving: kort overzicht over de sjabloon.

    ![Sjabloon opslaan](media/save-template-portal1.PNG)  <br />

5. Klik op **Opslaan**.

    > [AZURE.NOTE] Het blad van de sjabloon exporteren ziet u meldingen wanneer de geëxporteerde resourcemanager-sjabloon fouten bevat, maar u nog steeds kunnen opslaan in deze sjabloon voor een Resource Manager de sjablonen. Zorg ervoor dat u bezoekt en eventuele resourcemanager sjabloon problemen oplossen voordat u de geëxporteerde resourcemanager sjabloon opnieuw te distribueren.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B TE DRUKKEN. Methode 2: Een nieuwe sjabloon resource uit bladeren toevoegen

U kunt ook een nieuwe **sjabloon** toevoegen vanuit kladgebied met de + opdrachtknop toevoegen in **Bladeren > sjablonen**. U moet een naam, beschrijving en de sjabloon resourcemanager JSON op te geven.

![Sjabloon toevoegen](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery is dat een Tenant op basis van Azure resource provider. De sjabloon resource is gekoppeld aan de gebruiker die deze heeft gemaakt. Het is niet gekoppeld aan een specifieke abonnement. Een abonnement moet worden gekozen alleen bij de implementatie van een sjabloon.

## <a name="view-template-resources"></a>Sjabloon voor resources weergeven

Alle **sjablonen** beschikbaar zijn voor u is zichtbaar bij **Bladeren > sjablonen**. Dit geldt ook voor **sjablonen** die u hebt gemaakt en de randen die met u zijn gedeeld met verschillende machtigingsniveaus. Meer details in de sectie [toegangsbeheer](#access-control-for-a-tenant-resource-provider) hieronder.

![Sjabloon weergeven](media/view-template-portal1.PNG)  <br />

U kunt de details van een **sjabloon** weergeven door te klikken op in een item in de lijst.

![Sjabloon weergeven](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Een resource sjabloon bewerken

U kunt de stroom bewerken voor een **sjabloon** starten door te klikken met de rechtermuisknop op het item in de lijst bladeren of met de knop van de opdracht bewerken.

![Sjabloon bewerken](media/edit-template-portal1a.PNG)  <br />

U kunt de beschrijving of resourcemanager sjabloontekst bewerken. U kunt de naam niet bewerken, omdat deze de resourcenaam van een Resource Manager. Wanneer u de Resource Manager sjabloon bewerken JSON we worden gevalideerd om ervoor te zorgen dat de JSON geldig is. Kies **OK** en vervolgens op **Opslaan** om op te slaan de bijgewerkte sjabloon.

![Sjabloon bewerken](media/edit-template-portal2a.PNG)  <br />

U ziet een bevestingsmelding zodra de **sjabloon** is opgeslagen.

![Sjabloon bewerken](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Een resource sjabloon implementeren

U kunt een **sjabloon** die u op **Leesmachtigingen** implementeren. De implementatie-stroom Hiermee start u het blad voor implementatie van standaard Azure-sjabloon. Vul de waarden voor de sjabloonparameters resourcemanager om verder te gaan met de implementatie.

![Sjabloon implementeren](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Een sjabloon-resource deelt

Een resource **sjabloon** kan worden gedeeld met andere gebruikers. Delen lijkt veel op dezelfde manier naar [roltoewijzing voor elke bron op Azure](../active-directory/role-based-access-control-configure.md). De eigenaar van de **sjabloon** biedt machtigingen aan andere gebruikers die u kunnen werken met een sjabloon resource. De persoon of groep van personen waarmee die u de **sjabloon** met delen kunnen om de sjabloon resourcemanager en de galerie-eigenschappen weer te geven.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Toegangsbeheer voor de resources Microsoft.Gallery

Rol | Machtigingen
---|----
Eigenaar | Volledig beheer op de sjabloon resource inclusief delen, kunt
Lezer | Lees- en Execute(Deploy) kunnen op de sjabloon resource
Inzender | Klik op de sjabloon resource, kunt machtiging voor bewerken en verwijderen. Gebruiker delen niet de sjabloon met anderen

Selecteer **delen** op het item bladeren met de rechtermuisknop te klikken of op het blad weergave van een specifiek item. Hiermee wordt een ervaring delen gestart.

![Sjabloon delen](media/share-template-portal1a.png)  <br />

 U kunt nu een rol en een gebruiker of groep voor toegang tot een bepaalde **sjabloon**kiezen. De beschikbare rollen zijn eigenaar, Reader en Inzender. Meer details in de sectie [toegangsbeheer](#access-control-for-a-tenant-resource-provider) hierboven.

![Sjabloon delen](media/share-template-portal2b.png)  <br />

![Sjabloon delen](media/share-template-portal3b.png)  <br />

Klik op **selecteren** en **Ok**. U ziet nu de gebruikers of groepen die u hebt toegevoegd aan de resource.

![Sjabloon delen](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Een sjabloon kan alleen worden gedeeld met gebruikers en groepen in dezelfde tenant Azure Active Directory. Als u een sjabloon met een e-mailadres dat zich niet in uw tenant delen, een uitnodiging ontvangt waarin de gebruiker wordt gevraagd te nemen aan de tenant als gast.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van sjablonen van de Resource Manager, [ontwerpfuncties sjablonen](../resource-group-authoring-templates.md)
- Als u wilt weten over de functies die u in een sjabloon resourcemanager gebruiken kunt, Zie [sjabloon functies](../resource-group-template-functions.md)
- Zie [Aanbevolen procedures voor het ontwerpen van Azure resourcemanager sjablonen](../best-practices-resource-manager-design-templates.md) voor hulp bij het ontwerpen van uw sjablonen,
