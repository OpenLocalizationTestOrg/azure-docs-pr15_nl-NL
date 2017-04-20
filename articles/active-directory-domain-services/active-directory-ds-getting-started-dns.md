<properties
    pageTitle="Azure AD-domeinservices: Update DNS-instellingen voor het Azure virtuele netwerk | Microsoft Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Azure AD Domain Services - Update DNS-instellingen voor het Azure virtuele netwerk

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Taak 4: Bijwerken van DNS-instellingen voor het Azure virtuele netwerk
In de voorgaande configuratietaken uit te voeren, u hebt Azure AD Domain Services voor uw adreslijst ingeschakeld. De volgende taak is om ervoor te zorgen dat computers binnen het virtuele netwerk kunnen verbinding maken en gebruik van deze services. Werk de DNS-server-instellingen voor uw virtuele netwerk te laten verwijzen naar de twee IP-adressen waarop Azure AD-domeinservices beschikbaar op het virtuele netwerk is.

> [AZURE.NOTE] Houd er rekening mee omlaag de IP-adressen voor Azure AD-domeinservices weergegeven op het tabblad **configureren** van uw adreslijst, nadat u Azure AD-domeinservices voor de map hebt ingeschakeld.

Voer de volgende configuratiestappen uit als u wilt bijwerken van de server de DNS-instellingen voor het virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld uit.

1. Ga naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **netwerken te gebruiken** in het linkerdeelvenster.

    ![Virtuele netwerken knooppunt](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Selecteer het virtuele netwerk waarin u Azure AD-domeinservices om weer te geven van de eigenschappen ervan hebt ingeschakeld in het tabblad **Virtuele netwerken** .

4. Klik op het tabblad **configureren** .

    ![Virtuele netwerken knooppunt](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. Voer het IP-adressen van Azure AD-domeinservices in de sectie **DNS-servers** .

6. Zorg ervoor dat u zowel de IP-adressen die werden weergegeven in de sectie **Domain Services** op het tabblad **configureren** van uw adreslijst invoeren.

7. Als u wilt de DNS-server-instellingen voor dit virtuele netwerk hebt opgeslagen, klikt u op **Opslaan** in het taakvenster onder aan de pagina.

   ![Werk de DNS-server-instellingen voor het virtuele netwerk.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Na het bijwerken van de DNS-server-instellingen voor het virtuele netwerk, kan het enige een duren voor virtuele machines in het netwerk om de bijgewerkte DNS-configuratie. Als een virtuele machine geen verbinding maken met het domein is, kunt u vooraf de DNS-cache (bv. ' ipconfig/flushdns') op de virtuele machine. Deze opdracht dwingt vernieuwen van de DNS-instellingen op de virtuele machine.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Taak 5: inschakelen Wachtwoordsynchronisatie Azure AD Domain Services
De volgende configuratietaak is [Wachtwoordsynchronisatie Azure AD Domain Services inschakelen](active-directory-ds-getting-started-password-sync.md).
