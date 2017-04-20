<properties
    pageTitle="Azure AD-domeinservices: Netwerken richtlijnen | Microsoft Azure"
    description="Aandachtspunten voor de netwerken voor Azure Active Directory Domain Services"
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
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Aandachtspunten voor de netwerken voor Azure AD-domeinservices

## <a name="how-to-select-an-azure-virtual-network"></a>Het selecteren van een Azure virtuele netwerk
De volgende richtlijnen kunnen u een virtueel netwerk voor gebruik met Azure AD Domain Services selecteren.

### <a name="type-of-azure-virtual-network"></a>Type Azure virtuele netwerk

- Azure AD-domeinservices in een klassieke Azure virtuele netwerk, kunt u ook weer inschakelen.

- Azure AD-domeinservices **in virtuele netwerken die zijn gemaakt met Azure Resource Manager kan niet worden ingeschakeld**.

- U kunt een virtuele resourcemanager-netwerk verbinding maken met een klassieke virtuele netwerk waarin Azure AD-domeinservices is ingeschakeld. Daarna kunt u Azure AD-domeinservices in de virtuele resourcemanager-netwerk. Zie het gedeelte [netwerkconnectiviteit](active-directory-ds-networking.md#network-connectivity) voor meer informatie.

- **Regionale virtuele netwerken**: als u een bestaand virtuele netwerk gebruiken wilt, moet u zorgen dat dit een regionale virtueel netwerk is.

    - Virtuele netwerken die gebruikmaken van de om oudere affiniteit-groepen kunnen niet worden gebruikt met Azure AD Domain Services.

    - Azure AD-domeinservices, [migreren oudere virtuele netwerken met regionale virtuele netwerken](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)gebruiken.


### <a name="azure-region-for-the-virtual-network"></a>Azure regio voor het virtuele netwerk

- Uw Azure AD-domeinservices beheerde domein wordt geïmplementeerd in hetzelfde Azure gebied, als het virtuele netwerk u ervoor kiezen om in te schakelen van de service in.

- Selecteer een virtueel netwerk in een Azure gebied wordt ondersteund door Azure AD Domain Services.

- Ga naar de pagina [Azure services per regio](https://azure.microsoft.com/regions/#services/) informatie van de Azure regio's waarin Azure AD-domeinservices beschikbaar is.


### <a name="requirements-for-the-virtual-network"></a>Vereisten voor het virtuele netwerk

- **Uw Azure werkbelasting nabijheid**: Selecteer het virtuele netwerk dat momenteel wordt gehost/wordt gehost in virtuele machines die toegang tot Azure AD Domain Services hebben.

- **Aangepaste/brengen zelf DNS-servers**: Zorg ervoor dat er geen aangepaste DNS-servers geconfigureerd voor het virtuele netwerk.

- **Bestaande domeinen met dezelfde domeinnaam**: Zorg ervoor dat er geen een bestaand domein met dezelfde domeinnaam beschikbaar op dat virtuele netwerk. Bijvoorbeeld dat u een domein met de naam 'contoso.com' al beschikbaar in een netwerk met de geselecteerde virtuele hebt. U wilt later een Azure AD-domeinservices beheerde domein met dezelfde domeinnaam (dit is 'contoso.com') in deze virtuele netwerk inschakelen. Er wordt een fout bij het inschakelen van Azure AD-domeinservices. Deze fout oplevert naamconflicten voor de naam van het domein op dat virtuele netwerk. In dit geval moet u een andere naam voor het instellen van uw beheerde domeinservices van Azure AD-domein. U kunt ook het bestaande domein opgeheven inrichten en gaat u verder met het inschakelen van Azure AD Domain Services.

> [AZURE.WARNING] U kunt domeinservices niet verplaatsen naar een ander virtuele netwerk nadat u de service hebt ingeschakeld.


## <a name="network-security-groups-and-subnet-design"></a>Beveiligingsgroepen netwerk en subnet ontwerpen
Een [Groep voor netwerk-beveiliging (NSG)](../virtual-network/virtual-networks-nsg.md) bevat een lijst met regels voor Access (Toegangsbeheerlijst) die toestaan of weigeren netwerkverkeer tot uw VM-sessies in een virtueel netwerk. NSGs kan worden gekoppeld aan subnetten of afzonderlijke VM exemplaren in dat subnet. Wanneer een NSG gekoppeld aan een subnet is, wordt de ACL regels toepassen op alle VM exemplaren in dat subnet. Daarnaast verkeer naar een afzonderlijke VM kan worden beperkt door te koppelen van een NSG rechtstreeks naar die VM verdere.

![Aanbevolen subnet ontwerpen](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Aanbevolen procedures voor het kiezen van een subnet
- Azure AD-domeinservices dashboard implementeren naar een **afzonderlijk speciale subnet** binnen uw Azure virtuele netwerk.

- NSGs worden niet worden toegepast op het speciale subnet voor uw domein beheerde. Als u NSGs op het speciale subnet toepassen moet, controleert u **Blokkeer de poorten vereist service en beheren van uw domein niet**.

- Het aantal beschikbare binnen de speciale subnet voor uw domein beheerde IP-adressen niet te beperken. Deze beperking wordt voorkomen dat de service twee domeincontrollers beschikbaar zijn voor uw domein beheerde aanbrengen.

- **Schakel geen Azure AD-domeinservices in het subnet gateway** van uw virtuele netwerk.


> [AZURE.WARNING] Wanneer u koppelt een NSG met een subnet waarin Azure AD Domain Services is ingeschakeld, wordt er mogelijk afgebroken mogelijkheid van de Microsoft service en beheren van het domein. Daarnaast kunnen is synchronisatie tussen uw Azure AD-tenant en uw beheerde domein onderbroken. **De SLA geldt niet voor implementaties waar een NSG is toegepast, waardoor Azure AD-domeinservices uit bijwerken en beheren van uw domein.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Vereiste voor Azure AD-domeinservices poorten
De volgende poorten zijn vereist voor Azure AD-domeinservices service en onderhouden van uw beheerde domein. Zorg ervoor dat deze poorten niet worden geblokkeerd voor het subnet waarin u uw beheerde domein hebt ingeschakeld.

| Poortnummer | Doel |
|---|---|
| 443 | Synchronisatie met uw Azure AD-tenant |
| 3389 | Beheer van uw domein |
| 5986 | Beheer van uw domein |
| 636 | Beveiligde LDAP (leesbaar TEKSTFORMAAT) toegang tot uw beheerde domein |



## <a name="network-connectivity"></a>Netwerkconnectiviteit
Een beheerde domeinservices van Azure AD-domein kan worden ingeschakeld alleen binnen één klassieke virtual netwerk in Azure wordt aangegeven. Virtuele netwerken die zijn gemaakt met Azure Resource Manager worden niet ondersteund.


### <a name="scenarios-for-connecting-azure-networks"></a>Scenario's voor het verbinden van Azure-netwerken te gebruiken
Verbinding maken met Azure virtuele netwerken om de beheerde domein in een van de volgende implementatie-scenario's te gebruiken:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>De beheerde domein in meer dan één Azure klassieke virtuele netwerk gebruiken
U kunt andere Azure klassieke virtuele netwerken verbinden met het Azure klassieke virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld. Deze VPN-verbinding kunt u het beheerde domein gebruiken met uw werkbelasting geïmplementeerd in andere virtuele netwerken.

![Klassieke virtuele netwerkconnectiviteit](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>De beheerde domein gebruiken in een virtuele resourcemanager-netwerk
U kunt een virtuele resourcemanager-netwerk koppelen aan het Azure klassieke virtuele netwerk waarin u Azure AD-domeinservices hebt ingeschakeld. Deze verbinding kunt u het beheerde domein gebruiken met uw werkbelasting geïmplementeerd in de virtuele resourcemanager-netwerk.

![Resourcemanager naar klassieke virtuele netwerkconnectiviteit](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Opties voor een netwerkverbinding

- **VNet-naar-VNet verbindingen via een VPN-verbinding naar website**: een virtueel netwerk verbinden met een ander virtuele netwerk (VNet VNet) is vergelijkbaar met een virtueel netwerk verbinden met een on-premises site-locatie. Beide typen connectivity gebruiken een VPN-gateway te leveren van een beveiligde tunnel met IPsec/IKE.

    ![Virtuele netwerkconnectiviteit via VPN Gateway](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Meer informatie - virtuele netwerken via VPN gateway](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet-naar-VNet verbindingen met virtuele netwerk peering**: Virtual network peering is een methode die met elkaar twee virtuele netwerken in hetzelfde gebied via het backbonenetwerk Azure verbindt. Nadat de peered, wordt de twee virtuele netwerken weergegeven als een handtekening voor alle connectivity doeleinden. Ze nog steeds worden beheerd als afzonderlijke resources, maar virtuele machines in deze virtuele netwerken met elkaar kunnen communiceren via privé IP-adressen.

    ![Virtuele netwerkconnectiviteit met peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Meer informatie - virtuele netwerk peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Verwante onderwerpen

- [Azure virtuele netwerk peering](../virtual-network/virtual-network-peering-overview.md)

- [Een verbinding VNet-naar-VNet voor het implementatiemodel klassieke configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure netwerk beveiligingsgroepen](../virtual-network/virtual-networks-nsg.md)
