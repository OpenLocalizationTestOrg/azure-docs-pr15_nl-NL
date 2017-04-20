<properties
    pageTitle="Technische bètaversie van Azure stapel App Service 1 voordat u begint | Microsoft Azure"
    description="Stappen om te voltooien voordat u distribueert Web-Apps op Azure-Stack"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Voordat u aan de slag met Azure stapel Web Apps

> [AZURE.NOTE] De volgende informatie is alleen van toepassing op Azure stapel TP1 implementaties.

U moet een paar items Azure stapel Web-Apps moet installeren:

- Een voltooide implementatie van [Azure stapel Technical Preview-versie 1](azure-stack-run-powershell-script.md)
- Onvoldoende ruimte in het bestandssysteem Azure stapel voor een kleine implementatie van Azure stapel Web Apps.  De benodigde ruimte is ongeveer 20GB schijfruimte.
- [Een server waarop SQL Server wordt uitgevoerd](#SQL-Server).

>[AZURE.NOTE] De volgende stappen plaats op de Client-VM vindt.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Voordat u Azure stapel Web Apps implementeren

Als u wilt implementeren een resource-provider, moet u de PowerShell geïntegreerde uitvoeren van scripts Environment(ISE) uitvoeren als beheerder. Daarom moet u cookies en JavaScript toestaan in het profiel van Internet Explorer waarmee u zich aanmelden bij Azure Active Directory.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Internet Explorer Verbeterde beveiliging uitschakelen

1.  Meld u aan bij de computer Azure stapel bewijs-van-concept (Haalbaarheidstest) als **AzureStack/beheerder**en open vervolgens **Serverbeheer**.
2.  **Verbeterde beveiliging van Internet Explorer** voor zowel beheerders en gebruikers uitschakelen.
3.  Meld u aan bij de ClientVM.AzureStack.local virtuele machine als een beheerder en open vervolgens **Serverbeheer**.
4.  **Verbeterde beveiliging van Internet Explorer** voor zowel beheerders en gebruikers uitschakelen.

## <a name="enable-cookies"></a>Cookies inschakelen

1.  Klik op **Start**, > **alle apps**> **Windows Bureau-accessoires**. Met de rechtermuisknop op **Internet Explorer** > **meer** > **uitvoeren als beheerder**.
2.  Als u wordt gevraagd, selecteert u **Gebruik aanbevolen beveiliging**en selecteer vervolgens **OK**.
3.  In Internet Explorer, selecteer **Extra** (het tandwielpictogram) > **Internetopties** > **Privacy** > **Geavanceerd**.
4.  Selecteer **Geavanceerde**. Zorg ervoor dat beide selectievakjes **accepteren** zijn ingeschakeld en selecteer vervolgens **OK** en kies **OK** opnieuw.
5.  Sluit Internet Explorer en start opnieuw PowerShell ISE als beheerder.

## <a name="install-the-latest-version-of-azure-powershell"></a>Installeer de meest recente versie van Azure PowerShell

1.  Meld u aan bij de computer Azure stapel Haalbaarheidstest als **AzureStack/beheerder**.
2.  Gebruik de verbinding met extern bureaublad te melden bij de ClientVM.AzureStack.local virtuele machine als beheerder.
3.  Open **Het Configuratiescherm** en selecteer **een programma verwijderen**. 
4.  Met de rechtermuisknop op **Microsoft Azure PowerShell - November-2015** en klik op **verwijderen**.
5.  Na het verwijderen is voltooid, start opnieuw op de ClientVM.AzureStack.local virtuele machine
6.  Download en installeer de meest recente [Azure PowerShell](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL Server

Het installatieprogramma van Azure stapel Web Apps is standaard ingesteld op het gebruik van de SQL Server die samen met de Provider voor SQL-Resource van Azure stapel is geïnstalleerd. Controleer of dat u moet u rekening houden van de database beheerdersgebruikersnaam en wachtwoord tijdens de installatie van de Resource Provider voor SQL Server (SQL RP). U moet beide tijdens de installatie van Azure stapel Web Apps.
Opmerking: U hebt ook de mogelijkheid om te implementeren en het gebruik van een andere server naar SQL Server wordt uitgevoerd. De SQL Server-instantie moeten gebruiken wanneer u de opties in het installatieprogramma van Azure stapel Web-Apps hebt voltooid, kunt u kiezen.
