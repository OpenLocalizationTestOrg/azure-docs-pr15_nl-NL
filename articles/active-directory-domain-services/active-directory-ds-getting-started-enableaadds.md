<properties
    pageTitle="Azure AD-domeinservices: Azure AD-domeinservices inschakelen | Microsoft Azure"
    description="Aan de slag met Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Azure AD-domeinservices inschakelen

## <a name="task-3-enable-azure-ad-domain-services"></a>Taak 3: Azure AD-domeinservices inschakelen
In deze taak inschakelen u Azure AD Domain Services voor uw adreslijst. De volgende configuratiestappen om in te schakelen Azure AD Domain Services voor uw adreslijst uitvoeren.

1. Ga naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **Active Directory** in het linkerdeelvenster.

3. Selecteer de Azure AD-tenant (directory) waarvoor u wilt inschakelen Azure AD Domain Services.

    ![Selecteer Azure AD-map](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klik op het tabblad **configureren** .

    ![Tabblad van map configureren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Schuif omlaag naar een sectie **domeinservices**.

    ![Sectie Domain Services-configuratie](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Schakelen tussen de optie getiteld **domeinservices voor deze map inschakelen** op **Ja**. Ziet u enkele meer configuratieopties voor Azure AD-domeinservices weergegeven op de pagina.

    ![Domain Services inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Wanneer u Azure AD Domain Services voor uw tenant inschakelt, Azure AD genereert worden opgeslagen en de Kerberos en NTLM referentie-hashes die vereist zijn voor het verifiëren van gebruikers.

7. Geef de **DNS-domeinnaam van domain services**.

   - De standaarddomeinnaam van de map (dat wil zeggen dat eindigt met de **. onmicrosoft.com** domeinachtervoegsel) is standaard ingeschakeld.

   - De lijst bevat alle domeinen die zijn geconfigureerd voor het telefoonboek van uw Azure AD-inclusief geverifieerd, evenals niet-geverifieerde domeinen die u op het tabblad 'Domeinen' configureren.

   - Bovendien kunt u ook een aangepaste domeinnaam typen. In dit voorbeeld hebben we hebt getypt in een aangepaste domeinnaam 'contoso100.com'.

     > [AZURE.WARNING] Controleer of het voorvoegsel domein van de naam van het domein dat u opgeeft (bijvoorbeeld 'contoso100' in de domeinnaam 'contoso100.com') minder dan 15 tekens is. U kunt een domein Azure AD Domain Services kan maken met een voorvoegsel domein meer dan 15 tekens.

8. Zorg ervoor dat de DNS-domeinnaam die u hebt gekozen voor het beheerde domein niet in het virtuele netwerk bestaat nog. Controleer met name als:

   - u hebt al een domein met dezelfde DNS-domeinnaam op het virtuele netwerk.

   - het virtuele netwerk dat u hebt geselecteerd heeft een VPN-verbinding met uw on-premises netwerk en u een domein met dezelfde DNS-domeinnaam hebben op uw on-premises netwerk.

   - Er wordt een bestaande cloudservice met die naam in het virtuele netwerk.

9. De volgende stap is om te selecteren van een virtueel netwerk waarin de gewenste Azure AD-domeinservices kan worden gecontroleerd. Selecteer de virtuele netwerk en speciale subnetten die u hebt gemaakt in de vervolgkeuzelijst getiteld **domeinservices verbinden met dit virtuele netwerk**.

   - Zorg ervoor dat het virtuele netwerk dat u hebt opgegeven deel uitmaakt van een Azure regio worden ondersteund door Azure AD Domain Services. Verwijzen naar de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) informatie van de Azure regio's waarin Azure AD-domeinservices beschikbaar is.

   - Virtuele netwerken die deel uitmaken van een gebied waar Azure AD-domeinservices niet wordt ondersteund doen niet weergegeven in de vervolgkeuzelijst.
   
   - Gebruik een speciale subnet binnen het virtuele netwerk voor Azure AD Domain Services. Controleer of dat u het subnet gateway niet selecteert. Zie [netwerken overwegingen](active-directory-ds-networking.md). 

   - Daarnaast worden virtuele netwerken die zijn gemaakt met Azure Resource Manager niet weergegeven in de vervolgkeuzelijst. Virtuele netwerken resourcemanager gebaseerde worden momenteel niet ondersteund door Azure AD Domain Services.

10. Als u wilt inschakelen Azure AD-domeinservices, klik op van het taakvenster onder aan de pagina op **Opslaan** .

11. De pagina wordt weergegeven een ' in behandeling...' Provincie, terwijl Azure AD-domeinservices is worden ingeschakeld voor uw map.

    ![Domeinservices - wachtende staat inschakelen](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure AD-domeinservices biedt beschikbaarheid voor uw domein beheerde. Nadat u Azure AD Domain Services inschakelt, ziet u de IP-adressen op welke domeinservices beschikbaar op de een door een virtueel netwerk te zien zijn. Het tweede IP-adres wordt binnenkort, weergegeven als u snel de service kan de beschikbaarheid voor uw domein. Wanneer beschikbaarheid geconfigureerd en actief zijn voor uw domein is, ziet u twee IP-adressen in de sectie **domeinservices** van het tabblad **configureren** .

12. U ziet het eerste IP-adres waarop Domain Services beschikbaar op uw virtual netwerk in het veld **IP-adres** op de pagina **configureren is** na ongeveer 20-30 minuten.

    ![Domeinservices ingeschakeld - eerste IP-adres deze is ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Wanneer beschikbaarheid operationele voor uw domein is, ziet u twee IP-adressen die worden weergegeven op de pagina. Uw beheerde domein is beschikbaar op uw geselecteerde virtuele netwerk op deze twee IP-adressen. Houd er rekening mee omlaag de IP-adressen zodat u kunt bijwerken de DNS-instellingen voor uw virtuele netwerk. Deze stap kunt virtuele machines in de virtuele netwerk verbinding maken met het domein voor bewerkingen zoals aan domein toevoegen.

    ![Domeinservices ingeschakeld - beide IP-adressen deze is ingericht](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Afhankelijk van de grootte van uw Azure AD-tenant (aantal gebruikers, groepen, enz.), duurt synchronisatie naar uw beheerde domein. Er gebeurt dit synchronisatieproces op de achtergrond. Voor grote tenants met tientallen duizenden objecten, het duurt mogelijk een dag of twee voor alle gebruikers, groepslidmaatschappen en referenties moeten worden gesynchroniseerd.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Taak 4 - Update DNS-instellingen voor het Azure virtuele netwerk
De volgende configuratietaak is [de DNS-instellingen voor het Azure virtuele netwerk bijwerken](active-directory-ds-getting-started-dns.md).
