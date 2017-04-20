<properties
    pageTitle="Azure Active Directory Domain Services: Een VM RHEL toevoegen aan een beheerde domein | Microsoft Azure"
    description="Deelnemen aan een virtuele Red rol Enterprise Linux machine Azure AD Domain Services"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Een rood rol Enterprise Linux 7 virtuele machine toevoegen aan een beheerde domein
Dit artikel leest u hoe u een rood rol Enterprise Linux (RHEL) 7 virtuele machine te koppelen aan een beheerde domein van Azure AD Domain Services.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Een rood rol Enterprise Linux virtuele machine inrichten
De volgende stappen voor het inrichten van een RHEL 7 virtuele machine met behulp van de Azure portal uitvoeren.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).

    ![Azure portal dashboard](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Klik op het linkerdeelvenster op **Nieuw** en typt u **Rood rol** in de zoekbalk zoals wordt weergegeven in de volgende schermafbeelding. Vermeldingen voor Red rol Enterprise Linux worden weergegeven in de lijst met zoekresultaten. Klik op de **rode rol Enterprise Linux 7.2**.

    ![RHEL selecteert in zoekresultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. De zoekresultaten in het deelvenster **Alles** moeten de afbeelding rood rol Enterprise Linux 7.2 bevatten. Klik op **rood rol Enterprise Linux 7.2** om meer informatie over de VM-afbeelding weer te geven.

    ![RHEL selecteert in zoekresultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. Klik in het deelvenster **rood rol Enterprise Linux 7.2** ziet u meer informatie over de VM-afbeelding. Selecteer **klassieke**in de vervolgkeuzelijst **Selecteer een implementatiemodel** . Klik vervolgens op de knop **maken** .

    ![Details van de afbeelding weergeven](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Voer de **Hostnaam** voor de nieuwe virtuele machine in het deelvenster **VM maken** . Een gebruikersnaam lokale beheerder ook opgeven in het veld **gebruikersnaam** en **wachtwoord**. U kunt ook een SSH-sleutel gebruiken om de van de lokale beheerdergebruiker te verifiëren. Een **Laag prijzen** voor de virtuele machine ook selecteren.

    ![VM - basisgegevens maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klik op **optionele configuratie**. Klik op **netwerk**in het deelvenster **optioneel config** .

    ![Maken van VM - virtueel netwerk configureren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Hiermee opent u het deelvenster met een **netwerk**met de titel. Klik in het deelvenster **netwerk** op **Virtueel netwerk** om te selecteren van het virtuele netwerk waarnaar de VM Linux moet worden geïmplementeerd. Hiermee opent u het deelvenster **Virtual Network** . Kies de optie **gebruiken een bestaand virtual netwerk** in het deelvenster **Virtual Network** . Selecteer vervolgens het virtuele netwerk waarin Azure AD-domeinservices beschikbaar is. In dit voorbeeld Kies we het virtuele netwerk 'MyPreviewVNet'.

    ![Maken van VM - virtueel netwerk selecteren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. Klik op de knop **OK** in het deelvenster **optioneel config** .

    ![VM - virtueel netwerk geselecteerd maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. U bent nu klaar om te maken van de virtuele machine. Klik op de knop **maken** in het deelvenster **VM maken** .

    ![VM - gereed maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Implementatie van de nieuwe virtuele machine op basis van de afbeelding RHEL 7.2 moet beginnen.

  ![VM - implementatie gestart maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Na een paar minuten moet de virtuele machine worden geïmplementeerd succes en klaar voor gebruik.

  ![VM - geïmplementeerd maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Extern verbinding maken met de nieuw ingerichte Linux virtuele machine
De RHEL 7.2 virtuele machine is ingericht in Azure wordt aangegeven. De volgende taak is extern verbinding maken met de virtuele machine.

**Verbinding maken met de RHEL 7.2 virtuele machine** Volg de instructies in het artikel [hoe u zich aanmeldt bij een virtuele machine waarop Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

De rest van de stappen wordt ervan uitgegaan dat u de stopverf SSH-client verbinding maakt met de RHEL virtuele machine gebruiken. Zie de [pagina voor het downloaden van stopverf](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)voor meer informatie.

1. Open het stopverf programma.

2. Voer de **Hostnaam** voor de zojuist gemaakte RHEL virtuele machine. In dit voorbeeld heeft onze VM de hostnaam 'contoso-rhel.cloudapp .net'. Als u niet zeker van de hostnaam van uw VM bent, raadpleegt u het dashboard VM op de Azure-portal.

    ![Stopverf verbinding maken](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Meld u aan bij de virtuele machine met de van de lokale beheerdersreferenties die u hebt opgegeven waarop de virtuele machine is gemaakt. In dit voorbeeld hebben we het lokale beheerdersaccount "mahesh" gebruikt.

    ![Stopverf login](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Vereiste pakketten installeren op de Linux virtuele machine
Nadat u verbinding maakt met de virtuele machine, wordt de volgende taak is vereist voor de aan domein toevoegen op de virtuele machine pakketten installeren. De volgende stappen uitvoeren:

1. **Realmd installeren:** Het pakket realmd wordt gebruikt voor aan domein toevoegen. Typ de volgende opdracht in uw stopverf terminal:

    sudo yum installeren realmd

    ![Realmd installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Na een paar minuten, moet het pakket realmd geïnstalleerd op de virtuele machine.

    ![realmd geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Sssd installeren:** Het pakket realmd, is afhankelijk van sssd domein join-bewerkingen uit te voeren. Typ de volgende opdracht in uw stopverf terminal:

    sudo yum installeren sssd

    ![Sssd installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Na een paar minuten, moet het pakket sssd geïnstalleerd op de virtuele machine.

    ![realmd geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Kerberos installeren:** Typ de volgende opdracht in uw stopverf terminal:

    sudo yum installeren krb5-workstation krb5-libs

    ![Kerberos installeren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Na een paar minuten, moet het pakket realmd geïnstalleerd op de virtuele machine.

    ![Kerberos is geïnstalleerd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>De Linux virtuele machine toevoegen aan het beheerde domein
Nu de vereiste pakketten zijn geïnstalleerd op de Linux virtuele machine, de volgende taak is de virtuele machine toevoegen aan de beheerde domein.

1. Ontdek de beheerde domein AAD Domain Services. Typ de volgende opdracht in uw stopverf terminal:

    sudo realm CONTOSO100.COM ontdekken

    ![Kennismaken met realm](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Zorg ervoor dat het domein bereikbaar vanuit de virtuele machine (probeer ping) is is als **realm ontdekken** niet vinden van uw domein beheerde is. Ook voor zorgen dat de virtuele machine daadwerkelijk is geïmplementeerd op hetzelfde virtuele netwerk waarin het beheerde domein beschikbaar is.

2. Kerberos geïnitialiseerd. Typ de volgende opdracht in uw stopverf terminal. Zorg ervoor dat u opgeeft dat een gebruiker die deel uitmaakt van de ' AAD domeincontroller ' beheerdersgroep. Alleen deze gebruikers kunnen computers toevoegen aan de beheerde domein.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Zorg ervoor dat u de domeinnaam in hoofdletters, anders kinit mislukt.

3. Deelnemen aan de computer op het domein. Typ de volgende opdracht in uw stopverf terminal. Geef de dezelfde gebruiker die u hebt opgegeven in de vorige stap ('kinit').

    sudo realm join--uitgebreide CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Realm join](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

U moet krijg een bericht ('is ingeschreven computer in realm') wanneer de computer met succes wordt toegevoegd aan het beheerde domein.


## <a name="verify-domain-join"></a>Controleer of aan domein toevoegen
U kunt snel controleren of de computer is toegevoegd aan de beheerde domein. Verbinding maken met het domein toegevoegd RHEL VM SSH en een domein-gebruikersaccount en schakel het selectievakje gebruiken om te zien of de gebruikersaccount juist is opgelost.

1. In uw stopverf terminal, typ de volgende opdracht verbinding maken met het domein RHEL VM via SSH zijn toegevoegd. Een domein-account die bij de beheerde domein hoort (bijvoorbeeld 'bob@CONTOSO100.COM' in dit geval.)

    SSH -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net

2. Typ de volgende opdracht uit om te zien als de basismap juist is geïnitialiseerd in uw stopverf terminal.

    pwd

3. Typ de volgende opdracht uit om te zien of het groepslidmaatschap correct zijn wordt opgelost in uw stopverf terminal.

    ID

Voert u de uitvoer van een steekproef van deze opdrachten:

![Controleer of aan domein toevoegen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Problemen oplossen aan domein toevoegen
Raadpleeg het artikel [deelnemen aan het domein van probleemoplossing](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Verwante onderwerpen
- [Azure AD-domeinservices - handleiding aan de slag](./active-directory-ds-getting-started.md)

- [Deelnemen aan een Windows Server-VM een beheerde domeinservices van Azure AD-domein](active-directory-ds-admin-guide-join-windows-vm.md)

- [Aanmelden met een virtuele machine waarop Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Kerberos installeren](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Rode rol Enterprise Linux 7 - handleiding voor Windows-integratie](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
