<properties
    pageTitle="Abonnement en Accounts richtlijnen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor abonnementen en -accounts op Azure."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Richtlijnen voor het abonnement en -accounts

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

In dit artikel bevat informatie met informatie over benadering van abonnement en account beheren als uw omgeving en gebruikers in omvang groeit.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Implementatie van richtlijnen voor abonnementen en -accounts

Beslissingen:

- Welke reeks abonnementen en accounts wilt u nodig hebt voor het hosten van uw IT-werkbelasting of infrastructuur?
- Hoe splits op de hiërarchie passend maken van uw organisatie?

Taken:

- Definieer uw logische organisatiehiërarchie zoals u bij het beheren van een abonnement wilt.
- Definiëren zodat deze overeenkomen met deze logische hiërarchie, de accounts die zijn vereist en abonnementen onder elk account.
- Maak de set abonnementen en accounts die uw naamgevingsconventie gebruiken.


## <a name="subscriptions-and-accounts"></a>Abonnementen en -accounts

Als u wilt werken met Azure, moet u een of meer Azure abonnementen. Informatiebronnen zoals virtuele machines (VMs) of virtuele netwerken bestaat niet in deze abonnementen.

- Zakelijke klanten hebben meestal een Enterprise-registratie, de bovenste resource in de hiërarchie, waarna is gekoppeld aan een of meer accounts.
- Voor consumenten en klanten zonder een Enterprise-registratie is de bovenste resource het account.
- Abonnementen zijn gekoppeld aan accounts en kunnen er een of meer abonnementen per account. Azure records betalingsgegevens op het niveau van abonnement.

Vanwege de limiet van twee niveaus op de relatie Account/abonnement is het belangrijk om uit te lijnen voor de naamgeving van abonnementen aan de behoeften van de facturering en accounts. Bijvoorbeeld als een globale bedrijf gebruikmaakt van Azure, kunnen ze ervoor kiezen om één account per regio en hebt abonnementen beheerd op het regioniveau:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

U kunt bijvoorbeeld de volgende structuur:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Als een gebied wil meer dan één abonnement dat is gekoppeld aan een bepaalde groep hebt, moet de naamgevingsconventie op een manier om te coderen van de extra gegevens op de account of de naam van het abonnement opnemen. Deze organisatie toestaat massaging billing gegevens om te genereren van de nieuwe niveaus van de hiërarchie tijdens facturering rapporten:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

De organisatie kan er als volgt te werk:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Wij bieden gedetailleerde facturering via een downloadbare bestand voor één account of voor alle accounts in een enterprise agreement.


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 