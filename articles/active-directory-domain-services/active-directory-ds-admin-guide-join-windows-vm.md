<properties
    pageTitle="Azure Active Directory Domain Services: Een Windows Server VM toevoegen aan een beheerde domein | Microsoft Azure"
    description="Deelnemen aan een virtuele machine van Windows Server Azure AD Domain Services"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Een Windows Server virtuele machine toevoegen aan een beheerde domein

> [AZURE.SELECTOR]
- [Azure klassieke portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

In dit artikel leest u hoe u deelnemen aan een virtuele machine met Windows Server 2012 R2 naar een beheerde domein Azure AD Domain Services, met behulp van de Azure klassieke portal.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Stap 1: De Windows Server virtuele machine maken
Volg de aanwijzingen in de zelfstudie [maken een virtuele machine waarop Windows in de portal van Azure klassieke wordt uitgevoerd](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Het is belangrijk om ervoor te zorgen dat de nieuwe virtuele machine is gekoppeld aan hetzelfde virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld. De optie Snelle maken wordt u om de virtuele machine te koppelen aan een virtueel netwerk niet ingeschakeld. Daarom moet u de optie uit galerie gebruiken om te maken van de virtuele machine.

Voer de volgende stappen uit als u wilt maken van een virtuele Windows-computer die zijn gekoppeld aan het virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld.

1. Klik in de klassieke Azure portal, op de opdrachtbalk onderaan in het venster op **Nieuw**.

2. Klik onder **berekenen**, **VM**Klik **Vanuit de galerie**.

3. Het eerste scherm kunt u **een afbeelding kiezen** voor uw virtuele machine in de lijst met beschikbare afbeeldingen. Kies de gewenste afbeelding.

    ![Selecteer afbeelding](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Het tweede scherm kunt u kiezen een computernaam, grootte en administratieve gebruikersnaam en wachtwoord. Gebruik de laag en de grootte die zijn vereist voor het uitvoeren van uw app of de werklast. De naam van de gebruiker die u hier kiest, is een van de lokale beheerdergebruiker op de computer. Voer geen referenties voor een domein-gebruikersaccount hier.

    ![Virtuele machine configureren](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Het derde scherm kunt u bronnen voor netwerken, opslag en beschikbaarheid configureren. Controleer of dat u het virtuele netwerk waarin u Azure AD-domeinservices in de vervolgkeuzelijst **Regio/affiniteit groep/virtuele netwerk** ingeschakeld selecteren. Geef een **Cloud Service DNS-naam** voor de virtuele machine.

    ![Selecteer virtuele netwerk voor virtuele machine](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Zorg ervoor dat u deelneemt aan de virtuele machine op hetzelfde virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld. Hierdoor kan de virtuele machine "Zie" het domein en uitvoeren van taken zoals het deelnemen aan het domein. Als u kiest de virtuele machine maken in een ander virtuele netwerk, verbinden dat virtuele netwerk het virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld.

6. Het vierde scherm kunt u de VM-Agent installeren en configureren van enkele van de beschikbare uitbreidingen.

    ![Klaar](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Nadat de virtuele machine is gemaakt, wordt in de klassieke portal de nieuwe virtuele machine onder het knooppunt **virtuele Machines** bevat. Zowel de virtuele machine en cloudservice worden automatisch gestart en hun status is vermeld als **actief**.

    ![VM is actief](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Stap 2: Verbinding maken met de Windows Server virtuele machine met de lokale beheerdersaccount
Nu kunnen we verbinding maken met de zojuist gemaakte Windows Server virtuele-computer om deze te koppelen aan het domein. Gebruik de van de lokale beheerdersreferenties die u hebt opgegeven bij het maken van de virtuele machine, tot stand te brengen.

De volgende stappen verbinding maken met de virtuele machine uitvoeren.

1. Navigeer naar **virtuele Machines** knooppunt in de klassieke portal. Selecteer de virtuele machine die u in stap 1 hebt gemaakt en klik op **verbinding maken met** op de opdrachtbalk onderaan in het venster.

    ![Verbinding maken met virtuele Windows-computer](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. De klassieke portal bericht dat u wilt openen of opslaan van een bestand met de 'RDP-' extensie, waarmee u verbinding maken met de virtuele machine. Klik op het bestand te openen wanneer deze is gedownload.

3. Voer uw **lokale beheerdersreferenties**, wat u hebt opgegeven bij het maken van de virtuele machine bij de prompt login. We hebben bijvoorbeeld 'localhost\mahesh' gebruikt in dit voorbeeld.

Nu moet u aangemeld bij de zojuist gemaakte virtuele Windows-computer met lokale Administrator-referenties. De volgende stap is de virtuele machine toevoegen aan het domein.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Stap 3: Deelnemen aan de Windows Server virtuele machine aan de beheerde AAD-DS-domein
Voer de volgende stappen uit als u wilt deelnemen aan de Windows Server virtuele machine aan de beheerde AAD-DS-domein.

1. Verbinding maken met het Windows-Server zoals wordt weergegeven in stap 2. Open **Serverbeheer**vanuit het startscherm.

2. Klik in het linkerdeelvenster van het venster Server Manager op **Lokale Server** .

    ![Serverbeheer op virtuele machine starten](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Klik op **werkgroep** onder de sectie **Eigenschappen** . Klik in het eigenschappenvenster **Systeemeigenschappen** op **wijzigen** als u wilt deelnemen aan het domein.

    ![Eigenschappen van Systeempagina](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Geef de domeinnaam van uw Azure AD-domeinservices domein in het tekstvak **domein** beheerd en klik op **OK**.

    ![Geef het domein wilt samenvoegen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. U wordt gevraagd of u uw referenties als u wilt deelnemen aan het domein in te voeren. Zorg ervoor dat u **de referenties voor een gebruiker die deel uitmaken van de beheerders van de domeincontroller AAD opgeven** groep. Alleen leden van deze groep hebben bevoegdheden machines toevoegen aan de beheerde domein.

    ![Geef de referenties voor aan domein toevoegen](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. U kunt referenties opgeven in een van de volgende manieren:

    - UPN-indeling: Geef de UPN-achtervoegsel voor de gebruikersaccount, zoals geconfigureerd in Azure AD. In dit voorbeeld de UPN-achtervoegsel van de gebruiker 'bob' is 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName opmaken: kunt u de accountnaam in de SAMAccountName-indeling. In dit voorbeeld moet de gebruiker 'bob' 'CONTOSO100\bob' invoeren.

        > [AZURE.NOTE] **Het is raadzaam de UPN-indeling gebruiken om te geven van referenties.** De SAMAccountName mogelijk worden automatisch gegenereerd als een gebruikerswachtwoord UPN voorvoegsel is te lang (bijvoorbeeld 'joereallylongnameuser'). Als meerdere gebruikers hetzelfde UPN voorvoegsel (bijvoorbeeld ' bob' genoemd) in uw Azure AD-tenant hebben, kan hun SAMAccountName-indeling kan worden automatisch gegenereerd door de service. In dat geval kan de indeling UPN betrouwbaar worden gebruikt voor aanmelding bij het domein.

7. Als het domein join is gelukt, ziet u de volgende welkomstbericht het domein. Start opnieuw op de virtuele machine voor het domein join-bewerking om te voltooien.

    ![Welkom bij het domein](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Problemen oplossen aan domein toevoegen
### <a name="connectivity-issues"></a>Verbindingsproblemen
Als de virtuele machine niet vinden van het domein is, gebruikt u verwijzen naar de volgende stappen:

- Zorg ervoor dat de virtuele machine is verbonden met hetzelfde virtuele netwerk als die u hebt ingeschakeld, is een domeinservices in. Zo niet, in de virtuele machine geen verbinding maken met het domein is en daarom kan niet deelnemen aan het domein.

- Als de virtuele machine is aangesloten op een ander virtuele netwerk, moet u ervoor zorgen dat dit virtuele netwerk is verbonden met het virtuele netwerk waarin u domeinservices hebt ingeschakeld.

- Probeer te pingen van het domein de domeinnaam van het beheerde domein (bijvoorbeeld ping contoso100.com) gebruikt. Als u niet kan doen bent, wil ping het IP-adressen voor het domein dat wordt weergegeven op de pagina waar u Azure AD Domain Services ingeschakeld (bijvoorbeeld ' ping adres 10.0.0.4'). Als u zich kunt ping het IP-adres, maar niet het domein, DNS mogelijk onjuist geconfigureerd. Mogelijk hebt u de IP-adressen van het domein als de DNS-servers voor het virtuele netwerk niet geconfigureerd.

- Probeer de DNS-resolvercache cache op de virtuele machine (' ipconfig/flushdns').

Als u toegang hebt tot het dialoogvenster dat wordt gevraagd om referenties voor deelname aan het domein, hoeft u niet verbindingsproblemen.


### <a name="credentials-related-issues"></a>Problemen met de referenties
Raadpleeg de volgende stappen uit als u problemen met referenties ondervindt en kan niet deelnemen aan het domein bent.

- Gebruik de UPN-indeling referenties opgeven. De SAMAccountName voor uw account mogelijk automatisch gegenereerde als er meerdere gebruikers met hetzelfde UPN voorvoegsel in uw tenant is ingeschakeld of als uw UPN-voorvoegsel te lang is. Daarom kan de SAMAccountName-indeling voor uw account afwijken van wat u verwacht of gebruiken in uw on-premises domein.

- Probeer de referenties van een gebruikersaccount die deel uitmaakt van de groep 'AAD domeincontroller beheerders' machines toevoegen aan de beheerde domein gebruiken.

- Zorg ervoor dat u [ingeschakeld Wachtwoordsynchronisatie](active-directory-ds-getting-started-password-sync.md) volgens de stappen in de handleiding aan de slag hebt.

- Zorg ervoor dat u de UPN van de gebruiker zoals geconfigureerd in Azure AD (bijvoorbeeld 'bob@domainservicespreview.onmicrosoft.com') aan te melden.

- Zorg ervoor dat u hebt lang genoeg gewacht Wachtwoordsynchronisatie om uit te voeren zoals opgegeven in de handleiding aan de slag.


## <a name="related-content"></a>Verwante onderwerpen

- [Azure AD-domeinservices - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Een beheerde domein van Azure AD Domain Services beheren](./active-directory-ds-admin-guide-administer-domain.md)
