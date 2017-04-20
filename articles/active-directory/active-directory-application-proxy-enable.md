<properties
    pageTitle="Azure AD-toepassingsproxy inschakelen | Microsoft Azure"
    description="Toepassingsproxy inschakelen in de portal van Azure klassieke en de verbindingslijnen voor de reverse-proxy installeren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# <a name="enable-application-proxy-in-the-azure-portal"></a>Toepassingsproxy inschakelen in de portal van Azure

In dit artikel begeleidt u bij de stappen voor het inschakelen van Microsoft Azure AD-toepassingsproxy voor uw adreslijst cloud in Azure AD.

Als u niet bekend met wat toepassingsproxy bent kunt u doen, meer informatie over [hoe u de beveiligde externe toegang tot on-premises implementatie toepassingen](active-directory-application-proxy-get-started.md).

## <a name="application-proxy-prerequisites"></a>Voorwaarden van toepassing-Proxy
Voordat u kunt inschakelen en toepassingsproxy services gebruiken, moet u beschikken over:

- Een [abonnement op Microsoft Azure AD eenvoudige of premium](active-directory-editions.md) en een Azure AD-map waarvoor u een globale beheerder bent.
- Een server met Windows Server 2012 R2 of Windows 8.1 of hoger, waarop u de verbindingslijn van de Proxy-toepassing kunt installeren. De server verzendt aanvragen naar de toepassingsproxy services in de cloud en deze nodig een HTTP of HTTPS-verbinding met de toepassingen die u wilt publiceren.

    - Voor eenmalige aanmelding op uw uitgegeven toepassingen, deze computer moet worden domein-lid in hetzelfde AD domein als de toepassingen die u wilt publiceren.

- Als er een firewall in het pad, ervoor zorgen dat dit openen is zodat de verbindingslijn HTTPS (TCP) aanvragen in de toepassingsproxy kunt aanbrengen. De verbindingslijn gebruikt deze poorten samen met subdomeinen die deel van de op hoog niveau domeinen msappproxy.net en servicebus.windows.net uitmaken. Controleer of de volgende poorten op **Uitgaand** verkeer openen:

  	| Poortnummer | Beschrijving |
  	| --- | --- |
  	| 80 | Uitgaande HTTP-verkeer voor beveiligingsvalidatie inschakelen. |
  	| 443 | Gebruikersverificatie ten opzichte van Azure AD (alleen vereist voor het registratieproces Connector) inschakelen |
  	| 10100 – 10120 | HTTP LOB antwoorden terug naar de proxy inschakelen |
  	| 9352, 5671 | Communicatie tussen de verbindingslijn richting van de Azure-service voor verzoeken voor oproepen inschakelen. |
  	| 9350 | Optionele, betere prestaties voor verzoeken voor oproepen inschakelen |
  	| 8080 | De verbindingslijn bootstrap volgorde en de verbindingslijn automatisch bijwerken inschakelen |
  	| 9090 | De verbindingslijn registratie (alleen vereist voor het registratieproces Connector) inschakelen |
  	| 9091 | Connector vertrouwen certificaat automatische verlenging inschakelen |

    Als uw firewall verkeer op basis van die afkomstig zijn van gebruikers afdwingt, opent u deze poorten voor verkeer die afkomstig zijn uit de Windows-services wordt uitgevoerd met een netwerkservice. Zorg er ook poort 8080 inschakelen voor systeemaccount.

- Als uw organisatie gebruikmaakt van proxyservers verbinding maken met internet, gaat u naar op het blogbericht [werken met bestaande on-premises implementatie-proxyservers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) voor meer informatie over het configureren van deze.

## <a name="step-1-enable-application-proxy-in-azure-ad"></a>Stap 1: Toepassingsproxy in Azure AD inschakelen
1. Meld u aan als beheerder in de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de map waarin u wilt inschakelen toepassingsproxy.

    ![Active Directory - pictogram](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Selecteer **configureren** in de directory-pagina en schuif omlaag naar **Toepassingsproxy**.
4. Wisselknop **Proxy toepassingsservices voor deze map inschakelen** **ingeschakeld**.

    ![Toepassingsproxy inschakelen](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Selecteer **nu downloaden**. Hiermee gaat u naar de **Azure AD toepassing Proxy verbindingslijn downloaden**. Lees en accepteer de licentievoorwaarden en klik op **downloaden** om op te slaan van de Windows Installer-bestand (.exe) voor de verbindingslijn.

## <a name="step-2-install-and-register-the-connector"></a>Stap 2: Installeren en registreren van de verbindingslijn
1. **AADApplicationProxyConnectorInstaller.exe** worden uitgevoerd op de server die u voorbereid volgens de vereisten.
2. Volg de instructies in de wizard te installeren.
3. Tijdens de installatie wordt u wordt gevraagd om te registreren van de verbindingslijn met de toepassingsproxy van uw Azure AD-tenant.

  - Geef uw referenties van Azure AD globale beheerder. Uw tenant hoofdbeheerder afwijken van uw Microsoft Azure-referenties.
  - Controleer of de beheerder die de verbindingslijn registreert is in dezelfde map waarvoor u de service toepassingsproxy hebt ingeschakeld. Als het domein van de tenant contoso.com is, de beheerder moet bijvoorbeeld admin@contoso.com of een andere alias in dat domein.
  - Als **Verbeterde beveiliging van Internet Explorer** is ingesteld **op** op de server waarop u de verbindingslijn installeert, kunt u het registratiescherm mogelijk geblokkeerd. Volg de instructies in het foutbericht om toegang te krijgen. Zorg ervoor dat Internet Explorer Verbeterde beveiliging uitgeschakeld is.
  - Als verbindingslijn registratie niet slaagt, raadpleegt u [Problemen met toepassingsproxy](active-directory-application-proxy-troubleshoot.md).  

4. Wanneer de installatie is voltooid, worden twee nieuwe services worden toegevoegd aan uw server:

    - **Microsoft AAD toepassing Proxy Connector** zorgt voor connectivity
    - **Microsoft AAD toepassing Proxy verbindingslijn bijwerken** is een geautomatiseerde updateservice, die regelmatig Hiermee wordt gecontroleerd op nieuwe versies van de verbindingslijn en updates van de verbindingslijn naar wens.

    ![App-Proxy verbindingslijn services - schermafbeelding](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Klik op **Voltooien** in het installatievenster.

Voor beschikbaarheid doeleinden, moet u ten minste twee verbindingslijnen distribueren. Als u wilt meer verbindingslijnen implementeert, herhaalt u stappen 2 en 3, hierboven. Elke verbindingslijn die moet afzonderlijk worden geregistreerd.

Als u verwijderen van de verbindingslijn wilt, verwijdert u zowel de service-Connector als de service bijwerken. Start de computer als u wilt de service volledig te verwijderen.


## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te [publiceren toepassingen met toepassingsproxy](active-directory-application-proxy-publish.md).

Als u toepassingen op afzonderlijke netwerken of verschillende locaties hebt, kunt u verbindingslijn groepen naar de andere verbindingslijnen indelen in logische eenheden. Meer informatie over het [werken met toepassingsproxy verbindingslijnen](active-directory-application-proxy-connectors.md).
