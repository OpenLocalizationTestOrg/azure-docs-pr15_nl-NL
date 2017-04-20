<properties 
    pageTitle="Azure RemoteApp probleemoplossing - starten en verbinding toepassingsfouten | Microsoft Azure" 
    description="Informatie over het oplossen van problemen met de begin- en verbinding maakt met toepassingen in Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



#<a name="troubleshoot-azure-remoteapp---application-launch-and-connection-failures"></a>Problemen met Azure RemoteApp - toepassingsfouten starten en verbinding 

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Toepassingen die zijn ingesloten in een RemoteApp Azure kunnen niet worden starten om verschillende redenen. In dit artikel worden de verschillende redenen en foutberichten gebruikers mogelijk ontvangt wanneer u probeert om toepassingen te starten. Het is ook moment spreekt over verbindingsfouten. (Maar problemen kunnen niet in dit artikel wordt beschreven wanneer u zich aanmeldt bij de Azure RemoteApp-client.)  

Lees verder voor informatie over algemene foutberichten vanwege app starten en verbinding fouten.

##<a name="were-getting-you-set-up-try-again-in-10-minutes"></a>We krijgt u instellen... Probeer opnieuw 10 minuten.

Deze fout betekent dat Azure RemoteApp is schaalbaarheid van die aan de capaciteitsbehoeften van de gebruikers beantwoorden. Op de achtergrond die worden meer Azure RemoteApp VM exemplaren gemaakt voor het verwerken van de capaciteitsbehoeften van uw gebruikers. Meestal dit duurt ongeveer vijf minuten, maar kan maximaal 10 minuten duren. Soms kan dit snel genoeg voorkomen en resources nodig zijn onmiddellijk. Bijvoorbeeld een 9 AM scenario waar veel gebruikers moeten uw app gebruiken in Azure RemoteAppn op hetzelfde moment. Als dit gebeurt er met u kunnen we **capaciteit modus** op de back-end inschakelen. Hiervoor een Azure ondersteuningsticket openen en er of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com). Er zeker van wilt opnemen van uw abonnements-ID in het verzoek.  

![We instellen van het punt](./media/remoteapp-apptrouble/ra-apptrouble1.png)

## <a name="could-not-auto-reconnect-to-your-applications-please-re-launch-your-application"></a>Kan niet automatisch-verbinding voor uw toepassingen, opnieuw start de toepassing  

Dit foutbericht wordt vaak zichtbaar als u Azure RemoteApp hebt gebruikt en zet vervolgens uw PC slaapstand langer dan 4 uur en vervolgens uw PC woke omhoog en de Azure RemoteApp-client probeert automatisch opnieuw verbinding maakt en time-out is overschreden.  Geef aan gebruikers terug naar de toepassing en probeert te openen vanuit de Azure RemoteApp-client.

![Kan niet automatisch-opnieuw verbinding maken met uw toepassingen](./media/remoteapp-apptrouble/ra-apptrouble2.png) 

## <a name="problems-with-the-temp-profile"></a>Problemen met het temp profiel 

Deze fout treedt op wanneer uw gebruikersprofiel (schijf voor het profiel van de gebruiker) kan niet worden gekoppeld en de gebruiker een tijdelijk profiel ontvangen.  Beheerders moeten Navigeer naar de verzameling in de portal van Azure en vervolgens gaat u naar het tabblad **sessies** en probeert te **Afmelden** de gebruiker. Hiermee wordt afdwingen van een volledig logboek mensen uit de sessie van de gebruiker - en vervolgens een gebruiker probeert aan een app opnieuw starten. Als dat niet lukt contact op met ondersteuning voor Azure en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

## <a name="azure-remoteapp-has-stopped-working"></a>Azure RemoteApp is gestopt

Dit foutbericht wordt weergegeven, betekent dat de Azure RemoteApp-client is een probleem hebt en u moet opnieuw worden gestart. Geef de instructie om gebruikers te sluiten: Selecteer **programma sluit** en vervolgens de Azure RemoteApp-client opnieuw starten.  Als het probleem zich blijft voordoen openen en Azure ondersteuningsticket en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com).

![Azure RemoteApp is gestopt](./media/remoteapp-apptrouble/ra-apptrouble3.png)  

## <a name="an-error-occurred-while-remote-desktop-connection-was-accessing-this-resource-retry-the-connection-or-contact-your-system-administrator"></a>Een fout opgetreden tijdens de verbinding met extern bureaublad is toegang tot deze bron. Probeer de verbinding of neem contact op met uw systeembeheerder

Dit is een generieke foutmelding: contact opnemen met Azure ondersteuning en of een e-mail sturen naar [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com) zodat we kunnen onderzoeken. 

![Algemene Azure RemoteApp-bericht](./media/remoteapp-apptrouble/ra-apptrouble4.png) 