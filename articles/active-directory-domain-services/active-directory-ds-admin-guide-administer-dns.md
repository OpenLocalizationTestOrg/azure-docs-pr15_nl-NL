<properties
    pageTitle="Azure Active Directory Domain Services: DNS op beheerde domeinen beheren | Microsoft Azure"
    description="DNS op Azure Active Directory Domain Services beheerde domeinen beheren"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>DNS op een beheerde domein van Azure AD Domain Services beheren
Azure Active Directory Domain Services bevat een DNS-(domein-naamresolutie)-server die DNS-oplossing voor het beheerde domein bevat. Mogelijk moet u soms DNS configureren voor de beheerde domein. Mogelijk moet u DNS-records voor machines die niet zijn toegevoegd aan het domein maken, configureren virtuele IP-adressen voor netwerktaakverdelers of externe DNS-doorstuurservers instellen. Daarom zijn gebruikers die deel uitmaakt van de ' AAD domeincontroller ' beheerdersgroep DNS-beheer van de bevoegdheden van het domein dat beheerde verleend.


## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van de taken in dit artikel, hebt u het volgende nodig:

1. Een geldig **abonnement op Azure**.

2. Een **Azure AD directory** - hetzij gesynchroniseerd met een on-premises adreslijst of een map alleen de cloud.

3. **Azure AD-domeinservices** moet zijn ingeschakeld voor de Azure AD-map. Als u dit nog niet hebt gedaan, voert u alle taken die worden beschreven in de [handleiding aan de slag](./active-directory-ds-getting-started.md).

4. Een **domein behoren VM** van waaruit u de beheerde domeinservices van Azure AD-domein beheren. Als u geen een VM, volgt u alle taken uit die in het artikel [deelnemen aan een virtuele Windows-computer naar een beheerde domein](./active-directory-ds-admin-guide-join-windows-vm.md)beschreven.

5. Moet u de referenties van een **gebruikersaccount van de ' AAD domeincontroller ' beheerdersgroep** in uw adreslijst, kunt u DNS beheren voor uw domein beheerde.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Taak 1: gegevens leveren een virtuele domein behoren machine extern beheren van DNS voor het domein dat beheerde
Azure AD-domeinservices beheerde domeinen kunnen extern worden beheerd via vertrouwde Active Directory beheerprogramma's zoals Active Directory administratieve Center (ADAC) of AD PowerShell. DNS-records voor het domein dat beheerde kan op dezelfde manier worden beheerd op afstand met de DNS-Server beheerprogramma's.

Beheerders in uw adreslijst Azure AD hebben geen bevoegdheden verbinding maken met domeincontrollers in het beheerde domein via Extern bureaublad. Leden van de ' AAD domeincontroller ' beheerdersgroep kunnen DNS beheren voor beheerde domeinen op afstand met hulpmiddelen voor de DNS-Server vanaf een computer met Windows Server/client die is gekoppeld aan de beheerde domein. Hulpmiddelen voor DNS-Server kunnen worden geïnstalleerd als onderdeel van de externe Server Administration Tools (RSAT) optionele functie op Windows Server en clientcomputers toegevoegd aan het beheerde domein.

De eerste taak is voor het inrichten van een Windows Server virtuele machine die is gekoppeld aan de beheerde domein. Raadpleeg het artikel [deelnemen aan een Windows Server virtuele machine naar een domein Azure AD-domeinservices beheerde](active-directory-ds-admin-guide-join-windows-vm.md)voor instructies.


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Taak 2: Installeer DNS-Server hulpmiddelen op de virtuele machine
De volgende stappen om te kunnen installeren van de DNS-beheerprogramma's op het domein toegevoegd virtuele machine uitvoeren. Zie Technet voor meer informatie over het [installeren en met de externe Server beheerprogramma's](https://technet.microsoft.com/library/hh831501.aspx).

1. Navigeer naar **virtuele Machines** knooppunt in de portal van Azure klassieke. Selecteer de virtuele machine die u hebt gemaakt in taak 1 en klik op **verbinding maken met** op de opdrachtbalk onderaan in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal bericht dat u wilt openen of opslaan van een bestand met de 'RDP-' extensie, waarmee u verbinding maken met de virtuele machine. Klik op het bestand wanneer het downloaden is voltooid.

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

9. Klik op de pagina **functies** Vouw het knooppunt **Externe Server beheerprogramma's** uit en klik op om uit te vouwen het knooppunt **Rol beheerprogramma's** . Selecteer **DNS-Server Tools** -functie in de lijst met hulpprogramma's voor functiebeheer.

    ![De pagina onderdelen van](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Klik op **de bevestigingspagina** op **installeren** om te kunnen installeren van de functie van de hulpmiddelen voor DNS-servers op de virtuele machine. Wanneer de installatie van voorziening voltooid is, klikt u op **sluiten** om de wizard **rollen toevoegen en functies** te sluiten.

    ![Bevestigingspagina](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Taak 3: starten de DNS management console DNS beheren
Nu dat de DNS-Server Tools-functie wordt geïnstalleerd op het domein VM die zijn gekoppeld, we de DNS-hulpprogramma's voor het beheer van DNS in het beheerde domein kunt gebruiken.

> [AZURE.NOTE] U moet lid zijn van de ' AAD domeincontroller ' beheerdersgroep, voor het beheer van DNS in het beheerde domein.

1. Klik op **Systeembeheer**vanuit het startscherm. Hier ziet u de **DNS-** console geïnstalleerd op de virtuele machine.

    ![Beheerprogramma's - DNS-Console](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klik op **DNS** om te starten van de DNS-beheerconsole.

3. Klik in het dialoogvenster **verbinding maken met de DNS-Server** de optie getiteld van **de volgende computer**op en voert u de DNS-naam van het beheerde domein (bijvoorbeeld ' contoso100.com').

    ![DNS-Console - verbinding maken met het domein](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. De DNS-Console maakt verbinding met de beheerde domein.

    ![DNS-Console - domein beheren](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. U kunt nu de DNS-console gebruiken om toe te voegen DNS entries voor computers binnen het virtuele netwerk waarin u AAD-domeinservices hebt ingeschakeld.

> [AZURE.WARNING] Wees voorzichtig met het beheer van DNS voor het beheerde domein met de DNS-beheerprogramma's. Zorg ervoor dat u **niet verwijderen of wijzigen van de ingebouwde DNS-records die worden gebruikt door domeinservices in het domein**. Ingebouwde DNS-records zijn domein DNS-records, DNS-records en andere records die wordt gebruikt voor de locatie van de domeincontroller. Als u deze records wijzigt, worden in een netwerk met de virtuele domeinservices onderbroken.


Zie het [artikel van de DNS-hulpmiddelen op Technet](https://technet.microsoft.com/library/cc753579.aspx) voor meer informatie over het beheren van DNS.


## <a name="related-content"></a>Verwante onderwerpen

- [Azure AD-domeinservices - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Deelnemen aan een Windows Server-VM een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Een beheerde domein van Azure AD Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)

- [DNS-beheer van de hulpprogramma 's](https://technet.microsoft.com/library/cc753579.aspx)
