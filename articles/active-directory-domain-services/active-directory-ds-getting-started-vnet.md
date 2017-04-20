<properties
    pageTitle="Azure AD-domeinservices: Maken of selecteren van een virtueel netwerk | Microsoft Azure"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="create-or-select-a-virtual-network-for-azure-ad-domain-services"></a>Maken of selecteren van een virtueel netwerk voor Azure AD-domeinservices

## <a name="guidelines-to-select-an-azure-virtual-network"></a>Richtlijnen voor het selecteren van een Azure virtuele netwerk
> [AZURE.NOTE] **Voordat u begint**: verwijzen naar [Overwegingen voor Azure AD-domeinservices netwerkproblemen](active-directory-ds-networking.md).


## <a name="task-2-create-an-azure-virtual-network"></a>Taak 2: Een Azure virtuele netwerk maken
De volgende configuratietaak is het opzetten van een Azure virtuele netwerk en een subnet erin. U inschakelen Azure AD-domeinservices in dit subnet binnen het netwerk van uw virtuele. Als u al een bestaand virtuele netwerk dat u wilt gebruiken, kunt u deze stap overslaan.

> [AZURE.NOTE] Zorg ervoor dat het Azure virtuele netwerk die u maakt of Kies voor gebruik met Azure AD-domeinservices deel uitmaakt van een Azure regio die wordt ondersteund door Azure AD Domain Services. Ga naar de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) informatie van de Azure regio's waarin Azure AD-domeinservices beschikbaar is.

Houd er rekening mee omlaag de naam van het virtuele netwerk zodat u het rechts virtuele netwerk bij het inschakelen van Azure AD-domeinservices in de van de volgende configuratiestap.

De volgende configuratiestappen uit als u wilt maken van een Azure virtuele netwerk waarin u wilt inschakelen Azure AD-domeinservices uitvoeren.

1. Ga naar de **klassieke Azure-portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Selecteer het knooppunt **netwerken te gebruiken** in het linkerdeelvenster.

    ![Netwerken knooppunt](./media/active-directory-domain-services-getting-started/networks-node.png)

3. Klik op het taakvenster onder aan de pagina op **Nieuw** .

    ![Virtuele netwerken knooppunt](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4. Selecteer in het knooppunt **Netwerkservices** **Virtual Network**.

5. Klik op **Snelle maken** als u wilt maken van een virtueel netwerk.

    ![Virtuele netwerk - snelle maken](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. Geef een **naam** voor uw virtuele netwerk. U kunt ook de **adresruimte** of **maximale VM tellen** configureren voor dit netwerk. Nu kunt u de instelling **DNS-server** is ingesteld op 'Geen' laten. U kunt de DNS-server instellen nadat u hebt uw inschakelen Azure AD domein Services bijwerken.

7. Zorg ervoor dat u een ondersteunde Azure regio in de vervolgkeuzelijst voor **locatie selecteert** . Ga naar de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) informatie van de Azure regio's waarin Azure AD-domeinservices beschikbaar is.

8. Als u wilt uw virtuele netwerk maken, klikt u op de knop **maken een virtueel netwerk** .

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet.png)

9. Nadat het virtuele netwerk is gemaakt, selecteert u het virtuele netwerk en klik op het tabblad **configureren** .

    ![Een subnet maken](./media/active-directory-domain-services-getting-started/create-vnet-properties.png)

10. Ga naar de sectie **virtueel netwerk adres spaties** . Klik op **subnet toevoegen** en geef een subnet met de naam **AaddsSubnet**. Klik op **Opslaan** als u wilt maken van het subnet.

    ![Maak een subnet voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/create-vnet-add-subnet.png)


<br>

## <a name="task-3---enable-azure-ad-domain-services"></a>Taak 3: inschakelen Azure AD domeinservices
De volgende configuratietaak is om in te [schakelen Azure AD Domain Services](active-directory-ds-getting-started-enableaadds.md).
