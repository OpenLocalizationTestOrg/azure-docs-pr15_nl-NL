<properties
   pageTitle="Toegang krijgen tot Azure virtuele Machines in Server Explorer | Microsoft Azure"
   description="Krijg een overzicht van hoe u kunt bekijken, maken en beheren van Azure virtuele machines (VMs) in Server Explorer in Visual Studio."
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

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Toegang krijgen tot Azure virtuele Machines in Server Explorer

Via de Verkenner Server in Visual Studio, kunt u informatie weergeven over uw virtuele machines die worden gehost door Azure.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Toegang krijgen tot het virtuele machines in Server Explorer

Als u virtuele machines die worden gehost door Azure hebt, kunt u deze kunt openen in Verkenner van de Server. U moet eerst aanmelden bij uw Azure-abonnement om weer te geven van uw mobiele services. Als u zich aanmeldt, opent u het snelmenu voor het Azure knooppunt in Server Explorer en kies **verbinding maken met Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Voor informatie over uw virtuele machines

1. In Server Explorer, kiest u een virtuele machine en kies vervolgens de toets F4 om het bijbehorende eigenschappenvenster weergeven.

    De volgende tabel ziet u welke eigenschappen zijn beschikbaar, maar ze zijn alle alleen-lezen. Om deze te wijzigen, gebruikt u de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Eigenschap|Beschrijving|
  	|---|---|
  	|DNS-naam|De URL met het internetadres van de virtuele machine.|
  	|Omgeving|Voor een virtuele machine is de waarde van deze eigenschap altijd ontstaan.|
  	|Naam|De naam van de virtuele machine.|
  	|Grootte|De grootte van de virtuele machine, dat overeenkomt met de hoeveelheid ruimte geheugen en schijfruimte die beschikbaar is. Zie procedures voor meer informatie: VM grootten configureren.|
  	|Status|Waarden zijn starten, gestart stoppen, gestopt en Status ophalen. Als ophalen Status wordt weergegeven, is de huidige status is onbekend. De waarden voor deze eigenschap afwijken van de waarden die worden gebruikt in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|De abonnements-ID voor uw Azure-account. U kunt deze informatie weergeven op de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885) door de eigenschappen voor een abonnement te bekijken.|

1. Kies een knooppunt eindpunt en vervolgens het venster **Eigenschappen** te bekijken.

1. De volgende tabel beschrijft de beschikbare eigenschappen van de eindpunten, maar ze zijn alleen-lezen. Als u wilt toevoegen of bewerken van de eindpunten voor een virtuele machine, de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)te gebruiken. 

  	|Eigenschap|Beschrijving|
  	|---|---|
  	|Naam|Een id voor het eindpunt.|
  	|Privé poort|De poort voor netwerktoegang interne in uw toepassing.|
  	|Protocol|Het protocol dat de transportlaag voor dit eindpunt gebruikt, TCP of UDP.|
  	|Openbare poort|De poort die wordt gebruikt voor openbare toegang tot uw toepassing.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure rollen in Visual Studio, [Extern bureaublad met Azure rollen](vs-azure-tools-remote-desktop-roles.md).
