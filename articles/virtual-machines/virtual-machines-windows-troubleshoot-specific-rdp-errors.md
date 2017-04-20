<properties
    pageTitle="Specifieke RDP-foutberichten voor Azure VMs | Microsoft Azure"
    description="Meer informatie over specifieke foutberichten die u ontvangt mogelijk wanneer u probeert verbinding met extern bureaublad naar een virtuele Windows-computer gebruiken in Azure wordt aangegeven"
    keywords="Externe bureaublad fout, verbinding met extern bureaublad fout, kan geen verbinding met VM, probleemoplossing extern bureaublad"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Voor een Windows-VM in Azure wordt aangegeven specifieke RDP-foutberichten oplossen
Het foutbericht een specifiek foutbericht wanneer u een verbinding met extern bureaublad met een Windows virtuele machine (VM) gebruikt in Azure wordt aangegeven. In dit artikel vindt u details van de meest voorkomende foutberichten heeft voorgedaan, samen met probleemoplossingen als u wilt ze kunt oplossen. Als u hebt problemen met verbinding maken met uw VM met RDP maar een foutmelding ondervinden, raadpleegt u de [problemen oplossen met extern bureaublad](virtual-machines-windows-troubleshoot-rdp-connection.md).

Zie de volgende onderwerpen voor informatie over specifieke foutberichten:

- [De externe sessie is beëindigd omdat er geen beschikbaar op te geven van een licentie externe bureaublad licentieservers](#rdplicense).
- [Extern bureaublad "naam" van de computer niet vinden](#rdpname).
- [Verificatie fout opgetreden. Local Security Authority niet bereikbaar](#rdpauth).
- [Windows-beveiliging-fout: uw referenties niet werken](#wincred).
- Op [deze computer kan geen verbinding maken met de externe computer](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>De externe sessie is beëindigd omdat er geen beschikbaar op te geven van een licentie externe bureaublad licentieservers.

Oorzaak: De respijtperiode 120 dagen licentievoorwaarden voor de functie Extern bureaublad-Server is verlopen en u moet licenties installeren.

Als een tijdelijke oplossing, een lokale kopie van het RDP-bestand opslaan in de portal en deze opdracht uitvoert op een PowerShell-opdrachtprompt om verbinding te maken. Deze stap wordt uitgeschakeld licenties voor alleen die verbinding:

        mstsc <File name>.RDP /admin

Als u niet meer dan twee gelijktijdige extern bureaublad-verbindingen met de VM wel behoefte, kunt u Serverbeheer verwijderen van de rol van het externe bureaublad-Server.

Zie het blogbericht [Azure VM mislukt waarbij 'Geen externe bureaublad licentieservers beschikbaar'](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/)voor meer informatie.

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Extern bureaublad vinden niet van de computer "naam".

Oorzaak: De extern bureaublad-client op uw computer kan de naam van de computer in de instellingen van het RDP-bestand niet omzetten.

Mogelijke oplossingen:

- Als u het intranet van een organisatie bent, zorgen dat uw computer toegang tot de proxyserver heeft en HTTPS-verkeer naar deze kunt verzenden.

- Als u een lokaal opgeslagen RDP-bestand gebruikt, probeert u de fase die wordt gegenereerd door de portal. Dit zorgt ervoor dat u de juiste DNS-naam voor de virtuele-computer of de cloudservice en de poort van het eindpunt van de VM hebben. Hier volgt een voorbeeld RDP-bestand gegenereerd door de portal:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Het gedeelte van dit bestand RDP heeft:
- De FQDN-naam van de cloudservice waarin de VM ("tailspin-azdatatier.cloudapp.net' in dit voorbeeld).

- De externe TCP-poort van het eindpunt voor extern bureaublad verkeer (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Een verificatiefout opgetreden. Local Security Authority niet bereikbaar.

Oorzaak: Het doel VM kan de beveiliging instantie vinden in het gedeelte van de gebruiker de naam van uw referenties.

Als uw gebruikersnaam is in het formulier *SecurityAuthority*\\*gebruikersnaam* (voorbeeld: CORP\User1), het gedeelte *SecurityAuthority* is de naam van de VM computer (voor de lokale certificeringsinstantie) of de naam van een Active Directory-domein.

Mogelijke oplossingen:

- Als de account lokale voor VM, zorg dat de naam van de VM juist is gespeld.

- Als het account in een Active Directory-domein is, controleert u de spelling van de domeinnaam.

- Als dit een Active Directory-domein-account is en de domeinnaam juist is gespeld, controleert u of een domeincontroller beschikbaar in dat domein is. Dit is een algemene probleem Azure virtuele netwerken die domeincontrollers dat een domeincontroller niet beschikbaar is omdat deze nog niet is begonnen bevatten. Als een tijdelijke oplossing kunt u een lokale beheerdersaccount gebruiken in plaats van een domeinaccount.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows-beveiliging-fout: uw referenties niet werken.

Oorzaak: Het doel VM kan geen valideren uw naam en het wachtwoord.

Een Windows-computer kunt valideren de referenties van een lokale account of een andere account.

- Voor lokale accounts, gebruikt u de *computernaam*\\syntaxis van de*gebruikersnaam* (voorbeeld: SQL1\Admin4798).
- Voor domeinaccounts, gebruikt u de *domeinnaam*\\syntaxis van de*gebruikersnaam* (voorbeeld: CONTOSO\peterodman).

Als u uw VM naar een domeincontroller in een nieuwe Active Directory-bos gepromoveerd, wordt het lokale beheerdersaccount waarvoor u zich hebt geregistreerd in met wordt geconverteerd naar een overeenkomstige account met hetzelfde wachtwoord in het nieuw bos en het domein. De lokale account wordt vervolgens verwijderd.

Als u die zijn aangemeld met de lokale DC1\DCAdmin-account en vervolgens de virtuele machine gepromoveerd tot een domeincontroller in een nieuw bos voor het domein corp.contoso.com, bijvoorbeeld de lokale DC1\DCAdmin-account wordt verwijderd en een nieuw domeinaccount (CORP\DCAdmin) wordt gemaakt met hetzelfde wachtwoord.

Zorg ervoor dat de accountnaam een naam die de virtuele machine kunt controleren of als een geldige account is en dat het wachtwoord klopt.

Als u het wachtwoord van het lokale beheerdersaccount wijzigen moet, raadpleegt u [het opnieuw instellen van een wachtwoord of de service Extern bureaublad voor Windows virtuele machines](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Deze computer geen verbinding maken met de externe computer.

Oorzaak: Het account dat wordt gebruikt om verbinding te heeft geen extern bureaublad aanmelden rechten.

Elke Windows-computer heeft een lokale gebruikersgroep van de extern bureaublad, waarin de accounts en groepen die u kunnen Meld u aan bij deze op afstand. Leden van de lokale beheerdersgroep hebt ook toegang, zelfs als deze accounts niet in de lokale groep voor extern bureaublad-gebruikers weergegeven worden. Voor een domein behoren machines bevat de lokale beheerdersgroep ook de domeinbeheerders voor het domein.

Zorg ervoor dat het account dat u gebruikt om te communiceren met extern bureaublad aanmelden rights heeft. Als een tijdelijke oplossing, een domein of lokale beheerdersaccount via verbinding met extern bureaublad. Als u wilt het gewenste account toevoegen aan de extern bureaublad lokale gebruikersgroep, gebruikt u de Microsoft Management Console-module (**Systeemwerkset > lokale gebruikers en groepen > groepen > Extern bureaublad-gebruikers**).


## <a name="next-steps"></a>Volgende stappen
Als geen van deze fouten opgetreden en u een onbekende probleem met de verbinding maken met RDP hebt, raadpleegt u de [problemen oplossen met extern bureaublad](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure diagnostics-pakket van IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Stappen voor probleemoplossing bij het openen van toepassingen op een VM, raadpleegt u [problemen oplossen met de toegang tot een toepassing wordt uitgevoerd op een VM Azure](virtual-machines-linux-troubleshoot-app-connection.md).
- Als u hebt met het gebruik van Secure Shell (SSH problemen) verbinding maken met een VM Linux in Azure wordt aangegeven, raadpleegt u [problemen met SSH verbindingen met een VM Linux in Azure wordt aangegeven](virtual-machines-linux-troubleshoot-ssh-connection.md).