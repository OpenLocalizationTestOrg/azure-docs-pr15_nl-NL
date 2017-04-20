<properties
    pageTitle="Azure Active Directory Domain Services: Een beheerde domein beheert | Microsoft Azure"
    description="Azure Active Directory Domain Services beheerde domeinen beheren"
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
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Een beheerde domein Azure Active Directory Domain Services beheren
In dit artikel leest u hoe u een beheerde domein van Azure Active Directory (AD) Domain Services beheren.


## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken in dit artikel, hebt u het volgende nodig:

1. Een geldig **abonnement op Azure**.

2. Een **Azure AD directory** - hetzij gesynchroniseerd met een on-premises adreslijst of een map alleen de cloud.

3. **Azure AD-domeinservices** moet zijn ingeschakeld voor de Azure AD-map. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **domein behoren VM** van waaruit u de beheerde domeinservices van Azure AD-domein beheren. Als u geen een VM, volgt u alle taken uit die in het artikel [deelnemen aan een virtuele Windows-computer naar een beheerde domein](./active-directory-ds-admin-guide-join-windows-vm.md)beschreven.

5. Moet u de referenties van een **gebruikersaccount van de ' AAD domeincontroller ' beheerdersgroep** in uw adreslijst, kunt u uw beheerde domein beheren.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Beheertaken die u op een beheerde domein uitvoeren kunt
Leden van de ' AAD domeincontroller ' beheerdersgroep krijgen bevoegdheden in de beheerde domein waarmee ze kunnen taken uitvoeren zoals:

- Machines toevoegen aan de beheerde domein.

- De ingebouwde GPO voor de containers 'AADDC Computers' en 'AADDC Users' in het beheerde domein configureren.

- DNS in het beheerde domein beheren.

- Maken en beheren (aangepaste organisatie eenheden) in het beheerde domein.

- Winst beheerderstoegang tot computers toegevoegd aan het beheerde domein.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Beheerdersbevoegdheden er geen op een beheerde domein
Het domein wordt beheerd door Microsoft, met inbegrip van activiteiten zoals patches, bewaken en, back-ups maken. Daarom het domein is vergrendeld en u beschikt niet over de bevoegdheden voor bepaalde administratieve taken in het domein. Er zijn enkele voorbeelden van taken die u niet kunt uitvoeren onder.

- U worden niet de beheerder van het domein of Ondernemingsadministrator bevoegdheden voor het domein dat beheerde verleend.

- U kunt het schema van het beheerde domein niet uitbreiden.

- U kunt geen verbinding maken met domeincontrollers voor de beheerde domein met extern bureaublad.

- U kunt domeincontrollers niet toevoegen aan de beheerde domein.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Taak 1: een domein behoren Windows Server virtuele machine om extern te beheren het domein dat beheerde inrichten
Azure AD-domeinservices beheerde domeinen kunnen worden beheerd met vertrouwde Active Directory-beheerprogramma's zoals Active Directory administratieve Center (ADAC) of AD PowerShell. Beheerders van de tenant hebben geen bevoegdheden verbinding maken met domeincontrollers in het beheerde domein via Extern bureaublad. Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen daarom beheerde domeinen op afstand met AD beheerprogramma's vanaf een computer met Windows Server/client die is gekoppeld aan de beheerde domein beheren. Beheerprogramma's van AD kunnen worden geïnstalleerd als onderdeel van de externe Server Administration Tools (RSAT) optionele functie op Windows Server en clientcomputers toegevoegd aan het beheerde domein.

De eerste stap is voor het instellen van een Windows Server virtuele machine die is gekoppeld aan de beheerde domein. Raadpleeg het artikel [deelnemen aan een Windows Server virtuele machine naar een domein Azure AD-domeinservices beheerde](active-directory-ds-admin-guide-join-windows-vm.md)voor instructies.

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Extern beheren het domein dat beheerde vanaf een clientcomputer (bijvoorbeeld Windows 10)
De instructies in dit artikel Windows Server virtuele machines voor het beheer van de AAD-DS beheerd domein. U kunt echter ook naar een Windows-client (bijvoorbeeld Windows 10) virtuele machine kunt doen met kiezen.

U kunt een [externe Server Administration Tools (RSAT) installeren](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) op een Windows-client virtuele machine volgens de instructies op TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Taak 2 - installeren Active Directory-beheerprogramma's op de virtuele machine
De volgende stappen om te kunnen installeren van de Active Directory-beheerprogramma's op het domein toegevoegd virtuele machine uitvoeren. Zie Technet voor meer [informatie over het installeren en met de externe Server beheerprogramma's](https://technet.microsoft.com/library/hh831501.aspx).

1. Navigeer naar **virtuele Machines** knooppunt in de portal van Azure klassieke. Selecteer de virtuele machine die u hebt gemaakt in taak 1 en klik op **verbinding maken met** op de opdrachtbalk onderaan in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal bericht dat u wilt openen of opslaan van een bestand met de 'RDP-' extensie, waarmee u verbinding maken met de virtuele machine. Klik op het bestand te openen wanneer deze is gedownload.

3. Gebruik de referenties van een gebruiker van de ' AAD domeincontroller ' beheerdersgroep bij de prompt login. Bijvoorbeeld: we gebruiken 'bob@domainservicespreview.onmicrosoft.com' in onze hoofdletters/kleine letters.

4. Open **Serverbeheer**vanuit het startscherm. Klik op **functies toevoegen en onderdelen** in het centrale deelvenster van het venster Server Manager.

    ![Serverbeheer op virtuele machine starten](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Klik op **volgende**op de pagina van de **rollen toevoegen en functies Wizard** **Voordat u begint** .

    ![Voordat u begint pagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Klik op de pagina **Installatietype** laat de **installatie op basis van rollen of functies gebaseerde** -optie ingeschakeld en klik op **volgende**.

    ![Pagina Type installatie](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Klik op de pagina **Server selectie** selecteert u de huidige virtuele machine uit de groep server en klik op **volgende**.

    ![De pagina Sjabloonselectie server](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Klik op **volgende**op de pagina **Serverrollen** . We overslaan deze pagina omdat we niet alle functies wilt op de server installeren.

9. Klik op de pagina **functies** Vouw het knooppunt **Externe Server beheerprogramma's** uit en klik op om uit te vouwen het knooppunt **Rol beheerprogramma's** . Selecteer functie **AD DS en AD-hulpprogramma's** in de lijst met hulpprogramma's voor functiebeheer.

    ![De pagina onderdelen van](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Klik op **de bevestigingspagina** op **installeren** voor het installeren de AD en AD LDS extra functie op de virtuele machine. Wanneer de installatie van voorziening voltooid is, klikt u op **sluiten** om de wizard **rollen toevoegen en functies** te sluiten.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Taak 3: verbinding maken met en het beheerde domein verkennen
Nu dat de AD beheerprogramma's zijn geïnstalleerd op het domein VM die zijn gekoppeld, we kunnen deze hulpmiddelen gebruiken om te verkennen en de beheerde domein beheert.

> [AZURE.NOTE] U moet lid zijn van de ' AAD domeincontroller ' beheerdersgroep, kunt u het domein dat beheerde beheren.

1. Klik op **Systeembeheer**vanuit het startscherm. Hier ziet u de AD beheerprogramma's geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's op de server is geïnstalleerd](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klik op **Beheerderscentrum voor Active Directory**.

    ![Active Directory-Beheerderscentrum](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Als u wilt verkennen het domein, klikt u op de domeinnaam in het linkerdeelvenster (bijvoorbeeld ' contoso100.com'). Zoals u ziet twee notitiecontainers respectievelijk 'AADDC Computers' en 'AADDC Users' genoemd.

    ![ADAC - weergave domein](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Klik op de container **AADDC gebruikers** om te zien van alle gebruikers en groepen die deel uitmaken van het domein dat beheerde genoemd. U ziet nu gebruikersaccounts en groepen uit uw Azure AD tenant weergeven omhoog in deze container. Kennisgeving in dit voorbeeld, een gebruikersaccount voor de gebruiker genoemd 'bob' en 'AAD domeincontroller beheerders' de groep zijn beschikbaar in deze container.

    ![ADAC - domeingebruikers](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Klik op de container **AADDC Computers** als u wilt zien van de computers toegevoegd aan deze beheerde domein genoemd. Hier ziet u een vermelding voor de huidige virtuele machine, die is toegevoegd aan het domein. Computeraccounts voor alle computers die zijn gekoppeld aan de beheerde domeinservices van Azure AD-domein worden opgeslagen in deze container 'AADDC Computers'.

    ![ADAC - domein computers die zijn gekoppeld](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Verwante onderwerpen

- [Azure AD-domeinservices - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Deelnemen aan een Windows Server-VM een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Hulpmiddelen voor het beheer van externe Server implementeren](https://technet.microsoft.com/library/hh831501.aspx)
