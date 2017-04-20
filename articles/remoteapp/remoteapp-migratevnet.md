<properties
    pageTitle="Het migreren van een RemoteApp VNET naar een Azure-VNET | Microsoft Azure"
    description="Meer informatie over het migreren van een RemoteApp VNET naar een Azure-VNET"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Het migreren van een siteverzameling hybride van een RemoteApp VNET naar een Azure-VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Goed nieuws! We hebt u aan het implementeren van hybride RemoteApp verzamelingen rechtstreeks aan op uw bestaande Azure virtuele netwerken (VNETs) in plaats van RemoteApp-specifieke VNETs maken ingeschakeld. Hiermee kunt u profiteren van de nieuwste VNET-functies (zoals ExpressRoute) en uw hybride verzamelingen direct netwerktoegang geven tot andere Azure services en virtuele machines geïmplementeerd in die VNET.  (Dit krijgt u betere prestaties en eenvoudiger instellen dan VNET-naar-VNET configuraties).


Stel dat u al een hybride RemoteApp-siteverzameling genoemd *OriginalCollection* met een RemoteApp VNET genaamd *RemoteAppVNET*hebt gemaakt. Hier vindt u de stappen om te migreren naar een nieuwe Azure VNET *AzureVNET*genoemd.

1.  Klik op het tabblad **netwerken te gebruiken** in de [beheerportal](http://manage.windowsazure.com/)maken een VNET *AzureVNET*, met de dezelfde locatie, DNS-configuratie en -adresruimte (voor ten minste één van de subnetten *AzureVNET* ) als u gebruikt voor *RemoteAppVNET*genoemd.
2.  Configureert *AzureVNET* aan beide host of netwerkverbinding de Active Directory-implementatie die *OriginalCollection* domein lid is.
3.  Klik op het tabblad **RemoteApps** door een nieuwe RemoteApp-verzameling genoemd *Nieuwe siteverzameling*te maken. (Gebruik de optie **maken met VNET** , niet **Snelle maken**.)
3.  Configureer *NewCollection* op een subnet in *AzureVNET*kunnen worden geïmplementeerd.
4.  *NewCollection* voor het gebruik van de afbeelding van dezelfde en domein join informatie als u gebruikt voor *OriginalCollection*configureren.
5.  Na een paar uur, wordt *NewCollection* weergegeven in uw lijst met collecties met een actieve status.

Nu, als u niet nodig hebt gebruikersinformatie migreren van de oorspronkelijke siteverzameling naar de nieuwe siteverzameling, werk deze stappen:

6.  *OriginalCollection*verwijderen.
7.  *RemoteAppVNET*verwijderen.

En u bent klaar!

U kunt ook als u migreren van gebruikersgegevens van de oorspronkelijke siteverzameling naar de nieuwe siteverzameling wilt, werk deze stappen:

6.  Een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) met uw Azure abonnements-ID, de naam van uw oorspronkelijke siteverzameling en de naam van de nieuwe verzameling, en vraag of uw gebruikersgegevens migreren.
7.  Binnen 2 werkdagen gaat het team RemoteApp de lijst voor gebruikerstoegang en alle gebruikersdocumenten en gebruikersinstellingen van de oorspronkelijke siteverzameling naar de nieuwe siteverzameling.
8.  *OriginalCollection*verwijderen.
9.  *RemoteAppVNET*verwijderen.

En nu u bent klaar!

Als u vragen hebt of speciale hulp nodig hebt, neemt u via e-mail verzenden [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
