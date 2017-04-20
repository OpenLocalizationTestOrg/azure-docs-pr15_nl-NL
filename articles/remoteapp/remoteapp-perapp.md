<properties
   pageTitle="Toepassingen voor afzonderlijke gebruikers publiceren in een verzameling Azure RemoteApp (Preview) | Microsoft Azure"
   description="Leer hoe u apps voor afzonderlijke gebruikers, in plaats van kunt publiceren afhankelijk van groepen in Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Toepassingen voor afzonderlijke gebruikers publiceren in een verzameling Azure RemoteApp (Preview)

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Dit artikel wordt uitgelegd hoe u toepassingen voor afzonderlijke gebruikers in een verzameling Azure RemoteApp publiceert. Dit is de nieuwe functies in Azure RemoteApp, die momenteel in "privé preview" en alleen beschikbaar voor vroege gebruikers voor evaluatie selecteren.

Oorspronkelijk Azure RemoteApp slechts één manier van "publicerende"-toepassingen zijn ingeschakeld: de beheerder apps van de afbeelding wilt publiceren en ze is zichtbaar voor alle gebruikers in de siteverzameling.

Een gebruikelijk is veel toepassingen opnemen in één afbeelding en implementeren van een siteverzameling om te kunnen de beheerkosten verlagen. Vaak is het mogelijk niet alle toepassingen relevant zijn voor alle gebruikers: beheerders liever apps voor afzonderlijke gebruikers publiceren, zodat ze onnodige toepassingen in de feed toepassing niet wordt weergegeven.

Dit is momenteel nu mogelijk in Azure RemoteApp – als een beperkte voorbeeldfunctie. Hier volgt een kort overzicht van de nieuwe functionaliteit voor:

1. Een siteverzameling kan worden ingesteld in twee modi:
 
  - de oorspronkelijke "modus voor het verzamelen", waarbij alle gebruikers in een siteverzameling kunnen zien gepubliceerde alle toepassingen. Dit is de standaardmodus.
  - de nieuwe 'toepassingsmodus', expliciet gebruikers zien waar alleen toepassingen die zijn toegewezen

2. Op het moment dat kan de toepassingsmodus alleen worden ingeschakeld met Azure RemoteApp PowerShell-cmdlets.

  - Als de waarde naar de toepassingsmodus, kan geen Gebruikerstoewijzing in de siteverzameling worden beheerd via de portal van Azure. Gebruikerstoewijzing moet worden beheerd via PowerShell-cmdlets.

3. Gebruikers zien alleen de toepassingen die rechtstreeks naar deze gepubliceerd. Echter het nog steeds mogelijk voor een gebruiker aan de andere toepassingen die beschikbaar zijn op de afbeelding via deze rechtstreeks in het besturingssysteem starten.
  - Deze functie biedt geen een beveiligde vergrendeling van toepassingen. alleen beperkt zicht van de in de toepassing feed.
  - Als u gebruikers vanuit toepassingen isoleren wilt, moet u afzonderlijke siteverzamelingen voor die gebruiken.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Hoe kom ik aan Azure RemoteApp PowerShell-cmdlets

Om te proberen de nieuwe functionaliteit voor de Preview-versie, moet u Azure PowerShell-cmdlets gebruiken. Dit is momenteel niet mogelijk de beheerportal van Azure gebruiken om te schakelen van de nieuwe publicatie modus van toepassing.

Controleer eerst of dat u hebt de [Azure PowerShell-module](../powershell-install-configure.md) hebt geïnstalleerd.

Vervolgens de PowerShell-console in de beheerdersmodus voor starten en de volgende cmdlet uitvoeren:

        Add-AzureAccount

U wordt gevraagd om uw Azure gebruikersnaam en wachtwoord. Zodra aangemeld, is mogelijk om uit te voeren Azure RemoteApp-cmdlets ten opzichte van uw Azure-abonnementen.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Hoe Controleer welke modus per siteverzameling bevindt zich in

De volgende cmdlet uitvoeren:

        Get-AzureRemoteAppCollection <collectionName>

![De modus van de siteverzameling controleren](./media/remoteapp-perapp/araacllelvel.png)

De eigenschap AclLevel kan hebben de volgende waarden:

- Verzameling: de oorspronkelijke publicerende modus. Alle gebruikers zien dat alle gepubliceerde apps.
- Toepassing: de nieuwe publicatie modus. Gebruikers zien alleen de apps die rechtstreeks naar deze gepubliceerd.

## <a name="how-to-switch-to-application-publishing-mode"></a>Overschakelen naar de publicatie van toepassing-modus

De volgende cmdlet uitvoeren:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Toepassing publicerende staat blijft: in eerste instantie zichtbaar voor alle gebruikers alle van de oorspronkelijke gepubliceerde apps.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Hoe u gebruikers aan wie een specifieke toepassing kunnen zien

De volgende cmdlet uitvoeren:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Hier ziet u alle gebruikers die de toepassing kunnen zien.

Opmerking: U kunt de toepassing aliassen ('app alias' in de syntaxis van de bovenstaande genoemd) zien door te voeren Get-AzureRemoteAppProgram - NaamVerzameling <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Het toewijzen van een toepassing aan een gebruiker

De volgende cmdlet uitvoeren:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

De gebruiker ziet nu de toepassing in de Azure RemoteApp-client en kan tot stand te brengen.

## <a name="how-to-remove-an-application-from-a-user"></a>Hoe u een toepassing van een gebruiker intrekken

De volgende cmdlet uitvoeren:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Feedback leveren
Waarderen we uw feedback en suggesties over deze functie preview. Vul de [enquête](http://www.instant.ly/s/FDdrb) te laten weten wat u ervan vindt.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>U kunt er nog niet naar de voorbeeldfunctie uitproberen?
Als u niet heeft deelgenomen de voorbeeldversie nog, gebruikt u deze [enquête](http://www.instant.ly/s/AY83p) toegang aanvragen.
